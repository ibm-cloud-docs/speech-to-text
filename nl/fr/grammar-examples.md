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

# Exemples de grammaires
{: #grammarExamples}

Les exemples suivants présentent une utilisation plus complexe des grammaires avec la reconnaissance vocale. La plupart des exemples sont fournis aux formats ABNF (`application/srgs`) et XML (`application/srgs+xml`). Utilisez ces exemples pour vous familiariser avec les grammaires.
{: shortdesc}

Dans une règle de production de grammaire, l'insertion d'une barre oblique inversée (`\`) est une erreur de syntaxe en format ABNF et est interprétée comme une valeur littérale en format XML.
{: tip}

## Grammaires de type confirmation
{: #confirmation}

Les grammaires de type confirmation sont utiles pour les applications qui attendent le renvoi d'un seul mot en réponse à une question. Les grammaires suivantes définissent une liste de réponses oui/non (yes et no) possibles.

<table style="width:50%">
  <caption>Tableau 1. Grammaires de type confirmation</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.abnf" download="confirm.abnf">confirm.abnf <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a>
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.xml" download="confirm.xml">confirm.xml <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a>
    </td>
  </tr>
</table>

## Grammaires de type liste
{: #list}

Les grammaires de type liste sont utiles pour les applications qui attendent que l'utilisateur sélectionne une option parmi un ensemble de chaînes prédéfini. Cet ensemble est parfois désigné par "liste à plat". Il comprend en principe une liste d'éléments du vocabulaire terminal et une structure de règle simple.

-   Les grammaires suivantes définissent une liste d'expressions correspondant à des chiffres.

    <table style="width:50%">
      <caption>Tableau 2. Grammaire de liste de nombres</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.abnf" download="list-numbers.abnf">list-numbers.abnf <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a>
        </td>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.xml" download="list-numbers.xml">list-numbers.xml <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a>
        </td>
      </tr>
    </table>

-   La grammaire suivante définit une liste de noms valides que l'utilisateur doit utiliser pour faire son choix, éventuellement pour sélectionner une personne dans un annuaire téléphonique.

    <table style="width:50%">
      <caption>Tableau 3. Grammaire de liste de noms</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-names.abnf" download="list-names.abnf">list-names.abnf <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a>
        </td>
        <td style="text-align:center">
          Non disponible
        </td>
      </tr>
    </table>

## Grammaire de numéros d'identification de véhicule (VIN)
{: #vin}

Les numéros d'identification de véhicule (VIN) utilisent un format alphanumérique déterminé, à l'instar des numéros de carte de crédit, des numéros de téléphone ou des numéros de sécurité sociale américains. L'énorme avantage des grammaires sur la méthode des n-grammes classique réside dans le fait qu'elles sont très efficaces pour la spécification de ces types de format.

Le format VIN est normalisé avec un nombre fixe de caractères. La méthode classique des n-grammes est peu efficace pour ces types de tâche. Contrairement aux grammaires, les n-grammes ne permettent pas de garantir le nombre requis de caractères fournis.

Les grammaires suivantes reconnaissent les codes VIN correspondant à la marque Honda. Ils sont plus complexes que les exemples précédents mais illustrent bien l'efficacité de ce type de grammaire.

<table style="width:50%">
  <caption>Tableau 4. Grammaires de codes VIN</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.abnf" download="vins.abnf">vins.abnf <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a>.
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.xml" download="vins.xml">vins.xml <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a>.
    </td>
  </tr>
</table>

Pour plus d'informations sur le format VIN, voir [Vehicle identification number](https://wikipedia.org/wiki/Vehicle_identification_number){: external}.

## Grammaires avec des éléments facultatifs
{: #optionalElements}

En rendant des éléments de réponse facultatifs, vous pouvez obtenir des grammaires plus flexibles en anticipant les réponses possibles des utilisateurs. La grammaire suivante met l'élément `$optionalphrase` entre crochets pour le rendre facultatif. L'utilisateur peut dire quelques expressions supplémentaires avant d'indiquer le numéro de sécurité sociale. Par exemple, il peut dire "my social is xxx xx xxxx" ou juste "xxx xx xxxx".

<table style="width:50%">
  <caption>Tableau 5. Grammaire d'élément facultatif</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/optional.abnf" download="optional.abnf">optional.abnf <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a>
    </td>
    <td style="text-align:center">
      Non disponible
    </td>
  </tr>
</table>

Pour plus d'informations sur les extensions facultatives dans les grammaires, voir [Section 2.5 Repeats](https://www.w3.org/TR/speech-grammar/#S2.5){: external} dans le document Speech Recognition Grammar Specification.

## Grammaires de désambiguïsation
{: #disambiguation}

D'autres exemples de grammaires possibles comprennent des situations où le niveau de confiance de la réponse obtenue dans la reconnaissance n'est pas très élevé. L'application doit alors s'assurer que la réponse de l'utilisateur est exacte. Dans ce cas, l'application peut créer de manière dynamique une grammaire incluant les réponses les plus plausibles et demander à l'utilisateur de répéter la réponse. Par exemple, l'application peut créer une grammaire incluant les deux options les plus plausibles et demander à l'utilisateur d'en choisir une : `Did you mean 'option 1' or 'option 9'?`

Les grammaires de désambiguïsation sont générées à l'aide d'un programme. Aucun exemple n'est fourni.
