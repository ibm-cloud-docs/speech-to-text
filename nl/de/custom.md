---

copyright:
  years: 2015, 2019
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

# Anpassungsschnittstelle
{: #customization}

Der {{site.data.keyword.speechtotextfull}}-Service bietet eine Anpassungsschnittstelle, mit deren Hilfe Sie die Spracherkennungsfunktionalität des Service erweitern können. Durch eine Anpassung können Sie die Genauigkeit von Spracherkennungsanforderungen verbessern, indem Sie ein Basismodell für Ihr Fachgebiet und Ihre Audiodaten anpassen. Die Anpassung ist nur bei einigen Sprachen und hier bei den verschiedenen Sprachen auf unterschiedlichen Unterstützungsstufen verfügbar; weitere Informationen finden Sie unter [Sprachunterstützung bei der Anpassung](#languageSupport).
{: shortdesc}

Die Anpassungsschnittstelle unterstützt sowohl angepasste Sprachmodelle als auch angepasste Akustikmodelle. Die Schnittstellen für die beiden Typen von angepassten Modellen sind ähnlich und unkompliziert zu verwenden. Auch die Verwendung der beiden Typen von angepassten Modellen bei einer Erkennungsanforderung ist einfach - Sie müssen lediglich die Anpassungs-ID des Modells zusammen mit der Anforderung angeben.

Die Spracherkennung funktioniert mit oder ohne angepasstes Modell identisch. Wenn Sie ein angepasstes Modell für die Spracherkennung verwenden, können Sie alle Eingabe- und Ausgabeparameter nutzen, die normalerweise bei einer Erkennungsanforderung verfügbar sind. Zusätzliche Angaben über alle verfügbaren Parameter enthält der Abschnitt [Parameterübersicht](/docs/services/speech-to-text/summary.html).

## Sprachmodellanpassung
{: #customLanguage-intro}

Der Service wurde für eine breite und allgemeine Zielgruppe entwickelt. Das Grundvokabular des Service enthält viele Wörter aus der Alltagssprache. Seine Modelle bieten für viele Anwendungen eine ausreichend genaue Erkennung. Möglicherweise kennen die Modelle jedoch spezielle Begriffe aus bestimmten Fachgebieten nicht.

Die Schnittstelle für die *Sprachmodellanpassung* kann die Genauigkeit der Spracherkennung bei Fachgebieten wie Medizin, Recht, Informationstechnologie usw. verbessern. Mithilfe der Sprachmodellanpassung können Sie das Vokabular eines Basismodells durch die Aufnahme von fachspezifischer Terminologie erweitern und anpassen.

Sie erstellen ein angepasstes Sprachmodell und fügen dann spezielle Korpora und Wörter für Ihr Fachgebiet hinzu. Sobald das angepasste Sprachmodell mit Ihrem erweiterten Vokabular trainiert wurde, können Sie es für die angepasste Spracherkennung nutzen. Der Service kann normalerweise jedes angepasste Modell in nur wenigen Minuten trainieren. Wie hoch der mit der Erstellung eines Modells verbundene Aufwand ist, richtet sich nach den Daten, die für das Modell verfügbar sind.

Weitere Informationen finden Sie in den folgenden Abschnitten:

-   [Angepasstes Sprachmodell erstellen](/docs/services/speech-to-text/language-create.html)
-   [Angepasstes Sprachmodell verwenden](/docs/services/speech-to-text/language-use.html)

## Akustikmodellanpassung
{: #customAcoustic-intro}

Analog wurde der Service mit akustischen Basismodellen entwickelt, die bei unterschiedlichen Audiomerkmalen gut funktionieren. In Fällen wie den Folgenden können Sie jedoch die Spracherkennung verbessern, indem Sie ein Grundmodell an Ihre Audiodaten anpassen:

-   Die Umgebung Ihres akustischen Kanals ist sehr speziell. Beispiele hierfür sind eine laute Umgebung, eine suboptimale Qualität oder Positionierung des Mikrofons oder eine Beeinträchtigung des Tons durch Fernfeldeffekte.
-   Die Sprechmuster der Redner sind atypisch. Beispiele hierfür sind ein ungewöhnlich schnell redender Sprecher oder Hintergrundgespräche in den Audiodaten.
-   Die Redner sprechen mit Akzent. Beispiele hierfür sind Sprecher, die nicht in ihrer Muttersprache bzw. einer Fremdsprache reden.

Die Schnittstelle für die *Akustikmodellanpassung* kann ein Basismodell an die Umgebung und die Sprecher anpassen. Sie erstellen zunächst ein angepasstes Akustikmodell und fügen dann Audiodaten (Audioressourcen) hinzu, die den akustischen Gegebenheiten der Audiodaten, die Sie transkribieren wollen, möglichst nahe kommen. Sobald das angepasste Akustikmodell mit Ihren Audioressourcen trainiert wurde, können Sie es für die angepasste Spracherkennung nutzen.

Wie lange der Service zum Trainieren des angepassten Modells benötigt, ist vom Umfang der im Modell enthaltenen Audiodaten abhängig. Normalerweise dauert das Training doppelt so lang wie die kumulierten Audiodaten. Wie hoch der mit der Erstellung eines Modells verbundene Aufwand ist, richtet sich nach den Audiodaten, die für das Modell verfügbar sind. Er ist außerdem davon abhängig, ob Sie Transkriptionen der Audiodaten verwenden.

Weitere Informationen finden Sie in den folgenden Abschnitten:

-   [Angepasstes Akustikmodell erstellen](/docs/services/speech-to-text/acoustic-create.html)
-   [Angepasstes Akustikmodell verwenden](/docs/services/speech-to-text/acoustic-use.html)

## Grammatiken
{: #grammars-intro}

Durch angepasste Sprachmodelle können Sie das Grundvokabular des Service erweitern. Mithilfe von *Grammatiken* können Sie die Wörter eingrenzen, die der Service aus diesem Vokabular erkennen kann. Wenn Sie zusammen mit einem angepassten Sprachmodell für die Spracherkennung eine Grammatik verwenden, kann der Service ausschließlich Wörter, Ausdrücke und Zeichenfolgen erkennen, die von der Grammatik erkannt werden. Da die Grammatik einen begrenzten Suchbereich für gültige Übereinstimmungen definiert, kann der Service Ergebnisse schneller und präziser liefern.

Sie fügen eine Grammatik zu einem angepassten Sprachmodell hinzu und trainieren das Modell dann genauso wie für einen Korpus. Anders als bei einem Korpus müssen Sie jedoch explizit angeben, dass während der Spracherkennung eine Grammatik mit einem angepassten Modell verwendet werden soll.

Weitere Informationen finden Sie in den folgenden Abschnitten:

-   [Grammatiken bei angepassten Sprachmodellen verwenden](/docs/services/speech-to-text/grammar.html)
-   [Grammatik zu einem angepassten Sprachmodell hinzufügen](/docs/services/speech-to-text/grammar-add.html)
-   [Grammatik bei der Spracherkennung verwenden](/docs/services/speech-to-text/grammar-use.html)

## Akustik- und Sprachanpassung kombiniert verwenden
{: #combined}

Schon die Verwendung eines angepassten Akustikmodells kann die Erkennungfunktionalität des Service verbessern. Falls jedoch für Ihre Beispielaudiodaten Transkriptionen oder zugehörige Korpora verfügbar sind, können Sie diese Daten nutzen, um die Qualität der Spracherkennung ausgehend vom angepassten Akustikmodell weiter zu verbessern.

Durch die Erstellung eines angepassten Sprachmodells, das Ihr angepasstes Akustikmodell ergänzt, können Sie die Spracherkennung verbessern, indem Sie beide Modelle zusammen einsetzen. Wenn Sie ein angepasstes Akustikmodell trainieren, können Sie ein angepasstes Sprachmodell angeben, das Transkriptionen der Audioressourcen oder ein Vokabular von fachspezifischen Wörtern in den Ressourcen enthält. Beim Transkribieren von Audiodaten akzeptiert der Service analog ein angepasstes Sprachmodell und/oder ein angepasstes Akustikmodell. Und falls Ihr angepasstes Sprachmodell eine Grammatik beinhaltet, können Sie dieses Modell und seine Grammatik zusammen mit einem angepassten Akustikmodell für die Spracherkennung verwenden.

Weitere Informationen finden Sie im folgenden Abschnitt:

-   [Angepasste Akustikmodelle und angepasste Sprachmodelle kombiniert verwenden](/docs/services/speech-to-text/acoustic-both.html)

Einige Sprachen unterstützen nicht sowohl Sprachanpassung als auch akustische Anpassung. Weitere Informationen finden Sie unter [Sprachunterstützung bei der Anpassung](#languageSupport).
{: note}

## Sprachunterstützung bei der Anpassung
{: #languageSupport}

Die Anpassung von Sprachmodellen und Akustikmodellen ist nur bei einigen Sprachen verfügbar. In der folgenden Tabelle ist jeweils die Unterstützungsstufe des Service für jede Sprache angegeben.

-   *GA* gibt an, dass die Schnittstelle für den Produktionseinsatz allgemein verfügbar ist.
-   *Beta* gibt an, dass die Schnittstelle in der Betaversion angeboten wird.
-   *Nicht unterstützt* bedeutet, dass die Schnittstelle für diese Sprache nicht verfügbar ist.

Sie können Breitband- und Schmalbandmodelle bei jeder unterstützten Sprache verwenden, für die sie verfügbar sind. Falls eine Sprache die Sprachmodellanpassung unterstützt, unterstützt sie auch Grammatiken. Eine Liste aller verfügbaren Modelle finden Sie im Abschnitt [Unterstützte Sprachmodelle](/docs/services/speech-to-text/models.html#modelsList).

<table>
  <caption>Tabelle 1. Sprachunterstützung bei der Anpassung</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 24%">
      Sprache
    </th>
    <th style="text-align:center; vertical-align:bottom; width 37%">
      Unterstützung für Anpassung von Sprachmodellen
    </th>
    <th style="text-align:center; vertical-align:bottom; width 37%">
      Unterstützung für Anpassung von Akustikmodellen
    </th>
  </tr>
  <tr>
    <td>Brasilianisches Portugiesisch</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Französisch</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Deutsch</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Japanisch</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Koreanisch</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Mandarin</td>
    <td style="text-align:center">Nicht unterstützt</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Modernes Hocharabisch</td>
    <td style="text-align:center">Nicht unterstützt</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Spanisch</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Britisches Englisch</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Amerikanisches Englisch</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
</table>

Mit den Methoden `GET /v1/models` und `GET /v1/models/{modell-id}` können Sie überprüfen, ob ein Sprachmodell die Sprachmodellanpassung unterstützt. Falls das Basismodell die Sprachmodellanpassung unterstützt, ist im Feld `supported_features` der Ausgabe der Methode für das Modell das Feld `custom_language_model` mit `true` festgelegt.

## Verwendungshinweise für die Anpassung
{: #customUsage}

Die folgenden Verwendungshinweise gelten sowohl für die Sprachmodellanpassung als auch für Akustikmodellanpassung.

### Eigentumsrecht an angepassten Modellen
{: #customOwner}

Eigner eines angepassten Modells ist die Instanz des {{site.data.keyword.speechtotextshort}}-Service, mit deren Berechtigungsnachweisen das Modell erstellt wird. Für jede Arbeit mit dem angepassten Modell müssen Sie die Serviceberechtigungsnachweise, die für diese Instanz des Service erstellt wurden, bei den Methoden der Anpassungsschnittstelle verwenden. Mit Berechtigungsnachweisen, die für andere Instanzen des Service erstellt wurden, kann das angepasste Modell weder angezeigt werden, noch ist der Zugriff auf das Modell möglich.

Alle Serviceberechtigungsnachweise, die für dieselbe Instanz des {{site.data.keyword.speechtotextshort}}-Service erhalten wurden, haben gemeinsam Zugriff auf alle angepassten Modelle, die für diese Serviceinstanz erstellt wurden. Um den Zugriff auf ein angepasstes Modell einzuschränken, erstellen Sie eine separate Instanz des Service und verwenden Sie ausschließlich die Berechtigungsnachweise für diese Serviceinstanz, um das Modell zu erstellen und damit zu arbeiten. Berechtigungsnachweise für andere Serviceinstanzen haben dann keinen Einfluss auf das Modell.

Ein Vorteil der gemeinsamen Nutzung von Eigentumsrechten durch mehrere Berechtigungsnachweise für eine Serviceinstanz besteht darin, dass Sie eine Gruppe von Berechtigungsnachweisen stornieren können, wenn sie beispielsweise beeinträchtigt wurden. Anschließend können Sie neue Berechtigungsnachweise für dieselbe Serviceinstanz erstellen und trotzdem das Eigentumsrecht an den angepassten Modellen, die mit den ursprünglichen Berechtigungsnachweisen erstellt wurden, sowie den Zugriff auf diese Modelle verwalten.

### Protokollierung von Anforderungen und Datenschutz
{: #customLogging}

Wie der Service die Anforderungsprotokollierung für Aufrufe der Anpassungsschnittstelle abwickelt, ist von der Anforderung abhängig:

-   Der Service *protokolliert keine Daten*, die zum Erstellen von angepassten Modellen verwendet werden. Wenn Sie beispielsweise in einem angepassten Sprachmodell mit Korpora und Wörtern arbeiten, müssen Sie den Anforderungsheader `X-Watson-Learning-Opt-Out` nicht festlegen. Ihre Trainingsdaten werden in keinem Fall verwendet, um die Basismodelle des Service zu verbessern.
-   Der Service *protokolliert Daten*, wenn ein angepasstes Modell bei einer Erkennungsanforderung verwendet wird. Wenn Sie die Protokollierung für Erkennungsanforderungen verhindern wollen, müssen Sie den Anforderungsheader `X-Watson-Learning-Opt-Out` auf `true` setzen.

Weitere Informationen finden Sie unter [Protokollierung von Anforderungen](/docs/services/speech-to-text/input.html#logging).

### Informationssicherheit
{: #customSecurity}

Sie können Daten, die für angepasste Sprachmodelle und angepasste Akustikmodelle hinzugefügt oder aktualisiert werden, eine Kunden-ID zuordnen. Zum Zuordnen einer Kunden-ID zu Korpora, angepassten Wörtern, Grammatiken und Audioressourcen übergeben Sie den Header `X-Watson-Metadata` mit den folgenden Methoden:

-   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
-   `POST /v1/customizations/{customization_id}/words`
-   `PUT /v1/customizations/{customization_id}/words/{word_name}`
-   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`
-   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

Bei Bedarf können Sie danach die Daten mit der Methode `DELETE /v1/user_data` löschen. Weitere Informationen finden Sie im Abschnitt [Informationssicherheit](/docs/services/speech-to-text/information-security.html).
