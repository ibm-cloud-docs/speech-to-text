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

# Usando um modelo de idioma customizado
{: #languageUse}

Depois de criar e treinar seu modelo de idioma customizado, é possível usá-lo em solicitações de reconhecimento de voz. Você usa o parâmetro de consulta `language_customization_id` para especificar o modelo de idioma customizado para uma solicitação, conforme mostrado nos exemplos a seguir. Também é possível informar ao serviço quanto peso dar às palavras do modelo customizado. Para obter mais informações, consulte [Usando o peso de customização](#weight). Deve-se emitir a solicitação com credenciais para a instância do serviço que possui o modelo.
{: shortdesc}

É possível criar múltiplos modelos de idioma customizados para os mesmos domínios ou domínios diferentes. No entanto, é possível especificar apenas um modelo de idioma customizado de cada vez com o parâmetro `language_customization_id`. Para obter exemplos que usam uma gramática com um modelo de idioma customizado, consulte [Usando uma gramática para reconhecimento de voz](/docs/services/speech-to-text?topic=speech-to-text-grammarUse).

-   Para a [interface do WebSocket](/docs/services/speech-to-text?topic=speech-to-text-websockets), use o método `/v1/recognize`. O modelo customizado especificado é usado para todas as solicitações que são enviadas por meio da conexão.

    ```javascript
    var token = {authentication-token};
    var wsURI = 'wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize'
      + '?access_token=' + IAM_access_token
      + '&model=es-ES_BroadbandModel'
      + '&language_customization_id={customization_id}';
    var websocket = new WebSocket(wsURI);
    ```
    {: codeblock}
-   Para a [interface de HTTP síncrona](/docs/services/speech-to-text?topic=speech-to-text-http), use o método `POST /v1/recognize`. O modelo customizado especificado é usado para essa solicitação.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}"
    ```
    {: pre}
-   Para a [interface de HTTP assíncrona](/docs/services/speech-to-text?topic=speech-to-text-async), use o método `POST /v1/recognitions`. O modelo customizado especificado é usado para essa solicitação.

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognitions?language_customization_id={customization_id}"
    ```
    {: pre}

Será possível omitir o modelo de idioma da solicitação se o modelo customizado for baseado no modelo de idioma padrão, `en-US_BroadbandModel`. Caso contrário, deve-se usar o parâmetro `model` para especificar o modelo base, conforme mostrado para o exemplo do WebSocket. Um modelo customizado pode ser usado somente com o modelo base para o qual ele é criado.

## Usando o peso de customização
{: #weight}

Um modelo de idioma customizado é uma combinação do modelo customizado e do modelo base que ele customiza. É possível informar ao serviço quanto peso dar a palavras do modelo de idioma customizado comparado com palavras do modelo base para reconhecimento de voz. O peso que é designado a um modelo customizado é conhecido como seu *peso de customização*.

Você especifica o peso relativo para um modelo de idioma customizado como um duplo entre 0,0 e 1,0. Por padrão, cada modelo de idioma customizado tem um peso de 0,3. O peso padrão produz o melhor desempenho no caso geral. Ele permite que as palavras OOV do modelo customizado e as palavras do vocabulário base sejam reconhecidas.

No entanto, em casos em que o áudio a ser transcrito faz uso frequente de palavras OOV do modelo customizado, aumentar o peso da customização pode melhorar a precisão dos resultados da transcrição. Tenha cuidado ao configurar o peso de customização. Embora um peso maior possa melhorar a precisão das frases do domínio do modelo customizado, ele também pode impactar negativamente o desempenho em frases que não são do domínio. (Mesmo que você configure o peso para 0,0, existe uma pequena probabilidade de que a transcrição possa incluir palavras customizadas devido à implementação da customização do modelo de idioma.)

Você especifica um peso de customização usando o parâmetro `customization_weight`. É possível especificar o parâmetro quando você treina um modelo de idioma customizado ou quando você o usa com uma solicitação de reconhecimento de voz.

-   Para uma solicitação de treinamento, o exemplo a seguir especifica um peso de customização igual a `0.5` com o método `POST /v1/customizations/{customization_id}/train`:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train?customization_weight=0.5"
    ```
    {: pre}

    A configuração de um peso de customização durante o treinamento salva o peso com o modelo de idioma customizado. Não é necessário passar o peso com cada solicitação de reconhecimento que usa o modelo customizado.

-   Para obter uma solicitação de reconhecimento, o exemplo a seguir especifica um peso de customização de `0.7` com o método `POST /v1/recognize`:

    ```bash
    curl -X POST -u "apikey:{apikey}"
    --header "Content-Type: audio/flac"
    --data-binary @audio-file1.flac
    "https://stream.watsonplatform.net/speech-to-text/api/v1/recognize?language_customization_id={customization_id}&customization_weight=0.7"
    ```
    {: pre}

    A configuração de um peso de customização durante o reconhecimento da fala substitui um peso que foi salvo com o modelo durante o treinamento.

## Resolução de problemas do uso de modelos de idioma customizados
{: #languageTroubleshoot}

Se você aplicar um modelo de idioma customizado ao reconhecimento de voz, mas descobrir que o serviço não parece estar usando palavras que o modelo contém, verifique os possíveis problemas a seguir:

-   Certifique-se de que esteja passando corretamente o ID de customização para a solicitação de reconhecimento, conforme mostrado nos exemplos anteriores.
-   Certifique-se de que o status do modelo customizado esteja `available`, significando que ele esteja totalmente treinado e pronto para uso. Para obter mais informações, consulte [Listando modelos de idioma customizados](/docs/services/speech-to-text?topic=speech-to-text-manageLanguageModels#listModels-language).
-   Verifique as pronúncias que foram geradas para as novas palavras para certificar-se de que elas estejam corretas. Para obter mais informações, consulte [Validando um recurso de palavras](/docs/services/speech-to-text?topic=speech-to-text-corporaWords#validateModel).
