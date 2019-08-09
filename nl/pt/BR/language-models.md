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

# Gerenciando modelos de idioma customizados
{: #manageLanguageModels}

A interface de customização inclui o método `POST /v1/customizations` para criar um modelo de idioma customizado. A interface também inclui o método `POST /v1/customizations/train` para treinar um modelo customizado nos dados mais recentes de seu recurso de palavras. Para
obter mais informações, consulte
{: shortdesc}

-   [Criar um modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language)
-   [Treinar o modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#trainModel-language)

Além disso, a interface inclui métodos para listar informações sobre os modelos de idioma customizados, reconfigurando um modelo customizado para seu estado inicial, fazendo upgrade de um modelo customizado e excluindo um modelo customizado. Não é possível treinar, reconfigurar, fazer upgrade ou excluir um modelo customizado enquanto o serviço está manipulando outra operação nesse modelo, inclusive a inclusão de recursos no modelo.

## Listando modelos de idioma customizados
{: #listModels-language}

A interface de customização fornece dois métodos para listar informações sobre os modelos de idioma customizados pertencentes às credenciais especificadas:

-   O método `GET /v1/customizations` lista informações sobre todos os modelos de idioma customizados ou sobre todos os modelos de idioma customizados para um idioma especificado.
-   O método `GET /v1/customizations/{customization_id}` lista informações sobre um modelo de idioma customizado especificado. Use esse método para pesquisar o serviço sobre o status de uma solicitação de treinamento ou uma solicitação para incluir novas palavras.

Os dois métodos retornam as informações a seguir sobre um modelo customizado:

-   `customization_id` identifica o Identificador Exclusivo Global (GUID) do modelo customizado. O GUID é usado para identificar o modelo em métodos da interface.
-   `created` é a data e hora na Hora Universal Coordenada (UTC) na qual o modelo customizado foi criado.
-   `updated` é a data e a hora na Hora Universal Coordenada (UTC) em que o modelo customizado foi modificado pela última vez.
-   `language` é o idioma do modelo customizado.
-   `dialect` é o dialeto do idioma para o modelo customizado, que não corresponde necessariamente ao idioma do modelo customizado para modelos de espanhol. Para obter mais informações, consulte a descrição do parâmetro `dialect` em [Criar um modelo de idioma customizado](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#createModel-language).
-   `owner` identifica as credenciais da instância de serviço que tem o modelo customizado.
-   `name` é o nome do modelo customizado.
-   `description` mostrará a descrição do modelo customizado, se uma foi fornecida em sua criação.
-   `base_model` indica o nome do modelo de idioma para o qual o modelo customizado foi criado.
-   `versions` fornece uma lista das versões disponíveis do modelo customizado. Cada elemento da matriz indica uma versão do modelo base com a qual o modelo customizado pode ser usado. Múltiplas versões existirão somente se o modelo customizado passar por upgrade. Caso contrário, somente uma única versão será mostrada. Para obter mais informações, consulte [Listando informações de versão para um modelo customizado](/docs/services/speech-to-text?topic=speech-to-text-customUpgrade#upgradeList).

O método também retorna um campo `status` que indica o estado do modelo customizado:

-   `pending` indica que o modelo foi criado. Ele está aguardando que os dados de treinamento válidos (corpora, gramáticas ou palavras) sejam incluídos ou que o serviço conclua a análise dos dados que foram incluídos.
-   `ready` indica que o modelo contém dados válidos e está pronto para ser treinado. Se o modelo contiver uma combinação de recursos válidos e inválidos, o treinamento do modelo falhará, a menos que você configure o parâmetro de consulta `strict` como `false`. Para obter mais informações, consulte [Falhas de treinamento](/docs/services/speech-to-text?topic=speech-to-text-languageCreate#failedTraining-language).
-   `training` indica que o modelo está sendo treinado nos dados.
-   `available` indica que o modelo está treinado e pronto para uso com uma solicitação de reconhecimento.
-   `upgrading` indica que o modelo está passando por upgrade.
-   `failed` indica que o treinamento do modelo falhou. Examine as palavras no recurso de palavras do modelo para determinar os erros que impediram o treinamento do modelo.

Além disso, a saída inclui um campo `progress` que indica o progresso atual do treinamento do modelo customizado. Se você usou o método `POST /v1/customizations/{customization_id}/train` para iniciar o treinamento do modelo, esse campo indicará o progresso atual dessa solicitação como uma porcentagem concluída. Nesse momento, o valor do campo será `100` se o status for `available`. Caso contrário, será `0`.

### Solicitações e respostas de exemplo
{: #listExample-language}

O exemplo a seguir inclui o parâmetro de consulta `language` para listar todos os modelos de idioma customizado em inglês dos EUA pertencentes às credenciais especificadas:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations?language=en-US"
```
{: pre}

As credenciais têm dois desses modelos. O primeiro modelo está aguardando dados ou está sendo processado pelo serviço. O segundo modelo está totalmente treinado e pronto para uso.

```javascript
{
  "customizations": [
    {
      "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
      "created": "2016-06-01T18:42:25.324Z",
      "updated": "2016-06-01T18:42:25.324Z",
      "language": "en-US",
      "dialect": "en-US",
      "versions": [
        "en-US_BroadbandModel.v07-06082016.06202016",
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model",
      "description": "Example custom language model",
      "base_model_name": "en-US_BroadbandModel",
      "status": "pending",
      "progress": 0
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732fae4829",
      "created": "2016-06-01T18:51:37.291Z",
      "updated": "2016-06-01T20:02:10.624Z",
      "language": "en-US",
      "dialect": "en-US",
      "versions": [
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model two",
      "description": "Example custom language model two",
      "base_model_name": "en-US_BroadbandModel",
      "status": "available",
      "progress": 100
    }
  ]
}
```
{: codeblock}

O exemplo a seguir retorna as informações sobre o modelo customizado que tem o ID de customização especificado:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "updated": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "dialect": "en-US",
  "versions": [
    "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom language model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

## Reconfigurando um modelo de idioma customizado
{: #resetModel-language}

Use o método `POST /v1/customizations/{customization_id}/reset` para reconfigurar um modelo customizado. A reconfiguração de um modelo remove todos os corpora e as palavras do modelo, inicializando o modelo para seu estado na criação. O método não exclui o próprio modelo ou os metadados, como seu nome e idioma. No entanto, quando você reconfigura um modelo, seu recurso de palavras está vazio e deve ser recriado por meio da inclusão de corpora e palavras.

### Solicitação de exemplo
{: #resetExample-language}

O exemplo a seguir reconfigura o modelo customizado com o ID de customização especificado:

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/reset"
```
{: pre}

## Excluindo um modelo de idioma customizado
{: #deleteModel-language}

Use o método `DELETE /v1/customizations/{customization_id}` para excluir um modelo de idioma customizado que você não precisa mais. Ele exclui todo o corpora que está associado ao modelo customizado e ao próprio modelo. Use esse método com cuidado: um modelo customizado e seus dados não podem ser recuperados depois da exclusão do modelo.

### Solicitação de exemplo
{: #deleteExample-language}

O exemplo a seguir exclui o modelo customizado com o ID de customização especificado:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"
```
{: pre}
