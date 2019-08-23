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

# 高可用性和灾难恢复
{: #ha-dr}

{{site.data.keyword.speechtotextfull}} 服务在任何 {{site.data.keyword.cloud_notm}} 位置（例如，达拉斯或华盛顿）都具有高可用性。但是，要从影响整个位置的潜在灾难中恢复，需要进行规划和准备。
{: shortdesc}

了解服务的配置、定制和使用情况是您的责任。您还应负责准备好在新位置重新创建服务实例，并在任何位置复原数据。

## 高可用性
{: #high-availability}

{{site.data.keyword.speechtotextshort}} 服务支持高可用性，没有单点故障。服务通过 {{site.data.keyword.cloud_notm}} 提供的多专区区域 (MZR) 功能，自动、透明地实现高可用性。

{{site.data.keyword.cloud_notm}} 支持在单个位置中不共享单点故障的多个专区。它还提供区域内各专区之间的自动负载均衡。

## 灾难恢复
{: #disaster-recovery}

如果 {{site.data.keyword.cloud_notm}} 位置遇到重大故障，包括潜在的数据丢失，那么灾难恢复可能会成为问题。由于 MZR 不能跨位置使用，因此您必须等待 {{site.data.keyword.IBM_notm}} 将不可用的位置恢复为联机状态。如果底层数据服务受到故障影响，那么还必须等待 {{site.data.keyword.IBM_notm}} 复原这些数据服务。

对于灾难性故障，{{site.data.keyword.IBM_notm}} 可能无法通过数据库备份恢复数据。在这种情况下，您需要复原数据，以将服务实例恢复为其最新状态。您可以将数据复原到同一位置，也可以复原到其他位置。

您的灾难恢复计划应包括了解、保留和准备复原在 {{site.data.keyword.cloud_notm}} 上保留的所有数据。这包括来自定制语言模型、定制声学模型和异步语音识别请求的数据。

通过保存的数据重新创建定制模型（尤其是定制声学模型）可能需要大量时间。在多个位置并行保留服务配置可以避免与灾难恢复关联的周转时间。
{: note}

### 定制语言模型的灾难恢复
{: #disaster-recovery-language}

对于定制语言模型，您需要保留并准备重新创建和复原定制语言模型及其语料库、语法和定制词。您还需要准备基于定制语言模型的数据和词资源来重新训练这些模型。

#### 备份定制语言模型
{: #disaster-recovery-language-backup}

保留以下有关定制语言模型的信息：

-   所有定制语言模型及其定义的列表。要列出有关定制模型的信息，请使用以下方法：
    -   使用 `GET /v1/customizations` 方法来列出有关所有定制模型的信息。
    -   使用 `GET /v1/customizations/{customization_id}` 方法来列出有关指定定制模型的信息。

    有关更多信息，请参阅[列出定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language)。
-   添加到定制语言模型的所有语料库文本文件的副本。要列出有关定制模型的语料库的信息，请使用以下方法：
    -   使用 `GET /v1/customizations/{customization_id}/corpora` 方法来列出定制模型的所有语料库。
    -   使用 `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法来列出有关定制模型的指定语料库的信息。

    有关更多信息，请参阅[列出定制语言模型的语料库](/docs/services/speech-to-text?topic=speech-to-text-manageCorpora#listCorpora)。
-   添加到定制语言模型的所有语法文件的副本。要列出有关定制模型的语法的信息，请使用以下方法：
    -   使用 `GET /v1/customizations/{customization_id}/grammars` 方法来列出有关定制模型的所有语法的信息。
    -   使用 `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法来列出有关定制模型的指定语法的信息。

有关更多信息，请参阅[列出定制语言模型的语法](/docs/services/speech-to-text?topic=speech-to-text-manageGrammars#listGrammars)。
-   有关您直接添加到定制语言模型的所有定制词的信息，包括这些词发音相似的读法和显示方式定义。要列出有关定制模型的未登录 (OOV) 词的信息，请使用以下方法：
    -   使用 `GET /v1/customizations/{customization_id}/words` 方法来列出有关定制模型中词的信息。可以使用 `word_type` 参数来列出模型中的所有词 (`all`)、用户直接添加的词 (`user`)、从语料库中抽取的词 (`corpora`) 或通过语法识别到的词 (`grammars`)。
    -   使用 `GET /v1/customizations/{customization_id}/words/{word_name}` 方法来列出有关定制模型中指定词的信息。

    有关更多信息，请参阅[列出定制语言模型中的词](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords)。

保留这些信息时，最好采用发生故障时可用于重新创建定制语言模型的格式。通过主动保留有关定制模型及其数据的信息，并提前准备好以下部分中列出的调用，您可以尽快恢复。

#### 复原定制语言模型
{: #disaster-recovery-language-restore}

如果需要从灾难中恢复，可以使用备份信息来重新创建定制语言模型及其数据：

1.  要重新创建定制语言模型，请使用 `POST /v1/customizations` 方法。有关更多信息，请参阅[创建定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language)。
1.  要向定制模型添加语料库文本文件，请使用 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法。有关更多信息，请参阅[向定制语言模型添加语料库](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addCorpus)。
1.  要向定制模型添加语法文件，请使用 `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法。有关更多信息，请参阅[向定制语言模型添加语法](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar)。
1.  要向定制模型添加多个词，请使用 `POST /v1/customizations/{customization_id}/words` 方法。要向定制模型添加单个词，请使用 `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法。有关更多信息，请参阅[向定制语言模型添加词](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addWords)。
1.  要在复原语料库、语法和定制词后对定制模型进行训练，请使用 `POST /v1/customizations/{customization_id}/train` 方法。有关更多信息，请参阅[训练定制语言模型](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language)。

用于添加语料库、语法和词的方法以及训练定制语言模型的方法是异步执行的。您需要监视这些请求，直到它们完成。

可以通过递增方式添加数据并训练定制语言模型，而不是在训练模型之前添加所有数据。例如，可以添加语料库，接着训练模型，然后再添加语法和各个词。您还可以通过递增方式添加各个语料库文本文件、语法文件和多组定制词，并基于这些内容来训练模型。

### 定制声学模型的灾难恢复
{: #disaster-recovery-acoustic}

对于定制声学模型，您需要保留并准备重新创建和复原定制声学模型及其音频资源。您还需要准备基于定制声学模型的音频资源来重新训练这些模型。

#### 备份定制声学模型
{: #disaster-recovery-acoustic-backup}

保留以下有关定制声学模型的信息：

-   所有定制声学模型及其定义的列表。要列出有关定制模型的信息，请使用以下方法：
    -   使用 `GET /v1/acoustic_customizations` 方法来列出有关所有定制模型的信息。
    -   使用 `GET /v1/acoustic_customizations/{customization_id}` 方法来列出有关指定定制模型的信息。

    有关更多信息，请参阅[列出定制声学模型](/docs/services/speech-to-text?topic=speech-to-text-manageAcousticModels#listModels-acoustic)。
-   添加到定制声学模型的所有音频资源（各个音频文件和归档文件）的副本。要列出有关定制模型的音频资源的信息，请使用以下方法：
    -   使用 `GET /v1/acoustic_customizations/{customization_id}/audio` 方法来列出有关定制模型的所有音频资源的信息。
    -   使用 `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法来列出有关定制模型的指定音频资源的信息。

    有关更多信息，请参阅[列出定制声学模型的音频资源](/docs/services/speech-to-text?topic=speech-to-text-manageAudio#listAudio)。

保留这些信息时，最好采用发生故障时可用于重新创建定制声学模型的格式。通过主动保留有关定制模型及其音频资源的信息，并提前准备好以下部分中列出的调用，您可以尽快恢复。

#### 复原定制声学模型
{: #disaster-recovery-acoustic-restore}

如果需要从灾难中恢复，可以使用备份信息来重新创建定制声学模型及其数据：

1.  要重新创建定制声学模型，请使用 `POST /v1/acoustic_customizations` 方法。有关更多信息，请参阅[创建定制声学模型](/docs/services/speech-to-text?topic=speech-to-text-acoustic#createModel-acoustic)。
1.  要向定制模型添加音频资源，请使用 `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法。有关更多信息，请参阅[向定制声学模型添加音频](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio)。
1.  要在复原音频资源后对定制模型进行训练，请使用 `POST /v1/acoustic_customizations/{customization_id}/train` 方法。有关更多信息，请参阅[训练定制声学模型](/docs/services/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic)。

用于添加音频资源的方法以及训练定制声学模型的方法是异步执行的。您需要监视这些请求，直到它们完成。

可以通过递增方式添加音频资源并训练定制声学模型，而不是在训练模型之前添加所有数据。例如，可以一次添加一个音频资源或分组添加音频资源，从而基于音频资源子集来训练模型，而不是一次基于所有音频对模型进行训练。

### 异步语音识别作业的灾难恢复
{: #disaster-recovery-async}

对于使用异步 HTTP 接口进行的语音识别，需要保留以下信息：

-   列入白名单以用于异步接口的所有回调 URL。如果发生故障，您可能需要使用 `POST /v1/register_callback` 方法来重新注册这些 URL。如果 URL 已列入白名单，那么此方法会返回相应的响应。
-   提交给异步接口进行语音识别的音频文件的副本。如果在接收或检索完成的异步作业的结果之前发生故障，那么在复原服务时，需要使用 `POST /v1/recognitions` 方法重新提交音频文件。已具有完成的异步作业的结果后，就不再需要保留音频文件。

有关更多信息，请参阅[异步 HTTP 接口](/docs/services/speech-to-text?topic=speech-to-text-async)。与定制模型的备份数据一样，您可以主动保留这些信息，并提前准备好重新发出必要的请求。
