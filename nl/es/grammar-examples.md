---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-06"

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

# Gramáticas de ejemplo
{: #grammarExamples}

En los ejemplos siguientes se describen usos más complejos para gramáticas con reconocimiento de voz. La mayoría de los ejemplos se proporcionan tanto en formato ABNF (`application/srgs`) como en formato XML (`application/srgs+xml`). Utilice estos ejemplos como ayuda para empezar a utilizar gramáticas.
{: shortdesc}

En una regla de producción para una gramática, incluir una barra inclinada invertida `\` constituye un error de sintaxis en ABNF y se interpreta como un valor literal en XML.
{: tip}

## Gramáticas de confirmación
{: #confirmation}

Las gramáticas de confirmación resultan útiles para las aplicaciones que esperan una respuesta de una sola palabra a una pregunta. En las siguientes gramáticas se define una lista de posibles respuestas positivas y negativas.

<table style="width:50%">
  <caption>Tabla 1. Gramáticas de confirmación</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.abnf" download="confirm.abnf">confirm.abnf <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a>
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.xml" download="confirm.xml">confirm.xml <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a>
    </td>
  </tr>
</table>

## Gramáticas de lista
{: #list}

Las gramáticas de lista resultan útiles para las aplicaciones que esperan que el usuario seleccione una opción de un conjunto predefinido de series. El conjunto a veces se denomina lista plana. Por lo general, consta de una lista de elementos de terminal y no incluye una estructura de reglas complicada.

-   En las siguientes gramáticas se define una lista de frases correspondientes a dígitos.

    <table style="width:50%">
      <caption>Tabla 2. Gramáticas de lista de números</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.abnf" download="list-numbers.abnf">list-numbers.abnf <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a>
        </td>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.xml" download="list-numbers.xml">list-numbers.xml <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a>
        </td>
      </tr>
    </table>

-   En la gramática siguiente se define una lista de nombres válidos entre los cuales el usuario puede elegir, tal vez para seleccionar un individuo de un directorio telefónico.

    <table style="width:50%">
      <caption>Tabla 3. Gramática de lista de nombres</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-names.abnf" download="list-names.abnf">list-names.abnf <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a>
        </td>
        <td style="text-align:center">
          No disponible
        </td>
      </tr>
    </table>

## Gramáticas de números de chasis
{: #vin}

Los números de chasis (en inglés, Vehicle Identification Numbers, VIN) utilizan un formato alfanumérico fijo, al igual que los números de tarjetas de crédito, los números de teléfono y los números de seguridad social de Estados Unidos. Una gran ventaja de las gramáticas sobre los n-gramas clásicos es que las gramáticas resultan muy eficaces para especificar este tipo de formatos.

El formato VIN está bien estandarizado y tiene un número fijo de caracteres. Los n-gramas clásicos ofrecen un bajo rendimiento para estos tipos de tareas. A diferencia de las gramáticas, no pueden garantizar que se especifique el número necesario de caracteres.

En las siguientes gramáticas se reconocen los códigos VIN de Honda. Son más complejos que los ejemplos anteriores, pero demuestran muy bien la potencia de las gramáticas.

<table style="width:50%">
  <caption>Tabla 4. Gramáticas de VIN</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.abnf" download="vins.abnf">vins.abnf <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a>.
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.xml" download="vins.xml">vins.xml <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a>.
    </td>
  </tr>
</table>

Para obtener más información sobre el formato VIN, consulte [Número de identificación de vehículo](https://wikipedia.org/wiki/Vehicle_identification_number){: external}.

## Gramáticas con elementos opcionales
{: #optionalElements}

Si hace que determinados elementos de una respuesta sean opcionales, puede conseguir gramáticas más flexibles anticipándose a la posible respuesta de los usuarios. En la siguiente gramática se especifica el elemento `$optionalphrase` entre corchetes para que sea opcional. El usuario puede pronunciar una de algunas frases adicionales antes de indicar el número de seguridad social. Por ejemplo, el usuario puede decir "my social is xxx xx xxxx" o simplemente "xxx xx xxxx".

<table style="width:50%">
  <caption>Tabla 5. Gramática con elemento opcional</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/optional.abnf" download="optional.abnf">optional.abnf <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo" title="Icono de enlace externo"></a>
    </td>
    <td style="text-align:center">
      No disponible
    </td>
  </tr>
</table>

Para obtener más información sobre las expansiones gramaticales opcionales, consulte [Sección 2.5 Repeticiones](https://www.w3.org/TR/speech-grammar/#S2.5){: external} de Speech Recognition Grammar Specification.

## Gramáticas desambiguación
{: #disambiguation}

Otras posibles gramáticas de ejemplo incluyen situaciones en las que la confianza de la respuesta reconocida no es muy alta, pero la aplicación tiene que estar segura de la respuesta del usuario. En este caso, la aplicación puede crear de forma dinámica una gramática que incluya las respuestas más probables y pedir al usuario que repita la respuesta. Por ejemplo, la aplicación puede crear una gramática que incluya las dos opciones más probables y pida al usuario que seleccione una de las dos: `Did you mean 'option 1' or 'option 9'?`

Las gramáticas de desambiguación se generan mediante programación. No se proporcionan ejemplos.
