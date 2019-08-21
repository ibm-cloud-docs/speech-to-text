---

copyright:
  years: 2019
lastupdated: "2019-06-24"

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

# Utilización conjunta del modelo acústico personalizado y del modelo de lenguaje personalizado
{: #useBoth}

Puede mejorar la precisión del reconocimiento de voz utilizando modelos complementarios de lenguaje personalizado y acústico personalizado. Puede utilizar ambos tipos de modelo durante el entrenamiento del modelo acústico y durante el reconocimiento de voz. Ambos modelos deben ser propiedad de la misma instancia de servicio, y ambos deben basarse en el mismo modelo de lenguaje base.
{: shortdesc}

El uso de un modelo acústico personalizado por sí solo o con un modelo de lenguaje personalizado con el que no se ha entrenado también puede resultar útil. Si el modelo acústico personalizado se ha entrenado con características acústicas que coincidan con el audio que se está transcribiendo, también puede mejorar la calidad de la transcripción.

## Entrenamiento de un modelo acústico personalizado con un modelo de lenguaje personalizado
{: #useBothTrain}

El entrenamiento de un modelo acústico personalizado solo con datos de audio también se denomina *entrenamiento no supervisado*. El uso de un modelo de lenguaje personalizado durante el entrenamiento recibe el nombre de *entrenamiento ligeramente supervisado*. El entrenamiento ligeramente supervisado puede mejorar la efectividad del modelo acústico personalizado.

Utilice el entrenamiento ligeramente supervisado para entrenar un modelo acústico personalizado con un modelo de lenguaje personalizado en los casos siguientes:

-   El modelo de lenguaje personalizado se basa en transcripciones (texto literal) procedentes de archivos de audio que ha añadido al modelo acústico personalizado.

    Como las transcripciones contienen el contenido exacto del audio, el entrenamiento con un modelo de lenguaje personalizado basado en transcripciones puede producir los mejores resultados. El servicio puede analizar el contenido de las transcripciones en el contexto y extraer palabras OOV (no definidas en el vocabulario) y n-gramas que le pueden ayudar a utilizar el audio de la manera más eficiente. Esto es especialmente cierto si los datos de audio duran menos de una hora.

    No es estrictamente necesario transcribir los datos de audio. Pero, si tiene transcripciones del audio, pueden mejorar la calidad del modelo acústico personalizado. Las transcripciones resultan especialmente valiosas si el audio contiene muchas palabras OOV.
-   El modelo de lenguaje personalizado se basa en un corpus (archivos de texto) o en una lista de palabras que son relevantes para el contenido de los archivos de audio.

    Si el audio contiene palabras específicas de un dominio que no se encuentran en el vocabulario base del servicio, la personalización del modelo acústico por sí sola no genera esas palabras durante la transcripción. La personalización del modelo de lenguaje es la única forma de ampliar el vocabulario base del servicio. Si no tiene transcripciones, entrene con un modelo de lenguaje personalizado que incluya palabras OOV del mismo dominio que los datos de audio. Incluso el entrenamiento con un modelo de lenguaje personalizado que incluya una lista de las palabras OOV que se utilizan en el audio puede resultar útil.

    Por ejemplo, suponga que está creando un modelo acústico personalizado que se basa en el audio del centro de servicio al cliente de un producto específico. Puede entrenar el modelo acústico personalizado con un modelo de lenguaje personalizado que se base en transcripciones de llamadas relacionadas o que incluya nombres de productos específicos que gestiona el centro de atención al cliente.

Para utilizar una transcripción o una lista de palabras, primero debe crear un modelo de lenguaje personalizado que incluya estos datos textuales. Para entrenar un modelo acústico personalizado con un modelo de lenguaje personalizado, ambos modelos personalizados deben estar basados en la misma versión del mismo modelo base. Si se dispone de una nueva versión del modelo base, debe actualizar ambos modelos a la misma versión del modelo base para que el entrenamiento tenga éxito.

Utilice el parámetro de consulta opcional `custom_language_model_id` del método `POST /v1/acoustic_customizations/{customization_id}/train` para entrenar el modelo acústico personalizado con un modelo de lenguaje personalizado. Pase el GUID del modelo acústico con el parámetro `customization_id` y el GUID del modelo de lenguaje personalizado con el parámetro `custom_language_model_id`. Ambos modelos deben ser propiedad de las credenciales que se pasan con la solicitud.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train?custom_language_model_id={customization_id}"
```
{: pre}

## Utilización de modelos de lenguaje personalizado y acústico personalizado durante el reconocimiento de voz
{: #useBothRecognize}

Puede especificar tanto un modelo de lenguaje personalizado como un modelo acústico personalizado con cualquier solicitud de reconocimiento. Si un modelo de lenguaje personalizado contiene palabras OOV del dominio del audio que se está reconociendo, puede utilizarlo con un modelo acústico personalizado durante el reconocimiento de voz para mejorar la precisión de la transcripción.

La utilización de un modelo de lenguaje personalizado puede mejorar la precisión de la transcripción, independientemente de si ha entrenado el modelo acústico personalizado con el modelo de lenguaje personalizado:

-   La utilización de modelos de lenguaje personalizado y acústico personalizado durante el entrenamiento mejora la calidad del modelo acústico personalizado.
-   La utilización de ambos tipos de modelo durante el reconocimiento de voz mejora la calidad de la transcripción.

Si un modelo de lenguaje personalizado incluye gramática, también puede utilizar el modelo de lenguaje personalizado y una de sus gramáticas con un modelo acústico personalizado durante el reconocimiento de voz.

En el ejemplo siguiente se pasan los dos tipos de modelo al método HTTP `POST /v1/recognize`. El GUID del modelo acústico personalizado se pasa con el parámetro `acoustic_customization_id` y el GUID del modelo de lenguaje personalizado se pasa con el parámetro `language_customization_id`. Ambos modelos deben ser propiedad de las credenciales que se pasan con la solicitud, y ambos deben basarse en el mismo modelo base (por ejemplo, `en-US_BroadbandModel`).

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?acoustic_customization_id={customization_id}&language_customization_id={customization_id}"
```
{: pre}

En el caso de una solicitud HTTP asíncrona, especifique los parámetros al crear el trabajo asíncrono. En el caso de WebSockets, pase los parámetros cuando establezca la conexión.
