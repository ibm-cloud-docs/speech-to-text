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
{:go: .ph data-hd-programlang='go'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:download: .download}
{:apikey: data-credential-placeholder='apikey'}
{:url: data-credential-placeholder='url'}
{:hide-dashboard: .hide-dashboard}

# Tutorial Introdução
{: #gettingStarted}

O serviço {{site.data.keyword.speechtotextfull}} transcreve áudio para texto para ativar os recursos de transcrição de voz para os aplicativos. Esse tutorial baseado em curl pode ajudá-lo a começar rapidamente com o serviço. Os exemplos mostram como chamar o método `POST /v1/recognize` do serviço para solicitar uma transcrição.
{: shortdesc}

O tutorial usa as chaves de API do {{site.data.keyword.cloud}} Identity and Access Management (IAM) para autenticação. As instâncias de serviço mais antigas podem continuar a usar o `{username}` e a `{password}` de suas credenciais de serviço existentes do Cloud Foundry para autenticação. Autentique usando a abordagem que está correta para sua instância de serviço. Para obter mais informações sobre o uso do serviço de autenticação do IAM, consulte a [atualização de serviço de 30 de outubro de 2018](/docs/services/speech-to-text/release-notes.html#October2018b) nas notas sobre a liberação.
{: important}

## Antes de iniciar
{: #before-you-begin}

- {: hide-dashboard}  Crie uma instância do serviço:
    1.  {: hide-dashboard} Acesse a página [{{site.data.keyword.speechtotextshort}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/catalog/services/speech-to-text){: new_window} no {{site.data.keyword.cloud_notm}} Catalog.
    1.  {: hide-dashboard} Inscreva-se para obter uma conta gratuita do {{site.data.keyword.cloud_notm}} ou efetue login.
    1.  {: hide-dashboard} Clique em **Criar**.
-   Copie as credenciais para autenticação em sua instância de serviço:
    1.  {: hide-dashboard} No [ painel do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/dashboard/apps){: new_window}, clique em sua instância de serviço {{site.data.keyword.speechtotextshort}} para acessar a página do painel de serviço do {{site.data.keyword.speechtotextshort}}.
    1.  Na página **Gerenciar**, clique em **Mostrar** para visualizar suas credenciais.
    1.  Copie os valores `API Key` e `URL`.
-   Certifique-se de que você tenha o comando `curl`.
    -   Os exemplos usam o comando `curl` para chamar métodos da interface de HTTP. Instale a versão para seu sistema operacional de [curl.haxx.se ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://curl.haxx.se/){: new_window}. Instale a versão que suporta o protocolo Secure Sockets Layer (SSL). Certifique-se de incluir o arquivo binário instalado em sua variável de ambiente `PATH`.

Quando você inserir um comando, substitua `{apikey}` e `{url}` por sua chave de API real e URL. Omita as chaves, que indicam um valor de variável, por meio do comando. Um valor real é semelhante ao exemplo a seguir:
{: hide-dashboard}

```bash
curl -X POST -u "apikey:L_HALhLVIksh1b73l97LSs6R_3gLo4xkujAaxm7i-b9x"
. . .
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{:pre}
{: hide-dashboard}

## Etapa 1: Transcrições de áudio sem opções
{: #transcribe}

Chame o método `POST /v1/recognize` para solicitar uma transcrição básica de um arquivo de áudio FLAC sem nenhum parâmetro de solicitação adicional.

1.  Faça download do arquivo de áudio de amostra <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a>.
1.  Emita o comando a seguir para chamar o método `/v1/recognize` do serviço para a transcrição básica sem parâmetros. O exemplo usa o cabeçalho `Content-Type` para indicar o tipo de áudio, `audio/flac`. O exemplo usa o modelo de idioma padrão, `en-US_BroadbandModel` para transcrição.
    -   {: hide-dashboard} Substitua `{apikey}` e `{url}` por sua chave de API e URL.
    -   Modifique `{path_to_file}` para especificar a localização do arquivo `audio-file.flac`.

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
              "confidence": 0.87
              "transcript": "several tornadoes touch down as a line of
severe thunderstorms swept through colorado on sunday "
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
              "confidence": 0.87
              "transcript": "several tornadoes touch down as a line
of severe thunderstorms swept through colorado on sunday "
            },
            {
              "transcript": "several tornadoes touched down as a line
of severe thunderstorms swept through colorado on sunday "
            },
            {
              "transcript": "several tornadoes touch down is a line
of severe thunderstorms swept through colorado on sunday "
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

-   Saiba mais sobre as interfaces e os SDKs que estão disponíveis para fazer solicitações de reconhecimento de voz na [Visão geral para desenvolvedores](/docs/services/speech-to-text/developer-overview.html).
-   Consulte as solicitações básicas de reconhecimento de voz para cada uma das interfaces do serviço em [Fazendo uma solicitação de reconhecimento](/docs/services/speech-to-text/basic-request.html).
-   Localize informações detalhadas sobre todos os métodos das interfaces do serviço na [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/speech-to-text){: new_window}.
