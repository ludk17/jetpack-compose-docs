# 📍 Mapa y Marcadores en Compose

Ya tienes la configuración lista. Ahora aprenderás a mostrar un mapa, mover la cámara a una ubicación específica y agregar marcadores para señalar lugares de interés.

---

## El composable `GoogleMap`

`GoogleMap` funciona igual que cualquier otro composable de Compose: recibe parámetros y dibuja contenido en pantalla. La diferencia es que internamente levanta un `MapView` nativo de Android.

```kotlin
import com.google.maps.android.compose.GoogleMap
import com.google.maps.android.compose.rememberCameraPositionState

@Composable
fun MapaBasico() {
    GoogleMap(
        modifier = Modifier.fillMaxSize()
    )
}
```

Por defecto el mapa aparece centrado en coordenadas `(0, 0)` (el océano Atlántico). Necesitas posicionar la cámara en un lugar útil.

---

## Posición de la cámara

La **cámara** es el "punto de vista" del mapa: su centro, zoom y rotación. Se controla con `CameraPositionState`.

```kotlin
import com.google.android.gms.maps.model.CameraPosition
import com.google.android.gms.maps.model.LatLng
import com.google.maps.android.compose.rememberCameraPositionState

@Composable
fun MapaCentrado() {
    val lima = LatLng(-12.0464, -77.0428)

    val cameraState = rememberCameraPositionState {
        position = CameraPosition.fromLatLngZoom(lima, 12f)
    }

    GoogleMap(
        modifier = Modifier.fillMaxSize(),
        cameraPositionState = cameraState
    )
}
```

### Niveles de zoom

| Zoom | Vista |
| :---: | :--- |
| `1f` | Mundo completo |
| `5f` | Continente |
| `10f` | Ciudad |
| `15f` | Calles y manzanas |
| `20f` | Edificios individuales |

---

## Mover la cámara programáticamente

Cuando el usuario toca un botón o se actualiza una ubicación, puedes animar la cámara con `animate()`:

```kotlin
import com.google.android.gms.maps.CameraUpdateFactory
import kotlinx.coroutines.launch

@Composable
fun MapaConBoton() {
    val scope = rememberCoroutineScope()
    val trujillo = LatLng(-8.1120, -79.0288)

    val cameraState = rememberCameraPositionState {
        position = CameraPosition.fromLatLngZoom(LatLng(-12.0464, -77.0428), 6f)
    }

    Box(modifier = Modifier.fillMaxSize()) {
        GoogleMap(
            modifier = Modifier.fillMaxSize(),
            cameraPositionState = cameraState
        )

        Button(
            onClick = {
                scope.launch {
                    cameraState.animate(
                        CameraUpdateFactory.newLatLngZoom(trujillo, 13f)
                    )
                }
            },
            modifier = Modifier
                .align(Alignment.BottomCenter)
                .padding(16.dp)
        ) {
            Text("Ir a Trujillo")
        }
    }
}
```

> [!TIP]
> `cameraState.animate()` es una función `suspend`, por eso necesita un `CoroutineScope` (`rememberCoroutineScope()`).

---

## Marcadores

Un **marcador** (`Marker`) es el alfiler rojo que señala un punto en el mapa. En `maps-compose` se coloca como composable hijo de `GoogleMap`:

```kotlin
import com.google.maps.android.compose.Marker
import com.google.maps.android.compose.rememberMarkerState

@Composable
fun MapaConMarcador() {
    val upn = LatLng(-8.1058, -79.0225)

    val cameraState = rememberCameraPositionState {
        position = CameraPosition.fromLatLngZoom(upn, 16f)
    }

    GoogleMap(
        modifier = Modifier.fillMaxSize(),
        cameraPositionState = cameraState
    ) {
        Marker(
            state = rememberMarkerState(position = upn),
            title = "UPN Trujillo",
            snippet = "Universidad Privada del Norte"
        )
    }
}
```

El `title` aparece en negrita al tocar el marcador. El `snippet` es el subtítulo debajo.

---

## Múltiples marcadores desde una lista

En una app real los marcadores vienen de datos (API, base de datos). Los iteras igual que en `LazyColumn`, pero dentro de `GoogleMap`:

```kotlin
data class Lugar(val nombre: String, val latLng: LatLng)

@Composable
fun MapaConVariosLugares() {
    val lugares = listOf(
        Lugar("UPN Trujillo", LatLng(-8.1058, -79.0225)),
        Lugar("Plaza de Armas", LatLng(-8.1091, -79.0255)),
        Lugar("Mall Aventura", LatLng(-8.0872, -79.0374))
    )

    val centro = LatLng(-8.1080, -79.0260)
    val cameraState = rememberCameraPositionState {
        position = CameraPosition.fromLatLngZoom(centro, 14f)
    }

    GoogleMap(
        modifier = Modifier.fillMaxSize(),
        cameraPositionState = cameraState
    ) {
        lugares.forEach { lugar ->
            Marker(
                state = rememberMarkerState(position = lugar.latLng),
                title = lugar.nombre
            )
        }
    }
}
```

---

## Configuraciones del mapa

### MapUiSettings — controles visuales

```kotlin
import com.google.maps.android.compose.MapUiSettings

GoogleMap(
    modifier = Modifier.fillMaxSize(),
    cameraPositionState = cameraState,
    uiSettings = MapUiSettings(
        zoomControlsEnabled = true,      // botones +/- en esquina
        myLocationButtonEnabled = false,  // botón de "mi ubicación"
        compassEnabled = true,            // brújula al rotar el mapa
        mapToolbarEnabled = false         // barra de herramientas al tocar marcador
    )
)
```

### MapProperties — tipo de mapa

```kotlin
import com.google.maps.android.compose.MapProperties
import com.google.android.gms.maps.model.MapType

GoogleMap(
    modifier = Modifier.fillMaxSize(),
    cameraPositionState = cameraState,
    properties = MapProperties(
        mapType = MapType.NORMAL,   // NORMAL, SATELLITE, HYBRID, TERRAIN
        isMyLocationEnabled = false  // requiere permiso de ubicación
    )
)
```

> [!IMPORTANT]
> `isMyLocationEnabled = true` en `MapProperties` **requiere** que el permiso `ACCESS_FINE_LOCATION` ya esté concedido. Si no, la app lanzará una excepción. Ver el siguiente archivo para manejar permisos correctamente.

---

## Capturar eventos del mapa

```kotlin
GoogleMap(
    modifier = Modifier.fillMaxSize(),
    cameraPositionState = cameraState,
    onMapClick = { latLng ->
        // El usuario tocó el mapa en esta coordenada
        println("Tocaste: lat=${latLng.latitude}, lng=${latLng.longitude}")
    },
    onMapLongClick = { latLng ->
        // Toque largo — útil para agregar marcadores manualmente
    }
)
```

---

## Ejemplo completo

```kotlin
@Composable
fun MapaCompleto() {
    var marcadores by remember { mutableStateOf(listOf<LatLng>()) }

    val cameraState = rememberCameraPositionState {
        position = CameraPosition.fromLatLngZoom(LatLng(-8.1058, -79.0225), 14f)
    }

    GoogleMap(
        modifier = Modifier.fillMaxSize(),
        cameraPositionState = cameraState,
        uiSettings = MapUiSettings(zoomControlsEnabled = true),
        onMapLongClick = { latLng ->
            marcadores = marcadores + latLng
        }
    ) {
        marcadores.forEach { punto ->
            Marker(
                state = rememberMarkerState(position = punto),
                title = "Lat: ${"%.4f".format(punto.latitude)}"
            )
        }
    }
}
```

Aquí, cada vez que el usuario hace un toque largo, se agrega un marcador en esa posición.

---

## Referencia rápida

| Elemento | Clase / Función |
| :--- | :--- |
| Composable del mapa | `GoogleMap(...)` |
| Estado de la cámara | `rememberCameraPositionState { }` |
| Posición + zoom | `CameraPosition.fromLatLngZoom(latLng, zoom)` |
| Animar cámara | `cameraState.animate(CameraUpdateFactory...)` |
| Agregar marcador | `Marker(state = rememberMarkerState(position = latLng))` |
| Tipo de mapa | `MapProperties(mapType = MapType.SATELLITE)` |
| Controles UI | `MapUiSettings(zoomControlsEnabled = true)` |
| Click en el mapa | `onMapClick = { latLng -> ... }` |

---

_Siguiente: [Geolocalización →](3-geolocalizacion.md)_
