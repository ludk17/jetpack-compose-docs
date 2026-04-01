# Componentes de Formulario

En Jetpack Compose, los componentes de formulario son las herramientas que permiten al usuario interactuar y enviar datos. Aquí tienes los más comunes explicados de forma sencilla.

---

## 1. Entradas de Texto (TextField)

Existen dos estilos principales para escribir texto: el estándar y el que tiene un borde delineado.

### TextField
Es el cuadro de texto clásico con una línea en la parte inferior.
```kotlin
TextField(
    value = "Texto escrito",
    onValueChange = { /* Acción al escribir */ },
    label = { Text("Nombre") }
)
```

### OutlinedTextField
Tiene un borde completo alrededor. Es muy común en diseños modernos.
```kotlin
OutlinedTextField(
    value = "Texto escrito",
    onValueChange = { /* Acción al escribir */ },
    label = { Text("Correo electrónico") }
)
```

---

## 2. Botones (Button)

El componente principal para ejecutar acciones.

```kotlin
Button(onClick = { /* Acción al hacer clic */ }) {
    Text("Enviar Formulario")
}
```

---

## 3. Casilla de Selección (Checkbox)

Se usa para opciones que se pueden marcar o desmarcar (ejemplo: "Acepto los términos").

```kotlin
Checkbox(
    checked = true, 
    onCheckedChange = { /* Acción al cambiar */ }
)
```

---

## 4. Interruptor (Switch)

Ideal para activar o desactivar ajustes (ejemplo: "Modo Oscuro").

```kotlin
Switch(
    checked = false, 
    onCheckedChange = { /* Acción al cambiar */ }
)
```

---

## 5. Botón de Opción (RadioButton)

Se usa cuando el usuario debe elegir solo una opción de un grupo.

```kotlin
RadioButton(
    selected = true, 
    onClick = { /* Acción al seleccionar */ }
)
```

---

> [!TIP]
> **Diferencia Visual:**
> - `TextField`: Estilo más minimalista, solo con una línea base.
> - `OutlinedTextField`: Estilo más robusto con un borde definido, ideal para formularios con muchos campos.

---

## Ejemplo: Formulario Completo

Así es como se verían todos los componentes juntos en una pantalla de registro sencilla:

```kotlin
@Composable
fun FormularioRegistro() {
    Column(
        modifier = Modifier
            .fillMaxWidth()
            .padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        Text(
            text = "Crear Cuenta", 
            style = MaterialTheme.typography.headlineMedium 
        )

        OutlinedTextField(
            value = "", 
            onValueChange = { }, 
            label = { Text("Nombre") },
            modifier = Modifier.fillMaxWidth()
        )

        OutlinedTextField(
            value = "", 
            onValueChange = { }, 
            label = { Text("Email") },
            modifier = Modifier.fillMaxWidth()
        )

        Row(verticalAlignment = Alignment.CenterVertically) {
            Checkbox(checked = false, onCheckedChange = { })
            Text("Acepto los términos")
        }

        Button(
            onClick = { /* Registrar */ },
            modifier = Modifier.fillMaxWidth()
        ) {
            Text("Enviar")
        }
    }
}
```

---

*Anterior: [← Componentes Básicos](2-componentes-basicos.md)*

_Siguiente: [Estado y Remember →](4-estados-remember.md)_

