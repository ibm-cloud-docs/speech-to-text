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

# Criando um modelo de idioma customizado
{: #languageCreate}

Siga estas etapas para criar um modelo de idioma customizado para o serviço {{site.data.keyword.speechtotextshort}}:
{: shortdesc}

1.  [Criar um modelo de idioma customizado](#createModel-language). É possível criar múltiplos modelos customizados para os mesmos domínios ou domínios diferentes. O processo é o mesmo para qualquer modelo que você criar. No entanto, é possível especificar apenas um único modelo customizado por vez com uma solicitação de reconhecimento.
1.  [Incluir um corpus no modelo de idioma customizado](#addCorpus). Um corpus é um documento de texto simples que usa a terminologia do domínio no contexto. O serviço constrói um vocabulário para um modelo customizado, extraindo termos dos corpora que não existem em seu vocabulário base. É possível incluir múltiplos corpora em um modelo customizado.
1.  [Incluir palavras no modelo de idioma customizado](#addWords). Também é possível incluir palavras customizadas em um modelo individualmente. Além disso, é possível usar os mesmos métodos para modificar palavras customizadas que são extraídas dos corpora. Os métodos permitem especificar a pronúncia das palavras e como elas são exibidas em uma transcrição de fala.
1.  [Treinar o modelo de idioma customizado](#trainModel-language). Depois de incluir palavras no modelo customizado, deve-se treinar o modelo com as palavras. O treinamento prepara o modelo customizado para uso no reconhecimento de voz. O modelo não usa palavras novas ou modificadas até você treiná-lo.
1.  Depois de treinar seu modelo customizado, é possível usá-lo com solicitações de reconhecimento. Se o áudio passado para transcrição contiver palavras específicas do domínio que são definidas no modelo customizado, os resultados da solicitação refletirão o vocabulário aprimorado do modelo. Para obter mais informações, consulte [Usando um modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-languageUse).

As etapas para criar um modelo de idioma customizado são iterativas. É possível incluir corpora e palavras e treinar ou retreinar um modelo com a frequência necessária.

Também é possível incluir gramáticas em um modelo de idioma customizado. As gramáticas restringem a resposta do serviço a apenas aquelas palavras que são reconhecidas por uma gramática. Para obter mais informações, consulte [Usando gramáticas com modelos de idioma customizados](/docs/services/speech-to-text?topic=speech-to-text-grammars).

A customização do modelo de idioma está disponível para a maioria dos idiomas. Para obter mais informações, consulte [Suporte ao idioma para customização](/docs/services/speech-to-text?topic=speech-to-text-customization#languageSupport).
{: note}

## Criar um modelo de idioma customizado
{: #createModel-language}

Use o método `POST /v1/customizations` para criar um novo modelo de idioma customizado. É possível criar qualquer número de modelos de idioma customizados, mas é possível usar apenas um modelo por vez com uma solicitação de reconhecimento de voz. O método aceita um objeto JSON que define os atributos do novo modelo customizado como o corpo da solicitação.

<table>
  <caption>Tabela 1. Atributos de um novo modelo de idioma customizado</caption>
  <tr>
    <th style="width:20%; text-align:left">Parâmetros</th>
    <th style="width:12%; text-align:center">Tipo de Dados</th>
    <th style="text-align:left">Descrição</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>Exigido</em></td>
    <td style="text-align:center">Sequência de caracteres</td>
    <td>
      Um nome definido pelo usuário para o novo modelo customizado. Use um nome que descreve o domínio do modelo customizado, tal como <code>Medical custom model</code> ou <code>Legal custom model</code>. Use um nome que seja exclusivo entre todos os modelos de idioma customizados que você possui.
      Use um nome localizado que corresponda ao idioma do modelo customizado.
    </td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>Exigido</em></td>
    <td style="text-align:center">Sequência de caracteres</td>
    <td>
      O nome do modelo de idioma que deve ser customizado pelo novo modelo customizado. Especifique um dos modelos de idioma suportados que é retornado pelo método <code>GET /v1/models</code>. O novo modelo pode ser usado somente com o modelo base que ele customiza.
    </td>
  </tr>
  <tr>
    <td><code>dialect</code><br/><em>Opcional</em></td>
    <td style="text-align:center">Sequência de caracteres</td>
    <td>
      O dialeto do idioma especificado que deve ser usado com o novo modelo customizado. Para a maioria dos idiomas, o dialeto corresponde ao idioma do modelo base por padrão. Por exemplo, `en-US` é usado para qualquer um dos modelos de idioma inglês dos EUA.<br><br>
      Para um idioma espanhol, o serviço cria um modelo customizado que é adequado para a fala em um dos dialetos a seguir:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          `es-ES` para espanhol castelhano (modelos `es-ES`)
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          `es-LA` para espanhol latino-americano (modelos `es-AR`, `es-CL`, `es-CO` e `es-PE`)
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          `es-US` para espanhol mexicano (América do Norte) (modelos `es-MX`)
        </li>
      </ul>
      O parâmetro é significativo somente para modelos de espanhol, para os quais é sempre possível omitir com segurança o parâmetro para que o serviço crie o mapeamento correto.
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          Se você especificar o parâmetro `dialect` para modelos de idioma não espanhol, seu valor deverá corresponder ao idioma do modelo base.
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          Se você especificar o `dialect` para modelos de idioma espanhol, seu valor deverá corresponder a um dos mapeamentos definidos conforme indicado (`es-ES`, `es-LA` ou `es-MX`).
        </li>
      </ul>
      Todos os valores de dialeto não fazem distinção entre maiúsculas e minúsculas.
    </td>
  </tr>
  <tr>
    <td><code>Descrição</code><br/><em>Opcional</em></td>
    <td style="text-align:center">Sequência de caracteres</td>
    <td>
      Uma descrição do novo modelo. Use uma descrição localizada que corresponda ao idioma do modelo customizado.
    </td>
  </tr>
</table>

O exemplo a seguir cria um novo modelo de idioma customizado denominado `Example model`. O modelo é criado para o modelo base `en-US-BroadbandModel` e tem a descrição `Example custom language model`. O cabeçalho `Content-Type` especifica que os dados JSON estão sendo passados para o método.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Example model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom language model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations"
```
{: pre}

O exemplo retorna o ID de customização do novo modelo. Cada modelo customizado é identificado por um ID de customização exclusivo, que é um Identificador Exclusivo Global (GUID). Você especifica um GUID do modelo customizado com o parâmetro `customization_id` das chamadas que estão associadas ao modelo.

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

O novo modelo customizado é de propriedade da instância do serviço cujas credenciais são usadas para criá-lo. Para obter mais informações, consulte [Propriedade de modelos customizados](/docs/services/speech-to-text?topic=speech-to-text-customization#customOwner).

## Incluir um corpus no modelo de idioma customizado
{: #addCorpus}

Depois de criar o modelo de idioma customizado, a próxima etapa é incluir dados (palavras específicas do domínio) no modelo. O meio recomendado de preencher um modelo customizado com novas palavras é incluir um ou mais corpora.

Um corpus é um arquivo de texto simples que idealmente contém sentenças de amostra do seu domínio. O serviço analisa o conteúdo de um arquivo de corpus e extrai quaisquer palavras que não estejam em seu vocabulário base. Essas palavras são referidas como palavras fora do vocabulário (OOV).

-   Para obter mais informações sobre como usar os corpora, consulte [Trabalhando com os corpora](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#workingCorpora).
-   Para obter mais informações sobre como o serviço inclui os corpora em um modelo, consulte [O que acontece ao incluir um arquivo de corpus?](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#parseCorpus)

Ao fornecer sentenças que incluem novas palavras, os corpora permitem que o serviço aprenda as palavras no contexto. Em seguida, é possível aumentar ou modificar as palavras do modelo individualmente. O treinamento de um modelo apenas com palavras individuais em vez de palavras incluídas dos corpora é mais demorado e pode produzir resultados menos eficazes.
{: tip}

Use o método `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` para incluir um corpus em um modelo customizado:

-   Especifique o ID de customização do modelo customizado com o parâmetro de caminho `customization_id`.
-   Especifique um nome para o corpus com o parâmetro de caminho `corpus_name`. Use um nome localizado que corresponda ao idioma do modelo customizado e reflita os conteúdos do corpus.
    -   Inclua um máximo de 128 caracteres no nome.
    -   Não utilize caracteres que precisem ser codificados pela URL. Por exemplo, não use espaços, barras, barras invertidas, dois-pontos, e comercial, aspas duplas, sinais de mais, sinais de igual, pontos de interrogação e assim por diante no nome. (O serviço não impede o uso desses caracteres. Mas como eles devem ser codificados pela URL onde quer que sejam usados, seu uso não é recomendado.)
    -   Não use o nome de um corpus ou gramática que já tenha sido incluído no modelo customizado.
    -   Não use o nome `user`, que é reservado pelo serviço para indicar palavras customizadas que são incluídas ou modificadas pelo usuário.
    -   Não use o nome `base_lm` ou `default_lm`. Ambos os nomes são reservados para uso futuro pelo serviço.
-   Passe o arquivo de texto do corpus como o corpo da solicitação.

É possível incluir um máximo de 90 mil palavras OOV e 10 milhões de palavras totais de todas as fontes combinadas. Isso inclui palavras dos corpora e das gramáticas e palavras que você inclui diretamente. Para obter mais informações, consulte [Quanto de dados eu preciso?](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#wordsResourceAmount)
{: note}

O exemplo a seguir inclui o arquivo de texto do corpus `healthcare.txt` no modelo customizado com o ID especificado O exemplo nomeia o corpus `assistência médica`.

```bash
curl -X POST -u "apikey:{apikey}"
--data-binary @healthcare.txt
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/healthcare"
```
{: pre}

O método também aceita um parâmetro de consulta `allow_overwrite` opcional que sobrescreve um corpus existente para um modelo customizado. Use o parâmetro se você precisar atualizar um arquivo de corpus depois de incluí-lo em um modelo.

O método é assíncrono. Ele pode levar um minuto ou dois para ser concluído. A duração da operação depende do número total de palavras no corpus, do número de novas palavras que o serviço localiza no corpus e da carga atual no serviço. Para obter mais informações sobre como verificar o status de um corpus, consulte [Monitorando a solicitação de inclusão de corpus](#monitorCorpus).

É possível incluir qualquer número de corpora em um modelo customizado chamando o método uma vez para cada arquivo de texto do corpus. A inclusão de um corpus deve ser totalmente concluída antes de poder incluir outra. Depois de incluir um corpus em um modelo customizado, examine as novas palavras customizadas para verificar os erros tipográficos e outros. Para obter mais informações, consulte [Validando um recurso de palavras](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel).

### Monitorando a solicitação de inclusão de corpus
{: #monitorCorpus}

O serviço retornará um código de resposta 201 se o corpus for válido. Em seguida, ele processará de forma assíncrona os conteúdos do corpus e extrairá novas palavras automaticamente. Não é possível enviar solicitações para incluir dados no modelo customizado ou para treinar o modelo até que a análise do serviço do corpus para a solicitação atual seja concluída.

Para determinar o status da análise, use o método `GET /v1/customizations/{customization_id}/corpora/{corpus_name}` para pesquisar o status do corpus. O método aceita o ID do modelo e o nome do corpus, conforme mostrado no exemplo a seguir:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/corpus1"
```
{: pre}

A resposta inclui o status do corpus:

```javascript
{
  "name": "corpus1",
  "total_words": 5037,
  "out_of_vocabulary_words": 401,
  "status": "analyzed"
}
```
{: codeblock}

O campo `status` tem um dos valores a seguir:

-   `analyzed` indica que o serviço analisou com êxito o corpus.
-   `being_processed` indica que o serviço ainda está analisando o corpus.
-   `undetermined` indica que o serviço encontrou um erro ao processar o corpus.

Use um loop para verificar o status do corpus a cada 10 segundos até que se torne `analyzed`. Para obter mais informações sobre como verificar o status dos corpora de um modelo, consulte [Listando os corpora para um modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-manageCorpora#listCorpora).

## Incluir palavras no modelo de idioma customizado
{: #addWords}

Embora a inclusão dos corpora seja o meio recomendado de incluir palavras em um modelo de idioma customizado, também é possível incluir palavras customizadas individuais no modelo diretamente. O serviço inclui as palavras customizadas no modelo customizado assim como faz com as palavras OOV que descobre por meio dos corpora.

Se você tiver apenas uma ou algumas palavras para incluir em um modelo, usar corpora para incluir as palavras pode não ser prático ou mesmo viável. A abordagem mais simples é incluir uma palavra com apenas sua ortografia. Mas também é possível fornecer múltiplas pronúncias para a palavra e indicar como a palavra deve ser exibida.

-   Para obter mais informações sobre como incluir palavras diretamente, consulte [Trabalhando com palavras customizadas](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#workingWords).
-   Para obter mais informações sobre como o serviço inclui palavras customizadas em um modelo, consulte [O que acontece ao incluir ou modificar uma palavra customizada?](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#parseWord)

É possível usar os métodos a seguir para incluir palavras em um modelo customizado:

-   O método `POST /v1/customizations/{customization_id}/words` inclui múltiplas palavras de uma vez. Você passa um objeto JSON que fornece informações sobre cada palavra por meio do corpo da solicitação. O exemplo a seguir inclui duas palavras customizadas, `HHonors` e `IEEE`, no modelo customizado com o ID especificado. O cabeçalho `Content-Type` especifica que os dados JSON estão sendo passados para o método.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"words\": [
      {\"word\": \"HHonors\", \"sounds_like\": [\"hilton honors\", \"H. honors\"], \"display_as\": \"HHonors\"},
      {\"word\": \"IEEE\", \"sounds_like\": [\"I. triple E.\"]}]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    Também é possível incluir as palavras de um arquivo. Por exemplo, o arquivo `words.json` define as mesmas duas palavras customizadas.

    ```
    {"words":
      [
        {"word": "HHonors", "sounds_like": ["hilton honors", "H. honors"], "display_as": "HHonors"},
        {"word": "IEEE", "sounds_like": ["I. triple E."]}
      ]
    }
    ```
    {: codeblock}

    O comando a seguir inclui as palavras do arquivo:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data-binary @words.json
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words"
    ```
    {: pre}

    Esse método é assíncrono. O tempo que leva para ser concluído depende do número de palavras que você inclui e da carga atual no serviço. Para obter mais informações sobre como verificar o status da operação, consulte [Monitorando a solicitação de inclusão de palavras](#monitorWords).
-   O método `PUT /v1/customizations/{customization_id}/words/{word_name}` inclui palavras individuais. Você passa um objeto JSON que fornece informações sobre a palavra. O exemplo a seguir inclui a palavra `NCAA` para o modelo com o ID especificado. O cabeçalho `Content-Type` novamente indica que os dados JSON estão sendo passados para o método.

    ```bash
    curl -X PUT -u "apikey:{apikey}"
    --header "Content-Type: application/json"
    --data "{\"sounds_like\": [\"N. C. A. A.\", \"N. C. double A.\"]}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/words/NCAA"
    ```
    {: pre}

    Esse método é síncrono. O serviço retorna um código de resposta que relata o sucesso ou a falha da solicitação imediatamente.

Como ocorre com a inclusão de corpora, examine as novas palavras customizadas para verificar os erros tipográficos e outros. Essa verificação é especialmente importante quando você inclui múltiplas palavras de uma vez. Para obter mais informações, consulte [Validando um recurso de palavras](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel).

### Monitorando a solicitação de inclusão de palavras
{: #monitorWords}

Ao usar o método `POST /v1/customizations/{customization_id}/words`, o serviço retornará um código de resposta 201 se os dados de entrada forem válidos. Em seguida, processa de forma assíncrona as palavras para inclui-las no modelo. A operação é geralmente mais rápida do que incluir um corpus ou treinar um modelo, mas sua duração depende do número de novas palavras que você inclui e do carregamento atual no serviço. Não é possível enviar solicitações para incluir dados no modelo customizado ou para treinar o modelo até que o serviço conclua a solicitação para incluir novas palavras.

Para determinar o status da solicitação, use o método `GET /v1/customizations/{customization_id}` para pesquisar o status do modelo. O método aceita o ID de customização do modelo e retorna informações que incluem o status do modelo, como no exemplo a seguir:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T18:45:11.737Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

O campo `status` relata o estado atual do modelo. Enquanto o serviço está processando novas palavras, o status permanece `pending`. Use um loop para verificar o status a cada 10 segundos até que se torne `ready` para indicar que a operação está concluída. Para obter mais informações sobre os possíveis valores de `status`, consulte [Monitorando a solicitação de treinamento do modelo](#monitorTraining-language).

### Modificando palavras em um modelo customizado
{: #modifyWord}

Também é possível usar os métodos `POST /v1/customizations/{customization_id}/words` e `PUT /v1/customizations/{customization_id}/words/{word_name}` para modificar ou aprimorar uma palavra em um modelo customizado. Você pode precisar usar os métodos para corrigir um erro tipográfico ou outro erro que foi cometido ao incluir uma palavra. Também pode ser necessário incluir definições de pronúncia para uma palavra existente.

Você usa os métodos para modificar a definição de uma palavra existente exatamente como você faz para incluir uma palavra. Os novos dados que você fornece para a palavra sobrescrevem a definição existente da palavra.

## Treinar o modelo de idioma customizado
{: #trainModel-language}

Depois de preencher um modelo de idioma customizado com novas palavras (incluindo os corpora, incluindo as gramáticas ou incluindo as palavras diretamente), deve-se treinar o modelo nos novos dados. O treinamento prepara o modelo customizado para usar os dados no reconhecimento de voz. O modelo não usa palavras que você inclui via qualquer meio até que seja treinada nos dados.

Use o método `POST /v1/customizations/{customization_id}/train` para treinar um modelo customizado. Você passa ao método o ID de customização do modelo que você deseja treinar, como no exemplo a seguir:

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
```
{: pre}

O método é assíncrono. O treinamento pode levar alguns minutos para ser concluído, dependendo do número de novas palavras nas quais o modelo está sendo treinado e da carga atual no serviço. Para obter mais informações sobre como verificar o status de uma operação de treinamento, consulte [Monitorando a solicitação de treinamento do modelo](#monitorTraining-language).

O método inclui os seguintes parâmetros de consulta opcionais:

-   O parâmetro `word_type_to_add` especifica as palavras nas quais o modelo customizado deve ser treinado:
    -   Especifique `all` ou omita o parâmetro para treinar o modelo em todas as suas palavras, independentemente de sua origem.
    -   Especifique `user` para treinar o modelo apenas em palavras que foram incluídas ou modificadas pelo usuário, ignorando palavras que foram extraídas apenas dos corpora ou gramáticas.

    Essa opção será útil se você incluir corpora com dados com ruído, como palavras que contêm erros tipográficos. Antes de treinar o modelo em tais dados, é possível usar o parâmetro de consulta `word_type` do método `GET /v1/customizations/{customization_id}/words` para revisar as palavras que são extraídas dos corpora e gramáticas. Para obter mais informações, consulte [Listando palavras de um modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-manageWords#listWords).
-   O parâmetro `customization_weight` especifica o peso relativo que é dado às palavras do modelo customizado, em vez de palavras do vocabulário base quando o modelo customizado é usado para reconhecimento de voz. Também é possível especificar um peso de customização com qualquer solicitação de reconhecimento que use o modelo customizado. Para obter mais informações, consulte [Usando o peso de customização](/docs/services/speech-to-text?topic=speech-to-text-languageUse#weight).
-   O parâmetro `strict` indica se o treinamento deverá continuar se o modelo customizado contiver uma combinação de recursos válidos e inválidos (corpora, gramáticas e palavras). Por padrão, o treinamento falhará se o modelo contiver um ou mais recursos inválidos. Configure o parâmetro como `false` para permitir que o treinamento continue, desde que o modelo contenha pelo menos um recurso válido. O serviço exclui recursos inválidos do treinamento. Para obter mais informações, consulte [Falhas de treinamento](#failedTraining-language).

### Monitorando a solicitação de treinamento do modelo
{: #monitorTraining-language}

O serviço retornará um código de resposta 200 se iniciar o processo de treinamento com êxito. O serviço não pode aceitar solicitações de treinamento subsequentes, ou solicitações para incluir novos corpora, gramáticas ou palavras, até que a solicitação existente seja concluída.

Para determinar o status de uma solicitação de treinamento, use o método `GET /v1/customizations/{customization_id}` para pesquisar o status do modelo. O método aceita o ID de customização do modelo e retorna informações sobre o modelo.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T18:45:11.737Z",
  "language": "en-US",
  "dialect": "en-US",
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

A resposta inclui os campos `status` e `progress` que relatam o estado do modelo customizado. O significado do campo `progress` depende do status do modelo. O campo `status` pode ter um dos valores a seguir:

-   `pending` indica que o modelo foi criado, mas está aguardando que os dados de treinamento válidos sejam incluídos ou que o serviço conclua a análise dos dados que foram incluídos. O campo `progress` é `0`.
-   `ready` indica que o modelo contém dados válidos e está pronto para ser treinado. O campo `progress` é `0`.

    Se o modelo contiver uma combinação de recursos válidos e inválidos (por exemplo, palavras customizadas válidas e inválidas), o treinamento do modelo falhará, a menos que você configure o parâmetro de consulta `strict` como `false`. Para obter mais informações, consulte [Falhas de treinamento](#failedTraining-language).
-   `training` indica que o modelo está sendo treinado. O campo `progress` muda de `0` para `100` quando o treinamento é concluído. <!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available` indica que o modelo está treinado e pronto para uso. O campo `progress` é `100`.
-   `upgrading` indica que o modelo está passando por upgrade. O campo `progress` é `0`.
-   `failed` indica que o treinamento do modelo falhou. O campo `progress` é `0`. Para obter mais informações, consulte [Falhas de treinamento](#failedTraining-language).

Use um loop para verificar o status a cada 10 segundos até que se torne `available`. Para obter mais informações sobre como verificar o status de um modelo customizado, consulte [Listando modelos de idioma customizados](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).

### Falhas de treinamento
{: #failedTraining-language}

O treinamento falha ao iniciar se o serviço estiver manipulando outra solicitação para o modelo de idioma customizado. Por exemplo, uma solicitação de treinamento falhará ao iniciar com um código de status de 409 se o serviço estiver

-   Processando um corpus ou gramática para gerar uma lista de palavras OOV
-   Processando palavras customizadas para validar ou gerar automaticamente as pronúncias
-   Manipulando outra solicitação de treinamento

O treinamento também falhará ao iniciar com um código de status de 400 se o modelo customizado

-   Não contiver novos dados de treinamento válidos (corpora, gramáticas ou palavras) desde que foi criado ou treinado pela última vez
-   Contiver um ou mais corpora, gramáticas ou palavras inválidas (por exemplo, uma palavra customizada tem uma pronúncia inválida)

Se a solicitação de treinamento falhar com um código de status de 400, o serviço configurará o status do modelo customizado como `failed`. Execute uma das ações a seguir:

-   Use os métodos da interface de customização para examinar os recursos do modelo e corrigir qualquer erro que você localizar:
    -   Para um corpus inválido, é possível corrigir o arquivo de texto do corpus e usar o parâmetro `allow_overwrite` do método `POST /v1/customizations/{customization_id}/corpora/{corpus_name}` para incluir o arquivo corrigido no modelo. Para obter mais informações, consulte [Incluir um corpus no modelo de idioma customizado](#addCorpus).
    -   Para uma gramática inválida, é possível corrigir o arquivo de gramática e usar o parâmetro `allow_overwrite` do método `POST /v1/customizations/{customization_id}/grammars/{grammar_name}` para incluir o arquivo corrigido no modelo. Para obter mais informações, consulte [Incluir uma gramática no modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-grammarAdd#addGrammar).
    -   Para uma palavra customizada inválida, é possível usar o método `POST /v1/customizations/{customization_id}/words` ou `PUT /v1/customizations/{customization_id}/words/{word_name}` para modificar a palavra diretamente no recurso de palavras do modelo. Para obter mais informações, consulte [Modificando palavras em um modelo customizado](#modifyWord).

    Para obter mais informações sobre como validar as palavras em um modelo de idioma customizado, consulte [Validando um recurso de palavras](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel).
-   Configure o parâmetro `strict` do método `POST /v1/customizations/{customization_id}/train` como `false` para excluir recursos inválidos do treinamento. O modelo deve conter pelo menos um recurso válido (corpus, gramática ou palavra) para que o treinamento seja bem-sucedido. O parâmetro `strict` é útil para treinamento de um modelo customizado que contém uma combinação de recursos válidos e inválidos.

## Exemplo de scripts
{: #exampleScripts}

É possível usar os scripts a seguir para fazer experimentos com as etapas para criar um modelo de idioma customizado:

-   Um script Python denominado <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a>. Para obter mais informações, consulte [Script Python de exemplo](#pythonScript).
-   Um Bash shell script denominado <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a>. Para obter mais informações, consulte [Shell script de exemplo](#shellScript).

Os dois scripts fornecem funcionalidade idêntica. Cada script cria um modelo customizado, inclui palavras de um arquivo de texto do corpus e inclui palavras únicas e múltiplas diretamente no modelo. O script consulta o modelo para listar palavras incluídas de um corpus e diretamente pelo usuário. Ele também treina o modelo e demonstra a pesquisa que é recomendada para monitorar os resultados de operações assíncronas.

É possível usar um dos dois arquivos de texto do corpus fornecidos com os scripts ou é possível testar com seus próprios arquivos de corpus:

-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/corpus.txt" download="corpus.txt">corpus.txt <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a> é um corpus de 6 KB abreviado que inclui seis termos relacionados à assistência médica em um modelo. Esse arquivo produz uma pequena quantia de saída ao ser usado com os scripts. Os scripts usam esse arquivo por padrão.
-   <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/healthcare.txt" download="healthcare.txt">healthcare.txt <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a> é um corpus de 164 KB mais rico que inclui muitos termos relacionados à assistência médica em um modelo. Esse arquivo produz muito mais saída ao ser usado com os scripts.

Por padrão, o novo modelo customizado que você cria com qualquer um dos scripts está disponível para uso com as solicitações de reconhecimento. Os scripts incluem uma etapa opcional para excluir o novo modelo customizado, que pode ser útil quando você está experimentando com o processo. Siga os comentários nos scripts para ativar a etapa de exclusão.

### Exemplo de script Python
{: #pythonScript}

Siga estas etapas para usar o script Python:

1.  Faça download do script Python denominado <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.py" download="testSTTcustom.py">testSTTcustom.py <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a>.
1.  Faça download dos arquivos de texto do corpus de exemplo para usar com o script. Você é livre para testar com um dos arquivos de texto do corpus ou com um arquivo de sua própria escolha. Por padrão, todos os arquivos de texto do corpus devem residir no mesmo diretório que o script.
1.  O script usa a biblioteca `requests` do Python para solicitações de HTTP para o serviço. Use `pip` ou `easy_install` para instalar a biblioteca para uso pelo script, por exemplo:

    ```bash
    pip install requests
    ```
    {: pre}

    Para obter mais informações sobre a biblioteca, consulte [pypi.python.org/pypi/requests](https://pypi.python.org/pypi/requests){: external}.
1.  Edite o script para substituir a sequência `password` `iam_apikey` pela chave de API de suas credenciais do {{site.data.keyword.speechtotextshort}}:

    ```
    password = "iam_apikey"
    ```
    {: codeblock}

1.  Execute o script inserindo o comando a seguir:

    ```bash
    python testSTTcustom.py
    ```
    {: pre}

O script usa a URL padrão a seguir para a localização Dallas: `https://stream.watsonplatform.net`. Se você criou sua instância de serviço em uma localização diferente, modifique as variáveis `uri` para usar sua localização. Por exemplo, use `https://gateway-wdc.watsonplatform.net` se sua instância de serviço residir na localização Washington, DC.
{: note}

### Shell script de exemplo
{: #shellScript}

Siga estas etapas para usar o Bash shell script:

1.  Faça download do shell script denominado <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/testSTTcustom.sh" download="testSTTcustom.sh">testSTTcustom.sh <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a>.
1.  Faça download dos arquivos de texto do corpus de exemplo para usar com o script. Você é livre para testar com um dos arquivos de texto do corpus ou com um arquivo de sua própria escolha. Por padrão, todos os arquivos de texto do corpus devem residir no mesmo diretório que o script.
1.  O script usa o comando `curl` para solicitações de HTTP para o serviço. Se você ainda não tiver transferido por download o `curl`, será possível instalar a versão para seu sistema operacional por meio de [curl.haxx.se](http://curl.haxx.se){: external}. Instale a versão que suporte o protocolo Secure Sockets Layer (SSL) e certifique-se de incluir o arquivo binário instalado em sua variável de ambiente `PATH`.
1.  Edite o script para substituir a sequência `PASSWORD` `iam_apikey` pela chave de API de suas credenciais do {{site.data.keyword.speechtotextshort}}:

    ```
    PASSWORD="iam_apikey"
    ```
    {: codeblock}

1.  Edite o script para substituir a sequência `URL` pela URL para a localização na qual você criou sua instância de serviço. O script usa a URL padrão a seguir para a localização Dallas:

    ```
    URL="https://stream.watsonplatform.net/speech-to-text/api/v1"
    ```
    {: codeblock}

1.  Certifique-se de que o script tenha permissões executáveis e, em seguida, execute o script inserindo o comando a seguir:

    ```bash
    ./testSTTcustom.sh
    ```
    {: pre}
