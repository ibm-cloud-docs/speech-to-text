---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-06"

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

# 音声フォーマット
{: #audio-formats}

{{site.data.keyword.speechtotextfull}} サービスは、さまざまなフォーマットの音声から発話を抽出できます。

-   音声と音声の記述および指定方法に詳しくない場合は、最初に[音声の特性と音声に関する用語](#terminology)をお読みください。
-   音声の使用方法をすでに理解している場合は、[サポートされている音声フォーマット](#formats)に進み、サービスでサポートされているフォーマットの詳細をお読みください。

最後のセクションである[データ制限および圧縮](#limits)、[音声の変換](#conversion)、および[音声認識の改善に関するヒント](#audioTips)は、サービスを最大限に活用する上で役立ちます。

## 音声の特性と音声に関する用語
{: #terminology}

さまざまなフォーマットの音声データの特性を説明する際に使われる用語を以下に示します。

### サンプリング・レート
{: #samplingRate}

*サンプリング・レート* (サンプリング頻度) は、1 秒あたりの取得音声サンプル数です。 サンプリング・レートはヘルツ (Hz) またはキロヘルツ (kHz) で測定されます。 例えば 1 秒あたり 16,000 サンプルのレートは 16,000 Hz (または 16 kHz) に相当します。 {{site.data.keyword.speechtotextshort}} サービスでは、モデルを指定して音声のサンプリング・レートを示します。

-   *広帯域*モデルは、サンプリング・レートが 16 KHz 以上の音声に使用します。{{site.data.keyword.IBM}} では、応答性が高いリアルタイム・アプリケーション (例えば、ライブ音声アプリケーション) では広帯域モデルを使用することをお勧めします。
-   *狭帯域*モデルは、サンプリング・レートが 8 KHz 以上の音声に使用します。 8 kHz は通常電話の音声に使用されるレートです。

このサービスではほとんどの言語とフォーマットで広帯域音声と狭帯域音声がサポートされています。 サービスで発話が認識される前に、音声のサンプリング・レートが、指定のモデルと一致するように自動的に調整されます。

-   広帯域モデルでは、16 kHz よりも高いサンプリング・レートで録音された音声が 16 kHz に変換されます。
-   狭帯域モデルでは、音声が 8 kHz に変換されます。

理論上、広帯域モデルでも狭帯域モデルでも 44 kHz の音声を送信できますが、このレートでは、音声のサイズが不必要に大きくなります。 送信できる音声の量を最大化させるためには、音声のサンプリング・レートを使用するモデルに一致させます。 サービスでは、モデル固有のサンプリング・レートよりサンプリング・レートが低い音声は受け入れません。

#### 音声フォーマットについての注記
{: #samplingRateNotes}

-   `audio/alaw`、`audio/l16`、および `audio/mulaw` フォーマットの場合、音声のレートを指定する必要があります。
-   `audio/basic` および `audio/g729` フォーマットの場合、サービスでは狭帯域音声のみがサポートされています。

#### 詳細情報
{: #samplingRateMore}

-   サンプリング・レートについて詳しくは、[Sampling (signal processing)](https://wikipedia.org/wiki/Sampling_%28signal_processing%29){: external} を参照してください。
-   サポートされている各言語についてサービスが提供するモデルについて詳しくは、[言語とモデル](/docs/services/speech-to-text?topic=speech-to-text-models)を参照してください。

### ビット・レート
{: #bitRate}

*ビット・レート*とは、1 秒あたりに送信されるデータのビット数です。 音声ストリームのビット・レートは K ビット/秒 (Kbps) で測定されます。 ビット・レートはサンプリング・レートと 1 サンプル当たりの格納ビット数から算出されます。 {{site.data.keyword.IBM}} では、音声認識の場合は音声を 1 サンプル当たり 16 ビットで録音することを推奨します。

例えば、広帯域サンプリング・レート 16 kHz と 1 サンプル当たり 16 ビットを使用する音声のビット・レートは 256 kbps になります (`(16,000 * 16) / 1000`)。

#### 詳細情報
{: #bitRateMore}

-   ビット・レートについて詳しくは、[Bit rate](https://wikipedia.org/wiki/Bit_rate){: external} を参照してください。
-   サンプリング・レートとビット・レートの一般的な説明については、[What are bit rates?](http://www.richardfarrar.com/what-are-bit-rates/){: external} および [Choosing bit rates for podcasts](http://www.richardfarrar.com/choosing-bit-rates-for-podcasts/){: external} を参照してください。

### 圧縮
{: #compression}

*圧縮*は、音声データのサイズを縮小する目的で多くの音声フォーマットで使用されています。 圧縮により、サンプル当たりの格納ビット数 が縮小され、その結果ビット・レートも縮小されます。 一部のフォーマットは圧縮を使用していませんが、ほとんどのフォーマットではいずれかの基本的な圧縮タイプが使用されています。

-   *ロスレス*圧縮では、品質を損なうことなく音声のサイズを縮小しますが、通常、圧縮率は小さくなります。
-   *非可逆*圧縮では、音声のサイズを最大 1/10 まで圧縮しますが、圧縮時に一部のデータと品質が損なわれ、それは修復できません。

{{site.data.keyword.speechtotextshort}} サービスでは、認識要求でサービスに送信できる音声の量を最大化するために、非可逆圧縮を問題なく使用できます。 人間の声のダイナミック・レンジは音楽などよりも限定的であるため、発話の場合、他のタイプの音声よりも大幅に低いビット・レートに対応できます。 {{site.data.keyword.IBM}} では、音声認識の場合は音声に 1 サンプル当たり 16 ビットを使用し、音声データを圧縮するフォーマットを使用することを推奨します。

#### 音声フォーマットについての注記
{: #compressionNotes}

-   `audio/ogg` および `audio/webm` フォーマットは、データのエンコードに使用するコーデック (Opus または Vorbis) に圧縮が依存するコンテナーです。
-   `audio/wav` フォーマットは、非圧縮、ロスレス、または非可逆のデータを格納できるコンテナーです。

#### 詳細情報
{: #compressionMore}

-   音声圧縮について詳しくは、[Data compression (Audio)](https://wikipedia.org/wiki/Data_compression#Audio){: external} を参照してください。
-   データ圧縮を使用して要求で送信できる音声の量を増やす方法について詳しくは、[データ制限および圧縮](#limits)を参照してください。

### チャネル
{: #channels}

*チャネル* は、録音音声のストリームの数を示します。

-   *モノラル* (モノ) 音声は 1 つのチャネルで構成されています。
-   *ステレオ音響* (ステレオ) 音声は通常 2 つのチャネルで構成されています。

{{site.data.keyword.speechtotextshort}} サービスは最大 16 チャネルの音声を受け入れます。 音声認識には 1 つのチャネルのみが使用されるため、複数チャネルの音声はトランスコーディング時に 1 チャネルのモノラル音声にダウンミックスされます。

#### 音声フォーマットについての注記
{: #channelsNotes}

-   `audio/l16` フォーマットの場合、音声が複数チャネルで構成されている場合にはチャネルの数を指定する必要があります。
-   `audio/wav` フォーマットの場合、サービスは最大 9 チャネルの音声を受け入れます。

#### 詳細情報
{: #channelsMore}

-   音声チャネルについて詳しくは、[Audio signal](https://wikipedia.org/wiki/Audio_signal){: external} を参照してください。

### エンディアンネス
{: #endianness}

*エンディアンネス* は、基盤となるコンピューター・アーキテクチャーによるデータ・バイトの並びを示します。

-   *ビッグ・エンディアン*では、データの並びが最上位ビットから始まります。
-   *リトル・エンディアン*では、データの並びが最下位ビットから始まります。

入力音声のエンディアンは、{{site.data.keyword.speechtotextshort}} サービスで自動的に検出されます。

#### 音声フォーマットについての注記
{: #endiannessNotes}

-   `audio/l16` フォーマットの場合、必要に応じてエンディアンネスを指定して自動検出を無効にできます。

#### 詳細情報
{: #endiannessMore}

-   エンディアンネスについて詳しくは、[Endianness](https://wikipedia.org/wiki/Endianness){: external} を参照してください。

### 可聴周波数
{: #frequency}

*可聴周波数* は、音声の可聴周波数の範囲を指します。 人間の標準的な可聴周波数は一般に 20 から 20,000 Hz と考えられています。 ステレオ分析を使用して、音声の周波数を示すスペクトルグラムを生成できます。

音声に適用されるサンプリング・レートは通常、音声の最大周波数の 2 倍です。 例えば、サンプリング・レート 16 kHz は、サンプリング対象の音声信号の最大周波数が 8 kHz であることを意味します。 サービスのモデルはこの点を反映して作成されています。

-   狭帯域モデルは、サンプリング・レートが 8 kHz の音声を使用して作成されています。 狭帯域モデルでは、4 kHz 以下の範囲の情報が検出されると期待されます。
-   広帯域モデルは、サンプリング・レートが 16 kHz の音声を使用して作成されています。 広帯域モデルでは、4 から 8 kHz の範囲内の情報が検出されると期待されます。

モデルのトレーニング・データはさまざまなチャネル (狭帯域モデルの場合は電話) から取得されます。 モデルは、トレーニング対象のチャネルの特性を反映しています。

#### アップサンプリング
{: #upsampling}

*アップサンプリング*では、音声のサンプリング・レートが増加しますが、音声に新しい情報は取り込まれません。 音声を高いレートでサンプリングした場合に取得される音声信号の近似が得られます。 音声データのサイズが増加します。

元々狭帯域周波数でサンプリングされた音声の情報は、0 から 4 kHz の範囲に限定されます。 狭帯域音声を高いサンプリング・レートにアップサンプリングしても、音声認識の正確度は改善しません。 狭帯域音声をアップサンプリングしても、広帯域モデルで予期される範囲の情報は欠落します。 さらに、狭帯域サンプルの予期される範囲で検出される情報は、広帯域サンプルの同じ範囲で検出される情報とは質的に異なります。 したがって、アップサンプリングを行うと、実質的に認識の正確度が低下します。

広帯域サンプリング・レート 16 kHz の場合、サンプリングされる音声信号の最大周波数は 8 kHz であると予期されます。 したがって、16 kHz でサンプリングする前に、元の信号を 8 kHz でフィルタリングする必要があります。 このようにしないと、*エイリアシング*と呼ばれる現象が原因で品質が低下します。 この理由を理解するには、[Nyquist frequency](https://wikipedia.org/wiki/Nyquist_frequency){: external} を参照してください。

わかりやすい例えとして、大型のフラット画面 HDTV で VHS テープを再生することがあります。 高解像度デバイスでテープを再生しても、ストリームに新しい情報は追加されないため、画像がぼやけます。 これは、フォーマットを高機能のデバイスと互換性のあるものにするだけです。 これは、音声のアップサンプリングにも該当します。

#### ダウンサンプリング
{: #downsampling}

*ダウンサンプリング* では、音声のサンプリング・レートが低下します。 音声を低いレートでサンプリングした場合に取得される音声信号の近似が得られます。 ダウンサンプリングでは、音声信号から情報は削除されませんが、音声データのサイズが縮小されます。

音声のダウンサンプリングは、場合によっては有効です。 例えば、音声のサンプリング・レートが 8 kHz よりも高く、*かつ*スペクトルグラフ検査によって 4 kHz よりも高い周波数がないことが判明した場合は、音声を 8 kHz にダウンサンプリングすることを検討してください。

#### 詳細情報
{: #frequencyMore}

-   可聴周波数について詳しくは、[Audio frequency](https://wikipedia.org/wiki/Audio_frequency){: external} を参照してください。
-   アップサンプリングについて詳しくは、[Upsampling](https://wikipedia.org/wiki/Upsampling){: external} を参照してください。
-   ダウンサンプリングについて詳しくは、[Downsampling](https://wikipedia.org/wiki/Downsampling_%28signal_processing%29){: external} を参照してください。

## サポートされている音声フォーマット
{: #formats}

表 1 に、サービスでサポートされている音声フォーマットのまとめを示します。

-   *音声フォーマットと圧縮*では、各フォーマットの定義と各フォーマットでサポートされている圧縮について説明します。 1 つの同期 HTTP 要求または WebSocket 要求で最大 100 MB の音声をサービスに送信できます。 圧縮をサポートしているフォーマットを使用すると、音声のサイズを縮小して、音声に渡すことができるデータの量を最大化できます。 詳しくは、[データ制限および圧縮](#limits)を参照してください。
-   *Content-Type の指定*は、`Content-Type` ヘッダーまたはこのヘッダーに相当するパラメーターを指定して、サービスに送信する音声のフォーマット (MIME タイプ) を指定する必要があるかどうかを示します。 要求では常に音声フォーマットを指定できますが、これは必ずしも必須ではありません。
    -   ほとんどのフォーマットでは、コンテンツ・タイプの指定はオプションです。 コンテンツ・タイプを省略するか、または `application/octet-stream` を指定してサービスでフォーマットを自動検出することができます。
    -   コンテンツ・タイプが必須のフォーマットもあります。 このようなフォーマットでは、サービスでのフォーマットの自動検出に必要な情報 (サンプリング・レートなど) が提供されません。
-   最後の列は、各フォーマットの追加の*必須パラメーター*と*オプション・パラメーター*を示します。 以下のセクションでは、これらのパラメーターについて詳しく説明します。

<table>
  <caption>表 1. サポートされている音声フォーマットのまとめ</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom">
      音声フォーマット<br>と圧縮
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Content-Type<br/>の指定
    </th>
    <th style="text-align:center; vertical-align:bottom">
      必須<br/>parameters
    </th>
    <th style="text-align:center; vertical-align:bottom">
      オプション・<br/>パラメーター
    </th>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/alaw](#alaw)<br/>非可逆
    </td>
    <td style="text-align:center">
      必須
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      なし
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/basic](#basic)<br/>非可逆
    </td>
    <td style="text-align:center">
      必須
    </td>
    <td style="text-align:center">
      なし
    </td>
    <td style="text-align:center">
      なし
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/flac](#flac)<br/>ロスレス
    </td>
    <td style="text-align:center">
      オプション
    </td>
    <td style="text-align:center">
      なし
    </td>
    <td style="text-align:center">
      なし
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/g729](#g729)<br/>非可逆
    </td>
    <td style="text-align:center">
      オプション
    </td>
    <td style="text-align:center">
      なし
    </td>
    <td style="text-align:center">
      なし
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/l16](#l16)<br/>なし
    </td>
    <td style="text-align:center">
      必須
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
      [audio/mpeg](#mp3)<br/>非可逆
    </td>
    <td style="text-align:center">
      オプション
    </td>
    <td style="text-align:center">
      なし
    </td>
    <td style="text-align:center">
      なし
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/mulaw](#mulaw)<br/>非可逆
    </td>
    <td style="text-align:center">
      必須
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      なし
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/ogg](#ogg)<br/>非可逆
    </td>
    <td style="text-align:center">
      オプション
    </td>
    <td style="text-align:center">
      なし
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/wav](#wav)<br/>なし、ロスレス<br/>または非可逆
    </td>
    <td style="text-align:center">
      オプション
    </td>
    <td style="text-align:center">
      なし
    </td>
    <td style="text-align:center">
      なし
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/webm](#webm)<br/>非可逆
    </td>
    <td style="text-align:center">
      オプション
    </td>
    <td style="text-align:center">
      なし
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
</table>

HTTP インターフェースで `curl` コマンドを使用して音声認識要求を行う場合は、`Content-Type` ヘッダーに音声フォーマットを指定するか、`"Content-Type: application/octet-stream"` を指定するか、または `"Content-Type:"` を指定する必要があります。 ヘッダー全体を省略すると、`curl` はデフォルト値 `application/x-www-form-urlencoded` を使用します。
{: important}

### audio/alaw フォーマット
{: #alaw}

*A-law* (`audio/alaw`) は、単一チャネルの非可逆音声フォーマットです。 このフォーマットで使用されるアルゴリズムは、`audio/basic` フォーマットと `audio/mulaw` フォーマットにより適用される u-law アルゴリズムに類似していますが、A-law アルゴリズムでは異なる信号特性が生成されます。 このフォーマットを使用する場合、サービスはフォーマット指定のための追加パラメーターを必要とします。

<table>
  <caption>表 2. `audio/alaw` フォーマットのパラメーター</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      パラメーター
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      説明
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>必須</em>
    </td>
    <td>
      音声が取り込まれたサンプリング・レートを示す整数。 例えば、8 kHz で取り込まれた音声データの場合は以下のパラメーターを指定します。<br/><br/>
      <code>audio/alaw;rate=8000</code>
    </td>
  </tr>
</table>

詳しくは、[A-law algorithm](https://wikipedia.org/wiki/A-law_algorithm){: external} を参照してください。

### audio/basic フォーマット
{: #basic}

*基本音声* (`audio/basic`) は、8 kHz でサンプリングされた 8 ビット u-law (または mu-law) データを使用してエンコードされた単一チャネルの非可逆音声フォーマットです。 このフォーマットは、音声のメディア・タイプを示す最低限の共通フォーマットを提供します。 このサービスでは、狭帯域モデルでのみ、`audio/basic` フォーマットのファイルの使用がサポートされます。

詳しくは、Internet Engineering Task Force (IETF) の [Request for Comment (RFC) 2046](https://tools.ietf.org/html/rfc2046){: external} と [iana.org/assignments/media-types/audio/basic](http://www.iana.org/assignments/media-types/audio/basic){: external} を参照してください。

### audio/flac フォーマット
{: #flac}

*FLAC (Free Lossless Audio Codec (FLAC))* (`audio/flac`) は、ロスレス音声フォーマットです。 詳しくは、[FLAC](https://wikipedia.org/wiki/FLAC){: external}を参照してください。

### audio/g729 フォーマット
{: #g729}

*G.729* (`audio/g729`) は、8 kHz でエンコードされたデータをサポートする非可逆音声フォーマットです。 このサービスでは G.729 Annex D のみがサポートされており、Annex J はサポートされていません。このサービスでは、狭帯域モデルでのみ、`audio/g729` フォーマットのファイルの使用がサポートされます。 詳しくは、[G.729](https://wikipedia.org/wiki/G.729){: external}を参照してください。

### audio/l16 フォーマット
{: #l16}

*16 ビットのリニア PCM (Pulse-Code Modulation)* (`audio/l16`) は、非圧縮音声フォーマットです。 このフォーマットは未加工の PCM ファイルを渡す場合に使用します。 リニア PCM 音声は、コンテナー WAV (Waveform Audio File Format) ファイルに入れることもできます。 `audio/l16` フォーマットを使用すると、サービスではフォーマットを指定するための追加の必須パラメーターとオプション・パラメーターが受け入れられます。

<table>
  <caption>表 3. `audio/l16` フォーマットのパラメーター</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      パラメーター
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      説明
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>必須</em>
    </td>
    <td>
      音声が取り込まれたサンプリング・レートを示す整数。 例えば、16 kHz で取り込まれた音声データの場合は以下のパラメーターを指定します。<br/><br/>
      <code>audio/l16;rate=16000</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>channels</code><br/><em>オプション</em>
    </td>
    <td>
      このサービスでは、デフォルトでは音声が単一チャネル音声として扱われます。
      <em>音声が複数チャネルで構成されている場合は、</em>チャネルの数を示す整数を指定する必要があります。 例えば、16 kHz で取り込まれる 2 チャネル音声データの場合は以下のパラメーターを指定します。<br/><br/>
      <code>audio/l16;rate=16000;channels=2</code><br/><br/>
      このサービスは最大 16 チャネルを受け入れます。 トランスコーディング時に音声が 1 チャネルにダウンミックスされます。
    </td>
  </tr>
  <tr>
    <td>
      <code>endianness</code><br/><em>オプション</em>
    </td>
    <td>
      デフォルトでは、サービスは着信音声のエンディアンネスを自動的に検出します。 ただし、`audio/l16` フォーマットの短い音声の場合、自動検出が失敗して接続が失われることがあります。 エンディアンネスを指定すると、自動検出が無効になります。 <code>big-endian</code> または <code>little-endian</code> を指定します。 例えば、16 kHz、リトル・エンディアン・フォーマットで取り込まれる音声データの場合は以下のパラメーターを指定します。<br/><br/>
      <code>audio/l16;rate=16000;endianness=little-endian</code><br/><br/>
      [Request for Comment (RFC) 2045](https://tools.ietf.org/html/rfc2045#section-5.1) の 5.1 項では、<code>audio/l16</code> データにビッグ・エンディアン・フォーマットが指定されていますが、多くの場合はリトル・エンディアン・フォーマットが使用されます。
    </td>
  </tr>
</table>

詳しくは、IETF の [Request for Comment (RFC) 2586](https://tools.ietf.org/html/rfc2586){: external}、および [Pulse-code modulation](https://wikipedia.org/wiki/Pulse-code_modulation){: external} を参照してください。

### audio/mp3 フォーマットと audio/mpeg フォーマット
{: #mp3}

*MP3* (`audio/mp3`) または *MPEG (Motion Picture Experts Group)* (`audio/mpeg`) は、非可逆音声フォーマットです。 (MP3 と MPEG は同じフォーマットを指します。) 詳しくは、[MP3](https://wikipedia.org/wiki/MP3){: external}を参照してください。

### audio/mulaw フォーマット
{: #mulaw}

*Mu-law* (`audio/mulaw`) は、単一チャネルの非可逆音声フォーマットです。 データのエンコードには u-law (mu-law) アルゴリズムが使用されます。 `audio/basic` フォーマットは、サンプリング・レートが常に 8 kHz の同等のフォーマットです。 このフォーマットを使用する場合、サービスはフォーマット指定のための追加パラメーターを必要とします。

<table>
  <caption>表 4. `audio/mulaw` フォーマットのパラメーター</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      パラメーター
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      説明
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>必須</em>
    </td>
    <td>
      音声が取り込まれたサンプリング・レートを示す整数。 例えば、8 kHz で取り込まれた音声データの場合は以下のパラメーターを指定します。<br/><br/>
      <code>audio/mulaw;rate=8000</code>
    </td>
  </tr>
</table>

詳しくは、[M-law algorithm](https://wikipedia.org/wiki/M-law_algorithm){: external} を参照してください。

### audio/ogg フォーマット
{: #ogg}

*Ogg* (`audio/ogg`) は、Xiph.org Foundation ([xiph.org/ogg](https://www.xiph.org/ogg){: external}) が維持管理するオープン・コンテナー・フォーマットです。 以下の非可逆コーデックを使用して圧縮された音声ストリームを使用できます。

-   *Opus* (`audio/ogg;codecs=opus`)。 詳しくは、[opus-codec.org](https://www.opus-codec.org/){: external} および [Opus (audio format)](https://wikipedia.org/wiki/Opus_%28audio_format%29){: external} を参照してください。特に *Containers* セクションを参照してください。
-   *Vorbis* (`audio/ogg;codecs=vorbis`)。 詳しくは、[xiph.org/vorbis](https://xiph.org/vorbis/){: external} および [Vorbis](https://wikipedia.org/wiki/Vorbis){: external} を参照してください。

コーデックを省略すると、入力音声からコーデックが自動的に検出されます。 Opus は優先されるコーデックです。Opus は Internet Engineering Task Force (IETF) により [Request for Comment (RFC) 6716](https://tools.ietf.org/html/rfc6716){: external} で標準化されています。

### audio/wav フォーマット
{: #wav}

*WAV (Waveform Audio File Format)* (`audio/wav`) は、非圧縮音声ストリームに対してよく使用されるコンテナーですが、圧縮音声も入れることができます。 サービスでは、任意のエンコーディングを使用した WAV 音声がサポートされています。 (FFmpeg の制約のため) サービスは最大 9 チャネルの音声を受け入れることができます。

WAV フォーマットについて詳しくは、[WAV](https://wikipedia.org/wiki/WAV){: external} を参照してください。 WAV 音声を Opus コーデックに変換することでサイズを縮小する方法について詳しくは、[Opus コーデックを使用する audio/ogg への変換](#conversionOgg)を参照してください。

### audio/webm フォーマット
{: #webm}

*WebM (Web Media)* (`audio/webm`) は、WebM プロジェクト ([webmproject.org](https://www.webmproject.org/){: external}) により維持管理されるオープン・コンテナー・フォーマットです。 以下の非可逆コーデックを使用して圧縮された音声ストリームを使用できます。

-   *Opus* (`audio/webm;codecs=opus`)。 詳しくは、[opus-codec.org](https://www.opus-codec.org/){: external} および [Opus (audio format)](https://wikipedia.org/wiki/Opus_%28audio_format%29){: external} を参照してください。特に *Containers* セクションを参照してください。
-   *Vorbis* (`audio/webm;codecs=vorbis`)。 詳しくは、[xiph.org/vorbis](https://xiph.org/vorbis/){: external} および [Vorbis](https://wikipedia.org/wiki/Vorbis){: external} を参照してください。

コーデックを省略すると、入力音声からコーデックが自動的に検出されます。

Chrome ブラウザーでマイクから音声を取り込み、WebM データ・ストリームとしてエンコードする方法を示す JavaScript コードについては、[jsbin.com/hedujihuqo/edit?js,console](https://jsbin.com/hedujihuqo/edit?js,console){: external} を参照してください。 このコードは、取り込んだ音声をサービスに送信しません。
{: tip}

## データ制限および圧縮
{: #limits}

このサービスは、1 つの同期 HTTP 要求または WebSocket 要求で最大 100 MB の音声データを受け入れます。 長時間継続する音声ストリームまたはサイズの大きいファイルの認識を行う場合は、以下の手順に従って、音声が 100 MB のデータ制限を超えないようにしてください。

-   16 kHz 以下のサンプリング・レート (広帯域モデルの場合) または 8 kHz 以下のサンプリング・レート (狭帯域モデルの場合)を使用し、1 サンプル当たり 16 ビットを使用します。 ターゲット・モデル (16 kHz または 8 kHz) よりも高いサンプリング・レートで録音された音声は、サービスによってモデルのレートに変換されます。 したがって、高い周波数を使用しても認識の正確度は改善されず、音声ストリームのサイズが増加します。
-   データ圧縮を提供するフォーマットで音声をエンコードします。 データをより効率的にエンコードすることで、100 MB の制限を超えずに、より多くの音声を送信できます。 `audio/ogg` や `audio/mp3` などの音声フォーマットでは、音声ストリームのサイズが大幅に縮小されます。 これらのフォーマットを使用すると、1 つの要求でより多くの量の音声を送信できます。

`audio/ogg` フォーマットを使用して音声を圧縮し過ぎると、認識の正確度が下がることがあります。 安全のため、音声のビット・レートは 24 kbps 以上に維持してください。

### 音声の概算サイズの比較
{: #compareSizes}

サンプリング・レート 16 kHz、1 サンプル当たり 16 ビットで 2 時間にわたる継続的音声伝送の結果のデータ・ストリームの概算サイズを検討します。

-   データが `audio/wav` フォーマットでエンコードされている場合、2 時間のストリームのサイズは 230 MB となり、サービスの 100 MB 制限を大幅に上回ります。
-   データが `audio/ogg` フォーマットでエンコードされている場合、2 時間のストリームのサイズは 23 MB であり、サービスの制限を下回ります。

以下の表に、同期 HTTP 要求または WebSocket 要求で音声認識のために送信できる音声の最長時間をフォーマット別に示します。 この時間には 100 MB の制限が反映されています。 実際の値は、音声の複雑さと実際の圧縮率に応じて異なります。

<table style="width:75%">
  <caption>表 5. さまざまなフォーマットでの音声の最長時間</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 50%">
      音声フォーマット
    </th>
    <th style="text-align:center; vertical-align:bottom; width 50%">
      音声の最長時間 (概算)
    </th>
  </tr>
  <tr>
    <td><code>audio/wav</code></td>
    <td style="text-align:center">55 分</td>
  </tr>
  <tr>
    <td><code>audio/flac</code></td>
    <td style="text-align:center">1 時間 40 分</td>
  </tr>
  <tr>
    <td><code>audio/mp3</code></td>
    <td style="text-align:center">3 時間 20 分</td>
  </tr>
  <tr>
    <td><code>audio/ogg</code></td>
    <td style="text-align:center">8 時間 40 分</td>
  </tr>
</table>

`audio/ogg;codecs=opus` フォーマットと `audio/webm;codecs=opus` フォーマットは一般に同等であり、サイズもほぼ同一です。 これらのフォーマットでは内部で同じコーデックが使用されます。異なるのはコンテナーのフォーマットだけです。

## 音声の変換
{: #conversion}

音声フォーマットの変換にはさまざまなツールを使用できます。 これらのツールは、サービスでサポートされていないフォーマットの音声や、非圧縮またはロスレスのフォーマットの音声の場合に役立ちます。 後者の場合、音声を非可逆フォーマットに変換してサイズを縮小できます。

音声のフォーマットの変換に使用できるフリーウェア・ツールを以下に示します。

-   Sound eXchange (SoX) ([sox.sourceforge.net](http://sox.sourceforge.net){: external})
-   FFmpeg ([ffmpeg.org](https://www.ffmpeg.org){: external})
-   Audacity&reg; ([audacityteam.org](http://www.audacityteam.org/){: external})
-   Opus コーデックの Ogg フォーマットの場合: **opus-tools** ([opus-codec.org/downloads/](http://opus-codec.org/downloads/){: external})

これらのツールでは、複数の音声フォーマットがクロスプラットフォームでサポートされています。 さらに、ツールの多くは、音声の再生にも使用できます。 ツールを使用する際は、適用される著作権法に違反しないでください。

### Opus コーデックを使用する audio/ogg への変換
{: #conversionOgg}

[**opus-tools**](http://opus-codec.org/downloads/){: external} には、Opus コーデックの Ogg 音声を処理するための 3 種類のコマンド・ライン・ユーティリティーが含まれています。

-   [**opusenc**](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusenc.html){: external} ユーティリティーは、WAV、FLAC などのフォーマットの音声を、Opus コーデックを使用した Ogg にエンコードします。 このページでは、音声ストリームの圧縮方法が説明されています。 圧縮は、リアルタイム音声をサービスに渡すときに役立ちます。
-   [**opusdec**](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusdec.html){: external} ユーティリティーは、Opus コーデックの音声を、非圧縮 PCM WAV ファイルにデコードします。
-   [**opusinfo**](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusinfo.html){: external} ユーティリティーは、Opus ファイルに関する情報と、Opus ファイルの妥当性検査を提供します。

多くのユーザーは音声認識用に WAV ファイルを送信します。 このサービスには、同期 HTTP 要求と WebSocket 要求での 100 MB のデータ制限があるため、WAV フォーマットでは 1 つの要求で認識できる音声の量が少量となります。 **opusenc** コマンドを使用して、音声を優先フォーマットの `audio/ogg:codecs=opus` に変換すると、1 回の認識要求で送信できる音声の量が大幅に増加します。

例として、256 kbps のビット・レートに 1 サンプル当たり 16 ビットを使用する非圧縮広帯域 (16 kHz) WAV ファイル (**input.wav**) の場合を考えましょう。 以下のコマンドは、音声を Opus コーデックを使用するファイル (**output.opus**) に変換します。

```bash
opusenc input.wav output.opus
```
{: pre}

この変換では、音声が 1/4 に圧縮され、ビット・レートが 64 kbps の出力ファイルが生成されます。 ただし、[Opus Recommended Settings](https://wiki.xiph.org/Opus_Recommended_Settings){: external} によれば、発話音声の場合は、ビット・レートを 24 kbps に下げても問題はなく、そのようにしても全帯域が維持されます。 そのようにビット・レートを下げることにより、音声を 1/10 に圧縮できます。 以下のコマンドは、`--bitrate` オプションを使用して、ビット・レート 24 kbps で出力ファイルを生成します。

```bash
opusenc --bitrate 24 input.wav output.opus
```
{: pre}

**opusenc** ユーティリティーによる圧縮は高速です。 リアルタイムよりも約 100 倍高速に圧縮が行われます。 コマンドが完了すると、実行時間と結果の音声データに関する詳細情報を示す出力がコンソールに書き込まれます。

## 音声認識を改善するためのヒント
{: #audioTips}

音声認識の品質の改善に役立つヒントを以下に示します。

-   音声の録音方法によって、サービスの結果に大きな差が生じることがあります。 音声認識は、入力音声の品質に大きく左右される可能性があります。 可能な限り高い正確度を実現するには、入力音声の品質を可能な限り高くしてください。
    -   可能な場合は近くに配置した音声指向マイク (ヘッドセットなど) を使用し、必要に応じてマイクの設定を調整してください。 音声の取り込みにプロ用マイクを使用すると、サービスの機能性が最大限に発揮されます。
    -   システム内蔵マイクは使用しないでください。 一般にモバイル・デバイスやタブレットの内蔵マイクでは不十分なことがよくあります。
    -   話者がマイクに近い位置にいることを確認してください。 話者とマイクの距離が広がると、正確度が低下します。 例えば 10 フィートの距離では、適切な結果が生成されにくくなります。
-   音声認識は、バックグラウンド・ノイズや人間の発話のニュアンスに左右されます。
    -   エンジン音、作業装置、通りの騒音、バックグラウンドでの会話などは、認識の正確度が大幅に低下する原因となる可能性があります。
    -   方言的なアクセントや発音の違いも正確度の低下の原因となることがあります。

    音声にこれらの特性がある場合は、音響モデル・カスタマイズを使用して音声認識の正確度を改善することを検討してください。 詳しくは、[カスタマイズ・インターフェース](/docs/services/speech-to-text?topic=speech-to-text-customization)を参照してください
