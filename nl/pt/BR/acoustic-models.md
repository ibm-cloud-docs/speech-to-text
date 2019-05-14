---

copyright:
  years: 2019
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

# Gerenciando modelos acústicos customizados
{: #manageAcousticModels}

A interface de customização inclui o método `POST /v1/acoustic_customizations` para criar um modelo acústico customizado. A interface também inclui o método `POST /v1/acoustic_customizations/train` para treinar um modelo customizado em seus recursos de áudio mais recentes. Para obter mais informações, veja a documentação a seguir:
{: shortdesc}

-   [Criar um modelo acústico customizado](/docs/services/speech-to-text/acoustic-create.html#createModel-acoustic)
-   [Treinar o modelo acústico customizado](/docs/services/speech-to-text/acoustic-create.html#trainModel-acoustic)

Além disso, a interface inclui métodos para listar informações sobre os modelos acústicos customizados, a reconfiguração de um modelo customizado para seu estado inicial e a exclusão de um modelo customizado.

## Listando modelos acústicos customizados
{: #listModels-acoustic}

A interface de customização fornece dois métodos para listar as informações sobre os modelos acústicos customizados que são propriedade das credenciais de serviço especificadas:

-   O método `GET /v1/acoustic_customizations` lista as informações sobre todos os modelos acústicos customizados ou sobre todos os modelos acústicos customizados para um idioma especificado.
-   O método `GET /v1/acoustic_customizations/{customization_id}` lista as informações sobre um modelo acústico customizado especificado. Use esse método para pesquisar o serviço sobre o status de uma solicitação de treinamento.

Os dois métodos retornam as informações a seguir sobre um modelo acústico customizado:

-   `customization_id` identifica o Identificador Exclusivo Global (GUID) do modelo customizado. O GUID é usado para identificar o modelo em métodos da interface.
-   `created` é a data e hora na Hora Universal Coordenada (UTC) na qual o modelo customizado foi criado.
-   `language` é o idioma do modelo customizado.
-   `owner` identifica as credenciais da instância de serviço que tem o modelo customizado.
-   `name` é o nome do modelo customizado.
-   `description` mostrará a descrição do modelo customizado, se uma foi fornecida em sua criação.
-   `base_model_name` indica o nome do modelo de idioma para o qual o modelo customizado foi criado.
-   `versions` fornece uma lista das versões disponíveis do modelo customizado. Cada elemento da matriz indica uma versão do modelo base com a qual o modelo customizado pode ser usado. Múltiplas versões existirão somente se o modelo customizado passar por upgrade. Caso contrário, somente uma única versão será mostrada. Para obter mais informações, consulte [Listando informações de versão para um modelo customizado](/docs/services/speech-to-text/custom-upgrade.html#upgradeList).

Os métodos também retornam um campo `status` que indica o estado do modelo customizado:

-   `pending` indica que o modelo foi criado. Ele está aguardando que os dados de treinamento sejam incluídos ou que o serviço conclua a análise dos dados que foram incluídos.
-   `ready` indica que o modelo contém dados de áudio e está pronto para ser treinado.
-   `training` indica que o modelo está sendo treinado nos dados de áudio.
-   `available` indica que o modelo está treinado e pronto para uso com as solicitações de reconhecimento.
-   `upgrading` indica que o modelo está passando por upgrade.
-   `failed` indica que o treinamento do modelo falhou. Examine os recursos de áudio do modelo para determinar o que impediu o modelo de ser treinado. Erros possíveis incluem áudio insuficiente, áudio em excesso ou um recurso de áudio inválido.

Além disso, a saída inclui um campo `progress` que indica o progresso atual do treinamento do modelo customizado. Se você usou o método `POST /v1/acoustic_customizations/{customization_id}/train` para iniciar o treinamento do modelo, esse campo indicará o progresso atual dessa solicitação como uma porcentagem concluída. Nesse momento, o valor do campo será `100` se o status for `available`. Caso contrário, será `0`.

### Solicitações e respostas de exemplo
{: #listExample-acoustic}

O exemplo a seguir inclui o parâmetro de consulta `language` para listar todos os modelos acústicos customizados do inglês dos EUA que são propriedade das credenciais de serviço:

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations?language=en-US"
```
{: pre}

Dois desses modelos são propriedade das credenciais de serviço. O primeiro modelo está aguardando dados ou está sendo processado pelo serviço. O segundo modelo está totalmente treinado e pronto para uso.

```javascript
{
  "customizations": [
    {
      "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
      "created": "2016-06-01T18:42:25.324Z",
      "language": "en-US",
      "versions": [
        "en-US_BroadbandModel.v07-06082016.06202016",
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model one",
      "description": "Example custom acoustic model",
      "base_model_name": "en-US_BroadbandModel",
      "status": "pending",
      "progress": 0
    },
    {
      "customization_id": "8391f918-3b76-e109-763c-b7732fae4829",
      "created": "2016-06-01T18:51:37.291Z",
      "language": "en-US",
      "versions": [
        "en-US_BroadbandModel.v2017-11-15"
      ],
      "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
      "name": "Example model two",
      "description": "Example custom acoustic model two",
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
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
  "language": "en-US",
  "versions": [
    "en-US_BroadbandModel.v07-06082016.06202016",
    "en-US_BroadbandModel.v2017-11-15"
  ],
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model one",
  "description": "Example custom acoustic model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "pending",
  "progress": 0
}
```
{: codeblock}

## Reconfigurando um modelo acústico customizado
{: #resetModel-acoustic}

Use o método `POST /v1/acoustic_customizations/{customization_id}/reset` para reconfigurar um modelo acústico customizado. A reconfiguração de um modelo customizado remove todos os recursos de áudio do modelo, inicializando o modelo para seu estado na criação. O método não exclui o próprio modelo ou os metadados, como seu nome e idioma. No entanto, quando você reconfigura um modelo, seus recursos de áudio são removidos e devem ser recriados.

### Solicitação de exemplo
{: #resetExample-acoustic}

O exemplo a seguir reconfigura o modelo acústico customizado com o ID de customização especificado:

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/reset"
```
{: pre}

## Excluindo um modelo acústico customizado
{: #deleteModel-acoustic}

Use o método `DELETE /v1/acoustic_customizations/{customization_id}` para excluir um modelo acústico customizado que você não precisa mais. Ele exclui todo o áudio que está associado ao modelo customizado e ao próprio modelo. Use esse método com cuidado: um modelo customizado e seus dados não podem ser recuperados depois da exclusão do modelo.

### Solicitação de exemplo
{: #deleteExample-acoustic}

O exemplo a seguir exclui o modelo acústico customizado com o ID de customização especificado:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}"
```
{: pre}
