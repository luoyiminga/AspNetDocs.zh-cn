---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: 添加控制器 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 6b38d757d37374b14979f8a079a46158ff64f9c3
ms.sourcegitcommit: f774732a3960fca079438a88a5472c37cf7be08a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2019
ms.locfileid: "68810775"
---
# <a name="adding-a-controller"></a>添加控制器

作者: [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

MVC 代表*模型-视图-控制器*。 MVC 是用于开发应用程序的一种模式, 该模式设计良好、可测试且易于维护。 基于 MVC 的应用程序包含:

- **M**模式:类, 这些类表示应用程序的数据, 并使用验证逻辑来强制执行该数据的业务规则。
- **V**视图:应用程序用于动态生成 HTML 响应的模板文件。
- **C**控制器:处理传入浏览器请求、检索模型数据, 然后指定将响应返回到浏览器的视图模板的类。

我们将在本系列教程中介绍所有这些概念, 并向您演示如何使用它们来生成应用程序。

首先, 让我们创建一个控制器类。 在**解决方案资源管理器**中, 右键单击 "*控制器*" 文件夹, 然后依次单击 "**添加**"、"**控制器**"。

![](adding-a-controller/_static/image1.png)

在 "**添加基架**" 对话框中, 单击 " **MVC 5 控制器-空**", 然后单击 "**添加**"。

![](adding-a-controller/_static/image2.png)  

将新控制器命名为 "HelloWorldController", 并单击 "**添加**"。

![添加控制器](adding-a-controller/_static/image3.png)

请注意,**解决方案资源管理器**创建了一个名为*HelloWorldController.cs*的新文件和一个新的文件夹*Views\HelloWorld*。 控制器在 IDE 中处于打开状态。

![](adding-a-controller/_static/image4.png)

将该文件的内容替换为以下代码。

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

控制器方法将以 HTML 形式返回一个字符串作为示例。 控制器名`HelloWorldController`为, 第一种方法名为`Index`。 让我们从浏览器中调用它。 运行应用程序 (按 F5 或 Ctrl + F5)。 在浏览器中, &quot;将&quot; HelloWorld 追加到地址栏中的路径。 (例如, 在下图中, 它是`http://localhost:1234/HelloWorld.`) 浏览器中的页面将类似于以下屏幕截图所示。 在上面的方法中, 代码直接返回了一个字符串。 你已告诉系统只返回了一些 HTML, 但确实返回了!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC 根据传入 URL 调用不同的控制器类 (以及其中的不同操作方法)。 ASP.NET MVC 使用的默认 URL 路由逻辑使用如下格式来确定要调用的代码:

`/[Controller]/[ActionName]/[Parameters]`

在*应用\_启动/RouteConfig*文件中设置路由格式。

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

当你运行应用程序但不提供任何 URL 段时, 它将默认为 "Home" 控制器和上述代码的 "默认值" 部分中指定的 "索引" 操作方法。

URL 的第一部分确定要执行的控制器类。 因此, */HelloWorld*映射到`HelloWorldController`类。 URL 的第二部分确定要执行的类的操作方法。 因此, */HelloWorld/Index*将导致`Index`执行`HelloWorldController`类的方法。 请注意, 我们只需要浏览到 */HelloWorld* , 并`Index`在默认情况下使用该方法。 这是因为, 名`Index`为的方法是将在控制器上调用的默认方法, 如果未显式指定一个方法。 URL 段的第三部分 (`Parameters`) 针对的是路由数据。 稍后, 我们将在本教程中看到路由数据。

浏览到 `http://localhost:xxxx/HelloWorld/Welcome`。 此方法将运行并返回该&quot;字符串, 这是欢迎操作方法 ... `Welcome`&quot;. 默认 MVC 映射为`/[Controller]/[ActionName]/[Parameters]`。 对于此 URL，采用 `HelloWorld` 控制器和 `Welcome` 操作方法。 目前尚未使用 URL 的 `[Parameters]` 部分。

![](adding-a-controller/_static/image6.png)

让我们略微修改该示例, 以便可以将一些参数信息从 URL 传递到控制器 (例如, */HelloWorld/Welcome？ name = Scott&amp;numtimes = 4*)。 `Welcome`将方法更改为包含两个参数, 如下所示。 请注意, 该代码使用C#可选的参数功能, 指示如果没有`numTimes`为该参数传递值, 则该参数应默认为1。

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> 安全说明:上面的代码使用[httputility.javascriptstringencode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx)保护应用程序免受恶意输入 (即 JavaScript) 的攻击。 有关详细信息，请参阅[操作说明：通过将 HTML 编码应用于字符串](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx)来防范 Web 应用程序中的脚本攻击。

 运行应用程序并浏览到示例 URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`)。 可在 URL 中对 `name` 和 `numtimes` 使用其他值。 [ASP.NET MVC 模型绑定系统](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)自动将命名参数从地址栏中的查询字符串映射到方法中的参数。

![](adding-a-controller/_static/image7.png)

在上面的示例中, 未使用 URL `Parameters`段 () `name` , 且和`numTimes`参数作为[查询字符串](http://en.wikipedia.org/wiki/Query_string)进行传递。 ? 上述 URL 中的 (问号) 是一个分隔符, 查询字符串如下所示。 &amp; 字符用于分隔查询字符串。

将欢迎方法替换为以下代码:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

运行应用程序并输入以下 URL:`http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

这次第三个 URL 段与 route 参数`ID.`匹配`Welcome`时, 操作方法包含与`ID` `RegisterRoutes`方法中的 URL 规范匹配的参数 ()。

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

在 ASP.NET MVC 应用程序中, 更常见的做法是将参数作为路由数据 (就像上面的 ID 那样) 传递到查询字符串。 你还可以添加路由, 以将`name`和`numtimes`传入参数作为 URL 中的路由数据。 在*App\_Start\RouteConfig.cs*文件中, 添加 "Hello" 路由:

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

运行应用程序并浏览到`/localhost:XXX/HelloWorld/Welcome/Scott/3`。

![](adding-a-controller/_static/image9.png)

对于许多 MVC 应用程序而言, 默认路由是正常的。 你将在本教程的后面部分了解如何使用模型绑定器传递数据, 并且你无需修改该的默认路由。

在这些示例中, 控制器已执行&quot;MVC 的 VC&quot;部分, 即视图和控制器工作。 控制器将直接返回 HTML。 通常情况下, 你不希望控制器直接返回 HTML, 因为这会对代码非常麻烦。 我们通常会使用单独的视图模板文件来帮助生成 HTML 响应。 接下来, 我们来看看如何实现此目的。

> [!div class="step-by-step"]
> [上一页](getting-started.md)
> [下一页](adding-a-view.md)
