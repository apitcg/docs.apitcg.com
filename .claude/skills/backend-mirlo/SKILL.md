---
name: backend-mirlo
description: Convenciones obligatorias para APIs backend en Node.js con Express 5, Mongoose y ES Modules puros (nunca TypeScript). Usar al crear un proyecto de API nuevo, al agregar rutas, controladores o servicios, al configurar variables de entorno, el alias `@/` o la conexión a MongoDB, y al revisar o migrar un backend Node existente. Incluye una plantilla completa lista para copiar.
---

# backend-mirlo

> **Este archivo es autocontenido.** Lo que el texto menciona como `assets/...`, `scripts/...` o `references/...` no son archivos aparte: están al final de este mismo documento.

Estándar fijo para cualquier API en Node. No son sugerencias: si el código entregado no cumple estas reglas, está mal y hay que corregirlo antes de entregar.

## Cuándo aplica

Cualquier proyecto de API en Node. Además:

| Situación | Skill que también aplica |
|---|---|
| El proyecto tiene el frontend en un repo aparte | `workspace-mirlo` |
| Se crean o modifican modelos | `models-mirlo` |
| Se agregan, leen o documentan variables de entorno | `env-vars-mirlo` |

## Reglas innegociables

1. **JS puro con ES Modules.** Nunca TypeScript.
2. **`"type": "module"`** en `package.json`.
3. **Alias `@/` para importar desde `src/`**, resuelto con un loader nativo de Node. Nunca con el campo `imports` de `package.json`: ese campo exige claves que partan con `#` y falla con `@/`.
4. **Puerto por defecto 5000**, definido en `src/config/env.js`.
5. **`process.env` se lee en un solo archivo**: `src/config/env.js`. El resto importa `env` desde ahí.
6. **Siempre `cors`** en `app.js`.
7. **Nunca validar duplicados a mano**: dejar que Mongo tire `11000` y capturarlo.
8. **Siempre crear `.gitignore` y `.env.example`**; nunca subir el `.env` real.

## Estructura de carpetas

```
src/
├── config/
│   ├── env.js          ← único lugar donde se lee process.env
│   └── db.js
├── middlewares/
├── models/
├── routes/
├── controllers/
├── services/
├── utils/
└── app.js
server.js               ← entrypoint, en la raíz
alias.js                ← registra el loader del alias @/
alias-hooks.js          ← hook de resolución
jsconfig.json           ← para que el editor entienda @/
.env
.env.example
.gitignore
package.json
```

## Proyecto nuevo: cómo partir

Los archivos base viven en `assets/plantilla/`. **Son la fuente de verdad: no reescribirlos de memoria.**

Con acceso al filesystem, copiar la plantilla completa y renombrar los dos archivos que van con punto:

```bash
cp -r assets/plantilla/. ./mi-proyecto/
cd mi-proyecto
mv gitignore .gitignore
mv env.example .env.example
cp .env.example .env
npm install
```

Después:

- Cambiar `name` en `package.json`.
- Cambiar el nombre de la base de datos en `.env.example` y en el `.env` real.
- `src/app.js` viene con `user.routes.js` como ejemplo. Reemplazar ese import y esa entrada del array `routes` por los recursos reales, o el arranque falla.
- Recorrer el checklist del final.

Sin acceso al filesystem (respondiendo en chat), leer los archivos de `assets/plantilla/` y entregarlos como bloques de código, ya renombrados a `.gitignore` y `.env.example`.

## El alias `@/`

`alias.js` registra el hook y `alias-hooks.js` traduce `@/x` a `src/x`. `jsconfig.json` es solo para el autocompletado del editor: no afecta el runtime.

**El alias solo existe si el proceso arranca con `--import ./alias.js`.** Los scripts de `package.json` ya lo llevan, pero cualquier script suelto necesita el mismo flag:

```bash
node --import ./alias.js scripts/seed.js
```

Alternativa con dependencia: el paquete `esm-module-alias`. Se prefiere el loader propio porque no agrega dependencias y sobrevive a `npm ci --omit=dev`.

## Registro de rutas

`src/app.js` tiene un array `routes`. **Esa es la única forma de registrar rutas.** No usar `app.use('/api/x', xRoutes)` suelto.

```js
const routes = [
  { path: '/api/users', route: userRoutes },
  { path: '/api/products', route: productRoutes },
];

routes.forEach((r) => app.use(r.path, r.route));
```

Cada recurso nuevo se agrega como un objeto más en el array.

## Manejo de duplicados

Nunca consultar antes de insertar para ver si existe. Dejar que Mongo falle y capturar el código:

```js
// ❌ No hacer esto
const existing = await User.findOne({ email });
if (existing) return res.status(409).json({ message: 'Email ya existe' });

// ✅ Hacer esto
try {
  const user = await User.create({ email, username, password });
  res.status(201).json(user);
} catch (error) {
  if (error.code === 11000) {
    return res.status(409).json({ message: 'Email o username ya existe' });
  }
  res.status(500).json({ message: 'Error interno del servidor' });
}
```

Para que funcione, los campos correspondientes llevan `unique: true` en el schema.

`Model.create()` sí dispara los hooks `pre('save')`, así que sirve con modelos de `_id` slug o counter.

## Comentarios

El nivel de comentario de las plantillas es parte de la convención, no decoración. En todo lo que se entregue:

- **Cabecera de archivo**: bloque `─────` con el nombre del archivo, qué hace y cómo se usa.
- **JSDoc en cada función exportada**: `@param` con el significado de cada argumento y `@returns` con lo que devuelve.
- **Comentario en cada decisión no obvia**: por qué un timeout es 8000, por qué se falla rápido, qué pasa si falta el flag.

Las plantillas de `assets/plantilla/` son el ejemplo de referencia.

## Secretos

No logear `MONGO_URI`: trae usuario y contraseña. En general, nada que salga de `env` y sea secreto se imprime en consola.

## Express 5

El proyecto va con Express 5. Si el código viene de Express 4, o si fallan comodines, parámetros opcionales o el manejo de errores `async`, leer `references/express-5.md`.

## Checklist al crear proyecto nuevo

- [ ] `package.json` con `"type": "module"` y scripts con `--import ./alias.js`
- [ ] `alias.js` + `alias-hooks.js` en la raíz y `jsconfig.json` para el editor
- [ ] Estructura `src/` completa
- [ ] `src/config/env.js` con zod y sin ningún `process.env` fuera de ahí
- [ ] `src/config/db.js` importando `env`
- [ ] `src/app.js` con `cors` y el array `routes[]`
- [ ] `server.js` en la raíz
- [ ] `.env.example` y `.gitignore` con `.env`
- [ ] Modelos con `unique: true` donde corresponde y duplicados vía `11000`
- [ ] Cabecera de archivo y JSDoc en las funciones exportadas

---

# Anexos: referencias

## Express 5: lo que cambia respecto a 4

Leer esto al migrar un proyecto desde Express 4, o cuando algo que funcionaba en 4 falla en 5.

| Cambio | Qué hacer |
|---|---|
| Los errores de funciones `async` se propagan solos al middleware de errores | Se puede sacar el `try/catch` puramente defensivo, pero **mantenerlo** donde se devuelve un status específico (409, 404) |
| Los comodines necesitan nombre | `app.get('*')` → `app.get('/*splat')` |
| Parámetros opcionales cambian de sintaxis | `/users/:id?` → `/users{/:id}` |
| `app.del()` ya no existe | usar `app.delete()` |
| `req.query` es un getter | no reasignarlo |

## try/catch: cuándo se saca y cuándo se mantiene

Se saca cuando el `catch` solo repetía lo que ya hace el middleware de errores de `app.js`:

```js
// Antes (Express 4)
export const getUsers = async (req, res) => {
  try {
    const users = await User.find();
    res.json(users);
  } catch (error) {
    res.status(500).json({ message: 'Error interno del servidor' });
  }
};

// Después (Express 5): el error viaja solo al middleware de errores
export const getUsers = async (req, res) => {
  const users = await User.find();
  res.json(users);
};
```

Se mantiene cuando hay que distinguir un caso y devolver otro status:

```js
export const createUser = async (req, res) => {
  try {
    const user = await User.create(req.body);
    res.status(201).json(user);
  } catch (error) {
    // Sin este catch, un duplicado se convertiría en 500
    if (error.code === 11000) {
      return res.status(409).json({ message: 'Email o username ya existe' });
    }
    throw error;
  }
};
```

## Antes de subir la versión

- Buscar `'*'` y `':param?'` en `src/routes/`.
- Buscar `app.del(`.
- Buscar asignaciones a `req.query`.
- Revisar que el middleware de errores de `app.js` siga siendo el último `app.use`.

---

# Anexos: plantillas y scripts

## `assets/plantilla/alias-hooks.js`

```js
// ─────────────────────────────────────────────
// alias-hooks.js — hook de resolución de módulos.
// Corre en un thread aparte, por eso recibe la raíz por `initialize`.
// ─────────────────────────────────────────────

let srcUrl = null;

/**
 * Recibe los datos pasados en register().
 * @param {{ srcUrl: string }} data - URL absoluta de la carpeta src/
 * @returns {void}
 */
export function initialize(data) {
  srcUrl = data.srcUrl;
}

/**
 * Traduce los especificadores que empiezan con "@/" a una URL dentro de src/.
 * @param {string} specifier - Lo que se escribió en el import
 * @param {object} context - Contexto de resolución de Node
 * @param {Function} nextResolve - Siguiente hook de la cadena
 * @returns {Promise<object>} Resultado de la resolución
 */
export async function resolve(specifier, context, nextResolve) {
  if (specifier.startsWith('@/')) {
    return nextResolve(new URL(specifier.slice(2), srcUrl).href, context);
  }
  return nextResolve(specifier, context);
}
```

## `assets/plantilla/alias.js`

```js
// ─────────────────────────────────────────────
// alias.js — registra el hook que traduce "@/x" a "./src/x"
// Uso: node --import ./alias.js server.js
// ─────────────────────────────────────────────
import { register } from 'node:module';

register('./alias-hooks.js', import.meta.url, {
  // La raíz del alias: todo lo que empiece con @/ cuelga de src/
  data: { srcUrl: new URL('./src/', import.meta.url).href },
});
```

## `assets/plantilla/env.example`

```
PORT=5000
MONGO_URI=mongodb://localhost:27017/nombre_db
```

## `assets/plantilla/gitignore`

```
node_modules/
.env
dist/
*.log
.DS_Store
```

## `assets/plantilla/jsconfig.json`

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": { "@/*": ["./src/*"] }
  },
  "exclude": ["node_modules"]
}
```

## `assets/plantilla/package.json`

```json
{
  "name": "nombre-proyecto",
  "version": "1.0.0",
  "type": "module",
  "main": "server.js",
  "engines": { "node": ">=24" },
  "scripts": {
    "dev": "node --import ./alias.js --watch server.js",
    "start": "node --import ./alias.js server.js"
  },
  "dependencies": {
    "cors": "^2.8.5",
    "dotenv": "^17.0.0",
    "express": "^5.2.1",
    "mongoose": "^9.0.0",
    "zod": "^4.0.0"
  }
}
```

## `assets/plantilla/server.js`

```js
// ─────────────────────────────────────────────
// server.js — Entrypoint. Conecta la base y levanta Express.
// Uso: npm run dev
// ─────────────────────────────────────────────
import { env } from '@/config/env.js';
import app from '@/app.js';
import connectDB from '@/config/db.js';

const start = async () => {
  await connectDB();
  app.listen(env.PORT, () => {
    console.log(`Servidor corriendo en http://localhost:${env.PORT}`);
  });
};

start();
```

## `assets/plantilla/src/app.js`

```js
// ─────────────────────────────────────────────
// app.js — Instancia de Express y registro de rutas.
// Cada recurso nuevo se agrega al array `routes`.
// ─────────────────────────────────────────────
import express from 'express';
import cors from 'cors';
import userRoutes from '@/routes/user.routes.js';

const app = express();

app.use(cors());
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Registro central de rutas: agregar acá cada recurso nuevo
const routes = [
  { path: '/api/users', route: userRoutes },
  // { path: '/api/products', route: productRoutes },
];

routes.forEach((r) => app.use(r.path, r.route));

// Middleware de errores: en Express 5 los rechazos de promesas llegan solos hasta acá
app.use((err, req, res, next) => {
  console.error(err);
  res.status(500).json({ message: 'Error interno del servidor' });
});

export default app;
```

## `assets/plantilla/src/config/db.js`

```js
// ─────────────────────────────────────────────
// db.js — Conexión a MongoDB con Mongoose.
// Se importa en server.js antes de levantar el servidor.
// ─────────────────────────────────────────────
import mongoose from 'mongoose';
import { env } from '@/config/env.js';

/**
 * Conecta a MongoDB y corta el proceso si no lo logra.
 * @returns {Promise<void>}
 */
const connectDB = async () => {
  try {
    await mongoose.connect(env.MONGO_URI, {
      // Tiempo máximo esperando un servidor disponible (ms)
      serverSelectionTimeoutMS: 8000,
    });
    console.log('MongoDB conectado');
  } catch (error) {
    console.error('Error de conexión a MongoDB:', error.message);
    process.exit(1);
  }
};

export default connectDB;
```

## `assets/plantilla/src/config/env.js`

```js
// ─────────────────────────────────────────────
// env.js — ÚNICO punto donde se lee process.env.
// Valida al arranque y mata el proceso si falta algo.
// ─────────────────────────────────────────────
import 'dotenv/config';
import { z } from 'zod';

const schema = z.object({
  NODE_ENV: z.enum(['development', 'production', 'test']).default('development'),

  // Default visible y versionado; nunca un `|| 5000` escondido en server.js
  PORT: z.coerce.number().int().positive().default(5000),

  // Secreto: sin default. Si falta, la app no arranca.
  MONGO_URI: z.string().min(1, 'MONGO_URI es obligatoria'),
});

const parsed = schema.safeParse(process.env);

// Falla rápido: mejor no levantar que levantar mal configurado
if (!parsed.success) {
  console.error('Configuración inválida:');
  for (const issue of parsed.error.issues) {
    console.error(`  ${issue.path.join('.')}: ${issue.message}`);
  }
  process.exit(1);
}

export const env = parsed.data;
```

## `assets/plantilla/src/controllers/.gitkeep`

```
Carpeta reservada por la estructura estándar de backend-mirlo. Borrar este archivo al agregar el primer módulo.
```

## `assets/plantilla/src/middlewares/.gitkeep`

```
Carpeta reservada por la estructura estándar de backend-mirlo. Borrar este archivo al agregar el primer módulo.
```

## `assets/plantilla/src/models/.gitkeep`

```
Carpeta reservada por la estructura estándar de backend-mirlo. Borrar este archivo al agregar el primer módulo.
```

## `assets/plantilla/src/routes/.gitkeep`

```
Carpeta reservada por la estructura estándar de backend-mirlo. Borrar este archivo al agregar el primer módulo.
```

## `assets/plantilla/src/services/.gitkeep`

```
Carpeta reservada por la estructura estándar de backend-mirlo. Borrar este archivo al agregar el primer módulo.
```

## `assets/plantilla/src/utils/.gitkeep`

```
Carpeta reservada por la estructura estándar de backend-mirlo. Borrar este archivo al agregar el primer módulo.
```
