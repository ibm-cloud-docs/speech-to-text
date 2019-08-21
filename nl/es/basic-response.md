---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-24"

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

# Visión general de los resultados del reconocimiento
{: #basic-response}

Independientemente de la interfaz que utilice, el servicio {{site.data.keyword.speechtotextfull}} devuelve resultados de la transcripción que reflejan los parámetros de entrada y de salida que especifique. El servicio devuelve todo el contenido de la respuesta JSON en el juego de caracteres UTF-8.
{: shortdesc}

## Respuesta de transcripción básica
{: #response}

El servicio devuelve la respuesta siguiente para los ejemplos del apartado [Cómo realizar una solicitud de reconocimiento](/docs/services/speech-to-text?topic=speech-to-text-basic-request). En los ejemplos solo se pasa un archivo de audio y su tipo de contenido. En el audio se dice una sola frase sin pausas perceptibles entre las palabras.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

El servicio devuelve un objeto `SpeechRecognitionResults`, que es el objeto de respuesta de nivel superior. Para estas sencillas solicitudes, el objeto incluye un campo `results` y un campo `result_index`:

-   El campo `results` contiene una matriz de información sobre los resultados de la transcripción. En este ejemplo, el campo `alternatives` incluye la transcripción (`transcript`) y el nivel de confianza (`confidence`) del servicio en los resultados. El campo `final` tiene el valor `true`, lo que indica que estos resultados no cambiarán.
-   El campo `result_index` proporciona un identificador exclusivo para los resultados. En el ejemplo se muestran los resultados finales para una solicitud con un solo archivo de audio que no tiene pausas y la solicitud no incluye parámetros adicionales. De modo que el servicio devuelve un solo campo `result_index` con el valor `0`, que siempre es el índice inicial.

Si el audio de entrada es más complejo o si la solicitud incluye parámetros adicionales, los resultados pueden contener mucha más información.

### El campo alternatives
{: #responseAlternatives}

El campo `alternatives` contiene una matriz de resultados de la transcripción. Para esta solicitud, la matriz incluye un solo elemento.

-   El campo `confidence` es una puntuación que indica el nivel de confianza del servicio en la transcripción, que en este ejemplo se acerca al 90 por ciento.
-   El campo `transcript` proporciona los resultados de la transcripción.

Los campos `final` y `result_index` califican el significado de estos campos.

### El campo final
{: #responseFinal}

El campo `final` indica si la transcripción muestra los resultados finales de la transcripción:

-   El campo tiene el valor `true` si se trata de resultados finales; se garantiza que no cambiarán. El servicio no envía más actualizaciones de las transcripciones que devuelve como resultados finales.
-   El campo tiene el valor `false` para los resultados provisionales, que están sujetos a cambios. Si utiliza el parámetro `interim_results` con la interfaz WebSocket, el servicio devuelve hipótesis provisionales en evolución en forma de varios campos `results` a medida que transcribe el audio. El campo `final` siempre tiene el valor `falso` para los resultados provisionales. El servicio establece el campo en `true` en el caso de los resultados finales del audio. El servicio no envía más actualizaciones para la transcripción de ese audio.

Para obtener resultados provisionales, utilice la interfaz WebSocket y establezca el parámetro `interim_results` en `true`. Para obtener más información, consulte [Resultados provisionales](/docs/services/speech-to-text?topic=speech-to-text-output#interim).

### El campo result_index
{: #responseResultIndex}

El campo `result_index` proporciona un identificador de los resultados que es exclusivo para dicha solicitud. Si solicita resultados provisionales, el servicio envía varios campos `results` correspondientes a hipótesis progresivas del audio de entrada. Los índices de los resultados provisionales correspondientes al mismo audio siempre tienen el mismo valor, así como los resultados finales del mismo audio.

Una vez que recibe resultados finales de un audio, el servicio no envía ningún otro resultado con ese índice para el resto de la solicitud. El índice correspondiente cualquier resultado posterior se incrementa en uno.

Independientemente de si solicita resultados provisionales, el servicio puede devolver varios resultados finales con distintos índices si el audio incluye pausas o periodos prolongados de silencio. Para obtener más información, consulte [Pausas y silencio](#pauses-silence).

Si el audio genera varios resultados finales, concatene los elementos `transcript` de los resultados finales para ensamblar la transcripción completa del audio. Ensamble los resultados por orden de `result_index`. Puede omitir los resultados provisionales cuyo campo `final` tenga el valor `false`.

### Contenido de respuesta adicional
{: #responseAdditional}

Muchos de los parámetros de salida que están disponibles para el reconocimiento de voz influyen en el contenido de la respuesta del servicio. Por ejemplo, los parámetros siguientes hacen que el servicio devuelva información adicional sobre el audio:

-   El parámetro `max_alternatives` solicita varias transcripciones alternativas. La matriz `alternatives` incluye un elemento para cada alternativa. Cada elemento de la matriz incluye un campo `transcript`. Solo la alternativa más probable incluye un campo de `confidence`.
-   Los parámetros `word_confidence` y `timestamps` solicitan información adicional acerca de las palabras individuales de la transcripción. La matriz `alternatives` incluye campos adicionales con esos nombres.
-   Los parámetros `keywords` y `keywords_threshold` solicitan la detección de palabras clave. La matriz `results` incluye un campo `keywords_result`.
-   El parámetro `word_alternatives_threshold` solicita resultados con una acústica similar para palabras individuales. La matriz `results` incluye un campo `word_alternatives`.
-   El parámetro `interim_results` de la interfaz WebSocket solicita una hipótesis de transcripción provisional. Como se ha mencionado anteriormente, el servicio envía varias respuestas a medida que transcribe el audio.
-   El parámetro `speaker_labels` identifica a los oradores individuales de un intercambio con varios participantes. La respuesta incluye un campo `speaker_labels` en el mismo nivel que los campos `results` y `results_index`.

Para obtener más información acerca de estos y otros parámetros que pueden afectar a la respuesta del servicio, consulte [Características de salida](/docs/services/speech-to-text?topic=speech-to-text-output). Para obtener más información acerca de todos los parámetros disponibles, consulte el [Resumen de parámetros](/docs/services/speech-to-text?topic=speech-to-text-summary).

## Pausas y silencio
{: #pauses-silence}

El servicio transcribe toda una secuencia de audio hasta que la secuencia termina o hasta que se excede el tiempo de espera. Sin embargo, si el audio incluye pausas o un silencio extendido entre palabras o frases habladas, los resultados de reconocimiento pueden incluir múltiples resultados finales.

El intervalo de pausa predeterminado que utiliza el servicio para determinar resultados finales separados es de aproximadamente un segundo. En el caso de la mayoría de los idiomas, el intervalo de pausa es exactamente de 0,8 segundos; para el chino, el intervalo es de 0,6 segundos. No se puede cambiar la duración del intervalo de pausa.

La forma en que el servicio devuelve los resultados depende de la interfaz que se utilice. En los ejemplos siguientes se muestran respuestas con dos resultados finales de las interfaces HTTP y WebSocket. En ambos casos se utiliza el mismo audio de entrada. En el audio se pronuncia la frase "one two three four five six," con una pausa de un segundo entre las palabras "three" y "four."

-   *En el caso de las interfaces HTTP,* el servicio envía un solo objeto `SpeechRecognitionResults`. La matriz `alternatives` tiene un elemento para cada resultado final. La respuesta tiene un solo campo `result_index` con el valor `0`.

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
        {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

-   *En el caso de la interfaz WebSocket,* el servicio envía dos respuestas con dos objetos `SpeechRecognitionResults` diferentes. Cada objeto de respuesta tiene un campo `result_index` diferente, con el valor `0` para la primera respuesta y con el valor `1` para la segunda respuesta.

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
      ]
      "result_index": 0
    }
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
          "final": true
        }
      ],
      "result_index": 1
    }
    ```
    {: codeblock}

Si los resultados incluyen varios resultados finales, concatene los elementos `transcript` de los resultados finales para ensamblar la transcripción completa del audio.

Un silencio de 30 segundos en la secuencia de audio puede dar lugar a un [tiempo de espera excedido de inactividad](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-inactivity).
{: note}

## Marcadores de duda
{: #hesitation}

El servicio puede incluir marcadores de duda en una transcripción si descubre breves muletillas o pausas en el habla. Estas pausas, también conocidas como falta de fluidez, pueden incluir muletillas como "uhm", "uh", "hmm" y expresiones no léxicas relacionadas. A menos que tenga que usarlos para su aplicación, puede filtrar los marcadores de duda de forma segura de una transcripción.

En inglés, el servicio utiliza la señal de duda `%HESITATION`, tal como se muestra en el ejemplo siguiente. Otros idiomas pueden utilizar otros marcadores; por ejemplo, el japonés utiliza la señal `D_`.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Los marcadores de duda también pueden aparecer en otros campos de una transcripción. Por ejemplo, si solicita [indicaciones de fecha y hora de palabras](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps) para las palabras individuales de una transcripción, el servicio muestra la hora de inicio y de finalización de cada marcador de duda.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            . . .
            [
              "that",
              7.31,
              7.69
            ],
            [
              "%HESITATION",
              7.69,
              7.98
            ],
            [
              "that's",
              7.98,
              8.41
            ],
            [
              "a",
              8.41,
              8.48
            ],
            . . .
          ],
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Uso de mayúsculas
{: #capitalization}

Para la mayoría de los idiomas, el servicio no utiliza mayúsculas en las transcripciones de respuesta. Si el uso de mayúsculas es importante para su aplicación, debe poner en mayúscula la primera palabra de cada frase y cualquier otro término que deba ir en mayúsculas.

*Solo para inglés de Estados Unidos,* el servicio escribe en mayúsculas muchos nombres propios. Por ejemplo, el servicio devuelve el texto siguiente para la frase especificada:

```
Barack Obama graduated from Columbia University
```
{: codeblock}

En el caso de otros idiomas, el servicio devuelve el texto siguiente:

```
barack obama graduated from columbia university
```
{: codeblock}

El servicio siempre aplica este uso de mayúsculas a inglés de EE. UU., independientemente de si utiliza el formateo inteligente.

## Puntuación
{: #punctuation}

El servicio no inserta signos de puntuación en las transcripciones de respuesta de forma predeterminada. Debe añadir los signos de puntuación que necesite a los resultados del servicio.

Para algunos idiomas, puede utilizar el formateo inteligente para indicar al servicio que sustituya los signos de puntuación, por ejemplo comas, puntos, signos de interrogación y signos de exclamación, por determinadas series de palabras clave. Para obtener más información, consulte [Formateo inteligente](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting).
