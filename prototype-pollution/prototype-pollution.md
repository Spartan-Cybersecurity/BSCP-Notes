# ¿Prototype Pollution?

La vulnerabilidad _Prototype Pollution_ es una forma de ataque en el cual un atacante puede inyectar propiedades arbitrarias en los prototipos de objetos nativos de JavaScript, lo que puede llevar a la modificación de las propiedades y comportamientos de todos los objetos que heredan de esos prototipos.

## <mark style="color:red;">¿Qué es un prototipo en JavaScript?</mark>

En JavaScript, los objetos pueden heredar propiedades y métodos de un prototipo. Cada objeto tiene un prototipo (accesible a través de `__proto__` o `Object.getPrototypeOf`), y el prototipo a su vez tiene su propio prototipo, formando una cadena de prototipos (prototype chain). Por ejemplo, si se crea un objeto vacío `{}`, este hereda de `Object.prototype`.

## <mark style="color:red;">¿Qué es Prototype Pollution?</mark>

_Prototype Pollution_ ocurre cuando un atacante puede modificar el prototipo de objetos nativos, como `Object.prototype`, `Array.prototype`, etc., añadiendo o sobrescribiendo propiedades. Esto afecta a todos los objetos que heredan de ese prototipo, lo que puede tener consecuencias devastadoras para la aplicación.

### <mark style="color:red;">Ejemplo de Prototype Pollution</mark>

Consideremos el siguiente ejemplo simplificado:

```javascript
// Código vulnerable
let params = JSON.parse('{"__proto__": {"polluted": "yes"}}');

// Uso de params en el código
let obj = {};
console.log(obj.polluted); // Output: "yes"
```

En este ejemplo, `params` se parsea desde un JSON que contiene la propiedad `__proto__`. Al asignar este objeto, se modifica el prototipo de todos los objetos, añadiendo una propiedad `polluted` con el valor `"yes"`. Ahora, cualquier objeto creado en el programa tiene una propiedad `polluted` con ese valor.

## <mark style="color:red;">¿Por qué surge la vulnerabilidad?</mark>

La vulnerabilidad de _Prototype Pollution_ puede surgir en los siguientes escenarios:

1. <mark style="color:red;">**Manipulación Insegura de Objetos**</mark><mark style="color:red;">:</mark> Cuando el código permite a los usuarios o datos externos modificar objetos directamente, sin sanitizar o validar adecuadamente las claves.
2. <mark style="color:red;">**Bibliotecas Vulnerables**</mark><mark style="color:red;">:</mark> Algunas bibliotecas pueden manejar de manera insegura la asignación de propiedades en objetos, permitiendo la contaminación de prototipos. Esto es común en bibliotecas que manipulan datos JSON, query strings, etc.
3. <mark style="color:red;">**Falta de Sanitización**</mark><mark style="color:red;">:</mark> No filtrar o validar adecuadamente las propiedades de los objetos que se construyen o modifican a partir de datos no confiables.

## <mark style="color:red;">Escenarios Comunes Donde Puede Surgir</mark>

1. <mark style="color:red;">**Aplicaciones Web**</mark><mark style="color:red;">:</mark> En aplicaciones web que procesan datos de entrada del usuario, como formularios, parámetros de URL, o datos JSON, un atacante podría enviar datos especialmente diseñados para modificar los prototipos.
2. <mark style="color:red;">**APIs**</mark><mark style="color:red;">:</mark> APIs que aceptan y procesan datos JSON de clientes pueden ser vulnerables si no sanitizan adecuadamente las propiedades de los objetos.
3. <mark style="color:red;">**Bibliotecas de Terceros**</mark><mark style="color:red;">:</mark> Usar bibliotecas de terceros que no están adecuadamente protegidas contra esta vulnerabilidad puede introducir _Prototype Pollution_ en tu aplicación.

## <mark style="color:red;">Prevención</mark>

Para prevenir la vulnerabilidad de _Prototype Pollution_, se deben seguir varias prácticas:

1. <mark style="color:red;">**Sanitización de Datos**</mark><mark style="color:red;">:</mark> Filtrar y sanitizar las propiedades de los objetos antes de procesarlos. Asegurarse de que los datos no contengan claves peligrosas como `__proto__`, `constructor`, `prototype`.
2. <mark style="color:red;">**Validación de Entradas**</mark><mark style="color:red;">:</mark> Implementar una validación estricta de los datos de entrada para asegurarse de que solo las propiedades esperadas sean permitidas.
3. <mark style="color:red;">**Uso Seguro de Bibliotecas**</mark><mark style="color:red;">:</mark> Elegir y usar bibliotecas que sean seguras y mantenerlas actualizadas para protegerse contra vulnerabilidades conocidas.
