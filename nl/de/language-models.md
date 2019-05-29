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

# Angepasste Sprachmodelle verwalten
{: #manageLanguageModels}

Die Anpassungsschnittstelle enthält die Methode `POST /v1/customizations` zum Erstellen eines angepassten Sprachmodells. Außerdem enthält die Schnittstelle die Methode `POST /v1/customizations/train` zum Trainieren eines angepassten Modells mit den aktuellen Daten der zugehörigen Wörterressource. Weitere Informationen enthält die folgende Dokumentation:
{: shortdesc}

-   [Angepasstes Sprachmodell erstellen](/docs/services/speech-to-text/language-create.html#createModel-language)
-   [Angepasstes Sprachmodell trainieren](/docs/services/speech-to-text/language-create.html#trainModel-language)

Darüber hinaus enthält die Schnittstelle Methoden zum Auflisten von Informationen zu angepassten Sprachmodellen und zum Zurücksetzen eines angepassten Modells in den Anfangsstatus und zum Löschen eines angepassten Modells.

## Angepasste Sprachmodelle auflisten
{: #listModels-language}

Die Anpassungsschnittstelle bietet zwei Methoden zum Auflisten von Informationen zu den angepassten Sprachmodellen, deren Eigner die angegebenen Serviceberechtigungsnachweise sind:

-   Die Methode `GET /v1/customizations` listet Informationen zu allen angepassten Sprachmodellen oder zu allen angepassten Sprachmodellen für eine angegebene Sprache auf.
-   Die Methode `GET /v1/customizations/{customization_id}` listet Informationen zu einem angegebenen angepassten Sprachmodell auf. Verwenden Sie diese Methode, um im Service den Status einer Trainingsanforderung oder einer Anforderung zum Hinzufügen neuer Wörter abzufragen.

Beide Methoden geben die folgenden Informationen zu einem angepassten Modell zurück:

-   `customization_id` gibt die GUID (Globally Unique Identifier) des angepassten Modells zurück. Die GUID dient zum Identifizieren des Modells in den Methoden der Schnittstelle.
-   `created` ist der Zeitpunkt (Datum und Uhrzeit) in koordinierter Weltzeit (Coordinated Universal Time, UTC), an dem das angepasste Modell erstellt wurde.
-   `language` ist die Sprache des angepassten Modells.
-   `dialect` ist der Dialekt der Sprache für das angepasste Sprachmodell.
-   `owner` gibt die Berechtigungsnachweise der Serviceinstanz an, die Eigner des angepassten Modells ist.
-   `name` ist der Name des angepassten Modells.
-   `description` ist eine Beschreibung des angepassten Modells, sofern beim Erstellen des Modells eine Beschreibung angegeben wurde.
-   `base_model` gibt den Namen des Sprachmodells an, für das das angepasste Modell erstellt wurde.
-   `versions` stellt eine Liste der verfügbaren Versionen des angepassten Modells zur Verfügung. Jedes Element in dem Array gibt eine Version des Basismodells an, mit der das angepasste Modell verwendet werden kann. Mehrere Versionen sind nur vorhanden, wenn das angepasste Modell aktualisiert wird. Andernfalls wird nur eine einzige Version angezeigt. Weitere Informationen finden Sie im Abschnitt [Versionsinformationen für ein angepasstes Modell auflisten](/docs/services/speech-to-text/custom-upgrade.html#upgradeList).

Die Methode gibt außerdem ein Feld `status` zurück, das den Status des angepassten Modells angibt.

-   `pending` gibt an, dass das Modell erstellt wurde. Das Modell wartet darauf, dass entweder Trainingsdaten hinzugefügt werden, oder dass die Analyse der hinzugefügten Daten abgeschlossen wird.
-   `ready` gibt an, dass das Modell Daten enthält und jetzt trainiert werden kann.
-   `training` gibt an, dass das Modell momentan mit Daten trainiert wird.
-   `available` gibt an, dass das Modell trainiert wurde und nun in einer Erkennungsanforderung verwendet werden kann.
-   `upgrading` gibt an, dass das Modell momentan aktualisiert wird.
-   `failed` gibt an, dass das Trainieren des Modells fehlgeschlagen ist. Prüfen Sie die Wörter in der Wörterressource des Modells auf Fehler, die das Trainieren des Modells verhindern.

Darüber hinaus enthält die Ausgabe ein Feld `progress`, in dem der aktuelle Fortschritt beim Trainieren des angepassten Modells angegeben wird. Wenn Sie das Training für das Modell mithilfe der Methode `POST /v1/customizations/{customization_id}/train` gestartet haben, wird in diesem Feld der aktuelle Fortschritt der zugehörigen Anforderung als Prozentwert angegeben. Bei Fertigstellung des Trainings wird im Feld der Wert `100` angegebenen, sofern der Status `available` lautet; andernfalls wird der Wert `0` angegeben.

### Beispiele für Anforderungen und Antworten
{: #listExample-language}

Das folgende Beispiel enthält den Abfrageparameter `language`, um alle angepassten Sprachmodelle für amerikanisches Englisch aufzulisten, deren Eigner die Serviceberechtigungsnachweise sind:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations?language=en-US"
```
{: pre}

Die Serviceberechtigungsnachweise sind Eigner von zwei solchen Modellen. Das erste Modell ist für Daten empfangsbereit oder wird vom Service verarbeitet. Das zweite Modell ist vollständig trainiert und betriebsbereit.

```javascript
{
  "customizations": [
    {
      "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
      "created": "2016-06-01T18:42:25.324Z",
      "language": "en-US",
      "dialect": "en-US",
      "versions": [
        "en-US_BroadbandModel.v07-06082016.06202016",
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model",
      "description": "Example custom language model",
      "base_model_name": "en-US_BroadbandModel",
      "status": "pending",
      "progress": 0
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732fae4829",
      "created": "2016-06-01T18:51:37.291Z",
      "language": "en-US",
      "dialect": "en-US",
      "versions": [
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model two",
      "description": "Example custom language model two",
      "base_model_name": "en-US_BroadbandModel",
      "status": "available",
      "progress": 100
    }
  ]
}
```
{: codeblock}

Das folgende Beispiel gibt Informationen zu dem angepassten Modell zurück, das die angegebene Anpassungs-ID aufweist:

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
  "versions": [
    "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

## Angepasstes Sprachmodell zurücksetzen
{: #resetModel-language}

Mit der Methode `POST /v1/customizations/{customization_id}/reset` können Sie ein angepasstes Modell zurücksetzen. Beim Zurücksetzen eines Modells werden alle Korpora und Wörter aus dem Modell entfernt, d. h. das Modell wird auf den ursprünglichen Erstellungsstatus zurückgesetzt. Die Methode löscht weder das Modell noch die zugehörigen Metadaten wie Name und Sprache. Nach dem Zurücksetzen des Modells ist die zugehörige Wörterressource leer und muss erneut erstellt werden (durch Hinzufügen von Korpora und Wörtern).

### Beispielanforderung
{: #resetExample-language}

Im folgenden Beispiel wird das angepasste Modell mit der angegebenen Anpassungs-ID zurückgesetzt:

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/reset"
```
{: pre}

## Angepasstes Sprachmodell löschen
{: #deleteModel-language}

Mit der Methode `DELETE /v1/customizations/{customization_id}` können Sie ein angepasstes Sprachmodell löschen, das nicht mehr benötigt wird. Diese Methode löscht alle Korpora und Wörter, die dem angepassten Modell zugeordnet sind, sowie das Modell selbst. Verwenden Sie diese Methode mit Vorsicht: Nach dem Löschen können Sie das angepasste Modell und die zugehörigen Daten nicht wiederherstellen.

### Beispielanforderung
{: #deleteExample-language}

Im folgenden Beispiel wird das angepasste Modell mit der angegebenen Anpassungs-ID gelöscht:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}
