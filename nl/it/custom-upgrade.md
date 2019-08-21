---

copyright:
  years: 2017, 2019
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

# Upgrade dei modelli personalizzati
{: #customUpgrade}

Per migliorare la qualità del riconoscimento vocale, il servizio {{site.data.keyword.speechtotextfull}} aggiorna occasionalmente i modelli di base. Poiché i modelli di base per le diverse lingue sono indipendenti l'uno dall'altro, così come i modelli a banda larga e a banda stretta per una lingua, gli aggiornamenti ai singoli modelli di base non influiscono sugli altri modelli. Le [Note sulla release](/docs/services/speech-to-text?topic=speech-to-text-release-notes) documentano tutti gli aggiornamenti dei modelli di base.
{: shortdesc}

Quando viene rilasciata una nuova versione di un modello di base, devi eseguire l'upgrade di tutti i modelli di lingua e acustici personalizzati creati sul modello di base per usufruire degli aggiornamenti. I tuoi modelli personalizzati continuano a utilizzare la versione precedente del modello di base finché non completi l'upgrade. Come con tutte le operazioni di personalizzazione, per eseguire l'upgrade di un modello devi utilizzare le credenziali dell'istanza del servizio che possiede tale modello.

Esegui l'upgrade alla versione più recente di un modello di base aggiornato il prima possibile. Quanto prima esegui l'upgrade di un modello personalizzato, tanto prima puoi sperimentare il miglioramento delle prestazioni del nuovo modello. Inoltre, le versioni precedenti dei modelli di base possono essere rimosse in futuro. Per incoraggiare l'upgrade, il servizio restituisce un messaggio di avvertenza con i risultati delle richieste di riconoscimento che utilizzano modelli personalizzati basati su modelli di base precedenti.

## Come funziona l'upgrade
{: #upgradeOverview}

Quando viene rilasciato per la prima volta un nuovo modello di base, i modelli personalizzati esistenti si basano ancora sulla versione precedente del modello di base. Finché non si esegue l'upgrade di un modello personalizzato, tutte le operazioni su tale modello personalizzato, come l'aggiunta di dati o l'addestramento del modello, influiscono sulla versione esistente del modello. Allo stesso modo, tutte le richieste di riconoscimento che specificano il modello personalizzato utilizzano la versione esistente del modello.

L'upgrade produce due versioni di un modello personalizzato, una basata sulla versione precedente del modello di base e una basata sulla versione più recente. Una volta eseguito l'upgrade di un modello personalizzato, tutte le operazioni su quel modello personalizzato influiscono sulla nuova versione con upgrade del modello. Non è più possibile aggiungere dati o addestrare la versione precedente del modello. Inoltre, non puoi annullare un'operazione di upgrade.

Quando esegui l'upgrade di un modello personalizzato, non devi eseguire l'upgrade delle sue singole risorse. Per un modello di lingua personalizzato, il servizio esegue automaticamente l'upgrade di tutti i corpora, le grammatiche e le parole definiti per il modello. Allo stesso modo, quando esegui l'upgrade di un modello acustico personalizzato, il servizio esegue automaticamente l'upgrade delle sue risorse audio.

Per impostazione predefinita, il servizio utilizza la versione più recente del modello personalizzato per le richieste di riconoscimento. Tuttavia, puoi continuare a utilizzare la versione precedente di un modello personalizzato per il riconoscimento vocale. Per ulteriori informazioni, vedi [Effettuazione di richieste di riconoscimento con modelli personalizzati di cui è stato eseguito l'upgrade](#upgradeRecognition).

## Upgrade di un modello di lingua personalizzato
{: #upgradeLanguage}

Attieniti alla seguente procedura per eseguire l'upgrade di un modello di lingua personalizzato:

1.  Assicurati che il modello di lingua personalizzato sia nello stato `ready` o `available`. Puoi controllare lo stato di un modello utilizzando il metodo `GET /v1/customizations/{customization_id}`. Se lo stato del modello è `ready`, esegui l'upgrade del modello prima di addestrarlo sui suoi dati più recenti.

1.  Esegui l'upgrade del modello di lingua personalizzato utilizzando il metodo `POST /v1/customizations/{customization_id}/upgrade_model`:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    Il metodo di upgrade è asincrono. Il suo completamento può richiedere alcuni minuti a seconda del numero di parole contenute nella risorsa di parole del modello e del carico corrente sul servizio.

Se il processo di upgrade viene avviato correttamente, il servizio restituisce un codice di risposta 200. Puoi monitorare lo stato dell'upgrade utilizzando il metodo `GET /v1/customizations/{customization_id}` per eseguire il polling dello stato del modello. Utilizza un loop per controllare lo stato ogni 10 secondi.

Durante l'upgrade, il modello personalizzato ha lo stato `upgrading`. Al termine dell'upgrade, il modello riprende lo stato che aveva prima dell'upgrade (`ready` o `available`). Il controllo dello stato di un'operazione di upgrade è identico al controllo dello stato di un'operazione di addestramento. Per ulteriori informazioni, vedi [Monitoraggio della richiesta di addestramento del modello](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#monitorTraining-language).

Il servizio non può accettare in alcun modo richieste di modifica del modello finché la richiesta di upgrade non viene completata. Tuttavia, puoi continuare a emettere richieste di riconoscimento con la versione esistente del modello durante l'upgrade.

## Upgrade di un modello acustico personalizzato
{: #upgradeAcoustic}

Attieniti alla seguente procedura per eseguire l'upgrade di un modello acustico personalizzato. Se il modello acustico personalizzato è stato addestrato con un modello di lingua personalizzato, devi eseguire due passi di upgrade aggiuntivi dove indicato.

1.  *Se il modello acustico personalizzato è stato addestrato con un modello di lingua personalizzato,* devi prima eseguire l'upgrade del modello di lingua personalizzato alla versione più recente del modello di base. Per ulteriori informazioni, vedi [Upgrade di un modello di lingua personalizzato](#upgradeLanguage).

1.  Assicurati che il modello acustico personalizzato sia nello stato `ready` o `available`. Puoi controllare lo stato di un modello utilizzando il metodo `GET /v1/acoustic_customizations/{customization_id}`. Se lo stato del modello è `ready`, esegui l'upgrade del modello prima di addestrarlo sui suoi dati più recenti.

1.  Esegui l'upgrade del modello acustico personalizzato utilizzando il metodo `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    Il metodo di upgrade è asincrono. Il suo completamento può richiedere pochi minuti o alcune ore, a seconda della quantità di dati audio contenuti nel modello e del carico corrente sul servizio. Come per l'addestramento, l'upgrade richiede in genere circa il doppio della lunghezza dei dati audio del modello.

1.  *Se il modello acustico personalizzato è stato addestrato con un modello di lingua personalizzato,* esegui di nuovo l'upgrade del modello acustico personalizzato, questa volta con il modello di lingua personalizzato precedentemente sottoposto ad upgrade. Utilizza il parametro di query `custom_language_model_id` per specificare l'ID di personalizzazione del modello di lingua personalizzato.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model?custom_language_model_id={custom_language_model_id}"
    ```
    {: pre}

    Ancora una volta, il metodo di upgrade è asincrono e l'upgrade richiede in genere circa il doppio della lunghezza dei dati audio del modello.

    La richiesta di upgrade del modello acustico con il modello di lingua potrebbe non riuscire con un codice di risposta 400 e il messaggio `No input data modified since last training`. Se si verifica questo errore, aggiungi il parametro di query booleano `force` alla richiesta e imposta il parametro su `true`. Utilizza il parametro per forzare un upgrade di un modello acustico personalizzato solo in questa particolare situazione.
    {: note}

Se il processo di upgrade viene avviato correttamente, il servizio restituisce un codice di risposta 200. Puoi monitorare lo stato dell'upgrade utilizzando il metodo `GET /v1/acoustic_customizations/{customization_id}` per eseguire il polling dello stato del modello. Utilizza un loop per controllare lo stato una volta al minuto.

Durante l'upgrade, il modello personalizzato ha lo stato `upgrading`. Al termine dell'upgrade, il modello riprende lo stato che aveva prima dell'upgrade (`ready` o `available`). Il controllo dello stato di un'operazione di upgrade è identico al controllo dello stato di un'operazione di addestramento. Per ulteriori informazioni, vedi [Monitoraggio della richiesta di addestramento del modello](/docs/services/speech-to-text?topic=speech-to-text-acoustic#monitorTraining-acoustic).

Il servizio non può accettare in alcun modo richieste di modifica del modello finché la richiesta di upgrade non viene completata. Tuttavia, puoi continuare a emettere richieste di riconoscimento con la versione esistente del modello durante l'upgrade.

## Errori di upgrade
{: #upgradeFailures}

L'upgrade di un modello personalizzato non viene avviato se il servizio sta gestendo un'altra richiesta per il modello, ad esempio una richiesta di addestramento o una richiesta per aggiungere dati. La richiesta di upgrade non viene avviata anche nei seguenti casi:

-   Il modello personalizzato è in uno stato diverso da `ready` o `available`.
-   Il modello personalizzato non contiene dati (parole personalizzate o risorse audio).
-   Per un modello acustico personalizzato che è stato addestrato con un modello di lingua personalizzato, i modelli personalizzati si basano su versioni diverse del modello di base. Devi eseguire l'upgrade del modello di lingua personalizzato prima di eseguire l'upgrade del modello acustico personalizzato.

## Elenco delle informazioni sulla versione per un modello personalizzato
{: #upgradeList}

Per visualizzare le versioni del modello di base per cui è disponibile un modello personalizzato, utilizza i seguenti metodi:

-   Per elencare le informazioni su un modello di lingua personalizzato, utilizza il metodo `GET /v1/customizations/{customization_id}`. Per ulteriori informazioni, vedi [Elenco dei modelli di lingua personalizzati](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).
-   Per elencare le informazioni su un modello acustico personalizzato, utilizza il metodo `GET /v1/acoustic_customizations/{customization_id}`. Per ulteriori informazioni, vedi [Elenco dei modelli acustici personalizzati](/docs/services/speech-to-text?topic=speech-to-text-manageAcousticModels#listModels-acoustic).

In entrambi i casi, l'output include un campo `versions` che mostra le informazioni relative ai modelli di base per il modello personalizzato. Il seguente output mostra le informazioni per un modello di lingua personalizzato di cui è stato eseguito l'upgrade:

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "versions": [
    "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

Il campo `versions` indica che il modello personalizzato è disponibile per due versioni del modello di base: la versione precedente, `en-US_BroadbandModel.v07-06082016.06202016` e la versione più recente, `en-US_BroadbandModel.v2017-11-15`. Se il modello personalizzato non viene sottoposto ad upgrade o esiste solo una singola versione del suo modello di base, il campo `versions` mostra una sola versione.

## Effettuazione di richieste di riconoscimento con modelli personalizzati di cui è stato eseguito l'upgrade
{: #upgradeRecognition}

Per impostazione predefinita, il servizio utilizza la versione più recente di un modello personalizzato specificato con una richiesta di riconoscimento. Tuttavia, anche dopo l'upgrade di un modello personalizzato, puoi continuare a effettuare richieste di riconoscimento con la versione precedente del modello. Utilizza il parametro `base_model_version` di un metodo di riconoscimento per specificare la versione di un modello di base da utilizzare per il riconoscimento vocale.

Ad esempio, la seguente richiesta HTTP specifica che deve essere utilizzata la versione precedente del modello base. Pertanto, viene utilizzata anche la versione precedente del modello di lingua personalizzato specificato.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_BroadbandModel&base_model_version=en-US_BroadbandModel.v07-06082016.06202016&language_customization_id={customization_id}"
```
{: pre}

Puoi utilizzare questa funzione per testare le prestazioni e l'accuratezza di un modello personalizzato rispetto alla vecchia e nuova versione del suo modello di base. Se ritieni che le prestazioni di un modello di cui è stato eseguito l'upgrade non siano del tutto ottimali (ad esempio, non vengono più riconosciute alcune parole), puoi continuare a utilizzare la versione precedente con le richieste di riconoscimento.

La sezione [Versione del modello di base](/docs/services/speech-to-text?topic=speech-to-text-input#version) descrive il parametro `base_model_version` e come il servizio determina quali versioni del modello di base e personalizzato utilizzare con una richiesta di riconoscimento. Oltre a tali informazioni, considera i seguenti problemi quando passi sia i modelli di lingua che acustici personalizzati con una richiesta di riconoscimento:

-   Entrambi i modelli personalizzati devono essere basati sullo stesso modello di base (ad esempio, `en-US_BroadbandModel`).
-   Se entrambi i modelli personalizzati sono basati sul modello di base precedente, il servizio utilizza il vecchio modello di base per il riconoscimento.
-   Se entrambi i modelli personalizzati sono basati sul modello di base più recente, il servizio utilizza il nuovo modello di base per il riconoscimento.
-   Se solo uno dei due modelli personalizzati viene sottoposto ad upgrade al modello di base più recente, il servizio utilizza il vecchio modello di base per il riconoscimento. Seleziona il vecchio modello di base perché quella è la versione che i due modelli personalizzati hanno in comune.
