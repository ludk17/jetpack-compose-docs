# 🔥 Introducción a Firebase Firestore

Cloud Firestore es una base de datos NoSQL flexible y escalable para el desarrollo móvil, web y de servidores de Firebase y Google Cloud.

## 🏢 La Analogía: El Almacén Infinito

Imagina que tienes un **almacén gigante** para guardar expedientes de una universidad.

1.  **Colecciones (Pasillos):** Tienes un pasillo llamado "Estudiantes". Es el lugar general donde guardas todo lo relacionado a ellos.
2.  **Documentos (Expedientes):** Dentro del pasillo "Estudiantes", cada estudiante tiene su propio **folder o expediente** físico.
3.  **Campos (Fichas):** Dentro de cada folder, hay fichas con datos: "Nombre: Juan", "Edad: 20", "Carrera: Ingeniería".

> **Nota:** A diferencia de una base de datos SQL (como Excel o MySQL) donde todos los estudiantes deben tener exactamente las mismas columnas, en Firestore un estudiante podría tener un campo "Celular" y otro no, y no pasa nada. ¡Es flexible!

---

## 📄 Conceptos Clave

| Concepto | Descripción | Equivalente SQL |
| :--- | :--- | :--- |
| **Colección** | Un contenedor de documentos. | Tabla |
| **Documento** | Una unidad de almacenamiento con datos. | Fila / Registro |
| **Campo** | Pares de llave-valor (nombre: "Juan"). | Columna |

### 🧭 Estructura de Datos
La ruta para llegar a un dato siempre sigue el patrón: `Colección > Documento > Colección > Documento...`

Ejemplo: `usuarios (coleccion) > juan123 (documento) > pedidos (coleccion) > orden_001 (documento)`

---

## 🛠️ Configuración Inicial (Resumen)

Para usar Firestore en Android, necesitamos:
1.  **Firebase Console:** Crear un proyecto y registrar nuestra app.
2.  **Archivo JSON:** Descargar `google-services.json` y ponerlo en la carpeta `app/`.
3.  **Dependencias:** Agregar el SDK de Firestore en el archivo `build.gradle`.

```kotlin
// build.gradle (Module :app)
dependencies {
    implementation("com.google.firebase:firebase-firestore-ktx:24.10.0")
}
```

---

> [!TIP]
> Firestore funciona en **tiempo real**. Si cambias un dato en la consola de Firebase, tu aplicación en el celular puede enterarse al instante sin necesidad de refrescar.

_Siguiente: [CRUD en Firestore →](2-crud-firestore.md)_
