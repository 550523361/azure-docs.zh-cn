---
title: 如何为 Azure Key Vault 生成和传输受 HSM 保护的密钥 - Azure Key Vault | Microsoft Docs
description: 使用这篇文章可帮助你规划、生成然后传输自己的受 HSM 保护的密钥，以便与 Azure 密钥保管库一起使用。 也称为 BYOK 或自带密钥。
services: key-vault
author: amitbapat
manager: devtiw
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: ambapat
ms.openlocfilehash: 58cf3358a9e908070ce9003d05dd0b576b1d2d3f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "81429689"
---
# <a name="import-hsm-protected-keys-to-key-vault"></a>将 HSM 保护的密钥导入 Key Vault

为了提高可靠性，在使用 Azure 密钥保管库时，可以在硬件安全模块 (HSM) 中导入或生成永不离开 HSM 边界的密钥。 这种情况通常被称为*自带密钥*，简称 BYOK。 Azure Key Vault 使用 Hsm （FIPS 140-2 Level 2）的 nCipher nShield 系列来保护密钥。

此功能不适用于 Azure 中国世纪互联。

> [!NOTE]
> 有关 Azure Key Vault 的详细信息，请参阅[什么是 Azure Key Vault？](../general/overview.md)  
> 如需包括为受 HSM 保护的密钥创建密钥保管库的入门教程，请参阅[什么是 Azure 密钥保管库？](../general/overview.md)。

## <a name="supported-hsms"></a>支持的 Hsm

通过两种不同的方法（具体取决于所使用的 Hsm）支持将受 HSM 保护的密钥传输到 Key Vault。 使用下表确定应该使用哪种方法来生成 Hsm，然后传输自己的受 HSM 保护的密钥，以便与 Azure Key Vault 一起使用。 

|供应商名称|供应商类型|支持的 HSM 模型|支持的 HSM 密钥传输方法|
|---|---|---|---|
|nCipher|制造商|<ul><li>Hsm 系列 nShield</li></ul>|[使用旧的 BYOK 方法](hsm-protected-keys-legacy.md)|
|Thales|制造商|<ul><li>SafeNet Luna HSM 7 系列固件版本7.3 或更高版本</li></ul>| [使用 new BYOK 方法（预览版）](hsm-protected-keys-vendor-agnostic-byok.md)|
|Fortanix|HSM 即服务|<ul><li>自我防御密钥管理服务（SDKMS）</li></ul>|[使用 new BYOK 方法（预览版）](hsm-protected-keys-vendor-agnostic-byok.md)|


## <a name="next-steps"></a>后续步骤

按照[Key Vault 最佳实践](../general/best-practices.md)，确保密钥的安全性、持久性和监视。
