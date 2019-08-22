---

copyright:
  years: 2017, 2019
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

# 建立自訂聲學模型
{: #acoustic}

請遵循下列步驟，為 {{site.data.keyword.speechtotextshort}} 服務建立自訂聲學模型：
{: shortdesc}

1.  [建立自訂聲學模型](#createModel-acoustic)。您可以為相同或不同的領域或環境建立多個自訂模型。對於您建立的任何模型，此程序都相同。不過，在辨識要求中，您一次只能指定單一自訂聲學模型。
1.  [將音訊新增至自訂聲學模型](#addAudio)。服務針對聲學建模和語音辨識，接受相同的音訊檔格式。它也接受包含多個音訊檔的保存檔。保存檔是新增音訊資源的偏好方法。您可以重複此方法，將更多音訊或保存檔新增至自訂模型。
1.  [訓練自訂聲學模型](#trainModel-acoustic)。將音訊資源新增至自訂模型之後，您必須訓練模型。訓練會準備好自訂聲學模型，以便用於語音辨識。訓練可能會花費大量時間。訓練長度取決於模型包含的音訊資料量。

    您可以在訓練自訂聲學模型期間，指定說明程式自訂語言模型。自訂語言模型中如果包含音訊檔的轉錄或來自您音訊檔領域的 OOV 字組，便可以改善自訂聲學模型的品質。如需相關資訊，請參閱[搭配使用自訂語言模型來訓練自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-useBoth#useBothTrain)。
1.  訓練自訂模型之後，就可以將它用於辨識要求中。如果傳遞給轉錄的音訊聲音品質類似於自訂模型，則結果便會反映服務經過加強的理解。如需相關資訊，請參閱[使用自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-acousticUse)。

    您可以在相同的辨識要求中同時傳遞自訂聲學模型和自訂語言模型，以進一步改善辨識正確性。如需相關資訊，請參閱[在語音辨識期間使用自訂語言模型及自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-useBoth#useBothRecognize)。

建立自訂聲學模型的步驟會反覆執行。您可以按照需要的頻率，新增或刪除音訊，然後訓練或重新訓練模型。您必須重新訓練模型，才能使音訊的任何變更生效。重新訓練模型時，所有音訊資料（而不只是新的資料）都會用於訓練。因此，訓練時間會與模型中包含的音訊總量相稱。

聲學模型自訂作業已針對所有語言提供為測試版功能。如需相關資訊，請參閱[自訂作業的語言支援](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)。
{: note}

## 建立自訂聲學模型
{: #createModel-acoustic}

請使用 `POST /v1/acoustic_customizations` 方法來建立新的自訂聲學模型。您可以建立任意數目的自訂聲學模型，但一次只能在一個語音辨識要求中使用一個自訂聲學模型。此方法接受 JSON 物件作為要求的內文，該物件會定義新自訂模型的屬性。

<table>
  <caption>表 1. 新自訂聲學模型的屬性</caption>
  <tr>
    <th style="width:20%; text-align:left">參數</th>
    <th style="width:12%; text-align:center">資料類型</th>
    <th style="text-align:left">說明</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>必要</em></td>
    <td style="text-align:center">字串</td>
    <td>
      新自訂聲學模型的使用者定義名稱。所使用的名稱請說明自訂模型的聲音環境，例如 <code>Mobile custom model</code> 或 <code>Noisy car custom
      model</code>。請使用在您擁有之所有自訂聲學模型之間都唯一的名稱。請使用符合自訂模型語言的本地化名稱。</td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>必要</em></td>
    <td style="text-align:center">字串</td>
    <td>
      要由新模型自訂的基礎模型名稱。您必須使用
      <code>GET /v1/models</code> 方法所傳回的模型名稱。新的自訂模型只能搭配它自訂的基礎模型使用。</td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>      選用性</em></td>
    <td style="text-align:center">字串</td>
    <td>
      新模型的說明。請使用符合自訂模型語言的本地化說明。</td>
  </tr>
</table>

下列範例會建立名為 `Example acoustic model` 的新自訂聲學模型。會針對基礎模型 `en-US_BroadbandModel` 建立模型，且其說明為 `Example custom acoustic model`。`Content-Type` 標頭指定 JSON 資料正在傳遞給方法。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Example acoustic model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom acoustic model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations"
```
{: pre}

範例會傳回新模型的自訂作業 ID。每個自訂模型都以唯一的自訂作業 ID 來識別，此 ID 是一個廣域唯一 ID (GUID)。請使用與模型相關聯之呼叫的 `customization_id` 參數來指定自訂模型的 GUID。

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

新的自訂模型是由將其認證用來建立此模型的服務實例所擁有。如需相關資訊，請參閱[自訂模型的所有權](/docs/services/speech-to-text?topic=speech-to-text-customization#customOwner)。

## 將音訊新增至自訂聲學模型
{: #addAudio}

建立自訂聲學模型之後，下一步是在其中新增音訊資源。請使用 `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法來新增音訊資源至自訂模型。您可以新增

-   任何格式的個別音訊檔，該格式須受到語音辨識的支援。如需相關資訊，請參閱[音訊格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。
-   包含多個音訊檔的保存檔（**.zip** 或 **.tar.gz** 檔案）。將多個音訊檔收集成為單一保存檔，然後載入該單一檔案，效率會比起個別新增音訊檔大幅提高。

請將音訊資源傳遞作為要求的內文，並指派 `audio_name` 給資源。如需相關資訊，請參閱[使用音訊資源](/docs/services/speech-to-text?topic=speech-to-text-audioResources)。

下列範例顯示音訊及保存檔類型資源的新增作業：

-   此範例會將音訊類型的資源新增至具有指定 `customization_id` 的自訂聲學模型。`Content-Type` 標頭會將音訊的類型識別為 `audio/wav`。音訊檔 **audio1.wav** 會作為要求的內文傳遞，而且會提供名稱 `audio1` 給資源。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/wav"
    --data-binary @audio1.wav
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
    ```
    {: pre}

-   此範例會將保存檔類型的資源新增至指定的自訂聲學模型。`Content-Type` 標頭會將保存檔的類型識別為 `application/zip`。`Contained-Contented-Type` 標頭指出保存檔中包含的所有檔案格式為 `audio/l16`，且以 16 kHz 的速率取樣。保存檔 **audio2.zip** 會作為要求的內文傳遞，而且會提供名稱 `audio2` 給資源。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/zip"
    --header "Contained-Content-Type: audio/l16;rate=16000"
    --data-binary @audio2.zip
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
    ```
    {: pre}

方法也接受選用性的 `allow_overwrite` 查詢參數，以改寫自訂模型的現有音訊資源。如果您在將音訊資源新增至模型之後需要更新音訊資源，請使用此參數。

此方法為非同步的方法。視音訊的持續時間而定，可能需要數秒的時間才能完成。針對保存檔，作業的長度取決於音訊檔的持續時間。如需檢查新增音訊資源之要求狀態的相關資訊，請參閱[監視新增音訊要求](#monitorAudio)。

您可以對每一個音訊或保存檔呼叫一次方法，將任意數目的音訊資源新增至自訂模型。您可以發出多個要求，以同時新增不同的音訊資源。

您必須新增最少 10 分鐘、最多 200 小時的音訊（其中包含語音而非靜音）至自訂模型，然後才能訓練。音訊或保存檔類型的資源都不得大於 100 MB。如需相關資訊，請參閱[新增音訊資源的準則](/docs/services/speech-to-text?topic=speech-to-text-audioResources#audioGuidelines)。

### 監視新增音訊要求
{: #monitorAudio}

如果音訊有效，服務會傳回 201 回應碼。然後，它會以非同步方式分析音訊檔的內容，並自動擷取音訊的相關資訊，例如長度、取樣率及編碼。在完成現行要求的所有音訊資源的服務分析之前，無法訓練自訂模型。

若要判定要求的狀態，請使用 `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法，來輪詢音訊的狀態。此方法會接受自訂模型的 GUID 及音訊資源的名稱。它的回應包含資源的 `status`，其值為下列其中一項：

-   `ok` 指出音訊是可接受的，且分析已完成。
-   `being_processed` 指出服務仍在分析音訊。
-   `invalid` 指出無法接受該音訊檔以進行處理。可能是格式錯誤、取樣率錯誤，或者它不是音訊檔。對於保存檔而言，如果其中包含的任何音訊檔無效，則整個保存檔都無效。

回應的內容及 `status` 欄位的位置，取決於資源的類型是音訊還是保存檔。

-   *若為音訊類型的資源，*`status` 欄位位於最上層 (`AudioListing`) 物件。

    ```bash
    curl -X GET -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
    ```
    {: pre}

    ```javascript
    {
      "duration": 131,
      "name": "audio1",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 22050
      }
      "status": "ok"
    }
    ```
    {: codeblock}

-   *若為保存檔類型的資源，*`status` 欄位位於第二層 (`AudioResource`) 物件，該物件巢狀位於 `container` 欄位中。

    ```bash
    curl -X GET -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
    ```
    {: pre}

    ```javascript
    {
      "container": {
        "duration": 556,
        "name": "audio2",
        "details": {
          "type": "archive",
          "compression": "zip"
        },
        "status": "ok"
      },
      . . .
    }
    ```
    {: codeblock}

請使用迴圈每隔幾秒檢查一次音訊資源的狀態，直到它變成 `ok` 為止。如需方法所傳回其他欄位的相關資訊，請參閱[列出自訂聲學模型的音訊資源](/docs/services/speech-to-text?topic=speech-to-text-manageAudio#listAudio)。

## 訓練自訂聲學模型
{: #trainModel-acoustic}

將音訊資源移入自訂聲學模型之後，您必須根據新資料來訓練模型。訓練會準備好自訂模型，以便用於語音辨識。在您根據新資料訓練模型之前，模型無法用於辨識要求。此外，模型不會反映以新音訊資源或變更過之音訊資源形式存在的自訂模型更新，直到您用這些變更來進行訓練為止。

請使用 `POST /v1/acoustic_customizations/{customization_id}/train` 方法來訓練自訂模型。請將想要訓練之模型的自訂作業 ID 傳遞給方法，如下列範例所示。

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train"
```
{: pre}

此方法為非同步的方法。根據自訂聲學模型所包含的音訊資料量，以及服務的現行負載，訓練可能需要花費大約數分鐘或幾小時才能完成。一般準則是，訓練自訂聲學模型需要音訊資料長度的大約二到四倍時間。時間範圍取決於正在訓練的模型及音訊的本質，例如音訊是否有雜訊。例如，可能需要 4 到 8 小時才能訓練包含 2 小時音訊的模型。如需檢查訓練作業狀態的相關資訊，請參閱[監視訓練模型要求](#monitorTraining-acoustic)。

此方法包含下列選用性的查詢參數：

-   `custom_language_model_id` 參數指定要在訓練期間使用的個別建立的自訂語言模型。您可以使用自訂語言模型來進行訓練，模型中包含音訊檔的轉錄，或是包含與音訊檔內容相關的語料庫或 OOV 字組。自訂聲學模型和自訂語言模型都必須根據相同版本的相同基礎模型，訓練才能成功。如需相關資訊，請參閱[搭配使用自訂語言模型來訓練自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-useBoth#useBothTrain)。

-   `strict` 參數指出在自訂模型包含有效和無效音訊資源的組合時，訓練是否繼續進行。依預設，如果模型包含一個以上的無效資源，訓練便會失敗。將參數設為 `false` 以容許只要模型包含至少一個有效的資源便繼續進行訓練。服務會將無效的資源排除在訓練之外。如需相關資訊，請參閱[訓練失敗](#failedTraining-acoustic)。

### 監視訓練模型要求
{: #monitorTraining-acoustic}

如果順利起始訓練程序，則服務會傳回 200 回應碼。在現有訓練要求完成之前，服務無法接受後續訓練要求，也無法接受新增更多音訊資源的要求。

若要判定訓練要求的狀態，請使用 `GET /v1/acoustic_customizations/{customization_id}` 方法，來輪詢模型的狀態。此方法會接受聲學模型的自訂作業 ID，並傳回其狀態，如下列範例所示：

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T22:11:13.298Z",
  "language": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom acoustic model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

回應包含 `status` 和 `progress` 欄位，它們會報告模型的現行狀態。`progress` 欄位的意義取決於模型的狀態。`status` 欄位可以具有下列其中一個值：

-   `pending` 指出已建立模型，但模型正在等待新增有效的訓練資料，或是正在等待服務完成已新增之資料的分析。`progress` 欄位是 `0`。
-   `ready` 指出模型包含有效的資料，且已準備好進行訓練。`progress` 欄位是 `0`。

    如果模型包含有效和無效音訊資源的組合，則除非您將 `strict` 查詢參數設定為 `false`，否則模型訓練會失敗。如需相關資訊，請參閱[訓練失敗](#failedTraining-acoustic)。
-   `training` 指出模型正在訓練中。當訓練完成時，`progress` 欄位會從 `0` 變更為 `100`。
-   `available` 指出模型已訓練好，可以使用。`progress` 欄位是 `100`。
-   `upgrading` 指出模型正在升級中。`progress` 欄位是 `0`。
-   `failed` 指出模型的訓練失敗。`progress` 欄位為 `0`。如需相關資訊，請參閱[訓練失敗](#failedTraining-acoustic)。

請使用迴圈每一分鐘檢查一次訓練的狀態，直到模型變成 `available` 為止。如需方法所傳回之其他欄位的相關資訊，請參閱[列出自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-manageAcousticModels#listModels-acoustic)。

### 訓練失敗
{: #failedTraining-acoustic}

如果服務正在處理自訂聲學模型的另一個要求，則訓練無法開始。衝突的要求可能是另一個訓練要求，或是將音訊資源新增至模型的要求。服務會傳回狀態碼 409。

訓練也會由於下列原因而無法開始：

-   自訂模型包含的音訊資料少於 10 分鐘。
-   自訂模型包含的音訊資料多於 200 小時。
-   自訂模型的一個以上音訊資源無效。
-   您使用 `custom_language_model_id` 查詢參數傳遞了不相容的自訂語言模型。兩個自訂模型都必須根據相同版本的相同基礎模型。

此服務會傳回狀態碼 400，並將自訂模型的狀態設定為 `failed`。採取下列其中一項動作：

-   使用 `GET /v1/acoustic_customizations/{customization_id}/audio` 和 `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法來檢查模型的音訊資源。如需相關資訊，請參閱[列出自訂聲學模型的音訊資源](/docs/services/speech-to-text?topic=speech-to-text-manageAudio#listAudio)。

    對於每個無效音訊資源，請執行下列其中一項：
    -   更正音訊資源，然後使用 `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法的 `allow_overwrite` 參數將更正後的音訊新增到模型。如需相關資訊，請參閱[將音訊新增至自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio)。
    -   使用 `DELETE /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法從模型刪除音訊資源。如需相關資訊，請參閱[從自訂聲學模型刪除音訊資源](/docs/services/speech-to-text?topic=speech-to-text-manageAudio#deleteAudio)。
-   將 `POST /v1/acoustic_customizations/{customization_id}/train` 方法的 `strict` 參數設定為 `false`，以從訓練排除無效的音訊資源。模型必須包含至少一個有效音訊資源，訓練才能成功。`strict` 參數對於訓練包含有效和無效音訊資源組合的自訂模型非常有用。
