# ¿Cuándo y cómo usar `resourcesTypes`?

Los `resourceTypes` en RAML son una poderosa herramienta para definir tipos de recursos que pueden ser reutilizados en toda la especificación de la API. Su uso se centra en evitar la redundancia de código y simplificar el mantenimiento de la especificación. Aquí proporcionaremos detalles adicionales sobre cuándo y cómo aprovechar al máximo los `resourceTypes`:

## 1. Escenarios de uso:

- `Recursos Comunes`: Cuando varios recursos comparten características similares, como parámetros, respuestas o métodos, los resourceTypes son ideales para evitar la repetición y garantizar la coherencia en toda la API.

- `Patrones de Diseño`: Para aplicar patrones de diseño específicos a la API, como el `HATEOAS` (_Hypermedia as the Engine of Application State_), donde se incluyen enlaces a recursos relacionados en las respuestas. Al definir un resourceType con la lógica del patrón, puedes asegurar una implementación coherente en todos los recursos que lo utilicen.

## 2. Declaración en el Archivo RAML:

Los `resourceTypes` se declaran en la sección `resourceTypes` del archivo RAML. A continuación, se muestra un ejemplo básico de cómo se declararía un `resourceType`:

```yaml
resourceTypes:
    - commonResource:
        get:
            responses:
                200:
                    body:
                        application/json:
                            example: |
                                {
                                    "message": "This is a common response."
                                }
```

## 3. Aplicación en Recursos:

- Después de declarar un resourceType, puedes aplicarlo a un recurso específico utilizando el atributo type. Aquí hay un ejemplo de cómo se aplicaría el resourceType anterior a un recurso:

```yaml
/exampleResource:
  type: commonResource
```

## 4. Personalización de resourceTypes:

Los `resourceTypes` pueden ser configurados con parámetros que permiten personalizar su comportamiento en diferentes contextos. Esto brinda flexibilidad al reutilizar un `resourceType` en distintos recursos. A continuación, se muestra un ejemplo básico de cómo definir y usar parámetros en un `resourceType`:

```yaml
resourceTypes:
  - commonResource:
      get:
        queryParameters:
          customParameter:
            type: string
            required: true
```

Al aplicar este `resourceType`, puedes personalizar el parámetro `customParameter` según las necesidades específicas de cada recurso.

<hr/>

En resumen, `resourceTypes` en RAML son una herramienta fundamental para promover la reutilización y coherencia en la especificación de una API. Al entender cuándo y cómo utilizarlos, los desarrolladores pueden optimizar el diseño y mantenimiento de la API, asegurando consistencia y eficiencia en todo el desarrollo.

# Ejemplo práctico

1. Volvemos a nuestro proyecto de `bank-sys-api-ex`

2. Agregamos el siguiente código justo debajo del `title`:

```yaml
resourceTypes:
  healthCheck:
    description: Get <<resourcePathName | !uppercase>>  
    get:
      responses:
        200:
          body:
            application/json:
              example:
                {
                  time: "10:30 am",
                  health: "ok"
                }

/health:
  type:
    healthCheck

/otherHealth:
  type:
    healthCheck
```

> [!NOTE]
> también podemos usar esto en el description del resourceTypes: `"This is to get <<resourcePathName>>"`

3. Revisamos la `Documentation` y notaremos que el description de `health` y de `otherHealth` cambian de forma automatica por el como definimos el description en el `resourceType`

Otros ejemplos de funciones/parámetros:
- !singularize
- !pluralize
- !uppercase
- !lowercase
- !uppercamelcase
- !lowercamelcase
- !upperunderscorecase
- !lowerunderscorecase
- !upperhyphencase
- !lowerhyphencase

# Traits

Los `traits` son fragmentos de código reutilizables que se pueden aplicar a los métodos `HTTP` de los recursos definidos en una especificación _RAML_. Los `traits` permiten modularizar y estandarizar el comportamiento común de las APIs, como la autenticación, la paginación, la limitación de velocidad o la validación de parámetros. Los `traits` se pueden definir en un archivo _RAML_ separado y referenciarlos desde el archivo principal, o se pueden definir dentro del mismo archivo _RAML_. Para aplicar un trait a un método `HTTP`, se utiliza la clave `is` seguida de una lista de los nombres de los `traits`

## 1. Escenarios de Uso:

- `Autenticación`: Los traits permiten definir y aplicar lógica de autenticación de manera consistente en varios métodos HTTP, evitando la repetición de código.

- `Paginación`: Para implementar la paginación de resultados en recursos que devuelven conjuntos extensos de datos.

- `Limitación de Velocidad`: Los traits facilitan la incorporación de límites de velocidad para controlar el acceso y prevenir abusos.

- `Validación de Parámetros`: Para establecer reglas de validación de parámetros y asegurar la integridad de las solicitudes.

## 2. Definición y Referencia:

Los traits pueden ser definidos en un archivo RAML separado, promoviendo la reutilización en múltiples especificaciones. Alternativamente, pueden ser definidos directamente dentro del mismo archivo RAML donde se utilizan.

```yaml
traits:
  - authenticationTrait:
      headers:
        Authorization:
          type: string
          description: "Token de autenticación"
```

Este ejemplo define un trait llamado `authenticationTrait` que especifica la inclusión de un encabezado de autorización en la solicitud.

## 3. Aplicación en Métodos HTTP:

Los traits se aplican a los métodos HTTP de un recurso utilizando la clave `is` seguida de una lista de los nombres de los traits.

```yaml
/exampleResource:
  get:
    is: [authenticationTrait]
```

En este caso, el trait `authenticationTrait` se aplica al método HTTP GET del recurso `/exampleResource`.

## 4. Parámetros en Traits:

Los traits pueden aceptar parámetros, permitiendo la personalización de su comportamiento en diferentes contextos.

```yaml
traits:
  - paginationTrait:
      queryParameters:
        page:
          type: number
          default: 1
          description: "Número de página"
        pageSize:
          type: number
          default: 10
          description: "Número de elementos por página"
```

## 5. Personalización en Recursos:

Los parámetros de traits pueden ser personalizados al 
aplicar el trait en un recurso específico.

```yaml
/paginatedResource:
  get:
    is: [paginationTrait(pageSize=15)]
```

En este ejemplo, el trait `paginationTrait` se aplica al recurso `/paginatedResource` con un tamaño de página personalizado de 15.

<hr/>

Los traits en RAML ofrecen una forma efectiva de modularizar y estandarizar comportamientos comunes en una API, promoviendo la reutilización de código y la coherencia en toda la especificación. Al comprender cuándo y cómo utilizarlos, los desarrolladores pueden mejorar la mantenibilidad y la consistencia de sus especificaciones de API.

# Ejemplo práctico

1. Justo debajo del `resourceType` crearemos nuestro `trait`

2. Dicho `trait` tendra el siguiente código:

    ```yaml
    traits:
        typeOfVerification:
            queryParameters:
                username: string
                password: string
                mobileOtp: string
    ```

3. Luego modificaremos nuestro `/getAccountSummary` agregando el trait haciendo uso del `is`:

    ```yaml
    /getAccountSummary:
        description: "To get bank account details of a customer"
        get:
            queryParameters:
                accountNumber:
                    type: number
            is:
                - typeOfVerification
    ```

4. Si revisamos la sección de `Documentation` veremos que ahora nos pide los datos que definimos en el `trait`

5. Agregamos el `trait` de igual manera en `/createAccount`

6. Pero el `mobileOtp` no nos interesa tenerlo en `createAccount`, así que haremos unos cambios, comenzando desde el `trait`:

    ```yaml
    traits:
        typeOfVerification:
            queryParameters:
                username: string
                password: string
                <<typeOfOtp>>: string
    ```

7. Eso hará que nos salten errores es donde usamos con anterioridad el `trait`, corregiremos eso, comenzando por `/getAccountSummary`

    ```yaml
    /getAccountSummary:
        description: "To get bank account details of a customer"
        get:
            queryParameters:
                accountNumber:
                    type: number
            is:
                - {typeOfVerification : {typeOfOtp: "mobileOtp"}}
    ```

8. Finalmente en `/createAccount` hacemos lo mismo, solo que en vez de tener **"mobileOtp"**, colocaremos **emailOtp**

9. Revisamos en `Documentation` que todo vaya en orden

# Definir un esquema usando `Data Types`

1. En el botón `+` crearemos un nuevo `file`, el cuál tendra el nombre de `createAccountRequirements` y sera de tipo `Data Type`

2. Dentro del archivo agregamos el siguiente código:

```yaml
type: object
properties:
  accountNumber: number
  address:
    type: array
    items:
      properties:
        typeOfAddress:
          type: string
          enum:
            - "home"
            - "work"
        city:
          type: string
          minLength: 3
          maxLength: 15
        zip:
          type: number
          minimum: 0000
          maximum: 9999
```

#### Explicación:

> [!NOTE]
> Pendiente

3. Modificamos el `/createAccount` de la siguiente manera:

```yaml
/createAccount:
  description: "To create a new account"
  post:
    queryParameters:
      branch: string
    is:
      - {typeOfVerification : {typeOfOtp: "emailOtp"}}
    body:
      application/json:
        type: !include createAccountRequeriments.raml
        example:
          {
            "accountNumber": 1234,
            "address": [{
              "typeOfAddress": "home",
              "city": "edomex",
              "zip": 1234
            }]
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

4. Pero en vez de tener el ejemplo ahí, creamos un nuevo `file` de tipo `example` de nombre `createRequestExample` y agregamos el ejemplo anterior:

```yaml
#%RAML 1.0 NamedExample
{
  "accountNumber": 1234,
  "address": [{
    "typeOfAddress": "home",
    "city": "edomex",
    "zip": 1234
  }]
}
```

5. Modificamos de nuevo `/createAccount`:

```yaml
/createAccount:
  description: "To create a new account"
  post:
    queryParameters:
      branch: string
    is:
      - {typeOfVerification : {typeOfOtp: "emailOtp"}}
    body:
      application/json:
        type: !include createAccountRequeriments.raml
        example: !include createRequestExample.raml
```