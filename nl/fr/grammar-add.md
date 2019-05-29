---

copyright:
  years: 2015, 2019
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

# Ajout d'une grammaire à un modèle de langue personnalisé
{: #grammarAdd}

Avant de pouvoir utiliser une grammaire dans le cadre de la reconnaissance vocale, vous devez d'abord utiliser l'interface de personnalisation pour ajouter la grammaire à un modèle de langue personnalisé. Les étapes permettant d'ajouter une grammaire à un modèle de langue personnalisé ressemblent à celles utilisées pour ajouter des corpus et des mots personnalisés :
{: shortdesc}

1.  [Créez un modèle de langue personnalisé](#createModel-grammar). Vous pouvez créer un modèle personnalisé ou utiliser un modèle existant.
1.  [Ajoutez une grammaire au modèle de langue personnalisé](#addGrammar). Le service valide la grammaire pour en vérifier l'exactitude.
1.  [Validez les mots de la grammaire](#validateGrammar). Vous vérifiez l'exactitude des prononciations possibles (sounds-like) pour les mots OOV (Out-Of-Vocabulary) reconnus par la grammaire.
1.  [Entraînez le modèle de langue personnalisé](#trainGrammar). Le service prépare le modèle personnalisé et la grammaire pour qu'ils soient utilisés dans la reconnaissance vocale. 
1.  Vous pouvez à présent utiliser le modèle personnalisé et la grammaire dans les demandes de reconnaissance vocale. Pour plus d'informations, voir [Utilisation d'une grammaire pour la reconnaissance vocale](/docs/services/speech-to-text/grammar-use.html).

Ces étapes sont itératives. Vous pouvez ajouter des grammaires, ainsi que des corpus et des mots personnalisés à un modèle de langue personnalisé aussi souvent que nécessaire. Vous devez entraîner le modèle personnalisé sur les nouvelles ressources de données (grammaires, corpus ou mots personnalisés) que vous ajoutez. Lorsque vous l'utilisez pour la reconnaissance vocale, un modèle personnalisé répercute les données sur lesquelles il a été entraîné en dernier.

Vous pouvez utiliser des grammaires avec une langue compatible avec la personnalisation de modèle de langue. La personnalisation de modèle de langue est disponible pour la plupart des langues. Pour plus d'informations, voir [Support de langue pour la personnalisation](/docs/services/speech-to-text/custom.html#languageSupport).
{: note}

## Création d'un modèle de langue personnalisé
{: #createModel-grammar}

Pour utiliser une grammaire avec la reconnaissance vocale, vous devez l'ajouter à un modèle de langue personnalisé. Vous pouvez utiliser un modèle de langue personnalisé existant ou créer un modèle de personnalisé en utilisant la méthode `POST /v1/customizations`. Pour plus d'informations sur la création d'un modèle personnalisé, voir [Création d'un modèle de langue personnalisé](/docs/services/speech-to-text/language-create.html#createModel-language).

Un modèle de langue personnalisé peut contenir des corpus et des mots personnalisés, ainsi que des grammaires. Lors de la reconnaissance vocale, vous pouvez utiliser le modèle personnalisé avec ou sans grammaires. Cependant, lorsque vous utilisez une grammaire, le service reconnaît uniquement les mots issus de la grammaire spécifiée.

## Ajout d'une grammaire au modèle de langue personnalisé
{: #addGrammar}

Vous utilisez la méthode `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` pour ajouter un fichier de grammaire à un modèle personnalisé.

-   Spécifiez l'ID de personnalisation du modèle de langue personnalisé avec le paramètre de chemin `customization_id`.
-   Indiquez un nom pour la grammaire avec le paramètre de chemin `grammar_name`. Utilisez un nom localisé qui correspond à la langue du modèle personnalisé et qui reflète le contenu de la grammaire.
    -   Indiquez un nom ne dépassant pas 128 caractères.
    -   N'incluez pas d'espace, de barre oblique ou de barre oblique inversée dans le nom.
    -   N'utilisez pas le nom d'une grammaire ou d'un corpus ayant déjà été ajouté au modèle personnalisé.
    -   N'utilisez pas le nom `user`, qui est réservé par le service pour désigner des mots personnalisés ajoutés ou modifiés par l'utilisateur.

-   Utilisez l'en-tête de demande `Content-Type` pour spécifier le format de la grammaire :
    -   `application/srgs` pour une grammaire au format ABNF
    -   `application/srgs+xml` pour une grammaire au format XML

-   Transmettez le fichier contenant la grammaire en tant que corps de la demande.

L'exemple suivant ajoute le fichier de grammaire nommé `confirm.abnf` au modèle personnalisé avec l'ID spécifié. Dans l'exemple, le nom de la grammaire est `confirm-abnf`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/srgs"
--data-binary @confirm.abnf
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

La méthode accepte également le paramètre de requête facultatif `allow_overwrite` que vous pouvez inclure pour remplacer une grammaire existante du même nom. Utilisez ce paramètre si vous devez mettre à jour une grammaire après l'avoir ajoutée à un modèle.

Cette méthode est asynchrone. Le service peut prendre quelques secondes pour analyser la grammaire, en fonction de la taille de la grammaire et de la charge en cours sur le service. Pour plus d'informations sur la vérification du statut d'une grammaire, voir [Surveillance de la demande d'ajout d'une grammaire](#monitorGrammar).

Vous pouvez ajouter n'importe quel nombre de grammaires à un modèle personnalisé en appelant la méthode une fois pour chaque fichier de grammaire. L'ajout d'une grammaire doit être complètement terminé avant d'en ajouter une autre.

### Surveillance de la demande d'ajout de grammaire
{: #monitorGrammar}

Le service renvoie un code de réponse 201 si la grammaire est valide. Il traite ensuite le contenu de la grammaire de manière asynchrone et extrait automatiquement les mots nouveaux pouvant être reconnus par la grammaire. Vous ne pouvez pas soumettre des demandes d'ajout de ressources de données supplémentaires à un modèle personnalisé ou entraîner le modèle, tant que le service n'a pas terminé l'analyse de la grammaire pour la demande en cours.

Afin de déterminer le statut de l'analyse, utilisez la méthode `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` pour interroger le statut de la grammaire. La méthode accepte l'ID du modèle personnalisé et le nom de la grammaire. L'exemple suivant vérifie le statut de la grammaire nommée `confirm-abnf`.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/confirm-abnf"
```
{: pre}

La réponse comprend le statut de la grammaire :

```javascript
{
  "name": "confirm-abnf",
  "out_of_vocabulary_words": 0,
  "status": "being_processed"
}
```
{: codeblock}

La zone `status` a l'une des valeurs suivantes :

-   `analyzed` indique que l'analyse de la grammaire par le service a abouti.
-   `being_processed` indique que le service est encore en train d'analyser la grammaire. 
-   `undetermined` indique que le service a rencontré une erreur lors du traitement de la grammaire.

Utilisez une boucle pour vérifier le statut de la grammaire toutes les 10 secondes jusqu'à ce qu'il passe à `analyzed`. Pour plus d'informations sur la vérification du statut d'une grammaire, voir [Affichage de la liste des grammaires d'un modèle de langue personnalisé](/docs/services/speech-to-text/grammar-manage.html#listGrammars).

### Traitement des échecs d'ajout de grammaire
{: #handleFailures}

Si l'analyse d'une grammaire échoue, le service définit le statut de cette grammaire avec la valeur `undetermined` et inclut une zone `error` avec une description de l'erreur contenant le statut de la grammaire. Vous pouvez également utiliser la méthode `GET /v1/customizations/{customization_id}` utilisée pour vérifier le statut du modèle personnalisé. En cas d'échec de l'ajout d'une grammaire, la sortie comprend un message d'erreur de ce type :

```javascript
{
  . . .
  "error": "{\"code\":500, \"code_description\":\"Internal Server Error\",
\". . . Cannot compile grammar . . .\"},
  . . .
}
```
{: codeblock}

Cette erreur n'a aucune incidence sur le statut du modèle personnalisé, mais la grammaire ne peut pas être utilisée avec le modèle. Vous pouvez traiter le problème en corrigeant le fichier de grammaire et en relançant la demande pour l'ajouter au modèle personnalisé. Définissez le paramètre `allow_overwrite` avec la valeur `true` pour remplacer la version du fichier de grammaire ayant échoué.

Vous pouvez également supprimer la grammaire du modèle personnalisé pour l'instant, puis corriger et ajouter à nouveau le fichier de grammaire ultérieurement. Pour plus d'informations sur la suppression d'une grammaire, voir [Suppression d'une grammaire d'un modèle de langue personnalisé](/docs/services/speech-to-text/grammar-manage.html#deleteGrammar).

## Validation des mots de la grammaire
{: #validateGrammar}

Lorsque vous ajoutez un fichier de grammaire à un modèle de langue personnalisé, le service analyse la grammaire pour déterminer si elle reconnaît des mots qui ne font pas déjà partie du vocabulaire de base du service. Ces mots sont désignés par OOV (Out-Of-Vocabulary). Le service ajoute les mots OOV à la ressource de mots du modèle personnalisé. Le but de la ressource de mots consiste à définir des mots que ne sont pas déjà présents dans le vocabulaire du service.

Des définitions dans la ressource de mots indiquent au service comment transcrire les mots OOV. Ces informations comprennent une zone `sounds_like` indiquant au service comment se prononce le mot, une zone `display_as` indiquant au service comment afficher le mot et une zone `source` indiquant comment le mot a été ajouté au modèle personnalisé. Pour plus d'informations sur la ressource de mots et les mots OOV, voir [Ressource de mots](/docs/services/speech-to-text/language-resource.html#wordsResource).

Après avoir ajouté une grammaire à un modèle personnalisé, il est recommandé d'examiner les mots OOV dans la ressource de mots du modèle pour vérifier leur prononciation possible. Toutes les grammaires ne comportent pas forcément de mots OOV, mais en principe, il est bon de vérifier la ressource de mots. Pour vérifier les mots du modèle personnalisé après avoir ajouté une grammaire, utilisez les méthodes suivantes :

-   Utilisez la méthode `GET /v1/customizations/{customization_id}/words` pour répertorier tous les mots d'un modèle personnalisé. Transmettez la valeur `grammars` avec le paramètre `word_type` de la méthode pour répertorier uniquement les mots ajoutés à partir de grammaires.
-   Utilisez la méthode `GET /v1/customizations/{customization_id}/words/{word_name}` pour afficher un mot individuel d'un modèle.

Vérifiez que les prononciations possibles des mots sont exactes et correctes. Recherchez les erreurs typographiques et d'autres erreurs dans les mots eux-mêmes. Pour plus d'informations sur la validation et la correction des mots dans un modèle personnalisé, voir [Validation d'une ressource de mots](/docs/services/speech-to-text/language-resource.html#validateModel).

## Entraînement du modèle de langue personnalisé
{: #trainGrammar}

La dernière étape avant de pouvoir utiliser une grammaire avec un modèle de langue personnalisé consiste à entraîner le modèle. Vous utilisez le même processus que pour l'entraînement d'un modèle personnalisé sur de nouveaux corpus ou de nouveaux mots personnalisés. L'entraînement du modèle compile la grammaire pour l'utiliser dans le cadre d'une reconnaissance vocale. Le service convertit la grammaire du format texte d'origine au format binaire utilisé pour la reconnaissance vocale. Vous ne pouvez pas utiliser la grammaire tant que vous n'avez pas entraîné le modèle.

Pour entraîner un modèle personnalisé, vous utilisez la méthode `POST /v1/customizations/{customization_id}/train`. Vous transmettez à la méthode, l'ID de personnalisation (customization_id) du modèle que vous voulez entraîner, comme illustré dans l'exemple suivant.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

La méthode d'entraînement est asynchrone. L'entraînement dure en principe quelques secondes. Mais il peut prendre plus de temps selon la taille et la complexité de la grammaire et en fonction de la charge en cours sur le service. Pour plus d'informations sur la vérification du statut d'une opération d'entraînement, voir [Surveillance de la demande d'entraînement](#monitorTraining-grammar).

### Surveillance de la demande d'entraînement
{: #monitorTraining-grammar}

Le service renvoie le code de réponse 200 si le processus d'entraînement a été initié avec succès. Le service ne peut accepter d'autres demandes d'entraînement ou de nouvelles demandes d'ajout de grammaires, de corpus ou de mots, tant que la demande d'entraînement en cours n'est pas terminée.

Afin de déterminer le statut d'une demande d'entraînement, utilisez la méthode `GET /v1/customizations/{customization_id}` pour interroger le statut du modèle. La méthode accepte l'ID de personnalisation du modèle et renvoie des informations de ce type sur le modèle :

```
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2018-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

La zone `status` a la valeur `training` tant que l'entraînement du modèle est en cours d'exécution. Utilisez une boucle pour vérifier le statut du modèle toutes les 10 secondes jusqu'à ce qu'il passe à `available`. Pour plus d'informations sur la surveillance d'une demande d'entraînement et les autres valeurs de statut possibles, voir [Surveillance de la demande d'entraînement du modèle](/docs/services/speech-to-text/language-create.html#monitorTraining-language).
