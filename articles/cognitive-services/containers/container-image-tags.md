---
title: 认知服务容器映像标记
titleSuffix: Azure Cognitive Services
description: 所有认知服务容器映像标记的全面列表。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: reference
ms.date: 11/17/2020
ms.author: aahi
ms.openlocfilehash: 09a83c28d07540b8ecd813e7ab2f10ceee891d7a
ms.sourcegitcommit: 6a770fc07237f02bea8cc463f3d8cc5c246d7c65
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95792990"
---
# <a name="azure-cognitive-services-container-image-tags-and-release-notes"></a>Azure 认知服务容器图像标记和发行说明

Azure 认知服务提供了许多容器映像。 容器注册表和相应的存储库因容器映像而异。 每个容器映像名称都提供多个标记。 容器映像标记是对容器映像进行版本控制的机制。 本文旨在作为综合性的参考资料使用，其中列出了所有认知服务容器映像及其可用标记。

> [!TIP]
> 在使用 [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) 时，请密切注意容器注册表、存储库、容器映像名称和相应标记的大小写，因为它们是区分大小写的。

## <a name="anomaly-detector"></a>异常检测器

可以在容器注册表联合中找到 [异常探测器][ad-containers] 容器映像 `mcr.microsoft.com` 。 它驻留在 `azure-cognitive-services/decision` 存储库中，名为 `anomaly-detector`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/decision/anomaly-detector`。

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/decision/anomaly-detector/tags/list)的完整列表。

# <a name="latest-version"></a>[最新版本](#tab/current)

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013560003-amd64-preview` |      |

# <a name="previous-versions"></a>[旧版](#tab/previous)

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `1.1.012300001-amd64-preview` |       |

---

## <a name="read-ocr-optical-character-recognition"></a>读取 OCR (光学字符识别) 

可以在容器注册表联合中找到 [计算机视觉][cv-containers] 读取 OCR 容器映像 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services` 存储库中，名为 `read`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/vision/read`。

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/vision/read/tags/list)的完整列表。

# <a name="latest-version"></a>[最新版本](#tab/current)

发行说明 `3.2-preview.1` ：

* 新的3.2 容器

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `latest`                      |       |
| `3.2-preview.1` |  |

# <a name="previous-versions"></a>[旧版](#tab/previous)

发行说明 `v2.0.013250001-amd64-preview` ：

* 进一步降低容器的内存使用率。
* 对于多 pod 安装程序，外部缓存是必需的。 例如，设置 Redis 以用于缓存。
* 修复了 Redis 缓存设置为0时丢失的结果 `ResultExpirationPeriod` 。
* 删除26MB 的请求正文大小限制。 容器现在可以接受大于 26MB 的文件。
* 向控制台日志记录添加时间戳和生成版本。

的发行说明 `1.1.013050001-amd64-preview`

* 添加了 `ReadEngineConfig:ResultExpirationPeriod` 用于指定系统何时应清除识别结果的容器初始化配置。 
    * 此设置为小时，默认值为48小时。
    * 该设置可减少用于存储结果的内存使用量，尤其是在使用容器内存中存储时。
    * 示例 1。 ReadEngineConfig:ResultExpirationPeriod=1，系统将在进程结束后 1 小时清除识别结果。
    * 如果将此配置设置为0，则系统会在检索结果后清除识别结果。

* 修复了在向系统传递无效的图像格式时出现500内部服务器错误。 现在，它将会返回 400 错误：

    ```json
    {
        "error": {
        "code": "InvalidImageSize",
        "message": "Image must be between 1024 and 209715200 bytes."
        }
    }
    ```

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `2.0.013250001-amd64-preview` |       |
| `1.1.013050001-amd64-preview` |       |
| `1.1.011580001-amd64-preview` |       |
| `1.1.009920003-amd64-preview` |       |
| `1.1.009910003-amd64-preview` |       |

---


## <a name="form-recognizer"></a>表单识别器

可以在容器注册表 "联合" 中找到 [窗体识别器][fr-containers] 容器映像 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services/custom-form` 存储库中，名为 `labeltool`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool`。

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/custom-form/labeltool/tags/list)的完整列表。

# <a name="latest-version"></a>[最新版本](#tab/current)

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |


# <a name="previous-versions"></a>[旧版](#tab/previous)

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `1.1.008640001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |

---

## <a name="language-understanding-luis"></a>语言理解 (LUIS)

在 `mcr.microsoft.com` 容器注册表联合项中可以找到 [LUIS][lu-containers] 容器映像。 该映像驻留在 `azure-cognitive-services/language` 存储库中，名为 `luis`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/language/luis`。

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/language/luis/tags/list)的完整列表。

# <a name="latest-version"></a>[最新版本](#tab/current)

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012280003-amd64-preview` |       |


# <a name="previous-version"></a>[先前版本](#tab/previous)

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `1.1.012130003-amd64-preview` |       |

---

## <a name="custom-speech-to-text"></a>自定义语音转文本

可以在容器注册表 "联合" 中找到 [自定义语音到文本][sp-cstt] 的容器映像 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services/speechservices/` 存储库中，名为 `custom-speech-to-text`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-speech-to-text`。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-speech-to-text/tags/list)的完整列表。


# <a name="latest-version"></a>[最新版本](#tab/current)

发行说明 `2.7.0-amd64` ：

**功能**
* 默认情况下，标点设置为启用。

请注意，由于包含短语列表，此容器映像的大小已增加。

| 映像标记                    | 说明 | 摘要                                                                  |
|-------------------------------|:------|:------------------------------------------------------------------------|
| `latest`                      |       | `sha256:d1573c2543cb7afedb0122da0995f345767b02f9c5f181950acf1509ca65726` |
| `2.7.0-amd64`                 |       | `sha256:d1573c2543cb7afedb0122da0995f345767b02f9c5f181950acf1509ca65726` |


# <a name="previous-version"></a>[先前版本](#tab/previous)
发行说明 `2.6.0-amd64` ：

**功能**
* 支持 phraselist v2 
* 短语列表在以下区域设置中受支持：
    * en au
    * en-ca
    * en-gb
    * en
    * zh-cn
    * zh-cn
* 支持自定义基本模型下载。 
    * `BaseModelLocale=<locale>`与命令一起 `docker run` 使用
* 完全迁移到 .NET 3。1

**修复项**
* 修复了在 Diarization 模式下置信分数始终为1的问题
* 已迁移到 TextAnalytics 3.0 api

请注意，由于包含短语列表，此容器映像的大小已增加。

发行说明 `2.5.0-amd64` ：

**功能**
* 支持自定义模型上的自定义发音
* 支持 Azure 和 Azure 美国政府云

**修复项**
* 在 Diarization 模式下修复作为非超级用户运行的用户问题

| 映像标记                    | 说明               |
|-------------------------------|:--------------------|
| `2.6.0-amd64`                 |                     |
| `2.5.0-amd64`                 |   第一 GA 版本    |

---

## <a name="custom-text-to-speech"></a>自定义文本到语音转换

可在容器注册表 "联合" 中找到 [自定义的文本到语音][sp-ctts] 容器图像 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services/speechservices/` 存储库中，名为 `custom-text-to-speech`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-text-to-speech`。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-text-to-speech/tags/list)的完整列表。


# <a name="latest-version"></a>[最新版本](#tab/current)

发行说明 `1.9.0-amd64` ：

定期每月发布

| 映像标记                    | 说明 | 摘要                                                                  |
|-------------------------------|:------|:------------------------------------------------------------------------|
| `latest`                      |       | `sha256:e0397cf12d1367b13dd258f782bb513c93afcd5ee4b897794fe533205336355` |
| `1.9.0-amd64`                 |       | `sha256:e0397cf12d1367b13dd258f782bb513c93afcd5ee4b897794fe533205336355` |


# <a name="previous-version"></a>[先前版本](#tab/previous)
发行说明 `1.8.0-amd64` ：

**功能**
* 完全迁移到 .NET 3。1

发行说明 `1.7.0-amd64` ：

**功能**
* 部分迁移到 .NET 3。1

| 映像标记                    | 说明               |
|-------------------------------|:--------------------|
| `1.8.0-amd64`                 |                     |
| `1.7.0-amd64`                 |   第一 GA 版本    |

---

## <a name="speech-to-text"></a>语音转文本

" [语音到文本][sp-stt] " 容器映像可在容器注册表 "联合" 中找到 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services/speechservices/` 存储库中，名为 `speech-to-text`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/speechservices/speech-to-text`。 可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/speech-to-text/tags/list)的完整列表。

由于从语音到文本的 v 2.5.0， *美国政府弗吉尼亚州* 区域支持映像。 使用此区域时，请使用 *美国政府弗吉尼亚* 帐单终结点和 API 密钥。

# <a name="latest-version"></a>[最新版本](#tab/current)

发行说明 `2.7.0-amd64-<locale>` ：

**功能**
* 支持以下新的区域设置：
    * ar-bh、ar-iq、ar-jo、ar-lb、ar-om、ar-sy
    * bg-bg
    * el-gr
    * en-香港，en-ie，en-us，en sg，zh-cn
    * es-ar、es、es、es、es、es、es、es、es、es、es、es、es、es、es、es、es、es、es、es、es、es、es、es、uy、es-ve
    * et-ee
    * ga-ie
    * hr-hr
    * hu-hu
    * lt-lt
    * lv-lv
    * mt-mt
    * ro-ro
    * sk-sk
    * sl-sl
* 默认情况下启用标点符号。

请注意，由于包含短语列表，此容器映像的大小已增加。 

| 映像标记                    | 说明                                                                                                |
|-------------------------------|:-----------------------------------------------------------------------------------------------------|
| `latest`                      | 具有区域设置的容器映像 `en-US` 。                                                             |
| `2.7.0-amd64-<locale>`        | `<locale>`使用下面列出的一个可用区域设置替换。 例如，`2.7.0-amd64-en-us`。 |

此容器具有以下可用的区域设置。

| V 2.7.0 的区域设置           | 备注                                    | 摘要                                                                  |
|-----------------------------|:-----------------------------------------|:------------------------------------------------------------------------|
| `ar-ae`                     | 具有区域设置的容器映像 `ar-AE` 。 | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-bh`                     | 具有区域设置的容器映像 `ar-BH` 。 | `sha256:5a2c140661f50d0c95587121ec1ab8895289f4dda5b3ad14074413e869e6bd4` |
| `ar-eg`                     | 具有区域设置的容器映像 `ar-EG` 。 | `sha256:783bb8321fcfb7890b0c99935099f7e84c85a698c2fe0031c661e265358d79c` |
| `ar-iq`                     | 具有区域设置的容器映像 `ar-IQ` 。 | `sha256:abd0101f73c1cf71f30da7b11b93d2a7ac8877dbfcfc2d34553d20705aca7a2` |
| `ar-jo`                     | 具有区域设置的容器映像 `ar-JO` 。 | `sha256:d4c7fd2a1637e163aa106c23b6a759e8c78366c60ece83b3aabfe93ebabae07` |
| `ar-kw`                     | 具有区域设置的容器映像 `ar-KW` 。 | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-lb`                     | 具有区域设置的容器映像 `ar-LB` 。 | `sha256:20e5c9105e86625c72de54290a6eb07630d35c3760f729c4b855e3661583dfe` |
| `ar-om`                     | 具有区域设置的容器映像 `ar-OM` 。 | `sha256:97f1b44f2cbb837a2ef86441a0a52a07f706240edb6ef6618ee4db8cbbe1c19` |
| `ar-qa`                     | 具有区域设置的容器映像 `ar-QA` 。 | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-sa`                     | 具有区域设置的容器映像 `ar-SA` 。 | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-sy`                     | 具有区域设置的容器映像 `ar-SY` 。 | `sha256:51980a2e2c3dd3548deedcedaf5fc688db602a5eced1a4b7df7d10750393623` |
| `bg-bg`                     | 具有区域设置的容器映像 `bg-BG` 。 | `sha256:1c1acf0fbb353ebb04692f37eb4d4cdf0b4e309720dd7e709001dada0d1ea81` |
| `ca-es`                     | 具有区域设置的容器映像 `ca-ES` 。 | `sha256:c60baa0007f61c7652b97b49645215de63411125d627c974c09222e316df204` |
| `cs-cz`                     | 具有区域设置的容器映像 `cs-CZ` 。 | `sha256:3fa09fc3a6bde6b77df2444aae8fc78b5f25fb9010171d1682db116ea5801f5` |
| `da-dk`                     | 具有区域设置的容器映像 `da-DK` 。 | `sha256:4b26dbba50c2771943880b68e0e4ea0713d0e3bb8bad884454849bccc9e94a3` |
| `de-de`                     | 具有区域设置的容器映像 `de-DE` 。 | `sha256:5109ed80b1fecf4db0328adcd50528d0aa9e726b5fc84587c40aaea4e91256d` |
| `el-gr`                     | 具有区域设置的容器映像 `el-GR` 。 | `sha256:fc8b466c588bf097efac2b79454d5ac0df5c6990398f07ede9be7e1d536e4bd` |
| `en-au`                     | 具有区域设置的容器映像 `en-AU` 。 | `sha256:3461892a27fc3eb3f9610b2def00bc15f380c6b9797c90ceca19e6abb55f6a6` |
| `en-ca`                     | 具有区域设置的容器映像 `en-CA` 。 | `sha256:a0509be39785f1e869bd96ab10e7c07d3f4e61c9aa17ff5900076e7bd64ba11` |
| `en-gb`                     | 具有区域设置的容器映像 `en-GB` 。 | `sha256:1b976fc7ac109e61dcf74af3652c12535e3db92931d2d0bb2ea59bd46f9efed` |
| `en-hk`                     | 具有区域设置的容器映像 `en-HK` 。 | `sha256:0b1e1df101f978869c98f6e50632712016b8311fc89b334e7f44e968d64bf2f` |
| `en-ie`                     | 具有区域设置的容器映像 `en-IE` 。 | `sha256:c5ba0d3c7219ce39f0b918a51a7cae8a65c277f564279cad920e068725aa39f` |
| `en-in`                     | 具有区域设置的容器映像 `en-IN` 。 | `sha256:e907f07be498f024103f6fe6abffa23e242bf3585724741b29a2f3f41d0899c` |
| `en-nz`                     | 具有区域设置的容器映像 `en-NZ` 。 | `sha256:66845f6ce20ae71d609867c6eb4772366ce042499e4bcdce4c1b579daf7fad7` |
| `en-ph`                     | 具有区域设置的容器映像 `en-PH` 。 | `sha256:e7874653bf66b1a1ab344b3391eb8767be34260b7f11b62fd057cbe17b805b2` |
| `en-sg`                     | 具有区域设置的容器映像 `en-SG` 。 | `sha256:827cdb158280e6f4037f4815410c7aa78abf9c6467876c1504aecfef787bdd7` |
| `en-us`                     | 具有区域设置的容器映像 `en-US` 。 | `sha256:248d17340055e3e137219ddc234c605e6a53ceead136ea55c9697c352da6a8d` |
| `en-za`                     | 具有区域设置的容器映像 `en-ZA` 。 | `sha256:a8abc99f498db7088bb25acec47da81e90b6a5eaa1c6f78e0f9a314d839d0ae` |
| `es-ar`                     | 具有区域设置的容器映像 `es-AR` 。 | `sha256:edf78429630851b6eb01f54f8a8a1aeeda9971c6a834403a204662eda22b3b9` |
| `es-bo`                     | 具有区域设置的容器映像 `es-BO` 。 | `sha256:5832b44f1da2f6b9a097c99babfbc370d8d0eabe1ff8daabec2c3f482dc9d63` |
| `es-cl`                     | 具有区域设置的容器映像 `es-CL` 。 | `sha256:409a712b96235e154472134f96ff9272265f1e5b555e00ad03c2260b0781009` |
| `es-co`                     | 具有区域设置的容器映像 `es-CO` 。 | `sha256:99792bc083dc16e0edf15491e6a840d786c9140b747551563a8d98f66f0b415` |
| `es-cr`                     | 具有区域设置的容器映像 `es-CR` 。 | `sha256:21fe14a538e5b8b2d288b00b8f5a02d87469e285f32e725155042079f336ac9` |
| `es-cu`                     | 具有区域设置的容器映像 `es-CU` 。 | `sha256:05d40eae01cec4c42c4febd379cd61373eb43d0aacfd47b988bb95e6a6ad216` |
| `es-do`                     | 具有区域设置的容器映像 `es-DO` 。 | `sha256:73dd0e0d4f39a259563ee7cc18c2e72c9ab20c52905fe343e0413ca7c4b3f0d` |
| `es-ec`                     | 具有区域设置的容器映像 `es-EC` 。 | `sha256:c3e69139ef365fe9332b5b68b43458242c7dad9d9f2b557431272306e81cb9e` |
| `es-es`                     | 具有区域设置的容器映像 `es-ES` 。 | `sha256:bd83fcfc116ba645a0e12a7a93b6ada74a8f701172f826a91c5f223a1dbaa61` |
| `es-gt`                     | 具有区域设置的容器映像 `es-GT` 。 | `sha256:5bb9b18b91b74e123e3720893d88bfcb0a87dac31a1f7171d23c7cb1fa09fee` |
| `es-hn`                     | 具有区域设置的容器映像 `es-HN` 。 | `sha256:941d108a4b76eb554e8f13cf5090665a702de3ebf35b75e4350f0916dfccd72` |
| `es-mx`                     | 具有区域设置的容器映像 `es-MX` 。 | `sha256:cebea03732781b4425500d162ae6580bbd7ce9b5f4ede988c4570fe311d8567` |
| `es-ni`                     | 具有区域设置的容器映像 `es-NI` 。 | `sha256:8ba165f94ad840936ebd0af17a0a63aa08a6292e7ad9029f5b93eef41165eb9` |
| `es-pa`                     | 具有区域设置的容器映像 `es-PA` 。 | `sha256:c61b7f1b6801a03c3eab0dd1aede87017a86bc7368ded2f8bad8d9e5f60d0d3` |
| `es-pe`                     | 具有区域设置的容器映像 `es-PE` 。 | `sha256:447a3ab3f302aba24d201d9f5b2877ffcd64dfd5e9d6b88d9924847160b2de2` |
| `es-pr`                     | 具有区域设置的容器映像 `es-PR` 。 | `sha256:a53b3295c986e91ee8cf93ebe1057b997c76ef7f99913508b859311a194fdd4` |
| `es-py`                     | 具有区域设置的容器映像 `es-PY` 。 | `sha256:85b3f75e75e63e29521daf772ee68a59ac2428579512501aa81dc51a2315652` |
| `es-sv`                     | 具有区域设置的容器映像 `es-SV` 。 | `sha256:db5ece7ba536e38d5de59cd37807630ab76589dcf1c97e253f98d7f44d9424e` |
| `es-us`                     | 具有区域设置的容器映像 `es-US` 。 | `sha256:99f2743725bb71e25543484f49bcfde14584ccbbaaa912678938d69d965075a` |
| `es-uy`                     | 具有区域设置的容器映像 `es-UY` 。 | `sha256:a3e11c16a97a1ae76408d812b2fee1e4b3ba07160bbcb62a22814523568ee5d` |
| `es-ve`                     | 具有区域设置的容器映像 `es-VE` 。 | `sha256:8cb431aafd84263ead8de946377c1d3f2ddfa7e172b8a4c5aa7ba477c5b41f0` |
| `et-ee`                     | 具有区域设置的容器映像 `et-EE` 。 | `sha256:943e7cf894e9d75341a58993104824c1c8cd8da1322cc5a732e9d53882c6523` |
| `fi-fi`                     | 具有区域设置的容器映像 `fi-FI` 。 | `sha256:35658e9dce796cb96a1371f250398e86351ea1b5ada080da7ce8471b30c7cae` |
| `fr-ca`                     | 具有区域设置的容器映像 `fr-CA` 。 | `sha256:62256cad671e8baa03fdd4c5f4eca7d5c5effedd64cafd9020ba72c9c4210e0` |
| `fr-fr`                     | 具有区域设置的容器映像 `fr-FR` 。 | `sha256:b385993232d9daa327d1a7b067268927b17f36eed3e8d423748794544c62746` |
| `ga-ie`                     | 具有区域设置的容器映像 `ga-IE` 。 | `sha256:ab9abdb993b0f7487edda8200f1393ac44ba4888c0f444a02afb6c85ca3e393` |
| `gu-in`                     | 具有区域设置的容器映像 `gu-IN` 。 | `sha256:328e69488f2948722d7ccc97e266071f61a8c9f65cd671688490955806526de` |
| `hi-in`                     | 具有区域设置的容器映像 `hi-IN` 。 | `sha256:b9b0bfec80aa53d06ea2cbd9097f753ec5caaf00ac2f00321ae7ad916fd7fa6` |
| `hr-hr`                     | 具有区域设置的容器映像 `hr-HR` 。 | `sha256:ab849cd2eeea682f8958bba8986fe90f0f7bb3b447512a10cf464e8e1ce4ea5` |
| `hu-hu`                     | 具有区域设置的容器映像 `hu-HU` 。 | `sha256:30f239b155d91523442cf74a1f2732304fa2b50ae7b786833bb6a020b982621` |
| `it-it`                     | 具有区域设置的容器映像 `it-IT` 。 | `sha256:288f95413870eb9d33bf1dabfa6fbd6b55b0faa52e4d5face3171d1dd4ddbdd` |
| `ja-jp`                     | 具有区域设置的容器映像 `ja-JP` 。 | `sha256:e3ab37a80c215dec565eca212f57eb81887fc2894452868dff92e3bd42c4bb9` |
| `ko-kr`                     | 具有区域设置的容器映像 `ko-KR` 。 | `sha256:c1208b8459333b606af516cd7806e9d4d5e002247bb1225e1f246563b356890` |
| `lt-lt`                     | 具有区域设置的容器映像 `lt-LT` 。 | `sha256:8dec331161d3c29fc65ba6651fcc6cfe69fa314519f408b5f9f8eb27da09830` |
| `lv-lv`                     | 具有区域设置的容器映像 `lv-LV` 。 | `sha256:7cf31282910b339666bb2b0a555caa7fc6ae414eea4423a41f35c3527f83235` |
| `mr-in`                     | 具有区域设置的容器映像 `mr-IN` 。 | `sha256:9cb012bd58ef7723d4905d6fa3c1fde96e33c354b3d96d4e3ff69cf6e1bfe3a` |
| `mt-mt`                     | 具有区域设置的容器映像 `mt-MT` 。 | `sha256:a0094c032ea555b168ec5751ab3257337d902d526e9ae335671fb751a352378` |
| `nb-no`                     | 具有区域设置的容器映像 `nb-NO` 。 | `sha256:6bbc326e20a6a785b1ca33143b42a060858efb67b863a267d6efb7aebb48f87` |
| `nl-nl`                     | 具有区域设置的容器映像 `nl-NL` 。 | `sha256:94b4ddf4cc80fa666e422f8416aea3f98ebe4842dfe9b1f4bfea7c47eb61127` |
| `pl-pl`                     | 具有区域设置的容器映像 `pl-PL` 。 | `sha256:58e5f78bf772c3c8cbd5f0c5d6e67f5348e04e3f893d84738a2a3e964bab256` |
| `pt-br`                     | 具有区域设置的容器映像 `pt-BR` 。 | `sha256:f500ef956bd28807f40df1f9f0520e437c5084f61a3be6d1379e746887d5b7c` |
| `pt-pt`                     | 具有区域设置的容器映像 `pt-PT` 。 | `sha256:c841d2dbe5f40adf6039242c106985febb1a44212feb55d9769fe31134ec116` |
| `ro-ro`                     | 具有区域设置的容器映像 `ro-RO` 。 | `sha256:93271c39c0a134e987a069c2a65289acff9869ae0d90fdcb39928c9ef0fd86b` |
| `ru-ru`                     | 具有区域设置的容器映像 `ru-RU` 。 | `sha256:8d6b3c600e56cc96813b8c14b7916c5539a20ba561dc1c6d5bbef6285d6eef6` |
| `sk-sk`                     | 具有区域设置的容器映像 `sk-SK` 。 | `sha256:6d604092cc6c964663a1c97d91c8f1c8cf4b46d07427d03f7041c0cc55eb521` |
| `sl-si`                     | 具有区域设置的容器映像 `sl-SI` 。 | `sha256:f237ed58fedefcc749e74be1258cc70e5a690ee6c5a6b6388bd24075faa61da` |
| `sv-se`                     | 具有区域设置的容器映像 `sv-SE` 。 | `sha256:da4233e6658b00eefdadb9d4acd889c6550a5e2a4a7af7a9f915c878abd4c9c` |
| `ta-in`                     | 具有区域设置的容器映像 `ta-IN` 。 | `sha256:22b77606d25e9c2f52bf3cad6218782b4719f6a9dcfadc770468d266758a56c` |
| `te-in`                     | 具有区域设置的容器映像 `te-IN` 。 | `sha256:7f4d11372862ca1d65fc9b868e2d775701b8e6eabd786c90c4e9ab82ba86e88` |
| `th-th`                     | 具有区域设置的容器映像 `th-TH` 。 | `sha256:69033bcd7c0f59d31bafec6c2b7a9ff343928cdd58c16105415c291d555d37b` |
| `tr-tr`                     | 具有区域设置的容器映像 `tr-TR` 。 | `sha256:4b7d339846a0d371dfe25aa2e626f131003c01329c9a1da468eb3703ef176ea` |
| `zh-cn`                     | 具有区域设置的容器映像 `zh-CN` 。 | `sha256:a428459830fb766083212f71c5638a65ce30d8dd84f6c624ae22768e8a76976` |
| `zh-hk`                     | 具有区域设置的容器映像 `zh-HK` 。 | `sha256:7a2903462b67336a6ce4c8e2faac42052f0a4392d1d5eb3839758cc8d0429f1` |
| `zh-tw`                     | 具有区域设置的容器映像 `zh-TW` 。 | `sha256:30fd2b3660e047d24a46fbba14ba282f15bc0339ec93f49afd0d02ff4069146` |


# <a name="previous-version"></a>[先前版本](#tab/previous)

发行说明 `2.6.0-amd64-<locale>` ：

**功能**
* 升级到最新模型并完全迁移到 .NET 3。1
* 支持 phraselist v2
* 短语列表在以下区域设置中受支持：
    * en au
    * en-ca
    * en-gb
    * en
    * zh-cn
    * zh-cn
* 支持新的区域设置 `cs-CZ` 
    * 当前不支持大写和标点。

**修复项**
* 修复了 Diarization 模式下置信分数始终为1的问题
* 已迁移使用 TextAnalytics 3.0 API

请注意，由于包含短语列表，此容器映像的大小已增加。 

发行说明 `2.5.0-amd64-<locale>` ：

**功能**
* 支持 Azure 美国政府云

**修复项**
* 修复了在 Diarization 模式下作为非根用户运行的问题

| 映像标记                  | 说明                                    |
|-----------------------------|:-----------------------------------------|
| `2.6.0-amd64-<locale>`      | `<locale>`使用下面列出的一个可用区域设置替换。 例如，`2.6.0-amd64-en-us`。 |
| `2.5.0-amd64-<locale>`      | `<locale>`使用下面列出的一个可用区域设置替换。 例如，`2.5.0-amd64-en-us`。 |


此容器具有以下可用的区域设置。

| V 2.6.0 的区域设置           | 备注                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | 具有区域设置的容器映像 `ar-AE` 。 |
| `ar-eg`                     | 具有区域设置的容器映像 `ar-EG` 。 |
| `ar-kw`                     | 具有区域设置的容器映像 `ar-KW` 。 |
| `ar-qa`                     | 具有区域设置的容器映像 `ar-QA` 。 |
| `ar-sa`                     | 具有区域设置的容器映像 `ar-SA` 。 |
| `ca-es`                     | 具有区域设置的容器映像 `ca-ES` 。 |
| `cs-cz`                     | 具有区域设置的容器映像 `cs-CZ` 。 |
| `da-dk`                     | 具有区域设置的容器映像 `da-DK` 。 |
| `de-de`                     | 具有区域设置的容器映像 `de-DE` 。 |
| `en-au`                     | 具有区域设置的容器映像 `en-AU` 。 |
| `en-ca`                     | 具有区域设置的容器映像 `en-CA` 。 |
| `en-gb`                     | 具有区域设置的容器映像 `en-GB` 。 |
| `en-in`                     | 具有区域设置的容器映像 `en-IN` 。 |
| `en-nz`                     | 具有区域设置的容器映像 `en-NZ` 。 |
| `en-us`                     | 具有区域设置的容器映像 `en-US` 。 |
| `es-es`                     | 具有区域设置的容器映像 `es-ES` 。 |
| `es-mx`                     | 具有区域设置的容器映像 `es-MX` 。 |
| `fi-fi`                     | 具有区域设置的容器映像 `fi-FI` 。 |
| `fr-ca`                     | 具有区域设置的容器映像 `fr-CA` 。 |
| `fr-fr`                     | 具有区域设置的容器映像 `fr-FR` 。 |
| `gu-in`                     | 具有区域设置的容器映像 `gu-IN` 。 |
| `hi-in`                     | 具有区域设置的容器映像 `hi-IN` 。 |
| `it-it`                     | 具有区域设置的容器映像 `it-IT` 。 |
| `ja-jp`                     | 具有区域设置的容器映像 `ja-JP` 。 |
| `ko-kr`                     | 具有区域设置的容器映像 `ko-KR` 。 |
| `mr-in`                     | 具有区域设置的容器映像 `mr-IN` 。 |
| `nb-no`                     | 具有区域设置的容器映像 `nb-NO` 。 |
| `nl-nl`                     | 具有区域设置的容器映像 `nl-NL` 。 |
| `pl-pl`                     | 具有区域设置的容器映像 `pl-PL` 。 |
| `pt-br`                     | 具有区域设置的容器映像 `pt-BR` 。 |
| `pt-pt`                     | 具有区域设置的容器映像 `pt-PT` 。 |
| `ru-ru`                     | 具有区域设置的容器映像 `ru-RU` 。 |
| `sv-se`                     | 具有区域设置的容器映像 `sv-SE` 。 |
| `ta-in`                     | 具有区域设置的容器映像 `ta-IN` 。 |
| `te-in`                     | 具有区域设置的容器映像 `te-IN` 。 |
| `th-th`                     | 具有区域设置的容器映像 `th-TH` 。 |
| `tr-tr`                     | 具有区域设置的容器映像 `tr-TR` 。 |
| `zh-cn`                     | 具有区域设置的容器映像 `zh-CN` 。 |
| `zh-hk`                     | 具有区域设置的容器映像 `zh-HK` 。 |
| `zh-tw`                     | 具有区域设置的容器映像 `zh-TW` 。 |

| V 2.5.0 的区域设置           | 备注                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | 具有区域设置的容器映像 `ar-AE` 。 |
| `ar-eg`                     | 具有区域设置的容器映像 `ar-EG` 。 |
| `ar-kw`                     | 具有区域设置的容器映像 `ar-KW` 。 |
| `ar-qa`                     | 具有区域设置的容器映像 `ar-QA` 。 |
| `ar-sa`                     | 具有区域设置的容器映像 `ar-SA` 。 |
| `ca-es`                     | 具有区域设置的容器映像 `ca-ES` 。 |
| `da-dk`                     | 具有区域设置的容器映像 `da-DK` 。 |
| `de-de`                     | 具有区域设置的容器映像 `de-DE` 。 |
| `en-au`                     | 具有区域设置的容器映像 `en-AU` 。 |
| `en-ca`                     | 具有区域设置的容器映像 `en-CA` 。 |
| `en-gb`                     | 具有区域设置的容器映像 `en-GB` 。 |
| `en-in`                     | 具有区域设置的容器映像 `en-IN` 。 |
| `en-nz`                     | 具有区域设置的容器映像 `en-NZ` 。 |
| `en-us`                     | 具有区域设置的容器映像 `en-US` 。 |
| `es-es`                     | 具有区域设置的容器映像 `es-ES` 。 |
| `es-mx`                     | 具有区域设置的容器映像 `es-MX` 。 |
| `fi-fi`                     | 具有区域设置的容器映像 `fi-FI` 。 |
| `fr-ca`                     | 具有区域设置的容器映像 `fr-CA` 。 |
| `fr-fr`                     | 具有区域设置的容器映像 `fr-FR` 。 |
| `gu-in`                     | 具有区域设置的容器映像 `gu-IN` 。 |
| `hi-in`                     | 具有区域设置的容器映像 `hi-IN` 。 |
| `it-it`                     | 具有区域设置的容器映像 `it-IT` 。 |
| `ja-jp`                     | 具有区域设置的容器映像 `ja-JP` 。 |
| `ko-kr`                     | 具有区域设置的容器映像 `ko-KR` 。 |
| `mr-in`                     | 具有区域设置的容器映像 `mr-IN` 。 |
| `nb-no`                     | 具有区域设置的容器映像 `nb-NO` 。 |
| `nl-nl`                     | 具有区域设置的容器映像 `nl-NL` 。 |
| `pl-pl`                     | 具有区域设置的容器映像 `pl-PL` 。 |
| `pt-br`                     | 具有区域设置的容器映像 `pt-BR` 。 |
| `pt-pt`                     | 具有区域设置的容器映像 `pt-PT` 。 |
| `ru-ru`                     | 具有区域设置的容器映像 `ru-RU` 。 |
| `sv-se`                     | 具有区域设置的容器映像 `sv-SE` 。 |
| `ta-in`                     | 具有区域设置的容器映像 `ta-IN` 。 |
| `te-in`                     | 具有区域设置的容器映像 `te-IN` 。 |
| `th-th`                     | 具有区域设置的容器映像 `th-TH` 。 |
| `tr-tr`                     | 具有区域设置的容器映像 `tr-TR` 。 |
| `zh-cn`                     | 具有区域设置的容器映像 `zh-CN` 。 |
| `zh-hk`                     | 具有区域设置的容器映像 `zh-HK` 。 |
| `zh-tw`                     | 具有区域设置的容器映像 `zh-TW` 。 |

---

## <a name="text-to-speech"></a>文本转语音

可在容器注册表 "联合" 中找到 [文本到语音的][sp-tts] 容器图像 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services/speechservices/` 存储库中，名为 `text-to-speech`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/speechservices/text-to-speech`。

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/text-to-speech/tags/list)的完整列表。


# <a name="latest-version"></a>[最新版本](#tab/current)

发行说明 `1.9.0-amd64-<locale-and-voice>` ：

* 定期每月发布

| 映像标记                                  | 说明                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `latest`                                    | 具有 `en-US` 区域设置和 `en-US-AriaRUS` 语音的容器映像。                                            | 
| `1.9.0-amd64-<locale-and-voice>`            | `<locale>`使用下面列出的一个可用区域设置替换。 例如，`1.9.0-amd64-en-us-ariarus`。  |


| V 1.9.0 的区域设置                          | 备注                                                                      | 摘要                         |
|---------------------------------------------|:---------------------------------------------------------------------------|:-------------------------------|
| `ar-eg-hoda`                                | 具有 `ar-EG` 区域设置和 `ar-EG-Hoda` 语音的容器映像。            | `sha256:2b19cfd2212d6517b286aa18617d2f9d1dd1520078b559cbbf9240599270d10` | 
| `ar-sa-naayf`                               | 具有 `ar-SA` 区域设置和 `ar-SA-Naayf` 语音的容器映像。           | `sha256:6063aae5fb15c62b234cf945220916516a06ca81354c5311dee02af4d8cb0d3` |
| `bg-bg-ivan`                                | 具有 `bg-BG` 区域设置和 `bg-BG-Ivan` 语音的容器映像。            | `sha256:c6786916464755e64ffa64e69e8f3e7ef16115bac00bb6ea1e45368c42c58d1` |
| `ca-es-herenarus`                           | 具有 `ca-ES` 区域设置和 `ca-ES-HerenaRUS` 语音的容器映像。       | `sha256:2a8a1accbf99e2746c9345b77e2f261e0111227312c402cc2e1cd8760cdc82a` |
| `cs-cz-jakub`                               | 具有 `cs-CZ` 区域设置和 `cs-CZ-Jakub` 语音的容器映像。           | `sha256:3e464356bb08c9c966af2b28a88ccafd591aecd2e37a0fedb356bd443720e8d` |
| `da-dk-hellerus`                            | 具有 `da-DK` 区域设置和 `da-DK-HelleRUS` 语音的容器映像。        | `sha256:b85c43080804103673ff99dddea644a516c4103e8b1f11fa3dd34857492cd40` |
| `de-at-michael`                             | 具有 `de-AT` 区域设置和 `de-AT-Michael` 语音的容器映像。         | `sha256:87b57ee61f964e4d72e75d860c499fa3b3d8dbda6a96c97d696beb20aa8b2a9` |
| `de-ch-karsten`                             | 具有 `de-CH` 区域设置和 `de-CH-Karsten` 语音的容器映像。         | `sha256:ab1385b9746f4f054204302b9d564a433ae03748021b8ed71b4a3a224af1e9b` |
| `de-de-heddarus`                            | 具有 `de-DE` 区域设置和 `de-DE-Hedda` 语音的容器映像。           | `sha256:82185a710c87f9dde678d88036867559ab3bf5f08f234d60d1548d3e106db57` |
| `de-de-hedda`                               | 具有 `de-DE` 区域设置和 `de-DE-Hedda` 语音的容器映像。           | `sha256:82185a710c87f9dde678d88036867559ab3bf5f08f234d60d1548d3e106db57` |
| `de-de-stefan-apollo`                       | 具有 `de-DE` 区域设置和 `de-DE-Stefan-Apollo` 语音的容器映像。   | `sha256:56a1c63e7e6a0f5623ddc1f6a44ac6e51471d073e02e14e8c8b1e577930d816` |
| `el-gr-stefanos`                            | 具有 `el-GR` 区域设置和 `el-GR-Stefanos` 语音的容器映像。        | `sha256:ccbbb09f29ff8f276e246037183c7a3e9a3eb5bf33a942b22205cce3c6857f2` |
| `en-au-catherine`                           | 具有 `en-AU` 区域设置和 `en-AU-Catherine` 语音的容器映像。       | `sha256:0c7374890f963e1ae9507e89dc9965a94723bd57802826c0677cd5262189783` |
| `en-au-hayleyrus`                           | 具有 `en-AU` 区域设置和 `en-AU-HayleyRUS` 语音的容器映像。       | `sha256:7430bf8eace8294ca085f36ea56399261b2b4f69027e86649e8f3868fc3d811` |
| `en-ca-heatherrus`                          | 具有 `en-CA` 区域设置和 `en-CA-HeatherRUS` 语音的容器映像。      | `sha256:0166ce1de3d669ea4ad80738c63369b7032125a54ecabade07241d740a94cfe` |
| `en-ca-linda`                               | 具有 `en-CA` 区域设置和 `en-CA-Linda` 语音的容器映像。           | `sha256:50bed6a7bde9b793d307bcc3ace4c0f28d4a33c7a4dad9b3a394dc39a3e1c28` |
| `en-gb-george-apollo`                       | 具有 `en-GB` 区域设置和 `en-GB-George-Apollo` 语音的容器映像。   | `sha256:50b800c0018a39609ddb1cee1b10062bf38a907644c393d20786db7c3ade748` |
| `en-gb-hazelrus`                            | 具有 `en-GB` 区域设置和 `en-GB-HazelRUS` 语音的容器映像。        | `sha256:2aa79394dfeac8cec0cc1704a5199949cfccf347fe61161d02c7000c4ffcfa6` |
| `en-gb-susan-apollo`                        | 具有 `en-GB` 区域设置和 `en-GB-Susan-Apollo` 语音的容器映像。    | `sha256:7a3174b3aae5f10241e731d392b56f124808cdd506f881ced919ced73d836c0` |
| `en-ie-sean`                                | 具有 `en-IE` 区域设置和 `en-IE-Sean` 语音的容器映像。            | `sha256:2457202fadb2354fc8d3666432096bd87c07760a4e3f4dbcc49853fff658577` |
| `en-in-heera-apollo`                        | 具有 `en-IN` 区域设置和 `en-IN-Heera-Apollo` 语音的容器映像。    | `sha256:e4068cd7ca4272ea94819e2ba8743d2a76c8710b162db5e9ecbde6c92c12877` |
| `en-in-priyarus`                            | 具有 `en-IN` 区域设置和 `en-IN-PriyaRUS` 语音的容器映像。        | `sha256:9d63a0ed53ac06178ab84588551421c0e1d04b8bad3321410ebb99c3ca2a9e8` |
| `en-in-ravi-apollo`                         | 具有 `en-IN` 区域设置和 `en-IN-Ravi-Apollo` 语音的容器映像。     | `sha256:67049c9ce591336655943f5030afcfdaa150a8aace7b372425a69cc33a6b7b9` |
| `en-us-aria24krus`                          | 具有 `en-US` 区域设置和 `en-US-Aria24kRUS` 语音的容器映像。      | `sha256:a95acf6874bf3df7ae8e96be779f80cb5405d21250227b0c4b3ddbcb3014082` |
| `en-us-ariarus`                             | 具有 `en-US` 区域设置和 `en-US-AriaRUS` 语音的容器映像。         | `sha256:a95acf6874bf3df7ae8e96be779f80cb5405d21250227b0c4b3ddbcb3014082` |
| `en-us-benjaminrus`                         | 具有 `en-US` 区域设置和 `en-US-BenjaminRUS` 语音的容器映像。     | `sha256:93cd49adaaa2a1bdfb06ab655be164ae66f206cb7c03a2cbd59e5fba70610ab` |
| `en-us-guy24krus`                           | 具有 `en-US` 区域设置和 `en-US-Guy24kRUS` 语音的容器映像。       | `sha256:7b788bfcaae4c63c274ca15924bfd861cfcafd5fec13f685d80babc25b2949d` |
| `en-us-zirarus`                             | 具有 `en-US` 区域设置和 `en-US-ZiraRUS` 语音的容器映像。         | `sha256:bfc87a77df5695ad43481348500fba8f6a7b495708fba200706049469b5ba97` |
| `es-es-helenarus`                           | 具有 `es-ES` 区域设置和 `es-ES-HelenaRUS` 语音的容器映像。       | `sha256:0b6c17aca75efb64aa9bfc0d83303038fe58d4b2fb1fc94c9380a4335b80796` |
| `es-es-laura-apollo`                        | 具有 `es-ES` 区域设置和 `es-ES-Laura-Apollo` 语音的容器映像。    | `sha256:d6fcffc944c37a2dd0de29c39b82f3f8cce3a95ad925d2814ed7538335d5d4f` |
| `es-es-pablo-apollo`                        | 具有 `es-ES` 区域设置和 `es-ES-Pablo-Apollo` 语音的容器映像。    | `sha256:a460bc53d9083d3c3770129995cf96cc1069ae4e8101f1739d304fe210f0af0` |
| `es-mx-hildarus`                            | 具有 `es-MX` 区域设置和 `es-MX-HildaRUS` 语音的容器映像。        | `sha256:5b7578fc5b00158dfa674d95a3f1d57f22eb285e8333b4006d1fe1808bda7ba` |
| `es-mx-raul-apollo`                         | 具有 `es-MX` 区域设置和 `es-MX-Raul-Apollo` 语音的容器映像。     | `sha256:03922fb017783c86d788c72e01c7ede440f8f3c913c86cab19bad4dfc2e4a2b` |
| `fi-fi-heidirus`                            | 具有 `fi-FI` 区域设置和 `fi-FI-HeidiRUS` 语音的容器映像。        | `sha256:146c1f98d6fa061016eba41db6e7b654eef222d37f35406d4b43477bb2ff897` |
| `fr-ca-caroline`                            | 具有 `fr-CA` 区域设置和 `fr-CA-Caroline` 语音的容器映像。        | `sha256:1ee2e53f12ad1c72665d2aef64e9d4a7f9ea05670cad84dcae5e75409494f32` |
| `fr-ca-harmonierus`                         | 具有 `fr-CA` 区域设置和 `fr-CA-HarmonieRUS` 语音的容器映像。     | `sha256:a21d25d3ac699af4e9ba9194aadd9b45f35fd9205224f3429a4c7da41fc38fe` |
| `fr-ch-guillaume`                           | 具有 `fr-CH` 区域设置和 `fr-CH-Guillaume` 语音的容器映像。       | `sha256:216125a9bd89a95d3c4dc2d7e031398659427b3aa7d4663d23a65737972e42b` |
| `fr-fr-hortenserus`                         | 具有 `fr-FR` 区域设置和 `fr-FR-HortenseRUS` 语音的容器映像。     | `sha256:795a698120eecbd80c48e738f73300739c1698ca859130ddb4236317bcdf70f` |
| `fr-fr-julie-apollo`                        | 具有 `fr-FR` 区域设置和 `fr-FR-Julie-Apollo` 语音的容器映像。    | `sha256:f6eb70d523c435c2e3a713b32a8af4a781df7ec043caad2fc7f458ee341eb2f` |
| `fr-fr-paul-apollo`                         | 具有 `fr-FR` 区域设置和 `fr-FR-Paul-Apollo` 语音的容器映像。     | `sha256:28864c662a20f459b3051b1da2967a605e06267e6408285f7c2552748cf4eed` |
| `he-il-asaf`                                | 具有 `he-IL` 区域设置和 `he-IL-Asaf` 语音的容器映像。            | `sha256:eaa834bac6b69abef096b36a8baead741db78fe438af3d30f60abde3631d639` |
| `hi-in-hemant`                              | 具有 `hi-IN` 区域设置和 `hi-IN-Hemant` 语音的容器映像。          | `sha256:cfea0fa7cce9cc512f2fbb8b76f1c00fe5c32fad853c90b15934cf4ee6262fa` |
| `hi-in-kalpana-apollo`                      | 具有 `hi-IN` 区域设置和 `hi-IN-Kalpana-Apollo` 语音的容器映像。  | `sha256:afbd6cc0413f3a3c9f6df044b6df6d9dac9e8e888c2cb619fefbdc3e105c644` |
| `hi-in-kalpana`                             | 具有 `hi-IN` 区域设置和 `hi-IN-Kalpana` 语音的容器映像。         | `sha256:afbd6cc0413f3a3c9f6df044b6df6d9dac9e8e888c2cb619fefbdc3e105c644` |
| `hr-hr-matej`                               | 具有 `hr-HR` 区域设置和 `hr-HR-Matej` 语音的容器映像。           | `sha256:86683597c62752b4d769b69e5294979fafd4c277aaef1536e1cb19f9f06c0bf` |
| `hu-hu-szabolcs`                            | 具有 `hu-HU` 区域设置和 `hu-HU-Szabolcs` 语音的容器映像。        | `sha256:aa64eed28ca2ad060e2e02188e0401bf34e4caf7e2182b70a30ce33b3c11c9c` |
| `id-id-andika`                              | 具有 `id-ID` 区域设置和 `id-ID-Andika` 语音的容器映像。          | `sha256:0e1394d231a57a1df8163ccb634dc2ef2f8103b10608a40ab3efc5c0fbe9ded` |
| `it-it-cosimo-apollo`                       | 具有 `it-IT` 区域设置和 `it-IT-Cosimo-Apollo` 语音的容器映像。   | `sha256:eef97f2817fc24405823a5fe4e825244db32279b44c0e6631e8ad9a5c1acf40` |
| `it-it-luciarus`                            | 具有 `it-IT` 区域设置和 `it-IT-LuciaRUS` 语音的容器映像。        | `sha256:ebc331b0685f482d2f55619fa81fd451fd7c8f107f9cd7ad159bc6213ae4e33` |
| `ja-jp-ayumi-apollo`                        | 具有 `ja-JP` 区域设置和 `ja-JP-Ayumi-Apollo` 语音的容器映像。    | `sha256:e9cb7dfd2eec154c8f3d530c16b66e8558c5955a2edaede69740067f00e43cf` |
| `ja-jp-harukarus`                           | 具有 `ja-JP` 区域设置和 `ja-JP-HarukaRUS` 语音的容器映像。       | `sha256:93ce2ef6177c0d8ac70b61df8b11fcbcdfd3c0be0cc51cd8644f26679a741c2` |
| `ja-jp-ichiro-apollo`                       | 具有 `ja-JP` 区域设置和 `ja-JP-Ichiro-Apollo` 语音的容器映像。   | `sha256:6a18bae69ac63b42ba992b8b74d8d31d91ca984d61b5f62f38be988cf38645e` |
| `ko-kr-heamirus`                            | 具有 `ko-KR` 区域设置和 `ko-KR-HeamiRUS` 语音的容器映像。        | `sha256:7a48252d4ada2af43f9266a70113426d330bac192348cbdc929022295a0e727` |
| `ms-my-rizwan`                              | 具有 `ms-MY` 区域设置和 `ms-MY-Rizwan` 语音的容器映像。          | `sha256:90e2ecac14f8e960934fd013d208fc2a0afe1bfff037d5648d422bda8d8a76e` |
| `nb-no-huldarus`                            | 具有 `nb-NO` 区域设置和 `nb-NO-HuldaRUS` 语音的容器映像。        | `sha256:217b61bd6244b5effda8f12a2c563ce1b4572e9c5b8a08df143665f9ff754e4` |
| `nl-nl-hannarus`                            | 具有 `nl-NL` 区域设置和 `nl-NL-HannaRUS` 语音的容器映像。        | `sha256:fbff48dfc9dfadadf377867b28f6e3a3bd605e59da20f77a531efcc7d85d16e` |
| `pl-pl-paulinarus`                          | 具有 `pl-PL` 区域设置和 `pl-PL-PaulinaRUS` 语音的容器映像。      | `sha256:856a033a09925773fa4b4531e199ab7c03c537f366acecbda60f8d21735725e` |
| `pt-br-daniel-apollo`                       | 具有 `pt-BR` 区域设置和 `pt-BR-Daniel-Apollo` 语音的容器映像。   | `sha256:2d1ec975f1aee56a6fc6039d154fb3f2fbeb4636f7078c5dfe99aeddb6a3634` |
| `pt-br-heloisarus`                          | 具有 `pt-BR` 区域设置和 `pt-BR-HeloisaRUS` 语音的容器映像。      | `sha256:b7d629f37ab3305274764264dc08fab5236e60ef18d40e987618115db67ce44` |
| `pt-pt-heliarus`                            | 具有 `pt-PT` 区域设置和 `pt-PT-HeliaRUS` 语音的容器映像。        | `sha256:8b380ae7e4aac9d4ada4d15fa9e667387bc9ca038796d9b6999953bfbc97259` |
| `ro-ro-andrei`                              | 具有 `ro-RO` 区域设置和 `ro-RO-Andrei` 语音的容器映像。          | `sha256:b00ca7f1411169a5baf7263a8d7e5eed1a72084d9489eaf458429dfc338564a` |
| `ru-ru-ekaterinarus`                        | 具有 `ru-RU` 区域设置和 `ru-RU-EkaterinaRUS` 语音的容器映像。    | `sha256:31c588c31e3ac67305af66091e7756dfc4ca454317d0228116ea0b2fedf5d71` |
| `ru-ru-irina-apollo`                        | 具有 `ru-RU` 区域设置和 `ru-RU-Irina-Apollo` 语音的容器映像。    | `sha256:e76437f8da7c279b38d2643defc997a13b4a364e9a212895cdb33a9a3f6457f` |
| `ru-ru-pavel-apollo`                        | 具有 `ru-RU` 区域设置和 `ru-RU-Pavel-Apollo` 语音的容器映像。    | `sha256:461c1efa6cce0b10a87f338bc637aca76aef8458061a688870fb3343d682da0` |
| `sk-sk-filip`                               | 具有 `sk-SK` 区域设置和 `sk-SK-Filip` 语音的容器映像。           | `sha256:7fb0cfab4c0fe2913eb20f28a25c6663015d62f82e7e7864d9f7fac2d27697b` |
| `sl-si-lado`                                | 具有 `sl-SI` 区域设置和 `sl-SI-Lado` 语音的容器映像。            | `sha256:5336173d410e10ffeb5dc211a583887e33754319c757914955057d398dfbb0a` |
| `sv-se-hedvigrus`                           | 具有 `sv-SE` 区域设置和 `sv-SE-HedvigRUS` 语音的容器映像。       | `sha256:5dc8cdcc3054386bf69596707d9d261d4db5bfd09f1882ceb4e29238a34b24e` |
| `ta-in-valluvar`                            | 具有 `ta-IN` 区域设置和 `ta-IN-Valluvar` 语音的容器映像。        | `sha256:74ea485f23e4c1fe0029e06894860aa0188c36c0e14ea3584a06d4216ccef56` |
| `te-in-chitra`                              | 具有 `te-IN` 区域设置和 `te-IN-Chitra` 语音的容器映像。          | `sha256:ff2977a98ef691da543db08be9cfe04d7fc3bf8f78b29310c163e47303b2ddd` |
| `th-th-pattara`                             | 具有 `th-TH` 区域设置和 `th-TH-Pattara` 语音的容器映像。         | `sha256:ba7e2c0e5e75d9f2b52aa50c97728616c43e81f48c15e24665e4c2ea5770a8f` |
| `tr-tr-sedarus`                             | 具有 `tr-TR` 区域设置和 `tr-TR-SedaRUS` 语音的容器映像。         | `sha256:375a8ceae89ea1f0dda551feff30ae3679231189b527992edbc49988d042d66` |
| `vi-vn-an`                                  | 具有 `vi-VN` 区域设置和 `vi-VN-An` 语音的容器映像。              | `sha256:b6f82148295b38b4039c45c48695ec50b4e97cd02b18d49c39bf9fca3bec958` |
| `zh-cn-huihuirus`                           | 具有 `zh-CN` 区域设置和 `zh-CN-HuihuiRUS` 语音的容器映像。       | `sha256:3e773931f3adaac92cba43773a241692a2b471ebe73ec51c475df8ff63b7ee1` |
| `zh-cn-kangkang-apollo`                     | 具有 `zh-CN` 区域设置和 `zh-CN-Kangkang-Apollo` 语音的容器映像。 | `sha256:05fc0d5075a1094caf70d98b4a9469952be52cb6eb4d9f7b9ff4ae961100c7b` |
| `zh-cn-yaoyao-apollo`                       | 具有 `zh-CN` 区域设置和 `zh-CN-Yaoyao-Apollo` 语音的容器映像。   | `sha256:d7613bcefc48e85b9d6f07c8cd223c16d4958bcf7f24087575250e97c593ac1` |
| `zh-hk-danny-apollo`                        | 具有 `zh-HK` 区域设置和 `zh-HK-Danny-Apollo` 语音的容器映像。    | `sha256:efe22bc123dac9312dcaeb859a377d81f61fbb25ef46e4678d36ec6bebc5d32` |
| `zh-hk-tracy-apollo`                        | 具有 `zh-HK` 区域设置和 `zh-HK-Tracy-Apollo` 语音的容器映像。    | `sha256:802c60bc65012c03ffe96268dca79b8c6dcd0c5cc6180ec271c50ef5c9ba132` |
| `zh-hk-tracyrus`                            | 具有 `zh-HK` 区域设置和 `zh-HK-TracyRUS` 语音的容器映像。        | `sha256:802c60bc65012c03ffe96268dca79b8c6dcd0c5cc6180ec271c50ef5c9ba132` |
| `zh-tw-hanhanrus`                           | 具有 `zh-TW` 区域设置和 `zh-TW-HanHanRUS` 语音的容器映像。       | `sha256:95d58922463d577d4c4722ab722a5768af35fb62236d47f6709717dea758909` |
| `zh-tw-yating-apollo`                       | 具有 `zh-TW` 区域设置和 `zh-TW-Yating-Apollo` 语音的容器映像。   | `sha256:33eec6e3aaaedafaf3969746eeaf97a1760e763505decfe2abaa03f5054bfd2` |
| `zh-tw-zhiwei-apollo`                       | 具有 `zh-TW` 区域设置和 `zh-TW-Zhiwei-Apollo` 语音的容器映像。   | `sha256:456db2898b2e5a9c30b7071ce6ea3f141438cbf1aa4899c7ffccfc2f0dde5bd` |


# <a name="previous-version"></a>[先前版本](#tab/previous)

发行说明 `1.8.0-amd64-<locale-and-voice>` ：

**功能**

* 完全迁移到 .NET 3。1

发行说明 `1.7.0-amd64-<locale-and-voice>` ：

**功能**

* 将组件升级到 .NET 3。1

| 映像标记                                  | 说明                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `1.8.0-amd64-<locale-and-voice>`            | `<locale>`使用下面列出的一个可用区域设置替换。 例如，`1.8.0-amd64-en-us-ariarus`。  |
| `1.7.0-amd64-<locale-and-voice>`            | 第一 GA 版本。 `<locale>`使用下面列出的一个可用区域设置替换。 例如，`1.7.0-amd64-en-us-ariarus`。  |


| V 1.8.0 的区域设置                          | 说明                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | 具有 `ar-EG` 区域设置和 `ar-EG-Hoda` 语音的容器映像。            |
| `ar-sa-naayf`                               | 具有 `ar-SA` 区域设置和 `ar-SA-Naayf` 语音的容器映像。           |
| `bg-bg-ivan`                                | 具有 `bg-BG` 区域设置和 `bg-BG-Ivan` 语音的容器映像。            |
| `ca-es-herenarus`                           | 具有 `ca-ES` 区域设置和 `ca-ES-HerenaRUS` 语音的容器映像。       |
| `cs-cz-jakub`                               | 具有 `cs-CZ` 区域设置和 `cs-CZ-Jakub` 语音的容器映像。           |
| `da-dk-hellerus`                            | 具有 `da-DK` 区域设置和 `da-DK-HelleRUS` 语音的容器映像。        |
| `de-at-michael`                             | 具有 `de-AT` 区域设置和 `de-AT-Michael` 语音的容器映像。         |
| `de-ch-karsten`                             | 具有 `de-CH` 区域设置和 `de-CH-Karsten` 语音的容器映像。         |
| `de-de-hedda`                               | 具有 `de-DE` 区域设置和 `de-DE-Hedda` 语音的容器映像。           |
| `de-de-heddarus`                            | 具有 `de-DE` 区域设置和 `de-DE-Hedda` 语音的容器映像。           |
| `de-de-stefan-apollo`                       | 具有 `de-DE` 区域设置和 `de-DE-Stefan-Apollo` 语音的容器映像。   |
| `el-gr-stefanos`                            | 具有 `el-GR` 区域设置和 `el-GR-Stefanos` 语音的容器映像。        |
| `en-au-catherine`                           | 具有 `en-AU` 区域设置和 `en-AU-Catherine` 语音的容器映像。       |
| `en-au-hayleyrus`                           | 具有 `en-AU` 区域设置和 `en-AU-HayleyRUS` 语音的容器映像。       |
| `en-ca-heatherrus`                          | 具有 `en-CA` 区域设置和 `en-CA-HeatherRUS` 语音的容器映像。      |
| `en-ca-linda`                               | 具有 `en-CA` 区域设置和 `en-CA-Linda` 语音的容器映像。           |
| `en-gb-george-apollo`                       | 具有 `en-GB` 区域设置和 `en-GB-George-Apollo` 语音的容器映像。   |
| `en-gb-hazelrus`                            | 具有 `en-GB` 区域设置和 `en-GB-HazelRUS` 语音的容器映像。        |
| `en-gb-susan-apollo`                        | 具有 `en-GB` 区域设置和 `en-GB-Susan-Apollo` 语音的容器映像。    |
| `en-ie-sean`                                | 具有 `en-IE` 区域设置和 `en-IE-Sean` 语音的容器映像。            |
| `en-in-heera-apollo`                        | 具有 `en-IN` 区域设置和 `en-IN-Heera-Apollo` 语音的容器映像。    |
| `en-in-priyarus`                            | 具有 `en-IN` 区域设置和 `en-IN-PriyaRUS` 语音的容器映像。        |
| `en-in-ravi-apollo`                         | 具有 `en-IN` 区域设置和 `en-IN-Ravi-Apollo` 语音的容器映像。     |
| `en-us-benjaminrus`                         | 具有 `en-US` 区域设置和 `en-US-BenjaminRUS` 语音的容器映像。     |
| `en-us-guy24krus`                           | 具有 `en-US` 区域设置和 `en-US-Guy24kRUS` 语音的容器映像。       |
| `en-us-aria24krus`                          | 具有 `en-US` 区域设置和 `en-US-Aria24kRUS` 语音的容器映像。      |
| `en-us-ariarus`                             | 具有 `en-US` 区域设置和 `en-US-AriaRUS` 语音的容器映像。         |
| `en-us-zirarus`                             | 具有 `en-US` 区域设置和 `en-US-ZiraRUS` 语音的容器映像。         |
| `es-es-helenarus`                           | 具有 `es-ES` 区域设置和 `es-ES-HelenaRUS` 语音的容器映像。       |
| `es-es-laura-apollo`                        | 具有 `es-ES` 区域设置和 `es-ES-Laura-Apollo` 语音的容器映像。    |
| `es-es-pablo-apollo`                        | 具有 `es-ES` 区域设置和 `es-ES-Pablo-Apollo` 语音的容器映像。    |
| `es-mx-hildarus`                            | 具有 `es-MX` 区域设置和 `es-MX-HildaRUS` 语音的容器映像。        |
| `es-mx-raul-apollo`                         | 具有 `es-MX` 区域设置和 `es-MX-Raul-Apollo` 语音的容器映像。     |
| `fi-fi-heidirus`                            | 具有 `fi-FI` 区域设置和 `fi-FI-HeidiRUS` 语音的容器映像。        |
| `fr-ca-caroline`                            | 具有 `fr-CA` 区域设置和 `fr-CA-Caroline` 语音的容器映像。        |
| `fr-ca-harmonierus`                         | 具有 `fr-CA` 区域设置和 `fr-CA-HarmonieRUS` 语音的容器映像。     |
| `fr-ch-guillaume`                           | 具有 `fr-CH` 区域设置和 `fr-CH-Guillaume` 语音的容器映像。       |
| `fr-fr-hortenserus`                         | 具有 `fr-FR` 区域设置和 `fr-FR-HortenseRUS` 语音的容器映像。     |
| `fr-fr-julie-apollo`                        | 具有 `fr-FR` 区域设置和 `fr-FR-Julie-Apollo` 语音的容器映像。    |
| `fr-fr-paul-apollo`                         | 具有 `fr-FR` 区域设置和 `fr-FR-Paul-Apollo` 语音的容器映像。     |
| `he-il-asaf`                                | 具有 `he-IL` 区域设置和 `he-IL-Asaf` 语音的容器映像。            |
| `hi-in-hemant`                              | 具有 `hi-IN` 区域设置和 `hi-IN-Hemant` 语音的容器映像。          |
| `hi-in-kalpana-apollo`                      | 具有 `hi-IN` 区域设置和 `hi-IN-Kalpana-Apollo` 语音的容器映像。  |
| `hi-in-kalpana`                             | 具有 `hi-IN` 区域设置和 `hi-IN-Kalpana` 语音的容器映像。         |
| `hr-hr-matej`                               | 具有 `hr-HR` 区域设置和 `hr-HR-Matej` 语音的容器映像。           |
| `hu-hu-szabolcs`                            | 具有 `hu-HU` 区域设置和 `hu-HU-Szabolcs` 语音的容器映像。        |
| `id-id-andika`                              | 具有 `id-ID` 区域设置和 `id-ID-Andika` 语音的容器映像。          |
| `it-it-cosimo-apollo`                       | 具有 `it-IT` 区域设置和 `it-IT-Cosimo-Apollo` 语音的容器映像。   |
| `it-it-luciarus`                            | 具有 `it-IT` 区域设置和 `it-IT-LuciaRUS` 语音的容器映像。        |
| `ja-jp-ayumi-apollo`                        | 具有 `ja-JP` 区域设置和 `ja-JP-Ayumi-Apollo` 语音的容器映像。    |
| `ja-jp-harukarus`                           | 具有 `ja-JP` 区域设置和 `ja-JP-HarukaRUS` 语音的容器映像。       |
| `ja-jp-ichiro-apollo`                       | 具有 `ja-JP` 区域设置和 `ja-JP-Ichiro-Apollo` 语音的容器映像。   |
| `ko-kr-heamirus`                            | 具有 `ko-KR` 区域设置和 `ko-KR-HeamiRUS` 语音的容器映像。        |
| `ms-my-rizwan`                              | 具有 `ms-MY` 区域设置和 `ms-MY-Rizwan` 语音的容器映像。          |
| `nb-no-huldarus`                            | 具有 `nb-NO` 区域设置和 `nb-NO-HuldaRUS` 语音的容器映像。        |
| `nl-nl-hannarus`                            | 具有 `nl-NL` 区域设置和 `nl-NL-HannaRUS` 语音的容器映像。        |
| `pl-pl-paulinarus`                          | 具有 `pl-PL` 区域设置和 `pl-PL-PaulinaRUS` 语音的容器映像。      |
| `pt-br-daniel-apollo`                       | 具有 `pt-BR` 区域设置和 `pt-BR-Daniel-Apollo` 语音的容器映像。   |
| `pt-br-heloisarus`                          | 具有 `pt-BR` 区域设置和 `pt-BR-HeloisaRUS` 语音的容器映像。      |
| `pt-pt-heliarus`                            | 具有 `pt-PT` 区域设置和 `pt-PT-HeliaRUS` 语音的容器映像。        |
| `ro-ro-andrei`                              | 具有 `ro-RO` 区域设置和 `ro-RO-Andrei` 语音的容器映像。          |
| `ru-ru-ekaterinarus`                        | 具有 `ru-RU` 区域设置和 `ru-RU-EkaterinaRUS` 语音的容器映像。    |
| `ru-ru-irina-apollo`                        | 具有 `ru-RU` 区域设置和 `ru-RU-Irina-Apollo` 语音的容器映像。    |
| `ru-ru-pavel-apollo`                        | 具有 `ru-RU` 区域设置和 `ru-RU-Pavel-Apollo` 语音的容器映像。    |
| `sk-sk-filip`                               | 具有 `sk-SK` 区域设置和 `sk-SK-Filip` 语音的容器映像。           |
| `sl-si-lado`                                | 具有 `sl-SI` 区域设置和 `sl-SI-Lado` 语音的容器映像。            |
| `sv-se-hedvigrus`                           | 具有 `sv-SE` 区域设置和 `sv-SE-HedvigRUS` 语音的容器映像。       |
| `ta-in-valluvar`                            | 具有 `ta-IN` 区域设置和 `ta-IN-Valluvar` 语音的容器映像。        |
| `te-in-chitra`                              | 具有 `te-IN` 区域设置和 `te-IN-Chitra` 语音的容器映像。          |
| `th-th-pattara`                             | 具有 `th-TH` 区域设置和 `th-TH-Pattara` 语音的容器映像。         |
| `tr-tr-sedarus`                             | 具有 `tr-TR` 区域设置和 `tr-TR-SedaRUS` 语音的容器映像。         |
| `vi-vn-an`                                  | 具有 `vi-VN` 区域设置和 `vi-VN-An` 语音的容器映像。              |
| `zh-cn-huihuirus`                           | 具有 `zh-CN` 区域设置和 `zh-CN-HuihuiRUS` 语音的容器映像。       |
| `zh-cn-kangkang-apollo`                     | 具有 `zh-CN` 区域设置和 `zh-CN-Kangkang-Apollo` 语音的容器映像。 |
| `zh-cn-yaoyao-apollo`                       | 具有 `zh-CN` 区域设置和 `zh-CN-Yaoyao-Apollo` 语音的容器映像。   |
| `zh-hk-danny-apollo`                        | 具有 `zh-HK` 区域设置和 `zh-HK-Danny-Apollo` 语音的容器映像。    |
| `zh-hk-tracy-apollo`                        | 具有 `zh-HK` 区域设置和 `zh-HK-Tracy-Apollo` 语音的容器映像。    |
| `zh-hk-tracyrus`                            | 具有 `zh-HK` 区域设置和 `zh-HK-TracyRUS` 语音的容器映像。        |
| `zh-tw-hanhanrus`                           | 具有 `zh-TW` 区域设置和 `zh-TW-HanHanRUS` 语音的容器映像。       |
| `zh-tw-yating-apollo`                       | 具有 `zh-TW` 区域设置和 `zh-TW-Yating-Apollo` 语音的容器映像。   |
| `zh-tw-zhiwei-apollo`                       | 具有 `zh-TW` 区域设置和 `zh-TW-Zhiwei-Apollo` 语音的容器映像。   |

| V 1.7.0 的区域设置                          | 说明                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | 具有 `ar-EG` 区域设置和 `ar-EG-Hoda` 语音的容器映像。            |
| `ar-sa-naayf`                               | 具有 `ar-SA` 区域设置和 `ar-SA-Naayf` 语音的容器映像。           |
| `bg-bg-ivan`                                | 具有 `bg-BG` 区域设置和 `bg-BG-Ivan` 语音的容器映像。            |
| `ca-es-herenarus`                           | 具有 `ca-ES` 区域设置和 `ca-ES-HerenaRUS` 语音的容器映像。       |
| `cs-cz-jakub`                               | 具有 `cs-CZ` 区域设置和 `cs-CZ-Jakub` 语音的容器映像。           |
| `da-dk-hellerus`                            | 具有 `da-DK` 区域设置和 `da-DK-HelleRUS` 语音的容器映像。        |
| `de-at-michael`                             | 具有 `de-AT` 区域设置和 `de-AT-Michael` 语音的容器映像。         |
| `de-ch-karsten`                             | 具有 `de-CH` 区域设置和 `de-CH-Karsten` 语音的容器映像。         |
| `de-de-hedda`                               | 具有 `de-DE` 区域设置和 `de-DE-Hedda` 语音的容器映像。           |
| `de-de-heddarus`                            | 具有 `de-DE` 区域设置和 `de-DE-Hedda` 语音的容器映像。           |
| `de-de-stefan-apollo`                       | 具有 `de-DE` 区域设置和 `de-DE-Stefan-Apollo` 语音的容器映像。   |
| `el-gr-stefanos`                            | 具有 `el-GR` 区域设置和 `el-GR-Stefanos` 语音的容器映像。        |
| `en-au-catherine`                           | 具有 `en-AU` 区域设置和 `en-AU-Catherine` 语音的容器映像。       |
| `en-au-hayleyrus`                           | 具有 `en-AU` 区域设置和 `en-AU-HayleyRUS` 语音的容器映像。       |
| `en-ca-heatherrus`                          | 具有 `en-CA` 区域设置和 `en-CA-HeatherRUS` 语音的容器映像。      |
| `en-ca-linda`                               | 具有 `en-CA` 区域设置和 `en-CA-Linda` 语音的容器映像。           |
| `en-gb-george-apollo`                       | 具有 `en-GB` 区域设置和 `en-GB-George-Apollo` 语音的容器映像。   |
| `en-gb-hazelrus`                            | 具有 `en-GB` 区域设置和 `en-GB-HazelRUS` 语音的容器映像。        |
| `en-gb-susan-apollo`                        | 具有 `en-GB` 区域设置和 `en-GB-Susan-Apollo` 语音的容器映像。    |
| `en-ie-sean`                                | 具有 `en-IE` 区域设置和 `en-IE-Sean` 语音的容器映像。            |
| `en-in-heera-apollo`                        | 具有 `en-IN` 区域设置和 `en-IN-Heera-Apollo` 语音的容器映像。    |
| `en-in-priyarus`                            | 具有 `en-IN` 区域设置和 `en-IN-PriyaRUS` 语音的容器映像。        |
| `en-in-ravi-apollo`                         | 具有 `en-IN` 区域设置和 `en-IN-Ravi-Apollo` 语音的容器映像。     |
| `en-us-benjaminrus`                         | 具有 `en-US` 区域设置和 `en-US-BenjaminRUS` 语音的容器映像。     |
| `en-us-guy24krus`                           | 具有 `en-US` 区域设置和 `en-US-Guy24kRUS` 语音的容器映像。       |
| `en-us-aria24krus`                          | 具有 `en-US` 区域设置和 `en-US-Aria24kRUS` 语音的容器映像。      |
| `en-us-ariarus`                             | 具有 `en-US` 区域设置和 `en-US-AriaRUS` 语音的容器映像。         |
| `en-us-zirarus`                             | 具有 `en-US` 区域设置和 `en-US-ZiraRUS` 语音的容器映像。         |
| `es-es-helenarus`                           | 具有 `es-ES` 区域设置和 `es-ES-HelenaRUS` 语音的容器映像。       |
| `es-es-laura-apollo`                        | 具有 `es-ES` 区域设置和 `es-ES-Laura-Apollo` 语音的容器映像。    |
| `es-es-pablo-apollo`                        | 具有 `es-ES` 区域设置和 `es-ES-Pablo-Apollo` 语音的容器映像。    |
| `es-mx-hildarus`                            | 具有 `es-MX` 区域设置和 `es-MX-HildaRUS` 语音的容器映像。        |
| `es-mx-raul-apollo`                         | 具有 `es-MX` 区域设置和 `es-MX-Raul-Apollo` 语音的容器映像。     |
| `fi-fi-heidirus`                            | 具有 `fi-FI` 区域设置和 `fi-FI-HeidiRUS` 语音的容器映像。        |
| `fr-ca-caroline`                            | 具有 `fr-CA` 区域设置和 `fr-CA-Caroline` 语音的容器映像。        |
| `fr-ca-harmonierus`                         | 具有 `fr-CA` 区域设置和 `fr-CA-HarmonieRUS` 语音的容器映像。     |
| `fr-ch-guillaume`                           | 具有 `fr-CH` 区域设置和 `fr-CH-Guillaume` 语音的容器映像。       |
| `fr-fr-hortenserus`                         | 具有 `fr-FR` 区域设置和 `fr-FR-HortenseRUS` 语音的容器映像。     |
| `fr-fr-julie-apollo`                        | 具有 `fr-FR` 区域设置和 `fr-FR-Julie-Apollo` 语音的容器映像。    |
| `fr-fr-paul-apollo`                         | 具有 `fr-FR` 区域设置和 `fr-FR-Paul-Apollo` 语音的容器映像。     |
| `he-il-asaf`                                | 具有 `he-IL` 区域设置和 `he-IL-Asaf` 语音的容器映像。            |
| `hi-in-hemant`                              | 具有 `hi-IN` 区域设置和 `hi-IN-Hemant` 语音的容器映像。          |
| `hi-in-kalpana-apollo`                      | 具有 `hi-IN` 区域设置和 `hi-IN-Kalpana-Apollo` 语音的容器映像。  |
| `hi-in-kalpana`                             | 具有 `hi-IN` 区域设置和 `hi-IN-Kalpana` 语音的容器映像。         |
| `hr-hr-matej`                               | 具有 `hr-HR` 区域设置和 `hr-HR-Matej` 语音的容器映像。           |
| `hu-hu-szabolcs`                            | 具有 `hu-HU` 区域设置和 `hu-HU-Szabolcs` 语音的容器映像。        |
| `id-id-andika`                              | 具有 `id-ID` 区域设置和 `id-ID-Andika` 语音的容器映像。          |
| `it-it-cosimo-apollo`                       | 具有 `it-IT` 区域设置和 `it-IT-Cosimo-Apollo` 语音的容器映像。   |
| `it-it-luciarus`                            | 具有 `it-IT` 区域设置和 `it-IT-LuciaRUS` 语音的容器映像。        |
| `ja-jp-ayumi-apollo`                        | 具有 `ja-JP` 区域设置和 `ja-JP-Ayumi-Apollo` 语音的容器映像。    |
| `ja-jp-harukarus`                           | 具有 `ja-JP` 区域设置和 `ja-JP-HarukaRUS` 语音的容器映像。       |
| `ja-jp-ichiro-apollo`                       | 具有 `ja-JP` 区域设置和 `ja-JP-Ichiro-Apollo` 语音的容器映像。   |
| `ko-kr-heamirus`                            | 具有 `ko-KR` 区域设置和 `ko-KR-HeamiRUS` 语音的容器映像。        |
| `ms-my-rizwan`                              | 具有 `ms-MY` 区域设置和 `ms-MY-Rizwan` 语音的容器映像。          |
| `nb-no-huldarus`                            | 具有 `nb-NO` 区域设置和 `nb-NO-HuldaRUS` 语音的容器映像。        |
| `nl-nl-hannarus`                            | 具有 `nl-NL` 区域设置和 `nl-NL-HannaRUS` 语音的容器映像。        |
| `pl-pl-paulinarus`                          | 具有 `pl-PL` 区域设置和 `pl-PL-PaulinaRUS` 语音的容器映像。      |
| `pt-br-daniel-apollo`                       | 具有 `pt-BR` 区域设置和 `pt-BR-Daniel-Apollo` 语音的容器映像。   |
| `pt-br-heloisarus`                          | 具有 `pt-BR` 区域设置和 `pt-BR-HeloisaRUS` 语音的容器映像。      |
| `pt-pt-heliarus`                            | 具有 `pt-PT` 区域设置和 `pt-PT-HeliaRUS` 语音的容器映像。        |
| `ro-ro-andrei`                              | 具有 `ro-RO` 区域设置和 `ro-RO-Andrei` 语音的容器映像。          |
| `ru-ru-ekaterinarus`                        | 具有 `ru-RU` 区域设置和 `ru-RU-EkaterinaRUS` 语音的容器映像。    |
| `ru-ru-irina-apollo`                        | 具有 `ru-RU` 区域设置和 `ru-RU-Irina-Apollo` 语音的容器映像。    |
| `ru-ru-pavel-apollo`                        | 具有 `ru-RU` 区域设置和 `ru-RU-Pavel-Apollo` 语音的容器映像。    |
| `sk-sk-filip`                               | 具有 `sk-SK` 区域设置和 `sk-SK-Filip` 语音的容器映像。           |
| `sl-si-lado`                                | 具有 `sl-SI` 区域设置和 `sl-SI-Lado` 语音的容器映像。            |
| `sv-se-hedvigrus`                           | 具有 `sv-SE` 区域设置和 `sv-SE-HedvigRUS` 语音的容器映像。       |
| `ta-in-valluvar`                            | 具有 `ta-IN` 区域设置和 `ta-IN-Valluvar` 语音的容器映像。        |
| `te-in-chitra`                              | 具有 `te-IN` 区域设置和 `te-IN-Chitra` 语音的容器映像。          |
| `th-th-pattara`                             | 具有 `th-TH` 区域设置和 `th-TH-Pattara` 语音的容器映像。         |
| `tr-tr-sedarus`                             | 具有 `tr-TR` 区域设置和 `tr-TR-SedaRUS` 语音的容器映像。         |
| `vi-vn-an`                                  | 具有 `vi-VN` 区域设置和 `vi-VN-An` 语音的容器映像。              |
| `zh-cn-huihuirus`                           | 具有 `zh-CN` 区域设置和 `zh-CN-HuihuiRUS` 语音的容器映像。       |
| `zh-cn-kangkang-apollo`                     | 具有 `zh-CN` 区域设置和 `zh-CN-Kangkang-Apollo` 语音的容器映像。 |
| `zh-cn-yaoyao-apollo`                       | 具有 `zh-CN` 区域设置和 `zh-CN-Yaoyao-Apollo` 语音的容器映像。   |
| `zh-hk-danny-apollo`                        | 具有 `zh-HK` 区域设置和 `zh-HK-Danny-Apollo` 语音的容器映像。    |
| `zh-hk-tracy-apollo`                        | 具有 `zh-HK` 区域设置和 `zh-HK-Tracy-Apollo` 语音的容器映像。    |
| `zh-hk-tracyrus`                            | 具有 `zh-HK` 区域设置和 `zh-HK-TracyRUS` 语音的容器映像。        |
| `zh-tw-hanhanrus`                           | 具有 `zh-TW` 区域设置和 `zh-TW-HanHanRUS` 语音的容器映像。       |
| `zh-tw-yating-apollo`                       | 具有 `zh-TW` 区域设置和 `zh-TW-Yating-Apollo` 语音的容器映像。   |
| `zh-tw-zhiwei-apollo`                       | 具有 `zh-TW` 区域设置和 `zh-TW-Zhiwei-Apollo` 语音的容器映像。   |

---

## <a name="neural-text-to-speech"></a>神经文本转语音

可在容器注册表 "联合" 中找到 [神经文本到语音][sp-ntts] 容器映像 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services/speechservices/` 存储库中，名为 `neural-text-to-speech`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/speechservices/neural-text-to-speech`。

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/neural-text-to-speech/tags/list)的完整列表。


# <a name="latest-version"></a>[最新版本](#tab/current)

发行说明 `v1.3.0` ：
* 神经文本到语音转换容器现已正式发布。 

| 映像标记                                  | 说明                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | 具有 `en-US` 区域设置和 `en-US-AriaNeural` 语音的容器映像。      |
| `1.3.0-amd64-<locale-and-voice>`    | `<locale>`使用下面列出的一个可用区域设置替换。 例如，`1.3.0-amd64-en-us-arianeural`。 |


| v 1.3.0 区域设置和语音           | 说明                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `de-de-katjaneural`                 | 具有 `de-DE` 区域设置和 `de-DE-KatjaNeural` 语音的容器映像。     |
| `en-au-natashaneural`               | 具有 `en-AU` 区域设置和 `en-AU-NatashaNeural` 语音的容器映像。   |
| `en-ca-claraneural`                 | 具有 `en-CA` 区域设置和 `en-CA-ClaraNeural` 语音的容器映像。     |
| `en-gb-libbyneural`                 | 具有 `en-GB` 区域设置和 `en-GB-LibbyNeural` 语音的容器映像。     |
| `en-gb-mianeural`                   | 具有 `en-GB` 区域设置和 `en-GB-MiaNeural` 语音的容器映像。       |
| `en-us-arianeural`                  | 具有 `en-US` 区域设置和 `en-US-AriaNeural` 语音的容器映像。      |
| `en-us-guyneural`                   | 具有 `en-US` 区域设置和 `en-US-GuyNeural` 语音的容器映像。       |
| `es-es-elviraneural`                | 具有 `es-ES` 区域设置和 `es-ES-ElviraNeural` 语音的容器映像。    |
| `es-mx-dalianeural`                 | 具有 `es-MX` 区域设置和 `es-MX-DaliaNeural` 语音的容器映像。     |
| `fr-ca-sylvieneural`                | 具有 `fr-CA` 区域设置和 `fr-CA-SylvieNeural` 语音的容器映像。    |
| `fr-fr-deniseneural`                | 具有 `fr-FR` 区域设置和 `fr-FR-DeniseNeural` 语音的容器映像。    |
| `it-it-elsaneural`                  | 具有 `it-IT` 区域设置和 `it-IT-ElsaNeural` 语音的容器映像。      |
| `ja-jp-nanamineural`                | 具有 `ja-JP` 区域设置和 `ja-JP-NanamiNeural` 语音的容器映像。    |
| `ko-kr-sunhineural`                 | 具有 `ko-KR` 区域设置和 `ko-KR-SunHiNeural` 语音的容器映像。     |
| `pt-br-franciscaneural`             | 具有 `pt-BR` 区域设置和 `pt-BR-FranciscaNeural` 语音的容器映像。 |
| `zh-cn-xiaoxiaoneural`              | 具有 `zh-CN` 区域设置和 `zh-CN-XiaoxiaoNeural` 语音的容器映像。  |

# <a name="previous-version"></a>[先前版本](#tab/previous)

| 映像标记                                  | 说明                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | 具有 `en-US` 区域设置和 `en-US-AriaNeural` 语音的容器映像。      |
| `1.2.0-amd64-<locale-and-voice>-preview`    | `<locale>`使用下面列出的一个可用区域设置替换。 例如，`1.2.0-amd64-en-us-arianeural-preview`。 |


| 1.2.0 预览区域设置和语音           | 说明                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | 具有 `en-US` 区域设置和 `en-US-AriaNeural` 语音的容器映像。      |
| `de-de-katjaneural-preview`                 | 具有 `de-DE` 区域设置和 `de-DE-KatjaNeural` 语音的容器映像。     |
| `en-au-natashaneural-preview`               | 具有 `en-AU` 区域设置和 `en-AU-NatashaNeural` 语音的容器映像。   |
| `en-ca-claraneural-preview`                 | 具有 `en-CA` 区域设置和 `en-CA-ClaraNeural` 语音的容器映像。     |
| `en-gb-libbyneural-preview`                 | 具有 `en-GB` 区域设置和 `en-GB-LibbyNeural` 语音的容器映像。     |
| `en-gb-mianeural-preview`                   | 具有 `en-GB` 区域设置和 `en-GB-MiaNeural` 语音的容器映像。       |
| `en-us-arianeural-preview`                  | 具有 `en-US` 区域设置和 `en-US-AriaNeural` 语音的容器映像。      |
| `en-us-guyneural-preview`                   | 具有 `en-US` 区域设置和 `en-US-GuyNeural` 语音的容器映像。       |
| `es-es-elviraneural-preview`                | 具有 `es-ES` 区域设置和 `es-ES-ElviraNeural` 语音的容器映像。    |
| `es-mx-dalianeural-preview`                 | 具有 `es-MX` 区域设置和 `es-MX-DaliaNeural` 语音的容器映像。     |
| `fr-ca-sylvieneural-preview`                | 具有 `fr-CA` 区域设置和 `fr-CA-SylvieNeural` 语音的容器映像。    |
| `fr-fr-deniseneural-preview`                | 具有 `fr-FR` 区域设置和 `fr-FR-DeniseNeural` 语音的容器映像。    |
| `it-it-elsaneural-preview`                  | 具有 `it-IT` 区域设置和 `it-IT-ElsaNeural` 语音的容器映像。      |
| `ja-jp-nanamineural-preview`                | 具有 `ja-JP` 区域设置和 `ja-JP-NanamiNeural` 语音的容器映像。    |
| `ko-kr-sunhineural-preview`                 | 具有 `ko-KR` 区域设置和 `ko-KR-SunHiNeural` 语音的容器映像。     |
| `pt-br-franciscaneural-preview`             | 具有 `pt-BR` 区域设置和 `pt-BR-FranciscaNeural` 语音的容器映像。 |
| `zh-cn-xiaoxiaoneural-preview`              | 具有 `zh-CN` 区域设置和 `zh-CN-XiaoxiaoNeural` 语音的容器映像。  |

---

## <a name="speech-language-detection"></a>语音语言检测

可以在容器注册表联合中找到 [语音语言检测][sp-lid] 容器映像 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services/speechservices/` 存储库中，名为 `language-detection`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/speechservices/language-detection`。

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/language-detection/tags/list)的完整列表。

| 映像标记                                  | 说明                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                       |      |
| `1.1.0-amd64-preview`                       |      |

## <a name="key-phrase-extraction"></a>关键短语提取

容器映像可在容器注册表 "联合" 中找到 `mcr.microsoft.com` 。 该映像驻留在 `azure-cognitive-services/textanalytics/` 存储库中，名为 `keyphrase`。 完全限定的容器映像名称为 `mcr.microsoft.com/azure-cognitive-services/textanalytics/keyphrase`。

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/keyphrase/tags/list)的完整列表。

# <a name="latest-version"></a>[最新版本](#tab/current)


| 映像标记                    | 说明 |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` |       |

# <a name="previous-versions"></a>[旧版](#tab/previous)

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `1.1.012840001-amd64` |       |
| `1.1.012830001-amd64`    |       |

---

## <a name="text-language-detection"></a>文本语言检测

在 `mcr.microsoft.com` 容器注册表联合项中可以找到[语言检测][ta-la]容器映像。 该映像驻留在 `azure-cognitive-services/textanalytics/` 存储库中，名为 `language`。 完全限定的容器映像名称为， `mcr.microsoft.com/azure-cognitive-services/textanalytics/language`


此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/language/tags/list)的完整列表。

# <a name="latest-versions"></a>[最新版本](#tab/current)

| 映像标记                    | 说明 |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` | |
   

# <a name="previous-versions"></a>[旧版](#tab/previous)


| 映像标记                    | 说明 |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012840001-amd64` |   |
| `1.1.012830001-amd64` |   |

---

## <a name="sentiment-analysis"></a>情绪分析

在 `mcr.microsoft.com` 容器注册表联合项中可以找到[情绪分析][ta-se]容器映像。 该映像驻留在 `azure-cognitive-services/textanalytics/` 存储库中，名为 `sentiment`。 完全限定的容器映像名称为， `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment`

此容器映像具有以下可用标记。 你还可以 [在 MCR 上找到标记](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/sentiment/tags/list)的完整列表。

| 映像标记 | 说明                                         |
|------------|:----------------------------------------------|
| `latest`   |                                               |
| `3.0-en`   | 情绪分析 v3（英语）               |
| `3.0-es`   | 情绪分析 v3（西班牙语）               |
| `3.0-fr`   | 情绪分析 v3（法语）                |
| `3.0-it`   | 情绪分析 v3（意大利语）               |
| `3.0-de`   | 情绪分析 v3（德语）                |
| `3.0-zh`   | 情绪分析 v3（简体中文）  |
| `3.0-zht`  | 情绪分析 v3（繁体中文） |
| `3.0-ja`   | 情绪分析 v3（日语）              |
| `3.0-pt`   | 情绪分析 v3（葡萄牙语）            |
| `3.0-nl`   | 情绪分析 v3（荷兰语）                 |
| `2.1`    | 情绪分析 v2      |

[ad-containers]: ../anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[fr-containers]: ../form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ../luis/luis-container-howto.md
[sp-stt]: ../speech-service/speech-container-howto.md?tabs=stt
[sp-cstt]: ../speech-service/speech-container-howto.md?tabs=cstt
[sp-tts]: ../speech-service/speech-container-howto.md?tabs=tts
[sp-ctts]: ../speech-service/speech-container-howto.md?tabs=ctts
[sp-ntts]: ../speech-service/speech-container-howto.md?tabs=ntts
[sp-lid]: ../speech-service/speech-container-howto.md?tabs=lid
[ta-kp]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-la]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-se]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment
