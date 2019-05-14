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

# Trabalhando com recursos de áudio
{: #audioResources}

É possível incluir arquivos de áudio individuais ou archives que contêm múltiplos arquivos de áudio em um modelo acústico customizado. O meio recomendado para incluir recursos de áudio é incluir archives. A criação e a inclusão de um único archive é consideravelmente mais eficiente do que a inclusão de múltiplos arquivos de áudio individualmente.
{: shortdesc}

## Incluindo um recurso de áudio
{: #addAudioResource}

Use o método `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` para incluir um tipo de recurso de áudio em um modelo acústico customizado. Passe o recurso de áudio como o corpo da solicitação e inclua os parâmetros a seguir:

-   O parâmetro de caminho `customization_id` para especificar o ID de customização do modelo.
-   O parâmetro do caminho `audio_name` para especificar um nome para o recurso de áudio.
    -   Use um nome localizado que corresponda ao idioma do modelo customizado e reflita os conteúdos do recurso.
    -   Inclua um máximo de 128 caracteres no nome.
    -   Não inclua espaços, `/` (barras) ou `\` (barras invertidas) no nome.
    -   Não use o nome de um recurso de áudio que já tenha sido incluído no modelo customizado.

Ao atualizar os recursos de áudio de um modelo, deve-se treinar o modelo para que as mudanças entrem em vigor durante a transcrição. Para obter mais informações, consulte [Treinar o modelo acústico customizado](/docs/services/speech-to-text/acoustic-create.html#trainModel-acoustic).

## Incluindo um arquivo de áudio
{: #addAudioType}

Para incluir um arquivo de áudio individual em um modelo acústico customizado, especifique o formato (tipo MIME) do áudio com o cabeçalho `Content-Type`. É possível incluir áudio com qualquer formato que seja suportado para uso com solicitações de reconhecimento. Inclua os parâmetros `rate`, `channels` e `endianness` com a especificação de formatos que os requer. Para obter mais informações sobre os formatos de áudio suportados, consulte [Formatos de áudio](/docs/services/speech-to-text/audio-formats.html).

A especificação `application/octet-stream` para um formato de áudio não é suportada para recursos de áudio.
{: note}

O exemplo a seguir de [Incluir áudio no modelo acústico customizado](/docs/services/speech-to-text/acoustic-create.html#addAudio) inclui um arquivo `audio/wav`:

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @audio1.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
```
{: pre}

## Incluindo um archive
{: #addArchiveType}

O meio preferencial de incluir áudio em um modelo acústico customizado é incluir um archive que inclua múltiplos arquivos de áudio. É possível incluir os seguintes tipos de archive, especificando o tipo do archive com o cabeçalho da solicitação `Content-Type`:

-   Um arquivo **.zip** especificando `application/zip`
-   Um arquivo **.tar.gz** especificando `application/gzip`

Também pode ser necessário especificar o cabeçalho `Contained-Content-Type` dependendo do formato dos arquivos que você está incluindo:

-   Para arquivos de áudio do tipo `audio/alaw`, `audio/basic`, `audio/l16` ou `audio/mulaw`, deve-se usar o cabeçalho `Contained-Content-Type` para especificar o formato dos arquivos de áudio. Inclua os parâmetros `rate`, `channels` e `endianness`, onde necessário. Nesse caso, todos os arquivos de áudio contidos no archive devem ter o mesmo formato de áudio.
-   Para arquivos de áudio de todos os outros tipos, é possível omitir o cabeçalho `Contained-Content-Type`. Nesse caso, os arquivos de áudio contidos no archive podem ter qualquer um dos formatos não listados no marcador anterior. Eles não precisam ter o mesmo formato.

Não use o cabeçalho `Contained-Content-Type` ao incluir um recurso de tipo áudio.
{: note}

O nome de um arquivo de áudio que é integrado em um recurso de tipo archive deve atender às mesmas restrições de nomenclatura que o parâmetro `audio_name`. Além disso, não use o nome de um arquivo de áudio que já tenha sido incluído no modelo customizado como parte de um recurso de tipo archive.

O exemplo a seguir de [Incluir áudio no modelo acústico customizado](/docs/services/speech-to-text/acoustic-create.html#addAudio) inclui um arquivo `application/zip` que contém arquivos de áudio no formato `audio/l16` que são amostrados a 16 kHz:

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/zip"
--header "Contained-Content-Type: audio/l16;rate=16000"
--data-binary @audio2.zip
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
```
{: pre}

## Diretrizes para incluir recursos de áudio
{: #audioGuidelines}

A melhoria na precisão do reconhecimento que você pode esperar do uso de um modelo acústico customizado depende de uma série de fatores. Esses fatores incluem a quantidade de dados de áudio que o modelo acústico customizado contém e quão semelhantes esses dados são para o áudio que está sendo transcrito. O aprimoramento também depende de se o modelo acústico customizado é treinado com um modelo de idioma customizado correspondente.

Siga estas diretrizes ao incluir recursos de áudio em um modelo acústico customizado:

-   Inclua pelo menos 10 minutos e não mais que 200 horas de áudio para um modelo acústico customizado. O áudio deve incluir fala, não silêncio.

    A qualidade do áudio faz a diferença quando você está determinando o quanto incluir. Quanto melhor o áudio do modelo refletir as características do áudio que deve ser reconhecido, melhor a qualidade do modelo customizado para reconhecimento de voz. Se o áudio for de boa qualidade, a inclusão de mais poderá melhorar a precisão da transcrição. Mas incluir até cinco a dez horas de áudio de boa qualidade pode fazer uma diferença positiva.
-   Inclua recursos de áudio que não sejam maiores que 100 MB. Todos os recursos de tipo áudio e archive são limitados a um tamanho máximo de 100 MB.

    Para maximizar a quantidade de áudio que pode ser incluída com um único recurso, considere usar um formato de áudio que ofereça a compactação. Para obter mais informações, consulte [Limites de dados e compactação](/docs/services/speech-to-text/audio-formats.html#limits).
-   Inclua conteúdo de áudio que reflita as condições acústicas do canal do áudio que você planeja transcrever. Por exemplo, se seu aplicativo lidar com áudio que tenha ruído de plano de fundo de um veículo em movimento, use o mesmo tipo de dados para construir o modelo customizado.
-   Certifique-se de que a taxa de amostragem de um arquivo de áudio corresponda à taxa de amostragem do modelo base para o modelo acústico customizado:
    -   Para modelos de banda larga, a taxa de amostragem deve ser de pelo menos 16 kHz (16.000 amostras por segundo).
    -   Para modelos de banda estreita, a taxa de amostragem deve ser de pelo menos 8 kHz (8000 amostras por segundo).

    Se a taxa de amostragem do áudio for maior que a taxa de amostragem mínima necessária, o serviço baixa a resolução do áudio para a taxa apropriada. Se a taxa de amostragem do áudio for menor que a taxa mínima necessária, o serviço rotulará o arquivo de áudio como `invalid`. Se qualquer arquivo de áudio contido em um archive for inválido, o serviço considerará o archive inteiro inválido.
-   Criar um modelo de idioma customizado para usar com seu modelo acústico customizado nos casos a seguir:
    -   Se seu áudio for menor que uma hora de duração, crie um modelo de idioma customizado com base nas transcrições do áudio para obter os melhores resultados.
    -   Se seu áudio for específico do domínio e contiver palavras exclusivas que não estão localizadas no vocabulário base do serviço, use a customização do modelo de idioma para expandir o vocabulário base do serviço. A customização do modelo acústico sozinha não pode produzir essas palavras durante a transcrição.

    Para obter mais informações, consulte [Usando modelos acústicos e de idioma customizados juntos](/docs/services/speech-to-text/acoustic-both.html).
