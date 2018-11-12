---
title: include 文件
description: include 文件
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 08/28/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 8c8f3cd67186450fdcf65c177ea0353d297a3b01
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263966"
---
## <a name="generate-the-certificate-signing-request-file"></a>生成证书签名请求文件

Apple 推送通知服务 (APNS) 使用证书对推送通知进行身份验证。 请遵照这些说明来创建用于发送和接收通知的所需推送证书。 有关这些概念的详细信息，请参阅正式的 [Apple Push Notification 服务](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html)文档。

生成证书签名请求 (CSR) 文件，Apple 将使用该文件生成签名的推送证书。

1. 在 Mac 上，运行 **Keychain Access** 工具。 可以从启动板上的“Utilities”或“Other”文件夹中打开该工具。
2. 单击“Keychain Access”，展开“Certificate Assistant”（证书助理），并单击“Request a Certificate from a Certificate Authority...”（从证书颁发机构请求证书...）。

    ![使用 Keychain Access 请求新证书](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. 选择“用户电子邮件地址”和“公用名称”，确保已选择“保存到磁盘”，并单击“继续”。 将“CA Email Address”（CA 电子邮件地址）字段保留空白，因为它不是必填字段。

    ![所需证书信息](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. 在“Save As”（另存为）中为证书签名请求 (CSR) 文件键入一个名称，在“Where”（位置）中选择一个位置，并单击“Save”（保存）。

    ![为证书选择一个文件名](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)

    此操作会将 CSR 文件保存到选定位置；默认位置是桌面。 请记住为此文件选择的位置。

接下来，将要向 Apple 注册应用、启用推送通知并上传导出的 CSR 以创建一个推送证书。

## <a name="register-your-app-for-push-notifications"></a>为推送通知注册应用程序

要将推送通知发送到 iOS 应用程序，必须向 Apple 注册应用程序，还要注册推送通知。  

1. 如果尚未注册应用程序，请导航到 Apple 开发人员中心的 [iOS 设置门户](https://go.microsoft.com/fwlink/p/?LinkId=272456)，使用 Apple ID 登录，单击“Identifiers”（标识符），然后单击“App IDs”（应用 ID），最后单击“+”号注册新的应用。

    ![iOS 预配门户应用 ID 页](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. 更新新应用的以下三个字段，并单击“Continue”（继续）： 

    * **Name（名称）**：在“App ID Description”（应用 ID 说明）部分的“Name”（名称）字段中为应用键入一个描述性名称。
    * **Bundle Identifier**（捆绑标识符）：在“Explicit App ID”（显式应用 ID）部分下，使用[应用分发指南](https://help.apple.com/xcode/mac/current/#/dev91fe7130a)中所述的 `<Organization Identifier>.<Product Name>` 格式输入“Bundle Identifier”（捆绑标识符）。 使用的“Organization Identifier”（组织标识符）和“Product Name”（产品名称）必须与在创建 XCode 项目时使用的组织标识符与产品名称匹配。 在下面的屏幕截图中，*NotificationHubs* 用作组织标识符，*GetStarted* 用作产品名称。 如果确保此值与在 XCode 项目中使用的值匹配，则就可以在 XCode 中使用正确的发布配置文件。
    * **推送通知**：在“应用服务”部分选中“推送通知”选项。

    ![用于注册新应用 ID 的窗体](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

    此操作会生成应用 ID 并请求你确认信息。 单击“注册”  以确认新的应用 ID。

    单击“注册”后，会看到“注册已完成”屏幕，如下图所示。 单击“完成”。

    ![应用 ID 注册完成，显示了权利](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)

3. 在开发人员中心的“App IDs”（应用 ID）下，找到创建的应用 ID，然后单击其所在行。

    ![应用 ID 列表](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)

    单击应用 ID 会显示应用详细信息。 单击底部的“Edit”（编辑）按钮。

    ![编辑应用 ID 页](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)

4. 滚动到屏幕底部并单击“Development Push SSL Certificate”（开发推送 SSL 证书 ）部分下的“Create Certificate...”（创建证书...）按钮。

    ![“为应用 ID 创建证书”按钮](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    将显示“添加 iOS 证书”助手。

    > [!NOTE]
    > 本教程使用开发证书。 注册生产证书时使用相同的过程。 只需确保在发送通知时使用相同的证书类型。

5. 单击“Choose File”（选择文件），浏览到在第一个任务中创建的 CSR 文件保存到的位置，并单击“Generate”（生成）。

    ![已生成证书的 CSR 上传页](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

6. 门户创建证书之后，请单击“Download”（下载）按钮，并单击“Done”（完成）。

    ![已生成证书的下载页](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    这将下载证书并将其保存到计算机上的 **Downloads** 文件夹。

    ![在 Downloads 文件夹中找到证书文件](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [!NOTE]
    > 默认情况下，下载的文件（开发证书）名为 **aps_development.cer**。

7. 双击下载的推送证书 **aps_development.cer**。

    此操作将在密钥链中安装新证书，如下图所示：

    ![Keychain Access 证书列表，显示了新证书](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [!NOTE]
    > 证书中的名称可能会不同，但将以 **Apple Development iOS Push Services** 作为前缀。

8. 在 Keychain Access 中，右键单击在“Certificates”（证书）类别中创建的新推送证书。 单击“导出”，为文件命名，选择“.p12”格式，并单击“保存”。

    ![将证书作为 p12 格式导出](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)

    记下导出的 .p12 证书的文件名和位置。 它用于启用 APNS 身份验证。

    > [!NOTE]
    > 本教程会创建 QuickStart.p12 文件。 文件名和位置可能不同。

## <a name="create-a-provisioning-profile-for-the-app"></a>为应用程序创建配置文件

1. 返回 [iOS 预配门户](https://go.microsoft.com/fwlink/p/?LinkId=272456)，选择“Provisioning Profiles”（预配配置文件），选择“All”（全部），并单击 **+**（加号）按钮创建一个新的配置文件。 将显示“添加 iOS 预配配置文件”向导：

    ![预配配置文件列表](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. 选择“开发”下的“iOS 应用程序开发”作为预配配置文件类型，并单击“继续”。

3. 接下来，从“App ID”（应用 ID）下拉列表中选择创建的应用 ID，并单击“Continue”（继续）

    ![选择应用 ID](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)

4. 在“Select certificates”（选择证书）屏幕中，选择经常用于代码签名的开发证书，并单击“Continue”（继续）。 此证书不是所创建的推送证书。

    ![选择证书](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)

5. 接下来，选择要用于测试的“Devices”（设备），并单击“Continue”（继续）。

    ![选择设备](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)

6. 最后，在“Profile Name”（配置文件名称）中为配置文件选取一个名称，并单击“Generate”（生成）。

    ![选择预配配置文件名称](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)

7. 创建新的预配配置文件后，请单击下载该文件，并将其安装在 Xcode 开发计算机上。 然后单击“完成”。

    ![下载预配配置文件](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)
