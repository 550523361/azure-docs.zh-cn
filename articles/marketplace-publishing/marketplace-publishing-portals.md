---
title: 为市场创建产品/服务所需的各种门户概述 | Microsoft Docs
description: 为市场创建供应项所需的各种门户的概述
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: 89ce82b3-c28a-4b0d-b37a-db3112160a4e
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2016
ms.author: hascipio
ROBOTS: NOINDEX
ms.openlocfilehash: d1cc0efa1da90d90bd035eaa495a1336b6ff96c2
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54074171"
---
# <a name="portals-you-will-need"></a>所需的门户

在启动发布产品/服务的过程之前，让我们先介绍你所需的各种门户。 下面是有关以下各门户的简要概述：开发人员中心、Azure 发布门户和 Azure 门户（按照与其进行交互的顺序显示）。                                                                            

## <a name="developer-center"></a>开发人员中心
[http://dev.windows.com/registration?accountprogram=azure](http://dev.windows.com/registration?accountprogram=azure)

### <a name="description"></a>Description
创建 Microsoft 开发人员中心帐户是一次性的任务。 在创建开发人员帐户前，请确保公司没有开发人员中心帐户。 在该过程中，我们会收集银号帐户信息、税务信息和公司地址信息。

> [!NOTE]
> 如果仅发布免费套餐（或提供自带的许可），则不需要税务和银行信息。
> 
> 

### <a name="identityaccount-used"></a>所用的身份/帐户
理想情况下，此值为通讯组列表或安全组（例如，azurepublishing@partnercompany.com）。 通讯组列表或安全组**必须**已注册为 Microsoft 帐户。

> [!TIP]
> 建议使用通讯组列表或安全组，因为它会删除任何个人帐户的依赖性，尽管也可以使用个人帐户。
> 
> 

## <a name="publishing-portal"></a>发布门户
[https://publish.windowsazure.com](https://publish.windowsazure.com)

### <a name="description"></a>Description

此值表示用于处理产品/服务并进行发布的门户（市场营销、定价、发布、证书（如果适用）等）。

### <a name="identityaccount-used"></a>所用的身份/帐户
首次登录到发布门户时，必须使用上述的通讯组列表或安全组。 稍后，即可将其他用户添加为共同管理员。 此列表显示了映射到开发人员中心注册数据的方式。

## <a name="azure-portal"></a>Azure 门户
[Azure 门户](https://portal.azure.com)

### <a name="description"></a>Description
此值表示可以在 Azure 市场中查看暂存和发布的供应项的门户（适用于 VM、解决方案模板和基于 Azure 资源管理器的开发人员服务）。

### <a name="identityaccount-used"></a>所用的身份/帐户
从发布的门户中暂存某套餐时，需要将订阅 ID 列入允许列表。 登录到此门户来测试暂存的套餐时，必须使用同一个订阅（具有与之关联的用户名和密码）。

## <a name="see-also"></a>另请参阅
* [入门：如何为 Azure 市场发布产品/服务](marketplace-publishing-getting-started.md)

