---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# La interfaz HTTP asíncrona
{: #async}

La interfaz HTTP asíncrona del servicio {{site.data.keyword.speechtotextfull}} proporciona métodos para transcribir audio a través de llamadas de no bloqueo al servicio. La interfaz utiliza series secretas especificadas por el usuario y firmas digitales para proporcionar un nivel de seguridad para las solicitudes que se realizan a través del protocolo HTTP. Para utilizar la interfaz asíncrona, puede
{: shortdesc}

-   Registrar un URL de devolución de llamada para que el servicio le notifique automáticamente el estado del trabajo y los resultados.
-   Sondear el servicio para obtener manualmente el estado del trabajo y los resultados.

Los dos enfoques no son mutuamente excluyentes. Puede optar por recibir notificaciones de devolución de llamada pero seguir sondeando el servicio para ver el estado más reciente o ponerse en contacto con el servicio para recuperar resultados manualmente. En las secciones siguientes se describe cómo utilizar la interfaz HTTP asíncrona con ambos enfoques.

Envíe un máximo de 1 GB y un mínimo de 100 bytes de datos de audio con una sola solicitud. Para obtener información acerca de los formatos de audio y sobre el uso de la compresión para maximizar la cantidad de audio que puede enviar con una solicitud, consulte [Formatos de audio](/docs/services/speech-to-text/audio-formats.html). Para obtener más información acerca de los métodos individuales de la interfaz, revise la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

## Modelos de uso
{: #usage}

Cuando trabaje con la interfaz HTTP asíncrona del servicio, puede ver el estado del trabajo y recibir resultados de las siguientes maneras:

-   Mediante el uso de notificaciones de devolución de llamada:
    1.  Llame al método `POST /v1/register_callback` para registrar un URL de devolución de llamada con el servicio. Puede proporcionar una serie secreta opcional especificada por el usuario para habilitar la autenticación y la integridad de los datos para las devoluciones de llamada que se envían al URL.
    1.  Llame al método `POST /v1/recognitions` con un URL de devolución de llamada ya registrado al que el servicio envía notificaciones cuando cambia el estado de los trabajos. Debe especificar una lista de los sucesos sobre los que desea recibir notificación. De forma predeterminada, el servicio envía notificaciones cuando se inicia un trabajo, cuando finaliza y si se produce un error. También puede solicitar los resultados de la solicitud en la notificación de finalización. De lo contrario, tendrá que utilizar el método `GET /v1/recognitions/{id}` para recuperar los resultados.
-   Mediante el sondeo del servicio:
    1.  Llame al método `POST /v1/recognitions` sin URL de devolución de llamada, sucesos ni señal de usuario.
    1.  Llame de forma periódica al método `GET /v1/recognitions` para comprobar el estado de los trabajos más recientes o al método `GET /v1/recognitions/{id}` para comprobar el estado de un trabajo específico.
    1.  Si comprueba el estado del trabajo con el método `GET /v1/recognitions`, llame al método `GET /v1/recognitions/{id}` para recuperar los resultados del trabajo una vez que haya finalizado.

Los dos enfoques se pueden utilizar juntos. Puede sondear el servicio para obtener el estado más reciente de un trabajo que se ha creado con un URL de devolución de llamada. Por ejemplo, es posible que desee obtener el estado de un trabajo si las notificaciones tardan en llegar. También es posible que quiera comprobar el estado si sospecha que ha perdido una o varias notificaciones debido a un error del servicio o de la red.

## Registro de un URL de devolución de llamada
{: #register}

Un URL de devolución de llamada se registra mediante una llamada al método `POST /v1/register_callback`. Una vez que haya registrado un URL de devolución de llamada, puede utilizarlo para recibir notificaciones para un número indefinido de trabajos. El proceso de registro consta de cuatro pasos:

1.  Llama al método `POST /v1/register_callback` y pasa un URL de devolución de llamada. Si lo desea, también puede especificar un secreto especificado por el usuario. El servicio utiliza el secreto para calcular las firmas de algoritmo hash seguro 1 (SHA1) con código de autenticación de mensajes con clave hash (HMAC) para la autenticación y la integridad de los datos. En el ejemplo siguiente se registra una devolución de llamada de usuario que responde en el URL `http://{user_callback_path}/results`. La llamada incluye el secreto de usuario `ThisIsMySecret`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/register_callback?callback_url=http://{user_callback_path}/results&user_secret=ThisIsMySecret"
    ```
    {: pre}

1.  El servicio intenta validar, o colocar en la lista blanca, el URL de devolución de llamada si todavía no se ha registrado enviando una solicitud `GET` al URL de devolución de llamada. El servicio pasa una serie alfanumérica aleatoria de cambio de código mediante el parámetro de consulta `challenge_string` de la solicitud. La solicitud incluye una cabecera `Accept` que especifica `text/plain` como el tipo de respuesta necesario.

    Si la llamada al método `register_callback` incluía un secreto de usuario, la solicitud `GET` procedente del servicio también incluye una cabecera `X-Callback-Signature` que especifica la firma HMAC-SHA1 de la serie de cambio. El servicio calcula la firma utilizando como clave el secreto de usuario.

    ```
    GET http://{user_callback_path}/results?challenge_string=n9ArPGMQ36Hiu7QC
    header: X-Callback-Signature {HMAC-SHA1_signature}
    ```
    {: codeblock}

1.  El usuario responde a la solicitud `GET` procedente del servicio con el código de estado 200. Incluya la serie de cambio que ha enviado el servicio en la respuesta. Incluya la serie en texto sin formato en el cuerpo de la respuesta y establezca la cabecera de respuesta `Content-Type` en `text/plain`.

    Si la solicitud `POST` inicial incluía un secreto de usuario, puede calcular la firma HMAC-SHA1 de la serie de cambio utilizando el secreto como clave. Si el servicio envió la solicitud `GET`, la firma coincide con el valor especificado por la cabecera `X-Callback-Signature`.

    ```
    response code: 200 OK
    body: n9ArPGMQ36Hiu7QC
    ```
    {: codeblock}

1.  El servicio comprueba si la serie de cambio se devuelve en el cuerpo de la respuesta a su solicitud `GET`. Si es así, el servicio coloca el URL de devolución de llamada en la lista blanca y responde a la solicitud `POST` original con el código de estado 201. El cuerpo de la respuesta incluye un objeto JSON con un campo `status` que tiene el valor `created` y un campo `url` que tiene el valor del URL de devolución de llamada.

    ```
    response code: 201 Created
    body: {
      "status": "created",
      "url": "http://{user_callback_path}/results"
    }
    ```
    {: codeblock}

El servicio solo envía una solicitud `GET` a un URL de devolución de llamada durante el proceso de registro. El servicio debe recibir una respuesta con el código de respuesta 200 que incluya la serie de cambio en su cuerpo en un plazo de cinco segundos. Si no es así, no coloca el URL en la lista blanca. En lugar de ello, envía el código de estado 400 como respuesta a la solicitud `POST /v1/register_callback`. Si el URL de devolución de llamada ya se había colocado correctamente en la lista blanca, el servicio envía el código de estado 200 como respuesta a la solicitud `POST` inicial.

### Consideraciones sobre seguridad
{: #security-async}

Cuando haya utilizado correctamente el método `POST /v1/register_callback` para registrar un URL de devolución de llamada, el servicio coloca el URL en la lista blanca para indicar que se ha verificado y se puede utilizar con notificaciones de devolución de llamada. Si especifica un secreto de usuario con la llamada de registro, la lista blanca también significa que el URL se ha validado para añadir seguridad. La especificación de un secreto de usuario proporciona autenticación e integridad de datos para las solicitudes que utilizan el URL de devolución de llamada con la interfaz HTTP asíncrona.

El servicio utiliza el secreto de usuario para calcular una firma HMAC-SHA1 en la carga útil de cada notificación de devolución de llamada que envía al URL. El servicio envía la firma mediante la cabecera `X-Callback-Signature` con cada notificación. El cliente puede utilizar el secreto para calcular su propia firma de cada carga útil de notificación. Si su firma coincide con el valor de la cabecera `X-Callback-Signature`, el cliente sabe que la notificación ha sido enviada por el servicio y que su contenido no se ha alterado durante la transmisión. Esto garantiza que el cliente no es víctima de un ataque de intermediario.

HTTPS resulta ideal para aplicaciones de producción. Sin embargo, durante el desarrollo de aplicaciones y la creación de prototipos, las notificaciones de devolución de llamada basadas en HTTP que reciben soporte del servicio pueden simplificar y acelerar el proceso de desarrollo evitando el gasto de HTTPS.

<!--

However, communicating over the HTTPS protocol is always the most secure means of learning job status and retrieving results. Using the HTTPS `GET /v1/recognitions/{id}` method to retrieve the results of a job is therefore more secure that receiving the results via callback notification. While the use of HMAC-SHA1 signatures based on a user secret ensures authentication and data integrity for callback notifications, it does not provide confidentiality. Conversely, because it encrypts the body of the response, HTTPS can provide authentication, integrity, *and* confidentiality.

HTTPS, however, is not ideal in terms of additional development overhead. Moreover, although the service validates SSL certificates to prevent man-in-the-middle attacks when you use HTTPS, validation is not foolproof if you use self-signed certificates, which enable encryption but not authentication or data integrity. During application development, user secrets and digital signatures provide a suitable level of security for requests made over the HTTP protocol. You can work with HTTP callback notifications during prototyping and move to HTTPS only for application deployment.

-->

### Anulación del registro de un URL de devolución de llamada
{: #unregister}

Puede anular el registro de un URL de devolución de llamada colocado en la lista blanca en cualquier momento mediante una llamada al método `POST /v1/unregister_callback`. La anulación del registro de un URL de devolución de llamada puede resultar útil para probar la aplicación con el servicio. Cuando haya anulado el registro de un URL de devolución de llamada, ya no puede utilizarlo con solicitudes de reconocimiento asíncronas.

## Creación de un trabajo
{: #create}

Un trabajo de reconocimiento se crea mediante una llamada al método `POST /v1/recognitions`. El modo en que recibe el estado y los resultados del trabajo depende del enfoque que utilice y de los parámetros que pase:

-   *Para utilizar notificaciones de devolución de llamada*, incluya el parámetro de consulta `callback_url` para especificar un URL al que el servicio debe enviar notificaciones de devolución de llamada cuando cambie el estado del trabajo. También puede especificar los siguientes parámetros de consulta opcionales:
    -   `events` para suscribirse a una lista de sucesos de notificación. De forma predeterminada, el servicio envía notificaciones de devolución de llamada cuando se inicia el trabajo (suceso `recognitions.started`), cuando finaliza el trabajo (suceso `recognitions.completed`) y si se produce un error (suceso `recognitions.failed`). Puede especificar un subconjunto de sucesos o utilizar el suceso `recognitions.completed_with_results` en lugar del suceso `recognitions.completed` para incluir los resultados con la notificación de finalización del trabajo.
    -   `user_token` para especificar una serie que se va a incluir con cada notificación para el trabajo. Puesto que puede utilizar el mismo URL de devolución de llamada con un número indefinido de trabajos, puede aprovechar las señales de usuario para diferenciar las notificaciones correspondientes a distintos trabajos.
-   *Para utilizar el sondeo*, omita los parámetros de consulta `callback_url`, `events` y `user_token`. Debe utilizar los métodos `GET /v1/recognitions` o `GET /v1/recognitions/{id}` para comprobar el estado del trabajo, utilizando el último para recuperar los resultados cuando finaliza el trabajo.

En ambos casos, puede incluir el parámetro de consulta `results_ttl` para especificar el número de minutos durante los que deben estar disponibles los resultados después de que finalice el trabajo.

Además de los parámetros anteriores, que son específicos de la interfaz asíncrona, el método `POST /v1/recognitions` da soporte a la mayoría de los parámetros a los que dan soporte las interfaces WebSocket y HTTP síncrona. Para obtener más información, consulte el [Resumen de parámetros](/docs/services/speech-to-text/summary.html).

### Notificaciones de devolución de llamada
{: #notifications}

Si el trabajo se crea con un URL de devolución de llamada, el servicio envía una notificación de devolución de llamada `POST` de HTTP al URL registrado cuando se produce un suceso especificado. El cuerpo de una notificación básica consta de un objeto JSON con la estructura siguiente:

```javascript
{
  "id": "{job_id}",
  "event": "{recognitions_status}",
  "user_token": "{user_token}"
}
```
{: codeblock}

El campo `id` identifica el ID del trabajo que ha generado la devolución de llamada y el campo `event` identifica el suceso que ha desencadenado la devolución de llamada. El campo `user_token` incluye la señal de usuario para el trabajo si se ha especificado una. De lo contrario, el campo es una serie vacía. Si el suceso es `recognitions.completed_with_results`, el objeto incluye un campo `results` que proporciona los resultados de la solicitud de reconocimiento. El cliente puede responder a la notificación de devolución de llamada con el código de estado 200.

Si el URL de devolución de llamada se ha registrado con un secreto de usuario, el servicio también envía la cabecera `X-Callback-Signature` con la notificación de devolución de llamada. La cabecera especifica la firma HMAC-SHA1 del cuerpo de la solicitud. El servicio calcula la firma utilizando el secreto de usuario como clave. El cliente puede calcular la firma de la carga útil de la notificación de devolución de llamada para asegurarse de que coincide con la firma de la cabecera. Por ejemplo, el siguiente código de Python sencillo calcula la firma en la serie `notification_payload`:

```python
from hashlib import sha1
import hmac
hashed = hmac.new(user_secret, notification_payload, sha1)
signature = hashed.digest().encode("base64").rstrip('\n')
```
{: codeblock}

### Ejemplo de devolución de llamada
{: #callback}

En el ejemplo siguiente se crea un trabajo que está asociado con el URL de devolución de llamada colocado anteriormente en la lista blanca `http://{user_callback_path}/results`. El ejemplo pasa la señal de usuario `job25` para identificar el trabajo en las notificaciones de devolución de llamada que envía el servicio. La llamada utiliza los sucesos predeterminados, por lo que el usuario debe llamar al método `GET /v1/recognitions/{id}` para recuperar los resultados cuando el servicio envía una notificación de devolución de llamada para indicar que el trabajo ha finalizado. La llamada establece el parámetro de consulta `timestamps` de la solicitud de reconocimiento en `true`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?callback_url=http://{user_callback_path}/results&user_token=job25&timestamps=true"
```
{: pre}

El servicio devuelve el estado de la solicitud, que es `waiting`, para indicar que el servicio está preparando el trabajo para su proceso. La respuesta incluye la hora de creación, el ID de trabajo y el URL en el que obtener más información sobre el trabajo.

```javascript
{
  "created": "2016-08-17T19:15:17.926Z",
  "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bd734c0-e575-21f3-de03-f932aa0468a0",
  "status": "waiting"
}
```
{: codeblock}

### Ejemplo de sondeo
{: #polling}

En el ejemplo siguiente se crea un trabajo que no está asociado con un URL de devolución de llamada. El usuario debe sondear el servicio para saber cuándo finaliza el trabajo y luego recuperar los resultados con el método `GET /v1/recognitions/{id}`. Al igual que en el ejemplo anterior, la llamada establece el parámetro `timestamps` de la solicitud de reconocimiento en `true`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?timestamps=true"
```
{: pre}

El servicio devuelve el estado `processing` para indicar que ya está procesando el trabajo, junto con la hora de creación, el ID de trabajo y el URL para obtener información sobre el trabajo.

```javascript
{
  "created": "2016-08-17T19:13:23.622Z",
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "status": "processing"
}
```
{: codeblock}

## Comprobación del estado y recuperación de los resultados de un trabajo
{: #job}

Llame al método `GET /v1/recognitions/{id}` para comprobar el estado del trabajo especificado con el parámetro de vía de acceso `id`. La respuesta siempre incluye el ID y el estado del trabajo y su hora de creación y de actualización. Si el estado es `completed`, la respuesta también incluye los resultados de la solicitud de reconocimiento.

El método `GET /v1/recognitions/{id}` es el único sistema para recuperar los resultados del trabajo si

-   El trabajo se ha enviado sin un URL de devolución de llamada.
-   El trabajo se ha enviado con un URL de devolución de llamada pero sin especificar el suceso `recognitions.completed_with_results`.
-   El trabajo no es uno de los 100 trabajos pendientes más recientes. Cuando se omite el parámetro de vía de acceso `id`, solo se devuelven los 100 trabajos más recientes.

Sin embargo, todavía puede utilizar el método para recuperar los resultados para un trabajo que ha especificado un URL de devolución de llamada y el suceso `recognitions.completed_with_results`. Puede recuperar los resultados de cualquier trabajo tantas veces como desee mientras estén disponibles. Un trabajo y sus resultados están disponibles hasta que los suprime con el método `DELETE /v1/recognitions/{id}` o hasta que vence el tiempo de vida del trabajo, lo que se produce en primer lugar. De forma predeterminada, los resultados caducan después de una semana a menos que especifique otro tiempo de vida con el parámetro `results_ttl` del método `POST /v1/recognitions`.

### Ejemplo de estado sin resultados
{: #withoutResults}

En el ejemplo siguiente se comprueba el estado del trabajo con el ID especificado. El trabajo aún no ha finalizado, por lo que la respuesta no incluye resultados.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}

```javascript
{
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "created": "2016-08-17T19:13:23.622Z",
  "updated": "2016-08-17T19:13:24.434Z",
  "status": "processing"
}
```
{: codeblock}

### Ejemplo de estado con resultados
{: #withResults}

En el ejemplo siguiente se solicita el estado del trabajo con el ID especificado. El trabajo ha finalizado, por lo que la respuesta incluye los resultados de la solicitud.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}

```javascript
{
  "id": "398fcd80-330a-22ba-93ce-1a73f454dd98",
  "results": [
    {
      "result_index": 0,
      "results": [
        {
          "final": true,
          "alternatives": [
            {
              "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday ",
              "timestamps": [
                [
                  "several",
                  1,
                  1.52
                ],
                [
                  "tornadoes",
                  1.52,
                  2.15
                ],
                . . .
                [
                  "Sunday",
                  5.74,
                  6.33
                ]
              ],
              "confidence": 0.89
            }
          ]
        }
      ]
    }
  ],
  "created": "2016-08-17T19:11:04.298Z",
  "updated": "2016-08-17T19:11:16.003Z",
  "status": "completed"
}
```
{: codeblock}

## Comprobación del estado de los trabajos más recientes
{: #jobs}

Llame al método `GET /v1/recognitions` para comprobar el estado de los trabajos más recientes. El método devuelve el estado de los 100 trabajos pendientes más recientes que están asociados a las credenciales de servicio con las que se llama. El método devuelve el ID y el estado de cada trabajo, junto con su hora de creación y de actualización. Si un trabajo se ha creado con un URL de devolución de llamada y una señal de usuario, el método también devuelve la señal de usuario correspondiente al trabajo.

La respuesta incluye uno de los estados siguientes:

-   `waiting` si el servicio está preparando el trabajo para su proceso. Este estado es el estado inicial de todos los trabajos. El trabajo permanece en este estado hasta que el servicio tiene capacidad para empezar a procesarlo.
-   `processing` si el servicio está procesando el trabajo de forma activa.
-   `completed` si el servicio ha terminado de procesar el trabajo. Si el trabajo especificada un URL de devolución de llamada y el suceso `recognitions.completed_with_results`, el servicio envía los resultados con la notificación de devolución de llamada. Si no es así, utilice el método `GET /v1/recognitions/{id}` para obtener los resultados.
-   `failed` si el trabajo ha fallado por algún motivo.

Un trabajo y sus resultados están disponibles hasta que los suprime con el método `DELETE /v1/recognitions/{id}` o hasta que vence el tiempo de vida del trabajo, lo que se produce en primer lugar.

### Ejemplo
{: #statusExample}

En el ejemplo siguiente se solicita el estado de los trabajos actuales más recientes que están asociados con las credenciales de servicio del emisor de la llamada. El usuario tiene tres trabajos pendientes en diversos estados. El primer trabajo se ha creado con un URL de devolución de llamada y una señal de usuario.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions"
```
{: pre}

```javascript
{
  "recognitions": [
    {
      "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
      "created": "2016-08-17T19:15:17.926Z",
      "updated": "2016-08-17T19:15:17.926Z",
      "status": "waiting",
      "user_token": "job25"
    },
    {
      "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
      "created": "2016-08-17T19:13:23.622Z",
      "updated": "2016-08-17T19:13:24.434Z",
      "status": "processing"
    },
    {
      "id": "398fcd80-330a-22ba-93ce-1a73f454dd98",
      "created": "2016-08-17T19:11:04.298Z",
      "updated": "2016-08-17T19:11:16.003Z",
      "status": "completed"
    }
  ]
}
```
{: codeblock}

## Supresión de un trabajo
{: #delete-async}

Puede utilizar el método `DELETE /v1/recognitions/{id}` para suprimir el trabajo especificado con el parámetro de vía de acceso `id`. Un trabajo se suele suprimir después de obtener sus resultados del servicio. Una vez que se suprime un trabajo, sus resultados dejan de estar disponibles. No puede suprimir un trabajo que el servicio esté procesando de forma activa.

De forma predeterminada, el servicio mantiene los resultados de cada trabajo hasta que caduca el tiempo de vida del trabajo. El tiempo de vida predeterminado es una semana, pero puede utilizar el parámetro `results_ttl` del método `POST /v1/recognitions` para especificar el número de minutos que el servicio debe mantener los resultados.

### Ejemplo
{: #deleteExample-async}

En el ejemplo siguiente se suprime el trabajo con el ID especificado:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}
