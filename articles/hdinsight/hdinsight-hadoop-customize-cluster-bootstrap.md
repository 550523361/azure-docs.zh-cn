---
title: 使用 Bootstrap 自定义 HDInsight 群集 - Azure
description: 了解如何使用 Bootstrap 自定义 HDInsight 群集。
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: hrasheed
ms.openlocfilehash: 66c3345c1387e5f1535bbe2bc9a8eafdbb7423d2
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2019
ms.locfileid: "58447800"
---
# <a name="customize-hdinsight-clusters-using-bootstrap"></a>使用 Bootstrap 自定义 HDInsight 群集

有时，用户可能需要配置配置文件，包括：

* clusterIdentity.xml
* core-site.xml
* gateway.xml
* hbase-env.xml
* hbase-site.xml
* hdfs-site.xml
* hive-env.xml
* hive-site.xml
* mapred-site
* oozie-site.xml
* oozie-env.xml
* storm-site.xml
* tez-site.xml
* webhcat-site.xml
* yarn-site.xml
* server.properties（kafka-broker 配置）

Bootstrap 的使用方式有三种：

* 使用 Azure PowerShell
* 使用 .NET SDK
* 使用 Azure 资源管理器模板

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

有关在创建时在 HDInsight 群集上安装其他组件的信息，请参阅：

* [使用脚本操作自定义 HDInsight 群集 (Linux)](hdinsight-hadoop-customize-cluster-linux.md)

## <a name="use-azure-powershell"></a>使用 Azure PowerShell
以下 PowerShell 代码将自定义 [Apache Hive](https://hive.apache.org/) 配置：

```powershell
# hive-site.xml configuration
$hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }

$config = New-AzHDInsightClusterConfig `
    | Set-AzHDInsightDefaultStorage `
        -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -StorageAccountKey $defaultStorageAccountKey `
    | Add-AzHDInsightConfigValues `
        -HiveSite $hiveConfigValues 

New-AzHDInsightCluster `
    -ResourceGroupName $existingResourceGroupName `
    -ClusterName $clusterName `
    -Location $location `
    -ClusterSizeInNodes $clusterSizeInNodes `
    -ClusterType Hadoop `
    -OSType Linux `
    -Version "3.6" `
    -HttpCredential $httpCredential `
    -Config $config 
```

可在[附录](#appendix-powershell-sample)中找到完整的有效 PowerShell 脚本。

**验证更改：**

1. 登录到 [Azure 门户](https://portal.azure.com)。
2. 单击左侧菜单中的“HDInsight 群集”。 如果看不到该群集，请先单击“所有服务”。
3. 单击刚使用 PowerShell 脚本创建的群集。
4. 在边栏选项卡的顶部单击“仪表板”以打开 Ambari UI。
5. 在左侧菜单中，单击“Hive”。
6. 在“摘要”中单击“HiveServer2”。
7. 单击“配置”选项卡。
8. 在左侧菜单中，单击“Hive”。
9. 单击“高级”选项卡。
10. 向下滚动，并展开“高级 hive-site”。
11. 在本部分中查找 **hive.metastore.client.socket.timeout**。

下面是有关自定义其他配置文件的更多示例：

```xml
# hdfs-site.xml configuration
$HdfsConfigValues = @{ "dfs.blocksize"="64m" } #default is 128MB in HDI 3.0 and 256MB in HDI 2.1

# core-site.xml configuration
$CoreConfigValues = @{ "ipc.client.connect.max.retries"="60" } #default 50

# mapred-site.xml configuration
$MapRedConfigValues = @{ "mapreduce.task.timeout"="1200000" } #default 600000

# oozie-site.xml configuration
$OozieConfigValues = @{ "oozie.service.coord.normal.default.timeout"="150" }  # default 120
```
有关详细信息，请参阅 Azim Uddin 的标题为[自定义 HDInsight 群集创建](https://blogs.msdn.com/b/bigdatasupport/archive/2014/04/15/customizing-hdinsight-cluster-provisioning-via-powershell-and-net-sdk.aspx)的博客。

## <a name="use-net-sdk"></a>使用 .NET SDK
请参阅[使用 .NET SDK 在 HDInsight 中创建基于 Linux 的群集](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-bootstrap)。

## <a name="use-resource-manager-template"></a>使用 Resource Manager 模板
可在 Resource Manager 模板中使用 Bootstrap：

```json
"configurations": {
    �
    "hive-site": {
        "hive.metastore.client.connect.retry.delay": "5",
        "hive.execution.engine": "mr",
        "hive.security.authorization.manager": "org.apache.hadoop.hive.ql.security.authorization.DefaultHiveAuthorizationProvider"
    }
}
```

![HDInsight Hadoop 自定义群集 Bootstrap Azure 资源管理器模板](./media/hdinsight-hadoop-customize-cluster-bootstrap/hdinsight-customize-cluster-bootstrap-arm.png)

## <a name="see-also"></a>另请参阅
* [在 HDInsight 中创建 Apache Hadoop 群集][hdinsight-provision-cluster]提供了有关如何使用其他自定义选项创建 HDInsight 群集的说明。
* [为 HDInsight 开发脚本操作脚本][hdinsight-write-script]
* [在 HDInsight 群集上安装并使用 Apache Spark][hdinsight-install-spark]
* [在 HDInsight 群集上安装并使用 Apache Giraph](hdinsight-hadoop-giraph-install.md)。

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions-linux.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "群集创建期间的阶段"

## <a name="appendix-powershell-sample"></a>附录：PowerShell 示例
此 PowerShell 脚本将创建一个 HDInsight 群集并自定义 Hive 设置：

```powershell
####################################
# Set these variables
####################################
#region - used for creating Azure service names
$nameToken = "<ENTER AN ALIAS>" 
#endregion

#region - cluster user accounts
$httpUserName = "admin"  #HDInsight cluster username
$httpPassword = "<ENTER A PASSWORD>" #"<Enter a Password>"

$sshUserName = "sshuser" #HDInsight ssh user name
$sshPassword = "<ENTER A PASSWORD>" #"<Enter a Password>"
#endregion

####################################
# Service names and varialbes
####################################
#region - service names
$namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

$resourceGroupName = $namePrefix + "rg"
$hdinsightClusterName = $namePrefix + "hdi"
$defaultStorageAccountName = $namePrefix + "store"
$defaultBlobContainerName = $hdinsightClusterName

$location = "East US 2"
#endregion

# Treat all errors as terminating
$ErrorActionPreference = "Stop"

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
try{Get-AzContext}
catch{Connect-AzAccount}
#endregion

#region - Create an HDInsight cluster
####################################
# Create dependent components
####################################
Write-Host "Creating a resource group ..." -ForegroundColor Green
New-AzResourceGroup `
    -Name  $resourceGroupName `
    -Location $location

Write-Host "Creating the default storage account and default blob container ..."  -ForegroundColor Green
New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName `
    -Location $location `
    -Type Standard_GRS

$defaultStorageAccountKey = (Get-AzStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
$defaultStorageContext = New-AzStorageContext `
                                -StorageAccountName $defaultStorageAccountName `
                                -StorageAccountKey $defaultStorageAccountKey
New-AzStorageContainer `
    -Name $defaultBlobContainerName `
    -Context $defaultStorageContext #use the cluster name as the container name

####################################
# Create a configuration object
####################################
$hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }

$config = New-AzHDInsightClusterConfig `
    | Set-AzHDInsightDefaultStorage `
        -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -StorageAccountKey $defaultStorageAccountKey `
    | Add-AzHDInsightConfigValues `
        -HiveSite $hiveConfigValues 

####################################
# Create an HDInsight cluster
####################################
$httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

$sshPW = ConvertTo-SecureString -String $sshPassword -AsPlainText -Force
$sshCredential = New-Object System.Management.Automation.PSCredential($sshUserName,$sshPW)

New-AzHDInsightCluster `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -Location $location `
    -ClusterSizeInNodes 1 `
    -ClusterType Hadoop `
    -OSType Linux `
    -Version "3.6" `
    -HttpCredential $httpCredential `
    -SshCredential $sshCredential `
    -Config $config

####################################
# Verify the cluster
####################################
Get-AzHDInsightCluster -ClusterName $hdinsightClusterName

#endregion
```
