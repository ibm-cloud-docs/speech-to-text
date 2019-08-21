---

copyright:
  years: 2017, 2019
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

# Creazione di un modello acustico personalizzato
{: #acoustic}

Per creare un modello acustico personalizzato per il servizio {{site.data.keyword.speechtotextshort}}, attieniti alla seguente procedura:
{: shortdesc}

1.  [Crea un modello acustico personalizzato](#createModel-acoustic). Puoi creare più modelli personalizzati per domini o ambienti uguali o diversi. Il processo è lo stesso per qualsiasi modello che crei. Tuttavia, puoi specificare solo un singolo modello acustico personalizzato alla volta con una richiesta di riconoscimento.
1.  [Aggiungi l'audio al modello acustico personalizzato](#addAudio). Per la modellazione acustica, il servizio accetta gli stessi formati di file audio di quelli che accetta per il riconoscimento vocale. Accetta anche i file di archivio che contengono più file audio. I file di archivio sono il modo preferito per aggiungere le risorse audio. Puoi ripetere il metodo per aggiungere più file audio o di archivio a un modello personalizzato.
1.  [Addestra il modello acustico personalizzato](#trainModel-acoustic). Dopo aver aggiunto risorse audio al modello personalizzato, devi addestrare il modello. L'addestramento prepara il modello acustico personalizzato per l'utilizzo nel riconoscimento vocale. L'addestramento può richiedere una notevole quantità di tempo. La durata dell'addestramento dipende dalla quantità di dati audio contenuti nel modello.

    Puoi specificare un modello di lingua personalizzato di supporto durante l'addestramento del modello acustico personalizzato. Un modello di lingua personalizzato che include le trascrizioni dei tuoi file audio o le parole OOV dal dominio dei tuoi file audio può migliorare la qualità del modello acustico personalizzato. Per ulteriori informazioni, vedi [Addestramento di un modello acustico personalizzato con un modello di lingua personalizzato](/docs/services/speech-to-text?topic=speech-to-text-useBoth#useBothTrain).
1.  Dopo aver addestrato il tuo modello personalizzato, puoi utilizzarlo con le richieste di riconoscimento. Se l'audio passato per la trascrizione ha qualità acustiche simili all'audio del modello personalizzato, i risultati riflettono una migliore comprensione del servizio. Per ulteriori informazioni, vedi [Utilizzo di un modello acustico personalizzato](/docs/services/speech-to-text?topic=speech-to-text-acousticUse).

    Puoi passare sia un modello acustico personalizzato che un modello di lingua personalizzato nella stessa richiesta di riconoscimento per migliorare ulteriormente l'accuratezza del riconoscimento. Per ulteriori informazioni, vedi [Utilizzo dei modelli di lingua e acustici personalizzati durante il riconoscimento vocale](/docs/services/speech-to-text?topic=speech-to-text-useBoth#useBothRecognize).

I passi per la creazione di un modello acustico personalizzato sono iterativi. Puoi aggiungere o eliminare l'audio e addestrare o riaddestrare un modello tutte le volte che lo ritieni necessario. Devi ripetere l'addestramento di un modello per rendere effettive le modifiche apportate al suo audio. Quando ripeti l'addestramento di un modello, tutti i dati audio vengono utilizzati nell'addestramento (non solo i nuovi dati). Quindi il tempo di addestramento è commisurato alla quantità totale di audio contenuta nel modello.

La personalizzazione del modello acustico è disponibile come funzionalità beta per tutte le lingue. Per ulteriori informazioni, vedi [Supporto delle lingue per la personalizzazione](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport).
{: note}

## Crea un modello acustico personalizzato
{: #createModel-acoustic}

Utilizza il metodo `POST /v1/acoustic_customizations` per creare un nuovo modello acustico personalizzato. Puoi creare un numero qualsiasi di modelli acustici personalizzati, ma puoi utilizzare solo un modello acustico personalizzato alla volta con una richiesta di riconoscimento vocale. Il metodo accetta un oggetto JSON che definisce gli attributi del nuovo modello personalizzato come corpo della richiesta.

<table>
  <caption>Tabella 1. Attributi di un nuovo modello acustico personalizzato</caption>
  <tr>
    <th style="width:20%; text-align:left">Parametro</th>
    <th style="width:12%; text-align:center">Tipo di dati</th>
    <th style="text-align:left">Descrizione</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>      Obbligatorio
    </em></td>
    <td style="text-align:center">Stringa</td>
    <td>
      Un nome definito dall'utente per il nuovo modello acustico personalizzato. Utilizza un nome
      che descriva l'ambiente acustico del modello personalizzato, ad
      esempio <code>Mobile custom model</code> o <code>Noisy car custom
      model</code>. Utilizza un nome che sia univoco tra tutti i modelli acustici
      personalizzati di tua proprietà. Utilizza un nome localizzato che corrisponda alla lingua
      del modello personalizzato.
    </td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>      Obbligatorio
    </em></td>
    <td style="text-align:center">Stringa</td>
    <td>
      Il nome del modello di base che deve essere personalizzato dal nuovo
      modello. Devi utilizzare il nome di un modello restituito dal
      metodo <code>GET /v1/models</code>. Il nuovo modello personalizzato può
      essere utilizzato solo con il modello di base che personalizza.
    </td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>      Facoltativo
    </em></td>
    <td style="text-align:center">Stringa</td>
    <td>
      Una descrizione del nuovo modello. Utilizza una descrizione localizzata che
      corrisponda alla lingua del modello personalizzato.
    </td>
  </tr>
</table>

Il seguente esempio crea un nuovo modello acustico personalizzato denominato `Example acoustic model`. Il modello viene creato per il modello di base `en-US_BroadbandModel` e ha la descrizione `Example custom acoustic model`. L'intestazione `Content-Type` specifica che al metodo si stanno passando dei dati JSON.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Example acoustic model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom acoustic model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations"
```
{: pre}

L'esempio restituisce l'ID di personalizzazione del nuovo modello. Ogni modello personalizzato è identificato da un ID di personalizzazione univoco, che è un GUID (Globally Unique Identifier). Specifichi il GUID di un modello personalizzato con il parametro `customization_id` delle chiamate associate al modello.

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

Il nuovo modello personalizzato appartiene all'istanza del servizio di cui vengono utilizzate le credenziali per crearlo. Per ulteriori informazioni, vedi [Proprietà di modelli personalizzati](/docs/services/speech-to-text?topic=speech-to-text-customization#customOwner).

## Aggiungi l'audio al modello acustico personalizzato
{: #addAudio}

Una volta creato il tuo modello acustico personalizzato, il passo successivo è aggiungere le risorse audio. Utilizza il metodo `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` per aggiungere una risorsa audio a un modello personalizzato. Puoi aggiungere

-   Un singolo file audio in qualsiasi formato supportato per il riconoscimento vocale. Per ulteriori informazioni, vedi [Formati audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).
-   Un file di archivio (file **.zip** o **.tar.gz**) che include più file audio. La raccolta di più file audio in un singolo file di archivio e il caricamento di quel singolo file è significativamente più efficiente rispetto all'aggiunta dei singoli file audio.

Passa la risorsa audio come corpo della richiesta e assegna alla risorsa un `audio_name`. Per ulteriori informazioni, vedi [Utilizzo delle risorse audio](/docs/services/speech-to-text?topic=speech-to-text-audioResources).

I seguenti esempi mostrano l'aggiunta di risorse di tipo audio e di tipo archivio:

-   Questo esempio aggiunge una risorsa di tipo audio al modello acustico personalizzato con il valore `customization_id` specificato. L'intestazione `Content-Type` identifica il tipo di audio come `audio/wav`. Il file audio, **audio1.wav**, viene passato come corpo della richiesta e alla risorsa viene fornito il nome `audio1`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/wav"
    --data-binary @audio1.wav
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
    ```
    {: pre}

-   Questo esempio aggiunge una risorsa di tipo archivio al modello acustico personalizzato specificato. L'intestazione `Content-Type` identifica il tipo di archivio come `application/zip`. L'intestazione `Contained-Contented-Type` indica che tutti i file contenuti nell'archivio hanno il formato `audio/l16` e sono campionati a una frequenza di 16 kHz. Il file di archivio, **audio2.zip**, viene passato come corpo della richiesta e alla risorsa viene fornito il nome `audio2`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/zip"
    --header "Contained-Content-Type: audio/l16;rate=16000"
    --data-binary @audio2.zip
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
    ```
    {: pre}

Il metodo accetta anche un parametro di query facoltativo `allow_overwrite` per sovrascrivere una risorsa audio esistente per un modello personalizzato. Utilizza il parametro se devi aggiornare una risorsa audio dopo averla aggiunta a un modello.

Il metodo è asincrono. Il suo completamento può richiedere diversi secondi in base alla durata dell'audio. Per un file di archivio, la durata dell'operazione dipende dalla durata dei file audio. Per ulteriori informazioni sul controllo dello stato di una richiesta di aggiunta di una risorsa audio, vedi [Monitoraggio della richiesta di aggiunta audio](#monitorAudio).

Puoi aggiungere qualsiasi numero di risorse audio a un modello personalizzato chiamando il metodo una volta per ciascun file audio o di archivio. Puoi effettuare più richieste per aggiungere contemporaneamente diverse risorse audio.

Devi aggiungere un minimo di 10 minuti e un massimo di 200 ore di audio che include il discorso, non il silenzio, a un modello personalizzato prima di poterlo addestrare. Nessuna risorsa di tipo audio o di tipo archivio può essere maggiore di 100 MB. Per ulteriori informazioni, vedi [Linee guida per l'aggiunta di risorse audio](/docs/services/speech-to-text?topic=speech-to-text-audioResources#audioGuidelines).

### Monitoraggio della richiesta di aggiunta audio
{: #monitorAudio}

Se l'audio è valido, il servizio restituisce un codice di risposta 201. Quindi, analizza in modo asincrono il contenuto del file o dei file audio ed estrae automaticamente informazioni sull'audio quali lunghezza, frequenza di campionamento e codifica. Non puoi addestrare il modello personalizzato finché il servizio non completa l'analisi di tutte le risorse audio per le richieste correnti.

Per determinare lo stato della richiesta, utilizza il metodo `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` per eseguire il polling dello stato
dell'audio. Il metodo accetta il GUID del modello personalizzato e il nome della risorsa audio. La sua risposta include lo `status` della risorsa, che ha uno dei seguenti valori:

-   `ok` indica che l'audio è accettabile e l'analisi è stata completata.
-   `being_processed` indica che il servizio sta ancora analizzando l'audio.
-   `invalid` indica che il file audio non è accettabile per l'elaborazione. Potrebbe avere il formato o la frequenza di campionamento errati o non essere un file audio. Per un file di archivio, se uno qualsiasi dei file audio che contiene non è valido, l'intero archivio non sarà valido.

Il contenuto della risposta e la posizione del campo `status` dipendono dal tipo di risorsa, ossia audio o archivio.

-   *Per una risorsa di tipo audio,* il campo `status` si trova nell'oggetto di primo livello (`AudioListing`).

    ```bash
    curl -X GET -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
    ```
    {: pre}

    ```javascript
    {
      "duration": 131,
      "name": "audio1",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 22050
      }
      "status": "ok"
    }
    ```
    {: codeblock}

-   *Per una risorsa di tipo archivio,* il campo `status` si trova nell'oggetto di secondo livello (`AudioResource`) che è nidificato nel campo `container`.

    ```bash
    curl -X GET -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
    ```
    {: pre}

    ```javascript
    {
      "container": {
        "duration": 556,
        "name": "audio2",
        "details": {
          "type": "archive",
          "compression": "zip"
        },
        "status": "ok"
      },
      . . .
    }
    ```
    {: codeblock}

Utilizza un loop per controllare lo stato della risorsa audio ogni pochi secondi finché non diventa `ok`. Per ulteriori informazioni sugli altri campi che vengono restituiti dal metodo, vedi [Elenco delle risorse audio per un modello acustico personalizzato](/docs/services/speech-to-text?topic=speech-to-text-manageAudio#listAudio).

## Addestra il modello acustico personalizzato
{: #trainModel-acoustic}

Una volta popolato un modello acustico personalizzato con le risorse audio, devi addestrare il modello sui nuovi dati. L'addestramento prepara un modello personalizzato per l'utilizzo nel riconoscimento vocale. Un modello non può essere utilizzato per le richieste di riconoscimento finché non lo addestri sui nuovi dati. Inoltre, gli aggiornamenti a un modello personalizzato sotto forma di risorse audio nuove o modificate non vengono applicati dal modello finché non lo addestri con le modifiche.

Utilizzi il metodo `POST /v1/acoustic_customizations/{customization_id}/train` per addestrare un modello personalizzato. Passi al metodo l'ID di personalizzazione del modello che vuoi addestrare, come nel seguente esempio.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train"
```
{: pre}

Il metodo è asincrono. Il completamento dell'addestramento può richiedere pochi minuti o alcune ore, a seconda della quantità di dati audio contenuti nel modello acustico personalizzato e del carico corrente sul servizio. Una linea guida generale è che l'addestramento di un modello acustico personalizzato richiede da due a quattro volte la lunghezza dei suoi dati audio. L'intervallo di tempo dipende dal modello che viene addestrato e dalla natura dell'audio, ad esempio se l'audio è pulito o rumoroso. Ad esempio, possono essere necessarie da 4 a 8 ore per addestrare un modello che contiene 2 ore di audio. Per ulteriori informazioni sul controllo dello stato di un'operazione di addestramento, vedi [Monitoraggio della richiesta di addestramento del modello](#monitorTraining-acoustic).

Il metodo include i seguenti parametri di query facoltativi:

-   Il parametro `custom_language_model_id` specifica un modello di lingua personalizzato creato separatamente che deve essere utilizzato durante l'addestramento. Puoi eseguire l'addestramento con un modello di lingua personalizzato che contiene le trascrizioni dei tuoi file audio o che contiene corpora o parole OOV che sono rilevanti per il contenuto dei file audio. Per poter eseguire l'addestramento, i modelli acustici e di lingua personalizzati devono essere basati sulla stessa versione dello stesso modello di base. Per ulteriori informazioni, vedi [Addestramento di un modello acustico personalizzato con un modello di lingua personalizzato](/docs/services/speech-to-text?topic=speech-to-text-useBoth#useBothTrain).
-   Il parametro `strict` indica se l'addestramento deve continuare se il modello personalizzato contiene una combinazione di risorse audio valide e non valide. Per impostazione predefinita, l'addestramento non riesce se il modello contiene una o più risorse non valide. Imposta il parametro su `false` per consentire all'addestramento di continuare finché il modello contiene almeno una risorsa valida. Il servizio esclude le risorse non valide dall'addestramento. Per ulteriori informazioni, vedi [Errori di addestramento](#failedTraining-acoustic).

### Monitoraggio della richiesta di addestramento del modello
{: #monitorTraining-acoustic}

Se il processo di addestramento viene avviato correttamente, il servizio restituisce un codice di risposta 200. Il servizio non può accettare richieste di addestramento successive o richieste di aggiunta di ulteriori risorse audio finché non viene completata la richiesta di addestramento esistente.

Per determinare lo stato di una richiesta di addestramento, utilizza il metodo `GET /v1/acoustic_customizations/{customization_id}` per eseguire il polling dello stato del modello. Il metodo accetta l'ID di personalizzazione del modello acustico e restituisce il suo stato, come nel seguente esempio:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T22:11:13.298Z",
  "language": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom acoustic model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

La risposta include i campi `status` e `progress` che riportano lo stato corrente del modello. Il significato del campo `progress` dipende dallo stato del modello. Il campo `status` può avere uno dei seguenti valori:

-   `pending` indica che il modello è stato creato ma è in attesa che vengano aggiunti dati di addestramento validi o che il servizio finisca di analizzare i dati che erano stati aggiunti. Il campo `progress` è `0`.
-   `ready` indica che il modello contiene dati validi ed è pronto per essere addestrato. Il campo `progress` è `0`.

    Se il modello contiene una combinazione di risorse audio valide e non valide, l'addestramento del modello non riesce a meno che non imposti il parametro di query `strict` su `false`. Per ulteriori informazioni, vedi [Errori di addestramento](#failedTraining-acoustic).
-   `training` indica che il modello è in fase di addestramento. Il campo `progress` cambia da `0` a `100` al termine dell'addestramento. <!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available` indica che il modello è addestrato e pronto per l'uso. Il campo `progress` è `100`.
-   `upgrading` indica che il modello è in fase di upgrade. Il campo `progress` è `0`.
-   `failed` indica che l'addestramento del modello non è riuscito. Il campo `progress` è `0`. Per ulteriori informazioni, vedi [Errori di addestramento](#failedTraining-acoustic).

Utilizza un loop per controllare lo stato dell'addestramento una volta al minuto finché il modello non diventa `available`. Per ulteriori informazioni sugli altri campi che vengono restituiti dal metodo, vedi [Elenco dei modelli acustici personalizzati](/docs/services/speech-to-text?topic=speech-to-text-manageAcousticModels#listModels-acoustic).

### Errori di addestramento
{: #failedTraining-acoustic}

L'addestramento non viene avviato se il servizio sta gestendo un'altra richiesta per il modello acustico personalizzato. Una richiesta in conflitto potrebbe essere un'altra richiesta di addestramento o una richiesta per aggiungere risorse audio al modello. Il servizio restituisce il codice di stato 409.

L'addestramento non viene avviato anche per i seguenti motivi:

-   Il modello personalizzato contiene meno di 10 minuti di dati audio.
-   Il modello personalizzato contiene più di 200 ore di dati audio.
-   Una o più risorse audio del modello personalizzato non sono valide.
-   Hai passato un modello di lingua personalizzato incompatibile con il parametro di query `custom_language_model_id`. Entrambi i modelli personalizzati devono essere basati sulla stessa versione dello stesso modello di base.

Il servizio restituisce il codice di stato 400 e imposta lo stato del modello personalizzato su `failed`. Effettua una delle seguenti azioni:

-   Utilizza i metodi `GET /v1/acoustic_customizations/{customization_id}/audio` e `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` per esaminare le risorse audio del modello. Per ulteriori informazioni, vedi [Elenco delle risorse audio per un modello acustico personalizzato](/docs/services/speech-to-text?topic=speech-to-text-manageAudio#listAudio).

    Per ogni risorsa audio non valida, effettua una delle seguenti operazioni:
    -   Correggi la risorsa audio e utilizza il parametro `allow_overwrite` del metodo `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` per aggiungere l'audio corretto al modello. Per ulteriori informazioni, vedi [Aggiungi l'audio al modello acustico personalizzato](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio).
    -   Utilizza il metodo `DELETE /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` per eliminare la risorsa audio dal modello. Per ulteriori informazioni, vedi [Eliminazione di una risorsa audio da un modello acustico personalizzato](/docs/services/speech-to-text?topic=speech-to-text-manageAudio#deleteAudio).
-   Imposta il parametro `strict` del modello `POST /v1/acoustic_customizations/{customization_id}/train` su `false` per escludere le risorse audio non valide dall'addestramento. Per poter eseguire l'addestramento, è necessario che il modello contenga almeno una risorsa audio valida. Il parametro `strict` è utile per addestrare un modello personalizzato che contiene una combinazione di risorse audio valide e non valide.
