---
title: 教程：为 GroupTalk 配置自动用户预配 Azure Active Directory |Microsoft Docs
description: 了解如何自动将用户 Azure AD 帐户预配到 GroupTalk 以及取消其预配。
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: e537d393-2724-450f-9f5b-4611cdc9237c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/18/2020
ms.author: Zhchia
ms.openlocfilehash: 107ce33f753b275f694558ec1fec2f07e2316b36
ms.sourcegitcommit: 2e9643d74eb9e1357bc7c6b2bca14dbdd9faa436
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031146"
---
# <a name="tutorial-configure-grouptalk-for-automatic-user-provisioning"></a>教程：为 GroupTalk 配置自动用户预配

本教程介绍了需要在 GroupTalk 和 Azure Active Directory (Azure AD) 中执行的步骤，以配置自动用户预配。 配置后，Azure AD 使用 Azure AD 预配服务自动设置用户和组并取消其预配到 [GroupTalk](https://www.grouptalk.com/) 。 有关此服务的功能、工作原理以及常见问题的重要详细信息，请参阅[使用 Azure Active Directory 自动将用户预配到 SaaS 应用程序和取消预配](../manage-apps/user-provisioning.md)。 


## <a name="capabilities-supported"></a>支持的功能
> [!div class="checklist"]
> * 在 GroupTalk 中创建用户
> * 当用户不再需要访问权限时，删除 GroupTalk 中的用户
> * 使用户属性在 Azure AD 和 GroupTalk 之间保持同步
> * 在 GroupTalk 中预配组和组成员身份

## <a name="prerequisites"></a>先决条件

本教程中概述的方案假定你已具有以下先决条件：

* [Azure AD 租户](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 中[有权](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)配置预配的用户帐户（例如应用管理员、云应用管理员、应用所有者或全局管理员）。 
* GroupTalk 中具有管理员权限的用户帐户。

## <a name="step-1-plan-your-provisioning-deployment"></a>步骤 1。 规划预配部署
1. 了解[预配服务的工作原理](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)。
2. 确定谁在[预配范围](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)中。
3. 确定要 [在 Azure AD 与 GroupTalk 之间映射](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)的数据。 

## <a name="step-2-configure-grouptalk-to-support-provisioning-with-azure-ad"></a>步骤 2。 配置 GroupTalk 以支持 Azure AD 的预配

1. 联系 GroupTalk 支持， support@grouptalk.com 其中包含要与 Azure AD 集成的 **租户名称** 和 **ID** 。
2. 当你收到 Azure AD 集成所需的设置已准备就绪时，请登录 GroupTalk 管理员并导航到你的组织视图。 
3. Azure AD 集成配置项目应可见。 单击它以验证 **租户名称** 和 **ID**  以获取 **JWT (机密令牌)**。 
4. GroupTalk 租户 URL 是 `https://api.grouptalk.com/api/scim/` 。 在上一步中检索到的 **租户 URL** 和 **机密令牌** 将在 Azure 门户的 GroupTalk 应用程序的 "预配" 选项卡中输入。 

## <a name="step-3-add-grouptalk-from-the-azure-ad-application-gallery"></a>步骤 3. 从 Azure AD 应用程序库添加 GroupTalk

从 Azure AD 应用程序库中添加 **GroupTalk** ，开始管理预配到 GroupTalk。

1. 单击 " **注册 GroupTalk** " 按钮，这会将你路由到 GroupTalk 管理应用程序。
2. 如果你已登录到 GroupTalk，请注销以转到登录屏幕。 选择 "Azure AD" 选项卡，并单击 " **登录** " 按钮。

    ![GroupTalk](media/grouptalk-provisioning-tutorial/login.png)

3. 使用你的 AD 管理帐户登录，并接受 GroupTalk 应用程序的访问权限。 此操作完成后，你将收到一条错误消息，指出该用户不存在。 这是预期的，因为用户尚未预配到 GroupTalk，但你现在已将 GroupTalk 添加到租户。
4. 返回到 Azure 门户，并验证 **GroupTalk** 是否现已添加到企业应用程序。

可在[此处](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)详细了解如何从库中添加应用程序。 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>步骤 4. 定义谁在预配范围中 

使用 Azure AD 预配服务，可以根据对应用程序的分配和/或用户/组的属性来限定谁在预配范围内。 如果选择根据分配来查看要将谁预配到应用，则可以使用以下[步骤](../manage-apps/assign-user-or-group-access-portal.md)将用户和组分配给应用程序。 如果选择仅根据用户或组的属性来限定要对谁进行预配，可以使用[此处](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)所述的范围筛选器。 

* 将用户和组分配到 GroupTalk 时，必须选择 " **默认" 访问权限** 以外的其他角色。 具有“默认访问”角色的用户将从预配中排除，并在预配日志中被标记为未有效授权。 如果应用程序上唯一可用的角色是默认访问角色，则可以[更新应用程序清单](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps)以添加其他角色。 

* 先小部分测试。 在向全员推出之前，请先使用少量的用户和组进行测试。 如果预配范围设置为分配的用户和组，则可以先尝试将一两个用户或组分配到应用。 当预配范围设置为所有用户和组时，可以指定[基于属性的范围筛选器](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)。 


## <a name="step-5-configure-automatic-user-provisioning-to-grouptalk"></a>步骤 5。 配置 GroupTalk 的自动用户预配 

本部分介绍了如何配置 Azure AD 预配服务以基于 Azure AD 中的用户和/或组分配在 TestApp 中创建、更新和禁用用户和/或组。

### <a name="to-configure-automatic-user-provisioning-for-grouptalk-in-azure-ad"></a>若要在 Azure AD 中配置 GroupTalk 的自动用户预配：

1. 登录 [Azure 门户](https://portal.azure.com)。 依次选择“企业应用程序”、“所有应用程序” 。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

2. 在应用程序列表中，选择 " **GroupTalk**"。

    ![应用程序列表中的 GroupTalk 链接](common/all-applications.png)

3. 选择“预配”选项卡。

    ![“预配”选项卡](common/provisioning.png)

4. 将“预配模式”设置为“自动”。

    ![“预配”选项卡“自动”](common/provisioning-automatic.png)

5. 在 " **管理员凭据** " 部分下，输入你在步骤2之前检索到的 GROUPTALK 租户 URL 和机密令牌。 单击 " **测试连接** " 以确保 Azure AD 可以连接到 GroupTalk。 如果连接失败，请确保 GroupTalk 帐户具有管理员权限，然后重试。 你始终可以获取新的机密令牌，如步骤2中所述。

    ![令牌](common/provisioning-testconnection-tenanturltoken.png)

6. 在“通知电子邮件”字段中，输入应接收预配错误通知的个人或组的电子邮件地址，并选中“发生故障时发送电子邮件通知”复选框 。

    ![通知电子邮件](common/provisioning-notification-email.png)

7. 选择“保存”。

8. 在 " **映射** " 部分下，选择 " **将 Azure Active Directory 用户同步到 GroupTalk**"。

9. 在 " **属性映射** " 部分中，查看从 Azure AD 同步到 GroupTalk 的用户属性。 选为 " **匹配** " 属性的特性用于匹配 GroupTalk 中的用户帐户以执行更新操作。 如果选择更改 [匹配的目标属性](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)，将需要确保 GroupTalk API 支持基于该属性筛选用户。 选择“保存”按钮以提交任何更改。

   |Attribute|类型|支持筛选|
   |---|---|---|
   |userName|字符串|&check;|
   |phoneNumbers[type eq "mobile"].value|字符串|&check;|
   |emails[type eq "work"].value|字符串|&check;|
   |活动|Boolean|
   |name.givenName|字符串|
   |name.familyName|字符串|
   |externalId|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:costCenter|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:organization|字符串|
   |urn： ietf： params： scim：架构：扩展： grouptalk：2.0： User： label1|String|
   |urn： ietf： params： scim：架构：扩展： grouptalk：2.0： User： label2|String|
   |urn： ietf： params： scim：架构：扩展： grouptalk：2.0： User：标签3|String|
   |urn： ietf： params： scim：架构：扩展： grouptalk：2.0： User： label4|String|
   |urn： ietf： params： scim：架构：扩展： grouptalk：2.0： User： label5|String|


10. 在 " **映射** " 部分下，选择 " **将 Azure Active Directory 组同步到 GroupTalk**"。

11. 在 " **属性映射** " 部分中，查看从 Azure AD 同步到 GroupTalk 的组属性。 选为 " **匹配** " 属性的特性用于匹配 GroupTalk 中的组以执行更新操作。 选择“保存”按钮以提交任何更改。

      |Attribute|类型|支持筛选|
      |---|---|---|
      |displayName|字符串|&check;|
      |members|参考|
      |externalId|字符串|      
      |urn： ietf： params： scim：架构：扩展： grouptalk：2.0：组：说明|字符串|

12. 若要配置范围筛选器，请参阅[范围筛选器教程](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)中提供的以下说明。

13. 若要为 GroupTalk 启用 Azure AD 预配服务，请在 "**设置**" 部分中将 "**预配状态**" 更改为 **"打开**"。

    ![预配状态已打开](common/provisioning-toggle-on.png)

14. 通过在 "**设置**" 部分的 "**范围**" 中选择所需的值，定义要预配到 GroupTalk 的用户和/或组。

    ![预配范围](common/provisioning-scope.png)

15. 已准备好预配时，单击“保存”  。

    ![保存预配配置](common/provisioning-configuration-save.png)

此操作会对“设置”部分的“范围”中定义的所有用户和组启动初始同步周期 。 初始周期执行的时间比后续周期长，只要 Azure AD 预配服务正在运行，后续周期大约每隔 40 分钟就会进行一次。 

## <a name="step-6-monitor-your-deployment"></a>步骤 6. 监视部署
配置预配后，请使用以下资源来监视部署：

1. 通过[预配日志](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs)来确定哪些用户已预配成功或失败
2. 检查[进度栏](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user)来查看预配周期的状态以及完成进度
3. 如果怀疑预配配置处于非正常状态，则应用程序将进入隔离状态。 可在[此处](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)了解有关隔离状态的详细信息。
4. 可以联系 GroupTalk 支持，以便在 GroupTalk 管理员内将其他预配日志设置为自定义报表。它们可能会提供有关用户和组未能正确预配的其他提示。

## <a name="additional-resources"></a>其他资源

* [管理企业应用的用户帐户预配](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>后续步骤

* [了解如何查看日志并获取有关预配活动的报告](../manage-apps/check-status-user-account-provisioning.md)
