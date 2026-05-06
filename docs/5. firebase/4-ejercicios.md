# 📝 Ejercicios: Firebase Firestore

Pon en práctica lo aprendido sobre Firestore con estos retos técnicos.

---

## 🛒 Reto 1: Catálogo de Productos (CRUD)

Imagina que estás creando una app para una bodega.

1.  Crea una `data class Producto` con: `id`, `nombre`, `precio` y `stock`.
2.  Crea una función para **agregar** un producto nuevo.
3.  Crea una función para **actualizar el stock** de un producto dado su ID.
4.  Crea una función para **eliminar** productos que ya no tengan stock (stock == 0).

---

## 🔍 Reto 2: Buscador de Libros (Filtros)

En una colección llamada `libros`:

1.  Realiza una consulta para obtener todos los libros del autor **"Gabriel García Márquez"**.
2.  Realiza una consulta para obtener libros cuyo **precio sea menor a 50 soles** y ordénalos por título.
3.  **Bonus:** ¿Qué pasa si intentas ordenar por `titulo` pero filtras por `precio`? Recuerda el tema de los **índices**.

---

## 📑 Reto 3: Lista Infinita (Paginación)

Simula el comportamiento de una red social:

1.  Crea una colección `posts` con campos `texto` y `fecha`.
2.  Implementa una función `cargarNuevosPosts()` que traiga los últimos **5 posts** (ordenados por fecha descendente).
3.  Implementa un botón "Cargar más" que traiga los siguientes 5 posts sin repetir los anteriores.

---

> [!TIP]
> Para probar estos ejercicios, no necesitas una UI compleja. Puedes usar `Log.d()` o `println()` dentro de los listeners de éxito para verificar que los datos están llegando correctamente.

---

_Volver al inicio: [README.md](../../README.md)_
