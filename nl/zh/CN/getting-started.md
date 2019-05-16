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

# 入门教程
{: #gettingStarted}

{{site.data.keyword.speechtotextfull}} 服务用于将音频转录为文本，以支持针对各种应用程序的语音转录功能。本教程基于 curl，可帮助您快速开始使用服务。其中的示例说明了如何调用服务的 `POST /v1/recognize` 方法来请求抄本。
{: shortdesc}

本教程使用 {{site.data.keyword.cloud}} Identity and Access Management (IAM) API 密钥进行认证。较旧的服务实例可能会继续使用其现有 Cloud Foundry 服务凭证中的 `{username}` 和 `{password}` 进行认证。请使用适合您服务实例的方法进行认证。有关服务使用 IAM 认证的更多信息，请参阅发行说明中的 [2018 年 10 月 30 日服务更新](/docs/services/speech-to-text/release-notes.html#October2018b)。
{: important}

## 开始之前
{: #before-you-begin}

- {: hide-dashboard} 创建服务的实例：
    1.  {: hide-dashboard} 转至 {{site.data.keyword.cloud_notm}}“目录”中的 [{{site.data.keyword.speechtotextshort}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/catalog/services/speech-to-text){: new_window} 页面。
    1.  {: hide-dashboard} 注册免费的 {{site.data.keyword.cloud_notm}} 帐户或登录。
    1.  {: hide-dashboard} 单击**创建**。
-   复制要用于向服务实例进行认证的凭证：
    1.  {: hide-dashboard} 在 [{{site.data.keyword.cloud_notm}}“仪表板”![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/dashboard/apps){: new_window} 中，单击 {{site.data.keyword.speechtotextshort}} 服务实例以转至 {{site.data.keyword.speechtotextshort}} 服务仪表板页面。
    1.  在**管理**页面上，单击**显示**以查看凭证。
    1.  复制 `API 密钥`和 `URL` 值。
-   确保您具有 `curl` 命令。
    -   示例使用 `curl` 命令来调用 HTTP 接口的方法。通过 [curl.haxx.se ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://curl.haxx.se/){: new_window} 来安装适用于您操作系统的版本。请安装支持安全套接字层 (SSL) 协议的版本。确保在 `PATH` 环境变量上包含安装的二进制文件。

输入命令时，请将 `{apikey}` 和 `{url}` 替换为实际 API 密钥和 URL。在命令中省略用于指示变量值的花括号。实际值类似于以下示例：
{: hide-dashboard}

```bash
curl -X POST -u "apikey:L_HALhLVIksh1b73l97LSs6R_3gLo4xkujAaxm7i-b9x"
. . .
"https://stream.watsonplatform.net/speech-to-text/api/v1/recognize"
```
{:pre}
{: hide-dashboard}

## 步骤 1：在不使用选项的情况下转录音频
{: #transcribe}

调用不包含其他请求参数的 `POST /v1/recognize` 方法来请求 FLAC 音频文件的基本抄本。

1.  下载样本音频文件 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a>。
1.  发出以下命令以在不使用任何参数的情况下调用服务的 `/v1/recognize` 方法，从而获取基本转录。示例使用 `Content-Type` 头来指示音频的类型 `audio/flac`。此示例使用缺省语言模型 `en-US_BroadbandModel` 进行转录。
    -   {: hide-dashboard} 将 `{apikey}` 和 `{url}` 替换为您的 API 密钥和 URL。
    -   修改 `{path_to_file}` 以指定 `audio-file.flac` 文件的位置。

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize"{: url}
    ```
    {: pre}

    服务返回的转录结果如下：

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

## 步骤 2：使用选项转录音频
{: #transcribeOptions}

调用 `POST /v1/recognize` 方法来转录相同的 FLAC 音频文件，但这次指定两个转录参数。

1.  如果需要，请下载样本音频文件 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a>。
1.  发出以下命令以使用两个额外参数调用服务的 `/v1/recognize` 方法。将 `timestamps` 参数设置为 `true`，以指示音频流中每个词的开始和结束时间。将 `max_alternatives` 参数设置为 `3`，以接收三种最有可能的转录替代项。示例使用 `Content-Type` 头来指示音频的类型 `audio/flac`，并且请求使用缺省模型 `en-US_BroadbandModel`。
    -   {: hide-dashboard} 将 `{apikey}` 和 `{url}` 替换为您的 API 密钥和 URL。
    -   修改 `{path_to_file}` 以指定 `audio-file.flac` 文件的位置。

    ```bash
    curl -X POST -u "apikey:{apikey}"{: apikey} \
    --header "Content-Type: audio/flac" \
    --data-binary @{path_to_file}audio-file.flac \
    "{url}/v1/recognize?timestamps=true&max_alternatives=3"{: url}
    ```
    {: pre}

    服务返回以下结果，其中包含时间戳记和三个替代转录：

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

## 后续步骤

-   在[开发者概述](/docs/services/speech-to-text/developer-overview.html)中了解有关可用于发出语音识别请求的接口和 SDK 的更多信息。
-   请参阅[发出识别请求](/docs/services/speech-to-text/basic-request.html)中每个服务接口的基本语音识别请求。
-   在 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/speech-to-text){: new_window} 中查找有关服务接口的所有方法的详细信息。

