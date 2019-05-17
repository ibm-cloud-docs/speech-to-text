---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-12"

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

# Cómo trabajar con corpus y con palabras personalizadas
{: #corporaWords}

Puede llenar un modelo de lenguaje personalizado con palabras añadiendo corpus o gramáticas al modelo, o bien añadiendo directamente palabras personalizadas:
{: shortdesc}

-   **Corpus:** el método recomendado para cumplimentar un modelo de lenguaje personalizado con palabras es añadir uno o varios corpus al modelo. Cuando se añade un corpus, el servicio analiza el archivo y añade automáticamente las palabras nuevas que encuentra al modelo personalizado. La adición de un corpus a un modelo personalizado permite que el servicio extraiga palabras específicas del dominio en contexto, lo que ayuda a garantizar mejores resultados de la transcripción. Para obtener más información, consulte [Cómo trabajar con corpus](#workingCorpora).
-   **Gramáticas:** puede añadir gramáticas a un modelo personalizado para limitar el reconocimiento de voz a las palabras o frases que se reconocen mediante una gramática. Cuando añade una gramática a un modelo, el servicio añade automáticamente cualquier palabra nueva que encuentra al modelo, tal y como lo hace con el corpus. Para obtener más información, consulte [Utilización de gramáticas con modelos de lenguaje personalizados](/docs/services/speech-to-text/grammar.html).
-   **Palabras individuales:** también puede añadir directamente palabras personalizadas individuales a un modelo. El servicio añade las palabras al modelo tal y como hace con las palabras que descubre de corpus o gramáticas. Cuando añade una palabra directamente, puede especificar varias pronunciaciones e indicar cómo se va a mostrar la palabra. También puede actualizar palabras existentes para modificar o para aumentar las definiciones que se han extraído de corpus o gramáticas. Para obtener más información, consulte [Cómo trabajar con palabras personalizadas](#workingWords).

Independientemente de cómo las añada, el servicio almacena todas las palabras que el usuario añade a un modelo de lenguaje personalizado en el recurso de palabras del modelo.

## El recurso de palabras
{: #wordsResource}

El *recurso de palabras* incluye todas las palabras que el usuario añade de corpus, de gramáticas o directamente. Su finalidad es definir la pronunciación y la ortografía de las palabras que no están en el vocabulario base del servicio. Las definiciones indican al servicio cómo transcribir estas *palabras no definidas en el vocabulario (OOV)*.

El recurso de palabras contiene la siguiente información acerca de cada palabra OOV. El servicio crea las definiciones de las palabras que se extraen de los corpus y de las gramáticas. El usuario especifica las características para las palabras que añade o modifica directamente.

-   `word`: la ortografía de la palabra tal como está en el corpus o en la gramática o tal como la ha añadido.
-   `sounds_like`: la pronunciación de la palabra. Para las palabras extraídas de corpus y de gramáticas, el valor representa la forma en que el servicio cree que se pronuncia la palabra en base a sus reglas de lenguaje. En muchos casos, la pronunciación refleja la ortografía del campo `word`.

    Puede utilizar el campo `sounds_like` para modificar la pronunciación de la palabra. También puede utilizar el campo para especificar varias pronunciaciones para una palabra. Para obtener más información, consulte [Utilización del campo sounds_like](#soundsLike).
-   `display_as`: la ortografía de la palabra que el servicio utiliza en las transcripciones. El campo indica cómo se va a mostrar la palabra. En la mayoría de los casos, la ortografía coincide con el valor del campo `word`.

    Puede utilizar el campo `display_as` para especificar una ortografía distinta para la palabra. Para obtener más información, consulte [Utilización del campo display_as](#displayAs).
-   `source`: cómo se ha añadido la palabra al recurso de palabras. Si el servicio ha extraído la palabra de un corpus o de una gramática, el campo muestra el nombre de dicho recurso. Puesto que el servicio puede encontrar la misma palabra en varios recursos, el campo puede mostrar varios nombres de corpus o de gramática. El campo incluye la serie `user` si añade o modifica la palabra directamente.

Cuando actualice el recurso de palabras de un modelo, debe entrenar el modelo para que los cambios entren en vigor durante la transcripción. Para obtener más información, consulte [Entrenamiento del modelo de lenguaje personalizado](/docs/services/speech-to-text/language-create.html#trainModel-language).

## ¿Cuántos datos necesito?
{: #wordsResourceAmount}

Hay muchos factores que contribuyen a la cantidad de datos que necesita para conseguir un modelo de lenguaje personalizado efectivo. No es posible proporcionar el número exacto de palabras que necesita añadir para cualquier modelo o aplicación personalizado.

Dependiendo del caso de uso, incluso agregar algunas palabras directamente a un modelo personalizado puede mejorar la calidad del modelo. Pero la adición de palabras OOV de un corpus que muestra las palabras en el contexto en el que se utilizan en el audio puede mejorar enormemente la precisión de la transcripción. Para obtener más información, consulte [Cómo trabajar con corpus](#workingCorpora).

El servicio limita el número de palabras que puede añadir a un modelo de lenguaje personalizado:

-   Puede añadir un máximo de 90 mil palabras OOV al recurso de palabras de un modelo personalizado. Esto incluye las palabras de OOV de todas las fuentes (corpus, gramáticas y palabras personalizadas individuales que se añaden directamente).
-   Puede añadir un máximo de 10 millones de palabras a un modelo personalizado de todas las fuentes combinadas. Esta figura incluye todas las palabras, tanto palabras OOV como palabras que ya forman parte del vocabulario básico del servicio, que se incluyen en los corpus o en las gramáticas. En el caso de los corpus, el servicio utiliza estas palabras adicionales para aprender el contexto en el que pueden aparecer palabras OOV, razón por la cual los corpus son un método más efectivo para mejorar la precisión del reconocimiento.

Un recurso de palabras grande puede aumentar la latencia del reconocimiento de voz, pero el efecto exacto es difícil de cuantificar o de predecir. Al igual que sucede con la cantidad de datos que se necesitan para generar un modelo personalizado efectivo, el impacto en el rendimiento de un recurso de palabras grande depende de muchos factores. Pruebe el modelo personalizado con diferentes cantidades de datos para determinar el rendimiento de los modelos y los datos.

## Cómo trabajar con corpus
{: #workingCorpora}

Utilice el método `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` para añadir un corpus a un modelo personalizado. Un corpus es un archivo de texto sin formato que contiene frases de ejemplo del dominio. En el siguiente ejemplo se muestra un corpus abreviado en inglés correspondiente al dominio de sanidad. Normalmente los archivos de corpus son mucho más grandes.

```
Am I at risk for health problems during travel?
Some people are more likely to have health problems when traveling outside the United States.
How Is Coronary Microvascular Disease Treated?
If you're diagnosed with coronary MVD and also have anemia, you may benefit from treatment for that condition.
Anemia is thought to slow the growth of cells needed to repair damaged blood vessels.
What causes autoimmune hepatitis?
A combination of autoimmunity, environmental triggers, and a genetic predisposition can lead to autoimmune hepatitis.
What research is being done for Spinal Cord Injury?
The National Institute of Neurological Disorders and Stroke NINDS conducts spinal cord research in its laboratories at the National Institutes of Health NIH.
NINDS also supports additional research through grants to major research institutions across the country.
Some of the more promising rehabilitation techniques are helping spinal cord injury patients become more mobile.
What is Osteogenesis imperfecta OI?
. . .
```
{: codeblock}

El reconocimiento de voz se basa en algoritmos estadísticos para analizar el audio. Las palabras de un modelo personalizado compiten con las palabras del vocabulario base del servicio, así como con otras palabras del modelo. (Factores como el ruido del audio y el acento de los oradores también afectan a la calidad de la transcripción.)

La precisión de la transcripción puede depender en gran medida de la forma en que se definen las palabras en un modelo y de la forma en que los oradores las pronuncian. Para mejorar la precisión del servicio, utilice corpus para proporcionar tantos ejemplos como sea posible de cómo se utilizan las palabras OOV en el dominio. Repetir las palabras OOV en los corpus puede mejorar la calidad del modelo de lenguaje personalizado. La forma en que se duplican las palabras en los corpus depende de cómo espera que los usuarios las pronuncien en el audio que se va a reconocer. Cuantas más frases que representan el contexto en el que los oradores utilizan palabras del dominio añada, mejor es la precisión del reconocimiento del servicio.

El servicio no aplica un algoritmo de coincidencia de palabras simple. Su transcripción depende del contexto en el que se utilicen las palabras. Cuando analiza un corpus, el servicio incluye información sobre n-gramas (bi-gramas, tri-gramas, etc.) de las frases del corpus del modelo personalizado. Esta información ayuda al servicio a transcribir el audio con mayor precisión, y explica por qué el entrenamiento de un modelo personalizado con los corpus resulta más eficaz que entrenarlo con palabras personalizadas.

Por ejemplo, los contables utilizan un conjunto común de normas y procedimientos que reciben el nombre de principios de contabilidad generalmente aceptados (GAAP). Cuando cree un modelo personalizado para un dominio financiero, especifique frases que utilicen el término GAAP en su contexto. Las frases ayudan al servicio a distinguir entre frases generales, como por ejemplo (el ejemplo se muestra en inglés), "the gap between them is small" y frases específicas del dominio, como "GAAP provides guidelines for measuring and disclosing financial information".

En general, es mejor que en los corpus se utilicen palabras en diferentes contextos, lo que puede mejorar la forma en que el servicio aprende las palabras. Sin embargo, si los usuarios utilizan las palabras solo en un par de contextos, el hecho de mostrar las palabras en otros contextos no mejora la calidad del modelo personalizado. Los oradores nunca utilizan las palabras en esos contextos. Si es probable que los oradores utilicen la misma frase con frecuencia, repetir esa frase en los corpus puede mejorar la calidad del modelo. (En algunos casos, incluso añadir algunas palabras personalizadas directamente a un modelo personalizado puede marcar una diferencia positiva.)

### Preparación de un archivo de texto de corpus
{: #prepareCorpus}

Siga estas directrices para preparar un archivo de texto de corpus:

-   Especifique un archivo de texto sin formato que esté codificado en UTF-8 si contiene caracteres que no sean ASCII. El servicio asume la codificación UTF-8 si encuentra dichos caracteres.

    Asegúrese de conocer la codificación de caracteres de los archivos de texto del corpus. El servicio conserva la codificación que encuentra en los archivos de texto. Debe utilizar dicha codificación cuando trabaje con palabras en el modelo de lenguaje personalizado. Para obtener más información, consulte [Codificación de caracteres](#charEncoding).
    {: important}
-   Utilice de forma coherente las mayúsculas y minúsculas de las palabras en el corpus. El recurso de palabras distingue entre mayúsculas y minúsculas. Combine letras mayúsculas y en minúsculas y utilice mayúsculas solo cuando realmente desee especificarlas.
-   Incluya cada frase del corpus en su propia línea, y termine cada línea con un retorno de carro. La inclusión de varias frases en la misma línea puede degradar la precisión.
-   Añada nombres personales como unidades individuales en líneas separadas. No añada las palabras de un nombre en líneas separadas o como palabras personalizadas individuales, y no incluya varios nombres en la misma línea del corpus. En el ejemplo siguiente se muestra la forma correcta de mejorar la precisión de reconocimiento para tres nombres:

    ```
    Gakuto Kutara
    Sebastian Leifson
    Malcolm Ingersol
    ```
    {: codeblock}

    Incluya información contextual adicional si está disponible, por ejemplo `Doctor Sebastian Leifson` o `President Malcolm Ingersol`. Al igual que sucede con todas las palabras, el hecho de duplicar los nombres varias veces y, si es posible, en diferentes contextos, puede mejorar la precisión del reconocimiento.
-   Tenga cuidado en no cometer errores tipográficos. El servicio da por supuesto que los errores tipográficos son palabras nuevas. A menos que las corrija antes de entrenar el modelo, el servicio las añade al vocabulario del modelo. Recuerde el dicho *Garbage in, garbage out!* (si los datos que entran son basura, los que salen también serán basura).

Una mayor cantidad de frases da lugar a una mayor precisión. Sin embargo, el servicio impone un límite de 10 millones de palabras en total y 90 mil palabras OOV de todas las fuentes combinadas para un modelo.

### ¿Qué ocurre cuando añado un archivo de corpus?
{: #parseCorpus}

Cuando se añade un archivo de corpus, el servicio analiza el contenido del archivo. Extrae las palabras nuevas (OOV) que encuentra y añade cada palabra OOV al recurso de palabras del modelo personalizado. Para condensar el significado más importante del contenido, el servicio señaliza y analiza los datos que lee de un archivo de corpus. En las secciones siguientes se describe la forma en que el servicio analiza un archivo de corpus para cada idioma al que da soporte.

#### Análisis de inglés, francés, alemán, español y portugués de Brasil
{: #corpusLanguages}

Las siguientes descripciones se aplican a inglés de EE. UU. y del Reino Unido, francés, alemán, español y portugués de Brasil.

-   Convierte los números en sus palabras equivalentes, por ejemplo:
    -   *En inglés,* `500` se convierte en `five hundred` y `0.15` se convierte en `zero point fifteen`.
    -   *En francés,* `500` se convierte en `cinq cents` y `0,15` se convierte en <code>z&eacute;ro quinze</code>.
    -   *En alemán,* `500` se convierte en <code>f&uuml;nfhundert</code> y `0,15` se convierte en <code>null punkt f&uuml;nfzehn</code>.
    -   *En español,* `500` se convierte en `quinientos` y `0,15` se convierte en `cero coma quince`.
    -   *En portugués de Brasil,* `500` se convierte en `quinhentos` y `0,15` se convierte en `zero ponto quinze`.
-   Convierte las señales que incluyen determinados símbolos en representaciones de series significativas, por ejemplo:
    -   Convierte un signo de dólar, `$`, y un número:
        -   *En inglés,* `$100` se convierte en `one hundred dollars`.
        -   *En francés,* `$100` se convierte en `cent dollar`.
        -   *En alemán,* `$100` y `100$` se convierten en `einhundert dollar`.
        -   *En español,* `$100` y `100$` se convierten en <code>cien d&oacute;lares</code> (o en `cien pesos` si el dialecto es `es-LA`).
        -   *En portugués de Brasil,*`$100` y `100$` se convierten en <code>cem d&oacute;lares</code>.
    -   Convierte un signo de euro, <code>&euro;</code>, y un número:
        -   *En inglés,* <code>&euro;100</code> se convierte en `one hundred euros`.
        -   *En francés,* <code>&euro;100</code> se convierte en `cent euros`.
        -   *En alemán,* <code>&euro;100</code> y <code>100&euro;</code> se convierten en `einhundert euro`.
        -   *En español,* <code>&euro;100</code> y <code>100&euro;</code> se convierten en `cien euros`.
        -   *En portugués de Brasil,* <code>&euro;100</code> y <code>100&euro;</code> se convierten en `cem euros`.
    -   Convierte un signo de porcentaje, `%`, precedido de un número:
        -   *En inglés,* `100%` se convierte en `one hundred percent`.
        -   *En francés,* `100%` se convierte en `cent pourcent`.
        -   *En alemán,* `100%` se convierte en `einhundert prozent`.
        -   *En español,* `100%` se convierte en `cien por ciento`.
        -   *En portugués de Brasil,* `100%` se convierte en `cem por cento`.

    Esta lista no es exhaustiva. El servicio realiza ajustes similares para otros caracteres según sea necesario.
-   Procesa los caracteres no alfanuméricos, de puntuación y especiales en su contexto. Por ejemplo, el servicio elimina un signo de dólar, `$`, o un símbolo de euro, <code>&euro;</code>, a menos que vaya seguido de un número. El proceso depende del contexto y es coherente en todos los idiomas admitidos.
-   Pasa por alto las frases entre paréntesis, `( )`, entre símbolos de mayor y menor que, `< >`, entre corchetes, `[ ]` o entre llaves, `{ }`.

#### Análisis de japonés
{: #corpusLanguages-jaJP}

-   Convierte todos los caracteres en caracteres de ancho completo.
-   Convierte los números en sus palabras equivalentes; por ejemplo, `500` se convierte en <code>&#20116;&#30334;</code> y `0.15` se convierte en <code>&#12295;&#12539;&#19968;&#20116;</code>.
-   No convierte las señales que incluyen símbolos en sus series equivalentes; por ejemplo, `100%` se convierte en <code>&#30334;&#65285;</code>.
-   No elimina automáticamente la puntuación. {{site.data.keyword.IBM_notm}} recomienda encarecidamente eliminar la puntuación si la aplicación se basa en una transcripción y no en un dictado.

#### Análisis de coreano
{: #corpusLanguages-koKR}

-   Convierte los números en sus palabras equivalentes; por ejemplo, <code>10</code> se convierte en <code>&#49901;</code>.
-   Elimina los siguientes caracteres de puntuación y especiales: `- ( ) * : . , ' "`. Sin embargo, no todos los caracteres de puntuación y especiales que se eliminan para otros idiomas se eliminan para el coreano, por ejemplo:
    -   Elimina el símbolo de punto (`.`) solo cuando aparece al final de una línea de entrada.
    -   No elimina el símbolo de tilde (`~`).
    -   No elimina ni procesa de ningún modo los símbolos Unicode de carácter amplio, como por ejemplo <code>&#8230;</code> (tres puntos o elipsis).

    En general, {{site.data.keyword.IBM_notm}} recomienda eliminar los signos de puntuación, los caracteres especiales y los caracteres Unicode amplios antes de procesar el archivo del corpus.
-   No elimina ni pasa por alto las frases entre paréntesis, `( )`, entre símbolos de mayor y menor que, `< >`, entre corchetes, `[ ]` o entre llaves, `{ }`.
-   Convierte las señales que incluyen determinados símbolos en representaciones de series significativas, por ejemplo:
    -   `24%` se convierte en <code>&#51060;&#49901;&#49324;&#54140;&#49468;&#53944;</code>.
    -   `$10` se convierte en <code>&#49901;&#45804;&#47084;</code>.

    Esta lista no es exhaustiva. El servicio realiza ajustes similares para otros caracteres según sea necesario.
-   En el caso de frases consistentes en caracteres en latín (inglés) o en una combinación de caracteres Hangul y en latín, el servicio crea palabras OOV para las frases tal como aparecen en el archivo del corpus. Y crea pronunciaciones de las palabras basadas en transcripciones en Hangul.
   - Asigna a la palabra OOV `London` la pronunciación <code>&#47088;&#45912;</code>.
   - Asigna a la palabra OOV <code>IBM&#54856;&#54168;&#51060;&#51648;</code> la pronunciación <code>&#50500;&#51060;&#32;&#48708;&#32;&#50656;&#32;&#54856;&#54168;&#51060;&#51648;</code>.

## Cómo trabajar con palabras personalizadas
{: #workingWords}

Puede utilizar los métodos `POST /v1/customizations/{customization_id}/words` y `PUT /v1/customizations/{customization_id}/words/{word_name}` para añadir palabras nuevas al recurso de palabras de un modelo personalizado. También puede utilizar los métodos para modificar o aumentar una palabra en un recurso de palabras.

Por ejemplo, es posible que tenga que utilizar los métodos para corregir un error tipográfico u otro error cometido al añadir una palabra de un corpus. También es posible que tenga que añadir definiciones de pronunciaciones para una palabra existente. Si modifica una palabra existente, los nuevos datos que proporcione sobrescriben la definición existente de la palabra en el recurso de palabras. Las reglas para añadir una palabra también se aplican a la modificación de una palabra existente.

### Codificación de caracteres
{: #charEncoding}

En general, lo más probable es que añada la mayoría de las palabras desde los corpus. Asegúrese de conocer la codificación de caracteres que se utiliza en los archivos de texto para los corpus. El servicio conserva la codificación que encuentra en los archivos de texto.

Debe utilizar dicha codificación cuando trabaje con palabras individuales en el modelo de lenguaje personalizado. Cuando especifique una palabra con el método `GET`, `PUT` o `DELETE /v1/customizations/{customization_id}/words/{word_name}`, debe codificar en URL el valor `word_name` que pase en el URL si la palabra incluye caracteres que no sean ASCII.

Por ejemplo, en la tabla siguiente se muestra el aspecto de la misma letra en dos codificaciones diferentes, ASCII y UTF-8. Puede pasar el carácter ASCII en un URL como `z`. Debe pasar el carácter UTF-8 como `%EF%BD%9A`.

<table>
  <caption>Tabla 1. Ejemplos de codificación de caracteres</caption>
  <tr>
    <th style="text-align:left">Letra</th>
    <th style="text-align:center">Codificación</th>
    <th style="text-align:center">Valor</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      `z`
    </td>
    <td style="text-align:center">
      ASCII
    </td>
    <td style="text-align:center">
      `0x7a` (`7a`)
    </td>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      <code>&#xff5a;</code>
    </td>
    <td style="text-align:center">
      UTF-8 hexadecimal
    </td>
    <td style="text-align:center">
      `0xEF 0xBD 0x9A` (`efbd9a`)
    </td>
  </tr>
</table>

### Utilización del campo sounds_like
{: #soundsLike}

El campo `sounds_like` especifica la forma en que los oradores pronuncian una palabra. De forma predeterminada, el servicio cumplimenta automáticamente el campo con la ortografía de la palabra. Puede proporcionar hasta cinco pronunciaciones alternativas para una palabra que sea difícil de pronunciar o que se pueda pronunciar de diferentes maneras. Considere la posibilidad de utilizar el campo para

-   *Proporcionar diferentes pronunciaciones para los acrónimos.* Por ejemplo, el acrónimo `NCAA` se puede pronunciar tal como se escribe o, de forma coloquial, como *N. C. double A.* En el siguiente ejemplo se añaden ambas pronunciaciones para la palabra `NCAA`:

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

-   *Gestionar palabras en otros idiomas.* Por ejemplo, la palabra en francés <code>gar&ccedil;on</code> contiene un carácter que no existe en el idioma inglés. Puede especificar la pronunciación `gaarson`, sustituyendo <code>&ccedil;</code> por `s`, para indicar al servicio la forma en que los oradores que hablan inglés pronuncian la palabra.

El reconocimiento de voz utiliza algoritmos estadísticos para analizar el audio, por lo que el hecho de añadir una palabra no garantiza que el servicio lo transcodifique con absoluta precisión. Cuando añada una palabra, tenga en cuenta cómo se puede pronunciar. Utilice el campo `sounds_like` para proporcionar varias pronunciaciones que reflejen la forma en que se puede pronunciar una palabra. En las secciones siguientes se proporcionan directrices específicas del idioma para especificar una pronunciación.

#### Directrices para inglés (EE. UU. y Reino Unido)
{: #wordLanguages-enUS-enGB}

*Directrices para inglés de EE. UU. y del Reino Unido:*

-   Utilice caracteres alfabéticos en inglés: `a-z` y `A-Z`.
-   Utilice palabras reales o inventadas que se puedan pronunciar en inglés para las palabras difíciles de pronunciar, como por ejemplo `shuchesnie` para la palabra `Sczcesny`.
-   Sustituya por letras equivalentes en inglés las letras que no existen en inglés, como por ejemplo `s` para <code>&ccedil;</code> o `ny` para <code>&ntilde;</code>.
-   Sustituya las letras acentuadas por letras no acentuadas, por ejemplo `a` para <code>&agrave;</code> o `e` para <code>&egrave;</code>.
-   Puede incluir varias palabras separadas por espacios, pero el servicio impone un máximo de 40 caracteres en total sin incluir espacios.

*Directrices solo para inglés de EE. UU.:*

-   Para pronunciar una sola letra, utilice la letra seguida de un punto. Si el punto va seguido de otro carácter, asegúrese de utilizar un espacio entre el punto y el siguiente carácter. Por ejemplo, utilice `N. C. A. A.`, *no* `N.C.A.A.`
-   Escriba los números; por ejemplo, `seventy-five` para `75`.

*Directrices solo para inglés del Reino Unido:*

-   **No puede** utilizar puntos ni guiones en pronunciaciones en inglés del Reino Unido.
-   Para pronunciar una sola letra, utilice la letra seguida de un espacio. Por ejemplo, utilice `N C A A`, *no* `N. C. A. A.`, `N.C.A.A.` ni `NCAA`.
-   Escriba los números sin guiones; por ejemplo, `seventy five` para `75`.

#### Directrices para francés, alemán, español y portugués de Brasil
{: #wordLanguages-esES-frFR}

-   **No puede** utilizar guiones en pronunciaciones.
-   Utilice los caracteres alfabéticos que sean válidos para el idioma: `a-z` y `A-Z`, incluidas las letras acentuadas válidas.
-   Para pronunciar una sola letra, utilice la letra seguida de un punto. Si el punto va seguido de otro carácter, asegúrese de utilizar un espacio entre el punto y el siguiente carácter. Por ejemplo, utilice `N. C. A. A.`, *no* `N.C.A.A.`
-   Utilice palabras reales o inventadas que se puedan pronunciar en el idioma para las palabras difíciles de pronunciar.
-   Escriba los números sin guiones; por ejemplo, para `75` utilice
    -   *Francés:* `soixante quinze`
    -   *Alemán:* <code>f&uuml;nfundsiebzig</code>
    -   *Español:* `setenta y cinco`
    -   *Portugués de Brasil:* `setenta e cinco`
-   Puede incluir varias palabras separadas por espacios, pero el servicio impone un máximo de 40 caracteres en total sin incluir espacios.

#### Directrices para japonés
{: #wordLanguages-jaJP}

-   Utilice solo caracteres Katakana de ancho completo mediante el símbolo de extensión <code>&#8213;</code> (*chou-on* o &#38263;&#38899;, en japonés). No utilice caracteres de media anchura.
-   Utilice sonidos contraídos (*yoh-on* o &#25303;&#38899;, en japonés) solo en los siguientes contextos de sílabas:

    <code>&#12452;&#12455;</code>, <code>&#12454;&#12451;</code>, <code>&#12454;&#12455;</code>, <code>&#12454;&#12457;</code>, <code>&#12461;&#12451;</code>, <code>&#12461;&#12515;</code>, <code>&#12461;&#12517;</code>, <code>&#12461;&#12519;</code>, <code>&#12462;&#12515;</code>, <code>&#12462;&#12517;</code>, <code>&#12462;&#12519;</code>, <code>&#12463;&#12449;</code>, <code>&#12463;&#12451;</code>, <code>&#12463;&#12455;</code>, <code>&#12463;&#12457;</code>,<br/>
<code>&#12464;&#12449;</code>, <code>&#12464;&#12457;</code>, <code>&#12471;&#12451;</code>, <code>&#12471;&#12455;</code>, <code>&#12471;&#12515;</code>, <code>&#12471;&#12517;</code>, <code>&#12471;&#12519;</code>, <code>&#12472;&#12451;</code>, <code>&#12472;&#12455;</code>, <code>&#12472;&#12515;</code>, <code>&#12472;&#12517;</code>, <code>&#12472;&#12519;</code>, <code>&#12473;&#12451;</code>, <code>&#12474;&#12451;</code>, <code>&#12481;&#12455;</code>,<br/>
<code>&#12481;&#12515;</code>, <code>&#12481;&#12517;</code>, <code>&#12481;&#12519;</code>, <code>&#12482;&#12455;</code>, <code>&#12482;&#12515;</code>, <code>&#12482;&#12517;</code>, <code>&#12482;&#12519;</code>, <code>&#12484;&#12449;</code>, <code>&#12484;&#12451;</code>, <code>&#12484;&#12455;</code>, <code>&#12484;&#12457;</code>, <code>&#12486;&#12451;</code>, <code>&#12486;&#12517;</code>, <code>&#12487;&#12451;</code>, <code>&#12487;&#12515;</code>,<br/>
<code>&#12487;&#12517;</code>, <code>&#12487;&#12519;</code>, <code>&#12488;&#12453;</code>, <code>&#12489;&#12453;</code>, <code>&#12491;&#12455;</code>, <code>&#12491;&#12515;</code>, <code>&#12491;&#12517;</code>, <code>&#12491;&#12519;</code>, <code>&#12498;&#12515;</code>, <code>&#12498;&#12517;</code>, <code>&#12498;&#12519;</code>, <code>&#12499;&#12515;</code>, <code>&#12499;&#12517;</code>, <code>&#12499;&#12519;</code>, <code>&#12500;&#12451;</code>,<br/>
<code>&#12500;&#12515;</code>, <code>&#12500;&#12517;</code>, <code>&#12500;&#12519;</code>, <code>&#12501;&#12449;</code>, <code>&#12501;&#12451;</code>, <code>&#12501;&#12455;</code>, <code>&#12501;&#12457;</code>, <code>&#12501;&#12517;</code>, <code>&#12511;&#12515;</code>, <code>&#12511;&#12517;</code>, <code>&#12511;&#12519;</code>, <code>&#12522;&#12451;</code>, <code>&#12522;&#12455;</code>, <code>&#12522;&#12515;</code>, <code>&#12522;&#12517;</code>,<br/>
<code>&#12522;&#12519;</code>, <code>&#12532;&#12449;</code>, <code>&#12532;&#12451;</code>, <code>&#12532;&#12455;</code>, <code>&#12532;&#12457;</code>, <code>&#12532;&#12517;</code>

-   Utilice solo las sílabas siguientes después de un sonido asimilado (*soku-on* o &#20419;&#38899; en japonés):

    <code>&#12496;</code>, <code>&#12499;</code>, <code>&#12502;</code>, <code>&#12505;</code>, <code>&#12508;</code>, <code>&#12481;</code>, <code>&#12481;&#12455;</code>, <code>&#12481;&#12515;</code>, <code>&#12481;&#12517;</code>, <code>&#12481;&#12519;</code>, <code>&#12480;</code>, <code>&#12487;</code>, <code>&#12487;&#12451;</code>, <code>&#12489;</code>, <code>&#12489;&#12453;</code>, <code>&#12501;</code>,<br/>
<code>&#12501;&#12449;</code>, <code>&#12501;&#12451;</code>, <code>&#12501;&#12455;</code>, <code>&#12501;&#12457;</code>, <code>&#12460;</code>, <code>&#12462;</code>, <code>&#12464;</code>, <code>&#12466;</code>, <code>&#12468;</code>, <code>&#12495;</code>, <code>&#12498;</code>, <code>&#12504;</code>, <code>&#12507;</code>, <code>&#12472;</code>, <code>&#12472;&#12455;</code>, <code>&#12472;&#12515;</code>,<br/>
<code>&#12472;&#12517;</code>, <code>&#12472;&#12519;</code>, <code>&#12459;</code>, <code>&#12461;</code>, <code>&#12463;</code>, <code>&#12465;</code>, <code>&#12467;</code>, <code>&#12461;&#12515;</code>, <code>&#12461;&#12517;</code>, <code>&#12461;&#12519;</code>, <code>&#12497;</code>, <code>&#12500;</code>, <code>&#12503;</code>, <code>&#12506;</code>, <code>&#12509;</code>, <code>&#12500;&#12515;</code>,<br/>
<code>&#12500;&#12517;</code>, <code>&#12500;&#12519;</code>, <code>&#12469;</code>, <code>&#12473;</code>, <code>&#12475;</code>, <code>&#12477;</code>, <code>&#12471;</code>, <code>&#12471;&#12455;</code>, <code>&#12471;&#12515;</code>, <code>&#12471;&#12517;</code>, <code>&#12471;&#12519;</code>, <code>&#12479;</code>, <code>&#12486;</code>, <code>&#12488;</code>, <code>&#12484;</code>, <code>&#12470;</code>,<br/>
<code>&#12474;</code>, <code>&#12476;</code>, <code>&#12478;</code>

-   No utilice <code>&#12531;</code> como primer carácter de una palabra. Por ejemplo, utilice <code>&#12454;&#12540;&#12531;&#12488;</code> en lugar de <code>&#12531;&#12540;&#12488;</code>; esta última opción no es válida.
-   Muchas palabras compuestas constan de *prefijo+nombre* o *nombre+sufijo*. El vocabulario base del servicio cubre la mayoría de las palabras compuestas que aparecen con frecuencia (por ejemplo, <code>&#x9577;&#x96FB;&#x8A71;</code> y <code>&#x53E4;&#x65B0;&#x805E;</code>), pero no las que aparecen con poca frecuencia. Si el corpus contiene palabras compuestas, añádalas como una palabra como primer paso de la personalización. Por ejemplo, <code>&#x53E4;&#x925B;&#x7B46;</code> no es una expresión común en textos generales en japonés; si la utiliza con frecuencia, añádala al modelo personalizado para mejorar la precisión de la transcripción.
-   No utilice un sonido asimilado final.

#### Directrices para coreano
{: #wordLanguages-koKR}

-   Utilice caracteres, símbolos y sílabas de coreano Hangul.
-   También puede utilizar caracteres alfabéticos en inglés: `a-z` y `A-Z`.
-   No utilice caracteres o símbolos que no estén incluidos en los conjuntos anteriores.

### Utilización del campo display_as
{: #displayAs}

El campo `display_as` especifica cómo se visualiza una palabra en una transcripción. Está pensado para los casos en los que desea que el servicio muestre una serie distinta de la ortografía de la palabra. Por ejemplo, puede indicar que la palabra `hhonors` se muestre como `HHonors`, independientemente de si suena como `hilton honors` o `h honors`.

```bash
curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/hhonors"
```
{: pre}

Otro ejemplo: puede indicar que la palabra `IBM` se muestre como <code>IBM&trade;</code>.

<pre><code class="language-bash">curl -X PUT -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"sounds_like\": [\"I. B. M.\"], \"display_as\":\"IBM&#8482;\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IBM"</code></pre>

#### Interacción con el formateo inteligente y la ocultación numérica
{: #displaySmart}

Si utiliza los parámetros `smart_formatting` o `redaction` con una solicitud de reconocimiento, tenga en cuenta que el servicio aplica el formateo inteligente y la ocultación a una palabra antes de considerar el campo `display_as` para la palabra. Es posible que tenga que experimentar con resultados para asegurarse de que las características no interfieran con el modo en que se visualizan las palabras personalizadas. También es posible que tenga que añadir palabras personalizadas para dar cabida a los efectos.

Por ejemplo, suponga que añade la palabra personalizada `one` con un campo `display_as` de `one`. El formateo inteligente cambia la palabra `one` por el número `1` y el valor de display-as no se aplica. Para solucionar este problema, puede añadir una palabra personalizada para el número `1` y aplicar el campo `display_as` a dicha palabra.

Para obtener más información sobre cómo trabajar con estas características, consulte [Formateo inteligente](/docs/services/speech-to-text/output.html#smart_formatting) y [Ocultación numérica](/docs/services/speech-to-text/output.html#redaction).

### ¿Qué ocurre cuando añado o modifico una palabra personalizada?
{: #parseWord}

El modo en que el servicio responde a una solicitud de añadir o modificar una palabra personalizada depende de los valores que proporcione y de si la palabra existe en el vocabulario base del servicio.

<table>
  <caption>Tabla 1. Adición de palabras personalizadas con distintos campos</caption>
  <tr>
    <th style="width:20%; text-align:center; vertical-align:bottom">El campo <code>sounds_like</code> es...</th>
    <th style="width:20%; text-align:center; vertical-align:bottom">El campo <code>display_as</code> es...</th>
    <th style="text-align:left; vertical-align:bottom">Respuesta</th>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      No especificado
    </td>
    <td style="text-align:center; vertical-align:top">
      No especificado
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Si la palabra no existe en el vocabulario base del
          servicio,</em> el servicio establece el campo <code>sounds_like</code>
          en su pronunciación de la palabra. Establece el campo
          <code>display_as</code> en el valor del campo
          <code>word</code>.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Si la palabra existe en el vocabulario base del servicio,</em>
          el servicio deja los campos <code>sounds_like</code> y
          <code>display_as</code> vacíos. Estos campos solo están vacíos si la palabra
          existe en el vocabulario base del servicio. La presencia de la palabra
          en el recurso de palabras del modelo no perjudica pero es innecesaria.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Especificado
    </td>
    <td style="text-align:center; vertical-align:top">
      No especificado
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Si el campo <code>sounds_like</code> es válido,</em> el
          servicio establece el campo <code>display_as</code> en el valor del
          campo <code>word</code>.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Si el campo <code>sounds_like</code> no es válido:</em>
          <ul style="margin-left:20px; padding:0px">
            <li style="margin:10px 0px; line-height:120%">
              El método <code>POST
                /v1/customizations/{customization_id}/words</code>
              añade un campo <code>error</code> a la palabra en el recurso de palabras
              del modelo.
            </li>
            <li style="margin:10px 0px; line-height:120%">
              El método <code>PUT
                /v1/customizations/{customization_id}/words/{word_name}</code>
              falla con el código de respuesta 400 y un mensaje de error. El servicio
              no añade la palabra al recurso de palabras.
            </li>
          </ul>
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      No especificado
    </td>
    <td style="text-align:center; vertical-align:top">
      Especificado
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Si la palabra no existe en el vocabulario base del servicio,</em> el servicio establece el campo <code>sounds_like</code> en su pronunciación de la palabra y deja el campo <code>display_as</code> tal como está especificado.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Si la palabra existe en el vocabulario base del servicio,</em> el servicio deja el valor <code>sounds_like</code> vacío y el campo <code>display_as</code> tal como está especificado.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center; vertical-align:top">
      Especificado
    </td>
    <td style="text-align:center; vertical-align:top">
      Especificado
    </td>
    <td style="text-align:left; vertical-align:top">
      <ul style="margin-left:20px; padding:0px">
        <li style="margin:10px 0px; line-height:120%">
          <em>Si el campo <code>sounds_like</code> es válido,</em> el servicio establece los campos <code>sounds_like</code> y <code>display_as</code> en los valores especificados.
        </li>
        <li style="margin:10px 0px; line-height:120%">
          <em>Si el campo <code>sounds_like</code> no es válido,</em> el servicio responde igual que en el caso en el que se especifica el campo <code>sounds_like</code> pero no el campo <code>display_as</code>.
        </li>
      </ul>
    </td>
  </tr>
</table>

## Validación de un recurso de palabras
{: #validateModel}

Especialmente cuando añade un corpus a un modelo de lenguaje personalizado o cuando añade varias palabras personalizadas a la vez, examine las palabras OOV del recurso de palabras del modelo.

-   *Busque errores tipográficos y de otro tipo.* Especialmente cuando se añaden corpus, que pueden ser grandes, es fácil cometer errores. Los errores tipográficos en un archivo de corpus (o de gramática) tienen la consecuencia no deseada de añadir nuevas palabras al recurso de palabras de un modelo, al igual que sucede con las etiquetas HTML mal formadas que se dejan en un archivo de corpus.
-   *Verifique las pronunciaciones.* El servicio genera automáticamente pronunciaciones para las palabras OOV. En la mayoría de los casos, estas pronunciaciones son suficientes. Pero, en el caso de palabras que tienen ortografías inusuales o que son difíciles de pronunciar y en el caso de acrónimos y términos técnicos, se recomienda revisar la precisión de las pronunciaciones.

Para validar y, si es necesario, corregir una palabra de un modelo personalizado, independientemente de cómo se haya añadido al recurso de palabras, utilice los métodos siguientes:

-   Cree una lista de palabras de un modelo personalizado mediante el método `GET /v1/customizations/{customization_id}/words` o consulte una palabra individual con el método `GET /v1/customizations/{customization_id}/words/{word_name}`. Para obtener más información, consulte [Listado de las palabras de un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-words.html#listWords).
-   Modifique las palabras de un modelo personalizado para corregir errores o añada valores sounds-like o display-as mediante el método `POST /v1/customizations/{customization_id}/words` o el método `PUT /v1/customizations/{customization_id}/words/{word_name}`. Para obtener más información, consulte [Cómo trabajar con palabras personalizadas](#workingWords).
-   Suprima las palabras extrañas que se incorporan por error (por ejemplo, por errores tipográficos o de otro tipo en un corpus) con el método `DELETE /v1/customizations/{customization_id}/words/{word_name}`. Para obtener más información, consulte [Supresión de una palabra de un modelo de lenguaje personalizado](/docs/services/speech-to-text/language-words.html#deleteWord).
    -   Si la palabra se ha extraído de un corpus, puede actualizar el archivo de texto del corpus para corregir el error y luego volver a cargar el archivo con el parámetro `allow_overwrite` del método `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`. Para obtener más información, consulte [Adición de un corpus al modelo de lenguaje personalizado](/docs/services/speech-to-text/language-create.html#addCorpus).
    -   Si la palabra se ha extraído de una gramática, puede actualizar el archivo de la gramática para corregir el error y luego volver a cargar el archivo con el parámetro `allow_overwrite` del método `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`. Para obtener más información, consulte [Adición de una gramática al modelo de lenguaje personalizado](/docs/services/speech-to-text/grammar-add.html#addGrammar).
