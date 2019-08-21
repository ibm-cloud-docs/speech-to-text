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

# Utilizzo di un modello di lingua personalizzato
{: #languageUse}

Dopo che hai creato e addestrato il tuo modello di lingua personalizzato, puoi utilizzarlo nelle richieste di riconoscimento vocale. Utilizzi il parametro di query `language_customization_id` per specificare il modello di lingua personalizzato per una richiesta, come mostrato nei seguenti esempi. Puoi anche indicare al servizio quanto peso dare alle parole dal modello personalizzato. Per ulteriori informazioni, vedi [Utilizzo del peso di personalizzazione](#weight). Devi immettere la richiesta con le credenziali per l'istanza del servizio a cui appartiene il modello.
{: shortdesc}

Puoi creare più modelli di lingua personalizzati per lo stesso dominio o per domini differenti. Puoi tuttavia specificare solo un singolo modello di lingua personalizzato per volta con il parametro `language_customization_id`. Per degli esempi che utilizzano una grammatica con un modello di lingua personalizzato, vedi [Utilizzo di una grammatica per il riconoscimento vocale](/docs/services/speech-to-text?topic=speech-to-text-grammarUse).

-   Per l'[interfaccia WebSocket](/docs/services/speech-to-text?topic=speech-to-text-websockets), utilizza il metodo `/v1/recognize`. Il modello personalizzato specificato viene utilizzato per tutte le richieste inviate tramite la connessione.

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=es-ES_BroadbandModel'
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   Per l'[interfaccia HTTP sincrona](/docs/services/speech-to-text?topic=speech-to-text-http), utilizza il metodo `POST /v1/recognize`. Il modello personalizzato specificato viene utilizzato per tale richiesta.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}"
    ```
    {: pre}
-   Per l'[interfaccia HTTP asincrona](/docs/services/speech-to-text?topic=speech-to-text-async), utilizza il metodo `POST /v1/recognitions`. Il modello personalizzato specificato viene utilizzato per tale richiesta.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?language_customization_id={customization_id}"
    ```
    {: pre}

Puoi omettere il modello di lingua dalla richiesta se il modello personalizzato è basato sul modello di lingua predefinito, `en-US_BroadbandModel`. Altrimenti, devi utilizzare il parametro `model` per specificare il modello di base, come mostrato per l'esempio WebSocket. Un modello personalizzato può essere utilizzato solo con il modello di base per il quale viene creato.

## Utilizzo del peso di personalizzazione
{: #weight}

Un modello di lingua personalizzato è una combinazione del modello personalizzato e del modello di base che esso personalizza. Puoi indicare al servizio quanto peso dare alle parole dal modello di lingua personalizzato rispetto alle parole dal modello di base per il riconoscimento vocale. Il peso assegnato a un modello personalizzato viene indicato come suo *peso di personalizzazione*.

Specifichi il peso relativo per un modello di lingua personalizzato come un valore double compreso tra 0.0 e 1.0. Per impostazione predefinita, ogni modello di lingua personalizzato ha un peso di 0.3. Il peso predefinito produce le migliori prestazioni nel caso generale. Consente il riconoscimento sia delle parole OOV dal modello personalizzato che delle parole dal vocabolario di base.

Tuttavia, nei casi in cui l'audio da trascrivere faccia un uso frequente di parole OOV dal modello personalizzato, aumentare il peso di personalizzazione può migliorare l'accuratezza dei risultati della trascrizione. Presta attenzione quando imposti il peso di personalizzazione. Se da una parte un peso maggiore può migliorare l'accuratezza delle frasi dal dominio del modello personalizzato, dall'altra può avere delle ripercussioni negative sulle prestazioni delle frasi non del dominio. (Anche se imposti il peso su 0.0, esiste una piccola probabilità che la trascrizione possa includere delle parole personalizzate a causa dell'implementazione della personalizzazione del modello di lingua).

Specifichi un peso di personalizzazione utilizzando il parametro `customization_weight`. Puoi specificare il parametro quando addestri un modello di lingua personalizzato o quando lo utilizzi con una richiesta di riconoscimento vocale.

-   Per una richiesta di addestramento, il seguente esempio specifica un peso di personalizzazione di `0.5` con il metodo `POST /v1/customizations/{customization_id}/train`:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train?customization_weight=0.5"
    ```
    {: pre}

    L'impostazione di un peso di personalizzazione durante l'addestramento salva il peso con il modello di lingua personalizzato. Non devi necessariamente passare il peso con ciascuna richiesta di riconoscimento che utilizza il modello personalizzato.

-   Per una richiesta di riconoscimento, il seguente esempio specifica un peso di personalizzazione di `0.7` con il metodo `POST /v1/recognize`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.7"
    ```
    {: pre}

    L'impostazione di un peso di personalizzazione durante il riconoscimento vocale sovrascrive un peso che era stato salvato con il modello durante l'addestramento.

## Risoluzione dei problemi di utilizzo di modelli di lingua personalizzati
{: #languageTroubleshoot}

Se applichi un modello di lingua personalizzato per il riconoscimento vocale ma rilevi che il servizio non sembra stia utilizzando le parole contenute nel modello, controlla l'eventuale presenza dei seguenti possibili problemi:

-   Assicurati di passare correttamente l'ID di personalizzazione alla richiesta di riconoscimento come mostrato negli esempi precedenti.
-   Assicurati che lo stato del modello personalizzato sia `available`, a indicare che è completamente addestrato e pronto all'uso. Per ulteriori informazioni, vedi [Elenco dei modelli di lingua personalizzati](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).
-   Controlla le pronunce che erano state generate per le nuove parole per assicurarti che siano corrette. Per ulteriori informazioni, vedi [Convalida di una risorsa di parole](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel).
