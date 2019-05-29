---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-11"

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

# Angepasstes Sprachmodell erstellen
{: #languageCreate}

Führen Sie die folgenden Schritte aus, um ein angepasstes Sprachmodell für den {{site.data.keyword.speechtotextshort}}-Service zu erstellen:
{: shortdesc}

1.  [Angepasstes Sprachmodell erstellen](#createModel-language). Sie können mehrere angepasste Modelle für dasselbe oder verschiedene Fachgebiete erstellen. Der Erstellungsprozess ist für jedes Modell, das Sie erstellen, gleich. In einer Erkennungsanforderung können Sie immer nur ein einziges angepasstes Modell angeben.
1.  [Korpus zum angepassten Sprachmodell hinzufügen](#addCorpus). Ein Korpus ist ein Klartextdokument, in dem Terminologie aus dem jeweiligen Fachgebiet verwendet wird. Der Service extrahiert aus den Korpora Begriffe, die nicht im Basisvokabular enthalten sind, und erstellt daraus ein Vokabular für das angepasste Modell. Sie können mehrere Korpora zu einem angepassten Modell hinzufügen.
1.  [Wörter zum angepassten Sprachmodell hinzufügen](#addWords). Sie können auch angepasste Wörter einzeln zu einem Modell hinzufügen. Außerdem können Sie mit den gleichen Methoden angepasste Wörter ändern, die aus Korpora extrahiert wurden. Mithilfe der Methoden können Sie Aussprachevarianten für Wörter angeben und festlegen, wie die Wörter in einem Sprachtranskript angezeigt werden.
1.  [Angepasstes Sprachmodell trainieren](#trainModel-language). Nach dem Hinzufügen von Wörter zu dem angepassten Modell müssen Sie das Modell anhand der Wörter trainieren. Durch das Trainieren wird das angepasste Modell auf die Verwendung in der Spracherkennung vorbereitet. Neue oder geänderte Wörter werden erst nach dem Trainieren von dem Modell verwendet.
1.  Nachdem Sie das angepasste Modell trainiert haben, können Sie es in Erkennungsanforderungen verwenden. Wenn die zum Transkribieren übergebenen Audiodaten fachspezifische Wörter enthalten, die im angepassten Modell definiert sind, wird das erweiterte Vokabular des Modells in den Ergebnissen der Anforderungen berücksichtigt. Weitere Informationen finden Sie im Abschnitt [Angepasstes Sprachmodell verwenden](/docs/services/speech-to-text/language-use.html).

Die Schritte zum Erstellen eines benutzerdefinierten Sprachmodells sind iterativ. Sie können beliebig oft Korpora und/oder Wörter hinzufügen und das Modell nach Bedarf trainieren bzw. erneut trainieren.

Außerdem können Sie Grammatiken zu einem angepassten Sprachmodell hinzufügen. Grammatiken begrenzen die Antwort des Service auf diejenigen Wörter, die von einer Grammatik erkannt werden. Weitere Informationen finden Sie im Abschnitt [Grammatiken mit angepassten Sprachmodellen verwenden](/docs/services/speech-to-text/grammar.html).

Die Sprachmodellanpassung ist für die meisten Sprachen verfügbar. Weitere Informationen finden Sie im Abschnitt [Sprachunterstützung für Anpassung](/docs/services/speech-to-text/custom.html#languageSupport).
{: note}

## Angepasstes Sprachmodell erstellen
{: #createModel-language}

Mit der Methode `POST /v1/customizations` können Sie ein neues angepasstes Sprachmodell erstellen. Sie können beliebig viele angepasste Sprachmodelle erstellen, aber in einer Spracherkennungsanforderung kann jeweils nur ein Modell verwendet werden. Die Methode akzeptiert ein JSON-Objekt, das die Attribute des neuen angepassten Modells als Hauptteil der Anforderung definiert.

<table>
  <caption>Tabelle 1. Attribute für ein neues angepasstes Sprachmodell</caption>
  <tr>
    <th style="width:20%; text-align:left">Parameter</th>
    <th style="width:12%; text-align:center">Datentyp</th>
    <th style="text-align:left">Beschreibung</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>Erforderlich</em></td>
    <td style="text-align:center">String</td>
    <td>
      Ein benutzerdefinierter Name für das neue angepasste Modell. Verwenden Sie einen
      Namen, der das Fachgebiet des angepassten Modells beschreibt (z. B. <code>Angepasstes
      Modell für Gesundheit</code> oder <code>Angepasstes Modell für Recht</code>. Der verwendete
      Name sollte innerhalb Ihrer angepassten Sprachmodelle eindeutig sein.
      Verwenden Sie einen lokalisierten Namen in der Sprache des angepassten Modells.
    </td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>Erforderlich</em></td>
    <td style="text-align:center">String</td>
    <td>
      Der Name des Sprachmodells, das durch das neue angepasste Modell
      modifiziert werden soll. Geben Sie eines der unterstützten Sprachmodelle
      an, das von der Methode <code>GET /v1/models</code> zurückgegeben wird. Das
      neue Modell kann nur zusammen mit dem von ihm modifizierten Basismodell verwendet werden.
    </td>
  </tr>
  <tr>
    <td><code>dialect</code><br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td>
      Der Dialekt aus der angegebenen Sprache, der in dem angepassten
      Modell verwendet werden soll. Der Dialekt stimmt standardmäßig mit
      der Sprache des Basismodells überein (z. B. der Dialekt <code>en-US</code>
      für eines der Sprachmodelle für amerikanisches Englisch,
      <code>en-US_BroadbandModel</code> oder
      <code>en-US_NarrowbandModel</code>).<br/></br>
      Der Parameter ist nur für Modelle in Spanisch von Bedeutung, für die
      der Service ein angepasstes Modell erstellt, das für Sprachdaten mit dem
      angegebenen Dialekt geeignet ist:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          <code>es-ES</code> für Spanisch (Kastilien), die Standardeinstellung
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>es-LA</code> für Spanisch (Lateinamerika)
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>es-US</code> für Spanisch (Mexiko, Nordamerika)
        </li>
      </ul>
      Sie müssen einen Dialekt angeben, der für das Basismodell zulässig ist.
    </td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td>
      Eine Beschreibung des neuen Modells. Verwenden Sie eine lokalisierte Beschreibung
      in der Sprache des angepassten Modells.
    </td>
  </tr>
</table>

Im folgenden Beispiel wird ein neues angepasstes Sprachmodell mit dem Namen `Example model` erstellt. Das Modell wird für das Basismodell `en-US-BroadbandModel` erstellt und die zugehörige Beschreibung lautet `Example custom language model`. Der Header `Content-Type` gibt an, dass JSON-Daten an die Methode übergeben werden.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Example model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom language model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations"
```
{: pre}

Das Beispiel gibt die Anpassungs-ID des neuen Modells zurück. Jedes angepasste Modell wird durch eine eindeutige Anpassungs-ID identifiziert. Diese ID ist eine global eindeutige ID (Globally Unique Identifier, GUID). Geben Sie die GUID für ein angepasstes Modell mit dem Parameter `customization_id` der Aufrufe an, die dem Modell zugeordnet sind.

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

Eigner des neuen angepassten Modells ist die Serviceinstanz, deren Berechtigungsnachweise beim Erstellen des Modells verwendet wurden. Weitere Informationen finden Sie im Abschnitt [Eigentumsrecht an angepassten Modellen](/docs/services/speech-to-text/custom.html#customOwner).

## Korpus zum angepassten Sprachmodell hinzufügen
{: #addCorpus}

Nachdem Sie das angepasste Sprachmodell erstellt haben, müssen im nächsten Schritt Daten (fachspezifische Wörter) zu dem Modell hinzugefügt werden. Die empfohlene Vorgehensweise zum Füllen eines angepassten Modells besteht darin, mindestens ein Korpus hinzuzufügen.

Ein Korpus ist eine einfache Textdatei, die im Idealfall Beispielsätze aus dem zugehörigen Fachgebiet enthält. Der Service analysiert den Inhalt der Korpusdatei und extrahiert daraus alle Wörter, die nicht im Basisvokabular des Service vorkommen. Diese Wörter werden als vokabularexterne Wörter (Out-Of-Vocabulary, OOV) bezeichnet.

-   Weitere Informationen zur Verwendung von Korpora finden Sie im Abschnitt [Mit Korpora arbeiten](/docs/services/speech-to-text/language-resource.html#workingCorpora).
-   Weitere Informationen zur Vorgehensweise des Service beim Hinzufügen von Korpora zu einem Modell finden Sie im Abschnitt [Was passiert, wenn ich eine Korpusdatei hinzufüge?](/docs/services/speech-to-text/language-resource.html#parseCorpus)

Durch das Bereitstellen von Sätzen, die neue Wörter enthalten, ermöglichen Korpora dem Service das Erlernen der Wörter im Kontext. Anschließend können Sie die Wörter im Modell einzeln erweitern oder ändern. Das Trainieren eines Modells mit einzelnen Wörtern ist zeitaufwendiger als das Hinzufügen von Wörtern aus Korpora und kann zu weniger effizienten Ergebnissen führen.
{: tip}

Mit der Methode `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` können Sie ein Korpus zu einem angepassten Modell hinzufügen:

-   Geben Sie die Anpassungs-ID des angepassten Modells im Parameter `customization_id` an.
-   Geben Sie einen Namen für das Korpus im Pfadparameter `corpus_name` an. Verwenden Sie einen lokalisierten Namen in der Sprache des angepassten Modells, der den Inhalt des Korpus bezeichnet.
    -   Der Name sollte nicht mehr als 128 Zeichen umfassen.
    -   Verwenden Sie im Namen keine Leerzeichen, Schrägstriche (`/`) oder umgekehrten Schrägstriche (\).
    -   Verwenden Sie nicht den Namen eines Korpus, das bereits zu dem angepassten Modell hinzugefügt wurde.
    -   Verwenden Sie nicht den Namen `user`, der im Service für angepasste Wörter reserviert ist, die vom Benutzer hinzugefügt oder geändert werden.
-   Übergeben Sie die Korpustextdatei als Hauptteil der Anforderung.

Sie können maximal 90.000 vokabularexterne Wöter (OOV-Worter) und insgesamt bis zu 10.000.000 Wörter aus allen Quellen zusammen hinzufügen. Dazu gehören Wörter aus Korpora und Grammatiken sowie Wörter, die Sie direkt hinzufügen. Weitere Informationen finden Sie im Abschnitt [Wie viele Daten brauche ich?](/docs/services/speech-to-text/language-resource.html#wordsResourceAmount)
{: note}

Im folgenden Beispiel wird die Korpustextdatei `healthcare.txt` zu dem angepassten Modell mit der angegebenen ID hinzugefügt. Im Beispiel wird dem Korpus der Name `healthcare` zugeordnet.

```bash
curl -X POST -u "apikey:{apikey}"
--data-binary @healthcare.txt
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/healthcare"
```
{: pre}

Die Methode akzeptiert außerdem den optionalen Parameter `allow_overwrite`, um ein vorhandenes Korpus für ein angepasstest Modell zu überschreiben. Verwenden Sie diesen Parameter, wenn Sie eine Korpusdatei aktualisieren möchten, die Sie zu einem Modell hinzugefügt haben.

Die Methode wird asynchron ausgeführt. Es kann ein bis zwei Minuten dauern, bis die Methode beendet ist. Die Ausführungsdauer ist von der Gesamtzahl der Wörter im Korpus, von der Anzahl der im Korpus gefundenen neuen Wörter und von der aktuellen Auslastung des Service abhängig. Weitere Informationen zum Überprüfen des Status eines Korpus finden Sie im Abschnitt [Anforderung zum Hinzufügen eines Korpus überwachen](#monitorCorpus).

Sie können beliebig viele Korpora zu einem angepassten Modell hinzufügen, indem Sie die Methode für jede Korpustextdatei einmal aufrufen. Das Hinzufügen eines Korpus muss vollständig abgeschlossen sein, bevor ein weiteres Korpus hinzugefügt werden kann. Prüfen Sie nach dem Hinzufügen eines Korpus zu einem angepassten Modell die neuen angepassten Wörter auf Schreibfehler und andere Fehler. Weitere Informationen finden Sie im Abschnitt [Wörterressource prüfen](/docs/services/speech-to-text/language-resource.html#validateModel).

### Anforderung zum Hinzufügen eines Korpus überwachen
{: #monitorCorpus}

Der Service gibt einen Antwortcode 201 zurück, wenn das Korpus gültig ist. Anschließend verarbeitet der Service den Korpusinhalt und extrahiert automatisch neue Wörter. Sie können keine weiteren Anforderungen zum Hinzufügen von Daten für das angepasste Modell oder zum Trainieren des Modells übergeben, bis die Analyse des Korpus für die aktuelle Anforderung abgeschlossen ist.

Den Status der Korpusanalyse können Sie mit der Methode `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` ermitteln. Die Methode akzeptiert die ID des Modells und den Namen des Korpus, wie im folgenden Beispiel gezeigt:

```bash
curl -X GET -u "apikey:{api_schlüssel}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

In der Antwort wird der Status des Korpus angegeben:

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

Im Feld `status` wird einer der folgenden Werte angegeben:

-   `analyzed` gibt an, dass das Korpus vom Service erfolgreich analysiert wurde.
-   `being_processed` gibt an, dass der Service die Analyse des Korpus noch nicht abgeschlossen hat.
-   `undetermined` gibt an, dass der Service beim Verarbeiten des Korpus einen Fehler festgestellt hat.

Überprüfen Sie mit einer Schleife den Status des Korpus alle 10 Sekunden, bis der Status `analyzed` angegeben wird. Weitere Informationen zur Statusüberprüfung für die Korpora eines Modells finden Sie im Abschnitt [Korpora für angepasstes Sprachmodell auflisten](/docs/services/speech-to-text/language-corpora.html#listCorpora).

## Wörter zum angepassten Sprachmodell hinzufügen
{: #addWords}

Obwohl das Hinzufügen von Korpora die empfohlene Methode zum Erweitern des Vokabulars in einem angepassten Sprachmodell ist, können auch einzelne Wörter direkt im Modell hinzugefügt werden. Der Service fügt die angepassten Wörter auf die gleiche Weise zum angepassten Modell hinzu wie vokabularexterne Wörter, die aus Korpora extrahiert wurden.

Wenn Sie nur wenige Wörter zu einem Modell hinzufügen möchten, ist die Verwendung von Korpora möglicherweise keine sinnvolle oder realistische Option. In diesem Fall ist die einfachste Methode das Hinzufügen eines einzelnen Wortes mit zugehöriger Schreibweise. Sie können auch mehrere Aussprachevarianten für das Wort bereitstellen und angeben, wie das Wort angezeigt werden soll.

-   Weitere Informationen zum direkten Hinzufügen von Wörtern finden Sie im Abschnitt [Mit angepassten Wörtern arbeiten](/docs/services/speech-to-text/language-resource.html#workingWords).
-   Weitere Informationen zur Vorgehensweise des Service beim Hinzufügen angepasster Wörter zu einem Modell finden Sie im Abschnitt [Was passiert, wenn ich ein angepasstes Wort hinzufüge oder ändere?](/docs/services/speech-to-text/language-resource.html#parseWord)

Mit den folgenden Methoden können Sie Wörter zu einem angepassten Modell hinzufügen:

-   Die Methode `POST /v1/customizations/{customization_id}/words` fügt mehrere Wörter gleichzeitig hinzu. Dabei wird ein JSON-Objekt mit Informationen zu jedem Wort im Hauptteil der Anforderung übergeben. Im folgenden Beispiel werden die beiden angepassten Wörter `HHonors` und `IEEE` zum angepassten Modell mit der angegebenen ID hinzugefügt. Der Header `Content-Type` gibt an, dass JSON-Daten an die Methode übergeben werden.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"words\": [
      {\"word\": \"HHonors\", \"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"},
      {\"word\": \"IEEE\", \"sounds_like\": [\"I. triple E.\"]}]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    Sie können die Wörter auch aus einer Datei hinzufügen. Beispielsweise sind in der Datei `words.json` dieselben beiden angepassten Wörter definiert.

    ```
    {"words":
      [
        {"word": "HHonors", "sounds_like": ["hilton honors", "H. honors"], "display_as": "HHonors"},
        {"word": "IEEE", "sounds_like": ["I. triple E."]}
      ]
    }
    ```
    {: codeblock}

    Der folgende Befehl fügt die Wörter aus der Datei hinzu:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data-binary @words.json
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    Dies ist eine asynchrone Methode. Die benötigte Ausführungsdauer ist von der Anzahl der hinzugefügten Wörter und von der aktuellen Auslastung des Service abhängig. Weitere Informationen zur Statusprüfung für die Operation finden Sie im Abschnitt [Anforderung zum Hinzufügen von Wörtern überwachen](#monitorWords).
-   Die Methode `PUT /v1/customizations/{customization_id}/words/{word_name}` fügt einzelne Wörter hinzug. Dabei übergeben Sie ein JSON-Objekt mit Informationen zu dem Wort. Im folgenden Beispiel wird das Wort `NCAA` zu dem Modell mit der angegebenen ID hinzugefügt. Der Header `Content-Type` gibt auch in diesem Fall an, dass JSON-Daten an die Methode übergeben werden.

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

    Dies ist eine asynchrone Methode. Der vom Service zurückgegebene Antwortcode meldet sofort den Erfolg oder das Fehlschlagen der Anforderung.

Überprüfen Sie (wie beim Hinzufügen von Korpora) die neuen angepassten Wörter auf Schreibfehler und andere Fehler. Diese Überprüfung ist besonders wichtig, wenn Sie mehrere Wörter gleichzeitig hinzufügen. Weitere Informationen finden Sie im Abschnitt [Wörterressource prüfen](/docs/services/speech-to-text/language-resource.html#validateModel).

### Anforderung zum Hinzufügen von Wörtern überwachen
{: #monitorWords}

Bei Verwendung der Methode `POST /v1/customizations/{customization_id}/words` gibt der Service einen Antwortcode 201 zurück, wenn die Eingabedaten gültig sind. Anschließend werden die Wörter asynchron verarbeitet, um sie zum Modell hinzuzufügen. Dieses Verfahren ist in der Regel schneller als das Hinzufügen eines Korpus oder das Trainieren eines Modells; die Dauer hängt jedoch von der Anzahl der neuen Wörter ab, die Sie hinzufügen, und von der aktuellen Auslastung des Service. Sie können keine weiteren Anforderungen zum Hinzufügen von Daten für das angepasste Modell oder zum Trainieren des Modells übergeben, bis der Service die Anforderung zum Hinzufügen neuer Wörter abgeschlossen hat.

Den Status der Anforderung können Sie mit der Methode `GET /v1/customizations/{customization_id}` ermitteln. Die Methode akzeptiert die Anpassungs-ID des Modells und gibt Informationen zurück, die den Status des Modells enthalten, wie im folgenden Beispiel gezeigt:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

Im Feld `status` wird der aktuelle Status des Modells angegeben. Solange der Service neue Wörter verarbeitet, wird der Status `pending` angegeben. Überprüfen Sie mit einer Schleife den Status alle 10 Sekunden, bis der Status `ready` darauf hinweist, dass der Vorgang abgeschlossen ist. Weitere Informationen zu möglichen Werten für `status` finden Sie im Abschnitt [Anforderung zum Trainieren des Modells überwachen](#monitorTraining-language).

### Wörter in einem angepassten Modell ändern
{: #modifyWord}

Sie können auch die Methoden `POST /v1/customizations/{customization_id}/words` und `PUT /v1/customizations/{customization_id}/words/{word_name}` verwenden, um ein Wort in einem angepassten Modell zu ändern oder zu erweitern. Mit diesen Methoden können Sie bei Bedarf einen Schreibfehler oder einen anderen Fehler korrigieren, der beim Hinzufügen eines Wortes zu dem Modell aufgetreten ist. Außerdem können Sie gleich klingende Aussprachevarianten für ein vorhandenes Wort hinzufügen.

Mit diesen Methoden können Sie die Definition eines vorhandenen Wortes auf die gleiche Weise ändern wie beim Hinzufügen eines Wortes. Die neuen Angaben, die Sie für das Wort bereitstellen, überschreiben die vorhandene Definition des Wortes.

## Angepasstes Sprachmodell trainieren
{: #trainModel-language}

Wenn Sie ein angepasstes Sprachmodel mit neuen Wörtern bestücken (durch Hinzufügen von Korpora oder Grammatiken bzw. durch direktes Hinzufügen des Wortes) müssen Sie das Modell mit den neuen Daten trainieren. Durch Trainieren wird das angepasste Modell vorbereitet, damit die Daten bei der Spracherkennung genutzt werden können. Wörter, die Sie mit einem beliebigen Verfahren hinzugefügt haben, werden vom Modell erst verwendet, nachdem Sie das Modell mithilfe der neuen Daten trainiert haben.

Mit der Methode `POST /v1/customizations/{customization_id}/train` können Sie ein angepasstes Modell trainieren. In der Methode übergeben Sie die Anpassungs-ID des Modells, das Sie trainieren möchten, wie im folgenden Beispiel gezeigt:

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

Sie können den optionalen Abfrageparameter `word_type_to_add` verwenden, um die Wörter anzugeben, mit denen das angepasste Modell trainiert werden soll.

-   Geben Sie `all` an oder lassen Sie den Parameter weg, um das Modell mit allen zugehörigen Wörtern (unabhängig von ihrer Herkunft) zu trainieren.
-   Geben Sie `user` an, um das Modell nur mit Wörtern zu trainieren, die von dem angegebenen Benutzer hinzugefügt oder geändert wurden. Dabei werden Wörter ignoriert, die nur aus Korpora oder Grammatiken extrahiert wurden.

    Diese Option ist hilfreich zum Hinzufügen von Daten mit Störanteilen (z. B. Wörter mit Schreibfehlern). Bevor Sie das Modell mit solchen Daten trainieren, können Sie mit dem Abfrageparameter `word_type` in der Methode `GET /v1/customizations/{customization_id}/words` Wörter überprüfen, die aus Korpora und Grammatiken extrahiert wurden. Weitere Informationen finden Sie im Abschnitt [Wörter aus angepasstem Sprachmodell auflisten](/docs/services/speech-to-text/language-words.html#listWords).

Zusätzlich können Sie den optionalen Abfrageparameter `customization_weight` verwenden. Dieser Parameter gibt die relative Gewichtung für Wörter aus dem angepassten Modell im Vergleich zu Wörtern aus dem Basisvokabular an, wenn das angepasste Modell für die Spracherkennung verwendet wird. Mit jeder Erkennungsanforderung, von der das angepasste Modell verwendet wird, können Sie auch eine Anpassungsgewichtung angeben. Weitere Informationen finden Sie im Abschnitt [Anpassungsgewichtung verwenden](/docs/services/speech-to-text/language-use.html#weight).

Dies ist eine asynchrone Methode. Die Dauer des Trainingsvorgangs kann mehrere Minuten betragen und ist von der Anzahl der neuen Wörter und von der aktuellen Auslastung des Service abhängig. Weitere Informationen zur Statusprüfung für den Trainingsvorgang finden Sie im Abschnitt [Anforderung zum Trainieren des Modells überwachen](#monitorTraining-language).

### Anforderung zum Trainieren des Modells überwachen
{: #monitorTraining-language}

Der Service gibt einen Antwortcode 200 zurück, wenn der Trainingsvorgang erfolgreich eingeleitet wurde. Der Service kann keine weiteren Anforderungen für Trainingsvorgänge bzw. Anforderungen zum Hinzufügen neuer Korpora, Grammatiken oder Wörter akzeptieren, bis die aktuelle Anforderung abgeschlossen ist.

Den Status der Trainingsanforderung können Sie mit der Methode `GET /v1/customizations/{customization_id}` ermitteln. Die Methode akzeptiert die Anpassungs-ID des Modells und gibt Informationen zu dem Modell zurück.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
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

Die Antwort enthält die Felder `status` und `progress`, die den Status des angepassten Modells angeben. Die Bedeutung des Felds `progress` hängt vom Status des Modells ab. Im Feld `status` kann einer der folgenden Werte angegeben werden:

-   `pending` gibt an, dass das Modell erstellt wurde und sich im Wartestatus befindet, da entweder noch keine Trainingsdaten hinzugefügt wurden oder die Analyse der hinzugefügten Daten noch nicht abgeschlossen ist. Das Feld `progress` ist auf den Wert `0` gesetzt.
-   `ready` gibt an, dass das Modell zum Trainieren bereit ist. Das Feld `progress` ist auf den Wert `0` gesetzt.
-   `training` gibt an, dass das Modell momentan trainiert wird. Der Wert im Feld `progress` wird von `0` schrittweise auf `100` erhöht, bis das Training abgeschlossen ist. <!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available` gibt an, dass das Modell trainiert wurde und jetzt verwendet werden kann. Das Feld `progress` ist auf den Wert `100` gesetzt.
-   `upgrading` gibt an, dass das Modell momentan aktualisiert wird. Das Feld `progress` ist auf den Wert `0` gesetzt.
-   `failed` gibt an, dass das Trainieren des Modells fehlgeschlagen ist. Das Feld `progress` ist auf den Wert `0` gesetzt.

Überprüfen Sie mit einer Schleife den Status alle 10 Sekunden, bis der Status `available` angegeben wird. Weitere Information zur Statusprüfung für ein angepasstes Modell finden Sie im Abschnitt [Angepasste Sprachmodelle auflisten](/docs/services/speech-to-text/language-models.html#listModels-language).

### Fehler beim Trainieren
{: #failedTraining-language}

Das Training kann nicht gestartet werden, wenn der Service momentan eine andere Anforderung für das angepasste Sprachmodell ausführt. In den folgenden Situationen schlägt eine Trainingsanforderung fehl:

-   Der Service verarbeitet momentan ein Korpus oder eine Grammatik, um eine Liste mit vokabularexternen Wörtern zu generieren
-   Der Service überprüft momentan angepasste Wörter oder generiert gleich klingende Aussprachevarianten
-   Der Service verarbeitet momentan eine andere Trainingsanforderung

Auch die folgenden Ursachen können zum Fehlschlagen des Trainings führen:

-   Seit dem Erstellen oder dem letzten Trainieren des angepassten Modells wurden keine Trainingsdaten (Korpora, Grammatiken oder Wörter) hinzugefügt.
-   Für mindestens ein Wort, das zu dem angepassten Modell hinzugefügt wurde, sind ungültige Aussprachevarianten vorhanden, die korrigiert werden müssen.

Wenn der Trainingsvorgang für ein angepasstes Modell den Status `failed` aufweist, können Sie mit den Methoden aus der Anpassungsschnittstelle die Wörter in dem Modell untersuchen und gefundene Fehler beheben. Weitere Informationen finden Sie im Abschnitt [Wörterressource prüfen](/docs/services/speech-to-text/language-resource.html#validateModel).

## Beispielscripts
{: #exampleScripts}

Sie können die folgenden Scripts verwenden, um mit den Schritten zum Erstellen eines angepassten Modells zu experimentieren:

-   Ein Python-Script mit dem Namen <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a>. Weitere Informationen finden Sie im Abschnitt [Beispiel für Python-Script](#pythonScript).
-   Ein Bash-Shell-Script mit dem Namen <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a>. Weitere Informationen finden Sie im Abschnitt [Beispiel für Shell-Script](#shellScript).

Die beiden Scripts stellen identische Funktionalität bereit. Jedes Script erstellt ein angepasstes Modell, fügt Wörter aus einer Korpustextdatei hinzu und fügt sowohl Einzelwörter als auch mehrere Wörter direkt zum Modell hinzu. Das Script führt eine Abfrage in dem Modell aus und listet die aus einem Korpus hinzugefügten Wörter sowie die vom Benutzer direkt hinzugefügten Wörter auf. Außerdem wird das Modell trainiert und das empfohlene Polling zum Überwachen der Ergebnisse asynchroner Operationen veranschaulicht.

Sie können jede der beiden bereitgestellten Korpustextdateien mit den Scripts verwenden (oder Sie können eigene Korpusdateien zum Testen verwenden):

-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/corpus.txt" download="corpus.txt">corpus.txt <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a> enthält ein abgekürztes Korpus (6 KB), das sechs Begriffe aus dem Gesundheitswesen zu einem Modell hinzufügt. Diese Datei erzeugt bei Verwendung mit Scripts eine kleine Menge Ausgabedaten. Die Datei wird von den Scripts standardmäßig verwendet.
-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/healthcare.txt" download="healthcare.txt">healthcare.txt <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a> ist ein etwas größeres Korpus (164 KB), das viele Begriffe aus dem Gesundheitswesen zu einem Modell hinzufügt. Diese Datei erzeugt bei Verwendung mit den Scripts wesentlich umfangreichere Ausgabedaten. 

Das neue angepasste Modell, das Sie mit einem dieser Scripts erstellt haben, ist standardmäßig zur Verwendung in Erkennungsanforderungen verfügbar. Die Scripts enthalten einen optionalen Schritt zum Löschen des neuen angepassten Modells. Dieser Schritt kann beim Experimentieren mit dem Prozess nützlich sein. Richten Sie sich nach den Kommentaren in den Scripts, um den Löschvorang zu aktivieren.

### Beispiel für Python-Script
{: #pythonScript}

Gehen Sie wie folgt vor, um das Python-Script zu verwenden:

1.  Laden Sie das Python-Script mit dem Namen <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a> herunter.
1.  Laden Sie die Beispieldateien mit Korpustext herunter, die mit dem Script verwendet werden sollen. Zum Testen können Sie wahlweise eine der bereitgestellten Korpustextdateien oder eine eigene Datei verwenden. Alle Korpustextdateien müssen standardmäßig im selben Verzeichnis enthalten sein wie das Script.
1.  Das Script verwendet die Python-Bibliothek `requests` für HTTP-Anforderungen an den Service. Verwenden Sie `pip` oder `easy_install`, um die Bibliothek zur Verwendung mit dem Script zu installieren. Beispiel: 

    ```bash
    pip install requests
    ```
    {: pre}

    Weitere Informationen zu der Bibliothek finden Sie unter [pypi.python.org/pypi/requests ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://pypi.python.org/pypi/requests){: new_window}.
1.  Bearbeiten Sie das Script und ersetzen Sie die für `password` angegebene Zeichenfolge `iam_apikey` durch den API-Schlüssel aus Ihren {{site.data.keyword.speechtotextshort}}-Serviceberechtigungsnachweisen.

    ```
    password = "iam_apikey"
    ```
    {: codeblock}

1.  Führen Sie das Script aus, indem Sie den folgenden Befehl eingeben:

    ```bash
    python testSTTcustom.py
    ```
    {: pre}

Das Script verwendet die folgende Standard-URL für den Standort Dallas: `https://stream.watsonplatform.net`. Wenn Ihre Serviceinstanz an einem anderen Standort erstellt wurde, ändern Sie die Variablen für `uri` so, dass Ihr Standort verwendet wird. Beispiel: Verwenden Sie `https://gateway-wdc.watsonplatform.net`, wenn sich Ihre Serviceinstanz am Standort Washington DC befindet.
{: note}

### Beispiel für Shell-Script
{: #shellScript}

Gehen Sie wie folgt vor, um das Bash-Shell-Script zu verwenden:

1.  Laden Sie das Shell-Script mit dem Namen <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a> herunter.
1.  Laden Sie die Beispieldateien mit Korpustext herunter, die mit dem Script verwendet werden sollen. Zum Testen können Sie wahlweise eine der bereitgestellten Korpustextdateien oder eine eigene Datei verwenden. Alle Korpustextdateien müssen standardmäßig im selben Verzeichnis enthalten sein wie das Script.
1.  Das Script verwendet den Befehl `curl` für HTTP-Anforderungen an den Service. Falls Sie `curl` noch nicht heruntergeladen haben, können Sie die Version für Ihr Betriebssystem von [curl.haxx.se ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://curl.haxx.se){: new_window} herunterladen. Installieren Sie die Version, die das Protokoll Secure Sockets Layer (SSL) unterstützt, und stellen Sie sicher, dass die installierte Binärdatei in Ihre Umgebungsvariable `PATH` eingefügt wird.
1.  Bearbeiten Sie das Script und ersetzen Sie die für `PASSWORD` angegebene Zeichenfolge `iam_apikey` durch den API-Schlüssel aus Ihren {{site.data.keyword.speechtotextshort}}-Serviceberechtigungsnachweisen:

    ```
    PASSWORD="iam_apikey"
    ```
    {: codeblock}

1.  Bearbeiten Sie das Script, um die Zeichenfolge `URL` durch die URL für den Standort zu ersetzen, an dem Ihre Serviceinstanz erstellt wurde. Das Script verwendet die folgende Standard-URL für den Standort Dallas:

    ```
    URL="https://stream.watsonplatform.net/speech-to-text/api/v1"
    ```
    {: codeblock}

1.  Stellen Sie sicher, dass das Script über Ausführungsberechtigungen verfügt und führen Sie das Script aus, indem Sie den folgenden Befehl eingeben:

    ```bash
    ./testSTTcustom.sh
    ```
    {: pre}
