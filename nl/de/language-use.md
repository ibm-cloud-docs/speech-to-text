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

# Angepasstes Sprachmodell verwenden
{: #languageUse}

Nachdem Sie das angepasste Sprachmodell erstellt und trainiert haben, können Sie es in Spracherkennungsanforderungen verwenden. Im Parameter `language_customization_id` können Sie das angepasste Sprachmodell für eine Anforderung angeben, wie in den folgenden Beispielen gezeigt. Sie können angeben, welche Gewichtung der Service den Wörtern aus dem angepassten Modell zuteilen soll. Weitere Informationen finden Sie im Abschnitt [Anpassungsgewichtung verwenden](#weight). Sie müssen die Anforderung mit Serviceberechtigungsnachweisen für die Instanz des Service absetzen, die Eigner des Modells ist.
{: shortdesc}

Sie können mehrere angepasste Sprachmodelle für dasselbe Fachgebiet oder für verschiedene Fachgebiete erstellen. Mit dem Parameter `language_customization_id` kann jedoch nur ein angepasstes Sprachmodell auf einmal angegeben werden. Beispiele für die Verwendung einer Grammatik mit einem angepassten Sprachmodell finden Sie im Abschnitt [Grammatik für Spracherkennung verwenden](/docs/services/speech-to-text/grammar-use.html).

-   Verwenden Sie für die [WebSocket-Schnittstelle](/docs/services/speech-to-text/websockets.html) die Methode `/v1/recognize`. Das angegebene angepasste Modell wird für alle über die Verbindung gesendeten Anforderungen verwendet.

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=es-ES_BroadbandModel'
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   Verwenden Sie für die [synchrone HTTP-Schnittstelle](/docs/services/speech-to-text/http.html) die Methode `POST /v1/recognize`. Das angegebene angepasste Modell wird für diese Anforderung verwendet.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}"
    ```
    {: pre}
-   Verwenden Sie für die [asynchrone HTTP-Schnittstelle](/docs/services/speech-to-text/async.html) die Methode `POST /v1/recognitions`. Das angegebene angepasste Modell wird für diese Anforderung verwendet.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?language_customization_id={customization_id}"
    ```
    {: pre}

Sie müssen das Sprachmodell in der Anforderung nicht angeben, wenn das angepasste Modell auf dem Standardsprachmodell `en-US_BroadbandModel` basiert. Andernfalls müssen Sie im Parameter `model` das Basismodell angeben, wie im Beispiel für WebSocket gezeigt. Ein angepasstes Modell kann nur zusammen mit dem Basismodell verwendet werden, für das es erstellt wurde.

## Anpassungsgewichtung verwenden
{: #weight}

Ein angepasstes Sprachmodell ist eine Kombination aus dem angepassten Modell und dem Basismodell, das von ihm angepasst wird. Sie können angeben, welche Gewichtung der Service den Wörtern aus dem angepassten Sprachmodell bei der Spracherkennung im Vergleich zu Wörtern aus dem Basismodell zuteilen soll. Die einem angepassten Modell zugeordnete Gewichtung wird als *Anpassungsgewichtung* bezeichnet.

Die relative Gewichtung für ein angepasstes Sprachmodell wird als Doppelzeichen im Bereich zwischen 0,0 und 1,0 angegeben. Standardmäßig wird jedem angepassten Sprachmodell die Gewichtung 0,3 zugeordnet. Mit dieser Standardgewichtung werden im Allgemeinen die besten Leistungswerte erzielt. Sie ermöglicht sowohl die Erkennung von OOV-Wörtern aus dem angepassten Modell als auch von Wörtern aus dem Basisvokabular.

Wenn die zu transkribierenden Audiodaten jedoch häufig auf OOV-Wörter aus dem angepassten Modell zurückgreifen, kann die Genauigkeit der Transkriptionsergebnisse durch eine höhere Anpassungsgewichtung verbessert werden. Gehen Sie beim Festlegen der Anpassungsgewichtung mit besonderer Sorgfalt vor. Eine höhere Gewichtung kann die Genauigkeit bei Ausdrücken aus dem Fachgebiet des angepassten Modells erhöhen, aber sie kann auch die Leistung bei fachgebietsfremden Ausdrücken beeinträchtigen. (Selbst bei der Gewichtung 0,0 besteht aufgrund der Implementierung der Sprachmodellanpassung eine geringe Wahrscheinlichkeit, dass die Transkription angepasste Wörter enthalten kann.)

Die Anpassungsgewichtung kann mit dem Parameter `customization_weight` angegeben werden. Dieser Parameter wird beim Trainieren eines angepassten Sprachmodells oder bei dessen Verwendung in einer Spracherkennungsanforderung angegeben.

-   Im folgenden Beispiel wird für eine Trainingsanforderung die Anpassungsgewichtung `0,5` in der Methode `POST /v1/customizations/{customization_id}/train` angegeben:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train?customization_weight=0.5"
    ```
    {: pre}

    Eine in der Trainingsphase angegebene Anpassungsgewichtung wird zusammen mit dem angepassten Sprachmodell gespeichert. Sie müssen die Gewichtung nicht mit jeder Erkennungsanforderung übergeben, die das angepasste Modell verwendet.

-   Im folgenden Beispiel wird für eine Erkennungsanforderung die Anpassungsgewichtung `0,7` in der Methode `POST /v1/recognize` angegeben:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.7"
    ```
    {: pre}

    Durch das Festlegen einer Anpassungsgewichtung während der Spracherkennung wird die beim Trainieren zusammen mit dem Modell gespeicherte Gewichtung überschrieben.

## Fehlerbehebung bei der Verwendung angepasster Sprachmodelle
{: #languageTroubleshoot}

Wenn der Service trotz Anwendung eines angepassten Sprachmodells bei der Spracherkennung scheinbar nicht die Wörter aus dem Modell verwendet, überprüfen Sie, ob die folgenden potenziellen Probleme vorliegen:

-   Stellen Sie sicher, dass die Anpassungs-ID ordnungsgemäß an die Erkennungsanforderung übergeben wird, wie im Abschnitt [Angepasstes Sprachmodell verwenden](#languageUse) dargestellt.
-   Stellen Sie sicher, dass das angepasste Modell den Status `available` (verfügbar) aufweist, d. h. das Modell ist vollständig trainiert und betriebsbereit. Weitere Informationen finden Sie im Abschnitt [Angepasste Sprachmodelle auflisten](/docs/services/speech-to-text/language-models.html#listModels-language).
-   Prüfen Sie die für die neuen Wörter generierten Aussprachevarianten und stellen Sie sicher, dass diese korrekt sind. Weitere Informationen finden Sie im Abschnitt [Wörterressource prüfen](/docs/services/speech-to-text/language-resource.html#validateModel).
