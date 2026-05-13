# 💾 Jetpack DataStore

Hay datos pequeños que no pertenecen a Firestore — preferencias del usuario, el tema oscuro/claro, si ya vio el tutorial de bienvenida, el UID en caché. Para eso existe **Jetpack DataStore**: almacenamiento local, simple y seguro, que vive en el dispositivo.

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

## Existen dos tipos de DataStore

1. **Preferences DataStore** — guarda pares clave-valor simples (como SharedPreferences, pero seguro). Este es el que usaremos.
2. **Proto DataStore** — guarda objetos tipados con Protocol Buffers. Más complejo, para casos avanzados.

---

## Configuración

### Dependencia

En `build.gradle.kts` (módulo app):

```kotlin
dependencies {
    implementation("androidx.datastore:datastore-preferences:1.1.1")
}
```

### Crear la instancia (una sola vez)

Crea un archivo `DataStoreManager.kt`:

```kotlin
import android.content.Context
import androidx.datastore.core.DataStore
import androidx.datastore.preferences.core.Preferences
import androidx.datastore.preferences.preferencesDataStore

// Extensión que crea el DataStore con nombre "mis_preferencias"
val Context.dataStore: DataStore<Preferences> by preferencesDataStore(name = "mis_preferencias")
```

> [!IMPORTANT]
> Esta línea debe estar **fuera de cualquier clase**, a nivel de archivo. El delegado `by preferencesDataStore` garantiza que solo se crea una instancia del archivo en todo el proceso.

---

## Definir claves

Cada valor que guardas necesita una **clave tipada**:

```kotlin
import androidx.datastore.preferences.core.booleanPreferencesKey
import androidx.datastore.preferences.core.intPreferencesKey
import androidx.datastore.preferences.core.stringPreferencesKey

object PreferenciasKeys {
    val TEMA_OSCURO = booleanPreferencesKey("tema_oscuro")
    val NOMBRE_USUARIO = stringPreferencesKey("nombre_usuario")
    val INTENTOS_LOGIN = intPreferencesKey("intentos_login")
}
```

---

## Leer datos

Leer de DataStore devuelve un `Flow<Preferences>`, lo que significa que se actualiza automáticamente cada vez que el valor cambia:

```kotlin
import androidx.datastore.preferences.core.Preferences
import kotlinx.coroutines.flow.Flow
import kotlinx.coroutines.flow.map

fun leerTemaOscuro(context: Context): Flow<Boolean> {
    return context.dataStore.data
        .map { preferencias ->
            preferencias[PreferenciasKeys.TEMA_OSCURO] ?: false // false es el valor por defecto
        }
}
```

---

## Escribir datos

Escribir requiere una corrutina (no bloquea el hilo principal):

```kotlin
import androidx.datastore.preferences.core.edit
import kotlinx.coroutines.CoroutineScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch

fun guardarTemaOscuro(context: Context, activado: Boolean) {
    CoroutineScope(Dispatchers.IO).launch {
        context.dataStore.edit { preferencias ->
            preferencias[PreferenciasKeys.TEMA_OSCURO] = activado
        }
    }
}
```

---

## Integración con ViewModel y Compose

La forma correcta en una app real es manejar DataStore desde un **Repository** y exponer los valores como `StateFlow` en el ViewModel.

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

```kotlin
import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import kotlinx.coroutines.flow.SharingStarted
import kotlinx.coroutines.flow.stateIn
import kotlinx.coroutines.launch

class ConfiguracionViewModel(
    private val repo: PreferenciasRepository
) : ViewModel() {

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

### Pantalla de Configuración

```kotlin
@Composable
fun ConfiguracionScreen(viewModel: ConfiguracionViewModel) {
    val temaOscuro by viewModel.temaOscuro.collectAsState()
    val nombre by viewModel.nombreUsuario.collectAsState()

    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp)
    ) {
        Text("Configuración", style = MaterialTheme.typography.headlineMedium)

        Spacer(Modifier.height(24.dp))

        Row(
            modifier = Modifier.fillMaxWidth(),
            verticalAlignment = Alignment.CenterVertically,
            horizontalArrangement = Arrangement.SpaceBetween
        ) {
            Text("Tema oscuro")
            Switch(
                checked = temaOscuro,
                onCheckedChange = { viewModel.cambiarTema(it) }
            )
        }

        Spacer(Modifier.height(16.dp))

        Text("Nombre guardado: $nombre", style = MaterialTheme.typography.bodyMedium)
    }
}
```

---

## Caso práctico: recordar si el usuario ya inició sesión

Un patrón muy común es combinar Firebase Auth con DataStore: cuando el usuario inicia sesión, guardas su UID localmente. Al abrir la app, lees el UID de DataStore para saber si mostrar el Login o la pantalla principal sin llamar a Firebase.

```kotlin
object PreferenciasKeys {
    val UID_USUARIO = stringPreferencesKey("uid_usuario")
}

// Guardar al hacer login exitoso
suspend fun guardarUID(context: Context, uid: String) {
    context.dataStore.edit { it[PreferenciasKeys.UID_USUARIO] = uid }
}

// Leer al arrancar la app
fun leerUID(context: Context): Flow<String> =
    context.dataStore.data.map { it[PreferenciasKeys.UID_USUARIO] ?: "" }

// Limpiar al cerrar sesión
suspend fun limpiarUID(context: Context) {
    context.dataStore.edit { it.remove(PreferenciasKeys.UID_USUARIO) }
}
```

---

## Borrar un valor específico

```kotlin
suspend fun limpiarNombre(context: Context) {
    context.dataStore.edit { preferencias ->
        preferencias.remove(PreferenciasKeys.NOMBRE_USUARIO)
    }
}
```

## Borrar todo

```kotlin
suspend fun limpiarTodo(context: Context) {
    context.dataStore.edit { it.clear() }
}
```

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
