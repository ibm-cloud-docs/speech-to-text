---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-12"

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

# 使用語料庫和自訂字組
{: #corporaWords}

您可以將語料庫或文法新增至模型，或直接新增自訂字組，以將字組移入自訂語言模型：
{: shortdesc}

-   **語料庫：**將字組移入自訂模型的建議方法，是將一個以上語料庫新增至模型。當您新增語料庫時，服務會分析檔案，並將其找到的任何新字組新增至自訂模型。新增語料庫至自訂模型容許服務擷取環境定義中的領域特定字組，這有助於確保獲得較佳的轉錄結果。如需相關資訊，請參閱[使用語料庫](#workingCorpora)。
-   **文法：**您可以將文法新增至自訂模型，以將語音辨識限制在文法所辨識的字組或詞組。當您將文法新增至模型時，服務會自動將其找到的任何新字組新增至模型，就像對語料庫的作法一樣。如需相關資訊，請參閱[將文法與自訂語言模型搭配使用](/docs/services/speech-to-text/grammar.html)。
-   **個別字組：**您也可以直接將個別自訂字組新增至模型。服務將字組新增至模型的方式，就像新增從語料庫或文法發現的字組一樣。當您直接新增字組時，可以指定多個發音，並指出要如何顯示該字組。您也可以更新現有字組，以修改或擴增從語料庫或文法擷取的定義。如需相關資訊，請參閱[使用自訂字組](#workingWords)。

不論您如何新增字組，服務都會將您新增至自訂語言模型的所有字組儲存在模型的字組資源中。

## 字組資源
{: #wordsResource}

*字組資源* 包括您從語料庫、文法或直接新增的所有字組。其目的是要定義尚未出現在服務基礎詞彙中之字組的發音和拼法。定義會指示服務如何轉錄這些*未登錄詞彙 (OOV) 字組*。

字組資源包含每個 OOV 字組的下列資訊。服務會為擷取自語料庫和文法的字組建立定義。您可以為直接新增或修改的字組指定特徵。

-   `word`：字組的拼法，可以在語料庫或文法中找到，或是由您新增。
-   `sounds_like`：字組的發音。對於從語料庫和文法擷取的字組，此值代表服務根據其語言規則所認定的字組發音方式。在許多情況下，發音會反映 `word` 欄位的拼法。

    您可以使用 `sounds_like` 欄位來修改字組的發音。您也可以使用此欄位來為字組指定多個發音。如需相關資訊，請參閱[使用 sounds_like 欄位](#soundsLike)。
-   `display_as`：服務在轉錄文字中使用的字組拼法。此欄位指出字組的顯示方式。在大部分情況下，此拼法與 `word` 欄位的值相符。

    您可以使用 `display_as` 欄位來為字組指定不同的拼法。如需相關資訊，請參閱[使用 display_as 欄位](#displayAs)。
-   `source`：字組新增至字組資源的方式。如果服務是從語料庫或文法擷取字組，則此欄位會列出該資源的名稱。因為服務可能會在多個資源中遇到相同的字組，所以此欄位可能會列出多個語料庫或文法名稱。如果您是直接新增或修改字組，則此欄位會包含字串 `user`。

當您以任何方式更新模型的字組資源時，都必須訓練模型，以讓變更在轉錄期間生效。如需相關資訊，請參閱[訓練自訂語言模型](/docs/services/speech-to-text/language-create.html#trainModel-language)。

## 我需要多少資料？
{: #wordsResourceAmount}

許多因素都會影響有效的自訂語言模型所需的資料量。無法提供您需要為任何自訂模型或應用程式新增的確切字組數目。

視使用案例而定，即使直接在自訂模型中新增幾個字組，也可以提升模型的品質。然而，如果是從一個在音訊中使用之環境定義中顯示字組的語料庫中新增 OOV 字組，則可以大幅提升轉錄的正確性。如需相關資訊，請參閱[使用語料庫](#workingCorpora)。

服務會限制您可以新增至自訂語言模型的字組數目：

-   您最多可以新增 9 萬個 OOV 字組至自訂模型的字組資源，其中包括來自所有來源的 OOV 字組（語料庫、文法，以及您直接新增的個別自訂字組）。
-   您可以從所有來源新增最多 1000 萬個字組至自訂模型。這個數量包括內含在語料庫或文法中的所有字組（OOV 字組和已屬於服務基礎詞彙的字組）。就語料庫而言，服務會使用這些額外的字組來學習 OOV 字組可以出現的環境定義，這就是為什麼語料庫是提升辨識正確性更有效的方法。

大型字組資源會增加語音辨識的延遲，但實際的效果很難量化或預測。如同產生有效自訂模型所需的資料量，大型字組資源的效能影響取決於許多因素。您可以使用不同的資料量來測試自訂模型，以判斷模型和資料的效能。

## 使用語料庫
{: #workingCorpora}

您可以使用 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法，將語料庫新增至自訂模型。語料庫是純文字檔案，包含您領域中的句子範例。下列範例顯示醫療保健領域的簡短語料庫。語料庫檔案通常會長很多。

```
Am I at risk for health problems during travel?
Some people are more likely to have health problems when traveling outside the United States.
How Is Coronary Microvascular Disease Treated?
If you're diagnosed with coronary MVD and also have anemia, you may benefit from treatment for that condition.
Anemia is thought to slow the growth of cells needed to repair damaged blood vessels.
What causes autoimmune hepatitis?
A combination of autoimmunity, environmental triggers, and a genetic predisposition can lead to autoimmune hepatitis.
What research is being done for Spinal Cord Injury?
The National Institute of Neurological Disorders and Stroke NINDS conducts spinal cord research in its laboratories at the National Institutes of Health NIH.
NINDS also supports additional research through grants to major research institutions across the country.
Some of the more promising rehabilitation techniques are helping spinal cord injury patients become more mobile.
What is Osteogenesis imperfecta OI?
. . .
```
{: codeblock}

語音辨識根據統計演算法來分析音訊。來自自訂模型的字組，會與來自服務基礎詞彙的字組以及模型的其他字組競爭。（音訊雜訊和說話者口音這類因素也會影響轉錄品質。）

轉錄的正確性大部分取決於字組在模型中的定義方式，以及說話者如何表達這些字組。若要提升服務的正確性，請使用語料庫盡可能多提供 OOV 字組在該領域中的用法範例。重複語料庫中的 OOV 字組可以提升自訂語言模型的品質。如何重複語料庫中的字組，視您希望使用者在要辨識的音訊中表達這些字組的方式。您新增的句子中有愈多可以代表說話者使用該領域字組的環境定義，服務的辨識正確性就會更好。

服務不會套用簡單的字組比對演算法。其轉錄結果取決於使用字組的環境定義。服務在剖析語料庫時，會在自訂模型的語料庫句子中加入 n-grams（bi-grams、tri-grams 等等）的相關資訊。此資訊有助於服務轉錄音訊時更加精確，而且也說明了為什麼使用語料庫來訓練自訂模型，會比單獨使用自訂字組來訓練更有價值。

例如，會計師會遵循一套通用的標準和程序，也就是所謂的「一般公認會計原則 (GAAP)」。當您建立金融領域的自訂模型時，請提供在環境定義中使用 GAAP 一詞的句子。這些句子有助於服務區分一般詞組（例如「它們之間的落差很小」）與領域專用詞組（例如「GAAP 提供測量及揭露財務資訊的準則」）。

一般而言，語料庫最好在不同環境定義中使用字組，如此可以提升服務學習這些字組的程度。不過，如果使用者只在幾個環境定義中說出這些字組，則在其他環境定義中顯示這些字組並不會提升自訂模型的品質。說話者永遠不會在那些環境定義中使用這些字組。如果說話者可能會經常使用相同的詞組，則在語料庫中重複該詞組就可以提升模型的品質。（在某些情況下，即使直接在自訂模型中新增幾個自訂字組，也可以造成正向的改變。）

### 準備語料庫文字檔
{: #prepareCorpus}

請遵循下列準則來準備語料庫文字檔：

-   如果包含非 ASCII 字元，則提供以 UTF-8 編碼的純文字檔。服務在遇到這類字元時，會採用 UTF-8 編碼。

    請確定您知道語料庫文字檔的字元編碼。服務會保留它在文字檔中找到的編碼。在使用自訂語言模型中的字組時，必須使用該編碼。如需相關資訊，請參閱[字元編碼](#charEncoding)。
    {: important}
-   對語料庫中的字組使用一致的大寫。字組資源會區分大小寫。只有在必要時，才會混合大小寫字母及使用大寫。
-   一行包含一個語料庫的句子，並使用換行符號終止每一行。將多個句子包含在同一行可能會降低正確性。
-   將個人姓名作為離散單位新增在個別字行上。不要將姓名中的字組新增至不同行中或新增作為個別的自訂字組，且不要在語料庫的同一行中包含多個姓名。下列範例顯示為三個姓名提升正確性的正確方法：

    ```
    Gakuto Kutara
    Sebastian Leifson
    Malcolm Ingersol
    ```
    {: codeblock}

    在適用處包含額外的環境定義資訊，例如 `Doctor Sebastian Leifson` 或 `President Malcolm Ingersol`。如同所有字組一樣，多次重複姓名，且在不同的環境定義中重複使用（可能的話），可提升辨識正確性。
-   小心拼字錯誤。服務會假設拼字錯誤為新字組。除非您在訓練模型之前予以更正，否則服務會將其新增至模型的詞彙。請記住這句格言：*Garbage in, garbage out!*（垃圾進，垃圾出！）。

句子愈多，結果愈精確。然而，服務會限制模型的所有來源合併的字組總數上限為 1000 萬個，而 OOV 字組為 9 萬個。

### 新增語料庫檔案時會發生什麼情況？
{: #parseCorpus}

當您新增語料庫檔案時，服務會分析檔案的內容。它會擷取發現的所有新 (OOV) 字組，並將每個 OOV 字組新增至自訂模型的字組資源中。為了要從內容中提煉出最精確的含義，服務會對其從語料庫檔案中讀取的資料進行記號化及剖析。以下各節將說明服務如何針對每個支援的語言剖析語料庫檔案。

#### 英文、法文、德文、西班牙文及巴西葡萄牙文的剖析
{: #corpusLanguages}

下列說明適用於美式和英式英文、法文、德文、西班牙文及巴西葡萄牙文。

-   將數字轉換成其對等的字組，例如：
    -   *若為英文，*`500` 會變成 `five hundred`，而 `0.15` 會變成 `zero point fifteen`。
    -   *若為法文，*`500` 會變成 `cinq cents`，而 `0,15` 會變成 <code>z&eacute;ro quinze</code>。
    -   *若為德文，*`500` 會變成 <code>f&uuml;nfhundert</code>，而 `0,15` 會變成 <code>null punkt f&uuml;nfzehn</code>。
    -   *若為西班牙文，*`500` 會變成 `quinientos`，而 `0,15` 會變成 `cero coma quince`。
    -   *若為巴西葡萄牙文，*`500` 會變成 `quinhentos`，而 `0,15` 會變成 `zero ponto quinze`。
-   將包含特定符號的記號轉換成有意義的字串表示法，例如：
    -   轉換 `$`（錢幣符號）和數字：
        -   *若為英文，*`$100` 會變成 `one hundred dollars`。
        -   *若為法文，*`$100` 會變成 `cent dollar`。
        -   *若為德文，*`$100` 和 `100$` 會變成 `einhundert dollar`。
        -   *若為西班牙文，*`$100` 和 `100$` 會變成 <code>cien d&oacute;lares</code>（如果用語是 `es-LA`，則變成 `cien pesos`）。
        -   *若為巴西葡萄牙文，* `$100` 和 `100$` 會變成 <code>cem d&oacute;lares</code>。
    -   轉換 <code>&euro;</code>（歐元符號）和數字：
        -   *若為英文，*<code>&euro;100</code> 會變成 `one hundred euros`。
        -   *若為法文，*<code>&euro;100</code> 會變成 `cent euros`。
        -   *若為德文，*<code>&euro;100</code> 和 <code>100&euro;</code> 會變成 `einhundert euro`。
        -   *若為西班牙文，*<code>&euro;100</code> 和 <code>100&euro;</code> 會變成 `cien euros`。
        -   *若為巴西葡萄牙文，*<code>&euro;100</code> 和 <code>100&euro;</code> 會變成 `cem euros`。
    -   轉換前面有數字的 `%`（百分比符號）：
        -   *若為英文，*`100%` 會變成 `one hundred percent`。
        -   *若為法文，*`100%` 會變成 `cent pourcent`。
        -   *若為德文，*`100%` 會變成 `einhundert prozent`。
        -   *若為西班牙文，*`100%` 會變成 `cien por ciento`。
        -   *若為巴西葡萄牙文，*`100%` 會變成 `cem por cento`。

    此清單未盡其詳。服務會視需要針對其他字元進行類似的調整。
-   視其環境定義，來處理非英數字元、標點符號及特殊字元。例如，服務會移除 `$`（錢幣符號）或 <code>&euro;</code>（歐元符號），除非後面接著數字。處理方式取決於環境定義，且在整個支援的語言中保持一致。
-   忽略以 `( )`（括弧）、`< >`（角括弧）、`[ ]`（方括弧）或 `{ }`（大括弧）括住的詞組。

#### 日文的剖析
{: #corpusLanguages-jaJP}

-   將所有字元轉換成全形字元。
-   將數字轉換成其對等的字組，例如，`500` 會變成 <code>&#20116;&#30334;</code>，而 `0.15` 會變成 <code>&#12295;&#12539;&#19968;&#20116;</code>。
-   不會將包含符號的記號轉換成對等字串，例如，`100%` 會變成 <code>&#30334;&#65285;</code>。
-   不會自動移除標點符號。如果您的應用程式是以轉錄為基礎，而不是以聽寫為基礎，則 {{site.data.keyword.IBM_notm}} 強烈建議您移除標點符號。

#### 韓文的剖析
{: #corpusLanguages-koKR}

-   將數字轉換成其對等的字組，例如，<code>10</code> 會變成 <code>&#49901;</code>。
-   移除下列標點符號和特殊字元：`- ( ) * : . , ' "`。不過，針對其他語言移除的標點符號和特殊字元，並不是全部都會針對韓文移除，例如：
    -   只會移除出現在輸入行尾端的句點 (`.`) 符號。
    -   不會移除波狀 (`~`) 符號。
    -   不會移除或處理 Unicode 寬字元符號，例如 <code>&#8230;</code>（三個點或省略符號）。

    一般而言，{{site.data.keyword.IBM_notm}} 會建議您先移除標點符號、特殊字元和 Unicode 寬字元，然後再處理語料庫檔案。
-   不會移除或忽略以 `( )`（括弧）、`< >`（角括弧）、`[ ]`（方括弧）或 `{ }`（大括弧）括住的詞組。
-   將包含特定符號的記號轉換成有意義的字串表示法，例如：
    -   `24%` 會變成 <code>&#51060;&#49901;&#49324;&#54140;&#49468;&#53944;</code>。
    -   `$10` 會變成 <code>&#49901;&#45804;&#47084;</code>。

    此清單未盡其詳。服務會視需要針對其他字元進行類似的調整。
-   對於包含拉丁（英文）字元或混合韓文和拉丁字元的詞組，服務會完全依照該詞組出現在語料庫檔案中的形式，為其建立 OOV 字組。然後，它會為根據韓文轉錄的字組，建立類似音發音。
   - 它為 OOV 字組 `London` 提供的類似音為 <code>&#47088;&#45912;</code>。
   - 它為 OOV 字組 <code>IBM&#54856;&#54168;&#51060;&#51648;</code> 提供的類似音為 <code>&#50500;&#51060;&#32;&#48708;&#32;&#50656;&#32;&#54856;&#54168;&#51060;&#51648;</code>。

## 使用自訂字組
{: #workingWords}

您可以使用 `POST /v1/customizations/{customization_id}/words` 和 `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法，將新字組新增至自訂模型的字組資源。您也可以使用這些方法來修改或擴增字組資源中的字組。

例如，您可能會需要使用這些方法來更正從語料庫新增字組時所造成的拼字錯誤或其他錯誤。您可能也需要為現有字組新增類似音定義。如果您修改現有字組，則提供的新資料會改寫該字組在字組資源中的現有定義。新增字組的規則也適用於修改現有字組。

### 字元編碼
{: #charEncoding}

一般而言，您可能會從語料庫中新增大部分自訂字組。請確定您知道語料庫文字檔中使用的字元編碼。服務會保留它在文字檔中找到的編碼。

在使用自訂語言模型中的個別字組時，必須使用該編碼。當您使用 `GET`、`PUT` 或 `DELETE /v1/customizations/{customization_id}/words/{word_name}` 方法指定字組時，如果字組包含非 ASCII 字元，則必須將您在 URL 中傳遞的 `word_name` 以 URL 編碼。

例如，下表顯示相同字母以兩種不同編碼 ASCII 和 UTF-8 呈現的樣子。您可以在 URL 上以 `z` 來傳遞 ASCII 字元。您必須以 `%EF%BD%9A` 來傳遞 UTF-8 字元。
<table>
  <caption>表 1. 字元編碼範例</caption>
  <tr>
    <th style="text-align:left">字母</th>
    <th style="text-align:center">編碼</th>
    <th style="text-align:center">值</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      `z`
    </td>
    <td style="text-align:center">
      ASCII
    </td>
    <td style="text-align:center">
      `0x7a` (`7a`)
    </td>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      <code>&#xff5a;</code>
    </td>
    <td style="text-align:center">
      UTF-8 十六進位
    </td>
    <td style="text-align:center">
      `0xEF 0xBD 0x9A` (`efbd9a`)
    </td>
  </tr>
</table>

### 使用 sounds_like 欄位
{: #soundsLike}

`sounds_like` 欄位指定說話者的字組發音方式。依預設，服務會自動以該字組的拼字來填入此欄位。您可以針對很難發音的字組，或是可以用不同方式發音的字組，提供最多五個替代發音。請考慮使用此欄位來：

-   *為字首語提供不同的發音。* 例如，字首語 `NCAA` 可以依其拼法或口語發音為 *N. C. double A*。下列範例為 `NCAA` 這個字新增這兩種類似音發音。

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

-   *處理外來字組。* 例如，法文字組 <code>gar&ccedil;on</code> 含有在英語中找不到的字元。您可以指定 `gaarson` 的類似音，將 <code>&ccedil;</code> 取代為 `s`，以告訴該服務說英文的人會如何發這個字的音。

語音辨識會使用統計演算法來分析音訊，因此新增字組並不保證服務會完全準確地將其轉碼。當您新增字組時，請考量它可能如何發音。使用 `sounds_like` 欄位來提供可反映字組念法的各種發音。以下各節提供用來指定類似音發音的語言特定準則。

#### 適用於英文（美式和英式）的準則
{: #wordLanguages-enUS-enGB}

*適用於美式和英式英文的準則：*

-   使用英文字母：`a-z` 和 `A-Z`。
-   使用能夠以英文發音的真實或虛構字組來代表很難發音的字組，例如以 `shuchesnie` 代表字組 `Sczcesny`。
-   以對等英文字母來替換非英文字母，例如以 `s` 替換 <code>&ccedil;</code>，或以 `ny` 代表 <code>&ntilde;</code>。
-   以無重音字母來替換重音字母，例如以 `a` 替換 <code>&agrave;</code>，或以 `e` 替換 <code>&egrave;</code>。
-   您可以包含以空格區隔的多個字組，但服務強制字元總數上限為 40（不包含空格）。

*僅適用於美式英文的準則：*

-   若要發出單一字母的音，請在字母後面接上一個句點。如果句點後面接著另一個字元，請務必在句點與下一個字元之間使用空格。例如，使用 `N. C. A. A.`，而*不是* `N.C.A.A.`。
-   使用數字的拼字，例如 `seventy-five` 代表 `75`。

*僅適用於英式英文的準則：*

-   若為英式英文，您**不能**在類似音發音中使用句點或橫線。
-   若要發出單一字母的音，請在字母後面接上一個空格。例如，使用 `N C A A`，而*不是* `N. C. A. A.`、`N.C.A.A.` 或 `NCAA`。
-   使用數字的拼字，不包含橫線，例如 `seventy five` 代表 `75`。

#### 適用於法文、德文、西班牙文及巴西葡萄牙文的準則
{: #wordLanguages-esES-frFR}

-   您**不能**在類似音發音中使用橫線。
-   使用對該語言有效的英文字母：`a-z` 和 `A-Z`，包括有效的重音字母。
-   若要發出單一字母的音，請在字母後面接上一個句點。如果句點後面接著另一個字元，請務必在句點與下一個字元之間使用空格。例如，使用 `N. C. A. A.`，而*不是* `N.C.A.A.`。
-   使用能夠以該語言發音的真實或虛構字組來代表很難發音的字組。
-   使用數字的拼字，不包含橫線，例如，要表示 `75`，請使用：
    -   *法文：*`soixante quinze`
    -   *德文：*<code>f&uuml;nfundsiebzig</code>
    -   *西班牙文：*`setenta y cinco`
    -   *巴西葡萄牙文：*`setenta e cinco`
-   您可以包含以空格區隔的多個字組，但服務強制字元總數上限為 40（不包含空格）。

#### 適用於日文的準則
{: #wordLanguages-jaJP}

-   使用 <code>&#8213;</code> 延長符號（日文中的 *chou-on* 或 &#38263;&#38899;），以只使用全形片假名字元。不要使用半形字元。
-   在下列音節環境定義中，只使用拗音（日文中的 *yoh-on* 或 &#25303;&#38899;）：

    <code>&#12452;&#12455;</code>、<code>&#12454;&#12451;</code>、<code>&#12454;&#12455;</code>、<code>&#12454;&#12457;</code>、<code>&#12461;&#12451;</code>、<code>&#12461;&#12515;</code>、<code>&#12461;&#12517;</code>、<code>&#12461;&#12519;</code>、<code>&#12462;&#12515;</code>、<code>&#12462;&#12517;</code>、<code>&#12462;&#12519;</code>、<code>&#12463;&#12449;</code>、<code>&#12463;&#12451;</code>、<code>&#12463;&#12455;</code>、<code>&#12463;&#12457;</code>、<br/>
<code>&#12464;&#12449;</code>、<code>&#12464;&#12457;</code>、<code>&#12471;&#12451;</code>、<code>&#12471;&#12455;</code>、<code>&#12471;&#12515;</code>、<code>&#12471;&#12517;</code>、<code>&#12471;&#12519;</code>、<code>&#12472;&#12451;</code>、<code>&#12472;&#12455;</code>、<code>&#12472;&#12515;</code>、<code>&#12472;&#12517;</code>、<code>&#12472;&#12519;</code>、<code>&#12473;&#12451;</code>、<code>&#12474;&#12451;</code>、<code>&#12481;&#12455;</code>、<br/>
<code>&#12481;&#12515;</code>、<code>&#12481;&#12517;</code>、<code>&#12481;&#12519;</code>、<code>&#12482;&#12455;</code>、<code>&#12482;&#12515;</code>、<code>&#12482;&#12517;</code>、<code>&#12482;&#12519;</code>、<code>&#12484;&#12449;</code>、<code>&#12484;&#12451;</code>、<code>&#12484;&#12455;</code>、<code>&#12484;&#12457;</code>、<code>&#12486;&#12451;</code>、<code>&#12486;&#12517;</code>、<code>&#12487;&#12451;</code>、<code>&#12487;&#12515;</code>、<br/>
<code>&#12487;&#12517;</code>、<code>&#12487;&#12519;</code>、<code>&#12488;&#12453;</code>、<code>&#12489;&#12453;</code>、<code>&#12491;&#12455;</code>、<code>&#12491;&#12515;</code>、<code>&#12491;&#12517;</code>、<code>&#12491;&#12519;</code>、<code>&#12498;&#12515;</code>、<code>&#12498;&#12517;</code>、<code>&#12498;&#12519;</code>、<code>&#12499;&#12515;</code>、<code>&#12499;&#12517;</code>、<code>&#12499;&#12519;</code>、<code>&#12500;&#12451;</code>、<br/>
<code>&#12500;&#12515;</code>、<code>&#12500;&#12517;</code>、<code>&#12500;&#12519;</code>、<code>&#12501;&#12449;</code>、<code>&#12501;&#12451;</code>、<code>&#12501;&#12455;</code>、<code>&#12501;&#12457;</code>、<code>&#12501;&#12517;</code>、<code>&#12511;&#12515;</code>、<code>&#12511;&#12517;</code>、<code>&#12511;&#12519;</code>、<code>&#12522;&#12451;</code>、<code>&#12522;&#12455;</code>、<code>&#12522;&#12515;</code>、<code>&#12522;&#12517;</code>、<br/>
<code>&#12522;&#12519;</code>、<code>&#12532;&#12449;</code>、<code>&#12532;&#12451;</code>、<code>&#12532;&#12455;</code>、<code>&#12532;&#12457;</code>、<code>&#12532;&#12517;</code>

-   只在促音（日文中的 *soku-on* 或 &#20419;&#38899;）之後使用下列音節：

    <code>&#12496;</code>、<code>&#12499;</code>、<code>&#12502;</code>、<code>&#12505;</code>、<code>&#12508;</code>、<code>&#12481;</code>、<code>&#12481;&#12455;</code>、<code>&#12481;&#12515;</code>、<code>&#12481;&#12517;</code>、<code>&#12481;&#12519;</code>、<code>&#12480;</code>、<code>&#12487;</code>、<code>&#12487;&#12451;</code>、<code>&#12489;</code>、<code>&#12489;&#12453;</code>、<code>&#12501;</code>、<br/>
<code>&#12501;&#12449;</code>、<code>&#12501;&#12451;</code>、<code>&#12501;&#12455;</code>、<code>&#12501;&#12457;</code>、<code>&#12460;</code>、<code>&#12462;</code>、<code>&#12464;</code>、<code>&#12466;</code>、<code>&#12468;</code>、<code>&#12495;</code>、<code>&#12498;</code>、<code>&#12504;</code>、<code>&#12507;</code>、<code>&#12472;</code>、<code>&#12472;&#12455;</code>、<code>&#12472;&#12515;</code>、<br/>
<code>&#12472;&#12517;</code>、<code>&#12472;&#12519;</code>、<code>&#12459;</code>、<code>&#12461;</code>、<code>&#12463;</code>、<code>&#12465;</code>、<code>&#12467;</code>、<code>&#12461;&#12515;</code>、<code>&#12461;&#12517;</code>、<code>&#12461;&#12519;</code>、<code>&#12497;</code>、<code>&#12500;</code>、<code>&#12503;</code>、<code>&#12506;</code>、<code>&#12509;</code>、<code>&#12500;&#12515;</code>、<br/>
<code>&#12500;&#12517;</code>、<code>&#12500;&#12519;</code>、<code>&#12469;</code>、<code>&#12473;</code>、<code>&#12475;</code>、<code>&#12477;</code>、<code>&#12471;</code>、<code>&#12471;&#12455;</code>、<code>&#12471;&#12515;</code>、<code>&#12471;&#12517;</code>、<code>&#12471;&#12519;</code>、<code>&#12479;</code>、<code>&#12486;</code>、<code>&#12488;</code>、<code>&#12484;</code>、<code>&#12470;</code>、<br/>
<code>&#12474;</code>、<code>&#12476;</code>、<code>&#12478;</code>

-   不要使用 <code>&#12531;</code> 作為字組的第一個字元。例如，使用 <code>&#12454;&#12540;&#12531;&#12488;</code>，而不是 <code>&#12531;&#12540;&#12488;</code>，後者是無效的。
-   許多複合字是由*字首+名詞* 或*名詞+字尾* 組成。服務的基礎詞彙涵蓋經常出現的大部分複合字（例如，<code>&#x9577;&#x96FB;&#x8A71;</code> 和 <code>&#x53E4;&#x65B0;&#x805E;</code>），但沒有不常出現的那些複合字。如果您的語料庫通常包含複合字，自訂作業的首要步驟就是將其新增為一個字組。例如，<code>&#x53E4;&#x925B;&#x7B46;</code> 在一般日文中並不常見；如果您經常使用它，請將它新增至您的自訂模型，以提升轉錄的正確性。
-   不要在字尾使用促音。

#### 適用於韓文的準則
{: #wordLanguages-koKR}

-   使用韓國的韓文字元、符號及音節。
-   您也可以使用拉丁（英文）字母：`a-z` 和 `A-Z`。
-   不要使用前面字組中未包含的任何字元或符號。

### 使用 display_as 欄位
{: #displayAs}

`display_as` 欄位指定字組在轉錄文字中的顯示方式。其目的是要讓服務以不同於該字組拼字的方式顯示字串。例如，您可以指定將字組 `hhonors` 顯示為 `HHonors`，而不論它聽起像是 `hilton honors` 或 `h honors`。

```bash
curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

再舉一個例子，您可以指定將字組 `IBM` 顯示為 <code>IBM&trade;</code>。

<pre><code class="language-bash">curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"I. B. M.\"], \"display_as\":\"IBM&#8482;\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IBM"</code></pre>

#### 與智慧型格式化和數字編寫互動
{: #displaySmart}

如果您在辨識要求中使用 `smart_formatting` 或 `redaction` 參數，請注意服務先會將智慧型格式化和編寫套用於字組，然後才考量該字組的 `display_as` 欄位。您可能需要試驗結果，以確保這些特性不會干擾自訂字組的顯示方式。您可能也需要新增自訂字組來配合這些效果。

例如，假設您新增自訂字組 `one`，而其 `display_as` 欄位為 `one`。智慧型格式化會將字組 `one` 變更為數字 `1`，而未套用 display-as 值。若要解決這個問題，您可以為數字 `1` 新增自訂字組，並將相同的 `display_as` 欄位套用於該字組。

如需使用這些特性的相關資訊，請參閱[智慧型格式化](/docs/services/speech-to-text/output.html#smart_formatting)和[數字編寫](/docs/services/speech-to-text/output.html#redaction)。

### 新增或修改自訂字組時會發生什麼情況？
{: #parseWord}

服務如何回應新增或修改自訂字組的要求，取決於您提供哪些值，以及服務的基礎詞彙中是否有該字組存在。

<table>
  <caption>表 1. 新增具有不同欄位的自訂字組</caption>
  <tr>
    <th style="width:20%; text-align:center; vertical-align:bottom"><code>sounds_like</code> 欄位為...</th>
    <th style="width:20%; text-align:center; vertical-align:bottom"><code>display_as</code> 欄位為...</th>
    <th style="text-align:left; vertical-align:bottom">回應</th>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      未指定
    </td>
    <td style="text-align:center; vertical-align:top">
      未指定
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>如果服務的基礎詞彙中不存在該字組，</em>服務會將 <code>sounds_like</code> 欄位設為其字組的發音。它會將 <code>display_as</code> 欄位設為 <code>word</code> 欄位的值。</li>
        <li style="margin:10px 0px; line-height:120%">
          <em>如果服務的基礎詞彙中存在該字組，</em>服務會將 <code>sounds_like</code> 和 <code>display_as</code> 欄位保留為空的。只有在服務的基礎詞彙中存在該字組時，這些欄位才會是空的。該字組存在於模型的字組資源中並無害，但沒有必要。</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      已指定
    </td>
    <td style="text-align:center; vertical-align:top">
      未指定
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>如果 <code>sounds_like</code> 欄位有效，</em>服務會將 <code>display_as</code> 欄位設為 <code>word</code> 欄位的值。
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>如果 <code>sounds_like</code> 欄位無效：</em>
          <ul style="margin-left:20px; padding:0px">
            <li style="margin:10px 0px; line-height:120%">
              <code>POST /v1/customizations/{customization_id}/words</code> 方法會在模型的字組資源中，將 <code>error</code> 欄位新增至字組。
            </li>
            <li style="margin:10px 0px; line-height:120%">
              <code>PUT /v1/customizations/{customization_id}/words/{word_name}</code> 方法失敗，並傳回 400 回應碼和錯誤訊息。服務不會將字組新增至字組資源。</li>
          </ul>
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      未指定
    </td>
    <td style="text-align:center; vertical-align:top">
      已指定
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>如果服務的基礎詞彙中不存在該字組，</em>服務會將 <code>sounds_like</code> 欄位設為其字組的發音，並依指定保留 <code>display_as</code> 欄位。</li>
        <li style="margin:10px 0px; line-height:120%">
          <em>如果服務的基礎詞彙中存在該字組，</em>服務會將 <code>sounds_like</code> 欄位保留為空的，並依指定保留 <code>display_as</code> 欄位。
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      已指定
    </td>
    <td style="text-align:center; vertical-align:top">
      已指定
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>如果 <code>sounds_like</code> 欄位有效，</em>服務會將 <code>sounds_like</code> 和 <code>display_as</code> 欄位設定為指定值。</li>
        <li style="margin:10px 0px; line-height:120%">
          <em>如果 <code>sounds_like</code> 欄位無效，</em>服務會依照已指定 <code>sounds_like</code> 欄位、但未指定 <code>display_as</code> 欄位的情況回應。
        </li>
      </ul>
    </td>
  </tr>
</table>

## 驗證字組資源
{: #validateModel}

尤其是當您將語料庫新增至自訂語言模型，或一次新增多個自訂字組時，請檢查模型字組資源中的 OOV 字組。

-   *尋找拼字錯誤和其他錯誤。*尤其是當您新增語料庫（可能很大）時，很容易會發生錯誤。語料庫（或文法）檔案中的拼字錯誤會造成將新字組新增至模型字組資源的意外結果，就像將格式錯誤的 HTML 標籤留在語料庫檔案一樣。
-   *驗證類似音發音。*服務會自動為 OOV 字組產生類似音發音。在大部分情況下，這些發音已足夠。然而，如果是具有不尋常拼法或很難發音的字組，以及字首語和技術用語，則建議檢查發音的正確性。

若要驗證及（必要的話）更正自訂模型的字組，而不論其新增至字組資源的方式，請使用下列方法：

-   使用 `GET /v1/customizations/{customization_id}/words` 方法列出自訂模型中的所有字組，或使用 `GET /v1/customizations/{customization_id}/words/{word_name}` 方法查詢個別字組。如需相關資訊，請參閱[列出自訂語言模型中的字組](/docs/services/speech-to-text/language-words.html#listWords)。
-   修改自訂模型中的字組以更正錯誤，或使用 `POST /v1/customizations/{customization_id}/words` 或 `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法新增 sounds-like 或 display-as 值。如需相關資訊，請參閱[使用自訂字組](#workingWords)。
-   使用 `DELETE /v1/customizations/{customization_id}/words/{word_name}` 方法，刪除誤建的無關字組（例如，語料庫中拼字錯誤或其他錯誤）。如需相關資訊，請參閱[刪除自訂語言模型中的字組](/docs/services/speech-to-text/language-words.html#deleteWord)。
    -   如果字組是擷取自語料庫，您可以改為更新語料庫文字檔來更正錯誤，然後使用 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法的 `allow_overwrite` 參數來重新載入檔案。如需相關資訊，請參閱[將語料庫新增至自訂語言模型](/docs/services/speech-to-text/language-create.html#addCorpus)。
    -   如果字組是擷取自文法，您可以更新文法檔來更正錯誤，然後使用 `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法的 `allow_overwrite` 參數來重新載入檔案。如需相關資訊，請參閱[將文法新增至自訂語言模型](/docs/services/speech-to-text/grammar-add.html#addGrammar)。
