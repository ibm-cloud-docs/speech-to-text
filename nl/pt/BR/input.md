---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-24"

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

# Recursos de entrada
{: #input}

O serviço {{site.data.keyword.speechtotextfull}} oferece os recursos a seguir para especificar como o serviço deve executar uma solicitação de reconhecimento de voz. Todos os parâmetros de entrada que são descritos nas seções a seguir são opcionais. Somente o áudio de entrada é necessário.
{: shortdesc}

-   Para obter exemplos de solicitações de reconhecimento de voz simples para cada uma das interfaces do serviço, consulte [Fazendo uma solicitação de reconhecimento](/docs/services/speech-to-text?topic=speech-to-text-basic-request).
-   Para obter uma lista em ordem alfabética de todos os parâmetros de reconhecimento de voz disponíveis, incluindo seu status (geralmente disponível ou beta) e idiomas suportados, consulte o [Resumo de parâmetro](/docs/services/speech-to-text?topic=speech-to-text-summary).

## Modelos customizados
{: #custom-input}

A customização do idioma e do modelo acústico está disponível em diferentes níveis de suporte (geralmente disponíveis ou beta) para diferentes idiomas. Para obter mais informações, consulte [Suporte ao idioma para customização](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport).
{: note}

Todas as interfaces aceitam um modelo customizado para uso em uma solicitação de reconhecimento:

-   *Modelos de idioma customizados* expandem o vocabulário base do serviço com a terminologia de domínios específicos. Use o parâmetro `language_customization_id` para incluir um modelo de idioma customizado com uma solicitação. Também é possível especificar um parâmetro `customization_weight` opcional. O parâmetro indica o peso relativo que é fornecido para palavras do modelo customizado em vez de palavras do vocabulário base.

    Para obter mais informações, consulte [Usando um modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-languageUse).
-   Os *modelos acústicos customizados* adaptam o modelo acústico de base do serviço às características acústicas de seu ambiente e dos falantes. Use o parâmetro `acoustic_customization_id` para incluir um modelo acústico customizado com uma solicitação. É possível especificar um modelo de idioma customizado e um modelo acústico customizado com uma solicitação.

    Para obter mais informações, consulte [Usando um modelo acústico customizado](/docs/services/speech-to-text?topic=speech-to-text-acousticUse).

Os modelos customizados são baseados em um dos modelos de idioma que estão descritos em [Idiomas e modelos](/docs/services/speech-to-text?topic=speech-to-text-models). Um modelo customizado pode ser usado somente com o modelo base para o qual ele é criado. Se seu modelo customizado for baseado em um modelo diferente do `en-US_BroadbandModel` padrão, você também deverá especificar o nome do modelo com a solicitação. Para usar um modelo customizado, deve-se emitir a solicitação com credenciais para a instância do serviço que tem o modelo customizado.

Para obter uma introdução à customização, consulte [A interface de customização](/docs/services/speech-to-text?topic=speech-to-text-customization).

### Exemplos de modelo customizado
{: #customExample}

A solicitação de exemplo a seguir inclui o parâmetro `language_customization_id` para usar o modelo de idioma customizado com o ID especificado. Ele inclui o parâmetro `customization_weight` para indicar que as palavras do modelo customizado devem receber um peso relativo de `0.5`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.5"
```
{: pre}

A solicitação de exemplo a seguir usa um modelo de idioma customizado e um modelo acústico customizado. O primeiro é identificado com o parâmetro `language_customization_id`, o último com o parâmetro `acoustic_customization_id`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&acoustic_customization_id={customization_id}"
```
{: pre}

Para exemplos que usam modelos customizados com cada uma das interfaces do serviço, consulte

-   [Usando um modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-languageUse)
-   [Usando um modelo acústico customizado](/docs/services/speech-to-text?topic=speech-to-text-acousticUse)

## Gramáticas
{: #grammars-input}

O recurso de gramáticas é funcionalidade beta. O serviço suporta as gramáticas para todos os idiomas para os quais ele suporta a customização do modelo de idioma.
{: note}

É possível incluir gramáticas em um modelo de idioma customizado e usá-las para reconhecimento de voz. As gramáticas usam uma especificação de idioma formal para definir um conjunto de regras de produção para sequências de transcrição. As regras especificam como formar sequências válidas por meio do alfabeto do idioma.

Quando você usa uma gramática para reconhecimento de voz, o serviço reconhece apenas frases que são reconhecidas pela gramática. Ao limitar o espaço de procura para sequências válidas, o serviço pode entregar resultados mais rapidamente e com mais precisão.

Todas as interfaces aceitam os parâmetros a seguir para uma solicitação de reconhecimento:

-   O parâmetro `language_customization_id` identifica o modelo de idioma customizado para o qual a gramática está definida. Deve-se emitir a solicitação com credenciais para a instância do serviço que possui o modelo.
-   O parâmetro `grammar_name` especifica a gramática que você deseja usar. É possível especificar somente uma única gramática com uma solicitação.

Para obter mais informações, consulte [Usando gramáticas com modelos de idioma customizados](/docs/services/speech-to-text?topic=speech-to-text-grammars).

### Exemplo de gramáticas
{: #grammarsExample}

A solicitação de exemplo a seguir inclui os parâmetros `language_customization_id` e `grammar_name` para restringir a resposta do serviço às sequências que estão definidas na gramática denominada `list-abnf`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&grammar_name=list-abnf"
```
{: pre}

Para obter exemplos que usam gramáticas com cada uma das interfaces do serviço, consulte [Usando uma gramática para reconhecimento de voz](/docs/services/speech-to-text?topic=speech-to-text-grammarUse).

## Versão do modelo base
{: #version}

Para melhorar a qualidade do reconhecimento de voz, o serviço atualiza ocasionalmente os modelos de idioma base descritos em [Idiomas e modelos](/docs/services/speech-to-text?topic=speech-to-text-models). Os modelos base para idiomas são independentes uns dos outros, assim como os modelos de banda larga e de banda estreita para um idioma. Portanto, as atualizações para os modelos base ocorrem independentemente umas das outras e não têm efeito em outros modelos.

Quando existirem múltiplas versões de um modelo base, o parâmetro opcional `base_model_version` especificará a versão do modelo a ser usada com uma solicitação de reconhecimento. O parâmetro é destinado principalmente para uso com modelos customizados que são atualizados para um novo modelo base, mas ele pode ser usado sem um modelo customizado. A versão de um modelo base usada para uma solicitação depende da transmissão do parâmetro `base_model_version`. Ela também depende da especificação de um modelo customizado (idioma, acústico ou ambos) com a solicitação.

-   *Se você não especificar um modelo customizado com a solicitação*

    -   Omita o parâmetro `base_model_version` para usar a versão mais recente do modelo base.
    -   Especifique o parâmetro `base_model_version` para usar uma versão específica do modelo base.

-   *Se você especificar um modelo customizado com a solicitação*

    -   Omita o parâmetro `base_model_version` para usar a versão mais recente do modelo base para a qual é feito o upgrade do modelo customizado. Por exemplo, se for feito upgrade do modelo customizado para a versão mais recente do modelo base, o serviço usará as versões mais recentes dos modelos base e customizado.
    -   Especifique o parâmetro `base_model_version` para usar as versões especificadas de ambos os modelos, base e customizado.

O parâmetro é destinado para uso com modelos customizados. Portanto, é possível aprender sobre as versões disponíveis de um modelo base apenas listando informações sobre um modelo customizado que é baseado nele.

-   Para obter mais informações sobre como fazer upgrade de modelos customizados, consulte [Fazendo upgrade de modelos customizados](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade).
-   Para obter mais informações sobre o uso de diferentes versões de modelos base e customizados para reconhecimento de voz, consulte [Fazendo solicitações de reconhecimento com modelos customizados que passaram por upgrade](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeRecognition).

## Transmissão de áudio
{: #transmission}

*Com a interface do WebSocket,* os dados de áudio são sempre transmitidos para o serviço por meio da conexão. É possível passar os dados por meio do soquete todos de uma vez ou passá-los para o caso de uso em tempo real à medida que se tornam disponíveis. O serviço retorna os resultados à medida que eles se tornam disponíveis.

*Com as interfaces de HTTP*, é possível transmitir áudio para o serviço de uma das maneiras a seguir:

-   *Entrega única* - Você omite o cabeçalho `Transfer-Encoding` e passa todos os dados de áudio para o serviço de uma vez como uma única entrega.
-   *Fluxo* - Você configura o cabeçalho da solicitação `Transfer-Encoding` para o valor `chunked` e flui os dados por meio de uma conexão persistente. Os dados não precisam existir completamente antes de passá-los ao serviço. É possível transmitir os dados à medida que eles se tornam disponíveis. O serviço envia os resultados somente quando recebe o chunk final, que você indica enviando um chunk vazio.

    Para obter mais informações sobre o fluxo de áudio em partes com o cabeçalho `Transfer-Encoding`, consulte
    -   [Codificação de transferência em chunk](https://wikipedia.org/wiki/Chunked_transfer_encoding){: external}
    -   [Codificações de transferência](https://tools.ietf.org/html/rfc7230#section-4){: external} no *IETF RFC 7320 HTTP/1.1: sintaxe de mensagem e roteamento*

Com as interfaces de HTTP, o serviço sempre transcreve o fluxo de áudio inteiro antes de enviar qualquer resultado. Os resultados podem incluir múltiplos elementos `transcript` para indicar frases que são separadas por pausas. Concatene os elementos `transcript` para montar a transcrição completa.

O serviço impõe tempos limites em uma sessão de fluxo. Ele poderá finalizar uma sessão de fluxo se detectar um período estendido de silêncio ou não receber áudio durante um período de 30 segundos. Para obter mais informações sobre os tempos limites e como evitá-los, consulte [Tempos limites](#timeouts).

### Exemplo de transmissão de áudio
{: #transmissionExample}

A solicitação de exemplo a seguir especifica `chunked` para o cabeçalho `Transfer-Encoding` para usar o modo de fluxo. A conexão permanece aberta para aceitar chunks adicionais de áudio.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--header "Transfer-Encoding: chunked"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{: pre}

## Tempos limites
{: #timeouts}

Quando você inicia uma sessão de fluxo com os métodos de reconhecimento de voz HTTP ou WebSocket, o serviço força a inatividade e os tempos limites de sessão. Se um tempo limite decorrer durante uma sessão de fluxo, o serviço fechará a conexão. Seu aplicativo deve se recuperar normalmente de possíveis conexões fechadas.

Quando você transmite áudio por meio de HTTP, o serviço envia um caractere de espaço em sua resposta a cada 20 segundos. O serviço faz isso para melhorar a usabilidade ao evitar o tempo limite de inatividade de REST do HTTP de 30 segundos. Para manter a conexão ativa enquanto o reconhecimento está em andamento, o serviço continua a enviar esse caractere de espaço até que conclua sua transcrição. O caractere de espaço não tem efeito em dados de resposta codificados por JSON.

Esse tempo limite de inatividade de HTTP é diferente do tempo limite de inatividade do serviço. A interface do WebSocket não está sujeita a esse tempo limite de HTTP.
{: note}

### Tempo limite de inatividade
{: #timeouts-inactivity}

Um *tempo limite de inatividade* (código de status HTTP 400) ocorre quando o serviço está recebendo áudio, mas detecta apenas silêncio contínuo ou atividade de não fala (sem fala) por 30 segundos. O serviço envia a mensagem de erro `No speech detected for 30s`. O tempo limite de inatividade é útil, por exemplo, para finalizar uma sessão quando um usuário simplesmente se afaste de um microfone em tempo real.

O tempo limite de inatividade padrão é 30 segundos. É possível substituir esse valor usando o parâmetro `inactivity_timeout`. Especifique um valor maior para aumentar o tempo limite de inatividade. Especifique um valor de `-1` para configurar o tempo limite de inatividade para o infinito. Você é cobrado por todos os áudios que envia para o serviço, incluindo o silêncio, de modo que o aumento do tempo limite de inatividade pode incorrer em encargos adicionais para uma sessão de fluxo que envia somente silêncio.

A solicitação de exemplo a seguir configura o tempo limite de inatividade para 60 segundos. A solicitação envia um arquivo inicial para iniciar a sessão de fluxo.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Transfer-Encoding: chunked"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file1.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?inactivity_timeout=60"
```
{: pre}

### Tempo limite de sessão
{: #timeouts-session}

Um *tempo limite de sessão* (código de status HTTP 408) ocorre quando você falha ao enviar áudio suficiente para manter uma sessão de fluxo ativa. O serviço pode considerar uma sessão inativa e acionar um tempo limite de sessão pelas razões a seguir:

-   Você falha ao enviar pelo menos 15 segundos de áudio para o serviço em qualquer janela de 30 segundos.

    Até que você envie o último chunk para indicar o término do fluxo, deve-se enviar pelo menos 15 segundos de áudio dentro de qualquer período de 30 segundos. O áudio poderá ser silencioso se você configurar o parâmetro `inactivity_timeout` para um valor maior ou para `-1`. Você é cobrado pela duração de qualquer áudio que envia para o serviço, incluindo o silêncio.
-   Você transmite o áudio a uma taxa que é muito mais lenta do que o tempo real.

    O ideal é você iniciar uma solicitação para estabelecer uma sessão imediatamente antes de obter o áudio para transcrição. Então, mantenha a sessão enviando áudio a uma taxa próxima do tempo real.

Não é necessário se preocupar com o tempo limite de sessão depois de enviar o último chunk para indicar o final do fluxo. O serviço continua a processar o áudio até retornar os resultados da transcrição final.

Quando você transcrever um fluxo de áudio longo, o serviço poderá levar mais de 30 segundos para processar o áudio e gerar uma resposta. O serviço não começa a calcular o tempo limite de sessão até finalizar o processamento de todo o áudio que ele recebeu. O tempo de processamento do serviço não pode fazer com que a sessão exceda o tempo limite de sessão de 30 segundos.

Por exemplo, se você enviar uma hora de áudio nos primeiros 10 segundos de uma sessão, o serviço poderá levar 300 segundos para processar o áudio.  Para manter essa sessão ativa, você precisa enviar pelo menos mais 15 segundos de algum áudio para a sessão, incluindo silêncio, e não passando de 340 segundos.

Nesse exemplo, se você enviasse mais 15 segundos de áudio na marca de 100 segundos da sessão, o serviço poderia gastar dois segundos adicionais ao processar esse áudio. Nesse caso, você precisaria enviar mais 15 segundos de áudio para a sessão, até 342 segundos.

Não confie no tempo de processamento ou em se você recebeu resultados para determinar se uma sessão de fluxo está inativa. Suponha que o serviço possa processar todos os áudios instantaneamente e enviar os dados para o serviço adequadamente. Se você transmitir áudio em tempo real, não se atrase ao enviar o áudio a meio tempo real (15 segundos de áudio) em qualquer janela de 30 segundos. Essa taxa geralmente é suficiente para acomodar a latência de rede e os atrasos.
{: important}

## Criação de log da solicitação
{: #logging}

Por padrão, a {{site.data.keyword.IBM_notm}} registra todas as solicitações para os serviços do {{site.data.keyword.watson}} e seus resultados. A criação de log é feita somente para melhorar os serviços para futuros usuários. Os dados registrados não são
compartilhados ou tornados públicos.

Se você estiver preocupado com a proteção das informações pessoais dos usuários ou não desejar que as suas solicitações sejam registradas pela IBM, será possível escolher não ter os dados do log da IBM (cancelar). É possível cancelar a criação de log no nível da conta ou no nível da solicitação de API. Para obter mais informações, consulte [Controlando a criação de log de solicitação para os serviços do {{site.data.keyword.watson}}](/docs/services/watson?topic=watson-gs-logging-overview).

## Segurança de informações
{: #security-input}

A segurança de informações inclui recursos para associar um ID do cliente aos dados que são passados para o serviço com uma solicitação. Você associa um ID do cliente aos dados transmitindo o cabeçalho `X-Watson-Metadata` com a solicitação. Se necessário, será possível excluir os dados usando o método `DELETE /v1/user_data`. Para obter mais informações, consulte [Segurança de informações](/docs/services/speech-to-text?topic=speech-to-text-information-security).
