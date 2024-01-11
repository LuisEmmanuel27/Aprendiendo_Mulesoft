# DataWeave 2.0: Operadores `map` y `mapObject`

Los operadores `map` y `mapObject` son herramientas poderosas en DataWeave 2.0 que facilitan la transformación y manipulación de datos. Aquí exploramos sus características, aplicaciones y cuándo utilizarlos:

## Operador `map`:

### ¿Qué es el Operador `map`?

- **Definición:** El operador `map` se utiliza para iterar sobre una colección (como una lista o un arreglo) y aplicar una transformación a cada elemento de la colección.

### ¿Por qué Usar `map`?

- **Iteración Simple:** Cuando necesitas realizar la misma operación en cada elemento de una colección, el operador `map` simplifica el proceso.

**Ejemplo:**
```dw
%dw 2.0
output application/json
---
[1, 2, 3] map ((item) -> item * 2)
```
En este ejemplo, cada elemento de la lista se multiplica por 2.

### ¿Cuándo No Usar `map`?

- **Estructuras Anidadas:** Si estás trabajando con datos anidados y necesitas acceder a diferentes niveles de la estructura, puede ser más adecuado utilizar `mapObject` para un control más granular.

## Operador `mapObject`:

### ¿Qué es el Operador `mapObject`?

- **Definición:** El operador `mapObject` se utiliza para iterar sobre un objeto y aplicar una transformación a cada par clave-valor.

### ¿Por qué Usar `mapObject`?

- **Manipulación de Objetos:** Cuando necesitas realizar transformaciones específicas en pares clave-valor dentro de un objeto, `mapObject` brinda mayor flexibilidad.

**Ejemplo:**
```dw
%dw 2.0
output application/json
---
{
  name: "John",
  age: 30
} mapObject ((value, key) -> key ++ "_transformed" : value)
```
En este ejemplo, cada clave se transforma añadiendo "_transformed" al final.

### ¿Cuándo No Usar `mapObject`?

- **Colecciones Simples:** Si estás trabajando con listas o arreglos, y no necesitas acceder a las claves de un objeto, el operador `map` puede ser más apropiado.

## Consideraciones Importantes:

- **Iteración vs. Transformación Detallada:** `map` es ideal para iterar sobre elementos de una colección, mientras que `mapObject` es más adecuado cuando necesitas una manipulación más específica a nivel de clave-valor.

- **Combinación de Operadores:** A menudo, proyectos complejos pueden requerir la combinación de `map` y `mapObject` para abordar diferentes niveles de estructuras de datos.

Ambos operadores son fundamentales en el arsenal de un desarrollador de DataWeave, proporcionando herramientas flexibles para manipular datos en diversos escenarios y estructuras. La elección entre `map` y `mapObject` dependerá de la naturaleza de los datos y las transformaciones requeridas.

# Actividad, Parte 1

1. Ya sea en el [Dataweave PlayGround](https://dataweave.mulesoft.com/learn/dataweave) o desde el `Anypoint Studio` podremos realizar la actividad, usare el Dataweave PlayGround por cuestiones de facilidad

## map

2. En el _payload_ vamos a colocar un `JSON` como este:
  ```json
  [
    {
      "id": 1,
      "firstname": "Jazmin",
      "lastname": "dee",
      "age": "25",
      "country": "USA",
      "branch": "computing"
    },
    {
      "id": 2,
      "firstname": "Maria",
      "lastname": "Concepcion",
      "age": "27",
      "country": "Mexico",
      "branch": "chemistry"
    },
    {
      "id": 3,
      "firstname": "Hector",
      "lastname": "Duran",
      "age": "23",
      "country": "Salvador",
      "branch": "electronics"
    }
  ]
  ```

3. De momento nos interesa que en el _script_ solo tengamos esto:
  ```properties
  %dw 2.0
  output application/json
  ---
  payload
  ```

  veremos que en el _output_ aparece nuestro `JSON` con normalidad y estaremos listos para probar unas cosas

4. Haremos uso del operador `map` que es utilizado en `Arrays` como es en este caso, iremos poco a poco, primero agrega el siguiente código:
  ```properties
  %dw 2.0
  output application/json
  ---
  payload map {

  }
  ```

  y observa que aparece en el _output_

5. Ahora agrega lo siguiente:
  ```properties
  %dw 2.0
  output application/json
  ---
  payload map {
    "name": $
    "test": $$
  }
  ```

  y observa que aparece en el _output_

> [!NOTE]
> El "$" nos da el valor de un valor en particular de campo/clave, en el caso del paso 5. como no estamos específicando nos retorna todo el payload completo. Mientras que el "$$" nos da el índice del campo

6. Ahora modificamos el codigo quedando de la siguiente forma:
  ```properties
    %dw 2.0
  output application/json
  ---
  payload map {
    "name": $.firstname ++ " " ++ $.lastname
  }
  ```

  y observa que aparece en el _output_

> [!NOTE]
> Puedes probar esto en `Anypoit Studio` haciendo lo mismo que hicimos en [La parte 1 de DataWeave, en la Actividad, Parte 3](./7_Dataweave_Part1.md/#actividad-parte-3)

7. Ahora agreguemos lo siguiente:
  ```properties
  %dw 2.0
  output application/json
  ---
  payload map {
      "name" : $.firstname ++ " " ++ $.lastname,
      "branch" : $.branch,
      "rollno" : $.firstname ++ "-" ++ $.lastname ++  "-" ++  $.id
  }
  ```

  y observa que aparece en el _output_

8. Ahora, otra forma de usar el `map` es con función `flecha` similar a otros lenguajes como JS se ve de la siguiente manera:
  ```properties
  payload map ((item, index) -> {
    "item" : item,
    "index" : index
  })
  ```

  pruebala y observa el resultado

> [!TIP]
> La forma de comentar código es con los "//" o con "/* */"

9. Prueba realizando lo del paso _7_ pero esta vez con la función `flecha`:
  ```properties
  payload map ((item, index) -> {
      "name" : item.firstname ++ " " ++ item.lastname,
      "branch" : item.branch,
      "rollno" : item.firstname ++ "-" ++ item.lastname ++  "-" ++  item.id
  })
  ```

10. Ahora intentaremos anidar un `map` dentro de otro, para ello vamos a modificar el _payload_ quedando de esta manera:
  ```json
  [
      {
          "id": 1,
          "firstname": "Jazmin",
          "lastname": "dee",
          "age": "25",
          "country": "USA",
          "branch": ["A","B","C"]
      },
      {
          "id": 2,
          "firstname": "Maria",
          "lastname": "Concepcion",
          "age": "27",
          "country": "Mexico",
          "branch": ["D","E"]
      },
      {
          "id": 3,
          "firstname": "Hector",
          "lastname": "Duran",
          "age": "23",
          "country": "Salvador",
          "branch": ["F"]
      }
  ]
  ```

11. Modificaremos el código de manera que ahora también recorreremos los arrays de `branch` y colocaremos una descripción de manera dinámica:
  ```properties
  payload map ((item, index) -> { 
      "name" : item.firstname ++ " " ++ item.lastname,
      "branch" : item.branch map {
          "name" : $,
          "description" : item.firstname ++ " " ++ item.lastname ++ " is studying in " ++ $,
      },
      "rollno" : item.firstname ++ "-" ++ item.lastname ++  "-" ++  item.id
  })
  ```

  observa el resultado

> [!IMPORTANT]
> Utilizamos un `map` con función flecha para estructurar un objeto que contiene otro `map` sin hacer uso directo de la función flecha. Esto se hace para evitar conflictos con el uso de "$" en el segundo `map`, ya que el programa podría tener dificultades para determinar a qué dato nos referimos.

12. Si queremos que el segundo map sea también con función flecha solo basta con mantener las buenas practicas y nombrar de una manera entendible el item y el index de este:
  ```properties
  payload map ((item, index) -> { 
      "name" : item.firstname ++ " " ++ item.lastname,
      "branch" : item.branch map ((item2, index2) -> {
        "name" : item2,
        "description" : item.firstname ++ " " ++ item.lastname ++ " is studying in " ++ item2,
      }),
      "rollno" : item.firstname ++ "-" ++ item.lastname ++  "-" ++  item.id
  })
  ```

> [!NOTE]
> Imaginemos que en nuestro _payload_ hace falta el valor del `lastname` eso nos dara error en el programa ya que es un `null` y al ser null no podemos concatenarlo con el `++`, para evitar un problema así podemos haceralgo similar al operador ternario, el `default`, en este caso seria algo como `(item.lastname default "")` con eso le estamos diciendo que en caso de que no se encuente lo sustitulla por lo que tengamos entre las comillas

## mapObject

13. Para probar el `mapObject` necesitamos modificar el _payload_ ya que no funciona con `arrays` solo con `objetos`, por ello nuestro _payload_ quedara algo así:
  ```json
  {
    "id": 1,
    "firstname": "Jazmin",
    "lastname": "Dee",
    "age": "25",
    "country": "USA",
    "branch": ["A","B","C"]
  }
  ```

14. Borramos todo lo que tengamos en nuestro _script_ y simplemente lo dejamos como en un inicio:
  ```properties
  %dw 2.0
  output application/json
  ---
  payload 
  ```

15. Ahora comenzaremos a usar el `mapObject` de manera muy similar al `map`:
  ```properties
  %dw 2.0
  output application/json
  ---
  payload mapObject {
      "probando dollar" : $
  }
  ```

  prueba primero con un solo "$", luego con "$$" y finalmente con "$$$" para ver lo que sucede en el _output_

16. Ahora que pasa si necesitamos un array pero, en este caso, solo de los `keys`, pues para eso podemos hacer lo siguiente:
  ```properties
  %dw 2.0
  output application/json
  ---
  (payload mapObject {
      "keys" : $$
  }).*keys
  ```

17. Al igual que con `map` podemos hacer uso de la función `flecha`, si la usamos para hacer algo como en el código anterior tendríamos algo como esto:
  ```
  %dw 2.0
  output application/json
  ---
  (payload mapObject ((value, key, index) -> {
      keys : key,
  })).*keys
  ```