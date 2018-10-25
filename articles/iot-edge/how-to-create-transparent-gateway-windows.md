---
title: 使用 Azure IoT Edge 创建透明网关 - Windows| Microsoft Docs
description: 使用 Azure IoT Edge 创建可为多个设备处理信息的透明网关
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 6/20/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: f584e2cdcb038c6f8e9fcdbeecc22fb957bd7f8d
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2018
ms.locfileid: "49394795"
---
# <a name="create-a-windows-iot-edge-device-that-acts-as-a-transparent-gateway"></a>创建充当透明网关的 Windows IoT Edge 设备

本文提供了有关将 IoT Edge 设备用作透明网关的详细说明。 对于本文的其余部分，术语 IoT Edge 网关指的是用作透明网关的 IoT Edge 设备。 有关详细信息，请参阅[如何将 IoT Edge 设备用作网关](./iot-edge-as-gateway.md)，其中给出了概念概述。

>[!NOTE]
>当前：
> * 如果网关与 IoT 中心断开连接，则下游设备无法通过网关进行身份验证。
> * 支持 Edge 的设备无法连接到 IoT Edge 网关。 
> * 下游设备不能使用文件上传。

创建透明网关的难点在于安全地将网关连接到下游设备。 Azure IoT Edge 支持使用 PKI 基础结构在这些设备之间建立安全的 TLS 连接。 在这种情况下，我们可以将下游设备连接到充当透明网关的 IoT Edge 设备。  若要保持合理的安全性，下游设备应确认 Edge 设备的标识，因为仅想让设备连接到网关，而不是潜在的恶意网关。

可以创建任何启用设备网关拓扑所需的信任的证书基础结构。 在本文中，我们假设使用相同的证书设置来启用 IoT 中心的 [X.509 CA 安全性](../iot-hub/iot-hub-x509ca-overview.md)，其中涉及与特定 IoT 中心（IoT 中心所有者 CA）关联的 X.509 CA 证书，以及通过此 CA 签名的一系列证书和 Edge 设备的 CA。

![网关设置](./media/how-to-create-transparent-gateway/gateway-setup.png)

网关在连接启动期间向下游设备显示其 Edge 设备 CA 证书。 下游设备检查以确保 Edge 设备 CA 证书由所有者 CA 证书签名。 此过程允许下游设备确认网关是否来自受信任的源。

以下步骤将演示创建证书并将它们安装在正确位置的过程。

## <a name="prerequisites"></a>先决条件
1. 在要用作透明网关的 Windows 设备上[安装 Azure IoT Edge 运行时](./how-to-install-iot-edge-windows-with-windows.md)。

1. 获取 OpenSSL for Windows。 有许多方法可以安装 OpenSSL：

   >[!NOTE]
   >如果已在 Windows 设备上安装了 OpenSSL，则可以跳过此步骤，但请确保 `%PATH%` 环境变量中有 `openssl.exe` 可用。

   * 下载并安装任何[第三方 OpenSSL 二进制文件](https://wiki.openssl.org/index.php/Binaries)，例如从 [SourceForge 上的此项目](https://sourceforge.net/projects/openssl/)下载并安装。
   
   * 在计算机上下载 OpenSSL 源代码并自行生成二进制文件，或者通过 [vcpkg](https://github.com/Microsoft/vcpkg) 生成。 下面列出的说明使用 vcpkg 下载源代码，并在 Windows 计算机上编译和安装 OpenSSL，所用的步骤都很简单。

      1. 导航到要安装 vcpkg 的目录。 从现在开始，我们将该目录称为 $VCPKGDIR。 按照说明下载并安装 [vcpkg](https://github.com/Microsoft/vcpkg)。
   
      1. 安装 vcpkg 后，在 powershell 提示符下运行以下命令以安装适用于 Windows x64 的 OpenSSL 包。 此安装通常需要大约 5 分钟才能完成。

         ```PowerShell
         .\vcpkg install openssl:x64-windows
         ```
      1. 将 `$VCPKGDIR\installed\x64-windows\tools\openssl` 添加到 `PATH` 环境变量中，以便可以调用 `openssl.exe` 文件。

1. 导航到要在其中工作的目录。 从现在开始，我们将它称为 $WRKDIR。  所有文件都将在此目录中创建。
   
   cd $WRKDIR

1.  使用以下命令获取脚本以生成所需的非生产证书。 这些脚本可帮助创建必要的证书来设置透明网关。

      ```PowerShell
      git clone https://github.com/Azure/azure-iot-sdk-c.git
      ```

1. 将配置文件和脚本文件复制到工作目录中。 此外，设置环境变量 OPENSSL_CONF 以使用 openssl_root_ca.cnf 配置文件。

   ```PowerShell
   copy azure-iot-sdk-c\tools\CACertificates\*.cnf .
   copy azure-iot-sdk-c\tools\CACertificates\ca-certs.ps1 .
   $env:OPENSSL_CONF = "$PWD\openssl_root_ca.cnf"
   ```

1. 通过运行以下命令，启用 PowerShell 来运行脚本

   ```PowerShell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted
   ```

1. 使用以下命令，通过点源将脚本使用的函数引入 PowerShell 的全局命名空间
   
   ```PowerShell
   . .\ca-certs.ps1
   ```

1. 通过运行以下命令，验证 OpenSSL 是否已正确安装，并确保不会与现有证书发生名称冲突。 如果出现问题，脚本应说明如何在系统上修复这些问题。

   ```PowerShell
   Test-CACertsPrerequisites
   ```

## <a name="certificate-creation"></a>证书创建
1. 创建所有者 CA 证书和一个中间证书。 所有证书都放置在 `$WRKDIR` 中。

      ```PowerShell
      New-CACertsCertChain rsa
      ```

1. 使用以下命令创建 Edge 设备 CA 证书和私钥。

   >[!NOTE]
   > 不要使用与网关的 DNS 主机名相同的名称。 这样做会导致客户端对这些证书的认证失败。

   ```PowerShell
   New-CACertsEdgeDevice "<gateway device name>"
   ```

## <a name="certificate-chain-creation"></a>证书链创建
使用以下命令从所有者 CA 证书、中间证书和 Edge 设备 CA 证书创建证书链。 将它置于链文件中可以方便地将其安装在充当透明网关的 Edge 设备上。

   ```PowerShell
   Write-CACertsCertificatesForEdgeDevice "<gateway device name>"
   ```

   该脚本创建以下证书和密钥：
   * `$WRKDIR\certs\new-edge-device.*`
   * `$WRKDIR\private\new-edge-device.key.pem`
   * `$WRKDIR\certs\azure-iot-test-only.root.ca.cert.pem`

## <a name="installation-on-the-gateway"></a>在网关上安装
1. 从 Edge 设备上的 $WRKDIR 任意位置复制以下文件，我们将其称为 $CERTDIR。 如果已在 Edge 设备上生成证书，请跳过此步骤。

   * 设备 CA 证书 - `$WRKDIR\certs\new-edge-device-full-chain.cert.pem`
   * 设备 CA 私钥 - `$WRKDIR\private\new-edge-device.key.pem`
   * 所有者 CA - `$WRKDIR\certs\azure-iot-test-only.root.ca.cert.pem`

2. 将安全守护程序配置 yaml 文件中的 `certificate` 属性设置为放置证书和密钥文件的路径。

```yaml
certificates:
  device_ca_cert: "$CERTDIR\\certs\\new-edge-device-full-chain.cert.pem"
  device_ca_pk: "$CERTDIR\\private\\new-edge-device.key.pem"
  trusted_ca_certs: "$CERTDIR\\certs\\azure-iot-test-only.root.ca.cert.pem"
```
## <a name="deploy-edgehub-to-the-gateway"></a>将 EdgeHub 部署到网关
Azure IoT Edge 的主要功能之一是能够从云中将模块部署到 IoT Edge 设备。 本节创建了一个看似为空的部署；但是，即使没有其他模块，Edge 中心也会自动添加到所有部署中。 Edge 中心是 Edge 设备上唯一需要充当透明网关的模块，因此创建空的部署就已足够。 
1. 在 Azure 门户中导航到 IoT 中心。
2. 转到“IoT Edge”并选择要用作网关的 IoT Edge 设备。
3. 选择“设置模块”。
4. 选择“**下一步**”。
5. 在“指定路由”步骤中，应当已有一个默认路由，它将来自所有模块的所有消息都发送到 IoT 中心。 如果没有，请添加以下代码，然后选择“下一步”。
   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```
6. 在“审阅模板”步骤中，选择“提交”。

## <a name="installation-on-the-downstream-device"></a>在下游设备上安装
下游设备可以是使用 [Azure IoT 设备 SDK](../iot-hub/iot-hub-devguide-sdks.md) 的任何应用程序，如[使用 .NET 将设备连接到 IoT 中心](../iot-hub/quickstart-send-telemetry-dotnet.md)中描述的简单设备。 下游设备应用程序必须信任**所有者 CA** 证书，以便验证与网关设备的 TLS 连接。 通常可通过两种方法执行此步骤：在 OS 级别，或（对于某些语言而言）在应用程序级别。

### <a name="os-level"></a>OS 级别
在 OS 证书存储中安装此证书可使所有应用程序将所有者 CA 证书用作受信任的证书。

* Ubuntu - 下面是如何在 Ubuntu 主机上安装 CA 证书的示例。

   ```cmd
   sudo cp $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem  /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
   sudo update-ca-certificates
   ```
 
    应看到一条消息“正在更新 /etc/ssl/certs 中的证书...已添加 1 个，已删除 0 个；已完成。”

* Windows - 下面是如何在 Windows 主机上安装 CA 证书的示例。
  1. 在“开始”菜单上，键入“管理计算机证书”。 这应该会启动一个名为 `certlm` 的实用程序。
  2. 导航到“证书本地计算机” > “受信任的根证书” > “证书”> 右键单击 >“所有任务” > “导入”以启动证书导入向导。
  3. 按照指示执行步骤并导入证书文件 $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem。
  4. 完成后，应看到“已成功导入”消息。

### <a name="application-level"></a>应用程序级别
对于 .NET 应用程序，可以添加以下片段来信任 PEM 格式的证书。 使用 `$CERTDIR\certs\azure-iot-test-only.root.ca.cert.pem` 初始化变量 `certPath`。

   ```
   using System.Security.Cryptography.X509Certificates;

   ...

   X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
   store.Open(OpenFlags.ReadWrite);
   store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
   store.Close();
   ```

## <a name="connect-the-downstream-device-to-the-gateway"></a>将下游设备连接到网关
使用引用网关设备主机名的连接字符串来初始化 IoT 中心设备 sdk。 通过将 `GatewayHostName` 属性追加到设备连接字符串来完成此步骤。 例如，此处为设备的示例设备连接字符串，我们向它追加了 `GatewayHostName` 属性：

   ```
   HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com
   ```

   >[!NOTE]
   >这是测试所有内容是否已正确设置的示例命令。 应看到一条消息“验证成功”。
   >
   >openssl s_client -connect mygateway.contoso.com:8883 -CAfile $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem -showcerts

## <a name="routing-messages-from-downstream-devices"></a>路由来自下游设备的消息
IoT Edge 运行时可以像模块发送的消息一样路由从下游设备发送的消息。 这允许将任何数据发送到云之前在网关上运行的模块中执行分析。 下面的路由将用于将消息从名为 `sensor` 的下游设备发送到模块名称 `ai_insights`。

   ```json
   { "routes":{ "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" } }
   ```

有关消息路由的详细信息，请参阅[模块组合](./module-composition.md)。

[!INCLUDE [iot-edge-extended-ofline-preview](../../includes/iot-edge-extended-offline-preview.md)]

## <a name="next-steps"></a>后续步骤
[了解开发 IoT Edge 模块的要求和工具](module-development.md)。
