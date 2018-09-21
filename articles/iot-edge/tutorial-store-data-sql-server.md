---
title: 使用 Azure IoT Edge SQL 模块存储数据 | Microsoft Docs
description: 了解如何使用 SQL Server 模块将数据存储到本地的 IoT Edge 设备
services: iot-edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 08/30/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 19d2472b526c38880f9241ec448f8a9d4a327f2a
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294180"
---
# <a name="tutorial-store-data-at-the-edge-with-sql-server-databases"></a>教程：使用 SQL Server 数据库在边缘存储数据

使用 Azure IoT Edge 和 SQL Server 在边缘存储和查询数据。 Azure IoT Edge 内置了基本的存储功能，可以在设备脱机的情况下缓存消息，在重新建立连接后再转发这些消息。 不过，你可能需要更高级的存储功能，例如在本地查询数据的功能。 整合本地数据库以后，IoT Edge 设备就可以执行更复杂的计算而不需维持到 IoT 中心的连接。 例如，现场技术员可以在本地计算机上将过去几天的传感器数据可视化，即使该数据每月只上传到云中一次（目的是改进机器学习模型）。

本文提供有关将 SQL Server 数据库部署到 IoT Edge 设备的说明。 在 IoT Edge 设备上运行的 Azure Functions 可将传入的数据结构化，然后将其发送到数据库。 本文中的步骤也适用于在容器中运行的其他数据库，例如 MySQL 或 PostgreSQL。

本教程介绍如何执行下列操作： 

> [!div class="checklist"]
> * 使用 Visual Studio Code 创建 Azure Functions
> * 将 SQL 数据库部署到 IoT Edge 设备
> * 使用 Visual Studio Code 生成模块并将其部署到 IoT Edge 设备
> * 查看生成的数据

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>先决条件

Azure IoT Edge 设备：

* 可以按照适用于 [Linux](quickstart-linux.md) 或 [Windows 设备](quickstart.md)的快速入门中的步骤，将开发计算机或虚拟机用作 Edge 设备。

云资源：

* Azure 中的标准层 [IoT 中心](../iot-hub/iot-hub-create-through-portal.md)。 

开发资源：

* [Visual Studio Code](https://code.visualstudio.com/)。 
* [适用于 Visual Studio Code 的 C#（由 OmniSharp 提供支持）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)扩展。 
* 适用于 Visual Studio Code 的 [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 扩展。 
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download)。 
* [Docker CE](https://docs.docker.com/install/)。 

## <a name="create-a-container-registry"></a>创建容器注册表
本教程将使用适用于 VS Code 的 Azure IoT Edge 扩展来生成模块并从文件创建**容器映像**。 然后将该映像推送到用于存储和管理映像的**注册表**。 最后，从注册表部署在 IoT Edge 设备上运行的映像。  

在此教程中，可以使用任意兼容 Docker 的注册表。 可以在云中使用的两个常见 Docker 注册表服务分别是 [Azure 容器注册表](https://docs.microsoft.com/azure/container-registry/)和 [Docker 中心](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)。 本教程使用 Azure 容器注册表。 

1. 在 [Azure 门户](https://portal.azure.com)中，选择“创建资源” > “容器” > “容器注册表”。

    ![创建容器注册表](./media/tutorial-deploy-function/create-container-registry.png)

2. 为注册表命名，然后选择一个订阅。
3. 至于资源组，建议使用包含 IoT 中心的资源组名称。 将所有资源置于同一组中可以对其集中管理。 例如，删除用于测试的资源组会删除包含在组中的所有测试资源。 
4. 将 SKU 设置为“基本”，并将“管理员用户”切换到“启用”。 
5. 单击“创建”。
6. 创建容器注册表以后，导航到其中，然后选择“访问键”。 
7. 复制“登录服务器”、“用户名”和“密码”的值。 本教程后面会用到这些值。 

## <a name="create-a-function-project"></a>创建函数项目

若要将数据发送到数据库中，需要通过一个模块将数据进行适当的结构化并存储在表中。 

以下步骤将介绍如何使用 Visual Studio Code 和 Azure IoT Edge 扩展来创建 IoT Edge 函数。

1. 打开 Visual Studio Code。
2. 打开 VS Code 集成终端，方法是选择“视图” > “终端”。
3. 打开 VS Code 命令面板，方法是选择“视图” > “命令面板”。
4. 在命令面板中，键入并运行“Azure: 登录”命令，然后按说明登录 Azure 帐户。 如果已登录，则可跳过此步骤。
3. 在命令面板中，键入并运行“Azure IoT Edge: 新建 IoT Edge 解决方案”命令。 在命令面板中提供以下信息，以便创建解决方案： 
   1. 选择要在其中创建解决方案的文件夹。 
   2. 提供解决方案的名称，或者接受默认的 **EdgeSolution**。
   3. 选择“Azure Functions - C#”作为模块模板。 
   4. 将模块命名为 **sqlFunction**。 
   5. 将在上一部分创建的 Azure 容器注册表指定为第一个模块的映像存储库。 将 **localhost:5000** 替换为复制的登录服务器值。 最终的字符串看起来类似于 **\<注册表名称\>.azurecr.io/sqlFunction**。

4. VS Code 窗口将加载你的 IoT Edge 解决方案空间。 有一个 **modules** 文件夹、一个 **.vscode** 文件夹，以及一个部署清单模板文件。 打开 **modules** > **sqlFunction** > **EdgeHubTrigger-Csharp** > **run.csx**。

5. 将此文件的内容替换为以下代码：

   ```csharp
   #r "Microsoft.Azure.Devices.Client"
   #r "Newtonsoft.Json"
   #r "System.Data.SqlClient"

   using System.IO;
   using Microsoft.Azure.Devices.Client;
   using Newtonsoft.Json;
   using Sql = System.Data.SqlClient;
   using System.Threading.Tasks;

   // Filter messages based on the temperature value in the body of the message and the temperature threshold value.
   public static async Task Run(Message messageReceived, IAsyncCollector<Message> output, TraceWriter log)
   {
       const int temperatureThreshold = 25;
       byte[] messageBytes = messageReceived.GetBytes();
       var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

       if (!string.IsNullOrEmpty(messageString))
       {
           // Get the body of the message and deserialize it
           var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

           //Store the data in SQL db
           const string str = "<sql connection string>";
           using (Sql.SqlConnection conn = new Sql.SqlConnection(str))
           {
           conn.Open();
           var insertMachineTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'machine', " + messageBody.machine.temperature + ");";
           var insertAmbientTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'ambient', " + messageBody.ambient.temperature + ");"; 
               using (Sql.SqlCommand cmd = new Sql.SqlCommand(insertMachineTemperature + "\n" + insertAmbientTemperature, conn))
               {
               //Execute the command and log the # rows affected.
               var rows = await cmd.ExecuteNonQueryAsync();
               log.Info($"{rows} rows were updated");
               }
           }

           if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
           {
               // Send the message to the output as the temperature value is greater than the threshold
               var filteredMessage = new Message(messageBytes);
               // Copy the properties of the original message into the new Message object
               foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
               {
                   filteredMessage.Properties.Add(prop.Key, prop.Value);
               }
               // Add a new property to the message to indicate it is an alert
               filteredMessage.Properties.Add("MessageType", "Alert");
               // Send the message        
               await output.AddAsync(filteredMessage);
               log.Info("Received and transferred a message with temperature above the threshold");
           }
       }
   }

   //Define the expected schema for the body of incoming messages
   class MessageBody
   {
       public Machine machine {get;set;}
       public Ambient ambient {get; set;}
       public string timeCreated {get; set;}
   }
   class Machine
   {
      public double temperature {get; set;}
      public double pressure {get; set;}         
   }
   class Ambient
   {
      public double temperature {get; set;}
      public int humidity {get; set;}         
   }
   ```

6. 在第 24 行中，将 **\<sql connection string\>** 字符串替换为以下字符串。 **Data Source** 属性引用 SQL Server 容器名称。将在下一部分使用名称“SQL”来创建该名称。 

   ```C#
   Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;
   ```

7. 保存 **run.csx** 文件。 

## <a name="add-a-sql-server-container"></a>添加 SQL Server 容器

[部署清单](module-composition.md)声明将要由 IoT Edge 运行时安装在 IoT Edge 设备上的具体模块。 已在上一部分添加了生成自定义 Functions 模块所需的代码，但 SQL Server 模块已经生成。 只需要求 IoT Edge 运行时包括它，然后在设备上配置它即可。 

1. 在 Visual Studio Code 资源管理器中打开 **deployment.template.json** 文件。 
2. 找到 **moduleContent.$edgeAgent.properties.desired.modules** 节。 应该会列出两个模块：**tempSensor** 模块，用于生成模拟数据，以及 **sqlFunction** 模块。
3. 如果使用了 Windows 容器，请修改 **sqlFunction.settings.image** 部分。
    ```json
    "image": "${MODULES.sqlFunction.windows-amd64}"
    ```
4. 添加以下代码来声明第三个模块。 在 sqlFunction 部分后添加一个逗号并插入以下内容：

   ```json
   "sql": {
       "version": "1.0",
       "type": "docker",
       "status": "running",
       "restartPolicy": "always",
       "settings": {
           "image": "",
           "createOptions": ""
       }
   }
   ```

   如果在添加 JSON 元素时有任何困惑，下面提供了一个示例。 ![添加 SQL Server 容器](./media/tutorial-store-data-sql-server/view_json_sql.png)

5. 根据 IoT Edge 设备的 Docker 容器的类型，使用以下代码更新 **sql.settings** 参数：

   * Windows 容器：

      ```json
      "image": "microsoft/mssql-server-windows-developer",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"C:\\\\mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

   * Linux 容器：

      ```json
      "image": "microsoft/mssql-server-linux:2017-latest",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"MSSQL_SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"/var/opt/mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

   >[!Tip]
   >每当在生产环境中创建 SQL Server 容器时，都应该[更改默认的系统管理员密码](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker#change-the-sa-password)。

6. 保存 **deployment.template.json** 文件。

## <a name="build-your-iot-edge-solution"></a>生成 IoT Edge 解决方案

在前面部分，你创建了一个包含一个模块的解决方案，然后向部署清单模板添加了另一个。 现在需生成该解决方案，创建用于模块的容器映像，然后将映像推送到容器注册表。 

1. 在 .env 文件中，为 IoT Edge 运行时提供注册表凭据，使之能够访问模块映像。 找到 **CONTAINER_REGISTRY_USERNAME** 和 **CONTAINER_REGISTRY_PASSWORD** 部分并在等号后插入你的凭据： 

   ```env
   CONTAINER_REGISTRY_USERNAME_yourContainerReg=<username>
   CONTAINER_REGISTRY_PASSWORD_yourContainerReg=<password>
   ```

2. 保存 .env 文件。
3. 在 Visual Studio Code 中登录到你的容器注册表，以便将映像推送到你的注册表。 使用你添加到 .env 文件中的凭据。 在集成终端中输入以下命令：

    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```
    系统会提示输入密码。 将密码粘贴到提示符处，然后按 **Enter**。

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

4. 在 VS Code 资源管理器中右键单击“deployment.template.json”文件，然后选择“生成并推送 IoT Edge 解决方案”。 

## <a name="deploy-the-solution-to-a-device"></a>将解决方案部署到设备

可以通过 IoT 中心设置设备上的模块，但是也可以通过 Visual Studio Code 访问 IoT 中心和设备。 在此部分，请先设置对 IoT 中心的访问权限，然后使用 VS Code 将解决方案部署到 IoT Edge 设备。 

1. 在 VS Code 命令面板中，选择“Azure IoT 中心: 选择 IoT 中心”。
2. 根据提示登录到 Azure 帐户。 
3. 在命令面板中选择 Azure 订阅，然后选择 IoT 中心。 
4. 在 VS Code 资源管理器中，展开“Azure IoT 中心设备”部分。 
5. 右键单击要将其作为部署目标的设备，然后选择“为单个设备创建部署”。 
6. 在文件资源管理器中导航到解决方案中的 **config** 文件夹，然后选择 **deployment.json**。 单击“选择 Edge 部署清单”。 

如果部署成功，则会在 VS Code 输出中列显确认消息。 也可查看设备上的所有模块是否都已启动并运行。 

在 IoT Edge 设备上运行以下命令，以便查看模块的状态。 可能需要几分钟时间。

   ```PowerShell
   iotedge list
   ```

## <a name="create-the-sql-database"></a>创建 SQL 数据库

对设备应用部署清单时，会运行三个模块。 tempSensor 模块生成模拟环境数据。 sqlFunction 模块会提取数据并针对数据库设置其格式。 

本部分介绍如何设置用于存储温度数据的 SQL 数据库。 

1. 在命令行工具中连接到数据库。 
   * Windows 容器：
   
      ```cmd
      docker exec -it sql cmd
      ```
    
   * Linux 容器： 

      ```bash
      docker exec -it sql bash
      ```

2. 打开 SQL 命令工具。
   * Windows 容器：

      ```cmd
      sqlcmd -S localhost -U SA -P "Strong!Passw0rd"
      ```

   * Linux 容器： 

      ```bash
      /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
      ```

3. 创建数据库： 

   * Windows 容器
      ```sql
      CREATE DATABASE MeasurementsDB
      ON
      (NAME = MeasurementsDB, FILENAME = 'C:\mssql\measurementsdb.mdf')
      GO
      ```

   * Linux 容器
      ```sql
      CREATE DATABASE MeasurementsDB
      ON
      (NAME = MeasurementsDB, FILENAME = '/var/opt/mssql/measurementsdb.mdf')
      GO
      ```

4. 定义表。

   ```sql
   CREATE TABLE MeasurementsDB.dbo.TemperatureMeasurements (measurementTime DATETIME2, location NVARCHAR(50), temperature FLOAT)
   GO
   ```

可以自定义 SQL Server docker 文件，以自动设置要在多个 IoT Edge 设备上部署的 SQL Server。 有关详细信息，请参阅 [Microsoft SQL Server 容器演示项目](https://github.com/twright-msft/mssql-node-docker-demo-app)。 

## <a name="view-the-local-data"></a>查看本地数据

表创建好以后，sqlFunction 模块就会开始在 IoT Edge 设备的本地 SQL Server 2017 数据库中存储数据。 

在 SQL 命令工具中运行以下命令，以便查看格式化的表数据： 

   ```sql
   SELECT * FROM MeasurementsDB.dbo.TemperatureMeasurements
   GO
   ```

   ![查看本地数据](./media/tutorial-store-data-sql-server/view-data.png)



## <a name="clean-up-resources"></a>清理资源

如果打算继续学习下一篇建议的文章，可以保留已创建的资源和配置，以便重复使用。 还可以继续使用相同的 IoT Edge 设备作为测试设备。 

否则，可以删除本文中创建的本地配置和 Azure 资源，以避免收费。 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]



## <a name="next-steps"></a>后续步骤

本教程介绍了如何创建 Azure Functions 模块，其中包含用于筛选 IoT Edge 设备生成的原始数据的代码。 做好生成自己的模块的准备以后，即可详细了解如何[使用用于 Visual Studio Code 的 Azure IoT Edge 开发 Azure Functions](how-to-develop-csharp-function.md)。 

继续阅读后续教程，了解如何使用 Azure IoT Edge 通过其他方式将数据转化为边缘业务见解。

> [!div class="nextstepaction"]
> [在 Azure 流分析中使用浮点窗口查找平均值](tutorial-deploy-stream-analytics.md)
