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

# 인식 결과 이해
{: #basic-response}

사용하는 인터페이스와 관계없이 {{site.data.keyword.speechtotextfull}} 서비스는 사용자가 지정하는 입력 및 출력 매개변수를 반영하는 변환 결과를 리턴합니다. 이 서비스는 모든 JSON 응답 컨텐츠를 UTF-8 문자 세트로 리턴합니다.
{: shortdesc}

## 기본 변환 응답
{: #response}

이 서비스는 [인식 요청 작성](/docs/services/speech-to-text/basic-request.html)의 예제에 대해 다음과 같은 응답을 리턴합니다. 이 예제에서는 오디오 파일과 해당 컨텐츠 유형만 전달합니다. 이 오디오는 단어 사이에 뚜렷한 일시정지가 없는 하나의 문장을 말합니다.

```javascript
{
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

이 서비스는 최상위 레벨 응답 오브젝트인 `SpeechRecognitionResults` 오브젝트를 리턴합니다. 이러한 단순 요청의 경우 오브젝트에 하나의 `results` 필드와 하나의 `result_index` 필드가 포함됩니다.

-   `results` 필드는 변환 결과에 대한 정보의 배열을 제공합니다. 이 예제에서는 `alternatives` 필드가 `transcript` 및 서비스의 `confidence`를 결과에 포함합니다. 이러한 결과가 변경되지 않음을 표시하기 위해 `final` 필드의 값은 `true`입니다. 
-   `result_index` 필드는 결과에 대한 고유 ID를 제공합니다. 이 예제는 일시정지가 없는 단일 오디오 파일이 포함된 요청에 대한 최종 결과를 표시하며 요청에 추가 매개변수가 없습니다. 따라서 이 서비스는 항상 초기 인덱스인 `0` 값을 가지는 단일 `result_index` 필드를 리턴합니다.

입력 오디오가 더 복잡하거나 요청에 추가 매개변수가 포함되어 있는 경우 결과에 더 많은 정보가 포함될 수 있습니다.

### alternatives 필드
{: #responseAlternatives}

`alternatives` 필드는 변환 결과의 배열을 제공합니다. 이 요청의 경우 배열에는 하나의 요소가 포함됩니다.

-   `confidence` 필드는 음성 내용에 대한 서비스의 신뢰도를 표시하는 점수이며, 이 예제에서는 90%에 근접합니다.
-   `transcript` 필드는 변환 결과를 제공합니다.

`final` 및 `result_index` 필드는 이러한 필드의 의미를 규정합니다.

### final 필드
{: #responseFinal}

`final` 필드는 음성 내용이 최종 변환 결과를 표시하는지 여부를 나타냅니다.

-   변경되지 않도록 보장되는 최종 결과의 경우에는 이 필드가 `true`입니다. 이 서비스는 최종 결과로 리턴하는 음성 내용에 대한 추가 업데이트를 전송하지 않습니다.
-   변경될 수 있는 중간 결과의 경우에는 이 필드가 `false`입니다. WebSocket 인터페이스에서 `interim_results` 매개변수를 사용하는 경우 이 서비스가 오디오를 변환할 때 여러 `results` 필드의 양식으로 진화하는 중간 가설을 리턴합니다. 중간 결과의 경우 `final` 필드는 항상 `false`입니다. 오디오에 대한 최종 결과의 경우 서비스가 이 필드를 `true`로 설정합니다. 이 서비스는 해당 오디오의 변환에 대한 추가 업데이트를 전송하지 않습니다.

중간 결과를 얻으려면 WebSocket 인터페이스를 사용하고 `interim_results` 매개변수를 `true`로 설정하십시오. 자세한 정보는 [중간 결과](/docs/services/speech-to-text/output.html#interim)를 참조하십시오.

### result_index 필드
{: #responseResultIndex}

`result_index` 필드는 해당 요청에 대해 고유한 결과의 ID를 제공합니다. 중간 결과를 요청하는 경우 이 서비스는 입력 오디오의 진화하는 가설에 대해 여러 `results` 필드를 전송합니다. 동일한 오디오에 대한 최종 결과와 마찬가지로 동일한 오디오에 대한 중간 결과의 인덱스 값은 항상 동일합니다.

오디오에 대한 최종 결과를 수신한 후에는 이 서비스가 해당 인덱스를 사용하여 나머지 요청에 대한 추가 결과를 전송하지 않습니다. 추가 결과에 대한 인덱스는 1씩 증가합니다.

중간 결과를 요청하는지 여부에 관계없이 오디오에 일시정지 또는 연장된 무음 구간이 포함된 경우 서로 다른 인덱스를 사용하여 여러 최종 결과를 리턴할 수 있습니다. 자세한 정보는 [일시정지 및 무음](#pauses-silence)을 참조하십시오.

오디오가 여러 최종 결과를 생성하는 경우 최종 결과의 `transcript` 요소를 병합하여 오디오의 전체 변환을 어셈블하십시오. `result_index`를 기준으로 결과를 어셈블하십시오. `final` 필드가 `false`인 중간 결과는 무시할 수 있습니다.

### 추가 응답 컨텐츠
{: #responseAdditional}

음성 인식에 사용 가능한 많은 출력 매개변수는 서비스 응답의 컨텐츠에 영향을 줍니다. 예를 들어, 다음 매개변수를 사용하면 서비스가 오디오에 대한 추가 정보를 리턴합니다.

-   `max_alternatives` 매개변수는 여러 대체 음성 내용을 요청합니다. `alternatives` 배열에는 각 대안마다 별도의 요소가 포함됩니다. 배열의 각 요소에는 `transcript` 필드가 포함됩니다. 가장 가능성이 높은 대안에만 `confidence` 필드가 포함됩니다.
-   `word_confidence` 및 `timestamps` 매개변수는 음성 내용의 개별 단어에 대한 추가 정보를 요청합니다. `alternatives` 배열에는 해당 이름의 추가 필드가 포함됩니다.
-   `keywords` 및 `keywords_threshold` 매개변수는 키워드 발견을 요청합니다. `results` 배열에는 `keywords_result` 필드가 포함됩니다.
-   `word_alternatives_threshold` 매개변수는 개별 단어에 대한 음향적으로 유사한 결과를 요청합니다. `results` 배열에는 `word_alternatives` 필드가 포함됩니다.
-   WebSocket 인터페이스의 `interim_results` 매개변수는 중간 변환 가설을 요청합니다. 이전에 언급한 대로 이 서비스는 오디오를 변환할 때 여러 응답을 전송합니다.
-   `speaker_labels` 매개변수는 다중 참여 대화의 개별 화자를 식별합니다. 응답에는 `results` 및 `results_index` 필드와 동일한 레벨의 `speaker_labels` 필드가 포함됩니다.

서비스의 응답에 영향을 줄 수 있는 이러한 매개변수 및 기타 매개변수에 대한 자세한 정보는 [출력 기능](/docs/services/speech-to-text/output.html)을 참조하십시오. 사용 가능한 모든 매개변수에 대한 자세한 정보는 [매개변수 요약](/docs/services/speech-to-text/summary.html)을 참조하십시오.

## 일시정지 및 무음
{: #pauses-silence}

이 서비스는 스트림이 종료되거나 제한시간 초과가 발생할 때까지 전체 오디오 스트림을 변환합니다. 그러나 오디오의 음성 단어 또는 구문 사이에 일시정지나 연장된 무음이 포함된 경우 인식 결과에 여러 최종 결과가 포함될 수 있습니다.

이 서비스에서 개별 최종 결과를 판별하는 데 사용하는 기본 일시정지 간격은 약 1초입니다. 대부분의 언어에서 일시정지 간격은 정확히 0.8초입니다. 중국어의 경우 0.6초입니다. 일시정지 간격의 지속 시간을 변경할 수 없습니다.

서비스가 결과를 리턴하는 방법은 사용하는 인터페이스에 따라 다릅니다. 다음 예제는 HTTP 및 WebSocket 인터페이스의 두 가지 최종 결과가 포함된 응답을 보여줍니다. 두 경우 모두에 동일한 입력 오디오가 사용됩니다. 이 오디오는 "three"와 "four" 사이에 1초 동안의 일시정지를 두고 "one two three four five six"라는 구문을 말합니다. 

-   *HTTP 인터페이스의 경우* 이 서비스가 단일 `SpeechRecognitionResults` 오브젝트를 전송합니다. `alternatives` 배열에는 각 최종 결과마다 별도의 요소가 포함됩니다. 응답에는 값이 `0`인 하나의 `result_index` 필드가 있습니다.

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
        {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

-   *WebSocket 인터페이스의 경우* 이 서비스가 두 개의 다른 `SpeechRecognitionResults` 오브젝트가 포함된 두 개의 개별 응답을 전송합니다. 각 응답 오브젝트에는 서로 다른 `result_index` 필드가 있습니다. 이 필드의 값은 첫 번째 응답의 경우 `0`이고 두 번째 응답의 경우 `1`입니다.

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
      ]
      "result_index": 0
    }
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
          "final": true
        }
      ],
      "result_index": 1
    }
    ```
    {: codeblock}

결과에 여러 최종 결과가 포함된 경우 최종 결과의 `transcript` 요소를 연결하여 오디오의 전체 변환을 어셈블하십시오.

스트리밍된 오디오에서 30초 동안 무음이 지속되면 [비활성 제한시간 초과](/docs/services/speech-to-text/input.html#timeouts-inactivity)가 발생할 수 있습니다.
{: note}

## 망설임 표지(hesitation marker)
{: #hesitation}

이 서비스는 음성에서 짧은 채움말 또는 일시정지를 발견하는 경우 음성에 망설임 표지(hesitation marker)를 포함할 수 있습니다. 눌변이라고도 하는 이러한 일시정지에는 "음", "어", "흠"과 같은 채움말 및 관련된 비어휘적인 발화가 포함될 수 있습니다. 망설임 표지(hesitation marker)를 애플리케이션에 사용할 필요가 없으면 음성 내용에서 안전하게 필터링할 수 있습니다.

영어에서는 이 서비스가 다음 예제에 표시된 대로 망설임 토큰 `%HESITATION`을 사용합니다. 기타 언어에서는 다른 표지를 사용할 수 있습니다. 예를 들어, 일본어에서는 `D_`라는 토큰을 사용합니다.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

망설임 표지(hesitation marker)는 음성 내용의 다른 필드에도 표시될 수 있습니다. 예를 들어, 음성 내용의 개별 단어에 대한 [단어 시간소인](/docs/services/speech-to-text/output.html#word_timestamps)을 요청하는 경우 이 서비스가 각 망설임 표지(hesitation marker)의 시작 및 종료 시간을 보고합니다.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            . . .
            [
              "that",
              7.31,
              7.69
            ],
            [
              "%HESITATION",
              7.69,
              7.98
            ],
            [
              "that's",
              7.98,
              8.41
            ],
            [
              "a",
              8.41,
              8.48
            ],
            . . .
          ],
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## 대문자 표시
{: #capitalization}

대부분의 언어의 경우 이 서비스는 응답 내용에서 대문자 표시를 사용하지 않습니다. 대문자 표시가 애플리케이션에 중요한 경우 각 문장의 첫 번째 단어와 대문자 표시가 적합한 기타 용어를 대문자로 표시해야 합니다.

*미국 영어의 경우에만* 이 서비스가 많은 고유 명사를 대문자로 표시합니다. 예를 들어, 이 서비스는 지정된 구문에 대해 다음 텍스트를 리턴합니다. 

```
Barack Obama graduated from Columbia University
```
{: codeblock}

다른 언어의 경우에는 서비스가 다음 텍스트를 리턴합니다.

```
barack obama graduated from columbia university
```
{: codeblock}

이 서비스는 스마트 형식화를 사용하는지 여부에 관계없이 항상 이 대문자 표시를 미국 영어에 적용합니다.

## 문장 부호
{: #punctuation}

이 서비스는 기본적으로 응답 음성 내용에 문장 부호를 삽입하지 않습니다. 서비스의 결과에 필요한 문장 부호를 추가해야 합니다.

미국 영어의 경우 스마트 형식화를 사용하여 특정 키워드 문자열에 대해 쉼표, 마침표, 물음표 및 느낌표와 같은 문장 부호를 대체하도록 서비스에 지시할 수 있습니다. 자세한 정보는 [스마트 형식화](/docs/services/speech-to-text/output.html#smart_formatting)를 참조하십시오.
