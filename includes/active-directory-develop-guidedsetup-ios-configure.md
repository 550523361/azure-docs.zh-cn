---
title: include 文件
description: include 文件
services: active-directory
documentationcenter: dev-center-name
author: brandwe
manager: mtillman
ms.openlocfilehash: ca7a47fbe2c5ee2a4eb10abf3b9b50a2d28c252e
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2018
ms.locfileid: "46466138"
---
## <a name="register-your-application"></a>注册应用程序
如接下来的两部分中所述，可以采用两种方式之一注册应用程序。

### <a name="option-1-express-mode"></a>选项 1：快速模式
现在需要在 Microsoft 应用程序注册门户中注册应用程序：
1. 通过 [Microsoft 应用程序注册门户](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure)注册应用程序
2.  输入应用程序的名称和电子邮件
3.  确保选中“指导式设置”选项
4.  按照说明获取应用程序 ID，并将其粘贴到代码中

### <a name="option-2-advanced-mode"></a>选项 2：高级模式

1.  转到 [Microsoft 应用程序注册门户](https://apps.dev.microsoft.com/portal/register-app)
2.  输入应用程序的名称
3.  确保取消选中“指导式设置”选项
4.  单击 `Add Platform`，选择 `Native Application` 并单击 `Save`
5.  返回 Xcode。 在 `ViewController.swift` 中，将以“ `let kClientID`”开头的行替换为刚注册的应用程序 ID：

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
按住 Control 键并单击 <code>Info.plist</code> 打开上下文菜单，然后单击：<code>Open As</code>> <code>Source Code</code>
</li>
<li>
在 <code>dict</code> 根节点下添加以下代码：
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Your_Application_Id_Here]</string>
        </array>
    </dict>
</array>
```
<ol start="8">
<li>
将 <i><code>[Your_Application_Id_Here]</code></i> 替换为刚注册的应用程序 ID
</li>
</ol>
