---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-21"

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

# 自訂作業介面
{: #customization}

{{site.data.keyword.speechtotextfull}} 服務提供自訂作業介面，您可以用來擴增其語音辨識功能。您可以使用自訂作業，藉由自訂網域和音訊的基礎模型，來改善語音辨識要求的正確性。自訂作業僅適用於某些語言，而且對不同語言有不同層次的支援；請參閱[自訂作業的語言支援](#languageSupport)。
{: shortdesc}

自訂作業介面同時支援自訂語言模型和自訂聲學模型。這兩種類型的自訂模型，其介面類似且用法直接明確。在辨識要求中使用任一類型的自訂模型也很直接明確：您在要求中指定模型的自訂作業 ID。

無論是否有自訂模型，語音辨識的運作方式都一樣。當您將自訂模型用於語音辨識時，您可以使用辨識要求通常可用的所有輸入和輸出參數。如需所有可用參數的相關資訊，請參閱[參數摘要](/docs/services/speech-to-text?topic=speech-to-text-summary)。

您必須有標準定價方案才能使用語言模型或聲學模型自訂作業。精簡方案的使用者無法使用自訂作業介面。如需相關資訊，請參閱 {{site.data.keyword.speechtotextshort}} 服務的[定價頁面](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external}。
{: note}

## 語言模型自訂作業
{: #customLanguage-intro}

該服務在開發時是以廣泛的一般對象為考量。該服務的基礎詞彙包含在日常交談中使用的許多字組。其模型為許多應用程式提供足夠精確的辨識。然而，它們可能缺乏與特定領域相關聯之特定術語的知識。

*語言模型自訂作業* 介面可以改善諸如醫藥、法律、資訊科技及其他領域的語音辨識的正確性。使用語言模型自訂作業，您可以擴充及修改基礎模型的詞彙，以包含領域專用術語。

您可以建立自訂語言模型，並新增領域專用的語料庫和字組。在您根據加強的詞彙訓練自訂語言模型之後，即可將它用於自訂的語音辨識。該服務通常可以在幾分鐘內訓練任何自訂模型。建立模型所採用的工作層次，視您可用於模型的資料而定。

如需相關資訊，請參閱：

-   [建立自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-languageCreate)
-   [使用自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-languageUse)

## 聲學模型自訂作業
{: #customAcoustic-intro}

同樣地，該服務是使用適用於各種音訊特徵的基礎聲學模型所開發。然而，在類似下列情況下，調整基礎模型以符合您的音訊，可改善語音辨識：

-   您的聲音頻道環境是唯一的。例如，環境有噪音，麥克風品質或位置欠佳，或音訊遭受到遠場效應的影響。
-   說話者的語音模式非典型。例如，說話者講話速度異常快，或音訊包含閒聊。
-   說話者有自己的口音。例如，音訊包含了以非母語或第二語言講話的說話者。

*聲學模型自訂作業* 介面可以因應您的環境和說話者來調整基礎模型。您可以建立自訂聲學模型，並新增與您要轉錄之音訊的聲音特徵幾乎相符的音訊資料（音訊資源）。在您以音效資源訓練自訂聲學模型之後，即可將它用於自訂的語音辨識。

此服務訓練自訂模型所花費的時間長度，取決於該模型所包含的音訊資料量。一般而言，訓練長度是累加音訊長度的兩倍。建立模型所採用的工作層次，視您可用於模型的音訊資料而定。它也取決於您是否使用音訊轉錄。

如需相關資訊，請參閱：

-   [建立自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-acoustic)
-   [使用自訂聲學模型](/docs/services/speech-to-text?topic=speech-to-text-acousticUse)

## 文法
{: #grammars-intro}

自訂語言模型容許您擴充該服務的基礎詞彙。*文法* 可讓您限制服務可從該詞彙辨識的字組。當您使用文法與自訂語言模型搭配來進行語音辨識時，該服務只能辨識文法所能辨識的字組、詞組和字串。由於文法為有效的相符項定義有限的搜尋空間，因此該服務可以更快速且更精確地傳遞結果。

您可以將文法新增至自訂語言模型，並像您訓練語料庫那樣來訓練此模型。不過，與語料庫不同，您必須明確指定在語音辨識期間要將文法與自訂模型搭配使用。

如需相關資訊，請參閱：

-   [將文法與自訂語言模型搭配使用](/docs/services/speech-to-text?topic=speech-to-text-grammars)
-   [將文法新增至自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd)
-   [使用文法進行語音辨識](/docs/services/speech-to-text?topic=speech-to-text-grammarUse)

## 同時使用聲學和語言自訂作業
{: #combined}

單獨使用自訂聲學模型可以改善服務的辨識功能。然而，如果您的範例音訊可使用轉錄或相關語料庫，您可以使用該資料來進一步改善基於自訂聲學模型的語音辨識品質。

藉由建立自訂語言模型來補充自訂聲學模型，您可以同時使用這兩個模型來加強語音辨識。當您訓練自訂聲學模型時，可以指定自訂語言模型，其中包括音訊資源的轉錄或資源中領域專用字組的詞彙。同樣地，當您轉錄音訊時，該服務會接受自訂語言模型、自訂聲學模型或兩者。如果您的自訂語言模型包含文法，則可以將該模型和文法與自訂聲學模型搭配使用，以進行語音辨識。

如需相關資訊，請參閱[同時使用自訂聲學模型和自訂語言模型](/docs/services/speech-to-text?topic=speech-to-text-useBoth)。

部分語言不會同時支援語言自訂和聲學自訂。如需相關資訊，請參閱[自訂作業的語言支援](#languageSupport)。
{: note}

## 自訂作業的語言支援
{: #languageSupport}

語言和聲學模型自訂作業僅適用於部分語言。下表記載該服務支援每種語言之自訂作業的層次。

-   *GA* 指出介面已正式發行，可供正式作業使用。
-   *測試版* 指出介面可作為測試版供應項目使用。
-   *不支援* 表示介面不適用於該語言。

您可以將寬頻和窄頻模型與任何受支援的可用語言搭配使用。如果語言支援語言模型自訂作業，則它也支援文法。如需所有可用模型的清單，請參閱[支援的語言模型](/docs/services/speech-to-text?topic=speech-to-text-models#modelsList)。

<table>
  <caption>表 1. 自訂作業的語言支援</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 24%">
      語言
    </th>
    <th style="text-align:center; vertical-align:bottom; width 37%">
      支援語言模型自訂作業
    </th>
    <th style="text-align:center; vertical-align:bottom; width 37%">
      支援聲學模型自訂作業
    </th>
  </tr>
  <tr>
    <td>阿拉伯文（現代標準）</td>
    <td style="text-align:center">不支援</td>
    <td style="text-align:center">測試版</td>
  </tr>
  <tr>
    <td>巴西葡萄牙文</td>
    <td style="text-align:center">GA 版</td>
    <td style="text-align:center">測試版</td>
  </tr>
  <tr>
    <td>中文（普通話）</td>
    <td style="text-align:center">不支援</td>
    <td style="text-align:center">測試版</td>
  </tr>
  <tr>
    <td>英文（英國）</td>
    <td style="text-align:center">GA 版</td>
    <td style="text-align:center">測試版</td>
  </tr>
  <tr>
    <td>英文（美國）</td>
    <td style="text-align:center">GA 版</td>
    <td style="text-align:center">測試版</td>
  </tr>
  <tr>
    <td>法文</td>
    <td style="text-align:center">GA 版</td>
    <td style="text-align:center">測試版</td>
  </tr>
  <tr>
    <td>德文</td>
    <td style="text-align:center">GA 版</td>
    <td style="text-align:center">測試版</td>
  </tr>
  <tr>
    <td>日文</td>
    <td style="text-align:center">GA 版</td>
    <td style="text-align:center">測試版</td>
  </tr>
  <tr>
    <td>韓文</td>
    <td style="text-align:center">GA 版</td>
    <td style="text-align:center">測試版</td>
  </tr>
  <tr>
    <td>西班牙文（阿根廷）</td>
    <td style="text-align:center">測試版</td>
    <td style="text-align:center">測試版</td>
  </tr>
  <tr>
    <td>西班牙文（卡斯提亞）</td>
    <td style="text-align:center">GA 版</td>
    <td style="text-align:center">測試版</td>
  </tr>
  <tr>
    <td>西班牙文（智利）</td>
    <td style="text-align:center">測試版</td>
    <td style="text-align:center">測試版</td>
  </tr>
  <tr>
    <td>西班牙文（哥倫比亞）</td>
    <td style="text-align:center">測試版</td>
    <td style="text-align:center">測試版</td>
  </tr>
  <tr>
    <td>西班牙文（墨西哥）</td>
    <td style="text-align:center">測試版</td>
    <td style="text-align:center">測試版</td>
  </tr>
  <tr>
    <td>西班牙文（秘魯）</td>
    <td style="text-align:center">測試版</td>
    <td style="text-align:center">測試版</td>
  </tr>
</table>

您可以使用 `GET /v1/models` 和 `GET /v1/models/{model_id}` 方法，來檢查某語言模型是否支援語言模型自訂作業。如果基礎模型支援語言模型自訂作業，則此方法的模型輸出中的 `supported_features` 欄位會將 `custom_language_model` 欄位設為 `true`。

## 自訂作業的使用注意事項
{: #customUsage}

下列使用注意事項同時適用於語言模型自訂作業及聲學模型自訂作業。

### 自訂模型的所有權
{: #customOwner}

自訂模型是由將其認證用來建立此模型的 {{site.data.keyword.speechtotextshort}} 服務實例所擁有。若要以任何方式使用自訂模型，您必須搭配使用該服務實例的認證與自訂作業介面的方法。針對其他服務實例所建立的認證無法檢視或存取自訂模型。

針對相同 {{site.data.keyword.speechtotextshort}} 服務實例取得的所有認證，會共用對為該服務實例建立之所有自訂模型的存取。若要限制對自訂模型的存取，請建立個別服務實例，並且只使用該服務實例的認證來建立及使用模型。其他服務實例的認證無法影響自訂模型。

透過服務實例的認證來共用所有權的優點是，您可以取消一組認證（例如，如果它們遭洩漏的話）。然後，您可以為相同的服務實例建立新的認證，並仍保留對使用原始認證建立之自訂模型的所有權和存取權。

### 要求記載和資料隱私權
{: #customLogging}

服務如何處理自訂作業介面呼叫的要求記載，取決於要求：

-   服務*不會* 記載用來建置自訂模型的資料。例如，當使用自訂語言模型中的語料庫和字組時，您不需要設定 `X-Watson-Learning-Opt-Out` 要求標頭。您的訓練資料絕不會用來改善服務的基礎模型。
-   服務*會* 在搭配使用自訂模型與辨識要求時記載資料。您必須將 `X-Watson-Learning-Opt-Out` 要求標頭設為 `true`，以防止記載辨識要求。

如需相關資訊，請參閱[要求記載](/docs/services/speech-to-text?topic=speech-to-text-input#logging)。

### 資訊安全
{: #customSecurity}

您可以建立客戶 ID 與針對自訂語言模型及自訂聲學模型而新增或更新之資料的關聯。您可以使用下列方法傳遞 `X-Watson-Metadata` 標頭，以建立客戶 ID 與語料庫、自訂字組、文法及音訊資源的關聯：

-   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
-   `POST /v1/customizations/{customization_id}/words`
-   `PUT /v1/customizations/{customization_id}/words/{word_name}`
-   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`
-   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

必要的話，您可以使用 `DELETE /v1/user_data` 方法來刪除資料。如需相關資訊，請參閱[資訊安全](/docs/services/speech-to-text?topic=speech-to-text-information-security)。
