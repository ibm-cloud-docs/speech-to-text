---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-07"

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

# Visão geral para desenvolvedores
{: #developerOverview}

É possível acessar os recursos do serviço {{site.data.keyword.speechtotextfull}} usando uma interface do WebSocket e usando interfaces HTTP Representational State Transfer (REST) síncronas ou assíncronas. Também é possível customizar os modelos de idioma do serviço para se adequarem ao seu domínio e ambiente. Os SDKs estão disponíveis para simplificar o desenvolvimento de aplicativos em várias linguagens de programação.
{: shortdesc}

## Programando com o serviço
{: #programming}

[Fazendo uma solicitação de reconhecimento](/docs/services/speech-to-text/basic-request.html) mostra como solicitar transcrição básica com cada uma das interfaces de programação do serviço:

-   [A interface do WebSocket](/docs/services/speech-to-text/websockets.html) oferece uma implementação eficiente, de baixa latência e de alto rendimento por meio de uma conexão full duplex.
-   [A interface de HTTP síncrona](/docs/services/speech-to-text/http.html) fornece uma interface básica para transcrever áudio com solicitações de bloqueio.
-   [A interface de HTTP assíncrona](/docs/services/speech-to-text/async.html) fornece uma interface sem bloqueio que permite registrar uma URL de retorno de chamada para receber notificações ou para pesquisar o status e os resultados da tarefa.

As interfaces fornecem os mesmos recursos de reconhecimento de voz, mas você pode especificar o mesmo parâmetro que um cabeçalho da solicitação, um parâmetro de consulta ou um parâmetro de um objeto JSON, dependendo da interface utilizada. Além disso, as interfaces HTTP do WebSocket e síncronas aceitam um máximo de 100 MB de dados de áudio com uma única solicitação. A interface de HTTP assíncrona aceita um máximo de 1 GB de dados de áudio.

-   Para obter descrições de todos os parâmetros de reconhecimento de voz disponíveis, consulte o [Resumo de parâmetro](/docs/services/speech-to-text/summary.html).
-   Para obter descrições de todos os métodos e seus parâmetros, juntamente com exemplos, consulte a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

## Vantagens da interface do WebSocket
{: #advantages}

A interface do WebSocket tem várias vantagens sobre a interface de HTTP:

-   A interface do WebSocket fornece um canal de comunicação single socket, full duplex. A interface permite que o cliente envie solicitações e áudio para o serviço e receba resultados por meio de uma única conexão em um modo assíncrono.
-   Ela fornece uma experiência de programação muito mais simples e mais poderosa. O serviço envia respostas acionadas por evento para as mensagens do cliente, eliminando a necessidade de o cliente pesquisar o servidor.
-   Ela permite estabelecer e usar uma única conexão autenticada indefinidamente. As interfaces HTTP requerem que você autentique cada chamada para o serviço.
-   Isso reduz a latência. Os resultados do reconhecimento chegam mais rapidamente porque o serviço os envia diretamente para o cliente. A interface de HTTP requer quatro solicitações e conexões distintas para alcançar os mesmos resultados.
-   Reduz a utilização da rede. O protocolo WebSocket é leve. Ele requer apenas uma única conexão para executar reconhecimento de voz em tempo real.
-   Ele ativa o áudio a ser transmitido diretamente dos navegadores (clientes HTML5 WebSocket) para o serviço.

## Customizando o serviço
{: #customizing}

[A interface de customização](/docs/services/speech-to-text/custom.html) permite que você crie modelos customizados para melhorar os recursos de reconhecimento de voz do serviço:

-   [Modelos de idioma customizados](/docs/services/speech-to-text/language-create.html) permitem definir palavras específicas do domínio para um modelo base. Os modelos de idioma customizados expandem o vocabulário base do serviço com a terminologia específica para domínios como medicina e direito.
-   Os [modelos acústicos customizados](/docs/services/speech-to-text/acoustic-create.html) permitem que você adapte um modelo base para as características acústicas de seu ambiente e dos falantes. Os modelos acústicos customizados melhoram a capacidade do serviço de reconhecer a fala para características acústicas específicas.
-   As [gramáticas](/docs/services/speech-to-text/grammar.html) permitem que você restrinja as frases que o serviço pode reconhecer para aquelas definidas nas regras da gramática. Ao limitar o espaço de procura para sequências válidas, o serviço pode entregar resultados mais rapidamente e com mais precisão. Gramáticas são suportadas com modelos de idioma customizados.

É possível usar um modelo de idioma customizado, um modelo acústico customizado, ou ambos, para o reconhecimento de voz com qualquer uma das interfaces do serviço.

## Suporte ao CORS
{: #cors}

O serviço suporta o Cross-Origin Resource Sharing (CORS). Usando o CORS, as páginas da web podem solicitar recursos diretamente de um domínio externo. O CORS evita a política de segurança da mesma origem, que, de outra maneira, impede essas solicitações. Como o serviço suporta CORS, uma página da web pode se comunicar diretamente com o serviço sem passar a solicitação por meio do servidor da web que hospeda a página.

Por exemplo, uma página da web que é carregada por meio de um servidor no {{site.data.keyword.cloud}} pode chamar a API de customização diretamente, ignorando o servidor {{site.data.keyword.cloud_notm}}. Para obter mais informações, consulte [enable-cors.org ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://enable-cors.org/){: new_window}.

## Usando kits de desenvolvimento de software
{: #sdks}

Os SDKs estão disponíveis para o serviço {{site.data.keyword.speechtotextshort}} para simplificar o desenvolvimento de aplicativos de fala. Os SDKs do {{site.data.keyword.ibmwatson}} estão disponíveis para muitas linguagens de programação e plataformas populares.

-   Para obter uma lista completa de SDKs e links para os SDKs no GitHub, consulte [Usando SDKs](/docs/services/watson/getting-started-sdks.html).
-   Para obter informações detalhadas sobre todos os métodos do Node, Java&trade;, Python, Ruby, Swift e Go SDKs para o serviço {{site.data.keyword.speechtotextshort}}, consulte a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

## Aprendendo mais sobre o desenvolvimento de aplicativo
{: #learn}

Para obter mais informações sobre como trabalhar com os serviços do {{site.data.keyword.watson}} e o {{site.data.keyword.cloud_notm}}, consulte o seguinte:

-   Para obter uma introdução ao trabalho com serviços do {{site.data.keyword.watson}} e o {{site.data.keyword.cloud_notm}}, consulte [Introdução ao {{site.data.keyword.watson}} e ao {{site.data.keyword.cloud_notm}}](/docs/services/watson/index.html).
-   Todas as novas instâncias de serviço usam o {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) para autenticação. As instâncias de serviço mais antigas podem continuar a usar o `{username}` e a `{password}` de suas credenciais de serviço existentes do Cloud Foundry para autenticação. Para obter mais informações sobre a autenticação no serviço, veja a [Atualização de serviço
de 30 de outubro de 2018](/docs/services/speech-to-text/release-notes.html#October2018b) nas notas sobre a liberação.
-   Para obter informações sobre como controlar a solicitação padrão de criação de log que é executada para todos os serviços do {{site.data.keyword.watson}}, consulte [Controlando solicitação de criação de log para serviços do {{site.data.keyword.watson}}](/docs/services/watson/getting-started-logging.html).
