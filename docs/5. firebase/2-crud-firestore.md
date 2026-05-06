# 🏗️ CRUD en Firestore

CRUD es el acrónimo de **C**reate (Crear), **R**ead (Leer), **U**pdate (Actualizar) y **D**elete (Eliminar). Veremos cómo realizar estas operaciones básicas.

## 📦 El Modelo de Datos

Primero, definimos cómo se ve un "Estudiante" en nuestro código Kotlin:

```kotlin
data class Estudiante(
    val id: String = "", // Lo usaremos para guardar el ID del documento
    val nombre: String = "",
    val codigo: String = "",
    val promedio: Double = 0.0
)
```

---

## 🚀 Operaciones Básicas

Para interactuar con Firestore, primero obtenemos una instancia:
```kotlin
val db = Firebase.firestore
```

### 1. Create (Crear)
Podemos agregar un documento de dos formas principales: usando un `HashMap` o enviando un objeto de nuestra clase directamente.

#### Opción A: Usando un HashMap
Es útil cuando no tienes una clase definida o solo quieres enviar un par de campos.

```kotlin
fun agregarEstudianteMapa(nombre: String, codigo: String) {
    val datos = hashMapOf(
        "nombre" to nombre,
        "codigo" to codigo,
        "promedio" to 0.0
    )

    db.collection("estudiantes")
        .add(datos)
        .addOnSuccessListener { doc ->
            println("Agregado con ID: ${doc.id}")
        }
}
```

#### Opción B: Usando el Objeto directamente (Recomendado)
Firestore puede convertir automáticamente una `data class` a un documento, siempre que sus propiedades tengan valores por defecto.

```kotlin
fun agregarEstudianteObjeto(estudiante: Estudiante) {
    db.collection("estudiantes")
        .add(estudiante)
        .addOnSuccessListener { doc ->
            println("Estudiante guardado: ${doc.id}")
        }
}
```

### 2. Read (Leer)
Podemos leer todos los documentos de una colección.

```kotlin
fun leerEstudiantes(onSuccess: (List<Estudiante>) -> Unit) {
    db.collection("estudiantes")
        .get()
        .addOnSuccessListener { result ->
            val lista = result.map { document ->
                document.toObject(Estudiante::class.java).copy(id = document.id)
            }
            onSuccess(lista)
        }
}
```

### 3. Update (Actualizar)
Para actualizar, necesitamos el ID del documento. Tenemos dos opciones dependiendo de si queremos cambiar un solo campo o todo el objeto.

#### Opción A: Actualizar un solo atributo (`.update()`)
Es la forma más eficiente si solo quieres cambiar un dato específico sin tocar los demás.

```kotlin
fun actualizarPromedio(id: String, nuevoPromedio: Double) {
    db.collection("estudiantes").document(id)
        .update("promedio", nuevoPromedio)
        .addOnSuccessListener { println("Campo actualizado!") }
}
```

#### Opción B: Actualizar todo el documento (`.set()`)
Esta opción reemplaza **todo** el contenido del documento por el nuevo objeto que envíes.

```kotlin
fun sobrescribirEstudiante(estudiante: Estudiante) {
    // Usamos el ID del estudiante para saber qué documento reemplazar
    db.collection("estudiantes").document(estudiante.id)
        .set(estudiante)
        .addOnSuccessListener { println("Documento completo actualizado!") }
}
```

> [!CAUTION]
> Ten cuidado con `.set()`: Si el objeto que envías no tiene ciertos campos que el documento original sí tenía, esos campos **se borrarán**. Si solo quieres actualizar un par de campos, usa `.update()`.

### 4. Delete (Eliminar)
Igual que actualizar, requiere el ID.

```kotlin
fun eliminarEstudiante(estudianteId: String) {
    db.collection("estudiantes").document(estudianteId)
        .delete()
        .addOnSuccessListener { 
            println("Estudiante eliminado") 
        }
}
```

---

## 📊 Resumen de Métodos

| Acción | Método en Firestore | Necesita ID? |
| :--- | :--- | :--- |
| **Crear** | `.add()` o `.set()` | No (add) / Si (set) |
| **Leer todos** | `.get()` sobre la colección | No |
| **Leer uno** | `.get()` sobre el documento | Si |
| **Actualizar** | `.update()` | Si |
| **Borrar** | `.delete()` | Si |

---

> [!IMPORTANT]
> Los métodos de Firestore son **asíncronos**. Siempre debes manejar el resultado dentro de los listeners (`addOnSuccessListener`) o usando Corrutinas (`await()`).

_Siguiente: [Consultas, Paginación y Búsqueda →](3-consultas-avanzadas.md)_
