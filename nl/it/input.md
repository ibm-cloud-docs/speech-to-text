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

# Funzioni di input
{: #input}

Il servizio {{site.data.keyword.speechtotextshort}} offre le seguenti funzioni per specificare il modo in cui il servizio deve eseguire una richiesta di riconoscimento vocale. Tutti i parametri di input descritti nelle seguenti sezioni sono facoltativi. Solo l'audio di input è obbligatorio.
{: shortdesc}

-   Per degli esempi di semplici richieste di riconoscimento vocale per ciascuna delle interfacce del servizio, vedi [Effettuazione di una richiesta di riconoscimento](/docs/services/speech-to-text/basic-request.html).
-   Per un elenco alfabetico di tutti i parametri di riconoscimento vocale disponibili, compresi il loro stato (generalmente disponibile o beta) e le lingue supportate, vedi il [Riepilogo dei parametri](/docs/services/speech-to-text/summary.html).

## Modelli personalizzati
{: #custom-input}

La personalizzazione del modello di lingua e acustico è disponibile a diversi livelli di supporto (generalmente disponibile o beta) per le diverse lingue. Per ulteriori informazioni, vedi [Supporto delle lingue per la personalizzazione](/docs/services/speech-to-text/custom.html#languageSupport).
{: note}

Tutte le interfacce accettano un modello personalizzato per l'utilizzo in una richiesta di riconoscimento:

-   I *modelli di lingua personalizzati* espandono il vocabolario di base del servizio con la terminologia da specifici domini. Utilizza il parametro `language_customization_id` per includere un modello di lingua personalizzato con una richiesta. Puoi anche specificare un parametro `customization_weight` facoltativo. Il parametro indica il peso relativo che viene dato alle parole dal modello personalizzato invece che alle parole dal vocabolario di base.

    Per ulteriori informazioni, vedi [Utilizzo di un modello di lingua personalizzato](/docs/services/speech-to-text/language-use.html).
-   I *modelli acustici personalizzati* adattano il modello acustico di base del servizio per le caratteristiche acustiche del tuo ambiente e dei tuoi parlanti. Utilizza il parametro `acoustic_customization_id` per includere un modello acustico personalizzato con una richiesta. Puoi specificare sia un modello di lingua personalizzato che un modello acustico personalizzato con una richiesta.

    Per ulteriori informazioni, vedi [Utilizzo di un modello acustico personalizzato](/docs/services/speech-to-text/acoustic-use.html).

I modelli personalizzati sono basati su uno dei modelli di lingua descritti in [Lingue e modelli](/docs/services/speech-to-text/models.html). Un modello personalizzato può essere utilizzato solo con il modello di base per il quale viene creato. Se il tuo modello personalizzato è basato su un modello diverso da `en-US_BroadbandModel`, il valore predefinito, devi specificare anche il nome del modello con la richiesta. Per utilizzare un modello personalizzato, devi immettere la richiesta con le credenziali del servizio create per l'istanza del servizio proprietaria del modello personalizzato.

Per un'introduzione alla personalizzazione, vedi [L'interfaccia di personalizzazione](/docs/services/speech-to-text/custom.html).

### Esempi di modello personalizzato
{: #customExample}

La seguente richiesta di esempio include il parametro `language_customization_id` per utilizzare il modello di lingua personalizzato con l'ID specificato. Include il parametro `customization_weight` per indicare che alle parole dal modello personalizzato deve essere dato un peso relativo di `0.5`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.5"
```
{: pre}

La seguente richiesta di esempio utilizza sia un modello di lingua personalizzato che un modello acustico personalizzato. Il primo viene identificato con il parametro `language_customization_id` e il secondo con il parametro `acoustic_customization_id`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&acoustic_customization_id={customization_id}"
```
{: pre}

Per degli esempi che utilizzano i modelli personalizzati con ciascuna delle interfacce del servizio, vedi:

-   [Utilizzo di un modello di lingua personalizzato](/docs/services/speech-to-text/language-use.html)
-   [Utilizzo di un modello acustico personalizzato](/docs/services/speech-to-text/acoustic-use.html)

## Grammatiche
{: #grammars-input}

La funzione delle grammatiche è una funzionalità beta. Il servizio supporta le grammatiche per tutte le lingue per le quali supporta la personalizzazione del modello di lingua.
{: note}

Puoi aggiungere le grammatiche a un modello di lingua personalizzato e utilizzarle per il riconoscimento vocale. Le grammatiche utilizzano una specifica della lingua formale per definire un set di regole di produzione per la trascrizione di stringhe. Le regole specificano come formare delle stringhe valide dall'alfabeto della lingua.

Quando utilizzi una grammatica per il riconoscimento vocale, il servizio riconosce solo le frasi che sono riconosciute dalla grammatica. Limitando lo spazio di ricerca per le stringhe valide, il servizio può fornire i risultati in modo più veloce e più accurato. 

Tutte le interfacce accettano i seguenti parametri per una richiesta di riconoscimento:

-   Il parametro `language_customization_id` identifica il modello di lingua personalizzato per cui è definita la grammatica. Devi immettere la richiesta con le credenziali del servizio per l'istanza del servizio proprietaria del modello.
-   Il parametro `grammar_name` specifica la grammatica che desideri utilizzare. Puoi specificare solo una singola grammatica con una richiesta.

Per ulteriori informazioni, vedi [Utilizzo di grammatiche con i modelli di lingua personalizzati](/docs/services/speech-to-text/grammar.html).

### Esempio di grammatiche
{: #grammarsExample}

La seguente richiesta di esempio include i parametri `language_customization_id` e `grammar_name` per limitare la risposta del servizio alle stringhe definite nella grammatica denominata `list-abnf`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&grammar_name=list-abnf"
```
{: pre}

Per degli esempi che utilizzano le grammatiche con ciascuna delle interfacce del servizio, vedi [Utilizzo di una grammatica per il riconoscimento vocale](/docs/services/speech-to-text/grammar-use.html).

## Versione del modello di base
{: #version}

Per migliorare la qualità del riconoscimento vocale, il servizio di tanto in tanto aggiorna i modelli di lingua di base descritti in [Lingue e modelli](/docs/services/speech-to-text/models.html). I modelli di base per le lingue sono indipendenti l'uno dall'altro, come lo sono i modelli a banda larga e a banda stretta per una lingua. Pertanto, gli aggiornamenti ai modelli di base si verificano indipendentemente l'uno dall'altro e non hanno alcun effetto sugli altri modelli.

Quando esistono più versioni di un modello di base, il parametro `base_model_version` facoltativo specifica la versione del modello da utilizzare con una richiesta di riconoscimento. Il parametro è concepito principalmente per essere utilizzato con i modelli personalizzati aggiornati per un nuovo modello di base ma può essere utilizzato senza un modello personalizzato. La versione di un modello di base utilizzata per una richiesta dipende dal fatto che tu passi o meno il parametro `base_model_version`. Dipende anche dal fatto che tu specifichi o meno un modello personalizzato (di lingua, acustico o entrambi) con la richiesta.

-   *Se non specifichi un modello personalizzato con la richiesta*

    -   Ometti il parametro `base_model_version` per utilizzare l'ultima versione del modello di base.
    -   Specifica il parametro `base_model_version` per utilizzare una versione specifica del modello di base.

-   *Se specifichi un modello personalizzato con la richiesta*

    -   Ometti il parametro `base_model_version` per utilizzare l'ultima versione del modello di base a cui viene eseguito l'upgrade del modello personalizzato. Ad esempio, se viene eseguito l'upgrade del modello personalizzato all'ultima versione del modello di base, il servizio utilizza le ultime versioni dei modelli di base e personalizzato.
    -   Specifica il parametro `base_model_version` per utilizzare le versioni specificate sia del modello di base che di quello personalizzato.

Il parametro è concepito per essere utilizzato con i modelli personalizzati. Di conseguenza, è possibile acquisire informazioni sulle versioni disponibili di un modello di base solo elencando le informazioni su un modello personalizzato basato su di esso.

-   Per ulteriori informazioni sull'upgrade dei modelli personalizzati, vedi [Upgrade dei modelli personalizzati](/docs/services/speech-to-text/custom-upgrade.html).
-   Per ulteriori informazioni sull'utilizzo di versioni diverse dei modelli di base e personalizzati per il riconoscimento vocale, vedi [Effettuazione di richieste di riconoscimento con modelli personalizzati di cui è stato eseguito l'upgrade](/docs/services/speech-to-text/custom-upgrade.html#upgradeRecognition).

## Trasmissione audio
{: #transmission}

*Con l'interfaccia WebSocket,* i dati audio vengono sempre inviati in streaming al servizio sulla connessione. Puoi passare i dati attraverso il socket tutti insieme oppure puoi passare i dati per il caso di utilizzo dal vivo man mano che diventano disponibili. Il servizio restituisce i risultati man mano che diventano disponibili.

*Con le interfacce HTTP,* puoi trasmettere l'audio al servizio in uno dei due modi di seguito indicati:

-   *Fornitura unica* - ometti l'intestazione `Transfer-Encoding` e passi tutti i dati audio al servizio tutti in una volta come una singola fornitura.
-   *Streaming* - imposti l'intestazione della richiesta `Transfer-Encoding` sul valore `chunked` e invii in streaming i dati su una connessione persistente. I dati non devono esistere completamente prima che tu ne esegua lo streaming al servizio. Puoi inviare in streaming i dati man mano che diventano disponibili. Il servizio invia i risultati solo quando riceve la porzione finale, che indichi inviando una porzione vuota.

    Per ulteriori informazioni sull'invio in streaming di audio suddiviso in porzioni con l'intestazione `Transfer-Encoding`, vedi
    -   [Chunked transfer encoding ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://en.wikipedia.org/wiki/Chunked_transfer_encoding){: new_window}
    -   [Transfer Codings ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://tools.ietf.org/html/rfc7230#section-4){: new_window} in *IETF RFC 7320 HTTP/1.1: Message Syntax and Routing*

Con le interfacce HTTP, il servizio trascrive sempre l'intero flusso audio prima di inviare eventuali risultati. I risultati possono includere più elementi `trascript` per indicare le frasi separate da pause. Concatena gli elementi `trascript` per assemblare la trascrizione completa.

Il servizio applica i timeout su una sessione di streaming. Può terminare una sessione di streaming se rileva un lungo periodo di silenzio o non riceve audio durante un periodo di 30 secondi. Per ulteriori informazioni sui timeout e su come evitarli, vedi [Timeout](#timeouts).

### Esempio di trasmissione audio
{: #transmissionExample}

La seguente richiesta di esempio specifica `chunked` per l'intestazione `Transfer-Encoding` per utilizzare la modalità di streaming. La connessione rimane aperta per accettare ulteriori porzioni di audio.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--header "Transfer-Encoding: chunked"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## Timeout
{: #timeouts}

Quando avvii una sessione di streaming con i metodi di riconoscimento vocale HTTP o WebSocket, il servizio applica i timeout di inattività e sessione. Se un timeout scade durante una sessione di streaming, il servizio chiude la connessione. La tua applicazione deve risolvere normalmente le possibili connessioni chiuse.

Quando invii in streaming l'audio su HTTP, il servizio invia un carattere di spazio nella sua risposta ogni 20 secondi. Il servizio esegue tale operazione per migliorare l'utilizzabilità evitando il timeout di inattività REST HTTP di 30 secondi. Per mantenere attiva la connessione mentre è in corso il riconoscimento, il servizio continua a inviare questo carattere di spazio finché non completa la sua trascrizione. Il carattere di spazio non ha alcun effetto sui dati di risposta con codifica JSON.

Il timeout di inattività HTTP è diverso dal timeout di inattività del servizio. L'interfaccia WebSocket non è soggetta a questo timeout HTTP.
{: note}

### Timeout di inattività 
{: #timeouts-inactivity}

Un *timeout di inattività* (codice di stato HTTP 400) si verifica quando il servizio sta ricevendo audio ma rileva solo silenzio continuo o attività non vocale (nessun discorso) per 30 secondi. Il servizio invia il messaggio di errore `No speech detected for 30s`. Il timeout di inattività è utile, ad esempio, per terminare una sessione quando un utente semplicemente si allontana da un microfono dal vivo.

Il timeout di inattività predefinito è 30 secondi. Puoi sovrascrivere questo valore utilizzando il parametro `inactivity_timeout` . Specifica un valore maggiore per aumentare il timeout di inattività. Specifica un valore di `-1` per impostare il timeout di inattività su un tempo indefinito. Ti viene addebitato tutto l'audio che invii al servizio, compreso il silenzio, quindi aumentare il timeout di inattività può comportare degli addebiti aggiuntivi per una sessione di streaming che invia solo silenzio.

La seguente richiesta di esempio imposta il timeout di inattività su 60 secondi. La richiesta invia un file iniziale per avviare la sessione di streaming.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Transfer-Encoding: chunked"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?inactivity_timeout=60"
```
{: pre}

### Timeout della sessione
{: #timeouts-session}

Un *timeout della sessione* (codice di stato HTTP 408) si verifica quando non riesci a inviare una quantità di audio sufficiente a mantenere attiva una sessione di streaming. Il servizio può considerare inattiva una sessione e attivare un timeout della sessione per i seguenti motivi:

-   Non riesci a inviare almeno 15 secondi di audio al servizio in qualsiasi finestra di 30 secondi.

    Finché non invii l'ultima porzione per indicare la fine del flusso, devi inviare almeno 15 secondi di audio entro qualsiasi periodo di 30 secondi. L'audio può consistere in silenzio se imposti il parametro `inactivity_timeout` su un valore più grande o su `-1`. Ti viene addebitata la durata di qualsiasi audio che invii al servizio, compreso il silenzio.
-   Invii il flusso audio a una velocità molto più lenta di quella in tempo reale.

    Idealmente, inizi una richiesta di stabilire una sessione immediatamente prima di ottenere l'audio per la trascrizione. Mantieni quindi la sessione inviando l'audio a una velocità prossima a quella in tempo reale.

Non ti devi preoccupare del timeout della sessione dopo che hai inviato l'ultima porzione per indicare la fine del flusso. Il servizio continua a elaborare l'audio finché non restituisce i risultati finali della trascrizione.

Quando trascrivi un flusso audio lungo, il servizio può impiegare più di 30 secondi per elaborare l'audio e generare una risposta. Il servizio inizia a calcolare il timeout della sessione solo dopo che ha terminato l'elaborazione di tutto l'audio che ha ricevuto, Il tempo di elaborazione del servizio non può causare per la sessione il superamento del suo timeout di 30 secondi.

Ad esempio, se invii un'ora di audio nei primi 10 secondi di una sessione, il servizio può impiegare 300 secondi per elaborare l'audio. Per mantenere questa sessione attiva, devi inviare almeno altri 15 secondi di audio, compreso il silenzio, non più tardi di 340 secondi dall'inizio della sessione.

In questo esempio, se invii altri 15 secondi di audio al segno dei 100 secondi della sessione, il servizio può dedicare due secondi in più all'elaborazione di questo audio. In questo caso, devi inviare altri 15 secondi di audio non più tardi di 342 secondi dall'inizio della sessione.

Non fare affidamento sul tempo di elaborazione o sull'avere eventualmente ricevuto risultati per determinare se una sessione di streaming è inattiva. Presumi che il servizio possa elaborare tutto l'audio istantaneamente e invia i dati al servizio di conseguenza. Se invii in streaming l'audio in tempo reale, non restare indietro con l'invio di audio a metà del tempo reale (15 secondi di audio) in qualsiasi finestra di 30 secondi. Questa frequenza è di norma sufficiente per tenere conto dei ritardi e della latenza di rete.
{: important}

## Registrazione delle richieste
{: #logging}

Per impostazione predefinita, {{site.data.keyword.IBM_notm}} registra tutte le richieste ai servizi {{site.data.keyword.watson}} e i loro risultati. La registrazione viene eseguita solo per migliorare i servizi per i futuri utenti. I dati registrati non vengono condivisi o resi pubblici.

Se sei preoccupato della protezione della riservatezza delle informazioni personali degli utenti o a parte ciò non voi che le tue richieste siano registrate da IBM, puoi scegliere che IBM non registri i dati (rifiuto esplicito).Puoi scegliere di rifiutare esplicitamente la registrazione a livello di account o a livello di richieste API. Per ulteriori informazioni, vedi [Controllo della registrazione delle richieste per i servizi {{site.data.keyword.watson}}](/docs/services/watson/getting-started-logging.html).

## Sicurezza delle informazioni
{: #security-input}

La sicurezza delle informazioni include le funzioni per associare un ID cliente ai dati passati al servizio con una richiesta. Associ un ID cliente ai dati passando l'intestazione `X-Watson-Metadata` con la richiesta. Se necessario, puoi quindi eliminare i dati utilizzando il metodo `DELETE /v1/user_data`. Per ulteriori informazioni, vedi [Sicurezza delle informazioni](/docs/services/speech-to-text/information-security.html).
