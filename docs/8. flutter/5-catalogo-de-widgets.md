# 5. Catálogo de Widgets

> Tu **diccionario de widgets** 📖. No necesitas leerlo de corrido: búscalo aquí cuando no recuerdes qué hace un widget o qué propiedades tiene.

---

## 🔎 Índice rápido: "Quiero..."

| Quiero... | Widget | Ir a |
|---|---|---|
| Crear la estructura de la pantalla | `Scaffold` | [Scaffold](#scaffold) |
| Poner una barra superior con título | `AppBar` | [AppBar](#appbar) |
| Evitar que el contenido quede bajo la barra de estado | `SafeArea` | [SafeArea](#safearea) |
| Mostrar texto | `Text` | [Text](#text) |
| Mostrar un ícono | `Icon` | [Icon](#icon) |
| Mostrar una imagen (con o sin bordes redondeados) | `Image`, `ClipRRect` | [Image y ClipRRect](#image-y-cliprrect) |
| Mostrar una foto o iniciales en un círculo | `CircleAvatar` | [CircleAvatar](#circleavatar) |
| Poner widgets uno **debajo** de otro | `Column` | [Column](#column) |
| Poner widgets uno **al lado** de otro | `Row` | [Row](#row) |
| Que un widget ocupe el espacio sobrante | `Expanded` | [Expanded](#expanded) |
| Empujar widgets hacia los extremos | `Spacer` | [Spacer](#spacer) |
| Centrar un widget | `Center` | [Center](#center) |
| Ubicar un widget en una esquina | `Align` | [Align](#align) |
| Poner widgets **encima** de otros | `Stack`, `Positioned` | [Stack y Positioned](#stack-y-positioned) |
| Que los elementos bajen a otra línea si no caben | `Wrap` | [Wrap](#wrap) |
| Agregar espacio alrededor de un widget | `Padding` | [Padding](#padding) |
| Separar dos widgets / dar tamaño fijo | `SizedBox` | [SizedBox](#sizedbox) |
| Darle color, bordes o tamaño a una caja | `Container` | [Container](#container) |
| Dibujar una línea separadora | `Divider` | [Divider](#divider) |
| Agrupar contenido en una tarjeta | `Card` | [Card](#card) |
| Mostrar una etiqueta pequeña ("Wi-Fi", "Nuevo") | `Chip` | [Chip](#chip) |
| Una fila con ícono, título y subtítulo | `ListTile` | [ListTile](#listtile) |
| Que la pantalla tenga scroll | `SingleChildScrollView` | [SingleChildScrollView](#singlechildscrollview) |
| Mostrar una lista con scroll | `ListView` | [ListView](#listview) |
| Un botón | `ElevatedButton`, `FilledButton`, `OutlinedButton`, `TextButton`, `IconButton` | [Botones](#botones) |
| Un botón flotante | `FloatingActionButton` | [FloatingActionButton](#floatingactionbutton) |

Y las **clases de apoyo** que usarás dentro de los widgets:

| Clase | Para qué | Ir a |
|---|---|---|
| `TextStyle` | Tamaño, color y grosor del texto | [TextStyle](#textstyle) |
| `EdgeInsets` | Medidas de padding y margin | [EdgeInsets](#edgeinsets) |
| `Colors` | Paleta de colores | [Colors](#colors) |
| `BoxDecoration` | Fondo, bordes y sombras de un `Container` | [BoxDecoration](#boxdecoration) |
| `BorderRadius` | Esquinas redondeadas | [BorderRadius](#borderradius) |
| `MainAxisAlignment` / `CrossAxisAlignment` | Alinear hijos de `Column` y `Row` | [MainAxisAlignment y CrossAxisAlignment](#mainaxisalignment-y-crossaxisalignment) |
| `Alignment` | Posición dentro de una caja (esquinas, centro) | [Alignment](#alignment) |
| `BoxFit` | Cómo se ajusta una imagen a su espacio | [BoxFit](#boxfit) |

---

## 🧠 Antes de empezar: cómo leer un widget

Todos los widgets se crean igual: **nombre del widget + paréntesis + parámetros con nombre**.

```dart
Text(
  'Hola',                          // 👈 parámetro posicional (sin nombre)
  style: TextStyle(fontSize: 20),  // 👈 parámetro con nombre
  textAlign: TextAlign.center,     // 👈 parámetro con nombre
)
```

Esto ya lo viste en [Parámetros Nombrados](../7.%20dart/3-parametros-nombrados.md). En Flutter casi todo es un parámetro con nombre.

### `child` vs `children`

Piensa en una **caja** 📦 y un **estante** 📚:

| Parámetro | Recibe | Widgets que lo usan |
|---|---|---|
| `child:` | **Un solo** widget (la caja guarda una cosa) | `Padding`, `Center`, `Container`, `SizedBox`, `Card`, `Expanded`, `Align` |
| `children:` | **Una lista** de widgets `[ ]` (el estante guarda varias) | `Column`, `Row`, `Stack`, `Wrap`, `ListView` |

```dart
Padding(
  padding: const EdgeInsets.all(16),
  child: Column(            // 👈 Padding tiene UN hijo: el Column
    children: [             // 👈 Column tiene VARIOS hijos
      Text('Uno'),
      Text('Dos'),
    ],
  ),
)
```

> [!TIP]
> ¿Necesitas meter varios widgets donde solo cabe un `child`? Agrúpalos en un `Column` o un `Row`.

### Constructores con nombre (los "métodos" que más vas a usar)

Algunos widgets y clases tienen **varias formas de crearse**, separadas por un punto. Son como distintos "modos" del mismo widget:

| Forma | Qué hace |
|---|---|
| `Image.network(url)` | Imagen desde internet |
| `Image.asset(ruta)` | Imagen guardada en el proyecto |
| `ListView.builder(...)` | Lista que construye sus elementos a demanda |
| `ElevatedButton.icon(...)` | Botón con ícono **y** texto (también existe en `FilledButton`, `OutlinedButton` y `TextButton`) |
| `FloatingActionButton.extended(...)` | Botón flotante con ícono y texto |
| `EdgeInsets.all(16)` | Mismo espacio en los 4 lados |
| `EdgeInsets.symmetric(horizontal: 16, vertical: 8)` | Espacio horizontal y vertical distinto |
| `EdgeInsets.only(top: 8, left: 4)` | Espacio solo en los lados que indiques |
| `BorderRadius.circular(12)` | Todas las esquinas redondeadas igual |
| `SizedBox.square(dimension: 50)` | Caja cuadrada de 50 x 50 |

### ¿Dónde pongo `const`?

Si un widget y todo lo que tiene dentro **nunca cambia**, puede ser `const`. El editor te lo sugiere con una línea azul: acepta la sugerencia.

```dart
const Text('Siempre igual')            // ✅ puede ser const
Text('Hola $nombre')                   // ❌ depende de una variable: no puede ser const
ElevatedButton(onPressed: () {}, ...)  // ❌ tiene una función: no puede ser const
```

> [!TIP]
> **Busca sin salir del editor:**
> - `Ctrl + Espacio` dentro de los paréntesis de un widget → muestra **todas** sus propiedades.
> - Pasa el mouse sobre un widget → muestra su documentación.
> - `Ctrl + clic` (o `Cmd + clic` en Mac) → abre el código fuente del widget.
> - La documentación oficial de cada widget está en [api.flutter.dev](https://api.flutter.dev).

---

## 🏗️ Estructura de la pantalla

### `Scaffold`

El **esqueleto** de una pantalla: tiene "huecos" predefinidos para la barra superior, el contenido y los botones. Lo viste a fondo en [Scaffold](2-scaffold.md).

| Propiedad | Recibe | Para qué |
|---|---|---|
| `appBar` | `AppBar` | Barra superior |
| `body` | Un widget | Contenido principal |
| `floatingActionButton` | `FloatingActionButton` | Botón flotante |
| `backgroundColor` | `Color` | Color de fondo de la pantalla |

```dart
Scaffold(
  appBar: AppBar(title: const Text('Inicio')),
  body: const Center(child: Text('Contenido')),
)
```

### `AppBar`

La **barra superior** de la pantalla.

| Propiedad | Recibe | Para qué |
|---|---|---|
| `title` | Widget (casi siempre `Text`) | Título |
| `leading` | Widget | Lo que va a la **izquierda** del título (ícono de menú o de volver) |
| `actions` | Lista de widgets `[ ]` | Íconos a la **derecha** |
| `centerTitle` | `bool` | Centrar el título |
| `backgroundColor` | `Color` | Color de fondo |
| `foregroundColor` | `Color` | Color del título y los íconos |

```dart
AppBar(
  leading: IconButton(onPressed: () {}, icon: const Icon(Icons.menu)),
  title: const Text('Mis cursos'),
  centerTitle: true,
  backgroundColor: Colors.indigo,
  foregroundColor: Colors.white,
  actions: [
    IconButton(onPressed: () {}, icon: const Icon(Icons.search)),
    IconButton(onPressed: () {}, icon: const Icon(Icons.more_vert)),
  ],
)
```

### `SafeArea`

Si tu pantalla **no** tiene `AppBar`, el contenido puede quedar debajo de la barra de estado (hora, batería) o del "notch". `SafeArea` lo empuja a la zona visible.

```dart
Scaffold(
  body: SafeArea(
    child: Text('No quedo tapado por la barra de estado'),
  ),
)
```

---

## 📝 Mostrar contenido

### `Text`

Muestra un texto.

| Propiedad | Recibe | Para qué |
|---|---|---|
| *(primer parámetro)* | `String` | El texto a mostrar |
| `style` | `TextStyle` | Tamaño, color, grosor → ver [TextStyle](#textstyle) |
| `textAlign` | `TextAlign.left` / `.center` / `.right` / `.justify` | Alineación del texto |
| `maxLines` | `int` | Máximo de líneas |
| `overflow` | `TextOverflow.ellipsis` | Qué hacer si no cabe (`...`) |

```dart
const Text(
  'Este es un texto muy largo que no cabe en una sola línea de la pantalla',
  maxLines: 1,
  overflow: TextOverflow.ellipsis, // muestra "Este es un texto muy largo que..."
  style: TextStyle(fontSize: 16, color: Colors.grey),
)
```

### `Icon`

Muestra un ícono de Material Design.

| Propiedad | Recibe | Para qué |
|---|---|---|
| *(primer parámetro)* | `Icons.nombre` | Qué ícono mostrar |
| `size` | `double` | Tamaño (por defecto 24) |
| `color` | `Color` | Color |

```dart
const Icon(Icons.star, color: Colors.amber, size: 32)
```

> [!TIP]
> Busca íconos en [fonts.google.com/icons](https://fonts.google.com/icons). El nombre se escribe en minúsculas con guiones bajos: *"Shopping Cart"* → `Icons.shopping_cart`.

### `Image` y `ClipRRect`

Muestra una imagen. Casi siempre la crearás con un **constructor con nombre**:

| Forma | De dónde sale la imagen |
|---|---|
| `Image.network('https://...')` | De internet |
| `Image.asset('assets/foto.png')` | De la carpeta del proyecto (hay que declararla en `pubspec.yaml`) |

| Propiedad | Recibe | Para qué |
|---|---|---|
| `width` / `height` | `double` | Tamaño. `double.infinity` = todo el ancho disponible |
| `fit` | `BoxFit` | Cómo se ajusta al espacio → ver [BoxFit](#boxfit) |

```dart
Image.network(
  'https://picsum.photos/600/300',
  width: double.infinity,
  height: 200,
  fit: BoxFit.cover,
)
```

Para **redondear las esquinas** de una imagen, envuélvela en `ClipRRect` (*"recorta con rectángulo redondeado"*), como si la pasaras por un molde con esquinas curvas ✂️:

```dart
ClipRRect(
  borderRadius: BorderRadius.circular(16),
  child: Image.network('https://picsum.photos/300', width: 150),
)
```

### `CircleAvatar`

Un **círculo** para fotos de perfil o iniciales.

| Propiedad | Recibe | Para qué |
|---|---|---|
| `radius` | `double` | Tamaño (radio del círculo) |
| `backgroundColor` | `Color` | Color de fondo |
| `backgroundImage` | `NetworkImage(url)` o `AssetImage(ruta)` | Foto dentro del círculo |
| `child` | Widget | Contenido (iniciales o ícono) |

```dart
Row(
  children: [
    const CircleAvatar(
      radius: 30,
      backgroundColor: Colors.indigo,
      child: Text('AT', style: TextStyle(color: Colors.white)),
    ),
    const SizedBox(width: 12),
    CircleAvatar(
      radius: 30,
      backgroundImage: NetworkImage('https://i.pravatar.cc/150'),
    ),
  ],
)
```

---

## 📐 Organizar (layout)

### `Column`

Pone sus hijos **uno debajo del otro** ↕️, como los pisos de un edificio 🏢.

| Propiedad | Recibe | Para qué |
|---|---|---|
| `children` | Lista de widgets | Los hijos |
| `mainAxisAlignment` | `MainAxisAlignment` | Alineación **vertical** → ver [tabla](#mainaxisalignment-y-crossaxisalignment) |
| `crossAxisAlignment` | `CrossAxisAlignment` | Alineación **horizontal** |
| `mainAxisSize` | `MainAxisSize.min` / `.max` | Ocupar solo lo necesario (`min`) o todo el alto (`max`, por defecto) |

```dart
const Column(
  crossAxisAlignment: CrossAxisAlignment.start, // todos alineados a la izquierda
  children: [
    Text('Ana Torres', style: TextStyle(fontWeight: FontWeight.bold)),
    Text('Ingeniería de Sistemas'),
    Text('Ciclo 6'),
  ],
)
```

### `Row`

Pone sus hijos **uno al lado del otro** ↔️, como los asientos de una fila del cine 🎬. Tiene las **mismas propiedades** que `Column`, pero los ejes se invierten.

```dart
const Row(
  mainAxisAlignment: MainAxisAlignment.spaceEvenly, // reparte el espacio
  children: [
    Icon(Icons.home),
    Icon(Icons.search),
    Icon(Icons.person),
  ],
)
```

> [!WARNING]
> Si los hijos de un `Row` son más anchos que la pantalla, verás una **franja amarilla y negra** (*overflow*). Solución: envuelve el hijo largo en [`Expanded`](#expanded).

### `Expanded`

Hace que un hijo de `Row` o `Column` **ocupe todo el espacio sobrante**. Es como estirar un elástico 🪢 hasta llenar el hueco.

| Propiedad | Recibe | Para qué |
|---|---|---|
| `child` | Widget | El widget que se estira |
| `flex` | `int` | Cuántas "partes" del espacio toma (por defecto 1) |

```dart
Row(
  children: [
    Expanded(
      flex: 2, // toma 2 partes
      child: Container(height: 50, color: Colors.indigo),
    ),
    Expanded(
      flex: 1, // toma 1 parte
      child: Container(height: 50, color: Colors.amber),
    ),
  ],
)
// Resultado: el azul ocupa 2/3 del ancho y el amarillo 1/3
```

Uso muy común: **dos botones del mismo ancho**:

```dart
Row(
  children: [
    Expanded(child: OutlinedButton(onPressed: () {}, child: const Text('Cancelar'))),
    const SizedBox(width: 12),
    Expanded(child: FilledButton(onPressed: () {}, child: const Text('Aceptar'))),
  ],
)
```

### `Spacer`

Un espacio vacío **elástico** que empuja a los demás widgets hacia los extremos.

```dart
const Row(
  children: [
    Text('Total'),
    Spacer(), // 👈 empuja el precio hasta la derecha
    Text('S/ 25.00', style: TextStyle(fontWeight: FontWeight.bold)),
  ],
)
```

### `Center`

Centra a su hijo (horizontal y verticalmente) dentro del espacio disponible.

```dart
const Center(
  child: Text('Estoy al centro'),
)
```

### `Align`

Como `Center`, pero puedes elegir **dónde** ubicar al hijo usando [`Alignment`](#alignment).

```dart
const Align(
  alignment: Alignment.topRight,
  child: Icon(Icons.close),
)
```

### `Stack` y `Positioned`

Pone sus hijos **uno encima de otro** 🥞, como hojas transparentes apiladas. El **primero** de la lista queda **al fondo**; el último, **adelante**.

`Positioned` (solo dentro de un `Stack`) ubica un hijo a cierta distancia de los bordes.

| `Positioned` | Recibe | Para qué |
|---|---|---|
| `top`, `bottom`, `left`, `right` | `double` | Distancia desde cada borde del `Stack` |
| `child` | Widget | El widget a ubicar |

```dart
Stack(
  children: [
    // 1. Al fondo: la imagen
    Image.network('https://picsum.photos/400/200'),
    // 2. Encima: una etiqueta en la esquina superior izquierda
    Positioned(
      top: 8,
      left: 8,
      child: Container(
        padding: const EdgeInsets.all(6),
        color: Colors.red,
        child: const Text('NUEVO', style: TextStyle(color: Colors.white)),
      ),
    ),
  ],
)
```

### `Wrap`

Como un `Row`, pero cuando los hijos **no caben, bajan a la siguiente línea** ↩️ (como las palabras de un párrafo). Ideal para etiquetas o [`Chip`](#chip).

| Propiedad | Recibe | Para qué |
|---|---|---|
| `children` | Lista de widgets | Los hijos |
| `spacing` | `double` | Espacio **horizontal** entre hijos |
| `runSpacing` | `double` | Espacio **vertical** entre líneas |

```dart
const Wrap(
  spacing: 8,
  runSpacing: 8,
  children: [
    Chip(label: Text('Kotlin')),
    Chip(label: Text('Compose')),
    Chip(label: Text('Dart')),
    Chip(label: Text('Flutter')),
    Chip(label: Text('Firebase')),
  ],
)
```

---

## 📏 Espaciado y decoración

### `Padding`

Agrega **espacio interno** alrededor de su hijo, como el relleno de un sobre acolchado ✉️. Las medidas se dan con [`EdgeInsets`](#edgeinsets).

```dart
const Padding(
  padding: EdgeInsets.symmetric(horizontal: 16, vertical: 8),
  child: Text('Con espacio alrededor'),
)
```

### `SizedBox`

Una caja de **tamaño fijo**. Tiene dos usos:

1. **Separador** vacío entre widgets (el uso más común).
2. **Forzar un tamaño** a su hijo.

```dart
Column(
  children: [
    const Text('Título'),
    const SizedBox(height: 16), // 👈 separador vertical
    SizedBox(
      width: double.infinity,   // 👈 el botón ocupa todo el ancho
      child: ElevatedButton(onPressed: () {}, child: const Text('Continuar')),
    ),
  ],
)
```

> [!TIP]
> En un `Column` separa con `SizedBox(height: ...)`. En un `Row` separa con `SizedBox(width: ...)`.

### `Container`

La **navaja suiza** 🔧: tamaño, color, márgenes, padding y bordes en un solo widget.

| Propiedad | Recibe | Para qué |
|---|---|---|
| `width` / `height` | `double` | Tamaño |
| `padding` | `EdgeInsets` | Espacio **dentro** de la caja |
| `margin` | `EdgeInsets` | Espacio **fuera** de la caja |
| `color` | `Color` | Color de fondo (solo si **no** usas `decoration`) |
| `decoration` | `BoxDecoration` | Color, bordes redondeados, sombra, borde → ver [BoxDecoration](#boxdecoration) |
| `alignment` | `Alignment` | Dónde ubicar al hijo dentro de la caja |
| `child` | Widget | Contenido |

```dart
Container(
  width: double.infinity,
  margin: const EdgeInsets.only(bottom: 12),
  padding: const EdgeInsets.all(16),
  decoration: BoxDecoration(
    color: Colors.indigo.shade50,
    borderRadius: BorderRadius.circular(12),
  ),
  child: const Text('Soy una caja decorada'),
)
```

**Padding vs margin** 🖼️: piensa en un cuadro con marco. El `padding` es el espacio entre el marco y la foto; el `margin` es el espacio entre el marco y la pared.

> [!WARNING]
> No uses `color:` y `decoration:` a la vez en el mismo `Container`: da error. Si usas `decoration`, el color va **dentro** de `BoxDecoration`.

### `Divider`

Una **línea horizontal** para separar secciones.

| Propiedad | Recibe | Para qué |
|---|---|---|
| `height` | `double` | Espacio vertical total que ocupa (la línea queda al medio) |
| `thickness` | `double` | Grosor de la línea |
| `color` | `Color` | Color de la línea |

```dart
const Divider(height: 32, thickness: 1)
```

### `Card`

Una **tarjeta** con esquinas redondeadas y una sombra suave, para agrupar información relacionada (como una ficha 🗂️).

| Propiedad | Recibe | Para qué |
|---|---|---|
| `child` | Widget | Contenido |
| `elevation` | `double` | Qué tan "elevada" se ve (tamaño de la sombra) |
| `color` | `Color` | Color de fondo |
| `margin` | `EdgeInsets` | Espacio alrededor de la tarjeta |

```dart
const Card(
  elevation: 2,
  child: Padding(
    padding: EdgeInsets.all(16),
    child: Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text('Desarrollo Móvil', style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
        SizedBox(height: 4),
        Text('4 créditos · Lunes y miércoles'),
      ],
    ),
  ),
)
```

> **Nota:** `Card` **no** tiene `padding`. Para separar el contenido de los bordes, mete un `Padding` dentro.

### `Chip`

Una **etiqueta** pequeña y redondeada, como las pegatinas de categoría 🏷️.

| Propiedad | Recibe | Para qué |
|---|---|---|
| `label` | Widget (casi siempre `Text`) | El texto |
| `avatar` | Widget (casi siempre `Icon`) | Ícono a la izquierda |
| `backgroundColor` | `Color` | Color de fondo |

```dart
const Chip(
  avatar: Icon(Icons.wifi, size: 18),
  label: Text('Wi-Fi gratis'),
)
```

---

## 📜 Listas y scroll

### `ListTile`

Una **fila prediseñada** con espacio para un ícono, un título, un subtítulo y algo al final. Ahorra mucho trabajo.

```
┌─────────────────────────────────────────┐
│ [leading]  title                [trailing] │
│            subtitle                      │
└─────────────────────────────────────────┘
```

| Propiedad | Recibe | Para qué |
|---|---|---|
| `leading` | Widget | Izquierda (ícono, `CircleAvatar`) |
| `title` | Widget | Texto principal |
| `subtitle` | Widget | Texto secundario (debajo del título) |
| `trailing` | Widget | Derecha (flecha, precio, ícono) |
| `onTap` | Función | Qué pasa al tocar la fila |

```dart
ListTile(
  leading: const Icon(Icons.schedule),
  title: const Text('Horario'),
  subtitle: const Text('Lunes a viernes, 8:00 a. m. - 6:00 p. m.'),
  trailing: const Icon(Icons.chevron_right),
  onTap: () {},
)
```

### `SingleChildScrollView`

Le da **scroll** a un contenido que puede ser más alto que la pantalla. Envuelve tu `Column` principal en él.

| Propiedad | Recibe | Para qué |
|---|---|---|
| `child` | Widget (casi siempre `Column`) | El contenido con scroll |
| `padding` | `EdgeInsets` | Espacio alrededor del contenido |

```dart
Scaffold(
  appBar: AppBar(title: const Text('Detalle')),
  body: SingleChildScrollView(
    padding: const EdgeInsets.all(16),
    child: Column(
      children: [
        // ... muchos widgets: si no caben, el usuario puede hacer scroll
      ],
    ),
  ),
)
```

> [!TIP]
> **¿`SingleChildScrollView` o `ListView`?** Usa `SingleChildScrollView` + `Column` para una pantalla de **contenido variado** (imagen, textos, botones). Usa `ListView` para una **lista de elementos parecidos** (contactos, productos).

### `ListView`

Una **lista con scroll**. Tiene dos formas:

| Forma | Cuándo usarla |
|---|---|
| `ListView(children: [...])` | Pocos elementos, escritos a mano |
| `ListView.builder(itemCount: ..., itemBuilder: ...)` | Muchos elementos o elementos que vienen de una lista de datos |

```dart
final cursos = ['Kotlin', 'Compose', 'Dart', 'Flutter'];

ListView.builder(
  itemCount: cursos.length,                 // cuántos elementos hay
  itemBuilder: (context, index) {           // cómo dibujar el elemento número "index"
    return ListTile(
      leading: CircleAvatar(child: Text('${index + 1}')),
      title: Text(cursos[index]),
    );
  },
)
```

> [!WARNING]
> Un `ListView` dentro de un `Column` da el error *"Vertical viewport was given unbounded height"*. Envuélvelo en `Expanded`.

---

## 👆 Botones

### Botones

Todos los botones tienen dos propiedades clave:

| Propiedad | Recibe | Para qué |
|---|---|---|
| `onPressed` | Función `() {}` | Qué pasa al presionarlo. Si es `null`, el botón se ve **deshabilitado** |
| `child` | Widget (casi siempre `Text`) | Contenido del botón |

| Botón | Aspecto | Úsalo para |
|---|---|---|
| `FilledButton` | Fondo de color sólido | La acción **más importante** de la pantalla |
| `ElevatedButton` | Fondo claro con sombra | Acción principal |
| `OutlinedButton` | Solo borde | Acción secundaria |
| `TextButton` | Solo texto | Acciones menores ("Cancelar", "Ver más") |
| `IconButton` | Solo ícono (usa `icon:` en vez de `child:`) | Barras de herramientas, `AppBar` |

```dart
Column(
  children: [
    FilledButton(onPressed: () {}, child: const Text('Comprar')),
    ElevatedButton(onPressed: () {}, child: const Text('Agregar al carrito')),
    OutlinedButton(onPressed: () {}, child: const Text('Ver detalles')),
    TextButton(onPressed: () {}, child: const Text('Cancelar')),
    IconButton(onPressed: () {}, icon: const Icon(Icons.favorite_border)),
    const ElevatedButton(onPressed: null, child: Text('Deshabilitado')),
  ],
)
```

**Botón con ícono y texto:** usa el constructor `.icon`, que recibe `icon:` y `label:` (en lugar de `child:`).

```dart
FilledButton.icon(
  onPressed: () {},
  icon: const Icon(Icons.phone),
  label: const Text('Llamar'),
)
```

> **Nota:** Por ahora pon `onPressed: () {}` (una función vacía). El botón se verá activo pero no hará nada. Más adelante le darás acciones reales.

### `FloatingActionButton`

El **botón redondo flotante** de la esquina inferior derecha. Se coloca en la propiedad `floatingActionButton` del `Scaffold`, **no** dentro del `body`.

| Forma | Aspecto |
|---|---|
| `FloatingActionButton(child: Icon(...))` | Círculo con un ícono |
| `FloatingActionButton.extended(icon: ..., label: ...)` | Píldora con ícono y texto |

```dart
Scaffold(
  body: const Center(child: Text('Mis notas')),
  floatingActionButton: FloatingActionButton.extended(
    onPressed: () {},
    icon: const Icon(Icons.add),
    label: const Text('Nueva nota'),
  ),
)
```

---

## 🧰 Clases de apoyo

Estas clases **no se ven en pantalla** por sí solas: son los "valores" que les pasas a los widgets.

### `TextStyle`

Define cómo se ve un texto. Se pasa en la propiedad `style` de `Text`.

| Propiedad | Ejemplo | Para qué |
|---|---|---|
| `fontSize` | `18` | Tamaño |
| `fontWeight` | `FontWeight.bold`, `FontWeight.w300` | Grosor (`w100` muy delgado ... `w900` muy grueso) |
| `fontStyle` | `FontStyle.italic` | Cursiva |
| `color` | `Colors.grey` | Color |
| `letterSpacing` | `1.5` | Espacio entre letras |
| `height` | `1.5` | Alto de línea (espacio entre renglones) |
| `decoration` | `TextDecoration.underline`, `TextDecoration.lineThrough` | Subrayado o tachado |

```dart
const Text(
  'S/ 30.00',
  style: TextStyle(
    fontSize: 14,
    color: Colors.grey,
    decoration: TextDecoration.lineThrough, // precio tachado
  ),
)
```

### `EdgeInsets`

Medidas de espacio para `padding` y `margin`.

| Constructor | Resultado |
|---|---|
| `EdgeInsets.all(16)` | 16 en los 4 lados |
| `EdgeInsets.symmetric(horizontal: 16, vertical: 8)` | 16 izquierda/derecha, 8 arriba/abajo |
| `EdgeInsets.only(top: 8, left: 16)` | Solo en los lados indicados (`top`, `bottom`, `left`, `right`) |
| `EdgeInsets.zero` | Sin espacio |

### `Colors`

La paleta de colores de Material.

| Forma | Ejemplo |
|---|---|
| Color base | `Colors.indigo`, `Colors.red`, `Colors.green`, `Colors.amber`, `Colors.brown` |
| Tono más claro u oscuro | `Colors.indigo.shade50` (muy claro) ... `Colors.indigo.shade900` (muy oscuro) |
| Blanco y negro | `Colors.white`, `Colors.black` |
| Negro/blanco semitransparente | `Colors.black54`, `Colors.black26`, `Colors.white70` |
| Transparente | `Colors.transparent` |
| Color exacto (hexadecimal) | `Color(0xFF6200EE)` → `0xFF` + el código `6200EE` |

### `BoxDecoration`

La "pintura" de un `Container`: fondo, esquinas, borde y sombra.

| Propiedad | Ejemplo | Para qué |
|---|---|---|
| `color` | `Colors.white` | Color de fondo |
| `borderRadius` | `BorderRadius.circular(12)` | Esquinas redondeadas |
| `border` | `Border.all(color: Colors.grey, width: 1)` | Borde alrededor |
| `boxShadow` | `[BoxShadow(color: Colors.black26, blurRadius: 8)]` | Sombra |
| `shape` | `BoxShape.circle` | Caja circular |

```dart
Container(
  padding: const EdgeInsets.all(16),
  decoration: BoxDecoration(
    color: Colors.white,
    borderRadius: BorderRadius.circular(12),
    border: Border.all(color: Colors.grey.shade300),
    boxShadow: const [
      BoxShadow(color: Colors.black12, blurRadius: 8, offset: Offset(0, 4)),
    ],
  ),
  child: const Text('Caja con borde y sombra'),
)
```

### `BorderRadius`

Esquinas redondeadas. Se usa en `BoxDecoration`, `ClipRRect` y otros.

| Constructor | Resultado |
|---|---|
| `BorderRadius.circular(12)` | Las 4 esquinas con radio 12 |
| `BorderRadius.only(topLeft: Radius.circular(12), topRight: Radius.circular(12))` | Solo las esquinas indicadas |

### `MainAxisAlignment` y `CrossAxisAlignment`

Alinean a los hijos de `Column` y `Row`. El **eje principal** es la dirección en la que se acomodan los hijos; el **eje cruzado** es el perpendicular.

| Widget | `mainAxisAlignment` | `crossAxisAlignment` |
|---|---|---|
| `Column` | Vertical ↕️ | Horizontal ↔️ |
| `Row` | Horizontal ↔️ | Vertical ↕️ |

| `MainAxisAlignment.` | Resultado (con 3 hijos en un `Row`) |
|---|---|
| `start` | `[A][B][C]            ` |
| `center` | `      [A][B][C]      ` |
| `end` | `            [A][B][C]` |
| `spaceBetween` | `[A]       [B]       [C]` |
| `spaceAround` | `  [A]    [B]    [C]  ` |
| `spaceEvenly` | `   [A]   [B]   [C]   ` |

| `CrossAxisAlignment.` | Resultado |
|---|---|
| `start` | Al inicio (en `Column`: a la izquierda) |
| `center` | Al centro (por defecto) |
| `end` | Al final (en `Column`: a la derecha) |
| `stretch` | Estira a los hijos para ocupar todo el ancho (o alto) |

### `Alignment`

Una posición dentro de una caja. Se usa en `Align`, `Container(alignment:)` y `Stack(alignment:)`.

```
topLeft      topCenter      topRight
centerLeft   center         centerRight
bottomLeft   bottomCenter   bottomRight
```

### `BoxFit`

Cómo se ajusta una imagen al espacio que le diste (`width` y `height`).

| `BoxFit.` | Resultado |
|---|---|
| `cover` | Llena todo el espacio, recortando lo que sobra (el más usado para fotos) |
| `contain` | Se ve la imagen completa, puede dejar espacios vacíos |
| `fill` | Llena todo el espacio deformando la imagen |
| `fitWidth` | Ajusta al ancho |

---

## ⚠️ Errores Comunes de Principiantes

1. **Poner varios widgets en `child`:** `child` recibe **uno**. Si necesitas varios, usa un `Column` o `Row` con `children`.
2. **Olvidar los corchetes en `children`:** se escribe `children: [ ... ]`, aunque haya un solo hijo.
3. **Franja amarilla y negra (overflow):** el contenido no cabe. Usa `Expanded` en un `Row` o `SingleChildScrollView` en la pantalla.
4. **`color` y `decoration` juntos en un `Container`.**
5. **Poner `Positioned` fuera de un `Stack`:** solo funciona como hijo directo de un `Stack`.
6. **Olvidar las comas finales:** pon una coma `,` después de cada parámetro y cada `)`. Así el editor formatea el código ordenado (`Shift + Alt + F` en VS Code).

---

_Siguiente: [Ejercicio: Pantalla Stateless →](6-ejercicio-pantalla-stateless.md)_
