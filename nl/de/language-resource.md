---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-12"

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

# Mit Korpora und angepassten Wörtern arbeiten
{: #corporaWords}

Sie können ein angepasstes Sprachmodell mit Wörtern bestücken, indem Sie Korpora und Grammatiken zu dem Modell hinzufügen oder indem Sie angepasste Wörter direkt hinzufügen.
{: shortdesc}

-   **Korpora:** Die empfohlene Vorgehensweise zum Füllen eines angepassten Sprachmodells mit Wörtern ist das Hinzufügen mindestens eines Korpus zu dem Modell. Beim Hinzufügen eines Korpus analysiert der Service die Datei und fügt alle gefundenen neuen Wörter automatisch zu dem angepassten Modell hinzu. Beim Hinzufügen eines Korpus zu einem angepassten Modell kann der Service fachspezifische Wörter aus dem Kontext extrahieren und dadurch bessere Transkriptionsergebnisse liefern. Weitere Informationen finden Sie im Abschnitt [Mit Korpora arbeiten](#workingCorpora).
-   **Grammatiken:** Sie können Grammatiken zu einem angepassten Modell hinzufügen, um die Spracherkennung auf die Wörter oder Ausdrücke zu begrenzen, die von einer Grammatik erkannt werden. Beim Hinzufügen einer Grammatik zu einem Modell fügt der Service alle gefundenen neuen Wörter automatisch zu dem Modell hinzu (wie beim Hinzufügen von Korpora). Weitere Informationen finden Sie im Abschnitt [Grammatiken mit angepassten Sprachmodellen verwenden](/docs/services/speech-to-text/grammar.html).
-   **Einzelne Wörter:** Sie können auch einzelne angepasste Wörter direkt zu einem Modell hinzufügen. Der Service fügt die Wörter auf die gleiche Weise zu dem Modell hinzu wie neue Wörter aus Korpora oder Grammatiken. Wenn Sie ein Wort direkt hinzufügen, können Sie mehrere Aussprachevarianten angeben und festlegen, wie das Wort angezeigt werden soll. Außerdem können Sie vorhandene Wörter aktualisieren, um die aus Korpora oder Grammatiken extrahierten Definitionen zu ändern oder zu erweitern. Weitere Informationen finden Sie im Abschnitt [Mit angepassten Wörtern arbeiten](#workingWords).

Unabhängig von der verwendeten Vorgehensweise speichert der Service alle Wörter, die Sie zu einem angepassten Sprachmodell hinzufügen, in der Wörterressource des Modells.

## Die Wörterressource
{: #wordsResource}

Die *Wörterressource* enthält alle Wörter, die Sie aus Korpora bzw. Grammatiken oder direkt hinzufügen. Der Zweck der Wörterressource besteht darin, die Aussprache und Schreibweise von Wörtern zu definieren, die im Basisvokabular des Service nicht enthalten sind. Anhand der Definitionen kann der Service festlegen, wie diese *vokabularexternen Wörter* (Out-Of-Vocabulary-Wörter, OOV-Wörter) transkribiert werden.

Die Wörterressource enthält die folgenden Informationen zu jedem OOV-Wort. Der Service erstellt die Definitionen für die aus Korpora und Grammatiken extrahierten Wörter. Sie geben die Merkmale der angepassten Wörter an, die Sie direkt hinzufügen oder ändern.

-   `word`: Die Schreibweise für das Wort, das aus einem Korpus oder einer Grammatik stammt oder von Ihnen direkt hinzugefügt wurde. 
-   `sounds_like`: Die Aussprache des Wortes. Bei Wörtern, die aus Korpora und Grammatiken extrahiert wurden, geht der Service aufgrund der jeweiligen Sprachregeln davon aus, dass die angegebene Aussprache zutrifft. In vielen Fällen entspricht die Aussprache der Schreibweise im Feld `word`.

    Sie können das Feld `sounds_like` verwenden, um die Aussprache des Wortes zu ändern. Das Feld kann auch verwendet werden, um mehrere Aussprachevarianten für ein Wort anzugeben. Weitere Informationen finden Sie im Abschnitt [Das Feld 'sounds_like' verwenden](#soundsLike).
-   `display_as`: Gibt die Schreibweise für das Wort an, die vom Service in Transkripts verwendet wird. Dieses Feld gibt an, wie das Wort angezeigt werden soll. In den meisten Fällen stimmt die Schreibweise mit dem Wert aus dem Feld `word` überein.

    Sie können das Feld `display_as` verwenden, um eine andere Schreibweise für das Wort anzugeben. Weitere Informationen finden Sie im Abschnitt [Das Feld 'display_as' verwenden](#displayAs).
-   `source`: Gibt an, wie das Wort zur Wörterressource hinzugefügt wurde. Wenn das Wort von dem Service aus einem Korpus oder einer Grammatik extrahiert wurde, wird in diesem Feld der Name der betreffenden Ressource angegeben. Da das gleiche Wort vom Service in mehreren Ressourcen gefunden werden kann, werden in diesem Feld gegebenenfalls mehrere Korpus- oder Grammatiknamen aufgelistet. Das Feld enthält die Zeichenfolge `user`, wenn das betreffende Wort von Ihnen direkt hinzugefügt oder geändert wurde.

Wenn Sie Änderungen an der Wörterressource eines Modells vornehmen, müssen Sie das Modell trainieren, damit die Änderungen beim Transkribieren verwendet werden. Weitere Informationen finden Sie im Abschnitt [Angepasstes Sprachmodell trainieren](/docs/services/speech-to-text/language-create.html#trainModel-language).

## Wie viele Daten brauche ich?
{: #wordsResourceAmount}

Die für ein effizientes angepasstes Sprachmodell erforderliche Datenmenge hängt von vielen Faktoren ab. Wie viele Wörter für ein angepasstes Modell oder eine angepasste Anwendung hinzugefügt werden sollten, lässt sich nicht genau beziffern.

Je nach Anwendungsfall kann die Qualität des Modells schon durch das direkte Hinzufügen weniger Wörter verbessert werden. Das Hinzufügen von vokabularexternen Wörtern aus einem Korpus, das die Wörter in dem Kontext darstellt, in dem Sie in Audiodaten vorkommen, kann die Transkriptionsgenauigkeit ganz erheblich verbessern. Weitere Informationen finden Sie im Abschnitt [Mit Korpora arbeiten](#workingCorpora).

Der Service gibt folgende Grenzwerte für die Anzahl der Wörter vor, die Sie zu einem angepassten Sprachmodell hinzufügen können: 

-   Sie können maximal 90.000 vokabularexterne Wörter (OOV-Wörter) zur Wörterressource eines angepassten Modells hinzufügen. Dazu zählen OOV-Wörter aus allen Quellen (Korpora, Grammatiken und von Ihnen direkt hinzugefügte Wörter).
-   Sie können maximal 10.000.000 Wörter aus allen Quellen zu einem angepassten Modell hinzufügen. Dazu zählen alle Wörter, sowohl OOV-Wörter als auch Wörter aus dem Basisvokabular des Service und Wörter, die aus Korpora und Grammatiken stammen. Im Falle von Korpora kann der Service an diesen zusätzlichen Wörtern zugleich erkennen, in welchem Kontext die OOV-Wörter vorkommen. Dies macht Korpora zu einem besonders effektiven Werkzeug, um die Erkennungsgenauigkeit zu verbessern.

Eine umfangreiche Wörterressource kann die Latenzzeiten bei der Spracherkennung verlängern. Wie ausgeprägt dieser Effekt ist, lässt sich jedoch nur schwer quantifizieren oder vorhersagen. Wie die erforderliche Datenmenge für ein effizientes angepasstes Modell hängt auch die Beeinträchtigung der Leistung durch eine umfangreiche Wörterressource von vielen verschiedenen Faktoren ab. Testen Sie Ihr angepasstes Modell mit unterschiedlichen Datenvolumen, um die Leistungswerte für Ihr Modell und die zugehörigen Daten zu ermitteln.

## Mit Korpora arbeiten
{: #workingCorpora}

Mit der Methode `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` können Sie ein Korpus zu einem angepassten Modell hinzufügen. Ein Korpus ist eine einfache Textdatei mit Beispielsätzen aus dem jeweiligen Fachgebiet. Das folgende Beispiel zeigt ein Korpusfragment für das Gesundheitswesen. Eine Korpusdatei ist in der Regel sehr viel länger.

```
Am I at risk for health problems during travel?
Some people are more likely to have health problems when traveling outside the United States.
How Is Coronary Microvascular Disease Treated?
If you're diagnosed with coronary MVD and also have anemia, you may benefit from treatment for that condition.
Anemia is thought to slow the growth of cells needed to repair damaged blood vessels.
What causes autoimmune hepatitis?
A combination of autoimmunity, environmental triggers, and a genetic predisposition can lead to autoimmune hepatitis.
What research is being done for Spinal Cord Injury?
The National Institute of Neurological Disorders and Stroke NINDS conducts spinal cord research in its laboratories at the National Institutes of Health NIH.
NINDS also supports additional research through grants to major research institutions across the country.
Some of the more promising rehabilitation techniques are helping spinal cord injury patients become more mobile.
What is Osteogenesis imperfecta OI?
. . .
```
{: codeblock}

Die Spracherkennung basiert auf der Analyse von Audiodaten mithilfe statistischer Algorithmen. Wörter aus einem angepassten Modell konkurrieren mit Wörtern aus dem Basisvokabular und mit anderen Wörtern aus dem Modell. (Außerdem beeinflussen Faktoren wie Störanteile in den Audiodaten und Akzente der Sprecher die Qualität der Transkription.)

Die Genauigkeit der Transkription kann sehr stark von den Definitionen der Wörter in einem Modell und von den Aussprachevarianten der Sprecher abhängen. Um die Genauigkeit eines Service zu verbessern, sollten Sie mithilfe von Korpora möglichst viele Verwendungsbeispiele für OOV-Wörter im fachspezifischen Kontext bereitstellen. Das wiederholte Vorkommen von OOV-Wörtern in Korpora kann sich vorteilhaft auf die Qualität des angepassten Sprachmodells auswirken. Welche Aussprachevarianten aus Korpora übernommen werden, hängt davon ab, wie die Wörter von den Sprechern in den zu untersuchenden Audiodaten vermutlich ausgesprochen werden. Je mehr hinzugefügte Sätze den Kontext wiedergeben, in dem Sprecher die Wörter aus dem jeweiligen Fachgebiet verwenden, umso besser die Erkennungsgenauigkeit des Service.

Der Service wendet nicht nur einen einfachen Algorithmus für Wortübereinstimmungen an. Die Transkription des Service basiert auf dem Kontext, in dem die Wörter verwendet werden. Beim Parsing eines Korpus bezieht der Service Informationen zu n-Grammen (Bigramme, Trigramme usw.) aus den im Korpus enthaltenen Sätzen in das angepasste Modell ein. Mithilfe dieser Informationen kann der Service die Audiodaten noch genauer transkribieren. Das erklärt auch, warum das Trainieren eines angepassten Modells mithilfe von Kopora einen größeren Mehrwert liefert als das Trainieren mit angepassten Wörtern.

Beispiel: Buchhalter greifen auf eine allgemeine Gruppe von Standards und Verfahren zurück, die als GAAP (Generally Accepted Accounting Principles) bezeichnet werden. Geben Sie daher beim Erstellen eines angepassten Modells für ein Fachgebiet aus dem Finanzwesen Sätze an, die den Begriff GAAP im Kontext verwenden. Mithilfe dieser Sätze kann der Service besser zwischen allgemeinen Ausdrücken wie "the gap between them is small" und fachspezifischen Ausdrücken wie "GAAP provides guidelines for measuring and disclosing financial information" unterscheiden.

In der Regel ist es von Vorteil, wenn Korpora Wörter in verschiedenen Kontexten enthalten. Auf diese Weise kann das Erlernen von Wörtern in dem Service verbessert werden. Wenn die Wörter von den Benutzern jedoch nur in wenigen Kontexten verwendet werden, trägt das Anzeigen der Wörter in anderen Kontexten nicht zu einer besseren Qualität des angepassten Modells bei, da die Wörter von den Sprechern nicht in diesen anderen Kontexten verwendet werden. Wenn Sprecher häufig denselben Ausdruck verwenden, kann das wiederholte Auftreten dieses Ausdrucks in Korpora die Qualität des Modells verbessern. (In manchen Fällen kann sich auch das direkte Hinzufügen weniger angepasster Wörter in einem angepassten Modell positiv auswirken.)

### Korpustextdatei vorbereiten
{: #prepareCorpus}

Gehen Sie wie folgt vor, um eine Korpustextdatei vorzubereiten:

-   Stellen Sie eine einfache Textdatei mit UTF-8-Codierung bereit, wenn die Datei Nicht-ASCII-Zeichen enthält. Der Service setzt die UTF-8-Codierung voraus, wenn solche Zeichen vorkommen.

    Stellen Sie sicher, dass Sie die Zeichencodierung Ihrer Korpustextdateien kennen. Der Service behält die in den Textdateien verwendete Codierung bei. Beim Arbeiten mit den Wörtern im angepassten Sprachmodell müssen Sie diese Codierung verwenden. Weitere Informationen finden Sie im Abschnitt [Zeichencodierung](#charEncoding).
    {: important}
-   Verwenden Sie für die Wörter im Korpus eine konsistente Großschreibung. In der Wörterressource muss die Groß-/Kleinschreibung beachtet werden. Verwenden Sie die gemischte Groß-/Kleinschreibung und die durchgängige Großschreibung nur, wenn dies ausdrücklich gewünscht ist.
-   Fügen Sie jeden Satz aus dem Korpus in eine eigene Zeile ein und beenden Sie jede Zeile mit einem Wagenrücklauf. Mehrere Sätze in einer Zeile können die Genauigkeit beeinträchtigen.
-   Fügen Sie Personennamen als getrennte Einheiten in separaten Zeilen hinzu. Fügen Sie die einzelnen Wörter eines Namens nicht in separaten Zeilen oder als einzelne angepasste Wörter hinzu und geben Sie nicht mehrere Namen in derselben Zeile des Korpus an. Das folgende Beispiel veranschaulicht die richtige Vorgehensweise, um die Erkennungsgenauigkeit für drei Namen zu verbessern: 

    ```
    Gakuto Kutara
    Sebastian Leifson
    Malcolm Ingersol
    ```
    {: codeblock}

    Geben Sie zusätzliche Kontextinformationen an (falls verfügbar). Beispiel: `Doktor Sebastian Leifson` oder `Vorsitzender Malcolm Ingersol`. Wie bei allen Wörtern gilt auch hier, dass die wiederholte Nennung von Namen, falls möglich in verschiedenen Kontexten, die Erkennungsgenauigkeit verbessern kann.
-   Vermeiden Sie Schreibfehler. Der Service geht bei Schreibfehlern davon aus, dass es sich um neue Wörter handelt. Wenn Sie die Schreibfehler nicht korrigieren, werden diese vom Service zum Vokabular des Modells hinzugefügt. Denken Sie an den Spruch: *Wo Müll reingeht, kommt auch Müll raus!*

Mehr Sätze erzielen eine größere Genauigkeit. Im Service gilt für jedes Modell eine Obergrenze von maximal 10.000.000 Wörtern und 90.000 OOV-Wörtern aus allen Quellen zusammen.

### Was passiert, wenn ich eine Korpusdatei hinzufüge?
{: #parseCorpus}

Wenn Sie eine Korpusdatei hinzufügen, analysiert der Service den Inhalt der Datei. Alle neuen Wörter werden extrahiert und als OOV-Wörter zur Wörterressource des angepassten Modells hinzugefügt. Um den größtmöglichen Bedeutungsgehalt aus dem Inhalt zu destillieren, fügt der Service Tokens ein und analysiert die aus einer Korpusdatei gelesenen Daten. In den folgenden Abschnitten wird die Vorgehensweise des Service beim Parsing einer Korpusdatei für die einzelnen unterstützten Sprachen erläutert.

#### Parsing für Englisch, Französisch, Deutsch, Spanisch und brasilianisches Portugiesisch
{: #corpusLanguages}

Die folgenden Beschreibungen gelten für amerikanisches und britisches Englisch, Französisch, Deutsch, Spanisch und brasilianisches Portugiesisch.

-   Zahlen werden in entsprechende Wörter umgewandelt:
    -   *Englisch:* `500` wird zu `five hundred` und `0.15` wird zu `zero point fifteen`.
    -   *Französisch:* `500` wird zu `cinq cents` und `0,15` wird zu <code>z&eacute;ro quinze</code>.
    -   *Deutsch:* `500` wird zu <code>f&uuml;nfhundert</code> und `0,15` wird zu <code>null komma f&uuml;nfzehn</code>.
    -   *Spanisch:* `500` wird zu `quinientos` und `0,15` wird zu `cero coma quince`.
    -   *Brasilianisches Portugiesisch:* `500` wird zu `quinhentos` und `0,15` wird zu `zero ponto quinze`.
-   Tokens, die bestimmte Symbole enthalten, werden in aussagefähige Zeichenfolgedarstellungen umgewandelt, wie in den folgenden Beispielen gezeigt:
    -   Ein Dollarzeichen (`$`) mit einer Zahl wird wie folgt umgewandelt:
        -   *Englisch:* `$100` wird zu `one hundred dollars`.
        -   *Französisch:* `$100` wird zu `cent dollar`.
        -   *Deutsch:* `$100` und `100$` werden zu `einhundert dollar`.
        -   *Spanisch:* `$100`  und `100$` werden zu <code>cien d&oacute;lares</code> (oder `cien pesos`, wenn der Dialekt `es-LA` angegeben ist).
        -   *Brasilianisches Portugiesisch:* `$100` und `100$` werden zu <code>cem d&oacute;lares</code>.
    -   Ein Euro-Zeichen (<code>&euro;</code>) mit einer Zahl wird wie folgt umgewandelt:
        -   *Englisch:* <code>&euro;100</code> wird zu `one hundred euros`.
        -   *Französisch:* <code>&euro;100</code> wird zu `cent euros`.
        -   *Deutsch:* <code>&euro;100</code> und <code>100&euro;</code> werden zu `einhundert euro`.
        -   *Spanisch:* <code>&euro;100</code> und <code>100&euro;</code> werden zu `cien euros`.
        -   *Brasilianisches Portugiesisch:* <code>&euro;100</code> und <code>100&euro;</code> werden zu `cem euros`.
    -   Ein Prozentzeichen (`%` ) mit einer Zahl davor wird wie folgt umgewandelt:
        -   *Englisch:* `100%` wird zu `one hundred percent`.
        -   *Französisch:* `100%` wird zu `cent pourcent`.
        -   *Deutsch:* `100 %` wird zu `einhundert prozent`.
        -   *Spanisch:* `100%` wird zu `cien por ciento`.
        -   *Brasilianisches Portugiesisch:* `100%` wird zu `cem por cento`.

    Diese Liste ist nicht vollständig. Der Service nimmt für andere Zeichen bei Bedarf ähnliche Anpassungen vor.
-   Verarbeitung nicht alphanumerischer Zeichen sowie Interpunktions- und Sonderzeichen entsprechend dem Kontext. Beispiel: Der Service entfernt ein Dollarzeichen (`$`) oder Euro-Symbol (<code>&euro;</code>), wenn danach keine Zahl folgt. Diese Verarbeitung erfolgt kontextabhängig und konsistent für alle unterstützten Sprachen.
-   Ignorieren von Ausdrücken, die in runde Klammern `( )`, spitze Klammern `< >`, eckige Klammern `[ ]` oder geschweifte Klammern `{ }` eingeschlossen sind.

#### Parsing für Japanisch
{: #corpusLanguages-jaJP}

-   Alle Zeichen werden in Zeichen mit voller Breite umgewandelt.
-   Zahlen werden in die entsprechenden Wörter umgewandelt. Beispiel: `500` wird zu <code>&#20116;&#30334;</code> und `0,15` wird zu <code>&#12295;&#12539;&#19968;&#20116;</code>.
-   Tokens, die Symbole enthalten, werden nicht in die entsprechenden Zeichenfolgen umgewandelt. Beispiel: `100 %` wird zu <code>&#30334;&#65285;</code>.
-   Die Interpunktion wird nicht automatisch entfernt. {{site.data.keyword.IBM_notm}} empfiehlt dringend, die Interpunktion zu entfernen, wenn Ihre Anwendung transkriptionsbasiert ist und nicht diktatbasiert.

#### Parsing für Koreanisch
{: #corpusLanguages-koKR}

-   Zahlen werden in die entsprechenden Wörter umgewandelt. Beispiel: <code>10</code> wird zu <code>&#49901;</code>.
-   Die Interpunktions- und Sonderzeichen `- ( ) * : . , ' "` werden entfernt. Für Koreanisch werden jedoch nicht alle Interpunktions- und Sonderzeichen entfernt, die für andere Sprachen entfernt werden. Beispiele:
    -   Ein Punkt (`.`) wird nur entfernt, wenn er am Ende einer Eingabezeile steht.
    -   Die Tilde (`~`) wird nicht entfernt.
    -   Unicode-Breitzeichen werden weder entfernt noch anderweitig verarbeitet (z. B. die Auslassungspunkte <code>&#8230;</code>).

    {{site.data.keyword.IBM_notm}} empfiehlt generell, Interpunktionszeichen, Sonderzeichen und Unicode-Breitzeichen zu entfernen, bevor eine Korpusdatei verarbeitet wird.
-   Es werden keine Ausdrücke entfernt oder ignoriert, die in runde Klammern `( )`, spitze Klammern `< >`, eckige Klammern `[ ]` oder geschweifte Klammern `{ }` eingeschlossen sind.
-   Tokens, die bestimmte Symbole enthalten, werden in aussagefähige Zeichenfolgedarstellungen umgewandelt, wie in den folgenden Beispielen gezeigt:
    -   `24 %` wird zu <code>&#51060;&#49901;&#49324;&#54140;&#49468;&#53944;</code>.
    -   `$ 10` wird zu <code>&#49901;&#45804;&#47084;</code>.

    Diese Liste ist nicht vollständig. Der Service nimmt für andere Zeichen bei Bedarf ähnliche Anpassungen vor.
-   Für Ausdrücke, die aus lateinischen (englischen) Zeichen oder aus einer Mischung aus Hangul-Zeichen und lateinischen Zeichen bestehen, erstellt der Service OOV-Wörter, die genau den Ausdrücken in der Korpusdatei entsprechen. Außerdem werden gleich klingende Aussprachevarianten für die Wörter auf der Basis von Hangul-Transkriptionen erstellt.
   - Dem OOV-Wort `London` wird eine gleich klingende Aussprache von <code>&#47088;&#45912;</code> zugeordnet.
   - Dem OOV-Wort <code>IBM&#54856;&#54168;&#51060;&#51648;</code> wird eine gleich klingende Aussprache von <code>&#50500;&#51060;&#32;&#48708;&#32;&#50656;&#32;&#54856;&#54168;&#51060;&#51648;</code> zugeordnet.

## Mit angepassten Wörtern arbeiten
{: #workingWords}

Mit den Methoden `POST /v1/customizations/{customization_id}/words` und `PUT /v1/customizations/{customization_id}/words/{word_name}` können Sie neue Wörter zu der Wörterressource eines angepassten Modells hinzufügen. Sie können mit diesen Methoden auch ein Wort in einer Wörterressource ändern oder erweitern.

Sie können die Methoden beispielsweise verwenden, um einen Schreibfehler oder einen anderen Fehler in einem Wort zu korrigieren, das aus einem Korpus hinzugefügt wird. Außerdem können Sie gleich klingende Aussprachevarianten für ein vorhandenes Wort hinzufügen. Wenn Sie ein vorhandenes Wort ändern, überschreiben die neuen Daten, die Sie bereitstellen, die vorhandene Definition des Wortes in der Wörterressource. Die Regeln zum Hinzufügen eines Wortes gelten auch für das Ändern eines vorhandenen Wortes.

### Zeichencodierung
{: #charEncoding}

In der Regel werden die meisten angepassten Wörter aus Korpora hinzugefügt. Stellen Sie sicher, dass Sie die Zeichencodierung kennen, die in den Textdateien für Ihre Korpora verwendet wird. Der Service behält die in den Textdateien verwendete Codierung bei. 

Beim Arbeiten mit den einzelnen Wörtern im angepassten Sprachmodell müssen Sie diese Codierung verwenden. Wenn Sie in der Methode `GET`, `PUT` oder `DELETE /v1/customizations/{customization_id}/words/{word_name}` ein Wort angeben, muss das Element `word_name`, das Sie in der URL übergeben, URL-codiert sein, wenn das Wort Nicht-ASCII-Zeichen enthält.

In der folgenden Tabelle wird der scheinbar gleiche Buchstabe in der ASCII-Codierung und in der UTF-8-Codierung dargestellt. Das ASCII-Zeichen kann in einer URL als `z` übergeben werden. Das UTF-8-Zeichen muss als `%EF%BD%9A` übergeben werden.

<table>
  <caption>Tabelle 1. Beispiele für Zeichencodierung</caption>
  <tr>
    <th style="text-align:left">Buchstabe</th>
    <th style="text-align:center">Codierung</th>
    <th style="text-align:center">Wert</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      `z`
    </td>
    <td style="text-align:center">
      ASCII
    </td>
    <td style="text-align:center">
      `0x7a` (`7a`)
    </td>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      <code>&#xff5a;</code>
    </td>
    <td style="text-align:center">
      UTF-8, hexadezimal
    </td>
    <td style="text-align:center">
      `0xEF 0xBD 0x9A` (`efbd9a`)
    </td>
  </tr>
</table>

### Verwendung des Felds 'sounds_like'
{: #soundsLike}

Das Feld `sounds_like` gibt an, wie ein Wort von Sprechern ausgesprochen wird. In der Standardeinstellung fügt der Service die geschriebene Form des Wortes automatisch in das Feld ein. Sie können bis zu fünf Aussprachevarianten für ein Wort angeben, das unterschiedlich ausgesprochen werden kann. Ziehen Sie die Verwendung des Felds für folgende Zwecke in Betracht:

-   *Aussprachevarianten für Akronyme angeben.* Beispiel: Das Akronym `NCAA` kann so ausgesprochen werden, wie es geschrieben wird, oder es kann umgangssprachlich als *N. C. double A.* ausgesprochen werden. Im folgenden Beispiel werden diese beiden gleich klingenden Aussprachevarianten für das Wort `NCAA` hinzugefügt:

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

-   *Fremdwörter verarbeiten.* Beispiel: Das französische Wort <code>gar&ccedil;on</code> enthält ein Zeichen, das in der englischen Sprache nicht vorkommt. Sie können die gleich klingende Variante `gaarson` hinzufügen, in der <code>&ccedil;</code> durch `s` ersetzt wird, um dem Service mitzuteilen, wie Sprecher der englischen Sprache das Wort aussprechen würden.

Bei der Spracherkennung werden statistische Algorithmen für die Analyse von Audiodaten verwendet, d. h. durch bloßes Hinzufügen eines Wortes kann nicht sichergestellt werden, dass das Wort vom Service mit hinreichender Genauigkeit transcodiert wird. Berücksichtigen Sie beim Hinzufügen auch die Aussprache des hinzugefügten Wortes. Verwenden Sie das Feld `sounds_like`, um verschiedene Aussprachevarianten für ein Wort anzugeben. Die folgenden Abschnitte enthalten sprachspezifische Richtlinien zum Angeben von gleich klingenden Aussprachevarianten.

#### Richtlinien für Englisch (amerikanisches und britisches Englisch)
{: #wordLanguages-enUS-enGB}

*Richtlinien, die für amerikanisches und britisches Englisch gelten*

-   Verwenden Sie die Zeichen des englischen Alphabets: `a-z` und `A-Z`.
-   Verwenden Sie tatsächliche oder erfundene Wörter, die in Englisch aussprechbar sind, für schwer auszusprechende Wörter. Beispiel: `shuchesnie` für das Wort `Sczcesny`.
-   Ersetzen Sie Buchstaben, die in der englischen Sprache nicht vorkommen, durch vergleichbare englische Buchstaben. Beispiel: `s` für <code>&ccedil;</code> oder `ny` für <code>&ntilde;</code>.
-   Ersetzen Sie akzentuierte Zeichen durch nicht akzentuierte Zeichen. Beispiel: `a` für <code>&agrave;</code> oder `e` für <code>&egrave;</code>.
-   Sie können mehrere Wörter durch Leerzeichen getrennt angeben, der Service erzwingt jedoch eine maximale Länge von insgesamt 40 Zeichen (ohne Leerzeichen).

*Richtlinien, die nur für amerikanisches Englisch gelten*

-   Verwenden Sie zum Aussprechen eines einzelnen Buchstabens den betreffenden Buchstaben, gefolgt von einem Punkt. Wenn nach dem Punkt ein weiteres Zeichen folgt, muss zwischen dem Punkt und dem nachfolgenden Zeichen ein Leerzeichen stehen. Beispiel: `N. C. A. A.` und *nicht* `N.C.A.A.`
-   Verwenden Sie für Zahlen den entsprechenden Wortlaut. Beispiel: `seventy-five` für `75`.

*Richtlinien, die nur für britisches Englisch gelten*

-   In gleich klingenden Aussprachevarianten für britisches Englisch dürfen **keine** Punkte oder Gedankenstriche verwendet werden.
-   Verwenden Sie zum Aussprechen eines einzelnen Buchstabens den betreffenden Buchstaben, gefolgt von einem Leerzeichen. Beispiel: `N C A A` und *nicht* `N. C. A. A.`, `N.C.A.A.` oder `NCAA`.
-   Verwenden Sie für Zahlen den entsprechenden Wortlaut ohne Gedankenstriche. Beispiel: `seventy five` für `75`.

#### Richtlinien für Französisch, Deutsch und brasilianisches Portugiesisch
{: #wordLanguages-esES-frFR}

-   In gleich klingenden Aussprachevarianten dürfen **keine** Gedankenstriche verwendet werden.
-   Verwenden Sie die alphabetischen Zeichen aus der jeweiligen Sprache: `a-z` und `A-Z` sowie die zulässigen akzentuierten Zeichen.
-   Verwenden Sie zum Aussprechen eines einzelnen Buchstabens den betreffenden Buchstaben, gefolgt von einem Punkt. Wenn nach dem Punkt ein weiteres Zeichen folgt, muss zwischen dem Punkt und dem nachfolgenden Zeichen ein Leerzeichen stehen. Beispiel: `N. C. A. A.` und *nicht* `N.C.A.A.`
-   Verwenden Sie tatsächliche oder erfundene Wörter, die in der jeweiligen Sprache aussprechbar sind, für schwer auszusprechende Wörter.
-   Verwenden Sie für Zahlen den entsprechenden Wortlaut ohne Gedankenstriche. Verwenden Sie für `75` zum Beispiel Folgendes:
    -   *Französisch:* `soixante quinze`
    -   *Deutsch:* <code>f&uuml;nfundsiebzig</code>
    -   *Spanisch:* `setenta y cinco`
    -   *Brasilianisches Portugiesisch:* `setenta e cinco`
-   Sie können mehrere Wörter, getrennt durch Leerzeichen angeben, der Service erzwingt jedoch eine maximale Länge von insgesamt 40 Zeichen (ohne Leerzeichen).

#### Richtlinien für Japanisch
{: #wordLanguages-jaJP}

-   Verwenden Sie ausschließlich Katakana-Zeichen mit voller Breite unter Angabe des Dehnungssymbols <code>&#8213;</code> (*chou-on* oder &#38263;&#38899; in Japanisch). Verwenden Sie keine Zeichen mit halber Breite.
-   Verwenden Sie Lautkombinationen (*yoh-on* oder &#25303;&#38899; in Japanisch) nur in den folgenden Silbenkontexten:

    <code>&#12452;&#12455;</code>, <code>&#12454;&#12451;</code>, <code>&#12454;&#12455;</code>, <code>&#12454;&#12457;</code>, <code>&#12461;&#12451;</code>, <code>&#12461;&#12515;</code>, <code>&#12461;&#12517;</code>, <code>&#12461;&#12519;</code>, <code>&#12462;&#12515;</code>, <code>&#12462;&#12517;</code>, <code>&#12462;&#12519;</code>, <code>&#12463;&#12449;</code>, <code>&#12463;&#12451;</code>, <code>&#12463;&#12455;</code>, <code>&#12463;&#12457;</code>,<br/>
<code>&#12464;&#12449;</code>, <code>&#12464;&#12457;</code>, <code>&#12471;&#12451;</code>, <code>&#12471;&#12455;</code>, <code>&#12471;&#12515;</code>, <code>&#12471;&#12517;</code>, <code>&#12471;&#12519;</code>, <code>&#12472;&#12451;</code>, <code>&#12472;&#12455;</code>, <code>&#12472;&#12515;</code>, <code>&#12472;&#12517;</code>, <code>&#12472;&#12519;</code>, <code>&#12473;&#12451;</code>, <code>&#12474;&#12451;</code>, <code>&#12481;&#12455;</code>,<br/>
<code>&#12481;&#12515;</code>, <code>&#12481;&#12517;</code>, <code>&#12481;&#12519;</code>, <code>&#12482;&#12455;</code>, <code>&#12482;&#12515;</code>, <code>&#12482;&#12517;</code>, <code>&#12482;&#12519;</code>, <code>&#12484;&#12449;</code>, <code>&#12484;&#12451;</code>, <code>&#12484;&#12455;</code>, <code>&#12484;&#12457;</code>, <code>&#12486;&#12451;</code>, <code>&#12486;&#12517;</code>, <code>&#12487;&#12451;</code>, <code>&#12487;&#12515;</code>,<br/>
<code>&#12487;&#12517;</code>, <code>&#12487;&#12519;</code>, <code>&#12488;&#12453;</code>, <code>&#12489;&#12453;</code>, <code>&#12491;&#12455;</code>, <code>&#12491;&#12515;</code>, <code>&#12491;&#12517;</code>, <code>&#12491;&#12519;</code>, <code>&#12498;&#12515;</code>, <code>&#12498;&#12517;</code>, <code>&#12498;&#12519;</code>, <code>&#12499;&#12515;</code>, <code>&#12499;&#12517;</code>, <code>&#12499;&#12519;</code>, <code>&#12500;&#12451;</code>,<br/>
<code>&#12500;&#12515;</code>, <code>&#12500;&#12517;</code>, <code>&#12500;&#12519;</code>, <code>&#12501;&#12449;</code>, <code>&#12501;&#12451;</code>, <code>&#12501;&#12455;</code>, <code>&#12501;&#12457;</code>, <code>&#12501;&#12517;</code>, <code>&#12511;&#12515;</code>, <code>&#12511;&#12517;</code>, <code>&#12511;&#12519;</code>, <code>&#12522;&#12451;</code>, <code>&#12522;&#12455;</code>, <code>&#12522;&#12515;</code>, <code>&#12522;&#12517;</code>,<br/>
<code>&#12522;&#12519;</code>, <code>&#12532;&#12449;</code>, <code>&#12532;&#12451;</code>, <code>&#12532;&#12455;</code>, <code>&#12532;&#12457;</code>, <code>&#12532;&#12517;</code>

-   Verwenden Sie nach einem assimilierten Laut (*soku-on* oder &#20419;&#38899; in Japanisch) nur die folgenden Silben:

    <code>&#12496;</code>, <code>&#12499;</code>, <code>&#12502;</code>, <code>&#12505;</code>, <code>&#12508;</code>, <code>&#12481;</code>, <code>&#12481;&#12455;</code>, <code>&#12481;&#12515;</code>, <code>&#12481;&#12517;</code>, <code>&#12481;&#12519;</code>, <code>&#12480;</code>, <code>&#12487;</code>, <code>&#12487;&#12451;</code>, <code>&#12489;</code>, <code>&#12489;&#12453;</code>, <code>&#12501;</code>,<br/>
<code>&#12501;&#12449;</code>, <code>&#12501;&#12451;</code>, <code>&#12501;&#12455;</code>, <code>&#12501;&#12457;</code>, <code>&#12460;</code>, <code>&#12462;</code>, <code>&#12464;</code>, <code>&#12466;</code>, <code>&#12468;</code>, <code>&#12495;</code>, <code>&#12498;</code>, <code>&#12504;</code>, <code>&#12507;</code>, <code>&#12472;</code>, <code>&#12472;&#12455;</code>, <code>&#12472;&#12515;</code>,<br/>
<code>&#12472;&#12517;</code>, <code>&#12472;&#12519;</code>, <code>&#12459;</code>, <code>&#12461;</code>, <code>&#12463;</code>, <code>&#12465;</code>, <code>&#12467;</code>, <code>&#12461;&#12515;</code>, <code>&#12461;&#12517;</code>, <code>&#12461;&#12519;</code>, <code>&#12497;</code>, <code>&#12500;</code>, <code>&#12503;</code>, <code>&#12506;</code>, <code>&#12509;</code>, <code>&#12500;&#12515;</code>,<br/>
<code>&#12500;&#12517;</code>, <code>&#12500;&#12519;</code>, <code>&#12469;</code>, <code>&#12473;</code>, <code>&#12475;</code>, <code>&#12477;</code>, <code>&#12471;</code>, <code>&#12471;&#12455;</code>, <code>&#12471;&#12515;</code>, <code>&#12471;&#12517;</code>, <code>&#12471;&#12519;</code>, <code>&#12479;</code>, <code>&#12486;</code>, <code>&#12488;</code>, <code>&#12484;</code>, <code>&#12470;</code>,<br/>
<code>&#12474;</code>, <code>&#12476;</code>, <code>&#12478;</code>

-   Verwenden Sie <code>&#12531;</code> nicht als erstes Zeichen in einem Wort. Verwenden Sie beispielsweise <code>&#12454;&#12540;&#12531;&#12488;</code> anstelle von <code>&#12531;&#12540;&#12488;</code>, das ungültig ist.
-   Viele zusammengesetzte Wörter bestehen aus *Präfix + Substantiv* oder aus *Substantiv + Suffix*. Das Basisvokabular des Service enthält die meisten der häufig vorkommenden Komposita (z. B. <code>&#x9577;&#x96FB;&#x8A71;</code> und <code>&#x53E4;&#x65B0;&#x805E;</code>), jedoch nicht die selten vorkommenden Komposita. Wenn Ihr Korpus viele zusammengesetzte Wörter enthält, fügen Sie diese im ersten Anpassungsschritt jeweils als ein Wort hinzu. Beispiel: <code>&#x53E4;&#x925B;&#x7B46;</code> kommt in allgemeinen japanischen Texten nicht häufig vor. Wenn Sie dieses Wort häufig verwenden, fügen Sie es zu Ihrem angepassten Modell hinzu, um die Transkriptionsgenauigkeit zu verbessern.
-   Verwenden Sie keinen assimilierten Laut als abschließendes Element.

#### Richtlinien für Koreanisch
{: #wordLanguages-koKR}

-   Verwenden Sie koreanische Hangul-Zeichen, -Symbole und -Silben.
-   Sie können auch die Zeichen aus dem lateinischen (englischen) Alphabet verwenden: `a-z` und `A-Z`.
-   Verwenden Sie keine Zeichen oder Symbole, die in den oben angegebenen Gruppen nicht enthalten sind.

### Verwendung des Felds 'display_as'
{: #displayAs}

Im Feld `display_as` wird angegeben, wie ein Wort in einer Aufzeichnung angezeigt wird. Dieses Feld ist für den Fall vorgesehen, dass der Service eine Zeichenfolge anzeigen soll, die von der Schreibweise des Wortes abweicht. Sie können beispielsweise angeben, dass das Wort `hhonors` als `HHonors` dargestellt werden soll, unabhängig davon, ob es wie `hilton honors` oder wie `h honors` klingt.

```bash
curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

Sie können beispielsweise auch angeben, dass das Wort `IBM` als <code>IBM&trade;</code> angezeigt werden soll.

<pre><code class="language-bash">curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"I. B. M.\"], \"display_as\":\"IBM&#8482;\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IBM"</code></pre>

#### Interaktion mit intelligenter Formatierung und Zahlenschwärzung
{: #displaySmart}

Wenn Sie die Parameter `smart_formatting` oder `redaction` mit einer Erkennungsanforderung verwenden, sollten Sie berücksichtigen, dass der Service die intelligente Formatierung und das Schwärzen auf ein Wort anwendet, bevor das Feld `display_as` für das Wort berücksichtigt wird. Es kann hilfreich sein, mit den Ergebnissen zu experimentieren, um sicherzustellen, dass sich diese Funktionen nicht nachteilig auf die Darstellung Ihrer angepassten Wörter auswirken. Gegebenenfalls kann es erforderlich werden, angepasste Wörter hinzufügen, um diese Auswirkungen zu berücksichtigen.

Angenommen, Sie fügen ein angepasstes Wort `one` ein und geben im Feld `display_as` den Wert `one` ein. Die Funktion für intelligente Formatierung ändert das Wort `one` in die Zahl `1` und der Wert aus dem Feld 'display-as' wird nicht angewendet. Um dieses Problem zu umgehen, können Sie ein angepasstes Wort für die Zahl `1` hinzufügen und denselben Wert aus dem Feld `display_as` auf dieses Wort anwenden.

Weitere Informationen zum Arbeiten mit diesen Funktionen finden Sie in den Abschnitten [Intelligente Formatierung](/docs/services/speech-to-text/output.html#smart_formatting) und [Zahlenschwärzung](/docs/services/speech-to-text/output.html#redaction).

### Was passiert, wenn ich ein angepasstes Wort hinzufüge oder ändere?
{: #parseWord}

Wie der Service auf eine Anforderung zum Hinzufügen oder Ändern eines angepassten Wortes reagiert, hängt von den Werten ab, die Sie angeben, und davon, ob das Wort im Basisvokabular des Service vorhanden ist.

<table>
  <caption>Tabelle 1. Angepasste Wörter mit verschiedenen Feldern hinzufügen</caption>
  <tr>
    <th style="width:20%; text-align:center; vertical-align:bottom">Das Feld <code>sounds_like</code> ist...</th>
    <th style="width:20%; text-align:center; vertical-align:bottom">Das Feld <code>display_as</code> ist...</th>
    <th style="text-align:left; vertical-align:bottom">Antwort</th>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Nicht angegeben
    </td>
    <td style="text-align:center; vertical-align:top">
      Nicht angegeben</td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Wenn das Wort nicht im Basisvokabular des Service enthalten
          ist,</em> fügt der Service in das Feld <code>sounds_like</code>
          die eigene Aussprache für das Wort ein. Im Feld
          <code>display_as</code> wird der Wert aus dem Feld
          <code>word</code> angegeben.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Wenn das Wort im Basisvokabular des Service enthalten ist,</em>
          lässt der Service die Felder <code>sounds_like</code> und
          <code>display_as</code> leer. Diese Felder sind nur leer, wenn
          das Wort im Basisvokabular des Service enthalten ist. Das
          Vorhandensein des Wortes in der Wörterressource des Modells hat
          zwar keine negative Auswirkung, aber es ist unnötig.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Angegeben
    </td>
    <td style="text-align:center; vertical-align:top">
      Nicht angegeben
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Wenn das Feld <code>sounds_like</code> gültig ist,</em> gibt der
          Service im Feld <code>display_as</code> den Wert aus dem Feld
          <code>word</code> an.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Wenn das Feld <code>sounds_like</code> ungültig ist:</em>
          <ul style="margin-left:20px; padding:0px">
            <li style="margin:10px 0px; line-height:120%">
              Die Methode <code>POST
                /v1/customizations/{customization_id}/words</code> fügt
              ein Feld <code>error</code> für das Wort zur
              Wörterressource des Modells hinzu.
            </li>
            <li style="margin:10px 0px; line-height:120%">
              Die Methode <code>PUT
                /v1/customizations/{customization_id}/words/{word_name}</code>
              schlägt mit einem Antwortcode 400 und einer Fehlernachricht fehl. Der
              Service fügt das Wort nicht zur Wörterressource hinzu.
            </li>
          </ul>
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Nicht angegeben
    </td>
    <td style="text-align:center; vertical-align:top">
      Angegeben
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Wenn das Wort nicht im Basisvokabular des Service enthalten
          ist,</em> fügt der Service in das Feld <code>sounds_like</code>
          die eigene Aussprache für das Wort ein und behält den im Feld
          <code>display_as</code> angegebenen Wert bei.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Wenn das Wort im Basisvokabular des Service enthalten ist,</em>
          lässt der Service das Feld <code>sounds_like</code> leer und behält
          den im Feld <code>display_as</code> angegebenen Wert bei.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Angegeben
    </td>
    <td style="text-align:center; vertical-align:top">
      Angegeben
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Wenn das Feld <code>sounds_like</code> gültig ist,</em> setzt der
          Service die Felder <code>sounds_like</code> und <code>display_as</code>
          auf die angegebenen Werte.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Wenn das Feld <code>sounds_like</code> ungültig ist,</em>
          antwortet der Service wie in dem Fall, in dem das Feld
          <code>sounds_like</code> angegeben ist und das Feld
          <code>display_as</code> nicht.
        </li>
      </ul>
    </td>
  </tr>
</table>

## Wörterressource prüfen
{: #validateModel}

Untersuchen Sie insbesondere beim Hinzufügen eins Korpus zu einem angepassten Sprachmodell oder beim gleichzeitigen Hinzufügen mehrerer angepasster Wörter die OOV-Wörter in der Wörterressource des Modells.

-   *Suchen Sie nach Schreibfehlern und anderen Fehlern.* Besonders beim Hinzufügen von Korpora, die sehr umfangreich sein können, sind Fehler keine Seltenheit. Schreibfehler in einer Korpus- oder Grammatikdatei haben die unbeabsichtigte Nebenwirkung, dass neue Wörter in der Wörterressource eines Modells hinzugefügt werden. Gleiches gilt für fehlerhaft formatierte HTML-Tags in einer Korpusdatei.
-   *Überprüfen Sie die gleich klingenden Aussprachevarianten.* Der Service generiert automatisch gleich klingende Aussprachevarianten für OOV-Wörter. In den meisten Fällen reichen diese Aussprachevarianten aus. Bei Wörtern, die über ungewöhnliche Schreibweisen verfügen oder schwer auszusprechen sind, sowie bei Akronymen und Fachbegriffen empfiehlt es sich jedoch, die Richtigkeit der Aussprachevarianten zu überprüfen.

Um ein Wort für ein angepasstes Modell zu prüfen und gegebenenfalls zu korrigieren (unabhängig davon, wie es zur Wörterressource hinzugefügt wurde), können Sie die folgenden Methoden verwenden:

-   Listen Sie alle Wörter aus einem angepassten Modell mithilfe der Methode `GET /v1/customizations/{customization_id}/words` auf oder fragen Sie ein einzelnes Wort mit der Methode `GET /v1/customizations/{customization_id}/words/{word_name}` ab. Weitere Informationen finden Sie im Abschnitt [Wörter aus angepasstem Sprachmodell auflisten](/docs/services/speech-to-text/language-words.html#listWords).
-   Ändern Sie Wörter in einem angepassten Modell, um Fehler zu beheben oder um Werte für die Felder 'sounds-like' oder 'display-as' hinzuzufügen, mit der Methode `POST /v1/customizations/{customization_id}/words` bzw. mit der Methode `PUT /v1/customizations/{customization_id}/words/{word_name}`. Weitere Informationen finden Sie im Abschnitt [Mit angepassten Wörtern arbeiten](#workingWords).
-   Löschen Sie nicht dazugehörige Wörter, die irrtümlich eingefügt wurden (z. B. durch Schreibfehler oder andere Fehler in einerm Korpus) mithilfe der Methode  `DELETE /v1/customizations/{customization_id}/words/{word_name}`. Weitere Informationen finden Sie im Abschnitt [Wort aus einem angepassten Sprachmodell löschen](/docs/services/speech-to-text/language-words.html#deleteWord).
    -   Wenn das Wort aus einem Korpus extrahiert wurde, können Sie stattdessen die Korpustextdatei aktualisieren, um den Fehler zu korrigieren, und anschließend die Datei unter Verwendung des Parameters `allow_overwrite` in der Methode `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` erneut laden. Weitere Informationen finden Sie im Abschnitt [Korpus zum angepassten Sprachmodell hinzufügen](/docs/services/speech-to-text/language-create.html#addCorpus). 
    -   Wenn das Wort aus einer Grammatik extrahiert wurde, können Sie die Grammatikdatei aktualisieren, um den Fehler zu korrigieren, und anschließend die Datei unter Verwendung des Parameters `allow_overwrite` in der Methode `POST /v1/customizations/{customization_id}/grammars/{grammar_kname}` erneut laden. Weitere Informationen finden Sie im Abschnitt [Grammatik zu einem angepassten Sprachmodell hinzufügen](/docs/services/speech-to-text/grammar-add.html#addGrammar).
