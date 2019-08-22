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

# 建立自訂語言模型
{: #languageCreate}

請遵循下列步驟來為 {{site.data.keyword.speechtotextshort}} 服務建立自訂語言模型：
{: shortdesc}

1.  [建立自訂語言模型](#createModel-language)。您可以針對相同或不同的領域建立多個自訂模型。建立任何模型的程序都相同。不過，在辨識要求中，一次只能指定單一自訂模型。
1.  [將語料庫新增至自訂語言模型](#addCorpus)。語料庫是純文字文件，使用領域環境定義中的術語。服務會從語料庫中擷取其基礎詞彙中所沒有的術語，為自訂模型建置詞彙。您可以將多個語料庫新增至自訂模型。
1.  [將字組新增至自訂語言模型](#addWords)。您也可以將自訂字組個別新增至模型。此外，您還可以使用相同的方法來修改從語料庫中擷取的自訂字組。這些方法可讓您指定字組的發音，以及字組在語音轉錄中的顯示方式。
1.  [訓練自訂語言模型](#trainModel-language)。將字組新增至自訂模型之後，您必須根據這些字組來訓練模型。訓練會準備要用於語音辨識的自訂模型。模型必須經過訓練，才會使用新的或已修改的字組。
1.  訓練自訂模型之後，即可將其用於辨識要求。如果傳遞來進行轉錄的音訊中，包含自訂模型中定義的領域專用字組，則要求的結果會反映模型的加強詞彙。如需相關資訊，請參閱[使用自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-languageUse)。

用來建立自訂語言模型的步驟會反覆進行。您可以新增語料庫、新增字組，並根據需要經常訓練或重新訓練模型。

您也可以將文法新增至自訂語言模型。文法會限制服務只能回應文法所辨識的那些字組。如需相關資訊，請參閱[將文法與自訂語言模型搭配使用](/docs/services/speech-to-text?topic=speech-to-text-grammars)。

語言模型自訂作業適用於大部分語言。如需相關資訊，請參閱[自訂作業的語言支援](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)。
{: note}

## 建立自訂語言模型
{: #createModel-language}

您可以使用 `POST /v1/customizations` 方法來建立新的自訂語言模型。您可以建立任意數目的自訂語言模型，但一次只能將一個模型用於語音辨識要求。此方法接受 JSON 物件，它會將新自訂模型的屬性定義為要求的內文。

<table>
  <caption>表 1. 新自訂語言模型的屬性</caption>
  <tr>
    <th style="width:20%; text-align:left">參數</th>
    <th style="width:12%; text-align:center">資料類型</th>
    <th style="text-align:left">說明</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>必要</em></td>
    <td style="text-align:center">字串</td>
    <td>
新自訂模型的使用者定義名稱。請使用說明自訂模型領域的名稱，例如 <code>Medical custom model</code> 或 <code>Legal custom model</code>。所使用的名稱應為您擁有之所有自訂語言模型中的唯一名稱。請使用符合自訂模型語言的本地化名稱。</td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>必要</em></td>
    <td style="text-align:center">字串</td>
    <td>
要由新自訂模型自訂的語言模型名稱。請指定 <code>GET /v1/models</code> 方法所傳回的其中一個支援語言模型。新模型只能搭配它自訂的基礎模型使用。</td>
  </tr>
  <tr>
    <td><code>dialect</code><br/><em>      選用性</em></td>
    <td style="text-align:center">字串</td>
    <td>
      指定語言中要用於新自訂模型的用語。對於大多數語言，依預設，此用語符合基礎模型的語言。例如，`en-US` 用於任一美式英文語言模型。<br><br>
      對於西班牙文語言，服務會建立適合下列其中一種用語的語音的自訂模型：
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          `es-ES` 代表卡斯提亞西班牙文（`es-ES` 模型）
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          `es-LA` 代表拉丁美洲西班牙文（`es-AR`、`es-CL`、`es-CO` 和 `es-PE` 模型）
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          `es-US` 代表墨西哥（北美）西班牙文（`es-MX` 模型）
        </li>
      </ul>
      此參數僅對西班牙文模型有意義，您一律可以安全地省略此參數，以讓服務建立正確的對映。
        <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          如果對非西班牙文語言模型指定 `dialect` 參數，則其值必須符合基礎模型的語言。
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          如果對西班牙文語言模型指定 `dialect`，則其值必須符合所指出的其中一個已定義對映（`es-ES`、`es-LA` 或 `es-MX`）。
        </li>
      </ul>
      所有 dialect 值都不區分大小寫。
    </td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>      選用性</em></td>
    <td style="text-align:center">字串</td>
    <td>
      新模型的說明。請使用符合自訂模型語言的本地化說明。</td>
  </tr>
</table>

下列範例會建立名為 `Example model` 的新自訂語言模型。會針對基礎模型 `en-US-BroadbandModel` 建立模型，且具有說明 `Example custom language model`。`Content-Type` 標頭指定 JSON 資料正在傳遞給方法。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Example model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom language model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations"
```
{: pre}

範例會傳回新模型的自訂作業 ID。每個自訂模型都以唯一的自訂作業 ID 來識別，此 ID 是一個廣域唯一 ID (GUID)。請使用與模型相關聯之呼叫的 `customization_id` 參數來指定自訂模型的 GUID。

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

新的自訂模型是由將其認證用來建立此模型的服務實例所擁有。如需相關資訊，請參閱[自訂模型的所有權](/docs/services/speech-to-text?topic=speech-to-text-customization#customOwner)。

## 將語料庫新增至自訂語言模型
{: #addCorpus}

建立自訂語言模型之後，下一步是將資料（領域特定字組）新增至模型。將新字組移入自訂模型的建議方法是新增一個以上語料庫。

語料庫是純文字檔案，很適合包含您領域中的句子範例。服務會剖析語料庫檔案的內容，並擷取其基礎詞彙中所沒有的任何字組。這類字組稱為未登錄詞彙 (OOV) 字組。

-   如需使用語料庫的相關資訊，請參閱[使用語料庫](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#workingCorpora)。
-   如需服務如何將語料庫新增至模型的相關資訊，請參閱[新增語料庫檔案時會發生什麼情況？](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#parseCorpus)

語料庫會提供包含新字組的句子，以容許服務學習環境定義中的字組。然後，您可以個別擴增或修改模型的字組。相對於根據從語料庫新增的字組，只根據個別字組來訓練模型會較為耗時，且會產生較不具效益的結果。
{: tip}

您可以使用 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法，將語料庫新增至自訂模型：

-   使用 `customization_id` 路徑參數來指定自訂模型的自訂作業 ID。
-   使用 `corpus_name` 路徑參數指定語料庫的名稱。請使用符合自訂模型語言並反映語料庫內容的本地化名稱。
    -   名稱中最多可包含 128 個字元。
    -   請不要使用需要進行 URL 編碼的字元。例如，請不要在名稱中使用空格、斜線、反斜線、冒號、'&' 符號、雙引號、加號、等號、問號等等。（服務不會避免使用這些字元。但因為它們在任何使用之處都要進行 URL 編碼，因此強烈阻止其使用。）
    -   請不要使用已新增至自訂模型的語料庫或文法名稱。
    -   請不要使用名稱 `user`，服務已保留此名稱來表示使用者所新增或修改的自訂字組。
    -   請不要使用名稱 `base_lm` 或 `default_lm`。兩個名稱都由服務保留供未來之用。
-   將語料庫文字檔當作要求內文來傳遞。

您最多可以新增 9 萬個 OOV 字組，所有來源合計最多 1 千萬個字組，其中包括來自語料庫和文法的字組，以及您直接新增的字組。如需相關資訊，請參閱[我需要多少資料？](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordsResourceAmount)
{: note}

下列範例會將語料庫文字檔 `healthcare.txt` 新增至具有指定 ID 的自訂模型。此範例將語料庫命名為 `healthcare`。

```bash
curl -X POST -u "apikey:{apikey}"
--data-binary @healthcare.txt
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/healthcare"
```
{: pre}

這個方法也接受選用的 `allow_overwrite` 查詢參數，可改寫自訂模型的現有語料庫。如果您在將語料庫檔案新增至模型之後需要更新語料庫檔案，請使用此參數。

此方法為非同步的方法。大約需要一、兩分鐘才會完成。作業的持續時間取決於語料庫中的字組總數、服務在語料庫中找到的新字組數目，以及服務的現行負載。如需檢查語料庫狀態的相關資訊，請參閱[監視新增語料庫要求](#monitorCorpus)。

您可以對每個語料庫文字檔各呼叫一次此方法，將任意數目的語料庫新增至自訂模型。必須徹底完成一個語料庫的新增之後，才能再新增另一個語料庫。將語料庫新增至自訂模型之後，請檢查新的自訂字組以找出拼字和其他錯誤。如需相關資訊，請參閱[驗證字組資源](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel)。

### 監視新增語料庫要求
{: #monitorCorpus}

如果語料庫有效，則服務會傳回 201 回應碼，然後，它會非同步處理語料庫的內容，並自動擷取新的字組。在服務完成現行要求的語料庫分析之前，您無法提交要求將資料新增至自訂模型，或是訓練模型。

若要判定分析的狀態，請使用 `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法來輪詢語料庫的狀態。此方法接受模型的 ID 和語料庫的名稱，如下列範例所示：

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

回應包括語料庫的狀態：

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

`status` 欄位具有下列其中一個值：

-   `analyzed` 表示服務已順利分析語料庫。
-   `being_processed` 表示服務仍在分析語料庫。
-   `undetermined` 表示服務在處理語料庫時發生錯誤。

使用迴圈，每隔 10 秒檢查一次語料庫的狀態，直到它變成 `analyzed` 為止。如需檢查模型語料庫狀態的相關資訊，請參閱[列出自訂語言模型的語料庫](/docs/services/speech-to-text?topic=speech-to-text-manageCorpora#listCorpora)。

## 將字組新增至自訂語言模型
{: #addWords}

雖然將字組新增至自訂語言模型的建議方法是新增語料庫，但您還是可以直接新增個別自訂字組至模型。服務將自訂字組新增至自訂模型的方式，就像新增從語料庫發現的 OOV 字組一樣。

如果您只需要將一個或少數幾個字組新增至模型，使用語料庫來新增字組可能不實際，或甚至不可行。最簡單的方法是新增一個只有拼字的字組。然而，您也可以為該字組提供多種發音，並指出如何顯示該字組。

-   如需直接新增字組的相關資訊，請參閱[使用自訂字組](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#workingWords)。
-   如需服務如何將自訂字組新增至模型的相關資訊，請參閱[新增或修改自訂字組時會發生什麼情況？](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#parseWord)

您可以使用下列方法將字組新增至自訂模型：

-   `POST /v1/customizations/{customization_id}/words` 方法可一次新增多個字組。您可以透過要求的內文來傳遞 JSON 物件，以提供每個字組的相關資訊。下列範例會將兩個自訂字組 `HHonors` 和 `IEEE` 新增至具有指定 ID 的自訂模型。`Content-Type` 標頭指定 JSON 資料正在傳遞給方法。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"words\": [
      {\"word\": \"HHonors\", \"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"},
      {\"word\": \"IEEE\", \"sounds_like\": [\"I. triple E.\"]}]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    您也可以從檔案新增字組。例如，`words.json` 檔案會定義相同的兩個自訂字組。

    ```
    {"words":
      [
        {"word": "HHonors", "sounds_like": ["hilton honors", "H. honors"], "display_as": "HHonors"},
        {"word": "IEEE", "sounds_like": ["I. triple E."]}
      ]
    }
    ```
    {: codeblock}

    下列指令會從檔案中新增字組：

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data-binary @words.json
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

此方法為非同步，完成作業所需的時間，取決於您新增的字組數目和服務的現行負載。如需檢查作業狀態的相關資訊，請參閱[監視新增字組要求](#monitorWords)。
-   `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法可新增個別字組。您可以傳遞 JSON 物件，以提供字組的相關資訊。下列範例會將字組 `NCAA` 新增至具有指定 ID 的模型。`Content-Type` 標頭同樣是指定將 JSON 資料傳遞給方法。

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

    此方法為同步。服務會立即傳回回應碼，以報告要求成功或失敗。

如同新增語料庫，請檢查新的自訂字組以找出拼字和其他錯誤。當您一次新增多個字組時，此檢查尤其重要。如需相關資訊，請參閱[驗證字組資源](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel)。

### 監視新增字組要求
{: #monitorWords}

當您使用 `POST /v1/customizations/{customization_id}/words` 方法時，如果輸入資料有效，服務會傳回 201 回應碼，然後，非同步處理字組，以將其新增至模型。此作業通常會比新增語料庫或訓練模型更快，但其所需的時間，取決於您新增的字組數目和服務的現行負載。在服務完成新增字組的要求之前，您無法提交要求將資料新增至自訂模型，或是訓練模型。

若要判定要求的狀態，請使用 `GET /v1/customizations/{customization_id}` 方法來輪詢模型的狀態。此方法接受模型的自訂作業 ID，並傳回包含模型狀態的資訊，如下列範例所示：

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

`status` 欄位報告模型的現行狀態。當服務在處理新的字組時，狀態會維持為 `pending`。使用迴圈，每隔 10 秒檢查一次狀態，直到它變成 `ready` 為止，表示作業已完成。如需可能的 `status` 值的相關資訊，請參閱[監視訓練模型要求](#monitorTraining-language)。

### 修改自訂模型中的字組
{: #modifyWord}

您也可以使用 `POST /v1/customizations/{customization_id}/words` 和 `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法來修改或擴增自訂模型中的字組。您可能會需要使用這些方法來更正將字組新增至模型時所造成的拼字錯誤或其他錯誤。您可能也需要為現有字組新增類似音定義。

您可以使用這些方法來修改現有字組的定義，就像新增字組一樣。您為字組提供的新資料會改寫字組的現有定義。

## 訓練自訂語言模型
{: #trainModel-language}

在將新字組移入自訂語言模型（藉由新增語料庫、新增文法或直接新增字組）之後，您必須根據新資料來訓練模型。訓練會準備要將資料用於語音辨識的自訂模型。在根據資料訓練模型之前，模型不會使用您透過任何方法新增的字組。

您可以使用 `POST /v1/customizations/{customization_id}/train` 方法來訓練自訂模型。將您想要訓練之模型的自訂作業 ID 傳遞給方法，如下列範例所示：

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

此方法為非同步的方法。訓練需要幾分鐘才能完成，取決於用來訓練模型的新字組數目和服務的現行負載。如需檢查訓練作業狀態的相關資訊，請參閱[監視訓練模型要求](#monitorTraining-language)。

此方法包含下列選用性的查詢參數：

-   `word_type_to_add` 參數指定要用來訓練自訂模型的字組：
    -   指定 `all` 或省略此參數會根據其所有字組來訓練模型，不論字組來源為何。
    -   指定 `user` 只會根據使用者新增或修改的字組來訓練模型，而忽略只從語料庫或文法中擷取的字組。

    如果您新增的語料庫含有混雜資料（例如包含拼字錯誤的字組），這個選項會很有幫助。在根據這類資料訓練模型之前，您可以使用 `GET /v1/customizations/{customization_id}/words` 方法的 `word_type` 查詢參數，以檢閱從語料庫和文法中擷取的字組。如需相關資訊，請參閱[列出自訂語言模型中的字組](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords)。
-   將自訂模型用於語音辨識時，`customization_weight` 參數指定的相對權重是提供給來自自訂模型的字組，而不是來自基礎詞彙的字組。您也可以隨著使用自訂模型的任何辨識要求來指定自訂權重。如需相關資訊，請參閱[使用自訂權重](/docs/services/speech-to-text?topic=speech-to-text-languageUse#weight)。
-   `strict` 參數指出在自訂模型包含有效和無效資源的組合（語料庫、文法和字組）時，訓練是否繼續進行。依預設，如果模型包含一個以上的無效資源，訓練便會失敗。將參數設為 `false` 以容許只要模型包含至少一個有效的資源便繼續進行訓練。服務會將無效的資源排除在訓練之外。如需相關資訊，請參閱[訓練失敗](#failedTraining-language)。

### 監視訓練模型要求
{: #monitorTraining-language}

如果服務順利起始訓練程序，則會傳回 200 回應碼。在現有要求完成之前，該服務無法接受後續訓練要求，或是新增語料庫、文法或字組的要求。

若要判定訓練要求的狀態，請使用 `GET /v1/customizations/{customization_id}` 方法來輪詢模型的狀態。此方法接受模型的自訂作業 ID，並傳回模型的相關資訊。

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

回應包含 `status` 和 `progress` 欄位，用來報告自訂模型的狀態。`progress` 欄位的意義取決於模型的狀態。`status` 欄位可以具有下列其中一個值：

-   `pending` 指出已建立模型，但模型正在等待新增有效的訓練資料，或是正在等待服務完成已新增之資料的分析。`progress` 欄位是 `0`。
-   `ready` 指出模型包含有效的資料，且已準備好進行訓練。`progress` 欄位是 `0`。

    如果模型包含有效和無效資源的組合（例如，有效和無效的自訂字組），則除非您將 `strict` 查詢參數設定為 `false`，否則模型訓練會失敗。如需相關資訊，請參閱[訓練失敗](#failedTraining-language)。
-   `training` 指出模型正在訓練中。當訓練完成時，`progress` 欄位會從 `0` 變更為 `100`。
-   `available` 指出模型已訓練好，可以使用。`progress` 欄位是 `100`。
-   `upgrading` 指出模型正在升級中。`progress` 欄位是 `0`。
-   `failed` 指出模型的訓練失敗。`progress` 欄位為 `0`。如需相關資訊，請參閱[訓練失敗](#failedTraining-language)。

使用迴圈，每隔 10 秒檢查一次狀態，直到它變成 `available` 為止。如需檢查自訂模型狀態的相關資訊，請參閱[列出自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language)。

### 訓練失敗
{: #failedTraining-language}

如果服務正在處理自訂語言模型的另一個要求，則訓練無法開始。例如，服務正在執行下列作業時，無法啟動訓練要求，狀態碼為 409：

-   正在處理語料庫或文法，以產生 OOV 字組清單
-   正在處理自訂字組，以驗證或自動產生類似音發音
-   正在處理另一個訓練要求

如果自訂模型存在以下情況，訓練也無法啟動，狀態碼為 400：

-   從自訂模型建立或前次訓練之後，未包含任何新的有效訓練資料（語料庫、文法或字組）
-   包含一個以上無效的語料庫、文法或字組（例如，自訂字組具有無效的發音相似的讀法）

如果訓練要求失敗且狀態碼為 400，則服務會將自訂模型的狀態設定為 `failed`。採取下列其中一項動作：

-   使用自訂作業介面的方法來檢查模型的資源，並修正您發現的所有錯誤：
    -   對於無效的語料庫，可以更正語料庫文字檔，然後使用 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法的 `allow_overwrite` 參數將更正後的檔案新增到模型。如需相關資訊，請參閱[將語料庫新增至自訂語言模型](#addCorpus)。
    -   對於無效的文法，可以更正文法檔，然後使用 `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法的 `allow_overwrite` 參數將更正後的檔案新增到模型。如需相關資訊，請參閱[將文法新增至自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar)。
    -   對於無效的自訂字組，可以使用 `POST /v1/customizations/{customization_id}/words` 或 `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法直接在模型的字組資源中修改該字組。如需相關資訊，請參閱[修改自訂模型中的字組](#modifyWord)。

    如需驗證自訂語言模型中字組的相關資訊，請參閱[驗證字組資源](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel)。
-   將 `POST /v1/customizations/{customization_id}/train` 方法的 `strict` 參數設定為 `false`，以從訓練排除無效的資源。模型必須包含至少一個有效資源（語料庫、文法或字組），訓練才能成功。`strict` 參數對於訓練包含有效和無效資源組合的自訂模型非常有用。

## Script 範例
{: #exampleScripts}

您可以使用下列 Script 來實驗建立自訂語言模型的步驟：

-   名為 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a> 的 Python Script。如需相關資訊，請參閱 [Python Script 範例](#pythonScript)。
-   名為 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a> 的 Bash Shell Script。如需相關資訊，請參閱 [Shell Script 範例](#shellScript)。

這兩個 Script 提供相同的功能。每個 Script 都會建立自訂模型、從語料庫文字檔中新增字組，以及直接將單一和多個字組新增至模型。Script 會查詢模型，以列出從語料庫新增的字組，以及直接由使用者新增的字組。它也會訓練模型，並示範建議用來監視非同步作業結果的輪詢。

您可以將提供的這兩個語料庫文字檔之一與 Script 搭配使用，也可以使用自己的語料庫檔案來進行測試：

-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/corpus.txt" download="corpus.txt">corpus.txt <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a> 是縮減的 6 KB 語料庫，其將六個醫療保健相關術語新增至模型。此檔案與 Script 搭配使用時，會產生少量輸出。依預設，這些 Script 會使用此檔案。
-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/healthcare.txt" download="healthcare.txt">healthcare.txt <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a> 是較豐富的 164 KB 語料庫，其將許多醫療保健相關術語新增至模型。此檔案與 Script 搭配使用時，會產生更多輸出。

依預設，您使用任一 Script 建立的新自訂模型都可與辨識要求搭配使用。這些 Script 中包含刪除新自訂模型的選用步驟，這在您實驗此程序時會很有幫助。請遵循 Script 中的註解來啟用刪除步驟。

### Python Script 範例
{: #pythonScript}

請遵循下列步驟來使用 Python Script：

1.  下載名為 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a> 的 Python Script。
1.  下載語料庫範例文字檔來與 Script 搭配使用。您可以使用其中一個語料庫文字檔或使用自己選擇的檔案來進行測試。依預設，所有語料庫文字檔都必須位於 Script 所在的相同目錄中。
1.  Script 使用 Python `requests` 程式庫來處理傳送至服務的 HTTP 要求。請使用 `pip` 或 `easy_install` 來安裝程式庫，以供 Script 使用，例如：

    ```bash
    pip install requests
    ```
    {: pre}

    如需程式庫的相關資訊，請參閱 [pypi.python.org/pypi/requests](https://pypi.python.org/pypi/requests){: external}。
1.  編輯 Script，將 `password` 字串 `iam_apikey` 取代為 {{site.data.keyword.speechtotextshort}} 認證中的 API 金鑰：

    ```
    password = "iam_apikey"
    ```
    {: codeblock}

1.  輸入下列指令來執行 Script：

    ```bash
    python testSTTcustom.py
    ```
    {: pre}

此 Script 使用下列預設 URL 代表達拉斯位置：`https://stream.watsonplatform.net`。如果您將服務實例建立在不同的位置，請修改 `uri` 變數，以使用您的位置。例如，如果您的服務實例位於華盛頓特區位置，請使用 `https://gateway-wdc.watsonplatform.net`。
{: note}

### Shell Script 範例
{: #shellScript}

請遵循下列步驟來使用 Bash Shell Script：

1.  下載名為 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a> 的 Shell Script。
1.  下載語料庫範例文字檔來與 Script 搭配使用。您可以使用其中一個語料庫文字檔或使用自己選擇的檔案來進行測試。依預設，所有語料庫文字檔都必須位於 Script 所在的相同目錄中。
1.  Script 使用 `curl` 指令來處理傳送至服務的 HTTP 要求。如果尚未下載 `curl`，則可以從 [curl.haxx.se](http://curl.haxx.se){: external} 安裝適用於您作業系統的版本。請安裝支援 Secure Sockets Layer (SSL) 通訊協定的版本，且務必在 `PATH` 環境變數中包含已安裝的二進位檔。
1.  編輯 Script，將 `PASSWORD` 字串 `iam_apikey` 取代為 {{site.data.keyword.speechtotextshort}} 認證中的 API 金鑰：

    ```
    PASSWORD="iam_apikey"
    ```
    {: codeblock}

1.  編輯 Script，以將 `URL` 字串取代為您建立服務實例所在位置的 URL。此 Script 使用下列預設 URL 代表達拉斯位置：

    ```
    URL="https://stream.watsonplatform.net/speech-to-text/api/v1"
    ```
    {: codeblock}

1.  請確定 Script 具有執行檔許可權，然後輸入下列指令來執行 Script：

    ```bash
    ./testSTTcustom.sh
    ```
    {: pre}
