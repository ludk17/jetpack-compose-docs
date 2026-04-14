# CLAUDE.md

Este archivo proporciona orientación a Claude Code (claude.ai/code) al trabajar con el código en este repositorio.

## Descripción del proyecto

Repositorio de **documentación en Markdown** para el curso de Android Jetpack Compose en la Universidad Privada del Norte (UPN). No hay sistema de build, código compilado ni pruebas — solo archivos `.md` que explican conceptos a principiantes.

## Estructura del repositorio

```
docs/
├── 1. fundamentos/     # Semana 1: Bases de Compose (layouts, componentes, estado)
└── 2. http/            # Semana 2+: HTTP, APIs, REST, Retrofit
```

Los archivos están numerados secuencialmente dentro de cada sección (ej. `1-modifier-column-box.md`, `2-componentes-basicos.md`). El `README.md` funciona como índice del curso y debe actualizarse al agregar nuevos archivos.

## Convenciones de contenido

**Idioma:** Todo el contenido está escrito en **español**.

**Estilo de escritura:** Los conceptos se introducen con analogías del mundo real antes de las definiciones técnicas (ej. una cafetería para explicar sync vs async). Orientado a principiantes sin experiencia previa en Android.

**Características de Markdown utilizadas:**
- Bloques de código Kotlin con ejemplos `@Composable`
- Tablas para resúmenes de referencia rápida
- Diagramas Mermaid para comparaciones visuales (`sequenceDiagram`, etc.)
- Callouts de GitHub: `> [!TIP]`, `> **Nota:**`
- Links de navegación al final: `_Siguiente: [Nombre →](siguiente-archivo.md)_`

**Agregar un nuevo documento:**
1. Nombrarlo `N-kebab-case.md` siguiendo la secuencia existente en la carpeta de la sección.
2. Agregar un link de navegación al final apuntando al siguiente archivo.
3. Actualizar `README.md` para incluir el nuevo archivo en la tabla del índice del curso.

## Ejemplos de código

Todos los fragmentos de Kotlin deben ser autocontenidos, usar anotaciones `@Composable` y compilar contra Jetpack Compose (Material3). Mantener los ejemplos mínimos — solo el composable relevante, sin código boilerplate de Activity/Application a menos que sea necesario.
