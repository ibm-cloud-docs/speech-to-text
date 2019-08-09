---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-21"

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

# A interface de customização
{: #customization}

O serviço {{site.data.keyword.speechtotextfull}} oferece uma interface de customização que pode ser usada para aumentar os recursos de reconhecimento de voz. É possível usar a customização para melhorar a precisão das solicitações de reconhecimento de voz, customizando um modelo base para o seu domínio e áudio. A customização está disponível apenas para alguns idiomas e em diferentes níveis de suporte para idiomas diferentes. Consulte [Suporte ao idioma para customização](#languageSupport).
{: shortdesc}

A interface de customização suporta os modelos acústico e de idioma customizados. As interfaces para ambos os tipos de modelo customizado são semelhantes e fáceis de usar. Usar qualquer tipo de modelo customizado com uma solicitação de reconhecimento também é simples: você especifica o ID de customização do modelo com a solicitação.

O reconhecimento de voz funciona da mesma maneira com ou sem um modelo customizado. Quando você usa um modelo customizado para reconhecimento de voz, é possível usar todos os parâmetros de entrada e saída que estão normalmente disponíveis com uma solicitação de reconhecimento. Para obter mais informações sobre todos os parâmetros disponíveis, consulte o [Resumo de parâmetro](/docs/services/speech-to-text?topic=speech-to-text-summary).

Deve-se ter o plano de precificação Standard para usar a customização do modelo de idioma ou modelo acústico. Os usuários do plano Lite não podem usar a interface de customização. Para obter mais informações, consulte a [página de precificação](https://www.ibm.com/cloud/watson-speech-to-text/pricing){: external} para o serviço {{site.data.keyword.speechtotextshort}}.
{: note}

## Customização do modelo de idioma
{: #customLanguage-intro}

O serviço foi desenvolvido com um público amplo e geral em mente. O vocabulário base do serviço contém muitas palavras que são usadas na conversa diária. Seus modelos fornecem reconhecimento suficientemente preciso para muitos aplicativos. Mas eles podem não ter conhecimento de termos específicos associados a domínios específicos.

A interface de *customização do modelo de idioma* pode melhorar a precisão do reconhecimento de voz para domínios como medicina, direito, tecnologia da informação e outros. Usando a customização do modelo de idioma, é possível expandir e padronizar o vocabulário de um modelo base para incluir terminologia específica do domínio.

Você cria um modelo de idioma customizado e inclui corpora e palavras específicas para seu domínio. Depois de treinar o modelo de idioma customizado em seu vocabulário aprimorado, é possível usá-lo para reconhecimento de voz customizado. Geralmente, o serviço pode treinar qualquer modelo customizado em uma questão de minutos. O nível de esforço necessário para criar um modelo depende dos dados que você tem disponíveis para o modelo.

Para
obter mais informações, consulte

-   [Criando um modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-languageCreate)
-   [Usando um modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-languageUse)

## Customização do modelo acústico
{: #customAcoustic-intro}

Da mesma forma, o serviço foi desenvolvido com modelos acústicos básicos que funcionam bem para várias características de áudio. Mas, em casos como o seguinte, a adaptação de um modelo base para se adequar ao seu áudio pode melhorar o reconhecimento de voz:

-   Seu ambiente de canal acústico é exclusivo. Por exemplo, o ambiente tem ruído, a qualidade do microfone ou o posicionamento não são ideais ou o áudio sofre dos efeitos da distância.
-   Os padrões de fala dos seus falantes são atípicos. Por exemplo, um falante fala anormalmente rápido ou o áudio inclui conversas informais.
-   Os sotaques de seus falantes são acentuados. Por exemplo, o áudio inclui falantes que estão falando em um idioma não nativo ou segundo idioma.

A interface de *customização do modelo acústico* pode adaptar um modelo base para o seu ambiente e os seus falantes. Crie um modelo acústico customizado e inclua dados de áudio (recursos de áudio) que correspondam de perto à assinatura acústica do áudio que você deseja transcrever. Depois de treinar o modelo acústico customizado com os recursos de áudio, é possível usá-lo para reconhecimento de voz customizado.

O período de tempo que o serviço leva para treinar o modelo customizado depende de quanto dados de áudio o modelo contém. Em geral, o treinamento demora duas vezes o comprimento do áudio acumulativo. O nível de esforço necessário para criar um modelo depende dos dados de áudio que você tem disponíveis para o modelo. Isso também depende se você usa as transcrições do áudio.

Para
obter mais informações, consulte

-   [Criando um modelo acústico customizado](/docs/services/speech-to-text?topic=speech-to-text-acoustic)
-   [Usando um modelo acústico customizado](/docs/services/speech-to-text?topic=speech-to-text-acousticUse)

## Gramáticas
{: #grammars-intro}

Os modelos de idioma customizados permitem expandir o vocabulário base do serviço. As *gramáticas* permitem que você restrinja as palavras que o serviço pode reconhecer desse vocabulário. Quando você usa uma gramática com um modelo de idioma customizado para reconhecimento de voz, o serviço pode reconhecer apenas palavras, frases e sequências que são reconhecidas pela gramática. Como a gramática define um espaço de procura limitado para correspondências válidas, o serviço pode entregar os resultados mais rapidamente e com mais precisão.

Você inclui uma gramática em um modelo de idioma customizado e treina o modelo do mesmo modo que você faz para um corpus. Diferentemente de um corpus, no entanto, deve-se especificar explicitamente que uma gramática deve ser usada com um modelo customizado durante o reconhecimento de voz.

Para
obter mais informações, consulte

-   [Usando gramáticas com modelos de idioma customizados](/docs/services/speech-to-text?topic=speech-to-text-grammars)
-   [Incluindo uma gramática em um modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd)
-   [Usando uma gramática para reconhecimento de voz](/docs/services/speech-to-text?topic=speech-to-text-grammarUse)

## Usando a customização acústica e de idioma juntas
{: #combined}

Usar um modelo acústico customizado sozinho pode melhorar os recursos de reconhecimento do serviço. Mas se as transcrições ou corpora relacionados estiverem disponíveis para seu áudio de exemplo, será possível usar os dados para melhorar ainda mais a qualidade do reconhecimento de voz com base no modelo acústico customizado.

Ao criar um modelo de idioma customizado que complementa seu modelo acústico customizado, é possível aprimorar o reconhecimento de voz usando os dois modelos juntos. Ao treinar um modelo acústico customizado, é possível especificar um modelo de idioma customizado que inclua as transcrições dos recursos de áudio ou um vocabulário de palavras específicas do domínio por meio dos recursos. Da mesma forma, ao transcrever o áudio, o serviço aceita um modelo de idioma customizado, um modelo acústico customizado, ou ambos. E se seu modelo de idioma customizado incluir uma gramática, será possível usar esse modelo e gramática com um modelo acústico customizado para reconhecimento de voz.

Para obter mais informações, consulte [Usando modelos acústicos e de idioma customizados juntos](/docs/services/speech-to-text?topic=speech-to-text-useBoth).

Alguns idiomas não suportam a customização acústica e de idioma. Para obter mais informações, consulte [Suporte ao idioma para customização](#languageSupport).
{: note}

## Suporte ao idioma para customização
{: #languageSupport}

A customização de idioma e modelo acústico está disponível apenas para alguns idiomas. A tabela a seguir documenta o nível no qual o serviço suporta a customização para cada idioma.

-   *GA* indica que a interface está geralmente disponível para uso de produção.
-   *Beta* indica que a interface está disponível como uma oferta beta.
-   *Não suportado* significa que a interface não está disponível para esse idioma.

É possível usar os modelos de banda larga e de banda estreita com qualquer idioma suportado para o qual eles estão disponíveis. Se um idioma suportar customização do modelo de idioma, ele também suportará gramáticas. Para obter uma lista de todos os modelos disponíveis, consulte [Modelos de idiomas suportados](/docs/services/speech-to-text?topic=speech-to-text-models#modelsList).

<table>
  <caption>Tabela 1. Suporte ao idioma para customização</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 24%">
      idioma
    </th>
    <th style="text-align:center; vertical-align:bottom; width 37%">
      Suporte para customização do modelo de idioma
    </th>
    <th style="text-align:center; vertical-align:bottom; width 37%">
      Suporte para customização do modelo acústico
    </th>
  </tr>
  <tr>
    <td>Árabe (padrão moderno)</td>
    <td style="text-align:center">Não suportado</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Português do Brasil</td>
    <td style="text-align:center">disponibilidade geral</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Chinês (mandarim)</td>
    <td style="text-align:center">Não suportado</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Inglês (Reino Unido)</td>
    <td style="text-align:center">disponibilidade geral</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Inglês (Estados Unidos)</td>
    <td style="text-align:center">disponibilidade geral</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Francês</td>
    <td style="text-align:center">disponibilidade geral</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Alemão</td>
    <td style="text-align:center">disponibilidade geral</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Japonês</td>
    <td style="text-align:center">disponibilidade geral</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Coreano</td>
    <td style="text-align:center">disponibilidade geral</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Espanhol (argentino)</td>
    <td style="text-align:center">Beta</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Espanhol (castelhano)</td>
    <td style="text-align:center">disponibilidade geral</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Espanhol (chileno)</td>
    <td style="text-align:center">Beta</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Espanhol (colombiano)</td>
    <td style="text-align:center">Beta</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Espanhol (mexicano)</td>
    <td style="text-align:center">Beta</td>
    <td style="text-align:center">Beta</td>
  </tr>
  <tr>
    <td>Espanhol (peruano)</td>
    <td style="text-align:center">Beta</td>
    <td style="text-align:center">Beta</td>
  </tr>
</table>

É possível usar os métodos `GET /v1/models` e `GET /v1/models/{model_id}` para verificar se um modelo de idioma suporta customização do modelo de idioma. Se o modelo base suportar a customização do modelo de idioma, o campo `supported_features` da saída do método para o modelo configurará o campo `custom_language_model` como `true`.

## Observações de uso para customização
{: #customUsage}

As notas de uso a seguir se aplicam à customização de modelo de idioma e à customização de modelo acústico.

### Propriedade de modelos customizados
{: #customOwner}

Um modelo customizado é de propriedade da instância do serviço do {{site.data.keyword.speechtotextshort}} cujas credenciais são usadas para criá-lo. Para trabalhar com o modelo customizado de qualquer maneira, deve-se usar credenciais para essa instância do serviço com métodos da interface de customização. Credenciais que são criadas para outras instâncias do serviço não podem visualizar ou acessar o modelo customizado.

Todas as credenciais que são obtidas para a mesma instância do serviço {{site.data.keyword.speechtotextshort}} compartilham o acesso para todos os modelos customizados criados para essa instância de serviço. Para restringir o acesso a um modelo customizado, crie uma instância separada do serviço e use apenas as credenciais para essa instância de serviço para criar e trabalhar com o modelo. Credenciais para outras instâncias de serviço não podem afetar o modelo customizado.

Uma vantagem de compartilhar a propriedade entre as credenciais para uma instância de serviço é que é possível cancelar um conjunto de credenciais, por exemplo, se elas ficarem comprometidas. É possível, então, criar novas credenciais para a mesma instância de serviço e ainda manter a propriedade e o acesso aos modelos customizados criados com as credenciais originais.

### Criação de log e privacidade de dados da solicitação
{: #customLogging}

Como o serviço manipula a criação de log da solicitação para chamadas para a interface de customização depende da solicitação:

-   O serviço *não* registra dados que são usados para construir modelos customizados. Por exemplo, ao trabalhar com corpora e palavras em um modelo de idioma customizado, não é necessário configurar o cabeçalho da solicitação `X-Watson-Learning-Opt-Out`. Seus dados de treinamento nunca são usados para melhorar os modelos base do serviço.
-   O serviço *registra* os dados quando um modelo customizado é usado com uma solicitação de reconhecimento. Deve-se configurar o cabeçalho da solicitação `X-Watson-Learning-Opt-Out` como `true` para evitar a criação de log para solicitações de reconhecimento.

Para obter mais informações, consulte [Criação de log de solicitação](/docs/services/speech-to-text?topic=speech-to-text-input#logging).

### Segurança de informações
{: #customSecurity}

É possível associar um ID do cliente aos dados que são incluídos ou que passam por upgrade para os modelos acústico e de idioma customizados. Associe um ID do cliente à corpora, palavras customizadas, gramáticas e recursos de áudio passando o cabeçalho `X-Watson-Metadata` com os métodos a seguir:

-   `POST /v1/customizations/{customization_id}/corpora/{corpus_name}`
-   `POST /v1/customizations/{customization_id}/words`
-   `PUT /v1/customizations/{customization_id}/words/{word_name}`
-   `POST /v1/customizations/{customization_id}/grammars/{grammar_name}`
-   `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}`

Se necessário, será possível excluir os dados usando o método `DELETE /v1/user_data`. Para obter mais informações, consulte [Segurança de informações](/docs/services/speech-to-text?topic=speech-to-text-information-security).
