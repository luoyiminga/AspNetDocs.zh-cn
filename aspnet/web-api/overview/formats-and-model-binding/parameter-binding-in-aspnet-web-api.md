---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: ASP.NET Web API-ASP.NET 4.x 中的参数绑定
author: MikeWasson
description: 描述 Web API 如何绑定参数，以及如何在 ASP.NET 4.x 中自定义绑定过程。
ms.author: riande
ms.date: 07/11/2013
ms.custom: seoapril2019
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5386532ab581e023d93d16a5d4107e07f40b986f
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985829"
---
# <a name="parameter-binding-in-aspnet-web-api"></a>ASP.NET Web API 中的参数绑定

作者： [Mike Wasson](https://github.com/MikeWasson)

[!INCLUDE[](~/includes/coreWebAPI.md)]

本文介绍了 Web API 如何绑定参数，以及你如何自定义绑定过程。 Web API 在控制器上调用方法时，必须设置参数的值，此过程称为“绑定”。

默认情况下，Web API 使用以下规则进行参数绑定：

- 如果参数为“简单”类型，Web API 会尝试从 URI 中获取值。 简单类型包括 .NET[基元类型](https://msdn.microsoft.com/library/system.type.isprimitive.aspx)（**int**、 **bool**、 **double**等），以及 **TimeSpan**、**DateTime**、**Guid**、**decimal** 和 **string**，*此外还有*借助类型转换器即可从字符串进行转换的类型。 （稍后介绍有关类型转换器的详细信息。）
- 对于复杂类型，Web API 尝试使用[媒体类型格式化程序](media-formatters.md)从消息正文中读取值。

例如，下面是典型的 Web API 控制器方法：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

*Id* 参数是&quot;简单&quot;类型，因此 Web API 尝试从请求 URI 中获取值。 *item* 参数是复杂类型，因此 Web API 使用媒体类型格式化程序从请求正文中读取值。

为了从 URI 中获取值，Web API 会在路由数据和 URI 查询字符串中进行查找。 路由系统分析 URI 并将其与某个路由匹配时，会填充路由数据。 有关详细信息，请参阅[路由和操作选择](../web-api-routing-and-actions/routing-and-action-selection.md)。

本文其余部分介绍如何自定义模型绑定过程。 但是，对于复杂类型，请尽可能使用媒体类型格式化程序。 HTTP 的关键原则在于，资源在消息正文中发送，同时使用内容协商来指定资源表示形式。 媒体类型格式化程序正是专为此而设计的。

## <a name="using-fromuri"></a>使用 [FromUri]

若要强制 Web API 从 URI 读取复杂类型，请向参数添加 **[FromUri]** 特性。 下面的示例定义 `GeoPoint` 类型，以及从 URI 获取 `GeoPoint` 的控制器方法。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

客户端可以将纬度和经度值放在查询字符串中，Web API 会使用它们来构造`GeoPoint`。 例如：

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>使用 [FromBody]

若要强制 Web API 从请求正文读取简单类型，请向参数添加 **[FromBody]** 特性：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

在此示例中，Web API 会使用媒体类型格式化程序从请求正文读取 *name* 的值。下面是一个客户端请求示例。 下面是一个示例客户端请求。

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

当参数具有 [FromBody] 时，Web API 使用 Content-Type 标头来选择格式化程序。 在此示例中，内容类型是 &quot;application/json&quot;，请求正文是原始的 JSON 字符串（不是 JSON 对象）。

最多允许一个参数从消息正文中读取。因此以下代码不起作用： 这不起作用：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

之所以实施此规则，是因为请求正文可能存储在只能读取一次的非缓冲流中。

## <a name="type-converters"></a>类型转换器

可以创建 **TypeConverter** 并提供字符串转换，让 Web API 将一个类视为简单类型（这样 Web API 就会尝试将其从 URI 绑定）。

下面的代码演示一个表示地理点的 `GeoPoint` 类，以及一个可以从字符串转换为 **实例的**TypeConverter`GeoPoint`。 `GeoPoint` 类使用可指定类型转换器的 **[TypeConverter]** 特性来修饰。 此示例借鉴了 Mike Stall 的博文 [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)（如何在 MVC/WebAPI 中绑定到操作签名中的自定义对象）。）

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

现在 Web API 会将 `GeoPoint` 视为简单类型，这意味着它会尝试从 URI 绑定`GeoPoint` 类型的参数。 不需要在参数中包括 **[FromUri]** 。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

客户端可以使用如下所示的 URI 调用方法：

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>模型绑定器

类型转换器更灵活的选项是创建自定义模型绑定器。 可以通过模型绑定器访问 HTTP 请求、操作说明、路由数据中的原始值等内容。

创建模型绑定器需要实现 **IModelBinder** 接口。 此接口定义单个方法，即 **BindModel**：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

下面是 `GeoPoint` 对象的模型绑定器。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

模型绑定器从值提供程序获取原始输入值。 这种设计可以将两种不同的功能分隔开：

- 值提供程序获取 HTTP 请求并填充键值对字典。
- 模型绑定器使用此字典来填充模型。

Web API 中的默认值提供程序从路由数据和查询字符串中获取值。 例如，如果 URI 为`http://localhost/api/values/1?location=48,-122`，则值提供程序将创建以下键值对：

- id = &quot;1&quot;
- location = &quot;48122&quot;

(假设默认路由模板，这是&quot;api / {controller} / {id}&quot;。)

要绑定的参数的名称存储在 **ModelBindingContext.ModelName** 属性中。 模型绑定器会在字典中查找包含此值的键。 如果此值存在，并且可以转换为 `GeoPoint`，模型绑定器会将绑定的值赋给 **ModelBindingContext.Model** 属性。

请注意，模型绑定器不限于简单类型转换。 模型绑定器首先在包含已知位置的表中进行查找，如果失败，则会使用类型转换。

**设置模型绑定器**

可以通过多种方法来设置模型绑定器。 可以向参数添加 **[ModelBinder]** 特性

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

也可以向类型添加 **[ModelBinder]** 特性。 Web API 会对该类型的所有参数使用指定的模型绑定器。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

最后，可以将模型绑定器提供程序添加到 **HttpConfiguration**。 模型绑定器提供程序只是一个用于创建模型绑定器的工厂类。 可以通过从 [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) 类派生来创建提供程序。 但是，如果模型绑定器处理的是单一类型，则更简单的方法是使用内置的 **SimpleModelBinderProvider**，后者是专为此而设计的。 下面的代码演示如何执行此操作。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

使用模型绑定提供程序时，仍需向参数添加 **[ModelBinder]** 特性，目的是告知 Web API 使用模型绑定器，而不是使用媒体类型格式化程序。 不过，现在无需在特性中指定模型绑定器的类型：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>值提供程序

前面提到过，模型绑定器从值提供程序中获取值。 若要编写自定义值提供程序，请实现 **IValueProvider** 接口。 以下示例演示了如何在请求中从 Cookie 提取值：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

还需要通过从**ValueProviderFactory**类派生来创建值提供程序工厂。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

将值提供程序工厂添加到**HttpConfiguration** ，如下所示。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Web API 编写所有值提供程序，因此模型绑定器在调用 **ValueProvider.GetValue** 时，会从第一个能够生成该值的值提供程序接收值。

或者，可以使用**ValueProvider**属性在参数级别设置值提供程序工厂，如下所示：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

这会告知 Web API 使用具有指定值提供程序工厂的模型绑定，而不是使用任何其他已注册的值提供程序。

## <a name="httpparameterbinding"></a>HttpParameterBinding

模型绑定器是一个演示较通用机制的具体实例。 如果查看 **[ModelBinder]** 特性，你会发现它派生自抽象类 **ParameterBindingAttribute**。 此类定义的单个方法 **GetBinding** 可返回 **HttpParameterBinding** 对象：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

**HttpParameterBinding** 负责将参数绑定到值。 如果使用 **[ModelBinder]** ，此特性会返回一个 **HttpParameterBinding** 实现，该实现使用 **IModelBinder** 执行实际的绑定。 你还可以实现你自己的**HttpParameterBinding**。

例如，假设要从`if-match`请求中获取 etag 和`if-none-match`标头。 首先，我们将定义一个表示 Etag 的类。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

我们还将定义一个枚举，指示是从 `if-match` 标头还是从 `if-none-match` 标头获取 ETag。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

下面是一个 **HttpParameterBinding**，该类从所需标头获取 ETag，并将其绑定到类型为 ETag 的参数：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

**ExecuteBindingAsync**方法进行绑定。 在此方法中，将绑定参数值添加到**HttpActionContext**中的**ActionArgument**字典中。

> [!NOTE]
> 如果**ExecuteBindingAsync**方法读取请求消息的正文，请重写**WillReadBody**属性以返回 true。 请求正文可能是一次只能读取一次的未缓冲流，因此 Web API 强制执行最多一个绑定可以读取消息正文的规则。

若要应用自定义**HttpParameterBinding**，可以定义派生自**ParameterBindingAttribute**的属性。 对于`ETagParameterBinding`，我们将定义两个属性，一个`if-match`用于标头， `if-none-match`一个用于标头。 两者均派生自抽象基类。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

下面是使用`[IfNoneMatch]`属性的控制器方法。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

除了**ParameterBindingAttribute**以外，还有另一个挂钩用于添加自定义**HttpParameterBinding**。 在**HttpConfiguration**对象上， **ParameterBindingRules**属性是类型（**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**）的匿名函数的集合。 例如，可以添加 GET 方法上的任何 ETag 参数使用`ETagParameterBinding`的`if-none-match`规则：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

对于绑定不适用`null`的参数，函数应返回。

## <a name="iactionvaluebinder"></a>IActionValueBinder

整个参数绑定过程由可插入服务**IActionValueBinder**控制。 **IActionValueBinder**的默认实现执行以下操作：

1. 在参数上查找**ParameterBindingAttribute** 。 这包括 **[FromBody]** 、 **[FromUri]** 和 **[ModelBinder]** 或自定义属性。
2. 否则，请在**HttpConfiguration. ParameterBindingRules**中查找返回非 null **HttpParameterBinding**的函数。
3. 否则，请使用前面介绍的默认规则。 

    - 如果参数类型为 "simple" 或具有类型转换器，请从 URI 进行绑定。 这等效于将 **[FromUri]** 特性置于参数上。
    - 否则，请尝试从消息正文读取参数。 这等效于将 **[FromBody]** 放置在参数上。

如果需要，可以将整个**IActionValueBinder**服务替换为自定义实现。

## <a name="additional-resources"></a>其他资源

[自定义参数绑定示例](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Mike 停止了撰写有关 Web API 参数绑定的一系列优秀博客文章：

- [Web API 执行参数绑定的方式](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Web API 的 MVC 样式参数绑定](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [如何绑定到 MVC/Web API 中的操作签名中的自定义对象](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [如何在 Web API 中创建自定义值提供程序](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [后台的 Web API 参数绑定](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
