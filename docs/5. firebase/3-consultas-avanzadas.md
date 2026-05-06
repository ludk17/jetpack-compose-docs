# 🔍 Consultas Avanzadas: Búsqueda y Paginación

Cuando tenemos miles de documentos, no podemos descargarlos todos a la vez. Aquí es donde entran las consultas, los filtros y la paginación.

## 🔎 Búsquedas y Filtros

Firestore permite filtrar datos usando el método `where()`.

### Búsqueda Exacta
```kotlin
// Buscar estudiantes de una carrera específica
db.collection("estudiantes")
    .whereEqualTo("carrera", "Ingeniería")
    .get()
```

### Búsqueda por Rango (Filtros)
```kotlin
// Buscar estudiantes con promedio mayor a 15
db.collection("estudiantes")
    .whereGreaterThan("promedio", 15.0)
    .orderBy("promedio") // Importante: Si usas rangos, debes ordenar por ese campo
    .get()
```

### Búsqueda por Prefijo (Simulando LIKE)
Aunque Firestore no tiene `LIKE`, podemos simular un **"empieza con"** usando un rango de strings. El caracter `\uf8ff` es un valor muy alto en Unicode, lo que nos permite marcar un final para el rango.

```kotlin
fun buscarPorNombre(texto: String) {
    db.collection("estudiantes")
        .orderBy("nombre")
        .startAt(texto)
        .endAt(texto + "\uf8ff")
        .get()
}
```

> **Nota:** Esto solo funciona para el **inicio** del texto (ej: "Jua" encuentra "Juan"). No sirve para buscar texto en medio (ej: "ua" no encontrará "Juan"). Para eso se requieren servicios externos.

---

## 📑 Paginación

La paginación evita que la app se sature cargando solo "pedazos" de la lista (ej. 10 en 10). 

> [!IMPORTANT]
> **Sin ordenamiento no hay paginación:** Para que Firestore sepa qué documento sigue después de otro, la lista debe estar ordenada por algún campo (ej. nombre, fecha, puntaje). Si no usas `.orderBy()`, los resultados podrían cambiar de posición y la paginación fallará.

---

### Paso 1: Cargar la primera página
```kotlin
var ultimoDocumento: DocumentSnapshot? = null

fun cargarPrimeraPagina() {
    db.collection("estudiantes")
        .orderBy("nombre", Query.Direction.ASCENDING) // También puedes usar DESCENDING
        .limit(10) // Solo traemos 10
        .get()
        .addOnSuccessListener { result ->
            // Guardamos el último para saber dónde empezar la siguiente
            ultimoDocumento = result.documents.lastOrNull()
            
            // ... procesar lista ...
        }
}
```

### Paso 2: Cargar la siguiente página
```kotlin
fun cargarSiguientePagina() {
    ultimoDocumento?.let { snapshot ->
        db.collection("estudiantes")
            .orderBy("nombre")
            .startAfter(snapshot) // Empezar DESPUÉS del último que vimos
            .limit(10)
            .get()
            .addOnSuccessListener { result ->
                ultimoDocumento = result.documents.lastOrNull()
                
                // ... agregar nuevos elementos a la lista existente ...
            }
    }
}
```

---

## ⚡ Índices: El mapa del tesoro

Cuando haces consultas complejas (combinando varios `where` o un `where` con un `orderBy`), Firestore te pedirá crear un **Índice**.

- **¿Qué es?** Es como el índice de un libro que le dice a Firestore exactamente dónde están los datos sin tener que leer todo el "libro" (la colección).
- **¿Cómo se crea?** Si intentas ejecutar una consulta que requiere un índice y no existe, Firebase te dará un **link en el Logcat**. Solo haz clic en él y Firebase lo creará por ti.

---

## 💡 Comparativa de Consultas

| Requisito | Método | Nota |
| :--- | :--- | :--- |
| **Igualdad** | `.whereEqualTo()` | El más común. |
| **Prefijo (LIKE)** | `.startAt(t).endAt(t + "\uf8ff")` | Simula "empieza con". |
| **Desigualdad** | `.whereNotEqualTo()` | Excluir valores. |
| **Límite** | `.limit(n)` | Esencial para rendimiento. |
| **Orden** | `.orderBy("campo")` | Por defecto es ascendente. |
| **Continuar** | `.startAfter(doc)` | Usado para paginación. |

---

_Siguiente: [Ejercicios Prácticos →](4-ejercicios.md)_
