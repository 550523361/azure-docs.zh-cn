---
title: 了解 Azure 媒体服务建议的编码器 | Microsoft 文档
description: 本文列出了 Azure 媒体服务推荐的本地编码器。
services: media-services
keywords: 编码;编码器;媒体
author: dbgeorge
manager: johndeu
ms.author: johndeu
ms.date: 03/20/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 89b01a3fb066f181f5ec54b481b71feaa7a6ae08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "77131406"
---
# <a name="recommended-on-premises-encoders"></a>推荐的本地编码器

使用 Azure 媒体服务实时传送视频流时，可以指定通道接收输入流的方式。 如果选择将本地编码器与实时编码通道结合使用，则编码器应推送高质量单比特率流作为输出。 如果选择将本地编码器与直通通道结合使用，则编码器应推送具备所有所需输出质量的多比特率流作为输出。 有关详细信息，请参阅[使用本地编码器实时传送视频流](media-services-live-streaming-with-onprem-encoders.md)。

## <a name="encoder-requirements"></a>编码器要求

使用 HTTPS 或 RTMPS 协议时，编码器必须支持 TLS 1.2。

## <a name="live-encoders-that-output-rtmp"></a>输出 RTMP 的实时编码器 

Azure 媒体服务建议使用下列将 RTMP 作为输出的实时编码器之一：

- Adobe Flash 媒体实时编码器 3.2
- Haivision Makito X HEVC
- Haivision KB
- 由于 TLS 1.2 要求，远程直播有线广播（版本 13.0.2 或更高版本）

  使用 RTMPS 协议时，编码器必须支持 TLS 1.2。
- Teradek Slice 756
- TriCaster 8000
- Tricaster Mini HD-4
- OBS Studio
- VMIX
- xStream
- Switcher Studio (iOS)

## <a name="live-encoders-that-output-fragmented-mp4"></a>输出分片 MP4 的实时编码器 

Azure 媒体服务建议使用下列将多比特率片段 MP4（平滑流式处理）作为输出的实时编码器之一：

- Media Excel Hero Live 和 Hero 4K (UHD/HEVC)
- Ateme TITAN Live
- Cisco 数字媒体编码器 2200
- 元素实时（版本 2.14.15 及以上，由于 TLS 1.2 要求）

  使用 HTTPS 协议时，编码器必须支持 TLS 1.2。
- Envivio 4Caster C4 Gen III
- Imagine Communications Selenio MCP3

> [!NOTE]
> 实时编码器可以将单比特率流发送到直通通道，但并不建议此配置，因为它不允许对客户端进行自适应比特率流式处理。

## <a name="how-to-become-an-on-premises-encoder-partner"></a>如何成为本地编码器合作伙伴

作为 Azure 媒体服务本地编码器合作伙伴，媒体服务通过向企业客户推荐编码器来推广你的产品。 若要成为本地编码器合作伙伴，必须验证本地编码器与媒体服务的兼容性。 为此，请完成以下验证：

直通通道验证
1. 创建或访问 Azure 媒体服务帐户
2. 创建并启动“直通”**** 通道
3. 配置编码器推送多比特率实时流。
4. 创建已发布的直播活动
5. 运行实时编码器大约 10 分钟
6. 停止直播活动
7. 创建、启动流式处理终结点、使用 Azure[媒体播放器等](https://aka.ms/azuremediaplayer)播放器监视存档资产，以确保播放没有所有质量级别的可见故障（或者，在步骤 6 之前的实时会话期间，通过预览 URL 观看和验证）
8. 记录资产 ID、为实时存档发布的流式处理 URL，以及实时编码器所使用的设置和版本
9. 在创建每个示例后重置通道状态
10. 为编码器所支持的所有配置重复（具有或无广告信号/字幕/不同编码速度）步骤 3-9

实时编码通道验证
1. 创建或访问 Azure 媒体服务帐户
2. 创建并启动“实时编码”**** 通道
3. 配置编码器推送单比特率实时流。
4. 创建已发布的直播活动
5. 运行实时编码器大约 10 分钟
6. 停止直播活动
7. 创建、启动流式处理终结点、使用 Azure[媒体播放器等](https://aka.ms/azuremediaplayer)播放器监视存档资产，以确保播放没有所有质量级别的可见故障（或者，在步骤 6 之前的实时会话期间，通过预览 URL 观看和验证）
8. 记录资产 ID、为实时存档发布的流式处理 URL，以及实时编码器所使用的设置和版本
9. 在创建每个示例后重置通道状态
10. 为编码器所支持的所有配置（具有和无广告信号/字幕/不同编码速度）重复步骤 3-9

使用寿命验证
1. 创建或访问 Azure 媒体服务帐户
2. 创建并启动“直通”**** 通道
3. 配置编码器推送多比特率实时流。
4. 创建已发布的直播活动
5. 运行实时编码器持续一周或更长时间
6. 使用[Azure 媒体播放器](https://aka.ms/azuremediaplayer)等播放器不时观看实时流式处理（或存档资产），以确保播放没有可见的故障
7. 停止直播活动
8. 记录资产 ID、为实时存档发布的流式处理 URL，以及实时编码器所使用的设置和版本

最后，通过电子邮件 amsstreaming@microsoft.com 将记录的设置和实时存档参数发送到媒体服务。 收到后，媒体服务会对实时编码器中的示例执行验证测试。 有关此过程的任何问题，请联系媒体服务。
