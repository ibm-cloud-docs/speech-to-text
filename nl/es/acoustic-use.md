---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-07"

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

# Utilización de un modelo acústico personalizado
{: #acousticUse}

Una vez que haya creado y entrenado su modelo acústico personalizado, puede utilizarlo en solicitudes de reconocimiento de voz. Utilice el parámetro de consulta `acoustic_customization_id` para especificar el modelo acústico personalizado para una solicitud, tal como se muestra en los ejemplos siguientes. Debe enviar la solicitud con las credenciales de servicio correspondientes a la instancia del servicio propietaria del modelo.
{: shortdesc}

También puede especificar un modelo de lenguaje personalizado para que se utilice con la solicitud, lo que puede mejorar la precisión de la transcripción. Para obtener más información, consulte [Utilización de modelos de lenguaje personalizado y acústico personalizado durante el reconocimiento de voz](/docs/services/speech-to-text/acoustic-both.html#useBothRecognize).

Puede crear varios modelos acústicos personalizados para los mismos dominios o entornos o para dominios o entornos diferentes. Sin embargo, solo puede especificar un modelo acústico personalizado a la vez con el parámetro `acoustic_customization_id`.

-   En el caso de la [interfaz WebSocket](/docs/services/speech-to-text/websockets.html), utilice el método `/v1/recognize`. El modelo personalizado especificado se utiliza para todas las solicitudes que se envían a través de la conexión.

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=en-US_NarrowbandModel'
      + '&acoustic_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   En el caso de la [interfaz HTTP síncrona](/docs/services/speech-to-text/http.html), utilice el método `POST /v1/recognize`. El modelo personalizado especificado se utiliza para dicha solicitud.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}"
    ```
    {: pre}
-   En el caso de la [interfaz HTTP asíncrona](/docs/services/speech-to-text/async.html), utilice el método `POST /v1/recognitions`. El modelo personalizado especificado se utiliza para dicha solicitud.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?acoustic_customization_id={customization_id}"
    ```
    {: pre}

Puede omitir el modelo de lenguaje de la solicitud si el modelo personalizado se basa en el modelo predeterminado, `en-US_BroadbandModel`. De lo contrario, debe utilizar el parámetro `model` para especificar el modelo base, tal como se muestra para el ejemplo de WebSocket. Un modelo personalizado solo se puede utilizar con el modelo base para el que se ha creado.
