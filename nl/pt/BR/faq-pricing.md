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

# Perguntas frequentes sobre precificação
{: #faq-pricing}

1.  <span style="color:#003F69">Qual é o preço para usar o serviço {{site.data.keyword.speechtotextshort}} padrão do {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}}?</span>

    O serviço {{site.data.keyword.speechtotextshort}} é precificado em US$ 0,02 por minuto. Esse preço se aplica ao uso dos modelos de banda larga e de banda estreita. Para obter mais informações, consulte a página de entrada do serviço {{site.data.keyword.speechtotextshort}} [ ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/watson/developercloud/speech-to-text.html#pricing-block){: new_window}.

1.  <span style="color:#003F69">O que significa "precificação por minuto"? Vocês cobram pelo número de minutos de áudio que eu envio para o serviço ou o número de minutos que o serviço leva para processar o áudio?</span>

    O preço é baseado na quantia de áudio que é enviada para o serviço, não no tempo que o serviço leva para processar o áudio.

1.  <span style="color:#003F69">Vocês arredondam para o minuto mais próximo para cada chamada de API? Por exemplo, se eu enviar dois arquivos de áudio com 30 segundos de comprimento cada, serei cobrado por dois minutos ou por um minuto?</span>

    O {{site.data.keyword.IBM_notm}} não arredonda o comprimento do áudio para cada chamada de API que o serviço recebe. Em vez disso, o {{site.data.keyword.IBM_notm}} agrega todos os usos para o mês e arredonda para o minuto mais próximo no final do mês. Nesse exemplo, se você enviar dois arquivos de áudio de 30 segundos de comprimento cada, o {{site.data.keyword.IBM_notm}} somará a duração total do áudio a um minuto e cobrará US$ 0,02.

1.  <span id="graduated" style="color:#003F69">Como funciona a precificação em camadas graduadas baseada em volume?</span>

    O modelo de precificação em camadas é destinado a fornecer aos usuários de alto volume descontos adicionais à medida que eles continuam a usar o serviço. A precificação por minuto é reduzida para minutos extras de áudio quando determinados limites para o total de áudio mensal são atendidos. Para obter mais informações, consulte a página de precificação do [ ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/catalog/services/speech-to-text){: new_window} para o serviço.

1.  <span style="color:#003F69">Para a precificação em camadas, qual seria o total de encargos se eu utilizasse o serviço para transcrever, por exemplo, 275 mil minutos de áudio em um mês?</span>

    -   Para os primeiros 250 mil minutos de áudio, você seria cobrado a US$ 0,02/minuto: 250.000 * US$ 0,02 = US$ 5.000,00.
    -   Para os restantes 25 mil minutos de áudio, você seria cobrado pela taxa reduzida de US$ 0,015/minuto: 25.000 * US$ 0,015 = US$ 375,00.
    -   Nesse cenário, seu total de encargos para o mês seria US$ 5.375,00.

1.  <span style="color:#003F69">Qual é o preço para usar a interface de customização do serviço? Sou cobrado pela criação e armazenamento de um modelo customizado?</span>

    Para a *customização de modelo de idioma*, a {{site.data.keyword.IBM_notm}} não cobra para criar ou hospedar um modelo de idioma customizado, apenas para usar o modelo com uma solicitação de reconhecimento. Usar um modelo de idioma customizado para transcrição incorre em um encargo de complemento de US$ 0,03 por minuto. Esse encargo é além do encargo de uso padrão de US$ 0,02 por minuto. Ele se aplica a todos os idiomas suportados pela interface de customização. Portanto, o total de encargos para usar um modelo de idioma customizado para reconhecimento de voz é US$ 0,05 por minuto.

    Para a *customização de modelo acústico*, que é uma interface beta para todos os idiomas suportados, a {{site.data.keyword.IBM_notm}} não cobra pela criação, pela hospedagem ou pelo reconhecimento de voz com um modelo acústico customizado. O uso grátis de modelos acústicos customizados para solicitações de reconhecimento está sujeito a mudar no futuro.
