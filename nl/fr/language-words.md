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

# Gestion des mots personnalisés
{: #manageWords}

L'interface de personnalisation comprend les méthodes `POST /v1/customizations/{customization_id}/words` et `PUT /v1/customizations/{customization_id}/words/{word_name}`, utilisées pour ajouter ou modifier des mots pour un modèle personnalisé. Pour plus d'informations, voir [Ajout de mots au modèle de langue personnalisé](/docs/services/speech-to-text/language-create.html#addWords). L'interface comprend également les méthodes suivantes pour répertorier et supprimer des mots pour un modèle de langue personnalisé.
{: shortdesc}

Vous serez sans doute amené à ajouter la plupart des mots personnalisés à partir de corpus. Vérifiez que vous connaissez le codage de caractères utilisé dans les fichiers texte de vos corpus. Le service conserve le codage qu'il trouve dans les fichiers texte. Vous devez utiliser ce codage lorsque vous utilisez des mots individuels dans le modèle de langue personnalisé. Lorsque vous spécifiez un mot avec la méthode `GET`, `PUT` ou `DELETE /v1/customizations/{customization_id}/words/{word_name}`, vous devez coder le paramètre `word_name` que vous transmettez dans l'URL en codage URL si le mot comprend des caractères non ASCII. Pour plus d'informations, voir [Codage de caractères](/docs/services/speech-to-text/language-resource.html#charEncoding).
{: important}

## Affichage de la liste des mots d'un modèle de langue personnalisé
{: #listWords}

L'interface de personnalisation offre deux méthodes permettant de répertorier les mots d'un modèle de langue personnalisé :

-   La méthode `GET /v1/customizations/{customization_id}/words` répertorie les informations sur les mots à partir de la ressource de mots du modèle personnalisé. Cette méthode comprend deux paramètres de requête facultatifs :
    -   Le paramètre `word_type` indique les mots qui doivent être répertoriés :

        -   `all` (valeur par défaut) affiche tous les mots.
        -   `user` affiche uniquement les mots qui ont été ajoutés ou modifiés par l'utilisateur.
        -   `corpora` affiche uniquement les mots OOV (Out-Of-Vocabulary) extraits des corpus.
        -   `grammars` affiche uniquement les mots OOV extraits des grammaires.
    -   Le paramètre `sort` indique comment classer les mots. Il accepte deux arguments pour indiquer comment les mots doivent être triés : `alphabetical` (par ordre alphabétique) et `count` (par nombre). Vous pouvez éventuellement ajouter un signe `+` ou `-` devant un argument pour indiquer si les résultats doivent être triés par ordre croissant ou décroissant. Par défaut, la méthode affiche les mots par ordre alphabétique croissant. 
-   La méthode `GET /v1/customizations/{customization_id}/words/{word_name}` répertorie les informations sur un seul mot spécifié dans la ressource de mots du modèle.

En plus d'une zone `word` identifiant le mot, les deux méthodes renvoient les informations suivantes sur chaque mot :

-   Une zone `sounds_like` présentant le tableau des prononciations du mot. Ce tableau peut inclure des prononciations possibles automatiquement générées par le service si aucune valeur sounds-like n'est fournie pour le mot. Pour plus d'informations, voir [Utilisation de la zone sounds_like](/docs/services/speech-to-text/language-resource.html#soundsLike).
-   Une zone `display_as` indiquant comment orthographier le mot personnalisé que le service affiche dans les transcriptions. La zone contient une chaîne vide si aucune valeur display-as n'est fournie pour le mot, auquel cas le mot s'affiche comme il se prononce. Pour plus d'informations, voir [Utilisation de la zone display_as](/docs/services/speech-to-text/language-resource.html#displayAs).
-   Une zone `source` indiquant comment le mot a été ajouté à la ressource de mots du modèle personnalisé. Cette zone comprend le nom de chaque corpus et grammaire d'où provient le mot extrait par le service. Si vous avez modifié ou ajouté directement le mot, cette zone comprend la chaîne `user`.
-   Une zone `count` indiquant le nombre de fois que le mot apparaît dans l'ensemble des corpus et des grammaires. Par exemple si le mot est détecté cinq fois dans un corpus et sept fois dans un autre, la valeur de count sera `12`.

    Si vous ajoutez un mot personnalisé à un modèle avant son ajout dans un corpus ou une grammaire, le nombre commence à `1`. Si le mot est d'abord ajouté à partir d'un corpus ou d'une grammaire, avant d'être modifié par la suite, le nombre correspond uniquement au nombre de fois qu'il a été détecté dans les corpus et les grammaires.

    Pour les modèles personnalisés créés avant l'introduction de la zone `count`, cette zone reste toujours à `0`. Pour mettre à jour la zone des modèles de ce type, ajoutez à nouveau les corpus et les grammaires du modèle et incluez le paramètre `allow_overwrite` avec la demande.
    {: note}

Si le service détecte un ou plusieurs problèmes dans la définition d'un mot personnalisé, la sortie comprend une zone `error`. Cette zone fournit un tableau répertoriant chaque élément du problème dans la définition et un message avec la description du problème.

Une erreur peut se produire, si vous ajoutez par exemple un mot personnalisé avec une zone `sounds_like` non valide, qui enfreint l'une des règles d'ajout d'une prononciation. Vous ne pouvez pas entraîner un modèle personnalisé dont la ressource de mots contient un mot avec une erreur. Vous devez corriger ou supprimer le mot pour pouvoir entraîner le modèle.

### Exemples de demandes et de réponses
{: #listExample-words}

L'exemple suivant répertorie tous les mots, quel que soit leur type, du modèle personnalisé avec l'ID de personnalisation (customization_id) spécifié. Les mots s'affichent dans l'ordre de tri par défaut, c'est-à-dire par ordre alphabétique croissant.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
```
{: pre}

La ressource de mots du modèle contient quatre mots. Le premier mot a été ajouté directement par l'utilisateur, mais sa zone `sounds_like` contient une erreur : cette zone ne peut pas contenir de nombres. Les autres mots ont été ajoutés par l'utilisateur ou à la fois par l'utilisateur et à partir de corpus.

```javascript
{
  "words": [
    {
      "word": "75.00",
      "sounds_like": ["75 dollars"],
      "display_as": "75.00",
      "count": 1,
      "source": ["user"],
      "error": [{"75 dollars": "Numbers are not allowed in sounds_like. You can try for example 'seventy five dollars'."}]
    },
    {
      "word": "HHonors",
      "sounds_like": [
        "hilton honors",
        "H. honors"
      ],
      "display_as": "HHonors",
      "count": 1,
      "source": [
        "corpus1",
        "user"
      ]
    },
    {
      "word": "IEEE",
      "sounds_like": ["I. triple E."],
      "display_as": "IEEE",
      "count": 3,
      "source": [
        "corpus1",
        "corpus2",
        "user"
      ]
    },
    {
      "word": "tomato",
      "sounds_like": [
        "tomatoh",
        "tomayto"
      ],
      "display_as": "tomato",
      "count": 1,
      "source": ["user"]
    }
  ]
}
```
{: codeblock}

L'exemple suivant présente des informations sur le mot `NCAA` figurant dans la ressource de mots du modèle spécifié :

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
```
{: pre}

L'utilisateur a ajouté le mot en premier. Le service a ensuite trouvé deux occurrences de ce mot dans le corpus nommé `corpus3`.

```javascript
{
  "word": "NCAA",
  "sounds_like": [
    "N. C. A. A.",
    "N. C. double A."
  ],
  "display_as": "NCAA",
  "count": 3,
  "source": [
    "corpus3",
    "user"
  ]
}
```
{: codeblock}

## Suppression d'un mot d'un modèle de langue personnalisé
{: #deleteWord}

Utilisez la méthode `DELETE /v1/customizations/{customization_id}/words/{word_name}` pour supprimer un mot d'un modèle de langue personnalisé. Employez cette méthode pour retirer des mots ajoutés par erreur, par exemple à partir d'un corpus avec des données incorrectes.

Vous pouvez supprimer un mot que vous avez ajouté à la ressource de mots du modèle personnalisé par n'importe quel moyen. Toutefois, vous ne pouvez pas supprimer un mot du vocabulaire de base du service. La suppression d'un mot dans un modèle personnalisé supprime uniquement la prononciation personnalisée du mot, le mot reste dans le vocabulaire de base.

Le retrait d'un mot dans un modèle personnalisé n'affecte pas le modèle tant que vous ne l'entraînez pas à nouveau avec la méthode `POST /v1/customizations/{customization_id}/train`. Si le modèle avait déjà été entraîné sur le mot, le modèle continue à appliquer le mot à la reconnaissance vocale même après que vous l'ayez supprimé de sa ressource de mots. Vous devez entraîner à nouveau le modèle pour répercuter la suppression.

### Exemple de demande
{: #deleteExample-word}

L'exemple suivant supprime le mot `IEEE` du modèle personnalisé avec l'ID de personnalisation (customization_id) spécifié :

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/IEEE"
```
{: pre}
