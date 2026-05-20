# 📝 Ejercicios: Navegación

Pon en práctica lo aprendido sobre navegación en Jetpack Compose con estos retos.

---

## 🧭 Reto 1: Tres pantallas en secuencia

Crea una app con tres pantallas conectadas en orden:

1. Crea tres composables: `PantallaA`, `PantallaB` y `PantallaC`.
2. Cada pantalla debe mostrar su nombre y un botón **"Siguiente"**.
3. El flujo debe ser: `A → B → C`.
4. En `PantallaC` el botón debe decir **"Inicio"** y regresar a `PantallaA` (sin acumular pantallas en la pila — usa `popUpTo`).

> [!TIP]
> Para ir de C de vuelta a A sin que el usuario pueda presionar Atrás varias veces, necesitarás `popUpTo("pantallaA") { inclusive = false }`.

---

## 🛍️ Reto 2: Lista de productos con detalle

Simula una tienda con navegación entre lista y detalle:

1. Crea una lista fija de 5 productos, cada uno con `id: Int` y `nombre: String`.
2. Muestra los productos en una `LazyColumn`. Al tocar uno, navega a una pantalla de detalle.
3. La pantalla de detalle debe recibir el `id` del producto como argumento de ruta y mostrar el nombre correspondiente.
4. **Bonus:** muestra también el `nombre` pasándolo como segundo argumento en la ruta (`"detalle/{id}/{nombre}"`).

---

## 🔐 Reto 3: Flujo de Login

Implementa un flujo de autenticación básico:

1. La app arranca en una pantalla de **Login** con un campo de texto para el nombre y un botón **"Entrar"**.
2. Al presionar el botón (sin validar nada), navega a la pantalla **Home** que saluda al usuario por su nombre.
3. Usa `popUpTo("login") { inclusive = true }` para que el botón Atrás no regrese al Login.
4. Agrega un botón **"Cerrar sesión"** en Home que regrese al Login y limpie la pila.

> [!IMPORTANT]
> El nombre del usuario debe pasarse como argumento de la ruta para que la pantalla Home lo muestre sin usar variables globales.

---

## 🗺️ Reto 4 (Desafío): Navegación con barra inferior

Crea una app con una `BottomNavigationBar` que tenga tres tabs: **Inicio**, **Buscar** y **Perfil**.

1. Cada tab debe mostrar una pantalla distinta con un texto identificador.
2. Al cambiar de tab, la pantalla anterior no debe acumularse en la pila.
3. Si el usuario está en **Inicio** y toca **Inicio** nuevamente, el mapa debe regresar a la raíz de ese tab (no duplicar la entrada).

> [!TIP]
> Usa `saveState = true` y `restoreState = true` dentro del bloque `navigate { }` para que cada tab recuerde su estado al volver a ella.

```kotlin
navController.navigate(ruta) {
    popUpTo(navController.graph.startDestinationId) { saveState = true }
    launchSingleTop = true
    restoreState = true
}
```

---

_Módulo de Navegación completado._
