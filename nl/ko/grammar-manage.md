---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-04"

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

# 문법 관리
{: #manageGrammars}

사용자 정의 인터페이스에는 사용자 정의 언어 모델에 문법을 추가하기 위한 `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 메소드가 포함되어 있습니다. 자세한 정보는 [사용자 정의 언어 모델에 문법 추가](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar)를 참조하십시오. 이 인터페이스에는 사용자 정의 언어 모델의 문법을 나열하고 삭제하기 위한 다음 메소드도 포함되어 있습니다.
{: shortdesc}

## 사용자 정의 언어 모델의 문법 나열
{: #listGrammars}

사용자 정의 인터페이스는 사용자 정의 언어 모델의 문법에 대한 정보를 나열하기 위한 두 가지 메소드를 제공합니다.

-   `GET /v1/customizations/{customization_id}/grammars` 메소드는 사용자 정의 모델의 모든 문법에 대한 정보를 나열합니다.
-   `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` 메소드는 사용자 정의 모델의 지정된 문법에 대한 정보를 나열합니다.

두 메소드는 문법에 대한 동일한 정보를 리턴합니다. 정보에는 문법의 `name` 및 문법에서 인식되는 `out-of_vocabulary_words` 수가 포함됩니다. 또한 응답에는 문법을 사용자 정의 모델에 추가할 때 문법에 대한 서비스의 분석을 확인하는 데 있어서 중요한 문법의 `status`가 포함됩니다.

-   `being_processed`는 서비스가 `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 요청에 대한 응답으로 문법을 여전히 처리 중임을 표시합니다.
-   `analyzed`는 서비스가 문법을 성공적으로 처리하고 사용자 정의 모델에 추가했음을 표시합니다.
-   `undetermined`는 서비스가 문법을 처리하는 동안 오류가 발생했음을 의미합니다. 문법에 대해 리턴되는 정보에는 오류를 정정하기 위한 안내를 제공하는 오류 메시지가 포함되어 있습니다.

    예를 들어, 문법이 올바르지 않거나 기존 문법과 동일한 이름의 문법을 추가하려고 했을 수 있습니다. 문법을 다시 추가하고 `allow_overwrite` 매개변수를 요청에 포함시킬 수 있습니다. 문법을 삭제한 후 다시 추가할 수도 있습니다.

### 예제 요청 및 응답
{: #listExample-grammars}

다음 예제에서는 지정된 사용자 정의 ID를 사용하는 사용자 정의 모델에 추가된 모든 문법에 대한 정보를 나열합니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars"
```
{: pre}

세 개의 문법 `confirm-xml`, `confirm-abnf` 및 `list-abnf`가 사용자 정의 모델에 추가되었습니다.

```javascript
{
  "grammars": [
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-xml",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-abnf",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 8,
      "name": "list-abnf",
      "status": "analyzed"
    }
  ]
}
```
{: codeblock}

다음 예제는 `list-abnf`라는 지정된 문법에 대한 정보를 표시합니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/list-abnf"
```
{: pre}

```javascript
{
  "out_of_vocabulary_words": 8,
  "name": "list-abnf",
  "status": "analyzed"
}
```
{: codeblock}

## 사용자 정의 언어 모델에서 문법 삭제
{: #deleteGrammar}

사용자 정의 언어 모델에서 기존 문법을 제거하려면 `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` 메소드를 사용하십시오. 서비스가 문법을 삭제하면 다음 경우를 제외하고 사용자 정의 모델의 단어 리소스에서 문법과 연관된 모든 OOV 단어가 제거됩니다.

-   다른 문법 또는 말뭉치에서도 이 단어를 추가했습니다.
-   `POST /v1/customizations/{customization_id}/words` 또는 `PUT /v1/customizations/{customization_id}/words/{word_name}` 메소드를 사용하여 어떤 방식으로든 단어를 수정했습니다.

문법을 제거해도 `POST /v1/customizations/{customization_id}/train` 메소드를 사용하여 업데이트된 데이터에 대해 모델을 훈련할 때까지 사용자 정의 모델에 영향이 미치지 않습니다. 문법에 대해 모델을 훈련한 경우 모델을 재훈련할 때까지 문법을 음성 인식에 계속 사용할 수 있습니다.

### 예제 요청
{: #deleteExample-grammar}

다음 예제에서는 지정된 사용자 정의 ID를 사용하는 사용자 정의 모델에서 `list-abnf`라는 문법을 삭제합니다.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/ {customization_id}/grammars/list-abnf"
```
{: pre}
