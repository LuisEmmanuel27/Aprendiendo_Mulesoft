# Dataweave 2.0

Dataweave 2.0 es un lenguaje de transformación de datos que permite manipular, filtrar, enriquecer y transformar datos de diferentes fuentes y formatos. Dataweave 2.0 se integra con la plataforma de MuleSoft, que ofrece una solución completa para la integración y el desarrollo de aplicaciones basadas en APIs. 

Con Dataweave 2.0, se puede acceder a los datos de cualquier origen, ya sea una base de datos, un servicio web, un archivo, una cola de mensajes o cualquier otro tipo de recurso. Dataweave 2.0 también permite definir la estructura y el formato de salida de los datos, ya sea JSON, XML, CSV, Excel o cualquier otro formato soportado por MuleSoft. Dataweave 2.0 facilita la creación de transformaciones complejas y personalizadas mediante una sintaxis simple y expresiva, que se puede escribir tanto en modo gráfico como en modo textual. 

Dataweave 2.0 también ofrece funciones integradas para realizar operaciones comunes sobre los datos, como agrupar, ordenar, filtrar, mapear, reducir, unir, dividir y más. Además, Dataweave 2.0 permite crear funciones personalizadas y reutilizables para encapsular la lógica de negocio y mejorar la legibilidad y el mantenimiento del código. Dataweave 2.0 es un lenguaje poderoso y flexible que facilita la transformación de datos en el contexto de MuleSoft.

# Configuración de Propiedades

En DataWeave 2.0, la configuración de propiedades te permite establecer parámetros específicos que afectan el comportamiento de tus transformaciones. Estas propiedades permiten personalizar el entorno de ejecución de DataWeave según las necesidades de tu aplicación. Aquí hay una visión técnica de cómo puedes trabajar con la configuración de propiedades en DataWeave 2.0:

## Definición de Propiedades:

Para utilizar propiedades en DataWeave 2.0, primero las defines en tu archivo de propiedades externo o en la sección de configuración de tu archivo DataWeave. Puedes especificar propiedades como el tipo de datos predeterminado, la codificación de caracteres, etc.

**Ejemplo de Archivo de Propiedades:**
```properties
dw {
    default-input-encoding: 'UTF-8'
    default-output-encoding: 'UTF-8'
    default-exports: 'all'
}
```

## DataWeave 2.0: Referencia de Propiedades en Transformación

En DataWeave 2.0, la referencia de propiedades en transformaciones te permite acceder a los valores definidos en la configuración de propiedades. Esto brinda flexibilidad al personalizar dinámicamente el entorno de ejecución de tus transformaciones. Aquí está la explicación focalizada en cómo puedes referenciar propiedades utilizando la función `p()` y las expresiones `${}` y `#[]`:

### Utilizando `p()`:

En DataWeave, puedes utilizar la función `p()` para hacer referencia a las propiedades definidas en tu configuración. Esta función permite acceder al valor de la propiedad de manera dinámica durante la ejecución de la transformación.

**Ejemplo de Uso de `p()`:**
```dw
%dw 2.0
output application/json encoding p('dw.default-output-encoding')
---
{
  message: "¡Hola, Mundo!"
}
```

En este ejemplo, estamos utilizando `p('dw.default-output-encoding')` para obtener dinámicamente la codificación de caracteres de salida definida en las propiedades.

### Expresiones `${}` y `#[]`:

Además de `p()`, puedes utilizar las expresiones `${}` y `#[]` para referenciar propiedades en tu transformación.

**Ejemplo de Uso de `${}`:**
```dw
%dw 2.0
output application/json encoding ${p('dw.default-output-encoding')}
---
{
  message: "¡Hola, Mundo!"
}
```

En este caso, estamos utilizando `${}` para incorporar dinámicamente el valor de la propiedad de codificación de caracteres de salida en la transformación.

**Ejemplo de Uso de `#[]`:**
```dw
%dw 2.0
output application/json encoding #[
  p('dw.default-output-encoding')
]
---
{
  message: "¡Hola, Mundo!"
}
```

Aquí, `#[]` se utiliza para realizar la misma referencia dinámica a la propiedad de codificación de caracteres de salida.

### Consideraciones Importantes:

- **Dinamicidad:** Estas expresiones permiten una referencia dinámica, lo que significa que el valor de la propiedad se evalúa en tiempo de ejecución.
- **Flexibilidad:** Puedes utilizar la sintaxis que mejor se adapte a tu preferencia o escenario específico.

La referencia de propiedades en DataWeave 2.0 te proporciona varias opciones para incorporar dinámicamente valores de configuración en tus transformaciones, dándote flexibilidad y control sobre el comportamiento de tus flujos de datos.

## Ventajas de Configuración de Propiedades:

- **Adaptabilidad:** Puedes ajustar dinámicamente la configuración de DataWeave según los requisitos de cada transformación.
- **Reusabilidad:** Al definir propiedades comunes, puedes reutilizarlas en varias transformaciones, manteniendo consistencia en tu aplicación.

## Propiedades Comunes:

Algunas propiedades comunes incluyen:

- **default-input-encoding:** Establece la codificación de caracteres predeterminada para las entradas.
- **default-output-encoding:** Establece la codificación de caracteres predeterminada para las salidas.
- **default-exports:** Controla qué elementos se exportan automáticamente sin necesidad de especificarlos en cada transformación.

## Consideraciones Importantes:

- **Prioridad de Propiedades:** Las propiedades definidas más cerca de la transformación tienen prioridad sobre las definidas en niveles superiores.
- **Validación de Propiedades:** DataWeave valida las propiedades durante la compilación, ayudando a identificar posibles errores antes de la ejecución.

La configuración de propiedades en DataWeave 2.0 proporciona un nivel adicional de control sobre tus transformaciones, permitiéndote adaptar el comportamiento de DataWeave a las necesidades específicas de cada escenario.

> [!IMPORTANT]
> Recuerde, en cualquiera de los casos, debemos reiniciar la aplicación si hay cualquier cambio realizado en el archivo de propiedades para que se reflejen los nuevos valores

# Actividad, Parte 1

1. Vamos a seleccionar nuestro `Listener` y editar la `Connector Configuration` para que en `Port` tenga lo siguiente:
    ```dw
    ${http.port}
    ```

    guardamos y seguimos

2. Si intentaramos correr el programa fallara, así que ahora en nuestro explorador de archivos seleccionamos el paquete de `src/main/resources` y creamos un nuevo archivo `file` al cual le daremos el nombre de `test.properties`

3. En dicho archivo agregaremos lo siguiente:
    ```yaml
    http.port : 8083
    ```

4. Si intentamos correr ahora el programa, volverá a fallar, eso es por que no le hemos dicho que debe hacer uso de dicho archivo, pero vamos a crear uno nuevo, igual en el mismo lugar pero con el nombre de `dev.properties` en el cuál agregaremos lo siguiente:
    ```yaml
    http.port : 8082
    ```

5. Ahora para que sepa que debe hacer uso de nuestros `properties` iremos a la sección de `Global Elements` y daremos click en `Create`

6. Buscamos `Configuration properties` y lo creamos

7. Luego de eso aparecera en nuestro listado de elementos globales, le daremos doble click o en `Edit` mientras lo tenemos seleccionado

8. Tendremos que seleccionar alguno de los archivos que previamente creamos, de momento seleccionamos `dev.properties` y guardamos

9. Si corremos el programa ahora si debería funcionar

10. Ahora pasaremos al `Transform Message`, en el cuál cambiaremos el código que tiene por el siguiente:
    ```properties
    %dw 2.0
    output application/json
    ---
    {
        "Value using dollar" : '${message}',
        "Value using p" : p('message')
    }
    ```

> [!NOTE]
> Colocamos literalmente "dollar" en el mensaje ya que si usabamos el "$" nos iba a dar error ya que el dw lo iba a considerar como parte del código y no hacer caso al error que nos marca el código, el programa igualmente debería funcionar como se espera

11. En cuanto al message, este lo definiremos en nuestro `test.properties` de la siguiente manera:
    ```yaml
    http.port : 8083
    message : Message from properties
    ```

    Como estamos usando el otro archivo asegurade de cambiarlo en los `Global Elements`

12. Corremos el programa y lo probamos en `RapidAPI` recordando en modificar el puerto de ser necesario y deberíamos ver lo siguiente:
    ```json
    {
        "Value using dollar": "Message from properties",
        "Value using p": "Message from properties"
    }
    ```

### Nota de la actividad

Si eliminamos el `message` de nuestro `test.properties` podremos notar una diferencia importante con el uso de `${}` y el `p('')`. Si comentamos o borramos la parte del `dw` que tiene **"Value using p" : p('message')** y corremos el programa este fallará ya que no encuentra el `message` que previamente eliminamos.

Pero si ahora eliminamos (ya que si comentamos igual fallará) el **"Value using dollar" : '${message}',** y corremos el programa este no fallará y cuando hagamos la petición el JSON que nos aparece sera el siguiente
```json
{
    "Value using p": null
}
```

# El Poder de las Características de Vista Previa (Preview Feature)

Las características de vista previa en DataWeave 2.0 representan una potente herramienta que permite a los desarrolladores interactuar con sus datos de manera dinámica y comprender cómo se transforman antes de aplicar las transformaciones en tiempo de ejecución. Aquí desglosamos por qué estas características son fundamentales y cómo aprovecharlas eficazmente:

## Transformación de Datos:

La transformación de datos es la piedra angular de DataWeave 2.0, y las características de vista previa potencian este proceso. Al previsualizar tus transformaciones, puedes observar cómo se alteran los datos antes de implementar los cambios en tus flujos de integración. Esto simplifica la depuración y ajuste fino de tus transformaciones.

## Extracción de Datos:

Con las características de vista previa, puedes extraer porciones específicas de tus datos y ver cómo se presentan antes de decidir cómo manipularlas. Esto facilita la identificación de patrones, la validación de datos y la toma de decisiones informadas sobre cómo estructurar tus transformaciones.

## Conceptos de Objeto y Matriz:

Entender los conceptos de objeto y matriz es crucial al trabajar con DataWeave. Las características de vista previa te permiten explorar cómo se representan estos conceptos en tus datos. Puedes observar la estructura jerárquica de los objetos y la disposición ordenada de los elementos en las matrices, lo que es esencial para navegar eficazmente a través de tus datos.

## Visualización de Resultados Interactiva:

La característica de vista previa ofrece una visualización interactiva de los resultados de tus transformaciones. Puedes experimentar con diferentes expresiones y funciones directamente en la interfaz de usuario para ver instantáneamente cómo afectan tus datos. Esto agiliza el proceso de desarrollo y reduce el tiempo necesario para obtener los resultados deseados.

## Mejor Comprensión del Lenguaje DataWeave:

Al interactuar con las características de vista previa, puedes desarrollar una comprensión más profunda del lenguaje DataWeave 2.0. Puedes experimentar con diversas funciones y expresiones sin la necesidad de ejecutar el código completo, lo que acelera el aprendizaje y la adopción efectiva del lenguaje.

## Facilita la Depuración:

La capacidad de previsualizar transformaciones facilita la depuración al permitirte identificar rápidamente posibles problemas antes de la ejecución completa. Puedes detectar errores, realizar ajustes y perfeccionar tus transformaciones de manera más eficiente.

En resumen, las características de vista previa en DataWeave 2.0 ofrecen una valiosa capacidad de exploración y experimentación antes de la implementación, permitiéndote entender, depurar y optimizar tus transformaciones de datos de manera efectiva.