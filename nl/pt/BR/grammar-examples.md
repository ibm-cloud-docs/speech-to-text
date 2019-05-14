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

# Gramáticas de exemplo
{: #grammarExamples}

Os exemplos a seguir descrevem usos mais complexos para gramáticas com reconhecimento de voz. A maioria dos exemplos é fornecida em ambos os formatos, ABNF (`aplication/srgs`) e XML (`application/srgs+xml`). Use esses exemplos para ajudar a iniciar com gramáticas.
{: shortdesc}

Em uma regra de produção para uma gramática, incluir uma `\` (barra invertida) é um erro de sintaxe no ABNF e é interpretado como um valor literal em XML.
{: tip}

## Gramáticas de conformação
{: #confirmation}

As gramáticas de confirmação são úteis para aplicativos que esperam uma resposta de uma palavra a uma pergunta. As gramáticas a seguir definem uma lista de possíveis respostas sim e não.

<table style="width:50%">
  <caption>Tabela 1. Gramáticas de confirmação</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.abnf" download="confirm.abnf">confirm.abnf <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a>
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/confirm.xml" download="confirm.xml">confirm.xml <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a>
    </td>
  </tr>
</table>

## Listar gramáticas
{: #list}

Listar gramáticas é útil para aplicativos que esperam que o usuário selecione uma opção por meio de um conjunto predefinido de sequências. Às vezes, o conjunto é chamado de lista simples. Ele geralmente consiste em uma lista de elementos de terminal e não inclui uma estrutura de regra complicada.

-   As gramáticas a seguir definem uma lista de frases para dígitos.

    <table style="width:50%">
      <caption>Tabela 2. Listar gramáticas de número</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.abnf" download="list-numbers.abnf">list-numbers.abnf <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a>
        </td>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-numbers.xml" download="list-numbers.xml">list-numbers.xml <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a>
        </td>
      </tr>
    </table>

-   A gramática a seguir define uma lista de nomes válidos por meio dos quais o usuário pode escolher, talvez para selecionar um indivíduo de uma lista telefônica.

    <table style="width:50%">
      <caption>Tabela 3. Listar gramática de nomes</caption>
      <tr>
        <th style="text-align:center">ABNF</th>
        <th style="text-align:center">XML</th>
      </tr>
      <tr>
        <td style="text-align:center">
          <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/list-names.abnf" download="list-names.abnf">list-names.abnf <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a>
        </td>
        <td style="text-align:center">
          Não está disponível
        </td>
      </tr>
    </table>

## Gramáticas de número de identificação de veículo
{: #vin}

Os números de identificação de veículo (VINs) usam um formato alfanumérico fixo, assim como os números de cartão de crédito, os números de telefone e os números de seguridade social dos EUA. Uma grande vantagem das gramáticas sobre as n-grams clássicas é que as gramáticas são muito efetivas para especificar esses formatos.

O formato VIN é bem padronizado e tem um número fixo de caracteres. As n-grams clássicas executam mal para esses tipos de tarefas. Ao contrário das gramáticas, elas não podem assegurar que o número necessário de caracteres seja fornecido.

As gramáticas a seguir reconhecem os códigos VIN da Honda. Elas são mais complexas do que os exemplos anteriores, mas demonstram o poder das gramáticas adequadamente.

<table style="width:50%">
  <caption>Tabela 4. Gramáticas de VIN</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.abnf" download="vins.abnf">vins.abnf <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a>.
    </td>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/vins.xml" download="vins.xml">vins.xml <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a>.
    </td>
  </tr>
</table>

Para obter mais informações sobre o formato VIN, consulte [Número de identificação de veículo ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Vehicle_identification_number){: new_window} na Wikipédia.

## Gramáticas com elementos opcionais
{: #optionalElements}

Ao tornar opcionais determinados elementos de uma resposta, é possível tornar as gramáticas mais flexíveis antecipando como os usuários podem responder. A gramática a seguir inclui colchetes ao redor do elemento `$optionalphrase` para torná-lo opcional. O usuário pode falar uma de algumas frases adicionais antes de indicar o número de seguridade social. Por exemplo, o usuário pode dizer "meu número de seguridade social é xxx xx xxxx" ou apenas "xxx xx xxxx".

<table style="width:50%">
  <caption>Tabela 5. Gramática de elemento opcional</caption>
  <tr>
    <th style="text-align:center">ABNF</th>
    <th style="text-align:center">XML</th>
  </tr>
  <tr>
    <td style="text-align:center">
      <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/grammars/optional.abnf" download="optional.abnf">optional.abnf <img src="../../icons/launch-glyph.svg" alt="Ícone de link externo" title="Ícone de link externo"></a>
    </td>
    <td style="text-align:center">
      Não está disponível
    </td>
  </tr>
</table>

Para obter mais informações sobre expansões opcionais em gramáticas, consulte a [Seção 2.5 Repetições ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.w3.org/TR/speech-grammar/#S2.5){: new_window} da Speech Recognition Grammar Specification.

## Gramáticas de desambiguação
{: #disambiguation}

Outras gramáticas de exemplo possíveis incluem situações nas quais a confiança da resposta reconhecida não é muito alta, mas o aplicativo precisa ter certeza da resposta do usuário. Nesse caso, o aplicativo pode criar dinamicamente uma gramática que inclui as respostas mais prováveis e pedir ao usuário para repetir a resposta. Por exemplo, o aplicativo pode criar uma gramática que inclui as duas opções mais prováveis e pedir ao usuário para selecionar uma das duas: `Você quis dizer 'opção 1' ou 'opção 9'?`

As gramáticas de desambiguação são geradas programaticamente. Nenhum exemplo é fornecido.
