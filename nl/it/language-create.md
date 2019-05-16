---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-11"

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

# Creazione di un modello di lingua personalizzato
{: #languageCreate}

Attieniti alla seguente procedura per creare un modello di lingua personalizzato per il servizio {{site.data.keyword.speechtotextshort}}:
{: shortdesc}

1.  [Crea un modello di lingua personalizzato](#createModel-language). Puoi creare più modelli personalizzati per lo stesso dominio o per domini differenti. Il processo è lo stesso per qualsiasi modello che crei. Puoi tuttavia specificare solo un singolo modello personalizzato per volta con una richiesta di riconoscimento.
1.  [Aggiungi un corpus al modello di lingua personalizzato](#addCorpus). Un corpus è un documento di testo semplice che utilizza la terminologia dal dominio in contesto. Il servizio crea un vocabolario per un modello personalizzato estraendo i termini dai corpora che non esistono nel suo vocabolario di base. Puoi aggiungere più corpora a un modello personalizzato.
1.  [Aggiungi parole a un modello di lingua personalizzato](#addWords). Puoi anche aggiungere parole personalizzate a un modello singolarmente. Puoi inoltre utilizzare gli stessi metodi per modificare le parole personalizzate estratte dai corpora. I metodi ti consentono di specificare la pronuncia delle parole e il modo in cui le parole vengono visualizzate in una trascrizione vocale.
1.  [Addestra il modello di lingua personalizzato](#trainModel-language). Una volta che hai aggiunto le parole al modello personalizzato, devi addestrare il modello sulle parole. L'addestramento prepara il modello personalizzato per l'utilizzo nel riconoscimento vocale. Il modello non utilizza parole nuove o modificate fino a che non lo addestri.
1.  Dopo aver addestrato il tuo modello personalizzato, puoi utilizzarlo con le richieste di riconoscimento. Se l'audio passato per le trascrizioni contiene delle parole specifiche per il dominio che sono definite nel modello personalizzato, i risultati della richiesta riflettono il vocabolario migliorato del modello. Per ulteriori informazioni, vedi [Utilizzo di un modello di lingua personalizzato](/docs/services/speech-to-text/language-use.html).

La procedura per creare un modello di lingua personalizzato è iterativa. Puoi aggiungere corpora e parole e addestrare o riaddestrare un modello tutte le volte che lo ritieni necessario.

Puoi anche aggiungere grammatiche a un modello di lingua personalizzato. Le grammatiche limitano la risposta del servizio alle sole parole da esse riconosciute. Per ulteriori informazioni, vedi [Utilizzo di grammatiche con i modelli di lingua personalizzati](/docs/services/speech-to-text/grammar.html).

La personalizzazione del modello di lingua è disponibile per la maggior parte delle lingue. Per ulteriori informazioni, vedi [Supporto delle lingue per la personalizzazione](/docs/services/speech-to-text/custom.html#languageSupport).
{: note}

## Crea un modello di lingua personalizzato
{: #createModel-language}

Utilizzi il metodo `POST /v1/customizations` per creare un nuovo modello di lingua personalizzato. Puoi creare qualsiasi numero di modelli di lingua personalizzati ma puoi utilizzare solo un singolo modello per volta con una richiesta di riconoscimento vocale. Il metodo accetta un oggetto JSON che definisce gli attributi del nuovo modello personalizzato come corpo della richiesta.

<table>
  <caption>Tabella 1. Attributi di un nuovo modello di lingua personalizzato</caption>
  <tr>
    <th style="width:20%; text-align:left">Parametro</th>
    <th style="width:12%; text-align:center">Tipo di dati</th>
    <th style="text-align:left">Descrizione</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>Obbligatorio</em></td>
    <td style="text-align:center">Stringa</td>
    <td>
      Un nome definito dall'utente per il nuovo modello personalizzato. Utilizza un nome che
      descrive il dominio del modello personalizzato, come ad esempio <code>Medical
        custom model</code> o <code>Legal custom model</code>. Utilizza un
      nome che sia univoco tra tutti i modelli di lingua personalizzati di tua proprietà.
      Utilizza un nome localizzato che corrisponda alla lingua del modello personalizzato.
    </td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>Obbligatorio</em></td>
    <td style="text-align:center">Stringa</td>
    <td>
      Il nome del modello di lingua che deve essere personalizzato dal
      nuovo modello personalizzato. Specifica uno dei modelli di lingua supportati
      di cui esegue la restituzione il metodo <code>GET /v1/models</code>. Il nuovo
      modello può essere utilizzato solo con il modello di base che personalizza.
    </td>
  </tr>
  <tr>
    <td><code>dialect</code><br/><em>Facoltativo</em></td>
    <td style="text-align:center">Stringa</td>
    <td>
      Il dialetto della lingua specificata che deve essere utilizzato con il
      modello personalizzato. Per impostazione predefinita, il dialetto corrisponde alla lingua del
      modello di base; ad esempio, il dialetto è <code>en-US</code>
      per entrambi i modelli di lingua inglese (Stati Uniti),
      <code>en-US_BroadbandModel</code> o
      <code>en-US_NarrowbandModel</code>.<br/></br>
      Il parametro è significativo solo per i modelli in spagnolo, per cui
      il servizio crea un modello personalizzato adatto per un discorso nel
      dialetto indicato:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          <code>es-ES</code> per lo spagnolo castigliano (il valore predefinito)
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>es-LA</code> per lo spagnolo dell'America Latina
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>es-US</code> per lo spagnolo del Nord America (messicano)
        </li>
      </ul>
      Se specifichi un dialetto, deve essere valido per il modello di base.
    </td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>Facoltativo</em></td>
    <td style="text-align:center">Stringa</td>
    <td>
      Una descrizione del nuovo modello. Utilizza una descrizione localizzata che
      corrisponda alla lingua del modello personalizzato.
    </td>
  </tr>
</table>

Il seguente esempio crea un nuovo modello di lingua personalizzato denominato `Example model`. Il modello viene creato per il modello di base `en-US-BroadbandModel` e ha la descrizione `Example custom language model`. L'intestazione `Content-Type` specifica che al metodo si stanno passando dei dati JSON.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Example model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom language model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations"
```
{: pre}

L'esempio restituisce l'ID di personalizzazione del nuovo modello. Ogni modello personalizzato è identificato da un ID di personalizzazione univoco, che è un GUID (Globally Unique Identifier). Specifichi il GUID di un modello personalizzato con il parametro `customization_id` delle chiamate associate al modello.

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

Il nuovo modello personalizzato appartiene all'istanza del servizio di cui vengono utilizzate le credenziali per crearlo. Per ulteriori informazioni, vedi [Proprietà di modelli personalizzati](/docs/services/speech-to-text/custom.html#customOwner).

## Aggiungi un corpus al modello di lingua personalizzato
{: #addCorpus}

Dopo che hai creato il tuo modello di lingua personalizzato, il passo successivo consiste nell'aggiungere dati (parole specifiche per il dominio) al modello. Il metodo consigliato per popolare un modello personalizzato con nuove parole consiste nell'aggiungere uno o più corpora.

Un corpus è un file di testo semplice che idealmente contiene frasi di esempio dal tuo dominio. Il servizio analizza il contenuto di un file di corpus ed estrae le parole che non sono presenti nel suo vocabolario di base.Tali parole sono indicate come parole OOV (out-of-vocabulary).

-   Per ulteriori informazioni sull'utilizzo dei corpora, vedi [Utilizzo dei corpora](/docs/services/speech-to-text/language-resource.html#workingCorpora).
-   Per ulteriori informazioni su come il servizio aggiunge i corpora a un modello, vedi [Cosa succede quando aggiungo un file di corpus?](/docs/services/speech-to-text/language-resource.html#parseCorpus)

Fornendo frasi che includono nuove parole, i corpora consentono al servizio di imparare le parole in contesto. Puoi quindi aumentare o modificare le parole del modello singolarmente.  Addestrare un modello solo su singole parole invece che sulle parole aggiunte dai corpora è più dispendioso in termini di tempo e può produrre risultati meno efficaci.
{: tip}

Utilizzi il metodo `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` per aggiungere un corpus a un modello personalizzato:

-   Specifica l'ID di personalizzazione del modello personalizzato con il parametro di percorso `customization_id`.
-   Specifica un nome per il corpus con il parametro di percorso `corpus_name`. Utilizza un nome localizzato che corrisponda alla lingua del modello personalizzato e riflette il contenuto del corpus.
    -   Includi un massimo di 128 caratteri nel nome.
    -   Non includere spazi, `/` (barre) o `\` (barre rovesciate) nel nome.
    -   Non utilizzare il nome di un corpus che è già stato aggiunto al modello personalizzato.
    -   Non utilizzare il nome `user`, che è riservato dal servizio per denotare le parole personalizzate aggiunte o modificate dall'utente.
-   Passa il file di testo di corpus come corpo della richiesta.

Puoi aggiungere un massimo di 90.000 parole OOV e 10 milioni di parole in totale da tutte le origini combinate. Questo include le parole da corpora e grammatiche e le parole che aggiungi direttamente. Per ulteriori informazioni, vedi [Di quanti dati ho bisogno?](/docs/services/speech-to-text/language-resource.html#wordsResourceAmount)
{: note}

Il seguente esempio aggiunge il file di testo di corpus `healthcare.txt` al modello personalizzato con l'ID specificato. L'esempio denomina il corpus `healthcare`.

```bash
curl -X POST -u "apikey:{apikey}"
--data-binary @healthcare.txt
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/healthcare"
```
{: pre}

Il metodo accetta anche un parametro di query `allow_overwrite` facoltativo che sovrascrive un corpus esistente per un modello personalizzato. Utilizza il parametro se devi aggiornare un file di corpus dopo averlo aggiunto a un modello.

Il metodo è asincrono. Il suo completamento può richiedere uno o due minuti. La durata dell'operazione dipende dal numero totale di parole nel corpus, dal numero di nuove parole che il servizio trova nel corpus e dal carico corrente sul servizio. Per ulteriori informazioni sul controllo dello stato di un corpus, vedi [Monitoraggio della richiesta di aggiunta del corpus](#monitorCorpus).

Puoi aggiungere qualsiasi numero di corpora a un modello personalizzato richiamando il metodo una volta per ciascun file di testo di corpus. L'aggiunta di un corpus deve essere del tutto completa prima di poterne aggiungere un altro. Dopo che hai aggiunto un corpus a un modello personalizzato, esamina le nuove parole personalizzate per controllare l'eventuale presenza di errori di battitura e di altro tipo. Per ulteriori informazioni, vedi [Convalida di una risorsa di parole](/docs/services/speech-to-text/language-resource.html#validateModel).

### Monitoraggio della richiesta di aggiunta del corpus
{: #monitorCorpus}

Il servizio restituisce un codice di risposta 201 se il corpus è valido. Elabora quindi in modo asincrono il contenuto del corpus ed estrae automaticamente le nuove parole. Non puoi inoltrare le richieste di aggiunta di dati al modello personalizzato o di addestramento del modello fino al completamento dell'analisi del corpus da parte del servizio per la richiesta corrente.

Per determinare lo stato dell'analisi, utilizza il metodo `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` per eseguire il polling dello stato del corpus. Il metodo accetta l'ID del modello e il nome del corpus, come mostrato nel seguente esempio:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

La risposta include lo stato del corpus:

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

Il campo `status` ha uno dei seguenti valori:

-   `analyzed` indica che il servizio ha eseguito correttamente l'analisi del corpus.
-   `being_processed` indica che il servizio sta ancora analizzando il corpus.
-   `undetermined` indica che il servizio ha riscontrato un errore durante l'elaborazione del corpus.

Utilizza un loop per controllare lo stato del corpus ogni 10 secondi fino a quando non diventa `analyzed`. Per ulteriori informazioni sul controllo dello stato dei corpora di un modello, vedi [Elenco dei corpora per un modello di lingua personalizzato](/docs/services/speech-to-text/language-corpora.html#listCorpora).

## Aggiungi parole al modello di lingua personalizzato
{: #addWords}

Anche se l'aggiunta di corpora è il metodo consigliato per aggiungere parole a un modello di lingua personalizzato, puoi anche aggiungere delle singole parole personalizzate al modello direttamente. Il servizio aggiunge le parole personalizzate al modello personalizzato proprio come fa con le parole OOV che rileva dai corpora.

Se hai solo una o poche parole da aggiungere a un modello, utilizzare i corpora per aggiungere le parole potrebbe non essere pratico o neanche fattibile. L'approccio più semplice consiste nell'aggiungere una parola con solo la sua ortografia. Puoi però anche fornire più pronunce per la parola e indicare come deve essere visualizzata la parola.

-   Per ulteriori informazioni sull'aggiunta di parole direttamente, vedi [Utilizzo delle parole personalizzate](/docs/services/speech-to-text/language-resource.html#workingWords).
-   Per ulteriori informazioni su come il servizio aggiunge parole personalizzate a un modello, vedi [Cosa succede quando aggiungo o modifico una parola personalizzata?](/docs/services/speech-to-text/language-resource.html#parseWord)

Puoi utilizzare i seguenti metodi per aggiungere parole a un modello personalizzato:

-   Il metodo `POST /v1/customizations/{customization_id}/words` aggiunge più parole per volta. Passi un oggetto JSON che fornisce informazioni su ciascuna parola tramite il corpo della richiesta. Il seguente esempio aggiunge due parole personalizzate, `HHonors` e `IEEE`, al modello personalizzato con l'ID specificato. L'intestazione `Content-Type` specifica che al metodo si stanno passando dei dati JSON.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"words\": [
      {\"word\": \"HHonors\", \"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"},
      {\"word\": \"IEEE\", \"sounds_like\": [\"I. triple E.\"]}]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    Puoi anche aggiungere le parole da un file. Ad esempio, il file `words.json` definisce le stesse due parole personalizzate.

    ```
    {"words":
      [
        {"word": "HHonors", "sounds_like": ["hilton honors", "H. honors"], "display_as": "HHonors"},
        {"word": "IEEE", "sounds_like": ["I. triple E."]}
      ]
    }
    ```
    {: codeblock}

    Il seguente comando aggiunge le parole dal file:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data-binary @words.json
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    Questo metodo è asincrono. Il tempo che ci vuole per completare l'operazione dipende dal numero di parole che aggiungi e dal carico corrente sul servizio. Per ulteriori informazioni sul controllo dello stato dell'operazione, vedi [Monitoraggio della richiesta di aggiunta di parole](#monitorWords).
-   Il metodo `PUT /v1/customizations/{customization_id}/words/{word_name}` aggiunge singole parole. Passi un oggetto JSON che fornisce le informazioni sulla parola. Il seguente esempio aggiunge la parola `NCAA` al modello con l'ID specificato. Ancora, l'intestazione `Content-Type` indica che si stanno passando dei dati JSON al metodo.

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

    Questo metodo è sincrono. Il servizio restituisce un codice di risposta che notifica l'esito positivo o negativo della richiesta immediatamente.

Come con l'aggiunta di corpora, esamina le nuove parole personalizzate per controllare l'eventuale presenza di errori di battitura e di altro tipo. Questo controllo è particolarmente importante quando aggiungi più parole contemporaneamente. Per ulteriori informazioni, vedi [Convalida di una risorsa di parole](/docs/services/speech-to-text/language-resource.html#validateModel).

### Monitoraggio della richiesta di aggiunta di parole
{: #monitorWords}

Quando utilizzi il metodo `POST /v1/customizations/{customization_id}/words`, il servizio restituisce un codice di risposta 201 se i dati di input non sono validi. Elabora quindi in modo asincrono le parole per aggiungerle al modello. L'operazione è generalmente più veloce dell'aggiunta di un corpus o dell'addestramento di un modello ma la sua durata dipende dal numero di nuove parole aggiunte e dal carico corrente sul servizio. Non puoi inoltrare richieste di aggiunta di dati al modello personalizzato o di addestramento del modello fino a quando il servizio non completa la richiesta di aggiunta di nuove parole.

Per determinare lo stato della richiesta, utilizza il metodo `GET /v1/customizations/{customization_id}` per eseguire il polling dello stato del modello. Il metodo accetta l'ID di personalizzazione del modello e restituisce le informazioni che includono lo stato del modello, come nel seguente esempio:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

Il campo `status` notifica lo stato corrente del modello. Mentre il servizio sta elaborando le nuove parole, lo stato rimane `pending`. Utilizza un loop per controllare lo stato ogni 10 secondi finché non diventa `ready` per indicare che l'operazione è stata completata. Per ulteriori informazioni sui possibili valori di `status`, vedi [Monitoraggio della richiesta di addestramento del modello](#monitorTraining-language).

### Modifica di parole in un modello personalizzato
{: #modifyWord}

Puoi anche utilizzare i metodi `POST /v1/customizations/{customization_id}/words` e `PUT /v1/customizations/{customization_id}/words/{word_name}` per modificare o aumentare una parola in un modello personalizzato. Potresti dover utilizzare i metodi per correggere un errore di battitura oppure un altro errore che era stato fatto quando una parola è stata aggiunta al modello. Potresti anche dover aggiungere delle definizioni di suono simile (sounds-like) per una parola esistente.

Utilizzi i metodi per modificare la definizione di una parola esistente esattamente come fai per aggiungere una parola. I nuovi dati che fornisci per la parola sovrascrivono la definizione esistente della parola.

## Addestra il modello di lingua personalizzato
{: #trainModel-language}

Dopo che hai popolato un modello di lingua personalizzato con nuove parole (aggiungendo corpora e grammatiche oppure direttamente le parole), devi addestrare il modello sui nuovi dati. L'addestramento prepara il modello personalizzato a utilizzare i dati nel riconoscimento vocale. Il modello non utilizza le parole che aggiungi servendoti di un qualsiasi metodo finché non ne esegui l'addestramento sui dati.

Utilizzi il metodo `POST /v1/customizations/{customization_id}/train` per addestrare un modello personalizzato. Passi al metodo l'ID di personalizzazione del modello che vuoi addestrare, come nel seguente esempio:

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

Puoi utilizzare il parametro di query `word_type_to_add` facoltativo per specificare le parole su cui deve essere addestrato il modello personalizzato:

-   Specifica `all` oppure ometti il parametro per addestrare il modello su tutte le sue parole, indipendentemente dalla loro origine.
-   Specifica `user` per addestrare il modello solo sulle parole che erano state aggiunte o modificate dall'utente, ignorando quelle estratte solo da corpora o grammatiche.

    Questa opzione è utile se aggiungi dei corpora con dati che contengono informazioni extra senza senso, come ad esempio delle parole che contengono errori di battitura. Prima di addestrare il modello su tali dati, puoi utilizzare il parametro di query `word_type` del metodo `GET /v1/customizations/{customization_id}/words` per esaminare le parole estratte da corpora e grammatiche. Per ulteriori informazioni, vedi [Elenco delle parole da un modello di lingua personalizzato](/docs/services/speech-to-text/language-words.html#listWords).

Puoi inoltre utilizzare il parametro di query `customization_weight` facoltativo. Il parametro specifica il peso relativo che viene dato alle parole dal modello personalizzato invece che alle parole dal vocabolario di base quando il modello personalizzato viene utilizzato per il riconoscimento vocale. Puoi anche specificare un peso di personalizzazione con qualsiasi richiesta di riconoscimento che utilizza il modello personalizzato. Per ulteriori informazioni, vedi [Utilizzo del peso di personalizzazione](/docs/services/speech-to-text/language-use.html#weight). 

Il metodo è asincrono. Il completamento dell'addestramento può richiedere qualche minuto, a seconda del numero di nuove parole su cui si sta eseguendo l'addestramento del modello e del carico corrente sul servizio. Per ulteriori informazioni sul controllo dello stato di un'operazione di addestramento, vedi [Monitoraggio della richiesta di addestramento del modello](#monitorTraining-language).

### Monitoraggio della richiesta di addestramento del modello
{: #monitorTraining-language}

Il servizio restituisce un codice di risposta 200 se inizia il processo di addestramento con esito positivo. Il servizio non può accettare richieste di addestramento successive, o richieste di aggiunta di nuovi corpora, grammatiche o parole, fino a quando la richiesta esistente non viene completata.

Per determinare lo stato di una richiesta di addestramento, utilizza il metodo `GET /v1/customizations/{customization_id}` per eseguire il polling dello stato del modello. Il metodo accetta l'ID di personalizzazione del modello e restituisce le informazioni relative al modello.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

La risposta include i campi `status` e `progress` che notificano lo stato del modello personalizzato. Il significato del campo `progress` dipende dallo stato del modello. Il campo `status` può avere uno dei seguenti valori:

-   `pending` indica che il modello è stato creato ma che sta attendendo che vengano aggiunti i dati di addestramento o che il servizio termini l'analisi dei dati che erano stati aggiunti. Il campo `progress` è `0`.
-   `ready` indica che il modello è pronto per essere addestrato. Il campo `progress` è `0`.
-   `training` indica che il modello è in fase di addestramento. Il campo `progress` cambia da `0` a `100` al termine dell'addestramento. <!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available` indica che il modello è addestrato e pronto per l'uso. Il campo `progress` è `100`.
-   `upgrading` indica che il modello è in fase di upgrade. Il campo `progress` è `0`.
-   `failed` indica che l'addestramento del modello non è riuscito. Il campo `progress` è `0`.

Utilizza un loop per controllare lo stato ogni 10 secondi fino a quando non diventa `available`. Per ulteriori informazioni sul controllo dello stato di un modello personalizzato, vedi [Elenco dei modelli di lingua personalizzati](/docs/services/speech-to-text/language-models.html#listModels-language).

### Errori di addestramento
{: #failedTraining-language}

L'addestramento non viene avviato se il servizio sta gestendo un'altra richiesta per il modello di lingua personalizzato. Ad esempio, l'avvio di una richiesta di addestramento non riesce se il servizio sta

-   Elaborando un corpus o una grammatica per generare un elenco di parole OOV
-   Elaborando parole personalizzate per convalidare o generare automaticamente pronunce dal suono simile (sounds-like)
-   Gestendo un'altra richiesta di addestramento

L'avvio dell'addestramento può non riuscire anche per i seguenti motivi:

-   Non sono stati aggiunti dati di addestramento (corpora, grammatiche o parole) al modello personalizzato da quando è stato creato o da quando ne è stato eseguito l'ultima volta l'addestramento.
-   Una o più parole che erano state aggiunte al modello personalizzato hanno delle pronunce dal suono simile (sounds-like) che devi correggere.

Se lo stato dell'addestramento di un modello personalizzato è `failed`, utilizza i metodi dell'interfaccia di personalizzazione per esaminare le parole del modello e correggere gli eventuali errori che rilevi. Per ulteriori informazioni, vedi [Convalida di una risorsa di parole](/docs/services/speech-to-text/language-resource.html#validateModel).

## Script di esempio 
{: #exampleScripts}

Puoi utilizzare i seguenti script per fare delle prove con i passi per la creazione di un modello di lingua personalizzato:

-   Uno script Python denominato <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="Icona link esterno" title="Icona link esterno"></a>. Per ulteriori informazioni, vedi [Script Python di esempio](#pythonScript).
-   Uno script shell Bash denominato <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="Icona link esterno" title="Icona link esterno"></a>. Per ulteriori informazioni, vedi [Script shell di esempio](#shellScript).

I due script forniscono una funzionalità identica. Ogni script crea un modello personalizzato, aggiunge le parole da un file di testo di corpus e aggiunge sia una che più parole al modello direttamente. Lo script interroga il modello per elencare le parole aggiunte da un corpus e direttamente dall'utente. Addestra anche il modello ed evidenzia il polling consigliato per il monitoraggio dei risultati delle operazioni asincrone.

Puoi utilizzare l'uno o l'altro dei file di testo di corpus forniti con gli script oppure puoi eseguire test con dei tuoi file di corpus:

-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/corpus.txt" download="corpus.txt">corpus.txt <img src="../../icons/launch-glyph.svg" alt="Icona link esterno" title="Icona link esterno"></a> è un corpus di 6 KB abbreviato che aggiunge sei termini correlati al settore dell'assistenza sanitaria a un modello. Questo file produce una piccola quantità di output quando viene utilizzato con gli script. Gli script utilizzano questo file per impostazione predefinita.
-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/healthcare.txt" download="healthcare.txt">healthcare.txt <img src="../../icons/launch-glyph.svg" alt="Icona link esterno" title="Icona link esterno"></a> è un corpus di 164 KB più ricco che aggiunge molti termini correlati al settore dell'assistenza sanitaria a un modello. Questo file produce molto più output quando viene utilizzato con gli script.

Per impostazione predefinita, il nuovo modello personalizzato che crei con l'uno o l'altro script è disponibile per l'utilizzo con le richieste di riconoscimento. Gli script includono un passo facoltativo per l'eliminazione del nuovo modello personalizzato, che può essere utile quando stai facendo delle prove con il processo. Attieniti ai commenti negli script per abilitare il passo di eliminazione.

### Script Python di esempio
{: #pythonScript}

Per utilizzare lo script Python, attieniti alla seguente procedura:

1.  Scarica lo script Python denominato <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="Icona link esterno" title="Icona link esterno"></a>.
1.  Scarica i file di testo di corpus di esempio da utilizzare con lo script. Puoi liberamente eseguire test con l'uno o l'altro dei file di testo di corpus o con un file di tua scelta. Per impostazione predefinita, tutti i file di testo di corpus devono risiedere nella stessa directory dello script.
1.  Lo script utilizza la libreria `requests` di Python per le richieste HTTP al servizio. Utilizza `pip` o `easy_install` per installare la libreria per l'utilizzo da parte dello script, ad esempio:

    ```bash
    pip install requests
    ```
    {: pre}

    Per ulteriori informazioni sulla libreria, vedi [pypi.python.org/pypi/requests ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://pypi.python.org/pypi/requests){: new_window}.
1.  Modifica lo script per sostituire la stringa `password` `iam_apikey` con la chiave API dalle tue credenziali del servizio {{site.data.keyword.speechtotextshort}}:

    ```
    password = "iam_apikey"
    ```
    {: codeblock}

1.  Esegui lo script immettendo il seguente comando:

    ```bash
    python testSTTcustom.py
    ```
    {: pre}

Lo script utilizza il seguente URL predefinito per l'ubicazione Dallas: `https://stream.watsonplatform.net`. Se hai creato la tua istanza del servizio in un'ubicazione diversa, modifica le variabili `uri` per utilizzare la tua ubicazione. Ad esempio, utilizza `https://gateway-wdc.watsonplatform.net` se la tua istanza del servizio risiede nell'ubicazione Washington, DC.
{: note}

### Script shell di esempio
{: #shellScript}

Per utilizzare lo script shell Bash, attieniti alla seguente procedura:

1.  Scarica lo script shell denominato <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="Icona link esterno" title="Icona link esterno"></a>.
1.  Scarica i file di testo di corpus di esempio da utilizzare con lo script. Puoi liberamente eseguire test con l'uno o l'altro dei file di testo di corpus o con un file di tua scelta. Per impostazione predefinita, tutti i file di testo di corpus devono risiedere nella stessa directory dello script.
1.  Lo script utilizza il comando `curl` per le richieste HTTP al servizio. Se non hai già scaricato `curl`, puoi installare la versione per il tuo sistema operativo da [curl.haxx.se ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://curl.haxx.se){: new_window}. Installa la versione che supporta il protocollo SSL (Secure Sockets Layer) e assicurati di includere il file binario installato nella tua variabile di ambiente `PATH`.
1.  Modifica lo script per sostituire la stringa `PASSWORD` `iam_apikey` con la chiave API dalle tue credenziali del servizio {{site.data.keyword.speechtotextshort}}:

    ```
    PASSWORD="iam_apikey"
    ```
    {: codeblock}

1.  Modifica lo script per sostituire la stringa `URL` con l'URL per l'ubicazione in cui hai creato l'istanza del servizio. Lo script utilizza il seguente URL predefinito per l'ubicazione Dallas:

    ```
    URL="https://stream.watsonplatform.net/speech-to-text/api/v1"
    ```
    {: codeblock}

1.  Assicurati che lo script disponga delle autorizzazioni eseguibili ed eseguilo quindi immettendo il seguente comando:

    ```bash
    ./testSTTcustom.sh
    ```
    {: pre}
