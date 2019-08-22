---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-24"

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

# 輸入特性
{: #input}

{{site.data.keyword.speechtotextfull}} 服務提供下列特性來指定該服務要如何執行語音辨識要求。下列各節中說明的所有輸入參數都是選用的。只有輸入音訊是必要的。
{: shortdesc}

-   如需每個服務介面的簡單語音辨識要求的範例，請參閱[提出辨識要求](/docs/services/speech-to-text?topic=speech-to-text-basic-request)。
-   如需所有可用語音辨識參數按字母順序排列的清單，包括其狀態（正式發行版本或測試版）和支援的語言，請參閱[參數摘要](/docs/services/speech-to-text?topic=speech-to-text-summary)。

## 自訂模型
{: #custom-input}

在不同的支援層次上（正式發行版本或測試版）對不同語言提供語言和聲學模型自訂作業。如需相關資訊，請參閱[自訂作業的語言支援](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)。
{: note}

所有介面都接受一個用於辨識要求的自訂模型：

-   *自訂語言模型* 使用特定領域中的術語來擴充服務的基礎詞彙。請使用 `language_customization_id` 參數，在要求中包括自訂語言模型。您也可以指定選用的 `customization_weight` 參數。此參數指出的相對加權是提供給來自自訂模型的字組，而不是來自基礎詞彙的字組。

    如需相關資訊，請參閱[使用自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-languageUse)。
-   *自訂聲學模型* 可讓您針對環境和說話者的聲音特徵來調整服務的基礎聲學模型。請使用 `acoustic_customization_id` 參數，在要求中包括自訂聲學模型。您可以在要求中同時指定自訂語言模型和自訂聲學模型。

    如需相關資訊，請參閱[使用自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-acousticUse)。

自訂模型是以[語言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)中所說明的其中一種語言模型為基礎。自訂模型只能與建立它的基礎模型搭配使用。如果自訂模型是以預設值 `en-US_BroadbandModel` 以外的模型為基礎，則您還必須在要求中指定模型名稱。若要使用自訂模型，您必須使用擁有自訂模型之服務實例的認證來發出要求。

如需自訂作業的簡介，請參閱[自訂作業介面](/docs/services/speech-to-text?topic=speech-to-text-customization)。

### 自訂模型範例
{: #customExample}

下列範例要求包括 `language_customization_id` 參數，以使用具有指定 ID 的自訂語言模型。它包含 `customization_weight` 參數，以指出對自訂模型中的字組要賦予 `0.5` 的相對加權。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.5"
```
{: pre}

下列範例要求同時使用自訂語言模型和自訂聲學模型。前者以 `language_customization_id` 參數識別，後者以 `acoustic_customization_id` 參數識別。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&acoustic_customization_id={customization_id}"
```
{: pre}

如需將自訂模型與每個服務介面搭配使用的範例，請參閱：

-   [使用自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-languageUse)
-   [使用自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-acousticUse)

## 文法
{: #grammars-input}

文法特性是測試版功能。該服務對於其支援語言模型自訂作業的所有語言都支援文法。
{: note}

您可以將文法新增至自訂語言模型，並將它們用於語音辨識。文法使用正式語言規格來定義一組用於轉錄字串的正式作業規則。這些規則指定如何從語言的英文字母構成有效的字串。

當您使用文法來進行語音辨識時，該服務只能辨識文法所能辨識的詞組。藉由限制有效字串的搜尋空間，該服務可以更快速且更精確地傳遞結果。

所有介面都接受下列參數來進行辨識要求：

-   `language_customization_id` 參數識別要定義文法的自訂語言模型。您必須使用擁有該模型之服務實例的認證來發出要求。
-   `grammar_name` 參數指定您要使用的文法。您只能在一個要求中指定單一文法。

如需相關資訊，請參閱[將文法與自訂語言模型搭配使用](/docs/services/speech-to-text?topic=speech-to-text-grammars)。

### 文法範例
{: #grammarsExample}

下列範例要求包括 `language_customization_id` 和 `grammar_name` 參數，以限制該服務對於名為 `list-abnf` 的文法中所定義的字串的回應。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&grammar_name=list-abnf"
```
{: pre}

如需將文法與每個服務介面搭配使用的範例，請參閱[使用文法進行語音辨識](/docs/services/speech-to-text?topic=speech-to-text-grammarUse)。

## 基礎模型版本
{: #version}

為了改善語音辨識品質，該服務有時候會更新[語言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)中所說明的基礎語言模型。語言的基礎模型彼此獨立，就像語言的寬頻及窄頻模型那樣。因此，基礎模型的更新彼此獨立進行，且對其他模型沒有影響。

當基礎模型有多個版本存在時，選用的 `base_model_version` 參數可指定要在辨識要求中使用的模型版本。此參數主要是與已針對新基礎模型而更新的自訂模型搭配使用，但在沒有自訂模型的情況下也可以使用它。用於要求的基礎模型版本，視您是否傳遞 `base_model_version` 參數而定。它也取決於您是否在要求中指定自訂模型（語言、聲學或兩者）。

-   *如果您未在要求中指定自訂模型*

    -   省略 `base_model_version` 參數，以使用基礎模型的最新版本。
    -   指定 `base_model_version` 參數，以使用基礎模型的特定版本。

-   *如果您在要求中指定自訂模型*

    -   省略 `base_model_version` 參數，以使用自訂模型升級至的基礎模型最新版本。例如，如果自訂模型升級至基礎模型的最新版本，則該服務會使用基礎模型和自訂模型的最新版本。
    -   指定 `base_model_version` 參數，以使用基礎模型和自訂模型的指定版本。

此參數主要是與自訂模型搭配使用。因此，您只能透過列出以它為基礎的自訂模型的相關資訊，來瞭解基礎模型的可用版本。

-   如需升級自訂模型的相關資訊，請參閱[升級自訂模型](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade)。
-   如需使用不同版本的基礎模型和自訂模型來進行語音辨識的相關資訊，請參閱[使用升級的自訂模型提出辨識要求](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeRecognition)。

## 音訊傳輸
{: #transmission}

*使用 WebSocket 介面*，音訊資料一律透過連線來串流至服務。您可以透過 Socket 一次傳遞所有資料，也可以在資料可用時傳遞現用案例的資料。該服務會在有結果可用時傳回結果。

*使用 HTTP 介面*，您可以透過下列其中一種方式將音訊傳輸至服務：

-   *一次性遞送* - 您省略 `Transfer-Encoding` 標頭，以單次遞送方式，一次將所有音訊資料傳遞至服務。
-   *串流* - 您可以將 `Transfer-Encoding` 要求標頭設為 `chunked` 值，並透過持續性連線來串流資料。在您將資料串流至服務之前，資料不需要完全存在。您可以在資料可用時串流資料。該服務只有在收到最終片段（藉由傳送空的片段來指出）時才會傳送結果。

    如需使用 `Transfer-Encoding` 標頭串流片段音訊的相關資訊，請參閱：
    -   [區塊傳送編碼](https://wikipedia.org/wiki/Chunked_transfer_encoding){: external}
    -   *IETF RFC 7320 HTTP/1.1：訊息語法和遞送* 中的[傳送程式碼](https://tools.ietf.org/html/rfc7230#section-4){: external}

使用 HTTP 介面，該服務會在傳送任何結果之前，一律先轉錄整個音訊串流。結果可以包含多個 `transcript` 元素，以指出以暫停隔開的詞組。連結 `transcript` 元素，以組合完整的文字記錄。

服務會在串流階段作業上強制執行逾時。如果它在 30 秒內偵測到長時間靜音或沒有收到音訊，則可以終止串流階段作業。如需逾時以及如何避免逾時的相關資訊，請參閱[逾時](#timeouts)。

### 音訊傳輸範例
{: #transmissionExample}

下列範例要求對 `Transfer-Encoding` 標頭指定 `chunked`，以使用串流模式。連線保持開啟狀態，以接受其他音訊片段。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--header "Transfer-Encoding: chunked"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## 逾時
{: #timeouts}

當您使用 HTTP 或 WebSocket 語音辨識方法來起始串流階段作業時，該服務會強制執行閒置及階段作業逾時。如果在串流階段作業期間發生逾時失效，服務就會關閉連線。您的應用程式必須從可能的關閉連線中溫和回復。

當您透過 HTTP 來串流音訊時，該服務會每 20 秒在其回應中傳送一個空格字元。該服務這麼做是為了避免掉 30 秒的 HTTP REST 閒置逾時，以改善可用性。為了在進行辨識時保持連線作用中，該服務會繼續傳送此空格字元，直到它完成其轉錄為止。空格字元不會影響 JSON 編碼的回應資料。

這個 HTTP 閒置逾時與該服務的閒置逾時不同。WebSocket 介面不受限於這個 HTTP 逾時。
{: note}

### 閒置逾時
{: #timeouts-inactivity}

當服務正在接收音訊時，卻只偵測到連續靜音或非語音活動（無語音）長達 30 秒，此時即會發生*閒置逾時*（HTTP 狀態碼 400）。該服務會傳送此錯誤訊息：`未偵測到語音的時間達到 30 秒`。閒置逾時很有用，例如，當使用者離開現場麥克風時終止階段作業。

預設閒置逾時為 30 秒。您可以使用 `inactivity_timeout` 參數來置換此值。請指定較大的值來增加閒置逾時值。指定 `-1` 值，可將閒置逾時設為無限。您傳送至該服務的所有音訊都要收費，包括靜音在內，因此，增加閒置逾時會使得只傳送靜音的串流階段作業產生額外的費用。

下列範例要求是將閒置逾時設為 60 秒。此要求會傳送起始檔案，以開始串流階段作業。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Transfer-Encoding: chunked"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?inactivity_timeout=60"
```
{: pre}

### 階段作業逾時
{: #timeouts-session}

當您無法傳送足夠的音訊來保持串流階段作業作用中時，會發生*階段作業逾時*（HTTP 狀態碼 408）。基於下列原因，該服務可能認為階段作業閒置而觸發階段作業逾時：

-   您無法在任何 30 秒的時間範圍內至少傳送 15 秒的音訊至服務。

    除非您傳送最後一個片段來指出串流結束，否則就必須在任何 30 秒的期間內至少傳送 15 秒的音訊。如果您將 `inactivity_timeout` 參數設為較大的值或 `-1`，則音訊可能為靜音。您傳送至服務的任何音訊（包括靜音）的持續時間都要收費。
-   您串流音訊的速度比即時慢上許多。

    在理想狀況下，取得轉錄的音訊之前，您應起始要求來建立階段作業。然後，您要以接近即時的速度傳送音訊來維護階段作業。

在傳送最後一個片段來指出串流結束之後，您就不需要再擔心階段作業逾時。該服務會繼續處理音訊，直到它傳回最終轉錄結果為止。

當您轉錄冗長的音訊串流時，該服務可能需要超過 30 秒的時間來處理音訊並產生回應。要等到該服務完成處理所收到的所有音訊之後，它才會開始計算階段作業逾時。該服務的處理時間不得讓階段作業超出 30 秒階段作業逾時。

例如，如果您在階段作業的前 10 秒內傳送一小時的音訊，該服務可能需要 300 秒來處理此音訊。若要保持此階段作業的作用中狀態，您需要在階段作業開始 340 秒以內，至少再多傳送 15 秒的某種音訊（包括靜音）。

在此範例中，如果您要在階段作業的 100 秒標示處傳送另一個 15 秒音訊，該服務可能要花費額外兩秒來處理此音訊。在此情況下，您必須多傳送 15 秒的音訊進入階段作業中，但不超過 342 秒。

請不要根據處理時間或您是否收到結果來判定串流階段作業是否閒置。假設該服務可以立即處理所有音訊，並據此將資料傳送至服務。如果您即時串流處理音訊，在任何 30 秒時間範圍內，以半即時（15 秒的音訊）傳送音訊時，請不要落後。此速度通常足以應付網路延遲和一般延遲。
{: important}

## 要求記載
{: #logging}

依預設，{{site.data.keyword.IBM_notm}} 會記載對 {{site.data.keyword.watson}} 服務的所有要求及其結果。記載只是為了改善服務以供未來使用者使用。所記載的資料不會共用或公開。


如果您擔心保護使用者個人資訊的隱私權，或不希望 IBM 記載您的要求，您可以選擇不要讓 IBM 記載資料（拒絕）。您可以選擇拒絕帳戶層次或 API 要求層次的記載。如需相關資訊，請參閱[控制 {{site.data.keyword.watson}} 服務的要求記載](/docs/services/watson?topic=watson-gs-logging-overview)。

## 資訊安全
{: #security-input}

資訊安全包括的特性可以建立客戶 ID 與透過要求傳遞給服務之資料的關聯。您藉由在要求中傳遞 `X-Watson-Metadata` 標頭，以建立客戶 ID 與資料的關聯。必要的話，您可以使用 `DELETE /v1/user_data` 方法來刪除資料。如需相關資訊，請參閱[資訊安全](/docs/services/speech-to-text?topic=speech-to-text-information-security)。
