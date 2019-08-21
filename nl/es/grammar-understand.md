---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-04"

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

# Visión general de las gramáticas
{: #grammarUnderstand}

En los ejemplos siguientes se presenta el soporte que da el servicio {{site.data.keyword.speechtotextfull}} a las gramáticas. En los ejemplos se crean dos gramáticas ABNF sencillas y se muestran los resultados posibles cuando se utilizan para el reconocimiento de voz. Los ejemplos ilustran la importancia de examinar la puntuación de confianza que incluye el servicio con una transcripción.
{: shortdesc}

Los ejemplos solo proporcionan los resultados de las solicitudes de reconocimiento de voz. Para ver ejemplos que muestran cómo se pasa una gramática para el reconocimiento de voz, consulte [Utilización de una gramática para el reconocimiento de voz](/docs/services/speech-to-text?topic=speech-to-text-grammarUse). Los ejemplos también son muy básicos. Para ver ejemplos de gramáticas más complejas, consulte [Gramáticas de ejemplo](/docs/services/speech-to-text?topic=speech-to-text-grammarExamples).

## Coincidencias de una sola frase: la gramática yesno
{: #yesnoGrammar}

El primer ejemplo define una gramática `yesno` muy sencilla que acepta dos respuestas válidas de una sola palabra: `yes` y `no`. La gramática es útil en los casos en los que el usuario solo debe responder con una de las dos frases.

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $yesno;

$yesno = yes | no ;
```
{: codeblock}

Cuando se aplica esta gramática a una solicitud de reconocimiento de voz, el servicio puede devolver una transcripción con una puntuación que indica su confianza en la coincidencia. También puede no devolver ningún resultado si la entrada claramente no coincide con ninguna de las dos frases.

Por ejemplo, si el usuario responde `yes`, es probable que el servicio devuelva una respuesta muy parecida a la del resultado siguiente. La puntuación del campo `confidence` indica una coincidencia perfectamente fiable.

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 1.0,
          "transcript": "yes"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Pero supongamos, por ejemplo, que el usuario responde `nope`. El servicio puede devolver un resultado con una puntuación de confianza muy baja o puede no devolver ningún resultado en absoluto. Un resultado vacío es la indicación más clara de que la respuesta no coincide con la gramática. Es más probable que se devuelva una respuesta vacía con gramáticas complejas, en las que una respuesta válida debe coincidir con una secuencia específica de varias frases.

## Coincidencias de varias frases: la gramática names
{: #namesGrammar}

Con una gramática de varias frases, la respuesta del usuario debe estar completa para que se reconozca. El usuario no puede omitir una palabra ni detenerse a mitad de respuesta. La ausencia de una sola palabra puede hacer que el servicio devuelva un resultado vacío.

Además, el servicio puede devolver varias transcripciones si el usuario pronuncia frases separadas por un silencio suficiente para indicar que son expresiones independientes. Por ejemplo, examine la gramática sencilla `names`, que puede coincidir con uno de tres nombres de varias palabras.

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $names;
$names = Yi Wen Tan | Yon See | Youngjoon Lee ;
```
{: codeblock}

Supongamos que el usuario pronuncia uno de los nombres de las reglas de la gramática, `Yon See`. El servicio devuelve una respuesta que indica un nivel muy alto de confianza en la coincidencia.

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.92,
          "transcript": "Yon See"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Ahora supongamos que el usuario pronuncia dos nombres separados por un silencio suficiente, de al menos 0,8 segundos, para indicar que son expresiones separadas: `Yon See` [1,0 segundo de silencio] `Yi Wen Tan`. En este caso, el servicio envía dos respuestas separadas con una puntuación de confianza diferente para cada transcripción.

```
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.92,
          "transcript": "Yon See"
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
},
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.83,
          "transcript": "Yi Wen Tan"
        }
      ],
      "final": true
    }
  ],
  "result_index": 1
}
```
{: codeblock}

Por último, supongamos que el usuario pronuncia algo como `Yon See` [1,0 segundo de silencio] `Young Says He`. Para la primera frase, `Yon See`, el servicio envía una coincidencia positiva con una puntuación de confianza, de forma similar a la de los ejemplos anteriores. Para la segunda frase, `Young Says He`, el servicio puede dar una de estas dos respuestas posibles:

-   Es posible que no envíe ninguna respuesta, lo que indica que la frase no coincide con ninguna de las reglas de la gramática.
-   Es posible que envíe una respuesta con una puntuación de confianza baja, lo que indica que la frase es acústicamente parecida a una de las reglas, pero no constituye una coincidencia probable.

## Puntuaciones de confianza y resultados vacíos
{: #confidenceScores}

En el caso de gramáticas relativamente sencillas como las de los ejemplos anteriores, el servicio puede devolver un resultado con una puntuación de confianza baja incluso cuando la respuesta no parece coincidir con la gramática en absoluto. Esto puede parecer sorprendente, pero una respuesta con una puntuación de confianza baja puede indicar la mejor coincidencia que ha encontrado el servicio para el audio dado, aunque es poco probable que la respuesta coincida con la gramática. Si la respuesta no coincide con la gramática, el valor de confianza de los resultados suele ser lo suficientemente bajo como para indicar la poca probabilidad de que la gramática pueda generar la respuesta.

Siempre tenga en cuenta la puntuación de confianza al evaluar si una respuesta satisface una gramática. Si no sabe qué umbral se debe establecer para el rechazo de un resultado en función en su confianza, utilice un valor inicial de 0,5. Si se produce una tasa de rechazos inesperadamente alta de resultados correctos, reduzca el umbral de confianza; por ejemplo, 0,1 podría ser una buena opción para algunas aplicaciones. Si se produce un gran número de reconocimientos incorrectos, aumente el umbral de confianza para la aplicación.

En muchos casos, un resultado vacío o un resultado con una puntuación de confianza muy baja es una respuesta válida. Esto puede indicar que el usuario no ha entendido la pregunta o las opciones disponibles. Siempre se puede reconocer la respuesta de audio del usuario sin la gramática, pero entonces se corre el riesgo de obtener un resultado que no es válido para la gramática. Las gramáticas proporcionan resultados más fiables que los n-gramas, que siempre devuelven un resultado para cualquier cosa que no sea silencio absoluto.

El hecho de saber que el resultado debe ser una de las frases válidas definidas por una gramática es una característica potente que puede simplificar el manejo de respuestas de una aplicación. En términos generales, el servicio puede devolver resultados con una confianza alta solo para las expresiones generadas por una gramática. En el caso de la gramática `yesno` del primer ejemplo, para reconocer de forma fiable la frase `nope` como una respuesta válida, debe añadir dicha frase a la gramática.
