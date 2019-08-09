---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-03"

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
{:go: .ph data-hd-programlang='go'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:download: .download}
{:apikey: data-credential-placeholder='apikey'}
{:url: data-credential-placeholder='url'}
{:hide-dashboard: .hide-dashboard}

# Tutorial de introdução
{: #gettingStarted}

O serviço {{site.data.keyword.speechtotextfull}} transcreve áudio para texto para ativar os recursos de transcrição de voz para os aplicativos. Esse tutorial baseado em curl pode ajudá-lo a começar rapidamente com o serviço. Os exemplos mostram como chamar o método `POST /v1/recognize` do serviço para solicitar uma transcrição.
{: shortdesc}

## Antes de iniciar
{: #before-you-begin}

- {: hide-dashboard}  Crie uma instância do serviço:
    1.  {: hide-dashboard} Acesse a [página do {{site.data.keyword.speechtotextshort}}](https://{DomainName}/catalog/services/speech-to-text){: external} no catálogo do {{site.data.keyword.cloud_notm}}.
    1.  {: hide-dashboard} Inscreva-se para obter uma conta gratuita do {{site.data.keyword.cloud_notm}} ou efetue login.
    1.  {: hide-dashboard} Clique em **Criar**.
-   Copie as credenciais para autenticação em sua instância de serviço:
    1.  {: hide-dashboard} Na [Lista de recursos do {{site.data.keyword.cloud_notm}}](https://{DomainName}/resources){: external}, cliqu na sua instância de serviço do {{site.data.keyword.speechtotextshort}} para acessar a página do painel de serviço {{site.data.keyword.speechtotextshort}}.
    1.  Na página **Gerenciar**, clique em **Mostrar** para visualizar suas credenciais.
    1.  Copie os valores `API Key` e `URL`.

### Utilizando os exemplos de curl
{: #getting-started-curl}

Este tutorial usa o comando `curl` para chamar os métodos da interface HTTP do serviço. Certifique-se de que você tenha o comando `curl` instalado em seu sistema.

1.  Para testar se o `curl` está instalado, execute o comando a seguir na linha de comandos. Se a saída listar a versão de `curl` que suporta o Secure Sockets Layer (SSL), o tutorial estará configurado.

    ```bash
    curl -V
    ```
    {: pre}

1.  Se necessário, instale a versão do `curl` com a SSL ativada para o seu sistema operacional por meio do [curl.haxx.se](https://curl.haxx.se/){: external}.

Omita as chaves dos exemplos. Elas indicam valores de variáveis.
{: tip}

## Etapa 1: Transcrições de áudio sem opções
{: #transcribe}

Chame o método `POST /v1/recognize` para solicitar uma transcrição básica de um arquivo de áudio FLAC sem nenhum parâmetro de solicitação adicional.

1.  Faça download do arquivo de áudio de amostra <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a>.
1.  Emita o comando a seguir para chamar o método `/v1/recognize` do serviço para a transcrição básica sem parâmetros. O exemplo usa o cabeçalho `Content-Type` para indicar o tipo de áudio, `audio/flac`. O exemplo usa o modelo de idioma padrão, `en-US_BroadbandModel` para transcrição.
    -   {: hide-dashboard} Substitua `{apikey}` e `{url}` por sua chave de API e URL.
    -   Modifique `{path_to_file}` para especificar a localização do arquivo `audio-file.flac`.

    *Usuários do Windows,* substituam a barra invertida (``\`) no final de cada linha por um acento circunflexo (``^`). Certifiquem-se de que não haja espaços à direita.
    {: tip}

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize"{: url}
    ```
    {: pre}

    O serviço retorna os resultados da transcrição a seguir:

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "confidence": 0.96
              "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through Colorado on Sunday "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

## Etapa 2: Transcrever áudio com opções
{: #transcribeOptions}

Chame o método `POST /v1/recognize` para transcrever o mesmo arquivo de áudio FLAC, mas especifique dois parâmetros de transcrição.

1.  Se necessário, faça download do arquivo de áudio de amostra <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a>.
1.  Emita o comando a seguir para chamar o método `/v1/recognize` do serviço com dois parâmetros extras. Configure o parâmetro `timestamps` como `true` para indicar o início e o fim de cada palavra no fluxo de áudio. Configure o parâmetro `max_alternatives` como `3` para receber as três alternativas mais prováveis para a transcrição. O exemplo usa o cabeçalho `Content-Type` para indicar o tipo de áudio, `audio/flac` e a solicitação usa o modelo padrão, `en-US_BroadbandModel`.
    -   {: hide-dashboard} Substitua `{apikey}` e `{url}` por sua chave de API e URL.
    -   Modifique `{path_to_file}` para especificar a localização do arquivo `audio-file.flac`.

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize?timestamps=true&max_alternatives=3"{: url}
    ```
    {: pre}

    O serviço retorna os resultados a seguir, que incluem os registros de data e hora e três transcrições alternativas:

    ```javascript
    {
      "results": [
        {
          "alternatives": [
            {
              "timestamps": [
                ["several":, 1.0, 1.51],
                ["tornadoes":, 1.51, 2.15],
                ["touch":, 2.15, 2.5],
                . . .
              ]
            },
            {
              "confidence": 0.96
              "transcript": "several tornadoes touch down as a line
of severe thunderstorms swept through Colorado on Sunday "
            },
            {
              "transcript": "several tornadoes touched down as a line of
severe thunderstorms swept through Colorado on Sunday "
            },
            {
              "transcript": "several tornadoes touch down as a line
of severe thunderstorms swept through Colorado and Sunday "
            }
          ],
          "final": true
        }
      ],
      "result_index": 0
    }
    ```
    {: codeblock}

## Próximas Etapas

-   Saiba mais sobre as interfaces e os SDKs que estão disponíveis para fazer solicitações de reconhecimento de voz na [Visão geral para desenvolvedores](/docs/services/speech-to-text?topic=speech-to-text-developerOverview).
-   Consulte as solicitações básicas de reconhecimento de voz para cada uma das interfaces do serviço em [Fazendo uma solicitação de reconhecimento](/docs/services/speech-to-text?topic=speech-to-text-basic-request).
-   Localize informações detalhadas sobre todos os métodos das interfaces do serviço na [Referência de API](https://{DomainName}/apidocs/speech-to-text){: external}.
