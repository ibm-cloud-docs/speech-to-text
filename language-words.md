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

# Angepasste Wörter verwalten
{: #manageWords}

Die Anpassungsschnittstelle enthält die Methoden `POST /v1/customizations/{customization_id}/words` und `PUT /v1/customizations/{customization_id}/words/{word_name}` zum Hinzufügen bzw. Ändern von Wörtern für ein angepasstes Modell. Weitere Informationen finden Sie im Abschnitt [Wörter zum angepassten Sprachmodell hinzufügen](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addWords). Die Schnittstelle enthält außerdem die folgenden Methoden zum Auflisten bzw. Löschen von Wörtern für ein angepasstes Sprachmodell.
{: shortdesc}

Angepasste Wörter werden häufig aus Kopora hinzugefügt. Stellen Sie sicher, dass Sie die Zeichencodierung kennen, die in den Textdateien für Ihre Korpora verwendet wird. Der Service behält die in den Textdateien verwendete Codierung bei. Beim Arbeiten mit den einzelnen Wörtern im angepassten Sprachmodell müssen Sie diese Codierung verwenden. Wenn Sie in der Methode `GET`, `PUT` oder `DELETE /v1/customizations/{customization_id}/words/{word_name}` ein Wort angeben, muss das Element `word_name`, das Sie in der URL übergeben, URL-codiert sein, wenn das Wort Nicht-ASCII-Zeichen enthält. Weitere Informationen finden Sie im Abschnitt [Zeichencodierung](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#charEncoding).
{: important}

## Wörter aus angepasstem Sprachmodell auflisten
{: #listWords}

Die Anpassungsschnittstelle stellt die beiden folgenden Methoden zum Auflisten von Wörtern aus einem angepassten Sprachmodell zur Verfügung:

-   Die Methode `GET /v1/customizations/{customization_id}/words` listet Informationen zu den Wörtern aus der Wörterressource des angepassten Modells auf. Die Methode enthält zwei optionale Abfrageparameter.
    -   Der Parameter `word_type` gibt an, welche Wörter aufgelistet werden sollen:

        -   `all` (Standardwert) zeigt alle Wörter an.
        -   `user` zeigt nur angepasste Wörter an, die vom Benutzer hinzugefügt oder geändert wurden.
        -   `corpora` zeigt nur OOV-Wörter an, die aus Korpora extrahiert wurden.
        -   `grammars` zeigt nur OOV-Wörter an, die aus Grammatiken extrahiert wurden.
    -   Der Parameter `sort` gibt an, in welcher Reihenfolge die Wörter aufgelistet werden. Der Parameter akzeptiert zwei Argumente, die die Sortierung der Wörter angeben: `alphabetical` und `count`. Sie können einem Argument optional das Zeichen `+` oder `-` voranstellen, um anzugeben, ob die Ergebnisse in auf- oder absteigender Reihenfolge sortiert werden sollen. Standardmäßig werden die Wörter von der Methode in aufsteigender alphabetischer Reihenfolge angezeigt.
-   Die Methode `GET /v1/customizations/{customization_id}/words/{word_name}` listet Informationen zu einem einzelnen angegebenen Wort aus der Wörterressource des Modells auf.

Außer dem Feld `word`, in dem das Wort angegeben wird, geben beide Methoden die folgenden Informationen zu jedem Wort zurück:

-   Ein Feld `sounds_like` mit einem Array von Aussprachevarianten für das Wort. Das Array kann die gleich klingende Aussprachevariante enthalten, die vom Service automatisch generiert wird, wenn für das Wort kein Wert im Feld 'sounds_like' angegeben ist. Weitere Informationen finden Sie im Abschnitt [Das Feld 'sounds_like' verwenden](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#soundsLike).
-   Ein Feld `display_as`, in dem die Schreibweise des angepassten Worts angegeben wird, die vom Service in Transkriptionen angezeigt wird. Das Feld enthält eine leere Zeichenfolge, wenn für das Wort kein Wert für 'display_as' angegeben ist. In diesem Fall wird das Wort in der dazugehörigen Schreibweise angezeigt. Weitere Informationen finden Sie im Abschnitt [Das Feld 'display_as' verwenden](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#displayAs).
-   Ein Feld `source`, das angibt, wie das Wort zur Wörterressource des angepassten Modells hinzugefügt wurde. Das Feld enthält die Namen aller Korpora und Grammatiken, aus denen das Wort vom Service extrahiert wurde. Wenn das Wort von Ihnen direkt geändert oder hinzugefügt wurde, enthält das Feld die Zeichenfolge `user`.
-   Ein Feld `count`, das angibt, wie oft das Wort in allen Korpora und Grammatiken gefunden wurde. Wenn das Wort beispielsweise fünf Mal in einem Korpus vorkommt und sieben Mal in einem anderen Korpus, wird der Zählerwert `12` angegeben.

    Wenn Sie ein angepasstes Wort zu einem Modell hinzufügen, bevor es aus einem Korpus oder einer Grammatik hinzugefügt wird, beginnt der Zähler mit dem Wert `1`. Wird das Wort zuerst aus einem Korpus oder einer Grammatik hinzugefügt und später geändert, gibt der Zähler nur die Vorkommen des Wortes in Korpora und Grammatiken an.

    Bei angepassten Modellen, die vor der Einführung des Felds `count` erstellt wurden, behält das Feld immer den Wert `0` bei. Um den Zählerwert für solche Modelle zu aktualisieren, fügen Sie die Korpora und Grammatiken des Modells erneut hinzu und geben Sie dabei in der Anforderung den Parameter `allow_overwrite` an.
    {: note}

Wenn der Service mindestens ein Problem für die Definition eines angepassten Worts feststellt, enthält die Ausgabe ein Feld `error`. Dieses Feld enthält ein Array mit einer Auflistung der einzelnen Problemelemente aus der Definition und eine Nachricht mit einer Beschreibung des Problems.

Ein Fehler kann beispielsweise auftreten, wenn Sie ein angepasstes Wort mit einem ungültigen Feld `sounds_like` hinzufügen, das gegen die Regeln zum Hinzufügen von Aussprachevarianten verstößt. Ein angepasstes Modell, das ein fehlerhaftes Wort enthält, kann nicht trainiert werden. Sie müssen das Wort korrigieren oder löschen, bevor Sie das Modell trainieren können.

### Beispiele für Anforderungen und Antworten
{: #listExample-words}

Im folgenden Beispiel werden alle Wörter (unabhängig vom Typ) aus dem angepassten Modell mit der angegebenen Anpassungs-ID aufgelistet. Die Wörter werden in der Standardsortierreihenfolge (alphabetisch und aufsteigend) angezeigt.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
```
{: pre}

Die Wörterressource für das Modell enthält vier Wörter. Das erste Wort wurde vom Benutzer direkt hinzugefügt, aber das zugehörige Feld `sounds_like` enthält einen Fehler (das Feld darf keine Zahlen enthalten). Die übrigen Wörter wurden vom Benutzer und/oder aus Korpora hinzugefügt.

```javascript
{
  "words": [
    {
      "word": "75.00",
      "sounds_like": ["75 dollars"],
      "display_as": "75.00",
      "count": 1,
      "source": ["user"],
      "error": [{"75 dollars": "In 'sounds_like' sind keine Zahlen zulässig. Versuchen Sie es z. B. mit der Angabe 'seventy five dollars'."}]
    },
    {
      "word": "HHonors",
      "sounds_like": [
        "hilton honors",
        "H. honors"
      ],
      "display_as": "HHonors",
      "count": 1,
      "source": [
        "corpus1",
        "user"
      ]
    },
    {
      "word": "IEEE",
      "sounds_like": ["I. triple E."],
      "display_as": "IEEE",
      "count": 3,
      "source": [
        "corpus1",
        "corpus2",
        "user"
      ]
    },
    {
      "word": "tomato",
      "sounds_like": [
        "tomatoh",
        "tomayto"
      ],
      "display_as": "tomato",
      "count": 1,
      "source": ["user"]
    }
  ]
}
```
{: codeblock}

Das folgende Beispiel zeigt Informationen zu dem Wort `NCAA` aus der Wörterressource des angegebenen Modells an:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
```
{: pre}

Das Wort wurde ursprünglich vom Benutzer hinzugefügt. Später wurde das Wort vom Service zweimal aus `corpus3` extrahiert.

```javascript
{
  "word": "NCAA",
  "sounds_like": [
    "N. C. A. A.",
    "N. C. double A."
  ],
  "display_as": "NCAA",
  "count": 3,
  "source": [
    "corpus3",
    "user"
  ]
}
```
{: codeblock}

## Wort aus einem angepassten Sprachmodell löschen
{: #deleteWord}

Mit der Methode `DELETE /v1/customizations/{customization_id}/words/{word_name}` können Sie ein Wort aus einem angepassten Sprachmodell löschen. Verwenden Sie diese Methode zum Entfernen von Wörtern, die irrtümlich aus einem Korpus mit fehlerhaften Daten hinzugefügt wurden.

Sie können jedes Wort entfernen, das Sie mit einer beliebigen Methode zur Wörterressource des angepassten Modells hinzugefügt haben. Sie können jedoch keine Wörter aus dem Basisvokabular des Service löschen. Beim Löschen eines Wortes aus einem angepassten Modell wird nur die angepasste Aussprachevariante für das Wort gelöscht. Das Wort selbst bleibt im Basisvokabular erhalten.

Das Entfernen eines Wortes aus einem angepassten Modell wirkt sich erst auf das Modell aus, wenn Sie das Modell mit der Methode `POST /v1/customizations/{customization_id}/train` erneut trainieren. Wenn das Modell zuvor mit dem Wort trainiert wurde, wird das Wort weiterhin bei der Spracherkennung verwendet, auch nachdem Sie das Wort aus der Wörterressource des Modells gelöscht haben. Sie müssen das Modell erneut trainieren, damit das Löschen des Wortes berücksichtigt wird.

### Beispielanforderung
{: #deleteExample-word}

Im folgenden Beispiel wird das Wort `IEEE` aus dem angepassten Modell mit der angegebenen Anpassungs-ID gelöscht:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IEEE"
```
{: pre}
