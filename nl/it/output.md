---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-10"

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

# Funzioni di output
{: #output}

Il servizio {{site.data.keyword.speechtotextfull}} offre le seguenti funzioni per indicare le informazioni che il servizio deve includere nei suoi risultati della trascrizione per una richiesta di riconoscimento vocale. Tutti i parametri di output sono facoltativi.
{: shortdesc}

-   Per degli esempi di semplici richieste di riconoscimento vocale per ciascuna delle interfacce del servizio, vedi [Effettuazione di una richiesta di riconoscimento](/docs/services/speech-to-text?topic=speech-to-text-basic-request).
-   Per degli esempi e delle descrizioni di risposte di riconoscimento vocale, vedi [Descrizione dei risultati del riconoscimento](/docs/services/speech-to-text?topic=speech-to-text-basic-response). Il servizio restituisce tutto il contenuto della risposta JSON nel set di caratteri UTF-8.
-   Per un elenco alfabetico di tutti i parametri di riconoscimento vocale disponibili, compresi il loro stato (generalmente disponibile o beta) e le lingue supportate, vedi il [Riepilogo dei parametri](/docs/services/speech-to-text?topic=speech-to-text-summary).

## Etichette del parlante
{: #speaker_labels}

La funzione di etichette del parlante è una funzionalità beta disponibile per inglese (Stati Uniti) e spagnolo (sia i modelli a banda larga che quelli a banda stretta) e inglese (Regno Unito) (solo il modello a banda stretta).
{: note}

Le etichette del parlante identificano gli individui e le parole da essi espresse in uno scambio tra più partecipanti. (L'etichettare chi ha parlato e quando è a volte indicato come *diarizzazione dei parlanti*.) Puoi utilizzare le informazioni per sviluppare una trascrizione persona per persona di un flusso audio, come ad esempio un contatto a un call center. In alternativa, puoi farne uso per animare uno scambio con un avatar o un robot conversazionale. Per delle prestazioni ottimali, utilizza un audio lungo almeno un minuto.

Le etichette del parlante sono ottimizzate per scenari con due parlanti. Funzionano al meglio per le conversazioni telefoniche che coinvolgono due persone in uno scambio prolungato. Possono gestire fino a sei parlanti ma più di due parlanti possono produrre prestazioni variabili. Gli scambi tra due persone avvengono di norma su supporti a banda stretta ma puoi utilizzare etichette del parlante con modelli supportati a banda stretta e a banda larga.

Per utilizzare la funzione, imposti il parametro `speaker_labels` su `true` per una richiesta di riconoscimento; per impostazione predefinita, il parametro è `false`. Il servizio identifica i parlanti in base alle singole parole dell'audio. Si basa su un tempo di inizio e di fine di una parola per identificarne il parlante. Pertanto, abilitare le etichette del parlante forza anche il parametro `timestamps` a essere `true` (vedi [Date/ore delle parole](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps)).

### Esempio di etichette del parlante
{: #speakerLabelsExample}

La seguente richiesta di esempio mostra una risposta che include le etichette del parlante. I valori numerici associati a ciascun elemento dell'array `timestamps` sono i tempi di inizio e di fine della parola nell'audio.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-multi.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel&speaker_labels=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "hello",
              0.68,
              1.19
            ],
            [
              "yeah",
              1.47,
              1.93
            ],
            [
              "yeah",
              1.96,
              2.12
            ],
            [
              "how's",
              2.12,
              2.59
            ],
            [
              "Billy",
              2.59,
              3.17
            ],
            . . .
          ]
          "confidence": 0.82,
          "transcript": "hello yeah yeah how's Billy "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0,
  "speaker_labels": [
    {
      "from": 0.68,
      "to": 1.19,
      "speaker": 2,
      "confidence": 0.42,
      "final": false
    },
    {
      "from": 1.47,
      "to": 1.93,
      "speaker": 1,
      "confidence": 0.52,
      "final": false
    },
    {
      "from": 1.96,
      "to": 2.12,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.12,
      "to": 2.59,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.59,
      "to": 3.17,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    . . .
  ]
}
```
{: codeblock}

Il campo `trascrizione` mostra la trascrizione finale dell'audio, che elenca le parole come sono state pronunciate da tutti i partecipanti. Confrontando e sincronizzando le etichette del parlante con le date/ore, puoi riassemblare la conversazione così come si è verificata.

<table style="width:50%">
  <caption>Tabella 1. Esempio di etichette del parlante</caption>
  <tr>
    <th style="text-align:left">Data/ora</th>
    <th style="text-align:left">Etichetta del parlante</th>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"hello",<br/>
      0.68,<br/>
      1.19</code>
    </td>
    <td>
      <code>"from": 0.68,<br/>
      "to": 1.19,<br/>
      "speaker": 2,<br/>
      "confidence": 0.42,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.47,<br/>
      1.93</code>
    </td>
    <td>
      <code>"from": 1.47,<br/>
      "to": 1.93,<br/>
      "speaker": 1,<br/>
      "confidence": 0.52,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.96,<br/>
      2.12</code>
    </td>
    <td>
      <code>"from": 1.96,<br/>
      "to": 2.12,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"how's",<br/>
      2.12,<br/>
      2.59</code>
    </td>
    <td>
      <code>"from": 2.12,<br/>
      "to": 2.59,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"Billy",<br/>
      2.59,<br/>
      3.17</code>
    </td>
    <td>
      <code>"from": 2.59,<br/>
      "to": 3.17,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
</table>

I risultati indicano chiaramente in che modo è iniziato questo scambio tra due persone:

-   **Parlante 2** - "Hello?"
-   **Parlante 1** - "Yeah?"
-   **Parlante 2** - "Yeah, how's Billy?"

Nell'esempio, i campi `confidence` indicano quanto il servizio ritiene attendibile la sua identificazione dei parlanti. Il campo `final` ha un valore di `false` per ciascuna parola. Questo valore indica che il servizio sta ancora elaborando l'audio. Il servizio potrebbe modificare la sua identificazione del parlante o la sua attendibilità per le singole parole prima di aver finito.

### ID parlante per le etichette del parlante
{: #speakerLabelsIDs}

L'esempio illustra anche un aspetto interessante degli ID parlante. Nei campi `speaker`, il primo parlante ha un ID di `2` e il secondo ha un ID di `1`. Il servizio sviluppa una migliore comprensione degli schemi dei parlanti man mano che elabora l'audio. Può pertanto modificare gli ID parlante per le singole parole e può anche aggiungere e rimuovere parlanti finché non genera i suoi risultati finali.

Di conseguenza, gli ID parlante potrebbero non essere sequenziali, contigui od ordinati. Ad esempio, gli ID di norma iniziano da `0` ma, nell'esempio precedente, il primo ID è `1`. Il servizio ha probabilmente omesso un'assegnazione di parola precedente per il parlante `0` sulla base di un'ulteriore analisi dell'audio. Le omissioni possono verificarsi anche per parlanti successivi. Le omissioni possono causare degli scarti nella numerazione e produrre risultati per due parlanti che sono etichettati, ad esempio, `0` e `2`. Un'altra considerazione è che i parlanti possono abbandonare una conversazione. Di conseguenza, un partecipante che contribuisce solo alle prime fasi di una conversazione potrebbe non comparire nei risultati successivi.

### Richiesta dei risultati provvisori per le etichette del parlante
{: #speakerLabelsInterim}

Con l'interfaccia WebSocket, puoi richiedere i risultati provvisori e le etichette del parlante (vedi [Risultati provvisori](/docs/services/speech-to-text?topic=speech-to-text-output#interim)). I risultati finali sono generalmente migliori rispetto ai risultati provvisori. I risultati provvisori possono però aiutare a identificare l'evoluzione di una trascrizione e l'assegnazione delle etichette del parlante. I risultati provvisori possono indicare dove gli ID e i parlanti temporanei sono apparsi o scomparsi. Tuttavia, il servizio può riutilizzare gli ID dei parlanti che identifica inizialmente e che successivamente riconsidera od omette. Pertanto, un ID potrebbe fare riferimento a due parlanti diversi nei risultati provvisori e in quelli finali.

Quando richiedi sia i risultati provvisori che le etichette del parlante, i risultati finali per lunghi flussi audio potrebbero arrivare molto dopo che sono stati restituiti i risultati provvisori iniziali. È anche possibile che qualche risultato provvisorio includa solo un campo `speaker_labels` senza i campi `results` e `result_index`. Se non richiedi i risultati provvisori, il servizio restituisce i risultati finali che includono i campi `results` e `result_index` e un singolo campo `speaker_labels`.

Il servizio invia i risultati finali quando il flusso audio è completo oppure in risposta a un timeout, a seconda di quale evento si verifica per primo. Il servizio imposta il campo `final` su `true` solo per l'ultima parola delle etichette del parlante che restituisce nell'uno o nell'altro caso.

### Considerazioni sulle prestazioni per le etichette del parlante
{: #speakerLabelsPerformance}

Come notato in precedenza, la funzione di etichette del parlante è ottimizzata per le conversazioni tra due persone, come le comunicazioni con un call center. Pertanto, devi considerare i seguenti potenziali problemi delle prestazioni:

-   Le prestazioni per l'audio con un singolo parlante possono essere scadenti. Le variazioni nella qualità audio o nella voce del parlante possono indurre il servizio a identificare dei parlanti in più che non sono presenti. Tali parlanti sono indicati come allucinazioni.
-   In modo analogo, le prestazioni per l'audio con un parlante dominante, come ad esempio un podcast, possono essere scadenti. Il servizio tende a farsi sfuggire i parlanti che parlano per i periodi di tempo più brevi e può anche produrre delle allucinazioni.
-   Le prestazioni per l'audio con più di sei parlanti sono indefinite. La funzione può gestire un massimo di sei parlanti.
-   Le prestazioni per le espressioni brevi possono essere meno accurate che per quelle lunghe. Il servizio produce risultati migliori quando i partecipanti parlano per tempi più lunghi, almeno 30 secondi per ogni parlante. La quantità relativa di audio disponibile per ciascun parlante può anch'essa influire sulle prestazioni.
-   Le prestazioni possono ridursi per i primi 30 secondi di discorso. Di norma, migliorano a un livello ragionevole dopo 1 minuto di audio.

Come con tutte le trascrizioni, le prestazioni possono essere influenzate anche dalla scarsa qualità audio, dal rumore di fondo, dal modo di parlare di una persona, dai parlanti che si sovrappongono e da altri aspetti dell'audio.

## Individuazione di parole chiave
{: #keyword_spotting}

La funzione di individuazione di parole chiave rileva specifiche stringhe in una trascrizione. Il servizio può individuare la stessa parola chiave più volte e notificare ciascuna ricorrenza. Il servizio individua le parole chiave solo nei risultati finali, non in quelli provvisori. Per impostazione predefinita, il servizio non esegue alcuna individuazione di parole chiave.

Per utilizzare l'individuazione di parole chiave, devi specificare entrambi i seguenti parametri:

-   Utilizza il parametro `keywords` per specificare un array di stringhe da individuare. Il servizio non individua alcuna parola chiave se ometti il parametro o specifichi un array vuoto. Una stringa di parole chiave può includere più di un token. Ad esempio, la parola chiave `Speech to Text` ha tre token.

    Per l'inglese (Stati Uniti), il servizio normalizza ciascuna parola chiave per mettere in corrispondenza le stringhe pronunciate con quelle scritte. Ad esempio, normalizza i numeri per mettere in corrispondenza il modo in cui sono pronunciati rispetto a come sono scritti. Per le altre lingue, le parole chiave devono essere specificate come vengono pronunciate.

    Puoi individuare un massimo di 1000 parole chiave con una singola richiesta. Le parole chiave non sono sensibili a maiuscole/minuscole.
-   Utilizza il parametro `keywords_threshold` per specificare una probabilità tra 0.0 e 1.0 di una corrispondenza di parola chiave. La soglia indica il limite inferiore per il livello di attendibilità che il servizio deve avere perché una parola corrisponda alla parola chiave. Una parola chiave viene individuata nella trascrizione solo se la sua attendibilità è superiore o uguale alla soglia specificata.

    La specifica di una soglia ridotta può, potenzialmente, produrre molte corrispondenze. Se specifichi una soglia, devi anche specificare una o più parole chiave. Ometti il parametro per non restituire alcuna corrispondenza.

L'individuazione di parole chiave è necessaria per identificare le parole chiave in un flusso audio. Non puoi identificare le parole chiave elaborando una trascrizione finale perché la trascrizione rappresenta i migliori risultati della decodifica del servizio per l'audio di input. Non include i token con punteggi di attendibilità più bassi che potrebbero rappresentare una parola rilevante. Pertanto, l'applicazione degli strumenti di elaborazione del testo a una trascrizione sul lato client potrebbe non identificare le parole chiave. È necessaria una rappresentazione più nutrita di risultati della decodifica e tale rappresentazione è disponibile solo sul server.

### Risultati dell'individuazione di parole chiave
{: #keywordSpottingResults}

Il servizio restituisce i risultati in un campo `keywords_result` che è un elemento dell'array `results`. Il campo `keywords_result` è un dizionario, o un array associativo, di proprietà enumerabili. Ciascuna proprietà è identificata da una specifica parola chiave e include un array di oggetti. Il servizio restituisce un elemento dell'array per ciascuna corrispondenza che trova per la parola chiave. L'oggetto per ogni corrispondenza comprende i seguenti campi:

-   `normalized_text` è la parola chiave specificata che viene normalizzata alla frase pronunciata che corrispondeva nell'audio di input.
-   `start_time` è il tempo di inizio, in secondi, della corrispondenza.
-   `end_time` è il tempo di fine, in secondi, della corrispondenza.
-   `confidence` è l'attendibilità del servizio che la corrispondenza rappresenti la parola chiave specificata. L'attendibilità deve essere almeno grande quanto la soglia specificata per essere inclusa nei risultati.

Una parola chiave per cui il servizio non trova alcuna corrispondenza viene omessa dall'array. Una parola chiave potrebbe non essere trovata se

-   L'audio semplicemente non include la parola chiave. L'assenza della parola chiave è la spiegazione più ovvia.
-   La soglia è impostata troppo alta. Il servizio potrebbe identificare la parola chiave ma con un livello più basso di attendibilità, nel qual caso omette la corrispondenza dai risultati.
-   Una stringa di parole chiave che contiene più token (ad esempio, `Speech to Text`) viene espressa con troppo silenzio tra i suoi token. Quando il servizio trascrive l'audio, ritaglia il flusso in una serie di blocchi. Ogni blocco rappresenta una porzione continua di audio che non ha un intervallo di silenzio che supera il mezzo secondo. Crea un array di oggetti di risultato che è formato da questi blocchi.

    Il servizio mette in corrispondenza una parola chiave multi-token solo se

    -   I token della parola chiave sono nello stesso blocco.
    -   I token sono adiacenti o separati da uno scarto di non più di 0,1 secondi.

    Quest'ultimo caso può verificarsi se un breve riempitivo, o espressione non lessicale, come "um" o "beh," è presente tra due token della parola chiave. Per ulteriori informazioni, vedi [Indicatori di esitazione](/docs/services/speech-to-text?topic=speech-to-text-basic-response#hesitation).

### Esempio di individuazione di parole chiave
{: #keywordSpottingExample}

La seguente richiesta di esempio imposta il parametro `keywords` su un array con codifica URL di tre stringhe (`colorado`, `tornado` e `tornadoes`) e il parametro `keywords_threshold` su `0.5`. Il servizio trova le ricorrenze che soddisfano i requisiti di `colorado` e `tornadoes`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?keywords=%22colorado%22%2C%22tornado%22%2C%22tornadoes%22&keywords_threshold=0.5"
```
{: pre}

```javascript
{
  "results": [
    {
      "keywords_result": {
        "colorado": [
          {
            "normalized_text": "Colorado",
            "start_time": 4.94,
            "confidence": 0.91,
            "end_time": 5.62
          }
        ],
        "tornadoes": [
          {
            "normalized_text": "tornadoes",
            "start_time": 1.52,
            "confidence": 1.0,
            "end_time": 2.15
          }
        ]
      },
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Numero massimo di alternative
{: #max_alternatives}

Il parametro `max_alternative` accetta un valore numerico intero che indica al servizio di restituire le *n* migliori ipotesi alternative per i risultati. Per impostazione predefinita, il servizio restituisce solo un singolo risultato della trascrizione, che è equivalente all'impostazione del parametro su `1`. Impostando `max_alternatives` su un numero maggiore di 1, chiedi al servizio di restituire quel numero di trascrizioni delle migliori alternative. (Se specifichi un valore di `0`, il servizio utilizza il valore predefinito `1`.)

Il servizio notifica un punteggio di attendibilità solo per la migliore alternativa che restituisce. Nella maggior parte dei casi, questa è l'alternativa da scegliere.

### Esempio di numero massimo di alternative
{: #maximumAlternativesExample}

La seguente richiesta di esempio imposta il parametro `max_alternatives` su `3`; il servizio notifica un'attendibilità per la più probabile delle tre alternative.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?max_alternatives=3"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touched down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado and Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Risultati provvisori
{: #interim}

La funzione dei risultati provvisori è disponibile solo con l'interfaccia WebSocket.
{: note}

I risultati provvisori sono le ipotesi intermedie di una trascrizione che probabilmente varierà prima che il servizio restituisca i suoi risultati finali. Il servizio restituisce i risultati provvisori non appena li genera. I risultati provvisori sono utili per

-   Applicazioni interattive e trascrizione in tempo reale
-   Flussi audio lunghi, la cui trascrizione può richiedere un po' di tempo

I risultati provvisori arrivano più spesso e più rapidamente dei risultati finali. Puoi utilizzarli per abilitare la tua applicazione a rispondere più velocemente o per misurare lo stato di avanzamento della trascrizione.

Per ricevere i risultati provvisori, imposta il parametro JSON `interim_results` su `true` nel messaggio `start` per una richiesta di riconoscimento WebSocket. Se ometti il parametro `interim_results` o lo imposti su `false`, il servizio restituisce solo una singola trascrizione JSON alla fine dell'audio. Per utilizzare il parametro, attieniti alle seguenti linee guida:

-   Ometti il parametro oppure impostalo su `false` se stai eseguendo la trascrizione offline o batch, non hai bisogno della latenza minima o desideri un singolo risultato JSON per tutto l'audio.
-   Imposta il parametro su `true` se desideri che i risultati arrivino in modo progressivo man mano che il servizio elabora l'audio o se desideri i risultati con una latenza minima. Tieni presente che il servizio può aggiornare i risultati provvisori man mano che elabora ulteriore audio.

I risultati provvisori sono indicati nella risposta JSON con il campo `final` impostato su `false`. Il servizio può aggiornare tali risultati con trascrizione più accurate man mano che elabora ulteriore audio. I risultati finali sono identificati con il campo `final` impostato su `true`. Il servizio non esegue ulteriori aggiornamenti dei risultati finali.

### Esempio di risultati provvisori
{: #interimResultsExample}

Il seguente esempio abbreviato richiede i risultati provvisori per una richiesta WebSocket. Nella sua risposta, il servizio imposta l'attributo `final` su `true` solo per il risultato finale.

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token;
var websocket = new WebSocket(wsURI);

websocket.onopen = function(evt) { onOpen(evt) };
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050',
    interim_results: true
  };
  websocket.send(JSON.stringify(message));
  websocket.send(blob);
}

websocket.onmessage = function(evt) { onMessage(evt) };
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several to "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  . . .
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Alternative alle parole
{: #word_alternatives}

La funzione di alternative alle parole (nota anche come Confusion Networks) notifica le ipotesi per le alternative acusticamente simili per le parole dell'audio di input. Ad esempio, la parola `Austin` potrebbe essere la migliore ipotesi per una parola dall'audio. La parola `Boston` è però un'altra possibile ipotesi nello stesso intervallo di tempo. Le ipotesi condividono un tempo di inizio e uno di fine comuni ma hanno ortografie differenti e punteggi di attendibilità di solito differenti.

Per impostazione predefinito, il servizio non notifica le alternative alle parole. Per indicare che desideri ricevere le ipotesi alternative, utilizzi il parametro `word_alternatives_threshold` per specificare una probabilità compresa tra 0.0 e 1.0. La soglia indica il limite inferiore per il livello di attendibilità che il servizio deve attribuire a un'ipotesi per restituirla come alternativa di parola. Un'ipotesi viene restituita solo se la sua attendibilità è maggiore o uguale alla soglia specificata.

Puoi pensare alle alternative alle parole come la linea temporale per una trascrizione che è segmentata in intervalli più piccoli, o contenitori. Ciascun contenitore può avere una o più ipotesi con ortografie e attendibilità differenti. Il parametro `word_alternatives_threshold` controlla la densità dei risultati restituita dal servizio. La specifica di una soglia ridotta può, potenzialmente, produrre molte ipotesi.

### Risultati delle alternative alle parole
{: #wordAlternativesResults}

Il servizio restituisce i risultati in un campo `word_alternative` che è un elemento dell'array `results` . Il campo `word_alternative` è un array di oggetti, ciascuno dei quali fornisce i seguenti campi per una parola alternativa:

-   `start_time` indica il tempo, in secondi, a cui inizia nell'audio di input la parola per cui vengono restituite le ipotesi.
-   `end_time` indica il tempo, in secondi, a cui finisce nell'audio di input la parola per cui vengono restituite le ipotesi.
-   `alternatives` è un array di oggetti di ipotesi. Ciascun oggetto include un'attendibilità (`confidence`) che indica il punteggio di attendibilità del servizio per l'ipotesi e una parola (`word`) che identifica l'ipotesi. L'attendibilità deve essere almeno grande quanto la soglia specificata per essere inclusa nei risultati. Il servizio ordina le alternative in base all'attendibilità.

### Esempio di alternative alle parole
{: #wordAlternativesExample}

La seguente richiesta di esempio specifica una `word_alternatives_threshold` di `0.9`. L'output include potenziali ipotesi e punteggi di attendibilità per un certo numero di parole, insieme ai loro tempi di inizio e fine.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.9"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 1.01,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "several"
            }
          ],
          "end_time": 1.52
        },
        {
          "start_time": 1.52,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "tornadoes"
            }
          ],
          "end_time": 2.15
        },
        {
          "start_time": 3.39,
          "alternatives": [
            {
              "confidence": 0.96,
              "word": "severe"
            }
          ],
          "end_time": 3.77
        },
        {
          "start_time": 3.77,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "thunderstorms"
            }
          ],
          "end_time": 4.51
        },
        {
          "start_time": 4.51,
          "alternatives": [
            {
              "confidence": 0.97,
              "word": "swept"
            }
          ],
          "end_time": 4.81
        }
      ],
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Attendibilità delle parole
{: #word_confidence}

Il parametro `word_confidence` indica al servizio se fornire misure di attendibilità per le parole della trascrizione. Per impostazione predefinita, il servizio notifica una misura di attendibilità solo per la trascrizione finale nel suo insieme. L'impostazione di `word_confidenza` su `true` indica al servizio di notificare una misura di attendibilità per ogni singola parola della trascrizione.

Una misura di attendibilità indica la stima del servizio che la parola trascritta sia corretta in base alla prova acustica. I punteggi di attendibilità vanno da 0.0 a 1.0.

-   Un punteggio di 1.0 indica che la trascrizione corrente della parola riflette il risultato più probabile.
-   Un punteggio di 0.5 significa che la parola ha una probabilità del 50 percento di essere corretta.

### Esempio di attendibilità delle parole
{: #wordConfidenceExample}

Il seguente esempio richiede i punteggi di attendibilità per le parole della trascrizione.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_confidence=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday ",
          "word_confidence": [
            [
              "several",
              1.0
            ],
            [
              "tornadoes",
              1.0
            ],
            [
              "touch",
              0.52
            ],
            [
              "down",
              0.90
            ],
            . . .
            [
              "on",
              0.31
            ],
            [
              "Sunday",
              0.99
            ]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Date/ore delle parole
{: #word_timestamps}

Il parametro `timestamps` indica al servizio se produrre date/ore per le parole che trascrive. Per impostazione predefinita, il servizio non notifica alcuna data/ora. L'impostazione di `timestamps` su `true` indica al servizio di notificare il tempo di inizio e fine in secondi per ciascuna parola relativamente all'inizio dell'audio.

### Esempio di date/ore di parole
{: #wordTimestampsExample}

Il seguente esempio richiede le date/ore per le parole della trascrizione.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?timestamps=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "several",
              1.01,
              1.52
            ],
            [
              "tornadoes",
              1.52,
              2.15
            ],
            [
              "touch",
              2.15,
              2.5
            ],
            [
              "down",
              2.5,
              2.81
            ],
            . . .
            [
              "on",
              5.62,
              5.74
            ],
            [
              "Sunday",
              5.74,
              6.34
            ]
          ],
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Formattazione intelligente
{: #smart_formatting}

La funzione di formattazione intelligente è una funzionalità beta disponibile per inglese (Stati Uniti), giapponese e spagnolo.
{: note}

Il parametro `smart_formatting` indica al servizio di convertire le seguenti stringhe in rappresentazioni più convenzionali:

-   Date
-   Ore
-   Serie di cifre e numeri
-   Numeri di telefono
-   Valori valutari
-   Indirizzi web e email internet

Imposti il parametro `smart_formatting` su `true` per abilitare la formattazione intelligente. Per impostazione predefinita, il servizio non esegue la formattazione intelligente.

Il servizio applica la formattazione intelligente solo alla trascrizione finale di una richiesta di riconoscimento. Applica la formattazione intelligente immediatamente prima di restituire i risultati al client, dopo che è stata completata la normalizzazione del testo. La conversione rende più leggibile la trascrizione e consente una migliore post-elaborazione dei risultati della trascrizione.

### Punteggiatura (inglese Stati Uniti)
{: #smartFormattingPunctuation}

Per l'inglese (Stati Uniti), la funzione indica anche al servizio di sostituire con simboli di punteggiatura le seguenti stringhe di parole chiave pronunciate nell'audio.

<table style="width:50%">
  <caption>Tabella 2. Parole chiave di punteggiatura della formattazione intelligente</caption>
  <tr>
    <th style="width:45%; text-align:left">Stringa di parole chiave</th>
    <th style="text-align:center">Punteggiatura risultante</th>
  </tr>
  <tr>
    <td>
      Virgola
    </td>
    <td style="text-align:center">
      <code>,</code>
    </td>
  </tr>
  <tr>
    <td>
      Punto
    </td>
    <td style="text-align:center">
      <code>.</code>
    </td>
  </tr>
  <tr>
    <td>
      Punto interrogativo
    </td>
    <td style="text-align:center">
      <code>?</code>
    </td>
  </tr>
  <tr>
    <td>
      Punto esclamativo
    </td>
    <td style="text-align:center">
      <code>!</code>
    </td>
  </tr>
</table>

Il servizio converte queste stringhe di parole chiave in simboli solo nei punti appropriati. Ad esempio, il servizio converte la frase pronunciata

```
the warranty period is short period
```
{: codeblock}

nel seguente testo in una trascrizione

```
the warranty period is short.
```
{: codeblock}

### Differenze linguistiche
{: #smartFormattingDifferences}

La formattazione intelligente è basata sulla presenza di parole chiave ovvie nella trascrizione. Le lingue supportate gestiscono la formattazione intelligente in modo leggermente diverso.

#### Inglese (Stati Uniti) e spagnolo
{: #smartFormattingEnglishSpanish}

-   Le ore sono identificate da parole chiave quali `AM`, `PM` o `EST`.
-   Le ore in formato a 24 ore sono convertite se sono identificate dalla parola chiave `hours`.
-   I numeri di telefono devono essere `911` oppure un numero con 10 o 11 cifre che inizia con il numero `1`.

#### Giapponese
{: #smartFormattingJapanese}

-   Gli indirizzi web e email internet non vengono convertiti.
-   I numeri di telefono devono essere 10 o 11 cifre e iniziare con dei prefissi validi per i numeri di telefono in Giappone. Ad esempio, dei prefissi validi includono `03` e `090`.
-   Le parole in inglese sono convertite in caratteri ASCII (*hankaku*). Ad esempio, <code>&#65321;&#65314;&#65325;</code> viene convertito in `IBM`.
-   I termini ambigui potrebbero non essere convertiti se non è disponibile un contesto sufficiente. Ad esempio, non è chiaro se <code>&#19968;&#26178;</code> e <code>&#21313;&#20998;</code> fanno riferimento a delle date/ore.
-   La punteggiatura viene gestita allo stesso modo con o senza formattazione intelligente. Ad esempio, in base ai calcoli delle probabilità. viene selezionato uno di <code>&#12459;&#12531;&#12510;</code> o `,`.

### Esempio di formattazione intelligente
{: #smartFormattingExample}

Il seguente esempio richiede la formattazione intelligente con una richiesta di riconoscimento impostando il parametro `smart_formatting` su `true`. Le seguenti sezioni mostrano gli effetti della formattazione intelligente sui risultati di una richiesta.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?smart_formatting=true"
```
{: pre}

### Risultati della formattazione intelligente
{: #smartFormattingResults}

La seguente tabella mostra degli esempi di trascrizioni finali sia con che senza la formattazione intelligente. Le trascrizioni sono basate sull'audio in inglese (Stati Uniti).

<table summary="Ogni riga di intestazione è seguita da più righe di esempio che mostrano l'effetto della formattazione intelligente per l'elemento identificato nell'intestazione.">
  <caption>Tabella 3. Trascrizioni di esempio della formattazione intelligente</caption>
  <tr>
    <th id="without_formatting" style="width:45%; text-align:left">Senza
      la formattazione intelligente</th>
    <th id="with_formatting" style="text-align:left">Con la
      formattazione intelligente</th>
  </tr>
  <tr>
    <th id="Dates" colspan="2">
      <strong>Date</strong>
    </th>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on ten oh six nineteen seventy
    </td>
    <td headers="Dates with_formatting">
      I was born on 10/6/1970
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on the ninth of December nineteen hundred
    </td>
    <td headers="Dates with_formatting">
      I was born on 12/9/1900
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      Today is June sixth
    </td>
    <td headers="Dates with_formatting">
      Today is June 6
    </td>
  </tr>
  <tr>
    <th id="Times" colspan="2">
      <strong>Ore</strong>
    </th>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      The meeting starts at nine thirty AM
    </td>
    <td headers="Times with_formatting">
      The meeting starts at 9:30 AM
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      I am available at seven EST
    </td>
    <td headers="Times with_formatting">
      I am available at 7:00 EST
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      We meet at oh seven hundred hours
    </td>
    <td headers="Times with_formatting">
      We meet at 0700 hours
    </td>
  </tr>
  <tr>
    <th id="Numbers" colspan="2">
      <strong>Numeri</strong>
    </th>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      The quantity is one million one hundred and one
    </td>
    <td headers="Numbers with_formatting">
      The quantity is 1000101
    </td>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      One point five is between one and two
    </td>
    <td headers="Numbers with_formatting">
      1.5 is between 1 and 2
    </td>
  </tr>
  <tr>
    <th id="phone_numbers" colspan="2">
      <strong>Numeri di telefono</strong>
    </th>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at nine one four two three seven one thousand
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 914-237-1000
    </td>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at one nine one four nine oh nine twenty six forty five
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 1-914-909-2645
    </td>
  </tr>
  <tr>
    <th id="currency_values" colspan="2">
      <strong>Valori valutari</strong>
    </th>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      You owe me three thousand two hundred two dollars and sixty six
      cents
    </td>
    <td headers="currency_values with_formatting">
      You owe me $3202.66
    </td>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      The dollar rose to one hundred and nine point seven nine yen from
      one hundred and nine point seven two yen
    </td>
    <td headers="currency_values with_formatting">
      The dollar rose to 109.79 yen from 109.72 yen
    </td>
  </tr>
  <tr>
    <th id="internet_addresses" colspan="2">
      <strong>Indirizzi web e email internet</strong>
    </th>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      My email address is john dot doe at foo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      My email address is john.doe at foo.com
    </td>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      I saw the story on yahoo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      I saw the story on yahoo.com
    </td>
  </tr>
  <tr>
    <th id="Combinations" colspan="2">
      <strong>Combinazioni</strong>
    </th>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      The code is zero two four eight one and the date of service
      is May fifth two thousand and one
    </td>
    <td headers="Combinations with_formatting">
      The code is 02481 and the date of service is 5/5/2001
    </td>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      There are forty seven links on Yahoo dot com now
    </td>
    <td headers="Combinations with_formatting">
      There are 47 links on Yahoo.com now
    </td>
  </tr>
</table>

### Trascrizioni di esempio con pause lunghe
{: #smartFormattingLongPauses}

Nei casi in cui un'espressione contenga silenzi abbastanza lunghi, il servizio può suddividere la trascrizione in due o più porzioni finali. Ciò influenza i risultati della formattazione, come mostrato nei seguenti esempi.

<table>
  <caption>Tabella 4. Trascrizioni di esempio della formattazione intelligente per le pause lunghe</caption>
  <tr>
    <th style="width:45%; text-align:left">Discorso audio</th>
    <th style="text-align:left">Risultati della trascrizione formattata</th>
  </tr>
  <tr>
    <td>
      My phone number is nine one four five five seven three
      three nine two
    </td>
    <td>
      "My phone number is 914-557-3392"
    </td>
  </tr>
  <tr>
    <td>
      My phone number is nine one four <em>&lt;pause&gt;</em> five five
      seven three three nine two
    </td>
    <td>
      "My phone number is 914"<br/>
      "5573392"
    </td>
  </tr>
</table>

## Oscuramento numerico
{: #redaction}

La funzione di oscuramento numerico è una funzionalità beta disponibile per inglese (Stati Uniti), giapponese e coreano.
{: note}

Il parametro `redaction` indica al servizio di oscurare, o mascherare, i dati numerici dalle trascrizioni finali. La funzione oscura qualsiasi numero che abbia tre o più cifre consecutive sostituendo a ogni cifra un carattere `X`. Il suo utilizzo previsto è per oscurare i dati numerici sensibili, come ad esempio i numeri di carta di credito.

Per impostazione predefinita, il servizio non oscura i dati numerici. Imposta il parametro `redaction` su `true` per abilitare l'oscuramento numerico. Quando abiliti l'oscuramento, il servizio abilita automaticamente la formattazione intelligente, indipendentemente dal fatto che tu disabiliti esplicitamente tale funzione. Per garantire la massima sicurezza, il servizio applica anche i seguenti valori di parametro quando abiliti l'oscuramento:

-   Il servizio disabilita l'individuazione di parole chiave, indipendentemente dal fatto che specifichi dei valori per i parametri `keywords` e `keywords_threshold`.
-   Il servizio disabilita i risultati provvisori, indipendentemente dal fatto che imposti il parametro `interim_results` dell'interfaccia WebSocket su `true`.
-   Il servizio restituisce solo una singola trascrizione finale, indipendentemente dal fatto che specifichi un valore per il parametro `maximum_alternatives`.

La progettazione della funzione è simile a quella della funzione di formattazione intelligente esistente. Il servizio applica l'oscuramento solo alla trascrizione finale di una richiesta di riconoscimento, subito prima di restituire i risultati al client e dopo che è stata completata la normalizzazione del testo.

Le versioni future della funzione potrebbero espandere l'oscuramento per mascherare tutti i numeri di telefono, gli indirizzi, i conti bancari, i numeri di previdenza sociale e altri dati numerici sensibili.
{: note}

### Differenze linguistiche
{: #redactionDifferences}

La funzione opera esattamente come descritto per i modelli in inglese (Stati Uniti) ma presenta le seguenti differenze per i modelli in giapponese e coreano.

#### Giapponese
{: #redactionJapanese}

L'oscuramento in giapponese presenta le seguenti differenze:

-   Oltre a mascherare le stringhe di tre o più cifre consecutive, l'oscuramento maschera anche indirizzi e numeri, indipendentemente dal fatto che contengano meno di tre cifre.
-   In modo analogo, l'oscuramento maschera le informazioni relative alle date nelle date di nascita in stile giapponese. In giapponese, le informazioni sulle date sono di norma presentate in formato *Anno Domini* in latino ma, a volte, rispettano lo stile giapponese, in particolare per le date di nascita. In questo caso, l'anno e il mese sono mascherati anche se contengono solo una o due cifre. Ad esempio, l'oscuramento numerico modifica la seguente stringa come mostrato.

    <table style="width:50%">
      <caption>Tabella 5. Oscuramento di esempio di una data di nascita in stile giapponese</caption>
      <tr>
        <th style="text-align:left">Senza oscuramento</th>
        <th style="text-align:left">Con oscuramento</th>
      </tr>
      <tr>
        <td>
          &#24179;&#25104; 30&#24180; 2&#26376;
        </td>
        <td>
          &#24179;&#25104; XX&#24180; X&#26376;
        </td>
      </tr>
    </table>

#### Coreano
{: #redactionKorean}

L'oscuramento in coreano presenta le seguenti differenze:

-   La funzione di formattazione intelligente non è supportata. Il servizio esegue comunque l'oscuramento numerico per il coreano ma non esegue alcuna altra formattazione intelligente.
-   I caratteri cifra isolati vengono oscurati, ma lo stesso non succede per i possibili caratteri cifra inclusi come parte delle frasi in coreano. Ad esempio, il carattere <code>&#51060;</code> nella seguente frase non viene sostituito da un `X` perché è adiacente al seguente carattere:

    <code>&#51060;&#51077;&#45768;&#45796;</code>

    Se il carattere <code>&#51060;</code> fosse separato dal seguente carattere da uno spazio, verrebbe sostituito da una `X`, come descritto in [Risultati dell'oscuramento numerico](#redactionResults).

### Esempio di oscuramento numerico
{: #redactionExample}

Il seguente esempio richiede un oscuramento numerico con una richiesta di riconoscimento impostando il parametro `redaction` su `true`. Poiché la richiesta abilita l'oscuramento, il servizio abilita implicitamente la formattazione intelligente con la richiesta. Il servizio disabilita effettivamente gli altri parametri della richiesta in modo che non abbiano alcun effetto: il servizio restituisce una singola trascrizione finale e non riconosce alcuna parola chiave. La seguente sezione mostra gli effetti dell'oscuramento.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?&redaction=true&max_alternatives=3&keywords=%22birth%22%2C%22birthday%22&keywords_threshold=0.5"
```
{: pre}

### Risultati dell'oscuramento numerico
{: #redactionResults}

La seguente tabella mostra degli esempi di trascrizioni finali sia con che senza un oscuramento numerico in ogni lingua supportata.

<table style="width:90%" summary="Ciascuna riga di intestazione identifica una lingua ed è seguita da una singola riga che mostra la stessa trascrizione sia senza che con l'oscuramento numerico abilitato.">
  <caption>Tabella 6. Trascrizioni di esempio di oscuramento numerico</caption>
  <tr>
    <th id="without_redaction" style="text-align:left">Senza oscuramento</th>
    <th id="with_redaction" style="text-align:left">Con oscuramento</th>
  </tr>
  <tr>
    <th id="US_English" colspan="2">
      <strong>Inglese (Stati Uniti)</strong>
    </th>
  </tr>
  <tr>
    <td headers="US_English without_redaction">
      my credit card number is four one four seven two
    </td>
    <td headers="US_English with_redaction">
      my credit card number is XXXXX
    </td>
  </tr>
  <tr>
    <th id="Japanese" colspan="2">
      <strong>Giapponese</strong>
    </th>
  </tr>
  <tr>
    <td headers="Japanese without_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; &#22235; &#19968; &#22235; &#19971; &#20108;&#12391;&#12377;
    </td>
    <td headers="Japanese with_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; XXXXX &#12391;&#12377;
    </td>
  </tr>
  <tr>
    <th id="Korean" colspan="2">
      <strong>Coreano</strong>
    </th>
  </tr>
  <tr>
    <td headers="Korean without_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; &#49324; &#51068; &#49324; &#52832; &#51060; &#48264;&#51077;&#45768;&#45796;
    </td>
    <td headers="Korean with_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; XXXXX &#48264;&#51077;&#45768;&#45796;
    </td>
  </tr>
</table>

## Filtro di contenuto volgare
{: #profanity_filter}

La funzione di filtro di contenuto volgare è generalmente disponibile solo per l'inglese (Stati Uniti).
{: note}

Il parametro `profanity_filter` indica se il servizio deve censurare il contenuto volgare nei suoi risultati. Per impostazione predefinita, il servizio oscura tutto il contenuto volgare sostituendolo con una serie di asterischi nella trascrizione. L'impostazione del parametro su `false` visualizza le parole nell'output esattamente come sono trascritte.

Il servizio censura il contenuto volgare in tutte le trascrizioni finali e in qualsiasi altra trascrizione alternativa. Censura anche il contenuto volgare nei risultati associati alle alternative alle parole, all'attendibilità delle parole e alle date/ore delle parole. La sola eccezione è l'individuazione di parole chiave, per cui il servizio restituisce tutte le parole come sono specificate dall'utente, indipendentemente dal fatto che `profanity_filter` sia `true`.

### Esempio di filtro di contenuto volgare
{: #profanityFilteringExample}

Il seguente esempio mostra i risultati da un breve file audio trascritto con il valore `true` predefinito per il parametro `profanity_filter`. La richiesta imposta anche il parametro `word_alternatives_threshold` su un valore relativamente alto di `0.99` e i parametri `word_confidence` e `timestamps` su `true`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.99&word_confidence=true&timestamps=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 0.03,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "****"
            }
          ],
          "end_time": 0.25
        },
        {
          "start_time": 0.25,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "you"
            }
          ],
          "end_time": 0.56
        }
      ],
      "alternatives": [
        {
          "transcript": "**** you",
          "confidence": 0.99,
          "word_confidence": [
            ["****", 1.0],
            ["you", 0.99]
          ],
          "timestamps": [
            ["****", 0.03, 0.25],
            ["you", 0.25, 0.56]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}
