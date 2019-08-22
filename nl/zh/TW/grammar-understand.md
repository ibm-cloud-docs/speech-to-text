---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-04"

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

# 瞭解文法
{: #grammarUnderstand}

下列範例介紹 {{site.data.keyword.speechtotextfull}} 服務的文法支援。範例會建立兩個簡單的 ABNF 文法，並顯示它們用於語音辨識時的可能結果。這些範例說明檢查服務附在文字記錄之信賴分數的重要性。
{: shortdesc}

範例只會提供語音辨識要求的結果。如需示範如何傳遞文法以用於語音辨識的範例，請參閱[使用文法進行語音辨識](/docs/services/speech-to-text?topic=speech-to-text-grammarUse)。這些範例也非常基本。如需更複雜的文法範例，請參閱[文法範例](/docs/services/speech-to-text?topic=speech-to-text-grammarExamples)。

## 單一詞組相符項：yesno 文法
{: #yesnoGrammar}

第一個範例定義了一個非常簡單的 `yesno` 文法，它接受兩個有效的單一字組回應：`yes` 和 `no`。當使用者只能回應兩個詞組的其中一個時，此文法就很好用。

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $yesno;

$yesno = yes | no ;
```
{: codeblock}

當您將這個文法套用至語音辨識要求時，服務可以傳回文字記錄，並附上指出其對於相符項之信賴度的分數。如果輸入顯然不符合兩個詞組的其中之一，它也可能不傳回任何結果。

例如，如果使用者回覆 `yes`，則服務可能傳回非常類似下列結果的回應。`confidence` 欄位中的分數指出這是完全可靠的相符項。

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 1.0,
          "transcript": "yes"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

但舉例來說，假設使用者回覆 `nope`。服務可能傳回信賴分數很低的結果，或是完全不傳回結果。空的結果最清楚地表示了回應不符合文法。空的回應較可能發生在複雜的文法，也就是有效的回應必須符合特定的多詞組序列。

## 多詞組相符項：names 文法
{: #namesGrammar}

使用多詞組文法時，使用者的回應必須完整，才能夠被辨識出來。使用者不能省略字組，或在回應中間停止。即使缺少單一字組都可能導致服務傳回空的結果。

此外，如果使用者說出的詞組，中間以足夠的靜音隔開，表示它們是獨立的話語，則服務可能會傳回多份文字記錄。例如，請參考簡單的 `names` 文法，它可以比對三個多字組名稱之一。

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $names;
$names = Yi Wen Tan | Yon See | Youngjoon Lee ;
```
{: codeblock}

假設使用者說出了來自文法規則的其中一個名稱：`Yon See`。服務會傳回回應，指出對於相符項的信賴水準很高。

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.92,
          "transcript": "Yon See"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

現在，假設使用者說出了兩個名稱，且以足夠的靜音隔開（至少 0.8 秒），表示它們是分開的話語：`Yon See` [靜音 1.0 秒] `Yi Wen Tan`。在此案例中，服務會針對每一份文字記錄，傳送兩個不同的回應，具有不同的信賴分數。

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.92,
          "transcript": "Yon See"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
},
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.83,
          "transcript": "Yi Wen Tan"
        }
      ],
      "final": true
    }
  ],
  "result_index": 1
}
```
{: codeblock}

最後，請考慮這樣的情況：使用者改為說出類似 `Yon See` [靜音 1.0 秒] `Young Says He` 的話。針對第一個詞組 `Yon See`，服務會傳送具有信賴分數的正面相符項，類似先前的範例。針對第二個詞組 `Young Says He`，服務可能有兩種可能回應的其中一個：

-   它可能不會傳送任何回應，指出詞組不符合其中一個文法的規則。
-   它可能改為傳送具有低信賴分數的回應，指出詞組與其中一個規則在聲學方面相似，但不是一個可能的相符項。

## 信賴分數和空的結果
{: #confidenceScores}

若為相對簡單的文法，例如上述範例中的文法，即使回應看似完全不符合文法，服務也可能傳回具有低信賴分數的結果。這可能令人驚訝，但具有低信賴分數的回應可能指出服務針對給定音訊所能找到的最佳符合項，儘管此回應不可能符合文法。如果回應不符合文法，結果的信賴度值通常會低到足以指出文法不可能產生此回應。

在評估回應是否滿足文法時，請務必考量信賴分數。如果您不知道要設定什麼臨界值以根據結果的信賴度拒絕該結果，請使用起始值 0.5。如果您非預期地針對正確的結果遭遇高拒絕率，請減少信賴度臨界值；例如，對於部分應用程式而言，0.1 可能是個好選項。如果您遇到大量不正確的辨識，請增加應用程式的信賴度臨界值。

在許多情況下，空結果或具有很低信賴分數的結果，都是有效的回應。它可能指出使用者不瞭解問題或可用的選項。您隨時都可以在不使用文法的情況下辨識使用者的音訊回應，但您便得要承擔得到對文法而言無效之結果的風險。文法提供的結果比 N 元語法更可靠，N 元語法針對非完全靜音的任何內容都一律會傳回結果。

知道結果必須是由文法所定義的其中一個有效詞組，是一項可以簡化應用程式回應處理的強大特性。一般而言，服務只會針對文法所產生的話語，傳回具有高信賴度的結果。針對第一個範例中的 `yesno` 文法，若要能可靠地將詞組 `nope` 辨識為有效的回應，您必須將該詞組新增到文法中。
