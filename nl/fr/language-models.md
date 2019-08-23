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

# Gestion des modèles de langue personnalisés
{: #manageLanguageModels}

L'interface de personnalisation inclut la méthode `POST /v1/customizations` pour créer un modèle de langue personnalisé. L'interface comprend également la méthode `POST /v1/customizations/train` pour entraîner un modèle personnalisé sur les dernières données intégrées dans sa ressource de mots. Pour plus d'informations, voir
{: shortdesc}

-   [Création d'un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language)
-   [Entraînement du modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language)

De plus, l'interface inclut des méthodes pour répertorier des informations sur les modèles de langue personnalisés, réinitialiser un modèle personnalisé à son état initial, mettre à niveau un modèle personnalisé et supprimer un modèle personnalisé. Vous ne pouvez pas entraîner, réinitialiser, mettre à niveau ou supprimer un modèle personnalisé alors que le service traite une autre opération sur ce modèle, notamment l'ajout de ressources au modèle.

## Affichage de la liste des modèles de langue personnalisés
{: #listModels-language}

L'interface de personnalisation fournit deux méthodes pour répertorier les informations sur les modèles de langue personnalisés qui appartiennent aux données d'identification spécifiées :

-   La méthode `GET /v1/customizations` répertorie les informations sur tous les modèles de langue personnalisés ou sur tous les modèles de langue personnalisés d'une langue spécifiée.
-   La méthode `GET /v1/customizations/{customization_id}` répertorie les informations sur un modèle de langue personnalisé spécifié. Utilisez cette méthode pour interroger le service sur le statut d'une demande d'entraînement ou d'ajout de mots nouveaux.

Les deux méthodes renvoient les informations suivantes sur un modèle personnalisé :

-   `customization_id` identifie l'identificateur global unique (GUID) du modèle personnalisé. Le GUID sert à identifier le modèle dans les méthodes de l'interface.
-   `created` correspond à la date et l'heure de création du modèle en temps universel coordonné (TUC).
-   `updated` correspond à la date et l'heure de la dernière modification du modèle personnalisé en temps universel coordonné (TUC).
-   `language` est la langue du modèle personnalisé.
-   `dialect` indique le dialecte de la langue du modèle personnalisé, qui ne correspond pas nécessairement à la langue du modèle personnalisé pour les modèles espagnols. Pour plus d'informations, voir la description du paramètre `dialect` dans [Création d'un modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language).
-   `owner` identifie les données d'identification de l'instance de service propriétaire du modèle personnalisé.
-   `name` est le nom du modèle personnalisé.
-   `description` représente la description du modèle personnalisé, indiquée, le cas échéant, lors de la création du modèle.
-   `base_model` indique le nom du modèle de langue pour lequel a été créé le modèle personnalisé.
-   `versions` fournit une liste de versions disponibles du modèle personnalisé. Chaque élément du tableau indique une version du modèle de base avec lequel peut être utilisé le modèle personnalisé. Il existe plusieurs versions uniquement si le modèle personnalisé a fait l'objet d'une mise à niveau. Autrement, il n'y a qu'une seule version présentée. Pour plus d'informations, voir [Affichage de la liste des informations de version d'un modèle personnalisé](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeList).

La méthode renvoie également une zone `status` indiquant l'état du modèle personnalisé :

-   `pending` indique que le modèle a été créé. Il attend l'ajout de données d'entraînement valides (corpus, grammaires ou mots) ou que le service termine l'analyse des données ayant été ajoutées.
-   `ready` indique que le modèle contient des données valides et qu'il est prêt à être entraîné. Si le modèle contient à la fois des ressources valides et non valides, l'entraînement du modèle échoue tant que vous ne définissez pas le paramètre de requête `strict` sur `false`. Pour plus d'informations, voir [Echecs d'entraînement](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#failedTraining-language).
-   `training` indique que le modèle est en cours d'entraînement sur les données.
-   `available` indique que le modèle est entraîné et prêt à être utilisé avec une demande de reconnaissance.
-   `upgrading` indique que le modèle est en cours de mise à niveau.
-   `failed` indique que l'entraînement du modèle a échoué. Examinez les mots figurant dans la ressource de mots du modèle pour déterminer les erreurs qui ont empêché l'entraînement du modèle.

Par ailleurs, la sortie comprend une zone `progress` qui indique l'état d'avancement de l'entraînement du modèle personnalisé. Si vous avez utilisé la méthode `POST /v1/customizations/{customization_id}/train` pour lancer l'entraînement du modèle, cette zone indique l'état d'avancement de cette demande sous forme de pourcentage effectué. Pour le moment, la valeur de cette zone est `100` si la zone status a la valeur `available`. Autrement la zone progress a la valeur `0`.

### Exemples de demandes et de réponses
{: #listExample-language}

L'exemple suivant inclut le paramètre de requête `language` pour répertorier tous les modèles de langue personnalisés en anglais américain appartenant aux données d'identification spécifiées :

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations?language=en-US"
```
{: pre}

Les données d'identification sont propriétaires de deux modèles de ce type. Le premier modèle est en attente de données ou en cours de traitement par le service. Le second modèle a terminé son entraînement et est prêt à l'emploi.

```javascript
{
  "customizations": [
    {
      "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
      "created": "2016-06-01T18:42:25.324Z",
      "updated": "2016-06-01T18:42:25.324Z",
      "language": "en-US",
      "dialect": "en-US",
      "versions": [
        "en-US_BroadbandModel.v07-06082016.06202016",
        "en-US_BroadbandModel.v2017-11-15"
      ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732fae4829",
      "created": "2016-06-01T18:51:37.291Z",
      "updated": "2016-06-01T20:02:10.624Z",
      "language": "en-US",
      "dialect": "en-US",
      "versions": [
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model two",
      "description": "Example custom language model two",
      "base_model_name": "en-US_BroadbandModel",
      "status": "available",
      "progress": 100
    }
  ]
}
```
{: codeblock}

L'exemple suivant renvoie des informations sur le modèle personnalisé avec l'ID de personnalisation (customization_id) spécifié :

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
      "created": "2016-06-01T18:42:25.324Z",
      "updated": "2016-06-01T18:42:25.324Z",
      "language": "en-US",
      "dialect": "en-US",
      "versions": [
        "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

## Réinitialisation d'un modèle de langue personnalisé
{: #resetModel-language}

Pour réinitialiser un modèle personnalisé, utilisez la méthode `POST /v1/customizations/{customization_id}/reset`. Cette opération retire tous les corpus et les mots du modèle, rétablissant le modèle à l'état initial qu'il avait lors de sa création. Cette méthode ne supprime pas le modèle proprement dit ou les métadonnées telles que son nom et la langue associée. Cependant, lorsque vous réinitialisez un modèle, sa ressource de mots est vide et doit être recréée en ajoutant des corpus et des mots.

### Exemple de demande
{: #resetExample-language}

L'exemple suivant réinitialise le modèle personnalisé avec l'ID de personnalisation (customization_id) spécifié :

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/reset"
```
{: pre}

## Suppression d'un modèle de langue personnalisé
{: #deleteModel-language}

Utilisez la méthode `DELETE /v1/customizations/{customization_id}` pour supprimer un modèle de langue personnalisé dont vous n'avez plus besoin. La méthode supprime tous les corpus et mots associés au modèle personnalisé, y compris le modèle lui-même. Utilisez cette méthode avec précaution : un modèle personnalisé et ses données ne peuvent pas être récupérés une fois le modèle supprimé.

### Exemple de demande
{: #deleteExample-language}

L'exemple suivant supprime le modèle personnalisé avec l'ID de personnalisation (customization_id) spécifié :

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}
