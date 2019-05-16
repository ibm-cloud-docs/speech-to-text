---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-07"

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

# 使用定制语言模型
{: #languageUse}

创建并训练定制语言模型后，可以将其用于语音识别请求。使用 `language_customization_id` 查询参数来指定用于请求的定制语言模型，如以下示例中所示。您还可以指示服务要向定制模型中的词提供的权重。有关更多信息，请参阅[使用定制权重](#weight)。您必须使用拥有该模型的服务实例的服务凭证来发出请求。
{: shortdesc}

可以针对相同或不同的领域创建多个定制语言模型。但是，使用 `language_customization_id` 参数一次只能指定一个定制语言模型。有关将语法用于定制语言模型的示例，请参阅[将语法用于语音识别](/docs/services/speech-to-text/grammar-use.html)。

-   对于 [WebSocket 接口](/docs/services/speech-to-text/websockets.html)，请使用 `/v1/recognize` 方法。指定的定制模型将用于通过连接发送的所有请求。

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=es-ES_BroadbandModel'
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   对于[同步 HTTP 接口](/docs/services/speech-to-text/http.html)，请使用 `POST /v1/recognize` 方法。指定的定制模型将用于该请求。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}"
    ```
    {: pre}
-   对于[异步 HTTP 接口](/docs/services/speech-to-text/async.html)，请使用 `POST /v1/recognitions` 方法。指定的定制模型将用于该请求。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?language_customization_id={customization_id}"
    ```
    {: pre}

如果定制模型基于缺省语言模型 `en-US_BroadbandModel`，那么可以在请求中省略语言模型。否则，必须使用 `model` 参数来指定基本模型，如 WebSocket 示例中所示。定制模型只能用于创建该模型所针对的基本模型。

## 使用定制权重
{: #weight}

定制语言模型是定制模型及其定制的基本模型的组合。用于语音识别时，您可以指示服务向定制语言模型中的词提供相对于基本模型中的词的权重。分配给定制模型的权重称为其*定制权重*。

您可将定制语言模型的相对权重指定为介于 0.0 到 1.0 之间的双精度值。缺省情况下，每个定制语言模型的权重为 0.3。缺省权重在一般情况下可产生最佳性能。它允许同时识别定制模型中的 OOV 词和基本词汇表中的词。

但是，如果要转录的音频频繁使用定制模型中的 OOV 词，那么增加定制权重可提高转录结果的准确性。设置定制权重时，请务必谨慎。虽然较高的权重可以提高定制模型的领域中短语的准确性，但同时会对非领域的短语的性能产生负面影响。（即使将权重设置为 0.0，也存在因实现语言模型定制而导致转录可能包含定制词的小概率情况。）

使用 `customization_weight` 参数可指定定制权重。可以在训练定制语言模型时指定此参数，也可以将其用于语音识别请求。

-   对于训练请求，以下示例在 `POST /v1/customizations/{customization_id}/train` 方法中指定了定制权重 `0.5`：

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train?customization_weight=0.5"
    ```
    {: pre}

    在训练期间设置定制权重会随定制语言模型一起保存权重。您无需在使用该定制模型的每个识别请求中传递权重。

-   对于识别请求，以下示例在 `POST /v1/recognize` 方法中指定了定制权重 `0.7`：

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.7"
    ```
    {: pre}

    在语音识别期间设置定制权重会覆盖在训练期间随模型一起保存的权重。

## 对定制语言模型的使用进行故障诊断
{: #languageTroubleshoot}

如果将定制语言模型应用于语音识别，但发现服务似乎并未使用模型中包含的词，请检查是否有以下可能的问题：

-   确保正确地将定制标识传递给识别请求，如[使用定制语言模型](#languageUse)中所示。
-   确保定制模型的状态为 `available`，这意味着该模型已经过完全训练并可供使用。有关更多信息，请参阅[列出定制语言模型](/docs/services/speech-to-text/language-models.html#listModels-language)。
-   检查为新词生成的读法，以确保这些读法正确。有关更多信息，请参阅[验证词资源](/docs/services/speech-to-text/language-resource.html#validateModel)。
