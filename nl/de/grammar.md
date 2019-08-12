---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-24"

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

# Grammatiken bei angepassten Sprachmodellen verwenden
{: #grammars}

Der {{site.data.keyword.speechtotextfull}}-Service unterstützt die Verwendung von Grammatiken bei angepassten Sprachmodellen. Sie können Grammatiken zu einem angepassten Sprachmodell hinzufügen und sie für die Spracherkennung nutzen. Grammatiken schränken die Menge der Ausdrücke ein, die der Service aus Audiodaten erkennen kann.
{: shortdesc}

Eine Grammatik definiert mithilfe einer formalen Sprachspezifikation eine Reihe von Produktionsregeln für die Transkription von Zeichenfolgen. Die Regeln geben an, wie gültige Zeichenfolgen aus dem Alphabet der Sprache gebildet werden sollen. Wenn Sie auf die Spracherkennung eine Grammatik anwenden, kann der Service ausschließlich eine oder mehrere der Ausdrücke zurückgeben, die durch die Grammatik definiert sind.

Wenn beispielsweise bestimmte Wörter oder Ausdrücke wie *Ja* oder *Nein*, einzelne Buchstaben bzw. Ziffern oder eine Liste von Namen erkannt werden müssen, kann die Verwendung von Grammatiken effizienter als die Untersuchung von alternativen Wörtern und Transkriptionen sein. Durch die Begrenzung des Suchbereichs für gültige Zeichenfolgen kann der Service darüber hinaus schneller und präziser Ergebnisse liefern.

Wenn Sie für die Spracherkennung ein angepasstes Sprachmodell und eine Grammatik verwenden, kann der Service einen gültigen Ausdruck aus der Grammatik oder aber ein leeres Ergebnis zurückgeben. Falls das Ergebnis nicht leer ist, bezieht der Service wie bei allen Erkennungsanforderungen eine Konfidenzbewertung ein. Bei Grammatiken gibt die Bewertung die Wahrscheinlichkeit an, dass die Antwort mit der Grammatik übereinstimmt. Falsch-positive Antworten sind insbesondere bei einfachen Grammatiken immer möglich, weshalb Sie beim Auswerten der Serviceantwort die Konfidenz der Ergebnisse stets berücksichtigen müssen.

Die Funktion für die Grammatiken liegt als Betaversion vor. Der Service unterstützt Grammatiken für alle Sprachen, bei denen er die Anpassung des Sprachmodells unterstützt. Weitere Informationen finden Sie unter [Sprachunterstützung bei der Anpassung](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport).
{: note}

## Unterstützte Grammatikformate
{: #grammarFormats}

Der {{site.data.keyword.speechtotextshort}} -Service unterstützt Grammatiken, die in den folgenden Standardformaten definiert sind:

-   Das Format *Augmented Backus-Naur Form (ABNF)* verwendet eine Darstellung in einfachem Text, die der konventionellen BNF-Grammatik ähnelt. Der Medientyp für dieses Format ist `application/srgs`.
-   Das Format *XML Form* verwendet XML-Elemente zur Darstellung der Grammatik. Der Medientyp für dieses Format lautet `application/srgs+xml`.

Beide Grammatikformate besitzen hinsichtlich des Ausdrucksvermögens dieselbe Leistungsstärke wie eine kontextfreie Grammatik (Context-Free Grammar, CFG). Der Service kann jedoch nur reguläre Grammatiken des Typs 3 in der Chomsky-Hierarchie decodieren. Solche Grammatiken stellen endliche Automaten (engl.: finite state automata) dar.

Allgemeine Informationen zu Grammatiken finden Sie auf den folgenden Wikipedia-Seiten:

-   [Speech Recognition Grammar Specification](https://wikipedia.org/wiki/Speech_Recognition_Grammar_Specification){: external}
-   [Angereicherte Backus-Naur-Form](https://wikipedia.org/wiki/Augmented_Backus%E2%80%93Naur_form){: external}
-   [Chomsky-Hierarchie](https://wikipedia.org/wiki/Chomsky_hierarchy){: external}

## Speech Recognition Grammar Specification
{: #grammarSpecification}

Der {{site.data.keyword.speechtotextshort}}-Service unterstützt Grammatiken, die der W3C-Definition [Speech Recognition Grammar Specification Version 1.0](https://www.w3.org/TR/speech-grammar/){: external} entsprechen. Die Spezifikation enthält detaillierte Informationen zu den unterstützten Formaten und zum Definieren einer Grammatik. Angaben über die unterstützten Medientypen enthält der Abschnitt [Appendix G. Media Types and File Suffix](https://www.w3.org/TR/speech-grammar/#AppG){: external} in der Spezifikation.

Der Service unterstützt gegenwärtig *nicht* alle Merkmale von Speech Recognition Grammar Specification. Insbesondere werden die in den folgenden Abschnitten der Spezifikation beschriebenen Merkmale nicht unterstützt:

-   [Section 1.4 Semantic Interpretation](https://www.w3.org/TR/speech-grammar/#S1.4){: external} (Semantische Interpretation). {{site.data.keyword.IBM_notm}} strebt an, diese Funktion in einem künftigen Release zu unterstützen.
-   [Section 1.5 Embedded Grammars](https://www.w3.org/TR/speech-grammar/#S1.5){: external} (Eingebettete Grammatiken). {{site.data.keyword.IBM_notm}} strebt an, diese Funktion in einem künftigen Release zu unterstützen.
-   [Section 2.2.2 External Reference by URI](https://www.w3.org/TR/speech-grammar/#S2.2.2){: external} (Externe Referenz durch URI). Der Service unterstützt lediglich lokale Referenzen, wie unter [Section 2.2.1 Local References](https://www.w3.org/TR/speech-grammar/#S2.2.1){: external} beschrieben. Mit anderen Worten bedeutet dies, dass eine Grammatik eigenständig sein muss.
-   [Section 2.2.3 Special Rules](https://www.w3.org/TR/speech-grammar/#S2.2.3){: external} (Besondere Regeln).
-   [Section 2.2.4 Referencing N-gram Documents (Informative)](https://www.w3.org/TR/speech-grammar/#S2.2.4){: external} (Referenzierung von Dokumenten zu N-Grammen (Informativ)).
-   [Section 2.7 Language](https://www.w3.org/TR/speech-grammar/#S2.7){: external} (Sprache). Ein Sprachenwechsel wird durch den Service nicht unterstützt. Der Service unterstützt nur eine einzige globale Sprache pro Grammatik.

Die Wörter in der Grammatik müssen in UTF-8 codiert sein (ASCII ist eine Untergruppe von UTF-8). Die Verwendung einer anderen Codierung kann bei der Kompilierung der Grammatik Probleme oder bei der Decodierung nicht erwartete Ergebnisse verursachen. Eine im Header der Grammatik angegebene Codierung wird vom Service ignoriert.
{: note}
