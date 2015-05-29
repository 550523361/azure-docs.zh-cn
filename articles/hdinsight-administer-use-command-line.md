﻿<properties 
	pageTitle="使用跨平台命令行管理 Hadoop 群集 | Azure" 
	description="了解如何使用跨平台命令行界面，在支持 Node.js 的任何平台（包括 Windows、Mac 和 Linux）上管理 HDIsight 中的 Hadoop 群集。" 
	services="hdinsight" 
	editor="cgronlun" 
	manager="paulettm" 
	authors="mumian" 
	documentationCenter=""/>

<tags 
	ms.service="hdinsight" 
	ms.workload="big-data" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="03/31/2015" 
	wacn.date="" 
	ms.author="jgao"/>

# 使用跨平台命令行界面管理 HDInsight 中的 Hadoop 群集

了解如何使用 Azure 跨平台命令行界面管理 Azure HDInsight 中的 Hadoop 群集。该命令行工具在 Node.js 中实现。可以在支持 Node.js 的任意平台（包括 Windows、Mac 和 Linux）上使用它。 

该命令行工具是开源的。在 GitHub 中管理源代码（网址为 <a href= "https://github.com/WindowsAzure/azure-sdk-tools-xplat">https://github.com/WindowsAzure/azure-sdk-tools-xplat</a>）。 

本文只涉及从 Windows 使用命令行界面。有关如何使用命令行界面的一般指南，请参阅[如何使用针对 Mac 和 Linux 的 Azure 命令行工具][azure-command-line-tools]。有关完整的参考文档，请参阅[针对 Mac 和 Linux 的 Azure 命令行工具][azure-command-line-tool]。


## 先决条件

在开始阅读本文前，你必须具有：

- **Azure 订阅** - Azure 是基于订阅的平台。有关获取订阅的详细信息，请参阅[购买选项][azure-purchase-options]和[免费试用][azure-trial]。

## 安装
可以通过  *Node.js Package Manager (NPM)* 或 Windows 安装程序安装该命令行界面。

**使用 NPM 安装命令行界面**

1.	浏览到 **www.nodejs.org**。
2.	单击"安装"，然后使用默认设置按说明操作。
3.	从工作站打开"命令提示符"（或"Azure 命令提示符"，或"VS2012 的开发人员命令提示符"）。
4.	在命令提示符窗口中运行以下命令：

		npm install -g azure-cli

	> [AZURE.NOTE] 如果收到"未找到 NPM 命令"的错误消息，请验证以下路径位于 **PATH** 环境变量中：<i>C:\Program Files (x86)\nodejs;C:\Users\[username]\AppData\Roaming\npm</i>或 <i>C:\Program Files\nodejs;C:\Users\[username]\AppData\Roaming\npm</i>


5.	运行以下命令以验证安装：

		azure hdinsight -h

	可以在不同级别使用 **-h** 开关以显示帮助信息。例如：
		
		azure -h
		azure hdinsight -h
		azure hdinsight cluster -h
		azure hdinsight cluster create -h

**使用 Windows 安装程序安装命令行界面**

1.	浏览到 **http://www.windowsazure.cn/downloads/**。
2.	向下滚动到"命令行工具"部分，然后单击"跨平台命令行界面"，按 Web 平台安装程序向导的要求操作。

## 下载和导入 Azure 帐户 publishsettings 文件

在使用命令行界面前，你必须配置工作站和 Azure 之间的连接。命令行界面使用你的 Azure 订阅信息连接到你的帐户。可从 Azure 的 publishsettings 文件中获取此信息。然后，可以导入 publishsettings 文件作为永久性本地配置设置，命令行界面会将此设置用于后续操作。你只需导入你的 publishsettings 文件一次。

> [AZURE.NOTE] publishsettings 文件包含敏感信息。建议你删除该文件或采取其他措施来加密包含该文件的用户文件夹。在 Windows 上，修改文件夹属性或使用 BitLocker 驱动器加密。


**下载和导入 publishsettings 文件**

1.	打开命令提示符。
2.	运行以下命令来下载发布设置文件：

		azure account download
 
	![HDI.CLIAccountDownloadImport][image-cli-account-download-import]

	该命令显示下载文件的说明，包括 URL。

3.	打开 Internet Explorer 并浏览到命令行提示符窗口中所列的 URL。
4.	单击"保存"以将文件保存到工作站。
5.	从命令提示符窗口，运行以下命令以导入 publishsettings 文件：

		azure account import <file>

	在上一屏幕快照中，publishsettings 文件已保存到工作站上的 C:\HDInsight 文件夹。


## 设置 HDInsight 群集

[AZURE.INCLUDE [provisioningnote](../includes/hdinsight-provisioning.md)]


HDInsight 使用 Azure Blob 存储容器作为默认文件系统。你需要先拥有 Azure 存储帐户，然后才能创建 HDInsight 群集。 

在导入了 publishsettings 文件后，你可以使用以下命令创建一个存储帐户：

	azure account storage create [options] <StorageAccountName>


> [AZURE.NOTE] 存储帐户必须与 HDInsight 共置于同一数据中心。


有关使用 Azure 门户创建 Azure 存储帐户的信息，请参阅[创建、管理或删除存储帐户][azure-create-storageaccount]。

如果你已有存储帐户但是不知道帐户名称和帐户密钥，可以使用以下命令来检索该信息：

	-- Lists Storage accounts
	azure account storage list
	-- Shows a Storage account
	azure account storage show <StorageAccountName>
	-- Lists the keys for a Storage account
	azure account storage keys list <StorageAccountName>

有关使用 Azure 门户获取信息的详细信息，请参阅[创建、管理或删除存储帐户][azure-create-storageaccount]中的"查看、复制和重新生成存储访问密钥"部分。


如果容器不存在，可使用 **azure hdinsight cluster create** 命令创建它。如果选择预先创建容器，可以使用以下命令：

	azure storage container create --account-name <StorageAccountName> --account-key <StorageAccountKey> [ContainerName]
		
准备好存储帐户和 blob 容器后，你就可以创建群集了： 

	azure hdinsight cluster create --clusterName <ClusterName> --storageAccountName <StorageAccountName> --storageAccountKey <storageAccountKey> --storageContainer <StorageContainer> --nodes <NumberOfNodes> --location <DataCenterLocation> --username <HDInsightClusterUsername> --clusterPassword <HDInsightClusterPassword>

![HDI.CLIClusterCreation][image-cli-clustercreation]

















## 使用配置文件设置 HDInsight 群集
通常，你设置一个 HDInsight 群集，对其运行作业，然后删除该群集以降低成本。在命令行界面上，你可以选择将配置保存到文件，以便在每次设置群集时重用这些配置。  
 
	azure hdinsight cluster config create <file>
	 
	azure hdinsight cluster config set <file> --clusterName <ClusterName> --nodes <NumberOfNodes> --location "<DataCenterLocation>" --storageAccountName "<StorageAccountName>.blob.core.chinacloudapi.cn" --storageAccountKey "<StorageAccountKey>" --storageContainer "<BlobContainerName>" --username "<Username>" --clusterPassword "<UserPassword>"
	 
	azure hdinsight cluster config storage add <file> --storageAccountName "<StorageAccountName>.blob.core.chinacloudapi.cn"
	       --storageAccountKey "<StorageAccountKey>"
	 
	azure hdinsight cluster config metastore set <file> --type "hive" --server "<SQLDatabaseName>.database.chinacloudapi.cn"
	       --database "<HiveDatabaseName>" --user "<Username>" --metastorePassword "<UserPassword>"
	 
	azure hdinsight cluster config metastore set <file> --type "oozie" --server "<SQLDatabaseName>.database.chinacloudapi.cn"
	       --database "<OozieDatabaseName>" --user "<SQLUsername>" --metastorePassword "<SQLPassword>"
	 
	azure hdinsight cluster create --config <file>
		 


![HDI.CLIClusterCreationConfig][image-cli-clustercreation-config]


## 列出并显示群集详细信息
使用以下命令来列出和显示群集详细信息：
	
	azure hdinsight cluster list
	azure hdinsight cluster show <ClusterName>
	
![HDI.CLIListCluster][image-cli-clusterlisting]


## 删除群集
使用以下命令来删除群集：

	azure hdinsight cluster delete <ClusterName>




## 后续步骤
在本文中，你已了解如何执行不同的 HDInsight 群集管理任务。若要了解更多信息，请参阅以下文章：

* [使用 Azure 门户管理 HDInsight][hdinsight-admin-portal]
* [使用 Azure PowerShell 管理 HDInsight][hdinsight-admin-powershell]
* [Azure HDInsight 入门][hdinsight-get-started]
* [如何使用针对 Mac 和 Linux 的 Azure 命令行工具][azure-command-line-tools]
* [针对 Mac 和 Linux 的 Azure 命令行工具][azure-command-line-tool]


[azure-command-line-tools]: /documentation/articles/xplat-cli/
[azure-create-storageaccount]: /documentation/articles/storage-create-storage-account/ 
[azure-purchase-options]: /pricing/purchase-options/
[azure-trial]: /pricing/1rmb-trial/


[hdinsight-admin-portal]: /documentation/articles/hdinsight-administer-use-management-portal/
[hdinsight-admin-powershell]: /documentation/articles/hdinsight-administer-use-powershell/
[hdinsight-get-started]: /documentation/articles/hdinsight-get-started/

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png 
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/HDI.CLIListClusters.png "List and show clusters"

<!---HONumber=56-->