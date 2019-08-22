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

# 一起使用自訂聲學模型和自訂語言模型
{: #useBoth}

您可以藉由使用互補性的自訂語言模型及自訂聲學模型，來改善語音辨識的正確性。您可以在訓練聲學模型的期間，以及語音辨識的期間，使用這兩種類型的模型。兩種模型都必須由相同的服務實例所擁有，且兩者都必須根據相同的基礎語言模型。
{: shortdesc}

單獨使用自訂聲學模型，或是與未根據來進行訓練的自訂語言模型搭配使用，仍然可以有其實用之處。如果自訂聲學模型已根據聲音特徵進行訓練，且這些聲音特徵符合正在轉錄的音訊，則仍可以改善轉錄品質。

## 搭配使用自訂語言模型來訓練自訂聲學模型
{: #useBothTrain}

單獨使用音訊資料來訓練自訂聲學模型，稱為*非監督式訓練 (unsupervised training)*。在訓練期間使用自訂語言模型，稱為*輕度監督式訓練 (lightly supervised training)*。輕度監督式訓練可以改善自訂聲學模型的有效性。

在下列情況中，請使用輕度監督式訓練，搭配使用自訂語言模型來訓練自訂聲學模型：

-   自訂語言模型是根據來自於您新增至自訂聲學模型之音訊檔的轉錄（逐項文字）。

    由於轉錄包含確切的音訊內容，因此使用以轉錄為基礎的自訂語言模型來進行訓練，可以產生最佳的結果。服務可以根據上下文剖析轉錄的內容，並擷取 OOV 字組和 N 元語法，這有助於進行音訊的最有效利用。如果您的音訊資料長度少於一小時，尤其是如此。

    轉錄音訊資料並非嚴格必要。但如果您有音訊的轉錄，它們將可以改善自訂聲學模型的品質。如果音訊包含許多 OOV 字組，轉錄尤其具有價值。
-   自訂語言模型是根據與音訊檔內容相關的語料庫（文字檔）或字組清單。

    如果您的音訊包含特定領域的字組，且在服務的基礎詞彙裡找不到這些字組，則光是聲學模型自訂作業並不能在轉錄期間產生那些字組。語言模型自訂作業是擴展服務基礎詞彙的唯一方式。如果您沒有轉錄，請使用自訂語言模型來進行訓練，而該自訂語言模型中要包含來自與您音訊資料相同領域的 OOV 字組。即使是使用包含音訊所用 OOV 字組清單的自訂語言模型來進行訓練，都可以保證有所助益。

    例如，假設您正在建立根據特定產品之客服中心音訊的自訂聲學模型。您可以使用根據相關通話轉錄的自訂語言模型，或是包含客服中心所處理之特定產品名稱的自訂語言模型，來訓練自訂聲學模型。

若要使用轉錄或字組清單，請先建立包含此文字資料的自訂語言模型。若要搭配使用自訂語言模型來訓練自訂聲學模型，兩個自訂模型都必須根據相同版本的相同基礎模型。如果有新版本的基礎模型可用，您必須將兩個模型都升級為相同版本的基礎模型，訓練才能成功。

請使用 `POST /v1/acoustic_customizations/{customization_id}/train` 方法的選用性 `custom_language_model_id` 查詢參數，以便使用自訂語言模型來訓練您的自訂聲學模型。請使用 `customization_id` 參數傳遞聲學模型的 GUID，並使用 `custom_language_model_id` 參數傳遞自訂語言模型的 GUID。這兩種模型必須由隨要求一起傳遞的認證所擁有。

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train?custom_language_model_id={customization_id}"
```
{: pre}

## 在語音辨識期間使用自訂語言模型及自訂聲學模型
{: #useBothRecognize}

您可以搭配任何辨識要求，指定自訂語言模型和自訂聲學模型。如果自訂語言模型包含來自辨識中音訊領域的 OOV 字組，您可以在語音辨識期間，將它與自訂聲學模型搭配使用，以改善轉錄的正確性。

使用自訂語言模型可以改善轉錄正確性，而不論您是否搭配使用自訂語言模型來訓練自訂聲學模型：

-   在訓練期間同時使用自訂語言模型及自訂聲學模型，能夠改善自訂聲學模型的品質。
-   在語音辨識期間同時使用兩種類型的模型，能夠改善轉錄的品質。

如果自訂語言模型包含文法，您也可以在語音辨識期間，使用自訂語言模型和其中一個文法來搭配自訂聲學模型。

下列範例將兩種類型的模型都傳遞給 HTTP `POST /v1/recognize` 方法。請使用 `acoustic_customization_id` 參數傳遞自訂聲學模型的 GUID，並使用 `language_customization_id` 參數傳遞自訂語言模型的 GUID。兩個模型都必須由和要求一起傳遞的認證所擁有，且兩者都必須根據相同的基礎模型（例如 `en-US_BroadbandModel`）。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}&language_customization_id={customization_id}"
```
{: pre}

針對非同步 HTTP 要求，請在建立非同步工作時指定參數。針對 WebSocket，請在建立連線時傳遞參數。
