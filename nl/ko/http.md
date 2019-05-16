---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-08"

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

# 동기 HTTP 인터페이스
{: #http}

{{site.data.keyword.speechtotextfull}} 서비스의 동기 HTTP 인터페이스는 서비스로 음성 인식을 요청하기 위한 단일 `POST /v1/recognize` 메소드를 제공합니다. 이 메소드는 음성 내용을 얻는 가장 간단한 방법입니다. 이 메소드는 음성 인식 요청을 제출하는 두 가지 방법을 제공합니다.
{: shortdesc}

-   첫 번째는 요청 본문을 통해 모든 오디오를 단일 스트림에 전송합니다. 오퍼레이션의 매개변수를 요청 헤더 및 조회 매개변수로 지정합니다. 자세한 정보는 [기본 HTTP 요청 작성](#HTTP-basic)을 참조하십시오.
-   두 번째는 오디오를 다중 파트 요청으로 전송합니다. 요청 매개변수를 요청 헤더, 조회 매개변수 및 JSON 메타데이터의 조합으로 지정합니다. 자세한 정보는 [다중 파트 HTTP 요청 작성](#HTTP-multi)을 참조하십시오.

최대 100MB 및 최소 100바이트의 오디오 데이터를 단일 요청으로 제출하십시오. 오디오 형식에 대한 정보와 압축을 사용하여 요청과 함께 전송할 수 있는 오디오의 양을 최대화하는 방법에 대한 정보는 [오디오 형식](/docs/services/speech-to-text/audio-formats.html)을 참조하십시오. HTTP 인터페이스의 모든 메소드에 대한 정보는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/speech-to-text){: new_window}를 참조하십시오.

## 기본 HTTP 요청 작성
{: #HTTP-basic}

HTTP `POST /v1/recognize` 메소드는 오디오를 변환하는 간단한 방법을 제공합니다. 요청 본문을 통해 모든 오디오를 전달하고 매개변수를 요청 헤더 및 조회 매개변수로 지정합니다.

다음 `curl` 예제는 `audio-file.flac`이라는 단일 FLAC 파일에 대한 인식 요청을 전송합니다. 이 요청은 `model` 조회 매개변수를 생략하여 기본 언어 모델인 `en-US_BroadbandModel`을 사용하도록 합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

이 예제는 오디오에 대한 다음 변환을 리턴합니다.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

`POST /v1/recognize` 메소드는 요청에 대한 모든 오디오를 처리한 후에만 결과를 리턴합니다. 이 메소드는 일괄처리에 적합하지만 라이브 음성 인식에는 적합하지 않습니다. 라이브 오디오를 기록하려면 WebSocket 인터페이스를 사용하십시오.

데이터가 여러 오디오 파일로 구성된 경우 권장되는 오디오 제출 방법은 각 오디오 파일마다 하나씩, 여러 요청을 전송하는 것입니다. 루프로 요청을 제출할 수 있으며 선택적으로 병렬 처리를 통해 성능을 향상시킬 수 있습니다. 다중 파트 음성 인식을 사용하여 여러 오디오 파일을 단일 요청으로 전달할 수도 있습니다.

## 다중 파트 HTTP 요청 작성
{: #HTTP-multi}

`POST /v1/recognize` 메소드는 다중 파트 요청도 지원합니다. 모든 오디오 데이터를 다중 파트 양식 데이터로 전달합니다. 일부 매개변수를 요청 헤더 및 조회 매개변수로 전달하지만 JSON 메타데이터를 양식 데이터로 전달하여 대부분의 변환 측면을 제어합니다.

다중 파트 음성 인식은 다음 유스 케이스를 위한 것입니다.

-   단일 음성 인식 요청으로 여러 오디오 파일을 전달합니다.
-   JavaScript가 사용 안함으로 설정된 브라우저에서 사용할 경우. 양식 데이터를 기반으로 하는 다중 파트 요청에는 JavaScript를 사용할 필요가 없습니다.
-   인식 요청의 매개변수가 대부분의 HTTP 서버 및 프록시에서 부과되는 한계인 8KB보다 큰 경우. 예를 들어, 매우 큰 수의 키워드를 발견하면 요청의 크기가 이 한계를 초과하여 늘어날 수 있습니다. 다중 파트 요청은 이 제한조건을 피하기 위해 양식 데이터를 사용합니다.

다음 섹션에서는 다중 파트 요청에 사용하는 매개변수에 대해 설명하고 예제 요청을 표시합니다.

### 다중 파트 요청에 대한 매개변수
{: #multipartParameters}

다중 파트 음성 인식의 다음 매개변수를 요청 헤더, 조회 매개변수 및 양식 데이터로 지정합니다.

<table summary="표의 각 행에는 다중 파트 인식 요청에 가능한 하나의 매개변수 사용에 대해 설명되어 있습니다.">
  <caption>표 1. 다중 파트 요청에 대한 매개변수</caption>
  <tr>
    <th id="parameter" style="text-align:left; width:20%">매개변수</th>
    <th id="description" style="text-align:center; width:80%">설명</th>
  </tr>
  <tr>
    <td>
      <code>metadata</code>
      <br/><em>양식 데이터</em>
      <br/><em>오브젝트</em>
    </td>
    <td>
      <em>필수입니다.</em> 요청에 대한 변환 매개변수를 제공하는 JSON 오브젝트입니다. 오브젝트가 양식 데이터의 첫 번째 파트여야 합니다. 정보에 양식 데이터의 후속 파트에 있는 오디오에 대해 설명되어 있습니다. [다중 파트 요청에 대한 JSON 메타데이터](#multipartJSON)를 참조하십시오.
    </td>
  </tr>
  <tr>
    <td>
      <code>upload</code>
      <br/><em>양식 데이터</em>
      <br/><em>파일</em>
    </td>
    <td>
      <em>필수입니다.</em> 요청에 대한 양식 데이터의 나머지인 하나 이상의 오디오 파일입니다. 모든 오디오 파일의 형식이 동일해야 합니다. `curl` 명령을 사용하는 경우 요청의 각 파일마다 별도의 <code>--form</code> 옵션을 포함시키십시오.
    </td>
  </tr>
  <tr>
    <td>
      <code>Content-Type</code>
      <br/><em>헤더</em>
      <br/><em>문자열</em>
    </td>
    <td>
      <em>필수입니다.</em> 데이터가 메소드에 전달되는 방법을 표시하려면 `multipart/form-data`를 지정하십시오. JSON `part_content_type` 매개변수를 사용하여 오디오의 컨텐츠 유형을 지정합니다.
    </td>
  </tr>
  <tr>
    <td>
      <code>Transfer-Encoding</code>
      <br/><em>헤더</em>
      <br/><em>문자열</em>
    </td>
    <td>
      <em>선택사항입니다.</em> 오디오 데이터를 서비스에 스트리밍하려면 `chunked`를 지정하십시오. 모든 오디오를 단일 요청으로 전송하는 경우 이 매개변수를 생략하십시오.
    </td>
  </tr>
  <tr>
    <td>
      <code>model</code>
      <br/><em>조회</em>
      <br/><em>문자열</em>
    </td>
    <td>
      <em>선택사항입니다.</em> 요청에 사용될 모델의 ID입니다. 기본값은 `en-US_BroadbandModel`입니다.
    </td>
  </tr>
  <tr>
    <td>
      <code>language_customization_id</code>
      <br/><em>조회</em>
      <br/><em>문자열</em>
    </td>
    <td>
      <em>선택사항입니다.</em> 요청에 사용될 사용자 정의 언어 모델의 GUID입니다.
    </td>
  </tr>
  <tr>
    <td>
      <code>acoustic_customization_id</code>
      <br/><em>조회</em>
      <br/><em>문자열</em>
    </td>
    <td>
      <em>선택사항입니다.</em> 요청에 사용될 사용자 정의 음향 모델의 GUID입니다.
    </td>
  </tr>
  <tr>
    <td>
      <code>base_model_version</code>
      <br/><em>조회</em>
      <br/><em>문자열</em>
    </td>
    <td>
      <em>선택사항입니다.</em> 요청에 사용될 지정된 기본 모델의 버전입니다.
    </td>
  </tr>
</table>

조회 매개변수에 대한 자세한 정보는 [매개변수 요약](/docs/services/speech-to-text/summary.html)을 참조하십시오.

### 다중 파트 요청에 대한 JSON 메타데이터
{: #multipartJSON}

다중 파트 요청으로 전달하는 JSON 메타데이터에는 다음 필드가 포함될 수 있습니다.

-   `part_content_type`(문자열)
-   `data_parts_count`(정수)
-   `customization_weight`(숫자)
-   `inactivity_timeout`(정수)
-   `keywords`(문자열[])
-   `keywords_threshold`(숫자)
-   `max_alternatives`(정수)
-   `word_alternatives_threshold`(숫자)
-   `word_confidence`(부울)
-   `timestamps`(부울)
-   `profanity_filter`(부울)
-   `smart_formatting`(부울)
-   `speaker_labels`(부울)
-   `grammar_name`(문자열)
-   `redaction`(부울)

다음 두 개의 매개변수만 다중 파트 요청에 특정합니다.

-   `part_content_type` 필드는 대부분의 오디오 형식에 대해 *선택적*입니다. 이 필드는 `audio/alaw`, `audio/basic`, `audio/l16` 및 `audio/mulaw` 형식에 필요합니다. 이는 요청의 다음 파트에 있는 오디오의 형식을 지정합니다. 모든 오디오 파일의 형식이 동일해야 합니다.
-   `data_parts_count` 필드는 모든 요청에 대해 *선택적*입니다. 이 필드는 요청과 함께 전송된 오디오 파일의 수를 지정합니다. 서비스는 마지막(유일할 수 있음) 데이터 파트에 스트림 끝 발견을 적용합니다. 이 매개변수를 생략하면 서비스가 요청에서 파트 수를 판별합니다.

메타데이터의 기타 모든 매개변수는 선택적입니다. 사용 가능한 모든 매개변수에 대한 설명은 [매개변수 요약](/docs/services/speech-to-text/summary.html)을 참조하십시오.

### 예제 다중 파트 요청

다음 `curl` 예제는 `POST /v1/recognize` 메소드를 사용하여 다중 파트 인식 요청을 전달하는 방법을 보여줍니다. 요청이 두 개의 오디오 파일 **audio-file1.flac** 및 **audio-file2.flac**을 전달합니다. `metadata` 매개변수는 요청에 대한 대부분의 매개변수를 제공하며 `upload` 매개변수는 오디오 파일을 제공합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: multipart/form-data"
--form metadata="{\"part_content_type\":\"application/octet-stream\",
  \"data_parts_count\":2,
  \"timestamps\":true,
  \"word_alternatives_threshold\":0.9,
  \"keywords\":[\"colorado\",\"tornado\",\"tornadoes\"],
  \"keywords_threshold\":0.5}"
--form upload="@{path}audio-file1.flac"
--form upload="@{path}audio-file2.flac"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

이 예제는 오디오 파일에 대한 다음 변환을 리턴합니다. 서비스가 전송된 순서대로 두 개의 파일에 대한 결과를 리턴합니다. (이 예제 출력은 두 번째 파일의 결과를 축약한 것입니다.)

```javascript
{
   "results": [
      {
         "word_alternatives": [
            {
               "start_time": 0.03,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "the"
                  }
               ],
               "end_time": 0.09
            },
            {
               "start_time": 0.09,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "latest"
                  }
               ],
               "end_time": 0.62
            },
            {
               "start_time": 0.62,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "weather"
                  }
               ],
               "end_time": 0.87
            },
            {
               "start_time": 0.87,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "report"
                  }
               ],
               "end_time": 1.5
            }
         ],
         "keywords_result": {},
         "alternatives": [
            {
               "timestamps": [
                  [
                     "the",
                     0.03,
                     0.09
                  ],
                  [
                     "latest",
                     0.09,
                     0.62
                  ],
                  [
                     "weather",
                     0.62,
                     0.87
                  ],
                  [
                     "report",
                     0.87,
                     1.5
                  ]
               ],
               "confidence": 0.99,
               "transcript": "the latest weather report "
            }
         ],
         "final": true
      },
      {
         "word_alternatives": [
            {
               "start_time": 0.15,
               "alternatives": [
                  {
                     "confidence": 1.0,
                     "word": "a"
                  }
               ],
               "end_time": 0.3
            },
            {
               "start_time": 0.3,
               "alternatives": [
                  {
                     "confidence": 1.0,
                     "word": "line"
                  }
               ],
               "end_time": 0.64
            },
            . . .
            {
               "start_time": 4.58,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "Colorado"
                  }
               ],
               "end_time": 5.16
            },
            {
               "start_time": 5.16,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "on"
                  }
               ],
               "end_time": 5.32
            },
            {
               "start_time": 5.32,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "Sunday"
                  }
               ],
               "end_time": 6.04
            }
         ],
         "keywords_result": {
            "tornadoes": [
               {
                  "normalized_text": "tornadoes",
                  "start_time": 3.03,
                  "confidence": 0.98,
                  "end_time": 3.84
               }
            ],
            "colorado": [
               {
                  "normalized_text": "Colorado",
                  "start_time": 4.58,
                  "confidence": 0.98,
                  "end_time": 5.16
               }
            ]
         },
         "alternatives": [
            {
               "timestamps": [
                  [
                     "a",
                     0.15,
                     0.3
                  ],
                  [
                     "line",
                     0.3,
                     0.64
                  ],
                  . . .
                  [
                     "Colorado",
                     4.58,
                     5.16
                  ],
                  [
                     "on",
                     5.16,
                     5.32
                  ],
                  [
                     "Sunday",
                     5.32,
                     6.04
                  ]
               ],
               "confidence": 0.99,
               "transcript": "a line of severe thunderstorms with several
possible tornadoes is approaching Colorado on Sunday "
            }
         ],
         "final": true
      }
   ],
   "result_index": 0
}
```
{: codeblock}
