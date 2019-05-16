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

# Gestión de gramáticas
{: #manageGrammars}

La interfaz de personalización incluye el método `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` para añadir una gramática a un modelo de lenguaje personalizado. Para obtener más información, consulte [Adición de una gramática al modelo de lenguaje personalizado](/docs/services/speech-to-text/grammar-add.html#addGrammar). La interfaz también incluye los métodos siguientes para obtener una lista y suprimir gramáticas de un modelo de lenguaje personalizado.
{: shortdesc}

## Listado de gramáticas para un modelo de lenguaje personalizado
{: #listGrammars}

La interfaz de personalización proporciona dos métodos para obtener información acerca de las gramáticas de un modelo de lenguaje personalizado:

-   El método `GET /v1/customizations/{customization_id}/grammars` muestra información sobre todas las gramáticas correspondientes a un modelo personalizado.
-   El método `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` muestra información sobre la gramática especificada para un modelo personalizado.

Ambos métodos devuelven la misma información acerca de una gramática. La información incluye el nombre (`name`) de la gramática y el número de palabras no definidas en el vocabulario (`out-of_vocabulary_words`) que se reconocen en la gramática. La respuesta también incluye el `status` de la gramática, que es importante para comprobar el análisis del servicio de la gramática cuando se añade a un modelo personalizado:

-   `being_processed` indica que el servicio todavía está procesando la gramática como respuesta a una solicitud `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`.
-   `analyzed` indica que el servicio ha procesado correctamente la gramática y la ha añadido al modelo personalizado.
-   `undetermined` significa que el servicio ha encontrado un error al procesar la gramática. La información que se devuelve para la gramática incluye un mensaje de error que ofrece una guía para corregir el error.

    Por ejemplo, la gramática podría no ser válida, o podría haber intentado añadir una gramática con el mismo nombre que una gramática existente. Puede intentar añadir de nuevo la gramática e incluir el parámetro `allow_overwrite` con la solicitud. También puede suprimir la gramática y luego intentar añadirla de nuevo.

### Solicitudes y respuestas de ejemplo
{: #listExample-grammars}

En el ejemplo siguiente se muestra información sobre todas las gramáticas que se han añadido al modelo personalizado con el ID de personalización especificado.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars"
```
{: pre}

Se han añadido tres gramáticas correctamente al modelo personalizado: `confirm-xml`, `confirm-abnf` y `list-abnf`.

```javascript
{
  "grammars": [
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-xml",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-abnf",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 8,
      "name": "list-abnf",
      "status": "analyzed"
    }
  ]
}
```
{: codeblock}

En el ejemplo siguiente se muestra información sobre la gramática especificada, `list-abnf`.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/list-abnf"
```
{: pre}

```javascript
{
  "out_of_vocabulary_words": 8,
  "name": "list-abnf",
  "status": "analyzed"
}
```
{: codeblock}

## Supresión de una gramática de un modelo de lenguaje personalizado
{: #deleteGrammar}

Utilice el método `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` para eliminar una gramática existente de un modelo de lenguaje personalizado. Cuando suprime la gramática, el servicio elimina cualquier palabra OOV asociada con la gramática del recurso de palabras del modelo personalizado a menos que

-   La palabra también haya sido añadida por otra gramática o por un corpus.
-   La palabra se haya modificado de alguna manera con el método `POST /v1/customizations/{customization_id}/words` o con el método `PUT /v1/customizations/{customization_id}/words/{word_name}`.

La eliminación de un recurso de gramática no afecta al modelo personalizado hasta que se entrena el modelo con los datos actualizados mediante el método `POST /v1/customizations/{customization_id}/train`. Si ha entrenado correctamente el modelo con la gramática, la gramática sigue estando disponible para el reconocimiento de voz hasta que se vuelve a entrenar el modelo.

### Solicitud de ejemplo
{: #deleteExample-grammar}

En el ejemplo siguiente se suprime la gramática denominada `list-abnf` del modelo personalizado con el ID de personalización especificado.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/ {customization_id}/grammars/list-abnf"
```
{: pre}
