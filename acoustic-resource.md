---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-19"

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
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Mit Audioressourcen arbeiten
{: #audioResources}

Sie können einzelne Audiodateien oder Archivdateien, die mehrere Audiodateien enthalten, zu einem angepassten Akustikmodell hinzufügen. Das Hinzufügen von Archivdateien ist das empfohlene Verfahren zum Hinzufügen von Audioressourcen. Es ist wesentlich effizienter, eine einzige Archivdatei zu erstellen und hinzuzufügen, als mehrere Audiodateien nacheinander separat hinzuzufügen. Sie können auch Anforderungen übergeben, um mehrere verschiedene Audioressourcen gleichzeitig hinzuzufügen.
{: shortdesc}

## Audioressource hinzufügen
{: #addAudioResource}

Zum Hinzufügen einer Audioressource beliebigen Typs zu einem angepassten Akustikmodell verwenden Sie die Methode `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`. Hierbei übergeben Sie die Audioressource als Hauptteil der Anforderung und beziehen die folgenden Parameter ein:

-   `customization_id`: Dieser Pfadparameter gibt die Anpassungs-ID des Modells an.
-   `audio_name`: Dieser Pfadparameter gibt einen Namen für die Audioressource an.
    -   Verwenden Sie einen übersetzten Namen in der Sprache des angepassten Modells, der den Inhalt der Ressource wiedergibt.
    -   Geben Sie einen Namen mit einer maximalen Länge von 128 Zeichen an.
    -   Verwenden Sie keine Zeichen, die URL-codiert sein müssen. Verwenden Sie beispielsweise keine Leerzeichen, Schrägstriche, umgekehrte Schrägstriche, Doppelpunkte, Et-Zeichen, doppelte Anführungszeichen, Pluszeichen, Gleichheitszeichen, Fragezeichen usw. im Namen. (Der Service verhindert die Verwendung dieser Zeichen nicht. Da sie jedoch immer URL-codiert sein müssen, wird von ihrer Verwendung unbedingt abgeraten.)
    -   Verwenden Sie nicht den Namen einer Audioressource, die bereits zum angepassten Modell hinzugefügt wurde.

Nachdem Sie die Audioressourcen eines Modells aktualisiert haben, müssen Sie das Modell trainieren, damit die Änderungen bei der Transkription wirksam werden. Weitere Informationen finden Sie unter [Angepasstes Akustikmodell trainieren](/docs/services/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic).

## Audiodatei hinzufügen
{: #addAudioType}

Zum Hinzufügen einer einzelnen Audiodatei zu einem angepassten Akustikmodell geben Sie das Format (MIME-Typ) der Audiodaten mit dem Header `Content-Type` an. Die hinzugefügten Audiodaten können jedes beliebige Format aufweisen, das bei der Verwendung von Erkennungsanforderungen unterstützt wird. Beziehen Sie die Parameter `rate`, `channels` und `endianness` für die Spezifikation von Formaten ein, bei denen diese Parameter erforderlich sind. Weitere Informationen zu den unterstützten Audioformaten finden Sie im Abschnitt [Audioformate](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).

Die Spezifikation `application/octet-stream` für ein Audioformat wird bei Audioressourcen nicht unterstützt.
{: note}

Das folgende Beispiel aus dem Abschnitt [Audiodaten zum angepassten Akustikmodell hinzufügen](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio) fügt eine Datei mit dem Format `audio/wav` hinzu:

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @audio1.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
```
{: pre}

## Archivdatei hinzufügen
{: #addArchiveType}

Das bevorzugte Verfahren zum Hinzufügen von Audiodaten zu einem angepassten Akustikmodell ist die Verwendung einer Archivdatei, die mehrere Audiodateien enthält. Sie können die folgenden Typen von Archivdateien hinzufügen, indem Sie den Typ des Archivs mit dem Header `Content-Type` angeben:

-   Zum Hinzufügen einer Datei **.zip** geben Sie `application/zip` an.
-   Zum Hinzufügen einer Datei **.tar.gz** geben Sie `application/gzip` an.

Abhängig vom Format der Dateien, die Sie hinzufügen, müssen Sie unter Umständen auch den Header `Contained-Content-Type` angeben:

-   Bei Audiodateien des Typs `audio/alaw`, `audio/basic`, `audio/l16` oder `audio/mulaw` müssen Sie mit dem Header `Contained-Content-Type` das Format der Audiodateien angeben. Beziehen Sie bei Bedarf auch die Parameter `rate`, `channels` und `endianness` ein. In diesem Fall müssen alle Audiodateien, die in der Archivdatei enthalten sind, dasselbe Audioformat besitzen.
-   Bei Audiodateien aller anderen Typen können Sie den Header `Contained-Content-Type` weglassen. In diesem Fall können die in der Archivdatei enthaltenen Audiodaten jedes Format aufweisen, das nicht unter dem vorherigen Listenpunkt angegeben ist. Sie müssen nicht zwangsläufig dasselbe Format besitzen.

Verwenden Sie den Header `Contained-Content-Type` nicht, wenn Sie eine Audiodateiressource hinzufügen.
{: note}

Der Name einer Audiodatei, die in einer Archivressource enthalten ist, kann maximal 128 Zeichen umfasse. Dazu zählen die Dateierweiterung und alle Elemente des Namens (z. B. Schrägstriche).

Das folgende Beispiel aus dem Abschnitt [Audiodaten zum angepassten Akustikmodell hinzufügen](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio) fügt eine Datei mit dem Format `application/zip` hinzu, die mit 16 kHz abgetastete Audiodateien im Format `audio/l16` enthält:

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/zip"
--header "Contained-Content-Type: audio/l16;rate=16000"
--data-binary @audio2.zip
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
```
{: pre}

## Richtlinien für das Hinzufügen von Audioressourcen
{: #audioGuidelines}

Welche Verbesserung bei der Erkennungsgenauigkeit Sie von der Verwendung eines angepassten Akustikmodells erwarten können, hängt von einer Reihe von Faktoren ab. In diesem Zusammenhang ist beispielsweise von Bedeutung, wie viele Audiodaten im angepassten Akustikmodell enthalten sind und wie ähnlich diese Daten den zu transkribierenden Audiodaten sind. Die Verbesserung ist außerdem davon abhängig, ob das angepasste Akustikmodell mit einem entsprechenden angepassten Sprachmodell trainiert wurde.

Befolgen Sie beim Hinzufügen von Audioressourcen zu einem angepasten Akustikmodell die folgenden Richtlinien:

-   Fügen Sie zu einem angepassten Akustikmodell Audiodaten mit einer Länge von mindestens 10 Minuten und höchstens 200 Stunden hinzu. Die Audiodaten müssen Sprache enthalten, nicht Schweigen.

    Bei der Festlegung, wie viele Audiodaten Sie hinzufügen wollen, ist auch die Qualität der Audiodaten von Bedeutung. Je besser die Audiodaten des Modells die Merkmale der zu erkennenden Audiodaten widerspiegeln, desto besser ist die Qualität des angepassten Modells für die Spracherkennung. Falls die Audiodaten eine gute Qualität besitzen, kann die Transkriptionsgenauigkeit von weiteren Daten verbessert werden. Aber auch das Hinzufügen von fünf bis zehn Stunden Audiodaten mit guter Qualität kann einen positiven Unterschied ausmachen.
-   Fügen Sie Audioressourcen hinzu, die nicht größer als 100 MB sind. Alle Audio- und Archivressourcen sind auf eine maximale Größe von 100 MB begrenzt.

    Um möglichst umfangreiche Audiodaten zu einer einzigen Ressource hinzuzufügen, kann die Verwendung eines Audioformats sinnvoll sein, das eine Komprimierung bietet. Weitere Informationen enthält der Abschnitt [Datengrenzwerte und Komprimierung](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#limits).
-   Unterteilen Sie große Audiodateien in mehrere kleinere Dateien. Stellen Sie sicher, dass Sie die Dateien zwischen Wörtern in Sprechpausen trennen.

    Da Sie mehrere simultane Anforderungen zum Hinzufügen verschiedener Audioressourcen übergeben können, können Sie kleinere Dateien gleichzeitig hinzufügen. Dieser parallele Ansatz zum Hinzufügen von Audioressourcen kann die Analyse Ihrer Audiodaten durch den Service beschleunigen.
-   Fügen Sie Audioinhalt hinzu, der die akustischen Kanalbedingungen der Audiodaten widerspiegelt, die Sie transkribieren wollen. Falls Ihre Anwendung beispielsweise Audiodaten mit Hintergrundgeräuschen eines fahrenden Fahrzeugs verarbeitet, verwenden Sie denselben Typ von Daten, um das angepasste Modell zu erstellen.
-   Stellen Sie sicher, dass die Abtastfrequenz einer Audiodatei mit der Abtastfrequenz des Basismodells für das angepasste Akustikmodell übereinstimmt:
    -   Bei Breitbandmodellen muss die Abtastfrequenz mindestens 16 kHz (= 16.000 Abtastungen pro Sekunde) betragen.
    -   Bei Schmalbandmodellen muss die Abtastfrequenz mindestens 8 kHz (= 8.000 Abtastungen pro Sekunde) betragen.

    Falls die Abtastfrequenz der Audiodaten höher als die mindestens erforderliche Abtastfrequenz ist, setzt der Service die Audiodaten auf die entsprechende Frequenz herunter. Liegt die Abtastfrequenz der Audiodaten unter der erforderlichen Mindestfrequenz, kennzeichnet der Service die Audiodatei mit `invalid` als ungültig. Falls eine Archivdatei eine ungültige Audiodatei enthält, betrachtet der Service das gesamte Archiv als ungültig.
-   Erstellen Sie in den folgenden Fällen ein angepasstes Sprachmodell zur Verwendung mit Ihrem angepassten Akustikmodell:
    -   Falls Ihre Audiodaten kürzer als eine Stunde sind, erstellen Sie ein angepasstes Sprachmodell, das auf den Transkriptionen der Audiodaten basiert, um die bestmöglichen Ergebnisse zu erzielen.
    -   Falls es sich um fachspezifische Audiodaten mit speziellen Wörtern handelt, die nicht im Grundvokabular des Service enthalten sind, verwenden Sie die Sprachmodellanpassung, um das Grundvokabular des Service zu erweitern. Durch eine bloße Akustikmodellanpassung können solche Wörter während der Transkription nicht erzeugt werden.

    Weitere Informationen finden Sie unter [Angepasste Akustikmodelle und angepasste Sprachmodelle kombiniert verwenden](/docs/services/speech-to-text?topic=speech-to-text-useBoth).
