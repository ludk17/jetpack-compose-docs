# 📦 Pasando Argumentos entre Pantallas

A veces no solo queremos ir a otra pantalla, sino también llevar información con nosotros. Por ejemplo, si haces clic en un usuario de una lista, quieres pasar su `ID` a la pantalla de detalle.

En Compose Navigation, pasamos datos a través de la URL de la ruta, muy parecido a como funcionan las páginas web: `ruta/valor`.

## 1. Definir la ruta con parámetros

Para decir que una ruta espera un dato, usamos llaves `{}`:

```kotlin
// El nombre del parámetro es "userId"
composable("detalle/{userId}") { backStackEntry ->
    // Extraemos el valor que viene en la ruta
    val id = backStackEntry.arguments?.getString("userId")
    DetalleScreen(id)
}
```

## 2. Enviar el dato al navegar

Al usar `navigate()`, construimos el string con el valor real que queremos enviar:

```kotlin
val idUsuario = "123"
navController.navigate("detalle/$idUsuario")
```

## Ejemplo Completo

```kotlin
NavHost(navController = navController, startDestination = "lista") {
    
    // Pantalla 1: Lista
    composable("lista") {
        UserList(onUserClick = { id -> 
            navController.navigate("detalle/$id") 
        })
    }

    // Pantalla 2: Detalle (espera un ID)
    composable(
        route = "detalle/{userId}",
        arguments = listOf(navArgument("userId") { type = NavType.StringType })
    ) { backStackEntry ->
        val userId = backStackEntry.arguments?.getString("userId") ?: "Desconocido"
        UserDetailsScreen(userId)
    }
}
```

## Tipos de Datos compatibles

Por defecto, los argumentos son strings, pero puedes especificar otros tipos:

| Tipo | Definición en `navArgument` |
|---|---|
| **Int** | `type = NavType.IntType` |
| **Boolean** | `type = NavType.BoolType` |
| **Float** | `type = NavType.FloatType` |

---

> [!WARNING]
> No intentes pasar objetos complejos (como un objeto `Usuario` completo) a través de la navegación. Lo ideal es pasar solo el **ID** y que la pantalla de destino busque el objeto en una base de datos o API.

---
_Has completado el módulo de Navegación._
