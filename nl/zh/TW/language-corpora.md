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

# 管理語料庫
{: #manageCorpora}

自訂作業介面包含 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法，用來將語料庫新增至自訂語言模型。如需相關資訊，請參閱[將語料庫新增至自訂語言模型](/docs/services/speech-to-text/language-create.html#addCorpus)。此介面還包含下列方法，用來列出及刪除自訂語言模型的語料庫。
{: shortdesc}

## 列出自訂語言模型的語料庫
{: #listCorpora}

自訂作業介面提供兩種方法，用來列出自訂語言模型的語料庫相關資訊：

-   `GET /v1/customizations/{customization_id}/corpora` 方法列出自訂模型的所有語料庫相關資訊。
-   `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法列出自訂模型的指定語料庫相關資訊。

這兩種方法都會傳回語料庫的 `name`、從語料庫中讀取的 `total_words`，以及從語料庫中擷取的 `out-of-vocabulary_words` 數目。這兩種方法還會列出語料庫的 `status`。在檢查服務的語料庫分析，以回應新增語料庫至自訂模型的要求時，狀態非常重要：

-   `analyzed` 表示服務已順利分析語料庫。可以使用語料庫中的資料來訓練自訂模型。
-   `being_processed` 表示服務仍在分析語料庫。在分析完成之前，服務無法接受新增語料庫或字組的要求，或是訓練自訂模型的要求。
-   `undetermined` 表示服務在處理語料庫時發生錯誤。針對語料庫所傳回的資訊包含錯誤訊息，其提供更正錯誤的指引。

    例如，語料庫可能無效，或者您可能嘗試新增一個與現有語料庫同名的語料庫。您可以重新嘗試新增語料庫，並在要求中加入 `allow_overwrite` 參數。您也可以刪除該語料庫，然後嘗試再次新增語料庫。

### 要求與回應範例
{: #listExample-corpora}

下列範例列出具有指定自訂作業 ID 之自訂模型的所有語料庫：

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora"
```
{: pre}

已將三個語料庫新增至自訂模型。服務已順利分析 `corpus1`，仍在分析 `corpus2`，而 `corpus3` 的分析失敗。

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

下列範例針對具有指定自訂作業 ID 的自訂模型，傳回名為 `corpus1` 的語料庫相關資訊：

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

## 從自訂語言模型中刪除語料庫
{: #deleteCorpus}

使用 `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法，從自訂語言模型中移除現有語料庫。刪除語料庫時，服務會從自訂模型的字組資源中移除所有與語料庫相關聯的 OOV 字組，但下列情況除外：

-   這個字已由另一個語料庫或由文法所新增。
-   這個字已透過 `POST /v1/customizations/{customization_id}/words` 或 `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法加以修改。

在您使用 `POST /v1/customizations/{customization_id}/train` 方法在其更新的資料上訓練此模型之前，移除語料庫並不會影響自訂模型。如果您已順利訓練語料庫的模型，則語料庫中的字組會保留在模型的詞彙中，並套用於語音辨識，直到您重新訓練模型為止。

### 要求範例
{: #deleteExample-corpus}

下列範例從具有指定自訂作業 ID 的自訂模型中刪除名為 `corpus3` 的語料庫。

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus3"
```
{: pre}
