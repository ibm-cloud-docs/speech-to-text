---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-21"

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

# Gestione dei modelli di lingua personalizzati
{: #manageLanguageModels}

L'interfaccia di personalizzazione include il metodo `POST /v1/customizations` per creare un modello di lingua personalizzato. L'interfaccia include anche il metodo `POST /v1/customizations/train` per addestrare un modello personalizzato sui dati più recenti dalla sua risorsa di parole. Per ulteriori informazioni, vedi
{: shortdesc}

-   [Crea un modello di lingua personalizzato](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language)
-   [Addestra il modello di lingua personalizzato](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language)

Inoltre, l'interfaccia include dei metodi per elencare le informazioni sui modelli di lingua personalizzati, reimpostare un modello personalizzato al suo stato iniziale, eseguire l'upgrade di un modello personalizzato ed eliminare un modello personalizzato. Non puoi addestrare, reimpostare, eseguire l'upgrade o eliminare un modello personalizzato mentre il servizio gestisce un'altra operazione su quel modello, inclusa l'aggiunta di risorse al modello.

## Elenco dei modelli di lingua personalizzati
{: #listModels-language}

L'interfaccia di personalizzazione fornisce due metodi per elencare le informazioni sui modelli di lingua personalizzati che appartengono alle credenziali specificate:

-   Il metodo `GET /v1/customizations` elenca le informazioni su tutti i modelli di lingua personalizzati o su tutti i modelli di lingua personalizzati per una specifica lingua.
-   Il metodo `GET /v1/customizations/ {` elenca le informazioni su uno specifico modello di lingua personalizzato. Utilizza questo metodo per eseguire il polling del servizio in merito allo stato di una richiesta di addestramento o di una richiesta di aggiunta di nuove parole.

Entrambi i metodi restituiscono le seguenti informazioni su un modello personalizzato:

-   `customization_id` identifica il GUID (Globally Unique Identifier) del modello personalizzato. Il GUID viene utilizzato per identificare il modello nei metodi dell'interfaccia.
-   `created` è la data e l'ora in formato UTC (Coordinated Universal Time) in cui è stato creato il modello personalizzato.
-   `updated` è la data e l'ora in formato UTC (Coordinated Universal Time) in cui è stata apportata l'ultima modifica al modello personalizzato.
-   `language` è la lingua del modello personalizzato.
-   `dialect` è il dialetto della lingua per il modello personalizzato, che non corrisponde necessariamente alla lingua del modello personalizzato per i modelli in spagnolo. Per ulteriori informazioni, vedi la descrizione del parametro `dialect` in [Crea un modello di lingua personalizzato](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language).
-   `owner` identifica le credenziali dell'istanza del servizio proprietaria del modello personalizzato.
-   `name` è il nome del modello personalizzato.
-   `description` mostra la descrizione del modello personalizzato, se ne è stata fornita una alla sua creazione.
-   `base_model` indica il nome del modello di lingua per il quale è stato creato il modello personalizzato.
-   `versions` fornisce un elenco delle versioni disponibili del modello personalizzato. Ogni elemento dell'array indica una versione del modello di base con cui è possibile utilizzare il modello personalizzato. Esistono più versioni solo se viene eseguito un upgrade del modello personalizzato. Altrimenti, viene mostrata solo una singola versione. Per ulteriori informazioni, vedi [Elenco delle informazioni sulla versione per un modello personalizzato](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeList).

Il metodo restituisce anche un campo `status` che indica lo stato del modello personalizzato:

-   `pending` indica che il modello è stato creato. È in attesa che vengano aggiunti dati di addestramento validi (corpora, grammatiche o parole) o che il servizio finisca di analizzare i dati che erano stati aggiunti.
-   `ready` indica che il modello contiene dati validi ed è pronto per essere addestrato. Se il modello contiene una combinazione di risorse valide e non valide, l'addestramento del modello non riesce a meno che non imposti il parametro di query `strict` su `false`. Per ulteriori informazioni, vedi [Errori di addestramento](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#failedTraining-language).
-   `training` indica che il modello è in fase di addestramento sui dati.
-   `available` indica che il modello è addestrato e pronto per l'uso con una richiesta di riconoscimento.
-   `upgrading` indica che il modello è in fase di upgrade.
-   `failed` indica che l'addestramento del modello non è riuscito. Esamina le parole nella risorsa di parole del modello per determinare gli errori che hanno impedito l'addestramento del modello.

Inoltre, l'output include un campo `progress` che indica lo stato di avanzamento corrente dell'addestramento del modello personalizzato. Se hai utilizzato il metodo `POST /v1/customizations/{customization_id}/train` per iniziare l'addestramento del modello, questo campo indica lo stato di avanzamento corrente di tale richiesta come una percentuale di completamento. Attualmente, il valore del campo è `100` se lo stato è `available`; altrimenti, è `0`.

### Richieste e risposte di esempio
{: #listExample-language}

Il seguente esempio include il parametro di query `language` per elencare tutti i modelli di lingua personalizzati in inglese (Stati Uniti) che appartengono alle credenziali specificate:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations?language=en-US"
```
{: pre}

Le credenziali possiedono due di questi modelli. Il primo modello è in attesa di dati oppure il servizio ne sta eseguendo l'elaborazione. Il secondo modello è completamente addestrato e pronto per l'uso.

```javascript
{
  "customizations": [
    {
      "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
      "created": "2016-06-01T18:42:25.324Z",
      "updated": "2016-06-01T18:42:25.324Z",
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
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732fae4829",
      "created": "2016-06-01T18:51:37.291Z",
      "updated": "2016-06-01T20:02:10.624Z",
      "language": "en-US",
      "dialect": "en-US",
      "versions": [
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model two",
      "description": "Example custom language model two",
      "base_model_name": "en-US_BroadbandModel",
      "status": "available",
      "progress": 100
    }
  ]
}
```
{: codeblock}

Il seguente esempio restituisce informazioni sul modello personalizzato con l'ID di personalizzazione specificato:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T18:42:25.324Z",
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

## Reimpostazione di un modello di lingua personalizzato
{: #resetModel-language}

Utilizza il metodo `POST /v1/customizations/{customization_id}/reset` per reimpostare un modello personalizzato. La reimpostazione di un modello rimuove tutti i corpora e tutte le parole dal modello, inizializzando il modello al suo stato alla creazione. Il metodo non elimina il modello stesso o i metadati quali il suo nome e la sua lingua. Tuttavia, quando reimposti un modello, la sua risorsa di parole è vuota e deve essere ricreata aggiungendo corpora e parole.

### Richiesta di esempio
{: #resetExample-language}

Il seguente esempio reimposta il modello personalizzato con l'ID di personalizzazione specificato:

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/reset"
```
{: pre}

## Eliminazione di un modello di lingua personalizzato
{: #deleteModel-language}

Utilizza il metodo `DELETE /v1/customizations/{customization_id}` per eliminare un modello di lingua personalizzato di cui non hai più bisogno. Il metodo elimina tutti i corpora e tutte le parole associati al modello personalizzato e il modello stesso. Utilizza questo metodo con cautela: un modello personalizzato e i relativi dati non possono essere riacquisti una volta che hai eliminato il modello.

### Richiesta di esempio
{: #deleteExample-language}

Il seguente esempio elimina il modello personalizzato con l'ID di personalizzazione specificato:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}
