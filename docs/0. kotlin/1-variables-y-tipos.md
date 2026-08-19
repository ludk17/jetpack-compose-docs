# 1. Variables, Tipos de Datos y Plantillas de Texto

> Fundamentos esenciales de Kotlin para empezar con Jetpack Compose.

---

## 🏷️ Variables: `val` vs `var`

En Kotlin, para guardar información usamos dos palabras clave principales: `val` y `var`.

Imagina que estás organizando cosas en tu habitación:
- **`val` (Valor / Inmutable):** Es como un **tatuaje** o escribir con **tinta indeleble**. Una vez que le asignas un valor, **nunca más puede cambiar**.
- **`var` (Variable / Mutable):** Es como una **pizarra con tiza**. Puedes borrar y escribir un valor nuevo tantas veces como quieras.

```kotlin
// Inmutable (Recomendado por defecto)
val nombreApp = "MiSuperApp"
// nombreApp = "OtroNombre" // ❌ Error de compilación: no se puede reasignar

// Mutable
var contador = 0
contador = 1 // ✅ Válido
contador = contador + 1 // ✅ Válido
```

> [!TIP]
> **Regla de oro en Kotlin y Compose:**  
> Usa siempre `val` por defecto. Solo usa `var` si sabes con certeza que el valor necesita cambiar con el tiempo.

### 💡 ¿Por qué es vital en Jetpack Compose?
En Jetpack Compose, las funciones de interfaz (`@Composable`) se ejecutan muchas veces (un proceso llamado *Recomposición*). Mantener variables con `val` evita efectos secundarios no deseados e inconsistencias visuales.

---

## 🧱 Tipos de Datos Básicos

Kotlin es un lenguaje de **tipado estático** (cada dato tiene un tipo bien definido), pero tiene **inferencia de tipos** inteligente: el compilador adivina el tipo automáticamente si le das un valor inicial.

| Tipo | Descripción | Ejemplo |
|---|---|---|
| `String` | Texto entre comillas dobles | `val titulo = "Bienvenido"` |
| `Int` | Números enteros (32 bits) | `val edad = 20` |
| `Double` | Números decimales de alta precisión | `val precio = 19.99` |
| `Float` | Decimales flotantes (terminan en `f`) | `val opacidad = 0.85f` |
| `Boolean` | Valores de verdad (`true` o `false`) | `val estaLogueado = true` |

### Especificar el tipo explícitamente vs Inferencia

```kotlin
// Con inferencia automática de tipo (la forma más común en Kotlin):
val mensaje = "Hola a todos" // Kotlin detecta que es String
val cantidad = 5             // Kotlin detecta que es Int

// Especificando el tipo manualmente (con dos puntos `: Tipo`):
val usuario: String = "Ana"
val puntaje: Int = 100
val altura: Float = 1.75f
val activo: Boolean = false
```

---

## 💬 String Templates (Plantillas de Texto)

Olvídate de concatenar texto con `+` como en otros lenguajes (`"Hola " + nombre + " tienes " + edad + " años"`). En Kotlin usamos el símbolo **`$`** directamente dentro de las comillas dobles.

### Variables simples: `$variable`
```kotlin
val nombre = "Carlos"
val edad = 22

println("Hola $nombre, tienes $edad años.")
// Salida: Hola Carlos, tienes 22 años.
```

### Expresiones complejas: `${expresión}`
Si quieres calcular algo, llamar a un método o acceder a una propiedad, envuélvelo entre llaves `${...}`:

```kotlin
val precio = 50.0
val descuento = 10.0

println("Total a pagar: S/ ${precio - descuento}")
// Salida: Total a pagar: S/ 40.0

val producto = "zapatillas"
println("Producto: ${producto.uppercase()}")
// Salida: Producto: ZAPATILLAS
```

---

## 🎨 Conexión con Jetpack Compose

En Compose usarás variables y String templates todo el tiempo para mostrar datos en pantalla dentro del componente `Text`:

```kotlin
@Composable
fun PerfilUsuario() {
    val nombre = "Lucía"
    val seguidores = 1420
    
    Column {
        Text(text = "Perfil de $nombre")
        Text(text = "Seguidores: $seguidores")
        Text(text = "Estado: ${if (seguidores > 1000) "Popular ⭐" else "Nuevo 🌱"}")
    }
}
```

---

## ⚠️ Errores Comunes de Principiantes

1. **Olvidar la `f` en números decimales Float:**
   En Compose muchas medidas o alphas usan `Float`. Escribir `0.5` crea un `Double`, debes escribir `0.5f`.
2. **Intentar cambiar un `val`:**
   Si necesitas modificarlo, cámbialo a `var` o revisa si tu diseño puede usar una nueva constante `val`.
3. **Usar comillas simples para texto:**
   En Kotlin, `'A'` (comillas simples) es de tipo `Char` (un solo caracter). Para texto (`String`) siempre debes usar comillas dobles `"Texto"`.

---

_Siguiente: [Funciones y Sintaxis Moderna →](2-funciones-y-lambdas.md)_
