---
title: 将 Raspberry Pi 连接到 Azure IoT Central 应用程序 (C#) | Microsoft Docs
description: 作为设备开发人员，如何使用C#将 Raspberry Pi 连接到 Azure IoT Central 应用程序。
author: viv-liu
ms.author: viviali
ms.date: 09/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 7a66925dceee4bf90bc6a5cd155f99347bbd124e
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886015"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-c"></a>将 Raspberry Pi 连接到 Azure IoT Central 应用程序 (C#)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

本文介绍如何使用 C# 编程语言以设备开发人员的身份将 Raspberry Pi 连接到 Microsoft Azure IoT Central 应用程序。

## <a name="before-you-begin"></a>开始之前

若要完成本文中的步骤，需要以下组件：

* 基于“示例 Devkit”应用程序模板创建的 Azure IoT Central 应用程序。 有关详细信息，请参阅[创建应用程序快速入门](quick-deploy-iot-central.md)。
* 运行 Raspbian 操作系统的 Raspberry Pi 设备。 Raspberry Pi 必须能够连接到 internet。 有关详细信息，请参阅[设置 Raspberry Pi](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up/3)。

## <a name="sample-devkits-application"></a>**示例 Devkits** 应用程序

从“示例 Devkit”应用程序模板创建的应用程序包含一个具有以下特征的 Raspberry Pi 设备模板：

- 遥测数据，包括设备将收集的以下度量值：
  - 湿度
  - 温度
  - 压力
  - 磁力计 (X, Y, Z)
  - 加速计 (X, Y, Z)
  - 陀螺仪 (X, Y, Z)
- 设置
  - 电压
  - 当前
  - 风扇速度
  - 红外开关。
- 属性
  - “模具编号”设备属性
  - “位置”云属性

有关设备模板配置的完整详细信息，请参阅[Raspberry Pi 设备模板详细信息](#raspberry-pi-device-template-details)。

## <a name="add-a-real-device"></a>添加真实设备

在 Azure IoT Central 应用程序中，从**Raspberry Pi**设备模板添加一个实际设备。 记下设备连接详细信息（**作用域 id**、**设备 id**和**主密钥**）。 有关详细信息，请参阅[将真实设备添加到 Azure IoT Central 应用程序](tutorial-add-device.md)。

### <a name="create-your-net-application"></a>创建 .NET 应用程序

请在桌面计算机上创建和测试设备应用程序。

可以使用 Visual Studio Code 来完成以下步骤。 有关详细信息，请参阅 [Working with C#](https://code.visualstudio.com/docs/languages/csharp)（使用 C#）。

> [!NOTE]
> 可以根据需要使用其他代码编辑器来完成以下步骤。

1. 若要初始化 .NET 项目并添加所需的 NuGet 包，请运行以下命令：

   ```cmd/sh
   mkdir pisample
   cd pisample
   dotnet new console
   dotnet add package Microsoft.Azure.Devices.Client
   dotnet restore
   ```

1. 在 Visual Studio Code 中打开 `pisample` 文件夹， 然后打开 **pisample.csproj** 项目文件。 添加 `<RuntimeIdentifiers>` 标记，如以下代码片段所示：

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifiers>win-arm;linux-arm</RuntimeIdentifiers>
      </PropertyGroup>
      <ItemGroup>
        <PackageReference Include="Microsoft.Azure.Devices.Client" Version="1.19.0" />
      </ItemGroup>
    </Project>
    ```

    > [!NOTE]
    > 你的 **Microsoft.Azure.Devices.Client** 包版本号可能高于所示版本号。

1. 保存 **pisample.csproj**。 如果 Visual Studio Code 提示执行还原命令，请选择“还原”。

1. 打开 **Program.cs**，将内容替换为以下代码：

    ```csharp
    using System;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;

    namespace pisample
    {
      class Program
      {
        static string DeviceConnectionString = "{your device connection string}";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();
        static CancellationTokenSource cts;
        static double baseTemperature = 60;
        static double basePressure = 500;
        static double baseHumidity = 50;
        static void Main(string[] args)
        {
          Console.WriteLine("Raspberry Pi Azure IoT Central example");

          try
          {
            InitClient();
            SendDeviceProperties();

            cts = new CancellationTokenSource();
            SendTelemetryAsync(cts.Token);

            Console.WriteLine("Wait for settings update...");
            Client.SetDesiredPropertyUpdateCallbackAsync(HandleSettingChanged, null).Wait();
            Console.ReadKey();
            cts.Cancel();
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        public static void InitClient()
        {
          try
          {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        public static async void SendDeviceProperties()
        {
          try
          {
            Console.WriteLine("Sending device properties:");
            Random random = new Random();
            TwinCollection telemetryConfig = new TwinCollection();
            reportedProperties["dieNumber"] = random.Next(1, 6);
            Console.WriteLine(JsonConvert.SerializeObject(reportedProperties));

            await Client.UpdateReportedPropertiesAsync(reportedProperties);
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        private static async void SendTelemetryAsync(CancellationToken token)
        {
          try
          {
            Random rand = new Random();

            while (true)
            {
              double currentTemperature = baseTemperature + rand.NextDouble() * 20;
              double currentPressure = basePressure + rand.NextDouble() * 100;
              double currentHumidity = baseHumidity + rand.NextDouble() * 20;

              var telemetryDataPoint = new
              {
                humidity = currentHumidity,
                pressure = currentPressure,
                temp = currentTemperature
              };
              var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
              var message = new Message(Encoding.ASCII.GetBytes(messageString));

              token.ThrowIfCancellationRequested();
              await Client.SendEventAsync(message);

              Console.WriteLine("{0} > Sending telemetry: {1}", DateTime.Now, messageString);

              await Task.Delay(1000);
            }
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Intentional shutdown: {0}", ex.Message);
          }
        }

        private static async Task HandleSettingChanged(TwinCollection desiredProperties, object userContext)
        {
          try
          {
            Console.WriteLine("Received settings change...");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            string setting = "fanSpeed";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            setting = "setVoltage";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            setting = "setCurrent";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            setting = "activateIR";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            await Client.UpdateReportedPropertiesAsync(reportedProperties);
          }

          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        private static void AcknowledgeSettingChange(TwinCollection desiredProperties, string setting)
        {
          reportedProperties[setting] = new
          {
            value = desiredProperties[setting]["value"],
            status = "completed",
            desiredVersion = desiredProperties["$version"],
            message = "Processed"
          };
        }
      }
    }
    ```

    > [!NOTE]
    > 请在下一步更新占位符 `{your device connection string}`。

## <a name="run-your-net-application"></a>运行 .NET 应用程序

将特定于设备的连接字符串添加到代码中，以便设备使用 Azure IoT Central 进行身份验证。 按照这些说明使用之前记下的**作用域 id**、**设备 ID** **和主密钥**[生成设备连接字符串](howto-generate-connection-string.md)。

1. 将`{your device connection string}` **Program.cs**文件中的替换为生成的连接字符串。

1. 在命令行环境中运行以下命令：

   ```cmd/sh
   dotnet restore
   dotnet publish -r linux-arm
   ```

1. 将 `pisample\bin\Debug\netcoreapp2.1\linux-arm\publish` 文件夹复制到 Raspberry Pi 设备。 可以使用 **scp** 命令来复制文件，例如：

    ```cmd/sh
    scp -r publish pi@192.168.0.40:publish
    ```

    有关详细信息，请参阅 [Raspberry Pi 远程访问](https://www.raspberrypi.org/documentation/remote-access/)。

1. 登录到 Raspberry Pi 设备，在 shell 中运行以下命令：

    ```cmd/sh
    sudo apt-get update
    sudo apt-get install libc6 libcurl3 libgcc1 libgssapi-krb5-2 liblttng-ust0 libstdc++6 libunwind8 libuuid1 zlib1g
    ```

1. 在 Raspberry Pi 上运行以下命令：

    ```cmd/sh
    cd publish
    chmod 777 pisample
    ./pisample
    ```

    ![程序启动](./media/howto-connect-raspberry-pi-csharp/device_begin.png)

1. 在 Azure IoT Central 应用程序中，可以看到在 Raspberry Pi 上运行的代码如何与应用程序交互：

   * 在真实设备的“度量”页上，可以看到遥测数据。
   * 在“属性”页上，可以看到报告的“模具编号”属性的值。
   * 在“设置”页中，可以更改 Raspberry Pi 上的各种设置，例如电压和风扇速度。

     以下屏幕截图显示 Raspberry Pi 接收设置更改：

     ![Raspberry Pi 接收设置更改](./media/howto-connect-raspberry-pi-csharp/device_switch.png)

## <a name="raspberry-pi-device-template-details"></a>Raspberry Pi 设备模板详细信息

从“示例 Devkit”应用程序模板创建的应用程序包含一个具有以下特征的 Raspberry Pi 设备模板：

### <a name="telemetry-measurements"></a>遥测数据度量

| 字段名     | 单位  | 最小值 | 最大值 | 小数位数 |
| -------------- | ------ | ------- | ------- | -------------- |
| 湿度       | %      | 0       | 100     | 0              |
| temp           | °C     | -40     | 120     | 0              |
| 压力       | hPa    | 260     | 1260    | 0              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| accelerometerX | mg     | -2000   | 2000    | 0              |
| accelerometerY | mg     | -2000   | 2000    | 0              |
| accelerometerZ | mg     | -2000   | 2000    | 0              |
| gyroscopeX     | mdps   | -2000   | 2000    | 0              |
| gyroscopeY     | mdps   | -2000   | 2000    | 0              |
| gyroscopeZ     | mdps   | -2000   | 2000    | 0              |

### <a name="settings"></a>设置

数字设置

| 显示名称 | 字段名 | 单位 | 小数位数 | 最小值 | 最大值 | 初始 |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| 电压      | setVoltage | 伏 | 0              | 0       | 240     | 0       |
| 当前      | setCurrent | 安培  | 0              | 0       | 100     | 0       |
| 风扇速度    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

切换设置

| 显示名称 | 字段名 | 打开文本 | 关闭文本 | 初始 |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | ON      | OFF      | 关闭     |

### <a name="properties"></a>属性

| 类型            | 显示名称 | 字段名 | 数据类型                              |
| --------------- | ------------ | ---------- | -------------------------------------- |
| 设备属性 | 模具号   | dieNumber  | number                                 |
| Location        | Location     | location   | {lat： float，long： float，alt？： float} |

## <a name="next-steps"></a>后续步骤

现在，你已了解如何将 Raspberry Pi 连接到 Azure IoT Central 应用程序，接下来建议的下一步是了解如何为你自己的 IoT 设备[设置自定义设备模板](howto-set-up-template.md)。
