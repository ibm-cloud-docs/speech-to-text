---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

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

# L'interfaccia HTTP asincrona
{: #async}

L'interfaccia HTTP asincrona del servizio {{site.data.keyword.speechtotextfull}} fornisce metodi per la trascrizione dell'audio tramite chiamate non bloccanti al servizio. L'interfaccia utilizza stringhe segrete e firme digitali specificate dall'utente per fornire un livello di sicurezza per le richieste effettuate tramite il protocollo HTTP. Per utilizzare l'interfaccia asincrona, puoi
{: shortdesc}

-   Registrare un URL di callback in modo che il servizio notifichi automaticamente lo stato del lavoro e i risultati.
-   Eseguire il polling del servizio per ottenere manualmente lo stato del lavoro e i risultati.

I due approcci non si escludono a vicenda. Puoi scegliere di ricevere le notifiche di callback ma anche di eseguire il polling del servizio per lo stato più recente oppure contattare il servizio per richiamare i risultati manualmente. Le seguenti sezioni descrivono come utilizzare l'interfaccia HTTP asincrona con entrambi gli approcci.

Inoltra un massimo di 1 GB e un minimo di 100 byte di dati audio con una singola richiesta. Per informazioni sui formati audio e sull'utilizzo della compressione per massimizzare la quantità di audio che puoi inviare con una richiesta, vedi [Formati audio](/docs/services/speech-to-text/audio-formats.html). Per ulteriori informazioni sui singoli metodi dell'interfaccia, vedi la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

## Modelli di utilizzo
{: #usage}

Quando lavori con l'interfaccia HTTP asincrona del servizio, puoi scegliere di conoscere lo stato del lavoro e di ricevere i risultati nei seguenti modi:

-   Utilizzando le notifiche di callback:
    1.  Chiama il metodo `POST /v1/register_callback` per registrare un URL di callback con il servizio. Puoi fornire una stringa segreta facoltativa specificata dall'utente per abilitare l'autenticazione e l'integrità dei dati per i callback inviati all'URL.
    1.  Chiama il metodo `POST /v1/recognitions` con un URL di callback già registrato a cui il servizio invia le notifiche quando lo stato del lavoro cambia. Specifica un elenco di eventi per i quali ricevere una notifica. Per impostazione predefinita, il servizio invia le notifiche quando un lavoro viene avviato, quando viene completato e se si verifica un errore. Puoi anche richiedere i risultati della richiesta nella notifica di completamento. Altrimenti, devi utilizzare il metodo `GET /v1/recognitions/{id}` per richiamare i risultati.
-   Eseguendo il polling del servizio:
    1.  Chiama il metodo `POST /v1/recognitions` senza un URL di callback, eventi o token utente.
    1.  Chiama periodicamente il metodo `GET /v1/recognitions` per controllare lo stato dei lavori più recenti o il metodo `GET /v1/recognitions/{id}` per controllare lo stato di un lavoro specifico.
    1.  Se controlli lo stato del lavoro con il metodo `GET /v1/recognitions`, chiama il metodo `GET /v1/recognitions/{id}` per richiamare i risultati del lavoro una volta completato.

I due approcci possono essere utilizzati insieme. Puoi ancora eseguire il polling del servizio per ottenere lo stato più recente per un lavoro creato con un URL di callback. Ad esempio, potresti voler ottenere lo stato di un lavoro se la ricezione delle notifiche richiede troppo tempo. Potresti anche controllare lo stato se sospetti di aver perso una o più notifiche a causa di un errore di servizio o di rete.

## Registrazione di un URL di callback
{: #register}

Registra un URL di callback chiamando il metodo `POST /v1/register_callback`. Una volta registrato un URL di callback, puoi utilizzarlo per ricevere notifiche per un numero indefinito di lavori. Il processo di registrazione comprende quattro passi:

1.  Chiama il metodo `POST /v1/register_callback` e passa un URL di callback. Facoltativamente, puoi anche specificare un segreto specificato dall'utente. Il servizio utilizza il segreto per calcolare le firme HMAC (keyed-hash message authentication code) SHA1 (Secure Hash Algorithm 1) per l'autenticazione e l'integrità dei dati. Il seguente esempio registra un callback dell'utente che risponde all'URL `http://{user_callback_path}/results`. La chiamata include il segreto utente `ThisIsMySecret`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/register_callback?callback_url=http://{user_callback_path}/results&user_secret=ThisIsMySecret"
    ```
    {: pre}

1.  Il servizio tenta di convalidare, o inserire in whitelist, l'URL di callback se non è già registrato inviando una richiesta `GET` all'URL di callback. Il servizio passa una stringa di verifica alfanumerica casuale tramite il parametro di query `challenge_string` della richiesta. La richiesta include un'intestazione `Accept` che specifica `text/plain` come il tipo di risposta richiesto.

    Se la chiamata al metodo `register_callback` includeva un segreto utente, la richiesta `GET` dal servizio include anche un'intestazione `X-Callback-Signature` che specifica la firma HMAC-SHA1 della stringa di verifica. Il servizio calcola la firma utilizzando il segreto utente come chiave.

    ```
    GET http://{user_callback_path}/results?challenge_string=n9ArPGMQ36Hiu7QC
    header: X-Callback-Signature {HMAC-SHA1_signature}
    ```
    {: codeblock}

1.  Rispondi alla richiesta `GET` dal servizio con il codice di stato 200. Includi la stringa di verifica inviata dal servizio nella risposta. Includi la stringa in testo semplice nel corpo della risposta e imposta l'intestazione di risposta `Content-Type` su `text/plain`.

    Se la richiesta `POST` iniziale includeva un segreto utente, puoi calcolare una firma HMAC-SHA1 della stringa di verifica utilizzando il segreto come chiave. Se la richiesta `GET` è stata inviata dal servizio, la firma corrisponde al valore specificato dall'intestazione `X-Callback-Signature`.

    ```
    response code: 200 OK
    body: n9ArPGMQ36Hiu7QC
    ```
    {: codeblock}

1.  Il servizio controlla se la stringa di verifica viene restituita nel corpo della risposta alla sua richiesta `GET`. In tal caso, il servizio inserisce in whitelist l'URL di callback e risponde alla tua richiesta `POST` originale con il codice di stato 201. Il corpo della risposta include un oggetto JSON con un campo `status` che ha il valore `created` e un campo `url` che ha il valore del tuo URL di callback.

    ```
    response code: 201 Created
    body: {
      "status": "created",
      "url": "http://{user_callback_path}/results"
    }
    ```
    {: codeblock}

Il servizio invia solo una singola richiesta `GET` a un URL di callback durante il processo di registrazione. Il servizio deve ricevere una risposta con il codice di risposta 200 che include la stringa di verifica nel suo corpo entro cinque secondi. In caso contrario, non inserisce l'URL nella whitelist. Invece, invia il codice di stato 400 in risposta alla richiesta `POST /v1/register_callback`. Se l'URL di callback è già stato inserito in whitelist, il servizio invia il codice di stato 200 in risposta alla richiesta `POST` iniziale.

### Considerazioni sulla sicurezza
{: #security-async}

Quando utilizzi correttamente il metodo `POST /v1/register_callback` per registrare un URL di callback, il servizio inserisce l'URL nella whitelist per indicare che è stato verificato per l'utilizzo con le notifiche di callback. Se specifichi un segreto utente con la chiamata di registrazione, l'inserimento in whitelist indica anche che l'URL è stato convalidato per una maggiore sicurezza. La specifica di un segreto utente fornisce l'autenticazione e l'integrità dei dati per le richieste che utilizzano l'URL di callback con l'interfaccia HTTP asincrona.

Il servizio utilizza il segreto utente per calcolare una firma HMAC-SHA1 sul payload di ogni notifica di callback che invia all'URL. Il servizio invia la firma tramite l'intestazione `X-Callback-Signature` con ogni notifica. Il client può utilizzare il segreto per calcolare la sua propria firma di ogni payload di notifica. Se la sua firma corrisponde al valore dell'intestazione `X-Callback-Signature`, il client sa che la notifica è stata inviata dal servizio e che il suo contenuto non è stato modificato durante la trasmissione. Questa conoscenza garantisce che il client non sia vittima di un attacco MITM (man-in-middle).

HTTPS è ideale per le applicazioni di produzione. Tuttavia, durante lo sviluppo e la prototipazione delle applicazioni, le notifiche di callback basate su HTTP supportate dal servizio possono semplificare e accelerare il processo di sviluppo evitando i costi di HTTPS.

<!--

However, communicating over the HTTPS protocol is always the most secure means of learning job status and retrieving results. Using the HTTPS `GET /v1/recognitions/{id}` method to retrieve the results of a job is therefore more secure that receiving the results via callback notification. While the use of HMAC-SHA1 signatures based on a user secret ensures authentication and data integrity for callback notifications, it does not provide confidentiality. Conversely, because it encrypts the body of the response, HTTPS can provide authentication, integrity, *and* confidentiality.

HTTPS, however, is not ideal in terms of additional development overhead. Moreover, although the service validates SSL certificates to prevent man-in-the-middle attacks when you use HTTPS, validation is not foolproof if you use self-signed certificates, which enable encryption but not authentication or data integrity. During application development, user secrets and digital signatures provide a suitable level of security for requests made over the HTTP protocol. You can work with HTTP callback notifications during prototyping and move to HTTPS only for application deployment.

-->

### Annullamento della registrazione di un URL di callback
{: #unregister}

Puoi annullare in qualsiasi momento la registrazione di un URL di callback inserito in whitelist chiamando il metodo `POST /v1/unregister_callback`. L'annullamento della registrazione di un URL di callback può essere utile per testare la tua applicazione con il servizio. Dopo aver annullato la registrazione di un URL di callback, non puoi più utilizzarlo con le richieste di riconoscimento asincrone.

## Creazione di un lavoro
{: #create}

Crea un lavoro di riconoscimento chiamando il metodo `POST /v1/recognitions`. Il modo in cui apprendi lo stato e i risultati del lavoro dipende dall'approccio che utilizzi e dai parametri che passi:

-   *Per utilizzare le notifiche di callback*, includi il parametro di query `callback_url` per specificare un URL a cui il servizio deve inviare le notifiche di callback quando lo stato del lavoro cambia. Puoi anche specificare i seguenti parametri di query facoltativi:
    -   `events` per sottoscrivere a un elenco di eventi di notifica. Per impostazione predefinita, il servizio invia notifiche di callback quando il lavoro viene avviato (l'evento `recognitions.started`), quando il lavoro viene completato (l'evento `recognitions.completed`) e se si verifica un errore (l'evento `recognitions.failed`). Puoi specificare un sottoinsieme di eventi o utilizzare l'evento `recognitions.completed_with_results` anziché l'evento `recognitions.completed` per includere i risultati con la notifica di completamento del lavoro.
    -   `user_token` per specificare una stringa che deve essere inclusa in ciascuna notifica per il lavoro. Poiché puoi utilizzare lo stesso URL di callback con un numero indefinito di lavori, puoi utilizzare i token utente per differenziare le notifiche per i diversi lavori.
-   *Per utilizzare il polling*, ometti i parametri di query `callback_url`, `events` e `user_token`. Devi quindi utilizzare i metodi `GET /v1/recognitions` o `GET /v1/recognitions/{id}` per controllare lo stato del lavoro, utilizzando l'ultimo metodo per richiamare i risultati al termine del lavoro.

In entrambi i casi, puoi includere il parametro di query `results_ttl` per specificare il numero di minuti per i quali i risultati devono rimanere disponibili al termine del lavoro.

Oltre ai parametri precedenti, che sono specifici per l'interfaccia asincrona, il metodo `POST /v1/recognitions` supporta la maggior parte degli stessi parametri delle interfacce WebSocket e HTTP sincrona. Per ulteriori informazioni, vedi il [Riepilogo dei parametri](/docs/services/speech-to-text/summary.html).

### Notifiche di callback
{: #notifications}

Se il lavoro viene creato con un URL di callback, il servizio invia una notifica di callback HTTP `POST` all'URL registrato quando si verifica un evento specificato. Il corpo di una notifica di base è costituito da un oggetto JSON con la seguente struttura:

```javascript
{
  "id": "{job_id}",
  "event": "{recognitions_status}",
  "user_token": "{user_token}"
}
```
{: codeblock}

Il campo `id` identifica l'ID del lavoro che ha generato il callback e il campo `event` identifica l'evento che ha attivato il callback. Il campo `user_token` include il token utente per il lavoro se ne è stato specificato uno. In caso contrario, il campo è una stringa vuota. Se l'evento è `recognitions.completed_with_results`, l'oggetto include un campo `results` che fornisce i risultati della richiesta di riconoscimento. Il client può rispondere alla notifica di callback con il codice di stato 200.

Se l'URL di callback è stato registrato con un segreto utente, il servizio invia anche l'intestazione `X-Callback-Signature` con la notifica di callback. L'intestazione specifica la firma HMAC-SHA1 del corpo della richiesta. Il servizio calcola la firma utilizzando il segreto utente come chiave. Il client può calcolare la firma del payload per la notifica di callback per assicurarsi che corrisponda alla firma nell'intestazione. Ad esempio, il seguente codice Python semplice calcola la firma sulla stringa `notification_payload`:

```python
from hashlib import sha1
import hmac
hashed = hmac.new(user_secret, notification_payload, sha1)
signature = hashed.digest().encode("base64").rstrip('\n')
```
{: codeblock}

### Esempio di callback
{: #callback}

Il seguente esempio crea un lavoro associato all'URL di callback precedentemente inserito in whitelist `http://{user_callback_path}/results`. L'esempio passa il token utente `job25` per identificare il lavoro nelle notifiche di callback inviate dal servizio. La chiamata utilizza gli eventi predefiniti, pertanto l'utente deve chiamare il metodo `GET /v1/recognitions/{id}` per richiamare i risultati quando il servizio invia una notifica di callback per indicare che il lavoro è stato completato. La chiamata imposta il parametro di query `timestamps` della richiesta di riconoscimento su `true`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?callback_url=http://{user_callback_path}/results&user_token=job25&timestamps=true"
```
{: pre}

Il servizio restituisce lo stato della richiesta, che è `waiting` per indicare che il servizio sta preparando il lavoro per l'elaborazione. La risposta include l'ora di creazione, l'ID lavoro e l'URL dove ottenere maggiori informazioni sul lavoro.

```javascript
{
  "created": "2016-08-17T19:15:17.926Z",
  "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bd734c0-e575-21f3-de03-f932aa0468a0",
  "status": "waiting"
}
```
{: codeblock}

### Esempio di polling
{: #polling}

Il seguente esempio crea un lavoro che non è associato a un URL di callback. L'utente deve eseguire il polling del servizio per conoscere quando il lavoro viene completato e quindi richiamare i risultati con il metodo `GET /v1/recognitions/{id}`. Come nell'esempio precedente, la chiamata imposta il parametro `timestamps` della richiesta di riconoscimento su `true`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?timestamps=true"
```
{: pre}

Il servizio restituisce lo stato `processing` per indicare che sta già elaborando il lavoro, insieme all'ora di creazione, all'ID lavoro e all'URL per ottenere informazioni sul lavoro.

```javascript
{
  "created": "2016-08-17T19:13:23.622Z",
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "status": "processing"
}
```
{: codeblock}

## Controllo dello stato e richiamo dei risultati di un lavoro
{: #job}

Chiama il metodo `GET /v1/recognitions/{id}` per controllare lo stato del lavoro specificato con il parametro di percorso `id`. La risposta include sempre l'ID e lo stato del lavoro e le sue ore di creazione e aggiornamento. Se lo stato è `completed`, la risposta include anche i risultati della richiesta di riconoscimento.

Il metodo `GET /v1/recognitions/{id}` è l'unico modo per richiamare i risultati del lavoro se

-   Il lavoro è stato inoltrato senza un URL di callback.
-   Il lavoro è stato inoltrato con un URL di callback ma senza specificare l'evento `recognitions.completed_with_results`.
-   Il lavoro non è uno dei 100 ultimi lavori in sospeso. Quando ometti il parametro di percorso `id` vengono restituiti solo gli ultimi 100 lavori.

Tuttavia, puoi ancora utilizzare il metodo per richiamare i risultati per un lavoro che ha specificato un URL di callback e l'evento `recognitions.completed_with_results`. Puoi richiamare i risultati per qualsiasi lavoro tutte le volte che vuoi mentre rimangono disponibili. Un lavoro e i suoi risultati rimangono disponibili finché non li elimini con il metodo `DELETE /v1/recognitions/{id}` o finché non scade la durata (TTL) del lavoro, a seconda dell'evento che si verifica per primo. Per impostazione predefinita, i risultati scadono dopo una settimana, a meno che non specifichi una durata (TTL) diversa con il parametro `results_ttl` del metodo `POST /v1/recognitions`.

### Esempio di stato senza risultati
{: #withoutResults}

Il seguente esempio controlla lo stato del lavoro con l'ID specificato. Il lavoro non è stato ancora completato, quindi la risposta non include i risultati.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}

```javascript
{
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "created": "2016-08-17T19:13:23.622Z",
  "updated": "2016-08-17T19:13:24.434Z",
  "status": "processing"
}
```
{: codeblock}

### Esempio di stato con risultati
{: #withResults}

Il seguente esempio richiede lo stato del lavoro con l'ID specificato. Il lavoro è stato completato, quindi la risposta include i risultati della richiesta.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}

```javascript
{
  "id": "398fcd80-330a-22ba-93ce-1a73f454dd98",
  "results": [
    {
      "result_index": 0,
      "results": [
        {
          "final": true,
          "alternatives": [
            {
              "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday ",
              "timestamps": [
                [
                  "several",
                  1,
                  1.52
                ],
                [
                  "tornadoes",
                  1.52,
                  2.15
                ],
                . . .
                [
                  "Sunday",
                  5.74,
                  6.33
                ]
              ],
              "confidence": 0.89
            }
          ]
        }
      ]
    }
  ],
  "created": "2016-08-17T19:11:04.298Z",
  "updated": "2016-08-17T19:11:16.003Z",
  "status": "completed"
}
```
{: codeblock}

## Controllo dello stato dei lavori più recenti
{: #jobs}

Chiama il metodo `GET /v1/recognitions` per controllare lo stato dei lavori più recenti. Il metodo restituisce lo stato dei 100 lavori in sospeso più recenti associati alle credenziali del servizio con cui viene chiamato. Il metodo restituisce l'ID e lo stato di ogni lavoro, insieme alle relative ore di creazione e aggiornamento. Se un lavoro è stato creato con un URL di callback e un token utente, il metodo restituisce anche il token utente per il lavoro.

La risposta include uno dei seguenti stati:

-   `waiting` se il servizio sta preparando il lavoro per l'elaborazione. Questo è lo stato iniziale di tutti i lavori. Il lavoro rimane in questo stato finché il servizio non ha la capacità di iniziare a elaborarlo.
-   `processing` se il servizio sta elaborando attivamente il lavoro.
-   `completed` se il servizio ha terminato l'elaborazione del lavoro. Se il lavoro ha specificato un URL di callback e l'evento `recognitions.completed_with_results`, il servizio ha inviato i risultati con la notifica di callback. In caso contrario, utilizza il metodo `GET /v1/recognitions/{id}` per ottenere i risultati.
-   `failed` se il lavoro non è riuscito per qualche motivo.

Un lavoro e i suoi risultati rimangono disponibili finché non li elimini con il metodo `DELETE /v1/recognitions/{id}` o finché non scade la durata (TTL) del lavoro, a seconda dell'evento che si verifica per primo. 

### Esempio
{: #statusExample}

Il seguente esempio richiede lo stato degli ultimi lavori correnti associati alle credenziali del servizio del chiamante. L'utente ha tre lavori in sospeso in vari stati. Il primo lavoro è stato creato con un URL di callback e un token utente.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions"
```
{: pre}

```javascript
{
  "recognitions": [
    {
      "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
      "created": "2016-08-17T19:15:17.926Z",
      "updated": "2016-08-17T19:15:17.926Z",
      "status": "waiting",
      "user_token": "job25"
    },
    {
      "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
      "created": "2016-08-17T19:13:23.622Z",
      "updated": "2016-08-17T19:13:24.434Z",
      "status": "processing"
    },
    {
      "id": "398fcd80-330a-22ba-93ce-1a73f454dd98",
      "created": "2016-08-17T19:11:04.298Z",
      "updated": "2016-08-17T19:11:16.003Z",
      "status": "completed"
    }
  ]
}
```
{: codeblock}

## Eliminazione di un lavoro
{: #delete-async}

Puoi utilizzare il metodo `DELETE /v1/recognitions/{id}` per eliminare il lavoro specificato con il parametro di percorso `id`. In genere, elimini un lavoro dopo aver ottenuto i suoi risultati dal servizio. Una volta eliminato un lavoro, i suoi risultati non sono più disponibili. Non puoi eliminare un lavoro che il servizio sta elaborando attivamente.

Per impostazione predefinita, il servizio mantiene i risultati di ciascun lavoro fino alla scadenza della durata (TTL) del lavoro. La durata predefinita è una settimana, ma puoi utilizzare il parametro `results_ttl` del metodo `POST /v1/recognitions` per specificare il numero di minuti in cui il servizio deve mantenere i risultati.

### Esempio
{: #deleteExample-async}

Il seguente esempio elimina il lavoro con l'ID specificato:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}
