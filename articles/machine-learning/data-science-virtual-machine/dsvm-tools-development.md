---
title: 数据科学虚拟机数据开发工具 - Azure | Microsoft Docs
description: 了解 Data Science Virtual Machine 上预安装的工具和集成开发环境。
keywords: 数据科学工具, 数据科学虚拟机, 数据科学工具, Linux 数据科学
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: gokuma
ms.openlocfilehash: dd60c5d0210ffba373839fd0f194496c5dbcc20d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/18/2019
ms.locfileid: "57999407"
---
# <a name="development-tools-on-the-data-science-virtual-machine"></a>数据科学虚拟机开发工具

数据科学虚拟机 (DSVM) 通过捆绑多种常用工具和 IDE 提供高效的开发环境。 以下是 DSVM 中提供的一些工具。 

## <a name="visual-studio-2017"></a>Visual Studio 2017  

|    |           |
| ------------- | ------------- |
| 它是什么？   | 常规用途 IDE      |
| 支持的 DSVM 版本      | Windows      |
| 典型用途      | 软件开发    |
| 如何在 DSVM 上配置/安装它？      | 数据科学工作负荷（Python 和 R 工具）、Azure 工作负荷（Hadoop、Data Lake）、Node.js、SQL Server 工具、[用于 Visual Studio Code 的 Azure 机器学习](https://github.com/Microsoft/vs-tools-for-ai)    |
| 如何使用/运行它？      | 桌面快捷方式 (`C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE\devenv.exe`)    |
| DSVM 上的相关工具      |     Visual Studio Code、RStudio、Juno  |

## <a name="visual-studio-code"></a>Visual Studio Code 

|    |           |
| ------------- | ------------- |
| 它是什么？   | 常规用途 IDE      |
| 支持的 DSVM 版本      | Windows、Linux     |
| 典型用途      | 代码编辑器和 Git 集成   |
| 如何使用/运行它？      | Windows 中的桌面快捷方式 (`C:\Program Files (x86)\Microsoft VS Code\Code.exe`)、Linux 中的桌面快捷方式或终端 (`code`)    |
| DSVM 上的相关工具      |     Visual Studio 2017、RStudio、Juno  |

## <a name="rstudio--desktop"></a>RStudio Desktop 

|    |           |
| ------------- | ------------- |
| 它是什么？   | R 的客户端 IDE    |
| 支持的 DSVM 版本      | Windows、Linux      |
| 典型用途      |  R 开发     |
| 如何使用/运行它？      | Windows 中桌面快捷方式 (`C:\Program Files\RStudio\bin\rstudio.exe`)，Linux 中桌面快捷方式 (`/usr/bin/rstudio`)      |
| DSVM 上的相关工具      |   Visual Studio 2017、Visual Studio Code、Juno      |

## <a name="rstudio--server"></a>RStudio Server 

|    |           |
| ------------- | ------------- |
| 它是什么？   | R 的基于 Web 的 IDE    |
| 支持的 DSVM 版本      | Linux      |
| 典型用途      |  R 开发     |
| 如何使用/运行它？      | 使用 _systemctl enable rstudio-server_ 启用该服务，然后使用 _systemctl start rstudio-server_ 启动该服务。 然后可以在 http://your-vm-ip:8787 登录到 RStudio Server。       |
| DSVM 上的相关工具      |   Visual Studio 2017、Visual Studio Code、RStudio Desktop      |

## <a name="juno"></a>Juno 

|    |           |
| ------------- | ------------- |
| 它是什么？   | Julia 语言的客户端 IDE   |
| 支持的 DSVM 版本      | Windows、Linux      |
| 典型用途      |  Julia 开发     |
| 如何使用/运行它？      | Windows 中桌面快捷方式 (`C:\JuliaPro-0.5.1.1\Juno.bat`)，Linux 中桌面快捷方式 (`/opt/JuliaPro-VERSION/Juno`)      |
| DSVM 上的相关工具      |   Visual Studio 2017、Visual Studio Code、RStudio      |

## <a name="pycharm"></a>Pycharm

|    |           |
| ------------- | ------------- |
| 它是什么？   | Python 语言的客户端 IDE    |
| 支持的 DSVM 版本      | Linux      |
| 典型用途      |  Python 开发     |
| 如何使用/运行它？      | Linux 中桌面快捷方式 (`/usr/bin/pycharm`)      |
| DSVM 上的相关工具      |   Visual Studio 2017、Visual Studio Code、RStudio      |



## <a name="powerbi-desktop"></a>PowerBI Desktop 

|    |           |
| ------------- | ------------- |
| 它是什么？   | 交互式数据可视化和 BI 工具    |
| 支持的 DSVM 版本      | Windows  |
| 典型用途      |  数据可视化和构建仪表板   |
| 如何使用/运行它？      | 桌面快捷方式 (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| DSVM 上的相关工具      |   Visual Studio 2017、Visual Studio Code、Juno      |

