---
title: Visual Studio 中的 Key Vault 连接服务入门（ASP.NET Core 项目）| Microsoft Docs
description: 使用本教程帮助你了解如何将 Key Vault 支持添加到 ASP.NET 或 ASP.NET Core Web 应用程序。
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: e591771ee9c9cb12d9ec2ff61ec7f5a76691c8c7
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2018
ms.locfileid: "42145091"
---
# <a name="get-started-with-key-vault-connected-service-in-visual-studio-aspnet-core-projects"></a>Visual Studio 中的 Key Vault 连接服务入门（ASP.NET Core 项目）

> [!div class="op_single_selector"]
> - [入门](vs-key-vault-aspnet-core-get-started.md)
> - [发生了什么情况](vs-key-vault-aspnet-core-what-happened.md)

本文提供通过 Visual Studio 中的“添加连接服务”命令将 Key Vault 添加到 ASP.NET Core 项目后的其他指南。 如果尚未将该服务添加到项目，可以随时按照[使用 Visual Studio 连接服务将 Key Vault 添加到 Web 应用程序](vs-key-vault-add-connected-service.md)中的说明来执行此操作。

请参阅[我的 ASP.NET Core 项目发生了什么情况？](vs-key-vault-aspnet-core-what-happened.md)，了解添加连接服务时对项目所做的更改。

## <a name="after-you-connect"></a>连接后

1. 在 Azure 中将机密添加到 Key Vault。 若要转到门户中的相应位置，请单击用于管理此 Key Vault 中存储的机密的链接。 如果已关闭该页或项目，可以在 [Azure 门户](https://portal.azure.com)中通过选择“安全性”下的“所有服务”导航到它，选择 **Key Vault**，然后选择刚刚创建的 Key Vault。

   ![导航到门户](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. 在创建的 Key Vault 的 Key Vault 部分，依次选择“机密”、“生成/导入”。

   ![生成/导入机密](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. 输入一个机密（例如“MySecret”），并为其提供任何字符串值作为测试，然后选择“创建”按钮。

   ![创建机密](media/vs-key-vault-add-connected-service/create-a-secret.jpg)
 
1. （可选）输入另一个机密，但这一次通过将其命名为“Secrets--MySecret”将其放入某个类别。 此语法指定的类别 **Secrets** 包含机密 **MySecret**。
1. 在 Visual Studio 项目中，现在可以通过在代码中使用以下表达式来引用这些机密：
 
   ```csharp
      config["MySecret"] // Access a secret without a section
      config["Secrets:MySecret"] // Access a secret in a section
      config.GetSection("Secrets")["MySecret"] // Get the configuration section and access a secret in it.
   ```

祝贺你，现已允许 Web 应用使用 Key Vault 安全地访问存储的机密。

## <a name="clean-up-resources"></a>清理资源

不再需要资源组时，可将其删除。 这会删除 Key Vault 和相关的资源。 若要通过门户删除资源组，请执行以下操作：

1. 在门户顶部的“搜索”框中输入资源组的名称。 在搜索结果中看到在本快速入门中使用的资源组后，请将其选中。
2. 选择“删除资源组”。
3. 在“键入资源组名称:”框中，键入资源组的名称，然后选择“删除”。

# <a name="next-steps"></a>后续步骤

在 [Key Vault 开发人员指南](key-vault-developers-guide.md)中了解如何使用 Key Vault 进行开发