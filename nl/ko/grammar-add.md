---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-07"

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

# 사용자 정의 언어 모델에 문법 추가
{: #grammarAdd}

문법을 음성 인식에 사용하려면 먼저 사용자 정의 인터페이스를 사용하여 문법을 사용자 정의 언어 모델에 추가해야 합니다. 사용자 정의 언어 모델에 문법을 추가하는 단계는 말뭉치 또는 사용자 정의 단어를 추가하는 데 사용되는 단계와 유사합니다.
{: shortdesc}

1.  [사용자 정의 언어 모델을 작성하십시오](#createModel-grammar). 새 사용자 정의 모델을 작성하거나 기존 모델을 사용할 수 있습니다.
1.  [사용자 정의 언어 모델에 문법을 추가하십시오](#addGrammar). 서비스가 문법의 유효성을 검증하여 정확한지 확인합니다.
1.  [문법의 단어 유효성을 검증하십시오](#validateGrammar). 문법에서 인식되는 OOV(Out Of Vocabulary)에 대한 유사 발음의 정확도를 확인합니다.
1.  [사용자 정의 언어 모델을 훈련하십시오](#trainGrammar). 서비스가 사용자 정의 모델 및 문법을 음성 인식에 사용할 준비를 합니다.
1.  이제 사용자 정의 모델 및 문법을 음성 인식 요청에 사용할 수 있습니다. 자세한 정보는 [음성 인식에 문법 사용](/docs/services/speech-to-text/grammar-use.html)을 참조하십시오.

이러한 단계는 반복적입니다. 필요할 때마다 말뭉치 및 사용자 정의 단어와 문법을 사용자 정의 언어 모델에 추가할 수 있습니다. 추가하는 새 데이터 리소스(문법, 말뭉치 또는 사용자 정의 단어)에 대해 사용자 정의 모델을 훈련해야 합니다. 음성 인식에 사용하는 경우 사용자 정의 모델이 마지막으로 훈련된 데이터를 반영합니다.

언어 모델 사용자 정의를 지원하는 모든 언어에 문법을 사용할 수 있습니다. 언어 모델 사용자 정의는 대부분의 언어에 대해 사용할 수 있습니다. 자세한 정보는 [사용자 정의에 대한 언어 지원](/docs/services/speech-to-text/custom.html#languageSupport)을 참조하십시오.
{: note}

## 사용자 정의 언어 모델 작성
{: #createModel-grammar}

문법을 음성 인식에 사용하려면 사용자 정의 언어 모델에 추가해야 합니다. 기존 사용자 정의 언어 모델을 사용하거나 `POST /v1/customizations` 메소드를 사용하여 새 사용자 정의 모델을 작성할 수 있습니다. 새 사용자 정의 모델 작성에 대한 정보는 [사용자 정의 언어 모델 작성](/docs/services/speech-to-text/language-create.html#createModel-language)을 참조하십시오.

사용자 정의 언어 모델에는 문법만이 아니라 말뭉치 및 사용자 정의 단어도 포함될 수 있습니다. 음성 인식 중에 해당 문법을 사용하거나 사용하지 않고 사용자 정의 모델을 사용할 수 있습니다. 그러나 문법을 사용하는 경우 서비스가 지정된 문법의 단어만 인식합니다.

## 사용자 정의 모델 언어에 문법 추가
{: #addGrammar}

`POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 메소드를 사용하여 사용자 정의 모델에 문법 파일을 추가할 수 있습니다.

-   `customization_id` 경로 매개변수를 사용하여 사용자 정의 언어 모델의 사용자 정의 ID를 지정하십시오.
-   `grammar_name` 경로 매개변수를 사용하여 문법의 이름을 지정하십시오. 사용자 정의 모델의 언어와 일치하고 문법의 컨텐츠를 반영하는 현지화된 이름을 사용하십시오.
    -   이름에 최대 128자를 포함하십시오.
    -   이름에 공백, 슬래시 또는 백슬래시를 포함하지 마십시오.
    -   사용자 정의 모델에 이미 추가된 문법 또는 말뭉치의 이름을 사용하지 마십시오.
    -   서비스에서 사용자가 추가하거나 수정한 사용자 정의 단어를 나타내기 위해 예약한 `user`라는 이름을 사용하지 마십시오.

-   `Content-Type` 요청 헤더를 사용하여 문법의 형식을 지정하십시오.
    -   `application/srgs`: ABNF 문법의 경우
    -   `application/srgs+xml`: XML 문법의 경우

-   문법이 포함된 파일을 요청의 본문으로 전달하십시오.

다음 예제에서는 지정된 ID를 사용하는 사용자 정의 모델에 `confirm.abnf`라는 문법 파일을 추가합니다. 이 예제에서는 문법의 이름을 `confirm-abnf`로 지정합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/srgs"
--data-binary @confirm.abnf
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

이 메소드는 동일한 이름의 기존 문법을 겹쳐쓰기 위해 포함할 수 있는 선택적 `allow_overwrite` 조회 매개변수도 허용합니다. 문법을 모델에 추가한 후 업데이트해야 하는 경우에 이 매개변수를 사용하십시오.

이 메소드는 비동기식입니다. 문법의 크기 및 서비스의 현재 로드에 따라 서비스에서 문법을 분석하는 데 몇 초가 걸릴 수 있습니다. 문법의 상태를 확인하는 방법에 대한 정보는 [문법 추가 요청 모니터링](#monitorGrammar)을 참조하십시오.

각 문법 파일마다 한 번씩 메소드를 호출하여 사용자 정의 모델에 임의의 수의 문법을 추가할 수 있습니다. 하나의 문법을 추가하는 작업이 완전히 완료되어야 다른 문법을 추가할 수 있습니다. 

### 문법 추가 요청 모니터링
{: #monitorGrammar}

문법이 유효한 경우 서비스에서 201 응답 코드를 리턴합니다. 그런 다음 문법의 컨텐츠를 비동기식으로 처리하고 문법이 인식할 수 있는 새 단어를 자동으로 추출합니다. 서비스가 현재 요청에 대한 문법의 분석을 완료할 때까지 사용자 정의 모델에 더 많은 데이터를 추가하거나 모델을 훈련하는 요청을 제출할 수 없습니다.

분석의 상태를 판별하려면 `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` 메소드를 사용하여 문법의 상태를 폴링하십시오. 이 메소드는 사용자 정의 모델의 ID 및 문법의 이름을 받아들입니다. 다음 예제에서는 `confirm-abnf`라는 문법의 상태를 확인합니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

응답에는 문법의 상태가 포함됩니다.

```javascript
{
  "name": "confirm-abnf",
  "out_of_vocabulary_words": 0,
  "status": "being_processed"
}
```
{: codeblock}

`status` 필드의 값은 다음 중 하나입니다.

-   `analyzed`는 서비스가 문법을 성공적으로 분석했음을 표시합니다.
-   `being_processed`는 서비스가 문법을 여전히 분석 중임을 표시합니다.
-   `undetermined`는 서비스가 문법을 처리하는 동안 오류가 발생했음을 표시합니다.

루프를 사용하여 `analyzed`가 될 때까지 10초마다 문법의 상태를 확인할 수 있습니다. 문법의 상태를 확인하는 방법에 대한 자세한 정보는 [사용자 정의 언어 모델의 문법 나열](/docs/services/speech-to-text/grammar-manage.html#listGrammars)을 참조하십시오.

### 문법 추가 실패 처리
{: #handleFailures}

문법 분석에 실패하면 서비스가 문법의 상태를 `undetermined`로 설정하고 문법의 상태와 함께 실패에 대해 설명하는 `error` 필드를 포함합니다. `GET /v1/customizations/{customization_id}` 메소드를 사용하여 사용자 정의 모델의 상태를 확인할 수 있습니다. 문법 추가에 실패하면 출력에 다음과 같은 오류 메시지가 포함됩니다.

```javascript
{
  . . .
  "error": "{\"code\":500, \"code_description\":\"Internal Server Error\",
\". . . Cannot compile grammar . . .\"},
  . . .
}
```
{: codeblock}

사용자 정의 모델의 상태가 오류의 영향을 받지 않지만 문법을 모델에 사용할 수는 없습니다. 문법 파일을 정정하고 이를 사용자 정의 모델에 추가하는 요청을 반복하여 문제점을 해결할 수 있습니다. 실패한 문법 파일의 버전을 겹쳐쓰려면 `allow_overwrite` 매개변수를 `true`로 설정하십시오.

잠시 동안 사용자 정의 모델에서 문법을 삭제한 후 문법 파일을 정정하고 나중에 다시 추가할 수도 있습니다. 문법 삭제에 대한 자세한 정보는 [사용자 정의 언어 모델에서 문법 삭제](/docs/services/speech-to-text/grammar-manage.html#deleteGrammar)를 참조하십시오.

## 문법의 단어 유효성 검증
{: #validateGrammar}

문법 파일을 사용자 정의 언어 모델에 추가하면 서비스가 문법을 구문 분석하여 문법이 이미 서비스 기본 어휘의 일부가 아닌 단어를 인식하는지 여부를 판별합니다. 이러한 단어를 OOV(Out Of Vocabulary) 단어라고도 합니다. 서비스가 OOV 단어를 사용자 정의 모델의 단어 리소스에 추가합니다. 단어 리소스의 목적은 서비스의 어휘에 아직 존재하지 않는 단어를 정의하는 것입니다.

단어 리소스의 정의는 서비스에 OOV 단어를 변환하는 방법을 알립니다. 이 정보에는 서비스에 단어가 발음되는 방식을 알리는 `sounds_like` 필드, 서비스에 단어를 표시하는 방식을 알리는 `display_as` 필드 및 단어가 사용자 정의 모델에 추가된 방식을 표시하는 `source` 필드가 포함됩니다. 단어 리소스 및 OOV 단어에 대한 자세한 정보는 [단어 리소스](/docs/services/speech-to-text/language-resource.html#wordsResource)를 참조하십시오.

문법을 사용자 정의 모델에 추가한 후 모델의 단어 리소스에 있는 OOV 단어를 검사하여 유사 발음을 확인하는 것이 좋습니다. 모든 문법에 OOV 단어가 있는 것은 아니지만 단어 리소스를 확인하는 것은 일반적으로 좋은 아이디어입니다. 문법을 추가한 후 사용자 정의 모델의 단어를 확인하려면 다음 메소드를 사용하십시오.

-   `GET /v1/customizations/{customization_id}/words` 메소드를 사용하여 사용자 정의 모델의 모든 단어를 나열할 수 있습니다. 문법에서 추가된 단어만 나열하려면 이 메소드의 `word_type` 매개변수와 함께 `grammars` 값을 전달하십시오.
-   `GET /v1/customizations/{customization_id}/words/{word_name}` 메소드를 사용하여 모델의 개별 단어를 볼 수 있습니다.

단어의 유사 발음이 정확하고 올바른지 확인하십시오. 또한 단어 자체에서 철자 및 기타 오류를 찾으십시오. 사용자 정의 모델의 단어 유효성 검증 및 정정에 대한 자세한 정보는 [웹 리소스 유효성 검증](/docs/services/speech-to-text/language-resource.html#validateModel)을 참조하십시오.

## 사용자 정의 언어 모델 훈련
{: #trainGrammar}

문법을 사용자 정의 언어 모델에 사용하기 전의 최종 단계는 모델을 훈련하는 것입니다. 새 말뭉치 또는 새 사용자 정의 단어에 대해 사용자 정의 모델을 훈련하는 데 사용하는 것과 동일한 프로세스를 사용합니다. 모델을 훈련하면 문법이 음성 인식에 사용할 수 있도록 컴파일됩니다. 서비스가 음성 인식을 위해 원래 텍스트 기반 형식의 문법을 2진 런타임 형식으로 처리합니다. 모델을 훈련할 때까지 문법을 사용할 수 없습니다.

`POST /v1/customizations/{customization_id}/train` 메소드를 사용하여 사용자 정의 모델을 훈련할 수 있습니다. 다음 예제에서와 같이 훈련할 모델의 사용자 정의 ID를 메소드에 전달합니다.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

훈련 메소드는 비동기식입니다. 일반적으로 훈련을 완료하는 데 몇 초 정도만 걸립니다. 그러나 문법의 크기 및 복잡도와 서비스의 현재 로드에 따라 더 오래 걸릴 수 있습니다. 훈련 오퍼레이션의 상태를 확인하는 방법에 대한 정보는 [훈련 요청 모니터링](#monitorTraining-grammar)을 참조하십시오.

### 훈련 요청 모니터링
{: #monitorTraining-grammar}

훈련 프로세스가 성공적으로 시작되면 서비스에서 200 응답 코드를 리턴합니다. 현재 훈련 요청이 완료될 때까지 서비스가 후속 훈련 요청이나 새 문법, 말뭉치 또는 단어를 사용자 정의 모델에 추가하는 요청을 수락할 수 없습니다.

훈련 요청의 상태를 판별하려면 `GET /v1/customizations/{customization_id}` 메소드를 사용하여 모델의 상태를 폴링하십시오. 이 메소드는 모델의 사용자 정의 ID를 받아들이고 다음과 같은 모델에 대한 정보를 리턴합니다.

```
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2018-06-01T18:42:25.324Z",
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

모델이 훈련되는 동안 `status` 필드의 값은 `training`입니다. 루프를 사용하여 `available`이 될 때까지 10초마다 모델의 상태를 확인할 수 있습니다. 훈련 요청 및 기타 가능한 상태 값 모니터링에 대한 자세한 정보는 [모델 훈련 요청 모니터링](/docs/services/speech-to-text/language-create.html#monitorTraining-language)을 참조하십시오.
