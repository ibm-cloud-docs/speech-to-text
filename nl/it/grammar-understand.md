---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-04"

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

# Informazioni sulle grammatiche
{: #grammarUnderstand}

I seguenti esempi introducono il supporto del servizio {{site.data.keyword.speechtotextfull}} per le grammatiche. Gli esempi creano due semplici grammatiche ABNF e mostrano i possibili risultati quando vengono utilizzate per il riconoscimento vocale. Gli esempi illustrano l'importanza di esaminare il punteggio di attendibilità che il servizio include con una trascrizione.
{: shortdesc}

Gli esempi forniscono solo i risultati delle richieste di riconoscimento vocale. Per gli esempi che mostrano come passare una grammatica per il riconoscimento vocale, vedi [Utilizzo di una grammatica per il riconoscimento vocale](/docs/services/speech-to-text?topic=speech-to-text-grammarUse). Gli esempi sono anche molto semplici. Per esempi di grammatiche più complesse, vedi [Grammatiche di esempio](/docs/services/speech-to-text?topic=speech-to-text-grammarExamples).

## Corrispondenze di singole frasi: la grammatica yesno
{: #yesnoGrammar}

Il primo esempio definisce una grammatica `yesno` molto semplice che accetta due risposte valide di una sola parola, `yes` e `no`. La grammatica è utile nei casi in cui l'utente deve rispondere con una sola delle due frasi.

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $yesno;

$yesno = yes | no ;
```
{: codeblock}

Quando applichi questa grammatica a una richiesta di riconoscimento vocale, il servizio può restituire una trascrizione con un punteggio che indica la sua attendibilità nella corrispondenza. Può anche non restituire alcun risultato se l'input chiaramente non corrisponde a una delle due frasi.

Ad esempio, se l'utente risponde `yes`, il servizio probabilmente restituisce una risposta che è molto simile al seguente risultato. Il punteggio nel campo `confidence` indica una corrispondenza perfettamente affidabile.

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 1.0,
          "transcript": "yes"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Ma supponiamo, ad esempio, che l'utente risponda `nope`. Il servizio può restituire un risultato con un punteggio di attendibilità molto basso o nessun risultato. Un risultato vuoto è l'indicazione più chiara che la risposta non corrisponde alla grammatica. È più probabile che una risposta vuota si verifichi con grammatiche complesse, in cui una risposta valida deve corrispondere a una specifica sequenza di più frasi.

## Corrispondenze di più frasi: la grammatica names
{: #namesGrammar}

Con una grammatica a più frasi, la risposta dell'utente deve essere completa per essere riconosciuta. L'utente non può omettere una parola o fermarsi nel mezzo della risposta. L'assenza di anche solo una parola può far sì che il servizio restituisca un risultato vuoto.

Inoltre, il servizio può restituire più trascrizioni se l'utente pronuncia le frasi separate da sufficiente silenzio per indicare che si tratta di espressioni indipendenti. Ad esempio, considera la semplice grammatica `names`, che può corrispondere a uno dei tre nomi di più parole.

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $names;
$names = Yi Wen Tan | Yon See | Youngjoon Lee ;
```
{: codeblock}

Supponiamo che l'utente pronunci uno dei nomi dalle regole della grammatica, `Yon See`. Il servizio restituisce una risposta che indica un livello molto alto di attendibilità nella corrispondenza.

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.92,
          "transcript": "Yon See"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Supponiamo ora che l'utente pronunci due nomi separati da abbastanza silenzio, almeno 0,8 secondi, per indicare che si tratta di espressioni separate: `Yon See` [1,0 secondi di silenzio] `Yi Wen Tan`. In questo caso, il servizio invia due risposte separate con un punteggio di attendibilità diverso per ogni trascrizione.

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.92,
          "transcript": "Yon See"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
},
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.83,
          "transcript": "Yi Wen Tan"
        }
      ],
      "final": true
    }
  ],
  "result_index": 1
}
```
{: codeblock}

Infine, considera il caso in cui l'utente dice invece qualcosa come `Yon See` [1,0 secondi di silenzio] `Young Says He`. Per la prima frase, `Yon See`, il servizio invia una corrispondenza positiva con un punteggio di attendibilità simile agli esempi precedenti. Per la seconda frase, `Young Says He`, il servizio può avere una delle due possibili risposte:

-   Potrebbe non inviare alcuna risposta, che indica che la frase non corrisponde a una delle regole della grammatica.
-   Potrebbe invece inviare una risposta con un basso punteggio di attendibilità, il che indica che la frase è acusticamente simile a una delle regole ma non è una corrispondenza probabile.

## Punteggi di attendibilità e risultati vuoti
{: #confidenceScores}

Per grammatiche relativamente semplici come quelle degli esempi precedenti, il servizio può restituire un risultato con un basso punteggio di attendibilità anche quando la risposta non sembra corrispondere alla grammatica. Ciò potrebbe sembrare sorprendente, ma una risposta con un basso punteggio di attendibilità può indicare la migliore corrispondenza che il servizio potrebbe trovare per l'audio fornito, anche se è improbabile che la risposta corrisponda alla grammatica. Se la risposta non corrisponde alla grammatica, il valore di attendibilità dei risultati è in genere sufficientemente basso da indicare l'improbabilità che la grammatica possa generare la risposta.

Considera sempre il punteggio di attendibilità quando valuti se una risposta soddisfa una grammatica. Se non sai quale soglia impostare per il rifiuto di un risultato in base alla sua attendibilità, utilizza un valore iniziale di 0,5. Se riscontri una percentuale inaspettatamente elevata di risultati corretti rifiutati, riduci la soglia di attendibilità; ad esempio, 0,1 può essere una buona opzione per alcune applicazioni. Se riscontri un numero elevato di riconoscimenti non corretti, aumenta la soglia di attendibilità per la tua applicazione.

In molti casi, un risultato vuoto o un risultato con un punteggio di attendibilità molto basso è una risposta valida. Può indicare che l'utente non ha compreso la domanda o le opzioni disponibili. Puoi sempre riconoscere la risposta audio dell'utente senza la grammatica, ma corri il rischio di ottenere un risultato non valido per la grammatica. Le grammatiche forniscono risultati più affidabili rispetto agli n-grammi, che restituiscono sempre un risultato per qualsiasi cosa che non sia il completo silenzio.

Sapere che il risultato deve essere una delle frasi valide definite da una grammatica è una potente funzione che può semplificare la gestione della risposta di un'applicazione. In generale, il servizio può restituire risultati con alta attendibilità solo per le espressioni generate da una grammatica. Perché la grammatica `yesno` nel primo esempio riconosca in modo affidabile la frase `nope` come risposta valida, devi aggiungere tale frase alla grammatica.
