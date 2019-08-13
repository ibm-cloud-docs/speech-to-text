---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-21"

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

# 사용자 정의 언어 모델 작성
{: #languageCreate}

{{site.data.keyword.speechtotextshort}} 서비스의 사용자 정의 언어 모델을 작성하려면 다음 단계를 따르십시오.
{: shortdesc}

1.  [사용자 정의 언어 모델을 작성하십시오](#createModel-language). 동일하거나 다른 도메인에 대한 여러 사용자 정의 모델을 작성할 수 있습니다. 작성하는 모든 모델에 대한 프로세스는 동일합니다. 그러나 인식 요청에 사용하는 경우 하나의 사용자 정의 모델만 지정할 수 있습니다.
1.  [사용자 정의 언어 모델에 말뭉치를 추가하십시오](#addCorpus). 말뭉치는 컨텍스트에 따라 도메인의 용어를 사용하는 일반 텍스트 파일입니다. 서비스가 기본 어휘에 없는 용어를 말뭉치에서 추출하여 사용자 정의 모델에 대한 어휘를 빌드합니다. 여러 말뭉치를 사용자 정의 모델에 추가할 수 있습니다.
1.  [사용자 정의 언어 모델에 단어를 추가하십시오](#addWords). 사용자 정의 단어를 개별적으로 모델에 추가할 수도 있습니다. 또한 동일한 메소드를 사용하여 말뭉치에서 추출된 사용자 정의 단어를 수정할 수 있습니다. 메소드를 사용하여 단어의 발음과 단어가 음성 변환에 표시되는 방법을 지정할 수 있습니다.
1.  [사용자 정의 언어 모델을 훈련하십시오](#trainModel-language). 사용자 정의 모델에 단어를 추가한 후 단어에 대해 모델을 훈련해야 합니다. 훈련하면 사용자 정의 모델이 음성 인식에 사용할 준비가 됩니다. 모델은 훈련할 때까지 새 단어나 수정된 단어를 사용하지 않습니다.
1.  사용자 정의 모델을 훈련한 후 인식 요청에 사용할 수 있습니다. 변환을 위해 전달된 오디오가 사용자 정의 모델에 정의된 도메인 특정 단어를 포함하는 경우 요청의 결과에 모델의 향상된 어휘가 반영됩니다. 자세한 정보는 [사용자 정의 언어 모델 사용](/docs/services/speech-to-text?topic=speech-to-text-languageUse)을 참조하십시오.

사용자 정의 언어 모델을 작성하는 단계는 반복적입니다. 필요할 때마다 말뭉치를 추가하고, 단어를 추가하고, 모델을 훈련하거나 재훈련할 수 있습니다.

문법을 사용자 정의 언어 모델에 추가할 수도 있습니다. 문법은 서비스의 응답을 문법에서 인식되는 단어로만 제한합니다. 자세한 정보는 [사용자 정의 언어 모델에 문법 사용](/docs/services/speech-to-text?topic=speech-to-text-grammars)을 참조하십시오.

언어 모델 사용자 정의는 대부분의 언어에 대해 사용할 수 있습니다. 자세한 정보는 [사용자 정의에 대한 언어 지원](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)을 참조하십시오.
{: note}

## 사용자 정의 언어 모델 작성
{: #createModel-language}

`POST /v1/customizations` 메소드를 사용하여 새 사용자 정의 언어 모델을 작성합니다. 임의의 수의 사용자 정의 언어 모델을 작성할 수 있지만 음성 인식 요청에는 한 번에 하나의 모델만 사용할 수 있습니다. 이 메소드는 새 사용자 정의 모델의 속성을 요청의 본문으로 정의하는 JSON 오브젝트를 받아들입니다.

<table>
  <caption>표 1. 새 사용자 정의 언어 모델의 속성</caption>
  <tr>
    <th style="width:20%; text-align:left">매개변수</th>
    <th style="width:12%; text-align:center">데이터 유형</th>
    <th style="text-align:left">설명</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>필수</em></td>
    <td style="text-align:center">문자열</td>
    <td>
      새 사용자 정의 모델의 사용자 정의 이름입니다. 사용자 정의 모델의 도메인에 대해 설명하는 이름을 사용하십시오(예: <code>Medical custom model</code> 또는 <code>Legal custommodel</code>). 사용자가 소유하는 모든 사용자 정의 언어 모델 중에서 고유한 이름을 사용하십시오.
      사용자 정의 모델의 언어와 일치하는 현지화된 이름을 사용하십시오.
    </td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>필수</em></td>
    <td style="text-align:center">문자열</td>
    <td>
      새 사용자 정의 모델에서 사용자 정의할 언어 모델의 이름입니다. <code>GET /v1/models</code> 메소드에서 리턴하는 지원되는 언어 모델 중 하나를 지정하십시오. 새 모델은 이 모델이 사용자 정의하는 기본 모델을 통해서만 사용될 수 있습니다.
    </td>
  </tr>
  <tr>
    <td><code>dialect</code><br/><em>선택사항</em></td>
    <td style="text-align:center">문자열</td>
    <td>
      새 사용자 정의 모델에 사용될 지정된 언어의
      통용어입니다. 대부분의 언어에서는 통용어는 기본적으로 기본 모델의
      언어와 일치합니다. 예를 들어, `en-US`는 미국 영어 모델들 중
      하나에 사용됩니다. <br><br>
      스페인어의 경우 서비스는 다음 통용어 중 하나로 된 음성에 적합한
      사용자 정의 모델을 작성합니다.
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          카스티야 스페인어의 경우 `es-ES`(`es-ES` 모델)
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          라틴 아메리카 스페인어의 경우 `es-LA`(`es-AR`, `es-CL`, `es-CO`
          및 `es-PE` 모델)
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          멕시코(북미) 스페인어의 경우 `es-US`(`es-MX` 모델)
        </li>
      </ul>
      이 매개변수는 서비스가 올바른 맵핑을 작성하도록 항상 매개변수를
      안전하게 생략할 수 있는 스페인어 모델의 경우에만
      의미가 있습니다.
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          `dialect` 매개변수를 스페인어 이외의 언어 모델에 지정하는 경우
          값은 기본 모델의 언어와 일치해야 합니다.
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          `dialect`를 스페인어 언어 모델에 지정하는 경우 값은 표시된 대로
          정의된 맵핑 중 하나와 일치해야
          합니다(`es-ES`, `es-LA` 또는 `es-MX`).
        </li>
      </ul>
      모든 통용어 값은 대소문자를 구분하지 않습니다.
    </td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>선택사항</em></td>
    <td style="text-align:center">문자열</td>
    <td>
      새 모델에 대한 설명입니다. 사용자 정의 모델의 언어와 일치하는 현지화된 설명을 사용하십시오.
    </td>
  </tr>
</table>

다음 예제에서는 `Example model`이라는 새 사용자 정의 언어 모델을 작성합니다. 이 모델은 기본 모델 `en-US-BroadbandModel`용으로 작성되며 설명은 `Example custom language model`입니다. `Content-Type` 헤더는 JSON 데이터가 메소드에 전달됨을 지정합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Example model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom language model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations"
```
{: pre}

이 예제는 새 모델의 사용자 정의 ID를 리턴합니다. 각 사용자 정의 모델은 GUID(Globally Unique Identifier)인 고유 사용자 정의 ID로 식별됩니다. 모델과 연관된 호출의 `customization_id` 매개변수로 사용자 정의 모델의 GUID를 지정합니다.

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

새 사용자 정의 모델은 해당 인증 정보를 사용하여 이를 작성한 서비스의 인스턴스가 소유합니다. 자세한 정보는 [사용자 정의 모델의 소유권](/docs/services/speech-to-text?topic=speech-to-text-customization#customOwner)을 참조하십시오.

## 사용자 정의 언어 모델에 말뭉치 추가
{: #addCorpus}

사용자 정의 언어 모델을 작성한 후 다음 단계는 모델에 데이터(도메인 특정 단어)를 추가하는 것입니다. 사용자 정의 모델을 새 단어로 채울 때 권장되는 방법은 하나 이상의 말뭉치를 추가하는 것입니다.

말뭉치는 도메인의 샘플 문장을 이상적으로 포함하는 일반 텍스트 파일입니다. 서비스에서 말뭉치 파일의 컨텐츠를 구문 분석하고 기본 어휘에 없는 단어를 추출합니다. 이러한 단어를 OOV(Out Of Vocabulary) 단어라고 합니다.

-   말뭉치 사용에 대한 자세한 정보는 [말뭉치에 대한 작업](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#workingCorpora)을 참조하십시오.
-   서비스가 모델에 말뭉치를 추가하는 방법에 대한 자세한 정보는 [말뭉치 파일을 추가하면 어떻게 됩니까?](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#parseCorpus)를 참조하십시오.

말뭉치는 새 단어를 포함하는 문자을 제공하여 서비스가 컨텍스트에서 단어를 학습할 수 있도록 합니다. 그런 다음 모델의 단어를 개별적으로 기능 보강하거나 수정할 수 있습니다. 말뭉치에서 추가된 단어가 아니라 개별 단어에 대해서만 모델을 훈련하면 더 많은 시간이 소요되며 덜 효과적인 결과가 생성될 수 있습니다.
{: tip}

`POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 메소드를 사용하여 사용자 정의 모델에 말뭉치를 추가할 수 있습니다.

-   `customization_id` 경로 매개변수를 사용하여 사용자 정의 모델의 사용자 정의 ID를 지정하십시오.
-   `corpus_name` 경로 매개변수를 사용하여 말뭉치의 이름을 지정하십시오. 사용자 정의 모델의 언어와 일치하고 말뭉치의 컨텐츠를 반영하는 현지화된 이름을 사용하십시오.
    -   이름에 최대 128자를 포함하십시오.
    -   URL로 인코딩되어야 하는 문자는 사용하지 마십시오. 예를 들어, 이름에 공백, 슬래시, 백슬래시, 콜론, 앰퍼샌드, 큰따옴표, 더하기 부호, 등호, 물음표 등을 사용하지 마십시오. (서비스에서는 이러한 문자의 사용을 금지하지 않습니다. 그러나 이러한 문자를 사용할 때마다 URL로 인코딩되어야 합니다.) 
    -   사용자 정의 모델에 이미 추가된 말뭉치 또는 문법의 이름을 사용하지 마십시오.
    -   서비스에서 사용자가 추가하거나 수정한 사용자 정의 단어를 나타내기 위해 예약한 `user`라는 이름을 사용하지 마십시오.
    -   `base_lm` 또는 `default_lm`이라는 이름을 사용하지 마십시오. 두 이름은 모두 나중에 서비스에서 사용하도록 예약되어 있습니다. 
-   말뭉치 텍스트 파일을 요청의 본문으로 전달하십시오.

모든 소스를 합쳐서 최대 9만 개의 OOV 단어와 총 천만 개의 단어를 추가할 수 있습니다. 여기에는 말뭉치 및 문법의 단어와 사용자가 직접 추가한 단어가 포함됩니다. 자세한 정보는 [필요한 데이터의 양](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordsResourceAmount)을 참조하십시오.
{: note}

다음 예제에서는 지정된 ID를 사용하는 사용자 정의 모델에 말뭉치 텍스트 파일인 `healthcare.txt`를 추가합니다. 이 예제에서는 말뭉치의 이름을 `healthcare`로 지정합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--data-binary @healthcare.txt
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/healthcare"
```
{: pre}

이 메소드는 사용자 정의 모델에 대한 기존 말뭉치를 겹쳐쓰는 선택적 `allow_overwrite` 조회 매개변수도 허용합니다. 말뭉치 파일을 모델에 추가한 후 업데이트해야 하는 경우에 이 매개변수를 사용하십시오.

이 메소드는 비동기식입니다. 완료하는 데 1 - 2분 정도가 걸릴 수 있습니다. 오퍼레이션 지속 시간은 말뭉치에 있는 총 단어 수, 서비스가 말뭉치에서 찾은 새 단어 수 및 서비스의 현재 로드에 따라 다릅니다. 말뭉치의 상태를 확인하는 방법에 대한 자세한 정보는 [말뭉치 추가 요청 모니터링](#monitorCorpus)을 참조하십시오.

각 말뭉치 텍스트 파일마다 한 번씩 메소드를 호출하여 사용자 정의 모델에 원하는 수의 말뭉치를 추가할 수 있습니다. 하나의 말뭉치를 추가하는 작업이 완전히 완료되어야 다른 말뭉치를 추가할 수 있습니다. 말뭉치를 사용자 정의 모델에 추가한 후 새 사용자 정의 단어를 검사하여 철자 오류 및 기타 오류가 있는지 확인하십시오. 자세한 정보는 [단어 리소스 유효성 검증](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel)을 참조하십시오.

### 말뭉치 추가 요청 모니터링
{: #monitorCorpus}

말뭉치가 유효한 경우 서비스에서 201 응답 코드를 리턴합니다. 그런 다음 말뭉치의 컨텐츠를 비동기식으로 처리하고 자동으로 새 단어를 추출합니다. 서비스가 현재 요청에 대한 말뭉치의 분석을 완료할 때까지 사용자 정의 모델에 데이터를 추가하거나 모델을 훈련하는 요청을 제출할 수 없습니다.

분석의 상태를 판별하려면 `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` 메소드를 사용하여 말뭉치의 상태를 폴링하십시오. 다음 예제에 표시된 대로 이 메소드는 모델의 ID 및 말뭉치의 이름을 받아들입니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

응답에는 말뭉치의 상태가 포함됩니다.

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

`status` 필드의 값은 다음 중 하나입니다.

-   `analyzed`는 서비스가 말뭉치를 성공적으로 분석했음을 표시합니다.
-   `being_processed`는 서비스가 말뭉치를 여전히 분석 중임을 표시합니다.
-   `undetermined`는 서비스가 말뭉치를 처리하는 동안 오류가 발생했음을 표시합니다.

루프를 사용하여 `analyzed`가 될 때까지 10초마다 말뭉치의 상태를 확인할 수 있습니다. 모델의 말뭉치 상태 확인에 대한 자세한 정보는 [사용자 정의 언어 모델의 말뭉치 나열](/docs/services/speech-to-text?topic=speech-to-text-manageCorpora#listCorpora)을 참조하십시오.

## 사용자 정의 언어 모델에 단어 추가
{: #addWords}

사용자 정의 언어 모델에 단어를 추가할 때 말뭉치를 추가하는 것이 권장되지만 개별 사용자 정의 단어를 모델에 직접 추가할 수도 있습니다. 서비스는 말뭉치에서 검색한 OOV 단어를 추가하는 것과 마찬가지로 사용자 정의 단어를 사용자 정의 모델에 추가합니다.

모델에 추가할 단어가 하나 또는 몇 개만 있는 경우 말뭉치를 사용하여 단어를 추가하는 것이 실용적이지 않거나 실행 가능하지 않을 수 있습니다. 가장 단순한 접근 방법은 맞춤법이 있는 단어만 추가하는 것입니다. 그러나 단어에 대해 여러 개의 발음을 제공하고 단어가 표시되는 방법을 나타낼 수도 있습니다.

-   단어를 직접 추가하는 방법에 대한 자세한 정보는 [사용자 정의 단어에 대한 작업](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#workingWords)을 참조하십시오.
-   서비스가 모델에 사용자 정의 단어를 추가하는 방법에 대한 자세한 정보는 [사용자 정의 단어를 추가하거나 수정하면 어떻게 됩니까?](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#parseWord)를 참조하십시오.

다음 메소드를 사용하여 사용자 정의 모델에 단어를 추가할 수 있습니다.

-   `POST /v1/customizations/{customization_id}/words` 메소드는 한 범에 여러 단어를 추가합니다. 요청 본문을 통해 각 단어에 대한 정보를 제공하는 JSON 오브젝트를 전달합니다. 다음 예제에서는 지정된 ID를 사용하는 사용자 정의 모델에 `HHonors` 및 `IEEE`라는 두 개의 사용자 정의 단어를 추가합니다. `Content-Type` 헤더는 JSON 데이터가 메소드에 전달됨을 지정합니다.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"words\": [
      {\"word\": \"HHonors\", \"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"},
      {\"word\": \"IEEE\", \"sounds_like\": [\"I. triple E.\"]}]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    파일에서 단어를 추가할 수도 있습니다. 예를 들어, `words.json` 파일은 두 개의 동일한 사용자 정의 단어를 정의합니다.

    ```
    {"words":
      [
        {"word": "HHonors", "sounds_like": ["hilton honors", "H. honors"], "display_as": "HHonors"},
        {"word": "IEEE", "sounds_like": ["I. triple E."]}
      ]
    }
    ```
    {: codeblock}

    다음 명령은 파일에서 단어를 추가합니다.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data-binary @words.json
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    이 메소드는 비동기식입니다. 완료하는 데 걸리는 시간은 추가하는 단어의 수와 서비스의 현재 로드에 따라 다릅니다. 오퍼레이션의 상태를 확인하는 방법에 대한 자세한 정보는 [단어 추가 요청 모니터링](#monitorWords)을 참조하십시오.
-   `PUT /v1/customizations/{customization_id}/words/{word_name}` 메소드는 개별 단어를 추가합니다. 단어에 대한 정보를 제공하는 JSON 오브젝트를 전달합니다. 다음 예제에서는 지정된 ID를 사용하는 모델에 `NCAA`라는 단어를 추가합니다. `Content-Type` 헤더는 JSON 데이터가 메소드에 전달됨을 다시 표시합니다.

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

    이 메소드는 동기식입니다. 이 서비스는 요청의 성공 또는 실패를 즉시 보고하는 응답 코드를 리턴합니다.

말뭉치를 추가할 때와 마찬가지로 새 사용자 정의 단어를 검사하여 철자 오류 및 기타 오류가 있는지 확인하십시오. 특히 한 번에 여러 단어를 추가하는 경우 이와 같이 확인하는 것이 중요합니다. 자세한 정보는 [단어 리소스 유효성 검증](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel)을 참조하십시오.

### 단어 추가 요청 모니터링
{: #monitorWords}

`POST /v1/customizations/{customization_id}/words` 메소드를 사용할 때 입력 데이터가 유효한 경우 서비스에서 201 응답 코드를 리턴합니다. 그런 다음 단어를 비동기식으로 처리하여 모델에 추가합니다. 이 오퍼레이션은 일반적으로 말뭉치를 추가하거나 모델을 훈련하는 것보다 빠르지만 추가하는 새 단어의 수와 서비스의 현재 로드에 따라 지속 시간이 달라집니다. 서비스에서 새 단어 추가 요청을 완료할 때까지 사용자 정의 모델에 데이터를 추가하거나 모델을 훈련하는 요청을 제출할 수 없습니다.

요청의 상태를 판별하려면 `GET /v1/customizations/{customization_id}` 메소드를 사용하여 모델의 상태를 폴링하십시오. 다음 예제에서와 같이 이 메소드는 모델의 사용자 정의 ID를 받아들이고 모델의 상태를 포함하는 정보를 리턴합니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T18:45:11.737Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

`status` 필드는 모델의 현재 상태를 보고합니다. 서비스가 새 단어를 처리하는 동안 상태는 `pending`으로 남아 있습니다. 루프를 사용하여 오퍼레이션이 완료되었음을 표시하는 `ready`가 될 때까지 10초마다 상태를 확인할 수 있습니다. 가능한 `status` 값에 대한 자세한 정보는 [모델 훈련 요청 모니터링](#monitorTraining-language)을 참조하십시오.

### 사용자 정의 모델의 단어 수정
{: #modifyWord}

`POST /v1/customizations/{customization_id}/words` 및 `PUT /v1/customizations/{customization_id}/words/{word_name}` 메소드를 사용하여 사용자 정의 모델의 단어를 수정하거나 기능을 보강할 수 있습니다. 이러한 메소드를 사용하여 단어가 모델에 추가될 때 발생한 철자 오류 또는 기타 오류를 정정해야 할 수도 있습니다. 기존 단어에 대해 동음어 정의를 추가해야 할 수도 있습니다.

이러한 메소드를 사용하여 단어를 추가할 때와 마찬가지로 기존 단어의 정의를 수정할 수 있습니다. 단어에 대해 제공하는 새 데이터가 단어의 기존 정의를 겹쳐씁니다.

## 사용자 정의 언어 모델 훈련
{: #trainModel-language}

사용자 정의 언어 모델을 새 단어로 채운 후(말뭉치 또는 문법을 추가하거나 단어를 직접 추가하여) 새 데이터에 대해 모델을 훈련해야 합니다. 훈련을 통해 사용자 정의 모델이 음성 인식에서 데이터를 사용할 준비가 됩니다. 모델을 데이터에 대해 훈련할 때까지 모델이 어떤 방법으로든 추가한 단어를 사용하지 않습니다.

`POST /v1/customizations/{customization_id}/train` 메소드를 사용하여 사용자 정의 모델을 훈련할 수 있습니다. 다음 예제에서와 같이 훈련할 모델의 사용자 정의 ID를 메소드에 전달합니다.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

이 메소드는 비동기식입니다. 모델이 훈련되는 새 단어의 수 및 서비스의 현재 로드에 따라 훈련을 완료하는 데 몇 분이 걸릴 수 있습니다. 훈련 오퍼레이션의 상태를 확인하는 방법에 대한 자세한 정보는 [모델 훈련 요청 모니터링](#monitorTraining-language)을 참조하십시오.

이 메소드에는 다음과 같은 선택적 조회 매개변수가 포함됩니다.

-   `word_type_to_add` 매개변수는 사용자 모델이 훈련될 단어를 지정합니다. 
    -   원점에 관계없이 모든 단어에 대해 모델을 훈련하려면 `all`을 지정하거나 매개변수를 생략하십시오.
    -   말뭉치 또는 문법에서만 추출된 단어를 무시하고 사용자가 추가하거나 수정한 단어에 대해서만 훈련하려면 `user`를 지정하십시오.

    이 옵션은 노이즈가 있는 데이터(예: 철자 오류가 있는 단어)를 포함하는 말뭉치를 추가하는 경우에 유용합니다. 이러한 데이터에 대해 모델을 훈련하기 전에 `GET /v1/customizations/{customization_id}/words` 메소드의 `word_type` 조회 매개변수를 사용하여 말뭉치 및 문법에서 추출된 단어를 검토할 수 있습니다. 자세한 정보는 [사용자 정의 언어 모델의 단어 나열](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords)을 참조하십시오.
-   이 `customization_weight` 매개변수는 사용자 정의 모델이 음성 인식에 사용되는 경우 기본 어휘의 단어에 비해 사용자 정의 모델의 단어에 지정된 상대 가중치를 지정합니다. 사용자 정의 모델을 사용하는 인식 요청과 함께 사용자 정의 가중치를 지정할 수도 있습니다. 자세한 정보는 [사용자 정의 가중치 사용](/docs/services/speech-to-text?topic=speech-to-text-languageUse#weight)을 참조하십시오.
-   `strict` 매개변수는 사용자 정의 모델이 올바른 리소스와 올바르지 않는 리소스(말뭉치, 문법 및 단어)를 함께 포함하는 경우 훈련이 진행되는지 여부를 표시합니다. 기본적으로 모델에 하나 이상의 올바르지 않은 리소스가 포함된 경우 훈련에 실패합니다. 모델에 하나 이상의 올바른 리소스가 포함되어 있는 한 매개변수를 `false`로 설정하여 훈련을 지속시키십시오. 서비스는 훈련에서 올바르지 않은 리소스를 제외합니다. 자세한 정보는 [훈련 실패](#failedTraining-language)를 참조하십시오.

### 모델 훈련 요청 모니터링
{: #monitorTraining-language}

훈련 프로세스가 성공적으로 시작되면 서비스에서 200 응답 코드를 리턴합니다. 기존 요청이 완료될 때까지 서비스가 후속 훈련 요청 또는 새 말뭉치, 문법 또는 단어를 추가하는 요청을 수락할 수 없습니다.

훈련 요청의 상태를 판별하려면 `GET /v1/customizations/{customization_id}` 메소드를 사용하여 모델의 상태를 폴링하십시오. 이 메소드는 모델의 사용자 정의 ID를 받아들이고 모델에 대한 정보를 리턴합니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T18:45:11.737Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

응답에는 사용자 정의 모델의 상태를 보고하는 `status` 및 `progress` 필드가 포함되어 있습니다. `progress` 필드의 의미는 모델의 상태에 따라 다릅니다. `status` 필드의 값은 다음 중 하나일 수 있습니다.

-   `pending`은 모델이 작성되었지만 올바른 훈련 데이터가 추가되거나 서비스가 추가된 데이터의 분석을 완료할 때까지 대기 중임을 표시합니다. `progress` 필드는 `0`입니다.
-   `ready`는 모델이 올바른 데이터를 포함하고 있으며 훈련될 준비가 되었음을 표시합니다. `progress` 필드는 `0`입니다.

     모델이 올바른 오디오 리소스와 올바르지 않은 리소스를 함께 포함하는 경우(예를 들어, 올바른 사용자 정의 단어 및 올바르지 않은 사용자 정의 단어 모두) `strict` 조회 매개변수를 `false`로 설정하지 않으면 모델 훈련에 실패합니다. 자세한 정보는 [훈련 실패](#failedTraining-language)를 참조하십시오.
-   `training`은 모델을 훈련 중임을 표시합니다. 훈련이 완료되면 `progress` 필드가 `0`에서 `100`으로 변경됩니다. <!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available`은 모델이 훈련되었으며 사용할 준비가 되었음을 표시합니다. `progress` 필드는 `100`입니다.
-   `upgrading`은 모델을 업그레이드 중임을 표시합니다. `progress` 필드는 `0`입니다.
-   `failed`는 모델의 훈련이 실패했음을 표시합니다. `progress` 필드는 `0`입니다. 자세한 정보는 [훈련 실패](#failedTraining-language)를 참조하십시오.

루프를 사용하여 `available`이 될 때까지 10초마다 상태를 확인할 수 있습니다. 사용자 정의 모델의 상태를 확인하는 방법에 대한 자세한 정보는 [사용자 정의 언어 모델 나열](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language)을 참조하십시오.

### 훈련 실패
{: #failedTraining-language}

서비스가 사용자 정의 언어 모델에 대한 다른 요청을 처리 중인 경우 훈련을 시작하는 데 실패합니다. 예를 들어, 서비스가 다음을 수행 중인 경우 409의 상태 코드와 함께 훈련 요청을 시작하는 데 실패합니다.

-   OOV 단어를 생성하기 위한 말뭉치 또는 문법 처리
-   유사 발음의 유효성을 검증하거나 자동 생성하기 위한 사용자 정의 단어 처리
-   다른 훈련 요청 처리

사용자 정의 모델이 다음을 수행 중인 경우에도 400의 상태 코드와 함께 훈련을 시작하는 데 실패합니다.

-   훈련 데이터(말뭉치, 문법 또는 단어)가 작성되거나 마지막으로 훈련된 후 올바른 새 훈련 데이터가 포함되지 않습니다. 
-   하나 이상의 올바르지 않은 말뭉치, 문법 또는 단어가 포함됩니다(예를 들어, 사용자 정의 단어에는 올바르지 않은 유사 발음이 포함됨).

훈련 요청이 400의 상태 코드와 함께 실패하는 경우 서비스는 사용자 정의 모델의 상태를 `failed`로 설정합니다. 다음 조치 중 하나를 수행하십시오.

-   사용자 정의 인터페이스의 메소드를 사용하여 모델의 리소스를 검사하고 발견한 오류를 수정하십시오.
    -   올바르지 않은 말뭉치의 경우 말뭉치 텍스트 파일을 정정하고 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 메소드의 `allow_overwrite` 매개변수를 사용하여 정정된 파일을 모델에 추가할 수 있습니다. 자세한 정보는 [사용자 정의 언어 모델에 말뭉치 추가](#addCorpus)를 참조하십시오.
    -   올바르지 않은 문법의 경우 문법 파일을 정정하고 `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 메소드의 `allow_overwrite` 매개변수를 사용하여 정정된 파일을 모델에 추가할 수 있습니다. 자세한 정보는 [사용자 정의 언어 모델에 문법 추가](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar)를 참조하십시오.
    -   올바르지 않은 사용자 정의 단어의 경우 `POST /v1/customizations/{customization_id}/words` 또는 `PUT /v1/customizations/{customization_id}/words/{word_name}` 메소드를 사용하여 모델의 단어 리소스에서 단어를 직접 수정할 수 있습니다. 자세한 정보는 [사용자 정의 모델의 단어 수정](#modifyWord)을 참조하십시오.

    사용자 정의 언어 모델의 단어 유효성 검증에 대한 자세한 정보는 [단어 리소스 유효성 검증](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel)을 참조하십시오.
-   훈련에서 올바르지 않은 리소스를 제외하려면 `POST /v1/customizations/{customization_id}/train` 메소드의 `strict` 매개변수를 `false`로 설정하십시오. 훈련이 성공하려면 모델에는 하나 이상의 올바른 리소스(말뭉치, 문법 또는 단어)가 포함되어야 합니다. `strict` 매개변수는 올바른 오디오 리소스와 올바르지 않는 리소스가 함께 포함된 사용자 정의 모델을 훈련시키는 데 유용합니다. 

## 예제 스크립트
{: #exampleScripts}

다음 스크립트를 사용하여 사용자 정의 언어 모델을 작성하는 단계를 시험해 볼 수 있습니다.

-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>라는 Python 스크립트. 자세한 정보는 [예제 Python 스크립트](#pythonScript)를 참조하십시오.
-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>라는 Bash 쉘 스크립트. 자세한 정보는 [예제 쉘 스크립트](#shellScript)를 참조하십시오.

두 스크립트는 동일한 기능을 제공합니다. 각 스크립트는 사용자 정의 모델을 작성하고 말뭉치 텍스트 파일의 단어를 추가하며 단일 및 복수 단어를 모두 모델에 직접 추가합니다. 스크립트는 모델을 조회하여 말뭉치에서 추가된 단어와 사용자가 직접 추가한 단어를 나열합니다. 또한 모델을 훈련하고 비동기 오퍼레이션을 모니터하는 데 권장되는 폴링을 보여줍니다.

두 개의 제공된 말뭉치 텍스트 파일 중 하나를 스크립트와 함께 사용하거나 자체 말뭉치 파일로 테스트할 수 있습니다.

-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/corpus.txt" download="corpus.txt">corpus.txt <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>는 6개의 의료 관련 용어를 모델에 추가하는 축약된 6KB 말뭉치 파일입니다. 이 파일은 스크립트와 함께 사용될 때 소량의 출력을 생성합니다. 기본적으로 스크립트에서 이 파일을 사용합니다.
-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/healthcare.txt" download="healthcare.txt">healthcare.txt <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>는 많은 의료 관련 용어를 모델에 추가하는 더 풍부한 164KB 말뭉치입니다. 이 파일은 스크립트와 함께 사용될 때 훨씬 더 많은 출력을 생성합니다.

기본적으로 두 스크립트 중 하나를 사용하여 작성하는 새 사용자 정의 모델을 인식 요청에 사용할 수 있습니다. 스크립트에는 프로세스를 시험할 때 도움이 될 수 있는 새 사용자 정의 모델을 삭제하기 위한 선택적 단계가 포함되어 있습니다. 삭제 단계를 사용으로 설정하려면 스크립트의 주석을 따르십시오.

### 예제 Python 스크립트
{: #pythonScript}

Python 스크립트를 사용하려면 다음 단계를 수행하십시오.

1.  <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>라는 Python 스크립트를 다운로드하십시오.
1.  스크립트와 함께 사용할 예제 말뭉치 텍스트 파일을 다운로드하십시오. 말뭉치 텍스트 파일 또는 사용자가 선택한 파일을 사용하여 자유롭게 테스트할 수 있습니다. 기본적으로 모든 말뭉치 텍스트 파일은 스크립트와 동일한 디렉토리에 있어야 합니다.
1.  이 스크립트에서는 서비스에 대한 HTTP 요청에 Python `requests` 라이브러리를 사용합니다. `pip` 또는 `easy_install`을 사용하여 스크립트에서 사용할 라이브러리를 설치하십시오. 예를 들어, 다음과 같습니다.

    ```bash
    pip install requests
    ```
    {: pre}

    라이브러리에 대한 자세한 정보는 [pypi.python.org/pypi/requests](https://pypi.python.org/pypi/requests){: external}를 참조하십시오.
1.  스크립트를 편집하여 `password` 문자열 `iam_apikey`를 해당 {{site.data.keyword.speechtotextshort}} 인증 정보의 API 키로 대체하십시오.

    ```
    password = "iam_apikey"
    ```
    {: codeblock}

1.  다음 명령을 입력하여 스크립트를 실행하십시오.

    ```bash
    python testSTTcustom.py
    ```
    {: pre}

이 스크립트에서는 댈러스 위치에 대해 기본 URL `https://stream.watsonplatform.net`을 사용합니다. 다른 위치에서 서비스 인스턴스를 작성한 경우 해당 위치를 사용하도록 `uri` 변수를 수정하십시오. 예를 들어, 서비스 인스턴스가 워싱턴, DC 위치에 있는 경우 `https://gateway-wdc.watsonplatform.net`을 사용하십시오.
{: note}

### 예제 쉘 스크립트
{: #shellScript}

Bash 쉘 스크립트를 사용하려면 다음 단계를 수행하십시오.

1.  <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>라는 쉘 스크립트를 다운로드하십시오.
1.  스크립트와 함께 사용할 예제 말뭉치 텍스트 파일을 다운로드하십시오. 말뭉치 텍스트 파일 또는 사용자가 선택한 파일을 사용하여 자유롭게 테스트할 수 있습니다. 기본적으로 모든 말뭉치 텍스트 파일은 스크립트와 동일한 디렉토리에 있어야 합니다.
1.  이 스크립트에서는 서비스에 대한 HTTP 요청에 `curl` 명령을 사용합니다. `curl`을 아직 다운로드하지 않은 경우 [curl.haxx.se](http://curl.haxx.se){: external}에서 사용 중인 운영 체제에 해당하는 버전을 설치할 수 있습니다. SSL(Secure Sockets Layer) 프로토콜을 지원하는 버전을 설치하고 설치된 2진 파일이 `PATH` 환경 변수에 포함되도록 하십시오.
1.  스크립트를 편집하여 `PASSWORD` 문자열 `iam_apikey`를 해당 {{site.data.keyword.speechtotextshort}} 인증 정보의 API 키로 대체하십시오.

    ```
    PASSWORD="iam_apikey"
    ```
    {: codeblock}

1.  스크립트를 편집하여 `URL` 문자열을 서비스 인스턴스가 작성된 위치의 URL로 대체하십시오. 이 스크립트에서는 댈러스 위치에 대해 다음과 같은 기본 URL을 사용합니다.

    ```
    URL="https://stream.watsonplatform.net/speech-to-text/api/v1"
    ```
    {: codeblock}

1.  스크립트에 실행 권한이 있는지 확인한 후 다음 명령을 입력하여 스크립트를 실행하십시오.

    ```bash
    ./testSTTcustom.sh
    ```
    {: pre}
