---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: 在 TFS 中创建团队项目 |Microsoft Docs
author: jrjlee
description: 本主题介绍如何创建在 Team Foundation Server (TFS) 2010年的新的团队项目。
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: d12e0636ce3b6239d125305db4354278f9f24960
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2019
ms.locfileid: "65108782"
---
# <a name="creating-a-team-project-in-tfs"></a>在 TFS 中创建团队项目

通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何创建在 Team Foundation Server (TFS) 2010年的新的团队项目。

本主题窗体的一系列教程基于虚构公司 Fabrikam，Inc.的企业部署要求的一部分本系列教程将使用的示例解决方案&#x2014; [Contact Manager 解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;来表示真实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 通信的 web 应用程序Foundation (WCF) 服务和数据库项目。

## <a name="task-overview"></a>任务概述

若要预配，并在 TFS 中使用新的团队项目，你将需要完成以下高级步骤：

- 向将创建新的团队项目的用户授予权限。
- 创建团队项目。
- 向将处理该项目的团队成员授予权限。
- 签入一些内容。

本主题将演示如何执行这些过程中，并且它将识别的用户和作业可能需要负责的每个过程的角色。 请注意，具体取决于你的组织的结构，每个任务可能是不同的人员的责任。

任务和本主题中的演练假定您已安装并配置 TFS，并创建了团队项目集合配置过程的一部分。 有关这些假设的详细信息和方案的更多常规背景信息，请参阅[TFS 生成服务器配置用于 Web 部署](configuring-a-tfs-build-server-for-web-deployment.md)。

## <a name="grant-permissions-to-the-team-project-creator"></a>向团队项目创建者授予权限

若要创建新的团队项目，则需要这些权限：

- 您必须具有**创建新的项目**TFS 应用层上的权限。 通常通过将用户添加到授予此权限**项目集合管理员**TFS 组。 **Team Foundation Administrators**全局组也包含此权限。
- 您必须有权创建新的团队网站中对应于 TFS 团队项目集合的 SharePoint 站点集合。 通常通过将用户添加到与 SharePoint 组授予此权限**完全控制**权限在 sharepoint 站点集合。
- 如果使用的 SQL Server Reporting Services 功能，您必须是的成员**Team Foundation 内容管理员**Reporting Services 中的角色。

### <a name="who-performs-these-procedures"></a>谁将执行以下步骤？

通常情况下，用户或组管理 TFS 部署还会执行这些过程。

由于这是一种高特权的组的权限，新的团队项目通常由创建的用户的一小部分与负责管理 TFS 部署。 开发人员将不通常被授予创建新的团队项目所需的权限。

### <a name="grant-permissions-in-tfs"></a>在 TFS 中授予权限

如果你想要使用户能够创建新的团队项目，第一项高级任务是将用户添加到**项目集合管理员**组的团队项目集合。

**若要将用户添加到项目集合管理员组**

1. 在 TFS 服务器上，在**启动**菜单，依次指向**所有程序**，单击**Microsoft Team Foundation Server 2010**，然后单击**Team Foundation管理控制台**。
2. 在导航树视图中，展开**应用程序层**，然后单击**团队项目集合**。

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. 在中**团队项目集合**窗格中，选择的团队项目的集合你想要管理。

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. 上**常规**选项卡上，单击**组成员身份**。

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. 在中**全局组**对话框中，选择**项目集合管理员**组，然后依次**属性**。
6. 在中**Team Foundation Server 组属性**对话框中，选择**Windows 用户或组**，然后单击**添加**。

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. 在中**选择用户、 计算机或组**对话框中，键入你想要能够创建新的团队项目的用户的用户名称，单击**检查名称**，然后单击**确定**.

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. 在中**Team Foundation Server 组属性**对话框中，单击**确定**。
9. 在中**全局组**对话框中，单击**关闭**。

### <a name="grant-permissions-in-sharepoint-services"></a>在 SharePoint Services 中授予权限

接下来，您需要为用户授予权限对应于 TFS 团队项目集合的 SharePoint 站点集合中创建新的团队网站。

**若要授予对 SharePoint 站点集合的完全控制权限**

1. 在 Team Foundation Server 管理控制台中，在**团队项目集合**页上，选择你想要管理的团队项目集合。
2. 上**SharePoint 站点**选项卡上，记下的值**当前默认站点位置**URL。

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. 打开 Internet Explorer，然后转到你在步骤 2 中记下的 URL。

    > [!NOTE]
    > 如果不以创建团队项目集合的用户登录到 Windows，您将需要登录到 SharePoint 作为此用户才能继续。
4. 上**站点操作**菜单上，单击**站点设置**。

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. 上**站点设置**页面上，在**用户和权限**，单击**人员和组**。
6. 在左侧的导航窗格中，单击**组**。

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. 在**人员和组：所有组**页上，单击**为此站点设置用户组**。

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > 您可能会收到<strong>HTTP 404 未找到</strong>由于双 HTTP 编码 bug 的错误。 如果发生这种情况，请与此替换 URL:   
   > `[site_collection_URL]/_layouts/permsetup.aspx` 例如：  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. 上**为此站点设置用户组**页上，添加将创建到团队项目的用户**所有者**组，然后依次**确定**。

    ![](creating-a-team-project-in-tfs/_static/image10.png)

使用户能够创建团队项目集合中的新团队项目的详细信息，请参阅[为团队项目集合设置管理员权限](https://msdn.microsoft.com/library/dd547204.aspx)。

## <a name="create-a-new-team-project-and-add-users"></a>创建新的团队项目并添加用户

所需的权限后，可以使用**团队资源管理器**Visual Studio 2010 创建新的团队项目中的窗口。 这种方法提供了向导，收集所有所需的信息，并在 TFS、 SharePoint 和 SQL Server Reporting Services 中执行必要的任务。 此外需要针对新的团队项目的权限向成员授予权限的开发人员团队，以使他们能够添加和修改内容。

### <a name="who-performs-these-procedures"></a>谁将执行以下步骤？

通常是 TFS 管理员或开发人员团队负责人将执行以下步骤。

### <a name="create-a-new-team-project"></a>创建新的团队项目

下一个过程介绍如何在 TFS 2010 中创建新的团队项目。

**若要创建新的团队项目**

1. 上**启动**菜单，依次指向**所有程序**，单击**Microsoft Visual Studio 2010**，右键单击**Microsoft Visual Studio 2010**，然后单击**以管理员身份运行**。

    > [!NOTE]
    > 如果您不以管理员身份运行 Visual Studio 2010，新的团队项目向导将失败的最后一个步骤。
2. 如果**用户帐户控制**出现对话框，请单击**是**。
3. 在 Visual Studio 中，在**团队**菜单上，单击**连接到 Team Foundation Server**。

    > [!NOTE]
    > 如果已配置 TFS 服务器的连接，则可以省略步骤 4-7。
4. 在中**连接到团队项目**对话框中，单击**服务器**。
5. 在中**添加/删除 Team Foundation Server**对话框中，单击**添加**。
6. 在中**添加 Team Foundation Server**对话框中，提供你的 TFS 实例的详细信息，然后单击**确定**。

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. 在中**添加/删除 Team Foundation Server**对话框中，单击**关闭**。
8. 在中**连接到团队项目**对话框中，选择你想要连接到，选择团队的 TFS 实例项目的集合你想要添加到，，然后单击**Connect**。

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. 在中**团队资源管理器**窗口中，右键单击团队项目集合，然后单击**新团队项目**。

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. 在中**新的团队项目**对话框中，提供的名称和描述为团队项目，然后单击**下一步**。

    > [!NOTE]
    > 如果你的团队项目中包含空格，可能会遇到一些问题，当您使用 Internet 信息服务 (IIS) Web 部署工具 （Web 部署） 将输出路径从包部署。 路径中的空格可以使其更难运行 Web 部署命令。

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. 上**选择过程模板**页上，选择你想要使用以管理开发过程中，然后单击的过程模板**下一步**。

    > [!NOTE]
    > 有关 TFS 过程模板的更多信息，请参阅[过程模板和工具](https://msdn.microsoft.com/vstudio/aa718795)。
12. 上**团队站点设置**页上，保留默认设置保持不变，，然后单击**下一步**。
13. 此设置创建，或标识，与 TFS 团队项目相关联的 SharePoint 团队站点。 您的开发团队可以使用此站点来管理文档、 参与讨论线索、 创建 wiki 页面和执行其他不与代码相关的各种任务。 有关详细信息，请参阅[SharePoint 产品之间的交互和 Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx)。
14. 上**指定源代码管理设置**页上，保留默认设置保持不变，，然后单击**下一步**。
15. 此设置标识，或将其用作内容的根文件夹的 TFS 文件夹层次结构创建位置。
16. 上**确认团队项目设置**页上，单击**完成**。
17. 新的团队项目已成功创建时，在**创建的团队项目**页上，单击**关闭**。

### <a name="add-users-to-a-team-project"></a>将用户添加到团队项目

现在，已创建新的团队项目，可以将权限授予用户，使他们可以开始添加内容并进行协作。

**若要将用户添加到团队项目**

1. 在 Visual Studio 2010 中，在**团队资源管理器**窗口中，右键单击团队项目，依次指向**团队项目设置**，然后单击**组成员身份**。

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. 若要使用户能够添加、 修改和删除代码在源代码管理下的，需要将他或她到添加**contributors （参与者)** 组。
3. 在中**项目组**对话框中，选择**参与者**组，然后依次**属性**。

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. 在中**Team Foundation Server 组属性**对话框中，选择**Windows 用户或组**，然后单击**添加**。

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. 在中**选择用户、 计算机或组**对话框中，键入你想要添加到团队项目中，用户的用户名称，单击**检查名称**，然后单击**确定**。

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. 在中**Team Foundation Server 组属性**对话框中，单击**确定**。
7. 在中**项目组**对话框中，单击**关闭**。

## <a name="conclusion"></a>结束语

此时，新的团队项目已准备好使用，和你的开发人员团队可以开始添加内容和协作开发进程上。

下一主题[添加到源代码管理的内容](adding-content-to-source-control.md)，介绍如何将内容添加到源代码管理。

## <a name="further-reading"></a>其他阅读材料

有关在 TFS 中创建团队项目的更广泛指导，请参阅[创建团队项目](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx)。 使用户能够创建团队项目集合中的新团队项目的详细信息，请参阅[为团队项目集合设置管理员权限](https://msdn.microsoft.com/library/dd547204.aspx)。 将用户添加到团队项目的详细信息，请参阅[将用户添加到团队项目](https://msdn.microsoft.com/library/bb558971.aspx)。

> [!div class="step-by-step"]
> [上一页](configuring-team-foundation-server-for-web-deployment.md)
> [下一页](adding-content-to-source-control.md)
