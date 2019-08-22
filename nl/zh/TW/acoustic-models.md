---

copyright:
  years: 2019
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

# 管理自訂聲學模型
{: #manageAcousticModels}

自訂作業介面包含 `POST /v1/acoustic_customizations` 方法，這用來建立自訂聲學模型。介面還包含 `POST /v1/acoustic_customizations/train` 方法，這用來根據最新音訊資源訓練自訂模型。如需相關資訊，請參閱：
{: shortdesc}

-   [建立自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-acoustic#createModel-acoustic)
-   [訓練自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic)

此外，此介面還包含方法可列出自訂聲學模型的相關資訊、將自訂模型重設為起始狀態、升級自訂模型，以及刪除自訂模型。服務正在處理對自訂模型的其他作業（包括向模型新增音訊資源）時，無法訓練、重設、升級或刪除該模型。

## 列出自訂聲學模型
{: #listModels-acoustic}

自訂作業介面提供兩種方法來列出指定認證所擁有之自訂聲學模型的相關資訊：

-   `GET /v1/acoustic_customizations` 方法會列出所有自訂聲學模型的相關資訊，或指定語言之所有自訂聲學模型的相關資訊。
-   `GET /v1/acoustic_customizations/{customization_id}` 方法會列出指定自訂聲學模型的相關資訊。請使用此方法，向服務輪詢關於訓練要求的狀態。

兩種方法都會傳回自訂聲學模型的下列相關資訊：

-   `customization_id` 識別自訂模型的「廣域唯一 ID (GUID)」。GUID 用來在介面的方法中識別模型。
-   `created` 是建立自訂模型的日期和時間，以世界標準時間 (UTC) 表示。
-   `updated` 是前次修改自訂模型的日期和時間，以世界標準時間 (UTC) 表示。
-   `language` 是自訂模型的語言。
-   `owner` 識別擁有該自訂模型之服務實例的認證。
-   `name` 是自訂模型的名稱。
-   `description` 顯示自訂模型的說明（如果已在建立時提供的話）。
-   `base_model_name` 指出已為其建立自訂模型的語言模型名稱。
-   `versions` 提供自訂模型的可用版本清單。陣列的每一個元素都指出可搭配使用自訂模型的基礎模型版本。只有在已升級自訂模型時，才會有多個版本存在。否則，只會顯示單一版本。如需相關資訊，請參閱[列出自訂模型的版本資訊](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeList)。

這些方法也會傳回 `status` 欄位，指出自訂模型的狀態：

-   `pending` 指出已建立模型。它正在等待新增有效的訓練資料（音訊資源），或是正在等待服務完成已新增之資料的分析。
-   `ready` 指出模型包含有效的音訊資料，且已準備好進行訓練。如果模型包含有效和無效音訊資源的組合，則除非您將 `strict` 查詢參數設定為 `false`，否則模型訓練會失敗。如需相關資訊，請參閱[訓練失敗](/docs/services/speech-to-text?topic=speech-to-text-acoustic#failedTraining-acoustic)。
-   `training` 指出模型正在根據音訊資料進行訓練。
-   `available` 指出模型已訓練好，可以用於辨識要求。
-   `upgrading` 指出模型正在升級中。
-   `failed` 指出模型的訓練失敗。請檢查模型的音訊資源，以判定導致模型無法進行訓練的原因。可能的錯誤包括音訊不夠、太多音訊，或音訊資源無效。

此外，輸出包含 `progress` 欄位，它指出自訂模型訓練的現行進度。如果您使用 `POST /v1/acoustic_customizations/{customization_id}/train` 方法來開始訓練模型，此欄位會以完成百分比指出該要求的現行進度。此時，如果狀態是 `available`，則欄位的值是 `100`，否則為 `0`。

### 要求與回應範例
{: #listExample-acoustic}

下列範例包含 `language` 查詢參數，用於列出指定認證擁有的所有美式英文自訂聲學模型：

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations?language=en-US"
```
{: pre}

認證擁有兩個這類模型。第一個模型正在等待資料，或是正在由服務處理中。第二個模型已完全訓練過，可以使用。

```javascript
{
  "customizations": [
    {
      "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
      "created": "2016-06-01T18:42:25.324Z",
      "updated": "2016-06-01T18:42:25.324Z",
      "language": "en-US",
      "versions": [
        "en-US_BroadbandModel.v07-06082016.06202016",
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model one",
      "description": "Example custom acoustic model",
      "base_model_name": "en-US_BroadbandModel",
      "status": "pending",
      "progress": 0
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732fae4829",
      "created": "2016-06-01T18:51:37.291Z",
      "updated": "2016-06-01T19:21:06.825Z",
      "language": "en-US",
      "versions": [
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model two",
      "description": "Example custom acoustic model two",
      "base_model_name": "en-US_BroadbandModel",
      "status": "available",
      "progress": 100
    }
  ]
}
```
{: codeblock}

下列範例會傳回具有指定自訂作業 ID 之自訂模型的相關資訊：

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "versions": [
    "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model one",
  "description": "Example custom acoustic model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

## 重設自訂聲學模型
{: #resetModel-acoustic}

請使用 `POST /v1/acoustic_customizations/{customization_id}/reset` 方法來重設自訂聲學模型。重設自訂模型會從模型移除所有音訊資源，並將模型起始設定為建立時的狀態。此方法不會刪除模型本身或 meta 資料，例如其名稱和語言。不過，當您重設模型時，會移除其音訊資源，因此必須重建音訊資源。

### 要求範例
{: #resetExample-acoustic}

下列範例會重設具有指定自訂作業 ID 的自訂聲學模型：

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/reset"
```
{: pre}

## 刪除自訂聲學模型
{: #deleteModel-acoustic}

請使用 `DELETE /v1/acoustic_customizations/{customization_id}` 方法，來刪除不再需要的自訂聲學模型。此方法會刪除與自訂模型相關聯的所有音訊，以及模型本身。使用此方法時請小心：在您刪除模型之後，無法收回自訂模型及其資料。

### 要求範例
{: #deleteExample-acoustic}

下列範例會刪除具有指定自訂作業 ID 的自訂聲學模型：

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}
