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

# Grammatiche di esempio
{: #grammarExamples}

I seguenti esempi descrivono usi più complessi per grammatiche con il riconoscimento vocale. La maggior parte degli esempi sono forniti sia in formato ABNF (`application/srgs`) che in formato XML (`application/srgs+xml`). Utilizza questi esempi per aiutarti a iniziare con le grammatiche.
{: shortdesc}

In una regola di produzione per una grammatica, l'inclusione di un carattere `\` (barra rovesciata) è un errore di sintassi in ABNF e viene interpretato come valore letterale in XML.
{: tip}

## Grammatiche di conferma
{: #confirmation}

Le grammatiche di conferma sono utili per le applicazioni che prevedono una risposta di una sola parola in risposta a una domanda. Le seguenti grammatiche definiscono un elenco di possibili risposte sì e no.

<table style="width:50%">
  <caption>Tabella 1. Grammatiche di conferma</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.abnf" download="confirm.abnf">confirm.abnf <img src="../../icons/launch-glyph.svg" alt="Icona link esterno" title="Icona link esterno"></a>
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.xml" download="confirm.xml">confirm.xml <img src="../../icons/launch-glyph.svg" alt="Icona link esterno" title="Icona link esterno"></a>
    </td>
  </tr>
</table>

## Grammatiche di elenco
{: #list}

Le grammatiche di elenco sono utili per le applicazioni che si aspettano che l'utente selezioni un'opzione da una serie predefinita di stringhe. La serie viene a volte denominata elenco semplice. Generalmente è costituita da elementi terminali e non include una struttura di regole complicata.

-   Le seguenti grammatiche definiscono un elenco di frasi per le cifre.

    <table style="width:50%">
      <caption>Tabella 2. Grammatiche di numeri di elenco</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.abnf" download="list-numbers.abnf">list-numbers.abnf <img src="../../icons/launch-glyph.svg" alt="Icona link esterno" title="Icona link esterno"></a>
        </td>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.xml" download="list-numbers.xml">list-numbers.xml <img src="../../icons/launch-glyph.svg" alt="Icona link esterno" title="Icona link esterno"></a>
        </td>
      </tr>
    </table>

-   La seguente grammatica definisce un elenco di nomi validi tra cui l'utente può scegliere, magari per selezionare un individuo da una rubrica telefonica.

    <table style="width:50%">
      <caption>Tabella 3. Grammatica di nomi di elenco</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-names.abnf" download="list-names.abnf">list-names.abnf <img src="../../icons/launch-glyph.svg" alt="Icona link esterno" title="Icona link esterno"></a>
        </td>
        <td style="text-align:center">
          Non disponibile
        </td>
      </tr>
    </table>

## Grammatiche di VIN
{: #vin}

I VIN (o vehicle identification number) utilizzano un formato alfanumerico fisso, così come i numeri delle carte di credito, i numeri di telefono e i numeri di previdenza sociale degli Stati Uniti. Un grande vantaggio delle grammatiche rispetto ai classici n-grammi è che le grammatiche sono molto efficaci per specificare tali formati.

Il formato VIN è ben standardizzato e ha un numero fisso di caratteri. I classici n-grammi hanno uno scarso rendimento per questi tipi di attività. A differenza delle grammatiche, non possono garantire che venga fornito il numero richiesto di caratteri.

Le seguenti grammatiche riconoscono i codici VIN Honda. Sono più complesse rispetto agli esempi precedenti ma dimostrano perfettamente il potere delle grammatiche.

<table style="width:50%">
  <caption>Tabella 4. Grammatiche di VIN</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.abnf" download="vins.abnf">vins.abnf <img src="../../icons/launch-glyph.svg" alt="Icona link esterno" title="Icona link esterno"></a>.
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.xml" download="vins.xml">vins.xml <img src="../../icons/launch-glyph.svg" alt="Icona link esterno" title="Icona link esterno"></a>.
    </td>
  </tr>
</table>

Per ulteriori informazioni sul formato VIN, vedi [Vehicle identification number ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://en.wikipedia.org/wiki/Vehicle_identification_number){: new_window} in Wikipedia.

## Grammatiche con elementi facoltativi
{: #optionalElements}

Rendendo facoltativi alcuni elementi di una risposta, puoi rendere le grammatiche più flessibili anticipando il modo in cui gli utenti potrebbero rispondere. La seguente grammatica aggiunge parentesi quadre attorno all'elemento `$optionalphrase` per renderlo facoltativo. L'utente può pronunciare una di alcune frasi aggiuntive prima di indicare il numero di previdenza sociale. Ad esempio, l'utente può dire "my social is xxx xx xxxx" o solo "xxx xx xxxx".

<table style="width:50%">
  <caption>Tabella 5. Grammatica di elementi facoltativi</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/optional.abnf" download="optional.abnf">optional.abnf <img src="../../icons/launch-glyph.svg" alt="Icona link esterno" title="Icona link esterno"></a>
    </td>
    <td style="text-align:center">
      Non disponibile
    </td>
  </tr>
</table>

Per ulteriori informazioni sulle espansioni facoltative nelle grammatiche, vedi [Section 2.5 Repeats ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.w3.org/TR/speech-grammar/#S2.5){: new_window} della specifica SRGS (Speech Recognition Grammar Specification).

## Grammatiche di disambiguazione
{: #disambiguation}

Altre possibili grammatiche di esempio includono situazioni in cui l'attendibilità della risposta riconosciuta non è molto alta ma l'applicazione deve essere certa della risposta dell'utente. In questo caso, l'applicazione può creare dinamicamente una grammatica che includa le risposte più probabili e chiedere all'utente di ripetere la risposta. Ad esempio, l'applicazione può creare una grammatica che include le due opzioni più probabili e chiedere all'utente di selezionare una delle due: `Did you mean 'option 1' or 'option 9'?`

Le grammatiche di disambiguazione vengono generate in modo programmatico. Non vengono forniti esempi.
