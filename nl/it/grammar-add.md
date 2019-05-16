---

copyright:
  years: 2015, 2019
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

# Aggiunta di una grammatica a un modello di lingua personalizzato
{: #grammarAdd}

Prima di poter utilizzare una grammatica per il riconoscimento vocale, devi innanzitutto utilizzare l'interfaccia di personalizzazione per aggiungere la grammatica a un modello di lingua personalizzato. I passi per aggiungere una grammatica a un modello di lingua personalizzato sono simili a quelli utilizzati per aggiungere corpora o parole personalizzate:
{: shortdesc}

1.  [Crea un modello di lingua personalizzato](#createModel-grammar). Puoi creare un nuovo modello personalizzato o utilizzare un modello esistente.
1.  [Aggiungi una grammatica al modello di lingua personalizzato](#addGrammar). Il servizio convalida la grammatica per garantirne la correttezza.
1.  [Convalida le parole dalla grammatica](#validateGrammar). Verifica la correttezza delle pronunce dal suono simile (sounds-like) per qualsiasi parola OOV (out-of-vocabulary) riconosciuta dalla grammatica.
1.  [Addestra il modello di lingua personalizzato](#trainGrammar). Il servizio prepara il modello personalizzato e la grammatica per l'utilizzo nel riconoscimento vocale.
1.  Puoi ora utilizzare il modello personalizzato e la grammatica nelle richieste di riconoscimento vocale. Per ulteriori informazioni, vedi [Utilizzo di una grammatica per il riconoscimento vocale](/docs/services/speech-to-text/grammar-use.html).

Questi passi sono iterativi. Puoi aggiungere grammatiche, così come corpora e parole personalizzate, a un modello di lingua personalizzato tutte le volte che lo ritieni necessario. Devi addestrare il modello personalizzato su qualsiasi nuova risorsa di dati (grammatiche, corpora o parole personalizzate) che aggiungi. Quando lo usi per il riconoscimento vocale, un modello personalizzato riflette i dati relativi all'ultimo addestramento.

Puoi utilizzare le grammatiche con qualsiasi lingua che supporti la personalizzazione del modello di lingua. La personalizzazione del modello di lingua è disponibile per la maggior parte delle lingue. Per ulteriori informazioni, vedi [Supporto delle lingue per la personalizzazione](/docs/services/speech-to-text/custom.html#languageSupport).
{: note}

## Crea un modello di lingua personalizzato
{: #createModel-grammar}

Per utilizzare una grammatica con il riconoscimento vocale, devi aggiungerla a un modello di lingua personalizzato. Puoi utilizzare un modello di lingua personalizzato esistente o puoi creare un nuovo modello personalizzato utilizzando il metodo `POST /v1/customizations`. Per ulteriori informazioni sulla creazione di un nuovo modello personalizzato, vedi [Crea un modello di lingua personalizzato](/docs/services/speech-to-text/language-create.html#createModel-language).

Un modello di lingua personalizzato può contenere corpora e parole personalizzate così come le grammatiche. Durante il riconoscimento vocale, puoi utilizzare il modello personalizzato con o senza le sue grammatiche. Tuttavia, quando utilizzi una grammatica, il servizio riconosce solo le parole dalla grammatica specificata.

## Aggiungi una grammatica al modello di lingua personalizzato
{: #addGrammar}

Utilizza il metodo `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` per aggiungere un file di grammatica a un modello personalizzato.

-   Specifica l'ID di personalizzazione del modello di lingua personalizzato con il parametro di percorso `customization_id`.
-   Specifica un nome per la grammatica con il parametro di percorso `grammar_name`. Utilizza un nome localizzato che corrisponda alla lingua del modello personalizzato e che rifletta il contenuto della grammatica.
    -   Includi un massimo di 128 caratteri nel nome.
    -   Non includere spazi, barre o barre rovesciate nel nome.
    -   Non utilizzare il nome di una grammatica o un corpus che è già stato aggiunto al modello personalizzato.
    -   Non utilizzare il nome `user`, che è riservato dal servizio per denotare le parole personalizzate aggiunte o modificate dall'utente.

-   Utilizza l'intestazione della richiesta `Content-Type` per specificare il formato della grammatica:
    -   `application/srgs` per una grammatica ABNF
    -   `application/srgs+xml` per una grammatica XML

-   Passa il file che contiene la grammatica come corpo della richiesta.

Il seguente esempio aggiunge il file di grammatica denominato `confirm.abnf` al modello personalizzato con l'ID specificato. L'esempio denomina la grammatica `confirm-abnf`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/srgs"
--data-binary @confirm.abnf
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

Il metodo accetta anche un parametro di query facoltativo `allow_overwrite` che puoi includere per sovrascrivere una grammatica esistente con lo stesso nome. Utilizza il parametro se devi aggiornare una grammatica dopo averla aggiunta a un modello.

Il metodo è asincrono. L'analisi della grammatica da parte del servizio può richiedere alcuni secondi, a seconda della dimensione della grammatica e del carico corrente sul servizio. Per informazioni sul controllo dello stato di una grammatica, vedi [Monitoraggio della richiesta di aggiunta della grammatica](#monitorGrammar).

Puoi aggiungere qualsiasi numero di grammatiche a un modello personalizzato chiamando il metodo una volta per ciascun file di grammatica. L'aggiunta di una grammatica deve essere completata correttamente prima di poterne aggiungere un'altra.

### Monitoraggio della richiesta di aggiunta della grammatica
{: #monitorGrammar}

Se la grammatica è valida, il servizio restituisce un codice di risposta 201. Quindi, elabora in modo asincrono il contenuto della grammatica ed estrae automaticamente le nuove parole che la grammatica può riconoscere. Non puoi inoltrare richieste per aggiungere ulteriori risorse di dati a un modello personalizzato o per addestrare il modello finché il servizio non completa l'analisi della grammatica per la richiesta corrente.

Per determinare lo stato dell'analisi, utilizza il metodo `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` per eseguire il polling dello stato della grammatica. Il metodo accetta l'ID del modello personalizzato e il nome della grammatica. Il seguente esempio controlla lo stato della grammatica denominata `confirm-abnf`.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

La risposta include lo stato della grammatica:

```javascript
{
  "name": "confirm-abnf",
  "out_of_vocabulary_words": 0,
  "status": "being_processed"
}
```
{: codeblock}

Il campo `status` ha uno dei seguenti valori:

-   `analyzed` indica che il servizio ha analizzato correttamente la grammatica.
-   `being_processed` indica che il servizio sta ancora analizzando la grammatica.
-   `undetermined` indica che il servizio ha rilevato un errore durante l'elaborazione della grammatica.

Utilizza un loop per controllare lo stato della grammatica ogni 10 secondi finché non diventa `analyzed`. Per ulteriori informazioni sul controllo dello stato di una grammatica, vedi [Elenco delle grammatiche per un modello di lingua personalizzato](/docs/services/speech-to-text/grammar-manage.html#listGrammars).

### Gestione degli errori di aggiunta della grammatica
{: #handleFailures}

Se la sua analisi di una grammatica non riesce, il servizio imposta lo stato della grammatica su `undetermined` e include un campo `error` che descrive l'errore con lo stato della grammatica. Puoi anche utilizzare il metodo `GET /v1/customizations/{customization_id}` per controllare lo stato del modello personalizzato. Se l'aggiunta di una grammatica non riesce, l'output include un messaggio di errore come il seguente:

```javascript
{
  . . .
  "error": "{\"code\":500, \"code_description\":\"Internal Server Error\",
\". . . Cannot compile grammar . . .\"},
  . . .
}
```
{: codeblock}

Lo stato del modello personalizzato non viene influenzato dall'errore, ma la grammatica non può essere utilizzata con il modello. Puoi risolvere il problema correggendo il file di grammatica e ripetendo la richiesta per aggiungerlo al modello personalizzato. Imposta il parametro `allow_overwrite` su `true` per sovrascrivere la versione del file di grammatica che ha avuto esito negativo.

Puoi anche eliminare la grammatica dal modello personalizzato per il momento e quindi correggere e aggiungere nuovamente il file di grammatica in futuro. Per ulteriori informazioni sull'eliminazione di una grammatica, vedi [Eliminazione di una grammatica da un modello di lingua personalizzato](/docs/services/speech-to-text/grammar-manage.html#deleteGrammar).

## Convalida le parole dalla grammatica
{: #validateGrammar}

Quando aggiungi un file di grammatica a un modello di lingua personalizzato, il servizio analizza la grammatica per determinare se questa riconosce le parole che non fanno già parte del vocabolario di base del servizio. Tali parole sono indicate come parole OOV (out-of-vocabulary). Il servizio aggiunge le parole OOV alla risorsa di parole del modello personalizzato. Lo scopo della risorsa di parole è quello di definire le parole che non sono già presenti nel vocabolario del servizio.

Le definizioni nella risorsa di parole indicano al servizio come trascrivere le parole OOV. Le informazioni includono un campo `sounds_like` che indica al servizio come viene pronunciata la parola, un campo `display_as` che indica al servizio come visualizzare una parola e un campo `source` che indica il modo in cui la parola è stata aggiunta al modello personalizzato. Per ulteriori informazioni sulla risorsa di parole e sulle parole OOV, vedi [La risorsa di parole](/docs/services/speech-to-text/language-resource.html#wordsResource).

Dopo aver aggiunto una grammatica a un modello personalizzato, è buona norma esaminare le parole OOV nella risorsa di parole del modello per verificare le loro pronunce dal suono simile. Non tutte le grammatiche hanno le parole OOV, ma la verifica della risorsa di parole è generalmente una buona idea. Per controllare le parole del modello personalizzato dopo aver aggiunto una grammatica, utilizza i seguenti metodi:

-   Utilizza il metodo `GET /v1/customizations/{customization_id}/words` per elencare tutte le parole da un modello personalizzato. Passa il valore `grammars` con il parametro `word_type` del metodo per elencare solo le parole che sono state aggiunte dalle grammatiche.
-   Utilizza il metodo `GET /v1/customizations/{customization_id}/words/{word_name}` per visualizzare una singola parola da un modello.

Verifica che le pronunce dal suono simile delle parole siano accurate e corrette. Controlla anche errori tipografici e di altro tipo nelle parole stesse. Per ulteriori informazioni sulla convalida e correzione delle parole in un modello personalizzato, vedi [Convalida di una risorsa di parole](/docs/services/speech-to-text/language-resource.html#validateModel).

## Addestra il modello di lingua personalizzato
{: #trainGrammar}

Il passo finale prima di poter utilizzare una grammatica con un modello di lingua personalizzato è quello di addestrare il modello. Puoi utilizzare lo stesso processo che utilizzi per addestrare un modello personalizzato su nuovi corpora o nuove parole personalizzate. L'addestramento del modello compila la grammatica da utilizzare durante il riconoscimento vocale. Il servizio elabora la grammatica dal suo formato originale basato su testo in un formato di runtime binario per il riconoscimento vocale. Non puoi utilizzare la grammatica finché non addestri il modello.

Utilizzi il metodo `POST /v1/customizations/{customization_id}/train` per addestrare un modello personalizzato. Passi al metodo l'ID di personalizzazione del modello che vuoi addestrare, come nel seguente esempio.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

Il metodo di addestramento è asincrono. In genere, l'addestramento richiede solo pochi secondi. Ma può richiedere più tempo a seconda della dimensione e della complessità della grammatica e del carico corrente sul servizio. Per informazioni sul controllo dello stato di un'operazione di addestramento, vedi [Monitoraggio della richiesta di addestramento](#monitorTraining-grammar).

### Monitoraggio della richiesta di addestramento
{: #monitorTraining-grammar}

Se il processo di addestramento viene avviato correttamente, il servizio restituisce un codice di risposta 200. Il servizio non può accettare richieste di addestramento successive o richieste per aggiungere nuove grammatiche, corpora o parole al modello personalizzato finché non viene completata la richiesta di addestramento corrente.

Per determinare lo stato di una richiesta di addestramento, utilizza il metodo `GET /v1/customizations/{customization_id}` per eseguire il polling dello stato del modello. Il metodo accetta l'ID di personalizzazione del modello e restituisce informazioni come quelle che seguono:

```
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2018-06-01T18:42:25.324Z",
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

Il campo `status` ha il valore `training` mentre il modello è in fase di addestramento. Utilizza un loop per controllare lo stato del modello ogni 10 secondi finché non diventa `available`. Per ulteriori informazioni sul monitoraggio di una richiesta di addestramento e altri possibili valori di stato, vedi [Monitoraggio della richiesta di addestramento del modello](/docs/services/speech-to-text/language-create.html#monitorTraining-language).
