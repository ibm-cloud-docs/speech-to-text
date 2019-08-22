---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-19"

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

# 使用音訊資源
{: #audioResources}

您可以將個別音訊檔或包含多個音訊檔的保存檔，新增至自訂聲學模型。建議的方法是藉由新增保存檔來新增音訊資源。建立及新增單一保存檔，效率會比起個別新增多個音訊檔大幅提高。
您還可以提交要求以同時新增多個不同的音訊資源。
{: shortdesc}

## 新增音訊資源
{: #addAudioResource}

請使用 `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法來新增任一種類型的音訊資源至自訂聲學模型。請將音訊資源傳遞作為要求的內文，並包含下列參數：

-   `customization_id` 路徑參數，用來指定模型的自訂作業 ID。
-   `audio_name` 路徑參數，用來指定音訊資源的名稱。
    -   使用符合自訂模型語言並反映資源內容的本地化名稱。
    -   名稱中最多可包含 128 個字元。
    -   請不要使用需要進行 URL 編碼的字元。例如，請不要在名稱中使用空格、斜線、反斜線、冒號、'&' 符號、雙引號、加號、等號、問號等等。（服務不會避免使用這些字元。但因為它們在任何使用之處都要進行 URL 編碼，因此強烈阻止其使用。）
    -   不要使用已新增至自訂模型之音訊資源的名稱。

更新模型的音訊資源時，您必須訓練模型，變更才會在轉錄期間生效。如需相關資訊，請參閱[訓練自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic)。

## 新增音訊檔
{: #addAudioType}

若要將個別的音訊檔新增至自訂聲學模型，請使用 `Content-Type` 標頭指定音訊的格式（MIME 類型）。您可以新增具備支援用於辨識要求之任何格式的音訊。將 `rate`、`channels` 及 `endianness` 參數包含在需要它們的格式規格中。如需所支援音訊格式的相關資訊，請參閱[音訊格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。

音訊資源不支援音訊格式的 `application/octet-stream` 規格。
{: note}

來自[將音訊新增至自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio)的下列範例會新增 `audio/wav` 檔：

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @audio1.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
```
{: pre}

## 新增保存檔
{: #addArchiveType}

將音訊新增至自訂聲學模型的偏好方法是新增包含多個音訊檔的保存檔。您可以藉由使用 `Content-Type` 要求標頭指定保存檔的類型，新增下列類型的保存檔：

-   **.zip** 檔（透過指定 `application/zip`）
-   **.tar.gz** 檔（透過指定 `application/gzip`）

您可能也需要指定 `Concontained-Content-Type` 標頭，視您要新增的檔案格式而定：

-   針對類型為 `audio/alaw`、`audio/basic`、`audio/l16` 或 `audio/mulaw` 的音訊檔，您必須使用 `Contained-Content-Type` 標頭，指定音訊檔的格式。必要時，請包含 `rate`、`channels` 及 `endianness` 參數。在此情況下，包含在保存檔中的所有音訊檔都必須具有相同的音訊格式。
-   針對所有其他類型的音訊檔，您可以省略 `Contained-Content-Type` 標頭。在此情況下，保存檔中包含的音訊檔可以具有未在前一個項目符號中列出的任何格式。它們不需要有相同的格式。

新增音訊類型的資源時，請勿使用 `Contained-Content-Type` 標頭。
{: note}

包含在保存檔類型資源中的音訊檔的名稱最多可以包含 128 個字元。這包括副檔名和名稱的所有元素（例如，斜線）。

來自[將音訊新增至自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio)的下列範例會新增 `application/zip` 檔，檔案中包含以 16 kHz 取樣的 `audio/l16` 格式音訊檔：

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/zip"
--header "Contained-Content-Type: audio/l16;rate=16000"
--data-binary @audio2.zip
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
```
{: pre}

## 新增音訊資源的準則
{: #audioGuidelines}

對於因為使用自訂聲學模型而可以預期獲得的辨識正確性改善程度，取決於許多因素。這些因素包含自訂聲學模型包含的音訊資料量多寡，以及該資料與轉錄音訊之間的相似程度。改善程度也取決於自訂聲學模型是否搭配使用對應的自訂語言模型來進行訓練。

當您將音訊資源新增至自訂聲學模型時，請遵循下列準則：

-   新增至少 10 分鐘且不超過 200 小時的音訊至自訂聲學模型。音訊必須包含語音而非靜音。

    當您決定要新增多少音訊時，音訊的品質也會造成差異。模型的音訊越能反映出要辨識的音訊特徵，自訂模型的語音辨識品質就越好。如果音訊的品質良好，新增更多音訊可以改善轉錄正確性。但新增即使五到十小時的良好品質音訊，也可以產生正面的差異。
-   新增不大於 100 MB 的音訊資源。所有音訊類型和保存檔類型的資源都有 100 MB 大小上限的限制。

    若要使您可以在單一資源新增的音訊量達到最大，請考慮使用提供壓縮的音訊格式。如需相關資訊，請參閱[資料限制和壓縮](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#limits)。
-   將大型音訊檔拆分為多個較小的檔案。請務必在字組之間的靜音位置分割音訊。

    由於可以同時提交多個要求來新增不同的音訊資源，因此可以並行新增更小的檔案。這種平行新增音訊資源的方法可以加快服務的音訊分析速度。
-   新增音訊內容，該內容要反映您計劃轉錄之音訊的聲音頻道狀況。例如，如果您的應用程式所處理的音訊有來自移動中車輛的背景雜訊，則請使用相同類型的資料來建置自訂模型。
-   確定音訊檔的取樣率符合自訂聲學模型之基礎模型的取樣率：
    -   對於寬頻模型，取樣率必須至少為 16 kHz（每秒 16,000 個樣本）。
    -   對於窄頻模型，取樣率必須至少為 8 kHz（每秒 8,000 個樣本）。

如果音訊的取樣率高於最低的必要取樣率，服務便會降低音訊取樣率至適當的速率。如果音訊的取樣率低於最低的必要取樣率，服務會將音訊檔標示為 `invalid`。如果保存檔中包含的任何音訊檔無效，則服務會將整個保存檔視為無效。
-   在下列情況中，請建立自訂語言模型來搭配您的自訂聲學模型使用：
    -   如果音訊長度少於一小時，請根據音訊的轉錄建立自訂語言模型，以達到最佳結果。
    -   如果音訊屬於特定領域，且包含在服務基礎詞彙裡找不到的獨特字組，請使用語言模型自訂作業來擴展服務的基礎詞彙。光是聲學模型自訂作業並不能在轉錄期間產生那些字組。

    如需相關資訊，請參閱[同時使用自訂聲學模型和自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-useBoth)。
