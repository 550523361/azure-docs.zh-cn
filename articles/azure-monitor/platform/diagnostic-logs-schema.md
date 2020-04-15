---
title: Azure 资源日志支持的服务和架构
description: 了解 Azure 资源日志支持的服务和事件架构。
ms.subservice: logs
ms.topic: reference
ms.date: 10/22/2019
ms.openlocfilehash: 8abd8767d9bb7e3c4336f6600b94f6b3f4ea48f1
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2020
ms.locfileid: "81380523"
---
# <a name="supported-services-schemas-and-categories-for-azure-resource-logs"></a>Azure 资源日志支持的服务、架构和类别

> [!NOTE]
> 资源日志以前称为诊断日志。

[Azure Monitor 资源日志](../../azure-monitor/platform/platform-logs-overview.md)是 Azure 服务发出的日志，用于描述这些服务或资源的操作。 通过 Azure Monitor 提供的所有资源日志共享公共顶级架构，且每个服务都能灵活地为其事件发出唯一属性。

资源类型（为 `resourceId` 属性时可用）和 `category` 的组合唯一标识架构。 本文介绍了资源日志的顶级架构以及每个服务的架构链接。

## <a name="top-level-resource-logs-schema"></a>顶级资源日志架构

| 名称 | 必需/可选 | 描述 |
|---|---|---|
| time | 必选 | 事件时间戳 (UTC)。 |
| resourceId | 必选 | 发出事件的资源的资源 ID。 对于租户服务，其形式为 /tenants/tenant-id/providers/provider-name。 |
| tenantId | 对于租户日志而言是必需的 | 此事件关联到的 Active Directory 租户的租户 ID。 此属性仅用于租户级日志，它不会出现在资源级日志中。 |
| operationName | 必选 | 此事件表示的操作的名称。 如果该事件表示 RBAC 操作，则这是 RBAC 操作名称 （例如 Microsoft.Storage/storageAccounts/blobServices/blobs/Read）。 通常以资源管理器操作的形式建模，即使它们不是实际记录的资源管理器操作 (`Microsoft.<providerName>/<resourceType>/<subtype>/<Write/Read/Delete/Action>`) |
| operationVersion | 可选 | 如果使用 API 执行 operationName，则 api-version 与该操作关联（例如 `http://myservice.windowsazure.net/object?api-version=2016-06-01`). 如果没有与此操作相对应的 API，则该版本表示该操作的版本，以防与操作相关联的属性在将来发生更改。 |
| category | 必选 | 事件的日志类别。 类别是可以在特定资源上启用或禁用日志的粒度。 在事件的属性 blob 内显示的属性在特定日志类别和资源类型中相同。 典型的日志类别是“Audit”、“Operational”、“Execution”和“Request”。 |
| resultType | 可选 | 事件的状态。 典型值包括“Started”、“In Progress”、“Succeeded”、“Failed”、“Active”和“Resolved”。 |
| resultSignature | 可选 | 事件的子状态。 如果此操作对应于 REST API 调用，则这是相应 REST 调用的 HTTP 状态代码。 |
| resultDescription | 可选 | 此操作的静态文本说明，例如 “获取存储文件”。 |
| durationMs | 可选 | 操作持续时间，以毫秒为单位。 |
| callerIpAddress | 可选 | 调用方 IP 地址，如果该操作对应于来自具有公开 IP 地址的实体的 API 调用。 |
| correlationId | 可选 | 用于将一组相关事件组合在一起的 GUID。 通常情况下，如果两个事件具有相同 operationName，但具有两个不同状态（例如 “Started”和“Succeeded”），则它们共享相同的关联 ID。 这也可以代表事件之间的其他关系。 |
| identity | 可选 | 描述执行操作的用户或应用程序的标识的 JSON Blob。 通常，这将包括 Active Directory 中的授权和声明/JWT 令牌。 |
| 级别 | 可选 | 事件的严重级别。 必须是信息性、警告、错误或严重。 |
| location | 可选 | 发出事件的资源区域，例如 "美国东部"或"法国南部" |
| properties | 可选 | 与此特定类别的事件相关的任何扩展属性。 所有自定义/唯一属性都必须放入此架构的“B 部分”。 |

## <a name="service-specific-schemas-for-resource-logs"></a>资源日志的服务特定架构
资源诊断日志的架构因资源和日志类别而异。 此列表显示使资源日志和链接对服务和类别特定的架构可用的所有服务（如果可用）。

| 服务 | 架构和文档 |
| --- | --- |
| Azure Active Directory | [概述](../../active-directory/reports-monitoring/concept-activity-logs-azure-monitor.md)、[审核日志架构](../../active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema.md)和[登录架构](../../active-directory/reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md) |
| Analysis Services | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| API 管理 | [API 管理资源日志](../../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| 应用程序网关 |[应用程序网关的日志记录](../../application-gateway/application-gateway-diagnostics.md) |
| Azure 自动化 |[Azure 自动化的日志分析](../../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Azure 批处理日志记录](../../batch/batch-diagnostics.md) |
| Azure Database for MySQL | [Azure Database for MySQL 诊断日志](../../mysql/concepts-server-logs.md#diagnostic-logs) |
| Azure Database for PostgreSQL | [用于后格雷SQL日志的 Azure 数据库](../../postgresql/concepts-server-logs.md#diagnostic-logs) |
| Azure 数据资源管理器 | [Azure 数据资源管理器日志](/azure/data-explorer/using-diagnostic-logs) |
| 认知服务 | [Azure 认知服务的日志记录](../../cognitive-services/diagnostic-logging.md) |
| 容器注册表 | [Azure 容器注册表的日志记录](../../container-registry/container-registry-diagnostics-audit-logs.md) |
| 内容分发网络 | [CDN 的 Azure 日志](../../cdn/cdn-azure-diagnostic-logs.md) |
| CosmosDB | [Azure Cosmos DB 日志记录](../../cosmos-db/logging.md) |
| 数据工厂 | [使用 Azure Monitor 监视数据工厂](../../data-factory/monitor-using-azure-monitor.md) |
| Data Lake Analytics |[访问 Azure 数据湖分析的日志](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[访问 Azure 数据湖存储的日志](../../data-lake-store/data-lake-store-diagnostic-logs.md) |
| 事件中心 |[Azure 事件中心日志](../../event-hubs/event-hubs-diagnostic-logs.md) |
| Express Route | 架构不可用。 |
| Azure 防火墙 | 架构不可用。 |
| IoT 中心 | [IoT 中心操作](../../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Key Vault |[Azure 密钥保管库日志记录](../../key-vault/key-vault-logging.md) |
| Kubernetes 服务 |[Azure 库伯内斯日志记录](../../aks/view-master-logs.md#log-event-schema) |
| 负载均衡器 |[Azure 负载均衡器的 Log Analytics](../../load-balancer/load-balancer-monitor-log.md) |
| 逻辑应用 |[逻辑应用 B2B 自定义跟踪架构](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| 网络安全组 |[网络安全组 (NSG) 的 Log Analytics](../../virtual-network/virtual-network-nsg-manage-log.md) |
| DDOS 保护 | [管理 Azure DDoS 防护标准](../../virtual-network/manage-ddos-protection.md) |
| Power BI 专用 | [日志记录在 Azure 中嵌入的电源 BI](https://docs.microsoft.com/power-bi/developer/azure-pbie-diag-logs) |
| 恢复服务 | [Azure 备份的数据模型](../../backup/backup-azure-reports-data-model.md)|
| 搜索 |[允许并使用搜索流量分析](../../search/search-traffic-analytics.md) |
| 服务总线 |[Azure 服务总线日志](../../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL 数据库 | [Azure SQL 数据库日志记录](../../sql-database/sql-database-metrics-diag-logging.md) |
| 流分析 |[作业日志](../../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| 流量管理器 | [流量管理器日志架构](../../traffic-manager/traffic-manager-diagnostic-logs.md) |
| 虚拟网络 | 架构不可用。 |
| 虚拟网络网关 | 架构不可用。 |

## <a name="supported-log-categories-per-resource-type"></a>每种资源类型支持的日志类别

某些类别可能仅支持特定类型的资源。 这是以某种形式提供的所有可用的列表。  例如，Microsoft.Sql/服务器/数据库类别不适用于所有类型的数据库。 有关详细信息，请参阅有关[SQL 数据库诊断日志记录的信息](../../sql-database/sql-database-metrics-diag-logging.md)。 

|资源类型|类别|类别显示名称|
|---|---|---|
|Microsoft.AAD/domainServices|SystemSecurity|SystemSecurity|
|Microsoft.AAD/domainServices|AccountManagement|AccountManagement|
|Microsoft.AAD/domainServices|LogonLogoff|LogonLogoff|
|Microsoft.AAD/domainServices|ObjectAccess|ObjectAccess|
|Microsoft.AAD/domainServices|PolicyChange|PolicyChange|
|Microsoft.AAD/domainServices|PrivilegeUse|PrivilegeUse|
|Microsoft.AAD/domainServices|DetailTracking|DetailTracking|
|Microsoft.AAD/domainServices|DirectoryServiceAccess|DirectoryServiceAccess|
|Microsoft.AAD/domainServices|AccountLogon|AccountLogon|
|microsoft.aadiam/tenants|登录|登录|
|Microsoft.AnalysisServices/servers|引擎|引擎|
|Microsoft.AnalysisServices/servers|服务|服务|
|Microsoft.ApiManagement/service|GatewayLogs|ApiManagement 网关的相关日志|
|Microsoft.AppPlatform/Spring|ApplicationConsole|应用程序控制台|
|Microsoft.Automation/automationAccounts|JobLogs|作业日志|
|Microsoft.Automation/automationAccounts|JobStreams|作业流|
|Microsoft.Automation/automationAccounts|DscNodeStatus|Dsc 节点状态|
|Microsoft.Batch/batchAccounts|ServiceLog|服务日志|
|Microsoft.BatchAI/workspaces|BaiClusterEvent|BaiClusterEvent|
|Microsoft.BatchAI/workspaces|BaiClusterNodeEvent|BaiClusterNodeEvent|
|Microsoft.BatchAI/workspaces|BaiJobEvent|BaiJobEvent|
|Microsoft.Blockchain/blockchainMembers|BlockchainApplication|区块链应用程序|
|Microsoft.Blockchain/blockchainMembers|代理|代理|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|获取终结点的指标，例如带宽、流出量等。|
|Microsoft.ClassicNetwork/networksecuritygroups|网络安全组规则流事件|网络安全组规则流事件|
|Microsoft.CognitiveServices/accounts|审核|审核日志|
|Microsoft.CognitiveServices/accounts|RequestResponse|请求和响应日志|
|Microsoft.ContainerRegistry/registries|ContainerRegistryRepositoryEvents|RepositoryEvent 日志（预览）|
|Microsoft.ContainerRegistry/registries|ContainerRegistryLoginEvents|登录事件（预览）|
|Microsoft.ContainerService/managedClusters|kube-apiserver|Kubernetes API 服务器|
|Microsoft.ContainerService/managedClusters|kube-controller-manager|Kubernetes 控制器管理器|
|Microsoft.ContainerService/managedClusters|kube-scheduler|Kubernetes 计划程序|
|Microsoft.ContainerService/managedClusters|kube-audit|Kubernetes 审核|
|Microsoft.ContainerService/managedClusters|cluster-autoscaler|Kubernetes 群集自动缩放程序|
|Microsoft.Databricks/workspaces|dbfs|Databricks 文件系统|
|Microsoft.Databricks/workspaces|clusters|Databricks 群集|
|Microsoft.Databricks/workspaces|accounts|Databricks 帐户|
|Microsoft.Databricks/workspaces|jobs|Databricks 作业|
|Microsoft.Databricks/workspaces|笔记本|Databricks Notebook|
|Microsoft.Databricks/workspaces|ssh|Databricks SSH|
|Microsoft.Databricks/workspaces|工作区|Databricks 工作区|
|Microsoft.Databricks/workspaces|机密|Databricks 机密|
|Microsoft.Databricks/workspaces|sqlPermissions|Databricks SQLPermissions|
|Microsoft.Databricks/workspaces|instancePools|实例池|
|Microsoft.DataCatalog/datacatalogs|ScanStatusLogEvent|ScanStatus|
|Microsoft.DataFactory/factories|ActivityRuns|管道活动运行日志|
|Microsoft.DataFactory/factories|PipelineRuns|管道运行日志|
|Microsoft.DataFactory/factories|TriggerRuns|触发器运行日志|
|Microsoft.DataLakeAnalytics/accounts|审核|审核日志|
|Microsoft.DataLakeAnalytics/accounts|Requests|请求日志|
|Microsoft.DataLakeStore/accounts|审核|审核日志|
|Microsoft.DataLakeStore/accounts|Requests|请求日志|
|Microsoft.DataShare/accounts|共享|共享|
|Microsoft.DataShare/accounts|ShareSubscriptions|共享订阅|
|Microsoft.DataShare/accounts|SentShareSnapshots|已发送共享快照|
|Microsoft.DataShare/accounts|ReceivedShareSnapshots|已收到共享快照|
|Microsoft.DBforMySQL/servers|MySqlSlowLogs|MySQL 服务器日志|
|Microsoft.DBforMySQL/servers|MySqlAuditLogs|MySQL 审核日志|
|Microsoft.DBforPostgreSQL/servers|PostgreSQLLogs|PostgreSQL 服务器日志|
|Microsoft.DBforPostgreSQL/servers|QueryStoreRuntimeStatistics|PostgreSQL 查询存储运行时统计信息|
|Microsoft.DBforPostgreSQL/servers|QueryStoreWaitStatistics|PostgreSQL 查询存储等待统计信息|
|Microsoft.DBforPostgreSQL/serversv2|PostgreSQLLogs|PostgreSQL 服务器日志|
|Microsoft.DBforPostgreSQL/serversv2|QueryStoreRuntimeStatistics|PostgreSQL 查询存储运行时统计信息|
|Microsoft.DBforPostgreSQL/serversv2|QueryStoreWaitStatistics|PostgreSQL 查询存储等待统计信息|
|Microsoft.DesktopVirtualization/workspaces|检查点|检查点|
|Microsoft.DesktopVirtualization/workspaces|错误|错误|
|Microsoft.DesktopVirtualization/workspaces|管理|管理|
|Microsoft.DesktopVirtualization/workspaces|源|源|
|Microsoft.DesktopVirtualization/applicationGroups|检查点|检查点|
|Microsoft.DesktopVirtualization/applicationGroups|错误|错误|
|Microsoft.DesktopVirtualization/applicationGroups|管理|管理|
|Microsoft.DesktopVirtualization/hostPools|检查点|检查点|
|Microsoft.DesktopVirtualization/hostPools|错误|错误|
|Microsoft.DesktopVirtualization/hostPools|管理|管理|
|Microsoft.DesktopVirtualization/hostPools|连接|连接|
|Microsoft.DesktopVirtualization/hostPools|HostRegistration|HostRegistration|
|Microsoft.Devices/IotHubs|连接|连接|
|Microsoft.Devices/IotHubs|DeviceTelemetry|设备遥测|
|Microsoft.Devices/IotHubs|C2DCommands|C2D 命令|
|Microsoft.Devices/IotHubs|DeviceIdentityOperations|设备标识操作|
|Microsoft.Devices/IotHubs|FileUploadOperations|文件上传操作|
|Microsoft.Devices/IotHubs|路由|路由|
|Microsoft.Devices/IotHubs|D2C 孪生操作|D2C 孪生操作|
|Microsoft.Devices/IotHubs|C2DTwinOperations|C2D 孪生操作|
|Microsoft.Devices/IotHubs|TwinQueries|孪生查询|
|Microsoft.Devices/IotHubs|JobsOperations|作业操作|
|Microsoft.Devices/IotHubs|DirectMethods|直接方法|
|Microsoft.Devices/IotHubs|DistributedTracing|分布式跟踪（预览版）|
|Microsoft.Devices/IotHubs|配置|配置|
|Microsoft.Devices/IotHubs|DeviceStreams|设备流（预览版）|
|Microsoft.Devices/provisioningServices|DeviceOperations|设备操作|
|Microsoft.Devices/provisioningServices|ServiceOperations|服务操作|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.DocumentDB/databaseAccounts|MongoRequests|MongoRequests|
|Microsoft.DocumentDB/databaseAccounts|QueryRuntimeStatistics|QueryRuntimeStatistics|
|Microsoft.DocumentDB/databaseAccounts|PartitionKeyStatistics|PartitionKeyStatistics|
|Microsoft.DocumentDB/databaseAccounts|ControlPlaneRequests|ControlPlaneRequests|
|Microsoft.EnterpriseKnowledgeGraph/services|AuditEvent|AuditEvent 日志|
|Microsoft.EnterpriseKnowledgeGraph/services|DataIssue|DataIssue 日志|
|Microsoft.EnterpriseKnowledgeGraph/services|Requests|配置日志|
|Microsoft.EventHub/namespaces|ArchiveLogs|存档日志|
|Microsoft.EventHub/namespaces|OperationalLogs|操作日志|
|Microsoft.EventHub/namespaces|AutoScaleLogs|自动缩放日志|
|Microsoft.EventHub/namespaces|KafkaCoordinatorLogs|Kafka 协调器日志|
|Microsoft.EventHub/namespaces|KafkaUserErrorLogs|Kafka 用户错误日志|
|Microsoft.EventHub/namespaces|EventHubVNetConnectionEvent|VNet/IP 筛选连接日志|
|Microsoft.EventHub/namespaces|CustomerManagedKeyUserLogs|客户管理的密钥日志|
|Microsoft.HealthcareApis/services|AuditLogs|审核日志|
|Microsoft.Insights/AutoscaleSettings|AutoscaleEvaluations|自动缩放评估|
|Microsoft.Insights/AutoscaleSettings|AutoscaleScaleActions|自动缩放缩放操作|
|Microsoft.IoTSpaces/Graph|跟踪|跟踪|
|Microsoft.IoTSpaces/Graph|可运行|可运行|
|Microsoft.IoTSpaces/Graph|审核|审核|
|Microsoft.IoTSpaces/Graph|UserDefinedFunction|UserDefinedFunction|
|Microsoft.IoTSpaces/Graph|流入量|流入量|
|Microsoft.IoTSpaces/Graph|流出量|流出量|
|Microsoft.KeyVault/vaults|AuditEvent|审核日志|
|Microsoft.Kusto/Clusters|SucceededIngestion|成功引入操作|
|Microsoft.Kusto/Clusters|FailedIngestion|引入操作失败|
|Microsoft.Logic/workflows|WorkflowRuntime|工作流运行时诊断事件|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|集成帐户跟踪事件|
|Microsoft.MachineLearningServices/workspaces|AmlComputeClusterEvent|AmlComputeClusterEvent|
|Microsoft.MachineLearningServices/workspaces|AmlComputeClusterNodeEvent|AmlComputeClusterNodeEvent|
|Microsoft.MachineLearningServices/workspaces|AmlComputeJobEvent|AmlComputeJobEvent|
|Microsoft.Media/mediaservices|KeyDeliveryRequests|密钥传递请求|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|网络安全组事件|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|网络安全组规则计数器|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupFlowEvent|网络安全组规则流事件|
|Microsoft.Network/publicIPAddresses|DDoSProtectionNotifications|DDoS 保护通知|
|Microsoft.Network/publicIPAddresses|DDoSMitigationFlowLogs|DDoS 缓解决策的流日志|
|Microsoft.Network/publicIPAddresses|DDoSMitigationReports|DDoS 缓解措施报告|
|Microsoft.Network/virtualNetworks|VMProtectionAlerts|VM 保护警报|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|应用程序网关访问日志|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|应用程序网关性能日志|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|应用程序网关防火墙日志|
|Microsoft.Network/azurefirewalls|AzureFirewallApplicationRule|Azure 防火墙应用程序规则|
|Microsoft.Network/azurefirewalls|AzureFirewallNetworkRule|Azure 防火墙网络规则|
|Microsoft.Network/virtualNetworkGateways|GatewayDiagnosticLog|网关诊断日志|
|Microsoft.Network/virtualNetworkGateways|TunnelDiagnosticLog|隧道诊断日志|
|Microsoft.Network/virtualNetworkGateways|RouteDiagnosticLog|路由诊断日志|
|Microsoft.Network/virtualNetworkGateways|IKEDiagnosticLog|IKE 诊断日志|
|Microsoft.Network/virtualNetworkGateways|P2SDiagnosticLog|P2S 诊断日志|
|Microsoft.Network/trafficManagerProfiles|ProbeHealthStatusEvents|流量管理器探测运行状况结果事件|
|Microsoft.Network/expressRouteCircuits|PeeringRouteLog|对等互连路由表日志|
|Microsoft.Network/vpnGateways|GatewayDiagnosticLog|网关诊断日志|
|Microsoft.Network/vpnGateways|TunnelDiagnosticLog|隧道诊断日志|
|Microsoft.Network/vpnGateways|RouteDiagnosticLog|路由诊断日志|
|Microsoft.Network/vpnGateways|IKEDiagnosticLog|IKE 诊断日志|
|Microsoft.Network/frontdoors|FrontdoorAccessLog|Frontdoor 访问日志|
|Microsoft.Network/frontdoors|FrontdoorWebApplicationFirewallLog|Frontdoor Web 应用程序防火墙日志|
|Microsoft.Network/p2sVpnGateways|GatewayDiagnosticLog|网关诊断日志|
|Microsoft.Network/p2sVpnGateways|IKEDiagnosticLog|IKE 诊断日志|
|Microsoft.Network/p2sVpnGateways|P2SDiagnosticLog|P2S 诊断日志|
|Microsoft.Network/bastionHosts|BastionAuditLogs|堡垒审核日志|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|负载均衡器警报事件|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|负载均衡器探测运行状况|
|Microsoft.PowerBIDedicated/capacities|引擎|引擎|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Azure 备份报告数据|
|Microsoft.RecoveryServices/Vaults|CoreAzureBackup|核心 Azure 备份数据|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupJobs|附加 Azure 备份作业数据|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupAlerts|附加 Azure 备份警报数据|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupPolicy|附加 Azure 备份策略数据|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupStorage|附加 Azure 备份存储数据|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupProtectedInstance|附加 Azure 备份受保护实例数据|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Azure Site Recovery 作业|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Azure Site Recovery 事件|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Azure Site Recovery 复制项|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationStats|Azure Site Recovery 复制统计信息|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryRecoveryPoints|Azure Site Recovery 恢复点|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationDataUploadRate|Azure Site Recovery 复制数据上传速度|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryProtectedDiskDataChurn|Azure Site Recovery 受保护的磁盘数据改动|
|Microsoft.Search/searchServices|OperationLogs|操作日志|
|Microsoft.ServiceBus/namespaces|OperationalLogs|操作日志|
|Microsoft.Sql/servers/databases|SQLInsights|SQL Insights|
|Microsoft.Sql/servers/databases|AutomaticTuning|自动优化|
|Microsoft.Sql/servers/databases|QueryStoreRuntimeStatistics|查询存储运行时统计信息|
|Microsoft.Sql/servers/databases|QueryStoreWaitStatistics|查询存储等待统计信息|
|Microsoft.Sql/servers/databases|错误|错误|
|Microsoft.Sql/servers/databases|DatabaseWaitStatistics|数据库等待统计信息|
|Microsoft.Sql/servers/databases|超时|超时|
|Microsoft.Sql/servers/databases|块|块|
|Microsoft.Sql/servers/databases|死锁数|死锁数|
|Microsoft.Sql/servers/databases|审核|审核日志|
|Microsoft.Sql/servers/databases|SQLSecurityAuditEvents|SQL 安全审核事件|
|Microsoft.Sql/servers/databases|DmsWorkers|Dms 辅助角色|
|Microsoft.Sql/servers/databases|ExecRequests|Exec 请求|
|Microsoft.Sql/servers/databases|RequestSteps|请求步骤|
|Microsoft.Sql/servers/databases|SqlRequests|Sql 请求|
|Microsoft.Sql/servers/databases|等待|等待|
|Microsoft.Sql/managedInstances|ResourceUsageStats|资源使用情况统计信息|
|Microsoft.Sql/managedInstances|SQLSecurityAuditEvents|SQL 安全审核事件|
|Microsoft.Sql/managedInstances/databases|SQLInsights|SQL Insights|
|Microsoft.Sql/managedInstances/databases|QueryStoreRuntimeStatistics|查询存储运行时统计信息|
|Microsoft.Sql/managedInstances/databases|QueryStoreWaitStatistics|查询存储等待统计信息|
|Microsoft.Sql/managedInstances/databases|错误|错误|
|Microsoft.Storage/storageAccounts/tableServices|StorageRead|StorageRead|
|Microsoft.Storage/storageAccounts/tableServices|StorageWrite|StorageWrite|
|Microsoft.Storage/storageAccounts/tableServices|StorageDelete|StorageDelete|
|Microsoft.Storage/storageAccounts/blobServices|StorageRead|StorageRead|
|Microsoft.Storage/storageAccounts/blobServices|StorageWrite|StorageWrite|
|Microsoft.Storage/storageAccounts/blobServices|StorageDelete|StorageDelete|
|Microsoft.Storage/storageAccounts/fileServices|StorageRead|StorageRead|
|Microsoft.Storage/storageAccounts/fileServices|StorageWrite|StorageWrite|
|Microsoft.Storage/storageAccounts/fileServices|StorageDelete|StorageDelete|
|Microsoft.Storage/storageAccounts/queueServices|StorageRead|StorageRead|
|Microsoft.Storage/storageAccounts/queueServices|StorageWrite|StorageWrite|
|Microsoft.Storage/storageAccounts/queueServices|StorageDelete|StorageDelete|
|Microsoft.StreamAnalytics/streamingjobs|执行|执行|
|Microsoft.StreamAnalytics/streamingjobs|创作|创作|
|microsoft.web/hostingenvironments|AppServiceEnvironmentPlatformLogs|应用服务环境平台日志|
|microsoft.web/sites|FunctionAppLogs|函数应用程序日志|
|microsoft.web/sites|AppServiceHTTPLogs|HTTP 日志|
|microsoft.web/sites|AppServiceConsoleLogs|应用服务控制台日志|
|microsoft.web/sites|AppServiceAppLogs|应用服务应用程序日志|
|microsoft.web/sites|AppServiceFileAuditLogs|站点内容更改审核日志|
|microsoft.web/sites|AppServiceAuditLogs|访问审核日志|
|microsoft.web/sites/slots|FunctionAppLogs|函数应用程序日志|
|microsoft.web/sites/slots|AppServiceHTTPLogs|HTTP 日志|
|microsoft.web/sites/slots|AppServiceConsoleLogs|控制台日志|
|microsoft.web/sites/slots|AppServiceAppLogs|应用程序日志|
|microsoft.web/sites/slots|AppServiceFileAuditLogs|站点内容更改审核日志|
|microsoft.web/sites/slots|AppServiceAuditLogs|访问审核日志|

## <a name="next-steps"></a>后续步骤

* [详细了解资源日志](../../azure-monitor/platform/platform-logs-overview.md)
* [将资源日志流式传输到**事件中心**](../../azure-monitor/platform/resource-logs-stream-event-hubs.md)
* [使用 Azure Monitor REST API 更改资源日志诊断设置](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)
* [使用 Log Analytics 分析 Azure 存储中的日志](../../azure-monitor/platform/collect-azure-metrics-logs.md)
