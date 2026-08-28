---
name: env-vars-mirlo
description: Reglas para decidir qué valor va en el `.env` y qué valor va como constante en el código, cómo validarlo con zod en `src/config/env.js`, y por qué está prohibido el patrón `process.env.X || 'default'`. Usar al agregar o revisar cualquier valor de configuración, al escribir o editar `.env`, `.env.example` o el módulo de config, y al revisar código existente que lea `process.env`.
---

# env-vars-mirlo

El `.env` **no** es el lugar donde vive la configuración de la app. Es solo para valores que cambian según **dónde** corre el código (local / staging / prod) o que son **secretos**. Todo lo demás es una constante versionada.

## Cuándo aplica

Al escribir, revisar o agregar cualquier valor de configuración. Corre junto con `backend-mirlo`, que define dónde vive el módulo de config: `src/config/env.js`.

> La plantilla canónica de `src/config/env.js` está en `backend-mirlo/assets/plantilla/src/config/env.js`. No duplicarla acá ni reescribirla de memoria: esta skill decide **qué** entra en ese schema, `backend-mirlo` define **cómo** está armado el archivo.

## El test: dos preguntas antes de agregar una variable

1. ¿Este valor tiene que ser **distinto** en producción que en la máquina local?
2. ¿Es un **secreto** que no puede quedar versionado en el repo?

```
sí a alguna  → env var (validada en el schema, sin fallback disperso)
no a ambas   → constante en el código
```

Si la respuesta a ambas es no y aun así se quiere hacer configurable, no se convierte en env var global: se expone como **flag de CLI** o **parámetro de función**.

```
node --import ./alias.js sync.js --dtype fp32 --preproc crop
```

## Reglas

1. **Nunca meter configuración de la app en el `.env`.**
2. **Nada de env vars de relleno con fallback.** El patrón `process.env.LO_QUE_SEA || "default"` está prohibido: el default real queda invisible y nadie lo cambia nunca.
3. **Un solo punto de lectura.** `process.env` se lee únicamente en `src/config/env.js`. El resto del código importa `env` desde ahí.
4. **Validación al arranque** con zod, y la app muere si falta algo. Sin fallback silencioso.
5. **El `.env.example` debe ser corto y legible.** Si pasa de ~10 líneas, algo se coló que no correspondía.

## Defaults: dónde sí están permitidos

Un default **dentro del schema de validación** es legítimo, porque es visible, versionado y está en un solo lugar:

```js
// ✅ Default visible, en el único archivo donde se lee el entorno
PORT: z.coerce.number().int().positive().default(5000),
```

Lo prohibido es el default disperso y escondido:

```js
// ❌ El valor real vive acá, invisible, en medio del código
const PORT = process.env.PORT || 5000;
```

**Los secretos nunca llevan default.** `MONGO_URI`, `JWT_SECRET`, API keys: si faltan, la app no arranca.

## Qué va y qué no va en el `.env`

| Sí va | No va |
|---|---|
| Credenciales y secretos (`MONGO_URI`, `JWT_SECRET`, API keys, tokens) | Nombres de modelos, hiperparámetros, cuantización, preprocesado |
| Endpoints de servicios externos (`REDIS_URL`, `S3_ENDPOINT`, `SMTP_HOST`) | Rutas internas del proyecto (cache dirs, assets, outputs) |
| `PORT`, `HOST`, `NODE_ENV`, `LOG_LEVEL` | Timeouts, tamaños de batch, reintentos, page sizes |
| Flags de infraestructura que cambian por deploy | Cualquier valor con default hardcodeado que nadie cambia entre deploys |

## Cómo escribirlo

### Incorrecto

```js
// Cuatro env vars que nadie va a definir nunca, con el default escondido
export const embedding = {
  modelId: process.env.EMBEDDING_MODEL || 'Xenova/clip-vit-base-patch32',
  dtype: process.env.EMBED_DTYPE || 'q8',
  preproc: process.env.EMBED_PREPROC || 'pad',
  cacheDir: process.env.MODELS_CACHE_DIR || './models-cache',
};
```

### Correcto

```js
// Constantes versionadas y documentadas, visibles en el repo
export const EMBEDDING = {
  modelId: 'Xenova/clip-vit-base-patch32',
  // q8 = cuantizado (4x más liviano y rápido en CPU). Alternativa: fp32.
  dtype: 'q8',
  // pad = carta completa con letterboxing | crop = recorte central de CLIP.
  // Cambiarlo requiere re-sync con --force.
  preproc: 'pad',
  cacheDir: './models-cache',
};
```

El comentario al lado de cada constante no es opcional: es lo que reemplaza a la falsa configurabilidad que daba la env var.

## Ejemplos de decisiones

| Valor | Dónde va | Por qué |
|---|---|---|
| `MONGO_URI` | `.env` | Distinto en local y prod, y además es secreto |
| `PORT` | `.env`, con default en el schema | Lo define el entorno de deploy |
| `EMBEDDING_MODEL` | constante en código | Decisión de arquitectura, igual en todos lados |
| `EMBED_DTYPE` | constante en código | Tuning del proyecto, no del entorno |
| `MODELS_CACHE_DIR` | constante en código | Ruta interna del repo |
| `MAX_RETRIES` | constante en código | Default de comportamiento, no de deploy |
| `S3_BUCKET` | `.env` | Cambia entre staging y prod |

## Revisar un proyecto existente

Las dos violaciones se encuentran con grep:

```bash
# 1. process.env fuera del único punto de lectura
grep -rn "process\.env" src/ server.js --include="*.js" | grep -v "config/env.js"

# 2. Defaults escondidos con fallback
grep -rn "process\.env\.[A-Z_]\+ *||" .
```

Cada resultado del primer grep se mueve al schema de `src/config/env.js` o se convierte en constante, según el test de dos preguntas. Cada resultado del segundo es un default que hay que hacer visible o eliminar.

Después, revisar que el `.env.example` no tenga entradas huérfanas: toda línea del `.env.example` debe existir en el schema, y toda variable del schema sin default debe estar en el `.env.example`.

## Relación con Docker

En un proyecto dockerizado, el `docker-compose.yml` es el que inyecta las variables, con la misma lógica: `${VAR:?mensaje}` para las obligatorias (el compose falla antes de levantar) y `${VAR:-default}` para las opcionales.

Dentro del contenedor no hay archivo `.env` (está en el `.dockerignore`), así que `dotenv` simplemente no encuentra nada y las variables llegan por el entorno. Es lo esperado, no un bug.

## Checklist

- [ ] ¿Pasa el test de dos preguntas (cambia por entorno o es secreto)?
- [ ] ¿Se lee desde `src/config/env.js` y no desde `process.env` disperso?
- [ ] ¿Validada al arranque, sin `|| "default"` suelto?
- [ ] ¿Los secretos quedaron sin default?
- [ ] ¿Documentada en el `.env.example`?
- [ ] Si es un parámetro de comportamiento, ¿se movió a constante o a flag de CLI?
