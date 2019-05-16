---

copyright:
  years: 2019
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

# Gestión de recursos de audio
{: #manageAudio}

La interfaz de personalización incluye el método `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`, que sirve para añadir un recurso de audio a un modelo acústico personalizado. Para obtener más información, consulte [Adición de audio al modelo acústico personalizado](/docs/services/speech-to-text/acoustic-create.html#addAudio)). La interfaz también incluye los métodos siguientes para obtener una lista y suprimir recursos de audio de un modelo acústico personalizado.
{: shortdesc}

## Listado de recursos de audio para un modelo acústico personalizado
{: #listAudio}

La interfaz de personalización proporciona dos métodos para obtener información acerca de los recursos de audio de un modelo acústico personalizado:

-   El método `GET /v1/acoustic_customizations/{customization_id}/audio` muestra una lista con información sobre todos los recursos de audio que se han añadido a un modelo personalizado.
-   El método `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` muestra una lista con información sobre un recurso de audio especificado de un modelo personalizado.

Ambos métodos devuelven el nombre (`name`) del recurso de audio, más la siguiente información adicional:

-   `duration` es la duración del audio en segundos. En el caso de un archivo archivador, la figura representa la duración acumulada de todos los archivos de audio que están contenidos en el archivador.
-   `details` identifica el tipo del recurso: `audio` o `archive`. (El tipo es `undetermined` si el servicio no puede validar el recurso, posiblemente porque el usuario ha pasado por error un archivo que no contiene audio.) En el caso de un archivo de audio, los detalles incluyen los valores de `codec` y `frequency` del audio. En el caso de un archivo archivador, incluyen su tipo compresión (`compression`).

Además, si se obtiene una lista de todos los recursos de audio de un modelo, se devuelve el valor `total_minutes_of_audio` (minutos de audio), que es la suma de todos los recursos de audio válidos del modelo. Puede utilizar este valor para determinar si el modelo personalizado tiene suficiente o demasiado audio para empezar a entrenar.

Los métodos también muestran en una lista el estado de los datos de audio. El estado es importante para comprobar el análisis del servicio de los archivos de audio como respuesta a una solicitud de añadirlos a un modelo personalizado:

-   `ok` indica que el servicio ha analizado correctamente los datos de audio. Los datos se pueden utilizar para entrenar el modelo personalizado.
-   `being_processed` indica que el servicio todavía está analizando los datos de audio. El servicio no puede aceptar solicitudes de añadir nuevo audio ni de entrenar el modelo personalizado hasta que finaliza el análisis.
-   `invalid` indica que los datos de audio no son válidos para entrenar el modelo (posiblemente porque tienen un formato o una frecuencia de muestreo incorrectos, o bien porque están dañados).

### Ejemplo de solicitud: obtener una lista de todos los recursos de audio
{: #listExample-audio}

En el ejemplo siguiente se muestra una lista de todos los recursos de audio del modelo acústico personalizado con el ID de personalización especificado. El modelo acústico tiene tres recursos de audio. El servicio ha analizado correctamente `audio1` y `audio2`; todavía está analizando `audio3`.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio"
```
{: pre}

```javascript
{
  "total_minutes_of_audio": 11.45,
  "audio": [
    {
      "duration": 131,
      "name": "audio1",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 22050
      }
      "status": "ok"
    },
    {
      "duration": 556,
      "name": "audio2",
      "details": {
        "type": "archive",
        "compression": "zip"
      },
      "status": "ok"
    },
    {
      "duration": 0,
      "name": "audio3",
      "details": {},
      "status": "being_processed"
    }
  ]
}
```
{: codeblock}

### Solicitud de ejemplo: obtener información sobre un recurso de tipo audio
{: #getExampleAudio}

En el ejemplo siguiente se devuelve información sobre el recurso de tipo de audio denominado `audio1`. El recurso tiene una duración de 131 segundos y está codificado con el codec `pcm_s16le`. Se ha añadido correctamente al modelo.

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

### Solicitud de ejemplo: obtener información sobre un recurso de tipo archivo
{: #getExampleArchive}

En el ejemplo siguiente se devuelve información sobre el recurso de tipo archivador denominado `audio2`. El recurso es un archivo **.zip** que contiene más de 9 minutos de audio. También se ha añadido correctamente al modelo. Como muestra el ejemplo, cuando se consulta de información sobre un recurso de tipo archivador también se proporciona información acerca de los archivos que contiene.

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
  "audio": [
    {
      "duration": 121,
      "name": "audio-file1.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    {
      "duration": 133,
      "name": "audio-file2.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    . . .
  ]
}
```
{: codeblock}

## Supresión de un recurso de audio de un modelo acústico personalizado
{: #deleteAudio}

Utilice el método `DELETE /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` para eliminar un recurso de audio existente de un modelo acústico personalizado. Cuando se suprime un recurso de audio de tipo archivador, el servicio elimina todo el archivador de archivos. La interfaz actual no permite la supresión de archivos individuales de un recurso de archivado.

La eliminación de un recurso de audio no afecta al modelo personalizado hasta que se entrena el modelo con los datos actualizados mediante el método `POST /v1/acoustic_customizations/{customization_id}/train`. Si ha entrenado correctamente el modelo con el recurso, los datos de audio existentes se siguen utilizando para el reconocimiento de voz hasta que se vuelve a entrenar el modelo.

### Solicitud de ejemplo
{: #deleteExample-audio}

El siguiente método suprime el recurso de audio denominado `audio3` del modelo personalizado con el ID de personalización especificado:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio3"
```
{: pre}
