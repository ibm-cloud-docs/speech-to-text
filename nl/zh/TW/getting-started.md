---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

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
{:go: .ph data-hd-programlang='go'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:download: .download}
{:apikey: data-credential-placeholder='apikey'}
{:url: data-credential-placeholder='url'}
{:hide-dashboard: .hide-dashboard}

# 入門指導教學
{: #gettingStarted}

{{site.data.keyword.speechtotextfull}} 服務會將音訊轉錄為文字，以便啟用應用程式的語音轉錄功能。本指導教學是以 curl 為基礎，可協助您快速開始使用服務。範例會顯示如何呼叫服務的 `POST /v1/recognize` 方法來要求文字記錄。
{: shortdesc}

指導教學使用 {{site.data.keyword.cloud}} Identity and Access Management (IAM) API 金鑰來進行鑑別。較舊的服務實例可能繼續使用來自現有 Cloud Foundry 服務認證的 `{username}` 及 `{password}` 以進行鑑別。請使用適合您服務實例的方法來進行鑑別。如需服務之 IAM 鑑別使用的相關資訊，請參閱版本注意事項中的 [2018 年 10 月 30 日服務更新](/docs/services/speech-to-text/release-notes.html#October2018b)。
{: important}

## 開始之前
{: #before-you-begin}

- {: hide-dashboard}建立服務的實例：
    1.  {: hide-dashboard}移至「{{site.data.keyword.cloud_notm}} 型錄」中的 [{{site.data.keyword.speechtotextshort}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/catalog/services/speech-to-text){: new_window} 頁面。
    1.  {: hide-dashboard}註冊免費的 {{site.data.keyword.cloud_notm}} 帳戶，或者登入。
    1.  {: hide-dashboard}按一下**建立**。
-   複製認證以便向服務實例進行鑑別：
    1.  {: hide-dashboard}從 [{{site.data.keyword.cloud_notm}} 儀表板 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/dashboard/apps){: new_window}，按一下 {{site.data.keyword.speechtotextshort}} 服務實例，以前往 {{site.data.keyword.speechtotextshort}} 服務儀表板頁面。
    1.  在**管理**頁面上，按一下**顯示**以檢視您的認證。
    1.  複製 `API 金鑰`及 `URL` 值。
-   確定您有 `curl` 指令。
    -   這些範例使用 `curl` 指令來呼叫 HTTP 介面的方法。從 [curl.haxx.se ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://curl.haxx.se/){: new_window} 安裝作業系統的版本。安裝支援 Secure Sockets Layer (SSL) 通訊協定的版本。請務必在 `PATH` 環境變數中包括已安裝的二進位檔。

輸入指令時，請將 `{apikey}` 和 `{url}` 取代為您的實際 API 金鑰及 URL。請從指令中省略用來指出變數值的大括弧。實際的值類似下列範例：
{: hide-dashboard}

```bash
curl -X POST -u "apikey:L_HALhLVIksh1b73l97LSs6R_3gLo4xkujAaxm7i-b9x"
. . .
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{:pre}
{: hide-dashboard}

## 步驟 1：轉錄音訊且不使用任何選項
{: #transcribe}

呼叫 `POST /v1/recognize` 方法，以要求 FLAC 音訊檔的基本文字記錄，且不使用其他要求參數。

1.  下載範例音訊檔 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a>。
1.  發出下列指令，以呼叫服務的 `/v1/recognize` 方法進行基本轉錄，且不使用任何參數。範例使用 `Content-Type` 標頭來指出音訊的類型：`audio/flac`。此範例使用預設語言模型 `en-US_BroadbandModel` 進行轉錄。
    -   {: hide-dashboard}將 `{apikey}` 和 `{url}` 取代為您的 API 金鑰和 URL。
    -   修改 `{path_to_file}` 以指定 `audio-file.flac` 檔的位置。

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize"{: url}
    ```
    {: pre}

    服務會傳回下列轉錄結果：

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.87
              "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through colorado on sunday "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

## 步驟 2：轉錄音訊並使用選項
{: #transcribeOptions}

呼叫 `POST /v1/recognize` 方法來轉錄相同的 FLAC 音訊檔，但指定兩個轉錄參數。

1.  必要的話，下載範例音訊檔 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a>。
1.  發出下列指令，以呼叫服務的 `/v1/recognize` 方法，並使用兩個額外的參數。將 `timestamps` 參數設定為 `true`，以指出音訊串流中每個字組的開頭和結尾。將 `max_alternatives` 參數設定為 `3`，以接收轉錄的三個最可能替代項目。範例使用 `Content-Type` 標頭來指出音訊的類型 `audio/flac`，而要求使用預設模型 `en-US_BroadbandModel`。
    -   {: hide-dashboard}將 `{apikey}` 和 `{url}` 取代為您的 API 金鑰和 URL。
    -   修改 `{path_to_file}` 以指定 `audio-file.flac` 檔的位置。

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize?timestamps=true&max_alternatives=3"{: url}
    ```
    {: pre}

    服務傳回下列結果，其中包括時間戳記和三個替代的轉錄：

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "timestamps": [
                ["several":, 1.0, 1.51],
                ["tornadoes":, 1.51, 2.15],
                ["touch":, 2.15, 2.5],
                . . .
              ]
            },
            {
              "confidence": 0.87
              "transcript": "several tornadoes touch down as a line
of severe thunderstorms swept through colorado on sunday "
            },
            {
              "transcript": "several tornadoes touched down as a line
of severe thunderstorms swept through colorado on sunday "
            },
            {
              "transcript": "several tornadoes touch down is a line
of severe thunderstorms swept through colorado on sunday "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

## 後續步驟

-   在[開發人員概觀](/docs/services/speech-to-text/developer-overview.html)進一步瞭解可用來提出語音辨識要求的介面和 SDK。
-   請參閱[提出辨識要求](/docs/services/speech-to-text/basic-request.html)中關於每個服務介面的基本語音辨識要求。
-   在 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/speech-to-text){: new_window} 中，尋找服務介面之所有方法的詳細資訊。
