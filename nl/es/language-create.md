---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-11"

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

# Creación de un modelo de lenguaje personalizado
{: #languageCreate}

Siga estos pasos para crear un modelo de lenguaje personalizado para el servicio {{site.data.keyword.speechtotextshort}}:
{: shortdesc}

1.  [Cree un modelo de lenguaje personalizado](#createModel-language). Puede crear varios modelos personalizados para los mismos dominios o para dominios diferentes. El proceso es el mismo para cualquier modelo que cree. Sin embargo, solo puede especificar un único modelo personalizado a la vez con una solicitud de reconocimiento.
1.  [Añada un corpus al modelo de lenguaje personalizado](#addCorpus). Un corpus es un documento de texto sin formato que utiliza terminología del dominio en el contexto. El servicio crea un vocabulario para un modelo personalizado mediante la extracción de términos del corpus que no existen en su vocabulario base. Puede añadir varios corpus a un modelo personalizado.
1.  [Añada palabras al modelo de lenguaje personalizado](#addWords). También puede añadir palabras personalizadas a un modelo individualmente. Además, puede utilizar los mismos métodos para modificar las palabras personalizadas que se extraen de los corpus. Los métodos le permiten especificar la pronunciación de palabras y cómo se visualizan las palabras en una transcripción de voz.
1.  [Entrene el modelo de lenguaje personalizado](#trainModel-language). Una vez que haya añadido palabras al modelo personalizado, debe entrenar el modelo con las palabras. El entrenamiento prepara el modelo personalizado para que se utilice en el reconocimiento de voz. El modelo no utiliza palabras nuevas o modificadas hasta que lo entrena.
1.  Después de entrenar el modelo personalizado, puede utilizarlo con las solicitudes de reconocimiento. Si el audio que se pasa para la transcripción contiene palabras específicas del dominio que están definidas en el modelo personalizado, los resultados de la solicitud reflejan el vocabulario mejorado del modelo. Para obtener más información, consulte el apartado sobre [Utilización de un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-use.html).

Los pasos para la creación de un modelo de lenguaje personalizado son iterativos. Puede añadir corpus, añadir palabras y entrenar o volver a entrenar un modelo tantas veces como sea necesario.

También puede añadir gramáticas a un modelo de lenguaje personalizado. Las gramáticas restringen la respuesta del servicio a aquellas palabras que la gramática reconoce. Para obtener más información, consulte [Utilización de gramáticas con modelos de lenguaje personalizados](/docs/services/speech-to-text/grammar.html).

La personalización del modelo de lenguaje está disponible para la mayoría de idiomas. Para obtener más información, consulte [Soporte de idiomas para la personalización](/docs/services/speech-to-text/custom.html#languageSupport).
{: note}

## Creación de un modelo de lenguaje personalizado
{: #createModel-language}

Para crear un nuevo modelo de lenguaje personalizado se utiliza el método `POST /v1/customizations`. Puede crear tantos modelos de lenguaje personalizados como desee, pero solo puede utilizar un modelo a la vez con una solicitud de reconocimiento de voz. El método acepta un objeto JSON que define los atributos del nuevo modelo personalizado como el cuerpo de la solicitud.

<table>
  <caption>Tabla 1. Atributos de un nuevo modelo de lenguaje personalizado</caption>
  <tr>
    <th style="width:20%; text-align:left">Parámetro</th>
    <th style="width:12%; text-align:center">Tipo de datos</th>
    <th style="text-align:left">Descripción</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>Obligatorio</em></td>
    <td style="text-align:center">Serie</td>
    <td>
      Un nombre definido por el usuario para el nuevo modelo personalizado. Utilice un nombre que describa el dominio del modelo personalizado,  como por ejemplo <code>Modelo personalizado médico</code> o <code>Modelo personalizado legal</code>. Utilice un nombre que sea exclusivo entre todos sus modelos de lenguaje personalizado.
      Utilice un nombre en el idioma del modelo personalizado.
    </td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>Obligatorio</em></td>
    <td style="text-align:center">Serie</td>
    <td>
      El nombre del modelo de lenguaje que se va a personalizar mediante el nuevo modelo personalizado. Especifique uno de los modelos de lenguaje soportados que se devuelven mediante el método <code>GET /v1/models</code>. El nuevo modelo solo se puede utilizar con el modelo base que personaliza.
    </td>
  </tr>
  <tr>
    <td><code>dialect</code><br/><em>Opcional</em></td>
    <td style="text-align:center">Serie</td>
    <td>
      El dialecto del idioma especificado que se va a utilizar con el modelo personalizado. De forma predeterminada, el dialecto coincide con el idioma del modelo base; por ejemplo, el dialecto es <code>en-US</code> para los modelos de idioma en inglés de EE. UU., <code>en-US_BroadbandModel</code> o
      <code>en-US_NarrowbandModel</code>.<br/></br>
      El parámetro solo tiene sentido para los modelos en español, para los que el servicio crea un modelo personalizado que se adapta al habla del dialecto indicado:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          <code>es-ES</code> para español castellano (valor predeterminado)
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>es-LA</code> para español de Latinoamérica
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>es-US</code> para español de Norteamérica (México)
        </li>
      </ul>
      Si especifica un dialecto, debe ser válido para el modelo base.
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

En el ejemplo siguiente se crea un nuevo modelo de lenguaje personalizado denominado `Modelo de ejemplo`. El modelo se crea para el modelo base `en-US_BroadbandModel` y su descripción es `Modelo de lenguaje personalizado de ejemplo`. La cabecera `Content-Type` especifica que se pasan datos JSON al método.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Modelo de ejemplo\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Modelo de lenguaje personalizado de ejemplo\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations"
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

## Adición de un corpus al modelo de lenguaje personalizado
{: #addCorpus}

Una vez que se crea el modelo de lenguaje personalizado, el paso siguiente consiste en añadir datos (palabras específicas del dominio) al modelo. El método recomendado para cumplimentar un modelo personalizado con palabras nuevas consiste en añadir uno o varios corpus.

Un corpus es un archivo de texto sin formato que contiene frases de ejemplo del dominio. El servicio analiza el contenido de un archivo de corpus y extrae las palabras que no están en su vocabulario base. Estas palabras reciben el nombre de palabras no definidas en el vocabulario (OOV).

-   Para obtener más información sobre el uso de corpus, consulte [Cómo trabajar con corpus](/docs/services/speech-to-text/language-resource.html#workingCorpora).
-   Para obtener más información sobre la forma en que el servicio añade corpus a un modelo, consulte [¿Qué ocurre cuando añado un archivo de corpus?](/docs/services/speech-to-text/language-resource.html#parseCorpus)

Al proporcionar frases que incluyen palabras nuevas, los corpus permiten al servicio aprender las palabras en su contexto. Luego puede aumentar o modificar las palabras del modelo de forma individual. El entrenamiento de un modelo solo con palabras individuales, en contraposición con las palabras que añade el corpus, requiere más tiempo y puede producir resultados menos efectivos.
{: tip}

Utilice el método `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` para añadir un corpus a un modelo personalizado:

-   Especifique el ID de personalización del modelo personalizado con el parámetro de vía de acceso `customization_id`.
-   Especifique un nombre para el corpus con el parámetro de vía de acceso `corpus_name`. Utilice un nombre en el idioma del modelo personalizado que refleje el contenido del corpus.
    -   El nombre puede tener un máximo de 128 caracteres.
    -   No incluya espacios, `/` (barras inclinadas) ni `\` (barras inclinadas invertidas) en el nombre.
    -   No utilice el nombre de un corpus que ya se haya añadido al modelo personalizado.
    -   No utilice el nombre `user`, que está reservado por el servicio para indicar las palabras personalizadas que el usuario añade o modifica.
-   Pase el archivo de texto del corpus como el cuerpo de la solicitud.

Puede añadir un máximo de 90 mil palabras OOV y 10 millones de palabras en total de todas las fuentes combinadas. Esto incluye las palabras de los corpus y de las gramáticas, y las palabras que añade directamente. Para obtener más información, consulte [¿Cuántos datos necesito?](/docs/services/speech-to-text/language-resource.html#wordsResourceAmount)
{: note}

En el ejemplo siguiente se añade el archivo de texto de corpus `healthcare.txt` al modelo personalizado con el ID especificado. En el ejemplo se utiliza el corpus `healthcare`.

```bash
curl -X POST -u "apikey:{apikey}"
--data-binary @healthcare.txt
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/healthcare"
```
{: pre}

El método también acepta un parámetro de consulta opcional `allow_overwrite` que sobrescribe un corpus existente para un modelo personalizado. Utilice el parámetro si tiene que actualizar un archivo de corpus después de añadirlo a un modelo.

Este método es asíncrono. Puede tardar uno o dos minutos en completarse. La duración de la operación depende del número total de palabras del corpus, del número de palabras nuevas que el servicio encuentra en el corpus y de la carga actual del servicio. Para obtener más información sobre cómo comprobar el estado de un corpus, consulte [Supervisión de la solicitud de añadir un corpus](#monitorCorpus).

Puede añadir el número de corpus que desee a un modelo personalizado llamando al método una vez para cada archivo de texto de corpus. La adición de un corpus debe haber finalizado para poder añadir otro. Después de añadir un corpus a un modelo personalizado, examine las nuevas palabras personalizadas para comprobar si hay errores tipográficos y de otro tipo. Para obtener más información, consulte [Validación de un recurso de palabras](/docs/services/speech-to-text/language-resource.html#validateModel).

### Supervisión de la solicitud de añadir un corpus
{: #monitorCorpus}

El servicio devuelve un código de respuesta 201 si el corpus es válido. Luego procesa de forma asíncrona el contenido del corpus y extrae automáticamente las palabras nuevas. No puede enviar solicitudes para añadir datos al modelo personalizado ni para entrenar el modelo hasta que el servicio termine de analizar el corpus para la solicitud actual.

Para determinar el estado del análisis, utilice el método `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` para sondear el estado del corpus. El método acepta el ID del modelo y el nombre del corpus, tal como se muestra en el ejemplo siguiente:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

La respuesta incluye el estado del corpus:

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

El campo `status` tiene uno de los valores siguientes:

-   `analyzed` indica que el servicio ha analizado correctamente el corpus.
-   `being_processed` indica que el servicio todavía está analizando el corpus.
-   `undetermined` indica que el servicio ha encontrado un error al procesar el corpus.

Utilice un bucle para consultar el estado del corpus cada 10 segundos hasta que esté en el estado `analyzed`. Para obtener más información sobre cómo consultar el estado de los corpus de un modelo, consulte [Listado de corpus para un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-corpora.html#listCorpora).

## Adición de palabras al modelo de lenguaje personalizado
{: #addWords}

Aunque la adición de corpus es el método recomendado para añadir palabras a un modelo de lenguaje personalizado, también puede añadir directamente palabras personalizados individuales al modelo. El servicio añade las palabras personalizadas al modelo personalizado tal y como lo hace con las palabras OOV que descubre en los corpus.

Si solo tiene una o unas pocas palabras que añadir a un modelo, el uso de corpus para añadir las palabras podría no resultar práctico o incluso viable. La forma más sencilla de hacerlo es mediante la adición de una palabra con solo su ortografía. Pero también puede proporcionar varias pronunciaciones para la palabra e indicar cómo se debe mostrar.

-   Para obtener más información sobre cómo añadir palabras directamente, consulte [Cómo trabajar con palabras personalizadas](/docs/services/speech-to-text/language-resource.html#workingWords).
-   Para obtener más información acerca de la forma en que el servicio añade palabras personalizadas a un modelo, consulte [¿Qué ocurre cuando añado o modifico una palabra personalizada?](/docs/services/speech-to-text/language-resource.html#parseWord)

Puede utilizar los métodos siguientes para añadir palabras a un modelo personalizado:

-   El método `POST /v1/customizations/{customization_id}/words` añade varias palabras a la vez. Debe pasar un objeto JSON que contenga información sobre cada palabra con el cuerpo de la solicitud. En el ejemplo siguiente se añaden dos palabras personalizadas, `HHonors` e `IEEE`, al modelo personalizado con el ID especificado. La cabecera `Content-Type` especifica que se pasan datos JSON al método.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"words\": [
      {\"word\": \"HHonors\", \"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"},
      {\"word\": \"IEEE\", \"sounds_like\": [\"I. triple E.\"]}]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    También puede añadir las palabras desde un archivo. Por ejemplo, el archivo `words.json` define las mismas dos palabras personalizadas.

    ```
    {"words":
      [
        {"word": "HHonors", "sounds_like": ["hilton honors", "H. honors"], "display_as": "HHonors"},
        {"word": "IEEE", "sounds_like": ["I. triple E."]}
      ]
    }
    ```
    {: codeblock}

    El mandato siguiente añade las palabras del archivo:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data-binary @words.json
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    Este método es asíncrono. El tiempo que tarda en completarse depende del número de palabras que añade y de la carga actual del servicio. Para obtener más información sobre cómo comprobar el estado de la operación, consulte [Supervisión de la solicitud de añadir palabras](#monitorWords).
-   El método `PUT /v1/customizations/{customization_id}/words/{word_name}` añade palabras individuales. Debe pasar un objeto JSON que contenga información sobre la palabra. En el ejemplo siguiente se añade la palabra `NCAA` al modelo con el ID especificado. Como siempre, la cabecera `Content-Type` especifica que se pasan datos JSON al método.

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

    Este método es síncrono. El servicio devuelve un código de respuesta que muestra de forma inmediata si se ha ejecutado correctamente o no.

Al igual que sucede con la adición de corpus, examine las nuevas palabras personalizadas para comprobar si hay errores tipográficos y de otro tipo. Esta comprobación es especialmente importante cuando se añaden varias palabras a la vez. Para obtener más información, consulte [Validación de un recurso de palabras](/docs/services/speech-to-text/language-resource.html#validateModel).

### Supervisión de la solicitud de añadir palabras
{: #monitorWords}

Cuando se utiliza el método `POST /v1/customizations/{customization_id}/words`, el servicio devuelve el código de respuesta 201 si los datos de entrada son válidos. Luego procesa de forma asíncrona las palabras para añadirlas al modelo. La operación suele ser más rápida que la adición de un corpus o que el entrenamiento de un modelo, pero su duración depende del número de palabras nuevas que se añaden y de la carga actual del servicio. No puede enviar solicitudes para añadir datos al modelo personalizado ni para entrenar el modelo hasta que el servicio complete la solicitud de añadir palabras nuevas.

Para determinar el estado de la solicitud. utilice el método `GET /v1/customizations/{customization_id}` para sondear el estado del modelo. El método acepta el ID de personalización del modelo y devuelve información que incluye el estado del modelo, como en el ejemplo siguiente:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Modelo de ejemplo",
  "description": "Modelo de lenguaje personalizado de ejemplo",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

El campo `status` muestra el estado actual del modelo. Mientras el servicio está procesando palabras nuevas, el estado es `pending`. Utilice un bucle para consultar el estado cada 10 segundos hasta que esté en el estado `ready`, que indica que la operación se ha completado. Para obtener información sobre los valores posibles de `status`, consulte [Supervisión de la solicitud de entrenamiento del modelo](#monitorTraining-language).

### Modificación de las palabras de un modelo personalizado
{: #modifyWord}

También puede utilizar los métodos `POST /v1/customizations/{customization_id}/words` y `PUT /v1/customizations/{customization_id}/words/{word_name}` para modificar o aumentar una palabra en un modelo personalizado. Es posible que tenga que utilizar los métodos para corregir un error tipográfico u otro error que se haya cometido al añadir una palabra al modelo. También es posible que tenga que añadir definiciones de pronunciaciones para una palabra existente.

Los métodos para modificar la definición de una palabra existente se utilizan del mismo modo que los utilizados para añadir una palabra. Los nuevos datos que se proporcionan para la palabra sobrescriben la definición existente de la palabra.

## Entrenamiento del modelo de lenguaje personalizado
{: #trainModel-language}

Una vez que haya cumplimentado un modelo de lenguaje personalizado con palabras nuevas (mediante la adición de corpus, de gramáticas y de palabras directamente), debe entrenar el modelo con los nuevos datos. El proceso de entrenamiento prepara el modelo personalizado para que utilice los datos en el reconocimiento de voz. El modelo no utiliza las palabras que se añaden a través de cualquier método hasta que lo entrena con los datos.

Utilice el método `POST /v1/customizations/{customization_id}/train` para entrenar un modelo personalizado. Debe pasar al método el ID de personalización del modelo que desea entrenar, como en el ejemplo siguiente:

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

Puede utilizar el parámetro de consulta opcional `word_type_to_add` para especificar las palabras con las que se va a entrenar el modelo personalizado:

-   Especifique `all` o bien omita el parámetro para entrenar el modelo con todas sus palabras, independientemente de su origen.
-   Especifique `user` para entrenar el modelo solo con las palabras que el usuario ha añadido o modificado, sin tener en cuenta las palabras que solo se han extraído de corpus o de gramáticas.

    Esta opción resulta útil si añade corpus con datos incorrectos, como palabras que contienen errores tipográficos. Antes de entrenar el modelo con este tipo de datos, puede utilizar el parámetro de consulta `word_type` del método `GET /v1/customizations/{customization_id}/words` para revisar las palabras que se extraen de corpus y de gramáticas. Para obtener más información, consulte [Listado de las palabras de un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-words.html#listWords).

Además, puede utilizar el parámetro de consulta opcional `customization_weight`. El parámetro especifica la ponderación relativa que se otorga a las palabras del modelo personalizado en comparación con las palabras del vocabulario base cuando se utiliza el modelo personalizado para el reconocimiento de voz. También puede especificar una ponderación de personalización con cualquier solicitud de reconocimiento que utilice el modelo personalizado. Para obtener más información, consulte [Utilización de ponderaciones de personalización](/docs/services/speech-to-text/language-use.html#weight).

Este método es asíncrono. La formación puede tardar unos minutos en completarse en función del número de palabras nuevas con las que se está entrenando el modelo y de la carga actual del servicio. Para obtener más información sobre cómo consultar el estado de una operación de entrenamiento, consulte [Supervisión de la solicitud de entrenamiento del modelo](#monitorTraining-language).

### Supervisión de la solicitud de entrenamiento del modelo
{: #monitorTraining-language}

El servicio devuelve el código de respuesta 200 si inicia correctamente el proceso de entrenamiento. El servicio no acepta otras solicitudes de entrenamiento, ni solicitudes para añadir nuevos corpus, gramáticas o palabras, hasta que finaliza la solicitud actual.

Para determinar el estado de una solicitud de entrenamiento, utilice el método `GET /v1/customizations/{customization_id}` para sondear el estado del modelo. El método acepta el ID de personalización del modelo y devuelve información sobre el modelo.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Modelo de ejemplo",
  "description": "Modelo de lenguaje personalizado de ejemplo",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

La respuesta incluye los campos `status` y `progress` que muestran el estado del modelo personalizado. El significado del campo `progress` depende del estado del modelo. El campo `status` puede tener uno de los valores siguientes:

-   `pending` indica que el modelo se ha creado pero está a la espera de que se añadan datos de entrenamiento o que el servicio termine de analizar los datos que se han añadido. El campo `progress` es `0`.
-   `ready` indica que el modelo está listo para ser entrenado. El campo `progress` es `0`.
-   `training` indica que el modelo se está entrenando. El campo `progress` pasa de `0` a `100` cuando termina el entrenamiento. <!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available` indica que el modelo está entrenado y listo para su uso. El campo `progress` es `100`.
-   `upgrading` indica que el modelo se está actualizando. El campo `progress` es `0`.
-   `failed` indica que el entrenamiento del modelo ha fallado. El campo `progress` es `0`.

Utilice un bucle para consultar el estado cada 10 segundos hasta que esté en el estado `available`. Para obtener más información sobre cómo comprobar el estado de un modelo personalizado, consulte [Listado de modelos de lenguaje personalizado](/docs/services/speech-to-text/language-models.html#listModels-language).

### Errores de entrenamiento
{: #failedTraining-language}

El entrenamiento no se puede iniciar si el servicio está gestionando otra solicitud para el modelo de lenguaje personalizado. Por ejemplo, una solicitud de entrenamiento no se puede iniciar si el servicio está

-   Procesando un corpus o una gramática para generar una lista de palabras OOV
-   Procesando palabras personalizadas para validar o generar automáticamente pronunciaciones
-   Gestionando de otra solicitud de entrenamiento

También es posible que el entrenamiento no se inicie por los siguientes motivos:

-   No se han añadido datos de entrenamiento (corpus, gramáticas o palabras) al modelo personalizado desde que se creó o desde que se entrenó por última vez.
-   Una o varias de las palabras añadidos al modelo personalizado tienen pronunciaciones no válidas que debe corregir.

Si el estado del entrenamiento de un modelo personalizado es `failed`, utilice los métodos de la interfaz de personalización para examinar las palabras del modelo y corregir los errores que encuentre. Para obtener más información, consulte [Validación de un recurso de palabras](/docs/services/speech-to-text/language-resource.html#validateModel).

## Scripts de ejemplo
{: #exampleScripts}

Puede utilizar los scripts siguientes para experimentar con los pasos para crear un modelo de lenguaje personalizado:

-   Un script Python llamado <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a>. Para obtener más información, consulte el apartado [Script Python de ejemplo](#pythonScript).
-   Un script shell Bash llamado <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a>. Para obtener más información, consulte el apartado [Script shell de ejemplo](#shellScript).

Los dos scripts proporcionan la misma funcionalidad. Cada script crea un modelo personalizado, añade palabras de un archivo de texto de corpus y añade palabras individuales y múltiples directamente al modelo. El script consulta al modelo para mostrar una lista de las palabras añadidos de un corpus y directamente por el usuario. También entrena el modelo y demuestra el sondeo que se recomienda para supervisar los resultados de las operaciones asíncronas.

Puede utilizar cualquiera de los dos archivos de texto de corpus proporcionados con los scripts, o bien puede probar con sus propios archivos de corpus:

-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/corpus.txt" download="corpus.txt">corpus.txt <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a> es un corpus de 6 KB abreviado que añade a un modelo seis términos relacionados con el cuidado de la salud. Este archivo genera una pequeña cantidad de salida cuando se utiliza con los scripts. Los scripts utilizan este archivo de forma predeterminada.
-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/healthcare.txt" download="healthcare.txt">healthcare.txt <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a> es un corpus más completo de 164 KB que añade a un modelo muchos términos relacionados con el cuidado de la salud. Este archivo genera mucha más salida cuando se utiliza con los scripts.

De forma predeterminada, el nuevo modelo personalizado que crea con cualquiera de los scripts se puede utilizar con las solicitudes de reconocimiento. Los scripts incluyen un paso opcional para suprimir el nuevo modelo personalizado, que puede resultar útil cuando se está experimentando con el proceso. Siga los comentarios de los scripts para habilitar el paso de supresión.

### Script Python de ejemplo
{: #pythonScript}

Siga estos pasos para utilizar el script Python:

1.  Descargue el script Python llamado <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a>.
1.  Descargue los archivos de texto del corpus de ejemplo para utilizarlos con el script. Puede probar con cualquiera de los archivos de texto del corpus o con un archivo de su propia elección. De forma predeterminada, todos los archivos de texto del corpus deben residir en el mismo directorio que el script.
1.  El script utiliza la biblioteca de `requests` de Python para las solicitudes HTTP al servicio. Utilice `pip` o `easy_install` para instalar la biblioteca para que la utilice el script, por ejemplo:

    ```bash
    pip install requests
    ```
    {: pre}

    Para obtener más información acerca de la biblioteca, consulte [pypi.python.org/pypi/requests ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://pypi.python.org/pypi/requests){: new_window}.
1.  Edite el script para sustituir la serie `iam_apikey` de `password` por la clave de API de sus credenciales de servicio de {{site.data.keyword.speechtotextshort}}:

    ```
    password = "iam_apikey"
    ```
    {: codeblock}

1.  Ejecute el script con el mandato siguiente:

    ```bash
    python testSTTcustom.py
    ```
    {: pre}

El script utiliza el siguiente URL predeterminado correspondiente a la ubicación de Dallas: `https://stream.watsonplatform.net`. Si ha creado la instancia de servicio en otra ubicación, modifique las variables `uri` para que utilicen su ubicación. Por ejemplo, utilice `https://gateway-wdc.watsonplatform.net` si la instancia de servicio reside en la ubicación de Washington, DC.
{: note}

### Script shell de ejemplo
{: #shellScript}

Siga estos pasos para utilizar el script shell Bash:

1.  Descargue el script shell llamado <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a>.
1.  Descargue los archivos de texto del corpus de ejemplo para utilizarlos con el script. Puede probar con cualquiera de los archivos de texto del corpus o con un archivo de su propia elección. De forma predeterminada, todos los archivos de texto del corpus deben residir en el mismo directorio que el script.
1.  El script utiliza el mandato `curl` para las solicitudes HTTP al servicio. Si todavía no ha descargado `curl`, puede instalar la versión para su sistema operativo desde [curl.haxx.se ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://curl.haxx.se){: new_window}. Instale la versión que dé soporte al protocolo Secure Sockets Layer (SSL) y asegúrese de incluir el archivo binario instalado en su variable de entorno `PATH`.
1.  Edite el script para sustituir la serie `iam_apikey` de `PASSWORD` por la clave de API de sus credenciales de servicio de {{site.data.keyword.speechtotextshort}}:

    ```
    PASSWORD="iam_apikey"
    ```
    {: codeblock}

1.  Edite el script para sustituir la serie `URL` por el URL correspondiente a la ubicación en la que ha creado la instancia de servicio. El script utiliza el siguiente URL predeterminado correspondiente a la ubicación de Dallas:

    ```
    URL="https://stream.watsonplatform.net/speech-to-text/api/v1"
    ```
    {: codeblock}

1.  Asegúrese de que el script tiene permisos de ejecución y luego ejecute el script con el mandato siguiente:

    ```bash
    ./testSTTcustom.sh
    ```
    {: pre}
