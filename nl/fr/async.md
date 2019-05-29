---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

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

# L'interface HTTP asynchrone
{: #async}

L'interface HTTP asynchrone du service {{site.data.keyword.speechtotextfull}} offre des méthodes de transcription audio via des appels non bloquants au service. Cette interface emploie des signatures numériques et des chaînes secrètes définies par l'utilisateur afin d'assurer un certain niveau de sécurité pour les demandes effectuées via le protocole HTTP. Pour utiliser l'interface asynchrone, vous pouvez
{: shortdesc}

-   enregistrer une URL de rappel pour recevoir des notifications automatiques du service sur le statut du travail et les résultats.
-   interroger le service pour obtenir le statut du travail et les résultats manuellement.

Ces deux approches ne s'excluent pas mutuellement. Vous pouvez choisir de recevoir des notifications de rappel tout en continuant à interroger le service pour obtenir le dernier statut ou à contacter le service pour extraire les résultats manuellement. Les sections suivantes indiquent comment utiliser l'interface HTTP asynchrone avec ces deux approches.

Soumettez au maximum 1 Go et au minimum 100 octets de données audio avec une seule demande. Pour plus d'informations sur les formats audio et sur l'utilisation de la compression pour maximiser la quantité de données audio que vous pouvez envoyer avec une demande, voir [Formats audio](/docs/services/speech-to-text/audio-formats.html). Pour plus d'informations sur les méthodes individuelles de l'interface, voir [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

## Modèles d'utilisation
{: #usage}

Lorsque vous utilisez l'interface HTTP asynchrone, vous pouvez choisir d'obtenir le statut du travail et de recevoir les résultats ainsi :

-   Utiliser des notifications de rappel :
    1.  Appelez la méthode `POST /v1/register_callback` pour enregistrer une URL de rappel avec le service. Vous pouvez fournir une chaîne secrète spécifiée par l'utilisateur pour permettre l'authentification et assurer l'intégrité des données pour les rappels envoyés à l'URL.
    1.  Appelez la méthode `POST /v1/recognitions` avec une URL de rappel déjà enregistrée, à laquelle le service envoie des notifications à chaque changement de statut du travail. Vous spécifiez la liste des événements pour lesquels vous souhaitez recevoir des notifications. Par défaut, le service envoie des notifications au démarrage et à la fin d'un travail ou si une erreur se produit. Vous pouvez également demander les résultats de la demande dans une notification de fin. Autrement, vous devez utiliser la méthode `GET /v1/recognitions/{id}` pour extraire les résultats.
-   Interroger le service :
    1.  Appelez la méthode `POST /v1/recognitions` sans URL de rappel, événement ou jeton utilisateur.
    1.  Appelez régulièrement la méthode `GET /v1/recognitions` pour vérifier le statut des travaux les plus récents ou la méthode `GET /v1/recognitions/{id}` pour vérifier le statut d'un travail spécifique.
    1.  Si vous vérifiez le statut du travail avec la méthode `GET /v1/recognitions`, appelez la méthode `GET /v1/recognitions/{id}` pour extraire les résultats une fois le travail terminé.

Les deux approches peuvent être utilisées ensemble. Vous pouvez continuer à interroger le service pour obtenir le dernier statut d'un travail créé avec une URL de rappel. Par exemple, vous envisagerez peut-être d'obtenir le statut d'un travail si les notifications mettent un certain temps à arriver. Vous souhaiterez peut-être également vérifier le statut si vous suspectez que vous avez manqué une ou plusieurs notifications en raison d'une erreur de service ou d'une erreur réseau.

## Enregistrement d'une URL de rappel
{: #register}

Vous enregistrez une URL de rappel en appelant la méthode `POST /v1/register_callback`. Une fois cette URL enregistrée, vous pouvez l'utiliser afin de recevoir des notifications pour un nombre indéfini de travaux. Le processus d'enregistrement comprend quatre étapes :

1.  Vous appelez la méthode `POST /v1/register_callback` et transmettez une URL de rappel. Vous pouvez éventuellement indiquer une valeur secrète spécifiée par l'utilisateur. Le service utilise cette valeur pour calculer les signatures du code d'authentification de message par clé (HMAC) de l'algorithme de hachage sécurisé SHA1 pour assurer l'authentification et l'intégrité des données. L'exemple suivant enregistre un rappel utilisateur qui répond sur l'URL `http://{user_callback_path}/results`. Cet appel comprend la valeur secrète de l'utilisateur `ThisIsMySecret`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/register_callback?callback_url=http://{user_callback_path}/results&user_secret=ThisIsMySecret"
    ```
    {: pre}

1.  Le service effectue une tentative de validation ou d'inscription sur liste blanche de l'URL de rappel si cette URL n'est pas déjà enregistrée en envoyant une demande `GET` à l'URL de rappel. Le service transmet une chaîne de demande d'accès alphanumérique aléatoire via le paramètre de requête `challenge_string` de la demande. La demande comprend un en-tête `Accept` qui indique `text/plain` comme type de réponse obligatoire.

    Si l'appel à la méthode `register_callback` comprenait une valeur secrète d'utilisateur, la demande `GET` du service comprend également un en-tête `X-Callback-Signature` pour spécifier la signature HMAC-SHA1 de la chaîne de demande d'accès. Le service calcule la signature en utilisant la valeur secrète de l'utilisateur comme clé.

    ```
    GET http://{user_callback_path}/results?challenge_string=n9ArPGMQ36Hiu7QC
    header: X-Callback-Signature {HMAC-SHA1_signature}
    ```
    {: codeblock}

1.  Vous répondez à la demande `GET` du service avec le code de statut 200. Incluez la chaîne de demande d'accès envoyée par le service dans la réponse. Incluez la chaîne en texte brut dans le corps de la réponse et définissez l'en-tête de réponse `Content-Type` avec la valeur `text/plain`.

    Si la demande `POST` initiale comprenait une valeur secrète d'utilisateur, vous pouvez calculer une signature HMAC-SHA1 de chaîne de demande d'accès en utilisant cette valeur secrète comme clé. Si la demande `GET` était envoyée par le service, la signature correspond à la valeur indiquée par l'en-tête `X-Callback-Signature`.

    ```
    response code: 200 OK
    body: n9ArPGMQ36Hiu7QC
    ```
    {: codeblock}

1.  Le service vérifie si la chaîne de demande d'accès est renvoyée dans le corps de la réponse à sa demande `GET`. Dans l'affirmative, le service met l'URL de rappel sur liste blanche et répond à votre demande `POST` initiale avec le code de statut 201. Le corps de la réponse comprend un objet JSON avec une zone `status` avec la valeur `created` et une zone `url` avec la valeur de votre URL de rappel.

    ```
    response code: 201 Created
    body: {
      "status": "created",
      "url": "http://{user_callback_path}/results"
    }
    ```
    {: codeblock}

Le service envoie uniquement une seule demande `GET` à une URL de rappel lors du processus d'enregistrement. Il doit recevoir en retour un code de réponse 200 comprenant la chaîne de demande d'accès dans le corps du message dans un délai de cinq secondes. A défaut, il n'ajoutera pas l'URL sur liste blanche. Il enverra à la place le code de statut 400 en réponse à la demande `POST /v1/register_callback`. Si l'URL de rappel était déjà sur liste blanche, le service envoie le code de statut 200 en réponse à la demande `POST` initiale.

### Remarques sur la sécurité
{: #security-async}

Lorsque vous parvenez à enregistrer l'URL de rappel en utilisant la méthode `POST /v1/register_callback`, le service ajoute l'URL sur la liste blanche pour indiquer qu'elle a été agréée pour être utilisée avec des notifications de rappel. Si vous indiquez une valeur secrète d'utilisateur avec l'appel d'enregistrement, l'ajout de l'URL en liste blanche indique également qu'elle a été validée pour une sécurité renforcée. L'indication d'une valeur secrète d'utilisateur permet d'assurer l'authentification et l'intégrité des données utilisant l'URL de rappel avec l'interface HTTP asynchrone.

Le service utilise la valeur secrète de l'utilisateur pour calculer une signature HMAC-SHA1 sur le contenu de toutes les notifications de rappel qu'il envoie à l'URL. Il envoie la signature via l'en-tête `X-Callback-Signature` avec chaque notification. Le client peut utiliser la valeur secrète pour calculer sa propre signature dans chaque contenu de notification. Si la signature correspond à la valeur de l'en-tête `X-Callback-Signature`, le client sait que la notification a été envoyée par le service et que son contenu n'a pas été modifié lors de la transmission. Cela permet au client de s'assurer qu'il n'est pas victime d'une attaque dite de l'homme du milieu (MITM).

HTTPS est idéal pour les applications en production. Mais lors du développement et du prototypage de l'application, les notifications de rappel HTTP prises en charge par le service peuvent simplifier et accélérer le processus de développement en évitant le coût du protocole HTTPS.

<!--

However, communicating over the HTTPS protocol is always the most secure means of learning job status and retrieving results. Using the HTTPS `GET /v1/recognitions/{id}` method to retrieve the results of a job is therefore more secure that receiving the results via callback notification. While the use of HMAC-SHA1 signatures based on a user secret ensures authentication and data integrity for callback notifications, it does not provide confidentiality. Conversely, because it encrypts the body of the response, HTTPS can provide authentication, integrity, *and* confidentiality.

HTTPS, however, is not ideal in terms of additional development overhead. Moreover, although the service validates SSL certificates to prevent man-in-the-middle attacks when you use HTTPS, validation is not foolproof if you use self-signed certificates, which enable encryption but not authentication or data integrity. During application development, user secrets and digital signatures provide a suitable level of security for requests made over the HTTP protocol. You can work with HTTP callback notifications during prototyping and move to HTTPS only for application deployment.

-->

### Désenregistrement d'une URL de rappel
{: #unregister}

Vous pouvez annuler l'enregistrement d'une URL de rappel sur liste blanche à tout moment en appelant la méthode `POST /v1/unregister_callback`. Le désenregistrement d'une URL de rappel peut s'avérer utile pour tester votre application avec le service. Lorsque vous annulez l'enregistrement d'une URL de rappel, vous ne pouvez plus l'utiliser avec des demandes de reconnaissance asynchrones.

## Création d'un travail
{: #create}

Vous pouvez créer un travail de reconnaissance en appelant la méthode `POST /v1/recognitions`. Vous découvrez le statut et les résultats du travail selon l'approche que vous utilisez et les paramètres que vous transmettez :

-   *Pour utiliser des notifications de rappel*, incluez le paramètre de requête `callback_url` pour spécifier l'URL à laquelle le service doit envoyer des notifications de rappel à chaque changement de statut du travail. Vous pouvez également spécifier les paramètres de requête facultatifs suivants :
    -   `events` pour vous abonner à une liste d'événements de notification. Par défaut, le service envoie des notifications de rappel au démarrage du travail (événement `recognitions.started`), à la fin du travail (événement `recognitions.completed`) et si une erreur se produit (événement `recognitions.failed`). Vous pouvez préciser un sous-ensemble d'événements ou utiliser l'événement `recognitions.completed_with_results` à la place de l'événement `recognitions.completed` pour inclure les résultats avec la notification d'achèvement du travail (job-completed).
    -   `user_token` pour spécifier une chaîne à inclure dans chaque notification concernant le travail. Comme vous pouvez utiliser la même URL de rappel avec un nombre indéfini de travaux, vous pouvez tirer parti des jetons utilisateur pour distinguer les notifications de différents travaux.
-   *Pour utiliser l'interrogation*, omettez les paramètres de requête `callback_url`, `events` et `user_token`. Vous devez ensuite utiliser les méthodes `GET /v1/recognitions` ou `GET /v1/recognitions/{id}` pour vérifier le statut du travail, en utilisant ce statut pour extraire les résultats une fois le travail terminé.

Dans les deux cas, vous pouvez inclure le paramètre de requête `results_ttl` pour spécifier la durée en minutes de la disponibilité des résultats une fois le travail terminé.

En plus des paramètres précédents, qui sont propres à l'interface asynchrone, la méthode `POST /v1/recognitions` prend en charge la plupart des paramètres utilisés par les interfaces WebSocket et HTTP synchrone. Pour plus d'informations, voir [Récapitulatif des paramètres](/docs/services/speech-to-text/summary.html).

### Notifications de rappel
{: #notifications}

Si le travail est créé avec une URL de rappel, le service envoie une notification de rappel HTTP `POST` à l'URL enregistrée lorsqu'un événement spécifié se produit. Le corps d'une notification de base est constitué d'un objet JSON structuré ainsi :

```javascript
{
  "id": "{job_id}",
  "event": "{recognitions_status}",
  "user_token": "{user_token}"
}
```
{: codeblock}

La zone `id` identifie l'ID du travail qui a généré le rappel et la zone `event` identifie l'événement qui a déclenché le rappel. La zone `user_token` comprend le jeton utilisateur du travail, le cas échéant. Autrement, cette zone est une chaîne vide. S'il s'agit d'un événement `recognitions.completed_with_results`, l'objet comprend une zone `results` qui fournit les résultats de la demande de reconnaissance. Le client peut répondre à la notification de rappel avec le code de statut 200.

Si l'URL de rappel a été enregistrée avec une valeur secrète d'utilisateur, le service envoie également l'en-tête `X-Callback-Signature` avec la notification de rappel. Cet en-tête spécifie la signature HMAC-SHA1 du corps de la demande. Le service calcule la signature en utilisant la valeur secrète de l'utilisateur comme clé. Le client peut calculer la signature du contenu de la notification de rappel pour s'assurer qu'elle correspond à la signature figurant dans l'en-tête. Par exemple, le code Python simple suivant calcule la signature de la chaîne `notification_payload` :

```python
from hashlib import sha1
import hmac
hashed = hmac.new(user_secret, notification_payload, sha1)
signature = hashed.digest().encode("base64").rstrip('\n')
```
{: codeblock}

### Exemple de rappel
{: #callback}

L'exemple suivant crée un travail associé à l'URL de rappel `http://{user_callback_path}/results` qui a été ajoutée auparavant sur liste blanche. L'exemple transmet le jeton utilisateur `job25` pour identifier le travail dans les notifications de rappel envoyées par le service. Comme l'appel utilise les événements par défaut, l'utilisateur doit appeler la méthode `GET /v1/recognitions/{id}` pour extraire les résultats lorsque le service envoie une notification de rappel pour signaler que le travail est terminé. L'appel définit le paramètre de requête `timestamps` de la demande de reconnaissance avec la valeur `true`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?callback_url=http://{user_callback_path}/results&user_token=job25&timestamps=true"
```
{: pre}

Le service renvoie le statut de la demande, qui correspond à `waiting` pour indiquer que le service prépare le travail en vue de son traitement. La réponse comprend l'heure de création, l'ID du travail et l'URL où obtenir plus d'informations sur le travail.

```javascript
{
  "created": "2016-08-17T19:15:17.926Z",
  "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bd734c0-e575-21f3-de03-f932aa0468a0",
  "status": "waiting"
}
```
{: codeblock}

### Exemple d'interrogation
{: #polling}

L'exemple suivant crée un travail qui n'est pas associé à une URL de rappel. L'utilisateur doit interroger le service pour savoir à quel moment se termine le travail, puis extraire les résultats avec la méthode `GET /v1/recognitions/{id}`. Comme dans l'exemple précédent, l'appel définit le paramètre `timestamps` de la demande de reconnaissance avec la valeur `true`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?timestamps=true"
```
{: pre}

Le service renvoie le statut `processing` pour indiquer qu'il est en train de traiter le travail, ainsi que l'heure de création, l'ID du travail et l'URL permettant d'obtenir des informations sur le travail.

```javascript
{
  "created": "2016-08-17T19:13:23.622Z",
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "status": "processing"
}
```
{: codeblock}

## Vérification du statut et extraction des résultats d'un travail
{: #job}

Vous appelez la méthode `GET /v1/recognitions/{id}` pour vérifier le statut du travail qui est indiqué avec le paramètre de chemin `id`. La réponse comprend toujours l'ID et le statut du travail et son heure de création ou de mise à jour. Si le statut est `completed`, la réponse comprend également les résultats de la demande de reconnaissance.

La méthode `GET /v1/recognitions/{id}` est le seul moyen d'extraire les résultats du travail dans les cas suivants :

-   Le travail a été soumis sans URL de rappel.
-   Le travail a été soumis avec une URL de rappel mais sans spécifier l'événement `recognitions.completed_with_results`.
-   Le travail ne figure pas parmi les 100 derniers travaux en attente. Lorsque vous omettez le paramètre de chemin `id`, seuls les 100 derniers travaux sont renvoyés.

Cependant, vous pouvez continuer à utiliser cette méthode pour extraire les résultats d'un travail ayant spécifié une URL de rappel et l'événement `recognitions.completed_with_results`. Vous pouvez extraire les résultats d'un travail autant de fois que vous le souhaitez tant qu'ils sont disponibles. Un travail et ses résultats sont disponibles jusqu'à ce que vous les supprimiez avec la méthode `DELETE /v1/recognitions/{id}` ou jusqu'à ce que la durée de vie du travail soit écoulée, selon ce qui survient en premier. Par défaut, les résultats arrivent à expiration au bout d'une semaine sauf si vous indiquez une autre durée de vie avec le paramètre `results_ttl` de la méthode `POST /v1/recognitions`.

### Exemple de statut sans résultats
{: #withoutResults}

L'exemple suivant vérifie le statut du travail avec l'ID spécifié. Comme le travail n'est pas encore terminé, la réponse ne contient pas les résultats.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}

```javascript
{
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "created": "2016-08-17T19:13:23.622Z",
  "updated": "2016-08-17T19:13:24.434Z",
  "status": "processing"
}
```
{: codeblock}

### Exemple de statut avec résultats
{: #withResults}

L'exemple suivant demande le statut du travail avec l'ID spécifié. Comme le travail est terminé, la réponse contient les résultats de la demande.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}

```javascript
{
  "id": "398fcd80-330a-22ba-93ce-1a73f454dd98",
  "results": [
    {
      "result_index": 0,
      "results": [
        {
          "final": true,
          "alternatives": [
            {
              "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday ",
              "timestamps": [
                [
                  "several",
                  1,
                  1.52
                ],
                [
                  "tornadoes",
                  1.52,
                  2.15
                ],
                . . .
                [
                  "Sunday",
                  5.74,
                  6.33
                ]
              ],
              "confidence": 0.89
            }
          ]
        }
      ]
    }
  ],
  "created": "2016-08-17T19:11:04.298Z",
  "updated": "2016-08-17T19:11:16.003Z",
  "status": "completed"
}
```
{: codeblock}

## Vérification du statut des derniers travaux
{: #jobs}

Vous appelez la méthode `GET /v1/recognitions` pour vérifier le statut des derniers travaux. La méthode renvoie le statut des 100 travaux en attente les plus récents qui sont associés aux données d'identification du service avec lequel elle est appelée. La méthode renvoie l'ID et le statut de chaque travail, ainsi que son heure de création et de mise à jour. Si un travail a été créé avec une URL de rappel et un jeton utilisateur, la méthode renvoie également le jeton utilisateur du travail.

La réponse comprend l'un des états suivants :

-   `waiting` si le service est en cours de préparation du travail pour le traiter. Ce statut correspond à l'état initial de tous les travaux. Le travail demeure dans cet état jusqu'à ce que le service soit en mesure de commencer son traitement.
-   `processing` si le service est en train de traiter le travail.
-   `completed` si le service a terminé le traitement du travail. Si le travail spécifiait une URL de rappel et l'événement `recognitions.completed_with_results`, le service a renvoyé les résultats avec la notification de rappel. Autrement, utilisez la méthode `GET /v1/recognitions/{id}` pour obtenir les résultats.
-   `failed` si le travail a échoué pour une raison quelconque.

Un travail et ses résultats sont disponibles jusqu'à ce que vous les supprimiez avec la méthode `DELETE /v1/recognitions/{id}` ou jusqu'à ce que la durée de vie du travail soit écoulée, selon ce qui survient en premier. 

### Exemple
{: #statusExample}

L'exemple suivant demande le statut des derniers travaux en cours associés aux données d'identification de service du demandeur. L'utilisateur a trois travaux en attente, chacun avec des états différents. Le premier travail a été créé avec une URL de rappel et un jeton utilisateur.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions"
```
{: pre}

```javascript
{
  "recognitions": [
    {
      "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
      "created": "2016-08-17T19:15:17.926Z",
      "updated": "2016-08-17T19:15:17.926Z",
      "status": "waiting",
      "user_token": "job25"
    },
    {
      "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
      "created": "2016-08-17T19:13:23.622Z",
      "updated": "2016-08-17T19:13:24.434Z",
      "status": "processing"
    },
    {
      "id": "398fcd80-330a-22ba-93ce-1a73f454dd98",
      "created": "2016-08-17T19:11:04.298Z",
      "updated": "2016-08-17T19:11:16.003Z",
      "status": "completed"
    }
  ]
}
```
{: codeblock}

## Suppression d'un travail
{: #delete-async}

Vous pouvez utiliser la méthode `DELETE /v1/recognitions/{id}` pour supprimer le travail spécifié avec le paramètre de chemin `id`. En général, vous supprimez un travail après avoir obtenu ses résultats du service. Dès que vous avez supprimé un travail, ses résultats ne sont plus disponibles. Vous ne pouvez pas supprimer un travail qui serait actuellement en cours de traitement par le service.

Par défaut, le service conserve les résultats de chaque travail, jusqu'à expiration de la durée de vie du travail. La durée par défaut est d'une semaine, mais vous pouvez utiliser le paramètre `results_ttl` de la méthode `POST /v1/recognitions` pour spécifier la durée de conservation des résultats par le service en nombre de minutes.

### Exemple
{: #deleteExample-async}

L'exemple suivant supprime le travail avec l'ID spécifié :

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}
