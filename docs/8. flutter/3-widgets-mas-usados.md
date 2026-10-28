# 3. Widgets más usados

> Las piezas de LEGO que usarás en casi todas las pantallas: texto, imágenes, botones, espacios y layouts.

---

## 🗺️ Mapa rápido

| Categoría | Widgets | Equivalente en Compose |
|---|---|---|
| Mostrar contenido | `Text`, `Icon`, `Image` | `Text`, `Icon`, `Image` |
| Organizar (layout) | `Column`, `Row`, `Stack`, `Center` | `Column`, `Row`, `Box` |
| Espaciado y estilo | `Padding`, `SizedBox`, `Container` | `Modifier.padding`, `Spacer`, `Modifier.background` |
| Interacción | `ElevatedButton`, `TextButton`, `IconButton`, `TextField` | `Button`, `TextButton`, `IconButton`, `TextField` |
| Listas | `ListView`, `ListTile` | `LazyColumn`, `ListItem` |

> **Nota:** En Compose el espaciado y los colores se aplican con `Modifier`. En Flutter **no existe `Modifier`**: en su lugar **envuelves** el widget en otro widget (`Padding`, `Container`, `Center`...).

---

## 📝 Mostrar contenido

### `Text`

```dart
const Text(
  'Hola Flutter',
  style: TextStyle(
    fontSize: 24,
    fontWeight: FontWeight.bold,
    color: Colors.indigo,
  ),
)
```

### `Icon`

```dart
const Icon(Icons.favorite, color: Colors.red, size: 32)
```

> [!TIP]
> Puedes buscar todos los íconos disponibles en [fonts.google.com/icons](https://fonts.google.com/icons). Se usan como `Icons.nombre_del_icono`.

### `Image`

```dart
// Desde internet
Image.network('https://picsum.photos/300', width: 200)

// Desde los archivos del proyecto (requiere declararla en pubspec.yaml)
Image.asset('assets/logo.png', width: 100)
```

---

## 📐 Organizar: `Column`, `Row`, `Center` y `Stack`

Imagina que ordenas libros 📚:
- **`Column`** → los apilas uno **encima** de otro (vertical).
- **`Row`** → los pones uno **al lado** de otro en el estante (horizontal).
- **`Stack`** → los pones uno **sobre** otro, superpuestos (como `Box` en Compose).
- **`Center`** → pones un solo libro en el **centro** de la mesa.

```dart
Column(
  mainAxisAlignment: MainAxisAlignment.center,
  crossAxisAlignment: CrossAxisAlignment.start,
  children: const [
    Text('Nombre: Ana'),
    Text('Carrera: Sistemas'),
    Text('Ciclo: 6'),
  ],
)
```

```dart
Row(
  mainAxisAlignment: MainAxisAlignment.spaceBetween,
  children: const [
    Icon(Icons.star),
    Text('4.8'),
    Text('(120 reseñas)'),
  ],
)
```

### Alineación: eje principal y eje cruzado

| Widget | `mainAxisAlignment` (eje principal) | `crossAxisAlignment` (eje cruzado) |
|---|---|---|
| `Column` | Vertical ↕️ | Horizontal ↔️ |
| `Row` | Horizontal ↔️ | Vertical ↕️ |

Valores comunes de `mainAxisAlignment`: `start`, `center`, `end`, `spaceBetween`, `spaceAround`, `spaceEvenly`.

### `Stack`

```dart
Stack(
  children: [
    Image.network('https://picsum.photos/300'),
    const Positioned(
      bottom: 8,
      left: 8,
      child: Text('Foto del día', style: TextStyle(color: Colors.white)),
    ),
  ],
)
```

---

## 📏 Espaciado y estilo

### `Padding`

Agrega espacio **alrededor** de un widget.

```dart
const Padding(
  padding: EdgeInsets.all(16),
  child: Text('Con espacio alrededor'),
)
```

| `EdgeInsets` | Significado |
|---|---|
| `EdgeInsets.all(16)` | 16 en los cuatro lados |
| `EdgeInsets.symmetric(horizontal: 16, vertical: 8)` | 16 a los lados, 8 arriba y abajo |
| `EdgeInsets.only(top: 20)` | Solo arriba |

### `SizedBox`

Una caja vacía de tamaño fijo. Se usa mucho como **separador** entre widgets.

```dart
Column(
  children: const [
    Text('Título'),
    SizedBox(height: 16), // espacio de 16 entre ambos textos
    Text('Subtítulo'),
  ],
)
```

### `Container`

La "navaja suiza" 🔧: tamaño, color, bordes redondeados, margen y padding, todo en un widget.

```dart
Container(
  width: 200,
  padding: const EdgeInsets.all(16),
  margin: const EdgeInsets.all(8),
  decoration: BoxDecoration(
    color: Colors.indigo.shade50,
    borderRadius: BorderRadius.circular(12),
  ),
  child: const Text('Soy una tarjeta'),
)
```

> [!WARNING]
> Si usas `decoration`, el color va **dentro** de `BoxDecoration`. Poner `color:` y `decoration:` a la vez en el `Container` provoca un error.

---

## 👆 Interacción

### Botones

```dart
Column(
  children: [
    ElevatedButton(
      onPressed: () => print('Guardar'),
      child: const Text('Guardar'),
    ),
    TextButton(
      onPressed: () => print('Cancelar'),
      child: const Text('Cancelar'),
    ),
    IconButton(
      onPressed: () => print('Me gusta'),
      icon: const Icon(Icons.thumb_up),
    ),
  ],
)
```

| Botón | Aspecto | Uso típico |
|---|---|---|
| `ElevatedButton` | Con fondo y sombra | Acción principal |
| `FilledButton` | Fondo de color sólido | Acción principal (Material 3) |
| `OutlinedButton` | Solo borde | Acción secundaria |
| `TextButton` | Solo texto | Acciones menores ("Cancelar") |
| `IconButton` | Solo ícono | Barras de herramientas |

> **Nota:** Si `onPressed` es `null`, el botón se muestra **deshabilitado** (gris).

### `TextField`

Campo para que el usuario escriba.

```dart
TextField(
  decoration: const InputDecoration(
    labelText: 'Correo',
    hintText: 'ejemplo@upn.pe',
    border: OutlineInputBorder(),
    prefixIcon: Icon(Icons.email),
  ),
  onChanged: (texto) {
    print('Escribió: $texto');
  },
)
```

---

## 📜 Listas: `ListView` y `ListTile`

`ListView` muestra elementos con **scroll**. `ListTile` es una fila prediseñada con ícono, título y subtítulo.

```dart
ListView(
  children: const [
    ListTile(
      leading: Icon(Icons.person),
      title: Text('Ana Torres'),
      subtitle: Text('Sistemas'),
      trailing: Icon(Icons.chevron_right),
    ),
    ListTile(
      leading: Icon(Icons.person),
      title: Text('Luis Pérez'),
      subtitle: Text('Industrial'),
    ),
  ],
)
```

Para listas largas o que vienen de datos, usa **`ListView.builder`**: solo construye los elementos visibles (igual que `LazyColumn`).

```dart
final cursos = ['Kotlin', 'Compose', 'Dart', 'Flutter'];

ListView.builder(
  itemCount: cursos.length,
  itemBuilder: (context, index) {
    return ListTile(
      leading: Text('${index + 1}'),
      title: Text(cursos[index]),
    );
  },
)
```

---

## 🧩 Todo junto: tarjeta de perfil

```dart
import 'package:flutter/material.dart';

class PantallaPerfil extends StatelessWidget {
  const PantallaPerfil({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Perfil')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            const Icon(Icons.account_circle, size: 100, color: Colors.indigo),
            const SizedBox(height: 8),
            const Text(
              'Ana Torres',
              textAlign: TextAlign.center,
              style: TextStyle(fontSize: 22, fontWeight: FontWeight.bold),
            ),
            const Text('Ingeniería de Sistemas', textAlign: TextAlign.center),
            const SizedBox(height: 24),
            const Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                Text('📚 12 cursos'),
                Text('⭐ 4.9'),
              ],
            ),
            const SizedBox(height: 24),
            ElevatedButton(
              onPressed: () {},
              child: const Text('Editar perfil'),
            ),
          ],
        ),
      ),
    );
  }
}
```

> [!TIP]
> **¿Por qué tanto `const`?** Un widget `const` se crea una sola vez y Flutter no necesita reconstruirlo al redibujar. Si el editor te sugiere agregar `const`, acéptalo.

---

## ⚠️ Errores Comunes de Principiantes

1. **Buscar `Modifier`:** en Flutter se envuelve el widget (`Padding(child: ...)`) en lugar de encadenar modificadores.
2. **`ListView` dentro de `Column` sin tamaño:** provoca el error *"Vertical viewport was given unbounded height"*. Envuelve el `ListView` en `Expanded`.
3. **Contenido que se sale de la pantalla:** aparece una franja amarilla y negra (*overflow*). Usa `Expanded`, `SingleChildScrollView` o reduce tamaños.
4. **Poner `color` y `decoration` juntos en `Container`.**

---

_Siguiente: [Stateless vs Stateful →](4-stateless-vs-stateful.md)_
