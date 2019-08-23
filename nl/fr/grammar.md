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

# Utilisation de grammaires avec des modèles de langue personnalisés
{: #grammars}

Le service {{site.data.keyword.speechtotextfull}} prend en charge l'utilisation de grammaires avec des modèles de langue personnalisés. Vous pouvez ajouter des grammaires à un modèle de langue personnalisé et les utiliser pour la reconnaissance vocale. Les grammaires limitent l'ensemble d'expressions reconnaissables par le service dans les données audio.
{: shortdesc}

Une grammaire utilise une spécification linguistique formelle pour définir un ensemble de règles de production pour transcrire les chaînes. Ces règles spécifient comment former des chaînes valides à partir de l'alphabet de la langue concernée. Lorsque vous appliquez une grammaire à la reconnaissance vocale, le service ne peut renvoyer qu'une ou plusieurs expressions générées par la grammaire.

Par exemple, lorsque vous devez reconnaître des mots ou des expressions spécifiques, par exemple *yes* ou *no*, des lettres ou des nombres individuels, ou encore une liste de noms, l'utilisation de grammaires peut être plus efficace qu'examiner des retranscriptions ou d'autres propositions de mots. De plus, en limitant l'espace de recherche aux chaînes valides, le service peut délivrer des résultats plus rapides et plus précis.

Lorsque vous utilisez un modèle de langue personnalisé et une grammaire pour la reconnaissance vocale, le service peut renvoyer une expression valide de la grammaire ou un résultat vide. Si le résultat n'est pas vide, le service inclut une cote de confiance avec la retranscription finale, comme pour toutes les demandes de reconnaissance. Pour les grammaires, cette cote indique la probabilité de correspondance de la réponse par rapport à la grammaire. La possibilité de résultats positifs erronés est toujours possible, notamment pour les grammaires simples, par conséquent vous devez toujours considérer le niveau de confiance des résultats du service lorsque vous évaluez sa réponse.

La fonction correspondant aux grammaires est une fonctionnalité bêta. Le service prend en charge les grammaires pour toutes les langues compatibles avec la personnalisation de modèle de langue. Pour plus d'informations, voir [Support de langue pour la personnalisation](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport).
{: note}

## Formats de grammaire pris en charge
{: #grammarFormats}

Le service {{site.data.keyword.speechtotextshort}} prend en charge les grammaires définies aux formats standard suivants :

-   *Augmented Backus-Naur Form (ABNF)* - ce format utilise une représentation plain-text similaire à la grammaire BNF classique. Le type de média correspondant à ce format est `application/srgs`.
-   *Format XML* - ce format utilise des éléments XML pour représenter la grammaire. Le type de média correspondant à ce format est `application/srgs+xml`.

Ces deux formats de grammaire ont la puissance expressive d'une grammaire hors-contexte (CFG). Cependant, le service ne peut décoder que des grammaires régulières de Type 3 dans la hiérarchie de Chomsky. Les grammaires de ce type représentent des automates finis.

Pour obtenir des informations générales sur les grammaires, voir les pages Wikipedia suivantes :

-   [Spécification SRGS (Speech Recognition Grammar Specification)](https://wikipedia.org/wiki/Speech_Recognition_Grammar_Specification){: external}
-   [Format ABNF (Augmented Backus-Naur Form)](https://wikipedia.org/wiki/Augmented_Backus%E2%80%93Naur_form){: external}
-   [Hiérarchie Chomsky](https://wikipedia.org/wiki/Chomsky_hierarchy){: external}

## Spécification SRGS (Speech Recognition Grammar Specification)
{: #grammarSpecification}

Le service {{site.data.keyword.speechtotextshort}} prend en charge les grammaires définies par la norme W3C [Speech Recognition Grammar Specification Version 1.0](https://www.w3.org/TR/speech-grammar/){: external}. Cette spécification fournit des informations détaillées sur les formats pris en charge et indique comment définir une grammaire. Pour plus d'informations sur les types de média pris en charge, voir la section [Appendix G. Media Types and File Suffix](https://www.w3.org/TR/speech-grammar/#AppG){: external} de la spécification.

Actuellement, le service *ne prend pas en charge* toutes les fonctions de la spécification SRGS. Plus précisément, il ne prend pas en charge les fonctions décrites dans les sections suivantes de la spécification :

-   [Section 1.4 Semantic Interpretation](https://www.w3.org/TR/speech-grammar/#S1.4){: external}. {{site.data.keyword.IBM_notm}} s'efforce de trouver une solution pour prendre en charge cette fonction dans une édition ultérieure du service.
-   [Section 1.5 Embedded Grammars](https://www.w3.org/TR/speech-grammar/#S1.5){: external}. {{site.data.keyword.IBM_notm}} s'efforce de trouver une solution pour prendre en charge cette fonction dans une édition ultérieure du service.
-   [Section 2.2.2 External Reference by URI](https://www.w3.org/TR/speech-grammar/#S2.2.2){: external}. Le service prend uniquement en charge les références locales, conformément aux indications de la [Section 2.2.1 Local References](https://www.w3.org/TR/speech-grammar/#S2.2.1){: external}. En d'autres termes, une grammaire doit être autonome.
-   [Section 2.2.3 Special Rules](https://www.w3.org/TR/speech-grammar/#S2.2.3){: external}.
-   [Section 2.2.4 Referencing N-gram Documents (Informative)](https://www.w3.org/TR/speech-grammar/#S2.2.4){: external}.
-   [Section 2.7 Language](https://www.w3.org/TR/speech-grammar/#S2.7){: external}. Le service ne prend pas en charge le changement de langue. Il ne prend en charge qu'une langue globale par grammaire.

Les mots de la grammaire doivent être codés en UTF-8 (ASCII est un sous-ensemble du codage UTF-8). L'utilisation d'un autre codage peut provoquer des erreurs lors de la compilation de la grammaire ou des résultats imprévisibles lors du décodage. Le service ignore le codage spécifié dans l'en-tête de la grammaire.
{: note}
