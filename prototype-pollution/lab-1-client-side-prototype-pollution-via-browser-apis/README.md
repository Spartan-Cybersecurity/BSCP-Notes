---
description: >-
  https://portswigger.net/web-security/prototype-pollution/client-side/browser-apis/lab-prototype-pollution-client-side-prototype-pollution-via-browser-apis
---

# Lab 1: Client-side prototype pollution via browser APIs

Primero ejecutamos el siguiente script de js en la consola:

```
Object.prototype
```

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

Luego de lo anterior, se procede a realizar un GET concatenando lo siguiente:

{% code overflow="wrap" %}
```
/?__proto__[variable]=gerhishere
```
{% endcode %}

Y al ejecutar nuevamente el script previo, sale lo siguiente:

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

Y luego de lo anterior, validamos el codigo fuente para analizarlo:

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

El ataque de prototype pollution ocurre cuando un atacante puede modificar el prototipo de objetos base en JavaScript, como `Object.prototype`, lo que permite inyectar propiedades maliciosas que pueden ser utilizadas en todo el código que se basa en estos objetos.

En este caso, la función `deparam` podría ser explotada si los parámetros de la URL incluyen claves como `__proto__`. Por ejemplo, un atacante podría pasar una URL como esta:

Teniendo en cuenta lo anterior, se procede a modificar nuestro payload para desplegar una alerta en JS:

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>
