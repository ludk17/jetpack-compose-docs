# 📝 Ejercicios: Maps y Geolocalización

Pon en práctica lo aprendido sobre Google Maps y ubicación del dispositivo con estos retos.

---

## 📍 Reto 1: Mapa centrado en tu ciudad

Crea una pantalla con un mapa que:

1. Aparezca centrado en **Trujillo** (`LatLng(-8.1058, -79.0225)`) con zoom `13f`.
2. Tenga los controles de zoom habilitados (`zoomControlsEnabled = true`).
3. Muestre un marcador fijo en la **Plaza de Armas de Trujillo** con título `"Plaza de Armas"` y snippet `"Centro histórico de Trujillo"`.
4. Al tocar el marcador se debe ver el título y el snippet en una ventana emergente.

---

## 🗺️ Reto 2: Marcadores desde una lista

Crea una app que muestre varios puntos de interés en el mapa:

1. Define una `data class LugarInteres(val nombre: String, val descripcion: String, val latLng: LatLng)`.
2. Crea una lista con al menos **4 lugares** de tu ciudad (universidades, plazas, centros comerciales, etc.).
3. Muestra todos los lugares como marcadores en el mapa.
4. Cada marcador debe mostrar el `nombre` como título y la `descripcion` como snippet.
5. **Bonus:** agrega un botón flotante que anime la cámara al primer lugar de la lista.

---

## 🖱️ Reto 3: Agregar marcadores con toque largo

Crea un mapa interactivo donde el usuario pueda colocar sus propios marcadores:

1. Al hacer un **toque largo** en cualquier punto del mapa, agrega un marcador en esa coordenada.
2. El título del marcador debe mostrar las coordenadas redondeadas a 4 decimales: `"Lat: -8.1058, Lng: -79.0225"`.
3. Muestra un contador en la parte superior de la pantalla con el texto `"Marcadores: X"`.
4. Agrega un botón **"Limpiar"** que elimine todos los marcadores agregados.

> [!TIP]
> Guarda los marcadores en un `mutableStateListOf<LatLng>()` dentro de un `remember`. Cuando cambies la lista, Compose redibujará los marcadores automáticamente.

---

## 📡 Reto 4: Mi ubicación en el mapa

Crea una pantalla que muestre la ubicación actual del usuario:

1. Pide el permiso `ACCESS_FINE_LOCATION` al abrir la pantalla.
2. Si el permiso es **denegado**, muestra un mensaje: `"Activa la ubicación para usar esta función"` en lugar del mapa.
3. Si el permiso es **concedido**, obtén la última ubicación conocida con `FusedLocationProviderClient`.
4. Centra la cámara en la ubicación obtenida con zoom `16f` y coloca un marcador con el texto `"Estás aquí"`.
5. Agrega un botón **"Actualizar"** que vuelva a pedir la ubicación y recentre el mapa.

---

## 🏪 Reto 5 (Desafío): Tiendas cercanas

Combina marcadores fijos con la ubicación del usuario:

1. Define una lista de 5 tiendas ficticias con nombre y coordenadas cerca de Trujillo.
2. Muestra la ubicación del usuario (con permiso) y los marcadores de las tiendas en el mismo mapa.
3. Usa **color diferente** para el marcador del usuario: `BitmapDescriptorFactory.defaultMarker(BitmapDescriptorFactory.HUE_AZURE)`.
4. Cuando el usuario toque una tienda, muestra un `Toast` con el texto `"Seleccionaste: [nombre de la tienda]"`.

```kotlin
// Marcador con color personalizado
Marker(
    state = rememberMarkerState(position = miUbicacion),
    title = "Yo",
    icon = BitmapDescriptorFactory.defaultMarker(BitmapDescriptorFactory.HUE_AZURE)
)
```

> [!TIP]
> Para el `Toast` usa `LocalContext.current` dentro del composable y llámalo desde `onInfoWindowClick` del `Marker`.

---

_Sección: [Maps con Google Maps](1-configuracion-google-maps.md)_
