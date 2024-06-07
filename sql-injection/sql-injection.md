---
cover: ../.gitbook/assets/hacker.png
coverY: 0
---

# ¿SQL Injection?

## <mark style="color:red;">¿Qué es una inyección SQL (SQLi)?</mark>

La inyección SQL (SQLi) es una de las vulnerabilidades de seguridad web más críticas y comunes. Permite a un atacante manipular las consultas que una aplicación web envía a su base de datos. Esta manipulación puede permitirle acceder a datos a los que normalmente no tendría acceso, como información de otros usuarios o datos sensibles del sistema. Los atacantes también pueden modificar, eliminar o insertar datos, alterando así la integridad de la información y el funcionamiento de la aplicación.

## <mark style="color:red;">Impacto de un ataque de inyección SQL</mark>

Los efectos de una inyección SQL exitosa pueden ser devastadores. Los atacantes pueden:

* Obtener acceso a datos confidenciales como contraseñas, números de tarjetas de crédito e información personal de usuarios.
* Manipular o borrar datos críticos, lo que puede llevar a la corrupción de la base de datos.
* Realizar ataques de denegación de servicio (DoS) interrumpiendo el funcionamiento normal de la aplicación.

Las consecuencias de estos ataques incluyen pérdidas financieras, daños a la reputación, sanciones legales y regulatorias, y la pérdida de confianza de los clientes.

## <mark style="color:red;">Cómo detectar vulnerabilidades de inyección SQL</mark>

Detectar vulnerabilidades de inyección SQL implica un enfoque exhaustivo y sistemático. Algunas técnicas incluyen:

* <mark style="color:red;">**Inyección de caracteres especiales:**</mark> Introducir caracteres como `'` o `"` para ver si la aplicación genera errores o respuestas inesperadas.
* <mark style="color:red;">**Pruebas de lógica booleana:**</mark> Utilizar condiciones como `OR 1=1` y `OR 1=2` para identificar diferencias en las respuestas de la aplicación.
* <mark style="color:red;">**Inyección de carga útil (payload):**</mark> Usar comandos SQL específicos para alterar la lógica de la consulta original y observar los resultados.
* <mark style="color:red;">**Pruebas de tiempo:**</mark> Emplear consultas que generen retrasos intencionales (por ejemplo, `SLEEP(5)`) para ver si la respuesta de la aplicación se retrasa.
* <mark style="color:red;">**Pruebas fuera de banda (OAST):**</mark> Enviar cargas útiles que desencadenen interacciones de red fuera de banda para detectar respuestas no directas de la aplicación.

El uso de herramientas automatizadas como Burp Suite puede facilitar la identificación de estas vulnerabilidades, aunque siempre es recomendable complementarlo con análisis manuales.

## <mark style="color:red;">Puntos de inyección SQL en las consultas</mark>

Si bien la mayoría de las inyecciones SQL se encuentran en la cláusula WHERE de las consultas SELECT, es crucial entender que pueden ocurrir en cualquier parte de una consulta SQL. Algunos lugares comunes incluyen:

* <mark style="color:red;">**Consultas UPDATE:**</mark> Dentro de los valores actualizados o en la cláusula WHERE.
* <mark style="color:red;">**Consultas INSERT:**</mark> Dentro de los valores que se insertan en la base de datos.
* <mark style="color:red;">**Consultas SELECT:**</mark> En los nombres de las tablas o columnas, y en la cláusula ORDER BY.
* <mark style="color:red;">**Procedimientos almacenados:**</mark> En los parámetros de entrada de procedimientos almacenados que no están adecuadamente sanitizados.

## <mark style="color:red;">Mitigación y prevención de inyecciones SQL</mark>

Para protegerse contra las inyecciones SQL, es fundamental implementar medidas de seguridad robustas:

* <mark style="color:red;">**Uso de consultas parametrizadas:**</mark> Evitar la concatenación de cadenas en las consultas SQL y utilizar siempre parámetros preparados.
* <mark style="color:red;">**Validación y escape de entrada:**</mark> Validar y escapar adecuadamente toda la entrada del usuario para asegurar que no contenga caracteres maliciosos.
* <mark style="color:red;">**Principio de privilegios mínimos:**</mark> Asegurar que las cuentas de la base de datos utilizadas por la aplicación tengan los privilegios mínimos necesarios.
* <mark style="color:red;">**Monitoreo y registros:**</mark> Implementar sistemas de monitoreo y registrar las actividades sospechosas para detectar y responder rápidamente a posibles ataques.

La comprensión y aplicación de estas prácticas es esencial para fortalecer la seguridad de las aplicaciones web y proteger los datos sensibles contra accesos no autorizados.
