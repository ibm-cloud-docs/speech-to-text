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

# Gestión de modelos de lenguaje personalizado
{: #manageLanguageModels}

La interfaz de personalización incluye el método `POST /v1/customizations` para la creación de un modelo de lenguaje personalizado. La interfaz también incluye el método `POST /v1/customizations/train` para entrenar un modelo personalizado con los datos más recientes de su recurso de palabras. Para obtener más información,
consulte
{: shortdesc}

-   [Creación de un modelo de lenguaje personalizado](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language)
-   [Entrenamiento del modelo de lenguaje personalizado](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language)

Además, la interfaz incluye métodos para ver información acerca de los modelos de lenguaje personalizados, restablecer un modelo personalizado en su estado inicial, actualizar un modelo personalizado y suprimir un modelo personalizado. No puede entrenar, restablecer, actualizar o suprimir un modelo personalizado mientras el servicio está gestionando otra operación en ese modelo, incluida la adición de recursos al modelo.

## Listado de modelos de lenguaje personalizado
{: #listModels-language}

La interfaz de personalización proporciona dos métodos para ver información acerca de los modelos de lenguaje personalizado propiedad de las credenciales especificadas:

-   El método `GET /v1/customizations` muestra información sobre todos los modelos de lenguaje personalizado o sobre todos los modelos de lenguaje personalizado para un idioma especificado.
-   El método `GET /v1/customizations/{customization_id}` muestra información sobre un modelo de lenguaje personalizado especificado. Utilice este método para sondear el servicio sobre el estado de una solicitud de entrenamiento o de una solicitud para añadir palabras nuevas.

Ambos métodos devuelven la información siguiente acerca de un modelo personalizado:

-   `customization_id` identifica el GUID (identificador global exclusivo) del modelo personalizado. El GUID sirve para identificar el modelo en los métodos de la interfaz.
-   `created` es la fecha y la hora en Hora Universal Coordinada (UTC) en que se ha creado el modelo personalizado.
-   `updated` es la fecha y la hora en Hora Universal Coordinada (UTC) en que se ha actualizado el modelo personalizado.
-   `language` es el idioma del modelo personalizado.
-   `dialect` es el dialecto del idioma correspondiente al modelo personalizado, que no necesariamente coincide con el idioma del modelo personalizado para los modelos de español. Para obtener más información, consulte la descripción del parámetro `dialect` en [Creación de un modelo de lenguaje personalizado](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language).
-   `owner` identifica las credenciales de la instancia de servicio propietaria del modelo personalizado.
-   `name` es el nombre del modelo personalizado.
-   `description` muestra la descripción del modelo personalizado, si se ha especificada una al crearlo.
-   `base_model` indica el nombre del modelo de lenguaje para el que se ha creado el modelo personalizado.
-   `versions` proporciona una lista de las versiones disponibles del modelo personalizado. Cada elemento de la matriz indica una versión del modelo base con la que se puede utilizar el modelo personalizado. Solo existen varias versiones si se ha actualizado el modelo personalizado. De lo contrario, solo se muestra una versión. Para obtener más información, consulte [Listado de información de versión para un modelo personalizado](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeList).

El método también devuelve un campo `status` que indica el estado del modelo personalizado:

-   `pending` indica que el modelo se ha creado. Está a la espera de que se añadan datos de entrenamiento válidos (corpus, gramáticas o palabras) o de que el servicio termine de analizar datos que se han añadido.
-   `ready` indica que el modelo contiene datos válidos y que está listo para ser entrenado. Si el modelo contiene una mezcla de recursos válidos y no válidos, el entrenamiento del modelo falla a menos que establezca el parámetro de consulta `strict` en `false`. Para obtener más información, consulte [Errores de entrenamiento](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#failedTraining-language).
-   `training` indica que el modelo se está entrenando con los datos.
-   `available` indica que el modelo se ha entrenado y está preparado para que se utilice con una solicitud de reconocimiento.
-   `upgrading` indica que el modelo se está actualizando.
-   `failed` indica que el entrenamiento del modelo ha fallado. Examine las palabras del recurso de palabras del modelo para determinar los errores que han impedido que el modelo se entrene.

Además, la salida incluye un campo `progress` que indica el progreso actual del entrenamiento del modelo personalizado. Si ha utilizado el método `POST /v1/customizations/{customization_id}/train` para empezar a entrenar el modelo, este campo indica el progreso actual de dicha solicitud como un porcentaje completado. En este momento, el valor del campo es `100` si el estado es `available`; de lo contrario, es `0`.

### Solicitudes y respuestas de ejemplo
{: #listExample-language}

El ejemplo siguiente incluye el parámetro de consulta `language` para ver todos los modelos de lenguaje personalizados en inglés de Estados Unidos que son propiedad de las credenciales especificadas:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations?language=en-US"
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
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732fae4829",
      "created": "2016-06-01T18:51:37.291Z",
      "updated": "2016-06-01T20:02:10.624Z",
      "language": "en-US",
      "dialect": "en-US",
      "versions": [
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Modelo dos de ejemplo",
      "description": "Modelo dos de lenguaje personalizado de ejemplo",
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
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T18:42:25.324Z",
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

## Restablecimiento de un modelo de lenguaje personalizado
{: #resetModel-language}

Utilice el método `POST /v1/customizations/{customization_id}/reset` para restablecer un modelo personalizado. Cuando se restablece un modelo, se eliminan todos los corpus y todas las palabras del modelo y se inicializa el modelo con su estado en el momento de la creación. El método no suprime el propio modelo ni los metadatos, como por ejemplo su nombre y su idioma. Sin embargo, cuando se restablece un modelo, su recurso de palabras está vacío y se debe volver a crear añadiendo corpus y palabras.

### Solicitud de ejemplo
{: #resetExample-language}

En el ejemplo siguiente se restablece el modelo personalizado con el ID de personalización especificado:

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/reset"
```
{: pre}

## Supresión de un modelo de lenguaje personalizado
{: #deleteModel-language}

Utilice el método `DELETE /v1/customizations/{customization_id}` para suprimir un modelo de lenguaje personalizado que ya no necesita. El método suprime todos los corpus y todas las palabras que están asociados con el modelo personalizado y el propio modelo. Utilice este método con precaución: un modelo personalizado y sus datos no se pueden recuperar después de suprimir el modelo.

### Solicitud de ejemplo
{: #deleteExample-language}

En el ejemplo siguiente se suprime el modelo personalizado con el ID de personalización especificado:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}
