---
name: git-mirlo
description: Flujo obligatorio de git y pull requests: traer `main` antes de tocar nada, detectar la zona de riesgo entre la rama y `main`, resolver conflictos dejando funcionando los dos lados, y volver a `main` apenas se abre el PR. Usar cuando se pide abrir un PR, subir cambios o dejar algo listo para revisión; ante cualquier operación de git (pull, rama nueva, commit, push, merge, conflicto); y al empezar a trabajar sobre un repo compartido.
---

# git-mirlo

> **Este archivo es autocontenido.** Lo que el texto menciona como `assets/...`, `scripts/...` o `references/...` no son archivos aparte: están al final de este mismo documento.

## Cuándo aplica

Se activa cuando el usuario:

- pide **abrir un PR** o subir cambios ("hazme el PR", "sube esto", "deja esto listo para revisión");
- pide cualquier operación de git: `pull`, rama nueva, commit, push, merge, resolver un conflicto;
- va a empezar a trabajar sobre un repo que comparte con más gente.

No corre sola en medio de una edición de código.

> **Pedir el PR dispara el flujo completo, no solo el push.** El orden siempre es: traer `main` → revisar choques → resolver → push → PR → volver a `main`. Nunca hacer solo el push y abrir el PR a ciegas.

## Reglas

1. **Pull antes de tocar nada.** Ninguna edición empieza sobre una copia vieja del repo.
2. **Verificar que los cambios entrantes no se pisen con lo modificado.** Si se pisan, se integran los dos para que ambos funcionen; nunca se elige un lado y se borra el otro.
3. **Apenas se abre el PR, volver a `main`.** No quedarse trabajando en la rama del PR.
4. **Nunca resolver un conflicto descartando el trabajo ajeno** sin avisar y explicar por qué.
5. **Nunca `push --force`** sobre `main` ni sobre ramas que otra persona pueda estar usando.

## El flujo cuando se pide el PR

```bash
# 0. Dejar el árbol limpio: lo que no está commiteado no viaja al PR
#    y puede perderse en el checkout del paso 1
git status

# 1. Traer main actualizado
git checkout main && git pull

# 2. Volver a la rama y meterle main
git checkout nombre-del-cambio
git merge main

# 3. Revisar la zona de riesgo (ver sección siguiente)
bash scripts/zona-de-riesgo.sh

# 4. Subir
git push -u origin nombre-del-cambio
gh pr create --fill      # o abrir el PR desde la URL que imprime el push

# 5. Volver a main de inmediato
git checkout main && git pull
```

El merge de `main` se hace **antes** de abrir el PR, no después: los conflictos se resuelven en local, con el proyecto corriendo, no en la interfaz de GitHub.

### Si se llega al PR sin haber hecho pull antes

Pasa. No es motivo para saltarse nada: los pasos 1 y 2 hacen exactamente ese trabajo, solo que más tarde. Traer `main`, revisar los choques con calma y **probar el proyecto** antes de subir, porque puede haber semanas de cambios ajenos entrando de golpe.

## Verificar que los cambios no se pisen

```bash
bash scripts/zona-de-riesgo.sh          # compara contra main
bash scripts/zona-de-riesgo.sh develop  # o contra otra base
```

El script cruza las dos listas y marca con `[!]` los puntos calientes. Hace esto:

```bash
# Archivos que toca mi rama
git diff --name-only main...HEAD

# Archivos que cambiaron en main desde que salí
git diff --name-only HEAD...main
```

La intersección de esas dos listas es la zona de riesgo. Revisar **archivo por archivo**: que el script no marque nada no significa que no haya que mirar, significa que ningún archivo fue tocado por ambos lados.

### Los dos tipos de choque

| Tipo | Cómo se ve | Qué hacer |
|---|---|---|
| **Conflicto de texto** | git marca el archivo con `<<<<<<<` | Resolver dejando **las dos** funcionalidades |
| **Choque silencioso** | El merge pasa limpio y algo igual se rompe | Detectarlo a mano: es el peligroso |

### Puntos calientes

Revisar siempre estos archivos cuando aparecen en ambas listas:

| Archivo | Qué mirar | Skill relacionada |
|---|---|---|
| `src/app.js` | Dos ramas agregando entradas al array `routes[]` casi siempre chocan. Deben quedar **todas** las rutas registradas, no las de un lado | `backend-mirlo` |
| `package.json` / `package-lock.json` | Resolver el `package.json` a mano dejando las dependencias de ambos y regenerar el lock con `npm install`. **Nunca editar el lock a mano** | — |
| `.env.example` | Deben quedar las variables de los dos lados. Si una rama agregó una obligatoria, avisar para que se agregue al `.env` real y al `docker-compose.yml` | `env-vars-mirlo` |
| Modelos | Dos ramas creando modelos distintos con el mismo nombre de colección, o con `_id` slug que puede colisionar | `models-mirlo` |
| `globals.css` / tokens de Tailwind | Dos definiciones del mismo token | `frontend-mirlo` |
| `docker-compose.yml` | Puertos o `container_name` repetidos entre servicios | — |
| Servicios del frontend | Dos versiones de la misma función de fetch apuntando a rutas distintas | `frontend-mirlo` |

### Cómo se resuelve

1. Entender **qué quería lograr cada lado**. No mirar solo el texto.
2. Escribir una versión que cumpla **los dos** objetivos.
3. Correr el proyecto y probar las dos funcionalidades, no solo la propia.
4. **Reportar** qué se integró y qué se cambió para que convivieran.
5. Si de verdad son incompatibles (uno borra lo que el otro necesita), **detenerse y preguntar**. No decidir en silencio.

## Volver a main después del PR

- **No quedarse en la rama del PR.** El trabajo siguiente parte de `main` actualizado, en una rama nueva.
- Si el PR necesita correcciones, se vuelve **a propósito** a esa rama, se corrige, se hace push y se regresa a `main` de nuevo.
- Si el PR ya se mergeó, borrar la rama local: `git branch -d nombre-del-cambio`.
- Nunca dejar la sesión terminada parada en una rama de PR: el próximo cambio se hace encima sin darse cuenta.

## Al empezar un trabajo nuevo

```bash
git checkout main
git pull
git checkout -b nombre-del-cambio
```

## Nombres de rama y mensajes de commit

El set **no tiene todavía** convención de nombres de rama ni de mensajes de commit. Mientras no exista:

```bash
git log --oneline -20    # ver qué usa el repo
```

Seguir ese estilo. Si el repo no usa ninguno de forma consistente, **preguntar en vez de inventar un estándar nuevo**.

## Nunca

- `git push --force` sobre `main` o sobre una rama compartida.
- `git reset --hard` o `git checkout .` sin avisar antes: se pierde trabajo sin vuelta atrás.
- Resolver un conflicto quedándose con un solo lado porque era más rápido.
- Commitear el `.env`, `node_modules/` o `.next/`.
- Abrir el PR y seguir commiteando en esa rama como si fuera la de trabajo.

## Checklist antes de dar el PR por listo

- [ ] ¿Se trajo `main` a la rama antes de subir?
- [ ] ¿Se compararon los archivos que toca la rama contra los que cambiaron en `main`?
- [ ] ¿Se revisaron los puntos calientes (`app.js`, `package.json`, `.env.example`, modelos)?
- [ ] ¿Los conflictos quedaron resueltos dejando funcionando las dos partes?
- [ ] ¿Se probó el proyecto después de integrar?
- [ ] ¿Se reportó qué se integró?
- [ ] ¿Se volvió a `main` y se hizo `pull` después de abrir el PR?

---

# Anexos: plantillas y scripts

## `scripts/zona-de-riesgo.sh`

```bash
#!/usr/bin/env bash
# ─────────────────────────────────────────────
# zona-de-riesgo.sh — archivos tocados a la vez por esta rama y por la base.
# Marca con [!] los puntos calientes conocidos del set -mirlo.
#
# Uso: bash scripts/zona-de-riesgo.sh [rama-base]    (por defecto: main)
# ─────────────────────────────────────────────
set -uo pipefail

BASE="${1:-main}"

if ! git rev-parse --verify "$BASE" >/dev/null 2>&1; then
  echo "No existe la rama base '$BASE'. ¿Hiciste 'git fetch'?" >&2
  exit 1
fi

# Archivos que toca esta rama desde que salió de la base
mios=$(git diff --name-only "${BASE}...HEAD" | sort -u)

# Archivos que cambiaron en la base desde que salí
ajenos=$(git diff --name-only "HEAD...${BASE}" | sort -u)

if [ -z "$mios" ] || [ -z "$ajenos" ]; then
  echo "Sin zona de riesgo: uno de los dos lados no tiene cambios."
  exit 0
fi

interseccion=$(comm -12 <(printf '%s\n' "$mios") <(printf '%s\n' "$ajenos"))

if [ -z "$interseccion" ]; then
  echo "Sin zona de riesgo: ningún archivo fue tocado por ambos lados."
  exit 0
fi

# Puntos calientes: archivos donde el merge puede pasar limpio y romper igual
CALIENTES='src/app\.js|package\.json|package-lock\.json|\.env\.example|docker-compose\.yml|globals\.css|src/models/|src/services/'

echo "Zona de riesgo — revisar archivo por archivo:"
echo
while IFS= read -r archivo; do
  if printf '%s' "$archivo" | grep -Eq "$CALIENTES"; then
    echo "  [!] $archivo"
  else
    echo "      $archivo"
  fi
done <<< "$interseccion"
echo
echo "[!] = punto caliente: ver la sección 'Puntos calientes' del SKILL.md"
```
