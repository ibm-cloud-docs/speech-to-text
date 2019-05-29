---

copyright:
  years: 2019
lastupdated: "2019-03-10"

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

# Angepasste Akustikmodelle und angepasste Sprachmodelle kombiniert verwenden
{: #useBoth}

Sie können die Genauigkeit bei der Spracherkennung verbessern, indem Sie angepasste Sprachmodelle und angepasste Akustikmodelle kombiniert einsetzen. Beide Modelltypen können Sie  während des Trainings Ihres Akustikmodells und während der Spracherkennung verwenden. Beide Modelle müssen Eigentum derselben Serviceinstanz sein und auf demselben Basissprachmodell basieren.
{: shortdesc}

Die Verwendung eines angepassten Akustikmodells ohne ein Sprachmodell oder mit einem angepassten Sprachmodell, für das es nicht trainiert wurde, kann dennoch von Nutzen sein. Falls das angepasste Akustikmodell mit akustischen Merkmalen trainiert wurde, die zu den transkribierten Audiodaten passen, kann es trotzdem die Qualität der Transkription verbessern.

## Angepasstes Akustikmodell mit einem angepassten Sprachmodell trainieren
{: #useBothTrain}

Das Training eines angepassten Akustikmodells ausschließlich mit Audiodaten wird als *nicht überwachtes Training* bezeichnet. Die Verwendung eines angepassten Sprachmodells während des Trainings wird *mäßig überwachtes Training* genannt. Ein mäßig überwachtes Training kann die Effizienz Ihres angepassten Akustikmodells verbessern.

Verwenden Sie in den folgenden Fällen ein mäßig überwachtes Training, um ein angepasstes Akustikmodell mit einem angepassten Sprachmodell zu trainieren:

-   Das angepasste Sprachmodell basiert auf Transkriptionen (Verbatim-Text) aus den Audiodateien, die Sie zum angepassten Akustikmodell hinzugefügt haben.

    Da Transkriptionen den exakten Inhalt der Audiodaten enthalten, kann das Training mit einem angepassten Sprachmodell, das auf Transkriptionen basiert, die besten Ergebnisse erzielen. Der Service kann den Inhalt der Transkriptionen in ihrem Kontext analysieren und vokabularexterne Wörter sowie N-Gramme extrahieren, mit deren Hilfe er Ihre Audiodaten am effizientesten nutzen kann. Dies gilt insbesondere dann, wenn Ihre Audiodaten weniger als eine Stunde lang sind.

    Das Transkribieren der Audiodaten ist nicht unbedingt erforderlich. Falls Sie jedoch über Transkriptionen der Audiodaten verfügen, können diese die Qualität des angepassten Akustikmodells verbessern. Transkriptionen sind besonders zweckdienlich, wenn die Audiodaten viele vokabularexterne Wörter enthalten.
-   Das angepasste Sprachmodell basiert auf Korpora (Textdateien) oder einer Liste von Wörtern, die für den Inhalt der Audiodateien relevant sind.

    Falls Ihre Audiodaten fachspezifische Wörter enthalten, die nicht im Grundvokabular des Service zu finden sind, werden diese Wörter während der Transkription nicht allein durch die Akustikmodellanpassung erzeugt. Das Grundvokabular des Service kann einzig durch eine Sprachmodellanpassung erweitert werden. Falls Sie keine Transkriptionen besitzen, führen Sie das Training mit einem angepassten Sprachmodell durch, das vokabularexterne Wörter aus demselben Fachgebiet wie Ihre Audiodaten enthält. Sogar ein Training mit einem angepassten Sprachmodell, das eine Liste der in den Audiodaten verwendeten vokabularexternen Wörter enthält, kann sich als hilfreich erweisen.

    Beispiel: Sie erstellen ein angepasstes Akustikmodell, das auf Audiodaten aus einem Call-Center für ein bestimmtes Produkt basiert. Sie können das angepasste Akustikmodell mit einem angepassten Sprachmodell trainieren, das auf Transkriptionen von zugehörigen Aufrufen basiert oder die Namen bestimmter Produkte enthält, die vom Call-Center betreut werden.

Zur Verwendung einer Transkription oder einer Liste von Wörtern erstellen Sie zuerst ein angepasstes Sprachmodell, das diese Textdaten enthält. Damit ein angepasstes Akustikmodell mit einem angepassten Sprachmodell trainiert werden kann, müssen beide Modelle auf derselben Version desselben Basismodells basieren. Falls eine neue Version des Basismodells verfügbar gemacht wird, müssen Sie für beide Modelle ein Upgrade auf dieselbe Version des Basismodells durchführen, damit das Training erfolgreich verläuft.

Verwenden Sie zum Trainieren Ihres angepassten Akustikmodells mit einem angepassten Sprachmodell den Abfrageparameter `custom_language_model_id` der Methode `POST /v1/acoustic_customizations/{customization_id}/train`. Übergeben Sie die GUID des Akustikmodells mit dem Parameter `customization_id` und die GUID des angepassten Sprachmodells mit dem Parameter `custom_language_model_id`. Beide Modelle müssen Eigentum der Serviceberechtigungsnachweise sein, die mit der Anforderung übergeben werden.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train?custom_language_model_id={customization_id}"
```
{: pre}

## Angepasste Sprachmodelle und Akustikmodelle bei der Spracherkennung verwenden
{: #useBothRecognize}

Bei jeder Erkennungsanforderung können Sie sowohl ein angepasstes Sprachmodell als auch ein angepasstes Akustikmodell angeben. Falls ein angepasstes Sprachmodell vokabularexterne Wörter aus dem Fachgebiet der zu erkennenden Audiodaten enthält, können Sie durch die Verwendung dieses Modells zusammen mit einem angepassten Akustikmodell während der Spracherkennung die Genauigkeit der Transkription verbessern.

Die Verwendung eines angepassten Sprachmodells kann die Transkriptionsgenauigkeit unabhängig davon verbessern, ob Sie das angepasste Akustikmodell mit dem angepassten Sprachmodell trainiert haben:

-   Die kombinierte Verwendung eines angepassten Sprachmodells und eines angepassten Akustikmodells während des Trainings verbessert die Qualität des angepassten Akustikmodells.
-   Die Verwendung beider Modelltypen während der Spracherkennung verbessert die Qualität der Transkription.

Falls ein angepasstes Sprachmodell Grammatiken einbezieht, können Sie auch das angepasste Sprachmodell und eine seiner Grammatiken während der Spracherkennung zusammen mit einem angepassten Akustikmodell verwenden.

Im folgenden Beispiel werden beide Modelltypen an die HTTP-Methode `POST /v1/recognize` übergeben. Übergeben Sie die GUID des angepassten Akustikmodells mit dem Parameter `acoustic_customization_id` und die GUID des angepassten Sprachmodells mit dem Parameter `language_customization_id`. Beide Modelle müssen Eigentum der mit der Anforderung übergebenen Serviceberechtigungsnachweise sein und auf demselben Basismodell basieren (z. B. `en-US_BroadbandModel`).

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}&language_customization_id={anpassungs-id}"
```
{: pre}

Bei einer asynchronen HTTP-Anforderung geben Sie die Parameter an, wenn Sie den asynchronen Job erstellen. Bei WebSocket-Anforderungen übergeben Sie die Parameter, wenn Sie die Verbindung herstellen.
