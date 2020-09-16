---

copyright:
  years: 2015, 2020
lastupdated: "2020-09-16"

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

# Example grammars
{: #grammarExamples}

The following examples describe more complex uses for grammars with speech recognition. Most examples are provided in both ABNF (`application/srgs`) and XML (`application/srgs+xml`) formats. Use these examples to help you get started with grammars.
{: shortdesc}

In a production rule for a grammar, including a `\` (backslash) is a syntax error in ABNF and is interpreted as a literal value in XML.
{: tip}

## Confirmation grammars
{: #confirmation}

Confirmation grammars are useful for applications that expect a one-word answer in response to a question. The following grammars define a list of possible yes and no responses.

-   **ABNF** - [confirm.abnf](https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.abnf){: external}
-   **XML** - [confirm.xml](https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.xml){: external}

## List grammars
{: #list}

List grammars are useful for applications that expect the user to select an option from a predefined set of strings. The set is sometimes called a flat list. It typically consists of a list of terminal elements and does not include a complicated rule structure.

-   The following grammars define a list of phrases for digits:
    -   **ABNF** - [list-numbers.abnf](https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.abnf){: external}
    -   **XML** - [list-numbers.xml](https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.xml){: external}

-   The following grammar defines a list of valid names from which the user can choose, perhaps to select an individual from a telephone directory:
    -   **ABNF** - [list-names.abnf](https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-names.abnf){: external}
    -   **XML** - Not available

## Vehicle identification number grammars
{: #vin}

Vehicle identification numbers (VINs) use a fixed alphanumeric format, as do credit card numbers, telephone numbers, and US social security numbers. A great advantage of grammars over classical n-grams is that grammars are very effective for specifying such formats.

The VIN format is well standardized and has a fixed number of characters. Classic n-grams perform poorly for these types of tasks. Unlike grammars, they cannot ensure that the required number of characters is provided.

The following grammars recognize Honda VIN codes. They are more complex than the previous examples but demonstrate the power of grammars nicely.

-   **ABNF** - [vins.abnf](https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.abnf){: external}
-   **XML** - [vins.xml](https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.xml){: external}

For more information about the VIN format, see [Vehicle identification number](https://wikipedia.org/wiki/Vehicle_identification_number){: external}.

## Grammars with optional elements
{: #optionalElements}

By making certain elements of a response optional, you can make grammars more flexible by anticipating how users might respond. The following grammar adds square brackets around the element `$optionalphrase` to make it optional. The user can speak one of a few additional phrases before stating the social security number. For instance, the user can say "my social is xxx xx xxxx" or just "xxx xx xxxx".

-   **ABNF** - [optional.abnf](https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/optional.abnf){: external}
-   **XML** - Not available

For more information about optional expansions in grammars, see [Section 2.5 Repeats](https://www.w3.org/TR/speech-grammar/#S2.5){: external} of the Speech Recognition Grammar Specification.

## Disambiguation grammars
{: #disambiguation}

Other possible example grammars include situations in which the confidence of the recognized response is not very high but the application needs to be certain of the user's response. In this case, the application can dynamically create a grammar that includes the most likely responses and ask the user to repeat the answer. For example, the application can create a grammar that includes the two most likely options and ask the user to select one of the two: `Did you mean 'option 1' or 'option 9'?`

Disambiguation grammars are programmatically generated. No examples are provided.
