---

copyright:
  years: 2017, 2019
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

# Creación de un modelo acústico personalizado
{: #acoustic}

Siga estos pasos para crear un modelo acústico personalizado para el servicio {{site.data.keyword.speechtotextshort}}:
{: shortdesc}

1.  [Cree un modelo acústico personalizado](#createModel-acoustic). Puede crear varios modelos personalizados para los mismos dominios o entornos o para dominios o entornos diferentes. El proceso es el mismo para cualquier modelo que cree. Sin embargo, solo puede especificar un único modelo acústico personalizado a la vez con una solicitud de reconocimiento.
1.  [Añada audio al modelo acústico personalizado](#addAudio). El servicio acepta los mismos formatos de archivo de audio para el modelado acústico que para el reconocimiento de voz. También acepta archivos archivadores que contienen varios archivos de audio. Los archivos archivadores son el método recomendado para añadir recursos de audio. Puede repetir el método para añadir más archivos de audio o archivadores a un modelo personalizado.
1.  [Entrene el modelo acústico personalizado](#trainModel-acoustic). Cuando haya añadido recursos de audio al modelo personalizado, debe entrenar el modelo. El entrenamiento prepara el modelo acústico personalizado para que se utilice en el reconocimiento de voz. El entrenamiento puede llevar bastante tiempo. La duración del entrenamiento depende de la cantidad de datos de audio que contenga el modelo.

    Puede especificar un modelo de lenguaje personalizado de ayuda durante el entrenamiento de su modelo acústico personalizado. Un modelo de lenguaje personalizado que incluya transcripciones de sus archivos de audio o palabras OOV (no definidas en el vocabulario) del dominio de los archivos de audio puede mejorar la calidad del modelo acústico personalizado. Para obtener más información, consulte el tema sobre [Entrenamiento de un modelo acústico personalizado con un modelo de lenguaje personalizado](/docs/services/speech-to-text/acoustic-both.html#useBothTrain).
1.  Después de entrenar el modelo personalizado, puede utilizarlo con las solicitudes de reconocimiento. Si el audio que se pasa para la transcripción tiene calidades acústicas parecidas a las del audio del modelo personalizado, los resultados reflejan una mejor comprensión por parte del servicio. Para obtener más información, consulte [Utilización de un modelo acústico personalizado](/docs/services/speech-to-text/acoustic-use.html).

    Puede pasar tanto un modelo acústico personalizado como un modelo de lenguaje personalizado en la misma solicitud de reconocimiento para mejorar aún más la precisión del reconocimiento. Para obtener más información, consulte [Utilización de modelos de lenguaje personalizado y acústico personalizado durante el reconocimiento de voz](/docs/services/speech-to-text/acoustic-both.html#useBothRecognize).

Los pasos para la creación de un modelo acústico personalizado son iterativos. Puede añadir o suprimir audio y entrenar o volver a entrenar un modelo con la frecuencia que desee. Debe volver a entrenar un modelo para que los cambios en su audio entren en vigor. Cuando se vuelve a entrenar un modelo, se utiliza todos los datos de audio en el entrenamiento (no solo en los datos nuevos). Por lo tanto, el tiempo de entrenamiento es proporcional a la cantidad total de audio contenida en el modelo.

La personalización del modelo acústico está disponible como funcionalidad beta para todos los idiomas. Para obtener más información, consulte [Soporte de idiomas para la personalización](/docs/services/speech-to-text/custom.html#languageSupport).
{: note}

## Creación de un modelo acústico personalizado
{: #createModel-acoustic}

Para crear un nuevo modelo acústico personalizado se utiliza el método `POST /v1/acoustic_customizations`. Puede crear tantos modelos acústicos personalizados como desee, pero solo puede utilizar un modelo acústico personalizado a la vez con una solicitud de reconocimiento de voz. El método acepta un objeto JSON que define los atributos del nuevo modelo personalizado como el cuerpo de la solicitud.

<table>
  <caption>Tabla 1. Atributos de un nuevo modelo acústico personalizado</caption>
  <tr>
    <th style="width:20%; text-align:left">Parámetro</th>
    <th style="width:12%; text-align:center">Tipo de datos</th>
    <th style="text-align:left">Descripción</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>Obligatorio</em></td>
    <td style="text-align:center">Serie</td>
    <td>
      Un nombre definido por el usuario para el nuevo modelo acústico personalizado. Utilice un nombre que describa
      el entorno acústico del modelo personalizado, como por ejemplo <code>Modelo personalizado móvil</code> o <code>Modelo personalizado para coche</code>. Utilice un nombre que sea exclusivo entre todos sus modelos acústicos personalizados. Utilice un nombre en el idioma del modelo personalizado.
    </td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>Obligatorio</em></td>
    <td style="text-align:center">Serie</td>
    <td>
      El nombre del modelo base que debe personalizarse mediante el nuevo modelo. Debe utilizar el nombre de un modelo que devuelva el método <code>GET /v1/models</code>. El nuevo modelo personalizado solo se puede utilizar con el modelo base que personaliza.
    </td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>Opcional</em></td>
    <td style="text-align:center">Serie</td>
    <td>
      Una descripción del nuevo modelo. Utilice una descripción en el idioma del modelo personalizado.
    </td>
  </tr>
</table>

En el ejemplo siguiente se crea un nuevo modelo acústico personalizado denominado `Modelo acústico de ejemplo`. El modelo se crea para el modelo base `en-US_BroadbandModel` y su descripción es `Modelo acústico personalizado de ejemplo`. La cabecera `Content-Type` especifica que se pasan datos JSON al método.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Modelo acústico de ejemplo\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Modelo acústico personalizado de ejemplo\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations"
```
{: pre}

El ejemplo devuelve el ID de personalización del nuevo modelo. Cada modelo personalizado se identifica mediante un ID de personalización exclusivo, que es un GUID (identificador global exclusivo). El GUID del modelo personalizado se especifica con el parámetro `customization_id` de las llamadas que están asociadas con el modelo.

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

El nuevo modelo personalizado es propiedad de la instancia de servicio cuyas credenciales se utilizan para crearlo. Para obtener más información, consulte [Propiedad de modelos personalizados](/docs/services/speech-to-text/custom.html#customOwner).

## Adición de audio al modelo acústico personalizado
{: #addAudio}

Una vez que haya creado el modelo acústico personalizado, el paso siguiente consiste en añadir al mismo recursos de audio. Utilice el método `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` para añadir un recurso de audio a un modelo personalizado. Puede añadir:

-   Un archivo de audio individual en cualquier formato que esté soportado para el reconocimiento de voz (consulte [Formatos de audio](/docs/services/speech-to-text/audio-formats.html)).
-   Un archivo archivador (un archivo **.zip** o **.tar.gz**) que incluya varios archivos de audio. El hecho de recopilar varios archivos de audio en un solo archivo archivador y de cargar dicho archivo resulta más eficiente que añadir archivos de audio individualmente.

El recurso de audio se pasa como cuerpo de la solicitud y se asigna al recurso un `audio_name`. Para obtener más información, consulte [Cómo trabajar con recursos de audio](/docs/services/speech-to-text/acoustic-resource.html).

En los ejemplos siguientes se muestra cómo añadir recursos de tipo de audio y de tipo archivador:

-   En este ejemplo se añade un recurso de tipo de audio al modelo acústico personalizado con el `customization_id` especificado. La cabecera `Content-Type` identifica el tipo de audio como `audio/wav`. El archivo de audio, **audio1.wav**, se pasa como cuerpo de la solicitud y se asigna al recurso el nombre `audio1`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/wav"
    --data-binary @audio1.wav
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
    ```
    {: pre}

-   En este ejemplo se añade un recurso de tipo archivador al modelo acústico personalizado especificado. La cabecera `Content-Type` identifica el tipo de archive como `application/zip`. La cabecera `Contained-Content-Type` indica que todos los archivos contenidos en el archivador tienen el formato `audio/l16` y se muestran a una velocidad de 16 kHz. El archivo archivador, **audio2.zip**, se pasa como cuerpo de la solicitud y se asigna al recurso el nombre `audio2`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/zip"
    --header "Contained-Content-Type: audio/l16;rate=16000"
    --data-binary @audio2.zip
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
    ```
    {: pre}

El método también acepta un parámetro de consulta opcional `allow_overwrite` para sobrescribir un recurso de audio existente para un modelo personalizado. Utilice el parámetro si tiene que actualizar un recurso de audio después de añadirlo a un modelo.

Este método es asíncrono. Puede tardar varios segundos en completarse, en función de la duración del audio. En el caso de un archivo archivador, la duración de la operación depende de la duración de los archivos de audio. Para obtener más información sobre cómo comprobar el estado de una solicitud para añadir un recurso de audio, consulte [Supervisión de la solicitud de adición de audio](#monitorAudio).

Puede añadir a un modelo personalizado el número de recursos de audio que desee mediante una llamada al método para cada archivo de audio o para cada archivo archivado. Un recurso de audio se debe haber añadido por completo para poder añadir otro. Para poder entrenar un modelo personalizado, debe añadir un mínimo de 10 minutos y un máximo de 200 horas de audio que incluya voz, no silencio. Ningún recurso de tipo de audio o archivador puede tener más de 100 MB. Para obtener más información, consulte [Directrices para añadir recursos de audio](/docs/services/speech-to-text/acoustic-resource.html#audioGuidelines).

### Supervisión de la solicitud de adición de audio
{: #monitorAudio}

El servicio devuelve el código de respuesta 201 si el audio es válido. Luego analiza de forma asíncrona el contenido del archivo o archivos de audio y extrae automáticamente información sobre el audio, como su duración, la frecuencia de muestreo y la codificación. No puede enviar solicitudes para añadir más audio a un modelo acústico personalizado, ni entrenar el modelo, hasta que servicio termine de analizar todos los archivos de audio para la solicitud actual.

Para determinar el estado de la solicitud, utilice el método `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` para sondear el estado del audio. El método acepta el GUID del modelo personalizado y el nombre del recurso de audio. Su respuesta incluye el `status` (estado) del recurso, que tiene uno de los valores siguientes:

-   `ok` indica que el audio es aceptable y que el análisis está completo.
-   `being_processed` indica que el servicio todavía está analizando los datos de audio.
-   `invalid` indica que el archivo de audio no es aceptable para el proceso. Es posible que no tenga un formato correcto, que la frecuencia de muestreo sea errónea o que no sea un archivo de audio. En el caso de un archivo archivador, si alguno de los archivos de audio que contiene no es válido, el archivador no es válido.

El contenido de la respuesta y la ubicación del campo `status` dependen del tipo de recurso, que puede ser audio o archivador.

-   *En el caso de un recurso de tipo audio*, el campo `status` se encuentra en el objeto de nivel superior (`AudioListing`).

    ```bash
    curl -X GET -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
    ```
    {: pre}

    ```javascript
    {
      "duration": 131,
      "name": "audio1",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 22050
      }
      "status": "ok"
    }
    ```
    {: codeblock}

-   *En el caso de recurso de tipo archivador*, el campo `status` se encuentra en el objeto de segundo nivel (`AudioResource`), que está anidado en el campo `container`.

    ```bash
    curl -X GET -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
    ```
    {: pre}

    ```javascript
    {
      "container": {
        "duration": 556,
        "name": "audio2",
        "details": {
          "type": "archive",
          "compression": "zip"
        },
        "status": "ok"
      },
      . . .
    ```
    {: codeblock}

Utilice un bucle para comprobar el estado del recurso de audio cada pocos segundos hasta que esté en el estado `ok`. Para obtener más información acerca de otros campos que devuelve el método, consulte [Listado de recursos de audio de un modelo acústico personalizado](/docs/services/speech-to-text/acoustic-audio.html#listAudio).

## Entrenamiento del modelo acústico personalizado
{: #trainModel-acoustic}

Una vez que haya llenado un modelo acústico personalizado con recursos de audio, debe entrenar el modelo con los nuevos datos. El entrenamiento prepara un modelo personalizado para su uso en el reconocimiento de voz. No se puede utilizar un modelo para solicitudes de reconocimiento hasta que se le entrene con los nuevos datos. Además, las actualizaciones de un modelo personalizado en forma de recursos de audio nuevos o modificados no se reflejan en el modelo hasta que este se entrena con los cambios.

Para entrenar un modelo personalizado se utiliza el modelo `POST /v1/acoustic_customizations/{customization_id}/train`. Debe pasar al método el ID de personalización del modelo que desea entrenar, como en el ejemplo siguiente.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train"
```
{: pre}

El método también acepta un parámetro de consulta opcional `custom_language_model_id` para especificar un modelo de lenguaje personalizado creado por separado que se va a utilizar durante el entrenamiento. Se puede entrenar con un modelo de lenguaje personalizado que contenga transcripciones de archivos de audio o que contenga palabras del corpus o palabras OOV (no definidas en el vocabulario) que sean relevantes para el contenido de los archivos de audio. Ambos modelos personalizados se deben basar en la misma versión del mismo modelo base para que el entrenamiento tenga éxito. Para obtener más información, consulte el tema sobre [Entrenamiento de un modelo acústico personalizado con un modelo de lenguaje personalizado](/docs/services/speech-to-text/acoustic-both.html#useBothTrain).

El método de entrenamiento es asíncrono. El entrenamiento puede tardar entre minutos y horas en completarse, en función de la cantidad de datos de audio que contenga el modelo acústico personalizado y de la carga actual del servicio. Como directriz general, el entrenamiento de un modelo acústico personalizado tarda entre dos y cuatro veces la duración de sus datos de audio. El rango de tiempo depende del modelo que se está entrenando y de la naturaleza del audio (por ejemplo, si el audio es limpio o ruidoso). Por ejemplo, se puede tardar entre 4 y 8 horas en entrenar un modelo que contenga 2 horas de audio. Para obtener más información sobre cómo consultar el estado de una operación de entrenamiento, consulte [Supervisión de la solicitud de entrenamiento del modelo](#monitorTraining-acoustic).

### Supervisión de la solicitud de entrenamiento del modelo
{: #monitorTraining-acoustic}

El servicio devuelve el código de respuesta 200 si el proceso de entrenamiento se inicia correctamente. El servicio no acepta otras solicitudes de entrenamiento, ni solicitudes para añadir más recursos de audio, hasta que se complete la solicitud existente.

Para determinar el estado de una solicitud de entrenamiento, utilice el método `GET /v1/acoustic_customizations/{customization_id}` para sondear el estado del modelo. El método acepta el ID de personalización del modelo acústico y devuelve su estado, como en el ejemplo siguiente:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Modelo de ejemplo",
  "description": "Modelo acústico personalizado de ejemplo",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

La respuesta incluye los campos `status` y `progress` que muestran el estado actual del modelo. El significado del campo `progress` depende del estado del modelo. El campo `status` puede tener uno de los valores siguientes:

-   `pending` indica que el modelo se ha creado pero está a la espera de que se añadan datos de entrenamiento o que el servicio termine de analizar los datos que se han añadido. El campo `progress` es `0`.
-   `ready` indica que el modelo está listo para ser entrenado. El campo `progress` es `0`.
-   `training` indica que el modelo se está entrenando. El campo `progress` pasa de `0` a `100` cuando termina el entrenamiento. <!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available` indica que el modelo está entrenado y listo para su uso. El campo `progress` es `100`.
-   `upgrading` indica que el modelo se está actualizando. El campo `progress` es `0`.
-   `failed` indica que el entrenamiento del modelo ha fallado. El campo `progress` es `0`.

Utilice un bucle para comprobar el estado del entrenamiento una vez por minuto hasta que el modelo esté en estado `available`. Para obtener más información acerca de otros campos que devuelve el método, consulte [Listado de modelos acústicos personalizados](/docs/services/speech-to-text/acoustic-models.html#listModels-acoustic).

### Errores de entrenamiento
{: #failedTraining-acoustic}

El entrenamiento de un modelo acústico personalizado no se puede iniciar si el servicio está gestionando otra solicitud para el modelo personalizado. Una solicitud conflictiva podría ser otra solicitud de entrenamiento o una solicitud para añadir recursos de audio al modelo. También es posible que el entrenamiento no se inicie por los siguientes motivos:

-   El modelo personalizado contiene menos de 10 minutos de datos de audio.
-   El modelo personalizado contiene más de 200 horas de datos de audio.
-   Uno o varios de los recursos de audio del modelo personalizado no son válidos.
-   Ha pasado un modelo de lenguaje personalizado incompatible con el parámetro de consulta `custom_language_model_id`. Ambos modelos personalizados se deben basar en la misma versión del mismo modelo base.

Si el estado del entrenamiento de un modelo personalizado es `failed`, utilice los métodos `GET /v1/acoustic_customizations/{customization_id}/audio` y `GET /v1/acoustic_customizations/{custimozation_id}/audio/{audio_name}` para examinar los recursos de audio del modelo y solucionar los problemas que detecte. Para obtener más información, consulte [Listado de recursos de audio para un modelo acústico personalizado](/docs/services/speech-to-text/acoustic-audio.html#listAudio).
