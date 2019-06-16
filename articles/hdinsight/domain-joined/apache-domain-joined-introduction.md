---
title: 使用企业安全性套餐实现 Apache Hadoop 安全性简介
description: 了解企业安全性套餐如何支持企业安全的 4 个支柱。
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: overview
ms.date: 06/12/2019
ms.openlocfilehash: b7228fdf1bb67ff8029412174a883a3a0b123cfc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "67126200"
---
# <a name="what-is-enterprise-security-package-in-azure-hdinsight"></a>什么是 Azure HDInsight 中的企业安全性套餐

以前，Azure HDInsight 仅支持一个用户：本地管理员。这种情况对于应用程序团队或部门而言比较有利。 随着基于 Apache Hadoop 的工作负荷在企业部门中越来越普及，基于 Active Directory 的身份验证、多用户支持和基于角色的访问控制等企业级功能需求也变得日益重要。 

现可创建已加入 Active Directory 域的企业安全性套餐 (ESP) HDInsight 群集。 接下来可配置企业员工的列表，这些员工可通过 Azure Active Directory 进行身份验证并登录到 HDInsight 群集。 企业外部的任何人都无法登录或访问 HDInsight 群集。 

企业管理员可以使用 [Apache Ranger](https://ranger.apache.org/) 配置基于角色的访问控制 (RBAC) 来实现 Apache Hive 安全性。 配置 RBAC 可将数据访问权限限制为仅满足使用需求。 最后，管理员可以审核员工的数据访问权限和对访问控制策略所做的任何更改。 这样，管理员即可实现对其企业资源的高度控制。

Apache Oozie 现已在 ESP 群集上启用。 若要访问 Oozie Web UI，用户应启用[隧道](../hdinsight-linux-ambari-ssh-tunnel.md)。

企业安全性包含四大支柱 - 外围安全性、身份验证、授权和加密。

![企业安全性套餐 HDInsight 群集在企业安全四大支柱中的优势](./media/apache-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

## <a name="perimeter-security"></a>外围安全性
HDInsight 中的外围安全性通过虚拟网络和 Azure VPN 网关服务实现。 企业管理员可在虚拟网络中创建 ESP 群集，并使用网络安全组（防火墙规则）限制对虚拟网络的访问。 只有入站防火墙规则中定义的 IP 地址才能与 HDInsight 群集通信。 此配置可实现外围安全性。

另一层外围安全性通过 VPN 网关服务实现。 网关充当传入 HDInsight 群集的任何请求的第一道防线。 网关接受请求并进行验证，仅在此之后才会允许请求传递给群集中的其他节点。 通过这种方式，网关为群集中的其他名称和数据节点提供外围安全性。

## <a name="authentication"></a>Authentication
企业管理员可在[虚拟网络](https://azure.microsoft.com/services/virtual-network/)中创建 ESP HDInsight 群集。 HDInsight 群集的所有节点将加入企业管理的域。 此过程通过使用 [Azure Active Directory 域服务](../../active-directory-domain-services/overview.md)实现。 

借助这种设置，企业员工可以使用其域凭据登录到群集节点。 他们还可以使用自己的域凭据在其他已批准的终结点（例如 Apache Ambari 视图、ODBC、JDBC、PowerShell 和 REST API）上进行身份验证，以便与群集交互。 管理员拥有完全控制权，可以限制通过这些终结点与群集交互的用户数目。

## <a name="authorization"></a>授权
大多数企业遵循的最佳实践是，确保并非每位员工都有权访问所有企业资源。 同理，管理员可以针对群集资源定义基于角色的访问控制策略。 

例如，管理员可以配置 [Apache Ranger](https://ranger.apache.org/)，为 Hive 设置访问控制策略。 此功能可确保员工最多仅能访问为了成功完成其作业而要访问的数据。 此外，只有管理员能够通过 SSH 访问群集。

## <a name="auditing"></a>审核
若要跟踪对资源的未经授权或非故意的访问，则必须审核对群集资源和数据的所有访问。 这与阻止未经授权的用户访问 HDInsight 群集资源和保护数据具有同等重要性。 

管理员可以查看和报告对 HDInsight 群集资源与数据的所有访问。 管理员还可以查看和报告对在 Apache Ranger 支持的终结点中创建的访问控制策略进行的所有更改。 

ESP HDInsight 群集使用常见的 Apache Ranger UI 来搜索审核日志。 在后端，Ranger 使用 [Apache Solr](http://lucene.apache.org/solr/) 来存储和搜索日志。

## <a name="encryption"></a>加密
保护数据对于满足组织安全性和合规性要求具有重要意义。 除了限制未经授权的员工访问数据外，还应对数据加密。 

HDInsight 群集的两种数据存储（Azure Blob 存储和 Azure Data Lake Storage Gen1/Gen2）都支持在服务器端以透明方式进行静态[数据加密](../../storage/common/storage-service-encryption.md)。 HDInsight 安全群集将与这种服务器端静态数据加密功能无缝协作。

## <a name="next-steps"></a>后续步骤

* [HDInsight 群集规划](apache-domain-joined-architecture.md)
* [配置 ESP HDInsight 群集](apache-domain-joined-configure.md)
* [管理 ESP HDInsight 群集](apache-domain-joined-manage.md)