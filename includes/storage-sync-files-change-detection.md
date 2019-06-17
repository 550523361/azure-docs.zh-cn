---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: beb08c29587e4ce522131142fd61925b5af45fa9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "66114566"
---
与对服务器终结点所做的更改不同，使用 Azure 门户或 SMB 对 Azure 文件共享所做的更改不会立即检测到并复制。 Azure 文件尚没有更改通知或日记，因此无法在文件更改时自动启动同步会话。 在 Windows Server 上，Azure 文件同步使用 [Windows USN 日记](https://msdn.microsoft.com/library/windows/desktop/aa363798.aspx)可在文件更改时自动启动同步会话。<br /><br /> 为了检测对 Azure 文件共享所做的更改，Azure 文件同步有一个称为“更改检测作业”的计划作业  。 更改检测作业枚举文件共享中的每个文件，然后将它与该文件的同步版本进行比较。 当更改检测作业确定文件已更改时，Azure 文件同步会启动同步会话。 更改检测作业每 24 小时启动一次。 由于更改检测作业的工作原理是枚举 Azure 文件共享中的每个文件，因此大命名空间用时会长于较小的命名空间。 对于大命名空间，用时可能超过每 24 小时一次，才能确定哪些文件已更改。<br /><br />
请注意，使用 REST 对 Azure 文件共享所做的更改将不会更新 SMB 上次修改时间，亦不会被视为同步更改。 <br /><br />
我们正在探讨针对 Azure 文件共享添加类似于针对 Windows Server 上的卷使用的 USN 的更改检测功能。 请在 [Azure 文件 UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) 上为此功能投票，帮助我们确定将来开发此功能的优先级。
