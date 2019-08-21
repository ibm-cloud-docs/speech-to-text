---

copyright:
  years: 2019
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

# Gestione dei modelli acustici personalizzati
{: #manageAcousticModels}

L'interfaccia di personalizzazione include il metodo `POST /v1/acoustic_customizations` per la creazione di un modello acustico personalizzato. L'interfaccia include anche il metodo `POST /v1/acoustic_customizations/train` per l'addestramento di un modello personalizzato sulle sue ultime risorse audio. Per ulteriori informazioni, vedi
{: shortdesc}

-   [Crea un modello acustico personalizzato](/docs/services/speech-to-text?topic=speech-to-text-acoustic#createModel-acoustic)
-   [Addestra il modello acustico personalizzato](/docs/services/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic)

Inoltre, l'interfaccia include dei metodi per elencare le informazioni sui modelli acustici personalizzati, reimpostare un modello personalizzato al suo stato iniziale, eseguire l'upgrade di un modello personalizzato ed eliminare un modello personalizzato. Non puoi addestrare, reimpostare, eseguire l'upgrade o eliminare un modello personalizzato mentre il servizio gestisce un'altra operazione su quel modello, inclusa l'aggiunta di risorse audio al modello.

## Elenco dei modelli acustici personalizzati
{: #listModels-acoustic}

L'interfaccia di personalizzazione fornisce due metodi per elencare le informazioni sui modelli acustici personalizzati che appartengono alle credenziali specificate:

-   Il metodo `GET /v1/acoustic_customizations` elenca le informazioni su tutti i modelli acustici personalizzati o su tutti i modelli acustici personalizzati per una lingua specificata.
-   Il metodo `GET /v1/acoustic_customizations/{customization_id}` elenca le informazioni su un modello acustico personalizzato specificato. Utilizza questo metodo per eseguire il polling del servizio in merito allo stato di una richiesta di addestramento.

Entrambi i metodi restituiscono le seguenti informazioni relative a un modello acustico personalizzato:

-   `customization_id` identifica il GUID (Globally Unique Identifier) del modello personalizzato. Il GUID viene utilizzato per identificare il modello nei metodi dell'interfaccia.
-   `created` è la data e l'ora in formato UTC (Coordinated Universal Time) in cui è stato creato il modello personalizzato.
-   `updated` è la data e l'ora in formato UTC (Coordinated Universal Time) in cui è stata apportata l'ultima modifica al modello personalizzato.
-   `language` è la lingua del modello personalizzato.
-   `owner` identifica le credenziali dell'istanza del servizio proprietaria del modello personalizzato.
-   `name` è il nome del modello personalizzato.
-   `description` mostra la descrizione del modello personalizzato, se ne è stata fornita una alla sua creazione.
-   `base_model_name` indica il nome del modello di lingua per il quale è stato creato il modello personalizzato.
-   `versions` fornisce un elenco delle versioni disponibili del modello personalizzato. Ogni elemento dell'array indica una versione del modello di base con cui è possibile utilizzare il modello personalizzato. Esistono più versioni solo se viene eseguito un upgrade del modello personalizzato. Altrimenti, viene mostrata solo una singola versione. Per ulteriori informazioni, vedi [Elenco delle informazioni sulla versione per un modello personalizzato](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeList).

I metodi restituiscono anche un campo `status` che indica lo stato del modello personalizzato:

-   `pending` indica che il modello è stato creato. È in attesa che vengano aggiunti dati di addestramento validi (risorse audio) o che il servizio finisca di analizzare i dati che erano stati aggiunti.
-   `ready` indica che il modello contiene dati audio validi ed è pronto per essere addestrato. Se il modello contiene una combinazione di risorse audio valide e non valide, l'addestramento del modello non riesce a meno che non imposti il parametro di query `strict` su `false`. Per ulteriori informazioni, vedi [Errori di addestramento](/docs/services/speech-to-text?topic=speech-to-text-acoustic#failedTraining-acoustic).
-   `training` indica che il modello è in fase di addestramento sui dati audio.
-   `available` indica che il modello è addestrato e pronto per l'uso con le richieste di riconoscimento.
-   `upgrading` indica che il modello è in fase di upgrade.
-   `failed` indica che l'addestramento del modello non è riuscito. Esamina le risorse audio del modello per determinare cosa ha impedito l'addestramento del modello. Possibili errori includono audio insufficiente, troppo audio o una risorsa audio non valida.

Inoltre, l'output include un campo `progress` che indica lo stato di avanzamento corrente dell'addestramento del modello personalizzato. Se hai utilizzato il metodo `POST /v1/acoustic_customizations/{customization_id}/train` per avviare l'addestramento del modello, questo campo indica l'avanzamento corrente di tale richiesta come percentuale completa. Attualmente, il valore del campo è `100` se lo stato è `available`; altrimenti, è `0`.

### Richieste e risposte di esempio
{: #listExample-acoustic}

Il seguente esempio include il parametro di query `language` per elencare tutti i modelli acustici personalizzati in inglese (Stati Uniti) che appartengono alle credenziali specificate:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations?language=en-US"
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
      "versions": [
        "en-US_BroadbandModel.v07-06082016.06202016",
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model one",
      "description": "Example custom acoustic model",
      "base_model_name": "en-US_BroadbandModel",
      "status": "pending",
      "progress": 0
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732fae4829",
      "created": "2016-06-01T18:51:37.291Z",
      "updated": "2016-06-01T19:21:06.825Z",
      "language": "en-US",
      "versions": [
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model two",
      "description": "Example custom acoustic model two",
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
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "versions": [
    "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model one",
  "description": "Example custom acoustic model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

## Reimpostazione di un modello acustico personalizzato
{: #resetModel-acoustic}

Utilizza il metodo `POST /v1/acoustic_customizations/{customization_id}/reset` per reimpostare un modello acustico personalizzato. La reimpostazione di un modello personalizzato rimuove tutte le risorse audio dal modello, inizializzando il modello allo stato che aveva al momento della sua creazione. Il metodo non elimina il modello stesso o i metadati quali il suo nome e la sua lingua. Tuttavia, quando reimposti un modello, le sue risorse audio vengono rimosse e devono essere ricreate.

### Richiesta di esempio
{: #resetExample-acoustic}

Il seguente esempio reimposta il modello acustico personalizzato con l'ID di personalizzazione specificato:

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/reset"
```
{: pre}

## Eliminazione di un modello acustico personalizzato
{: #deleteModel-acoustic}

Utilizza il metodo `DELETE /v1/acoustic_customizations/{customization_id}` per eliminare un modello acustico personalizzato di cui non hai più bisogno. Il metodo elimina tutto l'audio associato al modello personalizzato e il modello stesso. Utilizza questo metodo con cautela: non è possibile recuperare un modello personalizzato e i relativi dati dopo aver eliminato il modello.

### Richiesta di esempio
{: #deleteExample-acoustic}

Il seguente esempio elimina il modello acustico personalizzato con l'ID di personalizzazione specificato:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}
