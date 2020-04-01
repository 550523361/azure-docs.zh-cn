---
title: 在 .NET 中创建和管理 blob 快照 - Azure 存储
description: 了解如何在指定时刻及时创建 blob 的只读快照以备份 blob 数据。
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/06/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 9bf5eea55002814f461d375b3db43a37fe4f7aa9
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2020
ms.locfileid: "80474096"
---
# <a name="create-and-manage-a-blob-snapshot-in-net"></a>在 .NET 中创建和管理 blob 快照

快照是在某一时间点拍摄的只读版本的 Blob。 快照可用于备份 Blob。 本文介绍如何使用[适用于 .NET 的 Azure 存储客户端库](/dotnet/api/overview/azure/storage?view=azure-dotnet)创建和管理 blob 快照。

## <a name="about-blob-snapshots"></a>关于 Blob 快照

[!INCLUDE [updated-for-az](../../../includes/storage-data-lake-gen2-support.md)]

Blob 的快照与其基本 Blob 相同，不过，Blob URI 的后面追加了一个 **DateTime** 值，用于指示快照的生成时间。 例如，如果页 Blob URI 为 `http://storagesample.core.blob.windows.net/mydrives/myvhd`，则快照 URI 将类似于 `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`。

> [!NOTE]
> 所有快照共享基本 Blob 的 URI。 基本 Blob 与快照之间的唯一区别体现在追加的 **DateTime** 值。
>

一个 Blob 可以有任意数目的快照。 快照会一直保留，直到被显式删除，这意味着快照的生存期不能长于其基本 Blob。 可以枚举与基本 Blob 关联的快照，以跟踪当前快照。

创建 Blob 的快照时，会将该 Blob 的系统属性复制到具有相同值的快照。 基本 Blob 的元数据也会复制到快照，除非创建快照时为其指定了不同的元数据。 创建快照后，可以读取、复制或删除它，但无法修改它。

任何与基本 Blob 关联的租约都不会影响快照。 无法获取快照上的租约。

VHD 文件用于存储 VM 磁盘的当前信息和状态。 可以将磁盘从 VM 分离或者关闭 VM，然后拍摄其 VHD 文件的快照。 可以在以后使用该快照文件检索该时间点的 VHD 文件并重新创建 VM。

## <a name="create-a-snapshot"></a>创建快照

若要创建块 blob 的快照，请使用以下方法之一：

- [CreateSnapshot](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.createsnapshot)
- [CreateSnapshotAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.createsnapshotasync)

以下代码示例演示如何创建快照。 本示例在创建快照时为其指定了其他元数据。

```csharp
private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
{
    // Create a new block blob in the container.
    CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

    // Add blob metadata.
    baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

    try
    {
        // Upload the blob to create it, with its metadata.
        await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of the base blob.
        // You can specify metadata at the time that the snapshot is created.
        // If no metadata is specified, then the blob's metadata is copied to the snapshot.
        Dictionary<string, string> metadata = new Dictionary<string, string>();
        metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
        await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="delete-snapshots"></a>删除快照

若要删除 blob，必须先删除该 blob 的所有快照。 可以单独删除快照，或指定在删除源 Blob 时删除所有快照。 如果尝试删除仍包含快照的 Blob，会发生错误。

若要删除 blob 快照，请使用以下 blob 删除方法之一，并包括 [DeleteSnapshotsOption](/dotnet/api/microsoft.azure.storage.blob.deletesnapshotsoption) 枚举。

- [删除](/dotnet/api/microsoft.azure.storage.blob.cloudblob.delete)
- [DeleteAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteasync)
- [DeleteIfExists](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteifexists)
- [DeleteIfExistsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteifexistsasync)

以下代码示例演示如何在 .NET 中删除 Blob 及其快照，其中 `blockBlob` 是 [CloudBlockBlob][dotnet_CloudBlockBlob] 类型的对象：

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="return-the-absolute-uri-to-a-snapshot"></a>返回快照的绝对 URI

以下代码示例创建一个快照并写出主位置的绝对 URI。

```csharp
//Create the blob service client object.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();

//Get a reference to a blob.
CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
blob.UploadText("This is a blob.");

//Create a snapshot of the blob and write out its primary URI.
CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);
```

## <a name="understand-how-snapshots-accrue-charges"></a>了解快照如何产生费用

创建快照（它是 Blob 的只读副本）会导致帐户产生额外的数据存储费用。 在设计应用程序时，务必要注意在哪些情况下会产生这些费用，以便能最大限度地降低成本。

### <a name="important-billing-considerations"></a>重要计费注意事项

以下列表包含创建快照时要考虑的要点。

- 不管唯一的块或页是在 Blob 还是快照中，存储帐户都会产生费用。 在更新快照所基于的 Blob 之前，帐户将不会就与 Blob 关联的快照产生额外费用。 更新基本 Blob 后，它与其快照分离。 发生这种情况时，需要支付每个 Blob 或快照中唯一块或页的费用。
- 在替换块 Blob 中的某个块后，会将该块作为唯一块进行收费。 即使该块具有的块 ID 和数据与它在快照中所具有的 ID 和数据相同也是如此。 重新提交块后，它将偏离它在任何快照中的对应部分，并且要为数据支付费用。 对于使用相同的数据更新的页 Blob 中的页面，也是如此。
- 通过调用 [UploadFromFile][dotnet_UploadFromFile]、[UploadText][dotnet_UploadText]、[UploadFromStream][dotnet_UploadFromStream] 或 [UploadFromByteArray][dotnet_UploadFromByteArray] 方法替换块 Blob 可替换该 Blob 中的所有块。 如果有与该 Blob 关联的快照，则基本 Blob 和快照中的所有块现在将发生偏离，并且你需为这两个 Blob 中的所有块支付费用。 即使基本 Blob 和快照中的数据保持相同也是如此。
- Azure BLOB 服务无法确定这两个块是否包含相同的数据。 每个上传和提交的块均被视为唯一的快，即使它具有相同的数据和块 ID 也是如此。 由于唯一的块会产生费用，因此请务必考虑到更新具有快照的 Blob 将导致产生其他唯一块和额外费用。

### <a name="minimize-cost-with-snapshot-management"></a>使用快照管理最大限度地降低成本

建议细致管理快照以避免产生额外费用。 可遵循这些最佳做法来帮助最大限度地减少快照存储成本：

- 除非应用程序设计需要保留与 Blob 关联的快照，否则请在更新 Blob 时删除并重新创建这些快照，即使你使用相同的数据进行更新也是如此。 通过删除并重新创建 Blob 的快照，可以确保 Blob 和快照不会发生偏离。
- 如果要保留 Blob 的快照，请避免调用 [UploadFromFile][dotnet_UploadFromFile]、[UploadText][dotnet_UploadText]、[UploadFromStream][dotnet_UploadFromStream] 或 [UploadFromByteArray][dotnet_UploadFromByteArray] 来更新 Blob。 这些方法将替换 Blob 中的所有块，导致基本 Blob 及其快照发生明显偏离。 相反，请使用 [PutBlock][dotnet_PutBlock] 和 [PutBlockList][dotnet_PutBlockList] 方法更新尽可能少的块。

### <a name="snapshot-billing-scenarios"></a>快照计费方案

下列方案说明了块 Blob 及其快照将如何产生费用。

#### <a name="scenario-1"></a>方案 1

在方案 1 中，基本 Blob 自创建快照后未进行更新，因此仅唯一块 1、2、3 会产生费用。

![Azure 存储资源](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

#### <a name="scenario-2"></a>方案 2

在方案 2 中，已更新基本 Blob，但未更新快照。 已更新块 3，即使它包含相同的数据和 ID，它也与快照中的块 3 不同。 因此，帐户需要为四个块支付费用。

![Azure 存储资源](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

#### <a name="scenario-3"></a>方案 3

在方案 3 中，已更新基本 Blob，但未更新快照。 块 3 已替换为基础 Blob 中的块 4，但快照仍反映块 3。 因此，帐户需要为四个块支付费用。

![Azure 存储资源](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

#### <a name="scenario-4"></a>方案 4

在方案 4 中，已完全更新基本 Blob，并且其中不包含任何原始块。 因此，帐户需要为所有八个唯一块支付费用。 如果使用如 [UploadFromFile][dotnet_UploadFromFile]、[UploadText][dotnet_UploadText]、[UploadFromStream][dotnet_UploadFromStream] 或 [UploadFromByteArray][dotnet_UploadFromByteArray] 之类的更新方法，则会出现此情况，因为这些方法将替换 Blob 的所有内容。

![Azure 存储资源](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>后续步骤

- 可在[使用递增快照备份 Azure 非托管 VM 磁盘](../../virtual-machines/windows/incremental-snapshots.md)中找到有关使用虚拟机 (VM) 磁盘快照的详细信息

- 有关 Blob 存储使用的其他代码示例，请参阅 [Azure 代码示例](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob)。 可以下载示例应用程序并运行，或在 GitHub 上浏览代码。

[dotnet_AccessCondition]: https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.accesscondition
[dotnet_CloudBlockBlob]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob.cloudblockblob
[dotnet_CreateSnapshotAsync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudpageblob.createsnapshotasync
[dotnet_HTTPStatusCode]: https://docs.microsoft.com/java/api/com.microsoft.store.partnercenter.network.httpstatuscode
[dotnet_PutBlockList]: /dotnet/api/microsoft.azure.storage.blob.cloudblockblob.putblocklist
[dotnet_PutBlock]: /dotnet/api/microsoft.azure.storage.blob.cloudblockblob.putblock
[dotnet_UploadFromByteArray]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob.cloudblob.uploadfrombytearray
[dotnet_UploadFromFile]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob.cloudblob.uploadfromfile
[dotnet_UploadFromStream]: /dotnet/api/microsoft.azure.storage.blob.cloudappendblob.uploadfromstream
[dotnet_UploadText]: /dotnet/api/microsoft.azure.storage.blob.cloudappendblob.uploadtext
