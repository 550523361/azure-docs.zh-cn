---
title: 在 Windows Server 虚拟机上运行 Azure IoT Edge | Microsoft Docs
description: 有关在 Windows Server 市场虚拟机上设置 Azure IoT Edge 的说明
author: gregman-msft
manager: arjmands
ms.reviewer: kgremban
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: philmea
ms.openlocfilehash: 5f88a21efd04c9dd24fe31e925a3b911b5ec9df2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "77045892"
---
# <a name="run-azure-iot-edge-on-windows-server-virtual-machines"></a>在 Windows Server 虚拟机上运行 Azure IoT Edge

使用 Azure IoT Edge 运行时可将设备转变为 IoT Edge 设备。 该运行时可以部署在像 Raspberry Pi 一样小的设备上，也可以部署在像工业服务器一样大的设备上。 使用 IoT Edge 运行时配置设备后，即可开始从云中部署业务逻辑。

若要了解有关 IoT Edge 运行时如何工作以及包含哪些组件的详细信息，请参阅[了解 Azure IoT Edge 运行时及其体系结构](iot-edge-runtime.md)。

本文列出了使用 [Windows Server](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview) Azure 市场套餐在 Windows Server 2019 虚拟机上运行 Azure IoT Edge 运行时的步骤。 若要在其他版本中使用，请遵照[在 Windows 上安装 Azure IoT Edge 运行时](how-to-install-iot-edge-windows.md)中的说明。

## <a name="deploy-from-the-azure-marketplace"></a>从 Azure 市场部署

1. 导航到 [Windows Server](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview) Azure 市场套餐，或者在 [Azure 市场](https://azuremarketplace.microsoft.com/)中搜索“Windows Server”
2. 选择“立即获取” 
3. 在“软件计划”中，找到“包含容器的 Windows Server 2019 Datacenter Server Core”，并在下一个对话框中选择“继续”。  
    * 对于其他版本包含容器的 Windows Server，也可以遵照这些说明
4. 进入 Azure 门户后，选择“创建”并遵循向导部署 VM。 
    * 首次试用 VM 时，在公共入站端口菜单中可以十分方便地使用密码和启用 RDP 与 SSH。
    * 如果你有资源密集型工作负荷，应通过添加更多 CPU 和/或内存来升级虚拟机大小。
5. 部署虚拟机后，请将其配置为连接到 IoT 中心：
    1. 从在 IoT 中心中创建的 IoT Edge 设备复制设备连接字符串。 请参阅过程[在 Azure 门户中检索连接字符串](how-to-register-device.md#retrieve-the-connection-string-in-the-azure-portal)。
    1. 在 Azure 门户中选择新建的虚拟机资源，并打开“运行命令”选项 
    1. 选择“RunPowerShellScript”选项 
    1. 将此脚本复制到包含你的设备连接字符串的命令窗口：

        ```powershell
        . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
        Install-IoTEdge -Manual -DeviceConnectionString '<connection-string>'
        ```

    1. 选择“运行”来执行该脚本，以安装 IoT Edge 运行时并设置连接字符串 
    1. 一两分钟后，将会看到一条消息，指出已成功安装并预配 Edge 运行时。

## <a name="deploy-from-the-azure-portal"></a>从 Azure 门户部署

1. 在 Azure 门户中，搜索“Windows Server”并选择“Windows Server 2019 Datacenter”以开始运行 VM 创建工作流。 
2. 在“选择软件计划”中，选择“包含容器的 Windows Server 2019 Datacenter Server Core”，然后选择“创建”  
3. 完成上述“从 Azure 市场部署”说明中的步骤 5。

## <a name="deploy-from-azure-cli"></a>从 Azure CLI 部署

1. 如果在桌面上使用 Azure CLI，请先登录：

   ```azurecli-interactive
   az login
   ```

1. 如果你有多个订阅，请选择要使用的订阅：
   1. 列出订阅：

      ```azurecli-interactive
      az account list --output table
      ```

   1. 复制要使用的订阅的 SubscriptionID 字段
   1. 结合复制的 ID 运行以下命令：

      ```azurecli-interactive
      az account set -s {SubscriptionId}
      ```

1. 创建新资源组（或者在后续步骤中指定现有的资源组）：

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```

1. 创建新虚拟机：

   ```azurecli-interactive
   az vm create -g IoTEdgeResources -n EdgeVM --image MicrosoftWindowsServer:WindowsServer:2019-Datacenter-Core-with-Containers:latest  --admin-username azureuser --generate-ssh-keys --size Standard_DS1_v2
   ```

   * 此命令将提示输入密码，但你可以添加选项 `--admin-password`，以便更轻松地在脚本中进行设置
   * 仅当使用远程桌面时，Windows Server Core 映像才支持命令行，因此，如果你偏向于完整桌面体验，请指定 `MicrosoftWindowsServer:WindowsServer:2019-Datacenter-with-Containers:latest` 作为映像

1. 设置设备连接字符串（如果你不熟悉此过程，可以按照[使用 Azure CLI 检索连接字符串](how-to-register-device.md#retrieve-the-connection-string-with-the-azure-cli)过程进行操作）：

   ```azurecli-interactive
   az vm run-command invoke -g IoTEdgeResources -n EdgeVM --command-id RunPowerShellScript --script ". {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `Install-IoTEdge -Manual -DeviceConnectionString '<connection-string>'"
   ```

## <a name="next-steps"></a>后续步骤

预配了安装运行时的 IoT Edge 设备后，现在可以[部署 IoT Edge 模块](how-to-deploy-modules-portal.md)。

如果无法正确安装 Edge 运行时，请参阅[故障排除](troubleshoot.md)页。

若要将现有安装更新到最新版本的 IoT Edge，请参阅[更新 IoT Edge 安全守护程序和运行时](how-to-update-iot-edge.md)。

在 [Windows 虚拟机文档](https://docs.microsoft.com/azure/virtual-machines/windows/)中详细了解如何使用 Windows 虚拟机。

若要在安装后通过 SSH 连接到此 VM，请遵循使用远程桌面或远程 PowerShell [安装 OpenSSH for Windows Server](https://docs.microsoft.com/windows-server/administration/openssh/openssh_install_firstuse#installing-openssh-with-powershell) 的指导。
