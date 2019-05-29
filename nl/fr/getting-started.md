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
{:go: .ph data-hd-programlang='go'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:download: .download}
{:apikey: data-credential-placeholder='apikey'}
{:url: data-credential-placeholder='url'}
{:hide-dashboard: .hide-dashboard}

# Tutoriel d'initiation
{: #gettingStarted}

Le service {{site.data.keyword.speechtotextfull}} transcrit les données audio sous forme de texte pour activer les fonctions de transcription vocale pour les applications. Ce tutoriel qui fait appel à la commande curl peut vous aider à expérimenter rapidement le service. Les exemples vous montrent comment appeler la méthode `POST /v1/recognize` du service pour demander une retranscription.
{: shortdesc}

Le tutoriel utilise des clés d'API {{site.data.keyword.cloud}} Identity and Access Management (IAM) pour l'authentification. Des instances de service plus anciennes peuvent continuer à utiliser le nom d'utilisateur `{username}` et le mot de passe `{password}` de leurs données d'identification de service Cloud Foundry pour l'authentification. Authentifiez-vous en utilisant l'approche qui convient à votre instance de service. Pour plus d'informations sur l'authentification IAM utilisée par le service, voir la [mise à jour de service du 30 octobre 2018](/docs/services/speech-to-text/release-notes.html#October2018b) dans les notes sur l'édition.
{: important}

## Avant de commencer
{: #before-you-begin}

- {: hide-dashboard}  Créez une instance du service :
    1.  {: hide-dashboard} Accédez à la page [{{site.data.keyword.speechtotextshort}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/catalog/services/speech-to-text){: new_window} du catalogue {{site.data.keyword.cloud_notm}}.
    1.  {: hide-dashboard} Inscrivez-vous pour un compte {{site.data.keyword.cloud_notm}} gratuit ou connectez-vous.
    1.  {: hide-dashboard} Cliquez sur **Create**.
-   Copiez les données d'identification pour vous authentifier à votre instance de service :
    1.  {: hide-dashboard} Dans le [tableau de bord {{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/dashboard/apps){: new_window}, cliquez sur votre instance de service {{site.data.keyword.speechtotextshort}} pour accéder à la page du tableau de bord du service {{site.data.keyword.speechtotextshort}}.
    1.  Sur la page **Manage**, cliquez sur **Show** pour afficher vos données d'identification.
    1.  Copiez les valeurs correspondant à `API Key` et `URL`.
-   Vérifiez que vous disposez de la commande `curl`.
    -   Les exemples utilisent la commande `curl` pour appeler les méthodes de l'interface HTTP. Installez la version correspondant à votre système d'exploitation à partir de [curl.haxx.se ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://curl.haxx.se/){: new_window}. Installez la version compatible avec le protocole SSL (Secure Sockets Layer). Veillez à inclure le fichier binaire installé indiqué dans votre variable d'environnement `PATH`.

Lorsque vous entrez une commande, remplacez `{apikey}` et `{url}` par vos clé d'API et URL effectives. Omettez les accolades, qui indiquent l'emplacement d'une variable dans la commande. Une valeur réelle ressemble à l'exemple suivant :
{: hide-dashboard}

```bash
curl -X POST -u "apikey:L_HALhLVIksh1b73l97LSs6R_3gLo4xkujAaxm7i-b9x"
. . .
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{:pre}
{: hide-dashboard}

## Etape 1 : Transcription audio sans options
{: #transcribe}

Appelez la méthode `POST /v1/recognize` pour demander une retranscription de base d'un fichier audio FLAC sans aucun paramètre de demande supplémentaire.

1.  Téléchargez l'exemple de fichier audio <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a>.
1.  Exécutez la commande suivante pour appeler la méthode `/v1/recognize` du service pour effectuer une transcription de base sans paramètre. L'exemple utilise l'en-tête `Content-Type` pour indiquer le type d'audio, `audio/flac`. L'exemple utilise le modèle de langue par défaut, `en-US_BroadbandModel`, pour la transcription.
    -   {: hide-dashboard} Remplacez `{apikey}` et `{url}` par vos clé d'API et URL.
    -   Remplacez `{path_to_file}` en indiquant l'emplacement du fichier `audio-file.flac`.

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize"{: url}
    ```
    {: pre}

    Le service renvoie les résultats de transcription suivants :

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.87
              "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through colorado on sunday "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

## Etape 2 : Transcription audio avec options
{: #transcribeOptions}

Appelez la méthode `POST /v1/recognize` pour transcrire le même fichier audio FLAC, mais cette fois en spécifiant deux paramètres de transcription.

1.  Si nécessaire, téléchargez l'exemple de fichier audio <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe" title="Icône de lien externe"></a>.
1.  Exécutez la commande suivante pour appeler la méthode `/v1/recognize` du service avec deux paramètres supplémentaires. Définissez le paramètre `timestamps` avec la valeur `true` pour indiquer le début et la fin de chaque mot dans le flux audio. Définissez le paramètre `max_alternatives` avec la valeur `3` pour recevoir les trois alternatives les plus plausibles pour la transcription. L'exemple utilise l'en-tête `Content-Type` pour indiquer le type d'audio, `audio/flac` et la demande utilise le modèle de langue par défaut, `en-US_BroadbandModel`.
    -   {: hide-dashboard} Remplacez `{apikey}` et `{url}` par vos clé d'API et URL.
    -   Remplacez `{path_to_file}` en indiquant l'emplacement du fichier `audio-file.flac`.

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize?timestamps=true&max_alternatives=3"{: url}
    ```
    {: pre}

    Le service renvoie les résultats suivants, qui comprennent des horodatages (timestamps) et trois transcriptions alternatives :

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "timestamps": [
                ["several":, 1.0, 1.51],
                ["tornadoes":, 1.51, 2.15],
                ["touch":, 2.15, 2.5],
                . . .
              ]
            },
            {
              "confidence": 0.87
              "transcript": "several tornadoes touch down as a line
of severe thunderstorms swept through colorado on sunday "
            },
            {
              "transcript": "several tornadoes touched down as a line
of severe thunderstorms swept through colorado on sunday "
            },
            {
              "transcript": "several tornadoes touch down is a line
of severe thunderstorms swept through colorado on sunday "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

## Etapes suivantes

-   Pour en savoir plus sur les interfaces et les logiciels SDK disponibles pour effectuer des demandes de reconnaissance vocale, consultez la rubrique [Présentation destinée aux développeurs](/docs/services/speech-to-text/developer-overview.html).
-   Consultez les demandes de reconnaissance vocale de base pour chacune des interfaces du service dans la rubrique [Effectuer une demande de reconnaissance](/docs/services/speech-to-text/basic-request.html).
-   Procurez-vous les informations détaillées sur toutes les méthodes des interfaces du service dans la rubrique [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
