---

copyright:
  years: 2019
lastupdated: "2019-06-19"

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

# Gestion des ressources audio
{: #manageAudio}

L'interface de personnalisation comprend la méthode `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`, qui est utilisée pour ajouter une ressource audio à un modèle acoustique personnalisé. Pour plus d'informations, voir [Ajout de données audio au modèle acoustique personnalisé](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio). Cette interface comprend également les méthodes suivantes permettant d'afficher la liste et de supprimer des ressources audio pour un modèle acoustique personnalisé.
{: shortdesc}

## Affichage de la liste des ressources audio d'un modèle acoustique personnalisé
{: #listAudio}

L'interface de personnalisation fournit deux méthodes pour répertorier les informations sur les ressources audio d'un modèle acoustique personnalisé :

-   La méthode `GET /v1/acoustic_customizations/{customization_id}/audio` répertorie les informations sur toutes les ressources audio qui ont été ajoutées à un modèle personnalisé.
-   La méthode `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` répertorie les informations sur une ressource audio spécifique d'un modèle personnalisé.

Ces deux méthodes renvoient le nom (`name`) de la ressource audio ainsi que les informations complémentaires suivantes :

-   `duration` correspond à la durée audio exprimée en secondes. Pour un fichier archive, le chiffre indiqué représente la durée cumulée de tous les fichiers audio inclus dans l'archive.
-   `details` identifie le type de ressource : `audio` ou `archive`. (Le type a la valeur `undetermined` si le service ne peut pas valider la ressource, vraisemblablement parce que l'utilisateur a transmis par erreur un fichier qui ne contient pas de données audio.) Pour un fichier audio, les détails comprennent les paramètres `codec` et `frequency` de l'audio. Pour un fichier archive, ils comprennent le type de `compression`.

En outre, répertorier les ressources audio d'un modèle renvoie la durée totale audio en minutes (`total_minutes_of_audio`) qui est égale à la somme de toutes les ressources audio valides pour le modèle. Vous pouvez utiliser la valeur de ce paramètre pour déterminer si les données audio sont suffisantes ou excessives pour commencer l'entraînement.

Les méthodes affichent également le statut des données audio. Le statut est important pour vérifier l'analyse des fichiers audio effectuée par le service en réponse à une demande pour les ajouter à un modèle personnalisé :

-   `ok` indique que le service a réussi à analyser les données audio. Les données peuvent être utilisées pour entraîner le modèle personnalisé.
-   `being_processed` indique que le service est encore en train d'analyser les données audio. Le service ne peut pas accepter de demandes d'ajout de nouvelles données audio ou d'entraînement du modèle personnalisé tant que l'analyse n'est pas terminée.
-   `invalid` indique que les données audio ne sont pas valides pour entraîner le modèle (vraisemblablement en raison d'une fréquence d'échantillonnage ou d'un format incorrect ou parce que ces données sont endommagées).

### Exemple de demande : affichage de la liste de toutes les ressources audio
{: #listExample-audio}

L'exemple suivant répertorie toutes les ressources audio du modèle acoustique personnalisé avec l'ID de personnalisation spécifié. Le modèle acoustique comporte trois ressources audio. Le service a analysé les ressources `audio1` et `audio2`, il est toujours en train d'analyser la ressource `audio3`.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio"
```
{: pre}

```javascript
{
  "total_minutes_of_audio": 11.45,
  "audio": [
    {
      "duration": 131,
      "name": "audio1",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 22050
      }
      "status": "ok"
    },
    {
      "duration": 556,
      "name": "audio2",
      "details": {
        "type": "archive",
        "compression": "zip"
      },
      "status": "ok"
    },
    {
      "duration": 0,
      "name": "audio3",
      "details": {},
      "status": "being_processed"
    }
  ]
}
```
{: codeblock}

### Exemple de demande : obtention d'informations à partir d'une ressource de type audio
{: #getExampleAudio}

L'exemple suivant renvoie des informations sur la ressource de type audio nommée `audio1`. Cette ressource a une durée de 131 secondes. Elle est codée avec le codec `pcm_s16le`. Elle a été ajoutée au modèle avec succès.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
```
{: pre}

```javascript
{
  "duration": 131,
  "name": "audio1",
  "details": {
    "codec": "pcm_s16le",
    "type": "audio",
    "frequency": 22050
  }
  "status": "ok"
}
```
{: codeblock}

### Exemple de demande : obtention d'informations pour une ressource de type archive
{: #getExampleArchive}

L'exemple suivant renvoie des informations sur la ressource de type archive nommée `audio2`. Il s'agit d'un fichier **.zip** contenant plus de 9 minutes d'audio. Cette ressource a été également ajoutée au modèle avec succès. Comme illustré dans l'exemple, la demande d'informations concernant une ressource de type archive fournit également des informations sur les fichiers qu'elle contient.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
```
{: pre}

```javascript
{
  "container": {
    "duration": 556,
    "name": "audio2",
    "details": {
      "type": "archive",
      "compression": "zip"
    },
    "status": "ok"
  },
  "audio": [
    {
      "duration": 121,
      "name": "audio-file1.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    {
      "duration": 133,
      "name": "audio-file2.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    . . .
  ]
}
```
{: codeblock}

## Suppression d'une ressource audio d'un modèle acoustique personnalisé
{: #deleteAudio}

Utilisez la méthode `DELETE /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` pour retirer une ressource audio existante d'un modèle acoustique personnalisé. Lorsque vous effectuez cette opération, le service retire toute l'archive de fichiers. Le service ne permet pas la suppression de fichiers individuels d'une ressource de type archive.

Le retrait d'une ressource audio n'affecte pas le modèle personnalisé tant que vous n'avez pas entraîné le modèle sur les données mises à jour à l'aide de la méthode `POST /v1/acoustic_customizations/{customization_id}/train`. Si vous avez entraîné le modèle sur la ressource et jusqu'à ce que vous l'entraîniez à nouveau, les données audio existantes sont toujours utilisées pour la reconnaissance vocale.

### Exemple de demande
{: #deleteExample-audio}

La méthode suivante supprime la ressource audio nommée `audio3` du modèle personnalisé avec l'ID de personnalisation spécifié :

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio3"
```
{: pre}
