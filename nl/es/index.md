---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-30"

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

# Acerca de
{: #about}

**Actualización del servicio:** *El servicio {{site.data.keyword.speechtotextshort}} se ha actualizado el 30 de julio de 2019. El servicio ahora ofrece modelos beta para cinco dialectos adicionales del español: argentino, chileno, colombiano, mexicano y peruano. Para obtener más información sobre los dialectos nuevos del español, consulte [Actualización de servicio del 30 de julio de 2019](/docs/services/speech-to-text?topic=speech-to-text-release-notes#July2019) en las notas de release.*

El servicio {{site.data.keyword.speechtotextfull}} proporciona prestaciones de transcripción de voz para las aplicaciones. El servicio aprovecha el aprendizaje de máquina para combinar el conocimiento de gramática, estructura de lenguaje y composición de señales de audio y de voz para transcribir con precisión la voz humana. Se actualiza continuamente y se perfecciona su transcripción a medida que recibe más conversación.
{: shortdesc}

El servicio proporciona varias interfaces que permiten que se adapte a cualquier aplicación en la que la voz es la entrada y una transcripción textual es la salida. Entre las aplicaciones de ejemplo se encuentran las siguientes:

-   Control de voz de aplicaciones, dispositivos incorporados y accesorios de vehículo
-   Transcripción de reuniones y conversaciones telefónicas
-   Dictado de mensajes y notas de correo electrónico

El servicio resulta ideal para clientes que tienen que extraer transcripciones de voz de alta calidad del audio del centro de atención telefónica. Los clientes de sectores como los servicios financieros, la atención sanitaria, los seguros y las telecomunicaciones pueden desarrollar aplicaciones nativas de la nube para la asistencia al cliente, la voz del cliente, la asistencia del agente y otras soluciones.

## Interfaces soportadas
{: #interfaces}

El servicio {{site.data.keyword.speechtotextshort}} ofrece tres interfaces para el reconocimiento de voz:

-   Una [interfaz WebSocket](/docs/services/speech-to-text?topic=speech-to-text-websockets) para establecer conexiones permanentes, de tipo dúplex completo y de baja latencia con el servicio. Puede pasar un máximo de 100 MB de datos de audio al servicio con una sola solicitud.
-   Una [interfaz HTTP síncrona](/docs/services/speech-to-text?topic=speech-to-text-http) para las llamadas HTTP básicas al servicio. Puede pasar un máximo de 100 MB de datos de audio al servicio.
-   Una [interfaz HTTP asíncrona](/docs/services/speech-to-text?topic=speech-to-text-async) para las llamadas de no bloqueo al servicio. Puede pasar hasta 1 GB de datos de audio con una solicitud.

El servicio también proporciona una [interfaz de personalización](/docs/services/speech-to-text?topic=speech-to-text-customization) que puede utilizar para adaptar el reconocimiento de voz a sus requisitos de idioma y acústica. Puede ampliar el vocabulario de un modelo con terminología específica del dominio o adaptar un modelo a las características acústicas de su audio. También puede añadir [gramáticas](/docs/services/speech-to-text?topic=speech-to-text-grammars) para restringir las frases que el servicio puede reconocer.

-   Para ver una descripción general del desarrollo de aplicaciones con el servicio, consulte [Visión general para los desarrolladores](/docs/services/speech-to-text?topic=speech-to-text-developerOverview).
-   Para ver ejemplos de solicitudes básicas de reconocimiento de voz con cada una de las interfaces del servicio, consulte [Cómo realizar una solicitud de reconocimiento](/docs/services/speech-to-text?topic=speech-to-text-basic-request).

Los SDK están disponibles en muchos lenguajes de programación para simplificar el uso del servicio. Para obtener más información, consulte la [Referencia de API](https://{DomainName}/apidocs/speech-to-text){: external}.

## Características de entrada
{: #inputFeatures}

Las interfaces de servicio comparten características de entrada comunes para transcribir voz a texto:

-   [Formatos de audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats): puede transcribir audio Ogg o Web Media (WebM) con codec Opus o Vorbis, MP3 (o MPEG), Waveform Audio File Format (WAV), Free Lossless Audio Codec (FLAC), Linear 16-bit Pulse-Code Modulation (PCM), G.729, A-Law, mu-law (o u-law) y audio básico. Si utiliza un formato que dé soporte a la compresión, puede maximizar la cantidad de datos de audio que puede enviar con una solicitud.
-   [Idiomas y modelos](/docs/services/speech-to-text?topic=speech-to-text-models): para la mayoría de los idiomas, puede transcribir el audio utilizando modelos de banda ancha o de banda estrecha. Utilice la banda ancha para el audio que se muestra a una velocidad mínima de 16 kHz. Utilice la banda estrecha para el audio que se muestra a una velocidad mínima de 8 kHz.
-   [Transmisión de audio](/docs/services/speech-to-text?topic=speech-to-text-input#transmission): puede pasar el audio como una secuencia continua de porciones de datos o como una entrega única en la que se pasan todos los datos a la vez. Con la secuencia, el servicio aplica [tiempos de espera excedidos](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts) de inactividad y de sesión.

## Características de salida
{: #outputFeatures}

Las interfaces también dan soporte a las siguientes características de salida comunes:

-   Las [etiquetas de orador](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels) reconocen diferentes oradores en el audio en inglés de EE.UU., inglés del Reino Unido, español o japonés. La transcripción etiqueta las contribuciones de cada orador en una conversación con varios participantes. (Funcionalidad beta.)
-   La [detección de palabras clave](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting) identifica frases habladas que coinciden con series de palabras clave especificadas con un nivel de confianza definido por el usuario. La detección de palabras clave resulta especialmente útil cuando las frases individuales del audio son más importantes que la transcripción completa. Por ejemplo, un sistema de soporte al cliente puede identificar palabras clave para determinar cómo direccionar las solicitudes de los usuarios.
-   Los [resultados provisionales](/docs/services/speech-to-text?topic=speech-to-text-output#interim) devuelven hipótesis progresivas a medida que progresa la transcripción. El servicio devuelve los resultados finales cuando la transcripción está completa. Los resultados provisionales solo están disponibles con la interfaz WebSocket.
-   El [número máximo de alternativas](/docs/services/speech-to-text?topic=speech-to-text-output#max_alternatives) proporcionan posibles transcripciones alternativas. El servicio indica los resultados finales en los que tiene la mayor confianza.
-   Las [alternativas a palabras](/docs/services/speech-to-text?topic=speech-to-text-output#word_alternatives) solicitan palabras alternativas que suenan de forma parecida a las palabras de una transcripción.
-   La [confianza de palabras](/docs/services/speech-to-text?topic=speech-to-text-output#word_confidence) devuelve niveles de confianza para cada palabra de una transcripción.
-   Las [indicaciones de fecha y hora de palabras](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps) devuelven indicaciones de fecha y hora correspondientes al principio y al final de cada palabra de una transcripción.
-   El [formateo inteligente](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting) convierte fechas, horas, números, valores de moneda, números de teléfono y direcciones de internet en formatos más legibles y convencionales en las transcripciones finales. En el caso del inglés de EE.UU., también puede proporcionar frases de palabras clave para incluir determinados signos de puntuación en las transcripciones finales. El formateo inteligente recibe soporte en audio en inglés de EE. UU., japonés y español. (Funcionalidad beta.)
-   La [ocultación numérica](/docs/services/speech-to-text?topic=speech-to-text-output#redaction) oculta, o enmascara, datos numéricos de una transcripción final. La ocultación sirve para eliminar la información personal confidencial, como números de tarjetas de crédito, de las transcripciones. La característica recibe soporte en audio en inglés de EE. UU., japonés y coreano. (Funcionalidad beta.)
-   El [filtrado de lenguaje obsceno](/docs/services/speech-to-text?topic=speech-to-text-output#profanity_filter) censura el lenguaje obsceno de las transcripciones en inglés de EE. UU.
-   Las [métricas de proceso](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics) proporcionan información de temporización detallada sobre el análisis del servicio del audio de entrada.
-   Las [métricas de audio](/docs/services/speech-to-text?topic=speech-to-text-metrics#audio_metrics) proporcionan información detallada sobre las características de señal del audio de entrada.

## Soporte de idiomas
{: #languages}

El servicio ofrece modelos para los siguientes idiomas y dialectos:

-   Árabe (estándar moderno)
-   Portugués de Brasil
-   Chino (mandarín)
-   Inglés (Reino Unido y Estados Unidos)
-   Francés
-   Alemán
-   Japonés
-   Coreano
-   Español (argentino, castellano, chileno, colombiano, mexicano y peruano)

El servicio no da soporte a todas las características en todos los idiomas. Además, da soporte a algunas características con disponibilidad genera (GA) para su uso en producción y a otras como ofertas beta para distintos idiomas.

-   El dialecto español castellano suele estar disponible. Los otros cinco dialectos del español son versiones beta.
-   Las interfaces WebSocket y HTTP están disponibles a nivel general para todos los idiomas.
-   El servicio ofrece modelos de banda ancha, modelos de banda estrecha, o ambos para distintos idiomas. Para obtener más información, consulte [Idiomas y modelos](/docs/services/speech-to-text?topic=speech-to-text-models).
-   Algunas características de reconocimiento de voz solo están disponibles para algunos idiomas. Para obtener más información, consulte el [Resumen de parámetros](/docs/services/speech-to-text?topic=speech-to-text-summary).
-   La interfaz de personalización del modelo de lenguaje está disponible a nivel general para la mayoría de idiomas. La interfaz de personalización del modelo acústico está disponible como funcionalidad beta para todos los idiomas. Para obtener más información, consulte [Soporte de idiomas para la personalización](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport).

## Precios
{: #pricing-index}

Para obtener más información sobre los planes de precios del servicio, consulte el servicio {{site.data.keyword.speechtotextshort}} en el [Catálogo de {{site.data.keyword.cloud}}](https://{DomainName}/catalog/services/speech-to-text){: external}. Para obtener respuestas a preguntas relacionadas con precios y ejemplos de costes por uso mensual, consulte [Preguntas frecuentes sobre precios](/docs/services/speech-to-text?topic=speech-to-text-faq-pricing).

## Pruebe el servicio
{: #tryOut}

Para ver ejemplos del servicio {{site.data.keyword.speechtotextshort}} en acción, consulte

-   Una [demo rápida](https://speech-to-text-demo.ng.bluemix.net/){: external} que transcribe el texto de la entrada de la secuencia de audio o de un archivo que el usuario haya cargado.
-   Aplicaciones en los [Kits de inicio](http://www.ibm.com/watson/developercloud/starter-kits.html){: external} de {{site.data.keyword.ibmwatson}} que muestran el uso del servicio.
-   La publicación del blog {{site.data.keyword.watson}} [Getting robots to listen: Using {{site.data.keyword.watson}}'s {{site.data.keyword.speechtotextshort}} service](https://www.ibm.com/blogs/watson/2016/07/getting-robots-listen-using-watsons-speech-text-service/){: external} que muestra cómo utilizar la interfaz WebSocket de servicio con Python para extraer la voz del audio. La publicación ofrece una guía de aprendizaje en la que se muestra el código y los pasos a seguir.
