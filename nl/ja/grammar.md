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

# カスタム言語モデルでの文法の使用
{: #grammars}

{{site.data.keyword.speechtotextfull}} サービスは、カスタム言語モデルでの文法の使用をサポートしています。 文法をカスタム言語モデルに追加して、音声認識のために使用できます。 文法によって、サービスが音声から認識できる句のセットが制限されます。
{: shortdesc}

文法では正式な言語仕様が使用され、ストリングの書き起こしに関する一連の作成ルールが定義されます。 これらのルールによって、言語の文字体系に基づいて有効なストリングを形成する方法が指定されます。 文法を音声認識に適用した場合、サービスはその文法によって生成される句の 1 つ以上のみを返すことができます。

例えば、特定の単語や句 (*はい* や*いいえ* など)、個別の文字や数字、または名前のリストを認識する必要がある場合は、代替の単語と書き起こし内容を調べるよりも、文法を使用する方が効果的な場合があります。 さらに、有効なストリングの検索スペースを制限することで、サービスは通常より高速かつ正確に結果を返すことができます。

カスタム言語モデルと文法を音声認識用に使用した場合、サービスはその文法から有効な句を返すことも、空の結果を返すこともあり得ます。 結果が空でない場合、サービスでは、すべての認識要求の場合と同様に、最終書き起こし内容に信頼度スコアが付加されます。 文法については、このスコアは応答がその文法に一致している可能性の高さを示します。 特にシンプルな文法の場合は誤認識は常に起こり得るため、サービスの応答を検証する際は、サービスの結果の信頼度を常に考慮する必要があります。

文法機能はベータ機能です。 サービスでは、言語モデルのカスタマイズをサポートしているすべての言語の文法がサポートされます。 詳しくは、[各言語でのカスタマイズのサポート](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport)を参照してください。
{: note}

## サポートされている文法フォーマット
{: #grammarFormats}

{{site.data.keyword.speechtotextshort}} サービスは、次の標準フォーマットで定義されている文法をサポートしています。

-   *Augmented Backus-Naur Form (ABNF)* では、従来の BNF 文法と似たプレーン・テキスト表現が使用されます。 このフォーマットのメディア・タイプは `application/srgs` です。
-   *XML Form* では、XML エレメントを使用して文法が表現されます。 このフォーマットのメディア・タイプは `application/srgs+xml` です。

どちらの文法フォーマットも、文脈自由文法 (CFG) の表現力を備えています。 ただし、サービスはチョムスキー階層内の 3 型正則文法のみをデコードできます。 このような文法は有限状態オートマトンを表現します。

文法に関する一般情報については、以下の Wikipedia ページを参照してください。

-   [Speech Recognition Grammar Specification](https://wikipedia.org/wiki/Speech_Recognition_Grammar_Specification){: external}
-   [Augmented Backus-Naur form](https://wikipedia.org/wiki/Augmented_Backus%E2%80%93Naur_form){: external}
-   [Chomsky hierarchy](https://wikipedia.org/wiki/Chomsky_hierarchy){: external}

## Speech Recognition Grammar Specification
{: #grammarSpecification}

{{site.data.keyword.speechtotextshort}} サービスは、W3C [Speech Recognition Grammar Specification Version 1.0](https://www.w3.org/TR/speech-grammar/){: external} で定義されている文法をサポートしています。 この仕様では、サポートされているフォーマットに関する詳細情報と、文法の定義に関する詳細情報が提供されています。 サポートされているメディア・タイプについては、当仕様の [Appendix G. Media Types and File Suffix](https://www.w3.org/TR/speech-grammar/#AppG){: external} を参照してください。

サービスは現時点では、Speech Recognition Grammar Specification のすべての機能をサポート*していません*。 具体的には、サービスは仕様の次のセクションで説明されている機能をサポートしていません。

-   [セクション 1.4 Semantic Interpretation](https://www.w3.org/TR/speech-grammar/#S1.4){: external}。 {{site.data.keyword.IBM_notm}} は、サービスの将来のリリースでこの機能をサポートすることを目指しています。
-   [セクション 1.5 Embedded Grammars](https://www.w3.org/TR/speech-grammar/#S1.5){: external}。 {{site.data.keyword.IBM_notm}} は、サービスの将来のリリースでこの機能をサポートすることを目指しています。
-   [セクション 2.2.2 External Reference by URI](https://www.w3.org/TR/speech-grammar/#S2.2.2){: external}。 サービスは、[セクション 2.2.1 Local References ](https://www.w3.org/TR/speech-grammar/#S2.2.1){: external} で説明されているローカル・リファレンスのみをサポートしています。 つまり、文法は自己完結型である必要があります。
-   [セクション 2.2.3 Special Rules](https://www.w3.org/TR/speech-grammar/#S2.2.3){: external}。
-   [セクション 2.2.4 Referencing N-gram Documents (Informative)](https://www.w3.org/TR/speech-grammar/#S2.2.4){: external}。
-   [セクション 2.7 Language](https://www.w3.org/TR/speech-grammar/#S2.7){: external}。 サービスは言語の切り替えをサポートしていません。 サービスは文法ごとに 1 つの世界言語のみをサポートしています。

文法内の単語では UTF-8 エンコード方式が使用されている必要があります (ASCII は UTF-8 のサブセットです)。 他のエンコード方式を使用している場合は、文法のコンパイル時に問題が発生する可能性や、デコード時に予期せぬ結果が生じる可能性があります。 サービスでは、文法のヘッダーで指定されたエンコード方式は無視されます。
{: note}
