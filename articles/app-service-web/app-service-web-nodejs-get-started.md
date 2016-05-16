<properties
	pageTitle="Azure 中的 Node.js Web 应用入门"
	description="学习如何将 Node.js 应用程序部署到 Azure 中的 Web 应用。"
	services="app-service\web"
	documentationCenter="nodejs"
	authors="cephalin"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service-web"
	ms.date="03/31/2016"
	wacn.date=""/>

# Azure 中的 Node.js Web 应用入门

> [AZURE.SELECTOR]
- [.Net](/documentation/articles/web-sites-dotnet-get-started)
- [Node.js](/documentation/articles/app-service-web-nodejs-get-started)
- [Java](/documentation/articles/web-sites-java-get-started)
- [PHP - Git](/documentation/articles/web-sites-php-mysql-deploy-use-git)
- [PHP - FTP](/documentation/articles/web-sites-php-mysql-deploy-use-ftp)
- [Python](/documentation/articles/web-sites-python-ptvs-django-mysql)

本教程说明如何创建一个简单的 [Node.js](http://nodejs.org) 应用程序，然后通过 cmd.exe 或 bash 等命令行将它部署到 [Azure Web 应用](/documentation/services/web-sites)中的 [Web 应用](/home/features/web-site)。本教程中的说明适用于任何能够运行 Node.js 的操作系统。

<a name="prereq"/>
## 先决条件

- Node.js。安装二进制文件可从[此处](https://nodejs.org/)获取。
- Yoeman。[此处](http://yeoman.io/)提供了安装说明。
- Git。安装二进制文件可从[此处](http://www.git-scm.com/downloads)获取。
- Azure CLI。[此处](/documentation/articles/xplat-cli-install)提供了安装说明。
- 一个 Azure 帐户。如果你没有帐户，可以[注册免费试用帐户](/pricing/1rmb-trial/?WT.mc_id=A261C142F)，或者[激活你的 Visual Studio 订户权益](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)。

## 创建并部署简单的 Node.js Web 应用

1. 打开所选的命令行终端以安装[适用于 Yoeman 的 Express 生成器](https://github.com/petecoop/generator-express)。

        npm install -g generator-express

2. 使用 `CD` 切换到工作目录并生成快速应用，如下所示：

        yo express
        
    出现提示时选择以下选项：

    `? Would you like to create a new directory for your project?` **Yes**  
    `? Enter directory name` **&lt;appname>**  
    `? Select a version to install:` **MVC**  
    `? Select a view engine to use:` **Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):` **None**  
    `? Select a database to use:` **None**  
    `? Select a build tool to use:` **Grunt**

3. 使用 `CD` 切换到新应用的根目录，并将它启动以确保它在开发环境中运行：

        npm start

    在浏览器中导航到 [http://localhost:3000](http://localhost:3000) 以确保可以看到 Express 主页。确认应用正常运行后，请使用 `Ctrl-C` 将它停止。
    
1. 如下所示登录到 Azure（为此需要 [Azure CLI](#prereq)）：

        azure login -e AzureChinaCloud -u <your username>

    根据提示，在浏览器中继续使用具有 Azure 订阅的 Microsoft 帐户登录。

2. 确保你仍位于应用的根目录中。在 Azure 中以下一个命令创建具有唯一应用名称的 Azure Web 应用资源。Web 应用的 URL 是 http://&lt;appname>.chinacloudsites.cn。

        azure site create --git <appname>

    根据提示选择要部署到的 Azure 区域。如果你从未设置 Azure 订阅的 Git/FTP 部署凭据，则系统还会提示你创建凭据。

3. 打开 config/config.js 并将生产端口更改为 `process.env.port`。生产 JSON 对象应如下所示：

        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }

    这样，你的 Node.js 应用便可以在 iisnode 侦听的默认端口上响应 Web 请求。
    
4. 保存更改，然后使用 git 将应用部署到 Azure：

        git add .
        git commit -m "<your commit message>"
        git push azure master

    Express 生成器已提供 .gitignore 文件，因此 `git push` 不会占用带宽来尝试上载 node\_modules/ 目录。

5. 最后，只需在浏览器中启动实时 Azure 应用：

        azure site browse

    现在，你应会看到 Node.js Web 应用在 Azure Web 应用中实时运行。
    
    ![](./media/app-service-web-nodejs-get-started/deployed-express-app.png)

## 更新 Node.js Web 应用

若要更新 Azure Web 应用中运行的 Node.js Web 应用，只需和首次部署时一样运行 `git add`、`git commit` 和 `git push`。
     
## Azure 如何部署 Node.js 应用

Azure 使用 [iisnode](https://github.com/tjanczuk/iisnode/wiki) 来运行 Node.js 应用。Azure CLI 和 Kudu 引擎（Git 部署）合作，让你在通过命令行开发和部署 Node.js 应用时获得顺畅的体验。

- `azure site create --git` 可识别 server.js 或 app.js 的常见 Node.js 模式，并在根目录中创建 iisnode.yml。你可以使用此文件来自定义 iisnode。
- 在 `git push azure master` 中，Kudu 可自动完成以下部署任务：

    - 如果 package.json 位于存储库根目录，请运行 `npm install --production`。
    - 在 package.json 中为指向启动脚本的 iisnode 生成 Web.config（例如 server.js 或 app.js）。
    - 自定义 Web.config 以让应用程序准备好使用 Node-Inspector 进行调试。
    
## 使用 Node.js 框架

如果你使用了流行的 Node.js 框架（例如 [Sails.js](http://sailsjs.org/) 或 [MEAN.js](http://meanjs.org/)）来开发应用，则可将这些应用部署到 Azure Web 应用。流行的 Node.js 框架有其特定的行为模式，并且其包依赖性不断更新。但是，Azure 为你提供 stdout 和 stderr 日志，让你确切地了解应用中发生了什么并做出相应的更改。有关详细信息，请参阅[从 iisnode 获取 stdout 和 stderr 日志](#iisnodelog)。

请参阅演示如何在 Azure 中使用特定框架的教程

- [Deploy a Sails.js web app to Azure（将 Sails.js Web 应用部署到 Azure）](/documentation/articles/app-service-web-nodejs-sails)
- [Create a Node.js chat application with Socket.IO in Azure Web App（在 Azure Web 应用使用 Socket.IO 创建 Node.js 聊天应用程序）](/documentation/articles/web-sites-nodejs-chat-app-socketio)
- [How to use io.js with Azure Web Apps（如何将 io.js 与 Azure Web 应用配合使用）](/documentation/articles/web-sites-nodejs-iojs)

## 使用特定的 Node.js 引擎

与平常在 package.json 中所做的一样，你可以在典型工作流中告知 Azure 使用特定的 Node.js 引擎。例如：

    "engines": {
        "node": "5.5.0"
    }, 

Kudu 部署引擎按以下顺序确定要使用哪个 Node.js 引擎：

- 首先，查看 iisnode.yml 以确认是否已指定 `nodeProcessCommandLine`。如果是，则使用它。
- 接下来，查看 package.json 以确认是否已在 `engines` 对象中指定 `"node": "..."`。如果是，则使用它。
- 默认情况下会选择默认的 Node.js 版本。

##<a name="iisnodelog"></a>从 iisnode 获取 stdout 和 stderr 日志

若要阅读 iisnode 记录，请遵循以下步骤：

1. 打开 Azure CLI 提供的 iisnode.yml 文件

2. 设置以下两个参数：

        loggingEnabled: true
        logDirectory: iisnode
    
    这两个参数相结合，告诉 App Service 中的 iisnode 要将其 stdout 和 stderror 输出放在 D:\\home\\site\\wwwroot**iisnode** 目录中。

3. 保存更改，然后使用以下 Git 命令将更改推送到 Azure：

        git add .
        git commit -m "<your commit message>"
        git push azure master
   
   现已配置 iisnode。接下来的步骤演示如何访问这些日志。
     
4. 在浏览器中访问应用的 Kudu 调试控制台，位置为：

        https://<appname>.scm.chinacloudsites.cn/DebugConsole 

5. 导航到 D:\\home\\site\\wwwroot\\iisnode

    ![](./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png)

6. 单击要阅读的日志的“编辑”图标。如果需要，也可以单击“下载”或“删除”。

    ![](./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png)

    现在可以查看日志以帮助调试 Azure 部署。
    
    ![](./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png)


## 使用 Node-Inspector 调试应用

如果你使用 Node-Inspector 来调试 Node.js 应用，可将它用于实时 Azure Web 应用。Node-Inspector 已预先安装在 Azure Web 应用的 iisnode 安装中。如果你通过 Git 部署，则从 Kudu 自动生成的 Web.config 已包含启用 Node-Inspector 所需的所有配置。

若要启用 Node-Inspector，请遵循以下步骤：

1. 打开位于存储库根目录中的 iisnode.yml，并指定以下参数： 

        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll

3. 保存更改，然后使用以下 Git 命令将更改推送到 Azure：

        git add .
        git commit -m "<your commit message>"
        git push azure master
   
4. 现在，只需在 URL 中添加 /debug 以导航到 package.json 中的启动脚本指定的应用启动文件。例如，

        http://<appname>.chinacloudsites.cn/server.js/debug
    
    或者，
    
        http://<appname>.chinacloudsites.cn/app.js/debug

## 更多资源

- [在 Azure 应用程序中指定 Node.js 版本](/documentation/articles/nodejs-specify-node-version-azure-apps)
- [如何在 Azure 中调试 Node.js Web 应用](/documentation/articles/web-sites-nodejs-debug)
- [将 Node.js 模块与 Azure 应用程序一起使用](/documentation/articles/nodejs-use-node-modules-azure-apps)
- [Azure Web Apps: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
- [Node.js 开发人员中心](/develop/nodejs/)
- [Azure 中的 Web 应用入门](/documentation/articles/app-service-web-get-started)

<!---HONumber=Mooncake_0509_2016-->