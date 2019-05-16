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

# 参数摘要
{: #summary}

下面是可用于语音识别的所有参数的摘要。有关 {{site.data.keyword.speechtotextshort}} 服务的所有方法的更多信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/speech-to-text){: new_window}。
{: shortdesc}

发出语音识别请求时，请考虑以下基本需求：

-   方法名称区分大小写。
-   HTTP 请求头不区分大小写。
-   HTTP 和 WebSocket 查询参数区分大小写。
-   JSON 字段名称区分大小写。
-   所有 JSON 响应内容都采用 UTF-8 字符集。

此外，请考虑以下特定于服务的需求：

-   您只需要指定输入音频。其他所有参数都是可选的。
-   如果在输入中指定了无效的查询参数或 JSON 字段，那么响应会包含 `warnings` 字段，用于描述无效自变量。不管有任何警告，请求都会成功。

## access_token
{: #summary-access-token}

（可选）用于与 WebSocket 接口建立已认证连接的 IAM 访问令牌（如果使用的是 Identity and Access Management (IAM) 认证）。有关更多信息，请参阅[打开连接](/docs/services/speech-to-text/websockets.html#WSopen)。

<table>
  <caption>表 1. access_token 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 连接请求的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      不支持
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      不支持
    </td>
  </tr>
</table>

## acoustic_customization_id
{: #summary-acoustic-customization-id}

（可选）定制声学模型的定制标识，可调整以适应环境和说话者的声学特征。缺省情况下，不会使用定制模型。有关更多信息，请参阅[定制模型](/docs/services/speech-to-text/input.html#custom-input)。

<table>
  <caption>表 2. acoustic_customization_id 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      Beta 功能，可用于所有语言
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 连接请求的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## base_model_version
{: #summary-base-model-version}

（可选）基本模型的版本。此参数主要用于已针对新基本模型更新的定制模型，但也可以在没有定制模型的情况下使用。缺省值取决于此参数是否与定制模型配合使用。有关更多信息，请参阅[基本模型版本](/docs/services/speech-to-text/input.html#version)。

<table>
  <caption>表 3. base_model_version 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 连接请求的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## Content-Type
{: #summary-content-type}

（可选）音频格式（MIME 类型），用于指定传递给服务的音频数据的格式。服务可以自动检测大多数音频的格式，因此对于大多数格式，此参数是可选的。对于 `audio/alaw`、`audio/basic`、`audio/l16` 和 `audio/mulaw` 格式，此参数是必需的。有关更多信息，请参阅[音频格式](/docs/services/speech-to-text/audio-formats.html)。

<table>
  <caption>表 4. Content-Type 语法</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 消息的 <code>content-type</code> 参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的请求头
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的请求头
    </td>
  </tr>
</table>

## customization_weight
{: #summary-customization-weight}

（可选）介于 0.0 到 1.0 之间的双精度值，用于指示服务要给予定制语言模型中的词相对于基本词汇表中的词的权重。缺省值为 0.3，除非在训练定制语言模型时指定了其他权重。有关更多信息，请参阅[定制模型](/docs/services/speech-to-text/input.html#custom-input)。

<table>
  <caption>表 5. customization_weight 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于美国英语、英国英语、巴西葡萄牙语、法语、德语、日语、韩语和西班牙语一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 消息的参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## grammar_name
{: #summary-grammar-name}

（可选）字符串，用来标识要用于语音识别的语法。服务仅识别由语法定义的字符串。必须同时指定语法的名称和为其定义语法的定制语言模型的定制标识。有关更多信息，请参阅[语法](/docs/services/speech-to-text/input.html#grammars-input)。

<table>
  <caption>表 6. grammar_name 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      Beta 功能，可用于美国英语、英国英语、巴西葡萄牙语、法语、德语、日语、韩语和西班牙语
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 消息的参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## inactivity_timeout
{: #summary-inactivity-timeout}

（可选）整数，用于指定服务不活动状态超时的秒数。不活动状态表示服务在流式音频中未检测到语音。缺省值为 30 秒。使用 `-1` 指示无穷大。有关更多信息，请参阅[不活动状态超时](/docs/services/speech-to-text/input.html#timeouts-inactivity)。

<table>
  <caption>表 7. inactivity_timeout 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 消息的参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## interim_results
{: #summary-interim-results}

（可选）布尔值，用于指示服务返回在最终抄本之前可能会更改的中间假设。缺省情况下 (`false`)，不会返回中间结果。有关更多信息，请参阅[中间结果](/docs/services/speech-to-text/output.html#interim)。

<table>
  <caption>表 8. interim_results 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 消息的参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      不支持
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      不支持
    </td>
  </tr>
</table>

## keywords
{: #summary-keywords}

（可选）关键字字符串的数组，服务会在输入音频中识别这些字符串。缺省情况下，不会执行关键字识别。有关更多信息，请参阅[关键字识别](/docs/services/speech-to-text/output.html#keyword_spotting)。

<table>
  <caption>表 9. keywords 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 消息的参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## keywords_threshold
{: #summary-keywords-threshold}

（可选）介于 0.0 到 1.0 之间的双精度值，用于指示关键字正匹配的最小阈值。缺省情况下，不会执行关键字识别。有关更多信息，请参阅[关键字识别](/docs/services/speech-to-text/output.html#keyword_spotting)。

<table>
  <caption>表 10. keywords_threshold 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 消息的参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## language_customization_id
{: #summary-language-customization-id}

（可选）定制语言模型的定制标识，该模型包含您的领域中的术语。缺省情况下，不会使用定制模型。有关更多信息，请参阅[定制模型](/docs/services/speech-to-text/input.html#custom-input)。

<table>
  <caption>表 11. language_customization_id 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于美国英语、英国英语、巴西葡萄牙语、法语、德语、日语、韩语和西班牙语一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 连接请求的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## max_alternatives
{: #summary-max-alternatives}

（可选）整数，用于指定服务返回的最大替代假设数。缺省情况下，服务会返回单个最终假设。有关更多信息，请参阅[最大替代项数](/docs/services/speech-to-text/output.html#max_alternatives)。

<table>
  <caption>表 12. max_alternatives 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 消息的参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## model
{: #summary-model}

（可选）model 用于指定音频中所讲的语言以及音频采样率：宽带或窄带。缺省情况下，会使用 `en-US_BroadbandModel`。有关更多信息，请参阅[语言和模型](/docs/services/speech-to-text/models.html)。

<table>
  <caption>表 13. model 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 连接请求的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## profanity_filter
{: #summary-profanity-filter}

（可选）布尔值，用于指示服务是否从抄本中检剔不雅言辞。缺省情况下 (`true`)，会过滤掉抄本中的不雅言辞。有关更多信息，请参阅[不雅言辞过滤](/docs/services/speech-to-text/output.html#profanity_filter)。

<table>
  <caption>表 14. profanity_filter 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于美国英语一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 消息的参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## redaction
{: #summary-redaction}

（可选）布尔值，用于指示服务是否对抄本中包含三个或更多个连续位的数字数据进行编辑。如果将 `redaction` 参数设置为 `true`，那么服务会自动强制将 `smart_formatting` 参数设置为 `true`。缺省情况下 (`false`)，不会编辑数字数据。有关更多信息，请参阅[数字编辑](/docs/services/speech-to-text/output.html#redaction)。

<table>
  <caption>表 15. redaction 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      Beta 功能，可用于美国英语、日语和韩语
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 消息的参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## smart_formatting
{: #summary-smart-formatting}

（可选）布尔值，用于指示服务是否将最终抄本中的日期、时间、数字、货币和类似值转换为更传统的表示法。对于美国英语，此功能还会将特定关键字短语转换为标点符号。缺省情况下 (`false`)，不会执行智能格式设置。有关更多信息，请参阅[智能格式设置](/docs/services/speech-to-text/output.html#smart_formatting)。

<table>
  <caption>表 16. smart_formatting 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      Beta 功能，可用于美国英语、日语和西班牙语
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 消息的参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## speaker_labels
{: #summary-speaker-labels}

（可选）布尔值，用于指示服务是否在多参与者交流中标识哪些人说了哪些词。如果将 `speaker_labels` 参数设置为 `true`，那么服务会自动强制将 `timestamps` 参数设置为 `true`。缺省情况下 (`false`)，不会返回说话者标签。有关更多信息，请参阅[说话者标签](/docs/services/speech-to-text/output.html#speaker_labels)。

<table>
  <caption>表 17. speaker_labels 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      Beta 功能，可用于美国英语、日语、西班牙语（宽带和窄带模型）和英国英语（仅限窄带模型）
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 消息的参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## timestamps
{: #summary-timestamps}

（可选）布尔值，用于指示服务是否为抄本中的词生成时间戳记。缺省情况下 (`false`)，不会返回时间戳记。有关更多信息，请参阅[词时间戳记](/docs/services/speech-to-text/output.html#word_timestamps)。

<table>
  <caption>表 18. timestamps 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 消息的参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## Transfer-Encoding
{: #summary-transfer-encoding}

（可选）值为 `chunked` 将使音频流式传输到服务。缺省情况下，音频会在一次传递中一次性全部发送。有关更多信息，请参阅[音频传输](/docs/services/speech-to-text/input.html#transmission)。

<table>
  <caption>表 19. Transfer-Encoding 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      不适用；始终流式传输
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的请求头
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的请求头
    </td>
  </tr>
</table>

## watson-token
{: #summary-watson-token}

（可选）{{site.data.keyword.watson}} 认证令牌，如果使用的是 Cloud Foundry 服务凭证，那么将使用此令牌来与 WebSocket 接口建立已认证连接。有关更多信息，请参阅[打开连接](/docs/services/speech-to-text/websockets.html#WSopen)。

<table>
  <caption>表 20. watson-token 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 连接请求的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      不支持
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      不支持
    </td>
  </tr>
</table>

## word_alternatives_threshold
{: #summary-word-alternatives-threshold}

（可选）介于 0.0 到 1.0 之间的双精度值，用于指定服务报告输入音频中词的发音相似替代项的阈值。缺省情况下，不会返回词替代项。有关更多信息，请参阅[词替代项](/docs/services/speech-to-text/output.html#word_alternatives)。

<table>
  <caption>表 21. word_alternatives_threshold 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 消息的参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## word_confidence
{: #summary-word-confidence}

（可选）布尔值，用于指示服务是否为抄本中的词提供置信度度量。缺省情况下 (`false`)，不会返回词置信度度量。有关更多信息，请参阅[词置信度](/docs/services/speech-to-text/output.html#word_confidence)。

<table>
  <caption>表 22. word_confidence 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 消息的参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查询参数
    </td>
  </tr>
</table>

## X-Watson-Authorization-Token
{: #summary-x-watson-authorization-token}

（可选）认证令牌，用于向服务发出已认证的请求，而无需在每次调用中嵌入服务凭证。缺省情况下，必须随每个请求一起传递服务凭证。Watson 认证令牌基于 Cloud Foundry 服务凭证，这些凭证使用 `username` 和 `password` 进行认证。

`X-Watson-Authorization-Token` 头不接受 IAM 令牌或 API 密钥。
{: note}

<table>
  <caption>表 23. X-Watson-Authorization-Token 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      不支持；请使用 <code>watson-token</code> 查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      每个请求的请求头
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      每个请求的请求头
    </td>
  </tr>
</table>

## X-Watson-Learning-Opt-Out
{: #summary-x-watson-learning-opt-out}

（可选）布尔值，指示是否选择性停用 {{site.data.keyword.IBM_notm}} 为了针对未来用户改进服务而执行的缺省请求日志记录。要阻止 IBM 访问您的数据以进行一般服务改进，请为此参数指定 <code>true</code>。有关更多信息，请参阅[请求日志记录](/docs/services/speech-to-text/input.html#logging)。

<table>
  <caption>表 24. X-Watson-Learning-Opt-Out 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 连接请求的 <code>x-watson-learning-opt-out</code> 查询参数
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      每个请求的请求头
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      每个请求的请求头
    </td>
  </tr>
</table>

## X-Watson-Metadata
{: #summary-x-watson-metadata}

（可选）字符串，用于将客户标识与为识别请求传递的数据相关联。此参数接受自变量 `customer_id={id}`。缺省情况下，没有任何客户标识与数据相关联。有关更多信息，请参阅[信息安全](/docs/services/speech-to-text/information-security.html)。

<table>
  <caption>表 25. X-Watson-Metadata 参数</caption>
  <tr>
    <th>可用性和用途</th>
    <th style="vertical-align:bottom">描述</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      对于所有语言一般可用
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 连接请求的 <code>x-watson-metadata</code> 查询参数（必须对该自变量进行 URL 编码，例如 `customer_id%3dmy_customer_ID`。）
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      POST <code>/v1/recognize</code> 请求的请求头
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **异步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/register_callback</code> 和 <code>POST /v1/recognitions</code> 请求的请求头
    </td>
  </tr>
</table>
