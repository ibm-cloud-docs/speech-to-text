---

copyright:
  years: 2015, 2019
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

# L'interface WebSocket
{: #websockets}

L'interface WebSocket du service {{site.data.keyword.speechtotextshort}} constitue le moyen le plus naturel d'interaction d'un client avec le service. Pour utiliser l'interface WebSocket en vue de la reconnaissance vocale, vous utilisez d'abord la méthode `/v1/recognize` afin d'établir une connexion permanente avec le service. Vous envoyez ensuite le texte et les messages binaires via cette connexion pour initier et gérer les demandes de reconnaissance.
{: shortdesc}

Le cycle de demande et de réponse d'une reconnaissance est constitué des étapes suivantes :

1.  [Ouverture d'une connexion](#WSopen)
1.  [Initiation d'une demande de reconnaissance](#WSstart)
1.  [Envoi des données audio et réception des résultats de reconnaissance](#WSaudio)
1.  [Fin d'une demande de reconnaissance](#WSstop)
1.  [Envoi de demandes supplémentaires et modifications des paramètres de demande](#WSmore)
1.  [Conservation d'une connexion active](#WSkeep)
1.  [Fermeture d'une connexion](#WSclose)

Lorsque le client envoie des données au service, il *doit* transmettre tous les paramètres JSON sous forme de messages texte et toutes les données audio sous forme de messages binaires.

Les fragments d'exemples de code suivants sont écrits en JavaScript et s'appuient sur l'API WebSocket HTML5. Pour plus d'informations sur le protocole WebSocket, voir le document de l'Internet Engineering Task Force (IETF) [Request for Comment (RFC) 6455](http://tools.ietf.org/html/rfc6455){: external}.
{: note}

## Ouverture d'une connexion
{: #WSopen}

Le service {{site.data.keyword.speechtotextshort}} utilise le protocole WebSocket Secure (WSS) pour rendre la méthode `/v1/recognize` disponible sur le noeud final suivant :

```
wss://{host_name}/speech-to-text/api/v1/recognize
```
{: codeblock}

Où `{host_name}` correspond à l'emplacement où est hébergée votre application :

-   `stream.watsonplatform.net` pour Dallas (nom d'hôte utilisé dans les exemples suivants)
-   `stream-fra.watsonplatform.net` pour Francfort
-   `gateway-syd.watsonplatform.net` pour Sydney
-   `gateway-wdc.watsonplatform.net` pour Washington, DC
-   `gateway-tok.watsonplatform.net` pour Tokyo
-   `gateway-lon.watsonplatform.net` pour Londres

Un client WebSocket appelle cette méthode avec les paramètres de requête suivants pour établir une connexion authentifiée avec le service. Si vous avez recours à l'authentification IAM (Identity and Access Management), utilisez le paramètre de requête `access_token`. Si vous avez recours à des données d'identification de service Cloud Foundry, utilisez le paramètre de requête `watson-token`.

<table>
  <caption>Tableau 1. Paramètres de la méthode
    <code>/v1/recognize</code></caption>
  <tr>
    <th style="width:23%; text-align:left">Paramètre</th>
    <th style="width:12%; text-align:center">Type de données</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>access_token</code>
      <br/><em>Facultatif</em></td>
    <td style="text-align:center">Chaîne</td>
    <td style="text-align:left">
      <em>Si vous utilisez l'authentification IAM, </em> transmettez un jeton
      d'accès IAM valide pour établir une connexion authentifiée avec le service.
      Vous transmettez avec l'appel un jeton d'accès IAM au lieu d'une clé
      d'API. Vous devez établir la connexion avant le délai d'expiration du
      jeton d'accès. Pour plus d'informations sur l'obtention d'un jeton d'accès, voir
      [Authentification avec des jetons IAM](/docs/services/watson?topic=watson-iam).<br/><br/>
      Vous transmettez un jeton d'accès uniquement pour établir une connexion authentifiée.
      Une fois la connexion établie, vous pouvez la garder active indéfiniment.
      Vous restez authentifié tant que vous conservez la connexion ouverte.
      Vous n'avez pas besoin d'actualiser le jeton d'accès pour une connexion active
      qui dure au-delà du délai d'expiration du jeton. Une connexion peut rester
      active même après la suppression du jeton ou de sa clé d'API.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>watson-token</code>
      <br/><em>Facultatif</em></td>
    <td style="text-align:center">Chaîne</td>
    <td style="text-align:left">
      <em>Si vous utilisez des données d'identification de service Cloud Foundry,</em> transmettez
      un jeton d'authentification {{site.data.keyword.watson}} pour établir une
      connexion authentifiée avec le service. Vous transmettez avec l'appel
      un jeton {{site.data.keyword.watson}} au lieu de données
      d'identification. Les jetons {{site.data.keyword.watson}} sont
      basés sur les données d'identification de service Cloud Foundry, qui utilisent un nom d'utilisateur (`username`)
      et un mot de passe (`password`) pour l'authentification de base HTTP. Pour savoir comment
      obtenir un jeton {{site.data.keyword.watson}}, voir
      [Jetons {{site.data.keyword.watson}}](/docs/services/watson?topic=watson-gs-tokens-watson-tokens).<br/><br/>
      Vous transmettez un jeton {{site.data.keyword.watson}} uniquement pour établir
      une connexion authentifiée. Une fois la connexion établie, vous pouvez la
      garder active indéfiniment. Vous restez authentifié tant que vous conservez
      la connexion ouverte.
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>model</code>
      <br/><em>Facultatif</em></td>
    <td style="text-align:center">Chaîne</td>
    <td style="text-align:left">
      Indique le modèle de langue à utiliser pour la transcription.
      Si vous n'indiquez pas de modèle, le service utilise le
      modèle <code>en-US_BroadbandModel</code> par défaut. Pour plus
      d'informations, voir
      [Langues et modèles](/docs/services/speech-to-text?topic=speech-to-text-models).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>language_customization_id</code>
      <br/><em>Facultatif</em></td>
    <td style="text-align:center">Chaîne</td>
    <td style="text-align:left">
      Spécifie l'identificateur unique global (GUID) d'un modèle de
      langue personnalisé à utiliser pour toutes les demandes envoyées
      via la connexion. Le modèle de base du modèle de langue personnalisé
      doit correspondre à la valeur du paramètre <code>model</code>. Si vous
      incluez un ID de personnalisation, vous devez faire la demande avec les données d'identification
      de l'instance du service propriétaire du modèle personnalisé. Par
      défaut, aucun modèle de langue personnalisé n'est utilisé. Pour plus d'informations, voir
      [L'interface de personnalisation](/docs/services/speech-to-text?topic=speech-to-text-customization).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>acoustic_customization_id</code>
      <br/><em>Facultatif</em></td>
    <td style="text-align:center">Chaîne</td>
    <td style="text-align:left">
      Spécifie l'identificateur unique global (GUID) d'un modèle
      acoustique personnalisé à utiliser pour toutes les demandes envoyées
      via la connexion. Le modèle de base du modèle acoustique personnalisé
      doit correspondre à la valeur du paramètre <code>model</code>. Si vous
      incluez un ID de personnalisation, vous devez faire la demande avec les données d'identification
      de l'instance du service propriétaire du modèle personnalisé. Par
      défaut, aucun modèle acoustique personnalisé n'est utilisé. Pour plus d'informations, voir
      [L'interface de personnalisation](/docs/services/speech-to-text?topic=speech-to-text-customization).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>base_model_version</code>
      <br/><em>Facultatif</em></td>
    <td style="text-align:center">Chaîne</td>
    <td style="text-align:left">
      Indique la version du modèle de base à utiliser pour toutes les
      demandes envoyées via la connexion. Ce paramètre est principalement
      conçu pour être utilisé avec des modèles personnalisés mis à niveau pour un nouveau
      modèle de base. La valeur par défaut dépend de l'utilisation de ce paramètre avec ou sans
      modèle personnalisé. Pour plus d'informations, voir
      [Version du modèle de base](/docs/services/speech-to-text?topic=speech-to-text-input#version).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-learning-opt-out</code>
      <br/><em>Facultatif</em></td>
    <td style="text-align:center">Booléen</td>
    <td style="text-align:left">
      Indique si le service consigne les demandes et les résultats envoyés
      via la connexion. Pour empêcher IBM d'accéder à vos données en vue
      d'apporter des améliorations générales au service, spécifiez la valeur <code>true</code> pour ce paramètre. Pour
      plus d'informations, voir
      [Journalisation des demandes](/docs/services/speech-to-text?topic=speech-to-text-input#logging).
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-metadata</code>
      <br/><em>Facultatif</em></td>
    <td style="text-align:center">Chaîne</td>
    <td style="text-align:left">
      Associe un ID client à toutes les données transmises via la
      connexion. Ce paramètre accepte l'argument
      <code>customer_id={id}</code>, où <code>id</code> représente une
      chaîne aléatoire ou générique à associer aux données. Vous devez
      coder l'argument de ce paramètre en codage URL, par exemple,
      `customer_id%3dmy_customer_ID`. Par défaut, aucun ID client n'est associé
      aux données. Pour plus d'informations, voir
      [Sécurité des informations](/docs/services/speech-to-text?topic=speech-to-text-information-security).
    </td>
  </tr>
</table>

Le fragment de code JavaScript suivant ouvre une connexion avec le service. L'appel à la méthode `/v1/recognize` transmet les paramètres de requête `access_token` et `model`, le dernier paramètre demandant au service d'utiliser le modèle à large bande espagnol. Après avoir établi la connexion, le client définit les programmes d'écoute d'événement (`onOpen`, `onClose`, etc.) pour répondre aux événements du service. Le client peut utiliser la connexion pour plusieurs demandes de reconnaissance.

```javascript
var IAM_access_token = '{access_token}';
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token
  + '&model=es-ES_BroadbandModel';
var websocket = new WebSocket(wsURI);
websocket.onopen = function(evt) { onOpen(evt) };
websocket.onclose = function(evt) { onClose(evt) };
websocket.onmessage = function(evt) { onMessage(evt) };
websocket.onerror = function(evt) { onError(evt) };
```
{: codeblock}

Le client peut ouvrir plusieurs connexions WebSocket simultanées au service. Le nombre de connexions simultanées est limité uniquement à la capacité du service, ce qui pose généralement des problèmes aux utilisateurs.

## Initiation d'une demande de reconnaissance
{: #WSstart}

Pour initier une demande de reconnaissance, le client envoie un message texte JSON au service via une connexion établie. Le client doit envoyer ce message avant d'envoyer des données audio à transcrire. Le message doit inclure le paramètre `action` mais peut généralement omettre le paramètre `content-type`.

<table>
  <caption>Tableau 2. Paramètres du message texte JSON</caption>
  <tr>
    <th style="width:23%; text-align:left">Paramètre</th>
    <th style="width:12%; text-align:center">Type de données</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>action</code><br/><em>Obligatoire</em></td>
    <td style="text-align:center">Chaîne</td>
    <td style="text-align:left">
      Spécifie l'action à effectuer :
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          <code>start</code> démarre une demande de reconnaissance ou spécifie
          de nouveaux paramètres pour des demandes ultérieures. Pour plus d'informations, voir
          [Envoi de demandes supplémentaires et modification des paramètres de demande](#WSmore).
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <code>stop</code> signale que toutes les données audio d'une demande
          ont été envoyées. Pour plus d'informations, voir
          [Fin d'une demande de reconnaissance](#WSstop).
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>content-type</code><br/><em>Facultatif</em></td>
    <td style="text-align:center">Chaîne</td>
    <td style="text-align:left">
      Identifie le format (type MIME) des données audio pour la demande.
      Ce paramètre est obligatoire pour les formats `audio/alaw`, `audio/basic`,
      `audio/l16` et `audio/mulaw`. Pour plus d'informations, voir
      [Formats audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).
    </td>
  </tr>
</table>

Le message peut également contenir des paramètres facultatifs pour indiquer d'autres aspects de traitement de la demande et les informations qui doivent être renvoyées. Pour plus d'informations sur les fonctions d'entrée et de sortie, voir [Récapitulatif des paramètres](/docs/services/speech-to-text?topic=speech-to-text-summary). Vous pouvez spécifier un modèle de langue, un modèle de langue personnalisé et un modèle acoustique personnalisé uniquement en tant que paramètres de requête de l'URL WebSocket.

Le fragment de code JavaScript suivant envoie des paramètres d'initialisation pour la demande de reconnaissance via la connexion WebSocket. Les appels sont inclus dans la fonction `onOpen` du client pour garantir qu'ils sont envoyés uniquement une fois que la connexion est établie.

```javascript
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/l16;rate=22050'
  };
  websocket.send(JSON.stringify(message));
}
```
{: codeblock}

Si le service reçoit bien la demande, il renvoie le message texte suivant pour indiquer qu'il est à l'écoute (`listening`) :

```javascript
{'state': 'listening'}
```
{: codeblock}

L'état `listening` indique que l'instance de service est configurée (votre message JSON `start` était valide) et qu'elle est prête à traiter un nouvel énoncé de demande de reconnaissance. Dès qu'il commence à écouter, le service traite toutes les données audio envoyées avant le message `listening`.

Si le client indique un paramètre de requête ou une zone JSON non valide pour la demande de reconnaissance, la réponse JSON du service comprend une zone `warnings`. Cette zone décrit chaque argument non valide. La demande aboutit malgré les avertissements contenus dans cette zone.

## Envoi des données audio et réception des résultats de reconnaissance
{: #WSaudio}

Après avoir envoyé le message `start` initial, le client peut commencer à envoyer des données audio au service. Il n'a pas besoin d'attendre que le service réponde au message `start` par le message `listening`. Le service renvoie les résultats de la transcription de manière asynchrone dans le même format que les résultats qu'il renvoie pour les interfaces HTTP.

Le client doit envoyer les données audio sous forme de données binaires. Il peut envoyer jusqu'à 100 Mo de données audio maximum avec un seul énoncé (par demande `send`). Il doit envoyer au moins 100 octets de données audio pour toute demande. Le client peut envoyer plusieurs énoncés via une seule connexion WebSocket. Pour plus d'informations sur l'utilisation de la compression pour maximiser la quantité de données audio que vous pouvez transmettre au service avec une demande, voir [Formats audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).

L'interface WebSocket impose une taille de trame maximale de 4 Mo. Le client peut définir cette taille maximale avec une valeur inférieure à 4 Mo. S'il n'est pas pratique de définir la taille de trame, le client peut définir la taille maximale des messages à moins de 4 Mo et envoyer les données audio sous forme de séquence de messages. Pour plus d'informations sur les cadres WebSocket, voir [IETF RFC 6455](http://tools.ietf.org/html/rfc6455){: external}.

Le fragment de code JavaScript suivant envoie des données au service sous forme de message binaire (blob) :

```javascript
websocket.send(blob);
```
{: codeblock}

Le fragment suivant reçoit des hypothèses de reconnaissance que le service renvoie de manière asynchrone. Les résultats sont gérés par la fonction `onMessage` du client.

```javascript
function onMessage(evt) {
  console.log(evt.data);
}
```
{: codeblock}

## Fin d'une demande de reconnaissance
{: #WSstop}

Lorsqu'il a terminé l'envoi des données audio d'une demande au service, le client *doit* signaler la fin de la transmission audio binaire au service de l'une des manières suivantes :

-   En envoyant un message texte JSON avec le paramètre `action` définie avec la valeur `stop` :

    ```javascript
    {action: 'stop'}
    ```
    {: codeblock}

-   En envoyant un message binaire vide, dans lequel l'objet blob spécifié est vide :

    ```javascript
    websocket.send(blob)
    ```
    {: codeblock}

Le service n'envoie pas les résultats finaux tant qu'il n'a pas reçu la confirmation d'achèvement de la transmission audio. Si vous oubliez de signaler que la transmission est terminée, la connexion peut expirer avant que le service envoie les résultats finaux.

Pour recevoir les résultats finaux entre plusieurs demandes de reconnaissance, le client doit signaler la fin de transmission de la première demande avant d'envoyer la demande suivante. Après avoir renvoyé les résultats finaux de la première demande, le service renvoie un autre message `{"state":"listening"}` au client. Ce message indique que le service est prêt à recevoir une autre demande.

## Envoi de demandes supplémentaires et modification des paramètres de demande
{: #WSmore}

Tant que la connexion WebSocket reste active, le client peut continuer à l'utiliser pour envoyer d'autres demandes de reconnaissance avec de nouvelles données audio. Par défaut, le service continue à utiliser les paramètres envoyés avec le message `start` précédent pour toutes les demandes qui suivent envoyées via la même connexion.

Pour modifier les paramètres des demandes suivantes, le client peut envoyer une autre message `start` avec les nouveaux paramètres après avoir reçu le message `{"state":"listening"}` du service. Le client peut modifier tous les paramètres sauf ceux qui ont été spécifiés à l'ouverture de la connexion (`model`, `language_customization_id`, etc.).

L'exemple suivant envoie un message `start` avec des nouveaux paramètres pour les demandes de reconnaissance suivantes envoyées via la connexion. Le message spécifie le même paramètre `content-type` que dans l'exemple précédent, mais demande au service de renvoyer des mesures de confiance (word_confidence) et des horodatages (timestamps) pour les mots de la transcription.

```javascript
var message = {
  action: 'start',
  content-type: 'audio/l16;rate=22050',
  word_confidence: true,
  timestamps: true
};
websocket.send(JSON.stringify(message));
```
{: codeblock}

## Conservation d'une connexion active
{: #WSkeep}

Le service met fin à la session et ferme la connexion en cas d'expiration du délai d'attente d'inactivité ou de session :

-   Une *expiration de délai d'attente d'inactivité* se produit si les données audio sont envoyées par le client mais que le service ne détecte pas de paroles. Ce délai est fixé à 30 secondes par défaut. Vous pouvez utiliser le paramètre `inactivity_timeout` pour spécifier une autre valeur, y compris `-1` pour définir une valeur illimitée à ce délai. Pour plus d'informations, voir [Délai d'attente d'inactivité](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-inactivity).
-   Une *expiration du délai d'attente de session* se produit si le service ne reçoit aucune donnée du client ou n'envoie aucun résultat intermédiaire pendant une durée de 30 secondes. Vous ne pouvez pas modifier la durée de ce délai d'attente mais vous pouvez étendre la session en envoyant au service des données audio, constituées uniquement de silence, avant l'expiration du délai. Vous devez également définir le paramètre `inactivity_timeout` avec la valeur `-1`. Vous êtes facturé pour toutes les données que vous envoyez au service, notamment le silence que vous envoyez pour étendre une session. Pour plus d'informations, voir [Délai d'attente de session](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-session).

Les clients et les serveurs WebSocket peuvent également échanger des *cadres de type ping-pong* pour éviter les délais de lecture en échangeant régulièrement de petites quantités de données. De nombreuses piles WebSocket échangent des cadres de type ping-pong, d'autres non. Pour déterminer si votre implémentation utilise des cadres de type ping-pong, consultez la liste de ses fonctionnalités. Vous ne pouvez pas déterminer ou gérer des cadres de type ping-pong à l'aide d'un programme.

Si votre pile WebSocket n'implémente pas de cadres de type ping-pong et que vous envoyez des fichiers audio longs, votre connexion peut connaître un délai de lecture. Pour éviter ces problèmes de délai, diffusez en continu du contenu audio sur le service ou demandez des résultats intermédiaires au service. Quelle que soit l'approche choisie, vous pouvez vous assurer que le manque de cadres de type ping-pong ne provoque pas la fermeture de votre connexion.

Pour plus d'informations sur les cadres de type ping-pong, voir la [Section 5.5.2 Ping](http://tools.ietf.org/html/rfc6455#section-5.5.2){: external} et la [Section 5.5.3 Pong](http://tools.ietf.org/html/rfc6455#section-5.5.3){: external} de l'IETF RFC 6455.

## Fermeture d'une connexion
{: #WSclose}

Lorsque le client a fini d'interagir avec le service, il peut fermer la connexion WebSocket. Lorsque la connexion est fermée, le client ne peut plus l'utiliser pour envoyer des demandes ou recevoir des résultats. Ne fermez la connexion qu'après avoir reçu tous les résultats d'une demande. La connexion finit par expirer et se fermer si vous ne la fermez pas de manière explicite.

Le fragment de code JavaScript suivant ferme une connexion ouverte :

```javascript
websocket.close();
```
{: codeblock}

## Code de retour WebSocket
{: #WSreturn}

Le service peut envoyer les codes de retour suivants au client via la connexion WebSocket :

-   `1000` indique la fermeture normale de la connexion, ce qui signifie que le but recherché en établissant cette connexion a été atteint.
-   `1002` indique que le service ferme la connexion en raison d'une erreur de protocole.
-   `1006` indique que la connexion ne s'est pas fermée normalement.
-   `1009` indique que la taille de trame dépasse la limite de 4 Mo.
-   `1011` indique que le service met fin à la connexion car il a rencontré une condition inattendue qui l'empêche de finaliser la demande.

Si le socket ferme avec une erreur, le client reçoit un message d'information sous la forme `{"error":"{message}"}` avant la fermeture du socket. Pour plus d'informations sur les codes retour WebSocket, voir le document [IETF RFC 6455](http://tools.ietf.org/html/rfc6455){: external}.

## Exemple de session WebSocket
{: #WSexample}

L'exemple suivant illustre une session WebSocket entre un client et le service {{site.data.keyword.speechtotextshort}}. La session est constituée de trois échanges distincts pour la rendre plus facile à suivre. Mais les trois échanges ne font pas tous partie d'une session unique avec le service. L'exemple se focalise sur l'échange des messages ; il ne présente pas l'ouverture et la fermeture de la connexion.

### Exemple du premier échange
{: #firstExample}

Dans le premier échange, le client envoie des données audio qui contiennent la chaîne `Name the Mayflower`. Le client envoie un message binaire avec un seul bloc de données audio PCM (`audio/l16`), pour lequel il indique la fréquence d'échantillonnage requise. Le client n'attend pas la réponse `{"state":"listening"}` du service pour envoyer les données audio et signaler la fin de la demande. L'envoi des données réduit immédiatement les temps d'attente car les données audio sont disponibles pour le service dès qu'il est prêt à traiter une demande de reconnaissance.

-   Le *client* envoie :

    ```javascript
    {
      "action": "start",
      "content-type": "audio/l16;rate=22050"
    }
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   Le *service* répond :

    ```javascript
    {"state": "listening"}
    {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### Exemple du deuxième échange
{: #secondExample}

Dans le deuxième échange, le client envoie des données audio qui contiennent la chaîne `Second audio transcript`. Le client envoie les données audio sous la forme d'un message binaire unique et utilise les mêmes paramètres qu'il a spécifiés dans la première demande.

-   Le *client* envoie :

    ```javascript
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   Le *service* répond :

    ```javascript
    {"results": [{"alternatives": [{"transcript": "second audio transcript "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}

### Exemple du troisième échange
{: #thirdExample}

Dans le troisième échange, le client envoie à nouveau les données audio qui contiennent la chaîne `Name the Mayflower`. Le client envoie encore un message binaire avec un seul bloc de données audio PCM. Mais cette fois, le client demande des résultats intermédiaires avec la transcription.

-   Le *client* envoie :

    ```javascript
    {
      "action": "start",
      "content-type": "audio/l16;rate=22050",
      "interim_results": true
    }
    <binary audio data>
    {
      "action": "stop"
    }
    ```
    {: codeblock}

-   Le *service* répond :

    ```javascript
    {"state":"listening"}
    {"results": [{"alternatives": [{"transcript": "name "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name may "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name may flour "}],
                  "final": false}], "result_index": 0}
    {"results": [{"alternatives": [{"transcript": "name the mayflower "}],
                  "final": true}], "result_index": 0}
    {"state":"listening"}
    ```
    {: codeblock}
