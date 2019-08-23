---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-30"

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

# 发行说明
{: #release-notes}

以下各部分记录了 {{site.data.keyword.speechtotextfull}} 服务的每个发行版和更新中包含的新功能和更改。这些信息包括任何已知限制。除非另有说明，否则所有更改都与较早的发行版兼容，并且会自动、透明地可供所有新应用程序和现有应用程序使用。
{: shortdesc}

## 已知限制
{: #limitations}

目前没有已知限制。

## 2019 年 7 月 30 日
{: #July2019}

现在，服务提供六种西班牙语方言的宽带和窄带语言模型：

-   阿根廷西班牙语（`es-AR_BroadbandModel` 和 `es-AR_NarrowbandModel`）
-   卡斯蒂利亚西班牙语（`es-ES_BroadbandModel` 和 `es-ES_NarrowbandModel`）
-   智利西班牙语（`es-CL_BroadbandModel` 和 `es-CL_NarrowbandModel`）
-   哥伦比亚西班牙语（`es-CO_BroadbandModel` 和 `es-CO_NarrowbandModel`）
-   墨西哥西班牙语（`es-MX_BroadbandModel` 和 `es-MX_NarrowbandModel`）
-   秘鲁西班牙语（`es-PE_BroadbandModel` 和 `es-PE_NarrowbandModel`）

卡斯蒂利亚西班牙语模型并不是新模型。这些模型已正式发布用于语音识别和语言模型定制，但对于声学模型定制是 Beta 功能。

其他五种方言是新增的，针对所有用途都是 Beta 功能。由于这些其他方言是 Beta 功能，因此它们可能未准备好用于生产，并且会随时更改。这些方言是初始产品，预计质量会随着时间和使用量而提高。

有关更多信息，请参阅以下各部分：

-   [支持的语言模型](/docs/services/speech-to-text?topic=speech-to-text-models#modelsList)
-   [支持定制的语言](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)

## 2019 年 6 月 24 日
{: #June2019b}

-   更新了以下窄带模型，以改进语音识别：
    -   美国英语窄带模型 (`en-US_NarrowbandModel`)
    -   巴西葡萄牙语窄带模型 (`pt-BR_NarrowbandModel`)

    缺省情况下，服务会自动将更新的模型用于所有语音识别请求。如果您具有基于这些模型的定制语言模型或定制声学模型，那么必须使用以下方法升级现有定制模型才能利用更新：
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    有关更多信息，请参阅[升级定制模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。
-   现在，服务允许同时提交多个请求，以将不同的音频资源添加到定制声学模型。先前，服务仅允许一次提交一个请求来向定制声学模型添加音频。
-   现在，用于列出有关定制语言模型和定制声学模型的信息的 HTTP `GET` 方法的输出包含 `updated` 字段。该字段指示上次修改定制模型的日期和时间，采用协调世界时 (UTC)。
-   在 `strict` 参数设置为 `false` 时，对于定制模型训练请求生成的警告，模式已更改。字段的名称 `warning_id` 和 `description` 分别更改为 `code` 和 `message`。有关更多信息，请参阅 [API 参考](https://{DomainName}/apidocs/speech-to-text){: external}。

## 2019 年 6 月 10 日
{: #June2019a}

[处理度量值](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics)仅可用于 WebSocket 和异步 HTTP 接口。不支持处理度量值用于同步 HTTP 接口。

## 较旧的发行版
{: #older}

-   [2019 年 5 月 17 日](#May2019b)
-   [2019 年 5 月 10 日](#May2019)
-   [2019 年 4 月 19 日](#April2019b)
-   [2019 年 4 月 3 日](#April2019)
-   [2019 年 3 月 21 日](#March2019d)
-   [2019 年 3 月 15 日](#March2019c)
-   [2019 年 3 月 11 日](#March2019b)
-   [2019 年 3 月 4 日](#March2019)
-   [2019 年 1 月 28 日](#January2019)
-   [2018 年 12 月 20 日](#December2018b)
-   [2018 年 12 月 13 日](#December2018a)
-   [2018 年 11 月 12 日](#November2018b)
-   [2018 年 11 月 7 日](#November2018a)
-   [2018 年 10 月 30 日](#October2018b)
-   [2018 年 10 月 9 日](#October2018a)
-   [2018 年 9 月 10 日](#September2018b)
-   [2018 年 9 月 7 日](#September2018a)
-   [2018 年 8 月 8 日](#August2018)
-   [2018 年 7 月 13 日](#July2018)
-   [2018 年 6 月 12 日](#June2018)
-   [2018 年 5 月 15 日](#May2018)
-   [2018 年 3 月 26 日](#March2018b)
-   [2018 年 3 月 1 日](#March2018a)
-   [2018 年 2 月 1 日](#February2018)
-   [2017 年 12 月 14 日](#December2017)
-   [2017 年 10 月 2 日](#October2017)
-   [2017 年 7 月 14 日](#July2017b)
-   [2017 年 7 月 1 日](#July2017a)
-   [2017 年 5 月 22 日](#May2017)
-   [2017 年 4 月 10 日](#April2017)
-   [2017 年 3 月 8 日](#March2017)
-   [2016 年 12 月 1 日](#December2016)
-   [2016 年 9 月 22 日](#September2016)
-   [2016 年 6 月 30 日](#June2016b)
-   [2016 年 6 月 23 日](#June2016a)
-   [2016 年 3 月 10 日](#March2016)
-   [2016 年 1 月 19 日](#January2016)
-   [2015 年 12 月 17 日](#December2015)
-   [2015 年 9 月 21 日](#September2015)
-   [2015 年 7 月 1 日](#July2015)

### 2019 年 5 月 17 日
{: #May2019b}

-   现在，服务对于语音识别请求，提供了两种类型的可选度量值：
    -   [处理度量值](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics)，用于提供有关服务的输入音频分析的详细计时信息。服务以指定的时间间隔返回度量值以及转录事件，例如中间结果和最终结果。使用这些度量值可测量服务转录音频的进度。
    -   [音频度量值](/docs/services/speech-to-text?topic=speech-to-text-metrics#audio_metrics)，用于提供有关输入音频信号特征的详细信息。在语音处理结束时，结果中提供整个输入音频的聚集度量值。使用这些度量值可确定音频的特征和质量。

    您可以使用任何语音识别请求来请求这两种类型的度量值。缺省情况下，服务不会为请求返回度量值。
-   更新了日语宽带模型 (`ja-JP_BroadbandModel`)，以改进语音识别。缺省情况下，服务会自动将更新的模型用于所有语音识别请求。如果您具有基于该模型的定制语言模型或定制声学模型，那么必须使用以下方法升级现有定制模型才能利用这些更新：
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    有关更多信息，请参阅[升级定制模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。

### 2019 年 5 月 10 日
{: #May2019}

更新了西班牙语语言模型，以改进语音识别：

-   `es-ES_BroadbandModel`
-   `es-ES_NarrowbandModel`

缺省情况下，服务会自动将更新的模型用于所有语音识别请求。如果您具有基于这些模型的定制语言模型或定制声学模型，那么必须使用以下方法升级现有定制模型才能利用更新：

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

有关更多信息，请参阅[升级定制模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。

### 2019 年 4 月 19 日
{: #April2019b}

-   现在，定制接口的训练方法包含 `strict` 查询参数，用于指示在定制模型包含有效和无效资源的组合时，训练是否继续进行。缺省情况下，如果定制模型包含一个或多个无效资源，训练会失败。将此参数设置为 `false`，以允许只要模型包含至少一个有效资源，就继续进行训练。服务会从训练中排除无效的资源。
    -   有关将 `strict` 参数用于 `POST /v1/customizations/{customization_id}/train` 方法的更多信息，请参阅[训练定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language)和[训练失败](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#failedTraining-language)。
    -   有关将 `strict` 参数用于 `POST /v1/acoustic_customizations/{customization_id}/train` 方法的更多信息，请参阅[训练定制声学模型](/docs/services/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic)和[训练失败](/docs/services/speech-to-text?topic=speech-to-text-acoustic#failedTraining-acoustic)。
-   现在，最多可以向定制语言模型的词资源添加 9 万个未登录词 (OOV)。先前最多可以添加 3 万个 OOV 词。此数字包括来自所有源（语料库、语法和直接添加的单个定制词）的 OOV 词。最多可以从所有源向定制模型添加共 1000 万个词。有关更多信息，请参阅[我需要多少数据？](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordsResourceAmount)。

### 2019 年 4 月 3 日
{: #April2019}

现在，定制声学模型最多可接受 200 小时的音频。先前的最大限制为 100 小时音频。

### 2019 年 3 月 21 日
{: #March2019d}

现在，用户只能查看与分配给其 {{site.data.keyword.cloud_notm}} 帐户的角色相关联的服务凭证信息。例如，如果您分配有 `reader` 角色，那么无法查看任何 `writer` 或更高级别的服务凭证。

此更改不会影响具有现有服务凭证的用户或应用程序的 API 访问权。此更改仅影响在 {{site.data.keyword.cloud_notm}} 中查看凭证。

有关服务密钥和用户角色的更多信息，请参阅 [IAM 服务 API 密钥](/docs/services/watson?topic=watson-api-key-bp#api-key-bp)。

### 2019 年 3 月 15 日
{: #March2019c}

现在，服务支持 A-law (`audio/alaw`) 格式的音频。有关更多信息，请参阅 [audio/alaw 格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#alaw)。

### 2019 年 3 月 11 日
{: #March2019b}

-   对于 `max_alternatives` 参数，服务再次接受值 `0`。如果指定 `0`，服务会自动使用缺省值 `1`。3 月 4 日服务更新所做的更改导致值 `0` 返回错误。（如果指定负值，服务会返回错误。）
-   对于 `word_alternatives_threshold` 参数，服务再次接受值 `0`。3 月 4 日服务更新所做的更改导致值 `0` 返回错误。（如果指定负值，服务会返回错误。）
-   现在，服务会返回所有置信度分数，并且最多精确到两位小数。此更改包括文字记录、词置信度、词替代项、关键字结果和说话者标签的置信度分数。

### 2019 年 3 月 4 日
{: #March2019}

更新了以下窄带语言模型，以改进语音识别：

-   `es-ES_NarrowbandModel`
-   `fr-FR_NarrowbandModel`
-   `pt-BR_NarrowbandModel`

缺省情况下，服务会自动将更新的模型用于所有语音识别请求。如果您具有基于这些模型的定制语言模型或定制声学模型，那么必须使用以下方法升级现有定制模型才能利用更新：

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

有关更多信息，请参阅[升级定制模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。

### 2019 年 1 月 28 日
{: #January2019}

现在，WebSocket 接口支持通过基于浏览器的 JavaScript 代码进行基于令牌的 Identity and Access Management (IAM) 认证。与此相反的限制已除去。要使用 WebSocket `/v1/recognize` 方法建立已认证的连接，请执行以下操作：

-   如果使用的是 IAM 认证，请包含 `access_token` 查询参数。
-   如果使用的是 Cloud Foundry 服务凭证，请包含 `watson-token` 查询参数。

有关更多信息，请参阅[打开连接](/docs/services/speech-to-text?topic=speech-to-text-websockets#WSopen)。

### 2018 年 12 月 20 日
{: #December2018b}

从 2019 年 1 月 8 日开始，语法接口在所有位置都可完全正常运行。
{: note}

-   现在，服务支持语法用于语音识别。语法可作为 Beta 功能用于支持语言模型定制的所有语言。可以向定制语言模型添加语法，并使用这些语法来限制服务可以从音频中识别的短语集。可以定义扩充巴科斯范式 (ABNF) 或 XML 格式的语法。

    以下四种方法可用于使用语法：
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 用于将语法文件添加到定制语言模型。
    -   `GET /v1/customizations/{customization_id}/grammars` 用于列出有关定制模型的所有语法的信息。
    -   `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` 用于返回有关定制模型的指定语法的信息。
    -   `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` 用于从定制模型中除去现有语法。

    可以将语法用于通过 WebSocket 和 HTTP 接口执行的语音识别。使用 `language_customization_id` 和 `grammar_name` 参数来标识要使用的定制模型和语法。目前，对于一个语音识别请求，只能使用单个语法。

    有关语法的更多信息，请参阅以下文档：
    -   [将语法用于定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-grammars)
    -   [了解语法](/docs/services/speech-to-text?topic=speech-to-text-grammarUnderstand)
    -   [向定制语言模型添加语法](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd)
    -   [将语法用于语音识别](/docs/services/speech-to-text?topic=speech-to-text-grammarUse)
    -   [管理语法](/docs/services/speech-to-text?topic=speech-to-text-manageGrammars)
    -   [示例语法](/docs/services/speech-to-text?topic=speech-to-text-grammarExamples)

    有关接口的所有方法的信息，请参阅 [API 参考](https://{DomainName}/apidocs/speech-to-text){: external}。
-   现在，新的数字编辑功能可用于掩蔽具有三位或更多连续位数的数字。编辑功能旨在从文字记录中除去敏感个人信息，例如信用卡号。通过在识别请求中将 `redaction` 参数设置为 `true` 可启用此功能。这是 Beta 功能，仅可用于美国英语、日语和韩语。有关更多信息，请参阅[数字编辑](/docs/services/speech-to-text?topic=speech-to-text-output#redaction)。
-   现在，以下新的德语和法语语言模型可用于服务：
    -   `de-DE_NarrowbandModel`
    -   `fr-FR_NarrowbandModel`

    这两种新模型都支持语言模型定制 (GA) 和声学模型定制 (Beta)。有关更多信息，请参阅[支持定制的语言](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)。
-   现在，提供了新的美国英语语言模型 `en-US_ShortForm_NarrowbandModel`。此新模型旨在用于交互式声音响应和自动客户支持解决方案。此模型支持语言模型定制 (GA) 和声学模型定制 (Beta)。有关更多信息，请参阅[美国英语短格式模型](/docs/services/speech-to-text?topic=speech-to-text-models#modelsShortform)。
-   更新了以下语言模型，以改进语音识别：
    -   `en-GB_NarrowbandModel`
    -   `es-ES_NarrowbandModel`

    缺省情况下，服务会自动将更新的模型用于所有语音识别请求。如果您具有基于这些模型的定制语言模型或定制声学模型，那么必须使用以下方法升级现有定制模型才能利用更新：
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    有关更多信息，请参阅[升级定制模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。
-   现在，服务支持 G.729 (`audio/g729`) 格式的音频。服务仅支持 G.729 附录 D 用于窄带音频。有关受支持音频格式的更多信息，请参阅[音频格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   现在，说话者标签功能可用于英国英语的窄带模型 (`en-GB_NarrowbandModel`)。这是用于所有受支持语言的 Beta 功能。有关更多信息，请参阅[说话者标签](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels)。
-   可以添加到定制声学模型的最大音频量从 50 小时增加到 100 小时。

### 2018 年 12 月 13 日
{: #December2018a}

现在，服务在 {{site.data.keyword.cloud_notm}} 伦敦位置 (**eu-gb**) 可用。与所有位置一样，伦敦也使用基于令牌的 IAM 认证。在此位置创建的所有新服务实例都使用 IAM 认证。

### 2018 年 11 月 12 日
{: #November2018b}

现在，服务支持智能格式设置用于日语语音识别。先前，服务仅支持智能格式设置用于美国英语和西班牙语。这是用于所有受支持语言的 Beta 功能。有关更多信息，请参阅[智能格式设置](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting)。

### 2018 年 11 月 7 日
{: #November2018a}

现在，服务在 {{site.data.keyword.cloud_notm}} 东京位置 (**jp-tok**) 可用。与所有位置一样，东京也使用基于令牌的 IAM 认证。在此位置创建的所有新服务实例都使用 IAM 认证。

### 2018 年 10 月 30 日
{: #October2018b}

所有位置的此服务都已迁移到基于令牌的 IAM 认证。所有 {{site.data.keyword.cloud_notm}} 服务现在都使用 IAM 认证。在各个位置，{{site.data.keyword.speechtotextshort}} 服务的迁移日期如下：

-   达拉斯 (**us-south**)：2018 年 10 月 30 日
-   法兰克福 (**eu-de**)：2018 年 10 月 30 日
-   华盛顿 (**us-east**)：2018 年 6 月 12 日
-   悉尼 (**au-syd**)：2018 年 5 月 15 日

迁移到 IAM 认证对新的和现有服务实例的影响有所不同：

-   *在任何位置创建的所有新服务实例*现在都使用 IAM 认证来访问服务。可以传递不记名令牌或 API 密钥：令牌支持已认证的请求，而无需在每个调用中嵌入服务凭证；API 密钥使用 HTTP 基本认证。使用任何 {{site.data.keyword.watson}} SDK 时，都可以传递 API 密钥，并让 SDK 来管理令牌的生命周期。
-   *在所指示迁移日期之前在某个位置中创建的现有服务实例*将继续使用其先前 Cloud Foundry 服务凭证中的 `{username}` 和 `{password}` 进行认证，直到将其迁移为使用 IAM 认证为止。有关迁移到 IAM 认证的更多信息，请参阅[将 Cloud Foundry 服务实例迁移到资源组](https://{DomainName}/docs/resources?topic=resources-migrate)。

有关更多信息，请参阅以下文档：

-   要了解服务实例使用的认证机制，请通过在 [{{site.data.keyword.cloud_notm}} 仪表板](https://{DomainName}/dashboard/apps){: external}上单击该实例来查看服务凭证。
-   有关将 IAM 令牌用于 Watson 服务的更多信息，请参阅[使用 IAM 令牌进行认证](/docs/services/watson?topic=watson-iam)。
-   有关将 IAM API 密钥用于 Watson 服务的更多信息，请参阅 [IAM 服务 API 密钥](/docs/services/watson?topic=watson-api-key-bp)。
-   有关使用 IAM 认证的示例，请参阅 [API 参考](https://{DomainName}/apidocs/speech-to-text){: external}。

### 2018 年 10 月 9 日
{: #October2018a}

-   现在，`Content-Type` 头对于语音识别请求是可选的。服务现在会自动检测大多数音频的音频格式（MIME 类型）。但对于以下格式，仍然必须指定内容类型：
    -   `audio/basic`
    -   `audio/l16`
    -   `audio/mulaw`

    如文中所述，为这些格式指定的内容类型必须包含采样率，并且可以选择包含音频的声道数和字节序。对于其他所有音频格式，都可以省略内容类型或指定内容类型 `application/octet-stream` 以让服务自动检测格式。

    使用 `curl` 命令通过 HTTP 接口发出语音识别请求时，必须使用 `Content-Type` 头指定音频格式，指定 `"Content-Type: application/octet-stream"` 或指定 `"Content-Type:"`。如果完全省略该头，`curl` 将使用缺省值 `application/x-www-form-urlencoded`。此文档中的大部分示例会继续指定语音识别请求的格式，而不管是否必需。
    {: important}

    此更改适用于以下方法：
    -   用于 WebSocket 请求的 `/v1/recognize`。现在，为通过已打开 WebSocket 连接发起请求而发送的文本消息的 `content-type` 字段是可选的。
    -   用于同步 HTTP 请求的 `POST /v1/recognize`。现在，`Content-Type` 头是可选的。（对于多部分请求，JSON 元数据的 `part_content_type` 字段现在也是可选的。）
    -   用于异步 HTTP 请求的 `POST /v1/recognitions`。现在，`Content-Type` 头是可选的。

    有关更多信息，请参阅[音频格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   更新了巴西葡萄牙语宽带模型 `pt-BR_BroadbandModel`，以改进语音识别。缺省情况下，服务会自动将更新的模型用于所有识别请求。如果您具有基于此模型的定制语言模型或定制声学模型，那么必须使用以下方法升级现有定制模型才能利用更新：
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    有关更多信息，请参阅[升级定制模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。
-   不推荐使用语音识别方法的 `customization_id` 参数，在未来发行版中会除去此参数。要为语音识别请求指定定制语言模型，请改为使用 `language_customization_id` 参数。此更改适用于以下方法：
    -   用于 WebSocket 请求的 `/v1/recognize`
    -   用于同步 HTTP 请求（多括多部分请求）的 `POST /v1/recognize`
    -   用于异步 HTTP 请求的 `POST /v1/recognitions`
-   从 2018 年 10 月 1 日开始，向服务传递以用于语音识别的所有音频都会收费。每月发送的前 1000 分钟的音频不再免费。有关服务价格套餐的更多信息，请参阅 [{{site.data.keyword.cloud_notm}} 目录](https://{DomainName}/catalog/services/speech-to-text){: external}中的 {{site.data.keyword.speechtotextshort}} 服务。

### 2018 年 9 月 10 日
{: #September2018b}

有关自初始发行版以来已修复问题的列表，请参阅[已解决的问题](#known_issues)。
{: important}

-   现在，服务支持德语宽带模型 `de-DE_BroadbandModel`。新的德语模型支持语言模型定制（一般可用）和声学模型定制 (Beta)。
    -   有关服务如何解析德语语料库的信息，请参阅[解析英语、法语、德语、西班牙语和巴西葡萄牙语](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages)。
    -   有关创建德语定制词的发音相似的读法的更多信息，请参阅[针对法语、德语、西班牙语和巴西葡萄牙语的准则](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR)。
-   现在，现有巴西葡萄牙语模型 `pt-BR_BroadbandModel` 和 `pt-BR_NarrowbandModel` 支持语言模型定制（一般可用）。这两个模型未更新为启用此支持，因此无需升级现有定制声学模型。
    -   有关服务如何解析巴西葡萄牙语语料库的信息，请参阅[解析英语、法语、德语、西班牙语和巴西葡萄牙语](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages)。
    -   有关创建巴西葡萄牙语定制词的发音相似的读法的更多信息，请参阅[针对法语、德语、西班牙语和巴西葡萄牙语的准则](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR)。
-   提供了美国英语和日语宽带和窄带模型的新版本：
    -   `en-US_BroadbandModel`
    -   `en-US_NarrowbandModel`
    -   `ja-JP_BroadbandModel`
    -   `ja-JP_NarrowbandModel`

    新模型提供了改进的语音识别功能。缺省情况下，服务会自动将更新的模型用于所有识别请求。如果您具有基于这些模型的定制语言模型或定制声学模型，那么必须使用以下方法升级现有定制模型才能利用更新：
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    有关更多信息，请参阅[升级定制模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。
-   现在，关键字识别和词替代项功能对于所有语言已一般可用 (GA)，而不再是 Beta 功能。有关更多信息，请参阅：
    -   [关键字识别](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting)
    -   [词替代项](/docs/services/speech-to-text?topic=speech-to-text-output#word_alternatives)

### 已解决的问题
{: #known_issues}

解决了与定制接口关联的以下已知问题。此列表会保留，以供过去可能遇到过这些问题的用户使用。

-   如果向定制语言模型或定制声学模型添加了数据，那么必须重新训练模型后，才能将其用于语音识别。此问题会出现在以下场景中：

    1.  用户创建了新的定制模型（语言或声学）并训练了该模型。
    1.  用户向定制模型添加了其他资源（词、语料库或音频），但未重新训练该模型。
    1.  用户无法将定制模型用于语音识别。将定制模型用于语音识别请求时，服务会返回以下格式的错误：

        ```javascript
        {
          "code_description": "错误请求",
          "code": 400,
          "error": "请求的定制语言模型不可用。
                    请确保已对定制模型进行训练。"
        }
        ```
        {: codeblock}

    要解决此问题，用户必须基于定制模型的最新数据重新训练该模型。然后，用户可以将定制模型用于语音识别。
-   在训练现有定制语言模型或定制声学模型之前，必须先将其升级到其基本模型的最新版本。此问题会出现在以下场景中：

    1.  用户具有基于已更新模型的现有定制模型（语言或声学）。
    1.  用户未先升级到基本模型的最新版本，就基于基本模型的旧版本来训练现有定制模型。
    1.  用户无法将定制模型用于语音识别。

    要解决此问题，用户必须使用 `POST /v1/customizations/{customization_id}/upgrade_model` 或 `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` 方法将定制模型升级到其基本模型的最新版本。然后，用户可以将定制模型用于语音识别。

这两个问题都已在生产环境中修复。

### 2018 年 9 月 7 日
{: #September2018a}

不再支持基于会话的 HTTP REST 接口。文档中除去了与会话相关的所有信息。以下方法不再可用：
{: important}

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

如果应用程序使用的是会话接口，那么必须迁移到其余的某个 HTTP REST 接口或迁移到 WebSocket 接口。有关更多信息，请参阅 [2018 年 8 月 8 日](#August2018)的服务更新。

### 2018 年 8 月 8 日
{: #August2018}

从 **2018 年 8 月 8 日**开始，将废弃基于会话的 HTTP REST 接口。从 **2018 年 9 月 7 日**开始，将从服务中除去会话 API 的所有方法，在此之后，您将无法再使用基于会话的接口。此直接废弃通知和 30 天除去通知适用于以下方法：

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

如果应用程序使用的是会话接口，那么必须在 9 月 7 日之前迁移到下列其中一个接口：
{: important}

-   对于基于流的语音识别（包括实时用例），请使用 [WebSocket 接口](/docs/services/speech-to-text?topic=speech-to-text-websockets)，此接口提供了对中间结果的访问和最短延迟。
-   对于基于文件的语音识别，请使用下列其中一个接口：

    -   对于包含最长几分钟的音频的较短文件，请使用[同步 HTTP 接口](/docs/services/speech-to-text?topic=speech-to-text-http) (`POST /v1/recognize`) 或[异步 HTTP 接口](/docs/services/speech-to-text?topic=speech-to-text-async) (`POST /v1/recognitions`)。
    -   对于包含超过几分钟的音频的更长文件，请使用异步 HTTP 接口。异步 HTTP 接口接受单个请求中最多 1 GB 音频数据。

WebSocket 和 HTTP 接口提供的结果与会话接口的相同（唯一的区别是 WebSocket 接口会提供中间结果）。您还可以使用其中一个 Watson SDK，以通过任何接口简化应用程序开发工作。有关更多信息，请参阅 [API 参考](https://{DomainName}/apidocs/speech-to-text){: external}。

### 2018 年 7 月 13 日
{: #July2018}

更新了西班牙语窄带模型 `es-ES_NarrowbandModel`，以改进语音识别。缺省情况下，服务会自动将更新的模型用于所有识别请求。如果您具有基于此模型的定制语言模型或定制声学模型，那么必须使用以下方法升级定制模型才能利用更新：

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

有关更多信息，请参阅[升级定制模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。

从此更新开始，以下两个版本的西班牙语窄带模型可用：

-   `es_ES.8kHz.general.lm20180522235959.am20180522235959`（当前）
-   `es_ES.8kHz.general.lm20180308235959.am20180308235959`（先前）

模型的以下版本不再可用：

-   `es_ES.8kHz.general.lm20171031235959.am20171031235959`

如果识别请求尝试使用基于现在不可用的基本模型的定制模型，该请求将使用未进行任何定制的最新基本模型。服务会返回以下警告消息：`将使用非定制的缺省基本模型，因为定制 {type} 模型是使用不再支持的基本模型版本构建的。`要恢复使用基于不可用的模型的定制模型，必须先使用上述的相应 `upgrade_model` 方法来升级模型。

### 2018 年 6 月 12 日
{: #June2018}

针对在华盛顿 (**us-east**) 托管的应用程序启用了以下功能：

-   现在，服务支持新的 API 认证过程。有关更多信息，请参阅 [2018 年 10 月 30 日服务更新](#October2018b)。
-   现在，服务支持 `X-Watson-Metadata` 头和 `DELETE /v1/user_data` 方法。有关更多信息，请参阅[信息安全](/docs/services/speech-to-text?topic=speech-to-text-information-security)。

### 2018 年 5 月 15 日
{: #May2018}

针对在悉尼 (**au-syd**) 托管的应用程序启用了以下功能：

-   现在，服务支持新的 API 认证过程。有关更多信息，请参阅 [2018 年 10 月 30 日服务更新](#October2018b)。
-   现在，服务支持 `X-Watson-Metadata` 头和 `DELETE /v1/user_data` 方法。有关更多信息，请参阅[信息安全](/docs/services/speech-to-text?topic=speech-to-text-information-security)。

### 2018 年 3 月 26 日
{: #March2018b}

-   现在，服务支持语言模型定制用于法语语言模型 `fr-FR_BroadbandModel`。法语模型已一般可用，可供在生产环境中与语言模型定制配合使用。
    -   有关服务如何解析法语语料库的更多信息，请参阅[解析英语、法语、德语、西班牙语和巴西葡萄牙语](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages)。
    -   有关创建法语定制词的发音相似的读法的更多信息，请参阅[针对法语、德语、西班牙语和巴西葡萄牙语的准则](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR)。
-   更新了西班牙语和韩语窄带模型（`es-ES_NarrowbandModel` 和 `ko-KR_NarrowbandModel`）以及法语宽带模型 `fr-FR_BroadbandModel`，以改进语音识别。缺省情况下，服务会自动将更新的模型用于所有识别请求。如果您具有基于其中任一模型的定制语言模型或定制声学模型，那么必须使用以下方法升级定制模型才能利用更新：

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    有关更多信息，请参阅[升级定制模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。
-   以下方法的 `version` 参数已重命名为 `base_model_version`：

    -   用于 WebSocket 请求的 `/v1/recognize`
    -   用于无会话 HTTP 请求的 `POST /v1/recognize`
    -   用于基于会话的 HTTP 请求的 `POST /v1/sessions`
    -   用于异步 HTTP 请求的 `POST /v1/recognitions`

    `base_model_version` 参数指定要用于语音识别的基本模型的版本。有关更多信息，请参阅[使用升级的定制模型发出识别请求](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeRecognition)和[基本模型版本](/docs/services/speech-to-text?topic=speech-to-text-input#version)。
-   现在，西班牙语和美国英语都支持智能格式设置。对于美国英语，现在该功能还可将关键字字符串转换为句点、逗号、问号和惊叹号等标点符号。有关更多信息，请参阅[智能格式设置](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting)。

### 2018 年 3 月 1 日
{: #March2018a}

更新了西班牙语和法语宽带模型 `es-ES_BroadbandModel` 和 `fr-FR_BroadbandModel`，以改进语音识别。缺省情况下，服务会自动将更新的模型用于所有识别请求。如果您具有基于其中任一模型的定制语言模型或定制声学模型，那么必须使用以下方法升级定制模型才能利用更新：

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

有关更多信息，请参阅[升级定制模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。该部分提供了有关升级定制模型的规则、升级效果和使用已升级模型的方法。

### 2018 年 2 月 1 日
{: #February2018}

现在，服务提供了韩语语言模型用于语音识别：`ko-KR_BroadbandModel`（对于采样率至少为 16 千赫兹的音频）和 `ko-KR_NarrowbandModel`（对于采样率至少为 8 千赫兹的音频）。有关更多信息，请参阅[语言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)。

对于语言模型定制，韩语模型已一般可用，可供生产环境使用；对于声学模型定制，这些模型还是 Beta 功能。有关更多信息，请参阅[支持定制的语言](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)。

-   有关服务如何解析韩语语料库的更多信息，请参阅[解析韩语](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages-koKR)。
-   有关创建韩语定制词的发音相似的读法的更多信息，请参阅[针对韩语的准则](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-koKR)。

### 2017 年 12 月 14 日
{: #December2017}

-   更新了美国英语模型 `en-US_BroadbandModel` 和 `en-US_NarrowbandModel`，以改进语音识别。缺省情况下，服务会自动将更新的模型用于所有识别请求。如果您具有基于美国英语模型的定制语言模型或定制声学模型，那么必须使用以下方法升级定制模型才能利用更新：

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

有关该过程的更多信息，请参阅[升级定制模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。该部分提供了有关升级定制模型的规则、升级效果和使用已升级模型的方法。目前，这些方法仅适用于新的美国英语基本模型。但在其他基本模型的升级可用时，这些信息也适用于这些模型的升级。
-   现在，用于发出识别请求的各种方法包含新的 `base_model_version` 参数，可用于发起请求，以要求使用基本模型和定制模型的较低版本或已升级版本。虽然 `base_model_version` 参数主要用于已升级的定制模型，但也可以在没有定制模型的情况下使用。有关更多信息，请参阅[基本模型版本](/docs/services/speech-to-text?topic=speech-to-text-input#version)。
-   现在，服务支持将声学模型定制作为所有可用语言的 Beta 功能。可以创建定制声学模型，以用于所有语言的宽带或窄带模型。有关定制（包括声学模型定制）的简介，请参阅[定制接口](/docs/services/speech-to-text?topic=speech-to-text-customization)。
-   现在，服务支持语言模型定制用于英国英语模型 `en-GB_BroadbandModel` 和 `en-GB_NarrowbandModel`。虽然服务以大致类似的方式来处理英国英语和美国英语语料库及定制词，但仍存在一些重要的差异：
    -   有关服务如何解析英国英语语料库的更多信息，请参阅[解析英语、法语、德语、西班牙语和巴西葡萄牙语](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages)。
    -   有关创建英国英语定制词的发音相似的读法的更多信息，请参阅[针对英语（美国和英国）的准则](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-enUS-enGB)。具体而言，对于英国英语，不能在发音相似的读法中使用句点或连字符。
-   现在，语言模型定制和所有关联的参数对于所有支持的语言已一般可用 (GA)：日语、西班牙语、英国英语和美国英语。

### 2017 年 10 月 2 日
{: #October2017}

-   现在，定制接口提供了声学模型定制。您可以创建定制声学模型，用于调整服务的基本模型，以适应您的环境和说话者。您可以基于与要转录的音频的声学特征匹配度更高的音频来填充和训练定制声学模型。然后，可将定制声学模型用于识别请求，以提高语音识别的准确性。

    定制声学模型用于对定制语言模型进行补充。可以使用定制语言模型来训练定制声学模型，并且可以在语音识别期间使用这两种类型的模型。声学模型定制是 Beta 接口，仅可用于美国英语、西班牙语和日语。

    -   有关定制接口支持的语言以及每种语言可用支持级别的更多信息，请参阅[支持定制的语言](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)。
    -   有关服务定制接口的更多信息，请参阅[定制接口](/docs/services/speech-to-text?topic=speech-to-text-customization)。
    -   有关创建定制声学模型的更多信息，请参阅[创建定制声学模型](/docs/services/speech-to-text?topic=speech-to-text-acoustic)。
    -   有关使用定制声学模型的更多信息，请参阅[使用定制声学模型](/docs/services/speech-to-text?topic=speech-to-text-acousticUse)。
    -   有关定制接口的所有方法的更多信息，请参阅 [API 参考](https://{DomainName}/apidocs/speech-to-text){: external}。
-   对于语言模型定制，服务现在包含一个 Beta 功能，用于为定制语言模型设置可选的定制权重。定制权重指定要提供给定制语言模型中的词的相对权重，该权重是相对于服务的基本词汇表中的词。可以在训练和语音识别期间设置定制权重。有关更多信息，请参阅[使用定制权重](/docs/services/speech-to-text?topic=speech-to-text-languageUse#weight)。
-   升级了 `ja-JP_BroadbandModel` 语言模型，以捕获基本模型中的改进。升级不会影响基于此模型的现有定制模型。
-   现在，服务包含一个参数，用于指定以 `audio/l16`（线性 16 位脉冲编码调制 (PCM)）格式提交的音频的字节序。除了在格式中指定 `rate` 和 `channels` 参数外，现在还可以使用 `endianness` 参数指定 `big-endian` 或 `little-endian`。有关更多信息，请参阅[音频格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。

### 2017 年 7 月 14 日
{: #July2017b}

-   现在，服务支持转录 MP3 或运动图像专家组 (MPEG) 格式的音频。有关受支持音频格式的更多信息，请参阅[音频格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   现在，语言模型定制接口将西班牙语作为 Beta 功能支持。可以基于以下任一基本西班牙语语言模型来创建定制模型：`es-ES_BroadbandModel` 或 `es-ES_NarrowbandModel`；有关更多信息，请参阅[创建定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-languageCreate)。 使用西班牙语定制语言模型的识别请求的定价与使用美国英语和日语模型的请求相同。
-   现在，传递到 `POST /v1/customizations` 方法以创建新定制语言模型的 JSON `CreateLanguageModel` 对象包含 `dialect` 字段。此字段指定该语言中要用于定制模型的方言。缺省情况下，方言与基本模型的语言相匹配。此参数仅对西班牙语模型有意义，服务可以为其创建适合下列其中一种方言的语音的定制模型：
    -   `es-ES`，用于卡斯蒂利亚西班牙语（缺省）
    -   `es-LA`，用于拉丁美洲西班牙语
    -   `es-US`，用于北美（墨西哥）西班牙语

    定制接口的 `GET /v1/customizations` 和 `GET /v1/customizations/{customization_id}` 方法会在其输出中包含定制模型的方言。有关更多信息，请参阅[创建定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language)和[列出定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language)。
-   已不推荐使用语言模型 `en-UK_BroadbandModel` 和 `en-UK_NarrowbandModel` 的名称。现在，为模型提供的名称为 `en-GB_BroadbandModel` 和 `en-GB_NarrowbandModel`。

    不推荐使用的 `en-UK_{model}` 名称会继续有效，但 `GET /v1/models` 方法在可用模型列表中不再返回这些名称。您仍可以直接使用 `GET /v1/models/{model_id}` 方法来查询这些名称。

### 2017 年 7 月 1 日
{: #July2017a}

-   现在，服务的语言模型定制接口对于其支持的两种语言（美国英语和日语）已一般可用 (GA)。{{site.data.keyword.IBM_notm}} 支持免费创建、托管或管理定制语言模型。如下一个项目符号中所述，{{site.data.keyword.IBM_notm}} 现在针对使用定制模型的识别请求，每分钟音频额外收费 0.03 美元 (USD)。
-   {{site.data.keyword.IBM_notm}} 更新了服务的定价，具体表现在：
    -   取消了使用窄带模型的附加价格
    -   为高用量客户提供累进分层定价
    -   对于使用美国英语或日语定制语言模型的识别请求，每分钟音频额外收费 0.03 美元 (USD)

    有关定价更新的更多信息，请参阅：
    -   博客帖子 [{{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} API - Pricing Updates](https://www.ibm.com/blogs/bluemix/2017/05/ibm-watson-speech-text-api-pricing-updates/){: external}
    -   [{{site.data.keyword.cloud_notm}} 目录](https://{DomainName}/catalog/services/speech-to-text){: external}中的 {{site.data.keyword.speechtotextshort}} 服务
    -   [定价常见问题](/docs/services/speech-to-text?topic=speech-to-text-faq-pricing)
-   无需再将空数据对象作为以下 `POST` 请求的主体传递：
    -   `POST /v1/sessions`
    -   `POST /v1/register_callback`
    -   `POST /v1/customizations/{customization_id}/train`
    -   `POST /v1/customizations/{customization_id}/reset`
    -   `POST /v1/customizations/{customization_id}/upgrade_model`

    例如，现在使用 `curl` 调用 `POST /v1/sessions` 方法，如下所示：

    ```bash
    curl -X POST -u "{username}:{password}"
    --cookie-jar cookies.txt
    "https://stream.watsonplatform.net/speech-to-text/api/v1/sessions"
    ```
    {: pre}

    无需再将以下 `curl` 选项随请求一起传递：`--data "{}"`。

    如果使用其中一个 `POST` 请求时遇到任何问题，请尝试在请求主体中传递空数据对象。传递空对象不会以任何方式更改请求的性质或含义。
    {: note}

### 2017 年 5 月 22 日
{: #May2017}

最初于 2017 年 3 月公布的以下废弃项现已生效：

-   从所有发起识别请求的方法中除去了 `continuous` 参数。现在，服务会对整个音频流进行转录，直至音频流结束或超时，两者以最先发生的时间为准。此行为等效于将原先的 `continuous` 参数设置为 `true`。先前，缺省情况下，如果省略了此参数或将其设置为 `false`，服务会在非语音（通常为静默）的第一个半秒处停止转录。

    将此参数设置为 `true` 的现有应用程序不会看到行为有变化。将此参数设置为 `false` 或依赖于缺省行为的应用程序很可能会看到变化。如果请求指定了此参数，现在服务会通过返回针对未知参数的警告消息进行响应：

    ```javascript
    "warnings": [
      "未知自变量：continuous。"
    ]
    ```
    {: codeblock}

    尽管发出此警告，请求仍会成功，并且现有会话或 WebSocket 连接不受影响。

    {{site.data.keyword.IBM_notm}} 根据开发者社区的大量反馈决定除去了此参数，这些反馈指出 `continuous=false` 几乎没有什么用，而且可能会降低总体转录准确性。
-   在不发送音频时，无法再避免会话超时：
    -   使用 WebSocket 接口时，客户机无法再通过发送将 `action` 参数设置为 `no-op` 的 JSON 文本消息来使连接保持活动。发送 `no-op` 消息不会生成错误，但也没有任何效果。
    -   通过 HTTP 接口使用会话时，客户机无法再通过发送 `GET /v1/sessions/{session_id}/recognize` 请求来延长会话。此方法仍会返回活动会话的状态，但不会使会话保持活动状态。现在，可以执行以下操作来使会话保持活动：
    -   将 `inactivity_timeout` 参数设置为 `-1` 以避免 30 秒不活动超时。
    -   向服务发送任何音频数据（包括只含静默的音频），以避免 30 秒会话超时。但会根据发送到服务的任何数据的持续时间向您收费，包括发送用于延长会话的静默。

    有关更多信息，请参阅[超时](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts)。理想情况下，您会在刚好获取音频进行转录之前建立会话，并通过以接近实时的速率发送音频来保持该会话。此外，确保应用程序从关闭的会话或连接正常恢复。

    {{site.data.keyword.IBM_notm}} 除去了此功能，以确保能继续为所有用户提供一流的低延迟语音识别服务。

### 2017 年 4 月 10 日
{: #April2017}

-   现在，服务支持说话者标签功能用于以下宽带模型：
    -   `en-US-BroadbandModel`
    -   `es-ES-BroadbandModel`
    -   `ja-JP_BroadbandModel`

    有关更多信息，请参阅[说话者标签](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels)。
-   现在，服务支持使用 Opus 或 Vorbis 编码解码器的 Web 媒体 (WebM) 音频格式。除了使用 Opus 编码解码器的 Ogg 音频格式外，服务现在还支持使用 Vorbis 编码解码器的 Ogg 音频格式。有关受支持音频格式的更多信息，请参阅[音频格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   现在，服务支持跨源资源共享 (CORS)，以允许基于浏览器的客户机直接调用服务。有关更多信息，请参阅 [CORS 支持](/docs/services/speech-to-text?topic=speech-to-text-developerOverview#cors)。
-   现在，异步 HTTP 接口提供了 `POST /v1/unregister_callback` 方法，用于注销列入白名单的回调 URL。有关更多信息，请参阅[注销回调 URL](/docs/services/speech-to-text?topic=speech-to-text-async#unregister)。
-   对于特别长的音频文件的识别请求，WebSocket 接口现在不再超时。您无需再使用 JSON `start` 消息来请求中间结果以避免超时。（2016 年 3 月 10 日的发行说明中描述了此问题。）
-   现在，如果尝试使用 `DELETE /v1/customizations/{customization_id}` 方法来删除不存在的定制模型，会返回 HTTP 响应代码 401。
-   现在，如果尝试使用 `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法来删除不存在的语料库，会返回 HTTP 响应代码 400。

### 2017 年 3 月 8 日
{: #March2017}

现在，异步 HTTP 接口已一般可用。在此日期之前，此接口是 Beta 功能。

### 2016 年 12 月 1 日
{: #December2016}

-   现在，服务为美国英语、西班牙语或日语的窄带音频提供了 Beta 说话者标签功能。此功能用于在多人交流中标识哪些词是哪些说话者说的。无会话、基于会话、异步和 WebSocket 识别方法均各自包含 `speaker_labels` 参数，此参数接受布尔值，以指示是否要在响应中包含说话者标签。有关此功能的更多信息，请参阅[说话者标签](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels)。
-   现在，除了美国英语外，日语也支持 Beta 语言模型定制接口。此接口的所有方法都支持日语。有关更多信息，请参阅以下各部分：
    -   有关更多信息，请参阅[创建定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-languageCreate)和[使用定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-languageUse)。
    -   有关添加语料库文本文件的一般注意事项和特定于日语的注意事项，请参阅[准备语料库文本文件](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#prepareCorpus)和[添加语料库文件时发生的情况](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#parseCorpus)。
    -   有关为定制词指定 `sounds_like` 字段时特定于日语的注意事项，请参阅[针对日语的准则](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-jaJP)。
    -   有关定制接口的所有方法的更多信息，请参阅 [API 参考](https://{DomainName}/apidocs/speech-to-text){: external}。
-   现在，语言模型定制接口包含 `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法，用于列出有关指定语料库的信息。此方法可用于监视向定制模型添加语料库的请求的状态。有关更多信息，请参阅[列出定制语言模型的语料库](/docs/services/speech-to-text?topic=speech-to-text-manageCorpora#listCorpora)。
-   现在，`GET /v1/customizations/{customization_id}/words` 和 `GET /v1/customizations/{customization_id}/words/{word_name}` 方法返回的 JSON 输出包含针对每个词的 `count` 字段。此字段指示在所有语料库中找到该词的次数。如果某个定制词在由任何语料库添加之前已添加到模型，那么计数会从 `1` 开始。如果该词先从语料库添加，并在以后进行修改，那么计数仅反映在语料库中找到该词的次数。有关更多信息，请参阅[列出定制语言模型中的词](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords)。

    对于在引入 `count` 字段之前创建的定制模型，此字段始终保持为 `0`。要针对此类模型更新此字段，请重新添加模型的语料库，并在 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法中包含 `allow_overwrite` 参数。
-   现在，`GET /v1/customizations/{customization_id}/words` 方法包含 `sort` 查询参数，用于控制列出词的顺序。此参数接受两个自变量 `alphabetical` 或 `count`，用于指示如何对词进行排序。可以将可选的 `+` 或 `-` 附加到自变量前面，以指示结果是按升序还是降序排序。缺省情况下，此方法会按字母顺序升序显示词。有关更多信息，请参阅[列出定制语言模型中的词](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords)。

    对于在引入 `count` 字段之前创建的定制模型，将 `count` 自变量与 `sort` 参数一起使用没有任何意义。请将缺省 `alphabetical` 自变量用于此类模型。
-   现在，可以作为 `GET /v1/customizations/{customization_id}/words` 和 `GET /v1/customizations/{customization_id}/words/{word_name}` 方法的 JSON 响应一部分返回的 `error` 字段是数组。如果服务发现定制词定义中有一个或多个问题，那么此字段会列出定义中每个有问题的元素，并提供描述问题的消息。有关更多信息，请参阅[列出定制语言模型中的词](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords)。
-   识别方法的 `keywords_threshold` 和 `word_alternatives_threshold` 参数不再接受空值。要在响应中省略关键字和词替代项，请省略这两个参数。指定的值必须为浮点数。

有关服务接口的更多信息，请参阅 [API 参考](https://{DomainName}/apidocs/speech-to-text){: external}。

### 2016 年 9 月 22 日
{: #September2016}

-   现在，服务为美国英语提供了新的 Beta 语言模型定制接口。可以使用此接口通过创建包含特定于领域的术语的定制语言模型，对服务的基本词汇表和语言模型进行定制。可以单独添加定制词，也可以让服务从语料库中抽取定制词。要将定制模型用于任何服务接口提供的语音识别方法，请传递 `customization_id` 查询参数。有关更多信息，请参阅：
    -   [创建定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-languageCreate)
    -   [使用定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-languageUse)
    -   [API 参考](https://{DomainName}/apidocs/speech-to-text){: external}
-   现在，受支持音频格式的列表包含 `audio/mulaw`，用于提供使用 u-law（或 mu-law）数据算法进行编码的单声道音频。使用此格式时，还必须指定捕获音频的采样率。有关更多信息，请参阅[音频格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   现在，`GET /v1/models` 和 `GET /v1/models/{model_id}` 方法会在针对每个语言模型的输出中返回 `supported_features` 字段。这一额外信息描述了模型是否支持定制。有关更多信息，请参阅 [API 参考](https://{DomainName}/apidocs/speech-to-text){: external}。

### 2016 年 6 月 30 日
{: #June2016b}

现在，Beta 异步 HTTP 接口支持服务所支持的所有语言。此接口先前仅可用于美国英语。有关更多信息，请参阅[异步 HTTP 接口](/docs/services/speech-to-text?topic=speech-to-text-async)和 [API 参考](https://{DomainName}/apidocs/speech-to-text){: external}。

### 2016 年 6 月 23 日
{: #June2016a}

-   现在，Beta 异步 HTTP 接口已可用。此接口通过非阻塞 HTTP 调用为美国英语转录提供了完整的识别功能。您可以注册回调 URL，并提供用户指定的私钥字符串，以通过数字签名实现认证和数据完整性。有关更多信息，请参阅[异步 HTTP 接口](/docs/services/speech-to-text?topic=speech-to-text-async)和 [API 参考](https://{DomainName}/apidocs/speech-to-text){: external}。
-   提供了 Beta 智能格式设置功能，用于在最终文字记录中将日期、时间、数字串和号码、电话号码、货币值以及因特网地址转换为更传统的表示法。通过在识别请求中将 `smart_formatting` 参数设置为 `true` 可启用此功能。这是 Beta 功能，仅可用于美国英语。有关更多信息，请参阅[智能格式设置](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting)。
-   现在，用于语音识别的受支持模型的列表包含 `fr-FR_BroadbandModel`，用于采样率最低为 16 千赫兹的法语音频。有关更多信息，请参阅[语言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)。
-   现在，受支持音频格式的列表包含 `audio/basic`。此格式提供了使用采样率为 8 千赫兹的 8 位 u-law（或 mu-law）数据进行编码的单声道音频。有关更多信息，请参阅[音频格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   各种识别方法都可以返回 `warnings` 响应，此响应包含有关请求中含有的无效查询参数或 JSON 字段的消息。警告的格式已更改。例如，`"warnings": "未知自变量：[u'{invalid_arg_1}'，u'{invalid_arg_2}']。"` 现在为 `"warnings": "未知自变量：{invalid_arg_1}，{invalid_arg_2}。"`
-   对于不将数据传递到服务的 HTTP `POST` 请求，必须包含 `{}` 格式的空请求主体。使用 `curl` 命令时，可通过 `--data` 选项来传递空数据。

### 2016 年 3 月 10 日
{: #March2016}

-   现在，两种形式的数据传输（一次性传递和流式传输）都将音频数据的大小限制为 100 MB，这与 WebSocket 接口的做法一样。以前，一次性方法的最大数据限制为 4 MB。有关更多信息，请参阅[音频传输](/docs/services/speech-to-text?topic=speech-to-text-input#transmission)（对于所有接口）和[发送音频和接收识别结果](/docs/services/speech-to-text?topic=speech-to-text-websockets#WSaudio)（对于 WebSocket 接口）。WebSocket 部分还讨论了 WebSocket 接口强制实施的最大帧或消息大小为 4 MB。
-   现在，识别请求的 JSON 响应可以包含有关请求中含有的无效查询参数或 JSON 字段的警告消息数组。数组的每个元素都是一个字符串，用于描述警告的性质，后跟无效自变量字符串数组。例如，`"warnings": [ "未知自变量：[u'{invalid_arg_1}'，u'{invalid_arg_2}']。" ]`. 有关更多信息，请参阅 [API 参考](https://{DomainName}/apidocs/speech-to-text){: external}。
-   不推荐使用*用于 Apple&reg; iOS 操作系统的 Beta {{site.data.keyword.watson}} 语音软件开发包 (SDK)*。 请改为使用*用于 Apple&reg; iOS 操作系统的 {{site.data.keyword.watson}} SDK*。新的 SDK 可从 GitHub 上 `watson-developer-cloud` 名称空间中的 [ios-sdk 存储库](https://github.com/watson-developer-cloud/ios-sdk){: external}中获取。

目前，WebSocket 接口存在以下已知问题：

-   对于特定长的音频文件的识别请求，服务可能需要几分钟才能生成最终结果。对于 WebSocket 接口，在服务准备响应期间，底层 TCP 连接会保持空闲。因此，该连接可能会由于超时而关闭。要避免使用 WebSocket 接口时发生此超时，请在 `start` 消息的 JSON 中请求中间结果 (`\"interim_results\": \"true\"`) 以发起请求。如果不需要中间结果，可以将其废弃。此问题将在未来更新中解决。

### 2016 年 1 月 19 日
{: #January2016}

2016 年 1 月 19 日，服务更新为包含新的不雅言辞过滤功能。缺省情况下，对于美国英语音频，服务会检剔其转录结果中的不雅言辞。有关更多信息，请参阅[不雅言辞过滤](/docs/services/speech-to-text?topic=speech-to-text-output#profanity_filter)。

### 2015 年 12 月 17 日
{: #December2015}

-   现在，服务提供关键字识别功能。您可以指定要在输入音频中匹配的关键字字符串数组。此外，还必须指定用户定义的置信度级别，词必须达到此置信度级别才会被视为关键字的匹配项。有关更多信息，请参阅[关键字识别](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting)。    关键字识别是 Beta 功能。
    
-   现在，服务提供词替代项功能。此功能会针对输入音频中满足用户定义的置信度级别的词，返回替代假设。有关更多信息，请参阅[词替代项](/docs/services/speech-to-text?topic=speech-to-text-output#word_alternatives)。    词替代项是 Beta 功能。
    
-   服务支持更多语言及其转录模型：对于英国英语，支持 `en-UK_BroadbandModel` 和 `en-UK_NarrowbandModel`，对于现代标准阿拉伯语，支持 `ar-AR_BroadbandModel`。有关更多信息，请参阅[语言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)。
-   HTTP 识别请求不再受 10 分钟平台超时的限制。现在，只要识别在持续进行，服务就会通过每 20 秒在响应 JSON 对象中发送空格字符来使连接保持活动。有关更多信息，请参阅[超时](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts)。
-   对于基于会话的 HTTP 方法 `GET /v1/sessions/{session_id}/observe_result` 和 `POST /v1/sessions/{session_id}/recognize`，服务不再返回 HTTP 状态码 490。现在，服务将改为使用 HTTP 状态码 400 进行响应。

    在服务针对基于会话的方法的错误返回的 JSON 响应中，现在服务还会包含新的 `session_closed` 字段。如果会话由于错误而关闭，那么此字段会设置为 `true`。有关任何方法的可能返回码的更多信息，请参阅 [API 参考](https://{DomainName}/apidocs/speech-to-text){: external}。
-   使用 `curl` 命令通过服务转录音频时，无需再使用 `--limit-rate` 选项将数据传输速率限制为不超过 40,000 字节/秒。

### 2015 年 9 月 21 日
{: #September2015}

-   有两个新的 Beta 移动 SDK 可用于语音服务。这两个 SDK 支持移动应用程序与 {{site.data.keyword.speechtotextshort}} 和 {{site.data.keyword.texttospeechshort}} 服务进行交互。
    -   *用于 Google Android&trade; 平台的 {{site.data.keyword.watson}} 语音 SDK* 支持将音频实时流式传输到 {{site.data.keyword.speechtotextshort}} 服务，并在您说话的同时接收音频的文字记录。该项目包含一个示例应用程序，用于显示与这两种语音服务的交互。SDK 可从 GitHub 上 `watson-developer-cloud` 名称空间中的 [speech-android-sdk 存储库](https://github.com/watson-developer-cloud/speech-android-sdk){: external}中获取。
    -   *用于 Apple&reg; iOS 操作系统的 {{site.data.keyword.watson}} 语音 SDK* 支持将音频流式传输到 {{site.data.keyword.speechtotextshort}} 服务，并在响应中接收音频的文字记录。SDK 可从 GitHub 上 `watson-developer-cloud` 名称空间中的 [speech-ios-sdk 存储库](https://github.com/watson-developer-cloud/speech-ios-sdk){: external}中获取。

    这两个 SDK 都支持使用 {{site.data.keyword.cloud_notm}} 服务凭证或认证令牌向语音服务进行认证。

    由于 SDK 是 Beta，因此未来会随时更改。
    {: note}
-   服务支持两种新的语言：巴西葡萄牙语和普通话。这两种新语言的模型为 `pt-BR_BroadbandModel`、`pt-BR_NarrowbandModel`、`zh-CN_BroadbandModel` 和 `zh-CN_NarrowbandModel`。有关更多信息，请参阅[语言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)。
-   对于使用 Opus 编码解码器的 Ogg 格式文件，HTTP `POST` 请求 `/v1/sessions/{session_id}/recognize` 和 `/v1/recognize` 以及 WebSocket `/v1/recognize` 请求都支持转录新的媒体类型：`audio/ogg;codecs=opus`。此外，这些方法的 `audio/wav` 格式现在支持任何编码。除去了有关使用线性 PCM 编码的限制。有关更多信息，请参阅[音频格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   现在，使用 HTTP 接口转录长音频文件时，服务支持克服超时。使用会话时，可以使用 `GET /v1/sessions/{session_id}/observe_result` 和 `POST /v1/sessions/{session_id}/recognize` 方法为长时间运行的识别任务指定序列标识，从而利用长时间轮询模式。通过使用这些方法的新 `sequence_id` 参数，可以在提交识别请求之前、期间或之后请求结果。
-   对于美国英语语言模型 `en_US_BroadbandModel` 和 `en_US_NarrowbandModel`，现在服务可正确对许多专有名词设置首字母大写。例如，服务的新返回文本显示为“Barack Obama graduated from Columbia University”，而不是“barack obama graduated from columbia university”。如果应用程序对专用名词的大小写有任何敏感性，您可能会对此更改感兴趣。
-   HTTP `DELETE /v1/sessions/{session_id}` 请求不会返回状态码 415“不支持的媒体类型”。此返回码已从该方法的文档中除去。

### 2015 年 7 月 1 日
{: #July2015}

服务于 2015 年 7 月 1 日从 Beta 转为一般可用 (GA)。{{site.data.keyword.speechtotextshort}} API 的 Beta 和 GA 版本之间存在以下差异。GA 发行版需要用户升级到服务的新版本。

-   HTTP API 的 GA 版本与 Beta 版本兼容。仅当显式指定模型名称时，才需要更改现有应用程序代码。例如，GitHub 中服务可用的样本代码包含 `demo.js` 文件中的以下代码行：

    ```javascript
    model: 'WatsonModel'
    ```
    {: codeblock}

    此行针对服务的 Beta 版本指定了缺省模型 `WatsonModel`。如果应用程序也指定了此模型，那么需要将其更改为使用 GA 版本支持的其中一个新模型。有关更多信息，请参阅下一个项目符号。
-   现在，服务支持新的编程模型，用于通过 WebSocket 连接在客户机和服务之间进行直接交互。通过使用此模型，客户机可以获取用于直接与服务进行通信的认证令牌。通过令牌，无需 {{site.data.keyword.cloud_notm}} 中的服务器端代理应用程序代表客户机来调用服务。令牌是客户机与服务交互的首选方法。

    服务会继续支持依赖于服务器端代理在客户机与服务之间中继音频和消息的旧编程模型。但是，新模型的效率更高，吞吐量更大。
-   现在，`POST /v1/sessions` 和 `POST /v1/recognize` 方法以及 WebSocket `/v1/recognize` 方法支持 `model` 查询参数。使用此参数可指定有关音频的信息：

    -   语言：*英语*、*日语*或*西班牙语*
    -   最小采样率：*宽带*（16 千赫兹）或*窄带*（8 千赫兹）

    有关更多信息，请参阅[语言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)。
-   现在，`recognize` 方法的 `Content-Type` 头除了支持 `audio/flac` 和 `audio/l16`，还支持 `audio/wav`（用于波形音频文件格式 (WAV) 文件）。有关受支持音频格式的更多信息，请参阅[音频格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   现在，`recognize` 方法支持若干新的查询参数，这些参数可用于定制服务以满足应用程序需求：
    -   `inactivity_timeout` 参数用于设置超时值（以秒为单位），如果服务在流式方式下检测到静默（无语音），那么静默时间超过此时间后，服务会关闭连接。缺省情况下，服务在 30 秒静默后会终止会话。有关更多信息，请参阅[超时](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts)。
    -   `max_alternatives` 参数用于指示服务返回音频转录的 *n* 个最佳替代假设。有关更多信息，请参阅[最大替代项数](/docs/services/speech-to-text?topic=speech-to-text-output#max_alternatives)。
    -   `word_confidence` 参数用于指示服务返回转录中每个词的置信度分数。有关更多信息，请参阅[词置信度](/docs/services/speech-to-text?topic=speech-to-text-output#word_confidence)。
    -   `timestamps` 参数用于指示服务返回相对于转录中每个词的音频开始时间的开始时间和结束时间。有关更多信息，请参阅[词时间戳记](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps)。
-   现在，`GET /v1/sessions/{session_id}/observeResult` 方法更名为 `GET /v1/sessions/{session_id}/observe_result`。为了实现向后兼容性，仍支持名称 `observeResult`。
-   现在，`GET /v1/sessions/{session_id}/observe_result`、`POST /v1/sessions/{session_id}/recognize` 和 `POST /v1/recognize` 方法包含头参数 `X-WDC-PL-OPT-OUT`，用于控制服务是否使用来自请求的音频和转录数据来改进未来的结果。WebSocket 接口包含等效的查询参数。指定值 `1` 可阻止服务使用音频和转录结果。此参数仅应用于当前请求。此新头将替换 Beta API 中的 `X-logging` 头。请参阅[控制 {{site.data.keyword.watson}} 服务的请求日志记录](/docs/services/watson?topic=watson-gs-logging-overview)。
-   现在，服务在流式方式下有每个会话 100 MB 数据的限制。通过为头 `Transfer-Encoding` 指定值 `chunked` 可指定流式方式。一次性传递音频文件仍将发送的数据大小限制为 4 MB。有关更多信息，请参阅[音频传输](/docs/services/speech-to-text?topic=speech-to-text-input#transmission)。
-   对于 `/v1/models`、`/v1/models/{model_id}`、`/v1/sessions`、`/v1/sessions/{session_id}`、`/v1/sessions/{session_id}/observe_result`、`/v1/sessions/{session_id}/recognize` 和 `/v1/recognize` 方法，添加了错误代码 415（“不支持的媒体类型”）。
-   对于 `/v1/sessions/{session_id}/recognize` 方法的 `POST` 和 `GET` 请求，修改了以下错误代码：
    -   错误代码 404（“找不到 Session_id”）包含描述性更强的消息（`POST` 和 `GET`）。
    -   错误代码 503（“会话已在处理请求。在同一会话中不允许并行请求。发生此错误后，会话仍会保持活动状态。”）包含描述性更强的消息（仅限 `POST`）。
-   对于 `/v1/sessions` 和 `/v1/recognize` 方法的 HTTP `POST` 请求，可能会返回错误代码 503（“服务不可用”）。在使用 `/v1/recognize` 方法创建 WebSocket 连接时，也可能返回此错误代码。
