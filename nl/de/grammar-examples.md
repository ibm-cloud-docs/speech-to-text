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

# Beispiele für Grammatiken
{: #grammarExamples}

Die hier aufgeführten Beispiele beschreiben komplexere Anwendungsfälle für Grammatiken bei der Spracherkennung. Die meisten Beispiele stehen sowohl im ABNF-Format (`application/srgs`) als auch im XML-Format (`application/srgs+xml`) zur Verfügung. Mithilfe dieser Beispiele können Sie in die Verwendung von Grammatiken einsteigen.
{: shortdesc}

Die Aufnahme eines umgekehrten Schrägstrichs (\) in eine Produktionsregel für eine Grammatik ist in ABNF ein Syntaxfehler und wird in XML als Literalwert interpretiert.
{: tip}

## Bestätigungsgrammatiken
{: #confirmation}

Bestätigungsgrammatiken sind bei Anwendungen zweckmäßig, die eine aus einem Wort bestehende Antwort auf eine Frage erwarten. Die folgenden Grammatiken definieren eine Liste der möglichen Antworten 'yes' und 'no'.

<table style="width:50%">
  <caption>Tabelle 1. Bestätigungsgrammatiken</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.abnf" download="confirm.abnf">confirm.abnf <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a>
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.xml" download="confirm.xml">confirm.xml <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a>
    </td>
  </tr>
</table>

## Listengrammatiken
{: #list}

Listengrammatiken sind für Anwendungen sinnvoll, bei denen der Benutzer eine Option aus einer vordefinierten Gruppe von Zeichenfolgen auswählen muss. Die Gruppe wird manchmal als 'unstrukturierte Liste' bezeichnet. Sie besteht in der Regel aus einer Liste von Terminalelementen und beinhaltet keine komplizierte Regelstruktur.

-   Die folgenden Grammatiken definieren eine Liste von Ausdrücken für Ziffern.

    <table style="width:50%">
      <caption>Table 2. Listengrammatiken für Zahlen</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.abnf" download="list-numbers.abnf">list-numbers.abnf <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a>
        </td>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.xml" download="list-numbers.xml">list-numbers.xml <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a>
        </td>
      </tr>
    </table>

-   Die folgende Grammatik definiert eine Liste von gültigen Namen, unter denen der Benutzer eine Auswahl treffen kann (um beispielsweise eine Einzelperson in einem Telefonverzeichnis auszuwählen).

    <table style="width:50%">
      <caption>Tabelle 3. Listengrammatik für Namen</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-names.abnf" download="list-names.abnf">list-names.abnf <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a>
        </td>
        <td style="text-align:center">
          Nicht verfügbar
        </td>
      </tr>
    </table>

## Grammatiken für Fahrzeug-Identifizierungsnummern
{: #vin}

Fahrzeug-Identifizierungsnummern (Vehicle Identification Numbers, VINs, kurz auch 'Fahrzeug-Ident.-Nr.' genannt) verwenden wie Kreditkarten, Telefonnummern und Sozialversicherungsnummern ein festes alphanumerisches Format. Grammatiken haben gegenüber klassischen N-Grammen den Vorteil, dass sie für die Angabe solcher Formate sehr effektiv sind.

Das VIN-Format ist sehr standardisiert und enthält eine feste Anzahl von Zeichen. Klassische N-Gramme sind für derlei Aufgaben wenig geeignet. Anders als Grammatiken können sie nicht gewährleisten, dass die erforderliche Anzahl von Zeichen angegeben ist.

Die folgenden Grammatiken erkennen VIN-Codes des Herstellers 'Honda'. Sie sind zwar komplexer als die vorherigen Beispiele, veranschaulichen jedoch auf sehr schöne Weise die Leistungsstärke von Grammatiken.

<table style="width:50%">
  <caption>Tabelle 4. Grammatiken für VINs</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.abnf" download="vins.abnf">vins.abnf <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a>
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.xml" download="vins.xml">vins.xml <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a>
    </td>
  </tr>
</table>

Weitere Informationen zum VIN-Format finden Sie im Wikipedia-Artikel [Vehicle identification number](https://wikipedia.org/wiki/Vehicle_identification_number){: external}.

## Grammatiken mit optionalen Elementen
{: #optionalElements}

Indem Sie bestimmte Elemente einer Antwort als optional definieren, können Sie Grammatiken flexibler antizipieren lassen, wie Benutzer möglicherweise antworten. Bei der folgenden Grammatik wird das Element `$optionalphrase` in eckige Klammern gesetzt und so als optional definiert. Der Benutzer kann vor Angabe der Sozialversicherungsnummer einige weitere Ausdrücke sprechen. Beispielsweise kann er 'Meine Sozialversicherungsnummer ist xxx xx xxxx' oder auch nur 'xxx xx xxxx' sagen.

<table style="width:50%">
  <caption>Tabelle 5. Grammatik mit optionalen Elementen</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/optional.abnf" download="optional.abnf">optional.abnf <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a>
    </td>
    <td style="text-align:center">
      Nicht verfügbar
    </td>
  </tr>
</table>

Weitere Informationen zu optionalen Erweiterungen in Grammatiken finden Sie unter [Section 2.5 Repeats](https://www.w3.org/TR/speech-grammar/#S2.5){: external} in der Speech Recognition Grammar Specification.

## Grammatiken für Begriffsklärung
{: #disambiguation}

Andere mögliche Beispiele für Grammatiken sind für Situationen gedacht, in denen die Konfidenz der erkannten Antwort nicht besonders hoch ist, die Anwendung jedoch Sicherheit über die Antwort des Benutzers benötigt. In diesem Fall kann die Anwendung dynamisch eine Grammatik erstellen, die die wahrscheinlichsten Antworten enthält, und den Benutzer bitten, die Antwort zu wiederholen. Beispielsweise kann die Anwendung eine Grammatik mit den beiden wahrscheinlichsten Optionen erstellen und den Benutzer auffordern, eine der beiden Optionen auszuwählen: `Meinten Sie 'Option 1' oder 'Option 9'?`

Grammatiken für die Begriffsklärung werden programmgesteuert generiert. Hierfür können keine Beispiele zur Verfügung gestellt werden.
