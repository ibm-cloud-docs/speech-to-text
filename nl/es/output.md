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

# Características de salida
{: #output}

El servicio {{site.data.keyword.speechtotextfull}} ofrece las siguientes características para indicar la información que el servicio debe incluir en sus resultados de transcripción para una solicitud de reconocimiento de voz. Todos los parámetros de salida son opcionales.
{: shortdesc}

-   Para ver ejemplos de solicitudes sencillas de reconocimiento de voz para cada una de las interfaces del servicio, consulte [Cómo realizar una solicitud de reconocimiento](/docs/services/speech-to-text?topic=speech-to-text-basic-request).
-   Para ver ejemplos y descripciones de respuestas de reconocimiento de voz, consulte [Visión general de los resultados del reconocimiento](/docs/services/speech-to-text?topic=speech-to-text-basic-response). El servicio devuelve todo el contenido de la respuesta JSON en el juego de caracteres UTF-8.
-   Para ver una lista ordenada alfabéticamente de todos los parámetros de reconocimiento de voz disponibles, incluido su estado (disponible a nivel general o beta) y los idiomas admitidos, consulte el [Resumen de parámetros](/docs/services/speech-to-text?topic=speech-to-text-summary).

## Etiquetas de orador
{: #speaker_labels}

La característica de etiquetas de orador es una funcionalidad beta que está disponible en inglés de EE. UU., japonés y español (tanto modelos de banda ancha como de banda estrecha) y en inglés del Reino Unido (solo modelo de banda estrecha).
{: note}

Las etiquetas de orador identifican qué persona ha pronunciado cada palabra en un intercambio con varios participantes. (El hecho de etiquetar quién habla y cuándo a veces recibe el nombre de *segmentación de oradores*.) Puede utilizar la información para desarrollar una transcripción persona a persona de una secuencia de audio, como por ejemplo la de contacto con un centro de atención al cliente. O bien puede utilizarlo para animar un intercambio con un robot o un avatar de conversación. Para obtener el mejor rendimiento, utilice un audio que dure al menos un minuto.

Las etiquetas de orador están optimizadas para los escenarios con dos oradores. Funcionan mejor para conversaciones telefónicas que en las que participan dos personas en un intercambio ampliado. Pueden manejar hasta seis oradores, pero si hay más de dos oradores el rendimiento puede ser variable. Los intercambios entre dos personas suelen realizarse a través de medios de banda estrecha, pero se pueden utilizar etiquetas de orador con los modelos de banda ancha y banda estrecha admitidos.

Para utilizar la característica, establezca el parámetro `speaker_labels` en `true` para una solicitud de reconocimiento; el parámetro tiene el valor `false` de forma predeterminada. El servicio identifica los oradores por medio de palabras individuales del audio. Se basa en el momento de inicio y de fin de la palabra para identificar su orador. Por lo tanto, la habilitación de las etiquetas de orador también impone que el parámetro `timestamps` tenga el valor `true` (consulte [Indicaciones de fecha y hora de palabras](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps)).

### Ejemplo de etiquetas de orador
{: #speakerLabelsExample}

La siguiente solicitud de ejemplo muestra una respuesta que incluye las etiquetas de orador. Los valores numéricos que están asociados a cada elemento de la matriz `timestamps` son las horas de inicio y de finalización de la palabra en el audio.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-multi.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel&speaker_labels=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "hello",
              0.68,
              1.19
            ],
            [
              "yeah",
              1.47,
              1.93
            ],
            [
              "yeah",
              1.96,
              2.12
            ],
            [
              "how's",
              2.12,
              2.59
            ],
            [
              "Billy",
              2.59,
              3.17
            ],
            . . .
          ]
          "confidence": 0.82,
          "transcript": "hello yeah yeah how's Billy "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0,
  "speaker_labels": [
    {
      "from": 0.68,
      "to": 1.19,
      "speaker": 2,
      "confidence": 0.42,
      "final": false
    },
    {
      "from": 1.47,
      "to": 1.93,
      "speaker": 1,
      "confidence": 0.52,
      "final": false
    },
    {
      "from": 1.96,
      "to": 2.12,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.12,
      "to": 2.59,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.59,
      "to": 3.17,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    . . .
  ]
}
```
{: codeblock}

El campo `transcript` muestra la transcripción final del audio, en la que se muestran las palabras tal como las han pronunciado los participantes. Comparando y sincronizando las etiquetas de orador con las indicaciones de fecha y hora, puede volver a ensamblar la conversación tal como se ha producido.

<table style="width:50%">
  <caption>Tabla 1. Ejemplo de etiquetas de orador</caption>
  <tr>
    <th style="text-align:left">Indicación de fecha y hora</th>
    <th style="text-align:left">Etiqueta de orador</th>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"hello",<br/>
      0.68,<br/>
      1.19</code>
    </td>
    <td>
      <code>"from": 0.68,<br/>
      "to": 1.19,<br/>
      "speaker": 2,<br/>
      "confidence": 0.42,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.47,<br/>
      1.93</code>
    </td>
    <td>
      <code>"from": 1.47,<br/>
      "to": 1.93,<br/>
      "speaker": 1,<br/>
      "confidence": 0.52,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.96,<br/>
      2.12</code>
    </td>
    <td>
      <code>"from": 1.96,<br/>
      "to": 2.12,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"how's",<br/>
      2.12,<br/>
      2.59</code>
    </td>
    <td>
      <code>"from": 2.12,<br/>
      "to": 2.59,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"Billy",<br/>
      2.59,<br/>
      3.17</code>
    </td>
    <td>
      <code>"from": 2.59,<br/>
      "to": 3.17,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
</table>

Los resultados indican claramente cómo ha comenzado este intercambio entre dos personas:

-   **Orador 2** - "Hello?"
-   **Orador 1** - "Yeah?"
-   **Orador 2** - "Yeah, how's Billy?"

En el ejemplo, los campos `confidence` indican la confianza del servicio en la identificación de los oradores. El campo `final` tiene el valor `false` para cada palabra. Este valor indica que el servicio todavía está procesando el audio. El servicio podría cambiar su identificación del orador o su confianza en palabras individuales antes de terminar.

### ID de orador para etiquetas de orador
{: #speakerLabelsIDs}

El ejemplo también ilustra un aspecto interesante de los ID de orador. En los campos `speaker`, el primer orador tiene el ID `2` y el segundo tiene el ID `1`. El servicio desarrolla una mejor comprensión de los patrones de orador a medida que procesa el audio. Por lo tanto, puede cambiar los ID de orador para palabras individuales y también puede añadir y eliminar oradores, hasta que genera sus resultados finales.

Como resultado, es posible que los ID de orador no sean secuenciales, contiguos u ordenados. Por ejemplo, los ID suelen comenzar por `0`, pero, en el ejemplo anterior, el primer ID es `1`. Es probable que el servicio haya omitido una asignación de palabra anterior para el orador `0` en base a un análisis adicional del audio. También es posible que se omitan los últimos oradores. Las omisiones pueden dar lugar a saltos en la numeración y pueden producir resultados para dos oradores que están etiquetados, por ejemplo, `0` y `2`. Otra consideración es que los oradores pueden abandonar una conversación. Por lo tanto, es posible que un participante que solo participa a las primeras fases de una conversación no aparezca en los resultados posteriores.

### Solicitud de resultados provisionales para etiquetas de orador
{: #speakerLabelsInterim}

Con la interfaz WebSocket, puede solicitar resultados provisionales, así como etiquetas de orador (consulte [Resultados provisionales](/docs/services/speech-to-text?topic=speech-to-text-output#interim)). Los resultados finales suelen ser mejores que los resultados provisionales. Sin embargo, los resultados provisionales pueden ayudar a identificar la evolución de una transcripción y la asignación de etiquetas de orador. Los resultados provisionales pueden indicar dónde han aparecido y desaparecido oradores e ID transitorios. Sin embargo, el servicio puede reutilizar los ID de los oradores que identifica inicialmente y luego reconsidera y omite. Por lo tanto, un ID puede hacer referencia a dos oradores diferentes en los resultados provisionales y finales.

Su solicita tanto resultados provisionales como etiquetas de orador, los resultados finales de las secuencias de audio largas pueden llegar bien después de que se devuelvan los resultados provisionales iniciales. También es posible que algunos resultados provisionales incluyan solo un campo `speaker_labels` sin los campos `results` y `result_index`. Si no solicita resultados provisionales, el servicio devuelve resultados finales que incluyen los campos `results` y `result_index` y un solo campo `speaker_labels`.

El servicio envía los resultados finales cuando la secuencia de audio está completa o como respuesta a un tiempo de espera excedido, lo que ocurra primero. El servicio establece el campo `final` en `true` solo para la última palabra de las etiquetas de orador que devuelve en cualquiera de los casos.

### Consideraciones sobre el rendimiento para las etiquetas de orador
{: #speakerLabelsPerformance}

Como se ha indicado anteriormente, la característica de etiquetas de orador está optimizada para conversaciones entre dos personas, como por ejemplo las comunicaciones con un centro de atención al cliente. Por lo tanto, tiene que tener en cuenta los siguientes problemas de rendimiento potenciales:

-   El rendimiento del audio con un solo orador puede ser bajo. Las variaciones en la calidad de audio o en la voz del orador pueden hacer que el servicio identifique oradores adicionales que no están presentes. Estos oradores reciben el nombre de alucinaciones.
-   Del mismo modo, el rendimiento de audio con un orador dominante, como por ejemplo un podcast, puede ser bajo. El servicio tiende a omitir los oradores que hablan poco rato, y también puede producir alucinaciones.
-   El rendimiento del audio con más de seis oradores es indefinido. La característica puede manejar un máximo de seis oradores.
-   El rendimiento de las expresiones cortas puede ser menos preciso que el de las expresiones largas. El servicio produce mejores resultados cuando los participantes hablan durante más tiempo, al menos 30 segundos por orador. La cantidad relativa de audio que está disponible para cada orador también puede afectar al rendimiento.
-   El rendimiento puede degradarse durante los 30 primeros segundos de discurso. Por lo general, mejora a un nivel razonable después de 1 minuto de audio.

Al igual que sucede con toda la transcripción, el rendimiento también puede verse afectado por la mala calidad del audio, por el ruido de fondo, por la forma de expresarse de una persona, por el solapamiento de oradores y por otros aspectos del audio.

## Detección de palabras clave
{: #keyword_spotting}

La característica de detección de palabra clave detecta las series especificadas en una transcripción. El servicio puede detectar la misma palabra clave varias veces e informar de cada aparición. El servicio solo detecta las palabras clave en los resultados finales, no en los resultados provisionales. De forma predeterminada, el servicio no realiza la detección de palabras clave.

Para utilizar la detección de palabras clave, debe especificar los siguientes parámetros:

-   Utilice el parámetro `keywords` para especificar una matriz de las series que se van a detectar. El servicio no detecta ninguna palabra clave si omite el parámetro o si especifica una matriz vacía. Una serie de palabra clave puede incluir más de una señal. Por ejemplo, la palabra clave `Speech to Text` tiene tres señales.

    En el caso de inglés de EE. UU., el servicio normaliza cada palabra clave para ajustar series habladas con series escritas. Por ejemplo, normaliza los números para que se ajusten a la forma en que se pronuncian, no a la forma en que se escriben. En el caso de otros idiomas, las palabras clave se deben especificar tal como se pronuncian.

    Puede detectar un máximo de 1000 palabras clave con una sola solicitud. Las palabras clave no distinguen entre mayúsculas y minúsculas.
-   Utilice el parámetro `keywords_threshold` para especificar una probabilidad comprendida entre 0,0 y 1,0 de que haya una coincidencia de palabras clave. El umbral indica el límite inferior del nivel de confianza que debe tener el servicio para que una palabra coincida con una palabra clave. Una palabra clave solo se detecta en la transcripción si su nivel de confianza es mayor o igual que el umbral especificado.

    La especificación de un umbral bajo puede producir potencialmente muchas coincidencias. Si especifica un umbral, también debe especificar una o varias palabras clave. Omita el parámetro para que no se devuelva ninguna coincidencia.

La detección de palabras clave es necesaria para identificar palabras clave en una secuencia de audio. No se pueden identificar palabras clave procesando una transcripción final porque la transcripción representa los mejores resultados de decodificación del servicio para el audio de entrada. No incluye las señales con puntuaciones de confianza bajas que puedan representar una palabra de interés. Por lo tanto, es posible que la aplicación de herramientas de proceso de texto a una transcripción en el lado del cliente no identifique palabras clave. Se necesita una representación más rica de los resultados de la decodificación, y dicha representación solo está disponible en el servidor.

### Resultados de la detección de palabras clave
{: #keywordSpottingResults}

El servicio devuelve los resultados en un campo `keywords_result` que es un elemento de la matriz `results`. El campo `keywords_result` es un diccionario, o una matriz asociativa, de propiedades enumerables. Cada propiedad se identifica mediante una palabra clave especificada e incluye una matriz de objetos. El servicio devuelve un elemento de la matriz para cada coincidencia que encuentra para la palabra clave. El objeto de cada coincidencia incluye los campos siguientes:

-   `normalized_text` es la palabra clave especificada normalizada con la frase hablada que coincide con el audio de entrada.
-   `start_time` es la hora de inicio en segundos de la coincidencia.
-   `end_time` es la hora de finalización en segundos de la coincidencia.
-   `confidence` es el nivel de confianza del servicio en que la coincidencia representa la palabra clave especificada. La confianza debe tener al menos el valor del umbral especificado para que se incluya en los resultados.

Una palabra clave para la que el servicio no encuentra coincidencias se omite de la matriz. Es posible que no se encuentre una palabra clave si

-   El audio simplemente no incluye la palabra clave. La ausencia de la palabra clave es la explicación más obvia.
-   El umbral definido es demasiado alto. Puede que el servicio identifique la palabra, pero con un nivel bajo de confianza, en cuyo caso omite la coincidencia de los resultados.
-   Una serie de palabra clave que contiene varias señales (por ejemplo, `Speech to Text`) se pronuncia demasiado bajo entre sus señales. Cuando el servicio transcribe el audio, divide la secuencia en una serie de bloques. Cada bloque representa una porción continua de audio que no tiene un intervalo de silencio que supera la mitad de un segundo. Crea en una matriz de objetos de resultados que consta de estos bloques.

    El servicio solo detecta una coincidencia de una palabra clave de varias señales si

    -   Las señales de la palabra clave se encuentran en el mismo bloque.
    -   Las señales son adyacentes o están separadas por un intervalo que no supera 0,1 segundo.

    Este último caso se puede producir si hay una breve muletilla o una expresión no léxica, como "uhm" o "well", entre dos señales de la palabra clave. Para obtener más información, consulte [Marcadores de duda](/docs/services/speech-to-text?topic=speech-to-text-basic-response#hesitation).

### Ejemplo de detección de palabras clave
{: #keywordSpottingExample}

La siguiente solicitud de ejemplo establece el parámetro `keywords` en una matriz codificada en URL de tres series (`colorado`, `tornado` y `tornadoes`) y el parámetro `keywords_threshold` en `0.5`. El servicio localiza apariciones de `colorado` y de `tornadoes`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?keywords=%22colorado%22%2C%22tornado%22%2C%22tornadoes%22&keywords_threshold=0.5"
```
{: pre}

```javascript
{
  "results": [
    {
      "keywords_result": {
        "colorado": [
          {
            "normalized_text": "Colorado",
            "start_time": 4.94,
            "confidence": 0.91,
            "end_time": 5.62
          }
        ],
        "tornadoes": [
          {
            "normalized_text": "tornadoes",
            "start_time": 1.52,
            "confidence": 1.0,
            "end_time": 2.15
          }
        ]
      },
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

## Número máximo de alternativas
{: #max_alternatives}

El parámetro `max_alternatives` acepta un valor entero que indica al servicio que devuelva las *n* mejores hipótesis alternativas para los resultados. De forma predeterminada, el servicio solo devuelve un resultado de la transcripción, lo que equivale a establecer el parámetro en `1`. Si se especifica para `max_alternatives` un número mayor que 1, se solicita al servicio que devuelva este número de las mejores transcripciones alternativas. (Si especifica `0`, el servicio utiliza el valor predeterminado, `1`.)

El servicio solo indica una puntuación de confianza para la mejor alternativa que devuelve. En la mayoría de los casos, esa es la alternativa a elegir.

### Ejemplo de número máximo de alternativas
{: #maximumAlternativesExample}

La solicitud de ejemplo siguiente establece el parámetro `max_alternatives` en `3`; el servicio indica un nivel de confianza para la más probable de las tres alternativas.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?max_alternatives=3"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touched down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado and Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Resultados provisionales
{: #interim}

La característica de resultados provisionales solo está disponible con la interfaz WebSocket.
{: note}

Los resultados provisionales son hipótesis intermedias de una transcripción que es probable que cambien antes de que el servicio devuelva sus resultados finales. El servicio devuelve resultados provisionales en cuanto los genera. Los resultados provisionales resultan útiles para

-   Aplicaciones interactivas y transcripción en tiempo real
-   Secuencias de audio largas, que pueden tardar bastante en transcribirse

Los resultados provisionales llegan más a menudo y con más rapidez que los resultados finales. Puede utilizarlos para permitir que la aplicación responda con mayor rapidez o para calibrar el progreso de la transcripción.

Para recibir resultados provisionales, establezca el parámetro JSON `interim_results` en `true` en el mensaje `start` de una solicitud de reconocimiento de WebSocket. Si omite el parámetro `interim_results` o si lo establece en `false`, el servicio solo devuelve una transcripción JSON al final del audio. Siga estas directrices para utilizar el parámetro:

-   Omita el parámetro o establézcalo en `false` si está realizando una transcripción fuera de línea o por lotes, no necesita una latencia mínima y desea un solo resultado JSON para todo el audio.
-   Establezca el parámetro en `true` si desea que los resultados lleguen de forma progresiva a medida que el servicio procesa el audio o si desea resultados con una latencia mínima. Tenga en cuenta que el servicio puede actualizar los resultados provisionales a medida que procesa más audio.

Los resultados provisionales se indican en la respuesta JSON con el campo `final` establecido en `false`. El servicio puede actualizar estos resultados con transcripciones más precisas a medida que procesa más audio. Los resultados finales se identifican con el campo `final` establecido en `true`. El servicio no realiza actualizaciones adicionales a los resultados finales.

### Ejemplo de resultados provisionales
{: #interimResultsExample}

En el siguiente ejemplo abreviado se solicitan resultados provisionales para una solicitud de WebSocket. En su respuesta, el servicio establece el atributo `final` en `true` solo para el resultado final.

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token;
var websocket = new WebSocket(wsURI);

websocket.onopen = function(evt) { onOpen(evt) };
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050',
    interim_results: true
  };
  websocket.send(JSON.stringify(message));
  websocket.send(blob);
}

websocket.onmessage = function(evt) { onMessage(evt) };
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several to "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  . . .
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
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

## Alternativas a palabras
{: #word_alternatives}

La característica de alternativas a palabras (también conocida como Confusion Networks, Redes de confusión) indica hipótesis para alternativas acústicamente parecidas de palabras de la entrada de audio. Por ejemplo, la palabra `Austin` puede ser la mejor hipótesis para una palabra del audio. Pero la palabra `Boston` es otra hipótesis posible en el mismo intervalo de tiempo. Las hipótesis comparten una hora de inicio y de finalización comunes, pero tienen distintas ortografías y generalmente distintas puntuaciones de confianza.

De forma predeterminada, el servicio no indica alternativas a palabras. Para indicar que desea recibir hipótesis alternativas, utilice el parámetro `word_alternatives_threshold` para especificar una probabilidad comprendida entre 0,0 y 1,0. El umbral indica el límite inferior del nivel de confianza que debe tener el servicio en una hipótesis para que la devuelva como una alternativa a una palabra. Solo se devuelve una hipótesis si su nivel de confianza es mayor o igual que el umbral especificado.

Se puede considerar que las alternativas a palabras constituyen una línea de tiempo para una transcripción dividida en intervalos más pequeños, o bandejas. Cada bandeja puede tener una o varias hipótesis con distintas ortografías y diferentes niveles de confianza. El parámetro `word_alternatives_threshold` controla la densidad de los resultados que devuelve el servicio. La especificación de un umbral bajo puede producir potencialmente muchas hipótesis.

### Resultados de alternativas a palabras
{: #wordAlternativesResults}

El servicio devuelve los resultados en un campo `word_alternatives` que es un elemento de la matriz `results`. El campo `word_alternatives` es una matriz de objetos, cada uno de los cuales proporciona los campos siguientes para una palabra alternativa:

-   `start_time` indica la hora en segundos en que comienza la palabra para la que se devuelven hipótesis en el audio de entrada.
-   `end_time` indica la hora en segundos en que finaliza la palabra para la que se devuelven hipótesis en el audio de entrada.
-   `alternatives` es una matriz de objetos de hipótesis. Cada objeto incluye un valor de `confidence` que indica la puntuación de confianza del servicio para la hipótesis y un valor `word` que identifica la hipótesis. La confianza debe tener al menos el valor del umbral especificado para que se incluya en los resultados. El servicio ordena las alternativas por nivel de confianza.

### Ejemplos de alternativas a palabras
{: #wordAlternativesExample}

En la siguiente solicitud de ejemplo se especifica un valor de `word_alternatives_threshold` de `0.9`. La salida incluye hipótesis potenciales y puntuaciones de confianza para varias palabras, junto con sus horas de inicio y de finalización.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.9"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 1.01,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "several"
            }
          ],
          "end_time": 1.52
        },
        {
          "start_time": 1.52,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "tornadoes"
            }
          ],
          "end_time": 2.15
        },
        {
          "start_time": 3.39,
          "alternatives": [
            {
              "confidence": 0.96,
              "word": "severe"
            }
          ],
          "end_time": 3.77
        },
        {
          "start_time": 3.77,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "thunderstorms"
            }
          ],
          "end_time": 4.51
        },
        {
          "start_time": 4.51,
          "alternatives": [
            {
              "confidence": 0.97,
              "word": "swept"
            }
          ],
          "end_time": 4.81
        }
      ],
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

## Confianza de palabras
{: #word_confidence}

El parámetro `word_confidence` indica al servicio si se deben proporcionar medidas de confianza para las palabras de la transcripción. De forma predeterminada, el servicio incida una medida de confianza solo para la transcripción final en conjunto. Si se establece `word_confidence` en `true`, se indica al servicio que informe de una medida de confianza para cada palabra individual de la transcripción.

Una medida de confianza indica la estimación del servicio de que la palabra transcrita es correcta en base a la evidencia acústica. Las puntuaciones de confianza van de 0,0 a 1,0.

-   Una puntuación de 1,0 indica que la transcripción actual de la palabra refleja el resultado más probable.
-   Una puntuación de 0,5 significa que la palabra tiene un 50 por ciento de probabilidad de ser correcta.

### Ejemplo de confianza de palabras
{: #wordConfidenceExample}

En el ejemplo siguiente se solicitan puntuaciones de confianza de palabras para las palabras de la transcripción.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_confidence=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday ",
          "word_confidence": [
            [
              "several",
              1.0
            ],
            [
              "tornadoes",
              1.0
            ],
            [
              "touch",
              0.52
            ],
            [
              "down",
              0.90
            ],
            . . .
            [
              "on",
              0.31
            ],
            [
              "Sunday",
              0.99
            ]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Indicaciones de fecha y hora de palabras
{: #word_timestamps}

El parámetro `timestamps` indica al servicio si se deben generar indicaciones de fecha y hora para las palabras que transcribe. De forma predeterminada, el servicio no indica fecha y hora. Si se establece `timestamps` en `true`, el servicio indica la hora de inicio y de finalización, en segundos, para cada palabra en relación con el principio del audio.

### Ejemplo de indicación de fecha y hora de palabras
{: #wordTimestampsExample}

En el ejemplo siguiente se solicitan indicaciones de fecha y hora para las palabras de la transcripción.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?timestamps=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "several",
              1.01,
              1.52
            ],
            [
              "tornadoes",
              1.52,
              2.15
            ],
            [
              "touch",
              2.15,
              2.5
            ],
            [
              "down",
              2.5,
              2.81
            ],
            . . .
            [
              "on",
              5.62,
              5.74
            ],
            [
              "Sunday",
              5.74,
              6.34
            ]
          ],
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

## Formateo inteligente
{: #smart_formatting}

La característica de formateo inteligente es una funcionalidad beta que está disponible en inglés de EE. UU., japonés y español.
{: note}

El parámetro `smart_formatting` indica al servicio que convierta las series siguientes en representaciones más convencionales:

-   Fechas
-   Horas
-   Serie de dígitos y números
-   Números de teléfono
-   Valores de moneda
-   Direcciones web y direcciones de correo electrónico de internet

Para habilitar el formateo inteligente, establezca el parámetro `smart_formatting` en `true`. De forma predeterminada, el servicio no realiza el formateo inteligente.

El servicio solo aplica el formateo inteligente a la transcripción final de una solicitud de reconocimiento. Aplica el formateo inteligente justo antes de devolver los resultados al cliente, cuando la normalización del texto se ha completado. La conversión facilita la lectura de la transcripción y permite un mejor proceso posterior de los resultados de la transcripción.

### Puntuación (inglés de EE. UU.)
{: #smartFormattingPunctuation}

En el caso de inglés de EE. UU., la característica indica al servicio que sustituya los signos de puntuación por las siguientes series de palabras clave habladas en el audio.

<table style="width:50%">
  <caption>Tabla 2. Palabras clave de puntuación del formateo inteligente</caption>
  <tr>
    <th style="width:45%; text-align:left">Serie de palabras clave</th>
    <th style="text-align:center">Puntuación resultante</th>
  </tr>
  <tr>
    <td>
      Coma
    </td>
    <td style="text-align:center">
      <code>,</code>
    </td>
  </tr>
  <tr>
    <td>
      Punto
    </td>
    <td style="text-align:center">
      <code>.</code>
    </td>
  </tr>
  <tr>
    <td>
      Signo de interrogación
    </td>
    <td style="text-align:center">
      <code>?</code>
    </td>
  </tr>
  <tr>
    <td>
      Signo de exclamación
    </td>
    <td style="text-align:center">
      <code>!</code>
    </td>
  </tr>
</table>

El servicio convierte estas series de palabras clave en símbolos solo en los lugares adecuados. Por ejemplo, el servicio convierte la frase hablada

```
the warranty period is short period
```
{: codeblock}

en el texto siguiente en una transcripción

```
the warranty period is short.
```
{: codeblock}

### Diferencias entre idiomas
{: #smartFormattingDifferences}

El formateo inteligente se basa en la presencia de palabras clave obvias en la transcripción. Los idiomas soportados manejan el formateo inteligente de forma ligeramente diferente.

#### Inglés de EE. UU. y español
{: #smartFormattingEnglishSpanish}

-   Las horas se identifican mediante palabras clave como, por ejemplo, `AM`, `PM` o `EST`.
-   Las horas en formato de 24 horas se convierten si se identifican mediante la palabra clave `hours`.
-   Los números de teléfono deben ser `911` o un número con 10 u 11 dígitos que comience por el número `1`.

#### Japonés
{: #smartFormattingJapanese}

-   Las direcciones web y las direcciones de correo electrónico de internet no se convierten.
-   Los números de teléfono deben tener 10 u 11 dígitos y deben comenzar por prefijos válidos para números de teléfono en Japón. Por ejemplo, algunos de los prefijos válidos son `03` y `090`.
-   Las palabras en inglés se convierten en caracteres ASCII (*hankaku*). Por ejemplo, <code>&#65321;&#65314;&#65325;</code> se convierte en `IBM`.
-   Es posible que los términos ambiguos no se conviertan si no hay suficiente contexto disponible. Por ejemplo, no está claro si <code>&#19968;&#26178;</code> y <code>&#21313;&#20998;</code> corresponden a horas.
-   La puntuación se gestiona del mismo modo con o sin formateo inteligente. Por ejemplo, en función de cálculos de probabilidades, se selecciona uno de los valores <code>&#12459;&#12531;&#12510;</code> o `,`.

### Ejemplo de formateo inteligente
{: #smartFormattingExample}

En el ejemplo siguiente se solicita el formateo inteligente con una solicitud de reconocimiento estableciendo el parámetro `smart_formatting` en `true`. En las secciones siguientes se muestran los efectos del formateo inteligente en los resultados de una solicitud.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?smart_formatting=true"
```
{: pre}

### Resultados del formateo inteligente
{: #smartFormattingResults}

En la tabla siguiente se muestran ejemplos de transcripciones finales con y sin formateo inteligente. Las transcripciones se basan en un audio en inglés de Estados Unidos.

<table summary="Cada fila de cabecera va seguida de varias filas de ejemplo que muestran el efecto del formateo inteligente sobre el elemento definido en la cabecera.">
  <caption>Tabla 3. Transcripciones de ejemplo de formateo inteligente</caption>
  <tr>
    <th id="without_formatting" style="width:45%; text-align:left">Sin formateo inteligente</th>
    <th id="with_formatting" style="text-align:left">Con formateo inteligente</th>
  </tr>
  <tr>
    <th id="Dates" colspan="2">
      <strong>Fechas</strong>
    </th>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on ten oh six nineteen seventy
    </td>
    <td headers="Dates with_formatting">
      I was born on 10/6/1970
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on the ninth of December nineteen hundred
    </td>
    <td headers="Dates with_formatting">
      I was born on 12/9/1900
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      Today is June sixth
    </td>
    <td headers="Dates with_formatting">
      Today is June 6
    </td>
  </tr>
  <tr>
    <th id="Times" colspan="2">
      <strong>Horas</strong>
    </th>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      The meeting starts at nine thirty AM
    </td>
    <td headers="Times with_formatting">
      The meeting starts at 9:30 AM
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      I am available at seven EST
    </td>
    <td headers="Times with_formatting">
      I am available at 7:00 EST
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      We meet at oh seven hundred hours
    </td>
    <td headers="Times with_formatting">
      We meet at 0700 hours
    </td>
  </tr>
  <tr>
    <th id="Numbers" colspan="2">
      <strong>Números</strong>
    </th>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      The quantity is one million one hundred and one
    </td>
    <td headers="Numbers with_formatting">
      The quantity is 1000101
    </td>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      One point five is between one and two
    </td>
    <td headers="Numbers with_formatting">
      1.5 is between 1 and 2
    </td>
  </tr>
  <tr>
    <th id="phone_numbers" colspan="2">
      <strong>Números de teléfono</strong>
    </th>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at nine one four two three seven one thousand
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 914-237-1000
    </td>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at one nine one four nine oh nine twenty six forty five
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 1-914-909-2645
    </td>
  </tr>
  <tr>
    <th id="currency_values" colspan="2">
      <strong>Valores de moneda</strong>
    </th>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      You owe me three thousand two hundred two dollars and sixty six
      cents
    </td>
    <td headers="currency_values with_formatting">
      You owe me $3202.66
    </td>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      The dollar rose to one hundred and nine point seven nine yen from
      one hundred and nine point seven two yen
    </td>
    <td headers="currency_values with_formatting">
      The dollar rose to 109.79 yen from 109.72 yen
    </td>
  </tr>
  <tr>
    <th id="internet_addresses" colspan="2">
      <strong>Direcciones web y direcciones de correo electrónico de internet</strong>
    </th>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      My email address is john dot doe at foo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      My email address is john.doe at foo.com
    </td>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      I saw the story on yahoo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      I saw the story on yahoo.com
    </td>
  </tr>
  <tr>
    <th id="Combinations" colspan="2">
      <strong>Combinaciones</strong>
    </th>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      The code is zero two four eight one and the date of service
      is May fifth two thousand and one
    </td>
    <td headers="Combinations with_formatting">
      The code is 02481 and the date of service is 5/5/2001
    </td>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      There are forty seven links on Yahoo dot com now
    </td>
    <td headers="Combinations with_formatting">
      There are 47 links on Yahoo.com now
    </td>
  </tr>
</table>

### Transcripciones de ejemplo con pausas largas
{: #smartFormattingLongPauses}

En los casos en los que una expresión contiene silencios suficientemente largos, el servicio puede dividir la transcripción en dos o más porciones finales. Esto afecta a los resultados del formateo, tal como se muestra en los ejemplos siguientes.

<table>
  <caption>Tabla 4. Transcripciones de ejemplo de formateo inteligente para pausas largas</caption>
  <tr>
    <th style="width:45%; text-align:left">Conversación de audio</th>
    <th style="text-align:left">Resultados de la transcripción formateada</th>
  </tr>
  <tr>
    <td>
      My phone number is nine one four five five seven three
      three nine two
    </td>
    <td>
      "My phone number is 914-557-3392"
    </td>
  </tr>
  <tr>
    <td>
      My phone number is nine one four <em>&lt;pause&gt;</em> five five
      seven three three nine two
    </td>
    <td>
      "My phone number is 914"<br/>
      "5573392"
    </td>
  </tr>
</table>

## Ocultación numérica
{: #redaction}

La característica de ocultación numérica es una funcionalidad beta que está disponible en inglés de EE. UU., japonés y coreano.
{: note}

El parámetro `redaction` indica al servicio que oculte, o enmascare, datos numéricos de las transcripciones finales. La característica oculta cualquier número que tenga tres o más dígitos consecutivos sustituyendo cada dígito por el carácter `X`. Está característica está pensada para ocultar datos numéricos confidenciales, como números de tarjetas de crédito.

De forma predeterminada, el servicio no oculta los datos numéricos. Establezca el parámetro `redaction` en `true` para habilitar la ocultación numérica. Cuando se habilita la ocultación, el servicio habilita automáticamente el formateo inteligente, independientemente de si se ha habilitado de forma explícita esta característica. Para garantizar la máxima seguridad, el servicio también impone los siguientes valores de parámetros cuando se habilita la ocultación:

-   El servicio desactiva la detección de palabras clave, independientemente de si especifica valores para los parámetros `keywords` y `keywords_threshold`.
-   El servicio inhabilita los resultados provisionales, independientemente de si se establece el parámetro `interim_results` de la interfaz WebSocket en `true`.
-   El servicio solo devuelve una transcripción final, independientemente de si se especifica un valor para el parámetro `maximum_alternatives`.

El diseño de la característica iguala la característica de formateo inteligente existente. El servicio solo aplica la ocultación a la transcripción final de una solicitud de reconocimiento, justo antes de devolver los resultados al cliente y después de que se complete la normalización del texto.

Las futuras versiones de la característica pueden ampliar la ocultación para enmascarar todos los números de teléfono, direcciones, cuentas, bancarias, números de seguridad social y otros datos numéricos confidenciales.
{: note}

### Diferencias entre idiomas
{: #redactionDifferences}

La característica funciona exactamente tal como se describe para los modelos en inglés de EE. UU., pero tiene las siguientes diferencias para los modelos en japonés y en coreano.

#### Japonés
{: #redactionJapanese}

La ocultación en japonés tiene las siguientes diferencias:

-   Además de enmascarar series de tres o más dígitos consecutivos, la ocultación también enmascara direcciones y números, aunque contengan menos de tres dígitos.
-   Del mismo modo, la ocultación también enmascara información de fechas en fechas de nacimiento de estilo japonés. En japonés, la información de fecha se suele presentar en el formato latín *Anno Domini* (d. C.), pero a veces sigue el estilo japonés, en particular para las fechas de nacimiento. En este caso, el año y el mes se enmascaran, aunque solo contienen uno o dos dígitos. Por ejemplo, la ocultación numérica cambia la siguiente serie tal como se muestra.

    <table style="width:50%">
      <caption>Tabla 5. Ejemplo de la ocultación de la fecha de nacimiento de estilo japonés</caption>
      <tr>
        <th style="text-align:left">Sin ocultación</th>
        <th style="text-align:left">Con ocultación</th>
      </tr>
      <tr>
        <td>
          &#24179;&#25104; 30&#24180; 2&#26376;
        </td>
        <td>
          &#24179;&#25104; XX&#24180; X&#26376;
        </td>
      </tr>
    </table>

#### Coreano
{: #redactionKorean}

La ocultación en coreano tiene las siguientes diferencias:

-   La característica de formateo inteligente no recibe soporte. El servicio sigue efectuando ocultaciones numéricas para coreano, pero no realiza ningún otro formateo inteligente.
-   Los caracteres digitales aislados se ocultan, pero los posibles caracteres digitales incluidos como parte de frases en coreano no se ocultan. Por ejemplo, el carácter <code>&#51060;</code> en la siguiente frase no se sustituye por una `X` porque es adyacente al siguiente carácter:

    <code>&#51060;&#51077;&#45768;&#45796;</code>

    Si el carácter <code>&#51060;</code> estuviera separado del siguiente carácter por un espacio, se sustituiría por una `X`, tal como se escribe en el apartado [Resultados de la ocultación numérica](#redactionResults).

### Ejemplo de ocultación numérica
{: #redactionExample}

En el ejemplo siguiente se solicita la ocultación numérica con una solicitud de reconocimiento estableciendo el parámetro `redaction` en `true`. Debido a que la solicitud habilita la ocultación, el servicio habilita implícitamente el formateo inteligente con la solicitud. El servicio inhabilita de forma efectiva los otros parámetros de la solicitud para que no tengan ningún efecto: el servicio devuelve una sola transcripción final y no reconoce ninguna palabra clave. En la sección siguiente se muestran los efectos de la ocultación.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?&redaction=true&max_alternatives=3&keywords=%22birth%22%2C%22birthday%22&keywords_threshold=0.5"
```
{: pre}

### Resultados de la ocultación numérica
{: #redactionResults}

En la tabla siguiente se muestran ejemplos de transcripciones finales con y sin ocultación numérica en cada idioma soportado.

<table style="width:90%" summary="Cada fila de cabecera identifica un idioma y va seguida de una sola fila que muestra la misma transcripción con y sin la ocultación numérica habilitada.">
  <caption>Tabla 6. Transcripciones de ejemplo de ocultación numérica</caption>
  <tr>
    <th id="without_redaction" style="text-align:left">Sin ocultación</th>
    <th id="with_redaction" style="text-align:left">Con ocultación</th>
  </tr>
  <tr>
    <th id="US_English" colspan="2">
      <strong>Inglés de EE. UU.</strong>
    </th>
  </tr>
  <tr>
    <td headers="US_English without_redaction">
      my credit card number is four one four seven two
    </td>
    <td headers="US_English with_redaction">
      my credit card number is XXXXX
    </td>
  </tr>
  <tr>
    <th id="Japanese" colspan="2">
      <strong>Japonés</strong>
    </th>
  </tr>
  <tr>
    <td headers="Japanese without_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; &#22235; &#19968; &#22235; &#19971; &#20108;&#12391;&#12377;
    </td>
    <td headers="Japanese with_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; XXXXX &#12391;&#12377;
    </td>
  </tr>
  <tr>
    <th id="Korean" colspan="2">
      <strong>Coreano</strong>
    </th>
  </tr>
  <tr>
    <td headers="Korean without_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; &#49324; &#51068; &#49324; &#52832; &#51060; &#48264;&#51077;&#45768;&#45796;
    </td>
    <td headers="Korean with_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; XXXXX &#48264;&#51077;&#45768;&#45796;
    </td>
  </tr>
</table>

## Filtrado de lenguaje obsceno
{: #profanity_filter}

La característica de filtrado de lenguaje obsceno solo está disponible a nivel general para inglés de Estados Unidos.
{: note}

El parámetro `profanity_filter` indica si el servicio va a censurar el lenguaje obsceno en sus resultados. De forma predeterminada, el servicio oculta el lenguaje obsceno y lo sustituye por una serie de asteriscos en la transcripción. Si se establece el parámetro en `false`, se visualizan las palabras en la salida exactamente tal y como se transcriben.

El servicio censura el lenguaje obsceno de todas las transcripciones finales y de todas las transcripciones alternativas. También censura el lenguaje obsceno de los resultados asociados con alternativas a palabras, con niveles de confianza de las palabras y con indicaciones de fecha y hora. La única excepción es la detección de palabras clave, para la que el servicio devuelve todas las palabras tal como las especifica el usuario, independientemente de si el valor de `profanity_filter` es `true`.

### Ejemplo de filtrado de lenguaje obsceno
{: #profanityFilteringExample}

En el ejemplo siguiente se muestran los resultados de un breve archivo de audio que se transcribe con el valor `true` predeterminado para el parámetro `profanity_filter`. La solicitud también establece el parámetro `word_alternatives_threshold` en un valor relativamente alto, `0.99`, y los parámetros `word_confidence` y `timestamps` en `true`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.99&word_confidence=true&timestamps=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 0.03,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "****"
            }
          ],
          "end_time": 0.25
        },
        {
          "start_time": 0.25,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "you"
            }
          ],
          "end_time": 0.56
        }
      ],
      "alternatives": [
        {
          "transcript": "**** you",
          "confidence": 0.99,
          "word_confidence": [
            ["****", 1.0],
            ["you", 0.99]
          ],
          "timestamps": [
            ["****", 0.03, 0.25],
            ["you", 0.25, 0.56]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}
