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

# 시작하기 튜토리얼
{: #gettingStarted}

{{site.data.keyword.speechtotextfull}} 서비스는 애플리케이션에 음성 인식 기능을 사용할 수 있도록 오디오를 텍스트로 변환합니다. 이 curl 기반 튜토리얼은 서비스를 빠르게 시작하는 데 도움이 될 수 있습니다. 이 예제는 서비스의 `POST /v1/recognize` 메소드를 호출하여 음성 내용을 요청하는 방법을 보여줍니다.
{: shortdesc}

## 시작하기 전에
{: #before-you-begin}

- {: hide-dashboard}  서비스 인스턴스를 작성하십시오.
    1.  {: hide-dashboard} {{site.data.keyword.cloud_notm}} Catalog의 [{{site.data.keyword.speechtotextshort}}](https://{DomainName}/catalog/services/speech-to-text){: external} 페이지로 이동하십시오.
    1.  {: hide-dashboard} 무료 {{site.data.keyword.cloud_notm}} 계정에 등록하거나 로그인하십시오.
    1.  {: hide-dashboard} **작성**을 클릭하십시오.
-   서비스 인스턴스에 인증하기 위한 인증 정보를 복사하십시오.
    1.  {: hide-dashboard} [{{site.data.keyword.cloud_notm}} 리소스 목록](https://{DomainName}/resources){: external}에서 {{site.data.keyword.speechtotextshort}} 서비스 인스턴스를 클릭하여 {{site.data.keyword.speechtotextshort}} 서비스 대시보드 페이지로 이동하십시오. 
    1.  **관리** 페이지에서 **표시**를 클릭하여 인증 정보를 보십시오.
    1.  `API 키` 및 `URL` 값을 복사하십시오.

### curl 예제 사용
{: #getting-started-curl}

이 튜토리얼에서는 `curl` 명령을 사용하여 서비스 HTTP 인터페이스의 메소드를 호출합니다. 사용자의 시스템에 `curl` 명령이 설치되어 있는지 확인하십시오.

1.  `curl` 설치 여부를 테스트하려면 명령행에 다음 명령을 실행하십시오. 출력에 SSL(Secure Sockets Layer)을 지원하는 `curl` 버전이 나열되는 경우 튜토리얼을 시작할 수 있습니다. 

    ```bash
    curl -V
    ```
    {: pre}

1.  필요한 경우 [curl.haxx.se](https://curl.haxx.se/){: external}에서 사용자의 운영 체제에 해당하는 `curl` 버전을 설치하고 SSL을 사용으로 설정하십시오. 

예제에서 중괄호를 삭제하십시오. 중괄호는 변수값을 표시합니다.
{: tip}

## 1단계: 옵션 없이 오디오 변환
{: #transcribe}

`POST /v1/recognize` 메소드를 호출하여 추가 요청 매개변수 없이 FLAC 오디오 파일의 기본 음성 내용을 요청하십시오.

1.  샘플 오디오 파일 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>를 다운로드하십시오.
1.  매개변수가 없는 기본 변환을 위해 서비스의 `/v1/recognize` 메소드를 호출하려면 다음 명령을 실행하십시오. 이 예제에서는 `Content-Type` 헤더를 사용하여 오디오 유형 `audio/flac`를 표시합니다. 이 예제에서는 기본 언어 모델 `en-US_BroadbandModel`을 변환에 사용합니다.
    -   {: hide-dashboard}`{apikey}` 및 `{url}`을 사용자의 API 키 및 URL로 대체하십시오.
    -   `{path_to_file}`을 수정하여 `audio-file.flac` 파일의 위치를 지정하십시오.

    *Windows 사용자의 경우* 각 행 끝에 백슬래시(``\`)를 캐럿(``^`)으로 대체합니다. 후행 공백이 없어야 합니다.
    {: tip}

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize"{: url}
    ```
    {: pre}

    이 서비스는 다음과 같은 변환 결과를 리턴합니다.

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

## 2단계: 옵션을 사용하여 오디오 변환
{: #transcribeOptions}

`POST /v1/recognize` 메소드를 호출하여 동일한 FLAC 오디오 파일을 호출하지만 두 개의 변환 매개변수를 지정하십시오.

1.  필요한 경우 샘플 오디오 파일 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘" title="외부 링크 아이콘"></a>를 다운로드하십시오.
1.  두 개의 추가 매개변수를 사용하여 서비스의 `/v1/recognize` 메소드를 호출하려면 다음 명령을 실행하십시오. 오디오 스트림의 각 단어에 대한 시작과 끝을 표시하도록 `timestamps` 매개변수를 `true`로 설정하십시오. 변환에 대한 가장 가능성이 높은 세 개의 대안을 수신하도록 `max_alternatives` 매개변수를 `3`을 설정하십시오. 이 예제에서는 `Content-Type` 헤더를 사용하여 오디오 유형 `audio/flac`를 표시하고 요청이 기본 모델인 `en-US_BroadbandModel`을 사용합니다.
    -   {: hide-dashboard}`{apikey}` 및 `{url}`을 사용자의 API 키 및 URL로 대체하십시오.
    -   `{path_to_file}`을 수정하여 `audio-file.flac` 파일의 위치를 지정하십시오.

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize?timestamps=true&max_alternatives=3"{: url}
    ```
    {: pre}

    이 서비스가 시간소인 및 세 개의 대체 변환이 포함된 다음과 같은 결과를 리턴합니다.

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
              "transcript": "several tornadoes touched down as a line
of severe thunderstorms swept through Colorado on Sunday "
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

## 다음 단계

-   [개발자를 위한 개요](/docs/services/speech-to-text?topic=speech-to-text-developerOverview)에서 음성 인식 요청을 수행하는 데 사용할 수 있는 인터페이스 및 SDK에 대해 자세히 알아보십시오.
-   [인식 요청 작성](/docs/services/speech-to-text?topic=speech-to-text-basic-request)에서 각 서비스 인스턴스의 기본 음성 인식 요청을 확인하십시오.
-   [API 참조](https://{DomainName}/apidocs/speech-to-text){: external}에서 서비스 인스턴스의 모든 메소드에 대한 자세한 정보를 찾으십시오.
