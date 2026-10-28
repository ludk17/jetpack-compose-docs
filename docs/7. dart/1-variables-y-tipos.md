# 1. Dart: Conceptos Básicos, Variables y Tipos de Datos

> Primeros pasos con Dart, el lenguaje detrás de Flutter. Si ya conoces Kotlin, verás que muchas ideas se repiten con otra sintaxis.

---

## 🎯 ¿Qué es Dart?

Dart es un lenguaje de programación creado por Google. Es el lenguaje que usa **Flutter** para construir aplicaciones móviles, web y de escritorio con un solo código.

Imagina que Kotlin y Dart son **dos idiomas hermanos** (como el español y el portugués): comparten gran parte del vocabulario y la gramática, pero cada uno tiene sus propias palabras y reglas. Si entiendes uno, aprender el otro es mucho más rápido.

| Característica | Kotlin | Dart |
|---|---|---|
| Creado por | JetBrains | Google |
| Framework de UI | Jetpack Compose | Flutter |
| Tipado | Estático con inferencia | Estático con inferencia |
| Null safety | ✅ Sí | ✅ Sí |
| Punto y coma `;` | Opcional | **Obligatorio** |

> [!TIP]
> Puedes probar todos los ejemplos de este documento sin instalar nada en **[DartPad](https://dartpad.dev)**, un editor de Dart en el navegador.

---

## 🚀 El punto de entrada: `main()`

Todo programa en Dart empieza en la función `main()`. Es la "puerta principal" por donde entra la ejecución.

```dart
void main() {
  print('¡Hola, Dart!');
}
```

Observa tres detalles importantes:
- `print()` muestra texto en la consola (equivalente a `println()` en Kotlin).
- El texto puede ir entre comillas **simples** `'...'` o **dobles** `"..."`. En Dart se prefieren las simples.
- **Cada instrucción termina en punto y coma `;`**. Olvidarlo es el error más común al venir de Kotlin.

### Comentarios

```dart
// Comentario de una línea

/*
  Comentario de
  varias líneas
*/

/// Comentario de documentación (aparece al pasar el mouse sobre una función o clase)
```

---

## 🏷️ Variables: `var`, `final` y `const`

Dart tiene tres formas principales de declarar variables. Volvamos a la analogía de la habitación:

- **`var`** → una **pizarra**: puedes borrar y escribir otro valor cuando quieras.
- **`final`** → una **hoja escrita con lapicero**: se escribe una sola vez (puede ser mientras el programa se ejecuta) y ya no cambia.
- **`const`** → algo **impreso de fábrica**: el valor debe conocerse antes de que el programa se ejecute (en tiempo de compilación).

```dart
void main() {
  // Mutable
  var contador = 0;
  contador = 1;            // ✅ Válido

  // Inmutable, se asigna una vez (puede calcularse al ejecutar)
  final horaInicio = DateTime.now();
  // horaInicio = DateTime.now(); // ❌ Error: no se puede reasignar

  // Constante de compilación
  const pi = 3.1416;
  // const ahora = DateTime.now(); // ❌ Error: DateTime.now() no se conoce al compilar
}
```

| Dart | Kotlin equivalente | ¿Se puede reasignar? | ¿Cuándo se conoce el valor? |
|---|---|---|---|
| `var` | `var` | ✅ Sí | Al ejecutar |
| `final` | `val` | ❌ No | Al ejecutar |
| `const` | `const val` | ❌ No | Al compilar |

> [!TIP]
> **Regla de oro (igual que en Kotlin):** usa `final` por defecto, y `var` solo cuando el valor realmente necesite cambiar. En Flutter verás `const` muchísimo, porque ayuda a que la interfaz se redibuje más rápido.

---

## 🧱 Tipos de Datos Básicos

Dart es de **tipado estático**: cada variable tiene un tipo fijo. Gracias a la **inferencia de tipos**, no siempre necesitas escribirlo.

| Tipo | Descripción | Ejemplo |
|---|---|---|
| `int` | Números enteros | `int edad = 20;` |
| `double` | Números decimales | `double precio = 19.99;` |
| `num` | Puede ser `int` **o** `double` | `num nota = 15;` |
| `String` | Texto | `String nombre = 'Ana';` |
| `bool` | Verdadero o falso | `bool activo = true;` |
| `dynamic` | Cualquier tipo (sin verificación) | `dynamic dato = 5;` |

> **Nota:** A diferencia de Kotlin, en Dart los tipos `int`, `double` y `bool` se escriben en **minúscula**. `String` sí va con mayúscula. Además, Dart no tiene `Float`: todos los decimales son `double`.

### Tipo explícito vs inferencia

```dart
void main() {
  // Con inferencia (Dart adivina el tipo)
  var mensaje = 'Hola a todos'; // String
  var cantidad = 5;             // int
  final precio = 9.90;          // double

  // Con tipo explícito (el tipo reemplaza a "var")
  String usuario = 'Ana';
  int puntaje = 100;
  double altura = 1.75;
  bool logueado = false;

  // final también acepta tipo explícito
  final String ciudad = 'Cajamarca';
}
```

> [!WARNING]
> Una vez que una variable tiene tipo, no puede guardar otro tipo:
> ```dart
> var edad = 20;
> edad = 'veinte'; // ❌ Error: un String no puede asignarse a un int
> ```
> Evita `dynamic` salvo que sea realmente necesario: desactiva estas verificaciones y los errores aparecerán recién al ejecutar.

---

## 💬 Interpolación de Strings

Igual que en Kotlin, se usa **`$`** para insertar variables dentro del texto, y **`${...}`** para expresiones.

```dart
void main() {
  final nombre = 'Carlos';
  final edad = 22;
  final precio = 50.0;

  print('Hola $nombre, tienes $edad años.');
  // Hola Carlos, tienes 22 años.

  print('El precio con IGV es ${precio * 1.18}');
  // El precio con IGV es 59.0

  print('Tu nombre tiene ${nombre.length} letras');
  // Tu nombre tiene 6 letras
}
```

### Strings de varias líneas

```dart
final poema = '''
Rosas rojas,
violetas azules,
Dart es genial.
''';
```

---

## 📦 Colecciones básicas: `List`, `Set` y `Map`

Las colecciones guardan varios valores en una sola variable.

| Colección | Analogía | Sintaxis | Kotlin equivalente |
|---|---|---|---|
| `List` | Fila de personas (ordenada, admite repetidos) | `[1, 2, 3]` | `mutableListOf(1, 2, 3)` |
| `Set` | Bolsa de canicas únicas (sin repetidos) | `{1, 2, 3}` | `mutableSetOf(1, 2, 3)` |
| `Map` | Diccionario (clave → valor) | `{'a': 1}` | `mutableMapOf("a" to 1)` |

```dart
void main() {
  // List
  final frutas = <String>['manzana', 'pera'];
  frutas.add('uva');
  print(frutas[0]);       // manzana
  print(frutas.length);   // 3

  // Set
  final colores = {'rojo', 'azul', 'rojo'};
  print(colores);         // {rojo, azul}  → el repetido se ignora

  // Map
  final notas = <String, int>{
    'Ana': 18,
    'Luis': 15,
  };
  notas['María'] = 20;
  print(notas['Ana']);    // 18
}
```

> **Nota:** En Dart, una `List` declarada con `final` **sí puede modificarse por dentro** (`add`, `remove`). `final` solo impide reasignar la variable a otra lista. Si quieres una lista totalmente inmutable, usa `const`:
> ```dart
> const dias = ['lunes', 'martes'];
> // dias.add('miércoles'); // ❌ Error al ejecutar: lista inmutable
> ```

---

## ❓ Null Safety (Seguridad ante nulos)

Igual que Kotlin, Dart **no permite `null` por defecto**. Para permitirlo, se agrega `?` al tipo.

```dart
void main() {
  String nombre = 'Ana';
  // nombre = null; // ❌ Error: String no acepta null

  String? apodo;       // Puede ser null (y empieza en null)
  apodo = 'Anita';     // ✅
  apodo = null;        // ✅
}
```

Los operadores son prácticamente los mismos que en Kotlin:

| Operador | Nombre | Dart | Kotlin |
|---|---|---|---|
| `?.` | Acceso seguro | `apodo?.length` | `apodo?.length` |
| `??` | Valor por defecto | `apodo ?? 'Sin apodo'` | `apodo ?: "Sin apodo"` |
| `!` | Afirmar que no es null | `apodo!.length` | `apodo!!.length` |
| `??=` | Asignar si es null | `apodo ??= 'Anónimo';` | — |

```dart
void main() {
  String? apodo;

  print(apodo?.length);           // null
  print(apodo ?? 'Sin apodo');    // Sin apodo

  apodo ??= 'Anónimo';
  print(apodo);                   // Anónimo
}
```

> [!WARNING]
> El operador `!` es como decirle a Dart *"confía en mí, no es null"*. Si te equivocas, la app se cae en tiempo de ejecución. Úsalo lo menos posible.

---

## ⚠️ Errores Comunes de Principiantes

1. **Olvidar el punto y coma `;`:** en Dart es obligatorio al final de cada instrucción.
2. **Escribir `Int` o `Boolean`:** en Dart son `int` y `bool` (minúscula).
3. **Usar `const` con valores que se calculan al ejecutar:** `const hoy = DateTime.now();` falla. Usa `final`.
4. **Pensar que `final` congela una lista:** solo impide reasignar la variable; su contenido sí puede cambiar.

---

_Siguiente: [Funciones →](2-funciones.md)_
