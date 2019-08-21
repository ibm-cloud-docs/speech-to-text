---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-06"

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

# Formati audio
{: #audio-formats}

Il servizio {{site.data.keyword.speechtotextfull}} può estrarre il discorso dall'audio in molti formati.

-   Se non hai familiarità con l'audio e con come viene descritto e specificato, inizia con [Caratteristiche e terminologia audio](#terminology) per aiutarti a iniziare.
-   Se hai già capito come usare l'audio, passa a [Formati audio supportati](#formats) per informazioni dettagliate sui formati supportati dal servizio.

Le sezioni finali, [Limiti e compressione dei dati](#limits), [Conversione audio](#conversion) e [Suggerimenti per migliorare il riconoscimento vocale](#audioTips), possono aiutarti ad ottenere il massimo dall'uso del servizio.

## Caratteristiche e terminologia audio
{: #terminology}

La seguente terminologia viene utilizzata per descrivere le caratteristiche dei dati audio per i diversi formati.

### Frequenza di campionamento
{: #samplingRate}

La *frequenza di campionamento* è il numero di campioni audio che vengono presi al secondo. La frequenza di campionamento viene misurata in Hertz (Hz) o kilohertz (kHz). Ad esempio, una frequenza di 16000 campioni al secondo è uguale a 16.000 Hz (o 16 kHz). Con il servizio {{site.data.keyword.speechtotextshort}}, specifichi un modello per indicare la frequenza di campionamento del tuo audio:

-   I modelli a *banda larga* vengono utilizzati per l'audio campionato a non meno di 16 kHz, che {{site.data.keyword.IBM}} consiglia per applicazioni reattive in tempo reale (ad esempio, per le applicazioni di riconoscimento vocale dinamico).
-   I modelli a *banda stretta* vengono utilizzati per l'audio campionato a non meno di 8 kHz, che è la frequenza che viene tipicamente utilizzata per l'audio telefonico.

Il servizio supporta sia l'audio a banda larga che a banda stretta per la maggior parte delle lingue e dei formati. Regola automaticamente la frequenza di campionamento del tuo audio in modo che corrisponda al modello specificato prima che riconosca il discorso.

-   Per i modelli a banda larga, il servizio converte a 16 kHz l'audio registrato a frequenze di campionamento più elevate.
-   Per i modelli a banda stretta, converte l'audio a 8 kHz.

In teoria, puoi inviare audio a 44 kHz con un modello a banda larga o a banda stretta, ma ciò aumenta inutilmente la dimensione dell'audio. Per massimizzare la quantità di audio che puoi inviare, abbina la frequenza di campionamento dell'audio al modello che usi. Il servizio non accetta l'audio campionato a una frequenza inferiore alla frequenza di campionamento intrinseca del modello.

#### Note sui formati audio
{: #samplingRateNotes}

-   Per i formati `audio/alaw`, `audio/l16` e `audio/mulaw`, devi specificare la frequenza del tuo audio.
-   Per i formati `audio/basic` e `audio/g729`, il servizio supporta solo l'audio a banda stretta.

#### Ulteriori informazioni
{: #samplingRateMore}

-   Per ulteriori informazioni sulle frequenze di campionamento, vedi [Sampling (signal processing)](https://wikipedia.org/wiki/Sampling_%28signal_processing%29){: external}.
-   Per ulteriori informazioni sui modelli offerti dal servizio per ciascuna lingua supportata, vedi [Lingue e modelli](/docs/services/speech-to-text?topic=speech-to-text-models).

### Velocità di bit
{: #bitRate}

La *velocità di bit* è il numero di bit di dati inviati al secondo. La velocità di bit per un flusso audio viene misurata in kilobit al secondo (kbps). La velocità di bit viene calcolata dalla frequenza di campionamento e dal numero di bit memorizzati per ogni campione. Per il riconoscimento vocale, {{site.data.keyword.IBM}} consiglia di registrare 16 bit per campione per il tuo audio.

Ad esempio, l'audio che utilizza una frequenza di campionamento a banda larga di 16 kHz e 16 bit per campione ha una velocità di bit di 256 kbps: `(16.000 * 16) / 1000`.

#### Ulteriori informazioni
{: #bitRateMore}

-   Per ulteriori informazioni sulle velocità di bit, vedi [Bit rate](https://wikipedia.org/wiki/Bit_rate){: external}.
-   Per una discussione generale sulle frequenze di campionamento e velocità di bit, vedi [What are bit rates?](http://www.richardfarrar.com/what-are-bit-rates/){: external} e [Choosing bit rates for podcasts](http://www.richardfarrar.com/choosing-bit-rates-for-podcasts/){: external}.

### Compressione
{: #compression}

La *compressione* viene utilizzata da molti formati audio per ridurre le dimensioni dei dati audio. La compressione riduce il numero di bit memorizzati per ogni campione e quindi la velocità di bit. Alcuni formati non utilizzano alcuna compressione, ma la maggior parte utilizza uno dei tipi di compressione di base:

-   La compressione *senza perdita* riduce la dimensione dell'audio senza alcuna perdita di qualità, ma il rapporto di compressione è in genere ridotto.
-   La compressione *con perdita* riduce la dimensione dell'audio fino a 10 volte, ma comporta anche la perdita della qualità e di alcuni dati.

Con il servizio {{site.data.keyword.speechtotextshort}}, puoi tranquillamente utilizzare la compressione con perdita per massimizzare la quantità di audio che puoi inviare al servizio con una richiesta di riconoscimento. Poiché la gamma dinamica della voce umana è più limitata, ad esempio, della musica, il discorso può consentire una velocità di bit che è molto inferiore rispetto ad altri tipi di audio. Per il riconoscimento vocale, {{site.data.keyword.IBM}} consiglia di utilizzare 16 bit per campione per il tuo audio e di adoperare un formato che comprime i dati audio.

#### Note sui formati audio
{: #compressionNotes}

-   I formati `audio/ogg` e `audio/webm` sono contenitori la cui compressione si basa sul codec che utilizzi per codificare i dati: Opus o Vorbis.
-   Il formato `audio/wav` è un contenitore che può includere dati non compressi, senza perdita o con perdita.

#### Ulteriori informazioni
{: #compressionMore}

-   Per ulteriori informazioni sulla compressione audio, vedi [Data compression (Audio)](https://wikipedia.org/wiki/Data_compression#Audio){: external}.
-   Per ulteriori informazioni sull'utilizzo della compressione dei dati per aumentare la quantità di audio che puoi inviare con una richiesta, vedi [Limiti e compressione dei dati](#limits).

### Canali
{: #channels}

I *canali* indicano il numero di flussi dell'audio registrato:

-   L'audio *monofonico* (o mono) ha solo un singolo canale.
-   L'audio *stereofonico* (o stereo) ha in genere due canali.

Il servizio {{site.data.keyword.speechtotextshort}} accetta l'audio con un massimo di 16 canali. Poiché utilizza solo un singolo canale per il riconoscimento vocale, il servizio esegue il downmix dell'audio con più canali su mono a un canale durante la transcodifica.

#### Note sui formati audio
{: #channelsNotes}

-   Per il formato `audio/l16`, devi specificare il numero di canali se il tuo audio ha più di un canale.
-   Per il formato `audio/wav`, il servizio accetta l'audio con un massimo di nove canali.

#### Ulteriori informazioni
{: #channelsMore}

-   Per ulteriori informazioni sui canali audio, vedi [Audio signal](https://wikipedia.org/wiki/Audio_signal){: external}.

### Proprietà endian
{: #endianness}

La *proprietà endian* indica come i byte di dati sono organizzati dall'architettura del computer sottostante:

-   *Big-endian* ordina i dati in base al bit più significativo.
-   *Little-endian* ordina i dati in base al bit meno significativo.

Il servizio {{site.data.keyword.speechtotextshort}} rileva automaticamente la proprietà endian dell'audio in entrata.

#### Note sui formati audio
{: #endiannessNotes}

-   Per il formato `audio/l16`, puoi specificare la proprietà endian per disabilitare il rilevamento automatico, laddove necessario.

#### Ulteriori informazioni
{: #endiannessMore}

-   Per ulteriori informazioni sulla proprietà endian, vedi [Endianness](https://wikipedia.org/wiki/Endianness){: external}.

### Frequenza audio
{: #frequency}

La *frequenza audio* si riferisce alla gamma di frequenze udibili nell'audio. La frequenza udibile standard per l'uomo è generalmente accettata da 20 a 20.000 Hz. Puoi utilizzare l'analisi spettrografica per produrre uno spettrogramma che rivela il contenuto di frequenza del tuo audio.

La frequenza di campionamento applicata all'audio è in genere il doppio della frequenza massima dell'audio. Ad esempio, una frequenza di campionamento di 16 kHz significa che la frequenza massima del segnale audio campionato è di 8 kHz. I modelli del servizio vengono creati tenendo conto di questi fattori.

-   I modelli a banda stretta vengono creati con audio campionato a 8 kHz. I modelli a banda stretta si aspettano di trovare informazioni in un intervallo inferiore o uguale a 4 kHz.
-   I modelli a banda larga vengono creati con audio campionato a 16 kHz. I modelli a banda larga si aspettano di trovare informazioni nell'intervallo da 4 a 8 kHz.

I dati di addestramento per i modelli vengono derivati da diversi canali (telefonia per i modelli a banda stretta). I modelli riflettono le caratteristiche dei canali su cui sono stati addestrati.

#### Sovracampionamento
{: #upsampling}

Il *sovracampionamento* aumenta la frequenza di campionamento dell'audio ma non introduce nuove informazioni al suo interno. Produce un'approssimazione del segnale audio che sarebbe stato ottenuto campionando l'audio a una frequenza maggiore. Aumenta la dimensione dei dati audio.

Le informazioni nell'audio che sono state inizialmente campionate con una frequenza a banda stretta sono limitate all'intervallo da 0 a 4 kHz. È improbabile che il sovracampionamento dell'audio a banda stretta a una frequenza di campionamento superiore migliori l'accuratezza del riconoscimento vocale. Se sovracampioni l'audio a banda stretta, sarà privo delle informazioni nell'intervallo previsto dai modelli a banda larga. Inoltre, le informazioni che si trovano nell'intervallo previsto di un campione a banda stretta sono qualitativamente diverse dalle informazioni che si trovano nello stesso intervallo di un campione a banda larga. Quindi, il sovracampionamento riduce effettivamente l'accuratezza del riconoscimento.

Per una frequenza di campionamento a banda larga di 16 kHz, la frequenza massima presente nel segnale audio campionato dovrebbe essere di 8 kHz. Pertanto, devi filtrare il segnale originale a 8 kHz prima di campionarlo con una frequenza di 16 kHz. Altrimenti, si verifica una riduzione delle prestazioni a causa del fenomeno noto come *aliasing*. Per comprenderne il motivo, vedi [Nyquist frequency](https://wikipedia.org/wiki/Nyquist_frequency){: external}.

Un confronto utile potrebbe essere quello di immaginare di guardare un nastro VHS su una grande TV HD a schermo piatto. L'immagine è sfocata perché la riproduzione del nastro su un dispositivo ad alta definizione non può effettivamente aggiungere nuove informazioni al flusso. Rende semplicemente il formato compatibile con il dispositivo migliore. Lo stesso vale per il sovracampionamento dell'audio.

#### Sottocampionamento
{: #downsampling}

Il *sottocampionamento* diminuisce la frequenza di campionamento dell'audio. Produce un'approssimazione del segnale audio che sarebbe stato ottenuto campionando l'audio a una frequenza inferiore. Il sottocampionamento non rimuove le informazioni dal segnale audio, ma riduce la dimensione dei dati audio.

Il sottocampionamento del tuo audio può essere efficace in alcuni casi. Ad esempio, se la frequenza di campionamento dell'audio è maggiore di 8 kHz *e* un esame spettrografico non rivela alcun contenuto di frequenza superiore a 4 kHz, prendi in considerazione il sottocampionamento dell'audio a 8 kHz.

#### Ulteriori informazioni
{: #frequencyMore}

-   Per ulteriori informazioni sulla frequenza audio, vedi [Audio frequency](https://wikipedia.org/wiki/Audio_frequency){: external}.
-   Per ulteriori informazioni sul sovracampionamento, vedi [Upsampling](https://wikipedia.org/wiki/Upsampling){: external}.
-   Per ulteriori informazioni sul sottocampionamento, vedi [Downsampling](https://wikipedia.org/wiki/Downsampling_%28signal_processing%29){: external}.

## Formati audio supportati
{: #formats}

La Tabella 1 fornisce un riepilogo dei formati audio supportati dal servizio.

-   *Formato audio e compressione* identifica ogni formato e indica la sua compressione supportata. Puoi inviare un massimo di 100 MB di audio al servizio con una singola richiesta WebSocket o HTTP sincrona. Utilizzando un formato che supporta la compressione, puoi ridurre la dimensione del tuo audio per massimizzare la quantità di dati che puoi passare al servizio. Per ulteriori informazioni, vedi [Limiti e compressione dei dati](#limits).
-   *Specifica tipo di contenuto* indica se devi utilizzare l'intestazione `Content-Type` o il parametro equivalente per specificare il formato (tipo MIME) dell'audio che invii al servizio. Puoi specificare il formato audio per qualsiasi richiesta, ma non è sempre necessario:
    -   Per la maggior parte dei formati, il tipo di contenuto è facoltativo. Puoi omettere il tipo di contenuto o specificare `application/octet-stream` affinché il servizio rilevi automaticamente il formato.
    -   Per altri formati, il tipo di contenuto è obbligatorio. Questi formati non forniscono le informazioni, come la frequenza di campionamento, di cui il servizio ha bisogno per rilevare automaticamente il loro formato.
-   Le colonne finali identificano ulteriori *Parametri obbligatori* e *Parametri facoltativi* per ogni formato. Le seguenti sezioni forniscono ulteriori informazioni su questi parametri.

<table>
  <caption>Tabella 1. Riepilogo dei formati audio supportati</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom">
      Formato audio<br>e compressione
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Specifica tipo<br/>di contenuto
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Parametri<br/>obbligatori
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Parametri<br/>facoltativi
    </th>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/alaw](#alaw)<br/>Con perdita
    </td>
    <td style="text-align:center">
      Obbligatorio
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      Nessuno
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/basic](#basic)<br/>Con perdita
    </td>
    <td style="text-align:center">
      Obbligatorio
    </td>
    <td style="text-align:center">
      Nessuno
    </td>
    <td style="text-align:center">
      Nessuno
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/flac](#flac)<br/>Senza perdita
    </td>
    <td style="text-align:center">
      Facoltativo
    </td>
    <td style="text-align:center">
      Nessuno
    </td>
    <td style="text-align:center">
      Nessuno
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/g729](#g729)<br/>Con perdita
    </td>
    <td style="text-align:center">
      Facoltativo
    </td>
    <td style="text-align:center">
      Nessuno
    </td>
    <td style="text-align:center">
      Nessuno
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/l16](#l16)<br/>Nessuno
    </td>
    <td style="text-align:center">
      Obbligatorio
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      <code>channels={integer}</code><br/>
      <code>endianness=big-endian</code><br/>
      <code>endianness=little-endian</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/mp3](#mp3)<br/>
      [audio/mpeg](#mp3)<br/>Con perdita
    </td>
    <td style="text-align:center">
      Facoltativo
    </td>
    <td style="text-align:center">
      Nessuno
    </td>
    <td style="text-align:center">
      Nessuno
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/mulaw](#mulaw)<br/>Con perdita
    </td>
    <td style="text-align:center">
      Obbligatorio
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      Nessuno
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/ogg](#ogg)<br/>Con perdita
    </td>
    <td style="text-align:center">
      Facoltativo
    </td>
    <td style="text-align:center">
      Nessuno
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/wav](#wav)<br/>Nessuna, senza perdita<br/>o con perdita
    </td>
    <td style="text-align:center">
      Facoltativo
    </td>
    <td style="text-align:center">
      Nessuno
    </td>
    <td style="text-align:center">
      Nessuno
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/webm](#webm)<br/>Con perdita
    </td>
    <td style="text-align:center">
      Facoltativo
    </td>
    <td style="text-align:center">
      Nessuno
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
</table>

Quando utilizzi il comando `curl` per effettuare una richiesta di riconoscimento vocale con le interfacce HTTP, devi specificare il formato audio con l'intestazione `Content-Type`, specificare `"Content-Type: application/octet-stream"` o specificare `"Content-Type:"`. Se ometti completamente l'intestazione, `curl` utilizza il valore predefinito `application/x-www-form-urlencoded`.
{: important}

### Formato audio/alaw
{: #alaw}

*A-law* (`audio/alaw`) è un formato audio con perdita a canale singolo. Utilizza un algoritmo simile all'algoritmo u-law applicato dai formati `audio/basic` e `audio/mulaw`, sebbene l'algoritmo A-law produca caratteristiche di segnale diverse. Quando utilizzi questo formato, il servizio richiede un parametro aggiuntivo sulla specifica del formato.

<table>
  <caption>Tabella 2. Parametro per il formato `audio/alaw`</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Parametro
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Descrizione
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Obbligatorio</em>
    </td>
    <td>
      Un numero intero che specifica la frequenza di campionamento a cui viene
      catturato l'audio. Ad esempio, specifica il seguente parametro per i dati
      audio catturati a 8 kHz:<br/><br/>
      <code>audio/alaw;rate=8000</code>
    </td>
  </tr>
</table>

Per ulteriori informazioni, vedi [A-law algorithm](https://wikipedia.org/wiki/A-law_algorithm){: external}.

### Formato audio/basic
{: #basic}

*Audio di base* (`audio/basic`) è un formato audio con perdita a canale singolo che viene codificato utilizzando dati u-law (o mu-law) a 8 bit campionati a 8 kHz. Questo formato fornisce un minimo denominatore comune per indicare il tipo di supporto audio. Il servizio supporta l'utilizzo di file in formato `audio/basic` solo con i modelli a banda stretta.

Per ulteriori informazioni, vedi IETF (Internet Engineering Task Force) [Request for Comment (RFC) 2046](https://tools.ietf.org/html/rfc2046){: external} e [iana.org/assignments/media-types/audio/basic](http://www.iana.org/assignments/media-types/audio/basic){: external}.

### Formato audio/flac
{: #flac}

*FLAC (Free Lossless Audio Codec )* (`audio/flac`) è un formato audio senza perdita. Per ulteriori informazioni, vedi [FLAC](https://wikipedia.org/wiki/FLAC){: external}.

### Formato audio/g729
{: #g729}

*G.729* (`audio/g729`) è un formato audio con perdita che supporta dati codificati a 8 kHz. Il servizio supporta solo l'appendice D di G.729, non l'appendice J. Il servizio supporta l'utilizzo di file in formato `audio/g729` solo con i modelli a banda stretta. Per ulteriori informazioni, vedi [G.729](https://wikipedia.org/wiki/G.729){: external}.

### Formato audio/l16
{: #l16}

*PCM (Pulse-Code Modulation) lineare a 16 bit* (`audio/l16`) è un formato audio non compresso. Utilizza questo formato per passare un file PCM raw. L'audio lineare PCM può anche essere trasportato all'interno di un file WAV (Waveform Audio File Format) contenitore. Quando utilizzi il formato `audio/l16`, il servizio accetta ulteriori parametri obbligatori e facoltativi sulla specifica del formato.

<table>
  <caption>Tabella 3. Parametri per il formato `audio/l16`</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Parametro
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Descrizione
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Obbligatorio</em>
    </td>
    <td>
      Un numero intero che specifica la frequenza di campionamento a cui viene
      catturato l'audio. Ad esempio, specifica il seguente parametro per i dati
      audio catturati a 16 kHz:<br/><br/>
      <code>audio/l16;rate=16000</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>channels</code><br/><em>Facoltativo</em>
    </td>
    <td>
      Per impostazione predefinita, il servizio considera l'audio come se avesse un singolo canale.
      <em>Se l'audio ha più di un canale,</em> devi specificare
      un numero intero che identifica il numero di canali. Ad esempio,
      specifica il seguente parametro per i dati audio a due canali
      catturati a 16 kHz:<br/><br/>
      <code>audio/l16;rate=16000;channels=2</code><br/><br/>
      Il servizio accetta un massimo di 16 canali. Esegue il downmix dell'audio
      su un solo canale durante la transcodifica.
    </td>
  </tr>
  <tr>
    <td>
      <code>endianness</code><br/><em>Facoltativo</em>
    </td>
    <td>
      Per impostazione predefinita, il servizio rileva automaticamente la proprietà endian dell'audio
      in entrata. Tuttavia, il rilevamento automatico può a volte non riuscire e rilasciare la
      connessione per un audio breve in formato `audio/l16`. La specifica
      della proprietà endian disabilita il rilevamento automatico. Specifica
      <code>big-endian</code> o <code>little-endian</code>. Ad
      esempio, specifica il seguente parametro per i dati audio
      catturati a 16 kHz in formato little-endian:<br/><br/>
      <code>audio/l16;rate=16000;endianness=little-endian</code><br/><br/>
      La sezione 5.1 di
      [Request for Comment (RFC) 2045](https://tools.ietf.org/html/rfc2045#section-5.1)
      specifica il formato big-endian per i dati <code>audio/l16</code>, ma
      molte persone utilizzano il formato little-endian.
    </td>
  </tr>
</table>

Per ulteriori informazioni, vedi IETF [Request for Comment (RFC) 2586](https://tools.ietf.org/html/rfc2586){: external} e [Pulse-code modulation](https://wikipedia.org/wiki/Pulse-code_modulation){: external}.

### Formati audio/mp3 e audio/mpeg
{: #mp3}

*MP3* (`audio/mp3`) o *MPEG (Motion Picture Experts Group)* (`audio/mpeg`) è un formato audio con perdita. (MP3 e MPEG si riferiscono allo stesso formato.) Per ulteriori informazioni, vedi [MP3](https://wikipedia.org/wiki/MP3){: external}.

### Formato audio/mulaw
{: #mulaw}

*Mu-law* (`audio/mulaw`) è un formato audio con perdita a canale singolo. I dati vengono codificati utilizzando l'algoritmo u-law (o mu-law). Il formato `audio/basic` è un formato equivalente che viene sempre campionato a 8 kHz. Quando utilizzi questo formato, il servizio richiede un parametro aggiuntivo sulla specifica del formato.

<table>
  <caption>Tabella 4. Parametro per il formato `audio/mulaw`</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Parametro
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Descrizione
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Obbligatorio</em>
    </td>
    <td>
      Un numero intero che specifica la frequenza di campionamento a cui viene
      catturato l'audio. Ad esempio, specifica il seguente parametro per i dati
      audio catturati a 8 kHz:<br/><br/>
      <code>audio/mulaw;rate=8000</code>
    </td>
  </tr>
</table>

Per ulteriori informazioni, vedi [M-law algorithm](https://wikipedia.org/wiki/M-law_algorithm){: external}.

### Formato audio/ogg
{: #ogg}

*Ogg* (`audio/ogg`) è un formato contenitore aperto gestito da Xiph.org Foundation ([xiph.org/ogg](https://www.xiph.org/ogg){: external}). Puoi utilizzare flussi audio compressi con i seguenti codec con perdita:

-   *Opus* (`audio/ogg;codecs=opus`). Per ulteriori informazioni, vedi [opus-codec.org](https://www.opus-codec.org/){: external} e [Opus (audio format)](https://wikipedia.org/wiki/Opus_%28audio_format%29){: external}. Consulta in particolare la sezione *Containers*.
-   *Vorbis* (`audio/ogg;codecs=vorbis`). Per ulteriori informazioni, vedi [xiph.org/vorbis](https://xiph.org/vorbis/){: external} e [Vorbis](https://wikipedia.org/wiki/Vorbis){: external}.

Se ometti il codec, il servizio lo rileva automaticamente dall'audio di input. Opus è il codec preferito; è standardizzato da IETF (Internet Engineering Task Force) come [Request for Comment (RFC) 6716](https://tools.ietf.org/html/rfc6716){: external}.

### Formato audio/wav
{: #wav}

*WAV (Waveform Audio File Format)* (`audio/wav`) è un formato contenitore che viene spesso utilizzato per flussi audio non compressi, ma può contenere anche audio compresso. Il servizio supporta l'audio WAV che utilizza qualsiasi codifica. Accetta l'audio WAV con un massimo di nove canali (a causa di una limitazione FFmpeg).

Per ulteriori informazioni sul formato WAV, vedi [WAV](https://wikipedia.org/wiki/WAV){: external}. Per ulteriori informazioni sulla riduzione della dimensione dell'audio WAV convertendolo nel codec Opus, vedi [Conversione in audio/ogg con il codec Opus](#conversionOgg).

### Formato audio/webm
{: #webm}

*Web Media (WebM)* (`audio/webm`) è un formato contenitore aperto gestito da WebM Project ([webmproject.org](https://www.webmproject.org/){: external}). Puoi utilizzare flussi audio compressi con i seguenti codec con perdita:

-   *Opus* (`audio/webm;codecs=opus`). Per ulteriori informazioni, vedi [opus-codec.org](https://www.opus-codec.org/){: external} e [Opus (audio format)](https://wikipedia.org/wiki/Opus_%28audio_format%29){: external}. Consulta in particolare la sezione *Containers*.
-   *Vorbis* (`audio/webm;codecs=vorbis`). Per ulteriori informazioni, vedi [xiph.org/vorbis](https://xiph.org/vorbis/){: external} e [Vorbis](https://wikipedia.org/wiki/Vorbis){: external}.

Se ometti il codec, il servizio lo rileva automaticamente dall'audio di input.

Per il codice JavaScript che mostra come catturare l'audio da un microfono in un browser Chrome e codificarlo in un flusso di dati WebM, vedi [jsbin.com/hedujihuqo/edit?js,console](https://jsbin.com/hedujihuqo/edit?js,console){: external}. Il codice non inoltra l'audio catturato al servizio.
{: tip}

## Limiti e compressione dei dati
{: #limits}

Il servizio accetta un massimo di 100 MB di dati audio per la trascrizione con una richiesta WebSocket o HTTP sincrona. Quando riconosci lunghi flussi audio continui o file di grandi dimensioni, attieniti alla seguente procedura per assicurarti che l'audio non superi il limite di 100 MB di dati:

-   Utilizza una frequenza di campionamento non superiore a 16 kHz (per i modelli a banda larga) o a 8 kHz (per i modelli a banda stretta) e utilizza 16 bit per ogni campione. Il servizio converte l'audio che è registrato a una frequenza di campionamento superiore al modello di destinazione (16 kHz o 8 kHz) alla frequenza del modello. Quindi frequenze più grandi non producono una maggiore accuratezza del riconoscimento, ma aumentano le dimensioni del flusso audio.
-   Codifica il tuo audio in un formato che offre la compressione dei dati. Codificando i tuoi dati in modo più efficiente, puoi inviare molto più audio senza superare il limite di 100 MB. I formati audio come `audio/ogg` e `audio/mp3` riducono significativamente le dimensioni del tuo flusso audio. Puoi utilizzare questi formati per inviare quantità maggiori di audio con una singola richiesta.

Se comprimi troppo l'audio con il formato `audio/ogg`, potresti ridurre l'accuratezza del riconoscimento. Per sicurezza, mantieni una velocità di bit di almeno 24 kbps per il tuo audio.

### Confronto tra dimensioni audio approssimative
{: #compareSizes}

Considera la dimensione approssimativa del flusso di dati che risulta da 2 ore di trasmissione vocale continua campionata a 16 kHz e a 16 bit per ogni campione:

-   Se i dati sono codificati con il formato `audio/wav`, il flusso di due ore ha una dimensione di 230 MB, ben oltre il limite di 100 MB del servizio.
-   Se i dati sono codificati nel formato `audio/ogg`, il flusso di due ore ha una dimensione di soli 23 MB, ben al di sotto del limite del servizio.

La seguente tabella approssima la durata massima dell'audio che può essere inviato per il riconoscimento vocale con una richiesta HTTP sincrona o WebSocket in diversi formati. La durata considera il limite del servizio di 100 MB. I valori effettivi possono variare in base alla complessità dell'audio e alla velocità di compressione raggiunta.

<table style="width:75%">
  <caption>Tabella 5. Durata massima dell'audio in diversi formati</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 50%">
      Formato audio
    </th>
    <th style="text-align:center; vertical-align:bottom; width 50%">
      Durata massima dell'audio (approssimativa)
    </th>
  </tr>
  <tr>
    <td><code>audio/wav</code></td>
    <td style="text-align:center">55 minuti</td>
  </tr>
  <tr>
    <td><code>audio/flac</code></td>
    <td style="text-align:center">1 ora 40 minuti</td>
  </tr>
  <tr>
    <td><code>audio/mp3</code></td>
    <td style="text-align:center">3 ore 20 minuti</td>
  </tr>
  <tr>
    <td><code>audio/ogg</code></td>
    <td style="text-align:center">8 ore 40 minuti</td>
  </tr>
</table>

I formati `audio/ogg;codecs=opus` e `audio/webm;codecs=opus` sono generalmente equivalenti e le loro dimensioni sono quasi identiche. Utilizzano internamente lo stesso codec; solo il formato contenitore è diverso.

## Conversione audio
{: #conversion}

Puoi utilizzare vari strumenti per convertire l'audio in un formato diverso. Gli strumenti possono essere utili quando l'audio è in un formato non supportato dal servizio o in un formato non compresso o senza perdita. In quest'ultimo caso, puoi convertire l'audio in un formato con perdita per ridurne le dimensioni.

Sono disponibili i seguenti strumenti gratuiti per convertire il tuo audio da un formato all'altro:

-   Sound eXchange (SoX) ([sox.sourceforge.net](http://sox.sourceforge.net){: external})
-   FFmpeg ([ffmpeg.org](https://www.ffmpeg.org){: external})
-   Audacity&reg; ([audacityteam.org](http://www.audacityteam.org/){: external})
-   Per il formato Ogg con il codec Opus, **opus-tools** ([opus-codec.org/downloads/](http://opus-codec.org/downloads/){: external})

Questi strumenti offrono un supporto multipiattaforma per più formati audio. Inoltre, puoi utilizzare molti degli strumenti per riprodurre l'audio. Non utilizzare gli strumenti per violare le leggi applicabili sul copyright.

### Conversione in audio/ogg con il codec Opus
{: #conversionOgg}

Gli [**opus-tools**](http://opus-codec.org/downloads/){: external} includono tre programmi di utilità da riga di comando per lavorare con l'audio Ogg nel codec Opus:

-   Il programma di utilità [**opusenc**](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusenc.html){: external} codifica l'audio da WAV, FLAC e altri formati in Ogg con il codec Opus. La pagina mostra come comprimere i flussi audio. La compressione è utile per passare l'audio in tempo reale al servizio.
-   Il programma di utilità [**opusdec**](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusdec.html){: external} decodifica l'audio dal codec Opus in file WAV PCM non compressi.
-   Il programma di utilità [**opusinfo**](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusinfo.html){: external} fornisce informazioni e controllo di validità per i file Opus.

Molti utenti inviano file WAV per il riconoscimento vocale. Con il limite di dati di 100 MB del servizio per le richieste WebSocket e HTTP sincrone, il formato WAV riduce la quantità di audio che può essere riconosciuto con una singola richiesta. Utilizzando il comando **opusenc** per convertire l'audio nel formato `audio/ogg:codecs=opus` preferito puoi aumentare notevolmente la quantità di audio che puoi inviare con una richiesta di riconoscimento.

Ad esempio, considera un file WAV (**input.wav**) a banda larga (16 kHz) non compresso che utilizza 16 bit per campione per una velocità di bit di 256 kbps. Il seguente comando converte l'audio in un file (**output.opus**) che utilizza il codec Opus:

```bash
opusenc input.wav output.opus
```
{: pre}

La conversione comprime l'audio di un fattore quattro e produce un file di output con una velocità di bit di 64 kbps. Tuttavia, in base alle [impostazioni consigliate Opus](https://wiki.xiph.org/Opus_Recommended_Settings){: external}, puoi ridurre in modo sicuro la velocità di bit a 24 kbps e mantenere comunque una banda completa per l'audio del discorso. Questa riduzione comprime l'audio di un fattore 10. Il seguente comando utilizza l'opzione `--bitrate` per produrre un file di output con una velocità di bit di 24 kbps:

```bash
opusenc --bitrate 24 input.wav output.opus
```
{: pre}

La compressione con il programma di utilità **opusenc** è veloce. La compressione avviene a una velocità circa 100 volte più veloce del tempo reale. Al termine, il comando scrive l'output sulla console che fornisce dettagli completi relativi al suo tempo di esecuzione e ai dati audio risultanti.

## Suggerimenti per migliorare il riconoscimento vocale
{: #audioTips}

I seguenti suggerimenti possono aiutarti a migliorare la qualità del riconoscimento vocale:

-   Il modo in cui registri l'audio può fare una grande differenza nei risultati del servizio. Il riconoscimento vocale può essere molto sensibile alla qualità dell'audio di input. Per ottenere la massima accuratezza, assicurati che la qualità dell'audio di input sia la migliore possibile.
    -   Utilizza un microfono orientato alla voce diretta (ad esempio quello degli auricolari) quando possibile e regola le impostazioni del microfono secondo necessità. Il servizio funziona al meglio quando vengono utilizzati microfoni professionali per catturare l'audio.
    -   Evita di utilizzare il microfono incorporato di un sistema. I microfoni che sono in genere installati su dispositivi mobili e tablet sono spesso inadeguati.
    -   Assicurati che i parlanti siano vicini ai microfoni. L'accuratezza diminuisce quando un parlante si allontana da un microfono. Ad esempio, a una distanza di 3 metri, il servizio non è in grado di produrre risultati adeguati.
-   Il riconoscimento vocale è sensibile al rumore di fondo e alle sfumature della voce umana.
    -   I rumori provenienti da motori, da dispositivi di lavoro, dalla strada e da conversazioni di fondo possono ridurre in modo significativo l'accuratezza del riconoscimento.
    -   Anche gli accenti regionali e le differenze di pronuncia possono ridurre l'accuratezza.

    Se il tuo audio ha queste caratteristiche, prendi in considerazione l'utilizzo della personalizzazione del modello acustico per migliorare l'accuratezza del riconoscimento vocale. Per ulteriori informazioni, vedi [L'interfaccia di personalizzazione](/docs/services/speech-to-text?topic=speech-to-text-customization).
