---
title: 配置 API 协议-Azure 事件网格 IoT Edge |Microsoft Docs
description: 配置 IoT Edge 上的事件网格公开的 API 协议。
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: ''
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 908bc941ee7379de067621e10adf5fd6ee6df559
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "76841804"
---
# <a name="configure-event-grid-api-protocols"></a>配置事件网格 API 协议

本指南提供事件网格模块的可能协议配置的示例。 事件网格模块为其管理和运行时操作公开了 API。 下表捕获协议和端口。

| 协议 | 端口 | 说明 |
| ---------------- | ------------ | ------------ |
| HTTP | 5888 | 默认情况下关闭。 仅在测试过程中有用。 不适用于生产工作负荷。
| HTTPS | 4438 | 默认

有关所有可能的配置，请参阅[安全性和身份验证](security-authentication.md)指南。

## <a name="expose-https-to-iot-modules-on-the-same-edge-network"></a>在同一边缘网络上向 IoT 模块公开 HTTPS

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=strict",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ]
}
 ```

## <a name="enable-https-to-other-iot-modules-and-non-iot-workloads"></a>启用 HTTPS 到其他 IoT 模块和非 IoT 工作负荷

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=strict",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ],
  "HostConfig": {
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ]
    }
  }
}
 ```

>[!NOTE]
> **PortBindings**部分允许将内部端口映射到容器主机的端口。 如果 IoT Edge 设备可公开访问，则此功能可让你从 IoT Edge 容器网络外部访问事件网格模块。

## <a name="expose-http-and-https-to-iot-modules-on-the-same-edge-network"></a>向同一边缘网络上的 IoT 模块公开 HTTP 和 HTTPS

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=enabled",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ]
}
 ```

## <a name="enable-http-and-https-to-other-iot-modules-and-non-iot-workloads"></a>为其他 IoT 模块和非 IoT 工作负荷启用 HTTP 和 HTTPS

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=enabled",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ],
  "HostConfig": {
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ],
      "5888/tcp": [
        {
          "HostPort": "5888"
        }
      ]
    }
  }
}
 ```

>[!NOTE]
> 默认情况下，每个 IoT 模块都是 bridge 网络创建的 IoT Edge 运行时的一部分。 它允许同一网络上的不同 IoT 模块相互通信。 **PortBindings**允许将容器内部端口映射到主机计算机上，从而使任何人都能够从外部访问事件网格模块的端口。

>[!IMPORTANT]
> 尽管可以在 IoT Edge 网络外部使端口可访问，但客户端身份验证会强制实际允许其调用模块的用户。
