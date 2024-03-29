---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 使用 Visual Studio 的 ASP.NET Web 部署：部署数据库更新 |Microsoft Docs
author: tdykstra
description: 本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure 应用服务 Web 应用或第三方托管提供商，通过使用...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 942cc3cbf472f76d2521247df97c856deb19b06b
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2019
ms.locfileid: "65131930"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>使用 Visual Studio 的 ASP.NET Web 部署：部署数据库更新

通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure 应用服务 Web 应用或第三方托管提供商，通过使用 Visual Studio 2012 或 Visual Studio 2010。 有关序列的信息，请参阅[系列中的第一个教程](introduction.md)。

## <a name="overview"></a>概述

在本教程中，将生成数据库更改和相关的代码的更改，在 Visual Studio 中，测试所做的更改，然后将更新部署到测试、 过渡和生产环境。

本教程首先演示如何更新由 Code First 迁移的数据库，然后它显示如何通过使用 dbDacFx 提供程序更新数据库更高版本。

提醒：如果收到错误消息或某些操作无法按完成以下教程，请务必检查[故障排除页](troubleshooting.md)。

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>使用 Code First 迁移部署数据库更新

在本部分中，您将添加到的出生日期列`Person`的基类`Student`和`Instructor`实体。 然后更新，以便它将显示新的列显示讲师数据的页面。 最后，将所做的更改部署到测试、 过渡和生产。

### <a name="add-a-column-to-a-table-in-the-application-database"></a>向应用程序数据库中的表添加列

1. 在中*ContosoUniversity.DAL*项目中，打开*Person.cs* ，并在末尾添加以下属性`Person`类 （应两个右大括号后面）：

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    接下来，更新`Seed`方法，以便它向新列提供值。 打开*migrations\ configuration.cs*并将代码块的开始为`var instructors = new List<Instructor>`与下面的代码块，其中包括出生日期信息：

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. 生成解决方案，然后打开**程序包管理器控制台**窗口。 请确保 ContosoUniversity.DAL 仍处于选中状态作为**默认项目**。
3. 在中**程序包管理器控制台**窗口中，选择**ContosoUniversity.DAL**作为**默认项目**，然后输入以下命令：

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    此命令完成后，Visual Studio 会打开定义新的类文件`DbMigration`类，然后在`Up`方法您可以看到创建的新列的代码。 `Up`方法时要实现此更改，将创建列和`Down`方法删除列时您正在回滚更改。

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. 生成解决方案，并输入以下命令中的**程序包管理器控制台**窗口 （请确保 ContosoUniversity.DAL 项目仍处于选中状态）：

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    实体框架运行`Up`方法，然后运行`Seed`方法。

### <a name="display-the-new-column-in-the-instructors-page"></a>讲师页中显示新的列

1. 在 ContosoUniversity 项目中，打开*Instructors.aspx*并添加新的模板字段来显示出生日期。 将其添加之间雇佣日期和 office 分配的：

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    （如果代码缩进进行同步，你可以按 CTRL-K，然后选择 CTRL-D 以便自动重新格式化该文件。）
2. 运行应用程序，然后单击**讲师**链接。

    当页面加载时，您看到它有新的出生日期字段。

    ![出生日期的讲师页](deploying-a-database-update/_static/image2.png)
3. 关闭浏览器。

### <a name="deploy-the-database-update"></a>部署数据库更新

1. 在中**解决方案资源管理器**选择 ContosoUniversity 项目。
2. 在中**Web 单键发布**工具栏上，单击**测试**发布配置文件，然后依次**发布 Web**。 (如果禁用工具栏，则选择中的 ContosoUniversity 项目**解决方案资源管理器**。)

    Visual Studio 将部署更新的应用程序，并在浏览器打开到主页页面。
3. 运行**讲师**页以验证是否已成功部署更新。

    当应用程序尝试访问此页的数据库时，Code First，更新数据库架构，并运行`Seed`方法。 显示页时，您看到的预期**出生日期**中它的日期的列。
4. 在中**Web 单键发布**工具栏上，单击**过渡**发布配置文件，然后依次**发布 Web**。
5. 运行**讲师**在过渡环境中以验证是否已成功部署更新的页。
6. 在中**Web 单键发布**工具栏上，单击**生产**发布配置文件，然后依次**发布 Web**。
7. 运行**讲师**在生产环境以验证是否已成功部署更新中的页。

    实际的生产应用程序更新中包含的数据库更改为您通常也将进行应用程序脱机部署期间使用*应用程序\_offline.htm*、 上一教程中所示。

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>将数据库更新部署使用 dbDacFx 提供程序

在本部分中，您将添加*注释*列与*用户*成员资格数据库中表，以创建页，可以显示和编辑每个用户的注释。 然后将所做的更改部署到测试、 过渡和生产。

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>向成员资格数据库中的表添加列

1. 在 Visual Studio 中打开**SQL Server 对象资源管理器**。
2. 展开 **(localdb) \v11.0**，展开**数据库**，展开**aspnet ContosoUniversity** (不**aspnet ContosoUniversity Prod**)然后展开**表**。

    如果没有看到 **(localdb) \v11.0**下**SQL Server**节点，右键单击**SQL Server**节点，然后单击**添加 SQL Server**。 在中**连接到服务器**对话框中输入 *(localdb) \v11.0*作为**服务器名称**，然后单击**Connect**。

    如果看不到**aspnet ContosoUniversity**中，运行该项目并使用登录*管理员*凭据 (密码*devpwd*)，然后刷新**SQL Server 对象资源管理器**窗口。
3. 右键单击**用户**表，并单击**视图设计器**。

    ![SSOX 视图设计器](deploying-a-database-update/_static/image3.png)
4. 在设计器中，添加*注释*列，并使其*nvarchar （128)* 和可以为 null，然后单击**更新**。

    ![添加注释列](deploying-a-database-update/_static/image4.png)
5. 在中**预览数据库更新**框中，单击**更新数据库**。

    ![预览数据库更新](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>创建页以显示和编辑新的列

1. 在中**解决方案资源管理器**，右键单击**帐户**文件夹中的 ContosoUniversity 项目中，单击**添加**，然后单击**新项**.
2. 创建一个新**Web 窗体使用母版页**并将其命名*UserInfo.aspx*。 接受默认值*Site.Master*文件作为主页面。
3. 将复制到以下标记`MainContent``Content`元素 (3 的最后一个`Content`元素):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. 右键单击*UserInfo.aspx*页上，单击**用浏览器查看**。
5. 登录你*管理员*用户凭据 (密码*devpwd*) 并将一些注释添加到用户以验证页是否工作正常。

    ![用户信息页面](deploying-a-database-update/_static/image6.png)
6. 关闭浏览器。

## <a name="deploy-the-database-update"></a>部署数据库更新

若要部署使用 dbDacFx 提供程序，只需选择**更新数据库**发布配置文件中的选项。 但是，为初始部署时使用此选项还配置一些其他的 SQL 脚本，运行： 那些仍处于配置文件，您必须阻止它们再次运行。

1. 打开**发布 Web**右键单击 ContosoUniversity 项目并单击向导**发布**。
2. 选择**测试**配置文件。
3. 单击**设置**选项卡。
4. 下**DefaultConnection**，选择**更新数据库**。
5. 禁用配置为用于初始部署运行其他脚本：

    1. 单击**配置数据库更新**。
    2. 在中**配置数据库更新**对话框中，清除的复选框旁边*Grant.sql*并*aspnet 数据 dev.sql*。
    3. 单击 **“关闭”**。
6. 单击**预览版**选项卡。
7. 下**数据库**和右侧**DefaultConnection**，单击**预览版数据库**链接。

    ![数据库预览](deploying-a-database-update/_static/image7.png)

    预览窗口显示了将在要进行匹配的源数据库架构的数据库架构的目标数据库中运行的脚本。 该脚本包括添加新列的 ALTER TABLE 命令。
8. 关闭**Database 预览版**对话框中，然后单击**发布**。

    Visual Studio 将部署更新的应用程序，并在浏览器打开到主页页面。
9. 运行用户信息页 (添加*Account/UserInfo.aspx*到主页 URL) 来验证是否已成功部署更新。 您必须输入登录*管理员*并*devpwd*。

    表中的数据尚未部署默认情况下，未配置数据部署脚本运行，因此不会发现在开发过程中添加的注释。 在过渡环境中以验证更改已部署到数据库，并且页面能够正常运行，可以立即添加新的注释。
10. 请按照相同的过程来将部署到过渡和生产。

    别忘了禁用额外的脚本。 相比测试配置文件的唯一区别是，将禁用在过渡环境中的只有一个脚本和生产配置文件，因为它们已配置为仅在运行*aspnet prod data.sql*。

    过渡和生产的凭据是管理员和 prodpwd。

    实际的生产应用程序更新中包含的数据库更改为您通常也将进行应用程序脱机部署期间通过上传*应用程序\_offline.htm*之前发布，将其删除然后中, 所示[前面的教程](deploying-a-code-update.md)。

## <a name="summary"></a>总结

现在，你已部署包含使用 Code First 迁移和 dbDacFx 提供程序的数据库更改的应用程序更新。

![出生日期的讲师页](deploying-a-database-update/_static/image8.png)

![用户信息页面](deploying-a-database-update/_static/image9.png)

下一步的教程演示如何使用命令行执行的部署。

> [!div class="step-by-step"]
> [上一页](deploying-a-code-update.md)
> [下一页](command-line-deployment.md)
