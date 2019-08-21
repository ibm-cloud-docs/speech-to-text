---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-04"

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

# Cómo realizar una solicitud de reconocimiento
{: #basic-request}

Para solicitar un reconocimiento de voz con el servicio {{site.data.keyword.speechtotextfull}}, solo debe proporcionar el audio que se va a transcribir. El servicio ofrece las mismas funciones básicas de transcripción con cada una de sus interfaces: la interfaz WebSocket, la interfaz HTTP síncrona y la interfaz HTTP asíncrona.
{: shortdesc}

En las secciones siguientes se muestran las solicitudes básicas de transcripción, sin parámetros de entrada ni de salida opcionales, para cada una de las interfaces del servicio:

-   En los ejemplos se envía un breve archivo FLAC llamado <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a>.
-   En los ejemplos se utiliza el modelo de lenguaje predeterminado, `en-US_BroadbandModel`.

En la sección [Visión general de los resultados del reconocimiento](/docs/services/speech-to-text?topic=speech-to-text-basic-response) se describe la respuesta del servicio para estos ejemplos.

## Envío de audio con una solicitud
{: #basic-request-audio}

El audio que se pasa al servicio debe estar en uno de los formatos admitidos por el servicio. Para la mayoría de audio, el servicio puede detectar automáticamente el formato. Para algún audio, debe especificar el formato con el parámetro `Content-Type` o equivalente. Para obtener más información, consulte [Formatos de audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats). (Para que quede más claro, en los ejemplos siguientes se especifica el formato de audio con todas las solicitudes.)

Con las interfaces WebSocket y HTTP síncrona, puede pasar un máximo de 100 MB de datos de audio con una sola solicitud. Con la interfaz HTTP asíncrona, puede pasar un máximo de 1 GB de datos de audio. Debe enviar al menos 100 bytes de audio con cualquier solicitud.

Si va a reconocer una gran cantidad de audio, puede dividir manualmente el audio en porciones más pequeñas. Pero por lo general resulta más eficiente y conveniente convertir el audio a un formato comprimido con cierta pérdida de calidad (lossy). La compresión puede maximizar la cantidad de datos que puede enviar con una sola solicitud. Especialmente si el audio está en formato WAV o FLAC, convertirlo a un formato comprimido con pérdida puede suponer una diferencia significativa.

-   Para obtener más información sobre los formatos de audio que utilizan compresión, consulte [Formatos de audio soportados](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#formats).
-   Para obtener más información sobre los efectos de la compresión y sobre la conversión de su audio a un formato que la utiliza, consulte los apartados sobre [Límites de datos y compresión](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#limits) y [Conversión de audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#conversion).

## Utilización de la interfaz WebSocket
{: #basic-request-websocket}

[La interfaz WebSocket](/docs/services/speech-to-text?topic=speech-to-text-websockets) ofrece una implementación eficiente que proporciona una baja latencia y un alto rendimiento a través de una conexión dúplex. Todas las solicitudes y respuestas se envían a través de la misma conexión WebSocket. Debido a sus ventajas, WebSockets es el mecanismo preferido para el reconocimiento de voz. Para obtener más información, consulte [Ventajas de la interfaz WebSocket](/docs/services/speech-to-text?topic=speech-to-text-developerOverview#advantages).

Para utilizar la interfaz WebSocket, primero debe utilizar el método `/v1/recognize` para establecer una conexión con el servicio. Debe especificar los parámetros, como el modelo de lenguaje y cualquier modelo personalizado que se vaya a utilizar para solicitudes enviadas a través de la conexión. Luego debe registrar escuchas de sucesos para gestionar las respuestas procedentes del servicio. Para realizar una solicitud, envíe un mensaje de texto JSON que incluya el formato de audio y los parámetros adicionales. Pase el audio como un mensaje binario (blob) y luego envíe un mensaje de texto para indicar el final del audio.

En el ejemplo siguiente se proporciona código JavaScript que establece una conexión y envía los mensajes binario y de texto para una solicitud de reconocimiento. El ejemplo no incluye el código para instalar los manejadores de sucesos.

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token;
var websocket = new WebSocket(wsURI);

websocket.send(JSON.stringify({
  action: 'start',
  content-type: 'audio/flac'
}));
websocket.send(blob);
websocket.send(JSON.stringify({action: 'stop'}));
```
{: codeblock}

## Utilización de la interfaz HTTP síncrona
{: #basic-request-sync}

[La interfaz HTTP síncrona](/docs/services/speech-to-text?topic=speech-to-text-http) ofrece la forma más sencilla de realizar una solicitud de reconocimiento. Utilice el método `POST /v1/recognize` para realizar una solicitud al servicio. Pase el audio y todos los parámetros en una sola solicitud.

En el siguiente ejemplo de `curl` se muestra una solicitud básica de reconocimiento HTTP:

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## Utilización de la interfaz HTTP asíncrona
{: #basic-request-async}

[La interfaz HTTP asíncrona](/docs/services/speech-to-text?topic=speech-to-text-async) ofrece interfaz que no es de bloqueo para transcribir el audio. Puede utilizar la interfaz registrando o no antes un URL de devolución de llamada con el servicio. Con un URL de devolución de llamada, el servicio envía notificaciones de devolución de llamada con el estado del trabajo y los resultados del reconocimiento. La interfaz utiliza firmas HMAC-SHA1 basadas en un secreto especificado por el usuario para proporcionar autenticación e integridad de los datos para sus notificaciones. Sin el URL de devolución de llamada, debe sondear el servicio para ver el estado del trabajo y los resultados. Con cualquiera de los enfoques, debe utilizar el método `POST /v1/recognitions` para realizar una solicitud de reconocimiento.

En el siguiente ejemplo de `curl` se muestra una solicitud sencilla de reconocimiento HTTP asíncrona. La solicitud no incluye un URL de devolución de llamada, por lo que debe sondear el servicio para obtener el estado de trabajo y la transcripción resultante.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions"
```
{: pre}
