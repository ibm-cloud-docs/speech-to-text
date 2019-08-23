---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-21"

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

# Langues et modèles
{: #models}

Le service {{site.data.keyword.speechtotextfull}} peut assurer la reconnaissance vocale dans de nombreuses langues. Avec toutes les interfaces, vous pouvez utiliser le paramètre `model` pour spécifier le modèle à utiliser pour une demande de reconnaissance vocale. Le modèle indique la langue correspondant aux données audio et la fréquence d'échantillonnage de ces données.
{: shortdesc}

## Modèles de langue pris en charge
{: #modelsList}

Pour la plupart des langues, le service prend en charge les modèles à large bande et à bande étroite :

-   *Les modèles à large bande* s'appliquent aux données audio échantillonnées à une fréquence supérieure ou égale à 16 kHz. Utilisez ces modèles pour les applications réactives en temps réel, par exemple, pour les applications de conversation en direct.
-   *Les modèles à bande étroite* s'appliquent aux données audio échantillonnées à une fréquence de 8 kHz. Utilisez ces modèles pour le décodage hors ligne d'une conversation téléphonique, qui est l'utilisation classique pour cette fréquence d'échantillonnage.

Le choix du modèle adapté à votre application est important. Utilisez le modèle qui correspond à la fréquence d'échantillonnage (et à la langue) de vos données audio. Le service ajuste automatiquement la fréquence d'échantillonnage de vos données audio pour correspondre au modèle que vous spécifiez. Pour plus d'informations, voir [Fréquence d'échantillonnage](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#samplingRate).

Pour obtenir la meilleure précision de reconnaissance possible, vous devez également considérer le contenu de vos données audio en termes de fréquence. Pour plus d'informations, voir [Fréquence audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats#frequency).
{: tip}

Le tableau 1 affiche la liste des modèles pris en charge pour chaque langue. Si vous omettez le paramètre `model` dans la demande, le service utilise par défaut le modèle anglais américain, `en-US_BroadbandModel`. Sauf mention *Bêta*, toutes les langues sont généralement disponibles (*GA*) pour une utilisation en production.

<table>
  <caption>Tableau 1. Modèles de langue pris en charge</caption>
  <tr>
    <th style="text-align:left">Langue</th>
    <th style="text-align:center">Modèle à large bande</th>
    <th style="text-align:center">Modèle à bande étroite</th>
  </tr>
  <tr>
    <td>Arabe (standard moderne)</td>
    <td style="text-align:center"><code>ar-AR_BroadbandModel</code></td>
    <td style="text-align:center">Non pris en charge</td>
  </tr>
  <tr>
    <td>Portugais brésilien</td>
    <td style="text-align:center"><code>pt-BR_BroadbandModel</code></td>
    <td style="text-align:center"><code>pt-BR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Chinois (Mandarin)</td>
    <td style="text-align:center"><code>zh-CN_BroadbandModel</code></td>
    <td style="text-align:center"><code>zh-CN_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Anglais (Royaume-Uni)</td>
    <td style="text-align:center"><code>en-GB_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-GB_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Anglais (Etats-Unis)</td>
    <td style="text-align:center"><code>en-US_BroadbandModel</code></td>
    <td style="text-align:center"><code>en-US_NarrowbandModel</code></br>
      <code>en-US_ShortForm_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Français</td>
    <td style="text-align:center"><code>fr-FR_BroadbandModel</code></td>
    <td style="text-align:center"><code>fr-FR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Allemand</td>
    <td style="text-align:center"><code>de-DE_BroadbandModel</code></td>
    <td style="text-align:center"><code>de-DE_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Japonais</td>
    <td style="text-align:center"><code>ja-JP_BroadbandModel</code></td>
    <td style="text-align:center"><code>ja-JP_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Coréen</td>
    <td style="text-align:center"><code>ko-KR_BroadbandModel</code></td>
    <td style="text-align:center"><code>ko-KR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Espagnol (Argentine, bêta)</td>
    <td style="text-align:center"><code>es-AR_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-AR_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Espagnol (castillan)</td>
    <td style="text-align:center"><code>es-ES_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-ES_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Espagnol (Chili, bêta)</td>
    <td style="text-align:center"><code>es-CL_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-CL_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Espagnol (Colombie, bêta)</td>
    <td style="text-align:center"><code>es-CO_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-CO_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Espagnol (Mexique, bêta)</td>
    <td style="text-align:center"><code>es-MX_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-MX_NarrowbandModel</code></td>
  </tr>
  <tr>
    <td>Espagnol (Pérou, bêta)</td>
    <td style="text-align:center"><code>es-PE_BroadbandModel</code></td>
    <td style="text-align:center"><code>es-PE_NarrowbandModel</code></td>
  </tr>
</table>

### Le modèle anglais américain simplifié
{: #modelsShortform}

Le modèle anglais américain simplifié, `en-US_ShortForm_NarrowbandModel`, peut améliorer la reconnaissance vocale pour les solutions de serveur vocal interactif et d'automatisation du service clients. Le modèle simplifié est entraîné pour la reconnaissance d'énoncés courts qui reviennent fréquemment dans les paramètres de support technique comme les centres d'appel d'assistance technique automatique ou avec du personnel. Le modèle est réglé, par exemple, pour des énoncés précis, tels que des chiffres, l'épellation de noms et de mots à un seul caractère, et les réponses de type oui-non. L'utilisation d'une grammaire en combinaison avec le modèle simplifié peut améliorer davantage les résultats de la reconnaissance.

Comme pour tous les modèles, les environnements bruyants peuvent avoir un effet négatif sur les résultats. Par exemple, les bruits de fonds des aéroports, des véhicules en mouvement, des salles de conférence et de plusieurs locuteurs peuvent entraîner des transcriptions moins précises.  Les données audio des haut-parleurs peuvent également réduire la précision en raison de l'écho obtenu avec des appareils de ce type. L'utilisation d'un modèle acoustique personnalisé avec le modèle simplifié peut neutraliser ces effets.

-   Pour plus d'informations sur la personnalisation de modèle de langue et de modèle acoustique, voir [L'interface de personnalisation](/docs/services/speech-to-text?topic=speech-to-text-customization).
-   Pour plus d'informations sur les grammaires, voir [Utilisation de grammaires avec des modèles de langue personnalisés](/docs/services/speech-to-text?topic=speech-to-text-grammars).

### Exemple de modèle de langue
{: #modelsExample}

L'exemple de demande HTTP suivant utilise le modèle `en-US-NarrowbandModel` pour la reconnaissance vocale :

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_NarrowbandModel"
```
{: pre}

## Affichage de la liste des modèles
{: #listModels}

L'interface HTTP fournit deux méthodes pour répertorier les informations sur les modèles pris en charge :

-   La méthode `GET /v1/models` répertorie les informations sur tous les modèles disponibles.
-   La méthode `GET /v1/models/{model_id}` répertorie les informations sur un modèle spécifié.

Les deux méthodes renvoient les informations suivantes sur un modèle :

-   `name` est le nom du modèle que vous utilisez dans une demande.
-   `language` est l'identificateur de langue du modèle (par exemple, `en-US`).
-   `rate` identifie la fréquence d'échantillonnage (fréquence minimale acceptable pour des données audio) utilisée pour le modèle, exprimée en hertz.
-   `url` est l'URI du modèle.
-   `description` fournit une brève description du modèle.
-   `supported_features` décrit les fonctions supplémentaires du service prises en charge avec le modèle :
    -   `custom_language_model` est une valeur booléenne qui indique si vous pouvez créer des modèles de langue personnalisés basés sur le modèle.
    -   `speaker_labels` indique si vous pouvez utiliser le paramètre `speaker_labels` avec le modèle.

### Exemples de demandes et de réponses
{: #listExample}

L'exemple suivant répertorie tous les modèles pris en charge par le service :

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/models"
```
{: pre}

```javascript
{
  "models": [
    {
      "name": "pt-BR_NarrowbandModel",
      "language": "pt-BR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/pt-BR_NarrowbandModel",
      "rate": 8000,
      "supported_features": {
        "custom_language_model": false,
        "speaker_labels": false
      },
      "description": "Brazilian Portuguese narrowband model."
    },
    {
      "name": "ko-KR_BroadbandModel",
      "language": "ko-KR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/ko-KR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": false
      },
      "description": "Korean broadband model."
    },
    {
      "name": "fr-FR_BroadbandModel",
      "language": "fr-FR",
      "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/fr-FR_BroadbandModel",
      "rate": 16000,
      "supported_features": {
        "custom_language_model": true,
        "speaker_labels": true
      },
      "description": "French broadband model."
    },
    . . .
  ]
}
```
{: codeblock}

L'exemple suivant fournit les informations sur le modèle à large bande anglais américain. Ce modèle prend en charge la personnalisation de modèle de langue et les étiquettes de locuteurs (speaker_labels).

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/models/en-US_BroadbandModel"
```
{: pre}

```javascript
{
  "rate": 16000,
  "name": "en-US_BroadbandModel",
  "language": "en-US",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/models/en-US_BroadbandModel",
  "supported_features": {
    "custom_language_model": true,
    "speaker_labels": true
  },
  "description": "US English broadband model."
}
```
{: codeblock}
