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

# Perguntas frequentes sobre precificação
{: #faq-pricing}

## Qual é o preço para usar o plano Lite do {{site.data.keyword.speechtotextshort}}?
{: #faq-pricing-zero}
{: faq}

O plano Lite permite que você inicie com 500 minutos por mês de reconhecimento de voz sem custo. O plano Lite não fornece acesso às interfaces de customização do serviço. Para obter mais informações, consulte a [página de precificação](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} para o serviço {{site.data.keyword.speechtotextshort}}.

## Qual é o preço para usar o plano Standard do {{site.data.keyword.speechtotextshort}}?
{: #faq-pricing-one}
{: faq}

O plano de precificação Standard é precificado a US$ 0,02 por minuto de fala reconhecida. Esse preço se aplica ao uso dos modelos de banda larga e de banda estreita. O plano usa um modelo de precificação em camadas e permite que você use as interfaces de customização por um encargo adicional. Para obter mais informações, consulte a [página de precificação](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} para o serviço {{site.data.keyword.speechtotextshort}}.

## O que significa "precificação por minuto"?
{: #faq-pricing-two}
{: faq}

O preço é baseado na quantia (número de minutos) de áudio que você envia para o serviço. O preço não depende de quanto tempo o serviço leva para processar o áudio.

## Você arredonda para o minuto mais próximo para cada chamada para a API?
{: #faq-pricing-three}
{: faq}

O {{site.data.keyword.IBM_notm}} não arredonda o comprimento do áudio para cada chamada de API que o serviço recebe. Em vez disso, o {{site.data.keyword.IBM_notm}} agrega todos os usos para o mês e arredonda para o minuto mais próximo no final do mês. Por exemplo, se você enviar dois arquivos de áudio com 30 segundos de duração cada um, a {{site.data.keyword.IBM_notm}} somará a duração do áudio total para um minuto e cobrará US$0,02.

## Como funciona a precificação em camadas graduadas baseada em volume?
{: #faq-pricing-four}
{: faq}

O modelo de precificação em camadas é destinado a fornecer aos usuários de alto volume descontos adicionais à medida que eles continuam a usar o serviço. A precificação por minuto é reduzida para minutos extras de áudio quando determinados limites para o total de áudio mensal são atendidos. Para obter mais informações, consulte a [página de precificação](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} para o serviço.

## Para precificação em camadas, qual seria a minha carga total se eu usasse o serviço para transcrever, por exemplo, 275 mil minutos de áudio em um mês?
{: #faq-pricing-five}
{: faq}

Para os primeiros 250 mil minutos de áudio, seria cobrado US$ 0,02 / minuto: 250.000 \* US$0,02 = US$ 5000,00. Para os restantes 25 mil minutos de áudio, seria cobrada a taxa reduzida de US$ 0,015 / minuto: 25.000 \* US$ 0,015 = US$ 375,00. Nesse caso, seu encargo total para o mês seria US$ 5375,00.

## Qual plano de precificação eu preciso para usar a interface de customização do serviço?
{: #faq-pricing-six}
{: faq}

Deve-se ter o plano de precificação Standard para usar a customização do modelo de idioma ou modelo acústico. Os usuários do plano Lite não podem usar a interface de customização. Para obter mais informações, consulte a [página de precificação](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} para o serviço {{site.data.keyword.speechtotextshort}}.

## Qual é o preço para usar a interface de customização do serviço?
{: #faq-pricing-seven}
{: faq}

Para a *customização de modelo de idioma*, a {{site.data.keyword.IBM_notm}} não cobra para criar ou hospedar um modelo de idioma customizado, apenas para usar o modelo com uma solicitação de reconhecimento. Usar um modelo de idioma customizado para transcrição incorre em um encargo de complemento de US$ 0,03 por minuto. Esse encargo é além do encargo de uso padrão de US$ 0,02 por minuto. Ele se aplica a todos os idiomas suportados pela interface de customização. Portanto, o total de encargos para usar um modelo de idioma customizado para reconhecimento de voz é US$ 0,05 por minuto.

Para a *customização de modelo acústico*, que é uma interface beta para todos os idiomas suportados, a {{site.data.keyword.IBM_notm}} não cobra pela criação, pela hospedagem ou pelo reconhecimento de voz com um modelo acústico customizado. O uso grátis de modelos acústicos customizados para solicitações de reconhecimento está sujeito a mudar no futuro.
