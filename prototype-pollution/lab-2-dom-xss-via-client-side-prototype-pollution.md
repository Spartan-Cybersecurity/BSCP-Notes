---
description: >-
  https://portswigger.net/web-security/prototype-pollution/client-side/lab-prototype-pollution-dom-xss-via-client-side-prototype-pollution
---

# Lab 2: DOM XSS via client-side prototype pollution

Primero ejecutamos lo siguiente:

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

Y luego modificamos la URL:

<figure><img src="../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

Se puede apreciar el retorno de `foo:bar`

Así que procedemos analizar el código fuente:

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

La linea susceptible a prototype:

```
let config = {params: deparam(new URL(location).searchParams.toString())};
```

Y por lo anterior, se procede a utilizar la siguiente carga:&#x20;

```
?__proto__[transport_url]=foo
```

<figure><img src="../.gitbook/assets/image (10) (1).png" alt=""><figcaption></figcaption></figure>

En la evidencia previa, se puede apreciar el src apuntando a foo.

Modificamos nuestro payload para obtener un XSS:

<figure><img src="../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>
