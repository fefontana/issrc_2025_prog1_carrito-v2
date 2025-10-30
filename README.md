# issrc_2025_prog1_carrito-v2


Excelente pregunta 👌 — el **spread operator (`...`)** es una herramienta poderosa en JavaScript, especialmente útil cuando querés **copiar objetos** para evitar modificar los originales (como tus productos del inventario).

Vamos a verlo paso a paso **en el contexto de tu carrito** 👇

---

## 🧩 1️⃣ ¿Por qué usar `...` al agregar al carrito?

Cuando haces esto:

```js
const itemCarrito = producto;
carrito.push(itemCarrito);
```

👉 Ambos apuntan **al mismo objeto en memoria**.
Si después cambias `itemCarrito.cantidad`, **también estás modificando el `producto` original**, y eso puede causar errores en el stock o inconsistencias.

Por eso usamos el spread operator:

```js
carrito.push({ ...producto });
```

🔹 Esto crea una **copia superficial** del objeto `producto`, es decir, un **nuevo objeto independiente** con las mismas propiedades.

---

## 🧩 2️⃣ Dónde aplicarlo en tu código

En tu función `agregarAlCarrito()` actual, lo mejor es aplicarlo justo al momento de **insertar en el carrito**, para que no haya referencias cruzadas:

### ✅ Versión mejorada

```js
function agregarAlCarrito() {
  const id = parseInt(prompt("🔢 Ingresá el ID del repuesto que deseas agregar:"));
  const producto = productos.find(p => p.id === id);

  if (!producto) return alert("❌ Producto no encontrado.");
  if (producto.stock <= 0) return alert("⚠️ No hay stock disponible.");

  const cantidad = parseInt(prompt(`📦 Stock disponible: ${producto.stock}\n¿Cuántas unidades deseas agregar?`));
  if (isNaN(cantidad) || cantidad <= 0) return alert("⚠️ Cantidad no válida.");
  if (cantidad > producto.stock) return alert("❌ No hay suficiente stock.");

  producto.stock -= cantidad;

  const itemCarrito = carrito.find(p => p.id === id);
  if (itemCarrito) {
    // 🔄 Ya existe → solo aumentar cantidad
    itemCarrito.cantidad += cantidad;
  } else {
    // 🧠 Spread operator: copia del producto + nueva propiedad "cantidad"
    carrito.push({ ...producto, cantidad });
  }

  alert(`✅ Agregaste ${cantidad} x ${producto.nombre} al carrito.`);
}
```

👉 **En este punto, `...producto` copia el objeto original**, pero lo guarda como un nuevo elemento en el array `carrito`, junto con la propiedad adicional `cantidad`.

---

## 🧩 3️⃣ Qué ventajas tiene

✅ Evita modificar directamente el array `productos`.
✅ Facilita la manipulación del carrito sin afectar el stock original.
✅ Permite que cada producto del carrito tenga sus propias propiedades (por ejemplo, `cantidad`, `subtotal`, `observaciones`, etc.).
✅ Hace el código más limpio y legible que usar `Object.assign()`.

---

## 🧠 Bonus: diferencias con `Object.assign()`

Estas dos líneas hacen lo mismo:

```js
carrito.push({ ...producto, cantidad });
carrito.push(Object.assign({}, producto, { cantidad }));
```

Pero el **spread operator** es más corto, legible y moderno (ES6+).
Solo hay que recordar que **no hace copia profunda**, o sea, si el producto tuviera objetos anidados (como `categoria: { id, nombre }`), esa parte seguiría compartiendo referencia.

---

## ✅ Conclusión

Usá el spread operator **solo al copiar objetos del inventario al carrito** o en cualquier lugar donde quieras **duplicar datos sin alterar los originales**.

---

✅ Características del nuevo diseño

Lista de productos: cada producto tiene un botón “Agregar”.

Carrito dinámico:

Mostrar nombre, precio, cantidad y subtotal.

Botones ➕ / ➖ para modificar cantidad mientras haya stock.

Botón Eliminar para sacar el producto.

Total actualizado automáticamente.

Vaciar carrito con confirmación.