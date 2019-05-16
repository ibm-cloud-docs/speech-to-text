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

# 音訊格式
{: #audio-formats}

{{site.data.keyword.speechtotextfull}} 服務可以使用許多不同的格式來從音訊中擷取語音。

-   如果您不熟悉音訊及其說明和指定方式，請從[音訊特徵和術語](#terminology)開始以協助您入門。
-   如果您已瞭解如何使用音訊，請跳至[支援的音訊格式](#formats)，以取得服務支援之格式的詳細資訊。

最後幾節為[資料限制和壓縮](#limits)、[音訊轉換](#conversion)和[改善語音辨識的秘訣](#audioTips)，它們可協助您善用此服務。

## 音訊特徵和術語
{: #terminology}

下列術語用來說明不同格式的音訊資料的特徵。

### 取樣率
{: #samplingRate}

*取樣率*（或取樣頻率）是指每秒取得的音訊樣本數目。取樣率的測量單位是赫茲 (Hz) 或千赫 ( kHz)。例如，每秒 16,000 個樣本的速率等於 16,000 赫玆（或 16 kHz）。使用 {{site.data.keyword.speechtotextshort}} 服務，您可以指定模型來指出音訊的取樣率：

-   *寬頻* 模型用於取樣率不小於 16 kHz 的音訊，{{site.data.keyword.IBM}} 建議用於具回應力的即時應用程式（例如，用於即時語音應用程式）。
-   *窄頻* 模型用於取樣率不小於 8 kHz 的音訊，此速率通常用於電話音訊。

此服務支援大部分語言及格式的寬頻和窄版音訊。它在辨識語音之前會先自動調整您音訊的取樣率，以符合您指定的模型。

-   若為寬頻模型，此服務會將以較高取樣率錄製的音訊轉換為 16 kHz。
-   若為窄頻模型，它會將音訊轉換為 8 kHz。

理論上，您可以使用寬頻或窄頻模型來傳送 44 kHz 音訊，但這會不必要地增加音訊的大小。若要將您可以傳送的音訊量最大化，請讓音訊的取樣率與您使用的模型相符。此服務不接受其取樣率小於模型本質取樣率的音訊。

#### 音訊格式的注意事項
{: #samplingRateNotes}

-   對於 `audio/alaw`、`audio/l16` 及 `audio/mulaw` 格式，您必須指定音訊的速率。
-   對於 `audio/basic` 和 `audio/g729` 格式，此服務只支援窄頻音訊。

#### 相關資訊
{: #samplingRateMore}

-   如需取樣率的相關資訊，請參閱 [en.wikipedia.org/wiki/Sampling ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/Sampling){: new_window}。選取*取樣（信號處理）*。
-   如需此服務針對每種支援的語言所提供之模型的相關資訊，請參閱[語言和模型](/docs/services/speech-to-text/models.html)。

### 位元速率
{: #bitRate}

*位元速率* 是指每秒傳送的資料位元數。音訊串流的位元速率是以每秒千位元 (kbps) 為單位測量。以取樣率和每個樣本儲存的位元數來計算位元速率。若為語音辨識，{{site.data.keyword.IBM}} 建議您為音訊錄製每個樣本 16 位元。

例如，若音訊使用寬頻取樣率 16 kHz 和每個樣本 16 位元，則其位元速率為 256 kbps：`(16,000 * 16) / 1000`。

#### 相關資訊
{: #bitRateMore}

-   如需位元速率的相關資訊，請參閱 [en.wikipedia.org/wiki/Bit_rate ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/Bit_rate){: new_window}。
-   如需取樣率和位元速率的一般討論，請參閱[何謂位元速率？![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://www.richardfarrar.com/what-are-bit-rates/){: new_window} 和[選擇播客的位元速率 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://www.richardfarrar.com/choosing-bit-rates-for-podcasts/){: new_window}。

### 壓縮
{: #compression}

許多音訊格式都使用*壓縮* 來減少音訊資料的大小。壓縮可減少每個樣本儲存的位元數，進而降低位元速率。部分格式不使用壓縮，但大部分使用下列其中一種基本壓縮類型：

-   *無損* 壓縮可減少音訊大小而不失去品質，但壓縮比例通常很小。
-   *有損* 壓縮可將音訊大小縮小到 10 倍之多，但在壓縮時會失去部分資料和某些品質而無法恢復。

使用 {{site.data.keyword.speechtotextshort}} 服務，您可以放心地使用有損壓縮，將您可以透過辨識要求而傳送給服務的音訊量最大化。因為人聲的動態範圍比較受限，譬如跟音樂相比，所以語音可以容納的位元速率遠比其他音訊類型還要低。對於語音辨識，{{site.data.keyword.IBM}} 建議您使用每個樣本 16 位元的音訊，並採用壓縮音訊資料的格式。

#### 音訊格式的注意事項
{: #compressionNotes}

-   `audio/ogg` 和 `audio/webm` 格式是容器，其壓縮根據您用來編碼資料的轉碼器：Opus 或 Vorbis。
-   `audio/wav` 格式是容器，可包含未經壓縮的資料、無損資料或有損資料。

#### 相關資訊
{: #compressionMore}

-   如需音訊壓縮的相關資訊，請參閱 [en.wikipedia.org/wiki/Data_compression#Audio ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/Data_compression#Audio){: new_window}。
-   如需使用資料壓縮來增加您可以透過要求傳送之音訊量的相關資訊，請參閱[資料限制和壓縮](#limits)。

### 頻道
{: #channels}

*頻道* 指出所錄製音訊的串流數：

-   *單聲道*（或 mono）音訊僅有單一頻道。
-   *立體聲*（或 stereo）音訊一般有兩個頻道。

{{site.data.keyword.speechtotextshort}} 服務接受最多 16 個頻道的音訊。因為它只使用單一頻道進行語音辨識，因此在轉碼期間，此服務會將具有多個頻道的音訊降混成一個頻道 mono。

#### 音訊格式的注意事項
{: #channelsNotes}

-   對於 `audio/l16` 格式，如果您的音訊具有多個頻道，則必須指定頻道數。
-   對於 `audio/wav` 格式，此服務接受最多有九個頻道的音訊。

#### 相關資訊
{: #channelsMore}

-   如需音訊頻道的相關資訊，請參閱 [en.wikipedia.org/wiki/Audio_signal ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/Audio_signal){: new_window}。

### 排列法
{: #endianness}

*排列法* 指出基礎電腦架構組織資料位元組的方式：

-   *大序排列法* 依最高有效位元來排序資料。
-   *小序排列法* 依最低有效位元來排序資料。

{{site.data.keyword.speechtotextshort}} 服務會自動偵測送入音訊的排列法。

#### 音訊格式的注意事項
{: #endiannessNotes}

-   對於 `audio/l16` 格式，您可以指定排列法來停用自動偵測（必要的話）。

#### 相關資訊
{: #endiannessMore}

-   如需排列法的相關資訊，請參閱 [en.wikipedia.org/wiki/Endianness ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/Endianness){: new_window}。

### 音訊頻率
{: #frequency}

*音訊頻率* 是指音訊中的有聲頻率範圍。通常可以接受的人類標準有聲頻率為 20 到 20,000 赫玆。您可以使用頻譜分析來產生頻譜，以顯示您音訊的頻率內容。

套用至音訊的取樣率通常是音訊的頻率上限的兩倍。例如，取樣率 16 kHz 表示取樣音訊信號的頻率上限是 8 kHz。此服務的模型是在此考量之下建立。

-   窄頻模型是以 8 kHz 取樣率的音訊來建置的。窄頻模型預期在小於或等於 4 kHz 範圍內找到資訊。
-   寬頻模型是以 16 kHz 取樣率的音訊來建置的。寬頻模型預期在 4 到 8 kHz 範圍內找到資訊。

模型的訓練資料衍生自不同的頻道（適用於窄頻模型的電話系統）。這些模型反映其受訓練之頻道的特徵。

#### 升頻取樣
{: #upsampling}

*升頻取樣* 會增加音訊的取樣率，但不會在音訊中引進新資訊。它會產生近似以較高比率取樣音訊所獲得的音訊信號。它會增加音訊資料的大小。

原本以窄頻頻率取樣的音訊資訊受限在 0-4 kHz 範圍內。以升頻取樣將將窄頻音訊升至更高的取樣率不太可能改善語音辨識正確性。如果您對窄頻音訊進行升頻取樣，它將缺少在寬頻模型預期範圍內的資訊。此外，在窄頻樣本的預期範圍內找到的資訊，與在寬頻樣本的相同範圍內找到的資訊，有本質上的不同。因此，升頻取樣實際上會導致辨識正確性欠佳。

對於寬頻取樣率 16 kHz，在取樣的音訊信號中出現的頻率上限預期為 8 kHz。因此，您必須在以 16 kHz 的比率取樣之前，先過濾 8 kHz 的原始信號。否則，會因為所謂的*別名化* 現象而發生退化。若要瞭解原因，請參閱 [Nyquist 頻率 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/Nyquist_frequency){: new_window}。

也許想像在大型平面電視 HDTV 上觀看 VHS 錄影帶會是比較有用的比較。影像會模糊，這是因為在高清裝置上播放錄影帶無法實際將新資訊新增至串流。它只是讓格式與更好的裝置相容。升頻取樣音訊也是如此。

#### 降頻取樣
{: #downsampling}

*降頻取樣* 會降低音訊的取樣率。它會產生近似以較低比率取樣音訊所獲得的音訊信號。降頻取樣不會從音訊信號中移除任何資訊，但會減少音訊資料的大小。

在某些情況下，對音訊進行降頻取樣可能比較實際。例如，如果音訊的取樣率大於 8 kHz，*且* 頻譜檢查顯示並沒有頻率內容大於 4 kHz，請考慮將音訊降頻取樣至 8 kHz。

#### 相關資訊
{: #frequencyMore}

-   如需音訊頻率的相關資訊，請參閱 [https://en.wikipedia.org/wiki/Audio_frequency ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/Audio_frequency){: new_window}。
-   如需升頻取樣的相關資訊，請參閱 [https://en.wikipedia.org/wiki/Upsampling ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/Upsampling){: new_window}。
-   如需降頻取樣的相關資訊，請參閱 [https://en.wikipedia.org/wiki/Downsampling ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/Downsampling_%28signal_processing%29){: new_window}。

## 支援的音訊格式
{: #formats}

表 1 提供該服務支援的音訊格式摘要。

-   *音訊格式和壓縮* 可識別每種格式，並指出其支援的壓縮。您可以使用單一同步 HTTP 或 WebSocket 要求，將最多 100 MB 的音訊傳送至服務。使用支援壓縮的格式，您可以減少音訊的大小，讓您可以傳遞給服務的資料量達到最大。如需相關資訊，請參閱[資料限制和壓縮](#limits)。
-   *內容類型規格* 指出您是否必須使用 `Content-Type` 標頭或對等參數，來指定您傳送給服務之音訊的格式（MIME 類型）。您可以指定任何要求的音訊格式，但不一定需要：
    -   對於大部分格式，內容類型是選用的。您可以省略內容類型，或指定 `application/octet-stream`，讓服務自動偵測格式。
    -   對於其他格式，內容類型是必要的。這些格式不會提供服務自動偵測其格式所需的資訊（例如取樣率）。
-   對於每種格式，最終直欄會識別其他*必要的參數* 和*選用參數*。下列各節提供這些參數的相關資訊。

<table>
  <caption>表 1. 支援的音訊格式摘要</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom">
      音訊格式<br>和壓縮
    </th>
    <th style="text-align:center; vertical-align:bottom">
      內容類型<br/>規格
    </th>
    <th style="text-align:center; vertical-align:bottom">
      必要的<br/>參數
    </th>
    <th style="text-align:center; vertical-align:bottom">
      選用<br/>參數
    </th>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/alaw](#alaw)<br/>有損
    </td>
    <td style="text-align:center">
      必要
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      無
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/basic](#basic)<br/>有損
    </td>
    <td style="text-align:center">
      必要
    </td>
    <td style="text-align:center">
      無
    </td>
    <td style="text-align:center">
      無
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/flac](#flac)<br/>無損
    </td>
    <td style="text-align:center">
      選用
    </td>
    <td style="text-align:center">
      無
    </td>
    <td style="text-align:center">
      無
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/g729](#g729)<br/>有損
    </td>
    <td style="text-align:center">
      選用
    </td>
    <td style="text-align:center">
      無
    </td>
    <td style="text-align:center">
      無
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/l16](#l16)<br/>無
    </td>
    <td style="text-align:center">
      必要
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
      [audio/mpeg](#mp3)<br/>有損
    </td>
    <td style="text-align:center">
      選用
    </td>
    <td style="text-align:center">
      無
    </td>
    <td style="text-align:center">
      無
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/mulaw](#mulaw)<br/>有損
    </td>
    <td style="text-align:center">
      必要
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      無
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/ogg](#ogg)<br/>有損
    </td>
    <td style="text-align:center">
      選用
    </td>
    <td style="text-align:center">
      無
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/wav](#wav)<br/>無、無損<br/>或有損
    </td>
    <td style="text-align:center">
      選用
    </td>
    <td style="text-align:center">
      無
    </td>
    <td style="text-align:center">
      無
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/webm](#webm)<br/>有損
    </td>
    <td style="text-align:center">
      選用
    </td>
    <td style="text-align:center">
      無
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
</table>

當您使用 `curl` 指令，搭配 HTTP 介面來提出語音辨識要求時，您必須使用 `Content-Type` 標頭來指定音訊格式，指定 `"Content-Type: application/octet-stream"`，或指定 `"Content-Type:"`。如果您完全省略標頭，`curl` 會使用預設值 `application/x-www-form-urlencoded`。
{: important}

### audio/alaw 格式
{: #alaw}

*A-law* (`audio/alaw`) 是單一頻道有損音訊格式。它使用的演算法類似 `audio/basic` 和 `audio/mulaw` 格式所套用的 u-law 演算法，不過 A-law 演算法會產生不同的信號特徵。當您使用此格式時，該服務需要格式規格有額外參數。

<table>
  <caption>表 2. `audio/alaw` 格式的參數</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      參數
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      說明
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>必要</em>
    </td>
    <td>
      一個整數，用來指定擷取音訊的取樣率。
      例如，對於以 8 kHz 擷取之音訊資料指定下列參數：<br/><br/>
      <code>audio/alaw;rate=8000</code>
    </td>
  </tr>
</table>

如需相關資訊，請參閱 [en.wikipedia.org/wiki/A-law_algorithm ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/A-law_algorithm){: new_window}。

### audio/basic 格式
{: #basic}

*基本音訊* (`audio/basic`) 是使用 8 位元 u-law（或 mu-law）資料進行編碼的單一頻道有損音訊格式，該資料是以 8 kHz 取樣。此格式提供最小公分母，以指出音訊的媒體類型。此服務只支援對窄頻模型使用 `audio/basic` 格式的檔案。

如需相關資訊，請參閱「網際網路工程任務小組 (IETF)」的 [Request for Comment (RFC) 2046 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://tools.ietf.org/html/rfc2046){: new_window} 和 [iana.org/assignments/media-types/audio/basic ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://www.iana.org/assignments/media-types/audio/basic){: new_window}。

### audio/flac 格式
{: #flac}

*自由無損音訊轉碼器 (FLAC)* (`audio/flac`) 為無損音訊格式。如需相關資訊，請參閱 [en.wikipedia.org/wiki/FLAC ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/FLAC){: new_window}。

### audio/g729 格式
{: #g729}

*G.729* (`audio/g729`) 是支援以 8 kHz 編碼之資料的有損音訊格式。此服務只支援 G.729 附錄 D，而不支援附錄 J。此服務只支援對窄頻模型使用 `audio/g729` 格式的檔案。如需相關資訊，請參閱 [en.wikipedia.org/wiki/G.729 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/G.729){: new_window}。

### audio/l16 格式
{: #l16}

*線性 16 位元脈衝編碼調變 (PCM)* (`audio/l16`) 是一種未經壓縮的音訊格式。使用此格式可傳遞原始 PCM 檔。也可以在容器 Waveform Audio File Format (WAV) 檔案內包含線性 PCM 音訊。當您使用 `audio/l16` 格式時，該服務會接受格式規格上的額外必要參數及選用參數。

<table>
  <caption>表格 3. `audio/l16` 格式的參數</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      參數
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      說明
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>必要</em>
    </td>
    <td>
      一個整數，用來指定擷取音訊的取樣率。
      例如，對於以 16 kHz 擷取之音訊資料指定下列參數：<br/><br/>
      <code>audio/l16;rate=16000</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>channels</code><br/><em>選用</em>
    </td>
    <td>
      依預設，此服務會將音訊當成具有單一頻道來處理。
      <em>如果音訊有多個頻道，</em>您必須指定一個整數來識別頻道數。例如，對於以 16 kHz 擷取之雙頻道音訊資料指定下列參數：<br/><br/>
      <code>audio/l16;rate=16000;channels=2</code><br/><br/>
      此服務最多接受 16 個頻道。
    在轉碼期間，它會將音訊降混成一個頻道。</td>
  </tr>
  <tr>
    <td>
      <code>endianness</code><br/><em>選用</em>
    </td>
    <td>
      依預設，此服務會自動偵測送入音訊的排列法。然而，其自動偵測有時可能會失敗，而會捨棄 `audio/l16` 格式的簡短音訊的連線。指定排列法會停用自動偵測。請指定 <code>big-endian</code> 或 <code>little-endian</code>。例如，對於以 16 kHz 擷取之音訊資料，以小序排列法格式指定下列參數：<br/><br/><code>audio/l16;rate=16000;endianness=little-endian</code><br/><br/>
      5.1 小節的
      <a target="_blank" href="https://tools.ietf.org/html/rfc2045#section-5.1">Request for Comment (RFC) 2045 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")</a> 對 <code>audio/l16</code> 資料指定大序排列法格式，但許多人使用小序排列法格式。
    </td>
  </tr>
</table>

如需相關資訊，請參閱 IETF 的 [Request for Comment (RFC) 2586 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://tools.ietf.org/html/rfc2586){: new_window} 和 [en.wikipedia.org/wiki/Pulse-code_modulation ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/Pulse-code_modulation){: new_window}。

### audio/mp3 及 audio/mpeg 格式
{: #mp3}

*MP3* (`audio/mp3`) 或 *Motion Picture Experts Group (MPEG)* (`audio/mpeg`) 是有損音訊格式。（MP3 和 MPEG 指的是相同的格式。）如需相關資訊，請參閱 [en.wikipedia.org/wiki/MP3 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/MP3){: new_window}。

### audio/mulaw 格式
{: #mulaw}

*Mu-law* (`audio/mulaw`) 是單一頻道有損音訊格式。資料是使用 u-law（或 m-law）演算法進行編碼。`audio/basic` 格式是一律以 8 kHz 取樣的相等格式。當您使用此格式時，該服務需要格式規格有額外參數。

<table>
  <caption>表 4. `audio/mulaw` 格式的參數</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      參數
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      說明
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>必要</em>
    </td>
    <td>
      一個整數，用來指定擷取音訊的取樣率。
      例如，對於以 8 kHz 擷取之音訊資料指定下列參數：<br/><br/>
      <code>audio/mulaw;rate=8000</code>
    </td>
  </tr>
</table>

如需相關資訊，請參閱 [en.wikipedia.org/wiki/M-law_algorithm ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/M-law_algorithm){: new_window}。

### audio/ogg 格式
{: #ogg}

*Ogg* (`audio/ogg`) 是由 Xiph.org Foundation（[xiph.org/ogg ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.xiph.org/ogg){: new_window}）維護的開放式容器格式。您可以使用以下列有損轉碼器壓縮的音訊串流：

-   *Opus* (`audio/ogg;codecs=opus`)。如需相關資訊，請參閱 [opus-codec.org ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.opus-codec.org/){: new_window} 及 [en.wikipedia.org/wiki/Opus（音訊格式）![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/Opus){: new_window}。
          在 *Opus（音訊格式）* 頁面上，請特別查看*容器* 區段。
        
-   *Vorbis* (`audio/ogg;codecs=vorbis`)。如需相關資訊，請參閱 [xiph.org/vorbis ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://xiph.org/vorbis/){: new_window} 及 [en.wikipedia.org/wiki/Vorbis ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/Vorbis){: new_window}。
        

如果您省略轉碼器，服務會從輸入音訊自動偵測它。Opus 是偏好的轉碼器；它由「網際網路工程任務小組 (IETF)」標準化，請參閱 [Request for Comment (RFC) 6716 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://tools.ietf.org/html/rfc6716){: new_window}。

### audio/wav 格式
{: #wav}

*Waveform Audio File Format (WAV)* (`audio/wav`) 是一種容器格式，通常用於未經壓縮的音訊串流，但它也可以包含壓縮音訊。服務支援使用任何編碼的 WAV 音訊。它接受最多有九個頻道的 WAV 音訊（因為 FFmpeg 限制）。

如需 WAV 格式的相關資訊，請參閱 [en.wikipedia.org/wiki/WAV ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/WAV){: new_window}。如需透過將 WAV 音訊轉換為 Opus 轉碼器來減少 WAV 音訊大小的相關資訊，請參閱[使用 Opus 轉碼器轉換為 audio/ogg](#conversionOgg)。

### audio/webm 格式
{: #webm}

*Web 媒體 (WebM)* (`audio/webm`) 是由 WebM 專案（[webmproject.org ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.webmproject.org/){: new_window}）維護的開放式容器格式。您可以使用以下列有損轉碼器壓縮的音訊串流：

-   *Opus* (`audio/webm;codecs=opus`)。如需相關資訊，請參閱 [opus-codec.org ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.opus-codec.org/){: new_window} 及 [en.wikipedia.org/wiki/Opus（音訊格式）![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/Opus){: new_window}。
          在 *Opus（音訊格式）* 頁面上，請特別查看*容器* 區段。
        
-   *Vorbis* (`audio/webm;codecs=vorbis`)。如需相關資訊，請參閱 [xiph.org/vorbis ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://xiph.org/vorbis/){: new_window} 及 [en.wikipedia.org/wiki/Vorbis ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://en.wikipedia.org/wiki/Vorbis){: new_window}。
        

如果您省略轉碼器，服務會從輸入音訊自動偵測它。

如需顯示如何在 Chrome 瀏覽器中從麥克風擷取音訊並將其編碼為 WebM 資料串流的 JavaScript 程式碼，請參閱 [jsbin.com/hedujihuqo/edit?js,console ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://jsbin.com/hedujihuqo/edit?js,console){: new_window}。此程式碼不會將擷取的音訊提交給服務。
{: tip}

## 資料限制和壓縮
{: #limits}

使用同步 HTTP 或 WebSocket 要求，此服務最多可接受 100 MB 的音訊資料進行轉錄。當您看到冗長的連續音訊串流或大型檔案時，請採取下列步驟，以確保音訊不超過 100 MB 的資料限制：

-   使用不大於 16 kHz（用於寬頻模型）或 8 kHz（用於窄頻模型）的取樣率，且每個樣本使用 16 位元。該服務會將以高於目標模型（16 kHz 或 8 kHz）的取樣率所錄製的音訊轉換為模型的速率。因此，較大的頻率雖不會導致更高的辨識正確性，但會增加音訊串流的大小。
-   以提供資料壓縮的格式將音訊進行編碼。藉由更有效率地編碼資料，您可以傳送更多音訊，而不超出 100 MB 限制。`audio/ogg` 及 `audio/mp3` 這類音訊格式，會大幅減少音訊串流的大小。您可以使用這些格式，使用單一要求來傳送更多的音訊量。

如果您使用 `audio/ogg` 格式大幅壓縮音訊，可能會失去一些辨識正確性。為了安全起見，請為音訊保留至少 24 kbps 的位元速率。

### 比較近似的音訊大小
{: #compareSizes}

請考量 2 小時連續語音傳輸結果的資料串流的估計大小，其是以 16 kHz 進行取樣，每個樣本為 16 位元：

-   如果資料是以 `audio/wav` 格式編碼，則兩小時串流的大小為 230 MB，遠超出此服務的 100 MB 限制。
-   如果資料是以 `audio/ogg` 格式編碼，則兩小時串流的大小只有 23 MB，遠低於此服務的限制。

下表估計可使用不同格式的同步 HTTP 或 WebSocket 要求來傳送用於語音辨識之音訊的持續時間上限。持續時間會考量 100 MB 服務限制。實際值會隨著音訊的複雜性及已達到的壓縮率而改變。

<table style="width:75%">
  <caption>表 5. 不同格式之音訊的持續時間上限</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 50%">
      音訊格式
    </th>
    <th style="text-align:center; vertical-align:bottom; width 50%">
      音訊的持續時間上限（大約）
    </th>
  </tr>
  <tr>
    <td><code>audio/wav</code></td>
    <td style="text-align:center">55 分鐘</td>
  </tr>
  <tr>
    <td><code>audio/flac</code></td>
    <td style="text-align:center">1 小時 40 分鐘</td>
  </tr>
  <tr>
    <td><code>audio/mp3</code></td>
    <td style="text-align:center">3 小時 20 分鐘</td>
  </tr>
  <tr>
    <td><code>audio/ogg</code></td>
    <td style="text-align:center">8 小時 40 分鐘</td>
  </tr>
</table>

`audio/ogg;codecs=opus` 及 `audio/webm;codecs=opus` 通常為相等的格式，且其大小幾乎相同。它們在內部使用相同的轉碼器；僅容器格式不同。

## 音訊轉換
{: #conversion}

您可以使用不同工具將音訊轉換成不同格式。當您的音訊格式不受服務支援，或者是未經壓縮的或無損的格式時，這些工具會很有幫助。在後者的情況下，您可以將音訊轉換為有損格式，以減少其大小。

可以使用下列免費工具，將您的音訊從某種格式轉換成另一種格式：

-   Sound eXchange (SoX) ([sox.sourceforge.net ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://sox.sourceforge.net){: new_window})
-   FFmpeg ([ffmpeg.org ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ffmpeg.org){: new_window})
-   Audacity&reg; ([audacityteam.org ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://www.audacityteam.org/){: new_window})
-   若為使用 Opus 轉碼器的 Ogg 格式，則為 **opus-tools** ([opus-codec.org/downloads/![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://opus-codec.org/downloads/){: new_window})

這些工具提供多種音訊格式的跨平台支援。此外，您還可以使用許多工具來播放音訊。請不要使用這些工具來違反適用的著作權法。

### 使用 Opus 轉碼器轉換為 audio/ogg
{: #conversionOgg}

[**opus-tools** ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://opus-codec.org/downloads/){: new_window} 包含三個指令行公用程式，可在 Opus 轉碼器中使用 Ogg 音訊：

-   [**opusenc** ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusenc.html){: new_window} 公用程式會將 WAV、FLAC 和其他格式的音訊編碼為使用 Opus 轉碼器的 Ogg。該頁面顯示如何壓縮音訊串流。壓縮對於將即時音訊傳遞至服務非常實用。
-   [**opusdec** ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusdec.html){: new_window} 公用程式將 Opus 轉碼器的音訊解碼為未經壓縮的 PCM WAV 檔。
-   [**opusinfo** ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusinfo.html){: new_window} 公用程式提供 Opus 檔案的相關資訊及驗證檢查。

許多使用者會傳送 WAV 檔案以進行語音辨識。使用該服務對於同步 HTTP 和 WebSocket 要求的 100 MB 資料限制，WAV 格式可減少單一要求能辨識的音訊量。使用 **opusenc** 指令，將音訊轉換成偏好的 `audio/ogg:codecs=opus` 格式，可大幅增加可透過辨識要求而傳送的音訊量。

例如，假設有一個未經壓縮的寬頻 (16 kHz) WAV 檔案 (**input.wav**)，其使用每個樣本 16 個位元，且位元速率為 256 kbps。下列指令會將音訊轉換為使用 Opus 轉碼器的檔案 (**output.opus**)：

```bash
opusenc input.wav output.opus
```
{: pre}

此轉換會以四倍壓縮音訊，並產生位元速率為 64 kbps 的輸出檔。不過，根據 [Opus 建議設定 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://wiki.xiph.org/Opus_Recommended_Settings){: new_window}，您可以放心地將位元速率減至 24 kbps，仍會保留語音音訊的全頻。此減少會以十倍壓縮音訊。下列指令使用 `--bitrate` 選項來產生位元速率為 24 kbps 的輸出檔：

```bash
opusenc --bitrate 24 input.wav output.opus
```
{: pre}

使用 **opusenc** 公用程式壓縮很快速。壓縮的執行速率大約比即時快 100 倍。當它完成時，該指令會將輸出寫入至主控台，提供關於其執行時間及產生的音訊資料的完整詳細資料。

## 改善語音辨識的秘訣
{: #audioTips}

下列秘訣可協助您改善語音辨識的品質：

-   錄製音訊的方式可能會對服務的結果產生重大影響。語音辨識對於輸入音訊品質非常敏感。為了獲得最佳可能正確性，請確保輸入音訊品質儘可能良好。
    -   儘可能使用靠近的語音導向麥克風（例如耳機），並在必要時調整麥克風設定。使用專業麥克風來擷取音訊時，該服務表現最好。
    -   避免使用系統的內建麥克風。通常安裝在行動裝置和平板電腦上的麥克風是不夠好的。
    -   確保說話者接近麥克風。當說話者離麥克風越遠時，精確度會下降。例如，若距離為 10 英尺，該服務要產生足夠的結果會很吃力。
-   語音辨識對於背景噪音和人聲的細微差別很敏感。
    -   引擎噪音、運作中裝置、馬路噪音和背景對話都會大幅降低辨識正確性。
    -   地區口音和發音差異也會降低正確性。

    如果您的音訊有這些特徵，請考慮使用聲學模型自訂作業來改善語音辨識的正確性。如需相關資訊，請參閱[自訂作業介面](/docs/services/speech-to-text/custom.html)。
