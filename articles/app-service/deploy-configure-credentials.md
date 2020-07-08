---
title: 配置部署凭据
description: 了解 Azure 应用服务中的部署凭据类型以及如何配置和使用它们。
ms.topic: article
ms.date: 08/14/2019
ms.reviewer: byvinyal
ms.custom: seodec18
ms.openlocfilehash: c6f7c2422e043da6df498fe81da938576687b916
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2020
ms.locfileid: "83649113"
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>为 Azure 应用服务配置部署凭据
[Azure 应用服务](https://go.microsoft.com/fwlink/?LinkId=529714)支持两种类型的凭据，这些凭据适用于[本地 GIT 部署](deploy-local-git.md)和 [FTP/S 部署](deploy-ftp.md)。 这些凭据与 Azure 订阅凭据不同。

[!INCLUDE [app-service-deploy-credentials](../../includes/app-service-deploy-credentials.md)]

## <a name="configure-user-level-credentials"></a><a name="userscope"></a>配置用户级别凭据

可以在应用的[资源页](../azure-resource-manager/management/manage-resources-portal.md#manage-resources)中配置用户级凭据。 不管是在哪个应用中配置这些凭据，该凭据适用于所有应用以及 Azure 帐户中的所有订阅。 

### <a name="in-the-cloud-shell"></a>在 Cloud Shell 中

若要在 [Cloud Shell](https://shell.azure.com) 中配置部署用户，请运行 [az webapp deployment user set](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) 命令。 将 \<username> 和 \<password> 替换为部署用户名和密码。 

- 用户名在 Azure 中必须唯一，并且为了本地Git推送，不能包含“@”符号。 
- 密码必须至少为 8 个字符，且具有字母、数字和符号这三种元素中的两种。 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

JSON 输出会将该密码显示为 `null`。 如果收到 `'Conflict'. Details: 409` 错误，请更改用户名。 如果收到 `'Bad Request'. Details: 400` 错误，请使用更强的密码。 

### <a name="in-the-portal"></a>在门户中

在 Azure 门户中，必须至少有一个应用，才能访问“部署凭据”页。 配置用户级别凭据的步骤：

1. 在 [Azure](https://portal.azure.com) 门户中，选择左侧菜单中的“应用服务” > “\<any_app>” > “部署中心” > “FTP” > “仪表板”。

    ![](./media/app-service-deployment-credentials/access-no-git.png)

    或者，如果已配置 Git 部署，请选择“应用服务” > “&lt;any_app>” > “部署中心” > “FTP/凭据”。

    ![](./media/app-service-deployment-credentials/access-with-git.png)

2. 选择“用户凭据”，配置用户名和密码，然后选择“保存凭据”。

设置部署凭据以后，即可在应用的“概述”页中查找 Git 部署用户名，

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

如果配置了 Git 部署，页面将显示“Git/部署用户名”；否则显示“FTP/部署用户名”。

> [!NOTE]
> Azure 不会显示用户级部署密码。 如果忘记密码，可以按照本部分的步骤重置凭据。
>
> 

## <a name="use-user-level-credentials-with-ftpftps"></a>对 FTP/FTPS 使用用户级凭据

使用用户级凭据对 FTP/FTPS 终结点进行身份验证时，需要以下格式的用户名：`<app-name>\<user-name>`

由于用户级凭据链接到用户而不是特定资源，因此用户名必须采用这种格式才能将登录操作定向到正确的应用终结点。

## <a name="get-and-reset-app-level-credentials"></a><a name="appscope"></a>获取和重置应用级凭据
获取应用级凭据：

1. 在 [Azure](https://portal.azure.com) 门户中，选择左侧菜单中的“应用服务” > “&lt;any_app>” > “部署中心” > “FTP/凭据”。

2. 选择“应用凭据”，然后选择“复制”链接以复制用户名或密码。

若要重置应用级别凭据，请选择相同对话框中的“重置凭据”。

## <a name="next-steps"></a>后续步骤

了解如何使用这些凭据通过[本地 Git](deploy-local-git.md) 或 [FTP/S](deploy-ftp.md) 部署应用。
