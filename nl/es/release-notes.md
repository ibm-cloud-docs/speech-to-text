---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-11"

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

# Notas del release
{: #release-notes}

En las secciones siguientes se describen las características nuevas y los cambios que se han incluido para cada release y actualización del servicio {{site.data.keyword.speechtotextfull}}. La información incluye todas las limitaciones conocidas. A menos que se indique lo contrario, todos los cambios son compatibles con releases anteriores y se encuentran disponibles de forma automática y transparente para todas las aplicaciones nuevas y existentes.
{: shortdesc}

## Limitaciones conocidas
{: #limitations}

No hay ninguna limitación conocida en este momento.

## 3 de abril de 2019
{: #April2019}

Ahora los modelos acústicos personalizados aceptan un máximo de 200 horas de audio. El límite máximo anterior era de 100 horas de audio.

## 21 de marzo de 2019
{: #March2019d}

Ahora los usuarios solo pueden ver información de credenciales de servicio asociada con el rol que se ha asignado a su cuenta de {{site.data.keyword.cloud_notm}}. Por ejemplo, si tiene asignado el rol de `lector`, ya no puede ver ninguna credencial de servicio de `escritor` o de niveles superiores.

Este cambio no afecta al acceso a la API para usuarios o aplicaciones con las credenciales de servicio existentes. El cambio solo afecta a la visualización de credenciales dentro de {{site.data.keyword.cloud_notm}}.

Para obtener más información sobre las claves de servicio y los roles de usuario, consulte [Claves de API del servicio IAM](/docs/services/watson?topic=watson-api-key-bp#api-key-bp).

## 15 de marzo de 2019
{: #March2019c}

Ahora el servicio da soporte al audio en formato de A-law (`audio/alaw`). Para obtener más información, consulte [Formato audio/alaw](/docs/services/speech-to-text/audio-formats.html#alaw).

## Releases anteriores
{: #older}

-   [11 de marzo de 2019](#March2019b)
-   [4 de marzo de 2019](#March2019)
-   [28 de enero de 2019](#January2019)
-   [20 de diciembre de 2018](#December2018b)
-   [13 de diciembre de 2018](#December2018a)
-   [12 de noviembre de 2018](#November2018b)
-   [7 de noviembre de 2018](#November2018a)
-   [30 de octubre de 2018](#October2018b)
-   [9 de octubre de 2018](#October2018a)
-   [10 de septiembre de 2018](#September2018b)
-   [7 de septiembre de 2018](#September2018a)
-   [8 de agosto de 2018](#August2018)
-   [13 de julio de 2018](#July2018)
-   [12 de junio de 2018](#June2018)
-   [15 de mayo de 2018](#May2018)
-   [26 de marzo de 2018](#March2018b)
-   [1 de marzo de 2018](#March2018a)
-   [1 de febrero de 2018](#February2018)
-   [14 de diciembre de 2017](#December2017)
-   [2 de octubre de 2017](#October2017)
-   [14 de julio de 2017](#July2017b)
-   [1 de julio de 2017](#July2017a)
-   [22 de mayo de 2017](#May2017)
-   [10 de abril de 2017](#April2017)
-   [8 de marzo de 2017](#March2017)
-   [1 de diciembre de 2016](#December2016)
-   [22 de septiembre de 2016](#September2016)
-   [30 de junio de 2016](#June2016b)
-   [23 de junio de 2016](#June2016a)
-   [10 de marzo de 2016](#March2016)
-   [19 de enero de 2016](#January2016)
-   [17 de diciembre de 2015](#December2015)
-   [21 de septiembre de 2015](#September2015)
-   [1 de julio de 2015](#July2015)

### 11 de marzo de 2019
{: #March2019b}

-   Para el parámetro `max_alternatives`, el servicio acepta de nuevo el valor `0`. Si especifica `0`, el servicio utiliza automáticamente el valor predeterminado, `1`. Un cambio realizado para la actualización del servicio del 4 de marzo hizo que el valor `0` devolviera un error. (El servicio devuelve un error si especifica un valor negativo.)
-   Para el parámetro `word_alternatives_threshold`, el servicio acepta de nuevo el valor `0`. Un cambio realizado para la actualización del servicio del 4 de marzo hizo que el valor `0` devolviera un error. (El servicio devuelve un error si especifica un valor negativo.)
-   Ahora el servicio devuelve todas las puntuaciones de confianza con una precisión máxima de dos posiciones decimales. Esto incluye las puntuaciones de confianza correspondientes a transcripciones, confianza de palabras, alternativas a palabras, resultados de palabra clave y las etiquetas de orador.

### 4 de marzo de 2019
{: #March2019}

Los siguientes modelos de lenguaje de banda estrecha se han actualizado para mejorar el reconocimiento de voz:

-   `es-ES_NarrowbandModel`
-   `fr-FR_NarrowbandModel`
-   `pt-BR_NarrowbandModel`

De forma predeterminada, el servicio utiliza automáticamente los modelos actualizados para todas las solicitudes de reconocimiento de voz. Si tiene modelos de lenguaje personalizado o acústico personalizado que se basan en los modelos, debe actualizar los modelos personalizados existentes para aprovechar las actualizaciones mediante los métodos siguientes:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Para obtener más información, consulte [Actualización de modelos personalizados](/docs/services/speech-to-text/custom-upgrade.html).

### 28 de enero de 2019
{: #January2019}

Ahora la interfaz WebSocket da soporte a la autenticación de IAM (Identity and Access Management) basada en señales desde el código JavaScript basado en navegador. Se ha eliminado la limitación de lo contrario. Para establecer una conexión autenticada con el método WebSocket `/v1/recognize`:

-   Si utiliza la autenticación de IAM, incluya el parámetro de consulta `access_token`.
-   Si utiliza las credenciales de servicio de Cloud Foundry, incluya el parámetro de consulta `watson-token`.

Para obtener más información, consulte el apartado sobre cómo [Abrir una conexión](/docs/services/speech-to-text/websockets.html#WSopen).

### 20 de diciembre de 2018
{: #December2018b}

La interfaz de gramática está completamente funcional en todas las ubicaciones desde el 8 de enero de 2019.
{: note}

-   Ahora el servicio da soporte a gramáticas para el reconocimiento de voz. Las gramáticas están disponibles como funciones beta para todos los idiomas que dan soporte a la personalización del modelo de lenguaje. Puede añadir gramáticas a un modelo de lenguaje personalizado y utilizarlas para restringir el conjunto de frases que el servicio puede reconocer del audio. Puede definir una gramática en formato Augmented Backus-Naur Form (ABNF) o en formato XML.

    Están disponibles los cuatro métodos siguientes para trabajar con gramáticas:
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` añade un archivo de gramática a un modelo de lenguaje personalizado.
    -   `GET /v1/customizations/{customization_id}/grammars` muestra información sobre todas las gramáticas correspondientes a un modelo personalizado.
    -   `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` devuelve información sobre la gramática especificada para un modelo personalizado.
    -   `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` elimina una gramática existente de un modelo personalizado.

    Puede utilizar una gramática para el reconocimiento de voz con las interfaces de WebSocket y HTTP. Utilice los parámetros `language_customization_id` y `grammar_name` para identificar el modelo personalizado y la gramática que desea utilizar. Actualmente, solo puede utilizar una gramática con una solicitud de reconocimiento de voz.

    Para obtener más información sobre gramáticas, consulte la documentación siguiente:
    -   [Utilización de gramáticas con modelos de lenguaje personalizado](/docs/services/speech-to-text/grammar.html)
    -   [Visión general de las gramáticas](/docs/services/speech-to-text/grammar-understand.html)
    -   [Adición de una gramática a un modelo de lenguaje personalizado](/docs/services/speech-to-text/grammar-add.html)
    -   [Utilización de una gramática para el reconocimiento de voz](/docs/services/speech-to-text/grammar-use.html)
    -   [Gestión de gramáticas](/docs/services/speech-to-text/grammar-manage.html)
    -   [Gramáticas de ejemplo](/docs/services/speech-to-text/grammar-examples.html)

    Para obtener información acerca de todos los métodos de la interfaz, revise la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Ahora dispone de una nueva característica de ocultación numérica para enmascarar números que tienen tres o más dígitos consecutivos. La ocultación sirve para eliminar la información personal confidencial, como números de tarjetas de crédito, de las transcripciones. Para habilitar esta característica, establezca el parámetro `redaction` en `true` en una solicitud de reconocimiento. La característica es una funcionalidad beta que solo está disponible en inglés de EE. UU., japonés y coreano. Para obtener más información, consulte [Ocultación numérica](/docs/services/speech-to-text/output.html#redaction).
-   Los siguientes nuevos modelos de alemán y francés están ahora disponibles con el servicio:
    -   `de-DE_NarrowbandModel`
    -   `fr-FR_NarrowbandModel`

    Los dos nuevos modelos dan soporte a la personalización de modelos de lenguaje (GA) y a la personalización de modelos acústicos (beta). Para obtener más información, consulte [Soporte de idiomas para la personalización](/docs/services/speech-to-text/custom.html#languageSupport).
-   Ya está disponible un nuevo modelo de idioma inglés de Estados Unidos, `en-US_ShortForm_NarrowbandModel`. El nuevo modelo está pensado para que se utilice en soluciones de respuesta de voz interactivas y de soporte automatizado al cliente. El modelo da soporte a la personalización de modelos de lenguaje (GA) y a la personalización de modelos acústicos (beta).
-   Los siguientes modelos de lenguaje se han actualizado para mejorar el reconocimiento de voz:
    -   `en-GB_NarrowbandModel`
    -   `es-ES_NarrowbandModel`

    De forma predeterminada, el servicio utiliza automáticamente los modelos actualizados para todas las solicitudes de reconocimiento de voz. Si tiene modelos de lenguaje personalizado o acústico personalizado que se basan en los modelos, debe actualizar los modelos personalizados existentes para aprovechar las actualizaciones mediante los métodos siguientes:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Para obtener más información, consulte [Actualización de modelos personalizados](/docs/services/speech-to-text/custom-upgrade.html).
-   Ahora el servicio da soporte a audio en el formato G.729 (`audio/g729`). El servicio solo da soporte a G.729 Anexo D para audio de banda estrecha. Para obtener más información acerca de los formatos de audio admitidos, consulte [Formatos de audio](/docs/services/speech-to-text/audio-formats.html).
-   La característica de etiquetas de orador está ahora disponible para el modelo de banda estrecha para inglés de Reino Unido (`en-GB_NarrowbandModel`). La característica es una funcionalidad beta para todos los idiomas soportados. Para obtener más información, consulte [Etiquetas de orador](/docs/services/speech-to-text/output.html#speaker_labels).
-   La duración máxima de audio que se puede añadir a un modelo acústico personalizado ha aumentado de 50 horas a 100 horas.

### 13 de diciembre de 2018
{: #December2018a}

El servicio está ahora disponible en la ubicación Londres de {{site.data.keyword.cloud_notm}} (**eu-gb**). Al igual que todas las ubicaciones, Londres utiliza la autenticación de IAM basada en señales. Todas las nuevas instancias del servicio que se crean en esta ubicación utilizan la autenticación de IAM.

### 12 de noviembre de 2018
{: #November2018b}

Ahora el servicio da soporte al formateo inteligente para el reconocimiento de voz en japonés. Anteriormente, el servicio solo daba soporte al formateo inteligente en inglés de Estados Unidos y español. La característica es una funcionalidad beta para todos los idiomas soportados. Para obtener más información, consulte [Formateo inteligente](/docs/services/speech-to-text/output.html#smart_formatting).

### 7 de noviembre de 2018
{: #November2018a}

El servicio está ahora disponible en la ubicación Tokio de {{site.data.keyword.cloud_notm}} (**jp-tok**). Al igual que todas las ubicaciones, Tokio utiliza la autenticación de IAM basada en señales. Todas las nuevas instancias del servicio que se crean en esta ubicación utilizan la autenticación de IAM.

### 30 de octubre de 2018
{: #October2018b}

El servicio se ha migrado a la autenticación de IAM basada en señales para todas las ubicaciones. Todos los servicios de {{site.data.keyword.cloud_notm}} utilizan ahora la autenticación de IAM. El servicio {{site.data.keyword.speechtotextshort}} se ha migrado en cada ubicación en las fechas siguientes:

-   Dalas (**us-south**): 30 de octubre de 2018
-   Frankfurt (**eu-de**): 30 de octubre de 2018
-   Washington, DC (**us-east**): 12 de junio de 2018
-   Sídney (**au-syd**): 15 de mayo de 2018

La migración a la autenticación de IAM afecta a las instancias nuevas y existentes del servicio de forma diferente:

-   *Todas las nuevas instancias del servicio que se crean en cualquier ubicación* ahora utilizan la autenticación de IAM para acceder al servicio. Puede pasar una señal de portadora o una clave de API: las señales admiten solicitudes autenticadas sin incluir credenciales de servicio en cada llamada; las claves de API utilizan la autenticación básica HTTP. Cuando utilice cualquiera de los SDK de {{site.data.keyword.watson}}, puede pasar la clave de API y dejar que el SDK gestione el ciclo de vida de las señales.
-   *Las instancias del servicio existentes que ha creado en una ubicación antes de la fecha de migración indicada* siguen utilizando `{username}` y `{password}` de sus credenciales de servicio de Cloud Foundry anteriores para la autenticación hasta que las migra para que utilicen la autenticación de IAM. Para obtener más información sobre cómo migrar a la autenticación de IAM, consulte [Migración de instancias de servicio de Cloud Foundry a un grupo de recursos](https://{DomainName}/docs/resources/instance_migration.html).

Para obtener más información, consulte la documentación siguiente:

-   Para obtener información sobre qué mecanismo de autenticación utiliza su servicio, consulte las credenciales de servicio pulsando la instancia en el panel de control de [{{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/dashboard/apps){: new_window}.
-   Para obtener más información sobre el uso de las señales de IAM con los servicios Watson, consulte [Autenticación con señales de IAM](/docs/services/watson/getting-started-iam.html).
-   Para obtener más información sobre el uso de claves de API de IAM con los servicios Watson, consulte [Claves de API de servicio de IAM](/docs/services/watson/apikey-bp.html).
-   Para ver ejemplos en los que se utiliza la autenticación de IAM, revise la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 9 de octubre de 2018
{: #October2018a}

-   La cabecera `Content-Type` es ahora opcional para las solicitudes de reconocimiento de voz. Ahora el servicio detecta automáticamente el formato de audio (tipo de MIME) de la mayoría de audio. Debe continuar especificando el tipo de contenido para los formatos siguientes:
    -   `audio/basic`
    -   `audio/l16`
    -   `audio/mulaw`

    Donde se indique, el tipo de contenido que especifique para estos formatos debe incluir la frecuencia de muestreo y puede incluir opcionalmente el número de canales y el valor endianness del audio. Para los demás formatos de audio, puede omitir el tipo de contenido o puede especificar un tipo de contenido `application/octet-stream` para que el servicio detecte automáticamente el formato.

    Cuando utilice el mandato `curl` para realizar una solicitud de reconocimiento de voz con la interfaz HTTP, debe especificar el formato de audio con la cabecera `Content-Type`, especificar `"Content-Type: application/octet-stream"` o especificar `"Content-Type:"`. Si omite la cabecera por completo, `curl` utiliza el valor predeterminado `application/x-www-form-urlencoded`. En la mayoría de los ejemplos de esta documentación se sigue especificando el formato de las solicitudes de reconocimiento de voz, independientemente de si es o no necesario.
    {: important}

    Este cambio se aplica a los métodos siguientes:
    -   `/v1/recognize` para solicitudes WebSocket. El campo `content-type` del mensaje de texto que se envía para iniciar una solicitud a través de una conexión WebSocket abierto es ahora opcional.
    -   `POST /v1/recognize` para solicitudes HTTP síncronas. La cabecera `Content-Type` es ahora opcional. (Para las solicitudes de varias partes, el campo `part_content_type` de los metadatos JSON también es ahora opcional.)
    -   `POST /v1/recognitions` para solicitudes HTTP asíncronas. La cabecera `Content-Type` es ahora opcional.

    Para obtener más información, consulte [Formatos de audio](/docs/services/speech-to-text/audio-formats.html).
-   El modelo de banda ancha en portugués de Brasil, `pt-BR_BroadbandModel`, se ha actualizado para mejorar el reconocimiento de voz. De forma predeterminada, el servicio utiliza automáticamente el modelo actualizado para todas las solicitudes de reconocimiento. Si tiene modelos de lenguaje personalizado o acústico personalizado que se basan en este modelo, debe actualizar los modelos personalizados existentes para aprovechar las actualizaciones mediante los métodos siguientes:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Para obtener más información, consulte [Actualización de modelos personalizados](/docs/services/speech-to-text/custom-upgrade.html).
-   El parámetro `customization_id` de los métodos de reconocimiento de voz ha quedado en desuso y se eliminará en futuros releases. Para especificar un modelo de lenguaje personalizado para una solicitud de reconocimiento de voz, utilice en su lugar el parámetro `language_customization_id`. Este cambio se aplica a los métodos siguientes:
    -   `/v1/recognize` para solicitudes WebSocket
    -   `POST /v1/recognize` para solicitudes HTTP síncronas (incluidas las solicitudes de varias partes)
    -   `POST /v1/recognitions` para solicitudes HTTP asíncronas
-   A partir del 1 de octubre de 2018, ahora se le facturará todo el audio que pase al servicio para el reconocimiento de voz. Los mil primeros minutos de audio que envíe cada mes ya no son gratuitos. Para obtener más información sobre los planes de precios del servicio, consulte el servicio {{site.data.keyword.speechtotextshort}} en el [catálogo de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/catalog/services/speech-to-text){: new_window}.

### 10 de septiembre de 2018
{: #September2018b}

Para ver una lista de los problemas que se han solucionado desde el release inicial, consulte [Problemas resueltos](#known_issues).
{: important}

-   Ahora el servicio da soporte a un modelo de banda ancha en alemán, `de-DE_BroadbandModel`. El nuevo modelo en alemán da soporte a la personalización de modelos de lenguaje (disponible a nivel general) y a la personalización de modelos acústicos (beta).
    -   Para obtener información acerca de cómo el servicio analiza el corpus en alemán, consulte [Análisis de inglés, francés, alemán, español y portugués de Brasil](/docs/services/speech-to-text/language-resource.html#corpusLanguages).
    -   Para obtener más información acerca de cómo crear pronunciaciones de sonidos similares de palabras personalizadas en alemán, consulte [Directrices para el francés, alemán, español y portugués de Brasil](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR).
-   Los modelos existentes en portugués de Brasil, `pt-BR_BroadbandModel` y `pt-BR_NarrowbandModel`, ahora dan soporte a la personalización del modelo de lenguaje (disponible a nivel general). Los modelos no se actualizaron para habilitar este soporte, por lo que no es necesario actualizar los modelos acústicos personalizados existentes.
    -   Para obtener información acerca de cómo el servicio analiza el corpus en portugués de Brasil, consulte [Análisis de inglés, francés, alemán, español y portugués de Brasil](/docs/services/speech-to-text/language-resource.html#corpusLanguages).
    -   Para obtener más información acerca de cómo crear pronunciaciones de sonidos similares de palabras personalizadas en portugués de Brasil, consulte [Directrices para el francés, alemán, español y portugués de Brasil](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR).
-   Hay nuevas versiones de los modelos de banda ancha y banda ancha en inglés y japonés disponibles:
    -   `en-US_BroadbandModel`
    -   `en-US_NarrowbandModel`
    -   `ja-JP_BroadbandModel`
    -   `ja-JP_NarrowbandModel`

    Los nuevos modelos ofrecen un mejor reconocimiento de voz. De forma predeterminada, el servicio utiliza automáticamente los modelos actualizados para todas las solicitudes de reconocimiento. Si tiene modelos de lenguaje personalizado o acústico personalizado que se basan en estos modelos, debe actualizar los modelos personalizados existentes para aprovechar las actualizaciones mediante los métodos siguientes:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Para obtener más información, consulte [Actualización de modelos personalizados](/docs/services/speech-to-text/custom-upgrade.html).
-   Las características de detección de palabras clave y alternativas a palabras están actualmente disponibles a nivel general (GA) en lugar de ser una funcionalidad beta para todos los idiomas. Para obtener más información,
consulte
    -   [Detección de palabras clave](/docs/services/speech-to-text/output.html#keyword_spotting)
    -   [Alternativas a palabras](/docs/services/speech-to-text/output.html#word_alternatives)

### Problemas resueltos
{: #known_issues}

Se han resuelto los siguientes problemas conocidos asociados con la interfaz de personalización. Esta lista se mantiene para los usuarios que puedan haber experimentado los problemas en el pasado.

-   Si añade datos a un modelo de lenguaje personalizado o a un modelo acústico personalizado, debe volver a entrenar el modelo antes de utilizarlo para el reconocimiento de voz. El problema se produce en el siguiente caso:

    1.  El usuario crea un nuevo modelo personalizado (de lenguaje o acústico) y entrena el modelo.
    1.  El usuario añade recursos adicionales (palabras, corpus o audio) al modelo personalizado, pero no vuelve a entrenar el modelo.
    1.  El usuario no puede utilizar el modelo personalizado para el reconocimiento de voz. El servicio devuelve un error con el siguiente formato cuando se utiliza con una solicitud de reconocimiento de voz:

        ```javascript
        {
          "code_description": "Bad Request",
          "code": 400,
          "error": "Requested custom language model is not available.
                    Please make sure the custom model is trained."
        }
        ```
        {: codeblock}

    Para solucionar temporalmente este problema, el usuario debe volver a entrenar el modelo personalizado con sus datos más recientes. A continuación, el usuario puede utilizar el modelo personalizado con el reconocimiento de voz.
-   Antes de entrenar un modelo de lenguaje personalizado o un modelo acústico personalizado, debe actualizarlo a la versión más reciente de su modelo base. El problema se produce en el siguiente caso:

    1.  El usuario tiene un modelo personalizado existente (de lenguaje o acústico) que se basa en un modelo que se ha actualizado.
    1.  El usuario entrena el modelo personalizado existente con la versión antigua del modelo base sin actualizar primero a la versión más reciente del modelo base.
    1.  El usuario no puede utilizar el modelo personalizado para el reconocimiento de voz.

    Para solucionar temporalmente este problema, el usuario debe utilizar el método `POST /v1/customizations/{customization_id}/upgrade_model` o `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` para actualizar el modelo personalizado a la versión más reciente de su modelo base. A continuación, el usuario puede utilizar el modelo personalizado con el reconocimiento de voz.

Ambos problemas se han solucionado en producción.

### 7 de septiembre de 2018
{: #September2018a}

La interfaz REST de HTTP basada en sesión ya no recibe soporte. Toda la información relacionada con las sesiones se ha eliminado de la documentación. Los métodos siguientes ya no están disponibles:
{: important}

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

Si la aplicación utiliza la interfaz de sesiones, debe migrar a una de las restantes interfaces REST de HTTP o a la interfaz WebSocket. Para obtener más información, consulte la actualización del servicio del [8 de agosto de 2018](#August2018).

### 8 de agosto de 2018
{: #August2018}

La interfaz REST de HTTP basada en sesión ha quedado en desuso desde el **8 de agosto de 2018**. Todos los métodos de la API de sesiones se eliminarán del servicio a partir del **7 de septiembre de 2018**, después de lo cual ya no podrá utilizar la interfaz basada en sesión. Este aviso de desuso y de eliminación en 30 días se aplica a los métodos siguientes:

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

Si la aplicación utiliza la interfaz de sesiones, debe migrar a una de las siguientes interfaces antes del 7 de septiembre:
{: important}

-   Para el reconocimiento de voz basado en secuencia (incluidos los casos de retransmisión en directo), utilice la [interfaz WebSocket](/docs/services/speech-to-text/websockets.html), que proporciona acceso a resultados provisionales y la latencia más baja.
-   Para el reconocimiento de voz basado en archivos, utilice una de las interfaces siguientes:

    -   Para archivos cortos de unos pocos minutos de audio, utilice la [interfaz HTTP síncrona](/docs/services/speech-to-text/http.html) `(POST /v1/recognize`) o la [interfaz HTTP asíncrona](/docs/services/speech-to-text/async.html) (`POST /v1/recognitions`).
    -   Para archivos más largos de más de unos pocos minutos de audio, utilice la interfaz HTTP asíncrona. La interfaz HTTP asíncrona acepta hasta 1 GB de datos de audio con una sola solicitud.

Las interfaces WebSocket y HTTP proporcionan los mismos resultados que la interfaz de sesiones (solo la interfaz WebSocket proporciona resultados provisionales). También puede utilizar uno de los SDK Watson, lo que simplifica el desarrollo de aplicaciones con cualquiera de las interfaces. Para obtener más información, revise la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 13 de julio de 2018
{: #July2018}

El modelo en español de banda estrecha, `es-ES_NarrowbandModel`, se ha actualizado para mejorar el reconocimiento de voz. De forma predeterminada, el servicio utiliza automáticamente el modelo actualizado para todas las solicitudes de reconocimiento. Si tiene modelos de lenguaje personalizado o acústico personalizado que se basan en este modelo, debe actualizar los modelos personalizados para aprovechar las actualizaciones mediante los métodos siguientes:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Para obtener más información, consulte [Actualización de modelos personalizados](/docs/services/speech-to-text/custom-upgrade.html).

A partir de esta actualización, se encuentran disponibles las dos versiones siguientes del modelo en español de banda estrecha:

-   `es_ES.8kHz.general.lm20180522235959.am20180522235959` (actual)
-   `es_ES.8kHz.general.lm20180308235959.am20180308235959` (anterior)

La versión siguiente del modelo ya no está disponible:

-   `es_ES.8kHz.general.lm20171031235959.am20171031235959`

Una solicitud de reconocimiento que intenta utilizar un modelo personalizado basado en el modelo base que ahora no está disponible utiliza el modelo base más reciente sin ninguna personalización. El servicio devuelve el mensaje de aviso siguiente: `Se utiliza el modelo base predeterminado no personalizado, porque el modelo personalizado {type} se ha creado con una versión del modelo base que ya no recibe soporte.` Para reanudar el uso de un modelo personalizado basado en el modelo no disponible, primero debe actualizar el modelo utilizando el método `upgrade_model` adecuado descrito anteriormente.

### 12 de junio de 2018
{: #June2018}

Se han habilitado las siguientes características para las aplicaciones alojadas en Washington, DC (**us-east**):

-   Ahora el servicio da soporte a un nuevo proceso de autenticación de API. Para obtener más información, consulte la [actualización de servicio del 30 de octubre de 2018](#October2018b).
-   Ahora el servicio da soporte a la cabecera `X-Watson-Metadata` y al método `DELETE /v1/user_data`. Para obtener más información, consulte [Seguridad de la información](/docs/services/speech-to-text/information-security.html).

### 15 de mayo de 2018
{: #May2018}

Se han habilitado las siguientes características para las aplicaciones alojadas en Sídney (**au-syd**):

-   Ahora el servicio da soporte a un nuevo proceso de autenticación de API. Para obtener más información, consulte la [actualización de servicio del 30 de octubre de 2018](#October2018b).
-   Ahora el servicio da soporte a la cabecera `X-Watson-Metadata` y al método `DELETE /v1/user_data`. Para obtener más información, consulte [Seguridad de la información](/docs/services/speech-to-text/information-security.html).

### 26 de marzo de 2018
{: #March2018b}

-   Ahora el servicio da soporte a la personalización del modelo de lenguaje para el modelo de lenguaje en francés, `fr-FR_BroadbandModel`. El modelo en francés está disponible a nivel general para el uso en producción con personalización de modelo de lenguaje.
    -   Para obtener más información acerca de cómo el servicio analiza el corpus en francés, consulte [Análisis de inglés, francés, alemán, español y portugués de Brasil](/docs/services/speech-to-text/language-resource.html#corpusLanguages).
    -   Para obtener más información acerca de cómo crear pronunciaciones de sonidos similares de palabras personalizadas en francés, consulte [Directrices para el francés, alemán, español y portugués de Brasil](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR).
-   Los modelos de banda estrecha en español y en coreano, `es-ES_NarrowbandModel` y `ko-KR_NarrowbandModel`, y el modelo de banda ancha en francés, `fr-FR_BroadbandModel`, se han actualizado para mejorar el reconocimiento de voz. De forma predeterminada, el servicio utiliza automáticamente los modelos actualizados para todas las solicitudes de reconocimiento. Si tiene modelos de lenguaje personalizado o acústico personalizado que se basan en alguno de estos modelos, debe actualizar los modelos personalizados para aprovechar las actualizaciones mediante los métodos siguientes:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Para obtener más información, consulte [Actualización de modelos personalizados](/docs/services/speech-to-text/custom-upgrade.html).
-   Se ha cambiado el nombre del parámetro `version` de los métodos siguientes por `base_model_version`:

    -   `/v1/recognize` para solicitudes WebSocket
    -   `POST /v1/recognize` para solicitudes HTTP sin sesión
    -   `POST /v1/sessions` para solicitudes HTTP basadas en sesión
    -   `POST /v1/recognitions` para solicitudes HTTP asíncronas

    El parámetro `base_model_version` especifica la versión de un modelo base que se va a utilizar para el reconocimiento de voz. Para obtener más información, consulte [Cómo realizar solicitudes de reconocimiento con modelos personalizados actualizados](/docs/services/speech-to-text/custom-upgrade.html#upgradeRecognition) y [Versión del modelo base](/docs/services/speech-to-text/input.html#version).
-   El formateo inteligente ahora recibe soporte para español, así como para inglés de EE. UU. Para inglés de EE. UU., la característica ahora también convierte series de palabras clave en signos de puntuación para puntos, comas, signos de interrogación y signos de exclamación. Para obtener más información, consulte [Formateo inteligente](/docs/services/speech-to-text/output.html#smart_formatting).

### 1 de marzo de 2018
{: #March2018a}

Los modelos de banda ancha en español y francés, `es-ES_BroadbandModel` y `fr-FR_BroadbandModel`, se han actualizado para mejorar el reconocimiento de voz. De forma predeterminada, el servicio utiliza automáticamente los modelos actualizados para todas las solicitudes de reconocimiento. Si tiene modelos de lenguaje personalizado o acústico personalizado que se basan en alguno de estos modelos, debe actualizar los modelos personalizados para aprovechar las actualizaciones mediante los métodos siguientes:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Para obtener más información, consulte [Actualización de modelos personalizados](/docs/services/speech-to-text/custom-upgrade.html). En la sección se muestran las reglas para actualizar modelos personalizados, los efectos de la actualización y los métodos para utilizar modelos actualizados.

### 1 de febrero de 2018
{: #February2018}

Ahora el servicio ofrece modelos para el idioma coreano para el reconocimiento de voz: `ko-KR_BroadbandModel` para el audio que se muestrea como mínimo de 16 kHz y `ko-KR_NarrowbandModel` para el audio que se muestrea a un mínimo de 8 kHz. Para obtener más información, consulte [Idiomas y modelos](/docs/services/speech-to-text/models.html).

Para la personalización del modelo de lenguaje, los modelos en coreano están disponibles a nivel general para su uso en producción; para la personalización del modelo acústico, son funciones beta. Para obtener más información, consulte [Soporte de idiomas para la personalización](/docs/services/speech-to-text/custom.html#languageSupport).

-   Para obtener más información sobre la forma en que el servicio analiza el corpus en coreano, consulte [Análisis de coreano](/docs/services/speech-to-text/language-resource.html#corpusLanguages-koKR).
-   Para obtener más información sobre cómo crear pronunciaciones de palabras personalizadas en coreano, consulte [Directrices para coreano](/docs/services/speech-to-text/language-resource.html#wordLanguages-koKR).

### 14 de diciembre de 2017
{: #December2017}

-   Los modelos en inglés de EE. UU., `en-US_BroadbandModel` y `en-US_NarrowbandModel`, se han actualizado para mejorar el reconocimiento de voz. De forma predeterminada, el servicio utiliza automáticamente los modelos actualizados para todas las solicitudes de reconocimiento. Si tiene modelos de lenguaje personalizado o acústico personalizado que se basan en los modelos en inglés de EE. UU., debe actualizar los modelos personalizados para aprovechar las actualizaciones mediante los métodos siguientes:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Para obtener más información sobre el procedimiento, consulte [Actualización de modelos personalizados](/docs/services/speech-to-text/custom-upgrade.html). En la sección se muestran las reglas para actualizar modelos personalizados, los efectos de la actualización y los métodos para utilizar modelos actualizados. Actualmente, los métodos solo se aplican a los nuevos modelos base en inglés de Estados Unidos. Sin embargo, la misma información se aplicará a las actualizaciones de otros modelos base a medida que estén disponibles.
-   Los diversos métodos para realizar solicitudes de reconocimiento ahora incluyen un nuevo parámetro `base_model_version` que puede utilizar para iniciar solicitudes que utilicen las versiones antigua o actualizada de los modelos base y personalizado. Aunque está pensado principalmente para que se utilice con modelos personalizados que se han actualizado, el parámetro `base_model_version` también se puede utilizar sin modelos personalizados. Para obtener más información, consulte [Versión del modelo base](/docs/services/speech-to-text/input.html#version).
-   Ahora el servicio da soporte a la personalización del modelo acústico como funcionalidad beta para todos los idiomas disponibles. Puede crear modelos acústicos personalizados para los modelos de banda ancha o de banda estrecha para todos los idiomas. Para ver una introducción a la personalización, que incluye la personalización del modelo acústico, consulte [La interfaz de personalización](/docs/services/speech-to-text/custom.html).
-   Ahora el servicio da soporte a la personalización del modelo de lenguaje para los modelos en inglés del Reino Unido, `en-GB_BroadbandModel` y `en-GB_NarrowbandModel`. Aunque el servicio maneja las palabras del corpus y las personalizadas de inglés del Reino Unido y de Estados Unidos de forma similar, existen algunas diferencias importantes:
    -   Para obtener más información acerca de cómo el servicio analiza el corpus en inglés del reino Unido, consulte [Análisis de inglés, francés, alemán, español y portugués de Brasil](/docs/services/speech-to-text/language-resource.html#corpusLanguages).
    -   Para obtener más información sobre cómo crear pronunciaciones de palabras personalizadas en inglés del Reino Unido, consulte [Directrices para inglés (EE. UU. y Reino Unido)](/docs/services/speech-to-text/language-resource.html#wordLanguages-enUS-enGB). Específicamente, para inglés del Reino Unido no se pueden utilizar puntos ni guiones en las pronunciaciones.
-   La personalización del modelo de lenguaje y todos los parámetros asociados están disponibles a nivel general (GA) para todos los idiomas a los que se da soporte: japonés, español, inglés del Reino Unido e inglés de EE. UU.

### 2 de octubre de 2017
{: #October2017}

-   La interfaz de personalización ofrece ahora personalización del modelo acústico. Puede crear modelos acústicos personalizados que adapten los modelos base del servicio a su entorno y a los oradores. Puede cumplimentar y entrenar un modelo acústico personalizado con el audio que mejor se ajuste a la firma acústica del audio que se desea transcribir. Luego puede utilizar el modelo acústico personalizado con las solicitudes de reconocimiento para aumentar la precisión del reconocimiento de voz.

    Los modelos acústicos personalizados complementan los modelos de lenguaje personalizado. Puede entrenar un modelo acústico personalizado con un modelo de lenguaje personalizado, y puede utilizar ambos tipos de modelos durante el reconocimiento de voz. La personalización del modelo acústico es una interfaz beta que solo está disponible para inglés de EE. UU., español y japonés.

    -   Para obtener más información sobre los idiomas que reciben soporte de la interfaz de personalización y el nivel de soporte que está disponible para cada idioma, consulte [Soporte de idiomas para la personalización](/docs/services/speech-to-text/custom.html#languageSupport).
    -   Para obtener más información acerca de la interfaz de personalización del servicio, consulte [La interfaz de personalización](/docs/services/speech-to-text/custom.html).
    -   Para obtener más información sobre cómo crear un modelo acústico personalizado, consulte [Creación de un modelo acústico personalizado](/docs/services/speech-to-text/acoustic-create.html).
    -   Para obtener más información sobre cómo utilizar un modelo acústico personalizado, consulte [Utilización de un modelo acústico personalizado](/docs/services/speech-to-text/acoustic-use.html).
    -   Para obtener más información sobre todos los métodos de la interfaz de personalización, revise la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Para la personalización del modelo de lenguaje, ahora el servicio incluye una característica beta que establece una ponderación de personalización opcional para un modelo de lenguaje personalizado. Una ponderación de personalización especifica la ponderación relativa que se debe dar a las palabras de un modelo de lenguaje personalizado frente a las palabras del vocabulario base del servicio. Puede establecer una ponderación de personalización durante el entrenamiento y durante el reconocimiento de voz. Para obtener más información, consulte [Utilización de ponderaciones de personalización](/docs/services/speech-to-text/language-use.html#weight).
-   El modelo de lenguaje `ja-JP_BroadbandModel` se ha actualizado para que capture las mejoras del modelo base. La actualización no afecta a los modelos personalizados existentes que se basan en el modelo.
-   Ahora el servicio incluye un parámetro para especificar el valor endianness del audio que se envía en formato `audio/l16` (modulación de código de pulsos de 16 bits (PCM)). Además de especificar los parámetros `rate` y `channels` con el formato, ahora también puede especificar `big-endian` o `little-endian` con el parámetro `endianness`. Para obtener más información, consulte [Formatos de audio](/docs/services/speech-to-text/audio-formats.html).

### 14 de julio de 2017
{: #July2017b}

-   Ahora el servicio da soporte a la transcripción de audio en formato MP3 o Motion Picture Experts Group (MPEG). Para obtener más información acerca de los formatos de audio admitidos, consulte [Formatos de audio](/docs/services/speech-to-text/audio-formats.html).
-   La interfaz de personalización del modelo de lenguaje ahora da soporte al español como funcionalidad beta. Puede crear un modelo personalizado basado en cualquiera de los modelos de lenguaje base en español: `es-ES_BroadbandModel` o `es-ES_NarrowbandModel`; para obtener más información, consulte [Creación de un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-create.html). Los precios de las solicitudes de reconocimiento que utilizan modelos de lenguaje personalizado en español son los mismos que los de las solicitudes que utilizan modelos en inglés de EE. UU. y en japonés.
-   El objeto JSON `CreateLanguageModel` que se pasa al método `POST /v1/customizations` para crear un nuevo modelo de lenguaje personalizado ahora incluye un campo `dialect`. El campo especifica el dialecto del idioma que se va a utilizar con el modelo personalizado. De forma predeterminada, el dialecto coincide con el idioma del modelo base. El parámetro solo tiene sentido para los modelos en español, para los que el servicio crea un modelo personalizado que se adapta al habla de uno de los siguientes dialectos:
    -   `es-ES` para español castellano (valor predeterminado)
    -   `es-LA` para español de Latinoamérica
    -   `es-US` para español de Norteamérica (México)

    Los métodos `GET /v1/customizations` y `GET /v1/customizations/{customization_id}` de la interfaz de personalización incluyen el dialecto de un modelo personalizado en su salida. Para obtener más información, consulte [Creación de un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-create.html#createModel-language) y [Listado de modelos de lenguaje personalizado](/docs/services/speech-to-text/language-models.html#listModels-language).
-   Los nombres de los modelos de lenguaje `en-UK_BroadbandModel` y `en-UK_NarrowbandModel` han quedado en desuso. Ahora los modelos están disponibles con los nombres `en-GB_BroadbandModel` y `en-GB_NarrowbandModel`.

    Los nombres en desuso `en-UK_{model}` siguen funcionando, pero el método `GET /v1/models` ya no devuelve los nombres en la lista de modelos disponibles. Todavía puede consultar los nombres directamente con el método `GET /v1/models/{model_id}`.

### 1 de julio de 2017
{: #July2017a}

-   La interfaz de personalización del modelo de lenguaje del servicio ahora está disponible a nivel general (GA) para los dos idiomas soportados, el inglés de EE. UU. y el japonés. {{site.data.keyword.IBM_notm}} no cobra por crear, alojar o gestionar modelos de lenguaje personalizado. Como se describe en el siguiente punto, {{site.data.keyword.IBM_notm}} ahora cobra 0,03 $ (dólares) adicionales por minuto de audio para las solicitudes de reconocimiento que utilizan modelos personalizados.
-   {{site.data.keyword.IBM_notm}} ha actualizado los precios del servicio
    -   Eliminando el precio adicional por utilizar modelos de banda estrecha
    -   Proporcionando un sistema de precio adicional por niveles para clientes con gran volumen
    -   Facturando 0,03 $ (dólares) adicionales por minuto de audio para las solicitudes de reconocimiento que utilizan los modelos de idioma personalizado en inglés de EE. UU. o en japonés

    Para obtener más información acerca de las actualizaciones de los precios, consulte
    -   La publicación del blog sobre [API de {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} - Actualizaciones de precios ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/blogs/bluemix/2017/05/ibm-watson-speech-text-api-pricing-updates/){: new_window}
    -   El servicio {{site.data.keyword.speechtotextshort}} en el [catálogo de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/catalog/services/speech-to-text){: new_window}
    -   Las [preguntas frecuentes sobre precios](/docs/services/speech-to-text/faq-pricing.html)
-   Ya no tiene que pasar un objeto de datos vacío como cuerpo para las siguientes solicitudes `POST`:
    -   `POST /v1/sessions`
    -   `POST /v1/register_callback`
    -   `POST /v1/customizations/{customization_id}/train`
    -   `POST /v1/customizations/{customization_id}/reset`
    -   `POST /v1/customizations/{customization_id}/upgrade_model`

    Por ejemplo, ahora se llama al método `POST /v1/sessions` con `curl` del siguiente modo:

    ```bash
    curl -X POST -u "{username}:{password}"
    --cookie-jar cookies.txt
    "https://stream.watsonplatform.net/speech-to-text/api/v1/sessions"
    ```
    {: pre}

    Ya no tiene que pasar la siguiente opción `curl` con la solicitud: `--data "{}"`.

    Si tiene algún problema con una de estas solicitudes `POST`, intente pasar un objeto de datos vacío con el cuerpo de la solicitud. El hecho de pasar un objeto vacío no modifica de ningún modo la naturaleza ni el significado de la solicitud.
    {: note}

### 22 de mayo de 2017
{: #May2017}

Las siguientes modificaciones anunciadas en marzo de 2017 han entrado en vigor:

-   El parámetro `continuous` se ha eliminado de todos los métodos que inician solicitudes de reconocimiento. Ahora el servicio transcribe una secuencia de audio completa hasta que termina o hasta que se excede el tiempo de espera, lo que ocurra primero. Este comportamiento es equivalente a establecer el anterior parámetro `continuous` en `true`. De forma predeterminada, anteriormente el servicio detenía la transcripción tras el primer medio segundo sin habla (normalmente silencio) si se omitía el parámetro o se si se establecía en `false`.

    Las aplicaciones existentes que establezcan el parámetro en `true` no verán ningún cambio en el comportamiento. Es probable que las aplicaciones que establezcan el parámetro en `false` o que se basen en el comportamiento predeterminado vean un cambio. Si una solicitud especifica el parámetro, ahora el servicio responde devolviendo un mensaje de aviso para el parámetro desconocido:

    ```javascript
    "warnings": [
      "Unknown arguments: continuous."
    ]
    ```
    {: codeblock}

    La solicitud se ejecuta correctamente a pesar del aviso, y una sesión existente o una conexión WebSocket no se ven afectadas.

    {{site.data.keyword.IBM_notm}} ha eliminado el parámetro en respuesta a los muchos comentarios de la comunidad de desarrolladores según los que especificar `continuous=false` añadía poco valor y podía reducir la precisión global de la transcripción.
-   Ya no se puede evitar un tiempo de espera excedido de sesión sin enviar audio:
    -   Cuando se utiliza la interfaz WebSocket, el cliente ya no puede mantener activa una conexión mediante el envío de un mensaje de texto JSON con el parámetro `action` establecido en `no-op`. El envío de un mensaje `no-op` no genera un error, pero no tiene ningún efecto.
    -   Cuando se utilizan sesiones con la interfaz HTTP, el cliente ya no puede ampliar la sesión mediante el envío de una solicitud `GET /v1/sessions/{session_id}/recognize`. El método sigue devolviendo el estado de una sesión activa, pero no mantiene activa la sesión.
    Ahora puede hacer lo siguiente para mantener activa una sesión:
    -   Establezca el parámetro `inactivity_timeout` en `-1` para evitar el tiempo de espera excedido de inactividad de 30 segundos.
    -   Envíe cualquier dato de audio, incluido silencio, al servicio para evitar el tiempo de espera excedido de la sesión de 30 segundos. Se le factura por la duración de los datos que envía al servicio, incluido el silencio que envíe para ampliar una sesión.

    Para obtener más información, consulte [Tiempos de espera excedidos](/docs/services/speech-to-text/input.html#timeouts). Lo ideal es establecer una sesión justo antes de obtener el audio para la transcripción y mantener la sesión enviando audio a una velocidad cercana al tiempo real. Además, asegúrese de que la aplicación se recupera correctamente de las sesiones o las conexiones cerradas.

    {{site.data.keyword.IBM_notm}} ha eliminado esta funcionalidad para asegurarse de que continúa ofreciendo a todos los usuarios un servicio de reconocimiento de voz óptimo de baja latencia.

### 10 de abril de 2017
{: #April2017}

-   Ahora el servicio da soporte a la característica de etiquetas de orador para los siguientes modelos de banda ancha:
    -   `en-US-BroadbandModel`
    -   `es-ES-BroadbandModel`
    -   `ja-JP_BroadbandModel`

    Para obtener más información, consulte [Etiquetas de orador](/docs/services/speech-to-text/output.html#speaker_labels).
-   Ahora el servicio da soporte al formato de audio Web Media (WebM) con el codec Opus o Vorbis. El servicio ahora también soporta el formato de audio Ogg con el codec Vorbis, además del codec Opus. Para obtener más información acerca de los formatos de audio admitidos, consulte [Formatos de audio](/docs/services/speech-to-text/audio-formats.html).
-   El servicio ahora da soporte a CORS (Cross-Origin Resource Sharing) para permitir que los clientes basados en navegador llamen al servicio directamente. Para obtener más información, consulte [Soporte de CORS](/docs/services/speech-to-text/developer-overview.html#cors).
-   Ahora la interfaz HTTP asíncrona ofrece un método `POST /v1/unregister_callback` que elimina el registro de un URL de devolución de llamada de la lista blanca. Para obtener más información, consulte [Anulación del registro de un URL de devolución de llamada](/docs/services/speech-to-text/async.html#unregister).
-   La interfaz WebSocket ya no supera el tiempo de espera para las solicitudes de reconocimiento de archivos de audio especialmente largos. Ya no es necesario solicitar resultados provisionales con el mensaje `start` de JSON para evitar el tiempo de espera excedido. (Este problema fue descrito en las notas del release del 10 de marzo de 2016.)
-   El método `DELETE /v1/customizations/{customization_id}` devuelve ahora el código de respuesta HTTP 401 si se intenta suprimir un modelo personalizado que no existe.
-   Ahora el método `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` devuelve el código de respuesta HTTP 400 si se intenta suprimir un corpus que no existe.

### 8 de marzo de 2017
{: #March2017}

La interfaz HTTP asíncrona ahora está disponible a nivel general. Antes de esta fecha, era una funcionalidad beta.

### 1 de diciembre de 2016
{: #December2016}

-   Ahora el servicio ahora ofrece una característica de etiquetas de orador beta para audio de banda estrecha en inglés de EE. UU., español o japonés. La característica identifica las palabras que pronuncia cada orador en un intercambio con varias personas. Cada uno de los métodos de reconocimiento sin sesión, basado en sesión, asíncrono y WebSocket incluye un parámetro `speaker_labels` que acepta un valor booleano que indica si se deben incluir etiquetas de orador en la respuesta. Para obtener más información acerca de la característica, consulte [Etiquetas de orador](/docs/services/speech-to-text/output.html#speaker_labels).
-   La interfaz de personalización del modelo de lenguaje beta ahora recibe soporte para el japonés además del inglés de EE. UU. Todos los métodos de la interfaz dan soporte al japonés. Para obtener más información, consulte las secciones siguientes:
    -   Para obtener más información, consulte [Creación de un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-create.html) y [Utilización de un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-use.html).
    -   Para ver consideraciones generales y específicas del japonés sobre la adición de un archivo de texto de corpus, consulte [Preparación de un archivo de corpus](/docs/services/speech-to-text/language-resource.html#prepareCorpus) y [¿Qué ocurre cuando se añade un archivo de corpus?](/docs/services/speech-to-text/language-resource.html#parseCorpus)
    -   Para ver consideraciones específicas del japonés cuando se especifica el campo `sounds_like` para una palabra personalizada, consulte [Directrices para el japonés](/docs/services/speech-to-text/language-resource.html#wordLanguages-jaJP).
    -   Para obtener más información sobre todos los métodos de la interfaz de personalización, revise la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   La interfaz de personalización del modelo de lenguaje incluye ahora un método `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` que muestra una lista con información acerca de un corpus especificado. El método resulta útil para supervisar el estado de una solicitud para añadir un corpus a un modelo personalizado. Para obtener más información, consulte [Listado de los corpus de un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-corpora.html#listCorpora).
-   La salida JSON que devuelven los métodos `GET /v1/customizations/{customization_id}/words` y `GET /v1/customizations/{customization_id}/words/{word_name}` ahora incluye un campo `count` para cada palabra. El campo indica el número de veces que se encuentra la palabra en todos los corpus. Si añade una palabra personalizada a un modelo antes de la añada un corpus, el recuento empieza en `1`. Si la palabra se añade primero desde un corpus y luego se modifica, el recuento solo refleja el número de veces que se encuentra en los corpus. Para obtener más información, consulte [Listado de las palabras de un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-words.html#listWords).

    Para los modelos personalizados creados antes de que existiera el campo `count`, el campo siempre permanece en `0`. Para actualizar el campo para dichos modelos, añada de nuevo el corpus del modelo e incluya el parámetro `allow_overwrite` con el método `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`.
-   El método `GET /v1/customizations/{customization_id}/words` incluye ahora un parámetro de consulta `sort` que controla el orden en el que se muestran las palabras en una lista. El parámetro acepta dos argumentos, `alphabetical` o `count`, que indican cómo se deben clasificar las palabras. Puede añadir un `+` o un `-` opcional antes de un argumento para indicar si los resultados se van a clasificar en orden ascendente o descendente. De forma predeterminada, el método muestra las palabras en orden alfabético ascendente. Para obtener más información, consulte [Listado de las palabras de un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-words.html#listWords).

    Para los modelos personalizados creados antes de la introducción del campo `count`, el uso del argumento `count` con el parámetro `sort` no tiene sentido. Utilice el argumento `alphabetical` predeterminado con dichos modelos.
-   El campo `error` que se puede devolver como parte de una respuesta JSON de los métodos `GET /v1/customizations/{customization_id}/words` y `GET /v1/customizations/{customization_id}/words/{word_name}` ahora es una matriz. Si el servicio ha descubierto uno o varios problemas con la definición de una palabra personalizada, el campo lista cada elemento de problema de la definición y proporciona un mensaje que describe el problema. Para obtener más información, consulte [Listado de las palabras de un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-words.html#listWords).
-   Los parámetros `keywords_threshold` y `word_alternatives_threshold` de los métodos de reconocimiento ya no aceptan un valor nulo. Para omitir alternativas de palabras clave y de palabras en la respuesta, omita los parámetros. El valor especificado debe ser un valor flotante.

Para obtener más información sobre la interfaz del servicio, revise la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 22 de septiembre de 2016
{: #September2016}

-   Ahora el servicio ofrece una nueva interfaz de personalización de modelo de lenguaje beta para inglés de Estados Unidos. Puede utilizar la interfaz para adaptar el vocabulario base y los modelos de lenguaje del servicio mediante la creación de modelos de lenguaje personalizado que incluyan terminología específica del dominio. Puede añadir palabras personalizadas individualmente o hacer que el servicio las extraiga del corpus. Para utilizar sus modelos personalizados con los métodos de reconocimiento de voz que ofrece cualquiera de las interfaces del servicio, pase el parámetro de consulta `customization_id`. Para obtener más información,
consulte
    -   [Creación de un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-create.html)
    -   [Utilización de un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-use.html)
    -   [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}
-   La lista de formatos de audio admitidos ahora incluye `audio/mulaw`, que proporciona audio de un solo canal codificado mediante el algoritmo de datos u-law (o mu-law). Cuando utilice este formato, también debe especificar la frecuencia de muestreo a la que se captura el audio. Para obtener más información, consulte [Formatos de audio](/docs/services/speech-to-text/audio-formats.html).
-   Los métodos `GET /v1/models` y `GET /v1/models/{model_id}` ahora devuelven un campo `supported_features` como parte de su salida para cada modelo de lenguaje. La información adicional describe si el modelo da soporte a la personalización. Para obtener más información, revise la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 30 de junio de 2016
{: #June2016b}

La interfaz HTTP asíncrona de la versión beta ahora da soporte a todos los idiomas soportados por el servicio. Anteriormente la interfaz solo estaba disponible para inglés de Estados Unidos. Para obtener más información, consulte [La interfaz HTTP asíncrona](/docs/services/speech-to-text/async.html) y la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 23 de junio de 2016
{: #June2016a}

-   Ahora hay una interfaz HTTP asíncrona disponible. La interfaz proporciona funciones completas de reconocimiento para la transcripción en inglés de EE. UU. a través de llamadas HTTP de no bloqueo. Puede registrar URL de devolución de llamada y proporcionar series de secretos especificados por el usuario para conseguir la autenticación y la integridad de los datos con firmas digitales. Para obtener más información, consulte [La interfaz HTTP asíncrona](/docs/services/speech-to-text/async.html) y la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Una característica de formateo inteligente beta que convierte fechas, horas, series de dígitos y números, números de teléfono, valores de moneda y direcciones de Internet en representaciones más convencionales en las transcripciones finales. Para habilitar esta característica, establezca el parámetro `smart_formatting` en `true` en una solicitud de reconocimiento. La característica es una funcionalidad beta que solo está disponible en inglés de EE. UU. Para obtener más información, consulte [Formateo inteligente](/docs/services/speech-to-text/output.html#smart_formatting).
-   La lista de modelos que reciben soporte para el reconocimiento de voz ahora incluye `fr-FR_BroadbandModel` para el audio en el idioma francés que se muestrea a un mínimo de 16 kHz. Para obtener más información, consulte [Idiomas y modelos](/docs/services/speech-to-text/models.html).
-   La lista de formatos de audio admitidos ahora incluye `audio/basic`. El formato proporciona audio de un solo canal que se codifica mediante datos de 8 bits u-law (o mu-law) que se muestrean a 8 kHz. Para obtener más información, consulte [Formatos de audio](/docs/services/speech-to-text/audio-formats.html).
-   Los diversos métodos de reconocimiento pueden devolver una respuesta `warnings` que incluye mensajes sobre parámetros de consulta o campos JSON no válidos incluidos en una solicitud. El formato de los avisos ha cambiado. Por ejemplo: `"warnings": "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']."` ahora es `"warnings": "Unknown arguments: {invalid_arg_1}, {invalid_arg_2}."`
-   Para las solicitudes HTTP `POST` que de lo contrario no pasan datos al servicio, debe incluir un cuerpo de solicitud vacío con el formato `{}`. Con el mandato `curl`, puede utilizar la opción `--data` para pasar datos vacíos.

### 10 de marzo de 2016
{: #March2016}

-   Ambas formas de transmisión de datos (entrega única o en secuencia) ahora imponen un límite de 100 MB en datos de audio, al igual que la interfaz WebSocket. Anteriormente, el enfoque de entrega única tenía un límite máximo de 4 MB de datos. Para obtener más información, consulte [Transmisión de audio](/docs/services/speech-to-text/input.html#transmission) (para todas las interfaces) y [Envío de audio y recepción de resultados de reconocimiento](/docs/services/speech-to-text/websockets.html#WSaudio) (para la interfaz WebSocket). En la sección sobre WebSocket también se trata la trama máxima o el tamaño de mensaje de 4 MB impuesto por la interfaz WebSocket.
-   La respuesta JSON a una solicitud de reconocimiento ahora puede incluir una matriz de mensajes de aviso correspondientes a parámetros de consulta o campos JSON no válidos incluidos con una solicitud. Cada elemento de la matriz es una serie que describe la naturaleza del aviso seguida de una matriz de series de argumentos no válidos. Por ejemplo, `"warnings": [ "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']." ]`. Para obtener más información, revise la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   El kit de desarrollo de software (SDK) de *{{site.data.keyword.watson}} Speech beta para el sistema operativo Apple&reg;* ha quedado en desuso. Utilice en su lugar el SDK de *{{site.data.keyword.watson}} para el sistema operativo Apple&reg; iOS*. El nuevo SDK está disponible en el [repositorio ios-sdk ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/watson-developer-cloud/ios-sdk){: new_window} en el espacio de nombres `watson-developer-cloud` en GitHub.

La interfaz WebSocket actualmente presenta el siguiente problema conocido:

-   El servicio puede tardar minutos en generar resultados finales para una solicitud de reconocimiento para un archivo de audio especialmente largo. En el caso de la interfaz WebSocket, la conexión TCP subyacente permanece inactiva mientras el servicio prepara la respuesta. Por lo tanto, la conexión se puede cerrar debido a un tiempo de espera excedido. Para evitar el tiempo de espera excedido con la interfaz WebSocket, solicite resultados provisionales (`\"interim_results\": \"true\"`) en JSON para que el mensaje `start` inicie la solicitud. Puede descartar los resultados provisionales si no los necesita. Este problema se resolverá en una futura actualización.

### 19 de enero de 2016
{: #January2016}

El servicio se ha actualizado para incluir una nueva función de filtrado de lenguaje obsceno el 19 de enero de 2016. De forma predeterminada, el servicio censura el lenguaje obsceno de sus resultados de transcripción para el audio en inglés de Estados Unidos. Para obtener más información, consulte [Filtrado de lenguaje obsceno](/docs/services/speech-to-text/output.html#profanity_filter).

### 17 de diciembre de 2015
{: #December2015}

-   Ahora el servicio ofrece una característica de detección de palabras clave. Puede especificar una matriz de series de palabras clave que se van a comparar con el audio de entrada. También debe especificar un nivel de confianza definido por el usuario que debe tener una palabra para que se considere que coincide con una palabra clave. Para obtener más información, consulte [Detección de palabras clave](/docs/services/speech-to-text/output.html#keyword_spotting).

    La característica de detección de palabras clave es una funcionalidad beta.
    {: note}
-   Ahora el servicio ofrece una característica de alternativas a palabras. La característica devuelve hipótesis alternativas para palabras del audio de entrada que cumplen un nivel de confianza definido por el usuario. Para obtener más información, consulte [Alternativas a palabras](/docs/services/speech-to-text/output.html#word_alternatives).

    La característica de alternativas a palabras es una funcionalidad beta.
    {: note}
-   El servicio da soporte a más idiomas con sus modelos de transcripción: `en-UK_BroadbandModel` y `en-UK_NarrowbandModel` para inglés del Reino Unido y `ar-AR_BroadbandModel` para árabe estándar moderno. Para obtener más información, consulte [Idiomas y modelos](/docs/services/speech-to-text/models.html).
-   Las solicitudes de reconocimiento HTTP ya no están sujetas a un tiempo de espera de la plataforma de 10 minutos. Ahora el servicio mantiene la conexión activa enviando un carácter de espacio en el objeto JSON de respuesta cada 20 segundos, siempre que el reconocimiento esté en curso. Para obtener más información, consulte [Tiempos de espera excedidos](/docs/services/speech-to-text/input.html#timeouts).
-   El servicio ya no devuelve el código de estado de HTTP 490 para los métodos HTTP basados en sesión `GET /v1/sessions/{session_id}/observe_result` y `POST /v1/sessions/{session_id}/recognize`. El servicio ahora responde con el código de estado de HTTP 400 en su lugar.

    En las respuestas JSON que devuelve para los errores con métodos basados en sesión, el servicio ahora también incluye un nuevo campo `session_closed`. El campo se establece en `true` si la sesión se cierra como resultado del error. Para obtener más información sobre los posibles códigos de retorno para cualquiera de los métodos, revise la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Cuando se utiliza el mandato `curl` para transcribir audio con el servicio, ya no es necesario utilizar la opción `--limit-rate` para transferir datos a una velocidad no superior a 40.000 bytes por segundo.

### 21 de septiembre de 2015
{: #September2015}

-   Hay dos nuevos SDK móviles beta disponibles para los servicios de voz. Los SDK permiten a las aplicaciones móviles interactuar tanto con el servicio {{site.data.keyword.speechtotextshort}} como con el servicio {{site.data.keyword.texttospeechshort}}.
    -   El SDK de *{{site.data.keyword.watson}} Speech para la plataforma Google Android&trade;* da soporte a secuencias de audio enviadas al servicio {{site.data.keyword.speechtotextshort}} en tiempo real y a la recepción de una transcripción de audio a medida que se habla. El proyecto incluye una aplicación de ejemplo que muestra la interacción con ambos servicios de voz. El SDK está disponible en el [repositorio speech-android-sdk ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/watson-developer-cloud/speech-android-sdk){: new_window} en el espacio de nombres `watson-developer-cloud` en GitHub.
    -   El SDK de *{{site.data.keyword.watson}} Speech para el sistema operativo Apple&reg; iOS* da soporte a secuencias de audio enviadas al servicio {{site.data.keyword.speechtotextshort}} y a la recepción de una transcripción del audio en la respuesta. El SDK está disponible en el [repositorio speech-ios-sdk ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/watson-developer-cloud/speech-ios-sdk){: new_window} en el espacio de nombres `watson-developer-cloud` en GitHub.

    Ambos SDK dan soporte a la autenticación con los servicios de voz mediante las credenciales del servicio {{site.data.keyword.cloud_notm}} o mediante señal de autenticación.

    Debido a que los SDK son beta, están sujetos a cambios en el futuro.
    {: note}
-   El servicio da soporte a dos nuevos idiomas, portugués de Brasil y chino mandarín. Los modelos correspondientes a estos idiomas son `pt-BR_BroadbandModel`, `pt-BR_NarrowbandModel`, `zh-CN_BroadbandModel` y `zh-CN_NarrowbandModel`. Para obtener más información, consulte [Idiomas y modelos](/docs/services/speech-to-text/models.html).
-   Las solicitudes HTTP `POST` `/v1/sessions/{session_id}/recognize` y `/v1/recognize`, al igual que la solicitud WebSocket `/v1/recognize`, dan soporte a la transcripción de un nuevo tipo de soporte: `audio/ogg;codecs=opus` para archivos en formato Ogg que utilizan el codec Opus. Además, el formato `audio/wav` para los métodos ahora da soporte a cualquier codificación. La restricción sobre el uso de codificación PCM lineal se ha eliminado. Para obtener más información, consulte [Formatos de audio](/docs/services/speech-to-text/audio-formats.html).
-   Ahora el servicio da soporte a la eliminación de tiempos de espera excedidos cuando se transcriben archivos de audio largos con la interfaz HTTP. Cuando utilice sesiones, puede utilizar un patrón de sondeo largo especificando los ID de secuencia con los métodos `GET /v1/sessions/{session_id}/observe_result` y `POST /v1/sessions/{session_id}/recognize` para tareas de reconocimiento de larga ejecución. Mediante el nuevo parámetro `sequence_id` de estos métodos, puede solicitar resultados antes, durante o después de enviar una solicitud de reconocimiento.
-   Para los modelos de lenguaje en inglés de Estados Unidos, `en_US_BroadbandModel` y `en_US_NarrowbandModel`, el servicio ahora coloca correctamente mayúsculas en muchos nombres propios. Por ejemplo, ahora el servicio devolvería el texto "Barack Obama graduated from Columbia University" en lugar de "barack obama graduated from columbia university". Este cambio puede resultar especialmente interesante si es importante para la aplicación la colocación de mayúsculas en los nombres propios.
-   La solicitud HTTP `DELETE /v1/sessions/{session_id}` no devuelve el código de estado 415 "Unsupported Media Type". Este código de retorno se ha eliminado de la documentación del método.

### 1 de julio de 2015
{: #July2015}

El servicio ha pasado de beta a disponibilidad general (GA) el 1 de julio de 2015. Existen las siguientes diferencias entre las versiones beta y GA de las API de {{site.data.keyword.speechtotextshort}}. El release GA requiere que los usuarios actualicen a la nueva versión del servicio.

-   La versión GA de la API HTTP es compatible con la versión beta. Solo tiene que cambiar el código de aplicación existente si ha especificado de forma explícita un nombre de modelo. Por ejemplo, el código de ejemplo disponible para el servicio de GitHub incluía la siguiente línea de código en el archivo `demo.js`:

    ```javascript
    model: 'WatsonModel'
    ```
    {: codeblock}

    Esta línea especificada el modelo predeterminado, `WatsonModel`, para la versión beta del servicio. Si la aplicación también especificaba este modelo, tiene que cambiarlo para que utilice uno de los nuevos modelos que reciben soporte de la versión GA. Para obtener más información, consulte el siguiente punto.
-   Ahora el servicio da soporte a un nuevo modelo de programación para la interacción directa entre un cliente y el servicio a través de una conexión WebSocket. Mediante el uso de este modelo, un cliente puede obtener una señal de autenticación para comunicarse directamente con el servicio. La señal elimina la necesidad de que una aplicación de proxy del lado del servidor de {{site.data.keyword.cloud_notm}} llame al servicio en nombre del cliente. Las señales constituyen el método recomendado para que los clientes interactúen con el servicio.

    El servicio sigue dando soporte al modelo de programación antiguo que se basaba en un proxy del lado del servidor para retransmitir audio y mensajes entre el cliente y el servicio. Pero el nuevo modelo es más eficiente y ofrece un mejor rendimiento. Para obtener más información sobre el nuevo modelo de programación, consulte [Modelos de programación para servicios {{site.data.keyword.watson}}](/docs/services/watson/getting-started-develop.html).
-   Los métodos `POST /v1/sessions` y `POST /v1/recognize`, junto con el método WebSocket `/v1/recognize`, ahora dan soporte a un parámetro de consulta `model`. El parámetro sirve para especificar información sobre el audio:

    -   El idioma: inglés (*English*), japonés (*Japanese*) o español (*Spanish*)
    -   La frecuencia mínima de muestreo: banda ancha (*bradband*, 16 kHz) o banda estrecha (*narrowband*, 8 kHz)

    Para obtener más información, consulte [Idiomas y modelos](/docs/services/speech-to-text/models.html).
-   La cabecera `Content-Type` de los métodos `recognize` ahora da soporte a los archivos `audio/wav` para Waveform Audio File Format (WAV), además de `audio/flac` y `audio/l16`. Para obtener más información acerca de los formatos de audio admitidos, consulte [Formatos de audio](/docs/services/speech-to-text/audio-formats.html).
-   Los métodos `recognize` ahora dan soporte a varios nuevos parámetros de consulta que puede utilizar para adaptar el servicio para que se ajuste a las necesidades de la aplicación:
    -   El parámetro `inactivity_timeout` establece el valor de tiempo de espera en segundos transcurrido el cual el servicio cierra la conexión si detecta silencio (ausencia de voz) en modalidad continua. De forma predeterminada, el servicio finaliza la sesión después de 30 segundos de silencio. Consulte [Tiempos de espera excedidos](/docs/services/speech-to-text/input.html#timeouts).
    -   El parámetro `max_alternatives` indica al servicio que devuelva las *n* mejores hipótesis alternativas para la transcripción de audio. Consulte [Número máximo de alternativas](/docs/services/speech-to-text/output.html#max_alternatives).
    -   El parámetro `word_confidence` indica al servicio que devuelva una puntuación de confianza para cada palabra de la transcripción. Consulte [Confianza de palabras](/docs/services/speech-to-text/output.html#word_confidence).
    -   El parámetro `timestamps` indica al servicio que devuelva la hora inicial y final con relación al inicio del audio para cada palabra de la transcripción. Consulte [Indicaciones de fecha y hora de palabras](/docs/services/speech-to-text/output.html#word_timestamps).
-   El método `GET /v1/sessions/{session_id}/observeResult` ahora se denomina `GET /v1/sessions/{session_id}/observe_result`. El nombre `observeResult` sigue recibiendo soporte por motivos de compatibilidad con versiones anteriores.
-   Los métodos `GET /v1/sessions/{session_id}/observe_result`, `POST /v1/sessions/{session_id}/recognize` y `POST /v1/recognize` ahora incluyen el parámetro de cabecera `X-WDC-PL-OPT-OUT` para controlar si el servicio utiliza los datos de audio y de transcripción de una solicitud para mejorar resultados futuros. La interfaz WebSocket incluye un parámetro de consulta equivalente. Especifique el valor `1` para impedir que el servicio utilice resultados de audio y de transcripción. El parámetro solo se aplica a la solicitud actual. La nueva cabecera sustituye la cabecera `X-logging` de la API beta. Consulte [Control del registro de solicitudes para servicios {{site.data.keyword.watson}}](../common/getting-started-logging.html).
-   Ahora el servicio tiene un límite de 100 MB de datos por sesión en modalidad continua. La modalidad continua se especifica con el valor `chunked` con la cabecera `Transfer-Encoding`. Una entrega única de un archivo de audio sigue imponiendo un límite de tamaño de 4 MB en los datos que se envían. Consulte [Transmisión de audio](/docs/services/speech-to-text/input.html#transmission).
-   Para los métodos `/v1/models`, `/v1/models/{model_id}`, `/v1/sessions`, `/v1/sessions/{session_id}`, `/v1/sessions/{session_id}/observe_result`, `/v1/sessions/{session_id}/recognize` y `/v1/recognize`, se ha añadido el código de error 415 ("Unsupported Media Type").
-   Para solicitudes `POST` y `GET` destinadas al método `/v1/sessions/{session_id}/recognize`, se han modificado los siguientes códigos de error:
    -   El código de error 404 ("Session_id not found") tiene un mensaje más descriptivo (`POST` y `GET`).
    -   El código de error 503 ("Session is already processing a request. Concurrent requests are not allowed on the same session. Session remains alive after this error.") tiene un mensaje más descriptivo (solo `POST`).
-   Para solicitudes HTTP `POST` destinadas a los métodos `/v1/sessions` y `/v1/recognize`, se puede devolver el código de error 503 ("Service Unavailable"). También se puede devolver el código de error cuando se crea una conexión WebSocket con el método `/v1/recognize`.
