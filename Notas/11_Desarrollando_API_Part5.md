# Validación en Mule 4

El componente de validación en Mule 4 desempeña un papel crucial al verificar si el contenido de un mensaje cumple con los criterios especificados. Aquí se detallan aspectos importantes sobre su funcionamiento:

**Verificación de Contenido:**
El componente de validación examina el contenido de un mensaje para asegurarse de que cumpla con ciertos criterios predefinidos. Estos criterios podrían incluir condiciones específicas, como la presencia de ciertos datos o el formato correcto de la carga útil.

**Flujo de Ejecución:**
Si las condiciones de validación se cumplen, el flujo de ejecución continúa hacia los conectores o componentes siguientes. En caso contrario, si no se cumplen las condiciones, el componente de validación generará un error.

**Manejo de Errores:**
La generación de un error por parte del componente de validación indica que el mensaje no cumple con los requisitos esperados. Este error puede ser manejado en el flujo, permitiendo tomar medidas específicas en respuesta a la invalidación del mensaje.

**Ejemplo de Uso:**
En un escenario práctico, el componente de validación podría utilizarse para asegurarse de que ciertos campos obligatorios estén presentes en un mensaje o que los valores numéricos estén dentro de un rango específico.

```xml
<validation:is-true expression="#[payload.amount > 0]" doc:name="Check Amount">
  <validation:on-error-continue />
</validation:is-true>
```

En este ejemplo, el componente de validación verifica si el campo "amount" en la carga útil del mensaje es mayor que cero. Si se cumple la condición, el flujo continúa; de lo contrario, se maneja el error de forma que el proceso no se vea interrumpido.

El componente de validación es esencial para garantizar la integridad de los datos y la consistencia en el flujo de integración, permitiendo definir y hacer cumplir reglas específicas en función de los requisitos del negocio.

# Actividad, Parte 1

1. Abrimos nuestro proyecto de siempre de `Anypoint Studio`

2. Agregamos un nuevo `Listener` a nuestro canvas y en el valor de `Path` colocaremos `/validation`

3. En nuestro `Mule Palette` si buscamos `validation` veremos que nos muestra una serie de conectores distintos para validar distintos tipos de datos, en este caso toma el de `Is number` y colocalo depués del `Listener`

4. En su `Module configuration` da click en el `+` y luego presiona `ok` a la ventana que aparece

5. Entre el `Listener` y el `Is number` coloca un `Transform Message` con el siguiente código:
    ```properties
    %dw 2.0
    output application/json
    ---
    payload.id
    ```

6. Ahora coloca otro `Transform Message` frente al `Is number` y coloca el siguiente código:
    ```properties
    %dw 2.0
    output application/json
    ---
    "Success"
    ```

7. Volvemos al `Is number` para terminar de configurarlo de la siguiente manera:
    - `value:` **#[payload]**
    - `Number type:` **INTEGER**
    - `Message:` **This is NOT a number, Please check

8. Coloca un `Breakpoint` en el primer `Transform Message`

9. Guarda todo y prueba el programa, para probarlo usa un POST (funciona con cualquier metodo pero por buenas practicas sera con POST) y en el body coloca un `JSON` simple como:
    ```json
    {
        "id": 123
    }
    ```

> [!TIP]
> Si intentamos probar con un id tipo `"id": "1234"` igual funcionara y nos dara el mensaje de `success`, eso es por que como reconoce que son solo digitos hace una conversión interna, pero si es algo como `"id": "123a"`, fallará ya que detectara que hay un caracter presente, por ende ya no hara una conversión

# Antes de continuar...

En este punto es necesario instalar o tener instalado `MySQL` o cualquier otra base de datos para continuar, de manera personal usare `MySQL`

Además también debemos crear una cuenta de prueba en `Salesforce`

# Tema