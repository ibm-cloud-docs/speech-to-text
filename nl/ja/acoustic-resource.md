---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

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

# 音声リソースの処理
{: #audioResources}

カスタム音響モデルには、個々の音声ファイル、または複数の音声ファイルを含むアーカイブ・ファイルを追加できます。推奨される音声リソースの追加方法は、アーカイブ・ファイルを追加する方法です。1 つのアーカイブ・ファイルを作成して追加する操作は、複数の音声ファイルを個別に追加するよりも大幅に効率的です。
{: shortdesc}

## 音声リソースの追加
{: #addAudioResource}

カスタム音響モデルにどちらのタイプの音声リソースを追加する場合でも、`POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` メソッドを使用します。音声リソースを要求の本体として渡し、次のパラメーターを含めます。

-   `customization_id` パス・パラメーター。モデルのカスタマイズ ID を指定します。
-   `audio_name` パス・パラメーター。音声リソースの名前を指定します。
    -   カスタム・モデルの言語に一致し、リソースの内容を反映したローカライズ名を使用します。
    -   名前の最大文字数は 128 文字です。
    -   名前には、スペース、`/` (スラッシュ)、または `\` (円記号) を含めないでください。
    -   カスタム・モデルに既に追加されている音声リソースの名前は使用しないでください。

モデルの音声リソースを更新したら、書き起こしでその変更内容が反映されるようにするために、モデルをトレーニングする必要があります。詳しくは、[カスタム音響モデルのトレーニング](/docs/services/speech-to-text/acoustic-create.html#trainModel-acoustic)を参照してください。

## 音声ファイルの追加
{: #addAudioType}

個々の音声ファイルをカスタム音響モデルに追加するには、`Content-Type` ヘッダーに音声のフォーマット (MIME タイプ) を指定します。認識要求で使用できるフォーマットの音声を追加できます。`rate`、`channels`、および `endianness` パラメーターを必要とするフォーマットを指定するときには、これらのパラメーターを指定します。サポートされる音声フォーマットについて詳しくは、[音声フォーマット](/docs/services/speech-to-text/audio-formats.html)を参照してください。

音声リソースでは、音声フォーマットの `application/octet-stream` の指定はサポートされていません。
{: note}

[カスタム音響モデルへの音声の追加](/docs/services/speech-to-text/acoustic-create.html#addAudio)にある以下の例では、`audio/wav` ファイルが追加されます。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @audio1.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
```
{: pre}

## アーカイブ・ファイルの追加
{: #addArchiveType}

音声をカスタム音響モデルに追加する場合は、複数の音声ファイルを含むアーカイブ・ファイルを追加する方法が推奨されます。以下のタイプのアーカイブ・ファイルを追加するには、`Content-Type` 要求ヘッダーにアーカイブのタイプを指定します。

-   **.zip** ファイル (`application/zip` を指定)
-   **.tar.gz** ファイル (`application/gzip` を指定)

また、追加するファイルのフォーマットによっては、`Contained-Content-Type` ヘッダーを指定する必要もあります。

-   `audio/alaw`、`audio/basic`、`audio/l16`、または `audio/mulaw` タイプの音声ファイルの場合、`Contained-Content-Type` ヘッダーを使用して音声ファイルのフォーマットを指定する必要があります。必要に応じて、`rate`、`channels`、および `endianness` パラメーターを指定します。この場合、アーカイブ・ファイルに含まれているすべての音声ファイルの音声フォーマットが同一である必要があります。
-   その他のすべてのタイプの音声ファイルの場合、`Contained-Content-Type` ヘッダーは省略できます。この場合、アーカイブ・ファイルに含まれている音声ファイルに、上記にないフォーマットが含まれていることがあります。同一のフォーマットである必要はありません。

音声タイプのリソースを追加するときには `Contained-Content-Type` ヘッダーを使用しないでください。
{: note}

アーカイブ・タイプのリソースに含まれている音声ファイルの名前は、`audio_name` パラメーターと同じ命名上の制約に従う必要があります。また、カスタム・モデルに既に追加されている音声ファイルの名前を、アーカイブ・タイプのリソースの一部として使用しないでください。

[カスタム音響モデルへの音声の追加](/docs/services/speech-to-text/acoustic-create.html#addAudio)にある以下の例では、サンプリング・レートが 16 kHz でフォーマットが `audio/l16` の音声ファイルを含む `application/zip` ファイルが追加されます。

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/zip"
--header "Contained-Content-Type: audio/l16;rate=16000"
--data-binary @audio2.zip
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
```
{: pre}

## 音声リソースの追加に関するガイドライン
{: #audioGuidelines}

カスタム音響モデルを使用した結果として期待される認識正確度の改善の度合いは、さまざまな要因に応じて異なります。これらの要因には、カスタム音響モデルに含まれる音声データの量や、書き起こしの対象の音声とそのデータがどの程度類似しているかなどがあります。改善の度合いには、カスタム音響モデルのトレーニングで対応するカスタム言語モデルが使用されているかどうかも影響します。

音声リソースをカスタム音響モデルに追加する際には、以下のガイドラインに従ってください。

-   カスタム音響モデルに追加できる音声の長さは、10 分から 200 時間までです。音声は無音ではなく、発話が含まれている必要があります。

    追加する音声の量を決定する際には、音声の品質によって違いが出ます。モデルの音声が、認識対象の音声の特性をよく反映したものであるほど、音声認識のカスタム・モデルの品質は向上します。音声の品質が良好な場合、音声を追加すると書き起こしの正確度が改善される可能性があります。ただし、5 時間から 10 時間分の高品質音声を追加すると、正確度に大きな違いが出る可能性があります。
-   追加する音声リソースは 100 MB 以下にしてください。音声タイプのリソースとアーカイブ・タイプのリソースの最大サイズは 100 MB に制限されています。

    1 つのリソースで追加できる音声の量を最大化するために、圧縮可能な音声フォーマットの利用を検討してください。詳しくは、[データ制限および圧縮](/docs/services/speech-to-text/audio-formats.html#limits)を参照してください。
-   書き起こす予定の音声の音響チャネル状態を反映する音声コンテンツを追加します。例えば、走行中の自動車による背景雑音が入っている音声を処理するアプリケーションの場合は、同じタイプのデータを使用してカスタム・モデルを作成します。
-   音声ファイルのサンプリング・レートが、カスタム音響モデルの基本モデルのサンプリング・レートに一致することを確認してください。
    -   広帯域モデルの場合、サンプリング・レートは 16 kHz (16,000 サンプル/秒) 以上でなければなりません。
    -   狭帯域モデルの場合、サンプリング・レートは 8 kHz (8000 サンプル/秒) でなければなりません。

    音声のサンプリング・レートが最小必須サンプリング・レートよりも高い場合、サービスは音声を適切なレートにダウンサンプリングします。音声のサンプリング・レートが最小必須サンプリング・レートよりも低い場合、サービスはその音声ファイルを `invalid` としてマークします。アーカイブ・ファイル内に無効な音声ファイルが含まれている場合、サービスはアーカイブ全体を無効とみなします。
-   以下の状況では、カスタム音響モデルと共に使用するカスタム言語モデルを作成します。
    -   音声の長さが 1 時間未満の場合、最適な結果を得るため、音声の書き起こしをベースにカスタム言語モデルを作成します。
    -   音声が分野固有であり、サービスの基本語彙にない分野固有の用語が含まれている場合、言語モデルをカスタマイズして、サービスの基本語彙を拡張します。音響モデルのカスタマイズだけでは、書き起こし中にこれらの単語は生成できません。

    詳しくは、[カスタム音響モデルとカスタム言語モデルの併用](/docs/services/speech-to-text/acoustic-both.html)を参照してください。
