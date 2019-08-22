---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-10"

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

# 參數摘要
{: #summary}

以下是所有可用於語音辨識的參數摘要。如需 {{site.data.keyword.speechtotextshort}} 服務的所有方法的相關資訊，請參閱 [API 參考資料](https://{DomainName}/apidocs/speech-to-text){: external}。
{: shortdesc}

當您提出語音辨識要求時，請考量下列基本需求：

-   方法名稱會區分大小寫。
-   HTTP 要求標頭不區分大小寫。
-   HTTP 和 WebSocket 查詢參數會區分大小寫。
-   JSON 欄位名稱會區分大小寫。
-   所有 JSON 回應內容都使用 UTF-8 字集。

另請考量下列服務特定需求：

-   您只需要指定輸入音訊。所有其他參數都是選用參數。
-   如果您指定無效的查詢參數或 JSON 欄位作為輸入的一部分，回應會包含 `warnings` 欄位，說明無效的引數。無論有任何警告，要求都會成功。

## access_token
{: #summary-access-token}

如果您使用 Identity and Access Management (IAM) 鑑別，這是您用來建立與 WebSocket 介面之已鑑別連線的選用性 IAM 存取記號。如需相關資訊，請參閱[開啟連線](/docs/services/speech-to-text?topic=speech-to-text-websockets#WSopen)。

<table>
  <caption>表 1. access_token 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 連線要求的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      不支援
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      不支援
    </td>
  </tr>
</table>

## acoustic_customization_id
{: #summary-acoustic-customization-id}

自訂聲學模型的選用性自訂作業 ID，該自訂聲學模型已針對環境和說話者的聲音特徵而調整。依預設，不使用任何自訂模型。如需相關資訊，請參閱[自訂模型](/docs/services/speech-to-text?topic=speech-to-text-input#custom-input)。

<table>
  <caption>表 2. acoustic_customization_id 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      所有語言皆為測試版
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 連線要求的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## audio_metrics
{: #summary-audio-metrics}

選用性的布林值，指出服務是否傳回有關輸入音訊信號特徵的度量值。依預設 (`false`)，服務不會傳回音訊度量值。如需相關資訊，請參閱[音訊度量值](/docs/services/speech-to-text?topic=speech-to-text-metrics#audio_metrics)。

<table>
  <caption>表 3. audio_metrics 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## base_model_version
{: #summary-base-model-version}

基礎模型的選用版本。此參數主要是與已針對新基礎模型而更新的自訂模型搭配使用，但在沒有自訂模型的情況下也可以使用它。預設值取決於參數是否搭配自訂模型使用。如需相關資訊，請參閱[基礎模型版本](/docs/services/speech-to-text?topic=speech-to-text-input#version)。

<table>
  <caption>表 4. base_model_version 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 連線要求的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## Content-Type
{: #summary-content-type}

選用的音訊格式（MIME 類型），用來指定您傳遞給服務的音訊資料格式。服務可以自動偵測大部分音訊的格式，因此參數對於大部分格式而言是選用性的。對於 `audio/alaw`、`audio/basic`、`audio/l16` 及 `audio/mulaw` 等格式而言是必要。如需相關資訊，請參閱[音訊格式](/docs/services/speech-to-text?topic=speech-to-text-audio-formats)。

<table>
  <caption>表 5. Content-Type 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的 <code>content-type</code> 參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的要求標頭
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的要求標頭
    </td>
  </tr>
</table>

## customization_weight
{: #summary-customization-weight}

介於 0.0 到 1.0 之間的選用性倍精準數，指出服務給與來自自訂語言模型之字詞，比上來自基礎詞彙之字組的相對權重。預設值是 0.3，除非在訓練自訂語言模型時指定了不同的權重。如需相關資訊，請參閱[自訂模型](/docs/services/speech-to-text?topic=speech-to-text-input#custom-input)。

<table>
  <caption>表 6. customization_weight 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對美式英文、英式英文、巴西葡萄牙文、法文、德文、日文、韓文和西班牙文正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## grammar_name
{: #summary-grammar-name}

識別要用於語音辨識之文法的選用性字串。服務只能辨識文法所定義的字串。您必須同時指定文法的名稱，以及文法定義對象之自訂語言模型的自訂作業 ID。如需相關資訊，請參閱[文法](/docs/services/speech-to-text?topic=speech-to-text-input#grammars-input)。

<table>
  <caption>表 7. grammar_name 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對美式英文、英式英文、巴西葡萄牙文、法文、德文、日文、韓文和西班牙文為測試版
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## inactivity_timeout
{: #summary-inactivity-timeout}

選用性的整數，用來指定服務閒置逾時的秒數。閒置表示服務在串流音訊中沒有偵測到任何語音。預設值是 30 秒。請使用 `-1` 來指出無限。如需相關資訊，請參閱[閒置逾時](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-inactivity)。

<table>
  <caption>表 8. inactivity_timeout 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## interim_results
{: #summary-interim-results}

選用性的布林值，用來指示服務傳回可能在最終文字記錄之前變更的中間假設。依預設 (`false`)，不會傳回過渡期間結果。如需相關資訊，請參閱[過渡期間結果](/docs/services/speech-to-text?topic=speech-to-text-output#interim)。

<table>
  <caption>表 9. interim_results 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      不支援
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      不支援
    </td>
  </tr>
</table>

## keywords
{: #summary-keywords}

選用性的關鍵字字串陣列，服務會在輸入音訊中辨識這些關鍵字字串。依預設，不會執行關鍵字辨識。如需相關資訊，請參閱[關鍵字辨識](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting)。

<table>
  <caption>表 10. keywords 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## keywords_threshold
{: #summary-keywords-threshold}

介於 0.0 到 1.0 之間的選用性倍精準數，指出正面關鍵字相符項的臨界值下限。依預設，不會執行關鍵字辨識。如需相關資訊，請參閱[關鍵字辨識](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting)。

<table>
  <caption>表 11. keywords_threshold 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## language_customization_id
{: #summary-language-customization-id}

自訂語言模型的選用性自訂作業 ID，該自訂語言模型包含來自您領域的專用術語。依預設，不使用任何自訂模型。如需相關資訊，請參閱[自訂模型](/docs/services/speech-to-text?topic=speech-to-text-input#custom-input)。

<table>
  <caption>表 12. language_customization_id 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對美式英文、英式英文、巴西葡萄牙文、法文、德文、日文、韓文和西班牙文正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 連線要求的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## max_alternatives
{: #summary-max-alternatives}

選用性的整數，用來指定服務傳回的替代假設數上限。依預設，服務會傳回單一最終假設。如需相關資訊，請參閱[替代項目數上限](/docs/services/speech-to-text?topic=speech-to-text-output#max_alternatives)。

<table>
  <caption>表 13. max_alternatives 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## model
{: #summary-model}

選用性模型，用來指定音訊說話所用的語言，以及它的取樣率：寬頻或窄頻。依預設，會使用 `en-US_BroadbandModel`。如需相關資訊，請參閱[語言和模型](/docs/services/speech-to-text?topic=speech-to-text-models)。

<table>
  <caption>表 14. model 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 連線要求的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## processing_metrics
{: #summary-processing-metrics}

選用性的布林值，指出服務是否傳回有關其輸入音訊處理的度量值。依預設 (`false`)，服務不會傳回處理度量值。如需相關資訊，請參閱[處理度量值](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics)。

<table>
  <caption>表 15. processing_metrics 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      不支援
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## processing_metrics_interval
{: #summary-processing-metrics-interval}

至少為 0.1 的選用性浮點值，指出服務將傳回處理度量值的間隔。如果 `processing_metrics` 參數為 `true`，則依預設服務每 1.0 秒傳回處理度量值一次。如需相關資訊，請參閱[處理度量值](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics)。

<table>
  <caption>表 16. processing_metrics_interval 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      不支援
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## profanity_filter
{: #summary-profanity-filter}

選用性的布林值，指出服務是否審查文字記錄中的褻瀆。依預設 (`true`)，會從文字記錄過濾褻瀆。如需相關資訊，請參閱[褻瀆內容過濾](/docs/services/speech-to-text?topic=speech-to-text-output#profanity_filter)。

<table>
  <caption>表 17. profanity_filter 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對美式英文正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## redaction
{: #summary-redaction}

選用性的布林值，指出服務是否編寫文字記錄中具有連續三位數以上的數字資料。如果您將 `redaction` 參數設定為 `true`，服務會自動將 `smart_formatting` 參數強制設定為 `true`。依預設 (`false`)，不會編寫數字資料。如需相關資訊，請參閱[數字編寫](/docs/services/speech-to-text?topic=speech-to-text-output#redaction)。

<table>
  <caption>表 18. redaction 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對美式英文、日文及韓文為測試版
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## smart_formatting
{: #summary-smart-formatting}

選用性的布林值，指出服務是否在最終文字記錄中，將日期、時間、數字、貨幣和類似值轉換為更為慣用的表示法。針對美式英文，此特性還會將特定關鍵字詞組轉換成標點符號。依預設 (`false`)，不會執行智慧型格式化。如需相關資訊，請參閱[智慧型格式化](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting)。

<table>
  <caption>表 19. smart_formatting 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對美式英文、日文及西班牙文為測試版
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## speaker_labels
{: #summary-speaker-labels}

選用性的布林值，指出服務是否識別哪些人在多參與者交流中講了哪些字組。如果您將 `speaker_labels` 參數設定為 `true`，服務會自動將 `timestamps` 參數強制設定為 `true`。依預設 (`false`)，不會傳回說話者標籤。如需相關資訊，請參閱[說話者標籤](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels)。


<table>
  <caption>表 20. speaker_labels 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對美式英文、日文及西班牙文（寬頻及窄頻模型）及英式英文（僅限窄頻模型）為測試版
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## timestamps
{: #summary-timestamps}

選用性的布林值，指出服務是否針對文字記錄中的字組產生時間戳記。依預設 (`false`)，不會傳回時間戳記。如需相關資訊，請參閱[字組時間戳記](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps)。

<table>
  <caption>表 21. timestamps 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## Transfer-Encoding
{: #summary-transfer-encoding}

選用性的值 `chunked`，導致音訊串流至服務。依預設，會以只有一次的遞送方式，一次傳送所有音訊。如需相關資訊，請參閱[音訊傳輸](/docs/services/speech-to-text?topic=speech-to-text-input#transmission)。

<table>
  <caption>表 22. Transfer-Encoding 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      不適用；一律串流
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的要求標頭
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的要求標頭
    </td>
  </tr>
</table>

## watson-token
{: #summary-watson-token}

如果您使用 Cloud Foundry 服務認證，這是您用來建立與 WebSocket 介面之已鑑別連線的選用性 {{site.data.keyword.watson}} 鑑別記號。如需相關資訊，請參閱[開啟連線](/docs/services/speech-to-text?topic=speech-to-text-websockets#WSopen)。

<table>
  <caption>表 23. watson-token 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 連線要求的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      不支援
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      不支援
    </td>
  </tr>
</table>

## word_alternatives_threshold
{: #summary-word-alternatives-threshold}

介於 0.0 到 1.0 之間的選用性倍精準數，用來指定服務針對輸入音訊的字組，報告聲學類似替代項目的臨界值。依預設，不會傳回替代字組。如需相關資訊，請參閱[替代字組](/docs/services/speech-to-text?topic=speech-to-text-output#word_alternatives)。

<table>
  <caption>表 24. word_alternatives_threshold 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## word_confidence
{: #summary-word-confidence}

選用性的布林值，指出服務是否針對文字記錄中的字組提供信賴度測量值。依預設 (`false`)，不會傳回字組信賴度測量值。如需相關資訊，請參閱[字組信賴度](/docs/services/speech-to-text?topic=speech-to-text-output#word_confidence)。

<table>
  <caption>表 25. word_confidence 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      JSON <code>start</code> 訊息的參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognize</code> 方法的查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/recognitions</code> 方法的查詢參數
    </td>
  </tr>
</table>

## X-Watson-Authorization-Token
{: #summary-x-watson-authorization-token}

選用性的鑑別記號，它會向服務提出已鑑別的要求，而不在每個呼叫中內嵌您的服務認證。依預設，每個要求都必須一起傳遞服務認證。Watson 鑑別記號是根據使用 `{username}` 和 `{password}` 來進行鑑別的 Cloud Foundry 服務認證。

`X-Watson-Authorization-Token` 標頭不接受 IAM 記號或 API 金鑰。
{: note}

<table>
  <caption>表 26. X-Watson-Authorization-Token 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      不支援；請使用 <code>watson-token</code> 查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      每個要求的要求標頭
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      每個要求的要求標頭
    </td>
  </tr>
</table>

## X-Watson-Learning-Opt-Out
{: #summary-x-watson-learning-opt-out}

選用性的布林值，指出您是否拒絕 {{site.data.keyword.IBM_notm}} 為了未來使用者改善服務而執行的預設要求記載。若要避免 IBM 存取您的資料以進行一般性的服務改善，請針對此參數指定 <code>true</code>。如需相關資訊，請參閱[要求記載](/docs/services/speech-to-text?topic=speech-to-text-input#logging)。

<table>
  <caption>表 27. X-Watson-Learning-Opt-Out 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 連線要求的 <code>x-watson-learning-opt-out</code> 查詢參數
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      每個要求的要求標頭
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      每個要求的要求標頭
    </td>
  </tr>
</table>

## X-Watson-Metadata
{: #summary-x-watson-metadata}

選用性的字串，它會建立客戶 ID 與針對辨識要求而傳遞之資料的關聯。此參數會接受引數 `customer_id={id}`。依預設，不會有任何客戶 ID 與資料相關聯。如需相關資訊，請參閱[資訊安全](/docs/services/speech-to-text?topic=speech-to-text-information-security)。

<table>
  <caption>表 28. X-Watson-Metadata 參數</caption>
  <tr>
    <th>可用性及使用</th>
    <th style="vertical-align:bottom">說明</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **可用性**
    </td>
    <td style="text-align:left">
      針對所有語言正式發行
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      <code>/v1/recognize</code> 連線要求的 <code>x-watson-metadata</code> 查詢參數
      （您必須將引數以 URL 編碼，例如 `customer_id%3dmy_customer_ID`。）
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **同步 HTTP**
    </td>
    <td style="text-align:left">
      POST <code>/v1/recognize</code> 要求的要求標頭
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **非同步 HTTP**
    </td>
    <td style="text-align:left">
      <code>POST /v1/register_callback</code> 及
      <code>POST /v1/recognitions</code> 要求的要求標頭
    </td>
  </tr>
</table>
