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

# 開發人員概觀
{: #developerOverview}

您可以使用 WebSocket 介面，以及使用同步或非同步「HTTP 具象狀態傳輸 (REST)」介面，來存取 {{site.data.keyword.speechtotextfull}} 服務的功能。您也可以自訂服務的語言模型，以符合您的領域及環境。SDK 可用來簡化許多程式設計語言的應用程式開發。
{: shortdesc}

## 服務的程式設計
{: #programming}

[提出辨識要求](/docs/services/speech-to-text/basic-request.html)顯示如何使用每個服務的程式設計介面來要求基本轉錄：

-   [WebSocket 介面](/docs/services/speech-to-text/websockets.html)透過全雙工連線提供有效率、低延遲及高傳輸量的實作。
-   [同步 HTTP 介面](/docs/services/speech-to-text/http.html)提供基本介面來透過區塊處理要求轉錄音訊。
-   [非同步 HTTP 介面](/docs/services/speech-to-text/async.html)提供非區塊處理介面，可讓您登錄回呼 URL 來接收通知，或輪詢服務以取得工作狀態和結果。

介面提供相同的語音辨識功能，但您可以指定相同的參數，諸如要求標頭、查詢參數或 JSON 物件的參數，視您使用的介面而定。此外，WebSocket 和同步 HTTP 介面在單一要求中最多接受 100 MB 的音訊資料。非同步 HTTP 介面最多接受 1 GB 音訊資料。

-   如需所有可用語音辨識參數的相關資訊，請參閱[參數摘要](/docs/services/speech-to-text/summary.html)。
-   如需所有方法及其參數的說明與範例，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/speech-to-text){: new_window}。

## WebSocket 介面的優點
{: #advantages}

WebSocket 介面有一些優於 HTTP 介面的優點：

-   WebSocket 介面提供單一 Socket 全雙工通訊通道。此介面可讓用戶端透過單一連線，以非同步方式將要求及音訊傳送至服務，以及接收結果。
-   它提供更簡單且更強大的程式設計體驗。該服務會針對用戶端的訊息傳送事件導向回應，不需要用戶端輪詢伺服器。
-   它容許您無限期地建立及使用單一已鑑別連線。HTTP 介面需要您鑑別服務的每一個呼叫。
-   它減少延遲。辨識結果會更快送達，因為服務會直接將它們傳送給用戶端。HTTP 介面需要四個不同的要求和連線，才能達到相同的結果。
-   它減少網路使用率。WebSocket 通訊協定為輕量型。它只需要單一連線即可執行即時語音辨識。
-   它可讓音訊直接從瀏覽器（HTML5 WebSocket 用戶端）串流至服務。

## 自訂服務
{: #customizing}

[自訂作業介面](/docs/services/speech-to-text/custom.html)可讓您建立自訂模型來改善服務的語音辨識功能：

-   [自訂語言模型](/docs/services/speech-to-text/language-create.html)可讓您定義基礎模型的領域專用字組。自訂語言模型使用領域（諸如醫療及法律）專用術語來擴充服務的基礎詞彙。
-   [自訂聲學模型](/docs/services/speech-to-text/acoustic-create.html)可讓您針對環境和說話者的聲音特徵來調整基礎模型。自訂聲學模型可改善服務辨識語音之特定聲音特徵的能力。
-   [文法](/docs/services/speech-to-text/grammar.html)可讓您將服務可辨識的詞組限制為文法規則中所定義的那些詞組。藉由限制有效字串的搜尋空間，該服務可以更快速且更精確地傳遞結果。自訂語言模型支援文法。

透過任何服務介面，您可以使用自訂語言模型、自訂聲學模型或同時使用這兩者，來進行語音辨識。

## CORS 支援
{: #cors}

服務支援「跨原點資源共用 (CORS)」。使用 CORS，網頁可以直接從外來網域要求資源。CORS 會規避相同來源安全原則，否則會阻止這類要求。因為服務支援 CORS，所以網頁可以直接與服務通訊，而無需透過管理頁面的 Web 伺服器傳遞要求。

例如，從 {{site.data.keyword.cloud}} 中的伺服器載入的網頁可以直接呼叫自訂 API，略過 {{site.data.keyword.cloud_notm}} 伺服器。如需相關資訊，請參閱 [enable-cors.org ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://enable-cors.org/){: new_window}。

## 使用軟體開發套件
{: #sdks}

SDK 可供 {{site.data.keyword.speechtotextshort}} 服務使用，以簡化語音應用程式的開發。許多熱門的程式設計語言和平台都有可用的 {{site.data.keyword.ibmwatson}} SDK。

-   如需 SDK 及 GitHub 上 SDK 之鏈結的完整清單，請參閱[使用 SDK](/docs/services/watson/getting-started-sdks.html)。
-   如需適用於 {{site.data.keyword.speechtotextshort}} 服務之所有 Node、Java&trade;、Python、Ruby、Swift 及 Go SDK 方法的詳細資訊，請參閱[ API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/speech-to-text){: new_window}。

## 進一步瞭解應用程式開發
{: #learn}

如需使用 {{site.data.keyword.watson}} 服務及 {{site.data.keyword.cloud_notm}} 的相關資訊，請參閱下列各項：

-   如需使用 {{site.data.keyword.watson}} 服務及 {{site.data.keyword.cloud_notm}} 的簡介，請參閱[開始使用 {{site.data.keyword.watson}} 和 {{site.data.keyword.cloud_notm}}](/docs/services/watson/index.html)。
-   所有新的服務實例都會使用 {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) 進行鑑別。舊版服務實例可能會繼續使用來自其現有 Cloud Foundry 服務認證的 `{username}` 及 `{password}` 進行鑑別。如需對服務進行鑑別的相關資訊，請參閱版本注意事項中的 [2018 年 10 月 30 日服務更新](/docs/services/speech-to-text/release-notes.html#October2018b)。
-   如需控制針對所有 {{site.data.keyword.watson}} 服務執行之預設要求記載的相關資訊，請參閱[控制 {{site.data.keyword.watson}} 服務的要求記載](/docs/services/watson/getting-started-logging.html)。
