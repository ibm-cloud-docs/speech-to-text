---

copyright:
  years: 2017, 2019
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

# 升級自訂模型
{: #customUpgrade}

為了改善語音辨識品質，{{site.data.keyword.speechtotextfull}} 服務有時候會更新基礎模型。因為不同語言的基礎模型彼此獨立，就像語言的寬頻及窄頻模型，所以對個別基礎模型的更新並不會影響其他模型。[版本注意事項](/docs/services/speech-to-text/release-notes.html)文件記載所有基礎模型更新。
{: shortdesc}

發行基礎模型的新版本時，您必須升級建置在基礎模型之上的任何自訂語言模型和自訂聲學模型，以充分運用更新項目。您的自訂模型會繼續使用基礎模型的舊版本，直到完成升級為止。就像所有自訂作業一樣，您必須使用擁有模型之服務實例的認證來升級它。

請盡快升級至更新基礎模型的最新版本。越快升級自訂模型，就能越快體驗新模型改善的效能。此外，將來也可以移除基礎模型的舊版本。為了鼓勵升級，該服務會傳回警告訊息，其中包含辨識要求的結果，這些要求所使用的自訂模型是以較舊的基礎模型為基礎。

## 升級的運作方式
{: #upgradeOverview}

第一次發行新的基礎模型時，現有的自訂模型仍會以基礎模型的舊版本為基礎。在升級自訂模型之前，該自訂模型上的所有作業（例如，將資料新增至模型或訓練模型）都會影響該模型的現有版本。同樣地，指定自訂模型的所有辨識要求會使用模型的現有版本。

升級會導致自訂模型有兩個版本，一個是基於基礎模型的舊版本，一個則是基於基礎模型的最新版本。升級自訂模型之後，該自訂模型上的所有作業都會影響該模型較新的已升級版本。無法再將資料新增至模型，或訓練模型的舊版本。此外，您無法復原升級作業。

升級任一類型的自訂模型時，您不需要升級其個別資源。對於自訂語言模型，該服務會自動升級對該模型所定義的所有語料庫、文法及字組。同樣地，當您升級自訂的聲學模型時，該服務會自動升級其音訊資源。

依預設，該服務會使用最新版的自訂模型來進行辨識要求。不過，您可以繼續使用自訂模型的舊版本來進行語音辨識。如需相關資訊，請參閱[使用升級的自訂模型提出辨識要求](#upgradeRecognition)。

## 升級自訂語言模型
{: #upgradeLanguage}

請遵循下列步驟來升級自訂語言模型：

1.  請確定自訂語言模型處於 `ready` 或 `available` 狀態。您可以使用 `GET /v1/customizations/{customization_id}` 方法來檢查模型的狀態。如果模型的狀態為 `ready`，請在其最新資料進行訓練之前先將該模型升級。

1.  使用 `POST /v1/customizations/{customization_id}/upgrade_model` 方法來升級自訂語言模型：

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    升級方法為非同步。這可能需要幾分鐘才能完成，取決於該模型的字組資源中的字組數量和該服務的現行負載。

如果順利起始升級程序，則服務會傳回 200 回應碼。您可以使用 `GET /v1/customizations/{customization_id}` 方法來輪詢模型的狀態，以監視升級的狀態。使用迴圈每隔 10 秒檢查一次狀態。

在升級時，自訂模型的狀態為 `upgrading`。當升級完成時，模型會回復成它升級之前的狀態（`ready` 或 `available`）。檢查升級作業的狀態等於檢查訓練作業的狀態。如需相關資訊，請參閱[監視訓練模型要求](/docs/services/speech-to-text/language-create.html#monitorTraining-language)。

在升級要求完成之前，該服務無法接受以任何方式修改模型的要求。不過，在升級期間，您可以繼續以模型的現有版本來發出辨識要求。

## 升級自訂聲學模型
{: #upgradeAcoustic}

請遵循下列步驟來升級自訂聲學模型。如果自訂聲學模型是使用自訂語言模型訓練，如有指示，您必須執行兩個額外的升級步驟。

1.  *如果自訂聲學模型是使用自訂語言模型訓練*，您必須先將自訂語言模型升級至基礎模型的最新版本。如需相關資訊，請參閱[升級自訂語言模型](#upgradeLanguage)。

1.  請確定自訂聲學模型處於 `ready` 或 `available` 狀態。您可以使用 `GET /v1/acoustic_customizations/{customization_id}` 方法來檢查模型的狀態。如果模型的狀態為 `ready`，請在其最新資料進行訓練之前先將該模型升級。

1.  使用 `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` 方法來升級自訂聲學模型：

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    升級方法為非同步。這可能需要幾分鐘或幾小時才能完成，取決於該模型包含的音效資料量和該服務的現行負載。如同訓練，升級通常需要的時間大約是模型音訊資料長度的兩倍。

1.  *如果自訂聲學模型是使用自訂語言模型訓練*，請再次升級自訂聲學模型，這次要使用先前升級的自訂語言模型。請使用 `custom_language_model_id` 查詢參數來指定自訂語言模型的自訂作業 ID。

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model?custom_language_model_id={custom_language_model_id}"
    ```
    {: pre}

    同樣地，升級方法為非同步，且升級通常需要的時間大約是模型音訊資料長度的兩倍。

    使用語言模型升級聲學模型的要求可能失敗，並顯示 400 回應碼及此訊息：`自前次訓練之後未修改任何輸入資料`。如果發生這個錯誤，請將布林 `force` 查詢參數新增至要求，並將該參數設為 `true`。在此特定狀況下，僅使用此參數來強制升級自訂聲學模型。{: note}

如果順利起始升級程序，則服務會傳回 200 回應碼。您可以使用 `GET /v1/acoustic_customizations/{customization_id}` 方法來輪詢模型的狀態，以監視升級的狀態。使用迴圈每分鐘檢查一次狀態。

在升級時，自訂模型的狀態為 `upgrading`。當升級完成時，模型會回復成它升級之前的狀態（`ready` 或 `available`）。檢查升級作業的狀態等於檢查訓練作業的狀態。如需相關資訊，請參閱[監視訓練模型要求](/docs/services/speech-to-text/acoustic-create.html#monitorTraining-acoustic)。

在升級要求完成之前，該服務無法接受以任何方式修改模型的要求。不過，在升級期間，您可以繼續以模型的現有版本來發出辨識要求。

## 升級失敗
{: #upgradeFailures}

如果服務正在處理模型的另一個要求（例如訓練要求或新增資料的要求），則自訂模型升級無法開始。在下列情況下，升級要求也無法開始：

-   自訂模型處於 `ready` 或 `available` 以外的狀態。
-   自訂模型未包含任何資料（自訂字組或音訊資源）。
-   對於使用自訂語言模型訓練的自訂聲學模型，自訂模型是以基礎模型的不同版本為基礎。您必須先升級自訂語言模型，然後才能升級自訂聲學模型。

## 列出自訂模型的版本資訊
{: #upgradeList}

若要查看可用自訂模型的基礎模型版本，請使用下列方法：

-   若要列出自訂語言模型的相關資訊，請使用 `GET /v1/customizations/{customization_id}` 方法。如需相關資訊，請參閱[列出自訂語言模型](/docs/services/speech-to-text/language-models.html#listModels-language)。
-   若要列出自訂聲學模型的相關資訊，請使用 `GET /v1/acoustic_customizations/{customization_id}` 方法。如需相關資訊，請參閱[列出自訂聲學模型](/docs/services/speech-to-text/acoustic-models.html#listModels-acoustic)。

在這兩種情況下，輸出都包含 `versions` 欄位，其顯示自訂模型之基礎模型的相關資訊。下列輸出顯示已升級的自訂語言模型的資訊：

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "versions": [
    "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

`versions` 欄位指出自訂模型可用於基礎模型的兩個版本：較舊版本 `en-US_BroadbandModel.v07-06082016.06202016` 及較新版本 `n-US_BroadbandModel.v2017-11-15`。如果自訂模型未升級或只有其基礎模型的單一版本存在，則 `versions` 欄位會顯示單一版本。

## 使用升級的自訂模型提出辨識要求
{: #upgradeRecognition}

依預設，該服務會使用所指定的最新版自訂模型來提出辨識要求。不過，即使在升級自訂模型之後，您仍然可以繼續使用該模型的較舊版本來提出辨識要求。您可以使用辨識方法的 `base_model_version` 參數，來指定要用於語音辨識的基礎模型版本。

例如，下列 HTTP 要求指定要使用基礎模型的舊版本。因此，也會使用所指定的自訂語言模型的舊版本。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_BroadbandModel&base_model_version=en-US_BroadbandModel.v07-06082016.06202016&language_customization_id={customization_id}"
```
{: pre}

您可以使用這個特性，同時對其基礎模型的舊版本和新版本來測試自訂模型的效能和正確性。如果您發現升級後的模型缺乏某些方面的效能（例如，無法再辨識某些字組），則您可以繼續使用舊版本來提出辨識要求。

[基礎模型版本](/docs/services/speech-to-text/input.html#version)說明 `base_model_version` 參數，以及該服務如何決定要使用基礎模型和自訂模型的哪些版本來提出辨識要求。除了這項資訊之外，當您在辨識要求中傳遞自訂語言模型和自訂聲學模型時，請考量下列問題：

-   這兩個自訂模型都必須以相同的基礎模型為基礎（例如，`en-US_BroadbandModel`）。
-   如果這兩個自訂模型都是以較舊的基礎模型為基礎，則該服務會使用舊的基礎模型進行辨識。
-   如果這兩個自訂模型都是以較新的基礎模型為基礎，則該服務會使用新的基礎模型進行辨識。
-   如果這兩個自訂模型只有其中一個升級至較新的基礎模型，則該服務會使用舊的基礎模型進行辨識。它會選取舊的基礎模型，因為它是這兩個自訂模型的共同版本。
