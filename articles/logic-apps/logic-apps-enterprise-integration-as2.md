---
title: 用于 B2B 企业集成的 AS2 消息 - Azure 逻辑应用 | Microsoft 文档
description: 在带有 Enterprise Integration Pack 的 Azure 逻辑应用中交换 AS2 消息以实现 B2B 企业集成
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: c9b7e1a9-4791-474c-855f-988bd7bf4b7f
ms.date: 06/08/2017
ms.openlocfilehash: 3413b235d9202530eb1a3129637e3746bbe6585b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/18/2019
ms.locfileid: "57872547"
---
# <a name="exchange-as2-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>在带有 Enterprise Integration Pack 的 Azure 逻辑应用中交换 AS2 消息以实现 B2B 企业集成

在交换 Azure 逻辑应用的 AS2 消息之前，必须先创建 AS2 协议并将它存储在集成帐户中。 下面是创建 AS2 协议的步骤。

## <a name="before-you-start"></a>开始之前

下面是需要准备好的项：

* 已定义的、与 Azure 订阅关联的[集成帐户](../logic-apps/logic-apps-enterprise-integration-accounts.md)
* 在集成帐户中至少已定义两个[合作伙伴](logic-apps-enterprise-integration-partners.md)，并且在“企业标识”下面配置了这些合作伙伴的 AS2 限定符

> [!NOTE]
> 创建协议时，协议文件中的内容必须与协议类型匹配。    

[创建集成帐户](../logic-apps/logic-apps-enterprise-integration-accounts.md)并[添加合作伙伴](logic-apps-enterprise-integration-partners.md)之后，可以遵循以下步骤来创建 AS2 协议。

## <a name="create-an-as2-agreement"></a>创建 AS2 协议

1.  登录 [Azure 门户](https://portal.azure.com "Azure portal")。  

2. 在 Azure 主菜单中，选择“所有服务”。 在搜索框中输入“集成”，然后选择“集成帐户”。

   ![查找集成帐户](./media/logic-apps-enterprise-integration-as2/overview-1.png)

   > [!TIP]
   > 如果未看到“所有服务”，可能需要先展开菜单。 在折叠的菜单顶部，选择“显示文本标签”。

3. 在“集成帐户”下，选择要创建协议的集成帐户。

   ![选择要在其中创建协议的集成帐户](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. 选择“协议”磁贴。 如果未添加“协议”磁贴，请先添加该磁贴。

    ![选择“协议”磁贴](./media/logic-apps-enterprise-integration-as2/agreement-1.png)

5. 在“协议”下，选择“添加”。

    ![选择“添加”](./media/logic-apps-enterprise-integration-as2/agreement-2.png)

6. 在“添加”下面，输入协议的**名称**。 对于“协议类型”，请选择“AS2”。 为协议选择“宿主合作伙伴”，“宿主标识”、“来宾合作伙伴”和“来宾标识”。

    ![提供协议详细信息](./media/logic-apps-enterprise-integration-as2/agreement-3.png)  

    | 属性 | 描述 |
    | --- | --- |
    | 名称 |协议的名称 |
    | 协议类型 | 应为 AS2 |
    | 管理方 |协议需要有管理方和托管方。 宿主合作伙伴代表配置协议的组织。 |
    | 管理方标识 |管理方的标识符 |
    | 托管方 |协议需要有管理方和托管方。 托管方代表与管理方进行交易的组织。 |
    | 托管方标识 |托管方的标识符 |
    | 接收设置 |这些属性适用于协议接收的所有消息。 |
    | 发送设置 |这些属性适用于协议发送的所有消息。 |

## <a name="configure-how-your-agreement-handles-received-messages"></a>配置协议如何处理收到的消息

设置协议属性后，可以配置此协议如何识别和处理从合作伙伴接收的传入消息。

1.  在“添加”下面，选择“接收设置”。
根据要与其交换消息的合作伙伴达成的协议来配置这些属性。 有关属性说明，请参阅本部分中的表格。

    ![配置“接收设置”](./media/logic-apps-enterprise-integration-as2/agreement-4.png)

2. 或者，可以选择“替代消息属性”来替代传入消息的属性。

3. 如需对所有传入消息进行签名，请选择“应对消息进行签名”。 在“证书”列表中，选择现有的[来宾合作伙伴公共证书](../logic-apps/logic-apps-enterprise-integration-certificates.md)来验证消息的签名。 如果没有该证书，可以创建一个。

4.  如果要对所有传入消息进行加密，请选择“应对消息进行加密”。 在“证书”列表中，选择现有的[宿主合作伙伴公共证书](../logic-apps/logic-apps-enterprise-integration-certificates.md)来解密传入的消息。 如果没有该证书，可以创建一个。

5. 若要压缩消息，请选择“应对消息进行压缩”。

6. 如需为接收的消息发送同步消息处置通知 (MDN)，请选择“发送 MDN”。

7. 如需为接收的消息发送已签名 MDN，请选择“发送已签名的 MDN”。

8. 如需为接收的消息发送异步 MDN，请选择“发送异步 MDN”。

9. 完成后，请务必选择“确定”保存设置。

协议现已准备就绪，可以处理符合所选设置的传入消息。

| 属性 | 描述 |
| --- | --- |
| 替代消息属性 |表示可替代接收消息中的属性。 |
| 对消息进行签名 |要求对消息进行数字签名。 配置来宾合作伙伴公共证书以进行签名验证。  |
| 对消息进行加密 |要求对消息进行加密。 未加密的消息会被拒绝。 配置主机合作伙伴私有证书以对消息进行解密。  |
| 对消息进行压缩 |要求对消息进行压缩。 未压缩的消息会被拒绝。 |
| MDN 文本 |要发送到消息发送方的默认消息处置通知 (MDN)。 |
| 发送 MDN |要求对 MDN 进行发送。 |
| 发送已签名的 MDN |要求对 MDN 进行签名。 |
| MIC 算法 |选择用于消息签名的算法。 |
| 发送异步 MDN | 要求对消息进行异步发送。 |
| 代码 | 指定要将 MDN 发送到的 URL。 |

## <a name="configure-how-your-agreement-sends-messages"></a>配置协议如何发送消息

可以配置此协议如何识别和处理发送给合作伙伴的传出消息。

1.  在“添加”下面，选择“发送设置”。
根据要与其交换消息的合作伙伴达成的协议来配置这些属性。 有关属性说明，请参阅本部分中的表格。

    ![设置“发送设置”属性](./media/logic-apps-enterprise-integration-as2/agreement-51.png)

2. 要将签名的消息发送到合作伙伴，请选择“启用消息签名”。 若要对消息签名，请在“MIC 算法”列表中，选择“宿主合作伙伴专用证书 MIC 算法”。 在“证书”列表中，选择现有的[宿主合作伙伴专用证书](../logic-apps/logic-apps-enterprise-integration-certificates.md)。

3. 要将加密的消息发送到合作伙伴，请选择“启用消息加密”。 若要加密消息，请在“加密算法”列表中，选择*来宾合作伙伴公共证书算法*。
在“证书”列表中，选择现有的[来宾合作伙伴公共证书](../logic-apps/logic-apps-enterprise-integration-certificates.md)。

4. 若要压缩消息，请选择“启用消息压缩”。

5. 要将 HTTP 内容类型标头展开为单个行，请选择“展开 HTTP 标头”。

6. 若要接收所发送消息的同步 MDN，请选择“请求 MDN”。

7. 若要接收所发送消息的已签名 MDN，请选择“请求已签名的 MDN”。

8. 若要接收所发送消息的异步 MDN，请选择“请求异步 MDN”。 如果选择此选项，请输入要将 MDN 发送到的 URL。

9. 若要启用回执的不可否认性，请选择“启用 NRR”。  

10. 若要指定要在 MIC 中使用或者在 AS2 消息或 MDN 的传出标头中签名的算法格式，请选择“SHA2 算法格式”。  

11. 完成后，请务必选择“确定”保存设置。

协议现已准备就绪，可以处理符合所选设置的传出消息。

| 属性 | 描述 |
| --- | --- |
| 启用消息签名 |要求对所有发送自此协议的消息进行签名。 |
| MIC 算法 |用于消息签名的算法。 配置主机合作伙伴私有证书 MIC 算法以对消息进行签名。 |
| 证书 |选择用于消息签名的证书。 配置主机合作伙伴私有证书以对消息进行签名。 |
| 启用消息加密 |要求对所有发送自协议的消息进行加密。 配置来宾合作伙伴公共证书算法以对消息进行加密。 |
| 加密算法 |要用于消息加密的加密算法。 配置来宾合作伙伴公共证书以对消息进行加密。 |
| 证书 |要用于对消息进行加密的证书。 配置来宾合作伙伴私有证书以对消息进行加密。 |
| 启用消息压缩 |要求对所有发送自协议的消息进行压缩。 |
| 展开 HTTP 标头 |将 HTTP 内容类型标头展开为单行。 |
| 请求 MDN |需要所有发送自此协议的消息的 MDN。 |
| 要求对 MDN 进行签名 |要求对所有发送到此协议的 MDN 进行签名。 |
| 要求发送异步 MDN |要求将异步 MDN 发送到此协议。 |
| 代码 |指定要将 MDN 发送到的 URL。 |
| 启用 NRR |需要回执的不可否认性 (NRR)，这是一种通信属性，提供数据已按址接收到的证据。 |
| SHA2 算法格式 |选择要在 MIC 中使用或者在 AS2 消息或 MDN 的传出标头中签名的算法格式 |

## <a name="find-your-created-agreement"></a>查找创建的协议

1. 设置完所有协议属性后，请在“添加”页中选择“确定”来完成创建协议，并返回到集成帐户。

    新添加的协议随即会出现在“协议”列表中。

2. 还可以在集成帐户概述中查看协议。 在集成帐户菜单中选择“概述”，并选择“协议”磁贴。 

   ![选择“协议”磁贴可查看所有协议](./media/logic-apps-enterprise-integration-as2/agreement-6.png)

## <a name="view-the-swagger"></a>查看 Swagger
请参阅 [Swagger 详细信息](/connectors/as2/)。 

## <a name="next-steps"></a>后续步骤
* [了解有关 Enterprise Integration Pack 的详细信息](logic-apps-enterprise-integration-overview.md "了解 Enterprise Integration Pack")  
