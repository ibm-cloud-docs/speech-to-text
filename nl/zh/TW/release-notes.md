---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-30"

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

# 版本注意事項
{: #release-notes}

以下各節記載針對 {{site.data.keyword.speechtotextfull}} 服務的每個版本和更新所包含的新特性和變更。此資訊包括所有已知限制。除非另有說明，否則所有變更都與舊版相容，且會自動且透通地適用於所有新的和現有的應用程式。
{: shortdesc}

## 已知限制
{: #limitations}

目前沒有已知限制。

## 2019 年 7 月 30 日
{: #July2019}

現在，服務提供六種西班牙文用語的寬頻和窄頻語言模型：

-   阿根廷西班牙文（`es-AR_BroadbandModel` 和 `es-AR_NarrowbandModel`）
-   卡斯提亞西班牙文（`es-ES_BroadbandModel` 和 `es-ES_NarrowbandModel`）
-   智利西班牙文（`es-CL_BroadbandModel` 和 `es-CL_NarrowbandModel`）
-   哥倫比亞西班牙文（`es-CO_BroadbandModel` 和 `es-CO_NarrowbandModel`）
-   墨西哥西班牙文（`es-MX_BroadbandModel` 和 `es-MX_NarrowbandModel`）
-   秘魯西班牙文（`es-PE_BroadbandModel` 和 `es-PE_NarrowbandModel`）

卡斯提亞西班牙文模型並不是新模型。這些模型已正式發行，適用於語音辨識和語言模型自訂作業，但對於聲學模型自訂作業是測試版功能。

其他五種用語是新增的，針對所有用途都是測試版功能。由於這些其他用語是測試版功能，因此它們可能未準備好用於正式作業，並且會隨時變更。這些用語是初始供應項目，預計品質會隨著時間和用量而提高。

如需相關資訊，請參閱下列各節：

-   [支援的語言模型](/docs/services/speech-to-text?topic=speech-to-text-models#modelsList)
-   [自訂作業的語言支援](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)

## 2019 年 6 月 24 日
{: #June2019b}

-   下列窄頻模型已更新，以改善語音辨識：
    -   美式英文窄頻模型 (`en-US_NarrowbandModel`)
    -   巴西葡萄牙文窄頻模型 (`pt-BR_NarrowbandModel`)

    依預設，服務會自動使用更新的模型來處理所有語音辨識要求。如果您的自訂語言模型或自訂聲學模型以這些模型為基礎，則必須使用下列方法來升級現有的自訂模型，以充分運用這些更新項目：
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

如需相關資訊，請參閱[升級自訂模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。
-   現在，服務容許同時提交多個要求，以將不同的音訊資源新增到自訂聲學模型。先前，服務僅容許一次提交一個要求來向自訂聲學模型新增音訊。
-   現在，用於列出有關自訂語言模型和自訂聲學模型的資訊的 HTTP `GET` 方法的輸出包含 `updated` 欄位。該欄位指出前次修改自訂模型的日期和時間，以世界標準時間 (UTC) 表示。
-   在 `strict` 參數設定為 `false` 時，對於自訂模型訓練要求產生的警告，綱目已變更。欄位的名稱 `warning_id` 和 `description` 分別變更為 `code` 和 `message`。如需相關資訊，請參閱 [API 參考資料](https://{DomainName}/apidocs/speech-to-text){: external}。

## 2019 年 6 月 10 日
{: #June2019a}

[處理度量值](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics)僅可用於 WebSocket 和非同步 HTTP 介面。不支援處理度量值用於同步 HTTP 介面。

## 舊版本
{: #older}

-   [2019 年 5 月 17 日](#May2019b)
-   [2019 年 5 月 10 日](#May2019)
-   [2019 年 4 月 19 日](#April2019b)
-   [2019 年 4 月 3 日](#April2019)
-   [2019 年 3 月 21 日](#March2019d)
-   [2019 年 3 月 15 日](#March2019c)
-   [2019 年 3 月 11 日](#March2019b)
-   [2019 年 3 月 4 日](#March2019)
-   [2019 年 1 月 28 日](#January2019)
-   [2018 年 12 月 20 日](#December2018b)
-   [2018 年 12 月 13 日](#December2018a)
-   [2018 年 11 月 12 日](#November2018b)
-   [2018 年 11 月 7 日](#November2018a)
-   [2018 年 10 月 30 日](#October2018b)
-   [2018 年 10 月 9 日](#October2018a)
-   [2018 年 9 月 10 日](#September2018b)
-   [2018 年 9 月 7 日](#September2018a)
-   [2018 年 8 月 8 日](#August2018)
-   [2018 年 7 月 13 日](#July2018)
-   [2018 年 6 月 12 日](#June2018)
-   [2018 年 5 月 15 日](#May2018)
-   [2018 年 3 月 26 日](#March2018b)
-   [2018 年 3 月 1 日](#March2018a)
-   [2018 年 2 月 1 日](#February2018)
-   [2017 年 12 月 14 日](#December2017)
-   [2017 年 10 月 2 日](#October2017)
-   [2017 年 7 月 14 日](#July2017b)
-   [2017 年 7 月 1 日](#July2017a)
-   [2017 年 5 月 22 日](#May2017)
-   [2017 年 4 月 10 日](#April2017)
-   [2017 年 3 月 8 日](#March2017)
-   [2016 年 12 月 1 日](#December2016)
-   [2016 年 9 月 22 日](#September2016)
-   [2016 年 6 月 30 日](#June2016b)
-   [2016 年 6 月 23 日](#June2016a)
-   [2016 年 3 月 10 日](#March2016)
-   [2016 年 1 月 19 日](#January2016)
-   [2015 年 12 月 17 日](#December2015)
-   [2015 年 9 月 21 日](#September2015)
-   [2015 年 7 月 1 日](#July2015)

### 2019 年 5 月 17 日
{: #May2019b}

-   現在，此服務提供語音辨識要求的兩種類型的選用度量值：
    -   [處理度量值](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics)提供關於服務對輸入音訊進行分析時的詳細計時資訊。服務會以指定的間隔傳回度量值，並且附上轉錄事件，例如過渡期間和最終結果。請使用度量值來測量服務的音訊轉錄進度。
    -   [音訊度量值](/docs/services/speech-to-text?topic=speech-to-text-metrics#audio_metrics)提供輸入音訊信號特徵的詳細資訊。結果會在語音處理結束時提供整個輸入音訊的聚集度量值。請使用度量值來判斷音訊的特徵及品質。

    您可以使用任何語音辨識要求來要求這兩種類型的度量值。依預設，服務不會針對要求傳回任何度量值。
-   日文寬頻模型 (`ja-JP_BroadbandModel`) 已更新，以改善語音辨識。依預設，服務會自動使用更新的模型來處理所有語音辨識要求。如果您的自訂語言模型或自訂聲學模型以這些模型為基礎，則必須使用下列方法來升級現有的自訂模型，以充分運用這些更新項目：
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    如需相關資訊，請參閱[升級自訂模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。

### 2019 年 5 月 10 日
{: #May2019}

西班牙文語言模型已更新，以改善語音辨識：

-   `es-ES_BroadbandModel`
-   `es-ES_NarrowbandModel`

依預設，服務會自動使用更新的模型來處理所有語音辨識要求。如果您的自訂語言模型或自訂聲學模型以這些模型為基礎，則必須使用下列方法來升級現有的自訂模型，以充分運用這些更新項目：

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

如需相關資訊，請參閱[升級自訂模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。

### 2019 年 4 月 19 日
{: #April2019b}

-   現在，自訂作業介面的訓練方法包含 `strict` 查詢參數，指出在自訂模型包含有效和無效資源的組合時，訓練是否繼續進行。依預設，如果自訂模型包含一個以上的無效資源，訓練就會失敗。將參數設為 `false` 以容許只要模型包含至少一個有效的資源便繼續進行訓練。服務會將無效的資源排除在訓練之外。
    -   如需將 `strict` 參數用於 `POST /v1/customizations/{customization_id}/train` 方法的相關資訊，請參閱[訓練自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language)和[訓練失敗](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#failedTraining-language)。
    -   如需將 `strict` 參數用於 `POST /v1/acoustic_customizations/{customization_id}/train` 方法的相關資訊，請參閱[訓練自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic)和[訓練失敗](/docs/services/speech-to-text?topic=speech-to-text-acoustic#failedTraining-acoustic)。
-   現在，您最多可以將 9 萬個未登錄詞彙 (OOV) 字組新增至自訂語言模型的字組資源，先前最大值是 3 萬個 OOV 字組。這個數字包括來自所有來源的 OOV 字組（語料庫、文法，以及您直接新增的個別自訂字組）。您可以從所有來源新增總計最多 1 千萬個字組至自訂模型。如需相關資訊，請參閱[我需要多少資料？](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordsResourceAmount)。

### 2019 年 4 月 3 日
{: #April2019}

自訂聲學模型現在最多可接受 200 個小時的音訊。之前的上限是 100 個小時的音訊。

### 2019 年 3 月 21 日
{: #March2019d}

現在，使用者可以只查看與已指派給其 {{site.data.keyword.cloud_notm}} 帳戶之角色相關聯的服務認證資訊。例如，如果您已獲指派 `reader` 角色，則再也看不到任何 `writer` 或更高層次的服務認證。

此變更不會影響含現有服務認證之使用者或應用程式的 API 存取權。此變更只會影響在 {{site.data.keyword.cloud_notm}} 內檢視認證。

如需服務金鑰和使用者角色的相關資訊，請參閱 [IAM 服務 API 金鑰](/docs/services/watson?topic=watson-api-key-bp#api-key-bp)。

### 2019 年 3 月 15 日
{: #March2019c}

此服務現在可支援 A-law (`audio/alaw`) 格式的音訊。如需相關資訊，請參閱 [audio/alaw 格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#alaw)。

### 2019 年 3 月 11 日
{: #March2019b}

-   對於 `max_alternatives` 參數，服務同樣接受值 `0`。如果您指定 `0`，則服務會自動使用預設值 `1`。對於 3 月 4 日服務程式更新所做的變更導致值 `0` 傳回錯誤。（如果您指定負數值，則服務會傳回錯誤。）
-   對於 `word_alternatives_threshold` 參數，服務同樣接受值 `0`。對於 3 月 4 日服務程式更新所做的變更導致值 `0` 傳回錯誤。（如果您指定負數值，則服務會傳回錯誤。）
-   服務現在會以最多兩位小數位數精準度傳回所有信賴分數，此變更包括文字記錄、字組信賴度、替代字組、關鍵字結果和說話者標籤的信賴分數。

### 2019 年 3 月 4 日
{: #March2019}

下列窄頻語言模型已更新，以改善語音辨識：

-   `es-ES_NarrowbandModel`
-   `fr-FR_NarrowbandModel`
-   `pt-BR_NarrowbandModel`

依預設，服務會自動使用更新的模型來處理所有語音辨識要求。如果您的自訂語言模型或自訂聲學模型以這些模型為基礎，則必須使用下列方法來升級現有的自訂模型，以充分運用這些更新項目：

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

如需相關資訊，請參閱[升級自訂模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。

### 2019 年 1 月 28 日
{: #January2019}

WebSocket 介面現在支援瀏覽器型 JavaScript 程式碼中的記號型 Identity and Access Management (IAM) 鑑別。已移除相反的限制。若要使用 WebSocket `/v1/recognize` 方法來建立已鑑別的連線，請執行下列動作：

-   如果您使用 IAM 鑑別，請包括 `access_token` 查詢參數。
-   如果您使用 Cloud Foundry 服務認證，請包括 `watson-token` 查詢參數。

如需相關資訊，請參閱[開啟連線](/docs/services/speech-to-text?topic=speech-to-text-websockets#WSopen)。

### 2018 年 12 月 20 日
{: #December2018b}

自 2019 年 1 月 8 日開始，文法介面在所有位置全面運作。
{: note}

-   此服務現在支援使用文法進行語音辨識。對於所有支援語言模型自訂作業的語言，可提供文法作為測試版功能。您可以將文法新增至自訂語言模型，並將它們用來限制服務可從音訊中辨識的詞組集。您可以用 Augmented Backus-Naur Form (ABNF) 或 XML Form 來定義文法。

    您可以使用下列四種方法來處理文法：
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 會將文法檔新增至自訂語言模型。
    -   `GET /v1/customizations/{customization_id}/grammars` 會列出自訂模型的所有文法相關資訊。
    -   `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` 會傳回自訂模型指定文法的相關資訊。
    -   `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` 會從自訂模型移除現有文法。

    您可以透過 WebSocket 和 HTTP 介面，使用文法來進行語音辨識。使用 `language_customization_id` 和 `grammar_name` 參數來識別您要使用的自訂模型和文法。目前您只能將單一文法與語音辨識要求搭配使用。

    如需文法的相關資訊，請參閱下列文件：
    -   [將文法與自訂語言模型搭配使用](/docs/services/speech-to-text?topic=speech-to-text-grammars)
    -   [瞭解文法](/docs/services/speech-to-text?topic=speech-to-text-grammarUnderstand)
    -   [將文法新增至自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd)
    -   [使用文法進行語音辨識](/docs/services/speech-to-text?topic=speech-to-text-grammarUse)
    -   [管理文法](/docs/services/speech-to-text?topic=speech-to-text-manageGrammars)
    -   [文法範例](/docs/services/speech-to-text?topic=speech-to-text-grammarExamples)

    如需此介面的所有方法的相關資訊，請參閱 [API 參考資料](https://{DomainName}/apidocs/speech-to-text){: external}。
-   現在有一個新的數字編寫特性，可用來遮蔽具有連續三位數以上的數字。編寫的目的是要移除文字記錄中的機密個人資訊，例如信用卡號碼。若要啟用此特性，您可以在辨識要求上，將 `redaction` 參數設為 `true`。此特性為測試版功能，僅適用於美式英文、日文和韓文。如需相關資訊，請參閱[數字編寫](/docs/services/speech-to-text?topic=speech-to-text-output#redaction)。
-   服務現在提供下列新的德文和法文語言模型：
    -   `de-DE_NarrowbandModel`
    -   `fr-FR_NarrowbandModel`

    這兩個新模型都可支援語言模型自訂作業 (GA) 和聲學模型自訂作業（測試版）。如需相關資訊，請參閱[自訂作業的語言支援](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)。
-   新的美式英文語言模型 `en-US_ShortForm_NarrowbandModel` 現已可供使用。這個新模型是用於「互動式語音回應」和「自動化客戶支援」解決方案。此模型可支援語言模型自訂作業 (GA) 和聲學模型自訂作業（測試版）。如需相關資訊，請參閱[美式英文短格式模型](/docs/services/speech-to-text?topic=speech-to-text-models#modelsShortform)。
-   下列語言模型已更新，以改善語音辨識：
    -   `en-GB_NarrowbandModel`
    -   `es-ES_NarrowbandModel`

    依預設，服務會自動使用更新的模型來處理所有語音辨識要求。如果您的自訂語言模型或自訂聲學模型以這些模型為基礎，則必須使用下列方法來升級現有的自訂模型，以充分運用這些更新項目：
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

如需相關資訊，請參閱[升級自訂模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。
-   此服務現在可支援 G.729 (`audio/g729`) 格式的音訊。對於窄頻音訊，此服務僅支援 G.729 Annex D。如需所支援音訊格式的相關資訊，請參閱[音訊格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   說話者標籤特性現在可用於英式英文的窄頻模型 (`en-GB_NarrowbandModel`)。此特性對於所有支援的語言都是測試版功能。如需相關資訊，請參閱[說話者標籤](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels)。

-   您可以新增至自訂聲學模型的音訊數量上限已從 50 個小時增加到 100 個小時。

### 2018 年 12 月 13 日
{: #December2018a}

{{site.data.keyword.cloud_notm}} 倫敦位置 (**eu-gb**) 現在可以使用此服務。與所有位置一樣，倫敦也會使用記號型 IAM 鑑別。所有在這個位置中建立的新服務實例都會使用 IAM 鑑別。

### 2018 年 11 月 12 日
{: #November2018b}

此服務現在支援使用智慧型格式化進行日文語音辨識。先前，此服務只支援美式英文和西班牙文的智慧型格式化。此特性對於所有支援的語言都是測試版功能。如需相關資訊，請參閱[智慧型格式化](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting)。

### 2018 年 11 月 7 日
{: #November2018a}

{{site.data.keyword.cloud_notm}} 東京位置 (**jp-tok**) 現在可以使用此服務。與所有位置一樣，東京也會使用記號型 IAM 鑑別。所有在這個位置中建立的新服務實例都會使用 IAM 鑑別。

### 2018 年 10 月 30 日
{: #October2018b}

所有位置的此服務都已移轉至記號型 IAM 鑑別。所有 {{site.data.keyword.cloud_notm}} 服務現在都會使用 IAM 鑑別。{{site.data.keyword.speechtotextshort}} 服務在每個位置的移轉日期如下：

-   達拉斯 (**us-south**)：2018 年 10 月 30 日
-   法蘭克福 (**eu-de**)：2018 年 10 月 30 日
-   華盛頓特區 (**us-east**)：2018 年 6 月 12 日
-   雪梨 (**au-syd**)：2018 年 5 月 15 日

移轉至 IAM 鑑別會對新的和現有的服務實例產生不同的影響：

-   *所有您在任何位置建立的新服務實例* 現在都會使用 IAM 鑑別來存取服務。您可以傳遞載送記號或 API 金鑰：記號可支援未在每個呼叫中內嵌服務認證的已鑑別要求；API 金鑰會使用 HTTP 基本鑑別。當您使用任何 {{site.data.keyword.watson}} SDK 時，可以傳遞 API 金鑰，並讓 SDK 管理記號的生命週期。
-   *在指出的移轉日期之前建立在某位置中的現有服務實例*，會繼續使用其先前 Cloud Foundry 服務認證中的 `{username}` 和 `{password}` 進行鑑別，直到您移轉它們以使用 IAM 鑑別為止。如需移轉至 IAM 鑑別的相關資訊，請參閱[將 Cloud Foundry 服務實例移轉至資源群組](https://{DomainName}/docs/resources?topic=resources-migrate)。

如需相關資訊，請參閱下列文件：

-   若要瞭解您服務實例所使用的鑑別機制，請在 [{{site.data.keyword.cloud_notm}} 儀表板](https://{DomainName}/dashboard/apps){: external}上按一下該實例，以檢視服務認證。
-   如需搭配使用 IAM 記號與 Watson 服務的相關資訊，請參閱[使用 IAM 記號鑑別](/docs/services/watson?topic=watson-iam)。
-   如需搭配使用 IAM API 金鑰與 Watson 服務的相關資訊，請參閱 [IAM 服務 API 金鑰](/docs/services/watson?topic=watson-api-key-bp)。
-   如需使用 IAM 鑑別的範例，請參閱 [API 參考資料](https://{DomainName}/apidocs/speech-to-text){: external}。

### 2018 年 10 月 9 日
{: #October2018a}

-   `Content-Type` 標頭現在是語音辨識要求的選用項目。此服務現在會自動偵測大部分音訊的音訊格式（MIME 類型）。您必須繼續為下列格式指定內容類型：
    -   `audio/basic`
    -   `audio/l16`
    -   `audio/mulaw`

    若有指出，您為這些格式指定的內容類型必須包含取樣率，並且可以選擇性地包含通道數目和音訊的排列法。對於所有其他音訊格式，您可以省略內容類型，或是指定 `application/octet-stream` 內容類型，讓服務自動偵測格式。

    當您透過 HTTP 介面使用 `curl` 指令來提出語音辨識要求時，必須使用 `Content-Type` 標頭來指定音訊格式，指定 `"Content-Type: application/octet-stream"`，或指定 `"Content-Type:"`。如果您完全省略標頭，`curl` 會使用 `application/x-www-form-urlencoded` 的預設值。本文件中的大部分範例都會繼續為語音辨識要求指定格式，不論是否必要。
    {: important}

    此變更適用於下列方法：
    -   用於 WebSocket 要求的 `/v1/recognize`。您現在透過開啟的 WebSocket 連線來傳送起始要求的文字訊息時，可以選擇是否使用 `content-type` 欄位。
    -   用於同步 HTTP 要求的 `POST /v1/recognize`。`Content-Type` 標頭現在是選用項目。（對於多部分要求，JSON meta 資料的 `part_content_type` 欄位現在也是選用項目。）
    -   用於非同步 HTTP 要求的 `POST /v1/recognitions`。`Content-Type` 標頭現在是選用項目。

    如需相關資訊，請參閱[音訊格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   巴西葡萄牙文寬頻模型 `pt-BR_BroadbandModel` 已更新，以改善語音辨識。依預設，服務會自動使用更新的模型來處理所有辨識要求。如果您的自訂語言模型或自訂聲學模型以此模型為基礎，則必須使用下列方法來升級現有的自訂模型，以充分運用這些更新項目：
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    如需相關資訊，請參閱[升級自訂模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。
-   語音辨識方法的 `customization_id` 參數已淘汰，未來版本中將予以移除。若要為語音辨識要求指定自訂語言模型，請改用 `language_customization_id` 參數。此變更適用於下列方法：
    -   用於 WebSocket 要求的 `/v1/recognize`
    -   用於同步 HTTP 要求（包括多部分要求）的 `POST /v1/recognize`
    -   用於非同步 HTTP 要求的 `POST /v1/recognitions`
-   自 2018 年 10 月 1 日開始，您傳遞至服務以進行語音辨識的所有音訊現在都要收費。您每個月傳送的前 1000 分鐘音訊不再免費。如需服務定價方案的相關資訊，請參閱 [{{site.data.keyword.cloud_notm}} 型錄](https://{DomainName}/catalog/services/speech-to-text){: external}中的 {{site.data.keyword.speechtotextshort}} 服務。

### 2018 年 9 月 10 日
{: #September2018b}

如需自初始版本以來已修正的問題清單，請參閱[已解決的問題](#known_issues)。
{: important}

-   此服務現在支援德文寬頻模型 `de-DE_BroadbandModel`。新的德文模型可支援語言模型自訂作業（通用版）和聲學模型自訂作業（測試版）。
    -   如需此服務如何為德文剖析語料庫的相關資訊，請參閱[英文、法文、德文、西班牙文及巴西葡萄牙文的剖析](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages)。
    -   如需為德文自訂字組建立類似音發音的相關資訊，請參閱[適用於法文、德文、西班牙文及巴西葡萄牙文的準則](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR)。
-   現有的巴西葡萄牙文模型 `pt-BR_BroadbandModel` 和 `pt-BR_NarrowbandModel`，現在可支援語言模型自訂作業（通用版）。這些模型尚未更新來啟用此支援，因此不需要升級現有的自訂聲學模型。
    -   如需此服務如何為巴西葡萄牙文剖析語料庫的相關資訊，請參閱[英文、法文、德文、西班牙文及巴西葡萄牙文的剖析](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages)。
    -   如需為巴西葡萄牙文自訂字組建立類似音發音的相關資訊，請參閱[適用於法文、德文、西班牙文及巴西葡萄牙文的準則](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR)。
-   新版本的美式英文和日文寬頻與窄頻模型已可供使用：
    -   `en-US_BroadbandModel`
    -   `en-US_NarrowbandModel`
    -   `ja-JP_BroadbandModel`
    -   `ja-JP_NarrowbandModel`

    新的模型提供改良的語音辨識。依預設，服務會自動使用更新的模型來處理所有辨識要求。如果您的自訂語言模型或自訂聲學模型以這些模型為基礎，則必須使用下列方法來升級現有的自訂模型，以充分運用這些更新項目：
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    如需相關資訊，請參閱[升級自訂模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。
-   關鍵字辨識和替代字組特性現在是所有語言的通用版 (GA)，而不是測試版功能。如需相關資訊，請參閱：
    -   [關鍵字辨識](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting)
    -   [替代字組](/docs/services/speech-to-text?topic=speech-to-text-output#word_alternatives)

### 已解決的問題
{: #known_issues}

下列與自訂作業介面相關聯的已知問題已獲得解決。此清單是針對過去可能遭遇過這些問題的使用者所維護。

-   如果您將資料新增至自訂語言模型或自訂聲學模型，則必須先重新訓練模型，才能用它來進行語音辨識。此問題會在下列情境中出現：

    1.  使用者建立新的自訂模型（語言或聲學）並訓練該模型。
    1.  使用者將其他資源（字組、語料庫或音訊）新增至自訂模型，但未重新訓練該模型。
    1.  使用者無法使用自訂模型來進行語音辨識。服務與語音辨識要求搭配使用時，傳回下列形式的錯誤：

        ```javascript
        {
          "code_description": "Bad Request",
          "code": 400,
          "error": "Requested custom language model is not available.
                    Please make sure the custom model is trained."
        }
        ```
        {: codeblock}

    若要解決此問題，使用者必須根據其最新資料來重新訓練自訂模型。然後，使用者才能將自訂模型與語音辨識搭配使用。
-   在訓練現有自訂語言或自訂聲學模型之前，您必須將其升級至基礎模型的最新版本。此問題會在下列情境中出現：

    1.  使用者現有自訂模型（語言或聲學）的基礎模型已更新。
    1.  使用者根據舊版的基礎模型來訓練現有自訂模型，而沒有先升級至基礎模型的最新版本。
    1.  使用者無法使用自訂模型來進行語音辨識。

    若要解決此問題，使用者必須使用 `POST /v1/customizations/{customization_id}/upgrade_model` 或 `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` 方法，將自訂模型升級至其基礎模型的最新版本。然後，使用者才能將自訂模型與語音辨識搭配使用。


這兩個問題在正式作業環境中都已獲得修正。

### 2018 年 9 月 7 日
{: #September2018a}

不再支援階段作業型 HTTP REST 介面。文件中已移除與階段作業相關的所有資訊。下列方法無法再使用：
{: important}

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

如果您的應用程式使用階段作業介面，則必須移轉至剩下的其中一個 HTTP REST 介面，或移轉至 WebSocket 介面。如需相關資訊，請參閱 [2018 年 8 月 8 日](#August2018)的服務程式更新。

### 2018 年 8 月 8 日
{: #August2018}

階段作業型 HTTP REST 介面自 **2018 年 8 月 8 日**開始已淘汰。自 **2018 年 9 月 7 日**開始，會從服務移除階段作業 API 的所有方法，之後您再也無法使用此階段作業型介面。這個立即淘汰和 30 天後移除的通知適用於下列方法：

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

如果您的應用程式使用階段作業介面，則必須在 9 月 7 日前移轉至下列其中一個介面：
{: important}

-   對於串流型語音辨識（包括即時使用案例），請使用 [WebSocket 介面](/docs/services/speech-to-text?topic=speech-to-text-websockets)，它可讓您存取臨時結果，並盡量縮短延遲。
-   對於檔案型語音辨識，請使用下列其中一個介面：

    -   對於包含最長幾分鐘的音訊的較短檔案，請使用[同步 HTTP 介面](/docs/services/speech-to-text?topic=speech-to-text-http) (`POST /v1/recognize`) 或[非同步 HTTP 介面](/docs/services/speech-to-text?topic=speech-to-text-async) (`POST /v1/recognitions`)。
    -   對於不只幾分鐘音訊的較長檔案，請使用非同步 HTTP 介面。非同步 HTTP 介面可接受單一要求最多 1 GB 的音訊資料。

WebSocket 和 HTTP 介面提供與階段作業介面相同的結果（只有 WebSocket 介面會提供臨時結果）。您也可以使用其中一個 Watson SDK，以簡化使用任何介面開發應用程式的程序。如需相關資訊，請參閱 [API 參考資料](https://{DomainName}/apidocs/speech-to-text){: external}。

### 2018 年 7 月 13 日
{: #July2018}

西班牙文窄頻模型 `es-ES_NarrowbandModel` 已更新，以改善語音辨識。依預設，服務會自動使用更新的模型來處理所有辨識要求。如果您的自訂語言模型或自訂聲學模型以此模型為基礎，則必須使用下列方法來升級自訂模型，以充分運用這些更新項目：

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

如需相關資訊，請參閱[升級自訂模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。

自此更新開始，下列兩個版本的西班牙文窄頻模型可供使用：

-   `es_ES.8kHz.general.lm20180522235959.am20180522235959`（現行版本）
-   `es_ES.8kHz.general.lm20180308235959.am20180308235959`（舊版）

下列版本的模型無法再使用：

-   `es_ES.8kHz.general.lm20171031235959.am20171031235959`

如果辨識要求嘗試使用的自訂模型是以現在無法使用的基礎模型為基礎，則會使用沒有任何自訂的最新基礎模型。服務會傳回下列警告訊息：`Using non-customized default base model, because your custom {type} model has been built with a version of the base model that is no longer supported.`。若要繼續使用的自訂模型以無法使用的模型為基礎，您必須先使用前述的適當 `upgrade_model` 方法來升級該模型。

### 2018 年 6 月 12 日
{: #June2018}

對於華盛頓特區 (**us-east**) 所管理的應用程式，已啟用下列特性：

-   服務現在支援新的 API 鑑別處理程序。如需相關資訊，請參閱 [2018 年 10 月 30 日服務更新](#October2018b)。
-   服務現在支援 `X-Watson-Metadata` 標頭和 `DELETE /v1/user_data` 方法。如需相關資訊，請參閱[資訊安全](/docs/services/speech-to-text?topic=speech-to-text-information-security)。

### 2018 年 5 月 15 日
{: #May2018}

對於雪梨 (**au-syd**) 所管理的應用程式，已啟用下列特性：

-   服務現在支援新的 API 鑑別處理程序。如需相關資訊，請參閱 [2018 年 10 月 30 日服務更新](#October2018b)。
-   服務現在支援 `X-Watson-Metadata` 標頭和 `DELETE /v1/user_data` 方法。如需相關資訊，請參閱[資訊安全](/docs/services/speech-to-text?topic=speech-to-text-information-security)。

### 2018 年 3 月 26 日
{: #March2018b}

-   服務現在可支援法文語言模型 `fr-FR_BroadbandModel` 的語言模型自訂作業。法文模型為適用於正式作業環境的通用版，可用於語言模型自訂作業。
    -   如需此服務如何為法文剖析語料庫的相關資訊，請參閱[英文、法文、德文、西班牙文及巴西葡萄牙文的剖析](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages)。
    -   如需為法文自訂字組建立類似音發音的相關資訊，請參閱[適用於法文、德文、西班牙文及巴西葡萄牙文的準則](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-esES-frFR)。
-   西班牙文和韓文窄頻模型 `es-ES_NarrowbandModel` 和 `ko-KR_NarrowbandModel`，以及法文寬頻模型 `fr-FR_BroadbandModel` 已更新，以改善語音辨識。依預設，服務會自動使用更新的模型來處理所有辨識要求。如果您的自訂語言模型或自訂聲學模型以這些模型為基礎，則必須使用下列方法來升級自訂模型，以充分運用這些更新項目：

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

如需相關資訊，請參閱[升級自訂模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。
-   下列方法的 `version` 參數已重新命名為 `base_model_version`：

    -   用於 WebSocket 要求的 `/v1/recognize`
    -   用於無階段作業 HTTP 要求的 `POST /v1/recognize`
    -   用於階段作業型 HTTP 要求的 `POST /v1/sessions`
    -   用於非同步 HTTP 要求的 `POST /v1/recognitions`

    `base_model_version` 參數指定要用於語音辨識的基礎模型版本。如需相關資訊，請參閱[使用升級的自訂模型提出辨識要求](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeRecognition)，以及[基礎模型版本](/docs/services/speech-to-text?topic=speech-to-text-input#version)。
-   現在，西班牙文以及美式英文皆可支援智慧型格式化。對於美式英文，此特性現在還會將關鍵字字串轉換成句點、逗點、問號及驚嘆號的標點符號。如需相關資訊，請參閱[智慧型格式化](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting)。

### 2018 年 3 月 1 日
{: #March2018a}

西班牙文和法文寬頻模型 `es-ES_BroadbandModel` 和 `fr-FR_BroadbandModel` 已更新，以改善語音辨識。依預設，服務會自動使用更新的模型來處理所有辨識要求。如果您的自訂語言模型或自訂聲學模型以這些模型為基礎，則必須使用下列方法來升級自訂模型，以充分運用這些更新項目：

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

如需相關資訊，請參閱[升級自訂模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。此節提供升級自訂模型的規則、升級的效果，以及使用已升級模型的方式。

### 2018 年 2 月 1 日
{: #February2018}

此服務現在提供韓語適用的語音辨識模型：`ko-KR_BroadbandModel` 適用於取樣率至少為 16 kHz 的音訊，而 `ko-KR_NarrowbandModel` 適用於取樣率至少為 8 kHz 的音訊。如需相關資訊，請參閱[語言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)。

對於語言模型自訂作業，韓文模型為適用於正式作業環境的通用版；對於聲學模型自訂作業，韓文模型為測試版功能。如需相關資訊，請參閱[自訂作業的語言支援](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)。


-   如需此服務如何為韓文剖析語料庫的相關資訊，請參閱[韓文的剖析](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages-koKR)。
-   如需為韓文自訂字組建立類似音發音的相關資訊，請參閱[適用於韓文的準則](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-koKR)。

### 2017 年 12 月 14 日
{: #December2017}

-   美式英文模型 `en-US_BroadbandModel` 和 `en-US_NarrowbandModel` 已更新，以改善語音辨識。依預設，服務會自動使用更新的模型來處理所有辨識要求。如果您的自訂語言模型或自訂聲學模型以美式英文模型為基礎，則必須使用下列方法來升級自訂模型，以充分運用這些更新項目：

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

如需此程序的相關資訊，請參閱[升級自訂模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。此節提供升級自訂模型的規則、升級的效果，以及使用已升級模型的方式。目前這些方法只適用於新的美式英文基礎模型。然而，當其他基礎模型可供使用時，相同的資訊也適用於其升級版本。
-   在用來提出辨識要求的各種方法中，現在包括一個新的 `base_model_version` 參數，您可用來起始使用舊版或升級版基礎模型和自訂模型的要求。雖然 `base_model_version` 參數主要是與已升級的自訂模型搭配使用，但也可以在沒有自訂模型的情況下使用該參數。如需相關資訊，請參閱[基礎模型版本](/docs/services/speech-to-text?topic=speech-to-text-input#version)。
-   此服務現在可支援聲學模型自訂作業，這是適用於所有可用語言的測試功能。您可以為所有語言的寬頻或窄頻模型建立自訂聲學模型。如需自訂作業的簡介（包括聲學模型自訂作業），請參閱[自訂作業介面](/docs/services/speech-to-text?topic=speech-to-text-customization)。
-   此服務現在可支援英式英文模型 `en-GB_BroadbandModel` 和 `en-GB_NarrowbandModel` 的語言模型自訂作業。雖然此服務處理英式和美式英文語料庫和自訂字組的方法大致類似，但還是有一些重要差異：
    -   如需此服務如何為英式英文剖析語料庫的相關資訊，請參閱[英文、法文、德文、西班牙文及巴西葡萄牙文的剖析](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#corpusLanguages)。
    -   如需為英式英文自訂字組建立類似音發音的相關資訊，請參閱[適用於英文（美式和英式）的準則](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-enUS-enGB)。具體而言，若為英式英文，您不能在類似音發音中使用句點或橫線。
-   語言模型自訂作業和所有關聯的參數現在是通用版 (GA)，適用於所有支援的語言：日文、西班牙文、英式英文及美式英文。

### 2017 年 10 月 2 日
{: #October2017}

-   自訂作業介面現在提供聲學模型自訂作業。您可以建立自訂聲學模型，調整服務的基礎模型來配合您的環境和說話者，以更接近所要轉錄之音訊的聲音特徵的音訊資料，來移入及訓練自訂聲學模型，然後將自訂聲學模型與辨識要求搭配使用，以提升語音辨識的正確性。

    自訂聲學模型與自訂語言模型互補。您可以搭配自訂語言模型來訓練自訂聲學模型，並且可以在語音辨識期間併用這兩種類型的模型。聲學模型自訂作業是測試版介面，僅適用於美式英文、西班牙文及日文。

    -   若要進一步瞭解自訂作業介面所支援的語言，以及每種語言可用的支援層次，請參閱[自訂作業的語言支援](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)。
    -   如需服務自訂作業介面的相關資訊，請參閱[自訂作業介面](/docs/services/speech-to-text?topic=speech-to-text-customization)。
    -   如需建立自訂聲學模型的相關資訊，請參閱[建立自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-acoustic)。
    -   如需使用自訂聲學模型的相關資訊，請參閱[使用自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-acousticUse)。
    -   如需自訂作業介面所有方法的相關資訊，請參閱 [API 參考資料](https://{DomainName}/apidocs/speech-to-text){: external}。
-   對於語言模型自訂作業，服務現在包含一項測試版特性，可以為自訂語言模型設定選用的自訂權重。自訂權重可針對自訂語言模型中的字組與服務基礎詞彙中的字組，指定所要賦予的相對權重。在訓練和語音辨識期間都可以設定自訂權重。如需相關資訊，請參閱[使用自訂權重](/docs/services/speech-to-text?topic=speech-to-text-languageUse#weight)。
-   `ja-JP_BroadbandModel` 語言模型已升級，可擷取基礎模型中的改良功能。此升級不會影響以該模型為基礎的現有自訂模型。
-   服務現在包含一個參數，可指定以 `audio/l16`（線性 16 位元脈衝編碼調變 (PCM)）格式提交的音訊排列法。除了在格式中指定 `rate` 和 `channels` 參數之外，您現在還可以使用 `endianness` 參數來指定 `big-endian` 或 `little-endian`。如需相關資訊，請參閱[音訊格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。

### 2017 年 7 月 14 日
{: #July2017b}

-   此服務現在可支援轉錄 MP3 或 Motion Picture Experts Group (MPEG) 格式的音訊。如需所支援音訊格式的相關資訊，請參閱[音訊格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   語言模型自訂作業介面現在支援西班牙文，作為測試版功能。您可以根據下列其中一個西班牙文基礎語言模型來建立自訂模型：`es-ES_BroadbandModel` 或 `es-ES_NarrowbandModel`；如需相關資訊，請參閱[建立自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-languageCreate)。使用西班牙文自訂語言模型的辨識要求定價，與使用美式英文和日文模型的要求相同。
-   您傳遞至 `POST /v1/customizations` 方法來建立新自訂語言模型的 JSON `CreateLanguageModel` 物件，現在包含 `dialect` 欄位。此欄位可指定要與自訂模型搭配使用之語言的用語。依預設，此用語符合基礎模型的語言。此參數只對西班牙文模型有意義，服務可以針對下列其中一種用語的語音，為其建立適合的自訂模型：
      
    -   `es-ES` 代表卡斯提亞西班牙文（預設值）
    -   `es-LA` 代表拉丁美洲西班牙文
    -   `es-US` 代表北美（墨西哥）西班牙文

    自訂作業介面的 `GET /v1/customizations` 和 `GET /v1/customizations/{customization_id}` 方法會在其輸出中包含自訂模型的用語。如需相關資訊，請參閱[建立自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language)和[列出自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language)。
-   語言模型 `en-UK_BroadbandModel` 和 `en-UK_NarrowbandModel` 的名稱已被淘汰。現在這兩個模型的名稱為 `en-GB_BroadbandModel` 和 `en-GB_NarrowbandModel`。

    已淘汰的 `en-UK_{model}` 名稱會繼續運作，但是 `GET /v1/models` 方法不再於可用模型清單中傳回這些名稱。您還是可以直接使用 `GET /v1/models/{model_id}` 方法來查詢這些名稱。

### 2017 年 7 月 1 日
{: #July2017a}

-   服務的語言模型自訂作業介面現在對於其支援的語言（美式英文和日文）都是通用版 (GA)。{{site.data.keyword.IBM_notm}} 不會針對建立、託管或管理自訂語言模型收費。如下一點所述，針對使用自訂模型的辨識要求，{{site.data.keyword.IBM_notm}} 現在的計費方式為每分鐘音訊加收 $0.03 (USD)。
-   {{site.data.keyword.IBM_notm}} 針對服務的定價更新如下：
    -   免除使用窄頻模型的附加價格
    -   為高流量客戶提供「累進分級定價」
    -   針對使用美式英文或日文自訂語言模型的辨識要求，其計費方式為每分鐘音訊加收 $0.03 (USD)。

    如需定價更新的相關資訊，請參閱：
    -   部落格文章 [{{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} API - Pricing Updates](https://www.ibm.com/blogs/bluemix/2017/05/ibm-watson-speech-text-api-pricing-updates/){: external}
    -   [{{site.data.keyword.cloud_notm}} 型錄](https://{DomainName}/catalog/services/speech-to-text){: external}中的 {{site.data.keyword.speechtotextshort}} 服務
    -   [定價常見問題](/docs/services/speech-to-text?topic=speech-to-text-faq-pricing)
-   針對下列 `POST` 要求，不再需要傳遞空的資料物件作為內文：
    -   `POST /v1/sessions`
    -   `POST /v1/register_callback`
    -   `POST /v1/customizations/{customization_id}/train`
    -   `POST /v1/customizations/{customization_id}/reset`
    -   `POST /v1/customizations/{customization_id}/upgrade_model`

    例如，您現在使用 `curl` 來呼叫 `POST /v1/sessions` 方法，如下所示：

    ```bash
    curl -X POST -u "{username}:{password}"
    --cookie-jar cookies.txt
    "https://stream.watsonplatform.net/speech-to-text/api/v1/sessions"
    ```
    {: pre}

    您不再需要在要求中傳遞下列 `curl` 選項：`--data "{}"`。

    如果您在使用其中一個 `POST` 要求時遇到任何問題，請嘗試在要求內文中傳遞空的資料物件。傳遞空的物件不會以任何方式變更要求的本質或意義。
    {: note}

### 2017 年 5 月 22 日
{: #May2017}

以下是 2017 年 3 月首度宣告的淘汰項目，現在已生效：

-   用來起始辨識要求的所有方法中，已移除 `continuous` 參數。服務現在會轉錄整個音訊串流，直到結束或逾時（以先發生者為準）為止。此行為相當於將之前的 `continuous` 參數設為 `true`。依預設，以前如果省略此參數或將其設為 `false`，服務會在非語音的前半秒（通常是靜音）停止轉錄。

    現有應用程式將此參數設為 `true`，不會看到任何行為變更。應用程式若將此參數設為 `false` 或是根據預設行為，則可能會看到變更。如果要求指定此參數，服務現在會傳回參數不明的警告訊息來回應：

    ```javascript
    "warnings": [
      "Unknown arguments: continuous."
    ]
    ```
    {: codeblock}

    儘管傳回警告，要求還是成功，且現有的階段作業或 WebSocket 連線不受影響。

    {{site.data.keyword.IBM_notm}} 已移除此參數，以回應廣大開發人員社群的意見，他們認為指定 `continuous=false` 沒什麼附加價值，而且會降低整體轉錄的正確性。
-   現在已無法在不傳送音訊的情況下，避免階段作業逾時：
    -   當您使用 WebSocket 介面時，用戶端已無法再藉由傳送將 `action` 參數設為 `no-op` 的 JSON 文字訊息，使連線保持作用中。傳送 `no-op` 訊息不會產生錯誤，但沒有作用。
    -   當您使用階段作業與 HTTP 介面搭配時，用戶端無法再藉由傳送 `GET /v1/sessions/{session_id}/recognize` 要求來延長階段作業。此方法仍然會傳回作用中階段作業的狀態，但不會使階段作業保持作用中。您現在可以執行下列動作來使階段作業保持作用中：
    -   將 `inactivity_timeout` 參數設為 `-1`，以避免 30 秒的閒置逾時。
    -   傳送任何音訊資料（包括只有靜音）至服務，以避免 30 秒的階段作業逾時。您傳送至服務的任何資料（包括您傳送以延長階段作業的靜音）的持續時間都要收費。

    如需相關資訊，請參閱[逾時](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts)。在理想狀況下，您會在取得要轉錄的音訊之前，才建立階段作業，並以接近即時的速度傳送音訊來維護階段作業。此外，也請確定您的應用程式會從關閉的階段作業或連線溫和回復。

    {{site.data.keyword.IBM_notm}} 已移除此功能，以確保繼續為所有使用者提供同級最佳、低延遲率的語音辨識服務。

### 2017 年 4 月 10 日
{: #April2017}

-   此服務現在支援下列寬頻模型的說話者標籤特性：
    -   `en-US-BroadbandModel`
    -   `es-ES-BroadbandModel`
    -   `ja-JP_BroadbandModel`

如需相關資訊，請參閱[說話者標籤](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels)。
-   服務現在支援搭配使用 Opus 或 Vorbis 轉碼器的「Web 媒體 (WebM)」音訊格式。除了 Opus 轉碼器之外，服務現在還支援搭配使用 Vorbis 轉碼器的 Ogg 音訊格式。如需所支援音訊格式的相關資訊，請參閱[音訊格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   服務現在支援「跨原點資源共用 (CORS)」，以容許瀏覽器型用戶端直接呼叫服務。如需相關資訊，請參閱 [CORS 支援](/docs/services/speech-to-text?topic=speech-to-text-developerOverview#cors)。
-   非同步 HTTP 介面現在提供 `POST /v1/unregister_callback` 方法，可移除列入白名單回呼 URL 的登錄項目。如需相關資訊，請參閱[取消登錄回呼 URL](/docs/services/speech-to-text?topic=speech-to-text-async#unregister)。
-   WebSocket 介面不會再因為辨識要求的音訊檔特別長而逾時。您不需要再為了避免逾時，而使用 JSON `start` 訊息來要求臨時結果。（這個問題已在 2016 年 3 月 10 日的版本注意事項中說明。）
-   如果您嘗試刪除不存在的自訂模型，`DELETE /v1/customizations/{customization_id}` 方法現在會傳回 HTTP 回應碼 401。
-   如果您嘗試刪除不存在的語料庫，`DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法現在會傳回 HTTP 回應碼 400。

### 2017 年 3 月 8 日
{: #March2017}

非同步 HTTP 介面現在已為通用版。在此日期之前，它還是測試版功能。

### 2016 年 12 月 1 日
{: #December2016}

-   此服務現在提供測試版的說話者標籤特性，可用於美式英文、西班牙文或日文的窄頻音訊。此特性會在多人交換作業中，識別哪些字是由哪些說話者說出。無階段作業、階段作業型、非同步和 WebSocket 辨識方法各包含一個 `speaker_labels` 參數，可接受布林值來指出回應中要包含哪些說話者標籤。如需此特性的相關資訊，請參閱[說話者標籤](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels)。
-   除了美式英文之外，日文現在也支援測試版語言模型自訂作業介面。此介面的所有方法都支援日文。如需相關資訊，請參閱下列各節：
    -   如需相關資訊，請參閱[建立自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-languageCreate)和[使用自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-languageUse)。
    -   如需新增語料庫文字檔的一般和日文特定考量，請參閱[準備語料庫文字檔](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#prepareCorpus)和[新增語料庫檔案時會發生什麼情況？](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#parseCorpus)。
    -   如需為自訂字組指定 `sounds_like` 欄位時的日文特定注意事項，請參閱[適用於日文的準則](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordLanguages-jaJP)。
    -   如需自訂作業介面所有方法的相關資訊，請參閱 [API 參考資料](https://{DomainName}/apidocs/speech-to-text){: external}。
-   語言模型自訂作業介面現在包含 `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法，可列出指定語料庫的相關資訊。此方法有助於監視將語料庫新增至自訂模型的要求狀態。如需相關資訊，請參閱[列出自訂語言模型的語料庫](/docs/services/speech-to-text?topic=speech-to-text-manageCorpora#listCorpora)。
-   `GET /v1/customizations/{customization_id}/words` 和 `GET /v1/customizations/{customization_id}/words/{word_name}` 方法傳回的 JSON 輸出現在會為每個字組包含 `count` 欄位。此欄位指出在所有語料庫中找到該字組的次數。如果您在任何語料庫尚未新增某自訂字組的情況下，將其新增至模型，則會從 `1` 開始計數。如果是先從語料庫新增該字組，之後再加以修改，則計數只會反映在語料庫中發現該字組的次數。如需相關資訊，請參閱[列出自訂語言模型中的字組](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords)。

    對於在 `count` 欄位存在之前所建立的自訂模型，此欄位會一直保持在 `0`。若要針對這類模型更新此欄位，請再次新增模型的語料庫，並且在 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法中包含 `allow_overwrite` 參數。
-   `GET /v1/customizations/{customization_id}/words` 方法現在包含 `sort` 查詢參數，可控制列出字組的順序。此參數接受兩個引數 `alphabetical` 或 `count` 來指定字組的排序方式。您可以在引數前面加上選用的 `+` 或 `-`，以指定結果是要依遞增還是遞減方式排序。依預設，此方法會按字母順序遞增顯示字組。如需相關資訊，請參閱[列出自訂語言模型中的字組](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords)。

    對於在導入 `count` 欄位之前所建立的自訂模型，使用 `count` 引數與 `sort` 參數搭配並無意義。請使用預設的 `alphabetical` 引數來搭配這類模型。
-   可透過 `GET /v1/customizations/{customization_id}/words` 和 `GET /v1/customizations/{customization_id}/words/{word_name}` 方法在 JSON 回應中傳回的 `error` 欄位現在是陣列。如果服務發現自訂字組的定義有一個以上問題，則此欄位會列出定義中的每個問題元素，並提供說明問題的訊息。如需相關資訊，請參閱[列出自訂語言模型中的字組](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords)。
-   辨識方法的 `keywords_threshold` 和 `word_alternatives_threshold` 已不再接受空值。若要在回應中省略關鍵字和替代字組，請省略這些參數。指定的值必須是浮點。

如需服務介面的相關資訊，請參閱 [API 參考資料](https://{DomainName}/apidocs/speech-to-text){: external}。

### 2016 年 9 月 22 日
{: #September2016}

-   服務現在針對美式英文提供新的測試版語言模型自訂作業介面。您可以在建立包含領域特定術語的自訂語言模型時，使用此介面來修改服務的基礎詞彙和語言模型。您可以個別新增自訂字組，也可以讓服務從語料庫中擷取字組。若要將自訂模型與任何服務介面所提供的語音辨識方法搭配使用，請傳遞 `customization_id` 查詢參數。如需相關資訊，請參閱：
    -   [建立自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-languageCreate)
    -   [使用自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-languageUse)
    -   [API 參考資料](https://{DomainName}/apidocs/speech-to-text){: external}
-   支援的音訊格式清單現在包括 `audio/mulaw`，其提供使用 u-law（或 mu-law）資料演算法進行編碼的單一頻道音訊。當您使用此格式時，必須同時指定擷取音訊的取樣率。如需相關資訊，請參閱[音訊格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   `GET /v1/models` 和 `GET /v1/models/{model_id}` 方法現在會針對每個語言模型，在其輸出中傳回 `supported_features` 欄位。此附加資訊說明模型是否支援自訂作業。如需相關資訊，請參閱 [API 參考資料](https://{DomainName}/apidocs/speech-to-text){: external}。

### 2016 年 6 月 30 日
{: #June2016b}

此測試版非同步 HTTP 介面現在支援服務所支援的所有語言。此介面先前只能用於美式英文。如需相關資訊，請參閱[非同步 HTTP 介面](/docs/services/speech-to-text?topic=speech-to-text-async)及 [API 參考資料](https://{DomainName}/apidocs/speech-to-text){: external}。

### 2016 年 6 月 23 日
{: #June2016a}

-   測試版非同步 HTTP 介面現已可供使用。此介面針對透過非封鎖 HTTP 呼叫的美式英文轉錄作業，提供完整的辨識功能。您可以登錄回呼 URL，並提供使用者指定的機密字串，以利用數位簽章來實現鑑別和資料完整性。如需相關資訊，請參閱[非同步 HTTP 介面](/docs/services/speech-to-text?topic=speech-to-text-async)及 [API 參考資料](https://{DomainName}/apidocs/speech-to-text){: external}。
-   測試版智慧型格式化特性，可在最終的轉錄文字中，將日期、時間、數字串、電話號碼、貨幣值和網際網路位址，轉換成慣用表示法。若要啟用此特性，您可以在辨識要求上，將 `smart_formatting` 參數設為 `true`。此特性為測試版功能，僅適用於美式英文。如需相關資訊，請參閱[智慧型格式化](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting)。
-   支援的語音辨識模型清單現在包括 `fr-FR_BroadbandModel`，適用於取樣率至少為 16 kHz 的法語音訊。如需相關資訊，請參閱[語言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)。
-   支援的音訊格式清單現在包括 `audio/basic`。此格式提供使用取樣率為 8 kHz 的 8 位元 u-law（或 mu-law）資料進行編碼的單一頻道音訊。如需相關資訊，請參閱[音訊格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   許多辨識方法都會傳回 `warnings` 回應，其中包含要求中隨附之無效查詢參數或 JSON 欄位的相關訊息。警告的格式已變更。例如，`"warnings": "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']."` 現在變成 `"warnings": "Unknown arguments: {invalid_arg_1}, {invalid_arg_2}."`
-   針對不另行將資料傳遞至服務的 HTTP `POST` 要求，您必須包含 `{}` 格式的空要求內文。您可以在 `curl` 指令中使用 `--data` 選項來傳遞空的資料。

### 2016 年 3 月 10 日
{: #March2016}

-   兩種形式的資料傳輸（一次性遞送和串流）現在強制規定音訊資料的大小限制為 100 MB，WebSocket 介面也是如此。早期，一次性方式的資料上限為 4 MB。如需相關資訊，請參閱[音訊傳輸](/docs/services/speech-to-text?topic=speech-to-text-input#transmission)（適用於所有介面）和[傳送音訊及接收辨識結果](/docs/services/speech-to-text?topic=speech-to-text-websockets#WSaudio)（適用於 WebSocket 介面）。WebSocket 區段也有討論 WebSocket 介面強制施行的 4 MB 訊框或訊息大小上限。
-   辨識要求的 JSON 回應現在可以包含要求中隨附之無效查詢參數或 JSON 欄位的警告訊息陣列。陣列中的每個元素都是說明警告本質的字串，後面接著無效引數字串的陣列。例如，`"warnings": [ "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']." ]`。如需相關資訊，請參閱 [API 參考資料](https://{DomainName}/apidocs/speech-to-text){: external}。
-   *Apple&reg; iOS 作業系統適用的 {{site.data.keyword.watson}} Speech Software Development Kit (SDK)* 測試版已淘汰。請改用 *Apple&reg; iOS 作業系統適用的 {{site.data.keyword.watson}} SDK*。新的 SDK 可從 GitHub 上 `watson-developer-cloud` 名稱空間中的 [ios-sdk 儲存庫](https://github.com/watson-developer-cloud/ios-sdk){: external}中取得。

WebSocket 介面目前有下列已知問題：

-   對於音訊檔特別長的辨識要求，服務可能需要幾分鐘才能產生最終結果。若為 WebSocket 介面，當服務在準備回應時，基礎 TCP 連線會保持閒置狀態。因此，連線可能會因為逾時而關閉。為避免 WebSocket 介面逾時，請在 JSON 中要求臨時結果 (`\"interim_results\": \"true\"`)，讓 `start` 訊息起始要求。如果您不需要臨時結果，可以將其捨棄。在未來的更新中會解決此問題。

### 2016 年 1 月 19 日
{: #January2016}

服務在 2016 年 1 月 19 日更新，以包含新的褻瀆內容過濾特性。依預設，服務會從美式英文音訊的轉錄結果審查褻瀆內容。如需相關資訊，請參閱[褻瀆內容過濾](/docs/services/speech-to-text?topic=speech-to-text-output#profanity_filter)。

### 2015 年 12 月 17 日
{: #December2015}

-   此服務現在提供關鍵字辨識特性。您可以指定要在輸入音訊中比對的關鍵字字串陣列。您也必須指定使用者定義的信賴水準，字組必須符合該水準，才能視為符合關鍵字。如需相關資訊，請參閱[關鍵字辨識](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting)。    關鍵字辨識特性是測試版功能。
    
-   此服務現在提供替代字組特性。此特性會針對輸入音訊中符合使用者定義信賴水準的字組，傳回替代假設字組。如需相關資訊，請參閱[替代字組](/docs/services/speech-to-text?topic=speech-to-text-output#word_alternatives)。    替代字組特性是測試版功能。
    
-   服務可支援更多語言來使用其轉錄模型：`en-UK_BroadbandModel` 和 `en-UK_NarrowbandModel` 適用於英式英文，而 `ar-AR_BroadbandModel` 適用於現代標準阿拉伯文。如需相關資訊，請參閱[語言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)。
-   HTTP 辨識要求不再受限於 10 分鐘的平台逾時。現在只要辨識作業仍在進行中，服務就會每隔 20 秒在回應 JSON 物件中傳送一個空格字元，以保存連線作用中。如需相關資訊，請參閱[逾時](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts)。
-   服務不再針對階段作業型 HTTP 方法 `GET /v1/sessions/{session_id}/observe_result` 和 `POST /v1/sessions/{session_id}/recognize` 傳回 HTTP 狀態碼 490。服務現在改用 HTTP 狀態碼 400 來回應。

    在服務針對階段作業型方法的錯誤所傳回的 JSON 回應中，現在還包含新的 `session_closed` 欄位。如果階段作業因錯誤而關閉，則此欄位會設為 `true`。如需任何方法的可能回覆碼的相關資訊，請參閱 [API 參考資料](https://{DomainName}/apidocs/speech-to-text){: external}。
-   當您使用 `curl` 指令搭配服務來轉錄音訊時，不再需要使用 `--limit-rate` 選項，以每秒不超過 40,000 個位元組的速率來傳送資料。

### 2015 年 9 月 21 日
{: #September2015}

-   提供了兩個新的測試版行動 SDK 供語音服務使用。這些 SDK 可讓行動應用程式與 {{site.data.keyword.speechtotextshort}} 和 {{site.data.keyword.texttospeechshort}} 服務兩者互動。
    -   *Google Android&trade; 平台適用的 {{site.data.keyword.watson}} Speech SDK* 可支援在您說話時即時將音訊串流至 {{site.data.keyword.speechtotextshort}} 服務，並接收音訊的轉錄文字。此專案包含應用程式範例，以示範與這兩種語音服務的互動狀況。SDK 可從 GitHub 上 `watson-developer-cloud` 名稱空間中的 [speech-android-sdk 儲存庫](https://github.com/watson-developer-cloud/speech-android-sdk){: external}中取得。
    -   *Apple&reg; iOS 作業系統適用的 {{site.data.keyword.watson}} Speech SDK* 可支援將音訊串流至 {{site.data.keyword.speechtotextshort}} 服務，並接收回應中的音訊轉錄文字。SDK 可從 GitHub 上 `watson-developer-cloud` 名稱空間中的 [speech-ios-sdk 儲存庫](https://github.com/watson-developer-cloud/speech-ios-sdk){: external}中取得。

    這兩個 SDK 都支援使用 {{site.data.keyword.cloud_notm}} 服務認證或鑑別記號來向語音服務進行鑑別。

    因為 SDK 是測試版，所以將來可能會有所變更。
    {: note}
-   此服務支援兩種新語言：巴西葡萄牙文和中文。這些新語言的模型為 `pt-BR_BroadbandModel`、`pt-BR_NarrowbandModel`、`zh-CN_BroadbandModel` 和 `zh-CN_NarrowbandModel`。如需相關資訊，請參閱[語言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)。
-   HTTP `POST` 要求 `/v1/sessions/{session_id}/recognize` 和 `/v1/recognize`，以及 WebSocket `/v1/recognize` 要求，都支援轉錄新的媒體類型：`audio/ogg;codecs=opus`（適用於使用 Opus 轉碼器的 Ogg 格式檔案）。此外，這些方法的 `audio/wav` 格式現在支援任何編碼。關於使用線性 PCM 編碼的限制已移除。如需相關資訊，請參閱[音訊格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   在您使用 HTTP 介面轉錄很長的音訊檔案時，此服務現在支援克服逾時的問題。當您使用階段作業時，可以採用長時間輪詢型樣，以 `GET /v1/sessions/{session_id}/observe_result` 和 `POST /v1/sessions/{session_id}/recognize` 方法來指定序列 ID，以處理長時間執行的辨識作業。使用這些方法的新 `sequence_id` 參數，您可以在提交辨識要求之前、期間或之後要求結果。
-   針對美式英文語言模型 `en_US_BroadbandModel` 和 `en_US_NarrowbandModel`，服務現在可以正確地使用許多專有名詞。例如，服務傳回的文字會是 "Barack Obama graduated from Columbia University"，而不是 "barack obama graduated from columbia university"。如果您的應用程式會區分專有名詞的大小寫，您可能會對此變更感興趣。
-   HTTP `DELETE /v1/sessions/{session_id}` 要求不會傳回狀態碼 415「不受支援的媒體類型」。此回覆碼已從該方法的文件移除。

### 2015 年 7 月 1 日
{: #July2015}

服務已在 2015 年 7 月 1 日從測試版移至通用版 (GA)。測試版和通用版 (GA) {{site.data.keyword.speechtotextshort}} API 之間存在下列差異。GA 版本需要使用者升級至新的服務版本。

-   GA 版 HTTP API 與測試版相容。只有在明確指定模型名稱時，才需要變更現有應用程式碼。例如，從 GitHub 取得的服務適用範例程式碼，在 `demo.js` 檔案中包含下列程式碼行：

    ```javascript
    model: 'WatsonModel'
    ```
    {: codeblock}

    此程式碼行為測試版服務指定預設模型 `WatsonModel`。如果您的應用程式也指定此模型，則您需要將其變更為使用 GA 版支援的其中一個新模型。如需相關資訊，請參閱下一點。
-   此服務現在支援一種新的程式設計模型，可讓用戶端與服務透過 WebSocket 連線直接互動。使用此模型，用戶端可以取得鑑別記號，以直接與服務進行通訊。該記號不需要 {{site.data.keyword.cloud_notm}} 中的伺服器端 Proxy 應用程式，即可代表用戶端呼叫服務。記號是用戶端與服務互動的偏好方法。

    服務會繼續支援根據伺服器端 Proxy 的舊程式設計模型，以在用戶端與服務之間轉遞音訊和訊息。但是，新模型更有效率，且提供更高的傳輸量。
-   `POST /v1/sessions` 和 `POST /v1/recognize` 方法，以及 WebSocket `/v1/recognize` 方法，現在都支援 `model` 查詢參數。您可以使用此參數來指定音訊的相關資訊：

    -   語言：*英文*、*日文* 或*西班牙文*
    -   取樣率下限：*寬頻* (16 kHz) 或*窄頻* (8 kHz)

    如需相關資訊，請參閱[語言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)。
-   除了 `audio/flac` 和 `audio/l16` 之外，`recognize` 方法的 `Content-Type` 標頭現在還支援適用於 Waveform Audio File Format (WAV) 檔案的 `audio/wav`。如需所支援音訊格式的相關資訊，請參閱[音訊格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   `recognize` 方法現在可支援一些新的查詢參數，可用來修改服務，以配合您的應用程式需求：
    -   `inactivity_timeout` 參數可設定逾時值（以秒為單位），在此期間之後，如果服務在串流模式中偵測到靜音（無任何語音），則會關閉連線。依預設，服務會在靜音 30 秒之後終止階段作業。如需相關資訊，請參閱[逾時](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts)。
    -   `max_alternatives` 參數會指示服務傳回 *n* 個音訊轉錄的最佳替代假設字組。如需相關資訊，請參閱[替代項目數上限](/docs/services/speech-to-text?topic=speech-to-text-output#max_alternatives)。
    -   `word_confidence` 參數會指示服務針對轉錄的每個字組傳回信賴分數。如需相關資訊，請參閱[字組信賴度](/docs/services/speech-to-text?topic=speech-to-text-output#word_confidence)。
    -   `timestamps` 參數會指示服務針對轉錄的每個字組傳回音訊開始的相對開始和結束時間。如需相關資訊，請參閱[字組時間戳記](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps)。
-   `GET /v1/sessions/{session_id}/observeResult` 方法現在命名為 `GET /v1/sessions/{session_id}/observe_result`。為了舊版相容性，`observeResult` 這個名稱仍受支援。
-   `GET /v1/sessions/{session_id}/observe_result`、`POST /v1/sessions/{session_id}/recognize` 及 `POST /v1/recognize` 方法現在包含標頭參數 `X-WDC-PL-OPT-OUT`，可控制服務是否使用來自要求的音訊和轉錄資料來改善將來的結果。WebSocket 介面包含對等的查詢參數。指定值 `1` 可防止服務使用音訊和轉錄結果。此參數僅適用於現行要求。新標頭會取代測試版 API 中的 `X-logging` 標頭。請參閱[控制 {{site.data.keyword.watson}} 服務的要求記載](/docs/services/watson?topic=watson-gs-logging-overview)。
-   服務現在限制串流模式中的每個階段作業最多可有 100 MB 的資料。您可以指定值 `chunked` 搭配標頭 `Transfer-Encoding` 來指定串流模式。一次性遞送的音訊檔仍強制所傳送的資料大小限制為 4 MB。如需相關資訊，請參閱[音訊傳輸](/docs/services/speech-to-text?topic=speech-to-text-input#transmission)。
-   對於 `/v1/models`、`/v1/models/{model_id}`、`/v1/sessions`、`/v1/sessions/{session_id}`、`/v1/sessions/{session_id}/observe_result`、`/v1/sessions/{session_id}/recognize` 及 `/v1/recognize` 方法，新增錯誤碼 415（「不受支援的媒體類型」）。
-   對用於 `/v1/sessions/{session_id}/recognize` 方法的 `POST` 和 `GET` 要求，修改下列錯誤碼：
    -   錯誤碼 404（「找不到 Session_id」）有較具敘述性的訊息（`POST` 和 `GET`）。
    -   錯誤碼 503（「階段作業已處理要求。在相同的階段作業上不容許並行要求。發生此錯誤之後，階段作業仍在作用中。」）有較具敘述性的訊息（僅適用 `POST`）。
-   對用於 `/v1/sessions` 和 `/v1/recognize` 方法的 HTTP `POST` 要求，可能會傳回錯誤碼 503（「服務無法使用」）。使用 `/v1/recognize` 方法建立 WebSocket 連線時，也可能會傳回此錯誤碼。
