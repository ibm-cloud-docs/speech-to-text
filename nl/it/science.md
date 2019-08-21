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

# La scienza dietro il servizio
{: #science}

Come descritto in [Pioneering Speech Recognition](https://www.ibm.com/ibm/history/ibm100/us/en/icons/speechreco/){: external}, {{site.data.keyword.IBM}} è stata in prima linea nella ricerca nel campo del riconoscimento vocale sin dai primi anni '60. Ad esempio, il documento di [Bahl, Jelinek e Mercer (1983)](/docs/services/speech-to-text?topic=speech-to-text-references#bahl1983) descrive l'approccio matematico di base al riconoscimento vocale utilizzato essenzialmente in tutti i moderni sistemi di riconoscimento vocale. E il documento di [Jelinek (1985)](/docs/services/speech-to-text?topic=speech-to-text-references#jelinek1985) descrive la creazione del primo sistema di riconoscimento vocale con un ampio vocabolario e in tempo reale per la dettatura. Questo documento descrive anche i problemi che sono ancora oggi argomenti di ricerca irrisolti.
{: shortdesc}

{{site.data.keyword.IBM_notm}} continua questa ricca tradizione di ricerca e sviluppo con il servizio {{site.data.keyword.speechtotextfull}}. {{site.data.keyword.IBM_notm}} ha dimostrato l'accuratezza del riconoscimento vocale da record del settore sui set di dati di benchmark pubblici per la trascrizione di Conversational Telephone Speech (CTS) ([Saon e altri, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#saon2017)) e Broadcast News (BN) ([Thomas e altri, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#thomas2019)). {{site.data.keyword.IBM_notm}} ha sfruttato le reti neurali per la modellazione della lingua ([Kurata e altri, 2017a](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a) e [Kurata e altri, 2017b](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a)), oltre a dimostrare l'efficacia della modellazione acustica.

I seguenti annunci riepilogano i recenti risultati del riconoscimento vocale di {{site.data.keyword.IBM_notm}}:

-   [Reaching new records in speech recognition](https://www.ibm.com/blogs/watson/2017/03/reaching-new-records-in-speech-recognition/){: external}
-   [{{site.data.keyword.IBM_notm}} Breaks Industry Record for Conversational Speech Recognition by Extending Deep Learning Technologies](https://www-03.ibm.com/press/us/en/pressrelease/51790.wss){: external}
-   [{{site.data.keyword.IBM_notm}} Sets New Transcription Performance Milestone on Automatic Broadcast News Captioning](https://www.ibm.com/blogs/research/2019/05/automatic-broadcast-news-captioning/){: external}

Questi risultati contribuiscono a far avanzare ulteriormente i servizi vocali di {{site.data.keyword.IBM_notm}}. Le idee recenti che meglio si adattano al servizio {{site.data.keyword.speechtotextshort}} basato sul cloud includono

-   *Per la modellazione della lingua,* {{site.data.keyword.IBM_notm}} sfrutta un modello di lingua basato su rete neurale per generare il testo di addestramento ([Suzuki e altri, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#suzuki2019)).
-   *Per la modellazione acustica,* {{site.data.keyword.IBM_notm}} utilizza un modello abbastanza compatto per adeguarsi alle limitazioni delle risorse del cloud. Per addestrare questo modello compatto, {{site.data.keyword.IBM_notm}} utilizza il sistema di "addestramento insegnante-studente / distillazione della conoscenza". Vengono per prima addestrate reti neurali grandi e potenti come LSTM (Long Short Term Memory), VGG e Residual Network (ResNet). L'output di queste reti viene quindi utilizzato come segnale dell'insegnante per addestrare un modello compatto per la distribuzione effettiva ([Fukuda e altri, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#fukuda2017)).

Per spingere ulteriormente, {{site.data.keyword.IBM_notm}} si concentra anche sulla modellazione end-to-end. Ad esempio, ha stabilito una solida pipeline di modellazione per i modelli diretti da acustica a parola ([Audhkhasi e altri, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2017) e [Audhkhasi e altri, 2018](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2018)) che sta migliorando ulteriormente ([Saon e altri, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#saon2019)). Si sta inoltre impegnando a creare modelli end-to-end compatti per la futura distribuzione sul cloud ([Kurata e Audhkhasi, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2019)).

Per ulteriori informazioni sulla ricerca scientifica alla base del servizio, consulta i documenti elencati in [Riferimenti di ricerca](/docs/services/speech-to-text?topic=speech-to-text-references).
