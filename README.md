# 📱 Jetpack Compose Docs

Repositorio de referencia para la clase de **Jetpack Compose**. Cada semana se subirá material nuevo con explicaciones y ejemplos prácticos, pensado para alguien que empieza desde cero.

---

## ¿Qué es Jetpack Compose?

Jetpack Compose es el toolkit moderno de Android para construir interfaces de usuario con **Kotlin**, sin necesidad de XML. Permite describir la UI de forma declarativa, es decir: describes *cómo debe verse* y Compose se encarga del resto.

---

## Contenido del curso

### 📂 1. Fundamentos
> Semana 1 — Bases del lenguaje visual de Compose

| Archivo | Temas |
|---|---|
| [Modifier, Column y Box](docs/1.%20fundamentos/1-modifier-column-box.md) | Layouts básicos, Modifier, apilamiento |
| [Componentes básicos](docs/1.%20fundamentos/2-componentes-basicos.md) | Text, Image, Icon, Button, Scaffold, LazyColumn |
| [Formularios](docs/1.%20fundamentos/3-componentes-formulario.md) | TextField, Checkbox, Slider, Switch, RadioButton |
| [Estado y Remember](docs/1.%20fundamentos/4-estados-remember.md) | mutableStateOf, remember, StateHoisting |

### 📂 2. HTTP y Networking
> Semana 2+ — Consumo de datos desde la web

| Archivo | Temas |
|---|---|
| [Protocolo HTTP](docs/2.%20http/1-que-es-http.md) | Conceptos, Petición, Respuesta, Status Codes |
| [¿Qué es una API?](docs/2.%20http/2-que-es-una-api.md) | Clientes, Servidores, Endpoints |
| [REST y JSON](docs/2.%20http/3-rest-y-json.md) | Formato JSON, Verbos REST (GET, POST, etc.) |
| [Partes de una petición](docs/2.%20http/4-partes-peticion-http.md) | URL, Headers, Body, Parameters |
| [Síncrono vs Asíncrono](docs/2.%20http/5-sync-vs-async.md) | Bloqueo de hilos, Corrutinas básicas |
| [Retrofit](docs/2.%20http/6-retrofit.md) | Configuración, Interfaz, Client, Implementation |
| ⏳ Retrofit Avanzado | Headers, Tokens de autenticación, Interceptores |
| [ViewModel](docs/2.%20http/7-viewmodel.md) | Separar lógica de la UI, lifecycle, viewModelScope |
| ⏳ Estados de carga | Loading, Error, Success con sealed classes |
| ⏳ Navegación | NavController, rutas, paso de parámetros entre pantallas |

---

> 🔄 *Este repositorio se actualiza semanalmente con el material de cada clase.*

---

## Requisitos previos

- Android Studio (versión Hedgehog o superior)
- Conocimientos básicos de Kotlin
- Un proyecto Android con Jetpack Compose configurado

## Cómo usar este repositorio

1. Clona el repositorio:
   ```bash
   git clone https://github.com/ludk17/jetpack-compose-docs.git
   ```
2. Navega a la carpeta `docs/` y abre el tema de la semana.
3. Copia los ejemplos en Android Studio y pruébalos.

---

## Estructura del proyecto

```
jetpack-compose-docs/
└── docs/
    ├── 1. fundamentos/
    │   ├── 1-modifier-column-box.md
    │   ├── 2-componentes-basicos.md
    │   ├── 3-componentes-formulario.md
    │   └── 4-estados-remember.md
    └── 2. http/
        ├── 1-que-es-http.md
        ├── 2-que-es-una-api.md
        ├── 3-rest-y-json.md
        ├── 4-partes-peticion-http.md
        ├── 5-sync-vs-async.md
        ├── 6-retrofit.md
        └── 7-viewmodel.md
```

---

*Universidad Privada del Norte — Curso de Desarrollo Android*
