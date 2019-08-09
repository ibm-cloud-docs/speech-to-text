---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-06"

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

# コーパスおよびカスタム単語の処理
{: #corporaWords}

カスタム言語モデルに単語を取り込むには、コーパスまたは文法をモデルに追加するか、またはカスタム単語を直接追加します。
{: shortdesc}

-   **コーパス:** カスタム言語モデルに単語を取り込む手段として推奨されるのは、1 つ以上のコーパスをモデルに追加することです。 コーパスを追加すると、サービスがそのファイルを分析し、検出した新しい単語を自動的にカスタム・モデルに追加します。 コーパスをカスタム・モデルに追加することで、サービスはコンテキストに応じて分野に固有の単語を抽出でき、書き起こし結果の改善に役立ちます。 詳しくは、[コーパスの処理](#workingCorpora)を参照してください。
-   **文法:** カスタム・モデルに文法を追加して、その文法によって認識される単語または句に音声認識を限定できます。 モデルに文法を追加すると、コーパスの場合と同様、サービスによって検出された新しい単語が自動的にモデルに追加されます。 詳しくは、[カスタム言語モデルでの文法の使用](/docs/services/speech-to-text?topic=speech-to-text-grammars)を参照してください。
-   **個別の単語:** 個別のカスタム単語を直接モデルに追加することもできます。 サービスはコーパスまたは文法で検出した単語を追加する場合と同じように、単語をモデルに追加します。 単語を直接追加すると、複数の発音を指定し、単語の表示方法を示すことができます。 また、既存の単語を更新して、コーパスまたは文法から抽出された定義を変更したり、拡張したりできます。 詳しくは、[カスタム単語の処理](#workingWords)を参照してください。

単語の追加方法に関わらず、サービスはカスタム言語モデルに追加したすべての単語をモデルの単語リソースに格納します。

## 単語リソース
{: #wordsResource}

*単語リソース* には、コーパスや文法から追加したり、直接追加したりしたすべての単語が含まれます。 その目的は、サービスの基本語彙にまだ含まれていない単語の発音とつづりを定義することです。 この定義では、*未知 (OOV) 語* の書き起こし方法をサービスに認識させます。

単語リソースには、各 OOV 語に関する以下の情報が含まれます。 サービスは、コーパスおよび文法から抽出された単語の定義を作成します。 直接追加または変更した単語の特性を指定します。

-   `word`: コーパスまたは文法で検出されたか、直接追加した単語のつづり。
-   `sounds_like`: 単語の発音。 コーパスおよび文法から抽出された単語の場合、この値はサービスが言語ルールに基づいて判断する単語の発音を表します。 多くの場合、発音は `word` フィールドのつづりを反映します。

    `sounds_like` フィールドを使用して単語の発音を変更できます。 このフィールドを使用して、単語に複数の発音を指定することもできます。 詳しくは、[sounds_like フィールドの使用](#soundsLike)を参照してください。
-   `display_as`: サービスが書き起こしで使用する単語のつづり。 このフィールドは、単語の表示方法を示します。 ほとんどの場合、つづりは `word` フィールドの値に一致します。

    `display_as` フィールドを使用して、単語の異なるつづりを指定できます。 詳しくは、[display_as フィールドの使用](#displayAs)を参照してください。
-   `source`: 単語が単語リソースに追加された方法。 サービスがコーパスまたは文法から単語を抽出した場合、このフィールドにはそのリソースの名前がリストされます。 サービスは複数のリソースで同じ単語を検出する場合があるため、このフィールドには複数のコーパス名または文法名がリストされる可能性があります。 単語を直接追加または変更した場合、このフィールドには `user` というストリングが格納されます。

いずれかの方法でモデルの単語リソースを更新する場合、モデルをトレーニングして、書き起こし中に変更が反映されるようにする必要があります。 詳しくは、[カスタム言語モデルのトレーニング](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language)を参照してください。

## 必要なデータ量
{: #wordsResourceAmount}

効果的なカスタム言語モデルに必要なデータの量は、多くの要因の影響を受けます。 カスタム・モデルやアプリケーションに追加する必要がある単語の正確な数を示すことはできません。

ユース・ケースによっては、数単語を直接カスタム・モデルに追加するだけで、モデルの品質が向上する場合があります。 ただし、音声で使用されているコンテキストで単語を示すコーパスから OOV 語を追加すると、書き起こしの正確度を大幅に改善できる場合があります。 詳しくは、[コーパスの処理](#workingCorpora)を参照してください。

このサービスでは、カスタム言語モデルに追加できる単語数が次のように制限されます。

-   カスタム・モデルの単語リソースには、最大で 9 万語の OOV 語を追加できます。 この数には、すべてのソース (コーパス、文法、および直接追加した個々のカスタム単語) からの OOV 語が含まれます。
-   カスタム・モデルには、すべてのソースから最大で合計 1,000 万の単語を追加できます。この数値には、コーパスまたは文法に含まれる、OOV 語と既にサービスの基本語彙に含まれる単語の両方が含まれます。 コーパスの場合、サービスは追加の単語を使用して、OOV 語が発生するコンテキストを学習します。このため、コーパスは認識の正確度を改善するより効果的な手段となります。

大規模な単語リソースによって音声認識の待ち時間が長くなりますが、正確な影響を定量化または予測することは困難です。 効果的なカスタム・モデルの作成に必要なデータ量と同様に、大規模な単語リソースのパフォーマンスに対する影響はさまざまな要因によって異なります。 さまざまなデータ量でカスタム・モデルをテストして、モデルのパフォーマンスおよびデータを決定します。

## コーパスの処理
{: #workingCorpora}

コーパスをカスタム・モデルに追加するには、`POST /v1/customizations/{customization_id}/corpora/{corpus_name}` メソッドを使用します。 コーパスは、対象分野で使用される例文が含まれる、プレーン・テキスト・ファイルです。 次のサンプルは、医療分野の簡易コーパスを示しています。 コーパス・ファイルは、通常、これよりも長くなります。

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

音声認識は、音声を分析する統計アルゴリズムに依存します。 カスタム・モデルの単語は、サービスの基本語彙に含まれる単語だけでなく、そのモデルの他の単語とも競合関係にあります (音声のノイズや話者のアクセントなどの要因も、書き起こしの品質に影響を与えます)。

書き起こしの正確度は、単語がモデルでどのように定義されているか、そして話者がそれらの単語をどのように発音するかによって大きく左右されます。 サービスの正確度を向上させるために、コーパスを使用して、OOV 語がその分野でどのように使用されるかを示す例文をできるだけ多く提供してください。 コーパスで OOV 語を繰り返すと、カスタム言語モデルの品質が向上します。 コーパスで単語を複製する方法は、認識される音声でユーザーがその単語を言う方法によって異なります。 話者が特定の分野の単語を使用するコンテキストを表す文を多く追加すればするほど、サービスの認識の正確度が高くなります。

サービスは単純な単語マッチング・アルゴリズムを適用しません。 その書き起こしは、単語が使用されるコンテキストによって異なります。 コーパスの解析時に、サービスはカスタム・モデルのコーパスの文から n グラム (バイグラム、トライグラムなど) に関する情報を含めます。 この情報は、サービスがより高い正確度で音声を書き起こすのに役立ちます。また、この情報によって、コーパスのカスタム・モデルに関するトレーニングがカスタム単語のみに関するトレーニングよりも価値がある理由が説明されます。

例えば、会計士は Generally Accepted Accounting Principles (GAAP) と呼ばれる一連の共通の基準および手順に従います。 財務分野のカスタム・モデルを作成する場合、コンテキストで用語 GAAP を使用する文を提供します。 このような文は、サービスが「the gap between them is small」などの一般的な句と「GAAP provides guidelines for measuring and disclosing financial information」などの分野中心の句を区別するのに役立ちます。

一般に、コーパスの方が異なるコンテキストでの単語の使用に向いており、サービスによる単語の学習を向上させることができます。 ただし、ユーザーが数個のコンテキストのみで単語を話す場合、別のコンテキストでその単語を示してもカスタム・モデルの品質は改善されません。 話者は、そのようなコンテキストで単語を使用しません。 話者が同じ句を頻繁に使用する可能性がある場合は、コーパスでその句を繰り返すとモデルの品質が向上します。 (数語のカスタム単語を直接カスタム・モデルに追加するだけで、モデルの品質が向上する場合もあります。)

### コーパス・テキスト・ファイルの準備
{: #prepareCorpus}

コーパス・テキスト・ファイルを準備するには、以下のガイドラインに従ってください。

-   プレーン・テキスト・ファイルに非 ASCII 文字が含まれている場合は、UTF-8 でエンコードされたファイルを用意します。 このサービスはそのような文字を検出すると、UTF-8 エンコードされたものと想定します。

    コーパス・テキスト・ファイルの文字エンコードを確認してください。 サービスはテキスト・ファイルで検出したエンコードを保持します。 カスタム言語モデルで単語を処理する場合は、このエンコードを使用する必要があります。 詳しくは、[文字エンコード](#charEncoding)を参照してください。
    {: important}
-   コーパス内の単語には、一貫性のある大文字化を使用してください。 単語リソースでは大/小文字が区別されます。 大文字と小文字を混ぜて、大文字化は意図する場合にのみ使用します。
-   コーパスのそれぞれの文を 1 行に含め、各行を復帰で終了します。 複数の文を同じ行に含めると、正確度が低下する可能性があります。
-   個別の行で個人名を不連続ユニットとして追加します。 個別の行で名前の単語を個別のカスタム単語として追加したり、コーパスの同じ行に複数の名前を含めたりしないでください。 次のサンプルは、3 つの名前の認識の正確度を向上させる正しい方法を示しています。

    ```
    Gakuto Kutara
    Sebastian Leifson
    Malcolm Ingersol
    ```
    {: codeblock}

    可能な場合は、`Doctor Sebastian Leifson` や `President Malcolm Ingersol` などの追加のコンテキスト情報を含めます。 すべての単語と同様に、可能な場合は名前を異なるコンテキストで複数回複製すると、認識の正確度を向上させることができます。
-   タイプミスに注意してください。 サービスでは、タイプミスは新しい単語と見なされます。 モデルをトレーニングする前に修正しない限り、サービスによりタイプミスがモデルの語彙に追加されます。 *Garbage in, garbage out!* (信頼できないデータからの結果は信頼できない) という格言を忘れないでください。

文が多いほど、正確度が高まります。 ただし、サービスはモデルのすべてのソースからの単語数の合計を最大 1,000 万語、OOV 語を 9 万語に制限します。

### コーパス・ファイル追加時の動作
{: #parseCorpus}

コーパス・ファイルを追加すると、サービスがその内容を分析します。 サービスは、検出した新しい (OOV) 語を抽出し、各 OOV 語をカスタム・モデルの単語リソースに追加します。 コンテキストから最大限の意味を引き出すために、このサービスはコーパス・ファイルから読み取ったデータをトークン化して解析します。 以降のセクションでは、各サポート対象言語のコーパス・ファイルをサービスが解析する方法を説明します。

#### 英語、フランス語、ドイツ語、スペイン語、およびブラジル・ポルトガル語の解析
{: #corpusLanguages}

次の説明は、米国英語と英国英語、フランス語、ドイツ語、スペイン語、およびブラジル・ポルトガル語に適用されます。

-   各数字は、次のように対応する単語に変換されます。
    -   *英語の場合、*`500` は `five hundred` になり、`0.15` は `zero point fifteen` になります。
    -   *フランス語の場合、* `500` は `cinq cents` になり、`0,15` は <code>z&eacute;ro virgule quinze</code> になります。
    -   *ドイツ語の場合、*`500` は <code>f&uuml;nfhundert</code> になり、`0,15` は <code>null punkt f&uuml;nfzehn</code> になります。
    -   *スペイン語の場合、*`500` は `quinientos` になり、`0,15` は `cero coma quince` になります。
    -   *ブラジル・ポルトガル語の場合、*`500` は `quinhentos` になり、`0,15` は `zero ponto quinze` になります。
-   特定の記号を含むトークンが、次のように意味のあるストリング表現に変換されます。
    -   `$` (ドル記号) と数字は、次のように変換されます。
        -   *英語の場合、*`$100` は `one hundred dollars` になります。
        -   *フランス語の場合、*`$100` は `cent dollars` になります。
        -   *ドイツ語の場合、*`$100` および `100$` は `einhundert dollar` になります。
        -   *スペイン語の場合、*`$100` および `100$` は <code>cien d&oacute;lares</code> (または方言が `es-LA` の場合は `cien pesos`) になります。
        -   *ブラジル・ポルトガル語の場合、*`$100` および `100$` は <code>cem d&oacute;lares</code> になります。
    -   <code>&euro;</code> (ユーロ記号) と数字は、次のように変換されます。
        -   *英語の場合、*<code>&euro;100</code> は `one hundred euros` になります。
        -   *フランス語の場合、*<code>&euro;100</code> は `cent euros` になります。
        -   *ドイツ語の場合、*<code>&euro;100</code> および <code>100&euro;</code> は `einhundert euro` になります。
        -   *スペイン語の場合、*<code>&euro;100</code> および <code>100&euro;</code> は `cien euros` になります。
        -   *ブラジル・ポルトガル語の場合、*<code>&euro;100</code> および <code>100&euro;</code> は `cem euros` になります。
    -   `%` (パーセント記号) とその後に続く数字は、次のように変換されます。
        -   *英語の場合、*`100%` は `one hundred percent` になります。
        -   *フランス語の場合、*`100%` は `cent pour cent` になります。
        -   *ドイツ語の場合、*`100%` は `einhundert prozent` になります。
        -   *スペイン語の場合、*`100%` は `cien por ciento` になります。
        -   *ブラジル・ポルトガル語の場合、*`100%` は `cem por cento` になります。

    このリストは、すべてを網羅したものではありません。 サービスは、他の文字についても、必要に応じて同様の調整を行います。
-   英数字以外の句読点と特殊文字は、コンテキストに応じて処理されます。 例えば、サービスにより後に数字が続かない `$` (ドル記号) や <code>&euro;</code> (ユーロ記号) が削除されます。 処理はコンテキストに依存し、サポート対象言語間で整合性があります。
-   `( )` (小括弧)、`< >` (不等号括弧)、`[ ]` (大括弧)、または `{ }` (中括弧) で囲まれた句は無視されます。

#### 日本語の解析
{: #corpusLanguages-jaJP}

-   すべての文字が全角文字に変換されます。
-   各数字はそれに対応する単語に変換されます。例えば、`500` は <code>&#20116;&#30334;</code> になり、`0.15` は <code>&#12295;&#12539;&#19968;&#20116;</code> になります。
-   記号を含むトークンは対応するストリングに変換されません。例えば、`100%` は <code>&#30334;&#65285;</code> になります。
-   句読点は自動的に削除されません。 {{site.data.keyword.IBM_notm}} では、アプリケーションが口述筆記ベースではなく書き起こしベースである場合は句読点を削除することを強くお勧めします。

#### 韓国語の解析
{: #corpusLanguages-koKR}

-   各数字は対応する単語に変換されます。例えば、<code>10</code> は <code>&#49901;</code> になります。
-   句読点と特殊文字 (`- ( ) * : . , ' "`) が削除されます。 ただし、他の言語で削除されたすべての句読点と特殊文字が韓国語でも削除されるとは限りません。次に例を示します。
    -   ピリオド (`.`) 記号は、入力の行の最後にある場合にのみ削除されます。
    -   ティルド (`~`) 記号は削除されません。
    -   <code>&#8230;</code> (3 点リーダーまたは省略符号) などのユニコード・ワイド文字記号は削除されず、処理されます。

    一般に、{{site.data.keyword.IBM_notm}} は句読点、特殊文字、およびユニコード・ワイド文字を削除してからコーパス・ファイルを処理することをお勧めします。
-   `( )` (小括弧)、`< >` (不等号括弧)、`[ ]` (大括弧)、または `{ }` (中括弧) で囲まれた句は削除も無視もされません。
-   特定の記号を含むトークンが、次のように意味のあるストリング表現に変換されます。
    -   `24%` は <code>&#51060;&#49901;&#49324;&#54140;&#49468;&#53944;</code> になります。
    -   `$10` は <code>&#49901;&#45804;&#47084;</code> になります。

    このリストは、すべてを網羅したものではありません。 サービスは、他の文字についても、必要に応じて同様の調整を行います。
-   ラテン (英語) 文字で構成される句またはハングル文字とラテン文字が混在する句の場合、サービスはその句に対して、コーパス・ファイルに表示されるとおりに OOV 語と作成します。 また、ハングルの書き起こしに基づいて同音異字発音を作成します。
   - OOV 語 `London` には、同音異字 <code>&#47088;&#45912;</code> が付与されます。
   - OOV 語 <code>IBM&#54856;&#54168;&#51060;&#51648;</code> には、同音異字 <code>&#50500;&#51060;&#32;&#48708;&#32;&#50656;&#32;&#54856;&#54168;&#51060;&#51648;</code>が付与されます。

## カスタム単語の処理
{: #workingWords}

`POST /v1/customizations/{customization_id}/words` メソッドおよび `PUT /v1/customizations/{customization_id}/words/{word_name}` メソッドを使用して、カスタム・モデルの単語リソースに新しい単語を追加できます。 これらのメソッドを使用して、単語リソースの単語を変更または拡張することもできます。

例えば、このメソッドを使用して、コーパスから単語を追加する際に発生したタイプミスやその他の誤りを修正する必要がある場合があります。 また、既存の単語に同音異字の定義を追加しなければならない場合もあります。 既存の単語を変更すると、指定する新しいデータによって、単語リソース内のその単語の既存の定義が上書きされます。 単語を追加する際のルールは、既存の単語を変更する場合にも適用されます。

### 文字エンコード
{: #charEncoding}

通常、ほとんどのカスタム単語をコーパスから追加します。 コーパスのテキスト・ファイルの文字エンコードを確認してください。 サービスはテキスト・ファイルで検出したエンコードを保持します。

カスタム言語モデルで個別の単語を処理する場合は、このエンコードを使用する必要があります。 `GET` メソッド、`PUT` メソッド、または `DELETE /v1/customizations/{customization_id}/words/{word_name}` メソッドを使用して単語を指定する際は、単語に非 ASCII 文字が含まれている場合、URL で渡す `word_name` を URL エンコードする必要があります。

例えば、以下の表は 2 つの異なるエンコード (ASCII および UTF-8) での同じ文字の表示を示しています。 URL で ASCII 文字を `z` として渡すことができます。UTF-8 文字は `%EF%BD%9A` として渡す必要があります。

<table style="width:75%">
  <caption>表 1. 文字エンコードの例</caption>
  <tr>
    <th style="width:15%; text-align:center">文字</th>
    <th style="width:40%; text-align:center">エンコード</th>
    <th style="width:45%; text-align:center">値</th>
  </tr>
  <tr>
    <td style="text-align:center">
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
    <td style="text-align:center">
      <code>&#xff5a;</code>
    </td>
    <td style="text-align:center">
      UTF-8 16 進数
    </td>
    <td style="text-align:center">
      `0xEF 0xBD 0x9A` (`efbd9a`)
    </td>
  </tr>
</table>

### sounds_like フィールドの使用
{: #soundsLike}

`sounds_like` フィールドは、話者による単語の発音方法を指定します。 デフォルトでは、サービスは自動的に、このフィールドに単語のつづりを取り込みます。 難しい発音の単語や別の方法でも発音できる単語には、最大 5 つの代替発音を指定できます。 以下を行う場合に、このフィールドの使用を検討します。

-   *頭字語に別の発音を指定する。* 例えば、頭字語 `NCAA` はつづりどおりに発音される場合と、口語的に *N. C. double A* と発音される場合があります。以下の例では、単語 `NCAA` にこれら両方の同音異字発音を追加しています。

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

-   *外来語を処理する。* 例えば、フランス語の単語 <code>gar&ccedil;on</code> には、英語にはない文字が含まれています。 <code>&ccedil;</code> を `s` で置き換えた `gaarson` という同音異字を指定することで、英語の話者がこの単語をどのように発音するかをこのサービスに認識させることがきます。

音声認識では、統計アルゴリズムを使用して音声を分析するため、単語を追加した場合に、その単語が完全な正確度でトランスコードされるとは限りません。 単語を追加する際は、その単語がどのように発音される可能性があるかを検討します。 `sounds_like` フィールドを使用して、単語がどのように発話される可能性があるかを表す、さまざまな発音を指定してください。 以降のセクションでは、同音異字発音を指定する際の言語に固有のガイドラインを示します。

#### 英語 (米国および英国) のガイドライン
{: #wordLanguages-enUS-enGB}

*米国英語と英国英語の両方のガイドライン:*

-   英字 `a-z` および `A-Z` を使用します。
-   発音するのが難しい単語には、英語で発音できる実際の単語または造語を使用します。例えば、単語 `shuchesnie` の場合は `Sczcesny` とします。
-   英字以外の文字は、相当する英字で置き換えます。例えば、<code>&ccedil;</code> は `s` で置き換え、<code>&ntilde;</code> は `ny` で置き換えます。
-   アクセント記号付きの文字は、アクセント記号なしの文字で置き換えます。例えば、<code>&agrave;</code> は `a` で置き換え、<code>&egrave;</code>は `e` で置き換えます。
-   複数の単語をスペースで区切って含めることはできますが、このサービスでは、スペースを除く最大 40 文字の上限が強制されます。

*米国英語のみのガイドライン:*

-   単一の文字を発音するには、文字の後にピリオドを続けます。 ピリオドの後に別の文字が続く場合は、ピリオドと次の文字の間にスペースを使用してください。 例えば、`N.C.A.A.` *ではなく* `N. C. A. A.` を使用します。
-   数字にはつづりを使用します。例えば、`75` は `seventy-five` とします。

*英国英語のみのガイドライン:*

-   英国英語では同音異字発音にピリオドやダッシュを使用することは**できません**。
-   単一の文字を発音するには、文字の後にスペースを続けます。 例えば、`N. C. A. A.`、`N.C.A.A.`、`NCAA` *ではなく*、`N C A A` を使用します。
-   数字には、ダッシュなしのつづりを使用します。例えば、`75` は `seventy five` とします。

#### フランス語、ドイツ語、スペイン語、およびブラジル・ポルトガル語のガイドライン
{: #wordLanguages-esES-frFR}

-   同音異字発音でダッシュを使用することは**できません**。
-   言語で有効な英字 `a-z` および `A-Z` (有効なアクセント記号付きの文字を含む) を使用します。
-   単一の文字を発音するには、文字の後にピリオドを続けます。 ピリオドの後に別の文字が続く場合は、ピリオドと次の文字の間にスペースを使用してください。 例えば、`N.C.A.A.` *ではなく* `N. C. A. A.` を使用します。
-   発音するのが難しい単語には、その言語で発音できる実際の単語または造語を使用します。
-   ダッシュなしの数値のつづりを使用します。例えば、`75` には以下を使用します。
    -   *フランス語:* `soixante quinze`
    -   *ドイツ語:* <code>f&uuml;nfundsiebzig</code>
    -   *スペイン語:* `setenta y cinco`
    -   *ブラジル・ポルトガル語:* `setenta e cinco`
-   複数の単語をスペースで区切って含めることはできますが、このサービスでは、スペースを除く最大 40 文字の上限が強制されます。

#### 日本語のガイドライン
{: #wordLanguages-jaJP}

-   長音記号 <code>&#8213;</code> (**&#38263;&#38899;) を使用して、全角のカタカナのみを使用します。 半角文字を使用しないでください。
-   **&#25303;&#38899;は、以下の音節でのみ使用します。

    <code>&#12452;&#12455;</code>、<code>&#12454;&#12451;</code>、<code>&#12454;&#12455;</code>、<code>&#12454;&#12457;</code>、<code>&#12461;&#12451;</code>、<code>&#12461;&#12515;</code>、<code>&#12461;&#12517;</code>、<code>&#12461;&#12519;</code>、<code>&#12462;&#12515;</code>、<code>&#12462;&#12517;</code>、<code>&#12462;&#12519;</code>、<code>&#12463;&#12449;</code>、<code>&#12463;&#12451;</code>、<code>&#12463;&#12455;</code>、<code>&#12463;&#12457;</code>、<br/>
<code>&#12464;&#12449;</code>、<code>&#12464;&#12457;</code>、<code>&#12471;&#12451;</code>、<code>&#12471;&#12455;</code>、<code>&#12471;&#12515;</code>、<code>&#12471;&#12517;</code>、<code>&#12471;&#12519;</code>、<code>&#12472;&#12451;</code>、<code>&#12472;&#12455;</code>、<code>&#12472;&#12515;</code>、<code>&#12472;&#12517;</code>、<code>&#12472;&#12519;</code>、<code>&#12473;&#12451;</code>、<code>&#12474;&#12451;</code>、<code>&#12481;&#12455;</code>、<br/>
<code>&#12481;&#12515;</code>、<code>&#12481;&#12517;</code>、<code>&#12481;&#12519;</code>、<code>&#12482;&#12455;</code>、<code>&#12482;&#12515;</code>、<code>&#12482;&#12517;</code>、<code>&#12482;&#12519;</code>、<code>&#12484;&#12449;</code>、<code>&#12484;&#12451;</code>、<code>&#12484;&#12455;</code>、<code>&#12484;&#12457;</code>、<code>&#12486;&#12451;</code>、<code>&#12486;&#12517;</code>、<code>&#12487;&#12451;</code>、<code>&#12487;&#12515;</code>、<br/>
<code>&#12487;&#12517;</code>、<code>&#12487;&#12519;</code>、<code>&#12488;&#12453;</code>、<code>&#12489;&#12453;</code>、<code>&#12491;&#12455;</code>、<code>&#12491;&#12515;</code>、<code>&#12491;&#12517;</code>、<code>&#12491;&#12519;</code>、<code>&#12498;&#12515;</code>、<code>&#12498;&#12517;</code>、<code>&#12498;&#12519;</code>、<code>&#12499;&#12515;</code>、<code>&#12499;&#12517;</code>、<code>&#12499;&#12519;</code>、<code>&#12500;&#12451;</code>、<br/>
<code>&#12500;&#12515;</code>、<code>&#12500;&#12517;</code>、<code>&#12500;&#12519;</code>、<code>&#12501;&#12449;</code>、<code>&#12501;&#12451;</code>、<code>&#12501;&#12455;</code>、<code>&#12501;&#12457;</code>、<code>&#12501;&#12517;</code>、<code>&#12511;&#12515;</code>、<code>&#12511;&#12517;</code>、<code>&#12511;&#12519;</code>、<code>&#12522;&#12451;</code>、<code>&#12522;&#12455;</code>、<code>&#12522;&#12515;</code>、<code>&#12522;&#12517;</code>、<br/>
<code>&#12522;&#12519;</code>、<code>&#12532;&#12449;</code>、<code>&#12532;&#12451;</code>、<code>&#12532;&#12455;</code>、<code>&#12532;&#12457;</code>、<code>&#12532;&#12517;</code>

-   つまる音 (**&#20419;&#38899;) の後には以下の音節のみを使用します。

    <code>&#12496;</code>、<code>&#12499;</code>、<code>&#12502;</code>、<code>&#12505;</code>、<code>&#12508;</code>、<code>&#12481;</code>、<code>&#12481;&#12455;</code>、<code>&#12481;&#12515;</code>、<code>&#12481;&#12517;</code>、<code>&#12481;&#12519;</code>、<code>&#12480;</code>、<code>&#12487;</code>、<code>&#12487;&#12451;</code>、<code>&#12489;</code>、<code>&#12489;&#12453;</code>、<code>&#12501;</code>、<br/>
<code>&#12501;&#12449;</code>、<code>&#12501;&#12451;</code>、<code>&#12501;&#12455;</code>、<code>&#12501;&#12457;</code>、<code>&#12460;</code>、<code>&#12462;</code>、<code>&#12464;</code>、<code>&#12466;</code>、<code>&#12468;</code>、<code>&#12495;</code>、<code>&#12498;</code>、<code>&#12504;</code>、<code>&#12507;</code>、<code>&#12472;</code>、<code>&#12472;&#12455;</code>、<code>&#12472;&#12515;</code>、<br/>
<code>&#12472;&#12517;</code>、<code>&#12472;&#12519;</code>、<code>&#12459;</code>、<code>&#12461;</code>、<code>&#12463;</code>、<code>&#12465;</code>、<code>&#12467;</code>、<code>&#12461;&#12515;</code>、<code>&#12461;&#12517;</code>、<code>&#12461;&#12519;</code>、<code>&#12497;</code>、<code>&#12500;</code>、<code>&#12503;</code>、<code>&#12506;</code>、<code>&#12509;</code>、<code>&#12500;&#12515;</code>、<br/>
<code>&#12500;&#12517;</code>、<code>&#12500;&#12519;</code>、<code>&#12469;</code>、<code>&#12473;</code>、<code>&#12475;</code>、<code>&#12477;</code>、<code>&#12471;</code>、<code>&#12471;&#12455;</code>、<code>&#12471;&#12515;</code>、<code>&#12471;&#12517;</code>、<code>&#12471;&#12519;</code>、<code>&#12479;</code>、<code>&#12486;</code>、<code>&#12488;</code>、<code>&#12484;</code>、<code>&#12470;</code>、<br/>
<code>&#12474;</code>、<code>&#12476;</code>、<code>&#12478;</code>

-   単語の先頭文字として<code>&#12531;</code>を使用しないでください。 例えば、無効な<code>&#12531;&#12540;&#12488;</code>ではなく、<code>&#12454;&#12540;&#12531;&#12488;</code>を使用します。
-   数多くの複合語が*接頭部 + 名詞* または*名詞 + 接尾部*で構成されています。 サービスの基本語彙には、頻繁に使用されるほとんどの複合語 (<code>&#x9577;&#x96FB;&#x8A71;</code>や<code>&#x53E4;&#x65B0;&#x805E;</code>など) が含まれていますが、頻繁に使用されない複合語は含まれていません。 コーパスに一般的に複合語が含まれている場合は、カスタマイズの最初のステップとして、複合語を 1 語として追加します。 例えば、<code>&#x53E4;&#x925B;&#x7B46;</code>は通常の日本語のテキストでは一般的ではありません。この単語を頻繁に使用する場合は、カスタム・モデルに追加して書き起こしの正確度を高めます。
-   末尾につまる音を使用しないでください。

#### 韓国語のガイドライン
{: #wordLanguages-koKR}

-   韓国語のハングル文字、記号、および音節を使用します。
-   ラテン語 (英語) の英字 `a-z` および `A-Z` を使用することもできます。
-   上記のセットに含まれていない文字や記号は使用しないでください。

### display_as フィールドの使用
{: #displayAs}

`display_as` フィールドは、書き起こしでの単語の表示方法を指定します。 このフィールドは、単語の本来のつづりとは異なるストリングをサービスで表示させる場合に使用します。 例えば、単語 `hhonors` を、発音が `hilton honors` または `h honors` のいずれであるかに関わらず、`HHonors` として表示するように指定できます。

```bash
curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

別の例として、単語 `IBM` を <code>IBM&trade;</code> として表示するように指定することもできます。

<pre><code class="language-bash">curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"I. B. M.\"], \"display_as\":\"IBM&#8482;\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IBM"</code></pre>

#### スマート・フォーマット設定および数値編集との対話
{: #displaySmart}

認識要求で `smart_formatting` パラメーターまたは `redaction` パラメーターを使用する場合は、サービスがスマート・フォーマット設定および編集を単語に適用してから、単語の `display_as` フィールドを考慮することに注意してください。 この場合、これらの機能がカスタム単語の表示方法に干渉しないことを確認するために、結果を試してみる必要がある可能性があります。 また、その結果を考慮してカスタム単語を追加しなければならない可能性があります。

例えば、`display_as` フィールドを `one` に設定して、カスタム単語 `one` を追加するとします。 スマート・フォーマット設定によって、単語 `one` は数字の `1` に変更されるため、display-as の値は適用されません。 この問題を回避するには、カスタム単語として数字 `1` を追加して、この単語に同じ `display_as` フィールドを適用します。

これらの機能の処理について詳しくは、[スマート・フォーマット設定](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting)および[数値の編集](/docs/services/speech-to-text?topic=speech-to-text-output#redaction)を参照してください。

### カスタム単語の追加または変更時の動作
{: #parseWord}

カスタム単語の追加または変更の要求に対するサービスの応答は、指定した値およびその単語がサービスの基本語彙に存在するかどうかによって異なります。

<table>
  <caption>表 1. 異なるフィールドを使用したカスタム単語の追加</caption>
  <tr>
    <th style="width:20%; text-align:center; vertical-align:bottom"><code>sounds_like</code> フィールドの値</th>
    <th style="width:20%; text-align:center; vertical-align:bottom"><code>display_as</code> フィールドの値</th>
    <th style="text-align:left; vertical-align:bottom">応答</th>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      指定なし
    </td>
    <td style="text-align:center; vertical-align:top">
      指定なし
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>該当する単語がサービスの基本語彙に
          存在しない場合、</em>サービスは <code>sounds_like</code> フィールドには
          その単語の発音を設定します。 また、
          <code>display_as</code> フィールドには
          <code>word</code> フィールドの値を設定します。
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>該当する単語がサービスの基本語彙に存在する場合、</em>
          サービスは、<code>sounds_like</code> フィールドおよび
          <code>display_as</code> フィールドを空のままにします。 これらのフィールドが空になるのは、
          該当する単語がサービスの基本語彙に存在する場合のみです。 この単語は、
          モデルの単語リソースに存在していても悪影響はありませんが、
          不必要なものです。
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      指定
    </td>
    <td style="text-align:center; vertical-align:top">
      指定なし
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em><code>sounds_like</code> フィールドが有効な場合、</em>
          サービスは <code>display_as</code> フィールドに
          <code>word</code> フィールドの値を設定します。
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em><code>sounds_like</code> フィールドが無効な場合:</em>
          <ul style="margin-left:20px; padding:0px">
            <li style="margin:10px 0px; line-height:120%">
              <code>POST
                /v1/customizations/{customization_id}/words</code>
              メソッドが、モデルの単語リソース内でその単語に <code>error</code> フィールドを
              追加します。
            </li>
            <li style="margin:10px 0px; line-height:120%">
              <code>PUT
                /v1/customizations/{customization_id}/words/{word_name}</code>
              メソッドは 400 応答コードで失敗し、エラー・メッセージが返されます。 サービスは
              その単語を単語リソースに追加しません。
            </li>
          </ul>
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      指定なし
    </td>
    <td style="text-align:center; vertical-align:top">
      指定
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>該当する単語がサービスの基本語彙に存在しない場合、</em>
          サービスは <code>sounds_like</code> フィールドには
          その単語の発音を設定し、<code>display_as</code> フィールドに
          指定された値はそのまま維持します。
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>該当する単語がサービスの基本語彙に存在する場合</em>、サービスは <code>sounds_like</code> フィールドを空のままにし、<code>display_as</code> フィールドに指定された値はそのまま維持します。
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      指定
    </td>
    <td style="text-align:center; vertical-align:top">
      指定
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em><code>sounds_like</code> フィールドが有効な場合、</em>
          サービスは <code>sounds_like</code> フィールドおよび <code>display_as</code>
          フィールドを指定された値に設定します。
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em><code>sounds_like</code> フィールドが無効な場合、</em>
          サービスは <code>sounds_like</code> フィールドが指定されている一方、
          <code>display_as</code> フィールドが指定されていない場合と
          同様に対応します。
        </li>
      </ul>
    </td>
  </tr>
</table>

## 単語リソースの検証
{: #validateModel}

特にコーパスをカスタム言語モデルに追加したり、複数のカスタム単語と一度に追加したりする場合は、モデルの単語リソース内の OOV 単語を確認します。

-   *誤字やその他の誤りがないか調べてください。* 特に大きなコーパスを追加する場合に誤りが生じやすくなります。 コーパス (または文法) ファイル内にタイプミスがあると、それらが新しい単語としてモデルの単語リソースに追加されるという予期せぬ結果をもたらします。これは、コーパス・ファイル内に誤った形式の HTML タグが残っている場合にも言えることです。
-   *同音異字発音を確認してください。* サービスは、OOV 語の同音異字発音を自動的に生成します。 これらの発音のほとんどは十分に正確なものです。 ただし、通常とは異なるつづりの単語や発音するのが難しい単語、さらに頭字語や技術用語については、発音の正確度を確認することが推奨されます。

カスタム・モデルの単語を確認し、必要に応じて修正するには、単語の単語リソースへの追加方法に関わらず、以下のメソッドを使用します。

-   `GET /v1/customizations/{customization_id}/words` メソッドを使用してカスタム・モデルのすべての単語をリストするか、`GET /v1/customizations/{customization_id}/words/{word_name}` メソッドを使用して個々の単語を照会します。 詳しくは、[カスタム言語モデルの単語のリスト](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords)を参照してください。
-   `POST /v1/customizations/{customization_id}/words` メソッドまたは `PUT /v1/customizations/{customization_id}/words/{word_name}` メソッドを使用して、カスタム・モデルの単語を変更して誤りを修正するか、同音異字または表示形式の値を追加します。 詳しくは、[カスタム単語の処理](#workingWords)を参照してください。
-   誤って (例えば、コーパス内のタイプミスやその他の誤りによって) 挿入された無関係な単語を削除するには、`DELETE /v1/customizations/{customization_id}/words/{word_name}` メソッドを使用します。 詳しくは、[カスタム言語モデルからの単語の削除](/docs/services/speech-to-text?topic=speech-to-text-manageWords#deleteWord)を参照してください。
    -   単語がコーパスから抽出された場合、コーパス・テキスト・ファイルを更新してエラーを修正してから、`POST /v1/customizations/{customization_id}/corpora/{corpus_name}` メソッドの `allow_overwrite` パラメーターを使用してファイルを再ロードすることもできます。 詳しくは、[カスタム言語モデルへのコーパスの追加](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addCorpus)を参照してください。
    -   単語が文法から抽出された場合、文法ファイルを更新してエラーを修正してから、`POST /v1/customizations/{customization_id}/grammars/{grammar_name}` メソッドの `allow_overwrite` パラメーターを使用してファイルを再ロードすることもできます。 詳しくは、[カスタム言語モデルへの文法の追加](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar)を参照してください。
