# Salesforce - Create

El conector 'Create' de Salesforce en MuleSoft es una herramienta poderosa que permite a los desarrolladores crear nuevos registros en los objetos de Salesforce mediante flujos de integración. Este conector es esencial para automatizar y simplificar procesos comerciales que requieren la creación de datos en la plataforma Salesforce. Aquí te proporciono una explicación general:

**Funcionalidad Básica:**
La operación 'Create' del conector Salesforce en MuleSoft se utiliza para insertar nuevos registros en un objeto específico dentro de Salesforce. Puedes pensar en ello como la acción de agregar una nueva entrada a una tabla en una base de datos, pero en el contexto de Salesforce.

**Configuración Requerida:**
Al utilizar la operación 'Create', se deben proporcionar ciertos detalles clave para garantizar la correcta creación del registro. Estos detalles incluyen:

- **Objeto de Destino:** Debes especificar el objeto de Salesforce en el cual se creará el nuevo registro (por ejemplo, 'Account', 'Contact', 'Opportunity').
  
- **Datos del Registro:** Se deben proporcionar los valores para los campos obligatorios y, opcionalmente, para aquellos campos que desees completar al crear el registro.

**Ejemplo Práctico:**
Supongamos que estás construyendo un flujo de integración que captura datos de un formulario web y necesita crear un nuevo contacto en Salesforce con esos datos. Utilizarías la operación 'Create' para lograr esto, mapeando los campos del formulario a los campos correspondientes del objeto 'Contact' en Salesforce.

**Consideraciones Adicionales:**
- **Manejo de Errores:** Es fundamental incluir mecanismos para manejar posibles errores al intentar crear un registro. MuleSoft ofrece capacidades robustas de manejo de errores que permiten controlar escenarios inesperados.

- **Seguridad:** Asegúrate de contar con las credenciales y permisos adecuados en Salesforce para realizar operaciones de creación de registros.

En resumen, el conector 'Create' de Salesforce en MuleSoft facilita la tarea de incorporar nuevos datos a Salesforce de manera automatizada y eficiente, lo que contribuye a la agilidad y eficacia de los procesos de negocio.

[Documentación](https://docs.mulesoft.com/salesforce-connector/latest/)

---

# Salesforce - On Modified Object

El conector 'On Modified Object' de Salesforce es una operación que detecta cambios en un objeto específico en Salesforce. Cuando se modifica un objeto, esta operación se activa y trae los cambios.

Por ejemplo, si tienes un objeto con 200 campos y se modifica uno de ellos, la operación 'On Modified Object' detectará este cambio y lo traerá¹. Sin embargo, actualmente, el conector no soporta especificar qué campos quieres recuperar¹. Esto significa que, aunque solo estés interesado en los cambios de un campo específico, la operación traerá los cambios en todos los campos del objeto.

Hay sugerencias para que el conector 'On Modified Object' soporte disparadores a nivel de campo, lo que permitiría filtrar disparadores innecesarios³. Por ejemplo, podrías configurar la operación 'On Modified Object' para un objeto 'Cuenta' y querer que se active solo si se actualizan campos específicos.

---

# File - Write

En MuleSoft, el conector 'Write' del tipo File es parte de las operaciones básicas para interactuar con archivos en el sistema de archivos local o en ubicaciones accesibles desde tu aplicación. A continuación, proporciono una explicación general sobre cómo se utiliza este conector:

**Funcionalidad Básica:**
La operación 'Write' del conector File se utiliza para escribir datos en un archivo en una ubicación específica del sistema de archivos. Puedes utilizar esta operación para crear nuevos archivos o sobrescribir el contenido de archivos existentes.

**Configuración Requerida:**
Al utilizar la operación 'Write', debes configurar algunos elementos esenciales:

1. **Ubicación del Archivo:** Especifica la ruta completa o relativa del archivo en el que deseas escribir. Esto incluye el nombre del archivo y su extensión.

2. **Datos a Escribir:** Proporciona los datos que deseas escribir en el archivo. Puedes ingresar directamente el contenido o mapearlo desde otra fuente en tu flujo de integración.

**Ejemplo Práctico:**
Imaginemos que en tu flujo de integración, deseas crear un archivo de texto llamado "output.txt" en una carpeta específica. Puedes usar la operación 'Write' para tomar los datos de tu flujo y escribirlos en ese archivo.

```xml
<file:write path="output.txt" content="#[payload]" />
```

En este ejemplo, `payload` representa los datos que deseas escribir en el archivo. Puedes adaptar la configuración según tus necesidades específicas.

**Consideraciones Adicionales:**
- **Manejo de Errores:** Asegúrate de incluir lógica de manejo de errores para abordar posibles problemas al escribir en el archivo, como falta de permisos, ubicación no válida, etc.

- **Parámetros Avanzados:** Dependiendo de tus necesidades, el conector 'Write' podría tener parámetros adicionales, como opciones de codificación, configuración de permisos, entre otros.

- **Seguridad:** Asegúrate de tener los permisos necesarios para escribir en la ubicación especificada y ten en cuenta la seguridad de los datos que estás escribiendo.

En resumen, el conector 'Write' del tipo File en MuleSoft proporciona una forma sencilla pero potente de interactuar con archivos en el sistema de archivos, facilitando la creación y actualización de archivos desde tus flujos de integración.

---

# Salesforce - Query

El conector `Query` de Salesforce en MuleSoft se utiliza para realizar consultas SOQL (Salesforce Object Query Language) a la base de datos de Salesforce y recuperar datos específicos de los objetos. A continuación, proporciono una explicación general sobre cómo se utiliza este conector:

**Funcionalidad Básica:**
La operación `Query` del conector Salesforce se emplea para ejecutar consultas SOQL y recuperar registros que cumplan con los criterios especificados en la consulta.

**Configuración Requerida:**
Para utilizar la operación `Query`, necesitas configurar los siguientes elementos:

1. **Consulta SOQL:** Define la consulta SOQL que determinará qué registros se recuperarán. La consulta puede incluir condiciones, filtrado y ordenación.

2. **Objeto de Salesforce:** Indica el objeto de Salesforce del cual deseas recuperar los registros. Por ejemplo, puedes realizar consultas en objetos estándar como 'Account' o en objetos personalizados.

**Ejemplo Práctico:**
Supongamos que deseas recuperar información sobre todas las cuentas ('Account') en Salesforce cuyo campo 'Type' sea igual a 'Customer'. Puedes utilizar la operación `Query` de la siguiente manera:

```xml
<salesforce:query config-ref="Salesforce_Config" query="SELECT Id, Name FROM Account WHERE Type = 'Customer'" />
```

En este ejemplo:
- `Salesforce_Config` hace referencia a la configuración del conector Salesforce que previamente has definido.
- La consulta SOQL selecciona los campos 'Id' y 'Name' de las cuentas donde el campo 'Type' es igual a 'Customer'.

**Consideraciones Adicionales:**
- **Manejo de Resultados:** Después de ejecutar la consulta, los resultados se almacenan generalmente en la carga útil (`payload`) del mensaje Mule. Puedes procesar estos resultados según tus necesidades en el flujo de integración.

- **Parametrización Dinámica:** Puedes parametrizar dinámicamente la consulta SOQL según las variables o datos en tu flujo.

- **Seguridad:** Asegúrate de que las credenciales y configuraciones de autenticación en la configuración del conector Salesforce sean correctas y seguras.

- **Manejo de Errores:** Implementa lógica de manejo de errores para abordar posibles problemas al ejecutar la consulta, como errores de autenticación o consultas mal formadas.

El conector `Query` de Salesforce en MuleSoft es esencial para integraciones que involucran la recuperación de datos específicos desde Salesforce para su procesamiento posterior en el flujo de integración.

---

# Salesforce - Update

El conector `Update` de Salesforce en MuleSoft se utiliza para actualizar registros específicos en la base de datos de Salesforce. A continuación, proporciono una explicación general sobre cómo se utiliza este conector:

**Funcionalidad Básica:**
La operación `Update` del conector Salesforce permite modificar registros existentes en Salesforce. Puedes actualizar uno o varios campos de un registro en función de ciertos criterios, como el ID del registro.

**Configuración Requerida:**
Para utilizar la operación `Update`, necesitas configurar los siguientes elementos:

1. **Objeto de Salesforce:** Indica el objeto de Salesforce al que pertenecen los registros que deseas actualizar (por ejemplo, 'Account', 'Contact', etc.).

2. **Registros a Actualizar:** Define los registros que deseas actualizar. Esto suele incluir el ID del registro y los campos que deseas modificar.

**Ejemplo Práctico:**
Supongamos que deseas actualizar el campo 'Status' de una cuenta en Salesforce donde el 'Account Number' es igual a cierto valor. Puedes utilizar la operación `Update` de la siguiente manera:

```xml
<salesforce:update config-ref="Salesforce_Config" type="Account">
    <salesforce:criteria>
        <salesforce:field column="AccountNumber" operator="equals" value="12345" />
    </salesforce:criteria>
    <salesforce:values>
        <salesforce:field column="Status" value="Active" />
    </salesforce:values>
</salesforce:update>
```

En este ejemplo:
- `Salesforce_Config` hace referencia a la configuración del conector Salesforce que previamente has definido.
- Se especifica el objeto 'Account'.
- En la sección `<salesforce:criteria>`, se define el criterio de búsqueda basado en el 'Account Number'.
- En `<salesforce:values>`, se especifica el campo 'Status' y el nuevo valor que se asignará.

**Consideraciones Adicionales:**
- **Manejo de Errores:** Implementa lógica de manejo de errores para abordar posibles problemas al intentar actualizar registros, como falta de permisos o campos no modificables.

- **Seguridad:** Asegúrate de que las credenciales y configuraciones de autenticación en la configuración del conector Salesforce sean correctas y seguras.

- **Registro Único vs. Múltiples Registros:** Puedes utilizar la operación `Update` para actualizar registros individuales o múltiples, dependiendo de tus necesidades.

El conector `Update` de Salesforce en MuleSoft es crucial para integraciones que requieren la capacidad de modificar información específica en Salesforce y mantener la coherencia de los datos.

---

# Salesforce - Upsert

El conector `Upsert` de Salesforce en MuleSoft se utiliza para actualizar registros existentes o insertar nuevos registros en función de un campo externo (no el ID) proporcionado en los datos de entrada. Aquí tienes una explicación general sobre cómo se utiliza:

**Funcionalidad Básica:**
La operación `Upsert` combina las funcionalidades de 'Update' e 'Insert'. Evalúa si ya existe un registro con el valor proporcionado en el campo externo especificado. Si existe, actualiza ese registro; si no existe, crea un nuevo registro.

**Configuración Requerida:**
Para utilizar la operación `Upsert`, necesitas configurar los siguientes elementos:

1. **Objeto de Salesforce:** Indica el objeto de Salesforce al que pertenecen los registros que deseas actualizar o insertar.

2. **Campo Externo:** Especifica el campo en el que se basará la operación de `Upsert`. Este campo debe ser único y no ser el ID del registro.

**Ejemplo Práctico:**
Supongamos que deseas actualizar o insertar una cuenta en Salesforce utilizando el 'Account Number' como el campo externo. Puedes utilizar la operación `Upsert` de la siguiente manera:

```xml
<salesforce:upsert config-ref="Salesforce_Config" type="Account" externalIdField="AccountNumber">
    <salesforce:values>
        <salesforce:field column="AccountNumber" value="12345" />
        <salesforce:field column="Status" value="Active" />
        <!-- Otros campos y valores -->
    </salesforce:values>
</salesforce:upsert>
```

En este ejemplo:
- `Salesforce_Config` hace referencia a la configuración del conector Salesforce que previamente has definido.
- Se especifica el objeto 'Account'.
- `externalIdField="AccountNumber"` indica que el campo 'AccountNumber' se utilizará como el campo externo para la operación `Upsert`.
- En `<salesforce:values>`, se proporcionan los valores que se actualizarán o insertarán.

**Consideraciones Adicionales:**
- **Manejo de Errores:** Implementa lógica de manejo de errores para abordar posibles problemas al intentar realizar la operación de `Upsert`, como campos externos duplicados.

- **Seguridad:** Asegúrate de que las credenciales y configuraciones de autenticación en la configuración del conector Salesforce sean correctas y seguras.

- **Campo Externo Único:** El campo externo debe ser único en Salesforce para garantizar la correcta identificación de los registros.

La operación `Upsert` es útil en situaciones donde no estás seguro de si el registro ya existe y deseas actualizarlo si es así o crearlo si no lo está, todo basado en un campo externo específico.