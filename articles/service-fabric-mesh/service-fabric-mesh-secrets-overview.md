---
title: 存储和使用 Azure Service Fabric 网格应用程序机密
description: Service Fabric 网格支持将机密作为 Azure 资源。 下面介绍如何在 Service Fabric 网格应用程序中存储和管理机密。
author: erikadoyle
ms.author: edoyle
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: da4eaa34840f14714616b85e24fd62cf65602b84
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "76277634"
---
# <a name="service-fabric-mesh-application-secrets"></a>Service Fabric 网格应用程序机密
Service Fabric 网格支持将机密作为 Azure 资源。 Service Fabric 网格机密可以是任何敏感文本信息，例如存储连接字符串、密码或应该安全存储和传输的其他值。

![网格机密概述][sf-mesh-secrets-overview]

## <a name="mesh-secrets-resources"></a>网格机密资源
网格应用程序机密包括：
* 一个**机密**资源，它是一个用于存储文本机密的容器。 **机密**资源中包含的机密将安全地进行存储和传输。
* 存储在**机密**资源容器中的一个或多个**机密/值**资源。 每个**机密/值**资源都由版本号予以区分。

## <a name="next-steps"></a>后续步骤 
若要详细了解 Service Fabric 网格机密，请参阅：
- [管理 Service Fabric 网格应用程序机密](service-fabric-mesh-howto-manage-secrets.md)
- [Service Fabric 资源模型简介](service-fabric-mesh-service-fabric-resources.md)

<!-- pics -->
[sf-mesh-secrets-overview]: ./media/service-fabric-mesh-secrets-overview/MeshAppSecretsOverview.png
