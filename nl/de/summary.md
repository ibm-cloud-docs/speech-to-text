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

# Parameterübersicht
{: #summary}

Die folgende Übersicht enthält alle für die Spracherkennung verfügbaren Parameter. Weitere Informationen zu allen Methoden des {{site.data.keyword.speechtotextshort}}-Service finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
{: shortdesc}

Beachten Sie beim Erstellen einer Spracherkennungsanforderung die folgenden Grundvoraussetzungen:

-   Bei Methodennamen muss die Groß-/Kleinschreibung beachtet werden.
-   HTTP-Anforderungsheader sind von Groß-/Kleinschreibung unabhängig.
-   Bei HTTP- und WebSocket-Abfrageparametern muss die Groß-/Kleinschreibung beachtet werden.
-   Bei JSON-Feldnamen muss die Groß-/Kleinschreibung beachtet werden.
-   Für alle Inhalte von JSON-Antworten wird der UTF-8-Zeichensatz verwendet.

Beachten Sie außerdem die folgenden servicespezifischen Anforderungen:

-   Sie müssen nur die Audioeingabedaten angeben. Alle anderen Parameter sind optional.
-   Wenn Sie einen ungültigen Abfrageparameter oder ein ungültiges JSON-Feld in der Eingabe angeben, enthält die Antwort ein Feld `warnings`, in dem das ungültige Argument beschrieben wird. Die Anforderung wird trotz der angegebenen Warnungen erfolgreich ausgeführt.

## access_token
{: #summary-access-token}

Bei Verwendung der Authentifizierung mit Identity and Access Management (IAM) ist dieser Parameter ein optionales IAM-Zugriffstoken zum Herstellen einer authentifizierten Verbindung zur WebSocket-Schnittstelle. Weitere Informationen finden Sie im Abschnitt [Verbindung öffnen](/docs/services/speech-to-text/websockets.html#WSopen).

<table>
  <caption>Tabelle 1. Parameter 'access_token'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Abfrageparameter für Verbindungsanforderung mit der Methode <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Nicht unterstützt
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Nicht unterstützt
    </td>
  </tr>
</table>

## acoustic_customization_id
{: #summary-acoustic-customization-id}

Eine optionale Anpassungs-ID für ein angepasstes Akustikmodell, das auf die Akustikmerkmale Ihrer Umgebung und der zugehörigen Sprecher abgestimmt ist. Standardmäßig wird kein angepasstes Modell verwendet. Weitere Informationen finden Sie im Abschnitt [Angepasste Modelle](/docs/services/speech-to-text/input.html#custom-input).

<table>
  <caption>Tabelle 2. Parameter 'acoustic_customization_id'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Betaversion für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Abfrageparameter für Verbindungsanforderung mit der Methode <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## base_model_version
{: #summary-base-model-version}

(Optional) Eine Version eines Basismodells. Der Parameter ist in erster Linie für die Verwendung mit angepassten Modellen bestimmt, die für ein neues Basismodell aktualisiert werden, kann aber auch ohne ein angepasstes Modell verwendet werden. Der Standardwert ist davon abhängig, ob der Parameter mit oder ohne ein angepasstes Modell verwendet wird. Weitere Informationen finden Sie im Abschnitt [Version des Basismodells](/docs/services/speech-to-text/input.html#version).

<table>
  <caption>Tabelle 3. Parameter 'base_model_version'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Abfrageparameter für Verbindungsanforderung mit der Methode <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## Content-Type
{: #summary-content-type}

(Optional) Ein Audioformat (MIME-Typ) zum Angeben des Formats der Audiodaten, die Sie an den Service übergeben. Der Service kann das Format der meisten Audiodaten automatisch erkennen; d. h. der Parameter ist für die meisten Formate optional. Er ist jedoch erforderlich für die Formate `audio/alaw`, `audio/basic`, `audio/l16` und `audio/mulaw`. Weitere Informationen finden Sie im Abschnitt [Audioformate](/docs/services/speech-to-text/audio-formats.html).

<table>
  <caption>Tabelle 4. Parameter 'Content-Type'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parameter <code>content-type</code> der JSON-Nachricht
      <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Anforderungsheader der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Anforderungsheader der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## customization_weight
{: #summary-customization-weight}

Ein optionales Doppelzeichen zwischen 0,0 und 1,0, das die relative Gewichtung angibt, die der Service den Wörtern aus einem angepassten Sprachmodell im Verhältnis zu Wörtern aus dem Basisvokabular zuordnet. Der Standardwert ist 0,3, sofern beim Trainieren des angepassten Sprachmodells keine andere Gewichtung angegeben wurde. Weitere Informationen finden Sie im Abschnitt [Angepasste Modelle](/docs/services/speech-to-text/input.html#custom-input).

<table>
  <caption>Tabelle 5. Parameter 'customization_weight'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für amerikanisches Englisch, britisches Englisch, brasilianisches Portugiesisch, Französisch, Deutsch, Japanisch, Koreanisch und Spanisch
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parameter der JSON-Nachricht <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## grammar_name
{: #summary-grammar-name}

Eine optionale Zeichenfolge, die eine Grammatik angibt, die für die Spracherkennung verwendet werden soll. Der Service erkennt nur Zeichenfolgen, die von der Grammatik definiert sind. Sie müssen sowohl den Namen der Grammatik angeben als auch die Anpassungs-ID des angepassten Sprachmodells, für das die Grammatik definiert ist. Weitere Informationen finden Sie im Abschnitt [Grammatiken](/docs/services/speech-to-text/input.html#grammars-input).

<table>
  <caption>Tabelle 6. Parameter 'grammar_name'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Betaversion für amerikanisches Englisch, britisches Englisch, brasilianisches Portugiesisch, Französisch, Deutsch, Japanisch, Koreanisch und Spanisch
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parameter der JSON-Nachricht <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## inactivity_timeout
{: #summary-inactivity-timeout}

Ein optionaler ganzzahliger Wert, der die Anzahl der Sekunden für das Inaktivitätszeitlimit des Service angibt. Inaktivität bedeutet, dass der Service keine Spracherkennung für die Streaming-Audiodaten durchführt. Der Standardwert ist 30 Sekunden. Der Wert `-1` bedeutet 'Unendlich'. Weitere Informationen finden Sie im Abschnitt [Inaktivitätszeitlimit](/docs/services/speech-to-text/input.html#timeouts-inactivity).

<table>
  <caption>Tabelle 7. Parameter 'inactivity_timeout'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parameter der JSON-Nachricht <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## interim_results
{: #summary-interim-results}

Ein optionaler boolescher Wert, der den Service anweist, temporäre Hypothesen zurückzugeben, die bis zum endgültigen Transkript noch geändert werden können. Die Standardeinstellung `false` gibt an, dass keine Zwischenergebnisse zurückgegeben werden. Weitere Informationen finden Sie im Abschnitt [Zwischenergebnisse](/docs/services/speech-to-text/output.html#interim).

<table>
  <caption>Tabelle 8. Parameter 'interim_results'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parameter der JSON-Nachricht <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Nicht unterstützt
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Nicht unterstützt
    </td>
  </tr>
</table>

## keywords
{: #summary-keywords}

Ein optionales Array mit Schlüsselwortzeichenfolgen, die der Service in den Audioeingabedaten erkennt. Die Funktion für Schlüsselworterkennung ist standardmäßig inaktiviert. Weitere Informationen finden Sie im Abschnitt [Schlüsselworterkennung](/docs/services/speech-to-text/output.html#keyword_spotting).

<table>
  <caption>Tabelle 9. Parameter 'keywords'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parameter der JSON-Nachricht <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## keywords_threshold
{: #summary-keywords-threshold}

Ein optionales Doppelzeichen zwischen 0,0 und 1,0, das den Mindestschwellenwert für eine positive Schlüsselwortübereinstimmung angibt. Die Funktion für Schlüsselworterkennung ist standardmäßig inaktiviert. Weitere Informationen finden Sie im Abschnitt [Schlüsselworterkennung](/docs/services/speech-to-text/output.html#keyword_spotting).

<table>
  <caption>Tabelle 10. Parameter 'keywords_threshold'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parameter der JSON-Nachricht <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## language_customization_id
{: #summary-language-customization-id}

Eine optionale Anpassungs-ID für ein angepasstes Sprachenmodell, das Terminologie aus Ihrem Fachgebiet enthält. Standardmäßig wird kein angepasstes Modell verwendet. Weitere Informationen finden Sie im Abschnitt [Angepasste Modelle](/docs/services/speech-to-text/input.html#custom-input).

<table>
  <caption>Tabelle 11. Parameter 'language_customization_id'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für amerikanisches Englisch, britisches Englisch, brasilianisches Portugiesisch, Französisch, Deutsch, Japanisch, Koreanisch und Spanisch
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Abfrageparameter für Verbindungsanforderung mit der Methode <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## max_alternatives
{: #summary-max-alternatives}

Ein optionaler ganzzahliger Wert, der die maximale Anzahl alternativer Hypothesen angibt, die vom Service zurückgegeben werden. Standardmäßig gibt der Service eine einzige endgültige Hypothese zurück. Weitere Informationen finden Sie im Abschnitt [Maximale Anzahl Alternativen](/docs/services/speech-to-text/output.html#max_alternatives).

<table>
  <caption>Tabelle 12. Parameter 'max_alternatives'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parameter der JSON-Nachricht <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## model
{: #summary-model}

Ein optionales Modell, das die Sprache angibt, in der die Audiodaten gesprochen werden, und die Abtastfrequenz, mit der sie erfasst wurden (Breitband oder Schmalband). Standardmäßig wird `en-US_BroadbandModel` verwendet. Weitere Informationen finden Sie im Abschnitt [Sprachen und Modelle](/docs/services/speech-to-text/models.html).

<table>
  <caption>Tabelle 13. Parameter 'model'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Abfrageparameter für Verbindungsanforderung mit der Methode <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## profanity_filter
{: #summary-profanity-filter}

Ein optionaler boolescher Wert, der angibt, ob der Service vulgäre Ausdrücke aus einem Transkript filtert (zensiert). Der Standardwert `true` bedeutet, dass vulgäre Ausdrücke aus dem Transkript gefiltert werden. Weitere Informationen finden Sie im Abschnitt [Vulgäre Ausdrücke filtern](/docs/services/speech-to-text/output.html#profanity_filter).

<table>
  <caption>Tabelle 14. Parameter 'profanity_filter'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für amerikanisches Englisch
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parameter der JSON-Nachricht <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## redaction
{: #summary-redaction}

Ein optionaler boolescher Wert, der angibt, ob der Service numerische Daten mit mindestens drei aufeinanderfolgenden Ziffern aus einem Transkript schwärzt (maskiert). Wenn Sie den Parameter `redaction` auf `true` setzen, erzwingt der Service automatisch, dass der Parameter `smart_formatting` auf `true` gesetzt wird. Die Standardeinstellung `false` gibt an, dass numerische Daten nicht geschwärzt werden. Weitere Informationen finden Sie im Abschnitt [Zahlenschwärzung](/docs/services/speech-to-text/output.html#redaction).

<table>
  <caption>Tabelle 15. Parameter 'redaction'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Betaversion für amerikanisches Englisch, Japanisch und Koreanisch
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parameter der JSON-Nachricht <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## smart_formatting
{: #summary-smart-formatting}

Ein optionaler boolescher Wert, der angibt, ob der Service Datums- und Zeitangaben, Zahlen sowie Währungswerte und ähnliche Werte im endgültigen Transkript in eine konventionelle Darstellung umwandelt. Für amerikanisches Englisch wandelt die Funktion außerdem bestimmte Schlüsselwortausdrücke in Interpunktionszeichen um. Die Standardeinstellung `false` gibt an, dass keine intelligente Formatierung vorgenommen wird. Weitere Informationen finden Sie im Abschnitt [Intelligente Formatierung](/docs/services/speech-to-text/output.html#smart_formatting).

<table>
  <caption>Tabelle 16. Parameter 'smart_formatting'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Betaversion für amerikanisches Englisch, Japanisch und Spanisch
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parameter der JSON-Nachricht <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## speaker_labels
{: #summary-speaker-labels}

Ein optionaler boolescher Wert, der angibt, ob der Service markiert, welche Personen in einer Konversation mit mehreren Beteiligten welche Worte gesprochen haben. Wenn Sie den Parameter `speaker_labels` auf `true` setzen, erzwingt der Service automatisch, dass der Parameter `timestamps` auf `true` gesetzt wird. Die Standardeinstellung `false` gibt an, dass keine Sprecherbezeichnungen zurückgegeben werden. Weitere Informationen finden Sie im Abschnitt [Sprecherbezeichnungen](/docs/services/speech-to-text/output.html#speaker_labels).

<table>
  <caption>Tabelle 17. Parameter 'speaker_labels'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Betaversion für amerikanisches Englisch, Japanisch und Spanisch (Breitband- und Schmalbandmodelle) und britisches Englisch (nur Schmalbandmodell)
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parameter der JSON-Nachricht <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## timestamps
{: #summary-timestamps}

Ein optionaler boolescher Wert, der angibt, ob der Service Zeitmarken für die Wörter des Transkripts erstellt. Die Standardeinstellung `false` gibt an, dass keine Zeitmarken zurückgegeben werden. Weitere Informationen finden Sie im Abschnitt [Wortzeitmarken](/docs/services/speech-to-text/output.html#word_timestamps).

<table>
  <caption>Tabelle 18. Parameter 'timestamps'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parameter der JSON-Nachricht <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## Transfer-Encoding
{: #summary-transfer-encoding}

Ein optionaler Wert für `chunked`, der bewirkt, dass die Audiodaten per Streaming zum Service übertragen werden. Standardmäßig werden die Audiodaten alle auf einmal als Einzelübertragung gesendet. Weitere Informationen finden Sie im Abschnitt [Übertragung von Audiodaten](/docs/services/speech-to-text/input.html#transmission).

<table>
  <caption>Tabelle 19. Parameter 'Transfer-Encoding'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Nicht zutreffend; immer per Streaming
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Anforderungsheader der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Anforderungsheader der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## watson-token
{: #summary-watson-token}

Bei Verwendung von Cloud Foundry-Serviceberechtigungsnachweisen ist dieser Parameter ein optionales {{site.data.keyword.watson}}-Authentifizierungstoken zum Herstellen einer authentifizierten Verbindung zur WebSocket-Schnittstelle. Weitere Informationen finden Sie im Abschnitt [Verbindung öffnen](/docs/services/speech-to-text/websockets.html#WSopen).

<table>
  <caption>Tabelle 20. Parameter 'watson-token'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Abfrageparameter für Verbindungsanforderung mit der Methode <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Nicht unterstützt
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Nicht unterstützt
    </td>
  </tr>
</table>

## word_alternatives_threshold
{: #summary-word-alternatives-threshold}

Ein optionales Doppelzeichen zwischen 0,0 und 1,0, das den Schwellenwert angibt, ab dem der Service automatisch ähnliche Alternativen für Wörter aus den Audioeingabedaten liefert. Standardmäßig werden keine Wortalternativen zurückgegeben. Weitere Informationen finden Sie im Abschnitt [Wortalternativen](/docs/services/speech-to-text/output.html#word_alternatives).

<table>
  <caption>Tabelle 21. Parameter 'word_alternatives_threshold'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parameter der JSON-Nachricht <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## word_confidence
{: #summary-word-confidence}

Ein optionaler boolescher Wert, der angibt, ob der Service Konfidenzwerte für die Wörter des Transkripts bereitstellt. Die Standardeinstellung `false` gibt an, dass keine Konfidenzwerte für Wörter zurückgegeben werden. Weitere Informationen finden Sie im Abschnitt [Wortkonfidenz](/docs/services/speech-to-text/output.html#word_confidence).

<table>
  <caption>Tabelle 22. Parameter 'word_confidence'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parameter der JSON-Nachricht <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Abfrageparameter der Methode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## X-Watson-Authorization-Token
{: #summary-x-watson-authorization-token}

Ein optionales Authentifizierungstoken, das authentifizierte Anforderungen an den Service absetzt, ohne Ihre Serviceberechtigungsnachweise in jeden Aufruf einzufügen. Standardmäßig müssen mit jeder Anforderung Serviceberechtigungsnachweise übergeben werden. Watson-Authentifizierungstokens basieren auf Cloud Foundry-Serviceberechtigungsnachweisen, die eine Kombination aus `{benutzername}` und `{kennwort}` zum Authentifizieren verwenden.

Der Header `X-Watson-Authorization-Token` akzeptiert weder IAM-Tokens noch API-Schlüssel.
{: note}

<table>
  <caption>Tabelle 23. Parameter 'X-Watson-Authorization-Token'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Wird nicht unterstützt. Verwenden Sie den Abfrageparameter <code>watson-token</code>.
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Anforderungsheader für jede Anforderung
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Anforderungsheader für jede Anforderung
    </td>
  </tr>
</table>

## X-Watson-Learning-Opt-Out
{: #summary-x-watson-learning-opt-out}

Ein optionaler boolescher Wert, der angibt, ob die Anforderungsprotokollierung inaktiviert werden soll, die von {{site.data.keyword.IBM_notm}} standardmäßig durchgeführt wird, um den Service für zukünftige Benutzer zu verbessern. Wenn Sie nicht zulassen möchten, dass Ihre Daten von IBM für die allgemeine Verbesserung des Service verwendet werden, geben Sie für diesen Parameter <code>true</code> an. Weitere Informationen finden Sie im Abschnitt [Anforderungsprotokollierung](/docs/services/speech-to-text/input.html#logging).

<table>
  <caption>Tabelle 24. Parameter 'X-Watson-Learning-Opt-Out'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Abfrageparameter <code>x-watson-learning-opt-out</code> der Verbindungsanforderung
      <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Anforderungsheader für jede Anforderung
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Anforderungsheader für jede Anforderung
    </td>
  </tr>
</table>

## X-Watson-Metadata
{: #summary-x-watson-metadata}

Eine optionale Zeichenfolge, die eine Kunden-ID den Daten zuordnet, die für Erkennungsanforderungen übergeben werden. Der Parameter akzeptiert das Argument `customer_id={id}`. Standardmäßig wird den Daten keine Kunden-ID zugeordnet. Weitere Informationen finden Sie im Abschnitt [Informationssicherheit](/docs/services/speech-to-text/information-security.html).

<table>
  <caption>Tabelle 25. Parameter 'X-Watson-Metadata'</caption>
  <tr>
    <th>Verfügbarkeit und Verwendung</th>
    <th style="vertical-align:bottom">Beschreibung</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Verfügbarkeit**
    </td>
    <td style="text-align:left">
      Allgemein verfügbar für alle Sprachen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Abfrageparameter <code>x-watson-metadata</code> der Verbindungsanforderung
      <code>/v1/recognize</code> (das Argument muss URL-codiert sein,
      zum Beispiel `customer_id%3dmy_customer_ID`).
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Synchrones HTTP**
    </td>
    <td style="text-align:left">
      Anforderungsheader der POST-Anforderung <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **Asynchrones HTTP**
    </td>
    <td style="text-align:left">
      Anforderungsheader für <code>POST /v1/register_callback</code>-
      und <code>POST /v1/recognitions</code>-Anforderungen
    </td>
  </tr>
</table>
