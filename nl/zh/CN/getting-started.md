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

# 入门教程
{: #gettingStarted}

{{site.data.keyword.speechtotextfull}} 服务用于将音频转录为文本，以支持针对各种应用程序的语音转录功能。本教程基于 curl，可帮助您快速开始使用服务。其中的示例说明了如何调用服务的 `POST /v1/recognize` 方法来请求文字记录。
{: shortdesc}

## 开始之前
{: #before-you-begin}

- {: hide-dashboard} 创建服务的实例：
    1.  {: hide-dashboard} 转至 {{site.data.keyword.cloud_notm}}“目录”中的 [{{site.data.keyword.speechtotextshort}}](https://{DomainName}/catalog/services/speech-to-text){: external} 页面。
    1.  {: hide-dashboard} 注册免费的 {{site.data.keyword.cloud_notm}} 帐户或登录。
    1.  {: hide-dashboard} 单击**创建**。
-   复制要用于向服务实例进行认证的凭证：
    1.  {: hide-dashboard} 在 [{{site.data.keyword.cloud_notm}} 资源列表](https://{DomainName}/resources){: external}中，单击 {{site.data.keyword.speechtotextshort}} 服务实例以转至 {{site.data.keyword.speechtotextshort}} 服务仪表板页面。
    1.  在**管理**页面上，单击**显示**以查看凭证。
    1.  复制 `API 密钥`和 `URL` 值。

### 使用 curl 示例
{: #getting-started-curl}

本教程使用 `curl` 命令来调用服务的 HTTP 接口的方法。请确保已在系统上安装了 `curl` 命令。

1.  要测试是否已安装 `curl`，请在命令行上运行以下命令。如果输出列出了支持安全套接字层 (SSL) 的 `curl` 版本，说明已准备好使用本教程。

    ```bash
    curl -V
    ```
    {: pre}

1.  如果需要，请从 [curl.haxx.se](https://curl.haxx.se/){: external} 安装适合您操作系统的启用 SSL 的 `curl` 版本。

省略示例中的花括号。花括号指示这是变量值。
{: tip}

## 步骤 1：在不使用选项的情况下转录音频
{: #transcribe}

调用不包含其他请求参数的 `POST /v1/recognize` 方法来请求 FLAC 音频文件的基本文字记录。

1.  下载样本音频文件 <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/speech-to-text/audio-file.flac" download="audio-file.flac">audio-file.flac <img src="../../icons/launch-glyph.svg" alt="外部链接图标" title="外部链接图标"></a>。
1.  发出以下命令以在不使用任何参数的情况下调用服务的 `/v1/recognize` 方法，从而获取基本转录。示例使用 `Content-Type` 头来指示音频的类型 `audio/flac`。此示例使用缺省语言模型 `en-US_BroadbandModel` 进行转录。
    -   {: hide-dashboard} 将 `{apikey}` 和 `{url}` 替换为您的 API 密钥和 URL。
    -   修改 `{path_to_file}` 以指定 `audio-file.flac` 文件的位置。

    *Windows 用户*应将每行末尾的反斜杠 (``\`) 替换为插入标记 (``^`)。确保无尾部空格。
    {: tip}

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

## 后续步骤

-   在[开发者概述](/docs/services/speech-to-text?topic=speech-to-text-developerOverview)中了解有关可用于发出语音识别请求的接口和 SDK 的更多信息。
-   请参阅[发出识别请求](/docs/services/speech-to-text?topic=speech-to-text-basic-request)中每个服务接口的基本语音识别请求。
-   在 [API 参考](https://{DomainName}/apidocs/speech-to-text){: external}中查找有关服务接口的所有方法的详细信息。
