---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-03"

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

# Informazioni
{: #about}

**Aggiornamento del servizio:** *il servizio {{site.data.keyword.speechtotextshort}} è stato aggiornato il 3 aprile 2019. Il servizio ora ti consente di aggiungere un massimo di 200 ore di audio a un modello acustico personalizzato. Per ulteriori informazioni, vedi l'[aggiornamento del servizio del 3 aprile 2019](/docs/services/speech-to-text/release-notes.html#April2019) nelle note sulla release*.

Il servizio {{site.data.keyword.speechtotextfull}} fornisce funzionalità di trascrizione vocale per le tue applicazioni. Il servizio sfrutta il machine learning per combinare la conoscenza della grammatica, la struttura della lingua e la composizione dei segnali audio e vocali per trascrivere accuratamente la voce umana. Aggiorna e perfeziona continuamente la sua trascrizione man mano che riceve più discorsi.
{: shortdesc}

Il servizio fornisce varie interfacce che lo rendono adatto a qualsiasi applicazione in cui il discorso è l'input e una trascrizione testuale è l'output. Le applicazioni di esempio includono

-   Controllo vocale di applicazioni, dispositivi integrati e accessori per veicoli
-   Trascrizione di riunioni e conference call
-   Dettatura di messaggi e-mail e note

## Interfacce supportate
{: #interfaces}

Il servizio {{site.data.keyword.speechtotextshort}} offre tre interfacce per il riconoscimento vocale:

-   Un'[interfaccia WebSocket](/docs/services/speech-to-text/websockets.html) per stabilire connessioni persistenti, full duplex e a bassa latenza con il servizio. Puoi passare un massimo di 100 MB di dati audio al servizio con una singola richiesta.
-   Un'[interfaccia HTTP sincrona](/docs/services/speech-to-text/http.html) per le chiamate HTTP di base al servizio. Puoi passare un massimo di 100 MB di dati audio con una richiesta.
-   Un'[interfaccia HTTP asincrona](/docs/services/speech-to-text/async.html) per chiamate non bloccanti al servizio. Puoi passare fino a 1 GB di dati audio con una richiesta.

Il servizio fornisce anche un'[interfaccia di personalizzazione](/docs/services/speech-to-text/custom.html) che puoi utilizzare per ottimizzare il riconoscimento vocale per i tuoi requisiti di lingua e acustici. Puoi espandere il vocabolario di un modello con la terminologia specifica per il dominio o adattare un modello per le caratteristiche acustiche del tuo audio. Puoi anche aggiungere delle [grammatiche](/docs/services/speech-to-text/grammar.html) per limitare le frasi che il servizio può riconoscere.

-   Per una descrizione di alto livello dello sviluppo di applicazioni con il servizio, vedi [Panoramica per gli sviluppatori](/docs/services/speech-to-text/developer-overview.html).
-   Per degli esempi di richieste di riconoscimento vocale di base con ciascuna delle interfacce del servizio, vedi [Effettuazione di una richiesta di riconoscimento](/docs/services/speech-to-text/basic-request.html).

Sono disponibili degli SDK in molti linguaggi di programmazione per semplificare l'utilizzo del servizio. Per ulteriori informazioni, vedi la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

## Funzioni di input
{: #inputFeatures}

Le interfacce del servizio condividono funzioni di input comuni per la trascrizione da voce a testo:

-   [Formati audio](/docs/services/speech-to-text/audio-formats.html) - Puoi trascrivere l'audio Ogg o Web Media (WebM) con il codec Opus o Vorbis, MP3 (o MPEG), WAV (Waveform Audio File Format), FLAC (Free Lossless Audio Codec), PCM (Pulse-Code Modulation) lineare a 16 bit, G.729, A-Law, mu-law (o u-law) e audio di base. Utilizzando un formato che supporta la compressione, puoi massimizzare la quantità di dati audio che puoi inviare con una richiesta.
-   [Lingue e modelli](/docs/services/speech-to-text/models.html) - Per la maggior parte delle lingue, puoi trascrivere l'audio utilizzando modelli a banda larga o a banda stretta. Utilizza la banda larga per l'audio campionato a una frequenza minima di 16 kHz. Utilizza la banda stretta per l'audio campionato a una frequenza minima di 8 kHz.
-   [Trasmissione audio](/docs/services/speech-to-text/input.html#transmission) - Puoi passare l'audio come flusso continuo di porzioni di dati o come fornitura unica che passa tutti i dati in una volta. Con l'invio in streaming, il servizio applica dei [timeout](/docs/services/speech-to-text/input.html#timeouts) di inattività e di sessione.

## Funzioni di output
{: #outputFeatures}

Le interfacce supportano anche le seguenti funzioni di output comuni:

-   Le [etichette del parlante](/docs/services/speech-to-text/output.html#speaker_labels) riconoscono parlanti diversi dall'audio in inglese americano, inglese britannico, spagnolo o giapponese. La trascrizione etichetta i contributi di ciascun parlante a una conversazione tra più partecipanti. (Funzionalità beta.)
-   L'[individuazione di parole chiave](/docs/services/speech-to-text/output.html#keyword_spotting) identifica le frasi pronunciate che corrispondono alle stringhe di parole chiave specificate con un livello di attendibilità definito dall'utente. L'individuazione di parole chiave è particolarmente utile quando le singole frasi dall'audio sono più importanti della trascrizione completa. Ad esempio, un sistema di supporto clienti potrebbe identificare le parole chiave per determinare come instradare le richieste degli utenti.
-   I [risultati provvisori](/docs/services/speech-to-text/output.html#interim) restituiscono ipotesi progressive durante lo svolgimento della trascrizione. Il servizio restituisce i risultati finali al termine della trascrizione. I risultati provvisori sono disponibili solo con l'interfaccia WebSocket.
-   Il [numero massimo di alternative](/docs/services/speech-to-text/output.html#max_alternatives) fornisce possibili trascrizioni alternative. Il servizio indica i risultati finali in cui ha la massima attendibilità.
-   Le [alternative alle parole](/docs/services/speech-to-text/output.html#word_alternatives) richiedono parole alternative che siano acusticamente simili alle parole di una trascrizione.
-   L'[attendibilità delle parole](/docs/services/speech-to-text/output.html#word_confidence) restituisce i livelli di attendibilità per ogni parola di una trascrizione. 
-   Le [date/ore delle parole](/docs/services/speech-to-text/output.html#word_timestamps) restituiscono le date e ore per l'inizio e la fine di ogni parola di una trascrizione.
-   La [formattazione intelligente](/docs/services/speech-to-text/output.html#smart_formatting) converte date, ore, numeri, valori di valuta, numeri di telefono e indirizzi Internet in formati convenzionali più leggibili nelle trascrizioni finali. Per l'inglese americano, puoi anche fornire frasi di parole chiave per includere determinati simboli di punteggiatura nelle trascrizioni finali. La formattazione intelligente è supportata per l'audio in inglese americano, giapponese e spagnolo. (Funzionalità beta.)
-   L'[oscuramento numerico](/docs/services/speech-to-text/output.html#redaction) oscura, o maschera, i dati numerici da una trascrizione finale. L'oscuramento ha lo scopo di rimuovere le informazioni personali sensibili, come i numeri delle carte di credito, dalle trascrizioni. La funzione è supportata per l'audio in inglese americano, giapponese e coreano. (Funzionalità beta.)
-   Il [filtro di contenuto volgare](/docs/services/speech-to-text/output.html#profanity_filter) censura il contenuto volgare nelle trascrizioni in inglese americano.

## Supporto delle lingue
{: #languages}

Il servizio offre modelli per le seguenti lingue: portoghese brasiliano, francese, tedesco, giapponese, coreano, cinese mandarino, arabo standard moderno, spagnolo, inglese britannico e inglese americano. Il servizio non supporta tutte le funzioni per tutte le lingue. Inoltre, supporta alcune funzioni come generalmente disponibili (GA) per l'uso in produzione e altre come offerte beta per diverse lingue.

-   Le interfacce WebSocket e HTTP sono generalmente disponibili per tutte le lingue.
-   Il servizio offre modelli a banda larga, modelli a banda stretta o entrambi per diverse lingue. Per ulteriori informazioni, vedi [Lingue e modelli](/docs/services/speech-to-text/models.html).
-   Alcune funzioni di riconoscimento vocale sono disponibili solo per determinate lingue. Per ulteriori informazioni, vedi il [Riepilogo dei parametri](/docs/services/speech-to-text/summary.html).
-   L'interfaccia di personalizzazione del modello di lingua è generalmente disponibile per la maggior parte delle lingue. L'interfaccia di personalizzazione del modello acustico è disponibile come funzionalità beta per tutte le lingue. Per ulteriori informazioni, vedi [Supporto delle lingue per la personalizzazione](/docs/services/speech-to-text/custom.html#languageSupport).

## Prezzi
{: #pricing-index}

Per ulteriori informazioni sui piani dei prezzi per il servizio, vedi il servizio {{site.data.keyword.speechtotextshort}} nel [Catalogo {{site.data.keyword.Bluemix_short}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/catalog/services/speech-to-text){: new_window}. Per le risposte alle domande relative ai prezzi e gli esempi sui costi di utilizzo mensili, vedi le [Domande frequenti sui prezzi](/docs/services/speech-to-text/faq-pricing.html).

## Prova il servizio
{: #tryOut}

Per esempi del servizio {{site.data.keyword.speechtotextshort}} in azione, vedi

-   Una [demo rapida ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://speech-to-text-demo.ng.bluemix.net/){: new_window} che trascrive il testo dall'immissione audio in streaming o da un file che carichi.
-   Applicazioni nei [Kit starter ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://www.ibm.com/watson/developercloud/starter-kits.html){: new_window} {{site.data.keyword.ibmwatson}} che mostrano il servizio.
-   Il post del blog {{site.data.keyword.watson}} [Getting robots to listen: Using {{site.data.keyword.watson}}'s {{site.data.keyword.speechtotextshort}} service ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/blogs/watson/2016/07/getting-robots-listen-using-watsons-speech-text-service/){: new_window} che mostra come utilizzare l'interfaccia WebSocket del servizio con Python per estrarre il discorso dall'audio. Il post fornisce un'esercitazione approfondita che descrive il codice e i passi coinvolti.
