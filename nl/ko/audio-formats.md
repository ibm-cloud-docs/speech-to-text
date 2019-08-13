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

# 오디오 형식
{: #audio-formats}

{{site.data.keyword.speechtotextfull}} 서비스는 다양한 형식의 오디오에서 음성을 추출할 수 있습니다.

-   오디오 및 오디오가 설명되고 지정되는 방법에 익숙하지 않은 경우 [오디오 특성 및 용어](#terminology)부터 시작하여 시작하는 데 도움을 받으십시오.
-   오디오 사용 방법을 이미 이해하고 있는 경우 이 서비스가 지원하는 형식에 대해 알아보려면 [지원되는 오디오 형식](#formats)으로 이동하십시오.

마지막 섹션 [데이터 한계 및 압축](#limits), [오디오 변환](#conversion) 및 [음성 인식을 개선하기 위한 팁](#audioTips)은 이 서비스를 최대한 활용하는 데 도움이 될 수 있습니다.

## 오디오 특성 및 용어
{: #terminology}

다음과 같은 용어가 다양한 형식의 오디오 데이터 특성에 대해 설명하는 데 사용됩니다.

### 샘플링 속도
{: #samplingRate}

*샘플링 속도*(또는 샘플링 빈도)는 초당 가져오는 오디오 샘플의 수입니다. 샘플링 빈도는 헤르츠(Hz) 또는 킬로헤르츠(kHz)로 측정됩니다. 예를 들어, 초당 16,000개 샘플의 속도는 16,000Hz(또는 16kHz)와 동일합니다. {{site.data.keyword.speechtotextshort}} 서비스를 사용하여 오디오의 샘플링 속도를 표시할 모델을 지정합니다.

-   *광대역* 모델은 16KHz 이상으로 샘플링된 오디오에 사용되며, {{site.data.keyword.IBM}}에서는 응답성이 뛰어난 실시간 애플리케이션(예: 실시간 음성 애플리케이션)에 사용하도록 권장합니다.
-   *협대역* 모델은 일반적으로 전화통신 오디오에 사용되는 속도인 8KHz로 샘플링된 오디오에 사용됩니다.

이 서비스는 대부분의 언어 및 형식에 대해 광대역 및 협대역 오디오를 모두 지원합니다. 음성을 인식하기 전에 사용자가 지정하는 모델과 일치하도록 오디오의 샘플링 속도를 자동으로 조정합니다.

-   광대역 모델의 경우 서비스가 더 높은 샘플링 속도로 녹음된 오디오를 16kHz로 변환합니다.
-   협대역 모델의 경우 오디오를 8kHz로 변환합니다.

이론적으로는 44KHz 오디오를 광대역 또는 협대역 모델로 전송할 수 있지만 오디오의 크기를 불필요하게 증가시킵니다. 전송할 수 있는 오디오의 양을 최대화하려면 오디오의 샘플링 속도를 사용자가 사용하는 모델과 일치십시오. 이 서비스는 모델의 내부 샘플링 속도보다 낮은 속도로 샘플링된 오디오를 허용하지 않습니다.

#### 오디오 형식에 대한 참고사항
{: #samplingRateNotes}

-   `audio/alaw`, `audio/l16` 및 `audio/mulaw` 형식의 경우 오디오의 속도를 지정해야 합니다.
-   `audio/basic` 및 `audio/g729` 형식의 경우 이 서비스는 협대역 오디오만 지원합니다.

#### 자세한 정보
{: #samplingRateMore}

-   샘플링 속도에 대한 자세한 정보는 [Sampling (signal processing)](https://wikipedia.org/wiki/Sampling_%28signal_processing%29){: external}을 참조하십시오.
-   이 서비스가 각각의 지원되는 언어에 대해 제공하는 모델에 대한 자세한 정보는 [언어 및 모델](/docs/services/speech-to-text?topic=speech-to-text-models)을 참조하십시오.

### 비트 전송률
{: #bitRate}

*비트 전송률*은 초당 전송되는 데이터 비트 수입니다. 오디오 스트림의 비트 전송률은 초당 킬로비트(kbps)로 측정됩니다. 비트 전송률은 샘플링 속도 및 샘플당 저장된 비트 수에서 계산됩니다. 음성 인식의 경우 {{site.data.keyword.IBM}}에서는 오디오에 대해 샘플당 16비트를 녹음하도록 권장합니다.

예를 들어, 16kHz의 광대역 샘플링 속도와 샘플당 16비트를 사용하는 오디오의 비트 전송률은 256kbps(`(16,000 * 16) / 1000`)입니다.

#### 자세한 정보
{: #bitRateMore}

-   비트 속도에 대한 자세한 정보는 [Bit rate](https://wikipedia.org/wiki/Bit_rate){: external}를 참조하십시오.
-   샘플링 속도 및 비트 전송률에 대한 일반 논의는 [What are bit rates?](http://www.richardfarrar.com/what-are-bit-rates/){: external} 및 [Choosing bit rates for podcasts](http://www.richardfarrar.com/choosing-bit-rates-for-podcasts/){: external}를 참조하십시오.

### 압축
{: #compression}

*압축*은 여러 오디오 형식에서 오디오 데이터의 크기를 줄이기 위해 사용됩니다. 압축은 샘플당 저장된 비트 수와 비트 전송률을 줄입니다. 일부 형식은 압축을 사용하지 않지만 대부분은 기본 유형의 압축 중 하나를 사용합니다.

-   *무손실* 압축은 품질의 손실 없이 오디오의 크기를 줄이지만 일반적으로 압축률이 낮습니다.
-   *손실* 압축은 오디오의 크기를 10배 정도 줄이지만 일부 데이터와 품질이 압축에서 영구적으로 손실됩니다.

{{site.data.keyword.speechtotextshort}} 서비스를 사용하면 손실 압축을 안전하게 사용하여 인식 요청과 함께 서비스에 전송할 수 있는 오디오의 양을 최대화할 수 있습니다. 인간 음성의 동적 범위는 음악보다 더 제한적이므로 음성이 다른 유형의 오디오보다 훨씬 더 낮은 비트 전송률을 수용할 수 있습니다. 음성 인식의 경우 {{site.data.keyword.IBM}}에서는 오디오에 대해 샘플당 16비트를 사용하고 오디오 데이터를 압축하는 형식을 사용하도록 권장합니다.

#### 오디오 형식에 대한 참고사항
{: #compressionNotes}

-   `audio/ogg` 및 `audio/webm` 형식은 압축이 데이터를 인코딩하는 데 사용하는 코덱(Opus 또는 Vorbis)에 의존하는 컨테이너입니다.
-   `audio/wav` 형식은 비압축, 무손실 또는 손실 데이터를 포함할 수 있는 컨테이너입니다.

#### 자세한 정보
{: #compressionMore}

-   오디오 압축에 대한 자세한 정보는 [Data compression (Audio)](https://wikipedia.org/wiki/Data_compression#Audio){: external}을 참조하십시오.
-   데이터 압축을 사용하여 요청과 함께 전송할 수 있는 오디오의 양을 늘리는 방법에 대한 자세한 정보는 [데이터 한계 및 압축](#limits)을 사용하십시오.

### 채널
{: #channels}

*채널*은 녹음된 오디오의 스트림 수를 표시합니다.

-   *모노럴*(또는 모노) 오디오에는 하나의 채널만 있습니다.
-   *스테레오포닉*(또는 스테레오) 오디오에는 일반적으로 두 개의 채널이 있습니다.

{{site.data.keyword.speechtotextshort}} 서비스는 최대 16개의 채널로 오디오를 수신합니다. 이 서비스는 음성 인식에 단일 채널만 사용하기 때문에 트랜스코딩 중에 다중 채널을 사용하는 오디오를 단일 채널 모노로 다운믹스합니다.

#### 오디오 형식에 대한 참고사항
{: #channelsNotes}

-   `audio/l16` 형식의 경우 오디오에 둘 이상의 채널이 있으면 채널의 수를 지정해야 합니다.
-   `audio/wav` 형식의 경우 이 서비스는 최대 9개의 채널로 오디오를 수신합니다.

#### 자세한 정보
{: #channelsMore}

-   오디오 채널에 대한 자세한 정보는 [Audio signal](https://wikipedia.org/wiki/Audio_signal){: external}을 참조하십시오.

### 엔디안
{: #endianness}

*엔디안*은 데이터의 바이트가 기본 컴퓨터 아키텍처에서 구성되는 방법을 표시합니다.

-   *빅 엔디안(big-endian)*은 최상위 비트를 기준으로 데이터를 정렬합니다.
-   *리틀(little-endian*은 최하위 비트를 기준으로 데이터를 정렬합니다.

{{site.data.keyword.speechtotextshort}} 서비스는 수신 오디오의 엔디안을 자동으로 검색합니다.

#### 오디오 형식에 대한 참고사항
{: #endiannessNotes}

-   `audio/l16` 형식에서는 필요한 경우 자동 검색을 사용 안함으로 설정하도록 엔디안을 지정할 수 있습니다.

#### 자세한 정보
{: #endiannessMore}

-   엔디안에 대한 자세한 정보는 [Endianness](https://wikipedia.org/wiki/Endianness){: external}를 참조하십시오.

### 오디오 주파수
{: #frequency}

*오디오 주파수*는 오디오의 가청 주파수 범위를 나타냅니다. 사람의 표준 가청 주파수는 일반적으로 20 - 20,000Hz로 허용됩니다. 스펙트로그래프 분석을 사용하여 오디오의 빈도수 컨텐츠를 표시하는 스펙트로그램을 생성할 수 있습니다.

오디오에 적용되는 샘플링 속도는 일반적으로 최대 오디오 주파수의 두 배입니다. 예를 들어, 샘플링 속도 16kHz는 샘플링된 오디오 신호의 최대 주파수가 8kHz임을 의미합니다. 서비스의 모델은 이를 염두에 두고 작성됩니다.

-   협대역 모델은 8kHz로 샘플링된 오디오로 빌드됩니다. 협대역 모델은 4kHz 이하의 범위에서 정보를 찾을 것으로 예상합니다.
-   광대역 모델은 16kHz로 샘플링된 오디오로 빌드됩니다. 광대역 모델은 4 - 8kHz 범위에서 정보를 찾을 것으로 예상합니다.

모델에 대한 훈련 데이터는 여러 채널(협대역 모델의 경우 전화통신)에서 파생됩니다. 모델은 훈련된 채널의 특성을 반영합니다.

#### 업샘플링
{: #upsampling}

*업샘플링*은 오디오의 샘플링 속도를 늘리지만 오디오에 새로운 정보를 도입하지 않습니다. 더 높은 속도로 오디오를 샘플링하여 얻은 오디오 신호의 근사값을 생성합니다. 또한 오디오 데이터의 크기를 늘립니다.

원래 협대역 주파수에서 샘플링된 오디오의 정보는 0 - 4kHz 범위로 제한됩니다. 협대역 오디오를 더 높은 샘플링 속도로 업샘플링해도 음성 인식 정확도가 향상되지 않을 수 있습니다. 협대역 오디오를 업샘플링하는 경우 광대역 모델이 예상하는 범위의 정보가 부족합니다. 또한 협대역 샘플의 예상 범위에서 발견되는 정보가 광대역 샘플의 동일한 범위에서 발견되는 정보와 질적으로 다릅니다. 따라서 업샘플링은 실제로 인식 정확도를 저하시킵니다.

16kHz의 광대역 샘플링 속도의 경우 샘플링된 오디오 신호에 존재하는 최대 주파수는 8kHz일 것으로 예상됩니다. 따라서 16kHz의 속도로 샘플링하기 전에 8kHz에서 원래 신호를 필터링해야 합니다. 그렇지 않으면 *앨리어싱*으로 알려진 현상으로 인해 성능 저하가 발생합니다. 그 이유를 이해하려면 [Nyquist frequency](https://wikipedia.org/wiki/Nyquist_frequency){: external}를 참조하십시오.

유용한 비교 방법은 대형 평면 HDTV에서 VHS 테이프를 본다고 상상하는 것입니다. 고화질 디바이스에서 테이프를 재생하면 실제로 새 정보가 스트림에 추가될 수 없으므로 이미지가 흐릿해집니다. 단순히 형식이 더 나은 장치와 호환되도록 하기 때문입니다. 업샘플링 오디오의 경우도 마찬가지입니다.

#### 다운샘플링
{: #downsampling}

*다운샘플링*은 오디오의 샘플링 속도를 줄입니다. 더 낮은 속도로 오디오를 샘플링하여 얻은 오디오 신호의 근사값을 생성합니다. 다운샘플링은 오디오 신호에서 정보를 제거하지 않지만 오디오 데이터의 크기를 줄입니다.

일부 경우에는 오디오를 다운샘플링하는 것이 효과적일 수 있습니다. 예를 들어, 오디오의 샘플링 속도가 8KHz보다 높고 *또한* 스펙트로그래프 검사에서 4KHz보다 높은 주파수 컨텐츠가 표시되지 않으면 오디오를 8KHz로 다운샘플링하는 것을 고려하십시오.

#### 자세한 정보
{: #frequencyMore}

-   오디오 주파수에 대한 자세한 정보는 [Audio frequency](https://wikipedia.org/wiki/Audio_frequency){: external}를 참조하십시오.
-   업샘플링에 대한 자세한 정보는 [>Upsampling](https://wikipedia.org/wiki/Upsampling){: external}을 참조하십시오.
-   다운샘플링에 대한 자세한 정보는 [Downsampling](https://wikipedia.org/wiki/Downsampling_%28signal_processing%29){: external}을 참조하십시오.

## 지원되는 오디오 형식
{: #formats}

표 1에서는 이 서비스가 지원하는 오디오 형식의 요약을 제공합니다.

-   *오디오 형식 및 압축*은 각 형식을 식별하며 지원되는 압축을 표시합니다. 최대 100MB의 오디오를 하나의 동기 HTTP 또는 WebSocket 요청으로 서비스에 전송할 수 있습니다. 압축을 지원하는 형식을 사용하면 오디오 크기를 줄여서 서비스에 전달할 수 있는 데이터의 양을 최대화할 수 있습니다. 자세한 정보는 [데이터 한계 및 압축](#limits)을 참조하십시오.
-   *Content-type 스펙*은 `Content-Type` 헤더 또는 동등한 매개변수를 사용하여 서비스에 전송하는 오디오의 형식(MIME 유형)을 지정해야 하는지 여부를 표시합니다. 요청에 대한 오디오 형식을 지정할 수 있지만 항상 필요한 것은 아닙니다.
    -   대부분의 형식에서 컨텐츠 유형은 선택사항입니다. 컨텐츠 유형을 생략하거나 서비스가 형식을 자동으로 발견하도록 `application/octet-stream`을 지정하십시오.
    -   기타 형식에서는 컨텐츠 유형이 필수입니다. 이러한 형식은 서비스가 형식을 자동으로 검색하는 데 필요한 정보(예: 샘플링 속도)를 제공하지 않습니다.
-   마지막 열은 각 형식에 대한 추가 *필수 매개변수* 및 *선택적 매개변수*를 식별합니다. 다음 섹션에서 이러한 매개변수에 대한 자세한 정보를 제공합니다.

<table>
  <caption>표 1. 지원되는 오디오 형식의 요약</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom">
      오디오 형식<br>및 압축
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Content-type<br/>스펙
    </th>
    <th style="text-align:center; vertical-align:bottom">
      필수<br/>매개변수
    </th>
    <th style="text-align:center; vertical-align:bottom">
      선택사항<br/>매개변수
    </th>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/alaw](#alaw)<br/>손실
    </td>
    <td style="text-align:center">
      필수
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
없음
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/basic](#basic)<br/>손실
    </td>
    <td style="text-align:center">
      필수
    </td>
    <td style="text-align:center">
없음
    </td>
    <td style="text-align:center">
없음
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/flac](#flac)<br/>무손실
    </td>
    <td style="text-align:center">
      선택사항
    </td>
    <td style="text-align:center">
없음
    </td>
    <td style="text-align:center">
없음
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/g729](#g729)<br/>손실
    </td>
    <td style="text-align:center">
      선택사항
    </td>
    <td style="text-align:center">
없음
    </td>
    <td style="text-align:center">
없음
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/l16](#l16)<br/>없음
    </td>
    <td style="text-align:center">
      필수
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
      [audio/mpeg](#mp3)<br/>손실
    </td>
    <td style="text-align:center">
      선택사항
    </td>
    <td style="text-align:center">
없음
    </td>
    <td style="text-align:center">
없음
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/mulaw](#mulaw)<br/>손실
    </td>
    <td style="text-align:center">
      필수
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
없음
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/ogg](#ogg)<br/>손실
    </td>
    <td style="text-align:center">
      선택사항
    </td>
    <td style="text-align:center">
없음
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/wav](#wav)<br/>없음, 무손실<br/>또는 손실
    </td>
    <td style="text-align:center">
      선택사항
    </td>
    <td style="text-align:center">
없음
    </td>
    <td style="text-align:center">
없음
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/webm](#webm)<br/>손실
    </td>
    <td style="text-align:center">
      선택사항
    </td>
    <td style="text-align:center">
없음
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
</table>

`curl` 명령을 사용하여 HTTP 인터페이스로 음성 인식을 요청하는 경우 `Content-Type` 헤더를 사용하여 오디오 형식을 지정하거나 `"Content-Type: application/octet-stream"` 또는 `"Content-Type:"`을 지정해야 합니다. 헤더를 완전히 생략하면 `curl`이 기본값인 `application/x-www-form-urlencoded`를 사용합니다.
{: important}

### audio/alaw 형식
{: #alaw}

*a-law*(`audio/alaw`)는 단일 채널의 손실 오디오 형식입니다. 이 형식은 `audio/basic` 및 `audio/mulaw` 형식에 적용되는 u-law 알고리즘과 유사한 알고리즘을 사용하지만 a-law 알고리즘은 다른 신호 특성을 생성합니다. 이 형식을 사용하는 경우 이 서비스에는 형식 스펙에 대한 추가 매개변수가 필요합니다.

<table>
  <caption>표 2. `audio/alaw` 형식에 대한 매개변수</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
매개변수
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
설명
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>필수</em>
    </td>
    <td>
      오디오가 캡처되는 샘플링 속도를 지정하는 정수입니다. 예를 들어, 8kHz로 캡처된 오디오 데이터의 경우 다음 매개변수를 지정하십시오.<br/><br/>
      <code>audio/alaw;rate=8000</code>
    </td>
  </tr>
</table>

자세한 정보는 [A-law algorithm](https://wikipedia.org/wiki/A-law_algorithm){: external}을 참조하십시오.

### audio/basic 형식
{: #basic}

*기본 오디오*(`audio/basic`)는 8KHz로 샘플링된 8비트 u-law(또는 mu-law) 데이터를 사용하여 인코딩된 단일 채널의 손실 오디오 형식입니다. 이 형식은 오디오의 매체 유형을 표시하기 위한 가장 낮은 공통분모를 제공합니다. 이 서비스는 협대역 모델에만 `audio/basic` 형식의 파일을 사용하도록 지원합니다.

자세한 정보는 IETF(Internet Engineering Task Force) [RFC(Request for Comment) 2046](https://tools.ietf.org/html/rfc2046){: external} 및 [iana.org/assignments/media-types/audio/basic](http://www.iana.org/assignments/media-types/audio/basic){: external}을 참조하십시오.

### audio/flac 형식
{: #flac}

*FLAC(Free Lossless Audio Codec)*(`audio/flac`)는 무손실 오디오 형식입니다. 자세한 정보는 [FLAC](https://wikipedia.org/wiki/FLAC){: external}를 참조하십시오.

### audio/g729 형식
{: #g729}

*G.729*(`audio/g729`)는 8kHz로 인코딩된 데이터를 지원하는 손실 오디오 형식입니다. 이 서비스는 Annex J가 아니라 G.729 Annex D만 지원하며 협대역 모델에만 `audio/g729` 형식의 파일을 사용하도록 지원합니다. 자세한 정보는 [G.729](https://wikipedia.org/wiki/G.729){: external}를 참조하십시오.

### audio/l16 형식
{: #l16}

*선형 16비트 PCM(Pulse-Code Modulation)*(`audio/l16`)은 비압축 오디오 형식입니다. 원시 PCM 파일을 전달하려면 이 형식을 사용하십시오. 선형 PCM 오디오는 컨테이너 WAV(Waveform Audio File Format) 파일 내부에서 전달될 수도 있습니다. `audio/l16` 형식을 사용하는 경우 서비스가 형식 스펙에 대한 추가 필수 및 선택적 매개변수를 허용합니다.

<table>
  <caption>표 3. `audio/l16` 형식에 대한 매개변수</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
매개변수
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
설명
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>필수</em>
    </td>
    <td>
      오디오가 캡처되는 샘플링 속도를 지정하는 정수입니다. 예를 들어, 16kHz로 캡처된 오디오 데이터의 경우 다음 매개변수를 지정하십시오.<br/><br/>
      <code>audio/l16;rate=16000</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>channels</code><br/><em>선택사항</em>
    </td>
    <td>
      기본적으로 이 서비스는 오디오에 단일 채널이 있는 것으로 처리합니다.
      <em>오디오에 둘 이상의 채널이 있는 경우</em> 채널 수를 식별하는 정수를 지정해야 합니다. 예를 들어, 16kHz로 캡처된 2채널 오디오 데이터의 경우 다음 매개변수를 지정하십시오.<br/><br/>
      <code>audio/l16;rate=16000;channels=2</code><br/><br/>
      이 서비스는 최대 16개의 채널을 허용합니다. 트랜스코딩 중에 오디오를 하나의 채널로 다운믹스합니다.
    </td>
  </tr>
  <tr>
    <td>
      <code>endianness</code><br/><em>선택사항</em>
    </td>
    <td>
      기본적으로 이 서비스는 수신 오디오의 엔디안을 자동으로 검색합니다. 그러나 때때로 자동 검색이 실패하고 `audio/l16` 형식의 짧은 오디오에 대한 연결을 삭제할 수 있습니다. 엔디안을 지정하면 자동 검색이 사용 안함으로 설정됩니다. <code>big-endian</code> 또는 <code>little-endian</code>을 지정하십시오. 예를
      들어, 리틀 엔디안(little-endian) 형식의 16kHz로 캡처된 오디오 데이터의 경우
      다음 매개변수를 지정하십시오.<br/><br/>
      <code>audio/l16;rate=16000;endianness=little-endian</code><br/><br/>
      [RFC(Request for Comment) 2045](https://tools.ietf.org/html/rfc2045#section-5.1)의
      5.1 섹션에서는 <code>audio/l16</code> 데이터에 빅 엔디안(big-endian)
      형식을 지정하지만 많은 사용자는 리틀 엔디안(little-endian) 형식을
      사용합니다.
    </td>
  </tr>
</table>

자세한 정보는 IETF [RFC(Request for Comment) 2586](https://tools.ietf.org/html/rfc2586){: external} 및 [Pulse-code modulation](https://wikipedia.org/wiki/Pulse-code_modulation){: external}을 참조하십시오.

### audio/mp3 및 audio/mpeg 형식
{: #mp3}

*MP3*(`audio/mp3`) 또는 *MPEG(Motion Picture Experts Group)*(`audio/mpeg`)은 손실 오디오 형식입니다. (MP3 및 MPEG은 동일한 형식을 나타냅니다.) 자세한 정보는 [MP3](https://wikipedia.org/wiki/MP3){: external}를 참조하십시오.

### audio/mulaw 형식
{: #mulaw}

*mu-law*(`audio/mulaw`)는 단일 채널의 손실 오디오 형식입니다. 데이터가 u-law(또는 mu-law) 알고리즘을 사용하여 인코딩됩니다. `audio/basic` 형식은 항상 8kHz로 샘플링되는 동등한 형식입니다. 이 형식을 사용하는 경우 이 서비스에는 형식 스펙에 대한 추가 매개변수가 필요합니다.

<table>
  <caption>표 4. `audio/mulaw` 형식에 대한 매개변수</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
매개변수
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
설명
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>필수</em>
    </td>
    <td>
      오디오가 캡처되는 샘플링 속도를 지정하는 정수입니다. 예를 들어, 8kHz로 캡처된 오디오 데이터의 경우 다음 매개변수를 지정하십시오.<br/><br/>
      <code>audio/mulaw;rate=8000</code>
    </td>
  </tr>
</table>

자세한 정보는 [M-law algorithm](https://wikipedia.org/wiki/M-law_algorithm){: external}을 참조하십시오.

### audio/ogg 형식
{: #ogg}

*Ogg*(`audio/ogg`)는 Xiph.org Foundation([xiph.org/ogg](https://www.xiph.org/ogg){: external})으로 유지보수되는 개방형 컨테이너 형식입니다. 다음과 같은 손실 코덱으로 압축된 오디오 스트림을 사용할 수 있습니다.

-   *Opus*(`audio/ogg;codecs=opus`). 자세한 정보는 [opus-codec.org](https://www.opus-codec.org/){: external} 및 [Opus(오디오 형식)](https://wikipedia.org/wiki/Opus_%28audio_format%29){: external}를 참조하십시오. 특히 *Containers* 섹션을 살펴보십시오.
-   *Vorbis*(`audio/ogg;codecs=vorbis`). 자세한 정보는 [xiph.org/vorbis](https://xiph.org/vorbis/){: external} 및 [Vorbis](https://wikipedia.org/wiki/Vorbis){: external}를 참조하십시오.

코덱을 생략하면 이 서비스가 자동으로 입력 오디오에서 코덱을 검색합니다. Opus는 선호되는 코덱이며 [RFC(Request for Comment) 6716](https://tools.ietf.org/html/rfc6716){: external}으로 IETF(Internet Engineering Task Force)가 표준화됩니다. 

### audio/wav 형식
{: #wav}

*WAV(Waveform Audio File Format)*(`audio/wav`)는 비압축 오디오 스트림에 자주 사용되는 컨테이너 형식이지만 압축 오디오도 포함할 수 있습니다. 이 서비스는 임의의 인코딩을 사용하는 WAV 오디오를 지원합니다. FFmpeg 제한사항으로 인해 최대 9개의 채널로 WAV 오디오를 수신합니다.

WAV 형식에 대한 자세한 정보는 [WAV](https://wikipedia.org/wiki/WAV){: external}를 참조하십시오. Opus 코덱으로 변환하여 WAV 오디오의 크기를 줄이는 방법에 대한 자세한 정보는 [Opus 코덱을 사용하는 audio/ogg로 변환](#conversionOgg)을 참조하십시오.

### audio/webm 형식
{: #webm}

*WebM(Web Media)*(`audio/webm`)은 WebM 프로젝트([webmproject.org](https://www.webmproject.org/){: external})에서 유지보수되는 개방형 컨테이너 형식입니다. 다음과 같은 손실 코덱으로 압축된 오디오 스트림을 사용할 수 있습니다.

-   *Opus*(`audio/webm;codecs=opus`). 자세한 정보는 [opus-codec.org](https://www.opus-codec.org/){: external} 및 [Opus(오디오 형식)](https://wikipedia.org/wiki/Opus_%28audio_format%29){: external}를 참조하십시오. 특히 *Containers* 섹션을 살펴보십시오.
-   *Vorbis*(`audio/webm;codecs=vorbis`). 자세한 정보는 [xiph.org/vorbis](https://xiph.org/vorbis/){: external} 및 [Vorbis](https://wikipedia.org/wiki/Vorbis){: external}를 참조하십시오.

코덱을 생략하면 이 서비스가 자동으로 입력 오디오에서 코덱을 검색합니다.

Chrome 브라우저에서 마이크의 오디오를 캡처하여 WebM 데이터 스트림으로 인코딩하는 방법을 보여주는 JavaScript 코드는 [jsbin.com/hedujihuqo/edit?js,console](https://jsbin.com/hedujihuqo/edit?js,console){: external}을 참조하십시오. 이 코드는 캡처된 오디오를 서비스에 제출하지 않습니다.
{: tip}

## 데이터 한계 및 압축
{: #limits}

이 서비스는 동기 HTTP 또는 WebSocket 요청으로 변환을 위해 최대 100MB의 오디오 데이터를 허용합니다. 긴 연속 오디오 스트림 또는 큰 파일을 인식하는 경우 다음 단계를 수행하여 오디오가 100MB 데이터 한계를 초과하지 않는지 확인하십시오.

-   16KHz(광대역 모델의 경우) 또는 8kHz(협대역 모델의 경우) 이하의 샘플링 속도를 사용하고 샘플당 16비트를 사용하십시오. 이 서비스는 대상 모델(16kHz 또는 8kHz)보다 더 높은 샘플링 속도로 녹음된 오디오를 모델의 속도로 변환합니다. 따라서 주파수가 높을수록 인식 정확도가 향상되지 않지만 오디오 스트림의 크기가 증가합니다.
-   데이터 압축을 제공하는 형식으로 오디오를 인코딩하십시오. 데이터를 더 효율적으로 인코딩하면 100MB 한계를 초과하지 않고 훨씬 더 많은 오디오를 전송할 수 있습니다. `audio/ogg` 및 `audio/mp3`와 같은 오디오 형식은 오디오 스트림의 크기를 크게 줄입니다. 이러한 형식을 사용하여 단일 요청으로 더 많은 양의 오디오를 전송할 수 있습니다.

`audio/ogg` 형식을 사용하여 오디오를 너무 심하게 압축하면 인식 정확도가 떨어질 수 있습니다. 안전하게 하려면 오디오에 대해 24kbps 이상의 비트 전송률을 유지하십시오.

### 대략적인 오디오 크기 비교
{: #compareSizes}

16kHz 및 샘플당 16비트로 샘플링된 2시간 동안의 연속 음성 전송으로 발생한 데이터 스트림의 대략적인 크기를 고려하십시오.

-   데이터가 `audio/wav` 형식으로 인코딩된 경우 2시간 길이 스트림의 크기가 서비스의 100B 한계를 크게 초과하는 230MB입니다.
-   데이터가 `audio/ogg` 형식으로 인코딩된 경우 2시간 길이 스트림의 크기가 서비스의 한계에 크게 못 미치는 23MB입니다.

다음 표는 동기 HTTP 또는 WebSocket 요청으로 음성 인식을 위해 전송될 수 있는 다양한 형식의 오디오에 대한 최대 지속 시간을 대략적으로 나타낸 것입니다. 지속 시간은 100MB 서비스 한계를 고려합니다. 실제 값은 오디오의 복잡도와 달성된 압축률에 따라 다를 수 있습니다.

<table style="width:75%">
  <caption>표 5. 다양한 형식의 오디오에 대한 최대 지속 시간</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 50%">
      오디오 형식
    </th>
    <th style="text-align:center; vertical-align:bottom; width 50%">
      오디오의 최대 지속 시간(근사치)
    </th>
  </tr>
  <tr>
    <td><code>audio/wav</code></td>
    <td style="text-align:center">55분</td>
  </tr>
  <tr>
    <td><code>audio/flac</code></td>
    <td style="text-align:center">1시간 40분</td>
  </tr>
  <tr>
    <td><code>audio/mp3</code></td>
    <td style="text-align:center">3시간 20분</td>
  </tr>
  <tr>
    <td><code>audio/ogg</code></td>
    <td style="text-align:center">8시간 40분</td>
  </tr>
</table>

`audio/ogg;codecs=opus` 및 `audio/webm;codecs=opus` 형식은 일반적으로 동등하며 크기가 거의 동일합니다. 내부적으로 동일한 코덱을 사용하며 컨테이너 형식만 서로 다릅니다.

## 오디오 변환
{: #conversion}

다양한 도구를 사용하여 오디오를 다른 형식으로 변환할 수 있습니다. 이러한 도구는 오디오가 서비스에서 지원되지 않는 형식이거나 비압축 또는 무손실 형식인 경우에 유용할 수 있습니다. 후자의 경우 오디오를 손실 형식으로 변환하여 크기를 줄일 수 있습니다.

다음 프리웨어 도구 중 하나를 사용하여 오디오를 한 형식에서 다른 형식으로 변환할 수 있습니다.

-   SoX(Sound eXchange)([sox.sourceforge.net](http://sox.sourceforge.net){: external})
-   FFmpeg([ffmpeg.org](https://www.ffmpeg.org){: external})
-   Audacity&reg;([audacityteam.org](http://www.audacityteam.org/){: external})
-   Opus 코덱을 사용하는 Ogg 형식의 경우 **opus-tools**([opus-codec.org/downloads/](http://opus-codec.org/downloads/){: external})

이러한 도구는 여러 오디오 형식에 대한 크로스 플랫폼 지원을 제공합니다. 또한 많은 도구를 사용하여 오디오를 재생할 수 있습니다. 이러한 도구를 사용하여 해당 저작권법을 위반하지 마십시오.

### Opus 코덱을 사용하는 audio/ogg로 변환
{: #conversionOgg}

[**opus-tools**](http://opus-codec.org/downloads/){: external}에는 Opus 코덱의 Ogg 오디오에 대해 작업하기 위한 세 개의 명령행 유틸리티가 포함되어 있습니다.

-   [**opusenc**](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusenc.html){: external} 유틸리티는 Opus 코덱을 사용하여 WAV, FLAC 및 기타 형식의 오디오를 Ogg로 인코딩합니다. 이 페이지에는 오디오 스트림을 압축하는 방법이 표시됩니다. 압축은 실시간 오디오를 서비스에 전달하는 데 유용합니다.
-   [**opusdec**](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusdec.html){: external} 유틸리티는 Opus 코덱의 오디오를 압축되지 않은 PCM WAV 파일로 디코딩합니다.
-   [**opusinfo**](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusinfo.html){: external} 유틸리티는 Opus 파일에 대한 정보와 유효성 검사를 제공합니다.

많은 사용자가 음성 인식을 위해 WAV 파일을 전송합니다. 이 서비스에는 동기 HTTP 및 WebSocket 요청에 대한 100MB 데이터 한계가 있으므로 WAV 형식은 단일 요청으로 인식될 수 있는 오디오의 양을 줄입니다. **opusenc** 명령을 사용하여 오디오를 선호하는 `audio/ogg:codecs=opus` 형식으로 변환하면 인식 요청과 함께 전송할 수 있는 오디오의 양이 크게 증가할 수 있습니다.

예를 들어, 256kps의 비트 전송률로 샘플당 16비트를 사용하는 비압축 광대역(16kHz) WAV 파일(**input.wav**)을 고려하십시오. 다음 명령은 Opus 코덱을 사용하는 파일(**output.opus**)로 오디오를 변환합니다.

```bash
opusenc input.wav output.opus
```
{: pre}

변환 시 오디오가 4배만큼 압축되고 64kbps의 비트 전송률로 출력 파일이 생성됩니다. 그러나 [Opus 권장 설정](https://wiki.xiph.org/Opus_Recommended_Settings){: external}에 따라 안전하게 비트 전송률을 24kbps로 줄이고 음성 오디오에 대해 전체 대역을 유지할 수 있습니다. 이와 같이 줄이면 오디오가 10배만큼 압축됩니다. 다음 명령은 `--bitrate` 옵션을 사용하여 24kbps의 비트 전송률로 출력 파일을 생성합니다.

```bash
opusenc --bitrate 24 input.wav output.opus
```
{: pre}

**opusenc** 유틸리티를 사용한 압축이 빠릅니다. 압축은 실제 시간보다 약 100배 더 빠른 속도로 발생합니다. 이 명령이 완료되면 실행 시간 및 결과 오디오 데이터에 대한 전체 세부사항을 제공하는 출력을 콘솔에 기록합니다.

## 음성 인식을 개선하기 위한 팁
{: #audioTips}

다음 팁은 음성 인식의 품질을 개선하는 데 도움이 될 수 있습니다.

-   오디오를 녹음하는 방법에 따라 서비스의 결과에 큰 차이가 발생할 수 있습니다. 음성 인식은 입력 오디오 품질에 매우 민감할 수 있습니다. 가능한 최상의 정확도를 얻으려면 입력 오디오 음질이 가능한 한 양호한지 확인하십시오.
    -   가능하면 가까운 음성 지향 마이크(예: 헤드셋)을 사용하고 필요한 경우 마이크 설정을 조정하십시오. 이 서비스는 전문 마이크를 사용하여 오디오를 캡처할 때 가장 잘 수행됩니다.
    -   시스템의 기본 제공 마이크를 사용하지 마십시오. 일반적으로 모바일 디바이스 및 태블릿에 설치되어 있는 마이크는 적절하지 않습니다.
    -   스피커가 마이크와 가까운지 확인하십시오. 스피커가 마이크에서 멀어질수록 정확도가 떨어집니다. 예를 들어, 10피트 거리에서는 이 서비스가 적절한 결과를 생성하는 데 어려움이 있습니다.
-   음성 인식은 주변 소음과 사람 음성의 미묘한 차이에 민감합니다.
    -   엔진 소음, 작동 중인 디바이스, 거리의 소음, 주변 대화는 인식 정확도를 크게 떨어뜨릴 수 있습니다.
    -   지역별 사투리나 발음의 차이도 정확도를 낮출 수 있습니다.

    오디오에 이러한 특성이 있는 경우 음성 인식의 정확도를 높이려면 음향 모델 사용자 정의 사용을 고려하십시오. 자세한 정보는 [사용자 정의 인터페이스](/docs/services/speech-to-text?topic=speech-to-text-customization)를 참조하십시오.
