---

copyright:
  years: 2015, 2019
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

# 機密保護
{: #information-security}

{{site.data.keyword.IBM}} は、お客様やパートナーに、データ・プライバシー、セキュリティー、およびガバナンスに関する革新的なソリューションを提供することに努めています。
{: shortdesc}

お客様自身が欧州連合の一般データ保護規則を含む各種法令を遵守するために必要な措置を講ずるのはお客様の責任です。 お客様のビジネスに影響を及ぼす可能性のある関連法令の特定およびそれらの解釈、ならびにかかる関連法令を遵守するためにお客様が講ずるべき必要措置に関する助言は、お客様の責任により適格な弁護士から得るものとします。
{: important}

本書に記載の製品、サービス、および他の機能が、すべてのお客様の状況に適しているとは限らず、使用する際に制約を受ける場合があります。 {{site.data.keyword.IBM_notm}} は、法律、会計または監査に関する助言を提供することはしませんし、IBM のサービスまたは製品が、お客様のあらゆる法令遵守の裏付けとなる表明または保証もいたしません。

以下の場所で作成された {{site.data.keyword.cloud}} {{site.data.keyword.watson}} リソースの GDPR サポートを要請する必要がある場合の手順

-   EU 内の場合、[EU で作成された {{site.data.keyword.cloud_notm}} Watson リソースのサポートの要求 (Requesting support for {{site.data.keyword.cloud_notm}} Watson resources created in the European Union)](/docs/services/watson/getting-started-gdpr-sar.html#request-EU) を参照してください。
-   EU 外の場合、[EU 外でのリソースのサポートの要求 (Requesting support for resources outside the European Union)](/docs/services/watson/getting-started-gdpr-sar.html#request-non-EU) を参照してください。

## EU 一般データ保護規則 (GDPR)
{: #gdpr}

{{site.data.keyword.IBM_notm}} は、お客様やパートナーに、データ・プライバシー、セキュリティー、およびガバナンスに関する革新的なソリューションを提供して、GDPR  に対する準拠が完了するまでの過程を支援することに努めています。

GDPR に対する準備を整えるための {{site.data.keyword.IBM_notm}} 独自の過程と、準拠が完了するまでの過程をサポートする弊社の GDPR 機能とオファリングについて詳しくは、[ここ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://www.ibm.com/gdpr){: new_window} を参照してください。

## Speech to Text サービスでのデータのラベル付けと削除
{: #gdpr-speech-to-text}

{{site.data.keyword.speechtotextfull}} サービスでは、認識要求、カスタム言語モデル、およびカスタム音響モデルに関連付けられたすべてのデータを削除できます。データを削除するには、次の手順を実行します。

1.  `X-Watson-Metadata` ヘッダーを使用して、要求を通じてサービスに渡されるデータに顧客 ID を関連付けます ([顧客 ID の指定](#specify-customer-id)を参照してください)。
1.  `DELETE /v1/user_data` メソッドを使用して、指定した顧客 ID に関連付けられたすべてのデータを削除します ([データの削除](#delete-pi)を参照してください)。

デフォルトでは、データには顧客 ID は関連付けられていません。

試験およびベータの機能は、実稼働環境での使用を意図していないため、データのラベル付けおよび削除時に期待どおりに機能することは保証されません。データのラベル付けと削除を必要とするソリューションを実装する場合は、試験およびベータの機能を使用しないでください。
{: note}

### 顧客 ID の指定
{: #specify-customer-id}

顧客 ID をデータに関連付けるには、情報を渡すための要求に `X-Watson-Metadata` ヘッダーを追加します。このヘッダーの引数として `customer_id={id}` というストリングを指定します。次の例では、`POST /v1/recognize` 要求を通じて渡されるデータに `my_customer_ID` という顧客 ID を関連付けます。

```bash
curl -X POST -u "apikey:{apikey}"
--header "X-Watson-Metadata: customer_id=my_customer_ID"
--header "Content-Type: audio/wav"
--data-binary @audio.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

顧客 ID には、`;` (セミコロン) と `=` (等号) 以外の任意の文字を使用できます。顧客 ID には、ランダムなストリングや一般的なストリングを指定してください。E メール・アドレスや Twitter ID などの個人情報を構成するストリングは指定しないでください。要求ごとに異なる顧客 ID を指定できます。要求で指定する顧客 ID は、その要求で使用される資格情報を持つサービス・インスタンスに関連付けられるため、その ID に関連付けられたデータを削除するには、そのサービス・インスタンスの資格情報を使用する必要があります。

以下のメソッドで `X-Watson-Metadata` ヘッダーを使用します。

-   WebSocket 要求の場合:
    -   `/v1/recognize`

    要求の `x-watson-metadata` クエリー・パラメーターで顧客 ID を指定して接続を開始します。このクエリー・パラメーターの引数は URL エンコードする必要があります (例: `customer_id%3dmy_customer_ID`)。顧客 ID は、この接続を介して送信される認識要求を通じて渡されるすべてのデータに関連付けられます。
-   同期 HTTP 要求の場合:
    -   `POST /v1/recognize`

    顧客 ID は、個別の要求を通じて送信されるデータに関連付けられます。
-   非同期 HTTP 要求の場合:
    -   `POST /v1/register_callback`
    -   `POST /v1/recognitions`

    顧客 ID は、ホワイトリストに登録されたコールバック URL に関連付けられるか、個別の認識要求を通じて送信されるデータに関連付けられます。
-   コーパス、カスタム単語、または文法をカスタム言語モデルに追加するための要求の場合:
    -   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
    -   `POST /v1/customizations/{customization_id}/words`
    -   `PUT /v1/customizations/{customization_id}/words/{word_name}`
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`

    顧客 ID は、その要求によって追加または更新されるコーパス、カスタム単語、または文法に関連付けられます。
-   音声リソースをカスタム音響モデルに追加するための要求の場合:
    -   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

    顧客 ID は、その要求によって追加または更新される音声リソースに関連付けられます。

### データの削除
{: #delete-pi}

任意の顧客 ID に関連付けられたすべてのデータを削除するには、`DELETE /v1/user_data` メソッドを使用します。`customer_id={id}` というストリングをクエリー・パラメーターとして要求を通じて渡します。次の例では、`my_customer_ID` という顧客 ID のすべてのデータを削除します。

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/user_data?customer_id=my_customer_ID"
```
{: pre}

`/v1/user_data` メソッドは、これらのデータを追加したメソッドが何であるかにかかわらず、指定された顧客 ID に関連付けられたすべてのデータを削除します。顧客 ID に関連付けられたデータがない場合は、このメソッドを実行しても何も効果がありません。この顧客 ID をデータに関連付けるために使用されたのと同じサービス・インスタンスの資格情報を使用して、要求を発行する必要があります。
