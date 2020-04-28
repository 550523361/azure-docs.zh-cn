---
title: 了解安全性最佳做法-Azure 数字孪生 |Microsoft Docs
description: 了解 Azure 数字孪生和物联网的最佳安全方案。
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/15/2020
ms.openlocfilehash: 5fc5ba447557aa89e8f0870c576d6d4c439f3353
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "76122553"
---
# <a name="azure-digital-twins-security-best-practices"></a>Azure 数字孪生安全最佳做法

Azure 数字孪生安全性支持对 IoT 图中特定资源和操作的精确访问。 它通过[对称为基于角色的访问控制](./security-role-based-access-control.md)的粒度角色和权限管理来实现此目的。

Azure 数字孪生还使用 Azure IoT 中存在的其他安全功能，包括 Azure Active Directory (Azure AD)。 为此，配置和保护基于 Azure 数字孪生生成的应用程序涉及使用当前建议的多种相同的 [Azure IoT 安全做法](../iot-fundamentals/iot-security-best-practices.md)。

本文汇总了要遵循的关键最佳做法。

> [!IMPORTANT]
> 为确保 IoT 空间的最大安全性，请查看其他安全资源。 请确保包括你的设备供应商。

> [!TIP]
> 使用[适用于 iot 的 Azure 安全中心](https://docs.microsoft.com/azure/asc-for-iot/)来帮助检测 iot 安全威胁和漏洞。

## <a name="iot-security-best-practices"></a>IoT 安全最佳实践

安全地保护 IoT 设备的一些关键做法包括：

> [!div class="checklist"]
> * 以防篡改的方式保护连接到 IoT 空间的每个设备。
> * 限制 IoT 空间中每个设备、传感器和人员的角色。 如果遭到入侵，影响将降到最低。
> * 考虑设备 IP 地址筛选和端口限制的可能使用。
> * 限制 I/O 和设备带宽，以提高性能。 通过阻止拒绝服务攻击，速率限制可增强安全性。
> * 使设备固件、操作系统和软件保持最新。
> * 定期审核并查看设备、软件、网络和网关安全最佳做法，因为它们会不断改进和发展。
> * 使用受信任的经过认证的安全系统、软件和设备。 例如，查看适用于 Azure 云[的符合性产品/服务](https://azure.microsoft.com/overview/trusted-cloud/compliance/)。

安全地保护 IoT 空间的一些关键做法包括：

> [!div class="checklist"]
> * 加密已保存、已存储或持久性数据。
> * 要求定期更改或刷新密码或密钥。
> * 仔细按角色限制访问权限和权限。 阅读下面的 "[基于角色的访问控制最佳实践](#role-based-access-control-best-practices)" 部分。
> * 请考虑使用分段网络拓扑，使每个网络上的设备彼此隔离。
> * 使用功能强大的加密。 需要较长的密码，请使用安全协议和[多重身份验证](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks)。

[监视器](./how-to-configure-monitoring.md)用于监视非正常操作范围内离群值、威胁或资源参数的 IoT 资源。 使用 Azure Analytics 进行监视管理。

> [!IMPORTANT]
> 阅读 Azure [iot 安全最佳实践](../iot-fundamentals/iot-security-best-practices.md)，开始综合性 iot 安全策略。

> [!NOTE]
> 有关事件处理和监视的详细信息，请参阅[通过 Azure 数字孪生路由事件和消息](./concepts-events-routing.md)。

## <a name="azure-active-directory-best-practices"></a>Azure Active Directory 最佳做法

Azure 数字孪生使用[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/authentication/)对用户进行身份验证并保护应用程序。 Azure Active Directory 支持各种新式体系结构的身份验证。 所有这些体系结构都以行业标准协议（例如 OAuth 2.0 或 OpenID Connect）。 保护 Azure Active Directory 的 IoT 空间的几个关键做法包括：

> [!div class="checklist"]
> * 将 Azure Active Directory 应用机密和密钥存储在安全位置，例如 [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)。
> * 使用受信任的[证书颁发机构](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md)颁发的证书，而不是使用应用密钥进行身份验证。
> * 为令牌限制 OAuth 2.0 的访问范围。
> * 验证令牌有效的时间长度以及它是否依然有效。
> * 设置令牌保持有效的适当时间长度。 刷新过期的令牌。
> * [根据基于角色的访问控制最佳实践](#role-based-access-control-best-practices)，删除未使用的**重定向 uri**和权限。

## <a name="role-based-access-control-best-practices"></a>基于角色的访问控制最佳做法

[!INCLUDE [digital-twins-rbac-best-practices](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="next-steps"></a>后续步骤

* 要了解有关 Azure IoT 最佳做法的详细信息，请参阅[物联网安全最佳实践](../iot-fundamentals/iot-security-best-practices.md)。

* 若要了解基于角色的访问控制，请阅读[基于角色的访问控制](./security-role-based-access-control.md)。

* 若要了解身份验证，请阅读[通过 API 进行身份验证](./security-authenticating-apis.md)。
