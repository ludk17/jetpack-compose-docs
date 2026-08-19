# 3. Null Safety y Control de Flujo

> En Android, el error más común históricamente era el temido `NullPointerException` (cuando la app se cierra inesperadamente). Kotlin resuelve esto directamente desde su sistema de tipos.

---

## 🛡️ Null Safety (Seguridad ante Nulos)

En Kotlin, por defecto **ninguna variable puede ser nula (`null`)**. Si intentas asignarle `null`, el código ni siquiera compilará.

```kotlin
var nombre: String = "Andrea"
// nombre = null // ❌ Error de compilación: Null can not be a value of a non-null type String
```

### 1. Tipos Anulables (`Type?`)
Si necesitas explícitamente que una variable pueda no tener valor (ser `null`), debes agregar un signo de interrogación `?` al final del tipo:

```kotlin
var correo: String? = "andrea@correo.com"
correo = null // ✅ Válido: es un String anulable (String?)
```

### 2. Llamada Segura (Safe Call: `?.`)
No puedes acceder a métodos de una variable anulable directamente porque podría ser `null`. El operador `?.` solo ejecuta la acción si la variable NO es nula; si es nula, devuelve `null` sin que la app crashee.

```kotlin
val telefono: String? = null

// println(telefono.length) // ❌ Error de compilación
println(telefono?.length)   // ✅ Imprime "null" de forma segura, no se cae la app
```

### 3. Operador Elvis (`?:`)
El operador Elvis te permite proporcionar un **valor por defecto** si una expresión resulta ser `null`:

```kotlin
val apodo: String? = null

// Si apodo es null, usa "Invitado"
val nombreAMostrar = apodo ?: "Invitado"
println(nombreAMostrar) // Imprime: Invitado
```

> [!NOTE]
> Se llama *Operador Elvis* porque si giras la cabeza, `?:` parece el peinado y los ojos de Elvis Presley. 🎸

### 4. Operador de Afirmación No Nula (`!!`)
Le dice al compilador: *"Te juro al 100% que esto no es null, y si lo es, deja que la app crashee"*.

```kotlin
val codigoSeguridad: String? = "1234"
val longitud = codigoSeguridad!!.length // ⚠️ Úsalo solo si estás totalmente seguro
```

> [!CAUTION]
> Evita usar `!!` en tu código diario. Si la variable llega a ser `null`, tu aplicación se cerrará con un `NullPointerException`. Usa `?.` o `?:` en su lugar.

---

## 🔀 Control de Flujo Moderno

### 1. `if` / `else` como Expresión
En Kotlin, `if` puede retornar un valor directamente (reemplazando al operador ternario `condicion ? a : b` de otros lenguajes):

```kotlin
val saldo = 120.0
val estadoCuenta = if (saldo > 0) "Con fondos" else "En quiebra"
```

### 2. La estructura `when`
`when` es el reemplazo moderno, limpio y superpotente de `switch`. Puede evaluar valores, rangos, tipos y condiciones booleanas:

```kotlin
val rol = "ADMIN"

val mensajePermiso = when (rol) {
    "ADMIN" -> "Acceso total al sistema"
    "DOCENTE" -> "Acceso a subir notas"
    "ALUMNO" -> "Acceso a ver clases"
    else -> "Rol desconocido o sin permisos"
}

// Evaluando rangos de números:
val calificacion = 17
val resultado = when (calificacion) {
    in 18..20 -> "Excelente 🏆"
    in 13..17 -> "Aprobado 👍"
    in 0..12 -> "Desaprobado ❌"
    else -> "Nota inválida"
}
```

---

## 🎨 Conexión con Jetpack Compose

En Compose, `if` y `when` son las herramientas principales para **mostrar u ocultar componentes** en la pantalla según el estado actual:

```kotlin
@Composable
fun EstadoDescarga(porcentaje: Int, error: String?) {
    Column {
        // 1. Mostrar mensaje de error solo si existe (Null check)
        if (error != null) {
            Text(text = "Error: $error", color = Color.Red)
        }

        // 2. Mostrar diferente interfaz según el avance con when
        when {
            porcentaje == 100 -> Text("¡Descarga completada! ✅")
            porcentaje in 1..99 -> Text("Descargando: $porcentaje% ⏳")
            else -> Text("En espera... ⏸️")
        }
    }
}
```

---

## 📋 Resumen Rápido

| Operador / Estructura | Significado | Ejemplo |
|---|---|---|
| `Tipo?` | Puede contener un valor o ser `null` | `val bio: String? = null` |
| `?.` | Ejecuta solo si no es nulo (Safe Call) | `usuario?.nombre` |
| `?:` | Si es nulo, usa el valor de la derecha (Elvis) | `usuario?.nombre ?: "Sin nombre"` |
| `if (...) a else b` | Retorna un valor según la condición | `val color = if (ok) Green else Red` |
| `when (x)` | Evalúa múltiples ramas de forma exhaustiva | `when (estado) { ... }` |

---

_Siguiente: [Clases, Data Classes y Modelado de Datos →](4-clases-y-data-classes.md)_
