# 1. `Future`, `async` y `await` en Dart

> Antes de pedir datos a internet necesitas entender cómo Dart **espera** algo que tarda, sin congelar la pantalla.

---

## 🍔 La analogía: el patio de comidas y el beeper

Vas al patio de comidas, pides una hamburguesa y te entregan un **beeper** 📟.

- El beeper **no es** la hamburguesa: es una **promesa** de que la hamburguesa llegará.
- Mientras esperas, **no te quedas congelado** frente al mostrador: te sientas, conversas, revisas el celular.
- Cuando el beeper vibra, vas y recoges tu pedido... **o** te dicen "se acabó el pan" (un **error**).

En Dart, ese beeper se llama **`Future`**.

| Patio de comidas | Dart |
|---|---|
| El beeper 📟 | `Future<Hamburguesa>` |
| Esperar a que vibre | `await` |
| Recoger la hamburguesa | El valor que devuelve el `Future` |
| "Se acabó el pan" | Una excepción (error) |
| Seguir conversando mientras esperas | La app sigue respondiendo (no se congela) |

> **Nota:** Si llevaste la parte de Kotlin del curso, esto es lo mismo que vimos en [Síncrono vs Asíncrono](../2.%20http/5-sync-vs-async.md): `Future` + `async`/`await` en Dart cumplen el papel de las funciones `suspend` y las corrutinas en Kotlin.

---

## 📦 ¿Qué es un `Future`?

Un `Future<T>` es un valor de tipo `T` que **todavía no existe**, pero existirá en el futuro.

| Tipo | Significa |
|---|---|
| `Future<String>` | "Más adelante te daré un `String`" |
| `Future<int>` | "Más adelante te daré un `int`" |
| `Future<List<Mascota>>` | "Más adelante te daré una lista de mascotas" |
| `Future<void>` | "Avisaré cuando termine, pero no devuelvo nada" |

Todo lo que **tarda** devuelve un `Future`: pedir datos a internet, leer un archivo, esperar un tiempo.

---

## ⏳ `async` y `await`

- **`async`** se escribe después de los paréntesis de una función para decir: *"esta función hace cosas que tardan"*. Una función `async` **siempre** devuelve un `Future`.
- **`await`** se escribe antes de un `Future` para decir: *"espera aquí hasta que el beeper vibre y dame el resultado"*.

```dart
// Simula algo que tarda 2 segundos (como una petición a internet)
Future<String> pedirCafe() async {
  await Future.delayed(const Duration(seconds: 2));
  return 'Café listo ☕';
}

void main() async {
  print('1. Hago el pedido');
  final cafe = await pedirCafe(); // ⏸️ espera 2 segundos
  print('2. $cafe');
}
```

Salida:

```
1. Hago el pedido
2. Café listo ☕        ← aparece 2 segundos después
```

Fíjate en tres detalles:

1. `pedirCafe` dice `return 'Café listo ☕'` (un `String`), pero su tipo es `Future<String>`. Dart lo **envuelve** solo por ser `async`.
2. Gracias a `await`, la variable `cafe` es un `String`, no un `Future<String>`.
3. `main` también lleva `async`, porque **dentro** usa `await`.

> [!WARNING]
> **`await` solo se puede usar dentro de una función marcada con `async`.** Si lo olvidas, verás el error: *"The await expression can only be used in an async function"*.

### ¿Qué pasa si olvido el `await`?

```dart
void main() async {
  final cafe = pedirCafe();  // ❌ sin await
  print(cafe);               // Imprime: Instance of 'Future<String>'
}
```

Te quedaste con el **beeper** en la mano, no con el café. 📟

---

## 🧯 Errores: `try` / `catch` / `finally`

Lo que tarda también puede **fallar** (no hay internet, el servidor no responde...). Un `Future` que falla lanza una **excepción** en el `await`. Para atraparla se usa `try`/`catch`:

```dart
Future<int> dividir(int a, int b) async {
  await Future.delayed(const Duration(milliseconds: 500));
  if (b == 0) {
    throw Exception('No se puede dividir entre cero'); // 💥 lanza un error
  }
  return a ~/ b; // ~/ es división entera
}

void main() async {
  try {
    final resultado = await dividir(10, 0);
    print('Resultado: $resultado'); // no llega aquí
  } catch (e) {
    print('Algo salió mal: $e');    // 👈 aquí cae el error
  } finally {
    print('Fin');                   // se ejecuta SIEMPRE, haya error o no
  }
}
```

Salida:

```
Algo salió mal: Exception: No se puede dividir entre cero
Fin
```

| Bloque | Cuándo se ejecuta |
|---|---|
| `try { }` | Siempre se intenta primero |
| `catch (e) { }` | Solo si algo dentro del `try` lanzó un error. `e` es el error |
| `finally { }` | Siempre al final, haya funcionado o no. **Es opcional** |
| `throw Exception('...')` | Lanza un error tú mismo |

> [!TIP]
> Regla práctica para este curso: **todo `await` que hable con internet va dentro de un `try`/`catch`**. Internet falla más seguido de lo que crees.

---

## 🔗 La otra forma: `.then()`

Vas a ver código antiguo (o en internet) que usa `.then()` en lugar de `await`:

```dart
pedirCafe().then((cafe) {
  print(cafe);
});
```

Hace lo mismo: "cuando el `Future` termine, ejecuta esta función con el resultado". En este curso **preferimos `async`/`await`** porque se lee de arriba hacia abajo, como código normal.

---

## 🔁 Comparación con Kotlin

| Kotlin (Compose) | Dart (Flutter) |
|---|---|
| `suspend fun cargar(): String` | `Future<String> cargar() async` |
| Llamar dentro de `viewModelScope.launch { }` | Llamar con `await` dentro de otra función `async` |
| `delay(2000)` | `await Future.delayed(const Duration(seconds: 2))` |
| `try { } catch (e: Exception) { }` | `try { } catch (e) { }` |
| `throw Exception("...")` | `throw Exception('...')` |

---

## ⚠️ Errores Comunes de Principiantes

1. **Olvidar `await`:** obtienes `Instance of 'Future<...>'` en lugar del dato.
2. **Usar `await` sin `async`:** error de compilación. Agrega `async` a la función que lo contiene.
3. **Declarar el tipo sin `Future`:** `String pedirCafe() async` da error; debe ser `Future<String>`.
4. **No atrapar errores:** si el `Future` falla y no hay `try`/`catch`, la app muestra un error en rojo o no hace nada.
5. **Pensar que `async` hace la función más rápida:** no la acelera; solo permite **esperar sin congelar** la app.

---

## 🏋️ Ejercicios (en [DartPad](https://dartpad.dev))

1. Crea `Future<String> descargarFoto()` que espere 3 segundos y devuelva `'foto.png'`. Llámala desde `main` e imprime el resultado.
2. Crea `Future<double> convertirSoles(double soles)` que espere 1 segundo y devuelva el valor en dólares. Si `soles` es negativo, lanza una excepción. Pruébala con `100` y con `-5`, atrapando el error.
3. Llama dos veces a `pedirCafe()` seguidas con `await` e imprime cuánto tardó en total. Pista: `DateTime.now()` antes y después.

---

_Siguiente: [MockAPI: tu propio backend de prueba →](2-mockapi.md)_
