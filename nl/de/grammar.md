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

# Grammatiken bei angepassten Sprachmodellen verwenden
{: #grammars}

Der {{site.data.keyword.speechtotextfull}}-Service unterstützt die Verwendung von Grammatiken bei angepassten Sprachmodellen. Sie können Grammatiken zu einem angepassten Sprachmodell hinzufügen und sie für die Spracherkennung nutzen. Grammatiken schränken die Menge der Ausdrücke ein, die der Service aus Audiodaten erkennen kann.
{: shortdesc}

Eine Grammatik definiert mithilfe einer formalen Sprachspezifikation eine Reihe von Produktionsregeln für die Transkription von Zeichenfolgen. Die Regeln geben an, wie gültige Zeichenfolgen aus dem Alphabet der Sprache gebildet werden sollen. Wenn Sie auf die Spracherkennung eine Grammatik anwenden, kann der Service ausschließlich eine oder mehrere der Ausdrücke zurückgeben, die durch die Grammatik definiert sind.

Wenn beispielsweise bestimmte Wörter oder Ausdrücke wie *Ja* oder *Nein*, einzelne Buchstaben bzw. Ziffern oder eine Liste von Namen erkannt werden müssen, kann die Verwendung von Grammatiken effizienter als die Untersuchung von alternativen Wörtern und Transkriptionen sein. Durch die Begrenzung des Suchbereichs für gültige Zeichenfolgen kann der Service darüber hinaus schneller und präziser Ergebnisse liefern.

Wenn Sie für die Spracherkennung ein angepasstes Sprachmodell und eine Grammatik verwenden, kann der Service einen gültigen Ausdruck aus der Grammatik oder aber ein leeres Ergebnis zurückgeben. Falls das Ergebnis nicht leer ist, bezieht der Service wie bei allen Erkennungsanforderungen eine Konfidenzbewertung ein. Bei Grammatiken gibt die Bewertung die Wahrscheinlichkeit an, dass die Antwort mit der Grammatik übereinstimmt. Falsch-positive Antworten sind insbesondere bei einfachen Grammatiken immer möglich, weshalb Sie beim Auswerten der Serviceantwort die Konfidenz der Ergebnisse stets berücksichtigen müssen.

Die Funktion für die Grammatiken liegt als Betaversion vor. Der Service unterstützt Grammatiken für alle Sprachen, bei denen er die Anpassung des Sprachmodells unterstützt.
{: note}

## Unterstützte Grammatikformate
{: #grammarFormats}

Der {{site.data.keyword.speechtotextshort}} -Service unterstützt Grammatiken, die in den folgenden Standardformaten definiert sind:

-   Das Format *Augmented Backus-Naur Form (ABNF)* verwendet eine Darstellung in einfachem Text, die der konventionellen BNF-Grammatik ähnelt. Der Medientyp für dieses Format ist `application/srgs`.
-   Das Format *XML Form* verwendet XML-Elemente zur Darstellung der Grammatik. Der Medientyp für dieses Format lautet `application/srgs+xml`.

Beide Grammatikformate besitzen hinsichtlich des Ausdrucksvermögens dieselbe Leistungsstärke wie eine kontextfreie Grammatik (Context-Free Grammar, CFG). Der Service kann jedoch nur reguläre Grammatiken des Typs 3 in der Chomsky-Hierarchie decodieren. Solche Grammatiken stellen endliche Automaten (engl.: finite state automata) dar.

Allgemeine Informationen zu Grammatiken finden Sie auf den folgenden Wikipedia-Seiten:

-   [Speech Recognition Grammar Specification ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Speech_Recognition_Grammar_Specification){: new_window}
-   [Augmented Backus-Naur form ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Augmented_Backus%E2%80%93Naur_form){: new_window}
-   [Chomsky hierarchy ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Chomsky_hierarchy){: new_window}

## Speech Recognition Grammar Specification
{: #grammarSpecification}

Der {{site.data.keyword.speechtotextshort}}-Service unterstützt Grammatiken, die der W3C-Definition [Speech Recognition Grammar Specification Version 1.0 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.w3.org/TR/speech-grammar/){: new_window} entsprechen. Die Spezifikation enthält detaillierte Informationen zu den unterstützten Formaten und zum Definieren einer Grammatik. Angaben über die unterstützten Medientypen enthält der Abschnitt [Appendix G. Media Types and File Suffix ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.w3.org/TR/speech-grammar/#AppG){: new_window} in der Spezifikation.

Der Service unterstützt gegenwärtig *nicht* alle Merkmale von Speech Recognition Grammar Specification. Insbesondere werden die in den folgenden Abschnitten der Spezifikation beschriebenen Merkmale nicht unterstützt:

-   [Section 1.4 Semantic Interpretation. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.w3.org/TR/speech-grammar/#S1.4){: new_window} {{site.data.keyword.IBM_notm}} strebt an, diese Funktion in einem küntigen Release zu unterstützen.
-   [Section 1.5 Embedded Grammars. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.w3.org/TR/speech-grammar/#S1.5){: new_window} {{site.data.keyword.IBM_notm}} strebt an, diese Funktion in einem küntigen Release zu unterstützen.
-   [Section 2.2.2 External Reference by URI. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.w3.org/TR/speech-grammar/#S2.2.2){: new_window} Der Service unterstützt lediglich lokale Referenzen wie unter [Section 2.2.1 Local References ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.w3.org/TR/speech-grammar/#S2.2.1){: new_window} beschrieben. Mit anderen Worten bedeutet dies, dass eine Grammatik eigenständig sein muss.
-   [Section 2.2.3 Special Rules. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.w3.org/TR/speech-grammar/#S2.2.3){: new_window}
-   [Section 2.2.4 Referencing N-gram Documents (Informative). ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.w3.org/TR/speech-grammar/#S2.2.4){: new_window}
-   [Section 2.7 Language. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.w3.org/TR/speech-grammar/#S2.7){: new_window} Ein Sprachenwechsel wird durch den Service nicht unterstützt. Der Service unterstützt nur eine einzige globale Sprache pro Grammatik.

Die Wörter in der Grammatik müssen in UTF-8 codiert sein (ASCII ist eine Untergruppe von UTF-8). Die Verwendung einer anderen Codierung kann bei der Kompilierung der Grammatik Probleme oder bei der Decodierung nicht erwartete Ergebnisse verursachen. Eine im Header der Grammatik angegebene Codierung wird vom Service ignoriert.
{: note}
