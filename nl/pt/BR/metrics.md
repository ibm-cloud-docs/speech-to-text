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

# Recursos de métricas
{: #metrics}

O serviço {{site.data.keyword.speechtotextfull}} pode retornar dois tipos de métricas opcionais para uma solicitação de reconhecimento de voz:

-   As [métricas de processamento](#processing_metrics) fornecem informações periódicas sobre o processamento do áudio de entrada do serviço. Use as métricas para calibrar o progresso do serviço na transcrição do áudio. As métricas de processamento estão disponíveis com as interfaces HTTP assíncrona e WebSocket.
-   As [Métricas de áudio](#audio_metrics) fornecem informações sobre as características de sinal do áudio de entrada. Use as métricas para determinar as características e a qualidade do áudio. As métricas de áudio estão disponíveis com todas as interfaces de reconhecimento de voz.

Por padrão, o serviço não retorna nenhuma métrica para uma solicitação.

## Métricas de processamento
{: #processing_metrics}

As métricas de processamento fornecem informações de sincronização detalhadas sobre a análise do áudio de entrada do serviço. O serviço retorna as métricas em intervalos especificados e com eventos de transcrição, tais como resultados temporários e finais.

As métricas incluem estatísticas sobre a quantidade de áudio que o serviço recebeu, quanto áudio o serviço transferiu para o mecanismo de reconhecimento de voz e por quanto tempo o serviço está processando o áudio. Se você solicitar rótulos do falante, as informações também mostrarão quanto áudio o serviço processou para determinar os rótulos do falante.

As métricas de processamento podem ajudar a medir o progresso de uma solicitação de reconhecimento. Elas também podem ajudar a distinguir a ausência de resultados devido a

-   Perda de áudio.
-   Perda de fala no áudio enviado.
-   Paralisações do mecanismo no servidor e nas paralisações de rede entre o cliente e o servidor. Para diferenciar entre as paralisações do mecanismo e da rede, os resultados são periódicos em vez de baseados em evento.

As métricas também podem ajudar a estimar o jitter de resposta examinando os horários de chegada periódicos. As métricas são geradas em um intervalo constante, portanto, qualquer diferença no horário de chegada é causada pelo jitter.

### Solicitando métricas de processamento
{: #processing_metrics_request}

Para solicitar métricas de processamento, use os parâmetros opcionais a seguir:

-   `processing_metrics` é um booleano que indica se o serviço retornará as métricas de processamento. Especifique `true` para solicitar as métricas. Por padrão, o serviço não retorna nenhuma métrica.
-   `processing_metrics_interval` é uma flutuação que especifica o intervalo, em segundos, do horário real em que o serviço deve retornar as métricas. Por padrão, o serviço retorna métricas uma vez por segundo. O serviço ignora esse parâmetro a menos que o parâmetro `processing_metrics` esteja configurado como `true`.

    O parâmetro aceita um valor mínimo de 0,1 segundos. O nível de precisão não é restrito, portanto, é possível especificar valores como 0,25 e 0,125. O serviço não impõe um valor máximo.

Como você fornece os parâmetros e como o serviço retorna as métricas de processamento diferem por interface:

-   Com a interface do WebSocket, você especifica os parâmetros com a mensagem JSON `start` para uma solicitação de reconhecimento de voz. O serviço calcula e retorna métricas em tempo real no intervalo solicitado.
-   Com a interface HTTP assíncrona, você especifica parâmetros de consulta com uma solicitação de reconhecimento de voz. O serviço calcula as métricas no intervalo solicitado, mas ele retorna todas as métricas para o áudio com os resultados da transcrição final.

O serviço também retorna as métricas de processamento para eventos de transcrição. Se você solicitar resultados temporários com a interface do WebSocket, poderá receber métricas com maior frequência do que o intervalo solicitado.

Para receber métricas de processamento apenas para eventos de transcrição em vez de em intervalos periódicos, configure o intervalo de processamento para um número grande. Se o intervalo for maior do que a duração do áudio, o serviço retornará as métricas de processamento apenas para eventos de transcrição.

### Entendendo as métricas de processamento
{: #processing_metrics_understand}

O serviço retorna as métricas de processamento no campo `processing_metrics` do objeto `SpeechRecognitionResults`. Para as métricas geradas em intervalos periódicos, o objeto contém apenas o campo `processing_metrics`, conforme mostrado no exemplo a seguir.

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

O campo `processing_metrics` inclui um objeto `ProcessingMetrics` que tem os campos a seguir:

-   `wall_clock_since_first_byte_received` é a quantidade de tempo real em segundos que foi transmitida desde que o serviço recebeu o primeiro byte de áudio de entrada. Os valores nesse campo são geralmente múltiplos do intervalo de métricas especificado, com duas diferenças:
    -   Os valores podem não refletir intervalos exatos, como 0,25, 0,5 e assim por diante. Os valores reais podem, por exemplo, ser 0,27, 0.52, e assim por diante, dependendo de quando o serviço recebe e processa o áudio.
    -   Os valores para eventos de transcrição não estão relacionados ao intervalo de processamento. O serviço retorna resultados acionados por eventos conforme eles ocorrem.
-   `periodic` indica se as métricas se aplicam a um intervalo periódico ou a um evento de transcrição:
    -   `true` significa que a resposta foi acionada por um intervalo de processamento. As informações contêm apenas métricas de processamento.
    -   `false` significa que a resposta foi acionada por um evento de transcrições. As informações contêm as métricas de processamento mais os resultados de transcrições.

    Use esse campo para identificar por que o serviço gerou a resposta e para filtrar resultados diferentes, se necessário.
-   `processed_audio` inclui um objeto `ProcessedAudio` que fornece informações de sincronização detalhadas sobre o processamento do áudio de entrada do serviço.

O objeto `ProcessedAudio` inclui os campos a seguir. Todos os campos referem-se a segundos de áudio a partir dessa resposta. Apenas o campo `wall_clock_since_first_byte_received` refere-se a tempo real decorrido.

-   `received` são os segundos de áudio que o serviço recebeu.
-   `seen_by_engine` são os segundos de áudio que o serviço passou para seu mecanismo de processamento de fala.
-   `transcription` são os segundos de áudio que o serviço processou para reconhecimento de voz.
-   `speaker_labels` são os segundos de áudio que o serviço processou para rótulos do falante. A resposta incluirá esse campo apenas se você solicitar rótulos do falante.

O mecanismo de processamento de fala analisa o áudio de entrada várias vezes. O objeto `processed_audio` mostra valores para áudio que o mecanismo processou e não lerá novamente. O áudio processado não tem efeito sobre hipóteses de reconhecimento futuras.

As métricas indicam o progresso e a complexidade do processamento do mecanismo:

-   `seen_by_engine` é o áudio que o serviço leu e passou para o mecanismo pelo menos uma vez.
-   `received` - `seen_by_engine` é áudio que foi colocado em buffer no serviço, mas ainda não foi visto ou processado pelo mecanismo.
-   O relacionamento entre os horários é `received` > = `seen_by_engine` > = `transcription` > = `speaker_labels`.

Os relacionamentos a seguir também podem ser úteis para entender os resultados:

-   Os valores dos campos `received` e `seen_by_engine` são maiores que os valores dos campos `transcription` e `speaker_labels` durante o processamento de reconhecimento de voz. O serviço deve receber o áudio antes de poder começar a processar os resultados.
-   Os valores dos campos `received` e `seen_by_engine` serão idênticos quando o serviço concluir o processamento do áudio. Os valores finais dos campos podem ser maiores que os valores dos campos `transcription` e `speaker_labels` por um número fracionário de segundos.
-   O valor do campo `speaker_labels` geralmente segue o valor do campo `transcription` durante o processamento de reconhecimento de voz. Os valores dos campos `transcription` e `speaker_labels` são idênticos quando o serviço conclui o processamento do áudio.

### Exemplo de métricas de processamento: interface WebSocket
{: #processing_metrics_example_websocket}

O exemplo a seguir mostra a mensagem de `início` que é transmitida para uma solicitação para a interface do WebSocket. A solicitação ativa as métricas de processamento e configura o intervalo de métricas de processamento para 0,25 segundos. Ela também configura os parâmetros `interim_results` e `speaker_labels` como `true`. O áudio contém a mensagem simples "hello world long pause stop".

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

A saída de exemplo a seguir mostra os primeiros poucos resultados de métricas de processamento que o serviço retorna para a solicitação.

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

### Exemplo de métricas de processamento: interface HTTP assíncrona
{: #processing_metrics_example_http}

O exemplo a seguir mostra uma solicitação de reconhecimento de voz para o método `/v1/recognitions` da interface HTTP assíncrona. A solicitação ativa as métricas de processamento e especifica um intervalo de 0,25 segundos. O arquivo de áudio novamente inclui a mensagem "hello world long pause stop".

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?processing_metrics=true&processing_metrics_interval=0.25"
```
{: pre}

A saída de exemplo a seguir mostra os primeiros dois resultados de métricas de processamento que o serviço retorna para a solicitação.

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

## Métricas de áudio
{: #audio_metrics}

As métricas de áudio fornecem informações detalhadas sobre as características de sinal do áudio de entrada. Os resultados fornecem métricas agregadas para o áudio de entrada inteiro na conclusão do processamento de fala. Para um usuário tecnicamente sofisticado, as métricas podem fornecer insights significativos sobre as características detalhadas do áudio.

É possível usar as métricas de áudio para fornecer uma indicação em tempo real de um problema com o áudio de entrada e possivelmente até mesmo de uma solução em potencial. Por exemplo, é possível fornecer uma mensagem como "Há muito ruído no fundo" ou "Aproxime-se do microfone". Também é possível usar uma ferramenta analítica off-line para revisar as características do sinal e sugerir dados que sejam adequados para futuras atualizações de modelo.

### Solicitando métricas de áudio
{: #audio_metrics_request}

Para solicitar as métricas de áudio, configure o parâmetro booleano `audio_metrics` como `true`. Por padrão, o serviço não retorna nenhuma métrica.

-   Com a interface do WebSocket, você especifica o parâmetro com a mensagem JSON `start` para uma solicitação de reconhecimento de voz.
-   Com as interfaces HTTP, você especifica um parâmetro de consulta com uma solicitação de reconhecimento de voz.

O serviço retorna as métricas de áudio com os resultados da transcrição final. Ele retorna as métricas apenas uma vez para o fluxo de áudio inteiro. Mesmo se o serviço retornar múltiplos resultados de transcrição (para diferentes blocos de áudio), ele retornará apenas uma única instância das métricas no final dos resultados.

A interface do WebSocket aceita múltiplos fluxos de áudio (ou arquivos) com uma única conexão. Você delimita os diferentes fluxos ao enviar mensagens `stop` ou BLOBs binários vazios para o serviço. Nesse caso, o serviço retorna métricas separadas para cada fluxo de áudio delimitado.

### Entendendo as métricas de áudio
{: #audio_metrics_understand}

O serviço retorna as métricas no campo `audio_metrics` do objeto `SpeechRecognitionResults`.

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

O campo `audio_metrics` inclui um objeto `AudioMetrics` que tem dois campos:

-   `sampling_interval` indica o intervalo em segundos (geralmente 0,1 segundos) no qual o serviço calculou as métricas de áudio.
-   O `accumulated` inclui um objeto `AudioMetricsDetails` que fornece informações detalhadas sobre as características de sinal do áudio de entrada.

Os campos a seguir do objeto `AudioMetricsDetails` fornecem valores unários:

-   `final` indica se as métricas são para o final do fluxo de áudio, significando que a transcrição está concluída. Atualmente, o campo é sempre `true`. O serviço retorna métricas apenas uma vez por fluxo de áudio. Os resultados fornecem as métricas agregadas para o fluxo completo.
-   `end_time` especifica o horário de encerramento, em segundos, do áudio ao qual as métricas se aplicam. Como as métricas se aplicam a todo o fluxo de áudio, o horário de término é sempre o comprimento do áudio.
-   `signal_to_noise_ratio` fornece a proporção sinal-ruído (SNR) para o sinal de áudio. O valor indica a proporção de fala para o ruído no áudio. Um valor válido está no intervalo de 0 a 100 decibéis (dB). O serviço omitirá o campo se ele não puder calcular o SNR para o áudio.
-   `speech_ratio` é a proporção de fala para segmentos de não fala no sinal de áudio. O valor fica no intervalo de 0,0 a 1,0.
-   `high_frequency_loss` indica a probabilidade de que o sinal de áudio esteja ausente da metade superior de seu conteúdo de frequência.
    -   Um valor próximo de 1,0 geralmente indica áudio que passou por upsampling artificialmente, o que impacta negativamente a precisão dos resultados da transcrição.
    -   Um valor em ou perto de 0,0 indica que o sinal de áudio é bom e tem um espectro completo.
    -   Um valor em torno de 0,5 significa que a detecção do conteúdo de frequência não é confiável ou está indisponível.

Os campos a seguir do objeto `AudioMetricsDetails` fornecem histogramas para características de sinal. Cada campo fornece uma matriz de objetos `AudioMetricsHistogramBin`. Uma única unidade em cada histograma é calculada com base em um comprimento de áudio `sampling_interval`.

-   `direct_current_offset` define um histograma do componente de corrente contínua (DC) acumulativo (DC) do sinal de áudio.
-   `clipping_rate` define um histograma da taxa de recorte para os segmentos de áudio. A taxa de recorte é definida como a fração de amostras no segmento que atinge o valor máximo ou mínimo oferecido pelo intervalo de quantização de áudio.

    O serviço detecta automaticamente um intervalo de áudio de modulação de código de pulso (PCM) de 16 bits (-32768 a +32767) ou um intervalo de unidade (-1,0 a +1,0). A taxa de recorte está entre 0,0 e 1,0. Valores mais altos indicam possível degradação do reconhecimento de voz.
-   `speech_level` define um histograma do nível de sinal em segmentos do áudio que contêm a fala. O nível do sinal é calculado como o valor da raiz quadrada média (RMS) em uma escala decibéis (dB) normalizada para o intervalo de 0,0 (nível mínimo) a 1.0 (nível máximo).
-   `non_speech_level` define um histograma do nível de sinal em segmentos do áudio que não contêm o discurso. O nível do sinal é calculado como o valor RMS em uma escala de decibéis normalizada para o intervalo de 0,0 (nível mínimo) a 1,0 (nível máximo).

Cada objeto `AudioMetricsHistogramBin` descreve um compartimento com limites definidos de `begin` e `end`. Cada compartimento indica a `count` ou o número de valores no intervalo das características de sinal para esse compartimento. O primeiro e o último compartimentos de um histograma são os compartimentos de limite. Eles cobrem os intervalos entre o infinito negativo e o primeiro limite, e entre o último limite e o infinito positivo, respectivamente.

### Exemplo de métricas de áudio
{: #audio_metrics_example}

O exemplo a seguir mostra uma solicitação de reconhecimento de voz com a interface HTTP síncrona que retorna as métricas de áudio. O arquivo de áudio inclui a mensagem simples "hello world long pause stop".

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitize?audio_metrics=true"
```
{: pre}

A resposta inclui as métricas de áudio para o áudio de entrada completo, que tem uma duração de 7,0 segundos. O áudio de entrada tem um pouco mais de fala do que segmentos não de voz: 37 a 33 segmentos, para um `speech_ratio` de 0,529. A taxa de recorte é muito baixa, indicando áudio de entrada de alta qualidade.

O campo `high_frequency_loss` tem um valor de 0,5, o que significa que a detecção do conteúdo de frequência do serviço não é confiável ou está indisponível. Os resultados omitiam o campo `signal_to_noise_ratio` porque o serviço não pôde calcular o SNR para o áudio.

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
