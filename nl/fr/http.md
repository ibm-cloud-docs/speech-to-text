---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-08"

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

# L'interface HTTP synchrone
{: #http}

L'interface HTTP synchrone du service {{site.data.keyword.speechtotextfull}} fournit une méthode `POST /v1/recognize` pour demander une reconnaissance vocale avec le service. Cette méthode est le moyen le plus simple d'obtenir une retranscription. Elle offre deux modes de soumission d'une demande de reconnaissance vocale :
{: shortdesc}

-   Le premier mode consiste à envoyer toutes les données audio en un seul flux via le corps de la demande. Vous spécifiez les paramètres de l'opération sous forme d'en-têtes de demande et de paramètres de requête. Pour plus d'informations, voir [Création d'une demande HTTP de base](#HTTP-basic).
-   Le second mode consiste à envoyer les données audio sous forme de demande multiparties. Vous spécifiez les paramètres de la demande avec une combinaison d'en-têtes de demande, de paramètres de requête et de métadonnées JSON. Pour plus d'informations, voir [Création d'une demande HTTP multiparties](#HTTP-multi).

Soumettez au maximum 100 Mo et au minimum 100 octets de données audio avec une seule demande. Pour plus d'informations sur les formats audio et sur l'utilisation de la compression pour maximiser la quantité de données audio que vous pouvez envoyer avec une demande, voir [Formats audio](/docs/services/speech-to-text/audio-formats.html). Pour plus d'informations sur toutes les méthodes de l'interface HTTP, voir [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

## Création d'une demande HTTP de base
{: #HTTP-basic}

La méthode HTTP `POST /v1/recognize` fournit un moyen simple de transmission audio. Vous transmettez la totalité des données audio via le corps de la demande et spécifiez les paramètres sous forme d'en-têtes de demande et de paramètres de requête.

L'exemple de commande `curl` suivant envoie une demande de reconnaissance pour un fichier FLAC unique nommé `audio-file.flac`. La demande omet le paramètre de requête `model` pour utiliser le modèle de langue par défaut, `en-US_BroadbandModel`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

L'exemple renvoie la retranscription suivante des données audio :

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.89,
          "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0
}
```
{: codeblock}

La méthode `POST /v1/recognize` ne renvoie les résultats qu'après avoir traité toutes les données audio d'une demande. La méthode convient au traitement par lots mais pas pour la reconnaissance vocale en direct. Utilisez l'interface WebSocket pour transcrire les données audio en direct.

Si vos données sont constituées de plusieurs fichiers audio, le moyen recommandé pour les soumettre consiste à envoyer plusieurs demandes, une pour chaque fichier audio. Vous pouvez soumettre les demandes dans une boucle, en utilisant éventuellement le parallélisme pour améliorer les performances. Vous pouvez également utiliser la reconnaissance vocale multiparties pour transmettre plusieurs fichiers audio en une seule demande.

## Création d'une demande HTTP multiparties
{: #HTTP-multi}

La méthode `POST /v1/recognize` prend également en charge les demandes multiparties. Vous transmettez toutes les données audio sous forme de données de formulaire en plusieurs parties. Vous spécifiez certains paramètres sous forme d'en-têtes de demande et de paramètres de requête, mais vous transmettez les métadonnées JSON sous forme de données de formulaire pour contrôler la plupart des aspects de la transcription.

La reconnaissance vocale multiparties est conçue pour les cas d'utilisation suivants :

-   Pour transmettre plusieurs fichiers audio dans une seule demande de reconnaissance vocale.
-   Dans les navigateurs avec JavaScript désactivé. Les demandes multiparties basées sur des données de formulaire ne nécessitent pas l'utilisation de JavaScript.
-   Lorsque les paramètres d'une demande de reconnaissance sont supérieurs à 8 ko, valeur correspondant à la limite imposée par la plupart des serveurs et des proxys HTTP. Par exemple, la reconnaissance d'un très grand nombre de mots clés peut augmenter la taille d'une demande au-delà de cette limite. Les demandes multiparties utilisent des données de formulaire pour éviter cette contrainte.

Les sections suivantes présentent les paramètres que vous utilisez pour les demandes multiparties, ainsi qu'un exemple de demande.

### Paramètres pour les demandes multiparties
{: #multipartParameters}

Vous spécifiez les paramètres suivants d'une reconnaissance vocale multiparties sous forme d'en-têtes de demande, de paramètres de requête et de données de formulaire.

<table summary="Chaque ligne du tableau décrit l'utilisation d'un paramètre possible pour une demande de reconnaissance multiparties.">
  <caption>Tableau 1. Paramètres des demandes multiparties</caption>
  <tr>
    <th id="parameter" style="text-align:left; width:20%">Paramètre</th>
    <th id="description" style="text-align:center; width:80%">Description</th>
  </tr>
  <tr>
    <td>
      <code>metadata</code>
      <br/><em>Objet</em>
      <br/><em>de données de formulaire</em>
    </td>
    <td>
      <em>Obligatoire.</em> Objet JSON qui fournit les paramètres de
      transcription pour la demande. L'objet doit être la première partie
      des données de formulaire. Les informations décrivent les données audio
      dans les parties suivantes des données de formulaires. Voir
      [Métadonnées JSON pour les demandes multiparties](#multipartJSON).
    </td>
  </tr>
  <tr>
    <td>
      <code>upload</code>
      <br/><em>Fichier</em>
      <br/><em>de données de formulaire</em>
    </td>
    <td>
      <em>Obligatoire.</em> Un ou plusieurs fichiers audio constituant les données
      de formulaire restantes de la demande. Tous les fichiers audio doivent être au même format.
      Avec la commande `curl`, incluez une option <code>--form</code> distincte
      pour chaque fichier de la demande.
    </td>
  </tr>
  <tr>
    <td>
      <code>Content-Type</code>
      <br/><em>Chaîne</em>
      <br/><em>d'en-tête</em>
    </td>
    <td>
      <em>Obligatoire.</em> Spécifiez `multipart/form-data` pour indiquer
      les données transmises dans la méthode. Vous spécifiez le type de contenu audio avec
      le paramètre JSON `part_content_type`.
    </td>
  </tr>
  <tr>
    <td>
      <code>Transfer-Encoding</code>
      <br/><em>Chaîne</em>
      <br/><em>d'en-tête</em>
    </td>
    <td>
      <em>Facultatif.</em> Indiquez `chunked` pour diffuser en continu les données audio
      au service. Omettez ce paramètre si vous envoyez toutes les données audio dans une seule demande.
    </td>
  </tr>
  <tr>
    <td>
      <code>model</code>
      <br/><em>Chaîne</em>
      <br/><em>de requête</em>
    </td>
    <td>
      <em>Facultatif.</em> Identificateur du modèle à utiliser avec la
      demande. La valeur par défaut est `en-US_BroadbandModel`.
    </td>
  </tr>
  <tr>
    <td>
      <code>language_customization_id</code>
      <br/><em>Chaîne</em>
      <br/><em>de requête</em>
    </td>
    <td>
      <em>Facultatif.</em> Identificateur global unique (GUID) d'un modèle de langue personnalisé
      à utiliser avec la demande.
    </td>
  </tr>
  <tr>
    <td>
      <code>acoustic_customization_id</code>
      <br/><em>Chaîne</em>
      <br/><em>de requête</em>
    </td>
    <td>
      <em>Facultatif.</em> Identificateur global unique (GUID) d'un modèle acoustique personnalisé
      à utiliser avec la demande.
    </td>
  </tr>
  <tr>
    <td>
      <code>base_model_version</code>
      <br/><em>Chaîne</em>
      <br/><em>de requête</em>
    </td>
    <td>
      <em>Facultatif.</em> Version du modèle de base spécifié à
      utiliser avec la demande.
    </td>
  </tr>
</table>

Pour plus d'informations sur les paramètres de requête, voir [Récapitulatif des paramètres](/docs/services/speech-to-text/summary.html).

### Métadonnées JSON pour les demandes multiparties
{: #multipartJSON}

Les métadonnées JSON que vous transmettez avec une demande multiparties peuvent inclure les zones suivantes :

-   `part_content_type` (chaîne)
-   `data_parts_count` (entier)
-   `customization_weight` (nombre)
-   `inactivity_timeout` (entier)
-   `keywords` (Chaîne[])
-   `keywords_threshold` (nombre)
-   `max_alternatives` (entier)
-   `word_alternatives_threshold` (nombre)
-   `word_confidence` (valeur booléenne)
-   `timestamps` (valeur booléenne)
-   `profanity_filter` (valeur booléenne)
-   `smart_formatting` (valeur booléenne)
-   `speaker_labels` (valeur booléenne)
-   `grammar_name` (chaîne)
-   `redaction` (valeur booléenne)

Seuls les deux paramètres suivants sont spécifiques aux demandes multiparties :

-   La zone `part_content_type` est *facultative* pour la plupart des formats audio. Elle est obligatoire pour les formats `audio/alaw`, `audio/basic`, `audio/l16` et `audio/mulaw`. Elle indique le format audio dans les parties suivantes de la demande. Tous les fichiers audio doivent être au même format.
-   La zone `data_parts_count` est *facultative* pour toutes les demandes. Elle indique le nombre de fichiers audio envoyés avec la demande. Le service applique une détection de fin de flux à la dernière partie des données (et parfois la seule). Si vous omettez ce paramètre, le service détermine le nombre de parties à partir de la demande.

Tous les autres paramètres de métadonnées sont facultatifs. Pour obtenir la description de tous les paramètres disponibles, voir [Récapitulatif des paramètres](/docs/services/speech-to-text/summary.html).

### Exemple de demande multiparties

L'exemple de commande `curl` suivant illustre comment transmettre une demande de reconnaissance multiparties avec la méthode `POST /v1/recognize`. La demande transmet deux fichiers audio, **audio-file1.flac** et **audio-file2.flac**. Le paramètre `metadata` fournit la plupart des paramètres de la demande. Les paramètres `upload` fournissent les fichiers audio.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: multipart/form-data"
--form metadata="{\"part_content_type\":\"application/octet-stream\",
  \"data_parts_count\":2,
  \"timestamps\":true,
  \"word_alternatives_threshold\":0.9,
  \"keywords\":[\"colorado\",\"tornado\",\"tornadoes\"],
  \"keywords_threshold\":0.5}"
--form upload="@{path}audio-file1.flac"
--form upload="@{path}audio-file2.flac"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

L'exemple renvoie la retranscription suivante des fichiers audio. Le service renvoie les résultats des deux fichiers audio dans l'ordre dans lequel ils sont envoyés. (L'exemple de sortie abrège les résultats du second fichier.)

```javascript
{
   "results": [
      {
         "word_alternatives": [
            {
               "start_time": 0.03,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "the"
                  }
               ],
               "end_time": 0.09
            },
            {
               "start_time": 0.09,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "latest"
                  }
               ],
               "end_time": 0.62
            },
            {
               "start_time": 0.62,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "weather"
                  }
               ],
               "end_time": 0.87
            },
            {
               "start_time": 0.87,
               "alternatives": [
                  {
                     "confidence": 0.96,
                     "word": "report"
                  }
               ],
               "end_time": 1.5
            }
         ],
         "keywords_result": {},
         "alternatives": [
            {
               "timestamps": [
                  [
                     "the",
                     0.03,
                     0.09
                  ],
                  [
                     "latest",
                     0.09,
                     0.62
                  ],
                  [
                     "weather",
                     0.62,
                     0.87
                  ],
                  [
                     "report",
                     0.87,
                     1.5
                  ]
               ],
               "confidence": 0.99,
               "transcript": "the latest weather report "
            }
         ],
         "final": true
      },
      {
         "word_alternatives": [
            {
               "start_time": 0.15,
               "alternatives": [
                  {
                     "confidence": 1.0,
                     "word": "a"
                  }
               ],
               "end_time": 0.3
            },
            {
               "start_time": 0.3,
               "alternatives": [
                  {
                     "confidence": 1.0,
                     "word": "line"
                  }
               ],
               "end_time": 0.64
            },
            . . .
            {
               "start_time": 4.58,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "Colorado"
                  }
               ],
               "end_time": 5.16
            },
            {
               "start_time": 5.16,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "on"
                  }
               ],
               "end_time": 5.32
            },
            {
               "start_time": 5.32,
               "alternatives": [
                  {
                     "confidence": 0.98,
                     "word": "Sunday"
                  }
               ],
               "end_time": 6.04
            }
         ],
         "keywords_result": {
            "tornadoes": [
               {
                  "normalized_text": "tornadoes",
                  "start_time": 3.03,
                  "confidence": 0.98,
                  "end_time": 3.84
               }
            ],
            "colorado": [
               {
                  "normalized_text": "Colorado",
                  "start_time": 4.58,
                  "confidence": 0.98,
                  "end_time": 5.16
               }
            ]
         },
         "alternatives": [
            {
               "timestamps": [
                  [
                     "a",
                     0.15,
                     0.3
                  ],
                  [
                     "line",
                     0.3,
                     0.64
                  ],
                  . . .
                  [
                     "Colorado",
                     4.58,
                     5.16
                  ],
                  [
                     "on",
                     5.16,
                     5.32
                  ],
                  [
                     "Sunday",
                     5.32,
                     6.04
                  ]
               ],
               "confidence": 0.99,
               "transcript": "a line of severe thunderstorms with several
possible tornadoes is approaching Colorado on Sunday "
            }
         ],
         "final": true
      }
   ],
   "result_index": 0
}
```
{: codeblock}
