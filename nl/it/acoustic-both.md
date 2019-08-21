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

# Utilizzo combinato dei modelli acustici e di lingua personalizzati
{: #useBoth}

Puoi migliorare l'accuratezza del riconoscimento vocale utilizzando dei modelli acustici e di lingua personalizzati complementari. Puoi utilizzare entrambi i tipi di modello durante l'addestramento del tuo modello acustico e durante il riconoscimento vocale. Entrambi i modelli devono appartenere alla stessa istanza del servizio ed entrambi devono essere basati sullo stesso modello di lingua di base.
{: shortdesc}

L'utilizzo di un modello acustico personalizzato da solo o con un modello di lingua personalizzato sul quale non è stato addestrato può comunque essere utile. Se il modello acustico personalizzato è stato addestrato sulle caratteristiche acustiche che corrispondono all'audio che viene trascritto, può comunque migliorare la qualità della trascrizione.

## Addestramento di un modello acustico personalizzato con un modello di lingua personalizzato
{: #useBothTrain}

L'addestramento di un modello acustico personalizzato con i soli dati audio viene definito *addestramento senza supervisione*. L'utilizzo di un modello di lingua personalizzato durante l'addestramento viene definito *addestramento con supervisione moderata*. L'addestramento con supervisione moderata può migliorare l'efficacia del tuo modello acustico personalizzato.

Utilizza l'addestramento con supervisione moderata per addestrare un modello acustico personalizzato con un modello di lingua personalizzato nei seguenti casi:

-   Il modello di lingua personalizzato è basato sulle trascrizioni (testo verbatim) dei file audio che hai aggiunto al modello acustico personalizzato.

    Poiché le trascrizioni contengono il contenuto esatto dell'audio, l'addestramento con un modello di lingua personalizzato basato sulle trascrizioni può produrre risultati migliori. Il servizio può analizzare il contenuto delle trascrizioni nel contesto ed estrarre parole OOV e n-grammi che possono aiutare a utilizzare con la massima efficienza il tuo audio. Questo è particolarmente vero se i tuoi dati audio sono lunghi meno di un'ora.

    La trascrizione dei dati audio non è strettamente necessaria, ma se hai delle trascrizioni dell'audio, queste possono migliorare la qualità del modello acustico personalizzato. Le trascrizioni sono particolarmente utili se l'audio contiene molte parole OOV.
-   Il modello di lingua personalizzato è basato sui corpora (file di testo) o su un elenco di parole rilevanti per il contenuto dei file audio.

    Se il tuo audio contiene parole specifiche per il dominio che non si trovano nel vocabolario di base del servizio, la personalizzazione del modello acustico da sola non produce quelle parole durante la trascrizione. La personalizzazione del modello di lingua è l'unico modo per espandere il vocabolario di base del servizio. Se non hai delle trascrizioni, esegui l'addestramento con un modello di lingua personalizzato che include parole OOV dallo stesso dominio dei tuoi dati audio. Anche l'addestramento con un modello di lingua personalizzato che include un elenco delle parole OOV utilizzate nell'audio può rivelarsi utile.

    Ad esempio, supponiamo che tu stia creando un modello acustico personalizzato basato sull'audio di call center per un prodotto specifico. Puoi addestrare il modello acustico personalizzato con un modello di lingua personalizzato che è basato sulle trascrizioni di chiamate correlate o che include nomi di prodotti specifici gestiti dal call center.

Per utilizzare una trascrizione o un elenco di parole, devi prima creare un modello di lingua personalizzato che contenga questi dati testuali. Per addestrare un modello acustico personalizzato con un modello di lingua personalizzato, entrambi i modelli personalizzati devono essere basati sulla stessa versione dello stesso modello di base. Se viene resa disponibile una nuova versione del modello di base, devi eseguire l'upgrade di entrambi i modelli alla stessa versione del modello di base per consentire l'addestramento.

Utilizza il parametro di query facoltativo `custom_language_model_id` del metodo `POST /v1/acoustic_customizations/{customization_id}/train` per addestrare il tuo modello acustico personalizzato con un modello di lingua personalizzato. Passa il GUID del modello acustico con il parametro `customization_id` e il GUID del modello di lingua personalizzato con il parametro `custom_language_model_id`. Entrambi i modelli devono appartenere alle credenziali passate con la richiesta.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train?custom_language_model_id={customization_id}"
```
{: pre}

## Utilizzo dei modelli di lingua e acustici personalizzati durante il riconoscimento vocale
{: #useBothRecognize}

Puoi specificare sia un modello di lingua personalizzato che un modello acustico personalizzato con qualsiasi richiesta di riconoscimento. Se un modello di lingua personalizzato contiene parole OOV dal dominio dell'audio che viene riconosciuto, puoi utilizzarlo con un modello acustico personalizzato durante il riconoscimento vocale per migliorare l'accuratezza della trascrizione.

L'utilizzo di un modello di lingua personalizzato può migliorare l'accuratezza della trascrizione indipendentemente dal fatto che tu abbia addestrato il modello acustico personalizzato con il modello di lingua personalizzato:

-   L'utilizzo dei modelli di lingua e acustici personalizzati durante l'addestramento migliora la qualità del modello acustico personalizzato.
-   L'utilizzo di entrambi i tipi di modello durante il riconoscimento vocale migliora la qualità della trascrizione.

Se un modello di lingua personalizzato include delle grammatiche, puoi anche utilizzare tale modello e una delle sue grammatiche con un modello acustico personalizzato durante il riconoscimento vocale.

Il seguente esempio passa entrambi i tipi di modello al metodo HTTP `POST /v1/recognize`. Passa il GUID del modello acustico personalizzato con il parametro `acoustic_customization_id` e il GUID del modello di lingua personalizzato con il parametro `language_customization_id`. Entrambi i modelli devono appartenere alle credenziali passate con la richiesta ed entrambi devono essere basati sullo stesso modello di base (ad esempio, `en-US_BroadbandModel`).

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}&language_customization_id={customization_id}"
```
{: pre}

Per una richiesta HTTP asincrona, specifichi i parametri quando crei il lavoro asincrono. Per WebSocket, passi i parametri quando stabilisci la connessione.
