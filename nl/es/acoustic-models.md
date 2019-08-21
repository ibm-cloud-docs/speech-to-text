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

# Gestión de modelos acústicos personalizados
{: #manageAcousticModels}

La interfaz de personalización incluye el método `POST /v1/acoustic_customizations` para crear un modelo acústico personalizado. La interfaz también incluye el método `POST /v1/acoustic_customizations/train` para entrenar un modelo personalizado con sus recursos de audio más recientes. Para obtener más información,
consulte
{: shortdesc}

-   [Creación de un modelo acústico personalizado](/docs/services/speech-to-text?topic=speech-to-text-acoustic#createModel-acoustic)
-   [Entrenamiento del modelo acústico personalizado](/docs/services/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic)

Además, la interfaz incluye métodos para ver información acerca de los modelos acústicos personalizados, restablecer un modelo personalizado en su estado inicial, actualizar un modelo personalizado y suprimir un modelo personalizado. No puede entrenar, restablecer, actualizar o suprimir un modelo personalizado mientras el servicio está gestionando otra operación en ese modelo, incluida la adición de recursos de audio al modelo.

## Listado de modelos acústicos personalizados
{: #listModels-acoustic}

La interfaz de personalización proporciona dos métodos para ver información acerca de los modelos acústicos personalizados propiedad de las credenciales de servicio:

-   El método `GET /v1/acoustic_customizations` muestra información sobre todos los modelos acústicos personalizados o sobre todos los modelos acústicos personalizados para un idioma especificado.
-   El método `GET /v1/acoustic_customizations/{customization_id}` muestra información sobre un modelo acústico personalizado especificado. Utilice este método para sondear el servicio sobre el estado de una solicitud de entrenamiento.

Ambos métodos devuelven la información siguiente acerca de un modelo acústico personalizado:

-   `customization_id` identifica el GUID (identificador global exclusivo) del modelo personalizado. El GUID sirve para identificar el modelo en los métodos de la interfaz.
-   `created` es la fecha y la hora en Hora Universal Coordinada (UTC) en que se ha creado el modelo personalizado.
-   `updated` es la fecha y la hora en Hora Universal Coordinada (UTC) en que se ha actualizado el modelo personalizado.
-   `language` es el idioma del modelo personalizado.
-   `owner` identifica las credenciales de la instancia de servicio propietaria del modelo personalizado.
-   `name` es el nombre del modelo personalizado.
-   `description` muestra la descripción del modelo personalizado, si se ha especificada una al crearlo.
-   `base_model_name` indica el nombre del modelo de lenguaje para el que se ha creado el modelo personalizado.
-   `versions` proporciona una lista de las versiones disponibles del modelo personalizado. Cada elemento de la matriz indica una versión del modelo base con la que se puede utilizar el modelo personalizado. Solo existen varias versiones si se ha actualizado el modelo personalizado. De lo contrario, solo se muestra una versión. Para obtener más información, consulte [Listado de información de versión para un modelo personalizado](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeList).

Los métodos también devuelven un campo `status` que indica el estado del modelo personalizado:

-   `pending` indica que el modelo se ha creado. Está a la espera de que se añadan datos de entrenamiento válidos (recursos de audio) o de que el servicio termine de analizar datos que se han añadido.
-   `ready` indica que el modelo contiene datos de audio válidos y que está listo para ser entrenado. Si el modelo contiene una mezcla de recursos de audio válidos y no válidos, el entrenamiento del modelo falla a menos que establezca el parámetro de consulta `strict` en `false`. Para obtener más información, consulte [Errores de entrenamiento](/docs/services/speech-to-text?topic=speech-to-text-acoustic#failedTraining-acoustic).
-   `training` indica que el modelo se está entrenando con los datos de audio.
-   `available` indica que el modelo se ha entrenado y está preparado para que se utilice con solicitudes de reconocimiento.
-   `upgrading` indica que el modelo se está actualizando.
-   `failed` indica que el entrenamiento del modelo ha fallado. Examine los recursos de audio del modelo para determinar qué es lo que ha impedido que se entrenara el modelo. Los posibles errores no incluyen audio insuficiente, exceso de audio o un recurso de audio no válido.

Además, la salida incluye un campo `progress` que indica el progreso actual del entrenamiento del modelo personalizado. Si ha utilizado el método `POST /v1/acoustic_customizations/{customization_id}/train` para empezar a entrenar el modelo, este campo indica el progreso actual de dicha solicitud como un porcentaje completado. En este momento, el valor del campo es `100` si el estado es `available`; de lo contrario, es `0`.

### Solicitudes y respuestas de ejemplo
{: #listExample-acoustic}

El ejemplo siguiente incluye el parámetro de consulta `language` para ver todos los modelos acústicos personalizados en inglés de Estados Unidos que son propiedad de las credenciales especificadas:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations?language=en-US"
```
{: pre}

Las credenciales poseen dos modelos de este tipo. El primer modelo está a la espera de datos o está siendo procesado por el servicio. El segundo modelo está completamente entrenado y listo para utilizarse.

```javascript
{
  "customizations": [
    {
      "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
      "created": "2016-06-01T18:42:25.324Z",
      "updated": "2016-06-01T18:42:25.324Z",
      "language": "en-US",
      "versions": [
        "en-US_BroadbandModel.v07-06082016.06202016",
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Modelo uno de ejemplo",
      "description": "Modelo acústico personalizado de ejemplo",
      "base_model_name": "en-US_BroadbandModel",
      "status": "pending",
      "progress": 0
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732fae4829",
      "created": "2016-06-01T18:51:37.291Z",
      "updated": "2016-06-01T19:21:06.825Z",
      "language": "en-US",
      "versions": [
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Modelo dos de ejemplo",
      "description": "Modelo acústico personalizado de ejemplo dos",
      "base_model_name": "en-US_BroadbandModel",
      "status": "available",
      "progress": 100
    }
  ]
}
```
{: codeblock}

En el ejemplo siguiente se devuelve información sobre el modelo personalizado que tiene el ID de personalización especificado:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "versions": [
    "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Modelo uno de ejemplo",
  "description": "Modelo acústico personalizado de ejemplo",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

## Restablecimiento de un modelo acústico personalizado
{: #resetModel-acoustic}

Utilice el método `POST /v1/acoustic_customizations/{customization_id}/reset` para restablecer un modelo acústico personalizado. Cuando se restablece un modelo personalizado, se eliminan todos los recursos de audio del modelo y se inicializa el modelo con su estado en el momento de la creación. El método no suprime el propio modelo ni los metadatos, como por ejemplo su nombre y su idioma. Sin embargo, cuando se restablece un modelo, sus recursos de audio se eliminan y se deben volver a crear.

### Solicitud de ejemplo
{: #resetExample-acoustic}

En el ejemplo siguiente se restablece el modelo acústico personalizado con el ID de personalización especificado:

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/reset"
```
{: pre}

## Supresión de un modelo acústico personalizado
{: #deleteModel-acoustic}

Utilice el método `DELETE /v1/acoustic_customizations/{customization_id}` para suprimir un modelo acústico personalizado que ya no necesita. El método suprime todo el audio que está asociado con el modelo personalizado y el propio modelo. Utilice este método con precaución: un modelo personalizado y sus datos no se pueden recuperar después de suprimir el modelo.

### Solicitud de ejemplo
{: #deleteExample-acoustic}

En el ejemplo siguiente se suprime el modelo acústico personalizado con el ID de personalización especificado:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}
