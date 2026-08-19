# 4. Clases, Data Classes y Modelado de Datos

> En una aplicación móvil necesitas representar información del mundo real: usuarios, productos, mensajes de chat y estados de pantalla. Kotlin simplifica esto al máximo.

---

## 🏛️ Clases y Propiedades Básicas

En Kotlin, el constructor principal se define directamente en la cabecera de la clase, sin necesidad de escribir código repetitivo (como getters, setters o constructores vacíos):

```kotlin
class Mascota(
    val nombre: String,
    var edad: Int,
    val especie: String = "Perro"
) {
    fun ladrar() {
        println("$nombre dice: ¡Guau guau!")
    }
}

// Instanciar un objeto (¡No se usa la palabra 'new'!):
val miPerro = Mascota("Firulais", 3)
println(miPerro.nombre) // Firulais
miPerro.edad = 4        // ✅ edad es 'var', se puede modificar
miPerro.ladrar()
```

---

## 📦 `data class` (Clases de Datos)

En el desarrollo de apps, el 90% de las clases son simplemente **contenedores de datos** (modelos de datos para una API, base de datos o listas en pantalla).

Al agregar la palabra `data` antes de `class`, Kotlin genera automáticamente bajo el capó:
- `toString()` para imprimir el objeto de forma legible (`Persona(nombre=Ana, edad=25)`).
- `equals()` y `hashCode()` para comparar objetos por sus valores y no por su dirección en memoria.
- `copy()` para crear copias modificando solo ciertos campos (clave para Compose).

```kotlin
data class Producto(
    val id: Int,
    val nombre: String,
    val precio: Double,
    val enStock: Boolean = true
)

val producto1 = Producto(1, "Laptop Gamer", 3500.0)
val producto2 = Producto(1, "Laptop Gamer", 3500.0)

// 1. Impresión limpia:
println(producto1) // Producto(id=1, nombre=Laptop Gamer, precio=3500.0, enStock=true)

// 2. Comparación por contenido:
println(producto1 == producto2) // true (En clases normales sería false)
```

### 🔄 La función `.copy()` y la Inmutabilidad
En Compose se recomienda que los datos sean inmutables (`val`). Si quieres modificar un dato, no alteras el objeto original; creas una copia con el nuevo valor usando `.copy()`:

```kotlin
val productoDescuento = producto1.copy(precio = 3199.0)
println(productoDescuento) 
// Producto(id=1, nombre=Laptop Gamer, precio=3199.0, enStock=true)
```

---

## 🚦 `sealed class` y `sealed interface` (Tipos Sellados)

### 📦 La analogía del paquete de compras

Imagina que compraste algo por internet y abres la app para ver el estado de tu pedido. Solo pueden pasar **3 cosas posibles en todo el universo**:

1. ⏳ **En camino:** No tienes el paquete todavía, solo estás esperando. *(No necesitas datos adicionales)*.
2. ✅ **Entregado:** ¡Llegó tu paquete! Y dentro viene tu producto. *(Trae datos: el producto entregado)*.
3. ❌ **Cancelado o Fallido:** Hubo un problema con la entrega. *(Trae datos: el motivo del fallo)*.

¿Cómo representamos esto en código?

---

### 🤔 ¿Por qué no usar un `enum` o una `interface` normal?

Para entender el poder de `sealed`, mira esta comparación:

| Opción | ¿Permite opciones fijas? | ¿Cada opción puede tener sus propios datos? | Veredicto para Compose |
|---|---|---|---|
| **`enum class`** | ✅ Sí (`CARGANDO`, `EXITO`, `ERROR`) | ❌ No (todas las opciones deben tener los mismos campos fijos). No puedes hacer que `ERROR` guarde un mensaje y `EXITO` guarde una lista de productos. | ⚠️ Se queda corto. |
| **`interface` normal** | ❌ No (cualquiera puede crear nuevas clases en cualquier parte del proyecto). | ✅ Sí. | ⚠️ Obliga a poner `else` en todos los `when`. |
| **`sealed interface`** | ✅ **Sí** (la lista de opciones está "sellada" y cerrada). | ✅ **Sí** (cada opción tiene solo los datos que necesita). | 🏆 **Perfecto para estados de pantalla.** |

---

### 💻 ¿Cómo se escribe en Kotlin?

La palabra **`sealed`** significa *"cerrado con sello"*: nadie fuera de este archivo puede inventar más opciones.

```kotlin
// 1. Definimos la categoría general "sellada"
sealed interface EstadoPantalla {

    // Opción A: Cargando (No lleva datos, por eso usamos 'object' que es una sola instancia)
    object Cargando : EstadoPantalla

    // Opción B: Éxito (Lleva la lista de productos descargados)
    data class Exito(val productos: List<String>) : EstadoPantalla

    // Opción C: Error (Lleva el texto del error para mostrárselo al usuario)
    data class Error(val mensaje: String) : EstadoPantalla
}
```

> [!TIP]
> - Usa **`object`** cuando la opción **no guarde datos** (como `Cargando` o `Vacio`).
> - Usa **`data class`** cuando la opción **sí transporte información** (como `Exito(val datos)` o `Error(val mensaje)`).

---

### 🎯 La gran ventaja: El compilador trabaja por ti con `when`

Cuando combinas una `sealed interface` con `when`, Kotlin sabe **exactamente cuáles son todas las opciones posibles**.

Si en el futuro agregas una 4ta opción (por ejemplo, `SinConexion`) y olvidas manejarla en tu pantalla, **el código no compilará hasta que la atiendas**. ¡Es un escudo contra bugs!

```kotlin
fun mostrarEstado(estado: EstadoPantalla) {
    // No se necesita rama 'else' porque el compilador ya conoce todas las ramas selladas
    when (estado) {
        is EstadoPantalla.Cargando -> {
            println("Girando rueda de carga...")
        }
        is EstadoPantalla.Exito -> {
            // Smart Cast: Kotlin sabe automáticamente que aquí 'estado' tiene el campo .productos
            println("Mostrando ${estado.productos.size} productos en pantalla")
        }
        is EstadoPantalla.Error -> {
            // Smart Cast: Kotlin sabe automáticamente que aquí 'estado' tiene el campo .mensaje
            println("Mostrando alerta roja: ${estado.mensaje}")
        }
    }
}
```

---

## 🎨 Conexión Real con Jetpack Compose

En Compose, una pantalla típicamente recibe un `EstadoPantalla` y dibuja una UI completamente distinta según el caso:

```kotlin
@Composable
fun PantallaProductos(estado: EstadoPantalla) {
    when (estado) {
        // Caso 1: Si está cargando, mostramos el spinner circular
        is EstadoPantalla.Cargando -> {
            Box(modifier = Modifier.fillMaxSize(), contentAlignment = Alignment.Center) {
                CircularProgressIndicator()
            }
        }

        // Caso 2: Si hubo éxito, dibujamos la lista con los productos recibidos
        is EstadoPantalla.Exito -> {
            LazyColumn {
                items(estado.productos) { producto ->
                    Text(text = "📦 $producto", modifier = Modifier.padding(16.dp))
                }
            }
        }

        // Caso 3: Si falló, mostramos el texto de error en color rojo
        is EstadoPantalla.Error -> {
            Box(modifier = Modifier.fillMaxSize(), contentAlignment = Alignment.Center) {
                Text(text = "❌ Error: ${estado.mensaje}", color = Color.Red)
            }
        }
    }
}
```

---

_Siguiente: [Colecciones y Operaciones Útiles →](5-colecciones-y-operaciones.md)_
