# 5. Colecciones y Operaciones Útiles

> En cualquier app móvil mostrarás listas: un feed de publicaciones, un catálogo de productos o una lista de chats. En Kotlin, trabajar con listas y transformarlas es simple y expresivo.

---

## 📚 Listas Inmutables vs Mutables

Al igual que con `val` y `var`, Kotlin distingue claramente entre listas de solo lectura y listas modificables:

```kotlin
// 1. Lista de solo lectura (Inmutable - Recomendada por defecto)
val frutas = listOf("Manzana", "Plátano", "Fresa")
// frutas.add("Naranja") // ❌ Error: no existe el método add en List

// 2. Lista modificable (Mutable)
val carrito = mutableListOf("Leche", "Pan")
carrito.add("Huevos")       // ✅ Válido
carrito.remove("Pan")       // ✅ Válido
```

> [!TIP]
> En Jetpack Compose preferimos pasar listas inmutables (`List<T>`) a los componentes visuales para que la interfaz sea predecible y eficiente.

---

## 🛠️ Operaciones Esenciales con Lambdas

Kotlin incluye funciones de orden superior listas para transformar y consultar datos sin escribir bucles `for` manuales:

### 1. `.map { ... }` (Transformar cada elemento)
Convierte una lista de un tipo en otra lista de otro tipo o formato:

```kotlin
val numeros = listOf(1, 2, 3, 4)
val dobles = numeros.map { it * 2 }
// dobles: [2, 4, 6, 8]

val nombres = listOf("juan", "maría", "pedro")
val nombresMayus = nombres.map { it.uppercase() }
// nombresMayus: ["JUAN", "MARÍA", "PEDRO"]
```

### 2. `.filter { ... }` (Filtrar elementos)
Conserva solo los elementos que cumplan una condición (ideal para barras de búsqueda):

```kotlin
data class Contacto(val nombre: String, val esFavorito: Boolean)

val agenda = listOf(
    Contacto("Carlos", true),
    Contacto("Valeria", false),
    Contacto("Diana", true)
)

val soloFavoritos = agenda.filter { it.esFavorito }
// soloFavoritos contendrá a Carlos y Diana

val busqueda = agenda.filter { it.nombre.contains("Car", ignoreCase = true) }
```

### 3. `.firstOrNull { ... }` (Buscar un elemento)
Busca el primer elemento que cumpla la condición. Si no existe, devuelve `null` de forma segura (sin lanzar excepciones):

```kotlin
val contactoBuscado = agenda.firstOrNull { it.nombre == "Valeria" }
println(contactoBuscado?.nombre) // Valeria
```

### 4. `.sortedBy { ... }` (Ordenar)
Ordena los elementos por una propiedad específica:

```kotlin
data class Estudiante(val nombre: String, val nota: Int)

val aula = listOf(
    Estudiante("Mateo", 14),
    Estudiante("Sofia", 19),
    Estudiante("Lucas", 11)
)

val ranking = aula.sortedByDescending { it.nota }
// ranking: Sofia (19), Mateo (14), Lucas (11)
```

---

## 🎨 Conexión con Jetpack Compose

En Compose, una lista de Kotlin se conecta directamente con `LazyColumn` (el equivalente moderno y optimizado de `RecyclerView` o listas scrolleables):

```kotlin
data class Noticia(val id: Int, val titulo: String, val categoria: String)

@Composable
fun ListaNoticias(noticias: List<Noticia>, categoriaFiltro: String) {
    // 1. Filtramos los datos que queremos mostrar
    val noticiasFiltradas = noticias.filter { it.categoria == categoriaFiltro }

    // 2. Renderizamos la lista en Compose
    LazyColumn {
        items(noticiasFiltradas) { noticia ->
            Card(modifier = Modifier.fillMaxWidth().padding(8.dp)) {
                Text(
                    text = noticia.titulo,
                    modifier = Modifier.padding(16.dp)
                )
            }
        }
    }
}
```

---

## 🗺️ Conjuntos y Mapas (Resumen)

Además de listas, tienes otras dos estructuras clave:

| Estructura | Creación | Característica Principal |
|---|---|---|
| **`List`** | `listOf("A", "B", "A")` | Ordenada y **permite duplicados**. |
| **`Set`** | `setOf("A", "B", "A")` | No tiene orden y **no admite duplicados** (solo queda `"A"`, `"B"`). |
| **`Map`** | `mapOf("PE" to "Perú", "MX" to "México")` | Pares de **Clave - Valor**. |

---

## 🎉 ¡Listo para empezar con Jetpack Compose!

Ahora que dominas la sintaxis básica de Kotlin:
- Variables inmutables (`val`) y String templates.
- Funciones con parámetros por defecto y **trailing lambdas**.
- **Null safety** (`?`, `?.`, `?:`).
- `data class` y jerarquías con `sealed interface`.
- Listas y transformaciones (`map`, `filter`).

¡Es momento de construir tus primeras pantallas interactivas!

---

_Siguiente: [Sección 1 — Modifier, Column y Box →](../1.%20fundamentos/1-modifier-column-box.md)_
