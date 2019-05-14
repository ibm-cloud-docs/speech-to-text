---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-11"

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

# A interface de HTTP assíncrona
{: #async}

A interface de HTTP assíncrona do serviço do {{site.data.keyword.speechtotextfull}} fornece métodos para transcrição de áudio por meio de chamadas sem bloqueio para o serviço. A interface emprega sequências secretas especificadas pelo usuário e assinaturas digitais para fornecer um nível de segurança para solicitações que são feitas por meio do protocolo HTTP. Para usar a interface assíncrona, é possível
{: shortdesc}

-   Registre uma URL de retorno de chamada para ser notificado pelo serviço do status da tarefa e os resultados automaticamente.
-   Pesquise o serviço para obter o status da tarefa e os resultados manualmente.

As duas abordagens não são mutuamente exclusivas. É possível optar por receber notificações de retorno de chamada, mas ainda pesquisar o serviço para obter o status mais recente ou entrar em contato com o serviço para recuperar os resultados manualmente. As seções a seguir descrevem como usar a interface de HTTP assíncrona com qualquer abordagem.

Submeta um máximo de 1 GB e um mínimo de 100 bytes de dados de áudio com uma única solicitação. Para obter informações sobre formatos de áudio e sobre como usar compactação para maximizar a quantia de áudio que você pode enviar com uma solicitação, consulte [Formatos de áudio](/docs/services/speech-to-text/audio-formats.html). Para obter mais informações sobre os métodos individuais da interface, consulte a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.

## Modelos de uso
{: #usage}

Quando você trabalhar com a interface assíncrona de HTTP do serviço, será possível optar por aprender sobre o status da tarefa e receber resultados das maneiras a seguir:

-   Usando notificações de retorno de chamada:
    1.  Chame o método `POST /v1/register_callback` para registrar uma URL de retorno de chamada com o serviço. É possível fornecer uma sequência secreta especificada pelo usuário opcional para ativar a autenticação e a integridade de dados para retornos de chamada que são enviados para a URL.
    1.  Chame o método `POST /v1/recognitions` com uma URL de retorno de chamada já registrada para a qual o serviço envia notificações quando o status da tarefa muda. Você especifica uma lista de eventos dos quais deve ser notificado. Por padrão, o serviço envia notificações quando uma tarefa é iniciada, quando ela é concluída e se ocorre um erro. Também é possível solicitar os resultados da solicitação na notificação de conclusão. Caso contrário, será necessário usar o método `GET /v1/recognitions/{id}` para recuperar os resultados.
-   Ao pesquisar o serviço:
    1.  Chame o método `POST /v1/recognitions` sem uma URL de retorno de chamada, eventos ou um token do usuário.
    1.  Periodicamente, chame o método `GET /v1/recognitions` para verificar o status das tarefas mais recentes ou o método `GET /v1/recognitions/{id}` para verificar o status de uma tarefa específica.
    1.  Se você verificar o status da tarefa com o método `GET /v1/recognitions`, chame o método `GET /v1/recognitions/{id}` para recuperar os resultados da tarefa depois que ela estiver concluída.

As duas abordagens podem ser usadas juntas. Ainda é possível pesquisar o serviço para obter o status mais recente para uma tarefa que é criada com uma URL de retorno de chamada. Por exemplo, você poderá desejar obter o status de uma tarefa se as notificações estiverem levando um tempo para chegar. Você também poderá verificar o status se suspeitar que perdeu uma ou mais notificações devido a um erro de serviço ou de rede.

## Registrando uma URL de retorno de chamada
{: #register}

Você registra uma URL de retorno de chamada ao chamar o método `POST /v1/register_callback`. Depois de registrar uma URL de retorno de chamada, é possível usá-la para receber notificações para um número indefinido de tarefas. O processo de registro inclui quatro etapas:

1.  Você chama o método `POST /v1/register_callback` e passa uma URL de retorno de chamada. Opcionalmente, é possível especificar também um segredo especificado pelo usuário. O serviço usa o segredo para calcular assinaturas Secure Hash Algorithm 1 (SHA1) de código de autenticação de mensagem hash com chave (HMAC) para autenticação e integridade de dados. O exemplo a seguir registra um retorno de chamada do usuário que responde na URL `http://{user_callback_path}/results`. A chamada inclui um segredo do usuário de `ThisIsMySecret`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/register_callback?callback_url=http://{user_callback_path}/results&user_secret=ThisIsMySecret"
    ```
    {: pre}

1.  O serviço tenta validar, ou inserir na lista de desbloqueio, a URL de retorno de chamada se ela ainda não estiver registrada, enviando uma solicitação `GET` para a URL de retorno de chamada. O serviço passa uma sequência de desafio alfanumérica aleatória por meio do parâmetro de consulta `challenge_string` da solicitação. A solicitação inclui um cabeçalho `Accept` que especifica `text/plain` como o tipo de resposta necessário.

    Se a chamada para o método `register_callback` incluiu um segredo do usuário, a solicitação `GET` do serviço também incluirá um cabeçalho `X-Callback-Signature` que especifica a assinatura HMAC-SHA1 da sequência de desafio. O serviço calcula a assinatura usando o segredo do usuário como a chave.

    ```
    GET http://{user_callback_path}/results?challenge_string=n9ArPGMQ36Hiu7QC
    header: X-Callback-Signature {HMAC-SHA1_signature}
    ```
    {: codeblock}

1.  Você responde à solicitação `GET` por meio do serviço com código de status 200. Inclua a sequência de desafio que foi enviada pelo serviço na resposta. Inclua a sequência em texto simples no corpo da resposta e configure o cabeçalho de resposta `Content-Type` para `text/plain`.

    Se a solicitação inicial do `POST` incluiu um segredo do usuário, será possível calcular uma assinatura HMAC-SHA1 da sequência de desafio usando o segredo como a chave. Se a solicitação `GET` foi enviada pelo serviço, a assinatura corresponderá ao valor especificado pelo cabeçalho `X-Callback-Signature`.

    ```
    response code: 200 OK
    body: n9ArPGMQ36Hiu7QC
    ```
    {: codeblock}

1.  O serviço verifica se a sequência de desafio é retornada no corpo da resposta para a sua solicitação `GET`. Se sim, o serviço inserirá na lista de desbloqueio a URL de retorno de chamada e responderá à sua solicitação `POST` original com o código de status 201. O corpo da resposta inclui um objeto JSON com um campo `status` que tem o valor `created` e um campo `url` que tem o valor de sua URL de retorno de chamada.

    ```
    response code: 201 Created
    body: {
      "status": "created",
      "url": "http://{user_callback_path}/results"
    }
    ```
    {: codeblock}

O serviço envia apenas uma única solicitação `GET` para uma URL de retorno de chamada durante o processo de registro. O serviço deve receber uma resposta com o código de resposta 200 que inclui a sequência de desafio em seu corpo dentro de cinco segundos. Caso contrário, ele não insere a URL na lista de desbloqueio. Em vez disso, ele envia o código de status 400 em resposta para a solicitação `POST /v1/register_callback`. Se a URL de retorno de chamada já tiver sido listada com êxito na lista de desbloqueio, o serviço enviará o código de status 200 em resposta à solicitação inicial do `POST`.

### Considerações de segurança
{: #security-async}

Quando você usa com êxito o método `POST /v1/register_callback` para registrar uma URL de retorno de chamada, o serviço incluirá a URL na lista de desbloqueio para indicar que ela é verificada para uso com notificações de retorno de chamada. Se você especificar um segredo do usuário com a chamada de registro, a lista de desbloqueio também significará que a URL foi validada para a segurança incluída. Especificar um segredo do usuário fornece autenticação e integridade de dados para solicitações que usam a URL de retorno de chamada com a interface de HTTP assíncrona.

O serviço usa o segredo do usuário para calcular uma assinatura HMAC-SHA1 sobre a carga útil de cada notificação de retorno de chamada que ele envia para a URL. O serviço envia a assinatura por meio do cabeçalho `X-Callback-Signature` com cada notificação. O cliente pode usar o segredo para calcular sua própria assinatura de cada carga útil de notificação. Se a sua assinatura corresponder ao valor do cabeçalho `X-Callback-Signature`, o cliente saberá que a notificação foi enviada pelo serviço e que seu conteúdo não foi alterado durante a transmissão. Esse conhecimento garante que o cliente não é a vítima de um ataque man-in-middle.

HTTPS é ideal para aplicativos de produção. Mas durante o desenvolvimento do aplicativo e a criação de protótipo, as notificações de retorno de chamada baseadas em HTTP que são suportadas pelo serviço podem simplificar e acelerar o processo de desenvolvimento, evitando a despesa de HTTPS.

<!--

However, communicating over the HTTPS protocol is always the most secure means of learning job status and retrieving results. Using the HTTPS `GET /v1/recognitions/{id}` method to retrieve the results of a job is therefore more secure that receiving the results via callback notification. While the use of HMAC-SHA1 signatures based on a user secret ensures authentication and data integrity for callback notifications, it does not provide confidentiality. Conversely, because it encrypts the body of the response, HTTPS can provide authentication, integrity, *and* confidentiality.

HTTPS, however, is not ideal in terms of additional development overhead. Moreover, although the service validates SSL certificates to prevent man-in-the-middle attacks when you use HTTPS, validation is not foolproof if you use self-signed certificates, which enable encryption but not authentication or data integrity. During application development, user secrets and digital signatures provide a suitable level of security for requests made over the HTTP protocol. You can work with HTTP callback notifications during prototyping and move to HTTPS only for application deployment.

-->

### Cancelando o registro de uma URL de retorno de chamada
{: #unregister}

É possível cancelar o registro de uma URL de retorno de chamada inserida em uma lista de desbloqueio a qualquer momento chamando o método `POST /v1/unregister_callback`. Cancelar o registro de uma URL de retorno de chamada pode ser útil para testar seu aplicativo com o serviço. Depois de cancelar o registro de uma URL de retorno de chamada, não será mais possível utilizá-la com solicitações de reconhecimento assíncrono.

## Criando uma tarefa
{: #create}

Crie uma tarefa de reconhecimento chamando o método `POST /v1/recognitions`. Como você aprende o status e os resultados da tarefa depende da abordagem que você usa e dos parâmetros que passa:

-   *Para usar as notificações de retorno de chamada*, inclua o parâmetro de consulta `callback_url` para especificar uma URL para a qual o serviço deve enviar notificações de retorno de chamada quando o status das mudanças da tarefa muda. Também é possível especificar os parâmetros de consulta opcionais a seguir:
    -   `events` para assinar uma lista de eventos de notificação. Por padrão, o serviço envia notificações de retorno de chamada quando a tarefa é iniciada (o evento `recognitions.started`), quando a tarefa é concluída (o evento `recognitions.complete`) e se ocorrer um erro (o evento `recognitions.failed`). É possível especificar um subconjunto dos eventos ou usar o evento `recognitions.completed_with_results` em vez do evento `recognitions.completed` para incluir os resultados com a notificação da tarefa concluída.
    -   `user_token` para especificar uma sequência que deve ser incluída com cada notificação para a tarefa. Como é possível usar a mesma URL de retorno de chamada com um número indefinido de tarefas, é possível aproveitar os tokens do usuário para diferenciar notificações para diferentes tarefas.
-   *Para usar a pesquisa*, omita os parâmetros de consulta `callback_url`, `events` e `user_token`. Deve-se, então, usar os métodos `GET /v1/recognitions` ou `GET /v1/recognitions/{id}` para verificar o status da tarefa, usando este último para recuperar os resultados quando a tarefa for concluída.

Em ambos os casos, é possível incluir o parâmetro de consulta `results_ttl` para especificar o número de minutos para os quais os resultados devem permanecer disponíveis após a conclusão da tarefa.

Além dos parâmetros anteriores, que são específicos para a interface assíncrona, o método `POST /v1/recognitions` suporta a maioria dos mesmos parâmetros que as interfaces HTTP síncrona e do WebSocket. Para obter mais informações, consulte o [Resumo de parâmetro](/docs/services/speech-to-text/summary.html).

### Notificações de retorno de chamada
{: #notifications}

Se a tarefa for criada com uma URL de retorno de chamada, o serviço enviará uma notificação de retorno de chamada HTTP `POST` para a URL registrada quando ocorrer um evento especificado. O corpo de uma notificação básica consiste em um objeto JSON com a estrutura a seguir:

```javascript
{
  "id": "{job_id}",
  "event": "{recognitions_status}",
  "user_token": "{user_token}"
}
```
{: codeblock}

O campo `id` identifica o ID da tarefa que gerou o retorno de chamada e o campo `event` identifica o evento que acionou o retorno de chamada. O campo `user_token` incluirá o token do usuário para a tarefa, se um foi especificado. Caso contrário, o campo será uma sequência vazia. Se o evento for `recognitions.completed_with_results`, o objeto incluirá um campo `results` que forneça os resultados da solicitação de reconhecimento. O cliente pode responder à notificação de retorno de chamada com o código de status 200.

Se a URL de retorno de chamada foi registrada com um segredo do usuário, o serviço também enviará o cabeçalho `X-Callback-Signature` com a notificação de retorno de chamada. O cabeçalho especifica a assinatura HMAC-SHA1 do corpo da solicitação. O serviço calcula a assinatura usando o segredo do usuário como uma chave. O cliente pode calcular a assinatura da carga útil para a notificação de retorno de chamada para ter certeza de que corresponde à assinatura no cabeçalho. Por exemplo, o código Python simples a seguir calcula a assinatura na sequência `notification_payload`:

```python
from hashlib import sha1
import hmac
hashed = hmac.new(user_secret, notification_payload, sha1)
signature = hashed.digest().encode("base64").rstrip('\n')
```
{: codeblock}

### Exemplo de retorno de chamada
{: #callback}

O exemplo a seguir cria uma tarefa que está associada à URL de retorno de chamada inserida na lista de desbloqueio anteriormente `http://{user_callback_path}/results`. O exemplo transmite o token do usuário `job25` para identificar a tarefa em notificações de retorno de chamada que são enviadas pelo serviço. A chamada usa os eventos padrão, portanto, o usuário deverá chamar o método `GET /v1/recognitions/{id}` para recuperar os resultados quando o serviço enviar uma notificação de retorno de chamada para indicar que a tarefa está concluída. A chamada configura o parâmetro de consulta `timestamps` da solicitação de reconhecimento para `true`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/flac"
--data-binary @{path}audio-file.flac
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?callback_url=http://{user_callback_path}/results&user_token=job25&timestamps=true"
```
{: pre}

O serviço retorna o status da solicitação, que é `waiting` para indicar que o serviço está preparando a tarefa para processamento. A resposta inclui o horário de criação, o ID da tarefa e a URL na qual obter mais informações sobre a tarefa.

```javascript
{
  "created": "2016-08-17T19:15:17.926Z",
  "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bd734c0-e575-21f3-de03-f932aa0468a0",
  "status": "waiting"
}
```
{: codeblock}

### Exemplo de pesquisa
{: #polling}

O exemplo a seguir cria uma tarefa que não está associada a uma URL de retorno de chamada. O usuário deve pesquisar o serviço para saber quando a tarefa está concluída e, em seguida, recuperar os resultados com o método `GET /v1/recognitions/{id}`. Como o exemplo anterior, a chamada configura o parâmetro `timestamps` da solicitação de reconhecimento para `true`.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @{path}audio-file.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?timestamps=true"
```
{: pre}

O serviço retorna um status de `processing` para indicar que ele já está processando a tarefa, juntamente com o horário de criação, o ID da tarefa e a URL para obter informações sobre a tarefa.

```javascript
{
  "created": "2016-08-17T19:13:23.622Z",
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "url": "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "status": "processing"
}
```
{: codeblock}

## Verificando o status e recuperando os resultados de uma tarefa
{: #job}

Chame o método `GET /v1/recognitions/{id}` para verificar o status da tarefa que é especificado com o parâmetro de caminho `id`. A resposta sempre inclui o ID e o status da tarefa e seus horários de criação e atualização. Se o status for `completed`, a resposta também incluirá os resultados da solicitação de reconhecimento.

O método `GET /v1/recognitions/ { id }` é a única maneira de recuperar os resultados da tarefa se

-   A tarefa foi enviada sem uma URL de retorno de chamada.
-   A tarefa foi enviada com uma URL de retorno de chamada, mas sem especificar o evento `recognitions.completed_with_results`.
-   A tarefa não é uma das 100 tarefas pendentes mais recentes. Quando você omitir o parâmetro de caminho `id`, somente as 100 tarefas mais recentes serão retornadas.

No entanto, ainda é possível usar o método para recuperar os resultados para uma tarefa que especificou uma URL de retorno de chamada e o evento `recognitions.completed_with_results`. É possível recuperar os resultados para qualquer tarefa quantas vezes você desejar enquanto elas permanecem disponíveis. Uma tarefa e seus resultados permanecem disponíveis até você excluí-los com o método `DELETE /v1/recognitions/{id}` ou até que o tempo de vida da tarefa expire, o que ocorrer primeiro. Por padrão, os resultados expiram após uma semana, a menos que você especifique um tempo de vida diferente com o parâmetro `results_ttl` do método `POST /v1/recognitions`.

### Exemplo de status sem resultados
{: #withoutResults}

O exemplo a seguir verifica o status da tarefa com o ID especificado. A tarefa ainda não está concluída, portanto, a resposta não inclui os resultados.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}

```javascript
{
  "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
  "created": "2016-08-17T19:13:23.622Z",
  "updated": "2016-08-17T19:13:24.434Z",
  "status": "processing"
}
```
{: codeblock}

### Exemplo de status com resultados
{: #withResults}

O exemplo a seguir solicita o status da tarefa com o ID especificado. A tarefa está concluída, portanto, a resposta inclui os resultados da solicitação.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}

```javascript
{
  "id": "398fcd80-330a-22ba-93ce-1a73f454dd98",
  "results": [
    {
      "result_index": 0,
      "results": [
        {
          "final": true,
          "alternatives": [
            {
              "transcript": "several tornadoes touch down as a line of severe thunderstorms swept through Colorado on Sunday ",
              "timestamps": [
                [
                  "several",
                  1,
                  1.52
                ],
                [
                  "tornadoes",
                  1.52,
                  2.15
                ],
                . . .
                [
                  "Sunday",
                  5.74,
                  6.33
                ]
              ],
              "confidence": 0.89
            }
          ]
        }
      ]
    }
  ],
  "created": "2016-08-17T19:11:04.298Z",
  "updated": "2016-08-17T19:11:16.003Z",
  "status": "completed"
}
```
{: codeblock}

## Verificando o status das tarefas mais recentes
{: #jobs}

Você chama o método `GET /v1/recognitions` para verificar o status das tarefas mais recentes. O método retorna o status das 100 tarefas mais recentes mais recentes que estão associadas às credenciais de serviço com as quais ela é chamada. O método retorna o ID e o status de cada tarefa, juntamente com suas horas de criação e atualização. Se uma tarefa foi criada com uma URL de retorno de chamada e um token do usuário, o método também retornará o token do usuário para a tarefa.

A resposta inclui um dos estados a seguir:

-   `waiting` se o serviço estiver preparando a tarefa para processamento. Esse status é o estado inicial de todas as tarefas. A tarefa permanece nesse estado até que o serviço tenha a capacidade para começar a processá-la.
-   `processing` se o serviço estiver processando ativamente a tarefa.
-   `completed` se o serviço tiver concluído o processamento da tarefa. Se a tarefa especificou uma URL de retorno de chamada e o evento `recognitions.completed_with_results`, o serviço enviou os resultados com a notificação de retorno de chamada. Caso contrário, use o método `GET /v1/recognitions/{id}` para obter os resultados.
-   `failed` se a tarefa falhou por algum motivo.

Uma tarefa e seus resultados permanecem disponíveis até você excluí-los com o método `DELETE /v1/recognitions/{id}` ou até que o tempo de vida da tarefa expire, o que ocorrer primeiro.

### Exemplo
{: #statusExample}

O exemplo a seguir solicita o status das tarefas atuais mais recentes que estão associadas às credenciais de serviço do responsável pela chamada. O usuário tem três tarefas pendentes em vários estados. A primeira tarefa foi criada com uma URL de retorno de chamada e um token do usuário.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions"
```
{: pre}

```javascript
{
  "recognitions": [
    {
      "id": "4bd734c0-e575-21f3-de03-f932aa0468a0",
      "created": "2016-08-17T19:15:17.926Z",
      "updated": "2016-08-17T19:15:17.926Z",
      "status": "waiting",
      "user_token": "job25"
    },
    {
      "id": "4bb1dca0-f6b1-11e5-80bc-71fb7b058b20",
      "created": "2016-08-17T19:13:23.622Z",
      "updated": "2016-08-17T19:13:24.434Z",
      "status": "processing"
    },
    {
      "id": "398fcd80-330a-22ba-93ce-1a73f454dd98",
      "created": "2016-08-17T19:11:04.298Z",
      "updated": "2016-08-17T19:11:16.003Z",
      "status": "completed"
    }
  ]
}
```
{: codeblock}

## Excluindo uma tarefa
{: #delete-async}

É possível usar o método `DELETE /v1/recognitions/{id}` para excluir a tarefa que é especificada com o parâmetro de caminho `id`. Geralmente, você exclui uma tarefa depois de obter seus resultados do serviço. Depois de excluir uma tarefa, seus resultados não estarão mais disponíveis. Não é possível excluir uma tarefa que o serviço está processando ativamente.

Por padrão, o serviço mantém os resultados de cada tarefa até que o tempo de vida da tarefa expire. O tempo de vida padrão é uma semana, mas é possível usar o parâmetro `results_ttl` do método `POST /v1/recognitions` para especificar o número de minutos que o serviço deve manter os resultados.

### Exemplo
{: #deleteExample-async}

O exemplo a seguir exclui a tarefa com o ID especificado:

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions/{job_id}"
```
{: pre}
