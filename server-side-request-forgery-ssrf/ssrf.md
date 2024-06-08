# ¿SSRF?

## <mark style="color:red;">¿Qué es Server-Side Request Forgery (SSRF)?</mark>

Server-Side Request Forgery (SSRF) es una vulnerabilidad de seguridad que permite a un atacante manipular un servidor para que realice solicitudes HTTP en su nombre. Esto puede incluir acceder a recursos internos, servicios protegidos, o realizar solicitudes maliciosas a sistemas externos. La explotación de SSRF puede tener graves consecuencias, incluyendo la exposición de información confidencial, la explotación de servicios internos, y la ampliación del acceso en la red interna del servidor objetivo.

## <mark style="color:red;">¿Cómo Funciona SSRF?</mark>

SSRF ocurre cuando una aplicación web permite a los usuarios especificar una URL o dirección que el servidor luego utiliza para realizar una solicitud. Si la aplicación no valida adecuadamente la entrada proporcionada por el usuario, un atacante puede manipular esta entrada para hacer que el servidor realice solicitudes a destinos arbitrarios.

## <mark style="color:red;">**Ejemplo de SSRF**</mark>

Considere una aplicación que permite a los usuarios ingresar una URL para obtener una vista previa del contenido de esa página:

```php
<?php
$url = $_GET['url'];
$response = file_get_contents($url);
echo $response;
?>
```

Si un atacante proporciona una URL maliciosa, como `http://localhost/admin`, el servidor realizará una solicitud a la URL proporcionada. Esto puede permitir al atacante acceder a recursos internos que normalmente estarían protegidos.

### <mark style="color:red;">**1. Acceder a un Recurso Interno**</mark>

Supongamos que el atacante quiere acceder al archivo `/etc/passwd` en un servidor Linux interno. Podría hacer lo siguiente:

```sh
curl "http://victima.com/?url=file:///etc/passwd"
```

En este ejemplo, el atacante está manipulando el parámetro `url` para que apunte a un archivo local en el servidor de la víctima. Si la aplicación vulnerable no valida adecuadamente el parámetro `url`, intentará leer el contenido de `/etc/passwd` y devolverlo en la respuesta.

### <mark style="color:red;">**2. Escanear la Red Interna**</mark>

Un atacante también puede usar SSRF para escanear la red interna del servidor de la víctima. Por ejemplo, para verificar si hay un servicio HTTP en ejecución en `http://192.168.1.1`:

```sh
curl "http://victima.com/?url=http://192.168.1.1:80"
```

Si el servidor de la víctima realiza la solicitud y devuelve contenido, el atacante puede determinar que hay un servicio en ejecución en esa dirección IP interna.

### <mark style="color:red;">**3. Acceder a un Servicio Interno**</mark>

Supongamos que hay una API interna accesible solo desde la red interna en `http://localhost/admin`. El atacante podría intentar acceder a esta API:

```sh
curl "http://victima.com/?url=http://localhost/admin"
```

Si la aplicación vulnerable no valida adecuadamente las URL y permite que se realicen solicitudes a `localhost`, el servidor de la víctima podría devolver el contenido de la API interna.
