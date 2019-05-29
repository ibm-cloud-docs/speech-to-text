---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-10"

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

# Häufig gestellte Fragen zur Preisstruktur
{: #faq-pricing}

1.  <span style="color:#003F69">Was kostet die Verwendung des {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}-Standardservice?</span>

    Für den {{site.data.keyword.speechtotextshort}}-Service werden $0,02 (USD) pro Minute berechnet. Dieser Preis gilt für die Verwendung von Breitband- und Schmalbandmodellen. Weitere Informationen finden Sie auf der [Landing-Page ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/speech-to-text.html#pricing-block){: new_window} für den {{site.data.keyword.speechtotextshort}}-Service.

1.  <span style="color:#003F69">Was bedeutet 'Preis pro Minute'? Wird die Anzahl der Minuten von Audiodaten, die an den Service gesendet werden, oder die Anzahl der Minuten in Rechnung gestellt, die der Service für die Verarbeitung der Audiodaten benötigt?</span>

    Grundlage für den Preis ist die Menge der Audiodaten, die an den Service gesendet werden, und nicht die Zeit, die der Service benötigt, um die Audiodaten zu verarbeiten.

1.  <span style="color:#003F69">Wird bei jedem Aufruf der API auf die nächste Minute aufgerundet? Wenn ich beispielsweise zwei Audiodateien mit einer Länge von jeweils 30 Sekunden sende, werden dann zwei Minuten oder wird eine Minute berechnet?</span>

    {{site.data.keyword.IBM_notm}} rundet die Länge der Audiodaten für jeden vom Service empfangenen API-Aufruf nicht auf. {{site.data.keyword.IBM_notm}} summiert stattdessen die gesamte monatliche Nutzung und rundet am Monatsende auf die nächste Minute auf. Für Ihr Beispiel bedeutet dies, dass {{site.data.keyword.IBM_notm}} bei zwei gesendeten Audiodateien mit einer Länge von jeweils 30 Sekunden die Dauer der gesamten Audiodaten auf eine Minute summiert und $0,02 (USD) in Rechnung stellt.

1.  <span id="graduated" style="color:#003F69">Wie funktioniert die nutzungsvolumenbasierte gestaffelte Preisgestaltung?</span>

    Das Modell der gestaffelten Preisgestaltung dient dazu, Benutzern mit hohem Nutzungsvolumen bei fortlaufender Verwendung des Service Rabatte zu gewähren. Sobald bestimmte Schwellenwerte für die monatliche Summe der Audiodaten erreicht werden, wird der minutengenaue Preistarif für weitere Minuten von Audiodaten reduziert. Zusätzliche Angaben finden Sie auf der [Seite für die Preisstruktur ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/catalog/services/speech-to-text){: new_window} des Service.

1.  <span style="color:#003F69">Wie hoch ist bei der gestaffelten Preisgestaltung meine Gesamtgebühr, wenn ich beispielsweise 275.000 Minuten Audiodaten in einem Monat durch den Service transkribieren lasse?</span>

    -   Für die ersten 250.000 Minuten Audiodaten werden Ihnen in diesem Fall $0,02 (USD) pro Minute berechnet, also 250.000 * $0,02 = $5000,00 (USD).
    -   Für die restlichen 25.000 Minuten Audiodaten wird Ihnen der reduzierte Tarif von $0,015 (USD) pro Minute in Rechnung gestellt, also 25.000 * $0,015 = $375,00 (USD).
    -   Die Gesamtgebühr für den Monat läge für dieses Szenario bei $5375,00 (USD).

1.  <span style="color:#003F69">Was kostet die Nutzung der Anpassungsschnittstelle des Service? Wird die Erstellung und Speicherung eines angepassten Modells in Rechnung gestellt?</span>

    Bei der *Sprachmodellanpassung* berechnet {{site.data.keyword.IBM_notm}} keine Gebühren für die Erstellung oder das Hosting eines angepassten Sprachmodells, sondern lediglich für die Verwendung des Modells bei einer Erkennungsanforderung. Für die Nutzung eines angepassten Sprachmodells für die Transkription fällt eine zusätzliche Gebühr in Höhe von $0,03 (USD) pro Minute an. Diese Gebühr wird zusätzlich zur Standardnutzungsgebühr von $0,02 (USD) pro Minute in Rechnung gestellt. Sie gilt für alle von der Anpassungsschnittstelle unterstützten Sprachen. Die Gesamtgebühr für die Verwendung eines angepassten Sprachmodells bei der Spracherkennung beträgt somit $0,05 (USD) pro Minute.

    Bei der *Akustikmodellanpassung*, die für alle unterstützten Sprachen als Betaschnittstelle verfügbar ist, berechnet {{site.data.keyword.IBM_notm}} keine Gebühren für die Erstellung, das Hosting oder die Spracherkennung mit einem angepassten Akustikmodell. Änderungen an der kostenlosen Nutzung von angepassten Akustikmodellen für Erkennungsanforderungen bleiben in der Zukunft vorbehalten.
