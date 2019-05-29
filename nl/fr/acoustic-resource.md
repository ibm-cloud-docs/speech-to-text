---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

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

# Utilisation des ressources audio
{: #audioResources}

Vous pouvez ajouter des fichiers audio individuels ou des fichiers archive contenant plusieurs fichiers audio à un modèle acoustique personnalisé. La méthode recommandée pour ajouter des ressources audio consiste à ajouter des fichiers archive. Créer et ajouter un fichier archive unique est beaucoup plus efficace que l'ajout de plusieurs fichiers audio individuels.
{: shortdesc}

## Ajout d'une ressource audio
{: #addAudioResource}

Vous utilisez la méthode `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` pour ajouter ces types de ressource audio à un modèle acoustique personnalisé. Vous transmettez la ressource audio en tant que corps de la demande avec les paramètres suivants :

-   Le paramètre de chemin `customization_id` pour spécifier l'ID de personnalisation du modèle.
-   Le paramètre de chemin `audio_name` pour spécifier le nom de la ressource audio.
    -   Utilisez un nom localisé qui correspond à la langue du modèle personnalisé et qui reflète le contenu de la ressource.
    -   Indiquez un nom ne dépassant pas 128 caractères.
    -   Ne mettez pas d'espace, de barre oblique '`/`' ou de barre oblique inversée `\` dans le nom.
    -   N'utilisez pas le nom d'une ressource audio qui a déjà été ajoutée au modèle personnalisé.

Lorsque vous mettez à jour les ressources audio d'un modèle avec la méthode de votre choix, vous devez entraîner le modèle pour que les modifications soient appliquées lors de la transcription. Pour plus d'informations, voir [Entraînement du modèle acoustique personnalisé](/docs/services/speech-to-text/acoustic-create.html#trainModel-acoustic).

## Ajout d'un fichier audio
{: #addAudioType}

Pour ajouter un fichier audio individuel à un modèle acoustique personnalisé, vous indiquez le format audio (type MIME) avec l'en-tête `Content-Type`. Vous pouvez ajouter des données audio dans un format pris en charge pour les demandes de reconnaissance. Incluez les paramètres `rate`, `channels` et `endianness` avec la spécification des formats qui les réclament. Pour plus d'informations sur les formats audio pris en charge, voir [Formats audio](/docs/services/speech-to-text/audio-formats.html).

La spécification `application/octet-stream` d'un format audio n'est pas prise en charge pour les ressources audio.
{: note}

L'exemple suivant extrait de la section [Ajout de données audio au modèle acoustique personnalisé](/docs/services/speech-to-text/acoustic-create.html#addAudio) ajoute un fichier `audio/wav` :

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @audio1.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
```
{: pre}

## Ajout d'un fichier archive
{: #addArchiveType}

La méthode privilégiée pour ajouter des données audio à un modèle acoustique personnalisé consiste à ajouter un fichier archive contenant plusieurs fichiers audio. Vous pouvez ajouter les types de fichier archive suivants en spécifiant le type d'archive dans l'en-tête de demande `Content-Type` :

-   Un fichier **.zip** en spécifiant `application/zip`
-   Un fichier **.tar.gz** en spécifiant `application/gzip`

Il vous faudra peut-être ajouter également l'en-tête `Contained-Content-Type` en fonction du format des fichiers que vous ajoutez :

-   Pour les fichiers audio de type `audio/alaw`, `audio/basic`, `audio/l16` ou `audio/mulaw`, vous devez utiliser l'en-tête `Contained-Content-Type` pour spécifier le format des fichiers audio. Incluez les paramètres `rate`, `channels` et `endianness`, le cas échéant. Dans ce cas, tous les fichiers audio contenus dans l'archive doivent avoir le même format audio.
-   Pour les fichiers audio de tous les autres types, vous pouvez omettre l'en-tête `Contained-Content-Type`. Dans ce cas, les fichiers audio contenus dans le fichier archive peuvent avoir n'importe quel format non répertorié dans la puce précédente. Ils n'ont pas besoin d'avoir le même format.

N'utilisez pas l'en-tête `Contained-Content-Type` lors de l'ajout d'une ressource de type audio.
{: note}

Le nom d'un fichier audio incorporé dans une ressource de type archive doit respecter les mêmes restrictions de dénomination que le paramètre `audio_name`. En outre, n'utilisez pas le nom d'un fichier audio qui a déjà été ajouté au modèle personnalisé dans une ressource de type archive.

L'exemple suivant extrait de la section [Ajout de données audio au modèle acoustique personnalisé](/docs/services/speech-to-text/acoustic-create.html#addAudio) ajoute un fichier au format `application/zip` qui contient des fichiers audio au format `audio/l16` avec une fréquence d'échantillonnage de 16 kHz :

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/zip"
--header "Contained-Content-Type: audio/l16;rate=16000"
--data-binary @audio2.zip
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
```
{: pre}

## Instructions pour ajouter des ressources audio
{: #audioGuidelines}

Une reconnaissance vocale plus précise avec l'utilisation d'un modèle acoustique personnalisé dépend d'un certain nombre de facteurs. Ces facteurs comprennent la quantité de données audio que contient le modèle acoustique personnalisé et le degré de similitude des données par rapport à la séquence audio en cours de transcription. L'amélioration dépend aussi de l'entraînement du modèle acoustique personnalisé avec un modèle de langue personnalisé correspondant.

Suivez ces instructions lorsque vous ajoutez des ressources audio à un modèle acoustique personnalisé :

-   Ajoutez au moins 10 minutes et jusqu'à 200 heures d'audio maximum à un modèle acoustique personnalisé. Les données audio doivent contenir uniquement de la parole et pas de silence.

    La qualité des données audio fait la différence lorsque vous déterminez la quantité à ajouter. Plus les données audio sont conformes aux caractéristiques audio à reconnaître, meilleure est la qualité du modèle personnalisé pour la reconnaissance vocale. Si la qualité audio est bonne, en rajouter permet d'obtenir une transcription plus précise. Ainsi, l'ajout de cinq à dix heures d'audio de bonne qualité peut faire la différence.
-   Ajoutez des données audio ne dépassant pas 100 Mo. Toutes les ressources de type audio et archive ont une taille limite de 100 Mo.

    Pour maximiser la quantité de données audio que vous ajoutez avec une seule ressource, envisagez d'utiliser un format audio permettant la compression des données. Pour plus d'informations, voir [Limites et compression de données](/docs/services/speech-to-text/audio-formats.html#limits).
-   Ajoutez du contenu audio reflétant les conditions du canal acoustique des données audio dont vous envisagez la transcription. Par exemple, si votre application doit composer avec le bruit de fond d'un véhicule en mouvement, utilisez le même type de données pour construire le modèle personnalisé.
-   Vérifiez que la fréquence d'échantillonnage d'un fichier audio correspond à celle du modèle de base pour le modèle acoustique personnalisé :
    -   Pour les modèles à large bande, la fréquence d'échantillonnage doit être d'au moins 16 kHz (16 000 échantillons par seconde).
    -   Pour les modèles à bande étroite, la fréquence d'échantillonnage doit être d'au moins 8 kHz (8000 échantillons par seconde).

    Si la fréquence d'échantillonnage des données audio est supérieure à la valeur minimale requise, le service procède au sous-échantillonnage des données à la fréquence appropriée. Si la fréquence d'échantillonnage est inférieure à la fréquence minimale requise, le service labellise le fichier audio avec la valeur `invalid`. Si un fichier audio contenu dans un fichier archive n'est pas valide, le service considère que toute l'archive est non valide.
-   Créez un modèle de langue personnalisé à utiliser avec votre modèle acoustique personnalisé dans les cas suivants :
    -   Si votre séquence audio dure moins d'une heure, créez un modèle de langue personnalisé basé sur les transcriptions de cette séquence pour obtenir les meilleurs résultats.
    -   Si vos données audio sont spécifiques à un domaine et contiennent des mots uniques qui ne figurent pas dans le vocabulaire de base du service, utilisez la personnalisation de modèle de langue personnalisé pour enrichir le vocabulaire de base du service. La personnalisation du modèle acoustique seule ne permet pas de générer ces mots lors de la transcription.

    Pour plus d'informations, voir [Utilisation d'un modèle acoustique personnalisé avec un modèle de langue personnalisé](/docs/services/speech-to-text/acoustic-both.html).
