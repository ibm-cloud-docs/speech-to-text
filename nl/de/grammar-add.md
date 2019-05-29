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

# Grammatik zu einem angepassten Sprachmodell hinzufügen
{: #grammarAdd}

Bevor Sie eine Grammatik für die Spracherkennung verwenden können, müssen Sie zunächst die Grammatik mithilfe der Anpassungsschnittstelle zu einem angepassten Sprachmodell hinzufügen. Eine Grammatik wird zu einem angepassten Sprachmodell mit denselben Schritten wie Korpora oder angepasste Wörter hinzugefügt.
{: shortdesc}

1.  [Erstellen Sie ein angepasstes Sprachmodell](#createModel-grammar). Sie können ein neues angepasstes Modell erstellen oder ein vorhandenes Modell verwenden.
1.  [Fügen Sie eine Grammatik zum angepassten Sprachmodell hinzu](#addGrammar). Der Service validiert die Grammatik, um ihre Richtigkeit sicherzustellen.
1.  [Validieren Sie die Wörter aus der Grammatik](#validateGrammar). Sie prüfen die Fehlerfreiheit von gleich klingenden Aussprachevarianten für alle vokabularexternen Wörter, die durch die Grammatik erkannt werden.
1.  [Trainieren Sie das angepasste Sprachmodell](#trainGrammar). Der Service bereitet das angepasste Modell und die Grammatik für die Verwendung bei der Spracherkennung vor.
1.  Jetzt können Sie das angepasste Modell und die Grammatik bei Spracherkennungsanforderungen verwenden. Weitere Informationen enthält der Abschnitt [Grammatik bei der Spracherkennung verwenden](/docs/services/speech-to-text/grammar-use.html).

Diese Schritte sind iterativ. Sie können Grammatiken sowie Korpora und angepasste (also benutzerdefinierte) Wörter so häufig wie nötig zu einem angepassten Sprachmodell hinzufügen. Sie müssen das angepasste Modell mit allen neuen Datenressourcen  trainieren, die Sie hinzufügen (Grammatiken, Korpora oder angepasste Wörter). Wenn Sie ein angepasstes Modell für die Spracherkennung verwenden, bildet das Modell die Daten ab, mit denen es zuletzt trainiert wurde.

Sie können Grammatiken bei jeder Sprache verwenden, die die Anpassung des Sprachmodells unterstützt. Die Sprachmodellanpassung ist für die meisten Sprachen verfügbar. Weitere Informationen finden Sie unter [Sprachunterstützung bei der Anpassung](/docs/services/speech-to-text/custom.html#languageSupport).
{: note}

## Angepasstes Sprachmodell erstellen
{: #createModel-grammar}

Damit Sie eine Grammatik für die Spracherkennung verwenden können, müssen Sie sie zu einem angepassten Sprachmodell hinzufügen. Sie können ein vorhandenes angepasstes Sprachmodell verwenden oder mit der Methode `POST /v1/customizations` ein neues angepasstes Modell erstellen. Informationen zum Erstellen eines neuen angepassten Modells enthält der Abschnitt [Angepasstes Sprachmodell erstellen](/docs/services/speech-to-text/language-create.html#createModel-language).

Ein angepasstes Sprachmodell kann Korpora und angepasste Wörter sowie Grammatiken beinhalten. Bei der Spracherkennung können Sie das angepasste Modell mit oder ohne Grammatiken verwenden. Wenn Sie eine Grammatik verwenden, erkennt der Service jedoch nur Wörter aus der angegebenen Grammatik.

## Grammatik zu einem angepassten Sprachmodell hinzufügen
{: #addGrammar}

Mit der Methode `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` können Sie eine Grammatikdatei zu einem angepassten Modell hinzufügen.

-   Geben Sie die Anpassungs-ID des angepassten Sprachmodells mit dem Pfadparameter `customization_id` an.
-   Geben Sie einen Namen für die Grammatik mit dem Pfadparameter `grammar_name` an. Verwenden Sie einen übersetzten Namen in der Sprache des angepassten Modells, der den Inhalt der Grammatik wiedergibt.
    -   Geben Sie einen Namen mit einer maximalen Länge von 128 Zeichen an.
    -   Verwenden Sie im Namen keine Leerzeichen, Schrägstriche oder umgekehrte Schrägstriche.
    -   Verwenden Sie nicht den Namen einer Grammatik oder eines Korpus, die/der bereits zum angepassten Modell hinzugefügt wurde.
    -   Verwenden Sie nicht den Namen `user`; dieser Name ist vom Service für die Kennzeichnung von angepassten Wörtern reserviert, die durch den Benutzer hinzugefügt oder geändert wurden.

-   Geben Sie mit dem Anforderungsheader `Content-Type` das Format der Grammatik an:
    -   `application/srgs` für eine ABNF-Grammatik
    -   `application/srgs+xml` für eine XML-Grammatik

-   Übergeben Sie die Datei, die die Grammatik enthält, als Hauptteil der Anforderung.

Im folgenden Beispiel wird die Grammatikdatei namens `confirm.abnf` zum angepassten Modell mit der angegebenen ID hinzugefügt.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/srgs"
--data-binary @confirm.abnf
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

Die Methode akzeptiert außerdem den optionalen Abfrageparameter `allow_overwrite`, mit dem eine vorhandene Grammatik desselben Namens überschrieben werden kann. Verwenden Sie den Parameter, wenn Sie eine Grammatik aktualisieren müssen, nachdem Sie sie zu einem Modell hinzugefügt haben.

Die Methode wird asynchron ausgeführt. Je nach Größe der Grammatik und aktueller Auslastung des Service kann es einige Minuten dauern, bis der Service die Grammatik analysiert hat. Im Abschnitt [Anforderung zum Hinzufügen einer Grammatik überwachen](#monitorGrammar) erfahren Sie genauer, wie Sie den Status einer Grammatik überprüfen können.

Sie können beliebig viele Grammatiken zu einem angepassten Modell hinzufügen, indem Sie die Methode für jede Grammatikdatei separat aufrufen. Das Hinzufügen einer Grammatik muss vollständig abgeschlossen sein, bevor Sie eine weitere Grammatik hinzufügen können.

### Anforderung zum Hinzufügen einer Grammatik überwachen
{: #monitorGrammar}

Der Service gibt den Antwortcode 201 zurück, wenn die Grammatik gültig ist. Anschließend verarbeitet er den Inhalt der Grammatik im asynchronen Modus und extrahiert automatisch neue Wörter, die von der Grammatik erkannt werden können. Anforderungen zum Hinzufügen weiterer Datenressourcen zu einem angepassten Modell oder zum Trainieren des Modells können Sie erst übergeben, wenn der Service die Analyse der Grammatik für die aktuelle Anforderung abgeschlossen hat.

Mit der Methode `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` können Sie den Status der Grammatik abfragen und so den Status der Analyse ermitteln. Die Methode akzeptiert die ID des angepassten Modells und den Namen der Grammatik. Im folgenden Beispiel wird der Status der Grammatik mit dem Namen `confirm-abnf` überprüft.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

Die Antwort enthält den Status der Grammatik:

```javascript
{
  "name": "confirm-abnf",
  "out_of_vocabulary_words": 0,
  "status": "being_processed"
}
```
{: codeblock}

Das Feld `status` weist einen der folgenden Werte auf:

-   `analyzed`: Der Service hat die Grammatik erfolgreich analysiert.
-   `being_processed`: Der Service ist gegenwärtig noch mit der Analyse der Grammatik beschäftigt.
-   `undetermined`: Der Service hat bei der Verarbeitung der Grammatik einen Fehler festgestellt.

Mithilfe einer Schleife können Sie den Status der Grammatik alle 10 Sekunden prüfen, bis er sich in `analyzed` ändert. Im Abschnitt [Grammatiken für ein angepasstes Sprachmodell auflisten](/docs/services/speech-to-text/grammar-manage.html#listGrammars) erfahren Sie genauer, wie Sie den Status einer Grammatik überprüfen können.

### Fehler in einer Grammatik behandeln
{: #handleFailures}

Falls die Analyse einer Grammatik fehlschlägt, setzt der Service die Grammatik auf den Status `undetermined` und bezieht in die Antwort ein Feld `error` ein, das den Fehler in Bezug auf den Status der Grammatik beschreibt. Außerdem können Sie den Status des angepassten Modells mit der Methode `GET /v1/customizations/{customization_id}` überprüfen. Wenn das Hinzufügen einer Grammatik fehlschlägt, enthält die Ausgabe eine Fehlernachricht wie die Folgende:

```javascript
{
  . . .
  "Fehler": "{\"Code\":500, \"Codebeschreibung\":\"Interner Serverfehler\",
\". . . Grammatik kann nicht kompiliert werden . . .\"},
  . . .
}
```
{: codeblock}

Der Status des angepassten Modells wird vom Fehler nicht beeinflusst, die Grammatik kann jedoch nicht mit dem Modell verwendet werden. Sie können das Problem beheben, indem Sie die Grammatikdatei korrigieren und die Anforderung, sie zum angepassten Modell hinzuzufügen, wiederholen. Geben Sie für den Parameter `allow_overwrite` den Wert `true` an, damit die fehlgeschlagene Version der Grammatikdatei überschrieben wird.

Sie können die Grammatik vorläufig auch aus dem angepassten Modell löschen, die Grammatik anschließend korrigieren und die Grammatikdatei später wieder hinzufügen. Weitere Informationen zum Löschen einer Grammatik enthält der Abschnitt [Grammatik aus einem angepassten Sprachmodell löschen](/docs/services/speech-to-text/grammar-manage.html#deleteGrammar).

## Wörter in der Grammatik validieren
{: #validateGrammar}

Wenn Sie eine Grammatikdatei zu einem angepassten Sprachmodell hinzufügen, analysiert der Service die Grammatik und ermittelt, ob die Grammatik Wörter erkennt, die noch nicht zum Grundvokabular des Service gehören. Solche Wörter werden als 'vokabularexterne Wörter' bezeichnet. Der Service fügt vokabularexterne Wörter zur Wörterressource des angepassten Modells hinzu. Zweck der Wörterressource ist das Definieren von Wörtern, die noch nicht im Vokabular des Services enthalten sind.

Die Definitionen in der Wörterressource weisen den Service an, wie die vokabularexternen Wörter zu transkribieren sind. Die Informationen enthalten ein Feld `sounds_like`, das den Service darüber informiert, wie das Wort ausgesprochen wird, ein Feld `display_as`, das den Service anweist, wie das Wort angezeigt werden soll, und ein Feld `source`, das angibt, wie das Wort zum angepassten Modell hinzugefügt wurde. Weitere Informationen zur Wörterressource und zu vokabularexternen Wörtern finden Sie im Abschnitt [Wörterressource](/docs/services/speech-to-text/language-resource.html#wordsResource).

Nachdem Sie eine Grammatik zu einem angepassten Modell hinzugefügt haben, ist es zweckdienlich, die vokabularexternen Wörter in der Wörterressource des Modells zu untersuchen und ihre gleich klingenden Aussprachevarianten zu prüfen. Nicht jede Grammatik enthält vokabularexterne Wörter, aber eine Überprüfung der Wörterressource ist generell sinnvoll. Mit den folgenden Methoden können Sie die Wörter eines angepassten Modells überprüfen, nachdem Sie eine Grammatik hinzugefügt haben:

-   Mit der Methode `GET /v1/customizations/{customization_id}/words` werden alle Wörter aus einem angepassten Modell aufgelistet. Übergeben Sie den Wert `grammars` im Parameter `word_type` der Methode, damit nur Wörter aufgelistet werden, die aus Grammatiken hinzugefügt wurden.
-   Mit der Methode `GET /v1/customizations/{customization_id}/words/{word_name}` wird ein einzelnes Wort aus einem Modell angezeigt.

Vergewissern Sie sich, dass die gleich klingenden Aussprachevarianten der Wörter präzise und richtig sind. Suchen Sie auch in den Wörtern selbst nach Schreib- und anderen Fehlern. Weitere Informationen zum Validieren und Korrigieren der Wörter in einem angepassten Modell finden Sie im Abschnitt [Wörterressource prüfen](/docs/services/speech-to-text/language-resource.html#validateModel).

## Angepasstes Sprachmodell trainieren
{: #trainGrammar}

Bevor Sie eine Grammatik mit einem angepassten Sprachmodell verwenden können, müssen Sie im letzten Schritt das Modell trainieren. Der erforderliche Prozess ist derselbe wie beim Trainieren eines angepassten Modells mit neuen Korpora oder neuen angepassten Wörtern. Durch das Training wird die Grammatik zur Verwendung bei der Spracherkennung kompiliert. Der Service wandelt die Grammatik bei der Verarbeitung aus ihrem ursprünglichen textbasierten Format in ein binäres Laufzeitformat für die Spracherkennung um. Die Verwendung der Grammatik ist erst nach dem Training des Modells möglich.

Zum Trainieren eines angepassten Modells verwenden Sie die Methode `POST /v1/customizations/{customization_id}/train`. An die Methode übergeben Sie wie im folgenden Beispiel die Anpassungs-ID des Modells, das Sie trainieren wollen.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

Die Methode für das Training wird asynchron ausgeführt. Normalerweise dauert das Training einige Sekunden. Es kann jedoch je nach Größe und Komplexität der Grammatik sowie der aktuellen Auslastung des Service auch mehr Zeit in Anspruch nehmen. Im Abschnitt [Anforderung zum Trainieren eines Modells überwachen](#monitorTraining-grammar) erfahren Sie, wie Sie den Status einer Trainingsoperation überprüfen können.

### Anforderung zum Training überwachen
{: #monitorTraining-grammar}

Der Service gibt den Antwortcode 200 zurück, wenn der Trainingsprozess erfolgreich gestartet wurde. Der Service kann nachfolgende Trainingsanforderungen oder Anforderungen zum Hinzufügen weiterer neuer Grammatiken, Korpora oder Wörter zum angepassten Modell erst akzeptieren, nachdem die aktuelle Trainingsanforderung abgeschlossen wurde.

Mit der Methode `GET /v1/customizations/{customization_id}` können Sie den Status des Modells abrufen und so den Status einer Trainingsanforderung ermitteln. Die Methode akzeptiert die Anpassungs-ID des Modells und gibt die folgenden Informationen zum Modell zurück:

```
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2018-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

Das Feld `status` hat den Wert `training`, während das Modell trainiert wird. Mithilfe einer Schleife können Sie den Status des Modells alle 10 Sekunden prüfen, bis er sich in `available` ändert. Im Abschnitt [Anforderung zum Trainieren eines Modells überwachen](/docs/services/speech-to-text/language-create.html#monitorTraining-language) erfahren Sie genauer, wie Sie eine Trainingsoperation und andere mögliche Statuswerte überwachen können.
