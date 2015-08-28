
<properties 
	pageTitle="组的动态成员资格疑难解答 | Windows Azure" 
	description="列出 Azure AD 中组的动态成员资格的疑难解答提示的主题。" 
	services="active-directory" 
	documentationCenter="" 
	authors="femila" 
	manager="swadhwa" 
	editor="Curtis"
	tags="azure-classic-portal"/>

<tags 
	ms.service="active-directory" 
	ms.date="07/13/2015" 
	wacn.date=""/>


# 组的动态成员资格疑难解答

| 症状 | 操作 |
|--------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 我在组上配置了一条规则，但组中并未更新任何成员资格 | 验证“目录配置”选项卡中“启用委派组管理”设置是否设为“是”。请注意，只有当使用具有分配有 Azure Active Directory Premium 许可证的帐户登录时，才能看到此设置。验证规则上用户特性的值 - 是否有满足规则的用户？ |
| 我配置了一条规则，但现在该规则的现有成员已被删除 | 这在意料之中 – 启用或更改规则时，将删除组的现有成员。将把规则评估所产生的用户集作为成员添加到组中。 |
| 添加或更改规则后，没有立即看到成员资格的变化，这是为什么呢？ | 将在异步后台进程中定期对专用成员资格进行评估。租户中的用户数和作为规则结果而创建的组的大小是决定评估时间的一个因素。通常，用户数较少的租户将在数分钟内看到组成员资格的更改。而用户数较多的租户则需 30 分钟或更长时间才能使成员资格发生更改。 |

下面的主题将提供有关 Azure Active Directory 的一些其他信息

* [使用 Azure Active Directory 组管理对资源的访问](/documentation/articles/active-directory-manage-groups)

* [什么是 Azure Active Directory？](/documentation/articles/active-directory-whatis)

* [将本地标识与 Azure Active Directory 集成](/documentation/articles/active-directory-aadconnect)

<!---HONumber=67-->