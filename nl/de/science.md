---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-03"

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

# Der wissenschaftliche Hintergrund für den Service
{: #science}

{{site.data.keyword.IBM}} ist schon seit den frühen 1960er Jahren ein Wegbereiter für die Forschung im Bereich der Spracherkennung (siehe [Pioneering Speech Recognition](https://www.ibm.com/ibm/history/ibm100/us/en/icons/speechreco/){: external}. Beispielsweise beschreiben [Bahl, Jelinek und Mercer (1983)](/docs/services/speech-to-text?topic=speech-to-text-references#bahl1983) den grundlegenden mathematischen Ansatz für die Spracherkennung, der von fast allen modernen Spracherkennungssystemen verwendet wird. Und die Veröffentlichung von [Jelinek (1985)](/docs/services/speech-to-text?topic=speech-to-text-references#jelinek1985) beschreibt die Erstellung des ersten Echtzeit-Spracherkennungssystems für umfangreiche Wörterbestände aus Diktaten. Außerdem werden Probleme erläutert, die auch beim heutigen Stand der Forschung weiterhin ungelöst sind.
{: shortdesc}

Diese Tradition der Forschung und Entwicklung setzt {{site.data.keyword.IBM_notm}} mit dem {{site.data.keyword.speechtotextfull}}-Service fort. {{site.data.keyword.IBM_notm}} hat eine Spracherkennungsgenauigkeit bei den öffentlichen Vergleichsdaten für Conversational Telephone Speech (CTS) ([Saon und andere, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#saon2017)) und Broadcast News-Transkription (BN) ([Thomas und andere, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#thomas2019)) demonstriert, die einen Branchenrekord darstellt. {{site.data.keyword.IBM_notm}} hat nicht nur die Effektivität akustischer Modellierung demonstriert, sondern auch neuronale Netze für die Sprachmodellierung eingesetzt ([Kurata und andere, 2017a](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a) und [Kurata und andere, 2017b](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a)).

Die folgenden Veröffentlichungen fassen die jüngsten Errungenschaften von {{site.data.keyword.IBM_notm}} im Bereich der Spracherkennung zusammen:

-   [Reaching new records in speech recognition](https://www.ibm.com/blogs/watson/2017/03/reaching-new-records-in-speech-recognition/){: external}
-   [{{site.data.keyword.IBM_notm}} Breaks Industry Record for Conversational Speech Recognition by Extending Deep Learning Technologies](https://www-03.ibm.com/press/us/en/pressrelease/51790.wss){: external}
-   [{{site.data.keyword.IBM_notm}} Sets New Transcription Performance Milestone on Automatic Broadcast News Captioning](https://www.ibm.com/blogs/research/2019/05/automatic-broadcast-news-captioning/){: external}

Diese Errungenschaften tragen zur weiteren Entwicklung der Speech-Services von {{site.data.keyword.IBM_notm}} bei. Aktuelle Ideen, die dem cloudbasierten {{site.data.keyword.speechtotextshort}}-Service am besten entsprechen:

-   *Für die Sprachmodellierung* nutzt {{site.data.keyword.IBM_notm}} ein neuronales netzbasiertes Sprachmodell, um Trainingstexte zu generieren ([Suzuki und andere, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#suzuki2019)).
-   *Für die akustische Modellierung* verwendet {{site.data.keyword.IBM_notm}} wegen der Ressourcenbeschränkungen der Cloud ein relativ kompaktes Modell. Für das Training dieses kompakten Modells verwendet {{site.data.keyword.IBM_notm}} "Teacher-Student Training/Knowledge Distillation", d. h. Training in Form von Lehrer und Schüler sowie Destillation von Wissen. Große und robuste neuronale Netze wie Long Short-Term Memory (LSTM), VGG und Residual Netzwerk (ResNet) werden zunächst trainiert. Die Ausgabe dieser Netze wird dann als Lehrersignale verwendet, um ein kompaktes Modell für die tatsächliche Bereitstellung zu trainieren ([Fukuda und andere, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#fukuda2017)).

Darüber hinaus konzentriert sich {{site.data.keyword.IBM_notm}} zusätzlich auf End-to-End-Modellierung. Beispielsweise wurde eine leistungsstarke Modellierungspipeline für direkte Akustik-Wort-Modelle ([Audhkhasi und andere, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2017) und [Audhkhasi und andere, 2018](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2018)) eingerichtet, die derzeit weiter verbessert wird ([Saon und andere, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#saon2019)). IBM ist zudem bestrebt, kompakte End-to-End-Modelle für die zukünftige Entwicklung in der Cloud ([Kurata und Audhkhasi, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2019)) zu erstellen.

Weitere Informationen zu der wissenschaftlichen Forschung, die hinter dem Service steht, finden Sie in den Dokumenten, die im Abschnitt [Referenzinformationen zur Forschung](/docs/services/speech-to-text?topic=speech-to-text-references) aufgelistet sind.
