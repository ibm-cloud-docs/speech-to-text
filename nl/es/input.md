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

# Características de entrada
{: #input}

El servicio {{site.data.keyword.speechtotextshort}} ofrece las siguientes características para especificar la forma en que el servicio debe realizar una solicitud de reconocimiento de voz. Todos los parámetros de entrada que se describen en las secciones siguientes son opcionales. Solo es obligatorio el audio de entrada.
{: shortdesc}

-   Para ver ejemplos de solicitudes sencillas de reconocimiento de voz para cada una de las interfaces del servicio, consulte [Cómo realizar una solicitud de reconocimiento](/docs/services/speech-to-text/basic-request.html).
-   Para ver una lista ordenada alfabéticamente de todos los parámetros de reconocimiento de voz disponibles, incluido su estado (disponible a nivel general o beta) y los idiomas admitidos, consulte el [Resumen de parámetros](/docs/services/speech-to-text/summary.html).

## Modelos personalizados
{: #custom-input}

Las personalizaciones del modelo acústico y del modelo de lenguaje están disponibles con distintos niveles de soporte (disponible a nivel general o beta) para los distintos idiomas. Para obtener más información, consulte [Soporte de idiomas para la personalización](/docs/services/speech-to-text/custom.html#languageSupport).
{: note}

Todas las interfaces aceptan un modelo personalizado para su uso en una solicitud de reconocimiento:

-   Los *modelos de lenguaje personalizado* amplían el vocabulario base del servicio con terminología de dominios específicos. Utilice el parámetro `language_customization_id` para incluir un modelo de lenguaje personalizado con una solicitud. También puede especificar un parámetro opcional `customization_weight`. El parámetro indica la ponderación relativa que se otorga a las palabras del modelo personalizado en comparación con las palabras del vocabulario base.

    Para obtener más información, consulte el apartado sobre [Utilización de un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-use.html).
-   Los *modelos acústicos personalizados* adaptan el modelo acústico base del servicio a las características acústicas de su entorno y de los oradores. Utilice el parámetro `acoustic_customization_id` para incluir un modelo acústico personalizado con una solicitud. Puede especificar tanto un modelo de lenguaje personalizado como un modelo acústico personalizado con una solicitud.

    Para obtener más información, consulte [Utilización de un modelo acústico personalizado](/docs/services/speech-to-text/acoustic-use.html).

Los modelos personalizados se basan en uno de los modelos de lenguaje que se describen en [Idiomas y modelos](/docs/services/speech-to-text/models.html). Un modelo personalizado solo se puede utilizar con el modelo base para el que se ha creado. Si el modelo personalizado se basa en un modelo distinto de `en-US_BroadbandModel`, el valor predeterminado, también se debe especificar el nombre del modelo con la solicitud. Para utilizar un modelo personalizado, debe enviar la solicitud con las credenciales de servicio que se crean para la instancia del servicio propietaria del modelo personalizado.

Para ver una introducción a la personalización, consulte [La interfaz de personalización](/docs/services/speech-to-text/custom.html).

### Ejemplos de modelos personalizados
{: #customExample}

La solicitud de ejemplo siguiente incluye el parámetro `language_customization_id` para utilizar el modelo de lenguaje personalizado con el ID especificado. Incluye el parámetro `customization_weight` que indica que a las palabras del modelo personalizado se les debe asignar una ponderación relativa de `0,5`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.5"
```
{: pre}

La solicitud de ejemplo siguiente utiliza un modelo de lenguaje personalizado y un modelo acústico personalizado. El primero se identifica con el parámetro `language_customization_id` y el segundo con el parámetro `acoustic_customization_id`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&acoustic_customization_id={customization_id}"
```
{: pre}

Para ver ejemplos en los que se utilizan modelos personalizados con cada una de las interfaces del servicio, consulte

-   [Utilización de un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-use.html)
-   [Utilización de un modelo acústico personalizado](/docs/services/speech-to-text/acoustic-use.html)

## Gramáticas
{: #grammars-input}

La característica de gramáticas es una funcionalidad beta. El servicio da soporte a gramáticas para todos los idiomas para los que da soporte a la personalización del modelo de lenguaje.
{: note}

Puede añadir gramáticas a un modelo de lenguaje personalizado y utilizarlas para el reconocimiento de voz. Las gramáticas utilizan una especificación de lenguaje formal para definir un conjunto de reglas de producción para transcribir series de caracteres. Las reglas especifican cómo formar series válidas a partir del alfabeto del idioma.

Cuando se utiliza una gramática para el reconocimiento de voz, el servicio solo puede reconocer las frases que reconoce la gramática. Al limitar el espacio de búsqueda de series válidas, el servicio puede ofrecer resultados más rápido y con mayor precisión.

Todas las interfaces aceptan los parámetros siguientes para una solicitud de reconocimiento:

-   El parámetro `language_customization_id` identifica el modelo de lenguaje personalizado para el que se ha definido la gramática. Debe enviar la solicitud con las credenciales de servicio correspondientes a la instancia del servicio propietaria del modelo.
-   El parámetro `grammar_name` especifica la gramática que desea utilizar. Solo puede especificar una gramática con una solicitud.

Para obtener más información, consulte [Utilización de gramáticas con modelos de lenguaje personalizados](/docs/services/speech-to-text/grammar.html).

### Ejemplo de gramáticas
{: #grammarsExample}

La solicitud de ejemplo siguiente incluye los parámetros `language_customization_id` y `grammar_name` para restringir la respuesta del servicio a las series definidas en la gramática denominada `list-abnf`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&grammar_name=list-abnf"
```
{: pre}

Para ver ejemplos en los que se utilizan gramáticas con cada una de las interfaces del servicio, consulte [Utilización de una gramática para el reconocimiento de voz](/docs/services/speech-to-text/grammar-use.html).

## Versión del modelo base
{: #version}

Para mejorar la calidad del reconocimiento de voz, el servicio actualiza de vez en cuando los modelos de lenguaje base que se describen en [Idiomas y modelos](/docs/services/speech-to-text/models.html). Los modelos base correspondiente a los idiomas son independientes entre sí, al igual que los modelos de banda ancha y de banda estrecha correspondientes a un idioma. Por lo tanto, las actualizaciones de los modelos base se producen de forma independiente entre sí y no tienen ningún efecto sobre otros modelos.

Cuando existen varias versiones de un modelo base, el parámetro opcional `base_model_version` especifica la versión del modelo que se va a utilizar con una solicitud de reconocimiento. El parámetro está pensado principalmente para que se utilice con modelos personalizados que se actualizan para un nuevo modelo base, pero se puede utilizar sin un modelo personalizado. La versión de un modelo base que se utiliza para una solicitud depende de si se pasa el parámetro `base_model_version`. También depende de si se especifica un modelo personalizado (de lenguaje, acústico o ambos) con la solicitud.

-   *Si no especifica un modelo personalizado con la solicitud*

    -   Omita el parámetro `base_model_version` para utilizar la versión más reciente del modelo base.
    -   Especifique el parámetro `base_model_version` para utilizar una versión específica del modelo base.

-   *Si especifica un modelo personalizado con la solicitud*

    -   Omita el parámetro `base_model_version` para utilizar la versión más reciente del modelo base a la que se actualiza el modelo personalizado. Por ejemplo, si el modelo personalizado se actualiza a la versión más reciente del modelo base, el servicio utiliza las versiones más recientes de los modelos base y personalizado.
    -   Especifique el parámetro `base_model_version` para utilizar las versiones especificadas de los modelos base y personalizado.

El parámetro está pensado para ser utilizado con modelos personalizados. Por lo tanto, solo puede obtener información sobre las versiones disponibles de un modelo base obteniendo información sobre un modelo personalizado basado en el mismo.

-   Para obtener más información sobre cómo actualizar modelos personalizados, consulte [Actualización de modelos personalizados](/docs/services/speech-to-text/custom-upgrade.html).
-   Para obtener más información sobre cómo utilizar distintas versiones de modelos base y personalizado para el reconocimiento de voz, consulte [Cómo realizar solicitudes de reconocimiento con modelos personalizados actualizados](/docs/services/speech-to-text/custom-upgrade.html#upgradeRecognition).

## Transmisión de audio
{: #transmission}

*Con la interfaz WebSocket,* los datos de audio siempre se envían en secuencia al servicio a través de la conexión. Puede pasar todos los datos a la vez a través del socket o puede pasar datos en directo a medida que estén disponibles. El servicio devuelve resultados a medida que están disponibles.

*Con las interfaces HTTP,* puede transmitir el audio al servicio de una de las formas siguientes:

-   *Entrega única*: se omite la cabecera `Transfer-Encoding` y se pasan todos los datos de audio al servicio de una sola vez como entrega única.
-   *Secuencia*: se establece la cabecera de solicitud `Transfer-Encoding` en el valor `chunked` y los datos se pasan en secuencia a través de una conexión permanente. No es necesario que existan todos los datos antes de transmitirlos en secuencia al servicio. Puede enviar en secuencia los datos a medida que están disponibles. El servicio solo envía resultados cuando recibe el bloque de datos final, que se indica mediante el envío de un bloque de datos vacío.

    Para obtener más información sobre el envío en secuencia de audio particionado con la cabecera `Transfer-Encoding`, consulte
    -   [en.wikipedia.org/wiki/Chunked_transfer_encoding (en inglés) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/Chunked_transfer_encoding){: new_window}
    -   [Codificaciones de transferencia ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://tools.ietf.org/html/rfc7230#section-4){: new_window} en *IETF RFC 7320 HTTP/1.1: Sintaxis de mensajes y direccionamiento*

Con las interfaces HTTP, el servicio siempre transcribe toda la secuencia de audio antes de enviar resultados. Los resultados pueden incluir varios elementos `transcript` para indicar frases que están separadas por pausas. Concatenen los elementos `transcript` para ensamblar la transcripción completa.

El servicio aplica tiempos de espera excedidos en una sesión de secuencia de audio. Puede finalizar una sesión de secuencia de audio si detecta un período de silencio prolongado o si no recibe ningún audio durante un período de 30 segundos. Para obtener información sobre tiempos de espera excedidos y cómo evitarlos, consulte [Tiempos de espera excedidos](#timeouts).

### Ejemplo de transmisión de audio
{: #transmissionExample}

La solicitud de ejemplo siguiente especifica `chunked` para la cabecera `Transfer-Encoding` para utilizar la modalidad de secuencia. La conexión permanece abierta para aceptar segmentos de audio adicionales.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--header "Transfer-Encoding: chunked"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## Tiempos de espera excedidos
{: #timeouts}

Cuando se inicia una sesión de secuencia de audio con los métodos de reconocimiento de voz HTTP o WebSocket, el servicio aplica tiempos de espera excedidos de inactividad y de sesión. Si se produce un tiempo de espera excedido durante una sesión de secuencia de audio, el servicio cierra la conexión. La aplicación se debe recuperarse de las posibles conexiones cerradas.

Cuando se transmiten secuencias de audio a través de HTTP, el servicio envía un carácter de espacio en su respuesta cada 20 segundos. Lo hace para facilitar el uso al evitar el tiempo de espera excedido de inactividad de REST HTTP de 30 segundos. Para mantener la conexión activa mientras el reconocimiento está en curso, el servicio continúa enviando este carácter de espacio hasta que completa la transcripción. El carácter de espacio no tiene ningún efecto en los datos de respuesta codificados en JSON.

Este tiempo de espera excedido de inactividad de HTTP es distinto del tiempo de espera excedido de inactividad del servicio. La interfaz WebSocket no está sujeta a este tiempo de espera excedido de HTTP.
{: note}

### Tiempo de espera excedido de inactividad
{: #timeouts-inactivity}

Se produce un *tiempo de espera excedido de inactividad* (código de estado 400 de HTTP) cuando el servicio recibe el audio, pero solo detecta silencio continuo o actividad sin voz (sin habla) durante 30 segundos. El servicio envía el mensaje de error `No se ha detectado voz durante 30 segundos`. El tiempo de espera excedido de inactividad resulta útil, por ejemplo, para finalizar una sesión cuando un usuario simplemente se aleja de un micrófono activo.

El tiempo de espera excedido de inactividad predeterminado es 30 segundos. Puede modificar este valor con el parámetro `inactivity_timeout`. Especifique un valor mayor para aumentar el tiempo de espera excedido de inactividad. Especifique el valor `-1` para establecer el tiempo de espera excedido de inactividad en infinito. Se le factura todo el audio que envía al servicio, incluido silencio, por lo que aumentar el tiempo de espera excedido de inactividad puede ocasionar un gasto adicional para una sesión de secuencia de audio que solo envíe silencio.

La solicitud de ejemplo siguiente establece el tiempo de espera excedido de inactividad en 60 segundos. La solicitud envía un archivo inicial para empezar la sesión de secuencia de audio.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Transfer-Encoding: chunked"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?inactivity_timeout=60"
```
{: pre}

### Tiempo de espera excedido de sesión
{: #timeouts-session}

Se produce un *tiempo de espera excedido de sesión* (código de estado 408 de HTTP) cuando no se envía suficiente audio para mantener activa una sesión de secuencia de audio. El servicio puede considerar que una sesión está desocupada y activar un tiempo de espera excedido de sesión por los motivos siguientes:

-   No se ha podido enviar al menos 15 segundos de audio al servicio en una ventana de 30 segundos.

    Hasta que envíe el último fragmento para indicar el final de la secuencia, debe enviar al menos 15 segundos de audio dentro de cada periodo de 30 segundos. El audio puede estar en silencio si establece el parámetro `inactivity_timeout` en un valor mayor o en `-1`. Se le factura por la duración del audio que envía al servicio, incluido silencio.
-   Envía una secuencia de audio a una velocidad mucho más lenta que si fuera en tiempo real.

    Lo ideal es que inicie una solicitud para establecer una sesión justo antes de obtener el audio para la transcripción. En ese caso, mantendría la sesión enviando audio a una velocidad cercana a la de tiempo real.

No debe preocuparse por el tiempo de espera excedido de sesión después de enviar el último fragmento para indicar el final de la secuencia. El servicio continúa procesando el audio hasta que devuelve los resultados finales de la transcripción.

Cuando se transcribe una secuencia de audio larga, el servicio puede tardar más de 30 segundos en procesar el audio y generar la respuesta. El servicio no empieza a calcular el tiempo de espera de la sesión hasta que finaliza el proceso de todo el audio que ha recibido. El tiempo de proceso del servicio no puede hacer que la sesión supere el tiempo de espera excedido de la sesión de 30 segundos.

Por ejemplo, si envía una hora de audio durante los 10 primeros segundos de una sesión, el servicio puede tardar 300 segundos en procesar el audio.  Para mantener viva esta sesión, tendría que enviar al menos 15 segundos más de audio, incluido silencio, no más tarde de 340 segundos a la sesión.

En este ejemplo, si fuera a enviar otros 15 segundos de audio a una marca de la sesión de 100 segundos, el servicio podría tardar otros dos segundos en procesar este audio. En este caso, tendría que enviar 15 segundos más de audio no más tarde de 342 segundos a la sesión.

No se base en el tiempo de proceso o en si se han recibido resultados para determinar si una sesión de secuencia de audio está desocupada. Suponga que el servicio puede procesar todo el audio de forma instantánea y envíe los datos al servicio en consecuencia. Si envía secuencias de audio en tiempo real, no se quede atrás enviando audio a la mitad del tiempo real (15 segundos de audio) en una ventana de 30 segundos. Esta velocidad suele ser suficiente para acomodar la latencia de red y los retardos.
{: important}

## Registro de solicitudes
{: #logging}

De forma predeterminada, {{site.data.keyword.IBM_notm}} registra todas las solicitudes a los servicios {{site.data.keyword.watson}} y sus resultados. El registro se realiza únicamente para mejorar los servicios para futuros usuarios. Los datos registrados no se comparten ni se hacen públicos.

Si le preocupa la protección de la privacidad de la información personal de los usuarios o no desea que IBM registre sus solicitudes, puede optar por que IBM no registre sus datos. Puede impedir el registro a cualquier nivel de cuenta o a cualquier nivel de solicitud de API. Para obtener más información, consulte [Control del registro de solicitudes para servicios {{site.data.keyword.watson}}](/docs/services/watson/getting-started-logging.html).

## Seguridad de la información
{: #security-input}

La seguridad de la información incluye características para asociar un ID de cliente con los datos que se pasan al servicio con una solicitud. Un ID de cliente se asocia a los datos pasando la cabecera `X-Watson-Metadata` con la respuesta. Si es necesario, luego puede suprimir los datos con el método `DELETE /v1/user_data`. Para obtener más información, consulte [Seguridad de la información](/docs/services/speech-to-text/information-security.html).
