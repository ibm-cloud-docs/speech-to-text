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

# Korpora verwalten
{: #manageCorpora}

Die Anpassungsschnittstelle enthält die Methode `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` zum Hinzufügen eines Korpus zu einem angepassten Sprachmodell. Weitere Informationen finden Sie im Abschnitt [Korpus zu angepasstem Sprachmodell hinzufügen](/docs/services/speech-to-text/language-create.html#addCorpus). Die Schnittstelle enthält außerdem die folgenden Methoden zum Auflisten und Löschen von Korpora für ein angepasstes Sprachmodell.
{: shortdesc}

## Korpora für angepasstes Sprachmodell auflisten
{: #listCorpora}

Die Anpassungsschnittstelle stellt die beiden folgenden Methoden zum Auflisten von Informationen zu den Korpora für ein angepasstes Sprachmodell zur Verfügung:

-   Die Methode `GET /v1/customizations/{customization_id}/corpora` listet Informationen zu allen Korpora für ein angepasstes Modell auf.
-   Die Methode `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` listet Informationen zu einem angegebenen Korpus für ein angepasstes Modell auf.

Beide Methoden geben den Namen (`name`) des Korpus, die Summe der aus dem Korpus gelesenen Wörter (`total_words`) und die Anzahl der aus dem Korpus extrahierten vokabularexternen Wörter (`out-of-vocabulary_words`) zurück. Darüber hinaus listen die Methoden den Status (`status`) des Korpus auf. Der Status ist wichtig für das Überprüfen der vom Service durchgeführten Analyse eines Korpus als Reaktion auf eine Anforderung zum Hinzufügen des Korpus zu einem angepassten Modell:

-   `analyzed` gibt an, dass das Korpus vom Service erfolgreich analysiert wurde. Das angepasste Modell kann mit Daten aus dem Korpus trainiert werden.
-   `being_processed` gibt an, dass der Service die Analyse des Korpus noch nicht abgeschlossen hat. Der Service kann Anforderungen zum Hinzufügen neuer Korpora oder Wörter bzw. zum Trainieren des angepassten Modells erst annehmen, nachdem die Analyse abgeschlossen ist.
-   `undetermined` gibt an, dass der Service beim Verarbeiten des Korpus einen Fehler festgestellt hat. Die für das Korpus zurückgegebenen Informationen enthalten eine Fehlernachricht mit Hinweisen zur Fehlerbehebung.

    Beispielsweise kann das Korpus ungültig sein oder Sie haben möglicherweise versucht, ein Korpus mit dem Namen eines bereits vorhandenen Korpus hinzuzufügen. Sie können versuchen, das Korpus erneut hinzuzufügen und dabei in der Anforderung den Parameter `allow_overwrite` angeben. Sie können auch das Korpus löschen und anschließend versuchen, es erneut hinzufügen.

### Beispiele für Anforderungen und Antworten
{: #listExample-corpora}

Im folgenden Beispiel werden alle Korpora für das angepasste Modell mit der angegebenen Anpassungs-ID aufgelistet:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora"
```
{: pre}

Drei Korpora wurden zu dem angepassten Modell hinzugefügt. Der Service hat `corpus1` erfolgreich analysiert. Die Analyse von `corpus2` ist noch nicht abgeschlossen und die Analyse von `corpus3` ist fehlgeschlagen.

```javascript
{
  "corpora": [
    {
      "name": "corpus1",
      "total_words": 5037,
      "out_of_vocabulary_words": 401,
      "status": "analyzed"
    },
    {
      "name": "corpus2",
      "total_words": 0,
      "out_of_vocabulary_words": 0,
      "status": "being_processed"
    },
    {
      "name": "corpus3",
      "total_words": 0,
      "out_of_vocabulary_words": 0,
      "status": "undetermined",
      "error": "Analysis of corpus 'corpus3.txt' failed. Please try adding the corpus again by setting the 'allow_overwrite' flag to 'true'."
    }
  ]
}
```
{: codeblock}

Das folgende Beispiel gibt Informationen zu dem Korpus mit dem Namen `corpus1` für das angepasste Modell mit der angegebenen Anpassungs-ID zurück:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

## Korpus aus angepasstem Sprachmodell löschen
{: #deleteCorpus}

Mit der Methode `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` können Sie ein vorhandenes Korpus aus einem angepassten Sprachmodell entfernen. Beim Löschen des Korpus entfernt der Service alle OOV-Wörter (OOV = Out Of Vocabulary, vokabularextern), die dem Korpus zugeordnet sind, aus der Wörterressource des angepassten Modells, es sei denn, eine der folgenden Bedingungen trifft zu:

-   Das Wort wurde auch von einem anderen Korpus oder einer anderen Grammatik hinzugefügt.
-   Das Wort wurde mit der Methode `POST /v1/customizations/{customization_id}/words` oder `PUT /v1/customizations/{customization_id}/words/{word_name}` geändert.

Das Entfernen eines Korpus wirkt sich erst auf das angepasste Modell aus, wenn Sie das Modell mit den aktualisierten Korpusdaten trainieren (mithilfe der Methode `POST /v1/customizations/{customization_id}/train`). Wenn Sie das Modell erfolgreich mit dem Korpus trainiert haben, werden Wörter aus dem Korpus in das Vokabular des Modells übernommen und bei der Spracherkennung angewendet, bis Sie das Modell erneut trainieren.

### Beispielanforderung
{: #deleteExample-corpus}

Im folgenden Beispiel wird das Korpus mit dem Namen `corpus3` aus dem angepassten Modell mit der angegeben Anpassungs-ID gelöscht.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus3"
```
{: pre}
