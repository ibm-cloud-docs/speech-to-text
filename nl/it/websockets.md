---

copyright:
  years: 2015, 2019
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

# L'interfaccia WebSocket
{: #websockets}

L'interfaccia WebSocket del servizio {{site.data.keyword.speechtotextshort}} è il modo più naturale per un client di interagire con il servizio. Per utilizzare l'interfaccia WebSocket per il riconoscimento vocale, devi prima utilizzare il metodo `/v1/recognize` per stabilire una connessione persistente con il servizio. Successivamente invia i messaggi di testo e binari tramite la connessione per avviare e gestire le richieste di riconoscimento.
{: shortdesc}

Il ciclo di richiesta e risposta del riconoscimento ha le seguenti fasi:

1.  [Apri una connessione](#WSopen)
1.  [Avvia una richiesta di riconoscimento](#WSstart)
1.  [Invia l'audio e ricevi i risultati del riconoscimento](#WSaudio)
1.  [Termina una richiesta di riconoscimento](#WSstop)
1.  [Invia ulteriori richieste e modifica i parametri della richiesta](#WSmore)
1.  [Mantieni una connessione attiva](#WSkeep)
1.  [Chiudi una connessione](#WSclose)

Quando il client invia i dati al servizio, *deve* passare tutti i messaggi JSON come messaggi di testo e tutti i dati audio come messaggi binari.

I frammenti di codice di esempio che seguono sono scritti in JavaScript e sono basati sull'API WebSocket HTML5. Per ulteriori informazioni sul protocollo WebSocket, vedi IETF (Internet Engineering Task Force) [Request for Comment (RFC) 6455](http://tools.ietf.org/html/rfc6455){: external}.
{: note}

## Apri una connessione
{: #WSopen}

Il servizio {{site.data.keyword.speechtotextshort}} utilizza il protocollo WSS (WebSocket Secure) per rendere il metodo `/v1/recognize` disponibile al seguente endpoint:

```
wss://{host_name}/speech-to-text/api/v1/recognize
```
{: codeblock}

dove `{host_name}` è l'ubicazione in cui è ospitata la tua applicazione:

-   `stream.watsonplatform.net` per Dallas (gli esempi riportati di seguito utilizzano questo nome host)
-   `stream-fra.watsonplatform.net` per Francoforte
-   `gateway-syd.watsonplatform.net` per Sydney
-   `gateway-wdc.watsonplatform.net` per Washington, DC
-   `gateway-tok.watsonplatform.net` per Tokyo
-   `gateway-lon.watsonplatform.net` per Londra

Un client WebSocket richiama questo metodo con i seguenti parametri di query per stabilire una connessione autenticata con il servizio. Se utilizzi l'autenticazione IAM (Identity and Access Management), utilizza il parametro di query `access_token`. Se utilizzi le credenziali del servizio Cloud Foundry, utilizza il parametro di query `watson-token`.

<table>
  <caption>Tabella 1. Parametri del metodo <code>/v1/recognize</code></caption>
  <tr>
    <th style="width:23%; text-align:left">Parametro</th>
    <th style="width:12%; text-align:center">Tipo di dati</th>
    <th style="text-align:left">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>access_token</code>
      <br/><em>      Facoltativo
    </em></td>
    <td style="text-align:center">Stringa</td>
    <td style="text-align:left">
      <em>Se utilizzi l'autenticazione IAM</em> passa un token di accesso IAM valido
      per stabilire una connessione autenticata con il servizio.
      Passa un token di accesso IAM invece di una chiave API con
      la chiamata. Devi stabilire la connessione prima di che il token
      di accesso scada. Per ulteriori informazioni su come ottenere un token di accesso, vedi
      [Autenticazione con i token IAM](/docs/services/watson?topic=watson-iam).<br/><br/>
      Passa un token di accesso solo per stabilire una connessione autenticata.
      Dopo aver stabilito una connessione, puoi mantenerla attiva a tempo indefinito.
      Rimani autenticato finché mantieni la connessione aperta.
      Non devi aggiornare il token di accesso per una connessione attiva
      che va oltre l'ora di scadenza del token. Una connessione può rimanere
      attiva anche dopo l'eliminazione del token o della sua chiave API.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>watson-token</code>
      <br/><em>      Facoltativo
    </em></td>
    <td style="text-align:center">Stringa</td>
    <td style="text-align:left">
      <em>Se utilizzi le credenziali del servizio Cloud Foundry,</em> passa un token di autenticazione
      {{site.data.keyword.watson}} valido per stabilire una connessione
      autenticata con il servizio. Passa un token
      {{site.data.keyword.watson}} invece delle credenziali del
      servizio con la chiamata. I token {{site.data.keyword.watson}} si basano
      sulle credenziali del servizio Cloud Foundry, che utilizzano `username`
      e `password` per l'autenticazione di base HTTP. Per informazioni su
      come ottenere un token {{site.data.keyword.watson}}, vedi
      [Token {{site.data.keyword.watson}}](/docs/services/watson?topic=watson-gs-tokens-watson-tokens).<br/><br/>
      Passa un token {{site.data.keyword.watson}} solo per stabilire
      una connessione autenticata. Dopo aver stabilito una connessione, puoi mantenerla attiva a tempo indefinito. Rimani autenticato finché mantieni la connessione aperta.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>model</code>
      <br/><em>      Facoltativo
    </em></td>
    <td style="text-align:center">Stringa</td>
    <td style="text-align:left">
      Specifica il modello di lingua che deve essere utilizzato per la trascrizione.
      Se non specifichi un modello, il servizio utilizza il modello
      <code>en-US_BroadbandModel</code> per impostazione predefinita. Per ulteriori informazioni,
      vedi
      [Lingue e modelli](/docs/services/speech-to-text?topic=speech-to-text-models).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>language_customization_id</code>
      <br/><em>      Facoltativo
    </em></td>
    <td style="text-align:center">Stringa</td>
    <td style="text-align:left">
      Specifica il GUID (globally unique identifier) di un modello
      di lingua personalizzato che deve essere utilizzato per tutte le richieste inviate
      sulla connessione. Il modello di base di un modello di lingua personalizzato
      deve corrispondere al valore del parametro <code>model</code>. Se
      includi un ID di personalizzazione, devi effettuare la richiesta con le credenziali
      per l'istanza del servizio a cui appartiene il modello personalizzato. Per impostazione predefinita,
      non viene utilizzato alcun modello di lingua personalizzato. Per ulteriori informazioni, vedi
      [L'interfaccia di personalizzazione](/docs/services/speech-to-text?topic=speech-to-text-customization).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>acoustic_customization_id</code>
      <br/><em>      Facoltativo
    </em></td>
    <td style="text-align:center">Stringa</td>
    <td style="text-align:left">
      Specifica il GUID (globally unique identifier) di un modello
      acustico personalizzato che deve essere utilizzato per tutte le richieste inviate
      sulla connessione. Il modello di base di un modello acustico personalizzato
      deve corrispondere al valore del parametro <code>model</code>. Se
      includi un ID di personalizzazione, devi effettuare la richiesta con le credenziali
      per l'istanza del servizio a cui appartiene il modello personalizzato. Per impostazione predefinita,
      non viene utilizzato alcun modello acustico personalizzato. Per ulteriori informazioni, vedi
      [L'interfaccia di personalizzazione](/docs/services/speech-to-text?topic=speech-to-text-customization).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>base_model_version</code>
      <br/><em>      Facoltativo
    </em></td>
    <td style="text-align:center">Stringa</td>
    <td style="text-align:left">
      Specifica la versione del modello (`model`) di base che deve essere utilizzato
      per tutte le richieste inviate tramite la connessione. Il parametro è concepito
      principalmente per l'utilizzo con i modelli personalizzati che sono stati aggiornati a
      un nuovo modello di base. Il valore predefinito dipende dal fatto che il parametro sia utilizzato
      con o senza un modello personalizzato. Per ulteriori informazioni, vedi
      [Versione del modello di base](/docs/services/speech-to-text?topic=speech-to-text-input#version).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-learning-opt-out</code>
      <br/><em>      Facoltativo
    </em></td>
    <td style="text-align:center">Booleano</td>
    <td style="text-align:left">
      Indica se le richieste dei log di servizio e i risultati vengono inviati
      tramite la connessione. Per impedire a IBM di accedere ai tuoi dati per miglioramenti generali del servizio,
      specifica <code>true</code> per il parametro. Per ulteriori informazioni,
      vedi
      [Registrazione delle richieste](/docs/services/speech-to-text?topic=speech-to-text-input#logging).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-metadata</code>
      <br/><em>      Facoltativo
    </em></td>
    <td style="text-align:center">Stringa</td>
    <td style="text-align:left">
      Associa un ID cliente a tutti i dati passati tramite
      la connessione. Il parametro accetta l'argomento
      <code>customer_id={id}</code>, dove <code>id</code> è una stringa
      casuale o generica che deve essere associata ai dati. Devi
      codificare in URL l'argomento nel parametro, ad esempio,
      `customer_id%3dmy_customer_ID`. Per impostazione predefinita, nessun ID cliente è associato
      ai dati. Per ulteriori informazioni, vedi
      [Sicurezza delle informazioni](/docs/services/speech-to-text?topic=speech-to-text-information-security).
    </td>
  </tr>
</table>

Il seguente frammento di codice JavaScript apre una connessione con il servizio. La chiamata al metodo `/v1/recognize` passa i parametri di query `access_token` e `model`, per indirizzare il servizio all'utilizzo del modello a banda larga per lo spagnolo. Dopo aver stabilito la connessione, il client definisce i listener di eventi (`onOpen`, `onClose` e così via) per rispondere agli eventi dal servizio. Il client può utilizzare la connessione per più richieste di riconoscimento.

```javascript
var IAM_access_token = '{access_token}';
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token
  + '&model=es-ES_BroadbandModel';
var websocket = new WebSocket(wsURI);
websocket.onopen = function(evt) { onOpen(evt) };
websocket.onclose = function(evt) { onClose(evt) };
websocket.onmessage = function(evt) { onMessage(evt) };
websocket.onerror = function(evt) { onError(evt) };
```
{: codeblock}

Il client può aprire più connessioni WebSocket simultanee al servizio. Il numero di connessioni simultanee è limitato solo dalla capacità del servizio, che generalmente non implica problemi per gli utenti.

## Avvia una richiesta di riconoscimento
{: #WSstart}

Per avviare una richiesta di riconoscimento, il client invia un messaggio di testo JSON al servizio sulla connessione stabilita. Il client deve inviare questo messaggio prima di inviare dell'audio per la trascrizione. Il messaggio deve includere il parametro `action` ma di solito può omettere il parametro `content-type`.

<table>
  <caption>Tabella 2. Parametri del messaggio di testo JSON</caption>
  <tr>
    <th style="width:23%; text-align:left">Parametro</th>
    <th style="width:12%; text-align:center">Tipo di dati</th>
    <th style="text-align:left">Descrizione</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>action</code><br/><em>      Obbligatorio
    </em></td>
    <td style="text-align:center">Stringa</td>
    <td style="text-align:left">
      Specifica l'azione che deve essere eseguita:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          <code>start</code> avvia una richiesta di riconoscimento o specifica
          dei nuovi parametri per le richieste successive. Per ulteriori informazioni, vedi
          [Invia ulteriori richieste e modifica i parametri della richiesta](#WSmore).
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>stop</code> indica che tutto l'audio di una richiesta
          è stato inviato. Per ulteriori informazioni, vedi
          [Termina una richiesta di riconoscimento](#WSstop).
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>content-type</code><br/><em>      Facoltativo
    </em></td>
    <td style="text-align:center">Stringa</td>
    <td style="text-align:left">
      Identifica il formato (tipo MIME) dei dati audio per la richiesta.
      Il parametro è obbligatorio per i formati `audio/alaw`, `audio/basic`,
      `audio/l16` e `audio/mulaw`. Per ulteriori informazioni, vedi
      [Formati audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).
    </td>
  </tr>
</table>

Il messaggio può anche includere dei parametri facoltativi per specificare altri aspetti di come la richiesta deve essere elaborata e le informazioni che devono essere restituite. Per informazioni su tutte le funzioni di input e output, vedi il [Riepilogo dei parametri](/docs/services/speech-to-text?topic=speech-to-text-summary). Puoi specificare un modello di lingua, un modello di lingua personalizzato e un modello acustico personalizzato solo come dei parametri di query dell'URL WebSocket.

Il seguente frammento di codice JavaScript invia i parametri di inizializzazione per la richiesta di riconoscimento sulla connessione WebSocket. Le chiamate vengono incluse nella funzione `onOpen` del client per garantire che siano inviate soltanto dopo che viene stabilita la connessione.

```javascript
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050'
  };
  websocket.send(JSON.stringify(message));
}
```
{: codeblock}

Se la richiesta viene ricevuta correttamente, il servizio restituisce il seguente messaggio di testo per indicare che è in ascolto (`listening`):

```javascript
{'state': 'listening'}
```
{: codeblock}

Lo stato `listening` indica che l'istanza del servizio è configurata (il tuo messaggio `start` JSON era valido) ed è pronta ad elaborare una nuova espressione per una richiesta di riconoscimento. Una volta che è in ascolto, il servizio elabora tutto l'audio che è stato inviato prima del messaggio `listening`.

Se il client specifica un parametro di query o un campo JSON non valido per la richiesta di riconoscimento, la risposta JSON del servizio include un campo `warnings`. Il campo descrive tutti gli argomenti non validi. La richiesta ha esito positivo nonostante le avvertenze.

## Invia l'audio e ricevi i risultati del riconoscimento
{: #WSaudio}

Dopo aver inviato il messaggio `start` iniziale, il client può iniziare ad inviare i dati audio al servizio. Il client non deve attendere che il servizio risponda al messaggio `start` con il messaggio `listening`. Il servizio restituisce i risultati della trascrizione in modo asincrono nello stesso formato in cui restituisce i risultati per le interfacce HTTP.

Il client deve inviare l'audio come dati binari. Il client può inviare un massimo di 100 MB di audio con una sola espressione (per richiesta `send`). Deve inviare almeno 100 byte di audio per ogni richiesta. Il client può inviare più espressioni su una sola connessione WebSocket. Per informazioni sull'utilizzo della compressione per massimizzare la quantità di audio che puoi passare al servizio con una richiesta, vedi [Formati audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).

L'interfaccia WebSocket impone una dimensione del frame massima di 4 MB. Il client può impostare la dimensione del frame massima su un valore inferiore a 4 MB. Se non è pratico impostare la dimensione del frame, il client può impostare la dimensione del messaggio massima su un valore inferiore di 4 MB e inviare i dati audio come una sequenza di messaggi. Per ulteriori informazioni sui frame WebSocket, vedi [IETF RFC 6455](http://tools.ietf.org/html/rfc6455){: external}.

Il seguente frammento di codice JavaScript invia i dati audio al servizio come un messaggio binario (blob):

```javascript
websocket.send(blob);
```
{: codeblock}

Il seguente frammento riceve le ipotesi di riconoscimento che il servizio restituisce in modo asincrono. I risultati sono gestiti dalla funzione `onMessage` del client.

```javascript
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

## Termina una richiesta di riconoscimento
{: #WSstop}

Quando si finisce di inviare dati audio per una richiesta al servizio, il client *deve* segnalare il termine della trasmissione dell'audio binario al servizio in uno dei seguenti modi:

-   Inviando un messaggio di testo JSON con il parametro `action` impostato sul valore `stop`:

    ```javascript
    {action: 'stop'}
    ```
    {: codeblock}

-   Inviando un messaggio binario vuoto, uno in cui il blob specificato è vuoto:

    ```javascript
    websocket.send(blob)
    ```
    {: codeblock}

Il servizio non invia i risultati finali finché non riceve conferma che la trasmissione audio è stata completata. Se non riesci a segnalare che la trasmissione è stata completata, la connessione può andare in timeout senza che il servizio invii i risultati finali.

Per ricevere i risultati finali tra più richieste di riconoscimento, il client deve indicare il termine della trasmissione per la richiesta precedente prima di inviare una richiesta successiva. Dopo aver restituito i risultati finali per la prima richiesta, il servizio restituisce un altro messaggio `{"state":"listening"}` al client. Questo messaggio indica che il servizio è pronto a ricevere un'altra richiesta.

## Invia ulteriori richieste e modifica i parametri della richiesta
{: #WSmore}

Mentre la connessione WebSocket è attiva, il client può continuare ad utilizzarla per inviare ulteriori richieste di riconoscimento con del nuovo audio. Per impostazione predefinita, il servizio continua ad utilizzare i parametri che sono stati inviati con il precedente messaggio `start` per tutte le richieste successive che vengono inviate tramite la stessa connessione.

Per modificare i parametri per le successive richieste, il client può inviare un altro messaggio `start` con i nuovi parametri dopo aver ricevuto i risultati del riconoscimento finali e il messaggio `{"state":"listening"}` dal servizio. Il client può modificare tutti i parametri ad eccezione di quelli che vengono specificati quando viene aperta la connessione (`model`, `language_customization_id` e così via).

Il seguente esempio invia un messaggio `start` con dei nuovi parametri per le successive richieste di riconoscimento che vengono inviate tramite la connessione. Il messaggio specifica lo stesso `content-type` dell'esempio precedente, ma indica al servizio di restituire le date/ore e le misure dell'attendibilità per le parole della trascrizione.

```javascript
var message = {
  action: 'start',
  content-type: 'audio/l16;rate=22050',
  word_confidence: true,
  timestamps: true
};
websocket.send(JSON.stringify(message));
```
{: codeblock}

## Mantieni una connessione attiva
{: #WSkeep}

Il servizio termina la sessione e chiude la connessione se si verifica un timeout di inattività o di sessione:

-   Un *timeout di inattività* si verifica se l'audio è stato inviato dal client ma il servizio non rileva alcun discorso. Il timeout di inattività è di 30 secondi per impostazione predefinita. Puoi utilizzare il parametro `inactivity_timeout` per specificare un valore diverso, incluso `-1` per impostare il timeout su infinito. Per ulteriori informazioni, vedi [Timeout di inattività](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-inactivity).
-   Un *timeout di sessione* si verifica se il servizio non riceve dati dal client o non invia dei risultati provvisori per 30 secondi. Non puoi modificare la durata di questo timeout, ma puoi estendere la sessione inviando al servizio dei dati audio, incluso soltanto il silenzio, prima che si verifichi il timeout. Devi inoltre impostare `inactivity_timeout` su `-1`. Ti viene effettuato un addebito per la durata di tutti i dati che invii al servizio, incluso il silenzio che invii per estendere la sessione. Per ulteriori informazioni, vedi [Timeout della sessione](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-session).

I client e i server WebSocket possono anche scambiare dei *frame ping-pong* per evitare timeout di lettura scambiando periodicamente piccole quantità di dati. Molti stack WebSocket si scambiano i frame ping-pong, ma alcuni non lo fanno. Per determinare se la tua implementazione utilizza i frame ping-pong, controlla la sua lista di funzioni. Non puoi determinare o gestire in modo programmatico i frame ping-pong.

Se il tuo stack WebSocket non implementa frame ping-pong e stai inviando file audio lunghi, si potrebbe verificare un timeout di lettura sulla tua connessione. Per evitare tali timeout, invia continuamente l'audio in streaming al servizio o richiedi i risultati provvisori dal servizio. Entrambi gli approcci possono garantire che la mancanza di frame ping-pong non causi la chiusura della tua connessione.

Per ulteriori informazioni sui frame ping-pong, vedi [Section 5.5.2 Ping](http://tools.ietf.org/html/rfc6455#section-5.5.2){: external} e [Section 5.5.3 Pong](http://tools.ietf.org/html/rfc6455#section-5.5.3){: external} di IETF RFC 6455.

## Chiudi una connessione
{: #WSclose}

Quando il client ha terminato di interagire con il servizio, può chiudere la connessione WebSocket. Dopo aver chiuso la connessione, il client non può più utilizzarla per inviare delle richieste o per ricevere dei risultati. Chiudi la connessione soltanto dopo aver ricevuto tutti i risultati per una richiesta. Infine la connessione può andare in timeout e venire chiusa se non la chiudi in modo esplicito.

Il seguente frammento di codice JavaScript chiude una connessione aperta:

```javascript
websocket.close();
```
{: codeblock}

## Codici di ritorno WebSocket
{: #WSreturn}

Il servizio può inviare i seguenti codici di ritorno al client tramite la connessione WebSocket:

-   `1000` indica la normale chiusura della connessione, il che significa che lo scopo per il quale la connessione è stata stabilita è stato soddisfatto.
-   `1002` indica che il servizio sta chiudendo la connessione a causa di un errore di protocollo.
-   `1006` indica che la connessione è stata chiusa in modo anomalo.
-   `1009` indica che la dimensione del frame ha superato il limite di 4 MB.
-   `1011` indica che il servizio sta terminando la connessione perché ha rilevato una condizione non prevista che gli impedisce di soddisfare la richiesta.

Se il socket viene chiuso con un errore, il client riceve un messaggio informativo del formato `{"error":"{message}"}` prima della chiusura del socket. Per ulteriori informazioni sui codici di ritorno WebSocket, vedi [IETF RFC 6455](http://tools.ietf.org/html/rfc6455){: external}.

## Sessione WebSocket di esempio
{: #WSexample}

Il seguente esempio mostra una sessione WebSocket tra un client e il servizio {{site.data.keyword.speechtotextshort}}. La sessione viene mostrata come tre scambi separati per renderla più semplice da seguire. Ma tutti e tre gli scambi fanno parte di una sola sessione con il servizio. L'esempio di concentra sullo scambio di messaggi; non mostra l'apertura e la chiusura della connessione.

### Primo scambio di esempio
{: #firstExample}

Nel primo scambio, il client invia dell'audio che contiene la stringa `Name the Mayflower`. Il client invia un messaggio binario con una sola porzione di dati audio (`audio/l16`) PCM, per i quali indica la frequenza di campionamento richiesta. Il client non attende la risposta `{"state":"listening"}` dal servizio per iniziare ad inviare i dati audio e per indicare il termine della richiesta. L'invio dei dati riduce immediatamente la latenza perché l'audio è disponibile per il servizio non appena è pronto a gestire una richiesta di riconoscimento.

-   Il *client* invia:

    ```javascript
    {
      "action": "start",
      "content-type": "audio/l16;rate=22050"
    }
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   Il *servizio* risponde:

    ```javascript
    {"state": "listening"}
    {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### Secondo scambio di esempio
{: #secondExample}

Nel secondo scambio, il client invia dell'audio che contiene la stringa `Second audio transcript`. Il client invia l'audio in un solo messaggio binario e utilizza gli stessi parametri specificati nella prima richiesta.

-   Il *client* invia:

    ```javascript
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   Il *servizio* risponde:

    ```javascript
    {"results": [{"alternatives": [{"transcript": "second audio transcript "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### Terzo scambio di esempio
{: #thirdExample}

Nel terzo scambio, il client invia nuovamente dell'audio che contiene la stringa `Name the Mayflower`. Il client invia nuovamente un messaggio binario con una sola porzione di dati audio PCM. Ma questa volta, il client richiede i risultati provvisori con la trascrizione.

-   Il *client* invia:

    ```javascript
    {
      "action": "start",
      "content-type": "audio/l16;rate=22050",
      "interim_results": true
    }
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   Il *servizio* risponde:

    ```javascript
    {"state":"listening"}
    {"results": [{"alternatives": [{"transcript": "name "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name may "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name may flour "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}
