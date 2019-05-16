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

# 말뭉치 관리
{: #manageCorpora}

사용자 정의 인터페이스에는 사용자 정의 언어 모델에 말뭉치를 추가하기 위한 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 메소드가 포함되어 있습니다. 자세한 정보는 [사용자 정의 언어 모델에 말뭉치 추가](/docs/services/speech-to-text/language-create.html#addCorpus)를 참조하십시오. 이 인터페이스에는 사용자 정의 언어 모델의 말뭉치를 나열하고 삭제하기 위한 다음 메소드도 포함되어 있습니다.
{: shortdesc}

## 사용자 정의 언어 모델의 말뭉치 나열
{: #listCorpora}

사용자 정의 인터페이스는 사용자 정의 언어 모델의 말뭉치에 대한 정보를 나열하기 위한 두 가지 메소드를 제공합니다.

-   `GET /v1/customizations/{customization_id}/corpora` 메소드는 사용자 정의 모델의 모든 말뭉치에 대한 정보를 나열합니다.
-   `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` 메소드는 사용자 정의 모델의 지정된 말뭉치에 대한 정보를 나열합니다.

두 메소드 모두 말뭉치의 `name`, 말뭉치에서 읽은 `total_words` 및 말뭉치에서 추출된 `out-of-vocabulary_words`의 수를 리턴합니다. 또한 이 메소드는 말뭉치의 `status`를 나열합니다. 상태는 사용자 정의 모델에 추가하는 요청에 대한 응답으로 말뭉치에 대한 서비스의 분석을 확인하는 데 있어서 중요합니다.

-   `analyzed`는 서비스가 말뭉치를 성공적으로 분석했음을 표시합니다. 말뭉치의 데이터로 사용자 정의 모델을 훈련할 수 있습니다.
-   `being_processed`는 서비스가 말뭉치를 여전히 분석 중임을 표시합니다. 분석이 완료될 때까지 서비스가 새 말뭉치 또는 단어를 추가하거나 사용자 정의 모델을 훈련하라는 요청을 승인할 수 없습니다.
-   `undetermined`는 서비스가 말뭉치를 처리하는 동안 오류가 발생했음을 표시합니다. 말뭉치에 대해 리턴되는 정보에는 오류를 정정하기 위한 안내를 제공하는 오류 메시지가 포함되어 있습니다.

    예를 들어, 말뭉치가 올바르지 않거나 기존 말뭉치와 동일한 이름의 말뭉치를 추가하려고 했을 수 있습니다. 말뭉치를 다시 추가하고 `allow_overwrite` 매개변수를 요청에 포함시킬 수 있습니다. 말뭉치를 삭제한 후 다시 추가할 수도 있습니다.

### 예제 요청 및 응답
{: #listExample-corpora}

다음 예제에서는 지정된 사용자 정의 ID를 사용하는 사용자 정의 모델에 대한 모든 말뭉치를 나열합니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora"
```
{: pre}

세 개의 말뭉치가 사용자 정의 모델에 추가되었습니다. 서비스가 `corpus1`을 성공적으로 분석했습니다. 또한 `corpus2`를 여전히 분석 중이며 `corpus3` 분석에 실패했습니다.

```javascript
{
  "corpora": [
    {
      "name": "corpus1",
      "total_words": 5037,
      "out_of_vocabulary_words": 401,
      "status": "analyzed"
    },
    {
      "name": "corpus2",
      "total_words": 0,
      "out_of_vocabulary_words": 0,
      "status": "being_processed"
    },
    {
      "name": "corpus3",
      "total_words": 0,
      "out_of_vocabulary_words": 0,
      "status": "undetermined",
      "error": "Analysis of corpus 'corpus3.txt' failed. Please try adding the corpus again by setting the 'allow_overwrite' flag to 'true'."
    }
  ]
}
```
{: codeblock}

다음 예제는 지정된 사용자 정의 ID를 사용하는 사용자 정의 모델의 `corpus1`이라는 말뭉치에 대한 정보를 리턴합니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

## 사용자 정의 언어 모델에서 말뭉치 삭제
{: #deleteCorpus}

사용자 정의 언어 모델에서 기존 말뭉치를 제거하려면 `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` 메소드를 사용하십시오. 서비스가 말뭉치를 삭제하면 다음 경우를 제외하고 사용자 정의 모델의 단어 리소스에서 말뭉치와 연관된 모든 OOV 단어가 제거됩니다.

-   다른 말뭉치 또는 문법에서도 이 단어를 추가했습니다.
-   `POST /v1/customizations/{customization_id}/words` 또는 `PUT /v1/customizations/{customization_id}/words/{word_name}` 메소드를 사용하여 어떤 방식으로든 단어를 수정했습니다.

말뭉치를 제거해도 `POST /v1/customizations/{customization_id}/train` 메소드를 사용하여 업데이트된 데이터에 대해 모델을 훈련할 때까지 사용자 정의 모델에 영향이 미치지 않습니다. 말뭉치에 대해 모델을 훈련한 경우 모델을 재훈련할 때까지 말뭉치의 단어가 모델의 어휘에 계속 남아 있고 음성 인식에 적용됩니다.

### 예제 요청
{: #deleteExample-corpus}

다음 예제는 지정된 사용자 정의 ID를 사용하는 사용자 정의 모델에서 `corpus3`이라는 말뭉치를 삭제합니다.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus3"
```
{: pre}
