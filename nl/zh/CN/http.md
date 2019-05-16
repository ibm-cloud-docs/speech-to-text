---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-08"

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

# 同步 HTTP 接口
{: #http}

{{site.data.keyword.speechtotextfull}} 服务的同步 HTTP 接口提供了单个 `POST /v1/recognize` 方法，用于向服务请求语音识别。此方法是获取抄本的最简单方法。它通过两种方式来提交语音识别请求：
{: shortdesc}

-   第一种方式是通过请求主体在单个流中发送所有音频。您可将操作的参数指定为请求头和查询参数。有关更多信息，请参阅[发出基本 HTTP 请求](#HTTP-basic)。
-   第二种方式是将音频作为多部分请求发送。您可将请求的参数指定为请求头、查询参数和 JSON 元数据的组合。有关更多信息，请参阅[发出多部分 HTTP 请求](#HTTP-multi)。

单个请求中提交的音频数据最大为 100 MB，最小为 100 字节。有关音频格式的信息以及有关使用压缩来最大化可以通过请求发送的音频量的信息，请参阅[音频格式](/docs/services/speech-to-text/audio-formats.html)。有关 HTTP 接口的所有方法的信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/speech-to-text){: new_window}。

## 发出基本 HTTP 请求
{: #HTTP-basic}

HTTP `POST /v1/recognize` 方法提供了简单的音频转录方法。您可通过请求主体传递所有音频，并将参数指定为请求头和查询参数。

以下 `curl` 示例发送对名为 `audio-file.flac` 的单个 FLAC 文件的识别请求。请求省略了 `model` 查询参数，这将使用缺省语言模型 `en-US_BroadbandModel`。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

示例返回了音频的以下抄本：

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

`POST /v1/recognize` 方法仅在处理了请求的所有音频后才会返回结果。此方法适用于批量处理，而不适用于实时语音识别。请使用 WebSocket 接口来转录实时音频。

如果数据包含多个音频文件，那么建议的音频提交方法是发送多个请求，每个音频文件对应一个请求。可通过循环方式提交请求，也可以选择以并行方式提交请求以提高性能。您还可以使用多部分语音识别，以通过单个请求来传递多个音频文件。

## 发出多部分 HTTP 请求
{: #HTTP-multi}

`POST /v1/recognize` 方法还支持多部分请求。您可将所有音频数据作为多部分表单数据传递。可将某些参数指定为请求头和查询参数，但需要将 JSON 元数据作为表单数据传递，以便对转录的大多数方面进行控制。

多部分语音识别适用于以下用例：

-   通过单个语音识别请求传递多个音频文件。
-   使用的是禁用了 JavaScript 的浏览器。基于表单数据的多部分请求不需要使用 JavaScript。
-   识别请求的参数大于 8 KB，这是大多数 HTTP 服务器和代理施加的限制。例如，识别大量关键字可能会使请求的大小超过此限制。多部分请求使用表单数据来避免此约束。

以下部分描述了用于多部分请求的参数，并显示了示例请求。

### 多部分请求的参数
{: #multipartParameters}

可以将多部分语音识别的以下参数指定为请求头、查询参数和表单数据。

<table summary="表的每一行描述了多部分识别请求中使用的一个可能的参数。">
  <caption>表 1. 多部分请求的参数</caption>
  <tr>
    <th id="parameter" style="text-align:left; width:20%">参数</th>
    <th id="description" style="text-align:center; width:80%">描述</th>
  </tr>
  <tr>
    <td>
      <code>metadata</code>
      <br/><em>表单数据</em>
      <br/><em>对象</em>
    </td>
    <td>
      <em>必需</em>。为请求提供转录参数的 JSON 对象。该对象必须是表单数据的第一部分。该信息描述表单数据的后续各部分中的音频。请参阅[多部分请求的 JSON 元数据](#multipartJSON)。
    </td>
  </tr>
  <tr>
    <td>
      <code>upload</code>
      <br/><em>表单数据</em>
      <br/><em>文件</em>
    </td>
    <td>
      <em>必需</em>。一个或多个音频文件，作为请求的表单数据的其余部分。所有音频文件必须具有相同的格式。使用 `curl` 命令时，请为请求的每个文件包含单独的 <code>--form</code> 选项。
    </td>
  </tr>
  <tr>
    <td>
      <code>Content-Type</code>
      <br/><em>头</em>
      <br/><em>字符串</em>
    </td>
    <td>
      <em>必需</em>。指定 `multipart/form-data` 以指示如何将数据传递到方法。使用 JSON `part_content_type` 参数指定音频的内容类型。
    </td>
  </tr>
  <tr>
    <td>
      <code>Transfer-Encoding</code>
      <br/><em>头</em>
      <br/><em>字符串</em>
    </td>
    <td>
      <em>可选</em>。指定 `chunked` 以将音频数据流式传输到服务。如果是通过单个请求发送所有音频，请省略此参数。
    </td>
  </tr>
  <tr>
    <td>
      <code>model</code>
      <br/><em>查询</em>
      <br/><em>字符串</em>
    </td>
    <td>
      <em>可选</em>。要用于请求的模型的标识。缺省值为 `en-US_BroadbandModel`。
    </td>
  </tr>
  <tr>
    <td>
      <code>language_customization_id</code>
      <br/><em>查询</em>
      <br/><em>字符串</em>
    </td>
    <td>
      <em>可选</em>。要用于请求的定制语言模型的 GUID。
    </td>
  </tr>
  <tr>
    <td>
      <code>acoustic_customization_id</code>
      <br/><em>查询</em>
      <br/><em>字符串</em>
    </td>
    <td>
      <em>可选</em>。要用于请求的定制声学模型的 GUID。
    </td>
  </tr>
  <tr>
    <td>
      <code>base_model_version</code>
      <br/><em>查询</em>
      <br/><em>字符串</em>
    </td>
    <td>
      <em>可选</em>。要用于请求的指定基本模型的版本。
    </td>
  </tr>
</table>

有关查询参数的更多信息，请参阅[参数摘要](/docs/services/speech-to-text/summary.html)。

### 多部分请求的 JSON 元数据
{: #multipartJSON}

使用多部分请求传递的 JSON 元数据可包含以下字段：

-   `part_content_type`（字符串）
-   `data_parts_count`（整数）
-   `customization_weight`（数字）
-   `inactivity_timeout`（整数）
-   `keywords` (string[])
-   `keywords_threshold`（数字）
-   `max_alternatives`（整数）
-   `word_alternatives_threshold`（数字）
-   `word_confidence`（布尔值）
-   `timestamps`（布尔值）
-   `profanity_filter`（布尔值）
-   `smart_formatting`（布尔值）
-   `speaker_labels`（布尔值）
-   `grammar_name`（字符串）
-   `redaction`（布尔值）

只有以下两个参数是特定于多部分请求的：

-   对于大多数音频格式，`part_content_type` 字段是*可选*的。对于 `audio/alaw`、`audio/basic`、`audio/l16` 和 `audio/mulaw` 格式，此字段是必需的。它指定请求后续部分中的音频的格式。所有音频文件必须为相同的格式。
-   对于所有请求，`data_parts_count` 字段是*可选*的。它指定随请求一起发送的音频文件数。服务会将流结束检测应用于最后一个（并且可能是仅有的一个）数据部分。如果省略此参数，服务将确定请求中的部分数。

元数据的其他所有参数都是可选的。有关所有可用参数的描述，请参阅[参数摘要](/docs/services/speech-to-text/summary.html)。

### 示例多部分请求

以下 `curl` 示例显示了如何使用 `POST /v1/recognize` 方法传递多部分识别请求。该请求传递两个音频文件：**audio-file1.flac** 和 **audio-file2.flac**。`metadata` 参数提供请求的大部分参数；`upload` 参数提供音频文件。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: multipart/form-data"
--form metadata="{\"part_content_type\":\"application/octet-stream\",
  \"data_parts_count\":2,
  \"timestamps\":true,
  \"word_alternatives_threshold\":0.9,
  \"keywords\":[\"colorado\",\"tornado\",\"tornadoes\"],
  \"keywords_threshold\":0.5}"
--form upload="@{path}audio-file1.flac"
--form upload="@{path}audio-file2.flac"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

示例返回了音频文件的以下抄本。服务会按两个文件的发送顺序返回其结果。（示例输出中缩略了第二个文件的结果。）

```javascript
{
   "results": [
      {
         "word_alternatives": [
            {
               "start_time": 0.03,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "the"
                  }
               ],
               "end_time": 0.09
            },
            {
               "start_time": 0.09,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "latest"
                  }
               ],
               "end_time": 0.62
            },
            {
               "start_time": 0.62,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "weather"
                  }
               ],
               "end_time": 0.87
            },
            {
               "start_time": 0.87,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "report"
                  }
               ],
               "end_time": 1.5
            }
         ],
         "keywords_result": {},
         "alternatives": [
            {
               "timestamps": [
                  [
                     "the",
                     0.03,
                     0.09
                  ],
                  [
                     "latest",
                     0.09,
                     0.62
                  ],
                  [
                     "weather",
                     0.62,
                     0.87
                  ],
                  [
                     "report",
                     0.87,
                     1.5
                  ]
               ],
               "confidence": 0.99,
               "transcript": "the latest weather report "
            }
         ],
         "final": true
      },
      {
         "word_alternatives": [
            {
               "start_time": 0.15,
               "alternatives": [
                  {
                     "confidence": 1.0,
                     "word": "a"
                  }
               ],
               "end_time": 0.3
            },
            {
               "start_time": 0.3,
               "alternatives": [
                  {
                     "confidence": 1.0,
                     "word": "line"
                  }
               ],
               "end_time": 0.64
            },
            . . .
            {
               "start_time": 4.58,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "Colorado"
                  }
               ],
               "end_time": 5.16
            },
            {
               "start_time": 5.16,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "on"
                  }
               ],
               "end_time": 5.32
            },
            {
               "start_time": 5.32,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "Sunday"
                  }
               ],
               "end_time": 6.04
            }
         ],
         "keywords_result": {
            "tornadoes": [
               {
                  "normalized_text": "tornadoes",
                  "start_time": 3.03,
                  "confidence": 0.98,
                  "end_time": 3.84
               }
            ],
            "colorado": [
               {
                  "normalized_text": "Colorado",
                  "start_time": 4.58,
                  "confidence": 0.98,
                  "end_time": 5.16
               }
            ]
         },
         "alternatives": [
            {
               "timestamps": [
                  [
                     "a",
                     0.15,
                     0.3
                  ],
                  [
                     "line",
                     0.3,
                     0.64
                  ],
                  . . .
                  [
                     "Colorado",
                     4.58,
                     5.16
                  ],
                  [
                     "on",
                     5.16,
                     5.32
                  ],
                  [
                     "Sunday",
                     5.32,
                     6.04
                  ]
               ],
               "confidence": 0.99,
               "transcript": "a line of severe thunderstorms with several
possible tornadoes is approaching Colorado on Sunday "
            }
         ],
         "final": true
      }
   ],
   "result_index": 0
}
```
{: codeblock}
