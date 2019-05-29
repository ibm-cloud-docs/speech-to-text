---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-15"

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

# WebSocket-Schnittstelle
{: #websockets}

Die WebSocket-Schnittstelle des {{site.data.keyword.speechtotextshort}}-Service ist das am besten geeignete Hilfsmittel für die Interaktion zwischen einem Client und dem Service. Wenn Sie die WebSocket-Schnittstelle für die Spracherkennung nutzen möchten, erstellen Sie zunächst mit der Methode `/v1/recognize` eine persistente Verbindung zu dem Service. Senden Sie anschließend Text- und Binärnachrichten über die Verbindung, um die Erkennungsanforderungen einzuleiten und zu verwalten.
{: shortdesc}

Der Prozess für Erkennungsanforderungen und Antworten umfasst die folgenden Schritte:

1.  [Verbindung öffnen](#WSopen)
1.  [Erkennungsanforderung einleiten](#WSstart)
1.  [Audiodaten übertragen und Erkennungsergebnisse empfangen](#WSaudio)
1.  [Erkennungsanforderung beenden](#WSstop)
1.  [Weitere Anforderungen senden und Anforderungsparameter ändern](#WSmore)
1.  [Verbindung offen halten](#WSkeep)
1.  [Verbindung schließen](#WSclose)

Wenn der Client Daten an den Service sendet, *müssen* alle JSON-Nachrichten als Textnachrichten und alle Audiodaten als binäre Nachrichten übergeben werden.

Die folgenden Snippets mit Beispielcode wurden in JavaScript geschrieben und basieren auf der HTML5-WebSocket-API. Weitere Informationen zum WebSocket-Protokoll finden Sie unter Internet Engineering Task Force (IETF) [Request for Comment (RFC) 6455 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://tools.ietf.org/html/rfc6455){: new_window}.
{: note}

## Verbindung öffnen
{: #WSopen}

Der {{site.data.keyword.speechtotextshort}}-Service verwendet das WSS-Protokoll (WSS = WebSocket Secure), um die Methode `/v1/recognize` am folgenden Endpunkt zur Verfügung zu stellen:

```
wss://{host_name}/speech-to-text/api/v1/recognize
```
{: codeblock}

Dabei ist `{host_name}` der Standort, an dem Ihre Anwendung gehostet wird:

-   `stream.watsonplatform.net` für Dallas (in den folgenden Beispielen wird dieser Hostname verwendet)
-   `stream-fra.watsonplatform.net` für Frankfurt
-   `gateway-syd.watsonplatform.net` für Sydney
-   `gateway-wdc.watsonplatform.net` für Washington DC
-   `gateway-tok.watsonplatform.net` für Tokio
-   `gateway-lon.watsonplatform.net` für London

Ein WebSocket-Client ruft diese Methoden mit den folgenden Abfrageparametern auf, um eine authentifizierte Verbindung zum Service herzustellen. Bei Verwendung der Authentifizierung mit Identity and Access Management (IAM) verwenden Sie den Abfrageparameter `access_token`. Bei Verwendung von Cloud Foundry-Serviceberechtigungsnachweisen verwenden Sie den Abfrageparameter `watson-token`.

<table>
  <caption>Tabelle 1. Parameter der Methode <code>/v1/recognize</code></caption>
  <tr>
    <th style="width:23%; text-align:left">Parameter</th>
    <th style="width:12%; text-align:center">Datentyp</th>
    <th style="text-align:left">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>access_token</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      <em>Wenn Sie die IAM-Authentifizierung verwenden,</em> übergeben Sie ein gültiges
      IAM-Zugriffstoken, um eine authentifizierte Verbindung zum Service herzustellen.
      Das IAM-Zugriffstoken wird in dem Aufruf anstelle eines API-Schlüssels
      übergeben. Sie müssen die Verbindung herstellen, bevor das Zugriffstoken
      abläuft. Informationen zum Anfordern eines Zugriffstokens finden Sie im Abschnitt
      [Authentifizierung mit IAM-Tokens durchführen](/docs/services/watson/getting-started-iam.html).<br/><br/>
      Ein Zugriffstoken muss nur übergeben werden, um eine authentifizierte Verbindung herzustellen.
      Nach dem Einrichten kann die Verbindung unbegrenzt offen gehalten werden.
      Sie bleiben authentifiziert, solange die Verbindung geöffnet ist.
      Das Zugriffstoken für eine Verbindung, die über die Tokenablaufzeit
      hinaus aktiv bleibt, muss nicht aktualisiert werden.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>watson-token</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      <em>Wenn Sie Cloud Foundry-Serviceberechtigungsnachweise verwenden,</em> übergeben Sie
      ein gültiges {{site.data.keyword.watson}}-Authentifizierungstoken, um eine
      authentifizierte Verbindung zum Service herzustellen. Das {{site.data.keyword.watson}}-Token
      wird in dem Aufruf anstelle von Serviceberechtigungsnachweisen übergeben. {{site.data.keyword.watson}}-Tokens
      basieren auf Cloud Foundry-Serviceberechtigungsnachweisen, die eine Kombination aus `benutzername`
      und `kennwort` für die HTTP-Basisauthentifizierung verwenden. Informationen zum Anfordern
      eines {{site.data.keyword.watson}}-Tokens finden Sie im Abschnitt
      [{{site.data.keyword.watson}}-Tokens](/docs/services/watson/getting-started-tokens.html).<br/><br/>
      Ein {{site.data.keyword.watson}}-Token muss nur übergeben werden, um eine
      authentifizierte Verbindung herzustellen. Nach dem Einrichten kann die Verbindung unbegrenzt offen gehalten werden.
      Sie bleiben authentifiziert, solange die Verbindung geöffnet ist.
      </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>model</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Gibt das Sprachmodell an, das für die Transkription verwendet werden soll.
      Wenn Sie kein Modell angeben, verwendet der Service standardmäßig
      das Modell <code>en-US_BroadbandModel</code>. Weitere Informationen
      finden Sie im Abschnitt
      [Sprachen und Modelle](/docs/services/speech-to-text/models.html).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>language_customization_id</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Gibt die GUID (Globally Unique Identifier) eines angepassten
      Sprachmodells an, das für alle Anforderungen verwendet werden soll,
      die über die Verbindung gesendet werden. Das Basismodell des angepassten
      Sprachmodells muss mit dem Wert des Parameters <code>model</code> übereinstimmen. Standardmäßig
      wird kein angepasstes Sprachmodell verwendet. Weitere Informationen finden Sie im Abschnitt
      [Die Anpassungsschnittstelle](/docs/services/speech-to-text/custom.html).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>acoustic_customization_id</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Gibt die GUID (Globally Unique Identifier) eines angepassten Akustikmodells
      an, das für alle Anforderungen verwendet werden soll, die über die Verbindung
      gesendet werden. Das Basismodell des angepassten Akustikmodells muss mit
      dem Wert des Parameters <code>model</code> übereinstimmen. Standardmäßig
      wird kein angepasstes Akustikmodell verwendet. Weitere Informationen finden Sie im Abschnitt
      [Die Anpassungsschnittstelle](/docs/services/speech-to-text/custom.html).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>base_model_version</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Gibt die Version des Basismodells (`model`) an, die für alle
      Anforderungen verwendet werden soll, die über die Verbindung gesendet werden. Dieser Parameter
      ist in erster Linie für die Verwendung mit angepassten Modellen bestimmt, für die ein Upgrade
      auf ein neues Basismodell durchgeführt wird. Der Standardwert ist davon abhängig, ob der
      Parameter mit oder ohne ein angepasstes Modell verwendet wird. Weitere Informationen finden
      Sie im Abschnitt [Version des Basismodells](/docs/services/speech-to-text/input.html#version).</td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-learning-opt-out</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">Boolesch</td>
    <td style="text-align:left">
      Gibt an, ob der Service Anforderungen und Ergebnisse protokolliert, die über
      die Verbindung gesendet werden. Wenn Sie nicht zulassen möchten, dass Ihre Daten
      von IBM für die allgemeine Verbesserung des Service verwendet werden, geben
      Sie für diesen Parameter <code>true</code> an. Weitere Informationen finden Sie im
      Abschnitt [Anforderungsprotokollierung](/docs/services/speech-to-text/input.html#logging).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-metadata</code>
      <br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Ordnet allen Daten die über die Verbindung gesendet werden,
      eine Kunden-ID zu. Der Parameter akzeptiert das Argument
      <code>customer_id={id}</code> (dabei ist <code>id</code> eine
      zufällige oder generische Zeichenfolge, die den Daten zugeordnet werden soll). Das
      Argument für den Parameter muss URL-codiert sein (z. B.
      `customer_id%3dmy_customer_ID`). Standardmäßig
      wird den Daten keine Kunden-ID zugeordnet. Weitere Informationen finden
      Sie im Abschnitt [Informationssicherheit](/docs/services/speech-to-text/information-security.html).</td>
  </tr>
</table>

Das folgende Snippet mit JavaScript-Code öffnet eine Verbindung zu dem Service. Der Aufruf der Methode `/v1/recognize` übergibt die Abfrageparameter `access_token` und `model`, wobei der zweite Parameter den Service anweist, das Breitbandmodell für Spanisch zu verwenden. Nach dem Herstellen der Verbindung definiert der Client die Ereignislistener (`onOpen`, `onClose` usw.), um auf Ereignisse vom Service zu antworten. Der Client kann die Verbindung für mehrere Erkennungsanforderungen verwenden.

```javascript
var IAM_access_token = '{access_token}';
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token
  + '&model=es-ES_BroadbandModel';
var websocket = new WebSocket(wsURI);
websocket.onopen = function(evt) { onOpen(evt) };
websocket.onclose = function(evt) { onClose(evt) };
websocket.onmessage = function(evt) { onMessage(evt) };
websocket.onerror = function(evt) { onError(evt) };
```
{: codeblock}

## Erkennungsanforderung einleiten
{: #WSstart}

Um eine Erkennungsanforderung einzuleiten, sendet der Client über die bestehende Verbindung eine JSON-Textnachricht an den Service. Der Client muss diese Nachricht senden, bevor Audiodaten zum Transkribieren gesendet werden. Die Nachricht muss den Parameter `action` enthalten. Der Parameter `content-type` kann in der Regel weggelassen werden.

<table>
  <caption>Tabelle 2. Parameter der JSON-Textnachricht</caption>
  <tr>
    <th style="width:23%; text-align:left">Parameter</th>
    <th style="width:12%; text-align:center">Datentyp</th>
    <th style="text-align:left">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>action</code><br/><em>Erforderlich</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Gibt die Aktion an, die ausgeführt werden soll:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          <code>start</code> startet eine Erkennungsanforderung oder gibt
          neue Parameter für nachfolgende Anforderungen an. Weitere Informationen
          finden Sie im Abschnitt [Zusätzliche Anforderungen senden und Anforderungsparameter ändern](#WSmore).
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>stop</code> gibt an, dass alle Audiodaten für eine Anforderung
          gesendet wurden. Weitere Informationen finden Sie im Abschnitt
          [Erkennungsanforderung beenden](#WSstop).
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>content-type</code><br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      Gibt das Format (MIME-Typ) der Audiodaten für die Anforderung an.
      Dieser Parameter ist für die Formate `audio/alaw`, `audio/basic`,
      `audio/l16` und `audio/mulaw` erforderlich. Weitere
      Informationen finden Sie im Abschnitt [Audioformate](/docs/services/speech-to-text/audio-formats.html).
</td>
  </tr>
</table>

Die Nachricht kann außerdem optionale Parameter enthalten, die andere Aspekte für die Verarbeitung der Anforderung sowie die zurückzugebenden Informationen angeben. Informationen zu allen Ein- und Ausgabefunktionen finden Sie im Abschnitt [Parameterübersicht](/docs/services/speech-to-text/summary.html). Ein Sprachmodell, ein angepasstes Sprachmodell und ein Akustikmodell können Sie nur als Abfrageparameter der WebSocket-URL angeben.

Das folgende Snippet mit JavaScript-Code sendet Initialisierungsparameter für die Erkennungsanforderung über die WebSocket-Verbindung. Die Aufrufe sind in der Funktion `onOpen` des Clients enthalten, um sicherzustellen, dass sie erst gesendet werden, nachdem die Verbindung hergestellt wurde.

```javascript
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050'
  };
  websocket.send(JSON.stringify(message));
}
```
{: codeblock}

Wenn die Anforderung ordnungsgemäß empfangen wurde, signalisiert der Service durch die folgende Textnachricht, dass er empfangsbereit (`listening`) ist:

```javascript
{'state': 'listening'}
```
{: codeblock}

Der Status `listening` gibt an, dass die Serviceinstanz konfiguriert ist (d. h., Sie haben eine gültige JSON-Nachricht `start` übergeben) und bereit ist, eine neue Äußerung für eine Erkennungsanforderung zu verarbeiten. Sobald der ervice empfangsbereit ist, werden alle Audiodaten verarbeitet, die vor dem Ausgeben der Nachricht `listening` gesendet wurden.

Wenn der Client einen ungültigen Abfrageparameter oder ein ungültiges JSON-Feld für die Erkennungsanforderung angibt, enthält die JSON-Antwort ein Feld `warnings`. In dem Feld werden alle ungültigen Argumente beschrieben. Die Anforderung wird trotz der gemeldeten Warnungen erfolgreich ausgeführt.

## Audiodaten senden und Erkennungsergebnisse empfangen
{: #WSaudio}

Nach dem Senden der einleitenden Nachricht `start` kann der Client mit dem Senden von Audiodaten an den Service beginnen. Der Client wartet nicht, bis der Service die Nachricht `start` mit der Nachricht `listening` beantwortet hat. Der Service gibt die Transkriptionsergebnisse asynchron im selben Format wie Ergebnisse für die HTTP-Schnittstellen zurück.

Der Client muss die Audiodaten als binäre Daten senden. Der Client kann maximal 100 MB an Audiodaten mit einer einzigen Äußerung senden (pro Anforderung `send`). Für jede Anforderung muss der Client mindestens 100 Byte an Audiodaten senden. Der Client kann mehrere Äußerungen über eine einzige WebSocket-Verbindung senden. Informationen zum Maximieren der Audiodatenmenge, die in einer Anforderung an den Service übergeben werden kann, finden Sie im Abschnitt [Audioformate](/docs/services/speech-to-text/audio-formats.html).

Für die WebSocket-Schnittstelle gilt eine maximale Rahmengröße von 4 MB. Der Client kann als maximale Rahmengröße einen Wert kleiner als 4 MB festlegen. Wenn das Festlegen der Rahmengröße nicht sinnvoll ist, kann der Client die maximale Nachrichtengröße auf einen Wert kleiner als 4 MB festlegen und die Audiodaten als eine Folge von Nachrichten senden.

Das folgende Snippet mit JavaScript-Code sendet Audiodaten als binäre Nachricht (BLOB) an den Service:

```javascript
websocket.send(blob);
```
{: codeblock}

Das folgende Codesnippet empfängt vom Service zurückgegebene Erkennungshypothesen asynchron. Die Ergebnisse werden von der Funktion `onMessage` des Clients verarbeitet.

```javascript
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

## Erkennungsanforderung beenden
{: #WSstop}

Wenn das Senden der Audiodaten für eine Anforderung an den Service abgeschlossen ist, *muss* der Client das Ende der binären Audiodatenübertragung zum Service auf eine der folgenden Arten kenntlich machen:

-   Durch Senden einer JSON-Textnachricht, mit einem Parameter `action`, der auf den Wert `stop` gesetzt ist:

    ```javascript
    {action: 'stop'}
    ```
    {: codeblock}

-   Durch Senden einer leeren binären Nachricht, in der das angegebene BLOB leer ist:

    ```javascript
    websocket.send(blob)
    ```
    {: codeblock}

Der Service sendet die endgültigen Ergebnisse erst, nachdem er die Bestätigung empfangen hat, dass die Audiodatenübertragung abgeschlossen ist. Wenn nicht signalisiert wird, dass die Übertragung abgeschlossen ist, wird möglicherweise das Verbindungszeitlimit überschritten und der Service sendet keine endgültigen Ergebnisse.

Um zwischen mehreren Erkennungsanforderungen endgültige Ergebnisse zu empfangen, muss der Client das Ende der Übertragung für die vorherige Anforderung signalisieren, bevor die nächste Anforderung gesendet wird. Nachdem der Service die endgültigen Ergebnisse für die erste Anforderung zurückgegeben hat, gibt er eine weitere Nachricht `{"state":"listening"}` an den Client zurück. Diese Nachricht gibt an, dass der Service bereit ist, eine weitere Anforderung zu empfangen.

## Weitere Anforderungen senden und Anforderungsparameter ändern
{: #WSmore}

Solange die WebSocket-Verbindung aktiv ist, kann der Client die Verbindung nutzen, um weitere Erkennungsanforderungen mit neuen Audiodaten zu senden. Standardmäßig verwendet der Service die mit der vorherigen Nachricht `start` übergebenen Parameter für alle nachfolgenden Anforderungen, die über dieselbe Verbindung gesendet werden.

Um die Parameter für nachfolgende Anforderungen zu ändern, kann der Client eine weitere Nachricht `start` mit den neuen Parametern senden, nachdem er die endgültigen Erkennungsergebnisse und die Nachricht `{"state":"listening"}` vom Service empfangen hat. Der Client kann alle Parameter ändern, außer denen, die beim Öffnen der Verbindung angegeben wurden (`model`, `language_customization_id` usw.).

Im folgenden Beispiel wird eine Nachricht `start` mit neuen Parametern für nachfolgende Erkennungsanforderungen gesendet, die über die Verbindung übertragen werden. In der Nachricht wird derselbe Parameter `content-type` wie im vorherigen Beispiel angegeben, aber der Service wird angewiesen, Konfidenzwerte und Zeitmarken für die Wörter der Transkription zurückzugeben.

```javascript
var message = {
  action: 'start',
  content-type: 'audio/l16;rate=22050',
  word_confidence: true,
  timestamps: true
};
websocket.send(JSON.stringify(message));
```
{: codeblock}

## Verbindung offen halten
{: #WSkeep}

Der Service beendet die Sitzung und trennt die Verbindung, wenn ein Zeitlimit für Inaktivität oder Sitzungsdauer erreicht ist:

-   Eine *Inaktivitätszeitlimitüberschreitung* tritt auf, wenn der Service in den vom Client gesendeten Audiodaten keine gesprochenen Äußerungen erkennen kann. Der Standardwert für das Inaktivitätszeitlimit sind 30 Sekunden. Mit dem Parameter `inactivity_timeout` können Sie einen anderen Wert angeben (z. B. auch den Wert `-1` für ein unbegrenztes Zeitlimit).
-   Eine *Sitzungszeitlimitüberschreitung* tritt auf, wenn der Service keine Daten vom Client empfängt oder 30 Sekunden lang keine Zwischenergebnisse sendet. Die Länge dieses Zeitlimits kann nicht geändert werden, aber Sie können die Sitzung aktiv halten, indem Sie einfach beliebige Audiodaten (z. B. Sprechpausen) an den Service senden, bevor das Zeitlimit abläuft. Außerdem müssen Sie den Parameter `inactivity_timeout` auf den Wert `-1` setzen. Die Übertragungszeit für die Daten, die Sie an den Service senden (einschließlich der Sprechpausen, um eine Sitzung aktiv zu halten), wird Ihnen in Rechnung gestellt.

Weitere Informationen finden Sie im Abschnitt [Zeitlimits](/docs/services/speech-to-text/input.html#timeouts). 

## Verbindung schließen
{: #WSclose}

Wenn die Interaktion des Clients mit dem Service beendet ist, kann die WebSocket-Verbindung geschlossen werden. Nach dem Schließen der Verbindung, kann der Client über die Verbindung keine Anforderungen mehr senden und keine Ergebnisse mehr empfangen. Schließen Sie die Verbindung erst, nachdem Sie alle Ergebnisse für eine Anforderung empfangen haben. Wenn Sie die Verbindung nicht explizit schließen, wird sie nach dem Ablaufen des Zeitlimits beendet.

Das folgende Snippet mit JavaScript-Code schließt eine bestehende Verbindung:

```javascript
websocket.close();
```
{: codeblock}

## WebSocket-Rückgabecodes
{: #WSreturn}

Der Service kann die folgenden Rückgabecodes über die WebSocket-Verbindung an den Client senden:

-   `1000` gibt die normale Beendigung der Verbindung an; d. h. der Zweck, zu dem die Verbindung hergestellt wurde, ist erfüllt.
-   `1002` gibt an, dass der Service die Verbindung wegen eines Protokollfehlers beendet.
-   `1006` gibt an, dass die Verbindung abnormal beendet wurde.
-   `1009` gibt an, dass die Obergrenze von 4 MB für die Rahmengröße überschritten wurde.
-   `1011` gibt an, dass der Service die Verbindung beendet, weil die Anforderung aufgrund einer nicht erwarteten Bedingung nicht erfüllt werden kann.

Wenn das Socket mit einem Fehler geschlossen wird, erhält der Client eine Informationsnachricht im Format `{"fehler":"{nachricht}"}`, bevor das Socket geschlossen wird. Weitere Informationen zu WebSocket-Rückgabecodes finden Sie unter [IETF RFC 6455 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://tools.ietf.org/html/rfc6455){: new_window}.

## Beispiel für WebSocket-Sitzung
{: #WSexample}

Das folgende Beispiel zeigt eine WebSocket-Sitzung zwischen einem Client und dem {{site.data.keyword.speechtotextshort}}-Service. Zur besseren Übersicht wird die Sitzung als drei separate Interaktionen dargestellt. Alle drei Interaktionen sind jedoch Teil einer einzigen Sitzung mit dem Service. Das Beispiel ist auf den Austausch von Nachrichten fokussiert, es zeigt nicht das Öffnen und Schließen der Verbindung.

### Erster Beispielaustausch
{: #firstExample}

Im ersten Austausch sendet der Client Audiodaten, in denen die Zeichenfolge `Name the Mayflower` enthalten ist. Der Client sendet eine binäre Nachricht mit einem einzigen Block von PCM-Audiodaten (`audio/l16`) und gibt die dafür erforderliche Abtastfrequenz an. Der Client wartet nicht auf die Antwort `{"state":"listening"}` vom Service, bevor mit dem Senden der Audiodaten begonnen und das Ende der Anforderung signalisiert wird. Durch das sofortige Senden der Daten wird die Latenzzeit verringert, da die Audiodaten für den Service verfügbar sind, sobald er bereit ist, die Erkennungsanforderung zu verarbeiten.

-   Der *Client* sendet Folgendes:

    ```javascript
    {
      "action": "start",
      "content-type": "audio/l16;rate=22050"
    }
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   Der *Service* antwortet wie folgt:

    ```javascript
    {"state": "listening"}
    {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### Zweiter Beispielaustausch
{: #secondExample}

Im zweiten Austausch sendet der Client Audiodaten, in denen die Zeichenfolge `Second audio transcript` enthalten ist. Der Client sendet die Audiodaten in einer einzigen binären Nachricht und verwendet die gleichen Parameter, die er in der ersten Anforderung angegeben hat.

-   Der *Client* sendet Folgendes:

    ```javascript
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   Der *Service* antwortet wie folgt:

    ```javascript
    {"results": [{"alternatives": [{"transcript": "second audio transcript "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### Dritter Beispielaustausch
{: #thirdExample}

Im dritten Austausch sendet der Client wieder Audiodaten, in denen die Zeichenfolge `Name the Mayflower` enthalten ist. Der Client sendet ebenso eine binäre Nachricht mit einem einzigen Block von PCM-Audiodaten. Diesmal fordert der Client jedoch Zwischenergebnisse mit der Transkription an.

-   Der *Client* sendet Folgendes:

    ```javascript
    {
      "action": "start",
      "content-type": "audio/l16;rate=22050",
      "interim_results": true
    }
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   Der *Service* antwortet wie folgt:

    ```javascript
    {"state":"listening"}
    {"results": [{"alternatives": [{"transcript": "name "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name may "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name may flour "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}
