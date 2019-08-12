---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-03"

subcollection: speech-to-text

---

{:faq: .faq}
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

# Häufig gestellte Fragen zur Preisstruktur
{: #faq-pricing}

## Was kostet die Verwendung des Lite-Plans für {{site.data.keyword.speechtotextshort}}?
{: #faq-pricing-zero}
{: faq}

Mit dem Lite-Plan erhalten Sie als Einstieg 500 Minuten pro Monat kostenlos bei der Spracherkennung. Der Lite-Plan bietet keinen Zugriff auf die Anpassungsschnittstellen des Service. Weitere Informationen finden Sie auf der [Seite für die Preisstruktur](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} für den {{site.data.keyword.speechtotextshort}}-Service.


## Was kostet die Verwendung des Standard-Plans für {{site.data.keyword.speechtotextshort}}?
{: #faq-pricing-one}
{: faq}

Für den Standard-Plan werden $0,02 (USD) pro Minute bei der Spracherkennung berechnet. Dieser Preis gilt für die Verwendung von Breitband- und Schmalbandmodellen. Der Plan verwendet ein gestaffeltes Preismodell und ermöglicht es Ihnen, die Anpassungsschnittstellen gegen eine zusätzliche Gebühr zu verwenden. Weitere Informationen finden Sie auf der [Seite für die Preisstruktur](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} für den {{site.data.keyword.speechtotextshort}}-Service.


## Was bedeutet 'Preis pro Minute'?
{: #faq-pricing-two}
{: faq}

Grundlage für den Preis ist der Umfang (Anzahl der Minuten) der Audiodaten, die an den Service gesendet werden, und nicht die Zeit, die der Service benötigt, um die Audiodaten zu verarbeiten.

## Wird bei jedem Aufruf der API auf die nächste Minute aufgerundet? 
{: #faq-pricing-three}
{: faq}

{{site.data.keyword.IBM_notm}} rundet die Länge der Audiodaten für jeden vom Service empfangenen API-Aufruf nicht auf. {{site.data.keyword.IBM_notm}} summiert stattdessen die gesamte monatliche Nutzung und rundet am Monatsende auf die nächste Minute auf. Dies bedeutet beispielsweise, dass {{site.data.keyword.IBM_notm}} bei zwei gesendeten Audiodateien mit einer Länge von jeweils 30 Sekunden die Dauer der gesamten Audiodaten auf eine Minute summiert und $0,02 (USD) in Rechnung stellt.

## Wie funktioniert die nutzungsvolumenbasierte gestaffelte Preisgestaltung?
{: #faq-pricing-four}
{: faq}

Das Modell der gestaffelten Preisgestaltung dient dazu, Benutzern mit hohem Nutzungsvolumen bei fortlaufender Verwendung des Service Rabatte zu gewähren. Sobald bestimmte Schwellenwerte für die monatliche Summe der Audiodaten erreicht werden, wird der minutengenaue Preistarif für weitere Minuten von Audiodaten reduziert. Weitere Informationen finden Sie auf der [Seite für die Preisstruktur](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} für den Service.

## Wie hoch ist bei der gestaffelten Preisgestaltung meine Gesamtgebühr, wenn ich beispielsweise 275.000 Minuten Audiodaten in einem Monat durch den Service transkribieren lasse?
{: #faq-pricing-five}
{: faq}

Für die ersten 250.000 Minuten Audiodaten werden Ihnen in diesem Fall $0,02 (USD) pro Minute berechnet, also 250.000 \* $0,02 = $5000,00 (USD). Für die restlichen 25.000 Minuten Audiodaten wird Ihnen der reduzierte Tarif von $0,015 (USD) pro Minute in Rechnung gestellt, also 25.000 \* $0,015 = $375,00 (USD). Die Gesamtgebühr für den Monat läge in diesem Fall bei $5375,00 (USD).

## Welchen Preisplan benötige ich, um die Anpassungsschnittstelle des Service zu verwenden?
{: #faq-pricing-six}
{: faq}

Um das Sprachmodell oder die akustische Modellanpassung verwenden zu können, müssen Sie über den Standard-Preistarif verfügen. Benutzer des Lite-Plans können die Anpassungsschnittstelle nicht verwenden. Weitere Informationen finden Sie auf der [Seite für die Preisstruktur](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} für den {{site.data.keyword.speechtotextshort}}-Service.


## Was kostet die Nutzung der Anpassungsschnittstelle des Service?
{: #faq-pricing-seven}
{: faq}

Bei der *Sprachmodellanpassung* berechnet {{site.data.keyword.IBM_notm}} keine Gebühren für die Erstellung oder das Hosting eines angepassten Sprachmodells, sondern lediglich für die Verwendung des Modells bei einer Erkennungsanforderung. Für die Nutzung eines angepassten Sprachmodells für die Transkription fällt eine zusätzliche Gebühr in Höhe von $0,03 (USD) pro Minute an. Diese Gebühr wird zusätzlich zur Standardnutzungsgebühr von $0,02 (USD) pro Minute in Rechnung gestellt. Sie gilt für alle von der Anpassungsschnittstelle unterstützten Sprachen. Die Gesamtgebühr für die Verwendung eines angepassten Sprachmodells bei der Spracherkennung beträgt somit $0,05 (USD) pro Minute.

Bei der *Akustikmodellanpassung*, die für alle unterstützten Sprachen als Betaschnittstelle verfügbar ist, berechnet {{site.data.keyword.IBM_notm}} keine Gebühren für die Erstellung, das Hosting oder die Spracherkennung mit einem angepassten Akustikmodell. Änderungen an der kostenlosen Nutzung von angepassten Akustikmodellen für Erkennungsanforderungen bleiben in der Zukunft vorbehalten.
