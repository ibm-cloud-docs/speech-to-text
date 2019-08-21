---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-10"

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

# La interfaz HTTP síncrona
{: #http}

La interfaz HTTP síncrona del servicio {{site.data.keyword.speechtotextfull}} proporciona un solo método `POST /v1/recognize` para solicitar el reconocimiento de voz con el servicio. Este método es el medio más sencillo para obtener una transcripción. Ofrece dos formas de enviar una solicitud de reconocimiento de voz:
{: shortdesc}

-   La primera envía todo el audio en una sola secuencia mediante el cuerpo de la solicitud. Los parámetros de la operación se especifican como cabeceras de solicitud y parámetros de consulta. Para obtener más información, consulte [Cómo realizar una solicitud HTTP básica](#HTTP-basic).
-   La segunda envía el audio como una solicitud de varias partes. Los parámetros de la solicitud se especifican como una combinación de cabeceras de solicitud, parámetros de consulta y metadatos JSON. Para obtener más información, consulte [Cómo realizar una solicitud HTTP de varias partes](#HTTP-multi).

Envíe un máximo de 100 MB y un mínimo de 100 bytes de datos de audio con una sola solicitud. Para obtener información acerca de los formatos de audio y sobre el uso de la compresión para maximizar la cantidad de audio que puede enviar con una solicitud, consulte [Formatos de audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats). Para obtener información sobre todos los métodos de interfaz HTTP, consulte la [Referencia de API](https://{DomainName}/apidocs/speech-to-text){: external}.

## Cómo realizar una solicitud HTTP básica
{: #HTTP-basic}

El método HTTP `POST /v1/recognize` proporciona un medio sencillo de transcribir audio. El usuario pasa todo el audio mediante el cuerpo de la solicitud y solicita y especifica los parámetros como cabeceras de solicitud y parámetros de consulta.

En el siguiente ejemplo de `curl` se envía una solicitud de reconocimiento para un solo archivo FLAC denominado `audio-file.flac`. La solicitud omite el parámetro de consulta `model` para utilizar el modelo de lenguaje predeterminado, `en-US_BroadbandModel`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

El ejemplo devuelve la siguiente transcripción para el audio:

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

El método `POST /v1/recognize` solo devuelve resultados después de procesar todo el audio correspondiente a una solicitud. El método resulta adecuado para el proceso por lotes, pero no para el reconocimiento de voz en directo. Utilice la interfaz WebSocket para transcribir audio en directo.

Si los datos constan de varios archivos de audio, el método recomendado para enviar el audio consiste en enviar varias solicitudes, una para cada archivo de audio. Puede enviar las solicitudes en un bucle, y si lo desea puede utilizar paralelismo para mejorar el rendimiento. También puede utilizar el reconocimiento de voz de varias partes para pasar varios archivos de audio con una sola solicitud.

## Cómo realizar una solicitud HTTP de varias partes
{: #HTTP-multi}

El método `POST /v1/recognize` también da soporte a las solicitudes de varias partes. Los datos de audio se pasan como datos con formato de varias partes. El usuario especifica algunos parámetros como cabeceras de consulta y parámetros de consulta, pero los metadatos JSON se pasan como datos de formulario para controlar la mayoría de los aspectos de la transcripción.

El reconocimiento de voz de varias partes está pensado para los siguientes casos de uso:

-   Para pasar varios archivos de audio con una sola solicitud de reconocimiento de voz.
-   Con navegadores para los que JavaScript está inhabilitado. Las solicitudes de varias partes basadas en datos de formulario no requieren el uso de JavaScript.
-   Cuando los parámetros de una solicitud de reconocimiento ocupan más de 8 KB, que es el límite impuesto por la mayoría de los servidores HTTP y los proxies. Por ejemplo, la detección de un gran número de palabras clave puede aumentar el tamaño de una solicitud más allá de este límite. Las solicitudes de varias partes utilizan datos de formulario para evitar esta restricción.

En las secciones siguientes se describen los parámetros que se utilizan para solicitudes de varias partes y se muestra una solicitud de ejemplo.

### Parámetros para las solicitudes de varias partes
{: #multipartParameters}

Los siguientes parámetros de reconocimiento de voz de varias partes se especifican como cabeceras de solicitud, parámetros de consulta y datos de formulario.

<table summary="Cada fila de la tabla describe el uso de un posible parámetro para una solicitud de reconocimiento de varias partes.">
  <caption>Tabla 1. Parámetros para las solicitudes de varias partes</caption>
  <tr>
    <th id="parameter" style="text-align:left; width:20%">Parámetro</th>
    <th id="description" style="text-align:center; width:80%">Descripción</th>
  </tr>
  <tr>
    <td>
      <code>metadata</code>
      <br/><em>Datos de formulario</em>
      <br/><em>Objeto</em>
    </td>
    <td>
      <em>Obligatorio.</em> Un objeto JSON que proporciona los parámetros de la transcripción
      para la solicitud. El objeto debe ser la primera parte de los datos de
      formulario. La información describe el audio en las siguientes partes
      de los datos de formulario. Consulte
      [Metadatos JSON para solicitudes de varias partes](#multipartJSON).
    </td>
  </tr>
  <tr>
    <td>
      <code>upload</code>
      <br/><em>Datos de formulario</em>
      <br/><em>Archivo</em>
    </td>
    <td>
      <em>Obligatorio.</em> Uno o varios archivos de audio como recordatorio de los datos
      de formulario para la solicitud. Todos los archivos de audio deben tener el mismo formato.
      Con el mandato `curl` incluya una opción <code>--form</code> para cada
      archivo de la solicitud.
    </td>
  </tr>
  <tr>
    <td>
      <code>Content-Type</code>
      <br/><em>Cabecera</em>
      <br/><em>Cadena</em>
    </td>
    <td>
      <em>Obligatorio.</em> Especifique `multipart/form-data` para indicar cómo se pasan
      los datos al método. Especifique el tipo de contenido del audio
      con el parámetro JSON `part_content_type`.
    </td>
  </tr>
  <tr>
    <td>
      <code>Transfer-Encoding</code>
      <br/><em>Cabecera</em>
      <br/><em>Cadena</em>
    </td>
    <td>
      <em>Opcional.</em> Especifique `chunked` para enviar los datos de audio en secuencia
      al servicio. Omita el parámetro si envía todo el audio con una sola solicitud.
    </td>
  </tr>
  <tr>
    <td>
      <code>model</code>
      <br/><em>Consulta</em>
      <br/><em>Cadena</em>
    </td>
    <td>
      <em>Opcional.</em> El identificador del modelo que se va a utilizar con
      la solicitud. El valor predeterminado es `en-US_BroadbandModel`.
    </td>
  </tr>
  <tr>
    <td>
      <code>language_customization_id</code>
      <br/><em>Consulta</em>
      <br/><em>Cadena</em>
    </td>
    <td>
      <em>Opcional.</em> El GUID de un modelo de lenguaje personalizado que se va a utilizar
      con la solicitud.
    </td>
  </tr>
  <tr>
    <td>
      <code>acoustic_customization_id</code>
      <br/><em>Consulta</em>
      <br/><em>Cadena</em>
    </td>
    <td>
      <em>Opcional.</em> El GUID de un modelo acústico personalizado que se va a utilizar
      con la solicitud.
    </td>
  </tr>
  <tr>
    <td>
      <code>base_model_version</code>
      <br/><em>Consulta</em>
      <br/><em>Cadena</em>
    </td>
    <td>
      <em>Opcional.</em> La versión del modelo base especificado que se va a utilizar con
      la solicitud.
    </td>
  </tr>
</table>

Para obtener más información acerca de los parámetros de consulta, consulte el [Resumen de parámetros](/docs/services/speech-to-text?topic=speech-to-text-summary).

### Metadatos JSON para solicitudes de varias partes
{: #multipartJSON}

Los metadatos JSON que se pasan con una solicitud de varias partes pueden incluir los campos siguientes:

-   `part_content_type` (serie)
-   `data_parts_count` (entero)
-   `customization_weight` (número)
-   `inactivity_timeout` (entero)
-   `keywords` (serie[])
-   `keywords_threshold` (número)
-   `max_alternatives` (entero)
-   `word_alternatives_threshold` (número)
-   `word_confidence` (booleano)
-   `timestamps` (booleano)
-   `profanity_filter` (booleano)
-   `smart_formatting` (booleano)
-   `speaker_labels` (booleano)
-   `grammar_name` (serie)
-   `redaction` (booleano)

Solo los dos parámetros siguientes son específicos de las solicitudes de varias partes:

-   El campo `part_content_type` es *opcional* para la mayoría de los formatos de audio. Es obligatorio para los formatos `audio/alaw`, `audio/basic`, `audio/l16` y `audio/mulaw`. Especifica el formato del audio en las partes siguientes de la solicitud. Todos los archivos de audio deben tener el mismo formato.
-   El campo `data_parts_count` es *opcional* para todas las solicitudes. Especifica el número de archivos de audio que se envían con la solicitud. El servicio aplica la detección de fin de secuencia a la última (y posiblemente la única) parte de los datos. Si omite el parámetro, el servicio determina el número de partes de la solicitud.

Todos los demás parámetros de los metadatos son opcionales. Para ver descripciones de todos los parámetros disponibles, consulte el [Resumen de parámetros](/docs/services/speech-to-text?topic=speech-to-text-summary).

### Ejemplo de solicitud de varias partes

En el siguiente ejemplo de `curl` se muestra cómo se pasa una solicitud de reconocimiento de varias partes con el método `POST /v1/recognice`. La solicitud pasa dos archivos de audio, **audio-file1.flac** y **audio-file2.flac**. El parámetro `metadata` proporciona la mayoría de los parámetros de la solicitud; los parámetros `upload` proporcionan los archivos de audio.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: multipart/form-data"
--form metadata="{\"part_content_type\":\"application/octet-stream\",
  \"data_parts_count\":2,
  \"timestamps\":true,
  \"word_alternatives_threshold\":0.9,
  \"keywords\":[\"colorado\",\"tornado\",\"tornadoes\"],
  \"keywords_threshold\":0.5}"
--form upload="@{path}audio-file1.flac"
--form upload="@{path}audio-file2.flac"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

El ejemplo devuelve la siguiente transcripción para los archivos de audio. El servicio devuelve los resultados correspondientes a los dos archivos en el orden en el que se envían. (En la salida de ejemplo se han abreviado los resultados para el segundo archivo.)

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 0.03,
          "alternatives": [
            {
              "confidence": 0.96,
                     "word": "the"
                  }
          ],
               "end_time": 0.09
        },
        {
          "start_time": 0.09,
               "alternatives": [
                  {
              "confidence": 0.96,
                     "word": "latest"
                  }
          ],
               "end_time": 0.62
        },
        {
          "start_time": 0.62,
               "alternatives": [
                  {
              "confidence": 0.96,
                     "word": "weather"
                  }
          ],
               "end_time": 0.87
        },
        {
          "start_time": 0.87,
               "alternatives": [
                  {
              "confidence": 0.96,
                     "word": "report"
                  }
          ],
               "end_time": 1.5
            }
      ],
         "keywords_result": {},
         "alternatives": [
            {
          "timestamps": [
            [
              "the",
                     0.03,
                     0.09
            ],
            [
              "latest",
                     0.09,
                     0.62
            ],
            [
              "weather",
                     0.62,
                     0.87
            ],
            [
              "report",
                     0.87,
                     1.5
                  ]
          ],
               "confidence": 0.99,
               "transcript": "the latest weather report "
            }
      ],
      "final": true
    },
    {
      "word_alternatives": [
        {
          "start_time": 0.15,
               "alternatives": [
                  {
              "confidence": 1.0,
                     "word": "a"
                  }
          ],
               "end_time": 0.3
        },
        {
          "start_time": 0.3,
               "alternatives": [
                  {
              "confidence": 1.0,
                     "word": "line"
                  }
          ],
               "end_time": 0.64
        },
        . . .
        {
          "start_time": 4.58,
               "alternatives": [
                  {
              "confidence": 0.98,
                     "word": "Colorado"
                  }
          ],
               "end_time": 5.16
        },
        {
          "start_time": 5.16,
               "alternatives": [
                  {
              "confidence": 0.98,
                     "word": "on"
                  }
          ],
               "end_time": 5.32
        },
        {
          "start_time": 5.32,
               "alternatives": [
                  {
              "confidence": 0.98,
                     "word": "Sunday"
                  }
          ],
               "end_time": 6.04
            }
      ],
         "keywords_result": {
        "tornadoes": [
               {
            "normalized_text": "tornadoes",
                  "start_time": 3.03,
                  "confidence": 0.98,
                  "end_time": 3.84
               }
        ],
            "colorado": [
               {
            "normalized_text": "Colorado",
                  "start_time": 4.58,
                  "confidence": 0.98,
                  "end_time": 5.16
               }
        ]
      },
      "alternatives": [
        {
          "timestamps": [
            [
              "a",
                     0.15,
                     0.3
            ],
            [
              "line",
                     0.3,
                     0.64
            ],
            . . .
            [
              "Colorado",
                     4.58,
                     5.16
            ],
            [
              "on",
                     5.16,
                     5.32
            ],
            [
              "Sunday",
                     5.32,
                     6.04
                  ]
          ],
               "confidence": 0.99,
               "transcript": "a line of severe thunderstorms with several
possible tornadoes is approaching Colorado on Sunday "
            }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}
