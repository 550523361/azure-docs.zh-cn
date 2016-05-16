<properties
 pageTitle="用于 Excel 和 SOA 的 HPC Pack 群集 | Azure"
 description="开始使用 Azure 中的 HPC Pack 群集运行大规模 Excel 和 SOA 工作负荷"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>

<tags
	ms.service="virtual-machines-windows"
	ms.date="02/19/2016"
	wacn.date=""/>

# 开始使用 Azure 中的 HPC Pack 群集运行 Excel 和 SOA 工作负荷

本文介绍如何使用 Azure 快速入门模板或（可选）Azure PowerShell 部署脚本将 Microsoft HPC Pack 群集部署在 Azure 基础结构服务 (IaaS) 上。你将使用设计为使用 HPC Pack 运行 Microsoft Excel 或面向服务的体系结构 (SOA) 工作负荷的 Azure 库 VM 映像。你可以使用群集从本地客户端计算机运行简单的 Excel HPC 和 SOA 服务。Excel HPC 服务提供 Excel 工作簿卸载和 Excel 用户定义的函数或 UDF。

> [AZURE.NOTE] Azure 具有用于创建和处理资源的两个不同的部署模型：[资源管理器和经典](/documentation/articles/resource-manager-deployment-model)。本文介绍如何使用 Resource Manager 部署模型。Microsoft 建议对大多数新的部署使用该模型，而不是经典部署模型。


下图在较高级别显示了将创建的 HPC Pack 群集。

![具有运行 Excel 工作负荷的节点的 HPC 群集][scenario]

## 先决条件

*   **客户端计算机** - 你将需要基于 Windows 的客户端计算机以运行 Azure PowerShell 群集部署脚本（如果你选择了该部署方法），并将示例 Excel 和 SOA 作业提交到群集。

*   **Azure 订阅** - 如果你没有 Azure 订阅，只需要花费几分钟就能创建一个[免费帐户](/pricing/1rmb-trial/)。

*   **内核配额** - 你可能需要增加内核配额，尤其是在你部署具有多核 VM 大小的多个群集节点时需要。如果你使用的是 Azure 快速入门模板，请注意资源管理器中的内核配额是按 Azure 区域设定的，你可能需要增加特定区域中的配额。请参阅 [Azure 订阅限制、配额和约束](/documentation/articles/azure-subscription-service-limits)。若要增加配额，可免费[建立联机客户支持请求](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)。

*   **Microsoft Office 许可证** - 如果你使用包含 Microsoft Excel 的 Marketplace HPC Pack VM 映像部署计算节点，将在计算节点上安装 Microsoft Excel Professional Plus 2013 的 30 天评估版。评估期结束后，需要提供有效的 Microsoft Office 许可证来激活 Excel，才能继续运行工作负荷。请参阅在本文稍后的 [Excel 激活](#excel-activation)。


## 步骤 1。在 Azure 中设置 HPC Pack 群集

我们将向你介绍两种设置群集的方法：第一种，使用 Azure 快速入门模板和 Azure 门户；第二种，使用 Azure PowerShell 部署脚本。


### 使用快速入门模板
使用 Azure 快速入门模板可在 Azure 门户中快速轻松地部署 HPC Pack 群集。当你在预览门户中打开该模板时，将显示简单的 UI，你可以在其中输入群集的设置。下面是相关步骤。

>[AZURE.TIP]如果需要，你可以使用 [Azure 应用商店模板](https://portal.azure.cn/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn)来专门为 Excel 工作负荷创建类似的群集。步骤与下文中的内容稍有不同。

1.  访问 [GitHub 上的“创建 HPC 群集”模板页](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)。如果需要，查看有关该模板和源代码的信息。

2.  单击“部署到 Azure”以启动使用 Azure 门户中的模板的部署。

    ![将模板部署到 Azure][github]

3.  在门户中，按照以下步骤输入 HPC 群集模板的参数。

    a.在“参数”页上，输入模板参数的值。（单击每个设置旁边的图标可获得帮助信息。） 下面的屏幕中将显示示例值。本示例将在 “hpc.local” 域中创建名为 “hpc01” 的新 HPC Pack 群集，该群集由 1 个头节点和 2 个计算节点组成。将从包括 Microsoft Excel 的 HPC Pack VM 映像创建计算节点。

    ![输入参数][parameters]

    >[AZURE.NOTE]将在 Windows Server 2012 R2 上从 HPC Pack 2012 R2 的[最新应用商店映像](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)自动创建头节点 VM。当前，该映像基于 HPC Pack 2012 R2 Update 3。
    >
    >将从所选计算节点系列的最新映像创建计算节点 VM。为包含 Microsoft Excel Professional Plus 2013 的评估版本的最新 HPC Pack 计算节点映像选择 **ComputeNodeWithExcel** 选项。如果要为常规 SOA 会话或 Excel UDF 卸载部署群集，请选择 **ComputeNode** 选项（不会安装 Excel）。

    b.选择订阅。

    c.为群集创建新资源组，如 *hpc01RG*。

    d.选择资源组的位置，例如“中国北部”。

    e.在“法律条款”页上，查看条款。如果你同意，请单击“创建”。在完成为模板设置值后，单击“创建”。

4.  在部署完成时（通常需要花费大约 30 分钟），从群集头节点导出群集证书文件。在稍后的步骤中，将在客户端计算机上导入此公用证书以为安全 HTTP 绑定提供服务器端身份验证。

    a.从 Azure 门户通过远程桌面连接到头节点。

     ![连接到头节点][connect]

    b.在证书管理器中使用标准过程导出不带私钥的头节点证书（位于 Cert:\\LocalMachine\\My 下）。在此示例中，导出 “CN = hpc01.chinaeast.chinacloudapp.cn”。

    ![导出证书][cert]

### 使用 HPC Pack IaaS 部署脚本

HPC Pack IaaS 部署脚本提供了另一种通用的方法来部署 HPC Pack 群集。它会在经典部署模型中创建一个群集，该模板使用 Azure Resource Manager 部署模型。此外，该脚本与 Azure 全球或 Azure 中国服务中的订阅兼容。

**其他先决条件**

* **Azure PowerShell** - 在客户端计算机上[安装并配置 Azure PowerShell](/documentation/articles/powershell-install-configure)（版本 0.8.10 或更高版本）。

* **HPC Pack IaaS 部署脚本** - 从 [Microsoft 下载中心](https://www.microsoft.com/download/details.aspx?id=44949)下载并解压缩最新版本的脚本。通过运行 `New-HPCIaaSCluster.ps1 -Version` 检查脚本的版本。本文基于版本 4.5.0 或更高版本的脚本。

**创建配置文件**

 HPC Pack IaaS 部署脚本使用描述 HPC 群集基础结构的 XML 配置文件作为输入。若要部署由 1 个头节点和 18 个计算节点（从包含 Microsoft Excel 的计算节点映像创建）组成的群集，请将你环境的值代入下面的示例配置文件。有关配置文件的详细信息，请参阅脚本文件夹中的 Manual.rtf 文件和[使用 HPC Pack IaaS 部署脚本创建 HPC 群集](/documentation/articles/virtual-machines-windows-classic-hpcpack-cluster-powershell-script)。

	<?xml version="1.0" encoding="utf-8"?>
	<IaaSClusterConfig>
	  <Subscription>
	    <SubscriptionName>MySubscription</SubscriptionName>
	    <StorageAccount>hpc01</StorageAccount>
	  </Subscription>
  	  <Location>China North</Location>
	  <VNet>
	    <VNetName>hpc-vnet01</VNetName>
	    <SubnetName>Subnet-1</SubnetName>
	  </VNet>
	  <Domain>
	    <DCOption>NewDC</DCOption>
	    <DomainFQDN>hpc.local</DomainFQDN>
	    <DomainController>
	      <VMName>HPCExcelDC01</VMName>
	      <ServiceName>HPCExcelDC01</ServiceName>
	      <VMSize>Medium</VMSize>
	    </DomainController>
	  </Domain>
	   <Database>
	    <DBOption>LocalDB</DBOption>
	  </Database>
	  <HeadNode>
	    <VMName>HPCExcelHN01</VMName>
	    <ServiceName>HPCExcelHN01</ServiceName>
	    <VMSize>Large</VMSize>
	    <EnableRESTAPI/>
	    <EnableWebPortal/>
	    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
	  </HeadNode>
	  <ComputeNodes>
	    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
	    <ServiceName>HPCExcelCN01</ServiceName>
	    <VMSize>Medium</VMSize>
	    <NodeCount>18</NodeCount>
	    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
	  </ComputeNodes>
	</IaaSClusterConfig>

**有关配置文件的说明**

* 头节点的 **VMName** **必须**与 **ServiceName** 完全相同，否则 SOA 作业将无法运行。

* 请确保指定 **EnableWebPortal**，以便生成头节点证书并将其导出。

* 该文件指定配置后 PowerShell 脚本 PostConfig.ps1 配置头节点的某些设置，如 Azure 存储空间连接字符串、从头节点中删除计算节点角色以及在部署节点时将所有节点联机。下面是示例脚本。

	    # add the HPC Pack powershell cmdlets
	        Add-PSSnapin Microsoft.HPC
	
	    # set the Azure storage connection string for the cluster
	        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'
	
    		# remove the compute node role for head node to make sure the Excel workbook won't run on head node
	        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode
	
	    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
	        $TotalNumOfNodes = 19
	
	        $ErrorActionPreference = 'SilentlyContinue'
	
	    # bring nodes online when they are deployed until all nodes are online
	        while ($true)
	        {
	          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
	          $OnlineNodes = @(Get-HpcNode -State Online)
	          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
	          {
	             break
	          }
	          sleep 60
	        }

**运行脚本**

1.  在客户端计算机上以管理员身份打开 PowerShell 控制台。

2.  将目录更改到脚本文件夹（在此示例中为 E:\\IaaSClusterScript）。

    	cd E:\IaaSClusterScript

3.  运行以下命令以部署 HPC Pack 群集。本示例假定配置文件位于 E:\\HPCDemoConfig.xml。


    	.\New-HpcIaaSCluster.ps1 -ConfigFile E:\HPCDemoConfig.xml -AdminUserName MyAdminName

HPC Pack 部署脚本将运行一段时间。此脚本将做的一件事情是导出并下载群集证书并将其保存到客户端计算机上当前用户的 Documents 文件夹中。此脚本将生成如下消息。在下面的步骤中，你将在相应的证书存储中导入证书。

You have enabled REST API or web portal on HPC Pack head node.Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:

	C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## 步骤 2.卸载 Excel 工作簿并从本地客户端运行 UDF

### Excel 激活

当对生产工作负荷使用 ComputeNodeWithExcel VM 映像时，需要提供有效的 Microsoft Office 许可密钥才能在计算节点上激活 Excel。否则，Excel 评估版将在 30 天内过期，并且运行 Excel 工作簿将不断失败并显示 COMException (0x800AC472)。如果发生这种情况，请登录到头节点并通过 HPC 群集管理器 clusrun 所有 Excel 计算节点上的 `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe`。
    
这可以让 Excel 再次获得 30 天的评估期。最多可以评估 2 次。然后，你需要提供有效的 Office 许可密钥。

安装在此 VM 映像上的 Office Professional Plus 2013 是采用常规批量许可密钥 (GVLK) 的批量许可版本，可以通过密钥管理服务 (KMS)/基于 Active Directory 的激活 (AD-BA) 或多重激活密钥 (MAK) 来激活。若要使用 KMS/AD-BA，请使用现有的 KMS 服务器，或使用 Microsoft Office 2013 批量许可包设置新服务器（可能位在头节点上）。然后，通过 Internet 或电话激活 KMS 主机密钥。然后运行 clusrun `ospp.vbs` 以设置 KMS 服务器和端口，并激活所有 Excel 计算节点上的 Office。若要使用 MAK，请先运行 clusrun `ospp.vbs` 以输入密钥，然后通过 Internet 或电话激活所有 Excel 计算节点。

>[AZURE.NOTE]Office Professsional Plus 2013 的零售产品密钥不适用于此 VM 映像。如果你拥有非 Office Professional Plus 2013 批量许可版 Office 或 Excel 版本的有效密钥和安装媒体，则也可以卸载此批量许可版本，然后安装你拥有的版本。可捕获重新安装的 Excel 计算节点作为自定义 VM 映像，以便在大规模部署中使用。

### 卸载 Excel 工作簿

按照下列步骤卸载要在 Azure 中的 HPC Pack 群集上运行的 Excel 工作簿。为此，必须已在客户端计算机上安装 Excel 2010 或 Excel 2013。

1. 使用步骤 1 中的方法之一通过 Excel 计算节点映像部署 HPC Pack 群集。获取群集证书文件 (.cer) 和群集用户名和密码。

2. 在客户端计算机上导入 Cert:\\CurrentUser\\Root 下的群集证书。

3. 确保已安装 Excel。使用与客户端计算机上的 Excel.exe 在同一文件夹中的以下内容创建 Excel.exe.config 文件。这可确保 HPC Pack 2012 R2 Excel COM 外接程序成功加载。

		<?xml version="1.0"?>
		<configuration>
		    <startup useLegacyV2RuntimeActivationPolicy="true">
		        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
		    </startup>
		</configuration>

4.	为你的计算机（[x64](http://www.microsoft.com/download/details.aspx?id=14632)，[x86](https://www.microsoft.com/download/details.aspx?id=5555)）下载完整的 [HPC Pack 2012 R2 Update 3 安装](http://www.microsoft.com/download/details.aspx?id=49922)并安装 HPC Pack 客户端，或下载并安装 [HPC Pack 2012 R2 Update 3 客户端实用工具](https://www.microsoft.com/download/details.aspx?id=49923)和相应的 Visual C++ 2010 可再发行组件。

5.	在此示例中，我们使用名为 ConvertiblePricing\_Complete.xlsb 的示例 Excel 工作簿，在[此处](https://www.microsoft.com/download/details.aspx?id=2939)可供下载。

6.	将 Excel 工作簿复制到工作文件夹，例如 D:\\Excel\\Run。

7.	打开 Excel 工作簿。在“开发”功能区上，单击“COM 外接程序”并确认 HPC Pack Excel COM 外接程序已成功加载，如下面的屏幕所示。

    ![HPC Pack 的 Excel 外接程序][addin]

8.	通过更改注释行编辑 Excel 中的 VBA 宏 HPCControlMacros，如下面的脚本中所示。替换为你的环境的相应值。

    ![HPC Pack 的 Excel 宏][macro]

	    'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
    		Private Const HPC_ClusterScheduler = "hpc01.chinaeast.chinacloudapp.cn"
	
	    'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
	    Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"
	
	    'HPCExcelClient.Initialize ActiveWorkbook
	    HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles
	
	    'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
	    HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"
	
	    'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
	    HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"

9.	将 Excel 工作簿复制到上载目录（例如 D:\\Excel\\Upload），如 VBA 宏中的 HPC\_DependsFiles 常量所指定。

10.	单击工作表上的“群集”按钮，在 Azure IaaS 群集上运行该工作簿。

### 运行 Excel UDF

若要运行 Excel UDF，请按照前面的步骤 1 - 3 设置客户端计算机。对于 Excel UDF，不需要将 Excel 应用程序安装在计算节点上，因此你可以在步骤 1 中选择普通计算节点映像，而不是带 Excel 的计算节点映像。

>[AZURE.NOTE] 在 Excel 2010 和 Excel 2013 群集连接器对话框中有 34 字符限制。如果完整的群集名称较长（例如 hpcexcelhn01.southeastasia.chinacloudapp.cn），则它会在对话框中容纳不下。解决方法是将计算机范围的变量（例如 “CCP\_IAASHN”）的值设置为长群集名称，并在对话框中输入 “%CCP\_IAASHN%” 作为群集头节点名称。

成功部署群集后，继续使用以下步骤来运行示例内置 Excel UDF。对于自定义 Excel UDF，请参阅这些[资源](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx)来构建 XLL并将它们部署在 IaaS 群集上。

1.	打开一个新的 Excel 工作簿。在“开发”功能区上，单击“外接程序”。然后，在对话框中单击“浏览”，导航到 %CCP\_HOME%Bin\\XLL32 文件夹，并选择示例 ClusterUDF32.xll。如果 ClusterUDF32 未存在于客户端计算机上，则可以从头节点上的 %CCP\_HOME%Bin\\XLL32 文件夹复制它。

    ![选择 UDF][udf]

2.	单击“文件”>“选项”>“高级”。在“公式”下，选中“允许用户定义的 XLL 函数运行计算群集”。然后，单击“选项”，在“群集头节点名称”中输入完整的群集名称 。（如前所述，此输入框限制为 34 个字符，因此较长的群集名称可能容纳不下。你可以在此处使用计算机范围的变量作为长群集名称。）

    ![配置 UDF][options]

3.	单击包含值 =XllGetComputerNameC() 的单元格，然后按 Enter 以在 IaaS 群集上运行 UDF 计算。该函数将只检索运行 UDF 的计算节点的名称。第一次运行时，凭据对话框将提示你输入用于连接到 IaaS 群集的用户名和密码。

    ![运行 UDF][run]

    当有大量单元格要计算时，按 Alt-Shift-Ctrl + F9 可运行所有单元格上的计算。

## 步骤 3.从本地客户端运行 SOA 工作负荷

若要在 HPC Pack IaaS 群集上运行常规 SOA 应用程序，先使用通用计算节点映像（因为你在计算节点上不需要 Excel）通过步骤 1 中的方法之一部署 IaaS 群集。然后，执行以下步骤。

1. 检索群集证书之后，在客户端计算机上的 Cert: \\CurrentUser\\Root 下导入它。

2. 安装 [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) 和 [HPC Pack 2012 R2 Update 3 客户端实用工具](https://www.microsoft.com/download/details.aspx?id=49923)，以便可以开发和运行 SOA 客户端应用程序。

3. 下载 HelloWorldR2 [示例代码](https://www.microsoft.com/download/details.aspx?id=41633)。在 Visual Studio 2010 或 2012 中打开 HelloWorldR2.sln。

4. 先生成 EchoService 项目，然后按照部署到本地群集的相同方式将服务部署到 IaaS 群集。有关详细步骤，请参阅 HelloWordR2 中的 Readme.doc。按如下所述修改并生成 HelloWorldR2 及其他项目，以便生成从本地客户端计算机在 Azure IaaS 群集上运行的 SOA 客户端应用程序。

### 在有 Azure 存储队列的情况下使用 Http 绑定

若要在有 Azure 存储队列的情况下使用 Http 绑定，需要对示例代码进行一些更改。

* 更新群集名称。

		// Before
		const string headnode = "[headnode]";
		// After e.g.
		const string headnode = "hpc01.chinaeast.chinacloudapp.cn";
		or
		const string headnode = "hpc01.chinacloudapp.cn";

* （可选）在 SessionStartInfo 中使用默认 TransportScheme 或显式将其设置为 Http。

    	info.TransportScheme = TransportScheme.Http;

* 对 BrokerClient 使用默认绑定。

		// Before
		using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
		// After
		using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))

    或者，显式使用 basicHttpBinding 进行设置。

		BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
		binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;

* （可选）在 SessionStartInfo 中将 UseAzureQueue 标志设置为 true。如果未设置，则在群集名称具有 Azure 域后缀并且 TransportScheme 为 Http 时，默认情况下它将设置为 true。

    	info.UseAzureQueue = true;

###在没有 Azure 存储队列的情况下使用 Http 绑定

为此，在 SessionStartInfo 中显式将 UseAzureQueue 标志设置为 false。

    	info.UseAzureQueue = false;

### 使用 NetTcp 绑定

若要使用 NetTcp 绑定，配置与连接到本地群集时类似。你需要在头节点 VM 上打开几个终结点。例如，如果你使用 HPC Pack IaaS 部署脚本创建了群集，请执行以下操作，在 Azure 经典门户中设置终结点。


1. 停止 VM。

2. 分别为会话、代理、代理工作线程和数据服务添加 TCP 端口 9090、9087、9091、9094

    ![配置终结点][endpoint]

3. 启动 VM。

除了将头名称变更为 IaaS 群集完整名称外，SOA 客户端应用程序不需要进行任何更改。

## 后续步骤

* 有关使用 HPC Pack 运行 Excel 工作负荷的详细信息，请参阅[这些资源](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx)。

* 有关使用 HPC Pack 部署和管理 SOA 服务的详细信息，请参阅[在 Microsoft HPC Pack 中管理 SOA 服务](https://technet.microsoft.com/zh-cn/library/ff919412.aspx)。

<!--Image references-->
[scenario]: ./media/virtual-machines-windows-excel-cluster-hpcpack/scenario.png
[github]: ./media/virtual-machines-windows-excel-cluster-hpcpack/github.png
[template]: ./media/virtual-machines-windows-excel-cluster-hpcpack/template.png
[parameters]: ./media/virtual-machines-windows-excel-cluster-hpcpack/parameters.png
[create]: ./media/virtual-machines-windows-excel-cluster-hpcpack/create.png
[connect]: ./media/virtual-machines-windows-excel-cluster-hpcpack/connect.png
[cert]: ./media/virtual-machines-windows-excel-cluster-hpcpack/cert.png
[addin]: ./media/virtual-machines-windows-excel-cluster-hpcpack/addin.png
[macro]: ./media/virtual-machines-windows-excel-cluster-hpcpack/macro.png
[options]: ./media/virtual-machines-windows-excel-cluster-hpcpack/options.png
[run]: ./media/virtual-machines-windows-excel-cluster-hpcpack/run.png
[endpoint]: ./media/virtual-machines-windows-excel-cluster-hpcpack/endpoint.png
[udf]: ./media/virtual-machines-windows-excel-cluster-hpcpack/udf.png

<!---HONumber=Mooncake_0509_2016-->