---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-10"

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

# Fonctions de sortie
{: #output}

Le service {{site.data.keyword.speechtotextfull}} offre les fonctions suivantes pour indiquer les informations que le service doit inclure dans ses résultats de transcription pour une demande de reconnaissance vocale. Tous les paramètres de sortie sont facultatifs.
{: shortdesc}

-   Pour obtenir des exemples de demandes de reconnaissance vocale simples pour chacune des interfaces du service, voir [Effectuer une demande de reconnaissance](/docs/services/speech-to-text?topic=speech-to-text-basic-request).
-   Pour obtenir des exemples et des descriptions de réponses de reconnaissance vocale, voir [Description des résultats de reconnaissance](/docs/services/speech-to-text?topic=speech-to-text-basic-response). Le service renvoie le contenu de toutes les réponses JSON en jeu de caractères UTF-8.
-   Pour obtenir une liste alphabétique de tous les paramètres de reconnaissance vocale disponibles, y compris leur statut (GA ou bêta) et les langues prises en charge, voir [Récapitulatif des paramètres](/docs/services/speech-to-text?topic=speech-to-text-summary).

## Etiquettes de locuteur (Speaker labels)
{: #speaker_labels}

La fonction d'étiquettes de locuteur (speaker labels) est une fonctionnalité bêta disponible pour l'anglais américain, le japonais et l'espagnol (pour les modèles à large bande et bande étroite) et pour l'anglais britannique (modèle à bande étroite uniquement).
{: note}

Les éléments speaker_labels identifient qui sont les différentes personnes qui parlent et les mots qu'elles prononcent dans un échange à plusieurs participants. (Indiquer qui parle et à quel moment est parfois désigné par *diarisation du locuteur*.) Vous pouvez utiliser ces informations pour développer la retranscription d'un flux audio d'une personne à une autre, par exemple un contact à un centre d'appels. Vous pouvez aussi l'utiliser pour animer un échange avec un robot conversationnel ou un avatar. Pour obtenir des performances optimales, utilisez au moins une minute d'audio.

Les étiquettes de locuteur sont optimisées pour les scénarios à deux locuteurs. Elles fonctionnent le mieux pour les conversations téléphoniques impliquant deux personnes dans un échange prolongé. Elles peuvent gérer jusqu'à six locuteurs, mais les résultats peuvent avoir des performances variables s'il y a plus de deux locuteurs. Les échanges à deux personnes sont en principe régies sur un support à bande étroite, mais vous pouvez utiliser les étiquettes de locuteur avec les modèles à large bande ou à bande étroite pris en charge.

Pour utiliser cette fonction, vous définissez le paramètre `speaker_labels` avec la valeur `true` pour une demande de reconnaissance ; le paramètre a la valeur `false` par défaut. Le service identifie les locuteurs par des mots audio individuels. Il s'appuie sur les moments de début et de fin d'un mot pour identifier son locuteur. Par conséquent, l'activation de la fonction d'étiquettes de locuteur force également le paramètre `timestamps` à avoir la valeur `true` (voir [Horodatages des mots](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps)).

### Exemple d'étiquettes de locuteur
{: #speakerLabelsExample}

L'exemple de requête suivant illustre une réponse incluant des étiquettes de locuteur (speaker_labels). Les valeurs numériques associées à chaque élément du tableau `timestamps` représentent les moments de début et de fin du mot dans l'audio.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-multi.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel&speaker_labels=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "hello",
              0.68,
              1.19
            ],
            [
              "yeah",
              1.47,
              1.93
            ],
            [
              "yeah",
              1.96,
              2.12
            ],
            [
              "how's",
              2.12,
              2.59
            ],
            [
              "Billy",
              2.59,
              3.17
            ],
            . . .
          ]
          "confidence": 0.82,
          "transcript": "hello yeah yeah how's Billy "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0,
  "speaker_labels": [
    {
      "from": 0.68,
      "to": 1.19,
      "speaker": 2,
      "confidence": 0.42,
      "final": false
    },
    {
      "from": 1.47,
      "to": 1.93,
      "speaker": 1,
      "confidence": 0.52,
      "final": false
    },
    {
      "from": 1.96,
      "to": 2.12,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.12,
      "to": 2.59,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    {
      "from": 2.59,
      "to": 3.17,
      "speaker": 2,
      "confidence": 0.41,
      "final": false
    },
    . . .
  ]
}
```
{: codeblock}

La zone `transcript` présente la retranscription finale de l'audio, qui répertorie les mots tels qu'ils sont énoncés par tous les participants. En comparant et en synchronisant les étiquettes de locuteur avec les horodatages, vous pouvez reconstituer la conversation telle qu'elle s'est déroulée.

<table style="width:50%">
  <caption>Tableau 1. Exemple d'étiquettes de locuteur</caption>
  <tr>
    <th style="text-align:left">Horodatage</th>
    <th style="text-align:left">Etiquette de locuteur</th>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"hello",<br/>
      0.68,<br/>
      1.19</code>
    </td>
    <td>
      <code>"from": 0.68,<br/>
      "to": 1.19,<br/>
      "speaker": 2,<br/>
      "confidence": 0.42,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.47,<br/>
      1.93</code>
    </td>
    <td>
      <code>"from": 1.47,<br/>
      "to": 1.93,<br/>
      "speaker": 1,<br/>
      "confidence": 0.52,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"yeah",<br/>
      1.96,<br/>
      2.12</code>
    </td>
    <td>
      <code>"from": 1.96,<br/>
      "to": 2.12,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"how's",<br/>
      2.12,<br/>
      2.59</code>
    </td>
    <td>
      <code>"from": 2.12,<br/>
      "to": 2.59,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
  <tr>
    <td style="vertical-align:top">
      <code>"Billy",<br/>
      2.59,<br/>
      3.17</code>
    </td>
    <td>
      <code>"from": 2.59,<br/>
      "to": 3.17,<br/>
      "speaker": 2,<br/>
      "confidence": 0.41,<br/>
      "final": false</code>
    </td>
  </tr>
</table>

Les résultats indiquent clairement comment a commencé l'échange entre ces deux personnes :

-   **Speaker 2** - "Hello?"
-   **Speaker 1** - "Yeah?"
-   **Speaker 2** - "Yeah, how's Billy?"

Dans cet exemple, les zones `confidence` indiquent la confiance du service en son identification des locuteurs. La zone `final` a la valeur `false` pour chaque mot. Cette valeur indique que le service est toujours en cours de traitement de l'audio. Le service peut changer son identification du locuteur ou sa confiance pour des mots individuels avant la fin.

### ID des locuteurs correspondant aux étiquettes de locuteur
{: #speakerLabelsIDs}

L'exemple illustre également un aspect intéressant lié aux ID des locuteurs. Dans les zones `speaker`, le premier locuteur a l'ID `2` et le second a l'ID `1`. Le service développe une meilleure connaissance des modèles de locuteur à mesure qu'il traite l'audio. Par conséquent, il peut changer les ID des locuteurs pour des mots individuels et peut également ajouter et supprimer des locuteurs, jusqu'à ce qu'il génère ses premiers résultats finaux.

Par conséquent, les ID des locuteurs ne sont pas forcément séquentiels, contigus ou ordonnés. Par exemple, les ID commencent en principe par `0`, mais dans l'exemple précédent, le premier ID avait la valeur `1`. Le service a sans doute omis une première affectation de mot pour le locuteur `0` en fonction d'une analyse audio approfondie. Ces omissions peuvent survenir également pour d'autres locuteurs par la suite. Les omissions peuvent se traduire par des écarts dans la numérotation et produire des résultats pour deux locuteurs avec, par exemple, les étiquettes `0` et `2`. Il est également possible que les locuteurs quittent la conversation. Ainsi, un participant qui ne contribue qu'au tout début d'une conversation peut disparaître dans les résultats ultérieurs.

### Demande de résultats intermédiaires pour les étiquettes de locuteur
{: #speakerLabelsInterim}

Avec l'interface WebSocket, vous pouvez demander des résultats intermédiaires ainsi que des étiquettes de locuteur (voir [Résultats intermédiaires](/docs/services/speech-to-text?topic=speech-to-text-output#interim)). Les résultats finaux sont en principe meilleurs que les résultats intermédiaires. Mais les résultats intermédiaires peuvent aider à identifier l'évolution d'une retranscription et l'affectation des étiquettes de locuteur. Les résultats intermédiaires peuvent indiquer à quel endroit les locuteurs de passage et les ID sont apparus ou ont disparu. Cependant, le service peut réutiliser les ID des locuteurs qu'il a identifiés initialement et les reconsidérer et les omettre par la suite. C'est pourquoi un ID peut se référer à deux locuteurs différents dans les résultats intermédiaires et les résultats finaux.

Lorsque vous demandez des résultats intermédiaires et des étiquettes de locuteur, les résultats finaux de flux audio longs peuvent arriver bien après le renvoi des premiers résultats intermédiaires. Il est également possible que certains résultats intermédiaires ne comprennent qu'une zone `speaker_labels` sans les zones `results` et `result_index`. Si vous ne demandez pas de résultats intermédiaires, le service renvoie les résultats finaux qui comprennent les zones `results` et `result_index`, avec une seule zone `speaker_labels`.

Le service envoie les résultats finaux lorsque le flux audio est complet ou en réponse à un délai d'attente, selon le cas. Le service définit la zone `final` avec la valeur `true` uniquement pour le dernier mot des étiquettes de locuteur qu'il renvoie dans les deux cas de figure.

### Facteurs de performance pour les étiquettes de locuteur
{: #speakerLabelsPerformance}

Comme indiqué précédemment, la fonction d'étiquettes de locuteur est optimisée pour les conversations entre deux personnes, telles que les communications avec un centre d'appel. Par conséquent, vous devez tenir compte des problèmes potentiels suivants en matière de performances :

-   Les performances audio avec un seul locuteur peuvent être médiocres. Les variations de qualité audio ou dans la voix du locuteur peuvent pousser le service à identifier d'autres locuteurs qui ne sont pas présents. Ces locuteurs sont désignés comme étant des hallucinations.
-   De même, les performances audio avec un locuteur dominant, par exemple un podcast, peuvent être médiocres. Le service a tendance à ignorer des locuteurs qui parlent pendant très peu de temps, ce qui peut également produire des hallucinations.
-   Les performances audio avec plus de six locuteurs ne sont pas définies. La fonction peut gérer un maximum de six locuteurs.
-   Les performances des énoncés courts peuvent être moins précises que celles des énoncés longs. Le service fournit de meilleurs résultats lorsque les participants parlent plus longtemps, au moins 30 secondes chacun. La durée audio relative disponible pour chaque locuteur peut également affecter les performances.
-   Les performances peuvent se dégrader pendant les 30 premières secondes de conversation. En principe, elles s'améliorent à un niveau raisonnable au bout d'1 minute d'audio.

Comme pour toutes les transcriptions, les performances peuvent également être affectées par une mauvaise qualité audio, des bruits de fond, la façon de parler d'une personne, des locuteurs qui parlent en même temps et d'autres aspects liés à la séquence audio.

## Détection de mots clés (Keyword spotting)
{: #keyword_spotting}

La fonction de détection de mots clés (keyword spotting) détecte des chaînes spécifiées dans une retranscription. Le service peut détecter le même mot clé plusieurs fois et signaler chaque occurrence. Le service détecte les mots clés uniquement dans les résultats finaux et non pas dans les résultats intermédiaires. Par défaut, le service ne détecte pas les mots clés.

Pour utiliser la détection de mots clés, vous devez spécifier les deux paramètres suivants :

-   Utilisez le paramètre `keywords` pour indiquer un tableau de chaînes à détecter. Le service ne détecte pas les mots clés si vous omettez ce paramètre ou indiquez un tableau vide. Une chaîne de mots clés peut inclure plusieurs sèmes. Par exemple, le mot clé `Speech to Text` comporte trois sèmes.

    Pour l'anglais américain, le service normalise chaque mot clé pour établir une correspondance entre mots parlés et chaînes écrites. Par exemple, il normalise les nombres pour mettre en corrélation leur prononciation par rapport à leur écriture. Pour d'autres langues, les mots clés doivent être spécifiés comme ils se prononcent.

    Vous pouvez détecter jusqu'à 1000 mots clés maximum dans une seule demande. Les mots clés ne sont pas sensibles à la casse.
-   Utilisez le paramètre `keywords_threshold` pour spécifier une probabilité entre 0,0 et 1,0 pour une occurrence de mot clé. Le seuil indique la limite inférieure du niveau de confiance que le service doit avoir pour qu'un mot corresponde au mot clé. Un mot clé est détecté dans la retranscription uniquement si son niveau de confiance est supérieur ou égal au seuil spécifié.

    La spécification d'un seuil faible peut produire un grand nombre d'occurrences. Si vous spécifiez un seuil, vous devez également indiquer un ou plusieurs mots clés. Omettez ce paramètre pour qu'aucune occurrence ne soit renvoyée.

La détection de mots clés est nécessaire pour identifier des mots clés dans un flux audio. Vous ne pouvez pas identifier de mots clés en traitant une retranscription finale car cette retranscription représente les meilleurs résultats de décodage du service pour l'entrée audio. Elle n'inclut pas les sèmes avec des cotes de confiance plus faibles pouvant représenter un mot qui vous intéresse. Ainsi, l'application d'outils de traitement de texte à une retranscription côté client risque de ne pas identifier les mots clés. Une représentation enrichie des résultats de décodage est nécessaire, et cette représentation n'est disponible qu'au niveau du serveur.

### Résultats de la détection de mots clés
{: #keywordSpottingResults}

Le service renvoie les résultats dans une zone `keywords_result` correspondant à un élément du tableau `results`. La zone `keywords_result` correspond à un dictionnaire ou un tableau associatif, de propriétés dénombrables. Chaque propriété est identifiée par un mot clé indiqué et comprend un tableau d'objets. Le service renvoie un élément du tableau pour chaque occurrence du mot clé qu'il a détectée. L'objet de chaque occurrence comprend les zones suivantes :

-   `normalized_text` est le mot clé normalisé dans l'expression parlée concordante dans l'entrée audio.
-   `start_time` est le début en secondes de l'occurrence.
-   `end_time` est la fin en secondes de l'occurrence.
-   `confidence` est le niveau de confiance du service pour que l'occurrence représente le mot clé spécifié. Le niveau de confiance doit être au moins équivalent au seuil indiqué pour être inclus dans les résultats.

Un mot clé dont le service ne détecte aucune occurrence est omis dans le tableau. Il est possible de ne pas détecter un mot clé dans les cas suivants :

-   Le mot clé ne figure tout simplement pas dans l'audio. L'absence du mot clé est l'explication la plus plausible.
-   La valeur du seuil est trop élevée. Le service peut identifier le mot clé mais à un niveau de confiance moins élevé, auquel cas il omet l'occurrence dans les résultats.
-   Une chaîne de mots clés contenant plusieurs sèmes (par exemple, `Speech to Text`) est prononcée avec trop de moments de silence entre les différents sèmes qui la composent. Lorsque le service transcrit l'audio, il découpe le flux en une série de blocs. Chaque bloc représente un segment audio continu sans intervalle de silence dépassant une demi-seconde. Il construit un tableau des objets de résultat constitué de ces blocs.

    Le service fait correspondre un mot clé à plusieurs sèmes uniquement si

    -   Les sèmes du mot clé figurent dans le même bloc.
    -   Les sèmes sont adjacents ou séparés par un écart inférieur ou égal à 0,1 seconde.

    Le dernier cas de figure peut se produire si un petit mot de remplissage ou un énoncé non lexical, de type "hum" ou "bon," se trouve entre deux sèmes du mot clé. Pour plus d'informations, voir [Marqueurs d'hésitation](/docs/services/speech-to-text?topic=speech-to-text-basic-response#hesitation).

### Exemple de détection de mots clés
{: #keywordSpottingExample}

Dans l'exemple de demande suivant, le paramètre `keywords` est défini par un tableau avec trois chaînes en codage URL (`colorado`, `tornado` et `tornadoes`) et le paramètre `keywords_threshold` est défini avec la valeur `0.5`. Le service détecte des occurrences éligibles de `colorado` et `tornadoes`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?keywords=%22colorado%22%2C%22tornado%22%2C%22tornadoes%22&keywords_threshold=0.5"
```
{: pre}

```javascript
{
  "results": [
    {
      "keywords_result": {
        "colorado": [
          {
            "normalized_text": "Colorado",
            "start_time": 4.94,
            "confidence": 0.91,
            "end_time": 5.62
          }
        ],
        "tornadoes": [
          {
            "normalized_text": "tornadoes",
            "start_time": 1.52,
            "confidence": 1.0,
            "end_time": 2.15
          }
        ]
      },
      "alternatives": [
        {
          "confidence": 0.96,
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

## Nombre maximal d'alternatives (Max alternatives)
{: #max_alternatives}

Le paramètre `max_alternatives` accepte une valeur d'entier qui demande au service de renvoyer les *n* meilleures hypothèses pour les résultats. Par défaut, le service renvoie un seul résultat de transcription, ce qui équivaut à définir le paramètre avec la valeur `1`. En définissant le paramètre `max_alternatives` avec un nombre supérieur à 1, vous demandez au service de renvoyer ce nombre de meilleures transcriptions alternatives. (Si vous indiquez la valeur `0`, le service utilise la valeur par défaut `1`.)

Le service signale une cote de confiance uniquement pour la meilleure alternative qu'il renvoie. Dans la plupart des cas, il s'agit de l'alternative à choisir.

### Exemple de nombre maximal d'alternatives
{: #maximumAlternativesExample}

Dans l'exemple de demande suivant, le paramètre `max_alternatives` est défini avec la valeur `3` ; le service signale une cote de confiance pour les trois alternatives les plus probables.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?max_alternatives=3"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touched down as a line of
severe thunderstorms swept through Colorado on Sunday "
        },
        {
          "transcript": "several tornadoes touch down as a line
of severe thunderstorms swept through Colorado and Sunday "
            }
          ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Résultats intermédiaires (Interim results)
{: #interim}

La fonction de résultats intermédiaires (Interim results) est disponible uniquement avec l'interface WebSocket.
{: note}

Les résultats intermédiaires sont des hypothèses intermédiaires d'une transcription qui sont susceptibles de changer avant le renvoi des résultats finaux du service. Le service renvoie des résultats intermédiaires dès qu'il les génère. Les résultats intermédiaires sont utiles pour

-   les applications interactives et la transcription en temps réel
-   les flux audio longs, dont la transcription peut prendre un certain temps

Les résultats intermédiaires arrivent plus souvent et plus rapidement que les résultats finaux. Vous pouvez les utiliser pour permettre à votre application de répondre plus rapidement ou pour mesurer la progression de la transcription.

Pour recevoir des résultats intermédiaires, définissez le paramètre JSON `interim_results` avec la valeur `true` dans le message `start` pour une demande de reconnaissance WebSocket. Si vous omettez le paramètre `interim_results` ou si vous le définissez avec la valeur `false`, le service ne renvoie qu'une seule retranscription JSON à la fin de l'audio. Suivez ces instructions pour utiliser ce paramètre :

-   Omettez ce paramètre ou définissez-le avec la valeur `false` si vous effectuez une transcription hors ligne ou par lots, si vous n'avez pas besoin de temps d'attente minimum et que vous voulez un seul résultat JSON pour tout l'audio.
-   Définissez ce paramètre avec la valeur `true` si vous souhaitez que les résultats arrivent progressivement à mesure que le service traite l'audio ou si vous voulez des résultats avec un temps d'attente minimum. N'oubliez pas que le service peut mettre à jour les résultats intermédiaires en progressant dans le traitement audio.

Les résultats intermédiaires sont indiqués dans la réponse JSON avec la zone `final` définie avec la valeur `false`. Le service peut mettre à jour les résultats de ce type avec des transcriptions plus précises lorsqu'il poursuit son traitement de l'audio. Les résultats finaux sont identifiés avec la zone `final` définie avec la valeur `true`. Le service n'effectue plus de mise à jour dans les résultats finaux.

### Exemple de résultats intermédiaires
{: #interimResultsExample}

L'exemple abrégé suivant demande des résultats intermédiaires pour une demande WebSocket. Dans sa réponse, le service définit l'attribut `final` avec la valeur `true` uniquement pour le résultat final.

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token;
var websocket = new WebSocket(wsURI);

websocket.onopen = function(evt) { onOpen(evt) };
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050',
    interim_results: true
  };
  websocket.send(JSON.stringify(message));
  websocket.send(blob);
}

websocket.onmessage = function(evt) { onMessage(evt) };
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several to "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  . . .
}{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
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

## Autres propositions de mots (Word alternatives)
{: #word_alternatives}

La fonction d'autres propositions de mots (word alternatives), également désignée par réseaux de confusion (CN - Confusion Networks) renvoie des hypothèses de propositions de mots similaires d'un point de vue acoustique dans l'entrée audio. Par exemple, le mot `Austin` peut être la meilleure hypothèse d'un mot dans l'audio. Mais le mot `Boston` est une autre hypothèse possible dans le même intervalle de temps. Les hypothèses partagent un début commun et une fin commune mais ont différentes orthographes et en général différentes cotes de confiance.

Par défaut, le service ne signale pas d'autres propositions de mots. Pour indiquer que vous voulez recevoir d'autres propositions d'hypothèses, utilisez le paramètre `word_alternatives_threshold` pour indiquer une probabilité entre 0,0 et 1,0. Le seuil indique la limite inférieure du niveau de confiance que le service doit avoir dans une hypothèse pour la renvoyer comme autre proposition de mot. Une hypothèse n'est renvoyée que si le niveau de confiance est supérieur ou égal au seuil spécifié.

Vous pouvez envisager les autres propositions de mots (word alternatives) comme une chronologie d'une retranscription qui est découpée en intervalles plus petits ou en casiers. Chaque casier peut avoir une ou plusieurs hypothèses avec différentes orthographes et différents niveaux de confiance. Le paramètre `word_alternatives_threshold` contrôle la densité des résultats renvoyés par le service. La spécification d'un seuil faible peut produire un grand nombre d'hypothèses.

### Résultats des autres propositions de mots
{: #wordAlternativesResults}

Le service renvoie les résultats dans une zone `word_alternatives` correspondant à un élément du tableau `results`. La zone `word_alternatives` est un tableau d'objets, dont chaque objet fournit les zones suivantes pour une autre proposition de mot :

-   `start_time` indique le début en secondes d'un mot pour lequel des hypothèses sont renvoyées dans l'entrée audio.
-   `end_time` indique la fin en secondes d'un mot pour lequel des hypothèses sont renvoyées dans l'entrée audio.
-   `alternatives` est un tableau d'objets d'hypothèse. Chaque objet comprend une zone `confidence` qui indique la cote de confiance de l'hypothèse du service et une zone `word` qui identifie l'hypothèse. Cette cote doit être au moins équivalente au seuil indiqué pour figurer dans les résultats. Le service classe les propositions par confiance.

### Exemple d'autres propositions de mots
{: #wordAlternativesExample}

L'exemple de demande suivant indique un seuil `word_alternatives_threshold` correspondant à `0.9`. La sortie comprend les hypothèses potentielles et les cotes de confiance d'un certain nombre de mots, y compris leur début et leur fin.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.9"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 1.01,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "several"
            }
          ],
          "end_time": 1.52
        },
        {
          "start_time": 1.52,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "tornadoes"
            }
          ],
          "end_time": 2.15
        },
        {
          "start_time": 3.39,
          "alternatives": [
            {
              "confidence": 0.96,
              "word": "severe"
            }
          ],
          "end_time": 3.77
        },
        {
          "start_time": 3.77,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "thunderstorms"
            }
          ],
          "end_time": 4.51
        },
        {
          "start_time": 4.51,
          "alternatives": [
            {
              "confidence": 0.97,
              "word": "swept"
            }
          ],
          "end_time": 4.81
        }
      ],
      "alternatives": [
        {
          "confidence": 0.96,
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

## Confiance des mots (Word confidence)
{: #word_confidence}

Le paramètre `word_confidence` indique au service s'il doit fournir des mesures de confiance pour les mots de la retranscription. Par défaut, le service renvoie une mesure de confiance uniquement pour la retranscription finale dans son ensemble. Définir le paramètre `word_confidence` avec la valeur `true` demande au service d'indiquer une mesure de confiance pour chaque mot individuel de la retranscription.

Une mesure de confiance indique l'estimation du service sur la justesse du mot transcrit en fonction de la preuve acoustique. Les cotes de confiance s'échelonnent de 0.0 à 1.0.

-   Un niveau de 1.0 indique que la transcription actuelle du mot correspond au résultat le plus probable.
-   Un niveau de 0.5 signifie que le mot a 50 pour cent de chance d'être correct.

### Exemple de confiance des mots
{: #wordConfidenceExample}

L'exemple suivant demande des cotes de confiance pour les mots d'une transcription.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_confidence=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday ",
          "word_confidence": [
            [
              "several",
              1.0
            ],
            [
              "tornadoes",
              1.0
            ],
            [
              "touch",
              0.52
            ],
            [
              "down",
              0.90
            ],
            . . .
            [
              "on",
              0.31
            ],
            [
              "Sunday",
              0.99
            ]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

## Horodatages des mots (Word timestamps)
{: #word_timestamps}

Le paramètre `timestamps` indique au service s'il faut fournir des horodatages pour les mots qu'il transcrit. Par défaut, le service ne signale pas les horodatages. Définir le paramètre `timestamps` avec la valeur `true` demande au service de signaler les moments de début et de fin en secondes pour chaque mot par rapport au début de la séquence audio.

### Exemple d'horodatages de mots
{: #wordTimestampsExample}

L'exemple suivant présente une demande d'horodatage pour les mots de la transcription.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?timestamps=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "several",
              1.01,
              1.52
            ],
            [
              "tornadoes",
              1.52,
              2.15
            ],
            [
              "touch",
              2.15,
              2.5
            ],
            [
              "down",
              2.5,
              2.81
            ],
            . . .
            [
              "on",
              5.62,
              5.74
            ],
            [
              "Sunday",
              5.74,
              6.34
            ]
          ],
          "confidence": 0.96,
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

## Formatage intelligent (Smart formatting)
{: #smart_formatting}

La fonction de formatage intelligent (Smart formatting) est une fonctionnalité bêta disponible pour l'anglais américain, le japonais et l'espagnol.
{: note}

Le paramètre `smart_formatting` demande au service de convertir les chaînes suivantes en représentations plus conventionnelles :

-   Dates
-   Heures
-   Série de chiffres et de nombres
-   Numéros de téléphone
-   Valeurs de devise
-   Adresses e-mail sur Internet et adresses Web

Pour activer le formatage intelligent, vous définissez le paramètre `smart_formatting` avec la valeur `true`. Par défaut, le service ne procède à aucun formatage intelligent.

Le service applique le formatage intelligent uniquement dans la retranscription finale d'une demande de reconnaissance. Il l'applique juste avant de renvoyer les résultats au client, lorsque la normalisation du texte est terminée. La conversion rend la retranscription plus lisible et permet un meilleur post-traitement des résultats de transcription.

### Ponctuation (anglais américain)
{: #smartFormattingPunctuation}

Pour l'anglais américain, cette fonction permet également au service de remplacer les signes de ponctuation des chaînes de mots clés prononcées suivantes dans l'audio.

<table style="width:50%">
  <caption>Tableau 2. Mots clés de ponctuation dans le formatage intelligent</caption>
  <tr>
    <th style="width:45%; text-align:left">Chaîne de mots clés</th>
    <th style="text-align:center">Ponctuation obtenue</th>
  </tr>
  <tr>
    <td>
      Comma
    </td>
    <td style="text-align:center">
      <code>,</code>
    </td>
  </tr>
  <tr>
    <td>
      Period
    </td>
    <td style="text-align:center">
      <code>.</code>
    </td>
  </tr>
  <tr>
    <td>
      Question mark
    </td>
    <td style="text-align:center">
      <code>?</code>
    </td>
  </tr>
  <tr>
    <td>
      Exclamation point
    </td>
    <td style="text-align:center">
      <code>!</code>
    </td>
  </tr>
</table>

Le service convertit ces chaînes de mots clés en signes uniquement aux emplacements appropriés. Par exemple, le service convertit cette phrase parlée

```
the warranty period is short period
```
{: codeblock}

en texte suivant dans une retranscription

```
the warranty period is short.
```
{: codeblock}

### Différences linguistiques
{: #smartFormattingDifferences}

Le formatage intelligent est basé sur la présence de mots clés évidents dans la retranscription. Les langues prises en charge traitent le formatage intelligent de manière légèrement différente.

#### Anglais américain et espagnol
{: #smartFormattingEnglishSpanish}

-   Les heures sont identifiées par des mots clés, tels que `AM`, `PM` ou `EST`.
-   Les heures militaires sont converties si elles sont identifiées par le mot clé `hours`.
-   Les numéros de téléphone doivent être `911` ou un numéro à 10 ou 11 chiffres commençant par le numéro `1`.

#### Japonais
{: #smartFormattingJapanese}

-   Les adresses e-mail sur Internet et les adresses Web ne sont pas converties.
-   Les numéros de téléphone doivent être à 10 ou 11 chiffres et commencer par des préfixes valides correspondant aux numéros de téléphone au Japon. Par exemple, les préfixes valides comprennent `03` et `090`.
-   Les mots anglais sont convertis en caractères ASCII (*hankaku*). Par exemple, <code>&#65321;&#65314;&#65325;</code> est converti en `IBM`.
-   Les termes ambigus ne peuvent pas être convertis à défaut de contexte suffisant. Par exemple, il est difficile de savoir si <code>&#19968;&#26178;</code> et <code>&#21313;&#20998;</code> se rapportent à des heures.
-   La ponctuation est traitée de la même manière avec ou sans formatage intelligent. Par exemple, en fonction des calculs de probabilité, une des valeurs <code>&#12459;&#12531;&#12510;</code> ou `,` est sélectionné.

### Exemple de formatage intelligent
{: #smartFormattingExample}

L'exemple suivant est une demande de formatage intelligent avec une demande de reconnaissance effectuée en définissant le paramètre `smart_formatting` avec la valeur `true`. Les sections suivantes montrent les effets de ce formatage dans les résultats d'une demande.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?smart_formatting=true"
```
{: pre}

### Résultats du formatage intelligent
{: #smartFormattingResults}

Le tableau suivant présente des exemples de retranscriptions finales avec et sans formatage intelligent. Les retranscriptions sont basées sur des données audio en anglais américain.

<table summary="Chaque ligne d'en-tête est suivie de plusieurs lignes d'exemples montrant l'effet du formatage intelligent pour l'élément identifié dans l'en-tête.">
  <caption>Tableau 3. Exemples de retranscriptions de formatage intelligent</caption>
  <tr>
    <th id="without_formatting" style="width:45%; text-align:left">Sans
      formatage intelligent</th>
    <th id="with_formatting" style="text-align:left">Avec formatage
      intelligent</th>
  </tr>
  <tr>
    <th id="Dates" colspan="2">
      <strong>Dates</strong>
    </th>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on ten oh six nineteen seventy
    </td>
    <td headers="Dates with_formatting">
      I was born on 10/6/1970
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      I was born on the ninth of December nineteen hundred
    </td>
    <td headers="Dates with_formatting">
      I was born on 12/9/1900
    </td>
  </tr>
  <tr>
    <td headers="Dates without_formatting">
      Today is June sixth
    </td>
    <td headers="Dates with_formatting">
      Today is June 6
    </td>
  </tr>
  <tr>
    <th id="Times" colspan="2">
      <strong>Heures</strong>
    </th>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      The meeting starts at nine thirty AM
    </td>
    <td headers="Times with_formatting">
      The meeting starts at 9:30 AM
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      I am available at seven EST
    </td>
    <td headers="Times with_formatting">
      I am available at 7:00 EST
    </td>
  </tr>
  <tr>
    <td headers="Times without_formatting">
      We meet at oh seven hundred hours
    </td>
    <td headers="Times with_formatting">
      We meet at 0700 hours
    </td>
  </tr>
  <tr>
    <th id="Numbers" colspan="2">
      <strong>Nombres</strong>
    </th>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      The quantity is one million one hundred and one
    </td>
    <td headers="Numbers with_formatting">
      The quantity is 1000101
    </td>
  </tr>
  <tr>
    <td headers="Numbers without_formatting">
      One point five is between one and two
    </td>
    <td headers="Numbers with_formatting">
      1.5 is between 1 and 2
    </td>
  </tr>
  <tr>
    <th id="phone_numbers" colspan="2">
      <strong>Numéros de téléphone</strong>
    </th>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at nine one four two three seven one thousand
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 914-237-1000
    </td>
  </tr>
  <tr>
    <td headers="phone_numbers without_formatting">
      Call me at one nine one four nine oh nine twenty six forty five
    </td>
    <td headers="phone_numbers with_formatting">
      Call me at 1-914-909-2645
    </td>
  </tr>
  <tr>
    <th id="currency_values" colspan="2">
      <strong>Valeurs de devise</strong>
    </th>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      You owe me three thousand two hundred two dollars and sixty six
      cents
    </td>
    <td headers="currency_values with_formatting">
      You owe me $3202.66
    </td>
  </tr>
  <tr>
    <td headers="currency_values without_formatting">
      The dollar rose to one hundred and nine point seven nine yen from
      one hundred and nine point seven two yen
    </td>
    <td headers="currency_values with_formatting">
      The dollar rose to 109.79 yen from 109.72 yen
    </td>
  </tr>
  <tr>
    <th id="internet_addresses" colspan="2">
      <strong>Adresses e-mail sur Internet et adresses Web</strong>
    </th>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      My email address is john dot doe at foo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      My email address is john.doe at foo.com
    </td>
  </tr>
  <tr>
    <td headers="internet_addresses without_formatting">
      I saw the story on yahoo dot com
    </td>
    <td headers="internet_addresses with_formatting">
      I saw the story on yahoo.com
    </td>
  </tr>
  <tr>
    <th id="Combinations" colspan="2">
      <strong>Combinaisons</strong>
    </th>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      The code is zero two four eight one and the date of service
      is May fifth two thousand and one
    </td>
    <td headers="Combinations with_formatting">
      The code is 02481 and the date of service is 5/5/2001
    </td>
  </tr>
  <tr>
    <td headers="Combinations without_formatting">
      There are forty seven links on Yahoo dot com now
    </td>
    <td headers="Combinations with_formatting">
      There are 47 links on Yahoo.com now
    </td>
  </tr>
</table>

### Exemple de retranscriptions avec de longues pauses
{: #smartFormattingLongPauses}

Dans les cas de figure où un énoncé comprend des silences relativement longs, le service peut diviser la retranscription en deux blocs finaux ou plus. Cela peut affecter les résultats du formatage, comme illustré dans les exemples suivants.

<table>
  <caption>Tableau 4. Exemples de retranscriptions de formatage intelligent avec de longues pauses</caption>
  <tr>
    <th style="width:45%; text-align:left">Audio et vocal</th>
    <th style="text-align:left">Résultats de la transcription formatée</th>
  </tr>
  <tr>
    <td>
      My phone number is nine one four five five seven three
      three nine two
    </td>
    <td>
      "My phone number is 914-557-3392"
    </td>
  </tr>
  <tr>
    <td>
      My phone number is nine one four <em>&lt;pause&gt;</em> five five
      seven three three nine two
    </td>
    <td>
      "My phone number is 914"<br/>
      "5573392"
    </td>
  </tr>
</table>

## Occultation numérique (Numeric redaction)
{: #redaction}

La fonction d'occultation numérique (numeric redaction) est une fonctionnalité bêta disponible pour l'anglais américain, le japonais et le coréen.
{: note}

Le paramètre `redaction` demande au service d'occulter, ou masquer, les données numériques dans les retranscriptions finales. Cette fonction occulte tout nombre ayant au moins trois chiffres consécutifs en remplaçant chaque chiffre par un caractère `X`. Cela permet d'occulter des données numériques sensibles, par exemple les numéros de carte de crédit.

Par défaut, le service n'occulte pas les données numériques. Définissez le paramètre `redaction` avec la valeur `true` pour activer l'occultation numérique. Lorsque vous activez l'occultation, le service active automatiquement le formatage intelligent, même si vous avez explicitement désactivé cette fonction. Pour garantir un niveau maximal de sécurité, le service applique les valeurs de paramètres suivantes lorsque vous activez l'occultation :

-   Le service désactive la détection de mots clés, même si vous avez indiqué des valeurs pour les paramètres `keywords` et `keywords_threshold`.
-   Le service désactive les résultats intermédiaires, même si vous avez défini le paramètre `interim_results` de l'interface WebSocket avec la valeur `true`.
-   Le service renvoie un seule retranscription finale, même si vous avez indiqué une valeur pour le paramètre `maximum_alternatives`.

La conception de cette fonction suit la fonction de formatage intelligent existante. Le service applique l'occultation uniquement dans la retranscription finale d'une demande de reconnaissance, juste avant le renvoi des résultats au client et lorsque la normalisation du texte est terminée.

Les prochaines versions de cette fonction pourront étendre la fonction d'occultation pour masquer tous les numéros de téléphone, les adresses, les comptes bancaires, les numéros de sécurité sociale et d'autres données numériques sensibles.
{: note}

### Différences linguistiques
{: #redactionDifferences}

La fonction fonctionne exactement de la même manière pour les modèles anglais américain mais avec les différences suivantes pour les modèles japonais et coréen.

#### Japonais
{: #redactionJapanese}

La fonction d'occultation en japonais comporte les différences suivantes :

-   En plus de masquer les chaînes d'au moins trois chiffres consécutifs, l'occultation masque les adresses et les numéros, même s'ils contiennent moins de trois chiffres.
-   De même, l'occultation masque aussi les informations de date pour les dates de naissance en style japonais. En japonais, les informations de date sont généralement présentées au format Latin *Anno Domini* mais elles suivent parfois le style japonais, notamment pour les dates de naissance. Dans ce cas, l'année et le mois sont masqués même s'ils contiennent juste un ou deux chiffres. Par exemple, l'occultation numérique modifie la chaîne suivante ainsi :

    <table style="width:50%">
      <caption>Tableau 5. Exemple d'occultation de date de naissance en style japonais</caption>
      <tr>
        <th style="text-align:left">Sans occultation</th>
        <th style="text-align:left">Avec occultation</th>
      </tr>
      <tr>
        <td>
          &#24179;&#25104; 30&#24180; 2&#26376;
        </td>
        <td>
          &#24179;&#25104; XX&#24180; X&#26376;
        </td>
      </tr>
    </table>

#### Coréen
{: #redactionKorean}

La fonction d'occultation en coréen comporte les différences suivantes :

-   La fonction de formatage intelligent n'est pas prise en charge. Le service effectue tout de même l'occulation numérique pour le coréen mais sans aucun autre formatage intelligent.
-   Les caractères présentant des chiffres isolés sont réduits, mais les chiffres éventuels contenus dans des expressions coréennes ne le sont pas. Par exemple, le caractère <code>&#51060;</code> dans l'expression suivante n'est pas remplacé par un `X` car il est adjacent au caractère suivant :

    <code>&#51060;&#51077;&#45768;&#45796;</code>

    Si le caractère <code>&#51060;</code> était séparé du caractère suivant par un espace, il serait remplacé par un `X`, comme indiqué à la section [Résultats d'occultation numérique](#redactionResults).

### Exemple d'occultation numérique
{: #redactionExample}

L'exemple suivant illustre une demande d'occultation numérique avec une demande de reconnaissance en définissant le paramètre `redaction` avec la valeur `true`. Comme la demande active l'occultation, le service active implicitement le formatage intelligent avec la demande. Le service désactive réellement les autres paramètres de la demande pour qu'ils soient sans effet : le service renvoie une seule retranscription finale et ne reconnaît aucun mot clé. La section suivante montrent les effets de l'occultation.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?&redaction=true&max_alternatives=3&keywords=%22birth%22%2C%22birthday%22&keywords_threshold=0.5"
```
{: pre}

### Résultats d'occultation numérique
{: #redactionResults}

Le tableau suivant présente des exemples de retranscriptions finales avec et sans occultation numérique dans chaque langue prise en charge.

<table style="width:90%" summary="Chaque ligne d'en-tête identifie une langue, suivie d'une ligne unique indiquant la même retranscription avec et sans la fonction d'occultation activée.">
  <caption>Tableau 6. Exemples de retranscription avec occultation numérique</caption>
  <tr>
    <th id="without_redaction" style="text-align:left">Sans occultation</th>
    <th id="with_redaction" style="text-align:left">Avec occultation</th>
  </tr>
  <tr>
    <th id="US_English" colspan="2">
      <strong>Anglais américain</strong>
    </th>
  </tr>
  <tr>
    <td headers="US_English without_redaction">
      my credit card number is four one four seven two
    </td>
    <td headers="US_English with_redaction">
      my credit card number is XXXXX
    </td>
  </tr>
  <tr>
    <th id="Japanese" colspan="2">
      <strong>Japonais</strong>
    </th>
  </tr>
  <tr>
    <td headers="Japanese without_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; &#22235; &#19968; &#22235; &#19971; &#20108;&#12391;&#12377;
    </td>
    <td headers="Japanese with_redaction">
      &#31169; &#12398;&#12463;&#12524;&#12472;&#12483;&#12488; &#12459;&#12540;&#12489; &#30058;&#21495; &#12399; XXXXX &#12391;&#12377;
    </td>
  </tr>
  <tr>
    <th id="Korean" colspan="2">
      <strong>Coréen</strong>
    </th>
  </tr>
  <tr>
    <td headers="Korean without_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; &#49324; &#51068; &#49324; &#52832; &#51060; &#48264;&#51077;&#45768;&#45796;
    </td>
    <td headers="Korean with_redaction">
      &#45236; &#49888;&#50857; &#52852;&#46300; &#48264;&#54840;&#45716; XXXXX &#48264;&#51077;&#45768;&#45796;
    </td>
  </tr>
</table>

## Filtrage des grossièretés (Profanity filtering)
{: #profanity_filter}

La fonction de filtrage des grossièretés (Profanity filtering) est disponible en version GA pour l'anglais américain uniquement.
{: note}

Le paramètre `profanity_filter` indique si le service doit censurer les grossièretés dans ses résultats. Par défaut, le service remplace toutes les grossièretés par une série d'astérisques dans la retranscription. Définir ce paramètre avec `false` affiche les mots dans la sortie tels qu'ils ont été transcrits.

Le service censure les grossièretés dans toutes les retranscriptions finales et dans toute autre retranscription. Il les censure également dans les résultats associés aux fonctions Autres propositions de mots (word alternatives), Confiance des mots (word confidence) et Horodatages de mots (word timestamps). L'unique exception est la détection de mots clés, pour laquelle le service renvoie tous les mots tels qu'ils sont spécifiés par l'utilisateur et même si le paramètre `profanity_filter` a la valeur `true`.

### Exemple de filtrage des grossièretés
{: #profanityFilteringExample}

L'exemple suivant présente les résultats d'un fichier audio court transcrit avec la valeur par défaut `true` indiquée pour le paramètre `profanity_filter`. La demande définit également le paramètre `word_alternatives_threshold` avec la valeur relativement élevée `0.99` et les paramètres `word_confidence` et `timestamps` avec la valeur `true`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?word_alternatives_threshold=0.99&word_confidence=true&timestamps=true"
```
{: pre}

```javascript
{
  "results": [
    {
      "word_alternatives": [
        {
          "start_time": 0.03,
          "alternatives": [
            {
              "confidence": 1.0,
              "word": "****"
            }
          ],
          "end_time": 0.25
        },
        {
          "start_time": 0.25,
          "alternatives": [
            {
              "confidence": 0.99,
              "word": "you"
            }
          ],
          "end_time": 0.56
        }
      ],
      "alternatives": [
        {
          "transcript": "**** you",
          "confidence": 0.99,
          "word_confidence": [
            ["****", 1.0],
            ["you", 0.99]
          ],
          "timestamps": [
            ["****", 0.03, 0.25],
            ["you", 0.25, 0.56]
          ]
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}
