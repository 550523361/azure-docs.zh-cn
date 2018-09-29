---
title: 在 Azure Blockchain Workbench 中管理用户
description: 如何在 Azure Blockchain Workbench 中管理用户。
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/26/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: ff2c6a2d9b2aec7abc684a4b189ccf31c454aaeb
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2018
ms.locfileid: "47093031"
---
# <a name="manage-users-in-azure-blockchain-workbench"></a>在 Azure Blockchain Workbench 中管理用户

Azure Blockchain Workbench 包含的用户管理功能适用于隶属于联盟的人员和组织。

## <a name="prerequisites"></a>先决条件

需部署 Blockchain Workbench。 有关部署详情，请参阅 [Azure Blockchain Workbench 部署](blockchain-workbench-deploy.md)。

## <a name="add-azure-ad-users"></a>添加 Azure AD 用户

Azure Blockchain Workbench 将 Azure Active Directory (Azure AD) 用于身份验证、访问控制和角色。 Blockchain Workbench Azure AD 租户中的用户可以在进行身份验证后使用 Blockchain Workbench。 向“管理员”应用程序角色添加可以进行交互并执行操作的用户。

在向应用程序和角色分配 Blockchain Workbench 用户之前，该用户必须存在于 Azure AD 租户中。 若要向 Azure AD 添加用户，请执行以下步骤：

1.  登录到 [Azure 门户](https://portal.azure.com)。
2.  在右上角选择帐户，然后切换到与 Blockchain Workbench 关联的 Azure AD 租户。
3.  选择“Azure Active Directory”>“用户”。 此时会看到目录中用户的列表。
4.  若要向目录添加用户，请选择“新用户”。 对于外部用户，请选择“新来宾用户”。

    ![新建用户](media/blockchain-workbench-manage-users/add-ad-user.png)

5.  完成新用户的必填字段。 选择**创建**。

若要更详细地了解如何管理 Azure AD 中的用户，请访问 [Azure AD](../active-directory/fundamentals/add-users-azure-active-directory.md) 文档。

## <a name="manage-blockchain-workbench-administrators"></a>管理 Blockchain Workbench 管理员

将用户添加到目录以后，下一步是选择充当 Blockchain Workbench 管理员的用户。 “管理员”组中的用户与 Blockchain Workbench 中的“管理员”应用程序角色相关联。 管理员可以添加或删除用户、为特定方案分配用户，以及创建新应用程序。

若要向 Azure AD 目录中的“管理员”组添加用户，请执行以下操作：

1.  登录到 [Azure 门户](https://portal.azure.com)。
2.  在右上角选择帐户，验证自己是否存在于与 Blockchain Workbench 关联的 Azure AD 租户中。
3.  选择“Azure Active Directory”>“企业应用程序”。
4.  选择适用于 Blockchain Workbench 的 Azure AD 客户端应用程序
    
    ![所有企业应用程序注册](media/blockchain-workbench-manage-users/select-blockchain-client-app.png)

5.  选择“用户和组”>“添加用户”。
6.  在“添加分配”中，选择“用户”。 选择或搜索需将其作为管理员添加的用户。 选择完以后，单击“选择”。

    ![添加分配](media/blockchain-workbench-manage-users/add-user-assignment.png)

9.  验证是否已将“角色”设置为“管理员”
10. 选择“分配”。 添加的用户显示在列表中，已向其分配管理员角色。

    ![区块链客户端应用用户](media/blockchain-workbench-manage-users/blockchain-admin-list.png)

## <a name="managing-blockchain-workbench-members"></a>管理 Blockchain Workbench 成员

请使用 Blockchain Workbench 应用程序来管理隶属于联盟的用户和组织。 可以添加或删除应用程序和角色的用户。

1. 在浏览器中[打开 Blockchain Workbench](blockchain-workbench-deploy.md#blockchain-workbench-web-url)，以管理员身份登录。

    ![Blockchain Workbench](media/blockchain-workbench-manage-users/blockchain-workbench-applications.png)

    成员将添加到每个应用程序。 成员可以通过一个或多个应用程序角色来启动合同或执行操作。

2. 若要管理应用程序的成员，请在“应用程序”窗格中选择应用程序磁贴。

    与所选应用程序关联的成员数反映在成员磁贴中。

    ![选择应用程序](media/blockchain-workbench-manage-users/blockchain-workbench-select-application.png)


#### <a name="add-member-to-application"></a>将成员添加到应用程序

1. 选择成员磁贴，以便显示当前成员的列表。
2. 选择“添加成员”。

    ![添加成员](media/blockchain-workbench-manage-users/application-add-members.png)

3. 搜索用户的名称。  仅列出存在于 Blockchain Workbench 租户中的 Azure AD 用户。 如果找不到用户，则需[添加 Azure AD 用户](#add-azure-ad-users)。

    ![添加成员](media/blockchain-workbench-manage-users/find-user.png)

4. 从下拉列表中选择一个**角色**。

    ![选择角色成员](media/blockchain-workbench-manage-users/application-select-role.png)

5. 选择“添加”，向应用程序添加具有关联角色的成员。

#### <a name="remove-member-from-application"></a>从应用程序中删除成员

1. 选择成员磁贴，以便显示当前成员的列表。
2. 对于要删除的用户，请从角色下拉列表中选择“删除”。

    ![删除成员](media/blockchain-workbench-manage-users/application-remove-member.png)

#### <a name="change-or-add-role"></a>更改或添加角色

1. 选择成员磁贴，以便显示当前成员的列表。
2. 对于要更改的用户，请单击下拉列表，然后选择新角色。

    ![更改角色](media/blockchain-workbench-manage-users/application-change-role.png)

## <a name="next-steps"></a>后续步骤

本操作指南文章介绍了如何管理 Azure Blockchain Workbench 的用户。 若要了解如何创建区块链应用程序，请继续学习下一篇操作指南文章。

> [!div class="nextstepaction"]
> [在 Azure Blockchain Workbench 中创建区块链应用程序](blockchain-workbench-create-app.md)
