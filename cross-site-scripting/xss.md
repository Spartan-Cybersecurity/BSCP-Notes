# ¿XSS?

## <mark style="color:red;">¿Qué es el Cross-Site Scripting (XSS)?</mark>

El Cross-Site Scripting (XSS) es una vulnerabilidad de seguridad que permite a un atacante inyectar scripts maliciosos en contenido que otros usuarios verán. Los scripts maliciosos se ejecutan en el contexto del navegador de la víctima, lo que puede permitir al atacante robar cookies, sesiones, credenciales, realizar acciones en nombre del usuario y otros actos maliciosos. XSS es una de las vulnerabilidades más comunes en aplicaciones web y se encuentra catalogada en el OWASP Top Ten.

## <mark style="color:red;">¿Cómo funciona el XSS?</mark>

El XSS explota la confianza que un navegador tiene en el contenido que recibe de un sitio web. Los atacantes inyectan código JavaScript malicioso en una página web que posteriormente será vista por otros usuarios. Cuando estos usuarios acceden a la página comprometida, el código malicioso se ejecuta en sus navegadores, teniendo acceso a la información sensible y a la interacción del usuario con la página web.

## <mark style="color:red;">**Pasos del proceso de un ataque XSS:**</mark>

1. <mark style="color:red;">**Identificación de una entrada vulnerable**</mark><mark style="color:red;">:</mark> El atacante encuentra un campo de entrada o URL en la aplicación web que no valida ni escapa adecuadamente las entradas del usuario.
2. <mark style="color:red;">**Inyección del script malicioso**</mark><mark style="color:red;">:</mark> El atacante inyecta un payload de JavaScript en el campo de entrada o URL vulnerable.
3. <mark style="color:red;">**Ejecución del script en el navegador de la víctima**</mark><mark style="color:red;">:</mark> Cuando un usuario legítimo accede a la página web que contiene la entrada manipulada, el navegador ejecuta el script malicioso.
4. <mark style="color:red;">**Robo de información o ejecución de acciones maliciosas**</mark><mark style="color:red;">:</mark> El script malicioso puede robar cookies, sesiones, credenciales, modificar la apariencia de la página, o redirigir a la víctima a sitios maliciosos.

### <mark style="color:red;">Prueba de Concepto de XSS</mark>

Una prueba de concepto simple de XSS reflejado puede ser una URL manipulada con un payload inyectado:

```html
http://victima.com/search?q=<script>alert('XSS')</script>
```

Cuando un usuario hace clic en este enlace, el navegador ejecutará el código JavaScript `alert('XSS')`, demostrando la existencia de la vulnerabilidad.

## <mark style="color:red;">Tipos de ataques XSS</mark>

### <mark style="color:red;">**XSS Reflejado:**</mark>

El XSS reflejado ocurre cuando el payload malicioso se inyecta en una solicitud HTTP y el servidor web refleja este contenido directamente en la respuesta. Es más común en formularios de búsqueda y parámetros de URL.

**Ejemplo**:

```html
http://victima.com/search?q=<script>alert('XSS')</script>
```

En este caso, si la aplicación no maneja correctamente la entrada del usuario, el script se reflejará en la respuesta y se ejecutará en el navegador de la víctima.

### <mark style="color:red;">**XSS Almacenado:**</mark>

El XSS almacenado (también conocido como persistente) se produce cuando el payload malicioso se almacena en el servidor, normalmente en una base de datos, y luego se muestra a otros usuarios sin la adecuada sanitización. Este tipo es más peligroso porque cualquier usuario que visite la página afectada ejecutará el script malicioso.

**Ejemplo**: Un atacante inserta un comentario en un foro con el siguiente contenido:

```html
<script>alert('XSS Almacenado')</script>
```

Cuando otros usuarios ven ese comentario, el script se ejecuta en sus navegadores.

### <mark style="color:red;">**XSS Basado en DOM:**</mark>

El XSS basado en DOM (Document Object Model) ocurre cuando el payload malicioso se ejecuta como resultado de modificaciones en el DOM en el lado del cliente, sin interacción directa con el servidor. Este tipo de XSS se origina y se ejecuta completamente en el navegador del cliente.

**Ejemplo**:

```javascript
// Una página web tiene el siguiente código JavaScript
var search = document.location.hash.substring(1);
document.getElementById('result').innerHTML = search;
```

Si un atacante manipula la URL para incluir un hash malicioso:

```html
http://victima.com/#<script>alert('XSS Basado en DOM')</script>
```

El script se ejecutará cuando la página procese el hash de la URL y lo inserte en el DOM.

## <mark style="color:red;">Consideraciones Adicionales</mark>

Para mitigar los ataques XSS, es crucial implementar una serie de prácticas de seguridad:

1. <mark style="color:red;">**Validación y escape de entrada**</mark><mark style="color:red;">:</mark> Validar y escapar adecuadamente todas las entradas del usuario antes de utilizarlas.
2. <mark style="color:red;">**Content Security Policy (CSP)**</mark><mark style="color:red;">:</mark> Implementar CSP para restringir la ejecución de scripts no confiables.
3. <mark style="color:red;">**Sanitización del contenido**</mark><mark style="color:red;">:</mark> Usar bibliotecas de sanitización para limpiar las entradas antes de almacenarlas o mostrarlas.
4. <mark style="color:red;">**HTTPOnly y Secure cookies**</mark><mark style="color:red;">:</mark> Configurar cookies con atributos `HttpOnly` y `Secure` para evitar el acceso a cookies desde scripts y asegurar su transmisión.
