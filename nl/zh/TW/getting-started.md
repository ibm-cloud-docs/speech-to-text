---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-03"

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

## 開始之前
{: #before-you-begin}

- {: hide-dashboard}建立服務的實例：
    1.  {: hide-dashboard}移至「{{site.data.keyword.cloud_notm}} 型錄」中的 [{{site.data.keyword.speechtotextshort}}](https://{DomainName}/catalog/services/speech-to-text){: external} 頁面。
    1.  {: hide-dashboard}註冊免費的 {{site.data.keyword.cloud_notm}} 帳戶，或者登入。
    1.  {: hide-dashboard}按一下**建立**。
-   複製認證以便向服務實例進行鑑別：
    1.  {: hide-dashboard} 在 [{{site.data.keyword.cloud_notm}} 資源清單](https://{DomainName}/resources){: external}中，按一下 {{site.data.keyword.speechtotextshort}} 服務實例以移至 {{site.data.keyword.speechtotextshort}} 服務儀表板頁面。
    1.  在**管理**頁面上，按一下**顯示**以檢視您的認證。
    1.  複製 `API 金鑰`及 `URL` 值。

### 使用 curl 範例
{: #getting-started-curl}

本指導教學使用 `curl` 指令來呼叫服務的 HTTP 介面的方法。請確定已在系統上安裝 `curl` 指令。

1.  若要測試是否已安裝 `curl`，請在指令行上執行下列指令。如果輸出列出支援 Secure Sockets Layer (SSL) 的 `curl` 版本，則已準備好使用本指導教學。

    ```bash
        curl -V
        ```
    {: pre}

1.  如果需要，請從 [curl.haxx.se](https://curl.haxx.se/){: external} 安裝適合您作業系統並已啟用 SSL 的 `curl` 版本。

省略範例中的大括弧。它們指出變數值。
{: tip}

## 步驟 1：轉錄音訊且不使用任何選項
{: #transcribe}

呼叫 `POST /v1/recognize` 方法，以要求 FLAC 音訊檔的基本文字記錄，且不使用其他要求參數。

1.  下載音訊檔範例 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a>。
1.  發出下列指令，以呼叫服務的 `/v1/recognize` 方法進行基本轉錄，且不使用任何參數。範例會使用 `Content-Type` 標頭來指出音訊的類型：`audio/flac`。此範例使用預設語言模型 `en-US_BroadbandModel` 進行轉錄。
    -   {: hide-dashboard}將 `{apikey}` 和 `{url}` 取代為您的 API 金鑰和 URL。
    -   修改 `{path_to_file}` 以指定 `audio-file.flac` 檔的位置。

    *Windows 使用者* 會將每行結尾的反斜線 (``\`) 取代為脫字符號 (``^`)。請確定沒有尾端空格。
    {: tip}

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
              "confidence": 0.96
              "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
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

1.  必要的話，下載音訊檔範例 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示" title="外部鏈結圖示"></a>。
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
              "confidence": 0.96
              "transcript": "several tornadoes touch down as a line
of severe thunderstorms swept through Colorado on Sunday "
            },
            {
              "transcript": "several tornadoes touched down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
            {
              "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado and Sunday "
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

-   在[開發人員概觀](/docs/services/speech-to-text?topic=speech-to-text-developerOverview)進一步瞭解可用來提出語音辨識要求的介面和 SDK。
-   請參閱[提出辨識要求](/docs/services/speech-to-text?topic=speech-to-text-basic-request)中關於每個服務介面的基本語音辨識要求。
-   在 [API 參考資料](https://{DomainName}/apidocs/speech-to-text){: external}中尋找有關服務介面的所有方法的詳細資料。
