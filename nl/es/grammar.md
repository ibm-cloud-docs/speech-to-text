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

# Utilización de gramáticas con modelos de lenguaje personalizado
{: #grammars}

El servicio {{site.data.keyword.speechtotextfull}} da soporte a la utilización de gramáticas con modelos de lenguaje personalizado. Puede añadir gramáticas a un modelo de lenguaje personalizado y utilizarlas para el reconocimiento de voz. Las gramáticas restringen el conjunto de frases que el servicio puede reconocer del audio.
{: shortdesc}

Una gramática utiliza una especificación de lenguaje formal para definir un conjunto de reglas de producción para transcribir series de caracteres. Las reglas especifican cómo formar series válidas a partir del alfabeto del idioma. Cuando se aplica una gramática al reconocimiento de voz, el servicio solo puede devolver una o varias de las frases generadas por la gramática.

Por ejemplo, si necesita reconocer palabras o frases específicas, como *yes* o *no*, letras o números individuales o una lista de nombres, el uso de gramáticas puede resultar más efectivo que examinar palabras y transcripciones alternativas. Además, al limitar el espacio de búsqueda de series válidas, el servicio puede ofrecer resultados más rápido y con mayor precisión.

Cuando se utiliza un modelo de lenguaje personalizado y una gramática para el reconocimiento de voz, el servicio puede devolver una frase válida de la gramática o un resultado vacío. Si el resultado no está vacío, el servicio incluye una puntuación de confianza con la transcripción final, como lo hace para todas las solicitudes de reconocimiento. En el caso de gramáticas, la puntuación indica la probabilidad de que la respuesta coincida con la gramática. Siempre existe la posibilidad de que se produzcan falsos positivos, especialmente para gramáticas sencillas, por lo que siempre debe tener en cuenta la confianza de los resultados del servicio cuando evalúe su respuesta.

La característica de gramáticas es una funcionalidad beta. El servicio da soporte a gramáticas para todos los idiomas para los que da soporte a la personalización del modelo de lenguaje. Para obtener más información, consulte [Soporte de idiomas para la personalización](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport).
{: note}

## Formatos de gramática soportados
{: #grammarFormats}

El servicio {{site.data.keyword.speechtotextshort}} da soporte a gramáticas definidas en los siguientes formatos estándares:

-   *Augmented Backus-Naur Form (ABNF)*, que utiliza una representación en texto sin formato que se parece a la gramática BNF tradicional. El tipo de soporte para este formato es `application/srgs`.
-   *Formato XML*, que utiliza elementos XML para representar la gramática. El tipo de soporte para este formato es `application/srgs+xml`.

Ambos formatos de gramática tienen la potencia de expresión de la gramática Context-Free Grammar (CFG). Sin embargo, el servicio solo puede decodificar gramáticas normales de tipo 3 en la jerarquía Chomsky. Ambas gramáticas representan el modelo Finite State Automata.

Para ver información general sobre gramáticas, consulte las siguientes páginas de la Wikipedia:

-   [Speech Recognition Grammar Specification](https://wikipedia.org/wiki/Speech_Recognition_Grammar_Specification){: external}
-   [Augmented Backus-Naur form](https://wikipedia.org/wiki/Augmented_Backus%E2%80%93Naur_form){: external}
-   [Jerarquía Chomsky](https://wikipedia.org/wiki/Chomsky_hierarchy){: external}

## Speech Recognition Grammar Specification
{: #grammarSpecification}

El servicio {{site.data.keyword.speechtotextshort}} da soporte a las gramáticas definidas por W3C [Speech Recognition Grammar Specification, versión 1.0](https://www.w3.org/TR/speech-grammar/){: external}. La especificación proporciona información detallada acerca de los formatos soportados y sobre la definición de una gramática. Para obtener información sobre los tipos de soporte admitidos, consulte [Appendix G. Media Types and File Suffix](https://www.w3.org/TR/speech-grammar/#AppG){: external} de la especificación.

El servicio *no* da soporte actualmente a todas las características de la especificación Speech Recognition Grammar Specification. En concreto, el servicio no da soporte a las características descritas en las secciones siguientes de la especificación:

-   [Section 1.4 Semantic Interpretation](https://www.w3.org/TR/speech-grammar/#S1.4){: external}. {{site.data.keyword.IBM_notm}} está trabajando para dar soporte a esta característica en un futuro release del servicio.
-   [Section 1.5 Embedded Grammars](https://www.w3.org/TR/speech-grammar/#S1.5){: external}. {{site.data.keyword.IBM_notm}} está trabajando para dar soporte a esta característica en un futuro release del servicio.
-   [Section 2.2.2 External Reference by URI](https://www.w3.org/TR/speech-grammar/#S2.2.2){: external}. El servicio solo da soporte a referencia locales, tal como se describe en [Section 2.2.1 Local References](https://www.w3.org/TR/speech-grammar/#S2.2.1){: external}. Es decir, una gramática debe ser auto contenida.
-   [Section 2.2.3 Special Rules](https://www.w3.org/TR/speech-grammar/#S2.2.3){: external}.
-   [Section 2.2.4 Referencing N-gram Documents (Informative)](https://www.w3.org/TR/speech-grammar/#S2.2.4){: external}.
-   [Section 2.7 Language](https://www.w3.org/TR/speech-grammar/#S2.7){: external}. El servicio no permite cambiar de idioma. El servicio solo da soporte a un idioma global por gramática.

Las palabras de la gramática deben estar en codificación UTF-8 (ASCII es un subconjunto de UTF-8). El uso de cualquier otra codificación puede dar lugar a problemas al compilar la gramática o a resultados inesperados en la decodificación. El servicio pasa por alto una codificación especificada en la cabecera de la gramática.
{: note}
