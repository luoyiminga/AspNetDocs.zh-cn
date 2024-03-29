---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: 使用 Web Farm Framework 创建服务器场 |Microsoft Docs
author: jrjlee
description: 本主题介绍如何使用 Web Farm Framework (WFF) 2.0 创建和配置 web 服务器场中服务器的集合。
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 204996514bed336e60ab77f184a923f04e7e2bba
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2019
ms.locfileid: "65106904"
---
# <a name="creating-a-server-farm-with-the-web-farm-framework"></a>使用 Web Farm Framework 创建服务器场

通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何使用 Web Farm Framework (WFF) 2.0 创建和配置 web 服务器场中服务器的集合。

WFF，可以跨多个负载平衡的 web 服务器同步 web 平台产品和组件、 web 应用程序、 网站和配置设置。 在方案中需要多个 web 服务器，例如过渡和生产环境，这可以极大地简化部署和配置过程。 可以部署到一台服务器的 web 应用程序&#x2014;*主服务器*&#x2014;和 WFF 都将自动复制在服务器场中的所有其他 web 服务器上该 web 应用程序。

## <a name="understanding-the-web-farm-framework"></a>了解 Web 场框架

WFF 2.0 可用于预配、 管理和将内容部署到一组 web 服务器。 WFF 部署包括三个关键的服务器角色：

- *控制器服务器*。 使用此服务器来创建和配置 WFF 服务器场。 控制器服务器管理的 web 平台组件、 配置设置和应用程序在服务器场的 web 服务器之间的同步。 在控制器服务器上，安装 WFF 2.0 和控制器服务器反过来将每个服务器场中的服务器上安装 WFF 代理。 控制器服务器从概念上讲不属于任何 WFF 服务器场中，并且单个控制器服务器可以管理多个服务器场。 在此方案中，使用单个 WFF 控制器服务器来创建和管理的过渡服务器场和生产服务器场。
- *主服务器*。 每个 WFF 服务器场包括一个主服务器。 当你安装 web 平台组件或应用程序部署到主服务器时，WFF 将同步到服务器场中的所有其他服务器所做的更改。
- *辅助服务器*。 每个 WFF 服务器场包含一个或多个辅助服务器。 对主服务器进行任何更改复制到服务器场中每个辅助服务器。

这将显示这些服务器角色如何与 Fabrikam，Inc.过渡和生产环境相关联：

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

在此方案中，在过渡环境和生产环境被配置为 WFF 服务器场。 单个 WFF 控制器服务器管理这两种场。 在每个服务器场，对主服务器的任何更改复制到每个辅助服务器。

在开始配置过渡和生产环境之前，我们建议阅读以下文章，熟悉 WFF 2.0 的关键概念：

- [IIS 7 的 Web 场框架 2.0 概述](https://go.microsoft.com/?linkid=9805126)
- [设置适用于 IIS 7 Web Farm Framework 2.0 的服务器场](https://go.microsoft.com/?linkid=9805127)
- [系统和 iis 7 Web Farm Framework 2.0 的平台要求](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>任务概述

若要完成的任务和本主题中的演练，你将需要至少三个服务器&#x2014;一个 WFF 控制器、 服务器场中，一个主 web 服务器和服务器场的一个或多个辅助 web 服务器。 您可以在任何时间将更多的辅助服务器添加到 WFF 服务器场中。 在高级别，来创建和配置，你需要在过渡或生产环境的 WFF 服务器场：

- 通过安装 Internet 信息服务 (IIS) 7.5 和 WFF 2.0 创建控制器服务器。
- 准备通过创建常见的管理员帐户和配置防火墙例外的主要和辅助服务器。
- 在控制器服务器上使用 IIS 管理器配置服务器场。
- 配置负载均衡使用 IIS 应用程序请求路由 (ARR) 或其他负载平衡技术。

任务和本主题中的演练假定您正在使用运行 Windows Server 2008 R2 的全新的服务器生成。 在开始为每个服务器之前，请确保：

- 安装 Windows Server 2008 R2 Service Pack 1 和所有可用的更新。
- 服务器已加入域。
- 在服务器具有静态 IP 地址。

> [!NOTE]
> 有关将计算机加入到域的详细信息，请参阅[将计算机加入到域并登录](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)。 有关配置静态 IP 地址的详细信息，请参阅[配置静态 IP 地址](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)。

## <a name="create-the-wff-controller-server"></a>创建 WFF 控制器服务器

若要创建 WFF 控制器服务器，你将需要安装 IIS 7 或更高版本和 WFF 2.0 或更高版本。 实际上，WFF 使用 IIS Web 部署工具 （Web 部署） 2.x 同步您的服务器场中的服务器。 如果使用 Web 平台安装程序来安装 WFF，安装程序将自动下载并安装适用于你的 Web 部署。

**若要创建 WFF 控制器服务器**

1. 下载并安装[Web 平台安装程序](https://go.microsoft.com/?linkid=9739157)。
2. 在顶部**Web 平台安装程序 3.0**窗口中，单击**产品**。
3. 在左侧和右侧的窗口中，在导航窗格中，单击**Server**。
4. 在 **IIS 7 建议配置** 行中，单击 **添加** 。
5. 在中<strong>Web 场框架 2。</strong><em>x</em>行中，单击<strong>添加</strong>。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. 单击“安装” 。 请注意，Web 平台安装程序具有 Web 部署工具，以及各种其他依赖项，添加到安装列表。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. 查看许可条款，如果同意条款，单击**我接受**。
8. 安装完成后，单击**完成**，然后关闭**Web 平台安装程序 3.0**窗口。

## <a name="configure-the-primary-and-secondary-servers"></a>配置主要和辅助服务器

在创建 WFF 服务器场之前，应完成一些准备任务将构成该场的 web 服务器上：

- 添加防火墙例外以允许**核心网络**，**远程管理**，并**文件和打印机共享**与 WFF 控制器服务器进行通信的功能.
- 创建域帐户 (例如， **FABRIKAM\stagingfarm**) 在 Active Directory 中将其添加到每个服务器上的本地管理员组。 创建服务器场时，您将使用此帐户作为服务器场管理员帐户。

有关如何在 Windows 防火墙中配置这些防火墙例外的详细信息，请参阅[系统要求和 IIS 7 Web Farm Framework 2.0 的平台要求](https://go.microsoft.com/?linkid=9805128)。 其他防火墙系统，请参阅产品文档。

下一步过程可用于将域帐户添加到 Windows Server 2008 R2 中的本地管理员组。 应在你想要添加到服务器场的每个服务器上执行此步骤&#x2014;换而言之，将相同的域帐户添加到主服务器和每个辅助服务器上的本地管理员组。

**若要将域帐户添加到本地管理员组**

1. 上**启动**菜单，依次指向**管理工具**，然后单击**服务器管理器**。
2. 在中**服务器管理器**窗口中的，在树视图窗格中，展开**配置**，展开**本地用户和组**，然后单击**组**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. 在中**组**窗格中，双击**管理员**。
4. 在中**Administrators 属性**对话框中，单击**添加**。
5. 在中**选择用户、 计算机、 服务帐户或组**对话框中，键入 （或浏览） 到您的域帐户 (例如， **FABRIKAM\stagingfarm**)，然后单击**确定**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. 在中**Administrators 属性**对话框中，单击**确定**。

你的服务器现已准备好添加到服务器场。 如果主服务器，可以配置该服务器以满足应用程序的需求之前或之后创建的服务器场&#x2014;在这两种情况下，WFF 将通过部署相同的产品、 组件或配置同步服务器向辅助服务器。 为简单起见，本教程假定已完成创建服务器场会配置主服务器。

## <a name="create-the-wff-server-farm"></a>创建 WFF 服务器场

此时，所有服务器都已准备好添加到 WFF 服务器场：

- 已在控制器服务器上安装 WFF。
- 已在主要和次要的 web 服务器上配置防火墙例外。
- 在主要和次要的 web 服务器上，已添加到本地管理员组的域帐户。

下一步是在 WFF 中创建服务器场。 您可以执行此操作从 IIS 管理器 WFF 控制器服务器上。

**若要创建 WFF 服务器场**

1. 在 WFF 控制器服务器上，在**启动**菜单，依次指向**管理工具**，然后单击**Internet 信息服务 (IIS) 管理器**。
2. 在中**连接**窗格中，展开本地服务器节点，右键单击**服务器场**，然后单击**创建服务器场**。
3. 在**创建服务器场**对话框框中，键入服务器场有意义的名称 (例如，**过渡场**)，然后选择**配置服务器场**。
4. 键入用户名和密码添加到每个服务器上的本地管理员组的域帐户。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. 单击 **“下一步”**。
6. 上**添加服务器**页上，键入完全限定的域名 (FQDN) 的主服务器，选择**主服务器**，然后单击**添加**。
7. 此时，WFF 将尝试联系主服务器使用你提供的凭据。 如果连接成功，将在添加到表的主服务器**添加服务器**页。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > 您可能已经注意到，**服务器是可用于负载平衡**默认处于选中状态。 WFF 使用 IIS ARR 模块来实现负载平衡，从而在服务器场中的 web 服务器间分发请求。 在大多数情况下，只会清除**服务器是可用于负载平衡**选项如果想要使用第三方负载平衡解决方案来代替。
8. 上**添加服务器**页上，键入第一个辅助服务器的 FQDN，然后单击**添加**。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. 为您的服务器场中的任何其他辅助服务器重复步骤 7，然后单击**完成**。

WFF 服务器场是正在运行。 任何 web 平台产品或主服务器和所有 web 应用程序或内容部署到主服务器安装的组件，将自动配置所有辅助服务器上。

WFF 是各种广泛而复杂的主题，并可以了解有关它的详细信息[适用于 IIS 7 的 Microsoft Web Farm Framework 2.0](https://go.microsoft.com/?linkid=9805129)网站。 目前，但是，有两个需要注意的功能区域：

- *应用程序预配*是跨服务器场中的所有辅助服务器从主服务器，例如 web 应用程序和配置设置复制内容的过程。 例如，如果联系人管理器示例解决方案部署到主暂存服务器时，WFF 应用程序预配过程将此将解决方案部署到所有辅助临时服务器。 默认情况下，应用程序预配过程在运行每隔 30 秒。
- *平台配置*是同步 web 平台产品和组件从主服务器到服务器场中的所有辅助服务器的过程。 例如，如果在主要的过渡服务器上安装 ASP.NET MVC 3，平台预配过程将使用 Web 平台安装程序在所有辅助临时服务器上安装 ASP.NET MVC 3。 默认情况下，平台预配过程运行每隔五分钟。

你可以管理的基本应用程序和平台预配设置从 IIS 管理器 WFF 控制器服务器上。

**浏览应用程序和平台配置设置**

1. 在 IIS 管理器中，在**连接**窗格中，选择你的服务器场。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. 在中**服务器场**窗格中，双击**应用程序预配**。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. 正如您所看到的当前配置的服务器场以同步每隔 30 秒的主服务器和辅助服务器之间的 web 内容和配置设置。
4. 单击**回**，然后双击**平台配置**。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. 正如您所看到的当前配置的服务器场来同步 web 平台产品和组件之间的主服务器和辅助服务器每隔五分钟。
6. 单击**回**。
7. 若要强制立即同步 web 平台产品的服务器场中**操作**窗格中，单击**预配平台**。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > 平台配置可能需要一些时间。 安装程序进程在后台运行在服务器场中的辅助服务器上。
8. 一旦已允许足够的时间才能完成预配过程，可以验证，产品和组件添加到主服务器现在已复制的辅助服务器上。 例如，您可以登录到辅助服务器上，使用**服务器管理器**窗口以验证是否已安装 web 服务器角色。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. 此外可以查看已安装的程序列表以验证已添加各种 web 平台组件。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>配置负载平衡

在创建 web 场时，你需要设置某种形式的负载平衡能够将 web 服务器之间的 HTTP 请求。 这可能是 Windows Server 2008 网络负载平衡，IIS ARR 或第三方基于软件的或基于硬件的负载平衡解决方案。

WFF 旨在与 IIS arr。 紧密集成 若要充分利用此集成，需要在 WFF 控制器服务器上安装 ARR 模块。 您然后定向到控制器服务器的所有 web 流量通常通过配置域名系统 (DNS) 记录。 控制器服务器然后会将分发在您的服务器场，基于服务器的可用性和各种其他条件的服务器之间的传入请求。

> [!NOTE]
> 无需使用 WFF; 使用 ARR你可以配置 WFF 使用第三方负载平衡解决方案。 有关详细信息，请参阅[概述适用于 IIS 7 Web Farm Framework 2.0](https://go.microsoft.com/?linkid=9805126)。

使用 ARR 负载平衡是一个复杂的主题，最的超出了本教程的范围。 但是，可以使用下一个过程中安装 ARR 模块并开始使用负载平衡。

**若要设置的 WFF 控制器服务器进行负载均衡**

1. WFF 控制器服务器上，启动 Web 平台安装程序。
2. 在顶部**Web 平台安装程序 3.0**窗口中，单击**产品**。
3. 在左侧和右侧的窗口中，在导航窗格中，单击**Server**。
4. 在中**应用程序请求路由 2.5**行中，单击**添加**。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. 单击**安装**，然后按照中的说明进行操作**Web 平台安装**窗口。
6. 安装完成后，启动 IIS 管理器中，然后在**连接**窗格中，单击你的服务器场节点。 请注意，有几个新图标已添加到**服务器场**窗格。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. 在中**服务器场**窗格中，双击**负载平衡**。
8. 在中**负载平衡**窗格中，选择一个负载平衡算法 (例如，**至少当前请求**)。

    > [!NOTE]
    > 有关负载平衡算法和其他配置设置的详细信息，请参阅[应用程序请求路由模块](https://go.microsoft.com/?linkid=9805130)。

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. 在中**操作**窗格中，单击**应用**。

你现在已配置基本负载均衡的服务器场中的服务器。 如果您所有在 web 场将流量定向到的控制器服务器，将根据可用性场中的服务器与所选的负载平衡算法之间分发请求。

有关如何使用 ARR 配置负载平衡的详细信息，请参阅[应用程序请求路由模块](https://go.microsoft.com/?linkid=9805130)。

## <a name="monitor-the-server-farm"></a>监视服务器场

你可以随时通过 IIS 管理器控制器服务器上监视服务器场的运行的状况。 在中**连接**窗格中，展开你的服务器场，然后单击**服务器**。 中心窗格中将显示的最近的活动的跟踪日志以及服务器场中的每个服务器的摘要。

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>结束语

启动并运行，现在应为 WFF 服务器场。 可以配置主服务器以支持无论你更喜欢哪种部署方法&#x2014;请参阅更多参考资料部分了解详细信息&#x2014;，且你的配置将复制在服务器场中每个辅助服务器上。

## <a name="further-reading"></a>其他阅读材料

有关配置和使用 WFF 的所有方面的更多指导，请参阅[适用于 IIS 7 的 Microsoft Web Farm Framework 2.0](https://go.microsoft.com/?linkid=9805129)网站。

> [!div class="step-by-step"]
> [上一页](configuring-a-database-server-for-web-deploy-publishing.md)
> [下一页](configuring-deployment-properties-for-a-target-environment.md)
