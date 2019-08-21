---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-10"

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

# L'interfaccia HTTP sincrona
{: #http}

L'interfaccia HTTP sincrona del servizio {{site.data.keyword.speechtotextfull}} fornisce un singolo metodo `POST /v1/recognize` per richiedere il riconoscimento vocale con il servizio. Questo metodo è il mezzo più semplice per ottenere una trascrizione. Offre due modi per inoltrare una richiesta di riconoscimento vocale:
{: shortdesc}

-   Il primo invia tutto l'audio in un singolo flusso tramite il corpo della richiesta. Specifichi i parametri dell'operazione come intestazioni di richiesta e parametri di query. Per ulteriori informazioni, vedi [Effettuazione di una richiesta HTTP di base](#HTTP-basic).
-   Il secondo invia l'audio come richiesta a più parti. Specifichi i parametri della richiesta come una combinazione di intestazioni di richiesta, parametri di query e metadati JSON. Per ulteriori informazioni, vedi [Effettuazione di una richiesta HTTP a più parti](#HTTP-multi).

Inoltra un massimo di 100 MB e un minimo di 100 byte di dati audio con una singola richiesta. Per informazioni sui formati audio e sull'utilizzo della compressione per massimizzare la quantità di audio che puoi inviare con una richiesta, vedi [Formati audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats). Per informazioni su tutti i metodi dell'interfaccia HTTP, vedi la [Guida di riferimento API](https://{DomainName}/apidocs/speech-to-text){: external}.

## Effettuazione di una richiesta HTTP di base
{: #HTTP-basic}

Il metodo HTTP `POST /v1/recognize` fornisce un modo semplice per trascrivere l'audio. Passi tutto l'audio tramite il corpo della richiesta e specifichi i parametri come intestazioni di richiesta e parametri di query.

Il seguente esempio `curl` invia una richiesta di riconoscimento per un singolo file FLAC denominato `audio-file.flac`. La richiesta omette il parametro di query `model` per utilizzare il modello di lingua predefinito, `en-US_BroadbandModel`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

L'esempio restituisce la seguente trascrizione per l'audio:

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Il metodo `POST /v1/recognize` restituisce i risultati solo dopo aver elaborato tutto l'audio per una richiesta. Il metodo è appropriato per l'elaborazione batch ma non per il riconoscimento vocale dinamico. Utilizza l'interfaccia WebSocket per trascrivere l'audio dinamico.

Se i tuoi dati sono costituiti da più file audio, il metodo consigliato per inoltrare l'audio consiste nell'inviare più richieste, una per ogni file audio. Puoi inoltrare le richieste in un loop, facoltativamente con parallelismo per migliorare le prestazioni. Puoi anche utilizzare il riconoscimento vocale a più parti per passare più file audio con una singola richiesta.

## Effettuazione di una richiesta HTTP a più parti
{: #HTTP-multi}

Il metodo `POST /v1/recognize` supporta anche le richieste a più parti (multipart). Passi tutti i dati audio come dati del modulo a più parti. Specifichi alcuni parametri come intestazioni di richiesta e parametri di query, ma passi i metadati JSON come dati del modulo per controllare la maggior parte degli aspetti della trascrizione.

Il riconoscimento vocale a più parti è destinato ai seguenti casi di utilizzo:

-   Per passare più file audio con una singola richiesta di riconoscimento vocale.
-   Con i browser per i quali JavaScript è disabilitato. Le richieste a più parti basate sui dati del modulo non richiedono l'utilizzo di JavaScript.
-   Quando i parametri di una richiesta di riconoscimento sono superiori a 8 KB, che è il limite imposto dalla maggior parte dei server e proxy HTTP. Ad esempio, l'individuazione di un numero molto elevato di parole chiave può aumentare le dimensioni di una richiesta oltre questo limite. Le richieste a più parti utilizzano i dati del modulo per evitare questa restrizione.

Le seguenti sezioni descrivono i parametri che utilizzi per le richieste a più parti e mostrano una richiesta di esempio.

### Parametri per le richieste a più parti
{: #multipartParameters}

Specifica i seguenti parametri del riconoscimento vocale a più parti come intestazioni di richiesta, parametri di query e dati del modulo.

<table summary="Ogni riga della tabella descrive l'utilizzo di un possibile parametro per una richiesta di riconoscimento a più parti.">
  <caption>Tabella 1. Parametri per le richieste a più parti</caption>
  <tr>
    <th id="parameter" style="text-align:left; width:20%">Parametro</th>
    <th id="description" style="text-align:center; width:80%">Descrizione</th>
  </tr>
  <tr>
    <td>
      <code>metadata</code>
      <br/><em>Oggetto</em>
      <br/><em>dati del modulo</em>
    </td>
    <td>
      <em>Obbligatorio.</em> Un oggetto JSON che fornisce i parametri di
      trascrizione per la richiesta. L'oggetto deve essere la prima parte
      dei dati del modulo. Le informazioni descrivono l'audio nelle
      parti successive dei dati del modulo. Vedi
      [Metadati JSON per le richieste a più parti](#multipartJSON).
    </td>
  </tr>
  <tr>
    <td>
      <code>upload</code>
      <br/><em>File</em>
      <br/><em>dati del modulo</em>
    </td>
    <td>
      <em>Obbligatorio.</em> Uno o più file audio come il resto dei
      dati del modulo per la richiesta. Tutti i file audio devono avere lo stesso formato.
      Con il comando `curl`, includi un'opzione <code>--form</code> separata
      per ogni file della richiesta.
    </td>
  </tr>
  <tr>
    <td>
      <code>Content-Type</code>
      <br/><em>Intestazione</em>
      <br/><em>Stringa</em>
    </td>
    <td>
      <em>Obbligatorio.</em> Specifica `multipart/form-data` per indicare
      come i dati vengono passati al metodo. Specifica il tipo di contenuto
      dell'audio con il parametro JSON `part_content_type`.
    </td>
  </tr>
  <tr>
    <td>
      <code>Transfer-Encoding</code>
      <br/><em>Intestazione</em>
      <br/><em>Stringa</em>
    </td>
    <td>
      <em>Facoltativo.</em> Specifica `chunked` per inviare in streaming
      i dati audio al servizio. Ometti il parametro se invii tutto l'audio con una singola richiesta.
    </td>
  </tr>
  <tr>
    <td>
      <code>model</code>
      <br/><em>Query</em>
      <br/><em>Stringa</em>
    </td>
    <td>
      <em>Facoltativo.</em> L'identificativo del modello che deve essere utilizzato
      con la richiesta. Il valore predefinito è `en-US_BroadbandModel`.
    </td>
  </tr>
  <tr>
    <td>
      <code>language_customization_id</code>
      <br/><em>Query</em>
      <br/><em>Stringa</em>
    </td>
    <td>
      <em>Facoltativo.</em> Il GUID di un modello di lingua personalizzato
      che deve essere utilizzato con la richiesta.
    </td>
  </tr>
  <tr>
    <td>
      <code>acoustic_customization_id</code>
      <br/><em>Query</em>
      <br/><em>Stringa</em>
    </td>
    <td>
      <em>Facoltativo.</em> Il GUID di un modello acustico personalizzato
      che deve essere utilizzato con la richiesta.
    </td>
  </tr>
  <tr>
    <td>
      <code>base_model_version</code>
      <br/><em>Query</em>
      <br/><em>Stringa</em>
    </td>
    <td>
      <em>Facoltativo.</em> La versione del modello di base specificato
      da utilizzare con la richiesta.
    </td>
  </tr>
</table>

Per ulteriori informazioni sui parametri di query, vedi il [Riepilogo dei parametri](/docs/services/speech-to-text?topic=speech-to-text-summary).

### Metadati JSON per le richieste a più parti
{: #multipartJSON}

I metadati JSON che passi con una richiesta a più parti possono includere i seguenti campi:

-   `part_content_type` (stringa)
-   `data_parts_count` (numero intero)
-   `customization_weight` (numero)
-   `inactivity_timeout` (numero intero)
-   `keywords` (stringa[])
-   `keywords_threshold` (numero)
-   `max_alternatives` (numero intero)
-   `word_alternatives_threshold` (numero)
-   `word_confidence` (booleano)
-   `timestamps` (booleano)
-   `profanity_filter` (booleano)
-   `smart_formatting` (booleano)
-   `speaker_labels` (booleano)
-   `grammar_name` (stringa)
-   `redaction` (booleano)

Solo i seguenti due parametri sono specifici per le richieste a più parti:

-   Il campo `part_content_type` è *facoltativo* per la maggior parte dei formati audio. È obbligatorio per i formati `audio/alaw`, `audio/basic`, `audio/l16` e `audio/mulaw`. Il campo specifica il formato dell'audio nelle seguenti parti della richiesta. Tutti i file audio devono essere nello stesso formato.
-   Il campo `data_parts_count` è *facoltativo* per tutte le richieste. Specifica il numero di file audio inviati con la richiesta. Il servizio applica il rilevamento di fine flusso all'ultima (e probabilmente l'unica) parte di dati. Se ometti il parametro, il servizio determina il numero di parti dalla richiesta.

Tutti gli altri parametri dei metadati sono facoltativi. Per le descrizioni di tutti i parametri disponibili, vedi il [Riepilogo dei parametri](/docs/services/speech-to-text?topic=speech-to-text-summary).

### Richiesta a più parti di esempio

Il seguente esempio `curl` mostra come passare una richiesta di riconoscimento a più parti con il metodo `POST /v1/recognize`. La richiesta passa due file audio, **audio-file1.flac** e **audio-file2.flac**. Il parametro `metadata` fornisce la maggior parte dei parametri della richiesta; i parametri `upload` forniscono i file audio.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: multipart/form-data"
--form metadata="{\"part_content_type\":\"application/octet-stream\",
  \"data_parts_count\":2,
  \"timestamps\":true,
  \"word_alternatives_threshold\":0.9,
  \"keywords\":[\"colorado\",\"tornado\",\"tornadoes\"],
  \"keywords_threshold\":0.5}"
--form upload="@{path}audio-file1.flac"
--form upload="@{path}audio-file2.flac"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

L'esempio restituisce la seguente trascrizione per i file audio. Il servizio restituisce i risultati per i due file nell'ordine in cui vengono inviati. (L'output di esempio abbrevia i risultati per il secondo file.)

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 0.03,
          "alternatives": [
            {
              "confidence": 0.96,
                     "word": "the"
                  }
          ],
               "end_time": 0.09
        },
        {
          "start_time": 0.09,
               "alternatives": [
                  {
              "confidence": 0.96,
                     "word": "latest"
                  }
          ],
               "end_time": 0.62
        },
        {
          "start_time": 0.62,
               "alternatives": [
                  {
              "confidence": 0.96,
                     "word": "weather"
                  }
          ],
               "end_time": 0.87
        },
        {
          "start_time": 0.87,
               "alternatives": [
                  {
              "confidence": 0.96,
                     "word": "report"
                  }
          ],
               "end_time": 1.5
            }
      ],
         "keywords_result": {},
         "alternatives": [
            {
          "timestamps": [
            [
              "the",
                     0.03,
                     0.09
            ],
            [
              "latest",
                     0.09,
                     0.62
            ],
            [
              "weather",
                     0.62,
                     0.87
            ],
            [
              "report",
                     0.87,
                     1.5
                  ]
          ],
               "confidence": 0.99,
               "transcript": "the latest weather report "
            }
      ],
         "final": true
      },
      {
      "word_alternatives": [
        {
          "start_time": 0.15,
               "alternatives": [
                  {
              "confidence": 1.0,
                     "word": "a"
                  }
          ],
               "end_time": 0.3
        },
        {
          "start_time": 0.3,
               "alternatives": [
                  {
              "confidence": 1.0,
                     "word": "line"
                  }
          ],
               "end_time": 0.64
        },
        . . .
        {
          "start_time": 4.58,
               "alternatives": [
                  {
              "confidence": 0.98,
                     "word": "Colorado"
                  }
          ],
               "end_time": 5.16
        },
        {
          "start_time": 5.16,
               "alternatives": [
                  {
              "confidence": 0.98,
                     "word": "on"
                  }
          ],
               "end_time": 5.32
        },
        {
          "start_time": 5.32,
               "alternatives": [
                  {
              "confidence": 0.98,
                     "word": "Sunday"
                  }
          ],
               "end_time": 6.04
            }
      ],
         "keywords_result": {
        "tornadoes": [
               {
            "normalized_text": "tornadoes",
                  "start_time": 3.03,
                  "confidence": 0.98,
                  "end_time": 3.84
               }
        ],
            "colorado": [
               {
            "normalized_text": "Colorado",
                  "start_time": 4.58,
                  "confidence": 0.98,
                  "end_time": 5.16
               }
        ]
      },
      "alternatives": [
        {
          "timestamps": [
            [
              "a",
                     0.15,
                     0.3
            ],
            [
              "line",
                     0.3,
                     0.64
            ],
            . . .
            [
              "Colorado",
                     4.58,
                     5.16
            ],
            [
              "on",
                     5.16,
                     5.32
            ],
            [
              "Sunday",
                     5.32,
                     6.04
                  ]
          ],
               "confidence": 0.99,
               "transcript": "a line of severe thunderstorms with several
possible tornadoes is approaching Colorado on Sunday "
            }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}
