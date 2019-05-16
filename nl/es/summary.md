---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-15"

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

# Resumen de parámetros
{: #summary}

A continuación se muestra un resumen de todos los parámetros disponibles para el reconocimiento de voz. Para obtener más información sobre todos los métodos del servicio {{site.data.keyword.speechtotextshort}}, revise la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
{: shortdesc}

Tenga en cuenta los siguientes requisitos básicos cuando realice una solicitud de reconocimiento de voz:

-   Los nombres de los métodos distinguen entre mayúsculas y minúsculas.
-   Las cabeceras de solicitudes HTTP no distinguen entre mayúsculas y minúsculas.
-   Los parámetros de consulta HTTP y WebSocket distinguen entre mayúsculas y minúsculas.
-   Los nombres de campos JSON distinguen entre mayúsculas y minúsculas.
-   Todo el contenido de la respuesta JSON está en el juego de caracteres UTF-8.

Tenga en cuenta también los siguientes requisitos específicos del servicio:

-   Solo es obligatorio especificar el audio de entrada. Todos los
demás parámetros son opcionales.
-   Si especifica un parámetro de consulta o un campo JSON no válido como parte de la entrada, la respuesta incluye un campo `warnings` que describe el argumento no válido. La solicitud tiene éxito a pesar de los avisos.

## access_token
{: #summary-access-token}

Si utiliza la autenticación de Identity and Access Management (IAM), una señal de acceso de IAM opcional que se utiliza para establecer una conexión autenticada con la interfaz WebSocket. Para obtener más información, consulte el apartado sobre cómo [Abrir una conexión](/docs/services/speech-to-text/websockets.html#WSopen).

<table>
  <caption>Tabla 1. El parámetro access_token</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro de consulta de la solicitud de conexión <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      No soportado
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      No soportado
    </td>
  </tr>
</table>

## acoustic_customization_id
{: #summary-acoustic-customization-id}

Un ID de personalización opcional para un modelo acústico personalizado que se adapta a las características acústicas de su entorno y de los oradores. De forma predeterminada, no se utiliza ningún modelo personalizado. Para obtener más información, consulte [Modelos personalizados](/docs/services/speech-to-text/input.html#custom-input).

<table>
  <caption>Tabla 2. El parámetro acoustic_customization_id</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Beta para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro de consulta de la solicitud de conexión <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## base_model_version
{: #summary-base-model-version}

Versión opcional de un modelo base. El parámetro está pensado principalmente para que se utilice con modelos personalizados que se actualizan para un nuevo modelo base, pero se puede utilizar sin un modelo personalizado. El valor predeterminado depende de si el parámetro se utiliza con o sin un modelo personalizado. Para obtener más información, consulte [Versión del modelo base](/docs/services/speech-to-text/input.html#version).

<table>
  <caption>Tabla 3. El parámetro base_model_version</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro de consulta de la solicitud de conexión <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## Content-Type
{: #summary-content-type}

Un formato de audio opcional (tipo de MIME) que especifica el formato de los datos de audio que se pasan al servicio. El servicio puede detectar automáticamente el formato de la mayoría de audio, por lo que el parámetro es opcional para la mayoría de los formatos. Es obligatorio para los formatos `audio/alaw`, `audio/basic`, `audio/l16` y `audio/mulaw`. Para obtener más información, consulte [Formatos de audio](/docs/services/speech-to-text/audio-formats.html).

<table>
  <caption>Tabla 4. El parámetro Content-Type</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro <code>content-type</code> del mensaje JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Cabecera de solicitud del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Cabecera de solicitud del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## customization_weight
{: #summary-customization-weight}

Un valor opcional comprendido entre 0,0 y 1,0 que indica la ponderación relativa que otorga el servicio a las palabras de un modelo de lenguaje personalizado frente a las palabras del vocabulario base. El valor predeterminado es 0,3 a menos que se haya especificado una ponderación distinta al entrenar el modelo de lenguaje personalizado. Para obtener más información, consulte [Modelos personalizados](/docs/services/speech-to-text/input.html#custom-input).

<table>
  <caption>Tabla 5. El parámetro personation_weight</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para inglés de EE. UU., inglés del Reino Unido, portugués de Brasil, francés, alemán, japonés, coreano y español
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro del mensaje <code>start</code> de JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## grammar_name
{: #summary-grammar-name}

Una serie opcional que identifica una gramática que se va a utilizar para el reconocimiento de voz. El servicio solo reconoce las series definidas por la gramática. Debe especificar tanto el nombre de la gramática como el ID de personalización del modelo de lenguaje personalizado para el que se define la gramática. Para obtener más información, consulte [Gramáticas](/docs/services/speech-to-text/input.html#grammars-input).

<table>
  <caption>Tabla 6. El parámetro grammar_name</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Beta para inglés de EE. UU., inglés del Reino Unido, portugués de Brasil, francés, alemán, japonés, coreano y español
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro del mensaje <code>start</code> de JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## inactivity_timeout
{: #summary-inactivity-timeout}

Un número entero opcional que especifica el número de segundos correspondiente al tiempo de espera excedido de inactividad del servicio. Inactividad significa que el servicio no detecta voz en la secuencia de audio. El valor predeterminado es 30 segundos. Utilice `-1` para indicar un tiempo infinito. Para obtener más información, consulte [Tiempo de espera excedido de inactividad](/docs/services/speech-to-text/input.html#timeouts-inactivity).

<table>
  <caption>Tabla 7. El parámetro inactivity_timeout</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro del mensaje <code>start</code> de JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## interim_results
{: #summary-interim-results}

Un valor booleano opcional que indica al servicio que devuelva hipótesis intermedias que es probable que cambien antes de la transcripción final. De forma predeterminada (`false`), no se devuelven resultados provisionales. Para obtener más información, consulte [Resultados provisionales](/docs/services/speech-to-text/output.html#interim).

<table>
  <caption>Tabla 8. El parámetro interim_results</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro del mensaje <code>start</code> de JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      No soportado
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      No soportado
    </td>
  </tr>
</table>

## keywords
{: #summary-keywords}

Una matriz opcional de series de palabras clave que el servicio detecta en el audio de entrada. De forma predeterminada, no se lleva a cabo la detección de palabras clave. Para obtener más información, consulte [Detección de palabras clave](/docs/services/speech-to-text/output.html#keyword_spotting).

<table>
  <caption>Tabla 9. El parámetro keywords</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro del mensaje <code>start</code> de JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## keywords_threshold
{: #summary-keywords-threshold}

Un valor opcional comprendido entre 0,0 y 1,0 que indica el umbral mínimo para una coincidencia positiva de palabra clave. De forma predeterminada, no se lleva a cabo la detección de palabras clave. Para obtener más información, consulte [Detección de palabras clave](/docs/services/speech-to-text/output.html#keyword_spotting).

<table>
  <caption>Tabla 10. El parámetro keywords_threshold</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro del mensaje <code>start</code> de JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## language_customization_id
{: #summary-language-customization-id}

Un ID de personalización opcional para un modelo de lenguaje personalizado que incluye terminología de su dominio. De forma predeterminada, no se utiliza ningún modelo personalizado. Para obtener más información, consulte [Modelos personalizados](/docs/services/speech-to-text/input.html#custom-input).

<table>
  <caption>Tabla 11. El parámetro language_customization_id</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para inglés de EE. UU., inglés del Reino Unido, portugués de Brasil, francés, alemán, japonés, coreano y español
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro de consulta de la solicitud de conexión <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## max_alternatives
{: #summary-max-alternatives}

Un número entero opcional que especifica el número máximo de hipótesis alternativas que devuelve el servicio. De forma predeterminada, el servicio devuelve una sola hipótesis final. Para obtener más información, consulte [Número máximo de alternativas](/docs/services/speech-to-text/output.html#max_alternatives).

<table>
  <caption>Tabla 12. El parámetro max_alternatives</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro del mensaje <code>start</code> de JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## model
{: #summary-model}

Un modelo opcional que especifica el idioma en el que se habla el audio y la velocidad a la que se ha muestreado: banda ancha o banda estrecha. De forma predeterminada, se utiliza `en-US_BroadbandModel`. Para obtener más información, consulte [Idiomas y modelos](/docs/services/speech-to-text/models.html).

<table>
  <caption>Tabla 13. El parámetro model</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro de consulta de la solicitud de conexión <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## profanity_filter
{: #summary-profanity-filter}

Un valor booleano opcional que indica si el servicio censura de una transcripción el lenguaje obsceno. De forma predeterminada (`true`), se filtra el lenguaje obsceno de la transcripción. Para obtener más información, consulte [Filtrado de lenguaje obsceno](/docs/services/speech-to-text/output.html#profanity_filter).

<table>
  <caption>Tabla 14. El parámetro profanity_filter</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para inglés de EE. UU.
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro del mensaje <code>start</code> de JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## redaction
{: #summary-redaction}

Un valor booleano opcional que indica si el servicio debe ocultar en una transcripción los datos numéricos con tres o más dígitos consecutivos. Si establece el parámetro `redaction` en `true`, el servicio impone automáticamente que el parámetro `smart_formatting` tenga el valor `true`. De forma predeterminada (`false`), los datos numéricos no se ocultan. Para obtener más información, consulte [Ocultación numérica](/docs/services/speech-to-text/output.html#redaction).

<table>
  <caption>Tabla 15. El parámetro redaction</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Beta para inglés de EE. UU., japonés y coreano
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro del mensaje <code>start</code> de JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## smart_formatting
{: #summary-smart-formatting}

Un valor booleano opcional que indica si el servicio convierte fechas, horas, números, moneda y valores similares en representaciones más convencionales en la transcripción final. Para inglés de EE. UU., la característica también convierte determinadas frases con palabras clave en signos de puntuación. De forma predeterminada (`false`), no se lleva a cabo el formateo inteligente. Para obtener más información, consulte [Formateo inteligente](/docs/services/speech-to-text/output.html#smart_formatting).

<table>
  <caption>Tabla 16. El parámetro smart_formatting</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Beta para inglés de EE. UU., japonés y español
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro del mensaje <code>start</code> de JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## speaker_labels
{: #summary-speaker-labels}

Un valor booleano opcional que indica si el servicio identifica las palabras que ha pronunciado cada persona en un intercambio con varios participantes. Si establece el parámetro `speaker_labels` en `true`, el servicio impone automáticamente que el parámetro `timestamps` tenga el valor `true`. De forma predeterminada (`false`), no se devuelven etiquetas de orador. Para obtener más información, consulte [Etiquetas de orador](/docs/services/speech-to-text/output.html#speaker_labels).

<table>
  <caption>Tabla 17. El parámetro speaker_labels</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Beta para inglés de EE. UU., japonés y español (modelos de banda ancha y de banda estrecha) y para inglés del Reino Unido (solo modelo de banda estrecha)
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro del mensaje <code>start</code> de JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## timestamps
{: #summary-timestamps}

Un valor booleano opcional que indica si el servicio genera indicaciones de fecha y hora para las palabras de la transcripción. De forma predeterminada (`false`), no se devuelven indicaciones de fecha y hora. Para obtener más información, consulte [Indicaciones de fecha y hora de palabras](/docs/services/speech-to-text/output.html#word_timestamps).

<table>
  <caption>Tabla 18. El parámetro timestamps</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro del mensaje <code>start</code> de JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## Transfer-Encoding
{: #summary-transfer-encoding}

Un valor opcional de `chunked` que hace que el audio se transmita en secuencia al servicio. De forma predeterminada, el audio se envía todo a la vez en una entrega única. Para obtener más información, consulte [Transmisión de audio](/docs/services/speech-to-text/input.html#transmission).

<table>
  <caption>Tabla 19. El parámetro Transfer-Encoding</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      No se aplica; siempre se transmite en secuencia
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Cabecera de solicitud del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Cabecera de solicitud del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## watson-token
{: #summary-watson-token}

Si utiliza credenciales de servicio de Cloud Foundry, una señal de autenticación opcional de {{site.data.keyword.watson}} que se utiliza para establecer una conexión autenticada con la interfaz WebSocket. Para obtener más información, consulte el apartado sobre cómo [Abrir una conexión](/docs/services/speech-to-text/websockets.html#WSopen).

<table>
  <caption>Tabla 20. El parámetro watson-token</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro de consulta de la solicitud de conexión <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      No soportado
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      No soportado
    </td>
  </tr>
</table>

## word_alternatives_threshold
{: #summary-word-alternatives-threshold}

Un valor opcional comprendido entre 0,0 y 1,0 que especifica el umbral en el cual el servicio muestra alternativas acústicamente similares para las palabras del audio de entrada. De forma predeterminada, no se devuelven alternativas a palabras. Para obtener más información, consulte [Alternativas a palabras](/docs/services/speech-to-text/output.html#word_alternatives).

<table>
  <caption>Tabla 21. El parámetro word_alternatives_threshold</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro del mensaje <code>start</code> de JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## word_confidence
{: #summary-word-confidence}

Un valor booleano opcional que indica si el servicio proporciona medidas de confianza para las palabras de la transcripción. De forma predeterminada (`false`), no se devuelven medidas de confianza. Para obtener más información, consulte [Confianza de palabras](/docs/services/speech-to-text/output.html#word_confidence).

<table>
  <caption>Tabla 22. El parámetro word_confidence</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro del mensaje <code>start</code> de JSON
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Parámetro de consulta del método <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## X-Watson-Authorization-Token
{: #summary-x-watson-authorization-token}

Una señal de autenticación opcional que realiza solicitudes autenticadas al servicio sin incluir las credenciales de servicio en cada llamada. De forma predeterminada, se deben pasar las credenciales de servicio con cada solicitud. Las señales de autenticación de Watson se basan en las credenciales de servicio de Cloud Foundry que utilizan los valores `{username}` y `{password}` para la autenticación.

La cabecera `X-Watson-Authorization-Token` no acepta señales de IAM ni claves de API.
{: note}

<table>
  <caption>Tabla 23. El parámetro X-Watson-Authorization-Token</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      No soportado; utilice el parámetro de consulta <code>watson-token</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Cabecera de solicitud de cada solicitud
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Cabecera de solicitud de cada solicitud
    </td>
  </tr>
</table>

## X-Watson-Learning-Opt-Out
{: #summary-x-watson-learning-opt-out}

Un valor booleano opcional que indica si se omite del registro de solicitudes predeterminado que {{site.data.keyword.IBM_notm}} realiza para mejorar el servicio para los usuarios futuros. Para impedir que IBM acceda a sus datos para realizar mejoras generales del servicio,
especifique <code>true</code> para el parámetro. Para obtener más información, consulte [Registro de solicitudes](/docs/services/speech-to-text/input.html#logging).

<table>
  <caption>Tabla 24. El parámetro X-Watson-Learning-Opt-Out</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro de consulta <code>x-watson-learning-opt-out</code> de la solicitud de conexión <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Cabecera de solicitud de cada solicitud
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Cabecera de solicitud de cada solicitud
    </td>
  </tr>
</table>

## X-Watson-Metadata
{: #summary-x-watson-metadata}

Una serie opcional que asocia un ID de cliente con los datos que se pasan para las solicitudes de reconocimiento. El parámetro acepta el argumento `customer_id={id}`. De forma predeterminada, no se asocia ningún ID de cliente a los datos. Para obtener más información, consulte [Seguridad de la información](/docs/services/speech-to-text/information-security.html).

<table>
  <caption>Tabla 25. El parámetro X-Watson-Metadata</caption>
  <tr>
    <th>Disponibilidad y utilización</th>
    <th style="vertical-align:bottom">Descripción</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilidad**
    </td>
    <td style="text-align:left">
      Disponible a nivel general para todos los idiomas
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Parámetro de consulta <code>x-watson-metadata</code> de la solicitud de
      conexión <code>/v1/recognize</code> (debe codificar el argumento en URL,
      por ejemplo `customer_id%3dmy_customer_ID`.)
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP síncrona**
    </td>
    <td style="text-align:left">
      Cabecera de solicitud de la solicitud POST <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asíncrona**
    </td>
    <td style="text-align:left">
      Cabecera de solicitud de las solicitudes <code>POST /v1/register_callback</code> y
      <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>
