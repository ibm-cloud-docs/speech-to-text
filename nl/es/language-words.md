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

# Gestión de palabras personalizadas
{: #manageWords}

La interfaz de personalización incluye los métodos `POST /v1/customizations/{customization_id}/words` y `PUT /v1/customizations/{customization_id}/words/{word_name}`, que sirven para añadir o modificar palabras para un modelo personalizado. Para obtener más información, consulte [Adición de palabras al modelo de lenguaje personalizado](/docs/services/speech-to-text/language-create.html#addWords). La interfaz también incluye los métodos siguientes para listar y suprimir palabras de un modelo de lenguaje personalizado.
{: shortdesc}

Lo más probable es que añada la mayoría de las palabras desde los corpus. Asegúrese de conocer la codificación de caracteres que se utiliza en los archivos de texto para los corpus. El servicio conserva la codificación que encuentra en los archivos de texto. Debe utilizar dicha codificación cuando trabaje con palabras individuales en el modelo de lenguaje personalizado. Cuando especifique una palabra con el método `GET`, `PUT` o `DELETE /v1/customizations/{customization_id}/words/{word_name}`, debe codificar en URL el valor `word_name` que pase en el URL si la palabra incluye caracteres que no sean ASCII. Para obtener más información, consulte [Codificación de caracteres](/docs/services/speech-to-text/language-resource.html#charEncoding).
{: important}

## Listado de las palabras de un modelo de lenguaje personalizado
{: #listWords}

La interfaz de personalización ofrece dos métodos para obtener una lista de las palabras de un modelo de lenguaje personalizado:

-   El método `GET /v1/customizations/{customization_id}/words` muestra información sobre las palabras del recurso de palabras del modelo personalizado. El método incluye dos parámetros de consulta opcionales:
    -   El parámetro `word_type` especifica las palabras que se van a mostrar en la lista:

        -   `all` (valor predeterminado) muestra todas las palabras.
        -   `user` muestra solo palabras personalizadas que el usuario ha añadido o modificado.
        -   `corpora` muestra solo las palabras OOV que se han extraído de corpus.
        -   `grammars` muestra solo palabras OOV que se han extraído de gramáticas.
    -   El parámetro `sort` indica cómo se van a ordenar las palabras. El parámetro acepta dos argumentos para indicar cómo se van a clasificar las palabras: `alphabetical` y `count`. Puede añadir un valor `+` o `-` opcional delante de un argumento para indicar si los resultados se van a clasificar en orden ascendente o descendente. De forma predeterminada, el método muestra las palabras en orden alfabético ascendente.
-   El método `GET /v1/customizations/{customization_id}/words/{word_name}` muestra información sobre una sola palabra especificada del recurso de palabras del modelo.

Además de un campo `word` que identifica la palabra, ambos métodos devuelven la información siguiente acerca de cada palabra:

-   Un campo `sounds_like` que representa una matriz de pronunciaciones de la palabra. La matriz puede incluir la pronunciación que genera automáticamente el servicio si no se proporciona ningún valor de pronunciación para la palabra. Para obtener más información, consulte [Utilización del campo sounds_like](/docs/services/speech-to-text/language-resource.html#soundsLike).
-   Un campo `display_as` que muestra la ortografía de la palabra personalizada que muestra el servicio en las transcripciones. El campo contiene una serie vacía si no se proporciona ningún valor display-as para la palabra, en cuyo caso la palabra se muestra tal como se ha pronunciado. Para obtener más información, consulte [Utilización del campo display_as](/docs/services/speech-to-text/language-resource.html#displayAs).
-   Un campo `source` que indica cómo se ha añadido la palabra al recurso de palabras del modelo personalizado. El campo incluye el nombre de cada corpus y de cada gramática de los que el servicio ha extraído la palabra. Si ha modificado o añadido la palabra directamente, el campo incluye la serie `user`.
-   Un campo `count` que indica el número de veces que se encuentra la palabra en todos los corpus y gramáticas. Por ejemplo, si la palabra aparece cinco veces en un corpus y siete veces en otro, su recuento es `12`.

    Si añade una palabra personalizada a un modelo antes de la añada un corpus o una gramática, el recuento empieza en `1`. Si la palabra se añade primero desde un corpus o una gramática y luego se modifica, el recuento solo refleja el número de veces que se encuentra en los corpus y en las gramáticas.

    Para los modelos personalizados que se han creado antes de la introducción del campo `count`, el campo siempre permanece en `0`. Para actualizar el campo para dichos modelos, añada de nuevo los corpus y las gramáticas del modelo e incluya el parámetro `allow_overwrite` con la solicitud.
    {: note}

Si el servicio descubre uno o varios problemas con la definición de una palabra personalizada, la salida incluye un campo `error`. El campo proporciona una matriz que muestra cada problema hallado en la definición y un mensaje que describe el problema.

Por ejemplo, se puede producir un error si añade una palabra personalizada con un campo `sounds_like` no válido, uno que infrinja una de las reglas para añadir una pronunciación. No puede entrenar un modelo personalizado cuyo recurso de palabras incluya una palabra con un error. Debe corregir o suprimir la palabra para poder entrenar el modelo.

### Solicitudes y respuestas de ejemplo
{: #listExample-words}

En el ejemplo siguiente se muestran todas las palabras, independientemente del tipo, del modelo personalizado con el ID de personalización especificado. Las palabras se muestran en el orden de clasificación predeterminado, es decir, en orden alfabético ascendente.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
```
{: pre}

El recurso de palabras del modelo contiene cuatro palabras. La primera palabra la ha añadido directamente el usuario, pero su campo `sounds_like` contiene un error: el campo no puede contener números. Las demás palabras han sido añadidas por el usuario o tanto por el usuario como por corpus.

```javascript
{
  "words": [
    {
      "word": "75.00",
      "sounds_like": ["75 dollars"],
      "display_as": "75.00",
      "count": 1,
      "source": ["user"],
      "error": [{"75 dollars": "Numbers are not allowed in sounds_like. You can try for example 'seventy five dollars'."}]
    },
    {
      "word": "HHonors",
      "sounds_like": [
        "hilton honors",
        "H. honors"
      ],
      "display_as": "HHonors",
      "count": 1,
      "source": [
        "corpus1",
        "user"
      ]
    },
    {
      "word": "IEEE",
      "sounds_like": ["I. triple E."],
      "display_as": "IEEE",
      "count": 3,
      "source": [
        "corpus1",
        "corpus2",
        "user"
      ]
    },
    {
      "word": "tomato",
      "sounds_like": [
        "tomatoh",
        "tomayto"
      ],
      "display_as": "tomato",
      "count": 1,
      "source": ["user"]
    }
  ]
}
```
{: codeblock}

En el ejemplo siguiente se muestra información sobre la palabra `NCAA` del recurso de palabras del modelo especificado:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
```
{: pre}

El usuario ha añadido la palabra inicialmente. Luego el servicio ha encontrado la palabra dos veces en `corpus3`.

```javascript
{
  "word": "NCAA",
  "sounds_like": [
    "N. C. A. A.",
    "N. C. double A."
  ],
  "display_as": "NCAA",
  "count": 3,
  "source": [
    "corpus3",
    "user"
  ]
}
```
{: codeblock}

## Supresión de una palabra de un modelo de lenguaje personalizado
{: #deleteWord}

Utilice el método `DELETE /v1/customizations/{customization_id}/words/{word_name}` para suprimir una palabra de un modelo de lenguaje personalizado. Utilice el método para eliminar palabras que se han añadido por error, por ejemplo de un corpus con datos erróneos.

Puede eliminar cualquier palabra que haya añadido al recurso de palabras del modelo personalizado mediante cualquier método. Sin embargo, no puede suprimir una palabra del vocabulario base del servicio. Cuando se suprime una palabra de un modelo personalizado, solo se suprime la pronunciación personalizada de la palabra; la palabra permanece en el vocabulario base.

La eliminación de una palabra de un modelo personalizado no afecta al modelo hasta que se vuelve a entrenar mediante el método `POST /v1/customizations/{customization_id}/train`. Si el modelo se ha entrenado previamente con la palabra, el modelo sigue aplicando la palabra al reconocimiento de voz incluso después de suprimir la palabra de su recurso de palabras. Debe volver a entrenar el modelo para que refleje la supresión.

### Solicitud de ejemplo
{: #deleteExample-word}

En el ejemplo siguiente se suprime la palabra `IEEE` del modelo personalizado con el ID de personalización especificado:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IEEE"
```
{: pre}
