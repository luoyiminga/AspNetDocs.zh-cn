---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: 创建自定义路由 (C#) |Microsoft Docs
author: microsoft
description: 了解如何将自定义路由添加到 ASP.NET MVC 应用程序。 在本教程中，您将学习如何修改 Global.asax 文件中的默认路由表。
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 58f72e390f0053d136ef00ddbda0b071ba225d98
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2019
ms.locfileid: "65123364"
---
# <a name="creating-custom-routes-c"></a>创建自定义路由 (C#)

by [Microsoft](https://github.com/microsoft)

> 了解如何将自定义路由添加到 ASP.NET MVC 应用程序。 在本教程中，您将学习如何修改 Global.asax 文件中的默认路由表。

在本教程中，您将学习如何将自定义的路由添加到 ASP.NET MVC 应用程序。 了解如何修改自定义的路由在 Global.asax 文件中的默认路由表。

对于许多简单的 ASP.NET MVC 应用程序，默认路由表将就可以正常工作。 但是，可能会发现有专门的路由的需要。 在这种情况下，可以创建自定义路由。

例如，假设您要生成的博客应用程序。 你可能想要处理传入的请求，如下所示：

/ 存档/12-25-2009 年

当用户输入此请求时，你想要返回到日期相对应的博客条目 2009 年 12 月 25 日。 若要处理这种类型的请求，需要创建一个自定义路由。

在列表 1 中的 Global.asax 文件包含一个新的自定义路由，名为博客，看起来像 /Archive/ 哪些句柄请求*条目日期*。

**代码清单 1-Global.asax （具有自定义路由）**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

您将添加到路由表路由的顺序非常重要。 我们新的自定义博客路由添加到之前的现有默认路由。 如果您颠倒顺序，则默认路由始终将获取调用而不是自定义路由。

自定义博客路由匹配的开头/存档/任何请求。 因此，它与匹配所有以下 Url:

- / 存档/12-25-2009 年

- / 存档/10-6-2004

- / 存档/apple

自定义的路由将传入请求映射到名为存档的控制器，并调用 Entry() 操作。 当调用 Entry() 方法时，条目日期在作为一个名为 entryDate 参数传递。

可以使用在代码清单 2 中的控制器使用博客自定义路由。

**代码清单 2-ArchiveController.cs**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

请注意，代码清单 2 中的 Entry() 方法接受类型为 DateTime 的参数。 MVC 框架非常智能，可自动将条目日期从 URL 转换 DateTime 值。 如果 URL 中的条目日期参数不能转换为日期时间，将引发错误 （请参阅图 1）。

**图 1-通过转换参数错误**

[![新建项目对话框](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)

**图 01**:通过转换参数的错误 ([单击此项可查看原尺寸图像](creating-custom-routes-cs/_static/image2.png))

## <a name="summary"></a>总结

本教程的目的是演示如何创建自定义路由。 您学习了如何将自定义的路由添加到表示博客条目在 Global.asax 文件中的路由表。 我们讨论了如何将请求的博客条目映射到名为 ArchiveController 的控制器和名为 Entry() 的控制器操作。

> [!div class="step-by-step"]
> [上一页](aspnet-mvc-controllers-overview-cs.md)
> [下一页](creating-a-route-constraint-cs.md)
