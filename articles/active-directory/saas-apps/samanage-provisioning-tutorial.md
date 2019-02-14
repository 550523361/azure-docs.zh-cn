---
title: 教程：使用 Azure Active Directory 为 Samanage 配置自动用户预配 | Microsoft Docs
description: 了解如何将 Azure Active Directory 配置为自动将用户帐户预配到 Samanage 和取消其预配。
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2018
ms.author: v-wingf-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: d620701bc8590bee746be35f69b0da890c359601
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2019
ms.locfileid: "56205352"
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>教程：为 Samanage 配置自动用户预配

本教程的目的是演示要将 Azure AD 配置为自动将用户和/或组预配到 Samanage 以及取消其预配需在 Samanage 和 Azure Active Directory (Azure AD) 中执行的步骤。

> [!NOTE]
> 本教程介绍在 Azure AD 用户预配服务之上构建的连接器。 有关此服务的功能、工作原理以及常见问题的重要详细信息，请参阅[使用 Azure Active Directory 自动将用户预配到 SaaS 应用程序和取消预配](../manage-apps/user-provisioning.md)。

## <a name="prerequisites"></a>先决条件

本教程中所述的方案假定你已具备以下项：

*   Azure AD 租户
*   拥有专业版套餐的 [Samanage 租户](https://www.samanage.com/pricing/)
*   在 Samanage 中具有管理员权限的用户帐户

> [!NOTE]
> Azure AD 预配集成依赖于 [Samanage Rest API](https://www.samanage.com/api/)，该 Rest API 可供拥有专业版套餐的帐户的 Samanage 开发人员使用。

## <a name="adding-samanage-from-the-gallery"></a>从库中添加 Samanage
在使用 Azure AD 为 Samanage 配置自动用户预配之前，需要从 Azure AD 应用程序库将 Samanage 添加到托管的 SaaS 应用程序列表。

**若要从 Azure AD 应用程序库中添加 Samanage，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。

    ![“Azure Active Directory”按钮][1]

2. 导航到“企业应用程序” > “所有应用程序”。

    ![“企业应用程序”部分][2]

3. 若要添加 Samanage，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮][3]

4. 在搜索框中，键入“Samanage”。

    ![Samanage 预配](./media/samanage-provisioning-tutorial/AppSearch.png)

5. 在结果面板中，选择“Samanage”，然后单击“添加”按钮将 Samanage 添加到 SaaS 应用程序列表。

    ![Samanage 预配](./media/samanage-provisioning-tutorial/AppSearchResults.png)

    ![Samanage 预配](./media/samanage-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-samanage"></a>将用户分配到 Samanage

Azure Active Directory 使用称为“分配”的概念来确定哪些用户应收到对所选应用的访问权限。 在自动用户预配的上下文中，只同步已分配到 Azure AD 中的应用程序的用户和/或组。

在配置和启用自动用户预配之前，应确定 Azure AD 中的哪些用户和/或组需要访问 Samanage。 确定后，可以按照此处的说明将这些用户和/或组分配到 Samanage：

*   [向企业应用分配用户或组](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-samanage"></a>将用户分配到 Samanage 的重要提示

*    现在，Samanage 角色将在 Azure 门户 UI 中自动动态填充。 在将 Samanage 角色分配给用户之前，请确保针对 Samanage 完成初始同步，以检索 Samanage 租户中的最新角色。

*    建议将单个 Azure AD 用户分配到 Samanage 来测试初始自动用户预配配置。 测试成功后，可以稍后分配其他用户和/或组。

*   如果将用户分配到 Samanage，必须在分配对话框中选择任何特定于应用程序的有效角色（如果可用）。 具有“默认访问权限”角色的用户排除在预配之外。

## <a name="configuring-automatic-user-provisioning-to-samanage"></a>配置 Samanage 的自动用户预配

本部分介绍如何配置 Azure AD 预配服务以基于 Azure AD 中的用户和/或组分配在 Samanage 中创建、更新和禁用用户和/或组。

> [!TIP]
> 还可选择按照 [Samanage 单一登录教程](samanage-tutorial.md)中提供的说明为 Samanage 启用基于 SAML 的单一登录。 可以独立于自动用户预配配置单一登录，尽管这两个功能互相补充。

### <a name="to-configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>若要在 Azure AD 中为 Samanage 配置自动用户预配，请执行以下操作：

1. 登录 [Azure 门户](https://portal.azure.com)，并浏览到“Azure Active Directory”>“企业应用程序”>“所有应用程序”。

2. 从 SaaS 应用程序列表中选择 Samanage。

    ![Samanage 预配](./media/samanage-provisioning-tutorial/AppInstanceSearch.png)

3. 选择“预配”选项卡。

    ![Samanage 预配](./media/samanage-provisioning-tutorial/ProvisioningTab.png)

4. 将“预配模式”设置为“自动”。

    ![Samanage 预配](./media/samanage-provisioning-tutorial/ProvisioningCredentials.png)

5. 在“管理员凭据”部分下，输入 Samanage 帐户的“管理员用户名”和“管理员密码”。 这些值的示例如下：

    *   在“管理员用户名”字段中，填入 Samanage 租户的管理员帐户的用户名。 示例：admin@contoso.com。

    *   在“管理员密码”字段中，填入管理员用户名所对应的管理员帐户的密码。

6. 填入步骤 5 中所示的字段后，单击“测试连接”以确保 Azure AD 可以连接到 Samanage。 如果连接失败，请确保 Samanage 帐户具有管理员权限，然后重试。

    ![Samanage 预配](./media/samanage-provisioning-tutorial/TestConnection.png)

7. 在“通知电子邮件”字段中，输入应接收预配错误通知的个人或组的电子邮件地址，并选中复选框“发生故障时发送电子邮件通知”。

    ![Samanage 预配](./media/samanage-provisioning-tutorial/EmailNotification.png)

8. 单击“ **保存**”。

9. 在“映射”部分下，选择“将 Azure Active Directory 用户同步到 Samanage”。

    ![Samanage 预配](./media/samanage-provisioning-tutorial/UserMappings.png)

10. 在“属性映射”部分中，查看从 Azure AD 同步到 Samanage 的用户属性。 选为“匹配”属性的特性用于匹配 Samanage 中的用户帐户以执行更新操作。 选择“保存”按钮以提交任何更改。

    ![Samanage 预配](./media/samanage-provisioning-tutorial/UserAttributeMapping.png)

11. 若要启用组映射，在“映射”部分下，选择“将 Azure Active Directory 组同步到 Samanage”。

    ![Samanage 预配](./media/samanage-provisioning-tutorial/GroupMappings.png)

12. 将“启用”设置为“是”将同步组。 在“属性映射”部分中，查看从 Azure AD 同步到 Samanage 的组属性。 选为“匹配”属性的特性用于匹配 Samanage 中的用户帐户以执行更新操作。 选择“保存”按钮以提交任何更改。

    ![Samanage 预配](./media/samanage-provisioning-tutorial/GroupAttributeMapping.png)

13. 若要配置范围筛选器，请参阅[范围筛选器教程](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)中提供的以下说明。

14. 若要为 Samanage 启用 Azure AD 预配服务，请在“设置”部分中将“预配状态”更改为“启用”。

    ![Samanage 预配](./media/samanage-provisioning-tutorial/ProvisioningStatus.png)

15. 通过在“设置”部分的“范围”中选择所需的值，定义要预配到 Samanage 的用户和/或组。 选择“同步所有用户和组”选项时，请考虑下面“连接器限制”部分中所述的限制。

    ![Samanage 预配](./media/samanage-provisioning-tutorial/ScopeSync.png)

16. 已准备好预配时，单击“保存”。

    ![Samanage 预配](./media/samanage-provisioning-tutorial/SaveProvisioning.png)


此操作会对“设置”部分的“范围”中定义的所有用户和/或组启动初始同步。 初始同步执行的时间比后续同步长，只要 Azure AD 预配服务正在运行，大约每隔 40 分钟就会进行一次同步。 可使用“同步详细信息”部分监视进度并跟踪指向预配活动报告的链接，这些报告描述了 Azure AD 预配服务对 Samanage 执行的所有操作。

若要详细了解如何读取 Azure AD 预配日志，请参阅[有关自动用户帐户预配的报告](../manage-apps/check-status-user-account-provisioning.md)。

## <a name="connector-limitations"></a>连接器限制

* 如果选择了“同步所有用户和组”选项并为 Samanage 角色属性配置了默认值，请确保“如果为 null，则为默认值(可选)”字段下的所需值采用以下格式表示：{"displayName":"role"}，其中“role”是所需的默认值。

## <a name="additional-resources"></a>其他资源

* [管理企业应用的用户帐户预配](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [什么是使用 Azure Active Directory 的应用程序访问和单一登录？](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>后续步骤

* [了解如何查看日志并获取有关预配活动的报告](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/samanage-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/samanage-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/samanage-provisioning-tutorial/tutorial_general_03.png
