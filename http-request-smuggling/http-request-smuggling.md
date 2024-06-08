# ¿HTTP request smuggling?

## <mark style="color:red;">¿Qué es HTTP Request Smuggling?</mark>

HTTP Request Smuggling (HRS) es una vulnerabilidad en la que un atacante interfiere con la forma en que un servidor web maneja las solicitudes HTTP, permitiendo que una solicitud maliciosa sea enviada a través de un servidor proxy o firewall sin ser detectada. Esta vulnerabilidad puede ser explotada para realizar diversos ataques, como la elusión de seguridad, la obtención de información sensible o la realización de ataques de denegación de servicio (DoS).

## <mark style="color:red;">¿Cómo Funciona HTTP Request Smuggling?</mark>

HTTP Request Smuggling se aprovecha de las diferencias en la forma en que los servidores (por ejemplo, front-end proxies y back-end servers) manejan y procesan las solicitudes HTTP. La vulnerabilidad surge debido a la inconsistencia en el manejo de los encabezados HTTP, específicamente los encabezados `Content-Length` y `Transfer-Encoding`.

### <mark style="color:red;">**Pasos del ataque:**</mark>

1. <mark style="color:red;">**Desincronización de Solicitudes**</mark><mark style="color:red;">:</mark> El atacante envía una solicitud maliciosa diseñada para desincronizar cómo el servidor front-end y el servidor back-end interpretan la longitud de la solicitud. Esto generalmente se logra utilizando encabezados `Content-Length` y `Transfer-Encoding` conflictivos.
2. <mark style="color:red;">**Inserción de una Solicitud Maliciosa**</mark><mark style="color:red;">:</mark> La solicitud maliciosa se inserta en el flujo de tráfico HTTP y se interpreta de manera diferente por los servidores. Mientras que el servidor front-end puede considerar que la solicitud está completa, el servidor back-end puede ver datos adicionales, interpretándolos como una nueva solicitud.
3. <mark style="color:red;">**Ejecución de la Solicitud Maliciosa**</mark><mark style="color:red;">:</mark> La solicitud maliciosa llega al servidor back-end como una solicitud completamente nueva, permitiendo al atacante ejecutar acciones no autorizadas.

## <mark style="color:red;">Ejemplo de HTTP Request Smuggling</mark>

Supongamos que un servidor front-end utiliza `Content-Length` para determinar el final de una solicitud, mientras que el servidor back-end utiliza `Transfer-Encoding`. Un atacante podría enviar la siguiente solicitud:

```makefile
POST / HTTP/1.1
Host: victima.com
Content-Length: 13
Transfer-Encoding: chunked

0

GET /malicious HTTP/1.1
Host: victima.com
```

En este caso:

* El servidor front-end puede interpretar la solicitud como un POST con un cuerpo vacío (debido al `Content-Length: 13`).
* El servidor back-end, utilizando `Transfer-Encoding: chunked`, puede ver `GET /malicious HTTP/1.1` como una nueva solicitud completa.

## <mark style="color:red;">Impacto de HTTP Request Smuggling</mark>

El impacto de la explotación de HRS puede ser significativo y variado:

1. <mark style="color:red;">**Elusión de Seguridad**</mark><mark style="color:red;">:</mark> Un atacante puede eludir controles de seguridad como firewalls y sistemas de detección de intrusos (IDS).
2. <mark style="color:red;">**Robo de Información**</mark><mark style="color:red;">:</mark> Posibilidad de obtener información sensible de otras solicitudes HTTP.
3. <mark style="color:red;">**Manipulación de Contenido**</mark><mark style="color:red;">:</mark> Alterar las respuestas del servidor para incluir contenido malicioso.
4. <mark style="color:red;">**Denegación de Servicio (DoS)**</mark><mark style="color:red;">:</mark> Causar desincronización en el manejo de solicitudes, resultando en la interrupción del servicio.

## <mark style="color:red;">Mitigación de HTTP Request Smuggling</mark>

Para protegerse contra HRS, es crucial implementar varias medidas de seguridad:

1. <mark style="color:red;">**Consistencia en el Manejo de Encabezados**</mark><mark style="color:red;">:</mark> Asegurarse de que todos los servidores en la cadena de procesamiento de solicitudes HTTP manejen los encabezados `Content-Length` y `Transfer-Encoding` de manera consistente.
2. <mark style="color:red;">**Deshabilitar Transfer-Encoding cuando no sea Necesario**</mark><mark style="color:red;">:</mark> Si no se necesita, deshabilitar el uso de `Transfer-Encoding: chunked` en el servidor.
3. <mark style="color:red;">**Actualizar y Parchear Software**</mark><mark style="color:red;">:</mark> Mantener los servidores web y proxies actualizados con los últimos parches de seguridad.
4. <mark style="color:red;">**Inspección de Solicitudes**</mark><mark style="color:red;">:</mark> Implementar inspección y validación rigurosas de las solicitudes HTTP para detectar y rechazar patrones maliciosos.
