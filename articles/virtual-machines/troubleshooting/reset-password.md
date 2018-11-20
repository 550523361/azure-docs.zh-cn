---
title: 如何在 Azure VM 上重置本地 Linux 密码 | Microsoft Docs
description: 在 Azure VM 上重置本地 Linux 密码的步骤简介
services: virtual-machines-linux
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: d96d75f4f2623476f7af4e6eea930c1f2c503e3a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/07/2018
ms.locfileid: "51226907"
---
# <a name="how-to-reset-local-linux-password-on-azure-vms"></a>如何在 Azure VM 上重置本地 Linux 密码

本文介绍了几种重置本地 Linux 虚拟机 (VM) 密码的方法。 如果用户帐户已过期或者只是想要创建新帐户，则可以使用以下方法来创建新的本地管理员帐户并重新获得对 VM 的访问权限。

## <a name="symptoms"></a>症状

无法登录到 VM 时会收到一条消息，指示所使用的密码不正确。 此外，无法在 Azure 门户上使用 VMAgent 重置密码。

## <a name="manual-password-reset-procedure"></a>手动密码重置过程

1.  删除 VM 并保留附加的磁盘。

2.  将 OS 驱动器作为数据磁盘附加到同一位置中的另一个临时 VM 中。

3.  在临时 VM 上运行以下 SSH 命令，成为超级用户。

    ```bash
    sudo su
    ```

4.  运行 fdisk -l，或查看系统日志以查找新附加的磁盘。 找到要装载的驱动器名称。 然后在临时 VM 上，查找相关的日志文件。

    ```bash
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ```

    下面是 grep 命令的输出示例：

    ```bash
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ```

5.  创建名为 tempmount 的装入点。

    ```bash
    mkdir /tempmount
    ```

6.  在该装入点上装载 OS 磁盘。 通常需要装载 sdc1 或 sdc2。 这将取决于损坏的计算机磁盘的 /etc 目录中的托管分区。

    ```bash
    mount /dev/sdc1 /tempmount
    ```

7.  在进行任何更改之前，请创建核心凭据文件的副本：

    ```bash
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ```

8.  重置所需的用户密码：

    ```bash
    passwd <<USER>> 
    ```

9.  将已修改的文件移动到断开的计算机磁盘上的正确位置。

    ```bash
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    ```

10. 返回到根目录并卸载磁盘。

    ```bash
    cd /
    umount /tempmount
    ```

11. 从管理门户分离磁盘。

12. 重新创建 VM。

## <a name="next-steps"></a>后续步骤

* [Troubleshoot Azure VM by attaching OS disk to another Azure VM](https://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)（通过将 OS 磁盘附加到另一个 Azure VM 对 Azure VM 进行故障排除）

* [Azure CLI: How to delete and re-deploy a VM from VHD](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)（Azure CLI：如何从 VHD 删除和重新部署 VM）
