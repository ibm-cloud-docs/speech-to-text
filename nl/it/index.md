---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-30"

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

# Informazioni
{: #about}

**Aggiornamento del servizio:** *il servizio {{site.data.keyword.speechtotextshort}} è stato aggiornato il 30 luglio 2019. Il servizio ora offre modelli beta per altri cinque dialetti spagnoli: argentino, cileno, colombiano, messicano e peruviano. Per ulteriori informazioni sui nuovi dialetti spagnoli, vedi l'[aggiornamento del servizio del 30 luglio 2019](/docs/services/speech-to-text?topic=speech-to-text-release-notes#July2019) nelle note sulla release.*

Il servizio {{site.data.keyword.speechtotextfull}} fornisce funzionalità di trascrizione vocale per le tue applicazioni. Il servizio sfrutta il machine learning per combinare la conoscenza della grammatica, la struttura della lingua e la composizione dei segnali audio e vocali per trascrivere accuratamente la voce umana. Aggiorna e perfeziona continuamente la sua trascrizione man mano che riceve più discorsi.
{: shortdesc}

Il servizio fornisce varie interfacce che lo rendono adatto a qualsiasi applicazione in cui il discorso è l'input e una trascrizione testuale è l'output. Le applicazioni di esempio includono

-   Controllo vocale di applicazioni, dispositivi integrati e accessori per veicoli
-   Trascrizione di riunioni e conference call
-   Dettatura di messaggi e-mail e note

Il servizio è ideale per i clienti che devono estrarre trascrizioni vocali di alta qualità dall'audio dei call center. I clienti in settori come servizi finanziari, assistenza sanitaria, assicurazioni e telecomunicazioni possono sviluppare applicazioni native del cloud per l'assistenza clienti, la voce dei clienti, l'assistenza degli agenti e altre soluzioni.

## Interfacce supportate
{: #interfaces}

Il servizio {{site.data.keyword.speechtotextshort}} offre tre interfacce per il riconoscimento vocale:

-   Un'[interfaccia WebSocket](/docs/services/speech-to-text?topic=speech-to-text-websockets) per stabilire connessioni persistenti, full duplex e a bassa latenza con il servizio. Puoi passare un massimo di 100 MB di dati audio al servizio con una singola richiesta.
-   Un'[interfaccia HTTP sincrona](/docs/services/speech-to-text?topic=speech-to-text-http) per le chiamate HTTP di base al servizio. Puoi passare un massimo di 100 MB di dati audio con una richiesta.
-   Un'[interfaccia HTTP asincrona](/docs/services/speech-to-text?topic=speech-to-text-async) per chiamate non bloccanti al servizio. Puoi passare fino a 1 GB di dati audio con una richiesta.

Il servizio fornisce anche un'[interfaccia di personalizzazione](/docs/services/speech-to-text?topic=speech-to-text-customization) che puoi utilizzare per ottimizzare il riconoscimento vocale per i tuoi requisiti di lingua e acustici. Puoi espandere il vocabolario di un modello con la terminologia specifica per il dominio o adattare un modello per le caratteristiche acustiche del tuo audio. Puoi anche aggiungere delle [grammatiche](/docs/services/speech-to-text?topic=speech-to-text-grammars) per limitare le frasi che il servizio può riconoscere.

-   Per una descrizione di alto livello dello sviluppo di applicazioni con il servizio, vedi [Panoramica per gli sviluppatori](/docs/services/speech-to-text?topic=speech-to-text-developerOverview).
-   Per degli esempi di richieste di riconoscimento vocale di base con ciascuna delle interfacce del servizio, vedi [Effettuazione di una richiesta di riconoscimento](/docs/services/speech-to-text?topic=speech-to-text-basic-request).

Sono disponibili degli SDK in molti linguaggi di programmazione per semplificare l'utilizzo del servizio. Per ulteriori informazioni, vedi la [Guida di riferimento API](https://{DomainName}/apidocs/speech-to-text){: external}.

## Funzioni di input
{: #inputFeatures}

Le interfacce del servizio condividono funzioni di input comuni per la trascrizione da voce a testo:

-   [Formati audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats) - Puoi trascrivere l'audio Ogg o Web Media (WebM) con il codec Opus o Vorbis, MP3 (o MPEG), WAV (Waveform Audio File Format), FLAC (Free Lossless Audio Codec), PCM (Pulse-Code Modulation) lineare a 16 bit, G.729, A-Law, mu-law (o u-law) e audio di base. Utilizzando un formato che supporta la compressione, puoi massimizzare la quantità di dati audio che puoi inviare con una richiesta.
-   [Lingue e modelli](/docs/services/speech-to-text?topic=speech-to-text-models) - Per la maggior parte delle lingue, puoi trascrivere l'audio utilizzando modelli a banda larga o a banda stretta. Utilizza la banda larga per l'audio campionato a una frequenza minima di 16 kHz. Utilizza la banda stretta per l'audio campionato a una frequenza minima di 8 kHz.
-   [Trasmissione audio](/docs/services/speech-to-text?topic=speech-to-text-input#transmission) - Puoi passare l'audio come flusso continuo di porzioni di dati o come fornitura unica che passa tutti i dati in una volta. Con l'invio in streaming, il servizio applica dei [timeout](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts) di inattività e di sessione.

## Funzioni di output
{: #outputFeatures}

Le interfacce supportano anche le seguenti funzioni di output comuni:

-   Le [etichette del parlante](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels) riconoscono parlanti diversi dall'audio in inglese americano, inglese britannico, spagnolo o giapponese. La trascrizione etichetta i contributi di ciascun parlante a una conversazione tra più partecipanti. (Funzionalità beta.)
-   L'[individuazione di parole chiave](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting) identifica le frasi pronunciate che corrispondono alle stringhe di parole chiave specificate con un livello di attendibilità definito dall'utente. L'individuazione di parole chiave è particolarmente utile quando le singole frasi dall'audio sono più importanti della trascrizione completa. Ad esempio, un sistema di supporto clienti potrebbe identificare le parole chiave per determinare come instradare le richieste degli utenti.
-   I [risultati provvisori](/docs/services/speech-to-text?topic=speech-to-text-output#interim) restituiscono ipotesi progressive durante lo svolgimento della trascrizione. Il servizio restituisce i risultati finali al termine della trascrizione. I risultati provvisori sono disponibili solo con l'interfaccia WebSocket.
-   Il [numero massimo di alternative](/docs/services/speech-to-text?topic=speech-to-text-output#max_alternatives) fornisce possibili trascrizioni alternative. Il servizio indica i risultati finali in cui ha la massima attendibilità.
-   Le [alternative alle parole](/docs/services/speech-to-text?topic=speech-to-text-output#word_alternatives) richiedono parole alternative che siano acusticamente simili alle parole di una trascrizione.
-   L'[attendibilità delle parole](/docs/services/speech-to-text?topic=speech-to-text-output#word_confidence) restituisce i livelli di attendibilità per ogni parola di una trascrizione.
-   Le [date/ore delle parole](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps) restituiscono le date e ore per l'inizio e la fine di ogni parola di una trascrizione.
-   La [formattazione intelligente](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting) converte date, ore, numeri, valori di valuta, numeri di telefono e indirizzi Internet in formati convenzionali più leggibili nelle trascrizioni finali. Per l'inglese americano, puoi anche fornire frasi di parole chiave per includere determinati simboli di punteggiatura nelle trascrizioni finali. La formattazione intelligente è supportata per l'audio in inglese americano, giapponese e spagnolo. (Funzionalità beta.)
-   L'[oscuramento numerico](/docs/services/speech-to-text?topic=speech-to-text-output#redaction) oscura, o maschera, i dati numerici da una trascrizione finale. L'oscuramento ha lo scopo di rimuovere le informazioni personali sensibili, come i numeri delle carte di credito, dalle trascrizioni. La funzione è supportata per l'audio in inglese americano, giapponese e coreano. (Funzionalità beta.)
-   Il [filtro di contenuto volgare](/docs/services/speech-to-text?topic=speech-to-text-output#profanity_filter) censura il contenuto volgare nelle trascrizioni in inglese americano.
-   Le [metriche di elaborazione](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics) forniscono informazioni temporali dettagliate sull'analisi dell'audio di input da parte del servizio. 
-   Le [metriche audio](/docs/services/speech-to-text?topic=speech-to-text-metrics#audio_metrics) forniscono informazioni dettagliate sulle caratteristiche di segnale dell'audio di input. 

## Supporto delle lingue
{: #languages}

Il servizio offre modelli per le seguenti lingue e dialetti:

-   Arabo (standard moderno)
-   Portoghese (Brasile)
-   Cinese (Mandarino)
-   Inglese (Regno Unito e Stati Uniti)
-   Francese
-   Tedesco
-   Giapponese
-   Coreano
-   Spagnolo (argentino, castigliano, cileno, colombiano, messicano e peruviano)

Il servizio non supporta tutte le funzioni per tutte le lingue. Inoltre, supporta alcune funzioni come generalmente disponibili (GA) per l'uso in produzione e altre come offerte beta per diverse lingue.

-   Il dialetto castigliano spagnolo è generalmente disponibile. Gli altri cinque dialetti spagnoli sono in beta.
-   Le interfacce WebSocket e HTTP sono generalmente disponibili per tutte le lingue.
-   Il servizio offre modelli a banda larga, modelli a banda stretta o entrambi per diverse lingue. Per ulteriori informazioni,
      vedi
      [Lingue e modelli](/docs/services/speech-to-text?topic=speech-to-text-models).
-   Alcune funzioni di riconoscimento vocale sono disponibili solo per determinate lingue. Per ulteriori informazioni, vedi il [Riepilogo dei parametri](/docs/services/speech-to-text?topic=speech-to-text-summary).
-   L'interfaccia di personalizzazione del modello di lingua è generalmente disponibile per la maggior parte delle lingue. L'interfaccia di personalizzazione del modello acustico è disponibile come funzionalità beta per tutte le lingue. Per ulteriori informazioni, vedi [Supporto delle lingue per la personalizzazione](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport).

## Prezzi
{: #pricing-index}

Per ulteriori informazioni sui piani dei prezzi per il servizio, vedi il servizio {{site.data.keyword.speechtotextshort}} nel [Catalogo {{site.data.keyword.cloud}}](https://{DomainName}/catalog/services/speech-to-text){: external}. Per le risposte alle domande relative ai prezzi e gli esempi sui costi di utilizzo mensili, vedi le [Domande frequenti sui prezzi](/docs/services/speech-to-text?topic=speech-to-text-faq-pricing).

## Prova il servizio
{: #tryOut}

Per esempi del servizio {{site.data.keyword.speechtotextshort}} in azione, vedi

-   Una [demo rapida](https://speech-to-text-demo.ng.bluemix.net/){: external} che trascrive il testo dall'immissione audio in streaming o da un file che carichi.
-   Applicazioni nei [Kit starter](http://www.ibm.com/watson/developercloud/starter-kits.html){: external} {{site.data.keyword.ibmwatson}} che mostrano il servizio.
-   Il post del blog {{site.data.keyword.watson}} [Getting robots to listen: Using {{site.data.keyword.watson}}'s {{site.data.keyword.speechtotextshort}} service](https://www.ibm.com/blogs/watson/2016/07/getting-robots-listen-using-watsons-speech-text-service/){: external} che mostra come utilizzare l'interfaccia WebSocket del servizio con Python per estrarre il discorso dall'audio. Il post fornisce un'esercitazione approfondita che descrive il codice e i passi coinvolti.
