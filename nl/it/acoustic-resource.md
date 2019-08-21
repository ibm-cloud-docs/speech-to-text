---

copyright:
  years: 2017, 2019
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

# Utilizzo delle risorse audio
{: #audioResources}

Puoi aggiungere singoli file audio o file di archivio che contengono più file audio a un modello acustico personalizzato. Il metodo consigliato per aggiungere le risorse audio consiste nell'aggiungere i file di archivio. La creazione e l'aggiunta di un singolo file di archivio è notevolmente più efficiente rispetto all'aggiunta di più file audio singolarmente. Puoi anche inoltrare richieste per aggiungere più risorse audio contemporaneamente.
{: shortdesc}

## Aggiunta di una risorsa audio
{: #addAudioResource}

Utilizza il metodo `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` per aggiungere entrambi i tipi di risorsa audio a un modello acustico personalizzato. Passa la risorsa audio come corpo della richiesta e includi i seguenti parametri:

-   Il parametro di percorso `customization_id` per specificare l'ID di personalizzazione del modello.
-   Il parametro di percorso `audio_name` per specificare un nome per la risorsa audio.
    -   Utilizza un nome localizzato che corrisponda alla lingua del modello personalizzato e che rifletta il contenuto della risorsa.
    -   Includi un massimo di 128 caratteri nel nome.
    -   Non utilizzare caratteri che devono essere codificati in URL. Ad esempio, non utilizzare spazi, barre, barre rovesciate, due punti, e commerciali, virgolette doppie, segni più, segni di uguale, punti interrogativi e così via nel nome. (Il servizio non impedisce l'utilizzo di questi caratteri. Tuttavia, poiché devono essere codificati in URL ovunque ricorrano, il loro uso è fortemente sconsigliato.)
    -   Non utilizzare il nome di una risorsa audio che è già stata aggiunta al modello personalizzato.

Quando aggiorni le risorse audio di un modello, devi addestrare il modello per rendere effettive le modifiche durante la trascrizione. Per ulteriori informazioni, vedi [Addestra il modello acustico personalizzato](/docs/services/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic).

## Aggiunta di un file audio
{: #addAudioType}

Per aggiungere un singolo file audio a un modello acustico personalizzato, specifica il formato (tipo MIME) dell'audio con l'intestazione `Content-Type`. Puoi aggiungere l'audio con qualsiasi formato supportato per le richieste di riconoscimento. Includi i parametri `rate`, `channels` e `endianness` con la specifica dei formati che li richiedono. Per ulteriori informazioni sui formati audio supportati, vedi [Formati audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).

La specifica `application/octet-stream` per un formato audio non è supportata per le risorse audio.
{: note}

Il seguente esempio tratto da [Aggiungi l'audio al modello acustico personalizzato](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio) aggiunge un file `audio/wav`:

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @audio1.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
```
{: pre}

## Aggiunta di un file di archivio
{: #addArchiveType}

Il metodo preferito per aggiungere l'audio a un modello acustico personalizzato consiste nell'aggiungere un file di archivio che include più file audio. Puoi aggiungere i seguenti tipi di file di archivio specificando il tipo di archivio con l'intestazione della richiesta `Content-Type`:

-   Un file **.zip** specificando `application/zip`
-   Un file **.tar.gz** specificando `application/gzip`

Potresti anche dover specificare l'intestazione `Contained-Content-Type` a seconda del formato dei file che stai aggiungendo:

-   Per i file audio di tipo `audio/alaw`, `audio/basic`, `audio/l16` o `audio/mulaw`, devi utilizzare l'intestazione `Contained-Content-Type` per specificare il formato dei file audio. Includi i parametri `rate`, `channels` e `endianness` dove necessario. In questo caso, tutti i file audio contenuti nel file di archivio devono avere lo stesso formato audio.
-   Per i file audio di tutti gli altri tipi, puoi omettere l'intestazione `Contained-Content-Type`. In questo caso, i file audio contenuti nel file di archivio possono avere qualsiasi formato non elencato nel punto precedente. Non è necessario che abbiano lo stesso formato.

Non utilizzare l'intestazione `Contained-Content-Type` quando aggiungi una risorsa di tipo audio.
{: note}

Il nome di un file audio contenuto in una risorsa di tipo archivio può includere un massimo di 128 caratteri. Ciò include l'estensione del file e tutti gli elementi del nome (ad esempio, le barre).

Il seguente esempio tratto da [Aggiungi l'audio al modello acustico personalizzato](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio) aggiunge un file `application/zip` che contiene file audio in formato `audio/l16` campionati a 16 kHz:

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/zip"
--header "Contained-Content-Type: audio/l16;rate=16000"
--data-binary @audio2.zip
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
```
{: pre}

## Linee guida per l'aggiunta di risorse audio
{: #audioGuidelines}

Il miglioramento dell'accuratezza del riconoscimento offerto dall'utilizzo di un modello acustico personalizzato dipende da una serie di fattori. Questi fattori includono la quantità di dati audio contenuti nel modello acustico personalizzato e la somiglianza dei dati con l'audio che viene trascritto. Il miglioramento dipende anche dal fatto che il modello acustico personalizzato sia addestrato o meno con un modello di lingua personalizzato corrispondente.

Segui queste linee guida quando aggiungi risorse audio a un modello acustico personalizzato:

-   Aggiungi almeno 10 minuti e non più di 200 ore di audio a un modello acustico personalizzato. L'audio deve includere il discorso, non il silenzio.

    La qualità dell'audio fa la differenza quando devi determinare la quantità da aggiungere. Quanto meglio l'audio del modello riflette le caratteristiche dell'audio da riconoscere, migliore sarà la qualità del modello personalizzato per il riconoscimento vocale. Se l'audio è di buona qualità, aggiungerne di più può migliorare l'accuratezza della trascrizione. Ma aggiungere anche da cinque a dieci ore di audio di buona qualità può fare la differenza.
-   Aggiungi risorse audio non superiori a 100 MB. Tutte le risorse di tipo audio e di tipo archivio sono limitate a una dimensione massima di 100 MB.

    Per massimizzare la quantità di audio che puoi aggiungere con una singola risorsa, considera l'utilizzo di un formato audio che offra la compressione. Per ulteriori informazioni, vedi [Limiti e compressione dei dati](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#limits).
-   Dividi i file audio di grandi dimensioni in diversi file più piccoli. Assicurati di suddividere l'audio tra le parole, in punti di silenzio.

    Poiché puoi inoltrare più richieste simultanee per aggiungere risorse audio diverse, puoi aggiungere contemporaneamente file più piccoli. Questo approccio parallelo all'aggiunta di risorse audio può accelerare l'analisi del tuo audio da parte del servizio.
-   Aggiungi contenuti audio che riflettano le condizioni del canale acustico dell'audio che intendi trascrivere. Ad esempio, se la tua applicazione si occupa dell'audio con rumore di fondo proveniente da un veicolo in movimento, utilizza lo stesso tipo di dati per creare il modello personalizzato.
-   Assicurati che la frequenza di campionamento di un file audio corrisponda alla frequenza di campionamento del modello di base per il modello acustico personalizzato:
    -   Per i modelli a banda larga, la frequenza di campionamento deve essere di almeno 16 kHz (16.000 campioni al secondo).
    -   Per i modelli a banda stretta, la frequenza di campionamento deve essere di almeno 8 kHz (8000 campioni al secondo).

    Se la frequenza di campionamento dell'audio è superiore quella minima richiesta, il servizio sottocampiona l'audio alla frequenza appropriata. Se la frequenza di campionamento dell'audio è inferiore alla frequenza minima richiesta, il servizio etichetta il file audio come `invalid`. Se qualsiasi file audio contenuto in un file di archivio non è valido, il servizio considera come non valido l'intero archivio.
-   Crea un modello di lingua personalizzato da utilizzare con il tuo modello acustico personalizzato nei seguenti casi:
    -   Se il tuo audio dura meno di un'ora, crea un modello di lingua personalizzato basato sulle trascrizioni dell'audio per ottenere i risultati migliori.
    -   Se il tuo audio è specifico per il dominio e contiene parole univoche che non si trovano nel vocabolario di base del servizio, utilizza la personalizzazione del modello di lingua per espandere il vocabolario di base del servizio. La personalizzazione del modello acustico da sola non può produrre quelle parole durante la trascrizione.

    Per ulteriori informazioni, vedi [Utilizzo combinato dei modelli acustici e di lingua personalizzati](/docs/services/speech-to-text?topic=speech-to-text-useBoth).
