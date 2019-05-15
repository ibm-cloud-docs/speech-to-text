---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

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

# Criando um modelo acústico customizado
{: #acoustic}

Siga estas etapas para criar um modelo acústico customizado para o serviço do {{site.data.keyword.speechtotextshort}}:
{: shortdesc}

1.  [Criar um modelo acústico customizado](#createModel-acoustic). É possível criar múltiplos modelos customizados para os mesmos domínios ou ambientes ou para domínios e ambientes diferentes. O processo é o mesmo para qualquer modelo que você criar. No entanto, é possível especificar apenas um único modelo acústico customizado por vez com uma solicitação de reconhecimento.
1.  [Incluir áudio no modelo acústico customizado](#addAudio). O serviço aceita os mesmos formatos de arquivo de áudio para modelagem acústica que aceita para reconhecimento de voz. Ele também aceita archives que contêm múltiplos arquivos de áudio. Os archives são o meio preferencial para incluir recursos de áudio. É possível repetir o método para incluir mais arquivos de áudio ou archive em um modelo customizado.
1.  [Treinar o modelo acústico customizado](#trainModel-acoustic). Depois de incluir recursos de áudio no modelo customizado, deve-se treinar o modelo. O treinamento prepara o modelo acústico customizado para uso no reconhecimento de voz. O treinamento pode levar uma quantia significativa de tempo. A duração do treinamento depende da quantia de dados de áudio que o modelo contém.

    É possível especificar um modelo de idioma customizado do assistente durante o treinamento de seu modelo acústico customizado. Um modelo de idioma customizado que inclui transcrições de seus arquivos de áudio ou palavras OOV do domínio de seus arquivos de áudio pode melhorar a qualidade do modelo acústico customizado. Para obter mais informações, consulte [Treinando um modelo acústico customizado com um modelo de idioma customizado](/docs/services/speech-to-text/acoustic-both.html#useBothTrain).
1.  Depois de treinar seu modelo customizado, é possível usá-lo com solicitações de reconhecimento. Se o áudio passado para a transcrição tiver qualidades acústicas semelhantes às do áudio do modelo customizado, os resultados refletirão o entendimento aprimorado do serviço. Para obter mais informações, consulte [Usando um modelo acústico customizado](/docs/services/speech-to-text/acoustic-use.html).

    É possível passar um modelo acústico customizado e um modelo de idioma customizado na mesma solicitação de reconhecimento para melhorar ainda mais a precisão de reconhecimento. Para obter mais informações, consulte [Usando modelos acústico e de idioma customizados durante o reconhecimento de voz](/docs/services/speech-to-text/acoustic-both.html#useBothRecognize).

As etapas para criar um modelo acústico customizado são iterativas. É possível incluir ou excluir áudio e treinar ou retreinar um modelo, tantas vezes quanto necessário. Deve-se retreinar um modelo para que qualquer mudança em seu áudio entre em vigor. Ao retreinar um modelo, todos os dados de áudio são usados no treinamento (não apenas os novos dados). Portanto, o tempo de treinamento é proporcional à quantidade total de áudio que está contida no modelo.

A customização do modelo acústico está disponível como funcionalidade beta para todos os idiomas. Para obter mais informações, consulte [Suporte ao idioma para customização](/docs/services/speech-to-text/custom.html#languageSupport).
{: note}

## Criar um modelo acústico customizado
{: #createModel-acoustic}

Use o método `POST /v1/acoustic_customizations` para criar um novo modelo acústico customizado. É possível criar qualquer número de modelos acústicos customizados, mas é possível usar apenas um modelo acústico customizado por vez com uma solicitação de reconhecimento de voz. O método aceita um objeto JSON que define os atributos do novo modelo customizado como o corpo da solicitação.

<table>
  <caption>Tabela 1. Atributos de um novo modelo acústico customizado</caption>
  <tr>
    <th style="width:20%; text-align:left">Parâmetros</th>
    <th style="width:12%; text-align:center">Tipo de Dados</th>
    <th style="text-align:left">Descrição</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>Necessário</em></td>
    <td style="text-align:center">String</td>
    <td>
      Um nome definido pelo usuário para o novo modelo acústico customizado. Use um nome que descreva o ambiente acústico do modelo customizado, como <code>Modelo customizado do dispositivo móvel</code> ou <code>Modelo customizado de carro barulhento</code>. Use um nome que seja exclusivo entre todos os modelos acústicos customizados que você possui. Use um nome localizado que corresponda ao idioma do modelo customizado.
    </td>
  </tr>
  <tr>
    <td><code>base_model_name</code><br/><em>Necessário</em></td>
    <td style="text-align:center">String</td>
    <td>
      O nome do modelo base que deve ser customizado pelo novo modelo. Deve-se usar o nome de um modelo que é retornado pelo
método <code>GET /v1/models</code>. O novo modelo customizado pode ser
usado apenas com o modelo base que ele customiza.
    </td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>Opcional</em></td>
    <td style="text-align:center">String</td>
    <td>
      Uma descrição do novo modelo. Use uma descrição localizada que corresponda ao idioma do modelo customizado.
    </td>
  </tr>
</table>

O exemplo a seguir cria um novo modelo acústico customizado denominado `Exemplo de modelo acústico`. O modelo é criado para o modelo base `en-US_BroadbandModel` e tem a descrição `Exemplo de modelo acústico customizado`. O cabeçalho `Content-Type` especifica que os dados JSON estão sendo passados para o método.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/json"
--data "{\"name\": \"Example acoustic model\",
  \"base_model_name\": \"en-US_BroadbandModel\",
  \"description\": \"Example custom acoustic model\"}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations"
```
{: pre}

O exemplo retorna o ID de customização do novo modelo. Cada modelo customizado é identificado por um ID de customização exclusivo, que é um Identificador Exclusivo Global (GUID). Você especifica um GUID do modelo customizado com o parâmetro `customization_id` das chamadas que estão associadas ao modelo.

```javascript
{
  "customization_id": "74f4807e-b5ff-4866-824e-6bba1a84fe96"
}
```
{: codeblock}

O novo modelo customizado é de propriedade da instância de serviço cujas credenciais são usadas para criá-lo. Para obter mais informações, consulte [Propriedade de modelos customizados](/docs/services/speech-to-text/custom.html#customOwner).

## Incluir áudio no modelo acústico customizado
{: #addAudio}

Depois de criar seu modelo acústico customizado, a próxima etapa é incluir recursos de áudio nele. Use o método `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` para incluir um recurso de áudio em um modelo customizado. É possível incluir

-   Um arquivo de áudio individual em qualquer formato que seja suportado para reconhecimento de voz (consulte [Formatos de áudio](/docs/services/speech-to-text/audio-formats.html)).
-   Um archive (um arquivo **.zip** ou **.tar.gz**) que inclui múltiplos arquivos de áudio. Reunir múltiplos arquivos de áudio em um único archive e carregar esse arquivo único é significativamente mais eficiente do que incluir arquivos de áudio individualmente.

Passe o recurso de áudio como o corpo da solicitação e designe ao recurso um `audio _name`. Para obter mais informações, consulte [Trabalhando com recursos de áudio](/docs/services/speech-to-text/acoustic-resource.html).

Os exemplos a seguir mostram a inclusão de ambos os recursos de tipo áudio e archive:

-   Este exemplo inclui um recurso de tipo áudio para o modelo acústico customizado com o `customization_id` especificado. O cabeçalho `Content-Type` identifica o tipo de áudio como `audio/wav`. O arquivo de áudio, **audio1.wav**, é passado como o corpo da solicitação e o recurso recebe o nome `audio1`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/wav"
    --data-binary @audio1.wav
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
    ```
    {: pre}

-   Esse exemplo inclui um recurso de tipo archive para o modelo acústico customizado especificado. O cabeçalho `Content-Type` identifica o tipo do archive como `application/zip`. O cabeçalho `Contained-Contented-Type` indica que todos os arquivos que estão contidos no archive têm o formato `audio/l16` e são amostrados a uma taxa de 16 kHz. O archive, **audio2.zip**, é passado como o corpo da solicitação e o recurso recebe o nome `audio2`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: application/zip"
    --header "Contained-Content-Type: audio/l16;rate=16000"
    --data-binary @audio2.zip
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
    ```
    {: pre}

O método também aceita um parâmetro de consulta opcional `allow_overwrite` para sobrescrever um recurso de áudio existente para um modelo customizado. Use o parâmetro se você precisar atualizar um recurso de áudio depois de incluí-lo em um modelo.

O método é assíncrono. Pode levar vários segundos para ser concluído, dependendo da duração do áudio. Para um archive, a duração da operação depende da duração dos arquivos de áudio. Para obter mais informações sobre como verificar o status de uma solicitação para incluir um recurso de áudio, consulte [Monitorando a solicitação de inclusão de áudio](#monitorAudio).

É possível incluir qualquer número de recursos de áudio em um modelo customizado chamando o método uma vez para cada arquivo de áudio ou archive. A inclusão de um recurso de áudio deve ser completamente concluída antes que outra possa ser incluída. Deve-se incluir um mínimo de 10 minutos e um máximo de 200 horas de áudio que inclui fala, não silêncio, em um modelo customizado antes de poder treiná-lo. Nenhum recurso de tipo áudio ou archive pode ser maior que 100 MB. Para obter mais informações, consulte [Diretrizes para incluir recursos de áudio](/docs/services/speech-to-text/acoustic-resource.html#audioGuidelines).

### Monitorando a solicitação de inclusão de áudio
{: #monitorAudio}

O serviço retornará um código de resposta 201 se o áudio for válido. Em seguida, analisa de forma assíncrona os conteúdos dos arquivos de áudio e extraia automaticamente as informações sobre o áudio, como duração, taxa de amostragem e codificação. Não é possível enviar solicitações para incluir mais áudio em um modelo acústico customizado ou para treinar o modelo, até que a análise do serviço de todos os arquivos de áudio para a solicitação atual seja concluída.

Para determinar o status da solicitação, use o método `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` para pesquisar o status do áudio. O método aceita o GUID do modelo customizado e o nome do recurso de áudio. Sua resposta inclui o `status` do recurso, que tem um dos valores a seguir:

-   `ok` indica que o áudio é aceitável e a análise está concluída.
-   `being_processed` indica que o serviço ainda está analisando o áudio.
-   `invalid` indica que o arquivo de áudio não é aceitável para processamento. Ele pode ter o formato errado, a taxa de amostragem errada ou não ser um arquivo de áudio. Para um archive, se qualquer um dos arquivos de áudio que ele contiver for inválido, o archive inteiro será inválido.

O conteúdo da resposta e da localização do campo `status` depende do tipo de recurso, áudio ou archive.

-   *Para um recurso de tipo áudio*, o campo `status` está localizado no objeto de nível superior (`AudioListing`).

    ```bash
    curl -X GET -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
    ```
    {: pre}

    ```javascript
    {
      "duration": 131,
      "name": "audio1",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 22050
      }
      "status": "ok"
    }
    ```
    {: codeblock}

-   *Para um recurso de tipo archive*, o campo `status` está localizado no objeto de segundo nível (`AudioResource`) que está aninhado no campo `container`.

    ```bash
    curl -X GET -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
    ```
    {: pre}

    ```javascript
    {
      "container": {
        "duration": 556,
        "name": "audio2",
        "details": {
          "type": "archive",
          "compression": "zip"
        },
        "status": "ok"
      },
      . . .
    ```
    {: codeblock}

Use um loop para verificar o status do recurso de áudio a cada poucos segundos até que ele se torne `ok`. Para obter mais informações sobre outros campos que são retornados pelo método, consulte [Listando recursos de áudio para um modelo acústico customizado](/docs/services/speech-to-text/acoustic-audio.html#listAudio).

## Treinar o modelo acústico customizado
{: #trainModel-acoustic}

Depois de preencher um modelo acústico customizado com recursos de áudio, deve-se treinar o modelo nos novos dados. O treinamento prepara um modelo customizado para uso no reconhecimento de voz. Um modelo não pode ser usado para solicitações de reconhecimento até você treiná-lo nos novos dados. Além disso, as atualizações para um modelo customizado na forma de recursos de áudio novos ou mudados não são refletidas pelo modelo até que você o treine com as mudanças.

Use o método `POST /v1/acoustic_customizations/{customization_id}/train` para treinar um modelo customizado. Passe ao método o ID de customização do modelo que você deseja treinar, como no exemplo a seguir.

```bash
curl -X POST -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/train"
```
{: pre}

O método também aceita um parâmetro de consulta opcional `custom_language_model_id` para especificar um modelo de idioma customizado criado separadamente que deve ser usado durante o treinamento. É possível treinar com um modelo de idioma customizado que contém transcrições de seus arquivos de áudio ou que contém palavras dos corpora ou OOV que são relevantes para os conteúdos dos arquivos de áudio. Os dois modelos customizados devem ser baseados na mesma versão do mesmo modelo base para que o treinamento obtenha êxito. Para obter mais informações, consulte [Treinando um modelo acústico customizado com um modelo de idioma customizado](/docs/services/speech-to-text/acoustic-both.html#useBothTrain).

O método de treinamento é assíncrono. O treinamento pode levar minutos ou horas para ser concluído, dependendo da quantia de dados de áudio que o modelo acústico customizado contém e da carga atual no serviço. De modo geral, o treinamento de um modelo acústico customizado leva aproximadamente de duas a quatro vezes o comprimento de seus dados de áudio. O intervalo de tempo depende do modelo que está sendo treinado e da natureza do áudio, como se o áudio é limpo ou tem ruído. Por exemplo, pode levar entre quatro e oito horas para treinar um modelo que contém duas horas de áudio. Para obter mais informações sobre como verificar o status de uma operação de treinamento, consulte [Monitorando a solicitação de treinamento do modelo](#monitorTraining-acoustic).

### Monitorando a solicitação de treinamento do modelo
{: #monitorTraining-acoustic}

O serviço retornará um código de resposta 200 se o processo de treinamento for iniciado com êxito. O serviço não pode aceitar solicitações de treinamento subsequentes ou solicitações para incluir mais recursos de áudio até que a solicitação existente seja concluída.

Para determinar o status de uma solicitação de treinamento, use o método `GET /v1/acoustic_customizations/{customization_id}` para pesquisar o status do modelo. O método aceita o ID de customização do modelo acústico e retorna seu status, como no exemplo a seguir:

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
  "owner": "297cfd08-330a-22ba-93ce-1a73f454dd98",
  "name": "Example model",
  "description": "Example custom acoustic model",
  "base_model_name": "en-US_BroadbandModel",
  "status": "training",
  "progress": 0
}
```
{: codeblock}

A resposta inclui os campos `status` e `progress` que relatam o estado atual do modelo. O significado do campo `progress` depende do status do modelo. O campo `status` pode ter um dos valores a seguir:

-   `pending` indica que o modelo foi criado, mas está aguardando que os dados de treinamento sejam incluídos ou que o serviço conclua a análise dos dados que foram incluídos. O campo `progress` é `0`.
-   `ready` indica que o modelo está pronto para ser treinado. O campo `progress` é `0`.
-   `training` indica que o modelo está sendo treinado. O campo `progress` muda de `0` para `100` quando o treinamento é concluído. <!-- The `progress` field indicates the progress of the training as a percentage complete. -->
-   `available` indica que o modelo está treinado e pronto para uso. O campo `progress` é `100`.
-   `upgrading` indica que o modelo está passando por upgrade. O campo `progress` é `0`.
-   `failed` indica que o treinamento do modelo falhou. O campo `progress` é `0`.

Use um loop para verificar o status do treinamento uma vez por minuto até que o modelo se torne `available`. Para obter mais informações sobre outros campos que são retornados pelo método, consulte [Listando modelos acústicos customizados](/docs/services/speech-to-text/acoustic-models.html#listModels-acoustic).

### Falhas de treinamento
{: #failedTraining-acoustic}

O treinamento de um modelo acústico customizado falhará ao iniciar se o serviço estiver manipulando outra solicitação para o modelo customizado. Uma solicitação conflitante pode ser outra solicitação de treinamento ou uma solicitação para incluir recursos de áudio no modelo. O treinamento também pode falhar ao iniciar pelos motivos a seguir:

-   O modelo customizado contém menos de 10 minutos de dados de áudio.
-   O modelo customizado contém mais de 200 horas de dados de áudio.
-   Um ou mais dos recursos de áudio do modelo customizado são inválidos.
-   Você passou um modelo de idioma customizado incompatível com o parâmetro de consulta `custom_language_model_id`. Os dois modelos customizados devem ser baseados na mesma versão do mesmo modelo base.

Se o status de um treinamento do modelo customizado for `failed`, use os métodos `GET /v1/acoustic_customizations/{customization_id}/audio` e `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` para examinar os recursos de áudio do modelo e lidar com qualquer problema que você encontrar. Para obter mais informações, consulte [Listando recursos de áudio para um modelo acústico customizado](/docs/services/speech-to-text/acoustic-audio.html#listAudio).
