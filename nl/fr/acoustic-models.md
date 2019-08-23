---

copyright:
  years: 2019
lastupdated: "2019-06-24"

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

# Gestion des modèles acoustiques personnalisés
{: #manageAcousticModels}

L'interface de personnalisation inclut la méthode `POST /v1/acoustic_customizations` pour créer un modèle acoustique personnalisé. Cette interface comprend également la méthode `POST /v1/acoustic_customizations/train` pour entraîner un modèle personnalisé sur les ressources audio associées les plus récentes. Pour plus d'informations, voir
{: shortdesc}

-   [Création d'un modèle acoustique personnalisé](/docs/services/speech-to-text?topic=speech-to-text-acoustic#createModel-acoustic)
-   [Entraînement du modèle acoustique personnalisé](/docs/services/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic)

De plus, l'interface inclut des méthodes pour répertorier des informations sur les modèles acoustiques personnalisés, réinitialiser un modèle personnalisé à son état initial, mettre à niveau un modèle personnalisé et supprimer un modèle personnalisé. Vous ne pouvez pas entraîner, réinitialiser, mettre à niveau ou supprimer un modèle personnalisé alors que le service traite une autre opération sur ce modèle, notamment l'ajout de ressources au modèle.

## Affichage de la liste des modèles acoustiques personnalisés
{: #listModels-acoustic}

L'interface de personnalisation fournit deux méthodes pour répertorier les informations sur les modèles acoustiques personnalisés qui appartiennent aux données d'identification spécifiées :

-   La méthode `GET /v1/acoustic_customizations` répertorie les informations sur tous les modèles acoustiques personnalisés ou sur tous les modèles acoustiques personnalisés d'une langue spécifiée.
-   La méthode `GET /v1/acoustic_customizations/{customization_id}` répertorie les informations sur un modèle acoustique personnalisé spécifié. Utilisez cette méthode pour interroger le service sur le statut d'une demande d'entraînement.

Les deux méthodes renvoient les informations suivantes sur un modèle acoustique personnalisé :

-   `customization_id` identifie l'identificateur global unique (GUID) du modèle personnalisé. Le GUID sert à identifier le modèle dans les méthodes de l'interface.
-   `created` correspond à la date et l'heure de création du modèle en temps universel coordonné (TUC).
-   `updated` correspond à la date et l'heure de la dernière modification du modèle personnalisé en temps universel coordonné (TUC).
-   `language` est la langue du modèle personnalisé.
-   `owner` identifie les données d'identification de l'instance de service propriétaire du modèle personnalisé.
-   `name` est le nom du modèle personnalisé.
-   `description` représente la description du modèle personnalisé, indiquée, le cas échéant, lors de la création du modèle.
-   `base_model_name` indique le nom du modèle de langue pour lequel a été créé le modèle personnalisé.
-   `versions` fournit une liste de versions disponibles du modèle personnalisé. Chaque élément du tableau indique une version du modèle de base avec lequel peut être utilisé le modèle personnalisé. Il existe plusieurs versions uniquement si le modèle personnalisé a fait l'objet d'une mise à niveau. Autrement, il n'y a qu'une seule version présentée. Pour plus d'informations, voir [Affichage de la liste des informations de version d'un modèle personnalisé](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeList).

Les méthodes renvoient également une zone `status` indiquant l'état du modèle personnalisé :

-   `pending` indique que le modèle a été créé. Il attend que des données d'entraînement valides (ressources audio) soient ajoutées ou que le service termine l'analyse des donnée ayant été ajoutées.
-   `ready` indique que le modèle contient des données audio valides et qu'il est prêt à être entraîné.     Si le modèle contient à la fois des ressources audio valides et non valides, l'entraînement du modèle échoue tant que vous ne définissez pas le paramètre de requête `strict` sur `false`. Pour plus d'informations, voir [Echecs d'entraînement](/docs/services/speech-to-text?topic=speech-to-text-acoustic#failedTraining-acoustic).
-   `training` indique que le modèle est en cours d'entraînement sur les données audio.
-   `available` indique que le modèle est entraîné et prêt à être utilisé avec des demandes de reconnaissance.
-   `upgrading` indique que le modèle est en cours de mise à niveau.
-   `failed` indique que l'entraînement du modèle a échoué. Examinez les ressources audio du modèle pour déterminer ce qui a pu empêcher l'entraînement du modèle. Des données audio insuffisantes ou en trop grande quantité, ou encore une ressource audio non valide figurent parmi les erreurs possibles.

Par ailleurs, la sortie comprend une zone `progress` qui indique l'état d'avancement de l'entraînement du modèle personnalisé. Si vous avez utilisé la méthode `POST /v1/acoustic_customizations/{customization_id}/train` pour lancer l'entraînement du modèle, cette zone indique l'état d'avancement de cette demande sous forme de pourcentage effectué. Pour le moment, la valeur de cette zone est `100` si la zone status a la valeur `available`. Autrement la zone progress a la valeur `0`.

### Exemples de demandes et de réponses
{: #listExample-acoustic}

L'exemple suivant inclut le paramètre de requête `language` pour répertorier tous les modèles acoustiques personnalisés en anglais américain appartenant aux données d'identification spécifiées :

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations?language=en-US"
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
      "versions": [
        "en-US_BroadbandModel.v07-06082016.06202016",
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model one",
      "description": "Example custom acoustic model",
      "base_model_name": "en-US_BroadbandModel",
      "status": "pending",
      "progress": 0
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732fae4829",
      "created": "2016-06-01T18:51:37.291Z",
      "updated": "2016-06-01T19:21:06.825Z",
      "language": "en-US",
      "versions": [
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model two",
      "description": "Example custom acoustic model two",
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
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
      "created": "2016-06-01T18:42:25.324Z",
      "updated": "2016-06-01T18:42:25.324Z",
      "language": "en-US",
      "versions": [
        "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model one",
  "description": "Example custom acoustic model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

## Réinitialisation d'un modèle acoustique personnalisé
{: #resetModel-acoustic}

Utilisez la méthode `POST /v1/acoustic_customizations/{customization_id}/reset` pour réinitialiser un modèle acoustique personnalisé. Cette opération retire toutes les ressources audio du modèle, rétablissant le modèle à l'état initial qu'il avait lors de sa création. Cette méthode ne supprime pas le modèle proprement dit ou les métadonnées telles que son nom et la langue associée. Cependant, lorsque vous réinitialisez un modèle, ses ressources audio sont retirées et il doit être recréé.

### Exemple de demande
{: #resetExample-acoustic}

L'exemple suivant réinitialise le modèle acoustique personnalisé avec l'ID de personnalisation (customization_id) spécifié :

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/reset"
```
{: pre}

## Suppression d'un modèle acoustique personnalisé
{: #deleteModel-acoustic}

Utilisez la méthode `DELETE /v1/acoustic_customizations/{customization_id}` pour supprimer un modèle acoustique personnalisé dont vous n'avez plus besoin. La méthode supprime toutes les données audio associées au modèle personnalisé, y compris le modèle lui-même. Utilisez cette méthode avec précaution : un modèle personnalisé et ses données ne peuvent pas être récupérés une fois le modèle supprimé.

### Exemple de demande
{: #deleteExample-acoustic}

L'exemple suivant supprime le modèle acoustique personnalisé avec l'ID de personnalisation (customization_id) spécifié :

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}
