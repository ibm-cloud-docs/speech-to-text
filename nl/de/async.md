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
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Asynchrone HTTP-Schnittstelle
{: #async}

Die asynchrone HTTP-Schnittstelle des {{site.data.keyword.speechtotextfull}}-Service bietet Methoden, mit denen Audiodaten über nicht blockierende Aufrufe des Service transkribiert werden können. Die Schnittstelle stellt durch den Einsatz von benutzerdefinierten geheimen Zeichenfolgen und digitalen Signaturen eine Sicherheitsstufe für Anforderungen bereit, die über das HTTP-Protokoll erfolgen. Für die Verwendung der asynchronen Schnittstelle haben Sie zwei Möglichkeiten.
{: shortdesc}

-   Registrieren Sie eine Callback-URL, damit Sie vom Service automatisch über den Jobstatus und die Ergebnisse benachrichtigt werden.
-   Fragen Sie den Service ab, um den Jobstatus und die Ergebnisse manuell abzurufen.

Die beiden Methoden schließen sich nicht gegenseitig aus. Sie können sich für den Empfang von Callback-Benachrichtigungen entscheiden und trotzdem den neuesten Status vom Service abfragen oder Ergebnisse manuell vom Service abrufen. In den folgenden Abschnitten ist beschrieben, wie Sie die asynchrone HTTP-Schnittstelle bei beiden Methoden verwenden.

Übergeben Sie mit einer einzelnen Anforderung mindestens 100 Byte und höchstens 1 GB Audiodaten. Angaben über Audioformate und die Verwendung der Komprimierung zur Maximierung der Audiodatenmenge, die mit einer Anforderung gesendet werden kann, enthält der Abschnitt [Audioformate](/docs/services/speech-to-text/audio-formats.html). Weitere Informationen zu den einzelnen Methoden der Schnittstelle finden Sie in der [API-Referenz. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/speech-to-text){: new_window}

## Verwendungsmodelle
{: #usage}

Bei der Arbeit mit der asynchronen HTTP-Schnittstelle des Service haben Sie die folgenden Möglichkeiten, um sich über den Jobstatus zu informieren und Ergebnisse zu empfangen:

-   Verwendung von Callback-Benachrichtigungen:
    1.  Rufen Sie die Methode `POST /v1/register_callback` auf, um eine Callback-URL beim Service zu registrieren. Sie können optional eine benutzerdefinierte geheime Zeichenfolge bereitstellen, um die Authentifizierung und die Datenintegrität für Callbacks zu aktivieren, die an die URL gesendet werden.
    1.  Rufen Sie die Methode `POST /v1/recognitions` mit einer bereits registrierten Callback-URL auf, an die der Service Benachrichtigungen sendet, wenn sich der Status des Jobs ändert. Sie geben eine Liste der Ereignisse an, über die Sie benachrichtigt werden wollen. Standardmäßig sendet der Service Benachrichtigungen, wenn ein Job gestartet wird, wenn ein Job abgeschlossen ist und wenn ein Fehler auftritt. Sie können anfordern, dass in der Abschlussbenachrichtigung auch die Ergebnisse der Anforderung gesendet werden. Andernfalls müssen Sie die Ergebnisse mit der Methode `GET /v1/recognitions/{id}` abrufen.
-   Abfrage des Service:
    1.  Rufen Sie die Methode `POST /v1/recognitions` ohne Callback-URL, Ereignisse oder Benutzertoken auf.
    1.  Rufen Sie in regelmäßigen Abständen die Methode `GET /v1/recognitions` auf, um den Status der neuesten Jobs zu überprüfen, oder rufen Sie die Methode `GET /v1/recognitions/{id}` auf, um den Status eines bestimmten Jobs zu überprüfen.
    1.  Falls Sie den Jobstatus mit der Methode `GET /v1/recognitions` überprüfen, rufen Sie die Methode `GET /v1/recognitions/{id}` auf, um nach Abschluss des Jobs die Ergebnisse abzurufen.

Die beiden Verfahren können kombiniert verwendet werden. Für einen Job, der mit einer Callback-URL erstellt wurde, können Sie trotzdem den neuesten Status beim Service abfragen. Es kann beispielsweise vorkommen, dass Sie den Status eines Jobs abrufen wollen, wenn Benachrichtigungen erst nach einer Weile eintreffen. Eine Überprüfung des Status kann außerdem sinnvoll sein, wenn Sie vermuten, dass Sie aufgrund eines Service- oder Netzfehlers eine oder mehrere Benachrichtigungen nicht erhalten haben.

## Callback-URL registrieren
{: #register}

Zum Registrieren einer Callback-URL rufen Sie die Methode `POST /v1/register_callback` auf. Sobald Sie eine Callback-URL registriert haben, können Sie sie für den Empfang von Benachrichtigungen über eine unbegrenzte Anzahl von Jobs verwenden. Der Registrierungsprozess umfasst vier Schritte:

1.  Rufen Sie die Methode `POST /v1/register_callback` auf und übergeben Sie eine Callback-URL. Optional können Sie auch einen benutzerdefinierten geheimen Schlüssel angeben. Der Service verwendet den geheimen Schlüssel für die Berechnung der Signaturen gemäß Secure Hash Algorithm 1 (SHA1) von Keyed-Hash Message Authentication Code (HMAC), die für die Authentifizierung und Datenintegrität eingesetzt werden. Im folgenden Beispiel wird ein Callback für einen Benutzer registriert, der an der URL `http://{user_callback_path}/results` antwortet. Der Aufruf enthält den geheimen Benutzerschlüssel `ThisIsMySecret`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/register_callback?callback_url=http://{user_callback_path}/results&user_secret=ThisIsMySecret"
    ```
    {: pre}

1.  Der Service versucht, die Callback-URL zu validieren (oder der Whitelist zuzuordnen), falls diese noch nicht registriert wurde. Hierzu sendet er eine Anforderung `GET` an die Callback-URL. Der Service übergibt eine zufällige alphanumerische Abfragezeichenfolge über den Abfrageparameter `challenge_string` der Anforderung. Die Anforderung enthält einen Header `Accept`, der `text/plain` als erforderlichen Antworttyp angibt.

    Falls der Aufruf der Methode `register_callback` einen geheimen Benutzerschlüssel enthalten hat, enthält die Anforderung `GET` aus dem Service auch einen Header `X-Callback-Signature`, der die HMAC-SHA1-Signatur der Abfragezeichenfolge angibt. Der Service berechnet die Signatur und verwendet hierzu den geheimen Benutzerschlüssel als Schlüssel.

    ```
    GET http://{user_callback_path}/results?challenge_string=n9ArPGMQ36Hiu7QC
    header: X-Callback-Signature {HMAC-SHA1_signature}
    ```
    {: codeblock}

1.  Sie antworten auf die Anforderung `GET` aus dem Service mit dem Statuscode 200. Beziehen Sie die vom Service gesendete Abfragezeichenfolge in die Antwort ein. Nehmen Sie die Zeichenfolge als unverschlüsselten Text in den Hauptteil der Antwort auf und legen Sie für den Antwortheader `Content-Type` den Wert `text/plain` fest.

    Falls die ursprüngliche Anforderung `POST` einen geheimen Benutzerschlüssel enthalten hat, können Sie eine HMAC-SHA1-Signatur der Abfragezeichenfolge berechnen, indem Sie den geheimen Schlüssel als Schlüssel verwenden. Sofern die Anforderung `GET` durch den Service gesendet wurde, stimmt die Signatur mit dem Wert überein, der im Header `X-Callback-Signature` angegeben ist.

    ```
    response code: 200 OK
    body: n9ArPGMQ36Hiu7QC
    ```
    {: codeblock}

1.  Der Service überprüft, ob die Abfragezeichenfolge im Hauptteil der Antwort auf seine Anforderung `GET` zurückgegeben wurde. Ist dies der Fall, listet der Service die Callback-URL auf und antwortet auf Ihre ursprüngliche Anforderung `POST` mit dem Statuscode 201. Der Hauptteil der Antwort enthält ein JSON-Objekt mit einem Feld `status`, das den Wert `created` hat, und einem Feld `url`, das den Wert Ihrer Callback-URL enthält.

    ```
    response code: 201 Created
    body: {
      "status": "created",
      "url": "http://{user_callback_path}/results"
    }
    ```
    {: codeblock}

Der Service sendet während des Registrierungsprozesses nur eine einzige Anforderung `GET` an eine Callback-URL. Der Service muss innerhalb von 5 Sekunden eine Antwort mit dem Antwortcode 200 empfangen, deren Hauptteil die Abfragezeichenfolge enthält. Andernfalls wird die URL nicht in die Whitelist aufgenommen, sondern stattdessen als Antwort auf die Anforderung `POST /v1/register_callback` der Statuscode 400 gesendet. Falls die Callback-URL bereits erfolgreich in die Whitelist aufgenommen wurde, sendet der Service als Antwort auf die ursprüngliche Anforderung `POST` den Statuscode 200.

### Sicherheitsaspekte
{: #security-async}

Wenn Sie eine Callback-URL mit der Methode `POST /v1/register_callback` erfolgreich registriert haben, nimmt der Service die URL in die Whitelist auf und macht dadurch kenntlich, dass die URL zur Verwendung bei Callback-Benachrichtigung verifiziert ist. Falls Sie mit dem Registrierungsaufruf einen geheimen Benutzerschlüssel angeben, bedeutet die Aufnahme in die Whitelist außerdem, dass die URL für die erweiterte Sicherheit validiert wurde. Die Angabe eines geheimen Benutzerschlüssels bietet Authentifizierung und Datenintegrität für Anforderungen, die die Callback-URL in Verbindung mit der asynchronen HTTP-Schnittstelle verwenden.

Der Service berechnet mithilfe des geheimen Benutzerschlüssels eine HMAC-SHA1-Signatur für die Nutzdaten aller Callback-Benachrichtigungen, die er an die URL sendet. Der Service sendet die Signatur zusammen mit jeder Benachrichtigung im Header `X-Callback-Signature`. Der Client kann mithilfe des geheimen Schlüssels seine eigene Signatur für die Nutzdaten jeder Benachrichtigung berechnen. Falls seine Signatur mit dem Wert des Headers `X-Callback-Signature` übereinstimmt, weiß der Client, dass die Benachrichtigung durch den Service gesendet und ihr Inhalt während der Übertragung nicht geändert wurde. Dieses Wissen garantiert, dass der Client nicht Opfer eines Angriffs geworden ist, der als 'Man-in-the-Middle' (MITM) oder 'Janus-Angriff' bezeichnet wird.

HTTPS ist für Produktionsanwendungen die ideale Lösung. Während der Anwendungsentwicklung und Prototyperstellung können die vom Service unterstützten  HTTP-basierten Callback-Benachrichtigungen jedoch den Entwicklungsprozess vereinfachen und beschleunigen, weil der mit HTTPS verbundene Aufwand vermieden wird.

<!--

However, communicating over the HTTPS protocol is always the most secure means of learning job status and retrieving results. Using the HTTPS `GET /v1/recognitions/{id}` method to retrieve the results of a job is therefore more secure that receiving the results via callback notification. While the use of HMAC-SHA1 signatures based on a user secret ensures authentication and data integrity for callback notifications, it does not provide confidentiality. Conversely, because it encrypts the body of the response, HTTPS can provide authentication, integrity, *and* confidentiality.

HTTPS, however, is not ideal in terms of additional development overhead. Moreover, although the service validates SSL certificates to prevent man-in-the-middle attacks when you use HTTPS, validation is not foolproof if you use self-signed certificates, which enable encryption but not authentication or data integrity. During application development, user secrets and digital signatures provide a suitable level of security for requests made over the HTTP protocol. You can work with HTTP callback notifications during prototyping and move to HTTPS only for application deployment.

-->

### Registrierung einer Callback-URL zurücknehmen
{: #unregister}

Sie können die Registrierung einer Callback-URL, die in der Whitelist aufgeführt ist, jederzeit mit der Methode `POST /v1/unregister_callback` aufheben. Das Aufheben der Registrierung einer Callback-URL kann nützlich sein, wenn Sie Ihre Anwendung mit dem Service testen. Nachdem Sie die Registrierung einer Callback-URL aufgehoben haben, können Sie die URL nicht mehr bei asynchronen Erkennungsanforderungen verwenden.

## Job erstellen
{: #create}

Zum Erstellen eines Erkennungsjobs rufen Sie die Methode `POST /v1/recognitions` auf. Auf welche Weise Sie den Status des Jobs und die Ergebnisse erfahren, variiert abhängig von der verwendeten Strategie und den übergebenen Parametern:

-   Zur *Verwendung von Callback-Benachrichtigungen* beziehen Sie den Abfrageparameter `callback_url` ein und geben Sie eine URL an, an die der Service Callback-Benachrichtigungen senden soll, sobald sich der Status des Jobs ändert. Sie können außerdem die folgenden optionalen Abfrageparameter angeben:
    -   Geben Sie den Parameter `events` ein, um eine Liste von Benachrichtigungsereignissen zu abonnieren. Standardmäßig sendet der Service Callback-Benachrichtigungen, wenn der Job gestartet wird (Ereignis `recognitions.started`), wenn der Job abgeschlossen ist (Ereignis `recognitions.completed`) und falls ein Fehler auftritt (Ereignis `recognitions.failed`). Sie können eine Teilmenge der Ereignisse angeben oder das Ereignis `recognitions.completed_with_results` anstelle des Ereignisses `recognitions.completed` verwenden, damit die Ergebnisse zusammen mit der Benachrichtigung über den Jobabschluss gesendet werden.
    -   Geben Sie mit dem Parameter `user_token` eine Zeichenfolge an, die in jeder Benachrichtigung über den Job enthalten sein soll. Da Sie dieselbe Callback-URL bei einer unbegrenzten Anzahl von Jobs verwenden können, ermöglicht die Verwendung von Benutzertokens eine Unterscheidung zwischen den Benachrichtigungen für verschiedene Jobs.
-   Zur *Verwendung von Abfragen* lassen Sie die Abfrageparameter `callback_url`, `events` und `user_token` weg. Anschließend müssen Sie die Methoden `GET /v1/recognitions` oder `GET /v1/recognitions/{id}` verwenden, um den Status des Jobs zu überprüfen; mit der zweiten Methode werden die Ergebnisse nach dem Abschluss eines Jobs abgerufen.

In beiden Fällen können Sie durch das Hinzufügen des Abfrageparameters `results_ttl` angeben, wie viele Minuten die Ergebnisse nach dem Abschluss des Jobs verfügbar bleiben sollen.

Neben den obigen Parametern, die speziell für die asynchrone Schnittstelle gelten, unterstützt die Methode `POST /v1/recognitions` einen Großteil derselben Parameter wie die WebSocket-Schnittstelle oder die synchrone HTTP-Schnittstelle. Weitere Informationen enthält der Abschnitt [Parameterübersicht](/docs/services/speech-to-text/summary.html).

### Callback-Benachrichtigungen
{: #notifications}

Falls der Job mit einer Callback-URL erstellt wird, sendet der Service eine HTTP-Callback-Benachrichtigung `POST` an die registrierte URL, sobald ein bestimmtes Ereignis auftritt. Der Hauptteil einer Basisbenachrichtigung besteht aus einem JSON-Objekt mit der folgenden Struktur:

```javascript
{
  "id": "{job-id}",
  "event": "{erkennungsstatus}",
  "user_token": "{benutzertoken}"
}
```
{: codeblock}

Das Feld `id` gibt die ID des Jobs an, der den Callback generiert hat, und das Feld `event` gibt das Ereignis an, das den Callback ausgelöst hat. Das Feld `user_token` enthält das Benutzertoken für den Job, sofern ein Token angegeben wurde. Andernfalls besteht der Inhalt dieses Feldes aus einer leeren Zeichenfolge. Beim Ereignis `recognitions.completed_with_results` enthält das Objekt ein Feld `results`, in dem die Ergebnisse der Erkennungsanforderung zur Verfügung gestellt werden. Der Client kann auf die Callback-Benachrichtigung mit dem Statuscode 200 antworten.

Falls zusammen mit der Callback-URL ein geheimer Benutzerschlüssel registriert wurde, sendet der Service mit der Callback-Benachrichtigung außerdem den Header `X-Callback-Signature`. Im Header ist die HMAC-SHA1-Signatur für den Hauptteil der Anforderung angegeben. Der Service berechnet die Signatur und verwendet hierzu den geheimen Benutzerschlüssel als Schlüssel. Der Client kann die Signatur für die Nutzdaten der Callback-Benachrichtigung berechnen und auf diese Weise sicherstellen, dass sie mit der Signatur im Header übereinstimmt. Der folgende einfache Python-Code berechnet beispielsweise die Signatur für die Zeichenfolge `notification_payload`:

```python
from hashlib import sha1
import hmac
hashed = hmac.new(user_secret, notification_payload, sha1)
signature = hashed.digest().encode("base64").rstrip('\n')
```
{: codeblock}

### Beispiel für Callback
{: #callback}

Im folgenden Beispiel wird ein Job erstellt, der der Callback-URL `http://{user_callback_path}/results` zugeordnet ist, die zuvor in die Whitelist aufgenommen wurde. Im Beispiel wird der Job in den Callback-Benachrichtigungen, die vom Service gesendet werden, durch das Benutzertoken `job25` gekennzeichnet. Der Aufruf verwendet die Standardereignisse; der Benutzer muss daher die Methode `GET /v1/recognitions/{id}` aufrufen, um die Ergebnisse abzurufen, wenn der Service eine Callback-Benachrichtigung über den Abschluss des Jobs sendet. Der Aufruf legt für den Abfrageparameter `timestamps` der Erkennungsanforderung die Einstellung `true` fest.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?callback_url=http://{user_callback_path}/results&user_token=job25&timestamps=true"
```
{: pre}

Der Service gibt den Status der Anforderung zurück. Er lautet `waiting` und gibt an, dass der Service den Job für die Verarbeitung vorbereitet. Die Antwort enthält die Erstellungszeit, die Job-ID und die URL, über die weitere Informationen zum Job abgerufen werden können.

```javascript
{
  "created": "2016-08-17T19:15:17.926Z",
  "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bd734c0-e575-21f3-de03-f932aa0468a0",
  "status": "waiting"
}
```
{: codeblock}

### Beispiel für Abfrage
{: #polling}

Im folgenden Beispiel wird ein Job erstellt, dem keine Callback-URL zugeordnet ist. Der Benutzer muss den Service abfragen, um sich über den Abschluss des Jobs zu informieren, und anschließend die Ergebnisse mit der Methode `GET /v1/recognitions/{id}` abrufen. Wie beim vorherigen Beispiel legt der Aufruf für den Parameter `timestamps` der Erkennungsanforderung den Wert `true` fest.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?timestamps=true"
```
{: pre}

Der Service gibt den Status `processing` zurück, um kenntlich zu machen, dass er den Job bereits verarbeitet. Außerdem gibt er die Erstellungszeit, die Job-ID und die URL für den Abruf von Informationen zum Job zurück.

```javascript
{
  "created": "2016-08-17T19:13:23.622Z",
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "status": "processing"
}
```
{: codeblock}

## Status eines Jobs überprüfen und Jobergebnisse abrufen
{: #job}

Durch einen Aufruf der Methode `GET /v1/recognitions/{id}` überprüfen Sie den Status des Jobs, der durch den Pfadparameter `id` angegeben wird. Die Antwort enthält immer die ID und den Status des Jobs sowie dessen Erstellungs- und Aktualisierungszeit. Falls der Status `completed` (= abgeschlossen) lautet, enthält die Antwort auch die Ergebnisse der Erkennungsanforderung.

In den folgenden Fällen ist die Methode `GET /v1/recognitions/{id}` die einzige Möglichkeit zum Abrufen von Jobergebnissen:

-   Der Job wurde ohne Callback-URL übergeben.
-   Der Job wurde mit einer Callback-URL, jedoch ohne Angabe des Ereignisses `recognitions.completed_with_results` übergeben.
-   Der Job ist keiner der 100 letzten ausstehenden Jobs. Wenn Sie den Pfadparameter `id` weglassen, werden nur die 100 letzten Jobs zurückgegeben.

Mit der Methode können Sie jedoch immer auch die Ergebnisse für einen Job abrufen, der eine Callback-URL und das Ereignis `recognitions.completed_with_results` angegeben hat. Sie können die Ergebnisse für einen beliebigen Job so häufig wie gewünscht abrufen, während die Ergebnisse verfügbar bleiben. Ein Job und seine Ergebnisse bleiben verfügbar, bis Sie sie mit der Methode `DELETE /v1/recognitions/{id}` löschen oder bis die Lebensdauer des Jobs abläuft (je nachdem, was zuerst eintritt). Standardmäßig verfallen Ergebnisse nach einer Woche, sofern Sie mit dem Parameter `results_ttl` der Methode `POST /v1/recognitions` keine andere Lebensdauer angegeben haben.

### Beispiel für Status ohne Ergebnisse
{: #withoutResults}

Im folgenden Beispiel wird der Status des Jobs mit der angegebenen ID überprüft. Der Job ist noch nicht abgeschlossen, so dass die Antwort die Ergebnisse nicht enthält.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job-id}"
```
{: pre}

```javascript
{
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "created": "2016-08-17T19:13:23.622Z",
  "updated": "2016-08-17T19:13:24.434Z",
  "status": "processing"
}
```
{: codeblock}

### Beispiel für Status mit Ergebnissen
{: #withResults}

Im folgenden Beispiel wird der Status des Jobs mit der angegebenen ID angefordert. Der Job ist abgeschlossen, so dass die Antwort die Ergebnisse der Anforderung enthält.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job-id}"
```
{: pre}

```javascript
{
  "id": "398fcd80-330a-22ba-93ce-1a73f454dd98",
  "results": [
    {
      "result_index": 0,
      "results": [
        {
          "final": true,
          "alternatives": [
            {
              "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday ",
              "timestamps": [
                [
                  "several",
                  1,
                  1.52
                ],
                [
                  "tornadoes",
                  1.52,
                  2.15
                ],
                . . .
                [
                  "Sunday",
                  5.74,
                  6.33
                ]
              ],
              "confidence": 0.89
            }
          ]
        }
      ]
    }
  ],
  "created": "2016-08-17T19:11:04.298Z",
  "updated": "2016-08-17T19:11:16.003Z",
  "status": "completed"
}
```
{: codeblock}

## Status der letzten Jobs überprüfen
{: #jobs}

Durch einen Aufruf der Methode `GET /v1/recognitions` können Sie den Status der letzten Jobs überprüfen. Die Methode gibt den Status der letzten 100 ausstehenden Jobs zurück, die den Serviceberechtigungsnachweisen zugeordnet sind, mit denen die Methode aufgerufen wird. Die Methode gibt die ID und den Status jedes Jobs zusammen mit seiner Erstellungs- und Aktualisierungszeit zurück. Wenn ein Job mit einer Callback-URL und einem Benutzertoken erstellt wurde, gibt die Methode auch das Benutzertoken für den Job zurück.

Die Antwort enthält einen der folgenden Status:

-   `waiting`: Der Service bereitet den Job für die Verarbeitung vor. Dieser Status ist der Anfangsstatus aller Jobs. Der Job bleibt in diesem Status, bis der Service über die Kapazität für den Beginn der Verarbeitung verfügt.
-   `processing`: Der Service verarbeitet den Job aktiv.
-   `completed`: Der Service hat die Verarbeitung des Jobs fertiggestellt. Falls der Job eine Callback-URL und das Ereignis `recognitions.completed_with_results` angegeben hat, sendet der Service zusammen mit der Callback-Benachrichtigung die Ergebnisse. Andernfalls müssen Sie die Ergebnisse mit der Methode `GET /v1/recognitions/{id}` abrufen.
-   `failed`: Der Job ist aus einem nicht genauer bezeichneten Grund fehlgeschlagen.

Ein Job und seine Ergebnisse bleiben verfügbar, bis Sie sie mit der Methode `DELETE /v1/recognitions/{id}` löschen oder bis die Lebensdauer des Jobs abläuft (je nachdem, was zuerst eintritt).

### Beispiel
{: #statusExample}

Im folgenden Beispiel wird der Status der letzten aktuellen Jobs angefordert, die den Serviceberechtigungsnachweisen des aufrufenden Benutzers zugeordnet sind. Für den Benutzer gibt es drei ausstehende Jobs mit verschiedenen Statuswerten. Der erste Job wurde mit einer Callback-URL und einem Benutzertoken erstellt.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions"
```
{: pre}

```javascript
{
  "recognitions": [
    {
      "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
      "created": "2016-08-17T19:15:17.926Z",
      "updated": "2016-08-17T19:15:17.926Z",
      "status": "waiting",
      "user_token": "job25"
    },
    {
      "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
      "created": "2016-08-17T19:13:23.622Z",
      "updated": "2016-08-17T19:13:24.434Z",
      "status": "processing"
    },
    {
      "id": "398fcd80-330a-22ba-93ce-1a73f454dd98",
      "created": "2016-08-17T19:11:04.298Z",
      "updated": "2016-08-17T19:11:16.003Z",
      "status": "completed"
    }
  ]
}
```
{: codeblock}

## Job löschen
{: #delete-async}

Mit der Methode `DELETE /v1/recognitions/{id}` können Sie den Job löschen, der im Pfadparameter `id` angegeben ist. Normalerweise löschen Sie einen Job, nachdem Sie seine Ergebnisse aus dem Service abgerufen haben. Sobald Sie einen Job gelöscht haben, sind seine Ergebnisse nicht mehr verfügbar. Ein Job, den der Service aktiv verarbeitet, kann nicht gelöscht werden.

Standardmäßig bewahrt der Service die Ergebnisse jedes Jobs auf, bis die Lebensdauer des Jobs abläuft. Die Standardlebensdauer beträgt eine Woche. Mit dem Parameter `results_ttl` der Methode `POST /v1/recognitions` können Sie jedoch angeben, wie viele Minuten der Service die Ergebnisse aufbewahren soll.

### Beispiel
{: #deleteExample-async}

Im folgenden Beispiel wird der Job mit der angegebenen ID gelöscht:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job-id}"
```
{: pre}
