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

# 關於
{: #about}

**服務更新：***{{site.data.keyword.speechtotextshort}} 服務已於 2019 年 7 月 30 日更新。現在，服務為其他五種西班牙文用語提供測試版模型：阿根廷、智利、哥倫比亞、墨西哥和秘魯。如需新西班牙文用語的相關資訊，請參閱版本注意事項中的 [2019 年 7 月 30 日服務更新](/docs/services/speech-to-text?topic=speech-to-text-release-notes#July2019)。*

{{site.data.keyword.speechtotextfull}} 服務為應用程式提供語音轉錄功能。該服務運用機器學習來結合文法知識、語言結構以及音訊和語音信號組合，以精確地轉錄人聲。它會隨著收到更多的語音，而持續地更新及調整其轉錄。
{: shortdesc}

該服務提供各種介面，適用於以語音作為輸入與以文字記錄作為輸出的任何應用程式。應用程式範例包括：

-   應用程式、內嵌裝置及汽車配件的語音控制
-   轉錄會議及電話會議
-   口述電子郵件訊息和注意事項

此服務對於需要從呼叫中心音訊中解壓縮高品質語音文字記錄的客戶來說是理想的選擇。金融服務、醫療衛生、保險和電信等行業的客戶可以開發用於客戶關懷、客戶語音、代理人協助和其他解決方案的雲端原生應用程式。

## 支援的介面
{: #interfaces}

{{site.data.keyword.speechtotextshort}} 服務提供三個用於語音辨識的介面：

-   [WebSocket 介面](/docs/services/speech-to-text?topic=speech-to-text-websockets)，用來建立與服務之間的持續、全雙工、低延遲連線。您可以使用單一要求，將最多 100 MB 的音訊資料傳遞至服務。
-   對服務進行基本 HTTP 呼叫的[同步 HTTP 介面](/docs/services/speech-to-text?topic=speech-to-text-http)。您可以使用要求來傳遞最多 100 MB 的音訊資料。
-   對服務進行非區塊處理呼叫的[非同步 HTTP 介面](/docs/services/speech-to-text?topic=speech-to-text-async)。您可以使用要求來傳遞最多 1GB 的音訊資料。

該服務也提供[自訂作業介面](/docs/services/speech-to-text?topic=speech-to-text-customization)，您可以用來針對您的語言和聲學需求而調整語音辨識。您可以使用領域專用術語來擴充模型的詞彙，或針對音訊的聲音特徵來調整模型。您也可以新增[文法](/docs/services/speech-to-text?topic=speech-to-text-grammars)，以限制服務可以辨識的詞組。

-   如需使用服務進行應用程式開發的高階說明，請參閱[開發人員概觀](/docs/services/speech-to-text?topic=speech-to-text-developerOverview)。
-   如需每個服務介面的基本語音辨識要求的範例，請參閱[提出辨識要求](/docs/services/speech-to-text?topic=speech-to-text-basic-request)。

許多程式設計語言都有可用的 SDK 可用來簡化服務的使用。如需相關資訊，請參閱 [API 參考資料](https://{DomainName}/apidocs/speech-to-text){: external}。

## 輸入特性
{: #inputFeatures}

該服務的介面共用語音轉文字的一般輸入特性：

-   [音訊格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats) - 您可以搭配使用下列格式來轉錄 Ogg 或「Web 媒體 (WebM)」音訊：Opus 或 Vorbis 轉碼器、MP3（或 MPEG）、Waveform Audio File Format (WAV)、「自由無損音訊轉碼器 (FLAC)」、「線性 16 位元脈衝編碼調變 (PCM)」、G.729、A-Law、mu-law（或 u-law）及基本音訊。使用支援壓縮的格式，您可以將在一個要求中可傳送的音訊資料量最大化。
-   [語言和模型](/docs/services/speech-to-text?topic=speech-to-text-models) - 對於大部分語言，您可以使用寬頻或窄頻模型來轉錄音訊。對於以 16 kHz 的最小速度取樣的音訊使用寬頻。對於以 8 kHz 的最小速度取樣的音訊使用窄頻。
-   [音訊傳輸](/docs/services/speech-to-text?topic=speech-to-text-input#transmission) - 您可以用資料片段的連續串流方式或用一次性遞送方式（一次傳遞所有資料）來傳遞音訊。該服務使用串流來強制執行閒置和階段作業[逾時](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts)。

## 輸出特性
{: #outputFeatures}

這些介面也支援下列一般輸出特性：

-   [說話者標籤](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels)，從美式英文、英式英文、西班牙文或日文的音訊中，辨識不同的說話者。轉錄會標示每個說話者對於多參與者交談的貢獻。（測試版功能。）
-   [關鍵字辨識](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting)，識別與指定的關鍵字字串相符且具有使用者定義之信賴水準的口說詞組。關鍵字辨識特別適用於音訊中的個別詞組比完整轉錄更重要的情況。例如，客戶支援中心系統可能會識別關鍵字，以決定如何遞送使用者要求。
-   [臨時結果](/docs/services/speech-to-text?topic=speech-to-text-output#interim)，在轉錄進行時傳回漸進假設。該服務會在轉錄完成時傳回最終結果。僅 WebSocket 介面有提供臨時結果。
-   [替代項目數上限](/docs/services/speech-to-text?topic=speech-to-text-output#max_alternatives)，提供可能的替代文字記錄。該服務指出最終結果，其具有最大信賴度。
-   [替代字組](/docs/services/speech-to-text?topic=speech-to-text-output#word_alternatives)，要求其發音類似於文字記錄字組的替代字組。
-   [字組信賴度](/docs/services/speech-to-text?topic=speech-to-text-output#word_confidence)，針對文字記錄的每個字組傳回信賴水準。
-   [字組時間戳記](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps)，針對文字記錄的每個字組傳回開始和結束的時間戳記。
-   [智慧型格式化](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting)，在最終文字記錄中，將日期、時間、數字、貨幣值、電話號碼及網際網路位址轉換為更具可讀性的慣用格式。若為美式英文，您也可以提供關鍵字詞組，在最終文字記錄中包含特定標點符號。美式英文、日文和西班牙文音訊支援智慧型格式化。（測試版功能。）
-   [數字編寫](/docs/services/speech-to-text?topic=speech-to-text-output#redaction)，編寫或遮蔽最終文字記錄中的數字資料。編寫的目的是要移除文字記錄中的機密個人資訊，例如信用卡號碼。美式英文、日文和韓文音訊支援此特性。（測試版功能。）
-   [褻瀆過濾](/docs/services/speech-to-text?topic=speech-to-text-output#profanity_filter)，審查美式英文文字記錄中的褻瀆文字。
-   [處理度量值](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics)提供關於服務對輸入音訊進行分析時的詳細計時資訊。
-   [音訊度量值](/docs/services/speech-to-text?topic=speech-to-text-metrics#audio_metrics)提供輸入音訊信號特徵的詳細資訊。

## 語言支援
{: #languages}

此服務提供下列語言和用語的模型：

-   阿拉伯文（現代標準）
-   巴西葡萄牙文
-   中文（普通話）
-   英文（英國和美國）
-   法文
-   德文
-   日文
-   韓文
-   西班牙文（阿根廷、卡斯提亞、智利、哥倫比亞、墨西哥和秘魯）

服務不支援所有語言的所有特性。此外，它支援可作為正式作業用途之正式發行版本（GA 版）的部分特性，以及作為不同語言的測試版供應項目的其他特性。

-   西班牙文卡斯提亞用語已正式發行。其他五種西班牙文用語是測試版功能。
-   WebSocket 和 HTTP 介面已正式發行，適用於所有語言。
-   該服務提供不同語言的寬頻模型、窄頻模型或兩者。如需相關資訊，請參閱[語言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)。
-   某些語音辨識特性僅適用於部分語言。如需相關資訊，請參閱[參數摘要](/docs/services/speech-to-text?topic=speech-to-text-summary)。
-   語言模型自訂作業介面已正式發行，適用於大部分語言。聲學模型自訂作業介面可作為所有語言的測試版功能使用。如需相關資訊，請參閱[自訂作業的語言支援](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)。


## 計價
{: #pricing-index}

如需服務定價方案的相關資訊，請參閱 [{{site.data.keyword.cloud}} 型錄](https://{DomainName}/catalog/services/speech-to-text){: external}中的 {{site.data.keyword.speechtotextshort}} 服務。如需與計價和每月使用成本範例相關問題的答案，請參閱[計價常見問題 (FAQ)](/docs/services/speech-to-text?topic=speech-to-text-faq-pricing)。

## 試用服務
{: #tryOut}

如需 {{site.data.keyword.speechtotextshort}} 服務運行狀況的範例，請參閱：

-   [快速示範](https://speech-to-text-demo.ng.bluemix.net/){: external}，它轉錄來自串流音訊輸入或您所上傳檔案中的文字。
-   示範該服務的 {{site.data.keyword.ibmwatson}} [入門範本套件](http://www.ibm.com/watson/developercloud/starter-kits.html){: external}中的應用程式。
-   {{site.data.keyword.watson}} 部落格文章 [Getting robots to listen: Using {{site.data.keyword.watson}}'s {{site.data.keyword.speechtotextshort}} service](https://www.ibm.com/blogs/watson/2016/07/getting-robots-listen-using-watsons-speech-text-service/){: external}，其顯示如何使用該服務的 WebSocket 介面與 Python 搭配，以從音訊中擷取語音。該文章提供示範程式碼及相關步驟的完整指導教學。
