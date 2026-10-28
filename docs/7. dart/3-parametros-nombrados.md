# 3. Funciones con Parámetros Nombrados

> Los parámetros nombrados son la característica de Dart que más vas a usar en Flutter: **cada widget** los utiliza.

---

## 🍔 La analogía: pedir una hamburguesa

Imagina dos formas de pedir en un restaurante:

- **Por posición:** *"Una hamburguesa, sí, no, doble, sin."* 🤔 El mesero tiene que adivinar qué significa cada palabra según el orden.
- **Por nombre:** *"Una hamburguesa con **queso:** sí, **cebolla:** no, **carne:** doble, **salsa:** sin."* 😎 Imposible confundirse, y el orden da igual.

Los parámetros nombrados son la segunda forma: **cada valor viaja con su etiqueta**.

```dart
// ❓ ¿Qué significa cada valor?
crearUsuario('Ana', 20, true, false);

// ✅ Se entiende solo
crearUsuario(nombre: 'Ana', edad: 20, esAdmin: true, verificado: false);
```

---

## 📝 Declaración: llaves `{ }`

Los parámetros nombrados se declaran **entre llaves** `{ }`, y al llamar la función se escribe `nombre: valor`.

```dart
void saludar({String nombre = 'invitado', String saludo = 'Hola'}) {
  print('$saludo, $nombre');
}

void main() {
  saludar();                                  // Hola, invitado
  saludar(nombre: 'Ana');                     // Hola, Ana
  saludar(saludo: 'Buenos días');             // Buenos días, invitado
  saludar(saludo: 'Buenas', nombre: 'Luis');  // Buenas, Luis  → el orden no importa
}
```

> **Nota:** Por defecto, los parámetros nombrados son **opcionales**. Por eso cada uno necesita un **valor por defecto**, ser **anulable** (`?`), o ser marcado como **`required`**.

---

## 🔒 Las tres formas de declarar un parámetro nombrado

### 1. Con valor por defecto

Si no se envía, toma ese valor.

```dart
void configurar({int volumen = 50}) {
  print('Volumen: $volumen');
}

configurar();             // Volumen: 50
configurar(volumen: 80);  // Volumen: 80
```

### 2. Anulable `?`

Si no se envía, vale `null`.

```dart
void mostrarPerfil({String? bio}) {
  print(bio ?? 'Este usuario no tiene biografía');
}

mostrarPerfil();                       // Este usuario no tiene biografía
mostrarPerfil(bio: 'Amo programar');  // Amo programar
```

### 3. Obligatorio: `required`

Se **debe** enviar siempre; si no, el código no compila.

```dart
void registrar({required String correo, required String clave}) {
  print('Registrando $correo');
}

void main() {
  registrar(correo: 'ana@upn.pe', clave: '1234'); // ✅
  // registrar(correo: 'ana@upn.pe');             // ❌ Error: falta 'clave'
}
```

### Resumen

| Declaración | ¿Obligatorio? | Si no se envía... |
|---|---|---|
| `{int volumen = 50}` | ❌ No | Usa `50` |
| `{String? bio}` | ❌ No | Vale `null` |
| `{required String correo}` | ✅ Sí | Error de compilación |
| `{String correo}` | — | ❌ **Error:** no es anulable ni tiene valor por defecto |

---

## 🔀 Combinar posicionales y nombrados

Puedes mezclar ambos: los **posicionales van primero** y los **nombrados al final, dentro de llaves**.

```dart
double calcularPrecio(
  double base, {
  double descuento = 0,
  bool incluirIgv = true,
}) {
  var total = base - (base * descuento);
  if (incluirIgv) {
    total = total * 1.18;
  }
  return total;
}

void main() {
  print(calcularPrecio(100));                                    // 118.0
  print(calcularPrecio(100, descuento: 0.5));                    // 59.0
  print(calcularPrecio(100, incluirIgv: false));                 // 100.0
  print(calcularPrecio(100, incluirIgv: false, descuento: 0.5)); // 50.0
}
```

> [!TIP]
> **Regla práctica:** usa posicionales para el dato **principal y obvio** (como el texto de un `Text`), y nombrados para todo lo demás (configuraciones, opciones, callbacks).

> [!WARNING]
> En una misma función **no puedes** combinar posicionales opcionales `[ ]` con nombrados `{ }`. Elige uno de los dos.

---

## 🆚 Comparación con Kotlin

En Kotlin **cualquier** parámetro puede pasarse por nombre si quien llama lo decide. En Dart, **quien declara la función** decide cuáles son nombrados usando `{ }`.

```kotlin
// Kotlin
fun crearMensaje(texto: String, autor: String = "Anónimo", urgente: Boolean = false) { }

crearMensaje("Hola", urgente = true)          // nombrado opcional
crearMensaje(texto = "Hola", autor = "Ana")   // incluso el primero
```

```dart
// Dart
void crearMensaje(String texto, {String autor = 'Anónimo', bool urgente = false}) {}

crearMensaje('Hola', urgente: true);          // ✅
// crearMensaje(texto: 'Hola');               // ❌ texto es posicional, no tiene nombre
```

| Concepto | Kotlin | Dart |
|---|---|---|
| Pasar por nombre | `autor = "Ana"` | `autor: 'Ana'` |
| ¿Quién decide si es nombrado? | Quien **llama** | Quien **declara** (`{ }`) |
| Obligatorio | Sin valor por defecto | `required` |
| Opcional | Con valor por defecto | Valor por defecto o `?` |

---

## 📱 ¿Por qué es vital en Flutter?

Así se ve un widget de Flutter. **Todo** son parámetros nombrados (salvo el texto de `Text`, que es posicional):

```dart
Container(
  width: 200,
  padding: const EdgeInsets.all(16),
  color: Colors.blue,
  child: const Text(
    'Hola Flutter',
    style: TextStyle(fontSize: 20, color: Colors.white),
  ),
)
```

Compáralo con Compose, donde también usamos argumentos con nombre:

```kotlin
Text(
    text = "Hola Compose",
    fontSize = 20.sp,
    color = Color.White,
    modifier = Modifier.padding(16.dp)
)
```

Como estos widgets tienen **decenas** de opciones, los parámetros nombrados permiten enviar solo las que necesitas, en cualquier orden.

---

## 🏋️ Ejercicios

1. Crea una función `crearAlarma` con los parámetros nombrados `hora` (**obligatorio**, `String`), `repetir` (`bool`, por defecto `false`) y `etiqueta` (`String?`). Debe imprimir algo como: `Alarma 07:00 - Clase (repite: true)`. Si no hay etiqueta, mostrar `Sin etiqueta`.
2. Crea una función `area` que reciba `base` y `altura` como **nombrados obligatorios** y devuelva el área de un triángulo usando `=>`.
3. ¿Por qué esta declaración no compila? ¿Cómo la arreglarías de tres formas distintas?
   ```dart
   void enviar({String destino}) {}
   ```

---

_Siguiente: [Clases e Instancias →](4-clases-e-instancias.md)_
