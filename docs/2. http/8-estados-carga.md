# ⏳ Estados de Carga (UI States)

Cuando pedimos datos a internet, la respuesta no es instantánea. La aplicación puede pasar por diferentes estados mientras espera. Si no manejamos esto, el usuario verá una pantalla blanca y pensará que la app se trabó.

## Los 3 estados básicos

1.  **Loading (Cargando):** Estamos esperando la respuesta. Mostramos un `CircularProgressIndicator`.
2.  **Success (Éxito):** Tenemos los datos. Los mostramos en la lista.
3.  **Error:** Algo salió mal (no hay internet, el servidor falló). Mostramos un mensaje de error y un botón de "Reintentar".

## Representando estados con Sealed Classes

La mejor forma de manejar esto en Kotlin es con una `sealed class`:

```kotlin
sealed class GameState {
    object Loading : GameState()
    data class Success(val games: List<Game>) : GameState()
    data class Error(val message: String) : GameState()
}
```

## Implementación en el ViewModel

```kotlin
class GameViewModel : ViewModel() {
    var state by mutableStateOf<GameState>(GameState.Loading)
        private set

    init {
        fetchGames()
    }

    private fun fetchGames() {
        viewModelScope.launch {
            state = GameState.Loading
            try {
                val response = repository.getGames()
                state = GameState.Success(response)
            } catch (e: Exception) {
                state = GameState.Error("Error de conexión")
            }
        }
    }
}
```

## En la UI (Compose)

Simplemente usamos un `when` para decidir qué componente dibujar:

```kotlin
@Composable
fun GameScreen(viewModel: GameViewModel) {
    val state = viewModel.state

    when (state) {
        is GameState.Loading -> {
            CircularProgressIndicator()
        }
        is GameState.Success -> {
            GameList(state.games)
        }
        is GameState.Error -> {
            Text(text = state.message)
        }
    }
}
```

---
_Siguiente: [Módulo 3: Introducción a la Navegación →](../3. navegacion/1-que-es-navegacion.md)_
