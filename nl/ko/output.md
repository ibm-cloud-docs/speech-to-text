---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

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

# 출력 기능
{: #output}

{{site.data.keyword.speechtotextshort}} 서비스는 이 서비스가 음성 인식 요청에 대한 변환 결과에 포함할 정보를 표시하기 위해 다음과 같은 기능을 제공합니다. 모든 출력 매개변수는 선택사항입니다.
{: shortdesc}

-   각 서비스 인스턴스에 대한 단순 음성 인식 요청의 예제는 [인식 요청 작성](/docs/services/speech-to-text/basic-request.html)을 참조하십시오.
-   음성 인식 응답의 예제 및 설명은 [인식 결과 이해](/docs/services/speech-to-text/basic-response.html)를 참조하십시오. 이 서비스는 모든 JSON 응답 컨텐츠를 UTF-8 문자 세트로 리턴합니다.
-   상태(GA 또는 베타) 및 지원되는 언어를 포함한 사용 가능한 모든 음성 인식 매개변수의 알파벳순 목록은 [매개변수 요약](/docs/services/speech-to-text/summary.html)을 참조하십시오.

## 화자 레이블
{: #speaker_labels}

화자 레이블 기능은 미국 영어, 일본어, 스페인어(광대역 및 협대역 모델 모두) 및 영국 영어(협대역 모델 전용)에 사용할 수 있는 베타 기능입니다.
{: note}

화자 레이블은 다중 참여자 대화에서 어떤 개인이 어떤 단어를 말했는지를 식별합니다. (누가 언제 말했는지에 대한 레이블 지정을 *화자 분할(speaker diarization)*이라고도 합니다.) 이 정보를 사용하여 콜센터 문의와 같은 오디오 스트림의 개인별 대화 내용을 개발할 수 있습니다. 또는 이를 사용하여 대화형 로봇 또는 아바타와의 대화를 애니메이션으로 만들 수 있습니다. 최상의 성능을 위해서는 1분 이상 길이의 오디오를 사용하십시오. 

화자 레이블은 2명의 화자 시나리오에 최적화되어 있습니다. 확장된 대화에 두 사람이 참여하는 전화 통화에 가장 적합합니다. 최대 6명의 화자를 처리할 수 있지만 화자가 3명 이상이면 성능이 가변적일 수 있습니다. 2명 간의 대화는 일반적으로 협대역 매체를 통해 수행되지만 지원되는 협대역 및 광대역 모델에 화자 레이블을 사용할 수 있습니다.

이 기능을 사용하려면 인식 요청에 대해 `speaker_labels` 매개변수를 `true`로 설정합니다. 기본적으로 이 매개변수는 `false`입니다. 서비스는 오디오의 개별 단어로 화자를 식별합니다. 단어의 시작 및 종료 시간에 의존하여 해당 화자를 식별합니다. 따라서 화자 레이블을 사용으로 설정하면 `timestamps` 매개변수도 강제로 `true`가 됩니다([단어 시간소인](/docs/services/speech-to-text/output.html#word_timestamps) 참조).

### 화자 레이블 예제
{: #speakerLabelsExample}

다음 에제 요청은 화자 레이블이 포함된 응답을 보여줍니다. `시간소인` 배열의 각 요소와 연관된 숫자 값은 오디오에서 단어의 시작 및 종료 시간입니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-multi.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel&speaker_labels=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "hello",
              0.68,
              1.19
            ],
            [
              "yeah",
              1.47,
              1.93
            ],
            [
              "yeah",
              1.96,
              2.12
            ],
            [
              "how's",
              2.12,
              2.59
            ],
            [
              "Billy",
              2.59,
              3.17
            ],
            . . .
          ]
          "confidence": 0.82,
          "transcript": "hello yeah yeah how's Billy "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0,
  "speaker_labels": [
    {
      "from": 0.68,
      "to": 1.19,
      "speaker": 2,
      "confidence": 0.42,
      "final": false
    },
    {
      "from": 1.47,
      "to": 1.93,
      "speaker": 1,
      "confidence": 0.52,
      "final": false
    },
    {
      "from": 1.96,
      "to": 2.12,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.12,
      "to": 2.59,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.59,
      "to": 3.17,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    . . .
  ]
}
```
{: codeblock}

`transcript` 필드는 모든 참가자가 말한 단어를 나열하는 오디오의 최종 음성 내용을 표시합니다. 화자 레이블을 시간소인과 비교하고 동기화하여 발생한 대화를 리어셈블할 수 있습니다.

<table style="width:50%">
  <caption>표 1. 화자 레이블 예제</caption>
  <tr>
    <th style="text-align:left">시간소인</th>
    <th style="text-align:left">화자 레이블</th>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"hello",<br/>
      0.68,<br/>
      1.19</code>
    </td>
    <td>
      <code>"from": 0.68,<br/>
      "to": 1.19,<br/>
      "speaker": 2,<br/>
      "confidence": 0.42,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.47,<br/>
      1.93</code>
    </td>
    <td>
      <code>"from": 1.47,<br/>
      "to": 1.93,<br/>
      "speaker": 1,<br/>
      "confidence": 0.52,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.96,<br/>
      2.12</code>
    </td>
    <td>
      <code>"from": 1.96,<br/>
      "to": 2.12,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"how's",<br/>
      2.12,<br/>
      2.59</code>
    </td>
    <td>
      <code>"from": 2.12,<br/>
      "to": 2.59,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"Billy",<br/>
      2.59,<br/>
      3.17</code>
    </td>
    <td>
      <code>"from": 2.59,<br/>
      "to": 3.17,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
</table>

이 결과는 이 두 사람 간의 대화가 다음으로 시작되었다는 것을 명확히 보여줍니다.

-   **화자 2** - "Hello?"
-   **화자 1** - "Yeah?"
-   **화자 2** - "Yeah, how's Billy?"

이 예제에서 `confidence` 필드는 서비스가 화자를 식별하는 신뢰도를 표시합니다. 각 단어에 대한 `final` 필드의 값은 `false`입니다. 이 값은 서비스가 오디오를 여전히 처리 중임을 표시합니다. 서비스가 수행되기 전에 화자의 ID 또는 개별 단어에 대한 신뢰도를 변경할 수 있습니다.

### 화자 레이블에 대한 화자 ID
{: #speakerLabelsIDs}

이 예제는 또한 화자 ID의 흥미로운 측면을 보여줍니다. `speaker` 필드에서 첫 번째 화자의 ID는 `2`이며 두 번째 화자의 ID는 `1`입니다. 이 서비스는 오디오를 처리할 때 화자의 패턴을 더 잘 이해할 수 있도록 합니다. 따라서 개별 단어에 대한 화자 ID를 변경할 수 있으며 최종 결과를 생성할 때까지 화자를 추가하거나 제거할 수도 있습니다.

결과적으로, 화자 ID는 순차적이거나 연속적이거나 순서가 지정되지 않을 수 있습니다. 예를 들어, ID는 일반적으로 `0`에서 시작하지만 이전 예제에서 가장 빠른 ID는 `1`입니다. 서비스가 추가 오디오 분석을 기반으로 화자 `0`에 대한 이전 단어 지정을 생략했을 수 있습니다. 이후 화자에게도 생략이 발생할 수 있습니다. 생략으로 인해 번호 매기기에 간격이 발생하거나 레이블 지정된(예: `0` 및 `2`) 두 명의 화자에 대한 결과가 생성될 수 있습니다. 또 다른 고려사항은 화자가 대화에서 떠날 수 있다는 것입니다. 따라서 대화의 초기 단계에만 기여하는 참가자가 이후 결과에 나타나지 않을 수도 있습니다.

### 화자 레이블에 대한 중간 결과 요청
{: #speakerLabelsInterim}

WebSocket 인터페이스를 사용하는 경우 화자 레이블 및 중간 결과를 요청할 수 있습니다([중간 결과](/docs/services/speech-to-text/output.html#interim) 참조). 일반적으로 최종 결과는 중간 결과보다 더 양호합니다. 그러나 중간 결과는 음성 내용의 전개 및 화자 레이블의 지정을 식별하는 데 도움이 될 수 있습니다. 중간 결과는 임시 화자 및 ID가 표시되거나 사라진 위치를 표시할 수 있습니다. 그러나 이 서비스는 처음에 식별하고 나중에 다시 고려하며 생략하는 화자의 ID를 재사용할 수 있습니다. 따라서 ID가 중간 및 최종 결과에서 서로 다른 두 명의 화자를 나타낼 수 있습니다.

중간 결과 및 화자 레이블을 모두 요청하는 경우 초기 중간 결과가 리턴된지 한참 후에 긴 오디오 스트림에 대한 최종 결과가 도착할 수 있습니다. 또한 일부 중간 결과에는 `results` 및 `result_index` 필드 없이 `speaker_labels` 필드만 포함될 수 있습니다. 중간 결과를 요청하지 않으면 서비스가 `results` 및 `result_index` 필드와 단일 `speaker_labels` 필드를 포함하는 최종 결과를 리턴합니다.

이 서비스는 오디오 스트림 완료 시 또는 제한시간 초과에 대한 응답으로(둘 중 먼저 발생하는 것을 기준으로 함) 최종 결과를 전송합니다. 두 경우 모두 서비스가 리턴하는 화자 레이블의 마지막 단어에 대한 `final` 필드만 `true`로 설정합니다.

### 화자 레이블에 대한 성능 고려사항
{: #speakerLabelsPerformance}

이전에 언급한 대로 화자 레이블 기능은 콜센터 의사소통과 같은 두 사람 간의 대화에 최적화되어 있습니다. 따라서 다음과 같은 잠재적 성능 문제를 고려해야 합니다.

-   단일 화자가 포함된 오디오에 대한 성능이 낮을 수 있습니다. 오디오 품질이나 화자의 음성이 변하면 이 서비스가 존재하지 사람을 추가 화자를 식별할 수 있습니다. 이러한 화자를 환청이라고도 합니다.
-   마찬가지로 팟캐스트와 같은 주도 화자가 있는 오디오의 경우 성능이 저하될 수 있습니다. 이 서비스는 더 짧은 시간 동안 말하는 화자를 놓치기 쉽고 환청을 일으킬 수도 있습니다.
-   6명보다 더 많은 화자가 있는 오디오의 성능은 정의되지 않습니다. 이 기능은 최대 6명의 화자를 처리할 수 있습니다.
-   짧은 발화에 대한 성능은 긴 발화보다 덜 정확할 수 있습니다. 이 서비스는 참가자가 더 오랜 시간 동안 말할 때 더 나은 결과를 생성합니다(화자당 30초 이상). 각 화자에 대해 사용 가능한 상대적인 오디오의 양도 성능에 영향을 줄 수 있습니다.
-   음성의 처음 30초 동안은 성능이 저하될 수 있습니다. 일반적으로 오디오의 1분 후에는 합리적인 레벨로 향상됩니다.

모든 변환과 마찬가지로, 성능은 낮은 오디오 품질, 주변의 소음, 사람의 말투, 겹치는 화자 및 기타 오디오의 측면에 영향을 받을 수도 있습니다.

## 키워드 발견
{: #keyword_spotting}

키워드 발견 기능은 변환에서 지정된 문자열을 발견합니다. 이 서비스는 동일한 키워드를 여러 번 발견하고 발생할 때마다 보고할 수 있습니다. 이 서비스는 중간 결과가 아니라 최종 결과에서만 키워드를 발견합니다. 기본적으로 이 서비스는 키워드 발견을 수행하지 않습니다.

키워드 발견을 사용하려면 다음 매개변수를 둘 다 지정해야 합니다.

-   `keywords` 매개변수를 사용하여 발견될 문자열의 배열을 지정합니다. 매개변수를 생략하거나 비어 있는 배열을 지정하면 서비스가 키워드를 발견하지 않습니다. 키워드 문자열에 둘 이상의 토큰이 포함될 수 있습니다. `Speech to Text` 키워드에는 세 개의 토큰이 있습니다.

    미국 영어의 경우 이 서비스는 음성 문자열 대 기록된 문자열과 일치하도록 각 키워드를 정규화합니다. 예를 들어, 기록하는 방식이 아니라 말하는 방식과 일치하도록 숫자를 정규화합니다. 다른 언어의 경우 말하는 방식대로 키워드를 지정해야 합니다.

    단일 요청으로 최대 1000개의 키워드를 발견할 수 있습니다. 키워드는 대소문자를 구분하지 않습니다.
-   `keywords_threshold` 매개변수를 사용하여 키워드 일치에 대해 0.0 - 1.0 사이의 확률을 지정합니다. 이 임계값은 단어가 키워드와 일치하려면 서비스가 보유해야 하는 신뢰수준의 하한을 나타냅니다. 신뢰도가 지정된 임계값보다 크거나 같은 경우에만 변환에서 키워드가 발견됩니다.

    작은 임계값을 지정하면 잠재적으로 많은 일치 항목이 생성될 수 있습니다. 임계값을 지정하는 경우 하나 이상의 키워드도 지정해야 합니다. 일치 항목을 리턴하지 않으려면 이 매개변수를 생략하십시오.

키워드 발견은 오디오 스트림에서 키워드를 식별하는 데 필요합니다. 변환은 서비스가 입력 오디오에 대해 수행한 최상의 디코딩 결과를 나타내기 때문에 최종 변환을 처리하여 키워드를 식별할 수 없습니다. 관심 단어를 나타낼 수 있는 신뢰도 점수가 더 낮은 토큰을 포함하지 않습니다. 따라서 클라이언트 측에서 변환에 텍스트 처리 도구를 적용해도 키워드가 식별되지 않을 수 있습니다. 디코딩 결과를 더 풍부하게 표시해야 하며 이러한 표현은 서버에서만 사용할 수 있습니다.

### 키워드 발견 결과
{: #keywordSpottingResults}

이 서비스는 `results` 배열의 요소인 `keywords_result` 필드에 결과를 리턴합니다. `keywords_result` 필드는 열거 가능 특성의 사전 또는 연관 배열입니다. 각 특성은 지정된 키워드로 식별되며 오브젝트 배열을 포함합니다. 이 서비스는 키워드에 대해 찾은 각 일치 항목에 대한 배열의 한 요소를 리턴합니다. 각 일치 항목에 대한 오브젝트에는 다음 필드가 포함됩니다.

-   `normalized_text`는 입력 오디오에서 일치하는 음성 구문으로 정규화되는 지정된 키워드입니다.
-   `start_time`은 일치 항목의 시작 시간(초)입니다.
-   `end_time`은 일치 항목의 종료 시간(초)입니다.
-   `confidence`는 일치 항목이 지정된 키워드를 표시하는 서비스의 신뢰도입니다. 신뢰도는 최소한 결과에 포함될 지정된 임계값만큼 커야 합니다. 

서비스에서 일치하는 항목을 찾을 수 없는 키워드는 배열에서 생략됩니다. 다음과 같은 경우 키워드를 찾을 수 없습니다.

-   오디오에 키워드가 포함되어 있지 않습니다. 키워드의 부재가 가장 명백한 설명입니다.
-   임계값이 너무 높게 설정되었습니다. 이 서비스는 신뢰수준이 낮은 키워드를 식별할 수 있지만, 이 경우 결과에서 일치 항목을 생략합니다.
-   여러 토큰이 포함된 키워드 문자열(예: `Speech to Text`)을 말할 때 토큰 사이에 너무 많은 무음이 있습니다. 서비스가 오디오를 기록할 때 스트림을 일련의 블록으로 자릅니다. 각 블록은 무음 간격이 0.5초를 초과하지 않는 연속 오디오 청크를 나타냅니다. 이러한 블록으로 구성된 결과 오브젝트의 배열을 생성합니다.

    서비스는 다음의 경우에만 다중 토큰 키워드와 일치합니다.

    -   키워드의 토큰이 동일한 블록에 있습니다.
    -   토큰이 서로 인접하거나 0.1초 이하의 간격으로 구분되어 있습니다.

    간단한 채움말 또는 어휘가 아닌 발화(예: "uhm" 또는 "well")가 키워드의 두 토큰 사이에 있으면 후자의 경우가 발생할 수 있습니다. 자세한 정보는 [망설임 표지(hesitation marker)](/docs/services/speech-to-text/basic-response.html#hesitation)를 참조하십시오.

### 키워드 발견 예제
{: #keywordSpottingExample}

다음 예제 요청은 `keywords` 매개변수를 URL로 인코딩된 세 개의 문자열(`colorado`, `tornado` 및 `tornadoes`) 배열로 설정하고 `keywords_threshold` 매개변수를 `0.5`로 설정합니다. 이 서비스는 `colorado` 및 `tornadoes`가 적격하게 발생했는지 찾습니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?keywords=%22colorado%22%2C%22tornado%22%2C%22tornadoes%22&keywords_threshold=0.5"
```
{: pre}

```javascript
{
  "results": [
    {
      "keywords_result": {
        "colorado": [
          {
            "normalized_text": "Colorado",
            "start_time": 4.94,
            "confidence": 0.91,
            "end_time": 5.62
          }
        ],
        "tornadoes": [
          {
            "normalized_text": "tornadoes",
            "start_time": 1.52,
            "confidence": 1.0,
            "end_time": 2.15
          }
        ]
      },
      "alternatives": [
        {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## 최대 대체 수
{: #max_alternatives}

`max_alternatives` 매개변수는 결과에 대한 *n*개의 최적의 대체 가설을 리턴하도록 서비스에 알리는 정수 값을 허용합니다. 기본적으로 이 서비스는 하나의 변환 결과만 리턴하며, 이는 이 매개변수를 `1`로 설정하는 것과 동등합니다. `max_alternatives`를 1보다 큰 수로 설정하면 최적의 대체 변환을 해당 수만큼 리턴하도록 서비스에 요청합니다. (`0` 값을 지정하면 이 서비스가 기본값인 `1`을 사용합니다.)

이 서비스는 리턴하는 최적의 대안에 대해서만 신뢰도 점수를 보고합니다. 대부분의 경우 이것이 선택할 수 있는 대안이 됩니다.

### 최대 대체 수 예제
{: #maximumAlternativesExample}

다음 예제 요청은 `max_alternatives` 매개변수를 `3`으로 설정합니다. 이 서비스는 세 가지 대안 중 가장 가능성이 높은 대안에 대한 신뢰도를 보고합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?max_alternatives=3"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touched down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touch down is a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## 중간 결과
{: #interim}

중간 결과 기능은 WebSocket 인터페이스에서만 사용할 수 있습니다.
{: note}

중간 결과는 서비스가 최종 결과를 리턴하기 전에 변경될 가능성이 있는 중간 버전의 음성 내용입니다. 이 서비스는 중간 결과를 생성하자마자 리턴합니다. 중간 결과는 다음의 경우에 유용합니다.

-   대화식 애플리케이션 및 실시간 변환
-   변환하는 데 다소 시간이 걸릴 수 있는 긴 오디오 스트림

중간 결과는 최종 결과보다 더 자주, 더 빠르게 도착합니다. 중간 결과를 사용하여 애플리케이션이 더 빠르게 응답하거나 변환 진행상태를 알아보도록 할 수 있습니다. 

중간 결과를 수신하려면 WebSocket 인식 요청에 대한 `start` 메시지에서 `interim_results` JSON 매개변수를 `true`를 설정하십시오. `interim_results` 매개변수를 생략하거나 `false`로 설정하면 서비스가 오디오 종료 시 단일 JSON 음성 내용만 리턴합니다. 이 매개변수를 사용하려면 다음 가이드라인을 따르십시오.

-   오프라인 또는 일괄처리 변환을 수행 중이며 최소 대기 시간이 필요하지 않고 모든 오디오에 대한 단일 JSON 결과를 원하는 경우에는 이 매개변수를 생략하거나 `false`로 설정하십시오.
-   서비스가 오디오를 처리할 때 계속해서 결과가 도착하도록 하거나 최소 대기 시간으로 결과를 얻으려는 경우 이 매개변수를 `true`로 설정하십시오. 이 서비스는 더 많은 오디오를 처리하면서 중간 결과를 업데이트할 수 있습니다.

중간 결과는 `final` 필드가 `false`로 설정된 JSON 응답에 표시됩니다. 이 서비스는 추가 오디오를 처리할 때 이러한 결과를 보다 정확한 변환으로 업데이트할 수 있습니다. `final` 필드가 `true`로 설정된 경우 최종 결과로 식별됩니다. 이 서비스는 최종 결과를 추가로 업데이트하지 않습니다.

### 중간 결과 예제
{: #interimResultsExample}

다음의 축약된 예제는 WebSocket 요청에 대한 중간 결과를 요청합니다. 서비스는 해당 응답에서 최종 결과에 대한 `final` 속성만 `true`로 설정합니다.

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token;
var websocket = new WebSocket(wsURI);

websocket.onopen = function(evt) { onOpen(evt) };
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050',
    interim_results: true
  };
  websocket.send(JSON.stringify(message));
  websocket.send(blob);
}

websocket.onmessage = function(evt) { onMessage(evt) };
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several to "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  . . .
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## 단어 대체
{: #word_alternatives}

단어 대체 기능(혼동 네트워크라고도 함)은 입력 오디오의 단어에 대한 음향적으로 유사한 대체 단어의 가설을 보고합니다. 예를 들어, `Austin`이라는 단어는 오디오의 단어에 대한 가장 좋은 가설이 될 수 있습니다. 그러나 `Boston`이라는 단어는 동일한 시간 간격에서 가능한 또 다른 가설입니다. 가설은 공통 시작 및 종료 시간을 공유하지만 맞춤법이 서로 다르며 일반적으로 신뢰도 점수가 서로 다릅니다.

기본적으로 이 서비스는 단어 대체를 보고하지 않습니다. 대체 가설을 수신하도록 표시하려면 `word_alternatives_threshold` 매개변수를 사용하여 0.0 - 1.0 사이의 확률을 지정합니다. 이 임계값은 서비스가 단어 대체로 리턴할 가설에서 보유해야 하는 신뢰수준의 하한을 나타냅니다. 가설은 신뢰도가 지정된 임계값보다 크거나 같은 경우에만 리턴됩니다.

단어 대체를 더 작은 간격 또는 구간으로 분할되는 변환에 대한 타임라인으로 생각할 수 있습니다. 각 구간에는 맞춤법 및 신뢰도가 다른 하나 이상의 가설이 있을 수 있습니다. `word_alternatives_threshold` 매개변수는 서비스가 리턴하는 결과를 밀도를 제어합니다. 작은 임계값을 지정하면 잠재적으로 많은 가설이 생성될 수 있습니다.

### 단어 대체 결과
{: #wordAlternativesResults}

이 서비스는 `results` 배열의 요소인 `word_alternatives` 필드에 결과를 리턴합니다. `word_alternatives` 필드는 각각 대체 단어에 대한 다음 필드를 제공하는 오브젝트의 배열입니다. 

-   `start_time`은 가설이 리턴된 단어가 입력 오디오에서 시작되는 시간(초)을 표시합니다.
-   `end_time`은 가설이 리턴된 단어가 입력 오디오에서 종료되는 시간(초)을 표시합니다.
-   `alternatives`는 가설 오브젝트의 배열입니다. 각 오브젝트에는 가설에 대한 서비스의 신뢰도 점수를 표시하는 `confidence` 및 가설을 식별하는 `word`가 포함됩니다. 신뢰도는 최소한 결과에 포함될 지정된 임계값만큼 커야 합니다. 서비스는 대체를 신뢰도에 따라 정렬합니다.

### 단어 대체 예제
{: #wordAlternativesExample}

다음 예제 요청은 `word_alternatives_threshold`를 `0.9`로 지정합니다. 출력에는 시작 및 종료 시간과 함께 여러 단어의 잠재적인 가설과 신뢰도 점수가 포함됩니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.9"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 1.01,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "several"
            }
          ],
          "end_time": 1.52
        },
        {
          "start_time": 1.52,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "tornadoes"
            }
          ],
          "end_time": 2.15
        },
        {
          "start_time": 3.39,
          "alternatives": [
            {
              "confidence": 0.96,
              "word": "severe"
            }
          ],
          "end_time": 3.77
        },
        {
          "start_time": 3.77,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "thunderstorms"
            }
          ],
          "end_time": 4.51
        },
        {
          "start_time": 4.51,
          "alternatives": [
            {
              "confidence": 0.97,
              "word": "swept"
            }
          ],
          "end_time": 4.81
        }
      ],
      "alternatives": [
        {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## 단어 신뢰도
{: #word_confidence}

`word_confidence` 매개변수는 음성 내용의 단어에 대한 신뢰도 측정값을 제공할지 여부를 서비스에 알립니다. 기본적으로 이 서비스는 최종 음성 내용 전체에 대한 신뢰도 측정값만 보고합니다. `word_confidence`를 `true`로 설정하면 서비스에 음성 내용의 각 개별 단어에 대한 신뢰도 측정값을 보고하도록 지시합니다.

신뢰도 측정값은 음향 증거를 기반으로 변환된 단어가 정확한지에 대한 서비스의 추정을 나타냅니다. 신뢰도 점수의 범위는 0.0 - 1.0입니다.

-   신뢰도 1.0은 단어의 현재 변환이 가장 가능성 있는 결과를 반영함을 표시합니다.
-   신뢰도 0.5는 단어가 정확할 가능성이 50%임을 의미합니다.

### 단어 신뢰도 예제
{: #wordConfidenceExample}

다음의 예제에서는 변환의 단어에 대한 단어 신뢰도 점수를 요청합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_confidence=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday ",
          "confidence": 0.89,
          "word_confidence": [
            [
              "several",
              1.0
            ],
            [
              "tornadoes",
              1.0
            ],
            [
              "touch",
              0.52
            ],
            [
              "down",
              0.90
            ],
            . . .
            [
              "on",
              0.31
            ],
            [
              "Sunday",
              0.99
            ]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## 단어 시간소인
{: #word_timestamps}

`timestamps` 매개변수는 변환하는 단어의 시간소인을 생성할지 여부를 서비스에 알립니다. 기본적으로 이 서비스는 시간소인을 보고하지 않습니다. `timestamps`를 `true`로 설정하면 오디오의 시작에 상대적인 각 단어의 시작 및 종료 시간(초)을 보고하도록 서비스에 지시합니다.

### 단어 시간소인 예제
{: #wordTimestampsExample}

다음 예제에서는 변환의 단어에 대한 시간소인을 요청합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?timestamps=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "several",
              1.01,
              1.52
            ],
            [
              "tornadoes",
              1.52,
              2.15
            ],
            [
              "touch",
              2.15,
              2.5
            ],
            [
              "down",
              2.5,
              2.81
            ],
            . . .
            [
              "on",
              5.62,
              5.74
            ],
            [
              "Sunday",
              5.74,
              6.34
            ]
          ],
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## 스마트 형식화
{: #smart_formatting}

스마트 형식화 기능은 미국 영어, 일본어 및 스페인어에 사용할 수 있는 베타 기능입니다.
{: note}

`smart_formatting` 매개변수는 다음 문자열을 더 일반적인 표시로 변환하도록 서비스에 지시합니다.

-   날짜
-   시간
-   일련의 숫자 및 번호
-   전화번호
-   통화 가치
-   인터넷 이메일 및 웹 주소

스마트 형식화를 사용으로 설정하려면 `smart_formatting` 매개변수를 `true`로 설정합니다. 기본적으로 이 서비스는 스마트 형식화를 수행하지 않습니다.

이 서비스는 인식 요청의 최종 음성 내용에만 스마트 형식화를 적용합니다. 텍스트 정규화가 완료되면 결과를 클라이언트에 리턴하기 직전에 스마트 형식화를 적용합니다. 변환을 통해 음성 내용을 더 쉽게 읽을 수 있으며 변환 결과의 사후 처리를 더 효과적으로 수행할 수 있습니다.

### 문장 부호(미국 영어)
{: #smartFormattingPunctuation}

미국 영어의 경우 이 기능은 오디오에서 문장 부호를 다음 음성 키워드 문자열로 대체하도록 서비스에 지시합니다.

<table style="width:50%">
  <caption>표 2. 스마트 형식화 문장 부호 키워드</caption>
  <tr>
    <th style="width:45%; text-align:left">키워드 문자열</th>
    <th style="text-align:center">결과 문장 부호</th>
  </tr>
  <tr>
    <td>
      쉼표
    </td>
    <td style="text-align:center">
      <code>,</code>
    </td>
  </tr>
  <tr>
    <td>
      마침표
    </td>
    <td style="text-align:center">
      <code>.</code>
    </td>
  </tr>
  <tr>
    <td>
      물음표
    </td>
    <td style="text-align:center">
      <code>?</code>
    </td>
  </tr>
  <tr>
    <td>
      느낌표
    </td>
    <td style="text-align:center">
      <code>!</code>
    </td>
  </tr>
</table>

이 서비스는 적절한 위치에 있는 경우에만 이러한 키워드 문자열을 기호로 변환합니다. 예를 들어, 이 서비스는 아래와 같은 음성 구문을 

```
the warranty period is short period
```
{: codeblock}

음성 내용에서 다음 텍스트로 변환합니다.

```
the warranty period is short.
```
{: codeblock}

### 언어 차이점
{: #smartFormattingDifferences}

스마트 형식화는 음성 내용에 명백한 키워드가 존재하는지를 기반으로 합니다. 지원되는 언어에서 스마트 형식화를 약간 다르게 처리합니다.

#### 미국 영어 및 스페인어
{: #smartFormattingEnglishSpanish}

-   시간은 `AM`, `PM` 또는 `EST`와 같은 키워드로 식별됩니다.
-   군사 시간은 `hours`라는 키워드로 식별되는 경우 변환됩니다.
-   전화번호는 `911` 또는 숫자 `1`로 시작하는 10 또는 11자리의 숫자여야 합니다.

#### 일본어
{: #smartFormattingJapanese}

-   인터넷 이메일 및 웹 주소는 변환되지 않습니다.
-   전화번호는 10자리 또는 11자리여야 하며 일본의 전화번호에 유효한 접두부로 시작해야 합니다. 예를 들어, 유효한 접두부에는 `03` 및 `090`이 있습니다.
-   영어 단어는 ASCII(*hankaku*) 문자로 변환됩니다. 예를 들어, <code>&#65321;&#65314;&#65325;</code>은 `IBM`이 됩니다.
-   충분한 컨텍스트가 사용 불가능한 경우 모호한 용어가 변환되지 않을 수 있습니다. 예를 들어, <code>&#19968;&#26178;</code> 및 <code>&#21313;&#20998;</code>이 시간을 나타내는지 여부가 명확하지 않습니다.
-   문장 부호는 스마트 형식화를 수행하는지 여부와 관계없이 동일하게 처리됩니다. 예를 들어, 확률 계산을 기반으로 <code>&#12459;&#12531;&#12510;</code> 또는 `,` 중 하나가 선택됩니다.

### 스마트 형식화 예제
{: #smartFormattingExample}

다음 예제에서는 `smart_formatting` 매개변수를 `true`로 설정하여 인식 요청에 대한 스마트 형식화를 요청합니다. 다음 섹션에서는 스마트 형식화가 요청의 결과에 미치는 영향을 보여줍니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?smart_formatting=true"
```
{: pre}

### 스마트 형식화 결과
{: #smartFormattingResults}

다음 표에서는 스마트 형식화를 적용하거나 적용하지 않은 최종 음성 내용의 예제를 보여줍니다. 음성 내용은 미국 영어 오디오를 기반으로 합니다.

<table summary="각 표제 행 다음에는 표제에서 식별된 요소에 대한 스마트 형식화의 영향을 보여주는 다중 예제 행이 나옵니다.">
  <caption>표 3. 스마트 형식화 예제 음성 내용</caption>
  <tr>
    <th id="without_formatting" style="width:45%; text-align:left">스마트 형식화 미적용 시</th>
    <th id="with_formatting" style="text-align:left">스마트 형식화 적용 시</th>
  </tr>
  <tr>
    <th id="Dates" colspan="2">
      <strong>날짜</strong>
    </th>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on ten oh six nineteen seventy
    </td>
    <td headers="Dates with_formatting">
      I was born on 10/6/1970
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on the ninth of December nineteen hundred
    </td>
    <td headers="Dates with_formatting">
      I was born on 12/9/1900
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      Today is June sixth
    </td>
    <td headers="Dates with_formatting">
      Today is June 6
    </td>
  </tr>
  <tr>
    <th id="Times" colspan="2">
      <strong>시간</strong>
    </th>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      The meeting starts at nine thirty AM
    </td>
    <td headers="Times with_formatting">
      The meeting starts at 9:30 AM
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      I am available at seven EST
    </td>
    <td headers="Times with_formatting">
      I am available at 7:00 EST
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      We meet at oh seven hundred hours
    </td>
    <td headers="Times with_formatting">
      We meet at 0700 hours
    </td>
  </tr>
  <tr>
    <th id="Numbers" colspan="2">
      <strong>숫자</strong>
    </th>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      The quantity is one million one hundred and one
    </td>
    <td headers="Numbers with_formatting">
      The quantity is 1000101
    </td>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      One point five is between one and two
    </td>
    <td headers="Numbers with_formatting">
      1.5 is between 1 and 2
    </td>
  </tr>
  <tr>
    <th id="phone_numbers" colspan="2">
      <strong>전화번호</strong>
    </th>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at nine one four two three seven one thousand
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 914-237-1000
    </td>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at one nine one four nine oh nine twenty six forty five
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 1-914-909-2645
    </td>
  </tr>
  <tr>
    <th id="currency_values" colspan="2">
      <strong>통화 가치</strong>
    </th>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      You owe me three thousand two hundred two dollars and sixty six
      cents
    </td>
    <td headers="currency_values with_formatting">
      You owe me $3202.66
    </td>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      The dollar rose to one hundred and nine point seven nine yen from
      one hundred and nine point seven two yen
    </td>
    <td headers="currency_values with_formatting">
      The dollar rose to 109.79 yen from 109.72 yen
    </td>
  </tr>
  <tr>
    <th id="internet_addresses" colspan="2">
      <strong>인터넷 이메일 및 웹 주소</strong>
    </th>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      My email address is john dot doe at foo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      My email address is john.doe at foo.com
    </td>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      I saw the story on yahoo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      I saw the story on yahoo.com
    </td>
  </tr>
  <tr>
    <th id="Combinations" colspan="2">
      <strong>조합</strong>
    </th>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      The code is zero two four eight one and the date of service
      is May fifth two thousand and one
    </td>
    <td headers="Combinations with_formatting">
      The code is 02481 and the date of service is 5/5/2001
    </td>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      There are forty seven links on Yahoo dot com now
    </td>
    <td headers="Combinations with_formatting">
      There are 47 links on Yahoo.com now
    </td>
  </tr>
</table>

### 긴 일시정지가 있는 변환 예제
{: #smartFormattingLongPauses}

발화에 충분히 긴 무음이 포함되어 있는 경우 이 서비스는 음성 내용을 두 개 이상의 최종 청크로 분할할 수 있습니다. 다음 예제에 표시된 대로 이는 형식화의 결과에 영향을 줍니다.

<table>
  <caption>표 4. 긴 일시정지에 대한 스마트 형식화 예제 음성 내용</caption>
  <tr>
    <th style="width:45%; text-align:left">오디오 음성</th>
    <th style="text-align:left">형식화된 변환 결과</th>
  </tr>
  <tr>
    <td>
      My phone number is nine one four five five seven three
      three nine two
    </td>
    <td>
      "My phone number is 914-557-3392"
    </td>
  </tr>
  <tr>
    <td>
      My phone number is nine one four <em>&lt;pause&gt;</em> five five
      seven three three nine two
    </td>
    <td>
      "My phone number is 914"<br/>
      "5573392"
    </td>
  </tr>
</table>

## 숫자 교정
{: #redaction}

숫자 교정 기능은 미국 영어, 일본어 및 한국어에 사용할 수 있는 베타 기능입니다.
{: note}

`redaction` 매개변수는 최종 음성 내용에서 숫자 데이터를 교정하거나 마스킹하도록 서비스에 지시합니다. 이 기능은 각 숫자를 `X` 문자로 대체하여 세 개 이상의 연속 숫자가 포함된 번호를 교정합니다. 이 기능은 신용카드 번호와 같은 민감한 숫자 데이터를 교정하기 위한 것입니다.

기본적으로 이 서비스는 숫자 데이터를 교정하지 않습니다. 숫자 교정을 사용으로 설정하려면 `redaction` 매개변수를 `true`로 설정하십시오. 교정을 사용으로 설정하면 스마트 기능을 명시적으로 사용 안함으로 설정했는지 여부에 관계없이 이 서비스가 자동으로 스마트 기능을 사용으로 설정합니다. 최대 보안을 보장하기 위해 교정을 사용으로 설정한 경우 이 서비스가 다음 매개변수 값도 적용합니다.

-   이 서비스는 `keywords` 및 `keywords_threshold` 매개변수에 대한 값을 지정하는지 여부와 관계없이 키워드 발견을 사용 안함으로 설정합니다.
-   이 서비스는 WebSocket 인터페이스의 `interim_results` 매개변수를 `true`로 설정했는지 여부와 관계없이 중간 결과를 사용 안함으로 설정합니다.
-   이 서비스는 `maximum_alternatives` 매개변수에 대한 값을 지정하는지 여부와 관계없이 하나의 최종 음성 내용만 리턴합니다.

이 기능의 디자인은 기존의 스마트 형식화 기능과 유사합니다. 이 서비스는 클라이언트에 결과를 리턴하기 직전 및 텍스트 정규화가 완료된 후 인식 요청의 최종 음성 내용에만 교정을 적용합니다.

이 기능의 향후 버전에서는 모든 전화번호, 주소, 은행 계좌, 주민등록번호 및 기타 민감한 숫자 데이터를 마스킹하도록 교정을 확장할 수 있습니다.
{: note}

### 언어 차이점
{: #redactionDifferences}

이 기능은 미국 영어 모델에 대해 설명된 대로 정확히 작동하지만 일본어 및 한국어 모델에는 다음과 같은 차이점이 있습니다.

#### 일본어
{: #redactionJapanese}

일본어 교정에는 다음과 같은 차이점이 있습니다.

-   교정은 3자리 이상의 연속 숫자가 있는 문자열을 마스킹하는 것 이외에 3자리 미만의 숫자를 포함하는지 여부에 관계없이 주소 및 숫자를 마스킹합니다.
-   마찬가지로 교정은 일본식 생년월일의 날짜 정보를 마스킹합니다. 일본어에서는 날짜 정보가 일반적으로 라틴어 *Anno Domini*(서기) 형식으로 표시되지만 특히 생년월일의 경우에는 일본식을 따르기도 합니다. 이 경우, 연도 및 월은 하나 또는 두 자리 숫자를 포함하는 경우에도 마스킹됩니다. 예를 들어, 숫자 교정은 다음 문자열을 표시된 대로 변경합니다.

    <table style="width:50%">
      <caption>표 5. 일본식 생년월일의 교정 예제</caption>
      <tr>
        <th style="text-align:left">교정 미적용 시</th>
        <th style="text-align:left">교정 적용 시</th>
      </tr>
      <tr>
        <td>
          &#24179;&#25104; 30&#24180; 2&#26376;
        </td>
        <td>
          &#24179;&#25104; XX&#24180; X&#26376;
        </td>
      </tr>
    </table>

#### 한국어
{: #redactionKorean}

한국어 교정에는 다음과 같은 차이점이 있습니다.

-   스마트 형식화 기능이 지원되지 않습니다. 이 서비스는 한국어에 대해서도 숫자 교정을 수행하지만 다른 스마트 형식화는 수행하지 않습니다.
-   격리된 숫자 문자는 축소되지만 한국어 구문의 일부로 포함될 수 있는 격리된 숫자 문자는 그렇지 않습니다. 예를 들어, 다음 구문의 <code>&#51060;</code> 문자는 다음 문자와 인접해 있으므로 `X`로 대체되지 않습니다.

    <code>&#51060;&#51077;&#45768;&#45796;</code>

    <code>&#51060;</code> 문자가 다음 문자와 공백으로 분리된 경우에는 [숫자 교정 결과](#redactionResults)에 설명된 대로 `X`로 대체될 수 있습니다.

### 숫자 교정 예제
{: #redactionExample}

다음 예제에서는 `redaction` 매개변수를 `true`로 설정하여 인식 요청에 대한 숫자 교정을 요청합니다. 이 요청은 교정을 사용하므로 이 서비스가 내재적으로 요청에 스마트 형식화를 사용합니다. 이 서비스는 요청의 다른 매개변수를 효과적으로 사용 안함으로 설정하므로 이러한 매개변수는 적용되지 않습니다. 이 서비스는 단일 최종 음성 내용을 리턴하고 키워드를 인식하지 않습니다. 다음 섹션에서 교정의 영향을 보여줍니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?&redaction=true&max_alternatives=3&keywords=%22birth%22%2C%22birthday%22&keywords_threshold=0.5"
```
{: pre}

### 숫자 교정 결과
{: #redactionResults}

다음 표에서는 지원되는 각 언어에서 숫자 교정을 적용하거나 적용하지 않은 최종 음성 내용의 예제를 보여줍니다.

<table style="width:90%" summary="각 표제 행은 언어를 식별하며 숫자 교정이 사용 안함 및 사용으로 설정된 동일한 음성 내용을 보여주는 단일 행이 다음에 나옵니다.">
  <caption>표 6. 숫자 교정 예제 음성 내용</caption>
  <tr>
    <th id="without_redaction" style="text-align:left">교정 적용 안함</th>
    <th id="with_redaction" style="text-align:left">교정 적용</th>
  </tr>
  <tr>
    <th id="US_English" colspan="2">
      <strong>미국 영어</strong>
    </th>
  </tr>
  <tr>
    <td headers="US_English without_redaction">
      my credit card number is four one four seven two
    </td>
    <td headers="US_English with_redaction">
      my credit card number is XXXXX
    </td>
  </tr>
  <tr>
    <th id="Japanese" colspan="2">
      <strong>일본어</strong>
    </th>
  </tr>
  <tr>
    <td headers="Japanese without_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; &#22235; &#19968; &#22235; &#19971; &#20108;&#12391;&#12377;
    </td>
    <td headers="Japanese with_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; XXXXX &#12391;&#12377;
    </td>
  </tr>
  <tr>
    <th id="Korean" colspan="2">
      <strong>한국어</strong>
    </th>
  </tr>
  <tr>
    <td headers="Korean without_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; &#49324; &#51068; &#49324; &#52832; &#51060; &#48264;&#51077;&#45768;&#45796;
    </td>
    <td headers="Korean with_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; XXXXX &#48264;&#51077;&#45768;&#45796;
    </td>
  </tr>
</table>

## 욕설 필터링
{: #profanity_filter}

욕설 필터링 기능은 일반적으로 미국 영어에만 사용할 수 있습니다.
{: note}

`profanity_filter` 매개변수는 서비스가 결과에서 욕설을 검열할지 여부를 표시합니다. 기본적으로 이 서비스는 음성 내용에서 욕설을 일련의 별표로 대체하여 모든 욕설을 숨깁니다. 이 매개변수를 `false`로 설정하면 단어가 정확히 기록된 대로 출력에 표시됩니다.

이 서비스는 모든 최종 음성 내용 및 대체 음성 내용에서 욕설을 검열합니다. 또한 단어 대체, 단어 신뢰도 및 단어 시간소인과 연관된 결과에서 욕설을 검열합니다. 유일한 예외는 키워드 발견입니다. 이 기능의 경우 `profanity_filter`가 `true`인지 여부에 관계없이 서비스가 모든 단어를 사용자가 지정한 대로 리턴합니다.

### 욕설 필터링 예제
{: #profanityFilteringExample}

다음 예제는 `profanity_filter` 매개변수가 기본값인 `true` 값으로 설정된 상태로 변환된 간략한 오디오 파일에 대한 결과를 보여줍니다. 또한 이 요청은 `word_alternatives_threshold` 매개변수를 비교적 높은 값인 `0.99`로 설정하고 `word_confidence` 및 `timestamps` 매개변수를 `true`로 설정합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.99&word_confidence=true&timestamps=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 0.03,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "****"
            }
          ],
          "end_time": 0.25
        },
        {
          "start_time": 0.25,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "you"
            }
          ],
          "end_time": 0.56
        }
      ],
      "alternatives": [
        {
          "transcript": "**** you",
          "confidence": 0.99,
          "word_confidence": [
            ["****", 1.0],
            ["you", 0.99]
          ],
          "timestamps": [
            ["****", 0.03, 0.25],
            ["you", 0.25, 0.56]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}
