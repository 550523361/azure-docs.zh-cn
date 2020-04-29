---
title: 对 Azure blob 存储中的数据采样 - Team Data Science Process
description: 在 Azure blob 存储中数据进行采样的方法：通过以编程方式下载数据，并使用以 Python 编写的过程对其进行采样。
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 4832762a88073f4d819925659bf9078e18f60c2d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "76720266"
---
# <a name="sample-data-in-azure-blob-storage"></a><a name="heading"></a>对 Azure blob 存储中的数据采样

本文介绍对在 Azure blob 存储中数据进行采样的方法：通过以编程方式下载数据，并使用以 Python 编写的过程对其进行采样。

**为什么对数据进行采样？**
如果计划要分析的数据集很大，通常最好是对数据进行向下采样，以将数据减至较小但具备代表性且更易于管理的规模。 采样有助于数据理解、探索和特征工程。 它在 Cortana Analytics 进程中的作用是能够快速建立数据处理函数和机器学习模型的快速原型。

此采样任务是[团队数据科学流程 (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/) 中的一个步骤。

## <a name="download-and-down-sample-data"></a>下载和向下采样数据
1. 使用以下示例 Python 代码中的 Blob 服务从 Azure blob 存储下载数据： 
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))

2. 从以上下载的文件中将数据读入 Pandas 数据帧。
   
        import pandas as pd
   
        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. 使用 `numpy` 的 `random.choice` 向下采样数据，如下所示：
   
        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

现在，你可以使用上面的数据帧，其中包含一个用于进一步探索和功能生成的百分比示例。

## <a name="upload-data-and-read-it-into-azure-machine-learning"></a><a name="heading"></a>上传数据并读入 Azure 机器学习
可以使用以下示例代码向下采样数据，并直接在 Azure 机器学习中使用：

1. 将数据帧写入本地文件
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. 使用以下示例代码将本地文件上传到 Azure blob：
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
   
        try:
   
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
   
        except:            
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. 使用 Azure 机器学习[导入数据](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/)从 Azure blob 中读取数据，如下图中所示：

![blob 读取器](./media/sample-data-blob/reader_blob.png)

