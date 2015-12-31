<properties
	pageTitle="创建 Azure Batch 帐户 | Microsoft Azure"
	description="了解如何在 Azure 门户中创建 Azure Batch 帐户，以便在云中运行大规模并行工作负荷"
	services="batch"
	documentationCenter=""
	authors="dlepow"
	manager="timlt"
	editor=""/>

<tags
	ms.service="batch"
	ms.date="10/26/2015"
	wacn.date=""/>



# 在 Azure 门户中创建和管理 Azure Batch 帐户

> [AZURE.SELECTOR]
- [Azure 门户](batch-account-create-portal.md)
- [Batch Management .NET](batch-management-dotnet.md)

本文说明如何使用 [Azure 门户](https://manage.windowsazure.cn)来创建和管理 Azure Batch 帐户，以及帐户密钥等设置。你需要使用 Batch 帐户 URL 和关联的访问密钥来对所有 Batch API 请求进行身份验证。将 Batch 工作负荷的所有 Batch 资源（例如池、作业和任务）与特定 Batch 帐户相关联。

>[AZURE.NOTE]目前，预览门户支持用于管理 Batch 帐户和查看一些帐户资源的功能。开发人员可以通过 Batch API 来使用完整的 Batch 功能。

## 创建批处理帐户

1. 登录到 [Azure 门户](https://manage.windowsazure.cn)。

2. 单击“新建”>“计算”>“Batch 服务”。

	![应用商店中的批处理][marketplace_portal]

3. 检查信息，然后单击“创建”。

4. 在“新建批处理帐户”边栏选项卡中，输入以下信息：

	a.在“帐户名”中，输入要在批处理帐户 URL 中使用的唯一名称。

	>[AZURE.NOTE]Batch 帐户名在 Azure 中必须是唯一的，长度为 3 到 24 个字符，并且只能包含小写字母和数字。

	b.如果你有多个订阅，请单击“订阅”以选择要在其中创建帐户的可用订阅。

	c.单击“资源组”，为帐户选择现有的资源组，或创建一个新组。

	d.在“位置”中，选择批处理已上市的 Azure 区域。

	![创建批处理帐户][account_portal]

5. 单击“创建”完成帐户创建。

## 管理访问密钥和帐户设置
创建帐户之后，你可以在门户中找到它，以便管理访问密钥、已授权的用户和其他设置。

Batch 帐户 URL 显示在“Essentials”中。该 URL 的格式为 `https://<account_name>.<region>.batch.chinacloudapi.cn`。

若要查看和管理访问密钥，单击密钥图标。

![批处理帐户密钥][account_keys]

## 有关 Batch 帐户的其他注意事项

* 创建和管理 Batch 帐户的其他方法包括 [Batch PowerShell cmdlet](batch-powershell-cmdlets-get-started.md) 和 [Batch Management .NET 库](http://www.nuget.org/packages/Microsoft.Azure.Management.Batch/)。


* Azure 不收取 Batch 帐户的费用。当你使用 Azure 计算资源和其他服务时，只有在运行工作负荷时才需要付费（请参阅 [Batch 定价](/pricing/details/batch/)）。

* 你可以在单个 Batch 帐户中运行多个 Batch 工作负荷，或者在不同 Azure 区域的 Batch 帐户之间分散工作负荷。

* 如果你正在运行多个大规模 Batch 工作负荷，请注意适用于 Azure 订阅和每个 Batch 帐户的特定 [Batch 服务配额与限制](batch-quota-limit.md)。Batch 帐户的当前配额显示在预览门户上的帐户属性中。

## 后续步骤

* 有关 Batch 概念的详细信息，请参阅 [Azure Batch 功能概述](batch-api-basics.md)。

* 使用 [Batch .NET 客户端库](batch-dotnet-get-started.md)开始开发你的第一个应用程序。

[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG

<!---HONumber=Mooncake_1221_2015-->