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

# 메트릭 기능
{: #metrics}

{{site.data.keyword.speechtotextfull}} 서비스는 음성 인식 요청에 대한 두 가지 유형의 선택적 메트릭을 리턴할 수 있습니다. 

-   [처리 메트릭](#processing_metrics)은 입력 오디오의 서비스 처리에 대한 주기적인 정보를 제공합니다. 메트릭을 사용하여 오디오를 변환하는 중에 서비스 진행상태를 알아보십시오. WebSocket 및 비동기 HTTP 인터페이스에서 처리 메트릭을 사용할 수 있습니다. 
-   [오디오 메트릭](#audio_metrics)은 입력 오디오의 신호 특성에 대한 정보를 제공합니다. 메트릭을 사용하여 오디오의 특성 및 품질을 판별하십시오. 모든 음성 인식 인터페이스에서 오디오 메트릭을 사용할 수 있습니다.

기본적으로 이 서비스는 요청에 대한 메트릭을 리턴하지 않습니다. 

## 처리 메트릭
{: #processing_metrics}

처리 메트릭은 입력 오디오의 서비스 분석에 대한 자세한 타이밍 정보를 제공합니다. 서비스는 지정된 간격으로 변환 이벤트와 함께 메트릭을 리턴합니다(예: 중간 및 최종 결과). 

메트릭에는 서비스가 수신한 오디오의 양, 서비스가 음성 인식 엔진으로 전송되는 오디오의 양 및 서비스가 오디오를 처리하는 시간에 대한 통계가 포함됩니다. 스피커 레이블을 요청하는 경우 서비스가 스피커 레이블을 결정하기 위해 처리한 오디오의 양에 대한 정보도 표시됩니다. 

처리 메트릭은 인식 요청의 진행상태를 알아보는 데 도움이 될 수 있습니다. 또한 다음으로 인해 결과가 발생하지 않음을 식별하는 데에도 도움이 될 수 있습니다. 

-   오디오 부족
-   제출된 오디오의 음성 부족
-   서버의 엔진 구획 및 클라이언트와 서버 간의 네트워크 구획. 엔진과 네트워크 구획을 구별하기 위해 결과는 이벤트 기반이 아닌 주기적으로 발생합니다. 

메트릭은 주기적인 도달 시간을 조사하여 응답 지터를 예측하는 데에도 도움이 될 수 있습니다. 메트릭은 일정한 간격으로 생성되므로 도달 시간의 차이는 지터로 인해 발생합니다. 

### 처리 메트릭 요청
{: #processing_metrics_request}

처리 메트릭을 요청하려면 다음 선택적 매개변수를 사용하십시오.

-   `processing_metrics`는 서비스가 처리 메트릭을 리턴하는지 여부를 표시하는 부울입니다. 메트릭을 요청하려면 `true`를 지정하십시오. 기본적으로 이 서비스는 메트릭을 리턴하지 않습니다. 
-   `processing_metrics_interval`은 서비스가 메트릭을 리턴하기 위해 실제 벽 시간의 간격(초)을 지정하는 부동 소수점입니다. 기본적으로 이 서비스는 초당 한 번 메트릭을 리턴합니다. 서비스는 `processing_metrics` 매개변수가 `true`로 설정되지 않는 경우 이 매개변수를 무시합니다.

    매개변수는 0.1초의 최소값을 허용합니다. 정밀도 레벨은 제한되지 않으므로 0.25 및 0.125와 같은 값을 지정할 수 있습니다. 서비스는 최대값을 부과하지 않습니다. 

매개변수를 제공하는 방법 및 서비스가 처리 메트릭을 리턴하는 방법은 다음과 같이 인터페이스별로 달라집니다. 

-   WebSocket 인터페이스를 사용하면 음성 인식 요청으로 JSON `start` 메시지와 함께 매개변수를 지정합니다. 서비스는 요청된 간격으로 메트릭을 실시간으로 계산하고 리턴합니다. 
-   비동기 HTTP 인터페이스를 사용하면 음성 인식 요청으로 조회 매개변수를 지정합니다. 서비스는 요청된 간격으로 메트릭을 계산하지만 최종 변환 결과와 함께 오디오에 대한 모든 메트릭을 리턴합니다. 

서비스는 변환 이벤트에 대한 처리 메트릭도 리턴합니다. WebSocket 인터페이스를 사용하여 중간 결과를 요청하는 경우 요청된 간격보다 훨씬 더 자주 메트릭을 수신할 수 있습니다. 

주기적인 간격 대신 변환 이벤트에 대한 처리 메트릭만 수신하려면 처리 간격을 큰 수로 설정하십시오. 간격이 오디오의 지속 기간보다 긴 경우 서비스는 변환 이벤트에 대한 처리 메트릭만 리턴합니다. 

### 처리 메트릭 이해
{: #processing_metrics_understand}

서비스는 `SpeechRecognitionResults` 오브젝트의 `processing_metrics` 필드에 있는 처리 메트릭을 리턴합니다. 주기적인 간격으로 생성된 메트릭의 경우 오브젝트에는 다음 예제에서 표시된 대로 `processing_metrics` 필드만 포함됩니다. 

```javascript
{
  "processing_metrics": {
    "processed_audio": {
      "received": float,
      "seen_by_engine": float,
      "transcription": float,
      "speaker_labels": float
    },
    "wall_clock_since_first_byte_received": float,
    "periodic": boolean
  }
}
```
{: codeblock}

`processing_metrics` 필드에는 다음 필드가 있는 `ProcessingMetrics` 오브젝트가 포함됩니다. 

-   `wall_clock_since_first_byte_received`는 서비스가 입력 오디오의 첫 번째 바이트를 수신한 이후 경과한 실제 시간(초)입니다. 일반적으로 이 필드의 값은 지정된 메트릭 간격의 두 배이며, 두 가지 차이점은 다음과 같습니다. 
    -   값은 정확한 간격(예: 0.25, 0.5 등)을 반영하지 않습니다. 예를 들어, 실제 값은 0.27, 0.52 등이 될 수 있으며 서비스가 오디오를 수신하고 처리하는 시간에 따라 달라집니다. 
    -   변환 이벤트의 값은 처리 간격과 관련이 없습니다. 이 서비스 이벤트는 발생 시 이벤트 중심 결과를 리턴합니다. 
-   `periodic`은 메트릭이 주기적인 간격 또는 변환 이벤트에 적용되는지 여부를 표시합니다. 
    -   `true`는 응답이 처리 간격으로 트리거되었음을 의미합니다. 정보에는 처리 메트릭만 포함됩니다. 
    -   `false`는 응답이 변환 이벤트로 트리거되었음을 의미합니다. 정보에는 처리 메트릭 및 변환 결과가 포함됩니다. 

    이 필드를 사용하여 서비스가 응답을 생성한 이유를 식별하고 필요한 경우 다른 결과를 필터링하십시오.
-   `processed_audio`에는 입력 오디오의 서비스 처리에 대한 자세한 타이밍 정보를 제공하는 `ProcessedAudio` 오브젝트가 포함됩니다. 

`ProcessedAudio` 오브젝트에는 다음 필드가 포함됩니다. 모든 필드는 이 응답부터 오디오 시간(초)을 나타냅니다. `wall_clock_since_first_byte_received` 필드만 경과된 실시간을 나타냅니다. 

-   `received`는 서비스가 수신한 오디오 시간(초)입니다. 
-   `seen_by_engine`은 서비스가 음성 처리 엔진으로 전달된 오디오 시간(초)입니다. 
-   `transcription`은 서비스가 음성 인식에 대해 처리한 오디오 시간(초)입니다. 
-   `speaker_labels`는 서비스가 스피커 레이블에 대해 처리한 오디오 시간(초)입니다. 응답에는 스피커 레이블을 요청하는 경우에만 이 필드가 포함됩니다. 

음성 처리 엔진은 입력 오디오를 여러 번 분석합니다. `processed_audio` 오브젝트는 엔진이 처리한 오디오의 값을 표시하고 다시 읽지 않습니다. 처리된 오디오는 향후 인식 가설에 영향을 주지 않습니다. 

메트릭은 다음과 같이 엔진 처리의 진행상태 및 복잡도를 표시합니다. 

-   `seen_by_engine`은 서비스가 최소 한 번 읽고 엔진에 전달하는 오디오입니다. 
-   `received` - `seen_by_engine`은 서비스에서 버퍼링되었으나 엔진으로 표시되거나 처리되지 않은 오디오입니다. 
-   이 시간의 관계는 `received` >= `seen_by_engine` >= `transcription` >= `speaker_labels`입니다.

다음 관계도 결과를 이해하는 데 도움이 될 수 있습니다. 

-   `received` 및 `seen_by_engine` 필드의 값은 음성 인식 처리 중에 `transcription` 및 `speaker_labels` 필드의 값보다 큽니다. 서비스는 결과를 처리하기 전에 오디오를 수신해야 합니다. 
-   `received` 및 `seen_by_engine` 필드의 값은 서비스가 오디오 처리를 완료했을 때와 동일합니다. 필드의 최종 값은 소수(초)가 사용된 `transcription` 및 `speaker_labels` 필드의 값보다 클 수 있습니다. 
-   `speaker_labels` 필드의 값은 일반적으로 음성 인식 처리 중에 `transcription` 필드의 값을 추적합니다. `transcription` 및 `speaker_labels` 필드의 값은 서비스가 오디오 처리를 완료했을 때와 동일합니다. 

### 처리 메트릭 예제: WebSocket 인터페이스
{: #processing_metrics_example_websocket}

다음 예제에서는 요청에 대해 WebSocket 인터페이스로 전달되는 `start` 메시지를 보여줍니다. 요청은 처리 메트릭을 사용으로 설정하고 처리 메트릭 간격을 0.25초로 설정합니다. 또한 `interim_results` 및 `speaker_labels` 매개변수를 `true`로 설정합니다. 오디오에는 "Hello world의 긴 일시정지가 중지됩니다."라는 단순 메시지가 포함됩니다. 

```javascript
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/flac',
    processing_metrics: true,
    processing_metrics_interval: 0.25,
    interim_results: true,
    speaker_labels: true
  };
  websocket.send(JSON.stringify(message));
  websocket.send(blob);
}
```
{: codeblock}

다음 출력 예제에서는 서비스가 요청에 대해 리턴하는 처음 몇 개의 처리 메트릭 결과를 보여줍니다. 

```javascript
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 1.59,
      "transcription": 0.49,
      "speaker_labels": 0.0
    },
    "wall_clock_since_first_byte_received": 0.32,
    "periodic": true
  }
}
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 1.59,
      "transcription": 0.49,
      "speaker_labels": 0.0
    },
    "wall_clock_since_first_byte_received": 0.51,
    "periodic": false
  },
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "hello",
              0.43,
              0.76
            ],
            [
              "world",
              0.76,
              1.22
            ]
          ],
          "transcript": "hello world "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 2.59,
      "transcription": 1.25,
      "speaker_labels": 0.0
    },
    "wall_clock_since_first_byte_received": 0.57,
    "periodic": true
  }
}
. . .
```
{: codeblock}

### 처리 메트릭 예제: 비동기 HTTP 인터페이스
{: #processing_metrics_example_http}

다음 예제에서는 비동기 HTTP 인터페이스의 `/v1/recognitions` 메소드에 대한 음성 인식 요청을 보여줍니다. 요청은 처리 메트릭을 사용으로 설정하고 0.25초의 간격을 지정합니다. 오디오 파일에는 다시 "Hello world의 긴 일시정지가 중지됩니다."라는 메시지가 포함됩니다. 

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?processing_metrics=true&processing_metrics_interval=0.25"
```
{: pre}

다음 출력 예제에서는 서비스가 요청에 대해 리턴하는 처음 두 개의 처리 메트릭 결과를 보여줍니다. 

```javascript
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 1.59,
      "transcription": 0.49
    },
    "wall_clock_since_first_byte_received": 0.32,
    "periodic": true
  }
}
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 2.59,
      "transcription": 1.25
    },
    "wall_clock_since_first_byte_received": 0.57,
    "periodic": true
  }
}
. . .
```
{: codeblock}

## 오디오 메트릭
{: #audio_metrics}

오디오 메트릭은 입력 오디오의 신호 특성에 대한 자세한 정보를 제공합니다. 결과는 음성 처리 종료 시 전체 입력 오디오에 대해 집계된 메트릭을 제공합니다. 기술 전문가의 경우 메트릭은 오디오의 자세한 특성에 대한 의미 있는 통찰을 제공할 수 있습니다. 

오디오 메트릭을 사용하여 입력 오디오와 가능하면 잠재적인 솔루션까지도 포함된 실시간 표시를 제공할 수 있습니다. 예를 들어, "배경에 너무 큰 소음이 있음" 또는 "마이크에 더 가까이 다가오십시오."와 같은 메시지를 제공할 수 있습니다. 또한 오프라인 분석 도구를 사용하여 신호 특성을 검토하고 이후 모델 업데이트에 적합한 데이터를 제안할 수 있습니다.

### 오디오 메트릭 요청
{: #audio_metrics_request}

오디오 메트릭을 요청하려면 `audio_metrics` 부울 매개변수를 `true`로 설정하십시오. 기본적으로 이 서비스는 메트릭을 리턴하지 않습니다. 

-   WebSocket 인터페이스를 사용하면 음성 인식 요청으로 JSON `start` 메시지와 함께 매개변수를 지정합니다. 
-   HTTP 인터페이스를 사용하면 음성 인식 요청으로 조회 매개변수를 지정합니다. 

서비스는 최종 변환 결과와 함께 오디오 메트릭을 리턴합니다. 이는 전체 오디오 스트림에 대한 메트릭을 한 번만 리턴합니다. 서비스가 여러 변환 결과를 리턴하는 경우에도(다른 오디오 블록에 대해) 결과의 맨 끝에 있는 하나의 메트릭의 인스턴스만 리턴합니다. 

WebSocket 인터페이스는 한 번의 연결로 여러 오디오 스트림(또는 파일)을 허용합니다. `stop` 메시지 또는 비어 있는 2진 blob을 서비스에 전송하여 다른 스트림을 구분합니다. 이 경우 서비스는 분리된 오디오 스트림마다 별도의 메트릭을 리턴합니다. 

### 오디오 메트릭 이해
{: #audio_metrics_understand}

서비스는 `SpeechRecognitionResults` 오브젝트의 `processing_metrics` 필드에 있는 메트릭을 리턴합니다. 

```javascript
{
  "results": [
    . . .
  ],
  "result_index": integer,
  "audio_metrics": {
    "sampling_interval": float,
    "accumulated": {
      "final": boolean,
      "end_time": float,
      "signal_to_noise_ratio": float,
      "speech_ratio": float,
      "high_frequency_loss": float,
      "direct_current_offset": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ],
      "clipping_rate": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ],
      "speech_level": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ],
      "non_speech_level": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ]
    }
  }
}
```
{: codeblock}

`audio_metrics` 필드에는 다음 두 가지 필드가 있는 `AudioMetrics` 오브젝트가 포함되어 있습니다. 

-   `sampling_interval`은 서비스가 오디오 메트릭을 계산한 간격(초)(일반적으로 0.1초)을 표시합니다. 
-   `accumulated`에는 입력 오디오의 신호 특성에 대한 자세한 정보를 제공하는 `AudioMetricsDetails` 오브젝트가 포함됩니다. 

`AudioMetricsDetails` 오브젝트의 다음 필드에서는 단항 값을 제공합니다. 

-   `final`은 오디오 스트림 끝에 적합한 메트릭인지 여부를 표시하고, 이는 변환이 완료되었음을 의미합니다. 현재 필드는 항상 `true`입니다. 서비스는 오디오 스트림당 한 번만 메트릭을 리턴합니다. 결과는 전체 스트림에 대해 집계된 메트릭을 제공합니다.
-   `end_time`은 메트릭이 적용하는 오디오의 종료 시간(초)을 지정합니다. 메트릭이 전체 오디오 스트림에 적용되므로 종료 시간은 항상 오디오의 길이와 같습니다. 
-   `signal_to_noise_ratio`는 오디오 신호에 대한 신호 대 소음 비율(SNR)을 제공합니다. 값은 오디오에서 음성 대 소음 비율을 나타냅니다. 올바른 값의 범위는 0 - 100데시벨(dB)입니다. 서비스가 오디오의 SNR을 계산할 수 없는 경우 필드를 생략합니다. 
-   `speech_ratio`는 오디오 신호에서 음성 대 비음성 세그먼트의 비율입니다. 값의 범위는 0.0 - 1.0입니다.
-   `high_frequency_loss`는 오디오 신호에 주파수 특성의 상부 절반이 없는 가능성이 있음을 표시합니다. 
    -   일반적으로 1.0에 근접한 값은 인위적으로 업샘플링된 오디오를 나타내며 이는 변환 결과의 정확성에 부정적인 영향을 줍니다. 
    -   값이 0.0이거나 0.0에 근접한 값은 오디오 신호가 양호하며 전체 스펙트럼이 있음을 나타냅니다. 
    -   0.5에 근접한 값은 주파수 특성이 신뢰할 수 없거나 사용할 수 없음을 의미합니다. 

`AudioMetricsDetails` 오브젝트의 다음 필드에서는 신호 특성에 대한 히스토그램을 제공합니다. 각 필드는 `AudioMetricsHistogramBin` 오브젝트의 배열을 제공합니다. 각 히스토그램의 단일 단위는 오디오의 `sampling_interval` 길이를 기반으로 하여 계산됩니다. 

-   `direct_current_offset`은 오디오 신호에 대한 누적 직류(DC) 컴포넌트의 히스토그램을 정의합니다. 
-   `clipping_rate`는 오디오 세그먼트에 대한 클리핑 비율의 히스토그램을 정의합니다. 클리핑 비율은 오디오 양자화 범위로 제공되는 최대값 및 최소값에 도달하는 세그먼트에서 샘플의 소수로 정의됩니다. 

    서비스는 16비트 PCM(Pulse-Code Modulation) 오디오 범위(-32768 - +32767) 또는 단위 범위(-1.0 - +1.0)로 자동 발견합니다. 클리핑 비율의 범위는 0.0 - 1.0입니다. 값이 커지면 음성 인식의 성능이 저하될 가능성이 있음을 표시합니다.
-   `speech_level`은 음성이 포함되는 오디오의 세그먼트 내 신호 레벨의 히스토그램을 정의합니다. 신호 레벨은 0.0(최소 레벨) - 1.0(최대 레벨) 범위로 표준화되는 데시벨(dB) 스케일에서 RMS(Root-Mean-Square) 값으로 계산됩니다. 
-   `non_speech_level`은 음성이 포함되지 않은 오디오의 세그먼트 내 신호 레벨의 히스토그램을 정의합니다. 신호 레벨은 0.0(최소 레벨) - 1.0(최대 레벨) 범위로 표준화되는 데시벨 스케일에서 RMS 값으로 계산됩니다. 

각 `AudioMetricsHistogramBin` 오브젝트에서는 정의된 `begin` 및 `end` 경계를 포함한 바이너리에 대해 설명합니다. 각 바이너리는 해당 바이너리에 대한 신호 특성의 범위에서 값의 수 또는 `count`를 표시합니다. 히스토그램의 첫 번째 바이너리 및 마지막 바이너리는 경계 바이너리입니다. 음의 무한대와 첫 번째 경계 사이의 간격과 마지막 경계와 양의 무한대 사이의 간격을 각각 포함합니다. 

### 오디오 메트릭 예제
{: #audio_metrics_example}

다음 예제에서는 오디오 메트릭을 리턴하는 동기 HTTP 인터페이스가 포함된 음성 인식 요청을 보여줍니다. 오디오 파일에는 "Hello world의 긴 일시정지가 중지됩니다."라는 단순 메시지가 포함됩니다. 

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitize?audio_metrics=true"
```
{: pre}

응답에는 전체 입력 오디오에 대한 오디오 메트릭이 포함되며, 지속 기간은 7.0초입니다. 입력 오디오에는 0.529의 `speech_ratio`로 음성 세그먼트가 비음성 세그먼트보다 좀 더 많습니다(37 대 33개의 세그먼트). 고품질 입력 오디오를 나타내는 클리핑 비율이 매우 낮습니다. 

`high_frequency_loss` 필드에는 0.5 값이 있으며, 주파수 컨텐츠의 서비스 발견이 신뢰될 수 없거나 사용 불가능함을 의미합니다. 서비스가 오디오에 대한 SNR을 계산할 수 없으므로 결과에서는 `signal_to_noise_ratio` 필드를 생략합니다. 

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "hello world "
        }
      ],
      "final": true
    },
    {
      "alternatives": [
        {
          "confidence": 0.79,
          "transcript": "long pause "
        }
      ],
      "final": true
    },
    {
      "alternatives": [
        {
          "confidence": 0.97,
          "transcript": "stop "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0,
  "audio_metrics": {
    "sampling_interval": 0.1,
    "accumulated": {
      "final": true,
      "end_time": 7.0,
      "speech_ratio": 0.529,
      "high_frequency_loss": 0.5,
      "direct_current_offset": [
        {"begin": -1.0, "end": -0.9, "count": 0},
        {"begin": -0.9, "end": -0.7, "count": 0},
        {"begin": -0.7, "end": -0.5, "count": 0},
        {"begin": -0.5, "end": -0.3, "count": 0},
        {"begin": -0.3, "end": -0.1, "count": 0},
        {"begin": -0.1, "end": 0.1, "count": 70},
        {"begin": 0.1, "end": 0.3, "count": 0},
        {"begin": 0.3, "end": 0.5, "count": 0},
        {"begin": 0.5, "end": 0.7, "count": 0},
        {"begin": 0.7, "end": 0.9, "count": 0},
        {"begin": 0.9, "end": 1.0, "count": 0}
      ],
      "clipping_rate": [
        {"begin": 0.0, "end": 1e-05, "count": 70},
        {"begin": 1e-05, "end": 0.0001, "count": 0},
        {"begin": 0.0001, "end": 0.001, "count": 0},
        {"begin": 0.001, "end": 0.01, "count": 0},
        {"begin": 0.01, "end": 0.1, "count": 0},
        {"begin": 0.1, "end": 1.0, "count": 0}
      ],
      "speech_level": [
        {"begin": 0.0, "end": 0.1, "count": 37},
        {"begin": 0.1, "end": 0.2, "count": 0},
        {"begin": 0.2, "end": 0.3, "count": 0},
        {"begin": 0.3, "end": 0.4, "count": 0},
        {"begin": 0.4, "end": 0.5, "count": 0},
        {"begin": 0.5, "end": 0.6, "count": 0},
        {"begin": 0.6, "end": 0.7, "count": 0},
        {"begin": 0.7, "end": 0.8, "count": 0},
        {"begin": 0.8, "end": 0.9, "count": 0},
        {"begin": 0.9, "end": 1.0, "count": 0}
      ],
      "non_speech_level": [
        {"begin": 0.0, "end": 0.1, "count": 33},
        {"begin": 0.1, "end": 0.2, "count": 0},
        {"begin": 0.2, "end": 0.3, "count": 0},
        {"begin": 0.3, "end": 0.4, "count": 0},
        {"begin": 0.4, "end": 0.5, "count": 0},
        {"begin": 0.5, "end": 0.6, "count": 0},
        {"begin": 0.6, "end": 0.7, "count": 0},
        {"begin": 0.7, "end": 0.8, "count": 0},
        {"begin": 0.8, "end": 0.9, "count": 0},
        {"begin": 0.9, "end": 1.0, "count": 0}
      ]
    }
  }
}
```
{: codeblock}
