---
description: >-
  https://portswigger.net/web-security/prototype-pollution/client-side/lab-prototype-pollution-client-side-prototype-pollution-in-third-party-libraries
---

# Lab 5: Client-side prototype pollution in third-party libraries

Aunque técnicamente este ejercicio se puede resolver de manera manual, se recomienda utilizar DOM Invader:

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

Se detecta la posibilidad de realizar un ataque sobre: hash

Al realizar el escaneo, se encuentra lo siguiente:

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

Observe que DOM Invader ha accedido con éxito al receptor `setTimeout()`a través del dispositivo `hitCallback`.

Y al clickear en exploit:

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

Asi que teniendo en cuenta que ya tenemos el exploit funcional.

Se procede con al creacion del siguiente exploit:

```html
<script>
    location="https://0a310070045995a3844ecca90014008b.web-security-academy.net/#__proto__[hitCallback]=alert%28document.cookie%29"
</script>
```

El exploit anterior genera una alerta en JS para imprimir las cookies.
