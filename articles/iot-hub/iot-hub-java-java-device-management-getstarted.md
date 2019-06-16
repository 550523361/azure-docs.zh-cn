---
title: Azure IoT 中心设备管理入门 (Java) | Microsoft Docs
description: 如何使用 Azure IoT 中心设备管理启动远程设备重启。 使用适用于 Java 的 Azure IoT 设备 SDK 实现包含直接方法的模拟设备应用，并使用适用于 Java 的 Azure IoT 服务 SDK 实现调用直接方法的服务应用。
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 08/08/2017
ms.openlocfilehash: e9100a764ba3922e0254b7fa5cd03b18e204925f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "65596017"
---
# <a name="get-started-with-device-management-java"></a>设备管理入门 (Java)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

本教程演示如何：

* 使用 Azure 门户创建 IoT 中心，以及如何在 IoT 中心创建设备标识。

* 创建实现重启设备的直接方法的模拟设备应用。 直接方法是从云中调用的。

* 创建通过 IoT 中心在模拟设备应用上调用重启直接方法的应用。 之后此应用会监视设备的报告属性，查看重启操作何时完成。

本教程结束时，将会创建两个 Java 控制台应用：

simulated-device  。 此应用：

* 使用之前创建的设备标识连接到 IoT 中心。

* 接收重启直接方法调用。

* 模拟物理重启。

* 通过报告属性报告上次重启的时间。

trigger-reboot  。 此应用：

* 在模拟设备应用中调用直接方法。

* 显示模拟设备发送的直接方法调用的响应。

* 显示更新的报告属性。

> [!NOTE]
> 有关 SDK 的信息（可以使用这些 SDK 构建在设备和解决方案后端上运行的应用程序），请参阅 [Azure IoT SDK](iot-hub-devguide-sdks.md)。

要完成本教程，需要：

* Java SE 8。 <br/> [准备开发环境](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md)介绍了如何在 Windows 或 Linux 上安装本教程所用的 Java。

* Maven 3。  <br/> [准备开发环境](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md)介绍如何在 Windows 或 Linux 上安装本教程所用的 [Maven](https://maven.apache.org/what-is-maven.html)。

* 有效的 Azure 帐户。 （如果没有帐户，只需几分钟即可创建一个[免费帐户](https://azure.microsoft.com/pricing/free-trial/)。）

## <a name="create-an-iot-hub"></a>创建 IoT 中心

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>检索 IoT 中心的连接字符串

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>使用直接方法在设备上触发远程重新启动

本部分将创建一个进行如下操作的 Java 控制台应用：

1. 在模拟设备应用中调用重启直接方法。

2. 显示响应。

3. 轮询设备发送的报告属性，以确定重启的完成时间。

此控制台应用连接到 IoT 中心，以便调用该直接方法并读取报告属性。

1. 创建名为 dm-get-started 的空文件夹。

2. 使用命令提示符中的以下命令，在 dm-get-started 文件夹中创建名为 trigger-reboot 的 Maven 项目  。 以下内容演示了一条很长的命令：

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=trigger-reboot -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

3. 在命令提示符下，导航到 trigger-reboot 文件夹。

4. 使用文本编辑器打开 trigger-reboot 文件夹中的 pom.xml 文件，并在 dependencies 节点中添加以下依赖项  。 此依赖项使得可以使用应用中的 iot-service-client 包来与 IoT 中心进行通信：

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > 可以使用 [Maven 搜索](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22)检查是否有最新版本的 **iot-service-client**。

5. 在 **dependencies** 节点后添加以下 **build** 节点。 此配置指示 Maven 使用 Java 1.8 来生成应用：

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

6. 保存并关闭 pom.xml 文件。

7. 使用文本编辑器打开 trigger-reboot\src\main\java\com\mycompany\app\App.java 源文件。

8. 在该文件中添加以下 **import** 语句：

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwin;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

9. 将以下类级变量添加到 **App** 类。 将 `{youriothubconnectionstring}` 替换为在“创建 IoT 中心”部分记下的 IoT 中心连接字符串： 

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "reboot";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

10. 若要实现每隔 10 秒读取一次设备孪生提供的报告属性的线程，请将以下嵌套类添加到 App 类  ：

    ```java
    private static class ShowReportedProperties implements Runnable {
      public void run() {
        try {
          DeviceTwin deviceTwins = DeviceTwin.createFromConnectionString(iotHubConnectionString);
          DeviceTwinDevice twinDevice = new DeviceTwinDevice(deviceId);
          while (true) {
            System.out.println("Get reported properties from device twin");
            deviceTwins.getTwin(twinDevice);
            System.out.println(twinDevice.reportedPropertiesToString());
            Thread.sleep(10000);
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

11. 将 main  方法签名修改为抛出以下异常：

    ```java
    public static void main(String[] args) throws IOException
    ```

12. 若要在模拟设备上调用重启直接方法，请将以下代码添加到 main 方法  ：

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
      System.out.println("Invoke reboot direct method");
      MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, null);

      if(result == null)
      {
        throw new IOException("Invoke direct method reboot returns null");
      }
      System.out.println("Invoked reboot on device");
      System.out.println("Status for device:   " + result.getStatus());
      System.out.println("Message from device: " + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }
    ```

13. 若要启动轮询模拟设备提供的报告属性的线程，请将以下代码添加到 main 方法  ：

    ```java
    ShowReportedProperties showReportedProperties = new ShowReportedProperties();
    ExecutorService executor = Executors.newFixedThreadPool(1);
    executor.execute(showReportedProperties);
    ```

14. 若要能够停止应用，请将以下代码添加到 main 方法  ：

    ```java
    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    System.out.println("Shutting down sample...");
    ```

15. 保存并关闭 trigger-reboot\src\main\java\com\mycompany\app\App.java 文件。

16. 生成 trigger-reboot 后端应用并更正任何错误  。 在命令提示符下，导航到 trigger-reboot 文件夹并运行以下命令：

    `mvn clean package -DskipTests`

## <a name="create-a-simulated-device-app"></a>创建模拟设备应用程序

本部分中将创建一个模拟设备的 Java 控制台应用。 该应用侦听 IoT 中心发出的重启直接方法调用并快速响应该调用。 然后该应用会休眠一段时间，以模拟重启过程，之后该应用会使用报告属性通知 trigger-reboot 后端应用重启已完成  。

1. 使用命令提示符中的以下命令，在 dm-get-started 文件夹中创建名为 simulated-device 的 Maven 项目  。 以下内容是一条很长的命令：

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

2. 在命令提示符下，导航到 simulated-device 文件夹。

3. 使用文本编辑器打开 simulated-device 文件夹中的 pom.xml 文件，并在 dependencies 节点中添加以下依赖项  。 此依赖项使得可以使用应用中的 iot-service-client 包来与 IoT 中心进行通信：

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > 可以使用 [Maven 搜索](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22)检查是否有最新版本的 **iot-device-client**。

4. 在 **dependencies** 节点后添加以下 **build** 节点。 此配置指示 Maven 使用 Java 1.8 来生成应用：

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

5. 保存并关闭 pom.xml 文件。

6. 使用文本编辑器打开 simulated-device\src\main\java\com\mycompany\app\App.java 源文件。

7. 在该文件中添加以下 **import** 语句：

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.time.LocalDateTime;
    import java.util.Scanner;
    import java.util.Set;
    import java.util.HashSet;
    ```

7. 将以下类级变量添加到 **App** 类。 使用“创建设备标识”部分所述的设备连接字符串替换 `{yourdeviceconnectionstring}`  ：

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    ```

8. 若要为直接方法状态事件实现回调处理程序，请将以下嵌套类添加到 App 类  ：

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    ```

9. 若要为设备孪生状态事件实现回调处理程序，请将以下嵌套类添加到 App 类  ：

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
        public void execute(IotHubStatusCode status, Object context)
        {
            System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
    ```

10. 若要为属性事件实现回调处理程序，请将以下嵌套类添加到 App 类  ：

    ```java
    protected static class PropertyCallback implements PropertyCallBack<String, String>
    {
      public void PropertyCall(String propertyKey, String propertyValue, Object context)
      {
        System.out.println("PropertyKey:     " + propertyKey);
        System.out.println("PropertyKvalue:  " + propertyKey);
      }
    }
    ```

11. 若要实现一个模拟设备重启的线程，请将以下嵌套类添加到 App 类  。 线程会休眠五秒，然后设置 lastReboot 报告属性  ：

    ```java
    protected static class RebootDeviceThread implements Runnable {
      public void run() {
        try {
          System.out.println("Rebooting...");
          Thread.sleep(5000);
          Property property = new Property("lastReboot", LocalDateTime.now());
          Set<Property> properties = new HashSet<Property>();
          properties.add(property);
          client.sendReportedProperties(properties);
          System.out.println("Rebooted");
        }
        catch (Exception ex) {
          System.out.println("Exception in reboot thread: " + ex.getMessage());
        }
      }
    }
    ```

12. 若要在设备上实现直接方法，请将以下嵌套类添加到 App 类  。 模拟应用接收到重启直接方法的调用时，会向调用方返回确认，然后启动处理重启的线程  ：

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "reboot" :
          {
            int status = METHOD_SUCCESS;
            System.out.println("Received reboot request");
            deviceMethodData = new DeviceMethodData(status, "Started reboot");
            RebootDeviceThread rebootThread = new RebootDeviceThread();
            Thread t = new Thread(rebootThread);
            t.start();
            break;
          }
          default:
          {
            int status = METHOD_NOT_DEFINED;
            deviceMethodData = new DeviceMethodData(status, "Not defined direct method " + methodName);
          }
        }
        return deviceMethodData;
      }
    }
    ```

13. 修改 main 方法的签名以引发以下异常  ：

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

14. 若要实例化 DeviceClient，请将以下代码添加到 main 方法   ：

    ```java
    System.out.println("Starting device client sample...");
    client = new DeviceClient(connString, protocol);
    ```

15. 若要开始侦听直接方法调用，请将以下代码添加到 main 方法  ：

    ```java
    try
    {
      client.open();
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
      client.startDeviceTwin(new DeviceTwinStatusCallback(), null, new PropertyCallback(), null);
      System.out.println("Subscribed to direct methods and polling for reported properties. Waiting...");
    }
    catch (Exception e)
    {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
      client.close();
      System.out.println("Shutting down...");
    }
    ```

16. 若要关闭设备模拟器，请将以下代码添加到 main 方法  ：

    ```java
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

17. 保存并关闭 simulated-device\src\main\java\com\mycompany\app\App.java 文件。

18. 生成 simulated-device 后端应用并更正任何错误  。 在命令提示符下，导航到 simulated-device 文件夹并运行以下命令：

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>运行应用

现在可以运行应用了。

1. 在命令提示符下，在 simulated-device 文件夹中运行以下命令，开始侦听 IoT 中心发出的重启方法调用：

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![侦听重启直接方法调用的 Java IoT 中心模拟设备应用](./media/iot-hub-java-java-device-management-getstarted/launchsimulator.png)

2. 在命令提示符下，在 trigger-reboot 文件夹中运行以下命令，在模拟设备上从 IoT 中心调用重启方法：

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![调用重启直接方法的 Java IoT 中心服务应用](./media/iot-hub-java-java-device-management-getstarted/triggerreboot.png)

3. 模拟设备对重启直接方法调用做出响应：

    ![Java IoT 中心模拟设备应用对直接方法调用进行响应](./media/iot-hub-java-java-device-management-getstarted/respondtoreboot.png)

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]