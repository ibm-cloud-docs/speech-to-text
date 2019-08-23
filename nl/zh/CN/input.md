---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-24"

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

# 输入功能
{: #input}

{{site.data.keyword.speechtotextfull}} 服务提供了以下功能来指定服务将如何执行语音识别请求。以下各部分中描述的所有输入参数都是可选的。只有输入音频是必需的。
{: shortdesc}

-   有关每个服务接口的简单语音识别请求的示例，请参阅[发出识别请求](/docs/services/speech-to-text?topic=speech-to-text-basic-request)。
-   有关所有可用语音识别参数的列表（按字母顺序排列），包括其状态（一般可用或 Beta）和支持的语言，请参阅[参数摘要](/docs/services/speech-to-text?topic=speech-to-text-summary)。

## 定制模型
{: #custom-input}

语言模型和声学模型定制针对不同的语言提供不同的支持级别（一般可用或 Beta）。有关更多信息，请参阅[支持定制的语言](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)。
{: note}

所有接口都接受定制模型用于识别请求：

-   *定制语言模型*使用特定领域中的术语来扩展服务的基本词汇表。使用 `language_customization_id` 参数可在请求中包含定制语言模型。此外，还可以指定可选的 `customization_weight` 参数。此参数指示给予定制模型中的词相对于基本词汇表中的词的权重。

    有关更多信息，请参阅[使用定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-languageUse)。
-   *定制声学模型*可调整服务的基本声学模型，以适应环境和说话者的声学特征。使用 `acoustic_customization_id` 参数可在请求中包含定制声学模型。可以在请求中同时指定定制语言模型和定制声学模型。

    有关更多信息，请参阅[使用定制声学模型](/docs/services/speech-to-text?topic=speech-to-text-acousticUse)。

定制模型基于[语言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)中描述的其中一种语言模型。为某个基本模型创建的定制模型只能用于该基本模型。如果定制模型基于除 `en-US_BroadbandModel`（缺省值）以外的模型，那么还必须在请求中指定该模型的名称。要使用定制模型，必须使用拥有该定制模型的服务实例的凭证来发出请求。

有关定制的简介，请参阅[定制接口](/docs/services/speech-to-text?topic=speech-to-text-customization)。

### 定制模型示例
{: #customExample}

以下示例请求包含 `language_customization_id` 参数，用于指示使用具有指定标识的定制语言模型。另外还包含 `customization_weight` 参数，用于指示要给予定制模型中词的相对权重为 `0.5`。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.5"
```
{: pre}

以下示例请求同时使用定制语言模型和定制声学模型。前者使用 `language_customization_id` 参数进行标识，后者使用 `acoustic_customization_id` 参数进行标识。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&acoustic_customization_id={customization_id}"
```
{: pre}

有关将定制模型用于每个服务接口的示例，请参阅：

-   [使用定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-languageUse)
-   [使用定制声学模型](/docs/services/speech-to-text?topic=speech-to-text-acousticUse)

## 语法
{: #grammars-input}

语法是 Beta 功能。服务支持语法用于所有支持语言模型定制的语言。
{: note}

可以向定制语言模型添加语法，并将其用于语音识别。语法使用正式的语言规范来定义一组用于转录字符串的生产规则。这些规则指定如何通过语言的字母表来形成有效的字符串。

将语法用于语音识别时，服务仅识别语法识别到的短语。通过将搜索空间限制为搜索有效字符串，服务可以更快、更准确地交付结果。

所有接口都接受以下参数用于识别请求：

-   `language_customization_id` 参数用于标识为其定义了语法的定制语言模型。您必须使用拥有该模型的服务实例的凭证来发出请求。

-   `grammar_name` 参数用于指定要使用的语法。一个请求只能指定一个语法。

有关更多信息，请参阅[将语法用于定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-grammars)。

### 语法示例
{: #grammarsExample}

以下示例请求包含 `language_customization_id` 和 `grammar_name` 参数，用于将服务的响应限制为名为 `list-abnf` 的语法中所定义的字符串。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&grammar_name=list-abnf"
```
{: pre}

有关将语法用于每个服务接口的示例，请参阅[将语法用于语音识别](/docs/services/speech-to-text?topic=speech-to-text-grammarUse)。

## 基本模型版本
{: #version}

为了提高语音识别的质量，服务会不定期更新[语言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)中描述的基本语言模型。各个语言的基本模型相互独立，并且一种语言的宽带和窄带模型也同样相互独立。因此，对基本模型的更新是相互独立执行的，对其他模型没有影响。

一个基本模型存在多个版本时，可选的 `base_model_version` 参数可指定要用于识别请求的模型版本。此参数主要用于已针对新基本模型更新的定制模型，但也可以在没有定制模型的情况下使用。用于请求的基本模型版本取决于是否传递了 `base_model_version` 参数。它还取决于是否在请求中指定了定制模型（语言和/或声学）。

-   *如果未在请求中指定定制模型*

    -   省略 `base_model_version` 参数将使用基本模型的最新版本。
    -   指定 `base_model_version` 参数将使用基本模型的特定版本。

-   *如果在请求中指定了定制模型*

    -   省略 `base_model_version` 参数将使用定制模型升级到的基本模型最新版本。例如，如果定制模型已升级到基本模型的最新版本，那么服务会使用基本模型和定制模型的最新版本。
    -   指定 `base_model_version` 参数将使用基本模型和定制模型的指定版本。

此参数旨在与定制模型配合使用。因此，要了解基本模型的可用版本，只能通过列出基于该基本模型的定制模型的相关信息来实现。

-   有关升级定制模型的更多信息，请参阅[升级定制模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。
-   有关使用不同版本的基本模型和定制模型进行语音识别的更多信息，请参阅[使用升级的定制模型发出识别请求](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeRecognition)。

## 音频传输
{: #transmission}

*使用 WebSocket 接口时*，音频数据始终会通过连接流式传输到服务。您可以通过套接字一次性传递全部数据，也可以针对实时用例在数据可用时传递数据。服务将在结果可用时返回结果。

*使用 HTTP 接口时*，可以通过下列任一方式将音频传输到服务：

-   *一次性传递* - 省略 `Transfer-Encoding` 头，并在一次传递中一次性将所有音频数据传递到服务。
-   *流式传输* - 将 `Transfer-Encoding` 请求头设置为 `chunked` 值，并通过持久连接流式传输数据。在将数据流式传输到服务之前，这些数据无需完整存在。您可以在数据可用时对其进行流式传输。服务仅当收到最后一个块（通过发送空块来指示）时，才会发送结果。

    有关使用 `Transfer-Encoding` 头流式传输分块音频的更多信息，请参阅：
    -   [Chunked transfer encoding](https://wikipedia.org/wiki/Chunked_transfer_encoding){: external}
    -   *IETF RFC 7320 HTTP/1.1: Message Syntax and Routing* 中的 [Transfer Codings](https://tools.ietf.org/html/rfc7230#section-4){: external}

使用 HTTP 接口时，服务始终会转录整个音频流后，才会发送任何结果。结果可能包含多个 `transcript` 元素，指示由停顿分隔的各个短语。连接 `transcript` 元素可组合出完整的文字记录。

服务会对流式会话强制实施超时。如果服务检测到长时间静默，或者在 30 秒时间段内没有收到音频，那么服务会终止流式会话。有关超时以及如何避免超时的更多信息，请参阅[超时](#timeouts)。

### 音频传输示例
{: #transmissionExample}

以下示例请求为 `Transfer-Encoding` 头指定 `chunked`，用于指示使用流式传输方式。连接会保持打开状态，以接受其他音频块。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--header "Transfer-Encoding: chunked"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## 超时
{: #timeouts}

使用 HTTP 或 WebSocket 语音识别方法启动流式会话时，服务会强制实施不活动状态超时和会话超时。如果在流式会话期间达到超时，服务会关闭连接。应用程序必须从可能关闭的连接正常恢复。

通过 HTTP 流式传输音频时，服务每 20 秒会在其响应中发送一个空格字符。服务这样做是为了通过避免 30 秒的 HTTP REST 不活动状态超时来改进易用性。为了使连接在识别进行期间保持活动，服务会持续发送此空格字符，直至完成其转录。空格字符不会影响 JSON 编码的响应数据。

此 HTTP 不活动状态超时与服务的不活动状态超时不同。WebSocket 接口不受此 HTTP 超时的影响。
{: note}

### 不活动状态超时
{: #timeouts-inactivity}

服务正在接收音频，但在 30 秒内仅检测到持续静默或非语音活动（无语音），那么会发生*不活动状态超时*（HTTP 状态码 400）。服务将发送错误消息：`30 秒内未检测到语音`。不活动状态超时非常有用，例如，用户只要从现场麦克风走开，会话就会根据此超时终止。

缺省不活动状态超时为 30 秒。可以使用 `inactivity_timeout` 参数来覆盖此值。指定较大的值可增大不活动状态超时。指定值 `-1` 可将不活动状态超时设置为无穷大。但会根据发送到服务的所有音频（包括静默）向您收费，因此对于仅发送静默的流式会话，增大不活动状态超时可能会发生额外费用。

以下示例请求将不活动状态超时设置为 60 秒。请求发送了初始文件以开始流式会话。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Transfer-Encoding: chunked"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?inactivity_timeout=60"
```
{: pre}

### 会话超时
{: #timeouts-session}

未能发送足够的音频使流式会话保持活动状态时，会发生*会话超时*（HTTP 状态码 408）。出于以下原因，服务可能会将会话视为空闲，而触发会话超时：

-   在任何 30 秒时段内，未能向服务发送至少 15 秒的音频。

    在发送最后一个块以指示流结束之前，对于任何 30 秒时间段，都必须至少发送 15 秒的音频。如果将 `inactivity_timeout` 参数设置为更大的值或设置为 `-1`，那么音频可以为静默。但会根据发送到服务的任何音频的持续时间向您收费，包括静默。
-   流式传输音频的速率要比实时速率慢得多。

    理想情况下，请在获取音频进行转录之前立即发出建立会话的请求。然后，通过以接近实时的速率发送音频来保持会话。

在发送最后一个块以指示流结束之后，即无需担心会话超时。服务会继续处理音频，直到它返回最终转录结果。

转录长音频流时，服务可能需要 30 秒以上的时间来处理音频并生成响应。服务在完成处理收到的所有音频之前，不会开始计算会话超时。服务的处理时间不会导致会话超过 30 秒会话超时。

例如，如果在会话前 10 秒内发送了一小时的音频，服务处理该音频可能需要 300 秒。为了使此会话保持活动状态，需要在会话的前 340 秒内再发送至少 15 秒的一些音频，包括静默。

在此示例中，如果在会话的 100 秒标记处发送了另一个 15 秒音频，那么服务可能需要额外 2 秒时间来处理此音频。在这种情况下，您需要在会话的前 342 秒内再发送 15 秒的音频。

不要依赖于处理时间或是否收到结果来确定流式会话是否空闲。假定服务可以立即处理所有音频，并相应地将数据发送到服务。如果是实时流式传输音频，对于任何 30 秒时段，发送音频的速率不要慢于实时速率的一半（15 秒音频）。此速率通常足以满足网络等待时间和延迟的需求。
{: important}

## 请求日志记录
{: #logging}

缺省情况下，{{site.data.keyword.IBM_notm}} 会记录对 {{site.data.keyword.watson}} 服务的所有请求及其结果。日志记录仅用于针对未来用户改进服务。记录的数据不会共享或公开。

如果您对保护用户个人信息的隐私有担忧，或者不希望 IBM 记录您的请求，那么可以选择不让 IBM 记录数据（选择性停用）。您可以选择在帐户级别或 API 请求级别选择性停用日志记录。有关更多信息，请参阅[控制 {{site.data.keyword.watson}} 服务的请求日志记录](/docs/services/watson?topic=watson-gs-logging-overview)。

## 信息安全
{: #security-input}

信息安全包括用于将客户标识与通过请求传递到服务的数据相关联的功能。通过在请求中传递 `X-Watson-Metadata` 头，可将客户标识与数据相关联。如果需要，随后可以使用 `DELETE /v1/user_data` 方法来删除数据。有关更多信息，请参阅[信息安全](/docs/services/speech-to-text?topic=speech-to-text-information-security)。
