---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

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

# 了解语法
{: #grammarUnderstand}

以下示例介绍了 {{site.data.keyword.speechtotextfull}} 服务对语法的支持。这些示例创建了两个简单的 ABNF 语法，并显示将其用于语音识别时可能获得的结果。这些示例说明了检查服务在抄本中包含的置信度分数的重要性。
{: shortdesc}

这些示例仅提供语音识别请求的结果。有关说明如何传递语法用于语音识别的示例，请参阅[将语法用于语音识别](/docs/services/speech-to-text/grammar-use.html)。此外，这些示例是非常基本的例子。有关更复杂语法的示例，请参阅[示例语法](/docs/services/speech-to-text/grammar-examples.html)。

## 单短语匹配：yesno 语法
{: #yesnoGrammar}

第一个示例定义是非常简单的 `yesno` 语法，该语法接受两个有效的单个词响应：`yes` 和 `no`。对于用户只能使用两个短语之一进行响应的情况，此语法非常有用。

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $yesno;

$yesno = yes | no ;
```
{: codeblock}

将此语法应用于语音识别请求时，服务可以返回包含分数的抄本，分数指示服务对匹配项的置信度。如果输入显然与两个短语中的任一项都不匹配，那么也可以不返回任何结果。

例如，如果用户回复 `yes`，那么服务返回的响应可能非常类似于以下结果。`confidence` 字段中的分数指示这是完全可靠的匹配项。

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

但是，假设用户回复的是其他项，例如 `nope`。对于这种情况，服务可能会返回具有极低置信度分数的结果，或者根本不返回任何结果。空结果最明确地表明响应与语法不匹配。空响应更可能发生在复杂语法中，其中有效响应必须与特定多短语序列相匹配。

## 多短语匹配：名称语法
{: #namesGrammar}

使用多短语语法时，用户的响应必须完整才能被识别到。用户不能省略词或在响应中途停止。即使缺少一个词，也会导致服务返回空结果。

此外，如果用户说短语时，各短语之间的静默足够长，以指示它们是独立的话语时，那么服务可能返回多个抄本。例如，考虑简单的 `names` 语法，该语法可以与三个多词姓名之一相匹配。

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $names;
$names = Yi Wen Tan | Yon See | Youngjoon Lee ;
```
{: codeblock}

假设用户根据语法规则说了其中一个姓名 `Yon See`。服务返回的响应会指示匹配项的置信度级别非常高。

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

现在，假设用户在说两个姓名时中间有足够长的静默（至少为 0.8 秒），以指示它们是单独的话语：`Yon See` [静默 1.0 秒] `Yi Wen Tan`。在这种情况下，服务会为每个抄本发送具有不同置信度分数的两个单独响应。

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

最后，假设用户改为说类似下面的内容：`Yon See` [静默 1.0 秒] `Young Says He`。对于第一个短语 `Yon See`，服务会发送包含置信度分数的正匹配项，与先前的示例类似。对于第二个短语 `Young Says He`，服务的响应可能是下面两者之一：

-   服务可能不发送任何响应，这指示短语与所有语法规则都不匹配。
-   服务可能会改为发送具有低置信度分数的响应，这指示短语在声学上类似于其中一个规则，但不是可能的匹配项。

## 置信度分数和空结果
{: #confidenceScores}

对于如先前示例中所示的相对简单的语法，服务可能会返回低置信度分数的结果，即使响应看起来与语法根本不匹配也是如此。这可能看起来很意外，但低置信度分数的响应可能指示服务针对给定音频可以找到的最佳匹配项，尽管响应不太可能与语法相匹配。如果响应与语法不匹配，那么结果的置信度值通常会足够低，以指示语法不太可能生成响应。

在评估响应是否满足语法时，请始终考虑置信度分数。如果您不确定要设置什么阈值以用于根据其置信度来拒绝结果，请使用初始值 0.5。如果您遇到的正确结果拒绝率高得出奇，请降低置信度阈值；例如，对于某些应用程序，0.1 可能很适合。如果您遇到大量错误识别，请提高针对您应用程序的置信度阈值。

在许多情况下，空结果或具有极低置信度分数的结果是有效响应。它可能指示用户没有理解问题或可用选项。您始终可以在不使用语法的情况下识别用户的音频响应，但这样做可能会面临获得对于语法无效的结果的风险。语法提供的结果比 n 元语法更可靠，后者对于除了完全静默之外的其他任何情况都始终会返回结果。

能确定结果肯定是语法所定义的有效短语之一，这是一个十分强大的功能，可以简化应用程序的响应处理。一般而言，服务只对语法生成的话语返回具有高置信度的结果。对于第一个示例中的 `yesno` 语法，要可靠地将短语 `nope` 识别为有效响应，必须将该短语添加到语法中。
