# Retrofit: El cliente de Internet 🌐

En la sección anterior vimos que las peticiones HTTP son como pedidos en un restaurante. **Retrofit** es la herramienta más popular en Android para hacer esos pedidos de forma profesional, rápida y organizada.

### ¿Qué es Retrofit?
Es una librería que convierte una API de internet en una **interfaz de Java/Kotlin**. Básicamente, solo tienes que definir qué quieres pedir y Retrofit se encarga de todo el trabajo sucio de conectar con el servidor y convertir el JSON en objetos de Kotlin.

---

### Pasos para usar Retrofit

Para este ejemplo, imaginemos que queremos consultar una lista de videojuegos de una API.

#### 1. Agregar dependencias
En tu archivo `build.gradle` (Module :app), añade estas librerías:

```kotlin
implementation("com.squareup.retrofit2:retrofit:2.9.0")
implementation("com.squareup.retrofit2:converter-gson:2.9.0") // Para convertir JSON a Objetos
```

#### 2. Crear el Modelo (Data Class)
Define cómo lucen los datos que vas a recibir. Si el JSON tiene un campo "name" y "price", tu clase debe ser:

```kotlin
data class Game(
    val id: Int,
    val name: String,
    val genre: String
)
```

#### 3. Definir la Interfaz (El "Menú")
Aquí le dices a Retrofit qué rutas (endpoints) existen en la API.

```kotlin
interface GameApiService {
    @GET("games") // La ruta final de la URL
    suspend fun getAllGames(): List<Game> // 'suspend' indica que es ASÍNCRONO
}
```

#### 4. Crear la Instancia de Retrofit
Esta es la configuración central para que Retrofit sepa a qué servidor conectarse.

```kotlin
object RetrofitClient {
    private const val BASE_URL = "https://api.tu-servidor.com/"

    val instance: GameApiService by lazy {
        Retrofit.Builder()
            .baseUrl(BASE_URL)
            .addConverterFactory(GsonConverterFactory.create()) // Convierte el JSON automáticamente
            .build()
            .create(GameApiService::class.java)
    }
}
```

---

### 5. ¿Cómo usarlo en Compose? (Sin ViewModel)

Aunque lo ideal es usar ViewModels, puedes probar tus peticiones usando un `LaunchedEffect` para que la petición se haga cuando el componente se cargue.

```kotlin
@Composable
fun GameListScreen() {
    // Estado para guardar la lista de juegos
    var games by remember { mutableStateOf(listOf<Game>()) }

    // Ejecutamos la petición de forma asíncrona
    LaunchedEffect(Unit) {
        try {
            val response = RetrofitClient.instance.getAllGames()
            games = response // Guardamos el resultado en nuestro estado
        } catch (e: Exception) {
            // Manejar el error (ej. sin internet)
        }
    }

    // Dibujamos la lista
    LazyColumn {
        items(games) { game ->
            Text(text = game.name)
        }
    }
}
```

---

### Resumen de Componentes:

1. **Model:** Representa los datos (JSON).
2. **Interface:** Define las operaciones (GET, POST, etc.).
3. **Retrofit Builder:** Configura la conexión.
4. **Execution:** Llama a la función (usualmente dentro de una Corrutina).

> [!TIP]
> Recuerda siempre añadir el permiso de internet en tu archivo `AndroidManifest.xml`:
> `<uses-permission android:name="android.permission.INTERNET" />`
