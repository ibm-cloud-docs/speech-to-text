---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-30"

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

# Produktinformation
{: #about}

**Serviceaktualisierung: ** *Der {{site.data.keyword.speechtotextshort}}-Service wurde am 30. Juli 2019 aktualisiert. Der Service bietet nun Betamodelle für fünf weitere spanische Dialekte an: Spanisch (Argentinien), Spanisch (Chile), Spanisch (Kolumbien), Spanisch (Mexiko) und Spanisch (Peru). Weitere Informationen zu den neuen spanischen Dialekten finden Sie im Abschnitt zur [Serviceaktualisierung vom 30. Juli 2019](/docs/services/speech-to-text?topic=speech-to-text-release-notes#July2019) in den Releaseinformationen.*

Der {{site.data.keyword.speechtotextfull}}-Service stellt Sprachtranskriptionsfunktionen für Ihre Anwendungen bereit. Er nutzt maschinelles Lernen, um menschliche Stimme mithilfe von Kenntnissen über Grammatik, Sprachstruktur sowie die Bildung von Audio- und Sprachsignalen präzise zu transkribieren. Der Service aktualisiert und optimiert die Transkription kontinuierlich, sobald er weitere Spracheingabe empfängt.
{: shortdesc}

Der Service bietet verschiedene Schnittstellen, die ihn für jede Anwendung geeignet machen, bei der die Eingabe aus Sprache und die Ausgabe aus einer Transkription in Text besteht. Beispiele für solche Anwendungen:

-   Sprachsteuerung von Anwendungen, integrierten Einheiten und Fahrzeugzubehör
-   Transkriptionen von Besprechungen und Telefonkonferenzen
-   Diktate von E-Mail-Nachrichten und Memos

Der Service ist ideal für Kunden, die qualitativ hochwertige Sprachtranskripte aus Call-Center-Audiodaten extrahieren müssen. Kunden in Branchen wie Finanzdienstleistungen, Gesundheitswesen, Versicherungen und Telekommunikation können Cloud-native Anwendungen für die Kundenbetreuung, Sprachunterstützung von Kunden, Agentenunterstützung und andere Lösungen entwickeln.

## Unterstützte Schnittstellen
{: #interfaces}

Für die Spracherkennung bietet der {{site.data.keyword.speechtotextshort}}-Service drei Schnittstellen:

-   [WebSocket-Schnittstelle](/docs/services/speech-to-text?topic=speech-to-text-websockets) zum Aufbau von permanenten Vollduplexverbindungen zum Service mit niedriger Latenzzeit. Mit einer einzigen Anforderung können Sie maximal 100 MB Audiodaten an den Service übergeben.
-   [Synchrone HTTP-Schnittstelle](/docs/services/speech-to-text?topic=speech-to-text-http) für HTTP-Basisaufrufe des Service. Mit einer Anforderung können Sie maximal 100 MB Audiodaten übergeben.
-   [Asynchrone HTTP-Schnittstelle](/docs/services/speech-to-text?topic=speech-to-text-async) für nicht blockierende Aufrufe des Service. Mit einer Anforderung können Sie bis zu 1 GB Audiodaten übergeben.

Der Service bietet darüber hinaus eine [Anpassungsschnittstelle](/docs/services/speech-to-text?topic=speech-to-text-customization), mit der Sie die Spracherkennung für Ihre Sprache und Ihre akustischen Anforderungen optimieren können. Hierbei können Sie das Vokabular eines Modells durch fachspezifische Terminologie erweitern oder ein Modell für die akustischen Merkmale Ihrer Audiodaten anpassen. Außerdem können Sie [Grammatiken](/docs/services/speech-to-text?topic=speech-to-text-grammars) hinzufügen, um die durch den Service erkennbaren Ausdrücke einzuschränken.

-   Eine allgemeine Beschreibung der Anwendungsentwicklung mit dem Service finden Sie unter [Übersicht für Entwickler](/docs/services/speech-to-text?topic=speech-to-text-developerOverview).
-   Beispiele für Basisanforderungen zur Spracherkennung mit jeder Schnittstelle des Service enthält der Abschnitt [Erkennungsanforderung ausgeben](/docs/services/speech-to-text?topic=speech-to-text-basic-request).

SDKs sind in vielen Programmiersprachen verfügbar, um die Verwendung des Service zu vereinfachen. Weitere Informationen finden Sie in der [API-Referenz](https://{DomainName}/apidocs/speech-to-text){: external}.

## Eingabefunktionen
{: #inputFeatures}

Die Schnittstellen des Service nutzen gemeinsam gängige Eingabefunktionen, um Sprache in Text zu transkribieren:

-   [Audioformate](/docs/services/speech-to-text?topic=speech-to-text-audio-formats) - Sie können Audiodaten in den Formaten 'Ogg' oder 'Web Media' (WebM) mit Opus- oder Vorbis-Codec, MP3 (oder MPEG), WAF (Waveform Audio File Format, FLAC (Free Lossless Audio Codec), linearer 16-Bit-Pulscodemodulation (PCM), G.729 sowie A-Law-, Mu-Law- (bzw. U-Law) und Basisaudiodaten transkribieren. Wenn Sie ein Format verwenden, das Komprimierung unterstützt, können Sie die Menge der Audiodaten maximieren, die Sie mit einer Anforderung senden können.
-   [Sprachen und Modelle](/docs/services/speech-to-text?topic=speech-to-text-models) - Bei den meisten Sprachen können Sie Audiodaten mithilfe von Breitband- oder Schmalbandmodellen transkribieren. Verwenden Sie Breitbandmodelle für Audiodaten, die mit einer Frequenz von mindestens 16 kHz abgetastet werden. Verwenden Sie Schmalbandmodelle für Audiodaten, die mit einer Frequenz von mindestens 8 kHz abgetastet werden.
-   [Übertragung von Audiodaten](/docs/services/speech-to-text?topic=speech-to-text-input#transmission) - Sie können Audiodaten als zusammenhängenden Strom von Datenblöcken oder in Form einer Einmallieferung übergeben, bei der alle Daten gleichzeitig übertragen werden. Beim Streaming setzt der Service [Zeitlimits](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts) für Inaktivität und Sitzungen durch.

## Ausgabefunktionen
{: #outputFeatures}

Die Schnittstellen unterstützen außerdem die folgenden gängigen Ausgabefunktionen:

-   Die Funktion [Sprecherbezeichnungen](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels) erkennt unterschiedliche Sprecher aus Audiodaten in amerikanischem Englisch, britischem Englisch, Spanisch oder Japanisch. In der Transkription sind die Beiträge jedes einzelnen Sprechers zu einem Gespräch mit mehreren Teilnehmern gekennzeichnet. (Hierbei handelt es sich um Beta-Funktionalität.)
-   Die Funktion [Schlüsselworterkennung](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting) ermittelt gesprochene Wortfolgen, die mit angegebenen Schlüsselwortzeichenfolgen übereinstimmen, auf einem benutzerdefinierten Konfidenzniveau. Die auch als 'Keyword Spotting' bezeichnete Schlüsselworterkennung ist besonders von Nutzen, wenn einzelne Wortfolgen in den Audiodaten wichtiger als die vollständige Transkription sind. Beispielsweise könnte ein Kundendienstsystem durch die Erkennung von Schlüsselwörtern bestimmen, wie Benutzeranfragen weitergeleitet werden müssen.
-   Die Funktion [Zwischenergebnisse](/docs/services/speech-to-text?topic=speech-to-text-output#interim) gibt während der laufenden Verarbeitung der Transkription progressive Hypothesen zurück. Nach Abschluss der Transkription gibt der Service die Endergebnisse zurück. Zwischenergebnisse sind nur bei Verwendung der WebSocket-Schnittstelle verfügbar.
-   Die Funktion [Maximale Anzahl Alternativen](/docs/services/speech-to-text?topic=speech-to-text-output#max_alternatives) stellt mögliche alternative Transkriptionen bereit. Der Service gibt die Endergebnisse mit der größen Übereinstimmungswahrscheinlichkeit an.
-   Die Funktion [Wortalternativen](/docs/services/speech-to-text?topic=speech-to-text-output#word_alternatives) fordert alternative Wörter an, die den Wörtern einer Transkription akustisch ähneln.
-   Die Funktion [Wortkonfidenz](/docs/services/speech-to-text?topic=speech-to-text-output#word_confidence) gibt für jedes Wort einer Transkription das Konfidenzniveau zurück.
-   Die Funktion [Wortzeitmarken](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps) gibt Zeitmarken für den Anfang und das Ende jedes Wortes in einer Transkription zurück.
-   Die Funktion [Intelligente Formatierung](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting) konvertiert Datumsangaben, Uhrzeiten, Zahlen, Währungswerte, Telefonnummern und Internetadressen in der Endfassung der Transkriptionen in besser lesbare und herkömmliche Formate. Für amerikanisches Englisch können Sie außerdem Schlüsselwortphrasen angeben, damit die Endfassung von Transkriptionen bestimmte Interpunktionssymbole enthalten. Die intelligente Formatierung wird für Audiodaten in amerikanischem Englisch, Japanisch und Spanisch unterstützt. (Hierbei handelt es sich um Beta-Funktionalität.)
-   Die Funktion [Zahlenschwärzung](/docs/services/speech-to-text?topic=speech-to-text-output#redaction) macht numerische Daten in der Endfassung einer Transkription unkenntlich. Zweck der Schwärzung ist es, sensible personenbezogene Daten wie beispielsweise Kreditkartennummern aus Transkriptionen zu entfernen. Die Funktion wird für Audiodaten in amerikanischem Englisch, Japanisch und Koreanisch unterstützt. (Hierbei handelt es sich um Beta-Funktionalität.)
-   Die Funktion [Vulgäre Ausdrücke filtern](/docs/services/speech-to-text?topic=speech-to-text-output#profanity_filter) zensiert Vulgärsprache in Transkriptionen für amerikanisches Englisch.
-   [Verarbeitungsmetriken](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics) stellen detaillierte Zeitinformationen über die Analyse der Audioeingabedaten durch den Service bereit.
-   [Audiometriken](/docs/services/speech-to-text?topic=speech-to-text-metrics#audio_metrics) stellen detaillierte Informationen zu den Signalmerkmalen der Eingabeaudiodaten bereit.

## Sprachunterstützung
{: #languages}

Der Service bietet Modelle für die folgenden Sprachen und Dialekte:

-   Arabisch (Moderner Standard)
-   Brasilianisches Portugiesisch
-   Chinesisch (Mandarin)
-   Britisches Englisch und amerikanisches Englisch
-   Französisch
-   Deutsch
-   Japanisch
-   Koreanisch
-   Spanisch (Argentinien, Kastilien, Chile, Kolumbien, Mexiko und Peru)

Es werden durch den Service nicht alle Funktionen für alle Sprachen unterstützt. Zudem werden für unterschiedliche Sprachen einige Funktionen als allgemein verfügbare Funktionen für den Produktionseinsatz unterstützt, während andere Funktionen als Betaversion angeboten werden.

-   Der spanische Dialekt Kastilisch ist allgemein verfügbar. Die anderen fünf spanischen Dialekte sind Betamodelle.
-   Die WebSocket-Schnittstelle und die HTTP-Schnittstelle sind für alle Sprachen allgemein verfügbar.
-   Der Service bietet für verschiedene Sprachen Breitband- und/oder Schmalbandmodelle. Weitere Informationen finden Sie unter [Sprachen und Modelle](/docs/services/speech-to-text?topic=speech-to-text-models).
-   Einige Spracherkennungsfunktionen sind nur bei bestimmten Sprachen verfügbar. Weitere Informationen enthält der Abschnitt [Parameterübersicht](/docs/services/speech-to-text?topic=speech-to-text-summary).
-   Die Anpassungsschnittstelle für Sprachmodelle ist für die meisten Sprachen allgemein verfügbar. Die Anpassungsschnittstelle für Akustikmodelle ist für alle Sprachen als Beta-Funktionalität verfügbar. Weitere Informationen finden Sie unter [Sprachunterstützung bei der Anpassung](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport).

## Preisstruktur
{: #pricing-index}

Weitere Informationen zu den Preistarifen für den Service finden Sie auf der Seite für den {{site.data.keyword.speechtotextshort}}-Service im [{{site.data.keyword.cloud}}-Katalog](https://{DomainName}/catalog/services/speech-to-text){: external}. Antworten auf Fragen zur Preisstruktur und Beispiele für Kosten der monatlichen Nutzung enthält der Abschnitt [Häufig gestellte Fragen zur Preisstruktur](/docs/services/speech-to-text?topic=speech-to-text-faq-pricing).

## Service ausprobieren
{: #tryOut}

Beispiele für die Ausführung des {{site.data.keyword.speechtotextshort}}-Service bieten die folgenden Quellen:

-   Eine [Kurzdemo](https://speech-to-text-demo.ng.bluemix.net/){: external} zeigt die Transkription von Text aus einer Streamingaudioeingabe oder einer hochgeladenen Datei.
-   Anwendungen in den {{site.data.keyword.ibmwatson}}-[Starter-Kits](http://www.ibm.com/watson/developercloud/starter-kits.html){: external} veranschaulichen den Service.
-   Im {{site.data.keyword.watson}}-Blogbeitrag [Getting robots to listen: Using {{site.data.keyword.watson}}'s {{site.data.keyword.speechtotextshort}} service](https://www.ibm.com/blogs/watson/2016/07/getting-robots-listen-using-watsons-speech-text-service/){: external} erfahren Sie, wie die WebSocket-Schnittstelle mit Python eingesetzt wird, um Sprache aus Audiodaten zu extrahieren. Der Beitrag bietet ein umfassendes Lernprogramm, das den verwendeten Code und die betreffenden Schritte veranschaulicht.
