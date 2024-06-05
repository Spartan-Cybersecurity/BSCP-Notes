# Lab 1: Username enumeration via different responses

Para este ejercicio estaremos utilizando los dos siguientes diccionarios:

{% embed url="https://portswigger.net/web-security/authentication/auth-lab-usernames" %}

{% embed url="https://portswigger.net/web-security/authentication/auth-lab-passwords" %}

Inicialmente, se procede capturando la peticion encargada de realizar el login:

```
POST /login HTTP/2
Host: 0af9002e04d842e684fcb30100340063.web-security-academy.net
Cookie: session=aAzgywE45eRqUeCKg6T49EAcNX9mmXl5
Content-Length: 27
Cache-Control: max-age=0
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Upgrade-Insecure-Requests: 1
Origin: https://0af9002e04d842e684fcb30100340063.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0af9002e04d842e684fcb30100340063.web-security-academy.net/login
Accept-Encoding: gzip, deflate, br
Priority: u=0, i

username=test&password=test
```

La peticion previa es la encargada de realizar el login o inicio de sesion:

Y al tener el usuario o la contraseña incorrecta responde asi:

```html
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 3140

<!DOCTYPE html>
<html>
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labs.css rel=stylesheet>
        <title>Username enumeration via different responses</title>
    </head>
    <body>
        <script src="/resources/labheader/js/labHeader.js"></script>
        <div id="academyLabHeader">
            <section class='academyLabBanner'>
                <div class=container>
                    <div class=logo></div>
                        <div class=title-container>
                            <h2>Username enumeration via different responses</h2>
                            <a class=link-back href='https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses'>
                                Back&nbsp;to&nbsp;lab&nbsp;description&nbsp;
                                <svg version=1.1 id=Layer_1 xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' x=0px y=0px viewBox='0 0 28 30' enable-background='new 0 0 28 30' xml:space=preserve title=back-arrow>
                                    <g>
                                        <polygon points='1.4,0 0,1.2 12.6,15 0,28.8 1.4,30 15.1,15'></polygon>
                                        <polygon points='14.3,0 12.9,1.2 25.6,15 12.9,28.8 14.3,30 28,15'></polygon>
                                    </g>
                                </svg>
                            </a>
                        </div>
                        <div class='widgetcontainer-lab-status is-notsolved'>
                            <span>LAB</span>
                            <p>Not solved</p>
                            <span class=lab-status-icon></span>
                        </div>
                    </div>
                </div>
            </section>
        </div>
        <div theme="">
            <section class="maincontainer">
                <div class="container is-page">
                    <header class="navigation-header">
                        <section class="top-links">
                            <a href=/>Home</a><p>|</p>
                            <a href="/my-account">My account</a><p>|</p>
                        </section>
                    </header>
                    <header class="notification-header">
                    </header>
                    <h1>Login</h1>
                    <section>
                        <p class=is-warning>Invalid username</p>
                        <form class=login-form method=POST action="/login">
                            <label>Username</label>
                            <input required type=username name="username" autofocus>
                            <label>Password</label>
                            <input required type=password name="password">
                            <button class=button type=submit> Log in </button>
                        </form>
                    </section>
                </div>
            </section>
            <div class="footer-wrapper">
            </div>
        </div>
    </body>
</html>

```

Una palabra o frase clave es: `Invalid username`

Teniendo en cuenta lo anterior, enviamos la peticion al intruder en modo sniper y utilizamos el diccionario de usuarios:

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

La configuracion para los payloads es:

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Y en settings asi:

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Al filtrar por la columna que busca por invalid username logramos identificar que el usuario apple es el unico que tiene una respuesta con un lenght mayor y no tiene la frase de "invalid username" en la respuesta:

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

La respuesta para este usuario fue:

```
Incorrect password
```

Asi que ahora cambiaremos las posiciones de ataque y enviaremos multiples intentos de inicio de sesion para el usuario apple con diferentes contraseñas:

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Y finalizamos configurando tambien el payload:

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Y los settings asi:

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

La contraseña monitor funciono y se logra apreciar en la diferencia del tamaño de la respuesta y el incorrect password:

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>
