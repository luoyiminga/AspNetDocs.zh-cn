---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: 操作和 OData v4 中的函数使用 ASP.NET Web API 2.2 |Microsoft Docs
author: MikeWasson
description: 在 OData 中，操作和函数是一种方法来添加未轻松地定义为对实体的 CRUD 操作的服务器端行为。 本教程演示如何...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: f5af94e93e5b7f2351d40febbf1a468d635c9db1
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2019
ms.locfileid: "65133154"
---
# <a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>操作和使用 ASP.NET Web API 2.2 OData v4 中的函数

通过[Mike Wasson](https://github.com/MikeWasson)

> 在 OData 中，操作和函数是一种方法来添加未轻松地定义为对实体的 CRUD 操作的服务器端行为。 本教程演示如何将操作和函数添加到使用 Web API 2.2 的 OData v4 终结点。 本教程在本教程以[创建 OData v4 终结点使用 ASP.NET Web API 2](create-an-odata-v4-endpoint.md)
>
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
>
> - Web API 2.2
> - OData v4
> - Visual Studio 2013 (下载 Visual Studio 2017[此处](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>教程版本
>
> OData 版本 3，请参阅[ASP.NET Web API 2 中的 OData 操作](../odata-v3/odata-actions.md)。

之间的差异*操作*并*函数*是操作可能会有副作用，并且函数不这样做。 操作和函数可以返回数据。 操作的一些用途包括：

- 复杂的事务处理。
- 在一次处理多个实体。
- 允许仅对实体的某些属性的更新。
- 不是一个实体的发送数据。

函数可用于返回直接向某一实体或集合的信息不对应。

操作 （或函数） 可以针对单个实体或集合。 在 OData 术语中，这是*绑定*。 此外可以让&quot;未绑定&quot;操作/函数，作为服务上的静态操作调用。

## <a name="example-adding-an-action"></a>示例:添加操作

让我们来定义要对产品评估的操作。

> [!NOTE]
> 本教程基于本教程[创建 OData v4 终结点使用 ASP.NET Web API 2](create-an-odata-v4-endpoint.md)

首先，添加`ProductRating`模型来表示分级。

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

此外将添加**DbSet**到`ProductsContext`基类，因此 EF 将在数据库中创建的分级表。

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>将操作添加到 EDM

在 WebApiConfig.cs 中，添加以下代码：

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

**EntityTypeConfiguration.Action**方法将操作添加到实体数据模型 (EDM)。 **参数**方法指定操作的类型化的参数。

此代码还设置 EDM 的命名空间。 命名空间很重要，因为操作的 URI 包含完全限定的操作名称：

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> 在典型的 IIS 配置，此 URL 中的点将导致 IIS 返回 404 错误。 可以通过将以下部分添加到 Web.Config 文件来解决此问题：

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>为操作添加一个控制器方法

若要启用&quot;速率&quot;操作，添加以下方法`ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

请注意方法名与操作名称相匹配。 **[HttpPost]** 属性指定该方法是 HTTP POST 方法。

若要调用的操作，客户端发送一个 HTTP POST 请求，如下所示：

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

&quot;速率&quot;操作绑定到 Product 实例，因此该操作的 URI 是追加到 URI 的实体的完全限定的操作名称。 (回想一下，我们设置为 EDM 命名空间&quot;ProductService&quot;，因此完全限定的操作名称是&quot;ProductService.Rate&quot;。)

请求正文包含作为 JSON 有效负载的操作参数。 Web API 会自动将转换为 JSON 有效负载**ODataActionParameters**对象，它是只是参数值的字典。 使用此字典来访问你的控制器方法中的参数。

如果客户端发送错误的操作参数，值的格式设置**ModelState.IsValid**为 false。 检查此标志在控制器方法中的，如果返回错误**IsValid**为 false。

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>示例:添加函数

现在让我们添加返回最贵的产品的 OData 函数。 如之前，第一步将函数添加到 EDM。 在 WebApiConfig.cs 中，添加以下代码。

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

在这种情况下，此函数绑定到产品集合，而不是各个实例的产品。 客户端通过发送 GET 请求来调用此函数：

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

下面是此函数的控制器方法：

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

请注意，方法名称的函数名称匹配。 **[HttpGet]** 属性指定的方法是 HTTP GET 方法。

下面是 HTTP 响应：

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>示例:添加一个未绑定的函数

上一个示例是绑定到集合的函数。 在下一个示例，我们将创建*未绑定*函数。 作为服务上的静态操作称为未绑定的函数。 在此示例中该函数将返回针对给定的邮政编码销售税。

在 WebApiConfig 文件中，添加到 EDM 函数：

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

请注意，我们将调用**函数**是直接对**ODataModelBuilder**，而不是实体类型或集合。 这将告知模型生成器的函数是未绑定。

下面是实现该函数的控制器方法：

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

并不重要的 Web API 控制器将放在此方法。 可以将它放`ProductsController`，或定义单独的控制器。 **[ODataRoute]** 属性定义了该函数的 URI 模板。

下面是示例客户端请求：

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

HTTP 响应：

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
