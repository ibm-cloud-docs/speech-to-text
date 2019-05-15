---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-15"

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

# Formatos de áudio
{: #audio-formats}

O serviço {{site.data.keyword.speechtotextfull}} pode extrair a fala do áudio em muitos formatos diferentes.

-   Se você não estiver familiarizado com o áudio e como ele é descrito e especificado, inicie com [Características de áudio e terminologia](#terminology) para ajudá-lo a iniciar.
-   Se você já entende como usar áudio, acesse [Formatos de áudio suportados](#formats) para obter informações detalhadas sobre os formatos suportados pelo serviço.

As seções finais, [Limites de dados e compactação](#limits), [Conversão de áudio](#conversion)e [Dicas para reconhecimento de voz melhorado](#audioTips), podem ajudar a obter o máximo de seu uso do serviço.

## Características de áudio e terminologia
{: #terminology}

A terminologia a seguir é utilizada para descrever as características de dados de áudio para os diferentes formatos.

### Taxa de amostragem
{: #samplingRate}

*Taxa de amostragem* (ou frequência de amostragem) é o número de amostras de áudio que são obtidas por segundo. A taxa de amostragem é medida em Hertz (Hz) ou em kilohertz (kHz). Por exemplo, uma taxa de 16.000 amostras por segundo é igual a 16.000 Hz (ou 16 kHz). Com o serviço {{site.data.keyword.speechtotextshort}}, você especifica um modelo para indicar a taxa de amostragem de seu áudio:

-   Os modelos de *banda larga* são usados para áudio amostrado em pelo menos de 16 kHz, que o {{site.data.keyword.IBM}} recomenda para aplicativos responsivos, em tempo real (por exemplo, para aplicativos em fala em tempo real).
-   Os modelos de *banda estreita* são usados para áudio amostrado em pelo menos 8 kHz, que é a taxa geralmente usada para áudio telefônico.

O serviço suporta banda larga e áudio de banda estreita para a maioria dos idiomas e formatos. Ele ajustará automaticamente a taxa de amostragem de seu áudio para corresponder ao modelo que você especificar antes que ele reconheça a fala.

-   Para modelos de banda larga, o serviço converte áudio registrado em taxas de amostragem mais altas para 16 kHz.
-   Para modelos de banda estreita, ele converte o áudio para 8 kHz.

Em teoria, é possível enviar áudio de 44 kHz com um modelo de banda larga ou de banda estreita, mas isso aumenta desnecessariamente o tamanho do áudio. Para maximizar a quantia de áudio que você pode enviar, corresponda a taxa de amostragem de seu áudio ao modelo que você usa. O serviço não aceita áudio amostrado a uma taxa menor que a taxa de amostragem intrínseca do modelo.

#### Notas sobre formatos de áudio
{: #samplingRateNotes}

-   Para os formatos `audio/alaw`, `audio/l16` e `audio/mulaw`, deve-se especificar a taxa de seu áudio.
-   Para os formatos `audio/basic` e `audio/g729`, o serviço suporta apenas áudio de banda estreita.

#### Mais informações
{: #samplingRateMore}

-   Para obter mais informações sobre taxas de amostragem, consulte [en.wikipedia.org/wiki/Sampling ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Sampling){: new_window}. Selecione *Amostragem (processamento de sinal)*.
-   Para obter mais informações sobre os modelos que o serviço oferece para cada idioma suportado, consulte [Idiomas e modelos](/docs/services/speech-to-text/models.html).

### Taxa de bits
{: #bitRate}

*Taxa de bits* é o número de bits de dados enviados por segundo. A taxa de bits para um fluxo de áudio é medida em kilobits por segundo (kbps). A taxa de bits é calculada por meio da taxa de amostragem e do número de bits armazenados por amostra. Para reconhecimento de voz, a {{site.data.keyword.IBM}} recomenda que você registre 16 bits por amostra para seu áudio.

Por exemplo, o áudio que usa uma taxa de amostragem de banda larga de 16 kHz e 16 bits por amostra tem uma taxa de bits de 256 kbps: `(16.000 * 16) / 1000`.

#### Mais informações
{: #bitRateMore}

-   Para obter mais informações sobre taxas de bits, consulte [en.wikipedia.org/wiki/Bit_rate ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Bit_rate){: new_window}.
-   Para uma discussão geral sobre taxas de amostragem e taxas de bits, consulte [O que são taxas de bits? ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://www.richardfarrar.com/what-are-bit-rates/){: new_window} e [Escolhendo taxas de bits para podcasts ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://www.richardfarrar.com/choosing-bit-rates-for-podcasts/){: new_window}.

### Compactação
{: #compression}

A*compactação* é usada por muitos formatos de áudio para reduzir o tamanho dos dados de áudio. A compactação reduz o número de bits armazenados por amostra e, portanto, a taxa de bits. Alguns formatos não usam compactação, mas a maioria dos tipos usa um dos tipos básicos de compactação:

-   A compactação*sem perdas* reduz o tamanho do áudio sem perda de qualidade, mas a proporção de compactação é geralmente pequena.
-   A compactação*com perdas* reduz o tamanho do áudio por até 10 vezes, mas alguns dados e alguma qualidade são irrecuperavelmente perdidos na compactação.

Com o serviço {{site.data.keyword.speechtotextshort}}, é possível usar com segurança a compactação com perdas para maximizar a quantidade de áudio que é possível enviar para o serviço com uma solicitação de reconhecimento. Como o intervalo dinâmico da voz humana é mais limitado do que a música, por exemplo, a fala pode acomodar uma taxa de bits que é muito menor que os outros tipos de áudio. Para reconhecimento de voz, a {{site.data.keyword.IBM}} recomenda que você use 16 bits por amostra para seu áudio e empregue um formato que compacte os dados de áudio.

#### Notas sobre formatos de áudio
{: #compressionNotes}

-   Os formatos `audio/ogg` e `audio/webm` são contêineres cuja compactação depende do codec que você usa para codificar os dados: Opus ou Vorbis.
-   O formato `audio/wav` é um contêiner que pode incluir dados descompactados, sem perdas ou com perdas.

#### Mais informações
{: #compressionMore}

-   Para obter mais informações sobre a compactação de áudio, consulte [en.wikipedia.org/wiki/Data_compression#Audio ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Data_compression#Audio){: new_window}.
-   Para obter mais informações sobre como usar a compactação de dados para aumentar a quantia de áudio que é possível enviar com uma solicitação, consulte [Limites de dados e compactação](#limits).

### Canais
{: #channels}

*Canais* indicam o número de fluxos do áudio gravado:

-   O áudio *Monaural* (ou mono) tem somente um único canal.
-   O áudio*estereofônico* (ou estéreo) normalmente tem dois canais.

O serviço {{site.data.keyword.speechtotextshort}} aceita áudio com um máximo de 16 canais. Como ele usa somente um único canal para reconhecimento de voz, o serviço faz efetua downmix do áudio com múltiplos canais para um canal mono durante a transcodificação.

#### Notas sobre formatos de áudio
{: #channelsNotes}

-   Para o formato `audio/l16`, deve-se especificar o número de canais se seu áudio tiver mais de um canal.
-   Para o formato `audio/wav`, o serviço aceita áudio com um máximo de nove canais.

#### Mais informações
{: #channelsMore}

-   Para obter mais informações sobre os canais de áudio, consulte [en.wikipedia.org/wiki/Audio_signal ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Audio_signal){: new_window}.

### Ordenação
{: #endianness}

*Ordenação* indica como os bytes de dados são organizados pela arquitetura de computador subjacente:

-   *Big endian* ordena os dados pelo bit mais significativo.
-   *Little endian* ordena os dados pelo bit menos significativo.

O serviço {{site.data.keyword.speechtotextshort}} detecta automaticamente a ordenação do áudio recebido.

#### Notas sobre formatos de áudio
{: #endiannessNotes}

-   Para o formato `audio/l16`, é possível especificar a ordenação para desativar a detecção automática, se necessário.

#### Mais informações
{: #endiannessMore}

-   Para obter mais informações sobre a ordenação, consulte [en.wikipedia.org/wiki/Endianness ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Endianness){: new_window}.

### Frequência de áudio
{: #frequency}

*Frequência de áudio* refere-se ao intervalo de frequências audíveis no áudio. A frequência audível padrão para humanos é geralmente aceita como 20 a 20.000 Hz. É possível usar a análise espectrográfica para produzir um espectrograma que revela o conteúdo de frequência de seu áudio.

A taxa de amostragem que é aplicada ao áudio é tipicamente duas vezes a frequência máxima do áudio. Por exemplo, uma taxa de amostragem de 16 kHz significa que a frequência máxima do sinal de áudio amostrado é de 8 kHz. Os modelos do serviço são criados com isso em mente.

-   Os modelos de banda estreita são construídos com áudio que é amostrado em 8 kHz. Os modelos de banda estreita esperam localizar informações em um intervalo que seja menor que ou igual a 4 kHz.
-   Os modelos de banda larga são construídos com áudio que é amostrado a 16 kHz. Os modelos de banda larga esperam localizar informações no intervalo de 4 a 8 kHz.

Os dados de treinamento para os modelos são derivados de diferentes canais (telefonia para modelos de banda estreita). Os modelos refletem as características dos canais nos quais eles foram treinados.

#### Upsampling
{: #upsampling}

*Upsampling* aumenta a taxa de amostragem do áudio, mas não apresenta novas informações para o áudio. Produz uma aproximação do sinal de áudio que teria sido obtida por meio da amostragem do áudio a uma taxa mais alta. Aumenta o tamanho dos dados de áudio.

As informações em áudio que são originalmente amostradas em uma frequência de banda estreita são limitadas ao intervalo de 0 a 4 kHz. Efetuar upsampling de áudio de banda estreita para uma taxa de amostragem mais alta provavelmente não melhorará a precisão do reconhecimento de voz. Se você efetuar upsampling do áudio de banda estreita, ele terá informações ausentes no intervalo que os modelos de banda larga esperam. Além disso, as informações que estão localizadas no intervalo esperado de uma amostra de banda estreita são qualitativamente diferentes das informações que são localizadas no mesmo intervalo de uma amostra de banda larga. Portanto, o upsampling resulta na degradação da precisão do reconhecimento.

Para uma taxa de amostragem de banda larga de 16 kHz, espera-se que a frequência máxima presente no sinal de áudio amostrado seja 8 kHz. Portanto, deve-se filtrar o sinal original em 8 kHz antes de amostrá-lo com uma taxa de 16 kHz. Caso contrário, a degradação ocorre devido ao fenômeno conhecido como *aliasing*. Para entender por que, consulte [Frequência do Nyquist ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Nyquist_frequency){: new_window}.

Uma comparação útil pode ser imaginar a visualização de uma fita VHS em uma grande tela plana HDTV. A imagem é borrada porque a reprodução da fita em um dispositivo de alta definição não pode realmente incluir novas informações no fluxo. Isso simplesmente torna o formato compatível com o dispositivo melhor. O mesmo se aplica ao upsampling de áudio.

#### Downsampling
{: #downsampling}

*Downsampling* diminui a taxa de amostragem do áudio. Produz uma aproximação do sinal de áudio que teria sido obtida por meio da amostragem do áudio a uma taxa mais baixa. O downsampling não remove nenhuma informação do sinal de áudio, mas reduz o tamanho dos dados de áudio.

Efetuar downsampling de seu áudio pode ser efetivo em alguns casos. Por exemplo, se a taxa de amostragem de seu áudio for maior que 8 kHz *e* um exame espectrográfico revelar nenhum conteúdo de frequência maior que 4 kHz, considere efetuar downsampling do áudio para 8 kHz.

#### Mais informações
{: #frequencyMore}

-   Para obter mais informações sobre a frequência de áudio, consulte [https://en.wikipedia.org/wiki/Audio_frequency ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Audio_frequency){: new_window}.
-   Para obter mais informações sobre upsampling, consulte [https://en.wikipedia.org/wiki/Upsampling ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Upsampling){: new_window}.
-   Para obter mais informações sobre downsampling, consulte [https://en.wikipedia.org/wiki/Downsampling ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Downsampling_%28signal_processing%29){: new_window}.

## Formatos de áudio suportados
{: #formats}

A Tabela 1 fornece um resumo dos formatos de áudio que o serviço suporta.

-   *Formato de áudio e compactação* identifica cada formato e indica sua compactação suportada. É possível enviar um máximo de 100 MB de áudio para o serviço com uma solicitação síncrona de HTTP ou WebSocket. Usando um formato que suporta compactação, é possível reduzir o tamanho do seu áudio para maximizar a quantidade de dados que podem ser transmitidos para o serviço. Para obter mais informações, consulte [Limites de dados e compactação](#limits).
-   *Especificação do tipo de conteúdo* indica se deve-se usar o cabeçalho `Content-Type` ou parâmetro equivalente para especificar o formato (tipo MIME) do áudio que você envia para o serviço. É possível especificar o formato de áudio para qualquer solicitação, mas isso nem sempre é necessário:
    -   Para a maioria dos formatos, o tipo de conteúdo é opcional. É possível omitir o tipo de conteúdo ou especificar `application/octet-stream` para fazer com que o serviço detecte automaticamente o formato.
    -   Para outros, o tipo de conteúdo é necessário. Esses formatos não fornecem as informações, como a taxa de amostragem, que o serviço precisa para detectar automaticamente seu formato.
-   As colunas finais identificam *Parâmetros necessários* adicionais e *Parâmetros opcionais* para cada formato. As seções a seguir fornecem mais informações sobre esses parâmetros.

<table>
  <caption>Tabela 1. Resumo de formatos de áudio suportados</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom">
      Formato de áudio<br>e compactação
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Especificação<br/>do tipo de conteúdo
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Parâmetros<br/>necessários
    </th>
    <th style="text-align:center; vertical-align:bottom">
      Parâmetros<br/>opcionais
    </th>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/alaw](#alaw)<br/>Com perdas
    </td>
    <td style="text-align:center">
      Necessárias
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      Nenhum
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/basic](#basic)<br/>Com perdas
    </td>
    <td style="text-align:center">
      Necessárias
    </td>
    <td style="text-align:center">
      Nenhum
    </td>
    <td style="text-align:center">
      Nenhum
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/flac](#flac)<br/>Sem perdas
    </td>
    <td style="text-align:center">
      Opcional
    </td>
    <td style="text-align:center">
      Nenhum
    </td>
    <td style="text-align:center">
      Nenhum
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/g729](#g729)<br/>Com perdas
    </td>
    <td style="text-align:center">
      Opcional
    </td>
    <td style="text-align:center">
      Nenhum
    </td>
    <td style="text-align:center">
      Nenhum
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/l16](#l16)<br/>Nenhum
    </td>
    <td style="text-align:center">
      Necessárias
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      <code>channels={integer}</code><br/>
      <code>endianness=big-endian</code><br/>
      <code>endianness=little-endian</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/mp3](#mp3)<br/>
      [audio/mpeg](#mp3)<br/>Com perdas
    </td>
    <td style="text-align:center">
      Opcional
    </td>
    <td style="text-align:center">
      Nenhum
    </td>
    <td style="text-align:center">
      Nenhum
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/multa](#mulaw)<br/>Com perdas
    </td>
    <td style="text-align:center">
      Necessárias
    </td>
    <td style="text-align:center">
      <code>rate={integer}</code>
    </td>
    <td style="text-align:center">
      Nenhum
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/ogg](#ogg)<br/>Com perdas
    </td>
    <td style="text-align:center">
      Opcional
    </td>
    <td style="text-align:center">
      Nenhum
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/wav](#wav)<br/>Nenhum, sem perdas<br/>ou com perdas
    </td>
    <td style="text-align:center">
      Opcional
    </td>
    <td style="text-align:center">
      Nenhum
    </td>
    <td style="text-align:center">
      Nenhum
    </td>
  </tr>
  <tr>
    <td style="text-align:left">
      [audio/webm](#webm)<br/>Com perdas
    </td>
    <td style="text-align:center">
      Opcional
    </td>
    <td style="text-align:center">
      Nenhum
    </td>
    <td style="text-align:center">
      <code>codecs=opus</code><br/><code>codecs=vorbis</code>
    </td>
  </tr>
</table>

Quando você usa o comando `curl` para fazer uma solicitação de reconhecimento de voz com as interfaces HTTP, deve-se especificar o formato de áudio com o cabeçalho `Content-Type`, especificar `"Content-Type: application/octet-stream"`ou especificar `"Content-Type:"`. Se você omitir o cabeçalho completamente, `curl` usará um valor padrão de `application/x-www-form-urlencoded`.
{: important}

### Formato audio/alaw
{: #alaw}

*A-law* (`audio/alaw`) é um formato de áudio de canal único com perdas. Ele usa um algoritmo que é semelhante ao algoritmo u-law aplicado pelos formatos `audio/basic` e `audio/mulaw`, embora o algoritmo A-law produza diferentes características de sinal. Quando você usa esse formato, o serviço requer um parâmetro extra na especificação de formato.

<table>
  <caption>Tabela 2. Parâmetro para o formato `audio/alaw`</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Parâmetro
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Descrição
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Necessário</em>
    </td>
    <td>
      Um número inteiro que especifica a taxa de amostragem na qual o áudio é capturado. Por exemplo, especifique o seguinte parâmetro para dados de áudio capturados em 8 kHz:<br/><br/>
      <code>audio/alaw;rate=8000</code>
    </td>
  </tr>
</table>

Para obter mais informações, consulte [en.wikipedia.org/wiki/A-law_algorithm ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/A-law_algorithm){: new_window}.

### Formato audio/basic
{: #basic}

*Áudio básico* (`audio/basic`) é um formato de áudio de canal único com perdas que é codificado usando dados de 8 bits u-law (ou mu-law) que são amostrados em 8 kHz. Esse formato fornece um denominador comum mais baixo para indicar o tipo de mídia de áudio. O serviço suporta o uso de arquivos no formato `audio/basic` apenas com modelos de banda estreita.

Para obter mais informações, consulte o Internet Engineering Task Force (IETF) [Request for Comment (RFC) 2046 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://tools.ietf.org/html/rfc2046){: new_window} e [iana.org/assignments/media-types/audio/basic ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://www.iana.org/assignments/media-types/audio/basic){: new_window}.

### Formato audio/flac
{: #flac}

*Free Lossless Audio Codec (FLAC)* (`audio/flac`) é um formato de áudio sem perdas. Para obter mais informações, consulte [en.wikipedia.org/wiki/FLAC ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/FLAC){: new_window}.

### Formato audio/g729
{: #g729}

*G.729* (`audio/g729`) é um formato de áudio com perdas que suporta dados que são codificados a 8 kHz. O serviço suporta apenas G.729 Anexo D, não o anexo J. O serviço suporta o uso de arquivos no formato `audio/g729` somente com modelos de banda estreita. Para obter mais informações, consulte [en.wikipedia.org/wiki/G.729 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/G.729){: new_window}.

### Formato audio/l16
{: #l16}

*Linear 16-bit Pulse-Code Modulation (PCM)* (`audio/l16`) é um formato de áudio não compactado. Use esse formato para passar um arquivo PCM bruto. O áudio PCM linear também pode ser transportado dentro de um arquivo Waveform Audio File Format (WAV) do contêiner. Quando você usa o formato `audio/l16`, o serviço aceita parâmetros extras necessários e opcionais na especificação de formato.

<table>
  <caption>Tabela 3. Parâmetros para o formato `audio/l16`</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Parâmetro
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Descrição
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Necessário</em>
    </td>
    <td>
      Um número inteiro que especifica a taxa de amostragem na qual o áudio é capturado. Por exemplo, especifique o seguinte parâmetro para dados de áudio capturados a 16 kHz:<br/><br/>
      <code>audio/l16;rate=16000</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>channels</code><br/><em>Opcional</em>
    </td>
    <td>
      Por padrão, o serviço trata o áudio como se ele tivesse um único canal.
      <em>Se o áudio tiver mais de um canal</em>, você deverá especificar um número inteiro que identifica o número de canais. Por exemplo, especifique o parâmetro a seguir para os dados de áudio de dois canais capturado em 16 kHz:<br/><br/>
      <code>audio/l16;rate=16000;channels=2</code><br/><br/>
      O serviço aceita 16 canais, no máximo. Ele efetua downmix do áudio para um canal durante a transcodificação.
    </td>
  </tr>
  <tr>
    <td>
      <code>endianness</code><br/><em>Opcional</em>
    </td>
    <td>
      Por padrão, o serviço detecta automaticamente a ordenação do áudio recebido. Mas, às vezes, sua detecção automática pode falhar e descartar a conexão para áudio curto no formato `audio/l16`. Especificar a ordenação desativa a detecção automática. Especifique <code>big-endian</code> ou <code>little-endian</code>. Por exemplo, especifique o parâmetro a seguir para dados de áudio capturados a 16 kHz em formato little-endian:<br/><br/>
      <code>audio/l16;rate=16000;endianness=little-endian</code><br/><br/>
      Seção 5.1 da
      <a target="_blank" href="https://tools.ietf.org/html/rfc2045#section-5.1">Request for Comment (RFC) 2045 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")</a>
      especifica o formato big-endian para os dados <code>audio/l16</code>, mas
      muitas pessoas usam o formato little endian.
    </td>
  </tr>
</table>

Para obter mais informações, consulte o IETF [Request for Comment (RFC) 2586 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://tools.ietf.org/html/rfc2586){: new_window} e [en.wikipedia.org/wiki/Pulse-code_modulation ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Pulse-code_modulation){: new_window}.

### Formatos audio/mp3 e audio/mpeg
{: #mp3}

*MP3* (`audio/mp3`) ou *Motion Picture Experts Group (MPEG)* (`audio/mpeg`) é um formato de áudio com perdas. (MP3 e MPEG referem-se ao mesmo formato.) Para obter mais informações, consulte [en.wikipedia.org/wiki/MP3 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/MP3){: new_window}.

### Formato audio/mulaw
{: #mulaw}

*Mu-law* (`audio/mulaw`) é um formato de áudio de canal único com perdas. Os dados são codificados usando o algoritmo u-law (ou mu-law). O formato `audio/basic` é um formato equivalente que é sempre amostrado em 8 kHz. Quando você usa esse formato, o serviço requer um parâmetro extra na especificação de formato.

<table>
  <caption>Tabela 4. Parâmetro para o formato `audio/mulaw`</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 20%">
      Parâmetro
    </th>
    <th style="text-align:left; vertical-align:bottom; width 80%">
      Descrição
    </th>
  </tr>
  <tr>
    <td>
      <code>rate</code><br/><em>Necessário</em>
    </td>
    <td>
      Um número inteiro que especifica a taxa de amostragem na qual o áudio é capturado. Por exemplo, especifique o seguinte parâmetro para dados de áudio capturados em 8 kHz:<br/><br/>
      <code>audio/mulaw;rate=8000</code>
    </td>
  </tr>
</table>

Para obter mais informações, consulte [en.wikipedia.org/wiki/M-law_algorithm ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/M-law_algorithm){: new_window}.

### Formato audio/ogg
{: #ogg}

*Ogg* (`audio/ogg`) é um formato de contêiner aberto que é mantido pela Xiph.org Foundation ([xiph.org/ogg ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.xiph.org/ogg){: new_window}). É possível usar fluxos de áudio que são compactados com os seguintes codecs com perdas:

-   *Opus* (`audio/ogg;codecs=opus`). Para obter mais informações, consulte [opus-codec.org ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.opus-codec.org/){: new_window} e [ en.wikipedia.org/wiki/Opus (formato de áudio) ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Opus){: new_window}. Na página *Opus (formato de áudio)*, procure especialmente na seção *Contêineres*.
-   *Vorbis* (`audio/ogg;codecs=vorbis`). Para obter mais informações, consulte [xiph.org/vorbis ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://xiph.org/vorbis/){: new_window} e [en.wikipedia.org/wiki/Vorbis ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Vorbis){: new_window}.

Se você omitir o codec, o serviço o detectará automaticamente por meio do áudio de entrada. Opus é o codec preferencial; ele é padronizado pelo Internet Engineering Task Force (IETF) como [Request for Comment (RFC) 6716 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://tools.ietf.org/html/rfc6716){: new_window}.

### Formato audio/wav
{: #wav}

*Waveform Audio File Format (WAV)* (`audio/wav`) é um formato de contêiner frequentemente usado para fluxos de áudio descompactados, mas pode conter áudio compactado também. O serviço suporta áudio WAV que usa qualquer codificação. Ele aceita áudio WAV com um máximo de nove canais (devido a uma limitação de FFmpeg).

Para obter mais informações sobre o formato WAV, consulte [en.wikipedia.org/wiki/WAV ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/WAV){: new_window}. Para obter mais informações sobre como reduzir o tamanho do áudio do WAV, convertendo-o para o codec do Opus, consulte [Convertendo em audio/ogg com o codec do Opus](#conversionOgg).

### Formato audio/webm
{: #webm}

*Web Midia (WebM)* (`audio/webm`) é um formato de contêiner aberto mantido pelo projeto WebM ([webmproject.org ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.webmproject.org/){: new_window}). É possível usar fluxos de áudio que são compactados com os seguintes codecs com perdas:

-   *Opus* (`audio/webm;codecs=opus`). Para obter mais informações, consulte [opus-codec.org ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.opus-codec.org/){: new_window} e [ en.wikipedia.org/wiki/Opus (formato de áudio) ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Opus){: new_window}. Na página *Opus (formato de áudio)*, procure especialmente na seção *Contêineres*.
-   *Vorbis* (`audio/webm;codecs=vorbis`). Para obter mais informações, consulte [xiph.org/vorbis ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://xiph.org/vorbis/){: new_window} e [en.wikipedia.org/wiki/Vorbis ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Vorbis){: new_window}.

Se você omitir o codec, o serviço o detectará automaticamente por meio do áudio de entrada.

Para o código JavaScript que mostra como capturar áudio de um microfone em um navegador Chrome e codificá-lo em um fluxo de dados do WebM, consulte [jsbin.com/hedujihuqo/edit?js, console ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://jsbin.com/hedujihuqo/edit?js,console){: new_window}. O código não envia o áudio capturado para o serviço.
{: tip}

## Limites de dados e compactação
{: #limits}

O serviço aceita um máximo de 100 MB de dados de áudio para a transcrição com uma solicitação síncrona de HTTP ou WebSocket. Quando você reconhecer longos fluxos de áudio contínuos ou arquivos grandes, execute as etapas a seguir para assegurar que seu áudio não exceda o limite de 100 MB de dados:

-   Use uma taxa de amostragem de até 16 kHz (para modelos de banda larga) ou 8 kHz (para modelos de banda estreita) e use 16 bits por amostra. O serviço converte áudio gravado em uma taxa de amostragem que é mais alta que o modelo de destino (16 kHz ou 8 kHz) para a taxa do modelo. Portanto, as frequências maiores não resultam em uma precisão de reconhecimento aprimorada, mas aumentam o tamanho do fluxo de áudio.
-   Codifique seu áudio em um formato que ofereça compactação de dados. Ao codificar seus dados com mais eficiência, é possível enviar muito mais áudio sem exceder o limite de 100 MB. Formatos de áudio como `audio/ogg` e `audio/mp3` reduzem significativamente o tamanho de seu fluxo de áudio. É possível usar esses formatos para enviar quantidades maiores de áudio com uma única solicitação.

Se você compacta o áudio muito severamente com o formato `audio/ogg`, é possível perder alguma precisão de reconhecimento. Para segurança, retenha uma taxa de bits de pelo menos 24 kbps para seu áudio.

### Comparando tamanhos de áudio aproximados
{: #compareSizes}

Considere o tamanho aproximado do fluxo de dados que resulta de 2 horas da transmissão de fala contínua que é amostrada em 16 kHz e em 16 bits por amostra:

-   Se os dados forem codificados com o formato `audio/wav`, o fluxo de duas horas terá um tamanho de 230 MB, bem além do limite de 100 MB do serviço.
-   Se os dados forem codificados no formato `audio/ogg`, o fluxo de duas horas terá um tamanho de apenas 23 MB, bem abaixo do limite do serviço.

A tabela a seguir aproxima a duração máxima de áudio que pode ser enviada para reconhecimento de voz com uma solicitação síncrona de HTTP ou WebSocket em formatos diferentes. A duração considera o limite de serviço de 100 MB. Os valores reais podem variar dependendo da complexidade do áudio e da taxa de compactação atingida.

<table style="width:75%">
  <caption>Tabela 5. Duração máxima do áudio em diferentes formatos</caption>
  <tr>
    <th style="text-align:left; vertical-align:bottom; width 50%">
      Formato de áudio
    </th>
    <th style="text-align:center; vertical-align:bottom; width 50%">
      Duração máxima do áudio (aproximado)
    </th>
  </tr>
  <tr>
    <td><code>audio/wav</code></td>
    <td style="text-align:center">55 minutos</td>
  </tr>
  <tr>
    <td><code>audio/flac</code></td>
    <td style="text-align:center">1 hora e 40 minutos</td>
  </tr>
  <tr>
    <td><code>audio/mp3</code></td>
    <td style="text-align:center">3 horas e 20 minutos</td>
  </tr>
  <tr>
    <td><code>audio/ogg</code></td>
    <td style="text-align:center">8 horas e 40 minutos</td>
  </tr>
</table>

Os formatos `audio/ogg;codecs=opus` e `audio/webm;codecs=opus` são geralmente equivalentes e seus tamanhos são quase idênticos. Internamente, eles usam o mesmo codec. Apenas o formato do contêiner é diferente.

## Conversão de áudio
{: #conversion}

É possível usar várias ferramentas para converter o áudio em um formato diferente. As ferramentas podem ser úteis quando seu áudio está em um formato que não é suportado pelo serviço ou que está em um formato descompactado ou sem perda. No último caso, é possível converter o áudio em um formato com perdas para reduzir seu tamanho.

As ferramentas freeware a seguir estão disponíveis para converter seu áudio de um formato para outro:

-   Sound eXchange (SoX) ([sox.sourceforge.net ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://sox.sourceforge.net){: new_window})
-   FFmpeg ([ffmpeg.org ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ffmpeg.org){: new_window})
-   Audacity&reg; ([audacityteam.org ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://www.audacityteam.org/){: new_window})
-   Para o formato Ogg com o codec do Opus, **opus-tools** ([opus-codec.org/downloads/ ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://opus-codec.org/downloads/){: new_window})

Essas ferramentas oferecem suporte de plataforma cruzada para múltiplos formatos de áudio. Além disso, é possível usar muitas das ferramentas para reproduzir seu áudio. Não use as ferramentas para violar leis de copyright aplicáveis.

### Convertendo para audio/ogg com o codec do Opus
{: #conversionOgg}

O [**opus-tools** ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://opus-codec.org/downloads/){: new_window} inclui três utilitários de linha de comandos para trabalhar com áudio Ogg no codec do Opus:

-   O utilitário [**opusenc** ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusenc.html){: new_window} codifica áudio do WAV, do FLAC e de outros formatos para Ogg com o codec do Opus. A página mostra como compactar fluxos de áudio. A compactação é útil para passar o áudio em tempo real para o serviço.
-   O utilitário [**opusdec** ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusdec.html){: new_window} decodifica áudio do codec do Opus para arquivos WAV PCM descompactados.
-   O utilitário [**opusinfo**![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mf4.xiph.org/jenkins/view/opus/job/opus-tools/ws/man/opusinfo.html){: new_window} fornece informações sobre a verificação de validade para arquivos Opus.

Muitos usuários enviam arquivos WAV para reconhecimento de voz. Com o limite de dados de 100 MB do serviço para solicitações síncronas de HTTP e do WebSocket, o formato WAV reduz a quantia de áudio que pode ser reconhecida com uma única solicitação. Usar o comando **opusenc** para converter o áudio no formato `audio/ogg:codecs=opus` preferencial pode aumentar grandemente a quantia de áudio que é possível enviar com uma solicitação de reconhecimento.

Por exemplo, considere um arquivo WAV de banda larga descompactado (16 kHz) (**input.wav**) que usa 16 bits por amostra para uma taxa de bits de 256 kbps. O comando a seguir converte o áudio em um arquivo (**output.opus**) que usa o codec do Opus:

```bash
opusenc input.wav output.opus
```
{: pre}

A conversão compacta o áudio por um fator de quatro e produz um arquivo de saída com uma taxa de bits de 64 kbps. No entanto, de acordo com as [Configurações recomendadas do Opus ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://wiki.xiph.org/Opus_Recommended_Settings){: new_window}, é possível reduzir com segurança a taxa de bits para 24 kbps e ainda reter uma banda completa para áudio de fala. Essa redução compacta o áudio por um fator de 10. O comando a seguir usa a opção `--bitrate` para produzir um arquivo de saída com uma taxa de bits de 24 kbps:

```bash
opusenc --bitrate 24 input.wav output.opus
```
{: pre}

A compactação com o utilitário **opusenc** é rápida. A compactação acontece a uma taxa que é aproximadamente 100 vezes mais rápida do que o tempo real. Quando ele é concluído, o comando grava a saída no console que fornece detalhes completos sobre seu tempo de execução e os dados de áudio resultantes.

## Dicas para reconhecimento de voz melhorado
{: #audioTips}

As dicas a seguir podem ajudar a melhorar a qualidade do reconhecimento de voz:

-   A forma como você grava áudio pode fazer uma grande diferença nos resultados do serviço. O reconhecimento de voz pode ser muito sensível à qualidade de áudio de entrada. Para obter a máxima precisão, assegure-se de que a qualidade de áudio de entrada seja tão boa quanto possível.
    -   Use um microfone próximo, orientado para fala (como um fone) sempre que possível e ajuste as configurações do microfone, se necessário. O serviço funciona melhor quando microfones profissionais são usados para capturar áudio.
    -   Evite usar um microfone integrado do sistema. Os microfones que são normalmente instalados em dispositivos móveis e tablets frequentemente são inadequados.
    -   Assegure-se de que os falantes próximos dos microfones. A precisão diminui à medida que um falante se afasta de um microfone. Em uma distância de 10 metros, por exemplo, o serviço tem dificuldade para produzir resultados adequados.
-   O reconhecimento de voz é sensível ao ruído de plano de fundo e às nuances da fala humana.
    -   O ruído do mecanismo, os dispositivos de trabalho, o ruído de rua e as conversas em segundo plano podem reduzir significativamente a precisão do reconhecimento.
    -   Os sotaques regionais e as diferenças na pronúncia também podem reduzir a precisão.

    Se seu áudio tiver essas características, considere usar a customização do modelo acústico para melhorar a precisão do reconhecimento de voz. Para obter mais informações, consulte [A interface de customização](/docs/services/speech-to-text/custom.html).
