# 7. Guía paso a paso: Pantalla "Café UPN" ☕

> Construimos juntos, pieza por pieza, la pantalla del [Ejercicio 6](6-ejercicio-pantalla-stateless.md). En cada paso verás **qué** agregamos, **por qué** usamos ese widget y **qué** deberías ver en el emulador.

> [!WARNING]
> Si todavía no intentaste el [Ejercicio 6](6-ejercicio-pantalla-stateless.md) por tu cuenta, hazlo primero. Esta guía sirve para **comparar** tu solución o para destrabarte, no para copiar sin entender.

---

## 🗺️ Cómo usar esta guía

- Escribe el código **a mano** en lugar de copiarlo y pegarlo. Así memorizas los nombres de los widgets y sus propiedades.
- Después de cada paso, **guarda** el archivo y mira el resultado con **Hot Reload** ⚡.
- Si algo no se ve igual, compara tu código con el del paso antes de avanzar.
- Los comentarios `// 👇 PASO N` marcan **dónde** va cada pieza nueva.

Este es el plan: vamos de afuera hacia adentro, como al construir una casa 🏠. Primero el terreno y las paredes (`MaterialApp`, `Scaffold`), luego los pisos (`Column`) y al final los muebles (textos, imágenes, botones).

```mermaid
flowchart LR
    A["Paso 0<br/>Proyecto"] --> B["Pasos 1-2<br/>Estructura"]
    B --> C["Pasos 3-6<br/>Contenido"]
    C --> D["Paso 7<br/>Widget propio"]
    D --> E["Pasos 8-10<br/>Info y acciones"]
```

---

## Paso 0: Crear el proyecto

Abre una terminal y ejecuta:

```bash
flutter create cafe_upn
cd cafe_upn
```

Abre la carpeta en VS Code o Android Studio, ve a `lib/main.dart` y **borra todo** su contenido. Escribe este punto de partida:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MiApp());
}

class MiApp extends StatelessWidget {
  const MiApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Café UPN',
      theme: ThemeData(
        colorSchemeSeed: Colors.brown,
        useMaterial3: true,
      ),
      home: const PantallaCafeteria(),
    );
  }
}

class PantallaCafeteria extends StatelessWidget {
  const PantallaCafeteria({super.key});

  @override
  Widget build(BuildContext context) {
    return const Placeholder();
  }
}
```

🧠 **¿Qué hace cada parte?**

| Código | Para qué |
|---|---|
| `runApp(const MiApp())` | Arranca la app con el widget raíz `MiApp` |
| `MaterialApp` | Da el estilo de Material Design a toda la app → [repaso](1-que-es-flutter-y-materialapp.md) |
| `debugShowCheckedModeBanner: false` | Oculta la cinta roja "DEBUG" de la esquina |
| `colorSchemeSeed: Colors.brown` | Genera toda la paleta de colores a partir del café 🟤 (botones, chips y la barra la toman automáticamente) |
| `home: const PantallaCafeteria()` | La primera pantalla que se muestra |
| `Placeholder()` | Una caja con una "X" que marca "aquí irá algo" |

Ejecuta la app (`flutter run` o el botón ▶️).

✅ **Deberías ver:** una caja gris con una "X" ocupando toda la pantalla.

---

## Paso 1: Esqueleto y barra superior

Reemplaza el `return const Placeholder();` de `PantallaCafeteria` por un `Scaffold` con su `AppBar`:

```dart
class PantallaCafeteria extends StatelessWidget {
  const PantallaCafeteria({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        leading: IconButton(
          onPressed: () {},
          icon: const Icon(Icons.arrow_back),
        ),
        title: const Text('Café UPN'),
        centerTitle: true,
        actions: [
          IconButton(onPressed: () {}, icon: const Icon(Icons.favorite_border)),
          IconButton(onPressed: () {}, icon: const Icon(Icons.share)),
        ],
      ),
      body: const Placeholder(), // 👈 lo reemplazamos en el siguiente paso
    );
  }
}
```

🧠 **¿Por qué así?**

- `Scaffold` es el esqueleto de **toda** pantalla: tiene el hueco `appBar` arriba y el hueco `body` para el contenido. → [Scaffold](5-catalogo-de-widgets.md#scaffold)
- En el `AppBar`, `leading` es lo que va a la **izquierda** del título y `actions` es una **lista** de lo que va a la **derecha**. → [AppBar](5-catalogo-de-widgets.md#appbar)
- Usamos `IconButton` y no solo `Icon` porque son **botones**: el usuario espera poder tocarlos. → [Botones](5-catalogo-de-widgets.md#botones)
- `onPressed: () {}` es una función vacía: el botón se ve activo, pero todavía no hace nada.

✅ **Deberías ver:** una barra arriba con ← a la izquierda, "Café UPN" al centro y ♡ ⤴ a la derecha. Debajo, el `Placeholder`.

---

## Paso 2: Contenido con scroll

Reemplaza `body: const Placeholder(),` por:

```dart
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // 👇 Aquí irán los pasos 3 al 9, uno debajo del otro
          ],
        ),
      ),
```

🧠 **¿Por qué así?**

- La pantalla final es **más alta** que el celular. Sin `SingleChildScrollView` aparecería la franja amarilla y negra de *overflow*. → [SingleChildScrollView](5-catalogo-de-widgets.md#singlechildscrollview)
- Ponemos el `padding` en el `SingleChildScrollView` para que **todo** el contenido quede separado 16 de los bordes, sin repetirlo en cada widget. → [EdgeInsets](5-catalogo-de-widgets.md#edgeinsets)
- `Column` porque los elementos van **uno debajo del otro**. → [Column](5-catalogo-de-widgets.md#column)
- `CrossAxisAlignment.start` porque en un `Column` el eje cruzado es el **horizontal**: así todo se alinea a la **izquierda** (por defecto se centraría). → [MainAxisAlignment y CrossAxisAlignment](5-catalogo-de-widgets.md#mainaxisalignment-y-crossaxisalignment)

✅ **Deberías ver:** la barra superior y la pantalla en blanco. Es normal: el `Column` está vacío.

> [!TIP]
> Desde aquí, **todos** los pasos siguientes agregan widgets **dentro** de `children: [ ]`, al final de la lista, en orden.

---

## Paso 3: Portada con etiqueta "Abierto"

Dentro de `children`, agrega:

```dart
            // 👇 PASO 3: portada
            Stack(
              children: [
                // Capa 1 (al fondo): la imagen
                ClipRRect(
                  borderRadius: BorderRadius.circular(16),
                  child: Image.network(
                    'https://picsum.photos/600/300',
                    height: 200,
                    width: double.infinity,
                    fit: BoxFit.cover,
                  ),
                ),
                // Capa 2 (adelante): la etiqueta
                Positioned(
                  top: 12,
                  left: 12,
                  child: Container(
                    padding: const EdgeInsets.symmetric(horizontal: 12, vertical: 6),
                    decoration: BoxDecoration(
                      color: Colors.green,
                      borderRadius: BorderRadius.circular(20),
                    ),
                    child: const Text(
                      'Abierto',
                      style: TextStyle(color: Colors.white, fontWeight: FontWeight.bold),
                    ),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 16),
```

🧠 **¿Por qué así?**

- La etiqueta va **encima** de la foto, así que necesitamos `Stack`. El **primer** hijo queda al fondo y el **último**, adelante. → [Stack y Positioned](5-catalogo-de-widgets.md#stack-y-positioned)
- `Image.network` no tiene propiedad para redondear esquinas; por eso la envolvemos en `ClipRRect`, que la "recorta" con esquinas curvas. → [Image y ClipRRect](5-catalogo-de-widgets.md#image-y-cliprrect)
- `width: double.infinity` hace que la imagen ocupe **todo el ancho** disponible.
- `BoxFit.cover` llena el espacio de 200 de alto **sin deformar** la foto (recorta lo que sobra). → [BoxFit](5-catalogo-de-widgets.md#boxfit)
- `Positioned` ubica la etiqueta a 12 del borde superior y 12 del izquierdo **del `Stack`**.
- La etiqueta es un `Container` porque necesitamos **fondo de color + esquinas redondeadas + padding**. Como usamos `decoration`, el color va **dentro** de `BoxDecoration`. → [Container](5-catalogo-de-widgets.md#container), [BoxDecoration](5-catalogo-de-widgets.md#boxdecoration)
- `SizedBox(height: 16)` separa la portada de lo que viene después. → [SizedBox](5-catalogo-de-widgets.md#sizedbox)

✅ **Deberías ver:** una foto con esquinas redondeadas y una píldora verde "Abierto" en su esquina superior izquierda.

> **Nota:** `picsum.photos` devuelve una foto al azar. Si al recargar cambia la imagen, es normal.

---

## Paso 4: Título y calificación

Debajo del `SizedBox` del paso anterior, agrega:

```dart
            // 👇 PASO 4: título y calificación
            const Text(
              'Café UPN',
              style: TextStyle(fontSize: 26, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 4),
            const Row(
              children: [
                Icon(Icons.star, color: Colors.amber, size: 20),
                SizedBox(width: 4),
                Text('4.8', style: TextStyle(fontWeight: FontWeight.bold)),
                SizedBox(width: 4),
                Text('(230 reseñas)', style: TextStyle(color: Colors.grey)),
                Spacer(),
                Text('S/ 8 - 25'),
              ],
            ),
            const SizedBox(height: 16),
```

🧠 **¿Por qué así?**

- El título destaca por su **tamaño** y **grosor**, que se definen con `TextStyle`. → [TextStyle](5-catalogo-de-widgets.md#textstyle)
- La calificación es una línea con varios elementos **uno al lado del otro**: eso es un `Row`. → [Row](5-catalogo-de-widgets.md#row)
- En un `Row` separamos con `SizedBox(width: ...)` (ancho), no con `height`.
- "(230 reseñas)" va en gris para que tenga **menos protagonismo** que la calificación. → [Colors](5-catalogo-de-widgets.md#colors)
- `Spacer` ocupa todo el espacio libre del medio y **empuja** el precio hasta el borde derecho. → [Spacer](5-catalogo-de-widgets.md#spacer)
- Todo el `Row` puede ser `const` porque nada de lo que tiene dentro cambia.

✅ **Deberías ver:** "Café UPN" en grande y, debajo, ★ 4.8 (230 reseñas) a la izquierda y S/ 8 - 25 a la derecha.

---

## Paso 5: Etiquetas de servicios

```dart
            // 👇 PASO 5: etiquetas
            const Wrap(
              spacing: 8,
              runSpacing: 8,
              children: [
                Chip(avatar: Icon(Icons.coffee, size: 18), label: Text('Café')),
                Chip(avatar: Icon(Icons.cake, size: 18), label: Text('Postres')),
                Chip(avatar: Icon(Icons.wifi, size: 18), label: Text('Wi-Fi')),
                Chip(avatar: Icon(Icons.pets, size: 18), label: Text('Pet friendly')),
              ],
            ),
```

🧠 **¿Por qué así?**

- `Chip` es el widget hecho para **etiquetas** pequeñas: `label` es el texto y `avatar` el ícono de la izquierda. → [Chip](5-catalogo-de-widgets.md#chip)
- ¿Por qué `Wrap` y no `Row`? Cuatro chips **no caben** en una línea de un celular. Un `Row` daría *overflow*; `Wrap` baja los que no caben a la siguiente línea. → [Wrap](5-catalogo-de-widgets.md#wrap)
- `spacing` separa los chips **en la misma línea** y `runSpacing` separa **una línea de la otra**.

✅ **Deberías ver:** tres chips en una línea y "Pet friendly" en la siguiente (depende del ancho de tu emulador). Gira el emulador: ahora deberían caber todos en una sola línea.

> [!TIP]
> Haz la prueba: cambia `Wrap` por `Row` y guarda. Verás la franja amarilla y negra. Luego vuelve a `Wrap`.

---

## Paso 6: Sobre nosotros

```dart
            // 👇 PASO 6: sobre nosotros
            const Divider(height: 32),
            const Text(
              'Sobre nosotros',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            const Text(
              'Café de especialidad preparado con granos de la sierra de La Libertad. '
              'Un espacio tranquilo para estudiar, trabajar o conversar con amigos.',
              style: TextStyle(color: Colors.black54, height: 1.4),
            ),
            const SizedBox(height: 24),
```

🧠 **¿Por qué así?**

- `Divider` dibuja la línea que separa las etiquetas de la nueva sección. Su `height: 32` es el espacio **total** que ocupa: la línea queda en medio, con 16 arriba y 16 abajo. Por eso no necesitamos `SizedBox` antes ni después. → [Divider](5-catalogo-de-widgets.md#divider)
- El párrafo usa `Colors.black54` (negro semitransparente) para verse más **suave** que el título. → [Colors](5-catalogo-de-widgets.md#colors)
- `height: 1.4` en `TextStyle` agrega espacio **entre renglones**, para que el párrafo se lea mejor.
- El texto largo se escribe en **dos cadenas seguidas**: Dart las une automáticamente. Así la línea de código no queda larguísima.

✅ **Deberías ver:** una línea gris, el subtítulo "Sobre nosotros" y el párrafo en dos o tres renglones.

---

## Paso 7: Tu propio widget, `ProductoDestacado` ⭐

Las tres tarjetas del menú son **iguales**: solo cambian el ícono, el nombre y el precio. En lugar de escribir el mismo `Container` tres veces, creamos **un molde** 🍪 y lo usamos tres veces con datos distintos.

### 7.1. Crear el widget

Al **final** de `main.dart`, **fuera** de la clase `PantallaCafeteria`, agrega una clase nueva:

```dart
class ProductoDestacado extends StatelessWidget {
  final IconData icono;
  final String nombre;
  final double precio;

  const ProductoDestacado({
    super.key,
    required this.icono,
    required this.nombre,
    required this.precio,
  });

  @override
  Widget build(BuildContext context) {
    return Container(
      padding: const EdgeInsets.all(12),
      decoration: BoxDecoration(
        color: Colors.brown.shade50,
        borderRadius: BorderRadius.circular(12),
      ),
      child: Column(
        children: [
          Icon(icono, size: 32, color: Colors.brown),
          const SizedBox(height: 8),
          Text(nombre, style: const TextStyle(fontWeight: FontWeight.bold)),
          Text('S/ ${precio.toStringAsFixed(2)}'),
        ],
      ),
    );
  }
}
```

🧠 **¿Por qué así?**

- Es un `StatelessWidget` porque sus datos **no cambian** mientras se muestra. → [Stateless vs Stateful](4-stateless-vs-stateful.md)
- Los datos llegan por el **constructor** como parámetros **nombrados** y `required`: así es imposible olvidarse de alguno al usarlo. → [Parámetros Nombrados](../7.%20dart/3-parametros-nombrados.md)
- Las propiedades son `final` porque, una vez creado, el widget no cambia sus datos.
- El tipo de `icono` es `IconData`: es el tipo de valores como `Icons.coffee`.
- `precio.toStringAsFixed(2)` convierte `9.5` en `'9.50'`: siempre dos decimales, como en un precio real.
- `Icon(icono, ...)` **no** lleva `const` porque `icono` es una variable que llega por el constructor.
- `Colors.brown.shade50` es un café muy claro, ideal para fondos. → [Colors](5-catalogo-de-widgets.md#colors)

### 7.2. Usarlo en la pantalla

De vuelta en `PantallaCafeteria`, dentro de `children` y debajo del paso 6, agrega:

```dart
            // 👇 PASO 7: menú destacado
            const Text(
              'Menú destacado',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            const Row(
              children: [
                Expanded(
                  child: ProductoDestacado(icono: Icons.coffee, nombre: 'Latte', precio: 9.5),
                ),
                SizedBox(width: 8),
                Expanded(
                  child: ProductoDestacado(icono: Icons.icecream, nombre: 'Frappé', precio: 12.0),
                ),
                SizedBox(width: 8),
                Expanded(
                  child: ProductoDestacado(icono: Icons.bakery_dining, nombre: 'Croissant', precio: 6.0),
                ),
              ],
            ),
            const SizedBox(height: 24),
```

🧠 **¿Por qué así?**

- Usamos `ProductoDestacado` **igual que cualquier widget de Flutter**: nombre + parámetros. ¡Acabas de crear tu propio widget! 🎉
- Cada tarjeta va dentro de un `Expanded`: los tres se reparten el ancho **en partes iguales**. Sin `Expanded`, cada tarjeta mediría según su texto y "Croissant" sería más ancha que "Latte". → [Expanded](5-catalogo-de-widgets.md#expanded)
- El `Row` completo puede ser `const` porque `ProductoDestacado` tiene un constructor `const` y le pasamos valores fijos.

✅ **Deberías ver:** tres cajitas café claro del mismo ancho, cada una con su ícono, nombre y precio con dos decimales.

---

## Paso 8: Tarjeta de información

```dart
            // 👇 PASO 8: información
            const Card(
              child: Column(
                children: [
                  ListTile(
                    leading: Icon(Icons.schedule),
                    title: Text('Horario'),
                    subtitle: Text('Lun a Sáb · 7:00 a. m. - 9:00 p. m.'),
                  ),
                  Divider(height: 1),
                  ListTile(
                    leading: Icon(Icons.location_on),
                    title: Text('Dirección'),
                    subtitle: Text('Av. Principal 123, Trujillo'),
                  ),
                  Divider(height: 1),
                  ListTile(
                    leading: Icon(Icons.phone),
                    title: Text('Teléfono'),
                    subtitle: Text('(044) 123 456'),
                  ),
                ],
              ),
            ),
            const SizedBox(height: 24),
```

🧠 **¿Por qué así?**

- `Card` agrupa información relacionada con fondo, esquinas redondeadas y sombra, sin tener que decorar un `Container` a mano. → [Card](5-catalogo-de-widgets.md#card)
- `Card` tiene **un solo** `child`; como queremos tres filas, metemos un `Column` dentro.
- Cada fila es un `ListTile`: ya trae el espacio para ícono (`leading`), título (`title`) y detalle (`subtitle`) bien alineados. Hacerlo a mano necesitaría un `Row` + `Column` + `Padding` por fila. → [ListTile](5-catalogo-de-widgets.md#listtile)
- `Divider(height: 1)` dibuja una línea pegada entre filas, sin espacio extra (los `ListTile` ya tienen su propio relleno).
- Aquí **no** usamos `Padding` dentro del `Card` porque `ListTile` ya trae su propio espacio interno.

✅ **Deberías ver:** una tarjeta con tres filas (horario, dirección y teléfono), cada una con su ícono, separadas por líneas finas.

---

## Paso 9: Botones de acción

```dart
            // 👇 PASO 9: botones
            Row(
              children: [
                Expanded(
                  child: OutlinedButton.icon(
                    onPressed: () {},
                    icon: const Icon(Icons.phone),
                    label: const Text('Llamar'),
                  ),
                ),
                const SizedBox(width: 12),
                Expanded(
                  child: FilledButton.icon(
                    onPressed: () {},
                    icon: const Icon(Icons.directions),
                    label: const Text('Cómo llegar'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 80), // espacio para que el FAB no tape los botones
```

🧠 **¿Por qué así?**

- El constructor `.icon` crea un botón con **ícono y texto**. Por eso recibe `icon:` y `label:` en lugar de `child:`. → [Botones](5-catalogo-de-widgets.md#botones)
- `FilledButton` (relleno de color) para **"Cómo llegar"**, la acción principal. `OutlinedButton` (solo borde) para **"Llamar"**, la secundaria. Así el usuario sabe cuál es la más importante.
- Otra vez `Expanded`: los dos botones miden **lo mismo**, sin importar el largo de su texto.
- Este `Row` **no** puede ser `const` porque los botones reciben una función `() {}`. Por eso `const` va en cada `Icon`, `Text` y `SizedBox` por separado.
- El `SizedBox(height: 80)` final deja espacio vacío al fondo: sin él, al hacer scroll hasta abajo, el botón flotante del siguiente paso taparía "Cómo llegar".

✅ **Deberías ver:** dos botones del mismo ancho: "Llamar" con borde y "Cómo llegar" con fondo café.

---

## Paso 10: Botón flotante

El botón flotante **no** va dentro del `body`: tiene su propio hueco en el `Scaffold`. Agrégalo debajo del `appBar`:

```dart
    return Scaffold(
      appBar: AppBar(
        // ... lo del paso 1
      ),
      // 👇 PASO 10: botón flotante
      floatingActionButton: FloatingActionButton.extended(
        onPressed: () {},
        icon: const Icon(Icons.event_seat),
        label: const Text('Reservar'),
      ),
      body: SingleChildScrollView(
        // ... lo de los pasos 2 al 9
      ),
    );
```

🧠 **¿Por qué así?**

- `floatingActionButton` es una propiedad del `Scaffold`: él se encarga de ubicarlo en la esquina inferior derecha y de que **flote** sobre el contenido mientras haces scroll. → [FloatingActionButton](5-catalogo-de-widgets.md#floatingactionbutton), [Scaffold](5-catalogo-de-widgets.md#scaffold)
- `.extended` crea la versión "píldora" con ícono **y** texto, más clara que un círculo con solo un ícono.
- El orden de las propiedades dentro del `Scaffold` no importa: `floatingActionButton` puede ir antes o después de `body`.

✅ **Deberías ver:** la pantalla completa del [Ejercicio 6](6-ejercicio-pantalla-stateless.md), con el botón "Reservar" flotando abajo a la derecha. ¡Terminaste! 🎉

---

## 📄 Código completo

<details>
<summary>Ver <code>lib/main.dart</code> completo</summary>

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MiApp());
}

class MiApp extends StatelessWidget {
  const MiApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Café UPN',
      theme: ThemeData(
        colorSchemeSeed: Colors.brown,
        useMaterial3: true,
      ),
      home: const PantallaCafeteria(),
    );
  }
}

class PantallaCafeteria extends StatelessWidget {
  const PantallaCafeteria({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        leading: IconButton(
          onPressed: () {},
          icon: const Icon(Icons.arrow_back),
        ),
        title: const Text('Café UPN'),
        centerTitle: true,
        actions: [
          IconButton(onPressed: () {}, icon: const Icon(Icons.favorite_border)),
          IconButton(onPressed: () {}, icon: const Icon(Icons.share)),
        ],
      ),
      floatingActionButton: FloatingActionButton.extended(
        onPressed: () {},
        icon: const Icon(Icons.event_seat),
        label: const Text('Reservar'),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // PASO 3: portada
            Stack(
              children: [
                ClipRRect(
                  borderRadius: BorderRadius.circular(16),
                  child: Image.network(
                    'https://picsum.photos/600/300',
                    height: 200,
                    width: double.infinity,
                    fit: BoxFit.cover,
                  ),
                ),
                Positioned(
                  top: 12,
                  left: 12,
                  child: Container(
                    padding: const EdgeInsets.symmetric(horizontal: 12, vertical: 6),
                    decoration: BoxDecoration(
                      color: Colors.green,
                      borderRadius: BorderRadius.circular(20),
                    ),
                    child: const Text(
                      'Abierto',
                      style: TextStyle(color: Colors.white, fontWeight: FontWeight.bold),
                    ),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 16),

            // PASO 4: título y calificación
            const Text(
              'Café UPN',
              style: TextStyle(fontSize: 26, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 4),
            const Row(
              children: [
                Icon(Icons.star, color: Colors.amber, size: 20),
                SizedBox(width: 4),
                Text('4.8', style: TextStyle(fontWeight: FontWeight.bold)),
                SizedBox(width: 4),
                Text('(230 reseñas)', style: TextStyle(color: Colors.grey)),
                Spacer(),
                Text('S/ 8 - 25'),
              ],
            ),
            const SizedBox(height: 16),

            // PASO 5: etiquetas
            const Wrap(
              spacing: 8,
              runSpacing: 8,
              children: [
                Chip(avatar: Icon(Icons.coffee, size: 18), label: Text('Café')),
                Chip(avatar: Icon(Icons.cake, size: 18), label: Text('Postres')),
                Chip(avatar: Icon(Icons.wifi, size: 18), label: Text('Wi-Fi')),
                Chip(avatar: Icon(Icons.pets, size: 18), label: Text('Pet friendly')),
              ],
            ),

            // PASO 6: sobre nosotros
            const Divider(height: 32),
            const Text(
              'Sobre nosotros',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            const Text(
              'Café de especialidad preparado con granos de la sierra de La Libertad. '
              'Un espacio tranquilo para estudiar, trabajar o conversar con amigos.',
              style: TextStyle(color: Colors.black54, height: 1.4),
            ),
            const SizedBox(height: 24),

            // PASO 7: menú destacado
            const Text(
              'Menú destacado',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            const Row(
              children: [
                Expanded(
                  child: ProductoDestacado(icono: Icons.coffee, nombre: 'Latte', precio: 9.5),
                ),
                SizedBox(width: 8),
                Expanded(
                  child: ProductoDestacado(icono: Icons.icecream, nombre: 'Frappé', precio: 12.0),
                ),
                SizedBox(width: 8),
                Expanded(
                  child: ProductoDestacado(icono: Icons.bakery_dining, nombre: 'Croissant', precio: 6.0),
                ),
              ],
            ),
            const SizedBox(height: 24),

            // PASO 8: información
            const Card(
              child: Column(
                children: [
                  ListTile(
                    leading: Icon(Icons.schedule),
                    title: Text('Horario'),
                    subtitle: Text('Lun a Sáb · 7:00 a. m. - 9:00 p. m.'),
                  ),
                  Divider(height: 1),
                  ListTile(
                    leading: Icon(Icons.location_on),
                    title: Text('Dirección'),
                    subtitle: Text('Av. Principal 123, Trujillo'),
                  ),
                  Divider(height: 1),
                  ListTile(
                    leading: Icon(Icons.phone),
                    title: Text('Teléfono'),
                    subtitle: Text('(044) 123 456'),
                  ),
                ],
              ),
            ),
            const SizedBox(height: 24),

            // PASO 9: botones
            Row(
              children: [
                Expanded(
                  child: OutlinedButton.icon(
                    onPressed: () {},
                    icon: const Icon(Icons.phone),
                    label: const Text('Llamar'),
                  ),
                ),
                const SizedBox(width: 12),
                Expanded(
                  child: FilledButton.icon(
                    onPressed: () {},
                    icon: const Icon(Icons.directions),
                    label: const Text('Cómo llegar'),
                  ),
                ),
              ],
            ),
            const SizedBox(height: 80),
          ],
        ),
      ),
    );
  }
}

class ProductoDestacado extends StatelessWidget {
  final IconData icono;
  final String nombre;
  final double precio;

  const ProductoDestacado({
    super.key,
    required this.icono,
    required this.nombre,
    required this.precio,
  });

  @override
  Widget build(BuildContext context) {
    return Container(
      padding: const EdgeInsets.all(12),
      decoration: BoxDecoration(
        color: Colors.brown.shade50,
        borderRadius: BorderRadius.circular(12),
      ),
      child: Column(
        children: [
          Icon(icono, size: 32, color: Colors.brown),
          const SizedBox(height: 8),
          Text(nombre, style: const TextStyle(fontWeight: FontWeight.bold)),
          Text('S/ ${precio.toStringAsFixed(2)}'),
        ],
      ),
    );
  }
}
```

</details>

---

## 🧯 Si algo sale mal

| Síntoma | Causa probable | Solución |
|---|---|---|
| Franja amarilla y negra en el borde derecho | Un `Row` con hijos que no caben | Usa `Wrap` (paso 5) o envuelve hijos en `Expanded` (pasos 7 y 9) |
| Franja amarilla y negra abajo | Falta el scroll | Revisa que el `Column` esté dentro de `SingleChildScrollView` (paso 2) |
| La imagen no aparece | El emulador no tiene internet | Verifica la conexión del emulador o prueba abriendo el navegador en él |
| Error *"The argument type 'List<Widget>' can't be assigned to 'Widget'"* | Usaste `child:` con una lista `[ ]` | `child` recibe un widget; para varios usa `children` |
| Error *"Cannot provide both a color and a decoration"* | `color` y `decoration` juntos en un `Container` | Mueve el `color` dentro de `BoxDecoration` (pasos 3 y 7) |
| Error *"The named parameter 'precio' is required"* | Al usar `ProductoDestacado` olvidaste un parámetro | Pasa `icono`, `nombre` y `precio` |
| Error *"Invalid constant value"* | Pusiste `const` en algo que usa una variable o una función | Quita ese `const` (ej. el `Row` de botones o `Icon(icono)`) |
| El botón "Reservar" tapa "Cómo llegar" | Falta el espacio final | Agrega el `SizedBox(height: 80)` al final del `Column` (paso 9) |

---

## 🎓 Lo que aprendiste

| Concepto | Dónde lo usaste |
|---|---|
| Estructura de pantalla con `Scaffold` y `AppBar` | Pasos 1 y 10 |
| Scroll con `SingleChildScrollView` | Paso 2 |
| Widgets encima de otros con `Stack` y `Positioned` | Paso 3 |
| Empujar a los extremos con `Spacer` | Paso 4 |
| Contenido que baja de línea con `Wrap` | Paso 5 |
| Repartir el ancho en partes iguales con `Expanded` | Pasos 7 y 9 |
| Crear tu propio `StatelessWidget` con parámetros | Paso 7 |
| Filas prediseñadas con `ListTile` dentro de un `Card` | Paso 8 |
| Elegir el botón según la importancia de la acción | Paso 9 |

Ahora intenta los [Retos extra del Ejercicio 6](6-ejercicio-pantalla-stateless.md) **sin** mirar esta guía. 💪

---

_Anterior: [← Ejercicio: Pantalla Stateless](6-ejercicio-pantalla-stateless.md)_
