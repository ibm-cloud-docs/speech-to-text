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

# Audioformate
{: #audio-formats}

Der {{site.data.keyword.speechtotextfull}}-Service kann Sprache aus Audiodaten in vielen unterschiedlichen Formaten extrahieren.

-   Falls Sie mit Audiodaten sowie deren Beschreibung und Angabe nicht vertraut sind, lesen Sie zum leichteren Einstieg zunächst den Abschnitt [Merkmale von Audiodaten und Terminologie](#terminology).

-   Wenn Sie bereits Erfahrungen in der Verwendung von Audiodaten besitzen, finden Sie im Abschnitt [Unterstützte Audioformate](#formats) ausführliche Informationen zu den Formaten, die der Service unterstützt.

In den Abschnitten [Datengrenzwerte und Komprimierung](#limits), [Konvertierung von Audiodaten](#conversion) und [Tipps für die Verbesserung der Spracherkennung](#audioTips) am Ende dieses Dokuments erfahren Sie, wie Sie den Service optimal nutzen können.

## Merkmale von Audiodaten und Terminologie
{: #terminology}

Zur Beschreibung der Merkmale von Audiodaten bei den unterschiedlichen Formaten wird die folgende Terminologie verwendet.

### Abtastfrequenz
{: #samplingRate}

Die *Abtastfrequenz* (oder Abtastrate) gibt die Anzahl der pro Sekunde durchgeführten Abtastungen an. Die Abtastfrequenz wird in Hertz (Hz) oder Kilohertz (kHz) gemessen. Beispielsweise entspricht eine Rate von 16.000 Abtastungen pro Sekunde der Frequenz 16.000 Hz (bzw. 16 kHz). Beim {{site.data.keyword.speechtotextshort}}-Service geben Sie die Abtastfrequenz Ihrer Audiodaten mithilfe eines Modells an:

-   *Breitbandmodelle* werden für Audiodaten verwendet, die mit einer Frequenz von nicht unter 16 kHz abgetastet werden. {{site.data.keyword.IBM}} empfiehlt dies für reaktionsfähige Echtzeitanwendungen (beispielsweise für Anwendungen mit Sprache im Live-Modus).
-   *Schmalbandmodelle* werden für Audiodaten verwendet, die mit einer Frequenz von nicht weniger als 8 kHz abgetastet werden; diese Frequenz wird normalerweise für Telefonton verwendet.

Bei den meisten Sprachen und Formaten unterstützt der Service sowohl Breitband- als auch Schmalbandformate. Die Abtastfrequenz Ihrer Audiodaten wird vom Service automatisch an das von Ihnen angegebene Modell angepasst, bevor der Service die Spracherkennung durchführt.

-   Bei Breitbandmodellen konvertiert der Service Audiodaten, die mit höheren Abtastfrequenzen aufgezeichnet wurden, in 16 kHz.
-   Bei Schmalbandmodellen konvertiert er die Audiodaten in 8 kHz.

Audiodaten mit 44 kHz können Sie theoretisch mit einem Breitbandmodell oder einem Schmalbandmodell senden, was jedoch die Größe der Audiodaten unnötig erhöht. Um so viele Audiodaten wie möglich senden zu können, sollten Sie die Abtastfrequenz Ihrer Audiodaten an das verwendete Modell angleichen. Audiodaten, die mit einer geringeren Frequenz als der internen Abtastfrequenz des Modells abgetastet werden, werden vom Service nicht akzeptiert.

#### Hinweise zu Audioformaten
{: #samplingRateNotes}

-   Bei den Formaten `audio/alaw`, `audio/l16` und `audio/mulaw` müssen Sie die Frequenz Ihrer Audiodaten angeben.
-   Bei den Formaten `audio/basic` und `audio/g729` unterstützt der Service ausschließlich Schmalbandaudiodaten.

#### Weitere Informationen
{: #samplingRateMore}

-   Weitere Informationen zu Abtastfrequenzen finden Sie unter der Adresse [en.wikipedia.org/wiki/Sampling. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Sampling){: new_window} Wählen Sie dort *Sampling (signal processing)* aus.
-   Zusätzliche Angaben über die Modelle, die der Service für jede unterstützte Sprache bietet, enthält der Abschnitt [Sprachen und Modelle](/docs/services/speech-to-text/models.html).

### Bitübertragungsrate
{: #bitRate}

Die *Bitübertragungsrate* gibt die Anzahl der pro Sekunde gesendeten Datenbit an. Die Bitübertragungsrate für einen Audiodatenstrom wird in Kilobit pro Sekunde (Kb/s) gemessen. Die Bitübertragungsrate wird aus der Abtastfrequenz und der Anzahl der pro Abtastung gespeicherten Bit berechnet. Für die Spracherkennung empfiehlt {{site.data.keyword.IBM}} die Aufzeichnung von 16 Bit pro Abtastung für die Audiodaten.

Beispiel: Audiodaten, die eine Breitbandabtastfrequenz von 16 kHz und 16 Bit pro Abtastung verwenden, besitzen eine Bitübertragungsrate von 256 Kb/s: `(16,000 * 16) / 1000`.

#### Weitere Informationen
{: #bitRateMore}

-   Weitere Informationen zu Bitübertragungsraten finden Sie unter der Adresse [en.wikipedia.org/wiki/Bit_rate. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Bit_rate){: new_window}
-   Eine allgemeine Beschreibung von Abtastfrequenzen und Bitübertragungsraten können Sie auf den Seiten [What are bit rates? ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://www.richardfarrar.com/what-are-bit-rates/){: new_window} und [Choosing bit rates for podcasts ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://www.richardfarrar.com/choosing-bit-rates-for-podcasts/){: new_window} nachlesen.

### Komprimierung
{: #compression}

*Komprimierung* wird von vielen Audioformaten verwendet, um die Größe der Audiodaten zu reduzieren. Durch die Komprimierung wird die Anzahl der pro Abtastung gespeicherten Bit und somit die Bitübertragungsrate verringert. Einige Formate verwenden keine Komprimierung; bei den meisten Formaten wird jedoch einer der Basiskomprimierungstypen eingesetzt:

-   Die *verlustfreie* Komprimierung reduziert die Größe der Audiodaten ohne Qualitätsverlust, allerdings mit einem normalerweise geringen Komprimierungsverhältnis.
-   Die *verlustbehaftete* Komprimierung verringert die Größe der Audiodaten um das bis zu Zehnfache, hierbei gehen jedoch einige Daten und ein bestimmter Anteil der Qualität unwiederbringlich verloren.

Beim {{site.data.keyword.speechtotextshort}}-Service können Sie problemlos die verlustbehaftete Komprimierung nutzen und so die Menge der Audiodaten maximieren, die mit einer Erkennungsanforderung an den Service gesendet werden können. Da die menschliche Stimme im Dynamikbereich stärker eingeschränkt ist als beispielsweise Musik, kann Sprache eine viel geringere Bitübertragungsrate als andere Typen von Audiodaten verwenden. Für die Spracherkennung empfiehlt {{site.data.keyword.IBM}} die Verwendung von 16 Bit pro Abtastung für die Audiodaten und die Anwendung eines Formats, bei dem die Audiodaten komprimiert werden.

#### Hinweise zu Audioformaten
{: #compressionNotes}

-   Die Formate `audio/ogg` und `audio/webm` sind Container, deren Komprimierung sich nach dem Codec richtet, mit dem die Daten codiert werden (Opus oder Vorbis).
-   Das Format `audio/wav` ist ein Container, der nicht komprimierte, verlustfreie oder verlustbehaftete Daten enthalten kann.

#### Weitere Informationen
{: #compressionMore}

-   Weitere Informationen zur Komprimierung von Audiodaten finden Sie unter der Adresse [en.wikipedia.org/wiki/Data_compression#Audio. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Data_compression#Audio){: new_window}
-   Zusätzliche Angaben darüber, wie Sie die Menge der mit einer Anforderung gesendeten Daten durch den Einsatz der Datenkomprimierung erhöhen können, enthält der Abschnitt [Datengrenzwerte und Komprimierung](#limits).

### Kanäle
{: #channels}

*Kanäle* geben die Anzahl der Datenströme in der Tonaufzeichnung an.

-   Audiodaten des Typs *Monaural* (kurz 'Mono') besitzen nur einen einzigen Kanal.
-   Audiodaten des Typs *Stereophonisch* (kurz 'Stereo') besitzen in der Regel zwei Kanäle.

Der {{site.data.keyword.speechtotextshort}}-Service akzeptiert Audiodaten mit maximal 16 Kanälen. Da für die Spracherkennung nur ein einziger Kanal genutzt wird, setzt der Service während der Transcodierung Audiodaten mit mehreren Kanälen auf Einkanalmono herunter.

#### Hinweise zu Audioformaten
{: #channelsNotes}

-   Beim Format `audio/l16` müssen Sie die Anzahl der Kanäle angeben, falls die Audiodaten mehr als einen Kanal enthalten.
-   Beim Format `audio/wav` akzeptiert der Service Audiodaten mit maximal neun Kanälen.

#### Weitere Informationen
{: #channelsMore}

-   Weitere Informationen zu Audiokanälen finden Sie unter der Adresse [en.wikipedia.org/wiki/Audio_signal. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Audio_signal){: new_window}

### Endianness
{: #endianness}

Die *Endianness* gibt an, wie Datenbyte durch die zugrundeliegende Computerarchitektur organisiert werden:

-   Bei Verwendung von *Big Endian* werden Daten anhand des höchstwertigen Bit angeordnet.
-   Bei Verwendung von *Little Endian* werden Daten anhand des niedrigstwertigen Bit angeordnet.

Der {{site.data.keyword.speechtotextshort}}-Service erkennt die Endianess der eingehenden Audiodaten automatisch.

#### Hinweise zu Audioformaten
{: #endiannessNotes}

-   Beim Format `audio/l16` können Sie bei Bedarf die Endianess angeben und die automatische Erkennung inaktivieren.

#### Weitere Informationen
{: #endiannessMore}

-   Weitere Informationen zur Endianess finden Sie unter der Adresse [en.wikipedia.org/wiki/Endianness. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Endianness){: new_window}

### Tonfrequenz
{: #frequency}

Die *Tonfrequenz* bezeichnet den Bereich der hörbaren Frequenzen in den Audiodaten. Als für Menschen hörbare Standardfrequenz wird allgemein 20 bis 20.000 Hertz akzeptiert. Mithilfe einer spektrografischen Analyse können Sie ein Spektrogramm erzeugen, das die enthaltenen Frequenzen Ihrer Audiodaten sichtbar macht.

Die Abtastfrequenz, die auf Audiodaten angewendet wird, ist normalerweise doppelt so hoch wie die höchste Frequenz der Audiodaten. Eine Abtastfrequenz von 16 kHz bedeutet beispielsweise, dass die höchste Frequenz des abgetasteten Tonsignals 8 kHz beträgt. Dies wurde bei der Erstellung der Modelle für den Service berücksichtigt.

-   Die Schmalbandmodelle wurden mit Audiodaten erstellt, die mit 8 kHz abgetastet werden. Schmalbandmodelle erwarten Informationen in einem Bereich, der kleiner-gleich 4 kHz ist.
-   Die Breitbandmodelle wurden mit Audiodaten erstellt, die mit 16 kHz abgetastet werden. Breitbandmodelle erwarten Informationen im Bereich von 4 bis 8 kHz.

Die Trainingsdaten für die Modelle werden aus unterschiedlichen Kanälen (bei Schmalbandmodellen der Telefonie) abgeleitet. Die Modelle bilden die Merkmale der Kanäle ab, mit denen sie trainiert wurden.

#### Erhöhung der Abtastfrequenz
{: #upsampling}

Bei der auch *Upsampling* genannten Erhöhung der Abtastfrequenz wird zwar die Abtastfrequenz der Audiodaten heraufgesetzt, aber es werden keine neuen Informationen in die Audiodaten aufgenommen. Es entsteht ein Näherungswert für das Tonsignal, das durch eine Abtastung der Audiodaten mit einer höheren Frequenz erhalten werden würde. Die Größe der Audiodaten nimmt hierbei zu.

Die Informationen in Audiodaten, die ursprünglich mit einer Schmalbandfrequenz erstellt wurden, ist auf den Bereich von 0 bis 4 kHz beschränkt. Bei einer Erhöhung der Abtastfrequenz für Schmalbandaudiodaten ist es unwahrscheinlich, dass sich die Genauigkeit der Spracherkennung verbessert. Falls Sie die Abtastfrequenz für Schmalbandaudiodaten erhöhen, fehlen Informationen in dem Bereich, den die Breitbandmodelle erwarten. Darüber hinaus weichen die im erwarteten Bereich einer Schmalbandabtastung gefundenen Daten qualitativ von den Informationen ab, die in demselben Bereich einer Breitbandabtastung gefunden werden. Die Erhöhung der Abtastfrequenz führt somit eigentlich zu einer verminderten Genauigkeit bei der Erkennung.

Bei einer Breitbandabtastfrequenz von 16 kHz liegt die erwartete Höchstfrequenz im abgetasteten Tonsignal bei 8 kHz. Sie müssen daher das ursprüngliche Signal bei 8 kHz filtern, bevor Sie es mit einer Frequenz von 16
kHz abtasten. Andernfalls findet aufgrund eines Phänomens, das als *Aliasing* bezeichnet wird, eine Verschlechterung statt. Informationen zu den Ursachen finden Sie auf der Seite [Nyquist frequency. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Nyquist_frequency){: new_window}

In diesem Zusammenhang kann es sinnvoll sein, sich zum Vergleich die Wiedergabe eines VHS-Bandes auf einem großen Flachbildschirm mit HDTV vorzustellen. Das Bild ist unscharf, weil die Wiedergabe eines Bandes auf einem hochauflösenden Gerät keine neuen Informationen zum Datenstrom hinzufügen kann. Sie macht lediglich das Format mit dem besseren Gerät kompatibel. Gleiches gilt für die Erhöhung der Abtastfrequenz von Audiodaten.

#### Verringerung der Abtastfrequenz
{: #downsampling}

Bei diesem Vorgang, der auch als *Downsampling* bezeichnet wird, wird die Abtastfrequenz der Audiodaten verringert. Es entsteht ein Näherungswert für das Tonsignal, das durch eine Abtastung der Audiodaten mit einer niedrigeren Frequenz erhalten werden würde. Durch die Verringerung der Abtastfrequenz werden zwar keine Informationen aus dem Tonsignal entfernt, aber die Größe der Audiodaten verringert sich.

Das Downsampling von Audiodaten kann in einigen Fällen wirkungsvoll sein. Falls beispielsweise die Abtastfrequenz Ihrer Audiodaten größer als 8 kHz ist *und* eine spektrografische Überprüfung ergibt, dass kein Frequenzinhalt größer als 4 kHz ist, kann die Verringerung der Abtastfrequenz für die Audiodaten auf 8 kHz sinnvoll sein.

#### Weitere Informationen
{: #frequencyMore}

-   Weitere Informationen zur Tonfrequenz finden Sie unter der Adresse [https://en.wikipedia.org/wiki/Audio_frequency. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Audio_frequency){: new_window}
-   Zusätzliche Angaben über das Upsampling können Sie unter der Adresse [https://en.wikipedia.org/wiki/Upsampling ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Upsampling){: new_window} nachlesen.
-   Näheres zum Downsampling ist unter der Adresse [https://en.wikipedia.org/wiki/Downsampling ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Downsampling_%28signal_processing%29){: new_window} zu finden.

## Unterstützte Audioformate
{: #formats}

Tabelle 1 vermittelt Ihnen einen Überblick über die vom Service unterstützten Audioformate.

-   In der Spalte *Audioformat und Komprimierung* ist das jeweilige Format und die von ihm unterstützte Komprimierung angegeben. Mit einer einzigen WebSocket- oder synchronen HTTP-Anforderung können Sie Audiodaten mit einer maximalen Größe von 100 MB an den Service senden. Bei Verwendung eines Formats, das eine Komprimierung unterstützt, können Sie die Größe Ihrer Audiodaten verringern, um die Menge der an den Service übertragbaren Daten zu erhöhen. Weitere Informationen enthält der Abschnitt [Datengrenzwerte und Komprimierung](#limits).
-   In der Spalte *Spezifikation des Inhaltstyps* ist angegeben, ob Sie den Header `Content-Type` oder einen funktional entsprechenden Parameter verwenden müssen, um das Format (MIME-Typ) der an den Service gesendeten Audiodaten anzugeben. Sie können zwar das Audioformat für jede Anforderung angeben, aber dies ist nicht immer nötig:
    -   Bei den meisten Formaten ist der Inhaltstyp optional. Sie können den Inhaltstyp weglassen oder `application/octet-stream` angeben, damit der Service das Format automatisch erkennt.
    -   Bei anderen Formaten ist der Inhaltstyp erforderlich. Diese Formate stellen die Informationen, die der Service zur automatischen Erkennung Ihres Formats benötigt (z. B. die Abtastfrequenz), nicht zur Verfügung.
-   In den weiteren Spalten sind für jedes Format zusätzliche *erforderliche Parameter* und *optionale Parameter* aufgeführt. In den folgenden Abschnitten finden Sie weitere Informationen zu diesen Parametern.

<table>
  <caption>Tabelle 1. Übersicht über unterstützte Audioformate</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom">
      Audioformat<br>und Komprimierung
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Spezifikation<br/>des Inhaltstyps
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Erforderliche<br/>Parameter
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Optionale<br/>Parameter
    </th>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/alaw](#alaw)<br/>Verlustbehaftet
    </td>
    <td style="text-align:center">
      Erforderlich
    </td>
    <td style="text-align:center">
      <code>rate={ganzzahl}</code>
    </td>
    <td style="text-align:center">
      Keine
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/basic](#basic)<br/>Verlustbehaftet
    </td>
    <td style="text-align:center">
      Erforderlich
    </td>
    <td style="text-align:center">
      Keine
    </td>
    <td style="text-align:center">
      Keine
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/flac](#flac)<br/>Verlustfrei
    </td>
    <td style="text-align:center">
      Optional
    </td>
    <td style="text-align:center">
      Keine
    </td>
    <td style="text-align:center">
      Keine
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/g729](#g729)<br/>Verlustbehaftet
    </td>
    <td style="text-align:center">
      Optional
    </td>
    <td style="text-align:center">
      Keine
    </td>
    <td style="text-align:center">
      Keine
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/l16](#l16)<br/>Keine
    </td>
    <td style="text-align:center">
      Erforderlich
    </td>
    <td style="text-align:center">
      <code>rate={ganzzahl}</code>
    </td>
    <td style="text-align:center">
      <code>channels={ganzzahl}</code><br/>
      <code>endianness=big-endian</code><br/>
      <code>endianness=little-endian</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/mp3](#mp3)<br/>
      [audio/mpeg](#mp3)<br/>Verlustbehaftet
    </td>
    <td style="text-align:center">
      Optional
    </td>
    <td style="text-align:center">
      Keine
    </td>
    <td style="text-align:center">
      Keine
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/mulaw](#mulaw)<br/>Verlustbehaftet
    </td>
    <td style="text-align:center">
      Erforderlich
    </td>
    <td style="text-align:center">
      <code>rate={ganzzahl}</code>
    </td>
    <td style="text-align:center">
      Keine
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/ogg](#ogg)<br/>Verlustbehaftet
    </td>
    <td style="text-align:center">
      Optional
    </td>
    <td style="text-align:center">
      Keine
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/wav](#wav)<br/>Keine, verlustfrei<br/>oder verlustbehaftet
    </td>
    <td style="text-align:center">
      Optional
    </td>
    <td style="text-align:center">
      Keine
    </td>
    <td style="text-align:center">
      Keine
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/webm](#webm)<br/>Verlustbehaftet
    </td>
    <td style="text-align:center">
      Optional
    </td>
    <td style="text-align:center">
      Keine
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
</table>

Wenn Sie bei den HTTP-Schnittstellen den Befehl `curl` verwenden, um eine Spracherkennungsanforderung auszugeben, müssen Sie entweder das Audioformat mit dem Header `Content-Type` angeben, oder `"Content-Type: application/octet-stream"` bzw. `"Content-Type:"` angeben. Falls Sie den Header komplett weglassen, verwendet `curl` den Standardwert `application/x-www-form-urlencoded`.
{: important}

### Format 'audio/alaw'
{: #alaw}

*A-Law* (`audio/alaw`) ist ein verlustbehaftetes Einkanalaudioformat. Es verwendet einen ähnlichen Algorithmus wie den durch die Formate `audio/basic` und `audio/mulaw` angewendeten U-Law-Algorithmus. Der A-Law-Algorithmus erzeugt jedoch andere Signalmerkmale. Bei Verwendung dieses Formats macht der Service einen zusätzlichen Parameter für die Formatspezifikation erforderlich.

<table>
  <caption>Tabelle 2. Parameter für das Format `audio/alaw`</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Parameter
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Beschreibung
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Erforderlich</em>
    </td>
    <td>
      Eine ganze Zahl, mit der die Abtastfrequenz angegeben wird, in der
      die Audiodaten aufgezeichnet wurden. Geben Sie beispielsweise den folgenden
      Parameter für Audiodaten an, die mit 8 kHz aufgezeichnet wurden:<br/><br/>
      <code>audio/alaw;rate=8000</code>
    </td>
  </tr>
</table>

Weitere Informationen finden Sie unter der Adresse [en.wikipedia.org/wiki/A-law_algorithm. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/A-law_algorithm){: new_window}

### Format 'audio/basic'
{: #basic}

Das *Basisaudioformat* (`audio/basic`) ist ein verlustbehaftetes Einkanalaudioformat, das unter Verwendung von mit 8 kHz abgetasteten 8-Bit-U-Law-Daten (oder Mu-Law-Daten) codiert wird. Dieses Format ist der kleinste gemeinsame Nenner für die Angabe des Medientyps von Audiodaten. Der Service unterstützt die Verwendung von Dateien im Format `audio/basic` nur bei Schmalbandmodellen.

Weitere Informationen finden Sie auf der von der Internet Engineering Task Force (IETF) betriebenen Seite [Request for Comment (RFC) 2046 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://tools.ietf.org/html/rfc2046){: new_window} sowie unter der Adresse [iana.org/assignments/media-types/audio/basic. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://www.iana.org/assignments/media-types/audio/basic){: new_window}

### Format 'audio/flac'
{: #flac}

*Free Lossless Audio Codec*, kurz 'FLAC' (`audio/flac`), ist ein verlustfreies Audioformat. Weitere Informationen finden Sie unter der Adresse [en.wikipedia.org/wiki/FLAC. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/FLAC){: new_window}

### Format 'audio/g729'
{: #g729}

*G.729* (`audio/g729`) ist ein verlustbehaftetes Format, das bei 8 kHz codierte Daten unterstützt. Der Service unterstützt G.729 nur mit Annex D und nicht mit Annex J. Die Verwendung von Dateien im Format `audio/g729` wird durch den Service nur bei Schmalbandmodellen unterstützt. Weitere Informationen finden Sie unter der Adresse [en.wikipedia.org/wiki/G.729. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/G.729){: new_window}

### Format 'audio/l16'
{: #l16}

Die *lineare 16-Bit-Pulscodemodulation (PCM)* ist ein nicht komprimiertes Audioformat (`audio/l16`). Verwenden Sie dieses Format, um eine unbearbeitete PCM-Datei zu übergeben. Lineare PCM-Audiodaten können auch innerhalb einer WAV-Datei als Container übergeben werden. Bei Verwendung des Formats `audio/l16` akzeptiert der Service zusätzliche erforderliche und optionale Parameter in der Formatspezifikation.

<table>
  <caption>Tabelle 3. Parameter für das Format `audio/l16`</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Parameter
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Beschreibung
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Erforderlich</em>
    </td>
    <td>
      Eine ganze Zahl, mit der die Abtastfrequenz angegeben wird, in der
      die Audiodaten aufgezeichnet wurden. Geben Sie beispielsweise den folgenden
      Parameter für Audiodaten an, die mit 16 kHz aufgezeichnet wurden:<br/><br/>
      <code>audio/l16;rate=16000</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>channels</code><br/><em>Optional</em>
    </td>
    <td>
      Standardmäßig behandelt der Service Audiodaten so, als ob sie über einen
      einzigen Kanal verfügen. <em>Falls die Audiodaten mehrere Kanäle
      enthalten,</em> müssen Sie die Anzahl der Kanäle mit einer ganzen Zahl
      angeben. Geben Sie beispielsweise den folgenden Parameter für
      mit 16 kHz aufgezeichnete Zweikanalaudiodaten an:<br/><br/>
      <code>audio/l16;rate=16000;channels=2</code><br/><br/>
      Der Service akzeptiert höchstens 16 Kanäle. Während der Transcodierung
      wird die Anzahl der Kanäle auf 1 heruntergesetzt.
    </td>
  </tr>
  <tr>
    <td>
      <code>endianness</code><br/><em>Optional</em>
    </td>
    <td>
      Die Endianess von eingehenden Audiodaten wird durch den Service
      standardmäßig automatisch erkannt. Die automatische Erkennung kann jedoch
      manchmal fehlschlagen und die Verbindung für kurze Audiodaten im
      Format `audio/l16` trennen. Durch die Angabe der
      Endianess wird die automatische Erkennung inaktiviert. Geben Sie entweder
      <code>big-endian</code> oder <code>little-endian</code> an. Geben Sie
      beispielsweise für mit 16 kHz aufgezeichnete Audiodaten im Little-Endian-Format den
      folgenden Parameter an:<br/><br/>
      <code>audio/l16;rate=16000;endianness=little-endian</code><br/><br/>
      In Abschnitt 5.1 ist auf der Seite
      <a target="_blank" href="https://tools.ietf.org/html/rfc2045#section-5.1">Request for Comment (RFC) 2045 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")</a>
      das Big-Endian-Format für Daten im Format <code>audio/l16</code>
      angegeben, aber vielfach wird das Little-Endian-Format verwendet.
    </td>
  </tr>
</table>

Weitere Informationen finden Sie auf der IETF-Seite [Request for Comment (RFC) 2586 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://tools.ietf.org/html/rfc2586){: new_window} sowie unter der Adresse [en.wikipedia.org/wiki/Pulse-code_modulation. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Pulse-code_modulation){: new_window}

### Formate 'audio/mp3' und 'audio/mpeg'
{: #mp3}

*MP3* (`audio/mp3`) oder *Motion Picture Experts Group (MPEG)* (`audio/mpeg`) ist ein verlustbehaftetes Audioformat. (MP3 und MPEG bezeichnen dasselbe Format.) Weitere Informationen finden Sie unter der Adresse [en.wikipedia.org/wiki/MP3. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/MP3){: new_window}

### Format 'audio/mulaw'
{: #mulaw}

*Mu-Law* (`audio/mulaw`) ist ein verlustbehaftetes Einkanalaudioformat. Die Daten werden unter Verwendung des U-Law-Algorithmus (oder Mu-Law-Algorithmus) codiert. Das Format `audio/basic` ist ein funktional entsprechendes Format, das immer mit 8 kHz abgetastet wird. Bei Verwendung dieses Formats macht der Service einen zusätzlichen Parameter für die Formatspezifikation erforderlich.

<table>
  <caption>Tabelle 4. Parameter für das Format `audio/mulaw`</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Parameter
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Beschreibung
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Erforderlich</em>
    </td>
    <td>
      Eine ganze Zahl, mit der die Abtastfrequenz angegeben wird, in der
      die Audiodaten aufgezeichnet wurden. Geben Sie beispielsweise den folgenden
      Parameter für Audiodaten an, die mit 8 kHz aufgezeichnet wurden:<br/><br/>
      <code>audio/mulaw;rate=8000</code>
    </td>
  </tr>
</table>

Weitere Informationen finden Sie unter der Adresse [en.wikipedia.org/wiki/M-law_algorithm. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/M-law_algorithm){: new_window}

### Format 'audio/ogg'
{: #ogg}

*Ogg* (`audio/ogg`) ist ein freigegebenes Containerformat, das von der Xiph.org Foundation verwaltet wird ([xiph.org/ogg ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.xiph.org/ogg){: new_window}). Sie können komprimierte Audiodatenströme zusammen mit den folgenden verlustbehafteten Codecs verwenden:

-   *Opus* (`audio/ogg;codecs=opus`). Weitere Informationen finden Sie unter den Adressen [opus-codec.org ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.opus-codec.org/){: new_window} und [en.wikipedia.org/wiki/Opus (audio format) ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Opus){: new_window}. Lesen Sie auf der Seite *Opus (audio format)* insbesondere den Abschnitt *Containers*. 
-   *Vorbis* (`audio/ogg;codecs=vorbis`). Weitere Informationen finden Sie unter den Adressen [xiph.org/vorbis ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://xiph.org/vorbis/){: new_window} und [en.wikipedia.org/wiki/Vorbis. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Vorbis){: new_window}

Falls Sie den Codec nicht angeben, erkennt der Service ihn automatisch anhand der Eingabeaudiodaten. Opus ist der bevorzugte Codec; er wurde durch die Internet Engineering Task Force (IETF) standardisiert (siehe [Request for Comment (RFC) 6716 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://tools.ietf.org/html/rfc6716){: new_window}).

### Format 'audio/wav'
{: #wav}

*Waveform Audio File Format (WAV)* (`audio/wav`) ist ein häufig für nicht komprimierte Audiodatenströme verwendetes Containerformat, das jedoch auch komprimierte Audiodaten enthalten kann. Der Service unterstützt WAV-Audiodaten mit einer beliebigen Codierung und (aufgrund einer Einschränkung bei FFmpeg) maximal neun Kanälen.

Weitere Informationen zum WAF-Format finden Sie unter der Adresse [en.wikipedia.org/wiki/WAV. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/WAV){: new_window} Zusätzliche Angaben über die Reduzierung der Größe von WAV-Audiodaten durch ihre Konvertierung in den Opus-Codec enthält der Abschnitt [Mit Opus-Codec in 'audio/ogg' konvertieren](#conversionOgg).

### Format 'audio/webm'
{: #webm}

*Web Media (WebM)* (`audio/webm`) ist ein freigegebenes Containerformat, das durch das WebM-Projekt verwaltet wird ([webmproject.org ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.webmproject.org/){: new_window}). Sie können komprimierte Audiodatenströme zusammen mit den folgenden verlustbehafteten Codecs verwenden:

-   *Opus* (`audio/webm;codecs=opus`). Weitere Informationen finden Sie unter den Adressen [opus-codec.org ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.opus-codec.org/){: new_window} und [en.wikipedia.org/wiki/Opus (audio format) ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Opus){: new_window}. Lesen Sie auf der Seite *Opus (audio format)* insbesondere den Abschnitt *Containers*. 
-   *Vorbis* (`audio/webm;codecs=vorbis`). Weitere Informationen finden Sie unter den Adressen [xiph.org/vorbis ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://xiph.org/vorbis/){: new_window} und [en.wikipedia.org/wiki/Vorbis. ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://en.wikipedia.org/wiki/Vorbis){: new_window}

Falls Sie den Codec nicht angeben, erkennt der Service ihn automatisch anhand der Eingabeaudiodaten. 

Unter der Adresse [jsbin.com/hedujihuqo/edit?js,console ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://jsbin.com/hedujihuqo/edit?js,console){: new_window} können Sie sich JavaScript-Code ansehen, der zeigt, wie Audiodaten mit einem Mikrofon in einem Chrome-Browser aufgezeichnet und in einem WebM-Datenstrom codiert werden. Der Code übergibt nicht die aufgezeichneten Audiodaten an den Service.
{: tip}

## Datengrenzwerte und Komprimierung
{: #limits}

Der Service akzeptiert bei einer WebSocket- oder synchronen HTTP-Anforderung Audiodaten mit einer maximalen Größe von 100 MB. Stellen Sie bei der Erkennung von langen zusammenhängenden Audiodatenströmen oder umfangreichen Dateien mit den folgenden Schritten sicher, dass Ihre Audiodaten den Datengrenzwert von 100 MB nicht überschreiten:

-   Verwenden Sie eine Abtastfrequenz von maximal 16 kHz (bei Breitbandmodellen) bzw. 8 kHz (bei Schmalbandmodellen) und verwenden Sie 16 Bit pro Abtastung. Der Service konvertiert Audiodaten, die mit einer höheren Abtastfrequenz als der des Zielmodells (16 kHz bzw. 8 kHz) aufgezeichnet wurden, in die Frequenz des Modells. Höhere Frequenzen führen somit nicht zu einer verbesserten Genauigkeit der Erkennung, erhöhen jedoch die Größe des Audiodatenstroms.
-   Codieren Sie Ihre Audiodaten in einem Format, das Datenkomprimierung bietet. Wenn Sie Ihre Daten effizienter codieren, können Sie weitaus mehr Audiodaten senden, ohne den Grenzwert von 100 MB zu überschreiten. Audioformate wie `audio/ogg` und `audio/mp3` verkleinern den Audiodatenstrom ganz erheblich. Bei Verwendung dieser Formate können Sie mit einer einzigen Anforderung viel mehr Audiodaten senden.

Falls Sie die Audiodaten mit dem Format `audio/ogg` zu stark komprimieren, kann die Erkennungsgenauigkeit zu einem gewissen Grad abnehmen. Behalten Sie zur Sicherheit für Ihre Audiodaten mindestens eine Bitübertragungsrate von 24 Kb/s bei.

### Vergleich der ungefähren Größen von Audiodaten
{: #compareSizes}

Ein Datenstrom, der aus einer Übertragung von zweistündiger zusammenhängender Sprache mit einer Abtastfrequenz von 16 kHz und 16 Bit pro Abtastung resultiert, hat ungefähr die folgende Größe:

-   Falls die Daten mit dem Format `audio/wav` codiert sind, hat der zweistündige Datenstrom eine Größe von 230 MB und liegt somit deutlich über dem Grenzwert von 100 MB.
-   Falls die Daten im Format `audio/ogg` codiert sind, beträgt die Größe des zweistündigen Datenstroms lediglich 23 MB und liegt damit deutlich unter dem Grenzwert des Service.

In der folgenden Tabelle ist die ungefähre maximale Dauer von Audiodaten aufgeführt, die mit einer synchronen HTTP-Anforderung oder einer WebSocket-Anforderung für die Spracherkennung in unterschiedlichen Formaten gesendet werden können. Die Dauer berücksichtigt den Grenzwert des Service von 100 MB. Die tatsächlichen Werte können je nach Komplexität der Audiodaten und  der erzielten Komprimierungsrate variieren.

<table style="width:75%">
  <caption>Tabelle 5. Maximale Dauer von Audiodaten in verschiedenen Formaten</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 50%">
      Audioformat
    </th>
    <th style="text-align:center; vertical-align:bottom; width 50%">
      Maximale Dauer der Audiodaten (ungefähr)
    </th>
  </tr>
  <tr>
    <td><code>audio/wav</code></td>
    <td style="text-align:center">55 Minuten</td>
  </tr>
  <tr>
    <td><code>audio/flac</code></td>
    <td style="text-align:center">1 Stunde 40 Minuten</td>
  </tr>
  <tr>
    <td><code>audio/mp3</code></td>
    <td style="text-align:center">3 Stunden 20 Minuten</td>
  </tr>
  <tr>
    <td><code>audio/ogg</code></td>
    <td style="text-align:center">8 Stunden 40 Minuten</td>
  </tr>
</table>

Die Formate `audio/ogg;codecs=opus` und `audio/webm;codecs=opus` sind im Allgemeinen funktional entsprechend und ihre Größenwerte sind fast identisch. Sie verwenden intern denselben Codec; nur das Containerformat ist unterschiedlich.

## Konvertierung von Audiodaten
{: #conversion}

Es gibt verschiedene Tools, mit denen Sie Ihre Audiodaten in ein anderes Format konvertieren können. Die Tools können von Nutzen sein, wenn Ihre Audiodaten in einem vom Service nicht unterstützten Format, einem nicht komprimierten Format oder einem verlustfreien Format vorliegen. Im letzteren Fall können Sie die Audiodaten in ein verlustbehaftetes Format konvertieren, um ihre Größe zu verringern.

Zum Konvertieren Ihrer Audiodaten aus einem Format in ein anderes Format sind die folgenden Freeware-Tools verfügbar:

-   Sound eXchange (SoX) ([sox.sourceforge.net ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://sox.sourceforge.net){: new_window})
-   FFmpeg ([ffmpeg.org ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ffmpeg.org){: new_window})
-   Audacity&reg; ([audacityteam.org ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://www.audacityteam.org/){: new_window})
-   Format 'Ogg' mit Opus-Codec: **Opus-Tools** ([opus-codec.org/downloads/ ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://opus-codec.org/downloads/){: new_window})

Diese Tools bieten eine plattformübergreifende Unterstützung für viele Audioformate. Darüber hinaus können Sie mit vielen dieser Tools Ihre Audiodaten wiedergeben. Achten Sie bei der Nutzung der Tools darauf, keine geltenden Urheberrechtsgesetze zu verletzen.

### In Format 'audio/ogg' mit Opus-Codec konvertieren
{: #conversionOgg}

Die [**Opus-Tools**  ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://opus-codec.org/downloads/){: new_window} umfassen drei Befehlszeilendienstprogramme zur Arbeit mit Ogg-Audiodaten im Opus-Codec:

-   Das Dienstprogramm [**opusenc** ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusenc.html){: new_window} codiert Audiodaten aus WAV, FLAC und anderen Formaten in Ogg mit Opus-Codec. Auf der Seite wird gezeigt, wie Audiodatenströme komprimiert werden. Die Komprimierung ist zur Übergabe von echtzeitorientierten Audiodaten an den Service von Nutzen.
-   Das Dienstprogramm [**opusdec** ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusdec.html){: new_window} codiert Audiodaten aus dem Opus-Codec in nicht komprimierte PCM-WAV-Dateien.
-   Das Dienstprogramm [**opusinfo** ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusinfo.html){: new_window} stellt Informationen und eine Gültigkeitsprüfung für Opus-Dateien zur Verfügung.

Viele Benutzer senden für die Spracherkennung WAV-Dateien. Angesichts des 100 MB betragenden Datengrenzwert des Service für WebSocket- und synchrone HTTP-Anforderungen verringert das WAV-Format die Menge der Audiodaten, die mit einer einzigen Anforderung erkannt werden können. Die Konvertierung der Audiodaten mit dem Befehl **opusenc** in das bevorzugte Format `audio/ogg:codecs=opus` kann die Menge der mit einer Erkennungsanforderugn sendbaren Audiodaten beträchtlich erhöhen.

Beispiel: Eine nicht komprimierte WAV-Breitbanddatei (16 kHz) namens **input.wav** verwendet 16 Bit pro Abtastung für eine Bitübertragungsrate von 256 Kb/s. Der folgende Befehl konvertiert die Audiodaten in eine Datei namens **output.opus**, die den Opus-Code verwendet:

```bash
opusenc input.wav output.opus
```
{: pre}

Die Konvertierung komprimiert die Audiodaten um den Faktor 4 und erzeugt eine Ausgabedatei mit einer Bitübertragungsrate von 64 Kb/s. Gemäß den [empfohlenen Einstellungen für Opus ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://wiki.xiph.org/Opus_Recommended_Settings){: new_window} können Sie jedoch die Bitübertragungsrate ohne Weiteres auf 24 Kb/s verringern und trotzdem die volle Bandbreite für Sprachaudiodaten beibehalten. Diese Verringerung komprimiert die Audiodaten um den Faktor 10. Der folgende Befehl verwendet die Option `--bitrate`, um eine Ausgabedatei mit einer Bitübertragungsrate von 24 Kb/s zu erzeugen:

```bash
opusenc --bitrate 24 input.wav output.opus
```
{: pre}

Die Komprimierung mit dem Dienstprogramm **opusenc** wird zügig ausgeführt. Die Komprimierung findet mit einer Geschwindigkeit statt, die ungefähr 100 Mal schneller als Echtzeit ist. Nach dem Abschluss der Komprimierung schreibt der Befehl Ausgabe in die Konsole, die vollständige Details über die Ausführungsdauer und die resultierenden Audiodaten enthält.

## Tipps für die Verbesserung der Spracherkennung
{: #audioTips}

Die folgenden Tipps können Ihnen dabei helfen, die Qualität der Spracherkennung zu verbessern:

-   Die Art der Aufzeichnung von Audiodaten kann einen großen Unterschied bei den Ergebnissen des Service ausmachen. Die Spracherkennung kann sehr sensibel auf die Tonqualität der Eingabe reagieren. Stellen Sie zur Erzielung der größtmöglichen Genauigkeit sicher, dass die Tonqualität der Eingabe so gut wie möglich ist.
    -   Verwenden Sie nach Möglichkeit ein nahes und sprachorientiertes Mikrofon (z. B. ein Headset) und passen Sie Mikrofoneinstellungen bei Bedarf an. Die Leistung des Service ist am besten, wenn Audiodaten mithilfe von Profi-Mikrofonen aufgezeichnet werden.
    -   Vermeiden Sie die Verwendung des integrierten Mikrofons eines Systems. Die normalerweise in Mobilgeräten und Tablets installierten Mikrofone sind häufig unzulänglich.
    -   Achten Sie darauf, dass sich die Sprecher nahe am Mikrofon befinden. Die Genauigkeit nimmt ab, je mehr sich ein Sprecher von einem Mikrofon entfernt. Beispielsweise hat der Service bei einer Entfernung von 3 Metern Schwierigkeiten, angemessene Ergebnisse zu erzeugen.
-   Die Spracherkennung reagiert empfindlich auf Hintergrundgeräusche und Nuancen der menschlichen Sprache.
    -   Motorgeräusche, Arbeitsgeräte, Straßenlärm und Hintergrundgespräche können die Erkennungsgenauigkeit erheblich verringern.
    -   Regionale Akzente und Unterschiede bei der Aussprache können die Genauigkeit ebenfalls reduzieren.

    Falls Ihre Audiodaten diese Merkmale aufweisen, kann die Verwendung eines angepassten Akustikmodells sinnvoll sein, um die Genauigkeit der Spracherkennung zu verbessern. Weitere Informationen enthält der Abschnitt [Anpassungsschnittstelle](/docs/services/speech-to-text/custom.html).
