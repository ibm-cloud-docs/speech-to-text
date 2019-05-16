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
{:go: .ph data-hd-programlang='go'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:download: .download}
{:apikey: data-credential-placeholder='apikey'}
{:url: data-credential-placeholder='url'}
{:hide-dashboard: .hide-dashboard}

# Esercitazione introduttiva
{: #gettingStarted}

Il servizio {{site.data.keyword.speechtotextfull}} trascrive l'audio in testo per abilitare le funzionalità di trascrizione vocale per le applicazioni. Questa esercitazione basata su curl può aiutarti a iniziare a utilizzare subito il servizio. Gli esempi ti mostrano come chiamare il metodo `POST /v1/recognize` per richiedere una trascrizione.
{: shortdesc}

L'esercitazione utilizza le chiavi API di {{site.data.keyword.cloud}} IAM (Identity and Access Management) per l'autenticazione. Le istanze del servizio meno recenti potrebbero continuare a utilizzare `{username}` e `{password}` delle loro credenziali esistenti del servizio Cloud Foundry per eseguire l'autenticazione. Esegui l'autenticazione utilizzando l'approccio corretto per la tua istanza del servizio. Per ulteriori informazioni sull'utilizzo dell'autenticazione IAM da parte del servizio, vedi l'[aggiornamento del servizio del 30 ottobre 2018](/docs/services/speech-to-text/release-notes.html#October2018b) nelle note sulla release.
{: important}

## Prima di cominciare
{: #before-you-begin}

- {: hide-dashboard}  Crea un'istanza del servizio:
    1.  {: hide-dashboard} Vai alla pagina [{{site.data.keyword.speechtotextshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/catalog/services/speech-to-text){: new_window} nel catalogo {{site.data.keyword.cloud_notm}}.
    1.  {: hide-dashboard} Registrati per un account {{site.data.keyword.cloud_notm}} gratuito o accedi.
    1.  {: hide-dashboard} Fai clic su **Crea**.
-   Copia le credenziali per eseguire l'autenticazione presso la tua istanza del servizio:
    1.  {: hide-dashboard} Dal [dashboard {{site.data.keyword.cloud_notm}}![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/dashboard/apps){: new_window}, fai clic sulla tua istanza del servizio {{site.data.keyword.speechtotextshort}} per andare alla pagina del dashboard del servizio {{site.data.keyword.speechtotextshort}}.
    1.  Nella pagina **Gestisci**, fai clic su **Mostra** per visualizzare le tue credenziali.
    1.  Copia i valori `API Key` e `URL`.
-   Assicurati di avere il comando `curl`.
    -   Gli esempi utilizzano il comando `curl` per chiamare i metodi dell'interfaccia HTTP. Installa la versione per il tuo sistema operativo da [curl.haxx.se ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://curl.haxx.se/){: new_window}. Installa la versione che supporta il protocollo SSL (Secure Sockets Layer). Assicurati di includere il file binario installato nella tua variabile di ambiente `PATH`.

Quando immetti un comando, sostituisci `{apikey}` e `{url}` con la tua chiave API e il tuo URL effettivi. Ometti dal comando le parentesi graffe, che indicano un valore di variabile. Un valore effettivo è simile al seguente esempio:
{: hide-dashboard}

```bash
curl -X POST -u "apikey:L_HALhLVIksh1b73l97LSs6R_3gLo4xkujAaxm7i-b9x"
. . .
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{:pre}
{: hide-dashboard}

## Passo 1: trascrivi l'audio senza opzioni
{: #transcribe}

Chiama il metodo `POST /v1/recognize` per richiedere una trascrizione di base di un file audio FLAC senza parametri di richiesta aggiuntivi.

1.  Scarica il file audio di esempio <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="Icona link esterno" title="Icona link esterno"></a>.
1.  Immetti il seguente comando per chiamare il metodo `/v1/recognize` del servizio per la trascrizione di base senza parametri. L'esempio utilizza l'intestazione `Content-Type` per indicare il tipo di audio, `audio/flac`. Per la trascrizione, l'esempio utilizza il modello di lingua predefinito `en-US_BroadbandModel`.
    -   {: hide-dashboard} Sostituisci `{apikey}` e `{url}` con la tua chiave API e il tuo URL.
    -   Modifica `{path_to_file}` per specificare l'ubicazione del file `audio-file.flac`.

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize"{: url}
    ```
    {: pre}

    Il servizio restituisce i seguenti risultati di trascrizione:

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.87
              "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through colorado on sunday "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

## Passo 2: trascrivi l'audio con opzioni
{: #transcribeOptions}

Chiama il metodo `POST /v1/recognize` per trascrivere lo stesso file audio FLAC, ma specificando due parametri di trascrizione.

1.  Se necessario, scarica il file audio di esempio <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="Icona link esterno" title="Icona link esterno"></a>.
1.  Immetti il seguente comando per chiamare il metodo `/v1/recognize` del servizio con due parametri supplementari. Imposta il parametro `timestamps` su `true` per indicare l'inizio e la fine di ogni parola nel flusso audio. Imposta il parametro `max_alternatives` su `3` per ricevere le tre alternative più probabili per la trascrizione. L'esempio utilizza l'intestazione `Content-Type` per indicare il tipo di audio, `audio/flac` e la richiesta utilizza il modello predefinito, `en-US_BroadbandModel`.
    -   {: hide-dashboard} Sostituisci `{apikey}` e `{url}` con la tua chiave API e il tuo URL.
    -   Modifica `{path_to_file}` per specificare l'ubicazione del file `audio-file.flac`.

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize?timestamps=true&max_alternatives=3"{: url}
    ```
    {: pre}

    Il servizio restituisce i seguenti risultati, che includono le date/ore e tre trascrizioni alternative:

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "timestamps": [
                ["several":, 1.0, 1.51],
                ["tornadoes":, 1.51, 2.15],
                ["touch":, 2.15, 2.5],
                . . .
              ]
            },
            {
              "confidence": 0.87
              "transcript": "several tornadoes touch down as a line
of severe thunderstorms swept through colorado on sunday "
            },
            {
              "transcript": "several tornadoes touched down as a line
of severe thunderstorms swept through colorado on sunday "
            },
            {
              "transcript": "several tornadoes touch down is a line
of severe thunderstorms swept through colorado on sunday "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

## Passi successivi

-   Ottieni ulteriori informazioni sulle interfacce e sugli SDK disponibili per effettuare richieste di riconoscimento vocale nella [Panoramica per gli sviluppatori](/docs/services/speech-to-text/developer-overview.html).
-   Vedi le richieste di riconoscimento vocale di base per ciascuna delle interfacce del servizio in [Effettuazione di una richiesta di riconoscimento](/docs/services/speech-to-text/basic-request.html).
-   Trova informazioni dettagliate su tutti i metodi delle interfacce del servizio nella [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
