# ¿XML external entity?

## <mark style="color:red;">¿Qué es la Inyección de Entidades Externas XML (XXE)?</mark>

La inyección de entidades externas XML (XXE) es una vulnerabilidad que se produce cuando una aplicación procesa entradas XML que incluyen referencias a entidades externas. Un atacante puede explotar esta vulnerabilidad para leer archivos locales, realizar escaneos internos de red, y en algunos casos, ejecutar código remoto o causar una denegación de servicio. XXE es una vulnerabilidad crítica porque puede permitir acceso no autorizado a información sensible y potencialmente comprometer toda la infraestructura.

## <mark style="color:red;">¿Cómo Funciona la Inyección de Entidades Externas XML?</mark>

XXE se basa en la capacidad de los parsers XML para definir entidades externas, que son esencialmente referencias a recursos externos. Un atacante puede inyectar entidades maliciosas en un documento XML que la aplicación procesa, lo que puede llevar a una serie de ataques dependiendo de cómo se manejen las entidades externas.

### <mark style="color:red;">**Ejemplo de un Documento XML Vulnerable**</mark>

Un documento XML estándar podría verse así:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE root [
    <!ENTITY test "Hello, world!">
]>
<root>&test;</root>
```

Si el parser XML de la aplicación permite la definición y expansión de entidades externas, un atacante podría manipular el documento XML para incluir una entidad que apunta a un archivo local:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE root [
    <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<root>&xxe;</root>
```

Cuando el parser procesa este XML, intentará expandir la entidad `&xxe;` y leerá el contenido del archivo `/etc/passwd`, exponiendo así información sensible.

## <mark style="color:red;">Tipos de Ataques XXE</mark>

### <mark style="color:red;">**Lectura de Archivos Locales**</mark>

Los atacantes pueden usar XXE para leer archivos locales en el servidor. Al definir una entidad externa que apunta a un archivo del sistema, pueden obtener acceso a datos confidenciales.

```xml
<!ENTITY xxe SYSTEM "file:///etc/passwd">
```

### <mark style="color:red;">**Scanning de Red**</mark>

XXE puede utilizarse para realizar un escaneo interno de la red. Mediante la definición de entidades externas que apuntan a diferentes IPs y puertos, un atacante puede descubrir servicios y sistemas internos.

```xml
<!ENTITY xxe SYSTEM "http://192.168.1.1:8080">
```

### <mark style="color:red;">**Ejecución de Código Remoto**</mark>

En casos extremos, XXE puede permitir la ejecución de código remoto si el parser XML se combina con otras vulnerabilidades presentes en el sistema.
