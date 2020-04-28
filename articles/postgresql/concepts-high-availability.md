---
title: 高可用性-Azure Database for PostgreSQL-单服务器
description: 本文提供了有关 Azure Database for PostgreSQL 单服务器中的高可用性的信息。
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 80229ff78c4570db583f1218d5d2f72da2dec388
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "74768565"
---
# <a name="high-availability-concepts-in-azure-database-for-postgresql---single-server"></a>Azure Database for PostgreSQL - 单一服务器中的高可用性概念
Azure Database for PostgreSQL 服务提供有保证的高级别可用性。 有资金支持的服务级别协议 (SLA) 在正式版本发布后的可用性为 99.99%。 使用此服务期间，几乎没有应用程序故障时间。

## <a name="high-availability"></a>高可用性
高可用性 (HA) 模型以节点级中断发生时的内置故障转移机制为依据。 硬件故障或响应服务部署都有可能会导致节点级中断发生。

在任何时候，对 Azure Database for PostgreSQL 数据库服务器做出的更改都发生在事务的上下文中。 更改会在事务提交时同步记录到 Azure 存储中。 如果发生节点级中断，数据库服务器会自动新建节点，并将数据存储附加到新节点。 这会删除任何活动连接，并且不会提交任何即时事务。

## <a name="application-retry-logic-is-essential"></a>应用程序重试逻辑至关重要
必须构建 PostgreSQL 数据库应用程序，以检测和重试已删除的连接和失败的事务，这一点很重要。 当应用程序重试时，应用程序的连接会在透明状态下重定向到新创建的实例，以取代失败实例。

在 Azure 内部，网关用于将连接重定向到新建实例。 发生中断后，整个故障转移过程一般需要几十秒才能完成。 因为重定向由网关内部处理，所以对于客户端应用程序，外部连接字符串也是这样。

## <a name="scaling-up-or-down"></a>横向扩展或缩减
与 HA 模型类似，在 Azure Database for PostgreSQL 横向扩展或缩减时，将创建具有指定大小的新服务器实例。 现有数据存储会从原始实例中拆离，并附加到新实例中。

执行缩放操作期间，数据库连接会中断。 客户端应用程序的连接中断，未提交的未结事务也会遭取消。 在客户端应用程序重试连接或建立新连接后，网关便会将连接定向到新设置大小的实例。 

## <a name="next-steps"></a>后续步骤
- 了解如何[处理暂时性连接错误](concepts-connectivity.md)
- 了解如何[使用只读副本复制数据](howto-read-replicas-portal.md)
