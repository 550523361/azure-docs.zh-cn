---
title: Azure 区块链令牌帐户管理
description: 使用 Azure 区块链令牌帐户管理，可以创建组并链接区块链帐户，以控制对区块链操作的访问。
ms.date: 11/04/2019
ms.topic: conceptual
ms.reviewer: brendal
ms.openlocfilehash: 9931ef59e613501ba6feaedf3ac5d4721f0df752
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "74326102"
---
# <a name="azure-blockchain-tokens-account-management"></a>Azure 区块链令牌帐户管理

[!INCLUDE [Preview note](./includes/preview.md)]

对于区块链解决方案，用户可能需要不同级别的访问权限，才能使用 Azure 区块链令牌服务创建的令牌。 在大多数区块链方案中，需要规划和部署在分类帐上存在的不同区块链帐户。 还需要跨参与者管理访问权限。使用 Azure 区块链令牌帐户管理，可以创建组并链接区块链帐户，以控制对区块链操作的访问。

## <a name="blockchain-networks"></a>区块链网络

Azure 区块链令牌可在一组区块链网络中部署和管理令牌。 可以将单个区块链分类帐或几个区块链分类帐连接到服务。

## <a name="accounts"></a>帐户

对于连接到 Azure 区块链令牌的区块链网络，服务将创建和管理帐户私钥对，并执行事务签名和提交。 Azure 区块链令牌还提供标识映射，以匹配分类帐上具有公钥标识的帐户。

## <a name="groups"></a>组

利用组，你可以跨连接的网络管理大量区块链帐户。 你可以跟踪和审核目录中哪些应用程序和用户能够通过 Azure 区块链令牌 Api 使用帐户。 例如，你可以将表示不同业务线或不同角色的一组帐户分组，并对区块链令牌进行分组。

你还可以将组关联到 Azure Active Directory 的用户或服务主体，并且此主体有权访问组及其关联的帐户。  

## <a name="next-steps"></a>后续步骤

详细了解可用的 [Azure Blockchain Tokens 模板](templates.md)。
