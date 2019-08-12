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

# Grammatiken verwalten
{: #manageGrammars}

Mit der Methode `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` der Anpassungsschnittstelle können Sie eine Grammatik zu einem angepassten Sprachmodell hinzufügen. Weitere Informationen enthält der Abschnitt [Grammatik zum angepassten Sprachmodell hinzufügen](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar). Die Schnittstelle enthält außerdem die hier aufgeführten Methoden für das Auflisten und Löschen von Grammatiken für ein angepasstes Sprachmodell.
{: shortdesc}

## Grammatiken für ein angepasstes Sprachmodell auflisten
{: #listGrammars}

Die Anpassungsschnittstelle bietet zwei Methoden für das Auflisten von Informationen zu den Grammatiken eines angepassten Sprachmodells:

-   Die Methode `GET /v1/customizations/{customization_id}/grammars` listet Informationen zu allen Grammatiken für ein angepasstes Modell auf.
-   Die Methode `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` listet Informationen zu einer angegebenen Grammatik für ein angepasstes Modell auf.

Beide Methoden geben dieselben Informationen zu einer Grammatik zurück. Die Angaben enthalten im Feld `name` den Namen der Grammatik und im Feld `out-of_vocabulary_words` die Anzahl der von der Grammatik erkannten vokabularexternen Wörter. Die Antwort enthält das Feld `status` für die Grammatik, das von Bedeutung ist, wenn Sie beim Hinzufügen einer Grammatik zu einem angepassten Modell die Analyse der Grammatik durch den Service überprüfen wollen.

-   `being_processed`: Der Service verarbeitet gegenwärtig noch die Grammatik als Reaktion auf eine Anforderung `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`.
-   `analyzed`: Der Service hat die Grammatik erfolgreich verarbeitet und zum angepassten Modell hinzugefügt.
-   `undetermined`: Der Service hat bei der Verarbeitung der Grammatik einen Fehler festgestellt. Die zurückgegebenen Informationen zur Grammatik beinhalten eine Fehlernachricht, die Anleitungen für die Behebung des Fehlers einschließt.

    Möglicherweise ist die Grammatik ungültig oder Sie haben versucht, eine Grammatik mit demselben Namen wie eine vorhandene Grammatik hinzuzufügen. Sie können versuchen, die Grammatik erneut hinzuzufügen, und den Parameter `allow_overwrite` in die Anforderung einzubeziehen. Sie können auch die Grammatik löschen und versuchen, sie erneut hinzuzufügen.

### Beispielanforderungen und -antworten
{: #listExample-grammars}

Im folgenden Beispiel werden Informationen zu allen Grammatiken aufgelistet, die zum angepassten Modell mit der angegebenen Anpassungs-ID hinzugefügt wurden.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars"
```
{: pre}

Die drei Grammatiken `confirm-xml`, `confirm-abnf` und `list-abnf` wurden erfolgreich zum angepassten Modell hinzugefügt.

```javascript
{
  "grammars": [
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-xml",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-abnf",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 8,
      "name": "list-abnf",
      "status": "analyzed"
    }
  ]
}
```
{: codeblock}

Mit dem folgenden Beispiel werden Informationen zur angegebenen Grammatik namens `list-abnf` angezeigt.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/list-abnf"
```
{: pre}

```javascript
{
  "out_of_vocabulary_words": 8,
  "name": "list-abnf",
  "status": "analyzed"
}
```
{: codeblock}

## Grammatik aus einem angepassten Sprachmodell löschen
{: #deleteGrammar}

Mit der Methode `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` können Sie eine vorhandene Grammatik aus einem angepassten Sprachmodell entfernen. Beim Löschen der Grammatik entfernt der Service alle vokabularexternen Wörter, die zur Grammatik gehören, aus der Wörterressource des angepassten Modells, allerdings mit Ausnahme der folgenden Fälle:

-   Das Wort wurde auch durch eine andere Grammatik oder durch einen Korpus hinzugefügt.
-   Das Wort wurde mit der Methode `POST /v1/customizations/{customization_id}/words` oder `PUT /v1/customizations/{customization_id}/words/{word_name}` geändert.

Das Entfernen einer Grammatik wirkt sich erst dann auf das angepasste Modell aus, wenn Sie es unter Verwendung der Methode `POST /v1/customizations/{customization_id}/train` mit seinen aktualisierten Daten trainieren. Falls Sie das Modell mit der Grammatik erfolgreich trainiert hatten, bleibt die Grammatik für die Spracherkennung verfügbar, bis Sie das Modell erneut trainieren.

### Beispielanforderung
{: #deleteExample-grammar}

Im folgenden Beispiel wird die Grammatik namens `list-abnf` aus dem angepassten Modell mit der angegebenen Anpassungs-ID gelöscht.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/list-abnf"
```
{: pre}
