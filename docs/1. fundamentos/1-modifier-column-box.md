# 1. Fundamentos de Jetpack Compose

> Referencia rápida para quien empieza desde cero con Jetpack Compose.

---

## ¿Qué es un Composable?

Un **Composable** es una función anotada con `@Composable` que describe una pieza de la interfaz de usuario. En lugar de crear vistas con XML, describes cómo debería verse la pantalla con funciones de Kotlin.

```kotlin
@Composable
fun Saludo() {
    Text(text = "¡Hola, mundo!")
}
```

---

## Modifier

`Modifier` es una herramienta que permite **personalizar y decorar** cualquier composable: cambiar su tamaño, agregar padding, color de fondo, clicks, etc.

Se encadena como una lista de instrucciones que se aplican en orden.

### 📌 Reglas de Oro

- **Es una lista ordenada:** El orden de las funciones SÍ altera el resultado (ej. poner `padding` antes o después de `background`).
- **Es opcional pero vital:** Casi todos los Composables aceptan un `modifier` como parámetro.
- **No contiene lógica:** Solo define apariencia y respuesta física (clics, gestos).

### ✅ Lo que SÍ hace:

- **Tamaños:** `size(40.dp)`, `fillMaxWidth()`, `height(100.dp)`.
- **Estética:** `background(Color.Red)`, `clip(RoundedCornerShape(8.dp))`.
- **Espaciado:** `padding(16.dp)`.
- **Interacción:** `clickable { /* acción */ }`.

### Ejemplos básicos

**Agregar tamaño y color de fondo:**

```kotlin
@Composable
fun CajaRoja() {
    Box(
        modifier = Modifier
            .size(100.dp)
            .background(Color.Red)
    )
}
```

**Agregar padding:**

```kotlin
@Composable
fun TextoConPadding() {
    Text(
        text = "Con espacio",
        modifier = Modifier.padding(16.dp)
    )
}
```

**Hacer que ocupe todo el ancho disponible:**

```kotlin
@Composable
fun TextoCompleto() {
    Text(
        text = "Ancho completo",
        modifier = Modifier.fillMaxWidth()
    )
}
```

**Detectar clicks:**

```kotlin
@Composable
fun TextoClickeable() {
    Text(
        text = "Tócame",
        modifier = Modifier.clickable {
            println("¡Me tocaron!")
        }
    )
}
```

### Modificadores más usados

| Modifier            | ¿Para qué sirve?      |
| ------------------- | --------------------- |
| `padding(dp)`       | Espacio interior      |
| `size(dp)`          | Ancho y alto fijo     |
| `fillMaxWidth()`    | Ocupa todo el ancho   |
| `fillMaxHeight()`   | Ocupa todo el alto    |
| `fillMaxSize()`     | Ocupa todo el espacio |
| `background(color)` | Color de fondo        |
| `clickable { }`     | Detectar toque        |
| `border(...)`       | Agregar borde         |
| `clip(shape)`       | Recortar con forma    |

> **Nota:** El orden importa. `padding` antes de `background` se comporta diferente que al revés.

---

## Column

`Column` organiza sus hijos **de forma vertical**, uno debajo del otro, igual que un `LinearLayout` vertical en XML.

### Ejemplo básico

```kotlin
@Composable
fun MiColumna() {
    Column {
        Text(text = "Primero")
        Text(text = "Segundo")
        Text(text = "Tercero")
    }
}
```

### Con alineación y espaciado

```kotlin
@Composable
fun ColumnaOrdenada() {
    Column(
        modifier = Modifier
            .fillMaxWidth()
            .padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(8.dp),
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text(text = "Elemento 1")
        Text(text = "Elemento 2")
        Text(text = "Elemento 3")
    }
}
```

### Parámetros clave de Column

| Parámetro             | Descripción                             |
| --------------------- | --------------------------------------- |
| `verticalArrangement` | Cómo distribuir los hijos verticalmente |
| `horizontalAlignment` | Cómo alinear los hijos horizontalmente  |

**Valores comunes de `verticalArrangement`:**

- `Arrangement.Top` — desde arriba (por defecto)
- `Arrangement.Bottom` — desde abajo
- `Arrangement.Center` — al centro
- `Arrangement.SpaceBetween` — espacio entre elementos
- `Arrangement.spacedBy(8.dp)` — espacio fijo entre elementos

**Valores comunes de `horizontalAlignment`:**

- `Alignment.Start` — a la izquierda
- `Alignment.CenterHorizontally` — centrado
- `Alignment.End` — a la derecha

---

## Box

`Box` apila sus hijos **uno encima del otro** (como un `FrameLayout`). Es útil para superponer elementos o posicionar algo dentro de un contenedor.

### Ejemplo básico

```kotlin
@Composable
fun MiBox() {
    Box(
        modifier = Modifier
            .size(150.dp)
            .background(Color.Blue)
    ) {
        Text(
            text = "Encima",
            color = Color.White
        )
    }
}
```

### Superponer elementos

```kotlin
@Composable
fun BoxSuperposicion() {
    Box(
        modifier = Modifier.size(200.dp)
    ) {
        // Fondo
        Box(
            modifier = Modifier
                .fillMaxSize()
                .background(Color.LightGray)
        )

        // Texto encima, centrado
        Text(
            text = "Centro",
            modifier = Modifier.align(Alignment.Center)
        )

        // Texto en la esquina
        Text(
            text = "Esquina",
            modifier = Modifier.align(Alignment.BottomEnd)
        )
    }
}
```

### `align()` dentro de Box

Dentro de un `Box`, puedes usar `Modifier.align(...)` en cada hijo para posicionarlo:

| Alineación               | Posición              |
| ------------------------ | --------------------- |
| `Alignment.TopStart`     | Arriba a la izquierda |
| `Alignment.TopCenter`    | Arriba al centro      |
| `Alignment.TopEnd`       | Arriba a la derecha   |
| `Alignment.CenterStart`  | Centro izquierda      |
| `Alignment.Center`       | Centro                |
| `Alignment.CenterEnd`    | Centro derecha        |
| `Alignment.BottomStart`  | Abajo a la izquierda  |
| `Alignment.BottomCenter` | Abajo al centro       |
| `Alignment.BottomEnd`    | Abajo a la derecha    |

---

## Comparación rápida

| Composable | Organización          |
| ---------- | --------------------- |
| `Column`   | Vertical (↓)          |
| `Row`      | Horizontal (→)        |
| `Box`      | Apilado / superpuesto |

---

## Ejemplo combinado

```kotlin
@Composable
fun TarjetaUsuario() {
    Box(
        modifier = Modifier
            .fillMaxWidth()
            .background(Color.White)
            .padding(16.dp)
    ) {
        Column(
            verticalArrangement = Arrangement.spacedBy(4.dp)
        ) {
            Text(text = "Juan Pérez", style = MaterialTheme.typography.titleMedium)
            Text(text = "juan@email.com", style = MaterialTheme.typography.bodySmall)
        }

        Text(
            text = "Admin",
            modifier = Modifier.align(Alignment.TopEnd),
            color = Color.Gray
        )
    }
}
```

---

_Siguiente: [Row y LazyColumn →](../2.%20listas/README.md)_
