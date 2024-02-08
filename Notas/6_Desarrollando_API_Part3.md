# Developing APIs PART 3

Para esta parte necesitamos recordar o saber algo importante hacerca de los `Mule Events`, los `atributos` son algo que son _inmutables_ y que _cambiarán_ cuando se conecten a _sistemas externos._

Pero en lo realizado en las partes anteriores no hemos visto en acción esa propiedad, asi que de eso se tratara esta actividad que se va a realizar

# Actividad, Parte 1

1. Ver el siguiente [video](https://www.youtube.com/watch?v=sCcZTPmdV1U&list=PL61bQcdxsK6_f5GDV3f2STtMTS3nYIlYO&index=11), del minuto "0:00" al "8:13"

# Set Payload vs Transform Message

Ambos "Set Payload" y "Transform Message" son componentes en Mule 4 que se utilizan para manipular el contenido de un mensaje.

## Set Payload:

- **Función Principal:** Cambia el contenido completo del mensaje por un nuevo valor específico.
- **Uso Común:** Útil cuando deseas establecer un valor específico o constante como la carga útil del mensaje.
- **Ejemplo:**
  ```xml
  <set-payload value="¡Hola, Mundo!" doc:name="Set Payload"/>
  ```
  
## Transform Message:

- **Función Principal:** Transforma la carga útil del mensaje mediante expresiones de DataWeave o lenguajes de transformación compatibles.
- **Uso Común:** Ideal para realizar transformaciones complejas en el contenido del mensaje, como cambiar el formato, filtrar datos, etc.
- **Ejemplo:**
  ```xml
  <transform-message doc:name="Transform Message">
      <dw:input-payload mimeType="application/json"/>
      <dw:set-payload><![CDATA[%dw 2.0
        output application/json
        ---
        {
          greeting: "¡Hola, Mundo!"
        }]]></dw:set-payload>
  </transform-message>
  ```

## Ventajas de Set Payload:

- **Simplicidad:** Es más sencillo y directo para establecer un valor fijo en la carga útil del mensaje.
- **Rendimiento:** Puede ser más eficiente para casos simples donde solo se necesita cambiar el valor sin procesamiento adicional.

## Desventajas de Set Payload:

- **Limitaciones en Transformación:** No proporciona capacidades avanzadas de transformación como lo hace "Transform Message."

## Ventajas de Transform Message:

- **Flexibilidad:** Permite transformaciones más complejas utilizando expresiones de DataWeave u otros lenguajes de transformación.
- **Manejo de Variables:** Puede trabajar fácilmente con variables y acceder a ellas durante la transformación.

## Desventajas de Transform Message:

- **Mayor Complejidad:** Puede resultar más complejo para casos simples donde solo se necesita cambiar el valor sin transformaciones significativas.

## Manejo de Variables con Transform Message:

En "Transform Message," puedes acceder y manipular variables de manera eficiente. Por ejemplo, puedes utilizar variables en tus expresiones de transformación de DataWeave para crear lógica dinámica basada en el contenido de las variables.

**Ejemplo:**
```xml
<set-variable variableName="nombre" value="Mundo" doc:name="Set Variable"/>
<transform-message doc:name="Transform Message">
    <dw:input-payload mimeType="application/json"/>
    <dw:set-payload><![CDATA[%dw 2.0
        output application/json
        ---
        {
          greeting: "¡Hola, " ++ vars.nombre ++ "!"
        }]]></dw:set-payload>
</transform-message>
```

En este ejemplo, la variable "nombre" se utiliza dinámicamente en la expresión de transformación para personalizar el saludo. Esta capacidad de manejar variables hace que "Transform Message" sea poderoso y adaptable a escenarios más complejos.

# Actividad, Parte 2

1. Ver el [video](https://www.youtube.com/watch?v=sCcZTPmdV1U&list=PL61bQcdxsK6_f5GDV3f2STtMTS3nYIlYO&index=11) a partir del minuto "8:13"