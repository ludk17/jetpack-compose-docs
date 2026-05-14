# 💾 Jetpack DataStore

Hay datos pequeños que no pertenecen a Firestore — preferencias del usuario, el tema oscuro/claro, si ya vio el tutorial de bienvenida. Para eso existe **Jetpack DataStore**: almacenamiento local, simple y seguro, que vive en el dispositivo.

---

## ¿DataStore vs SharedPreferences vs Firestore?

| | SharedPreferences | DataStore | Firestore |
| :--- | :---: | :---: | :---: |
| **Dónde vive** | Dispositivo | Dispositivo | Nube |
| **Async seguro** | No (bloquea el hilo) | Sí (Flow/corrutinas) | Sí |
| **Tipado** | No | Sí (Preferences) | Sí |
| **Ideal para** | Nada (deprecado) | Prefs locales | Datos compartidos |

> [!TIP]
> DataStore es el reemplazo oficial de SharedPreferences. Si ves código viejo con `getSharedPreferences()`, puedes migrarlo a DataStore.

---

## Configuración inicial

### Dependencia

En `build.gradle.kts` (módulo app):

```kotlin
dependencies {
    implementation("androidx.datastore:datastore-preferences:1.1.1")
}
```

### Crear la instancia

Crea un archivo `DataStoreManager.kt`:

```kotlin
import android.content.Context
import androidx.datastore.core.DataStore
import androidx.datastore.preferences.core.Preferences
import androidx.datastore.preferences.preferencesDataStore

val Context.dataStore: DataStore<Preferences> by preferencesDataStore(name = "mis_preferencias")
```

> [!IMPORTANT]
> Esta línea debe estar **fuera de cualquier clase**, a nivel de archivo. El delegado `by preferencesDataStore` garantiza que solo se crea una instancia en todo el proceso.

---

## Nivel 1 — Directo en el Composable

El enfoque más simple para entender cómo funciona DataStore: leer y escribir directo desde el Composable.

> **¿Qué es el `context`?**
> En Android, el `context` es el acceso a los recursos del sistema: archivos, preferencias, permisos, la carpeta de la app, etc. Puedes imaginarlo como "la llave del edificio" — sin ella no puedes abrir nada. En Compose lo obtenemos con `LocalContext.current`.

```kotlin
import androidx.compose.runtime.*
import androidx.compose.ui.platform.LocalContext
import androidx.datastore.preferences.core.booleanPreferencesKey
import androidx.datastore.preferences.core.edit
import kotlinx.coroutines.flow.map
import kotlinx.coroutines.launch

@Composable
fun PantallaConfiguracion() {
    val context = LocalContext.current
    val scope = rememberCoroutineScope()

    // Definimos la clave del valor que queremos guardar
    val temaKey = booleanPreferencesKey("tema_oscuro")

    // Leemos el valor como un State de Compose
    val temaOscuro by context.dataStore.data
        .map { it[temaKey] ?: false }
        .collectAsState(initial = false)

    Switch(
        checked = temaOscuro,
        onCheckedChange = { activado ->
            scope.launch {
                context.dataStore.edit { it[temaKey] = activado }
            }
        }
    )
}
```

**Qué pasa aquí paso a paso:**

1. `booleanPreferencesKey("tema_oscuro")` — crea la clave con la que identificamos el valor en el archivo.
2. `context.dataStore.data.map { it[temaKey] ?: false }` — lee el Flow; el `?: false` es el valor por defecto si nunca se guardó nada.
3. `.collectAsState(initial = false)` — convierte el Flow en un `State` que Compose observa y redibuja automáticamente cuando cambia.
4. `scope.launch { dataStore.edit { ... } }` — escribe en una corrutina para no bloquear el hilo de UI.

> **Problema de este enfoque:** si el usuario rota la pantalla, el Composable se destruye y recrea, y el Flow se reinicia. Para eso usamos ViewModel.

---

## Nivel 2 — Con ViewModel

Movemos la lógica al ViewModel para que **sobreviva rotaciones** y el Composable solo se encargue de mostrar datos.

### ViewModel

```kotlin
import android.content.Context
import androidx.datastore.preferences.core.booleanPreferencesKey
import androidx.datastore.preferences.core.edit
import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import kotlinx.coroutines.flow.SharingStarted
import kotlinx.coroutines.flow.map
import kotlinx.coroutines.flow.stateIn
import kotlinx.coroutines.launch

class ConfiguracionViewModel(context: Context) : ViewModel() {

    private val dataStore = context.dataStore
    private val temaKey = booleanPreferencesKey("tema_oscuro")

    val temaOscuro = dataStore.data
        .map { it[temaKey] ?: false }
        .stateIn(viewModelScope, SharingStarted.WhileSubscribed(5000), false)

    fun cambiarTema(activado: Boolean) {
        viewModelScope.launch {
            dataStore.edit { it[temaKey] = activado }
        }
    }
}
```

### Composable

El Composable ahora solo observa y llama funciones — no sabe nada de DataStore:

```kotlin
@Composable
fun PantallaConfiguracion(viewModel: ConfiguracionViewModel) {
    val temaOscuro by viewModel.temaOscuro.collectAsState()

    Switch(
        checked = temaOscuro,
        onCheckedChange = { viewModel.cambiarTema(it) }
    )
}
```

> **Problema de este enfoque:** el ViewModel conoce directamente a DataStore. Si mañana cambiamos el almacenamiento, hay que modificar el ViewModel. Para eso usamos Repository.

---

## Nivel 3 — Con Repository (buenas prácticas)

Agregamos una capa intermedia: el **Repository** se encarga de todo lo relacionado con DataStore. El ViewModel solo le pide datos, sin saber cómo se guardan.

```
Composable  →  ViewModel  →  Repository  →  DataStore
```

### Claves centralizadas

Ahora sí tiene sentido agrupar todas las claves en un solo lugar:

```kotlin
import androidx.datastore.preferences.core.booleanPreferencesKey
import androidx.datastore.preferences.core.stringPreferencesKey

object PreferenciasKeys {
    val TEMA_OSCURO = booleanPreferencesKey("tema_oscuro")
    val NOMBRE_USUARIO = stringPreferencesKey("nombre_usuario")
}
```

### Repository

```kotlin
import android.content.Context
import androidx.datastore.preferences.core.edit
import kotlinx.coroutines.flow.Flow
import kotlinx.coroutines.flow.map

class PreferenciasRepository(private val context: Context) {

    val temaOscuro: Flow<Boolean> = context.dataStore.data
        .map { it[PreferenciasKeys.TEMA_OSCURO] ?: false }

    val nombreUsuario: Flow<String> = context.dataStore.data
        .map { it[PreferenciasKeys.NOMBRE_USUARIO] ?: "" }

    suspend fun guardarTemaOscuro(activado: Boolean) {
        context.dataStore.edit { it[PreferenciasKeys.TEMA_OSCURO] = activado }
    }

    suspend fun guardarNombreUsuario(nombre: String) {
        context.dataStore.edit { it[PreferenciasKeys.NOMBRE_USUARIO] = nombre }
    }
}
```

### ViewModel

El ViewModel ya no importa DataStore ni conoce las claves:

```kotlin
import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import kotlinx.coroutines.flow.SharingStarted
import kotlinx.coroutines.flow.stateIn
import kotlinx.coroutines.launch

class ConfiguracionViewModel(private val repo: PreferenciasRepository) : ViewModel() {

    val temaOscuro = repo.temaOscuro
        .stateIn(viewModelScope, SharingStarted.WhileSubscribed(5000), false)

    val nombreUsuario = repo.nombreUsuario
        .stateIn(viewModelScope, SharingStarted.WhileSubscribed(5000), "")

    fun cambiarTema(activado: Boolean) {
        viewModelScope.launch { repo.guardarTemaOscuro(activado) }
    }

    fun guardarNombre(nombre: String) {
        viewModelScope.launch { repo.guardarNombreUsuario(nombre) }
    }
}
```

### Composable

El Composable no cambia respecto al Nivel 2:

```kotlin
@Composable
fun PantallaConfiguracion(viewModel: ConfiguracionViewModel) {
    val temaOscuro by viewModel.temaOscuro.collectAsState()
    val nombre by viewModel.nombreUsuario.collectAsState()

    Column(modifier = Modifier.fillMaxSize().padding(16.dp)) {
        Row(
            modifier = Modifier.fillMaxWidth(),
            verticalAlignment = Alignment.CenterVertically,
            horizontalArrangement = Arrangement.SpaceBetween
        ) {
            Text("Tema oscuro")
            Switch(checked = temaOscuro, onCheckedChange = { viewModel.cambiarTema(it) })
        }
        Spacer(Modifier.height(16.dp))
        Text("Nombre guardado: $nombre")
    }
}
```

---

## ¿Cuándo usar cada nivel?

| Nivel | Cuándo usarlo |
| :--- | :--- |
| **Nivel 1 — Directo** | Aprender cómo funciona DataStore, prototipos rápidos |
| **Nivel 2 — ViewModel** | Apps pequeñas donde la fuente de datos no cambiará |
| **Nivel 3 — Repository** | Apps reales con más de un origen de datos |

> [!TIP]
> En el curso siempre apuntamos al **Nivel 3**. Empieza con el Nivel 1 para entender la mecánica, luego refactoriza paso a paso.

---

## Referencia rápida

| Operación | Código |
| :--- | :--- |
| Leer un valor | `dataStore.data.map { it[CLAVE] ?: default }` |
| Escribir un valor | `dataStore.edit { it[CLAVE] = valor }` |
| Borrar un valor | `dataStore.edit { it.remove(CLAVE) }` |
| Borrar todo | `dataStore.edit { it.clear() }` |
| Tipo booleano | `booleanPreferencesKey("nombre")` |
| Tipo texto | `stringPreferencesKey("nombre")` |
| Tipo entero | `intPreferencesKey("nombre")` |
