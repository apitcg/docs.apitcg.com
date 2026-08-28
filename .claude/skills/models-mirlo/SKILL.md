---
name: models-mirlo
description: Convenciones para modelos de Mongoose y el CRUD obligatorio que va con cada uno (controlador, rutas y registro en el array `routes[]`). Usar al crear o modificar cualquier modelo de Mongoose. El tipo de `_id` es opt-in: por defecto el ObjectId de Mongoose, y slug o counter solo cuando el usuario los pide explícitamente. Tiene precedencia sobre los ejemplos de modelo de `backend-mirlo`.
---

# models-mirlo

> **Este archivo es autocontenido.** Lo que el texto menciona como `assets/...`, `scripts/...` o `references/...` no son archivos aparte: están al final de este mismo documento.

## El `_id` es opt-in — leer esto primero

**Si el usuario no escribió `slug` ni `counter` en su mensaje, se usa el `_id` por defecto de Mongoose (ObjectId) y se sigue adelante.** Sin preguntar, sin proponer alternativas, sin mencionar que existen.

```
¿El usuario pidió slug o counter?
├── No  → modelo por defecto. No abrir references/. Fin de la decisión.
├── slug    → leer references/id-slug.md
└── counter → leer references/id-counter.md
```

Los archivos de `references/` y las plantillas de `assets/plantillas/counter/` y `assets/plantillas/slug/` **solo se abren cuando el usuario pidió ese tipo de `_id`**.

## Cuándo aplica

Al crear o modificar cualquier modelo de Mongoose. Tiene precedencia sobre los ejemplos de modelo de `backend-mirlo`.

| Situación | Skill que también aplica |
|---|---|
| Estructura del proyecto, alias `@/`, array `routes[]` | `backend-mirlo` |

## Reglas

1. **Por defecto, `_id` de Mongoose (ObjectId).** No se declara, no se pregunta, no se sugiere cambiarlo.
2. **Slug o counter solo cuando el usuario lo pide**, escribiendo la palabra o describiendo claramente uno de los dos.
3. **Cada modelo lleva su CRUD completo**: controlador + rutas + registro en el array `routes[]` de `app.js`.
4. **ES Modules** en todos los archivos.
5. **Opciones del schema**: siempre `timestamps: true` y `versionKey: false`. `_id: false` va **solo** cuando se declara un `_id` propio (slug o counter); con ObjectId no se pone, porque le quita a Mongoose el campo que sí necesita.
6. **Los modelos se exportan por defecto** (`export default`), y se importan sin llaves.

```js
// Opciones del schema en el caso por defecto — sin _id: false
{
  timestamps: true,
  versionKey: false,
}
```

> **Mongoose 9**: los hooks `pre` **ya no reciben `next()`**. Un hook `async` termina cuando termina la promesa. Si el proyecto sigue en Mongoose 8, agregar el parámetro `next` y llamarlo al final.

## Las plantillas

En `assets/plantillas/`. Se leen como patrón antes de escribir, no se reconstruyen de memoria:

| Plantilla | Se imita al crear |
|---|---|
| `Product.js` | el modelo, en `src/models/` |
| `product.controller.js` | el controlador, en `src/controllers/` |
| `product.routes.js` | las rutas, en `src/routes/` |

Los nombres de archivo son parte de la convención: `<Recurso>.js` en singular capitalizado para el modelo, `<recurso>.controller.js` y `<recurso>.routes.js` en minúscula.

## Los tres archivos van juntos

Un modelo sin su CRUD está incompleto. El orden:

1. `src/models/<Recurso>.js`
2. `src/controllers/<recurso>.controller.js`
3. `src/routes/<recurso>.routes.js`
4. **Registro en `src/app.js`** — el único paso que modifica un archivo existente, y por eso el que más se olvida:

```js
import productRoutes from '@/routes/product.routes.js';

const routes = [
  { path: '/api/users', route: userRoutes },
  { path: '/api/products', route: productRoutes },
];
```

Nunca con un `app.use()` suelto.

## Sobre el manejo de errores del controlador

El mismo controlador sirve el `_id` que sirva. Lo único que cambia con ObjectId es que un id mal formado tira `CastError`, y eso es un **400, no un 500**.

Los `catch` que devuelven 400, 404 y 409 son los que `backend-mirlo` manda mantener en Express 5: distinguen un caso y devuelven otro status. Los que solo devuelven 500 son los que esa misma skill permite sacar, porque el middleware de errores de `app.js` ya hace eso. La plantilla los deja puestos; si el proyecto los saca, se sacan solo los de 500.

> Se usa `remove` y no `delete` porque `delete` es palabra reservada en JS.

## Nombres y status codes

| Elemento | Convención | Ejemplo |
|---|---|---|
| Funciones del controlador | `create`, `list`, `getById`, `update`, `remove` | — |
| Argumento de `getNextSequence` | plural, minúscula, inglés | `'products'` |
| Ruta HTTP | inglés plural | `/api/products` |

| Situación | Status |
|---|---|
| Creado | 201 |
| Éxito | 200 |
| Validación fallida o id inválido | 400 |
| No encontrado | 404 |
| Duplicado | 409 |
| Error de servidor | 500 |

## Checklist

- [ ] ¿El usuario pidió slug o counter? Si no, `_id` por defecto y sin preguntar.
- [ ] ¿`timestamps: true` y `versionKey: false`?
- [ ] ¿`_id: false` solo si se declaró un `_id` propio?
- [ ] ¿Hook `pre('save')` sin `next()` (Mongoose 9), si aplica?
- [ ] ¿`export default` del modelo?
- [ ] ¿Controlador, rutas y registro en el array `routes[]`?
- [ ] ¿`CastError` devuelto como 400 y no como 500?
- [ ] ¿Cada campo del schema comentado y cada función del controlador con JSDoc?

---

# Anexos: referencias

## `_id` tipo counter

**Leer esto solo si el usuario pidió counter.** Si no lo pidió, el modelo va con el ObjectId por defecto.

ID numérico auto-incremental, para productos, pedidos o facturas.

## Los dos archivos de infraestructura

Se copian **una sola vez por proyecto**, no uno por modelo. Si ya existen, no se tocan:

| Plantilla | Destino |
|---|---|
| `assets/plantillas/counter/Counter.js` | `src/models/Counter.js` |
| `assets/plantillas/counter/getNextSequence.js` | `src/utils/getNextSequence.js` |

## El modelo

```js
import mongoose from 'mongoose';
import { getNextSequence } from '@/utils/getNextSequence.js';

const productSchema = new mongoose.Schema(
  {
    // ID numérico auto-incremental, asignado en el hook pre('save')
    _id: { type: Number },

    // Nombre visible del producto
    name: {
      type: String,
      required: [true, 'El nombre es requerido'],
      trim: true,
    },
    // ... resto de campos, cada uno comentado
  },
  {
    timestamps: true,
    versionKey: false,
    _id: false,
  }
);

// Mongoose 9: el hook no recibe next(); basta con que la promesa termine
productSchema.pre('save', async function () {
  if (this.isNew) {
    this._id = await getNextSequence('products');
  }
});

export default mongoose.model('Product', productSchema);
```

## Qué revisar

- `_id: false` en las opciones del schema, porque se declaró un `_id` propio.
- El argumento de `getNextSequence` va en **plural, minúscula, inglés**: `'products'`.
- El hook `pre('save')` **no recibe `next()`** en Mongoose 9. Si el proyecto sigue en Mongoose 8, agregar el parámetro y llamarlo al final.
- La secuencia se asigna en `pre('save')`, así que hay que crear con `new Model()` + `.save()` o con `Model.create()`, nunca con `insertMany` ni con `updateOne({ upsert: true })`, que no disparan el hook.

## `_id` tipo slug

**Leer esto solo si el usuario pidió slug.** Si no lo pidió, el modelo va con el ObjectId por defecto.

ID string legible para URLs: `"brandon-olivares"`, `"cafe-con-leche"`, `"pokemon-tcg"`.

## El archivo de infraestructura

Se copia **una sola vez por proyecto**. Si ya existe, no se toca:

| Plantilla | Destino |
|---|---|
| `assets/plantillas/slug/slug.js` | `src/utils/slug.js` |

## El modelo

```js
import mongoose from 'mongoose';
import { generateSlugId } from '@/utils/slug.js';

const categorySchema = new mongoose.Schema(
  {
    // ID legible generado desde `name` (ej: "cafe-con-leche")
    _id: {
      type: String,
      lowercase: true,
      trim: true,
      minlength: 1,
      maxlength: 200,
      match: /^[a-z0-9-]+$/,
    },

    // Nombre visible de la categoría, base del slug
    name: {
      type: String,
      required: true,
      trim: true,
      index: true,
    },
    // ... resto de campos, cada uno comentado
  },
  {
    timestamps: true,
    versionKey: false,
    _id: false,
  }
);

categorySchema.pre('save', generateSlugId);

export default mongoose.model('Category', categorySchema);
```

## Qué revisar

- `_id: false` en las opciones del schema, porque se declaró un `_id` propio.
- El `match: /^[a-z0-9-]+$/` tiene que coincidir con lo que produce `generateSlug`. Si se cambia uno, se cambia el otro.
- El hook usa `generateSlugId` importado, no una función anónima: así la validación de "name es requerido" queda en un solo lugar.

## Ojo con los slugs al actualizar

`findByIdAndUpdate` **no dispara `pre('save')`**, así que cambiar `name` **no** regenera el `_id`. Es el comportamiento correcto —el id es la URL y no debe moverse— pero hay que tenerlo claro:

- Después de un `update` que cambia `name`, el slug queda apuntando al nombre viejo. No es un bug.
- Dos nombres distintos pueden generar el mismo slug ("Café Latte" y "cafe latte"). El segundo `save` falla con `11000`, que el controlador ya devuelve como 409.

---

# Anexos: plantillas y scripts

## `assets/plantillas/Product.js`

```js
// ─────────────────────────────────────────────
// Modelo: Product
// _id por defecto de Mongoose (ObjectId): no se declara.
// ─────────────────────────────────────────────
import mongoose from 'mongoose';

const productSchema = new mongoose.Schema(
  {
    // Nombre visible del producto
    name: {
      type: String,
      required: [true, 'El nombre es requerido'],
      trim: true,
    },

    // Precio actual en CLP
    price: { type: Number, default: 0 },
    // ... resto de campos, cada uno comentado
  },
  {
    timestamps: true,
    versionKey: false,
  }
);

export default mongoose.model('Product', productSchema);
```

## `assets/plantillas/counter/Counter.js`

```js
// ─────────────────────────────────────────────
// Modelo: Counter
// Guarda la última secuencia entregada por colección.
// Se crea una sola vez por proyecto, no uno por modelo.
// ─────────────────────────────────────────────
import mongoose from 'mongoose';

const counterSchema = new mongoose.Schema(
  {
    // Nombre de la colección a la que pertenece la secuencia
    _id: { type: String },
    // Último número entregado
    seq: { type: Number, default: 0 },
  },
  { versionKey: false }
);

export default mongoose.model('Counter', counterSchema);
```

## `assets/plantillas/counter/getNextSequence.js`

```js
import Counter from '@/models/Counter.js';

/**
 * Incrementa y retorna el siguiente número de secuencia de una colección.
 * @param {string} name - Nombre de la colección en plural (ej: 'products')
 * @returns {Promise<number>} Siguiente número de la secuencia
 */
export const getNextSequence = async (name) => {
  const counter = await Counter.findByIdAndUpdate(
    name,
    { $inc: { seq: 1 } },
    { new: true, upsert: true }
  );
  return counter.seq;
};
```

## `assets/plantillas/product.controller.js`

```js
import Product from '@/models/Product.js';

/**
 * Crea un producto.
 * @param {Request} req - Body: campos del producto
 * @param {Response} res - 201 con el creado, 409 si duplicado, 400 si validación falla
 */
export const create = async (req, res) => {
  try {
    const doc = new Product(req.body);
    await doc.save();
    res.status(201).json(doc);
  } catch (error) {
    if (error.code === 11000) {
      return res.status(409).json({ message: 'Ya existe un registro con ese identificador.' });
    }
    res.status(400).json({ message: error.message });
  }
};

/**
 * Retorna la lista paginada de productos.
 * @param {Request} req - Query: page (default 1), limit (default 20)
 * @param {Response} res - { data, total, page, totalPages }
 */
export const list = async (req, res) => {
  try {
    const { page = 1, limit = 20 } = req.query;
    const skip = (Number(page) - 1) * Number(limit);
    // Documentos y total en paralelo para no encadenar dos viajes a la base
    const [docs, total] = await Promise.all([
      Product.find().skip(skip).limit(Number(limit)).lean(),
      Product.countDocuments(),
    ]);
    res.json({ data: docs, total, page: Number(page), totalPages: Math.ceil(total / Number(limit)) });
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
};

/**
 * Retorna un producto por su _id.
 * @param {Request} req - Params: id
 * @param {Response} res - 200 con el producto, 404 si no existe, 400 si el id es inválido
 */
export const getById = async (req, res) => {
  try {
    const doc = await Product.findById(req.params.id).lean();
    if (!doc) return res.status(404).json({ message: 'No encontrado.' });
    res.json(doc);
  } catch (error) {
    // Con _id ObjectId, un id mal formado no es un error del servidor
    if (error.name === 'CastError') {
      return res.status(400).json({ message: 'Identificador inválido.' });
    }
    res.status(500).json({ message: error.message });
  }
};

/**
 * Actualiza un producto por su _id.
 * @param {Request} req - Params: id | Body: campos a actualizar
 * @param {Response} res - 200 con el actualizado, 404 si no existe, 400 si falla
 */
export const update = async (req, res) => {
  try {
    const doc = await Product.findByIdAndUpdate(req.params.id, req.body, {
      new: true,
      runValidators: true,
    }).lean();
    if (!doc) return res.status(404).json({ message: 'No encontrado.' });
    res.json(doc);
  } catch (error) {
    if (error.code === 11000) {
      return res.status(409).json({ message: 'Ya existe un registro con ese identificador.' });
    }
    res.status(400).json({ message: error.message });
  }
};

/**
 * Elimina un producto por su _id.
 * @param {Request} req - Params: id
 * @param {Response} res - 200 con confirmación, 404 si no existe, 400 si el id es inválido
 */
export const remove = async (req, res) => {
  try {
    const doc = await Product.findByIdAndDelete(req.params.id).lean();
    if (!doc) return res.status(404).json({ message: 'No encontrado.' });
    res.json({ message: 'Eliminado correctamente.', id: req.params.id });
  } catch (error) {
    if (error.name === 'CastError') {
      return res.status(400).json({ message: 'Identificador inválido.' });
    }
    res.status(500).json({ message: error.message });
  }
};
```

## `assets/plantillas/product.routes.js`

```js
import { Router } from 'express';
import { create, list, getById, update, remove } from '@/controllers/product.controller.js';

const router = Router();

// POST /api/products — crea un producto. Body: { name, ... }. 201 | 409 | 400
router.post('/', create);

// GET /api/products — lista paginada. Query: page, limit. 200 { data, total, page, totalPages }
router.get('/', list);

// GET /api/products/:id — obtiene uno por id. 200 | 404 | 400
router.get('/:id', getById);

// PUT /api/products/:id — actualiza por id. Body: campos a cambiar. 200 | 404 | 400
router.put('/:id', update);

// DELETE /api/products/:id — elimina por id. 200 | 404 | 400
router.delete('/:id', remove);

export default router;
```

## `assets/plantillas/slug/slug.js`

```js
/**
 * Genera un slug a partir de un texto.
 * @param {string} text - Texto a convertir
 * @returns {string} Slug en kebab-case y sin acentos. Ej: "Café Latte" → "cafe-latte"
 */
export function generateSlug(text) {
  return text
    .toLowerCase()
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '') // eliminar acentos
    .replace(/[^a-z0-9\s-]/g, '')    // solo letras, números, espacios y guiones
    .trim()
    .replace(/\s+/g, '-')            // espacios a guiones
    .replace(/-+/g, '-');            // guiones repetidos a uno solo
}

/**
 * Hook de Mongoose: genera el _id como slug a partir del campo `name`.
 * @returns {void}
 * Uso: schema.pre('save', generateSlugId)
 */
export function generateSlugId() {
  if (this.isNew && !this._id && this.name) {
    this._id = generateSlug(this.name);
  }
  if (!this._id) {
    throw new Error('No se pudo generar el _id: el campo "name" es requerido');
  }
}
```
