# Grupo B · Ejercicio 1: Lista "Objetos Perdidos UPN" desde MockAPI 🔎

> Tu primera app de Flutter que **habla con un servidor**. Vas a crear tu propia API en MockAPI y mostrar sus datos en una lista que maneja los estados de **carga**, **error**, **vacío** y **éxito**.

---

## 🎯 Objetivo

La oficina de Bienestar UPN guarda los **objetos perdidos** que se encuentran en el campus: casacas, botellas, cargadores, calculadoras... Quiere una app donde los estudiantes revisen si su objeto apareció. Los objetos **no** se escriben en el código: viven en un servidor y la app los **pide por HTTP**.

Al terminar habrás practicado:

- Crear un proyecto y un recurso en **MockAPI** con su esquema.
- Instalar y usar el paquete **`http`** (`GET`).
- Convertir JSON en objetos con **`fromJson`**.
- Separar el código en **modelo**, **servicio** y **pantalla**.
- Cargar datos en **`initState`** y manejar los estados con **`setState`**.
- Mostrar listas con **`ListView.builder`**, **`Card`** y **`ListTile`**.
- Refrescar con **`RefreshIndicator`** y con un botón.

### 🧠 Piensa como si atendieras la oficina de objetos perdidos

Cuando alguien pregunta *"¿encontraron mi casaca?"*, quien atiende nunca se queda callado: dice *"déjame revisar"* (cargando), *"no tengo acceso al registro ahora, vuelve a intentar"* (error), *"no hay nada reportado"* (vacío) o le muestra lo que hay (éxito). **Tu pantalla debe responder igual en los cuatro casos.**

---

## 🚫 Reglas del ejercicio (léelas antes de empezar)

1. **Prohibido usar IA para generar código**: nada de ChatGPT, Gemini, Copilot, Claude, Cursor, ni el asistente de IA del editor. **Desactiva** el autocompletado con IA de Android Studio o VS Code antes de empezar.
2. Tu **única fuente** es la documentación de este repositorio (enlazada en cada paso). El autocompletado **normal** del editor (el que sugiere nombres de clases y propiedades) sí está permitido.
3. **No hay código para copiar** en este ejercicio. La documentación tiene ejemplos con **mascotas**; tú tienes que **entenderlos** y **adaptarlos** a **objetos perdidos**.
4. Debes poder **explicar cada línea** de tu código en la sustentación. Si no sabes por qué está una línea, no la pongas.
5. Lleva una **bitácora de errores** (ver [Entregable](#-entregable)): cada error que te salga, qué decía y cómo lo resolviste.

> [!TIP]
> Atascarse es parte del ejercicio. Antes de abrir una pista: **lee el mensaje de error completo**, vuelve al documento enlazado y compara tu código con el ejemplo línea por línea.

### 📚 Documentación que necesitas

| Tema | Documento |
|---|---|
| Esperar algo que tarda | [1. Future, async y await](../../../docs/9.%20flutter%20http/1-future-async-await.md) |
| Crear tu API | [2. MockAPI](../../../docs/9.%20flutter%20http/2-mockapi.md) |
| Hacer peticiones y leer JSON | [3. El paquete http y JSON](../../../docs/9.%20flutter%20http/3-paquete-http.md) |
| Modelo y servicio | [4. Modelos y clase de servicio](../../../docs/9.%20flutter%20http/4-modelos-y-servicio.md) |
| Pantalla con estados | [5. Mostrar datos en pantalla](../../../docs/9.%20flutter%20http/5-mostrar-datos-en-pantalla.md) |
| Widgets visuales | [Catálogo de Widgets](../../../docs/8.%20flutter/5-catalogo-de-widgets.md), [Stateless vs Stateful](../../../docs/8.%20flutter/4-stateless-vs-stateful.md) |

---

## 🖼️ Así debe quedar

```
 ESTADO: ÉXITO                          ESTADO: CARGANDO
┌────────────────────────────────────┐ ┌────────────────────────────────────┐
│  Objetos Perdidos UPN           ⟳  │ │  Objetos Perdidos UPN           ⟳  │
├────────────────────────────────────┤ ├────────────────────────────────────┤
│ 14 objetos reportados              │ │                                    │
│ ┌────────────────────────────────┐ │ │                                    │
│ │ (img) Casaca azul             ✅│ │ │                                    │
│ │       Biblioteca · Ana Torres   │ │ │               ◌                    │
│ │                     hace 2 días│ │ │                                    │
│ └────────────────────────────────┘ │ │                                    │
│ ┌────────────────────────────────┐ │ │                                    │
│ │ (img) Calculadora Casio       ❓│ │ │                                    │
│ │       Laboratorio · Luis Rojas  │ │ └────────────────────────────────────┘
│ │                 hace 41 días 🔴│ │
│ └────────────────────────────────┘ │  ESTADO: ERROR
│ ┌────────────────────────────────┐ │ ┌────────────────────────────────────┐
│ │ (img) Botella de metal        ❓│ │ │  Objetos Perdidos UPN           ⟳  │
│ │       Cafetería · Carla Díaz    │ │ ├────────────────────────────────────┤
│ │                     hace 5 días│ │ │               ☁✕                   │
│ └────────────────────────────────┘ │ │  No pudimos cargar los objetos.   │
│              ...                   │ │                                    │
│     (jalar hacia abajo = recargar) │ │          [ Reintentar ]           │
└────────────────────────────────────┘ └────────────────────────────────────┘

 ✅ = ya devuelto a su dueño   ❓ = sigue sin dueño
 🔴 = sin dueño y con 30 días o más (en rojo: pronto se donará)
```

---

## 🪜 Paso a paso

### Paso 1: Crear tu API en MockAPI

- [ ] Crea un proyecto llamado **`objetos-perdidos-upn`** con el prefijo **`/api/v1`**.
- [ ] Crea el recurso **`objetos`** con **exactamente** este esquema (borra los campos de ejemplo que no estén aquí):

| Campo | Tipo final del valor | Cómo rellenarlo (sugerencia) | Significado |
|---|---|---|---|
| `id` | texto | Object ID (automático) | Lo genera MockAPI |
| `nombre` | texto | Faker.js → algo como `commerce.productName`, o `String` | Qué objeto es |
| `lugar` | texto | `String` | Dónde se encontró. Uno de: `Biblioteca`, `Cafetería`, `Laboratorio`, `Patio` |
| `reportadoPor` | texto | Faker.js → `name.fullName` | Quién lo entregó en la oficina |
| `dias` | **número** | `Number`, o Faker.js → un generador de número | Días que lleva en la oficina |
| `devuelto` | **booleano** | `Boolean`, o Faker.js → un generador de booleano | `true` si ya volvió con su dueño |
| `foto` | texto (URL) | Faker.js → `image.avatar` o uno de URL de imagen | URL de una imagen del objeto |

> [!TIP]
> Elige el relleno que quieras, pero comprueba en el navegador que el **tipo final** sea el de la tabla: `dias` sin comillas, `devuelto` como `true`/`false` y `foto` como una URL que se abre. Los datos generados no conocen tus 4 lugares: por eso creas los objetos reales con Postman.

> **Nota:** El campo de ejemplo `name` (Faker.js → `name.fullName`) que trae el recurso no está en esta tabla: **bórralo**. Si no, cada objeto que crees desde tu app tendrá un `name` inventado. Y fíjate que `reportadoPor` lleva una **P mayúscula** en medio: así debe escribirse en todos lados.

- [ ] Genera unos **10 registros** de prueba.
- [ ] Con **Postman**, crea **3 objetos reales** usando los lugares de la tabla: uno **devuelto**, uno **sin devolver con menos de 30 días** y uno **sin devolver con 30 días o más**.
- [ ] Abre tu URL en el **navegador** y verifica que ves la lista.

📖 Consulta: [2. MockAPI](../../../docs/9.%20flutter%20http/2-mockapi.md)

> [!IMPORTANT]
> **Punto de control:** antes de seguir, responde mirando el navegador: ¿la respuesta empieza con `[` o con `{`? ¿El `id` viene entre comillas? ¿Y `dias`? ¿Qué valores ves en `devuelto`? Tus respuestas definen los tipos de tu modelo.

<details>
<summary>💡 Pista 1: me sale <code>"dias": "12"</code> con comillas</summary>

El tipo del campo en el esquema quedó como texto. Edita el recurso, cámbialo a número y vuelve a generar los datos. Si el registro lo creaste tú con Postman, revisa que escribiste el número **sin** comillas.

</details>

<details>
<summary>💡 Pista 2: la URL me responde "Not found"</summary>

Compara letra por letra el nombre del recurso en MockAPI con el final de tu URL. Mayúsculas, plural y el prefijo cuentan. Revisa también que el endpoint esté en **On** en la configuración del recurso.

</details>

<details>
<summary>💡 Pista 3: los objetos que creé con Postman salen con un campo raro y sin mis datos</summary>

Revisa en Postman que el body sea **raw** y en formato **JSON**. Está explicado en el paso 5 de la documentación de MockAPI. Borra ese registro raro con `DELETE`: si lo dejas, tu app fallará al convertirlo.

</details>

---

### Paso 2: Preparar el proyecto Flutter

- [ ] Crea el proyecto `objetos_perdidos_upn`.
- [ ] Instala el paquete `http`.
- [ ] Agrega el permiso de **internet** en el manifiesto de Android.
- [ ] Crea las carpetas `modelos`, `servicios` y `pantallas` dentro de `lib`.
- [ ] Deja `main.dart` con solo `runApp`, un `MaterialApp` (Material 3, sin la etiqueta *debug*, con el color que quieras) y, por ahora, un `Placeholder` como `home`.

📖 Consulta: [3. El paquete http](../../../docs/9.%20flutter%20http/3-paquete-http.md), [4. Organizar los archivos](../../../docs/9.%20flutter%20http/4-modelos-y-servicio.md)

<details>
<summary>💡 Pista: ¿cómo sé que el paquete se instaló bien?</summary>

Abre `pubspec.yaml` y busca `http` dentro de `dependencies`. Si está ahí, escribir el `import` del paquete en un archivo `.dart` no debería marcarse en rojo.

</details>

---

### Paso 3: El modelo `ObjetoPerdido`

- [ ] Crea la clase `ObjetoPerdido` en `modelos/objeto_perdido.dart` con un atributo `final` por cada campo del esquema.
- [ ] Todos los atributos son obligatorios (parámetros nombrados con `required`).
- [ ] Agrega el constructor **`ObjetoPerdido.fromJson`** que recibe un `Map` y arma el objeto.
- [ ] **Prueba el modelo sin internet:** en `main`, antes de `runApp`, copia **un** objeto del navegador como texto, decodifícalo, conviértelo con `fromJson` e imprime su nombre con `print`. Si ves el nombre en la consola, tu modelo funciona. Luego borra esa prueba.

📖 Consulta: [4. Modelos: fromJson](../../../docs/9.%20flutter%20http/4-modelos-y-servicio.md), [Clases e Instancias](../../../docs/7.%20dart/4-clases-e-instancias.md)

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

Alguna clave que lees en `fromJson` no existe en el JSON. Compara **carácter por carácter** las claves de tu código con las del navegador. Sospechoso número uno: la **P** de `reportadoPor`.

</details>

<details>
<summary>💡 Pista 4: ¿por qué la clase no se llama <code>Objeto</code>?</summary>

Podría, pero `Object` (en inglés) es una clase muy importante de Dart y es fácil confundirlas al leer. Un nombre más específico evita errores.

</details>

---

### Paso 4: El servicio `ObjetoPerdidoApi`

- [ ] Crea la clase `ObjetoPerdidoApi` en `servicios/objeto_perdido_api.dart`.
- [ ] Tu URL base va en **una sola** constante privada.
- [ ] Un método `obtenerObjetos` que hace `GET` a `/objetos` y devuelve una **lista de `ObjetoPerdido`** (dentro de un `Future`).
- [ ] Si el código de estado **no** es el esperado, el método **lanza una excepción** con un mensaje que incluya el código.

📖 Consulta: [3. GET y JSON](../../../docs/9.%20flutter%20http/3-paquete-http.md), [4. Clase de servicio](../../../docs/9.%20flutter%20http/4-modelos-y-servicio.md)

<details>
<summary>💡 Pista 1: el orden de las piezas</summary>

Armar la URL → pedir y **esperar** → revisar el código → decodificar el texto → convertir **cada** elemento con `fromJson` → devolver. Si alguno falla, lanzar error.

</details>

<details>
<summary>💡 Pista 2: <i>'Iterable&lt;ObjetoPerdido&gt;' can't be assigned to 'List&lt;ObjetoPerdido&gt;'</i></summary>

Convertiste cada elemento, pero el resultado todavía no es una lista. Falta un último paso al final de la conversión.

</details>

---

### Paso 5: La pantalla y sus estados

- [ ] Crea `ListaObjetos` en `pantallas/lista_objetos.dart` y ponla como `home` en `main.dart`.
- [ ] Decide: ¿`StatelessWidget` o `StatefulWidget`? (Justifícalo en tu bitácora.)
- [ ] El estado guarda **tres** cosas: si está cargando, el mensaje de error (o nada) y la lista de objetos.
- [ ] Los objetos se piden **una sola vez** al abrir la pantalla.
- [ ] Una función de carga que: pone "cargando", pide los datos, y según el resultado guarda la lista **o** el mensaje de error. **En los dos casos** deja de cargar.
- [ ] El cuerpo del `Scaffold` muestra **una** de estas cuatro vistas:

| Estado | Qué se ve |
|---|---|
| Cargando | Un indicador circular centrado |
| Error | Ícono de nube tachada, el texto **"No pudimos cargar los objetos."** y un botón **Reintentar** que vuelve a cargar |
| Vacío | Ícono de caja o inventario y el texto **"No hay objetos reportados 🎉"** |
| Éxito | La lista (por ahora basta con un `ListTile` que muestre solo el nombre) |

- [ ] La `AppBar` dice **"Objetos Perdidos UPN"**.

📖 Consulta: [5. Mostrar datos en pantalla](../../../docs/9.%20flutter%20http/5-mostrar-datos-en-pantalla.md)

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

En el editor escribe `Icons.cloud_` y `Icons.inventory` y deja que el autocompletado te muestre opciones.

</details>

---

### Paso 6: El diseño de cada objeto

- [ ] Cada objeto va dentro de una **tarjeta** con un poco de margen.
- [ ] Dentro, una fila tipo `ListTile` con:
  - **Izquierda:** la foto del objeto en un **círculo**.
  - **Título:** el nombre del objeto en negrita.
  - **Subtítulo:** `lugar · reportado por` (por ejemplo, *"Biblioteca · Ana Torres"*).
  - **Derecha:** **arriba** un ícono que indique si ya fue devuelto (check verde si sí, signo de pregunta naranja si no) y **debajo** el texto `hace N días`.
- [ ] El texto de los días se ve **en rojo y en negrita** solo si el objeto **no** ha sido devuelto **y** lleva **30 días o más**. En cualquier otro caso, en gris.
- [ ] Si `dias` es `1`, debe decir **"hace 1 día"** (singular), no "hace 1 días".

📖 Consulta: [Card](../../../docs/8.%20flutter/5-catalogo-de-widgets.md#card), [ListTile](../../../docs/8.%20flutter/5-catalogo-de-widgets.md#listtile), [CircleAvatar](../../../docs/8.%20flutter/5-catalogo-de-widgets.md#circleavatar), [TextStyle](../../../docs/8.%20flutter/5-catalogo-de-widgets.md#textstyle), [ListView](../../../docs/8.%20flutter/5-catalogo-de-widgets.md#listview)

<details>
<summary>💡 Pista 1: ¿cómo elijo el ícono y su color?</summary>

Mira cómo el ejemplo de mascotas elige un ícono y un color distintos según si está vacunada. Es la misma idea con otro atributo booleano.

</details>

<details>
<summary>💡 Pista 2: a la derecha necesito dos cosas, una encima de otra</summary>

Esa posición del `ListTile` recibe **un solo** widget. ¿Qué widget apila cosas **verticalmente**? Además, dile que ocupe solo el alto **mínimo**, o se saldrá del `ListTile`.

</details>

<details>
<summary>💡 Pista 3: la condición del color rojo tiene dos partes</summary>

Las dos cosas deben cumplirse **a la vez**: "no devuelto" **y** "30 días o más". En Dart, "y" se escribe `&&` y "no" se escribe `!`. Te conviene guardar el resultado en una variable `bool` con un nombre claro (por ejemplo, `porDonar`) y usarla para el color y el grosor.

</details>

<details>
<summary>💡 Pista 4: "día" o "días"</summary>

Otra condición con `? :`, esta vez dentro de la interpolación del texto o en una variable aparte. Pruébala creando con Postman un objeto con `dias` igual a `1`.

</details>

---

### Paso 7: Contador y refrescar

- [ ] Encima de la lista, un texto **"N objetos reportados"** con el número real de objetos.
- [ ] La lista se puede **jalar hacia abajo** para recargar.
- [ ] En la `AppBar`, a la derecha, un botón con ícono de **refrescar** que también recarga.
- [ ] **Prueba:** con la app abierta, crea un objeto desde Postman. Recarga con cada uno de los dos métodos: el objeto nuevo debe aparecer y el contador debe subir.

📖 Consulta: [5. RefreshIndicator](../../../docs/9.%20flutter%20http/5-mostrar-datos-en-pantalla.md), [Expanded](../../../docs/8.%20flutter/5-catalogo-de-widgets.md#expanded), [AppBar](../../../docs/8.%20flutter/5-catalogo-de-widgets.md#appbar)

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

📖 Consulta: [1. Future.delayed](../../../docs/9.%20flutter%20http/1-future-async-await.md), [2. MockAPI](../../../docs/9.%20flutter%20http/2-mockapi.md)

---

## ⚠️ Errores frecuentes

| Lo que ves | Por qué pasa | Qué revisar |
|---|---|---|
| *The argument type 'String' can't be assigned to 'Uri'* | Pasaste el texto de la URL directo | Convierte el texto en `Uri` |
| *type 'List\<dynamic\>' is not a subtype of type 'Map\<String, dynamic\>'* | Decodificaste una lista como si fuera un objeto | ¿La respuesta empieza con `[` o con `{`? |
| *type 'String' is not a subtype of type 'int'* | `dias` llega como texto | Tipo del campo en MockAPI o el registro que creaste con Postman |
| *type 'Null' is not a subtype of type 'String'* | Una clave no existe en el JSON | Nombres de las claves en `fromJson` (¡`reportadoPor`!) |
| *Instance of 'Future\<...\>'* en pantalla o consola | Falta `await` | La línea donde llamas al servicio |
| El círculo gira para siempre | No se apaga "cargando" en el `catch`, o falta `setState` | La función de carga |
| La app hace peticiones sin parar | Cargas dentro de `build` | Usa el método que corre una sola vez |
| *Vertical viewport was given unbounded height* | Lista dentro de `Column` sin límite | Envuelve la lista |
| *A RenderFlex overflowed* en la derecha del `ListTile` | El `Column` de la derecha ocupa todo el alto | Tamaño mínimo del `Column` |
| Un objeto devuelto con 40 días sale en rojo | La condición solo revisa los días | Las **dos** partes de la condición |
| La app instalada no carga, pero en *debug* sí | Falta el permiso de internet | `AndroidManifest.xml` |

---

## ✅ Lista de verificación

- [ ] Mi recurso en MockAPI tiene los 7 campos con los tipos correctos y veo los datos en el navegador.
- [ ] Hay **tres** archivos además de `main.dart`: modelo, servicio y pantalla.
- [ ] La URL base está escrita **una sola vez**.
- [ ] `ObjetoPerdido` tiene `fromJson` y los tipos coinciden con el JSON.
- [ ] El servicio revisa el código de estado y lanza una excepción si no es el esperado.
- [ ] La carga empieza en el método correcto y **no** en `build`.
- [ ] Probé y vi los **cuatro** estados: cargando, error, vacío y éxito.
- [ ] **Reintentar** funciona después de un error.
- [ ] Cada objeto muestra foto circular, nombre, lugar · reportado por, ícono de devuelto y días.
- [ ] Solo los objetos **sin devolver** con **30 días o más** se ven en rojo.
- [ ] Con `dias` igual a 1 dice "hace 1 día".
- [ ] El contador muestra el número real de objetos.
- [ ] Puedo recargar jalando hacia abajo **y** con el botón de la `AppBar`.

---

## 📦 Entregable

1. El proyecto Flutter (carpeta `lib/` y `pubspec.yaml`).
2. Capturas de pantalla de los **cuatro estados**. En la de éxito deben verse los 3 objetos que creaste con Postman.
3. Tu **bitácora de errores**, como mínimo con 5 entradas en este formato:

| # | Mensaje de error (o síntoma) | ¿Qué lo causaba? | ¿Cómo lo resolví? | Documento que me ayudó |
|---|---|---|---|---|
| 1 | | | | |

4. La respuesta, en 2 o 3 líneas, a: **¿por qué la pantalla es Stateful y el modelo `ObjetoPerdido` no lo es?**

---

## 🚀 Retos extra

1. **Solo sin dueño:** agrega en la `AppBar` un botón que alterne entre mostrar todos los objetos y solo los que **no** han sido devueltos. Hazlo **pidiéndole al servidor** el filtro (parámetro en la URL), no filtrando en Dart. Tu servicio necesitará un parámetro.
2. **Los más antiguos primero:** que la lista llegue ordenada de **más** a **menos** días, para que los que están por donarse salgan arriba. Otra vez: que lo haga el servidor.
3. **Detalle del objeto:** al tocar un objeto, abre una pantalla nueva que pida **ese** objeto por su `id` (otro método en tu servicio) y lo muestre con la foto grande. Esa pantalla también tiene sus estados de carga y error. 📖 [7. Navegar entre pantallas](../../../docs/9.%20flutter%20http/7-navegar-entre-pantallas.md)
4. **Buscador:** agrega arriba de la lista un campo de texto con un botón de lupa. Al tocar la lupa, la lista se recarga mostrando solo los objetos que contienen ese texto (usa la búsqueda de MockAPI). Si el campo está vacío, se muestran todos. Cuando la búsqueda no encuentra nada, MockAPI responde `404` con `"Not found"` (compruébalo en el navegador). Tu app debe mostrar el estado **vacío**, no el de error. ¿Dónde conviene resolverlo: en el servicio o en la pantalla? 📖 [2. MockAPI: buscar](../../../docs/9.%20flutter%20http/2-mockapi.md), [6. TextEditingController](../../../docs/9.%20flutter%20http/6-formularios.md)

---

_Siguiente: [Grupo B · Ejercicio 2: Formulario "Reportar objeto" →](2-formulario-objetos-perdidos-mockapi.md)_
