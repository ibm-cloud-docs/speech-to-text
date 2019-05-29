---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

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

# Description des grammaires
{: #grammarUnderstand}

Les exemples suivants présentent le support des grammaires du service {{site.data.keyword.speechtotextfull}}. Dans ces exemples, deux grammaires ABNF simples sont créées et les résultats possibles sont affichés lorsqu'elles sont utilisées dans le cadre de la reconnaissance vocale. Ces exemples illustrent l'importance de la cote de confiance que le service inclut dans une retranscription.
{: shortdesc}

Ces exemples fournissent uniquement les résultats des demandes de reconnaissance vocale. Pour consulter des exemples illustrant comment transmettre une grammaire pour la reconnaissance vocale, voir [Utilisation d'une grammaire pour la reconnaissance vocale](/docs/services/speech-to-text/grammar-use.html). Ces exemples sont également très simples. Pour obtenir des exemples de grammaire plus complexes, voir [Exemples de grammaires](/docs/services/speech-to-text/grammar-examples.html).

## Correspondances à une seule expression : la grammaire yesno
{: #yesnoGrammar}

Le premier exemple définit une grammaire `yesno` très simple qui accepte deux réponses contenant chacune un mot unique, `yes` et `no`. La grammaire s'avère utile dans les cas où l'utilisateur doit répondre avec une seule de ces deux expressions.

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $yesno;

$yesno = yes | no ;
```
{: codeblock}

Lorsque vous appliquez cette grammaire à une demande de reconnaissance vocale, le service peut renvoyer une retranscription avec une cote indiquant le niveau de confiance qu'il attribue à la correspondance. Il est possible qu'il ne renvoie aucun résultat si l'entrée ne correspond pas à l'une des deux expressions.

Par exemple, si l'utilisateur répond `yes`, le service renverra une réponse correspondant très probablement au résultat suivant. La valeur indiquée dans la zone `confidence` indique une correspondance parfaitement fiable.

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

Mais supposons, par exemple, que l'utilisateur réponde par `nope`. Le service peut renvoyer un résultat avec une cote de confiance très faible ou aucun résultat. Un résultat vide est l'indication très nette que la réponse ne correspond pas à la grammaire. Vous avez toutes les chances d'obtenir une réponse vide avec des grammaires plus complexes, où une réponse valide doit correspondre à une séquence précise d'expressions multiples.

## Correspondances à plusieurs expressions : la grammaire names
{: #namesGrammar}

Avec une grammaire contenant plusieurs expressions, la réponse de l'utilisateur doit être complète pour faire l'objet d'une reconnaissance. L'utilisateur ne peut pas oublier un mot ou s'arrêter au milieu de la réponse. L'absence d'un seul mot peut amener le service à renvoyer un résultat vide.

De plus, le service peut renvoyer plusieurs retranscriptions si l'utilisateur prononce des expressions séparées par des périodes de silence suffisantes pour indiquer qu'il s'agit d'énoncés indépendants. Par exemple, examinons la grammaire simple `names`, qui permet d'établir la correspondance d'un mot sur des noms constitués de trois mots.

```
#ABNF 1.0 ISO-8859-1;
language en-US;
mode voice;
root $names;
$names = Yi Wen Tan | Yon See | Youngjoon Lee ;
```
{: codeblock}

Supposons que l'utilisateur cite l'un des noms mentionnés dans les règles de cette grammaire, `Yon See`. Le service renvoie une réponse indiquant un niveau de confiance très élevé pour la correspondance.

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

Supposons maintenant que l'utilisateur cite deux noms séparés par une période de silence prononcée, d'au moins 0,8 seconde, pour indiquer qu'il s'agit d'énoncés distincts : `Yon See` [1,0 seconde de silence] `Yi Wen Tan`. Dans ce cas, le service envoie deux réponses distinctes avec une cote de confiance différente pour chaque retranscription.

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

Et enfin, considérons le cas où l'utilisateur, dit à la place quelque chose comme `Yon See` [1,0 seconde de silence] `Young Says He`. Pour la première expression, `Yon See`, le service envoie une correspondance positive avec une cote de confiance, à l'instar des exemples précédents. Pour la seconde expression, `Young Says He`, le service peut donner l'une des deux réponses possibles suivantes :

-   Soit aucune réponse, ce qui indique que l'expression ne correspond pas à l'une des règles de la grammaire.
-   Il peut aussi envoyer une réponse avec une cote de confiance faible, ce qui indique que l'expression est similaire acoustiquement à l'une des règles mais qu'elle ne constitue probablement pas une correspondance.

## Cotes de confiance et résultats vides
{: #confidenceScores}

Pour les grammaires relativement simples comme celles illustrées dans les exemples précédents, le service peut renvoyer un résultat avec une cote de confiance faible même si la réponse ne semble pas du tout correspondre à la grammaire. Cela peut sembler surprenant, mais une réponse avec une cote de confiance faible peut indiquer la meilleure correspondance que peut trouver le service pour les données audio fournies, même si la réponse a peu de chance de correspondre à la grammaire. Si la réponse ne correspond pas à la grammaire, la valeur de confiance des résultats est suffisamment faible pour indiquer que la grammaire a peu de chance de générer une réponse.

Tenez toujours compte de la cote de confiance pour évaluer si une réponse satisfait aux critères de la grammaire. Si vous ne savez pas quel est le seuil à définir pour le rejet d'un résultat en fonction de sa cote de confiance, utilisez la valeur initiale 0.5. Si vous êtes surpris d'obtenir des taux de rejet particulièrement importants de résultats pourtant corrects, diminuez le seuil de confiance. Par exemple, 0.1 peut être un bon choix pour certaines applications. Si vous obtenez un grand nombre de reconnaissances incorrectes, augmentez le seuil de confiance pour votre application.

Dans de nombreux cas, un résultat vide ou un résultat avec une cote de confiance très faible constitue une réponse valide. C'est parfois le signe que l'utilisateur n'a pas compris la question ou les options disponibles. Vous pouvez toujours reconnaître la réponse audio de l'utilisateur sans tenir compte de la grammaire, mais vous risquez alors d'obtenir un résultat non valide par rapport à la grammaire. Les grammaires fournissent des résultats plus fiables que la méthode n-grammes qui renvoie toujours un résultat pour tout ce qui n'est pas du silence complet.

Savoir que le résultat doit être l'une des expressions valides définies par une grammaire est un atout efficace qui peut simplifier le traitement des réponses d'une application. En règle générale, le service peut renvoyer des résultats avec un degré de confiance élevé uniquement pour les énoncés générés par une grammaire. Pour que la grammaire `yesno` illustrée dans le premier exemple reconnaisse sans hésitation l'expression `nope` comme une réponse valide, vous devez ajouter cette expression dans la grammaire.
