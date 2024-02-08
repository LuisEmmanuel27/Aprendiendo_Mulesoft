# ¿Qué es una especificación de API?

Una especificación de API es un conjunto formal de reglas, directrices y descripciones que define la forma en que los desarrolladores deben interactuar con una interfaz de programación de aplicaciones (API). Esta especificación sirve como un contrato digital que establece las reglas para el intercambio de datos y la comunicación entre sistemas.

Las especificaciones de API son esenciales para la correcta implementación y utilización de una API, y generalmente incluyen los siguientes elementos:

## 1. Definición de Endpoints:

Especifica los puntos de conexión (endpoints) de la API, indicando las URL o URI a las cuales los desarrolladores pueden enviar solicitudes y recibir respuestas.

## 2. Métodos HTTP Permitidos:

Describe los métodos HTTP que se pueden utilizar en cada endpoint (por ejemplo, GET, POST, PUT, DELETE) y cómo se deben utilizar para realizar operaciones específicas.

## 3. Parámetros y Formatos de Datos:

Detalla los tipos de parámetros que se deben enviar en las solicitudes (query parameters, path parameters, request body) y cómo deben estructurarse. También especifica los formatos de datos aceptados y devueltos (por ejemplo, JSON, XML).

## 4. Autenticación y Autorización:

Indica los mecanismos de autenticación necesarios para acceder a la API, así como los niveles de autorización requeridos para realizar ciertas acciones.

## 5. Respuestas y Códigos de Estado HTTP:

Especifica los posibles códigos de estado HTTP que la API puede devolver en respuesta a una solicitud, junto con la estructura y el significado de las respuestas.

## 6. Ejemplos de Uso:

Proporciona ejemplos concretos de cómo realizar solicitudes a la API y cómo interpretar las respuestas, lo que facilita a los desarrolladores comprender y utilizar la interfaz de manera efectiva.

## 7. Documentación Adicional:

Puede incluir información adicional sobre las limitaciones, las mejores prácticas, y cualquier consideración especial que los desarrolladores deban tener en cuenta al interactuar con la API.

## 8. Formatos de Especificación:

Puede estar escrita en diferentes formatos, siendo Swagger/OpenAPI, RAML, y GraphQL algunos de los estándares comunes utilizados para documentar y especificar APIs.

Una especificación de API es esencial para promover la interoperabilidad entre sistemas y facilitar la colaboración entre desarrolladores y consumidores de la API. Además, sirve como una herramienta valiosa para la documentación, la prueba automatizada y el mantenimiento a lo largo del ciclo de vida de la API.

# Actividad, Parte 1

1. Creamos en el `Design Center` una nueva `API Specification`

2. Le damos el nombre de `bank-sys-api-ex`

3. Agregamos lo siguiente:
    ```yaml
    #%RAML 1.0
    title: bank-sys-api-ex

    /getAccountSummary:
        description: "To get bank account details of a customer"
        get:
            queryParameters:
                accountNumber:
                    type: number
            responses:
                200:
                    body:
                        application/json:
                            example:
                                {
                                    "BankAccountNumber": 123456,
                                    "Name": "Jhon Dee",
                                    "Country": "México"
                                }
    ```

### Explicación del código

Este código `RAML` (RESTful API Modeling Language) describe la especificación de una API con el título "bank-sys-api-ex".

- Encabezado:
    - `#%RAML 1.0`: Indica la versión de RAML que se está utilizando (en este caso, la versión 1.0).

- Título de la API:
    - `title: bank-sys-api-ex`: Proporciona un título descriptivo para la API, en este caso, "bank-sys-api-ex".

- Recurso /getAccountSummary:
    - Define un recurso llamado `/getAccountSummary` que representa una operación para obtener el resumen de la cuenta bancaria de un cliente.

- Descripción del Recurso:
    - `description: "To get bank account details of a customer"`: Proporciona una breve descripción del propósito del recurso.

- Operación GET:
    - `get:`: Indica que este recurso es accesible mediante el método HTTP GET.

- Parámetros de Consulta (queryParameters):
    - `queryParameters:`: Define los parámetros de consulta que se pueden incluir en la solicitud GET.
    - `accountNumber:`: Especifica un parámetro de consulta llamado `accountNumber`.
    - `type`: number: Indica que el parámetro accountNumber debe ser de tipo numérico.

- Respuestas (responses):
    - `responses:`: Define las respuestas posibles que la API puede devolver.
    - `200:`: Representa una respuesta exitosa (código de estado HTTP 200).
    - `body:`: Define el cuerpo de la respuesta.
    - `application/json:`: Indica que el cuerpo de la respuesta estará en formato JSON.
    - `example:`: Proporciona un ejemplo del cuerpo de la respuesta en formato JSON.

    ```yaml
    {
        "BankAccountNumber": 123456,
        "Name": "Jhon Dee",
        "Country": "México"
    }
    ```

    Este ejemplo muestra la estructura de datos que se espera en una respuesta exitosa, incluyendo el número de cuenta bancaria, el nombre del titular y el país asociado.

4. Seguido del código anterior agregamos el siguiente:

    ```yaml
    /createAccount:
        description: "To create a new account"
        post:
            queryParameters:
                branch: string
            body:
                application/json:
                    example:
                    {
                        "name": "Emily Dee",
                        "DOB": "27-dec-1997"
                    }
            responses:
                201:
                    body:
                        application/json:
                            example:
                            {
                                "message": "Account created!"
                            }
    ```

5. Finalmente podremos ver el resultado en el botón de `Documentation`

6. Publicamos la API

> [!NOTE]
> De momento decidí omitir el paso 6