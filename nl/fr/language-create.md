---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-11"

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

# Création d'un modèle de langue personnalisé
{: #languageCreate}

Pour créer un modèle de langue personnalisé pour le service {{site.data.keyword.speechtotextshort}}, procédez comme suit :
{: shortdesc}

1.  [Créez un modèle de langue personnalisé](#createModel-language). Vous pouvez créer plusieurs modèles personnalisés pour un même domaine ou pour des domaines différents. Le processus est le même quel que soit le modèle que vous créez. Cependant, vous ne pouvez spécifier qu'un seul modèle personnalisé à la fois avec une demande de reconnaissance.
1.  [Ajoutez un corpus au modèle de langue personnalisé](#addCorpus). Un corpus est un document en texte brut qui utilise la terminologie d'un domaine en contexte. Le service élabore un vocabulaire pour un modèle personnalisé en extrayant des termes de différents corpus qui n'existent pas à l'origine dans son vocabulaire de base. Vous pouvez ajouter plusieurs corpus à un modèle personnalisé.
1.  [Ajoutez des mots au modèle de langue personnalisé](#addWords). Vous pouvez également ajouter des mots personnalisés à un modèle individuellement. Par ailleurs, vous pouvez utiliser les mêmes méthodes pour modifier des mots personnalisés extraits des corpus. Les méthodes vous permettent de spécifier la prononciation des mots et leur mode d'affichage dans la retranscription de la parole.
1.  [Entraînez le modèle de langue personnalisé](#trainModel-language). Lorsque vous ajoutez des mots au modèle personnalisé, vous devez entraîner le modèle avec ces mots. Cet entraînement prépare le modèle personnalisé pour qu'il soit utilisé dans la reconnaissance vocale. Le modèle n'utilise pas de mots nouveaux ou modifiés tant que vous ne l'avez pas entraîné.
1.  Après avoir entraîné votre modèle personnalisé, vous pouvez l'utiliser avec des demandes de reconnaissance. Si les données audio transmises pour la transcription contiennent des mots spécifiques à un domaine définis dans le modèle personnalisé, les résultats de la demande reflètent le vocabulaire amélioré du modèle. Pour plus d'informations, voir [Utilisation d'un modèle de langue personnalisé](/docs/services/speech-to-text/language-use.html).

Les étapes de création d'un modèle de langue personnalisé sont itératives. Vous pouvez ajouter des corpus, ajouter des mots, et entraîner ou ré-entraîner un modèle aussi souvent que nécessaire.

Vous pouvez également ajouter des grammaires à un modèle de langue personnalisé. Les grammaires limitent la réponse du service uniquement aux mots qu'elles reconnaissent. Pour plus d'informations, voir [Utilisation de grammaires avec des modèles de langue personnalisés](/docs/services/speech-to-text/grammar.html).

La personnalisation de modèle de langue est disponible pour la plupart des langues. Pour plus d'informations, voir [Support de langue pour la personnalisation](/docs/services/speech-to-text/custom.html#languageSupport).
{: note}

## Création d'un modèle de langue personnalisé
{: #createModel-language}

Vous utilisez la méthode `POST /v1/customizations` pour créer un modèle de langue personnalisé. Vous pouvez créer autant de modèles personnalisés que vous voulez, mais vous ne pouvez utiliser qu'un seul modèle à la fois avec une demande de reconnaissance vocale. Cette méthode accepte un objet JSON qui définit les attributs du nouveau modèle personnalisé en tant que corps de la demande.

<table>
  <caption>Tableau 1. Attributs d'un nouveau modèle de langue personnalisé</caption>
  <tr>
    <th style="width:20%; text-align:left">Paramètre</th>
    <th style="width:12%; text-align:center">Type de données</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>Obligatoire</em></td>
    <td style="text-align:center">Chaîne</td>
    <td>
      Nom défini par l'utilisateur pour désigner le nouveau modèle personnalisé. Utilisez un nom
      caractéristique du domaine du modèle personnalisé, par exemple <code>Medical
        custom model</code> ou <code>Legal custom model</code>. Utilisez un
      nom unique par rapport à tous les modèles de langue personnalisés que vous possédez.
      Utilisez un nom localisé qui correspond à la langue du modèle personnalisé.
    </td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>Obligatoire</em></td>
    <td style="text-align:center">Chaîne</td>
    <td>
      Nom du modèle de langue qui doit être personnalisé par le nouveau
      modèle personnalisé. Spécifiez l'un des modèles de langue pris en charge
      renvoyé par la méthode <code>GET /v1/models</code>. Le nouveau
      modèle ne peut être utilisé qu'avec le modèle de base qu'il personnalise.
    </td>
  </tr>
  <tr>
    <td><code>dialect</code><br/><em>Facultatif</em></td>
    <td style="text-align:center">Chaîne</td>
    <td>
      Dialecte de la langue spécifiée qui doit être utilisé avec le
      modèle personnalisé. Par défaut, le dialecte correspond à la langue
      du modèle de base ; par exemple, le dialecte est <code>en-US</code>
      pour les modèles <code>en-US_BroadbandModel</code> ou
      <code>en-US_NarrowbandModel</code> correspondant à
      l'anglais américain.<br/></br>
      Ce paramètre n'est pertinent que pour les modèles espagnols,
      pour lesquels le service crée un modèle personnalisé qui convient
      aux discours dans le dialecte indiqué :
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          <code>es-ES</code> pour l'espagnol castillan (valeur par défaut)
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>es-LA</code> pour l'espagnol latino-américain</li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>es-US</code> pour l'espagnol d'Amérique du Nord (mexicain)
        </li>
      </ul>
      Si vous spécifiez un dialecte, il doit être valide pour le modèle de base.
    </td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>Facultatif</em></td>
    <td style="text-align:center">Chaîne</td>
    <td>
      Description du nouveau modèle. Utilisez une description localisée
      correspondant à la langue du modèle personnalisé.
    </td>
  </tr>
</table>

L'exemple suivant crée un modèle de langue personnalisé nommé `Example model`. Ce modèle est créé pour le modèle de base `en-US-BroadbandModel` avec la description `Example custom language model`. L'en-tête `Content-Type` indique que les données JSON sont transmises à la méthode.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Example model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom language model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations"
```
{: pre}

L'exemple renvoie l'ID de personnalisation du nouveau modèle. Chaque modèle personnalisé est identifié par un ID de personnalisation unique, correspondant à un identificateur global unique (GUID). Vous spécifiez le GUID d'un modèle personnalisé dans le paramètre `customization_id` des appels qui sont associés au modèle.

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

Le nouveau modèle personnalisé appartient à l'instance de service dont les données d'identification sont utilisées pour créer le service. Pour plus d'informations, voir [Propriété des modèles personnalisés](/docs/services/speech-to-text/custom.html#customOwner).

## Ajout d'un corpus au modèle de langue personnalisé
{: #addCorpus}

Lorsque vous avez créé votre modèle de langue personnalisé, la prochaine étape consiste à lui ajouter des données (mots spécifiques à un domaine). La méthode recommandée pour alimenter un modèle personnalisé avec des mots nouveaux est d'ajouter un ou plusieurs corpus.

Un corpus est un fichier en texte brut qui contient idéalement des exemples de phrases tirées de votre domaine. Le service analyse le contenu d'un fichier de corpus et en extrait les mots qui ne figurent pas dans son vocabulaire de base. Ces mots sont désignés par OOV (Out-Of-Vocabulary).

-   Pour plus d'informations sur l'utilisation des corpus, voir [Utilisation des corpus](/docs/services/speech-to-text/language-resource.html#workingCorpora).
-   Pour en savoir plus sur comment le service ajoute des corpus à un modèle, voir [Que se passe-t-il lorsque j'ajoute un fichier de corpus ?](/docs/services/speech-to-text/language-resource.html#parseCorpus)

En fournissant des phrases avec des mots nouveaux, les corpus permettent au service d'apprendre ces mots dans leur contexte. Vous pouvez enrichir ou modifier les mots du modèle individuellement. L'entraînement d'un modèle uniquement sur des mots individuels par opposition à des corpus prend plus de temps et peut produire des résultats moins probants.
{: tip}

Vous utilisez la méthode `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` pour ajouter un corpus à un modèle personnalisé :

-   Spécifiez l'ID de personnalisation du modèle personnalisé avec le paramètre de chemin `customization_id`.
-   Indiquez un nom pour le corpus avec le paramètre de chemin `corpus_name`. Utilisez un nom localisé qui correspond à la langue du modèle personnalisé et qui reflète le contenu du corpus.
    -   Indiquez un nom ne dépassant pas 128 caractères.
    -   Ne mettez pas d'espace, de barre oblique '`/`' ou de barre oblique inversée `\` dans le nom.
    -   N'utilisez pas le nom d'un corpus qui a déjà été ajouté au modèle personnalisé.
    -   N'utilisez pas le nom `user`, qui est réservé par le service pour désigner des mots personnalisés ajoutés ou modifiés par l'utilisateur.
-   Transmettez le fichier texte du corpus en tant que corps de la demande.

Vous pouvez ajouter jusqu'à 90 mille mots OOV maximum et 10 millions de mots au total provenant de toutes sources confondues. Ces valeurs comprennent les mots issus de corpus et de grammaires, ainsi que les mots que vous ajoutez directement. Pour plus d'informations, voir [De quelle quantité de données ai-je besoin ?](/docs/services/speech-to-text/language-resource.html#wordsResourceAmount)
{: note}

L'exemple suivant ajoute le fichier texte de corpus `healthcare.txt` au modèle personnalisé avec l'ID spécifié. Dans cet exemple, le corpus est nommé `healthcare`.

```bash
curl -X POST -u "apikey:{apikey}"
--data-binary @healthcare.txt
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/healthcare"
```
{: pre}

La méthode accepte également le paramètre de requête facultatif `allow_overwrite` qui remplace un corpus existant pour un modèle personnalisé. Utilisez ce paramètre si vous devez mettre à jour un fichier de corpus après l'avoir ajouté à un modèle.

Cette méthode est asynchrone. Son exécution nécessite environ une minute ou deux. La durée de l'opération dépend du nombre total de mots dans le corpus, du nombre de mots nouveaux que le service détecte dans le corpus et de la charge en cours sur le service. Pour plus d'informations sur la vérification du statut d'un corpus, voir [Surveillance de la demande d'ajout d'un corpus](#monitorCorpus).

Vous pouvez ajouter n'importe quel nombre de corpus à un modèle personnalisé en appelant la méthode une fois pour chaque fichier texte de corpus. L'ajout d'un corpus doit être entièrement terminé avant d'en ajouter un autre. Après avoir ajouté un corpus à un modèle personnalisé, examinez les nouveaux mots personnalisés pour rechercher d'éventuelles erreurs de typographie ou d'autres erreurs. Pour plus d'informations, voir [Validation d'une ressource de mots](/docs/services/speech-to-text/language-resource.html#validateModel).

### Surveillance de la demande d'ajout d'un corpus
{: #monitorCorpus}

Le service renvoie le code de réponse 201 si le corpus est valide. Il traite ensuite le contenu du corpus de manière asynchrone et extrait automatiquement les mots nouveaux. Vous ne pouvez pas soumettre de demande d'ajout de données à un modèle personnalisé ou entraîner le modèle tant que le service n'a pas terminé l'analyse du corpus pour la demande en cours.

Pour déterminer le statut de l'analyse, utilisez la méthode `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` permettant d'interroger le statut du corpus. Cette méthode accepte l'ID du modèle et le nom du corpus, comme illustré dans l'exemple suivant :

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

La réponse comprend le statut du corpus :

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

La zone `status` a l'une des valeurs suivantes :

-   `analyzed` indique que l'analyse du corpus par le service a abouti.
-   `being_processed` indique que le service est encore en train d'analyser le corpus.
-   `undetermined` indique que le service a rencontré une erreur lors du traitement du corpus.

Utilisez une boucle pour vérifier le statut du corpus toutes les 10 secondes jusqu'à ce qu'il passe à `analyzed`. Pour plus d'informations sur la vérification de statut des corpus d'un modèle, voir [Affichage de la liste des corpus d'un modèle de langue personnalisé](/docs/services/speech-to-text/language-corpora.html#listCorpora).

## Ajout de mots au modèle de langue personnalisé
{: #addWords}

Bien que l'ajout de corpus soit la méthode recommandée pour ajouter des mots à un modèle de langue personnalisé, vous pouvez également ajouter directement des mots personnalisés individuels au modèle. Le service ajoute ces mots au modèle personnalisé de la même manière que les mots OOV qu'il a reconnus dans les corpus.

Si vous n'avez qu'un seul mot ou quelques mots à ajouter au modèle, l'utilisation de corpus pour ajouter les mots n'apparaît pas comme une solution pratique ou viable. L'approche la plus simple est d'ajouter un mot avec son orthographe. Mais vous pouvez également fournir plusieurs prononciations du mot et indiquer comment il doit être affiché.

-   Pour plus d'informations sur l'ajout direct de mots, voir [Utilisation des mots personnalisés](/docs/services/speech-to-text/language-resource.html#workingWords).
-   Pour en savoir plus sur comment le service ajoute des mots personnalisés à un modèle, voir [Que se passe-t-il lorsque j'ajoute ou modifie un mot personnalisé ?](/docs/services/speech-to-text/language-resource.html#parseWord)

Vous pouvez utiliser les méthodes suivantes pour ajouter des mots à un modèle personnalisé :

-   La méthode `POST /v1/customizations/{customization_id}/words` ajoute plusieurs mots à la fois. Vous transmettez un objet JSON qui fournit des informations sur chaque mot via le corps de la demande. L'exemple suivant ajoute deux mots personnalisés, `HHonors` et `IEEE`, au modèle personnalisé avec l'ID spécifié. L'en-tête `Content-Type` indique que les données JSON sont transmises à la méthode.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"words\": [
      {\"word\": \"HHonors\", \"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"},
      {\"word\": \"IEEE\", \"sounds_like\": [\"I. triple E.\"]}]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    Vous pouvez également ajouter ces mots à partir d'un fichier. Par exemple, le fichier `words.json` définit ces deux mots personnalisés.

    ```
    {"words":
      [
        {"word": "HHonors", "sounds_like": ["hilton honors", "H. honors"], "display_as": "HHonors"},
        {"word": "IEEE", "sounds_like": ["I. triple E."]}
      ]
    }
    ```
    {: codeblock}

    La commande suivante ajoute les mots à partir du fichier :

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data-binary @words.json
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    Cette méthode est asynchrone. Sa durée d'exécution dépend du nombre de mots que vous ajoutez et de la charge en cours sur le service. Pour plus d'informations sur la vérification du statut de l'opération, voir [Surveillance de la demande d'ajout de mots](#monitorWords).
-   La méthode `PUT /v1/customizations/{customization_id}/words/{word_name}` ajoute des mots individuels. Vous transmettez un objet JSON qui fournit des informations sur le mot. L'exemple suivant ajoute le mot `NCAA` au modèle avec l'ID spécifié. L'en-tête `Content-Type` indique à nouveau que les données JSON sont transmises à la méthode.

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

    Cette méthode est synchrone. Le service renvoie un code de réponse qui indique le succès ou la réussite d'une demande immédiatement.

Comme pour l'ajout de corpus, examinez les nouveaux mots personnalisés pour rechercher d'éventuelles erreurs typographiques ou d'autres erreurs. Cette vérification est importante lorsque vous ajoutez plusieurs mots en même temps. Pour plus d'informations, voir [Validation d'une ressource de mots](/docs/services/speech-to-text/language-resource.html#validateModel).

### Surveillance de la demande d'ajout de mots
{: #monitorWords}

Lorsque vous utilisez la méthode `POST /v1/customizations/{customization_id}/words`, le service renvoie le code de réponse 201 si les données d'entrée sont valides. Il traite ensuite les mots de manière asynchrones pour les ajouter au modèle. L'opération est en général plus rapide que l'ajout d'un corpus ou l'entraînement d'un modèle, mais sa durée dépend du nombre de mots nouveaux que vous ajoutez et de la charge en cours sur le service. Vous ne pouvez pas soumettre des demandes d'ajout de données au modèle personnalisé, ni entraîner le modèle tant que le service n'a pas terminé la demande d'ajout des mots nouveaux.

Pour déterminer le statut de la demande, utilisez la méthode `GET /v1/customizations/{customization_id}` pour interroger le statut du modèle. La méthode accepte l'ID de personnalisation du modèle et renvoie les informations avec le statut du modèle, comme illustré dans l'exemple suivant :

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

La zone `status` indique l'état en cours du modèle. Tant que le service traite les mots nouveaux, le statut reste `pending`. Utilisez une boucle pour vérifier le statut toutes les 10 secondes jusqu'à ce qu'il passe à `ready` pour indiquer que l'opération est terminée. Pour plus d'informations sur les valeurs possibles pour `status`, voir [Surveillance de la demande d'entraînement du modèle](#monitorTraining-language).

### Modification des mots dans un modèle personnalisé
{: #modifyWord}

Vous pouvez également utiliser les méthodes `POST /v1/customizations/{customization_id}/words` et `PUT /v1/customizations/{customization_id}/words/{word_name}` pour modifier ou compléter un mot dans un modèle personnalisé. Vous serez peut-être amené à utiliser ces méthodes pour corriger une erreur typographique ou une autre faute qui s'est glissée lorsque le mot a été ajouté au modèle. Il vous faudra peut-être également ajouter des définitions de prononciations possibles pour un mot existant. 

Vous utilisez ces méthodes pour modifier la définition d'un mot existant exactement de la même manière que vous procédez pour ajouter un mot. Les nouvelles données que vous fournissez au mot remplacent la définition existante du mot.

## Entraînement du modèle de langue personnalisé
{: #trainModel-language}

Lorsque vous avez alimenté un modèle de langue personnalisé avec des mots nouveaux (en ajoutant des corpus, des grammaires ou par ajout direct de mots), vous devez entraîner le modèle sur les nouvelles données. Cet entraînement prépare le modèle personnalisé à utiliser ces données dans la reconnaissance vocale. Le modèle n'utilise pas les mots que vous ajoutez avec la méthode de votre choix, tant que vous ne l'entraînez pas sur les données.

Pour entraîner un modèle personnalisé, vous utilisez la méthode `POST /v1/customizations/{customization_id}/train`. Vous transmettez à la méthode, l'ID de personnalisation (customization_id) du modèle que vous souhaitez entraîner, comme illustré dans l'exemple suivant :

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

Vous pouvez utiliser le paramètre de requête facultatif `word_type_to_add` pour spécifier les mots sur lesquels doit être entraîné le modèle personnalisé :

-   Spécifiez `all` ou omettez ce paramètre pour entraîner le modèle sur tous les mots qu'il comporte, quelle que soit leur origine.
-   Spécifiez `user` pour entraîner le modèle uniquement sur les mots qui ont été ajoutés ou modifiés par l'utilisateur, en ignorant les mots qui ont été extraits uniquement à partir des corpus ou des grammaires.

    Cette option s'avère utile si vous ajoutez des corpus avec des données bruitées, par exemple des mots contenant des erreurs typographiques. Avant d'entraîner le modèle sur des données de ce type, utilisez le paramètre de requête `word_type` de la méthode `GET /v1/customizations/{customization_id}/words` pour réviser les mots extraits de corpus ou de grammaires. Pour plus d'informations, voir [Affichage de la liste des mots d'un modèle de langue personnalisé](/docs/services/speech-to-text/language-words.html#listWords).

Par ailleurs, vous pouvez utiliser le paramètre de requête facultatif `customization_weight`. Ce paramètre indique le poids relatif attribué aux mots du modèle personnalisé par opposition aux mots du vocabulaire de base lorsque le modèle personnalisé est utilisé pour la reconnaissance vocale. Vous pouvez également indiquer un poids de personnalisation avec toutes les demandes de reconnaissance utilisant le modèle personnalisé. Pour plus d'informations, voir [Utilisation d'un poids de personnalisation](/docs/services/speech-to-text/language-use.html#weight).

Cette méthode est asynchrone. L'entraînement peut prendre quelques minutes en fonction du nombre de mots nouveaux sur lesquels est entraîné le modèle et de la charge en cours sur le service. Pour plus d'informations sur la vérification du statut d'une opération d'entraînement, voir [Surveillance de la demande d'entraînement du modèle](#monitorTraining-language).

### Surveillance de la demande d'entraînement du modèle
{: #monitorTraining-language}

Le service renvoie le code de réponse 200 s'il a initié avec succès le processus d'entraînement. Il n'accepte pas d'autres demandes d'entraînement ou des demandes d'ajout de nouveaux corpus, de nouvelles grammaires ou de mots nouveaux, tant que la demande existante n'est pas terminée.

Pour déterminer le statut d'une demande d'entraînement, utilisez la méthode `GET /v1/customizations/{customization_id}` pour interroger le statut du modèle. La méthode accepte l'ID de personnalisation du modèle et renvoie des informations concernant le modèle.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
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

La réponse comprend les zones `status` et `progress` qui indiquent l'état du modèle personnalisé. La signification de la zone `progress` dépend du statut du modèle. La zone `status` peut avoir l'une des valeurs suivantes :

-   `pending` indique que le modèle a été créé mais attend que des données d'entraînement soient ajoutées ou que le service termine l'analyse des données qui ont été ajoutées. La zone `progress` a la valeur `0`.
-   `ready` indique que le modèle est prêt pour l'entraînement. La zone `progress` a la valeur `0`.
-   `training` indique que le modèle est en cours d'entraînement. La zone `progress` passe de `0` à `100` lorsque l'entraînement est terminé. <!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available` indique que l'entraînement du modèle est terminé et que le modèle est prêt à l'emploi. La zone `progress` a la valeur `100`.
-   `upgrading` indique que le modèle est en cours de mise à niveau. La zone `progress` a la valeur `0`.
-   `failed` indique que l'entraînement du modèle a échoué. La zone `progress` a la valeur `0`.

Utilisez une boucle pour vérifier le statut toutes les 10 secondes jusqu'à ce qu'il passe à `available`. Pour plus d'informations sur la vérification du statut d'un modèle personnalisé, voir [Affichage de la liste des modèles de langue personnalisés](/docs/services/speech-to-text/language-models.html#listModels-language).

### Echec d'entraînement
{: #failedTraining-language}

L'entraînement ne parvient pas à démarrer si le service traite une autre demande pour le modèle de langue personnalisé. Par exemple, une demande d'entraînement échoue si le service est en train d'effectuer les opérations suivantes :

-   Traitement d'un corpus ou d'une grammaire pour générer une liste de mots OOV (Out Of Vocabulary)
-   Traitement de mots personnalisés pour valider ou générer automatiquement des prononciations possibles
-   Traitement d'une autre demande d'entraînement

De même, l'entraînement ne parvient pas à démarrer pour les raisons suivantes :

-   Il n'y a pas eu de données d'entraînement (corpus, grammaires ou mots) ajoutées au modèle personnalisé depuis sa création ou son dernier entraînement.
-   Un ou plusieurs mots ont été ajoutés au modèle avec des prononciations possibles non valides que vous devez corriger.

Si le statut d'entraînement d'un modèle personnalisé est `failed`, utilisez des méthodes de l'interface de personnalisation pour examiner les mots du modèle et corriger les erreurs que vous trouvez. Pour plus d'informations, voir [Validation d'une ressource de mots](/docs/services/speech-to-text/language-resource.html#validateModel).

## Exemples de scripts
{: #exampleScripts}

Vous pouvez utiliser les scripts suivants pour tester les étapes de création d'un modèle de langue personnalisé :

-   Un script Python nommé <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a>. Pour plus d'informations, voir [Exemple de script Python](#pythonScript).
-   Un script shell bash nommé <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a>. Pour plus d'informations, voir [Exemple de script shell](#shellScript).

Les deux scripts fournissent des fonctionnalités identiques. Chaque script crée un modèle personnalisé, ajoute des mots à partir d'un fichier texte de corpus et ajoute directement un ou plusieurs mots au modèle. Le script interroge le modèle pour obtenir la liste des mots ajoutés à partir d'un corpus et ajoutés directement par l'utilisateur. Il entraîne aussi le modèle et présente l'interrogation recommandée pour surveiller les résultats des opérations asynchrones.

Vous pouvez utiliser l'un des deux fichiers texte de corpus fournis avec les scripts, ou vous pouvez effectuer les tests avec vos propres fichiers de corpus :

-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/corpus.txt" download="corpus.txt">corpus.txt <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a> est un corpus abrégé de 6 ko qui ajoute six termes dans le domaine de la santé à un modèle. Ce fichier produit une sortie de petite taille lorsqu'il est utilisé avec les scripts. Les scripts utilisent ce fichier par défaut.
-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/healthcare.txt" download="healthcare.txt">healthcare.txt <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a> est un corpus plus riche de 164 ko qui ajoute de nombreux termes liés à la santé à un modèle. Ce fichier produit plus de résultats lorsqu'il est utilisé avec les scripts.

Par défaut, le nouveau modèle personnalisé que vous créez avec l'un de ces scripts est prêt à être utilisé avec les demandes de reconnaissance. Les scripts comprennent une étape facultative pour supprimer le nouveau modèle personnalisé, ce qui peut s'avérer utile lorsque vous êtes en train d'expérimenter le processus. Suivez les commentaires indiqués dans les scripts pour activer l'étape de suppression.

### Exemple de script Python
{: #pythonScript}

Pour utiliser le script Python, procédez comme suit :

1.  Téléchargez le script Python nommé <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a>.
1.  Téléchargez des exemples de fichiers de corpus à utiliser avec le script. Vous êtes libre de tester avec l'un des fichiers texte de corpus ou avec un fichier de votre choix. Par défaut, tous les fichiers texte de corpus doivent résider dans le même répertoire que le script.
1.  Le script utilise la bibliothèque Python `requests` pour les demandes HTTP envoyées au service. Utilisez `pip` ou `easy_install` pour installer la bibliothèque qu'utilisera le script, par exemple :

    ```bash
    pip install requests
    ```
    {: pre}

    Pour plus d'informations sur cette bibliothèque, voir [pypi.python.org/pypi/requests ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://pypi.python.org/pypi/requests){: new_window}.
1.  Editez le script pour remplacer la chaîne `password` `iam_apikey` par la clé d'API de vos données d'identification de service {{site.data.keyword.speechtotextshort}} :

    ```
    password = "iam_apikey"
    ```
    {: codeblock}

1.  Exécutez le script en entrant la commande suivante :

    ```bash
    python testSTTcustom.py
    ```
    {: pre}

Le script utilise l'URL par défaut suivante pour l'emplacement Dallas : `https://stream.watsonplatform.net`. Si vous avez créé votre instance de service à un autre endroit, modifiez les variables `uri` pour utiliser votre emplacement. Par exemple, utilisez `https://gateway-wdc.watsonplatform.net` si votre instance de service réside à Washington, DC.
{: note}

### Exemple de script shell
{: #shellScript}

Pour utiliser un script shell bash, procédez comme suit :

1.  Téléchargez le script shell nommé <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a>.
1.  Téléchargez des exemples de fichiers de corpus à utiliser avec le script. Vous êtes libre de tester avec l'un des fichiers texte de corpus ou avec un fichier de votre choix. Par défaut, tous les fichiers texte de corpus doivent résider dans le même répertoire que le script.
1.  Le script utilise la commande `curl` pour les demandes HTTP envoyées au service. Si vous n'avez pas déjà téléchargé `curl`, vous pouvez installer la version correspondant à votre système d'exploitation à partir du site [curl.haxx.se ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://curl.haxx.se){: new_window}. Installez la version compatible avec le protocole Secure Sockets Layer (SSL) et veillez à inclure le fichier binaire installé dans votre variable d'environnement `PATH`.
1.  Editez le script pour remplacer la chaîne `PASSWORD` `iam_apikey` par la clé d'API de vos données d'identification de service {{site.data.keyword.speechtotextshort}} :

    ```
    PASSWORD="iam_apikey"
    ```
    {: codeblock}

1.  Editez le script pour remplacer la chaîne `URL` par l'URL correspondant à l'emplacement où vous avez créé votre instance de service. Le script utilise l'URL par défaut suivante correspondant à l'emplacement Dallas :

    ```
    URL="https://stream.watsonplatform.net/speech-to-text/api/v1"
    ```
    {: codeblock}

1.  Assurez-vous que le script dispose des droits d'exécution, puis exécutez le script en entrant la commande suivante :

    ```bash
    ./testSTTcustom.sh
    ```
    {: pre}
