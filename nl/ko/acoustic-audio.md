---

copyright:
  years: 2019
lastupdated: "2019-06-19"

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

# 오디오 리소스 관리
{: #manageAudio}

사용자 정의 인터페이스에는 오디오 리소스를 사용자 정의 음향 모델에 추가하는 데 사용되는 `POST /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 메소드가 포함되어 있습니다. 자세한 정보는 [사용자 정의 음향 모델에 오디오 추가](/docs/services/speech-to-text?topic=speech-to-text-acoustic#addAudio)를 참조하십시오. 이 인터페이스에는 사용자 정의 음향 모델의 오디오 리소스를 나열하고 삭제하기 위한 다음 메소드도 포함되어 있습니다.
{: shortdesc}

## 사용자 정의 음향 모델에 대한 오디오 리소스 나열
{: #listAudio}

사용자 정의 인터페이스는 사용자 정의 음향 모델의 오디오 리소스에 대한 정보를 나열하기 위한 두 가지 메소드를 제공합니다.

-   `GET /v1/acoustic_customizations/{customization_id}/audio` 메소드는 사용자 정의 모델에 추가된 모든 오디오 리소스에 대한 정보를 나열합니다.
-   `GET /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 메소드는 사용자 정의 모델의 지정된 오디오 리소스에 대한 정보를 나열합니다.

두 메소드 모두 오디오 리소스의 `name` 및 다음 추가 정보를 리턴합니다.

-   `duration`은 오디오의 길이(초 단위)입니다. 아카이브 파일의 경우 이 수치는 아카이브에 포함된 모든 오디오 파일의 누적 기간을 나타냅니다.
-   `details`는 리소스의 유형(`audio` 또는 `archive`)을 식별합니다. (사용자가 실수로 오디오가 포함되지 않은 파일을 전달했기 때문에 서비스가 리소스의 유효성을 검증할 수 없는 경우 유형은 `undetermined`입니다. 오디오 파일의 경우 세부사항에는 오디오의 `codec` 및 `frequency`가 포함됩니다. 아카이브 파일의 경우에는 해당 `compression` 유형이 포함됩니다.

또한 모델의 모든 오디오 리소스를 나열하면 모델의 모든 유효한 오디오 리소스에 대해 합산된 `total_minutes_of_audio`가 리턴됩니다. 이 값을 사용하여 사용자 정의 모델에 훈련을 시작하기에 충분하거나 너무 많은 오디오가 있는지를 판별할 수 있습니다.

또한 이 메소드는 오디오 데이터의 상태를 나열합니다. 상태는 사용자 정의 모델에 추가하는 요청에 대한 응답으로 오디오 파일에 대한 서비스의 분석을 확인하는 데 있어서 중요합니다.

-   `ok`는 서비스가 오디오 데이터를 성공적으로 분석했음을 표시합니다. 이 데이터를 사용하여 사용자 정의 모델을 훈련할 수 있습니다.
-   `being_processed`는 서비스가 오디오 데이터를 여전히 분석 중임을 표시합니다. 분석이 완료될 때까지 서비스가 새 오디오를 추가하거나 사용자 정의 모델을 훈련하라는 요청을 승인할 수 없습니다.
-   `invalid`는 오디오 데이터가 모델을 훈련하는 데 유효하지 않음을 표시합니다(데이터의 형식 또는 샘플링 속도가 잘못되었거나 데이터가 손상되었기 때문일 수 있음).

### 예제 요청: 모든 오디오 리소스 나열
{: #listExample-audio}

다음 예제는 지정된 사용자 정의 ID를 사용하는 사용자 정의 음향 모델에 대한 모든 오디오 리소스를 나열합니다. 음향 모델에는 세 개의 오디오 리소스가 있습니다. 서비스가 `audio1` 및 `audio2`를 성공적으로 분석했으며 `audio3`을 여전히 분석 중입니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio"
```
{: pre}

```javascript
{
  "total_minutes_of_audio": 11.45,
  "audio": [
    {
      "duration": 131,
      "name": "audio1",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 22050
      }
      "status": "ok"
    },
    {
      "duration": 556,
      "name": "audio2",
      "details": {
        "type": "archive",
        "compression": "zip"
      },
      "status": "ok"
    },
    {
      "duration": 0,
      "name": "audio3",
      "details": {},
      "status": "being_processed"
    }
  ]
}
```
{: codeblock}

### 예제 요청: 오디오 유형 리소스에 대한 정보 가져오기
{: #getExampleAudio}

다음 예제는 `audio1`이라는 오디오 유형 리소스에 대한 정보를 리턴합니다. 리소스는 131초 길이이며 `pcm_s16le` 코텍을 사용하여 인코딩됩니다. 이 리소스는 모델에 추가되었습니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio1"
```
{: pre}

```javascript
{
  "duration": 131,
  "name": "audio1",
  "details": {
    "codec": "pcm_s16le",
    "type": "audio",
    "frequency": 22050
  }
  "status": "ok"
}
```
{: codeblock}

### 예제 요청: 아카이브 유형 리소스에 대한 정보 가져오기
{: #getExampleArchive}

다음 예제는 `audio2`라는 아카이브 유형 리소스에 대한 정보를 리턴합니다. 이 리소스는 9분 이상의 오디오를 포함하는 **.zip** 파일입니다. 이 리소스도 모델에 추가되었습니다. 예제에 표시된 대로 아카이브 유형 리소스에 대한 정보를 조회하면 해당 리소스에 포함된 파일에 대한 정보도 제공됩니다.

```bash
curl -X GET -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio2"
```
{: pre}

```javascript
{
  "container": {
    "duration": 556,
    "name": "audio2",
    "details": {
      "type": "archive",
      "compression": "zip"
    },
    "status": "ok"
  },
  "audio": [
    {
      "duration": 121,
      "name": "audio-file1.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    {
      "duration": 133,
      "name": "audio-file2.wav",
      "details": {
        "codec": "pcm_s16le",
        "type": "audio",
        "frequency": 16000
      },
      "status": "ok"
    },
    . . .
  ]
}
```
{: codeblock}

## 사용자 정의 음향 모델에서 오디오 리소스 삭제
{: #deleteAudio}

사용자 정의 음향 모델에서 기존 오디오 리소스를 제거하려면 `DELETE /v1/acoustic_customizations/{customization_id}/audio/{audio_name}` 메소드를 사용하십시오. 아카이브 유형 오디오 리소스를 삭제하면 서비스가 전체 파일 아카이브를 제거합니다. 서비스에서는 아카이브 리소스에서 개별 파일을 삭제할 수 없습니다.

오디오 리소스를 제거해도 `POST /v1/acoustic_customizations/{customization_id}/train` 메소드를 사용하여 업데이트된 데이터에 대해 모델을 훈련할 때까지 사용자 정의 모델에 영향이 미치지 않습니다. 리소스에 대해 모델을 훈련한 경우 모델을 재훈련할 때까지 기존 오디오 데이터가 계속 음성 인식에 사용됩니다.

### 예제 요청
{: #deleteExample-audio}

다음 메소드는 지정된 사용자 정의 ID를 사용하는 사용자 정의 모델에서 `audio3`이라는 오디오 리소스를 삭제합니다.

```bash
curl -X DELETE -u "apikey:{apikey}"
"https://stream.watsonplatform.net/speech-to-text/api/v1/acoustic_customizations/{customization_id}/audio/audio3"
```
{: pre}
