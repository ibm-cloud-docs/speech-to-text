---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-24"

subcollection: speech-to-text

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
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

# La interfaz WebSocket
{: #websockets}

La interfaz WebSocket del servicio {{site.data.keyword.speechtotextshort}} es la forma más natural de que un cliente interactúe con el servicio. Para utilizar la interfaz WebSocket para el reconocimiento de voz, primero debe utilizar el método `/v1/recognize` para establecer una conexión permanente con el servicio. A continuación, debe enviar mensajes de texto y binarios a través de la conexión para iniciar y gestionar las solicitudes de reconocimiento.
{: shortdesc}

El ciclo de solicitud de reconocimiento y respuesta consta de los pasos siguientes:

1.  [Abrir una conexión](#WSopen)
1.  [Iniciar una solicitud de reconocimiento](#WSstart)
1.  [Enviar audio y recibir los resultados del reconocimiento](#WSaudio)
1.  [Finalizar una solicitud de reconocimiento](#WSstop)
1.  [Enviar solicitudes adicionales y modificar los parámetros de la solicitud](#WSmore)
1.  [Mantener activa una conexión](#WSkeep)
1.  [Cerrar una conexión](#WSclose)

Cuando el cliente envía datos al servicio, *debe* pasar todos los mensajes JSON como mensajes de texto y todos los datos de audio como mensajes binarios.

Los fragmentos de código de ejemplo que se muestran a continuación se han escrito en JavaScript y se basan en la API de WebSocket HTML5. Para obtener más información sobre el protocolo WebSocket, consulte [Request for Comment (RFC) 6455](http://tools.ietf.org/html/rfc6455){: external} en la publicación Internet Engineering Task Force (IETF).
{: note}

## Abrir una conexión
{: #WSopen}

El servicio {{site.data.keyword.speechtotextshort}} utiliza el protocolo WebSocket Secure (WSS) para hacer que el método `/v1/recognize` esté disponible en el punto final siguiente:

```
wss://{host_name}/speech-to-text/api/v1/recognize
```
{: codeblock}

donde `{host_name}` es la ubicación en la que está alojada la aplicación:

-   `stream.watsonplatform.net` para Dallas (en los ejemplos siguientes se utiliza este nombre de host)
-   `stream-fra.watsonplatform.net` para Frankfurt
-   `gateway-syd.watsonplatform.net` para Sídney
-   `gateway-wdc.watsonplatform.net` para Washington, DC
-   `gateway-tok.watsonplatform.net` para Tokio
-   `gateway-lon.watsonplatform.net` para Londres

Un cliente WebSocket llama a este método con los siguientes parámetros de consulta para establecer una conexión autenticada con el servicio. Si utiliza la autenticación de Identity and Access Management (IAM), utilice el parámetro de consulta `access_token`. Si utiliza las credenciales de servicio de Cloud Foundry, utilice el parámetro de consulta `watson-token`.

<table>
  <caption>Tabla 1. Parámetros del método <code>/v1/recognize</code></caption>
  <tr>
    <th style="width:23%; text-align:left">Parámetro</th>
    <th style="width:12%; text-align:center">Tipo de datos</th>
    <th style="text-align:left">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>access_token</code>
      <br/><em>      Opcional
    </em></td>
    <td style="text-align:center">Serie</td>
    <td style="text-align:left">
      <em>Si utiliza la autenticación IAM,</em> pase una señal de acceso de IAM válida para establecer una conexión autenticada con el servicio.
      Pase una señal de acceso de IAM en lugar de pasar una clave de API con la llamada. Debe establecer la conexión antes de que caduque la señal de acceso. Para obtener información sobre cómo obtener una señal de acceso, consulte [Autenticación con señales de IAM](/docs/services/watson?topic=watson-iam).<br/><br/>
      Pase una señal de acceso sol para establecer una conexión autenticada.
      Una vez establecida la conexión, puede mantenerla activa de forma indefinida.
      Seguirá estando autenticado mientras mantenga la conexión abierta.
      No es necesario renovar la señal de acceso para una conexión activa que dure más que el periodo de caducidad de la señal. Una conexión puede permanecer activa incluso después de suprimir la señal o su clave de API.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>watson-token</code>
      <br/><em>      Opcional
    </em></td>
    <td style="text-align:center">Serie</td>
    <td style="text-align:left">
      <em>Si utiliza credenciales de servicio de Cloud Foundry,</em> pase una señal de autenticación de {{site.data.keyword.watson}} válida para establecer una conexión autenticada con el servicio. Pase una señal de {{site.data.keyword.watson}} en lugar de pasar credenciales del servicio con la llamada. Las señales de {{site.data.keyword.watson}} se basan en credenciales de servicio de Cloud Foundry, que utilizan `username` y `password` para la autenticación básica de HTTP. Para obtener información sobre cómo obtener una señal de {{site.data.keyword.watson}}, consulte [señales de {{site.data.keyword.watson}}](/docs/services/watson?topic=watson-gs-tokens-watson-tokens).<br/><br/>
      Pase una señal de {{site.data.keyword.watson}} solo para establecer una conexión autenticada. Una vez establecida la conexión, puede mantenerla activa de forma indefinida. Seguirá estando autenticado mientras mantenga la conexión abierta.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>model</code>
      <br/><em>      Opcional
    </em></td>
    <td style="text-align:center">Serie</td>
    <td style="text-align:left">
      Especifica el modelo de lenguaje que se debe utilizar para la transcripción.
      Si no especifica un modelo, el servicio utiliza el modelo <code>en-US_BroadbandModel</code> de forma predeterminada. Para obtener más información, consulte [Idiomas y modelos](/docs/services/speech-to-text?topic=speech-to-text-models).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>language_customization_id</code>
      <br/><em>      Opcional
    </em></td>
    <td style="text-align:center">Serie</td>
    <td style="text-align:left">
      Especifica el identificador global exclusivo (GUID) de un modelo de lenguaje personalizado que se va a utilizar para todas las solicitudes que se envían a través de la conexión. El modelo base del modelo de lenguaje personalizado debe coincidir con el valor del parámetro <code>model</code>. Si incluye un ID de personalización, debe realizar la solicitud con las credenciales de la instancia del servicio propietaria del modelo personalizado. De forma predeterminada, no se utiliza ningún modelo de lenguaje personalizado. Para obtener más información, consulte [La interfaz de personalización](/docs/services/speech-to-text?topic=speech-to-text-customization).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>acoustic_customization_id</code>
      <br/><em>      Opcional
    </em></td>
    <td style="text-align:center">Serie</td>
    <td style="text-align:left">
      Especifica el identificador global exclusivo (GUID) de un modelo acústico personalizado que se va a utilizar para todas las       solicitudes que se envían a través de la conexión. El modelo base del modelo acústico personalizado debe coincidir con el valor del parámetro <code>model</code>. Si incluye un ID de personalización, debe realizar la solicitud con las credenciales de la instancia del servicio propietaria del modelo personalizado. De forma predeterminada, no se utiliza ningún modelo acústico personalizado. Para obtener más información, consulte [La interfaz de personalización](/docs/services/speech-to-text?topic=speech-to-text-customization).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>base_model_version</code>
      <br/><em>      Opcional
    </em></td>
    <td style="text-align:center">Serie</td>
    <td style="text-align:left">
      Especifica la versión del `modelo` base que se va a utilizar para todas las solicitudes que se envíen a través de la conexión. El parámetro está pensado principalmente para que se utilice con modelos personalizados que se actualizan para un nuevo modelo base. El valor predeterminado depende de si el parámetro se utiliza con o sin un modelo personalizado. Para obtener más información, consulte [Versión del modelo base](/docs/services/speech-to-text?topic=speech-to-text-input#version).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-learning-opt-out</code>
      <br/><em>      Opcional
    </em></td>
    <td style="text-align:center">Booleano</td>
    <td style="text-align:left">
      Indica si el servicio registra las solicitudes y los resultados que se envían a través de la conexión. Para impedir que IBM acceda a sus datos para realizar mejoras generales del servicio, especifique <code>true</code> para el parámetro. Para obtener más información, consulte [Registro de solicitudes](/docs/services/speech-to-text?topic=speech-to-text-input#logging).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-metadata</code>
      <br/><em>      Opcional
    </em></td>
    <td style="text-align:center">Serie</td>
    <td style="text-align:left">
      Asocia un ID de cliente con todos los datos que se pasan a través de la conexión. El parámetro acepta el argumento <code>customer_id={id}</code>, donde <code>id</code> es la serie aleatoria o genérica que se va a asociar con los datos. Debe codificar en URL el argumento para el parámetro, por ejemplo `customer_id%3dmy_customer_ID`. De forma predeterminada, no se asocia ningún ID de cliente a los datos. Para obtener más información, consulte [Seguridad de la información](/docs/services/speech-to-text?topic=speech-to-text-information-security).
    </td>
  </tr>
</table>

El siguiente fragmento de código JavaScript abre una conexión con el servicio. La llamada al método `/v1/recognize` pasa los parámetros de consulta `access_token` y `model`, este último para indicar al servicio que utilice el modelo de banda ancha en español. Después de establecer la conexión, el cliente define las escuchas de sucesos (`onOpen`, `onClose`, etc.) para responder a los sucesos procedentes del servicio. El cliente puede utilizar la conexión para varias solicitudes de reconocimiento.

```javascript
var IAM_access_token = '{access_token}';
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token
  + '&model=es-ES_BroadbandModel';
var websocket = new WebSocket(wsURI);
websocket.onopen = function(evt) { onOpen(evt) };
websocket.onclose = function(evt) { onClose(evt) };
websocket.onmessage = function(evt) { onMessage(evt) };
websocket.onerror = function(evt) { onError(evt) };
```
{: codeblock}

El cliente puede abrir varias conexiones WebSocket simultáneas en el servicio. El número de conexiones simultáneas está limitado únicamente por la capacidad del servicio, que no suele plantear problemas a los usuarios.

## Iniciar una solicitud de reconocimiento
{: #WSstart}

Para iniciar una solicitud de reconocimiento, el cliente envía un mensaje de texto JSON al servicio a través de la conexión establecida. El cliente debe enviar este mensaje antes de enviar cualquier audio para la transcripción. El mensaje debe incluir el parámetro `action`, pero normalmente puede omitir el parámetro `content-type`.

<table>
  <caption>Tabla 2. Parámetros del mensaje de texto JSON</caption>
  <tr>
    <th style="width:23%; text-align:left">Parámetro</th>
    <th style="width:12%; text-align:center">Tipo de datos</th>
    <th style="text-align:left">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>action</code><br/><em>      Obligatorio
    </em></td>
    <td style="text-align:center">Serie</td>
    <td style="text-align:left">
      Especifica la acción que se va a llevar a cabo:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          <code>start</code> inicia una solicitud de reconocimiento o especifica nuevos parámetros para siguientes solicitudes. Para obtener más información, consulte [Enviar solicitudes adicionales y modificar parámetros de la solicitud](#WSmore).
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>stop</code> indica que se ha enviado todo el audio correspondiente a una solicitud. Para obtener más información, consulte [Finalizar una solicitud de reconocimiento](#WSstop).
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>content-type</code><br/><em>      Opcional
    </em></td>
    <td style="text-align:center">Serie</td>
    <td style="text-align:left">
      Identifica el formato (tipo de MIME) de los datos de audio de la solicitud.
      El parámetro es obligatorio para los formatos `audio/alaw`, `audio/basic`, `audio/l16` y `audio/mulaw`. Para obtener más información, consulte [Formatos de audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).
    </td>
  </tr>
</table>

El mensaje también puede incluir parámetros opcionales para especificar otros aspectos sobre cómo se va a procesar la solicitud y la información que se va a devolver. Para obtener información sobre todas las características de entrada y salida, consulte el [Resumen de parámetros](/docs/services/speech-to-text?topic=speech-to-text-summary). Puede especificar un modelo de lenguaje, un modelo de lenguaje personalizado y un modelo acústico personalizado solo como parámetros de consulta del URL de WebSocket.

El siguiente fragmento de código JavaScript envía parámetros de inicialización para la solicitud de reconocimiento a través de la conexión WebSocket. Las llamadas están incluidas en la función `onOpen` del cliente para garantizar que se envíen solo después de que se establezca la conexión.

```javascript
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050'
  };
  websocket.send(JSON.stringify(message));
}
```
{: codeblock}

Si recibe la solicitud correctamente, el servicio devuelve el mensaje de texto siguiente para indicar que está en estado `listening` (a la escucha):

```javascript
{'state': 'listening'}
```
{: codeblock}

El estado `listening` indica que la instancia de servicio está configurada (el mensaje JSON `start` era válido) y preparada para procesar una nueva expresión de una solicitud de reconocimiento. Una vez que empieza a escuchar, el servicio procesa cualquier audio que se haya enviado antes del mensaje `listening`.

Si el cliente especifica un parámetro de consulta o un campo JSON no válido para la solicitud de reconocimiento, la respuesta JSON del servicio incluye un campo `warnings` (de aviso). El campo describe cada argumento no válido. La solicitud tiene éxito a pesar de los avisos.

## Enviar audio y recibir los resultados del reconocimiento
{: #WSaudio}

Después de enviar el mensaje inicial de `start`, el cliente puede empezar a enviar datos de audio al servicio. No es necesario que el cliente espere a que el servicio responda al mensaje `start` con el mensaje `listening`. El servicio devuelve los resultados de la transcripción de forma asíncrona en el mismo formato en que devuelve los resultados para las interfaces HTTP.

El cliente debe enviar el audio como datos binarios. El cliente puede enviar un máximo de 100 MB de datos de audio con una sola expresión (por solicitud `send`). Debe enviar al menos 100 bytes de audio con cualquier solicitud. El cliente puede enviar varias expresiones a través de una sola conexión WebSocket. Para obtener información sobre cómo utilizar la compresión para maximizar la cantidad de audio que puede enviar al servicio con una solicitud, consulte [Formatos de audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).

La interfaz WebSocket impone un tamaño máximo de trama de 4 MB. El cliente puede establecer el tamaño máximo de trama en menos de 4 MB. Si no resulta práctico establecer el tamaño de trama, el cliente puede establecer el tamaño máximo de mensaje en menos de 4 MB y enviar los datos de audio como una secuencia de mensajes. Para obtener más información sobre los marcos de WebSocket, consulte [IETF RFC 6455](http://tools.ietf.org/html/rfc6455){: external}.

El siguiente fragmento de código JavaScript envía datos de audio al servicio como un mensaje binario (blob):

```javascript
websocket.send(blob);
```
{: codeblock}

El siguiente fragmento de código recibe las hipótesis de reconocimiento que el servicio devuelve de forma asíncrona. Los resultados se manejan mediante la función `onMessage` del cliente.

```javascript
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

## Finalizar una solicitud de reconocimiento
{: #WSstop}

Cuando termina de enviar los datos de audio para una solicitud al servicio, el cliente *debe* indicar el final de la transmisión de audio en binario al servicio de una de las formas siguientes:

-   Mediante el envío de un mensaje de texto JSON con el parámetro `action` establecido en `stop`:

    ```javascript
    {action: 'stop'}
    ```
    {: codeblock}

-   Mediante el envío de un mensaje binario vacío, uno en el que el blob especificado esté vacío:

    ```javascript
    websocket.send(blob)
    ```
    {: codeblock}

El servicio no envía los resultados finales hasta que recibe la confirmación de que la transmisión de audio está completa. Si no se indica que la transmisión se ha completado, la conexión puede agotar el tiempo de espera sin que el servicio envíe los resultados finales.

Para recibir los resultados finales entre varias solicitudes de reconocimiento, el cliente debe indicar el final de la transmisión de la solicitud anterior antes de enviar la siguiente solicitud. Después de devolver los resultados finales para la primera solicitud, el servicio devuelve otro mensaje `{"state":"listening"}` al cliente. Este mensaje indica que el servicio está listo para recibir otra solicitud.

## Enviar solicitudes adicionales y modificar los parámetros de la solicitud
{: #WSmore}

Mientras la conexión de WebSocket permanezca activa, el cliente puede seguir utilizando la conexión para enviar más solicitudes de reconocimiento con un nuevo audio. De forma predeterminada, el servicio sigue utilizando los parámetros que se enviaron con el mensaje `start` anterior para las siguientes solicitudes que se envíen a través de la misma conexión.

Para cambiar los parámetros para las siguientes solicitudes, el cliente puede enviar otro mensaje `start` con los nuevos parámetros después de recibir del servicio los resultados finales del reconocimiento y el mensaje `{"state":"listening"}`. El cliente puede cambiar cualquier parámetro excepto los que se especifican al abrir la conexión (`model`, `language_customization_id`, etc.).

El ejemplo siguiente envía un mensaje `start` con parámetros nuevos para las siguientes solicitudes de reconocimiento que se envían a través de la conexión. El mensaje especifica el mismo `content-type` que el ejemplo anterior, pero indica al servicio que devuelva las medidas de confianza y las indicaciones de fecha y hora para las palabras de la transcripción.

```javascript
var message = {
  action: 'start',
  content-type: 'audio/l16;rate=22050',
  word_confidence: true,
  timestamps: true
};
websocket.send(JSON.stringify(message));
```
{: codeblock}

## Mantener activa una conexión
{: #WSkeep}

El servicio finaliza la sesión y cierra la conexión si se llega al tiempo de espera excedido de inactividad o de sesión:

-   Se produce un *tiempo de espera excedido de inactividad* si el cliente está enviando audio pero el servicio no detecta voz. El tiempo de espera excedido predeterminado de inactividad es de 30 segundos. Puede utilizar el parámetro `inactivity_timeout` para especificar otro valor, incluido `-1` para establecer un tiempo de espera excedido infinito. Para obtener más información, consulte [Tiempo de espera excedido de inactividad](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-inactivity).
-   Se produce un *tiempo de espera excedido de sesión* si el servicio no recibe datos del cliente o si no envía ningún resultado provisional durante 30 segundos. No puede cambiar la duración de este tiempo de espera excedido, pero puede ampliar la sesión enviando al servicio datos de audio, silencio incluido, antes de que se alcance el tiempo de espera excedido. También debe establecer el parámetro `inactivity_timeout` en `-1`. Se le factura por la duración de los datos que envía al servicio, incluido el silencio que envíe para ampliar una sesión. Para obtener más información, consulte [Tiempo de espera excedido de sesión](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-session).

Los clientes y servidores de WebSocket también intercambian *marcos de tipo ping-pong* para evitar los tiempos de espera excedidos de lectura intercambiando periódicamente pequeñas cantidades de datos. Muchas pilas de WebSocket intercambian marcos de tipo ping-pong, pero algunas no lo hacen. Para determinas si su implementación utiliza los marcos de tipo ping-pong, compruebe las listas de funciones. No puede determinar o gestionar marcos de tipo ping-pong de forma programática.

Si su pila de WebSocket no implementa los marcos de tipo ping-pong y se están enviando archivos de audio largos, su conexión puede experimentar un tiempo de espera excedido de lectura. Para evitar estos tiempos de espera, la secuencia de audio continuada al servicio o solicitar resultados provisionales del servicio. Cualquier enfoque puede garantizar que la falta de marcos de tipo ping-pong no haga que se cierre la conexión.

Para obtener más información sobre los marcos de tipo ping-pong, consulte [Sección 5.5.2 Ping](http://tools.ietf.org/html/rfc6455#section-5.5.2){: external} y [Sección 5.5.3 Pong](http://tools.ietf.org/html/rfc6455#section-5.5.3){: external} de IETF RFC 6455.

## Cerrar una conexión
{: #WSclose}

Cuando el cliente termina de interactuar con el servicio, puede cerrar la conexión WebSocket. Una vez que se ha cerrado la conexión, el cliente ya no puede utilizarla para enviar solicitudes ni para recibir resultados. Cierre la conexión solo después de recibir todos los resultados de una solicitud. Pasado un tiempo, la conexión supera el tiempo de espera y se cierra si no la cierra de forma explícita.

El siguiente fragmento de código JavaScript cierra una conexión abierta:

```javascript
websocket.close();
```
{: codeblock}

## Códigos de retorno de WebSocket
{: #WSreturn}

El servicio puede enviar los siguientes códigos de retorno al cliente a través de la conexión WebSocket:

-   `1000` indica el cierre normal de la conexión, lo que significa que se ha cumplido el objetivo para el que se había establecido la conexión.
-   `1002` indica que el servicio está cerrando la conexión debido a un error de protocolo.
-   `1006` indica que la conexión se ha cerrado de forma anómala.
-   `1009` indica que el tamaño de trama ha superado el límite de 4 MB.
-   `1011` indica que el servicio está terminando la conexión porque ha encontrado una condición inesperada que le impide cumplir la solicitud.

Si el socket se cierra con un error, el cliente recibe un mensaje informativo con el formato `{"error":"{message}"}` antes de que se cierre el socket. Para obtener más información sobre los códigos de retorno de WebSocket, consulte [IETF RFC 6455](http://tools.ietf.org/html/rfc6455){: external}.

## Ejemplo de sesión de WebSocket
{: #WSexample}

En el ejemplo siguiente se muestra una sesión de WebSocket entre un cliente y el servicio {{site.data.keyword.speechtotextshort}}. La sesión se muestra como tres intercambios separados para facilitar el seguimiento. Pero los tres intercambios forman parte de una única sesión con el servicio. El ejemplo se centra en el intercambio de mensajes; no muestra la apertura ni el cierre de la conexión.

### Primer intercambio de ejemplo
{: #firstExample}

En el primer intercambio, el cliente envía audio que contiene la serie `Name the Mayflower`. El cliente envía un mensaje binario con una sola porción de datos de audio PCM (`audio/l16`), para los que indica la frecuencia de muestreo necesaria. El cliente no espera a recibir la respuesta `{"state":"listening"}` del servicio para empezar a enviar los datos de audio y para señalar el fin de la solicitud. El envío inmediato de los datos reduce la latencia porque el audio está disponible para el servicio en cuanto está preparado para gestionar una solicitud de reconocimiento.

-   El *cliente* envía:

    ```javascript
    {
      "action": "start",
      "content-type": "audio/l16;rate=22050"
    }
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   El *servicio* responde:

    ```javascript
    {"state": "listening"}
    {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### Segundo intercambio de ejemplo
{: #secondExample}

En el segundo intercambio, el cliente envía audio que contiene la serie `Second audio transcript`. El cliente envía el audio en un solo mensaje binario y utiliza los mismos parámetros que ha especificado en la primera solicitud.

-   El *cliente* envía:

    ```javascript
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   El *servicio* responde:

    ```javascript
    {"results": [{"alternatives": [{"transcript": "second audio transcript "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### Tercer intercambio de ejemplo
{: #thirdExample}

En el tercer intercambio, el cliente vuelve a enviar audio que contiene la serie `Name the Mayflower`. El cliente vuelve a enviar un mensaje binario con una sola porción de datos de audio PCM. Pero, esta vez, el cliente solicita resultados provisionales con la transcripción.

-   El *cliente* envía:

    ```javascript
    {
      "action": "start",
      "content-type": "audio/l16;rate=22050",
      "interim_results": true
    }
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   El *servicio* responde:

    ```javascript
    {"state":"listening"}
    {"results": [{"alternatives": [{"transcript": "name "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name may "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name may flour "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}
