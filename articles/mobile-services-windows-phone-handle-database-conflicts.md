<properties linkid="develop-mobile-tutorials-optimistic-concurrent-data-wp8" urlDisplayName="Optimistic concurrency" pageTitle="Handle database write conflicts with optimistic concurrency (Windows Store) | Mobile Dev Center" metaKeywords="" description="Learn how to handle database write conflicts on both the server and in your Windows Store application." metaCanonical="" disqusComments="1" umbracoNaviHide="1" documentationCenter="Mobile" title="Handling database write conlicts" authors="wesmc" />

# 处理数据库写入冲突

<div class="dev-center-tutorial-selector sublanding">
<a href="/zh-cn/develop/mobile/tutorials/handle-database-write-conflicts-dotnet/" title="Windows Store C#">Windows 应用商店 C\#</a>
<a href="/zh-cn/documentation/articles/mobile-services-windows-store-javascript-handle-database-conflicts/" title="Windows Store JavaScript">Windows 应用商店 JavaScript</a>
<a href="/zh-cn/develop/mobile/tutorials/handle-database-write-conflicts-wp8/" title="Windows Phone" class="current">Windows Phone</a>
</div>

本教程旨在帮助你更好地理解在两个或两个以上客户端写入 Windows Phone 8 应用程序中的同一条数据库记录时，如何处理发生的冲突。在某些情况下，两个或两个以上客户端可能会同时将更改写入同一项目。如果没有任何冲突检测，则最后一次写入会覆盖任何以前的更新，即使这并不是所需要的结果。移动服务为检测和解决这些冲突提供支持。本主题将指导你完成用于处理服务器上和应用程序中数据库写入冲突的步骤。

在本教程中，你将向快速入门应用程序添加功能以处理更新 TodoItem 数据库时发生的争用。本教程将指导你完成以下基本步骤：

1.  [更新应用程序以允许更新][更新应用程序以允许更新]
2.  [在应用程序中启用冲突检测][在应用程序中启用冲突检测]
3.  [测试应用程序中的数据库写入冲突][测试应用程序中的数据库写入冲突]
4.  [使用服务器脚本自动解决冲突][使用服务器脚本自动解决冲突]

本教程需要的内容如下：

-   Microsoft Visual Studio 2012 Express for Windows Phone 8 或更高版本。
-   在 Windows 8 上运行的 [Windows Phone 8 SDK][Windows Phone 8 SDK]。
-   [Azure 帐户][Azure 帐户]
-   本教程基于移动服务快速入门。在开始本教程之前，必须先完成[移动服务入门][移动服务入门]。
-   Azure 移动服务 NuGet 包 1.1.0 或更高版本。若要获取最新版本，请执行以下步骤：

    1.  在 Visual Studio 中，打开项目并右键单击解决方案资源管理器中的项目，然后单击“管理 Nuget 包” 。

        ![][0]

    2.  展开“联机” 并单击“Microsoft 和 .NET” 。在“搜索”文本框中输入“Azure 移动服务” 。针对 Azure 移动服务  NuGet 包单击“安装” 。

        ![][1]

<a name="uiupdate"></a>
## 更新 UI更新应用程序以允许更新

在本节中，你将更新 TodoList 用户界面，以便更新列表框控件中每个项目的文本。列表框将包含针对数据库表中每个项目的复选框和文本框控件。你将能够更新 TodoItem 的文本字段。应用程序将处理文本框的 `LostFocus` 事件以更新数据库中的项目。

1.  在 Visual Studio 中，打开你在[移动服务入门][移动服务入门]教程中下载的 TodoList 项目。
2.  在 Visual Studio 解决方案资源管理器中，打开 MainPage.xaml，将 `phone:LongListSelector` 定义替换为下面显示的列表框并保存更改。

        <ListBox Grid.Row="4" Grid.ColumnSpan="2" Name="ListItems">
        <ListBox.ItemTemplate>
        <DataTemplate>
        <StackPanel Orientation="Horizontal">
        <CheckBox Name="CheckBoxComplete" IsChecked="{Binding Complete, Mode=TwoWay}" Checked="CheckBoxComplete_Checked" Margin="10,5" VerticalAlignment="Center"/>
        <TextBox x:Name="ToDoText" Width="330" Text="{Binding Text, Mode=TwoWay}" AcceptsReturn="False" LostFocus="ToDoText_LostFocus"/>
        </StackPanel>
        </DataTemplate>
        </ListBox.ItemTemplate>
        </ListBox>

3.  在 Visual Studio 解决方案资源管理器中，打开 MainPage.xaml.cs 并添加以下 `using` 指令。

        using System.Threading.Tasks;

4.  在 Visual Studio 解决方案资源管理器中，打开 MainPage.xaml.cs。向 MainPage 添加 `LostFocus` 事件的事件处理程序，如下所示。

        private async void ToDoText_LostFocus(object sender, RoutedEventArgs e)
        {
        TextBox tb = (TextBox)sender;
        TodoItem item = tb.DataContext as TodoItem;
        //let's see if the text changed
        if (item.Text != tb.Text)
            {
        item.Text = tb.Text;
        await UpdateToDoItem(item);
            }
        }

5.  在 MainPage.xaml.cs 中，添加事件处理程序中引用的 MainPage `UpdateToDoItem()` 方法的定义，如下所示。

        private async Task UpdateToDoItem(TodoItem item)
        {
        try
            {
        //update at the remote table
        await todoTable.UpdateAsync(item);
            }
        catch (Exception ex)
            {
        MessageBox.Show(ex.Message, "Update Failed", MessageBoxButton.OK);
            }
        }

现在，当文本框失去焦点时，应用程序将对每个项目的文本更改写回到数据库。

<a name="enableOC"></a>
## 启用乐观并发在应用程序中启用冲突检测

在某些情况下，两个或两个以上客户端可能会同时将更改写入同一项目。如果没有任何冲突检测，则最后一次写入会覆盖任何以前的更新，即使这并不是所需要的结果。[乐观并发控制][乐观并发控制]假定每个事务均可以提交，因此不使用任何资源锁定。提交事务之前，乐观并发控制将验证是否没有其他事务修改了数据。如果数据已修改，则将回滚正在提交的事务。Azure 移动服务通过使用添加到每个表的 `__version` 系统属性列跟踪对每个项目的更改，来支持乐观并发控制。在本节中，我们将允许应用程序通过 `__version` 系统属性检测这些写入冲突。如果记录自上次查询以来已更改，则应用程序在更新尝试期间将收到 `MobileServicePreconditionFailedException` 发出的通知。然后，它将能够选择是将其更改提交到数据库，还是保持对数据库的上次更改不变。有关移动服务的系统属性的详细信息，请参阅[系统属性][系统属性]。

1.  在 MainPage.xaml.cs 中，使用以下代码更新 "TodoItem" 类定义以包括 \*\*\_\_version\*\* 系统属性，从而启用对写入冲突检测的支持：

        public class TodoItem
        {
        public string Id { get; set; }            
        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }            
        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }            
        [JsonProperty(PropertyName = "__version")]
        public string Version { set; get; }
        }

	<div class="dev-callout"><b>说明</b>

    <p>使用非类型表时，请通过将 Version 标志添加到表的 SystemProperties 来启用乐观并发。</p>

		<pre><code>//Enable optimistic concurrency by retrieving __version
	todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
	</code></pre>

	</div>

2.  将 `Version` 属性添加到 `TodoItem` 类后，如果记录自上次查询以来已更改，则在更新期间将通过 `MobileServicePreconditionFailedException` 异常通知应用程序。此异常包括服务器中该项目的最新版本。在 MainPage.xaml.cs 中，添加以下代码以在 `UpdateToDoItem()` 方法中处理异常。

        private async Task UpdateToDoItem(TodoItem item)
        {
        Exception exception = null;
        try
            {
        //update at the remote table
        await todoTable.UpdateAsync(item);
            }
        catch (MobileServicePreconditionFailedException<TodoItem> writeException)
            {
        exception = writeException;
            }
        catch (Exception ex)
            {
        exception = ex;
            }
        if (exception != null)
            {
        if (exception is MobileServicePreconditionFailedException<TodoItem>)
                {
        //conflict detected, the item has changed since the last query
        await ResolveConflict(item, ((MobileServicePreconditionFailedException<TodoItem>)exception).Item);
                }
        else
        MessageBox.Show(exception.Message, "Update Failed", MessageBoxButton.OK);
            }
        }

3.  在 MainPage.xaml.cs 中，添加 `UpdateToDoItem()` 中引用的 `ResolveConflict()` 方法的定义。请注意，为了解决冲突，在提交用户的决定之前，请将本地项目的版本设置为服务器中的已更新版本。否则，你将不断遇到冲突。

        private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)     
        {
        // Ask user to choose between the local text value or leaving the 
        // server's updated text value
        MessageBoxResult mbRes = MessageBox.Show(String.Format("The item has already been updated on the server.\n\n" +
        "Server value:{0} \n" +
        "Local value:{1}\n\n" +
        "Press OK to update the server with the local value.\n\n" +
        "Press CANCEL to keep the server value.", serverItem.Text, localItem.Text), 
        "CONFLICT DETECTED ", MessageBoxButton.OKCancel);
        // OK :After examining the updated text from the server, overwrite it
        //      with the changes made in this client.
        if (mbRes == MessageBoxResult.OK)
            {
        // Update the version of the item to the latest version
        // to resolve the conflict.Otherwise the exception
        // will be thrown again for the attempted update.
        localItem.Version = serverItem.Version;
        // Recursively updating just in case another conflict 
        // occurs while the user is deciding.
        await this.UpdateToDoItem(localItem);
            }
        // CANCEL :After examining the updated text from the server, leave 
        // the server item intact and refresh this client's query discarding 
        // the proposed changes.
        if (mbRes == MessageBoxResult.Cancel)
            {
        RefreshTodoItems();
            }
        }

<a name="test-app"></a>
## 测试应用程序测试应用程序中的数据库写入冲突

在本节中，你将通过在两个不同的 Windows Phone 8 模拟器（WVGA 和 WVGA 512M）中运行该应用程序来测试用于处理写入冲突的代码。这两个客户端应用程序将尝试更新同一项目的 `text` 属性，因此需要用户解决该冲突。

1.  在 Visual Studio 中，请确保从下拉框中选择“模拟器 WVGA 512MB” 作为部署目标，如下面的屏幕快照中所示。

    ![][2]

2.  在 Visual Studio 的菜单中，单击“生成” ，然后单击“部署解决方案” 。如果模拟器先前未运行，则模拟器加载 Windows Phone 8 操作系统将需要几分钟时间。在底部的输出窗口中验证生成解决方案并将其部署到 Windows Phone 8 模拟器是否成功。

    ![][3]

3.  在 Visual Studio 中，将部署目标下拉框更改为“模拟器 WVGA” 。

    ![][4]

4.  在 Visual Studio 的菜单中，单击“生成” ，然后单击“部署解决方案” 。在底部的输出窗口中验证生成解决方案并将其部署到 Windows Phone 8 模拟器是否成功。

    ![][3]

5.  并排放置正在运行的两个模拟器。我们可以模拟在这两个模拟器上运行的客户端应用程序之间发生并发写入冲突。在这两个模拟器中从右向左轻扫以查看已安装应用程序的列表。滚动到每个列表的底部，然后单击 Todolist 应用程序 。

    ![][5]

6.  在左侧模拟器中，将最后一个 TodoItem 的 `text` 更新为“Test Write 1” ，然后单击另一个文本框，以便 `LostFocus` 事件处理程序更新数据库。下面的屏幕快照显示了一个示例。

    ![][6]

7.  此时，右侧模拟器中相应项目的版本和文本值为旧版本和旧文本值。在右侧模拟器中，针对 text 属性输入“Test Write 2” 。然后单击另一个文本框，右侧模拟器中的 `LostFocus` 事件处理程序便会尝试使用旧版本更新数据库。

    ![][7]

8.  由于用于更新尝试的版本与服务器版本不匹配，移动服务 SDK 将引发 `MobileServicePreconditionFailedException`，此时可通过应用程序来解决此冲突。若要解决此冲突，可单击“确定” 从右侧应用程序提交值。也可以单击“取消” 放弃右侧应用程序中的值，让左侧应用程序提交相关值。

    ![][8]

<a name="scriptsexample"></a>
## 使用脚本处理冲突使用服务器脚本自动解决冲突

可以使用服务器脚本检测和解决写入冲突。当你可以使用脚本逻辑而不是用户交互来解决冲突时，这是可行的。在本节中，你将向应用程序的 TodoItem 表添加服务器端脚本。该脚本将用于解决冲突的逻辑如下所示：

-   如果 TodoItem 的 `complete` 字段设为 true，则可以认为该项已完成并且无法再更改 `text`。
-   如果 TodoItem 的 `complete` 字段仍为 false，则将提交更新 `text` 的尝试。

以下步骤将指导你完成添加服务器更新脚本并对其进行测试的过程。

1.  登录到 [Azure 管理门户][Azure 管理门户]，单击“移动服务” ，然后单击你的应用程序。

    ![][9]

2.  单击“数据” 选项卡，然后单击 TodoItem  表。

    ![][10]

3.  单击“脚本” ，然后选择“更新” 操作。

    ![][11]

4.  将现有脚本替换为以下函数，然后单击“保存” 。

        function update(item, user, request) { 
        request.execute({ 
        conflict:function (serverRecord) {
        // Only committing changes if the item is not completed.
        if (serverRecord.complete === false) {
        //write the updated item to the table
        request.execute();
                    }
        else
                    {
        request.respond(statusCodes.FORBIDDEN, 'The item is already completed.');
                    }
                }
            }); 
        }   

5.  在左侧模拟器的应用程序中更改最后一个项目的 TodoItem 文本。然后单击另一个文本框，让 `LostFocus` 事件处理程序更新数据库。

    ![][6]

6.  在右侧模拟器中，针对最后一个 TodoItem 的 text 属性输入不同的值。然后单击另一个文本框，右侧模拟器中的 `LostFocus` 事件处理程序便会尝试使用旧版本更新数据库

    ![][7]

7.  请注意，由于该项目未标记为“完成”而允许更新，服务器脚本解决了冲突，因此在应用程序中未遇到异常。若要查看更新是否确实成功，请单击左侧模拟器的应用程序中的“刷新” 以重新查询数据库。

    ![][12]

8.  在左侧模拟器的应用程序中，单击复选框以完成最后一个 TodoItem。

    ![][13]

9.  在右侧模拟器的应用程序中，尝试更新同一 TodoItem 的文本，并触发 `LostFocus` 事件。在对冲突进行响应时，由于该项目已完成，此脚本通过拒绝更新解决了冲突。

    ![][14]

<a name="next-steps"> </a>
## 后续步骤

本教程演示了在处理移动服务中的数据时，如何让 Windows Phone 8 应用程序处理写入冲突。接下来，请考虑完成数据系列中的以下教程之一：

-   [使用脚本验证和修改数据][使用脚本验证和修改数据]
    了解更多有关使用移动服务中的服务器脚本验证和更改从应用程序发送的数据的信息。

-   [使用分页优化查询][使用分页优化查询]
    了解如何使用查询中的分页控制单个请求中处理的数据量。

完成了数据系列后，你还可以尝试以下 Windows Phone 8 教程之一：

-   [身份验证入门][身份验证入门]
    了解如何对应用程序用户进行身份验证。

-   [推送通知入门][推送通知入门]
    了解如何使用移动服务将非常基本的推送通知发送到应用程序。

  [更新应用程序以允许更新]: #uiupdate
  [在应用程序中启用冲突检测]: #enableOC
  [测试应用程序中的数据库写入冲突]: #test-app
  [使用服务器脚本自动解决冲突]: #scriptsexample
  [Windows Phone 8 SDK]: http://go.microsoft.com/fwlink/p/?LinkID=268374
  [Azure 帐户]: http://www.windowsazure.com/zh-cn/pricing/free-trial/
  [移动服务入门]: /zh-cn/develop/mobile/tutorials/get-started-wp8
  [0]: ./media/mobile-services-windows-phone-handle-database-conflicts/mobile-manage-nuget-packages-VS.png
  [1]: ./media/mobile-services-windows-phone-handle-database-conflicts/mobile-manage-nuget-packages-dialog.png
  [乐观并发控制]: http://go.microsoft.com/fwlink/?LinkId=330935
  [系统属性]: http://go.microsoft.com/fwlink/?LinkId=331143
  [2]: ./media/mobile-services-windows-phone-handle-database-conflicts/mobile-EmulatorWVGA512MB.png
  [3]: ./media/mobile-services-windows-phone-handle-database-conflicts/mobile-build-deploy-wp8.png
  [4]: ./media/mobile-services-windows-phone-handle-database-conflicts/mobile-EmulatorWVGA.png
  [5]: ./media/mobile-services-windows-phone-handle-database-conflicts/mobile-start-apps-oc-wp8.png
  [6]: ./media/mobile-services-windows-phone-handle-database-conflicts/mobile-oc-apps-write1-wp8.png
  [7]: ./media/mobile-services-windows-phone-handle-database-conflicts/mobile-oc-apps-write2-wp8.png
  [8]: ./media/mobile-services-windows-phone-handle-database-conflicts/mobile-oc-apps-exception-wp8.png
  [Azure 管理门户]: https://manage.windowsazure.cn/
  [9]: ./media/mobile-services-windows-phone-handle-database-conflicts/mobile-services-selection.png
  [10]: ./media/mobile-services-windows-phone-handle-database-conflicts/mobile-portal-data-tables.png
  [11]: ./media/mobile-services-windows-phone-handle-database-conflicts/mobile-insert-script-users.png
  [12]: ./media/mobile-services-windows-phone-handle-database-conflicts/mobile-oc-apps-insync-wp8.png
  [13]: ./media/mobile-services-windows-phone-handle-database-conflicts/mobile-oc-apps-complete-checkbox-wp8.png
  [14]: ./media/mobile-services-windows-phone-handle-database-conflicts/mobile-oc-apps-already-completed-wp8.png
  [使用脚本验证和修改数据]: /zh-cn/develop/mobile/tutorials/validate-modify-and-augment-data-wp8
  [使用分页优化查询]: /zh-cn/develop/mobile/tutorials/add-paging-to-data-wp8
  [身份验证入门]: /zh-cn/develop/mobile/tutorials/get-started-with-users-wp8
  [推送通知入门]: /zh-cn/develop/mobile/tutorials/get-started-with-push-wp8
