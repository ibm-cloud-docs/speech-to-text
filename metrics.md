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

# Metrics features
{: #metrics}

The {{site.data.keyword.speechtotextfull}} service can return two types of optional metrics for a speech recognition request:

-   [Processing metrics](#processing_metrics) provide periodic information about the service's processing of the input audio. Use the metrics to gauge the service's progress in transcribing the audio. Processing metrics are available with the WebSocket and asynchronous HTTP interfaces.
-   [Audio metrics](#audio_metrics) provide information about the signal characteristics of the input audio. Use the metrics to determine the characteristics and quality of the audio. Audio metrics are available with all speech recognition interfaces.

By default, the service returns no metrics for a request.

## Processing metrics
{: #processing_metrics}

Processing metrics provide detailed timing information about the service's analysis of the input audio. The service returns the metrics at specified intervals and with transcription events, such as interim and final results.

The metrics include statistics about how much audio the service has received, how much audio the service has transferred to the speech recognition engine, and how long the service has been processing the audio. If you request speaker labels, the information also shows how much audio the service has processed to determine speaker labels.

Processing metrics can help you gauge the progress of a recognition request. They can also help you distinguish the absence of results due to

-   Lack of audio.
-   Lack of speech in submitted audio.
-   Engine stalls at the server and network stalls between the client and the server. To differentiate between engine and network stalls, results are periodic rather than event-based.

The metrics can also help you estimate response jitter by examining the periodic arrival times. Metrics are generated at a constant interval, so any difference in arrival times is caused by jitter.

### Requesting processing metrics
{: #processing_metrics_request}

To request processing metrics, use the following optional parameters:

-   `processing_metrics` is a boolean that indicates whether the service is to return processing metrics. Specify `true` to request the metrics. By default, the service returns no metrics.
-   `processing_metrics_interval` is a float that specifies the interval in seconds of real wall-clock time at which the service is to return metrics. By default, the service returns metrics once per second. The service ignores this parameter unless the `processing_metrics` parameter is set to `true`.

    The parameter accepts a minimum value of 0.1 seconds. The level of precision is not restricted, so you can specify values such as 0.25 and 0.125. The service does not impose a maximum value.

How you provide the parameters and how the service returns processing metrics differ by interface:

-   With the WebSocket interface, you specify the parameters with the JSON `start` message for a speech recognition request. The service calculates and returns metrics in real-time at the requested interval.
-   With the asynchronous HTTP interface, you specify query parameters with a speech recognition request. The service calculates the metrics at the requested interval, but it returns all metrics for the audio with the final transcription results.

The service also returns processing metrics for transcription events. If you request interim results with the WebSocket interface, you can receive metrics with greater frequency than the requested interval.

To receive processing metrics only for transcription events instead of at periodic intervals, set the processing interval to a large number. If the interval is larger than the duration of the audio, the service returns processing metrics only for transcription events.

### Understanding processing metrics
{: #processing_metrics_understand}

The service returns processing metrics in the `processing_metrics` field of the `SpeechRecognitionResults` object. For metrics generated at periodic intervals, the object contains only the `processing_metrics` field, as shown in the following example.

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

The `processing_metrics` field includes a `ProcessingMetrics` object that has the following fields:

-   `wall_clock_since_first_byte_received` is the amount of real time in seconds that has passed since the service received the first byte of input audio. Values in this field are generally multiples of the specified metrics interval, with two differences:
    -   Values might not reflect exact intervals such as 0.25, 0.5, and so on. Actual values might, for example, be 0.27, 0.52, and so on, depending on when the service receives and processes audio.
    -   Values for transcription events are not related to the processing interval. The service returns event-driven results as they occur.
-   `periodic` indicates whether the metrics apply to a periodic interval or to a transcription event:
    -   `true` means that the response was triggered by a processing interval. The information contains processing metrics only.
    -   `false` means that the response was triggered by a transcription event. The information contains processing metrics plus transcription results.

    Use this field to identify why the service generated the response and to filter different results if necessary.
-   `processed_audio` includes a `ProcessedAudio` object that provides detailed timing information about the service's processing of the input audio.

The `ProcessedAudio` object includes the following fields. All of the fields refer to seconds of audio as of this response. Only the `wall_clock_since_first_byte_received` field refers to elapsed real-time.

-   `received` is the seconds of audio that the service has received.
-   `seen_by_engine` is the seconds of audio that the service has passed to its speech processing engine.
-   `transcription` is the seconds of audio that the service has processed for speech recognition.
-   `speaker_labels` is the seconds of audio that the service has processed for speaker labels. The response includes this field only if you request speaker labels.

The speech processing engine analyzes the input audio multiple times. The `processed_audio` object shows values for audio that the engine has processed and will not read again. Processed audio has no effect on future recognition hypotheses.

The metrics indicate the progress and complexity of the engine's processing:

-   `seen_by_engine` is audio that the service has read and passed to the engine at least once.
-   `received` - `seen_by_engine` is audio that has been buffered at the service but has not yet been seen or processed by the engine.
-   The relationship between the times is `received` >= `seen_by_engine` >= `transcription` >= `speaker_labels`.

The following relationships can also be helpful in understanding the results:

-   The values of the `received` and `seen_by_engine` fields are greater than the values of the `transcription` and `speaker_labels` fields during speech recognition processing. The service must receive the audio before it can begin to process results.
-   The values of the `received` and `seen_by_engine` fields are identical when the service has finished processing the audio. The final values of the fields can be greater than the values of the `transcription` and `speaker_labels` fields by a fractional number of seconds.
-   The value of the `speaker_labels` field typically trails the value of the `transcription` field during speech recognition processing. The values of the `transcription` and `speaker_labels` fields are identical when the service has finished processing the audio.

### Processing metrics example: WebSocket interface
{: #processing_metrics_example_websocket}

The following example shows the `start` message that is passed for a request to the WebSocket interface. The request enables processing metrics and sets the processing metrics interval to 0.25 seconds. It also sets the `interim_results` and `speaker_labels` parameters to `true`. The audio contains the simple message "hello world long pause stop."

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

The following example output shows the first few processing metrics results that the service returns for the request.

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

### Processing metrics example: Asynchronous HTTP interface
{: #processing_metrics_example_http}

The following example shows a speech recognition request for the `/v1/recognitions` method of the asynchronous HTTP interface. The request enables processing metrics and specifies an interval of 0.25 seconds. The audio file again includes the message "hello world long pause stop".

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?processing_metrics=true&processing_metrics_interval=0.25"
```
{: pre}

The following example output shows the first two processing metrics results that the service returns for the request.

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

## Audio metrics
{: #audio_metrics}

Audio metrics provide detailed information about the signal characteristics of the input audio. The results provide aggregated metrics for the entire input audio at the conclusion of speech processing. For a technically sophisticated user, the metrics can provide meaningful insight into the detailed characteristics of the audio.

You can use audio metrics to provide a real-time indication of a problem with the input audio and possibly even a potential solution. For example, you can provide a message such as "There is too much noise in the background" or "Please come closer to the microphone." You can also use an offline analytical tool to review the signal characteristics and suggest data that is suitable for future model updates.

### Requesting audio metrics
{: #audio_metrics_request}

To request audio metrics, set the `audio_metrics` boolean parameter to `true`. By default, the service returns no metrics.

-   With the WebSocket interface, you specify the parameter with the JSON `start` message for a speech recognition request.
-   With the HTTP interfaces, you specify a query parameter with a speech recognition request.

The service returns audio metrics with the final transcription results. It returns the metrics just once for the entire audio stream. Even if the service returns multiple transcription results (for different blocks of audio), it returns only a single instance of the metrics at the very end of the results.

The WebSocket interface accepts multiple audio streams (or files) with a single connection. You delimit different streams by sending `stop` messages or empty binary blobs to the service. In this case, the service returns separate metrics for each delimited audio stream.

### Understanding audio metrics
{: #audio_metrics_understand}

The service return the metrics in the `audio_metrics` field of the `SpeechRecognitionResults` object.

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

The `audio_metrics` field includes an `AudioMetrics` object that has two fields:

-   `sampling_interval` indicates the interval in seconds (typically 0.1 seconds) at which the service calculated the audio metrics.
-   `accumulated` includes an `AudioMetricsDetails` object that provides detailed information about the signal characteristics of the input audio.

The following fields of the `AudioMetricsDetails` object provide unary values:

-   `final` indicates whether the metrics are for the end of the audio stream, meaning that transcription is complete. Currently, the field is always `true`. The service returns metrics just once per audio stream. The results provide aggregated metrics for the complete stream.
-   `end_time` specifies the end time in seconds of the audio to which the metrics apply. Because the metrics apply to the entire audio stream, the end time is always the length of the audio.
-   `signal_to_noise_ratio` provides the signal-to-noise ratio (SNR) for the audio signal. The value indicates the ratio of speech to noise in the audio. A valid value lies in the range of 0 to 100 decibels (dB). The service omits the field if it cannot compute the SNR for the audio.
-   `speech_ratio` is the ratio of speech to non-speech segments in the audio signal. The value lies in the range of 0.0 to 1.0.
-   `high_frequency_loss` indicates the probability that the audio signal is missing the upper half of its frequency content.
    -   A value close to 1.0 typically indicates artificially up-sampled audio, which negatively impacts the accuracy of the transcription results.
    -   A value at or near 0.0 indicates that the audio signal is good and has a full spectrum.
    -   A value around 0.5 means that detection of the frequency content is unreliable or unavailable.

The following fields of the `AudioMetricsDetails` object provide histograms for signal characteristics. Each field provides an array of `AudioMetricsHistogramBin` objects. A single unit in each histogram is calculated based on a `sampling_interval` length of audio.

-   `direct_current_offset` defines a histogram of the cumulative direct current (DC) component of the audio signal.
-   `clipping_rate` defines a histogram of the clipping rate for the audio segments. The clipping rate is defined as the fraction of samples in the segment that reach the maximum or minimum value that is offered by the audio quantization range.

    The service auto-detects either a 16-bit Pulse-Code Modulation (PCM) audio range (-32768 to +32767) or a unit range (-1.0 to +1.0). The clipping rate is between 0.0 and 1.0. Higher values indicate possible degradation of speech recognition.
-   `speech_level` defines a histogram of the signal level in segments of the audio that contain speech. The signal level is computed as the Root-Mean-Square (RMS) value in a decibel (dB) scale normalized to the range 0.0 (minimum level) to 1.0 (maximum level).
-   `non_speech_level` defines a histogram of the signal level in segments of the audio that do not contain speech. The signal level is computed as the RMS value in a decibel scale normalized to the range 0.0 (minimum level) to 1.0 (maximum level).

Each `AudioMetricsHistogramBin` object describes a bin with defined `begin` and `end` boundaries. Each bin indicates the `count` or number of values in the range of signal characteristics for that bin. The first and last bins of a histogram are the boundary bins. They cover the intervals between negative infinity and the first boundary, and between the last boundary and positive infinity, respectively.

### Audio metrics example
{: #audio_metrics_example}

The following example shows a speech recognition request with the synchronous HTTP interface that returns audio metrics. The audio file includes the simple message "hello world long pause stop".

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitize?audio_metrics=true"
```
{: pre}

The response includes the audio metrics for the complete input audio, which has a duration of 7.0 seconds. The input audio has slightly more speech than non-speech segments: 37 to 33 segments, for a `speech_ratio` of 0.529. The clipping rate is very low, indicating high-quality input audio.

The `high_frequency_loss` field has a value of 0.5, meaning that the service's detection of the frequency content is unreliable or unavailable. The results omit the `signal_to_noise_ratio` field because the service could not calculate the SNR for the audio.

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
