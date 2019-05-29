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

# Effectuer une demande de reconnaissance
{: #basic-request}

Pour effectuer une demande de reconnaissance vocale avec le service {{site.data.keyword.speechtotextfull}}, il vous suffit de fournir les données audio à transcrire. Le service offre les mêmes fonctions de transcription de base avec chacune de ses interfaces : l'interface WebSocket, l'interface HTTP synchrone et l'interface HTTP asynchrone.
{: shortdesc}

Les sections suivantes présentent des demandes de transcription de base, sans paramètres d'entrée ou de sortie, pour chacune des interfaces du service :

-   Les exemples soumettent un fichier FLAC court nommé <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a>.
-   Les exemples utilisent le modèle de langue par défaut, `en-US_BroadbandModel`.

La rubrique [Description des résultats de reconnaissance](/docs/services/speech-to-text/basic-response.html) présente la réponse du service correspondant à ces exemples.

## Envoi de données audio avec une demande
{: #basic-request-audio}

Les données audio que vous transmettez au service doivent être dans l'un des formats pris en charge par le service. Pour la plupart des données audio, le service peut détecter automatiquement le format. Pour d'autres, vous devez spécifier le format avec le paramètre `Content-Type` ou un paramètre équivalent. Pour plus d'informations, voir [Formats audio](/docs/services/speech-to-text/audio-formats.html). (Pour que ce soit plus clair, les exemples suivants indiquent le format audio avec toutes les demandes.)

Avec les interfaces WebSocket et HTTP synchrone, vous pouvez transmettre jusqu'à 100 Mo de données audio maximum avec une seule demande. Avec l'interface HTTP asynchrone, vous pouvez transmettre jusqu'à 1 Go de données audio maximum. Vous devez envoyer au moins 100 octets de données audio avec une demande.

Si vous effectuez la reconnaissance de grandes quantités de données audio, vous pouvez diviser manuellement ces données en blocs de plus petite taille. Mais en général, il est plus efficace et pratique de convertir les données audio dans un format compressé avec perte. La compression peut maximiser la quantité de données que vous pouvez envoyer avec une seule demande. S'il s'agit notamment de données audio au format WAV ou FLAC, la conversion au format avec perte peut constituer une différence appréciable.

-   Pour plus d'informations sur les formats utilisant la compression, voir [Formats audio pris en charge](/docs/services/speech-to-text/audio-formats.html#formats).
-   Pour plus d'informations sur les effets de la compression et sur la conversion de vos données audio en format avec compression, voir [Limites et compression de données](/docs/services/speech-to-text/audio-formats.html#limits) et [Conversion audio](/docs/services/speech-to-text/audio-formats.html#conversion).

## Utilisation de l'interface WebSocket
{: #basic-request-websocket}

[L'interface WebSocket](/docs/services/speech-to-text/websockets.html) offre une implémentation efficace, à faible temps d'attente et à haut débit via une connexion en duplex intégral. Toutes les demandes et les réponses sont envoyées via la même connexion Websocket. En raison des avantages qu'il procure, le protocole WebSockets constitue le mécanisme privilégié pour la reconnaissance vocale. Pour plus d'informations, voir [Avantages de l'interface WebSocket](/docs/services/speech-to-text/developer-overview.html#advantages).

Pour utiliser l'interface WebSocket, vous devez d'abord utiliser la méthode `/v1/recognize` pour établir une connexion avec le service. Vous spécifiez les paramètres, tels que le modèle de langue et tout modèle personnalisé à utiliser pour les demandes envoyées via la connexion. Vous enregistrez ensuite les programmes d'écoute pour traiter les réponses renvoyées par le service. Pour effectuer une demande, vous envoyez un message texte JSON incluant le format audio et des paramètres supplémentaires, le cas échéant. Vous transmettez les données audio sous forme de message binaire (blob), puis vous envoyez un message texte pour signaler la fin des données audio.

L'exemple suivant fournit un code JavaScript qui établit une connexion et envoie les messages texte et les messages binaires pour une demande de reconnaissance. L'exemple ne comprend pas le code utilisé pour installer les gestionnaires d'événements.

```javascript
var token = {authentication-token};
var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
  + '?access_token=' + IAM_access_token;
var websocket = new WebSocket(wsURI);

websocket.send(JSON.stringify({
  action: 'start',
  content-type: 'audio/flac'
}));
websocket.send(blob);
websocket.send(JSON.stringify({action: 'stop'}));
```
{: codeblock}

## Utilisation de l'interface HTTP synchrone
{: #basic-request-sync}

[L'interface HTTP synchrone](/docs/services/speech-to-text/http.html) constitue le moyen le plus simple d'effectuer une demande de reconnaissance. Vous utilisez la méthode `POST /v1/recognize` pour effectuer une demande auprès du service. Vous transmettez les données audio et tous les paramètres avec cette demande unique.

L'exemple de commande `curl` suivant présente une demande de reconnaissance HTTP de base :

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## Utilisation de l'interface HTTP asynchrone
{: #basic-request-async}

[L'interface HTTP asynchrone](/docs/services/speech-to-text/async.html) fournit une interface non bloquante pour la transcription audio. Vous pouvez utiliser l'interface avec ou sans enregistrement préalable d'une URL de rappel avec le service. Avec une URL de rappel, le service envoie des notifications de rappel comprenant le statut du travail et les résultats de reconnaissance. L'interface utilise des signatures HMAC-SHA1 basées sur une valeur secrète spécifiée par l'utilisateur pour permettre l'authentification et assurer l'intégrité des données de ses notifications. Sans URL de rappel, vous devez interroger le service pour connaître le statut du travail et obtenir les résultats. Avec l'une ou l'autre de ces approches, vous utilisez la méthode `POST /v1/recognitions` pour effectuer une demande de reconnaissance.

L'exemple de commande `curl` suivant présente une demande de reconnaissance HTTP asynchrone simple. La demande ne contenant pas d'URL de rappel, vous devez interroger le service pour obtenir le statut du travail et la retranscription qui en résulte.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions"
```
{: pre}
