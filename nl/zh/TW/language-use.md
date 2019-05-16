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

# 使用自訂語言模型
{: #languageUse}

建立及訓練自訂語言模型之後，即可將其用於語音辨識要求。使用 `language_customization_id` 查詢參數來指定要求的自訂語言模型，如下列範例所示。您也可以告訴服務要為自訂模型中的字組增加多少權重。如需相關資訊，請參閱[使用自訂權重](#weight)。您必須使用擁有該模型之服務實例的服務認證來發出要求。
{: shortdesc}

您可以針對相同或不同的領域建立多個自訂語言模型。不過，一次只能使用 `language_customization_id` 參數指定一個自訂語言模型。如需將文法與自訂語言模型搭配使用的範例，請參閱[使用文法進行語音辨識](/docs/services/speech-to-text/grammar-use.html)。

-   若為 [WebSocket 介面](/docs/services/speech-to-text/websockets.html)，請使用 `/v1/recognize` 方法。指定的自訂模型會用於透過連線傳送的所有要求。

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=es-ES_BroadbandModel'
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   若為[同步 HTTP 介面](/docs/services/speech-to-text/http.html)，請使用 `POST /v1/recognize` 方法。指定的自訂模型會用於該要求。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}"
    ```
    {: pre}
-   若為[非同步 HTTP 介面](/docs/services/speech-to-text/async.html)，請使用 `POST /v1/recognitions` 方法。指定的自訂模型會用於該要求。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?language_customization_id={customization_id}"
    ```
    {: pre}

如果自訂模型是以預設語言模型 `en-US_BroadbandModel` 為基礎，您可以省略要求中的語言模型。否則，您就必須使用 `model` 參數來指定基礎模型，如 WebSocket 範例所示。自訂模型只能與建立它的基礎模型搭配使用。

## 使用自訂權重
{: #weight}

自訂語言模型是自訂模型與其自訂之基礎模型的組合。您可以告訴服務，與語音辨識基礎模型中的字組相較，要為自訂語言模型中的字組增加多少權重。指派給自訂模型的權重稱為其*自訂權重*。

您可以將自訂語言模型的相對權重指定為介於 0.0 到 1.0 之間的倍數。依預設，每個自訂語言模型的權重為 0.3。在一般情況下，預設權重可產生最佳效能。它容許自訂模型中的 OOV 字組和基礎詞彙中的字組都能獲得辨識。

不過，如果要轉錄的音訊頻繁使用自訂模型中的 OOV 字組，則增加自訂權重可以提升轉錄結果的正確性。在設定自訂權重時，請多加留意。雖然較高的權重可以提升自訂模型領域中的詞組正確性，但也可能對非領域詞組的效能造成負面影響。（即使您將權重設為 0.0，但因為語言模型自訂作業的實作，轉錄結果仍有很小的機率會包含自訂字組）。

您可以使用 `customization_weight` 參數來指定自訂權重。您可以在訓練自訂語言模型時，或是將其用於語音辨識要求時，指定此參數。

-   針對訓練要求，下列範例以 `POST /v1/customizations/{customization_id}/train` 方法指定自訂權重 `0.5`：

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train?customization_weight=0.5"
    ```
    {: pre}

    在訓練期間設定自訂權重會隨著自訂語言模型儲存權重。您不需要隨著使用自訂模型的每個辨識要求來傳遞權重。

-   針對辨識要求，下列範例以 `POST /v1/recognize` 方法指定自訂權重 `0.7`：

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.7"
    ```
    {: pre}

    在語音辨識期間設定自訂權重會置換在訓練期間隨著模型儲存的權重。

## 使用自訂語言模型時的疑難排解
{: #languageTroubleshoot}

如果您將自訂語言模型套用至語音辨識，但發現服務似乎未使用模型所包含的字組，請檢查是否有下列可能的問題：

-   確定您已依照[使用自訂語言模型](#languageUse)中所示，將自訂作業 ID 正確傳遞至辨識要求。
-   確定自訂模型的狀態為 `available`，代表其經過完整訓練，已備妥可供使用。如需相關資訊，請參閱[列出自訂語言模型](/docs/services/speech-to-text/language-models.html#listModels-language)。
-   檢查為新字組產生的發音，確定其正確無誤。如需相關資訊，請參閱[驗證字組資源](/docs/services/speech-to-text/language-resource.html#validateModel)。
