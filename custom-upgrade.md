---

copyright:
  years: 2017, 2019
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

# Upgrade für angepasste Modelle durchführen
{: #customUpgrade}

Zur Verbesserung der Qualität bei der Spracherkennung werden die Basismodelle durch den {{site.data.keyword.speechtotextfull}}-Service von Zeit zu Zeit aktualisiert. Da die Basismodelle wie auch die Breitband- und Schmalbandmodelle für unterschiedliche Sprachen voneinander unabhängig sind, haben Aktualisierungen an einzelnen Basismodellen keinen Einfluss auf andere Modelle. Im Dokument [Releaseinformationen](/docs/services/speech-to-text?topic=speech-to-text-release-notes) sind alle Aktualisierungen der Basismodelle dokumentiert.
{: shortdesc}

Sobald eine neue Version für ein Basismodell freigegeben wird, müssen Sie für alle angepassten Sprach- und Akustikmodelle, die auf dem Basismodell aufbauen, ein Upgrade durchführen, um die Aktualisierungen zu nutzen. Bis Sie das Upgrade durchgeführt haben, verwenden Ihre angepassten Modelle weiterhin die ältere Version des Basismodells. Wie bei allen Anpassungsoperationen müssen Sie für das Upgrade eines Modells die Berechtigungsnachweise der Serviceinstanz verwenden, die Eigner des Modells ist.

Führen Sie das Upgrade auf die neueste Version eines aktualisierten Basismodells baldmöglichst durch. Je schneller Sie für ein angepasstes Modell ein Upgrade durchführen, desto eher können Sie von der verbesserten Leistung des neuen Modells profitieren. Ältere Versionen von Basismodellen werden zudem möglicherweise zu einem künftigen Zeitpunkt entfernt. Um Sie auf die Möglichkeit eines Upgrades hinzuweisen, gibt der Service zusammen mit den Ergebnissen für Erkennungsanforderungen, die auf älteren Basismodellen basierende angepasste Modelle verwenden, eine Warnung aus.

## Funktionsweise des Upgrades
{: #upgradeOverview}

Wenn ein neues Basismodell erstmalig freigegeben wird, basieren vorhandene angepasste Modelle weiterhin auf der älteren Version des Basismodells. Bis zum Upgrade eines angepassten Modells betreffen alle Operationen für dieses angepasste Modell (beispielsweise das Hinzufügen von Daten oder das Trainieren des Modells) die bestehende Version des Modells. Analog verwenden alle Erkennungsanforderungen, die das angepasste Modell angeben, die bestehende Version des Modells.

Nach einem Upgrade gibt es zwei Versionen eines angepassten Modells. Eine Version basiert auf dem Basismodell und eine Version basiert auf der neuesten Version des Basismodells. Sobald für ein angepasstes Modell ein Upgrade durchgeführt wurde, betreffen alle Operationen für dieses angepasste Modell die neuere Version des Modells nach dem Upgrade. Anschließend ist es nicht mehr möglich, Daten hinzuzufügen oder die ältere Version des Modells zu trainieren. Eine Upgradeoperation kann überdies nicht rückgängig gemacht werden.

Beim Upgrade eines angepassten Modells gleich welchen Typs müssen Sie für seine einzelnen Ressourcen kein Upgrade durchführen. Bei einem angepassten Sprachmodell führt der Service automatisch ein Upgrade für alle Korpora, Grammatiken und Wörter durch, die für das Modell definiert sind. Wenn Sie ein Upgrade für ein angepasstes Akustikmodell durchführen, führt der Service analog automatisch ein Upgrade für dessen Audioressourcen durch.

Standardmäßig verwendet der Service die aktuellste Version des angepassten Modells für Erkennungsanforderungen. Sie können jedoch weiterhin die ältere Version eines angepassten Modells für die Spracherkennung verwenden. Weitere Informationen enthält der Abschnitt [Erkennungsanforderungen mit angepassten Modellen nach einem Upgrade durchführen](#upgradeRecognition).

## Upgrade für ein angepasstes Sprachmodell durchführen
{: #upgradeLanguage}

Gehen Sie wie folgt vor, um ein Upgrade für ein angepasstes Sprachmodell durchzuführen:

1.  Vergewissern Sie sich, dass das angepasste Sprachmodell den Status `ready` oder `available` aufweist. Den Status eines Modells können Sie mit der Methode `GET /v1/customizations/{customization_id}` überprüfen. Falls der Status des Modells `ready` lautet, führen Sie das Upgrade für das Modell aus, bevor Sie es mit den neuesten Daten trainieren.

1.  Verwenden Sie zum Upgrade des angepassten Sprachmodells die Methode `POST /v1/customizations/{customization_id}/upgrade_model`:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    Die Methode für das Upgrade wird asynchron ausgeführt. Abhängig von der Anzahl der Wörter in der Wörterressource des Modells und der aktuellen Auslastung des Service kann das Upgrade einige Minuten dauern.

Der Service gibt den Antwortcode 200 zurück, wenn der Upgradeprozess erfolgreich gestartet wurde. Mit der Methode `GET /v1/customizations/{customization_id}` können Sie den Status des Modells abrufen und so den Status des Upgrades überwachen. Verwenden Sie eine Schleife, um den Status alle 10 Sekunden zu überprüfen.

Während des Upgrades weist das angepasste Modell den Status `upgrading` auf. Nach Abschluss des Upgrades erhält das Modell wieder den Status, den es vor dem Upgrade besaß (`ready` oder `available`). Der Status einer Upgradeoperation wird auf dieselbe Weise überprüft wie der Status einer Trainingsoperation. Weitere Informationen finden Sie unter [Anforderung zum Trainieren des Modells überwachen](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#monitorTraining-language).

Der Service kann keine Anforderungen zum Ändern des Modells akzeptieren, bis die Upgradeanforderung vollständig verarbeitet wurde. Während des Upgrades können Sie jedoch weiterhin Erkennungsanforderungen mit der vorhandenen Version des Modells ausgeben.

## Upgrade für ein angepasstes Akustikmodell durchführen
{: #upgradeAcoustic}

Gehen Sie wie folgt vor, um ein Upgrade für ein angepasstes Akustikmodell durchzuführen. Falls das angepasste Akustikmodell mit einem angepassten Sprachmodell trainiert wurde, müssen Sie zwei zusätzliche Upgradeschritte durchführen; diese sind entsprechend angegeben.

1.  *Falls das angepasste Akustikmodell mit einem angepassten Sprachmodell trainiert wurde,* müssen Sie zuerst für das angepasste Sprachmodell ein Upgrade auf die neueste Version des Basismodells durchführen. Weitere Informationen finden Sie unter [Upgrade für ein angepasstes Sprachmodell durchführen](#upgradeLanguage).

1.  Vergewissern Sie sich, dass das angepasste Akustikmodell den Status `ready` oder `available` aufweist. Den Status eines Modells können Sie mit der Methode `GET /v1/acoustic_customizations/{customization_id}` überprüfen. Falls der Status des Modells `ready` lautet, führen Sie das Upgrade für das Modell aus, bevor Sie es mit den neuesten Daten trainieren.

1.  Verwenden Sie zum Upgrade des angepassten Akustikmodells die Methode `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    Die Methode für das Upgrade wird asynchron ausgeführt. Abhängig von der Menge der Audiodaten, die das Modell enthält, und von der aktuellen  Auslastung des Service kann das Upgrade Minuten oder auch Stunden dauern. Wie beim Training ist die Dauer des Upgrades in der Regel etwa doppelt so lang wie die der Audiodaten des Modells.

1.  *Falls das angepasste Akustikmodell mit einem angepassten Sprachmodell trainiert wurde,* müssen Sie für das angepasste Akustikmodell erneut ein Upgrade durchführen, dieses Mal mit dem zuvor aktualisierten angepassten Sprachmodell. Geben Sie mit dem Abfrageparameter `custom_language_model_id` die Anpassungs-ID des angepassten Sprachmodells an.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model?custom_language_model_id={custom_language_model_id}"
    ```
    {: pre}

    Auch diese Methode für das Upgrade wird asynchron ausgeführt; das Upgrade dauert normalerweise wieder doppelt so lang wie die Audiodaten des Modells.

    Die Anforderung für das Upgrade des Akustikmodells mit dem Sprachmodell schlägt möglicherweise mit dem Antwortcode 400 und der Nachricht `Keine geänderten Eingabedaten seit dem letzten Training` fehl. Wenn dieser Fehler auftritt, fügen Sie der Anforderung den booleschen Abfrageparameter `force` hinzu und setzen Sie den Parameter auf `true`. Verwenden Sie den Parameter ausschließlich in dieser speziellen Situation, um das Upgrade eines angepassten Akustikmodells zu erzwingen.
    {: note}

Der Service gibt den Antwortcode 200 zurück, wenn der Upgradeprozess erfolgreich gestartet wurde. Mit der Methode `GET /v1/acoustic_customizations/{customization_id}` können Sie den Status des Modells abrufen und so den Status des Upgrades überwachen. Verwenden Sie eine Schleife, um den Status einmal pro Minute zu überprüfen.

Während des Upgrades weist das angepasste Modell den Status `upgrading` auf. Nach Abschluss des Upgrades erhält das Modell wieder den Status, den es vor dem Upgrade besaß (`ready` oder `available`). Der Status einer Upgradeoperation wird auf dieselbe Weise überprüft wie der Status einer Trainingsoperation. Weitere Informationen finden Sie unter [Anforderung zum Trainieren des Modells überwachen](/docs/services/speech-to-text?topic=speech-to-text-acoustic#monitorTraining-acoustic).

Der Service kann keine Anforderungen zum Ändern des Modells akzeptieren, bis die Upgradeanforderung vollständig verarbeitet wurde. Während des Upgrades können Sie jedoch weiterhin Erkennungsanforderungen mit der vorhandenen Version des Modells ausgeben.

## Upgradefehler
{: #upgradeFailures}

Das Upgrade eines angepassten Modells kann nicht gestartet werden, wenn der Service eine andere Anforderung für das Modell verarbeitet, wie z. B. eine Trainingsanforderung oder eine Anforderung zum Hinzufügen von Daten. Das Starten der Upgradeanforderung schlägt außerdem in den folgenden Fällen fehl:

-   Das angepasste Modell weist einen anderen Status als `ready` oder `available` auf.
-   Das angepasste Modell enthält keine Daten (angepasste Wörter oder Audioressourcen).
-   Bei einem angepassten Akustikmodell, das mit einem angepassten Sprachmodell trainiert wurde, basieren die angepassten Modelle auf unterschiedlichen Versionen des Basismodells. Sie müssen ein Upgrade für das angepasste Sprachmodell durchführen, bevor Sie das Upgrade des angepassten Akustikmodells durchführen.

## Versionsinformationen für ein angepasstes Modell auflisten
{: #upgradeList}

Mit den folgenden Methoden können Sie die Versionen des Basismodells auflisten, für die ein angepasstes Modell verfügbar ist:

-   Verwenden Sie zum Auflisten von Informationen zu einem angepassten Sprachmodell die Methode `GET /v1/customizations/{customization_id}`. Weitere Informationen finden Sie im Abschnitt [Angepasste Sprachmodelle auflisten](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).
-   Verwenden Sie zum Auflisten von Informationen zu einem angepassten Akustikmodell die Methode `GET /v1/acoustic_customizations/{customization_id}`. Weitere Informationen finden Sie im Abschnitt [Angepasste Akustikmodelle auflisten](/docs/services/speech-to-text?topic=speech-to-text-manageAcousticModels#listModels-acoustic).

In beiden Fällen umfasst die Ausgabe ein Feld `versions`, das Informationen zu den Basismodellen für das angepasste Modell enthält. Die folgende Ausgabe zeigt Informationen zu einem angepassten Sprachmodell:

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

Das Feld `versions` gibt an, dass das angepasste Modell für zwei Versionen des Basismodells verfügbar ist. Die ältere Version ist `en-US_BroadbandModel.v07-06082016.06202016`, die neuere Version heißt `en-US_BroadbandModel.v2017-11-15`. Falls für das angepasste Modell kein Upgrade durchgeführt wurde oder es nur eine einzige Version seines Basismodells gibt, ist im Feld `versions` eine einzige Version angegeben.

## Erkennungsanforderungen mit angepassten Modellen nach einem Upgrade durchführen
{: #upgradeRecognition}

Standardmäßig verwendet der Service die neueste Version eines angepassten Modells, das bei einer Erkennungsanforderung angegeben ist. Aber auch nach einem Upgrade für ein angepasstes Modell können Sie für Erkennungsanforderungen weiterhin die ältere Version des Modells verwenden. Um die Version eines Basismodells anzugeben, die für die Spracherkennung verwendet werden soll, verwenden Sie den Parameter `base_model_version` einer Erkennungsmethode.

Die folgende HTTP-Anforderung gibt beispielsweise an, dass die ältere Version des Basismodells verwendet werden soll. Daher wird auch die ältere Version des angegebenen angepassten Sprachmodells verwendet.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{pfad}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_BroadbandModel&base_model_version=en-US_BroadbandModel.v07-06082016.06202016&language_customization_id={anpassungs-id}"
```
{: pre}

Mithilfe dieser Funktion können Sie das Leistungsverhalten und die Genauigkeit eines angepassten Modells sowohl mit der alten als auch mit der neuen Version seines Basismodells testen. Falls Sie die Leistung eines Modells nach dem Upgrade nicht zufriedenstellt (weil beispielsweise bestimmte Wörter nicht mehr erkannt werden), können Sie weiterhin das ältere Modell bei Erkennungsanforderungen verwenden.

Im Abschnitt [Version des Basismodells](/docs/services/speech-to-text?topic=speech-to-text-input#version) ist der Parameter `base_model_version` beschrieben; dort erfahren Sie auch, wie der Service ermittelt, welche Versionen des Basismodells und des angepassten Modells bei einer Erkennungsanforderung zu verwenden sind. Neben diesen Informationen sollten Sie auch die folgenden Aspekte berücksichtigen, wenn Sie bei einer Erkennungsanforderung sowohl ein angepasstes Sprachmodell als auch ein angepasstes Akustikmodell übergeben:

-   Die beiden angepassten Modelle müssen auf demselben Basismodell basieren (z. B. `en-US_BroadbandModel`).
-   Falls die beiden angepassten Modelle auf dem älteren Basismodell basieren, verwendet der Service das alte Basismodell für die Erkennung.
-   Falls die beiden angepassten Modelle auf dem neueren Basismodell basieren, verwendet der Service das neue Basismodell für die Erkennung.
-   Falls nur für eines der beiden angepassten Modelle ein Upgrade auf das neuere Basismodell durchgeführt wurde, verwendet der Service das alte  Basismodell für die Erkennung. Er wählt das alte Basismodell aus, weil dies die Version ist, die von beiden angepassten Modellen gemeinsam verwendet wird.
