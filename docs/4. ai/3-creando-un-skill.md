# 3. Ejemplo: Creando un Skill Básico

> Paso a paso para darle una nueva "habilidad" a un Agente de IA.

---

## El Objetivo
Vamos a crear un skill llamado **"Generador de Temas"**. Este skill permitirá que el Agente nos sugiera una paleta de colores para nuestra app de Jetpack Compose basada en un concepto (ej: "Naturaleza", "Ciberpunk").

---

## Paso 1: Estructura del Skill (SKILL.md)

En lugar de simples archivos de texto, los skills modernos suelen definirse en archivos `.md` con un bloque de configuración al principio (llamado YAML frontmatter).

Para seguir el estándar de la industria, cada skill debe vivir en su propia carpeta dentro de una estructura específica:
`.agents/skills/[nombre-del-skill]/SKILL.md`

### Ejemplo de archivo SKILL.md:

```markdown
---
name: generar_paleta_colores
description: Genera una lista de 3 colores hexadecimales basados en un concepto creativo.
---

# Generador de Colores
Este skill debe recibir un concepto y devolver colores que armonicen.

## Instrucciones:
1. Analizar el concepto recibido.
2. Elegir un color primario, uno secundario y uno de acento.
3. Devolver los códigos en formato Hexadecimal.
```

---

## Paso 2: Implementar la Lógica

Aquí es donde escribimos el código que realmente hace el trabajo. En nuestro caso, simularemos una función que devuelve colores.

```kotlin
// Representación de nuestra lógica en Kotlin
fun generarPaletaColores(concepto: String): List<String> {
    return when (concepto.lowercase()) {
        "naturaleza" -> listOf("#2E7D32", "#81C784", "#C8E6C9")
        "ciberpunk" -> listOf("#F50057", "#00E5FF", "#651FFF")
        else -> listOf("#6200EE", "#03DAC6", "#3700B3")
    }
}
```

---

## Paso 3: El Agente en Acción

Cuando el usuario interactúa con el Agente, ocurre lo siguiente:

1.  **Usuario:** *"Oye, quiero hacer una app sobre el bosque, ¿qué colores me sugieres?"*
2.  **Agente (Razonamiento):** *"El usuario quiere colores para un tema de bosque. Tengo el skill `generar_paleta_colores` que acepta un concepto. Usaré 'naturaleza' como parámetro."*
3.  **Ejecución:** El sistema llama a la función y obtiene: `["#2E7D32", "#81C784", "#C8E6C9"]`.
4.  **Respuesta Final:** *"Para un tema de bosque, te sugiero estos colores: Verde Pino (#2E7D32), Verde Hoja (#81C784) y Verde Menta (#C8E6C9)."*

---

## 🛠️ Ejercicio Práctico

Imagina que quieres crear un skill que guarde notas de voz en tu app de Android.

**Estructura del archivo `SKILL.md`:**
```markdown
---
name: guardar_nota_voz
description: Almacena un recordatorio rápido a partir de una transcripción de audio.
---
# Guardado de Notas
Recibe el texto de la nota y una categoría opcional. 
Guarda el archivo en el almacenamiento local de la app.
```

---

> **Nota:** En herramientas reales como **Antigravity** o **Claude Code**, los skills son los que nos permiten leer tus archivos, ejecutar tus tests y ayudarte a programar mejor.

---

_Volver al inicio: [README.md](../../README.md)_
