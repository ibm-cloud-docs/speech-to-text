---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-11"

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

# Nota sobre a Liberação
{: #release-notes}

As seções a seguir documentam os novos recursos e mudanças que foram incluídos para cada liberação e atualização do serviço {{site.data.keyword.speechtotextfull}}. As informações incluem qualquer limitação conhecida. A menos que seja observado de outra forma, todas as alterações são compatíveis com liberações anteriores e são disponibilizadas de forma automática e transparente para todos os aplicativos novos e existentes.
{: shortdesc}

## Limitações Conhecidas
{: #limitations}

Nenhuma limitação conhecida neste momento.

## 3 de abril de 2019
{: #April2019}

Os modelos acústicos customizados aceitam agora um máximo de 200 horas de áudio. O limite máximo anterior era de 100 horas de áudio.

## 21 de Março de 2019
{: #March2019d}

Os usuários agora podem ver informações da credencial de serviço que estão associadas à função que foi designada à sua conta do {{site.data.keyword.cloud_notm}}. Por exemplo, se você estiver designado a uma função de `reader`, qualquer `writer` ou níveis mais altos de credenciais de serviço não estarão mais visíveis.

Essa mudança não afeta o acesso à API para usuários ou aplicativos com credenciais de serviço existentes. A mudança afeta somente a visualização de credenciais no {{site.data.keyword.cloud_notm}}.

Para obter mais informações sobre chaves de serviço e funções de usuário, consulte [Chaves de API do serviço do IAM](/docs/services/watson?topic=watson-api-key-bp#api-key-bp).

## 15 de março de 2019
{: #March2019c}

O serviço agora suporta áudio no formato A-law (`audio/alaw`). Para obter mais informações, consulte [formato audio/alaw](/docs/services/speech-to-text/audio-formats.html#alaw).

## Liberações mais antigas
{: #older}

-   [11 de março de 2019](#March2019b)
-   [4 de março de 2019](#March2019)
-   [28 de janeiro de 2019](#January2019)
-   [20 de dezembro de 2018](#December2018b)
-   [13 de dezembro de 2018](#December2018a)
-   [12 de novembro de 2018](#November2018b)
-   [7 de novembro de 2018](#November2018a)
-   [ 30 de outubro de 2018 ](#October2018b)
-   [9 de outubro de 2018](#October2018a)
-   [10 de setembro de 2018](#September2018b)
-   [7 de setembro de 2018](#September2018a)
-   [8 de agosto de 2018](#August2018)
-   [13 de julho de 2018](#July2018)
-   [12 de junho de 2018](#June2018)
-   [15 de maio de 2018](#May2018)
-   [26 de março de 2018](#March2018b)
-   [1º de março de 2018](#March2018a)
-   [1º de fevereiro de 2018](#February2018)
-   [14 de dezembro de 2017](#December2017)
-   [2 de outubro de 2017](#October2017)
-   [14 de julho de 2017](#July2017b)
-   [1º de julho de 2017](#July2017a)
-   [22 de maio de 2017](#May2017)
-   [10 de abril de 2017](#April2017)
-   [8 de março de 2017](#March2017)
-   [1º de dezembro de 2016](#December2016)
-   [22 de setembro de 2016](#September2016)
-   [30 de junho de 2016](#June2016b)
-   [23 de junho de 2016](#June2016a)
-   [10 de março de 2016](#March2016)
-   [19 de janeiro de 2016](#January2016)
-   [17 de dezembro de 2015](#December2015)
-   [21 de setembro de 2015](#September2015)
-   [1º de julho de 2015](#July2015)

### 11 de março de 2019
{: #March2019b}

-   Para o parâmetro `max_alternatives`, o serviço novamente aceita um valor de `0`. Se você especificar `0`, o serviço usará automaticamente o valor padrão, `1`. Uma mudança feita para a atualização de serviço de 4 de março fez com que um valor de `0` retornasse um erro. (O serviço retornará um erro se você especificar um valor negativo.)
-   Para o parâmetro `word_alternatives_threshold`, o serviço novamente aceita um valor de `0`. Uma mudança feita para a atualização de serviço de 4 de março fez com que um valor de `0` retornasse um erro. (O serviço retornará um erro se você especificar um valor negativo.)
-   O serviço agora retorna todas as pontuações de confiança com uma precisão máxima de duas casas decimais. Isso inclui pontuações de confiança para transcrições, confiança de palavra, alternativas de palavra, resultados da palavra-chave e rótulos do falante.

### 4 de março de 2019
{: #March2019}

Os modelos de idioma de banda estreita a seguir foram atualizados para reconhecimento de voz melhorado:

-   `es-ES_NarrowbandModel`
-   `fr-FR_NarrowbandModel`
-   `pt-BR_NarrowbandModel`

Por padrão, o serviço usa automaticamente os modelos atualizados para todas as solicitações de reconhecimento de voz. Se você tiver modelos customizados acústicos ou de idioma que são baseados nos modelos, deverá fazer upgrade de seus modelos customizados existentes para aproveitar as atualizações usando os métodos a seguir:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Para obter mais informações, consulte [Fazendo upgrade de modelos customizados](/docs/services/speech-to-text/custom-upgrade.html).

### 28 de janeiro de 2019
{: #January2019}

A interface do WebSocket agora suporta a autenticação do Identity and Access Management (IAM) baseada em token por meio do código JavaScript baseado em navegador. A limitação para o contrário foi removida. Para estabelecer uma conexão autenticada com o método `/v1/recognize` do WebSocket:

-   Se você usar a autenticação do IAM, inclua o parâmetro de consulta `access_token`.
-   Se você usar as credenciais de serviço do Cloud Foundry, inclua o parâmetro de consulta `watson-token`.

Para obter mais informações, consulte [Abrir uma conexão](/docs/services/speech-to-text/websockets.html#WSopen).

### 20 de dezembro de 2018
{: #December2018b}

A interface de gramática está totalmente funcional em todas as localizações a partir de 8 de janeiro de 2019.
{: note}

-   O serviço agora suporta gramáticas para reconhecimento de voz. As gramáticas estão disponíveis como funcionalidade beta para todos os idiomas que suportam a customização do modelo de idioma. É possível incluir gramáticas em um modelo de idioma customizado e utilizá-las para restringir o conjunto de frases que o serviço pode reconhecer do áudio. É possível definir uma gramática em Augmented Backus-Naur Form (ABNF) ou formulário XML.

    Os quatro métodos a seguir estão disponíveis para trabalhar com gramáticas:
    -   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` inclui um arquivo de gramática em um modelo de idioma customizado.
    -   `GET /v1/customizations/{customization_id}/grammars ` lista informações sobre todas as gramáticas para um modelo customizado.
    -   `GET /v1/customizations/{customization_id}/grammars/{grammar_name}` retorna informações sobre uma gramática especificada para um modelo customizado.
    -   `DELETE /v1/customizations/{customization_id}/grammars/{grammar_name}` remove uma gramática existente de um modelo customizado.

    É possível usar uma gramática para reconhecimento de voz com as interfaces do WebSocket e HTTP. Use os parâmetros `language_customization_id` e `grammar_name` para identificar o modelo customizado e a gramática que você deseja usar. Atualmente, é possível usar apenas uma gramática com uma solicitação de reconhecimento de voz.

    Para obter mais informações sobre gramáticas, consulte a documentação a seguir:
    -   [Usando gramáticas com modelos de idioma customizados](/docs/services/speech-to-text/grammar.html)
    -   [Entendendo as gramáticas](/docs/services/speech-to-text/grammar-understand.html)
    -   [Incluindo uma gramática em um modelo de idioma customizado](/docs/services/speech-to-text/grammar-add.html)
    -   [Usando uma gramática para reconhecimento de voz](/docs/services/speech-to-text/grammar-use.html)
    -   [Gerenciando gramáticas](/docs/services/speech-to-text/grammar-manage.html)
    -   [Gramáticas de exemplo](/docs/services/speech-to-text/grammar-examples.html)

    Para obter informações sobre todos os métodos da interface, consulte a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Um novo recurso de edição de dados numéricos agora está disponível para mascarar números que têm três ou mais dígitos consecutivos. A edição de dados destina-se a remover das transcrições as informações pessoais sensíveis, como números de cartão de crédito. Você ativa o recurso configurando o parâmetro `redaction` como `true` em uma solicitação de reconhecimento. O recurso é a funcionalidade beta que está disponível para inglês dos EUA, japonês e coreano somente. Para obter mais informações, consulte [Edição de dados numéricos](/docs/services/speech-to-text/output.html#redaction).
-   Os novos modelos de idioma alemão e francês a seguir agora estão disponíveis com o serviço:
    -   `de-DE_NarrowbandModel`
    -   `fr-FR_NarrowbandModel`

    Os dois novos modelos suportam a customização do modelo de idioma (GA) e a customização do modelo acústico (beta). Para obter mais informações, consulte [Suporte ao idioma para customização](/docs/services/speech-to-text/custom.html#languageSupport).
-   Um novo modelo de idioma inglês dos Estados Unidos, `en-US_ShortForm_NarrowbandModel`, agora está disponível. O novo modelo é destinado para uso nas soluções de suporte ao cliente automatizadas e resposta de voz interativa. O modelo suporta a customização do modelo de idioma (GA) e a customização do modelo acústico (beta).
-   Os modelos de idioma a seguir foram atualizados para reconhecimento de voz melhorado:
    -   `en-GB_NarrowbandModel`
    -   `es-ES_NarrowbandModel`

    Por padrão, o serviço usa automaticamente os modelos atualizados para todas as solicitações de reconhecimento de voz. Se você tiver modelos customizados acústicos ou de idioma que são baseados nos modelos, deverá fazer upgrade de seus modelos customizados existentes para aproveitar as atualizações usando os métodos a seguir:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Para obter mais informações, consulte [Fazendo upgrade de modelos customizados](/docs/services/speech-to-text/custom-upgrade.html).
-   O serviço agora suporta áudio no formato G.729 (`audio/g729`). O serviço suporta apenas o G.729 Annex D para áudio de banda estreita. Para obter mais informações sobre os formatos de áudio suportados, consulte [Formatos de áudio](/docs/services/speech-to-text/audio-formats.html).
-   O recurso de rótulos do falante está agora disponível para o modelo de banda estreita para inglês do Reino Unido (`en-GB_NarrowbandModel`). O recurso é funcionalidade beta para todos os idiomas suportados. Para obter mais informações, consulte [Rótulos do falante](/docs/services/speech-to-text/output.html#speaker_labels).
-   A quantidade máxima de áudio que você pode incluir em um modelo acústico customizado aumentou de 50 para 100 horas.

### 13 de dezembro de 2018
{: #December2018a}

O serviço agora está disponível na localização Londres do {{site.data.keyword.cloud_notm}} (**eu-gb**). Como todos os
locais, Londres usa a autenticação do IAM baseada em token. Todas as novas instâncias de serviço que você cria nessa localização usam a autenticação do IAM.

### 12 de novembro de 2018
{: #November2018b}

O serviço agora suporta a formatação inteligente para reconhecimento de voz em japonês. Anteriormente, o serviço suportava a formatação inteligente apenas para inglês dos EUA e Espanhol. O recurso é funcionalidade beta para todos os idiomas suportados. Para obter mais informações, consulte [Formatação inteligente](/docs/services/speech-to-text/output.html#smart_formatting).

### 7 de novembro de 2018
{: #November2018a}

O serviço agora está disponível na localização Tóquio do {{site.data.keyword.cloud_notm}} (**jp-tok**). Como todos os
locais, Tokyo usa a autenticação do IAM baseada em token. Todas as novas instâncias de serviço que você cria nessa localização usam a autenticação do IAM.

### 30 de outubro de 2018
{: #October2018b}

O serviço migrou para a autenticação do IAM baseada em token para todos os locais. Todos os serviços do {{site.data.keyword.cloud_notm}} agora usam a autenticação do IAM. O serviço {{site.data.keyword.speechtotextshort}} migrou em cada local nas datas a seguir:

-   Dallas (**us-south**): 30 de outubro de 2018
-   Frankfurt (**eu-de**): 30 de outubro de 2018
-   Washington, DC (**us-east**): 12 de junho de 2018
-   Sydney (**au-syd**): 15 de maio de 2018

A migração para a autenticação do IAM afeta instâncias de serviço novas e existentes de
maneira diferente:

-   *Todas as novas instâncias de serviço que você cria em qualquer local* agora
usam a autenticação do IAM para acessar o serviço. É possível passar um token de acesso ou uma chave de API: os tokens suportam solicitações autenticadas sem incorporar as credenciais de serviço em cada chamada; as chaves de API usam a autenticação básica de HTTP. Quando você usa qualquer um dos SDKs
do {{site.data.keyword.watson}}, pode passar a chave da API e deixar o SDK gerenciar o ciclo
de vida dos tokens.
-   *As instâncias de serviço existentes que você criou em uma localização antes da data de migração indicada* continuam a usar o `{username}` e `{password}` de suas credenciais de serviço do Cloud Foundry anteriores para autenticação até que você migre-as para usar a autenticação do IAM. Para obter mais informações sobre como migrar para a autenticação do IAM, consulte [Migrando instâncias de serviço do Cloud Foundry para um grupo de recursos](https://{DomainName}/docs/resources/instance_migration.html).

Para obter mais informações, veja a documentação a seguir:

-   Para saber qual mecanismo de autenticação sua instância de serviço usa, visualize suas
credenciais de serviço clicando na instância no painel do [{{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/dashboard/apps){: new_window}.
-   Para obter mais informações sobre como usar tokens do IAM com serviços do Watson, consulte [Autenticando com tokens do IAM](/docs/services/watson/getting-started-iam.html).
-   Para obter mais informações sobre como usar as chaves de API do IAM com serviços do Watson, consulte [Chaves de API de serviço do IAM](/docs/services/watson/apikey-bp.html).
-   Para obter exemplos que usam a autenticação do IAM, consulte a [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 9 de outubro de 2018
{: #October2018a}

-   O cabeçalho `Content-Type` agora é opcional para solicitações de reconhecimento de voz. O serviço agora detecta automaticamente o formato de áudio (tipo MIME) da maioria do áudio. Deve-se continuar a especificar o tipo de conteúdo para os formatos a seguir:
    -   `audio/basic`
    -   `audio/l16`
    -   `audio/mulaw`

    Quando indicado, o tipo de conteúdo que você especifica para esses formatos deve incluir a taxa de amostragem e pode, opcionalmente, incluir o número de canais e a ordenação do áudio. Para todos os outros formatos de áudio, é possível omitir o tipo de conteúdo ou especificar um tipo de conteúdo de `application/octet-stream` para que o serviço detecte automaticamente o formato.

    Quando você usa o comando `curl` para fazer uma solicitação de reconhecimento de voz com a interface de HTTP, deve-se especificar o formato de áudio com o cabeçalho `Content-Type`, especificar `"Content-Type: application/octet-stream"` ou especificar `"Content-Type:"`. Se você omitir o cabeçalho completamente, `curl` usará um valor padrão de `application/x-www-form-urlencoded`. A maioria dos exemplos nesta documentação continua a especificar o formato para solicitações de reconhecimento de voz independentemente de ele ser necessário.
    {: important}

    Essa mudança se aplica aos métodos a seguir:
    -   `/v1/recognize` para as solicitações do WebSocket. O campo `content-type` da mensagem de texto que você envia para iniciar uma solicitação por meio de uma conexão do WebSocket aberta agora é opcional.
    -   `POST /v1/recognize` para solicitações de HTTP síncronas. O cabeçalho `Content-Type` agora é opcional. (Para solicitações com múltiplas partes, o campo `part_content_type` dos metadados JSON agora também é opcional.)
    -   `POST /v1/recognitions` para solicitações de HTTP assíncronas. O cabeçalho `Content-Type` agora é opcional.

    Para obter mais informações, consulte [Formatos de áudio](/docs/services/speech-to-text/audio-formats.html).
-   O modelo de banda larga de português do Brasil, `pt-BR_BroadbandModel`, foi atualizado para reconhecimento de voz melhorado. Por padrão, o serviço usa automaticamente o modelo atualizado para todas as solicitações de reconhecimento. Se você tiver modelos customizados acústicos ou de idioma que são baseados nesse modelo, deverá fazer upgrade de seus modelos customizados existentes para aproveitar as atualizações usando os métodos a seguir:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Para obter mais informações, consulte [Fazendo upgrade de modelos customizados](/docs/services/speech-to-text/custom-upgrade.html).
-   O parâmetro `customization_id` dos métodos de reconhecimento de voz foi descontinuado e será removido em uma liberação futura. Para especificar um modelo de idioma customizado para uma solicitação de reconhecimento de voz, use o parâmetro `language_customization_id` no lugar. Essa mudança se aplica aos métodos a seguir:
    -   `/v1/recognize` para solicitações do WebSocket
    -   `POST /v1/recognize` para solicitações de HTTP síncronas (incluindo solicitações com múltiplas partes)
    -   `POST /v1/recognitions` para solicitações de HTTP assíncronas
-   A partir de 1º de outubro de 2018, agora você é cobrado por todo o áudio que passar para o serviço para reconhecimento de voz. Os primeiros mil minutos de áudio que você enviava todo mês não são mais grátis. Para obter mais informações sobre os planos de precificação para o serviço, consulte o serviço {{site.data.keyword.speechtotextshort}} no [Catálogo do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/catalog/services/speech-to-text){: new_window}.

### 10 de setembro de 2018
{: #September2018b}

Para obter uma lista de problemas que foram corrigidos desde a liberação inicial, consulte [Problemas resolvidos](#known_issues).
{: important}

-   O serviço agora suporta um modelo de banda larga de alemão, `de-DE_BroadbandModel`. O novo modelo de alemão suporta a customização do modelo de idioma (geralmente disponível) e a customização do modelo acústico (beta).
    -   Para obter informações sobre como o serviço analisa os corpora para alemão, consulte [Análise de inglês, francês, alemão, espanhol e português do Brasil](/docs/services/speech-to-text/language-resource.html#corpusLanguages).
    -   Para obter mais informações sobre como criar pronúncias para palavras customizadas em alemão, consulte [Diretrizes para francês, alemão, espanhol e português do Brasil](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR).
-   Os modelos de português do Brasil existentes, `pt-BR_BroadbandModel` e `pt-BR_NarrowbandModel`, agora suportam customização do modelo de idioma (geralmente disponível). Os modelos não foram atualizados para ativar esse suporte, portanto, nenhum upgrade de modelos acústicos customizados existentes é necessário.
    -   Para obter informações sobre como o serviço analisa os corpora para o português do Brasil, consulte [Análise de inglês, francês, alemão, espanhol e português do Brasil](/docs/services/speech-to-text/language-resource.html#corpusLanguages).
    -   Para obter mais informações sobre como criar pronúncias para palavras customizadas no português do Brasil, consulte [Diretrizes para francês, alemão, espanhol e português do Brasil](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR).
-   Novas versões dos modelos de banda larga e banda estreita de inglês dos EUA e japonês estão disponíveis:
    -   `en-US_BroadbandModel`
    -   `en-US_NarrowbandModel`
    -   `ja-JP_BroadbandModel`
    -   `ja-JP_NarrowbandModel`

    Os novos modelos oferecem melhor reconhecimento de voz. Por padrão, o serviço usa automaticamente os modelos atualizados para todas as solicitações de reconhecimento. Se você tiver modelos customizados acústicos ou de idioma que são baseados nesses modelos, deverá fazer upgrade de seus modelos customizados existentes para aproveitar as atualizações usando os métodos a seguir:
    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Para obter mais informações, consulte [Fazendo upgrade de modelos customizados](/docs/services/speech-to-text/custom-upgrade.html).
-   Os recursos de marcação de palavra-chave e de alternativas de palavra agora estão geralmente disponíveis (GA) em vez da funcionalidade beta para todos os idiomas. Para
obter mais informações, consulte
    -   [Marcação de palavra-chave](/docs/services/speech-to-text/output.html#keyword_spotting)
    -   [Alternativas de palavra](/docs/services/speech-to-text/output.html#word_alternatives)

### Problemas resolvidos
{: #known_issues}

Os problemas conhecidos a seguir que estavam associados à interface de customização foram resolvidos. Essa lista é mantida para usuários que podem ter encontrado os problemas no passado.

-   Se você incluir dados em um modelo customizado acústico ou de idioma, deverá retreinar o modelo antes de usá-lo para reconhecimento de voz. O problema é mostrado no cenário a seguir:

    1.  O usuário cria um novo modelo customizado (idioma ou acústico) e treina o modelo.
    1.  O usuário inclui recursos adicionais (palavras, corpora ou áudio) no modelo customizado, mas não executa o retreinamento do modelo.
    1.  O usuário não pode usar o modelo customizado para reconhecimento de voz. O serviço retorna um erro da seguinte forma quando usado com uma solicitação de reconhecimento de voz:

        ```javascript
        {
          "code_description": "Bad Request",
          "code": 400,
          "error": "Requested custom language model is not available.
                    Please make sure the custom model is trained."
        }
        ```
        {: codeblock}

    Para uma solução alternativa desse problema, o usuário deve retreinar o modelo customizado em seus dados mais recentes. Então, o usuário pode usar o modelo customizado com reconhecimento de voz.
-   Antes de treinar um modelo customizado acústico ou de idioma existente, deve-se fazer upgrade dele para a versão mais recente de seu modelo base. O problema é mostrado no cenário a seguir:

    1.  O usuário tem um modelo customizado existente (idioma ou acústico) que é baseado em um modelo que foi atualizado.
    1.  O usuário treina o modelo customizado existente com relação à versão antiga do modelo base sem primeiro fazer upgrade para a versão mais recente do modelo base.
    1.  O usuário não pode usar o modelo customizado para reconhecimento de voz.

    Para uma solução alternativa desse problema, o usuário deve usar o método `POST /v1/customizations/{customization_id}/upgrade_model` ou `POST /v1/acoustic_customizations/{customization_id}/upgrade_model` para fazer upgrade do modelo customizado para a versão mais recente de seu modelo base. Então, o usuário pode usar o modelo customizado com reconhecimento de voz.

Os dois problemas foram corrigidos na produção.

### 7 de setembro de 2018
{: #September2018a}

A interface REST de HTTP baseada em sessão não é mais suportada. Todas as informações relacionadas às sessões foram removidas da documentação. Os métodos a seguir não estão mais disponíveis:
{: important}

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

Se seu aplicativo usar a interface de sessões, você deverá migrar para uma das interfaces REST de HTTP restantes ou para a interface do WebSocket. Para obter mais informações, consulte a atualização de serviço para [8 de agosto de 2018](#August2018).

### 8 de agosto de 2018
{: #August2018}

A interface REST de HTTP baseada em sessão foi descontinuada a partir de **8 de agosto de 2018**. Todos os métodos da API de sessões serão removidos do serviço a partir de **7 de setembro de 2018**. Após essa data, não será mais possível usar a interface baseada em sessão. Este aviso de descontinuação imediata e de remoção em 30 dias se aplica aos métodos a seguir:

-   `POST /v1/sessions`
-   `POST /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/recognize`
-   `GET /v1/sessions/{session_id}/observe_result`
-   `DELETE /v1/sessions/{session_id}`

Se seu aplicativo usar a interface de sessões, você deverá migrar para uma das interfaces a seguir até 7 de setembro:
{: important}

-   Para reconhecimento de voz baseado em fluxo (incluindo casos de uso em tempo real), use a [interface do WebSocket](/docs/services/speech-to-text/websockets.html), que fornece acesso aos resultados provisórios e à mais baixa latência.
-   Para reconhecimento de voz baseado em arquivo, use uma das interfaces a seguir:

    -   Para arquivos mais curtos de até alguns poucos minutos de áudio, use a [interface de HTTP síncrona](/docs/services/speech-to-text/http.html)`(POST /v1/recognize`) ou a [interface de HTTP assíncrona](/docs/services/speech-to-text/async.html) (`POST /v1/recognitions`).
    -   Para arquivos mais longos de mais de alguns poucos minutos de áudio, use a interface de HTTP assíncrona. A interface de HTTP assíncrona aceita o máximo de 1 GB de dados de áudio com uma única solicitação.

As interfaces do WebSocket e HTTP fornecem os mesmos resultados que a interface de sessões (somente a interface do WebSocket fornece resultados provisórios). Também é possível usar um dos SDKs do Watson, que simplificam o desenvolvimento de aplicativos com qualquer uma das interfaces. Para obter mais informações, veja a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 13 de julho de 2018
{: #July2018}

O modelo de banda estreita de espanhol, `es-ES_NarrowbandModel`, foi atualizado para reconhecimento de voz melhorado. Por padrão, o serviço usa automaticamente o modelo atualizado para todas as solicitações de reconhecimento. Se você tiver modelos customizados acústicos ou de idioma que são baseados nesse modelo, deverá fazer upgrade de seus modelos customizados para aproveitar as atualizações usando os métodos a seguir:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Para obter mais informações, consulte [Fazendo upgrade de modelos customizados](/docs/services/speech-to-text/custom-upgrade.html).

No que se refere a essa atualização, estão disponíveis as seguintes duas versões do modelo de banda estreita de espanhol:

-   `es_ES.8kHz.general.lm20180522235959.am20180522235959` (atual)
-   `es_ES.8kHz.general.lm20180308235959.am20180308235959` (anterior)

A versão a seguir do modelo não está mais disponível:

-   `es_ES.8kHz.general.lm20171031235959.am20171031235959`

Uma solicitação de reconhecimento que tenta usar um modelo customizado que é baseado no modelo base que agora está indisponível, usa o modelo base mais recente sem nenhuma customização. O serviço retorna a mensagem de aviso a seguir: `Using non-customized default base model, because your custom {type} model has been built with a version of the base model that is no longer supported.` Para continuar usando um modelo customizado que é baseado no modelo indisponível, deve-se primeiro fazer upgrade do modelo usando o método `upgrade_model` apropriado descrito anteriormente.

### 12 de junho de 2018
{: #June2018}

Os recursos a seguir são ativados para aplicativos que estão hospedados em Washington, DC (**us-east**):

-   O serviço agora suporta um novo processo de autenticação de API. Para obter mais informações, consulte a [atualização de serviço de 30 de outubro de 2018](#October2018b).
-   O serviço agora suporta o cabeçalho `X-Watson-Metadata` e o método `DELETE /v1/user_data`. Para obter mais informações, consulte [Segurança de informações](/docs/services/speech-to-text/information-security.html).

### 15 de maio de 2018
{: #May2018}

Os recursos a seguir são ativados para aplicativos que estão hospedados em Sydney (**au-syd**):

-   O serviço agora suporta um novo processo de autenticação de API. Para obter mais informações, consulte a [atualização de serviço de 30 de outubro de 2018](#October2018b).
-   O serviço agora suporta o cabeçalho `X-Watson-Metadata` e o método `DELETE /v1/user_data`. Para obter mais informações, consulte [Segurança de informações](/docs/services/speech-to-text/information-security.html).

### 26 de março de 2018
{: #March2018b}

-   O serviço agora suporta a customização do modelo de idioma para o modelo de idioma francês, `fr-FR_BroadbandModel`. O modelo francês está geralmente disponível para uso de produção com customização do modelo de idioma.
    -   Para obter mais informações sobre como o serviço analisa os corpora para francês, consulte [Análise de inglês, francês, alemão, espanhol e português do Brasil](/docs/services/speech-to-text/language-resource.html#corpusLanguages).
    -   Para obter mais informações sobre como criar pronúncias para palavras customizadas em francês, consulte [Diretrizes para francês, alemão, espanhol e português do Brasil](/docs/services/speech-to-text/language-resource.html#wordLanguages-esES-frFR).
-   Os modelos de banda estreita de espanhol e coreano, `es-ES_NarrowbandModel` e `ko-KR_NarrowbandModel` e o modelo de banda larga de francês, `fr-FR_BroadbandModel`, foram atualizados para reconhecimento de voz melhorado. Por padrão, o serviço usa automaticamente os modelos atualizados para todas as solicitações de reconhecimento. Se você tiver modelos customizados acústicos ou de idioma que se baseiam em qualquer um desses modelos, deverá fazer upgrade de seus modelos customizados para aproveitar as atualizações usando os métodos a seguir:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Para obter mais informações, consulte [Fazendo upgrade de modelos customizados](/docs/services/speech-to-text/custom-upgrade.html).
-   O parâmetro `version` dos métodos a seguir foi renomeado para `base_model_version`:

    -   `/v1/recognize` para solicitações do WebSocket
    -   `POST /v1/recognize` para solicitações de HTTP sem sessão
    -   `POST /v1/sessions` para solicitações de HTTP baseados em sessão
    -   `POST /v1/recognitions` para solicitações de HTTP assíncronas

    O parâmetro `base_model_version` especifica a versão de um modelo base que deve ser usada para reconhecimento de voz. Para obter mais informações, consulte [Fazendo solicitações de reconhecimento com modelos customizados submetidos a upgrade](/docs/services/speech-to-text/custom-upgrade.html#upgradeRecognition) e [Versão do modelo base](/docs/services/speech-to-text/input.html#version).
-   A formatação inteligente agora é suportada para espanhol, bem como para inglês dos EUA. Para inglês dos EUA, agora o recurso também converte sequências de palavras-chave em símbolos de pontuação para pontos, vírgulas, pontos de interrogação e pontos de exclamação. Para obter mais informações, consulte [Formatação inteligente](/docs/services/speech-to-text/output.html#smart_formatting).

### 1º de março de 2018
{: #March2018a}

Os modelos de banda larga de espanhol e francês, `es-ES_BroadbandModel` e `fr-FR_BroadbandModel`, foram atualizados para reconhecimento de voz melhorado. Por padrão, o serviço usa automaticamente os modelos atualizados para todas as solicitações de reconhecimento. Se você tiver modelos customizados acústicos ou de idioma que se baseiam em qualquer um desses modelos, deverá fazer upgrade de seus modelos customizados para aproveitar as atualizações usando os métodos a seguir:

-   `POST /v1/customizations/{customization_id}/upgrade_model`
-   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

Para obter mais informações, consulte [Fazendo upgrade de modelos customizados](/docs/services/speech-to-text/custom-upgrade.html). A seção apresenta regras para fazer upgrade de modelos customizados, os efeitos do upgrade e as abordagens para usar modelos que passaram por upgrade.

### 1º de fevereiro de 2018
{: #February2018}

O serviço agora oferece modelos para o idioma coreano para reconhecimento de voz: `ko-KR_BroadbandModel` para áudio que é amostrado em 16 kHz, no mínimo, e `ko-KR_NarrowbandModel` para áudio que é amostrado em 8 kHz, no mínimo. Para obter mais informações, consulte [Idiomas e modelos](/docs/services/speech-to-text/models.html).

Para a customização do modelo de idioma, os modelos coreanos estão geralmente disponíveis para uso de produção. Para a customização do modelo acústico, eles são da funcionalidade beta. Para obter mais informações, consulte [Suporte ao idioma para customização](/docs/services/speech-to-text/custom.html#languageSupport).

-   Para obter mais informações sobre como o serviço analisa os corpora para o coreano, consulte [Análise do coreano](/docs/services/speech-to-text/language-resource.html#corpusLanguages-koKR).
-   Para obter mais informações sobre como criar pronúncias para palavras customizadas em coreano, consulte [Diretrizes para coreano](/docs/services/speech-to-text/language-resource.html#wordLanguages-koKR).

### 14 de dezembro de 2017
{: #December2017}

-   Os modelos em inglês dos EUA, `en-US_BroadbandModel` e `en-US_NarrowbandModel`, foram atualizados para reconhecimento de voz melhorado. Por padrão, o serviço usa automaticamente os modelos atualizados para todas as solicitações de reconhecimento. Se você tiver modelos customizados acústicos ou de idioma que se baseiam nos modelos em inglês dos EUA, deverá fazer upgrade de seus modelos customizados para aproveitar as atualizações usando os métodos a seguir:

    -   `POST /v1/customizations/{customization_id}/upgrade_model`
    -   `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`

    Para obter mais informações sobre o procedimento, consulte [Fazendo upgrade de modelos customizados](/docs/services/speech-to-text/custom-upgrade.html). A seção apresenta regras para fazer upgrade de modelos customizados, os efeitos do upgrade e as abordagens para usar modelos que passaram por upgrade. Atualmente, os métodos se aplicam apenas aos novos modelos base em inglês dos EUA. Mas as mesmas informações se aplicarão a upgrades de outros modelos base à medida que se tornarem disponíveis.
-   Os vários métodos para fazer solicitações de reconhecimento agora incluem um novo parâmetro `base_model_version` que pode ser usado para iniciar solicitações que usam as versões mais antigas ou com upgrade dos modelos base e customizados. Embora ele seja destinado principalmente para uso com modelos customizados que foram submetidos a upgrade, o parâmetro `base_model_version` também pode ser usado sem modelos customizados. Para obter mais informações, consulte [Versão do modelo base](/docs/services/speech-to-text/input.html#version).
-   O serviço agora suporta a customização do modelo acústico como funcionalidade beta para todos os idiomas disponíveis. É possível criar modelos acústicos customizados para modelos de banda larga ou de banda estreita para todos os idiomas. Para obter uma introdução à customização, incluindo a customização do modelo acústico, consulte [A interface de customização](/docs/services/speech-to-text/custom.html).
-   O serviço agora suporta a customização do modelo de idioma para os modelos de inglês do Reino Unido, `en-GB_BroadbandModel` e `en-GB_NarrowbandModel`. Embora o serviço manipule os corpora e as palavras customizadas em inglês do Reino Unido e dos EUA de um modo geralmente semelhante, existem algumas diferenças importantes:
    -   Para obter mais informações sobre como o serviço analisa os corpora para inglês do Reino Unido, consulte [Análise do inglês, francês, alemão, espanhol e português do Brasil](/docs/services/speech-to-text/language-resource.html#corpusLanguages).
    -   Para obter mais informações sobre como criar pronúncias para palavras customizadas em inglês do Reino Unido, consulte [Diretrizes para inglês (EUA e Reino Unido)](/docs/services/speech-to-text/language-resource.html#wordLanguages-enUS-enGB). Especificamente, para inglês do Reino Unido, não é possível usar pontos ou traços em pronúncias.
-   A customização do modelo de idioma e todos os parâmetros associados estão agora geralmente disponíveis (GA) para todos os idiomas suportados: japonês, espanhol, inglês do Reino Unido e inglês dos EUA.

### 2 de outubro de 2017
{: #October2017}

-   A interface de customização agora oferece customização do modelo acústico. É possível criar modelos acústicos customizados que adaptam os modelos base do serviço ao ambiente e aos falantes. Você preenche e treina um modelo acústico customizado no áudio que corresponde mais de perto à assinatura acústica do áudio que você deseja transcrever. Em seguida, use o modelo acústico customizado com solicitações de reconhecimento para aumentar a precisão do reconhecimento de voz.

    Os modelos acústicos customizados complementam os modelos de idioma customizados. É possível treinar um modelo acústico customizado com um modelo de idioma customizado e usar ambos os tipos de modelo durante o reconhecimento de voz. A customização do modelo acústico é uma interface beta que está disponível apenas para inglês dos EUA, espanhol e japonês.

    -   Para obter mais informações sobre os idiomas que são suportados pela interface de customização e o nível de suporte que está disponível para cada idioma, consulte [Suporte ao idioma para customização](/docs/services/speech-to-text/custom.html#languageSupport).
    -   Para obter mais informações sobre a interface de customização do serviço, consulte [A interface de customização](/docs/services/speech-to-text/custom.html).
    -   Para obter mais informações sobre como criar um modelo acústico customizado, consulte [Criando um modelo acústico customizado](/docs/services/speech-to-text/acoustic-create.html).
    -   Para obter mais informações sobre como usar um modelo acústico customizado, consulte [Usando um modelo acústico customizado](/docs/services/speech-to-text/acoustic-use.html).
    -   Para obter mais informações sobre todos os métodos da interface de customização, consulte a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Para customização do modelo de idioma, o serviço agora inclui um recurso beta que configura um peso de customização opcional para um modelo de idioma customizado. Um peso de customização especifica o peso relativo a ser fornecido para palavras de um modelo de idioma customizado versus as palavras do vocabulário base do serviço. É possível configurar um peso de customização durante o reconhecimento de treinamento e de fala. Para obter mais informações, consulte [Usando o peso de customização](/docs/services/speech-to-text/language-use.html#weight).
-   O modelo de idioma `ja-JP_BroadbandModel` foi atualizado para capturar melhorias no modelo base. O upgrade não afeta modelos customizados existentes que são baseados no modelo.
-   O serviço agora inclui um parâmetro para especificar a ordenação do áudio que é enviado no formato `audio/l16` (Linear 16-bit Pulse-Code Modulation (PCM)). Além de especificar os parâmetros `rate` e `channels` com o formato, agora é possível especificar também `big-endian` ou `little-endian` com o parâmetro `endianness`. Para obter mais informações, consulte [Formatos de áudio](/docs/services/speech-to-text/audio-formats.html).

### 14 de julho de 2017
{: #July2017b}

-   O serviço agora suporta a transcrição do áudio no formato MP3 ou Motion Picture Experts Group (MPEG). Para obter mais informações sobre os formatos de áudio suportados, consulte [Formatos de áudio](/docs/services/speech-to-text/audio-formats.html).
-   A interface de customização do modelo de idioma agora suporta o espanhol como funcionalidade beta. É possível criar um modelo customizado com base em qualquer um dos modelos de idioma base de espanhol: `es-ES_BroadbandModel` ou `es-ES_NarrowbandModel`; para obter mais informações, consulte [Criando um modelo de idioma customizado](/docs/services/speech-to-text/language-create.html). A precificação para as solicitações de reconhecimento que usam modelos de idioma customizados de espanhol é a mesma que para as solicitações que usam os modelos em japonês e inglês dos EUA.
-   O objeto JSON `CreateLanguageModel` que você passa para o método `POST /v1/customizations` para criar um novo modelo de idioma customizado agora inclui um campo `dialect`. O campo especifica o dialeto do idioma que deve ser usado com o modelo customizado. Por padrão, o dialeto corresponde ao idioma do modelo base. O parâmetro é significativo somente para modelos de espanhol, para os quais o serviço pode criar um modelo customizado que é adequado para fala em um dos dialetos a seguir:
    -   `es-ES` para espanhol castelhano (o padrão)
    -   `es-LA` para espanhol da América Latina
    -   `es-US` para espanhol da América do Norte (mexicano)

    Os métodos `GET /v1/customizations` e `GET /v1/customizations/{customization_id}` da interface de customização incluem o dialeto de um modelo customizado em sua saída. Para obter mais informações, consulte [Criando um modelo de idioma customizado](/docs/services/speech-to-text/language-create.html#createModel-language) e [Listando modelos de idioma customizados](/docs/services/speech-to-text/language-models.html#listModels-language).
-   Os nomes dos modelos de idioma `en-UK_BroadbandModel` e `en-UK_NarrowbandModel` foram descontinuados. Os modelos estão agora disponíveis com os nomes `en-GB_BroadbandModel` e `en-GB_NarrowbandModel`.

    Os nomes `en-UK_{model}` descontinuados continuam a funcionar, mas o método `GET /v1/models` não retorna mais os nomes na lista de modelos disponíveis. Ainda é possível consultar os nomes diretamente com o método `GET /v1/models/{model_id}`.

### 1º de julho de 2017
{: #July2017a}

-   A interface de customização do modelo de idioma do serviço agora está geralmente disponível (GA) para ambos os idiomas suportados, inglês dos EUA e japonês. A {{site.data.keyword.IBM_notm}} não cobra pela criação, hospedagem ou gerenciamento de modelos de idioma customizados. Conforme descrito no próximo marcador, a {{site.data.keyword.IBM_notm}} agora cobra um extra de US$ 0,03 por minuto de áudio para solicitações de reconhecimento que usam modelos customizados.
-   A {{site.data.keyword.IBM_notm}} atualizou a precificação para o serviço
    -   Eliminando o preço do complemento para o uso de modelos de banda estreita
    -   Fornecendo a precificação em camadas graduadas para clientes de alto volume
    -   Cobrando um valor adicional de US$ 0,03 por minuto de áudio para solicitações de reconhecimento que usam modelos de idioma customizados de inglês dos EUA ou japonês

    Para obter mais informações sobre as atualizações de precificação, consulte
    -   A postagem do blog [API do {{site.data.keyword.IBM_notm}} {{site.data.keyword.watson}} {{site.data.keyword.speechtotextshort}} - Atualizações de precificação ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/blogs/bluemix/2017/05/ibm-watson-speech-text-api-pricing-updates/){: new_window}
    -   O serviço {{site.data.keyword.speechtotextshort}} no [ catálogo do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/catalog/services/speech-to-text){: new_window}
    -   As [Perguntas frequentes sobre precificação](/docs/services/speech-to-text/faq-pricing.html)
-   Não é mais necessário passar um objeto de dados vazio como o corpo para as solicitações `POST` a seguir:
    -   `POST /v1/sessions`
    -   `POST /v1/register_callback`
    -   `POST /v1/customizations/{customization_id}/train`
    -   `POST /v1/customizations/{customization_id}/reset`
    -   `POST /v1/customizations/{customization_id}/upgrade_model`

    Por exemplo, você agora chama o método `POST /v1/sessions` com `curl`, como a seguir:

    ```bash
    curl -X POST -u "{username}:{password}"
    --cookie-jar cookies.txt
    "https://stream.watsonplatform.net/speech-to-text/api/v1/sessions"
    ```
    {: pre}

    Não é mais necessário passar a opção `curl` com a solicitação: `--data "{}"`.

    Se você tiver algum problema com uma dessas solicitações `POST`, tente passar um objeto de dados vazio com o corpo da solicitação. Transmitir um objeto vazio não muda a natureza ou o significado da solicitação de nenhuma maneira.
    {: note}

### 22 de maio de 2017
{: #May2017}

As descontinuações a seguir anunciadas pela primeira vez em março de 2017 estão agora em vigor:

-   O parâmetro `continuous` é removido de todos os métodos que iniciam solicitações de reconhecimento. O serviço agora transcreve um fluxo de áudio inteiro até que ele termine ou atinja o tempo limite, o que ocorrer primeiro. Esse comportamento é equivalente a configurar o parâmetro `continuous` antigo como `true`. Por padrão, previamente o serviço parou a transcrição no primeiro meio segundo sem fala (geralmente silêncio) se o parâmetro foi omitido ou configurado como `false`.

    Os aplicativos existentes que configuraram o parâmetro como `true` não verão nenhuma mudança no comportamento. Os aplicativos que configuraram o parâmetro como `false` ou que contaram com o comportamento padrão provavelmente verão uma mudança. Se uma solicitação especificar o parâmetro, o serviço agora responderá retornando uma mensagem de aviso para o parâmetro desconhecido:

    ```javascript
    "warnings": [
      "Unknown arguments: continuous."
    ]
    ```
    {: codeblock}

    Apesar do aviso, a solicitação é bem-sucedida e uma sessão existente ou uma conexão do WebSocket não é afetada.

    A {{site.data.keyword.IBM_notm}} removeu o parâmetro para responder ao feedback esmagador da comunidade do desenvolvedor, pois especificar `continuous=false` acrescentaria pouco valor e poderia reduzir a precisão geral da transcrição.
-   Não é mais possível evitar um tempo limite da sessão sem enviar áudio:
    -   Quando você usa a interface do WebSocket, o cliente não pode mais manter uma conexão ativa enviando uma mensagem de texto JSON com o parâmetro `action` configurado como `no-op`. O envio de uma mensagem `no-op` não gera um erro, mas não tem efeito.
    -   Quando você usa sessões com a interface de HTTP, o cliente não pode mais estender a sessão enviando uma solicitação `GET /v1/sessions/{session_id}/recognize`. O método ainda retorna o status de uma sessão ativa, mas ele não mantém a sessão ativa.
    Agora é possível fazer o seguinte para manter uma sessão ativa:
    -   Configure o parâmetro `inactivity_timeout` como `-1` para evitar o tempo limite de inatividade de 30 segundos.
    -   Envie quaisquer dados de áudio, incluindo apenas silêncio, para o serviço para evitar o tempo limite de sessão de 30 segundos. Você é cobrado pela duração de qualquer dado que envia para o serviço, incluindo o silêncio enviado para estender uma sessão.

    Para obter mais informações, consulte [Tempos limites](/docs/services/speech-to-text/input.html#timeouts). O ideal seria você estabelecer uma sessão imediatamente antes de obter o áudio para transcrição e manter a sessão enviando o áudio a uma taxa próxima do tempo real. Além disso, certifique-se de que seu aplicativo se recupere normalmente de sessões ou conexões fechadas.

    A {{site.data.keyword.IBM_notm}} removeu essa funcionalidade para assegurar que ela continue a oferecer a todos os usuários um serviço de reconhecimento de voz de baixa latência excelente.

### 10 de abril de 2017
{: #April2017}

-   O serviço agora suporta o recurso de rótulos do falante para os modelos de banda larga a seguir:
    -   `en-US-BroadbandModel`
    -   `es-ES-BroadbandModel`
    -   `ja-JP_BroadbandModel`

    Para obter mais informações, consulte [Rótulos do falante](/docs/services/speech-to-text/output.html#speaker_labels).
-   O serviço agora suporta o formato de áudio do Web Media (WebM) com o codec Opus ou Vorbis. O serviço agora também suporta o formato de áudio Ogg com o codec Vorbis, além do codec Opus. Para obter mais informações sobre os formatos de áudio suportados, consulte [Formatos de áudio](/docs/services/speech-to-text/audio-formats.html).
-   O serviço agora suporta o Compartilhamento de Recurso de Origem Cruzada (CORS) para permitir que os clientes baseados no navegador chamem o serviço diretamente. Para obter mais informações, consulte [Suporte ao CORS](/docs/services/speech-to-text/developer-overview.html#cors).
-   A interface de HTTP assíncrona agora oferece um método `POST /v1/unregister_callback` que remove o registro para uma URL de retorno de chamada incluída na lista de desbloqueio. Para obter mais informações, consulte [Cancelando o registro de uma URL de retorno de chamada](/docs/services/speech-to-text/async.html#unregister).
-   A interface do WebSocket não atinge mais o tempo limite para obter solicitações de reconhecimento para arquivos de áudio especialmente longos. Não é mais necessário solicitar resultados provisórios com a mensagem JSON `start` para evitar o tempo limite. (Esse problema foi descrito nas notas sobre a liberação de 10 de março de 2016.)
-   O método `DELETE /v1/customizations/{customization_id}` agora retorna o código de resposta de HTTP 401 se você tentar excluir um modelo customizado não existente.
-   O método `DELETE /v1/customizations/{customization_id}/corpora/{corpus_name}` agora retorna o código de resposta de HTTP 400 se você tenta excluir um corpus não existente.

### 8 de março de 2017
{: #March2017}

A interface de HTTP assíncrona agora está geralmente disponível. Antes dessa data, era funcionalidade beta.

### 1º de dezembro de 2016
{: #December2016}

-   O serviço agora oferece um recurso beta de rótulo de falante para áudio de banda estreita em inglês dos EUA, espanhol ou japonês. O recurso identifica quais palavras cada um dos falantes falou em uma interação com múltiplas pessoas. Os métodos de reconhecimento sem sessão, baseados em sessão, assíncronos e do WebSocket incluem um parâmetro `speaker_labels` que aceita um valor booleano para indicar se os rótulos do falante devem ser incluídos na resposta. Para obter mais informações sobre o recurso, consulte [Rótulos do falante](/docs/services/speech-to-text/output.html#speaker_labels).
-   A interface de customização do modelo de idioma beta agora é suportada para japonês, além do inglês dos EUA. Todos os métodos da interface suportam o japonês. Para obter mais informações, consulte as seções a seguir:
    -   Para obter mais informações, consulte [Criando um modelo de idioma customizado](/docs/services/speech-to-text/language-create.html) e [Usando um modelo de idioma customizado](/docs/services/speech-to-text/language-use.html).
    -   Para considerações gerais e específicas do japonês para incluir um arquivo de texto do corpus, consulte [Preparando um arquivo de corpus](/docs/services/speech-to-text/language-resource.html#prepareCorpus) e [O que acontece ao incluir um arquivo de corpus?](/docs/services/speech-to-text/language-resource.html#parseCorpus)
    -   Para considerações específicas do japonês ao especificar o campo `sounds_like` para uma palavra customizada, consulte [Diretrizes para japonês](/docs/services/speech-to-text/language-resource.html#wordLanguages-jaJP).
    -   Para obter mais informações sobre todos os métodos da interface de customização, consulte a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   A interface de customização do modelo de idioma agora inclui um método `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` que lista informações sobre um corpus especificado. O método é útil para monitorar o status de uma solicitação para incluir um corpus em um modelo customizado. Para obter mais informações, consulte [Listando os corpora para um modelo de idioma customizado](/docs/services/speech-to-text/language-corpora.html#listCorpora).
-   A saída JSON que é retornada pelos métodos `GET /v1/customizations/{customization_id}/words` e `GET /v1/customizations/{customization_id}/words/{word_name}` agora incluem um campo `count` para cada palavra. O campo indica o número de vezes que a palavra é localizada em todos os corpora. Se você incluir uma palavra customizada em um modelo antes que ela seja incluída por quaisquer corpora, a contagem iniciará em `1`. Se a palavra for incluída de um corpus primeiro e posteriormente modificada, a contagem refletirá somente o número de vezes que ela é localizada no corpus. Para obter mais informações, consulte [Listando palavras de um modelo de idioma customizado](/docs/services/speech-to-text/language-words.html#listWords).

    Para modelos customizados que foram criados antes da existência do campo `count`, o campo sempre permanece em `0`. Para atualizar o campo para esses modelos, inclua o corpo do modelo novamente e inclua o parâmetro `allow_overwrite` com o método `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`.
-   O método `GET /v1/customizations/{customization_id}/words` agora inclui um parâmetro de consulta `sort` que controla a ordem na qual as palavras devem ser listadas. O parâmetro aceita dois argumentos, `alphabetical` ou `count`, para indicar como as palavras devem ser classificadas. É possível pré-anexar um `+` ou `-` opcional a um argumento para indicar se os resultados devem ser classificados em ordem crescente ou decrescente. Por padrão, o método exibe as palavras em ordem alfabética crescente. Para obter mais informações, consulte [Listando palavras de um modelo de idioma customizado](/docs/services/speech-to-text/language-words.html#listWords).

    Para modelos customizados criados antes da introdução do campo `count`, o uso do argumento `count` com o parâmetro `sort` é sem sentido. Use o argumento `alphabetical` padrão com esses modelos.
-   O campo `error` que pode ser retornado como parte da resposta JSON dos métodos `GET /v1/customizations/{customization_id}/words` e `GET /v1/customizations/{customization_id}/words/{word_name}` agora é uma matriz. Se o serviço descobriu um ou mais problemas com a definição de uma palavra customizada, o campo listará cada elemento do problema da definição e fornecerá uma mensagem descrevendo o problema. Para obter mais informações, consulte [Listando palavras de um modelo de idioma customizado](/docs/services/speech-to-text/language-words.html#listWords).
-   Os parâmetros `keywords_threshold` e `word_alternatives_threshold` dos métodos de reconhecimento não aceitam mais um valor nulo. Para omitir palavras-chave e alternativas de palavras da resposta, omita os parâmetros. Um valor especificado deve ser um valor flutuante.

Para obter mais informações sobre a interface do serviço, consulte a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 22 de setembro de 2016
{: #September2016}

-   O serviço agora oferece uma nova interface de customização do modelo de idioma beta para inglês dos EUA. É possível usar a interface para customizar o vocabulário base do serviço e os modelos de idioma por meio da criação de modelos de idioma customizados que incluem terminologia específica do domínio. É possível incluir palavras customizadas individualmente ou fazer o serviço extraí-las dos corpora. Para usar seus modelos customizados com os métodos de reconhecimento de voz que são oferecidos por qualquer uma das interfaces do serviço, transmita o parâmetro de consulta `customization_id`. Para
obter mais informações, consulte
    -   [Criando um modelo de idioma customizado](/docs/services/speech-to-text/language-create.html)
    -   [Usando um modelo de idioma customizado](/docs/services/speech-to-text/language-use.html)
    -   [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}
-   A lista de formatos de áudio suportados agora inclui `audio/mulaw`, que fornece um áudio de canal único codificado usando o algoritmo de dados u-law (ou mu-law). Quando você usa esse formato, também deve-se especificar a taxa de amostragem na qual o áudio é capturado. Para obter mais informações, consulte [Formatos de áudio](/docs/services/speech-to-text/audio-formats.html).
-   Os métodos `GET /v1/models` e `GET /v1/models/{model_id}` agora retornam um campo `supported_features` como parte de sua saída para cada modelo de idioma. As informações adicionais descrevem se o modelo suporta customização. Para obter mais informações, veja a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 30 de junho de 2016
{: #June2016b}

A interface de HTTP assíncrona beta agora suporta todos os idiomas que são suportados pelo serviço. A interface estava disponível anteriormente somente para inglês dos EUA. Para obter mais informações, consulte [A interface de HTTP assíncrona](/docs/services/speech-to-text/async.html) e a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

### 23 de junho de 2016
{: #June2016a}

-   Uma interface de HTTP assíncrona beta agora está disponível. A interface fornece recursos de reconhecimento integral para a transcrição em inglês dos EUA por meio de chamadas HTTP sem bloqueio. É possível registrar as URLs de retorno de chamada e fornecer sequências secretas especificadas pelo usuário para alcançar a autenticação e a integridade de dados com assinaturas digitais. Para obter mais informações, consulte [A interface de HTTP assíncrona](/docs/services/speech-to-text/async.html) e a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Um recurso de formatação inteligente beta que converte datas, horários, séries de dígitos e números, números de telefone, valores de moeda e endereços da Internet em representações mais convencionais nas transcrições finais. Você ativa o recurso configurando o parâmetro `smart_formatting` como `true` em uma solicitação de reconhecimento. O recurso é funcionalidade beta que está disponível somente para inglês dos EUA. Para obter mais informações, consulte [Formatação inteligente](/docs/services/speech-to-text/output.html#smart_formatting).
-   A lista de modelos suportados para reconhecimento de voz agora inclui `fr-FR_BroadbandModel` para áudio no idioma francês que é amostrado em 16 kHz, no mínimo. Para obter mais informações, consulte [Idiomas e modelos](/docs/services/speech-to-text/models.html).
-   A lista de formatos de áudio suportados agora inclui `audio/basic`. O formato fornece áudio de canal único que é codificado usando os dados de 8 bits u-law (ou mu-law) que são amostrados em 8 kHz. Para obter mais informações, consulte [Formatos de áudio](/docs/services/speech-to-text/audio-formats.html).
-   Os vários métodos de reconhecimento podem retornar uma resposta `warnings` que inclui mensagens sobre parâmetros de consulta inválidos ou campos JSON que estão incluídos em uma solicitação. O formato dos avisos mudou. Por exemplo, `"warnings": "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']."` agora é `"warnings": "Unknown arguments: {invalid_arg_1}, {invalid_arg_2}."`
-   Para solicitações `POST` de HTTP que não transmitem dados de outra forma para o serviço, deve-se incluir um corpo da solicitação vazio do formulário `{}`. Com o comando `curl`, você usa a opção `--data` para passar os dados vazios.

### 10 de março de 2016
{: #March2016}

-   Ambas as formas de transmissão de dados (entrega única e fluxo) agora impõem um limite de tamanho de 100 MB nos dados de áudio, assim como faz a interface do WebSocket. Anteriormente, a abordagem de entrega única tinha um limite máximo de 4 MB de dados. Para obter mais informações, consulte [Transmissão de áudio](/docs/services/speech-to-text/input.html#transmission) (para todas as interfaces) e [Enviar áudio e receber os resultados de reconhecimento](/docs/services/speech-to-text/websockets.html#WSaudio) (para a interface do WebSocket). A seção WebSocket também discute o tamanho máximo do quadro ou mensagem de 4 MB imposto pela interface do WebSocket.
-   A resposta JSON para uma solicitação de reconhecimento agora pode incluir uma matriz de mensagens de aviso para parâmetros de consulta inválidos ou campos JSON que estão incluídos em uma solicitação. Cada elemento da matriz é uma sequência que descreve a natureza do aviso seguido por uma matriz de sequências de argumentos inválidos. Por exemplo, `"warnings": [ "Unknown arguments: [u'{invalid_arg_1}', u'{invalid_arg_2}']." ]`. Para obter mais informações, veja a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   O *Speech Software Development Kit (SDK) beta do {{site.data.keyword.watson}} para o sistema operacional Apple&reg; iOS* foi descontinuado. Em vez disso, use o SDK do *{{site.data.keyword.watson}} para o sistema operacional Apple&reg; iOS*. O novo SDK está disponível do [repositório ios-sdk ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/watson-developer-cloud/ios-sdk){: new_window} no namespace `watson-developer-cloud` no GitHub.

A interface do WebSocket atualmente tem o problema conhecido a seguir:

-   O serviço pode levar minutos para produzir resultados finais para uma solicitação de reconhecimento para um arquivo de áudio especialmente longo. Para a interface do WebSocket, a conexão TCP subjacente permanece inativa enquanto o serviço prepara a resposta. Portanto, a conexão pode ser fechada devido a um tempo limite. Para evitar o tempo limite com a interface do WebSocket, solicite resultados provisórios (`\"interim_results\": \"true\"`) no JSON para a mensagem `start` para iniciar a solicitação. É possível descartar os resultados provisórios se você não precisar deles. Esse problema será resolvido em uma atualização futura.

### 19 de janeiro de 2016
{: #January2016}

O serviço foi atualizado para incluir um novo recurso de filtragem de profanidade em 19 de janeiro de 2016. Por padrão, o serviço censura a profanidade de seus resultados de transcrição para o áudio inglês dos EUA. Para obter mais informações, consulte [Filtragem de profanidade](/docs/services/speech-to-text/output.html#profanity_filter).

### 17 de dezembro de 2015
{: #December2015}

-   O serviço agora oferece um recurso de marcação de palavra-chave. É possível especificar uma matriz de sequências de palavras-chave que devem ser correspondidas no áudio de entrada. Também deve-se especificar um nível de confiança definido pelo usuário que uma palavra deve atender para ser considerada uma correspondência para uma palavra-chave. Para obter mais informações, consulte [Marcação de palavra-chave](/docs/services/speech-to-text/output.html#keyword_spotting).

    O recurso de marcação de palavra-chave é funcionalidade beta.
    {: note}
-   O serviço agora oferece um recurso de alternativas de palavra. O recurso retorna hipóteses alternativas para palavras na entrada de áudio que atendem um nível de confiança definido pelo usuário. Para obter mais informações, consulte [Alternativas de palavra](/docs/services/speech-to-text/output.html#word_alternatives).

    O recurso de alternativas de palavra é funcionalidade beta.
    {: note}
-   O serviço suporta mais idiomas com seus modelos de transcrição: `en-UK_BroadbandModel` e `en-UK_NarrowbandModel` para inglês do Reino Unido e `ar-AR_BroadbandModel` para árabe padrão moderno. Para obter mais informações, consulte [Idiomas e modelos](/docs/services/speech-to-text/models.html).
-   As solicitações de reconhecimento de HTTP não estão mais sujeitas a um tempo limite de plataforma de 10 minutos. O serviço agora mantém a conexão ativa enviando um caractere de espaço no objeto JSON de resposta a cada 20 segundos, desde que o reconhecimento esteja em andamento. Para obter mais informações, consulte [Tempos limites](/docs/services/speech-to-text/input.html#timeouts).
-   O serviço não retorna mais o código de status HTTP 490 para os métodos de HTTP baseados em sessão `GET /v1/sessions/{session_id}/observe_result` e `POST /v1/sessions/{session_id}/recognize`. O serviço agora responde com o código de status HTTP 400 no lugar.

    Nas respostas JSON que ele retorna para erros com métodos baseados em sessão, o serviço agora também inclui um novo campo `session_closed`. O campo será configurado como `true` se a sessão estiver fechada como resultado do erro. Para obter mais informações sobre os possíveis códigos de retorno para qualquer método, consulte a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
-   Quando você usa o comando `curl` para transcrever áudio com o serviço, não é mais necessário usar a opção `-- limit-rate` para transferir dados em uma taxa até 40.000 bytes por segundo.

### 21 de setembro de 2015
{: #September2015}

-   Dois novos SDKs móveis beta estão disponíveis para os serviços de fala. Os SDKs ativam os aplicativos móveis para interagir com os serviços {{site.data.keyword.speechtotextshort}} e {{site.data.keyword.texttospeechshort}}.
    -   O *{{site.data.keyword.watson}} Speech SDK para a plataforma Google Android&trade;* suporta o fluxo de áudio para o serviço {{site.data.keyword.speechtotextshort}} em tempo real e o recebimento de uma transcrição do áudio à medida que você fala. O projeto inclui um aplicativo de exemplo que demonstra a interação com ambos os serviços de fala. O SDK está disponível no [repositório speech-android-sdk ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/watson-developer-cloud/speech-android-sdk){: new_window} no namespace `watson-developer-cloud` no GitHub.
    -   O *{{site.data.keyword.watson}} Speech SDK para o sistema operacional Apple&reg; iOS* suporta o fluxo de áudio para o serviço {{site.data.keyword.speechtotextshort}} e o recebimento de uma transcrição do áudio em resposta. O SDK está disponível no [repositório speech-ios-sdk ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/watson-developer-cloud/speech-ios-sdk){: new_window} no namespace `watson-developer-cloud` no GitHub.

    Os dois SDKs suportam a autenticação com os serviços de fala usando as credenciais de serviço do {{site.data.keyword.cloud_notm}} ou um token de autenticação.

    Como os SDKs são beta, eles estão sujeitos a mudanças no futuro.
    {: note}
-   O serviço suporta dois novos idiomas, o português do Brasil e o chinês mandarim. Os modelos para esses novos idiomas são `pt-BR_BroadbandModel`, `pt-BR_NarrowbandModel`, `zh-CN_BroadbandModel` e `zh-CN_NarrowbandModel`. Para obter mais informações, consulte [Idiomas e modelos](/docs/services/speech-to-text/models.html).
-   As solicitações de HTTP `POST` `/v1/sessions/{session_id}/recognize` e `/v1/recognize`, bem como a solicitação do WebSocket `/v1/recognize`, suportam a transcrição de um novo tipo de mídia: `audio/ogg;codecs=opus` para os arquivos de formato Ogg que usam o codec Opus. Além disso, o formato `audio/wav` para os métodos agora suporta qualquer codificação. A restrição sobre o uso da codificação PCM linear foi removida. Para obter mais informações, consulte [Formatos de áudio](/docs/services/speech-to-text/audio-formats.html).
-   O serviço agora suporta a superação de tempos limites ao transcrever arquivos de áudio longos com a interface de HTTP. Ao usar sessões, é possível empregar um padrão de pesquisa detalhada especificando IDs de sequência com os métodos `GET /v1/sessions/{session_id}/observe_result` e `POST /v1/sessions/{session_id}/recognize` para tarefas de reconhecimento de longa execução. Usando o novo parâmetro `sequence_id` desses métodos, é possível solicitar resultados antes, durante ou depois de enviar uma solicitação de reconhecimento.
-   Para os modelos de idioma inglês dos EUA, `en_US_BroadbandModel` e `en_US_NarrowbandModel`, o serviço agora insere letras maiúsculas em nomes próprios. Por exemplo, o serviço retornaria agora o texto "Barack Obama se graduou na Universidade de Columbia" em vez de "barack obama se graduou na universidade de columbia". Essa mudança pode ser interessante para você se o seu aplicativo for sensível de alguma maneira às letras maiúsculas de nomes próprios.
-   A solicitação de HTTP `DELETE /v1/sessions/{session_id}` não retorna o código de status 415 "Tipo de mídia não suportado". Esse código de retorno foi removido da documentação para o método.

### 1º de julho de 2015
{: #July2015}

O serviço mudou de beta para disponibilidade geral (GA) em 1º de julho de 2015. As diferenças a seguir existem entre as versões beta e GA das APIs do {{site.data.keyword.speechtotextshort}}. A liberação do GA requer que os usuários atualizem para a nova versão do serviço.

-   A versão GA da API de HTTP é compatível com a versão beta. Será necessário mudar seu código do aplicativo existente somente se você especificou explicitamente um nome do modelo. Por exemplo, o código de amostra disponível para o serviço do GitHub incluiu a linha de código a seguir no arquivo `demo.js`:

    ```javascript
    model: 'WatsonModel'
    ```
    {: codeblock}

    Essa linha especificou o modelo padrão, `WatsonModel`, para a versão beta do serviço. Se seu aplicativo também especificou esse modelo, será necessário mudá-lo para usar um dos novos modelos que são suportados pela versão GA. Para obter mais informações, consulte o próximo marcador.
-   O serviço agora suporta um novo modelo de programação para interação direta entre um cliente e o serviço por meio de uma conexão do WebSocket. Usando esse modelo, um cliente pode obter um token de autenticação para se comunicar diretamente com o serviço. O token ignora a necessidade de um aplicativo proxy do lado do servidor no {{site.data.keyword.cloud_notm}} para chamar o serviço em nome do cliente. Os tokens são os meios preferidos para os clientes interagirem com o serviço.

    O serviço continua a suportar o modelo de programação antigo que dependia de um proxy do lado do servidor para retransmitir áudio e mensagens entre o cliente e o serviço. Mas o novo modelo é mais eficiente e fornece um rendimento mais alto. Para obter mais informações sobre o novo modelo de programação, consulte [Modelos de programação para os serviços do {{site.data.keyword.watson}}](/docs/services/watson/getting-started-develop.html).
-   Os métodos `POST /v1/sessions` e `POST /v1/recognize`, juntamente com o método `/v1/recognize` do WebSocket, agora suportam um parâmetro de consulta `model`. Você usa o parâmetro para especificar informações sobre o áudio:

    -   O idioma: *inglês*, *japonês* ou *espanhol*
    -   A taxa mínima de amostragem: *banda larga* (16 kHz) ou *estreita banda* (8 kHz)

    Para obter mais informações, consulte [Idiomas e modelos](/docs/services/speech-to-text/models.html).
-   O cabeçalho `Content-Type` dos métodos `recognize` agora suporta `audio/wav` para arquivos Waveform Audio File Format (WAV), além de `audio/flac` e `audio/l16`. Para obter mais informações sobre os formatos de áudio suportados, consulte [Formatos de áudio](/docs/services/speech-to-text/audio-formats.html).
-   Os métodos `recognize` agora suportam vários novos parâmetros de consulta que podem ser usados para customizar o serviço para adequar as necessidades do seu aplicativo:
    -   O parâmetro `inactivity_timeout` configura o valor de tempo limite em segundos após o qual o serviço fecha a conexão se ele detecta silêncio (nenhuma fala) no modo de fluxo. Por padrão, o serviço finaliza a sessão após 30 segundos de silêncio. Consulte [Tempos limites](/docs/services/speech-to-text/input.html#timeouts).
    -   O parâmetro `max_alternatives` instrui o serviço a retornar as *n* melhores hipóteses alternativas para a transcrição de áudio. Consulte [Alternativas máximas](/docs/services/speech-to-text/output.html#max_alternatives).
    -   O parâmetro `word_confidence` instrui o serviço a retornar uma pontuação de confiança para cada palavra da transcrição. Consulte [Confiança de palavra](/docs/services/speech-to-text/output.html#word_confidence).
    -   O parâmetro `timestamps` instrui o serviço a retornar o horário de início e de encerramento com relação ao início do áudio para cada palavra da transcrição. Consulte [Registros de data e hora de palavra](/docs/services/speech-to-text/output.html#word_timestamps).
-   O método `GET /v1/sessions/{session_id}/observeResult` agora é denominado `GET /v1/sessions/{session_id}/observe_result`. O nome `observeResult` ainda é suportado para compatibilidade com versões anteriores.
-   Os métodos `GET /v1/sessions/{session_id}/observe_result`, `POST /v1/sessions/{session_id}/recognize` e `POST /v1/recognize` agora incluem o parâmetro de cabeçalho `X-WDC-PL-OPT-OUT` para controlar se o serviço usa os dados de áudio e transcrição de uma solicitação para melhorar os resultados futuros. A interface do WebSocket inclui um parâmetro de consulta equivalente. Especifique um valor de `1` para evitar que o serviço use os resultados de áudio e transcrição. O parâmetro se aplica somente à solicitação atual. O novo cabeçalho substitui o cabeçalho `X-logging` da API beta. Consulte [Controlando a criação de log de solicitação para os serviços do {{site.data.keyword.watson}}](../common/getting-started-logging.html).
-   O serviço agora tem um limite de 100 MB de dados por sessão no modo de fluxo. Você especifica o modo de fluxo especificando o valor `chunked` com o cabeçalho `Transfer-Encoding`. A entrega única de um arquivo de áudio ainda impõe um limite de tamanho de 4 MB nos dados que são enviados. Consulte [Transmissão de áudio](/docs/services/speech-to-text/input.html#transmission).
-   Para os métodos `/v1/models`, `/v1/models/{model_id}`, `/v1/sessions`, `/v1/sessions/{session_id}`, `/v1/sessions/{session_id}/observe_result`, `/v1/sessions/{session_id}/recognize` e `/v1/recognize`, o código de erro 415 ("Tipo de mídia não suportado") foi incluído.
-   Para as solicitações `POST` e `GET` para o método `/v1/sessions/{session_id}/recognize`, os códigos de erro a seguir foram modificados:
    -   O código de erro 404 ("Session_id não localizado") tem uma mensagem mais descritiva (`POST` e `GET`).
    -   Código de erro 503 ("A sessão já está processando uma solicitação. Solicitações simultâneas não são permitidas na mesma sessão. A sessão permanece ativa após esse erro".) tem uma mensagem mais descritiva (apenas`POST`).
-   Para solicitações `POST` de HTTP para os métodos `/v1/sessions` e `/v1/recognize`, o código de erro 503 ("Serviço indisponível") pode ser retornado. O código de erro também pode ser retornado ao criar uma conexão do WebSocket com o método `/v1/recognize`.
