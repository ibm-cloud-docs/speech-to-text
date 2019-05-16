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

# 音频格式
{: #audio-formats}

{{site.data.keyword.speechtotextfull}} 服务可从多种不同格式的音频中抽取语音。

-   如果您不熟悉音频，也不了解如何描述和指定音频，请首先参阅[音频特征和术语](#terminology)以帮助您入门。
-   如果您已了解如何使用音频，请跳转至[支持的音频格式](#formats)，以获取有关服务支持的格式的详细信息。

最后的[数据限制和压缩](#limits)、[音频转换](#conversion)和[关于改进语音识别的提示](#audioTips)部分，可以帮助您最充分地利用服务。

## 音频特征和术语
{: #terminology}

以下术语用于描述不同格式音频数据的特征。

### 采样率
{: #samplingRate}

*采样率*（或采样频率）是指每秒获取的音频样本数。采样率的度量单位为赫兹 (Hz) 或千赫兹 (kHz)。例如，每秒 16,000 个样本的采样率等于 16,000 赫兹（或 16 千赫兹）。通过 {{site.data.keyword.speechtotextshort}} 服务，可指定用于指示音频采样率的模型：

-   *宽带*模型用于采样率不低于 16 千赫兹的音频，{{site.data.keyword.IBM}} 建议将其用于响应式实时应用程序（例如，用于实时语音应用程序）。
-   *窄带*模型用于采样率不低于 8 千赫兹的音频，这是通常用于电话音频的采样率。

对于大多数语言和格式，服务都同时支持宽带和窄带音频。服务在识别语音之前，会自动调整音频的采样率，以便与指定的模型相匹配。

-   对于宽带模型，如果所录制音频的采样率高于 16 千赫兹，那么服务会将其转换为 16 千赫兹。
-   对于窄带模型，服务会将音频的采样率转换为 8 千赫兹。

理论上，可以使用宽带或窄带模型发送 44 千赫兹音频，但这会不必要地增加音频的大小。要最大限度提高可以发送的音频量，请使音频采样率与使用的模型相匹配。服务不会接受采样率低于模型固有采样率的音频。

#### 有关音频格式的说明
{: #samplingRateNotes}

-   对于 `audio/alaw`、`audio/l16` 和 `audio/mulaw` 格式，必须指定音频的采样率。
-   对于 `audio/basic` 和 `audio/g729` 格式，服务仅支持窄带音频。

#### 更多信息
{: #samplingRateMore}

-   有关采样率的更多信息，请参阅 [en.wikipedia.org/wiki/Sampling ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/Sampling){: new_window}。选择*采样（信号处理）*。
-   有关服务为每种受支持语言提供的模型的更多信息，请参阅[语言和模型](/docs/services/speech-to-text/models.html)。

### 比特率
{: #bitRate}

*比特率*是指每秒发送的数据比特数。音频流比特率的度量单位为千位/秒 (kbps)。比特率是根据采样率和每个样本存储的比特数计算的。对于语音识别，{{site.data.keyword.IBM}} 建议为音频录制 16 比特/样本。

例如，对于使用宽带采样率 16 千赫兹和 16 比特/样本的音频，其比特率为 256 千位/秒：`(16,000 * 16) / 1000`。

#### 更多信息
{: #bitRateMore}

-   有关比特率的更多信息，请参阅 [en.wikipedia.org/wiki/Bit_rate ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/Bit_rate){: new_window}。
-   有关采样率和比特率的一般讨论，请参阅 [What are bit rates? ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://www.richardfarrar.com/what-are-bit-rates/){: new_window} 和 [Choosing bit rates for podcasts ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://www.richardfarrar.com/choosing-bit-rates-for-podcasts/){: new_window}。

### 压缩
{: #compression}

许多音频格式都会使用*压缩*，以减小音频数据的大小。压缩可减小每个样本存储的比特数，从而降低比特率。某些格式不使用压缩，但大部分格式会使用其中一种基本类型的压缩：

-   *无损*压缩可减小音频的大小，而不会损失质量，但压缩率通常较小。
-   *有损*压缩可使音频大小最高减小 10 倍，但在压缩中会不可挽回地损失一些数据和一定程度的质量。

使用 {{site.data.keyword.speechtotextshort}} 服务时，可以安全地使用有损压缩来最大限度提高可以通过识别请求发送给服务的音频量。由于人声的动态范围比音乐等更有限，因此语音可以容纳的比特率比其他类型的音频低得多。对于语音识别，{{site.data.keyword.IBM}} 建议将 16 比特/样本用于音频，并利用压缩音频数据的格式。

#### 有关音频格式的说明
{: #compressionNotes}

-   `audio/ogg` 和 `audio/webm` 格式是其压缩依赖于对数据进行编码的编码解码器的容器：Opus 或 Vorbis。
-   `audio/wav` 格式是可以包含未压缩、无损或有损数据的容器。

#### 更多信息
{: #compressionMore}

-   有关音频压缩的更多信息，请参阅 [en.wikipedia.org/wiki/Data_compression#Audio ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/Data_compression#Audio){: new_window}。
-   有关使用数据压缩来增加可以通过请求发送的音频量的更多信息，请参阅[数据限制和压缩](#limits)。

### 声道
{: #channels}

*声道*指示所录制音频的流数：

-   *单声道*音频只有一个声道。
-   *立体声*音频通常有两个声道。

{{site.data.keyword.speechtotextshort}} 服务接受最多有 16 个声道的音频。由于服务仅将单声道用于语音识别，因此在代码转换期间会将多声道音频缩混成单声道音频。

#### 有关音频格式的说明
{: #channelsNotes}

-   对于 `audio/l16` 格式，如果音频有多个声道，那么必须指定声道数。
-   对于 `audio/wav` 格式，服务接受最多有 9 个声道的音频。

#### 更多信息
{: #channelsMore}

-   有关音频声道的更多信息，请参阅 [en.wikipedia.org/wiki/Audio_signal ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/Audio_signal){: new_window}。

### 字节序
{: #endianness}

*字节序*指示底层计算机体系结构如何组织数据的字节：

-   *大尾数法*按最高有效位对数据排序。
-   *小尾数法*按最低有效位对数据排序。

{{site.data.keyword.speechtotextshort}} 服务会自动检测传入音频的字节序。

#### 有关音频格式的说明
{: #endiannessNotes}

-   对于 `audio/l16` 格式，可以根据需要指定字节序以禁用自动检测。

#### 更多信息
{: #endiannessMore}

-   有关字节序的更多信息，请参阅 [en.wikipedia.org/wiki/Endianness ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/Endianness){: new_window}。

### 音频频率
{: #frequency}

*音频频率*是指音频中人耳可以听到的频率的范围。人耳可听到的标准频率公认为是 20 到 20,000 赫兹。可以使用声谱分析来生成显示音频频率内容的声谱图。

应用于音频的采样率通常是音频最大频率的两倍。例如，采样率为 16 千赫兹，表示被采样音频信号的最大频率为 8 千赫兹。创建服务的模型时，请记住这一点。

-   窄带模型是使用采样率为 8 千赫兹的音频构建的。窄带模型应该查找范围小于或等于 4 千赫兹的信息。
-   宽带模型是使用采样率为 16 千赫兹的音频构建的。宽带模型应该查找 4-8 千赫兹范围内的信息。

模型的训练数据源自不同通道（电话用于窄带模型）。模型可反映出对其进行训练的通道的特征。

#### 升采样
{: #upsampling}

*升采样*用于增大音频的采样率，而不会在音频中引入新的信息。升采样生成的是本该以更高采样率对音频采样来获得的音频信号的近似值。升采样会增大音频数据的大小。

以窄带频率原始采样的音频中的信息限制在 0 到 4 千赫兹范围内。通过对窄带音频执行升采样来提高其采样率，不太可能提高语音识别准确性。如果对窄带音频执行升采样，那么该音频会缺少宽带模型预期的范围内的信息。此外，在窄带样本的预期范围内找到的信息与在宽带样本中同一范围内找到的信息有质的不同。因此，升采样实际上会导致识别准确性下降。

对于 16 千赫兹的宽带采样率，被采样音频信号中的最大频率应该为 8 千赫兹。因此，在以 16 千赫兹的采样率对原始信号采样之前，必须按 8 千赫兹对该信号进行过滤。否则，由于称为*重叠*的现象，会发生质量下降。要了解原因，请参阅 [Nyquist frequency ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/Nyquist_frequency){: new_window}。

打个比方来说，假设在大型平板屏幕 HDTV 上查看 VHS 磁带。您会看到图像模糊不清，这是因为在高清设备上播放磁带无法将新信息真正添加到流中。它只是为了使格式能与更好的设备兼容。对音频执行升采样也是同样的道理。

#### 降采样
{: #downsampling}

*降采样*用于降低音频的采样率。降采样生成的是本该以更低采样率对音频采样来获得的音频信号的近似值。降采样不会从音频信号中除去任何信息，而确实可减小音频数据的大小。

在某些情况下，对音频执行降采样可能非常有效。例如，如果音频的采样率大于 8 千赫兹，*并且*声谱检查显示没有超过 4 千赫兹的频率内容，请考虑对音频执行降采样，使采样率降至 8 千赫兹。

#### 更多信息
{: #frequencyMore}

-   有关音频频率的更多信息，请参阅 [https://en.wikipedia.org/wiki/Audio_frequency ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/Audio_frequency){: new_window}。
-   有关升采样的更多信息，请参阅 [https://en.wikipedia.org/wiki/Upsampling ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/Upsampling){: new_window}。
-   有关降采样的更多信息，请参阅 [https://en.wikipedia.org/wiki/Downsampling ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/Downsampling_%28signal_processing%29){: new_window}。

## 支持的音频格式
{: #formats}

表 1 提供了服务支持的音频格式的摘要。

-   *音频格式和压缩*标识每种格式并指示其支持的压缩。通过单个同步 HTTP 或 WebSocket 请求，最多可以向服务发送 100 MB 的音频。通过使用支持压缩的格式，可以减小音频的大小，以最大限度提高可以传递给服务的数据量。有关更多信息，请参阅[数据限制和压缩](#limits)。
-   *内容类型规范*指示是否必须使用 `Content-Type` 头或等效参数来指定发送给服务的音频的格式（MIME 类型）。可以为任何请求指定音频格式，但这并不总是必要的：
    -   对于大多数格式，内容类型是可选的。可以省略内容类型或指定 `application/octet-stream` 让服务自动检测格式。
    -   对于其他格式，内容类型是必需的。这些格式不提供服务需要自动检测其格式的信息，例如采样率。
-   最后几列标识每种格式的其他*必需参数*和*可选参数*。以下各部分提供了有关这些参数的更多信息。

<table>
  <caption>表 1. 支持的音频格式摘要</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom">
      音频格式<br>和压缩
    </th>
    <th style="text-align:center; vertical-align:bottom">
      内容类型<br/>规范
    </th>
    <th style="text-align:center; vertical-align:bottom">
      必需<br/>参数
    </th>
    <th style="text-align:center; vertical-align:bottom">
      可选<br/>参数
    </th>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/alaw](#alaw)<br/>有损
    </td>
    <td style="text-align:center">
      必需
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      无
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/basic](#basic)<br/>有损
    </td>
    <td style="text-align:center">
      必需
    </td>
    <td style="text-align:center">
      无
    </td>
    <td style="text-align:center">
      无
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/flac](#flac)<br/>无损
    </td>
    <td style="text-align:center">
      可选
    </td>
    <td style="text-align:center">
      无
    </td>
    <td style="text-align:center">
      无
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/g729](#g729)<br/>有损
    </td>
    <td style="text-align:center">
      可选
    </td>
    <td style="text-align:center">
      无
    </td>
    <td style="text-align:center">
      无
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/l16](#l16)<br/>无
    </td>
    <td style="text-align:center">
      必需
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      <code>channels={integer}</code><br/>
      <code>endianness=big-endian</code><br/>
      <code>endianness=little-endian</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/mp3](#mp3)<br/>
      [audio/mpeg](#mp3)<br/>有损
    </td>
    <td style="text-align:center">
      可选
    </td>
    <td style="text-align:center">
      无
    </td>
    <td style="text-align:center">
      无
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/mulaw](#mulaw)<br/>有损
    </td>
    <td style="text-align:center">
      必需
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      无
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/ogg](#ogg)<br/>有损
    </td>
    <td style="text-align:center">
      可选
    </td>
    <td style="text-align:center">
      无
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/wav](#wav)<br/>无、无损<br/>或有损
    </td>
    <td style="text-align:center">
      可选
    </td>
    <td style="text-align:center">
      无
    </td>
    <td style="text-align:center">
      无
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/webm](#webm)<br/>有损
    </td>
    <td style="text-align:center">
      可选
    </td>
    <td style="text-align:center">
      无
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
</table>

使用 `curl` 命令通过 HTTP 接口发出语音识别请求时，必须使用 `Content-Type` 头指定音频格式，指定 `"Content-Type: application/octet-stream"` 或指定 `"Content-Type:"`。如果完全省略该头，`curl` 将使用缺省值 `application/x-www-form-urlencoded`。
{: important}

### audio/alaw 格式
{: #alaw}

*A-law* (`audio/alaw`) 是一种单声道有损音频格式。它使用的算法类似于 `audio/basic` 和 `audio/mulaw` 格式应用的 u-law 算法，但 A-law 算法会生成不同的信号特征。使用此格式时，服务需要有关格式规范的额外参数。

<table>
  <caption>表 2. `audio/alaw` 格式的参数</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      参数
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      描述
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>必需</em>
    </td>
    <td>
      一个整数，用于指定捕获音频的采样率。例如，对于以 8 千赫兹采样率捕获的音频数据，请指定以下参数：<br/><br/>
      <code>audio/alaw;rate=8000</code>
    </td>
  </tr>
</table>

有关更多信息，请参阅 [en.wikipedia.org/wiki/A-law_algorithm ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/A-law_algorithm){: new_window}。

### audio/basic 格式
{: #basic}

*基本音频* (`audio/basic`) 是一种单声道有损音频格式，使用采样率为 8 千赫兹的 8 位 u-law（或 mu-law）数据进行编码。此格式提供了用于指示音频媒体类型的最基础格式。服务仅支持将 `audio/basic` 格式的文件用于窄带模型。

有关更多信息，请参阅因特网工程任务组织 (IETF) 的 [Request for Comments (RFC) 2046 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://tools.ietf.org/html/rfc2046){: new_window} 和 [iana.org/assignments/media-types/audio/basic ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://www.iana.org/assignments/media-types/audio/basic){: new_window}。

### audio/flac 格式
{: #flac}

*自由无损音频编码解码器 (FLAC)* (`audio/flac`) 是一种无损音频格式。有关更多信息，请参阅 [en.wikipedia.org/wiki/FLAC ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/FLAC){: new_window}。

### audio/g729 格式
{: #g729}

*G.729* (`audio/g729`) 是一种有损音频格式，支持以 8 千赫兹编码的数据。服务仅支持 G.729 附录 D，不支持附录 J。服务仅支持将 `audio/g729` 格式的文件用于窄带模型。有关更多信息，请参阅 [en.wikipedia.org/wiki/G.729 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/G.729){: new_window}。

### audio/l16 格式
{: #l16}

*线性 16 位脉冲编码调制 (PCM)* (`audio/l16`) 是一种未压缩的音频格式。使用此格式可传递原始 PCM 文件。线性 PCM 音频还可以在容器波形音频文件格式 (WAV) 文件内传送。使用 `audio/l16` 格式时，服务会接受有关格式规范的额外必需参数和可选参数。

<table>
  <caption>表 3. `audio/l16` 格式的参数</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      参数
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      描述
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>必需</em>
    </td>
    <td>
      一个整数，用于指定捕获音频的采样率。例如，对于以 16 千赫兹采样率捕获的音频数据，请指定以下参数：<br/><br/>
      <code>audio/l16;rate=16000</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>channels</code><br/><em>可选</em>
    </td>
    <td>
      缺省情况下，服务会将音频视为具有单个声道。<em>如果音频有多个声道</em>，必须指定一个整数来标识声道数。例如，对于以 16 千赫兹采样率捕获的两声道音频数据，请指定以下参数：<br/><br/>
      <code>audio/l16;rate=16000;channels=2</code><br/><br/>
      服务最多接受 16 个声道。在代码转换期间，会将音频缩混为一个声道。
    </td>
  </tr>
  <tr>
    <td>
      <code>endianness</code><br/><em>可选</em>
    </td>
    <td>
      缺省情况下，服务会自动检测传入音频的字节序。但是，服务的自动检测功能有时会失败，并断开 `audio/l16` 格式短音频的连接。指定 endianness 可禁用自动检测。请指定 <code>big-endian</code> 或 <code>little-endian</code>。例如，对于以 16 千赫兹采样率捕获的小尾数法格式的音频数据，请指定以下参数：<br/><br/>
      <code>audio/l16;rate=16000;endianness=little-endian</code><br/><br/>
      <a target="_blank" href="https://tools.ietf.org/html/rfc2045#section-5.1">Request for Comments (RFC) 2045 ![外部链接图标 ](../../icons/launch-glyph.svg "外部链接图标 ")</a> 的第 5.1 部分为 <code>audio/l16</code> 数据指定了大尾数法格式，但许多人会使用小尾数法格式。
    </td>
  </tr>
</table>

有关更多信息，请参阅 [Request for Comments (RFC) 2586 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://tools.ietf.org/html/rfc2586){: new_window} 和 [en.wikipedia.org/wiki/Pulse-code_modulation ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/Pulse-code_modulation){: new_window}。

### audio/mp3 和 audio/mpeg 格式
{: #mp3}

*MP3* (`audio/mp3`) 或*运动图像专家组 (MPEG)* (`audio/mpeg`) 是一种有损音频格式。（MP3 和 MPEG 指的是相同的格式。）有关更多信息，请参阅 [en.wikipedia.org/wiki/MP3 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/MP3){: new_window}。

### audio/mulaw 格式
{: #mulaw}

*Mu-law* (`audio/mulaw`) 是一种单声道有损音频格式。数据使用 u-law（或 mu-law）算法进行编码。`audio/basic` 格式相当于采样率始终为 8 千赫兹的格式。使用此格式时，服务需要有关格式规范的额外参数。

<table>
  <caption>表 4. `audio/mulaw` 格式的参数</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      参数
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      描述
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>必需</em>
    </td>
    <td>
      一个整数，用于指定捕获音频的采样率。例如，对于以 8 千赫兹采样率捕获的音频数据，请指定以下参数：<br/><br/>
      <code>audio/mulaw;rate=8000</code>
    </td>
  </tr>
</table>

有关更多信息，请参阅 [en.wikipedia.org/wiki/M-law_algorithm ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/M-law_algorithm){: new_window}。

### audio/ogg 格式
{: #ogg}

*Ogg* (`audio/ogg`) 是一种开放式容器格式，由 Xiph.org Foundation ([xiph.org/ogg ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.xiph.org/ogg){: new_window}) 进行维护。可以使用通过以下有损编码解码器压缩的音频流：

-   *Opus* (`audio/ogg;codecs=opus`)。有关更多信息，请参阅 [opus-codec.org ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.opus-codec.org/){: new_window} 和 [en.wikipedia.org/wiki/Opus (audio format) ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/Opus){: new_window}。在 *Opus (audio format)* 页面上，请特别查看 *Containers* 部分。
-   *Vorbis* (`audio/ogg;codecs=vorbis`)。有关更多信息，请参阅 [xiph.org/vorbis ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://xiph.org/vorbis/){: new_window} 和 [en.wikipedia.org/wiki/Vorbis ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/Vorbis){: new_window}。

如果省略编码解码器，那么服务会自动在输入音频中检测编码解码器。Opus 是首选的编码解码器；因特网工程任务组织 (IETF) 已将其标准化为 [Request for Comments (RFC) 6716 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://tools.ietf.org/html/rfc6716){: new_window}。

### audio/wav 格式
{: #wav}

*波形音频文件格式 (WAV)* (`audio/wav`) 是一种容器格式，通常用于未压缩的音频流，但也可以包含压缩音频。服务支持使用任何编码的 WAV 音频。服务接受最多有 9 个声道（由于 FFmpeg 限制）的 WAV 音频。

有关 WAV 格式的更多信息，请参阅 [en.wikipedia.org/wiki/WAV ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/WAV){: new_window}。有关通过将 WAV 音频转换为 Opus 编码解码器来减小 WAV 音频大小的更多信息，请参阅[使用 Opus 编码解码器转换为 audio/ogg](#conversionOgg)。

### audio/webm 格式
{: #webm}

*Web 媒体 (WebM)* (`audio/webm`) 是一种开放式容器格式，由 WebM 项目 [webmproject.org ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.webmproject.org/){: new_window}) 进行维护。可以使用通过以下有损编码解码器压缩的音频流：

-   *Opus* (`audio/webm;codecs=opus`)。有关更多信息，请参阅 [opus-codec.org ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.opus-codec.org/){: new_window} 和 [en.wikipedia.org/wiki/Opus (audio format) ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/Opus){: new_window}。在 *Opus (audio format)* 页面上，请特别查看 *Containers* 部分。
-   *Vorbis* (`audio/webm;codecs=vorbis`)。有关更多信息，请参阅 [xiph.org/vorbis ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://xiph.org/vorbis/){: new_window} 和 [en.wikipedia.org/wiki/Vorbis ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://en.wikipedia.org/wiki/Vorbis){: new_window}。

如果省略编码解码器，那么服务会自动在输入音频中检测编码解码器。

有关用于说明如何在 Chrome 浏览器中从麦克风捕获音频并将其编码为 WebM 数据流的 JavaScript 代码，请参阅 [jsbin.com/hedujihuqo/edit?js,console ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://jsbin.com/hedujihuqo/edit?js,console){: new_window}。该代码不会将捕获到的音频提交给服务。
{: tip}

## 数据限制和压缩
{: #limits}

通过一个同步 HTTP 或 WebSocket 请求，服务最多可以接受 100 MB 的音频数据进行转录。识别到连续的长音频流或大型文件时，请执行以下步骤以确保音频不超过 100 MB 数据限制：

-   使用不大于 16 千赫兹（宽带模型）或 8 千赫兹（窄带模型）的采样率，并且使用 16 比特/样本。服务会将以高于目标模型（16 千赫兹或 8 千赫兹）的采样率录制的音频转换为模型的采样率。因此，增大频率并不会提高识别准确性，但确实会增加音频流的大小。
-   通过提供数据压缩的格式对音频进行编码。通过更高效地对数据编码，可以在不超过 100 MB 限制的情况下发送多得多的音频。`audio/ogg` 和 `audio/mp3` 等音频格式可显著减小音频流的大小。您可以使用这些格式在单个请求中发送更大的音频量。

如果使用 `audio/ogg` 格式过度压缩音频，那么可能会损失一些识别准确性。为安全起见，请为音频保留至少 24 千位/秒的比特率。

### 比较近似的音频大小
{: #compareSizes}

考虑使用 16 千赫兹采样率和 16 比特/样本的 2 小时持续语音传输所生成数据流的近似大小：

-   如果数据的编码格式为 `audio/wav`，那么 2 小时的流的大小为 230 MB，远远超过服务的 100 MB 限制。
-   如果数据的编码格式为 `audio/ogg`，那么 2 小时的流的大小只有 23 MB，远远低于服务的限制。

下表近似估计了可以通过一个同步 HTTP 或 WebSocket 请求发送用于语音识别的不同格式音频的最长持续时间。持续时间考虑了 100 MB 服务限制。根据音频的复杂性和实现的压缩率，实际值可能会有所不同。

<table style="width:75%">
  <caption>表 5. 不同格式音频的最长持续时间</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 50%">
      音频格式
    </th>
    <th style="text-align:center; vertical-align:bottom; width 50%">
      音频最长持续时间（近似值）
    </th>
  </tr>
  <tr>
    <td><code>audio/wav</code></td>
    <td style="text-align:center">55 分钟</td>
  </tr>
  <tr>
    <td><code>audio/flac</code></td>
    <td style="text-align:center">1 小时 40 分钟</td>
  </tr>
  <tr>
    <td><code>audio/mp3</code></td>
    <td style="text-align:center">3 小时 20 分钟</td>
  </tr>
  <tr>
    <td><code>audio/ogg</code></td>
    <td style="text-align:center">8 小时 40 分钟</td>
  </tr>
</table>

`audio/ogg;codecs=opus` 和 `audio/webm;codecs=opus` 格式基本等效，而且其大小也几乎相同。这两种格式在内部使用相同的编码解码器；不同的只是容器格式。

## 音频转换
{: #conversion}

可以使用各种工具将音频转换为不同的格式。音频是服务不支持的格式，或者是未压缩或无损格式时，这些工具会非常有用。在后一种情况下，可以将音频转换为有损格式以减小其大小。

以下免费软件工具可用于将音频从一种格式转换为另一种格式：

-   Sound eXchange (SoX) ([sox.sourceforge.net ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://sox.sourceforge.net){: new_window})
-   FFmpeg ([ffmpeg.org ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ffmpeg.org){: new_window})
-   Audacity&reg; ([audacityteam.org ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://www.audacityteam.org/){: new_window})
-   对于使用 Opus 编码解码器的 Ogg 格式，请使用 **opus-tools** ([opus-codec.org/downloads/ ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://opus-codec.org/downloads/){: new_window})

这些工具为多种音频格式提供跨平台支持。此外，还可以使用许多工具来播放音频。请勿使用这些工具来违反适用的版权法。

### 转换为使用 Opus 编码解码器的 audio/ogg
{: #conversionOgg}

[**opus-tools** ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://opus-codec.org/downloads/){: new_window} 包含三个命令行实用程序，用于在 Opus 编码解码器中处理 Ogg 音频：

-   [**opusenc** ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusenc.html){: new_window} 实用程序用于将 WAV、FLAC 和其他格式的音频编码为使用 Opus 编码解码器的 Ogg。此页面说明了如何压缩音频流。压缩对于将实时音频传递到服务非常有用。
-   [**opusdec** ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusdec.html){: new_window} 实用程序用于将音频从 Opus 编码解码器解码为未压缩的 PCM WAV 文件。
-   [**opusinfo** ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusinfo.html){: new_window} 实用程序用于提供有关 Opus 文件的信息并对这些文件执行有效性检查。

许多用户会发送 WAV 文件进行语音识别。由于服务为同步 HTTP 和 WebSocket 请求设定了 100 MB 数据限制，WAV 格式会减少可以通过单个请求识别的音频量。使用 **opusenc** 命令将音频转换为首选的 `audio/ogg:codecs=opus` 格式后，会大大增加可以通过一个识别请求发送的音频量。

例如，假设未压缩宽带（16 千赫兹）WAV 文件 (**input.wav**) 使用 16 比特/样本，比特率为 256 千位/秒。以下命令可将该音频转换为使用 Opus 编码解码器的文件 (**output.opus**)：

```bash
opusenc input.wav output.opus
```
{: pre}

该转换使音频压缩为原来的 1/4，并生成比特率为 64 千位/秒的输出文件。但是，根据 [Opus Recommended Settings ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://wiki.xiph.org/Opus_Recommended_Settings){: new_window}，您可以安全地将比特率降低到 24 千位/秒，同时仍保留语音音频的完整频带。此比特率降低将使音频压缩为原来的 1/10。以下命令使用 `--bitrate` 选项来生成比特率为 24 千位/秒的输出文件：

```bash
opusenc --bitrate 24 input.wav output.opus
```
{: pre}

**opusenc** 实用程序的压缩速度很快。压缩速度约比实际时间快 100 倍。此命令完成后，会将输出写入控制台，其中提供有关命令运行时间和生成的音频数据的完整详细信息。

## 关于改进语音识别的提示
{: #audioTips}

以下提示可以帮助您改进语音识别的质量：

-   音频录制方式对服务的结果会有很大的影响。语音识别对输入音频质量非常敏感。为了获得尽可能最高的准确性，请确保输入音频质量尽量高。
    -   尽量使用近讲语音型麦克风（例如，头戴式受话器），并根据需要调整麦克风设置。使用专业麦克风来捕获音频时，服务表现最佳。
    -   避免使用系统的内置麦克风。移动设备和平板电脑上通常安装的麦克风往往不够好。
    -   确保说话者靠近麦克风。说话者离麦克风越远，准确性越低。例如，如果离麦克风 10 英尺，服务很难生成足够好的结果。
-   语音识别对背景噪声和人声的细微差别非常敏感。
    -   发动机噪声、运转的设备、街道噪声和背景对话都会大幅降低识别准确性。
    -   地方口音和发音差异也会降低准确性。

    如果音频具有上述这些特征，请考虑使用声学模型定制来提高语音识别的准确性。有关更多信息，请参阅[定制接口](/docs/services/speech-to-text/custom.html)。
