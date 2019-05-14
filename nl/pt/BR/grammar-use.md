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

# Usando uma gramática para reconhecimento de voz
{: #grammarUse}

Assim que você criar e treinar o seu modelo de idioma customizado com a sua gramática, será possível usar a gramática em solicitações de reconhecimento de voz com as interfaces do WebSocket e de HTTP do serviço.
{: shortdesc}

-   Use o parâmetro `language_customization_id` para especificar o ID de customização (GUID) do modelo de idioma customizado para o qual a gramática está definida. Deve-se emitir a solicitação com as credenciais de serviço para a instância do serviço que tem o modelo.
-   Use o parâmetro `grammar_name` para especificar o nome da gramática. É possível especificar somente uma única gramática com uma solicitação.

Quando você usa uma gramática, o serviço reconhece apenas palavras da gramática especificada. O serviço não usa palavras customizadas que foram incluídas dos corpora, que foram incluídas ou modificadas individualmente ou que são reconhecidas por outras gramáticas.

-   Para a [interface do WebSocket](/docs/services/speech-to-text/websockets.html), você primeiro especifica o ID de customização com o parâmetro `language_customization_id` do método `/v1/recognize`. Você usa esse método para estabelecer uma conexão do WebSocket com o serviço.

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}

    Em seguida, especifique o nome da gramática com o parâmetro `grammar_name` na mensagem JSON `start` para a conexão ativa. Transmitir esse valor com a mensagem `start` permite mudar a gramática dinamicamente para cada solicitação que você envia por meio da conexão.

    ```javascript
    function onOpen(evt) {
      var message = {
        action: 'start',
        content-type: 'audio/l16;rate=22050',
        grammar_name: '{grammar_name}'
      };
      websocket.send(JSON.stringify(message));
      websocket.send(blob);
    }
    ```
    {: codeblock}
-   Para a [interface de HTTP síncrona](/docs/services/speech-to-text/http.html)., passe os dois parâmetros com o método `POST /v1/recognize`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
-   Para a [interface de HTTP assíncrona](/docs/services/speech-to-text/async.html), passe os dois parâmetros com o método `POST /v1/recognitions`.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?language_customization_id={customization_id}&grammar_name={grammar_name}"
    ```
    {: pre}
