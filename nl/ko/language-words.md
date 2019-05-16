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

# 사용자 정의 단어 관리
{: #manageWords}

사용자 정의 인터페이스에는 사용자 정의 모델의 단어를 추가하거나 수정하는 데 사용되는 `POST /v1/customizations/{customization_id}/words` 및 `PUT /v1/customizations/{customization_id}/words/{word_name}` 메소드가 포함되어 있습니다. 자세한 정보는 [사용자 정의 언어 모델에 단어 추가](/docs/services/speech-to-text/language-create.html#addWords)를 참조하십시오. 이 인터페이스에는 사용자 정의 언어 모델의 단어를 나열하고 삭제하기 위한 다음 메소드도 포함되어 있습니다.
{: shortdesc}

말뭉치에서 대부분의 사용자 정의 단어를 추가할 가능성이 있습니다. 말뭉치의 텍스트 파일에서 사용되는 문자 인코딩을 알고 있는지 확인하십시오. 서비스는 텍스트 파일에서 찾은 인코딩을 유지합니다. 사용자 정의 언어 모델의 개별 단어에 대해 작업할 때 해당 인코딩을 사용해야 합니다. `GET`, `PUT` 또는 `DELETE /v1/customizations/{customization_id}/words/{word_name}` 메소드를 사용하여 단어를 지정할 때 단어에 ACII가 아닌 문자가 포함된 경우 URL에 전달하는 `word_name`을 URL로 인코딩해야 합니다. 자세한 정보는 [문자 인코딩](/docs/services/speech-to-text/language-resource.html#charEncoding)을 참조하십시오.
{: important}

## 사용자 정의 언어 모델의 단어 나열
{: #listWords}

사용자 정의 인터페이스는 사용자 정의 언어 모델의 단어를 나열하는 두 가지 메소드를 제공합니다.

-   `GET /v1/customizations/{customization_id}/words` 메소드는 사용자 정의 모델의 단어 리소스에 있는 단어에 대한 정보를 나열합니다. 이 메소드에는 두 개의 선택적 조회 매개변수가 포함됩니다.
    -   `word_type` 매개변수는 나열될 단어를 지정합니다.

        -   `all`(기본값)은 모든 단어를 표시합니다.
        -   `user`는 사용자가 추가하거나 수정한 사용자 정의 단어만 표시합니다.
        -   `corpora`는 말뭉치에서 추출된 OOV 단어만 표시합니다.
        -   `grammars`는 문법에서 추출된 OOV 단어만 표시합니다.
    -   `sort` 매개변수는 단어가 정렬되는 방식을 표시합니다. 이 매개변수는 단어가 정렬되는 방식을 표시하는 두 개의 매개변수 `alphabetical` 및 `count`를 받아들입니다. 선택적 `+` 또는 `-`를 인수 앞에 추가하여 결과가 오름차순 또는 내림차순으로 정렬되는지를 표시할 수 있습니다. 기본적으로 이 메소드는 단어를 알파벳 오름차순으로 표시합니다.
-   `GET /v1/customizations/{customization_id}/words/{word_name}` 메소드는 모델의 단어 리소스에 있는 지정된 하나의 단어에 대한 정보를 나열합니다.


단어를 식별하는 `word` 필드 이외에 두 메소드 모두 각 단어에 대한 다음 정보를 리턴합니다.

-   단어에 대한 발음의 배열을 표시하는 `sounds_like` 필드. 단어에 대한 sounds-like 값이 제공되지 않으면 이 배열에는 서비스에서 자동으로 생성하는 유사 발음이 포함될 수 있습니다. 자세한 정보는 [sounds_like 필드 사용](/docs/services/speech-to-text/language-resource.html#soundsLike)을 참조하십시오.
-   서비스가 변환에 표시하는 사용자 정의 단어의 맞춤법을 표시하는 `display_as` 필드. 단어에 대한 display-as 값이 제공되지 않으면 이 필드에 빈 문자열이 포함되며, 이 경우 단어는 맞춤법대로 표시됩니다. 자세한 정보는 [display_as 필드 사용](/docs/services/speech-to-text/language-resource.html#displayAs)을 참조하십시오.
-   단어가 사용자 정의 모델의 단어 리소스에 추가된 방법을 표시하는 `source` 필드. 이 필드에는 서비스가 단어를 추출한 각 말뭉치 및 문법의 이름이 포함됩니다. 사용자가 직접 단어를 수정하거나 추가한 경우 이 필드에는 `user`라는 문자열이 포함됩니다.
-   모든 말뭉치 및 문법에서 단어가 발견된 횟수를 표시하는 `count` 필드. 예를 들어, 단어가 하나의 말뭉치에서 5회 발생하고 다른 말뭉치에서 7회 발생하는 경우 count는 `12`입니다.

    단어가 말뭉치 또는 문법에서 추가되기 전에 모델에 사용자 정의 단어를 추가하는 경우 count는 `1`에서 시작합니다. 단어가 먼저 말뭉치 또는 문법에서 추가되고 나중에 수정된 경우 count에는 단어가 말뭉치 및 문법에서 발견된 횟수만 반영됩니다. 

    `count` 필드가 도입되기 전에 작성된 사용자 정의 모델의 경우 이 필드는 항상 `0`으로 유지됩니다. 이러한 모델에 대한 필드를 업데이트하려면 모델의 말뭉치 및 문법을 다시 추가하고 `allow_overwrite` 매개변수를 요청에 포함하십시오.
    {: note}

서비스가 사용자 정의 단어의 정의에 대한 하나 이상의 문제점을 발견하면 출력에 `error` 필드가 포함됩니다. 이 필드는 해당 정의의 각 문제점 요소를 나열하는 배열과 문제점을 설명하는 메시지를 제공합니다.

예를 들어, 발음 추가 규칙 중 하나를 위반한 올바르지 않은 `sounds_like` 필드가 있는 사용자 정의 단어를 추가하는 경우 오류가 발생할 수 있습니다. 단어 리소스에 오류가 있는 단어가 포함된 사용자 정의 모델은 훈련할 수 없습니다. 모델을 훈련하기 전에 해당 단어를 수정하거나 삭제해야 합니다.

### 예제 요청 및 응답
{: #listExample-words}

다음 예제에서는 유형에 관계없이 지정된 사용자 정의 ID를 사용하는 사용자 정의 모델의 모든 단어를 나열합니다. 단어는 기본 정렬 순서인 알파벳 오름차순으로 표시됩니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
```
{: pre}

모델의 단어 리소스에는 4개의 단어가 포함됩니다. 첫 번째 단어는 사용자가 직접 추가했지만 해당 `sounds_like` 필드에 오류가 포함되어 있습니다. 이 필드에는 숫자가 포함될 수 없습니다. 다른 단어는 사용자 또는 사용자 및 말뭉치 모두에서 추가되었습니다.

```javascript
{
  "words": [
    {
      "word": "75.00",
      "sounds_like": ["75 dollars"],
      "display_as": "75.00",
      "count": 1,
      "source": ["user"],
      "error": [{"75 dollars": "Numbers are not allowed in sounds_like. You can try for example 'seventy five dollars'."}]
    },
    {
      "word": "HHonors",
      "sounds_like": [
        "hilton honors",
        "H. honors"
      ],
      "display_as": "HHonors",
      "count": 1,
      "source": [
        "corpus1",
        "user"
      ]
    },
    {
      "word": "IEEE",
      "sounds_like": ["I. triple E."],
      "display_as": "IEEE",
      "count": 3,
      "source": [
        "corpus1",
        "corpus2",
        "user"
      ]
    },
    {
      "word": "tomato",
      "sounds_like": [
        "tomatoh",
        "tomayto"
      ],
      "display_as": "tomato",
      "count": 1,
      "source": ["user"]
    }
  ]
}
```
{: codeblock}

다음 예제는 지정된 모델의 단어 리소스에서 가져온 `NCAA`라는 단어에 대한 정보를 보여줍니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
```
{: pre}

사용자가 처음에 단어를 추가했습니다. 그런 다음 서비스가 `corpus3`에서 이 단어를 두 번 찾았습니다.

```javascript
{
  "word": "NCAA",
  "sounds_like": [
    "N. C. A. A.",
    "N. C. double A."
  ],
  "display_as": "NCAA",
  "count": 3,
  "source": [
    "corpus3",
    "user"
  ]
}
```
{: codeblock}

## 사용자 정의 언어 모델에서 단어 삭제
{: #deleteWord}

사용자 정의 언어 모델에서 단어를 삭제하려면 `DELETE /v1/customizations/{customization_id}/words/{word_name}` 메소드를 사용하십시오. 이 메소드를 사용하여 잘못 추가된 단어(예: 잘못된 데이터가 있는 말뭉치의 단어)를 제거할 수 있습니다.

어떤 방식으로든 사용자 정의 모델의 단어 리소스에 추가한 모든 단어를 제거할 수 있습니다. 그러나 서비스의 기본 어휘에서 단어를 삭제할 수는 없습니다. 사용자 정의 모델에서 단어를 삭제하면 해당 단어의 사용자 정의 발음만 삭제됩니다. 단어는 기본 어휘에 남아 있습니다.

사용자 정의 모델에서 단어를 제거해도 `POST /v1/customizations/{customization_id}/train` 메소드를 사용하여 재훈련할 때까지 모델에 영향이 미치지 않습니다. 모델이 이전에 해당 단어에 대해 훈련된 경우 해당 단어 리소스에서 단어를 삭제한 후에도 모델이 음성 인식에 단어를 계속 적용합니다. 삭제를 반영하려면 모델을 재훈련해야 합니다.

### 예제 요청
{: #deleteExample-word}

다음 예제에서는 지정된 사용자 정의 ID를 사용하는 사용자 정의 모델에서 `IEEE`라는 단어를 삭제합니다.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IEEE"
```
{: pre}
