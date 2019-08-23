---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-04"

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

# Gestion des grammaires
{: #manageGrammars}

L'interface de personnalisation comprend la méthode `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` pour ajouter une grammaire à un modèle de langue personnalisé. Pour plus d'informations, voir [Ajout d'une grammaire au modèle de langue personnalisé](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar). L'interface comprend également les méthodes suivantes pour répertorier et supprimer des grammaires d'un modèle de langue personnalisé.
{: shortdesc}

## Affichage de la liste des grammaires d'un modèle de langue personnalisé
{: #listGrammars}

L'interface de personnalisation fournit deux méthodes pour répertorier des informations sur les grammaires d'un modèle de langue personnalisé :

-   La méthode `GET /v1/customizations/{customization_id}/grammars ` répertorie les informations sur toutes les grammaires d'un modèle personnalisé.
-   La méthode `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` répertorie les informations sur une grammaire spécifiée d'un modèle personnalisé.

Les deux méthodes renvoient les mêmes informations sur une grammaire. Ces informations comprennent le nom (`name`) de la grammaire et le nombre de mots ne figurant pas dans le vocabulaire de base du service (`out-of_vocabulary_words`) reconnus par la grammaire. La réponse comprend également le statut (`status`) de la grammaire, ce qui est important pour vérifier l'analyse de la grammaire effectuée par le service lorsque vous l'ajoutez à un modèle personnalisé :

-   `being_processed` indique que le service n'a pas terminé le traitement de la grammaire en réponse à une demande `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`.
-   `analyzed` indique que le traitement de la grammaire par le service a abouti et que la grammaire a été ajoutée au modèle personnalisé.
-   `undetermined` signifie que le service a rencontré une erreur lors du traitement de la grammaire. Les informations renvoyées pour la grammaire comprennent un message d'erreur indiquant comment corriger l'erreur.

    Par exemple, il se peut que la grammaire soit non valide, ou que vous ayez tenté d'ajouter une grammaire du même nom qu'une grammaire existante. Vous pouvez essayer d'ajouter à nouveau la grammaire en incluant le paramètre `allow_overwrite` avec la demande. Vous pouvez également supprimer la grammaire et tenter de l'ajouter à nouveau.

### Exemples de demandes et de réponses
{: #listExample-grammars}

L'exemple suivant répertorie les informations sur toutes les grammaires qui ont été ajoutées au modèle personnalisé avec l'ID de personnalisation (customization_id) spécifié.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars"
```
{: pre}

Trois grammaires ont été ajoutées au modèle personnalisé : `confirm-xml`, `confirm-abnf` et `list-abnf`.

```javascript
{
  "grammars": [
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-xml",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 0,
      "name": "confirm-abnf",
      "status": "analyzed"
    },
    {
      "out_of_vocabulary_words": 8,
      "name": "list-abnf",
      "status": "analyzed"
    }
  ]
}
```
{: codeblock}

L'exemple suivant affiche les informations sur la grammaire spécifiée, `list-abnf`.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/grammars/list-abnf"
```
{: pre}

```javascript
{
  "out_of_vocabulary_words": 8,
  "name": "list-abnf",
  "status": "analyzed"
}
```
{: codeblock}

## Suppression d'une grammaire d'un modèle de langue personnalisé
{: #deleteGrammar}

Utilisez la méthode `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` pour retirer une grammaire existante d'un modèle de langue personnalisé. Lorsque le service supprime la grammaire, il retire les mots n'appartenant pas au vocabulaire de base (out_of_vocabulary_words) associés à la grammaire de la ressource de mots du modèle personnalisé sauf si

-   le mot a également été ajouté par une autre grammaire ou par un corpus.
-   le mot a fait l'objet d'une modification avec la méthode `POST /v1/customizations/{customization_id}/words` ou `PUT /v1/customizations/{customization_id}/words/{word_name}`.

Le retrait d'une grammaire n'affecte pas le modèle personnalisé tant que vous n'avez pas entraîné le modèle sur ses données mises à jour à l'aide de la méthode `POST /v1/customizations/{customization_id}/train`. Si vous avez entraîné le modèle sur la grammaire, la grammaire reste disponible pour la reconnaissance vocale jusqu'à ce que vous entraîniez à nouveau le modèle.

### Exemple de demande
{: #deleteExample-grammar}

L'exemple suivant supprime la grammaire nommée `list-abnf` du modèle personnalisé avec l'ID de personnalisation (customization_id) spécifié.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/ {customization_id}/grammars/list-abnf"
```
{: pre}
