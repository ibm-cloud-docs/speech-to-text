---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-21"

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

# La interfaz de personalización
{: #customization}

El servicio {{site.data.keyword.speechtotextfull}} ofrece una interfaz de personalización que se puede utilizar para aumentar su capacidad de reconocimiento de voz. Puede utilizar la personalización para mejorar la precisión de las solicitudes de reconocimiento de voz personalizadas mediante la personalización de un modelo base para el dominio y el audio. La personalización solo está disponible para algunos idiomas y con distintos niveles de soporte para cada idioma; consulte [Soporte de idiomas para la personalización](#languageSupport).
{: shortdesc}

La interfaz de personalización da soporte al modelo de lenguaje personalizado y al modelo acústico personalizado. Las interfaces para ambos tipos de modelo personalizado son similares y resultan fáciles de utilizar. La utilización de cualquiera de los tipos de modelo personalizado con una solicitud de reconocimiento también es sencilla: debe especificar el ID de personalización del modelo con la solicitud.

El reconocimiento de voz funciona de la misma manera con o sin un modelo personalizado. Cuando se utiliza un modelo personalizado para el reconocimiento de voz, se pueden utilizar todos los parámetros de entrada y salida que normalmente están disponibles con una solicitud de reconocimiento. Para obtener más información acerca de todos los parámetros disponibles, consulte el [Resumen de parámetros](/docs/services/speech-to-text?topic=speech-to-text-summary).

Debe tener el plan de fijación de precios estándar para utilizar el modelo de lenguaje o la personalización del modelo acústico. Los usuarios del plan Lite no pueden utilizar la interfaz de personalización. Para obtener más información, consulte el servicio {{site.data.keyword.speechtotextshort}} en la [página de precios](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external}.
{: note}

## Personalización del modelo de lenguaje
{: #customLanguage-intro}

El servicio ha sido desarrollado pensando en una audiencia amplia y general. El vocabulario base del servicio contiene muchas palabras que se utilizan en conversaciones diarias. Sus modelos ofrecen un reconocimiento lo suficientemente preciso para muchas aplicaciones. Pero les puede faltar un conocimiento de términos específicos asociados a dominios particulares.

La interfaz de *personalización del modelo de lenguaje* puede mejorar la precisión del reconocimiento de voz para dominios como la medicina, el ámbito legal, la tecnología de la información y otros. Mediante la personalización del modelo de lenguaje, puede ampliar y adaptar el vocabulario de un modelo base para que incluya terminología específica del dominio.

Para crear un modelo de lenguaje personalizado, añada un corpus y palabras específicas del dominio. Cuando haya entrenado el modelo de lenguaje personalizado con su vocabulario ampliado, puede utilizarlo para el reconocimiento de voz personalizado. Normalmente, el servicio puede entrenar cualquier modelo personalizado en cuestión de minutos. El nivel de esfuerzo necesario para crear un modelo depende de los datos que tenga disponibles para el modelo.

Para obtener más información,
consulte

-   [Creación de un modelo de lenguaje personalizado](/docs/services/speech-to-text?topic=speech-to-text-languageCreate)
-   [Utilización de un modelo de lenguaje personalizado](/docs/services/speech-to-text?topic=speech-to-text-languageUse)

## Personalización de modelos acústicos
{: #customAcoustic-intro}

Del mismo modo, el servicio se ha desarrollado con modelos acústicos básicos que funcionan bien para diversas características de audio. Pero, en casos como los siguientes, el hecho de adaptar un modelo base para que se ajuste a su audio puede mejorar el reconocimiento de voz:

-   El entorno de canal acústico es exclusivo. Por ejemplo, el entorno es ruidoso, la calidad del micrófono o el posicionamiento no son los óptimos, o el audio sufre de efectos de campo lejano.
-   Los patrones de voz de los oradores son atípicos. Por ejemplo, un orador habla muy rápido o el audio incluye conversaciones informales.
-   Los oradores tienen acentos muy pronunciados. Por ejemplo, el audio incluye oradores que hablan en un idioma que no es el nativo.

La interfaz de *personalización del modelo acústico* puede adaptar un modelo base a su entorno y a los oradores. Debe crear un modelo acústico personalizado y añadir datos de audio (recursos de audio) que se ajusten a la firma acústica del audio que desea transcribir. Cuando haya entrenado el modelo acústico personalizado con sus recursos de audio, puede utilizarlo para el reconocimiento de voz personalizado.

El tiempo que tarda el servicio en entrenar el modelo personalizado depende de la cantidad de datos de audio que contiene el modelo. En general, el entrenamiento tarda el doble de lo que dura el audio acumulado. El nivel de esfuerzo necesario para crear un modelo depende de los datos de audio que tenga disponibles para el modelo. También depende de si se utilizan transcripciones del audio.

Para obtener más información,
consulte

-   [Creación de un modelo acústico personalizado](/docs/services/speech-to-text?topic=speech-to-text-acoustic)
-   [Utilización de un modelo acústico personalizado](/docs/services/speech-to-text?topic=speech-to-text-acousticUse)

## Gramáticas
{: #grammars-intro}

Los modelos de lenguaje personalizado le permiten ampliar el vocabulario base del servicio. Las *gramáticas* le permiten restringir las palabras que el servicio puede reconocer de ese vocabulario. Cuando se utiliza una gramática con un modelo de lenguaje personalizado para el reconocimiento de voz, el servicio solo puede reconocer las palabras, frases y series que reconoce la gramática. Puesto que la gramática define un espacio de búsqueda limitado de coincidencias válidas, el servicio puede ofrecer resultados más rápido y con mayor precisión.

La tarea de añadir una gramática a un modelo de lenguaje personalizado y de entrenar el modelo es igual que para un corpus. No obstante, a diferencia de lo que sucede con el corpus, debe especificar de forma explícita que se va a utilizar una gramática con un modelo personalizado durante el reconocimiento de voz.

Para obtener más información,
consulte

-   [Utilización de gramáticas con modelos de lenguaje personalizado](/docs/services/speech-to-text?topic=speech-to-text-grammars)
-   [Adición de una gramática a un modelo de lenguaje personalizado](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd)
-   [Utilización de una gramática para el reconocimiento de voz](/docs/services/speech-to-text?topic=speech-to-text-grammarUse)

## Utilización conjunta de la personalización acústica y de lenguaje
{: #combined}

El uso de un modelo acústico personalizado puede mejorar las prestaciones de reconocimiento del servicio. Pero, si las transcripciones o el corpus relacionado están disponibles para el audio de ejemplo, puede utilizar dichos datos para mejorar aún más la calidad del reconocimiento de voz basado en el modelo acústico personalizado.

Mediante la creación de un modelo de lenguaje personalizado que complemente el modelo acústico personalizado, puede mejorar el reconocimiento de voz utilizando los dos modelos juntos. Cuando entrena un modelo acústico personalizado, puede especificar un modelo de lenguaje personalizado que incluya transcripciones de los recursos de audio o un vocabulario de palabras específicas del dominio procedentes de los recursos. De forma similar, cuando transcribe el audio, el servicio acepta un modelo de lenguaje personalizado, un modelo acústico personalizado, o ambos. Y, si su modelo de lenguaje personalizado incluye una gramática, puede utilizar este modelo y esta gramática con un modelo acústico personalizado para el reconocimiento de voz.

Para obtener más información, consulte [Utilización conjunta del modelo acústico personalizado y modelo de lenguaje personalizado](/docs/services/speech-to-text?topic=speech-to-text-useBoth).

Algunos idiomas no dan soporte a la personalización de lenguaje y acústica. Para obtener más información, consulte [Soporte de idiomas para la personalización](#languageSupport).
{: note}

## Soporte de idiomas para la personalización
{: #languageSupport}

La personalización del modelo de lenguaje y del modelo acústico solo está disponible para algunos idiomas. En la tabla siguiente se indica el nivel al que el servicio da soporte a la personalización para cada idioma.

-   *GA* indica que la interfaz está disponible a nivel general para su uso en producción.
-   *Beta* indica que la interfaz está disponible como oferta beta.
-   *No soportada* indica que la interfaz no está disponible para ese idioma.

Puede utilizar tanto modelos de banda ancha y como de banda estrecha con cualquier idioma soportado para el que estén disponibles. Si un idioma da soporte a la personalización del modelo de lenguaje, también da soporte a gramáticas. Para ver una lista de todos los modelos disponibles, consulte [Modelos de lenguaje soportados](/docs/services/speech-to-text?topic=speech-to-text-models#modelsList).

<table>
  <caption>Tabla 1. Soporte de idiomas para la personalización</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 24%">
      Idioma
    </th>
    <th style="text-align:center; vertical-align:bottom; width 37%">
      Soporte para personalización del modelo de lenguaje
    </th>
    <th style="text-align:center; vertical-align:bottom; width 37%">
      Soporte para personalización del modelo acústico
    </th>
  </tr>
  <tr>
    <td>Árabe (estándar moderno)</td>
    <td style="text-align:center">No soportado</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Portugués de Brasil</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Chino (mandarín)</td>
    <td style="text-align:center">No soportado</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Inglés (Reino Unido)</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Inglés (Estados Unidos)</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Francés</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Alemán</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Japonés</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Coreano</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Español (argentino)</td>
    <td style="text-align:center">Beta</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Español (castellano)</td>
    <td style="text-align:center">GA</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Español (chileno)</td>
    <td style="text-align:center">Beta</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Español (colombiano)</td>
    <td style="text-align:center">Beta</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Español (mexicano)</td>
    <td style="text-align:center">Beta</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Español (peruano)</td>
    <td style="text-align:center">Beta</td>
    <td style="text-align:center">Beta</td>
  </tr>
</table>

Puede utilizar los métodos `GET /v1/models` y `GET /v1/models/{model_id}` para comprobar si un modelo de lenguaje da soporte a la personalización del modelo de lenguaje. Si un modelo base da soporte a la personalización del modelo de lenguaje, el campo `supported_features` de la salida del método para el modelo establece el campo `custom_language_model` en `true`.

## Notas de uso sobre la personalización
{: #customUsage}

Las siguientes notas de uso se aplican a la personalización del modelo de lenguaje y a la personalización del modelo acústico.

### Propiedad de los modelos personalizados
{: #customOwner}

Un modelo personalizado es propiedad de la instancia del servicio {{site.data.keyword.speechtotextshort}} cuyas credenciales se utilizan para crearlo. Para trabajar con el modelo personalizado de cualquier forma, debe utilizar las credenciales para dicha instancia del servicio con los métodos de la interfaz de personalización. Las credenciales que se crean para otras instancias del servicio no pueden ver ni acceder al modelo personalizado.

Todas las credenciales que se obtienen para la misma instancia del servicio {{site.data.keyword.speechtotextshort}} comparten acceso a todos los modelos personalizados creados para dicha instancia de servicio. Para restringir el acceso a un modelo personalizado, cree otra instancia del servicio y utilice únicamente las credenciales para dicha instancia del servicio para crear y para trabajar con el modelo. Las credenciales correspondientes a otras instancias de servicio no pueden afectar al modelo personalizado.

Una de las ventajas de compartir propiedad entre credenciales correspondientes a una instancia de servicio es que puede cancelar un conjunto de credenciales, por ejemplo si se ven comprometidas. Luego puede crear nuevas credenciales para la misma instancia de servicio y mantener la propiedad y el acceso a los modelos personalizados creados con las credenciales originales.

### Registro de solicitudes y privacidad de los datos
{: #customLogging}

La forma en que el servicio maneja el registro de solicitudes correspondiente a llamadas a la interfaz de personalización depende de la solicitud:

-   El servicio *no* registra los datos que se utilizan para crear modelos personalizados. Por ejemplo, cuando se trabaja con corpus y con y palabras en un modelo de lenguaje personalizado, no es necesario establecer la cabecera de solicitud `X-Watson-Learning-Opt-Out`. Los datos de entrenamiento nunca se utilizan para mejorar los modelos base del servicio.
-   El servicio *registra* los datos cuando se utiliza un modelo personalizado con una solicitud de reconocimiento. Debe establecer la cabecera de solicitud `X-Watson-Learning-Opt-Out` en `true` para evitar que se registren las solicitudes de reconocimiento.

Para obtener más información, consulte [Registro de solicitudes](/docs/services/speech-to-text?topic=speech-to-text-input#logging).

### Seguridad de la información
{: #customSecurity}

Puede asociar un ID de cliente con los datos que se añaden o se actualizan para los modelos de lenguaje personalizado y acústico personalizado. Para asociar un ID de cliente a un corpus, a palabras personalizadas, a gramáticas y a recursos de audio, pase la cabecera `X-Watson-Metadata` con los siguientes métodos:

-   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
-   `POST /v1/customizations/{customization_id}/words`
-   `PUT /v1/customizations/{customization_id}/words/{word_name}`
-   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`
-   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

Si es necesario, luego puede suprimir los datos con el método `DELETE /v1/user_data`. Para obtener más información, consulte [Seguridad de la información](/docs/services/speech-to-text?topic=speech-to-text-information-security).
