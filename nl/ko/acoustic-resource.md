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

# 오디오 리소스에 대한 작업
{: #audioResources}

개별 오디오 파일이나 여러 오디오 파일이 포함된 아카이브 파일을 사용자 정의 음향 모델에 추가할 수 있습니다. 권장되는 오디오 리소스 추가 방법은 아카이브 파일을 추가하는 것입니다. 하나의 아카이브 파일을 작성하여 추가하는 것이 여러 오디오 파일을 개별적으로 추가하는 것보다 훨씬 더 효율적입니다.
{: shortdesc}

## 오디오 리소스 추가
{: #addAudioResource}

두 유형 중 하나의 오디오 리소스를 사용자 정의 음향 모델에 추가하려면 `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 메소드를 사용합니다. 오디오 리소스를 요청의 본문으로 전달하고 다음 매개변수를 포함합니다.

-   `customization_id` 매개변수: 모델의 사용자 정의 ID를 지정합니다.
-   `audio_name` 경로 매개변수: 오디오 리소스의 이름을 지정합니다.
    -   사용자 정의 모델의 언어와 일치하고 리소스의 컨텐츠를 반영하는 현지화된 이름을 사용하십시오.
    -   이름에 최대 128자를 포함하십시오.
    -   이름에 공백, `/`(슬래시) 또는 `\`(백슬래시)를 포함하지 마십시오.
    -   사용자 정의 모델에 이미 추가된 오디오 리소스의 이름을 사용하지 마십시오.

모델의 오디오 리소스를 업데이트하는 경우 변환 중에 변경사항을 적용하려면 모델을 훈련해야 합니다. 자세한 정보는 [사용자 정의 음향 모델 훈련](/docs/services/speech-to-text/acoustic-create.html#trainModel-acoustic)을 참조하십시오.

## 오디오 파일 추가
{: #addAudioType}

개별 오디오 파일을 사용자 정의 음향 모델에 추가하려면 `Content-Type` 헤더를 사용하여 오디오의 형식(MIME 유형)을 지정합니다. 인식 요청에 사용하도록 지원되는 모든 형식의 오디오를 추가할 수 있습니다. `rate`, `channels` 및 `endianness` 매개변수를 필요한 형식의 스펙과 함께 포함하십시오. 지원되는 오디오 형식에 대한 자세한 정보는 [오디오 형식](/docs/services/speech-to-text/audio-formats.html)을 참조하십시오.

오디오 형식의 `application/octet-stream` 스펙은 오디오 리소스에 대해 지원되지 않습니다.
{: note}

다음 [사용자 정의 음향 모델에 오디오 추가](/docs/services/speech-to-text/acoustic-create.html#addAudio) 예제에서는 `audio/wav` 파일을 추가합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: audio/wav"
--data-binary @audio1.wav
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
```
{: pre}

## 아카이브 파일 추가
{: #addArchiveType}

사용자 정의 음향 모델에 오디오를 추가하는 선호 방법은 여러 오디오 파일이 포함된 아카이브 파일을 추가하는 것입니다. `Content-Type` 요청 헤더로 아카이브의 유형을 지정하여 다음 유형의 아카이브 파일을 추가할 수 있습니다.

-   **.zip** 파일: `application/zip`으로 지정
-   **.tar.gz** 파일: `application/gzip`으로 지정

추가할 파일의 형식에 따라 `Contained-Content-Type` 헤더를 지정해야 할 수도 있습니다.

-   `audio/alaw`, `audio/basic`, `audio/l16` 또는 `audio/mulaw` 유형의 오디오 파일의 경우 `Contained-Content-Type` 헤더를 사용하여 오디오 파일의 형식을 지정해야 합니다. 필요한 경우 `rate`, `channels` 및 `endianness` 매개변수를 포함하십시오. 이 경우 아카이브 파일에 포함된 모든 오디오 파일의 오디오 형식이 동일해야 합니다.
-   기타 모든 유형의 오디오 파일의 경우 `Contained-Content-Type` 헤더를 생략할 수 있습니다. 이 경우 아카이브 파일에 포함된 오디오 파일의 형식이 이전 글머리 기호에 나열되지 않은 형식일 수 있습니다. 동일한 형식일 필요는 없습니다.

오디오 유형 리소스를 추가할 때 `Contained-Content-Type` 헤더를 사용하지 마십시오.
{: note}

아카이브 유형 리소스 내에 임베드된 오디오 파일의 이름이 `audio_name` 매개변수와 동일한 이름 지정 제한사항을 충족해야 합니다. 또한 아카이브 유형 리소스의 일부로 사용자 정의 모델에 이미 추가된 오디오 파일의 이름을 사용하지 마십시오.

다음 [사용자 정의 음향 모델에 오디오 추가](/docs/services/speech-to-text/acoustic-create.html#addAudio) 예제에서는 샘플링 속도가 16kHz인 `audio/l16` 형식의 오디오 파일이 포함된 `application/zip` 파일을 추가합니다.

```bash
curl -X POST -u "apikey:{apikey}"
--header "Content-Type: application/zip"
--header "Contained-Content-Type: audio/l16;rate=16000"
--data-binary @audio2.zip
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
```
{: pre}

## 오디오 리소스 추가 관련 가이드라인
{: #audioGuidelines}

사용자 정의 음향 모델의 사용을 통해 예상할 수 있는 인식 정확도 개선은 여러 가지 요인에 따라 다릅니다. 이러한 요인에는 사용자 정의 음향 모델에 포함된 오디오 데이터의 양과 해당 데이터가 변환되는 데이터와 얼마나 유사한지가 포함됩니다. 또한 사용자 정의 음향 모델이 해당 사용자 정의 언어 모델로 훈련되는지 여부에 따라 향상되는 개선 정도가 달라집니다. 

오디오 리소스를 사용자 정의 음향 모델에 추가할 때 다음 가이드라인을 따르십시오.

-   최소 10분 - 최대 200시간 길이의 오디오를 사용자 정의 음향 모델에 추가하십시오. 오디오에는 무음이 아니라 음성이 포함되어 있어야 합니다.

    추가할 양을 판별할 때 오디오의 품질이 영향을 미칩니다. 모델의 오디오가 인식될 오디오의 특성을 잘 반영할수록 음성 인식에 대한 사용자 정의 모델의 품질이 더 향상됩니다. 오디오 품질이 양호한 경우 더 추가하면 변환 정확도가 개선될 수 있습니다. 그러나 5 - 10시간 분량이 되는 양질의 오디오를 추가해도 긍정적인 영향을 미칠 수 있습니다.
-   100MB 이하의 오디오 리소스를 추가하십시오. 모든 오디오 및 아카이브 유형 리소스는 최대 크기가 100MB로 제한됩니다.

    단일 리소스로 추가할 수 있는 오디오의 양을 최대화하려면 압축을 제공하는 오디오 형식 사용을 고려하십시오. 자세한 정보는 [데이터 한계 및 압축](/docs/services/speech-to-text/audio-formats.html#limits)을 참조하십시오.
-   변환하려는 오디오의 음향 채널 상태를 반영하는 오디오 컨텐츠를 추가하십시오. 예를 들어, 애플리케이션이 자동차에서 발생하는 배경 소음이 있는 오디오를 처리하는 경우 동일한 유형의 데이터를 사용하여 사용자 정의 모델을 빌드하십시오.
-   오디오 파일의 샘플링 속도가 사용자 정의 음향 모델에 대한 기본 모델의 샘플링 속도와 일치하는지 확인하십시오.
    -   광대역 모델의 경우 샘플링 속도는 16kHz(초당 16,000개 샘플) 이상이어야 합니다.
    -   협대역 모델의 경우 샘플링 속도는 8kHz(초당 8,000개 샘플) 이상이어야 합니다.

    오디오의 샘플링 속도가 최소 필수 샘플링 속도보다 높은 경우 서비스가 적절한 속도로 오디오를 다운샘플링합니다. 오디오의 샘플링 속도가 최소 필수 속도보다 낮은 경우 서비스가 오디오 파일의 레이블을 `invalid`로 지정합니다. 아카이브 파일에 포함된 오디오 파일 중에 올바르지 않은 것이 있으면 서비스가 전체 아카이브를 올바르지 않은 것으로 간주합니다.
-   다음과 같은 경우에 사용자 정의 음향 모델과 함께 사용할 사용자 정의 언어 모델을 작성하십시오.
    -   오디오의 길이가 1시간 미만인 경우 최상의 결과를 얻으려면 오디오 변환을 기반으로 사용자 정의 언어 모델을 작성하십시오.
    -   오디오가 도메인에 특정하고 서비스의 기본 어휘에 없는 고유 단어를 포함하는 경우 언어 모델 사용자 정의를 사용하여 서비스의 기본 어휘를 확장하십시오. 음향 모델 사용자 정의만으로 변환 중에 해당 단어를 생성할 수는 없습니다.

    자세한 정보는 [사용자 정의 음향 및 사용자 정의 언어 모델을 함께 사용](/docs/services/speech-to-text/acoustic-both.html)을 참조하십시오.
