---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-10"

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

# Fonctionnalités des mesures
{: #metrics}

Le service {{site.data.keyword.speechtotextfull}} peut renvoyer deux types de mesures facultatives pour une demande de reconnaissance vocale :

-   Les [mesures de traitement](#processing_metrics) fournissent des informations régulières sur le traitement du service de l'entrée audio. Utilisez ces mesures pour évaluer la progression du service de transcription du contenu audio. Les mesures de traitement sont disponibles avec les interfaces HTTP asynchrones et WebSocket.
-   Les [mesures audio](#audio_metrics) fournissent des informations sur les caractéristiques de signal de l'entrée audio. Utilisez ces mesures pour déterminer les caractéristique et la qualité du contenu audio. Les mesures audio sont disponibles avec toutes les interfaces de reconnaissance vocale.

Par défaut, le service ne renvoie aucune mesure pour une demande.


## Mesures de traitement
{: #processing_metrics}

Les mesures de traitement fournissent des informations de synchronisation détaillées sur l'analyse du service de l'entrée audio.  Le service renvoie les mesures à des intervalles spécifiés et avec des événements de transcription, par exemple des résultats temporaires et finaux. 

Les mesures comprennent des statistiques sur le volume de contenu audio reçu par le service, le volume de contenu audio transféré par le service au moteur de reconnaissance vocale et la durée pendant laquelle le service a traité le contenu audio. Si vous demandez des étiquettes de locuteur (speaker labels), les informations indiquent également le volume de contenu audio traité par le service pour déterminer des étiquettes de locuteur.

Les mesures de traitement vous permettent de mesurer la progression d'une demande de reconnaissance. Elles peuvent également vous permettre de distinguer l'absence de résultats due à 

-   l'absence de contenu audio ;
-   le manque de parole dans un contenu audio soumis ;
-   le blocage du moteur sur le serveur et le blocage du réseau entre le client et le serveur. Pour pouvoir faire la différence entre le blocage du moteur et le blocage du réseau, les résultats sont fournis de manière régulière plutôt qu'en fonction des événements.

Les métriques peuvent également vous aider à évaluer la gigue des réponses en examinant les heures d'arrivée périodiques. Elles sont générées à intervalles constants, de sorte que toute différence dans les heures d'arrivée est provoquée par la gigue.  

### Demande de mesures de traitement
{: #processing_metrics_request}

Pour demander des mesures de traitement, utilisez les paramètres facultatifs suivants :

-   `processing_metrics` est une valeur booléenne qui indique si le service renvoie des mesures de traitement. Indiquez `true` pour demander des mesures. Par défaut, le service ne renvoie aucune mesure.
-   `processing_metrics_interval` est une valeur flottante qui indique l'intervalle, en secondes, de l'heure réelle de l'horologe à laquelle le service doit renvoyer des mesures. Par défaut, le service renvoie des mesures toutes les secondes. Le service ignore ce paramètre tant que le paramètre `processing_metrics` n'est pas défini sur `true`.

    La valeur minimale par défaut de ce paramètre est 0,1 seconde. Le niveau de précision n'est pas limité ; vous pouvez donc indiquer des valeurs telles que 0,25 et 0,125. Le service n'impose pas de valeur maximale.

La manière dont vous fournissez les paramètres et la manière dont le service renvoie les mesures de traitement diffèrent selon les interfaces :

-   Avec l'interface WebSocket, vous indiquez les paramètres avec le message JSON `start` pour une demande de reconnaissance vocale. Le service calcule et renvoie des mesures en temps réel aux intervalles demandés.
-   Avec l'interface HTTP asynchrone, vous indiquez des paramètres de requête avec une demande de reconnaissance vocale. Le service calcule les mesures aux intervalles demandés, mais il renvoie toutes les mesures pour le contenu audio avec les résultats de transcription finaux.

Le service renvoie également des mesures de traitement pour les événements de transcription. Si vous demandez des résultats intermédiaires avec l'interface WebSocket, vous pouvez recevoir des mesures à une fréquence supérieure que l'intervalle demandé.

Pour recevoir des mesures de traitement uniquement pour les événements de transcription plutôt qu'à des intervalles réguliers, définissez l'intervalle de traitement sur un nombre élevé. Si l'intervalle est supérieur à la durée du contenu audio, le service renvoie des mesures de traitement uniquement pour les événements de transcription.

### Comprendre les mesures de traitement
{: #processing_metrics_understand}

Le service renvoie des mesures de traitement dans la zone `processing_metrics` de l'objet `SpeechRecognitionResults`. Pour les mesures générées à des intervalles réguliers, l'objet contient uniquement la zone `processing_metrics`, comme indiqué dans l'exemple suivant.

```javascript
{
  "processing_metrics": {
    "processed_audio": {
      "received": float,
      "seen_by_engine": float,
      "transcription": float,
      "speaker_labels": float
    },
    "wall_clock_since_first_byte_received": float,
    "periodic": boolean
  }
}
```
{: codeblock}

La zone `processing_metrics` contient un objet `ProcessingMetrics` qui comporte les zones suivantes :

-   `wall_clock_since_first_byte_received` correspond au temps réel, en secondes, qui s'est écoulé depuis que le service a reçu le premier octet d'entrée audio. Les valeurs de cette zone sont généralement des multiples de l'intervalle de mesures spécifié, avec deux différences :
    -   Les valeurs peuvent ne pas refléter des intervalles exacts tels que 0,25, 0,5, etc. Les valeurs réelles peuvent, par exemple, être 0,27, 0,52, etc., en fonction du moment où le service reçoit et traite de le contenu audio.
    -   Les valeurs des événements de transcription ne sont pas liées à l'intervalle de traitement. Le service renvoie les résultats liés aux événements lorsqu'ils se produisent.
-   `periodic` indique si les mesures s'appliquent à un intervalle régulier ou à un événement de transcription :
    -   `true` signifie que la réponse a été déclenchée par un intervalle de traitement. Les informations contiennent uniquement des mesures de traitement.
    -   `false` signifie que la réponse a été déclenchée par un événement de transcription. Les informations contiennent des mesures de traitement et des résultats de transcription.

    Utilisez cette zone pour identifier la raison pour laquelle le service a généré la réponse et pour filtrer différents résultats si nécessaire.
-   `processed_audio` inclut un objet `ProcessedAudio` qui fournit des informations de synchronisation détaillées sur le traitement du service de l'entrée audio.

L'objet `ProcessedAudio` comporte les zones suivantes. Toutes les zones font référence à des secondes de contenu audio à compter de cette réponse. Seule la zone `wall_clock_since_first_byte_received` fait référence au temps réel écoulé.

-   `received` correspond aux secondes de contenu audio reçues par le service.
-   `seen_by_engine` correspond aux secondes de contenu audio transmises par le service à son moteur de traitement de la parole.
-   `transcription` correspond aux secondes de contenu audio traitées pour la reconnaissance vocale par le service.
-   `speaker_labels` correspond aux secondes de contenu audio traitées pour les étiquettes de locuteur par le service. La réponse comprend cette zone uniquement si vous demandez des étiquettes de locuteur.

Le moteur de traitement de la parole analyse l'entrée audio plusieurs fois. L'objet `processed_audio` affiche les valeurs du contenu audio qui ont été traitées par le moteur et qui ne seront pas relues. Le contenu audio traité n’a aucun effet sur les hypothèses de reconnaissance futures.

Les mesures indiquent la progression et la complexité du traitement du moteur :

-   `seen_by_engine` correspond au contenu audio que le service a lu et transmis au moteur au moins une fois.
-   `received` - `seen_by_engine` correspond au contenu audio placé en mémoire tampon dans le service mais qui n'a pas encore été vu ou traité par le moteur.
-   La relation entre les heures est `received` >= `seen_by_engine` >= `transcription` >= `speaker_labels`.

Les relations suivantes peuvent également aider à comprendre les résultats :

-   Les valeurs des zones `received` et `seen_by_engine` sont supérieures aux valeurs des zones `transcription` et `speaker_labels` lors du traitement de la reconnaissance vocale. Le service doit recevoir le contenu audio pour pouvoir commencer à traiter les résultats.
-   Les valeurs des zones `received` et `seen_by_engine` sont identiques lorsque le service a terminé le traitement du contenu audio. Les valeurs finales des zones peuvent être supérieures aux valeurs des zones `transcription` et `speaker_labels` d'un nombre fractionnaire de secondes.
-   La valeur de la zone `speaker_labels` suit généralement la valeur de la zone `transcription` lors du traitement de la reconnaissance vocale. Les valeurs des zones `transcription` et `speaker_labels` sont identiques lorsque le service a terminé le traitement du contenu audio.

### Exemple de mesures de traitement : interface WebSocket
{: #processing_metrics_example_websocket}

L'exemple suivant montre le message `start` qui est transmis pour une demande à l'interface WebSocket. La demande active les mesures de traitement et définit leur intervalle sur 0,25 seconde. Elle définit également les paramètres `interim_results` et `speaker_labels` sur `true`. Le contenu audio contient le message simple "hello world long pause stop".

```javascript
function onOpen(evt) {
  var message = {
    action: 'start',
    content-type: 'audio/flac',
    processing_metrics: true,
    processing_metrics_interval: 0.25,
    interim_results: true,
    speaker_labels: true
  };
  websocket.send(JSON.stringify(message));
  websocket.send(blob);
}
```
{: codeblock}

L'exemple de sortie suivant montre les premiers résultats de mesures de traitement renvoyés par le service pour la demande.

```javascript
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 1.59,
      "transcription": 0.49,
      "speaker_labels": 0.0
    },
    "wall_clock_since_first_byte_received": 0.32,
    "periodic": true
  }
}
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 1.59,
      "transcription": 0.49,
      "speaker_labels": 0.0
    },
    "wall_clock_since_first_byte_received": 0.51,
    "periodic": false
  },
  "results": [
    {
      "alternatives": [
        {
          "timestamps": [
            [
              "hello",
              0.43,
              0.76
            ],
            [
              "world",
              0.76,
              1.22
            ]
          ],
          "transcript": "hello world "
        }
      ],
      "final": false
    }
  ],
  "result_index": 0
}
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 2.59,
      "transcription": 1.25,
      "speaker_labels": 0.0
    },
    "wall_clock_since_first_byte_received": 0.57,
    "periodic": true
  }
}
. . .
```
{: codeblock}

### Exemple de mesures de traitement : interface HTTP asynchrone
{: #processing_metrics_example_http}

L'exemple suivant montre une demande de reconnaissance vocale pour la méthode `/v1/recognitions` de l'interface HTTP asynchrone. La demande active les mesures de traitement et spécifie un intervalle de 0,25 seconde. Le fichier audio inclut à nouveau le message "hello world long pause stop".

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?processing_metrics=true&processing_metrics_interval=0.25"
```
{: pre}

L'exemple de sortie suivant montre les deux premiers résultats de mesures de traitement renvoyés par le service pour la demande.

```javascript
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 1.59,
      "transcription": 0.49
    },
    "wall_clock_since_first_byte_received": 0.32,
    "periodic": true
  }
}
{
  "processing_metrics": {
    "processed_audio": {
      "received": 7.04,
      "seen_by_engine": 2.59,
      "transcription": 1.25
    },
    "wall_clock_since_first_byte_received": 0.57,
    "periodic": true
  }
}
. . .
```
{: codeblock}

## Mesures audio
{: #audio_metrics}

Les mesures audio fournissent des informations détaillées sur les caractéristiques de signal de l'entrée audio.  Les résultats fournissent des mesures globales pour l'ensemble de l'entrée audio à la fin du traitement de la parole. Pour un utilisateur expérimenté techniquement, les mesures peuvent fournir un aperçu significatif des caractéristiques détaillées du contenu audio.

Vous pouvez utiliser des mesures audio pour fournir une indication en temps réel sur un problème d'entrée audio, voir même une éventuelle solution. Par exemple, vous pouvez fournir un message tel que "Il y a trop de bruit en arrière-plan" ou "Veuillez vous approcher du microphone." Vous pouvez également utiliser un outil analytique hors ligne pour examiner les caractéristiques de signal et suggérer des données adaptées aux futures mises à jour du modèle.

### Demande de mesures audio
{: #audio_metrics_request}

Pour demander des mesures audio, définissez le paramètre booléen `audio_metrics` sur `true`. Par défaut, le service ne renvoie aucune mesure.

-   Avec l'interface WebSocket, vous indiquez le paramètre avec le message JSON `start` pour une demande de reconnaissance vocale. 
-   Avec les interfaces HTTP, vous indiquez un paramètre de requête avec une demande de reconnaissance vocale.

Le service renvoie des mesures audio avec les résultats de transcription finaux. Il renvoie les mesures une seule fois pour l'ensemble du flux audio. Même si le service renvoie plusieurs résultats de transcription (pour différents blocs de contenu audio), il ne renvoie qu'une seule instance des mesures à la toute fin des résultats.

L'interface WebSocket accepte plusieurs flux audio (ou fichiers) avec une seule connexion. Vous pouvez délimiter différents flux en envoyant des messages `stop` ou des objets BLOB binaires vides au service. Dans ce cas, le service renvoie des mesures distinctes pour chaque flux audio délimité.

### Comprendre les mesures audio
{: #audio_metrics_understand}

Le service renvoie les mesures dans la zone `audio_metrics` de l'objet `SpeechRecognitionResults`.

```javascript
{
  "results": [
    . . .
  ],
  "result_index": integer,
  "audio_metrics": {
    "sampling_interval": float,
    "accumulated": {
      "final": boolean,
      "end_time": float,
      "signal_to_noise_ratio": float,
      "speech_ratio": float,
      "high_frequency_loss": float,
      "direct_current_offset": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ],
      "clipping_rate": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ],
      "speech_level": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ],
      "non_speech_level": [
        {"begin": float, "end": float, "count": integer},
        . . .
      ]
    }
  }
}
```
{: codeblock}

La zone `audio_metrics` contient un objet `AudioMetrics` qui comporte deux zones :

-   `sampling_interval` indique l'intervalle, en secondes (généralement 0,1 seconde), auquel le service a calculé les mesures audio.
-   `accumulated` contient un objet `AudioMetricsDetails` qui fournit des informations détaillées sur les caractéristiques de signal de l'entrée audio.

Les zones suivantes de l'objet `AudioMetricsDetails` fournissent des valeurs unaires :

-   `final` indique si les mesures correspondent à la fin du flux audio, ce qui signifie que la transcription est terminée. Actuellement, la zone est toujours `true`. Le service renvoie les mesures une seule fois par flux audio. Les résultats fournissent des mesures globales pour l'ensemble du flux.
-   `end_time` indique l'heure de fin du contenu audio, en secondes, à laquelle les mesures s'appliquent. Les mesures s'appliquant à l'ensemble du flux audio, l'heure de fin correspnd toujours à la longueur du contenu audio.
-   `signal_to_noise_ratio` fournit le rapport signal sur bruit (SNR) du signal audio. La valeur indique le rapport parole-bruit dans le contenu audio. Une valeur valide est comprise entre 0 et 100 décibels (dB). Le service omet la zone s'il ne peut pas calculer le rapport SNR du contenu audio.
-   `speech_ratio` correspond au rapport entre les segments de parole et sans parole dans le signal audio. La valeur est comprise entre 0,0 et 1,0.
-   `high_frequency_loss` indique la probabilité que le signal audio manque la moitié supérieure de son contenu en fréquence.
    -   Une valeur proche de 1,0 indique généralement un contenu audio échantillonné artificiellement, ce qui a un impact négatif sur la précision des résultats de transcription.
    -   Une valeur égale ou proche de 0,0 indique que le signal audio est bon et a un spectre complet.
    -   Une valeur d'environ 0,5 signifie que la détection du contenu en fréquence est peu fiable ou non disponible.

Les zones suivantes de l'objet `AudioMetricsDetails` fournissent des histogrammes pour les caractéristique de signal. Chaque zone fournit un tableau d'objets `AudioMetricsHistogramBin`. Une seule unité dans chaque histogramme est calculée en fonction de la longueur `sampling_interval` du contenu audio.

-   `direct_current_offset` définit un histogramme du composant de courant continu cumulatif direct (CC) du signal audio.
-   `clipping_rate` définit un histogramme du taux de coupure des segments audio. Le taux de coupure est défini comme la fraction d'échantillons du segment qui atteint la valeur maximale ou minimale offerte par la plage de quantification audio.

    Le service détecte automatiquement une plage audio (-32768 à +32767) à modulation par impulsions codées (PCM) 16 bits ou une plage d'unités (-1,0 à +1,0). Le taux de coupure est compris entre 0,0 et 1,0. Des valeurs plus élevées indiquent une dégradation possible de la reconnaissance vocale.
-   `speech_level` définit un histogramme du niveau de signal dans des segments du contenu audio contenant de la parole. Le niveau du signal correspond à la valeur quadratique moyenne (RMS) sur une échelle en décibels (dB) normalisée dans la plage allant de 0,0 (niveau minimum) à 1,0 (niveau maximum).
-   `non_speech_level` définit un histogramme du niveau de signal dans des segments de contenu audio ne contenant pas de parole. Le niveau du signal correspond à la valeur RMS sur une échelle en décibels normalisée dans la plage allant de 0,0 (niveau minimal) à 1,0 (niveau maximal).

Chaque objet `AudioMetricsHistogramBin` décrit une catégorie avec des limites `begin` et `end` définies. Chaque catégorie indique le nombre `count` ou le nombre de valeurs dans la plage des caractéristiques du signal pour cette catégorie Les première et dernière catégories d'un histogramme sont les catégories limites. Elles couvrent respectivement les intervalles entre l'infini négatif et la première limite et entre la dernière limite et l'infini positif.

### Exemple de mesures audio
{: #audio_metrics_example}

L'exemple suivant montre une demande de reconnaissance vocale avec l'interface HTTPS synchrone qui renvoie des mesures audio. Le fichier audio inclut le message simple "hello world long pause stop".

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitize?audio_metrics=true"
```
{: pre}

La réponse inclut les mesures audio pour l'ensemble de l'entrée audio complète, qui a une durée de 7,0 secondes. L'entrée audio contient légèrement plus de segments de parole que de segments sans parole : 37 à 33 segments, pour un rapport de parole `speech_ratio` de 0,529. Le taux de coupure est très faible, ce qui indique une qualité d'entrée audio élevée.

La zone `high_frequency_loss` a la valeur 0,5, ce qui signifie que la détection du contenu en fréquence du service n'est pas fiable ou n'est pas disponible. Les résultats omettent la zone `signal_to_noise_ratio` car le service n'est pas parvenu à calculer le rapport SNR pour le contenu audio.

```javascript
{
  "results": [
    {
      "alternatives": [
        {
          "confidence": 0.96,
          "transcript": "hello world "
        }
      ],
         "final": true
      },
      {
      "alternatives": [
        {
          "confidence": 0.79,
          "transcript": "long pause "
        }
      ],
         "final": true
      },
      {
      "alternatives": [
        {
          "confidence": 0.97,
          "transcript": "stop "
        }
      ],
      "final": true
    }
  ],
  "result_index": 0,
  "audio_metrics": {
    "sampling_interval": 0.1,
    "accumulated": {
      "final": true,
      "end_time": 7.0,
      "speech_ratio": 0.529,
      "high_frequency_loss": 0.5,
      "direct_current_offset": [
        {"begin": -1.0, "end": -0.9, "count": 0},
        {"begin": -0.9, "end": -0.7, "count": 0},
        {"begin": -0.7, "end": -0.5, "count": 0},
        {"begin": -0.5, "end": -0.3, "count": 0},
        {"begin": -0.3, "end": -0.1, "count": 0},
        {"begin": -0.1, "end": 0.1, "count": 70},
        {"begin": 0.1, "end": 0.3, "count": 0},
        {"begin": 0.3, "end": 0.5, "count": 0},
        {"begin": 0.5, "end": 0.7, "count": 0},
        {"begin": 0.7, "end": 0.9, "count": 0},
        {"begin": 0.9, "end": 1.0, "count": 0}
      ],
      "clipping_rate": [
        {"begin": 0.0, "end": 1e-05, "count": 70},
        {"begin": 1e-05, "end": 0.0001, "count": 0},
        {"begin": 0.0001, "end": 0.001, "count": 0},
        {"begin": 0.001, "end": 0.01, "count": 0},
        {"begin": 0.01, "end": 0.1, "count": 0},
        {"begin": 0.1, "end": 1.0, "count": 0}
      ],
      "speech_level": [
        {"begin": 0.0, "end": 0.1, "count": 37},
        {"begin": 0.1, "end": 0.2, "count": 0},
        {"begin": 0.2, "end": 0.3, "count": 0},
        {"begin": 0.3, "end": 0.4, "count": 0},
        {"begin": 0.4, "end": 0.5, "count": 0},
        {"begin": 0.5, "end": 0.6, "count": 0},
        {"begin": 0.6, "end": 0.7, "count": 0},
        {"begin": 0.7, "end": 0.8, "count": 0},
        {"begin": 0.8, "end": 0.9, "count": 0},
        {"begin": 0.9, "end": 1.0, "count": 0}
      ],
      "non_speech_level": [
        {"begin": 0.0, "end": 0.1, "count": 33},
        {"begin": 0.1, "end": 0.2, "count": 0},
        {"begin": 0.2, "end": 0.3, "count": 0},
        {"begin": 0.3, "end": 0.4, "count": 0},
        {"begin": 0.4, "end": 0.5, "count": 0},
        {"begin": 0.5, "end": 0.6, "count": 0},
        {"begin": 0.6, "end": 0.7, "count": 0},
        {"begin": 0.7, "end": 0.8, "count": 0},
        {"begin": 0.8, "end": 0.9, "count": 0},
        {"begin": 0.9, "end": 1.0, "count": 0}
      ]
    }
  }
}
```
{: codeblock}
