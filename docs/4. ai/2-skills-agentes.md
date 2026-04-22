# 2. Skills de los Agentes

> Las herramientas que permiten a la IA interactuar con el mundo real.

---

## ¿Qué es un Skill?

Si el Agente es el **Chef**, los **Skills** son sus **utensilios y electrodomésticos**. 

Un Agente por sí solo (el LLM) solo puede "hablar". Para que pueda "hacer", necesita Skills. Un Skill es una función específica que el Agente sabe cómo llamar cuando la necesita.

### Analogía del Mundo Real
Imagina a un carpintero:
- **Cerebro:** Sabe cómo hacer una mesa.
- **Skill (Martillo):** Capacidad de clavar.
- **Skill (Sierra):** Capacidad de cortar madera.
- **Skill (Metro):** Capacidad de medir.

Sin los skills, el carpintero sabe *teóricamente* cómo hacer la mesa, pero no tiene las herramientas para construirla.

---

## Anatomía de un Skill

Para que un Agente pueda usar un Skill, este debe tener dos partes principales:

1.  **Definición (Metadata):** El nombre y la descripción de qué hace el skill. Es lo que el Agente lee para decidir si usarlo.
2.  **Ejecución (Lógica):** El código real que se ejecuta (en Kotlin, Python, JS, etc.).

### Ejemplo conceptual:
- **Nombre:** `obtener_clima`
- **Descripción:** "Consulta el clima actual de una ciudad específica."
- **Parámetros:** `ciudad` (String).

---

## Tipos de Skills Comunes

| Categoría | Ejemplo de Skill | Función |
| :--- | :--- | :--- |
| **Búsqueda** | `google_search` | Buscar información actualizada en internet. |
| **Archivos** | `read_file` | Leer el contenido de un archivo de código. |
| **Terminal** | `run_command` | Ejecutar un comando en la consola. |
| **API** | `fetch_data` | Consultar un servicio externo (como una base de datos). |

---

## ¿Cómo elige el Agente qué Skill usar?

Es un proceso de **Coincidencia Semántica**:

1. El usuario pregunta: *"¿Qué archivos hay en la carpeta de fundamentos?"*.
2. El Agente revisa su lista de Skills.
3. Encuentra uno llamado `list_directory` con la descripción *"Lista el contenido de una carpeta"*.
4. El Agente razona: *"Para responder esta pregunta, debo usar `list_directory`"*.
5. Ejecuta el skill y obtiene los resultados.

> [!TIP]
> Una buena descripción es vital. Si la descripción de un skill es vaga, el Agente no sabrá cuándo usarlo o lo usará mal.

---

## Skills Disponibles y Personalización

No siempre tienes que empezar desde cero. Existe una gran variedad de skills ya listos para usar que puedes encontrar en el repositorio, usualmente referenciados o gestionados a través de scripts como `skills.sh`.

> [!WARNING]
> **Revisa antes de usar:** Aunque un skill ya exista, es vital que siempre revises su contenido y lo adaptes a tus necesidades específicas. Un skill diseñado para una tarea general podría no ser el más eficiente para tu proyecto particular.

---

_Siguiente: [Ejemplo: Creando un Skill →](3-creando-un-skill.md)_
