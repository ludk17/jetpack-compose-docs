# Ejercicio 4: Lista "Talleres UPN" desde MockAPI 🎓

> Tu primera app de Flutter que **habla con un servidor**. Vas a crear tu propia API en MockAPI y mostrar sus datos en una lista que maneja los estados de **carga**, **error**, **vacío** y **éxito**.

---

## 🎯 Objetivo

La oficina de Bienestar UPN quiere una app donde los estudiantes vean los **talleres extracurriculares** del ciclo (programación, dibujo, fútbol, inglés...). Los talleres **no** se escriben en el código: viven en un servidor y la app los **pide por HTTP**.

Al terminar habrás practicado:

- Crear un proyecto y un recurso en **MockAPI** con su esquema.
- Instalar y usar el paquete **`http`** (`GET`).
- Convertir JSON en objetos con **`fromJson`**.
- Separar el código en **modelo**, **servicio** y **pantalla**.
- Cargar datos en **`initState`** y manejar los estados con **`setState`**.
- Mostrar listas con **`ListView.builder`**, **`Card`** y **`ListTile`**.
- Refrescar con **`RefreshIndicator`** y con un botón.

### 🧠 Piensa como si atendieras una ventanilla de informes

Cuando un estudiante pregunta por los talleres, la persona de informes nunca se queda callada: dice *"un momento, lo reviso"* (cargando), *"se cayó el sistema, intenta de nuevo"* (error), *"este ciclo no hay talleres"* (vacío) o le entrega la lista (éxito). **Tu pantalla debe responder igual en los cuatro casos.**

---

## 🚫 Reglas del ejercicio (léelas antes de empezar)

1. **Prohibido usar IA para generar código**: nada de ChatGPT, Gemini, Copilot, Claude, Cursor, ni el asistente de IA del editor. **Desactiva** el autocompletado con IA de Android Studio o VS Code antes de empezar.
2. Tu **única fuente** es la documentación de este repositorio (enlazada en cada paso). El autocompletado **normal** del editor (el que sugiere nombres de clases y propiedades) sí está permitido.
3. **No hay código para copiar** en este ejercicio. La documentación tiene ejemplos con **mascotas**; tú tienes que **entenderlos** y **adaptarlos** a **talleres**.
4. Debes poder **explicar cada línea** de tu código en la sustentación. Si no sabes por qué está una línea, no la pongas.
5. Lleva una **bitácora de errores** (ver [Entregable](#-entregable)): cada error que te salga, qué decía y cómo lo resolviste.

> [!TIP]
> Atascarse es parte del ejercicio. Antes de abrir una pista: **lee el mensaje de error completo**, vuelve al documento enlazado y compara tu código con el ejemplo línea por línea.

### 📚 Documentación que necesitas

| Tema | Documento |
|---|---|
| Esperar algo que tarda | [1. Future, async y await](../../docs/9.%20flutter%20http/1-future-async-await.md) |
| Crear tu API | [2. MockAPI](../../docs/9.%20flutter%20http/2-mockapi.md) |
| Hacer peticiones y leer JSON | [3. El paquete http y JSON](../../docs/9.%20flutter%20http/3-paquete-http.md) |
| Modelo y servicio | [4. Modelos y clase de servicio](../../docs/9.%20flutter%20http/4-modelos-y-servicio.md) |
| Pantalla con estados | [5. Mostrar datos en pantalla](../../docs/9.%20flutter%20http/5-mostrar-datos-en-pantalla.md) |
| Widgets visuales | [Catálogo de Widgets](../../docs/8.%20flutter/5-catalogo-de-widgets.md), [Stateless vs Stateful](../../docs/8.%20flutter/4-stateless-vs-stateful.md) |

---

## 🖼️ Así debe quedar

```
 ESTADO: ÉXITO                          ESTADO: CARGANDO
┌────────────────────────────────────┐ ┌────────────────────────────────────┐
│  Talleres UPN                   ⟳  │ │  Talleres UPN                   ⟳  │
├────────────────────────────────────┤ ├────────────────────────────────────┤
│ 12 talleres disponibles            │ │                                    │
│ ┌────────────────────────────────┐ │ │                                    │
│ │ (img) Programación en Python  🎥│ │ │                                    │
│ │       Ana Torres · Tecnología   │ │ │               ◌                    │
│ │                       25 cupos │ │ │                                    │
│ └────────────────────────────────┘ │ │                                    │
│ ┌────────────────────────────────┐ │ │                                    │
│ │ (img) Dibujo a lápiz          📍│ │ │                                    │
│ │       Luis Rojas · Arte         │ │ └────────────────────────────────────┘
│ │                    3 cupos 🔴  │ │
│ └────────────────────────────────┘ │  ESTADO: ERROR
│ ┌────────────────────────────────┐ │ ┌────────────────────────────────────┐
│ │ (img) Fútbol mixto            📍│ │ │  Talleres UPN                   ⟳  │
│ │       Carla Díaz · Deporte      │ │ ├────────────────────────────────────┤
│ │                       30 cupos │ │ │               ☁✕                   │
│ └────────────────────────────────┘ │ │  No pudimos cargar los talleres.  │
│              ...                   │ │                                    │
│     (jalar hacia abajo = recargar) │ │          [ Reintentar ]           │
└────────────────────────────────────┘ └────────────────────────────────────┘

 🎥 = virtual   📍 = presencial   🔴 = 5 cupos o menos (en rojo)
```

---

## 🪜 Paso a paso

### Paso 1: Crear tu API en MockAPI

- [ ] Crea un proyecto llamado **`talleres-upn`** con el prefijo **`/api/v1`**.
- [ ] Crea el recurso **`talleres`** con **exactamente** este esquema (borra los campos de ejemplo que no estén aquí):

| Campo | Tipo final del valor | Cómo rellenarlo (sugerencia) | Significado |
|---|---|---|---|
| `id` | texto | Object ID (automático) | Lo genera MockAPI |
| `titulo` | texto | Faker.js → algo como `commerce.productName` o `lorem.words`, o `String` | Nombre del taller |
| `instructor` | texto | Faker.js → `name.fullName` | Nombre de quien lo dicta |
| `categoria` | texto | `String` | Una de: `Tecnología`, `Arte`, `Deporte`, `Idiomas` |
| `cupos` | **número** | `Number`, o Faker.js → un generador de número | Vacantes disponibles |
| `virtual` | **booleano** | `Boolean`, o Faker.js → un generador de booleano | `true` si es virtual, `false` si es presencial |
| `imagen` | texto (URL) | Faker.js → `image.avatar` o uno de URL de imagen | URL de una imagen |

> **Nota:** El campo de ejemplo `name` (Faker.js → `name.fullName`) que trae el recurso no está en esta tabla: **bórralo**. Si no, cada taller que crees desde tu app tendrá un `name` inventado.

> [!TIP]
> Elige el relleno que quieras, pero comprueba en el navegador que el **tipo final** sea el de la tabla: `cupos` sin comillas, `virtual` como `true`/`false` e `imagen` como una URL que se abre. Los datos generados no conocen tus 4 categorías: por eso creas los 3 talleres reales con Postman.

- [ ] Genera unos **10 registros** de prueba.
- [ ] Con **Postman**, crea **3 talleres reales** (uno con `cupos` menor o igual a 5), usando las categorías de la tabla.
- [ ] Abre tu URL en el **navegador** y verifica que ves la lista.

📖 Consulta: [2. MockAPI](../../docs/9.%20flutter%20http/2-mockapi.md)

> [!IMPORTANT]
> **Punto de control:** antes de seguir, responde mirando el navegador: ¿la respuesta empieza con `[` o con `{`? ¿El `id` viene entre comillas? ¿Y `cupos`? ¿Qué valores ves en `virtual`? Tus respuestas definen los tipos de tu modelo.

<details>
<summary>💡 Pista 1: me sale <code>"cupos": "25"</code> con comillas</summary>

El tipo del campo en el esquema quedó como texto. Edita el recurso, cámbialo a número y vuelve a generar los datos.

</details>

<details>
<summary>💡 Pista 2: la URL me responde "Not found"</summary>

Compara letra por letra el nombre del recurso en MockAPI con el final de tu URL. Mayúsculas, plural y el prefijo cuentan. Revisa también que el endpoint esté en **On** en la configuración del recurso.

</details>

<details>
<summary>💡 Pista 3: los talleres que creé con Postman salen con un campo raro y sin mis datos</summary>

Revisa en Postman que el body sea **raw** y en formato **JSON**. Está explicado en el paso 5 de la documentación de MockAPI. Borra ese registro raro con `DELETE`: si lo dejas, tu app fallará al convertirlo.

</details>

---

### Paso 2: Preparar el proyecto Flutter

- [ ] Crea el proyecto `talleres_upn`.
- [ ] Instala el paquete `http`.
- [ ] Agrega el permiso de **internet** en el manifiesto de Android.
- [ ] Crea las carpetas `modelos`, `servicios` y `pantallas` dentro de `lib`.
- [ ] Deja `main.dart` con solo `runApp`, un `MaterialApp` (Material 3, sin la etiqueta *debug*, con el color que quieras) y, por ahora, un `Placeholder` como `home`.

📖 Consulta: [3. El paquete http](../../docs/9.%20flutter%20http/3-paquete-http.md), [4. Organizar los archivos](../../docs/9.%20flutter%20http/4-modelos-y-servicio.md)

<details>
<summary>💡 Pista: ¿cómo sé que el paquete se instaló bien?</summary>

Abre `pubspec.yaml` y busca `http` dentro de `dependencies`. Si está ahí, escribir el `import` del paquete en un archivo `.dart` no debería marcarse en rojo.

</details>

---

### Paso 3: El modelo `Taller`

- [ ] Crea la clase `Taller` en `modelos/taller.dart` con un atributo `final` por cada campo del esquema.
- [ ] Todos los atributos son obligatorios (parámetros nombrados con `required`).
- [ ] Agrega el constructor **`Taller.fromJson`** que recibe un `Map` y arma el objeto.
- [ ] **Prueba el modelo sin internet:** en `main`, antes de `runApp`, copia **un** taller del navegador como texto, decodifícalo, conviértelo con `fromJson` e imprime su título con `print`. Si ves el título en la consola, tu modelo funciona. Luego borra esa prueba.

📖 Consulta: [4. Modelos: fromJson](../../docs/9.%20flutter%20http/4-modelos-y-servicio.md), [Clases e Instancias](../../docs/7.%20dart/4-clases-e-instancias.md)

<details>
<summary>💡 Pista 1: ¿de qué tipo es cada atributo?</summary>

Usa tus respuestas del punto de control del paso 1. Hay **cuatro** `String`, **un** número entero, **un** booleano... y el `id`, que tiene trampa (míralo en el navegador otra vez).

</details>

<details>
<summary>💡 Pista 2: el texto JSON tiene comillas dobles y Dart se queja</summary>

Si tu `String` de Dart está entre comillas **simples** (`'...'`), las comillas **dobles** del JSON pueden ir dentro sin problema.

</details>

<details>
<summary>💡 Pista 3: <i>type 'Null' is not a subtype of type 'String'</i></summary>

Alguna clave que lees en `fromJson` no existe en el JSON. Compara **carácter por carácter** las claves de tu código con las del navegador (por ejemplo, ¿escribiste la tilde en algún nombre de campo?).

</details>

---

### Paso 4: El servicio `TallerApi`

- [ ] Crea la clase `TallerApi` en `servicios/taller_api.dart`.
- [ ] Tu URL base va en **una sola** constante privada.
- [ ] Un método `obtenerTalleres` que hace `GET` a `/talleres` y devuelve una **lista de `Taller`** (dentro de un `Future`).
- [ ] Si el código de estado **no** es el esperado, el método **lanza una excepción** con un mensaje que incluya el código.

📖 Consulta: [3. GET y JSON](../../docs/9.%20flutter%20http/3-paquete-http.md), [4. Clase de servicio](../../docs/9.%20flutter%20http/4-modelos-y-servicio.md)

<details>
<summary>💡 Pista 1: el orden de las piezas</summary>

Armar la URL → pedir y **esperar** → revisar el código → decodificar el texto → convertir **cada** elemento con `fromJson` → devolver. Si alguno falla, lanzar error.

</details>

<details>
<summary>💡 Pista 2: <i>'Iterable&lt;Taller&gt;' can't be assigned to 'List&lt;Taller&gt;'</i></summary>

Convertiste cada elemento, pero el resultado todavía no es una lista. Falta un último paso al final de la conversión.

</details>

---

### Paso 5: La pantalla y sus estados

- [ ] Crea `ListaTalleres` en `pantallas/lista_talleres.dart` y ponla como `home` en `main.dart`.
- [ ] Decide: ¿`StatelessWidget` o `StatefulWidget`? (Justifícalo en tu bitácora.)
- [ ] El estado guarda **tres** cosas: si está cargando, el mensaje de error (o nada) y la lista de talleres.
- [ ] Los talleres se piden **una sola vez** al abrir la pantalla.
- [ ] Una función de carga que: pone "cargando", pide los datos, y según el resultado guarda la lista **o** el mensaje de error. **En los dos casos** deja de cargar.
- [ ] El cuerpo del `Scaffold` muestra **una** de estas cuatro vistas:

| Estado | Qué se ve |
|---|---|
| Cargando | Un indicador circular centrado |
| Error | Ícono de nube tachada, el texto **"No pudimos cargar los talleres."** y un botón **Reintentar** que vuelve a cargar |
| Vacío | Ícono de evento y el texto **"Este ciclo aún no hay talleres"** |
| Éxito | La lista (por ahora basta con un `ListTile` que muestre solo el título) |

- [ ] La `AppBar` dice **"Talleres UPN"**.

📖 Consulta: [5. Mostrar datos en pantalla](../../docs/9.%20flutter%20http/5-mostrar-datos-en-pantalla.md)

<details>
<summary>💡 Pista 1: ¿dónde inicio la carga?</summary>

No en `build`. Hay un método del `State` que se ejecuta **una sola vez** al aparecer la pantalla. Recuerda que ese método **no** puede ser `async`.

</details>

<details>
<summary>💡 Pista 2: el círculo gira para siempre</summary>

Revisa que **después** de recibir la respuesta (o el error) cambies la variable de "cargando" **dentro** de `setState`. ¿Lo haces también en el `catch`?

</details>

<details>
<summary>💡 Pista 3: ¿cómo elijo qué vista mostrar?</summary>

Saca el cuerpo a un método aparte que devuelva un `Widget` y usa varios `if` con `return`. El **orden** de las preguntas importa: ¿qué debe revisarse primero?

</details>

<details>
<summary>💡 Pista 4: ¿qué íconos uso?</summary>

En el editor escribe `Icons.cloud_` y `Icons.event_` y deja que el autocompletado te muestre opciones.

</details>

---

### Paso 6: El diseño de cada taller

- [ ] Cada taller va dentro de una **tarjeta** con un poco de margen.
- [ ] Dentro, una fila tipo `ListTile` con:
  - **Izquierda:** la imagen del taller en un **círculo**.
  - **Título:** el título del taller en negrita.
  - **Subtítulo:** `instructor · categoría` (por ejemplo, *"Ana Torres · Tecnología"*).
  - **Derecha:** **arriba** un ícono que indique la modalidad (cámara de video si es virtual, pin de ubicación si es presencial) y **debajo** el texto `N cupos`.
- [ ] Si quedan **5 cupos o menos**, el texto de cupos se ve **en rojo y en negrita**. Si no, en gris.

📖 Consulta: [Card](../../docs/8.%20flutter/5-catalogo-de-widgets.md#card), [ListTile](../../docs/8.%20flutter/5-catalogo-de-widgets.md#listtile), [CircleAvatar](../../docs/8.%20flutter/5-catalogo-de-widgets.md#circleavatar), [TextStyle](../../docs/8.%20flutter/5-catalogo-de-widgets.md#textstyle), [ListView](../../docs/8.%20flutter/5-catalogo-de-widgets.md#listview)

<details>
<summary>💡 Pista 1: ¿cómo elijo el ícono según la modalidad?</summary>

Mira cómo el ejemplo de mascotas elige un ícono distinto según si está vacunada. Es la misma idea con otro atributo booleano.

</details>

<details>
<summary>💡 Pista 2: a la derecha necesito dos cosas, una encima de otra</summary>

Esa posición del `ListTile` recibe **un solo** widget. ¿Qué widget apila cosas **verticalmente**? Además, dile que ocupe solo el alto **mínimo**, o se saldrá del `ListTile`.

</details>

<details>
<summary>💡 Pista 3: el color de los cupos</summary>

El color se decide con una condición sobre el número de cupos. Puedes usar el operador `? :` dentro del `TextStyle`, una vez para el color y otra para el grosor.

</details>

---

### Paso 7: Contador y refrescar

- [ ] Encima de la lista, un texto **"N talleres disponibles"** con el número real de talleres.
- [ ] La lista se puede **jalar hacia abajo** para recargar.
- [ ] En la `AppBar`, a la derecha, un botón con ícono de **refrescar** que también recarga.
- [ ] **Prueba:** con la app abierta, crea un taller desde Postman. Recarga con cada uno de los dos métodos: el taller nuevo debe aparecer y el contador debe subir.

📖 Consulta: [5. RefreshIndicator](../../docs/9.%20flutter%20http/5-mostrar-datos-en-pantalla.md), [Expanded](../../docs/8.%20flutter/5-catalogo-de-widgets.md#expanded), [AppBar](../../docs/8.%20flutter/5-catalogo-de-widgets.md#appbar)

<details>
<summary>💡 Pista 1: <i>Vertical viewport was given unbounded height</i></summary>

Pusiste una lista dentro de un `Column`. La lista no sabe cuánto alto le toca. Ya conoces el widget que le dice "toma todo el espacio que sobra".

</details>

<details>
<summary>💡 Pista 2: ¿qué recibe <code>onRefresh</code>?</summary>

Una función que devuelve un `Future`. ¿Ya tienes una función así? No escribas una nueva.

</details>

---

### Paso 8: Probar los estados que "no se ven"

Los estados de error y vacío casi nunca aparecen solos. **Provócalos** para comprobar que funcionan:

- [ ] **Error:** borra una letra de tu URL base, reinicia la app y verifica que aparece la vista de error. Arregla la URL y toca **Reintentar**: debe cargar.
- [ ] **Error sin internet:** activa el **modo avión** en el emulador y recarga.
- [ ] **Vacío:** crea en MockAPI un segundo recurso **sin datos** (por ejemplo, `vacio`, con el mismo esquema) y apunta por un momento tu servicio a ese recurso. Ábrelo antes en el navegador y anota **qué responde**. Tu app debe mostrar el estado **vacío**, no el de error.
- [ ] **Cargando:** para verlo con calma, agrega temporalmente una espera de 3 segundos antes de la petición en tu servicio. Luego quítala.

📖 Consulta: [1. Future.delayed](../../docs/9.%20flutter%20http/1-future-async-await.md), [2. Filtrar](../../docs/9.%20flutter%20http/2-mockapi.md)

---

## ⚠️ Errores frecuentes

| Lo que ves | Por qué pasa | Qué revisar |
|---|---|---|
| *The argument type 'String' can't be assigned to 'Uri'* | Pasaste el texto de la URL directo | Convierte el texto en `Uri` |
| *type 'List\<dynamic\>' is not a subtype of type 'Map\<String, dynamic\>'* | Decodificaste una lista como si fuera un objeto | ¿La respuesta empieza con `[` o con `{`? |
| *type 'String' is not a subtype of type 'int'* | `cupos` llega como texto | Tipo del campo en MockAPI |
| *type 'Null' is not a subtype of type 'String'* | Una clave no existe en el JSON | Nombres de las claves en `fromJson` |
| *Instance of 'Future\<...\>'* en pantalla o consola | Falta `await` | La línea donde llamas al servicio |
| El círculo gira para siempre | No se apaga "cargando" en el `catch`, o falta `setState` | La función de carga |
| La app hace peticiones sin parar | Cargas dentro de `build` | Usa el método que corre una sola vez |
| *Vertical viewport was given unbounded height* | Lista dentro de `Column` sin límite | Envuelve la lista |
| *A RenderFlex overflowed* en la derecha del `ListTile` | El `Column` de la derecha ocupa todo el alto | Tamaño mínimo del `Column` |
| La app instalada no carga, pero en *debug* sí | Falta el permiso de internet | `AndroidManifest.xml` |

---

## ✅ Lista de verificación

- [ ] Mi recurso en MockAPI tiene los 7 campos con los tipos correctos y veo los datos en el navegador.
- [ ] Hay **tres** archivos además de `main.dart`: modelo, servicio y pantalla.
- [ ] La URL base está escrita **una sola vez**.
- [ ] `Taller` tiene `fromJson` y los tipos coinciden con el JSON.
- [ ] El servicio revisa el código de estado y lanza una excepción si no es el esperado.
- [ ] La carga empieza en el método correcto y **no** en `build`.
- [ ] Probé y vi los **cuatro** estados: cargando, error, vacío y éxito.
- [ ] **Reintentar** funciona después de un error.
- [ ] Cada taller muestra imagen circular, título, instructor · categoría, ícono de modalidad y cupos.
- [ ] Los talleres con 5 cupos o menos se ven en rojo.
- [ ] El contador muestra el número real de talleres.
- [ ] Puedo recargar jalando hacia abajo **y** con el botón de la `AppBar`.

---

## 📦 Entregable

1. El proyecto Flutter (carpeta `lib/` y `pubspec.yaml`).
2. Capturas de pantalla de los **cuatro estados**.
3. Tu **bitácora de errores**, como mínimo con 5 entradas en este formato:

| # | Mensaje de error (o síntoma) | ¿Qué lo causaba? | ¿Cómo lo resolví? | Documento que me ayudó |
|---|---|---|---|---|
| 1 | | | | |

4. La respuesta, en 2 o 3 líneas, a: **¿por qué la pantalla es Stateful y el modelo `Taller` no lo es?**

---

## 🚀 Retos extra

1. **Solo virtuales:** agrega en la `AppBar` un botón que alterne entre mostrar todos los talleres y solo los virtuales. Hazlo **pidiéndole al servidor** el filtro (parámetro en la URL), no filtrando en Dart. Tu servicio necesitará un parámetro.
2. **Ordenar por cupos:** que la lista llegue ordenada de **menos** a **más** cupos, para que los que se están acabando salgan primero. Otra vez: que lo haga el servidor.
3. **Detalle del taller:** al tocar un taller, abre una pantalla nueva que pida **ese** taller por su `id` (otro método en tu servicio) y lo muestre con la imagen grande. Esa pantalla también tiene sus estados de carga y error. 📖 [7. Navegar entre pantallas](../../docs/9.%20flutter%20http/7-navegar-entre-pantallas.md)
4. **Buscador:** agrega arriba de la lista un campo de texto con un botón de lupa. Al tocar la lupa, la lista se recarga mostrando solo los talleres que contienen ese texto (usa la búsqueda de MockAPI). Si el campo está vacío, se muestran todos. Cuando la búsqueda no encuentra nada, MockAPI responde `404` con `"Not found"` (compruébalo en el navegador). Tu app debe mostrar el estado **vacío**, no el de error. ¿Dónde conviene resolverlo: en el servicio o en la pantalla? 📖 [2. MockAPI: buscar](../../docs/9.%20flutter%20http/2-mockapi.md), [6. TextEditingController](../../docs/9.%20flutter%20http/6-formularios.md)

---

_Anterior: [← Ejercicio 3: Pantalla Biblioteca UPN](3-pantalla-biblioteca-upn.md)_ · _Siguiente: [Ejercicio 5: Formulario "Nuevo taller" →](5-formulario-talleres-mockapi.md)_
