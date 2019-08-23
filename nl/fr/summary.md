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

# Récapitulatif des paramètres
{: #summary}

Voici un récapitulatif de tous les paramètres disponibles pour la reconnaissance vocale. Pour plus d'informations sur toutes les méthodes du service {{site.data.keyword.speechtotextshort}}, voir [Référence d'API](https://{DomainName}/apidocs/speech-to-text){: external}.
{: shortdesc}

Tenez compte des conditions de base à remplir lorsque vous effectuez une demande de reconnaissance vocale :

-   Les noms de méthode sont sensibles à la casse.
-   Les en-têtes de demande HTTP ne sont pas sensibles à la casse.
-   Les paramètres de requête HTTP et WebSocket sont sensibles à la casse.
-   Les noms de zone JSON sont sensibles à la casse.
-   Tout le contenu des réponses JSON utilise le jeu de caractères UTF-8.

Tenez compte également des exigences suivantes spécifiques au service :

-   Vous ne devez spécifier que l'entrée audio. Tous les autres paramètres sont facultatifs.
-   Si vous spécifiez un paramètre de requête ou une zone JSON non valide dans l'entrée, la réponse comprend une zone `warnings` pour décrire l'argument non valide. La demande aboutit malgré les avertissements de ce type.

## access_token
{: #summary-access-token}

Si vous utilisez l'authentification IAM (Identity and Access Management), il s'agit d'un jeton d'accès IAM facultatif que vous utilisez pour établir une connexion authentifiée avec l'interface WebSocket. Pour plus d'informations, voir [Ouverture d'une connexion](/docs/services/speech-to-text?topic=speech-to-text-websockets#WSopen).

<table>
  <caption>Tableau 1. Paramètre access_token</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la demande de connexion <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Non pris en charge
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Non pris en charge
    </td>
  </tr>
</table>

## acoustic_customization_id
{: #summary-acoustic-customization-id}

ID de personnalisation facultatif d'un modèle acoustique personnalisé adapté aux caractéristiques acoustiques de votre environnement et de vos locuteurs. Par défaut, aucun modèle personnalisé n'est utilisé. Pour plus d'informations, voir [Modèles personnalisés](/docs/services/speech-to-text?topic=speech-to-text-input#custom-input).

<table>
  <caption>Tableau 2. Paramètre acoustic_customization_id</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Version bêta pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la demande de connexion <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## audio_metrics
{: #summary-audio-metrics}

Valeur booléenne facultative indiquant si le service renvoie des mesures relatives aux caractéristiques de signal de l'entrée audio. Par défaut (valeur `false`), le service ne renvoie aucune mesure audio. Pour plus d'informations, voir [Audio metrics](/docs/services/speech-to-text?topic=speech-to-text-metrics#audio_metrics).

<table>
  <caption>Tableau 3. Paramètre audio_metrics</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## base_model_version
{: #summary-base-model-version}

Version facultative d'un modèle de base. Ce paramètre est principalement conçu pour être utilisé avec les modèles personnalisés mis à jour pour un nouveau modèle de base, mais il peut être utilisé sans modèle personnalisé. La valeur par défaut dépend de l'utilisation de ce paramètre avec ou sans modèle personnalisé. Pour plus d'informations, voir [Version du modèle de base](/docs/services/speech-to-text?topic=speech-to-text-input#version).

<table>
  <caption>Tableau 4. Paramètre base_model_version</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la demande de connexion <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## Content-Type
{: #summary-content-type}

Format audio (type MIME) facultatif indiquant le format des données audio que vous transmettez au service. Le service peut détecter automatiquement le format de la plupart des données audio, par conséquent ce paramètre est facultatif pour la plupart des formats. Il est obligatoire pour les formats `audio/alaw`, `audio/basic`, `audio/l16` et `audio/mulaw`. Pour plus d'informations, voir [Formats audio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats).

<table>
  <caption>Tableau 5. Paramètre Content-Type</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre <code>content-type</code> du message JSON
      <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      En-tête de demande de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      En-tête de demande de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## customization_weight
{: #summary-customization-weight}

Valeur facultative à deux chiffres comprise entre 0.0 et 1.0 indiquant le poids relatif que le service attribue aux mots d'un modèle de langue personnalisé par rapport aux mots du vocabulaire de base. La valeur par défaut est 0.3 sauf si un autre poids a été spécifié lors de l'entraînement du modèle de langue personnalisé. Pour plus d'informations, voir [Modèles personnalisés](/docs/services/speech-to-text?topic=speech-to-text-input#custom-input).

<table>
  <caption>Tableau 6. Paramètre customization_weight</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour l'anglais-américain, l'anglais britannique, le portugais brésilien, le français, l'allemand, le japonais, le coréen et l'espagnol
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## grammar_name
{: #summary-grammar-name}

Chaîne facultative identifiant une grammaire à utiliser pour la reconnaissance vocale. Le service ne reconnaît que les chaînes définies par la grammaire. Vous devez spécifier le nom de la grammaire et l'ID de personnalisation (customization_id) du modèle de langue personnalisé pour lequel est définie la grammaire. Pour plus d'informations, voir [Grammaires](/docs/services/speech-to-text?topic=speech-to-text-input#grammars-input).

<table>
  <caption>Tableau 7. Paramètre grammar_name</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Version bêta pour l'anglais-américain, l'anglais britannique, le portugais brésilien, le français, l'allemand, le japonais, le coréen et l'espagnol
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## inactivity_timeout
{: #summary-inactivity-timeout}

Nombre entier facultatif utilisé pour spécifier le nombre de secondes correspondant au délai d'attente d'inactivité du service. L'inactivité signifie que le service ne détecte aucune parole dans la diffusion audio en continu. La valeur par défaut est fixée à 30 secondes. Utilisez `-1` pour indiquer une valeur infinie. Pour plus d'informations, voir [Délai d'attente d'inactivité](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts-inactivity).

<table>
  <caption>Tableau 8. Paramètre inactivity_timeout</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## interim_results
{: #summary-interim-results}

Valeur booléenne facultative qui demande au service de renvoyer des hypothèses intermédiaires qui sont susceptibles de changer avant la retranscription finale. Par défaut (valeur `false`), les résultats intermédiaires ne sont pas renvoyés. Pour plus d'informations, voir [Résultats intermédiaires](/docs/services/speech-to-text?topic=speech-to-text-output#interim).

<table>
  <caption>Tableau 9. Paramètre interim_results</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Non pris en charge
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Non pris en charge
    </td>
  </tr>
</table>

## keywords
{: #summary-keywords}

Tableau facultatif de chaînes de mots clés que le service détecte dans l'entrée audio. Par défaut, la détection de mots clés n'est pas effectuée. Pour plus d'informations, voir [Détection de mots clés](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting).

<table>
  <caption>Tableau 10. Paramètre keywords</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## keywords_threshold
{: #summary-keywords-threshold}

Valeur facultative à deux chiffres comprise entre 0.0 et 1.0 indiquant le seuil minimal d'une correspondance positive de mot clé. Par défaut, la détection de mots clés n'est pas effectuée. Pour plus d'informations, voir [Détection de mots clés](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting).

<table>
  <caption>Tableau 11. Paramètre keywords_threshold</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## language_customization_id
{: #summary-language-customization-id}

ID de personnalisation facultatif d'un modèle de langue personnalisé incluant la terminologie de votre domaine. Par défaut, aucun modèle personnalisé n'est utilisé. Pour plus d'informations, voir [Modèles personnalisés](/docs/services/speech-to-text?topic=speech-to-text-input#custom-input).

<table>
  <caption>Tableau 12. Paramètre language_customization_id</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour l'anglais-américain, l'anglais britannique, le portugais brésilien, le français, l'allemand, le japonais, le coréen et l'espagnol
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la demande de connexion <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## max_alternatives
{: #summary-max-alternatives}

Nombre entier facultatif utilisé pour spécifier le nombre maximal d'hypothèses alternatives renvoyées par le service. Par défaut, le service ne renvoie qu'une seule hypothèse finale. Pour plus d'informations, voir [Nombre maximal d'alternatives](/docs/services/speech-to-text?topic=speech-to-text-output#max_alternatives).

<table>
  <caption>Tableau 13. Paramètre max_alternatives</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## model
{: #summary-model}

Modèle facultatif qui indique en quelle langue est énoncé l'audio et la fréquence à laquelle il a été échantillonné : large bande ou bande étroite. Par défaut, il s'agit du modèle `en-US_BroadbandModel`. Pour plus d'informations, voir [Langues et modèles](/docs/services/speech-to-text?topic=speech-to-text-models).

<table>
  <caption>Tableau 14. Paramètre model</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la demande de connexion <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## processing_metrics
{: #summary-processing-metrics}

Valeur booléenne facultative indiquant si le service renvoie des mesures relatives à son traitement de l'entrée audio. Par défaut (`false`), le service ne renvoie aucune mesure de traitement. Pour plus d'informations, voir [Processing metrics](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics).

<table>
  <caption>Tableau 15. Paramètre processing_metrics</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Non pris en charge
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## processing_metrics_interval
{: #summary-processing-metrics-interval}

Valeur flottante facultative d'au moins 0,1 indiquant l'intervalle auquel le service doit renvoyer des mesures de traitement. Si le paramètre `processing_metrics` a la valeur `true`, le service renvoie par défaut des mesures de traitement toutes les 1.0 secondes. Pour plus d'informations, voir [Processing metrics](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics).

<table>
  <caption>Tableau 16. Paramètre processing_metrics_interval</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Non pris en charge
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## profanity_filter
{: #summary-profanity-filter}

Valeur booléenne facultative indiquant si le service censure les grossièretés dans une retranscription. Par défaut (valeur `true`), les grossièretés sont exclues de la retranscription. Pour plus d'informations, voir [Filtrage des grossièretés](/docs/services/speech-to-text?topic=speech-to-text-output#profanity_filter).

<table>
  <caption>Tableau 17. Paramètre profanity_filter</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour l'anglais américain
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## redaction
{: #summary-redaction}

Valeur booléenne facultative indiquant si le service doit occulter (masquer) les données numériques comportant au moins trois chiffres consécutifs dans une retranscription. Si vous définissez le paramètre `redaction` avec la valeur `true`, le service oblige automatiquement le paramètre `smart_formatting` à avoir la valeur `true`. Par défaut (valeur `false`), les données numériques ne sont pas masquées. Pour plus d'informations, voir [Occultation numérique](/docs/services/speech-to-text?topic=speech-to-text-output#redaction).

<table>
  <caption>Tableau 18. Paramètre redaction</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Version bêta pour l'anglais américain, le japonais et le coréen
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## smart_formatting
{: #summary-smart-formatting}

Valeur booléenne facultative indiquant si le service doit convertir les dates, heures, nombres, devises et valeurs similaires en représentations plus conventionnelles dans la retranscription finale. Pour l'anglais américain, la fonction convertit également certains groupes de mots clés en signes de ponctuation. Par défaut (valeur `false`), le formatage intelligent n'est pas appliqué. Pour plus d'informations, voir [Formatage intelligent](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting).

<table>
  <caption>Tableau 19. Paramètre smart_formatting</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Version bêta pour l'anglais américain, le japonais et l'espagnol
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## speaker_labels
{: #summary-speaker-labels}

Valeur booléenne facultative indiquant si le service identifie à quelles personnes attribuer les mots énoncés dans un échange comportant plusieurs intervenants. Si vous définissez le paramètre `speaker_labels` avec la valeur `true`, le service oblige automatiquement le paramètre `timestamps` à avoir la valeur `true`. Par défaut (valeur `false`), ces étiquettes speaker_labels ne sont pas renvoyées. Pour plus d'informations, voir [Etiquettes de locuteur](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels).

<table>
  <caption>Tableau 20. Paramètre speaker_labels</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Version bêta pour l'anglais américain, le japonais et l'espagnol (modèles à large bande et à bande étroite) et pour l'anglais britannique (modèle à bande étroite uniquement)
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## timestamps
{: #summary-timestamps}

Valeur booléenne facultative indiquant si le service ajoute les horodatages des mots de la retranscription. Par défaut (valeur `false`), les horodatages ne sont pas renvoyés. Pour plus d'informations, voir [Horodatage des mots](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps).

<table>
  <caption>Tableau 21. Paramètre timestamps</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## Transfer-Encoding
{: #summary-transfer-encoding}

Avec la valeur facultative `chunked`, ce paramètre entraîne la diffusion audio en continu vers le service. Par défaut, les données audio sont envoyées en totalité sous forme de diffusion ponctuelle. Pour plus d'informations, voir [Transmission audio](/docs/services/speech-to-text?topic=speech-to-text-input#transmission).

<table>
  <caption>Tableau 22. Paramètre Transfer-Encoding</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Non applicable. Diffusion en continu systématique
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      En-tête de demande de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      En-tête de demande de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## watson-token
{: #summary-watson-token}

Si vous utilisez des données d'identification de service Cloud Foundry, il s'agit d'un jeton d'authentification {{site.data.keyword.watson}} facultatif que vous utilisez pour établir une connexion authentifiée avec l'interface WebSocket. Pour plus d'informations, voir [Ouverture d'une connexion](/docs/services/speech-to-text?topic=speech-to-text-websockets#WSopen).

<table>
  <caption>Tableau 23. Paramètre watson-token</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la demande de connexion <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Non pris en charge
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Non pris en charge
    </td>
  </tr>
</table>

## word_alternatives_threshold
{: #summary-word-alternatives-threshold}

Valeur facultative à deux chiffres comprise entre 0.0 et 1.0 indiquant le seuil utilisé par le service pour signaler d'autres propositions similaires acoustiquement pour des mots figurant dans l'entrée audio. Par défaut ces propositions ne sont pas renvoyées. Pour plus d'informations, voir [Autres propositions de mots](/docs/services/speech-to-text?topic=speech-to-text-output#word_alternatives).

<table>
  <caption>Tableau 24. Paramètre word_alternatives_threshold</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## word_confidence
{: #summary-word-confidence}

Valeur booléenne facultative indiquant si le service fournit des mesures de confiance pour les mots de la retranscription. Par défaut (valeur `false`), les mesures de confiance des mots ne sont pas renvoyées. Pour plus d'informations, voir [Confiance des mots](/docs/services/speech-to-text?topic=speech-to-text-output#word_confidence).

<table>
  <caption>Tableau 25. Paramètre word_confidence</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre du message JSON <code>start</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      Paramètre de requête de la méthode <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>

## X-Watson-Authorization-Token
{: #summary-x-watson-authorization-token}

Jeton d'authentification facultatif qui effectue des demandes authentifiées au service sans incorporer vos données d'identification de service dans tous les appels. Par défaut, les données d'identification de service doivent être transmises avec chaque demande. Les jetons d'authentification Watson sont basés sur les données d'identification de service Cloud Foundry qui utilisent un nom d'utilisateur (`{username}`) et un mot de passe (`{password}`) pour l'authentification.

L'en-tête `X-Watson-Authorization-Token` n'accepte pas les clés d'API ou les jetons IAM.
{: note}

<table>
  <caption>Tableau 26. Paramètre X-Watson-Authorization-Token</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Non pris en charge. utilisez le paramètre de requête <code>watson-token</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      En-tête de demande de chaque demande
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      En-tête de demande de chaque demande
    </td>
  </tr>
</table>

## X-Watson-Learning-Opt-Out
{: #summary-x-watson-learning-opt-out}

Valeur booléenne facultative indiquant si vous désactivez la journalisation des demandes par défaut, qui est appliquée par {{site.data.keyword.IBM_notm}} afin d'améliorer le service pour les futurs utilisateurs. Pour empêcher IBM d'accéder à vos données en vue d'apporter des améliorations générales au service, spécifiez la valeur <code>true</code> pour ce paramètre. Pour plus d'informations, voir [Journalisation des demandes](/docs/services/speech-to-text?topic=speech-to-text-input#logging).

<table>
  <caption>Tableau 27. Paramètre X-Watson-Learning-Opt-Out</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre de requête <code>x-watson-learning-opt-out</code> de
      la demande de connexion <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      En-tête de demande de chaque demande
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      En-tête de demande de chaque demande
    </td>
  </tr>
</table>

## X-Watson-Metadata
{: #summary-x-watson-metadata}

Chaîne facultative associant un ID client aux données transmises pour les demandes de reconnaissance. Ce paramètre accepte l'argument `customer_id={id}`. Par défaut, aucun ID client n'est associé aux données. Pour plus d'informations, voir [Sécurité des informations](/docs/services/speech-to-text?topic=speech-to-text-information-security).

<table>
  <caption>Tableau 28. Paramètre X-Watson-Metadata</caption>
  <tr>
    <th>Disponibilité et utilisation</th>
    <th style="vertical-align:bottom">Description</th>
  </tr>
  <tr>
    <td style="text-align:left; width:30%">
      **Disponibilité**
    </td>
    <td style="text-align:left">
      Disponible en version GA pour toutes les langues
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **WebSocket**
    </td>
    <td style="text-align:left">
      Paramètre de requête <code>x-watson-metadata</code> de
      la demande de connexion <code>/v1/recognize</code> (vous devez coder l'argument
      en codage URL, par exemple, `customer_id%3dmy_customer_ID`.)
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP synchrone**
    </td>
    <td style="text-align:left">
      En-tête de demande de la demande POST <code>/v1/recognize</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      **HTTP asynchrone**
    </td>
    <td style="text-align:left">
      En-tête de demande des demandes <code>POST /v1/register_callback</code>
      et <code>POST /v1/recognitions</code>
    </td>
  </tr>
</table>
