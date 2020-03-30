---
title: 在 Azure 通知中心配置 Apple 推送通知服务 |微软文档
description: 了解如何为 Azure 通知中心配置 Apple Push Notification 服务 (APNS) 设置。
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: eb1122ba3de0002507589d3e607d1e39d905c308
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "80127514"
---
# <a name="configure-apple-push-notification-service-settings-for-a-notification-hub-in-the-azure-portal"></a>为 Azure 门户中的通知中心配置 Apple 推送通知服务设置

本文介绍如何使用 Azure 门户为 Azure 通知中心配置 Apple Push Notification 服务 (APNS) 设置。 

## <a name="prerequisites"></a>先决条件
如果尚未创建通知中心，请立即创建。 有关详细信息，请参阅[在 Azure 门户中创建 Azure 通知中心](create-notification-hub-portal.md)。 

## <a name="configure-apple-push-notification-service"></a>配置 Apple Push Notification 服务

以下过程提供的步骤演示了如何为通知中心配置 Apple Push Notification 服务 (APNS) 设置：

1. 在 Azure 门户的“通知中心”页上，在左侧菜单中选择“Apple (APNS)”。********

1. 对于“身份验证模式”，请选择“证书”或“令牌”************。

   a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 如果选择“证书”****：
   * 选择文件图标，再选择要上传的 .p12 文件**。
   * 输入密码。
   * 选择“沙盒”**** 模式。 要将推送通知发送给从应用商店购买应用的用户，则选择“生产”模式****。

     ![Azure 门户中 APNS 证书配置的屏幕截图](./media/configure-apple-push-notification-service/notification-hubs-apple-config-cert.png)

   b.保留“数据库类型”设置，即设置为“共享”。 如果选择“令牌”****：

   * 输入“密钥 ID”、“绑定 ID”、“团队 ID”和“令牌”的值****************。
   * 选择“沙盒”**** 模式。 要将推送通知发送给从应用商店购买应用的用户，则选择“生产”模式****。

     ![Azure 门户中 APNS 令牌配置的屏幕截图](./media/configure-apple-push-notification-service/notification-hubs-apple-config-token.png)

## <a name="next-steps"></a>后续步骤
有关将通知推送到 iOS 设备的分步说明的教程，请参阅以下文章：[使用通知中心和 APNS 将通知推送到 iOS 设备](notification-hubs-ios-apple-push-notification-apns-get-started.md)
