---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-13"

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

# 开发者概述
{: #developerOverview}

您可以使用 WebSocket 接口以及使用同步或异步 HTTP 具象状态传输 (REST) 接口来访问 {{site.data.keyword.speechtotextfull}} 服务的功能。此外，还可以定制服务的语言模型以适合您的领域和环境。SDK 可用于简化许多编程语言的应用程序开发工作。
{: shortdesc}

## 使用服务进行编程
{: #programming}

[发出识别请求](/docs/services/speech-to-text?topic=speech-to-text-basic-request)说明了如何使用服务的每种编程接口来请求基本转录：

-   [WebSocket 接口](/docs/services/speech-to-text?topic=speech-to-text-websockets)通过全双工连接提供了低延迟、高吞吐量的高效实现。
-   [同步 HTTP 接口](/docs/services/speech-to-text?topic=speech-to-text-http)提供了用于对阻塞请求中的音频进行转录的基本接口。
-   [异步 HTTP 接口](/docs/services/speech-to-text?topic=speech-to-text-async)提供了非阻塞接口，允许您注册回调 URL 用于接收有关作业状态和结果的通知，或者轮询服务来获取作业状态和结果。

这些接口提供的语音识别功能相同，但您可以根据使用的接口，将同一参数指定为请求头、查询参数或 JSON 对象的参数。此外，WebSocket 和同步 HTTP 接口可接受单个请求中最多 100 MB 音频数据。异步 HTTP 接口接受最多 1 GB 音频数据。

-   有关所有可用语音识别参数的描述，请参阅[参数摘要](/docs/services/speech-to-text?topic=speech-to-text-summary)。
-   有关所有方法及其参数的描述以及示例，请参阅 [API 参考](https://{DomainName}/apidocs/speech-to-text){: external}。

## WebSocket 接口的优点
{: #advantages}

相比 HTTP 接口，WebSocket 接口有多项优点：

-   WebSocket 接口提供单套接字、全双工的通信信道。通过此接口，客户机可向服务发送请求和音频，并以异步方式通过单个连接接收结果。
-   此接口提供了简单、强大得多的编程体验。服务将事件驱动的响应发送到客户机的消息，从而无需客户机轮询服务器。
-   通过此接口，可建立并无限期使用单个已认证的连接。HTTP 接口需要认证对服务的每个调用。
-   缩短了等待时间。识别结果可更快地到达，因为服务会将结果直接发送到客户机。HTTP 接口需要四个不同的请求和连接才能实现相同的结果。
-   降低了网络利用率。WebSocket 协议是轻量级的。它只需要单个连接就能执行实时语音识别。
-   支持音频从浏览器（HTML5 WebSocket 客户机）直接流式传输到服务。

## 定制服务
{: #customizing}

[定制接口](/docs/services/speech-to-text?topic=speech-to-text-customization)支持创建定制模型，以改进服务的语音识别功能：

-   [定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-languageCreate)支持为基本模型定义特定于领域的词。定制语言模型使用特定于领域（例如，医学和法律）的术语来扩展服务的基本词汇表。
-   [定制声学模型](/docs/services/speech-to-text?topic=speech-to-text-acoustic)支持调整基本模型，以适应环境和说话者的声学特征。定制声学模型提高了服务识别语音中特定声学特征的能力。
-   [语法](/docs/services/speech-to-text?topic=speech-to-text-grammars)支持将服务可以识别的短语限制为语法规则中所定义的短语。通过将搜索空间限制为搜索有效字符串，服务可以更快、更准确地交付结果。定制语言模型支持语法。

可以使用定制语言模型和/或定制声学模型通过任何服务接口进行语音识别。

您必须有标准价格套餐，才能使用语言模型定制或声学模型定制。轻量套餐的用户无法使用定制接口。有关更多信息，请参阅 {{site.data.keyword.speechtotextshort}} 服务的[定价页面](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external}。
{: note}

## 获取度量值
{: #overview-metrics}

服务对于语音识别请求，提供了两种类型的可选度量值：

-   [处理度量值](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics)，用于提供有关服务的输入音频分析的详细计时信息。服务以指定的时间间隔返回度量值以及转录事件，例如中间结果和最终结果。可以使用度量值来测量服务转录音频的进度。
-   [音频度量值](/docs/services/speech-to-text?topic=speech-to-text-metrics#audio_metrics)，用于提供有关输入音频信号特征的详细信息。在语音处理结束时，结果中提供整个输入音频的聚集度量值。可以使用度量值来确定音频的特征和质量。

您可以使用 WebSocket 和异步 HTTP 接口来请求处理度量值。可以使用任何服务接口来请求音频度量值。缺省情况下，服务不会返回度量值。

## CORS 支持
{: #cors}

服务支持跨源资源共享 (CORS)。通过使用 CORS，Web 页面可以直接从外部域请求资源。CORS 可绕过同源安全策略；否则，这些策略会阻止此类请求。由于服务支持 CORS，因此 Web 页面可以直接与服务进行通信，而无需通过托管该页面的 Web 服务器来传递请求。

例如，从 {{site.data.keyword.cloud}} 中的服务器装入的 Web 页面可以直接调用定制 API，而绕过 {{site.data.keyword.cloud_notm}} 服务器。有关更多信息，请参阅 [enable-cors.org](https://enable-cors.org/){: external}。

## 使用软件开发包
{: #sdks}

有多个 SDK 可用于 {{site.data.keyword.speechtotextshort}} 服务，以简化语音应用程序的开发工作。{{site.data.keyword.ibmwatson}} SDK 可用于许多常用的编程语言和平台。

-   有关 SDK 的完整列表以及 GitHub 上 SDK 的链接，请参阅[使用 SDK](/docs/services/watson?topic=watson-using-sdks)。
-   有关 {{site.data.keyword.speechtotextshort}} 服务的 Node、Java&trade;、Python、Ruby、Swift 和 Go SDK 的所有方法的详细信息，请参阅 [API 参考](https://{DomainName}/apidocs/speech-to-text){: external}。

## 了解有关应用程序开发的更多信息
{: #learn}

有关使用 {{site.data.keyword.watson}} 服务和 {{site.data.keyword.cloud_notm}} 的更多信息，请参阅以下内容：

-   有关使用 {{site.data.keyword.watson}} 服务和 {{site.data.keyword.cloud_notm}} 的简介，请参阅 [{{site.data.keyword.watson}} 和 {{site.data.keyword.cloud_notm}} 入门](/docs/services/watson?topic=watson-about)。
-   所有新的服务实例都使用 {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) 进行认证。较旧的服务实例可能会继续使用其现有 Cloud Foundry 服务凭证中的 `{username}` 和 `{password}` 进行认证。有关向服务进行认证的更多信息，请参阅发行说明中的 [2018 年 10 月 30 日服务更新](/docs/services/speech-to-text?topic=speech-to-text-release-notes#October2018b)。
-   有关控制对所有 {{site.data.keyword.watson}} 服务执行的缺省请求日志记录的信息，请参阅[控制 {{site.data.keyword.watson}} 服务的请求日志记录](/docs/services/watson?topic=watson-gs-logging-overview)。
