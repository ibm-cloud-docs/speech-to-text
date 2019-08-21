---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-04"

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

# Actualización de modelos personalizados
{: #customUpgrade}

Para mejorar la calidad del reconocimiento de voz, el servicio {{site.data.keyword.speechtotextfull}} actualiza de vez en cuando los modelos base. Puesto que los modelos base correspondiente a los diferentes idiomas son independientes entre sí, al igual que los modelos de banda ancha y de banda estrecha correspondientes a un idioma, las actualizaciones de modelos base individuales no afectan a otros modelos. En las [Notas del release](/docs/services/speech-to-text?topic=speech-to-text-release-notes) se documentan todas las actualizaciones del modelo base.
{: shortdesc}

Cuando se publica una nueva versión de un modelo base, debe actualizar los modelos de lenguaje personalizado y acústico personalizado que se hayan creado en el modelo base para aprovechar las actualizaciones. Los modelos personalizados siguen utilizando la versión anterior del modelo base hasta que se completa la actualización. Al igual que sucede con todas las operaciones de personalización, debe utilizar las credenciales correspondientes a la instancia del servicio que es propietario de un modelo para poderlo actualizar.

Actualice a la versión más reciente de un modelo base actualizado lo antes posible. Cuanto antes se actualice un modelo personalizado, antes podrá disfrutar del rendimiento mejorado del nuevo modelo. Además, las versiones antiguas de los modelos base se pueden eliminar en un futuro. Para incentivar la actualización, el servicio devuelve un mensaje de aviso con los resultados de las solicitudes de reconocimiento que utilizan modelos personalizados que se basan en modelos base antiguos.

## Cómo funciona la actualización
{: #upgradeOverview}

Cuando se publica por primera vez un nuevo modelo base, los modelos personalizados existentes siguen basándose en la versión anterior del modelo base. Hasta que se actualiza un modelo personalizado, todas las operaciones realizadas sobre ese modelo personalizado, como la adición de datos o el entrenamiento del modelo, afectan a la versión existente del modelo. De forma similar, todas las solicitudes de reconocimiento que especifican el modelo personalizado utilizan la versión existente del modelo.

La actualización de lugar a dos versiones de un modelo personalizado, una basada en la versión anterior del modelo base y una basada en la versión más reciente del modelo base. Una vez que se actualiza un modelo personalizado, todas las operaciones realizadas sobre ese modelo personalizado afectan a la versión nueva y actualizada del modelo. Ya no es posible añadir datos a la versión anterior del modelo ni entrenar la versión anterior. Además, tenga en cuenta que una operación de actualización no puede deshacer.

Cuando actualiza cualquiera de los tipos de modelo personalizados, no es necesario que actualice sus recursos individuales. En el caso de un modelo de lenguaje personalizado, el servicio actualiza automáticamente cualquier corpus, gramática y palabra que se haya definido para el modelo. De la misma manera, cuando actualiza un modelo acústico personalizado, el servicio actualiza automáticamente sus recursos de audio.

De forma predeterminada, el servicio utiliza la versión más reciente del modelo personalizado para las solicitudes de reconocimiento. Sin embargo, puede seguir utilizando la versión anterior de un modelo personalizado para el reconocimiento de voz. Para obtener más información, consulte [Cómo realizar solicitudes de reconocimiento con modelos personalizados actualizados](#upgradeRecognition).

## Actualización de un modelo de lenguaje personalizado
{: #upgradeLanguage}

Siga estos pasos para actualizar un modelo de lenguaje personalizado:

1.  Asegúrese de que el modelo de lenguaje personalizado esté en el estado `ready` o `available`. Puede consultar el estado de un modelo con el método `GET /v1/customizations/{customization_id}`. Si el estado del modelo es `ready`, actualice el modelo antes de entrenarlo con sus datos más recientes.

1.  Actualice el modelo de lenguaje personalizado con el método `POST /v1/customizations/{customization_id}/upgrade_model`:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    El método de actualización es asíncrono. Puede tardar unos minutos en completarse, en función del número de palabras del recurso de palabras del modelo y de la carga actual del servicio.

El servicio devuelve el código de respuesta 200 si el proceso de actualización se inicia correctamente. Puede supervisar el estado de la actualización con el método `GET /v1/customizations/{customization_id}` para sondear el estado del modelo. Utilice un bucle para comprobar el estado cada 10 segundos.

Mientras se está actualizando, el modelo personalizado tiene el estado `upgrading`. Cuando finaliza la actualización, el modelo reanuda el estado que tenía antes de la actualización (`ready` o `available`). El estado de una operación de actualización se consulta del mismo modo que el estado de una operación de entrenamiento. Para obtener más información, consulte [Supervisión de la solicitud de entrenamiento del modelo](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#monitorTraining-language).

El servicio no acepta solicitudes para modificar el modelo de ninguna manera hasta que finaliza la solicitud de actualización. Sin embargo, puede seguir enviando solicitudes de reconocimiento con la versión existente del modelo durante la actualización.

## Actualización de un modelo acústico personalizado
{: #upgradeAcoustic}

Siga estos pasos para actualizar un modelo acústico personalizado. Si el modelo acústico personalizado se ha entrenado con un modelo de lenguaje personalizado, debe realizar dos pasos de actualización adicionales donde se indique.

1.  *Si el modelo acústico personalizado se ha entrenado con un modelo de lenguaje personalizado*, primero debe actualizar el modelo de lenguaje personalizado a la versión más reciente del modelo base. Para obtener más información, consulte [Actualización de un modelo de lenguaje personalizado](#upgradeLanguage).

1.  Asegúrese de que el modelo acústico personalizado esté en el estado `ready` o `available`. Puede consultar el estado de un modelo con el método `GET /v1/acoustic_customizations/{customization_id}`. Si el estado del modelo es `ready`, actualice el modelo antes de entrenarlo con sus datos más recientes.

1.  Actualice el modelo acústico personalizado con el método `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    El método de actualización es asíncrono. Puede tardar entre minutos y horas en completarse, en función de la cantidad de datos de audio que contenga el modelo de audio y de la carga actual del servicio. Al igual que sucede con el proceso de entrenamiento, la actualización suele tardar aproximadamente el doble de la duración del audio del modelo.

1.  *Si el modelo acústico personalizado se ha entrenado con un modelo de lenguaje personalizado*, vuelva a actualizar el modelo acústico personalizado, esta vez con el modelo de lenguaje personalizado actualizado previamente. Utilice el parámetro de consulta `custom_language_model_id` para especificar el ID de personalización del modelo de lenguaje personalizado.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model?custom_language_model_id={custom_language_model_id}"
    ```
    {: pre}

    De nuevo, el método de actualización es asíncrono y la actualización suele tardar aproximadamente el doble de la duración del audio del modelo.

    La solicitud de actualizar el modelo acústico con el modelo de lenguaje puede fallar con un código de respuesta 400 y el mensaje `No se han modificado datos de entrada desde el último entrenamiento`. Si se produce este error, añada el parámetro de consulta booleano `force` a la solicitud y establezca el parámetro en `true`. Utilice el parámetro sólo para imponer una actualización de un modelo acústico personalizado en esta situación en particular.
    {: note}

El servicio devuelve el código de respuesta 200 si el proceso de actualización se inicia correctamente. Puede supervisar el estado de la actualización con el método `GET /v1/acoustic_customizations/{customization_id}` para sondear el estado del modelo. Utilice un bucle para comprobar el estado una vez por minuto.

Mientras se está actualizando, el modelo personalizado tiene el estado `upgrading`. Cuando finaliza la actualización, el modelo reanuda el estado que tenía antes de la actualización (`ready` o `available`). El estado de una operación de actualización se consulta del mismo modo que el estado de una operación de entrenamiento. Para obtener más información, consulte [Supervisión de la solicitud de entrenamiento del modelo](/docs/services/speech-to-text?topic=speech-to-text-acoustic#monitorTraining-acoustic).

El servicio no acepta solicitudes para modificar el modelo de ninguna manera hasta que finaliza la solicitud de actualización. Sin embargo, puede seguir enviando solicitudes de reconocimiento con la versión existente del modelo durante la actualización.

## Errores de actualización
{: #upgradeFailures}

La actualización de un modelo personalizado no se puede iniciar si el servicio está gestionando otra solicitud para el modelo, como por ejemplo una solicitud de entrenamiento o una solicitud para añadir datos. La solicitud de actualización tampoco se puede iniciar en los casos siguientes:

-   El modelo personalizado se encuentra en un estado distinto de `ready` o `available`.
-   El modelo personalizado no contiene datos (palabras personalizadas o recursos de audio).
-   En el caso de un modelo acústico personalizado que se ha entrenado con un modelo de lenguaje personalizado, los modelos personalizados se basan en distintas versiones del modelo base. Debe actualizar el modelo de lenguaje personalizado antes de actualizar el modelo acústico personalizado.

## Listado de información de versión para un modelo personalizado
{: #upgradeList}

Para ver las versiones del modelo base para las que está disponible un modelo personalizado, utilice los siguientes métodos:

-   Para ver información sobre un modelo de lenguaje personalizado, utilice el modelo `GET /v1/customizations/{customization_id}`. Para obtener más información, consulte [Listado de modelos de lenguaje personalizado](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).
-   Para ver información sobre un modelo acústico personalizado, utilice el modelo `GET /v1/acoustic_customizations/{customization_id}`. Para obtener más información, consulte [Listado de modelos acústicos personalizados](/docs/services/speech-to-text?topic=speech-to-text-manageAcousticModels#listModels-acoustic).

En ambos casos, la salida incluye un campo `versions` que muestra información acerca de los modelos base para el modelo personalizado. La salida siguiente muestra información correspondiente a un modelo de lenguaje personalizado actualizado:

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "versions": [
    "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Modelo de ejemplo",
  "description": "Modelo de lenguaje personalizado de ejemplo",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

El campo `versions` indica que el modelo personalizado está disponible para dos versiones del modelo base: la versión antigua, `en-US_BroadbandModel.v07-06082016.06202016`, y la nueva, `en-US_BroadbandModel.v2017-11-15`. Si el modelo personalizado no se actualiza o si solo existe una versión de su modelo base, el campo `versions` muestra una sola versión.

## Cómo realizar solicitudes de reconocimiento con modelos personalizados actualizados
{: #upgradeRecognition}

De forma predeterminada, el servicio utiliza la versión más reciente de un modelo personalizado especificado con una solicitud de reconocimiento. Sin embargo, incluso después de que se actualice un modelo personalizado, puede seguir realizando solicitudes de reconocimiento con la versión anterior del modelo. Utilice el parámetro `base_model_version` de un método de reconocimiento para especificar la versión de un modelo base que se debe utilizar para el reconocimiento de voz.

Por ejemplo, en la siguiente solicitud HTTP se especifica que se va a utilizar la versión más antigua del modelo base. Por lo tanto, también se utiliza la versión más antigua del modelo de lenguaje personalizado especificado.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_BroadbandModel&base_model_version=en-US_BroadbandModel.v07-06082016.06202016&language_customization_id={customization_id}"
```
{: pre}

Puede utilizar esta característica para probar el rendimiento y la precisión de un modelo personalizando comparando las versiones antigua y nueva de su modelo base. Si detecta que el rendimiento de un modelo actualizado falla en algún sentido (por ejemplo, ya no se reconocen algunas palabras), puede seguir utilizando la versión antigua con las solicitudes de reconocimiento.

En la sección sobre [Versión del modelo base](/docs/services/speech-to-text?topic=speech-to-text-input#version) se describe el parámetro `base_model_version` y la forma en que el servicio determina qué versiones de los modelos base y personalizado debe utilizar con una solicitud de reconocimiento. Además de esta información, tenga en cuenta los problemas siguientes cuando pase los dos modelos, el de lenguaje personalizado y el acústico personalizado, con una solicitud de reconocimiento:

-   Ambos modelos personalizados se deben basar en el mismo modelo base (por ejemplo, `en-US_BroadbandModel`).
-   Si ambos modelos personalizados se basan en el modelo base antiguo, el servicio utiliza el modelo base antiguo para el reconocimiento.
-   Si ambos modelos personalizados se basan en el modelo base nuevo, el servicio utiliza el modelo base nuevo para el reconocimiento.
-   Si solo se actualiza uno de los dos modelos personalizados al modelo base más reciente, el servicio utiliza el modelo base antiguo para el reconocimiento. Selecciona el modelo base antiguo porque esa es la versión que los dos modelos personalizados tienen en común.
