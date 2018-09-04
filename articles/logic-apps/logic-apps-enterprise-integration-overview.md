---
title: B2B 企业集成概述 - Azure 逻辑应用 | Microsoft Docs
description: 使用 Azure 逻辑应用和 Enterprise Integration Pack 为企业集成解决方案构建自动化 B2B 工作流
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: dd517c4d-1701-4247-b83c-183c4d8d8aae
ms.date: 09/08/2016
ms.openlocfilehash: b2e2c81914e8c0440b358d59c7f0248db46b6c50
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2018
ms.locfileid: "43124286"
---
# <a name="overview-b2b-enterprise-integration-scenarios-in-azure-logic-apps-with-enterprise-integration-pack"></a>概述：带有 Enterprise Integration Pack 的 Azure 逻辑应用中的 B2B 企业集成方案

为了实现企业到企业 (B2B) 工作流和与 Azure 逻辑应用的无缝通信，可以使用 Microsoft 的基于云的解决方案（Enterprise Integration Pack）启用企业集成方案。 组织可以电子方式交换消息，即使它们使用不同的协议和格式，也是如此。 该包将不同的格式转换为组织的系统可以解释和处理的格式。 组织可以通过行业标准协议（包括 [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md)、[X12](logic-apps-enterprise-integration-x12.md) 和 [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md)）交换消息。 还可以使用加密和数字签名保护消息。

如果熟悉 BizTalk Server 或 Microsoft Azure BizTalk 服务，则可轻松地使用企业集成功能，因为大多数概念是类似的。 一个主要区别是企业集成使用集成帐户来简化 B2B 通信中使用的项目的存储和管理。 

在体系结构上，Enterprise Integration Pack 基于“集成帐户”。 这些帐户是基于云的容器，存储所有项目，例如架构、合作伙伴、证书、映射和协议。 可以使用这些项目设计、部署和维护 B2B 应用以及为逻辑应用构建 B2B 工作流。 但在使用这些项目之前，必须先将集成帐户链接到逻辑应用。 之后，逻辑应用便可以访问集成帐户的项目。

## <a name="why-should-you-use-enterprise-integration"></a>为什么应使用企业集成？

* 通过企业集成，可以将所有项目存储在一个位置，即集成帐户。
* 可以使用 Azure 逻辑应用引擎及其所有连接器来构建 B2B 工作流并将其与第三方软件即服务 (SaaS) 应用、本地应用以及自定义应用集成。
* 可以使用 Azure 函数为逻辑应用创建自定义代码。

## <a name="how-to-get-started-with-enterprise-integration"></a>如何开始使用企业集成？

可以通过 **Azure 门户**中的逻辑应用设计器来使用 Enterprise Integration Pack 构建和管理 B2B 应用。 还可以使用 [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.logicapp "逻辑应用 PowerShell") 来管理逻辑应用。

以下是在 Azure 门户中创建应用前必须先执行的高级步骤：

![概述映像](media/logic-apps-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>一些常见方案有哪些？

企业集成支持以下这些行业标准：

* EDI - 电子数据交换
* EAI - 企业应用程序集成

## <a name="heres-what-you-need-to-get-started"></a>以下是开始使用需要满足的条件

* 具有集成帐户的 Azure 订阅
* Visual Studio 2015，用于创建映射和架构
* [Visual Studio 2015 2.0 的 Microsoft Azure 逻辑应用企业集成工具](https://aka.ms/vsmapsandschemas)  

## <a name="try-it-now"></a>立即试用

[部署使用 Azure 逻辑应用 B2B 功能的完全正常运行的示例 AS2 发送和接收逻辑应用](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive)。

## <a name="learn-more"></a>了解详细信息
* [协议](../logic-apps/logic-apps-enterprise-integration-agreements.md "了解企业集成协议")
* [企业到企业 (B2B) 方案](../logic-apps/logic-apps-enterprise-integration-b2b.md "了解如何使用 B2B 功能创建逻辑应用")  
* [证书](logic-apps-enterprise-integration-certificates.md "了解企业集成证书")
* [平面文件编码/解码](logic-apps-enterprise-integration-flatfile.md "了解如何对平面文件内容进行编码和解码")  
* [集成帐户](../logic-apps/logic-apps-enterprise-integration-accounts.md "了解集成帐户")
* [映射](../logic-apps/logic-apps-enterprise-integration-maps.md "了解企业集成映射")
* [合作伙伴](logic-apps-enterprise-integration-partners.md "了解企业集成合作伙伴")
* [架构](logic-apps-enterprise-integration-schemas.md "了解企业集成架构")
* [XML 消息验证](logic-apps-enterprise-integration-xml.md "了解如何使用逻辑应用验证 XML 消息")
* [XML 转换](logic-apps-enterprise-integration-transform.md "了解企业集成映射")
* [企业集成连接器](../connectors/apis-list.md "了解 Enterprise Integration Pack 连接器")
* [集成帐户元数据](../logic-apps/logic-apps-enterprise-integration-metadata.md "了解集成帐户元数据")
* [监视 B2B 消息](logic-apps-monitor-b2b-message.md "深入了解 B2B 消息")
* [在 OMS 门户中跟踪 B2B 消息](logic-apps-track-b2b-messages-omsportal.md "深入了解在 OMS 门户中跟踪 B2B 消息")

