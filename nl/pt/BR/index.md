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

# Sobre
{: #about}

**Atualização de serviço:***O serviço {{site.data.keyword.speechtotextshort}} foi atualizado em 30 de julho de 2019. Agora, o serviço oferece modelos beta para cinco dialetos espanhóis adicionais: argentino, chileno, colombiano, mexicano e peruano. Para obter mais informações sobre os novos dialetos espanhóis, consulte a [Atualização de serviço de 30 de julho de 2019](/docs/services/speech-to-text?topic=speech-to-text-release-notes#July2019) nas notas sobre a liberação.*

O serviço {{site.data.keyword.speechtotextfull}} fornece recursos de transcrição de fala para seus aplicativos. O serviço aproveita o aprendizado de máquina para combinar o conhecimento da gramática, a estrutura do idioma e a composição de sinais de áudio e voz para transcrever com precisão a voz humana. Ele atualiza continuamente e refina sua transcrição à medida que recebe mais fala.
{: shortdesc}

O serviço fornece várias interfaces adequadas para qualquer aplicativo em que a fala é a entrada e uma transcrição textual é a saída. Exemplos de aplicativos incluem

-   Controle de voz de aplicativos, dispositivos integrados e acessórios de veículo
-   Transcrições de reuniões e conferências telefônicas
-   Ditado de notas e mensagens de e-mail

O serviço é ideal para clientes que precisam extrair transcrições de fala de alta qualidade do áudio da central de atendimento. Clientes em segmentos de mercado como serviços financeiros, assistência médica, seguro e telecomunicações podem desenvolver aplicativos nativos em nuvem para atendimento ao cliente, voz do cliente, assistência do agente e outras soluções.

## Interfaces Suportadas
{: #interfaces}

O serviço {{site.data.keyword.speechtotextshort}} oferece três interfaces para reconhecimento de voz:

-   Uma [interface do WebSocket](/docs/services/speech-to-text?topic=speech-to-text-websockets) para estabelecer conexões persistentes, full duplex e de baixa latência com o serviço. É possível passar um máximo de 100 MB de dados de áudio para o serviço com uma única solicitação.
-   Uma [interface de HTTP síncrona](/docs/services/speech-to-text?topic=speech-to-text-http) para chamadas HTTP básicas para o serviço. É possível passar um máximo de 100 MB de dados de áudio com uma solicitação.
-   Uma [interface de HTTP assíncrona](/docs/services/speech-to-text?topic=speech-to-text-async) para chamadas sem bloqueio para o serviço. É possível passar o máximo de 1 GB de dados de áudio com uma solicitação.

O serviço também fornece uma [interface de customização](/docs/services/speech-to-text?topic=speech-to-text-customization) que você pode usar para ajustar o reconhecimento de voz para seu idioma e requisitos acústicos. É possível expandir o vocabulário de um modelo com a terminologia específica do domínio ou adaptar um modelo para as características acústicas de seu áudio. Também é possível incluir [gramáticas](/docs/services/speech-to-text?topic=speech-to-text-grammars) para restringir as frases que o serviço pode reconhecer.

-   Para obter uma descrição de alto nível do desenvolvimento do aplicativo com o serviço, consulte [Visão geral para desenvolvedores](/docs/services/speech-to-text?topic=speech-to-text-developerOverview).
-   Para obter exemplos de solicitações de reconhecimento de voz básica com cada uma das interfaces do serviço, consulte [Fazendo uma solicitação de reconhecimento](/docs/services/speech-to-text?topic=speech-to-text-basic-request).

Os SDKs estão disponíveis em muitas linguagens de programação para simplificar seu uso do serviço. Para obter mais informações, consulte a [Referência de API](https://{DomainName}/apidocs/speech-to-text){: external}.

## Recursos de entrada
{: #inputFeatures}

As interfaces do serviço compartilham recursos de entrada comuns para transcrever a fala para texto:

-   [Formatos de áudio](/docs/services/speech-to-text?topic=speech-to-text-audio-formats) - É possível transcrever o áudio Ogg ou Web Media (WebM) com o codec do Opus ou Vorbis, MP3 (ou MPEG), Waveform Audio File Format (WAV), Free Lossless Audio Codec (FLAC), Linear 16-bit Pulse-Code Modulation (PCM), G.729, A-Law, mu-law (ou u-law) e áudio básico. Usando um formato que suporta compactação, é possível maximizar a quantia de dados de áudio que podem ser enviados com uma solicitação.
-   [Idiomas e modelos](/docs/services/speech-to-text?topic=speech-to-text-models) - Para a maioria dos idiomas, é possível transcrever áudio usando modelos de banda larga ou de banda estreita. Use a banda larga para áudio amostrado a uma taxa mínima de 16 kHz. Use uma banda estreita para áudio amostrado em uma taxa mínima de 8 kHz.
-   [Transmissão de áudio](/docs/services/speech-to-text?topic=speech-to-text-input#transmission) - É possível passar áudio como um fluxo contínuo de chunks de dados ou como uma entrega única que passa todos os dados de uma vez. Com o fluxo, o serviço força a inatividade e os [tempos limites de sessão](/docs/services/speech-to-text?topic=speech-to-text-input#timeouts)da sessão.

## Recursos de saída
{: #outputFeatures}

As interfaces também suportam os recursos de saída comuns a seguir:

-   [Rótulos do falante](/docs/services/speech-to-text?topic=speech-to-text-output#speaker_labels) reconhecem diferentes falantes por meio de áudio em inglês dos EUA, inglês do Reino Unido, espanhol ou japonês. A transcrição rotula as contribuições de cada falante em uma conversa com múltiplos participantes. (Funcionalidade beta.)
-   [Marcação de palavra-chave](/docs/services/speech-to-text?topic=speech-to-text-output#keyword_spotting) identifica frases faladas que correspondem às sequências de palavras-chave especificadas com um nível de confiança definido pelo usuário. A marcação de palavra-chave é especialmente útil quando frases individuais do áudio são mais importantes do que a transcrição completa. Por exemplo, um sistema de suporte ao cliente pode identificar palavras-chave para determinar como rotear as solicitações do usuário.
-   [Resultados provisórios](/docs/services/speech-to-text?topic=speech-to-text-output#interim) retornam hipóteses progressivas à medida que a transcrição progride. O serviço retorna os resultados finais quando a transcrição é concluída. Os resultados provisórios estão disponíveis somente com a interface do WebSocket.
-   [Alternativas máximas](/docs/services/speech-to-text?topic=speech-to-text-output#max_alternatives) fornecem possíveis transcrições alternativas. O serviço indica os resultados finais nos quais ele tem a maior confiança.
-   [Alternativas de palavra](/docs/services/speech-to-text?topic=speech-to-text-output#word_alternatives) solicitam palavras alternativas que são acusticamente semelhantes às palavras de uma transcrição.
-   [Confiança de palavra](/docs/services/speech-to-text?topic=speech-to-text-output#word_confidence) retorna os níveis de confiança para cada palavra de uma transcrição.
-   [Registros de data e hora da palavra](/docs/services/speech-to-text?topic=speech-to-text-output#word_timestamps) retornam registros de data e hora para o início e o término de cada palavra de uma transcrição.
-   [Formatação inteligente](/docs/services/speech-to-text?topic=speech-to-text-output#smart_formatting) converte datas, horas, números, valores de moeda, números de telefone e endereços da Internet em formulários mais legíveis e convencionais nas transcrições finais. Para inglês dos EUA, também é possível fornecer frases de palavra-chave para incluir determinados símbolos de pontuação em transcrições finais. A formatação inteligente é suportada para áudio em inglês dos EUA, japonês e espanhol. (Funcionalidade beta.)
-   [A edição de dados numéricos](/docs/services/speech-to-text?topic=speech-to-text-output#redaction) edita, ou mascara, dados numéricos de uma transcrição final. A edição de dados destina-se a remover das transcrições as informações pessoais sensíveis, como números de cartão de crédito. O recurso é suportado para áudio em inglês dos EUA, japonês e coreano. (Funcionalidade beta.)
-   [Filtragem de profanidade](/docs/services/speech-to-text?topic=speech-to-text-output#profanity_filter) censura a profanidade da transcrições em inglês dos EUA.
-   As [Métricas de processamento](/docs/services/speech-to-text?topic=speech-to-text-metrics#processing_metrics) fornecem informações de sincronização detalhadas sobre a análise do áudio de entrada do serviço.
-   As [Métricas de áudio](/docs/services/speech-to-text?topic=speech-to-text-metrics#audio_metrics) fornecem informações detalhadas sobre as características de sinal do áudio de entrada.

## Suporte ao idioma
{: #languages}

O serviço oferece modelos para os seguintes idiomas e dialetos:

-   Árabe (padrão moderno)
-   Português do Brasil
-   Chinês (mandarim)
-   Inglês (Reino Unido e Estados Unidos)
-   Francês
-   Alemão
-   Japonês
-   Coreano
-   Espanhol (argentino, castelhano, chileno, colombiano, mexicano e peruano)

O serviço não suporta todos os recursos para todos os idiomas. Além disso, ele suporta alguns recursos como geralmente disponíveis (GA) para uso de produção e outros como ofertas beta para diferentes idiomas.

-   O dialeto espanhol castelhano está geralmente disponível. Os outros cinco dialetos espanhóis são beta.
-   As interfaces HTTP e do WebSocket estão geralmente disponíveis para todos os idiomas.
-   O serviço oferece modelos de banda larga, modelos de banda estreita, ou ambos para idiomas diferentes. Para obter mais informações, consulte [Idiomas e modelos](/docs/services/speech-to-text?topic=speech-to-text-models).
-   Alguns recursos de reconhecimento de voz estão disponíveis apenas para alguns idiomas. Para obter mais informações, consulte o [Resumo de parâmetro](/docs/services/speech-to-text?topic=speech-to-text-summary).
-   A interface de customização do modelo de idioma geralmente está disponível para a maioria dos idiomas. A interface de customização do modelo acústico está disponível como funcionalidade beta para todos os idiomas. Para obter mais informações, consulte [Suporte ao idioma para customização](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport).

## Precificação
{: #pricing-index}

Para obter mais informações sobre os planos de precificação para o serviço, consulte o serviço {{site.data.keyword.speechtotextshort}} no catálogo do [{{site.data.keyword.cloud}}](https://{DomainName}/catalog/services/speech-to-text){: external}. Para obter respostas para perguntas relacionadas a precificação e exemplos de custos de uso mensais, consulte as [Perguntas frequentes sobre precificação](/docs/services/speech-to-text?topic=speech-to-text-faq-pricing).

## Experimente o serviço
{: #tryOut}

Para obter exemplos do serviço {{site.data.keyword.speechtotextshort}} em ação, consulte

-   Uma [demo rápida](https://speech-to-text-demo.ng.bluemix.net/){: external} que transcreve texto da entrada de áudio do fluxo ou de um arquivo que você transferiu por upload.
-   Os aplicativos no {{site.data.keyword.ibmwatson}}[Kits do iniciador](http://www.ibm.com/watson/developercloud/starter-kits.html){: external} que demonstram o serviço.
-   A postagem do blog do {{site.data.keyword.watson}} [Fazendo robôs atenderem: usando o serviço {{site.data.keyword.speechtotextshort}} do {{site.data.keyword.watson}}](https://www.ibm.com/blogs/watson/2016/07/getting-robots-listen-using-watsons-speech-text-service/){: external} que mostra como usar a interface WebSocket do serviço com Python para extrair a fala do áudio. O post fornece um tutorial completo que demonstra o código e as etapas envolvidos.
