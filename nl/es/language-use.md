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

# Utilización de un modelo de lenguaje personalizado
{: #languageUse}

Una vez que haya creado y entrenado su modelo de lenguaje personalizado, puede utilizarlo en solicitudes de reconocimiento de voz. Utilice el parámetro de consulta `language_customization_id` para especificar el modelo de lenguaje personalizado para una solicitud, tal como se muestra en los ejemplos siguientes. También puede indicar al servicio la ponderación que debe asignar a las palabras del modelo personalizado. Para obtener más información, consulte [Utilización de ponderaciones de personalización](#weight). Debe enviar la solicitud con las credenciales de servicio correspondientes a la instancia del servicio propietaria del modelo.
{: shortdesc}

Puede crear varios modelos de lenguaje personalizado para los mismos dominios o para dominios diferentes. Sin embargo, solo puede especificar un modelo de lenguaje personalizado a la vez con el parámetro `language_customization_id`. Para ver ejemplos que utilizan una gramática con un modelo de lenguaje personalizado, consulte [Utilización de una gramática para el reconocimiento de voz](/docs/services/speech-to-text/grammar-use.html).

-   En el caso de la [interfaz WebSocket](/docs/services/speech-to-text/websockets.html), utilice el método `/v1/recognize`. El modelo personalizado especificado se utiliza para todas las solicitudes que se envían a través de la conexión.

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=es-ES_BroadbandModel'
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   En el caso de la [interfaz HTTP síncrona](/docs/services/speech-to-text/http.html), utilice el método `POST /v1/recognize`. El modelo personalizado especificado se utiliza para dicha solicitud.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}"
    ```
    {: pre}
-   En el caso de la [interfaz HTTP asíncrona](/docs/services/speech-to-text/async.html), utilice el método `POST /v1/recognitions`. El modelo personalizado especificado se utiliza para dicha solicitud.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?language_customization_id={customization_id}"
    ```
    {: pre}

Puede omitir el modelo de lenguaje de la solicitud si el modelo personalizado se basa en el modelo de lenguaje predeterminado, `en-US_BroadbandModel`. De lo contrario, debe utilizar el parámetro `model` para especificar el modelo base, tal como se muestra para el ejemplo de WebSocket. Un modelo personalizado solo se puede utilizar con el modelo base para el que se ha creado.

## Utilización de ponderaciones de personalización
{: #weight}

Un modelo de lenguaje personalizado es una combinación del modelo personalizado y del modelo base que personaliza. Puede indicar al servicio la ponderación que debe asignar a las palabras desde el modelo de lenguaje personalizado en comparación con las palabras del modelo base para el reconocimiento de voz. La ponderación que se asigna a un modelo personalizado se denomina *ponderación de personalización*.

Se especifica la ponderación relativa de un modelo de lenguaje personalizado como un doble comprendido entre 0,0 y 1,0. De forma predeterminada, cada modelo de lenguaje personalizado tiene una ponderación de 0,3. La ponderación predeterminada genera el mejor rendimiento en el caso general. Permite que se reconozcan tanto las palabras OOV del modelo personalizado como las palabras del vocabulario base.

No obstante, en los casos en los que en el audio que se va a transcribir se utilizan muchas palabras OOV del modelo personalizado, aumentar la ponderación de personalización puede mejorar la precisión de los resultados de la transcripción. Tenga cuidado cuando establezca la ponderación de personalización. Si bien una ponderación más alta puede mejorar la precisión de las frases del dominio del modelo personalizado, también puede afectar negativamente al rendimiento en frases que no son del dominio. (Aunque establezca la ponderación 0,0, existe una pequeña probabilidad de que la transcripción incluya palabras personalizadas debido a la implementación de la personalización del modelo de lenguaje.)

La ponderación de personalización se especifica mediante el parámetro `customization_weight`. Puede especificar el parámetro al entrenar un modelo de lenguaje personalizado o al utilizarlo con una solicitud de reconocimiento de voz.

-   En el caso de una solicitud de entrenamiento, el ejemplo siguiente especifica una ponderación de personalización de `0,5` con el método `POST /v1/customizations/{customization_id}/train`:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train?customization_weight=0.5"
    ```
    {: pre}

    Si se establece una ponderación de personalización durante el entrenamiento, la ponderación se guarda con el modelo de lenguaje personalizado. No es necesario que pase la ponderación con cada solicitud de reconocimiento que utilice el modelo personalizado.

-   En el caso de una solicitud de reconocimiento, el ejemplo siguiente especifica una ponderación de personalización de `0,7` con el método `POST /v1/recognize`:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.7"
    ```
    {: pre}

    Una ponderación de personalización especificada durante el reconocimiento de voz sobrescribe una ponderación guardada con el modelo durante el entrenamiento.

## Resolución de problemas derivados del uso de modelos de lenguaje personalizado
{: #languageTroubleshoot}

Si aplica un modelo de lenguaje personalizado al reconocimiento de voz pero parece que el servicio no utiliza las palabras que contiene el modelo, compruebe los siguientes posibles problemas:

-   Asegúrese de que está pasando correctamente el ID de personalización a la solicitud de reconocimiento, tal como se muestra en el apartado [Utilización de un modelo de lenguaje personalizado](#languageUse).
-   Asegúrese de que el estado del modelo personalizado sea `available`, lo que significa que está completamente entrenado y listo para ser utilizado. Para obtener más información, consulte [Listado de modelos de lenguaje personalizado](/docs/services/speech-to-text/language-models.html#listModels-language).
-   Compruebe las pronunciaciones generadas para las palabras nuevas para asegurarse de que sean correctas. Para obtener más información, consulte [Validación de un recurso de palabras](/docs/services/speech-to-text/language-resource.html#validateModel).
