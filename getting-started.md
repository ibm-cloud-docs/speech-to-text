---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-03"

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
{:go: .ph data-hd-programlang='go'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:download: .download}
{:apikey: data-credential-placeholder='apikey'}
{:url: data-credential-placeholder='url'}
{:hide-dashboard: .hide-dashboard}

# Lernprogramm 'Einführung'
{: #gettingStarted}

Der {{site.data.keyword.speechtotextfull}}-Service transkribiert Audiodaten in Text und aktiviert auf diese Weise Sprachtranskriptionsfunktionalität für Anwendungen. Dieses curl-basierte Lernprogramm unterstützt Sie beim schnellen Einstieg in die Verwendung des Service. In den Beispielen wird veranschaulicht, wie Sie die Methode `POST /v1/recognize` des Service aufrufen, um eine Transkription anzufordern.
{: shortdesc}

## Vorbereitende Schritte
{: #before-you-begin}

- {: hide-dashboard}  Erstellen Sie eine Instanz des Service:
    1.  {: hide-dashboard} Rufen Sie die [{{site.data.keyword.speechtotextshort}}](https://{DomainName}/catalog/services/speech-to-text){: external}-Seite im {{site.data.keyword.cloud_notm}}-Katalog auf.
    1.  {: hide-dashboard} Registrieren Sie sich für ein kostenloses {{site.data.keyword.cloud_notm}}-Konto oder melden Sie sich an.
    1.  {: hide-dashboard} Klicken Sie auf **Erstellen**.
-   Kopieren Sie die Berechtigungsnachweise zur Authentifizierung bei Ihrer Serviceinstanz:
    1.  {: hide-dashboard} Klicken Sie in der [{{site.data.keyword.cloud_notm}}-Ressourcenliste](https://{DomainName}/resources){: external} auf Ihre {{site.data.keyword.speechtotextshort}}-Serviceinstanz, um die Dashboardseite des {{site.data.keyword.speechtotextshort}}-Service aufzurufen.
    1.  Klicken Sie auf der Seite **Verwalten** auf **Anzeigen**, um Ihre Berechtigungsnachweise anzuzeigen.
    1.  Kopieren Sie die Werte für den `API-Schlüssel` und die `URL`.

### curl-Beispiele verwenden
{: #getting-started-curl}

In diesem Lernprogramm wird der Befehl `curl` verwendet, um Methoden der HTTP-Schnittstelle des Service aufzurufen. Stellen Sie sicher, dass der Befehl `curl` auf Ihrem System installiert ist.

1.  Um zu testen, ob `curl` installiert ist, führen Sie den folgenden Befehl in der Befehlszeile aus. Wenn in der Ausgabe die `curl`-Version aufgelistet wird, die Secure Sockets Layer (SSL) unterstützt, sind Sie für das Lernprogramm bereit.

    ```bash
    curl -V
    ```
    {: pre}

1.  Falls erforderlich, installieren Sie die Version von `curl` mit aktivierter SSL für Ihr Betriebssystem über [curl.haxx.se](https://curl.haxx.se/){: external}.

Lassen Sie die geschweiften Klammern aus den Beispielen weg. Sie geben Variablenwerte an.
{: tip}

## Schritt 1: Audiodaten ohne Optionen transkribieren
{: #transcribe}

Rufen Sie die Methode `POST /v1/recognize` auf, um eine Basistranskription einer FLAC-Audiodatei ohne zusätzliche Parameter anzufordern.

1.  Laden Sie die Audiobeispieldatei <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a> herunter.
1.  Geben Sie den folgenden Befehl aus, um die Methode `/v1/recognize` des Service für eine Basistranskription ohne Parameter aufzurufen. Beim Beispiel wird der Typ der Audiodaten (`audio/flac`) mit dem Header `Content-Type` angegeben. Das Beispiel verwendet für die Transkription das Standardsprachmodell `en-US_BroadbandModel`.
    -   {: hide-dashboard} Ersetzen Sie die Platzhalter `{apikey}` und `{url}` durch Ihre Werte für den API-Schlüssel und die URL.
    -   Ändern Sie den Wert für `{path_to_file}`, um die Position der Datei `audio-file.flac` anzugeben.

    *Windows-Benutzer* ersetzen den umgekehrten Schrägstrich (``\`) am Ende jeder Zeile mit einem Winkelzeichen (``^`). Stellen Sie sicher, dass keine nachgestellten Leerzeichen vorhanden sind.
    {: tip}

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize"{: url}
    ```
    {: pre}

    Der Service gibt die folgenden Transkriptionsergebnisse zurück:

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.96
              "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

## Schritt 2: Audiodaten mit Optionen transkribieren
{: #transcribeOptions}

Rufen Sie die Methode `POST /v1/recognize` auf, um dieselbe FLAC-Audiodatei zu transkribieren. Geben Sie nun jedoch zwei Transkriptionsparameter an.

1.  Laden Sie bei Bedarf die Audiobeispieldatei <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a> herunter.
1.  Geben Sie den folgenden Befehl aus, um die Methode `/v1/recognize` des Service mit zwei zusätzlichen Parametern aufzurufen. Legen Sie für den Parameter `timestamps` die Einstellung `true` fest, damit der Anfang und das Ende jedes Wortes im Audiodatenstrom angegeben werden. Geben Sie für den Parameter `max_alternatives` den Wert `3` an, um die drei wahrscheinlichsten Alternativen für die Transkription zu erhalten. Beim Beispiel wird der Header `Content-Type` verwendet, um den Typ der Audiodaten anzugeben (`audio/flac`); die Anforderung verwendet das Standardmodell `en-US_BroadbandModel`.
    -   {: hide-dashboard} Ersetzen Sie die Platzhalter `{apikey}` und `{url}` durch Ihre Werte für den API-Schlüssel und die URL.
    -   Ändern Sie den Wert für `{path_to_file}`, um die Position der Datei `audio-file.flac` anzugeben.

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize?timestamps=true&max_alternatives=3"{: url}
    ```
    {: pre}

    Der Service gibt die folgenden Ergebnisse zurück, die Zeitmarken und drei alternative Transkriptionen enthalten:

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "timestamps": [
                ["several":, 1.0, 1.51],
                ["tornadoes":, 1.51, 2.15],
                ["touch":, 2.15, 2.5],
                . . .
              ]
            },
            {
              "confidence": 0.96
              "transcript": "several tornadoes touch down as a line
of severe thunderstorms swept through Colorado on Sunday "
            },
            {
              "transcript": "several tornadoes touched down as a line of
severe thunderstorms swept through Colorado on Sunday "
            },
            {
              "transcript": "several tornadoes touch down as a line
of severe thunderstorms swept through Colorado and Sunday "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

## Nächste Schritte

-   Informieren Sie sich im Abschnitt [Übersicht für Entwickler](/docs/services/speech-to-text?topic=speech-to-text-developerOverview) über die Schnittstellen und SDKs, die zur Ausgabe von Spracherkennungsanforderungen verfügbar sind.
-   Beschäftigen Sie sich mit den Basisanforderungen für die Spracherkennung, die für jede Schnittstelle des Service unter [Erkennungsanforderung ausgeben](/docs/services/speech-to-text?topic=speech-to-text-basic-request) beschrieben sind.
-   Ausführliche Informationen zu allen Methoden der Serviceschnittstellen sind in der [API-Referenz](https://{DomainName}/apidocs/speech-to-text){: external} enthalten.
