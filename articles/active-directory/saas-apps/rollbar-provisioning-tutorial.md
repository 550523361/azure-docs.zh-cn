---
title: 教程：为 Rollbar 配置自动用户预配 Azure Active Directory |Microsoft Docs
description: 了解如何配置 Azure Active Directory 以自动将用户帐户预配到 Rollbar 以及取消其预配。
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: d737aa16-8ab4-4c0c-a68b-2911623b41eb
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: Zhchia
ms.openlocfilehash: 27a26a0c8378f34794afd87cf11b6bb878f7b53c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "78248448"
---
# <a name="tutorial-configure-rollbar-for-automatic-user-provisioning"></a>教程：为 Rollbar 配置自动用户预配

本教程介绍了需要在 Rollbar 和 Azure Active Directory （Azure AD）中执行的步骤，以配置自动用户预配。 配置后，Azure AD 使用 Azure AD 预配服务自动设置用户和组并取消其预配到[Rollbar](https://rollbar.com/pricing/) 。 有关此服务的功能、工作原理以及常见问题的重要详细信息，请参阅[使用 Azure Active Directory 自动将用户预配到 SaaS 应用程序和取消预配](../manage-apps/user-provisioning.md)。 


## <a name="capabilities-supported"></a>支持的功能
> [!div class="checklist"]
> * 在 Rollbar 中创建用户
> * 当用户不再需要访问权限时，删除 Rollbar 中的用户
> * 使用户属性在 Azure AD 和 Rollbar 之间保持同步
> * 在 Rollbar 中预配组和组成员身份
> * [单一登录](https://docs.microsoft.com/azure/active-directory/saas-apps/rollbar-tutorial)到 Rollbar （推荐）

## <a name="prerequisites"></a>先决条件

本教程中概述的方案假定你已具有以下先决条件：

* [Azure AD 租户](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* 具有配置预配[权限](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)的 Azure AD 用户帐户（例如应用程序管理员、云应用程序管理员、应用程序所有者或全局管理员）。 
* 具有企业计划的[Rollbar 租户](https://rollbar.com/pricing/)。
* Rollbar 中具有管理员权限的用户帐户。

## <a name="step-1-plan-your-provisioning-deployment"></a>步骤 1。 规划预配部署
1. 了解[预配服务的工作原理](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)。
2. 确定谁在[预配范围](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)中。
3. 确定要[在 Azure AD 与 Rollbar 之间映射](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)的数据。 

## <a name="step-2-configure-rollbar-to-support-provisioning-with-azure-ad"></a>步骤 2。 配置 Rollbar 以支持 Azure AD 的预配

将 Rollbar 配置为使用 Azure AD 进行自动用户预配之前，需要在 Rollbar 上启用 SCIM 设置。

1. 登录到[Rollbar 管理控制台](https://rollbar.com/login/)。 单击 "**帐户设置**"。

    ![Rollbar 管理控制台](media/rollbar-provisioning-tutorial/image00.png)

2. 导航到**Rollbar 租户名称 > 标识提供者**。

    ![Rollbar 标识提供者](media/rollbar-provisioning-tutorial/idp.png)

3. 向下滚动到 "**预配选项**"。 复制访问令牌。 此值将在 Azure 门户的 Rollbar 应用程序的 "预配" 选项卡的 "**机密令牌**" 字段中输入。 选中 "**启用用户和团队设置**" 复选框，并单击 "**保存**"。

    ![Rollbar 访问令牌](media/rollbar-provisioning-tutorial/token.png)


## <a name="step-3-add-rollbar-from-the-azure-ad-application-gallery"></a>步骤 3. 从 Azure AD 应用程序库添加 Rollbar

从 Azure AD 应用程序库中添加 Rollbar，开始管理预配到 Rollbar。 如果以前为 SSO 设置了 Rollbar，则可以使用相同的应用程序。 但建议你在最初测试集成时创建一个单独的应用。 可在[此处](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)详细了解如何从库中添加应用程序。 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>步骤 4. 定义谁在预配范围中 

使用 Azure AD 预配服务，可以根据对应用程序的分配和/或用户/组的属性来限定谁在预配范围内。 如果选择根据分配来查看要将谁预配到应用，则可以使用以下[步骤](../manage-apps/assign-user-or-group-access-portal.md)将用户和组分配给应用程序。 如果选择仅根据用户或组的属性来限定要对谁进行预配，可以使用[此处](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)所述的范围筛选器。 

* 将用户和组分配到 Rollbar 时，必须选择 "**默认" 访问权限**以外的其他角色。 具有“默认访问”角色的用户将从预配中排除，并在预配日志中被标记为未有效授权。 如果应用程序上唯一可用的角色是默认访问角色，则可以[更新应用程序清单](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps)以添加其他角色。 

* 先小部分测试。 在向全员推出之前，请先使用少量的用户和组进行测试。 如果预配范围设置为分配的用户和组，则可以先尝试将一两个用户或组分配到应用。 当预配范围设置为所有用户和组时，可以指定[基于属性的范围筛选器](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)。 


## <a name="step-5-configure-automatic-user-provisioning-to-rollbar"></a>步骤 5。 配置 Rollbar 的自动用户预配 

本部分介绍了如何配置 Azure AD 预配服务以基于 Azure AD 中的用户和/或组分配在 TestApp 中创建、更新和禁用用户和/或组。

### <a name="to-configure-automatic-user-provisioning-for-rollbar-in-azure-ad"></a>若要在 Azure AD 中配置 Rollbar 的自动用户预配：

1. 登录 [Azure 门户](https://portal.azure.com)。 依次选择“企业应用程序”、“所有应用程序” 。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

2. 在应用程序列表中，选择“Rollbar”****。

    ![应用程序列表中的 Rollbar 链接](common/all-applications.png)

3. 选择“预配”选项卡。

    ![预配选项卡](common/provisioning.png)

4. 将“预配模式”设置为“自动”。

    ![“预配”选项卡](common/provisioning-automatic.png)

5. 在 "**管理员凭据**" 部分中，输入之前在 "**机密令牌**" 中检索到的访问令牌值。 单击 "**测试连接**" 以确保 Azure AD 可以连接到 Rollbar。 如果连接失败，请确保 Rollbar 帐户具有管理员权限，然后重试。

    ![设置](./media/rollbar-provisioning-tutorial/admin.png)

6. 在“通知电子邮件”字段中，输入应接收预配错误通知的个人或组的电子邮件地址，并选中“发生故障时发送电子邮件通知”复选框 。

    ![通知电子邮件](common/provisioning-notification-email.png)

7. 选择“保存”。

8. 在 "**映射**" 部分下，选择 "**将 Azure Active Directory 用户同步到 Rollbar**"。

9. 在 "**属性映射**" 部分中，查看从 Azure AD 同步到 Rollbar 的用户属性。 选为 "**匹配**" 属性的特性用于匹配 Rollbar 中的用户帐户以执行更新操作。 如果选择更改[匹配的目标属性](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)，将需要确保 Rollbar API 支持基于该属性筛选用户。 选择“保存”按钮以提交任何更改。

   |Attribute|类型|
   |---|---|
   |userName|字符串|
   |externalId|字符串|
   |活动|Boolean|
   |name.familyName|String|
   |name.givenName|String|
   |电子邮件 [类型 eq "work"]|String|

10. 在 "**映射**" 部分下，选择 "**将 Azure Active Directory 组同步到 Rollbar**"。

11. 在 "**属性映射**" 部分中，查看从 Azure AD 同步到 Rollbar 的组属性。 选为 "**匹配**" 属性的特性用于匹配 Rollbar 中的组以执行更新操作。 选择“保存”按钮以提交任何更改。

      |Attribute|类型|
      |---|---|
      |displayName|String|
      |externalId|字符串|
      |members|参考|

12. 若要配置范围筛选器，请参阅[范围筛选器教程](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)中提供的以下说明。

13. 若要为 Rollbar 启用 Azure AD 预配服务，请在 "**设置**" 部分中将 "**预配状态**" 更改为 **"打开**"。

    ![预配状态已打开](common/provisioning-toggle-on.png)

14. 通过在 "**设置**" 部分的 "**范围**" 中选择所需的值，定义要预配到 Rollbar 的用户和/或组。

    ![预配范围](common/provisioning-scope.png)

15. 已准备好预配时，单击“保存”。

    ![保存预配配置](common/provisioning-configuration-save.png)

此操作会对“设置”部分的“范围”中定义的所有用户和组启动初始同步周期 。 初始周期执行的时间比后续周期长，只要 Azure AD 预配服务正在运行，后续周期大约每隔 40 分钟就会进行一次。 

## <a name="step-6-monitor-your-deployment"></a>步骤 6. 监视部署
配置预配后，请使用以下资源来监视部署：

1. 通过[预配日志](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs)来确定哪些用户已预配成功或失败
2. 检查[进度栏](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user)来查看预配周期的状态以及完成进度
3. 如果怀疑预配配置处于非正常状态，则应用程序将进入隔离状态。 可在[此处](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)了解有关隔离状态的详细信息。

## <a name="additional-resources"></a>其他资源

* [管理企业应用的用户帐户预配](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>后续步骤

* [了解如何查看日志并获取有关预配活动的报告](../manage-apps/check-status-user-account-provisioning.md)
