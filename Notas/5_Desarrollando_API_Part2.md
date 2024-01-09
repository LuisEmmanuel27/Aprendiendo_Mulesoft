# Developing APIs PART 2

Continuando con lo realizado en la parte 1...

## Flow Reference Connector

El conector "Flow Reference" en Mule 4 es como el mensajero interno de tu aplicación. Su función es dirigir el flujo del mensaje hacia otro lugar específico en tu aplicación. Puedes pensar en él como el encargado de "llamar a otro equipo" dentro de tu aplicación para que realice una tarea específica.

**Ejemplo Simplificado:**
Imagina que tu aplicación es como un gran edificio con diferentes equipos en cada piso. Cuando el "Flow Reference" entra en acción, es como si alguien tomara el mensaje y dijera: "¡Oye, ve al piso 5 y dile al equipo de ventas que necesitamos ayuda!". Así, el mensaje se dirige directamente al lugar adecuado en tu aplicación.

**Configuración Básica:**
```xml
<flow name="flujoPrincipal">
    <!-- ... otros componentes del flujo ... -->
    <flow-ref name="equipoVentas" doc:name="Flow Reference"/>
</flow>

<flow name="equipoVentas">
    <!-- Este es el equipo de ventas que atenderá el mensaje -->
    <!-- ... otros componentes del flujo de ventas ... -->
</flow>
```

En este ejemplo, cuando el mensaje llega al "Flow Reference" llamado "equipoVentas," se dirige directamente al equipo de ventas para que realice su trabajo. Es como si alguien en tu aplicación estuviera haciendo una llamada interna para obtener ayuda específica. ¡Y así es como el "Flow Reference" guía el flujo del mensaje en Mule 4!

# Actividad, Parte 1

1. Haremos un pequeño cambio, quitaremos el `Set Payload` de nuestro `Flow` y en su lugar colocaremos un `Transform Message`, siendo que este ahora nos aparece una sección de código en la cual colocaremos lo siguiente

    ```json
    %dw 2.0
    output application/json
    ---
    "Hello World! Luis E. esta aquí"
    ```

2. Guardamos, corremos el programa, vamos a la url que ya conocemos y en vez de descargar un archivo aparecera un `JSON` en el navegador con el mensaje

3. Ahora haremos uso de un `Private Flow`, para ello arrastramos al canvas un nuevo `Flow` y lo nombraremos como `private-flow`

4. Arrastramos de nuestro `FLow` principal el conector de `Transform Message` y lo colocamos dentro del `Process` de nuestro `private-flow`

5. Seleccionamos ahora desde `Mule Palette` el conectorde `Flow Reference` y lo colocamos dentro del `Process` de nuestro `Flow` principal

6. Nos dara un error y es que necesitamos decirle a que le debe hacer referencia asi que seleccionando nuestro `Flow Reference` en la parte de inferior nos aparecera la opción de `Flow name`, simplemente damos click al toggle y seleccionamos nuestro `Private Flow` y todo deberia estar en orden

<div align="center">
    <img src="./img/practica2-ejemplo1.png" alt="ejemplo" width="700">
</div>

