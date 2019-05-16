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

# 將文法新增至自訂語言模型
{: #grammarAdd}

您必須先使用自訂作業介面將文法新增至自訂語言模型，然後才能使用文法進行語音辨識。將文法新增至自訂語言模型的步驟，與用來新增語料庫或自訂字組的那些步驟極為相似。
{: shortdesc}

1.  [建立自訂語言模型](#createModel-grammar)。您可以建立新的自訂模型或使用現有的模型。
1.  [將文法新增至自訂語言模型](#addGrammar)。服務會驗證文法，以確保其正確性。
1.  [驗證文法中的字組](#validateGrammar)。您可以驗證文法所辨識之任何未登錄詞彙 (OOV) 字組的類似音發音的正確性。
1.  [訓練自訂語言模型](#trainGrammar)。服務會準備要用於語音辨識的自訂模型和文法。
1.  您現在可以在語音辨識要求中使用自訂模型和文法。如需相關資訊，請參閱[使用文法進行語音辨識](/docs/services/speech-to-text/grammar-use.html)。

這些是反覆的步驟。您可以視需要，經常地在自訂語言模型中新增文法以及語料庫和自訂字組。您必須在您新增的任何資料資源（文法、語料庫或自訂字組）上訓練自訂模型。當您用它來進行語音辨識時，自訂模型會反映其前次訓練所在的資料。

您可以將文法與支援語言模型自訂作業的任何語言搭配使用。語言模型自訂作業適用於大部分語言。如需相關資訊，請參閱[自訂作業的語言支援](/docs/services/speech-to-text/custom.html#languageSupport)。
{: note}

## 建立自訂語言模型
{: #createModel-grammar}

若要搭配使用文法與語音辨識，您必須將文法新增至自訂語言模型。您可以使用現有的自訂語言模型，也可以使用 `POST /v1/customizations` 方法來建立新的自訂模型。如需建立新的自訂模型的相關資訊，請參閱[建立自訂語言模型](/docs/services/speech-to-text/language-create.html#createModel-language)。

自訂語言模型可以包含語料庫和自訂字組以及文法。在語音辨識期間，您可以使用搭配或不搭配其文法的自訂模型。不過，當您使用文法時，該服務只會辨識指定文法中的字組。

## 將文法新增至自訂語言模型
{: #addGrammar}

您可以使用 `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法，將文法檔新增至自訂模型。

-   使用 `customization_id` 路徑參數來指定自訂語言模型的自訂 ID。
-   使用 `grammar_name` 路徑參數指定文法的名稱。請使用符合自訂模型語言並反映文法內容的本地化名稱。
    -   名稱中最多可包含 128 個字元。
    -   請不要在名稱中包含空格、斜線或反斜線。
    -   請不要使用已新增至自訂模型的文法或語料庫名稱。
    -   請不要使用名稱 `user`，服務已保留此名稱來表示使用者所新增或修改的自訂字組。

-   請使用 `Content-Type` 要求標頭來指定文法的格式：
    -   `application/srgs` 代表 ABNF 文法
    -   `application/srgs+xml` 代表 XML 文法

-   傳遞包含文法的檔案作為要求的內文。

下列範例會將名為 `confirm.abnf` 的文法檔新增至具有指定 ID 的自訂模型。此範例將文法命名為 `confirm-abnf`。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/srgs"
--data-binary @confirm.abnf
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

這個方法也接受選用的 `allow_overwrite` 查詢參數，您可以包含這個參數來改寫具有相同名稱的現有文法。如果您在將文法新增至模型之後需要更新文法，請使用此參數。

此方法為非同步。視文法的大小和服務的現行負載而定，服務可能需要幾秒鐘來分析文法。如需檢查文法狀態的相關資訊，請參閱[監視新增文法要求](#monitorGrammar)。

您可以對每個文法檔呼叫此方法一次，將任何數目的文法新增至自訂模型。必須徹底完成一個文法的新增，然後才能新增另一個文法。

### 監視新增文法要求
{: #monitorGrammar}

如果文法有效，則服務會傳回 201 回應碼。然後，它會非同步處理文法的內容，並自動擷取文法可以辨識的新字組。在服務對現行要求的文法分析完成之前，您無法提交要求將其他資料資源新增至自訂模型，或訓練模型。

若要判定分析的狀態，請使用 `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法來輪詢文法的狀態。此方法接受自訂模型的 ID 和文法的名稱。下列範例檢查名為 `confirm-abnf` 的文法的狀態。

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

回應包括文法的狀態：

```javascript
{
  "name": "confirm-abnf",
  "out_of_vocabulary_words": 0,
  "status": "being_processed"
}
```
{: codeblock}

`status` 欄位具有下列其中一個值：

-   `analyzed` 指出服務已順利分析文法。
-   `being_processed` 指出服務仍在分析文法。
-   `undetermined` 指出服務在處理文法時發生錯誤。

使用迴圈，每 10 秒檢查一次文法的狀態，直到它變成 `analyzed` 為止。如需檢查文法狀態的相關資訊，請參閱[列出自訂語言模型的文法](/docs/services/speech-to-text/grammar-manage.html#listGrammars)。

### 處理新增文法失敗
{: #handleFailures}

如果其文法分析失敗，該服務會將文法的狀態設為 `undetermined`，並包含 `error` 欄位用來說明文法狀態的失敗。您也可以使用 `GET /v1/customizations/{customization_id}` 方法來檢查自訂模型的狀態。如果新增文法失敗，則輸出會包括類似下列內容的錯誤訊息：

```javascript
{
  . . .
  "error": "{\"code\":500, \"code_description\":\"Internal Server Error\",
\". . . Cannot compile grammar . . .\"},
  . . .
}
```
{: codeblock}

自訂模型的狀態不受錯誤影響，但該文法無法與模型搭配使用。您可以藉由更正文法檔並重複該要求將它新增至自訂模型，來解決問題。將 `allow_overwrite` 參數設為 `true`，以改寫失敗的文法檔的版本。

您也可以暫時從自訂模型中刪除該文法，然後更正文法，並在日後再次新增文法檔。如需刪除文法的相關資訊，請參閱[從自訂語言模型中刪除文法](/docs/services/speech-to-text/grammar-manage.html#deleteGrammar)。

## 驗證文法中的字組
{: #validateGrammar}

當您將文法檔新增至自訂語言模型時，服務會剖析文法，以判定文法是否可辨識不屬於該服務基礎詞彙的所有字組。這類字組稱為未登錄詞彙 (OOV) 字組。服務會將 OOV 字組新增至自訂模型的字組資源。字組資源的用途是定義尚未出現在服務詞彙中的字組。

字組資源中的定義告訴服務如何轉錄 OOV 字組。此資訊包含告訴服務這個字如何發音的 `sounds_like` 欄位，告訴服務如何顯示這個字的 `display_as` 欄位，以及指出如何將這個字新增至自訂模型的 `source` 欄位。如需字組資源和 OOV 字組的相關資訊，請參閱[字組資源](/docs/services/speech-to-text/language-resource.html#wordsResource)。

將文法新增至自訂模型之後，檢查模型字組資源中的 OOV 字組以驗證其類似音發音，是很好的作法。並非所有文法都有 OOV 字組，但驗證字組資源通常是好主意。若要在新增文法之後檢查自訂模型的字組，請使用下列方法：

-   使用 `GET /v1/customizations/{customization_id}/words` 方法，列出自訂模型中的所有字組。使用該方法的 `word_type` 參數傳遞 `grammars` 值，只會列出從文法中新增的字組。
-   使用 `GET /v1/customizations/{customization_id}/words/{word_name}` 方法，檢視模型中的個別字組。

驗證字組的類似音發音是否精確且正確無誤。同時也尋找字組本身的拼字錯誤和其他錯誤。如需驗證及更正自訂模型中字組的相關資訊，請參閱[驗證字組資源](/docs/services/speech-to-text/language-resource.html#validateModel)。

## 訓練自訂語言模型
{: #trainGrammar}

在搭配使用文法與自訂語言模型之前的最終步驟是訓練模型。您可以使用您在新的語料庫或新的自訂字組上用來訓練自訂模型的相同程序。訓練模型會編譯在語音辨識期間要使用的文法。此服務會將文法從其原始文字型格式處理成二進位運行環境格式，以進行語音辨識。除非您訓練模型，否則無法使用文法。

您使用 `POST /v1/customizations/{customization_id}/train` 方法來訓練自訂模型。將您想要訓練之模型的自訂作業 ID 傳遞給方法，如下列範例所示。

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

訓練方法非同步。訓練通常只需要幾秒鐘即可完成。但是，視文法的大小和複雜性以及服務的現行負載而定，有可能需要更長的時間。如需檢查訓練作業狀態的相關資訊，請參閱[監視訓練要求](#monitorTraining-grammar)。

### 監視訓練要求
{: #monitorTraining-grammar}

如果順利起始訓練程序，則服務會傳回 200 回應碼。在現行訓練要求完成之前，該服務無法接受後續訓練要求，或是要將新的文法、語料庫或字組新增至自訂模型的要求。

若要判定訓練要求的狀態，請使用 `GET /v1/customizations/{customization_id}` 方法來輪詢模型的狀態。該方法接受模型的自訂作業 ID，並傳回類似下列內容的模型相關資訊：

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

訓練模型時，`status` 欄位含有 `training` 值。使用迴圈，每 10 秒檢查一次模型的狀態，直到它變成 `available` 為止。如需監視訓練要求及其他可能狀態值的相關資訊，請參閱[監視訓練模型要求](/docs/services/speech-to-text/language-create.html#monitorTraining-language)。
