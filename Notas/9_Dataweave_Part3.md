# DataWeave 2.0: Explorando Operadores Clave

En DataWeave 2.0, los operadores juegan un papel crucial en la manipulación, transformación y filtrado de datos. Estos operadores proporcionan herramientas poderosas para abordar una variedad de escenarios de integración. A continuación, se presentan algunos de los operadores más importantes y ampliamente utilizados, incluyendo `map`, `mapObject`, `reduce`, y `pluck`.

## Operador `map`:

- **Tipo de Dato de Entrada:** Colección (Lista, Arreglo, etc.).
- **Tipo de Dato de Salida:** Colección con la misma longitud que la entrada.
- **Descripción:** Itera sobre cada elemento de la colección y aplica una transformación a cada uno.
- **Ejemplo:**
    ```properties
    %dw 2.0
    output application/json
    ---
    [1, 2, 3] map ((item) -> item * 2)
    ```
    En este ejemplo, cada elemento de la lista se multiplica por 2.

## Operador `mapObject`:

- **Tipo de Dato de Entrada:** Objeto.
- **Tipo de Dato de Salida:** Objeto.
- **Descripción:** Itera sobre cada par clave-valor del objeto y aplica una transformación a cada valor.
- **Ejemplo:**
  ```properties
  %dw 2.0
  output application/json
  ---
  {
    name: "John",
    age: 30
  } mapObject ((value, key) -> key ++ "_transformed" : value)
  ```
  En este ejemplo, cada clave se transforma añadiendo "_transformed" al final.

## Operador `reduce`:

- **Tipo de Dato de Entrada:** Colección y función de reducción.
- **Tipo de Dato de Salida:** Valor único.
- **Descripción:** Combina los elementos de la colección utilizando una función de reducción para obtener un valor único.
- **Ejemplo:**
  ```properties
  %dw 2.0
  output application/json
  ---
  [1, 2, 3] reduce ((item, acc) -> acc + item)
  ```
  En este ejemplo, se suman todos los elementos de la lista.

## Operador `pluck`:

- **Tipo de Dato de Entrada:** Objeto y lista de claves.
- **Tipo de Dato de Salida:** Objeto con las claves especificadas.
- **Descripción:** Filtra un objeto manteniendo solo las claves especificadas en la lista.
- **Ejemplo:**
  ```properties
  %dw 2.0
  output application/json
  ---
  {
    name: "John",
    age: 30,
    country: "USA"
  } pluck ["name", "country"]
  ```
  En este ejemplo, se extraen las claves "name" y "country" del objeto original.

## Operador `flatten`:

- **Tipo de Dato de Entrada:** Colección anidada.
- **Tipo de Dato de Salida:** Colección plana.
- **Descripción:** Convierte una colección anidada en una colección plana, eliminando la estructura de anidación, es útil cuando necesitas simplificar una estructura de datos anidada, convirtiéndola en una forma más plana y fácil de manejar.
- **Ejemplo:**
    ```dw
    %dw 2.0
    output application/json
    ---
    [[1, 2], [3, 4], [5, 6]] flatten
    ```
    En este ejemplo, la matriz anidada se convierte en una lista plana.

## Operador `filter`:

- **Tipo de Dato de Entrada:** Colección y expresión booleana.
- **Tipo de Dato de Salida:** Colección filtrada.
- **Descripción:** Filtra los elementos de la colección según la condición booleana especificada.
- **Ejemplo:**
    ```properties
    %dw 2.0
    output application/json
    ---
    [1, 2, 3, 4, 5] filter ((item) -> item > 2)
    ```
    En este ejemplo, se filtran los elementos mayores que 2.

## Operador `distinctBy`:

- **Tipo de Dato de Entrada:** Colección y expresión de extracción de clave.
- **Tipo de Dato de Salida:** Colección con elementos únicos según la expresión de extracción.
- **Descripción:** Elimina elementos duplicados de la colección basándose en la expresión de extracción de clave.
- **Ejemplo:**
    ```properties
    %dw 2.0
    output application/json
    ---
    [{"id": 1, "name": "John"}, {"id": 2, "name": "Jane"}, {"id": 1, "name": "Jim"}] distinctBy ((item) -> item.id)
    ```
    En este ejemplo, se eliminan los elementos duplicados basándose en la clave "id".

## Operador `orderBy`:

- **Tipo de Dato de Entrada:** Colección y expresión de ordenación.
- **Tipo de Dato de Salida:** Colección ordenada.
- **Descripción:** Ordena los elementos de la colección según la expresión de ordenación especificada.
- **Ejemplo:**
    ```properties
    %dw 2.0
    output application/json
    ---
    [{"id": 3, "name": "Alice"}, {"id": 1, "name": "Bob"}, {"id": 2, "name": "Charlie"}] orderBy ((item) -> item.id)
    ```
    En este ejemplo, se ordenan los elementos por la clave "id".

## Operador `merge`:

- **Tipo de Dato de Entrada:** Dos objetos.
- **Tipo de Dato de Salida:** Objeto combinado.
- **Descripción:** Combina dos objetos, agregando las propiedades del segundo al primero.
- **Ejemplo:**
    ```properties
    %dw 2.0
    output application/json
    ---
    {
        "name": "John",
        "age": 30
    } merge {
        "country": "USA"
    }
    ```
    En este ejemplo, se combinan dos objetos, agregando la propiedad "country" al primero.

# Actividad, Parte 1

1. Abre el [Dataweave PlayGround](https://dataweave.mulesoft.com/learn/dataweave) para poder comenzar a trabajar

2. Agregamos en el _payload_ un `JSON` como este:
    ```json
    [
        {
        "name": "Jhon",
        "id": 1 
        }
    ]
    ```

3. Segido de eso en el _script_ definimos un array como variable global, igual con forma de `JSON` de la siguiente forma:
    ```properties
    %dw 2.0
    output application/json

    var a = [{
        "city": "Toluca",
        "country": "Mexico"
    }]
    ---
    payload
    ```

## Flatten

4. Ahora podremos comencar a hacer pruebas, por ejemplo prueba de uno en uno los siguientes códigos:
    - `payload + a`
    - `payload ++ a`
    - `flatten(payload + a)`

    y observa como se comporta el _output_

5. Ahora modifiquemos el _payload_ quedando de la siguiente manera:
    ```json
    [
        {
        "name": "Jhon",
        "id": 1 
        },
        {
        "name": "Vanesa",
        "id": 2
        }
    ]
    ```

## Reduce

6. Borraremos la variable global que antes definimos y de código agrega lo siguiente y observa el _output_
    - `payload reduce($)`
    - `payload reduce($$)`

  Agrega una tercera persona en el _payload_ y prueba de nuevo los códigos anteriores

7. Ahora prueba lo siguiente:
    - `payload reduce($ + $$)`

8. Podemos hacer uso de los terminos **item** y **accumulator** de la siguiente manera:
    ```properties
    %dw 2.0
    output application/json
    ---
    payload reduce ((item, accumulator) -> (item ++ accumulator))
    ```

    observa lo que arroja el _output_

9. Ahora volvamos a definir una variable global, como esta:
    - `var a = [1,2,3,4,5]`

10. En el _script_ prueba los siguientes códigos y observa que lanza el _output_ con cada uno de estos:
    - `a reduce ($ + $$)`
    - `a reduce ($$ ++ $)`
    - `a reduce ($ ++ $$)`

11. Podemos hace uso igual de la función flecha con _item_ y _accumulator_ para obtener los mismos resultados, has la prueba
    - `a reduce ((item, accumulator) -> (item + accumulator))`
    - `a reduce ((item, accumulator) -> (item ++ accumulator))`

## Pluck

12. Ahora el _payload_ lo modificamos de la siguiente manera:
    ```json
    {
        "id": 1,
        "name": "Maria",
        "country": "Mexico",
        "state": "Guerrero"
    }
    ```

13. Seguido de eso probamos ahora `Pluck` de las siguientes formas:
    - `payload pluck ($)`
    - `payload pluck ($$)`
    - `payload pluck ($$$)`

    observa lo que resulta de cada uno en el _output_

14. Con la función flecha podemos obtener los mismos resultados, prueba:
    - `payload pluck ((value, key, index) -> value)`
    - `payload pluck ((value, key, index) -> key)`
    - `payload pluck ((value, key, index) -> index)`

# Importantes Sintaxis Comunes Utilizadas en DataWeave 2.0

Exploraremos diversas sintaxis y constructos esenciales en DataWeave 2.0, proporcionando explicaciones, ejemplos y el propósito de cada uno.

## Reduce:

- **Sintaxis:**
    ```properties
    [1, 2, 3] reduce ((item, acc) -> acc + item)
    ```
- **Descripción:** Combina los elementos de una colección utilizando una función de reducción, produciendo un valor único.

## Pluck:

- **Sintaxis:**
    ```properties
    {
        "name": "John",
        "age": 30,
        "country": "USA"
    } pluck ["name", "country"]
    ```
- **Descripción:** Filtra un objeto manteniendo solo las claves especificadas en la lista.

## Flatten:

- **Sintaxis:**
    ```properties
    [[1, 2], [3, 4], [5, 6]] flatten
    ```
- **Descripción:** Convierte una colección anidada en una colección plana, eliminando la estructura de anidación.

## If-Else:

- **Sintaxis:**
    ```properties
    if (condition) "true branch" else "false branch"
    ```
- **Descripción:** Realiza una evaluación condicional y devuelve uno de los dos valores según la condición.

## GroupBy:

- **Sintaxis:**
    ```properties
    [1, 2, 3, 4] groupBy ((item) -> item mod 2)
    ```
- **Descripción:** Agrupa los elementos de una colección según el resultado de una función de agrupación.

## DistinctBy:

- **Sintaxis:**
    ```properties
    [{"id": 1, "name": "John"}, {"id": 2, "name": "Jane"}, {"id": 1, "name": "Jim"}] distinctBy ((item) -> item.id)
    ```
- **Descripción:** Elimina elementos duplicados de una colección basándose en la expresión de extracción de clave.

## Custom Functions:

- **Sintaxis:**
    ```properties
    %function customFunction(param) param * 2
    ```
- **Descripción:** Define funciones personalizadas para su reutilización en transformaciones.

## Define Local Variables:

- **Sintaxis:**
    ```properties
    %var localVar = "Local Variable"
    ---
    localVar
    ```
- **Descripción:** Define variables locales para almacenar valores temporales dentro de una transformación.

Para cada tema, proporcionaremos ejemplos específicos, explicaciones detalladas de la sintaxis y discusiones sobre por qué y cuándo utilizar cada construcción. Esta guía ayudará a comprender y aplicar estas sintaxis comunes de manera efectiva en el contexto de transformaciones de datos en DataWeave.

# Actividad, Parte 2

1. Abrimos nuestro [Dataweave PlayGround](https://dataweave.mulesoft.com/learn/dataweave) para comenzar

## If / Else

2. En el _payload_ agregamos lo siguiente:
    ```json
    [
        {
            "name": "Maria",
            "subject": "Maths",
            "marks": 30
        },
        {
            "name": "Veronica",
            "subject": "Social",
            "marks": 70
        }
    ]
    ```

3. Ahora en el _script_ haremos uso del `map` y el `if/else` ya que haremos que en el _output_ nos marque `PASS` si en _marks_ tiene más de 60 y `FAIL` si tiene menos de 60:
    ```properties
    %dw 2.0
    output application/json
    ---
    payload map {
        "name": $.name,
        "marks": if($.marks > 60) "PASS" else "FAIL"
    }
    ```

4. Imaginemos que en el _payload_ tenemos la situación de que algún elemento no contenga `marks`, eso hará que falle el programa por que seria `null`, en sesiones pasadas vimos que podemos hacer uso del `default` para evitar este tipo de problemas así que podemos hacer lo mismo aqui:
    ```properties
    %dw 2.0
    output application/json
    ---
    payload map {
        "name": $.name,
        "marks": if(($.marks default 0) > 60) "PASS" else "FAIL"
    }
    ```

    haz la prueba eliminando unod e los `marks` del _payload_

5. 