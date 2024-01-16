# Consideraciones Importantes al Conectar con Sistemas Externos

Conectar con sistemas externos, ya sean bases de datos, Salesforce, SFTP o cualquier servicio web externo, requiere una atención cuidadosa y una comprensión detallada. Aquí hay aspectos clave a tener en cuenta:

**Detalles de Configuración:**
Antes de establecer la conexión, recopile los detalles de configuración esenciales necesarios para conectarse con el sistema externo. Esto puede incluir información como URL, credenciales, claves de API y otros parámetros relevantes.

**Requisitos de la Petición:**
Comprenda la estructura y los requisitos de la petición que el sistema externo espera. Esto implica conocer los parámetros necesarios, el formato de la solicitud y cualquier consideración específica de la API.

**Seguridad:**
Evalúe los aspectos de seguridad asociados con la conexión al sistema externo. Determine si se requieren tokens de autenticación, certificados SSL u otras medidas de seguridad para establecer una comunicación segura.

**Tipo de Respuesta:**
Explore el tipo de respuesta que el sistema externo devuelve. Comprender la estructura y el formato de la respuesta es fundamental para procesar y utilizar eficazmente los datos recuperados.

**Tiempo de Respuesta:**
Es beneficioso conocer el tiempo que el sistema externo tarda en responder. Esto permite configurar adecuadamente los tiempos de espera para gestionar situaciones en las que la respuesta puede demorarse.

Estas consideraciones garantizan una integración exitosa y eficiente con sistemas externos, asegurando que la comunicación sea fluida, segura y alineada con los requisitos específicos de cada sistema.

# HTTP Request

El componente HTTP Request se utiliza para realizar llamadas a otros servicios web, lo que es esencial en la integración de sistemas. Algunos puntos clave sobre su uso y configuración son los siguientes:

**Ubicación en el Flujo:**
El componente HTTP Request debe colocarse únicamente en el área "Process" de un flujo, private-flow o sub-flow.

**Configuración Necesaria:**
Asegúrese de proporcionar todos los datos necesarios para el componente HTTP Request, como la URL del servicio, el método HTTP a utilizar, y la carga útil esperada por el servicio externo. Además, considere los atributos esenciales como queryParams y headers.

**Detalles de Configuración Obligatorios:**
- **Protocolo:** Especifica el protocolo a utilizar (HTTP o HTTPS).
- **Método:** Indica el método HTTP que se utilizará para la solicitud (GET, POST, PUT, DELETE, etc.).
- **URL o Ruta:** Si proporciona la URL completa, no son necesarios detalles como host y puerto. Sin embargo, si proporciona solo la ruta, es necesario especificar host, puerto y ruta.
  
**Configuración Adicional:**
Puede pasar información adicional en las pestañas correspondientes del componente:
- **Cuerpo (Body):** Contiene la carga útil de la solicitud.
- **Encabezados (Headers):** Permite especificar encabezados HTTP adicionales.
- **Atributos (Attributes):** Proporciona detalles adicionales sobre la solicitud.
- **Parámetros de Consulta (QueryParams):** Permite incluir parámetros de consulta en la solicitud.

La configuración precisa de estos detalles garantiza una comunicación efectiva con el servicio externo, evitando errores y asegurando que la solicitud cumpla con los requisitos del servicio. La ubicación adecuada y la atención a los detalles son fundamentales para el correcto funcionamiento del componente HTTP Request en un flujo de integración.

# Actividad, Parte 1

1. Para la realización de las siguientes actividades será necesario hacer uso de alguna API externa, en este caso sera la de [Weatherstack](https://weatherstack.com/), crea una cuenta gratis para poder hacer uso de su `api key`

2. Una vez tengamos nuestro dashboad a la vista iremos a la [documentación](https://weatherstack.com/documentation) de dicha API

> [!IMPORTANT]
> Al ser una cuenta de uso gratis hay que usar `http` en lugar de `https` en la URL, ya que de no ser asi nos dara un mensaje de que debemos actualizar nuestro plan de pago

3. Hacemos una pequeña prueba de que todo este en orden ya sea en nuestro navegador o en nuestro tester de APIs, como en este caso que usare RapidAPI dondé colocaremos la siguiente URL
    ```url
    http://api.weatherstack.com/current?access_key={API_KEY}&query=New%20York
    ```

    si todo sale bien, nos dara una respuesta como esta:
    ```json
    {
        "request": {
            "type": "City",
            "query": "New York, United States of America",
            "language": "en",
            "unit": "m"
        },
        "location": {
            "name": "New York",
            "country": "United States of America",
            "region": "New York",
            "lat": "40.714",
            "lon": "-74.006",
            "timezone_id": "America/New_York",
            "localtime": "2024-01-16 12:22",
            "localtime_epoch": 1705407720,
            "utc_offset": "-5.0"
        },
        "current": {
            "observation_time": "05:22 PM",
            "temperature": -1,
            "weather_code": 311,
            "weather_icons": [
            "https://cdn.worldweatheronline.com/images/wsymbols01_png_64/wsymbol_0021_cloudy_with_sleet.png"
            ],
            "weather_descriptions": [
            "Light Freezing Rain, Mist"
            ],
            "wind_speed": 17,
            "wind_degree": 60,
            "wind_dir": "ENE",
            "pressure": 1008,
            "precip": 0.9,
            "humidity": 89,
            "cloudcover": 100,
            "feelslike": -5,
            "uv_index": 1,
            "visibility": 5,
            "is_day": "yes"
        }
    }
    ```

4. Abrimos nuestro `Anypoint Studio` en nuestro proyecto que llevamos trabajando desde los capitulos anteriores

5. Vamos a eliminar nuetros 2 `Logger` y nuestro `Transform Message`

6. Arrastramos el conector `HTTP Request` y lo colocamos a lado de nuestro `Listener`

<div align="center">
    <img src="./img/practica4-ejemplo1.png" alt="ventana" width="700"/>
</div>

7. Ahora en su configuración vamos a hacer lo siguiente, su `Display Name` colocaremos `Weater API`

8. Ahora en `Configuration` simplemente daremos click en el `+` dejaremos todo por default y simplemente damos `ok`

9. En `URL` colocaremos la url base que en este caso sería `http://api.weatherstack.com/current`, lo pondremos tal cual, sin dar click en _`Fx`_ o encerrarlo entre `""`

10. Más abajo veremos una serie de pestañas entre las que se encuentra `Query Parameters`, daremos click ahí y seguido de eso en el simbolo de `+`, debera aparecer un primer renglón en la tabla con datos por default, podremos modificarlos directamente o dar click en _`Fx`_, deberemos agregar el `query parameter` de nuestro `API_KEY` de la siguiente manera:
    ```json
    output application/java
    ---
    {
        "access_key" : "6640303e2b004bdd1409b2aaaf3ecbcc",
        "query" : "Mexico"
    }
    ```

    volvemos a dar click en _`Fx`_ y veremos que los datos del primer renglón y segundo renglón de la tabla ya coinciden con lo que colocamos en código

> [!NOTE]
> Recordemos que acces_key y query vienen de la URL que nos da la documentación de Weatherstack y que probamos previamente en el paso 3

11. Agregamos un `Tranform Message` a lado de nuestro `Request` y simplemente en el código agregamos lo siguiente:
    ```properties
    %dw 2.0
    output application/json
    ---
    payload
    ```

12. Guardamos todo, compilamos y si recordamos la URL para probar nuestra aplicación es en el puerto `8083`, osease: `http://localhost:8083/test`, probamos dicha URL con Postman o RapidAPI y debería retornarnos la respuesta de la API de `Weatherstack`

<div align="center">
    <img src="./img/practica4-ejemplo2.png" alt="ventana" width="700"/>
    <img src="./img/practica4-ejemplo3.png" alt="ventana" width="700"/>
</div>