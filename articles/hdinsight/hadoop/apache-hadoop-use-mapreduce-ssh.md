---
title: 将 MapReduce 和 SSH 连接与 HDInsight 中的 Apache Hadoop 配合使用 - Azure
description: 了解如何使用 SSH 通过 HDInsight 上的 Apache Hadoop 运行 MapReduce 作业。
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlunb
editor: cgronlun
tags: azure-portal
ms.assetid: 844678ba-1e1f-4fda-b9ef-34df4035d547
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 04/10/2018
ms.date: 01/14/2019
ms.author: v-yiso
ms.openlocfilehash: 3448a5e89f6930a5bdcb7d0d77b92576e58fc90b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "62129376"
---
# <a name="use-mapreduce-with-apache-hadoop-on-hdinsight-with-ssh"></a>通过 SSH 将 MapReduce 与 HDInsight 上的 Apache Hadoop 配合使用

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

了解如何从安全外壳 (SSH) 将 MapReduce 作业提交到 HDInsight。

> [!NOTE]
> 如果已熟悉如何使用基于 Linux 的 Apache Hadoop 服务器，但刚接触 HDInsight，请参阅[基于 Linux 的 HDInsight 提示](../hdinsight-hadoop-linux-information.md)。

## <a id="prereq"></a>先决条件

* 基于 Linux 的 HDInsight（HDInsight 上的 Hadoop）群集

  > [!IMPORTANT]
  > Linux 是 HDInsight 3.4 或更高版本上使用的唯一操作系统。 有关详细信息，请参阅 [HDInsight 在 Windows 上停用](../hdinsight-component-versioning.md#hdinsight-windows-retirement)。

* SSH 客户端。 有关详细信息，请参阅 [Use SSH with HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md)（对 HDInsight 使用 SSH）

## <a id="ssh"></a>使用 SSH 进行连接

使用 SSH 连接到群集。 例如，以下命令将以 **sshuser** 帐户身份连接到名为 **myhdinsight** 的群集：

```bash
ssh sshuser@myhdinsight-ssh.azurehdinsight.cn
```

**如果使用用于 SSH 身份验证的证书密钥**，则可能需要指定客户端系统上的私钥位置，例如：

```bash
ssh -i ~/mykey.key sshuser@myhdinsight-ssh.azurehdinsight.cn
```

**如果使用用于 SSH 身份验证的密码**，则需要根据提示提供密码。

有关将 SSH 与 HDInsight 配合使用的详细信息，请参阅[将 SSH 与 HDInsight 配合使用](../hdinsight-hadoop-linux-use-ssh-unix.md)。

## <a id="hadoop"></a>使用 Hadoop 命令

1. 连接到 HDInsight 群集后，使用以下命令启动 MapReduce 作业：

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    此命令启动 `hadoop-mapreduce-examples.jar` 文件中包含的 `wordcount` 类。 它使用 `/example/data/gutenberg/davinci.txt` 文档作为输入，并将输出存储在 `/example/data/WordCountOutput` 中。

    > [!NOTE]
    > 有关此 MapReduce 作业和示例数据的详细信息，请参阅[在 Apache Hadoop on HDInsight 中使用 MapReduce](hdinsight-use-mapreduce.md)。

2. 作业在处理时提供详细信息，并在完成时返回类似于以下文本的信息：

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. 作业完成后，使用以下命令列出输出文件：

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    此命令显示两个文件（`_SUCCESS` 和 `part-r-00000`）。 `part-r-00000` 文件包含此作业的输出。

    > [!NOTE]
    > 某些 MapReduce 作业可能会将结果拆分成多个 **part-r-#####** 文件。 如果是这样，请使用 ##### 后缀指示文件的顺序。

4. 若要查看输出，请使用以下命令：

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    此命令会显示一个列表，其内容为 wasb://example/data/gutenberg/davinci.txt 文件中包含的单词以及每个单词出现的次数。 以下文本是文件中所含数据的示例：

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>摘要

如你所见，Hadoop 命令提供简单的方法让你在 HDInsight 群集上运行 MapReduce 作业，并查看作业输出。

## <a id="nextsteps"></a>后续步骤

有关 HDInsight 中的 MapReduce 作业的一般信息：

* [在 HDInsight Hadoop 上使用 MapReduce](hdinsight-use-mapreduce.md)

有关 HDInsight 上的 Hadoop 的其他使用方法的信息：

* [将 Apache Hive 与 Apache Hadoop on HDInsight 配合使用](hdinsight-use-hive.md)
* [将 Apache Pig 与 Apache Hadoop on HDInsight 配合使用](hdinsight-use-pig.md)
