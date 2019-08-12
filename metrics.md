---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-10"

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

# Metrikfunktionen
{: #metrics}

Der {{site.data.keyword.speechtotextfull}}-Service kann zwei Typen optionaler Metriken für eine Spracherkennungsanforderung zurückgeben:

-   [Verarbeitungsmetriken](#processing_metrics) stellen regelmäßige Informationen über die Verarbeitung der Audioeingabedaten durch den Service bereit. Mithilfe der Metriken können Sie den Fortschritt des Service bei der Transkription der Audiodaten messen. Verarbeitungsmetriken stehen bei der WebSocket-Schnittstelle und bei der asynchronen HTTP-Schnittstellen zur Verfügung.
-   [Audiometriken](#audio_metrics) stellen Informationen über die Signalmerkmale der Audioeingabedaten bereit. Mithilfe der Metriken können Sie die Merkmale und die Qualität der Audiodaten feststellen. Audiometriken sind mit allen Spracherkennungsschnittstellen verfügbar.

In der Standardeinstellung gibt der Service keine Metriken für eine Anforderung zurück.

## Verarbeitungsmetriken
{: #processing_metrics}

Verarbeitungsmetriken stellen detaillierte Zeitinformationen über die Analyse der Audioeingabedaten durch den Service bereit. Der Service gibt die Metriken in festgelegten Intervallen und mit Transkriptionsereignissen zurück, z. B. als Zwischen- und Endergebnisse.

Zu den Metriken gehören Statistikdaten über das Volumen der Audiodaten, die der Service empfangen und an die Spracherkennungsengine übertragen hat, und über die Verarbeitungsdauer der Audiodaten im Service. Wenn Sie Sprecherbezeichnungen anfordern, sind auch Informationen zum Umfang der Audiodaten enthalten, die der Service zur Bestimmung der Sprecherbezeichnungen verarbeitet hat.

Verarbeitungsmetriken können Ihnen helfen, den Fortschritt einer Erkennungsanforderung zu messen. Sie können Sie auch dabei unterstützen, das Fehlen von Ergebnissen aufgrund der folgenden Ursachen zu unterscheiden:

-   Fehlende Audiodaten.
-   Fehlende Sprache in übergebenen Audiodaten.
-   Blockierungen der Engine auf dem Server und Blockierungen des Netzes zwischen dem Client und dem Server. Zur Unterscheidung zwischen Blockierungen der Engine und Blockierungen des Netzes sind die Ergebnisse nicht ereignisgesteuert, sondern periodisch.

Mit den Metriken können Sie auch durch die Untersuchung der regelmäßigen Antwortzeiten Antwortabweichungen besser einschätzen. Metriken werden in einem konstanten Intervall generiert, so dass jede Differenz in den Ankunftszeiten durch eine Abweichung (Jitter) verursacht wird.

### Verarbeitungsmetriken anfordern
{: #processing_metrics_request}

Verwenden Sie die folgenden optionalen Parameter, um Verarbeitungsmetriken anzufordern:

-   `processing_metrics` ist ein boolescher Wert, der angibt, ob der Service Verarbeitungsmetriken zurückgeben soll. Geben Sie `true` an, um die Metriken anzufordern. Standardmäßig gibt der Service keine Metriken zurück.
-   `processing_metrics_interval` ist ein Gleitkommawert, der das Intervall in Sekunden (reale Prozesslaufzeit) angibt, in dem der Service die Metriken zurückgeben soll. In der Standardeinstellung gibt der Service Metriken einmal pro Sekunde zurück. Der Service ignoriert diesen Parameter, es sei denn, der Parameter `processing_metrics` ist auf `true` gesetzt.

    Der zulässige Mindestwert für den Parameter ist 0,1 Sekunden. Die Genauigkeitsstufe ist nicht eingeschränkt, so dass Sie Werte wie beispielsweise 0,25 und 0,125 angeben können. Im Service ist kein Maximalwert vorgegeben.

Wie Sie die Parameter angeben und wie der Service Verarbeitungsmetriken zurückgibt, ist von der Schnittstelle abhängig:

-   Bei der WebSocket-Schnittstelle geben Sie die Parameter mit der JSON-Nachricht `start` für eine Spracherkennungsanforderung an. Der Service berechnet und gibt Metriken in Echtzeit in dem angeforderten Intervall zurück.
-   Bei der asynchronen HTTP-Schnittstelle geben Sie Abfrageparameter mit einer Spracherkennungsanforderung an. Der Service berechnet die Metriken in dem angeforderten Intervall, gibt aber alle Metriken für die Audiodaten mit den Endergebnissen für die Transkription zurück.

Darüber hinaus gibt der Service Verarbeitungsmetriken für Transkriptionsereignisse zurück. Wenn Sie Zwischenergebnisse mit der WebSocket-Schnittstelle anfordern, können Sie Metriken häufiger als in dem angeforderten Intervall empfangen.

Geben Sie für das Verarbeitungsintervall einen höheren Wert an, wenn Sie Verarbeitungsmetriken nur für Transkriptionsereignisse und nicht in regelmäßigen Intervallen empfangen möchten. Wenn das Intervall länger ist als die Dauer der Audiodaten, gibt der Service Verarbeitungsmetriken nur für Transkriptionsereignisse zurück.

### Verarbeitungsmetriken verstehen
{: #processing_metrics_understand}

Der Service gibt Verarbeitungsmetriken im Feld `processing_metrics` des Objekts `SpeechRecognitionResults` zurück. Für Metriken, die in regelmäßigen Intervallen generiert wurden, enthält das Objekt nur das Feld `processing_metrics`, wie im folgenden Beispiel gezeigt.

```javascript
{
  "processing_metrics": {
    "processed_audio": {
      "received": float,
      "seen_by_engine": float,
      "transcription": float,
      "speaker_labels": float
    },
    "wall_clock_since_first_byte_received": float,
    "periodic": boolean
  }
}
```
{: codeblock}

Das Feld `processing_metrics` enthält ein Objekt `ProcessingMetrics`, das die folgenden Felder enthält:

-   `wall_clock_since_first_byte_received` ist der Echtzeitraum in Sekunden, der vergangen ist, seit der Service das erste Byte der Eingabeaudiodaten empfangen hat. Die Werte in diesem Feld sind in der Regel ein Vielfaches des angegebenen Metrikintervalls. Dabei gibt es zwei Unterschiede:
    -   Werte stellen möglicherweise keine exakten Intervalle wie 0,25, 0,5 usw. dar. Die tatsächlichen Werte können beispielsweise 0,27, 0,52 usw. sein, je nachdem, wann der Service Audiodaten empfängt und verarbeitet.
    -   Werte für Transkriptionsereignisse sind vom Verarbeitungsintervall unabhängig. Sobald ereignisgesteuerte Ereignisse auftreten, gibt der Service diese zurück.
-   `periodic` gibt an, ob die Metriken sich auf ein regelmäßiges Intervall oder ein Transkriptionsereignis beziehen:
    -   `true` bedeutet, dass die Antwort durch ein Verarbeitungsintervall ausgelöst wurde. Die Informationen enthalten nur Verarbeitungsmetriken.
    -   `false` bedeutet, dass die Antwort durch ein Transkriptionsereignis ausgelöst wurde. Die Informationen enthalten Verarbeitungsmetriken und Transkriptionsergebnisse.

    In diesem Feld können Sie angeben, warum der Service die Antwort generiert hat, und ggf. verschiedene Ergebnisse filtern.
-   `processed_audio` enthält ein Objekt `ProcessedAudio`, das detaillierte Zeitinformationen über die Verarbeitung der Audioeingabedaten durch den Service bereitstellt.

Das Objekt `ProcessedAudio` enthält die folgenden Felder. Alle diese Felder beziehen sich auf Sekunden der Audiodaten zum Zeitpunkt dieser Antwort. Nur das Feld `wall_clock_since_first_byte_received` bezieht sich auf abgelaufene Echtzeit.

-   `received` gibt die Sekunden der Audiodaten an, die der Service empfangen hat.
-   `seen_by_engine` gibt die Sekunden der Audiodaten an, die der Service an seine Sprachverarbeitungsengine übergeben hat.
-   `transcription` gibt die Sekunden der Audiodaten an, die der Service für die Spracherkennung verarbeitet hat.
-   `speaker_labels` gibt die Sekunden der Audiodaten an, die der Service für Sprecherbezeichnungen verarbeitet hat. Dieses Feld ist nur dann in der Antwort enthalten, wenn Sie Sprecherbezeichnungen anfordern.

Die Sprachverarbeitungsengine analysiert die Eingabeaudiodaten mehrmals. Das Objekt `processed_audio` zeigt Werte für Audiodaten an, die die Engine verarbeitet hat und nicht erneut lesen wird. Verarbeitete Audiodaten haben keine Auswirkungen auf zukünftige Erkenntnishypothesen.

Die Metriken geben Aufschluss über den Fortschritt und die Komplexität der Verarbeitung der Engine:

-   `seen_by_engine` sind Audiodaten, die der Service gelesen und mindestens einmal an die Engine übergeben hat.
-   `received` - `seen_by_engine` sind Audiodaten, die im Service gepuffert, aber noch nicht an die Engine übergeben und noch nicht von ihr verabeitet wurden.
-   Zwischen den Zeiten besteht die folgende Beziehung: `received` >= `seen_by_engine` >= `transcription` >= `speaker_labels`.

Auch die folgenden Beziehungen können bei der Interpretation der Ergebnisse hilfreich sein:

-   Die Werte der Felder `received` und `seen_by_engine` sind größer als die Werte der Felder `Transkription` und `speaker_labels` während der Spracherkennungsverarbeitung. Der Service muss die Audiodaten empfangen, bevor er mit der Verarbeitung der Ergebnisse beginnen kann.
-   Die Werte der Felder `received` und `seen_by_engine` sind identisch, wenn der Service die Verarbeitung des Audiodaten beendet hat. Die Endwerte der Felder können um einen Bruchteil von Sekunden größer sein als die Werte der Felder `transcription` und `speaker_labels`.
-   Der Wert des Felds `speaker_labels` folgt normalerweise dem Wert des Felds `transcription` während der Spracherkennungsverarbeitung. Die Werte der Felder `Transkription</MD

### Beispiel für Verarbeitungsmetriken: WebSocket-Schnittstelle
{: #processing_metrics_example_websocket}

Das folgende Beispiel zeigt die Nachricht `start`, die für eine Anforderung an die WebSocket-Schnittstelle übergeben wird. Die Anforderung aktiviert Verarbeitungsmetriken und legt ein Intervall von 0,25 Sekunden für Verarbeitungsmetriken fest. Außerdem werden die Parameter `interim_results` und `speaker_labels` auf `true` gesetzt. Die Audiodaten enthalten die einfache Nachricht "hello world long pause stop".

```javascript
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/flac',
    processing_metrics: true,
    processing_metrics_interval: 0.25,
    interim_results: true,
    speaker_labels: true
  };
  websocket.send(JSON.stringify(message));
  websocket.send(blob);
}
```
{: codeblock}

Die folgende Beispielausgabe zeigt die ersten Ergebnisse der Verarbeitungsmetriken, die der Service für die Anforderung zurückgibt.

```javascript
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 1.59,
      "transcription": 0.49,
      "speaker_labels": 0.0
    },
    "wall_clock_since_first_byte_received": 0.32,
    "periodic": true
  }
}
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 1.59,
      "transcription": 0.49,
      "speaker_labels": 0.0
    },
    "wall_clock_since_first_byte_received": 0.51,
    "periodic": false
  },
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "hello",
              0.43,
              0.76
            ],
            [
              "world",
              0.76,
              1.22
            ]
          ],
          "transcript": "hello world "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 2.59,
      "transcription": 1.25,
      "speaker_labels": 0.0
    },
    "wall_clock_since_first_byte_received": 0.57,
    "periodic": true
  }
}
. . .
```
{: codeblock}

### Beispiel für Verarbeitungsmetriken: Asynchrone HTTP-Schnittstelle
{: #processing_metrics_example_http}

Das folgende Beispiel zeigt eine Spracherkennungsanforderung für die Methode `/v1/recognitions` der asynchronen HTTP-Schnittstelle. Die Anforderung aktiviert Verarbeitungsmetriken und gibt ein Intervall von 0,25 Sekunden an. Auch hier enthält die Audiodatei die Nachricht "hello world long pause stop".

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?processing_metrics=true&processing_metrics_interval=0.25"
```
{: pre}

Die folgende Beispielausgabe zeigt die ersten beiden Ergebnisse der Verarbeitungsmetriken, die der Service für die Anforderung zurückgibt.

```javascript
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 1.59,
      "transcription": 0.49
    },
    "wall_clock_since_first_byte_received": 0.32,
    "periodic": true
  }
}
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 2.59,
      "transcription": 1.25
    },
    "wall_clock_since_first_byte_received": 0.57,
    "periodic": true
  }
}
. . .
```
{: codeblock}

## Audiometriken
{: #audio_metrics}

Audiometriken stellen detaillierte Informationen über die Signalmerkmale der Audioeingabedaten bereit. In den Ergebnissen sind Metriken für die gesamten Audioeingabedaten zum Abschluss der Sprachverarbeitung zusammengefasst. Für einen technisch erfahrenen Benutzer können die Metriken aussagekräftige Informationen zu den detaillierten Merkmalen der Audiodaten liefern.

Mit Audiometriken können Sie einen Hinweis auf ein Problem der Eingabeaudiodaten in Echtzeit und eventuell sogar eine mögliche Lösung zur Verfügung stellen. Sie können z. B. die Nachricht "Die Hintergrundgeräusche sind zu stark" oder "Bitte näher an das Mikrofon kommen" bereitstellen. Außerdem können Sie mit einem Offline-Analysetool die Signalmerkmale überprüfen und Daten vorschlagen, die für zukünftige Modellaktualisierungen geeignet sind.

### Audiometriken anfordern
{: #audio_metrics_request}

Um Audiometriken anzufordern, setzen Sie den booleschen Parameter `audio_metrics` auf `true`. Standardmäßig gibt der Service keine Metriken zurück.

-   Bei der WebSocket-Schnittstelle geben Sie den Parameter mit der JSON-Nachricht `start` für eine Spracherkennungsanforderung an.
-   Bei den HTTP-Schnittstellen geben Sie einen Abfrageparameter mit einer Spracherkennungsanforderung an.

Der Service gibt die Audiometriken mit den Endergebnissen für die Transkription zurück. Die Metriken werden nur einmal für den gesamten Audiodatenstrom zurückgegeben. Auch wenn der Service mehrere Transkriptionsergebnisse (für verschiedene Audioblöcke) zurückgibt, gibt er nur eine einzige Instanz der Metriken am Ende der Ergebnisse zurück.

Die WebSocket-Schnittstelle akzeptiert mehrere Audiodatenströme (oder Audiodateien) mit einer einzigen Verbindung. Verschiedene Datenströme können Sie begrenzen, indem Sie `stop`-Nachrichten oder leere binäre BLOBs an den Service senden. In diesem Fall gibt der Service separate Metriken für jeden begrenzten Audiodatenstrom zurück.

### Audiometriken verstehen
{: #audio_metrics_understand}

Der Service gibt die Metriken im Feld `audio_metrics` des Objekts `SpeechRecognitionResults` zurück.

```javascript
{
  "results": [
    . . .
  ],
  "result_index": integer,
  "audio_metrics": {
    "sampling_interval": float,
    "accumulated": {
      "final": boolean,
      "end_time": float,
      "signal_to_noise_ratio": float,
      "speech_ratio": float,
      "high_frequency_loss": float,
      "direct_current_offset": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ],
      "clipping_rate": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ],
      "speech_level": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ],
      "non_speech_level": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ]
    }
  }
}
```
{: codeblock}

Das Feld `audio_metrics` enthält ein Objekt `AudioMetrics`, das über zwei Felder verfügt:

-   `sampling_interval` gibt das Intervall in Sekunden (normalerweise 0,1 Sekunden) an, in dem der Service die Audiometriken berechnet hat.
-   `accumulated` enthält ein Objekt `AudioMetricsDetails`, das detaillierte Informationen über die Signalmerkmale der Audioeingabedaten bereitstellt.

Die folgenden Felder des Objekts `AudioMetricsDetails` stellen unäre Werte bereit:

-   `final` gibt an, ob die Metriken für das Ende des Audiodatenstroms gelten, d. h. die Transkription ist abgeschlossen. Derzeit ist das Feld immer auf `true` gesetzt. Der Service gibt Metriken nur einmal pro Audiodatenstrom zurück. In den Ergebnissen sind Metriken für den vollständigen Datenstrom zusammengefasst.
-   `end_time` gibt die Endzeit der Audiodaten, auf die sich die Metriken beziehen, in Sekunden an. Da die Metriken für den gesamten Audiodatenstrom gelten, ist die Endzeit immer die Länge der Audiodaten.
-   `signal_to_noise_ratio` gibt das Signal-Rausch-Verhältnis (Signal-to-Noise Ratio, SNR) für das Tonsignal an. Der Wert gibt das Verhältnis zwischen Sprache und Rauschen in den Audiodaten an. Gültige Werte liegen im Bereich von 0 bis 100 Dezibel (dB). Der Service übergeht das Feld, wenn er den SNR-Wert für die Audiodaten nicht berechnen kann.
-   `speech_ratio` gibt das Verhältnis zwischen Sprachsegmenten und Segmenten ohne Sprachgeräusche im Tonsignal an. Gültige Werte liegen im Bereich von 0,0 bis 1,0.
-   `high_frequency_loss` gibt die Wahrscheinlichkeit an, dass die obere Hälfte des Frequenzinhalts im Tonsignal fehlt.
    -   Ein Wert nahe 1,0 gibt normalerweise Audiodaten mit künstlichem Upsampling an, was sich negativ auf die Genauigkeit der Transkriptionsergebnisse auswirkt.
    -   Der Wert 0,0 oder ein Wert nahe 0,0 gibt an, dass das Tonsignal gut ist und ein vollständiges Spektrum aufweist.
    -   Ein Wert um 0,5 gibt an, dass die Erkennung des Frequenzinhalts nicht zuverlässig oder nicht verfügbar ist.

Die folgenden Felder des Objekts `AudioMetricsDetails` stellen Histogramme für Signalmerkmale bereit. Jedes Feld stellt ein Array von `AudioMetricsHistogramBin`-Objekten bereit. Die Berechnung einer einzelnen Einheit in jedem Histogramm basiert auf der Länge der Audiodaten von `sampling_interval`.

-   `direct_current_offset` definiert ein Histogramm der kumulativen Gleichstrom-Komponente (DC) des Tonsignals.
-   `clipping_rate` definiert ein Histogramm der Clipping-Rate für die Tonsegmente. Die Clipping-Rate ist als der Bruchteil der Abtastungen in dem Segment definiert, die den Maximal- oder Minimalwert erreichen, der durch den Audioquantisierungsbereich festgelegt ist.

    Von dem Service wird entweder ein 16-Bit-PCM-Audiobereich (-32768 bis +32767) oder ein Einheitenbereich (-1,0 bis +1,0) automatisch erkannt (PCM = Pulscodemodulation). Die Clipping-Rate liegt im Bereich von 0,0 bis 1,0. Höhere Werte weisen auf eine mögliche Verschlechterung der Spracherkennung hin.
-   `speech_level` definiert ein Histogramm des Signalpegels in Segmenten der Audiodaten, die Sprache enthalten. Der Signalpegel wird als Effektivwert in einer Dezibel-Skala (dB) berechnet, die auf den Bereich 0,0 (Minimalpegel) bis 1,0 (Maximalpegel) normalisiert ist.
-   `non_speech_level` definiert ein Histogramm des Signalpegels in Segmenten der Audiodaten, die keine Sprache enthalten. Der Signalpegel wird als Effektivwert in einer Dezibel-Skala berechnet, die auf den Bereich 0,0 (Minimalpegel) bis 1,0 (Maximalpegel) normalisiert ist.

Jedes Objekt `AudioMetricsHistogramBin` beschreibt ein Fach mit definierten Anfangs- (`begin`) und Endbegrenzung (`end`). Jedes Fach gibt die Anzahl `count` der Werte in dem Bereich der Signalmerkmale für das jeweilige Fach an. Das erste und letzte Fach eines Histogramms sind die Grenzfächer. Sie decken die Intervalle zwischen negativ Unendlich und der ersten Begrenzung bzw. zwischen der letzten Begrenzung und positiv Unendlich ab.

### Beispiel für Audiometriken
{: #audio_metrics_example}

Das folgende Beispiel zeigt eine Spracherkennungsanforderung mit der synchronen HTTP-Schnittstelle, die Audiometriken zurückgibt. Die Audiodatei enthält die einfache Nachricht "hello world long pause stop".

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitize?audio_metrics=true"
```
{: pre}

Die Antwort enthält die Audiometriken für die kompletten Eingabeaudiodaten mit einer Länge von 7,0 Sekunden. Die Eingabeaudiodaten enthalten etwas mehr Sprache als Segmente ohne Sprache: 37 zu 33 Segmente, bei einem Wert von 0,529 für `speech_ratio`. Die Clipping-Rate ist sehr niedrig, was auf hochwertige Eingabeaudiodaten hinweist.

Das Feld `high_frequency_loss` weist den Wert 0,5 auf. Das bedeutet, dass die Erkennung des Frequenzinhalts des Service nicht zuverlässig oder nicht verfügbar ist. Das Feld `signal_to_noise_ratio` ist in den Ergebnissen nicht enthalten, weil der Service das Signal-Rausch-Verhältnis für die Audiodaten nicht berechnen konnte.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "hello world "
        }
      ],
          "final": true
        },
        {
      "alternatives": [
        {
          "confidence": 0.79,
          "transcript": "long pause "
        }
      ],
          "final": true
        },
        {
      "alternatives": [
        {
          "confidence": 0.97,
          "transcript": "stop "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0,
  "audio_metrics": {
    "sampling_interval": 0.1,
    "accumulated": {
      "final": true,
      "end_time": 7.0,
      "speech_ratio": 0.529,
      "high_frequency_loss": 0.5,
      "direct_current_offset": [
        {"begin": -1.0, "end": -0.9, "count": 0},
        {"begin": -0.9, "end": -0.7, "count": 0},
        {"begin": -0.7, "end": -0.5, "count": 0},
        {"begin": -0.5, "end": -0.3, "count": 0},
        {"begin": -0.3, "end": -0.1, "count": 0},
        {"begin": -0.1, "end": 0.1, "count": 70},
        {"begin": 0.1, "end": 0.3, "count": 0},
        {"begin": 0.3, "end": 0.5, "count": 0},
        {"begin": 0.5, "end": 0.7, "count": 0},
        {"begin": 0.7, "end": 0.9, "count": 0},
        {"begin": 0.9, "end": 1.0, "count": 0}
      ],
      "clipping_rate": [
        {"begin": 0.0, "end": 1e-05, "count": 70},
        {"begin": 1e-05, "end": 0.0001, "count": 0},
        {"begin": 0.0001, "end": 0.001, "count": 0},
        {"begin": 0.001, "end": 0.01, "count": 0},
        {"begin": 0.01, "end": 0.1, "count": 0},
        {"begin": 0.1, "end": 1.0, "count": 0}
      ],
      "speech_level": [
        {"begin": 0.0, "end": 0.1, "count": 37},
        {"begin": 0.1, "end": 0.2, "count": 0},
        {"begin": 0.2, "end": 0.3, "count": 0},
        {"begin": 0.3, "end": 0.4, "count": 0},
        {"begin": 0.4, "end": 0.5, "count": 0},
        {"begin": 0.5, "end": 0.6, "count": 0},
        {"begin": 0.6, "end": 0.7, "count": 0},
        {"begin": 0.7, "end": 0.8, "count": 0},
        {"begin": 0.8, "end": 0.9, "count": 0},
        {"begin": 0.9, "end": 1.0, "count": 0}
      ],
      "non_speech_level": [
        {"begin": 0.0, "end": 0.1, "count": 33},
        {"begin": 0.1, "end": 0.2, "count": 0},
        {"begin": 0.2, "end": 0.3, "count": 0},
        {"begin": 0.3, "end": 0.4, "count": 0},
        {"begin": 0.4, "end": 0.5, "count": 0},
        {"begin": 0.5, "end": 0.6, "count": 0},
        {"begin": 0.6, "end": 0.7, "count": 0},
        {"begin": 0.7, "end": 0.8, "count": 0},
        {"begin": 0.8, "end": 0.9, "count": 0},
        {"begin": 0.9, "end": 1.0, "count": 0}
      ]
    }
  }
}
```
{: codeblock}
