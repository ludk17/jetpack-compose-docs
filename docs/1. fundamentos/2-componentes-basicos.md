# Componentes Básicos de Jetpack Compose

> Referencia rápida de los composables más usados en el día a día.

---

## Text

Muestra texto en pantalla. Es el equivalente a `TextView` en XML.

### Ejemplo básico

```kotlin
@Composable
fun MiTexto() {
    Text(text = "Hola, mundo")
}
```

### Con estilo

```kotlin
@Composable
fun TextoEstilizado() {
    Text(
        text = "Título principal",
        fontSize = 24.sp,
        fontWeight = FontWeight.Bold,
        color = Color.DarkGray,
        textAlign = TextAlign.Center,
        modifier = Modifier.fillMaxWidth()
    )
}
```

### Estilos de tipografía predefinidos

En lugar de definir tamaño manualmente, usa el sistema de tipografía de Material:

```kotlin
Text(text = "Grande", style = MaterialTheme.typography.headlineMedium)
Text(text = "Normal", style = MaterialTheme.typography.bodyLarge)
Text(text = "Pequeño", style = MaterialTheme.typography.labelSmall)
```

### Parámetros más usados de Text

| Parámetro | Descripción |
|---|---|
| `text` | Contenido del texto |
| `fontSize` | Tamaño en `sp` |
| `fontWeight` | Grosor (Bold, Normal, Light…) |
| `color` | Color del texto |
| `textAlign` | Alineación horizontal |
| `maxLines` | Número máximo de líneas |
| `overflow` | Qué hacer si el texto no cabe (`Ellipsis`, `Clip`) |
| `style` | Estilo tipográfico de Material |

---

## Image

Muestra una imagen. Requiere importar la librería **Coil** para cargar imágenes desde URL.

### Imagen desde recursos locales

```kotlin
@Composable
fun ImagenLocal() {
    Image(
        painter = painterResource(id = R.drawable.foto),
        contentDescription = "Foto de perfil",
        modifier = Modifier.size(100.dp)
    )
}
```

### Imagen desde URL (con Coil)

Primero agrega la dependencia en `build.gradle`:
```
implementation("io.coil-kt:coil-compose:2.6.0")
```

```kotlin
@Composable
fun ImagenRemota() {
    AsyncImage(
        model = "https://picsum.photos/200",
        contentDescription = "Imagen de internet",
        modifier = Modifier
            .size(120.dp)
            .clip(CircleShape)
    )
}
```

### Parámetros más usados de Image

| Parámetro | Descripción |
|---|---|
| `painter` | Fuente de la imagen (local) |
| `contentDescription` | Descripción para accesibilidad |
| `contentScale` | Cómo recortar/escalar la imagen |
| `modifier` | Tamaño, forma, recorte, etc. |

**Valores de `contentScale`:**

| Valor | Comportamiento |
|---|---|
| `ContentScale.Fit` | Cabe completa sin recortar |
| `ContentScale.Crop` | Llena el espacio (puede recortar) |
| `ContentScale.FillBounds` | Se estira para llenar |

---

## Icon

Muestra un ícono vectorial. Usa los íconos de Material que vienen incluidos.

### Ejemplo básico

```kotlin
@Composable
fun MiIcono() {
    Icon(
        imageVector = Icons.Default.Favorite,
        contentDescription = "Favorito"
    )
}
```

### Con color y tamaño

```kotlin
@Composable
fun IconoPersonalizado() {
    Icon(
        imageVector = Icons.Default.Star,
        contentDescription = "Estrella",
        tint = Color(0xFFFFC107),
        modifier = Modifier.size(32.dp)
    )
}
```

### Grupos de íconos disponibles

| Grupo | Estilo |
|---|---|
| `Icons.Default` | Filled (relleno) |
| `Icons.Outlined` | Solo contorno |
| `Icons.Rounded` | Esquinas redondeadas |
| `Icons.Sharp` | Esquinas rectas |

> Puedes buscar íconos disponibles en: [fonts.google.com/icons](https://fonts.google.com/icons)

---

## Button

Botón interactivo. Al tocarlo ejecuta una acción.

### Ejemplo básico

```kotlin
@Composable
fun MiBoton() {
    Button(onClick = { println("¡Botón presionado!") }) {
        Text(text = "Presióname")
    }
}
```

### Con ícono adentro

```kotlin
@Composable
fun BotonConIcono() {
    Button(onClick = { /* acción */ }) {
        Icon(
            imageVector = Icons.Default.Send,
            contentDescription = null,
            modifier = Modifier.size(18.dp)
        )
        Spacer(modifier = Modifier.width(8.dp))
        Text(text = "Enviar")
    }
}
```

### Variantes de botón

| Composable | Apariencia |
|---|---|
| `Button` | Fondo sólido (primario) |
| `OutlinedButton` | Solo borde, sin fondo |
| `TextButton` | Sin borde ni fondo |
| `ElevatedButton` | Con sombra |
| `FilledTonalButton` | Tono secundario |

```kotlin
@Composable
fun VariantesBoton() {
    Column(verticalArrangement = Arrangement.spacedBy(8.dp)) {
        Button(onClick = {}) { Text("Filled") }
        OutlinedButton(onClick = {}) { Text("Outlined") }
        TextButton(onClick = {}) { Text("Text") }
    }
}
```

---

## Scaffold

`Scaffold` es el **esqueleto de una pantalla**. Proporciona la estructura estándar de Material Design: barra superior, barra inferior, botón flotante y contenido principal.

### Ejemplo básico

```kotlin
@Composable
fun PantallaConScaffold() {
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Mi App") }
            )
        }
    ) { innerPadding ->
        // El contenido va aquí
        // innerPadding evita que el contenido quede tapado por la barra
        Column(modifier = Modifier.padding(innerPadding)) {
            Text("Contenido de la pantalla")
        }
    }
}
```

### Con barra inferior y FAB

```kotlin
@Composable
fun PantallaCompleta() {
    Scaffold(
        topBar = {
            TopAppBar(title = { Text("Inicio") })
        },
        bottomBar = {
            BottomAppBar {
                Text("Barra inferior", modifier = Modifier.padding(16.dp))
            }
        },
        floatingActionButton = {
            FloatingActionButton(onClick = { /* acción */ }) {
                Icon(Icons.Default.Add, contentDescription = "Agregar")
            }
        }
    ) { innerPadding ->
        Box(modifier = Modifier.padding(innerPadding)) {
            Text("Contenido")
        }
    }
}
```

### Slots de Scaffold

| Parámetro | Descripción |
|---|---|
| `topBar` | Barra superior |
| `bottomBar` | Barra inferior |
| `floatingActionButton` | Botón flotante (FAB) |
| `snackbarHost` | Área para mostrar snackbars |
| `content` | Contenido principal (recibe `innerPadding`) |

> **Importante:** Siempre aplica `innerPadding` al contenido principal para evitar que quede oculto detrás de la `topBar` o `bottomBar`.

---

## LazyColumn

`LazyColumn` es como un `RecyclerView`: muestra una **lista larga de elementos** de forma eficiente, renderizando solo los elementos visibles en pantalla.

### Ejemplo básico

```kotlin
@Composable
fun MiLista() {
    val items = listOf("Manzana", "Banana", "Cereza", "Durazno", "Uva")

    LazyColumn {
        items(items) { fruta ->
            Text(
                text = fruta,
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(16.dp)
            )
        }
    }
}
```

### Con separadores

```kotlin
@Composable
fun ListaConDivisores() {
    val items = listOf("Item 1", "Item 2", "Item 3")

    LazyColumn {
        items(items) { item ->
            Text(
                text = item,
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(horizontal = 16.dp, vertical = 12.dp)
            )
            HorizontalDivider()
        }
    }
}
```

### Con header y footer

```kotlin
@Composable
fun ListaConHeader() {
    LazyColumn {
        item {
            Text(
                text = "Encabezado",
                fontWeight = FontWeight.Bold,
                modifier = Modifier.padding(16.dp)
            )
        }

        items(listOf("A", "B", "C")) { letra ->
            Text(text = letra, modifier = Modifier.padding(16.dp))
        }

        item {
            Text(
                text = "Pie de lista",
                color = Color.Gray,
                modifier = Modifier.padding(16.dp)
            )
        }
    }
}
```

### LazyColumn vs Column

| | `Column` | `LazyColumn` |
|---|---|---|
| ¿Cuándo usarlo? | Pocos elementos fijos | Listas largas o dinámicas |
| Rendimiento | Renderiza todo | Solo renderiza lo visible |
| Scroll automático | ✗ (necesita `verticalScroll`) | ✓ |

---

## Ejemplo combinado

Una pantalla típica usando todos los componentes:

```kotlin
@Composable
fun PantallaInicio() {
    val frutas = listOf("Manzana", "Banana", "Cereza", "Durazno", "Uva")

    Scaffold(
        topBar = {
            TopAppBar(title = { Text("Frutas") })
        },
        floatingActionButton = {
            FloatingActionButton(onClick = {}) {
                Icon(Icons.Default.Add, contentDescription = "Agregar")
            }
        }
    ) { innerPadding ->
        LazyColumn(
            modifier = Modifier
                .fillMaxSize()
                .padding(innerPadding)
        ) {
            items(frutas) { fruta ->
                Row(
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(16.dp),
                    verticalAlignment = Alignment.CenterVertically
                ) {
                    Icon(
                        imageVector = Icons.Default.Star,
                        contentDescription = null,
                        tint = Color(0xFFFFC107)
                    )
                    Spacer(modifier = Modifier.width(12.dp))
                    Text(text = fruta, style = MaterialTheme.typography.bodyLarge)
                }
                HorizontalDivider()
            }
        }
    }
}
```

---

*Anterior: [← Modifier, Column y Box](modifier-column-box.md)*
