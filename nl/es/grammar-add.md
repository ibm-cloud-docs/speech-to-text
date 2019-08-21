---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-24"

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

# Adición de una gramática a un modelo de lenguaje personalizado
{: #grammarAdd}

Para poder utilizar una gramática para el reconocimiento de voz, primero debe utilizar la interfaz de personalización para añadir la gramática a un modelo de lenguaje personalizado. Los pasos para añadir una gramática a un modelo de lenguaje personalizado son paralelos a los utilizados para añadir un corpus o palabras personalizadas:
{: shortdesc}

1.  [Cree un modelo de lenguaje personalizado](#createModel-grammar). Puede crear un modelo personalizado nuevo o utilizar un modelo existente.
1.  [Añada una gramática al modelo de lenguaje personalizado](#addGrammar). El servicio valida la gramática para garantizar su corrección.
1.  [Validar las palabras de la gramática](#validateGrammar). Debe verificar la exactitud de las pronunciaciones parecidas para cualquier palabra no definida en el vocabulario (OOV) que se reconozca en la gramática.
1.  [Entrene el modelo de lenguaje personalizado](#trainGrammar). El servicio prepara el modelo personalizado y la gramática para que se utilice en el reconocimiento de voz.
1.  Ahora puede utilizar el modelo personalizado y la gramática en las solicitudes de reconocimiento de voz. Para obtener más información, consulte [Utilización de una gramática para el reconocimiento de voz](/docs/services/speech-to-text?topic=speech-to-text-grammarUse).

Estos pasos son iterativos. Puede añadir gramáticas, así como corpus y palabras personalizadas, a un modelo de lenguaje personalizado con la frecuencia necesaria. Debe entrenar el modelo personalizado con cualquier nuevo recurso de datos (gramáticas, corpus o palabras personalizadas) que añada. Cuando se utiliza para el reconocimiento de voz, un modelo personalizado refleja los datos con los que se ha entrenado por última vez.

La característica de gramáticas es una funcionalidad beta. Puede utilizar gramáticas con cualquier idioma que dé soporte a la personalización del modelo de lenguaje. La personalización del modelo de lenguaje está disponible para la mayoría de idiomas. Para obtener más información, consulte [Soporte de idiomas para la personalización](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport).
{: note}

## Creación de un modelo de lenguaje personalizado
{: #createModel-grammar}

Para utilizar una gramática con el reconocimiento de voz, debe añadirlo a un modelo de lenguaje personalizado. Puede utilizar un modelo de lenguaje personalizado existente, o bien puede crear un nuevo modelo personalizado con el método `POST /v1/customizations`. Para obtener información sobre cómo crear un nuevo modelo personalizado, consulte [Creación de un modelo de lenguaje personalizado](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language).

Un modelo de lenguaje personalizado puede contener palabras del corpus y palabras personalizadas, así como gramáticas. Durante el reconocimiento de voz, puede utilizar el modelo personalizado con o sin gramáticas. Sin embargo, cuando se utiliza una gramática, el servicio solo reconoce las palabras de la gramática especificada.

## Adición de una gramática al modelo de lenguaje personalizado
{: #addGrammar}

Utilice el método `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` para añadir un archivo de gramática a un modelo personalizado.

-   Especifique el ID de personalización del modelo de lenguaje personalizado con el parámetro de vía de acceso `customization_id`.
-   Especifique un nombre para la gramática con el parámetro de vía de acceso `grammar_name`. Utilice un nombre en el idioma del modelo personalizado que refleje el contenido de la gramática.
    -   El nombre puede tener un máximo de 128 caracteres.
    -   No utilice caracteres que se tengan que codificar en URL. Por ejemplo, en el nombre no utilice espacios, barras inclinadas, barras inclinadas invertidas, ampersands, comillas dobles, signos más, signos de igual, interrogaciones, etc. (El servicio no impide el uso de estos caracteres. Pero como se tienen que codificar en URL siempre que se utilicen, se desaconseja utilizarlos.)
    -   No utilice el nombre de una gramática o corpus que ya se haya añadido al modelo personalizado.
    -   No utilice el nombre `user`, que está reservado por el servicio para indicar las palabras personalizadas que el usuario añade o modifica.
    -   No utilice el nombre `base_lm` ni `default_lm`. Ambos nombres se reservan para un posterior uso por parte del servicio.
-   Utilice la cabecera de solicitud `Content-Type` para especificar el formato de la gramática:
    -   `application/srgs` para una gramática ABNF
    -   `application/srgs+xml` para una gramática XML
-   Pase el archivo que contiene la gramática como el cuerpo de la solicitud.

En el ejemplo siguiente se añade el archivo de gramática denominado `confirm.abnf` al modelo personalizado con el ID especificado. En el ejemplo se asigna a la gramática el nombre `confirm-abnf`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/srgs"
--data-binary @confirm.abnf
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

El método también acepta un parámetro de consulta opcional `allow_overwrite` que puede incluir para sobrescribir una gramática existente con el mismo nombre. Utilice el parámetro si tiene que actualizar una gramática después de añadirla a un modelo.

Este método es asíncrono. El servicio puede tardar unos segundos en analizar la gramática, en función del tamaño de la gramática y de la carga actual del servicio. Para obtener información sobre cómo consultar el estado de una gramática, consulte [Supervisión de la solicitud de adición de gramática](#monitorGrammar).

Puede añadir el número de gramáticas que desee a un modelo personalizado llamando al método una vez para cada archivo de gramática. La adición de una gramática debe haber finalizado para poder añadir otra gramática.

### Supervisión de la solicitud de adición de gramática
{: #monitorGrammar}

El servicio devuelve un código de respuesta 201 si la gramática es válida. Luego procesa de forma asíncrona el contenido de la gramática y extrae automáticamente las palabras nuevas que la gramática puede reconocer. No puede enviar solicitudes para añadir más recursos de datos a un modelo personalizado, ni entrenar el modelo, hasta que el servicio termine de analizar la gramática para la solicitud actual.

Para determinar el estado del análisis, utilice el método `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` para sondear el estado de la gramática. El método acepta el ID del modelo personalizado y el nombre de la gramática. En el ejemplo siguiente se consulta el estado de la gramática denominada `confirm-abnf`.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

La respuesta incluye el estado de la gramática:

```javascript
{
  "name": "confirm-abnf",
  "out_of_vocabulary_words": 0,
  "status": "being_processed"
}
```
{: codeblock}

El campo `status` tiene uno de los valores siguientes:

-   `analyzed` indica que el servicio ha analizado correctamente la gramática.
-   `being_processed` indica que el servicio todavía está analizando la gramática.
-   `undetermined` indica que el servicio ha encontrado un error al procesar la gramática.

Utilice un bucle para consultar el estado de la gramática cada 10 segundos hasta que esté en el estado `analyzed`. Para obtener más información sobre cómo consultar el estado de una gramática, consulte [Listado de gramáticas para un modelo de lenguaje personalizado](/docs/services/speech-to-text?topic=speech-to-text-manageGrammars#listGrammars).

### Gestión de errores al añadir una gramática
{: #handleFailures}

Si el análisis de una gramática falla, el servicio establece el estado de la gramática en `undetermined` e incluye un campo `error` que describe la anomalía con el estado de la gramática. También puede utilizar el método `GET /v1/customizations/{customization_id}` para consultar el estado del modelo personalizado. Si falla la adición de una gramática, la salida incluye un mensaje de error como el siguiente:

```javascript
{
  . . .
  "error": "{\"code\":500, \"code_description\":\"Internal Server Error\",
\". . . Cannot compile grammar . . .\"},
  . . .
}
```
{: codeblock}

El estado del modelo personalizado no se ve afectado por el error, pero la gramática no se puede utilizar con el modelo. Puede solucionar el problema corrigiendo el archivo de gramática y repitiendo la solicitud para añadirla al modelo personalizado. Establezca el parámetro `allow_overwrite` en `true` para sobrescribir la versión del archivo de gramática que ha fallado.

También puede suprimir la gramática del modelo personalizado por el momento y luego corregir y volver a añadir el archivo de gramática en el futuro. Para obtener más información sobre cómo suprimir una gramática, consulte [Supresión de una gramática de un modelo de lenguaje personalizado](/docs/services/speech-to-text?topic=speech-to-text-manageGrammars#deleteGrammar).

## Validación de las palabras de la gramática
{: #validateGrammar}

Cuando se añade un archivo de gramática a un modelo de lenguaje personalizado, el servicio analiza la gramática para determinar si la gramática reconoce cualquier palabra que no forme parte del vocabulario base del servicio. Estas palabras reciben el nombre de palabras no definidas en el vocabulario (OOV). El servicio añade palabras OOV al recurso de palabras del modelo personalizado. El objetivo del recurso de palabras es definir palabras que no aparecen en el vocabulario del servicio.

Las definiciones del recurso de palabras indican al servicio cómo transcribe las palabras OOV. La información incluye un campo `sounds_like`, que indica al servicio cómo se pronuncia la palabra, un campo `display_as`, que indica al servicio cómo mostrar la palabra, y un campo `origen`, que indica cómo se ha añadido la palabra al modelo personalizado. Para obtener más información sobre el recurso de palabras y las palabras OOV, [El recurso de palabras](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordsResource).

Después de añadir una gramática a un modelo personalizado, se recomienda examinar las palabras OOV del recurso de palabras del modelo para verificar su pronunciación. No todas las gramáticas tienen palabras OOV, pero suele resultar aconsejable verificar el recurso de palabras. Para comprobar las palabras del modelo personalizado después de añadir una gramática, utilice los métodos siguientes:

-   Utilice el método `GET /v1/customizations/{customization_id}/words` para ver una lista de las palabras de un modelo personalizado. Pase el valor `grammars` con el parámetro `word_type` del método para ver una lista solo de las palabras que se han añadido de las gramáticas.
-   Utilice el método `GET /v1/customizations/{customization_id}/words/{word_name}` para ver una palabra individual de un modelo.

Verifique que la pronunciación de las palabras sea correcta y precisa. Compruebe si hay errores tipográficos o de otro tipo en las propias palabras. Para obtener más información sobre la validación y la corrección de las palabras en un modelo personalizado, consulte [Validación de un recurso de palabras](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel).

## Entrenamiento del modelo de lenguaje personalizado
{: #trainGrammar}

El paso final antes de poder utilizar una gramática con un modelo de lenguaje personalizado consiste en entrenar el modelo. Siga el mismo proceso que utiliza para entrenar un modelo personalizado con un nuevo corpus o nuevas palabras personalizadas. Cuando se entrena el modelo, se compila la gramática para utilizarla durante el reconocimiento de voz. El servicio procesa la gramática y la pasa de su formato basado en texto original a un formato de tiempo de ejecución binario para el reconocimiento de voz. No se puede utilizar la gramática hasta que se entrene el modelo.

Utilice el método `POST /v1/customizations/{customization_id}/train` para entrenar un modelo personalizado. Debe pasar al método el ID de personalización del modelo que desea entrenar, como en el ejemplo siguiente.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

El método de entrenamiento es asíncrono. Normalmente, el entrenamiento tarda unos segundos en completarse. Pero puede tardar más dependiendo del tamaño y de la complejidad de la gramática y de la carga actual del servicio. Para obtener información sobre cómo consultar el estado de una operación de entrenamiento, consulte [Supervisión de la solicitud de entrenamiento](#monitorTraining-grammar).

### Supervisión de la solicitud de entrenamiento
{: #monitorTraining-grammar}

El servicio devuelve el código de respuesta 200 si el proceso de entrenamiento se inicia correctamente. El servicio no acepta otras solicitudes de entrenamiento, ni solicitudes para añadir nuevas gramáticas, corpus o palabras al modelo personalizado, hasta que finaliza la solicitud de entrenamiento actual.

Para determinar el estado de una solicitud de entrenamiento, utilice el método `GET /v1/customizations/{customization_id}` para sondear el estado del modelo. El método acepta el ID de personalización del modelo y devuelve información como la que se indica a continuación sobre el modelo:

```
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2018-06-01T18:42:25.324Z",
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

El campo `status` tiene el valor `training` mientras se está entrenando el modelo. Utilice un bucle para consultar el estado del modelo cada 10 segundos hasta que esté en el estado `available`. Para obtener más información sobre la supervisión de una solicitud de entrenamiento y otros valores de estado posibles, consulte [Supervisión de la solicitud de entrenamiento del modelo](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#monitorTraining-language).
