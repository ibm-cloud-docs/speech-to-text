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

# Gestión de corpus
{: #manageCorpora}

La interfaz de personalización incluye el método `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` para añadir un corpus a un modelo de lenguaje personalizado. Para obtener más información, consulte [Adición de un corpus al modelo de lenguaje personalizado](/docs/services/speech-to-text/language-create.html#addCorpus). La interfaz también incluye los métodos siguientes para listar y suprimir corpus de un modelo de lenguaje personalizado.
{: shortdesc}

## Listado de corpus de un modelo de lenguaje personalizado
{: #listCorpora}

La interfaz de personalización proporciona dos métodos para obtener información acerca de los corpus de un modelo de lenguaje personalizado:

-   El método `GET /v1/customizations/{customization_id}/corpora` muestra información sobre todos los corpus correspondientes a un modelo personalizado.
-   El método `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` muestra información sobre el corpus especificado para un modelo personalizado.

Ambos métodos devuelven el nombre (`name`) del corpus, el número total de palabras (`total_words`) leídas del corpus el número de palabras no definidas en el vocabulario (`out-of-vocabulary_words`) extraídas del corpus. Los métodos también muestran una lista del estado (`status`) del corpus. El estado es importante para comprobar el análisis del servicio de un corpus como respuesta a una solicitud de añadirlo a un modelo personalizado:

-   `analyzed` indica que el servicio ha analizado correctamente el corpus. El modelo personalizado se puede entrenar con datos del corpus.
-   `being_processed` indica que el servicio todavía está analizando el corpus. El servicio no puede aceptar solicitudes de añadir nuevos corpus o palabras ni de entrenar el modelo personalizado hasta que finaliza el análisis.
-   `undetermined` indica que el servicio ha encontrado un error al procesar el corpus. La información que se devuelve para el corpus incluye un mensaje de error que ofrece una guía para corregir el error.

    Por ejemplo, el corpus podría no ser válido, o podría haber intentado añadir un corpus con el mismo nombre que un corpus existente. Puede intentar añadir de nuevo el corpus e incluir el parámetro `allow_overwrite` con la solicitud. También puede suprimir el corpus y luego intentar añadirlo de nuevo.

### Solicitudes y respuestas de ejemplo
{: #listExample-corpora}

El ejemplo siguiente muestra una lista de todos los corpus para el modelo personalizado con el ID de personalización especificado:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora"
```
{: pre}

Se han añadido tres corpus al modelo personalizado. El servicio ha analizado correctamente `corpus1`. Todavía está analizando `corpus2` y el análisis de `corpus3` ha fallado.

```javascript
{
  "corpora": [
    {
      "name": "corpus1",
      "total_words": 5037,
      "out_of_vocabulary_words": 401,
      "status": "analyzed"
    },
    {
      "name": "corpus2",
      "total_words": 0,
      "out_of_vocabulary_words": 0,
      "status": "being_processed"
    },
    {
      "name": "corpus3",
      "total_words": 0,
      "out_of_vocabulary_words": 0,
      "status": "undetermined",
      "error": "Analysis of corpus 'corpus3.txt' failed. Please try adding the corpus again by setting the 'allow_overwrite' flag to 'true'."
    }
  ]
}
```
{: codeblock}

En el ejemplo siguiente se devuelve información sobre el corpus llamado `corpus1` para el modelo personalizado con el ID de personalización especificado:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

## Supresión de un corpus de un modelo de lenguaje personalizado
{: #deleteCorpus}

Utilice el método `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` para eliminar un corpus existente de un modelo de lenguaje personalizado. Cuando suprime el corpus, el servicio elimina cualquier palabra OOV asociada con el corpus del recurso de palabras del modelo personalizado a menos que

-   La palabra también haya sido añadida por otro corpus o por una gramática.
-   La palabra se haya modificado de alguna manera con el método `POST /v1/customizations/{customization_id}/words` o con el método `PUT /v1/customizations/{customization_id}/words/{word_name}`.

La eliminación de un corpus no afecta al modelo personalizado hasta que se entrena el modelo con los datos actualizados mediante el método `POST /v1/customizations/{customization_id}/train`. Si ha entrenado correctamente el modelo con el corpus, las palabras del corpus permanecen en el vocabulario del modelo y se aplican al reconocimiento de voz hasta que se vuelve a entrenar el modelo.

### Solicitud de ejemplo
{: #deleteExample-corpus}

En el ejemplo siguiente se suprime el corpus denominado `corpus3` del modelo personalizado con el ID de personalización especificado.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus3"
```
{: pre}
