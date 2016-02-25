<properties
	pageTitle="使用虚拟网络扩展 HDInsight | Microsoft Azure"  
	description="了解如何使用 Azure 虚拟网络将 HDInsight 连接到其他云资源或者你数据中心内的资源"
	services="hdinsight"
	documentationCenter=""
	authors="Blackmist"
	manager="paulettm"
	editor="cgronlun"/>

<tags
	ms.service="hdinsight"
	ms.date="01/13/2015"
	wacn.date=""/>


#使用 Azure 虚拟网络扩展 HDInsight 功能

Azure 虚拟网络可让你扩展 Hadoop 解决方案以合并本地资源（例如 SQL Server），或者在云中资源之间创建安全的专用网络。

> [AZURE.NOTE] HDInsight 不支持基于地缘的 Azure 虚拟网络。在使用 HDInsight 时，你必须使用基于位置的虚拟网络。


##<a id="whatis"></a>Azure 虚拟网络是什么？

[Azure 虚拟网络](/documentation/services/networking/)允许你创建包含需要用于解决方案的资源的安全永久性网络。通过虚拟网络，你可以：

* 在专用网络（仅限云）中将云资源连接在一起。

	![仅限云配置示意图](./media/hdinsight-extend-hadoop-virtual-network/cloud-only.png)

	使用虚拟网络链接 Azure 服务与 HDInsight 可实现以下方案：

	* 从 Azure 虚拟机中运行的 Azure 网站或服务**调用 HDInsight 服务或作业**。

	* 在 HDInsight 与 SQL 数据库或 SQL Server 或其他运行于虚拟机的数据存储解决方案之间**直接传输数据**。

	* **组合多个 HDInsight 服务器**以构成单个解决方案。例如，使用 HDInsight Storm 服务器来使用传入数据，然后将经过处理的数据存储到 HDInsight HBase 服务器。原始数据也可以存储到 HDInsight Hadoop 服务器供将来使用 MapReduce 进行分析。

* 使用虚拟专用网络 (VPN) 将云资源连接到本地数据中心网络（站点到站点，或点到站点）。

	利用站点到站点配置，你可以使用硬件 VPN 或路由和远程访问服务将多个资源从数据中心连接到 Azure 虚拟网络。

	![站点到站点配置示意图](./media/hdinsight-extend-hadoop-virtual-network/site-to-site.png)

	利用点到站点配置，你可以使用软件 VPN 将特定资源连接到 Azure 虚拟网络。

	![点到站点配置示意图](./media/hdinsight-extend-hadoop-virtual-network/point-to-site.png)

	使用虚拟网络链接云和数据中心可让类似方案在仅限云的配置上实现。但是，如果不想受限于使用云中的资源，你也可以使用数据中心内的资源。

	* 在 HDInsight 与数据中心之间**直接传输数据**。例如，使用 Sqoop 在 SQL Server 往返传输数据，或读取业务线 (LOB) 应用程序生成的数据。

	* 从 LOB 应用程序**调用 HDInsight 服务或作业**。例如，使用 HBase Java API 来存储和检索 HDInsight HBase 群集的数据。

有关虚拟网络特性、优势和功能的详细信息，请参阅 [Azure 虚拟网络概述](/documentation/articles/virtual-networks-overview)。

> [AZURE.NOTE] 你必须创建 Azure 虚拟网络才能设置一个 HDInsight 群集。有关详细信息，请参阅[虚拟网络配置任务](/documentation/services/networking/)。

## 虚拟网络要求

> [AZURE.IMPORTANT] 在虚拟网络上创建 HDInsight 群集需要本部分中所述的特定虚拟网络配置。

* Azure HDInsight 仅支持基于位置的虚拟网络，目前无法处理基于地缘的虚拟网络。 

* 强烈建议针对每个 HDInsight 群集创建一个子网。

* 明确限制与 Internet 相互访问的 Azure 虚拟网络不支持 HDInsight。例如，使用网络安全组或 ExpressRoute 阻止 Internet 流量进入虚拟网络中的资源。HDInsight 服务是一个托管的服务，需要在预配期间和运行时访问 Internet，以便 Azure 能够监视群集的运行状况、启动群集资源故障转移，以及执行其他自动化管理任务。

    如果想要在阻止 Internet 流量的虚拟网络上使用 HDInsight，可以执行以下步骤：

    1. 在虚拟网络中创建新的子网。默认情况下，新子网能够与 Internet 通信。这样就可以在此子网上安装 HDInsight。由于新子网与受保护子网位于同一虚拟网络中，因此它也可以与受保护子网中安装的资源通信。
    
    2. 可以使用以下 PowerShell 语句来确认是否没有将特定的网络安全组或路由表附加到子网。将 __VIRTUALNETWORKNAME__ 替换你的虚拟网络名称，将 __SUBNET__ 替换为子网的名称。
        
            $vnet = Get-AzureVirtualNetwork -Name VIRTUALNETWORKNAME
            $vnet.Subnets | Where-Object Name -eq "SUBNET"
            
        在结果中，请注意 __NetworkSecurityGroup__ 和 __RouteTable__ 均为 `null`。
    
    2. 创建 HDInsight 群集。为群集配置虚拟网络设置时，请选择步骤 1 中创建的子网。

    > [AZURE.NOTE] 上述步骤假设未将通信限制为_虚拟网络 IP 地址范围中_的 IP 地址。如果已限制，则可能需要修改这些限制，以允许与新子网进行通信。

    有关网络安全组的详细信息，请参阅[网络安全组概述](/documentation/articles/virtual-networks-nsg)。有关在 Azure 虚拟网络中控制路由的详细信息，请参阅[用户定义的路由和 IP 转发](/documentation/articles/virtual-networks-udr-overview)。

有关如何在虚拟网络中设置 HDInsight 群集的详细信息，请参阅[在 HDInsight 中设置 Hadoop 群集](/documentation/articles/hdinsight-provision-clusters-v1)。

##<a id="tasks"></a>任务和信息

本部分包含常见任务信息和搭配使用 HDInsight 与虚拟网络时可能需要的信息。

###确定 FQDN

系统将为 HDInsight 群集分配虚拟网络接口的特定完全限定域名 (FQDN)。在从虚拟网络上的其他资源连接到群集时，应该使用这个地址。若要确定 FQDN，请使用以下 URL 来查询 Ambari 管理服务:

	https://<clustername>.azurehdinsight.cn/ambari/api/v1/clusters/<clustername>.azurehdinsight.cn/services/<servicename>/components/<componentname>

> [AZURE.NOTE] 有关如何将 Ambari 与 HDInsight 配合使用的详细信息，请参阅[使用 Ambari API 监视 HDInsight 中的 Hadoop 群集](/documentation/articles/hdinsight-monitor-use-ambari-api)。

必须指定群集名称和群集上运行的服务和组件，例如 YARN 资源管理器。

> [AZURE.NOTE] 返回的数据是一个 JavaScript 对象表示法 (JSON) 文档，其中包含有关组件的大量信息。若只要提取 FQDN，你应该使用 JSON 分析器来检索 `host_components[0].HostRoles.host_name` 值。

例如，若要从 HDInsight Hadoop 群集返回 FQDN，可以使用以下其中一种方法检索 YARN 资源管理器的数据：

* [Azure PowerShell](/documentation/articles/powershell-install-configure)

		$ClusterDnsName = <clustername>
		$Username = <cluster admin username>
		$Password = <cluster admin password>
		$DnsSuffix = ".azurehdinsight.cn"
		$ClusterFQDN = $ClusterDnsName + $DnsSuffix

		$webclient = new-object System.Net.WebClient
		$webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

		$Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/yarn/		components/resourcemanager"
		$Response = $webclient.DownloadString($Url)
		$JsonObject = $Response | ConvertFrom-Json
		$FQDN = $JsonObject.host_components[0].HostRoles.host_name
		Write-host $FQDN

* [cURL](http://curl.haxx.se/) 和 [jq](http://stedolan.github.io/jq/)

		curl -G -u <username>:<password> https://<clustername>.azurehdinsight.cn/ambari/api/v1/clusters/<clustername>.azurehdinsight.cn/services/yarn/components/resourcemanager | jq .host_components[0].HostRoles.host_name

###连接到 HBase

若要使用 Java API 远程连接到 HBase，必须确定 HBase 群集的 Zookeeper 仲裁地址，并在应用程序中加以指定。

若要获取 Zookeeper 仲裁地址，请使用以下其中一种方法来查询 Ambari 管理服务：

* [Azure PowerShell](/documentation/articles/powershell-install-configure)

		$ClusterDnsName = <clustername>
		$Username = <cluster admin username>
		$Password = <cluster admin password>
		$DnsSuffix = ".azurehdinsight.cn"
		$ClusterFQDN = $ClusterDnsName + $DnsSuffix

		$webclient = new-object System.Net.WebClient
		$webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

		$Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
        $Response = $webclient.DownloadString($Url)
        $JsonObject = $Response | ConvertFrom-Json
        Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'

* [cURL](http://curl.haxx.se/) 和 [jq](http://stedolan.github.io/jq/)

		curl -G -u <username>:<password> "https://<clustername>.azurehdinsight.cn/ambari/api/v1/clusters/<clustername>.azurehdinsight.cn/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum" | jq .items[0].properties[]

> [AZURE.NOTE] 有关如何将 Ambari 与 HDInsight 配合使用的详细信息，请参阅[使用 Ambari API 监视 HDInsight 中的 Hadoop 群集](/documentation/articles/hdinsight-monitor-use-ambari-api)。

获取仲裁信息后，请将其用于客户端应用程序中。

例如，对于使用 HBase API 的 Java 应用程序，你可以将 **hbase-site.xml** 文件添加到项目，并在此文件中指定仲裁信息，如下所示：

```
<configuration>
  <property>
    <name>hbase.cluster.distributed</name>
    <value>true</value>
  </property>
  <property>
    <name>hbase.zookeeper.quorum</name>
    <value>zookeeper0.address,zookeeper1.address,zookeeper2.address</value>
  </property>
  <property>
    <name>hbase.zookeeper.property.clientPort</name>
    <value>2181</value>
  </property>
</configuration>
```

###验证网络连接

某些服务，例如 SQL Server，可能会限制传入的网络连接。这会使得 HDInsight 无法成功使用这些服务。

如果你在从 HDInsight 访问服务时遇到问题，请参阅相关服务文档，以确保启用网络访问功能。你也可以通过在相同虚拟网络上创建 Azure 虚拟机来验证网络访问功能，并使用客户端实用工具来验证虚拟机是否可以通过虚拟网络连接服务。

##<a id="nextsteps"></a>后续步骤

以下示例演示了如何对 Azure 虚拟网络使用 HDInsight：

* [使用 HDInsight 中的 Storm 和 HBase 分析传感器数据](/documentation/articles/hdinsight-storm-sensor-data-analysis) - 演示如何在虚拟网络中配置 Storm 和 HBase 群集，以及如何从 Storm 将数据远程写入到 HBase。

* [在 HDInsight 中设置 Hadoop 群集](/documentation/articles/hdinsight-provision-clusters-v1) - 提供有关设置 Hadoop 群集的信息，包括有关使用 Azure 虚拟网络的信息。

* [将 Sqoop 与 HDinsight 中的 Hadoop 配合使用](/documentation/articles/hdinsight-use-sqoop) - 提供有关使用 Sqoop 通过虚拟网络传输 SQL Server 数据的信息。

若要了解有关 Azure 虚拟网络的详细信息，请参阅 [Azure 虚拟网络概述](/documentation/articles/virtual-networks-overview)。

<!---HONumber=Mooncake_0215_2016-->