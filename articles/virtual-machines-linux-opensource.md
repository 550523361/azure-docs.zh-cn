﻿<properties
  pageTitle="Azure 上的 Linux 和开源计算"
  description="本主题包含 Azure 上的 Linux 和开源计算的列表，包括基本的 Linux 用法、一些关于在 Azure 上运行或上载 Linux 映像的基础知识，以及关于特定技术或优化的其他内容。"
  documentationCenter=""
  authors="squillace"
  manager="timlt"
  editor="tysonn"/>

<!--The next line, with one pound sign at the beginning, is the page title-->
# Azure 上的 Linux 和开源计算

本文档尝试在一个位置列出由 Microsoft 及其合作伙伴编写的关于在 Microsoft Azure 上运行基于 Linux 的虚拟机以及其他开源计算环境和应用程序的所有主题。因为 Azure 和开源计算都是快速发展的目标，所以几乎可以肯定地说，本文档已过时了， *尽管*我们将尽最大努力来持续添加更新的主题并删除过时的主题。

## 一般说明
在本页的右侧对各个部分进行了拆分。（链接可能会出现在多个部分中，因为主题可能是关于多个概念、发行版或技术的）。此外，有多个主题介绍了各种 Linux 选项、映像存储库、案例研究以及关于如何上载你自己的自定义映像的操作指南主题： 

- [MSOpenTech VM Depot](https://vmdepot.msopentech.com/List/Index)
- [事件和演示：Microsoft Openness CEE](http://www.opennessatcee.com/)
- [操作指南：上载自己的发行版映像](/documentation/articles/virtual-machines-linux-create-upload-vhd/) （并且还包含使用[经 Azure 认可的分发的说明](/documentation/articles/virtual-machines-linux-endorsed-distributions/))
- [说明：在 Azure 中运行时的一般 Linux 要求](/documentation/articles/virtual-machines-linux-create-upload-vhd-generic/)
- [说明：Azure 上的 Linux 的一般简介](/documentation/articles/virtual-machines-linux-introduction/)

<!--
- [Distros](#distros) &mdash; Topics to do with a specific distro.
- [The Basics](#basics) &mdash; A lot of the basic things to do that you either know or need to know.
- [Community Images and Repositories](#images) &mdash; Other places for very useful information, repositories, and binaries.
- [Languages and Platforms](#langsandplats)
- [Samples and Scripts](#samples)
- [Auth and Encryption](#security) &mdash; Important security-related topics, not necessarily specific to Azure.
- [Devops, Management, and Optimization](#devops) &mdash; A big category, changing rapidly.
- [Support, Troubleshooting, and "It Just Doesn't Work"](#supportdebug) &mdash; Really.
-->

## 发行版

Linux 有大量的分发，通常按包管理系统进行划分：有些是基于 dpgk 的，例如 Debian 和 Ubuntu；另一些是基于 rpm 的，例如 CentOS、SUSE 和 RedHat。有些公司作为 Microsoft 的正式合作伙伴也提供了发行版映像并且是经认可的。其他一些是由社区提供的。本部分中的发行版具有关于它们的正式文章，即使它们只是在其他技术的示例中使用也是如此。

### Ubuntu

Ubuntu 是一种非常流行的经 Azure 认可的基于 dkpg 和 apt-get 包管理系统的 Linux 分发。

1. [操作指南：上载自己的 Ubuntu 映像](/documentation/articles/virtual-machines-linux-create-upload-vhd-ubuntu/)
2. [操作指南：Ubuntu LAMP 堆栈](/documentation/articles/virtual-machines-linux-install-lamp-stack/)
3. [操作指南：MySQL 群集](/documentation/articles/virtual-machines-linux-mysql-cluster/)
4. [操作指南：Node.js 和 Cassandra](/documentation/articles/virtual-machines-linux-nodejs-running-cassandra/)
5. [操作指南：IPython Notebook](/documentation/articles/virtual-machines-python-ipython-notebook/)
6. [钻研：使用 Docker 容器在 Linux 上运行 ASP.NET 5](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)

### [Debian](https://vmdepot.msopentech.com/List/Index?sort=Featured&search=Debian)

对于 Linux 和基于 dpgk 和 apt-get 包管理系统的开源世界来说，Debian 是一个重要的分发。MSOpenTech VM Depot 有多个可使用的映像。

### CentOS

CentOS Linux 分发是从 Red Hat Enterprise Linux (RHEL) 的源代码派生的一个稳定的、可预测、可管理且可重现的平台。

1. [MSOpenTech VM Depot](https://vmdepot.msopentech.com/List/Index?sort=Featured&search=centos)
3. [操作指南：为 Azure 准备自定义的基于 CentOS 的 VM](/documentation/articles/virtual-machines-linux-create-upload-vhd-centos/)
4. [博客：如何从 OpenLogic 部署 CentOS VM 映像](http://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
6. [操作指南：为 AMQP 和 Service Bus 安装 Apache Qpid Proton-C](http://msdn.microsoft.com/zh-cn/library/azure/dn235560.aspx)

### SUSE Enterprise Linux 和 OpenSUSE

9. [MSOpenTech VM Depot](https://vmdepot.msopentech.com/List/Index?sort=Featured&search=OpenSUSE)
11. [操作指南：安装和运行 MySQL](/documentation/articles/virtual-machines-linux-mysql-use-opensuse/)
12. [操作指南：准备自定义 SLES 或 openSUSE VM](/documentation/articles/virtual-machines-linux-create-upload-vhd-suse/)  
13. [SUSE 论坛](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)操作指南：移动到新的修补程序服务器

### CoreOS

CoreOS 是一个小型的经优化的发行版，适用于具有高度的自定义控制的纯计算规模。

11. [操作指南：在 Azure 上使用 CoreOS](/documentation/articles/virtual-machines-linux-coreos-how-to/)
12. [博客：TechEd Europe -- Windows Docker 客户端和 Linux 容器](http://azure.microsoft.com/blog/2014/10/28/new-docker-coreos-topics-linux-on-azure/)
13. [博客：Azure 正在变得更大、更快、更加开放](http://azure.microsoft.com/blog/2014/10/20/azures-getting-bigger-faster-and-more-open/)
14. [Github：在 Azure 上部署 CoreOS 快速入门](https://github.com/timfpark/coreos-azure)
15. [Github：使用 Spring Boot、MongoDB 和 CoreOS 部署 Java 应用程序](https://github.com/chanezon/azure-linux/tree/master/coreos/cloud-init)

#### [Oracle Linux](http://azure.microsoft.com/marketplace/?term=Oracle+Linux)
  2. [为 Azure 准备 Oracle Linux 虚拟机](/documentation/articles/virtual-machines-linux-create-upload-vhd-oracle/)

### FreeBSD

12. [MSOpenTech VM Depot](https://vmdepot.msopentech.com/List/Index?sort=Date&search=FreeBSD)
13. [博客：在 Azure 中运行 FreeBSD](http://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
14. [博客：轻松部署 FreeBSD](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
15. [博客：部署自定义的 FreeBSD 映像](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
17. [操作指南：安装 Azure Linux 代理](/documentation/articles/virtual-machines-linux-agent-user-guide/)

## 基础知识

1. [基础知识：Azure 命令行界面 (cli)](/documentation/articles/xplat-cli/)
4. [基础知识：证书的使用和管理](http://msdn.microsoft.com/zh-cn/library/azure/gg981929.aspx)
5. [基础知识：选择 Linux 用户名](/documentation/articles/virtual-machines-linux-usernames/)
6. [基础知识：使用 Azure 门户登录到 Linux VM](/documentation/articles/virtual-machines-linux-how-to-log-on/)
7. [基础知识：SSH](/documentation/articles/virtual-machines-linux-use-ssh-key/)
8. [基础知识：如何为 Linux 重置密码或 SSH 属性](/documentation/articles/virtual-machines-linux-use-vmaccess-reset-password-or-ssh/)
9. [基础知识：使用 Root](/documentation/articles/virtual-machines-linux-use-root-privileges/)
10. [基础知识：将数据磁盘附加到 Linux VM](/documentation/articles/virtual-machines-linux-how-to-attach-disk/)
11. [基础知识：从 Linux VM 分离数据磁盘](/documentation/articles/virtual-machines-linux-how-to-detach-disk/)
12. [博客中的基础知识：优化使用 Linux 和 Azure 时的存储、磁盘和性能](http://blogs.msdn.com/b/igorpag/archive/2014/10/23/azure-storage-secrets-and-linux-i-o-optimizations.aspx)
13. [基础知识：RAID](/documentation/articles/virtual-machines-linux-configure-raid/)
14. [基础知识：捕获 Linux VM 来创建模板](/documentation/articles/virtual-machines-linux-capture-image/)
15. [基础知识：Azure Linux 代理](/documentation/articles/virtual-machines-linux-agent-user-guide/)
16. [基础知识：Azure VM 扩展和功能](http://msdn.microsoft.com/zh-cn/library/azure/dn606311.aspx)
17. [基础知识：将自定义数据注入到 VM 中以用于 Cloud-init](/documentation/articles/virtual-machines-how-to-inject-custom-data/)
18. [博客中的基础知识：通过 12 个步骤在 Azure 上构建具有高可用性的 Linux](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
19. [博客中的基础知识：使用 xplat、node.js、jhawk 在 Azure 上自动设置 Linux](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
19. [使用 Azure x-plat cli 创建多 VM 部署](/documentation/articles/virtual-machines-create-multi-vm-deployment-xplat-cli/)
20. [基础知识：Azure Docker VM 扩展](/documentation/articles/virtual-machines-docker-vm-extension/)
23. [Azure 服务管理 REST API](https://msdn.microsoft.com/zh-cn/library/azure/ee460799.aspx) 参考
24. [Azure 上的 GlusterFS](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

## 社区映像和存储库
3. [MSOpenTech VM Depot](https://vmdepot.msopentech.com/List/Index) &mdash; 适用于社区提供的虚拟机映像。
4. [Github](https://github.com/Azure/) &mdash; 适用于 xplat-cli 以及许多其他工具和项目。
5. [Docker Hub 注册表](https://registry.hub.docker.com/) &mdash; Docker 容器映像的注册表。

## 语言和平台
### [Azure Java 开发人员中心](/develop/java/)

1. [映像](https://vmdepot.msopentech.com/List/Index?sort=Featured&search=java)
2. [操作指南：使用 AMQP 1.0 通过 Java 使用 Service Bus](http://msdn.microsoft.com/zh-cn/library/azure/jj841073.aspx)
3. [操作指南：使用 Azure 门户在 Linux 上设置 Tomcat7](/documentation/articles/virtual-machines-linux-setup-tomcat7-linux/)
5. [博客：适用于 Java 的 Azure 管理库入门](http://azure.microsoft.com/blog/2014/09/15/getting-started-with-the-azure-java-management-libraries/)
5. [Github 存储库：适用于 Eclipse with Java 的 Azure 工具包](https://github.com/MSOpenTech/WindowsAzureToolkitForEclipseWithJava)
6. [参考：适用于 Eclipse with Java 的 Azure 工具](http://msdn.microsoft.com/zh-cn/library/azure/hh694271.aspx)
7. [Github 存储库：适用于 IntelliJ IDEA 和 Android Studio 的 MS Open Tech Tools 插件](https://github.com/MSOpenTech/msopentech-tools-for-intellij)
7. [博客：MSOpenTech 对 OpenJDK 做出了贡献](http://msopentech.com/blog/2014/10/21/ms-open-techs-first-contribution-openjdk/)

### JVM 语言：

1. [Scala：在 Azure 云服务中运行 Play Framework 应用程序](http://msopentech.com/blog/2014/09/25/tutorial-running-play-framework-applications-microsoft-azure-cloud-services-2/)

### SDK 类型、安装和升级
4. [Azure 服务管理 SDK：Java](http://dl.windowsazure.com/javadoc/)
5. [Azure 服务管理 SDK：Go](https://github.com/MSOpenTech/azure-sdk-for-go)
5. [Azure 服务管理 SDK：Ruby](https://github.com/MSOpenTech/azure-sdk-for-ruby)
    - [操作指南：在 Rails 上安装 Ruby](/documentation/articles/virtual-machines-ruby-rails-web-app-linux/)
    - [操作指南：使用 Capistrano、Nginx、Unicorn 和 PostgreSQL 在 Rails 上安装 Ruby](/documentation/articles/virtual-machines-ruby-deploy-capistrano-host-nginx-unicorn/)
6. [Azure 服务管理 SDK：Python](https://github.com/Azure/azure-sdk-for-python)
    - [操作指南：Django Hello World Web 应用程序 (Mac-Linux)](/documentation/articles/virtual-machines-python-django-web-app-linux/)
7. [Azure 服务管理 SDK：Node.js](https://github.com/MSOpenTech/azure-sdk-for-node)
8. [Azure 服务管理 SDK：PHP](https://github.com/MSOpenTech/azure-sdk-for-php)
    - [操作指南：在 Azure VM 上安装 LAMP 堆栈](/documentation/articles/virtual-machines-linux-install-lamp-stack/)
9. [Azure 服务管理 SDK：.NET](https://github.com/Azure/azure-sdk-for-net)
10. [博客：Mono、ASP.NET 5、Linux 和 Docker](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)

## 示例和脚本

可查找此部分来快速填写。如果你有任何建议，请向我们发送 PR 或者在下文中的意见部分留言。

1. [使用 Azure x-plat cli 创建多 VM 部署](/documentation/articles/virtual-machines-create-multi-vm-deployment-xplat-cli/)
2. [Patrick Chanezon 的 Azure Linux Github 存储库](https://github.com/chanezon/azure-linux)

## 数据

本部分包含几种不同的存储方法和技术（包括非 SQL、关系型和大数据）的相关信息。

### 非 SQL

1. [博客：8 适用于 Azure 的开源非 SQL 数据库](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
2. Couchdb
    - [Slideshare (MSOpenTech)：在 Azure 上体验 CouchDb](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
    - [博客：使用 node.js、CORS 和 Grunt 运行 CouchDB 即服务](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)
3. MongoDB
    - [操作指南：使用 MongoLab 外接程序通过 MongoDB 在 Azure 上创建 Node.js 应用程序](/documentation/articles/store-mongolab-web-sites-nodejs-store-data-mongodb/)
4. Cassandra
    - [操作指南：在 Azure 上将 Cassandra 与 Linux 一起运行以及通过 Node.js 对其进行访问](/documentation/articles/virtual-machines-linux-nodejs-running-cassandra/)
5. Redis
    - [博客：Azure Redis 缓存服务中 Windows 上的 Redis](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
    - [博客：宣布推出适用于 Redis 预览版的 ASP.NET 会话状态提供程序](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)
6. RavenHQ
    - [博客：RavenHQ 目前在 Azure Marketplace 中可用了](http://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### 大数据
2. Hadoop/Cloudera  
	- [博客：在 Azure Linux VM 上安装 Hadoop](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
	- [操作指南：通过 HDInsight 使用 Hadoop 和 Hive 入门](/documentation/articles/hdinsight-get-started/)  
3. [Azure HDInsight](/documentation/services/hdinsight/) -- Azure 上的一项完全托管的 Hadoop 服务。

### 关系型数据
2. MySQL
    - [操作指南：安装和运行 MySQL](/documentation/articles/virtual-machines-linux-mysql-use-opensuse/)
    - [操作指南：优化 Azure 上的 MySQL 的性能](/documentation/articles/virtual-machines-linux-optimize-mysql-perf/)
    - [操作指南：MySQL 群集](/documentation/articles/virtual-machines-linux-mysql-cluster/)
    - [操作指南：使用 Marketplace 创建 MySQL 数据库](/documentation/articles/store-php-create-mysql-database/)
    - [操作指南：具有 Python 和 Visual Studio 的 Azure 网站上的 Django 和 MySQL](/documentation/articles/web-sites-python-ptvs-django-mysql/)
    - [如何：具有 WebMatrix 的 Azure 网站上的 PHP 和 MySQL](/documentation/articles/web-sites-php-mysql-use-webmatrix/)
7. MariaDB
    - [操作指南：创建多主的 MariaDb 群集](/documentation/articles/virtual-machines-mariadb-cluster/)
7. PostgreSQL
    - [操作指南：使用 Capistrano、Nginx、Unicorn 和 PostgreSQL 在 Rails 上安装 Ruby](/documentation/articles/virtual-machines-ruby-deploy-capistrano-host-nginx-unicorn/)
8. [使用 ILB 通过 corosync、pg_bouncer 安装 Postgres](https://github.com/chgeuer/postgres-azure)


## <a id='security'>身份验证和加密</a>

身份验证和加密是软件开发中的重要主题，并且网上有很多很多主题介绍了如何针对这两者使用合适的安全技术。我们将介绍一些基本的用法来快速启动和运行 Linux 和开源工作负荷，并指出用来重置或删除 Azure 上的远程安全功能的工具。这些是基本过程，我们很快将添加更复杂的方案。 

4. [基础知识：证书的使用和管理](http://msdn.microsoft.com/zh-cn/library/azure/gg981929.aspx)
7. [基础知识：SSH](/documentation/articles/virtual-machines-linux-use-ssh-key/)
8. [基础知识：如何为 Linux 重置密码或 SSH 属性](/documentation/articles/virtual-machines-linux-use-vmaccess-reset-password-or-ssh/)
9. [基础知识：使用 Root](/documentation/articles/virtual-machines-linux-use-root-privileges/)

## 开发运营、管理和优化

1. Docker
	- [适用于 Azure 上 的 Linux 的 Docker VM 扩展](/documentation/articles/virtual-machines-docker-vm-extension/)
	- [从 Azure 跨平台命令行界面 (xplat-cli) 使用 Docker VM 扩展](/documentation/articles/virtual-machines-docker-with-xplat-cli/)
	- [从 Azure 预览版门户使用 Docker VM 扩展](/documentation/articles/virtual-machines-docker-with-portal/)
	- [在 Azure Marketplace 中使用 Docker 快速入门](/documentation/articles/virtual-machines-docker-ubuntu-quickstart/)
	- [如何在 Azure 上使用 docker-machine]
	- [如何在 Azure 上将 docker 与 swarm 一起使用]
	
2. [Fleet with CoreOS](/documentation/articles/virtual-machines-linux-coreos-how-to/)
3. Deis
	- [GitHub 存储库：
在 Azure 中的 CoreOS 群集上安装 Deis](https://github.com/chanezon/azure-linux/tree/master/coreos/deis)
4. Kubernetes
	- [Kubernetes 可视化工具](http://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure)
5. Jenkins 和 Hudson
	- [博客：适用于 Azure 的 Jenkins Slave 插件](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
	- [GitHub 存储库：适用于 Azure 的 Jenkins Storage 插件](https://github.com/jenkinsci/windows-azure-storage-plugin)
	- [第三方：适用于 Azure 的 Hudson Slave 插件](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
	- [第三方：适用于 Azure 的 Hudson Storage 插件](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
10. Chef
	- [Chef 和虚拟机](/documentation/articles/virtual-machines-windows-install-chef-client/)

13. Powershell DSC for Linux
    - [博客：如何执行 Powershell DSC for Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
    - [Github：Docker 客户端 DSC](https://github.com/anweiss/DockerClientDSC)
13. [Ubuntu Juju](https://juju.ubuntu.com/docs/config-azure.html)
14. [适用于 Azure 的打包程序插件](https://github.com/msopentech/packer-azure)

<!--Anchors-->
[发行版]: #distros
[基础知识]: #basics
[社区映像和存储库]: #images
[语言和平台]: #langsandplats
[示例和脚本]: #samples
[身份验证和加密]: #security
[开发运营、管理和优化]: #devops
[支持、故障排除以及"它就是不工作"]: #supportdebug

<!--Link references--In actual articles, you only need a single period before the slash.-->
[如何在 Azure 上使用 docker-machine]: /documentation/articles/virtual-machines-docker-machine/
[如何在 Azure 上将 docker 与 swarm 一起使用]: /documentation/articles/virtual-machines-docker-swarm/

<!--HONumber=50-->