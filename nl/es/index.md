---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-03"

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

# Acerca de
{: #about}

**Actualización del servicio:** *el servicio {{site.data.keyword.speechtotextshort}} se ha actualizado el 3 de abril de 2019. Ahora el servicio le permite añadir un máximo de 200 horas de audio a un modelo acústico personalizado. Para obtener más información, consulte la [actualización del servicio del 3 de abril de 2019](/docs/services/speech-to-text/release-notes.html#April2019) en las notas del release*.

El servicio {{site.data.keyword.speechtotextfull}} proporciona prestaciones de transcripción de voz para las aplicaciones. El servicio aprovecha el aprendizaje de máquina para combinar el conocimiento de gramática, estructura de lenguaje y composición de señales de audio y de voz para transcribir con precisión la voz humana. Se actualiza continuamente y se perfecciona su transcripción a medida que recibe más conversación.
{: shortdesc}

El servicio proporciona varias interfaces que permiten que se adapte a cualquier aplicación en la que la voz es la entrada y una transcripción textual es la salida. Entre las aplicaciones de ejemplo se encuentran las siguientes:

-   Control de voz de aplicaciones, dispositivos incorporados y accesorios de vehículo
-   Transcripción de reuniones y conversaciones telefónicas
-   Dictado de mensajes y notas de correo electrónico

## Interfaces soportadas
{: #interfaces}

El servicio {{site.data.keyword.speechtotextshort}} ofrece tres interfaces para el reconocimiento de voz:

-   Una [interfaz WebSocket](/docs/services/speech-to-text/websockets.html) para establecer conexiones permanentes, de tipo dúplex completo y de baja latencia con el servicio. Puede pasar un máximo de 100 MB de datos de audio al servicio con una sola solicitud.
-   Una [interfaz HTTP síncrona](/docs/services/speech-to-text/http.html) para las llamadas HTTP básicas al servicio. Puede pasar un máximo de 100 MB de datos de audio al servicio.
-   Una [interfaz HTTP asíncrona](/docs/services/speech-to-text/async.html) para las llamadas de no bloqueo al servicio. Puede pasar hasta 1 GB de datos de audio con una solicitud.

El servicio también proporciona una [interfaz de personalización](/docs/services/speech-to-text/custom.html) que puede utilizar para adaptar el reconocimiento de voz a sus requisitos de idioma y acústica. Puede ampliar el vocabulario de un modelo con terminología específica del dominio o adaptar un modelo a las características acústicas de su audio. También puede añadir [gramáticas](/docs/services/speech-to-text/grammar.html) para restringir las frases que el servicio puede reconocer.

-   Para ver una descripción general del desarrollo de aplicaciones con el servicio, consulte [Visión general para los desarrolladores](/docs/services/speech-to-text/developer-overview.html).
-   Para ver ejemplos de solicitudes básicas de reconocimiento de voz con cada una de las interfaces del servicio, consulte [Cómo realizar una solicitud de reconocimiento](/docs/services/speech-to-text/basic-request.html).

Los SDK están disponibles en muchos lenguajes de programación para simplificar el uso del servicio. Para obtener más información, revise la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

## Características de entrada
{: #inputFeatures}

Las interfaces de servicio comparten características de entrada comunes para transcribir voz a texto:

-   [Formatos de audio](/docs/services/speech-to-text/audio-formats.html): puede transcribir audio Ogg o Web Media (WebM) con codec Opus o Vorbis, MP3 (o MPEG), Waveform Audio File Format (WAV), Free Lossless Audio Codec (FLAC), Linear 16-bit Pulse-Code Modulation (PCM), G.729, A-Law, mu-law (o u-law) y audio básico. Si utiliza un formato que dé soporte a la compresión, puede maximizar la cantidad de datos de audio que puede enviar con una solicitud.
-   [Idiomas y modelos](/docs/services/speech-to-text/models.html): para la mayoría de los idiomas, puede transcribir el audio utilizando modelos de banda ancha o de banda estrecha. Utilice la banda ancha para el audio que se muestra a una velocidad mínima de 16 kHz. Utilice la banda estrecha para el audio que se muestra a una velocidad mínima de 8 kHz.
-   [Transmisión de audio](/docs/services/speech-to-text/input.html#transmission): puede pasar el audio como una secuencia continua de porciones de datos o como una entrega única en la que se pasan todos los datos a la vez. Con la secuencia, el servicio aplica [tiempos de espera excedidos](/docs/services/speech-to-text/input.html#timeouts) de inactividad y de sesión.

## Características de salida
{: #outputFeatures}

Las interfaces también dan soporte a las siguientes características de salida comunes:

-   Las [etiquetas de orador](/docs/services/speech-to-text/output.html#speaker_labels) reconocen diferentes oradores en el audio en inglés de EE.UU., inglés del Reino Unido, español o japonés. La transcripción etiqueta las contribuciones de cada orador en una conversación con varios participantes. (Funcionalidad beta.)
-   La [detección de palabras clave](/docs/services/speech-to-text/output.html#keyword_spotting) identifica frases habladas que coinciden con series de palabras clave especificadas con un nivel de confianza definido por el usuario. La detección de palabras clave resulta especialmente útil cuando las frases individuales del audio son más importantes que la transcripción completa. Por ejemplo, un sistema de soporte al cliente puede identificar palabras clave para determinar cómo direccionar las solicitudes de los usuarios.
-   Los [resultados provisionales](/docs/services/speech-to-text/output.html#interim) devuelven hipótesis progresivas a medida que progresa la transcripción. El servicio devuelve los resultados finales cuando la transcripción está completa. Los resultados provisionales solo están disponibles con la interfaz WebSocket.
-   El [número máximo de alternativas](/docs/services/speech-to-text/output.html#max_alternatives) proporcionan posibles transcripciones alternativas. El servicio indica los resultados finales en los que tiene la mayor confianza.
-   Las [alternativas a palabras](/docs/services/speech-to-text/output.html#word_alternatives) solicitan palabras alternativas que suenan de forma parecida a las palabras de una transcripción.
-   La [confianza de palabras](/docs/services/speech-to-text/output.html#word_confidence) devuelve niveles de confianza para cada palabra de una transcripción.
-   Las [indicaciones de fecha y hora de palabras](/docs/services/speech-to-text/output.html#word_timestamps) devuelven indicaciones de fecha y hora correspondientes al principio y al final de cada palabra de una transcripción.
-   El [formateo inteligente](/docs/services/speech-to-text/output.html#smart_formatting) convierte fechas, horas, números, valores de moneda, números de teléfono y direcciones de internet en formatos más legibles y convencionales en las transcripciones finales. En el caso del inglés de EE.UU., también puede proporcionar frases de palabras clave para incluir determinados signos de puntuación en las transcripciones finales. El formateo inteligente recibe soporte en audio en inglés de EE. UU., japonés y español. (Funcionalidad beta.)
-   La [ocultación numérica](/docs/services/speech-to-text/output.html#redaction) oculta, o enmascara, datos numéricos de una transcripción final. La ocultación sirve para eliminar la información personal confidencial, como números de tarjetas de crédito, de las transcripciones. La característica recibe soporte en audio en inglés de EE. UU., japonés y coreano. (Funcionalidad beta.)
-   El [filtrado de lenguaje obsceno](/docs/services/speech-to-text/output.html#profanity_filter) censura el lenguaje obsceno de las transcripciones en inglés de EE. UU.

## Soporte de idiomas
{: #languages}

El servicio ofrece modelos para los siguientes idiomas: portugués de Brasil, francés, alemán, japonés, coreano, chino mandarín, árabe estándar moderno, español, inglés del Reino Unido e inglés de Estados Unidos. El servicio no da soporte a todas las características en todos los idiomas. Además, da soporte a algunas características con disponibilidad genera (GA) para su uso en producción y a otras como ofertas beta para distintos idiomas.

-   Las interfaces WebSocket y HTTP están disponibles a nivel general para todos los idiomas.
-   El servicio ofrece modelos de banda ancha, modelos de banda estrecha, o ambos para distintos idiomas. Para obtener más información, consulte [Idiomas y modelos](/docs/services/speech-to-text/models.html).
-   Algunas características de reconocimiento de voz solo están disponibles para algunos idiomas. Para obtener más información, consulte el [Resumen de parámetros](/docs/services/speech-to-text/summary.html).
-   La interfaz de personalización del modelo de lenguaje está disponible a nivel general para la mayoría de idiomas. La interfaz de personalización del modelo acústico está disponible como funcionalidad beta para todos los idiomas. Para obtener más información, consulte [Soporte de idiomas para la personalización](/docs/services/speech-to-text/custom.html#languageSupport).

## Precios
{: #pricing-index}

Para obtener más información sobre los planes de precios del servicio, consulte el servicio {{site.data.keyword.speechtotextshort}} en el [catálogo de {{site.data.keyword.Bluemix_short}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/catalog/services/speech-to-text){: new_window}. Para obtener respuestas a preguntas relacionadas con precios y ejemplos de costes por uso mensual, consulte [Preguntas frecuentes sobre precios](/docs/services/speech-to-text/faq-pricing.html).

## Pruebe el servicio
{: #tryOut}

Para ver ejemplos del servicio {{site.data.keyword.speechtotextshort}} en acción, consulte

-   Una [demostración rápida ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://speech-to-text-demo.ng.bluemix.net/){: new_window} que transcribe texto a partir de una secuencia de audio o de un archivo que carga el usuario.
-   Aplicaciones de los {{site.data.keyword.ibmwatson}} [Kits de inicio ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://www.ibm.com/watson/developercloud/starter-kits.html){: new_window} que muestran el uso del servicio.
-   La {{site.data.keyword.watson}} publicación del blog [Getting robots to listen: Using {{site.data.keyword.watson}}'s {{site.data.keyword.speechtotextshort}} service ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/blogs/watson/2016/07/getting-robots-listen-using-watsons-speech-text-service/){: new_window}, donde se muestra cómo utilizar la interfaz WebSocket del servicio con Python para extraer voz del audio. La publicación ofrece una guía de aprendizaje en la que se muestra el código y los pasos a seguir.
