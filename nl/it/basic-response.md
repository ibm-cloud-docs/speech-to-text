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

# Descrizione dei risultati del riconoscimento
{: #basic-response}

Indipendentemente dall'interfaccia che utilizzi, il servizio {{site.data.keyword.speechtotextfull}} restituisce i risultati della trascrizione che riflettono i parametri di input e output da te specificati. Il servizio restituisce tutto il contenuto della risposta JSON nel set di caratteri UTF-8.
{: shortdesc}

## Risposta della trascrizione di base
{: #response}

Il servizio restituisce la seguente risposta per gli esempi illustrati in [Effettuazione di una richiesta di riconoscimento](/docs/services/speech-to-text/basic-request.html). Gli esempi passano solo un file audio e il suo tipo di contenuto. L'audio pronuncia una singola frase senza pause evidenti tra le parole.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.89,
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

Il servizio restituisce un oggetto `SpeechRecognitionResults`, che è l'oggetto di risposta di primo livello. Per queste semplici richieste, l'oggetto include un campo `results` e un campo `result_index`:

-   Il campo `results` fornisce un array di informazioni sui risultati della trascrizione. Per questo esempio, il campo `alternatives` include i valori di `transcript` e `confidence` del servizio nei risultati. Il campo `final` ha il valore `true` per indicare che questi risultati non cambieranno.
-   Il campo `result_index` fornisce un identificativo univoco per i risultati. L'esempio mostra i risultati finali per una richiesta con un singolo file audio che non ha pause e la richiesta non include parametri aggiuntivi. Quindi il servizio restituisce un singolo campo `result_index` con un valore `0`, che è sempre l'indice iniziale.

Se l'audio di input è più complesso o la richiesta include parametri aggiuntivi, i risultati possono contenere molte più informazioni.

### Il campo alternatives
{: #responseAlternatives}

Il campo `alternatives` fornisce un array di risultati della trascrizione. Per questa richiesta, l'array include un singolo elemento.

-   Il campo `confidence` è un punteggio che indica l'attendibilità del servizio nella trascrizione, che per questo esempio si avvicina al 90 percento.
-   Il campo `transcript` fornisce i risultati della trascrizione.

I campi `final` e `result_index` qualificano il significato di questi campi.

### Il campo final
{: #responseFinal}

Il campo `final` indica se la trascrizione mostra i risultati finali della trascrizione:

-   Il campo è `true` per i risultati finali, ai quali non verrà apportata alcuna modifica. Il servizio non invia ulteriori aggiornamenti per le trascrizioni che restituisce come risultati finali.
-   Il campo è `false` per i risultati provvisori, che sono soggetti a modifiche. Se utilizzi il parametro `interim_results` con l'interfaccia WebSocket, il servizio restituisce ipotesi provvisorie in evoluzione sotto forma di più campi `results` durante la sua trascrizione dell'audio. Il campo `final` è sempre `false` per i risultati provvisori. Il servizio imposta il campo su `true` per i risultati finali dell'audio. Il servizio non invia ulteriori aggiornamenti per la trascrizione di quell'audio.

Per ottenere risultati provvisori, utilizza l'interfaccia WebSocket e imposta il parametro `interim_results` su `true`. Per ulteriori informazioni, vedi [Risultati provvisori](/docs/services/speech-to-text/output.html#interim).

### Il campo result_index
{: #responseResultIndex}

Il campo `result_index` fornisce un identificativo per i risultati che è univoco per quella richiesta. Se richiedi i risultati provvisori, il servizio invia più campi `results` per le ipotesi in evoluzione dell'audio di input. Gli indici dei risultati provvisori per lo stesso audio hanno sempre lo stesso valore, così come i risultati finali per lo stesso audio.

Una volta che hai ricevuto i risultati finali per qualsiasi audio, il servizio non invia ulteriori risultati con tale indice per il resto della richiesta. L'indice per eventuali ulteriori risultati viene incrementato di uno.

A prescindere che tu richieda o meno i risultati provvisori, il servizio può restituire più risultati finali con indici diversi se il tuo audio include pause o periodi prolungati di silenzio. Per ulteriori informazioni, vedi [Pause e silenzio](#pauses-silence).

Se il tuo audio produce più risultati finali, concatena gli elementi `transcript` dei risultati finali per assemblare la trascrizione completa dell'audio. Assembla i risultati ordinandoli per `result_index`. Puoi ignorare i risultati provvisori per i quali il campo `final` è `false`.

### Ulteriori contenuti di risposta
{: #responseAdditional}

Molti dei parametri di output disponibili per il riconoscimento vocale influiscono sui contenuti della risposta del servizio. Ad esempio, i seguenti parametri fanno in modo che il servizio restituisca ulteriori informazioni sull'audio:

-   Il parametro `max_alternatives` richiede più trascrizioni alternative. L'array `alternatives` include un elemento separato per ciascuna alternativa. Ogni elemento dell'array include un campo `transcript`. Solo l'alternativa più probabile include un campo `confidence`.
-   I parametri `word_confidence` e `timestamps` richiedono ulteriori informazioni sulle singole parole della trascrizione. L'array `alternatives` include campi aggiuntivi con quei nomi.
-   I parametri `keywords` e `keywords_threshold` richiedono l'individuazione di parole chiave. L'array `results` include un campo `keywords_result`.
-   Il parametro `word_alternatives_threshold` richiede risultati acusticamente simili per le singole parole. L'array `results` include un campo `word_alternatives`.
-   Il parametro `interim_results` dell'interfaccia WebSocket richiede ipotesi di trascrizione provvisorie. Come accennato in precedenza, il servizio invia più risposte mentre trascrive l'audio.
-   Il parametro `speaker_labels` identifica i singoli parlanti di uno scambio tra più partecipanti. La risposta include un campo `speaker_labels` allo stesso livello dei campi `results` e `results_index`.

Per ulteriori informazioni su questi e altri parametri che possono influire sulla risposta del servizio, vedi [Funzioni di output](/docs/services/speech-to-text/output.html). Per ulteriori informazioni su tutti i parametri disponibili, vedi [Riepilogo dei parametri](/docs/services/speech-to-text/summary.html).

## Pause e silenzio
{: #pauses-silence}

Il servizio trascrive un intero flusso audio finché il flusso non termina o si verifica un timeout. Tuttavia, se l'audio include pause o un silenzio prolungato tra le parole o le frasi pronunciate, i risultati del riconoscimento possono includere più risultati finali.

L'intervallo di pausa predefinito utilizzato dal servizio per determinare risultati finali separati è di circa un secondo. Per la maggior parte delle lingue, l'intervallo di pausa è esattamente di 0,8 secondi; per il cinese l'intervallo è di 0,6 secondi. Non puoi modificare la durata dell'intervallo di pausa.

Il modo in cui il servizio restituisce i risultati dipende dall'interfaccia che utilizzi. I seguenti esempi mostrano le risposte con due risultati finali dalle interfacce HTTP e WebSocket. In entrambi i casi viene utilizzato lo stesso audio di input. L'audio pronuncia la frase "one two three four five six," con una pausa di un secondo tra le parole "three" e "four."

-   *Per le interfacce HTTP,* il servizio invia un singolo oggetto `SpeechRecognitionResults`. L'array `alternatives` ha un elemento separato per ogni risultato finale. La risposta ha un singolo campo `result_index` con il valore `0`.

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
        {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

-   *Per l'interfaccia WebSocket,* il servizio invia due risposte separate con due diversi oggetti `SpeechRecognitionResults`. Ogni oggetto di risposta ha un campo `result_index` diverso, che ha il valore `0` per la prima risposta e `1` per la seconda risposta.

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
      ]
      "result_index": 0
    }
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
          "final": true
        }
      ],
      "result_index": 1
    }
    ```
    {: codeblock}

Se i tuoi risultati includono più risultati finali, concatena gli elementi `transcript` dei risultati finali per assemblare la trascrizione completa dell'audio.

Un silenzio di 30 secondi nell'audio inviato in streaming può comportare un [timeout di inattività](/docs/services/speech-to-text/input.html#timeouts-inactivity).
{: note}

## Indicatori di esitazione
{: #hesitation}

Il servizio può includere degli indicatori di esitazione in una trascrizione quando rileva brevi parole di riempimento o pause in un discorso. Note anche come disfluenze, tali pause possono includere parole di riempimento come "uhm", "uh", "hmm" ed espressioni non lessicali correlate. A meno che tu non debba utilizzarli per la tua applicazione, puoi tranquillamente filtrare gli indicatori di esitazione da una trascrizione.

In inglese, il servizio utilizza il token di esitazione `%HESITATION`, come mostrato nel seguente esempio. Altre lingue possono utilizzare indicatori diversi; ad esempio, il giapponese utilizza il token `D_`.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Gli indicatori di esitazione possono essere inclusi anche in altri campi di una trascrizione. Ad esempio, se richiedi le [date/ore](/docs/services/speech-to-text/output.html#word_timestamps) per le singole parole di una trascrizione, il servizio riporta l'ora di inizio e di fine di ciascun indicatore di esitazione.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            . . .
            [
              "that",
              7.31,
              7.69
            ],
            [
              "%HESITATION",
              7.69,
              7.98
            ],
            [
              "that's",
              7.98,
              8.41
            ],
            [
              "a",
              8.41,
              8.48
            ],
            . . .
          ],
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Maiuscole
{: #capitalization}

Per la maggior parte delle lingue, il servizio non utilizza maiuscole nelle trascrizioni di risposta. Se l'uso delle maiuscole è importante per la tua applicazione, devi scrivere in maiuscolo la prima parola di ogni frase e ogni altro termine per cui la maiuscola è appropriata.

*Solo per l'inglese americano,* il servizio scrive in maiuscolo molti nomi propri. Ad esempio, il servizio restituisce il seguente testo per la frase specificata:

```
Barack Obama graduated from Columbia University
```
{: codeblock}

Per altre lingue, il servizio restituisce il seguente testo:

```
barack obama graduated from columbia university
```
{: codeblock}

Il servizio applica sempre queste maiuscole all'inglese americano, a prescindere che utilizzi o meno la formattazione intelligente.

## Punteggiatura
{: #punctuation}

Il servizio non inserisce la punteggiatura nelle trascrizioni di risposta per impostazione predefinita. Devi aggiungere la punteggiatura che ti serve nei risultati del servizio.

Per l'inglese americano, puoi utilizzare la formattazione intelligente per indicare al servizio di sostituire i simboli di punteggiatura, come virgole, punti, punti interrogativi e punti esclamativi, per determinate stringhe di parole chiave. Per ulteriori informazioni, vedi [Formattazione intelligente](/docs/services/speech-to-text/output.html#smart_formatting).
