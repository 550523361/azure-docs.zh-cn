按照下列步骤操作以创建新的移动服务。

1.  登录到[管理门户][]。

2.  在导航窗格的底部，单击“+新建” 。

    ![][]

3.  展开“计算”和“移动服务” ，然后单击“创建”。 

    ![][1]

    此时将显示“新建移动服务” 对话框。

4.  在“创建移动服务” 页上，选择“创建免费的 20 MB SQL Database” ，选择 "Node.js" 运行时，然后在“URL” 文本框中键入新移动服务的子域名称，并等待名称验证。名称验证完成后，单击右箭头按钮，转到下一页。

    ![][2]

    此时将显示“指定数据库设置”页。 

	<div class="dev-callout">说明<b>

    <p>在本教程中，你将创建新的 SQL Database 实例和服务器。你可以重用此新数据库，并对其进行管理，如同任何其他 SQL Database 实例一样。如果你在新移动服务的同一区域已经有了数据库，则可选择“使用现有数据库”，然后再选择该数据库。 由于额外的带宽成本和更高的延迟，不建议使用位于不同区域的数据库。</p>
	</div>

5.  在“名称”中 ，键入新数据库的名称，然后键入“登录名”， 也就是新的 SQL Database 服务器的管理员登录名，键入并确认密码，然后单击勾选按钮完成该过程。

    ![][3]

	<div class="dev-callout">说明</b>

    <p>如果你提供的密码不符合最低要求或不匹配，将显示警告。
    我们建议你记下自己指定的管理员登录名和密码；今后你将需要使用这些信息来重用 SQL Database 实例或服务器。</p>
	</div>

你现在已经创建了你的移动应用程序可以使用的新移动服务。

  [管理门户]: https://manage.windowsazure.cn/
  []: ./media/mobile-services-create-new-service/plus-new.png
  [1]: ./media/mobile-services-create-new-service/mobile-create.png
  [2]: ./media/mobile-services-create-new-service/mobile-create-page1.png
  [3]: ./media/mobile-services-create-new-service/mobile-create-page2.png
