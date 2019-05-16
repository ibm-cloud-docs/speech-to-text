---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-03"

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

# 关于
{: #about}

**服务更新：***{{site.data.keyword.speechtotextshort}} 服务已于 2019 年 4 月 3 日更新。现在，服务允许您向一个定制声学模型添加最多 200 小时的音频。有关更多信息，请参阅发行说明中的 [2019 年 4 月 3 日服务更新](/docs/services/speech-to-text/release-notes.html#April2019)*。

{{site.data.keyword.speechtotextfull}} 服务针对您的应用提供了语音转录功能。服务利用机器学习将语法、语言结构以及音频和声音信号构成组合在一起，从而准确地转录人声。随着服务接收更多语音，服务会持续对其转录进行更新和优化。
{: shortdesc}

服务提供了多种接口，以便适用于输入为语音、输出为文本抄本的任何应用。示例应用包括：

-   应用程序、嵌入式设备和车辆附件的声音控制
-   转录会议和电话会议
-   听写电子邮件和注释

## 支持的接口
{: #interfaces}

{{site.data.keyword.speechtotextshort}} 服务提供了三个接口用于语音识别：

-   [WebSocket 接口](/docs/services/speech-to-text/websockets.html)，用于建立与服务的全双工、低延迟持久连接。通过单个请求，最多可以向服务传递 100 MB 的音频数据。
-   [同步 HTTP 接口](/docs/services/speech-to-text/http.html)，用于对服务进行基本 HTTP 调用。通过一个请求，最多可以传递 100 MB 的音频数据。
-   [异步 HTTP 接口](/docs/services/speech-to-text/async.html)，用于对服务进行非阻塞调用。通过一个请求，可以传递多达 1 GB 的音频数据。

服务还提供了[定制接口](/docs/services/speech-to-text/custom.html)，可用于调整语音识别，以满足您的语言和声学需求。可以使用特定于领域的术语来扩展模型的词汇表，或者调整模型以适应音频的声学特征。还可以添加[语法](/docs/services/speech-to-text/grammar.html)来限制服务可以识别的短语。

-   有关使用服务进行应用程序开发的高级别描述，请参阅[开发者概述](/docs/services/speech-to-text/developer-overview.html)。
-   有关使用每个服务接口发出基本语音识别请求的示例，请参阅[发出识别请求](/docs/services/speech-to-text/basic-request.html)。

SDK 以多种编程语言提供，可简化服务的使用。有关更多信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/speech-to-text){: new_window}。

## 输入功能
{: #inputFeatures}

服务的各接口共享用于将语音转录为文本的常用输入功能：

-   [音频格式](/docs/services/speech-to-text/audio-formats.html) - 可以转录使用 Opus 或 Vorbis 编码解码器的 Ogg 或 Web 媒体 (WebM) 音频、MP3（或 MPEG）、波形音频文件格式 (WAV)、自由无损音频编码解码器 (FLAC)、线性 16 位脉冲编码调制 (PCM)、G.729、A-Law、mu-law（或 u-law）和基本音频。通过使用支持压缩的格式，可以最大限度提高可在一个请求中发送的音频数据量。
-   [语言和模型](/docs/services/speech-to-text/models.html) - 对于大多数语言，可以使用宽带或窄带模型来转录音频。对于最小采样率为 16 千赫兹的音频，请使用宽带模型。对于最小采样率为 8 千赫兹的音频，请使用窄带模型。
-   [音频传输](/docs/services/speech-to-text/input.html#transmission) - 可以将音频作为连续数据块流传递，也可以一次性传递（一次性传递所有数据）。使用流式方法时，服务会强制实施不活动状态[超时](/docs/services/speech-to-text/input.html#timeouts)和会话超时。

## 输出功能
{: #outputFeatures}

各接口还支持以下常用输出功能：

-   [说话者标签](/docs/services/speech-to-text/output.html#speaker_labels)，用于标识美国英语、英国英语、西班牙语或日语音频中的不同说话者。转录会标注每个说话者对多参与者会话的贡献。（Beta 功能。）
-   [关键字识别](/docs/services/speech-to-text/output.html#keyword_spotting)，用于识别与具有用户定义置信度级别的指定关键字字符串相匹配的所讲短语。当音频中的各个短语比完整转录更重要时，关键字识别功能特别有用。例如，客户支持系统可识别关键字以确定如何路由用户请求。
-   [中间结果](/docs/services/speech-to-text/output.html#interim)，用于随着转录的进展而返回渐进假设。服务在转录完成后，会返回最终结果。中间结果仅可用于 WebSocket 接口。
-   [最大替代项数](/docs/services/speech-to-text/output.html#max_alternatives)，用于提供可能的替代抄本。服务会指示其置信度最大的最终结果。
-   [词替代项](/docs/services/speech-to-text/output.html#word_alternatives)，用于请求其发音与抄本中的词相似的替代词。
-   [词置信度](/docs/services/speech-to-text/output.html#word_confidence)，用于返回抄本中每个词的置信度级别。
-   [词时间戳记](/docs/services/speech-to-text/output.html#word_timestamps)，用于返回抄本中每个词的开始时间和结束时间的时间戳记。
-   [智能格式设置](/docs/services/speech-to-text/output.html#smart_formatting)，用于在最终抄本中将日期、时间、数字、货币值、电话号码和因特网地址转换为可读性更高的传统格式。对于美国英语，还可以提供关键字短语，以在最终抄本中包含特定标点符号。美国英语、日语和西班牙语音频支持智能格式设置。（Beta 功能。）
-   [数字编辑](/docs/services/speech-to-text/output.html#redaction)，用于编辑或掩蔽最终抄本中的数字数据。编辑功能旨在从抄本中除去敏感个人信息，例如信用卡号。美国英语、日语和韩语音频支持此功能。（Beta 功能。）
-   [不雅言辞过滤](/docs/services/speech-to-text/output.html#profanity_filter)，用于检剔美国英语抄本中的不雅言辞。

## 语言支持
{: #languages}

服务为以下语言提供了模型：巴西葡萄牙语、法语、德语、日语、韩语、普通话、现代标准阿拉伯语、西班牙语、英国英语和美国英语。服务并不支持所有功能用于所有语言。此外，服务支持一些功能作为一般可用 (GA) 功能用于生产，并支持另一些功能作为 Beta 功能用于不同的语言。

-   WebSocket 和 HTTP 接口对于所有语言一般可用。
-   服务针对不同的语言，提供宽带模型和/或窄带模型。有关更多信息，请参阅[语言和模型](/docs/services/speech-to-text/models.html)。
-   一些语音识别功能仅可用于某些语言。有关更多信息，请参阅[参数摘要](/docs/services/speech-to-text/summary.html)。
-   语言模型定制接口对于大多数语言一般可用。声学模型定制接口作为 Beta 功能可用于所有语言。有关更多信息，请参阅[支持定制的语言](/docs/services/speech-to-text/custom.html#languageSupport)。

## 定价
{: #pricing-index}

有关服务价格套餐的更多信息，请参阅 [{{site.data.keyword.Bluemix_short}} 目录 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/catalog/services/speech-to-text){: new_window} 中的 {{site.data.keyword.speechtotextshort}} 服务。有关与定价和每月使用成本示例相关的问题的答案，请参阅[定价常见问题](/docs/services/speech-to-text/faq-pricing.html)。

## 试用服务
{: #tryOut}

有关运行中 {{site.data.keyword.speechtotextshort}} 服务的示例，请参阅：

-   [快速演示 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://speech-to-text-demo.ng.bluemix.net/){: new_window}，用于演示如何从流式音频输入或上传的文件转录文本。
-   {{site.data.keyword.ibmwatson}} [入门模板工具包 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://www.ibm.com/watson/developercloud/starter-kits.html){: new_window} 中的应用程序，用于演示服务。
-   {{site.data.keyword.watson}} 博客帖子 [Getting robots to listen: Using {{site.data.keyword.watson}}'s {{site.data.keyword.speechtotextshort}} service ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/blogs/watson/2016/07/getting-robots-listen-using-watsons-speech-text-service/){: new_window}，用于说明如何将服务的 WebSocket 接口与 Python 配合使用，以从音频中抽取语音。该帖子提供了完整的教程，用于演示所涉及的代码和步骤。
