---

copyright:
  years: 2019
lastupdated: "2019-06-04"

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

# Hochverfügbarkeit und Disaster-Recovery
{: #ha-dr}

Der {{site.data.keyword.speechtotextfull}}-Service ist an jedem {{site.data.keyword.cloud_notm}}-Standort (z. B. Dallas oder Washington, DC) hoch verfügbar. Die mögliche Disaster-Recovery für einen gesamten Standort setzt jedoch Planung und  Vorbereitung voraus.
{: shortdesc}

Sie müssen selbst dafür sorgen, dass Sie Ihre Konfiguration, Ihre Anpassung und Ihre Nutzung des Service kennen. Sie sind ebenfalls dafür verantwortlich, alle Vorbereitungen für die erneute Erstellung einer Instanz des Service an einem neuen Standort und für die Wiederherstellung Ihrer Daten an einem beliebigen Standort zu treffen.

## Hochverfügbarkeit
{: #high-availability}

Der {{site.data.keyword.speechtotextshort}}-Service unterstützt eine Hochverfügbarkeit ohne Single Point of Failure. Er erzielt die Hochverfügbarkeit automatisch und transparent durch die von {{site.data.keyword.cloud_notm}} bereitgestellte Funktion für Regionen mit mehreren Zonen (Multi-Zone Region, MZR).

{{site.data.keyword.cloud_notm}} ermöglicht an einem einzigen Standort mehrere Zonen ohne einen gemeinsamen Single Point of Failure. Darüber hinaus wird ein automatischer Lastausgleich unter den Zonen innerhalb einer Region zur Verfügung gestellt.

## Disaster-Recovery
{: #disaster-recovery}

Die Disaster-Recovery kann problematisch sein, falls an einem {{site.data.keyword.cloud_notm}}-Standort ein erheblicher Fehler auftritt, der auch einen möglichen Datenverlust einschließt. Da Regionen mit mehreren Zonen nicht standortübergreifend verfügbar sind, müssen Sie warten, bis {{site.data.keyword.IBM_notm}} einen Standort wieder in den Onlinestatus versetzt, falls die Verfügbarkeit des Standortes unterbrochen wird. Wenn der Fehler die zugrunde liegenden Datenservices beeinträchtigt, müssen Sie außerdem warten, bis {{site.data.keyword.IBM_notm}} diese Datenservices wiederhergestellt hat.

Bei einem katastrophalen Fehler ist {{site.data.keyword.IBM_notm}} möglicherweise nicht in der Lage, Daten aus Datenbanksicherungen wiederherzustellen. In diesem Fall müssen Sie Ihre Daten wiederherstellen, um die Serviceinstanz wieder in den aktuellen Zustand zu versetzen. Die Daten können Sie an demselben oder oder an einem anderen Standort wiederherstellen.

Für Ihren Disaster-Recovery-Plan müssen Sie auch alle in {{site.data.keyword.cloud_notm}} verwalteten Daten kennen, aufbewahren und auf eine Wiederherstellung vorbereiten. Hierzu gehören auch Daten aus angepassten Sprachmodellen, angepassten Akustikmodellen und asynchronen Spracherkennungsanforderungen.

Die erneute Erstellung von angepassten Modellen aus gesicherten Daten kann besonders bei angepassten Akustikmodellen erheblich Zeit in Anspruch nehmen. Die Aufbewahrung paralleler Servicekonfigurationen an mehreren Standorten eliminiert die mit der Disaster-Recovery verbundene Bearbeitungszeit.
{: note}

### Disaster-Recovery bei angepassten Sprachmodellen
{: #disaster-recovery-language}

Für angepasste Sprachmodelle müssen Sie die Modelle selbst sowie deren Korpora, Grammatiken und angepasste Wörter aufbewahren und auf eine erneute Erstellung und Wiederherstellung vorbereiten. Außerdem müssen Sie darauf vorbereitet sein, Ihre angepassten Sprachmodelle mit den zugehörigen Daten und Wörterressourcen erneut zu trainieren.

#### Angepasste Sprachmodelle sichern
{: #disaster-recovery-language-backup}

Bewahren Sie die folgenden Informationen zu Ihren angepassten Sprachmodellen auf:

-   Liste aller angepassten Sprachmodelle und ihrer Definitionen. So können Sie Informationen zu Ihren angepassten Modellen auflisten:
    -   Verwenden Sie die Methode `GET /v1/customizations`, um Informationen zu allen angepassten Modellen aufzulisten.
    -   Verwenden Sie die Methode `GET /v1/customizations/{customization_id}`, um Informationen zu einem angegebenen angepassten Modell aufzulisten.

    Weitere Informationen finden Sie im Abschnitt [Angepasste Sprachmodelle auflisten](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).
-   Kopien aller Korpustextdateien, die Sie zu Ihren angepassten Sprachmodellen hinzufügen. So können Sie Informationen zu den Korpora für Ihre angepassten Modelle auflisten:
    -   Verwenden Sie die Methode `GET /v1/customizations/{customization_id}/corpora`, um alle Korpora für ein angepasstes Modell aufzulisten.
    -   Verwenden Sie die Methode `GET /v1/customizations/{customization_id}/corpora/{corpus_name}`, um Informationen zu einem bestimmten Korpus für ein angepasstes Modell aufzulisten.

    Weitere Informationen finden Sie im Abschnitt [Korpora für angepasstes Sprachmodell auflisten](/docs/services/speech-to-text?topic=speech-to-text-manageCorpora#listCorpora).
-   Kopien aller Grammatikdateien, die Sie zu Ihren angepassten Sprachmodellen hinzufügen. So können Sie Informationen zu den Grammatiken für Ihre angepassten Modelle auflisten:
    -   Verwenden Sie die Methode `GET /v1/customizations/{customization_id}/grammars`, um Informationen zu allen Grammatiken für ein angepasstes Modell aufzulisten.
    -   Verwenden Sie die Methode `GET /v1/customizations/{customization_id}/grammars/{grammar_name}`, um Informationen zu einer bestimmten Grammatik für ein angepasstes Modell aufzulisten.

    Weitere Informationen enthält der Abschnitt [Grammatiken für ein angepasstes Sprachmodell auflisten](/docs/services/speech-to-text?topic=speech-to-text-manageGrammars#listGrammars).
-   Informationen zu allen angepassten Wörtern, die Sie direkt zu Ihren angepassten Sprachmodellen hinzufügen; hierzu gehören auch die Definitionen für die Klangähnlichkeit (sounds-like) und die optische Ausgabe (display-as). So können Sie Informationen zu den vokabularexternen Wörtern für Ihre angepassten Modelle auflisten:
    -   Verwenden Sie die Methode `GET /v1/customizations/{customization_id}/words`, um Informationen zu allen Wörtern aus einem angepassten Modell aufzulisten. Mit dem Parameter `word_type` können Sie alle Wörter aus einem Modell (Wert `all`), vom Benutzer direkt hinzugefügte Wörter (Wert `user`), aus Korpora extrahierte Wörter (Wert `corpora`) oder durch Grammatiken erkannte Wörter (Wert `grammars`) auflisten.
    -   Verwenden Sie die Methode `GET /v1/customizations/{customization_id}/words/{word_name}`, um Informationen zu einem bestimmten Wort aus einem angepassten Modell aufzulisten.

    Weitere Informationen finden Sie im Abschnitt [Wörter aus angepasstem Sprachmodell auflisten](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords).

Es hat sich bewährt, diese Informationen in einem Format aufzubewahren, das die erneute Erstellung Ihrer angepassten Sprachmodelle bei einem Fehler möglich macht. Wenn Sie die Informationen zu Ihren angepassten Modellen und ihren Daten aktiv verwalten und im Voraus die im folgenden Abschnitt aufgeführten Aufrufe vorbereiten, können Sie die Wiederherstellung so schnell wie möglich durchführen.

#### Angepasste Sprachmodelle wiederherstellen
{: #disaster-recovery-language-restore}

Falls eine Disaster-Recovery erforderlich werden sollte, können Sie Ihre angepassten Sprachmodelle und die zugehörigen Daten mithilfe der Sicherungsinformationen erneut erstellen:

1.  Verwenden Sie für die erneute Erstellung Ihrer angepassten Sprachmodelle die Methode `POST /v1/customizations`. Weitere Informationen hierzu finden Sie im Abschnitt [Angepasstes Sprachmodell erstellen](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language).
1.  Verwenden Sie zum Hinzufügen der Korpustextdateien zu Ihren angepassten Modellen die Methode `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`. Weitere Informationen finden Sie im Abschnitt [Korpus zum angepassten Sprachmodell hinzufügen](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addCorpus). 
1.  Verwenden Sie zum Hinzufügen der Grammatikdateien zu Ihren angepassten Modellen die Methode `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`. Weitere Informationen hierzu enthält der Abschnitt [Grammatik zum angepassten Sprachmodell hinzufügen](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar). 
1.  Verwenden Sie zum Hinzufügen von mehreren Wörtern zu Ihren angepassten Modellen die Methode `POST /v1/customizations/{customization_id}/words`. Verwenden Sie zum Hinzufügen einzelner Wörter zu Ihren angepassten Modellen die Methode `PUT /v1/customizations/{customization_id}/words/{word_name}`. Weitere Informationen hierzu enthält der Abschnitt [Wörter zum angepassten Sprachmodell hinzufügen](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addWords).
1.  Verwenden Sie nach dem Wiederherstellen der Korpora, Grammatiken und angepassten Wörter zum Trainieren Ihrer angepassten Modelle die Methode `POST /v1/customizations/{customization_id}/train`. Weitere Informationen hierzu enthält der Abschnitt [Angepasstes Sprachmodell trainieren](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language).

Die Methoden, die Sie zum Hinzufügen von Korpora, Grammatiken und Wörtern sowie zum Trainieren eines angepassten Sprachmodells verwenden, werden asynchron ausgeführt. Sie müssen die Anforderungen überwachen, bis sie abgeschlossen sind.

Sie können nach und nach Daten hinzufügen und Ihre angepassten Sprachmodelle trainieren, statt alle Daten vor dem Trainieren der Modelle hinzuzufügen. Beispielsweise können Sie Ihre Korpora hinzufügen und anschließend Ihre Modelle trainieren, bevor Sie Grammatiken und einzelne Wörter hinzufügen. Sie können auch Schritt für Schritt einzelne Korpustextdateien, Grammatikdateien und Gruppen von angepassten Wörtern hinzufügen und Ihre Modelle damit trainieren.

### Disaster-Recovery bei angepassten Akustikmodellen
{: #disaster-recovery-acoustic}

Für angepasste Akustikmodelle müssen Sie die Modelle selbst sowie die zugehörigen Audioressourcen aufbewahren und auf eine erneute Erstellung und Wiederherstellung vorbereiten. Außerdem müssen Sie darauf vorbereitet sein, Ihre angepassten Akustikmodelle mit den zugehörigen Audioressourcen erneut zu trainieren.

#### Angepasste Akustikmodelle sichern
{: #disaster-recovery-acoustic-backup}

Bewahren Sie die folgenden Informationen zu Ihren angepassten Akustikmodellen auf:

-   Liste aller angepassten Akustikmodelle und ihrer Definitionen. So können Sie Informationen zu Ihren angepassten Modellen auflisten:
    -   Verwenden Sie die Methode `GET /v1/acoustic_customizations`, um Informationen zu allen angepassten Modellen aufzulisten.
    -   Verwenden Sie die Methode `GET /v1/acoustic_customizations/{customization_id}`, um Informationen zu einem angegebenen angepassten Modell aufzulisten.

    Weitere Informationen finden Sie im Abschnitt [Angepasste Akustikmodelle auflisten](/docs/services/speech-to-text?topic=speech-to-text-manageAcousticModels#listModels-acoustic).
-   Kopien aller Audioressourcen (sowohl einzelne Audiodateien als auch Archivdateien), die Sie zu Ihren angepassten Akustikmodellen hinzufügen. So können Sie Informationen zu den Autioressourcen für Ihre angepassten Modelle auflisten:
    -   Verwenden Sie die Methode `GET /v1/acoustic_customizations/{customization_id}/audio`, um Informationen zu allen Audioressourcen für ein angepasstes Modell aufzulisten.
    -   Verwenden Sie die Methode `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`, um Informationen zu einer bestimmten Audioressource für ein angepasstes Modell aufzulisten.

    Weitere Informationen enthält der Abschnitt [Audioressourcen für ein angepasstes Akustikmodell auflisten](/docs/services/speech-to-text?topic=speech-to-text-manageAudio#listAudio).

Es hat sich bewährt, diese Informationen in einem Format aufzubewahren, das die erneute Erstellung Ihrer angepassten Akustikmodelle bei einem Fehler möglich macht. Wenn Sie die Informationen zu Ihren angepassten Modellen und ihren Audioressourcen aktiv verwalten und im Voraus die im folgenden Abschnitt aufgeführten Aufrufe vorbereiten, können Sie die Wiederherstellung so schnell wie möglich durchführen.

#### Angepasste Akustikmodelle wiederherstellen
{: #disaster-recovery-acoustic-restore}

Falls eine Disaster-Recovery erforderlich werden sollte, können Sie Ihre angepassten Akustikmodelle und die zugehörigen Daten mithilfe der Sicherungsinformationen erneut erstellen:

1.  Verwenden Sie für die erneute Erstellung Ihrer angepassten Akustikmodelle die Methode `POST /v1/acoustic_customizations`. Weitere Informationen hierzu finden Sie im Abschnitt [Angepasstes Akustikmodell erstellen](/docs/services/speech-to-text?topic=speech-to-text-acoustic#createModel-acoustic).
1.  Verwenden Sie zum Hinzufügen der Audioressourcen zu Ihren angepassten Modellen die Methode `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`. Weitere Informationen hierzu enthält der Abschnitt [Audiodaten zum angepassten Akustikmodell hinzufügen](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio).
1.  Verwenden Sie nach dem Wiederherstellen der Audioressourcen die Methode `POST /v1/acoustic_customizations/{customization_id}/train`. Weitere Informationen finden Sie unter [Angepasstes Akustikmodell trainieren](/docs/services/speech-to-text?topic=speech-to-text-acoustic#trainModel-acoustic).

Die Methoden, die Sie zum Hinzufügen von Audioressourcen und zum Trainieren eines angepassten Akustikmodells verwenden, werden asynchron ausgeführt. Sie müssen die Anforderungen überwachen, bis sie abgeschlossen sind.

Sie können nach und nach Audioressourcen hinzufügen und Ihre angepassten Akustikmodelle trainieren, statt alle Daten vor dem Trainieren der Modelle hinzuzufügen. Beispielsweise können Sie Ihre Audioressourcen einzeln nacheinander oder in Gruppen hinzufügen und Ihre Modelle mit Teilmengen der Audioressourcen statt gleichzeitig mit allen Audiodaten trainieren.

### Disaster-Recovery bei asynchronen Spracherkennungsjobs
{: #disaster-recovery-async}

Für die Spracherkennung mit der asynchronen HTTP-Schnittstelle müssen Sie die folgenden Informationen aufbewahren:

-   Alle Callback-URLs, die Sie zur Verwendung mit der asynchronen Schnittstelle in die Whitelist aufgenommen haben. Falls ein Fehler auftritt, müssen Sie möglicherweise die URLs mit der Methode `POST /v1/register_callback` erneut registrieren. Die Methode gibt eine entsprechende Antwort zurück, wenn eine URL bereits in eine Whitelist aufgenommen wurde.
-   Kopien der Audiodateien, die Sie zur Spracherkennung an die asynchrone Schnittstelle übergeben. Falls ein Fehler auftritt, bevor Sie die Ergebnisse eines abgeschlossenen asynchronen Jobs empfangen oder abgerufen haben, müssen Sie die Audiodateien mit der Methode `POST /v1/recognitions` erneut übergeben, nachdem der Service wiederhergestellt worden ist. Sobald Sie die Ergebnisse eines abgeschlossenen asynchronen Jobs erhalten haben, müssen Sie die Audiodateien nicht mehr aufbewahren.

Weitere Informationen enthält der Abschnitt [Asynchrone HTTP-Schnittstelle](/docs/services/speech-to-text?topic=speech-to-text-async). Wie bei den Sicherungsdaten für angepasste Modelle können Sie diese Informationen aktiv verwalten und sich so im Voraus auf die erneute Ausgabe der erforderlichen Anforderungen vorbereiten.
