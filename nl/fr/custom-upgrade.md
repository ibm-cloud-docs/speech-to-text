---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-07"

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

# Mise à niveau des modèles personnalisés
{: #customUpgrade}

Afin d'améliorer la qualité de la reconnaissance vocale, le service {{site.data.keyword.speechtotextfull}} met à jour les modèles de base occasionnellement. Etant donné que les modèles de base correspondant aux différentes langues sont indépendants les uns des autres, tout comme les modèles à large bande et à bande étroite d'une langue, les mises à jour de modèles de base individuels n'affectent pas d'autres modèles. Les [notes sur l'édition](/docs/services/speech-to-text/release-notes.html) documentent toutes les mises à jour des modèles de base.
{: shortdesc}

Lorsqu'une nouvelle version d'un modèle de base est publiée, vous devez effectuer la mise à niveau des modèles de langue personnalisés ou des modèles acoustiques personnalisés construits sur le modèle de base pour tirer parti de ces mises à jour. Vos modèles personnalisés continuent à utiliser l'ancienne version du modèle de base tant que la mise à niveau n'est pas terminée. Comme pour toute opération de personnalisation, vous devez utiliser les données d'identification de l'instance du service propriétaire d'un modèle pour en effectuer la mise à niveau.

Effectuez la mise à niveau à la dernière version d'un modèle de base mis à jour dès que possible. Plus vite vous mettez à jour le modèle personnalisé, plus vite vous pourrez tester les avantages du nouveau modèle en termes de performances. Par ailleurs, les anciennes versions des modèles de base peuvent être retirées ultérieurement. Pour inciter à effectuer une mise à niveau, le service renvoie un message d'avertissement avec les résultats des demandes de reconnaissance utilisant des modèles personnalisés basés sur des modèles de base plus anciens.

## Comment fonctionne la mise à niveau ?
{: #upgradeOverview}

La première fois qu'un nouveau modèle de base est publié, les modèles personnalisés existants sont toujours basés sur l'ancienne version du modèle de base. Tant qu'un modèle personnalisé n'est pas mis à niveau, toutes les opérations sur ce modèle personnalisé, par exemple l'ajout de données ou l'entraînement du modèle, s'appliquent à la version existante du modèle. De même, toutes les demandes de reconnaissance qui spécifient le modèle personnalisé utilisent la version existante du modèle.

La mise à niveau produit deux versions d'un modèle personnalisé : une version basée sur l'ancienne version du modèle de base et une version basée sur la dernière version du modèle de base. Une fois le modèle personnalisé mis à niveau, toutes les opérations sur ce modèle s'appliquent à la dernière version mise à niveau du modèle. Il n'est plus possible d'ajouter des données ou d'entraîner la version antérieure du modèle. De plus, vous ne pouvez pas annuler une opération de mise à niveau.

Lorsque vous mettez à niveau un type de modèle personnalisé, vous n'avez pas besoin d'effectuer la mise à niveau de ses ressources individuelles. Pour un modèle de langue personnalisé, le service effectue automatiquement la mise à niveau des corpus, grammaires ou mots définis pour le modèle. De même, lorsque vous mettez à niveau un modèle acoustique personnalisé, le service effectue automatiquement la mise à niveau des ressources associées.

Par défaut, le service utilise la dernière version du modèle personnalisé pour les demandes de reconnaissance. Cependant, vous pouvez continuer à utiliser la version antérieure d'un modèle personnalisé pour la reconnaissance vocale. Pour plus d'informations, voir [Demandes de reconnaissance effectuées avec des modèles personnalisés mis à niveau](#upgradeRecognition).

## Mise à niveau d'un modèle de langue personnalisé
{: #upgradeLanguage}

Pour mettre à niveau un modèle de langue personnalisé, procédez comme suit :

1.  Vérifiez que le modèle de langue personnalisé est à l'état `ready` ou `available`. Pour effectuer la vérification du statut d'un modèle, vous pouvez utiliser la méthode `GET /v1/customizations/{customization_id}`. Si l'état du modèle est `ready`, mettez-le à niveau avant de l'entraîner avec les données les plus récentes.

1.  Mettez à niveau le modèle de langue personnalisé en utilisant la méthode `POST /v1/customizations/{customization_id}/upgrade_model` :

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    Cette méthode de mise à niveau est asynchrone. Elle peut prendre quelques minutes en fonction du nombre de mots figurant dans la ressource de mots du modèle et de la charge en cours sur le service.

Le service renvoie le code de réponse 200 si le processus de mise à niveau a été initié avec succès. Vous pouvez surveiller le statut d'une mise à niveau à l'aide de la méthode `GET /v1/customizations/{customization_id}` pour interroger le statut du modèle. Utilisez une boucle pour vérifier le statut toutes les 10 secondes. 

Lorsque le modèle personnalisé est en cours de mise à niveau, son statut est `upgrading`. Une fois la mise à niveau terminée, le modèle reprend le statut qu'il avait avant la mise à niveau (`ready` ou `available`). La vérification du statut d'une opération de mise à niveau est identique à la vérification du statut d'une opération d'entraînement. Pour plus d'informations, voir [Surveillance de la demande d'entraînement du modèle](/docs/services/speech-to-text/language-create.html#monitorTraining-language).

Le service ne peut accepter aucune demande de modification du modèle tant que la demande de mise à niveau n'est pas terminée. Vous pouvez toutefois continuer à émettre des demandes de reconnaissance avec la version existante du modèle pendant la mise à niveau.

## Mise à niveau d'un modèle acoustique personnalisé
{: #upgradeAcoustic}

Pour mettre à niveau un modèle acoustique personnalisé, procédez comme suit. Si le modèle acoustique personnalisé a été entraîné avec un modèle de langue personnalisé, vous devez effectuer deux étapes de plus dans la mise à niveau, à l'endroit indiqué.

1.  *Si le modèle acoustique personnalisé a été entraîné avec un modèle de langue personnalisé,* vous devez d'abord mettre à niveau le modèle de langue personnalisé à la dernière version du modèle de base. Pour plus d'informations, voir [Mise à niveau d'un modèle de langue personnalisé](#upgradeLanguage).

1.  Vérifiez que le modèle acoustique personnalisé est à l'état `ready` ou `available`. Pour effectuer la vérification du statut d'un modèle, vous pouvez utiliser la méthode `GET /v1/acoustic_customizations/{customization_id}`. Si l'état du modèle est `ready`, mettez-le à niveau avant de l'entraîner avec les données les plus récentes.

1.  Mettez à niveau le modèle acoustique personnalisé en utilisant la méthode `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` :

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    Cette méthode de mise à niveau est asynchrone. Elle peut prendre de quelques minutes à quelques heures en fonction de la quantité de données audio que contient le modèle et de la charge en cours sur le service. A l'instar de l'entraînement, la mise à niveau correspond en général à environ deux fois la durée des données audio du modèle.

1.  *Si le modèle acoustique personnalisé a été entraîné avec un modèle de langue personnalisé,* effectuez une nouvelle mise à niveau du modèle acoustique personnalisé, avec cette fois, le modèle de langue personnalisé qui vient de faire l'objet d'une mise à niveau. Utilisez le paramètre de requête `custom_language_model_id` pour spécifier l'ID de personnalisation du modèle de langue personnalisé.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model?custom_language_model_id={custom_language_model_id}"
    ```
    {: pre}

    Là encore, il s'agit d'une méthode de mise à niveau asynchrone, et en principe, la mise à niveau nécessite deux fois plus de temps que la durée des données audio.

    La demande de mise à niveau d'un modèle acoustique avec le modèle de langue peut échouer avec le code de réponse 400 et le message `No input data modified since last training`. Si cette erreur se produit, ajoutez le paramètre de requête booléen `force` dans la demande et définissez ce paramètre avec la valeur `true`. Utilisez ce paramètre uniquement pour forcer la mise à niveau d'un modèle acoustique personnalisé dans ce cas particulier.
    {: note}

Le service renvoie le code de réponse 200 si le processus de mise à niveau a été initié avec succès. Vous pouvez surveiller le statut d'une mise à niveau à l'aide de la méthode `GET /v1/acoustic_customizations/{customization_id}` pour interroger le statut du modèle. Utilisez une boucle pour vérifier le statut toutes les minutes. 

Lorsque le modèle personnalisé est en cours de mise à niveau, son statut est `upgrading`. Une fois la mise à niveau terminée, le modèle reprend le statut qu'il avait avant la mise à niveau (`ready` ou `available`). La vérification du statut d'une opération de mise à niveau est identique à la vérification du statut d'une opération d'entraînement. Pour plus d'informations, voir [Surveillance de la demande d'entraînement du modèle](/docs/services/speech-to-text/acoustic-create.html#monitorTraining-acoustic).

Le service ne peut accepter aucune demande de modification du modèle tant que la demande de mise à niveau n'est pas terminée. Vous pouvez toutefois continuer à émettre des demandes de reconnaissance avec la version existante du modèle pendant la mise à niveau.

## Echecs de mise à niveau
{: #upgradeFailures}

La mise à niveau d'un modèle personnalisé ne parvient pas à démarrer si le service est en train de traiter une autre demande pour le modèle, telle qu'une demande d'entraînement ou d'ajout de données. De même, la demande de mise à niveau ne parvient pas à démarrer dans les cas suivants :

-   Le modèle personnalisé n'est pas à l'état `ready` ou `available`.
-   Le modèle personnalisé ne contient pas de données (mots personnalisés ou ressources audio).
-   S'il s'agit d'un modèle acoustique personnalisé entraîné avec un modèle de langue personnalisé, les modèles personnalisés sont basés sur différentes versions du modèle de base. Vous devez mettre à niveau le modèle de langue personnalisé avant le modèle acoustique personnalisé. 

## Affichage de la liste des informations de version d'un modèle personnalisé
{: #upgradeList}

Pour voir les versions du modèle de base pour lequel un modèle personnalisé est disponible, utilisez les méthodes suivantes :

-   Pour afficher des informations sur un modèle de langue personnalisé, utilisez la méthode `GET /v1/customizations/{customization_id}`. Pour plus d'informations, voir [Affichage de la liste des modèles de langue personnalisés](/docs/services/speech-to-text/language-models.html#listModels-language).
-   Pour afficher des informations sur un modèle acoustique personnalisé, utilisez la méthode `GET /v1/acoustic_customizations/{customization_id}`. Pour plus d'informations, voir [Affichage de la liste des modèles acoustiques personnalisés](/docs/services/speech-to-text/acoustic-models.html#listModels-acoustic).

Dans les deux cas, la sortie comprend une zone `versions` indiquant des informations relatives aux modèles de base correspondant au modèle personnalisé. La sortie suivante illustre les informations obtenues pour un modèle de langue personnalisé mis à niveau :

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
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

La zone `versions` indique que le modèle personnalisé est disponible pour deux versions du modèle de base : la version antérieure, `en-US_BroadbandModel.v07-06082016.06202016` et la version la plus récente, `en-US_BroadbandModel.v2017-11-15`. Si le modèle personnalisé n'est pas mis à niveau ou s'il n'existe qu'une seule version de son modèle de base, une seule version est indiquée dans la zone `versions`.

## Demandes de reconnaissance effectuées avec des modèles personnalisés mis à niveau
{: #upgradeRecognition}

Par défaut, le service utilise la dernière version d'un modèle personnalisé spécifié avec une demande de reconnaissance. Cependant, même après la mise à niveau du modèle personnalisé, vous pouvez continuer à effectuer des demandes de reconnaissance avec l'ancienne version du modèle. Utilisez le paramètre `base_model_version` d'une méthode de reconnaissance pour spécifier la version d'un modèle de base à utiliser pour la reconnaissance vocale. 

Par exemple, la demande HTTP suivante précise qu'il faut utiliser l'ancienne version du modèle de base. Par conséquent, l'ancienne version du modèle de langue personnalisé indiqué est également utilisée.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_BroadbandModel&base_model_version=en-US_BroadbandModel.v07-06082016.06202016&language_customization_id={customization_id}"
```
{: pre}

Vous pouvez utiliser cette fonction pour tester les performances et l'exactitude d'un modèle personnalisé par rapport à l'ancienne et à la nouvelle version du modèle de base correspondant. Si vous constatez que les performances d'un modèle mis à niveau laissent à désirer (par exemple, si certains mots ne sont plus reconnus), vous pouvez continuer à utiliser l'ancienne version avec les demandes de reconnaissance.

La rubrique [Version du modèle de base](/docs/services/speech-to-text/input.html#version) décrit le paramètre `base_model_version` et indique comment le service détermine les versions des modèles de base et des modèles personnalisés à utiliser avec une demande de reconnaissance. En plus de ces informations, tenez compte des facteurs suivants lorsque vous transmettez à la fois un modèle de langue personnalisé et un modèle acoustique personnalisé avec une demande de reconnaissance :

-   Les deux modèles personnalisés doivent être basés sur le même modèle de base (par exemple, `en-US_BroadbandModel`).
-   Si les deux modèles personnalisés sont basés sur l'ancien modèle de base, le service utilise cet ancien modèle pour la reconnaissance.
-   Si les deux modèles personnalisés sont basés sur un modèle de base plus récent, le service utilisera ce modèle plus récent pour la reconnaissance.
-   Si un seul des deux modèles personnalisés est mis à niveau vers le modèle de base le plus récent, le service utilise l'ancien modèle de base pour la reconnaissance. Il sélectionne l'ancien modèle car il s'agit de la version commune aux deux modèles personnalisés.
