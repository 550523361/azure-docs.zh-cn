---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: cb160a140b5c0cb184a5172da10ade0de37c4fed
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164931"
---
<!--v-sharos 10/13/2105 virtual device security-->

使用 StorSimple 虚拟设备时，请注意以下安全事项：

* 虚拟设备是通过 Microsoft Azure 订阅保护的。 这意味着，如果使用虚拟设备时 Azure 订阅遭到攻击，存储在该虚拟设备上的数据也会受到影响。
* 在 Azure 经典门户中可以安全使用用于加密 Azure StorSimple 中存储的数据的证书公钥，私钥保留在 StorSimple 设备中。 在 StorSimple 虚拟设备上，公钥和私钥都存储在 Azure 中。
* 虚拟设备托管在 Microsoft Azure 数据中心内。

