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

# Eingabefunktionen
{: #input}

Mit den hier aufgeführten Funktionen des {{site.data.keyword.speechtotextshort}}-Service wird angegeben, wie der Service eine Spracherkennungsanforderung ausführen soll. Alle hier beschriebenen Eingabeparameter sind optional. Lediglich die Eingabeaudiodaten sind erforderlich.
{: shortdesc}

-   Beispiele für einfache Anforderungen zur Spracherkennung bei jeder Schnittstelle des Service enthält der Abschnitt [Erkennungsanforderung ausgeben](/docs/services/speech-to-text/basic-request.html).
-   Eine alphabetisch sortierte Liste aller verfügbaren Parameter für die Spracherkennung, in der auch der jeweilige Status (allgemein verfügbar oder Betaversion) sowie die unterstützten Sprachen angegeben sind, finden Sie in der [Parameterübersicht](/docs/services/speech-to-text/summary.html).

## Angepasste Modelle
{: #custom-input}

Die Anpassung des Sprachmodells und des Akustikmodells ist für unterschiedliche Sprachen auf verschiedenen Unterstützungsstufen (allgemein verfügbar oder Betaversion) verfügbar. Weitere Informationen finden Sie unter [Sprachunterstützung bei der Anpassung](/docs/services/speech-to-text/custom.html#languageSupport).
{: note}

Bei allen Schnittstellen kann in einer Erkennungsanforderung ein angepasstes Modell verwendet werden:

-   *Angepasste Sprachmodelle* erweitern das Grundvokabular des Service durch Terminologie aus bestimmten Fachgebieten. Mit dem Parameter `language_customization_id` können Sie ein angepasstes Sprachmodell in eine Anforderung einbeziehen. Außerdem können Sie einen optionalen Parameter namens `customization_weight` angeben. Der Parameter legt die relative Gewichtung fest, die Wörter aus dem angepassten Modell im Vergleich zu Wörtern aus dem Grundvokabular erhalten.

    Weitere Informationen finden Sie unter [Angepasstes Sprachmodell verwenden](/docs/services/speech-to-text/language-use.html).
-   *Angepasste Akustikmodelle* passen das akustische Basismodell des Service an die akustischen Besonderheiten Ihrer Umgebung und Sprecher an. Mit dem Parameter `acoustic_customization_id` können Sie ein angepasstes Akustikmodell in eine Anforderung einbeziehen. Für eine Anforderung können Sie sowohl ein angepasstes Sprachmodell als auch ein angepasstes Akustikmodell angeben.

    Weitere Informationen finden Sie unter [Angepasstes Akustikmodell verwenden](/docs/services/speech-to-text/acoustic-use.html).


Angepasste Modelle basieren auf einem der Sprachmodelle, die im Abschnitt [Sprachen und Modelle](/docs/services/speech-to-text/models.html) beschrieben sind. Ein angepasstes Modell kann nur in Verbindung mit dem Basismodell verwendet werden, für das es erstellt wurde. Wenn Ihr angepasstes Modell auf einem anderen Modell als `en-US_BroadbandModel` basiert, müssen Sie mit der Anforderung auch den Namen des Modells angeben. Zur Verwendung eines angepassten Modells müssen Sie die Anforderung mit den Serviceberechtigungsnachweisen ausgeben, die für die Instanz des Service erstellt wurden, bei der es sich um den Eigner des angepassten Modells handelt.

Eine Einführung in die Anpassung finden Sie im Abschnitt [Anpassungsschnittstelle](/docs/services/speech-to-text/custom.html).

### Beispiele für angepasste Modelle
{: #customExample}

Die folgende Beispielanforderung enthält den Parameter `language_customization_id`, damit das angepasste Sprachmodell mit der angegebenen ID verwendet wird. Durch den ebenfalls enthaltenen Parameter `customization_weight` wird angegeben, dass Wörter aus dem angepassten Modell die relative Gewichtung `0.5` erhalten sollen.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.5"
```
{: pre}

In der folgenden Beispielanforderung wird sowohl ein angepasstes Sprachmodell als auch ein angepasstes Akustikmodell verwendet. Das Sprachmodell wird mit dem Parameter `language_customization_id` angegeben, das Akustikmodell mit dem Parameter `acoustic_customization_id`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&acoustic_customization_id={anpassungs-id}"
```
{: pre}

Beispiele für die Verwendung von angepassten Modellen bei den einzelnen Schnittstellen des Service enthalten die folgenden Abschnitte:

-   [Angepasstes Sprachmodell verwenden](/docs/services/speech-to-text/language-use.html)
-   [Angepasstes Akustikmodell verwenden](/docs/services/speech-to-text/acoustic-use.html)

## Grammatiken
{: #grammars-input}

Die Funktion für die Grammatiken liegt als Betaversion vor. Der Service unterstützt Grammatiken für alle Sprachen, bei denen er die Anpassung des Sprachmodells unterstützt.
{: note}

Sie können Grammatiken zu einem angepassten Sprachmodell hinzufügen und sie für die Spracherkennung nutzen. Grammatiken definieren mithilfe einer formalen Sprachspezifikation eine Reihe von Produktionsregeln für die Transkription von Zeichenfolgen. Die Regeln geben an, wie gültige Zeichenfolgen aus dem Alphabet der Sprache gebildet werden sollen.

Wenn Sie eine Grammatik für die Spracherkennung verwenden, erkennt der Service ausschließlich Ausdrücke, die von der Grammatik erkannt werden. Durch die Einschränkung des Suchbereichs auf gültige Zeichenfolgen kann der Service Ergebnisse schneller und präziser liefern. 

Alle Schnittstellen akzeptieren die folgenden Parameter für eine Erkennungsanforderung:

-   Der Parameter `language_customization_id` gibt das angepasste Sprachmodell an, für das die Grammatik definiert ist. Sie müssen die Anforderung mit den Serviceberechtigungsnachweisen für diejenige Instanz des Service ausgeben, die Eigner des Modells ist.
-   Der Parameter `grammar_name` gibt die Grammatik an, die verwendet werden soll. Bei einer Anforderung können Sie nur eine einzige Grammatik angeben.

Weitere Informationen finden Sie unter [Grammatiken bei angepassten Sprachmodellen verwenden](/docs/services/speech-to-text/grammar.html).

### Beispiel für Grammatik
{: #grammarsExample}

Die folgende Beispielanforderung enthält die Parameter `language_customization_id` und `grammar_name`, damit die Antwort des Service auf Zeichenfolgen begrenzt wird, die in der Grammatik namens `list-abnf` definiert sind.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&grammar_name=list-abnf"
```
{: pre}

Beispiele für die Verwendung von Grammatiken bei den einzelnen Schnittstellen des Service enthält der Abschnitt [Grammatik für die Spracherkennung verwenden](/docs/services/speech-to-text/grammar-use.html).

## Version des Basismodells
{: #version}

Zur Verbesserung der Qualität bei der Spracherkennung aktualisiert der Service von Zeit zu Zeit die unter [Sprachen und Modelle](/docs/services/speech-to-text/models.html) beschriebenen Basissprachmodelle. Die Basismodelle für Sprachen sind, ebenso wie die Breitband- und Schmalbandmodelle für eine Sprache, voneinander unabhängig. Aktualisierungen der Basismodelle finden daher unabhängig voneinander statt und haben keinen Einfluss auf andere Modelle.

Wenn mehrere Versionen eines Basismodells vorhanden sind, gibt der optionale Parameter `base_model_version` die Version des Modells an, das bei einer Erkennungsanforderung verwendet werden soll. Der Parameter ist in erster Linie zur Verwendung bei angepassten Modellen gedacht, die für ein neues Basismodell aktualisiert wurden. Er kann jedoch auch ohne ein angepasstes Modell eingesetzt werden. Die für eine Anforderung verwendete Version eines Basismodells richtet sich danach, ob Sie den Parameter `base_model_version` übergeben. Sie ist außerdem davon abhängig, ob Sie mit der Anforderung ein angepasstes Modell (Sprachmodell und/oder Akustikmodell) angeben.

-   *Anforderung ohne Angabe eines angepassten Modells*

    -   Lassen Sie den Parameter `base_model_version` weg, damit die neueste Version des Basismodells verwendet wird.
    -   Geben Sie den Parameter `base_model_version` an, damit eine bestimmte Version des Basismodells verwendet wird.

-   *Anforderung mit Angabe eines angepassten Modells*

    -   Lassen Sie den Parameter `base_model_version` weg, damit die neueste Version des Basismodells verwendet wird, auf die für das angepasste Modell ein Upgrade durchgeführt wurde. Falls beispielsweise für das angepasste Modell ein Upgrade auf die neueste Version des Basismodells durchgeführt wurde, verwendet der Service die neuesten Versionen des Basismodells und des angepassten Modells.
    -   Geben Sie den Parameter `base_model_version` an, damit die angegebenen Versionen des Basismodells und des angepassten Modells verwendet werden.

Der Parameter ist zur Verwendung bei angepassten Modellen gedacht. Die verfügbaren Versionen eines Basismodells können Sie daher nur dadurch ermitteln, indem Sie Informationen zu einem angepassten Modell auflisten, das auf diesem Modell basiert.

-   Weitere Informationen zum Upgrade von angepassten Modellen finden Sie unter [Upgrade für angepasste Modelle durchführen](/docs/services/speech-to-text/custom-upgrade.html).
-   Zusätzliche Angaben über die Verwendung unterschiedlicher Versionen von Basismodellen und angepassten Modellen für die Spracherkennung enthält der Abschnitt [Erkennungsanforderungen mit angepassten Modellen nach durchgeführtem Upgrade ausgeben](/docs/services/speech-to-text/custom-upgrade.html#upgradeRecognition).

## Übertragung von Audiodaten
{: #transmission}

Bei der *WebSocket-Schnittstelle* werden Audiodaten immer über die Verbindung an den Service gestreamt. Über den Socket können Sie alle Daten auf einmal übergeben oder aber Daten für den aktiven Anwendungsfall senden, sobald sie zur Verfügung stehen. Der Service gibt Ergebnisse zurück, sobald sie verfügbar sind.

Bei den *HTTP-Schnittstellen* können Sie eines der folgenden Verfahren für die Übertragung von Audiodaten an den Service verwenden:

-   *Einmalübermittlung* - Sie lassen den Header `Transfer-Encoding` weg und übergeben alle Audiodaten in Form einer einzigen Übermittlung auf einmal an den Service.
-   *Streaming* - Sie legen für den Anforderungsheader `Transfer-Encoding` den Wert `chunked` fest und streamen die Daten über eine persistente Verbindung. Die Daten müssen nicht vollständig vorhanden sein, bevor Sie sie an den Service streamen. Sie können die Daten übertragen, sobald sie verfügbar sind. Der Service sendet erst dann Ergebnisse, wenn er den letzten Block empfängt, was Sie durch das Senden eines leeren Blocks kenntlich machen.

    Weitere Informationen zum Streamen von Audiodatenblöcken mit dem Header `Transfer-Encoding` enthalten die folgenden Quellen:
    -   [en.wikipedia.org/wiki/Chunked_transfer_encoding ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Chunked_transfer_encoding){: new_window}
    -   [Transfer Codings ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://tools.ietf.org/html/rfc7230#section-4){: new_window} in: *IETF RFC 7320 HTTP/1.1: Message Syntax and Routing*

Bei den HTTP-Schnittstellen transkribiert der Service immer den gesamten Audiodatenstrom, bevor er Ergebnisse sendet. Die Ergebnisse können mehrere Elemente `transcript` enthalten, um Ausdrücke zu kennzeichnen, die durch Sprechpausen voneinander getrennt sind. Verketten Sie die Elemente `transcript`, um die vollständige Transkription zusammenzusetzen.

Bei einer Streamingsitzung setzt der Service Zeitlimits durch. Er kann eine Streamingsitzung beenden, falls er für die Dauer von 30 Sekunden ein längeres Schweigen erkennt oder keine Audiodaten empfängt. Weitere Informationen zu Zeitlimits und ihrer Vermeidung finden Sie unter [Zeitlimits](#timeouts).

### Beispiel für Übertragung von Audiodaten
{: #transmissionExample}

In der folgenden Beispielanforderung ist der Wert `chunked` für den Header `Transfer-Encoding` angegeben, damit der Streaming-Modus verwendet wird. Die Verbindung bleibt geöffnet, damit weitere Audiodatenblöcke akzeptiert werden können.


```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--header "Transfer-Encoding: chunked"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## Zeitlimits
{: #timeouts}

Wenn Sie eine Streamingsitzung mit den Spracherkennungsmethoden der WebSocket- oder HTTP-Schnittstellen starten, setzt der Service Inaktivitäts- und Sitzungszeitlimits durch. Falls während einer Streamingsitzung ein Zeitlimit überschritten wird, schließt der Service die Verbindung. Ihre Anwendung muss mögliche geschlossene Verbindungen ordnungsgemäß wiederherstellen.

Wenn Sie Audiodaten über HTTP streamen, sendet der Service alle 20 Sekunden in seiner Antwort ein Leerzeichen. Hierdurch verbessert der Service den Bedienungskomfort, weil das HTTP-REST-Inaktivitätszeitlimit von 30 Sekunden vermieden wird. Damit die Verbindung aufrecht erhalten bleibt, während die Erkennung fortgesetzt wird, sendet der Service dieses Zeichen weiter, bis er die Transkription abgeschlossen hat. Auf die JSON-codierten Antwortdaten hat das Leerzeichen keinen Einfluss.

Dieses HTTP-Inaktivitätszeitlimit unterscheidet sich vom Inaktivitätszeitlimit des Service. Bei der WebSocket-Schnittstelle wird dieses HTTP-Zeitlimit nicht angewendet.
{: note}

### Inaktivitätszeitlimit
{: #timeouts-inactivity}

Das *Inaktivitätszeitlimit* (HTTP-Statuscode 400) tritt ein, wenn der Service Audiodaten empfängt, 30 Sekunden lang jedoch lediglich fortdauerndes Schweigen oder aber keine Sprachgeräusche empfängt. Der Service sendet in diesem Fall die Fehlernachricht `30 Sekunden lang keine Sprache erkannt`. Das Inaktivitätszeitlimit ist beispielsweise von Nutzen, um eine Sitzung zu beenden, wenn sich ein Benutzer einfach von einem eingeschalteten Mikrofon entfernt.

Der Standardwert für das Inaktivitätszeitlimit beträgt 30 Sekunden. Diesen Wert können Sie mit dem Parameter `inactivity_timeout` außer Kraft setzen. Geben Sie einen größeren Wert an, um das Inaktivitätszeitlimit zu erhöhen. Geben Sie den Wert `-1` an, um ein unendliches Inaktivitätszeitlimit zu definieren. Alle Audiodaten, die Sie an den Service senden, werden Ihnen in Rechnung gestellt, was auch Schweigen umfasst. Ein Heraufsetzen des Inaktivitätszeitlimits kann daher zusätzliche Gebühren für eine Streamingsitzung verursachen, über die lediglich Schweigen übertragen wird.

Mit der folgenden Beispielanforderung wird das Inaktivitätszeitlimit auf 60 Sekunden gesetzt. Die Anforderung sendet eine Anfangsdatei, um die Streamingsitzung zu starten.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Transfer-Encoding: chunked"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?inactivity_timeout=60"
```
{: pre}

### Sitzungszeitlimit
{: #timeouts-session}

Ein *Sitzungszeitlimit* (HTTP-Statuscode 408) tritt ein, wenn die von Ihnen gesendeten Audiodaten nicht ausreichen, um eine Streamingsitzung im aktiven Zustand zu halten. Der Service kann aus den folgenden Gründen eine Sitzung für inaktiv halten und ein Sitzungszeitlimit auslösen:

-   In jedem Zeitfenster von 30 Sekunden werden nicht mindestens 15 Sekunden lange Audiodaten an den Service gesendet.

    Bis Sie den letzten Block senden, um das Ende des Datenstroms kenntlich zu machen, müssen Sie in jedem 30 Sekunden langen Zeitraum mindestens 15 Sekunden lange Audiodaten senden. Bei den Audiodaten kann es sich um Schweigen handeln, falls Sie für den Parameter `inactivity_timeout` einen höheren Wert als `-1` festgelegt haben. Die gesamte Dauer der Audiodaten, die Sie an den Service senden, wird Ihnen in Rechnung gestellt, was auch Schweigen umfasst.

-   Das Streaming der Audiodaten erfolgt in einer Geschwindigkeit, die unter der Echtzeit liegt.

    Im Idealfall starten Sie eine Anforderung zum Aufbau einer Sitzung unmittelbar vor dem Erhalt von Audiodaten für die Transkription. Anschließend erhalten Sie die Sitzung aufrecht, indem Sie Audiodaten mit einer Geschwindigkeit senden, die nahezu Echtzeit entspricht.

Nachdem Sie den letzten Block gesendet haben, um das Ende der Sitzung anzugeben, müssen Sie sich keine Gedanken um das Sitzungszeitlimit machen. Der Service setzt die Verarbeitung der Audiodaten fort, bis er die Endergebnisse für die Transkription zurückgibt.

Wenn Sie einen langen Audiodatenstrom transkribieren, kann der Service mehr als 30 Sekunden benötigen, um die Audiodaten zu verarbeiten und eine Antwort zu generieren. Der Service beginnt erst dann mit der Berechnung des Sitzungszeitlimits, wenn er die Verarbeitung aller empfangenen Audiodaten abgeschlossen hat. Die vom Service zur Verarbeitung benötigte Zeit kann in keinem Fall ein Überschreiten des Sitzungszeitlimits von 30 Sekunden verursachen.

Beispiel: Falls Sie in den ersten 10 Sekunden einer Sitzung Audiodaten mit einer Dauer von einer Stunde senden, benötigt der Service möglicherweise 300 Sekunden, um die Audiodaten zu verarbeiten. Damit diese Sitzung aufrecht erhalten bleibt, müssen Sie spätestens nach 340 Sekunden mindestens 15 Sekunden Audiodaten an die Sitzung senden.

Wenn Sie bei diesem Beispiel nach 100 Sekunden Sitzungsdauer weitere 15 Sekunden Audiodaten an die Sitzung senden, benötigt der Service zur Verarbeitung dieser Audiodaten möglicherweise weitere zwei Sekunden. In diesem Fall müssten Sie spätestens nach 342 Sekunden weitere 15 Sekunden Audiodaten an die Sitzung senden.

Verlassen Sie sich bei der Ermittlung, ob eine Streamingsitzung inaktiv ist, nicht auf die Verarbeitungszeit oder darauf, dass Sie Ergebnisse empfangen haben. Gehen Sie davon aus, dass der Service alle Audiodaten sofort verarbeiten kann, und senden Sie dementsprechend Daten an den Service. Bleiben Sie beim Streaming von Audiodaten in Echtzeit in jedem Zeitfenster von 30 Sekunden mit dem Senden von Audiodaten nicht hinter der Hälfte der Echtzeit (15 Sekunden Audiodaten) zurück. Diese Geschwindigkeit reicht normalerweise aus, um Netzlatenz und Verzögerungen zu kompensieren.
{: important}

## Protokollierung von Anforderungen
{: #logging}

{{site.data.keyword.IBM_notm}} protokolliert standardmäßig alle Anforderungen an {{site.data.keyword.watson}}-Services und deren Ergebnisse. Die Protokollierung hat einzig den Zweck, die Services für künftige Benutzer zu verbessern. Die protokollierten Daten werden nicht geteilt oder öffentlich gemacht.

Falls Sie die persönlichen Daten von Benutzern schützen müssen oder aus anderem Grund keine Protokollierung Ihrer Anforderungen durch IBM wünschen, können Sie auswählen, dass IBM Daten nicht protokolliert, indem Sie die Protokollierung explizit ausschließen. Die Ablehnung der Protokollierung kann entweder auf Kontoebene oder auf Ebene der API-Anforderung erfolgen. Weitere Informationen enthält der Abschnitt [Anforderungsprotokollierung für {{site.data.keyword.watson}}-Services steuern](/docs/services/watson/getting-started-logging.html).

## Informationssicherheit
{: #security-input}

Die Informationssicherheit umfasst Funktionen, mit denen Sie Daten, die mit einer Anforderung an den Service übergeben werden, eine Kunden-ID zuordnen können. Zur Zuordnung einer Kunden-ID zu den Daten übergeben Sie mit der Anforderung einen Header `X-Watson-Metadata`. Bei Bedarf können Sie danach die Daten mit der Methode `DELETE /v1/user_data` löschen. Weitere Informationen finden Sie im Abschnitt [Informationssicherheit](/docs/services/speech-to-text/information-security.html).
