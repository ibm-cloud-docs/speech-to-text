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

# Características de las métricas
{: #metrics}

El servicio {{site.data.keyword.speechtotextfull}} puede devolver dos tipos de métricas opcionales para una solicitud de reconocimiento de voz:

-   [Métricas de proceso](#processing_metrics) proporciona información periódica sobre el proceso de servicio del audio de entrada. Utilice las métricas para medir el progreso de servicio transcribiendo el audio. Las métricas de proceso están disponibles con las interfaces WebSocket y HTTP asíncrono.
-   [Métricas de audio](#audio_metrics) proporcionan información sobre las características de señal del audio de entrada. Utilice las métricas para determinar las características y la calidad del audio. Las métricas de audio están disponibles con todas las interfaces de reconocimiento de voz.

De forma predeterminada, el servicio no devuelve métricas para una solicitud.

## Métricas de proceso
{: #processing_metrics}

Las métricas de proceso proporcionan información de temporización detallada sobre el análisis del servicio del audio de entrada. El servicio devuelve las métricas a intervalos específicos y con sucesos de transcripción como, por ejemplo, resultados provisionales y finales.

Entre las métricas se incluyen las estadísticas sobre la cantidad de datos de audio que ha recibido el servicio, la cantidad de datos de audio que ha transferido el servicio al motor de reconocimiento de voz y durante cuánto tiempo el servicio ha procesado los datos de audio. Si se solicitan las etiquetas de hablante, la información también muestra qué cantidad de datos de audio ha procesado el servicio para determinar las etiquetas de hablante.

Las métricas de proceso pueden resultar útiles para medir el progreso de una solicitud de reconocimiento. También pueden ayudarle a distinguir la ausencia de resultados debido a

-   Falta de audio.
-   Falta de discurso en el audio enviado.
-   Parones del motor en el servidor y parones de red entre el cliente y el servidor. Para distinguir entre los parones del motor y de red, los resultados son periódicamente en lugar de basarse en el suceso.

Las métricas también puede servir de ayuda a la hora de valorar el jitter de respuesta examinando los tiempos de llegada periódicos. Las métricas se generan a intervalos constantes, así que cualquier diferencia en los tiempos de llegada viene provocada por el jitter.

### Solicitud de métricas de proceso
{: #processing_metrics_request}

Para solicitar métricas de proceso, utilice los siguientes parámetros opcionales:

-   `processing_metrics` es un valor booleano que indica si el servicio debe devolver métricas de proceso. Especifique `true` para solicitar las métricas. De forma predeterminada, el servicio no devuelve ninguna métrica.
-   `processing_metrics_interval` es un valor flotante que especifica el intervalo en segundos de tiempo real en que el servicio debe devolver métricas. De forma predeterminada, el servicio devuelve métricas una vez por segundo. El servicio pasa por alto este parámetro a menos que se defina el parámetro `processing_metrics` en `true`.

    El parámetro acepta un valor mínimo de 0,1 segundos. El nivel de precisión no está restringido, por lo que puede especificar valores como, por ejemplo, 0,25 y 0,125. El servicio no impone ningún valor máximo.

La forma en que se proporcionan los parámetros y cómo devuelve el servicio las métricas de proceso vería según la interfaz:

-   Con la interfaz WebSocket, se especifican los parámetros con el mensaje JSON `start` para una solicitud de reconocimiento de voz. El servicio calcula y devuelve las métricas en tiempo real en el intervalo solicitado.
-   Con la interfaz HTTP asíncrona, se especifican los parámetros de consulta con una solicitud de reconocimiento de voz. El servicio calcula las métricas en el intervalo solicitado pero devuelve todas las métricas del audio con el resultado final de la transcripción.

El servicio también devuelve métricas de proceso para suceso de transcripción. Si solicita resultados provisionales con la interfaz
WebSocket, puede recibir métricas con mayor frecuencia que con el intervalo solicitado.

Para recibir métricas de proceso solamente para sucesos de transcripción en lugar de hacerlo a intervalos periódicos, defina el intervalo de proceso en un número mayor. Si el intervalo es mayor que la duración del audio, el servicio devuelve las métricas de proceso solamente para los sucesos de transcripción.

### Visión general de las métricas de proceso
{: #processing_metrics_understand}

El servicio devuelve métricas de proceso en el campo `processing_metrics` del objeto `SpeechRecognitionResults`. Para las métricas generadas a intervalos periódicos, el objeto solamente contiene el campo `processing_metrics`, tal como se muestra en el ejemplo siguiente.

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

El campo `processing_metrics` incluye un objeto `ProcessingMetrics` que tiene los campos siguientes:

-   `wall_clock_since_first_byte_received` es la cantidad de tiempo real en segundos que ha pasado desde que el servicio ha recibido el primer byte del audio de entrada. Los valores de este campo suelen ser múltiplos del intervalo de las métricas especificado, con dos diferencias:
    -   Los valores no pueden reflejar los intervalos exactos como, por ejemplo, 0,25, 0,5, etc. Los valores reales pueden ser, por ejemplo, 0,27, 0,52, etc., dependiendo de cuándo el servicio recibe y procesa el audio.
    -   Los valores para los sucesos de transcripción no están relacionados con el intervalo de proceso. El servicio devuelve resultados gestionados por sucesos a medida que se van produciendo.
-   `periodic` indica si las métricas se aplican a un intervalo periódico o a un suceso de transcripción:
    -   `true` significa que la respuesta la ha desencadenado un intervalo de proceso. La información solamente contiene métricas de proceso.
    -   `false` significa que la respuesta la ha desencadenado un suceso de transcripción. La información contiene métricas de proceso además de resultados de transcripción.

    Utilice este campo para identificar el motivo por el cual el servicio ha generado la respuesta y para filtrar diferentes resultados, si fuera necesario.
-   `processed_audio` incluye un objeto `ProcessedAudio` que proporciona información de temporización detallada sobre el proceso del servicio del audio de entrada.

El objeto `ProcessedAudio` incluye los campos siguientes. Todos los campos hacen referencia a segundos de audio a partir de esta respuesta. Solamente el campo `wall_clock_since_first_byte_received` hace referencia al tiempo real transcurrido.

-   `received` son los segundos de audio que ha recibido el servicio.
-   `seen_by_engine` son los segundos de audio que ha pasado el servicio al motor de proceso de habla.
-   `transcription` son los segundos de audio que el servicio ha procesado para el reconocimiento de voz.
-   `speaker_labels` son los segundos de audio que el servicio ha procesado para las etiquetas de hablante. La respuesta incluye este campo solamente si se solicitan etiquetas de hablante.

El motor de proceso de habla analiza el audio de entrada varias veces. El objeto `processed_audio` muestra los valores para el audio que ha procesado el motor y no los volverá a leer. El audio procesado no tiene ningún efecto sobre futuras hipótesis de reconocimiento.

Las métricas indican el progreso y la complejidad del proceso del motor:

-   `seen_by_engine` es audio que el servicio ha leído y ha pasado al motor al menos una vez.
-   `received` - `seen_by_engine` es audio que se ha almacenado en el almacenamiento intermedio del servicio pero que el motor todavía no ha visto o procesado.
-   La relación entre los tiempos es `received` >= `seen_by_engine` >= `transcription` >= `speaker_labels`.

Las relaciones siguientes también pueden resultar útiles a la hora de entender los resultados:

-   Los valores de los campos `received` y `seen_by_engine` son mayores que los valores de los campos `transcription` y `speaker_labels` durante el proceso de reconocimiento de voz. El servicio debe recibir el audio antes de que pueda empezar a procesar los resultados.
-   Los valores de los campos `received` y `seen_by_engine` son idénticos cuando el servicio ha terminado de procesar el audio. Los valores finales de los campos pueden ser mayores que los valores de los campos `transcription` y `speaker_labels` por un número fraccional de segundos.
-   El valor del campo `speaker_labels` suele recortar el valor del campo `transcription` durante el proceso de reconocimiento de voz. Los valores de los campos `transcription` y `speaker_labels` son idénticos cuando el servicio ha terminado de procesar el audio.

### Proceso de ejemplos de métricas: interfaz WebSocket
{: #processing_metrics_example_websocket}

En el ejemplo siguiente se muestra el mensaje `start` de una solicitud que se pasa a la interfaz WebSocket. La solicitud permite las métricas de proceso y establece el intervalo de las métricas de proceso en 0,25 segundos. Además, establece los parámetros `interim_results` y `speaker_labels` en `true`. El audio contiene el simple mensaje: "hello world long pause stop."

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

En la salida de ejemplo siguiente se muestran las primeras métricas de proceso que devuelve el servicio para la solicitud.

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

### Ejemplo de métricas de proceso: interfaz HTTP asíncrona
{: #processing_metrics_example_http}

En el ejemplo siguiente se muestra una solicitud de reconocimiento de voz para el método `/v1/recognitions` de la interfaz HTTP asíncrona. La solicitud permite las métricas de proceso y especifica un intervalo de 0,25 segundos. El archivo de audio vuelve a incluir el mensaje "hello world long pause stop".

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?processing_metrics=true&processing_metrics_interval=0.25"
```
{: pre}

En la salida de ejemplo siguiente se muestran las dos métricas de proceso que devuelve el servicio para la solicitud.

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

## Métricas de audio
{: #audio_metrics}

Las métricas de audio proporcionan información detallada sobre las características de señal del audio de entrada. Los resultados proporcionan métricas de agregación para todo el audio de entrada en la conclusión del proceso de habla. Para un usuario técnicamente sofisticado, las métricas pueden proporcionar un conocimiento significativo de las características detalladas del audio.

Puede utilizar las métricas de audio para proporcionar una indicación en tiempo real de un problema con el audio de entrada y posiblemente incluso una posible solución. Por ejemplo, puede proporcionar un mensaje del tipo "Hay demasiado ruido de fondo" o "Por favor, acérquese al micrófono". También puede utilizar una herramienta de análisis fuera de línea para revisar las características de señal y sugerir los datos que sean adecuados para futuras actualizaciones de modelos.

### Solicitud de métricas de audio
{: #audio_metrics_request}

Para solicitar métricas de audio, establezca el parámetro booleano `audio_metrics` en `true`. De forma predeterminada, el servicio no devuelve ninguna métrica.

-   Con la interfaz WebSocket, se especifica el parámetro con el mensaje JSON `start` para una solicitud de reconocimiento de voz.
-   Con las interfaces HTTP, especifique un parámetro de consulta con una solicitud de reconocimiento de voz.

El servicio devuelve métricas de audio con los resultados finales de transcripción. Devuelve las métricas sólo una vez para toda la secuencia de audio. Incluso si el servicio devuelve varios resultados de transcripción (para diferentes bloques de audio), devuelve solamente una instancia de las métricas muy al final de los resultados.

La interfaz WebSocket acepta varias secuencias de audio (o archivos) con una sola conexión. Delimite las distintas secuencias enviando mensajes `stop` o blobs binarios vacíos al servicio. En este caso, el servicio devuelve métricas independientes para cada secuencia de audio delimitada.

### Cómo funcionan las métricas de audio
{: #audio_metrics_understand}

El servicio devuelve las métricas en el campo `audio_metrics` del objeto `SpeechRecognitionResults`.

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

El campo `audio_metrics` incluye un objeto `AudioMetrics` que tiene dos campos:

-   `sampling_interval` indica el intervalo en segundos (normalmente 0,1 segundos) en que el servicio ha calculado las métricas de audio.
-   `accumulated` incluye un objeto `AudioMetricsDetails` que proporciona información detallada sobre las características de señal del audio de entrada.

Los siguientes campos del objeto `AudioMetricsDetails` proporcionan valores unarios:

-   `final` indica si las métricas son para el final de la secuencia de audio, lo que significa que la transcripción ha finalizado. Actualmente, el campo siempre es `true`. El servicio devuelve métricas sólo una vez por secuencia de audio. Los resultados proporcionan métricas agregadas para toda la secuencia.
-   `end_time` especifica el tiempo final en segundos del audio al que se aplican las métricas. Puesto que las métricas se aplican a toda la secuencia de audio, el tiempo final siempre es la longitud del audio.
-   `signal_to_noise_ratio` proporciona la proporción de señal a ruido (SNR - Signal-to-Noise Ratio) de la señal de audio. El valor indica la proporción de habla a ruido en el audio. Un valor válido estaría entre 0 y 100 decibelios (dB). El servicio omite el campo si no puede calcular el valor SNR para el audio.
-   `speech_ratio` es la proporción de segmentos de habla a no habla en la señal de audio. El valor válido estaría entre 0,0 y 1,0.
-   `high_frequency_loss` indica la probabilidad de que a la señal de audio le falte la mitad superior del contenido de frecuencia.
    -   Un valor cercano a 1.0 suele indicar audio de muestreo artificialmente alto, que afecta negativamente a la precisión de los resultados de la transcripción.
    -   Un valor de 0,0, o próximo a 0,0, indica que la señal de audio es buena y que tiene un espectro completo.
    -   Un valor alrededor de 0,5 significa que la detección del contenido de frecuencia es poco fiable o que no está disponible.

Los campos siguientes del objeto `AudioMetricsDetails` proporcionan histogramas para las características de señal. Cada campo proporciona una matriz de objetos `AudioMetricsHistogramBin`. Una sola unidad en cada histograma se calcula según la longitud de audio `sampling_interval`.

-   `direct_current_offset` define un histograma del componente de corriente directa (DC) acumulativa de la señal de audio.
-   `clipping_rate` define un histograma de la velocidad de recorte para los segmentos de audio. La velocidad de recorte se define como la fracción de muestras en el segmento que llegan al valor máximo o mínimo que ofrece el rango de cuantificación de audio.

    El servicio detecta automáticamente bien un rango de audio Pulse-Code Modulation (PCM) de 16 bits (entre -32768 y +32767), bien un rango unitario (entre -1,0 y +1,0). La velocidad de recorte está entre 0,0 y 1,0. Los valores superiores indican una posible degradación del reconocimiento de voz.
-   `speech_level` define un histograma del nivel de señal en los segmentos del audio que contienen habla. El nivel de señal se calcula como el valor Root-Mean-Square (RMS) en una escala de decibelios (dB) normalizado entre 0,0 (nivel mínimo) y 1,0 (nivel máximo).
-   `non_speech_level` define un histograma del nivel de señal en segmentos del audio que no contienen habla. El nivel de señal se calcula como el valor RMS en una escala de decibelios normalizado entre 0,0 (nivel mínimo) y 1,0 (nivel máximo).

Cada objeto `AudioMetricsHistogramBin` describe una bandeja con los límites `begin` y `end` definidos. Cada bandeja indica el valor `count` o el número de valores que se hallan dentro del rango de las características de señal para esa bandeja. Las bandejas primera y última de un histograma son las bandejas de límite. Abarcan intervalos entre el infinito negativo y el primer límite, y entre el último límite y el infinito positivo, respectivamente.

### Ejemplo de métricas de audio
{: #audio_metrics_example}

El ejemplo siguiente muestra una solicitud de reconocimiento de voz con la interfaz HTTP síncrona que devuelve métricas de audio. El archivo de audio contiene el simple mensaje "hello world long pause stop".

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitize?audio_metrics=true"
```
{: pre}

La respuesta incluye las métricas de audio para el audio de entrada completo, que tiene una duración de 7,0 segundos. El audio de entrada tiene algo más de habla que los segmentos sin habla: 37 y 33 segmentos, para un valor de 0,529 para `speech_ratio`. La velocidad de recorte es muy lenta, lo que indica una alta calidad del audio de entrada.

El campo `high_frequency_loss` tiene un valor de 0,5, lo que significa que la detección del servicio de contenido de frecuencia es poco fiable o que no está disponible. Los resultados omiten el campo `signal_to_noise_ratio` porque el servicio no ha podido calcular el valor SNR para el audio.

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
