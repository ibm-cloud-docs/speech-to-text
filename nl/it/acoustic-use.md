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

# Utilizzo di un modello acustico personalizzato
{: #acousticUse}

Una volta creato e addestrato il tuo modello acustico personalizzato, puoi utilizzarlo nelle richieste di riconoscimento vocale. Utilizza il parametro di query `acoustic_customization_id` per specificare il modello acustico personalizzato per una richiesta, come mostrato nei seguenti esempi. Devi immettere la richiesta con le credenziali del servizio per l'istanza del servizio proprietaria del modello.
{: shortdesc}

Puoi anche specificare un modello di lingua personalizzato da utilizzare con la richiesta, che può aumentare l'accuratezza della trascrizione. Per ulteriori informazioni, vedi [Utilizzo dei modelli di lingua e acustici personalizzati durante il riconoscimento vocale](/docs/services/speech-to-text/acoustic-both.html#useBothRecognize).

Puoi creare più modelli acustici personalizzati per domini o ambienti uguali o diversi. Tuttavia, puoi specificare un solo modello acustico personalizzato alla volta con il parametro `acoustic_customization_id`.

-   Per l'[interfaccia WebSocket](/docs/services/speech-to-text/websockets.html), utilizza il metodo `/v1/recognize`. Il modello personalizzato specificato viene utilizzato per tutte le richieste inviate tramite la connessione.

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=en-US_NarrowbandModel'
      + '&acoustic_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   Per l'[interfaccia HTTP sincrona](/docs/services/speech-to-text/http.html), utilizza il metodo `POST /v1/recognize`. Il modello personalizzato specificato viene utilizzato per tale richiesta.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}"
    ```
    {: pre}
-   Per l'[interfaccia HTTP asincrona](/docs/services/speech-to-text/async.html), utilizza il metodo `POST /v1/recognitions`. Il modello personalizzato specificato viene utilizzato per tale richiesta.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?acoustic_customization_id={customization_id}"
    ```
    {: pre}

Puoi omettere il modello di lingua dalla richiesta se il modello personalizzato è basato sul modello predefinito, `en-US_BroadbandModel`. Altrimenti, devi utilizzare il parametro `model` per specificare il modello di base, come mostrato per l'esempio WebSocket. Un modello personalizzato può essere utilizzato solo con il modello di base per il quale viene creato.
