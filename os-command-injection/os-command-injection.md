# ¿OS Command Injection?

## <mark style="color:red;">¿Qué es la Inyección de Comandos del Sistema Operativo?</mark>

La inyección de comandos del sistema operativo (OS Command Injection) en aplicaciones web es una vulnerabilidad que permite a un atacante ejecutar comandos arbitrarios en el sistema operativo del servidor que aloja la aplicación vulnerable. Esto sucede cuando la aplicación pasa entradas del usuario sin la debida validación y sanitización directamente a un shell del sistema operativo. La inyección de comandos puede resultar en la toma de control del servidor, la exfiltración de datos sensibles y la interrupción de servicios.

## <mark style="color:red;">¿Cómo Funciona la Inyección de Comandos?</mark>

La inyección de comandos se aprovecha cuando una aplicación web utiliza entradas del usuario como parte de un comando del sistema operativo sin realizar una validación y sanitización adecuadas. Los atacantes pueden insertar comandos adicionales utilizando caracteres especiales que el sistema operativo reconoce para encadenar comandos.

### <mark style="color:red;">**Ejemplo de Código Vulnerable en PHP**</mark>

Considere el siguiente script PHP, que toma una entrada del usuario y la pasa a un comando del sistema operativo:

```php
<?php
if (isset($_GET['ip'])) {
    $ip = $_GET['ip'];
    $output = shell_exec("ping -c 4 " . $ip);
    echo "<pre>$output</pre>";
}
?>
```

En este código, la entrada del usuario (`ip`) se concatena directamente con el comando `ping` sin ninguna validación. Un atacante puede explotar esta vulnerabilidad inyectando comandos adicionales.

### <mark style="color:red;">Ejemplo de Explotación</mark>

Si un atacante proporciona el siguiente input:

```bash
127.0.0.1; ls
```

El comando resultante que se ejecutará será:

```sh
ping -c 4 127.0.0.1; ls
```

El sistema operativo ejecutará ambos comandos: el ping y el `ls`, lo que permitirá al atacante ver la lista de archivos en el directorio actual.

Usando `curl`, la solicitud se vería así:

```sh
curl "http://victima.com/?ip=127.0.0.1; ls"
```

## <mark style="color:red;">Impacto de la Inyección de Comandos</mark>

El impacto de la inyección de comandos puede ser extremadamente severo, permitiendo al atacante:

* <mark style="color:red;">**Ejecutar comandos arbitrarios**</mark><mark style="color:red;">:</mark> Cualquier comando que el servidor pueda ejecutar puede ser ejecutado por el atacante.
* <mark style="color:red;">**Escalar privilegios**</mark><mark style="color:red;">:</mark> El atacante puede intentar elevar sus privilegios en el sistema.
* <mark style="color:red;">**Acceder a datos confidenciales**</mark><mark style="color:red;">:</mark> Leer, modificar o eliminar archivos en el sistema.
* <mark style="color:red;">**Interrumpir servicios**</mark><mark style="color:red;">:</mark> Apagar servicios, eliminar archivos críticos, etc.
