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

# 管理自訂字組
{: #manageWords}

自訂作業介面包含 `POST /v1/customizations/{customization_id}/words` 和 `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法，可用來新增或修改自訂模型的字組。如需相關資訊，請參閱[將字組新增至自訂語言模型](/docs/services/speech-to-text/language-create.html#addWords)。此介面還包含下列方法，用來列出及刪除自訂語言模型的字組。
{: shortdesc}

您可能會從語料庫中新增大部分自訂字組。請確定您知道語料庫文字檔中使用的字元編碼。服務會保留它在文字檔中找到的編碼。在使用自訂語言模型中的個別字組時，必須使用該編碼。當您使用 `GET`、`PUT` 或 `DELETE /v1/customizations/{customization_id}/words/{word_name}` 方法指定字組時，如果字組包含非 ASCII 字元，則必須將您在 URL 中傳遞的 `word_name` 以 URL 編碼。如需相關資訊，請參閱[字元編碼](/docs/services/speech-to-text/language-resource.html#charEncoding)。
{: important}

## 列出自訂語言模型中的字組
{: #listWords}

自訂作業介面提供兩種方法，用來列出自訂語言模型中的字組：

-   `GET /v1/customizations/{customization_id}/words` 方法，列出自訂模型字組資源中字組的相關資訊。此方法包含兩個選用的查詢參數：
    -   `word_type` 參數指定要列出的字組：

        -   `all`（預設值）顯示所有字組。
        -   `user` 只會顯示使用者新增或修改的自訂字組。
        -   `corpora` 只會顯示從語料庫擷取的 OOV 字組。
        -   `grammars` 只會顯示從文法擷取的 OOV 字組。
    -   `sort` 參數可指定字組的排序方式。此參數接受兩個引數來指定字組的排序方式：`alphabetical` 和 `count`。您可以在引數前面加上選用的 `+` 或 `-`，以指定結果是要依遞增還是遞減方式排序。依預設，此方法會按字母順序遞增顯示字組。
-   `GET /v1/customizations/{customization_id}/words/{word_name}` 方法會列出模型字組資源中單一指定字組的相關資訊。

除了用來識別字組的 `word` 欄位之外，這兩個方法還會傳回每個字組的下列相關資訊：

-   `sounds_like` 欄位會呈現字組的發音陣列。如果沒有為字組提供 sounds-like 值，此陣列會包含服務自動產生的類似音發音。如需相關資訊，請參閱[使用 sounds_like 欄位](/docs/services/speech-to-text/language-resource.html#soundsLike)。
-   `display_as` 欄位會顯示服務在轉錄結果中呈現的自訂字組拼字。如果沒有為字組提供 display-as 值，此欄位會包含空字串，在此情況下，該字組會依其拼法顯示。如需相關資訊，請參閱[使用 display_as 欄位](/docs/services/speech-to-text/language-resource.html#displayAs)。
-   `source` 欄位指出該字組新增至自訂模型字組資源的方式。此欄位包含服務從中擷取字組的每個語料庫和文法的名稱。如果您直接修改或新增字組，則此欄位會包含字串 `user`。
-   `count` 欄位指出在所有語料庫和文法中找到該字組的次數。例如，如果該字組在某個語料庫中出現五次，而在另一個語料庫中出現七次，則其計數為 `12`。

    如果您在任何語料庫或文法尚未新增某自訂字組的情況下，將其新增至模型，則會從 `1` 開始計數。如果是先從語料庫或文法新增該字組，之後再加以修改，則計數只會反映在語料庫和文法中發現該字組的次數。

    對於在導入 `count` 欄位之前所建立的自訂模型，此欄位會一直保持在 `0`。若要針對這類模型更新此欄位，請再次新增模型的語料庫和文法，並且在要求中包含 `allow_overwrite` 參數。
    {: note}

如果服務發現自訂字組的定義有一個以上問題，則輸出會包含 `error` 欄位。此欄位提供陣列，列出定義中的每個問題元素，以及說明該問題的訊息。

例如，如果您新增的自訂字組具有無效的 `sounds_like` 欄位，這違反了其中一個新增發音規則，則會發生錯誤。只要自訂模型的字組資源中包含具有錯誤的字組，您就無法訓練該自訂模型。您必須先更正或刪除該字組，才能訓練模型。

### 要求與回應範例
{: #listExample-words}

下列範例列出具有指定自訂作業 ID 之自訂模型中的所有字組（不論類型為何）。這些字組會依預設排序方式（按字母順序遞增）顯示。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
```
{: pre}

此模型的字組資源包含四個字組。第一個字組是由使用者直接新增，但其 `sounds_like` 欄位包含錯誤：「欄位中不得包含數字」。其他字組是由使用者新增，或是由使用者和語料庫兩者新增。

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

下列範例顯示指定模型的字組資源中字組 `NCAA` 的相關資訊：

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
```
{: pre}

使用者一開始就新增該字組，後來服務在 `corpus3` 中發現該字組兩次。

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

## 從自訂語言模型中刪除字組
{: #deleteWord}

使用 `DELETE /v1/customizations/{customization_id}/words/{word_name}` 方法，從自訂語言模型中刪除字組。使用此方法來移除錯誤新增的字組，例如，來自具有錯誤資料的語料庫。

您可以移除透過任何方法新增至自訂模型字組資源的任何字組。不過，您無法從服務的基礎詞彙中刪除字組。從自訂模型中刪除字組時，只會刪除該字組的自訂發音；該字組仍會保留在基礎詞彙中。

在您使用 `POST /v1/customizations/{customization_id}/train` 方法重新訓練模型之前，從自訂模型中移除字組並不會影響模型。如果先前曾使用該字組來訓練模型，則即使將該字組從其字組資源中刪除，此模型還是會繼續將該字組套用至語音辨識。您必須重新訓練模型，以反映刪除結果。

### 要求範例
{: #deleteExample-word}

下列範例從具有指定自訂作業 ID 的自訂模型中刪除字組 `IEEE`：

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IEEE"
```
{: pre}
