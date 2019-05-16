---

copyright:
  years: 2019
lastupdated: "2019-03-19"

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

# 高可用性及災難回復
{: #ha-dr}

在任何 {{site.data.keyword.cloud_notm}} 位置（例如，達拉斯或華盛頓特區）內，{{site.data.keyword.speechtotextfull}} 服務都高度可用。不過，從影響整個位置的潛在災難回復需要規劃和準備。
{: shortdesc}

您負責瞭解服務的配置、自訂作業和使用情形。同時負責備妥以在新位置重建服務實例，以及在任何位置還原資料。

## 高可用性
{: #high-availability}

{{site.data.keyword.speechtotextshort}} 服務支援沒有單一失敗點的高可用性。此服務會透過 {{site.data.keyword.cloud_notm}} 所提供的多區域地區 (MZR) 特性方法，自動且明確地達到高可用性。

{{site.data.keyword.cloud_notm}} 會啟用未在單一位置內共用單一失敗點的多個區域。它也會提供地區內各個區域之間的自動載入平衡。

## 災難回復
{: #disaster-recovery}

如果 {{site.data.keyword.cloud_notm}} 位置遇到包括潛在資料流失的重大故障，則災難回復可能會變成問題。因為在各個位置都無法使用 MZR，如果位置變成無法使用，您必須等待 {{site.data.keyword.IBM_notm}} 將該位置重新帶回線上。如果失敗導致基礎資料服務受損，則您還必須等待 {{site.data.keyword.IBM_notm}} 還原那些資料服務。

如果發生災難性失效，{{site.data.keyword.IBM_notm}} 可能無法從資料庫備份回復資料。在此情況下，您需要還原資料，以將服務實例回復至其最新狀態。您可以將資料還原至相同或不同的位置。

您的災難回復方案包括知道、保留及準備還原在 {{site.data.keyword.cloud_notm}} 上維護的所有資料。這包括自訂語言模型、自訂聲學模型及非同步語音辨識要求中的資料。

從儲存的資料中重建自訂模型，尤其是自訂聲學模型，可能需要相當長的時間。在多個位置維護平行服務配置，可免除與災難回復相關聯的處理時間。
{: note}

### 自訂語言模型的災難回復
{: #disaster-recovery-language}

對於自訂語言模型，您需要維護並準備重建及還原自訂語言模型及其語料庫、文法和自訂字組。您還需要準備在其資料和字組資源上重新訓練自訂語言模型。

#### 備份自訂語言模型
{: #disaster-recovery-language-backup}

保留自訂語言模型的下列相關資訊：

-   所有自訂語言模型及其定義的清單。若要列出自訂模型的相關資訊，請執行下列動作：
    -   使用 `GET /v1/customizations` 方法，列出所有自訂模型的相關資訊。
    -   使用 `GET /v1/customizations/{customization_id}` 方法，列出所指定自訂模型的相關資訊。

    如需相關資訊，請參閱[列出自訂語言模型](/docs/services/speech-to-text/language-models.html#listModels-language)。
-   您新增至自訂語言模型的所有語料庫文字檔的副本。若要列出自訂模型的語料庫的相關資訊，請執行下列動作：
    -   使用 `GET /v1/customizations/{customization_id}/corpora` 方法，列出自訂模型的所有語料庫。
    -   使用 `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法，列出自訂模型的指定語料庫的相關資訊。

    如需相關資訊，請參閱[列出自訂語言模型的語料庫](/docs/services/speech-to-text/language-corpora.html#listCorpora)。
-   您新增至自訂語言模型的所有文法檔的副本。若要列出自訂模型的文法的相關資訊，請執行下列動作：
    -   使用 `GET /v1/customizations/{customization_id}/grammars` 方法，列出自訂模型的所有文法的相關資訊。
    -   使用 `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法，列出自訂模型的指定文法的相關資訊。

    如需相關資訊，請參閱[列出自訂語言模型的文法](/docs/services/speech-to-text/grammar-manage.html#listGrammars)。
-   您直接新增至自訂語言模型的所有自訂字組的相關資訊，包括其類似音定義及顯示為定義。若要列出自訂模型的未登錄詞彙 (OOV) 字組的相關資訊，請執行下列動作：
    -   使用 `GET /v1/customizations/{customization_id}/words` 方法，列出自訂模型中的字組的相關資訊。您可以使用 `word_type` 參數列出模型中的 `all` 字組、由 `user` 直接新增的字組、從 `corpora` 擷取的字組，或 `grammars` 所辨識的字組。
    -   使用 `GET /v1/customizations/{customization_id}/words/{word_name}` 方法，列出自訂模型中的指定字組的相關資訊。

    如需相關資訊，請參閱[列出自訂語言模型中的字組](/docs/services/speech-to-text/language-words.html#listWords)。

最佳作法是以您可以在失敗時用來重建自訂語言模型的格式保留此資訊。主動維護自訂模型及其資料的相關資訊，以及提前準備下節列出的呼叫，可讓您盡快回復。

#### 還原自訂語言模型
{: #disaster-recovery-language-restore}

如果您需要從災難回復，則可以使用備份資訊來重建自訂語言模型及其資料：

1.  若要重建自訂語言模型，請使用 `POST /v1/customizations` 方法。如需相關資訊，請參閱[建立自訂語言模型](/docs/services/speech-to-text/language-create.html#createModel-language)。
1.  若要將語料庫文字檔新增至自訂模型，請使用 `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` 方法。如需相關資訊，請參閱[將語料庫新增至自訂語言模型](/docs/services/speech-to-text/language-create.html#addCorpus)。
1.  若要將文法檔新增至自訂模型，請使用 `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` 方法。如需相關資訊，請參閱[將文法新增至自訂語言模型](/docs/services/speech-to-text/grammar-add.html#addGrammar)。
1.  若要將多個字組新增至自訂模型，請使用 `POST /v1/customizations/{customization_id}/words` 方法。若要將單一字組新增至自訂模型，請使用 `PUT /v1/customizations/{customization_id}/words/{word_name}` 方法。如需相關資訊，請參閱[將字組新增至自訂語言模型](/docs/services/speech-to-text/language-create.html#addWords)。
1.  若要在還原語料庫、文法及自訂字組之後訓練自訂模型，請使用 `POST /v1/customizations/{customization_id}/train` 方法。如需相關資訊，請參閱[訓練自訂語言模型](/docs/services/speech-to-text/language-create.html#trainModel-language)。

您用來新增語料庫、文法及字組的方法，與用來訓練自訂語言模型的方法為非同步。您必須監視要求，直到它們完成為止。

您可以用漸進方式新增資料並訓練自訂語言模型，而不要在訓練模型之前新增所有資料。例如，您可以在新增文法及個別字組之前，先新增語料庫，然後訓練您的模型。您也可以用漸進方式，在個別語料庫文字檔、文法檔及自訂字組群組上，新增及訓練您的模型。

### 自訂聲學模型的災難回復
{: #disaster-recovery-acoustic}

對於自訂聲學模型，您需要維護並準備重建及還原自訂聲學模型及其音訊資源。您還需要準備在其音訊資源上重新訓練自訂聲學模型。

#### 備份自訂聲學模型
{: #disaster-recovery-acoustic-backup}

保留自訂聲學模型的下列相關資訊：

-   所有自訂聲學模型及其定義的清單。若要列出自訂模型的相關資訊，請執行下列動作：
    -   使用 `GET /v1/acoustic_customizations` 方法，列出所有自訂模型的相關資訊。
    -   使用 `GET /v1/acoustic_customizations/{customization_id}` 方法，列出所指定自訂模型的相關資訊。

    如需相關資訊，請參閱[列出自訂聲學模型](/docs/services/speech-to-text/acoustic-models.html#listModels-acoustic)。
-   您新增至自訂聲學模型的所有音訊資源（包括個別音訊檔和保存檔）的副本。若要列出自訂模型的音訊資源的相關資訊，請執行下列動作：
    -   使用 `GET /v1/acoustic_customizations/{customization_id}/audio` 方法，列出自訂模型的所有音訊資源的相關資訊。
    -   使用 `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法，列出自訂模型的指定音訊資源的相關資訊。

    如需相關資訊，請參閱[列出自訂聲學模型的音訊資源](/docs/services/speech-to-text/acoustic-audio.html#listAudio)。

最佳作法是以您在失敗時用來重建自訂聲學模型的格式保留此資訊。主動維護自訂模型及其音訊資源的相關資訊，以及提前準備下節列出的呼叫，可讓您盡快回復。

#### 還原自訂聲學模型
{: #disaster-recovery-acoustic-restore}

如果您需要從災難回復，則可以使用備份資訊來重建自訂聲學模型及其資料：

1.  若要重建自訂聲學模型，請使用 `POST /v1/acoustic_customizations` 方法。如需相關資訊，請參閱[建立自訂聲學模型](/docs/services/speech-to-text/acoustic-create.html#createModel-acoustic)。
1.  若要將音訊資源新增至自訂模型，請使用 `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 方法。如需相關資訊，請參閱[將音訊新增至自訂聲學模型](/docs/services/speech-to-text/acoustic-create.html#addAudio)。
1.  若要在還原音訊資源之後訓練自訂模型，請使用 `POST /v1/acoustic_customizations/{customization_id}/train` 方法。如需相關資訊，請參閱[訓練自訂聲學模型](/docs/services/speech-to-text/acoustic-create.html#trainModel-acoustic)。

您用來新增音訊資源的方法，與用來訓練自訂聲學模型的方法為非同步。您必須監視要求，直到它們完成為止。

您可以用漸進方式新增音訊資源並訓練自訂聲學模型，而不要在訓練模型之前新增所有資料。例如，您可以一次新增一個音訊資源，或以群組新增音訊資源，在音訊資源子集上訓練您的模型，而不是一次在所有音訊上訓練模型。

### 非同步語音辨識工作的災難回復
{: #disaster-recovery-async}

對於使用非同步 HTTP 介面的語音辨識，您需要維護下列資訊：

-   您列入白名單中以與非同步介面搭配使用的所有回呼 URL。如果發生失敗，您可能需要使用 `POST /v1/register_callback` 方法來重新登錄 URL。如果 URL 已列入白名單，此方法會傳回適當的回應。
-   您提交至非同步介面以進行語音辨識的音訊檔的副本。如果在接收或擷取已完成的非同步工作的結果之前發生失敗，您需要使用 `POST /v1/recognitions` 方法，在還原服務時重新提交音訊檔。一旦有了已完成的非同步工作的結果，您就不再需要維護音訊檔。

如需相關資訊，請參閱[非同步 HTTP 介面](/docs/services/speech-to-text/async.html)。如同自訂模型的備份資料一樣，您可以主動保留此資訊，並準備好提前重新發出必要的要求。
