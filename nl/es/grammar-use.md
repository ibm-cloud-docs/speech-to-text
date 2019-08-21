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

# Utilización de una gramática para el reconocimiento de voz
{: #grammarUse}

Una vez que haya creado y personalizado el modelo de con su gramática, se puede utilizar la gramática en las solicitudes de reconocimiento de voz con las interfaces WebSocket y HTTP del servicio.
{: shortdesc}

-   Utilice el parámetro `language_customization_id` para especificar el ID de personalización (GUID) del modelo de lenguaje personalizado para el que se ha definido la gramática. Debe enviar la solicitud con las credenciales para la instancia del servicio propietaria del modelo.
-   Utilice el parámetro `grammar_name` para especificar el nombre de la gramática. Solo puede especificar una gramática con una solicitud.

Cuando se utiliza una gramática, el servicio solo reconoce las palabras de la gramática especificada. El servicio no utiliza palabras personalizadas añadidas desde el corpus, añadidas o modificadas individualmente o reconocidas por otras gramáticas.

-   Para la [interfaz WebSocket](/docs/services/speech-to-text?topic=speech-to-text-websockets), especifique primero el ID de personalización con el parámetro `language_customization_id` del método `/v1/recognize`. Este método se utiliza para establecer una conexión WebSocket con el servicio.

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}

    Luego especifique el nombre de la gramática con el parámetro `grammar_name` en el mensaje `start` de JSON correspondiente a la conexión activa. Pasar este valor con el mensaje `start` le permite cambiar la gramática de forma dinámica para cada solicitud que envíe a través de la conexión.

    ```javascript
    function onOpen(evt) {
      var message = {
        action: 'start',
        content-type: 'audio/l16;rate=22050',
        grammar_name: '{grammar_name}'
      };
      websocket.send(JSON.stringify(message));
      websocket.send(blob);
    }
    ```
    {: codeblock}
-   Para la [interfaz HTTP síncrona](/docs/services/speech-to-text?topic=speech-to-text-http), pase ambos parámetros con el método `POST /v1/recognize`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
-   Para la [interfaz HTTP asíncrona](/docs/services/speech-to-text?topic=speech-to-text-async), pase ambos parámetros con el método `POST /v1/recognitions`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
