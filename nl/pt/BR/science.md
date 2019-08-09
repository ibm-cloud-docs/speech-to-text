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

# A ciência por trás do serviço
{: #science}

Conforme o [Pioneering Speech Recognition](https://www.ibm.com/ibm/history/ibm100/us/en/icons/speechreco/){: external} descreve, a {{site.data.keyword.IBM}} está na vanguarda da pesquisa de reconhecimento de voz desde o início dos anos 60. Por exemplo, [Bahl, Jelinek e Mercer (1983)](/docs/services/speech-to-text?topic=speech-to-text-references#bahl1983) descrevem a abordagem matemática básica ao reconhecimento de voz que é empregado em essencialmente todos os sistemas modernos de reconhecimento de voz. E [Jelinek (1985)](/docs/services/speech-to-text?topic=speech-to-text-references#jelinek1985) descreve a criação do primeiro sistema de reconhecimento de voz de grande vocabulário em tempo real para ditado. Este documento também descreve problemas que ainda hoje são tópicos de pesquisa não resolvidos.
{: shortdesc}

A {{site.data.keyword.IBM_notm}} continua essa rica tradição de pesquisa e desenvolvimento com o serviço {{site.data.keyword.speechtotextfull}}. A {{site.data.keyword.IBM_notm}} demonstrou a precisão do reconhecimento de voz recorde do segmento de mercado nos conjuntos de dados de referência públicos para o Conversational Telephone Speech (CTS) ([Saon e outros, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#saon2017)) e a transcrição de Broadcast News (BN) ([Thomas e outros, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#thomas2019)). A {{site.data.keyword.IBM_notm}} aproveitou as redes neurais para a modelagem de idioma ([Kurata e outros, 2017a](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a) e [Kurata e outros, 2017b](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2017a)), além de demonstrar a efetividade da modelagem acústica.

Os comunicados a seguir resumem as realizações de reconhecimento de voz recentes da {{site.data.keyword.IBM_notm}}:

-   [Alcançando novos recordes no reconhecimento de voz](https://www.ibm.com/blogs/watson/2017/03/reaching-new-records-in-speech-recognition/){: external}
-   [{{site.data.keyword.IBM_notm}} quebra recorde do segmento de mercado para reconhecimento de voz de conversação ao estender as tecnologias de deep learning](https://www-03.ibm.com/press/us/en/pressrelease/51790.wss){: external}
-   [{{site.data.keyword.IBM_notm}} estabelece novo marco de desempenho de transcrição em legendas automáticas de transmissão de notícias](https://www.ibm.com/blogs/research/2019/05/automatic-broadcast-news-captioning/){: external}

Essas realizações contribuem para mais avanços nos serviços de fala da {{site.data.keyword.IBM_notm}}. As ideias recentes que melhor se ajustem ao serviço {{site.data.keyword.speechtotextshort}} baseado em nuvem incluem

-   *Para a modelagem de idioma, a *{{site.data.keyword.IBM_notm}} aproveita um modelo de idioma baseado em rede neural para gerar texto de treinamento ([Suzuki e outros, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#suzuki2019)).
-   *Para modelagem acústica, a *{{site.data.keyword.IBM_notm}} usa um modelo razoavelmente compacto para acomodar as limitações de recursos da nuvem. Para treinar esse modelo compacto, a {{site.data.keyword.IBM_notm}} usa "treinamento professor-aluno/ destilação de conhecimento". Redes neurais grandes e fortes, como a Long Short-Term Memory (LSTM), a VGG, e a Residual Network (ResNet) são treinadas pela primeira vez. A saída dessas redes é então usada como sinais do professor para treinar um modelo compacto para a implementação real ([Fukuda e outros, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#fukuda2017)).

Para ir mais além, a {{site.data.keyword.IBM_notm}} também se concentra em modelagem de ponta a ponta. Por exemplo, ela estabeleceu um forte pipeline de modelagem para modelos diretos de acústica para palavra ([Audhkhasi e outros, 2017](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2017) e [Audhkhasi e outros, 2018](/docs/services/speech-to-text?topic=speech-to-text-references#audhkhasi2018)) que agora está melhor ainda ([Saon e outros, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#saon2019)). Ela também está fazendo esforços para criar modelos de ponta a ponta compactos para futura implementação na nuvem ([Kurata e Audhkhasi, 2019](/docs/services/speech-to-text?topic=speech-to-text-references#kurata2019)).

Para obter mais informações sobre a pesquisa científica por trás do serviço, consulte os documentos listados em [Referências de pesquisa](/docs/services/speech-to-text?topic=speech-to-text-references).
