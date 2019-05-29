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

# Description des résultats de reconnaissance
{: #basic-response}

Quelle que soit l'interface que vous utilisez, le service {{site.data.keyword.speechtotextfull}} renvoie les résultats de la transcription conformément aux paramètres d'entrée et de sortie que vous spécifiez. Le service renvoie le contenu de toutes les réponses JSON en jeu de caractères UTF-8.
{: shortdesc}

## Réponse de transcription de base
{: #response}

Le service renvoie la réponse suivante pour les exemples figurant dans la rubrique [Effectuer une demande de reconnaissance](/docs/services/speech-to-text/basic-request.html). Les exemples transmettent un seul fichier audio et son type de contenu. La séquence audio est constituée d'une seule phrase parlée sans pause perceptible entre les mots.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Le service renvoie un objet `SpeechRecognitionResults`, qui est l'objet de réponse de niveau supérieur. Pour ces demandes simples, l'objet comprend une zone `results` et une zone `result_index` :

-   La zone `results` fournit un tableau d'informations sur les résultats de la transcription. Dans cet exemple, la zone `alternatives` comprend les paramètres `transcript` et le niveau de confiance (`confidence`) du service dans les résultats. La zone `final` a la valeur `true` pour indiquer que ces résultats ne sont pas modifiables.
-   La zone `result_index` fournit un identificateur unique pour les résultats. L'exemple montre les résultats finaux d'une demande avec un seul fichier audio sans pause, et la demande ne comprend pas de paramètres supplémentaires. Par conséquent, le service renvoie une seule zone `result_index` avec la valeur `0`, qui correspond toujours à l'index initial.

Si l'entrée audio est plus complexe ou si la requête comprend d'autres paramètres, les résultats peuvent contenir beaucoup plus d'informations.

### Zone alternatives
{: #responseAlternatives}

La zone `alternatives` fournit une série de résultats de transcription. Pour cette demande, le tableau ne comprend qu'un seul élément.

-   La zone `confidence` est une cote qui indique la confiance du service en la retranscription, dans cet exemple, sa valeur avoisine 90 pour cent.
-   La zone `transcript` fournit les résultats de la transcription.

Les zones `final` et `result_index` qualifient la signification de ces zones.

### Zone final
{: #responseFinal}

La zone `final` indique si la retranscription présente les résultats finaux de la transcription :

-   La zone a la valeur `true` s'il s'agit des résultats finaux, ce qui garantit qu'ils ne changeront pas. Le service n'envoie plus aucune mise à jour pour les retranscriptions qu'il renvoie sous forme de résultats finaux.
-   La zone a la valeur `false` s'il s'agit de résultats intermédiaires, qui sont susceptibles de changer. Si vous utilisez le paramètre `interim_results` avec l'interface WebSocket, le service renvoie des hypothèses intermédiaires fluctuantes sous la forme de plusieurs zones `results` au fur et à mesure de la transcription audio. La zone `final` a toujours la valeur `false` quand il s'agit de résultats intermédiaires. Le service définit la zone avec la valeur `true` s'il s'agit des résultats finaux des données audio. Le service n'envoie plus de mise à jour pour la transcription de cette séquence audio.

Pour obtenir des résultats intermédiaires, utilisez l'interface WebSocket et définissez le paramètre `interim_results` avec la valeur `true`. Pour plus d'informations, voir [Résultats intermédiaires](/docs/services/speech-to-text/output.html#interim).

### Zone result_index
{: #responseResultIndex}

La zone `result_index` fournit un identificateur pour les résultats qui sont propres à cette demande. Si vous demandez des résultats intermédiaires, le service envoie plusieurs zones `results` présentant l'évolution des différentes hypothèses de l'entrée audio. Les index des résultats intermédiaires des mêmes données audio ont toujours la même valeur, tout comme les résultats finaux de ces mêmes données.

Après avoir reçu les résultats finaux de données audio, le service n'envoie pas d'autres résultats avec cet index dans la suite de la demande. L'index de résultats supplémentaires est incrémenté d'une unité.

Que vous ayez ou non demandé des résultats intermédiaires, le service peut renvoyer plusieurs résultats finaux avec différents index si vos données audio comprennent des pauses ou des périodes de silence prolongé. Pour plus d'informations, voir [Pauses et silence](#pauses-silence).

Si vos données audio produisent plusieurs résultats finaux, concaténez les éléments `transcript` des résultats finaux pour constituer la transcription complète des données audio. Assemblez les résultats par ordre d'index de résultat (`result_index`). Vous pouvez ignorer les résultats intermédiaires dont la zone `final` a la valeur `false`.

### Contenu de réponse supplémentaire
{: #responseAdditional}

De nombreux paramètres de sortie disponibles pour la reconnaissance vocale ont une incidence sur le contenu de la réponse du service. Par exemple, les paramètres suivants peuvent obtenir du service le renvoi d'informations supplémentaires sur les données audio :

-   Le paramètre `max_alternatives` demande plusieurs autres retranscriptions possibles. Le tableau `alternatives` comprend un élément distinct pour chaque alternative. Chaque élément du tableau comprend une zone `transcript`. Seule l'alternative la plus plausible comporte une zone `confidence`.
-   Les paramètres `word_confidence` et `timestamps` demandent des informations supplémentaires sur des mots individuels de la retranscription. Le tableau `alternatives` comprend d'autres zones avec ces noms.
-   Les paramètres `keywords` et `keywords_threshold` demandent la détection de mots clés. Le tableau `results` comprend une zone `keywords_result`.
-   Le paramètre `word_alternatives_threshold` demande des résultats acoustiques similaires pour des mots individuels. Le tableau `results` comprend une zone `word_alternatives`.
-   Le paramètre `interim_results` de l'interface WebSocket demande des hypothèses de transcription intermédiaires. Comme indiqué précédemment, le service envoie plusieurs réponses au fur et à mesure de la transcription audio.
-   Le paramètre `speaker_labels` identifie les locuteurs individuels d'un échange à plusieurs intervenants. La réponse comprend une zone `speaker_labels` au même niveau que les zones `results` et `results_index`.

Pour plus d'informations sur ces paramètres et d'autres paramètres susceptibles d'affecter la réponse du service, voir [Fonctions de sortie](/docs/services/speech-to-text/output.html). Pour plus d'informations sur tous les paramètres disponibles, voir [Récapitulatif des paramètres](/docs/services/speech-to-text/summary.html).

## Pauses et silence
{: #pauses-silence}

Le service transcrit un flux audio complet jusqu'à la fin du flux ou en cas d'expiration d'un délai d'attente. Cependant si le flux audio comprend des pauses ou des périodes de silence prolongé entre des mots ou des expressions, les résultats de reconnaissance peuvent inclure plusieurs résultats finaux.

L'intervalle de pause par défaut utilisé par le service pour déterminer des résultats finaux distincts est d'environ une seconde. Pour la plupart des langues, l'intervalle de pause est précisément de 0,8 seconde. Pour le chinois il est de 0,6 seconde. Vous ne pouvez pas modifier la durée de l'intervalle de pause.

La façon dont le service renvoie les résultats dépend de l'interface que vous utilisez. Les exemples suivants illustrent des réponses avec deux résultats finaux issus des interfaces HTTP et WebSocket. La même entrée audio est utilisée dans les deux cas. La séquence audio comporte la phrase parlée "one two three four five six," avec une seconde de pause entre les mots "three" et "four."

-   *Pour les interfaces HTTP,* le service envoie un seul objet `SpeechRecognitionResults`. Le tableau `alternatives` comporte un élément distinct pour chaque résultat final. La réponse comporte une seule zone `result_index` avec la valeur `0`.

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
        {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

-   *Pour l'interface WebSocket,* le service envoie deux réponses distinctes avec deux objets `SpeechRecognitionResults` différents. Chaque objet de réponse comporte une zone `result_index`, avec la valeur `0` pour la première réponse et `1` pour la deuxième réponse.

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.99,
              "transcript": "one two three "
            }
          ],
          "final": true
        },
      ]
      "result_index": 0
    }
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.98,
              "transcript": "four five six "
            }
          ],
          "final": true
        }
      ],
      "result_index": 1
    }
    ```
    {: codeblock}

Si vos résultats comprennent plusieurs résultats finaux, concaténez les éléments `transcript` des résultats finaux pour constituer la transcription complète des données audio. 

Un silence de 30 secondes dans un flux audio diffusé en continu peut entraîner l'expiration du [délai d'attente d'inactivité](/docs/services/speech-to-text/input.html#timeouts-inactivity).
{: note}

## Marqueurs d'hésitation
{: #hesitation}

Le service peut inclure des marqueurs d'hésitation dans une retranscription lorsqu'il détecte des petits mots de remplissage ou des pauses dans la séquence vocale. Egalement désignés par disfluences, les pauses de ce type peuvent comprendre des mots de remplissage, tels que "uhm", "uh", "hmm" et des énoncés non lexicaux associés. A moins d'en avoir besoin pour votre application, vous pouvez filtrer les marqueurs d'hésitation d'une retranscription.

En anglais, le service utilise le jeton d'hésitation `%HESITATION`, comme indiqué dans l'exemple suivant. D'autres langues peuvent utiliser des marqueurs différents, par exemple, le japonais utilise le jeton `D_`.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

Les marqueurs d'hésitation peuvent également apparaître dans d'autres zones d'une retranscription. Par exemple, si vous demandez des [horodatages de mots](/docs/services/speech-to-text/output.html#word_timestamps) pour les mots individuels d'une retranscription, le service signale le début et la fin de chaque marqueur d'hésitation.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            . . .
            [
              "that",
              7.31,
              7.69
            ],
            [
              "%HESITATION",
              7.69,
              7.98
            ],
            [
              "that's",
              7.98,
              8.41
            ],
            [
              "a",
              8.41,
              8.48
            ],
            . . .
          ],
          "confidence": 0.99,
          "transcript": ". . . that %HESITATION that's a . . ."
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Capitalisation
{: #capitalization}

Pour la plupart des langues, le service n'utilise pas de majuscules dans les retranscriptions des réponses. Si la capitalisation est importante pour votre application, vous devez capitaliser le premier mot de chaque phrase ainsi que d'autres termes pour lesquels la capitalisation est appropriée.

*Pour l'anglais américain uniquement,* le service capitalise de nombreux noms propres. Par exemple, le service renvoie le texte suivant pour l'expression spécifiée :

```
Barack Obama graduated from Columbia University
```
{: codeblock}

Pour d'autres langues, le service renvoie le texte suivant :

```
barack obama graduated from columbia university
```
{: codeblock}

Le service applique toujours cette capitalisation à l'anglais américain, que vous utilisiez ou non le formatage intelligent.

## Ponctuation
{: #punctuation}

Par défaut, le service n'insère pas de ponctuation dans les retranscriptions de réponses. Vous devez ajouter la ponctuation nécessaire dans les résultats du service.

Pour l'anglais américain, vous pouvez utiliser le formatage intelligent pour demander au service de remplacer les signes de ponctuation, par exemple les virgules, les points, les points d'interrogation et les points d'exclamation, pour certaines chaînes de mots clés. Pour plus d'informations, voir [Formatage intelligent](/docs/services/speech-to-text/output.html#smart_formatting).
