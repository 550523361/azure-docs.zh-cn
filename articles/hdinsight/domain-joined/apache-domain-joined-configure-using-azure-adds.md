---
title: 使用 Azure AD DS 配置已加入域的 HDInsight 群集
description: 了解如何使用 Azure Active Directory 域服务设置和配置已加入域的 HDInsight 群集
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 07/17/2018
ms.openlocfilehash: 17924b0a00f4605d41492768b0124c583664aca6
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "43042135"
---
# <a name="configure-a-domain-joined-hdinsight-cluster-by-using-azure-active-directory-domain-services"></a>使用 Azure Active Directory 域服务配置已加入域的 HDInsight 群集

已加入域的群集在 Azure HDInsight 群集上提供多用户访问。 已加入域的 HDInsight 群集连接到域，使域用户能够使用其域凭据对群集进行身份验证和运行大数据作业。 

本文介绍如何使用 Azure Active Directory 域服务 (Azure AD DS) 配置已加入域的 HDInsight 群集。

## <a name="enable-azure-ad-ds"></a>启用 Azure AD DS

要想能够创建已加入域的 HDInsight 群集，必须先启用 Azure AD DS。 有关详细信息，请参阅[使用 Azure 门户启用 Azure Active Directory 域服务](../../active-directory-domain-services/active-directory-ds-getting-started.md)。 

> [!NOTE]
> 只有租户管理员有权创建 Azure AD DS 实例。 如果将 Azure Data Lake Storage Gen1 用作 HDInsight 的默认存储，请确保 Data Lake Storage Gen1 的默认 Azure AD 租户与 HDInsight 群集的域相同。 由于 Hadoop 依赖于 Kerberos 和基本身份验证，因此需对有权访问群集的用户禁用多重身份验证。

预配 Azure AD DS 实例后，在 Azure Active Directory (Azure AD) 中创建一个具有适当权限的服务帐户。 如果此服务帐户已存在，则重置其密码并等待帐户同步到 Azure AD DS。 重置时会创建 Kerberos 密码哈希，并且同步到 Azure AD DS 可能需要长达 30 分钟的时间。 

该服务帐户需要以下权限：

- 将计算机加入到域，并将计算机主体放到在创建群集期间指定的 OU 中。
- 在群集创建期间指定的 OU 内创建服务主体。

> [!NOTE]
> 由于 Apache Zeppelin 使用域名对管理服务帐户进行身份验证，因此服务帐户必须具有与其 UPN 后缀相同的域名，Apache Zeppelin 才能正常工作。

若要详细了解 OU 以及如何管理它们，请参阅[在 Azure AD DS 托管域上创建 OU](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md)。 

安全 LDAP 适用于 Azure AD DS 托管域。 有关详细信息，请参阅[为 Azure AD DS 托管域配置安全 LDAP](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md)。

## <a name="create-a-domain-joined-hdinsight-cluster"></a>创建已加入域的 HDInsight 群集

下一步是使用 Azure AD DS 和前一部分中创建的服务帐户创建 HDInsight 群集。

将 Azure AD DS 实例和 HDInsight 群集放在同一 Azure 虚拟网络中会更方便。 如果选择将它们放在不同的虚拟网络中，则必须在那些虚拟网络之间建立对等互连，以便 HDInsight VM 能够与域控制器进行通信来使 VM 加入域。 有关详细信息，请参阅[虚拟网络对等互连](../../virtual-network/virtual-network-peering-overview.md)。

创建已加入域的 HDInsight 群集时，必须提供以下参数：

- **域名**：与 Azure AD DS 关联的域名。 例如 contoso.onmicrosoft.com。

- **域用户名**：在前面的部分中创建的 Azure ADDS DC 托管域中的服务帐户。 例如 hdiadmin@contoso.onmicrosoft.com。 此域用户将成为此 HDInsight 群集的管理员。

- **域密码**：服务帐户的密码。

- **组织单位**：要用于 HDInsight 群集的 OU 的可分辨名称。 例如 OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com。 如果此 OU 不存在，则 HDInsight 群集会尝试使用服务帐户拥有的权限创建 OU。 例如，如果服务帐户位于 Azure AD DS 管理员组中，则拥有创建 OU 的权限。 否则，需要先创建 OU，并授予服务帐户对该 OU 的完全控制。 有关详细信息，请参阅[在 Azure AD DS 托管域上创建 OU](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md)。

    > [!IMPORTANT]
    > 在 OU 后包括所有 DC 并以逗号分隔（例如 OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com）。

- **LDAPS URL**：例如 ldaps://contoso.onmicrosoft.com:636。

    > [!IMPORTANT]
    > 输入完整的 URL，包括“ldaps://”和端口号 (:636)。

- **访问用户组**：其用户要同步到群集的安全组。 例如，HiveUsers。 如果想要指定多个用户组，请使用分号“;”分隔。 预配之前，组必须存在于目录中。 有关详细信息，请参阅[在 Azure Active Directory 中创建组并添加成员](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)。 如果该组不存在，则会发生错误：“在 Active Directory 中找不到组 HiveUsers”。

以下屏幕截图显示了 Azure 门户中的配置：

   ![Azure HDInsight 中已加入域的 Active Directory 域服务配置](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png)。


## <a name="next-steps"></a>后续步骤
* 若要配置 Hive 策略和运行 Hive 查询，请参阅[为已加入域的 HDInsight 群集配置 Hive 策略](apache-domain-joined-run-hive.md)。
* 有关使用 SSH 连接到已加入域的 HDInsight 群集，请参阅[在 Linux、Unix 或 OS X 中的 HDInsight 上将 SSH 与基于 Linux 的 Hadoop 配合使用](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined)。

