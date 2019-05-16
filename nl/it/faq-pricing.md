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

# Domande frequenti sui prezzi
{: #faq-pricing}

1.  <span style="color:#003F69">Qual è il prezzo per l'utilizzo del servizio standard {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}}?</span>

    Il servizio {{site.data.keyword.speechtotextshort}} ha un prezzo di $0,02 (USD) al minuto. Questo prezzo si applica all'uso di modelli sia a banda larga che a banda stretta. Per ulteriori informazioni, vedi la [pagina di destinazione del servizio ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developercloud/speech-to-text.html#pricing-block){: new_window} {{site.data.keyword.speechtotextshort}}.

1.  <span style="color:#003F69">Cosa significa "prezzo al minuto"? Mi viene addebitato il numero di minuti di audio che invio al servizio o il numero di minuti che impiega il servizio per elaborare l'audio?</span>

    Il prezzo si basa sulla quantità di audio che viene inviata al servizio, non sul tempo che impiega il servizio per elaborare l'audio.

1.  <span style="color:#003F69">Il tempo viene arrotondato al minuto più vicino per ogni chiamata all'API? Ad esempio, se invio due file audio lunghi 30 secondi ciascuno, mi vengono addebitati due minuti o un minuto?</span>

    {{site.data.keyword.IBM_notm}} non arrotonda la lunghezza dell'audio per ogni chiamata API ricevuta dal servizio. Invece, {{site.data.keyword.IBM_notm}} aggrega tutto l'utilizzo per il mese e arrotonda al minuto più vicino alla fine del mese. In questo esempio, se invii due file audio che sono lunghi 30 secondi ciascuno, {{site.data.keyword.IBM_notm}} somma la durata dell'audio totale a un minuto e addebita $0,02 (USD).

1.  <span id="graduated" style="color:#003F69">Come funziona il prezzo a più livelli graduati basato sul volume?</span>

    Il modello di prezzo a più livelli ha lo scopo di offrire agli utenti ad alto volume ulteriori sconti man mano che continuano a utilizzare il servizio. Il prezzo al minuto viene ridotto per i minuti di audio supplementari quando vengono soddisfatte determinate soglie per l'audio mensile totale. Per ulteriori informazioni, vedi la [pagina dei prezzi ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/catalog/services/speech-to-text){: new_window} per il servizio.

1.  <span style="color:#003F69">Per il prezzo a più livelli, quale sarebbe il mio addebito totale se utilizzassi il servizio per trascrivere, ad esempio, 275 mila minuti di audio in un mese?</span>

    -   Per i primi 250 mila minuti di audio, ti verrebbero addebitati $0,02 (USD) / minuto: 250.000 * $0,02 = $5000,00 (USD).
    -   Per i restanti 25 mila minuti di audio, ti verrebbe addebitato il costo ridotto di $0,015 (USD) / minuto: 25.000 * $0,015 = $375,00 (USD).
    -   In questo scenario, il tuo addebito totale per il mese sarebbe di $5375,00 (USD).

1.  <span style="color:#003F69">Qual è il prezzo per l'utilizzo dell'interfaccia di personalizzazione del servizio? Mi viene addebitato un costo per la creazione e la memorizzazione di un modello personalizzato?</span>

    Per la *personalizzazione del modello di lingua*, {{site.data.keyword.IBM_notm}} non addebita alcun costo per la creazione o l'hosting di un modello di lingua personalizzato; addebita un costo solo per l'utilizzo del modello con una richiesta di riconoscimento. L'utilizzo di un modello di lingua personalizzato per la trascrizione comporta un addebito aggiuntivo di $0,03 (USD) al minuto. Questo addebito è in aggiunta al costo di utilizzo standard di $0,02 (USD) al minuto. Si applica a tutte le lingue supportate dall'interfaccia di personalizzazione. Quindi il costo totale per l'utilizzo di un modello di lingua personalizzato per il riconoscimento vocale è di $ 0,05 (USD) al minuto.

    Per la *personalizzazione del modello acustico*, che è un'interfaccia beta per tutte le lingue supportate, {{site.data.keyword.IBM_notm}} non addebita alcun costo per la creazione, l'hosting o il riconoscimento vocale con un modello acustico personalizzato. L'utilizzo gratuito di modelli acustici personalizzati per le richieste di riconoscimento è soggetto a modifiche in futuro.
