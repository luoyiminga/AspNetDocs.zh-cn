---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: ASP.NET Identity：使用 EntityFramework MySQL 提供程序使用 MySQL 存储 (C#)-ASP.NET 4.x
author: maumar
description: 本教程演示如何将 ASP.NET 标识的默认数据存储机制 EntityFramework （SQL 客户端提供程序） 替换为使用 MySQL 签约...
ms.author: riande
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: e89ed139657c5ce9ddcc56879946c62038919483
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2019
ms.locfileid: "65121446"
---
# <a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity：通过 EntityFramework MySQL 提供程序使用 MySQL 存储 (C#)

通过[Maurycy Markowski](https://github.com/maumar)， [Raquel Soares De Almeida](https://github.com/raquelsa)， [Robert McMurray](https://github.com/rmcmurray)

> 本教程演示如何替换的默认数据存储机制[ **ASP.NET 标识**](introduction-to-aspnet-identity.md) EntityFramework （SQL 客户端提供程序） 使用 MySQL 提供程序使用。

本教程中，将涵盖以下主题：

- 在 Azure 上创建 MySQL 数据库
- 创建 MVC 应用程序使用 Visual Studio 2013 MVC 模板
- 配置 EntityFramework 以使用 MySQL 数据库提供程序
- 运行应用程序以验证结果

在本教程结束时，会创建 MVC 应用程序与 ASP.NET 标识存储，使用在 Azure 中托管的 MySQL 数据库。

## <a name="creating-a-mysql-database-instance-on-azure"></a>在 Azure 上创建 MySQL 数据库实例

1. 登录到 [Azure 门户](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409)。
2. 单击**新建**底部的页上，并选择**存储区**:

    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. 在中**Choose 和外接程序**向导中，选择**ClearDB MySQL 数据库**，然后单击**下一步**框架底部的箭头：

   [单击以下图像以将其展开。 ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. 保留默认值**免费**计划，更改**名称**到**IdentityMySQLDatabase**，选择离你，最近的区域，然后单击**下一步**框架底部的箭头：

   [单击以下图像以将其展开。 ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. 单击**采购**复选标记以完成创建数据库。

   [单击以下图像以将其展开。 ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. 创建数据库后，你可以管理从**外接程序**在管理门户中的选项卡。 若要检索你的数据库的连接信息，请单击**连接信息**页的底部：

   [单击以下图像以将其展开。 ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. 通过单击通过复制按钮来复制连接字符串**CONNECTIONSTRING**字段并将其保存; 您将稍后在本教程中此信息用于 MVC 应用程序：

   [单击以下图像以将其展开。 ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>创建 MVC 应用程序项目

若要完成本教程的此部分中的步骤，您将首先需要安装[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 后安装 Visual Studio 后，使用以下步骤创建新的 MVC 应用程序项目：

1. 打开 Visual Studio 2103。
2. 单击**新的项目**从**启动**页上，也可以单击**文件**菜单，然后**新项目**:

   [单击以下图像以将其展开。 ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. 时**新的项目**对话框中，展开**Visual C#** 在模板列表中，然后单击**Web**，然后选择**ASP.NET Web 应用程序**. 将项目命名**IdentityMySQLDemo** ，然后单击**确定**:

   [单击以下图像以将其展开。 ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. 在中**新建 ASP.NET 项目**对话框中，选择**MVC** templatewith 默认选项; 这会配置**单个用户帐户**作为身份验证方法。 单击“确定”：

   [单击以下图像以将其展开。 ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>配置 EntityFramework 以使用 MySQL 数据库

### <a name="update-the-entity-framework-assembly-for-your-project"></a>更新你的项目的实体框架程序集

从 Visual Studio 2013 模板创建的 MVC 应用程序包含对引用[EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework)包，但那里具有已对其发布以来该程序集的更新，其中包含大量性能改进。 若要在应用程序中使用这些最新的更新，使用以下步骤。

1. 在 Visual Studio 中打开你的 MVC 项目。
2. 单击**工具**，然后单击**NuGet 包管理器**，然后单击**程序包管理器控制台**:

   [单击以下图像以将其展开。 ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. **程序包管理器控制台**会在 Visual Studio 的下半部分中显示。 类型&quot;**更新 EntityFramework** &quot;然后按 Enter:

   [单击以下图像以将其展开。 ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>安装 EntityFramework MySQL 提供程序

为了使 EntityFramework 以连接到 MySQL 数据库，您需要安装 MySQL 提供程序。 为此，请打开**程序包管理器控制台**并键入&quot;**安装包 MySql.Data.Entity-Pre**&quot;，然后按 Enter。

> [!NOTE]
> 这是预发行版本的程序集，并且这种情况下它可能包含 bug。 不应在生产环境中使用预发行版本的提供程序。

[单击下图以将其展开。]

[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>你的应用程序的 Web.config 文件进行项目配置更改

在本部分中，您将配置实体框架使用刚安装的 MySQL 提供程序，注册 MySQL 提供程序工厂，并从 Azure 添加你的连接字符串。

> [!NOTE]
> 下面的示例包含 MySql.Data.dll 特定程序集版本。 如果程序集版本更改，需要修改使用正确版本的合适配置设置。

1. 在 Visual Studio 2013 中打开你的项目的 Web.config 文件。
2. 找到用于实体框架定义的默认数据库提供程序和工厂的以下配置设置：

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. 这些配置设置替换为以下内容，他们将配置实体框架，以使用 MySQL 提供程序：

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. 找到&lt;connectionStrings&gt;部分并替换为以下代码，将定义在 Azure 托管的 MySQL 数据库的连接字符串 (请注意，还从更改 providerName 值时原始）：

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>添加自定义的 MigrationHistory 上下文

使用实体框架 Code First **MigrationHistory**表来跟踪模型更改并确保数据库架构和概念性架构之间的一致性。 但是，此表不适用于 MySQL 默认情况下由于太大的主键。 若要纠正这种情况下，你将需要压缩该表的密钥大小。 为此，请使用以下步骤：

1. 此表的架构信息中捕获**HistoryContext**，可修改任何其他**DbContext**。 若要执行此操作，添加名为的新类文件**MySqlHistoryContext.cs**到项目中，并将其内容替换为下面的代码：

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. 接下来需要配置要使用修改后的实体框架**HistoryContext**，而不是默认值。 这可以通过利用基于代码的配置功能。 若要执行此操作，添加名为的新类文件**MySqlConfiguration.cs**到你的项目，并将其内容为：

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>创建的自定义 EntityFramework 初始值设定项 ApplicationDbContext

在本教程中精选的 MySQL 提供程序当前不支持实体框架迁移，因此将需要使用模型初始值设定项来连接到数据库。 由于本教程在 Azure 上使用 MySQL 实例，需要创建自定义的实体框架初始值设定项。

> [!NOTE]
> 如果要连接到 Azure 或如果您使用的是在本地托管的数据库上的 SQL Server 实例，则不需要此步骤。

若要创建用于 MySQL 的自定义实体框架初始值设定项，请使用以下步骤：

1. 添加名为的新类文件**MySqlInitializer.cs**到项目中，并替换它是用下面的代码的内容：

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. 打开**IdentityModels.cs**项目，它位于文件**模型**目录，并将其内容替换以下：

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>运行应用程序和验证数据库

完成前面几节中的步骤后，应测试您的数据库。 为此，请使用以下步骤：

1. 按**Ctrl + F5**生成并运行 web 应用程序。
2. 单击**注册**页面顶部的选项卡：

   [单击以下图像以将其展开。 ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. 输入新用户名和密码，然后依次**注册**:

   [单击以下图像以将其展开。 ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. 此时在 MySQL 数据库中，创建了 ASP.NET 标识表和用户已注册并登录到应用程序：

   [单击以下图像以将其展开。 ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>安装 MySQL Workbench 工具，以验证数据

1. 安装**MySQL Workbench**工具从[MySQL 下载页](http://dev.mysql.com/downloads/windows/installer/)
2. 在安装向导中：**功能选择**选项卡上，选择**MySQL Workbench**下**应用程序**部分。
3. 启动应用并添加新的连接使用在本教程的急创建的 Azure MySQL 数据库的连接字符串数据。
4. 建立连接后，检查**ASP.NET 标识**上创建的表**IdentityMySQLDatabase。**
5. 你将看到，所有 ASP.NET 标识所都需的表创建如下图中所示：

   [单击以下图像以将其展开。 ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. 检查**aspnetusers**实例表若要检查的项，因为注册新用户。

   [单击以下图像以将其展开。 ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
