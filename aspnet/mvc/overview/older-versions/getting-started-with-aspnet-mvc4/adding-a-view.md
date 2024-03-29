---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: 添加视图 |Microsoft Docs
author: Rick-Anderson
description: 注意:本教程中的更新的版本提供了使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全、 更易于遵循，并演示...
ms.author: riande
ms.date: 08/28/2012
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 58dc8baf3f2e8e3cf412c0f9c7d9355f933c89d9
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2019
ms.locfileid: "65129967"
---
# <a name="adding-a-view"></a>添加视图

通过[Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > 本教程中的更新的版本是可用[此处](../../getting-started/introduction/getting-started.md)，它使用 ASP.NET MVC 5 和 Visual Studio 2013。 它是更安全、 更易于遵循，并演示更多的功能。

在本部分中想要修改`HelloWorldController`类，以使用视图模板文件来顺利封装生成 HTML 响应向客户端的过程。

使用视图模板文件，您将创建[Razor 视图引擎](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)使用 ASP.NET MVC 3 引入了。 基于 razor 的视图模板具有 *.cshtml*文件扩展名，并提供创建 HTML 输出使用 C# 的简洁方法。 Razor 字符和击键时编写视图模板，所需的数量降至最低，并启用编码工作流即快速、 流畅。

当前，`Index` 方法返回带有在控制器类中硬编码的消息的字符串。 更改`Index`方法以返回`View`对象，如下面的代码中所示：

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

`Index`上述方法使用视图模板生成到浏览器的 HTML 响应。 控制器方法 (也称为[操作方法](http://rachelappel.com/asp.net-mvc-actionresults-explained))，如`Index`上面，方法通常返回[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (或从派生的类[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx))，字符串等不基元类型。

在项目中，添加可用于查看模板`Index`方法。 若要执行此操作，右键单击内`Index`方法，并单击**添加视图**。

![](adding-a-view/_static/image1.png)

**添加视图**对话框随即出现。 保留默认值的方式变，并单击**添加**按钮：

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld*文件夹并*MvcMovie\Views\HelloWorld\Index.cshtml*创建文件。 你可以看到它们在**解决方案资源管理器**:

![](adding-a-view/_static/image3.png)

如下所示*Index.cshtml*已创建的文件：

![HelloWorldIndex](adding-a-view/_static/image4.png)

添加以下 HTML 下的`<h2>`标记。

[!code-html[Main](adding-a-view/samples/sample2.html)]

完整*MvcMovie\Views\HelloWorld\Index.cshtml*文件如下所示。

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

如果使用的 Visual Studio 2012 中，在解决方案资源管理器中，右键单击*Index.cshtml*文件，然后选择**在 Page Inspector 中的查看**。

![PI](adding-a-view/_static/image5.png)

[Page Inspector 教程](../../views/using-page-inspector-in-aspnet-mvc.md)包含有关此新工具的详细信息。

或者，运行该应用程序并浏览到`HelloWorld`控制器 (`http://localhost:xxxx/HelloWorld`)。 `Index`控制器中的方法未做大量的工作; 它只需运行该语句`return View()`，其指定该方法应使用视图模板文件来呈现到浏览器的响应。 由于未显式指定要使用的视图模板文件的名称，ASP.NET MVC 的默认设置为使用 *Index.cshtml* 视图中的文件 *\Views\HelloWorld* 文件夹。 下图显示了字符串&quot;Hello from our View Template ！&quot;硬编码的视图中。

![](adding-a-view/_static/image6.png)

看起来相当棒。 但是，请注意，浏览器的标题栏显示&quot;索引我 ASP.NET A&quot; ，并在页面顶部的大链接显示&quot;此处显示的徽标。&quot;下面&quot;你徽标此处。&quot;链接大约的注册和日志中的链接，以及下面链接到主页，并向页。 让我们更改其中一些。

## <a name="changing-views-and-layout-pages"></a>更改视图和布局页面

首先，你想要更改&quot;你徽标此处。&quot;在页面顶部的标题。 该文本是通用的每一页。 它是实际实现只能在一个位置，在项目中，即使它在应用程序中每一页显示。 转到 */视图/共享*中的文件夹**解决方案资源管理器**，然后打开 *\_Layout.cshtml*文件。 此文件称为*布局页面*共享，并且&quot;shell&quot;所有其他页使用的。

![_LayoutCshtml](adding-a-view/_static/image7.png)

布局模板可用于在一个位置指定您的网站的 HTML 容器布局，然后将其应用于你的站点中多个页面。 查找 `@RenderBody()` 行。 `RenderBody` 是显示创建的所有特定于视图的页面的占位符，已包装在布局页面中&quot;&quot;。 例如，如果选择关于链接*Views\Home\About.cshtml*内呈现视图`RenderBody`方法。

更改从布局模板中的站点标题标头&quot;你在此处显示徽标&quot;到&quot;MVC 电影&quot;。

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

标题元素的内容替换为以下标记：

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

运行应用程序，请注意，它现在显示&quot;MVC 电影&quot;。 单击**有关**链接，你会看到该页面的显示方式&quot;MVC 电影&quot;、 过。 我们能够在布局模板中一次进行更改，并具有在站点上的所有页面都反映新标题。

![](adding-a-view/_static/image8.png)

现在，让我们来更改索引视图的标题。

打开*MvcMovie\Views\HelloWorld\Index.cshtml*。 有两个位置进行更改： 首先，文本显示的标题中的浏览器中，然后在辅助标头 (`<h2>`元素)。 稍稍对它们进行一些更改，这样可以看出哪一段代码更改了应用的哪部分。

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

若要指示显示集上面的代码的 HTML 标题`Title`的属性`ViewBag`对象 (后者位于*Index.cshtml*视图模板)。 如果回顾一下布局模板的源代码，您会注意到，该模板使用此值在`<title>`元素作为的一部分`<head>`我们以前修改 HTML 的一部分。 使用此`ViewBag`方法时，你可以轻松地传递其他参数视图模板和布局文件之间。

运行应用程序，并浏览到`http://localhost:xx/HelloWorld`。 请注意，浏览器标题、主标题和辅助标题已更改。 （如果没有在浏览器中看到更改，则可能正在查看缓存的内容。 在浏览器中按 Ctrl + F5 强制加载来自服务器的响应。）使用创建浏览器标题`ViewBag.Title`中设置*Index.cshtml*查看模板和其他&quot;-电影应用&quot;添加布局文件中。

此外会注意到了中的内容*Index.cshtml*视图的模板已与合并 *\_Layout.cshtml*视图模板和单个 HTML 响应已发送到浏览器。 凭借布局模板可以很容易地对应用程序中所有页面进行更改。

![](adding-a-view/_static/image9.png)

我们一小段&quot;数据&quot;(在这种情况下&quot;Hello from our View Template ！&quot;消息) 不过是硬编码。 MVC 应用程序具有&quot;V&quot; （视图） 并且您已获得&quot;C&quot; （控制器），但没有&quot;M&quot; （模型） 尚未。 稍后，我们将演练如何创建数据库并从它检索模型数据。

## <a name="passing-data-from-the-controller-to-the-view"></a>将数据从控制器传递给视图

我们转到数据库，并讨论模型之前，不过，我们首先讨论一下将信息从控制器传递给视图。 控制器类调用以响应传入的 URL 请求。 控制器类是响应的编写处理传入浏览器的代码的请求、 从数据库检索数据，并最终决定将哪些类型发送回浏览器的位置。 查看模板然后可从控制器来生成并格式化对浏览器的 HTML 响应。

控制器负责提供使视图模板能够呈现到浏览器的响应所需的任何数据或对象。 最佳做法：**视图模板应永远不会执行业务逻辑或直接与数据库交互**。 相反，视图模板应使用仅由控制器提供给它的数据。 维护这&quot;关注点分离&quot;有助于保持代码干净、 可测试且更易于维护。

目前，`Welcome`操作方法中的`HelloWorldController`类采用`name`和一个`numTimes`参数，然后输出直接向浏览器的值。 而不是使控制器将作为一个字符串，此响应呈现，让我们更改控制器以改为使用视图模板。 视图模板将生成动态响应，这意味着你需要将适当的数据位从控制器传递给视图以生成响应。 可以为此，需要查看模板的动态数据 （参数） 放置在控制器`ViewBag`视图模板稍后可以访问的对象。

返回到*HelloWorldController.cs*文件，并更改`Welcome`方法以添加`Message`并`NumTimes`值设为`ViewBag`对象。 `ViewBag` 是一个动态对象，这意味着您可以随意设置其中;`ViewBag`对象具有定义的属性，直到你将内容放在它。 [ASP.NET MVC 模型绑定系统](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)会自动将映射命名的参数 (`name`和`numTimes`) 从方法中的参数到地址栏中的查询字符串。 完整的 HelloWorldController.cs 文件如下所示：

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

现在`ViewBag`对象包含将自动传递给视图的数据。

接下来，需要一个欢迎使用视图模板 ！ 在中**构建**菜单中，选择**生成 MvcMovie**以确保编译项目。

然后在右键单击`Welcome`方法，并单击**添加视图**。

![](adding-a-view/_static/image10.png)

下面是什么**添加视图**对话框如下所示：

![](adding-a-view/_static/image11.png)

单击**外**，然后添加以下代码`<h2>`中的新元素*Welcome.cshtml*文件。 你将创建一个循环，说&quot;Hello&quot;根据用户所说它应多次。 完整*Welcome.cshtml*文件如下所示。

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

运行应用程序并浏览到以下 URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

数据取自 URL 并传递给控制器使用现在[模型联编程序](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)。 控制器将数据打包到`ViewBag`对象并将该对象传递给视图。 该视图然后会以 html 格式的数据向用户显示。

![](adding-a-view/_static/image12.png)

在上面的示例中，我们使用`ViewBag`对象将数据从控制器传递到视图。 在本教程中后, 一种，我们将使用视图模型将数据从控制器传递到视图。 将数据传递到视图模型方法通常更方法相比是首选视图包。 请参阅博客文章[动态 V 强类型化视图](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)有关详细信息。

当然，这是一种类型的&quot;M&quot;模型，而不是数据库类。 让我们用学到的内容来创建一个电影数据库。

> [!div class="step-by-step"]
> [上一页](adding-a-controller.md)
> [下一页](adding-a-model.md)
