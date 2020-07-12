---
title: Windows 虚拟桌面 Linux 支持-Azure
description: Windows 虚拟桌面的简要概述 Linux 支持。
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 3dc9d62141a63574a6796982542cb1491108eedc
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259856"
---
# <a name="linux-support"></a>Linux 支持

合作伙伴可以使用适用于 Windows 虚拟桌面的 Linux SDK 来构建独立的 Windows 虚拟桌面客户端。 你还可以使用它在客户端应用程序上启用 Windows 虚拟桌面支持。 本快速指南将介绍 Linux SDK 的定义以及如何开始使用它。

## <a name="connect-with-your-linux-device"></a>连接到 Linux 设备

以下合作伙伴已批准适用于 Linux 设备的 Windows 虚拟桌面客户端。

|Partner|合作伙伴文档|合作伙伴支持|
|:------|:--------------------|:--------------|
|![IGEL 徽标](./media/partners/igel.png)|[IGEL 客户端文档](https://www.igel.com/igel-solution-family/windows-virtual-desktop/)|[IGEL 支持](https://www.igel.com/support/)|

## <a name="what-is-the-linux-sdk"></a>什么是 Linux SDK？

你可以使用 SDK Api 来检索资源源、连接到桌面或远程应用程序会话，并使用我们的第一方客户端支持的多个重定向。

> [!NOTE]
> SDK 目前正在开发中。 当 SDK 公开发布时，我们将使用有关访问 SDK 的说明来更新本文档。

### <a name="supported-linux-distributions"></a>支持的 Linux 分发

SDK 与大多数基于 Ubuntu 18.04 或更高版本的操作系统兼容。 如果你使用的是不同的 Linux 分发版，则可以与你联系，以确定如何最大程度地满足你的需求。

### <a name="feature-support"></a>功能支持

SDK 支持多个到桌面和远程应用程序会话的连接。 支持以下重定向：

| 重定向       | 支持 |
| :---------------- | :-------: |
| 键盘          | &#10004;  |
| 鼠标             | &#10004;  |
| 音频传入          | &#10004;  |
| 音频输出         | &#10004;  |
| 剪贴板 (文本)   | &#10004;  |
| 剪贴板 (图像)  | &#10004;  |
| 剪贴板 (文件)   | &#10004;  |
| 智能卡         | &#10004;  |
| 驱动器/文件夹      | &#10004;  |

SDK 还支持多个监视器显示配置，前提是你为会话选择的监视器是连续的。

我们将更新此文档，因为我们添加了对新功能和重定向的支持。 若要建议新功能和其他改进，请访问我们的[UserVoice 页](https://go.microsoft.com/fwlink/?linkid=2116523)。

## <a name="get-started-with-the-linux-sdk"></a>Linux SDK 入门

你需要执行以下操作，然后才能为 Windows 虚拟桌面开发 Linux 客户端：

1. 构建并部署用于测试或生产的 Windows 虚拟桌面环境。
2. 测试可用的第一方客户端，以熟悉 Windows 虚拟桌面用户体验。

## <a name="next-steps"></a>后续步骤

请查看以下客户端的文档：

- [Windows 桌面客户端](connect-windows-7-10.md)
- [Web 客户端](connect-web.md)
- [Android 客户端](connect-android.md)
- [macOS 客户端](connect-macos.md)
- [iOS 客户端](connect-ios.md)
