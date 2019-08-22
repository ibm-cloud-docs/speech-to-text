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

# 管理文法
{: #manageGrammars}

自訂作業介面包括 `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法，用來將文法新增至自訂語言模型。如需相關資訊，請參閱[將文法新增至自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar)。此介面還包含下列方法，用來列出及刪除自訂語言模型的文法。
{: shortdesc}

## 列出自訂語言模型的文法
{: #listGrammars}

自訂作業介面提供兩種方法，用來列出自訂語言模型的文法的相關資訊：

-   `GET /v1/customizations/{customization_id}/grammars` 方法列出自訂模型的所有文法的相關資訊。
-   `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法列出自訂模型的指定文法的相關資訊。

這兩種方法會傳回有關文法的相同資訊。這項資訊包含文法的 `name` 以及文法所辨識的 `out-of_vocabulary_words` 的數目。回應也包括文法的 `status`，當您將文法新增至自訂模型時，這對於檢查服務的文法分析很重要：

-   `being_processed` 指出服務仍在處理文法，以回應 `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 要求。
-   `analyzed` 指出服務已順利處理文法，並已將文法新增至自訂模型。
-   `undetermined` 表示服務在處理文法時發生錯誤。針對文法所傳回的資訊包含錯誤訊息，其提供更正錯誤的指引。

    例如，文法可能無效，或者您可能嘗試新增一個與現有文法同名的文法。您可以重新嘗試新增文法，並將 `allow_overwrite` 參數包含在要求中。您也可以刪除該文法，然後嘗試再次新增。

### 要求與回應範例
{: #listExample-grammars}

下列範例列出已新增至具有所指定自訂作業 ID 的自訂模型的所有文法的相關資訊。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars"
```
{: pre}

已順利將三個文法新增至自訂模型：`confirm-xml`、`confirm-abnf` 及 `list-abnf`。

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

下列範例顯示指定文法 `list-abnf` 的相關資訊。

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

## 從自訂語言模型刪除文法
{: #deleteGrammar}

使用 `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法從自訂語言模型移除現有的文法。當它刪除文法時，服務會從自訂模型的字組資源移除所有與文法相關聯的 OOV 字組，但下列情況除外：

-   這個字已由另一個文法或由語料庫所新增。
-   這個字已透過 `POST /v1/customizations/{customization_id}/words` 或 `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法加以修改。

在您使用 `POST /v1/customizations/{customization_id}/train` 方法，根據更新的資料訓練此模型之前，移除文法並不會影響自訂模型。如果您已順利根據文法訓練此模型，則文法仍可繼續用於語音辨識，直到您重新訓練模型為止。

### 要求範例
{: #deleteExample-grammar}

下列範例從具有指定自訂作業 ID 的自訂模型刪除名為 `list-abnf` 的文法。

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/ {customization_id}/grammars/list-abnf"
```
{: pre}
