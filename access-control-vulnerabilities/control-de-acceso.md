# ¿Control de Acceso?

## <mark style="color:red;">¿Qué es el Control de Acceso?</mark>

El control de acceso es un mecanismo de seguridad utilizado para regular quién o qué puede ver o utilizar recursos en un entorno informático. Es un componente fundamental de la seguridad de la información y de las aplicaciones, y se implementa para asegurar que los usuarios solo tengan acceso a los datos y funciones que están autorizados a utilizar. El control de acceso ayuda a proteger la integridad, confidencialidad y disponibilidad de los datos.

## <mark style="color:red;">Tipos de Control de Acceso</mark>

### <mark style="color:red;">**Controles de Acceso Verticales**</mark>

Los controles de acceso verticales (VAC, por sus siglas en inglés) determinan qué nivel de acceso tiene un usuario a un conjunto de recursos en función de su rol o jerarquía en una organización. Este tipo de control suele implementarse en aplicaciones donde los usuarios tienen diferentes niveles de privilegios.

<mark style="color:red;">**Ejemplo**</mark><mark style="color:red;">:</mark> En un sistema de gestión de proyectos, un administrador puede tener acceso total para crear, modificar y eliminar proyectos, mientras que un usuario regular solo puede ver y comentar en los proyectos.

<mark style="color:red;">**Vulnerabilidad**</mark><mark style="color:red;">:</mark> Una falla en los controles de acceso verticales puede permitir a un usuario de bajo nivel realizar acciones que deberían estar restringidas a usuarios con mayores privilegios, comprometiendo la seguridad de la aplicación y los datos.

### <mark style="color:red;">**Controles de Acceso Horizontales**</mark>

Los controles de acceso horizontales (HAC) aseguran que los usuarios solo puedan acceder a sus propios datos y no a los datos de otros usuarios en el mismo nivel de privilegio.

<mark style="color:red;">**Ejemplo**</mark><mark style="color:red;">:</mark> En una aplicación bancaria, un cliente debe poder ver y gestionar solo sus propias cuentas y transacciones, no las de otros clientes.

<mark style="color:red;">**Vulnerabilidad**</mark><mark style="color:red;">:</mark> Una vulnerabilidad en los controles de acceso horizontales puede permitir a un usuario acceder a la información de otros usuarios, lo que resulta en una violación de la privacidad y potencialmente en fraudes.

## <mark style="color:red;">¿Por qué las Vulnerabilidades en el Control de Acceso pueden ser Críticas?</mark>

Las vulnerabilidades en el control de acceso son particularmente críticas porque pueden comprometer la lógica de negocio y permitir a los atacantes realizar acciones no autorizadas que tienen un impacto significativo en la organización. Aquí se detallan algunas razones de su criticidad:

1. <mark style="color:red;">**Compromiso de la Confidencialidad**</mark><mark style="color:red;">:</mark> Las vulnerabilidades en los controles de acceso pueden permitir el acceso no autorizado a datos sensibles, como información personal, financiera o propiedad intelectual, lo cual puede resultar en violaciones de privacidad y pérdidas financieras.
2. <mark style="color:red;">**Compromiso de la Integridad**</mark><mark style="color:red;">:</mark> Los atacantes pueden modificar o eliminar datos críticos si logran eludir los controles de acceso, afectando la integridad de los datos y la confianza en los sistemas.
3. <mark style="color:red;">**Compromiso de la Disponibilidad**</mark><mark style="color:red;">:</mark> La manipulación indebida de recursos, como la eliminación de registros importantes, puede afectar la disponibilidad de los servicios para los usuarios legítimos.
4. <mark style="color:red;">**Impacto en la Lógica de Negocio**</mark><mark style="color:red;">:</mark> Las vulnerabilidades en los controles de acceso pueden permitir a los atacantes ejecutar acciones que afectan directamente la lógica de negocio, como aprobar transacciones no autorizadas, cambiar configuraciones críticas, o realizar actividades fraudulentas.
5. <mark style="color:red;">**Cumplimiento Normativo**</mark><mark style="color:red;">:</mark> Las fallas en los controles de acceso pueden resultar en incumplimientos normativos, exponiendo a la organización a sanciones legales y reputacionales.

## <mark style="color:red;">Ejemplo de Ataque y su Impacto</mark>

Un ejemplo ilustrativo es un sistema de gestión hospitalaria donde los controles de acceso no están adecuadamente implementados. Si un empleado con acceso limitado puede eludir los controles y acceder a registros médicos de otros pacientes, esto no solo viola la privacidad del paciente sino que también puede resultar en manipulación de datos médicos, afectando la atención al paciente y exponiendo al hospital a graves consecuencias legales.

## <mark style="color:red;">Conclusión</mark>

El control de acceso es un aspecto crucial de la seguridad de las aplicaciones y sistemas. La implementación robusta de controles de acceso verticales, horizontales y dependientes del contexto es esencial para proteger los recursos de una organización y garantizar que solo los usuarios autorizados puedan acceder a ellos. Las vulnerabilidades en estos controles pueden tener consecuencias graves, tanto para la seguridad como para la lógica de negocio, por lo que es fundamental abordar estas cuestiones con la mayor diligencia y precisión posibles.

4o
