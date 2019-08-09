---

copyright:
  years: 2019
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

# 高可用性および災害復旧
{: #ha-dr}

{{site.data.keyword.speechtotextfull}} サービスは、どの {{site.data.keyword.cloud_notm}} ロケーション (ダラスやワシントン DC など) 内でも高可用性を実現します。 ただし、ロケーション全体に被害をもたらす起こり得る災害から復旧するには、計画と準備が必要です。
{: shortdesc}

ユーザーは、サービスの構成、カスタマイズ、および使用法を理解する必要があります。 また、新しいロケーションでサービスのインスタンスを再作成する準備、および任意のロケーションでデータを復元する準備をしておく必要もあります。

## 高可用性
{: #high-availability}

{{site.data.keyword.speechtotextshort}} サービスは、単一障害点のない高可用性をサポートしています。 このサービスは、{{site.data.keyword.cloud_notm}} によって提供されるマルチ・ゾーン・リージョン (MZR) 機能を使用して高可用性を自動的かつ透過的に実現します。

{{site.data.keyword.cloud_notm}} は、単一ロケーション内の単一障害点を共有していない複数のゾーンを使用可能にします。 また、単一リージョン内の複数のゾーン間の自動ロード・バランシングも実現します。

## 災害復旧
{: #disaster-recovery}

災害復旧が問題となり得るのは、{{site.data.keyword.cloud_notm}} ロケーションでデータ損失を伴う可能性のある大きな障害が発生した場合です。 MZR を複数のロケーションにまたがって使用することはできないため、いずれかのロケーションが使用不能になった場合は、{{site.data.keyword.IBM_notm}} によってそのロケーションがオンライン状態に復旧されるまで待つ必要があります。 基盤のデータ・サービスがその障害によって損なわれている場合は、{{site.data.keyword.IBM_notm}} によってそれらのデータ・サービスが復元されるまで待つ必要もあります。

壊滅的な障害が発生した場合は、{{site.data.keyword.IBM_notm}} 側でデータベース・バックアップからデータをリカバリーできない可能性があります。 この場合は、ユーザー側でデータを復元して、ご使用のサービス・インスタンスを直近の状態に戻す必要があります。 データの復元先は、同じロケーションでも異なるロケーションでもかまいません。

災害復旧計画の内容は、{{site.data.keyword.cloud_notm}} 上で保持されているすべてのデータを把握して保存すること、およびこれらのデータを復元する準備をしておくことです。 これらのデータに含まれるのは、カスタム言語モデルのデータ、カスタム音響モデルのデータ、および非同期音声認識要求のデータです。

保存されたデータを使用してカスタム・モデル (特にカスタム音響モデル) を再作成するには、多大な時間がかかることがあります。 複数のロケーションに並列的なサービス構成を保持しておくと、災害復旧の所要時間を解消できます。
{: note}

### カスタム言語モデルの災害復旧
{: #disaster-recovery-language}

カスタム言語モデルについては、カスタム言語モデルとそれらのコーパス、文法、およびカスタム単語を保持する必要があるとともに、これらを再作成および復元する準備をしておく必要があります。 また、カスタム言語モデルをそれらのデータと単語リソースに基づいてリトレーニングする準備もしておく必要があります。

#### カスタム言語モデルのバックアップ
{: #disaster-recovery-language-backup}

カスタム言語モデルに関する以下の情報を保存してください。

-   すべてのカスタム言語モデルとそれらの定義のリスト。 カスタム・モデルに関する情報をリストするには、以下のようにします。
    -   `GET /v1/customizations` メソッドを使用して、すべてのカスタム・モデルに関する情報をリストします。
    -   `GET /v1/customizations/{customization_id}` メソッドを使用して、指定したカスタム・モデルに関する情報をリストします。

    詳しくは、[カスタム言語モデルのリスト](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language)を参照してください。
-   カスタム言語モデルに追加するすべてのコーパス・テキスト・ファイルのコピー。 カスタム・モデルのコーパスに関する情報をリストするには、以下のようにします。
    -   `GET /v1/customizations/{customization_id}/corpora` メソッドを使用して、カスタム・モデルのすべてのコーパスをリストします。
    -   `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` メソッドを使用して、カスタム・モデルの指定したコーパスに関する情報をリストします。

    詳しくは、[カスタム言語モデルのコーパスのリスト](/docs/services/speech-to-text?topic=speech-to-text-manageCorpora#listCorpora)を参照してください。
-   カスタム言語モデルに追加するすべての文法ファイルのコピー。 カスタム・モデルの文法に関する情報をリストするには、以下のようにします。
    -   `GET /v1/customizations/{customization_id}/grammars` メソッドを使用して、カスタム・モデルのすべての文法に関する情報をリストします。
    -   `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` メソッドを使用して、カスタム・モデルの指定した文法に関する情報をリストします。

    詳しくは、[カスタム言語モデルの文法のリスト](/docs/services/speech-to-text?topic=speech-to-text-manageGrammars#listGrammars)を参照してください。
-   カスタム言語モデルに直接追加するすべてのカスタム単語に関する情報 (これらの単語の同音異字定義と表示形式定義を含む)。 カスタム・モデルの未知 (OOV) 語に関する情報をリストするには、以下のようにします。
    -   `GET /v1/customizations/{customization_id}/words` メソッドを使用して、カスタム・モデル内の単語に関する情報をリストします。 `word_type` パラメーターを使用して、リストする単語のタイプを指定できます。`all` の場合は、モデル内のすべての単語がリストされて、`user` の場合は、該当ユーザーによって直接追加された単語がリストされて、`corpora` の場合は、コーパスから抽出された単語がリストされて、`grammars` の場合は、文法によって認識される単語がリストされます。
    -   `GET /v1/customizations/{customization_id}/words/{word_name}` メソッドを使用して、カスタム・モデル内の指定した単語に関する情報をリストします。

    詳しくは、[カスタム言語モデルの単語のリスト](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords)を参照してください。

これらの情報は、障害が発生した場合にカスタム言語モデルを再作成するために使用できるフォーマットで保存することが最適な方法です。 カスタム・モデルとそれらのデータに関する情報を積極的に保持して、次のセクションでリストしている呼び出しを事前に準備しておくことで、できる限り迅速な復旧が可能になります。

#### カスタム言語モデルの復元
{: #disaster-recovery-language-restore}

災害から復旧する必要がある場合は、バックアップ情報を使用してカスタム言語モデルとそれらのデータを再作成できます。

1.  カスタム言語モデルを再作成するには、`POST /v1/customizations` メソッドを使用します。 詳しくは、[カスタム言語モデルの作成](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language)を参照してください。
1.  コーパス・テキスト・ファイルをカスタム・モデルに追加するには、`POST /v1/customizations/{customization_id}/corpora/{corpus_name}` メソッドを使用します。 詳しくは、[カスタム言語モデルへのコーパスの追加](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addCorpus)を参照してください。
1.  文法ファイルをカスタム・モデルに追加するには、`POST /v1/customizations/{customization_id}/grammars/{grammar_name}` メソッドを使用します。 詳しくは、[カスタム言語モデルへの文法の追加](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar)を参照してください。
1.  複数の単語をカスタム・モデルに追加するには、`POST /v1/customizations/{customization_id}/words` メソッドを使用します。 単一の単語をカスタム・モデルに追加するには、`PUT /v1/customizations/{customization_id}/words/{word_name}` メソッドを使用します。 詳しくは、[カスタム言語モデルへの単語の追加](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addWords)を参照してください。
1.  コーパス、文法、およびカスタム単語の復元後にカスタム・モデルをトレーニングするには、`POST /v1/customizations/{customization_id}/train` メソッドを使用します。 詳しくは、[カスタム言語モデルのトレーニング](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language)を参照してください。

コーパス、文法、および単語を追加するために使用するメソッドと、カスタム言語モデルをトレーニングするために使用するメソッドは、非同期メソッドです。 したがって、これらの要求を完了時点までモニターする必要があります。

すべてのデータを追加してからモデルをトレーニングする代わりに、データを段階的に追加してカスタム言語モデルをトレーニングできます。 例えば、コーパスを追加してからモデルをトレーニングした後に、文法と個別の単語を追加できます。 また、個別のコーパス・テキスト・ファイル、文法ファイル、およびカスタム単語のグループを段階的に追加して、これらに基づいてモデルをトレーニングすることもできます。

### カスタム音響モデルの災害復旧
{: #disaster-recovery-acoustic}

カスタム音響モデルについては、カスタム音響モデルとそれらの音声リソースを保持する必要があるとともに、これらを再作成および復元する準備をしておく必要があります。 また、カスタム音響モデルをそれらの音声リソースに基づいてリトレーニングする準備もしておく必要があります。

#### カスタム音響モデルのバックアップ
{: #disaster-recovery-acoustic-backup}

カスタム音響モデルに関する以下の情報を保存してください。

-   すべてのカスタム音響モデルとそれらの定義のリスト。 カスタム・モデルに関する情報をリストするには、以下のようにします。
    -   `GET /v1/acoustic_customizations` メソッドを使用して、すべてのカスタム・モデルに関する情報をリストします。
    -   `GET /v1/acoustic_customizations/{customization_id}` メソッドを使用して、指定したカスタム・モデルに関する情報をリストします。

    詳しくは、[カスタム音響モデルのリスト](/docs/services/speech-to-text?topic=speech-to-text-manageAcousticModels#listModels-acoustic)を参照してください。
-   カスタム音響モデルに追加するすべての音声リソース (個別の音声ファイルとアーカイブ・ファイルの両方) のコピー。 カスタム・モデルの音声リソースに関する情報をリストするには、以下のようにします。
    -   `GET /v1/acoustic_customizations/{customization_id}/audio` メソッドを使用して、カスタム・モデルのすべての音声リソースに関する情報をリストします。
    -   `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` メソッドを使用して、カスタム・モデルの指定した音声リソースに関する情報をリストします。

    詳しくは、[カスタム音響モデルの音声リソースのリスト](/docs/services/speech-to-text?topic=speech-to-text-manageAudio#listAudio)を参照してください。

これらの情報は、障害が発生した場合にカスタム音響モデルを再作成するために使用できるフォーマットで保存することが最適な方法です。 カスタム・モデルとそれらの音声リソースに関する情報を積極的に保持して、次のセクションでリストしている呼び出しを事前に準備しておくことで、できる限り迅速な復旧が可能になります。

#### カスタム音響モデルの復元
{: #disaster-recovery-acoustic-restore}

災害から復旧する必要がある場合は、バックアップ情報を使用してカスタム音響モデルとそれらのデータを再作成できます。

1.  カスタム音響モデルを再作成するには、`POST /v1/acoustic_customizations` メソッドを使用します。 詳しくは、[カスタム音響モデルの作成](/docs/services/speech-to-text?topic=speech-to-text-acoustic#createModel-acoustic)を参照してください。
1.  カスタム・モデルに音声リソースを追加するには、`POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` メソッドを使用します。 詳しくは、[カスタム音響モデルへの音声の追加](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio)を参照してください。
1.  音声リソースの復元後にカスタム・モデルをトレーニングするには、`POST /v1/acoustic_customizations/{customization_id}/train` メソッドを使用します。 詳しくは、[カスタム音響モデルのトレーニング](/docs/services/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic)を参照してください。

音声リソースを追加するために使用するメソッドと、カスタム音響モデルをトレーニングするために使用するメソッドは、非同期メソッドです。 したがって、これらの要求を完了時点までモニターする必要があります。

すべてのデータを追加してからモデルをトレーニングする代わりに、音声リソースを段階的に追加してカスタム音響モデルをトレーニングできます。 例えば、音声リソースを 1 つずつまたはグループ単位で追加して、それらの音声リソースのサブセットに基づいてモデルをトレーニングできます (同時にすべての音声に基づいてモデルをトレーニングするのではなく)。

### 非同期音声認識ジョブの災害復旧
{: #disaster-recovery-async}

非同期 HTTP インターフェースを使用して音声認識を行うためには、以下の情報を保持する必要があります。

-   非同期インターフェースで使用するためにホワイトリストに登録するすべてのコールバック URL。 障害が発生した場合は、必要に応じて `POST /v1/register_callback` メソッドを使用してこれらの URL を再登録してください。 URL が既にホワイトリストに登録されている場合は、このメソッドは適切な応答を返します。
-   音声認識のために非同期インターフェースに送信する音声ファイルのコピー。 完了した非同期ジョブの結果を受信または取得する前に障害が発生した場合は、サービスが復元されたときに、`POST /v1/recognitions` メソッドを使用して音声ファイルを再送信する必要があります。 完了した非同期ジョブの結果が得られたら、音声ファイルを保持する必要はなくなります。

詳しくは、[非同期 HTTP インターフェース](/docs/services/speech-to-text?topic=speech-to-text-async)を参照してください。 カスタム・モデルのバックアップ・データと同様に、この情報を積極的に保存して、必要な要求を再発行する準備を事前にしておくことができます。
