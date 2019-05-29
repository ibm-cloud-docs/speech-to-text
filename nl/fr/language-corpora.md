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

# Gestion des corpus
{: #manageCorpora}

L'interface de personnalisation inclut la méthode `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` pour ajouter un corpus à un modèle de langue personnalisé. Pour plus d'informations, voir [Ajout d'un corpus au modèle de langue personnalisé](/docs/services/speech-to-text/language-create.html#addCorpus). L'interface comprend également les méthodes suivantes pour répertorier et supprimer des corpus d'un modèle de langue personnalisé.
{: shortdesc}

## Affichage de la liste des corpus d'un modèle de langue personnalisé
{: #listCorpora}

L'interface de personnalisation fournit deux méthodes pour répertorier des informations sur les corpus d'un modèle de langue personnalisé :

-   La méthode `GET /v1/customizations/{customization_id}/corpora` répertorie les informations sur tous les corpus d'un modèle de langue personnalisé.
-   La méthode `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` répertorie les informations sur un corpus spécifié d'un modèle personnalisé.

Ces deux méthodes renvoient le nom (`name`) du corpus, le nombre total de mots (`total_words`) lus à partir des corpus, ainsi que le nombre de mots n'appartenant pas au vocabulaire de base (`out-of-vocabulary_words`) extraits du corpus. Elles indiquent également le statut (`status`) du corpus. Connaître le statut est important pour vérifier l'analyse d'un corpus effectué par le service en réponse à une demande d'ajout de ce corpus à un modèle personnalisé :

-   `analyzed` indique que l'analyse du corpus par le service a abouti. Le modèle personnalisé peut être entraîné avec les données du corpus.
-   `being_processed` indique que le service est encore en train d'analyser le corpus. Le service ne peut pas accepter de demandes d'ajout de nouveaux corpus ou de mots nouveaux, ou d'entraînement du modèle personnalisé tant que l'analyse n'est pas terminée.
-   `undetermined` indique que le service a rencontré une erreur lors du traitement du corpus. Les informations renvoyées pour le corpus comprennent un message d'erreur indiquant comment corriger l'erreur.

    Par exemple, il se peut que le corpus soit non valide, ou que vous ayez tenté d'ajouter un corpus du même nom qu'un corpus existant. Vous pouvez essayer d'ajouter à nouveau le corpus en incluant le paramètre `allow_overwrite` avec la demande. Vous pouvez également supprimer le corpus, puis tenter de l'ajouter à nouveau.

### Exemples de demandes et de réponses
{: #listExample-corpora}

L'exemple suivant répertorie tous les corpus du modèle personnalisé avec l'ID de personnalisation (customization_id) spécifié :

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora"
```
{: pre}

Trois corpus (corpus1, corpus2 et corpus3) ont été ajoutés au modèle personnalisé. Le service a analysé `corpus1` avec succès. Il est encore en train d'analyser `corpus2` et son analyse de `corpus3` a échoué.

```javascript
{
  "corpora": [
    {
      "name": "corpus1",
      "total_words": 5037,
      "out_of_vocabulary_words": 401,
      "status": "analyzed"
    },
    {
      "name": "corpus2",
      "total_words": 0,
      "out_of_vocabulary_words": 0,
      "status": "being_processed"
    },
    {
      "name": "corpus3",
      "total_words": 0,
      "out_of_vocabulary_words": 0,
      "status": "undetermined",
      "error": "Analysis of corpus 'corpus3.txt' failed. Please try adding the corpus again by setting the 'allow_overwrite' flag to 'true'."
    }
  ]
}
```
{: codeblock}

L'exemple suivant renvoie des informations sur le corpus nommé `corpus1` du modèle personnalisé avec l'ID de personnalisation (customization_id) spécifié :

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

## Suppression d'un corpus d'un modèle de langue personnalisé
{: #deleteCorpus}

Utilisez la méthode `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` pour retirer un corpus existant d'un modèle de langue personnalisé. Lorsque le service supprime le corpus, il retire les mots n'appartenant pas au vocabulaire (out_of_vocabulary_words) associés au corpus de la ressource de mots du modèle personnalisé sauf si

-   le mot a également été ajouté par un autre corpus ou par une grammaire.
-   le mot a fait l'objet d'une modification avec la méthode `POST /v1/customizations/{customization_id}/words` ou `PUT /v1/customizations/{customization_id}/words/{word_name}`.

Le retrait d'un corpus n'affecte pas le modèle personnalisé tant que vous n'avez pas entraîné le modèle sur ses données mises à jour à l'aide de la méthode `POST /v1/customizations/{customization_id}/train`. Si vous avez entraîné le modèle sur le corpus, les mots du corpus restent dans le vocabulaire du modèle et s'appliquent à la reconnaissance vocale jusqu'à ce que vous entraîniez à nouveau le modèle.

### Exemple de demande
{: #deleteExample-corpus}

L'exemple suivant supprime le corpus nommé `corpus3` du modèle personnalisé avec l'ID de personnalisation (customization_id) spécifié.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus3"
```
{: pre}
