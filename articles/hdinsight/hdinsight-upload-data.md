---
title: 在 HDInsight 中上传 Hadoop 作业的数据
description: 了解如何使用 Azure 经典 CLI、Azure 存储资源管理器、Azure PowerShell、Hadoop 命令行或 Sqoop 在 HDInsight 中上传和访问 Hadoop 作业的数据。
keywords: etl hadoop, 将数据引入 hadoop, hadoop 加载数据
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.author: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/14/2018
ms.openlocfilehash: 44aaccee436011bd7d27bec87515fde0e898732e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "46985973"
---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>在 HDInsight 中上传 Hadoop 作业的数据

Azure HDInsight 提供一个基于 Azure 存储和 Azure Data Lake Store 的功能完备的 Hadoop 分布式文件系统 (HDFS)。 Azure 存储和 Data Lake Store 设计为一个 HDFS 扩展，为客户提供无缝体验。 它们通过启用 Hadoop 生态系统中的整套组件以直接操作其管理的数据。 Azure 存储和 Data Lake Store 是独立的文件系统，并且已针对数据的存储和计算进行了优化。 有关使用 Azure Blob 存储的益处，请参阅[将 Azure 存储与 HDInsight 配合使用][hdinsight-storage]和[将 Data Lake Store 与 HDInsight 配合使用](hdinsight-hadoop-use-data-lake-store.md)。

## <a name="prerequisites"></a>先决条件

在开始下一步之前，请注意以下要求：

* 一个 Azure HDInsight 群集。 有关说明，请参阅 [Azure HDInsight 入门][hdinsight-get-started]或[创建 HDInsight 群集](hdinsight-hadoop-provision-linux-clusters.md)。
* 学习以下两篇文章：

    - [将 Azure 存储与 HDInsight 配合使用][hdinsight-storage]
    - [将 Data Lake Store 与 HDInsight 配合使用](hdinsight-hadoop-use-data-lake-store.md)

## <a name="upload-data-to-azure-storage"></a>将数据上传到 Azure 存储

### <a name="command-line-utilities"></a>命令行实用程序
Microsoft 提供以下实用工具用于操作 Azure 存储：

| 工具 | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure 经典 CLI][azurecli] |✔ |✔ |✔ |
| [Azure PowerShell][azure-powershell] | | |✔ |
| [AzCopy][azure-azcopy] |✔ | |✔ |
| [Hadoop 命令](#commandline) |✔ |✔ |✔ |

> [!NOTE]
> 尽管 Azure 经典 CLI、Azure PowerShell 和 AzCopy 都可从 Azure 外部使用，但是 Hadoop 命令只能在 HDInsight 群集上使用。 使用该命令只能将数据从本地文件系统载入 Azure 存储。
>
>

#### <a id="xplatcli"></a>Azure 经典 CLI
Azure 经典 CLI 是一个跨平台工具，可用于管理 Azure 服务。 使用以下步骤将数据上传到 Azure 存储：

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

1. [安装和配置适用于 Mac、Linux 和 Windows 的 Azure 经典 CLI](../cli-install-nodejs.md)。
2. 打开命令提示符、bash 或其他 shell，并使用以下方法对 Azure 订阅进行身份验证。

    ```cli
    azure login
    ```

    出现提示时，输入订阅的用户名和密码。
3. 使用以下命令，列出订阅的存储帐户：

    ```cli
    azure storage account list
    ```

4. 选择包含要使用的 Blob 的存储帐户，并使用以下命令检索此帐户的密钥：

    ```cli
    azure storage account keys list <storage-account-name>
    ```

    此命令返回**主**密钥和**辅助**密钥。 复制**主**密钥值，因为后续步骤要用到它。
5. 使用以下命令检索存储帐户中的 Blob 容器列表：

    ```cli
    azure storage container list -a <storage-account-name> -k <primary-key>
    ```

6. 使用以下命令可将文件上传和下载到 Blob：

   * 上传文件：

        ```cli
        azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
        ```

   * 下载文件：

        ```cli
        azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>
        ```
    
> [!NOTE]
> 如果始终使用同一个存储帐户，可以设置以下环境变量，而无需为每条命令指定帐户和密钥：
>
> * **AZURE\_STORAGE\_ACCOUNT**：存储帐户名称
> * **AZURE\_STORAGE\_ACCESS\_KEY**：存储帐户密钥
>
>

#### <a id="powershell"></a>Azure PowerShell
Azure PowerShell 是一个脚本编写环境，可用于在 Azure 中控制和自动执行工作负荷的部署和管理。 有关配置工作站以运行 Azure PowerShell 的信息，请参阅[安装和配置 Azure PowerShell](/powershell/azure/overview)。

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**将本地文件上传到 Azure 存储**

1. 根据[安装和配置 Azure PowerShell](/powershell/azure/overview) 中的说明打开 Azure PowerShell 控制台。
2. 设置以下脚本中前五个变量的值：

    ```powershell
    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<StorageAccountName>"
    $containerName = "<ContainerName>"

    $fileName ="<LocalFileName>"
    $blobName = "<BlobName>"

    # Get the storage account key
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    # Create the storage context object
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

    # Copy the file from local workstation to the Blob container
    Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext
    ```

3. 将脚本粘贴到 Azure PowerShell 控制台中，以运行它来复制文件。

有关创建用来使用 HDInsight 的 PowerShell 脚本示例，请参阅 [HDInsight 工具](https://github.com/blackmist/hdinsight-tools)。

#### <a id="azcopy"></a>AzCopy
AzCopy 是一个命令行工具，用于简化将数据传入和传出 Azure 存储帐户的任务。 可以将它用作独立的工具，也可以将此工具融入到现有应用程序中。 [下载 AzCopy][azure-azcopy-download]。

AzCopy 语法为：

```command
AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]
```

有关详细信息，请参阅 [AzCopy - 上传/下载 Azure Blob 的文件][azure-azcopy]。

AzCopy on Linux 预览版已推出。  请参阅[宣布推出 AzCopy on Linux 预览版](https://blogs.msdn.microsoft.com/windowsazurestorage/2017/05/16/announcing-azcopy-on-linux-preview/)。

#### <a id="commandline"></a>Hadoop 命令行
仅当数据已存在于群集头节点中时，才可以使用 Hadoop 命令行将数据存储到 Azure 存储 Blob。

若要使用 Hadoop 命令，必须先使用以下方法之一连接到头节点：

* **基于 Windows 的 HDInsight**：[使用远程桌面连接](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)
* **基于 Linux 的 HDInsight**：使用 [SSH 或 PuTTY](hdinsight-hadoop-linux-use-ssh-unix.md) 进行连接。

连接之后，可以使用以下语法将文件上传到存储。

```bash
hadoop -copyFromLocal <localFilePath> <storageFilePath>
```

例如： `hadoop fs -copyFromLocal data.txt /example/data/data.txt`

由于 HDInsight 的默认文件系统在 Azure 存储中，/example/data.txt 实际是在 Azure 存储中。 也可以将该文件表示为：

    wasb:///example/data/data.txt

或

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

若要查看可用于文件的其他 Hadoop 命令的列表，请参阅 [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]
> 在 HBase 群集上，写入数据为 256 KB 时会使用默认块大小。 虽然在使用 HBase Api 或 REST API 时可良好运行，但使用 `hadoop` 或 `hdfs dfs` 命令编写大于 ~12 GB 的数据会导致错误。 有关详细信息，请参阅本文的[在 Blob 上编写时的存储异常](#storageexception)部分。
>
>

### <a name="graphical-clients"></a>图形客户端
某些应用程序还提供可配合 Azure 存储空间使用的图形界面。 下表是其中一些应用程序的列表：

| Client | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [用于 HDInsight 的 Microsoft Visual Studio 工具](hadoop/apache-hadoop-visual-studio-tools-get-started.md#explore-linked-resources) |✔ |✔ |✔ |
| [Azure 存储空间资源管理器](http://storageexplorer.com/) |✔ |✔ |✔ |
| [Cloud Storage Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [Azure 资源管理器](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |

#### <a name="visual-studio-tools-for-hdinsight"></a>用于 HDInsight 的 Visual Studio 工具
有关详细信息，请参阅[导航链接的资源](hadoop/apache-hadoop-visual-studio-tools-get-started.md#explore-linked-resources)。

#### <a id="storageexplorer"></a>Azure 存储资源管理器
*Azure 存储资源管理器*是用于在 Blob 中检查和更改数据的工具。 它是免费的开源工具，可从 [http://storageexplorer.com/](http://storageexplorer.com/) 下载。 也可以从此链接获取源代码。

使用该工具之前，必须知道 Azure 存储帐户名和帐户密钥。 有关如何获取此信息的说明，请参阅[创建、管理或删除存储帐户][azure-create-storage-account]中的“如何：查看、复制和重新生成存储访问密钥”部分。

1. 运行 Azure 存储空间资源管理器。 首次运行存储资源管理器时，系统会提示输入“存储帐户名”和“存储帐户密钥”。 如果以前运行过存储资源管理器，请使用“添加”按钮添加一个新的存储帐户名和密钥。

    输入 HDInsight 群集使用的存储帐户的名称和密钥，选择“保存并打开”。

    ![HDI.AzureStorageExplorer][image-azure-storage-explorer]
2. 在界面左侧的容器列表中，单击与 HDInsight 群集关联的容器名称。 默认情况下，这是 HDInsight 群集的名称，但如果在创建群集时输入了特定的名称，则该名称可能不同。
3. 在工具栏中选择上传图标。

    ![突出显示了上传图标的工具栏](./media/hdinsight-upload-data/toolbar.png)
4. 指定要上传的文件，并单击“打开”。 出现提示时，请选择“上传”将文件上传到存储容器的根目录。 如果要将文件上传到特定的路径，请在“目标”字段中输入该路径，然后选择“上传”。

    ![文件上传对话框](./media/hdinsight-upload-data/fileupload.png)

    上传完文件后，可以通过 HDInsight 群集中的作业来使用该文件。

### <a name="mount-azure-storage-as-local-drive"></a>将 Azure 存储装载为本地驱动器
请参阅[将 Azure 存储装载为本地驱动器](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx)。

### <a name="upload-using-services"></a>使用服务上传
#### <a name="azure-data-factory"></a>Azure 数据工厂
Azure 数据工厂服务是完全托管的服务，可将数据存储、数据处理及数据移动服务组合成有效、可缩放且可靠的数据生产管道。

Azure 数据工厂可用于将数据移到 Azure 存储，或创建数据管道来直接使用 HDInsight 功能，例如 Hive 和 Pig。

有关详细信息，请参阅 [Azure 数据工厂文档](https://azure.microsoft.com/documentation/services/data-factory/)。

#### <a id="sqoop"></a>Apache Sqoop
Sqoop 是一种为在 Hadoop 和关系数据库之间传输数据而设计的工具。 可以使用此工具将数据从关系数据库管理系统 (RDBMS)（如 SQL Server、MySQL 或 Oracle）中导入到 Hadoop 分布式文件系统 (HDFS)，在 Hadoop 中使用 MapReduce 或 Hive 转换数据，然后回过来将数据导出到 RDBMS。

有关详细信息，请参阅[将 Sqoop 与 HDInsight 配合使用][hdinsight-use-sqoop]。

### <a name="development-sdks"></a>开发 SDK
还可以使用 Azure SDK 通过以下编程语言来访问 Azure 存储：

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

有关安装 Azure SDK 的详细信息，请参阅 [Azure 下载](https://azure.microsoft.com/downloads/)

### <a name="troubleshooting"></a>故障排除
#### <a id="storageexception"></a>在 Blob 上编写时的存储异常
**症状**：使用 `hadoop` 或 `hdfs dfs` 命令在 HBase 群集上编写大于或等于 ~12 GB 的文件时，可能会遇到以下错误：

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

**原因**：写入 Azure 存储时，HDInsight 群集上的 HBase 的块大小将默认为 256KB。 尽管这对 HBase API 或 REST API 来说可良好运行，但使用 `hadoop` 或 `hdfs dfs` 命令行实用工具时则会导致错误。

**解决方法**：使用 `fs.azure.write.request.size` 指定更大的块大小。 可以通过 `-D` 参数在每次使用时执行此操作。 以下命令是将此参数用于 `hadoop` 命令的示例：

```bash
hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data
```

还可使用 Ambari 以全局方式增加 `fs.azure.write.request.size` 的值。 可使用以下步骤在 Ambari Web UI 中更改该值：

1. 在浏览器中，转到群集的 Ambari Web UI。 该地址为 https://CLUSTERNAME.azurehdinsight.net，其中“CLUSTERNAME”是群集名称。

    出现提示时，输入群集的管理员名称和密码。
2. 在屏幕左侧选择“HDFS”，并选择“配置”选项卡。
3. 在“筛选...”字段中输入 `fs.azure.write.request.size`。 这会在页面中间显示字段和当前值。
4. 将值从 262144 (256KB) 更改为新的值。 例如，4194304 (4MB)。

![通过 Ambari Web UI 更改值的图像](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

有关如何使用 Ambari 的详细信息，请参阅[使用 Ambari Web UI 管理 HDInsight 群集](hdinsight-hadoop-manage-ambari.md)。

## <a name="next-steps"></a>后续步骤
现在，已了解如何将数据导入 HDInsight，请阅读以下文章了解如何执行分析：

* [Azure HDInsight 入门][hdinsight-get-started]
* [以编程方式提交 Hadoop 作业][hdinsight-submit-jobs]
* [将 Hive 与 HDInsight 配合使用][hdinsight-use-hive]
* [将 Pig 与 HDInsight 配合使用][hdinsight-use-pig]

[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]:../storage/common/storage-create-storage-account.md
[azure-azcopy-download]:../storage/common/storage-use-azcopy.md
[azure-azcopy]:../storage/common/storage-use-azcopy.md

[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]:hadoop/submit-apache-hadoop-jobs-programmatically.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[azurecli]: ../cli-install-nodejs.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
