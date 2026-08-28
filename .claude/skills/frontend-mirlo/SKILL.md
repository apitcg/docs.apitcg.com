---
name: frontend-mirlo
description: Convenciones obligatorias para frontends en Next.js (App Router) con JS puro sin TypeScript, Tailwind CSS v4 y Sonner. Usar al crear un proyecto Next nuevo, al agregar páginas, componentes o servicios de API, al configurar Tailwind v4 o el alias `@/`, y al revisar o migrar un frontend Next existente. Incluye plantillas comentadas de página, servicio y componente.
---

# frontend-mirlo

> **Este archivo es autocontenido.** Lo que el texto menciona como `assets/...`, `scripts/...` o `references/...` no son archivos aparte: están al final de este mismo documento.

Estándar fijo para cualquier proyecto Next.js. No son sugerencias: si el código entregado no cumple, hay que corregirlo antes de entregar.

## Cuándo aplica

Cualquier proyecto Next.js. Además:

| Situación | Skill que también aplica |
|---|---|
| El backend vive en un repo hermano | `workspace-mirlo` |

## Reglas innegociables

1. **JS puro, sin TypeScript.** Todos los archivos son `.js`. El JSX va **dentro** de archivos `.js`: eso es correcto y esperado. No se crean `.tsx` ni `.jsx`.
2. **Stack**: Next.js (App Router) + Tailwind CSS v4 + Sonner.
3. **Siempre crear** `.gitignore` y `.env.example`. Nunca subir `.env.local`.
4. **Ninguna página supera las 1000 líneas.**
5. **Toda función** (fetch, handlers, helpers) vive fuera de la página, en `services/` o `lib/`, para poder reutilizarla.
6. **Componentes reutilizables** siempre en `components/`.
7. **Comentarios**: cada bloque visual dice qué tocar para cambiar tamaño, color y espaciado. Sin excepciones. Ver la sección "Comentarios en el JSX".

## Estructura de carpetas

```
src/
├── app/
│   ├── layout.js
│   ├── globals.css
│   ├── page.js
│   └── nombre-pagina/
│       └── page.js       ← cada página en su carpeta
├── components/           ← componentes reutilizables
├── lib/                  ← helpers y utilidades
├── services/             ← llamadas a la API
└── hooks/                ← custom hooks
public/
jsconfig.json             ← alias @/ hacia src/
postcss.config.mjs
next.config.js
.env.local
.env.example
.gitignore
package.json
```

## Inicialización

```bash
npx create-next-app@latest nombre-proyecto --js --app --src-dir --tailwind --eslint
cd nombre-proyecto
npm install sonner
mkdir -p src/components src/lib src/services src/hooks
```

> El flag correcto para JavaScript es `--js` (no `--no-typescript`).

`create-next-app` deja ya armados el `jsconfig.json`, el `postcss.config.mjs` y el `globals.css` con Tailwind. **No se reescriben: se verifican** contra las plantillas de `assets/plantillas/`.

Lo primero a verificar es el alias, porque sin él ningún `@/services/...` resuelve:

```json
{
  "compilerOptions": {
    "paths": { "@/*": ["./src/*"] }
  }
}
```

## Tailwind v4

En v4 **no hay `tailwind.config.js`**. La configuración vive en el CSS: `@import "tailwindcss"` y el bloque `@theme` en `src/app/globals.css`.

**Si aparece un `tailwind.config.js` en un proyecto, es v3.** No mezclar las dos formas de configurar: o se migra el proyecto a v4, o se trabaja con la sintaxis de v3, pero nunca las dos a la vez. Los tokens propios (`--color-marca` y similares) van en `@theme` y se usan después como `bg-marca`, `text-marca`.

## Las plantillas

Están en `assets/plantillas/` y se usan de dos formas distintas:

**Se copian tal cual** (ajustando nombres y textos):

| Archivo | Destino |
|---|---|
| `layout.js` | `src/app/layout.js` |
| `gitignore` | `.gitignore` |
| `env.example` | `.env.example` |

**Se verifican contra lo que generó `create-next-app`**: `jsconfig.json`, `postcss.config.mjs`, `globals.css`.

**Se leen como patrón antes de escribir un archivo nuevo** — no se copian, se imitan:

| Plantilla | Se imita al crear |
|---|---|
| `page.js` | cualquier página en `src/app/*/page.js` |
| `nombre.service.js` | cualquier servicio en `src/services/` |
| `NombreCard.js` | cualquier componente en `src/components/` |

Leerlas antes de escribir, no reconstruirlas de memoria: el nivel de comentario que llevan es parte de la convención, no decoración.

## Comentarios en el JSX

Cada bloque visual lleva un comentario que dice **qué tocar** para cambiar tamaño, color y espaciado. No describe lo que hace el código: describe la palanca.

```js
{/*
 * Input de nombre
 * - w-full: ancho completo del contenedor padre (w-64 para ancho fijo)
 * - border border-gray-300: color del borde
 * - rounded-lg: bordes redondeados
 * - px-4 py-2: padding horizontal y vertical (cambia la altura)
 * - focus:ring-2 focus:ring-blue-500: estilo al enfocar
 * - placeholder: texto de ayuda dentro del input
 */}
<input
  type="text"
  placeholder="Escribe el nombre aquí..."
  className="w-full border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
/>
```

Las plantillas `page.js` y `NombreCard.js` son el ejemplo completo de esto aplicado a una página y a un componente.

## Variables de entorno

`.env.example` lleva la URL del backend:

```
NEXT_PUBLIC_API_BASE_URL=http://localhost:5000/api
```

Las `NEXT_PUBLIC_*` **se inlinean en el bundle durante el build**. Cambiarlas en runtime no hace nada: hay que reconstruir. Importa al dockerizar: la imagen se construye con los valores de build, no con los del `docker run`.

> Por esa misma razón, en el frontend `process.env.NEXT_PUBLIC_*` se lee directo en el servicio que la usa. La regla de punto único de lectura de `env-vars-mirlo` es del backend: acá no aplica, porque Next necesita la referencia literal `process.env.NEXT_PUBLIC_X` en el código para poder reemplazarla al compilar.

## Sonner

El `<Toaster />` se monta una sola vez, en `layout.js`. Después, desde cualquier página o componente:

```js
import { toast } from 'sonner';

toast.success('Creado correctamente');
toast.error('Ocurrió un error');
toast.loading('Guardando...');
toast.warning('Revisa los datos');
```

## Cuando una página crece

El límite de 1000 líneas se sostiene sacando cosas de la página, no comprimiéndola:

- Bloques visuales que se repiten o que tienen entidad propia → `components/`
- Llamadas a la API → `services/`
- Helpers y transformaciones de datos → `lib/`
- Estado y efectos que se repiten entre páginas → `hooks/`

## Checklist al crear proyecto nuevo

- [ ] `create-next-app` con `--js --app --src-dir --tailwind`
- [ ] `jsconfig.json` con el alias `@/*` presente
- [ ] Tailwind v4: `@import "tailwindcss"` en `globals.css` y sin `tailwind.config.js`
- [ ] Sonner instalado y `<Toaster />` en `layout.js`
- [ ] Carpetas `components/`, `services/`, `lib/`, `hooks/` creadas
- [ ] `.env.example` con `NEXT_PUBLIC_API_BASE_URL`
- [ ] `.gitignore` con `.env.local` y `.next/`
- [ ] Ninguna página supera 1000 líneas
- [ ] Toda función externa en `services/` o `lib/`
- [ ] Cada bloque visual comentado con qué tocar para cambiar tamaño, color y espaciado

---

# Anexos: plantillas y scripts

## `assets/plantillas/NombreCard.js`

```js
// ─────────────────────────────────────────────
// Componente: NombreCard
// Descripción: card reutilizable para mostrar un item.
// Props: item (objeto con _id, name, description)
// ─────────────────────────────────────────────

export default function NombreCard({ item }) {
  return (
    /*
     * Card contenedor
     * - bg-white: fondo (cambiar para otro color de card)
     * - rounded-xl: bordes redondeados (rounded-lg para menos redondeo)
     * - shadow-md: sombra media (shadow-sm / shadow-lg)
     * - p-6: padding interno
     * - w-full: ancho completo de la celda del grid
     */
    <div className="bg-white rounded-xl shadow-md p-6 w-full">

      {/*
       * Título del item
       * - text-xl font-semibold: tamaño y peso
       * - text-gray-800: color
       * - mb-2: separación inferior
       */}
      <h2 className="text-xl font-semibold text-gray-800 mb-2">
        {item.name}
      </h2>

      {/* Descripción u otros campos */}
      <p className="text-gray-500 text-sm">{item.description}</p>

    </div>
  );
}
```

## `assets/plantillas/env.example`

```
NEXT_PUBLIC_API_BASE_URL=http://localhost:5000/api
```

## `assets/plantillas/gitignore`

```
node_modules/
.env.local
.next/
out/
dist/
*.log
.DS_Store
```

## `assets/plantillas/globals.css`

```css
@import "tailwindcss";

/* Tokens propios del proyecto: colores, fuentes, breakpoints.
   Se usan después como bg-marca, text-marca, etc. */
@theme {
  --color-marca: #1e293b;
}
```

## `assets/plantillas/jsconfig.json`

```json
{
  "compilerOptions": {
    "paths": { "@/*": ["./src/*"] }
  }
}
```

## `assets/plantillas/layout.js`

```js
import { Toaster } from 'sonner';
import './globals.css';

export const metadata = {
  title: 'Mi App',
  description: 'Descripción de la app',
};

export default function RootLayout({ children }) {
  return (
    <html lang="es">
      <body>
        {/* Toaster global de Sonner — posición y duración configurables aquí */}
        <Toaster position="top-right" richColors duration={3000} />
        {children}
      </body>
    </html>
  );
}
```

## `assets/plantillas/nombre.service.js`

```js
// ─────────────────────────────────────────────
// nombre.service.js — todas las llamadas a la API de "Nombre".
// Se importa desde las páginas o componentes que lo necesiten.
// ─────────────────────────────────────────────

const BASE_URL = process.env.NEXT_PUBLIC_API_BASE_URL;

/**
 * Obtiene la lista de items.
 * @returns {Promise<Array>} Items retornados por la API
 */
export async function getNombreItems() {
  const res = await fetch(`${BASE_URL}/nombres`);
  if (!res.ok) throw new Error('Error al obtener los items');
  return res.json();
}

/**
 * Crea un item nuevo.
 * @param {object} data - Campos del item
 * @returns {Promise<object>} Item creado
 */
export async function crearNombreItem(data) {
  const res = await fetch(`${BASE_URL}/nombres`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(data),
  });
  if (!res.ok) throw new Error('Error al crear el item');
  return res.json();
}

/**
 * Actualiza un item existente.
 * @param {string|number} id - _id del item
 * @param {object} data - Campos a actualizar
 * @returns {Promise<object>} Item actualizado
 */
export async function actualizarNombreItem(id, data) {
  const res = await fetch(`${BASE_URL}/nombres/${id}`, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(data),
  });
  if (!res.ok) throw new Error('Error al actualizar el item');
  return res.json();
}

/**
 * Elimina un item.
 * @param {string|number} id - _id del item
 * @returns {Promise<object>} Confirmación de la API
 */
export async function eliminarNombreItem(id) {
  const res = await fetch(`${BASE_URL}/nombres/${id}`, { method: 'DELETE' });
  if (!res.ok) throw new Error('Error al eliminar el item');
  return res.json();
}
```

## `assets/plantillas/page.js`

```js
// ─────────────────────────────────────────────────────────────
// Página: NombrePagina
// Descripción: [qué hace esta página]
// Funciones externas usadas: getNombreItems (services/nombre.service.js)
// ─────────────────────────────────────────────────────────────

'use client';

import { useEffect, useState } from 'react';
import { toast } from 'sonner';
import { getNombreItems } from '@/services/nombre.service.js';
import NombreCard from '@/components/NombreCard.js';

export default function NombrePagina() {
  const [items, setItems] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const cargar = async () => {
      try {
        const data = await getNombreItems();
        setItems(data);
      } catch (error) {
        toast.error('Error al cargar los datos');
      } finally {
        setLoading(false);
      }
    };
    cargar();
  }, []);

  return (
    /*
     * Contenedor principal
     * - min-h-screen: ocupa al menos toda la pantalla
     * - bg-gray-50: color de fondo (cambiar aquí para otro fondo)
     * - p-8: padding general (cambiar para más/menos espacio interior)
     */
    <main className="min-h-screen bg-gray-50 p-8">

      {/*
       * Título de la página
       * - text-3xl font-bold: tamaño y peso (cambiar text-3xl para otro tamaño)
       * - text-gray-800: color del texto
       * - mb-6: separación inferior con el contenido
       */}
      <h1 className="text-3xl font-bold text-gray-800 mb-6">
        Nombre Página
      </h1>

      {/* Estado de carga */}
      {loading && <p className="text-gray-500">Cargando...</p>}

      {/*
       * Grid de cards
       * - grid-cols-1 md:grid-cols-3: 1 columna en móvil, 3 en desktop
       *   (cambiar md:grid-cols-3 para más/menos columnas)
       * - gap-4: separación entre cards
       */}
      <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
        {items.map((item) => (
          <NombreCard key={item._id} item={item} />
        ))}
      </div>

    </main>
  );
}
```

## `assets/plantillas/postcss.config.mjs`

```js
export default {
  plugins: {
    '@tailwindcss/postcss': {},
  },
};
```
