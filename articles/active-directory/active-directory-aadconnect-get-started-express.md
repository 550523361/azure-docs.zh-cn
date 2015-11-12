<properties
	pageTitle="通过快速设置开始使用 Azure AD Connect | Microsoft Azure"
	description="了解如何下载、安装和运行 Azure AD Connect 的设置向导。"
	services="active-directory"
	documentationCenter=""
	authors="billmath"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory"
	ms.date="10/13/2015"
	wacn.date=""/>

# 通过快速设置开始使用 Azure AD Connect
以下文档将会帮助你开始使用 Azure Active Directory Connect。本文档说明如何使用 Azure AD Connect 的快速安装。

## 相关文档
如果你尚未阅读有关[将本地标识与 Azure Active Directory 集成](active-directory-aadconnect.md)的文档，下表提供了相关主题的链接。开始安装之前，需要完成以粗体显示的前两个主题。

| 主题 | |
| --------- | --------- |
| **下载 Azure AD Connect** | [下载 Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) |
| **硬件和先决条件** | [Azure AD Connect：硬件和先决条件](active-directory-aadconnect-prerequisites.md) |
| 使用自定义设置安装 | [Azure AD Connect 的自定义安装](active-directory-aadconnect-get-started-custom.md) |
| 从 DirSync 升级 | [从 Azure AD 同步工具 (DirSync) 升级](active-directory-aadconnect-dirsync-upgrade-get-started.md) |
| 安装后 | [验证安装并分配许可证](active-directory-aadconnect-whats-next.md) |
| 用于安装的帐户 | [有关 Azure AD Connect 帐户和权限的详细信息](active-directory-aadconnect-accounts-permissions.md) |


## Azure AD Connect 的快速安装
“快速设置”是默认选项，也是最常用的设置方案之一。使用此选项时，Azure AD Connect 将部署使用密码同步选项的同步。这适用于单一林，它允许你的用户使用本地密码登录到云。使用“快速设置”在完成安装之后自动开始同步处理（不过可以选择不打开）。使用此选项时，只需单击几下鼠标就能将本地目录扩展到云中。

![欢迎使用 Azure AD Connect](./media/active-directory-aadconnect-get-started/welcome.png)

### 使用快速设置安装 Azure AD Connect

1. 以本地管理员身份登录到要安装 Azure AD Connect 的服务器。这应该是你想要用作同步服务器的服务器。
2. 导航到 AzureADConnect.msi 并双击它
3. 在“欢迎”屏幕上，选中同意许可条款对应的框，然后单击“继续”。
4. 在“快速设置”屏幕上，单击“使用快速设置”。![欢迎使用 Azure AD Connect](./media/active-directory-aadconnect-get-started/express.png)
5. 在“连接到 Azure AD”屏幕上，输入 Azure AD 的 Azure 全局管理员用户名和密码。单击**“下一步”**。
6. 在“连接到 AD DS”屏幕上，输入企业管理员帐户的用户名和密码。单击“下一步”。![欢迎使用 Azure AD Connect](./media/active-directory-aadconnect-get-started/install4.png)
7. 在“已准备好配置”屏幕上，单击“安装”。
	- 在“已准备好配置”页上，可以取消选中“配置完成后立即开始同步过程”复选框。如果你这样做，向导将配置同步，但会保持禁用任务，直到你在任务计划程序将它重新启用。启用任务后，将每隔三小时运行同步一次。
	- 此外，你也可以选择选中“Exchange 混合部署”对应的复选框，以配置同步服务。如果你不打算在云中和本地配置 Exchange 邮箱，则不需要这样做。![欢迎使用 Azure AD Connect](./media/active-directory-aadconnect-get-started/readyinstall.png)<br>
8. 安装完成后，单击“退出”。


<br> <br>

有关使用快速安装的视频，请参阅以下内容：

<center>[AZURE.VIDEO azure-active-directory-connect-express-settings]</center>


## 后续步骤
安装 Azure AD Connect 后，可以[验证安装并分配许可证](active-directory-aadconnect-whats-next.md)。

了解有关[将本地标识与 Azure Active Directory 集成](active-directory-aadconnect.md)的详细信息。

<!---HONumber=79-->