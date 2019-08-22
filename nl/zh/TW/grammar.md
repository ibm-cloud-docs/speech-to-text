---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-24"

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

# 將文法與自訂語言模型搭配使用
{: #grammars}

{{site.data.keyword.speechtotextfull}} 服務支援使用文法與自訂語言模型搭配。您可以將文法新增至自訂語言模型，並將它們用於語音辨識。文法限制服務可從音訊中辨識的詞組集。
{: shortdesc}

文法使用正式語言規格來定義一組用於轉錄字串的正式作業規則。這些規則指定如何從語言的英文字母構成有效的字串。當您在語音辨識中套用文法時，該服務只會傳回該文法所產生的一個以上詞組。

例如，當您需要辨識 *yes* 或 *no* 這類特定字組或詞組，或是個別字母或數字，或是名稱清單時，使用文法會比檢查替代字組和文字記錄更有效率。此外，藉由限制有效字串的搜尋空間，該服務可以更快速且更精確地傳遞結果。

當您使用自訂語言模型和文法進行語音辨識時，該服務可以從文法中傳回有效的詞組或空白結果。如果結果不是空的，則該服務會包含具有最終文字記錄的信賴分數，就像它對所有辨識要求那樣。就文法而言，此分數指出回應符合文法的可能性。總是可能會發生誤判，尤其是簡單的文法，因此您在評估其回應時，一定要考慮到服務結果的信賴度。

文法特性是測試版功能。該服務對於其支援語言模型自訂作業的所有語言都支援文法。
如需相關資訊，請參閱[自訂作業的語言支援](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)。
{: note}

## 支援的文法格式
{: #grammarFormats}

{{site.data.keyword.speechtotextshort}} 服務支援以下列標準格式定義的文法：

-   *擴充巴科斯範式 (ABNF)*，其使用類似於傳統 BNF 文法的純文字表示法。此格式的媒體類型為 `application/srgs`。
-   *XML Form*，其使用 XML 元素來表示文法。此格式的媒體類型為 `application/srgs+xml`。

這兩種文法格式都具有「上下文無關文法 (CFG)」的表達能力。不過，該服務只能解碼喬姆斯基階層中的 Type-3 正規文法。這類文法代表有限狀態機。

如需有關文法的一般資訊，請參閱下列維基百科頁面：

-   [語音辨識語法規格](https://wikipedia.org/wiki/Speech_Recognition_Grammar_Specification){: external}
-   [擴充巴科斯範式](https://wikipedia.org/wiki/Augmented_Backus%E2%80%93Naur_form){: external}
-   [喬姆斯基階層](https://wikipedia.org/wiki/Chomsky_hierarchy){: external}

## 語音辨識語法規格
{: #grammarSpecification}

{{site.data.keyword.speechtotextshort}} 服務支援 W3C [語音辨識語法規格 1.0 版](https://www.w3.org/TR/speech-grammar/){: external}所定義的文法。此規格提供有關支援格式及定義文法的詳細資訊。如需支援媒體類型的相關資訊，請參閱此規格的[附錄 G. 媒體類型和檔案字尾](https://www.w3.org/TR/speech-grammar/#AppG){: external}。

該服務目前*不* 支援「語音辨識語法規格」的所有特性。具體而言，該服務不支援此規格的下列各節所說明的特性：

-   [1.4 節：語意解譯](https://www.w3.org/TR/speech-grammar/#S1.4){: external}。{{site.data.keyword.IBM_notm}} 致力於在該服務的未來版本支援此特性。
-   [1.5 節：內嵌的文法](https://www.w3.org/TR/speech-grammar/#S1.5){: external}。{{site.data.keyword.IBM_notm}} 致力於在該服務的未來版本支援此特性。
-   [2.2.2 節：URI 的外部參照](https://www.w3.org/TR/speech-grammar/#S2.2.2){: external}。服務只支援區域參照，如 [2.2.1 節：區域參照](https://www.w3.org/TR/speech-grammar/#S2.2.1){: external}所述。換言之，文法必須自行包含。
-   [2.2.3 節：特殊規則](https://www.w3.org/TR/speech-grammar/#S2.2.3){: external}。
-   [2.2.4 節：參照 N-gram 文件（僅供參考）](https://www.w3.org/TR/speech-grammar/#S2.2.4){: external}。
-   [2.7 節：語言](https://www.w3.org/TR/speech-grammar/#S2.7){: external}。服務不支援語言切換。服務只支援每個文法一種全球語言。

文法的字組必須為 UTF-8 編碼（ASCII 是 UTF-8 的一個子集）。使用任何其他編碼可能會導致在編譯文法時發生問題，或在解碼時發生非預期的結果。服務會忽略文法標頭中指定的編碼。
{: note}
