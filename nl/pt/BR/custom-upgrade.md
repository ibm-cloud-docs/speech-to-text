---

copyright:
  years: 2017, 2019
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

# Fazendo upgrade de modelos customizados
{: #customUpgrade}

Para melhorar a qualidade do reconhecimento de voz, o serviço {{site.data.keyword.speechtotextfull}} atualiza os modelos base ocasionalmente. Como os modelos base para diferentes idiomas são independentes uns dos outros, assim como os modelos de banda larga e de banda estreita para um idioma, as atualizações para modelos base individuais não afetam outros modelos. As [Notas sobre a liberação](/docs/services/speech-to-text/release-notes.html) documentam todas as atualizações do modelo base.
{: shortdesc}

Quando uma nova versão de um modelo base é liberada, deve-se fazer upgrade de qualquer modelo acústico e de idioma customizado construído no modelo base para aproveitar as atualizações. Seus modelos customizados continuam a usar a versão mais antiga do modelo base até que você conclua o upgrade. Como acontece com todas as operações de customização, deve-se usar credenciais para a instância do serviço que tem um modelo para passá-lo por upgrade.

Faça upgrade para a versão mais recente de um modelo base atualizado assim que possível. Quanto mais cedo você fizer upgrade de um modelo customizado, mais cedo poderá experimentar o desempenho aprimorado do novo modelo. Além disso, versões mais antigas de modelos base podem ser removidas futuramente. Para encorajar o upgrade, o serviço retorna uma mensagem de aviso com os resultados para solicitações de reconhecimento que usam modelos customizados baseados em modelos base mais antigos.

## Como o upgrade funciona
{: #upgradeOverview}

Quando um novo modelo base é lançado pela primeira vez, os modelos customizados existentes ainda são baseados na versão mais antiga do modelo base. Até que um modelo customizado passe por upgrade, todas as operações nesse modelo customizado, como incluir dados no modelo ou treinar o modelo, afetarão a versão existente do modelo. Da mesma forma, todas as solicitações de reconhecimento que especificam o modelo customizado usam a versão existente do modelo.

O upgrade resulta em duas versões de um modelo customizado, uma com base na versão mais antiga do modelo base e uma com base na versão mais recente do modelo base. Depois que um modelo customizado passa por upgrade, todas as operações nesse modelo customizado afetam a versão mais recente que passou por upgrade do modelo. Não é mais possível incluir dados em ou treinar a versão mais antiga do modelo. Além disso, não é possível desfazer uma operação de upgrade.

Quando você faz upgrade de qualquer tipo de modelo customizado, não é necessário fazer upgrade de seus recursos individuais. Para um modelo de idioma customizado, o serviço executa automaticamente o upgrade de qualquer corpora, gramáticas e palavras que são definidas para o modelo. Da mesma forma, quando você faz upgrade de um modelo acústico customizado, o serviço faz upgrade automaticamente de seus recursos de áudio.

Por padrão, o serviço usa a versão mais recente do modelo customizado para solicitações de reconhecimento. No entanto, é possível continuar a usar a versão mais antiga de um modelo customizado para reconhecimento de voz. Para obter mais informações, consulte [Fazendo solicitações de reconhecimento com modelos customizados com upgrade](#upgradeRecognition).

## Fazendo upgrade de um modelo de idioma customizado
{: #upgradeLanguage}

Siga estas etapas para fazer upgrade de um modelo de idioma customizado:

1.  Assegure-se de que o modelo de idioma customizado esteja no estado `ready` ou `available`. É possível verificar o status de um modelo usando o método `GET /v1/customizations/{customization_id}`. Se o estado do modelo for `ready`, faça upgrade do modelo antes de treiná-lo em seus dados mais recentes.

1.  Faça upgrade do modelo de idioma customizado usando o método `POST /v1/customizations/{customization_id}/upgrade_model`:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    O método de upgrade é assíncrono. Pode levar alguns minutos para ser concluído, dependendo do número de palavras no recurso de palavras do modelo e do carregamento atual no serviço.

O serviço retornará um código de resposta 200 se o processo de upgrade for iniciado com êxito. É possível monitorar o status do upgrade usando o método `GET /v1/customizations/{customization_id}` para pesquisar o status do modelo. Use um loop para verificar o status a cada 10 segundos.

Durante o upgrade, o modelo customizado tem o status `upgrading`. Quando o upgrade for concluído, o modelo retomará o status que tinha antes do upgrade (`ready` ou `available`). Verificar o status de uma operação de upgrade é idêntico a verificar o status de uma operação de treinamento. Para obter mais informações, consulte [Monitorando a solicitação de treinamento do modelo](/docs/services/speech-to-text/language-create.html#monitorTraining-language).

O serviço não pode aceitar solicitações para modificar o modelo de alguma forma até que a solicitação de upgrade seja concluída. No entanto, é possível continuar a emitir solicitações de reconhecimento com a versão existente do modelo durante o upgrade.

## Fazendo upgrade de um modelo acústico customizado
{: #upgradeAcoustic}

Siga estas etapas para fazer upgrade de um modelo acústico customizado. Se o modelo acústico customizado tiver sido treinado com um modelo de idioma customizado, você deverá executar duas etapas de upgrade extras onde indicado.

1.  *Se o modelo acústico customizado for treinado com um modelo de idioma customizado,* você deverá primeiro fazer upgrade do modelo de idioma customizado para a versão mais recente do modelo base. Para obter mais informações, consulte [Fazendo upgrade de um modelo de idioma customizado](#upgradeLanguage).

1.  Assegure-se de que o modelo acústico customizado esteja no estado `ready` ou no estado `available`. É possível verificar o status de um modelo usando o método `GET /v1/acoustic_customizations/{customization_id}`. Se o estado do modelo for `ready`, faça upgrade do modelo antes de treiná-lo em seus dados mais recentes.

1.  Faça upgrade do modelo acústico customizado usando o método `POST /v1/acoustic_customizations/{customization_id}/upgrade_model`:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model"
    ```
    {: pre}

    O método de upgrade é assíncrono. Pode levar alguns minutos ou horas para ser concluído, dependendo da quantia de dados de áudio que o modelo contém e do carregamento atual no serviço. Como ocorre com o treinamento, o upgrade geralmente leva aproximadamente duas vezes o comprimento dos dados de áudio do modelo.

1.  *Se o modelo acústico customizado tiver sido treinado com um modelo de idioma customizado,* faça upgrade do modelo acústico customizado novamente, desta vez com o modelo de idioma customizado anteriormente atualizado. Use o parâmetro de consulta `custom_language_model_id` para especificar o ID de customização do modelo de idioma customizado.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/upgrade_model?custom_language_model_id={custom_language_model_id}"
    ```
    {: pre}

    Mais uma vez, o método de upgrade é assíncrono e o upgrade geralmente leva aproximadamente o dobro do comprimento dos dados de áudio do modelo.

    A solicitação para fazer upgrade do modelo acústico com o modelo de idioma pode falhar com um código de resposta 400 e a mensagem `No input data modified since last training`. Se esse erro ocorrer, inclua o parâmetro de consulta `force` booleano na solicitação e configure o parâmetro como `true`. Use o parâmetro apenas para forçar um upgrade de um modelo acústico customizado nessa situação específica.
    {: note}

O serviço retornará um código de resposta 200 se o processo de upgrade for iniciado com êxito. É possível monitorar o status do upgrade, usando o método `GET /v1/acoustic_customizations/{customization_id}` para pesquisar o status do modelo. Use um loop para verificar o status uma vez por minuto.

Durante o upgrade, o modelo customizado tem o status `upgrading`. Quando o upgrade for concluído, o modelo retomará o status que tinha antes do upgrade (`ready` ou `available`). Verificar o status de uma operação de upgrade é idêntico a verificar o status de uma operação de treinamento. Para obter mais informações, consulte [Monitorando a solicitação de treinamento do modelo](/docs/services/speech-to-text/acoustic-create.html#monitorTraining-acoustic).

O serviço não pode aceitar solicitações para modificar o modelo de alguma forma até que a solicitação de upgrade seja concluída. No entanto, é possível continuar a emitir solicitações de reconhecimento com a versão existente do modelo durante o upgrade.

## Falhas de upgrade
{: #upgradeFailures}

O upgrade de um modelo customizado falhará ao iniciar se o serviço estiver manipulando outra solicitação para o modelo, como uma solicitação de treinamento ou uma solicitação para incluir dados. A solicitação de upgrade também falha ao iniciar nos casos a seguir:

-   O modelo customizado está em um estado diferente de `ready` ou `available`.
-   O modelo customizado não contém dados (palavras customizadas ou recursos de áudio).
-   Para um modelo acústico customizado que foi treinado com um modelo de idioma customizado, os modelos customizados são baseados em versões diferentes do modelo base. Deve-se fazer upgrade do modelo de idioma customizado antes de fazer upgrade do modelo acústico customizado.

## Listando informações da versão para um modelo customizado
{: #upgradeList}

Para ver as versões do modelo base para as quais um modelo customizado está disponível, use os métodos a seguir:

-   Para listar informações sobre um modelo de idioma customizado, use o método `GET /v1/customizations/{customization_id}`. Para obter mais informações, consulte [Listando modelos de idioma customizados](/docs/services/speech-to-text/language-models.html#listModels-language).
-   Para listar informações sobre um modelo acústico customizado, use o método `GET /v1/acoustic_customizations/{customization_id}`. Para obter mais informações, consulte [Listando modelos acústicos customizados](/docs/services/speech-to-text/acoustic-models.html#listModels-acoustic).

Em ambos os casos, a saída inclui um campo `versions` que mostra informações sobre os modelos base para o modelo customizado. A saída a seguir mostra informações para um modelo de idioma customizado que passou por upgrade:

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96",
  "created": "2016-06-01T18:42:25.324Z",
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

O campo `versions` indica que o modelo customizado está disponível para duas versões do modelo base: a versão mais antiga, `en-US_BroadbandModel.v07-06082016.06202016` e a versão mais recente, `en-US_BroadbandModel.v2017-11-15`. Se o modelo customizado não tiver upgrade efetuado ou apenas uma única versão de seu modelo base existir, o campo `versions` mostrará uma versão única.

## Fazendo solicitações de reconhecimento com modelos customizados que passaram por upgrade
{: #upgradeRecognition}

Por padrão, o serviço usa a versão mais recente de um modelo customizado que é especificado com uma solicitação de reconhecimento. No entanto, mesmo depois que o upgrade de um modelo customizado é efetuado, é possível continuar a fazer solicitações de reconhecimento com a versão mais antiga do modelo. Use o parâmetro `base_model_version` de um método de reconhecimento para especificar a versão de um modelo base que deve ser usado para reconhecimento de voz.

Por exemplo, a solicitação de HTTP a seguir especifica que a versão mais antiga do modelo base deve ser usada. Portanto, a versão mais antiga do modelo de idioma customizado especificado também é usada.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?model=en-US_BroadbandModel&base_model_version=en-US_BroadbandModel.v07-06082016.06202016&language_customization_id={customization_id}"
```
{: pre}

É possível usar esse recurso para testar o desempenho e a precisão de um modelo customizado com relação às versões antiga e nova de seu modelo base. Se você descobrir que o desempenho de um modelo que passou por upgrade está de alguma maneira insuficiente (por exemplo, determinadas palavras não são mais reconhecidas), será possível continuar a usar a versão mais antiga com solicitações de reconhecimento.

[Versão do modelo base](/docs/services/speech-to-text/input.html#version) descreve o parâmetro `base_model_version` e como o serviço determina quais versões da base e modelos customizados usar com uma solicitação de reconhecimento. Além dessas informações, considere os problemas a seguir quando você passar os modelos acústico e de idioma customizados com uma solicitação de reconhecimento:

-   Os dois modelos customizados devem ser baseados no mesmo modelo base (por exemplo, `en-US_BroadbandModel`).
-   Se ambos os modelos customizados forem baseados no modelo base mais antigo, o serviço usará o modelo base antigo para reconhecimento.
-   Se ambos os modelos customizados forem baseados no modelo base mais recente, o serviço usará o novo modelo base para reconhecimento.
-   Se apenas um dos dois modelos customizados passar por upgrade para o modelo base mais recente, o serviço usará o modelo base antigo para reconhecimento. Ele seleciona o modelo base antigo porque essa é a versão que os dois modelos customizados têm em comum.
