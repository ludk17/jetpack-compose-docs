# 💡 Estado y Remember en Jetpack Compose

En Compose, la interfaz es reactiva. Esto significa que la pantalla se redibuja automáticamente cada vez que los datos cambian. Para lograr esto, necesitamos manejar el Estado y la Memoria.

## 1. El concepto de Estado (State)

El Estado es cualquier valor que cambia con el tiempo y afecta lo que el usuario ve.

* Ejemplos: El texto en un buscador, si un Checkbox está marcado o si un botón de "Cargar" es visible.

En Compose, una variable normal no puede actualizar la pantalla por sí sola. Necesitamos que sea un "Estado observable". Para eso usamos mutableStateOf(valor). Esta función le dice a Compose: "Vigila esta variable; si cambia, redibuja la parte de la pantalla que la usa".

## 2. El problema de la "Amnesia": remember

Las funciones Composable se ejecutan muchas veces. Este proceso se llama Recomposición.

* El Problema: Cada vez que la función se vuelve a ejecutar, las variables locales se reinician. Si el usuario escribe una letra, la función se reinicia y la variable vuelve a su valor original (vacío).

* La Solución: remember. Es como una caja fuerte. Guarda el valor la primera vez que se ejecuta la función y, en las siguientes ejecuciones, te devuelve el valor guardado en lugar de reiniciarlo.

## 3. Ejemplo: De "Ciego" a "Inteligente"

### ❌ El error común (Sin memoria)

Aquí, la variable nombre vuelve a ser "" cada vez que el usuario intenta escribir una letra, porque la función "olvida" el cambio al recomponer.

```kotlin
@Composable
fun EjemploErroneo() {
    var nombre = "" // Se reinicia en cada recomposición
    
    TextField(
        value = nombre,
        onValueChange = { nombre = it } // El cambio se pierde al instante
    )
}
```

### ✅ La forma correcta (Con Memoria)

Usamos remember para mantener el dato y mutableStateOf para que Compose sepa que debe redibujar.

```kotlin
@Composable
fun EjemploCorrecto() {
    // Usamos "by" para manejar la variable de forma directa
    // Nota: Requiere importar runtime.getValue y runtime.setValue
    var nombre by remember { mutableStateOf("") }
    
    TextField(
        value = nombre,
        onValueChange = { nombre = it }, // "it" es el nuevo texto ingresado
        label = { Text("Escribe tu nombre") }
    )
}
```

## 4. ¿Qué es ese famoso it?

En Kotlin, cuando una función recibe un solo parámetro (en este caso, el texto que el usuario acaba de teclear), no es obligatorio ponerle un nombre manual. Kotlin usa automáticamente la palabra clave it.

Es simplemente un atajo para decir: "El dato que me acabas de entregar".

## 5. Formulario Práctico Completo

Un ejemplo real de cómo gestionar múltiples estados en un formulario:

```kotlin
@Composable
fun FormularioRegistro() {
    // 1. Declaramos la "memoria" de cada campo
    var email by remember { mutableStateOf("") }
    var password by remember { mutableStateOf("") }
    var aceptoTerminos by remember { mutableStateOf(false) }

    Column(modifier = Modifier.padding(16.dp)) {
        
        OutlinedTextField(
            value = email,
            onValueChange = { email = it },
            label = { Text("Correo electrónico") },
            modifier = Modifier.fillMaxWidth()
        )

        OutlinedTextField(
            value = password,
            onValueChange = { password = it },
            label = { Text("Contraseña") },
            modifier = Modifier.fillMaxWidth()
        )

        Row(verticalAlignment = Alignment.CenterVertically) {
            Checkbox(
                checked = aceptoTerminos,
                onCheckedChange = { aceptoTerminos = it }
            )
            Text("Acepto los términos y condiciones")
        }

        Button(
            onClick = { /* Lógica de registro */ },
            modifier = Modifier.fillMaxWidth(),
            enabled = email.isNotEmpty() && password.isNotEmpty() && aceptoTerminos
        ) {
            Text("Registrar")
        }
    }
}
```

### 📌 Resumen de conceptos

| Término | Definición técnica | Analogía | 
| ----- | ----- | ----- | 
| Estado | Datos que definen la UI en un momento dado. | El "qué" se muestra. | 
| Recomposición | El acto de volver a ejecutar el código de la UI. | El "refresco" de pantalla. | 
| remember | Preservar un objeto a través de la recomposición. | Una caja fuerte (memoria). | 
| mutableStateOf | Crea un estado que Compose puede observar. | Un sensor que avisa cambios. | 
| by | Delegación de propiedades en Kotlin. | Un atajo para no escribir .value. | 

*Anterior: [← Componentes de Formulario](3-componentes-formulario.md)*
*Siguiente: [Listas (LazyColumn) →](../2.%20listas/README.md)*