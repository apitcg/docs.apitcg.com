---
name: workspace-mirlo
description: Cómo trabajar cuando el frontend y el backend viven en repos separados: dónde está el backend hermano, cómo inferir su nombre, qué convenciones propias de ese repo leer antes de tocarlo, y qué trazar y reportar antes de cambiar algo. Usar solo si el proyecto está partido en dos repositorios; si todo está en una sola carpeta, no aplica.
---

# workspace-mirlo

> **Este archivo es autocontenido.** Lo que el texto menciona como `assets/...`, `scripts/...` o `references/...` no son archivos aparte: están al final de este mismo documento.

## Cuándo aplica

**Solo cuando el proyecto tiene frontend y backend en repos separados.** Si todo está en una sola carpeta, ignorar esta skill.

Pero **nunca asumir monolito** salvo que el usuario lo diga: la verificación es mirar la carpeta padre, no suponer.

```bash
bash scripts/ubicar-backend.sh
```

Ese script hace los pasos 1 y 2 del flujo: infiere el nombre del backend, lo localiza y lista sus archivos de convenciones. Si no encuentra ninguna carpeta `api.*`, ahí sí es monolito y esta skill no corre.

## Reglas

1. **Nunca asumir monolito** salvo que el usuario lo diga.
2. **El backend está en la carpeta padre** del frontend, como repositorio hermano.
3. **Si el usuario no dice el nombre exacto del backend**, inferirlo como `api.(nombredelfront).cl`. Si esa carpeta no existe, probar `.com`. Si hay varias carpetas `api.`, preguntar.
4. **Al revisar**, leer todo lo relacionado con lo que se pide: modelo, controlador, rutas, servicios y middlewares involucrados.
5. **Al implementar**, hay libertad total para modificar el backend, respetando las convenciones propias de ese proyecto.

## Localización del backend

```
C:\Repositorios\
├── roadtohero\              ← frontend (proyecto activo)
└── api.roadtohero.com\      ← backend (repositorio hermano)
```

| Frontend | Backend inferido |
|---|---|
| `discard` | `api.discard.cl` |
| `roadtohero` | `api.roadtohero.cl` → si no existe, `api.roadtohero.com` |
| `mi-tienda` | `api.mi-tienda.cl` |

El script cubre los cuatro casos: encontrado por convención, una sola candidata con otro nombre (confirmar antes de usarla), varias candidatas (preguntar) y ninguna (monolito).

## Qué convenciones mandan

Antes de tocar nada, buscar en el backend, en este orden:

```
../(backend)/.claude/skills/**/SKILL.md
../(backend)/.cursor/rules/*.mdc
../(backend)/*.mdc
../(backend)/CLAUDE.md
```

**Lo que diga ese repo gana.** Las convenciones `-mirlo` son el default, no la autoridad: si el backend tiene reglas propias, se siguen esas aunque contradigan a `backend-mirlo`. Solo si el repo no tiene nada propio aplican `backend-mirlo` y `models-mirlo`.

## Flujo al revisar

Trazar el flujo completo del dato pedido. Para cualquier revisión —por ejemplo "verifica cómo trae la información del usuario"— leer **todos** estos:

```
src/models/        → schema y campos
src/routes/        → qué endpoints expone
src/controllers/   → lógica de cada endpoint
src/services/      → lógica de negocio si existe
src/middlewares/   → autenticación, validación, permisos
```

Y reportar **antes** de cambiar:

- Qué endpoints existen para lo pedido
- Qué devuelve cada uno (campos y estructura)
- Qué middlewares interceptan la ruta
- Si hay algo roto, faltante o inconsistente

## Flujo al implementar

1. **Leer las convenciones del backend** antes de escribir código.
2. **Seguir lo que ya existe**: estructura de modelos, nombres de archivos, patrón de controladores.
3. Libertad para crear o modificar archivos, sin romper convenciones existentes.
4. **Verificar que las rutas nuevas queden registradas** en el array `routes[]` de `app.js`.
5. **Reportar** qué archivos se crearon o modificaron.

## Un cambio, dos repos

Son dos repositorios: un cambio que toca los dos lados son **dos commits, dos ramas y dos PRs**, y `git-mirlo` corre completa en cada uno. Nada de dar por subido el frontend porque se subió el backend.

Al reportar, decir en qué repo quedó cada archivo. Y si el backend agregó una variable obligatoria, el frontend necesita su `NEXT_PUBLIC_*` correspondiente en el `.env.example` de su propio repo.

## Ejemplos

| Petición | Acción |
|---|---|
| "Revisa por qué no llega la info del usuario" | `User.js` → rutas de usuario → controlador → middlewares de auth |
| "Verifica el endpoint de productos" | `Product.js` → `product.routes.js` → `product.controller.js` |
| "Implementa el carrito de compras" | Leer convenciones → `models-mirlo` (preguntar slug/counter) → modelo, controlador, rutas → registrar en `routes[]` |
| "Por qué falla el login" | Ruta `/auth/login` → controlador → middleware de JWT/bcrypt |

> El "preguntar slug/counter" del carrito es la excepción, no la regla: `models-mirlo` dice que el `_id` es opt-in y que si el usuario no lo pidió va ObjectId sin preguntar. Ver esa skill antes de preguntar nada.

## Checklist

- [ ] ¿Es monolito? Si sí, ignorar esta skill.
- [ ] ¿Se conoce el nombre exacto del backend? Si no, inferir con `.cl` primero.
- [ ] ¿Se leyeron las convenciones internas del backend antes de actuar?
- [ ] ¿Se trazó el flujo completo (modelo → ruta → controlador → middleware)?
- [ ] Si se va a implementar, ¿se reportaron los archivos a tocar?
- [ ] ¿El cambio quedó subido en **los dos** repos que tocó?

---

# Anexos: plantillas y scripts

## `scripts/ubicar-backend.sh`

```bash
#!/usr/bin/env bash
# ─────────────────────────────────────────────
# ubicar-backend.sh — encuentra el repo hermano del backend y lista
# los archivos de convenciones que haya que leer antes de tocarlo.
#
# Uso: bash scripts/ubicar-backend.sh [nombre-del-front] [carpeta-padre]
#      Por defecto usa el nombre de la carpeta actual y su padre (..).
# ─────────────────────────────────────────────
set -uo pipefail

FRONT="${1:-$(basename "$PWD")}"
PADRE="${2:-..}"

if [ ! -d "$PADRE" ]; then
  echo "No existe la carpeta padre '$PADRE'." >&2
  exit 1
fi

echo "Frontend: $FRONT"
echo "Buscando en: $(cd "$PADRE" && pwd)"
echo

# 1. Inferencia por convención: .cl primero, .com después
BACKEND=""
for tld in cl com; do
  candidato="$PADRE/api.$FRONT.$tld"
  if [ -d "$candidato" ]; then
    BACKEND="$candidato"
    echo "Backend encontrado por convención: api.$FRONT.$tld"
    break
  fi
done

# 2. Si la inferencia falló, mirar qué carpetas api.* hay
if [ -z "$BACKEND" ]; then
  mapfile -t otros < <(find "$PADRE" -maxdepth 1 -type d -name 'api.*' | sort)

  if [ "${#otros[@]}" -eq 0 ]; then
    echo "No hay ninguna carpeta api.* en el padre."
    echo "Puede que el proyecto sea monolito: en ese caso workspace-mirlo no aplica."
    exit 0
  fi

  if [ "${#otros[@]}" -gt 1 ]; then
    echo "Hay varias carpetas api.* — PREGUNTAR al usuario cuál es:"
    printf '  %s\n' "${otros[@]##*/}"
    exit 0
  fi

  BACKEND="${otros[0]}"
  echo "No existe api.$FRONT.cl ni api.$FRONT.com."
  echo "Hay una sola candidata: ${BACKEND##*/} — confirmar antes de usarla."
fi

echo
echo "Ruta: $(cd "$BACKEND" && pwd)"
echo
echo "Convenciones propias del backend (leer antes de tocar nada):"

encontrado=0
while IFS= read -r archivo; do
  echo "  $archivo"
  encontrado=1
done < <(
  find "$BACKEND/.claude/skills" -name 'SKILL.md' 2>/dev/null
  find "$BACKEND/.cursor/rules" -maxdepth 1 -name '*.mdc' 2>/dev/null
  find "$BACKEND" -maxdepth 1 -name '*.mdc' 2>/dev/null
  find "$BACKEND" -maxdepth 1 -name 'CLAUDE.md' 2>/dev/null
)

if [ "$encontrado" -eq 0 ]; then
  echo "  (ninguna) — aplican backend-mirlo y models-mirlo"
fi
```
