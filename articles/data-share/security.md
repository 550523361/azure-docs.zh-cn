---
title: Azure Data Share 的安全性概述
description: Azure Data Share 的安全性概述
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 06/05/2020
ms.openlocfilehash: 5d815c27ecc7825f0bc1e6772654b094a799b63d
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92216549"
---
# <a name="security-overview-for-azure-data-share"></a>Azure Data Share 的安全性概述

本文提供 Azure 数据共享服务的安全性概述。

## <a name="security-overview"></a>安全概述

Azure Data Share 利用 Azure 提供的基础安全措施来保护静态数据和传输中的数据。 将会对静态数据加密，通过基础数据存储提供支持。 也会对传输中的数据加密。 此外还会对静态的和传输中的数据共享元数据加密。 

可以在 Azure Data Share 资源级别设置访问控制，确保它由那些获得授权的用户访问。 

Azure 数据共享利用之前称为 MSI) 的托管标识来访问用于数据共享的数据存储 (。 在数据提供者和数据使用者之间没有凭据交换。 有关托管标识的详细信息，请参阅 [Azure 资源的托管标识](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)。 有关共享数据所需的角色和权限的详细信息，请参阅 [角色和要求](concepts-roles-permissions.md)。

## <a name="next-steps"></a>后续步骤

若要了解如何开始共享数据，请继续阅读[共享数据](share-your-data.md)教程。