---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: 向数据库添加新项 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 692269a2c11e529af78f24feca74bba704b5b54b
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59415149"
---
# <a name="add-a-new-item-to-the-database"></a>向数据库添加一个新项

通过[Mike Wasson](https://github.com/MikeWasson)

[下载已完成的项目](https://github.com/MikeWasson/BookService)

在本部分中，将添加为用户创建新的通讯簿的功能。 在 app.js 中，将添加到视图模型的以下代码：

[!code-javascript[Main](part-9/samples/sample1.js)]

在 Index.cshtml，将为以下标记：

[!code-html[Main](part-9/samples/sample2.html)]

替换为：

[!code-html[Main](part-9/samples/sample3.html)]

此标记将创建用于提交新作者的窗体。 作者下拉列表的值是数据绑定到`authors`视图模型中可观察量。 对于其他窗体输入，值是数据绑定到`newBook`视图模型属性。

在窗体上的提交处理程序绑定到`addBook`函数：

[!code-html[Main](part-9/samples/sample4.html)]

`addBook`函数读取的数据绑定窗体输入创建一个 JSON 对象的当前值。 然后它将发布到的 JSON 对象`/api/books`。

> [!div class="step-by-step"]
> [上一页](part-8.md)
> [下一页](part-10.md)
