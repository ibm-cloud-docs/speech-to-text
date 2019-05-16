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

# 输出功能
{: #output}

{{site.data.keyword.speechtotextshort}} 服务提供了以下功能来指示服务要包含在语音识别请求的转录结果中的信息。所有输出参数都是可选的。
{: shortdesc}

-   有关每个服务接口的简单语音识别请求的示例，请参阅[发出识别请求](/docs/services/speech-to-text/basic-request.html)。
-   有关语音识别响应的示例和描述，请参阅[了解识别结果](/docs/services/speech-to-text/basic-response.html)。服务会以 UTF-8 字符集返回所有 JSON 响应内容。

-   有关所有可用语音识别参数的列表（按字母顺序排列），包括其状态（一般可用或 Beta）和支持的语言，请参阅[参数摘要](/docs/services/speech-to-text/summary.html)。

## 说话者标签
{: #speaker_labels}

说话者标签是 Beta 功能，可用于美国英语、日语、西班牙语和英国英语，前三种语言的宽带和窄带模型均适用，英国英语仅限窄带模型。
{: note}

说话者标签用于标识哪些人在多参与者交流中说了哪些词。（标注谁在何时讲话有时也称为*说话者归类*。）可以使用这些信息来编制音频流（如对呼叫中心的联系）中每个人的抄本。或者，可以使用这些信息来制作与会话式机器人或头像交流的动画。为了获得最佳性能，请使用长度至少为一分钟的音频。

说话者标签针对双人会话场景进行了优化。它们最适合涉及两人长时间交流的电话会话。此功能最多可以处理六个说话者，但两个以上的说话者可能会使性能不稳定。双人交流通常是通过窄带媒体执行的，但您可以将说话者标签用于支持的窄带和宽带模型。

要使用此功能，请将识别请求的 `speaker_labels` 参数设置为 `true`；缺省情况下，此参数为 `false`。服务会按音频中的单个词来识别说话者。服务依赖于词的开始时间和结束时间来识别其说话者。因此，启用说话者标签还会强制将 `timestamps` 参数设置为 `true`（请参阅[词时间戳记](/docs/services/speech-to-text/output.html#word_timestamps)）。

### 说话者标签示例
{: #speakerLabelsExample}

以下示例请求显示了包含说话者标签的响应。与 `timestamps` 数组中的每个元素关联的数字值是音频中词的开始时间和结束时间。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-multi.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel&speaker_labels=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "hello",
              0.68,
              1.19
            ],
            [
              "yeah",
              1.47,
              1.93
            ],
            [
              "yeah",
              1.96,
              2.12
            ],
            [
              "how's",
              2.12,
              2.59
            ],
            [
              "Billy",
              2.59,
              3.17
            ],
            . . .
          ]
          "confidence": 0.82,
          "transcript": "hello yeah yeah how's Billy "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0,
  "speaker_labels": [
    {
      "from": 0.68,
      "to": 1.19,
      "speaker": 2,
      "confidence": 0.42,
      "final": false
    },
    {
      "from": 1.47,
      "to": 1.93,
      "speaker": 1,
      "confidence": 0.52,
      "final": false
    },
    {
      "from": 1.96,
      "to": 2.12,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.12,
      "to": 2.59,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.59,
      "to": 3.17,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    . . .
  ]
}
```
{: codeblock}

`transcript` 字段显示音频的最后一个抄本，其中列出了所有参与者所说的词。通过将说话者标签与时间戳记进行比较和同步，可以按会话实际发生情况来重新组合会话。

<table style="width:50%">
  <caption>表 1. 说话者标签示例</caption>
  <tr>
    <th style="text-align:left">时间戳记</th>
    <th style="text-align:left">说话者标签</th>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"hello",<br/>
      0.68,<br/>
      1.19</code>
    </td>
    <td>
      <code>"from": 0.68,<br/>
      "to": 1.19,<br/>
      "speaker": 2,<br/>
      "confidence": 0.42,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.47,<br/>
      1.93</code>
    </td>
    <td>
      <code>"from": 1.47,<br/>
      "to": 1.93,<br/>
      "speaker": 1,<br/>
      "confidence": 0.52,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.96,<br/>
      2.12</code>
    </td>
    <td>
      <code>"from": 1.96,<br/>
      "to": 2.12,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"how's",<br/>
      2.12,<br/>
      2.59</code>
    </td>
    <td>
      <code>"from": 2.12,<br/>
      "to": 2.59,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"Billy",<br/>
      2.59,<br/>
      3.17</code>
    </td>
    <td>
      <code>"from": 2.59,<br/>
      "to": 3.17,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
</table>

结果清楚地指示了这个双人交流是如何开始的：

-   **说话者 2** - "Hello?"
-   **说话者 1** - "Yeah?"
-   **说话者 2** - "Yeah, how's Billy?"

在示例中，`confidence` 字段指示服务对其说话者标识的置信度。对于每个词，`final` 字段的值均为 `false`。此值指示服务仍在处理音频。服务在完成处理之前，可能会更改其对说话者的标识或对单个词的置信度。

### 说话者标签的说话者标识
{: #speakerLabelsIDs}

此示例还说明了说话者标识的一个十分有意思的方面。在 `speaker` 字段中，第一个说话者的标识为 `2`，第二个说话者的标识为 `1`。随着服务不断处理音频，服务对说话者模式会有更好的了解。因此，服务在生成最终结果之前，可能会更改单个词的说话者标识，还可能会添加和除去说话者。

因此，说话者标识可能不是顺序、连续或有序的。例如，标识通常从 `0` 开始，但在先前的示例中，最早出现的标识为 `1`。服务可能是根据对音频的进一步分析，省略了对说话者 `0` 的较早的词分配。这样的省略操作可能还会对后面的说话者执行。省略操作可能导致编号出现间隔，并只为标注的两个说话者（例如，`0` 和 `2`）生成结果。另一个考虑因素是，说话者可能离开了会话。因此，仅参与会话早期阶段的参与者可能不会在后续结果中出现。

### 请求说话者标签的中间结果
{: #speakerLabelsInterim}

通过 WebSocket 接口，可以请求中间结果以及说话者标签（请参阅[中间结果](/docs/services/speech-to-text/output.html#interim)）。最终结果通常会优于中间结果。但是，中间结果可帮助识别抄本以及说话者标签分配的变化过程。中间结果可以指示瞬态说话者和标识在何处出现或消失。但是，服务可以复用初始识别到，但在后续重新考虑并省略的说话者标识。因此，一个标识在中间结果和最终结果中可能指的是两个不同的说话者。

请求中间结果和说话者标签时，长音频流的最终结果可能会在初始中间结果返回之后很久才到达。此外，某些中间结果还可能仅包含 `speaker_labels` 字段，而不包含 `results` 和 `result_index` 字段。如果不请求中间结果，那么服务返回的最终结果中包含 `results` 和 `result_index` 字段以及单个 `speaker_labels` 字段。

音频流完成时或对超时做出响应时（以先发生者为准），服务会发送最终结果。无论是哪种情况，服务都只会针对所返回的说话者标签的最后一个词将 `final` 字段设置为 `true`。

### 说话者标签的性能注意事项
{: #speakerLabelsPerformance}

如上所述，说话者标签功能已针对双人会话（例如，与呼叫中心的通信）进行了优化。因此，您需要考虑以下潜在的性能问题：

-   单个说话者的音频性能可能不佳。音频质量或说话者声音的变化可能会导致服务识别到不存在的额外说话者。这类说话者称为幻影。
-   与此类似，有主说话者的音频（如播客）的性能也可能不佳。服务往往会错过讲话时间较短的说话者，并且还可能会生成幻影。
-   超过 6 个说话者的音频性能不确定。此功能最多可处理 6 个说话者。
-   短时间话语的性能在准确性方面可能低于长时间话语。参与者讲话时间越长（每个说话者至少讲 30 秒），服务生成的结果越好。可用于每个说话者的相对音频量也会影响性能。
-   对于前 30 秒语音，性能可能会下降。对于 1 分钟后的音频，性能通常会提高到合理水平。

与所有转录一样，性能还可能受到音频质量差、背景噪声、个人讲话方式、说话者语音重叠以及音频其他方面的影响。

## 关键字识别
{: #keyword_spotting}

关键字识别功能用于在抄本中检测指定的字符串。服务可能多次识别到同一个关键字，并且每次识别到时都会进行报告。但服务仅在最终结果中（而不是在中间结果中）识别关键字。缺省情况下，服务不会执行关键字识别。

要使用关键字识别，必须同时指定以下两个参数：

-   使用 `keywords` 参数来指定要识别的字符串的数组。如果省略此参数或指定空数组，那么服务不会识别任何关键字。一个关键字字符串可以包含多个记号。例如，关键字 `Speech to Text` 有三个记号。

    对于美国英语，服务会将每个关键字进行规范化，以匹配口头和书面字符串。例如，服务会对数字进行规范化，以匹配其口头读法和书面写法。对于其他语言，关键字必须指定为其读法。

    通过单个请求最多可以识别 1000 个关键字。关键字不区分大小写。
-   使用 `keywords_threshold` 参数为关键字匹配指定介于 0.0 和 1.0 之间的概率。此阈值指示要使某个词与关键字相匹配，服务对该词必须具有的置信度级别下限。仅当关键字的置信度大于或等于指定的阈值时，才会在抄本中识别到该关键字。

    指定较小的阈值可能会生成许多匹配项。如果指定阈值，那么还必须指定一个或多个关键字。省略此参数不会返回匹配项。

关键字识别功能对于识别音频流中的关键字是必需的。无法通过处理最终抄本来识别关键字，因为抄本表示服务对输入音频的最佳解码结果。它不会包含置信度分数较低的记号，这些记号可能表示相关词。因此，将文本处理工具应用于客户端的抄本可能无法识别关键字。需要更丰富的解码结果表示法，并且该表示法仅在服务器上可用。

### 关键字识别结果
{: #keywordSpottingResults}

服务会在作为 `results` 数组中元素的 `keywords_result` 字段中返回结果。`keywords_result` 字段是可枚举属性的字典或关联数组。每个属性都通过指定的关键字进行标识，并包含对象的数组。服务会为针对关键字找到的每个匹配项返回数组的一个元素。每个匹配项的对象包含以下字段：

-   `normalized_text` 是规范化为输入音频中匹配的所说短语的指定关键字。
-   `start_time` 是匹配项的开始时间（以秒为单位）。
-   `end_time` 是匹配项的结束时间（以秒为单位）。
-   `confidence` 是服务对匹配项表示指定关键字的置信度。置信度必须等于或大于指定阈值，才能将匹配项包含在结果中。

对于服务找不到任何匹配项的关键字，会将其从数组中省略。在以下情况下，可能找不到关键字：

-   音频根本就不包含该关键字。没有关键字就是最明显的解释。
-   阈值设置得太高。服务可能识别到关键字，但其置信度级别较低，在这种情况下，会在结果中省略匹配项。
-   在说包含多个记号的关键字字符串（例如，`Speech to Text`）时，在其记号之间静默的时间太长。服务转录音频时，会将音频流切分成一系列的块。每个块表示一个连续的音频块，此块中没有超过半秒的静默时间间隔。它会构造由这些块组成的结果对象的数组。

    只有在以下情况下，服务才与多记号关键字相匹配：

    -   关键字的记号位于同一块中。
    -   记号相邻或者各记号的间隔时间不超过 0.1 秒。

    如果在关键字的两个记号之间有短暂的补白或非词汇话语（例如，“uhm”或“well”），那么可能会发生后一种情况。有关更多信息，请参阅[迟疑标记](/docs/services/speech-to-text/basic-response.html#hesitation)。

### 关键字识别示例
{: #keywordSpottingExample}

以下示例请求将 `keywords` 参数设置为由三个字符串组成的 URL 编码数组（`colorado`、`tornado` 和 `tornadoes`），并且 `keywords_threshold` 参数设置为 `0.5`。服务找到了符合条件的 `colorado` 和 `tornadoes` 实例。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?keywords=%22colorado%22%2C%22tornado%22%2C%22tornadoes%22&keywords_threshold=0.5"
```
{: pre}

```javascript
{
  "results": [
    {
      "keywords_result": {
        "colorado": [
          {
            "normalized_text": "Colorado",
            "start_time": 4.94,
            "confidence": 0.91,
            "end_time": 5.62
          }
        ],
        "tornadoes": [
          {
            "normalized_text": "tornadoes",
            "start_time": 1.52,
            "confidence": 1.0,
            "end_time": 2.15
          }
        ]
      },
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

## 最大替代项数
{: #max_alternatives}

`max_alternatives` 参数接受整数值，用于指示服务返回结果的 *n* 个最佳替代假设。缺省情况下，服务仅返回单个转录结果，这相当于将此参数设置为 `1`。通过将 `max_alternatives` 设置为大于 1 的数字，即表示要求服务返回该数量的最佳替代转录。（如果指定值 `0`，那么服务将使用缺省值 `1`。）

服务仅会针对所返回的最佳替代项报告置信度分数。在大多数情况下，这是要选择的替代项。

### 最大替代项数示例
{: #maximumAlternativesExample}

以下示例请求将 `max_alternatives` 参数设置为 `3`；因此，服务针对三个替代项中最可能的替代项报告了置信度。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?max_alternatives=3"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touched down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touch down is a line of
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

## 中间结果
{: #interim}

中间结果功能仅可用于 WebSocket 接口。
{: note}

中间结果是在服务返回其最终结果之前可能会更改的转录中间假设。服务一生成中间结果就会将其返回。对于以下情况，中间结果非常有用：

-   交互式应用程序和实时转录
-   长音频流，对其进行转录可能需要一段时间

相比最终结果，中间结果到达的速度更快，返回频率更高。可以使用中间结果来帮助应用程序更快响应，或者测量转录的进度。

要接收中间结果，请将 WebSocket 识别请求的 `start` 消息中的 `interim_results` JSON 参数设置为 `true`。如果省略 `interim_results` 参数或将其设置为 `false`，那么服务仅在音频结束时返回单个 JSON 抄本。请遵循以下准则来使用此参数：

-   如果要执行脱机转录或批量转录，不需要最短等待时间，以及需要的是所有音频的单个 JSON 结果，请省略此参数或将其设置为 `false`。
-   如果希望在服务处理音频时结果以渐进方式到达，或者如果需要具有最短等待时间的结果，请将此参数设置为 `true`。请记住，服务在处理更多音频时，可能会更新中间结果。

中间结果通过将 JSON 响应中的 `final` 字段设置为 `false` 来指示。服务在处理进一步音频时，可能会使用更准确的转录来更新此类结果。最终结果通过将 `final` 字段设置为 `true` 来标识。服务不会对最终结果做进一步更新。

### 中间结果示例
{: #interimResultsExample}

以下缩略示例请求的是 WebSocket 请求的中间结果。在其响应中，服务仅对于最终结果将 `final` 属性设置为 `true`。

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token;
var websocket = new WebSocket(wsURI);

websocket.onopen = function(evt) { onOpen(evt) };
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050',
    interim_results: true
  };
  websocket.send(JSON.stringify(message));
  websocket.send(blob);
}

websocket.onmessage = function(evt) { onMessage(evt) };
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several to "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  . . .
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
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

## 词替代项
{: #word_alternatives}

词替代项功能（也称为“混淆网络”）用于报告有关输入音频中词的发声相似替代项的假设。例如，词 `Austin` 可能是音频中某个词的最佳假设。但在同一时间间隔内，词 `Boston` 是另一个可能的假设。这些假设有相同的开始时间和结束时间，但有不同的拼写以及通常不同的置信度分数。

缺省情况下，服务不会报告词替代项。要指示您希望接收替代假设，请使用 `word_alternatives_threshold` 参数来指定介于 0.0 到 1.0 之间的概率。此阈值指示要使服务将某个假设作为词替代项返回，服务对该假设必须具有的置信度级别下限。仅当假设的置信度大于或等于指定阈值时，才会返回该假设。

可以将词替代项视为切分成更小时间间隔（或分箱）的抄本时间线。每个分箱可以有一个或多个具有不同拼写和置信度的假设。`word_alternatives_threshold` 参数用于控制服务返回的结果的密度。指定较小的阈值可能会生成许多假设。

### 词替代项结果
{: #wordAlternativesResults}

服务会在作为 `results` 数组中元素的 `word_alternatives` 字段中返回结果。`word_alternatives` 字段是对象数组，每个对象为替代词提供以下字段：

-   `start_time` 指示返回其假设的词在输入音频中开始的时间（以秒为单位）。
-   `end_time` 指示返回其假设的词在输入音频中结束的时间（以秒为单位）。
-   `alternatives` 是假设对象的数组。每个对象都包含 `confidence`（用于指示服务对该假设的置信度分数）和 `word`（用于标识该假设）。置信度必须等于或大于指定阈值，才能将匹配项包含在结果中。服务按置信度对替代项排序。

### 词替代项示例
{: #wordAlternativesExample}

以下示例请求将 `word_alternatives_threshold` 指定为 `0.9`。输出包含多个词的潜在假设和置信度分数，以及这些词的开始时间和结束时间。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.9"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 1.01,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "several"
            }
          ],
          "end_time": 1.52
        },
        {
          "start_time": 1.52,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "tornadoes"
            }
          ],
          "end_time": 2.15
        },
        {
          "start_time": 3.39,
          "alternatives": [
            {
              "confidence": 0.96,
              "word": "severe"
            }
          ],
          "end_time": 3.77
        },
        {
          "start_time": 3.77,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "thunderstorms"
            }
          ],
          "end_time": 4.51
        },
        {
          "start_time": 4.51,
          "alternatives": [
            {
              "confidence": 0.97,
              "word": "swept"
            }
          ],
          "end_time": 4.81
        }
      ],
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

## 词置信度
{: #word_confidence}

`word_confidence` 参数指示服务是否为转录中的词提供置信度度量。缺省情况下，服务仅将最终抄本作为整体报告置信度度量。将 `word_confidence` 设置为 `true` 会指示服务为抄本中的每个单独的词报告置信度度量。

置信度度量指示服务基于声学证据对转录词正确性的估计。置信度分数范围为 0.0 到 1.0。

-   分数为 1.0 表示词的当前转录反映了最有可能的结果。
-   分数为 0.5 表示词正确的可能性为 50%。

### 词置信度示例
{: #wordConfidenceExample}

以下示例请求了转录中词的词置信度分数。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_confidence=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday ",
          "confidence": 0.89,
          "word_confidence": [
            [
              "several",
              1.0
            ],
            [
              "tornadoes",
              1.0
            ],
            [
              "touch",
              0.52
            ],
            [
              "down",
              0.90
            ],
            . . .
            [
              "on",
              0.31
            ],
            [
              "Sunday",
              0.99
            ]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## 词时间戳记
{: #word_timestamps}

`timestamps` 参数指示服务是否为其转录的词生成时间戳记。缺省情况下，服务不会报告时间戳记。将 `timestamps` 设置为 `true` 会指示服务报告每个词相对于音频开头的开始时间和结束时间（以秒为单位）。

### 词时间戳记示例
{: #wordTimestampsExample}

以下示例请求了转录中词的时间戳记。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?timestamps=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "several",
              1.01,
              1.52
            ],
            [
              "tornadoes",
              1.52,
              2.15
            ],
            [
              "touch",
              2.15,
              2.5
            ],
            [
              "down",
              2.5,
              2.81
            ],
            . . .
            [
              "on",
              5.62,
              5.74
            ],
            [
              "Sunday",
              5.74,
              6.34
            ]
          ],
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

## 智能格式设置
{: #smart_formatting}

智能格式设置是 Beta 功能，可用于美国英语、日语和西班牙语。
{: note}

`smart_formatting` 参数指示服务将以下字符串转换为更常规的表示法：

-   日期
-   时间
-   数字串和号码
-   电话号码
-   货币值
-   因特网电子邮件和 Web 地址

将 `smart_formatting` 参数设置为 `true` 可启用智能格式设置。缺省情况下，服务不会执行智能格式设置。

服务仅将智能格式设置应用于识别请求的最终抄本。文本规范化完成后，服务在将结果返回给客户机之前，才会应用智能格式设置。该转换可使抄本更容易阅读，并能对转录结果进行更好的后处理。

### 标点（美国英语）
{: #smartFormattingPunctuation}

对于美国英语，此功能还会指示服务用标点符号来替换音频中的以下话语关键字字符串。

<table style="width:50%">
  <caption>表 2. 对标点关键字进行智能格式设置</caption>
  <tr>
    <th style="width:45%; text-align:left">关键字字符串</th>
    <th style="text-align:center">生成的标点</th>
  </tr>
  <tr>
    <td>
      逗号
    </td>
    <td style="text-align:center">
      <code>,</code>
    </td>
  </tr>
  <tr>
    <td>
      句点
    </td>
    <td style="text-align:center">
      <code>.</code>
    </td>
  </tr>
  <tr>
    <td>
      问号
    </td>
    <td style="text-align:center">
      <code>?</code>
    </td>
  </tr>
  <tr>
    <td>
      惊叹号
    </td>
    <td style="text-align:center">
      <code>!</code>
    </td>
  </tr>
</table>

服务仅在适当的位置将这些关键字字符串转换为符号。例如，服务会将以下所说的短语

```
the warranty period is short period
```
{: codeblock}

转换为抄本中的以下文本：

```
the warranty period is short.
```
{: codeblock}

### 语言差异
{: #smartFormattingDifferences}

智能格式设置基于在抄本中存在明显的关键字。支持的语言以略有不同的方式处理智能格式设置。

#### 美国英语和西班牙语
{: #smartFormattingEnglishSpanish}

-   时间通过关键字（例如，`AM`、`PM` 或 `EST`）进行标识。
-   如果军用时间通过关键字 `hours` 进行标识，那么会对其进行转换。
-   电话号码必须为 `911` 或者以数字 `1` 开头的 10 位或 11 位数字。

#### 日语
{: #smartFormattingJapanese}

-   不会转换因特网电子邮件和 Web 地址。
-   电话号码必须是 10 位或 11 位数字，并以日本电话号码的有效前缀开头。例如，有效前缀包括 `03` 和 `090`。
-   英语词会转换为 ASCII (*hankaku*) 字符。例如，<code>&#65321;&#65314;&#65325;</code> 会转换为 `IBM`。
-   如果没有充分的上下文可用，那么可能不会转换歧义词。例如，不清楚<code>&#19968;&#26178;</code>和<code>&#21313;&#20998;</code>是否是指时间。
-   不管使用还是不使用智能格式设置，对标点的处理方式都相同。例如，根据概率计算，选择了<code>&#12459;&#12531;&#12510;</code>或 `,` 其中一项。

### 智能格式设置示例
{: #smartFormattingExample}

以下示例在识别请求中通过将 `smart_formatting` 参数设置为 `true` 来请求智能格式设置。以下各部分说明了智能格式设置对请求结果的影响。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?smart_formatting=true"
```
{: pre}

### 智能格式设置结果
{: #smartFormattingResults}

下表显示了使用和不使用智能格式设置的最终抄本的示例。抄本基于美国英语音频。

<table summary="每个标题行后跟多行示例，用于显示标题中标识的元素的智能格式设置效果。">
  <caption>表 3. 智能格式设置示例抄本</caption>
  <tr>
    <th id="without_formatting" style="width:45%; text-align:left">不使用智能格式设置</th>
    <th id="with_formatting" style="text-align:left">使用智能格式设置</th>
  </tr>
  <tr>
    <th id="Dates" colspan="2">
      <strong>日期</strong>
    </th>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on ten oh six nineteen seventy
    </td>
    <td headers="Dates with_formatting">
      I was born on 10/6/1970
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on the ninth of December nineteen hundred
    </td>
    <td headers="Dates with_formatting">
      I was born on 12/9/1900
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      Today is June sixth
    </td>
    <td headers="Dates with_formatting">
      Today is June 6
    </td>
  </tr>
  <tr>
    <th id="Times" colspan="2">
      <strong>时间</strong>
    </th>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      The meeting starts at nine thirty AM
    </td>
    <td headers="Times with_formatting">
      The meeting starts at 9:30 AM
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      I am available at seven EST
    </td>
    <td headers="Times with_formatting">
      I am available at 7:00 EST
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      We meet at oh seven hundred hours
    </td>
    <td headers="Times with_formatting">
      We meet at 0700 hours
    </td>
  </tr>
  <tr>
    <th id="Numbers" colspan="2">
      <strong>数字</strong>
    </th>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      The quantity is one million one hundred and one
    </td>
    <td headers="Numbers with_formatting">
      The quantity is 1000101
    </td>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      One point five is between one and two
    </td>
    <td headers="Numbers with_formatting">
      1.5 is between 1 and 2
    </td>
  </tr>
  <tr>
    <th id="phone_numbers" colspan="2">
      <strong>电话号码</strong>
    </th>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at nine one four two three seven one thousand
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 914-237-1000
    </td>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at one nine one four nine oh nine twenty six forty five
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 1-914-909-2645
    </td>
  </tr>
  <tr>
    <th id="currency_values" colspan="2">
      <strong>货币值</strong>
    </th>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      You owe me three thousand two hundred two dollars and sixty six
      cents
    </td>
    <td headers="currency_values with_formatting">
      You owe me $3202.66
    </td>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      The dollar rose to one hundred and nine point seven nine yen from
      one hundred and nine point seven two yen
    </td>
    <td headers="currency_values with_formatting">
      The dollar rose to 109.79 yen from 109.72 yen
    </td>
  </tr>
  <tr>
    <th id="internet_addresses" colspan="2">
      <strong>因特网电子邮件和 Web 地址</strong>
    </th>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      My email address is john dot doe at foo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      My email address is john.doe at foo.com
    </td>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      I saw the story on yahoo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      I saw the story on yahoo.com
    </td>
  </tr>
  <tr>
    <th id="Combinations" colspan="2">
      <strong>组合</strong>
    </th>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      The code is zero two four eight one and the date of service
      is May fifth two thousand and one
    </td>
    <td headers="Combinations with_formatting">
      The code is 02481 and the date of service is 5/5/2001
    </td>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      There are forty seven links on Yahoo dot com now
    </td>
    <td headers="Combinations with_formatting">
      There are 47 links on Yahoo.com now
    </td>
  </tr>
</table>

### 包含长时间停顿的示例抄本
{: #smartFormattingLongPauses}

如果话语包含足够长的静默，那么服务会将抄本拆分为两个或更多个最终音块。这会影响格式设置结果，如以下示例中所示。

<table>
  <caption>表 4. 长时间停顿的智能格式设置示例脚本</caption>
  <tr>
    <th style="width:45%; text-align:left">音频语音</th>
    <th style="text-align:left">设置格式后的转录结果</th>
  </tr>
  <tr>
    <td>
      My phone number is nine one four five five seven three
      three nine two
    </td>
    <td>
      "My phone number is 914-557-3392"
    </td>
  </tr>
  <tr>
    <td>
      My phone number is nine one four <em>&lt;pause&gt;</em> five five
      seven three three nine two
    </td>
    <td>
      "My phone number is 914"<br/>
      "5573392"
    </td>
  </tr>
</table>

## 数字编辑
{: #redaction}

数字编辑是 Beta 功能，可用于美国英语、日语和韩语。
{: note}

`redaction` 参数指示服务对最终抄本中的数字数据执行编辑（或掩蔽）。此功能对具有三位或更多连续位数的任何数字，通过将每位数替换为 `X` 字符来进行编辑。此功能旨在编辑敏感数字信息，例如信用卡号。

缺省情况下，服务不会编辑数字数据。将 `redaction` 参数设置为 `true` 可启用数字编辑。启用编辑时，服务会自动启用智能格式设置，不管您是否显式禁用了智能格式设置功能。为了确保最大安全性，启用编辑时，服务还会强制实施以下参数值：

-   服务会禁用关键字识别，不管是否为 `keywords` 和 `keywords_threshold` 参数指定了值。
-   服务会禁用中间结果，不管是否将 WebSocket 接口的 `interim_results` 参数设置为 `true`。
-   服务仅返回单个最终抄本，不管是否为 `maximum_alternatives` 参数指定了值。

此功能的设计与现有智能格式设置功能类似。在将结果返回给客户机之前，且在文本规范化完成之后，服务才会将编辑功能应用于识别请求的最终抄本。

此功能的未来版本可能会扩展编辑能力，以掩蔽所有电话号码、街道地址、银行帐户、社会保障号和其他敏感数字数据。
{: note}

### 语言差异
{: #redactionDifferences}

此功能对于美国英语模型，工作方式与所述内容完全一样，但对日语和韩语模型有以下差异。

#### 日语
{: #redactionJapanese}

日语编辑具有以下差异：

-   除了掩蔽由三个或更多连续数字组成的字符串外，编辑功能还会掩蔽街道地址和编号，而不管它们包含的数字是否少于三位。
-   与此类似，编辑功能还会掩蔽日语样式出生日期中的日期信息。在日语中，日期信息通常以拉丁语*基督纪年*格式呈现，但有时也会采用日语样式，尤其是表示出生日期时。在这种情况下，会掩蔽年份和月份，即使它们只包含一位或两位数字。例如，数字编辑功能对以下字符串的更改如下所示。

    <table style="width:50%">
      <caption>表 5. 日语样式出生日期的示例编辑</caption>
      <tr>
        <th style="text-align:left">不使用编辑</th>
        <th style="text-align:left">使用编辑</th>
      </tr>
      <tr>
        <td>
          &#24179;&#25104; 30&#24180; 2&#26376;
        </td>
        <td>
          &#24179;&#25104; XX&#24180; X&#26376;
        </td>
      </tr>
    </table>

#### 韩语
{: #redactionKorean}

韩语编辑具有以下差异：

-   不支持智能格式设置功能。服务仍会对韩语执行数字编辑，但不会执行其他智能格式设置。
-   编辑孤立的数字字符，但不会编辑韩语短语中包含的可能数字字符。例如，以下短语中的字符<code>&#51060;</code>未替换为 `X`，因为它与以下字符相邻：

    <code>&#51060;&#51077;&#45768;&#45796;</code>

    如果<code>&#51060;</code>字符通过一个空格与后续字符分隔开，那么会将其替换为 `X`，如[数字编辑结果](#redactionResults)中所述。

### 数字编辑示例
{: #redactionExample}

以下示例在识别请求中通过将 `redaction` 参数设置为 `true` 来请求数字编辑。由于请求启用了编辑，因此服务会通过请求隐式启用智能格式设置。服务实际上会禁用请求的其他参数，使这些参数无效：服务会返回单个最终抄本，并且不识别任何关键字。以下部分显示了编辑的影响。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?&redaction=true&max_alternatives=3&keywords=%22birth%22%2C%22birthday%22&keywords_threshold=0.5"
```
{: pre}

### 数字编辑结果
{: #redactionResults}

下表显示了在每种支持的语言中使用和不使用数字编辑的最终抄本的示例。

<table style="width:90%" summary="每个标题行标识一种语言，随后一行显示不启用和启用数字编辑的同一抄本。">
  <caption>表 6. 数字编辑示例抄本</caption>
  <tr>
    <th id="without_redaction" style="text-align:left">不使用编辑</th>
    <th id="with_redaction" style="text-align:left">使用编辑</th>
  </tr>
  <tr>
    <th id="US_English" colspan="2">
      <strong>美国英语</strong>
    </th>
  </tr>
  <tr>
    <td headers="US_English without_redaction">
      my credit card number is four one four seven two
    </td>
    <td headers="US_English with_redaction">
      my credit card number is XXXXX
    </td>
  </tr>
  <tr>
    <th id="Japanese" colspan="2">
      <strong>日语</strong>
    </th>
  </tr>
  <tr>
    <td headers="Japanese without_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; &#22235; &#19968; &#22235; &#19971; &#20108;&#12391;&#12377;
    </td>
    <td headers="Japanese with_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; XXXXX &#12391;&#12377;
    </td>
  </tr>
  <tr>
    <th id="Korean" colspan="2">
      <strong>韩语</strong>
    </th>
  </tr>
  <tr>
    <td headers="Korean without_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; &#49324; &#51068; &#49324; &#52832; &#51060; &#48264;&#51077;&#45768;&#45796;
    </td>
    <td headers="Korean with_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; XXXXX &#48264;&#51077;&#45768;&#45796;
    </td>
  </tr>
</table>

## 不雅言辞过滤
{: #profanity_filter}

不雅言辞过滤功能仅对于美国英语一般可用。
{: note}

`profanity_filter` 参数指示服务是否要从其结果中检剔不雅言辞。缺省情况下，服务会通过将抄本中的所有不雅言辞替换为一系列星号，从而隐藏这些不雅言辞。将此参数设置为 `false` 时，输出中显示的词与转录的内容完全相同。

服务会从所有最终抄本和任何替代抄本中检剔不雅言辞。此外，服务还会从与词替代项、词置信度和词时间戳记关联的结果中检剔不雅言辞。唯一的例外是关键字识别，对于此功能，服务会返回用户指定的所有词，而不管 `profanity_filter` 是否为 `true`。

### 不雅言辞过滤示例
{: #profanityFilteringExample}

以下示例显示了使用 `profanity_filter` 参数的缺省值 `true` 值时，转录的简短音频文件的结果。请求还将 `word_alternatives_threshold` 参数设置为相对高的值 `0.99`，并将 `word_confidence` 和 `timestamps` 参数设置为 `true`。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.99&word_confidence=true&timestamps=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 0.03,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "****"
            }
          ],
          "end_time": 0.25
        },
        {
          "start_time": 0.25,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "you"
            }
          ],
          "end_time": 0.56
        }
      ],
      "alternatives": [
        {
          "transcript": "**** you",
          "confidence": 0.99,
          "word_confidence": [
            ["****", 1.0],
            ["you", 0.99]
          ],
          "timestamps": [
            ["****", 0.03, 0.25],
            ["you", 0.25, 0.56]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}
