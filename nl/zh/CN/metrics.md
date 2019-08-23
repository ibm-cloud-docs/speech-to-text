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

# 度量值功能
{: #metrics}

{{site.data.keyword.speechtotextfull}} 服务可以为语音识别请求返回两种类型的可选度量值：

-   [处理度量值](#processing_metrics)，用于提供有关服务的输入音频处理的周期性信息。使用这些度量值可测量服务转录音频的进度。处理度量值可用于 WebSocket 和异步 HTTP 接口。
-   [音频度量值](#audio_metrics)，用于提供有关输入音频信号特征的信息。使用这些度量值可确定音频的特征和质量。音频度量值可用于所有语音识别接口。

缺省情况下，服务不会为请求返回度量值。

## 处理度量值
{: #processing_metrics}

处理度量值提供有关服务的输入音频分析的详细计时信息。服务以指定的时间间隔返回度量值以及转录事件，例如中间结果和最终结果。

这些度量值包括有关服务已接收的音频量、服务已传输到语音识别引擎的音频量，以及服务已处理音频的时间长度的统计信息。如果请求说话者标签，那么这些信息中还会显示服务为了确定说话者标签而处理的音频量。

处理度量值可帮助您测量识别请求的进度。此外，处理度量值还有助于区分由于以下原因造成的结果缺失：

-   缺少音频。
-   提交的音频中缺少语音。
-   服务器上的引擎停止，以及客户机与服务器之间的网络停止。为了区分是引擎还是网络停止，结果为周期性值，而不是基于事件的值。

这些度量值还可以通过检查周期性到达时间来帮助估算响应抖动。度量值以恒定时间间隔生成，因此到达时间的任何差异都是由抖动引起的。

### 请求处理度量值
{: #processing_metrics_request}

要请求处理度量值，请使用以下可选参数：

-   `processing_metrics` 是一个布尔值，用于指示服务是否要返回处理度量值。指定 `true` 以请求这些度量值。缺省情况下，服务不会返回度量值。
-   `processing_metrics_interval` 是一个浮点数，用于指定服务返回度量值的时间间隔（按实际时钟时间，以秒为单位）。缺省情况下，服务每秒返回一次度量值。除非 `processing_metrics` 参数设置为 `true`，否则服务会忽略此参数。

    此参数接受的最小值为 0.1 秒。精度级别不受限制，因此可以指定 0.25 和 0.125 之类的值。服务不会强制实施最大值。

如何提供参数以及服务如何返回处理度量值因接口而异：

-   使用 WebSocket 接口时，可为语音识别请求指定带有 JSON `start` 消息的参数。服务将按请求的时间间隔实时计算并返回度量值。
-   使用异步 HTTP 接口时，可在语音识别请求中指定查询参数。服务将按请求的时间间隔计算度量值，但会返回音频的所有度量值以及最终转录结果。

服务还会返回针对转录事件的处理度量值。如果使用 WebSocket 接口请求中间结果，那么接收度量值的频率可能大于请求的时间间隔。

要仅接收针对转录事件而不是周期性时间间隔的处理度量值，请将处理时间间隔设置为较大的数字。如果时间间隔大于音频的持续时间，那么服务仅返回针对转录事件的处理度量值。

### 了解处理度量值
{: #processing_metrics_understand}

服务将在 `SpeechRecognitionResults` 对象的 `processing_metrics` 字段中返回处理度量值。对于按周期性时间间隔生成的度量值，对象仅包含 `processing_metrics` 字段，如以下示例中所示。

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

`processing_metrics` 字段包含的 `ProcessingMetrics` 对象具有以下字段：

-   `wall_clock_since_first_byte_received` 是自服务收到输入音频的第一个字节以来经过的实际时间长度（以秒为单位）。此字段中的值通常是指定度量值时间间隔的倍数，但有两点不同：
    -   值可能反映的不是精确的时间间隔，如 0.25、0.5 等。例如，实际值可能是 0.27、0.52 等，具体取决于服务何时收到和处理音频。
    -   针对转录事件的值与处理时间间隔无关。服务将在发生事件驱动的结果时返回这些结果。
-   `periodic` 指示度量值是应用于周期性时间间隔还是应用于转录事件：
    -   `true` 表示响应是由处理时间间隔触发的。这些信息仅包含处理度量值。
    -   `false` 表示响应是由转录事件触发的。这些信息包含处理度量值以及转录结果。

    使用此字段可确定服务生成响应的原因，并根据需要过滤不同的结果。
-   `processed_audio` 包含 `ProcessedAudio` 对象，用于提供有关服务的输入音频处理的详细计时信息。

`ProcessedAudio` 对象包含以下字段。所有字段都指的是与此响应相关的音频秒数。仅 `wall_clock_since_first_byte_received` 字段指的是经过的实际时间。

-   `received` 是服务收到的音频秒数。
-   `seen_by_engine` 是服务已传递到其语音处理引擎的音频秒数。
-   `transcription` 是服务为了进行语音识别而处理的音频秒数。
-   `speaker_labels` 是服务为了获取说话者标签而处理的音频秒数。仅当请求说话者标签时，响应才会包含此字段。

语音处理引擎会对输入音频进行多次分析。`processed_audio` 对象显示引擎已处理且不会再次读取的音频的值。已处理的音频对未来的识别假设没有影响。

度量值指示引擎处理的进度和复杂性：

-   `seen_by_engine` 是服务已读取并传递到引擎至少一次的音频。
-   `received` - `seen_by_engine` 是已在服务中缓冲但尚未由引擎查看或处理的音频。
-   这些时间之间的关系是 `received` >= `seen_by_engine` >= `transcription` >= `speaker_labels`。

以下关系也有助于了解结果：

-   在语音识别处理期间，`received` 和 `seen_by_engine` 字段的值大于 `transcription` 和 `speaker_labels` 字段的值。服务必须收到音频后，才能开始处理结果。
-   服务处理完音频后，`received` 和 `seen_by_engine` 字段的值完全相同。这两个字段的最终值可能比 `transcription` 和 `speaker_labels` 字段的值稍大几秒。
-   在语音识别处理期间，`speaker_labels` 字段的值通常小于 `transcription` 字段的值。但服务处理完音频后，`transcription` 和 `speaker_labels` 字段的值完全相同。

### 处理度量值示例：WebSocket 接口
{: #processing_metrics_example_websocket}

以下示例显示了为对 WebSocket 接口发出的请求传递的 `start` 消息。请求启用处理度量值，并将处理度量值时间间隔设置为 0.25 秒。此外，还将 `interim_results` 和 `speaker_labels` 参数设置为 `true`。音频包含简单的消息“hello world”以及较长的停顿。

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

以下示例输出显示服务为请求返回的前几个处理度量值结果。

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

### 处理度量值示例：异步 HTTP 接口
{: #processing_metrics_example_http}

以下示例显示了异步 HTTP 接口的 `/v1/recognitions` 方法的语音识别请求。请求启用处理度量值，并指定时间间隔为 0.25 秒。音频文件同样包含消息“hello world”以及较长的停顿。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?processing_metrics=true&processing_metrics_interval=0.25"
```
{: pre}

以下示例输出显示了服务为请求返回的前两个处理度量值结果。

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

## 音频度量值
{: #audio_metrics}

音频度量值提供有关输入音频信号特征的详细信息。在语音处理结束时，结果中提供整个输入音频的聚集度量值。对于技术成熟的用户，这些度量值可以提供对音频详细特征的有意义洞察。

可以使用音频度量值来实时指示输入音频中的问题，甚至可能提供潜在的解决方案。例如，可以提供“背景中噪声太多”或“请更靠近麦克风”等消息。您还可以使用脱机分析工具来查看信号特征，并建议适合未来模型更新的数据。

### 请求音频度量值
{: #audio_metrics_request}

要请求音频度量值，请将 `audio_metrics` 布尔值参数设置为 `true`。缺省情况下，服务不会返回度量值。

-   使用 WebSocket 接口时，可为语音识别请求指定带有 JSON `start` 消息的参数。
-   使用 HTTP 接口时，可在语音识别请求中指定查询参数。

服务会返回音频度量值以及最终转录结果。对于整个音频流，服务仅返回一次度量值。即使服务会返回多个转录结果（对于不同的音频块），也仅在结果的最末尾返回一次度量值。

WebSocket 接口通过单个连接接受多个音频流（或文件）。通过向服务发送 `stop` 消息或空的二进制 Blob，可对不同的流定界。在本例中，服务将针对每个定界的音频流返回单独的度量值。

### 了解音频度量值
{: #audio_metrics_understand}

服务将在 `SpeechRecognitionResults` 对象的 `audio_metrics` 字段中返回度量值。

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

`audio_metrics` 字段包含的 `AudioMetrics` 对象具有两个字段：

-   `sampling_interval` 指示服务计算音频度量值的时间间隔（以秒为单位，通常为 0.1 秒）。
-   `accumulated` 包含 `AudioMetricsDetails` 对象，用于提供有关输入音频信号特征的详细信息。

`AudioMetricsDetails` 对象的以下字段提供一元值：

-   `final` 指示度量值是否在音频流结束（即转录完成时）时提供。目前，该字段始终为 `true`。服务仅为每个音频流返回一次度量值。结果会为完整流提供聚集度量值。
-   `end_time` 指定音频中应用度量值的结束时间（以秒为单位）。由于度量值会应用于整个音频流，因此结束时间始终等于音频的长度。
-   `signal_to_noise_ratio` 提供音频信号的信噪比 (SNR)。该值指示音频中语音与噪声的比率。有效值位于 0 到 100 分贝 (dB) 范围内。如果服务无法计算音频的 SNR，那么将省略此字段。
-   `speech_ratio` 是音频信号中语音段与非语音段的比率。该值位于 0.0 到 1.0 范围内。
-   `high_frequency_loss` 指示音频信号的频率内容缺少上半部分的概率。
    -   接近 1.0 的值通常指示人工上采样的音频，这会对转录结果的准确性产生负面影响。
    -   值等于或接近 0.0 指示音频信号良好且有完整的频谱。
    -   值约为 0.5 表示频率内容检测不可靠或不可用。

`AudioMetricsDetails` 对象的以下字段提供信号特征的直方图。每个字段提供 `AudioMetricsHistogramBin` 对象的数组。每个直方图中的单个单元根据音频的 `sampling_interval` 长度进行计算。

-   `direct_current_offset` 定义音频信号的累积直流 (DC) 组件的直方图。
-   `clipping_rate` 定义音频段削波率的直方图。削波率定义为段中达到音频量化范围所提供的最大值或最小值的样本所占比例。

    服务会自动检测 16 位脉冲编码调制 (PCM) 音频范围（-32768 到 +32767）或单位范围（-1.0 到 +1.0）。削波率在 0.0 到 1.0 之间。较高的值指示语音识别可能降级。
-   `speech_level` 定义包含语音的音频段中信号级别的直方图。信号级别以均方根 (RMS) 值进行计算，以分贝 (dB) 为量程，已规范化为 0.0（最低级别）到 1.0（最大级别）范围。
-   `non_speech_level` 定义不包含语音的音频段中信号级别的直方图。信号级别以 RMS 值进行计算，以分贝为量程，已规范化为 0.0（最低级别）到 1.0（最大级别）范围。

每个 `AudioMetricsHistogramBin` 对象描述一个定义有 `begin` 和 `end` 边界的分箱。每个分箱指示该分箱的信号特征范围内值的 `count`（即，值的数量）。直方图的第一个和最后一个分箱是边界分箱。这两个分箱分别涵盖负无穷大到第一个边界的区间，以及最后一个边界到正无穷大的区间。

### 音频度量值示例
{: #audio_metrics_example}

以下示例显示了同步 HTTP 接口的语音识别请求，用于返回音频度量值。音频文件包含简单的消息“hello world”以及较长的停顿。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitize?audio_metrics=true"
```
{: pre}

响应包括完整输入音频的音频度量值，音频持续时间为 7.0 秒。输入音频的语音段（37 段）比非语音段（33 段）略多，`speech_ratio` 为 0.529。削波率极低，指示输入音频质量高。

`high_frequency_loss` 字段的值为 0.5，表示服务的频率内容检测不可靠或不可用。由于服务无法计算音频的 SNR，因此结果省略了 `signal_to_noise_ratio` 字段。

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
