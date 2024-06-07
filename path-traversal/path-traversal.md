# ¿Path Traversal?

## <mark style="color:red;">¿Qué es el Path Traversal?</mark>

El Path Traversal, también conocido como Directory Traversal, es una vulnerabilidad de seguridad que permite a un atacante acceder a archivos y directorios que están fuera del directorio raíz de la aplicación web. Esta vulnerabilidad ocurre cuando la aplicación no valida o sanitiza correctamente las entradas del usuario, permitiendo que el atacante manipule las rutas de archivo para acceder a ubicaciones restringidas del sistema de archivos del servidor.

## <mark style="color:red;">Funcionamiento del Path Traversal</mark>

El ataque de Path Traversal se basa en la manipulación de las rutas de archivo usando secuencias como `../` para retroceder en la estructura de directorios. Por ejemplo, si una aplicación permite a los usuarios acceder a archivos a través de una URL como `http://example.com/view?file=report.txt`, un atacante puede intentar acceder a otros archivos del sistema proporcionando rutas maliciosas como `../../etc/passwd`.

### <mark style="color:red;">**Pasos del ataque:**</mark>

1. <mark style="color:red;">**Identificación de una entrada vulnerable**</mark><mark style="color:red;">:</mark> El atacante encuentra un campo de entrada (por ejemplo, un parámetro en la URL) que se utiliza para especificar rutas de archivo.
2. <mark style="color:red;">**Manipulación de la ruta**</mark><mark style="color:red;">:</mark> El atacante modifica el valor del campo de entrada para incluir secuencias de Path Traversal como `../`.
3. <mark style="color:red;">**Acceso a archivos arbitrarios**</mark><mark style="color:red;">:</mark> El servidor interpreta la ruta manipulada y permite al atacante acceder a archivos fuera del directorio permitido.

## <mark style="color:red;">Ejemplo de Path Traversal</mark>

Considere una aplicación web con el siguiente código en PHP que lee el contenido de un archivo:

```php
<?php
$file = $_GET['file'];
include("/var/www/html/" . $file);
?>
```

Un atacante podría explotar esta vulnerabilidad proporcionando la siguiente URL:

```http
http://example.com/view?file=../../../../etc/passwd
```

En este ejemplo, el servidor interpretará la ruta como `/var/www/html/../../../../etc/passwd`, lo que resultará en la lectura del archivo `/etc/passwd`, que contiene información sensible del sistema.

## <mark style="color:red;">Lectura de Archivos Arbitrarios mediante Path Traversal</mark>

El acceso a archivos arbitrarios a través de Path Traversal permite a un atacante leer archivos sensibles del sistema, como archivos de configuración, archivos de contraseñas, y otros datos críticos. Esto puede tener graves implicaciones de seguridad, incluyendo la exposición de credenciales, configuraciones del sistema, y otra información confidencial.

### <mark style="color:red;">**Ejemplos de archivos críticos que podrían ser leídos:**</mark>

* `/etc/passwd`: Contiene información de usuarios en sistemas Unix/Linux.
* `/etc/shadow`: Contiene hashes de contraseñas en sistemas Unix/Linux.
* `C:\Windows\system32\config\SAM`: Contiene información de cuentas de usuario en sistemas Windows.
* Archivos de configuración de la aplicación (`config.php`, `.env`, etc.).

## <mark style="color:red;">Mitigación del Path Traversal</mark>

Para proteger las aplicaciones contra ataques de Path Traversal, se deben implementar varias estrategias de mitigación:

1. <mark style="color:red;">**Validación y Sanitización de Entradas**</mark><mark style="color:red;">:</mark> Validar y sanitizar todas las entradas del usuario para asegurarse de que solo se aceptan rutas permitidas. Por ejemplo, se puede verificar que la ruta solicitada no contenga secuencias como `../`.
2. <mark style="color:red;">**Uso de Rutas Absolutas y Permitidas**</mark><mark style="color:red;">:</mark> Limitar el acceso a una lista específica de rutas permitidas. En lugar de permitir rutas arbitrarias proporcionadas por el usuario, se debe trabajar con rutas absolutas que estén controladas y verificadas por el servidor.
3. <mark style="color:red;">**Configuración de Permisos Adecuados**</mark><mark style="color:red;">:</mark> Configurar el sistema de archivos del servidor de manera que los archivos sensibles no sean accesibles a través del servidor web. Esto incluye ajustar los permisos de archivos y directorios para limitar el acceso.
4. <mark style="color:red;">**Uso de Funciones Seguras**</mark><mark style="color:red;">:</mark> Utilizar funciones seguras y bibliotecas que proporcionen protección contra Path Traversal. Por ejemplo, en PHP, se puede usar `realpath()` para resolver y validar rutas de archivos.

## <mark style="color:red;">Ejemplo de Mitigación en PHP</mark>

Un ejemplo de cómo mitigar Path Traversal en PHP utilizando `realpath()`:

```php
<?php
$base_dir = "/var/www/html/";
$file = $_GET['file'];
$requested_path = realpath($base_dir . $file);

if (strpos($requested_path, $base_dir) !== 0 || !$requested_path) {
    die("Acceso denegado.");
}

include($requested_path);
?>
```

En este ejemplo, `realpath()` resuelve la ruta completa del archivo solicitado y se verifica que la ruta resultante esté dentro del directorio base permitido.

## <mark style="color:red;">Conclusión</mark>

El Path Traversal es una vulnerabilidad crítica que puede permitir a un atacante acceder a archivos y datos sensibles en el servidor. Implementar una adecuada validación y sanitización de entradas, así como el uso de funciones seguras y la configuración correcta de permisos, es esencial para proteger las aplicaciones contra este tipo de ataques. La comprensión y la mitigación de Path Traversal son fundamentales para asegurar la integridad y la confidencialidad de los datos en una aplicación web.
