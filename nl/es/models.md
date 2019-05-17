---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-07"

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

# Idiomas y modelos
{: #models}

El servicio {{site.data.keyword.speechtotextfull}} da soporte al reconocimiento de voz en muchos idiomas. Para todas las interfaces, puede utilizar el parámetro `model` para especificar el modelo para una solicitud de reconocimiento de voz. El modelo indica el idioma en el que se habla el audio y la velocidad a la que se ha muestreado.
{: shortdesc}

## Modelos de lenguaje soportados
{: #modelsList}

Para la mayoría de los idiomas, el servicio da soporte a los modelos de banda ancha y de banda estrecha:

-   Los *modelos de banda ancha* son para audio que se muestrea a una velocidad mayor o igual a 16 kHz. Utilice modelos de banda ancha para aplicaciones en tiempo real, como por ejemplo para aplicaciones de voz en directo.
-   Los *modelos de banda estrecha* son para audio que se muestra a 8 kHz. Utilice modelos de banda estrecha para la decodificación fuera de línea de voz por teléfono, que es el uso típico de esta frecuencia de muestreo.

La elección del modelo adecuado para su aplicación es importante. Utilice el modelo que se ajuste a la frecuencia de muestreo (y al idioma) de su audio. El servicio ajusta automáticamente la frecuencia de muestreo de su audio para que se adapte al modelo que especifique. Para obtener más información, consulte [Frecuencia de muestreo](/docs/services/speech-to-text/audio-formats.html#samplingRate).

Para conseguir la máxima precisión del reconocimiento, también debe tener en cuenta el contenido de la frecuencia del audio. Para obtener más información, consulte [Frecuencia de audio](/docs/services/speech-to-text/audio-formats.html#frequency).
{: tip}

En la Tabla 1 se muestran los modelos soportados para cada idioma. Si omite el parámetro `model` de una solicitud, el servicio utiliza el modelo de banda ancha en inglés de Estados Unidos, `en-US_BroadbandModel`, de forma predeterminada.

<table>
  <caption>Tabla 1. Modelos de lenguaje soportados</caption>
  <tr>
    <th style="text-align:left">Idioma</th>
    <th style="text-align:center">Modelo de banda ancha</th>
    <th style="text-align:center">Modelo de banda estrecha</th>
  </tr>
  <tr>
    <td>Portugués de Brasil</td>
    <td style="text-align:center"><code>pt-BR_BroadbandModel</code></td>
    <td style="text-align:center"><code>pt-BR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Francés</td>
    <td style="text-align:center"><code>fr-FR_BroadbandModel</code></td>
    <td style="text-align:center"><code>fr-FR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Alemán</td>
    <td style="text-align:center"><code>de-DE_BroadbandModel</code></td>
    <td style="text-align:center"><code>de-DE_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Japonés</td>
    <td style="text-align:center"><code>ja-JP_BroadbandModel</code></td>
    <td style="text-align:center"><code>ja-JP_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Coreano</td>
    <td style="text-align:center"><code>ko-KR_BroadbandModel</code></td>
    <td style="text-align:center"><code>ko-KR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Chino mandarín</td>
    <td style="text-align:center"><code>zh-CN_BroadbandModel</code></td>
    <td style="text-align:center"><code>zh-CN_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Árabe estándar moderno</td>
    <td style="text-align:center"><code>ar-AR_BroadbandModel</code></td>
    <td style="text-align:center">No soportado</td>
  </tr>
  <tr>
    <td>Español</td>
    <td style="text-align:center"><code>es-ES_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-ES_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Inglés del Reino Unido</td>
    <td style="text-align:center"><code>en-GB_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-GB_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Inglés de EE. UU.</td>
    <td style="text-align:center"><code>en-US_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-US_NarrowbandModel</code></br>
      <code>en-US_ShortForm_NarrowbandModel</code></td>
  </tr>
</table>

### El modelo de formato abreviado en inglés de EE. UU.
{: #modelsShortform}

El modelo de formato abreviado en inglés de EE. UU., `en-US_ShortForm_NarrowbandModel`, puede mejorar el reconocimiento de voz para soluciones de respuesta de voz interactiva (IVR) y de soporte automático al cliente. El modelo de formato abreviado está entrenado para reconocer expresiones abreviadas que se utilizan con frecuencia en configuraciones de soporte al cliente, como los centros de atención al cliente automáticos y humanos. El modelo está adaptado, por ejemplo, para expresiones precisas, como dígitos, pronunciaciones de nombres y de palabras de un solo carácter y respuestas de tipo sí o no. El uso de una gramática en combinación con el modelo de formato abreviado puede mejorar aún más los resultados del reconocimiento.

Al igual que sucede con todos los modelos, los entornos ruidosos pueden afectar negativamente a los resultados. Por ejemplo, el ruido acústico de fondo de los aeropuertos, de los vehículos en movimiento, de las salas de conferencias y de múltiples oradores puede reducir la precisión de la transcripción.  El audio procedente de los dispositivos manos libres también puede reducir la precisión debido al eco común de dichos dispositivos. La utilización de un modelo acústico personalizado con el modelo de formato abreviado puede contrarrestar dichos efectos.

-   Para obtener más información acerca de la personalización del modelo de lenguaje y del modelo acústico, consulte [La interfaz de personalización](/docs/services/speech-to-text/custom.html).
-   Para obtener más información acerca de las gramáticas, consulte [Utilización de gramáticas con modelos de lenguaje personalizado](/docs/services/speech-to-text/grammar.html).

### Ejemplo de modelo de lenguaje
{: #modelsExample}

En la siguiente solicitud HTTP de ejemplo se utiliza el modelo `en-US-NarrowbandModel` para el reconocimiento de voz:

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel"
```
{: pre}

## Listado de modelos
{: #listModels}

La interfaz HTTP proporciona dos métodos para obtener información acerca de los modelos soportados:

-   El método `GET /v1/models` muestra la información sobre todos los modelos disponibles.
-   El método `GET /v1/models/{model_id}` muestra información sobre un modelo especificado.

Ambos métodos devuelven la siguiente información acerca de un modelo:

-   `name` es el nombre del modelo que se utiliza en una solicitud.
-   `language` es el identificador de idioma del modelo (por ejemplo, `en-US`).
-   `rate` identifica la frecuencia de muestreo (frecuencia mínima aceptable para el audio) que utiliza el modelo en hercios.
-   `url` es el URI del modelo.
-   `description` proporciona una breve descripción del modelo.
-   `supported_features` describe las características de servicio adicionales que reciben soporte con el modelo:
    -   `custom_language_model` es un valor booleano que indica si puede crear modelos de lenguaje personalizado basados en el modelo.
    -   `speaker_labels` indica si se puede utilizar el parámetro `speaker_labels` con el modelo.

### Solicitudes y respuestas de ejemplo
{: #listExample}

En el ejemplo siguiente se muestran todos los modelos a los que da soporte el servicio:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/models"
```
{: pre}

```javascript
{
  "models": [
    {
      "name": "pt-BR_NarrowbandModel",
      "language": "pt-BR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/pt-BR_NarrowbandModel",
      "rate": 8000,
      "supported_features": {
        "custom_language_model": false,
        "speaker_labels": false
      },
      "description": "Brazilian Portuguese narrowband model."
    },
    {
      "name": "ko-KR_BroadbandModel",
      "language": "ko-KR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/ko-KR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": false
      },
      "description": "Korean broadband model."
    },
    {
      "name": "fr-FR_BroadbandModel",
      "language": "fr-FR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/fr-FR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": true
      },
      "description": "French broadband model."
    },
    . . .
  ]
}
```
{: codeblock}

El ejemplo siguiente muestra información sobre el modelo de banda ancha en inglés de Estados Unidos. El modelo da soporte a la personalización del modelo de lenguaje y a etiquetas de orador.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/models/en-US_BroadbandModel"
```
{: pre}

```javascript
{
  "rate": 16000,
  "name": "en-US_BroadbandModel",
  "language": "en-US",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/en-US_BroadbandModel",
  "supported_features": {
    "custom_language_model": true,
    "speaker_labels": true
  },
  "description": "US English broadband model."
}
```
{: codeblock}
