---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

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

Zur Authentifizierung verwendet das Lernprogramm API-Schlüssel von {{site.data.keyword.cloud}} Identity and Access Management (IAM). Ältere Serviceinstanzen verwenden für die Authentifizierung unter Umständen weiterhin die Werte für `{username}` (Benutzername) und `{password}` (Kennwort) aus ihren bestehenden Cloud Foundry-Serviceberechtigungsnachweisen. Verwenden Sie für die Authentifizierung die Methode, die für Ihre Serviceinstanz geeignet ist. Weitere Angaben über die Verwendung der IAM-Authentifizierung durch den Service finden Sie unter [Serviceaktualisierung vom 30. Oktober 2018](/docs/services/speech-to-text/release-notes.html#October2018b) in den Releaseinformationen.
{: important}

## Vorbereitende Schritte
{: #before-you-begin}

- {: hide-dashboard}  Erstellen Sie eine Instanz des Service:
    1.  {: hide-dashboard} Rufen Sie die Seite [{{site.data.keyword.speechtotextshort}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/catalog/services/speech-to-text){: new_window} im {{site.data.keyword.cloud_notm}}-Katalog auf.
    1.  {: hide-dashboard} Registrieren Sie sich für ein kostenloses {{site.data.keyword.cloud_notm}}-Konto oder melden Sie sich an.
    1.  {: hide-dashboard} Klicken Sie auf **Erstellen**.
-   Kopieren Sie die Berechtigungsnachweise zur Authentifizierung bei Ihrer Serviceinstanz:
    1.  {: hide-dashboard} Klicken Sie im [{{site.data.keyword.cloud_notm}}-Dashboard ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/dashboard/apps){: new_window} auf Ihre Instanz des {{site.data.keyword.speechtotextshort}}-Service, um die Dashboardseite für den {{site.data.keyword.speechtotextshort}}-Service aufzurufen.
    1.  Klicken Sie auf der Seite **Verwalten** auf **Anzeigen**, um Ihre Berechtigungsnachweise anzuzeigen.
    1.  Kopieren Sie die Werte für den `API-Schlüssel` und die `URL`.
-   Vergewissern Sie sich, dass der Befehl `curl` verfügbar ist.
    -   Die Beispiele verwenden den Befehl `curl`, um  Methoden der HTTP-Schnittstelle aufzurufen. Installieren Sie ausgehend von der Seite [curl.haxx.se ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://curl.haxx.se/){: new_window} die Version für Ihr Betriebssystem. Installieren Sie die Version, die das Protokoll 'Secure Sockets Layer' (SSL) unterstützt. Nehmen Sie unbedingt die installierte Binärdatei in Ihre Umgebungsvariable `PATH` auf.

Ersetzen Sie bei der Eingabe eines Befehls die Platzhalter `{apikey}` und `{url}` durch Ihre tatsächlichen Werte für den API-Schlüssel und die URL. Lassen Sie die geschweiften Klammern, die einen Variablenwert kenntlich machen, dabei weg. Ein tatsächlicher Wert sieht etwa wie im folgenden Beispiel aus:
{: hide-dashboard}

```bash
curl -X POST -u "apikey:L_HALhLVIksh1b73l97LSs6R_3gLo4xkujAaxm7i-b9x"
. . .
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{:pre}
{: hide-dashboard}

## Schritt 1: Audiodaten ohne Optionen transkribieren
{: #transcribe}

Rufen Sie die Methode `POST /v1/recognize` auf, um eine Basistranskription einer FLAC-Audiodatei ohne zusätzliche Parameter anzufordern.

1.  Laden Sie die Audiobeispieldatei <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="Symbol für externen Link" title="Symbol für externen Link"></a> herunter.
1.  Geben Sie den folgenden Befehl aus, um die Methode `/v1/recognize` des Service für eine Basistranskription ohne Parameter aufzurufen. Beim Beispiel wird der Typ der Audiodaten (`audio/flac`) mit dem Header `Content-Type` angegeben. Das Beispiel verwendet für die Transkription das Standardsprachmodell `en-US_BroadbandModel`. 
    -   {: hide-dashboard} Ersetzen Sie die Platzhalter `{apikey}` und `{url}` durch Ihre Werte für den API-Schlüssel und die URL.
    -   Ändern Sie den Wert für `{path_to_file}`, um die Position der Datei `audio-file.flac` anzugeben.

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
              "confidence": 0.87
              "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through colorado on sunday "
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
              "confidence": 0.87
              "transcript": "several tornadoes touch down as a line
of severe thunderstorms swept through colorado on sunday "
            },
            {
              "transcript": "several tornadoes touched down as a line
of severe thunderstorms swept through colorado on sunday "
            },
            {
              "transcript": "several tornadoes touch down is a line
of severe thunderstorms swept through colorado on sunday "
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

-   Informieren Sie sich im Abschnitt [Übersicht für Entwickler](/docs/services/speech-to-text/developer-overview.html) über die Schnittstellen und SDKs, die zur Ausgabe von Spracherkennungsanforderungen verfügbar sind.
-   Beschäftigen Sie sich mit den Basisanforderungen für die Spracherkennung, die für jede Schnittstelle des Service unter [Erkennungsanforderung ausgeben](/docs/services/speech-to-text/basic-request.html) beschrieben sind.
-   Ausführliche Informationen zu allen Methoden der Serviceschnittstellen sind in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/speech-to-text){: new_window} zu finden.
