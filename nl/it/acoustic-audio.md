---

copyright:
  years: 2019
lastupdated: "2019-06-19"

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

# Gestione delle risorse audio
{: #manageAudio}

L'interfaccia di personalizzazione include il metodo `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`, che viene utilizzato per aggiungere una risorsa audio a un modello acustico personalizzato. Per ulteriori informazioni, vedi [Aggiungi l'audio al modello acustico personalizzato](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio). L'interfaccia include anche i seguenti metodi per elencare ed eliminare le risorse audio per un modello acustico personalizzato.
{: shortdesc}

## Elenco delle risorse audio per un modello acustico personalizzato
{: #listAudio}

L'interfaccia di personalizzazione fornisce due metodi per elencare le informazioni relative alle risorse audio di un modello acustico personalizzato:

-   Il metodo `GET /v1/acoustic_customizations/{customization_id}/audio` elenca le informazioni su tutte le risorse audio che sono state aggiunte a un modello personalizzato.
-   Il metodo `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` elenca le informazioni su una risorsa audio specificata per un modello personalizzato.

Entrambi i metodi restituiscono il nome (`name`) della risorsa audio oltre alle seguenti informazioni aggiuntive:

-   `duration` è la lunghezza in secondi dell'audio. Per un file di archivio, la figura rappresenta la durata cumulativa di tutti i file audio contenuti nell'archivio.
-   `details` identifica il tipo di risorsa: `audio` o `archive`. (Il tipo è `undetermined` se il servizio non può convalidare la risorsa, probabilmente perché l'utente ha erroneamente passato un file che non contiene audio.) Per un file audio, i dettagli includono i valori `codec` e `frequency` dell'audio. Per un file di archivio, includono il suo tipo di `compression`.

Inoltre, l'elenco di tutte le risorse audio per un modello restituisce il valore di `total_minutes_of_audio` sommato su tutte le risorse audio valide per il modello. Puoi utilizzare questo valore per determinare se il modello personalizzato ha sufficiente o troppo audio per iniziare l'addestramento.

I metodi elencano anche lo stato dei dati audio. Lo stato è importante per controllare l'analisi dei file audio da parte del servizio in risposta a una richiesta di aggiunta a un modello personalizzato:

-   `ok` indica che il servizio ha analizzato correttamente i dati audio. I dati possono essere utilizzati per addestrare il modello personalizzato.
-   `being_processed` indica che il servizio sta ancora analizzando i dati audio. Il servizio non può accettare richieste per aggiungere nuovo audio o per addestrare il modello personalizzato finché l'analisi non sarà terminata.
-   `invalid` indica che i dati audio non sono validi per l'addestramento del modello (probabilmente perché hanno il formato o la frequenza di campionamento errati o perché sono danneggiati).

### Richiesta di esempio: elenca tutte le risorse audio
{: #listExample-audio}

Il seguente esempio elenca tutte le risorse audio per il modello acustico personalizzato con l'ID di personalizzazione specificato. Il modello acustico ha tre risorse audio. Il servizio ha analizzato correttamente `audio1` e `audio2`; sta ancora analizzando `audio3`.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio"
```
{: pre}

```javascript
{
  "total_minutes_of_audio": 11.45,
  "audio": [
    {
      "duration": 131,
      "name": "audio1",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 22050
      }
      "status": "ok"
    },
    {
      "duration": 556,
      "name": "audio2",
      "details": {
        "type": "archive",
        "compression": "zip"
      },
      "status": "ok"
    },
    {
      "duration": 0,
      "name": "audio3",
      "details": {},
      "status": "being_processed"
    }
  ]
}
```
{: codeblock}

### Richiesta di esempio: ottieni informazioni per una risorsa di tipo audio
{: #getExampleAudio}

Il seguente esempio restituisce informazioni sulla risorsa di tipo audio denominata `audio1`. La risorsa è lunga 131 secondi ed è codificata con il codec `pcm_s16le`. È stata aggiunta correttamente al modello.

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

### Richiesta di esempio: ottieni informazioni per una risorsa di tipo archivio
{: #getExampleArchive}

Il seguente esempio restituisce informazioni sulla risorsa di tipo archivio denominata `audio2`. La risorsa è un file **.zip** che contiene più di 9 minuti di audio. Anche questa è stata aggiunta correttamente al modello. Come mostra l'esempio, l'esecuzione di query nelle informazioni su una risorsa di tipo archivio fornisce anche informazioni sui file che contiene.

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
  "audio": [
    {
      "duration": 121,
      "name": "audio-file1.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    {
      "duration": 133,
      "name": "audio-file2.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    . . .
  ]
}
```
{: codeblock}

## Eliminazione di una risorsa audio da un modello acustico personalizzato
{: #deleteAudio}

Utilizza il metodo `DELETE /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` per rimuovere una risorsa audio esistente da un modello acustico personalizzato. Quando elimini una risorsa audio di tipo archivio, il servizio rimuove l'intero archivio di file. Il servizio non consente l'eliminazione di singoli file da una risorsa di archivio.

La rimozione di una risorsa audio non influisce sul modello personalizzato finché non addestri il modello sui relativi dati aggiornati utilizzando il metodo `POST /v1/acoustic_customizations/{customization_id}/train`. Se hai addestrato correttamente il modello sulla risorsa, finché non ripeti l'addestramento del modello, i dati audio esistenti continuano a essere utilizzati per il riconoscimento vocale.

### Richiesta di esempio
{: #deleteExample-audio}

Il seguente metodo elimina la risorsa audio denominata `audio3` dal modello personalizzato con l'ID di personalizzazione specificato:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio3"
```
{: pre}
