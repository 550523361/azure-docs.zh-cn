---
title: 排查 SSIS Integration Runtime 中的包执行问题
description: 本文提供有关排查 SSIS Integration Runtime 中的 SSIS 包执行问题的指导
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: wenjiefu
author: wenjiefu
ms.reviewer: sawinark
manager: shwang
ms.custom: seo-lt-2019
ms.date: 04/15/2019
ms.openlocfilehash: 8c85a652cde840336c51e1a5b5459f9dc591e0be
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414686"
---
# <a name="troubleshoot-package-execution-in-the-ssis-integration-runtime"></a>排查 SSIS Integration Runtime 中的包执行问题

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

本文描述了在执行 SSIS Integration Runtime 中的 SQL Server Integration Services (SSIS) 包时可能遇到的最常见错误， 并描述了潜在的原因，以及解决这些错误的操作。

## <a name="where-to-find-logs-for-troubleshooting"></a>在何处可以找到用于故障排除的日志

使用 Azure 数据工厂门户检查 SSIS 包执行活动的输出。 输出包括执行结果、错误消息和操作 ID。 有关详细信息，请参阅[监视管道](how-to-invoke-ssis-package-ssis-activity.md#monitor-the-pipeline)。

使用 SSIS 目录 (SSISDB) 检查执行活动的详细日志。 有关详细信息，请参阅[监视正在运行的包和其他操作](https://docs.microsoft.com/sql/integration-services/performance/monitor-running-packages-and-other-operations?view=sql-server-2017)。

## <a name="common-errors-causes-and-solutions"></a>常见错误、原因和解决方案

### <a name="error-message-connection-timeout-expired-or-the-service-has-encountered-an-error-processing-your-request-please-try-again"></a>错误消息："连接超时已过期"或"服务在处理您的请求时遇到错误。 请重试。”

下面是可能的原因和建议的操作：
* 数据源或目标过载。 检查数据源或目标的负载，并查看它是否有足够的容量。 例如，如果使用的是 Azure SQL 数据库，而该数据库可能会超时，请考虑纵向扩展。
* SSIS Integration Runtime 与数据源或目标之间的网络不稳定，当连接跨区域或者是在本地与 Azure 之间建立的时尤其如此。 执行以下步骤，在 SSIS 包中应用重试模式：
  * 确保 SSIS 包在失败时可以重新运行，且不产生负面影响（例如数据丢失或数据重复）。
  * 在 **"常规**"![选项卡上配置执行**SSIS 包**活动的**重试**和**重试间隔**。](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
  * 对于 ADO.NET 和 OLE DB 源或目标组件，请在 SSIS 包或 SSIS 活动的连接管理器中设置 **ConnectRetryCount** 和 **ConnectRetryInterval**。

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server-the-server-was-not-found-or-was-not-accessible"></a>错误消息："ADO NET 源未能获取连接"..." 同时显示“在与 SQL Server 建立连接时出现网络相关或特定于实例的错误。 找不到或无法访问服务器。”

此问题通常意味着无法从 SSIS 集成运行时访问数据源或目标。 原因可能各不相同。 请尝试以下操作：
* 确保正确传递数据源或目标的名称/IP。
* 确保已正确设置防火墙。
* 确保在数据源或目标为本地源或目标的情况下正确配置虚拟网络：
  * 可以通过在同一虚拟网络中预配 Azure VM，来验证问题是否与虚拟网络配置有关。 然后检查是否可以从 Azure VM 访问数据源或目标
  * 您可以在[加入 Azure-SSIS 集成运行时到虚拟网络](join-azure-ssis-integration-runtime-virtual-network.md)中查找有关将虚拟网络与 SSIS 集成运行时使用的详细信息。

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-could-not-create-a-managed-connection-manager"></a>错误消息："ADO NET 源未能获取连接"..." 并显示“无法创建托管的连接管理器。”

潜在的原因是在包中使用的 ADO.NET 提供程序未安装在 SSIS Integration Runtime 中。 可以使用自定义安装程序来安装该提供程序。 在[自定义 Azure SSIS Integration Runtime 的安装](how-to-configure-azure-ssis-ir-custom-setup.md)中可以找到有关自定义安装的更多详细信息

### <a name="error-message-the-connection--is-not-found"></a>错误消息："连接'...'找不到"

旧版 SQL Server Management Studio (SSMS) 中的某个已知问题可能导致此错误。 如果此包包含的自定义组件（例如 SSIS Azure Feature Pack 或合作伙伴组件）未安装在使用 SSMS 来执行部署的计算机上，则 SSMS 会删除该组件，导致此错误。 请将 [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) 升级到已解决了此问题的最新版本。

### <a name="error-messagessis-executor-exit-code--1073741819"></a>错误消息：“SSIS 执行程序退出代码: -1073741819。”

* 可能的原因和建议的操作：
  * 出现此错误的原因可能是，以多线程并行执行多个 Excel 源或目标时，Excel 源和目标遭到限制。 可通过以下方式解决此限制：将 Excel 组件更改为按顺序执行，或将其分隔到不同的包中，并在将 ExecuteOutOfProcess 属性设置为 True 的情况下，通过“执行包任务”来触发执行。

### <a name="error-message-there-is-not-enough-space-on-the-disk"></a>错误消息："磁盘上没有足够的空间"

此错误表示 SSIS Integration Runtime 节点中的本地磁盘空间已用尽。 请检查你的包或自定义安装是否占用了大量的磁盘空间：
* 如果包占用了磁盘，包执行完成后会释放磁盘空间。
* 如果自定义安装占用了磁盘，则你需要停止 SSIS Integration Runtime，修改脚本，然后再次启动 Integration Runtime。 为自定义安装指定的整个 Azure Blob 容器将复制到 SSIS Integration Runtime 节点，因此，请检查该容器中是否包含任何不必要的内容。

### <a name="error-message-failed-to-retrieve-resource-from-master-microsoftsqlserverintegrationservicesscalescaleoutcontractcommonmasterresponsefailedexception-code300004-descriptionload-file--failed"></a>错误消息："无法从主服务器检索资源。 微软.SqlServer.集成服务.Scale.Scaleout合同.通用.主响应失败例外：代码：300004。 说明: 加载文件 "***" 失败。”

* 可能的原因和建议的操作：
  * 如果 SSIS 活动正在从文件系统（包文件或项目文件）执行包，而使用你在 SSIS 活动中提供的包访问凭据无法访问项目、包或配置文件，则会出现此错误
    * 如果使用 Azure 文件：
      * 文件路径应从\\\\\<存储帐户名称\>.file.core.windows.net\\\<文件共享路径开始\>
      * 域应是“Azure”
      * 用户名应是 \<存储帐户名称\>
      * 密码应是 \<存储访问密钥\>
    * 如果使用本地文件，请检查是否正确配置了 VNet、包访问凭据和权限，以便 Azure-SSIS 集成运行时可以访问你的本地文件共享

### <a name="error-message-the-file-name--specified-in-the-connection-was-not-valid"></a>错误消息："文件名 '...'在连接中指定无效"

* 可能的原因和建议的操作：
  * 指定的文件名无效
  * 确保在连接管理器中使用 FQDN（完全限定的域名）而不是短时间

### <a name="error-message-cannot-open-file-"></a>错误消息："无法打开文件'...'

当包执行在 SSIS Integration Runtime 中的本地磁盘内找不到文件时，将发生此错误。 请尝试以下操作：
* 不要在 SSIS Integration Runtime 中执行的包内使用绝对路径。 改用当前执行工作目录 (.) 或临时文件夹 (%TEMP%)。
* 如果需要在 SSIS Integration Runtime 节点上保留某些文件，请根据[自定义安装](how-to-configure-azure-ssis-ir-custom-setup.md)中所述准备文件。 执行完成后，系统会清理工作目录中的所有文件。
* 改用 Azure 文件来存储 SSIS Integration Runtime 节点中的文件。 有关详细信息，请参阅[使用 Azure文件共享](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-files-file-shares?view=sql-server-2017#use-azure-file-shares)。

### <a name="error-message-the-database-ssisdb-has-reached-its-size-quota"></a>错误消息："数据库'SSISDB'已达到其大小配额"

可能的原因是，在 Azure SQL 数据库中创建的 SSISDB 数据库或者在创建 SSIS 集成运行时时的托管实例已达到其配额。 请尝试以下操作：
* 考虑增加数据库的 DTU。 可以在 [Azure SQL 数据库服务器的 SQL 数据库资源限制](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server)中找到详细信息。
* 检查包是否会生成许多日志。 如果是，则可配置一项弹性作业来清理这些日志。 有关详细信息，请参阅[使用 Azure 弹性数据库作业清理 SSISDB 日志](how-to-clean-up-ssisdb-logs-with-elastic-jobs.md)。

### <a name="error-message-the-request-limit-for-the-database-is--and-has-been-reached"></a>错误消息："数据库的请求限制是 ...并已达到。

如果在 SSIS Integration Runtime 中同时运行许多的包，可能会发生此错误，因为 SSISDB 已达到请求限制。 请考虑增加 SSISDB 的 DTC 来解决此问题。 可以在 [Azure SQL 数据库服务器的 SQL 数据库资源限制](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server)中找到详细信息。

### <a name="error-message-ssis-operation-failed-with-unexpected-operation-status-"></a>错误消息："SSIS 操作失败，操作状态意外：..."

该错误主要是由某个暂时性问题导致的，因此请尝试重新运行包执行。 执行以下步骤，在 SSIS 包中应用重试模式：

* 确保 SSIS 包在失败时可以重新运行，且不产生负面影响（例如数据丢失或数据重复）。
* 在 **"常规**"![选项卡上配置执行**SSIS 包**活动的**重试**和**重试间隔**。](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
* 对于 ADO.NET 和 OLE DB 源或目标组件，请在 SSIS 包或 SSIS 活动的连接管理器中设置 **ConnectRetryCount** 和 **ConnectRetryInterval**。

### <a name="error-message-there-is-no-active-worker"></a>错误消息："没有活动工作人员。

此错误通常表示 SSIS Integration Runtime 处于不正常状态。 请在 Azure 门户中检查状态和详细错误。 有关详细信息，请参阅 [Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime)。

### <a name="error-message-your-integration-runtime-cannot-be-upgraded-and-will-eventually-stop-working-since-we-cannot-access-the-azure-blob-container-you-provided-for-custom-setup"></a>错误消息："无法升级集成运行时，并最终停止工作，因为我们无法访问为自定义设置提供的 Azure Blob 容器。

当 SSIS Integration Runtime 无法访问针对自定义安装配置的存储时，将发生此错误。 请检查提供的共享访问签名 (SAS) URI 是否有效且未过期。

### <a name="error-message-microsoft-ole-db-provider-for-analysis-services-hresult-0x80004005-description-com-error-com-error-mscorlib-exception-has-been-thrown-by-the-target-of-an-invocation"></a>错误消息："微软 OLE DB 分析服务提供商。 'Hresult： 0x80004005 描述：' COM 错误： COM 错误： mscorlib;调用的目标已引发异常"

一种潜在原因是为 Azure Analysis Services 身份验证配置了已启用 Azure 多重身份验证的用户名或密码。 SSIS Integration Runtime 不支持这种身份验证。 尝试使用服务主体进行 Azure Analysis Services 身份验证：

1. 请根据[使用服务主体进行自动化](https://docs.microsoft.com/azure/analysis-services/analysis-services-service-principal)中所述准备服务主体。
2. 在连接管理器中，配置“使用特定的用户名和密码”：将“AppID”设为用户名，将“clientSecret”设为密码。************

### <a name="error-message-adonet-source-has-failed-to-acquire-the-connection-guid-with-the-following-error-message-login-failed-for-user-nt-authorityanonymous-logon-when-using-a-managed-identity"></a>错误消息："ADONET 源未能获取连接 [GUID]，并显示以下错误消息：用户使用托管标识时登录失败"NT AUTHORITY_ANONYMOUS LOGON"

确保当参数*ConnectUsing托管标识***为 true**时，不要将连接管理器的身份验证方法配置为**活动目录密码身份验证**。 可将其配置为“SQL 身份验证”，设置了 *ConnectUsingManagedIdentity* 时会忽略此配置。****

### <a name="error-message-0xc020801f-at--odata-source--cannot-acquire-a-managed-connection-from-the-run-time-connection-manager"></a>错误消息："0xC020801F 在 ...， OData 源 [...] ： 无法从运行时连接管理器获取托管连接"

一个可能的原因是，未在 SSIS 集成运行时中启用 OData 源所需的传输层安全性 (TLS)。 可以使用自定义设置在 SSIS 集成运行时中启用 TLS。 有关更多详细信息，请参见[无法通过 SSIS 连接 Project Online Odata](https://docs.microsoft.com/office365/troubleshoot/cant-connect-project-online-odata-from-ssis) 和[自定义 Azure-SSIS 集成运行时的设置](how-to-configure-azure-ssis-ir-custom-setup.md)。

### <a name="error-message-request-staging-task-with-operation-guid--fail-since-error-failed-to-dispatch-staging-operation-with-error-message-microsoftsqlserverintegrationservicesaisagentcoreaisagentexception-failed-to-load-data-proxy"></a>错误消息："请求具有操作 guid 的暂存任务...由于错误而失败：无法使用错误消息调度暂存操作：Microsoft.SqlServer.集成服务.AisAgentCore.AisAgentException：无法加载数据代理。

确保 Azure-SSIS 集成运行时配置了自托管集成运行时。 更多细节可在[ADF 中配置自托管 IR 作为 Azure-SSIS IR 的代理](self-hosted-integration-runtime-proxy-ssis.md)中找到。

### <a name="error-message-staging-task-status-failed-staging-task-error-errorcode-2010-errormessage-the-self-hosted-integration-runtime--is-offline"></a>错误消息："暂存任务状态：失败。 暂存任务错误：错误代码：2010，错误消息：自托管集成运行时...处于脱机状态"

请确保已安装并启动了自承载集成运行时。 有关更多详细信息，请参阅[创建和配置自承载集成运行时](create-self-hosted-integration-runtime.md)

### <a name="error-message-staging-task-error-errorcode-2906-errormessage-package-execution-failed-output-operationerrormessages-error-the-requested-ole-db-provider--is-not-registered-if-the-64-bit-driver-is-not-installed-run-the-package-in-32-bit-mode"></a>错误消息："暂存任务错误：错误代码：2906，错误消息：包执行失败.，输出：\"操作错误消息"："错误：请求的 OLE 数据库提供程序...未注册。 如果未安装 64 位驱动程序，请以 32 位模式运行包..."

确保包中的 OLE DB 连接器使用的相应提供程序正确安装在自托管集成运行时计算机上。 更多详细信息可在[ADF 中配置自托管 IR 作为 Azure-SSIS IR 的代理中找到](self-hosted-integration-runtime-proxy-ssis.md#prepare-the-self-hosted-ir)

### <a name="error-message-staging-task-error-errorcode-2906-errormessage-package-execution-failed-output-operationerrormessages-error-systemiofileloadexception-could-not-load-file-or-assembly-microsoftwindowsazurestorage-version-cultureneutral-publickeytoken31bf3856ad364e35-or-one-of-its-dependencies-the-located-assemblys-manifest-definition-does-not-match-the-assembly-reference"></a>错误消息："暂存任务错误：错误代码：2906，错误消息：包执行失败.，输出：{"操作错误消息"："错误：系统.IO.fileLoadException：无法加载文件或程序集'Microsoft.WindowsAzure.存储，版本=...，区域性=中性，PublicKeyToken_31bf385ad364e35'或其依赖项之一。 找到的程序集清单定义与程序集引用不匹配...”

一个可能的原因是自承载集成运行时未正确安装或升级。 建议下载并重新安装最新的自承载集成运行时。 有关更多详细信息，请参阅[创建和配置自承载集成运行时](create-self-hosted-integration-runtime.md#installation-best-practices)

### <a name="error-message-a-connection-is-required-when-requesting-metadata-if-you-are-working-offline-uncheck-work-offline-on-the-ssis-menu-to-enable-the-connection"></a>错误消息："请求元数据时需要连接。 如果正在脱机工作，请取消选中‘SSIS’菜单上的‘脱机工作’，以启用连接”

* 可能的原因和建议的操作：
  * 如果执行日志中还出现警告消息"组件不支持使用连接管理器，连接管理器具有连接ByProxy值设置为 true"，则这意味着连接管理器在尚未支持"ConnectByProxy"的组件上使用。 支持组件可在[ADF 中作为 Azure-SSIS IR 的代理配置自托管 IR 中找到](self-hosted-integration-runtime-proxy-ssis.md#enable-ssis-packages-to-connect-by-proxy)
  * 可以在 [SSMS 报告](https://docs.microsoft.com/sql/integration-services/performance/monitor-running-packages-and-other-operations?view=sql-server-2017#reports)或在 SSIS 包执行活动中指定的日志文件夹中找到执行日志。
  * 作为替代方法，vNet 也可用于访问本地数据。 有关更多详细信息，请参阅[将 Azure-SSIS Integration Runtime 加入虚拟网络](join-azure-ssis-integration-runtime-virtual-network.md)

### <a name="error-message-staging-task-status-failed-staging-task-error-errorcode-2906-errormessage-package-execution-failed-output-operationerrormessages-ssis-executor-exit-code--1n-loglocation-ssistelemetryexecutionlog-effectiveintegrationruntime--executionduration--durationinqueue--integrationruntimequeue--"></a>错误消息："暂存任务状态：失败。 暂存任务错误：错误代码：2906，错误消息：包执行失败.，输出：\"操作错误消息"："SSIS 执行器退出代码：-1.\n"，"Loglocation"："...\\SSIS遥测\\执行日志\\..."，"有效集成运行时"："..."，"执行持续时间"："，"持续时间在队列"：[集成运行时队列]：

确保可视化C++运行时安装在自托管集成运行时计算机上。 更多详细信息可在[ADF 中配置自托管 IR 作为 Azure-SSIS IR 的代理中找到](self-hosted-integration-runtime-proxy-ssis.md#prepare-the-self-hosted-ir)

### <a name="multiple-package-executions-are-triggered-unexpectedly"></a>意外触发多个包执行

* 可能的原因和建议的操作：
  * ADF 存储过程活动或 Lookup 活动用于触发 SSIS 包执行。 t-sql 命令可能会遇到暂时性问题，并触发重新运行，这将导致多个包执行。
  * 请改用 ExecuteSSISPackage 活动，以确保除非用户在活动中设置重试计数，否则包执行不会重新运行。 详细信息可在[https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)
  * 优化 t-sql 命令，以便能够通过检查是否已触发执行来重新运行

### <a name="package-execution-takes-too-long"></a>包执行时间太长

下面是可能的原因和建议的操作：

* 在 SSIS Integration Runtime 中计划了过多的包执行。 所有这些执行将在队列中等待发生。
  * 使用以下公式确定最大值：

    每个 IR 的最大并行执行计数 = 节点计数 * 每个节点的最大并行执行数
  * 若要了解如何设置节点计数以及每个节点的最大并行执行数，请参阅[在 Azure 数据工厂中创建 Azure-SSIS Integration Runtime](create-azure-ssis-integration-runtime.md)。
* SSIS Integration Runtime 已停止或处于不正常状态。 若要了解如何检查 SSIS Integration Runtime 状态和错误，请参阅 [Azure-SSIS Integration Runtime](monitor-integration-runtime.md#azure-ssis-integration-runtime)。

我们还建议您在 **"常规**"选项卡上设置超时：![在"常规"选项卡](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)上设置属性。

### <a name="poor-performance-in-package-execution"></a>包的执行性能不佳

请尝试以下操作：

* 确保 SSIS Integration Runtime 与数据源和目标位于同一区域。

* 将包执行的日志记录级别设置为“性能”，以收集执行中每个组件的持续时间信息。**** 有关详细信息，请参阅 [Integration Services (SSIS) 日志记录](https://docs.microsoft.com/sql/integration-services/performance/integration-services-ssis-logging)。

* 在 Azure 门户中检查 IR 节点性能：
  * 有关如何监视 SSIS Integration Runtime 的信息，请参阅 [Azure-SSIS Integration Runtime](monitor-integration-runtime.md#azure-ssis-integration-runtime)。
  * 可以查看 Azure 门户中数据工厂的指标来查找 SSIS Integration Runtime 的 CPU/内存历史记录。
    ![监视 SSIS Integration Runtime 的指标](media/ssis-integration-runtime-ssis-activity-faq/monitor-metrics-ssis-integration-runtime.png)
