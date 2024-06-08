# ¿Server-side template injection?

## <mark style="color:red;">¿Qué es la Inyección de Plantillas del Lado del Servidor (SSTI)?</mark>

La inyección de plantillas del lado del servidor (Server-Side Template Injection, SSTI) es una vulnerabilidad que permite a un atacante inyectar código malicioso en una plantilla del servidor que luego es interpretada y ejecutada por el motor de plantillas del servidor. Esto puede llevar a la ejecución remota de código, robo de datos, y compromisos del sistema. SSTI es particularmente peligrosa debido a su potencial para convertir una simple vulnerabilidad de inyección en una completa toma de control del servidor.

## <mark style="color:red;">¿Cómo Funciona la SSTI?</mark>

La SSTI ocurre cuando una aplicación web incorpora entradas del usuario sin la debida validación y sanitización en una plantilla del lado del servidor. Los motores de plantillas permiten la inserción dinámica de contenido en las páginas web, y cuando estos motores no manejan adecuadamente las entradas del usuario, se pueden introducir inyecciones maliciosas.

### <mark style="color:red;">**Ejemplo de Código Vulnerable**</mark>

Consideremos un ejemplo en Python utilizando el motor de plantillas Jinja2:

```python
from flask import Flask, request, render_template_string

app = Flask(__name__)

@app.route('/hello')
def hello():
    name = request.args.get('name', 'World')
    template = '<h1>Hello %s!</h1>' % name
    return render_template_string(template)
```

En este ejemplo, la entrada del usuario (`name`) se inserta directamente en la plantilla HTML sin ninguna validación, lo que puede permitir a un atacante inyectar código malicioso.

### <mark style="color:red;">Ejemplo de Explotación de SSTI</mark>

Si un atacante proporciona la siguiente entrada:

```
{{ 7*7 }}
```

La plantilla renderizada será:

```html
<h1>Hello 49!</h1>
```

Esto demuestra que el código proporcionado por el usuario es evaluado por el motor de plantillas Jinja2.

Una explotación más avanzada podría incluir:

```arduino
{{ config.items() }}
```

Lo que permitiría al atacante acceder a la configuración de la aplicación.

## <mark style="color:red;">Impacto de la SSTI</mark>

El impacto de la explotación de SSTI puede ser severo, incluyendo:

1. <mark style="color:red;">**Ejecución Remota de Código (RCE)**</mark><mark style="color:red;">:</mark> Un atacante puede ejecutar comandos arbitrarios en el servidor.
2. <mark style="color:red;">**Robo de Datos Sensibles**</mark><mark style="color:red;">:</mark> Acceso a variables de entorno, configuraciones, y datos confidenciales.
3. <mark style="color:red;">**Compromiso del Sistema**</mark><mark style="color:red;">:</mark> Escalar privilegios y comprometer el servidor.
4. <mark style="color:red;">**Destrucción de Datos**</mark><mark style="color:red;">:</mark> Modificar o eliminar datos en el servidor.
