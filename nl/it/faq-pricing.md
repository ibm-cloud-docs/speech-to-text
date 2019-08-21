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

# Domande frequenti sui prezzi
{: #faq-pricing}

## Qual è il prezzo per l'utilizzo del piano Lite di {{site.data.keyword.speechtotextshort}}?
{: #faq-pricing-zero}
{: faq}

Il piano Lite ti consente di iniziare con 500 minuti al mese di riconoscimento vocale senza alcun costo. Il piano Lite non fornisce l'accesso alle interfacce di personalizzazione del servizio. Per ulteriori informazioni, vedi la [pagina dei prezzi](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} per il servizio {{site.data.keyword.speechtotextshort}}.

## Qual è il prezzo per l'utilizzo del piano Standard di {{site.data.keyword.speechtotextshort}}?
{: #faq-pricing-one}
{: faq}

Il piano Standard ha un prezzo di $0,02 (USD) al minuto di discorso che riconosci. Questo prezzo si applica all'uso di modelli sia a banda larga che a banda stretta. Il piano utilizza un modello di prezzo a più livelli e ti consente di utilizzare le interfacce di personalizzazione a un costo aggiuntivo. Per ulteriori informazioni, vedi la [pagina dei prezzi](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} per il servizio {{site.data.keyword.speechtotextshort}}.

## Cosa significa "prezzo al minuto"?
{: #faq-pricing-two}
{: faq}

Il prezzo si basa sulla quantità (numero di minuti) di audio che invii al servizio. Il prezzo non dipende da quanto tempo impiega il servizio per elaborare l'audio.

## Il tempo viene arrotondato al minuto più vicino per ogni chiamata all'API?
{: #faq-pricing-three}
{: faq}

{{site.data.keyword.IBM_notm}} non arrotonda la lunghezza dell'audio per ogni chiamata API ricevuta dal servizio. Invece, {{site.data.keyword.IBM_notm}} aggrega tutto l'utilizzo per il mese e arrotonda al minuto più vicino alla fine del mese. Ad esempio, se invii due file audio che sono lunghi 30 secondi ciascuno, {{site.data.keyword.IBM_notm}} somma la durata dell'audio totale a un minuto e addebita $0,02 (USD).

## Come funziona il prezzo a più livelli graduati basato sul volume?
{: #faq-pricing-four}
{: faq}

Il modello di prezzo a più livelli ha lo scopo di offrire agli utenti ad alto volume ulteriori sconti man mano che continuano a utilizzare il servizio. Il prezzo al minuto viene ridotto per i minuti di audio supplementari quando vengono soddisfatte determinate soglie per l'audio mensile totale. Per ulteriori informazioni, vedi la [pagina dei prezzi](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} per il servizio.

## Per il prezzo a più livelli, quale sarebbe il mio addebito totale se utilizzassi il servizio per trascrivere, ad esempio, 275 mila minuti di audio in un mese?
{: #faq-pricing-five}
{: faq}

Per i primi 250 mila minuti di audio, ti verrebbero addebitati $0,02 (USD) / minuto: 250.000 \* $0,02 = $5000,00 (USD). Per i restanti 25 mila minuti di audio, ti verrebbe addebitato il costo ridotto di $0,015 (USD) / minuto: 25.000 \* $0,015 = $375,00 (USD). In questo caso, il tuo addebito totale per il mese sarebbe di $5375,00 (USD).

## Quale piano prezzi devo avere per utilizzare l'interfaccia di personalizzazione del servizio?
{: #faq-pricing-six}
{: faq}

Per utilizzare la personalizzazione del modello di lingua o del modello acustico, devi disporre del piano prezzi Standard. Gli utenti del piano Lite non possono utilizzare l'interfaccia di personalizzazione. Per ulteriori informazioni, vedi la [pagina dei prezzi](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} per il servizio {{site.data.keyword.speechtotextshort}}.

## Qual è il prezzo per l'utilizzo dell'interfaccia di personalizzazione del servizio?
{: #faq-pricing-seven}
{: faq}

Per la *personalizzazione del modello di lingua*, {{site.data.keyword.IBM_notm}} non addebita alcun costo per la creazione o l'hosting di un modello di lingua personalizzato; addebita un costo solo per l'utilizzo del modello con una richiesta di riconoscimento. L'utilizzo di un modello di lingua personalizzato per la trascrizione comporta un addebito aggiuntivo di $0,03 (USD) al minuto. Questo addebito è in aggiunta al costo di utilizzo standard di $0,02 (USD) al minuto. Si applica a tutte le lingue supportate dall'interfaccia di personalizzazione. Quindi il costo totale per l'utilizzo di un modello di lingua personalizzato per il riconoscimento vocale è di $ 0,05 (USD) al minuto.

Per la *personalizzazione del modello acustico*, che è un'interfaccia beta per tutte le lingue supportate, {{site.data.keyword.IBM_notm}} non addebita alcun costo per la creazione, l'hosting o il riconoscimento vocale con un modello acustico personalizzato. L'utilizzo gratuito di modelli acustici personalizzati per le richieste di riconoscimento è soggetto a modifiche in futuro.
