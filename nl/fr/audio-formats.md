---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-15"

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

# Formats audio
{: #audio-formats}

Le service {{site.data.keyword.speechtotextfull}} peut extraire des conversations audio dans de nombreux formats différents.

-   Si l'audio ne vous est pas familier et si vous ignorez comment il est décrit et spécifié, commencez par la section [Caractéristiques et terminologie audio](#terminology) pour vous y initier.
-   Si vous savez déjà comment utiliser l'audio, passez à la section [Formats audio pris en charge](#formats) pour obtenir des informations détaillées sur les formats pris en charge par le service.

Les sections finales, [Limites et compression de données](#limits), [Conversion audio](#conversion) et [Astuces pour améliorer la reconnaissance vocale](#audioTips), peuvent vous aider à profiter pleinement du service.

## Caractéristiques et terminologie audio
{: #terminology}

La terminologie suivante est utilisée pour décrire les caractéristiques des données audio pour les différents formats.

### Fréquence d'échantillonnage
{: #samplingRate}

La *fréquence d'échantillonnage* (ou taux d'échantillonnage) correspond au nombre d'échantillons audio pris par seconde. Cette fréquence est mesurée en hertz (Hz) ou kilohertz (kHz). Par exemple, une fréquence de 16 000 échantillons par seconde équivaut à 16 000 Hz (ou 16 kHz). Avec le service {{site.data.keyword.speechtotextshort}}, vous spécifiez un modèle pour indiquer la fréquence d'échantillonnage de vos données audio.

-   Les modèles à *large bande* sont utilisés pour les données audio échantillonnées à partir de 16 kHz, valeur recommandée par {{site.data.keyword.IBM}} pour des applications réactives en temps réel (par exemple, pour les applications de conversation en direct).
-   Les modèles à *bande étroite* sont utilisés pour les données audio échantillonnées à partir de 8 kHz, ce qui correspond à la fréquence utilisée en principe par les réseaux de téléphonie.

Le service prend en charge les modèles audio à large bande et à bande étroite pour la plupart des langues et des formats. Il ajuste automatiquement la fréquence d'échantillonnage de vos données audio pour correspondre au modèle que vous spécifiez avant de procéder à la reconnaissance vocale.

-   Pour les modèles à large bande, le service convertit les données audio enregistrées à des fréquences d'échantillonnage plus élevées pour passer à 16 kHz.
-   Pour les modèles à bande étroite, il convertit les données audio pour passer à 8 kHz.

En théorie, vous pouvez envoyer 44 kHz d'audio avec un modèle à large bande ou à bande étroite, mais vous augmentez ainsi inutilement la taille de l'audio. Pour minimiser la quantité d'audio que vous envoyez, faites correspondre la fréquence d'échantillonnage des données audio au modèle que vous utilisez. Le service n'accepte pas d'audio échantillonné à une fréquence inférieure à la fréquence d'échantillonnage intrinsèque du modèle.

#### Remarques à propos des formats audio
{: #samplingRateNotes}

-   Pour les formats `audio/alaw`, `audio/l16` et `audio/mulaw`, vous devez spécifier votre fréquence audio.
-   Pour les formats `audio/basic` et `audio/g729`, seuls les modèles audio à bandes étroite sont pris en charge.

#### Informations complémentaires
{: #samplingRateMore}

-   Pour plus d'informations sur les fréquences d'échantillonnage, voir [en.wikipedia.org/wiki/Sampling ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/Sampling){: new_window}. Sélectionnez *Sampling (signal processing)*.
-   Pour plus d'informations sur les modèles proposés par le service pour chaque langue prise en charge, voir [Langues et modèles](/docs/services/speech-to-text/models.html).

### Débit binaire
{: #bitRate}

Le *débit binaire* correspond au nombre de bits de données envoyées par seconde. Le débit binaire d'un flux audio est mesuré en kilobits par seconde (kbit/s). Le débit binaire est calculé à partir de la fréquence d'échantillonnage et du nombre de bits stockés par échantillon. Pour la reconnaissance vocale, {{site.data.keyword.IBM}} vous recommande d'enregistrer 16 bits par échantillon pour l'audio.

Par exemple, l'audio utilisant une fréquence d'échantillonnage large bande de 16 kHz et 16 bits par échantillon a un débit binaire de 256 kbit/s : `(16,000 * 16) / 1000`.

#### Informations complémentaires
{: #bitRateMore}

-   Pour plus d'informations sur les débits binaires, voir [en.wikipedia.org/wiki/Bit_rate ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/Bit_rate){: new_window}.
-   Pour une discussion générale sur les fréquences d'échantillonnage et les débits binaires, voir [What are bit rates? ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://www.richardfarrar.com/what-are-bit-rates/){: new_window} et [Choosing bit rates for podcasts ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://www.richardfarrar.com/choosing-bit-rates-for-podcasts/){: new_window}.

### Compression
{: #compression}

La *compression* est utilisée par de nombreux formats audio pour réduire la taille des données audio. Elle réduit le nombre de bits stockés par échantillon et par la même occasion le débit binaire. Certains formats n'utilisent pas la compression, mais la plupart des formats utilisent l'un des types de compression de base :

-   La compression *sans perte* réduit la taille des données audio sans perte de qualité, mais le taux de compression est en principe faible.
-   La compression *avec perte* divise par 10 la taille des données audio, mais au prix d'une perte irrémédiable de données et au détriment de la qualité lors de la compression.

Avec le service {{site.data.keyword.speechtotextshort}}, vous pouvez utiliser la compression avec perte en toute tranquillité pour maximiser la quantité de données audio que vous envoyez au service avec une demande de reconnaissance. Comme la plage dynamique de la voix humaine est plus limitée que par exemple celle de la musique, une conversation peut s'accommoder d'un débit binaire beaucoup plus faible que d'autres types d'audio. Pour la reconnaissance vocale, {{site.data.keyword.IBM}} vous recommande d'utiliser 16 bits par échantillon pour l'audio et d'employer un format qui compresse les données audio.

#### Remarques à propos des formats audio
{: #compressionNotes}

-   Les formats `audio/ogg` et `audio/webm` sont des conteneurs dont la compression s'appuie sur le codec que vous utilisez pour coder les données : Opus ou Vorbis.
-   Le format `audio/wav` est un conteneur qui comprend des données non compressées, avec perte ou sans perte.

#### Informations complémentaires
{: #compressionMore}

-   Pour plus d'informations sur la compression audio, voir [en.wikipedia.org/wiki/Data_compression#Audio ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/Data_compression#Audio){: new_window}.
-   Pour plus d'informations sur l'utilisation de la compression de données pour augmenter la quantité de données audio que vous pouvez transmettre avec une demande, voir [Limites et compression de données](#limits).

### Canaux
{: #channels}

Les *canaux* indiquent le nombre de flux dans l'enregistrement audio :

-   Le mode *monaural* (ou mono) ne comporte qu'un seul canal audio.
-   Le mode *stéréophonique* (ou stéréo) comporte en principe deux canaux audio.

Le service {{site.data.keyword.speechtotextshort}} accepte 16 canaux audio maximum. Comme il utilise un seul canal pour la reconnaissance vocale, le service règle le mode de mixage à plusieurs canaux sur un canal mono lors du transcodage.

#### Remarques à propos des formats audio
{: #channelsNotes}

-   Pour le format `audio/l16`, vous devez spécifier le nombre de canaux audio si vous avez plus d'un canal.
-   Pour le format `audio/wav`, le service accepte un maximum de neuf canaux audio.

#### Informations complémentaires
{: #channelsMore}

-   Pour plus d'informations sur les canaux audio, voir [en.wikipedia.org/wiki/Audio_signal ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/Audio_signal){: new_window}.

### Ordre d'octets
{: #endianness}

L'ordre d'octets (*endianness*) indique comment sont organisés les octets de données selon l'architecture d'ordinateur sous-jacente :

-   *Big-endian* organise les données par bit de poids fort.
-   *Little-endian* organise les données par bit de poids faible.

Le service {{site.data.keyword.speechtotextshort}} détecte automatiquement l'ordre d'octets des données audio entrantes.

#### Remarques à propos des formats audio
{: #endiannessNotes}

-   Pour le format `audio/l16`, vous pouvez spécifier l'ordre d'octets pour désactiver la détection automatique si nécessaire.

#### Informations complémentaires
{: #endiannessMore}

-   Pour plus d'informations sur l'ordre d'octets, voir [en.wikipedia.org/wiki/Endianness ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/Endianness){: new_window}.

### Fréquence audio
{: #frequency}

La *fréquence audio* désigne la plage de fréquences audibles dans le domaine audio. La fréquence audible standard pour les humains est comprise en général entre 20 et 20 000 Hz. Vous pouvez recourir à une analyse spectrographique pour obtenir un spectrogramme révélant le contenu de vos données audio en termes de fréquence.

La fréquence d'échantillonnage qui est appliquée aux données audio est en principe deux fois supérieure à la fréquence audio maximale. Par exemple, une fréquence d'échantillonnage de 16 kHz signifie que la fréquence maximale du signal audio échantillonné est de 8 kHz. Les modèles du service sont créés dans cette optique.

-   Les modèles à bande étroite sont construits avec des données audio échantillonnées à 8 kHz. Les modèles à bande étroite s'attendent à trouver des informations dans une plage inférieure ou égale à 4 kHz.
-   Les modèles à large bande sont construits avec des données audio échantillonnées à 16 kHz. Les modèles à large bande s'attendent à trouver des informations dans une plage de 4 à 8 kHz.

Les données d'entraînement des modèles sont dérivées des différents canaux (téléphonie pour les modèles à bande étroite). Les modèles reflètent les caractéristiques des canaux sur lesquels ils ont été entraînés.

#### Sur-échantillonnage
{: #upsampling}

Le *sur-échantillonnage* (upsampling) augmente la fréquence d'échantillonnage des données audio mais n'introduit pas de nouvelles informations dans le signal audio. Il produit un signal audio approximatif qui aurait été obtenu en échantillonnant les données audio à une fréquence plus élevée. Il augmente la taille des données audio.

Les informations audio échantillonnées à l'origine à une fréquence à bande étroite sont limitées à une plage de 0 à 4 kHz. Sur-échantillonner des données audio à bande étroite à une fréquence d'échantillonnage plus élevée offre peu de chance d'améliorer la précision de la reconnaissance vocale. Si vous sur-échantillonnez des données audio à bande étroite, il manquera des informations de plage escomptées par les modèles à large bande. De plus, les informations recueillies dans la plage prévue pour un échantillon à bande étroite sont qualitativement différentes de celles recueillies dans la même plage pour un échantillon à large bande. Par conséquent, le sur-échantillonnage nuit à l'exactitude de la reconnaissance.

Pour une fréquence d'échantillonnage à large bande de 16 kHz, la valeur prévue pour la fréquence maximale présente dans le signal audio échantillonné est de 8 kHz. Par conséquent, vous devez filtrer le signal d'origine à 8 kHz avant de l'échantillonner à une fréquence de 16 kHz. Autrement, la qualité est altérée en raison d'un phénomène connu sous le nom de *aliasing* (repliement de spectre). Pour comprendre pourquoi, voir [Nyquist frequency ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/Nyquist_frequency){: new_window}.

Une comparaison utile serait d'imaginer visionner une cassette VHS sur un grand écran plat HDTV. L'image serait floue car regarder la cassette sur un lecteur en haute définition ne peut réellement rien ajouter au flux d'informations. Cela permet juste de rendre le format compatible avec le meilleur lecteur. Il en va de même pour le sur-échantillonnage audio.

#### Sous-échantillonnage
{: #downsampling}

Le *sous-échantillonnage* (downsampling) diminue la fréquence d'échantillonnage audio. Il produit un signal audio approximatif qui aurait été obtenu en échantillonnant les données audio à une fréquence moins élevée. Le sous-échantillonnage ne supprime aucune information du signal audio, mais il réduit la taille des données audio.

Sous-échantillonner vos données audio peut s'avérer efficace dans certains cas. Par exemple, si la fréquence d'échantillonnage de vos données audio est supérieure à 8 kHz *et* qu'un examen spectrographique ne révèle aucun contenu de fréquence supérieure à 4 kHz, envisagez un sous-échantillonnage des données audio à 8 kHz.

#### Informations complémentaires
{: #frequencyMore}

-   Pour plus d'informations sur la fréquence audio, voir [https://en.wikipedia.org/wiki/Audio_frequency ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/Audio_frequency){: new_window}.
-   Pour plus d'informations sur le sur-échantillonnage, voir [https://en.wikipedia.org/wiki/Upsampling ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/Upsampling){: new_window}.
-   Pour plus d'informations sur le sous-échantillonnage, voir [https://en.wikipedia.org/wiki/Downsampling ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/Downsampling_%28signal_processing%29){: new_window}.

## Formats audio pris en charge
{: #formats}

Le tableau 1 présente un récapitulatif des formats audio pris en charge par le service.

-   Le *format audio et la compression* identifient chaque format et indiquent la compression compatible avec. Vous pouvez envoyer un maximum de 100 Mo de données audio au service avec une seule demande WebSocket ou HTTP synchrone. En utilisant un format prenant en charge la compression, vous pouvez réduire la taille de vos données audio pour maximiser la quantité de données que vous pouvez transmettre au service. Pour plus d'informations, voir [Limites et compression de données](#limits).
-   La *spécification content-type* indique si vous devez utiliser l'en-tête `Content-Type` ou un paramètre équivalent pour spécifier le format (type MIME) des données audio que vous envoyez au service. Vous pouvez indiquer le format audio pour n'importe quelle demande, mais ce n'est pas toujours nécessaire :
    -   Pour la plupart des formats, le type de contenu est facultatif. Vous pouvez omettre le type de contenu ou indiquer `application/octet-stream` pour que le service détecte automatiquement le format.
    -   Pour d'autres formats, le type de contenu est obligatoire. Ces formats ne fournissent pas les informations, telles que la fréquence d'échantillonnage, dont le service a besoin pour détecter automatiquement le format.
-   Les colonnes finales identifient d'autres *paramètres obligatoires* et *paramètres facultatifs* pour chaque format. Les sections suivantes délivrent plus d'informations sur ces paramètres.

<table>
  <caption>Tableau 1. Récapitulatif des formats audio pris en charge</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom">
      Format audio<br>et compression
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Spécification<br/>content-type
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Paramètres<br/>obligatoires
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Paramètres<br/>facultatifs
    </th>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/alaw](#alaw)<br/>Avec perte
    </td>
    <td style="text-align:center">
      Obligatoire
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      Aucun
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/basic](#basic)<br/>Avec perte
    </td>
    <td style="text-align:center">
      Obligatoire
    </td>
    <td style="text-align:center">
      Aucun
    </td>
    <td style="text-align:center">
      Aucun
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/flac](#flac)<br/>Sans perte
    </td>
    <td style="text-align:center">
      Facultatif
    </td>
    <td style="text-align:center">
      Aucun
    </td>
    <td style="text-align:center">
      Aucun
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/g729](#g729)<br/>Avec perte
    </td>
    <td style="text-align:center">
      Facultatif
    </td>
    <td style="text-align:center">
      Aucun
    </td>
    <td style="text-align:center">
      Aucun
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/l16](#l16)<br/>Aucune
    </td>
    <td style="text-align:center">
      Obligatoire
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      <code>channels={integer}</code><br/>
      <code>endianness=big-endian</code><br/>
      <code>endianness=little-endian</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/mp3](#mp3)<br/>
      [audio/mpeg](#mp3)<br/>Avec perte
    </td>
    <td style="text-align:center">
      Facultatif
    </td>
    <td style="text-align:center">
      Aucun
    </td>
    <td style="text-align:center">
      Aucun
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/mulaw](#mulaw)<br/>Avec perte
    </td>
    <td style="text-align:center">
      Obligatoire
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      Aucun
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/ogg](#ogg)<br/>Avec perte
    </td>
    <td style="text-align:center">
      Facultatif
    </td>
    <td style="text-align:center">
      Aucun
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/wav](#wav)<br/>Aucune, sans <br/>ou avec perte
    </td>
    <td style="text-align:center">
      Facultatif
    </td>
    <td style="text-align:center">
      Aucun
    </td>
    <td style="text-align:center">
      Aucun
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/webm](#webm)<br/>Avec perte
    </td>
    <td style="text-align:center">
      Facultatif
    </td>
    <td style="text-align:center">
      Aucun
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
</table>

Lorsque vous utilisez la commande `curl` pour effectuer une demande de reconnaissance vocale avec les interfaces HTTP, vous devez spécifier le format audio avec l'en-tête `Content-Type`, indiquer `"Content-Type: application/octet-stream"` ou `"Content-Type:"`. Si vous omettez complètement l'en-tête, la commande `curl` utilise la valeur par défaut `application/x-www-form-urlencoded`.
{: important}

### Format audio/alaw
{: #alaw}

*A-law* (`audio/alaw`) est un format audio avec perte mono-canal. Il utilise un algorithme similaire à l'algorithme u-law appliqué par les formats `audio/basic` et `audio/mulaw`, bien que l'algorithme A-law produise des caractéristiques de signal différentes. Lorsque vous utilisez ce format, le service nécessite un paramètre supplémentaire sur la spécification du format.

<table>
  <caption>Tableau 2. Paramètre pour le format `audio/alaw`</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Paramètre
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Description
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Obligatoire</em>
    </td>
    <td>
      Nombre entier indiquant la fréquence d'échantillonnage de capture
      audio. Par exemple, indiquez le paramètre suivant pour les données
      audio capturées à 8 kHz :<br/><br/>
      <code>audio/alaw;rate=8000</code>
    </td>
  </tr>
</table>

Pour plus d'informations, voir [en.wikipedia.org/wiki/A-law_algorithm ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/A-law_algorithm){: new_window}.

### Format audio/basic
{: #basic}

*Basic audio* (`audio/basic`) est un format audio avec perte mono-canal codé en utilisant des données u-law (ou mu-law) 8 bits échantillonnées à 8 kHz. Ce format fournit un plus petit dénominateur commun pour indiquer le type de support audio. Le service prend en charge l'utilisation de fichiers au format `audio/basic` uniquement avec les modèles à bande étroite.

Pour plus d'informations, voir le document de l'Internet Engineering Task Force (IETF) [Request for Comment (RFC) 2046 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://tools.ietf.org/html/rfc2046){: new_window} et [iana.org/assignments/media-types/audio/basic ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://www.iana.org/assignments/media-types/audio/basic){: new_window}.

### Format audio/flac
{: #flac}

*Free Lossless Audio Codec (FLAC)* (`audio/flac`) est un format audio sans perte. Pour plus d'informations, voir [en.wikipedia.org/wiki/FLAC ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/FLAC){: new_window}.

### Format audio/g729
{: #g729}

*G.729* (`audio/g729`) est un format audio avec perte qui prend en charge les données codées à 8 kHz. Le service prend en charge uniquement G.729 Annex D, et non pas Annex J, ainsi que l'utilisation des fichiers au format `audio/g729` uniquement avec les modèles à bande étroite. Pour plus d'informations, voir [en.wikipedia.org/wiki/G.729 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/G.729){: new_window}.

### Format audio/l16
{: #l16}

*PCM (Pulse-Code Modulation) 16 bits linéaire* (`audio/l16`) est un format audio non compressé. Utilisez ce format pour transmettre un fichier PCM brut. Le format audio PCM linéaire peut également être transporté dans un fichier conteneur WAV (Waveform Audio File Format). Lorsque vous utilisez le format `audio/l16`, le service accepte des paramètres supplémentaires obligatoires et facultatifs sur la spécification de format.

<table>
  <caption>Tableau 3. Paramètres pour le format `audio/l16`</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Paramètre
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Description
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Obligatoire</em>
    </td>
    <td>
      Nombre entier indiquant la fréquence d'échantillonnage de capture
      audio. Par exemple, indiquez le paramètre suivant pour les données
      audio capturées à 16 kHz :<br/><br/>
      <code>audio/l16;rate=16000</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>channels</code><br/><em>Facultatif</em>
    </td>
    <td>
      Par défaut, le service traite l'audio comme s'il n'y avait qu'un seul canal.
      <em>Si l'audio a plus d'un canal,</em> vous devez spécifier
      un nombre entier identifiant le nombre de canaux. Par exemple,
      indiquez le paramètre suivant pour des données audio à deux canaux
      capturées à 16 kHz :<br/><br/>
      <code>audio/l16;rate=16000;channels=2</code><br/><br/>
      Le service accepte jusqu'à 16 canaux maximum. Il règle le mixage
      audio à un canal lors du transcodage.
    </td>
  </tr>
  <tr>
    <td>
      <code>endianness</code><br/><em>Facultatif</em>
    </td>
    <td>
      Par défaut, le service détecte automatiquement l'ordre d'octets (endianness)
      des données audio entrantes. Mais cette détection peut parfois échouer et
      supprimer la connexion pour un fichier audio court au format `audio/l16`. Indiquer
      l'ordre d'octets désactive la détection automatique. Spécifiez
      <code>big-endian</code> ou <code>little-endian</code>. Par
      exemple, spécifiez le paramètre suivant pour des données audio
      capturées à 16 kHz au format little-endian :<br/><br/>
      <code>audio/l16;rate=16000;endianness=little-endian</code><br/><br/>
      La section 5.1 du document
      <a target="_blank" href="https://tools.ietf.org/html/rfc2045#section-5.1">Request for Comment (RFC) 2045 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")</a>
      spécifie le format big-endian pour les données <code>audio/l16</code>, mais
      nombreuses sont les personnes qui utilisent le format little-endian.
    </td>
  </tr>
</table>

Pour plus d'informations, voir l'IETF [Request for Comment (RFC) 2586 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://tools.ietf.org/html/rfc2586){: new_window} et [en.wikipedia.org/wiki/Pulse-code_modulation ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/Pulse-code_modulation){: new_window}.

### Formats audio/mp3 et audio/mpeg
{: #mp3}

*MP3* (`audio/mp3`) ou *MPEG (Motion Picture Experts Group)* (`audio/mpeg`) est un format audio avec perte. (MP3 et MPEG désignent le même format.) Pour plus d'informations, voir [en.wikipedia.org/wiki/MP3 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/MP3){: new_window}.

### Format audio/mulaw
{: #mulaw}

*Mu-law* (`audio/mulaw`) est un format audio avec perte mono-canal. Les données sont codées en utilisant l'algorithme u-law (ou mu-law). Le format `audio/basic` est un format équivalent qui est toujours échantillonné à 8 kHz. Lorsque vous utilisez ce format, le service nécessite un paramètre supplémentaire sur la spécification de format.

<table>
  <caption>Tableau 4. Paramètre pour le format `audio/mulaw`</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Paramètre
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Description
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Obligatoire</em>
    </td>
    <td>
      Nombre entier indiquant la fréquence d'échantillonnage de capture
      audio. Par exemple, spécifiez le paramètre suivant pour des données
      audio capturées à 8 kHz :<br/><br/>
      <code>audio/mulaw;rate=8000</code>
    </td>
  </tr>
</table>

Pour plus d'informations, voir [en.wikipedia.org/wiki/M-law_algorithm ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/M-law_algorithm){: new_window}.

### Format audio/ogg
{: #ogg}

*Ogg* (`audio/ogg`) est un format conteneur ouvert géré par Xiph.org Foundation ([xiph.org/ogg ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.xiph.org/ogg){: new_window}). Vous pouvez utiliser des flux audio compressés avec les codecs avec perte suivants :

-   *Opus* (`audio/ogg;codecs=opus`). Pour plus d'informations, voir [opus-codec.org ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.opus-codec.org/){: new_window} et [en.wikipedia.org/wiki/Opus (audio format) ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/Opus){: new_window}. Sur la page *Opus (audio format)*, examinez particulièrement la section *Containers*.
-   *Vorbis* (`audio/ogg;codecs=vorbis`). Pour plus d'informations, voir [xiph.org/vorbis ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://xiph.org/vorbis/){: new_window} et [en.wikipedia.org/wiki/Vorbis ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/Vorbis){: new_window}.

Si vous omettez le codec, le service le détecte automatiquement dans l'entrée audio. Opus est le codec privilégié ; il est normalisé par l'Internet Engineering Task Force (IETF) sous [Request for Comment (RFC) 6716 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://tools.ietf.org/html/rfc6716){: new_window}.

### Format audio/wav
{: #wav}

*Waveform Audio File Format (WAV)* (`audio/wav`) est un format conteneur souvent utilisé pour les flux audio non compressés, mais il peut également contenir des données audio compressées. Le service prend en charge le format audio WAV utilisant n'importe quel codage. Il accepte le format audio WAV avec une limite maximale de 9 canaux (en raison d'une limitation de l'outil FFmpeg).

Pour plus d'informations sur le format WAV, voir [en.wikipedia.org/wiki/WAV ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/WAV){: new_window}. Pour plus d'informations sur la réduction de la taille des données audio WAV en les convertissant avec le codec Opus, voir [Conversion au format audio/ogg avec le codec Opus](#conversionOgg).

### Format audio/webm
{: #webm}

*Web Media (WebM)* (`audio/webm`) est un format conteneur ouvert géré par le projet WebM ([webmproject.org ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.webmproject.org/){: new_window}). Vous pouvez utiliser des flux audio compressés avec les codecs avec perte suivants :

-   *Opus* (`audio/webm;codecs=opus`). Pour plus d'informations, voir [opus-codec.org ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.opus-codec.org/){: new_window} et [en.wikipedia.org/wiki/Opus (audio format) ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/Opus){: new_window}. Sur la page *Opus (audio format)*, examinez particulièrement la section *Containers*.
-   *Vorbis* (`audio/webm;codecs=vorbis`). Pour plus d'informations, voir [xiph.org/vorbis ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://xiph.org/vorbis/){: new_window} et [en.wikipedia.org/wiki/Vorbis ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://en.wikipedia.org/wiki/Vorbis){: new_window}.

Si vous omettez le codec, le service le détecte automatiquement dans l'entrée audio. 

Pour obtenir le code JavaScript qui montre comment faire des captures audio à partir d'un micro dans un navigateur Chrome et coder les données en flux de données WebM, voir [jsbin.com/hedujihuqo/edit?js,console ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://jsbin.com/hedujihuqo/edit?js,console){: new_window}. Le code ne soumet pas les captures audio au service.
{: tip}

## Limites et compression de données
{: #limits}

Le service accepte un maximum de 100 Mo de données audio pour la transcription avec une demande WebSocket ou HTTP synchrone. Lorsque vous reconnaissez des flux audio continus ou des gros fichiers, procédez comme suit pour garantir que vos données audio ne dépassent pas la taille limite de 100 Mo :

-   Utilisez une fréquence d'échantillonnage ne dépassant pas 16 kHz (pour les modèles à large bande) ou 8 kHz (pour les modèles à bande étroite), et utilisez 16 bits par échantillon. Le service convertit les données audio enregistrées à une fréquence d'échantillonnage plus élevée que le modèle cible (16 kHz ou 8 kHz) à la fréquence du modèle. Par conséquent, des fréquences plus élevées ne contribuent pas à une reconnaissance plus précise, mais elles augmentent la taille du flux audio.
-   Codez vos données audio dans un format avec compression de données. En codant vos données de manière plus efficace, vous pouvez envoyer beaucoup plus de données audio sans dépasser la taille limite de 100 Mo. Des formats audio, tels que `audio/ogg` et `audio/mp3` réduisent considérablement la taille de votre flux audio. Vous pouvez utiliser ces formats pour envoyer de plus grandes quantités de données audio avec une seule demande.

Si vous compressez vos données audio trop fortement avec le format `audio/ogg`, vous pouvez perdre en précision lors de la reconnaissance. Il est préférable de conserver un débit binaire d'au moins 24 kbit/s pour vos données audio.

### Comparaison des tailles audio approximatives
{: #compareSizes}

Envisagez une taille approximative d'un flux de données correspondant à 2 heures de transmission vocale continue échantillonnée à 16 kHz et à 16 bits par échantillon :

-   Si les données sont codées au format `audio/wav`, les deux heures de flux ont une taille de 230 Mo, bien au-delà de la limite de 100 Mo du service.
-   Si les données sont codées au format `audio/ogg`, les deux heures de flux ont une taille de seulement 23 Mo, bien au-dessous de la limite du service.

Le tableau suivant indique les valeurs approximatives de durée maximale des données audio pouvant être envoyées pour la reconnaissance vocale avec une demande WebSocket ou HTTP synchrone en différents formats. La durée tient compte de la limite du service établie à 100 Mo. Les valeurs réelles peuvent varier en fonction de la complexité audio et du taux de compression obtenu.

<table style="width:75%">
  <caption>Tableau 5. Durée maximale audio pour différents formats</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 50%">
      Format audio
    </th>
    <th style="text-align:center; vertical-align:bottom; width 50%">
      Durée audio maximale (approximative)
    </th>
  </tr>
  <tr>
    <td><code>audio/wav</code></td>
    <td style="text-align:center">55 minutes</td>
  </tr>
  <tr>
    <td><code>audio/flac</code></td>
    <td style="text-align:center">1 heure 40 minutes</td>
  </tr>
  <tr>
    <td><code>audio/mp3</code></td>
    <td style="text-align:center">3 heures 20 minutes</td>
  </tr>
  <tr>
    <td><code>audio/ogg</code></td>
    <td style="text-align:center">8 heures 40 minutes</td>
  </tr>
</table>

Les formats `audio/ogg;codecs=opus` et `audio/webm;codecs=opus` sont en principe équivalents, et leurs tailles sont quasiment identiques. Ils utilisent le même codec en interne, seul le format conteneur est différent.

## Conversion audio
{: #conversion}

Vous pouvez recourir à divers outils pour convertir vos données audio dans un autre format. Les outils peuvent s'avérer utiles lorsque vos données audio sont dans un format qui n'est pas pris en charge par le service ou dans un format non compressé ou sans perte. Dans ce dernier cas, vous pouvez convertir les données dans un format avec perte pour en réduire la taille.

Les outils logiciels gratuits suivants sont disponibles pour convertir vos données audio d'un format à un autre :

-   Sound eXchange (SoX) ([sox.sourceforge.net ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://sox.sourceforge.net){: new_window})
-   FFmpeg ([ffmpeg.org ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ffmpeg.org){: new_window})
-   Audacity&reg; ([audacityteam.org ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://www.audacityteam.org/){: new_window})
-   Pour le format Ogg avec le codec Opus, **opus-tools** ([opus-codec.org/downloads/ ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://opus-codec.org/downloads/){: new_window})

Ces outils offrent une prise en charge multiplateforme pour plusieurs formats audio. De plus, vous pouvez utiliser de nombreux outils pour lire vos données audio. Veillez à respecter les lois sur les droits d'auteur applicables en utilisant ces outils.

### Conversion en format audio/ogg avec le codec Opus
{: #conversionOgg}

Les outils [**opus-tools** ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://opus-codec.org/downloads/){: new_window} comprennent trois utilitaires de ligne de commande pour utiliser des données audio au format Ogg dans le codec Opus :

-   L'utilitaire [**opusenc** ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusenc.html){: new_window} code les données audio à partir des formats WAV, FLAC et d'autres formats en format Ogg avec le codec Opus. La page indique comment compresser les flux audio. La compression est utile pour transmettre des données audio en temps réel au service.
-   L'utilitaire [**opusdec** ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusdec.html){: new_window} décode les données audio à partir du codec Opus en fichiers WAV PCM non compressés.
-   L'utilitaire [**opusinfo** ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusinfo.html){: new_window} fournit des informations sur les fichiers Opus ainsi qu'une vérification de leur validité.

De nombreux utilisateurs envoient des fichiers WAV pour la reconnaissance vocale. Avec la taille limite de 100 Mo du service pour les demandes WebSocket et HTTP synchrones, le format WAV réduit la quantité de données audio pouvant être reconnues avec une seule demande. L'utilisation de la commande **opusenc** pour convertir les données audio au format privilégié `audio/ogg:codecs=opus` peut considérablement augmenter le volume de données audio que vous pouvez envoyer avec une demande de reconnaissance.

Par exemple, envisagez un fichier WAV à large bande (16 kHz) non compressé (**input.wav**) utilisant 16 bits par échantillon pour un débit binaire de 256 kbit/s. La commande suivante convertit le fichier audio en fichier (**output.opus**) qui utilise le codec Opus :

```bash
opusenc input.wav output.opus
```
{: pre}

La conversion offre une compression audio multipliée par quatre et produit un fichier de sortie avec un débit binaire de 64 kbit/s. Cependant, selon les [paramètres recommandés avec Opus ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://wiki.xiph.org/Opus_Recommended_Settings){: new_window}, vous pouvez réduire sans problème le débit binaire à 24 kbit/s tout en conservant une bande complète pour la voix. Cette réduction offre une compression audio multipliée par 10. La commande suivante utilise l'option `--bitrate` pour obtenir un fichier de sortie avec un débit binaire de 24 kbit/s :

```bash
opusenc --bitrate 24 input.wav output.opus
```
{: pre}

La compression avec l'utilitaire **opusenc** est rapide. Elle s'effectue à une fréquence environ 100 fois plus rapide qu'en temps réel. Lorsqu'elle se termine, la commande imprime la sortie dans la console et fournit les détails complets sur sa durée d'exécution et les données audio obtenues.

## Astuces pour améliorer la reconnaissance vocale
{: #audioTips}

Les astuces suivantes peuvent vous aider à améliorer la qualité de la reconnaissance vocale :

-   Votre manière d'enregistrer les données audio peut faire toute la différence au niveau des résultats du service. La reconnaissance vocale peut être très sensible à la qualité de l'entrée audio. Pour obtenir la meilleure précision possible, vérifiez que la qualité de l'entrée audio est irréprochable.
    -   Utilisez un micro pour la voix (par exemple un casque) autant que possible et réglez les paramètres du micro si nécessaire. Le service est optimal si vous utilisez des micros professionnels pour capturer des données audio.
    -   Evitez d'utiliser un micro intégré dans un système. Les micros habituellement installés sur les appareils mobiles et les tablettes sont souvent inadaptés.
    -   Veillez à ce que les haut-parleurs soient situés à proximité des micros. Vous obtenez un résultat moins précis si le haut-parleur est éloigné du micro. A une distance d'environ 3 mètres, par exemple, le service a du mal à produire des résultats corrects.
-   La reconnaissance vocale est sensible aux bruits de fond et aux nuances de la voix humaine.
    -   Les bruits de moteur, les appareils en fonctionnement, les bruits de la rue et les conversations en arrière-plan peuvent nuire considérablement à la précision de la reconnaissance.
    -   Les accents régionaux et les différences de prononciation peuvent également avoir un impact négatif sur la précision.

    Si vos données audio ont ces caractéristiques, envisagez d'utiliser la personnalisation de modèle acoustique pour obtenir une meilleure reconnaissance vocale. Pour plus d'informations, voir [L'interface de personnalisation](/docs/services/speech-to-text/custom.html).
