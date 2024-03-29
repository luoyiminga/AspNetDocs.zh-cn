---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: (包括 2011 年 4 月工具更新)ASP.NET MVC 3 是一个框架，用于构建可缩放的基于标准的 web 应用程序使用成熟设计模式...
ms.author: riande
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 0ff5e6244cb4c6ea15297272af85d91d469da6d0
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2019
ms.locfileid: "65121341"
---
# <a name="aspnet-mvc-3"></a>ASP.NET MVC 3

> *(包括 2011 年 4 月工具更新)*
> 
> ASP.NET MVC 3 是一个框架，用于构建可缩放的基于标准的 web 应用程序使用成熟设计模式和 ASP.NET 和.NET Framework 的强大功能。
> 
> 它将通过并行安装通过 ASP.NET MVC 2，以便开始立即使用 ！
> 
> 下载[此处安装程序](https://go.microsoft.com/fwlink/?LinkID=208140)

## <a name="top-features"></a>常用功能

- 通过 NuGet 可扩展集成搭建基架系统
- HTML 5 的启用了的项目模板
- 表达视图包括新的 Razor 视图引擎
- 使用依赖关系注入和全局操作筛选器的功能强大挂钩
- 丰富的 JavaScript 支持非介入式 JavaScript、 jQuery 验证，以及 JSON 绑定
- *读取完整功能列表[如下](#overview)*

## <a name="top-links"></a>顶部的链接

什么是 ASP.NET MVC 3 中的新增功能

- Phil Haack:[ASP.NET MVC 3 已发布](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman:[ASP.NET MVC3 WebMatrix、 NuGet、 IIS Express 和 Orchard 发布的上下文中的 Microsoft 年 1 月 Web 版本](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie:[宣布发布 ASP.NET MVC 3、 IIS Express、 SQL CE 4、 Web 场框架、 Orchard、 WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [ASP.NET MVC 3 的发行说明](../whitepapers/mvc3-release-notes.md)

安装和帮助

- 安装 ASP.NET MVC 3 使用[Web 平台安装程序 （推荐）](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- 安装 ASP.NET MVC 3 使用[安装程序可执行文件](https://go.microsoft.com/fwlink/?LinkID=208140)
- 安装[适用于 Visual Studio 11 开发者预览版的 ASP.NET MVC 3](https://go.microsoft.com/fwlink/?LinkID=208140)
- 读取[对 ASP.NET MVC 3 教程简介](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- 获取帮助和讨论中的 ASP.NET MVC 3[论坛](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>ASP.NET MVC 3 概述

ASP.NET MVC 3 上的 ASP.NET MVC 1 和 2，生成添加强大的功能，同时简化你的代码，并允许更深入的可扩展性。 本主题提供许多新功能包括在此版本中，划分为以下部分的概述：

- [使用 MvcScaffold 集成可扩展基架](#BM_MvcScaffolding)
- [HTML 5 的启用了的项目模板](#BM_HTML5)
- [Razor 视图引擎](#BM_TheRazorViewEngine)
- [对多个视图引擎的支持](#BM_Support_for_Multiple_View_Engines)
- [控制器的改进](#BM_Controller_Improvements)
- [JavaScript 和 Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [模型验证改进](#BM_Model_Validation_Improvements)
- [依赖关系注入的改进](#BM_Dependency_Injection_Improvements)
- [其他新增功能](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>使用 MvcScaffold 集成可扩展基架

新搭建基架系统轻松选择并开始高效地使用，如果你是全新到框架，还可以自动执行常见开发任务，如果您是经验丰富和已经知道自己在做什么。

这新的 NuGet 支持*基架*名为包**MvcScaffolding**。 术语"搭建基架"由许多软件技术来表示"快速生成可以在软件的基本概述然后编辑和自定义"。 我们正在创建 ASP.NET mvc 基架包是大有裨益几个方案中：

- **如果你第一次了解 ASP.NET MVC**，因为这样可以快速获取一些有用的工作代码，然后可以编辑，并根据需要调整。 它使您的查看空白页并不知道从何处着手缩短 ！
- **如果您非常熟悉 ASP.NET MVC，而且现在浏览外接程序的新的技术**如对象关系映射器、 视图引擎、 一个测试库等，因为这项技术的创建者可能还创建了基架程序包它。
- **如果您的工作涉及到反复创建相似的类或某种类型的文件**，因为您可以创建自定义输出测试装置、 部署脚本或任何其他所需的基架。 你的团队中的所有人也可以使用您自定义基架。

MvcScaffolding 中的其他功能包括：

- 对 C# 和 VB 项目的支持
- 支持 Razor 和 ASPX 视图引擎
- 支持为 ASP.NET MVC 区域的基架和使用自定义视图布局/母版
- 您可以轻松地将输出通过编辑自定义 T4 模板
- 您可以添加全新的基架使用 PowerShell 的自定义逻辑和自定义 T4 模板。 这些 （并向用户提供任何自定义参数） 会自动出现在控制台 tab 自动补全列表。
- 可以获取包含用于不同技术的其他框架的 NuGet 包 （例如，目前的概念一个 linq to SQL） 和混合和匹配它们一起

ASP.NET MVC 3 Tools Update 包括强大的 Visual Studio 支持此基架系统，如：

- 添加控制器对话框现在支持创建、 读取、 更新和删除控制器操作和相应的视图的完整自动基架。 默认情况下，这搭建基架以使用 EF Code First 数据访问代码。
- 添加控制器对话框支持*可扩展的基架*如通过 NuGet 包*MvcScaffolding*。 这允许插入到对话框，以允许你创建使用适用于 ODBCDirect 的其他数据访问技术，如 NHibernate 或甚至 JET 的基架，如果地自定义基架 ！

有关在 ASP.NET MVC 3 中的基架的详细信息，请参阅以下资源：

- Steve Sanderson 文章系列，包括： 

    1. [简介：创建 ASP.NET MVC 3 项目与 MvcScaffolding 包的基架](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [标准使用情况：典型用例和选项](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [一个对多关系](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [基架操作和单元测试](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [重写的 T4 模板](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [此文章：创建自定义基架](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- 来自 PDC 2010 会话的 Scott Hanselman 的博文[构建与 Microsoft"未命名包的 Web Love"博客](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [MVC 3 发行说明](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>HTML 5 项目模板

新建项目对话框中包括项目模板的复选框启用 HTML 5 版本。 这些模板利用 Modernizr 1.7，以提供对 HTML 和 CSS 3 在低级别浏览器中的兼容性支持。

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Razor 视图引擎

ASP.NET MVC 3 附带的新视图引擎名为 Razor 提供了以下好处：

- Razor 语法是干净且简洁，要求最小击键次数。
- Razor 是易于学习，部分因为它基于现有 C# 和 Visual Basic 等语言。
- Visual Studio 包含 Razor 语法的 IntelliSense 和代码着色。
- Razor 视图可以进行单元测试而无需运行该应用程序，或启动 web 服务器。

一些新 Razor 功能包括：

- `@model` 指定传递给视图的类型的语法。
- `@* *@` 注释语法。
- 若要指定默认值的功能 (如`layoutpage`) 为整个站点一次。
- `Html.Raw`方法而无需 HTML 编码的文本显示它。
- 对多个视图之间共享代码的支持 (*\_viewstart.cshtml*或 *\_viewstart.vbhtml*文件)。

Razor 还包括新的 HTML 帮助器，如下所示：

- `Chart`。 呈现的图表，产品/服务与 ASP.NET 4 中的图表控件相同的功能。
- `WebGrid`。 呈现数据网格中，完成，但分页和排序功能。
- `Crypto`。 使用哈希算法来创建正确加盐，哈希处理密码。
- `WebImage`。 呈现图像。
- `WebMail`。 发送电子邮件。

有关 Razor 的详细信息，请参阅以下资源：

- [Introducing Razor Scott Guthrie 的博客文章](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Scott Guthrie 的博客文章介绍@model关键字](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Scott Guthrie 的博客文章介绍 Razor 布局](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Razor API 快速参考](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [MVC 3 发行说明](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>对多个视图引擎的支持

**添加视图**对话框在 ASP.NET MVC 3 中的，可以选择你想要使用的视图引擎并**新项目**对话框可以指定一个项目的默认视图引擎。 您可以如选择 Web 窗体视图引擎 (ASPX)、 Razor 或开放源代码视图引擎[Spark](http://sparkviewengine.com/)， [NHaml](https://code.google.com/p/nhaml/)，或[NDjango](http://ndjango.org/)。

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>控制器的改进

### <a name="global-action-filters"></a>全局操作筛选器

有时你想要执行的逻辑操作方法运行之前或之后运行操作方法。 若要支持此功能，ASP.NET MVC 2 提供操作筛选器。 操作筛选器是提供一种声明性方式将操作前和操作后行为添加到特定控制器操作方法的自定义属性。 但是，在某些情况下您可能想要指定应用于所有操作方法的操作前或后操作行为。 MVC 3，可以通过将它们添加到指定的全局筛选器`GlobalFilters`集合。 有关全局操作筛选器的详细信息，请参阅以下资源：

- [MVC 3 Preview 上 Scott Guthrie 的博客](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [在 ASP.NET MVC 中进行筛选](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>新的"ViewBag"属性

MVC 2 控制器支持`ViewData`属性，可用于将数据传递给视图模板使用后期绑定字典 API。 在 MVC 3 中，您还可以使用稍微简单一些语法与`ViewBag`属性来完成相同的目的。 例如，而不是编写`ViewData["Message"]="text"`，可以编写`ViewBag.Message="text"`。 不需要定义要使用任何强类型化的类`ViewBag`属性。 因为它是动态属性，而是，可以只需获取或设置属性并将它们动态在运行时解析。 在内部，`ViewBag`属性存储为名称/值对中`ViewData`字典。 (注意： 在 MVC 3 的大多数预发布版本`ViewBag`属性的名称为`ViewModel`属性。)

### <a name="new-actionresult-types"></a>新的"ActionResult"类型

以下`ActionResult`类型和相应的帮助器方法是 MVC 3 中新增或增强：

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx)。 返回到客户端的 HTTP 状态代码为 404。
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx)。 返回临时重定向 （HTTP 302 状态代码） 或根据布尔参数的永久重定向 （HTTP 301 状态代码）。 与此更改后，结合[控制器](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx)类现在有三种方法来执行永久重定向： `RedirectPermanent`， `RedirectToRoutePermanent`，和`RedirectToActionPermanent`。 这些方法返回的实例`RedirectResult`与`Permanent`属性设置为`true`。
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx)。 返回用户指定的 HTTP 状态代码。

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>JavaScript 和 Ajax 改进

默认情况下，在 MVC 3 中的 Ajax 和验证帮助程序使用的非介入式 JavaScript 方法。 非介入式 JavaScript 可避免将内联 JavaScript 注入到 HTML。 这使您的 HTML 较小，越小混乱，并使其更轻松地换出或自定义 JavaScript 库。 在 MVC 3 中的验证帮助程序还使用`jQueryValidate`默认情况下的插件。 如果要使用 MVC 2 行为，则可以禁用非介入式 JavaScript 使用*web.config*文件设置。 有关 JavaScript 和 Ajax 改进的详细信息，请参阅以下资源：

- [维基百科网站上的非介入式 JavaScript 的基本简介](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Brad Wilson 的非介入式 JavaScript Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Brad Wilson 的非介入式 JavaScript 验证 Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [使用 Razor 和非介入式 JavaScript 创建 MVC 3 应用程序](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md)（ASP.NET 网站教程）
- [MVC 3 发行说明](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>默认情况下启用的客户端验证

在早期版本的 MVC，您需要显式调用`Html.EnableClientValidation`从一个视图，以启用客户端验证的方法。 在 MVC 3 中这是不再需要因为默认情况下启用客户端验证。 (您可以禁用此使用中的设置*web.config*文件。)

为了使客户端端验证正常工作，仍然需要引用相应 jQuery 和 jQuery 验证站点中的库。 可以在你自己的服务器上托管这些库，也可以从 Microsoft 或 Google Cdn 等内容分发网络 (CDN) 中引用它们。

### <a name="remote-validator"></a>远程验证程序

ASP.NET MVC 3 支持新[RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx)类，可用于利用 jQuery 验证插件中的远程验证程序的支持。 这使客户端验证库，可自动调用服务器端的服务器定义以执行才可进行的验证逻辑的自定义方法。

在以下示例中，`Remote`属性指定客户端验证将调用名为操作`UserNameAvailable`上`UsersController`类使用，以便验证`UserName`字段。

[!code-csharp[Main](mvc3/samples/sample1.cs)]

下面的示例显示了相应的控制器。

[!code-csharp[Main](mvc3/samples/sample2.cs)]

有关如何使用详细信息`Remote`属性，请参阅[如何：在 ASP.NET MVC 中实现远程验证](https://msdn.microsoft.com/library/gg508808(VS.98).aspx)MSDN 库中。

### <a name="json-binding-support"></a>JSON 绑定支持

ASP.NET MVC 3 包括使操作方法来接收 JSON 编码数据和模型绑定其操作方法参数的内置 JSON 绑定支持。 此功能是在涉及客户端模板和数据绑定方案中有用。 （客户端模板，您可以格式化和显示单个数据项组使用客户端执行的模板。）MVC 3 可轻松地将客户端模板连接在服务器上的发送和接收 JSON 数据的操作方法。 有关 JSON 绑定支持的详细信息，请参阅**JavaScript 和 AJAX 改进**一部分[Scott Guthrie 的 MVC 3 预览版博客文章](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)。

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>模型验证改进

### <a name="dataannotations-metadata-attributes"></a>"DataAnnotations"元数据属性

ASP.NET MVC 3 支持`DataAnnotations`元数据属性，如`DisplayAttribute`。

### <a name="validationattribute-class"></a>"ValidationAttribute"类

`ValidationAttribute`类在.NET Framework 4，以支持新改进了`IsValid`重载提供了有关当前验证上下文，如哪些对象正在进行验证的详细信息。 这使您可以在其中验证基于模型的另一个属性的当前值更丰富的方案。 例如，新`CompareAttribute`属性可用于比较两个模型的属性的值。 在以下示例中，`ComparePassword`属性必须与匹配`Password`是有效的字段。

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>验证接口

[IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx)接口可用于执行模型级别验证，并允许您以提供特定于整个模型，或在模型中的两个属性之间的状态的错误消息的验证. MVC 3 现在将检索来自错误`IValidatableObject`接口时模型绑定和自动标志或突出显示受影响的字段中使用内置的 HTML 窗体帮助器的视图。

[IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx)接口使 ASP.NET MVC，若要在运行时发现验证程序是否对客户端验证的支持。 此接口设计，以便它可以与多种验证框架集成。

有关验证接口的详细信息，请参阅**模型验证改进**一部分[Scott Guthrie 的 MVC 3 预览版博客文章](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)。 （但是，请注意，对"IValidateObject"博客中的引用应"IValidatableObject"。）

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>依赖关系注入的改进

ASP.NET MVC 3 应用依赖关系注入 (DI) 和将与依赖关系注入或控制反转 (IOC) 容器提供更好的支持。 在以下区域中添加了对 DI 支持：

- 控制器 （注册和注入控制器工厂，将注入控制器）。
- 视图 （注册和注入视图引擎，将依赖项注入到视图页）。
- 操作筛选器 （定位和注入筛选器）。
- 模型联编程序 （注册和注入）。
- 模型验证程序提供程序 （注册和注入）。
- 模型元数据提供程序 （注册和注入）。
- 值提供程序 （注册和注入）。

MVC 3 支持[Common Service Locator](https://github.com/unitycontainer/commonservicelocator)库并支持该库的任何 DI 容器`IServiceLocator`接口。 它还支持新`IDependencyResolver`的界面，可轻松地集成 DI 框架。

有关 MVC 3 中的 DI 的详细信息，请参阅以下资源：

- [Brad Wilson 的系列的服务位置上的博客文章](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [MVC 3 发行说明](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>其他新增功能

### <a name="nuget-integration"></a>NuGet 集成

ASP.NET MVC 3 会自动安装，并为其安装程序的一部分启用 NuGet。 NuGet 是免费的开放源代码程序包管理器，轻松地查找、 安装和在项目中使用.NET 库和工具。 它适用于所有 Visual Studio 项目类型 （包括 ASP.NET Web 窗体和 ASP.NET MVC）。

NuGet 支持开发人员维护的开放源代码项目 （例如，像 Moq、 NHibernate、 Ninject、 StructureMap、 NUnit、 Windsor、 RhinoMocks 和 Elmah 的项目） 其库打包并在联机库中注册它们。 这样，它将简单的.NET 开发人员想要使用这些库之一来查找包并将其安装在他们正在处理的项目中。

ASP.NET 3 工具更新后，项目模板包含 JavaScript 库预安装的 NuGet 包，因此它们可通过 NuGet 更新。 Entity Framework 代码优先也会预安装 NuGet 包的形式。

有关 NuGet 的详细信息，请参阅 [NuGet 文档](https://docs.microsoft.com/nuget/)。

### <a name="partial-page-output-caching"></a>部分页面输出缓存

ASP.NET MVC 具有支持输出缓存的整页响应之后的版本 1。 MVC 3 还支持部分页面输出缓存，使您能够轻松地缓存区域或响应的片段。 有关缓存的详细信息，请参阅**部分页面输出缓存**一部分[Scott Guthrie 的博客文章在 MVC 3 候选发布版](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx)和**子操作输出缓存**一部分[MVC 3 发行说明](../whitepapers/mvc3-release-notes.md)。

### <a name="granular-control-over-request-validation"></a>精细地控制请求验证

ASP.NET MVC 的自动帮助防范 XSS 和 HTML 注入攻击的内置请求验证。 但是，有时要显式禁用请求验证，例如，如果你想要允许用户发布 HTML 内容 （例如，在博客文章或 CMS 内容）。 现在，您可以添加[AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx)到模型属性，或查看模型，从而禁用基于每个属性在模型绑定期间请求验证。 有关请求验证的详细信息，请参阅以下资源：

- **非介入式 JavaScript 和验证**主题中[Scott Guthrie 的博客文章在 MVC 3 候选发布版](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx)。
- [MVC 3 发行说明](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>可扩展的"新建项目"对话框

可以在 ASP.NET MVC 3 中添加项目模板，视图引擎和单元测试项目到框架**新的项目**对话框。

### <a name="template-scaffolding-improvements"></a>模板基架的改进

ASP.NET MVC 3 基架模板更好地确定模型上的主键属性并适当地比在早期版本的 MVC 处理。 （例如，基架模板现在请确保主键不基架为可编辑窗体字段。）

默认情况下，现在使用的创建和编辑基架`Html.EditorFor`帮助器而不是`Html.TextBoxFor`帮助器。 这提高了支持的数据窗体中的模型的元数据批注特性何时**添加视图**对话框将生成一个视图。

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>"Html.LabelFor"和"Html.LabelForModel"的新重载

已添加的新方法重载`LabelFor`和`LabelForModel`帮助器方法。 新重载，可指定或重写的标签文本。

### <a name="sessionless-controller-support"></a>无会话控制器支持

在 ASP.NET MVC 3 中可以指示是否想要使用会话状态，控制器类，如果是，是否会话状态应为读/写或只读的。 有关无会话控制器支持的详细信息，请参阅[MVC 3 发行说明](../whitepapers/mvc3-release-notes.md)。

### <a name="new-additionalmetadataattribute-class"></a>"AdditionalMetadataAttribute"的新类

可以使用[AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx)属性来填充`ModelMetadata.AdditionalValues`模型属性的字典。 例如，如果视图模型具有一个属性，它应仅向管理员显示，你可以批注该属性，如下面的示例中所示：

[!code-csharp[Main](mvc3/samples/sample4.cs)]

呈现产品视图模型时，此元数据将提供到任何显示或编辑器的模板。 它是由您来解释元数据信息。

### <a name="accountcontroller-improvements"></a>AccountController 的改进

AccountController Internet 项目模板中的得到了显著改善。

### <a name="new-intranet-project-template"></a>新建 Intranet 项目模板

一个新的 Intranet 项目模板，包括其启用 Windows 身份验证，并删除 AccountController。
