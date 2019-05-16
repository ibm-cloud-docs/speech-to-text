---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

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

# 了解识别结果
{: #basic-response}

无论使用哪个接口，{{site.data.keyword.speechtotextfull}} 服务都会返回反映您指定的输入和输出参数的转录结果。服务会以 UTF-8 字符集返回所有 JSON 响应内容。
{: shortdesc}

## 基本转录响应
{: #response}

对于[发出识别请求](/docs/services/speech-to-text/basic-request.html)中的示例，服务返回了以下响应。这些示例仅传递音频文件及其内容类型。音频读的是单个句子，词与词之间没有明显停顿。

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

服务返回了 `SpeechRecognitionResults` 对象，这是顶级响应对象。对于这些简单请求，该对象包含一个 `results` 字段和一个 `result_index` 字段：

-   `results` 字段提供有关转录结果的信息的数组。对于此示例，`alternatives` 字段包含 `transcript` 以及服务对结果的置信度 (`confidence`)。`final` 字段的值为 `true`，指示这些结果不会更改。
-   `result_index` 字段提供结果的唯一标识。此示例显示了请求的最终结果，其中包含没有停顿的单个音频文件，并且请求不包含其他参数。因此，服务返回了值为 `0` 的单个 `result_index` 字段，这始终为初始索引。

如果输入音频较复杂，或者请求包含其他参数，那么结果包含的信息可能会多得多。

### alternatives 字段
{: #responseAlternatives}

`alternatives` 字段提供有关转录结果的数组。对于此请求，该数组包含单个元素。

-   `confidence` 字段是分数，用于指示服务对抄本的置信度，对于此示例，置信度接近 90%。
-   `transcript` 字段提供转录的结果。

`final` 和 `result_index` 字段限定了这些字段的含义。

### final 字段
{: #responseFinal}

`final` 字段指示抄本是否显示的是最终转录结果：

-   对于最终结果，此字段为 `true`，这些结果保证不会更改。对于服务作为最终结果返回的抄本，服务不会再发送进一步的更新。
-   对于中间结果，此字段为 `false`，这些结果会随时更改。如果将 `interim_results` 参数用于 WebSocket 接口，那么服务在转录音频时，会以多个 `results` 字段的形式返回不断变化的中间假设。对于中间结果，`final` 字段始终为 `false`。对于音频的最终结果，服务会将此字段设置为 `true`。此后，服务不会再发送对该音频转录的进一步更新。

要获取中间结果，请使用 WebSocket 接口并将 `interm_results` 参数设置为 `true`。有关更多信息，请参阅[中间结果](/docs/services/speech-to-text/output.html#interim)。

### result_index 字段
{: #responseResultIndex}

`result_index` 字段提供对该请求唯一的结果标识。如果请求中间结果，那么服务会发送多个 `results` 字段，以表示输入音频不断变化的假设。同一音频的中间结果的索引始终具有相同的值，同一音频的最终结果的索引值也相同。

收到任何音频的最终结果后，服务就不会再对该请求的其余部分发送具有该索引的进一步结果。任何进一步结果的索引都将递增 1。

如果音频包含停顿或长时间静默，无论您是否请求中间结果，服务都可能返回具有不同索引的多个最终结果。有关更多信息，请参阅[停顿和静默](#pauses-silence)。

如果音频生成了多个最终结果，请将最终结果的 `transcript` 元素连接在一起，以组合成音频的完整转录。请根据 `result_index` 按顺序组合结果。可以忽略 `final` 字段为 `false` 的中间结果。

### 其他响应内容
{: #responseAdditional}

可用于语音识别的许多输出参数都会影响服务响应的内容。例如，以下参数会使服务返回有关音频的其他信息：

-   `max_alternatives` 参数用于请求多个替代抄本。`alternatives` 数组包含表示每个替代项的单独元素。数组的每个元素都包含一个 `transcript` 字段。只有最有可能的替代项会包含 `confidence` 字段。
-   `word_confidence` 和 `timestamps` 参数用于请求有关抄本中各个词的其他信息。`alternatives` 数组包含具有这些名称的其他字段。
-   `keywords` 和 `keywords_threshold` 参数用于请求识别关键字。`results` 数组包含 `keywords_result` 字段。
-   `word_alternatives_threshold` 参数用于请求各个词的声学类似结果。`results` 数组包含 `word_alternatives` 字段。
-   WebSocket 接口的 `interim_results` 参数用于请求中间转录假设。如前所述，服务会在转录音频时发送多个响应。
-   `speaker_labels` 参数用于标识多个参与者交流中的各个说话者。响应包含与 `results` 和 `results_index` 字段位于同一级别的 `speaker_labels` 字段。

有关这些参数以及可能影响服务响应的其他参数的更多信息，请参阅[输出功能](/docs/services/speech-to-text/output.html)。有关所有可用参数的更多信息，请参阅[参数摘要](/docs/services/speech-to-text/summary.html)。

## 停顿和静默
{: #pauses-silence}

服务会转录整个音频流，直到流结束或发生超时为止。但是，如果音频在所读词或短语之间包含停顿或长时间静默，那么识别结果可能包含多个最终结果。

服务用于确定单独最终结果的缺省停顿时间间隔大约为 1 秒。对于大多数语言，停顿时间间隔恰好为 0.8 秒；对于中文，时间间隔为 0.6 秒。不能更改停顿时间间隔的持续时间。

服务如何返回结果取决于使用的接口。以下示例显示了来自 HTTP 和 WebSocket 接口的包含两个最终结果的响应。这两种情况使用的输入音频相同。音频读的是短语“one two three four five six”，在词“three”和“four”之间有 1 秒停顿。

-   *对于 HTTP 接口*，服务会发送单个 `SpeechRecognitionResults` 对象。`alternatives` 数组对于每个最终结果都有一个单独的元素。响应具有单个 `result_index` 字段，其值为 `0`。

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
        {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

-   *对于 WebSocket 接口*，服务通过两个不同的 `SpeechRecognitionResults` 对象发送两个单独的响应。每个响应对象具有不同的 `result_index` 字段，对于第一个响应，其值为 `0`，对于第二个响应，其值为 `1`。

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
      ]
      "result_index": 0
    }
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
          "final": true
        }
      ],
      "result_index": 1
    }
    ```
    {: codeblock}

如果结果包含多个最终结果，请将最终结果的 `transcript` 元素连接在一起，以组合成音频的完整转录。

流式音频中的 30 秒静默可能会导致[不活动超时](/docs/services/speech-to-text/input.html#timeouts-inactivity)。
{: note}

## 迟疑标记
{: #hesitation}

服务发现语音中有短暂补白或停顿时，可以在抄本中包含迟疑标记。此类停顿也称为非流利项，可能包括“uhm”、“uh”、“hmm”等补白以及相关的非词汇话语。除非您需要将这些话语用于应用程序，否则可以从抄本中安全地过滤掉迟疑标记。

在英语中，服务使用的迟疑标记为 `%HESITATION`，如以下示例中所示。其他语言可能使用不同的标记；例如，日语使用的是标记 `D_`。

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

迟疑标记还可以出现在抄本的其他字段中。例如，如果为抄本中的单个词请求了[词时间戳记](/docs/services/speech-to-text/output.html#word_timestamps)，那么服务会报告每个迟疑标记的开始时间和结束时间。

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            . . .
            [
              "that",
              7.31,
              7.69
            ],
            [
              "%HESITATION",
              7.69,
              7.98
            ],
            [
              "that's",
              7.98,
              8.41
            ],
            [
              "a",
              8.41,
              8.48
            ],
            . . .
          ],
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## 首字母大写
{: #capitalization}

对于大多数语言，服务不会在响应抄本中使用首字母大写。如果首字母大写对于应用程序很重要，那么必须对每个句子的第一个词以及需要首字母大写的其他任何词汇进行首字母大写。

*仅对于美国英语*，服务会对许多专有名词进行首字母大写。例如，对于指定短语，服务会返回以下文本：

```
Barack Obama graduated from Columbia University
```
{: codeblock}

对于其他语言，服务会返回以下文本：

```
barack obama graduated from columbia university
```
{: codeblock}

服务始终会将此首字母大写功能应用于美国英语，不管您是否使用了智能格式设置。

## 标点符号
{: #punctuation}

缺省情况下，服务不会在响应抄本中插入标点。您必须向服务结果添加所需的任何标点。

对于美国英语，可以使用智能格式设置来指导服务使用标点符号（例如，逗号、句点、问号和惊叹号）替换特定关键字字符串。有关更多信息，请参阅[智能格式设置](/docs/services/speech-to-text/output.html#smart_formatting)。
