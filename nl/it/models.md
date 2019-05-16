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

# Lingue e modelli
{: #models}

Il servizio {{site.data.keyword.speechtotextfull}} supporta il riconoscimento vocale in molte lingue. Per tutte le interfacce, puoi utilizzare il parametro `model` per specificare il modello per una richiesta di riconoscimento vocale. Il modello indica la lingua in cui viene pronunciato l'audio e la frequenza con la quale viene campionato.
{: shortdesc}

## Modelli di lingua supportati
{: #modelsList}

Per la maggior parte delle lingue, il servizio supporta sia i modelli a banda larga che i modelli a banda stretta:

-   I *modelli a banda larga* solo per l'audio campionato a un valore superiore o uguale a 16 kHz. Utilizza i modelli a banda larga per le applicazioni reattive e in tempo reale, ad esempio, per le applicazioni vocali dinamiche.
-   I *modelli a banda stretta* sono per l'audio campionato a 8 kHz. Utilizza i modelli a banda stretta per la decodifica offline di conversazioni telefoniche, che sono il tipico uso per questa frequenza di campionamento.

La scelta del modello corretto per la tua applicazione è importante. Utilizza il modello che corrisponde alla frequenza di campionamento (e alla lingua) del tuo audio. Il servizio regola automaticamente la frequenza di campionamento del tuo audio in modo che corrisponda al modello che specifichi. Per ulteriori informazioni, vedi [Frequenza di campionamento](/docs/services/speech-to-text/audio-formats.html#samplingRate).

Per ottenere la migliore accuratezza del riconoscimento, devi anche considerare il contenuto della frequenza del tuo audio. Per ulteriori informazioni, vedi [Frequenza audio](/docs/services/speech-to-text/audio-formats.html#frequency).
{: tip}

La tabella 1 elenca i modelli supportati per ogni lingua. Se ometti il parametro `model` da una richiesta, per impostazione predefinita il servizio utilizza il modello a banda larga dell'inglese (Stati Uniti) `en-US_BroadbandModel`.

<table>
  <caption>Tabella 1. Modelli di lingua supportati</caption>
  <tr>
    <th style="text-align:left">Lingua</th>
    <th style="text-align:center">Modello a banda larga</th>
    <th style="text-align:center">Modello a banda stretta</th>
  </tr>
  <tr>
    <td>Portoghese (Brasile)</td>
    <td style="text-align:center"><code>pt-BR_BroadbandModel</code></td>
    <td style="text-align:center"><code>pt-BR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Francese</td>
    <td style="text-align:center"><code>fr-FR_BroadbandModel</code></td>
    <td style="text-align:center"><code>fr-FR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Tedesco</td>
    <td style="text-align:center"><code>de-DE_BroadbandModel</code></td>
    <td style="text-align:center"><code>de-DE_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Giapponese</td>
    <td style="text-align:center"><code>ja-JP_BroadbandModel</code></td>
    <td style="text-align:center"><code>ja-JP_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Coreano</td>
    <td style="text-align:center"><code>ko-KR_BroadbandModel</code></td>
    <td style="text-align:center"><code>ko-KR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Cinese mandarino</td>
    <td style="text-align:center"><code>zh-CN_BroadbandModel</code></td>
    <td style="text-align:center"><code>zh-CN_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Arabo standard moderno</td>
    <td style="text-align:center"><code>ar-AR_BroadbandModel</code></td>
    <td style="text-align:center">Non supportato</td>
  </tr>
  <tr>
    <td>Spagnolo</td>
    <td style="text-align:center"><code>es-ES_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-ES_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Inglese (Regno Unito)</td>
    <td style="text-align:center"><code>en-GB_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-GB_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Inglese (Stati Uniti)</td>
    <td style="text-align:center"><code>en-US_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-US_NarrowbandModel</code></br>
      <code>en-US_ShortForm_NarrowbandModel</code></td>
  </tr>
</table>

### Il modello di formato breve dell'inglese (Stati Uniti)
{: #modelsShortform}

Il modello di formato breve dell'inglese (Stati Uniti), `en-US_ShortForm_NarrowbandModel`, può migliorare il riconoscimento vocale per le soluzioni IVR (Interactive Voice Response) e di supporto clienti automatizzato. Il modello di formato breve viene addestrato a riconoscere le espressioni brevi che ricorrono di frequente negli ambienti di supporto clienti come i call center di supporto automatizzati o con agenti umani. Il modello viene regolato, ad esempio, per espressioni precise quali le cifre, le ortografie scandendo singoli caratteri di nomi e parole e le risposte sì-no. L'utilizzo di una grammatica in combinazione con il modello di formato breve può ulteriormente migliorare i risultati del riconoscimento.

Come con tutti i modelli, gli ambienti rumorosi possono avere un impatto negativo sui risultati. Ad esempio, il rumore acustico di fondo di aeroporti, veicoli in movimento, sale conferenze e più parlanti può ridurre l'accuratezza della trascrizione.  Anche l'audio dei dispositivi di registrazione ambientale può ridurre l'accuratezza a causa dell'eco comune in tali dispositivi. L'utilizzo di un modello acustico personalizzato con il modello di formato breve può controbilanciare tali effetti.

-   Per ulteriori informazioni sulla personalizzazione del modello di lingua e del modello acustico, vedi [L'interfaccia di personalizzazione](/docs/services/speech-to-text/custom.html).
-   Per ulteriori informazioni sulle grammatiche, vedi [Utilizzo di grammatiche con modelli di lingua personalizzati](/docs/services/speech-to-text/grammar.html).

### Esempio di modello di lingua
{: #modelsExample}

La seguente richiesta HTTP di esempio utilizza il modello `en-US-NarrowbandModel` per il riconoscimento vocale:

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel"
```
{: pre}

## Elenco dei modelli
{: #listModels}

L'interfaccia HTTP fornisce due metodi per elencare le informazioni sui modelli supportati:

-   Il metodo `GET /v1/models` elenca le informazioni su tutti i modelli disponibili.
-   Il metodo `GET /v1/models/{model_id}` elenca le informazioni su uno specifico modello.

Entrambi i metodi restituiscono le seguenti informazioni su un modello:

-   `name` è il nome del modello che utilizzi in una richiesta.
-   `language` è l'identificativo della lingua del modello (ad esempio, `en-US`).
-   `rate` identifica la frequenza di campionamento (la frequenza minima accettabile per l'audio) che viene utilizzata dal modello, in Hertz.
-   `url` è l'URI per il modello.
-   `description` fornisce una breve descrizione del modello.
-   `supported_features` descrive le funzioni di servizio aggiuntive che sono supportate con il modello:
    -   `custom_language_model` è un valore booleano che indica se puoi creare dei modelli di lingua personalizzati basati sul modello.
    -   `speaker_label` indica se puoi utilizzare il parametro `speaker_label` con il modello.

### Richieste e risposte di esempio
{: #listExample}

Il seguente esempio elenca tutti i modelli che sono supportati dal servizio:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/models"
```
{: pre}

```javascript
{
  "models": [
    {
      "name": "pt-BR_NarrowbandModel",
      "language": "pt-BR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/pt-BR_NarrowbandModel",
      "rate": 8000,
      "supported_features": {
        "custom_language_model": false,
        "speaker_labels": false
      },
      "description": "Brazilian Portuguese narrowband model."
    },
    {
      "name": "ko-KR_BroadbandModel",
      "language": "ko-KR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/ko-KR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": false
      },
      "description": "Korean broadband model."
    },
    {
      "name": "fr-FR_BroadbandModel",
      "language": "fr-FR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/fr-FR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": true
      },
      "description": "French broadband model."
    },
    . . .
  ]
}
```
{: codeblock}

Il seguente esempio mostra le informazioni relative al modello a banda larga dell'inglese (Stati Uniti). Il modello supporta sia la personalizzazione del modello di lingua che le etichette dei parlanti.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/models/en-US_BroadbandModel"
```
{: pre}

```javascript
{
  "rate": 16000,
  "name": "en-US_BroadbandModel",
  "language": "en-US",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/en-US_BroadbandModel",
  "supported_features": {
    "custom_language_model": true,
    "speaker_labels": true
  },
  "description": "US English broadband model."
}
```
{: codeblock}
