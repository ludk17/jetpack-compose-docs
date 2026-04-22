# 🗺️ ¿Qué es la Navegación?

Navegar en una aplicación es como moverse por las diferentes habitaciones de una casa. No puedes estar en todas las habitaciones al mismo tiempo, pero necesitas una forma sencilla de ir de la cocina al dormitorio sin perderte.

En **Jetpack Compose**, la navegación nos permite cambiar lo que el usuario ve en pantalla basándose en sus acciones (como hacer clic en un botón).

## El Mapa y el GPS: Conceptos Clave

Para que la navegación funcione, Android usa tres piezas principales:

| Componente | Analogía | Función |
|---|---|---|
| **NavController** | El GPS | Es el encargado de realizar el "viaje". Tú le dices a dónde quieres ir y él te lleva. |
| **NavHost** | El Mapa | Es el contenedor donde se "dibujan" las pantallas. Define qué rutas existen. |
| **Rutas (Routes)** | Las Direcciones | Son strings (texto) que identifican cada pantalla, por ejemplo: `"inicio"` o `"perfil"`. |

## ¿Por qué no usar solo `if/else`?

Podríamos usar una variable de estado y un simple `if` para mostrar una pantalla u otra:

```kotlin
var screen by remember { mutableStateOf("home") }

if (screen == "home") {
    HomeScreen(onNavigate = { screen = "detail" })
} else {
    DetailScreen()
}
```

**Pero esto tiene problemas:**
1. No funciona el botón "Atrás" del celular (se cerraría la app).
2. Es difícil pasar datos entre pantallas.
3. Se vuelve un caos cuando tienes 10 pantallas.

Jetpack Compose Navigation soluciona esto gestionando la **Pila de Navegación** (Backstack), que es como un montón de platos: cada vez que navegas a una pantalla, pones un plato encima; cuando presionas "atrás", quitas el plato de arriba.

---

> [!IMPORTANT]
> Para usar navegación, debemos agregar la dependencia oficial en nuestro archivo `build.gradle.kts`:
> ```kotlin
> implementation("androidx.navigation:navigation-compose:2.7.7")
> ```

---
_Siguiente: [Configurando el NavHost →](2-navhost-y-rutas.md)_
