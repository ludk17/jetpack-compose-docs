# Estado y Remember en Jetpack Compose

En Compose, las funciones se vuelven a ejecutar cada vez que algo cambia. Para que la pantalla sea interactiva y "recuerde" datos mientras el usuario escribe o hace clic, necesitamos manejar el **Estado**.

---

## 1. ¿Qué es el Estado (State)?

Imagina que el estado es una **variable que tiene superpoderes**. Cuando el valor de esa variable cambia, Jetpack Compose lo detecta automáticamente y vuelve a dibujar la parte de la pantalla que usa ese valor. Este proceso se llama **Recomposición**.

## 2. El problema del olvido: remember

Cuando una función se vuelve a ejecutar (recomposición), todas las variables locales se reinician. Si escribes algo en un cuadro de texto, ¡se borraría al instante!

Para evitar esto usamos `remember`. Su trabajo es:
> "Guarda este valor y no lo olvides aunque la función se vuelva a ejecutar".

---

## 3. Ejemplo: De variable normal a Estado

### Sin Estado (No funciona)
```kotlin
@Composable
fun EjemploSinEstado() {
    var nombre = "" // Cada vez que el usuario escriba, esta variable volverá a ser ""
    
    TextField(
        value = nombre,
        onValueChange = { nombre = it } // El cambio se pierde
    )
}
```

### Con Estado y Remember (¡Funciona!)
```kotlin
@Composable
fun EjemploConEstado() {
    // Definimos el estado con superpoderes y memoria
    var nombre by remember { mutableStateOf("") }
    
    TextField(
        value = nombre,
        onValueChange = { nombre = it }, // Al cambiar el valor, Compose redibuja la pantalla
        label = { Text("Escribe tu nombre") }
    )
}
```

---

## 4. ¿Qué es el `it`?

Si te fijas, en `onValueChange = { nombre = it }`, aparece la palabra **`it`**.

En Kotlin, cuando una función recibe un solo dato (en este caso, el nuevo texto que el usuario acaba de escribir), no hace falta ponerle un nombre aburrido como `nuevoTexto`. Kotlin le asigna automáticamente el nombre **`it`**.

Es una forma rápida de decir: **"Lo que sea que el usuario acaba de escribir"**.

---

## 5. Formulario Simple con Estado

Aquí tienes un formulario sencillo donde los campos realmente funcionan:

```kotlin
@Composable
fun FormularioInteractivo() {
    // 1. Definimos los estados de cada campo
    var email by remember { mutableStateOf("") }
    var password by remember { mutableStateOf("") }
    var estaMarcado by remember { mutableStateOf(false) }

    Column(modifier = Modifier.padding(16.dp)) {
        
        OutlinedTextField(
            value = email,
            onValueChange = { email = it },
            label = { Text("Correo Electrónico") },
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
                checked = estaMarcado,
                onCheckedChange = { estaMarcado = it }
            )
            Text("Acepto recibir noticias")
        }

        Button(onClick = { /* Lógica de envío */ }) {
            Text("Entrar")
        }
    }
}
```

---

> [!IMPORTANT]
> **No olvides el `by`:** Usamos la palabra clave `by` para poder leer y escribir la variable directamente (como si fuera una variable normal), pero por detrás es un objeto de estado de Compose.

---

*Anterior: [← Componentes de Formulario](3-componentes-formulario.md)*

_Siguiente: [Listas (LazyColumn) →](../2.%20listas/README.md)_
