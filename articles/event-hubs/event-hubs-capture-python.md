---
title: 快速入门：从 Python 应用读取捕获的数据 - Azure 事件中心
description: 快速入门：使用 Azure Python SDK 演示事件中心捕获功能的脚本。
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
editor: ''
ms.assetid: bdff820c-5b38-4054-a06a-d1de207f01f6
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.custom: seodec18
ms.date: 01/15/2020
ms.author: shvija
ms.openlocfilehash: 6c830cf871c2ae650bb61e8b3712a664e9e405d4
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "77187287"
---
# <a name="quickstart-event-hubs-capture-walkthrough-python-azure-eventhub-version-1"></a>快速入门：事件中心捕获演练：Python（azure-eventhub 版本 1）

捕获是 Azure 事件中心的一项功能。 可使用捕获功能自动将事件中心内的流数据传送到所选的 Azure Blob 存储帐户。 使用此功能，可以轻松地对实时流数据执行批处理操作。 本文介绍如何通过 Python 使用事件中心捕获功能。 有关事件中心捕获功能的详细信息，请参阅[通过 Azure 事件中心捕获事件][Overview of Event Hubs Capture]。

本演练使用 [Azure Python SDK](https://azure.microsoft.com/develop/python/) 演示捕获功能。 *sender.py* 程序以 JSON 格式将模拟的环境遥测数据发送到事件中心。 事件中心使用捕获功能将此数据分批写入到 Blob 存储。 *capturereader.py* 应用读取这些 Blob，为每个设备创建一个追加文件，并在每个设备上将数据写入 *.csv* 文件。

> [!WARNING]
> 本快速入门适用于 Azure 事件中心 Python SDK 版本 1。 我们建议你将代码[迁移](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub/migration_guide.md)到 [Python SDK 版本 5](get-started-capture-python-v2.md)。

本演练中的操作： 

> [!div class="checklist"]
> * 在 Azure 门户中创建 Azure Blob 存储帐户和容器。
> * 启用事件中心捕获，并将其定向到你的存储帐户。
> * 使用 Python 脚本将数据发送到事件中心。
> * 使用另一个 Python 脚本通过事件中心捕获功能读取并处理文件。

## <a name="prerequisites"></a>先决条件

- Python 3.4 或更高版本，其中已安装并更新 `pip`。
  
- Azure 订阅。 如果没有订阅，请在开始之前[创建一个免费帐户](https://azure.microsoft.com/free/)。
  
- 按照以下文档中的说明创建的活动事件中心命名空间和事件中心：[快速入门：使用 Azure 门户创建事件中心](event-hubs-create.md)。 记下命名空间和事件中心名称，以便稍后在本演练中使用。 
  
  > [!NOTE]
  > 如果已有可用的存储容器，可以在创建事件中心时启用捕获并选择存储容器。 
  > 
  
- 事件中心共享访问密钥名称和主密钥值。 在“事件中心”页上的“共享访问策略”下查找或创建这些值。  默认访问密钥名称为 **RootManageSharedAccessKey**。 复制访问密钥名称和主密钥值，以便稍后在本演练中使用。 

## <a name="create-an-azure-blob-storage-account-and-container"></a>创建 Azure Blob 存储帐户和容器

创建用于捕获的存储帐户和容器。 

1. 登录 [Azure 门户][Azure portal]。
2. 在左侧导航栏中选择“存储帐户”，然后在“存储帐户”屏幕上选择“添加”。   
3. 在存储帐户创建屏幕上选择订阅和资源组，然后为存储帐户指定名称。 对于其他选项，可以保留默认值。 选择“查看 + 创建”，检查设置，然后选择“创建”。   
   
   ![创建存储帐户][1]
   
4. 部署完成后，选择“转到资源”，然后在存储帐户的“概述”屏幕上选择“容器”。   
5. 在“容器”屏幕上，选择“+ 容器”。   
6. 在“新建容器”屏幕上为容器指定名称，然后选择“确定”。   请记下该容器名称，以便稍后在本演练中使用。 
7. 在“容器”屏幕的左侧导航栏中，选择“访问密钥”。   复制“存储帐户名称”以及“密钥 1”下的“密钥”值，以便稍后在本演练中使用。   
 
## <a name="enable-event-hubs-capture"></a>启用事件中心捕获

1. 在 Azure 门户中导航到你的事件中心：从“所有资源”中选择其事件中心命名空间，在左侧导航栏中选择“事件中心”，然后选择你的事件中心。   
2. 在事件中心的“概述”屏幕上，选择“捕获事件”。  
3. 在“捕获”屏幕上，选择“打开”。   然后，在“Azure 存储容器”下，选择“选择容器”。   
4. 从“容器”屏幕上选择要使用的存储容器，然后选择“选择”。   
5. 在“捕获”屏幕上，选择“保存更改”。   

## <a name="create-a-python-script-to-send-events-to-event-hub"></a>创建用于将事件发送到事件中心的 Python 脚本
此脚本将向事件中心发送 200 个事件。 事件是以 JSON 格式发送的简单环境读数。

1. 打开常用的 Python 编辑器，如 [Visual Studio Code][Visual Studio Code]。
2. 创建名为 *sender.py* 的新文件。 
3. 将以下代码粘贴到 *sender.py* 中。 将事件中心的 \<namespace>、\<AccessKeyName>、\<primary key value> 和 \<eventhub> 替换为自己的值。
   
   ```python
   import uuid
   import datetime
   import random
   import json
   from azure.servicebus.control_client import ServiceBusService
   
   sbs = ServiceBusService(service_namespace='<namespace>', shared_access_key_name='<AccessKeyName>', shared_access_key_value='<primary key value>')
   devices = []
   for x in range(0, 10):
       devices.append(str(uuid.uuid4()))
   
   for y in range(0,20):
       for dev in devices:
           reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
           s = json.dumps(reading)
           sbs.send_event('<eventhub>', s)
       print(y)
   ```
4. 保存文件。

## <a name="create-a-python-script-to-read-capture-files"></a>创建用于读取捕获文件的 Python 脚本

此脚本读取捕获的文件，并为每个设备创建一个文件，用于仅写入该设备的数据。

1. 在 Python 编辑器中，创建名为 *capturereader.py* 的新文件。 
2. 将以下代码粘贴到 *capturereader.py* 中。 将 \<storageaccount>、\<storage account access key> 和 \<storagecontainer> 替换为保存的值。
   
   ```python
   import os
   import string
   import json
   import avro.schema
   from avro.datafile import DataFileReader, DataFileWriter
   from avro.io import DatumReader, DatumWriter
   from azure.storage.blob import BlockBlobService
   
   def processBlob(filename):
       reader = DataFileReader(open(filename, 'rb'), DatumReader())
       dict = {}
       for reading in reader:
           parsed_json = json.loads(reading["Body"])
           if not 'id' in parsed_json:
               return
           if not parsed_json['id'] in dict:
               list = []
               dict[parsed_json['id']] = list
           else:
               list = dict[parsed_json['id']]
               list.append(parsed_json)
       reader.close()
       for device in dict.keys():
           deviceFile = open(device + '.csv', "a")
           for r in dict[device]:
               deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')
   
   def startProcessing(accountName, key, container):
       print('Processor started using path: ' + os.getcwd())
       block_blob_service = BlockBlobService(account_name=accountName, account_key=key)
       generator = block_blob_service.list_blobs(container)
       for blob in generator:
           #content_length == 508 is an empty file, so only process content_length > 508 (skip empty files)
           if blob.properties.content_length > 508:
               print('Downloaded a non empty blob: ' + blob.name)
               cleanName = str.replace(blob.name, '/', '_')
               block_blob_service.get_blob_to_path(container, blob.name, cleanName)
               processBlob(cleanName)
               os.remove(cleanName)
           block_blob_service.delete_blob(container, blob.name)
   startProcessing('<storageaccount>', '<storage account access key>', '<storagecontainer>')
   ```

## <a name="run-the-python-scripts"></a>运行 Python 脚本

1. 打开其路径中包含 Python 的命令提示符，并运行以下命令以安装 Python 必备组件包：
   
   ```cmd
   pip install azure-storage
   pip install azure-servicebus
   pip install avro-python3
   ```
   
   如果 `azure-storage` 或 `azure` 的版本较低，则可能需要使用 `--upgrade` 选项。
   
   此外，可能需要运行以下命令。 在大多数系统上并不需要运行此命令。 
   
   ```cmd
   pip install cryptography
   ```
   
2. 从 *sender.py* 和 *capturereader.py* 保存到的目录运行以下命令：
   
   ```cmd
   start python sender.py
   ```
   
   该命令将启动一个新的 Python 进程用于运行发送程序。
   
3. 捕获完成运行后，运行以下命令：
   
   ```cmd
   python capturereader.py
   ```

   捕获处理器将从存储帐户容器下载所有非空 Blob，并将结果作为 *.csv* 文件写入本地目录。 

## <a name="next-steps"></a>后续步骤

若要详细了解事件中心，请参阅： 

* [事件中心捕获概述][Overview of Event Hubs Capture]
* [使用事件中心的示例应用程序](https://github.com/Azure/azure-event-hubs/tree/master/samples)
* [事件中心概述][Event Hubs overview]

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
