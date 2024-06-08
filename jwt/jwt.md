# ¿JWT?

## <mark style="color:red;">¿Qué son los JWTs?</mark>

JSON Web Tokens (JWT) son un estándar abierto (RFC 7519) que define una manera compacta y autocontenida de transmitir información de forma segura entre partes como un objeto JSON. Esta información puede ser verificada y confiable porque está firmada digitalmente. Los JWT se utilizan comúnmente para autenticación y autorización en aplicaciones web.

## <mark style="color:red;">Formato de un JWT</mark>

Un JWT consta de tres partes separadas por puntos (`.`): el encabezado (header), el payload y la firma (signature). Cada una de estas partes es una cadena codificada en Base64Url.

1.  <mark style="color:red;">**Encabezado (Header)**</mark><mark style="color:red;">:</mark> El encabezado típicamente consta de dos partes: el tipo de token, que es JWT, y el algoritmo de firma que se utiliza, como HMAC SHA256 o RSA.

    ```json
    {
      "alg": "HS256",
      "typ": "JWT"
    }
    ```

    Este JSON se codifica en Base64Url para formar la primera parte del JWT.
2.  <mark style="color:red;">**Payload**</mark><mark style="color:red;">:</mark> El payload contiene las declaraciones (claims). Las claims son declaraciones sobre una entidad (generalmente, el usuario) y datos adicionales. Hay tres tipos de claims:

    * <mark style="color:red;">**Registered claims**</mark><mark style="color:red;">:</mark> Claims predefinidos que no son obligatorios pero son recomendados, como `iss` (emisor), `exp` (expiración), `sub` (sujeto), `aud` (audiencia).
    * <mark style="color:red;">**Public claims**</mark><mark style="color:red;">:</mark> Claims personalizados que se utilizan para compartir información entre partes que se han acordado utilizar estas claims y sus significados.
    * <mark style="color:red;">**Private claims**</mark><mark style="color:red;">:</mark> Claims personalizados creados para compartir información entre partes que han acordado utilizar estos claims.

    ```json
    {
      "sub": "1234567890",
      "name": "John Doe",
      "admin": true
    }
    ```

    Este JSON se codifica en Base64Url para formar la segunda parte del JWT.
3.  <mark style="color:red;">**Firma (Signature)**</mark><mark style="color:red;">:</mark> Para crear la firma, se toma el encabezado codificado, el payload codificado, una clave secreta y el algoritmo especificado en el encabezado, y se firma. Por ejemplo, si se utiliza HMAC SHA256, la firma se crea de la siguiente manera:

    ```scss
    HMACSHA256(
      base64UrlEncode(header) + "." + base64UrlEncode(payload),
      secret
    )
    ```

    La firma se utiliza para verificar que el mensaje no haya sido alterado y, en el caso de tokens firmados con un par de claves públicas/privadas, también se puede verificar el emisor del JWT.

## <mark style="color:red;">Ejemplo de un JWT</mark>

Un JWT típico puede verse así:

{% code overflow="wrap" %}
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```
{% endcode %}

## <mark style="color:red;">JWT vs JWS vs JWE</mark>

* <mark style="color:red;">**JWT (JSON Web Token)**</mark><mark style="color:red;">:</mark> Es un estándar que define una manera compacta y autocontenida de transmitir información de forma segura entre partes como un objeto JSON. Puede estar firmado (JWS) o cifrado (JWE).
* <mark style="color:red;">**JWS (JSON Web Signature)**</mark><mark style="color:red;">:</mark> Es un objeto JSON que está firmado digitalmente utilizando un algoritmo de firma especificado. Cuando un JWT está firmado, se llama JWS. Este asegura la integridad de los datos y la autenticidad del emisor.
* <mark style="color:red;">**JWE (JSON Web Encryption)**</mark><mark style="color:red;">:</mark> Es un objeto JSON que está cifrado. JWE protege la confidencialidad del contenido al cifrar el JWT. Esto asegura que solo las partes autorizadas puedan leer el contenido del token.

### <mark style="color:red;">Diferencias Clave</mark>

* <mark style="color:red;">**JWT**</mark><mark style="color:red;">:</mark> Es un término general que puede referirse a un JWS o un JWE. La especificación del JWT abarca ambos tipos de tokens, aquellos que están firmados (JWS) y aquellos que están cifrados (JWE).
* <mark style="color:red;">**JWS**</mark><mark style="color:red;">:</mark> Se centra en la firma y la verificación de la integridad y la autenticidad del token. La información dentro del token es legible por cualquier persona que posea el token, pero puede ser verificada para asegurar que no ha sido alterada.
* <mark style="color:red;">**JWE**</mark><mark style="color:red;">:</mark> Se centra en la confidencialidad de los datos dentro del token. La información es cifrada y solo puede ser leída por las partes autorizadas que posean la clave de descifrado.

## <mark style="color:red;">Conclusión</mark>

Los JWTs son una herramienta poderosa y flexible para la autenticación y autorización en aplicaciones modernas, permitiendo la transmisión segura de información entre partes. Entender la diferencia entre JWT, JWS y JWE es crucial para implementar sistemas de autenticación y autorización robustos y seguros. Mientras que JWS proporciona integridad y autenticidad, JWE añade una capa adicional de confidencialidad al cifrar los datos transmitidos.
