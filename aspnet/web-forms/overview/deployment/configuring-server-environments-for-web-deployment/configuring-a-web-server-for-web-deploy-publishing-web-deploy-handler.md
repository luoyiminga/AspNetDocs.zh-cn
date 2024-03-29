---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: 配置 Web 服务器的 Web 部署发布 （Web 部署处理程序） |Microsoft Docs
author: jrjlee
description: 本主题介绍如何配置 Internet 信息服务 (IIS) web 服务器以支持 web 发布和使用 IIS Web 部署 Han 部署...
ms.author: riande
ms.date: 01/29/2017
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: 51a8fdf44199b5a4735e0e00657639b191f51255
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2019
ms.locfileid: "65125984"
---
# <a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a>配置用于 Web 部署发布的 Web 服务器（Web 部署处理程序）

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何配置 Internet 信息服务 (IIS) web 服务器以支持 web 发布和部署使用 IIS Web 部署处理程序。
> 
> 在处理 Web Deploy 2.0 或更高版本时，有三种主要方法可用于获取你的应用程序或 web 服务器上的站点。 你可以：
> 
> - 使用*Web 部署远程代理服务*。 这种方法需要较少配置 web 服务器，但您需要提供的本地服务器管理员凭据才能将任何内容部署到服务器。
> - 使用*Web 部署处理程序*。 这种方法更复杂，需要更多的初始工作来设置 web 服务器。 但是，在使用此方法时，你可以配置 IIS 以允许非管理员用户来执行部署。 Web 部署处理程序只是在 IIS 7 或更高版本中可用。
> - 使用*离线部署*。 这种方法需要 web 服务器的最低配置，但服务器管理员必须手动复制到服务器上的 web 包并将其导入通过 IIS 管理器。
> 
> 主要功能、 优势和一种方法的缺点的详细信息，请参阅[选择右方法对 Web 部署](choosing-the-right-approach-to-web-deployment.md)。

是，如果你想要允许非管理员用户将内容部署到特定的 IIS 网站。 这种方法通常是这些类型的方案中所需的：

- 过渡或生产环境中，会触发远程部署的人员或服务帐户是不太可能有权访问的服务器管理员凭据。
- 托管的环境中，你想要使远程用户能够更新自己的网站，而不为他们提供 web 服务器 （或对其他任何人的网站的访问） 的完全控制。

在开发或测试方案中，或在较小的组织中，使用服务器管理员凭据的部署内容通常是小于争用资源。 在这些情况下，在 web 服务器配置为支持部署使用的[Web 部署远程代理服务](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)提供更直接的方法。

## <a name="task-overview"></a>任务概述

若要配置 web 服务器来接受和部署 web 包从远程计算机使用 Web 部署处理程序方法，将需要：

- 创建或选择域用户帐户 （"非管理员用户"） 将用来执行部署其凭据。
- 安装 IIS 7.5，包括 Web 管理服务和基本身份验证模块。
- 安装 Web 部署 2.1 或更高版本。
- 配置 Web 管理服务，以允许远程连接，并启动服务。
- 创建一个 IIS 网站来承载部署的内容。
- 授予你对你的网站在 IIS 管理器的非管理员用户权限。
- 确保委派规则允许要添加和更改网站内容使用非管理员用户帐户的服务的 Web 管理服务。
- 配置所有防火墙以允许端口 8172 上的传入连接。

若要专门托管 ContactManager 示例解决方案，还需要为：

- 安装.NET Framework 4.0。
- 安装 ASP.NET MVC 3。

本主题将演示如何执行每个这些过程。 任务和本主题中的演练假定您正在使用运行 Windows Server 2016 的全新服务器内部版本。 在继续之前，请确保：

- Windows 2016 Server
- 服务器已加入域。
- 在服务器具有静态 IP 地址。

> [!NOTE]
> 有关将计算机加入到域的详细信息，请参阅[将计算机加入到域并登录](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)。 有关配置静态 IP 地址的详细信息，请参阅[配置静态 IP 地址](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)。

## <a name="install-products-and-components"></a>安装的产品和组件

本部分将指导您完成在 web 服务器上安装所需的产品和组件。 在开始之前，一个好的做法是运行 Windows 更新，以确保你的服务器是完全保持最新。

在这种情况下，你需要安装以下事项：

- **IIS 7 推荐配置**。 这使得**Web 服务器 (IIS)** 角色在 web 服务器上的和安装的 IIS 模块和托管的 ASP.NET 应用程序所需的组件组。
- **IIS:管理服务**。 将在 IIS 中安装 Web 管理服务 (WMSvc)。 此服务使您能够远程管理 IIS 网站，并公开给客户端的 Web 部署处理程序终结点。
- **IIS:基本身份验证**。 这会安装 IIS 基本身份验证模块。 此允许 Web 管理服务 (WMSvc) 进行身份验证所提供的凭据。
- **Web 部署工具 2.1 或更高版本**。 这在你的服务器上安装 Web 部署 （和其基础可执行文件，MSDeploy.exe）。 作为此过程的一部分，它将安装 Web 部署处理程序和与 Web 管理服务。
- **.NET framework 4.0**。 这是运行此版本的.NET Framework 生成的应用程序所需要的。
- **ASP.NET MVC 3**。 这会安装您需要运行 MVC 3 应用程序的程序集。

> [!NOTE]
> 本演练介绍如何使用 Web 平台安装程序来安装和配置各种组件。 尽管不一定要使用 Web 平台安装程序，它通过简化了安装过程会自动检测的依赖关系以及确保始终获得最新的产品版本。 有关详细信息，请参阅[Microsoft Web 平台安装程序](https://go.microsoft.com/?linkid=9805118)。

**若要安装必需的产品和组件**

1. 下载并安装[Web 平台安装程序](https://go.microsoft.com/?linkid=9805118)。
2. 安装完成后，Web 平台安装程序将自动启动。

    > [!NOTE]
    > 现在可以在任何时候从启动 Web 平台安装程序**启动**菜单。 为此，请在**启动**菜单上，单击**所有程序**，然后单击**Microsoft Web 平台安装程序**。
3. 在顶部**Web 平台安装程序**窗口中，单击**产品**。
4. 在左侧和右侧的窗口中，在导航窗格中，单击**框架**。
5. 在中**Microsoft.NET Framework 4**行，如果尚未安装.NET Framework，单击**添加**。

    > [!NOTE]
    > 你可能已安装.NET Framework 4.0 到 Windows 更新。 如果已安装的产品或组件，Web 平台安装程序将此信息指示通过替换**外**按钮，其文本**已安装**。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. 在中**ASP.NET MVC 3 (Visual Studio 2010)** 行中，单击**添加**。
7. 在导航窗格中，单击**Server**。
8. 在 **IIS 7 建议配置** 行中，单击 **添加** 。
9. 在中**Web 部署工具 2.1**行中，单击**添加**。
10. 在**IIS:基本身份验证**行中，单击**添加**。
11. 在**IIS:管理服务**行中，单击**添加**。
12. 单击“安装” 。 Web 平台安装程序将显示产品列表&#x2014;以及任何关联的依赖关系&#x2014;安装，并将提示你接受许可条款。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. 查看许可条款，如果同意条款，单击**我接受**。
14. 安装完成后，单击**完成**，然后关闭**Web 平台安装程序**窗口。

如果在安装 IIS 之前安装.NET Framework 4.0，您将需要运行[ASP.NET IIS 注册工具](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx)(aspnet\_regiis.exe) 若要向 IIS 注册 ASP.NET 的最新版本。 如果不这样做，您会发现 IIS 将 （如 HTML 文件） 中提供静态内容没有任何问题，但它将返回**HTTP 错误 404.0-未找到**尝试浏览到 ASP.NET 内容时。 下一步过程可用于确保在注册 ASP.NET 4.0。

**若要向 IIS 注册 ASP.NET 4.0**

1. 单击**启动**，然后键入**命令提示符下**。
2. 在搜索结果中，右键单击**命令提示符**，然后单击**以管理员身份运行**。
3. 在命令提示符窗口中， 导航到 **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** 目录。
4. 键入以下命令，然后按 Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. 如果计划托管 64 位 web 应用程序，任何时候，则应该向 IIS 注册 ASP.NET 的 64 位版本。 为此，请在命令提示符窗口中，导航到 **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** 目录。
6. 键入以下命令，然后按 Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

很好的做法是，Windows 更新再次使用此时若要下载并安装新的产品和组件已安装所有可用更新。

## <a name="configure-the-web-management-service"></a>配置 Web 管理服务

至此，已安装所需的一切下, 一步是在 IIS 中配置 Web 管理服务。 在高级别中，将需要完成以下任务：

- 启用基本身份验证在服务器级别。
- Web 管理服务配置为接受远程连接。
- 启动 Web 管理服务。
- 检查所需的 Web 管理服务委派规则已到位。

**若要配置 Web 管理服务**

1. 上**启动**菜单，依次指向**管理工具**，然后单击**Internet Information Services (IIS) Manager**。
2. 在 IIS 管理器中，在**连接**窗格中，单击服务器节点 (例如， **STAGEWEB1**)。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. 在中心窗格中下, **IIS**，双击**身份验证**。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image20.png)
4. 右键单击**基本身份验证**，然后单击**启用**。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. 在中**连接**窗格中，单击服务器节点，以返回到顶级设置。
6. 在中心窗格中下,**管理**，双击**管理服务**。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. 在中心窗格中，选择**启用远程连接**。

    > [!NOTE]
    > 如果已在运行 Web 管理服务，你将需要首先停止。
8. 在中**操作**窗格中，单击**启动**启动 Web 管理服务。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. 如果系统提示保存设置，请单击**是**。

    > [!NOTE]
    > 您可能想要将服务配置为自动启动。 若要执行此操作，打开服务控制台，右键单击**Web Management Service**，然后单击**属性**。 在中**启动类型**下拉列表中选择**自动**，然后单击**确定**。
10. 在中**连接**窗格中，单击服务器节点，以返回到顶级设置。
11. 在中心窗格中下,**管理**，双击**管理服务的委派**。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. 验证在中心窗格包含一组规则。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    这些规则允许经授权的 Web Management Service 用户使用各种 Web Deploy 提供程序。 例如，若要部署到通过 Web 部署处理程序的 IIS web 应用程序和内容，必须有一个允许所有经过身份验证 Web Management Service 用户使用的委派规则**contentPath**和**iisApp**提供程序 （您所见的屏幕截图中的最后一个规则）。

    如果你的产品和组件的顺序安装本主题中所述，最新版本的 Web 部署自动应将所有所需的委派规则添加到 Web 管理服务。 如果管理服务的委派页未显示任何规则，你将需要自行创建。 有关如何执行此操作的说明，请参阅[配置 Web 部署处理程序](https://go.microsoft.com/?linkid=9805124)。
13. 在中**连接**窗格中，单击服务器节点，以返回到顶级设置。

## <a name="create-and-configure-an-iis-website"></a>创建和配置 IIS 网站

可以将 web 内容部署到你的服务器之前，您需要创建和配置 IIS 网站来承载的内容。 Web 部署可以仅将 web 包部署到现有 IIS 网站;它不能为您创建的网站。 此外需要执行一些额外的配置，以允许非管理员帐户将远程部署内容。 在高级别中，将需要完成以下任务：

- 在文件系统来托管你的内容上创建一个文件夹。
- 创建 IIS 网站提供内容，并将其与本地文件夹关联。
- 授予读取权限的本地文件夹上的应用程序池标识。
- 授予对部署的 web 应用程序的域帐户所需的 IIS 权限。

尽管没有什么措施可以阻止您从内容部署到 IIS 中的默认网站，但不建议使用此方法用于测试或演示方案之外的任何内容。 若要模拟生产环境中，应使用特定于应用程序的要求的设置创建新的 IIS 网站。

**若要创建的 IIS 网站**

1. 在本地文件系统上创建一个文件夹以存储你的内容 (例如， **C:\DemoSite**)。
2. 上**启动**菜单，依次指向**管理工具**，然后单击**Internet Information Services (IIS) Manager**。
3. 在 IIS 管理器中，在**连接**窗格中，展开服务器节点 (例如， **STAGEWEB1**)。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. 右键单击**站点**节点，并单击**添加网站**。
5. 在中**站点名称**框中，键入为 IIS 网站的名称 (例如， **DemoSite**)。
6. 在中**物理路径**框中，键入 （或浏览到） 到本地文件夹的路径 (例如， **C:\DemoSite**)。
7. 在中**端口**框中，键入你想要托管网站的端口号 (例如， **85**)。

    > [!NOTE]
    > 标准端口号是 80 用于 HTTP 和 HTTPS 的 443。 但是，如果托管该网站在端口 80 上的，你需要停止默认网站，然后才能访问你的站点。
8. 将保留**主机名**框保留为空，除非你想要配置该网站的域名系统 (DNS) 记录，然后单击**确定**。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > 在生产环境中，你将很可能想要托管你的网站在端口 80 上并配置主机标头，以及匹配的 DNS 记录。 在 IIS 7 中配置主机标头的详细信息，请参阅[为网站 (IIS 7) 配置主机标头](https://technet.microsoft.com/library/cc753195(WS.10).aspx)。 Windows Server 中的 DNS 服务器角色的详细信息，请参阅[DNS 服务器概述](https://technet.microsoft.com/en-gb/library/cc770392.aspx)并[DNS 服务器](https://technet.microsoft.com/windowsserver/dd448607)。
9. 在中**操作**窗格下**编辑站点**，单击**绑定**。
10. 在中**站点绑定**对话框中，单击**添加**。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. 在中**添加网站绑定**对话框中，将**IP 地址**并**端口**以匹配您现有的站点配置。
12. 在中**主机名**框中，键入你的 web 服务器的名称 (例如， **STAGEWEB1**)，然后单击**确定**。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > 第一个站点绑定使你可以访问站点使用的 IP 地址和端口在本地或`http://localhost:85`。 第二个站点绑定使你可以使用计算机名称在域上的其他计算机访问站点 (例如， http://stageweb1:85)。
13. 在中**站点绑定**对话框中，单击**关闭**。
14. 在中**连接**窗格中，单击**应用程序池**。
15. 在中**应用程序池**窗格中，右键单击你的应用程序池的名称，然后单击**基本设置**。 默认情况下，应用程序池的名称将与你的网站的名称匹配 (例如， **DemoSite**)。
16. 在中 **.NET CLR 版本**列表中，选择 **.NET CLR v4.0.30319**，然后单击**确定**。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image21.png)

    > [!NOTE]
    > 示例解决方案需要.NET Framework 4.0。 这不是必需的 Web 部署一般情况下。

为了使你的网站提供内容，应用程序池标识必须具有读取存储内容的本地文件夹的权限。 在 IIS 7.5 应用程序池以运行唯一的应用程序池标识 （与以前版本的 IIS，其中应用程序池通常运行使用网络服务帐户） 默认情况下。 应用程序池标识不是真实的用户帐户和未出现在任何用户或组的列表&#x2014;相反，它动态启动时创建的应用程序池。 每个应用程序池标识添加到本地**IIS\_IUSRS**隐藏项的安全组。

若要授予对文件或文件夹上的应用程序池标识的权限，您有两个选项：

- 将权限分配给应用程序池标识直接，使用格式<strong>IIS AppPool\</strong ><em>[应用程序池名称]</em>(例如， <strong>IIS AppPool\DemoSite</strong>).
- 向其分配权限**IIS\_IUSRS**组。

最常用的方法是将权限分配给本地**IIS\_IUSRS**组，因为这种方法允许您更改应用程序池，而无需重新配置的文件系统权限。 下一个过程中使用此基于组的方法。

> [!NOTE]
> 在 IIS 7.5 的应用程序池标识的详细信息，请参阅[应用程序池标识](https://go.microsoft.com/?linkid=9805123)。

**若要配置 IIS 网站的文件夹权限**

1. 在 Windows 资源管理器，浏览到本地文件夹的位置。
2. 右键单击文件夹，然后依次**属性**。
3. 上**安全**选项卡上，单击**编辑**，然后单击**添加**。
4. 单击**位置**。 在中**位置**对话框中，选择本地服务器，然后单击**确定**。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. 在中**选择用户或组**对话框中，键入**IIS\_IUSRS**，单击**检查名称**，然后单击**确定**。
6. 在中<strong>的权限</strong><em>[文件夹名称]</em>对话框中，请注意，新的组已分配<strong>读取&amp;执行</strong>，<strong>列出文件夹内容</strong>，并<strong>读取</strong>默认情况下的权限。 保留此保持不变，然后单击<strong>确定</strong>。
7. 单击<strong>确定</strong>以关闭<em>[文件夹名称]</em><strong>属性</strong>对话框。

作为最后一项任务，必须为非管理员用户将用来将内容部署其凭据授予适当的权限。 此用户需要远程将内容部署到你的网站的权限。

**若要配置的 IIS 网站权限的非管理员域用户**

1. 在 IIS 管理器中，在**连接**窗格中，右键单击你网站的节点 (例如， **DemoSite**)，指向**部署**，然后单击**配置 Web部署发布**。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. 在中**配置 Web 部署发布**对话框中的，右侧**选择要授予发布权限的用户**列表中，单击省略号按钮。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. 在中**允许用户**对话框框中，键入你想要使用以部署内容，然后单击的帐户的域和用户**确定**。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. 在中**配置 Web 部署发布**对话框中，单击**安装程序**。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > 此操作在一个步骤中执行两个关键功能。 首先，它会根据上一部分中检查委派规则授予用户权限来修改 Web 管理服务，通过远程网站。 其次，它会授予用户完全控制的网站，这样用户就可以添加、 修改和设置权限的网站内容的源文件夹。
5. 在中**配置 Web 部署发布**对话框中，单击**关闭**。

## <a name="configure-firewall-exceptions"></a>配置防火墙例外

默认情况下，IIS Web 管理服务侦听 TCP 端口 8172。 如果 web 服务器上启用 Windows 防火墙，您将需要创建一个新入站的规则以允许端口 8172 上的 TCP 流量 （默认情况下，Windows 防火墙中允许所有出站流量）。 如果使用第三方防火墙，你将需要创建规则以允许流量。

| 方向 | 从端口 | 到端口 | 端口类型 |
| --- | --- | --- | --- |
| 入站 | 任意 | 8172 | TCP |
| 出站 | 8172 | 任意 | TCP |

在 Windows 防火墙中配置规则的详细信息，请参阅[配置防火墙规则](https://technet.microsoft.com/library/dd448559(WS.10).aspx)。 对于第三方防火墙，请参阅产品文档。

## <a name="conclusion"></a>结束语

你的 web 服务器现在应已准备好接受远程部署到 Web 部署处理程序中通过 Web 管理服务。 在尝试部署到服务器的 web 应用程序之前，你可能想要检查这些关键点：

- 已启用服务器级别在 IIS 中的基本身份验证？
- 已启用远程连接到 Web 管理服务？
- 已启动 Web 管理服务？
- 是否有管理服务委派规则在位置？
- 没有应用程序池标识具有读取访问权限的源文件夹的你的网站？
- 非管理员用户帐户是否在 IIS 中拥有站点级别的权限？
- 你的防火墙是否允许传入连接到 TCP 端口 8172 上的服务器？

## <a name="further-reading"></a>其他阅读材料

有关如何配置自定义 Microsoft Build Engine (MSBuild) 项目文件，以将 web 包部署到 Web 部署处理程序的指导，请参阅[配置目标环境的部署属性](configuring-deployment-properties-for-a-target-environment.md)。

> [!div class="step-by-step"]
> [上一页](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
> [下一页](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
