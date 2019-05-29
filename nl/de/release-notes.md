---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-11"

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

# Releaseinformationen
{: #release-notes}

In den folgenden Abschnitten sind die neuen Funktionen und die Änderungen dokumentiert, die bei jedem Release und jeder Aktualisierung des {{site.data.keyword.speechtotextfull}}-Service eingeführt wurden. Die Informationen enthalten alle bekannten Einschränkungen. Sofern nicht anders angegeben, sind alle Änderungen mit früheren Releases kompatibel sowie automatisch und transparent für alle neuen und vorhandenen Anwendungen verfügbar.
{: shortdesc}

## Bekannte Einschränkungen
{: #limitations}

Derzeit sind keine Einschränkungen bekannt.

## 3. April 2019
{: #April2019}

Angepasste Akustikmodelle akzeptieren jetzt Audiodaten für maximal 200 Stunden. Der bisherige Maximalwert waren Audiodaten für 100 Stunden.

## 21. März 2019
{: #March2019d}

Benutzer können nun ausschließlich Informationen zu Serviceberechtigungsnachweisen für die Rolle anzeigen, die ihrem {{site.data.keyword.cloud_notm}}-Konto zugeordnet ist. Falls Ihnen beispielsweise die Rolle `Leseberechtigter` zugeordnet ist, sind Serviceberechtigungsnachweise der Ebene `Schreibberechtigter` oder einer höheren Ebene für Sie nicht mehr sichtbar.

Diese Änderung betrifft nicht den API-Zugriff für Benutzer oder Anwendungen mit bestehenden Serviceberechtigungsnachweisen. Sie wirkt sich lediglich auf das Anzeigen von Berechtigungsnachweisen in {{site.data.keyword.cloud_notm}} aus. 

Weitere Informationen zu Serviceschlüsseln und Benutzerrollen enthält der Abschnitt [Service-API-Schlüssel von IAM](/docs/services/watson?topic=watson-api-key-bp#api-key-bp). 

## 15. März 2019
{: #March2019c}

Der Service unterstützt jetzt Audio im Format A-law (`audio/alaw`). Weitere Informationen finden Sie im Abschnitt [Format 'audio/alaw'](/docs/services/speech-to-text/audio-formats.html#alaw).

## Ältere Releases 
{: #older}

-   [11. März 2019](#March2019b)
-   [4. März 2019](#March2019)
-   [28. Januar 2019](#January2019)
-   [20. Dezember 2018](#December2018b)
-   [13. Dezember 2018](#December2018a)
-   [12. November 2018](#November2018b)
-   [7. November 2018](#November2018a)
-   [30. Oktober 2018](#October2018b)
-   [9. Oktober 2018](#October2018a)
-   [10. September 2018](#September2018b)
-   [7. September 2018](#September2018a)
-   [8. August 2018](#August2018)
-   [13. Juli 2018](#July2018)
-   [12. Juni 2018](#June2018)
-   [15. Mai 2018](#May2018)
-   [26. März 2018](#March2018b)
-   [1. März 2018](#March2018a)
-   [1. Februar 2018](#February2018)
-   [14. Dezember 2017](#December2017)
-   [2. Oktober 2017](#October2017)
-   [14. Juli 2017](#July2017b)
-   [1. Juli 2017](#July2017a)
-   [22. Mai 2017](#May2017)
-   [10. April 2017](#April2017)
-   [8. März 2017](#March2017)
-   [1. Dezember 2016](#December2016)
-   [22. September 2016](#September2016)
-   [30. Juni 2016](#June2016b)
-   [23. Juni 2016](#June2016a)
-   [10. März 2016](#March2016)
-   [19. Januar 2016](#January2016)
-   [17. Dezember 2015](#December2015)
-   [21. September 2015](#September2015)
-   [1. Juli 2015](#July2015)

### 11. März 2019
{: #March2019b}

-   Für den Parameter `max_alternatives` akzeptiert der Service wieder den Wert `0`. Wenn Sie `0` angeben, verwendet der Service automatisch den Standardwert `1`. Eine Änderung für die Aktualisierung des Service vom 4. März führte dazu, dass für den Wert `0` ein Fehler zurückgegeben wurde. (Der Service gibt einen Fehler zurück, wenn Sie einen negativen Wert angeben.)
-   Für den Parameter `word_alternatives_threshold` akzeptiert der Service wieder den Wert `0`. Eine Änderung für die Aktualisierung des Service vom 4. März führte dazu, dass für den Wert `0` ein Fehler zurückgegeben wurde. (Der Service gibt einen Fehler zurück, wenn Sie einen negativen Wert angeben.)
-   Der Service gibt jetzt alle Konfidenzwerte mit einer maximalen Genauigkeit von zwei Dezimalstellen zurück. Dazu gehören auch Konfidenzwerte für Aufzeichnungen, Wortkonfidenz, Wortalternativen, Schlüsselwortergebnisse und Sprecherbezeichnungen.

### 4. März 2019
{: #March2019}

Die folgenden Schmalbandsprachmodelle wurden aktualisiert, um die Spracherkennung zu verbessern:

-   `es-ES_NarrowbandModel`
-   `fr-FR_NarrowbandModel`
-   `pt-BR_NarrowbandModel`

Der Service verwendet standardmäßig für alle Spracherkennungsanforderungen automatisch die aktualisierten Modelle. Wenn Sie über benutzerdefinierte Sprachmodelle verfügen oder über Akustikmodelle, die auf den Modellen basieren, müssen Sie Ihre vorhandenen angepassten Modelle mit den folgenden Methoden aktualisieren, um von den Vorteilen der Aktualisierungen zu profitieren:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Weitere Informationen finden Sie im Abschnitt [Upgrade für angepasste Modelle durchführen](/docs/services/speech-to-text/custom-upgrade.html).

### 28. Januar 2019
{: #January2019}

Die WebSocket-Schnittstelle unterstützt jetzt die tokenbasierte Authentifizierung mit Identity and Access Management (IAM) mit browserbasiertem JavaScript-Code. Die bisherige Einschränkung auf das Gegenteil wurde behoben. Gehen Sie wie folgt vor, um eine authentifizierte Verbindung mit der WebSocket-Methode `/v1/recognize` herzustellen:

-   Wenn Sie die IAM-Authentifizierung verwenden, geben Sie den Abfrageparameter `access_token` an.
-   Wenn Sie Cloud Foundry-Serviceberechtigungsnachweise verwenden, geben Sie den Abfrageparameter `watson-token` an.

Weitere Informationen finden Sie im Abschnitt [Verbindung öffnen](/docs/services/speech-to-text/websockets.html#WSopen).

### 20. Dezember 2018
{: #December2018b}

Die Schnittstelle für Grammatik ist an allen Standorten seit dem 8. Januar 2019 voll funktionsfähig.
{: note}

-   Der Service unterstützt jetzt Grammatiken für die Spracherkennung. Grammatiken sind als Betafunktion für alle Sprachen verfügbar, die angepasste Sprachmodelle unterstützen. Sie können Grammatiken zu einem angepassten Sprachmodell hinzufügen, um die Gruppe der Ausdrücke zu beschränken, die der Service in Audiodaten erkennen kann. Grammatiken können im Format 'Augmented Backus-Naur Form (ABNF)' oder im Format 'XML Form' definiert werden.

    Die folgenden vier Methoden zum Arbeiten mit Grammatiken stehen zur Verfügung:
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` fügt eine Grammatikdatei zu einem angepassten Sprachmodell hinzu.
    -   `GET /v1/customizations/{customization_id}/grammars ` listet Informationen zu allen Grammatiken für ein angepasstes Modell auf.
    -   `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` gibt Informationen zu einer bestimmten Grammatik für ein angepasstes Modell zurück.
    -   `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` entfernt eine vorhandene Grammatik aus einem angepassten Modell.

    Sie können eine Grammatik für die Spracherkennung mit den WebSocket- und HTTP-Schnittstellen verwenden. Mit den Parametern `language_customization_id` und `grammar_name` können Sie das angepasste Modell und die Grammatik angeben, die Sie verwenden möchten. Derzeit kann in einer Spracherkennungsanforderung nur eine einzige Grammatik verwendet werden.

    Weitere Informationen zu Grammatiken finden Sie in der folgenden Dokumentation:
    -   [Grammatiken mit angepassten Sprachmodellen verwenden](/docs/services/speech-to-text/grammar.html)
    -   [Wissenswertes über Grammatiken](/docs/services/speech-to-text/grammar-understand.html)
    -   [Grammatik zu einem angepassten Sprachmodell hinzufügen](/docs/services/speech-to-text/grammar-add.html)
    -   [Grammatik für Spracherkennung verwenden](/docs/services/speech-to-text/grammar-use.html)
    -   [Grammatiken verwalten](/docs/services/speech-to-text/grammar-manage.html)
    -   [Beispiele für Grammatiken](/docs/services/speech-to-text/grammar-examples.html)

    Informationen zu  allen Methoden der Schnittstelle finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Eine neue Funktion zum Schwärzen von Zahlen ermöglicht jetzt die Maskierung von Zahlen, die aus mindestens drei aufeinanderfolgenden Ziffern bestehen. Durch Schwärzen sollen sensible personenbezogene Daten (z. B. Kreditkartennummern) in Aufzeichnungen unkenntlich gemacht werden. Sie können diese Funktion aktivieren, indem Sie in einer Erkennungsanforderung den Parameter `redaction` auf `true` setzen. Diese Funktion ist als Betafunktionalität nur für amerikanisches Englisch, Japanisch und Koreanisch verfügbar. Weitere Informationen finden Sie im Abschnitt [Zahlenschwärzung](/docs/services/speech-to-text/output.html#redaction).
-   Die folgenden neuen Sprachmodelle für Deutsch und Französisch sind jetzt mit dem Service verfügbar:
    -   `de-DE_NarrowbandModel`
    -   `fr-FR_NarrowbandModel`

    Beide neuen Modelle unterstützen die Sprachmodellanpassung (allgemein verfügbar) und die Akustikmodellanpassung (Betaversion). Weitere Informationen finden Sie im Abschnitt [Sprachunterstützung für Anpassung](/docs/services/speech-to-text/custom.html#languageSupport).
-   Ein neues Modell für amerikanisches Englisch, `en-US_ShortForm_NarrowbandModel`, ist jetzt verfügbar. Das neue Modell ist für die Verwendung in Lösungen für Interactive-Voice-Response und für automatisierte Kundenunterstützung vorgesehen. Das Modell unterstützt die Sprachmodellanpassung (allgemein verfügbar) und die Akustikmodellanpassung (Betaversion).
-   Die folgenden Sprachmodelle wurde aktualisiert, um die Spracherkennung zu verbessern:
    -   `en-GB_NarrowbandModel`
    -   `es-ES_NarrowbandModel`

    Der Service verwendet standardmäßig für alle Spracherkennungsanforderungen automatisch die aktualisierten Modelle. Wenn Sie über benutzerdefinierte Sprachmodelle verfügen oder über Akustikmodelle, die auf den Modellen basieren, müssen Sie Ihre vorhandenen angepassten Modelle mit den folgenden Methoden aktualisieren, um von den Vorteilen der Aktualisierungen zu profitieren:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Weitere Informationen finden Sie im Abschnitt [Upgrade für angepasste Modelle durchführen](/docs/services/speech-to-text/custom-upgrade.html).
-   Der Service unterstützt jetzt das Audioformat G.729 (`audio/g729`). Für schmalbandige Audiodaten unterstützt der Service nur G.729 Annex D. Weitere Informationen zu unterstützten Audioformaten finden Sie im Abschnitt [Audioformate](/docs/services/speech-to-text/audio-formats.html).
-   Die Funktion für Sprecherbezeichnungen ist jetzt für das schmalbandige Modell für britisches Englisch (`en-GB_NarrowbandModel`) verfügbar. Diese Funktion ist als Betafunktionalität für alle unterstützten Sprachen verfügbar. Weitere Informationen finden Sie im Abschnitt [Sprecherbezeichnungen](/docs/services/speech-to-text/output.html#speaker_labels).
-   Die maximale Menge an Audiodaten, die Sie zu einem angepassten Akustikmodell hinzufügen können, wurde von 50 Stunden auf 100 Stunden erhöht.

### 13. Dezember 2018
{: #December2018a}

Der Service ist jetzt am {{site.data.keyword.cloud_notm}}-Standort London (**eu-gb**) verfügbar. Wie an allen Standorten wird auch am Standort London die tokenbasierte IAM-Authentifizierung verwendet. Alle neuen Serviceinstanzen, die Sie an diesem Standort erstellen, verwenden die IAM-Authentifizierung.

### 12. November 2018
{: #November2018b}

Der Service unterstützt jetzt die intelligente Formatierung bei der Spracherkennung für Japanisch. Bisher hat der Service die intelligente Formatierung nur für amerikanisches Englisch und Spanisch unterstützt. Diese Funktion ist als Betafunktionalität für alle unterstützten Sprachen verfügbar. Weitere Informationen finden Sie im Abschnitt [Intelligente Formatierung](/docs/services/speech-to-text/output.html#smart_formatting).

### 7. November 2018
{: #November2018a}

Der Service ist jetzt am {{site.data.keyword.cloud_notm}}-Standort Tokio (**jp-tok**) verfügbar. Wie an allen Standorten wird auch am Standort Tokio die tokenbasierte IAM-Authentifizierung verwendet. Alle neuen Serviceinstanzen, die Sie an diesem Standort erstellen, verwenden die IAM-Authentifizierung.

### 30. Oktober 2018
{: #October2018b}

Der Service wurde für alle Standorte auf die tokenbasierte IAM-Authentifizierung umgestellt. Alle {{site.data.keyword.cloud_notm}}-Services verwenden jetzt die IAM-Authentifizierung. Der {{site.data.keyword.speechtotextshort}}-Service wurde für die einzelnen Standorte an den folgenden Terminen umgestellt:

-   Dallas (**us-south**): 30. Oktober 2018
-   Frankfurt (**eu-de**): 30. Oktober 2018
-   Washington DC (**us-east**): 12. Juni 2018
-   Sydney (**au-syd**): 15. Mai 2018

Die Umstellung auf die IAM-Authentifizierung wirkt sich auf neue und vorhandene Serviceinstanzen verschieden aus.

-   *Alle neuen Serviceinstanzen, die Sie an einem beliebigen Standort erstellen,* verwenden jetzt die IAM-Authentifizierung für den Servicezugriff. Sie können entweder ein Trägertoken oder einen API-Schlüssel übergeben. Tokens unterstützen authentifizierte Anforderungen ohne Einbinden der Serviceberechtigungsnachweise in jeden Aufruf. API-Schlüssel verwenden die HTTP-Basisauthentifizierung. Wenn Sie eines der {{site.data.keyword.watson}}-SDKs verwenden, können Sie den API-Schlüssel übergeben und den Lebenszyklus der Tokens vom SDK verwalten lassen.
-   *Vorhandene Serviceinstanzen, die Sie vor dem angegebenen Umstellungstermin an einem Standort erstellt haben,* verwenden weiterhin die Kombination aus `{benutzername}` und `{kennwort}` aus den vorherigen Cloud Foundry-Serviceberechtigungsnachweisen, bis Sie die betreffenden Services auf die Verwendung der IAM-Authentifizierung umstellen. Weitere Informationen zur Umstellung auf die IAM-Authentifizierung finden Sie im Abschnitt [Cloud Foundry-Serviceinstanzen auf eine Ressourcengruppe migrieren](https://{DomainName}/docs/resources/instance_migration.html).

Weitere Informationen finden Sie in der folgenden Dokumentation:

-   Um das von Ihrer Serviceinstanz verwendete Authentifizierungsverfahren zu ermitteln, zeigen Sie Ihre Serviceberechtigungsnachweise an, indem Sie im [{{site.data.keyword.cloud_notm}}-Dashboard ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/dashboard/apps){: new_window} auf die Instanz klicken.
-   Weitere Informationen zur Verwendung von IAM-Tokens mit Watson-Services finden Sie unter [Authentifizierung mit IAM-Tokens durchführen](/docs/services/watson/getting-started-iam.html).
-   Weitere Informationen zur Verwendung von IAM-API-Schlüsseln mit Watson-Services finden Sie unter [Service-API-Schlüssel von IAM](/docs/services/watson/apikey-bp.html).
-   Beispiele für die Verwendung der IAM-Authentifizierung finden Sie unter [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 9. Oktober 2018
{: #October2018a}

-   Der Header `Content-Type` ist jetzt für Spracherkennungsanforderungen optional. Der Service erkennt jetzt automatisch das Audioformat (MIME-Typ) der meisten Audiodaten. Für die folgenden Formate müssen Sie den Inhaltstyp (Content-Type) weiterhin angeben: 
    -   `audio/basic`
    -   `audio/l16`
    -   `audio/mulaw`

    Falls angegeben, muss der Inhaltstyp, den Sie für diese Formate angeben, die Abtastfrequenz enthalten und kann optional die Anzahl der Kanäle und die Endianess der Audiodaten enthalten. Bei allen anderen Audioformaten können Sie den Inhaltstyp übergehen oder den Inhaltstyp `application/octet-stream` angeben, damit der Service das Format automatisch erkennt.

    Wenn Sie den Befehl `curl` für eine Spracherkennungsanforderung mit der HTTP-Schnittstelle verwenden, müssen Sie das Audioformat im Header `Content-Type` entweder als `"Content-Type: application/octet-stream"` oder als `"Content-Type:"` angeben. Wenn Sie diesen Header vollständig weglassen, wird von `curl` der Standardwert `application/x-www-form-urlencoded` verwendet. In den meisten Beispielen in dieser Dokumentation wird weiterhin das Format für Spracherkennungsanforderungen angegeben, selbst wenn es nicht erforderlich ist.{: important}

    Diese Änderung gilt für die folgenden Methoden:
    -   `/v1/recognize` für WebSocket-Anforderungen. Das Feld `content-type` der Textnachricht, die Sie über eine geöffnete WebSocket-Verbindung senden können, um eine Anforderung zu initialisieren, ist jetzt optional.
    -   `POST /v1/recognize` für synchrone HTTP-Anforderungen. Der Header `Content-Type` ist jetzt optional. (Für mehrteilige Anforderungen ist das Feld `part_content_type` der JSON-Metadaten jetzt ebenfalls optional.
    -   `POST /v1/recognitions` für asynchrone HTTP-Anforderungen. Der Header `Content-Type` ist jetzt optional.

    Weitere Informationen finden Sie im Abschnitt [Audioformate](/docs/services/speech-to-text/audio-formats.html).
-   Das Breitbandmodell für brasilianisches Portugiesisch, `pt-BR_BroadbandModel`, wurde aktualisiert, um die Spracherkennung zu verbessern. Der Service verwendet standardmäßig für alle Spracherkennungsanforderungen automatisch das aktualisierte Modell. Wenn Sie über angepasste Sprachmodelle verfügen oder über angepasste Akustikmodelle, die auf diesem Modell basieren, müssen Sie Ihre vorhandenen angepassten Modelle mit den folgenden Methoden aktualisieren, um von den Vorteilen der Aktualisierungen zu profitieren:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Weitere Informationen finden Sie im Abschnitt [Upgrade für angepasste Modelle durchführen](/docs/services/speech-to-text/custom-upgrade.html).
-   Der Parameter `customization_id` der Spracherkennungsmethoden wird nicht mehr unterstützt. Er wird in einem künftigen Release entfernt. Wenn Sie ein angepasstes Sprachmodell für eine Spracherkennungsanforderung angeben möchten, verwenden Sie stattdessen den Parameter `language_customization_id`. Diese Änderung gilt für die folgenden Methoden:
    -   `/v1/recognize` für WebSocket-Anforderungen
    -   `POST /v1/recognize` für synchrone HTTP-Anforderungen (einschließlich mehrteiliger Anforderungen)
    -   `POST /v1/recognitions` für asynchrone HTTP-Anforderungen
-   Ab 1. Oktober 2018 sind alle zur Spracherkennung an den Service übergebenen Audiodaten gebührenpflichtig. Die ersten eintausend Minuten im Monat, die von Ihnen gesendet werden, sind nicht mehr kostenlos. Weitere Informationen zu den Preistarifen für den Service enthalten die Angaben zum {{site.data.keyword.speechtotextshort}}-Service im [{{site.data.keyword.cloud_notm}}-Katalog ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/catalog/services/speech-to-text){: new_window}.

### 10. September 2018
{: #September2018b}

Eine Auflistung der seit dem ersten Release behobenen Probleme finden Sie unter [Behobene Probleme](#known_issues).
{: important}

-   Der Service unterstützt jetzt ein Breitbandmodell für Deutsch (`de-DE_BroadbandModel`). Das neue Modell für Deutsch unterstützt die Sprachmodellanpassung (allgemein verfügbar) und die Akustikmodellanpassung (Betaversion).
    -   Weitere Informationen zur Vorgehensweise des Service beim Korpusparsing für Deutsch finden Sie im Abschnitt [Parsing für Englisch, Französisch, Deutsch, Spanisch und brasilianisches Portugiesisch](/docs/services/speech-to-text/language-resource.html#corpusLanguages).
    -   Weitere Informationen zum Erstellen gleich klingender Aussprachevarianten für angepasste Wörter in Deutsch finden Sie im Abschnitt [Richtlinien für Französisch, Deutsch, Spanisch und brasilianisches Portugiesisch](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR).
-   Die vorhandenen Modelle für brasilianisches Portugiesisch (`pt-BR_BroadbandModel` und `pt-BR_NarrowbandModel`) unterstützen jetzt auch die Sprachmodellanpassung (allgemein verfügbar). Zum Aktivieren dieser Unterstützung wurden die Modelle nicht aktualisiert, daher ist kein Upgrade der vorhandenen angepassten Akustikmodelle  erforderlich.
    -   Information zur Vorgehensweise des Service beim Korpusparsing für brasilianisches Portugiesisch finden Sie im Abschnitt [Parsing für Englisch, Französisch, Deutsch, Spanisch und brasilianisches Portugiesisch](/docs/services/speech-to-text/language-resource.html#corpusLanguages).
    -   Weitere Informationen zum Erstellen gleich klingender Aussprachevarianten für angepasste Wörter in brasilianischem Portugiesisch finden Sie im Abschnitt [Richtlinien für Französisch, Deutsch, Spanisch und brasilianisches Portugiesisch](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR).
-   Neue Versionen der Breit- und Schmalbandmodelle für amerikanisches Englisch und Japanisch sind verfügbar:
    -   `en-US_BroadbandModel`
    -   `en-US_NarrowbandModel`
    -   `ja-JP_BroadbandModel`
    -   `ja-JP_NarrowbandModel`

    Die neuen Modelle bieten eine verbesserte Spracherkennung. Der Service verwendet standardmäßig für alle Erkennungsanforderungen automatisch die aktualisierten Modelle. Wenn Sie über angepasste Sprachmodelle verfügen oder über angepasste Akustikmodelle, die auf diesen Modellen basieren, müssen Sie Ihre vorhandenen angepassten Modelle mit den folgenden Methoden aktualisieren, um von den Vorteilen der Aktualisierungen zu profitieren:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Weitere Informationen finden Sie im Abschnitt [Upgrade für angepasste Modelle durchführen](/docs/services/speech-to-text/custom-upgrade.html).
-   Die Funktionen für Schlüsselworterkennung und Wortalternativen sind jetzt für alle Sprachen allgemein verfügbar (GA) und nicht mehr als Betafunktionalität. Weitere Informationen finden Sie in den folgenden Abschnitten:
    -   [Schlüsselworterkennung](/docs/services/speech-to-text/output.html#keyword_spotting)
    -   [Wortalternativen](/docs/services/speech-to-text/output.html#word_alternatives)

### Behobene Probleme
{: #known_issues}

Die folgenden bekannten Probleme im Zusammenhang mit der Anpassungsschnittstelle wurden behoben. Diese Liste wird für Benutzer geführt, die in der Vergangenheit auf diese Probleme gestoßen sind.

-   Wenn Sie Daten zu einem angepassten Sprachmodell oder Akustikmodell hinzufügen, müssen Sie das Modell erneut trainieren, bevor es für die Spracherkennung verwendet wird. Das Problem tritt im folgenden Szenario auf:

    1.  Der Benutzer erstellt ein neues angepasstes Modell (Sprach- oder Akustikmodell) und trainiert das Modell.
    1.  Der Benutzer fügt zusätzliche Ressourcen (Wörter, Korpora oder Audio) zu dem angepassten Modell hinzu, ohne das Modell erneut zu trainieren.
    1.  Der Benutzer kann das angepasste Modell nicht für die Spracherkennung verwenden. Der Service gibt bei Verwendung mit einer Spracherkennungsanforderung einen Fehler wie den folgenden zurück:

        ```javascript
        {
          "code_description": "Falsche Anforderung",
          "code": 400,
          "error": "Das angeforderte angepasste Modell ist nicht verfügbar.
                    Stellen Sie sicher, dass das angepasste Modell trainiert wurde."
        }
        ```
        {: codeblock}

    Um dieses Problem zu umgehen, muss der Benutzer das angepasste Modell mit den zugehörigen aktuellen Daten erneut trainieren. Anschließend kann der Benutzer das angepasste Modell mit Spracherkennung verwenden.
-   Bevor Sie ein vorhandenes angepasstes Sprach- oder Akustikmodell trainieren, müssen Sie für das Modell ein Upgrade auf die aktuelle Version durchführen. Das Problem tritt im folgenden Szenario auf:

    1.  Der Benutzer verfügt über ein vorhandenes angepasstes Modell (Sprach- oder Akustikmodell), das auf einem aktualisierten Modell basiert.
    1.  Der Benutzer trainiert das angepasste Modell anhand der Vorgängerversion des Basismodells, ohne ein Upgrade auf die aktuelle Version des Basismodells durchzuführen.
    1.  Der Benutzer kann das angepasste Modell nicht für die Spracherkennung verwenden.

    Um dieses Problem zu umgehen, muss der Benutzer mit der Methode `POST /v1/customizations/{customization_id}/upgrade_model` oder `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` für das angepasste Modell ein Upgrade auf die aktuelle Version des Basismodells durchführen. Anschließend kann der Benutzer das angepasste Modell mit Spracherkennung verwenden.


Beide Probleme wurden in der Produktionsumgebung behoben.

### 7. September 2018
{: #September2018a}

Die sitzungsbasierte HTTP-REST-Schnittstelle wird nicht mehr unterstützt. Alle Informationen, die sich auf Sitzungen beziehen, werden aus der Dokumentation entfernt. Die folgenden Methoden sind nicht mehr verfügbar:{: important}

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

Wenn Ihre Anwendung die Sitzungsschnittstelle verwendet, müssen Sie ein Upgrade auf eine der übrigen HTTP-REST-Schnittstellen oder auf die WebSocket-Schnittstelle durchführen. Weitere Informationen finden Sie in der Serviceaktualisierung vom [8. August 2018](#August2018).

### 8. August 2018
{: #August2018}

Die sitzungsbasierte HTTP-REST-Schnittstelle wird ab dem **8. August 2018** nicht mehr unterstützt. Alle Methoden der Sitzungs-API werden ab dem **7. September 2018** aus dem Service entfernt. Danach können Sie die sitzungsbasierte Schnittstelle nicht mehr verwenden. Dieser Hinweis auf die sofortige Einstellung der Unterstützung und die Entfernung in 30 Tagen gilt für die folgenden Methoden:

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

Wenn Ihre Anwendung die Sitzungsschnittstelle verwendet, müssen Sie bis zum 7. September auf eine der folgenden Schnittstellen migrieren:
{: important}

-   Verwenden Sie für die datenstrombasierte Spracherkennung (einschließlich Live-Anwendungsfälle) die [WebSocket-Schnittstelle](/docs/services/speech-to-text/websockets.html). Sie bietet Zugriff auf vorläufige Ergebnisse und die niedrigste Latenz.
-   Verwenden Sie für die dateibasierte Spracherkennung eine der folgenden Schnittstellen:

    -   Verwenden Sie für kurze Dateien mit Audiodaten für wenige Minuten entweder die [synchrone HTTP-Schnittstelle](/docs/services/speech-to-text/http.html) `(POST /v1/recognize`) oder die [asynchrone HTTP-Schnittstelle](/docs/services/speech-to-text/async.html) (`POST /v1/recognitions`).
    -   Verwenden Sie für längere Dateien mit Audiodaten für viele Minuten die asynchrone HTTP-Schnittstelle. Die asynchrone HTTP-Schnittstelle akzeptiert in einer einzigen Anforderung bis zu 1 GB Audiodaten.

Die WebSocket- und HTTP-Schnittstellen stellen die gleichen Ergebnisse wie die Sitzungsschnittstelle bereit (nur die WebSocket-Schnittstelle liefert Zwischenergebnisse). Sie können auch eines der Watson-SDKs verwenden, die die Anwendungsentwicklung mit jeder dieser Schnittstellen vereinfacht. Weitere Informationen finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 13. Juli 2018
{: #July2018}

Das Schmalbandmodell für Spanisch, `es-ES_NarrowbandModel`, wurde aktualisiert, um die Spracherkennung zu verbessern. Der Service verwendet standardmäßig für alle Spracherkennungsanforderungen automatisch das aktualisierte Modell. Wenn Sie über angepasste Sprachmodelle verfügen oder über angepasste Akustikmodelle, die auf diesem Modell basieren, müssen Sie Ihre angepassten Modelle mit den folgenden Methoden aktualisieren, um von den Vorteilen der Aktualisierungen zu profitieren:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Weitere Informationen finden Sie im Abschnitt [Upgrade für angepasste Modelle durchführen](/docs/services/speech-to-text/custom-upgrade.html).

Ab dieser Aktualisierung sind die beiden folgenden Versionen des Schmalbandmodells für Spanisch verfügbar:

-   `es_ES.8kHz.general.lm20180522235959.am20180522235959` (aktuelle Version)
-   `es_ES.8kHz.general.lm20180308235959.am20180308235959` (vorherige Version)

Die folgende Version des Modells ist nicht mehr verfügbar:

-   `es_ES.8kHz.general.lm20171031235959.am20171031235959`

Eine Erkennungsanforderung, die ein angepasstes Modell zu verwenden versucht, das auf dem nicht mehr verfügbaren Basismodell basiert, greift auf das aktuelle Basismodell ohne jede Anpassung zurück. Der Service gibt die folgende Warnung zurück: `Ein nicht angepasstes Standardbasismodell wird verwendet, da Ihr angepasstes {typ}-Modell mit einer Version des Basismodells erstellt wurde, die nicht mehr unterstützt wird.` Um wieder ein angepasstes Modell zu verwenden, das auf dem nicht mehr verfügbaren Modell basiert, müssen Sie zuerst ein Upgrade für das Modell mit der entsprechenden Methode `upgrade_model` durchführen, wie zuvor beschrieben.

### 12. Juni 2018
{: #June2018}

Die folgenden Funktionen wurden für Anwendungen aktiviert, die in Washington DC (**us-east**) gehostet werden:

-   Der Service unterstützt jetzt einen neuen Prozess für API-Authentifizierung. Weitere Informationen finden Sie in der [Serviceaktualisierung vom 30. Oktober 2018](#October2018b).
-   Der Service unterstützt jetzt den Header `X-Watson-Metadata` und die Methode `DELETE /v1/user_data`. Weitere Informationen finden Sie im Abschnitt [Informationssicherheit](/docs/services/speech-to-text/information-security.html).

### 15. Mai 2018
{: #May2018}

Die folgenden Funktionen wurden für Anwendungen aktiviert, die in Sydney (**au-syd**) gehostet werden:

-   Der Service unterstützt jetzt einen neuen Prozess für API-Authentifizierung. Weitere Informationen finden Sie in der [Serviceaktualisierung vom 30. Oktober 2018](#October2018b).
-   Der Service unterstützt jetzt den Header `X-Watson-Metadata` und die Methode `DELETE /v1/user_data`. Weitere Informationen finden Sie im Abschnitt [Informationssicherheit](/docs/services/speech-to-text/information-security.html).

### 26. März 2018
{: #March2018b}

-   Der Sevice unterstützt jetzt die Sprachmodellanpassung für das das Sprachmodell für Französisch (`fr-FR_BroadbandModel`). Das Modell für Französisch ist für den Einsatz in Produktionsumgebungen mit Sprachmodellanpassung allgemein verfügbar.
    -   Weitere Informationen zur Vorgehensweise des Service beim Korpusparsing für Französisch finden Sie im Abschnitt [Parsing für Englisch, Französisch, Deutsch, Spanisch und brasilianisches Portugiesisch](/docs/services/speech-to-text/language-resource.html#corpusLanguages).
    -   Weitere Informationen zum Erstellen gleich klingender Aussprachevarianten für angepasste Wörter in Französisch finden Sie im Abschnitt [Richtlinien für Französisch, Deutsch, Spanisch und brasilianisches Portugiesisch](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR).
-   Die Schmalbandmodelle für Spanisch und Koreanisch (`es-ES_NarrowbandModel` und `ko-KR_NarrowbandModel`) und das Breitbandmodell für Französisch (`fr-FR_BroadbandModel`) wurden aktualisiert, um die Spracherkennung zu verbessern. Der Service verwendet standardmäßig für alle Erkennungsanforderungen automatisch die aktualisierten Modelle. Wenn Sie über angepasste Sprachmodelle verfügen oder über angepasste Akustikmodelle, die auf einem dieser Modelle basieren, müssen Sie Ihre angepassten Modelle mit den folgenden Methoden aktualisieren, um von den Vorteilen der Aktualisierungen zu profitieren:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Weitere Informationen finden Sie im Abschnitt [Upgrade für angepasste Modelle durchführen](/docs/services/speech-to-text/custom-upgrade.html).
-   Der Parameter `version` der folgenden Methoden wurde in `base_model_version` umbenannt:

    -   `/v1/recognize` für WebSocket-Anforderungen
    -   `POST /v1/recognize` für HTTP-Anforderungen ohne Sitzung
    -   `POST /v1/sessions` für sitzungsbasierte HTTP-Anforderungen
    -   `POST /v1/recognitions` für asynchrone HTTP-Anforderungen

    Der Parameter `base_model_version` gibt die Version eines Basismodells an, das für die Spracherkennung verwendet werden soll. Weitere Informationen finden Sie in den Abschnitten [Erkennungsanforderungen mit aktualisierten angepassten Modellen](/docs/services/speech-to-text/custom-upgrade.html#upgradeRecognition) und [Version des Basismodells](/docs/services/speech-to-text/input.html#version).
-   Die intelligente Formatierung wird jetzt für Spanisch und für amerikanisches Englisch unterstützt. Die Funktion für amerikanisches Englisch konvertiert jetzt auch Schlüsselwortzeichenfolgen in Interpunktionssymbole für Punkte, Kommas, Fragezeichen und Ausrufezeichen. Weitere Informationen finden Sie im Abschnitt [Intelligente Formatierung](/docs/services/speech-to-text/output.html#smart_formatting).

### 1. März 2018
{: #March2018a}

Die Breitbandmodelle für Spanisch und Französisch (`es-ES_BroadbandModel` und `fr-FR_BroadbandModel`) wurden aktualisiert, um die Spracherkennung zu verbessern. Der Service verwendet standardmäßig für alle Erkennungsanforderungen automatisch die aktualisierten Modelle. Wenn Sie über angepasste Sprachmodelle verfügen oder über angepasste Akustikmodelle, die auf einem dieser Modelle basieren, müssen Sie Ihre angepassten Modelle mit den folgenden Methoden aktualisieren, um von den Vorteilen der Aktualisierungen zu profitieren:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Weitere Informationen finden Sie im Abschnitt [Upgrade für angepasste Modelle durchführen](/docs/services/speech-to-text/custom-upgrade.html). In diesem Abschnitt werden Regeln für die Durchführung von Upgrades für angepasste Modelle, die Auswirkungen der Upgrades und Konzepte für die Verwendung aktualisierter Modelle beschrieben.

### 1. Februar 2018
{: #February2018}

Der Service bietet jetzt Modelle für die Spracherkennung für Koreanisch: `ko-KR_BroadbandModel` für Audiodaten mit einer Abtastfrequenz von mindestens 16 kHz  und `ko-KR_NarrowbandModel` für Audiodaten mit einer Abtastfrequenz von mindestens 8 kHz. Weitere Informationen finden Sie im Abschnitt [Sprachen und Modelle](/docs/services/speech-to-text/models.html).

Die Modelle für Koreanisch sind für den Einsatz in Produktionsumgebungen mit Sprachmodellanpassung allgemein verfügbar. Für die Akustikmodellanpassung sind sie als Betafunktionalität verfügbar. Weitere Informationen finden Sie im Abschnitt [Sprachunterstützung für Anpassung](/docs/services/speech-to-text/custom.html#languageSupport).


-   Weitere Informationen zur Vorgehensweise des Service beim Korpusparsing für Koreanisch finden Sie im Abschnitt [Parsing für Koreanisch](/docs/services/speech-to-text/language-resource.html#corpusLanguages-koKR).
-   Weitere Informationen zum Erstellen gleich klingender Aussprachevarianten für angepasste Wörter in Koreanisch finden Sie im Abschnitt [Richtlinien für Koreanisch](/docs/services/speech-to-text/language-resource.html#wordLanguages-koKR).

### 14. Dezember 2017
{: #December2017}

-   Die Modelle für amerikanisches Englisch (`en-US_BroadbandModel` und `en-US_NarrowbandModel`) wurden aktualisiert, um die Spracherkennung zu verbessern. Der Service verwendet standardmäßig für alle Erkennungsanforderungen automatisch die aktualisierten Modelle. Wenn Sie über angepasste Sprachmodelle verfügen oder über angepasste Akustikmodelle, die auf einem der Modelle für amerikanisches Englisch basieren, müssen Sie Ihre angepassten Modelle mit den folgenden Methoden aktualisieren, um von den Vorteilen der Aktualisierungen zu profitieren:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Weitere Informationen zur Vorgehensweise finden Sie im Abschnitt [Upgrade für angepasste Modelle durchführen](/docs/services/speech-to-text/custom-upgrade.html). In diesem Abschnitt werden Regeln für die Durchführung von Upgrades für angepasste Modelle, die Auswirkungen der Upgrades und Konzepte für die Verwendung aktualisierter Modelle beschrieben. Derzeit sind die Methoden nur auf die neuen Basismodelle für amerikanisches Englisch anwendbar. Dieselben Informationen gelten jedoch auch für Upgrades anderer Basismodelle, sobald sie verfügbar sind.
-   Die verschiedenen Methoden für Erkennungsanforderungen enthalten jetzt einen neuen Parameter `base_model_version` zum Initialisieren von Anforderungen, in denen die früheren oder aktualisierten Versionen von Basismodellen und angepassten Modellen verwendet werden. Obwohl der Parameter `base_model_version` in erster Linie zur Verwendung mit angepassten Modellen, die aktualisiert wurden, bestimmt ist, kann er auch ohne angepasste Modelle verwendet werden. Weitere Informationen finden Sie im Abschnitt [Version des Basismodells](/docs/services/speech-to-text/input.html#version).
-   Der Service unterstützt jetzt die Akustikmodellanpassung als Betafunktionalität für alle verfügbaren Sprachen. Sie können angepasste Akustikmodelle für Breitband- oder Schmalbandmodelle für alle Sprachen erstellen. Eine Einführung in die Anpassung (einschließlich Akustikmodellanpassung) finden Sie im Abschnitt [Die Anpassungsschnittstelle](/docs/services/speech-to-text/custom.html).
-   Der Service unterstützt jetzt die Sprachmodellanpassung für die Modelle für amerikanisches Englisch (`en-GB_BroadbandModel` und `en-GB_NarrowbandModel`). Obwohl der Service Korpora für britisches und amerikanisches Englisch und angepasste Wörter auf ähnliche Weise verarbeitet, sind einige wichtige Unterschiede zu beachten:
    -   Weitere Informationen zur Vorgehensweise des Service beim Parsing von Korpora für britisches Englisch finden Sie im Abschnitt [Parsing für Englisch, Französisch, Deutsch, Spanisch und brasilianisches Portugiesisch](/docs/services/speech-to-text/language-resource.html#corpusLanguages).
    -   Weitere Informationen zum Erstellen gleich klingender Aussprachevarianten für angepasste Wörter in britischem Englisch finden Sie im Abschnitt [Richtlinien für amerikanisches und britisches Englisch](/docs/services/speech-to-text/language-resource.html#wordLanguages-enUS-enGB). In gleich klingenden Aussprachevarianten für britisches Englisch dürfen keine Punkte und Gedankenstriche verwendet werden.
-   Die Sprachmodellanpassung und alle zugehörigen Parameter sind jetzt für alle unterstützten Sprachen allgemein verfügbar: Japanisch, Spanisch, britisches Englisch und amerikanisches Englisch.

### 2. Oktober 2017
{: #October2017}

-   Die Anpassungsschnittstelle bietet jetzt die Akustikmodellanpassung. Sie können jetzt angepasste Akustikmodelle erstellen, die die Basismodelle des Service an Ihre Umgebung und die verwendeten Sprecher anpassen. Zum Bestücken und Trainieren eines angepassten Akustikmodells werden Audiodaten verwendet, die noch enger an die akustische Signatur der Audiodaten angelehnt sind, die Sie transkribieren möchten. Anschließend können Sie das angepasste Akustikmodell mit Erkennungsanforderungen verwenden, um die Genauigkeit der Spracherkennung zu optimieren.

    Angepasste Akustikmodelle ergänzen angepasste Sprachmodelle. Sie können ein angepasstes Akustikmodell mit einem angepassten Sprachmodell trainieren und Sie können beide Modelltypen für die Spracherkennung verwenden. Die Akustikmodellanpassung ist als Schnittstelle (Betaversion) nur für amerikanisches Englisch, Spanisch und Japanisch verfügbar.

    -   Weitere Informationen zu den von der Anpassungsschnittstelle unterstützten Sprachen und zur verfügbaren Unterstützungsstufe für die einzelnen Sprachen finden Sie im Abschnitt [Sprachunterstützung für Anpassung](/docs/services/speech-to-text/custom.html#languageSupport).
    -   Weitere Informationen zur Anpassungsschnittstelle für den Service finden Sie im Abschnitt [Die Anpassungsschnittstelle](/docs/services/speech-to-text/custom.html).
    -   Weitere Informationen zum Erstellen eines angepassten Akustikmodells finden Sie im Abschnitt [Angepasstes Akustikmodell erstellen](/docs/services/speech-to-text/acoustic-create.html).
    -   Weitere Informationen zur Verwendung eines angepassten Akustikmodells finden Sie im Abschnitt [Angepasstes Akustikmodell verwenden](/docs/services/speech-to-text/acoustic-use.html).
    -   Weitere Informationen zu allen Methoden der Anpassungsschnittstelle finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Für die Sprachmodellanpassung stellt der Service jetzt eine Betafunktion zur Verfügung, die optional eine Anpassungsgewichtung für ein angepasstes Sprachmodell festlegt. Die Anpassungsgewichtung gibt die relative Gewichtung für Wörter aus einem angepassten Sprachmodell im Verhältnis zu Wörtern aus dem Basisvokabular des Service an. Eine Anpassungsgewichtung können Sie sowohl beim Trainieren als auch bei der Spracherkennung festlegen. Weitere Informationen finden Sie im Abschnitt [Anpassungsgewichtung verwenden](/docs/services/speech-to-text/language-use.html#weight).
-   Das Sprachmodell `ja-JP_BroadbandModel` wurde aktualisiert, um Verbesserungen des Basismodells zu nutzen. Die Aktualisierung wirkt sich nicht auf vorhandene angepasste Modelle aus, die auf dem Modell basieren.
-   Der Service enthält jetzt einen Parameter zum Angeben der Endianess von Audiodaten, die im Format `audio/l16` (lineare 16-Bit-Pulsecodemodulation (PCM)) übergeben werden. Neben den Parametern `rate` und `channels` für das Format können Sie jetzt zusätzlich den Wert `big-endian` oder `little-endian` mit dem Parameter `endianness` angeben. Weitere Informationen finden Sie im Abschnitt [Audioformate](/docs/services/speech-to-text/audio-formats.html).

### 14. Juli 2017
{: #July2017b}

-   Der Service unterstützt jetzt die Transkription von Audiodaten in den Formaten MP3 oder MPEG (Motion Picture Experts Group). Weitere Informationen zu unterstützten Audioformaten finden Sie im Abschnitt [Audioformate](/docs/services/speech-to-text/audio-formats.html).
-   Die Schnittstelle für Sprachmodellanpassung unterstützt jetzt Spanisch als Betafunktionalität. Sie können ein angepasstes Modell erstellen, das auf einem der Sprachmodelle für Spanisch (`es-ES_BroadbandModel` oder `es-ES_NarrowbandModel`) basiert. Weitere Informationen finden Sie im Abschnitt [Angepasstes Sprachmodell erstellen](/docs/services/speech-to-text/language-create.html). Für Erkennungsanforderungen, die angepasste Sprachmodelle für Spanisch verwenden, gilt die gleiche Preisstruktur wie bei Modellen für amerikanisches Englisch und Japanisch.
-   Das JSON-Objekt `CreateLanguageModel`, das Sie an die Methode `POST /v1/customizations` übergeben, um ein neues angepasstes Sprachmodell zu erstellen, enthält jetzt ein Feld `dialect`. Das Feld gibt den Dialekt der Sprache an, der für das angepasste Modell verwendet werden soll. Der Dialekt entspricht standardmäßig der Sprache des Basismodells. Der Parameter ist nur für Modelle in Spanisch von Bedeutung, für die der Service ein angepasstes Modell erstellen kann, das für Sprachdaten in einem der folgenden Dialekte geeignet ist:
    -   `es-ES` für Spanisch (Kastilien), die Standardeinstellung
    -   `es-LA` für Spanisch (Lateinamerika)
    -   `es-US` für Spanisch (Mexiko, Nordamerika)

    Die Methoden `GET /v1/customizations` und `GET /v1/customizations/{customization_id}` der Anpassungsschnittstelle enthalten in der Ausgabe den Dialekt eines angepassten Modells. Weitere Informationen finden Sie in den Abschnitten [Angepasstes Sprachmodell erstellen](/docs/services/speech-to-text/language-create.html#createModel-language) und [Angepasste Sprachmodelle auflisten](/docs/services/speech-to-text/language-models.html#listModels-language).
-   Die Namen der Sprachmodelle (`en-UK_BroadbandModel` und `en-UK_NarrowbandModel`) werden nicht mehr verwendet. Diese Modelle tragen jetzt die Namen `en-GB_BroadbandModel` und `en-GB_NarrowbandModel`.

    Die nicht mehr verwendeten Namen `en-UK_{model}` funktionieren weiterhin, aber die Methode `GET /v1/models` gibt die Namen nicht mehr in der Liste der verfügbaren Modelle aus. Sie können die Namen jedoch weiterhin direkt mit der Methode `GET /v1/models/{model_id}` abfragen.

### 1. Juli 2017
{: #July2017a}

-   Die Schnittstelle für Sprachmodellanpassung des Service ist jetzt für beide unterstützten Sprachen (amerikanisches Englisch und Japanisch) allgemein verfügbar. {{site.data.keyword.IBM_notm}} berechnet keine Gebühren für Erstellung, Hosting und Verwaltung angepasster Sprachmodelle. Wie im nächsten Listenpunkt erläutert, berechnet {{site.data.keyword.IBM_notm}} jetzt zusätzlich $ 0,03 (USD) pro Minute für Audiodaten für Erkennungsanforderungen, die angepasste Modelle verwenden.
-   Die {{site.data.keyword.IBM_notm}} Preisgestaltung für den Service wurde wie folgt geändert:
    -   Die Zusatzgebühr für die Verwendung von Schmalbandmodellen wurde gestrichen.
    -   Eine gestaffelte Preisgestaltung für Kunden mit hohem Verbrauch wird bereitgestellt.
    -   Eine Zusatzgebühr von $ 0,03 (USD) pro Minute gilt für Audiodaten für Erkennungsanforderungen, die angepasste Sprachmodelle für amerikanisches Englisch oder Japanisch verwenden.

    Weitere Informationen zur geänderten Preisgestaltung finden Sie hier:
    -   Blogbeitrag [{{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} API - Pricing Updates ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/blogs/bluemix/2017/05/ibm-watson-speech-text-api-pricing-updates/){: new_window}
    -   Der {{site.data.keyword.speechtotextshort}}-Service im [{{site.data.keyword.cloud_notm}}-Katalog ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/catalog/services/speech-to-text){: new_window}
    -   [Häufig gestellte Fragen (FAQs) zur Preisgestaltung](/docs/services/speech-to-text/faq-pricing.html)
-   Es ist nicht mehr erforderlich, ein leeres Datenobjekt als Hauptteil für die folgenden `POST`-Anforderungen zu übergeben:
    -   `POST /v1/sessions`
    -   `POST /v1/register_callback`
    -   `POST /v1/customizations/{customization_id}/train`
    -   `POST /v1/customizations/{customization_id}/reset`
    -   `POST /v1/customizations/{customization_id}/upgrade_model`

    Beispiel: Die Methode `POST /v1/sessions` wird jetzt mit `curl` wie folgt aufgerufen:

    ```bash
    curl -X POST -u "{benutzername}:{kennwort}"
    --cookie-jar cookies.txt
    "https://stream.watsonplatform.net/speech-to-text/api/v1/sessions"
    ```
    {: pre}

    Sie müssen die folgende `curl`-Option nicht mehr mit der Anforderung übergeben: `--data "{}"`.

    Wenn Probleme mit einer dieser `POST`-Anforderungen auftreten, versuchen Sie, ein leeres Datenobjekt mit dem Hauptteil der Anforderung zu übergeben. Durch das Übergeben eines leeren Objekts wird weder die Art noch die Bedeutung der Anforderung geändert.
    {: note}

### 22. Mai 2017
{: #May2017}

Die folgenden, im März 2017 erstmals angekündigten Einstellungen der Unterstützung sind jetzt wirksam:

-   Der Parameter `continuous` wurde aus allen Methoden entfernt, die Erkennungsanforderungen aufrufen. Der Service transkribiert jetzt einen vollständigen Audiodatenstrom bis er endet oder das Zeitlimit überschritten wird (je nachdem, was zuerst eintritt). Dieses Verhalten entspricht dem Festlegen des vorherigen Parameters `continuous` auf `true`. Standardmäßig wurde die Transkription für den Service nach der ersten halben Sekunde Stille (Sprechpause) gestoppt, wenn der Parameter nicht angegeben oder auf `false` gesetzt war.

    Für vorhandene Anwendungen, in denen der Parameter auf `true` gesetzt wird, bleibt das Verhalten unverändert. Für Anwendungen, in denen der Parameter auf `false` gesetzt oder das Standardverhalten verwendet wurde, kann eine Verhaltensänderung auftreten. Wenn der Parameter in einer Anforderung angegeben wird, gibt der Service jetzt eine Warnung für den unbekannten Parameter zurück:

    ```javascript
    "warnings": [
      "Unknown arguments: continuous."
    ]
    ```
    {: codeblock}

    Die Anforderung wird trotz Warnung erfolgreich ausgeführt und eine vorhandene Sitzung oder WebSocket-Verbindung ist nicht davon betroffen.

    Der Parameter wurde von {{site.data.keyword.IBM_notm}} als Antwort auf die mehrheitliche Auffassung der Entwicklercommunity entfernt, dass die Angabe `continuous=false` kaum Mehrwert bietet und die Transkriptionsgenauigkeit insgesamt beeinträchtigen kann.
-   Es ist nicht mehr möglich, eine Sitzungszeitlimitüberschreitung zu vermeiden, ohne Audiodaten zu senden:
    -   Wenn Sie die WebSocket-Schnittstelle verwenden, kann der Client eine Verbindung nicht mehr durch das Senden einer JSON-Textnachricht aufrecht erhalten, die den Parameter `action` mit dem Wert `no-op` enthält. Das Senden einer Nachricht `no-op` generiert keinen Fehler und hat keine Auswirkungen.
    -   Wenn Sie Sitzungen in Verbindung mit der HTTP-Schnittstelle verwenden, kann der Client die Sitzung nicht mehr durch das Senden einer Anforderung `GET /v1/sessions/{session_id}/recognize` verlängern. Diese Methode gibt zwar weiterhin den Status einer aktiven Sitzung zurück, aber die Sitzungsaktivität wird nicht aufrecht erhalten. Sie können nun wie folgt vorgehen, damit eine Sitzung aktiv bleibt:
    -   Setzen Sie den Parameter `inactivity_timeout` auf den Wert `-1`, um das Inaktivitätszeitlimit (30 Sekunden) zu umgehen.
    -   Senden Sie beliebige Audiodaten (die Daten können auch aus Stille (Sprechpause) bestehen) an den Service, um das Sitzungszeitlimit von 30 Sekunden zu umgehen. Die Übertragungszeit der Daten, die Sie an den Service senden (einschließlich der Sprechpausen, um eine Sitzung aktiv zu halten) wird Ihnen in Rechnung gestellt.

    Weitere Informationen finden Sie im Abschnitt [Zeitlimits](/docs/services/speech-to-text/input.html#timeouts). Im Idealfall würden Sie unmittelbar vor dem Abrufen der Audiodaten für die Transkription eine Sitzung aufbauen und diese Sitzung aktiv halten, indem Audiodaten nahezu in Echtzeit gesendet werden. Stellen Sie außerdem sicher, dass Ihre Anwendung geschlossene Sitzungen oder Verbindungen ordnungsgemäß verarbeitet.

    Diese Funktionalität wurde von {{site.data.keyword.IBM_notm}} entfernt, um sicherzustellen, dass für alle Benutzer eine leistungsfähige Spracherkennung mit niedrigen Latenzzeiten bereitgestellt wird.

### 10. April 2017
{: #April2017}

-   Der Service unterstützt jetzt die Funktion für Sprecherbezeichnungen in den folgenden Breitbandmodellen:
    -   `en-US-BroadbandModel`
    -   `es-ES-BroadbandModel`
    -   `ja-JP_BroadbandModel`

    Weitere Informationen finden Sie im Abschnitt [Sprecherbezeichnungen](/docs/services/speech-to-text/output.html#speaker_labels).
-   Der Service unterstützt jetzt das Audioformat 'Web Media' (WebM) mit dem Opus- oder Vorbis-Codec. Außerdem unterstützt er das Audioformat 'Ogg' nun zusätzlich zum Opus-Codec mit dem Vorbis-Codec. Weitere Informationen zu unterstützten Audioformaten finden Sie im Abschnitt [Audioformate](/docs/services/speech-to-text/audio-formats.html).
-   Der Service unterstützt jetzt Cross-Origin Resource Sharing (CORS), damit browserbasierte Clients den Service direkt aufrufen können. Weitere Informationen finden Sie im Abschnitt [CORS-Unterstützung](/docs/services/speech-to-text/developer-overview.html#cors).
-   Die asynchrone HTTP-Schnittstelle bietet jetzt eine Methode `POST /v1/unregister_callback`, die die Registrierung für eine in der Whitelist aufgeführte Callback-URL entfernt. Weitere Informationen finden Sie im Abschnitt [Registrierung einer Callback-URL rückgängig machen](/docs/services/speech-to-text/async.html#unregister).
-   In der WebSocket-Schnittstelle tritt bei Erkennungsanforderungen für besonders lange Audiodateien keine Zeitlimitüberschreitung mehr auf. Es ist nicht länger erforderlich, mithilfe der JSON-Nachricht `start` Zwischenergebnisse anzufordern, um die Zeitlimitüberschreitung zu vermeiden. (Dieses Problem wurde in den Releaseinformationen vom 10. März 2016 beschrieben.)
-   Die Methode `DELETE /v1/customizations/{customization_id}` gibt jetzt den HTTP-Antwortcode 401 zurück, wenn Sie versuchen, ein nicht vorhandenes  angepasstes Modell zu löschen.
-   Die Methode `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` gibt jetzt den HTTP-Antwortcode 400 zurück, wenn Sie versuchen, ein nicht vorhandenes Korpus zu löschen.

### 8. März 2017
{: #March2017}

Die asynchrone HTTP-Schnittstelle ist jetzt allgemein verfügbar. Bis zu diesem Datum wurde sie als Betafunktionalität bereitgestellt.

### 1. Dezember 2016
{: #December2016}

-   Der Service stellt jetzt Sprecherbezeichnungen für Schmalbandaudiodaten in den Sprachen amerikanisches Englisch, Spanisch oder Japanisch als Betafunktionalität bereit. Diese Funktion gibt an, welche Wörter in einem Austausch zwischen mehreren Personen von welchen Sprechern gesprochen wurden. Die Erkennungsmethoden 'sessionless', 'session-based', 'asynchronous' und 'WebSocket' enthalten jeweils einen Parameter `speaker_labels`, der einen booleschen Wert akzeptiert. Dieser Wert gibt an, ob Sprecherbezeichnungen in die Antwort einbezogen werden sollen. Weitere Informationen zu der Funktion finden Sie im Abschnitt [Sprecherbezeichnungen](/docs/services/speech-to-text/output.html#speaker_labels).
-   Die Betaversion der Schnittstelle für Sprachmodellanpassung wird jetzt für Japanisch und für amerikanisches Englisch unterstützt. Alle Methoden der Schnittstelle unterstützen Japanisch. Weitere Informationen finden Sie in den folgenden Abschnitten:
    -   Weitere Informationen finden Sie in den Abschnitten [Angepasstes Sprachmodell erstellen](/docs/services/speech-to-text/language-create.html) und [Angepasstes Sprachmodell verwenden](/docs/services/speech-to-text/language-use.html).
    -   Allgemeine Hinweise zum Hinzufügen einer Korpustextdatei und spezielle Hinweise für Japanisch finden Sie in den Abschnitten [Korpusdatei vorbereiten](/docs/services/speech-to-text/language-resource.html#prepareCorpus) und [Was passiert, wenn ich eine Korpusdatei hinzufüge?](/docs/services/speech-to-text/language-resource.html#parseCorpus)
    -   Spezielle Hinweise für Japanisch beim Angeben des Felds `sounds_like` für ein angepasstes Wort finden Sie im Abschnitt [Richtlinien für Japanisch](/docs/services/speech-to-text/language-resource.html#wordLanguages-jaJP).
    -   Weitere Informationen zu allen Methoden der Anpassungsschnittstelle finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Die Schnittstelle für Sprachmodellanpassung enthält jetzt eine Methode `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` zum Auflisten von Information zu einem angegebenen Korpus. Die Methode ist hilfreich zum Überwachen des Status einer Anforderung zum Hinzufügen eines Korpus zu einem angepassten Modell. Weitere Informationen finden Sie im Abschnitt [Korpora für angepasstes Sprachmodell auflisten](/docs/services/speech-to-text/language-corpora.html#listCorpora).
-   Die von den Methoden `GET /v1/customizations/{customization_id}/words` und `GET /v1/customizations/{customization_id}/words/{word_name}` zurückgegebene JSON-Ausgabe enthält jetzt ein Feld `count` für jedes Wort. Das Feld gibt an, wie oft das Wort in allen Korpora gefunden wurde. Wenn Sie ein angepasstes Wort zu einem Modell hinzufügen, bevor das Wort von einem Korpus hinzugefügt wurde, beginnt der Zähler mit dem Wert `1`. Wird das Wort zuerst von einem Korpus hinzugefügt und später geändert, gibt der Wert nur die Anzahl der gefundenen Vorkommen des Worts in den Korpora an. Weitere Informationen finden Sie im Abschnitt [Wörter aus angepasstem Sprachmodell auflisten](/docs/services/speech-to-text/language-words.html#listWords).

    Für angepasste Modelle, die erstellt wurden, bevor das Feld `count` hinzugefügt wurde, enthält das Feld immer den Wert `0`. Um das Feld für solche Modelle zu aktualisieren, fügen Sie die Korpora des Modells erneut hinzu und geben Sie den Parameter `allow_overwrite` mit der Methode `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` an.
-   Die Methode `GET /v1/customizations/{customization_id}/words` enthält jetzt einen Abfrageparameter `sort`, der angibt, in welcher Reihenfolge die Wörter aufgelistet werden sollen. Der Parameter akzeptiert die beiden Argumente `alphabetical` oder `count`, um die Sortierreihenfolge für die Wörter anzugeben. Sie können einem Argument optional das Zeichen `+` oder `-` voranstellen, um anzugeben, ob die Ergebnisse in aufsteigender oder absteigender Reihenfolge sortiert werden sollen. Standardmäßig werden die Wörter von der Methode in aufsteigender alphabetischer Reihenfolge angezeigt. Weitere Informationen finden Sie im Abschnitt [Wörter aus angepasstem Sprachmodell auflisten](/docs/services/speech-to-text/language-words.html#listWords).

    Für angepasste Modelle, die vor der Einführung des Felds `count` erstellt wurden, ist die Verwendung des Arguments `count` im Parameter `sort` ohne Bedeutung. Verwenden Sie für solche Modelle das Standardargument `alphabetical`.
-   Das Feld `error`, das als Teil der JSON-Antwort von den Methoden `GET /v1/customizations/{customization_id}/words` und `GET /v1/customizations/{customization_id}/words/{word_name}` zurückgegeben werden kann, ist jetzt ein Array. Wenn der Service mindestens ein Problem für die Definition eines angepassten Worts festgestellt hat, werden in dem Feld alle Problemelemente aus der Definition und eine Nachricht mit der Beschreibung des Problems aufgelistet. Weitere Informationen finden Sie im Abschnitt [Wörter aus angepasstem Sprachmodell auflisten](/docs/services/speech-to-text/language-words.html#listWords).
-   Die Parameter `keywords_threshold` und `word_alternatives_threshold` der Erkennungsmethoden akzeptieren keine Nullwerte mehr. Um Schlüsselwörter und Wortalternativen aus der Antwort auszuschließen, geben Sie die Parameter nicht an. Als Wert muss ein Gleitkommawert angegeben werden.

Weitere Informationen zur Schnittstelle des Service finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 22. September 2016
{: #September2016}

-   Der Service stellt jetzt eine neue Schnittstelle für die Sprachmodellanpassung für amerikanisches Englisch als Betaversion bereit. Mit dieser Schnittstelle können Sie das Basisvokabular und die Sprachmodelle des Service anpassen, indem Sie angepasste Sprachmodelle erstellen, die  fachspezifische Terminologie enthalten. Sie können angepasste Wörter einzeln hinzufügen oder von einem Service aus Korpora extrahieren lassen. Um Ihre angepassten Modelle mit den von einer der Serviceschnittstellen bereitgestellten Spracherkennungsmethoden zu verwenden, übergeben Sie den Abfrageparameter `customization_id`. Weitere Informationen finden Sie hier: 
    -   [Angepasstes Sprachmodell erstellen](/docs/services/speech-to-text/language-create.html)
    -   [Angepasstes Sprachmodell verwenden](/docs/services/speech-to-text/language-use.html)
    -   [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/speech-to-text){: new_window}
-   Die Liste der unterstützten Audioformate enthält jetzt das Format `audio/mulaw`, das Einkanalaudiodaten zur Verfügung stellt, die mit dem Datenalgorithmus 'u-law' (oder 'mu-law') codiert sind. Wenn Sie dieses Format verwenden, müssen Sie auch die Abtastfrequenz für die Erfassung von Audiodaten angeben. Weitere Informationen finden Sie im Abschnitt [Audioformate](/docs/services/speech-to-text/audio-formats.html).
-   Die Methoden `GET /v1/models` und `GET /v1/models/{model_id}` geben jetzt ein Feld `supported_features` als Teil ihrer Ausgabe für jedes Sprachmodell zurück. Diese zusätzlichen Informationen beschreiben, ob die Anpassung für dieses Modell unterstützt wird. Weitere Informationen finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 30. Juni 2016
{: #June2016b}

Die Betaversion der asynchronen HTTP-Schnittstelle bietet jetzt Unterstützung für alle Sprachen, die vom Service unterstützt werden. Bisher war die Schnittstelle nur für amerikanisches Englisch verfügbar. Weitere Informationen finden Sie im Abschnitt [Die asynchrone HTTP-Schnittstelle](/docs/services/speech-to-text/async.html) und in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 23. Juni 2016
{: #June2016a}

-   Die asynchrone HTTP-Schnittstelle ist jetzt als Betaversion verfügbar. Die Schnittstelle bietet alle Erkennungsfunktionen für Transkription in amerikanischem Englisch über nicht blockierende HTTP-Aufrufe. Sie können Callback-URLs registrieren und benutzerspezifische geheime Zeichenfolgen angeben, um die Authentifizierung und Datenintegrität mithilfe digitaler Signaturen umzusetzen. Weitere Informationen finden Sie im Abschnitt [Die asynchrone HTTP-Schnittstelle](/docs/services/speech-to-text/async.html) und in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Eine Betafunktion für intelligente Formatierung ermöglicht das Konvertieren von Datumsangaben, Zeitangaben, Ziffern- und Zahlenreihen, Telefonnummern, Währungswerten und Internetadressen in herkömmliche Darstellungen für endgültige Transkripte. Sie können diese Funktion aktivieren, indem Sie in einer Erkennungsanforderung den Parameter `smart_formatting` auf `true` setzen. Diese Funktion ist nur als Betafunktionalität für amerikanisches Englisch verfügbar. Weitere Informationen finden Sie im Abschnitt [Intelligente Formatierung](/docs/services/speech-to-text/output.html#smart_formatting).
-   Die Liste der für die Spracherkennung unterstützten Modelle enthält jetzt `fr-FR_BroadbandModel` für Audiodaten in Französisch mit einer Abtastfrequenz von mindestens 16 kHz. Weitere Informationen finden Sie im Abschnitt [Sprachen und Modelle](/docs/services/speech-to-text/models.html).
-   Die Liste der unterstützten Audioformate enthält jetzt das Format `audio/basic`. Dieses Format stellt Einkanalaudiodaten mit 8 Bit in der Codierung 'u-law' (bzw. 'mu-law') und mit einer Abtastfrequenz von 8 kHz bereit. Weitere Informationen finden Sie im Abschnitt [Audioformate](/docs/services/speech-to-text/audio-formats.html).
-   Die verschiedenen Erkennungsmethoden können eine Antwort mit Warnungen (`warnings`) zurückgeben, die Nachrichten über ungültige Abfrageparameter enthalten oder JSON-Felder, die in einer Anforderung enthalten sind. Das Format der Warnungen wurde geändert. Beispiel: `"warnings": "Unbekannte Argumente: [u'{invalid_arg_1}', u'{invalid_arg_2}']."` wurde geändert in: `"warnings": "Ungekannte Argumente: {invalid_arg_1}, {invalid_arg_2}."`
-   Für HTTP-Anforderungen `POST`, die keine anderen Daten an den Service übergeben, müssen Sie einen leeren Anforderungshauptteil mit dem Format `{}` übergeben. In Verbindung mit dem Befehl `curl` wird die Option `--data` verwendet, um die leeren Daten zu übergeben.

### 10. März 2016
{: #March2016}

-   Bei beiden Formen der Datenübertragung (Einzelübermittlung bzw. Streaming) gilt jetzt eine Größenbegrenzung auf 100 MB für die Audiodaten (wie bei der WebSocket-Schnittstelle). Bislang galt für die Einzelübertragung ein Maximalwert von 4 MB. Weitere Informationen finden Sie in den Abschnitten [Übertragung von Audiodaten](/docs/services/speech-to-text/input.html#transmission) (für alle Schnittstellen) und [Audiodaten senden und Erkennungsergebnisse empfangen](/docs/services/speech-to-text/websockets.html#WSaudio) (für die WebSocket-Schnittstelle). Im Abschnitt über WebSocket wird außerdem die maximale Frame- oder Nachrichtengröße von 4 MB für die WebSocket-Schnittstelle erläutert.
-   Die JSON-Antwort für eine Erkennungsanforderung kann jetzt ein Array mit Warnungen für einzelne Abfrageparameter oder JSON-Felder aus einer Anforderung enthalten. Jedes Element des Arrays ist eine Zeichenfolge, die die Art der Warnung beschreibt, gefolgt von einem Array mit ungültigen Argumenten. Beispiel: `"warnings": [ "Unbekannte Argumente: [u'{invalid_arg_1}', u'{invalid_arg_2}']." ].`. Weitere Informationen finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Die Betaversion des *{{site.data.keyword.watson}} Speech Software Development Kit (SDK) für das Apple&reg;-Betriebssystem iOS* wird nicht mehr verwendet. Verwenden Sie stattdessen das *{{site.data.keyword.watson}}-SDK für das Apple&reg;-Betriebssystem iOS*. Das neue SDK ist im [Repository 'ios-sdk' ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/watson-developer-cloud/ios-sdk){: new_window} im Namensbereich `watson-developer-cloud` bei GitHub verfügbar.

Für die WebSocket-Schnittstelle ist aktuell das folgende Problem bekannt:

-   Es kann Minuten dauern, bis der Service endgültige Ergebnisse zu einer Erkennungsanforderung für eine besonders umfangreiche Audiodatei liefert. Die zugrunde liegende TCP-Verbindung für die WebSocket-Schnittstelle bleibt inaktiv, solange der Service die Antwort vorbereitet. Dies kann dazu führen, dass die Verbindung aufgrund einer Zeitlimitüberschreitung beendet wird. Um die Zeitlimitüberschreitung der WebSocket-Schnittstelle zu vermeiden, fordern Sie Zwischenergebnisse (`\"interim_results\": \"true\"`) im JSON-Code für die Nachricht `start` an, um die Anforderung zu initialisieren. Sie können die Zwischenergebnisse löschen, wenn sie nicht benötigt werden. Dieses Problem wir in einem künftigen Update behoben.

### 19. Januar 2016
{: #January2016}

Der Service wurde aktualisiert und enthält seit dem 19. Januar 2016 eine neue Filterfunktion für vulgäre Ausdrücke. Der Service zensiert standardmäßig vulgäre Ausdrücke in Transkriptionsergebnissen der Audiodaten für amerikanisches Englisch. Weitere Informationen finden Sie im Abschnitt [Vulgäre Ausdrücke filtern](/docs/services/speech-to-text/output.html#profanity_filter).

### 17. Dezember 2015
{: #December2015}

-   Der Service bietet jetzt eine Funktion für Schlüsselworterkennung. Sie können ein Array mit Schlüsselwortzeichenfolgen angeben, die in den Eingabeaudiodaten erkannt werden sollen. Außerdem müssen Sie ein benutzerdefiniertes Konfidenzniveau angeben, das ein Wort aufweisen muss, damit es als Übereinstimmung mit einem Schlüsselwort infrage kommt. Weitere Informationen finden Sie im Abschnitt [Schlüsselworterkennung](/docs/services/speech-to-text/output.html#keyword_spotting).

    Die Funktion für Schlüsselworterkennung wird als Betafunktionalität bereitgestellt.
    {: note}
-   Der Service bietet jetzt eine Funktion für Wortalternativen. Diese Funktion liefert alternative Hypothesen für Wörter in den Eingabeaudiodaten, die ein vom Benutzer definiertes Konfidenzniveau aufweisen. Weitere Informationen finden Sie im Abschnitt [Wortalternativen](/docs/services/speech-to-text/output.html#word_alternatives).

    Die Funktion für Wortalternativen wird als Betafunktionalität bereitgestellt.
    {: note}
-   Der Service unterstützt jetzt mehr Sprachen durch die bereitgestellten Transkriptionsmodelle: `en-UK_BroadbandModel` und `en-UK_NarrowbandModel` für britisches Englisch und `ar-AR_BroadbandModel` für modernes Hocharabisch. Weitere Informationen finden Sie im Abschnitt [Sprachen und Modelle](/docs/services/speech-to-text/models.html).
-   HTTP-Erkennungsanforderungen unterliegen nicht mehr einem Plattformzeitlimit von 10 Minuten. Der Service hält die Verbindung aktiv, indem alle 20 Sekunden ein Leerzeichen im JSON-Antwortobjekt übergeben wird, solange die Erkennung ausgeführt wird. Weitere Informationen finden Sie im Abschnitt [Zeitlimits](/docs/services/speech-to-text/input.html#timeouts). 
-   Der Service gibt nicht mehr den HTTP-Statuscode 490 für die sitzungsbasierten HTTP-Methoden `GET /v1/sessions/{session_id}/observe_result` und `POST /v1/sessions/{session_id}/recognize` zurück. Stattdessen antwortet der Service jetzt mit dem HTTP-Statuscode 400.

    In den JSON-Antworten, die der Service für Fehler bei sitzungsbasierten Methoden zurückgibt, ist jetzt eine neues Feld `session_closed` enthalten. Dieses Feld wird auf `true` gesetzt, wenn die Sitzung aufgrund des Fehlers geschlossen wird. Weitere Informationen zu  möglichen Rückgabecodes für Methoden finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Wenn Sie den Befehl `curl` zum Transkribieren von Audiodaten mit dem Service verwenden, müssen Sie nicht mehr die Option `--limit-rate` angeben, damit nicht mehr als 40.000 Datenbyte pro Sekunde übertragen werden.

### 21. September 2015
{: #September2015}

-   Für die Speech-Services sind zwei neue Software Development Kits (SDKs) für mobile Geräte als Betafunktionalität verfügbar. Die SDKs ermöglichen mobilen Anwendungen die Interaktion mit {{site.data.keyword.speechtotextshort}}- und {{site.data.keyword.texttospeechshort}}-Services.
    -   Das *{{site.data.keyword.watson}} Speech SDK für die Google Android&trade;-Plattform* unterstützt das Streaming von Audiodaten zum {{site.data.keyword.speechtotextshort}}-Service in Echtzeit und das nahezu zeitgleiche Empfangen eines Transkripts der Audiodaten. Das Projekt enthält eine Beispielanwendung zur Veranschaulichung der Interaktion mit beiden Speech-Services. Das SDK ist im [Repository 'speech-android-sdk' ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/watson-developer-cloud/speech-android-sdk){: new_window} im Namensbereich `watson-developer-cloud` bei GitHub verfügbar.
    -   Das *{{site.data.keyword.watson}} Speech SDK für das Apple&reg;-Betriebssystem iOS* unterstützt das Streaming von Audiodaten zum {{site.data.keyword.speechtotextshort}}-Service und das Empfangen eines Transkripts der Audiodaten als Antwort. Das SDK ist im [Repository 'speech-ios-sdk' ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/watson-developer-cloud/speech-ios-sdk){: new_window} im Namensbereich `watson-developer-cloud` bei GitHub verfügbar.

    Beide SDKs unterstützen die Authentifizierung bei den Speech-Services entweder mit Ihren {{site.data.keyword.cloud_notm}}-Serviceberechtigungsnachweisen oder mit einem Authentifizierungstoken.

    Da die SDKs als Betafunktionalität vorliegen, bleiben künftige Änderungen vorbehalten.
    {: note}
-   Der Service unterstützt zwei weitere Sprachen - brasilianisches Portugiesisch und Chinesisch (Mandarin). Die Modelle für diese zusätzlichen Sprachen sind `pt-BR_BroadbandModel`, `pt-BR_NarrowbandModel`, `zh-CN_BroadbandModel` und `zh-CN_NarrowbandModel`. Weitere Informationen finden Sie im Abschnitt [Sprachen und Modelle](/docs/services/speech-to-text/models.html).
-   Die HTTP-`POST`-Anforderungen `/v1/sessions/{session_id}/recognize` und `/v1/recognize` sowie die WebSocket-Anforderung `/v1/recognize` unterstützen die Transkription eines neuen Medientyps: `audio/ogg;codecs=opus` für Dateien im Ogg-Format, die den Opus-Codec verwenden. Darüber hinaus unterstützt das Format `audio/wav` für die Methoden jetzt jede Codierung. Die Einschränkung in bezug auf die Verwendung der linearen PCM-Codierung wurde behoben. Weitere Informationen finden Sie im Abschnitt [Audioformate](/docs/services/speech-to-text/audio-formats.html).
-   Der Service unterstützt jetzt die Überwindung von Zeitlimits beim Transkribieren langer Audiodateien mit der HTTP-Schnittstelle. Beim Arbeiten mit Sitzungen können Sie ein langes Abfragemuster verwenden, indem Sie mit den Methoden `GET /v1/sessions/{session_id}/observe_result` und `POST /v1/sessions/{session_id}/recognize` Folgen-IDs für Erkennungstasks mit langer Laufzeit angeben. Mit dem neuen Parameter `sequence_id` für diese Methoden können Sie vor, während und nach dem Übergeben einer Erkennungsanforderung Ergebnisse anfordern.
-   Für die Sprachmodelle für amerikanisches Englisch (`en_US_BroadbandModel` und `en_US_NarrowbandModel`) verwendet der Service jetzt bei vielen Eigennamen die korrekte Großschreibung. Beispiel: Der Service gibt jetzt den Text "Barack Obama graduated from Columbia University" zurück und nicht "barack obama graduated from columbia university". Diese Änderung kann für Sie von Interesse sein, wenn Ihre Anwendung die Groß-/Kleinschreibung von Eigennamen berücksichtigen muss.
-   Die HTTP-Anforderung `DELETE /v1/sessions/{session_id}` gibt nicht den Statuscode 415 "Unsupported Media Type" zurück. Dieser Rückgabecode wurde aus der Dokumentation für die Methode entfernt.

### 1. Juli 2015
{: #July2015}

Der Service wird nicht mehr als Betaversion bereitgestellt, sondern ist seit dem 1. Juli 2015 allgemein verfügbar. Zwischen der Betaversion und der allgemein verfügbaren Version der {{site.data.keyword.speechtotextshort}}-APIs bestehen die folgenden Unterschiede. Für das allgemein verfügbare Release müssen die Benutzer ein Upgrade auf die neue Version des Service durchführen.

-   Die allgemein verfügbare Version der HTTP-API ist mit der Betaversion kompatibel. Sie müssen Ihren vorhandenen Anwendungscode nur ändern, wenn Sie einen Modellnamen explizit angegeben haben. Der Beispielcode für den Service bei GitHub enthielt beispielsweise die folgende Codezeile in der Datei `demo.js`:

    ```javascript
    model: 'WatsonModel'
    ```
    {: codeblock}

    In dieser Zeile war das Standardmodell `WatsonModel` für die Betaversion des Service angegeben. Wenn dieses Modell auch in Ihrer Anwendung angegeben wird, müssen Sie die Anwendung ändern, sodass eines der neuen Modelle verwendet wird, die von der allgemein verfügbaren Version unterstützt werden. Weitere Informationen finden Sie im nächsten Listenpunkt.
-   Der Service unterstützt jetzt ein neues Programmiermodell für die direkte Interaktion zwischen einem Client und dem Service über eine WebSocket-Verbindung. Bei Verwendung dieses Modells kann ein Client ein Authentifizierungstoken für die direkte Kommunikation mit dem Service anfordern. Dieses Token macht es für eine serverseitige Proxy-Anwendung in {{site.data.keyword.cloud_notm}} überflüssig, den Service im Namen des Clients aufzurufen. Tokens sind die bevorzugte Methode für die Interaktion von Clients mit dem Service.

    Der Service unterstützt weiterhin das frühere Programmiermodell, bei dem ein serverseitiger Proxy Audiodaten und Nachrichten zwischen dem Client und dem Service überträgt. Das neue Modell ist jedoch effizienter und bietet einen höheren Durchsatz. Weitere Informationen zu dem neuen Programmiermodell finden Sie im Abschnitt [Programmiermodelle für {{site.data.keyword.watson}}-Services](/docs/services/watson/getting-started-develop.html).
-   Die Methoden `POST /v1/sessions` und `POST /v1/recognize` sowie die WebSocket-Methode `/v1/recognize` unterstützen jetzt einen Abfrageparameter `model`. Mit diesem Parameter können Sie folgende Informationen zu den Audiodaten angeben:

    -   Sprache: *Englisch*, *Japanisch* oder *Spanisch*
    -   Mindestabtastfrequenz: *Breitband* (16 kHz) oder *Schmalband* (8 kHz)

    Weitere Informationen finden Sie im Abschnitt [Sprachen und Modelle](/docs/services/speech-to-text/models.html).
-   Der Header `Content-Type` in den `recognize`-Methoden unterstützt jetzt die Formate `audio/wav` für Waveform-Audiodateien (WAV-Dateien) sowie `audio/flac` und `audio/l16`. Weitere Informationen zu den unterstützten Audioformaten finden Sie im Abschnitt [Audioformate](/docs/services/speech-to-text/audio-formats.html).
-   Die `recognize`-Methoden unterstützen jetzt einige neue Abfrageparameter, mit denen Sie den Service an die Anforderungen Ihrer Anwendungen anpassen können: 
    -   Der Parameter `inactivity_timeout` legt das Zeitlimit in Sekunden fest, nach dem der Service die Verbindung beendet, wenn im Streaming-Modus eine Sprechpause (Stille) erkannt wird. Standardmäßig wird die Sitzung vom Service nach einer Sprechpause von 30 Sekunden beendet. Weitere Informationen finden Sie im Abschnitt [Zeitlimits](/docs/services/speech-to-text/input.html#timeouts).
    -   Der Parameter `max_alternatives` veranlasst den Service, die *n* besten alternativen Hypothesen für die Audiotranskription zurückzugeben. Weitere Informationen finden Sie im Abschnitt [Maximale Anzahl Alternativen](/docs/services/speech-to-text/output.html#max_alternatives).
    -   Der Parameter `word_confidence` veranlasst den Service, für jedes Wort der Transkription einen Konfidenzwert zurückzugeben. Weitere Informationen finden Sie im Abschnitt [Wortkonfidenz](/docs/services/speech-to-text/output.html#word_confidence).
    -   Der Parameter `timestamps` veranlasst den Service, die Anfangs- und Endzeit in Relation zum Start der Audiodaten für jedes Wort in der Transkription zurückzugeben. Weitere Informationen finden Sie im Abschnitt [Wortzeitmarken](/docs/services/speech-to-text/output.html#word_timestamps).
-   Die Methode `GET /v1/sessions/{session_id}/observeResult` wurde in `GET /v1/sessions/{session_id}/observe_result` umbenannt. Der vorherige Name `observeResult` wird für die Abwärtskompatibilität weiterhin unterstützt.
-   Die Methoden `GET /v1/sessions/{session_id}/observe_result`, `POST /v1/sessions/{session_id}/recognize` und `POST /v1/recognize` enthalten jetzt den Headerparameter `X-WDC-PL-OPT-OUT`. Dieser Parameter steuert, ob der Service die Audio- und Transkriptionsdaten aus der Anforderung verwendet, um zukünftige Ergebnisse zu verbessern. Die WebSocket-Schnittstelle enthält einen funktional entsprechenden Abfrageparameter. Geben Sie den Wert `1` an, um zu verhindern, dass der Service die Audio- und Transkriptionsergebnisse verwendet. Der Parameter gilt nur für die aktuelle Anforderung. Der neue Header ersetzt den Header `X-logging` aus der Betaversion der API. Weitere Informationen finden Sie im Abschnitt [Anforderungsprotokollierung für {{site.data.keyword.watson}}-Services steuern](../common/getting-started-logging.html).
-   Im Streaming-Modus gilt für den Service jetzt ein Grenzwert von 100 MB für Daten pro Sitzung. Den Streaming-Modus können Sie durch Angeben des Werts `chunked` mit dem Header `Transfer-Encoding` angeben. Bei der Einzelübertragung einer Audiodatei gilt weiterhin ein Größenlimit von 4 MB für die gesendeten Daten. Weitere Informationen finden Sie im Abschnitt [Übertragung von Audiodaten](/docs/services/speech-to-text/input.html#transmission).
-   Für die Methoden `/v1/models`, `/v1/models/{model_id}`, `/v1/sessions`, `/v1/sessions/{session_id}`, `/v1/sessions/{session_id}/observe_result`, `/v1/sessions/{session_id}/recognize` und `/v1/recognize` wurde der Fehlercode 415 (Unsupported Media Type, nicht unterstützter Medientyp) hinzugefügt.
-   Für die Anforderungen `POST` und `GET` an die Methode `/v1/sessions/{session_id}/recognize` wurden die folgenden Fehlercodes geändert:
    -   Für den Fehlercode 404 ("Session_id not found") wurde eine aussagekräftigere Nachricht hinzugefügt (`POST` und `GET`).
    -   Für den Fehlercode 503 ("Session is already processing a request. Concurrent requests are not allowed on the same session. Session remains alive after this error.") wurde eine aussagekräftigere Nachricht hinzugefügt (nur `POST`).
-   Für HTTP-`POST`-Anforderungen an die Methoden `/v1/sessions` und `/v1/recognize` kann der Fehlercode 503 ("Service Unavailable") zurückgegeben werden. Der Fehlercode kann auch beim Erstellen einer WebSocket-Verbindung mit der Methode `/v1/recognize` zurückgegeben werden.
