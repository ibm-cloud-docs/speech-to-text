---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-10"

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

# 計價常見問題 (FAQ)
{: #faq-pricing}

1.  <span style="color:#003F69">使用 {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} 標準服務的價格是多少？</span>

    {{site.data.keyword.speechtotextshort}} 服務以每分鐘 $0.02 (USD) 計價。此價格同時適用於寬頻和窄頻模型。如需相關資訊，請參閱 {{site.data.keyword.speechtotextshort}} [服務登入頁面 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/watson/developercloud/speech-to-text.html#pricing-block){: new_window}。

1.  <span style="color:#003F69">何謂「每分鐘計價」？您是針對我傳送給服務的音訊分鐘數或是服務處理音訊所需要的分鐘數來收費？</span>

    價格是根據傳送給服務的音訊量來計算，而不是根據服務處理音訊所需要的時間。

1.  <span style="color:#003F69">您會針對 API 的每個呼叫四捨五入至最近的分鐘嗎？例如，如果我傳送長度各為 30 秒的兩個音訊檔，您會以 2 分鐘或 1 分鐘向我收費？</span>

    {{site.data.keyword.IBM_notm}} 不會對服務接收的每個 API 呼叫的音訊長度執行四捨五入。相反地，{{site.data.keyword.IBM_notm}} 會在月底彙總當月的所有用量，並四捨五入為最接近的分鐘。在此範例中，如果您傳送長度各為 30 秒的兩個音訊檔，{{site.data.keyword.IBM_notm}} 會將音訊總計的持續時間共計為一分鐘，而收費 0.02 美元。

1.  <span id="graduated" style="color:#003F69">用量型分級分層計價如何運作？</span>

    分層計價模型的目的是為繼續使用服務的高用量使用者提供進一步的折扣。一旦達到每月音訊總計的特定臨界值時，就會減少音訊之額外分鐘數的每分鐘計價。如需相關資訊，請參閱服務的[計價頁面 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/catalog/services/speech-to-text){: new_window}。

1.  <span style="color:#003F69">以分層計價而言，假設我使用該服務轉錄一個月 275,000 的音訊分鐘數，我的總收費會是多少？</span>

    -   前 250,000 分鐘的音訊，會向您收取每分鐘 0.02 美元的費用：250,000 * $0.02 = $5000.00 (USD)。
    -   對於其餘的 25,000 分鐘的音訊，會降低費率向您收取每分鐘 0.015 美元的費用：25,000 * $0.015 = $375.00 (USD)。
    -   在此情境中，您當月的總費用為 $5375.00（美元）。

1.  <span style="color:#003F69">使用服務自訂作業介面的價格是多少？是否會向我收取建立及儲存自訂模型的費用？</span>

    對於*語言模型自訂作業*，{{site.data.keyword.IBM_notm}} 不會收取建立或管理自訂語言模型的費用，只有在辨識要求中使用模型時才會收費。使用自訂語言模型進行轉錄，每分鐘將額外收費 0.03 美元。此費用不包括每分鐘 0.02 美元的標準使用收費。它適用於自訂作業介面支援的所有語言。因此，使用自訂語言模型進行語音辨識的總收費是每分鐘 0.05 美元。

    就*聲學模型自訂作業*而言，它是所有受支援語言的測試版介面，{{site.data.keyword.IBM_notm}} 不會對使用自訂聲學模型來建立、管理或辨識語音進行收費。免費使用自訂聲學模型進行辨識要求，在將來可能會有所變更。
