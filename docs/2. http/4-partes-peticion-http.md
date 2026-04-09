# Las partes de una petición HTTP 🔍

Cuando le envías una petición a la cocina (servidor), no solo gritas "¡Quiero comida!". Tu pedido necesita estar detallado e ir a la dirección correcta. 

Para que la comunicación exista, necesitamos entender dos cosas: **La URL** y **El Cuerpo del Pedido**.

---

### 1. La Dirección (Partes de una URL)

Para saber a dónde ir, usamos la **URL** (básicamente el enlace web). Se divide en estas partes:

`https://api.mipagina.com:443/usuarios/lista?orden=ascendente`

* **Protocolo (`https://`):** Es el modo en que viaja la información. Piensa que `http` es un camión normal de mensajería, pero `https` es un camión blindado. (La 'S' es de Seguro 🔒).
* **Dominio (`api.mipagina.com`):** Es el nombre del restaurante. La computadora lo convierte internamente a una dirección que pueda entender (Dirección IP).
* **Puerto (`:443`):** Es por **qué puerta** específica del enorme edificio vas a entrar. Muchas veces no lo ves escrito porque las computadoras ya saben que si usas `https`, toca la puerta 443 por defecto.
* **Path o Ruta (`/usuarios/lista`):** Es la sección específica adentro del restaurante a la cual vas. (Ej: voy al área de los postres / dulces / helado).
* **Parámetros (`?orden=ascendente`):** Son filtros adicionales rápidos. *"Quiero la lista, pero por favor ordenada"*.

---

### 2. Anatomía de la Petición (El ticket del pedido)

Cuando el "mesero" lleva tu orden al servidor, lleva un "ticket" literal. Ese ticket se ve por dentro más o menos así:

```http
POST /usuarios/crear HTTP/1.1
Host: api.mipagina.com
Authorization: Bearer 12345
Content-Type: application/json
Accept-Language: es

{
  "nombre": "Juan Pérez",
  "edad": 25
}
```

**¿Qué significa cada cosa de ese ticket?**

1. **La línea principal (`POST /usuarios/crear`):** Dice qué quieres hacer (POST = Crear) y dónde (`/usuarios/crear`).
2. **Los Headers o Cabeceras (Lo que está arriba del paquete):**
   * Son instrucciones extra silenciosas para el mesero y el restaurante.
   * `Host`: Confirma a qué restaurante va.
   * `Authorization`: Es tu tarjeta VIP. Le dice a la cocina "Esta es mi clave secreta, sí tengo permiso de pedir aquí".
   * `Content-Type`: Le avisa a la cocina en qué idioma está la receta abajo. (En este caso dice: *"Oye, lo que sigue abajo está escrito en idioma `application/json`"*).
   * `Accept-Language`: "Por cierto, devuélveme el mensaje en Español (`es`)".
3. **El Body o Cuerpo (Lo que está entre corchetes `{ }`):**
   * Es la carnita del pedido. Aquí envías en formato **JSON** (como vimos en el capítulo anterior) la información de verdad, en este caso, los datos del nuevo usuario que estás creando. (Si la petición fuera un `GET` para leer información, usualmente esta parte está vacía).
