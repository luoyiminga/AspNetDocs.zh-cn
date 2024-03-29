---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: 在 Visual Studio 2012 中使用 Page Inspector |Microsoft Docs
author: rick-anderson
description: 在本动手实验中，你会发现新的工具来查找和修复 Visual Studio-Page Inspector 中的 web 页的问题。 Page Inspector 是一个新工具： b...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f42b1be2697ba7d1145b3e334fe8f4ebf019cd12
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2019
ms.locfileid: "65133557"
---
# <a name="using-page-inspector-in-visual-studio-2012"></a>在 Visual Studio 2012 中使用 Page Inspector

通过[Web 训练营团队](https://twitter.com/webcamps)

> 在本动手实验中，你会发现新的工具来查找和修复 Visual Studio-Page Inspector 中的 web 页的问题。
> 
> Page Inspector 是一个新工具，Visual studio 将浏览器诊断工具，并提供在浏览器、 ASP.NET 和源代码之间的集成的体验。 它呈现网页 （HTML、 Web 窗体、 ASP.NET MVC 或网页） 直接在 Visual Studio IDE 中，并允许您检查源代码和生成的输出。 Page Inspector，可轻松地分解网站、 快速生成从零开始的页面，并快速诊断问题。
> 
> 现在我们有许多及时地，如 ASP.NET MVC 和 WebForms 创建灵活且可缩放的网站的 Web 框架。 但是，它获取更难发现的页上的问题，因为 IDE 不支持在基于模板的页面和动态内容中的设计器视图。 因此，这些网站必须打开在浏览器中查看它们如何向用户显示。
> 
> Web 开发人员使用外部工具发现定期运行在浏览器中的问题。 然后，它们返回到 IDE，并着手修复。 这来回 IDE、 浏览器和分析工具之间的活动可能效率很低，并且有时需要全新部署和清除每个希望以重现某个问题的时间的缓存。
> 
> Page Inspector 通过组合在一起的使用组合的功能集这两个优势来桥接客户端 （浏览器工具） 和服务器 （ASP.NET 和源代码代码） 之间的 Web 开发中存在间隔。
> 
> 使用 Page Inspector，您可以看到源文件 （包括服务器端代码） 中的哪些元素具有生成在浏览器中呈现的 HTML 标记。 Page Inspector 还允许您修改 CSS 属性和 DOM 元素特性，若要查看立即反映在浏览器中的更改。
> 
> 本动手实验将向您介绍 Page Inspector 功能并向您展示如何使用它们来修复 Web 应用程序中的问题。 **此实验室中包含两个练习使用类似的流，但面向不同的技术。如果你是 ASP.NET MVC 开发人员，完成练习如果您是两个 WebForms 开发人员按照练习**。
> 
> 此实验室将引导你通过前面所述通过将细微的更改应用于源文件夹中提供的示例 Web 应用的新功能和增强功能。
> 
> 在 Web 训练营培训工具包中，可在包含所有示例代码和代码段[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)。

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目标

在本动手实验，您将学习如何：

- 分解使用 Page Inspector 的 Web 站点
- 检查并预览使用 Page Inspector 的 CSS 样式更改
- 检测和修复使用 Page Inspector 在网页中的问题

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>系统必备

必须具有要完成本实验的以下项：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更高 (读取[附录 A](#AppendixA)有关如何安装它的说明)。
- Internet Explorer 9 或更高版本

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>练习

本动手实验包括以下练习：

1. [练习 1:在 ASP.NET MVC 项目中使用 Page Inspector](#Exercise1)
2. [练习 2:在 WebForms 项目中使用 Page Inspector](#Exercise2)

> [!NOTE]
> 每个练习均附带位于该练习，可用于完成独立于其他每个练习的 Begin 文件夹中的起始解决方案。 在练习的源代码，还会发现一个 End 文件夹，其中包含具有无法完成相应练习中的步骤得到的代码的 Visual Studio 解决方案。 如果您在演练本动手实验需要更多帮助，可以使用这些解决方案作为指南。

估计的时间才能完成此实验：**30 分钟**。

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>练习 1：在 ASP.NET MVC 项目中使用 Page Inspector

在此练习中，您将学习如何预览和调试**ASP.NET MVC 4**解决方案： 使用**Page Inspector**。 首先，您将执行简要了解该工具来了解促进 Web 调试进程的功能。 然后，您可以在包含样式设置问题的网页中。 您将学习如何使用 Page Inspector 找到生成问题的源代码和修复此错误。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>任务 1-浏览页面检查器

在本任务中，您将学习如何在显示照片库的 ASP.NET MVC 4 项目的上下文中使用 Page Inspector。

1. 打开**开始**解决方案位于**源/Ex1-MVC4/开始/** 文件夹。

   1. 需要先下载某些缺少的 NuGet 包然后再继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
   2. 在中**管理 NuGet 包**对话框中，单击**还原**若要下载缺少的包。
   3. 最后，通过单击生成解决方案**构建** | **生成解决方案**。

      > [!NOTE]
      > 使用 NuGet 的优点之一是，您不必提供在项目中的所有库项目大小减少。 使用 NuGet 增强工具，请通过在 Packages.config 文件中，指定包版本可以下载第一次运行该项目的所有所需的库。 这就是原因需要将从本实验中打开现有解决方案后运行这些步骤。
2. 在解决方案资源管理器，找到**Index.cshtml**下查看 **/views/home**项目文件夹，右键单击它并选择**在 Page Inspector 中的查看**。

    ![选择一个文件，若要预览在 Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image1.png "选择要预览在 Page Inspector 中的文件")

    *选择要预览在 Page Inspector 中的文件*
3. Page Inspector 窗口将显示 */Home/Index* URL 映射到所选的视图的源。

    ![使用 PageInspector 第一次联系](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *使用 Page Inspector 第一次联系*

    Page Inspector 工具已集成在 Visual Studio 环境中。 该检查器包含嵌入式浏览器，以及功能强大的 HTML 探查器。 请注意，不需要运行解决方案，以查看您的页面的外观。

    > [!NOTE]
    > 当 Page Inspector 浏览器宽度小于所打开的页的宽度时，您不会正确地看到页面。 如果发生这种情况，调整页面检查器的宽度。
4. 单击**文件**在 Page Inspector 中的选项卡。

    您将看到正在撰写的索引页的所有源文件。 此功能有助于识别一眼的所有元素，尤其是当你正在使用分部视图和模板。 请注意，您还可以打开每个文件，是否单击的链接。

    ![The-Files-tab](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *文件选项卡*
5. 单击**切换检查模式下**按钮，位于左侧的选项卡。

    此工具会让您选择的页的任何元素，并查看其 HTML 和 Razor 代码。

    ![Toggle-Inspection-Mode-button](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *切换检查模式按钮*
6. 在 Page Inspector 浏览器中，将鼠标指针移动页面元素。 将鼠标指针移过所呈现页中的任何部分，而显示的元素类型和 Visual Studio 编辑器中突出显示相应的源标记或代码。

    ![在操作中检查模式](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *在操作中检查模式*

    > [!NOTE]
    > 不最大化页面检查器窗口或你将无法再看到显示的源代码的预览选项卡。 如果它最大化时单击 Page Inspector 中的元素，将显示所选内容的源代码，但它会隐藏页面检查器窗口。

    如果您注意到**Index.cshtml**文件中，你会注意到，突出显示的源代码的生成所选的元素的部分。 此功能便于编辑长源代码文件，提供指导支持和快速的方法来访问代码。

    ![检查元素](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *检查元素*
7. 单击**切换检查模式下**按钮 (![选择 HTML 选项卡以显示在 Page Inspector 浏览器中呈现的 HTML 代码。](using-page-inspector-in-visual-studio-2012/_static/image7.png "选择 HTML 选项卡以显示在 Page Inspector 浏览器中呈现的 HTML 代码。") ) 若要禁用光标。
8. 选择**HTML**选项卡以显示在 Page Inspector 浏览器中呈现的 HTML 代码。
9. 在 HTML 标记中，找到与 Koala 链接的列表项并选择它。

    请注意，当您选择的代码，相应的输出自动突出显示在浏览器中。 此功能可用于查看 HTML 块的页上的呈现方式。

    ![在页中的选择 HTML 元素](using-page-inspector-in-visual-studio-2012/_static/image8.png "页中的选择 HTML 元素")

    *在页面中选择 HTML 元素*
10. 单击**切换检查模式下**按钮以启用*检查模式下*单击导航栏。 右侧的 HTML 代码，在样式窗格中，将看到列表，与应用于所选元素的 CSS 样式。

    > [!NOTE]
    > 因为标头是站点布局的一部分，Page Inspector 将打开\_Layout.cshtml 文件并突出显示的代码段会受到影响。

    ![发现样式](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *发现所选元素的样式和源文件*
11. 启用切换检查指针，移动鼠标指针下面特色的蓝色条形，再单击一半的圆圈。

    ![选择元素](using-page-inspector-in-visual-studio-2012/_static/image10.png "选择元素")

    *选择元素*
12. 在样式窗格中，找到**背景图像**项下 **.main 内容**组。 **取消选中** **背景图像**，看看效果。 您将注意到浏览器将立即反映所做的更改，并且该圆形处于隐藏状态。

    > [!NOTE]
    > 在页面检查器样式的选项卡上应用的更改不会影响原始样式表。 您可以取消选中样式或更改无数次，但它们会在还原刷新页面后，其值。

    ![启用和禁用 CSS 样式](using-page-inspector-in-visual-studio-2012/_static/image11.png "启用和禁用 CSS 样式")

    *启用和禁用 CSS 样式*
13. 现在，单击**徽标此处**上使用检查模式下的标头的文本。
14. 在中**样式**选项卡上，找到**字体大小**CSS 属性下 **.site 标题**组。 双击属性值，然后将 2.3 em 值替换为**3 个 em**，然后按**ENTER**。 请注意，标题看上去更大。

    ![更改在 Page Inspector 中的 CSS 值](using-page-inspector-in-visual-studio-2012/_static/image12.png "在 Page Inspector 中的更改的 CSS 值")

    *更改在 Page Inspector 中的 CSS 值*
15. 单击**跟踪样式**选项卡上，Page Inspector 的右窗格中。 这是一种方法来查看应用于所选内容，按属性名称排序的所有样式。

    ![跟踪的 CSS 样式](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *所选元素的 CSS 样式跟踪*
16. Page Inspector 的另一个功能是布局窗格。 使用检查模式下，选择导航栏，然后单击**布局**右窗格上的选项卡。 你将看到所选元素的确切大小以及其偏移量、 边距、 填充和边框的大小。 请注意，您还可以修改此视图中的值。

    ![在 Page Inspector 中的元素布局](using-page-inspector-in-visual-studio-2012/_static/image14.png "在 Page Inspector 中的元素布局")

    *在 Page Inspector 中的元素布局*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>任务 2-查找并修复在照片库中的样式问题

如何诊断与以前版本的 Visual Studio 的网页问题？ 您将可能非常熟悉 web 调试工具，Visual Studio IDE，如 Internet Explorer 开发人员工具或 Firebug 之外运行。 浏览器只了解 HTML、 脚本和样式，而基础框架生成将呈现的 HTML。 为此，通常需要部署的整个站点，以查看网页的显示效果。

当你想要检测和修复问题，在您的网站时，可能必须遵循以下步骤：

1. 从 Visual Studio 中，运行该解决方案或部署 web 服务器上的页。
2. 在浏览器中打开开发人员工具使用，或只需打开源代码和样式并会尝试匹配该问题。 若要查找所涉及的文件，你会使用&quot;搜索&quot;或&quot;文件中的搜索&quot;样式类同名的功能。
3. 一旦检测到错误，停止 Web 浏览器和服务器。
4. 清除浏览器缓存。
5. 返回到 Visual Studio 来应用修补程序。 重复测试的所有步骤。

因为 ASP.NET MVC 4 中没有任何真正所见即所得，大部分样式问题上检测到后面的阶段，在运行或 web 应用程序部署后。 现在，使用 Page Inspector，可以预览任何页，而无需运行该解决方案。

在此任务中，将使用 Page inspector 和修复照片库应用程序的一些问题。

1. 使用 Page Inspector，找到**注册**并**登录**标头的左侧的链接。

    请注意，链接不会显示在预期位置在右侧，它们会显示类似项目符号列表。 现在将对齐到右侧的链接，并可以相应地重新应用。

    ![在链接中查找的注册和日志](using-page-inspector-in-visual-studio-2012/_static/image15.png "在链接中查找的注册和日志")

    *在链接中查找的注册和日志*
2. 选择切换检查模式下，单击要关闭，但不是在注册链接以打开其代码。

    请注意，链接的源代码位于 **\_LoginPartial.cshtml**文件，不 Index.cshtml 和\_Layout.cshtml，是您可以在第一个位置中查找的位置。 你具有已直接放在正确的源文件。
3. 在中**样式**选项卡上，找到并单击 **\<部分 > #login**项，它是下面的链接的 HTML 容器。

    请注意， **#login**样式会自动位于**Site.css**单击后。 此外，代码现在已突出显示。

    ![选择的 CSS 样式](using-page-inspector-in-visual-studio-2012/_static/image16.png "选择 CSS 样式")

    *选择的 CSS 样式*
4. 取消注释**文本对齐**属性中突出显示的代码通过删除开始和结束字符并保存**Site.css**文件。

    Page Inspector 可以识别的所有不同文件构成当前页上，以及它可以检测这些文件的任何更改时。 它向你发出警报时在浏览器中的当前页面不是与源文件同步。
5. 在 Page Inspector 浏览器中，单击位于地址栏以重新加载页面下方的栏。

    ![重新加载页面](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *重新加载页面*

    链接现在位于右侧，但它们看起来仍像项目符号列表。 现在，将删除的项目符号和水平对齐的链接。

    ![更新后的页面](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *更新后的页面*
6. 使用检查模式下，选择任一 **&lt;li&gt;** 包含的项&quot;注册&quot;并&quot;以用户身份登录&quot;链接。 然后，单击 **&lt;部分&gt;#login**访问项**Styles.css**代码。

    ![查找样式](using-page-inspector-in-visual-studio-2012/_static/image19.png "查找样式")

    *查找样式*
7. 在中**Style.css**，取消注释的代码 **#login li**项。 要添加的样式将隐藏项目符号和水平显示的项目。

    ![重新设计的登录链接](using-page-inspector-in-visual-studio-2012/_static/image20.png "重新设计的登录链接")

    *重新设计的登录链接*
8. 保存**Style.css**文件，并在位于以下地址来重新加载页面的栏上单击一次。 请注意，链接正确显示。

    ![链接到的右侧对齐](using-page-inspector-in-visual-studio-2012/_static/image21.png "链接到的右侧对齐")

    *链接到的右侧对齐*
9. 最后，您将更改标头标题。 使用检查模式下单击**徽标此处**文本并生成它的源代码获取。
10. 现在你已在 **\_Layout.cshtml**，将为**此处显示的徽标**文本**照片库**。 保存并更新 Page Inspector 浏览器。

    ![分配新的标题](using-page-inspector-in-visual-studio-2012/_static/image22.png "分配新的标题")

    *分配新的标题*

    ![照片库页](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *更新照片库页*
11. 最后，选择**PhotoGallery**项目，然后按**F5**运行应用程序。 请查看所做的更改按预期方式工作。

---

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>练习 2：在 WebForms 项目中使用 Page Inspector

在此练习中，您将学习如何预览和调试使用 Page Inspector 的 WebForms 解决方案。 首先会执行简要了解该工具来了解促进 Web 调试进程的 Page Inspector 功能。 然后，您可以在包含样式设置问题的网页中。 您将学习如何使用 Page Inspector 找到生成问题的源代码和修复此错误。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>任务 1-浏览页面检查器

在本任务中，您将学习如何在显示照片库 WebForms 项目的上下文中使用 Page Inspector 功能。

1. 打开**开始**解决方案位于**源/Ex2-WebForms/开始/** 文件夹。

   1. 需要先下载某些缺少的 NuGet 包然后再继续。 若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。
   2. 在中**管理 NuGet 包**对话框中，单击**还原**若要下载缺少的包。
   3. 最后，通过单击生成解决方案**构建** | **生成解决方案**。

      > [!NOTE]
      > 使用 NuGet 的优点之一是，您不必提供在项目中的所有库项目大小减少。 使用 NuGet 增强工具，请通过在 Packages.config 文件中，指定包版本可以下载第一次运行该项目的所有所需的库。 这就是原因需要将从本实验中打开现有解决方案后运行这些步骤。
2. 在解决方案资源管理器，找到**Default.aspx**页上，右键单击它并选择**在 Page Inspector 中的查看**。

    ![使用 Page Inspector 打开 Default.aspx](using-page-inspector-in-visual-studio-2012/_static/image24.png "打开 Default.aspx 使用 Page Inspector")

    *打开 Default.aspx 使用 Page Inspector*
3. Page Inspector 窗口将显示 Default.aspx。

    ![在 Page Inspector 中查看 Default.aspx](using-page-inspector-in-visual-studio-2012/_static/image25.png "在 Page Inspector 中查看 Default.aspx")

    *在 Page Inspector 中查看 Default.aspx*

    Page Inspector 工具已集成在 Visual Studio 环境中。 该检查器包含嵌入式浏览器，以及功能强大的 HTML 探查器将显示所选的代码。 请注意，不需要运行解决方案，以查看您的页面的外观。

    > [!NOTE]
    > 当 Page Inspector 浏览器宽度小于所打开的页的宽度时，您不会正确地看到页面。 如果发生这种情况，调整页面检查器的宽度。
4. 单击**文件**在 Page Inspector 中的选项卡。

    您将看到正在撰写呈现的默认页面的所有源文件。 这是一个有用的功能，尤其在使用用户控件和母版页时标识一眼的所有元素。 请注意，还可以导航到每个文件。

    ![文件选项卡](using-page-inspector-in-visual-studio-2012/_static/image26.png "文件选项卡")

    *文件选项卡*
5. 单击**切换检查模式下**按钮，位于左侧的选项卡。

    此工具会让您选择的页的任何元素，并查看其 HTML 代码和.aspx 源。

    ![切换检查模式按钮](using-page-inspector-in-visual-studio-2012/_static/image27.png "切换检查模式按钮")

    *切换检查模式按钮*
6. 在 Page Inspector 浏览器中，将鼠标移动页面元素。 将鼠标指针移过所呈现页中的任何部分，而显示的元素类型和 Visual Studio 编辑器中突出显示相应的源标记或代码。

    ![在操作中的检查模式下](using-page-inspector-in-visual-studio-2012/_static/image28.png "操作在检查模式")

    *在操作中检查模式*

    > [!NOTE]
    > 不最大化页面检查器窗口或你将无法再看到显示的源代码的预览选项卡。 如果它最大化时单击 Page Inspector 中的元素，将显示所选内容的源代码，但它会隐藏页面检查器窗口。

    如果您注意到**Default.aspx**文件中，你会注意到，突出显示的源代码的生成所选的元素的部分。 此功能便于长的源文件，提供指导支持和快速的方法来访问代码的版本。

    ![检查元素](using-page-inspector-in-visual-studio-2012/_static/image29.png "检查元素")

    *检查元素*
7. 单击**切换检查模式下**按钮 (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser。](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser。") ) 中的 Page Inspector 选项卡，若要禁用光标。
8. 选择**HTML**选项卡以显示在 Page Inspector 浏览器中呈现的 HTML 代码。
9. 在 HTML 代码中，找到与 Koala 链接的列表项并选择它。

    请注意，当您选择的代码，相应的输出是自动突出显示浏览器。 此功能可用于查看 HTML 块的页上的呈现方式。

    ![在页中选择 HTML 元素](using-page-inspector-in-visual-studio-2012/_static/image31.png "的页中选择 HTML 元素")

    *在页中选择 HTML 元素*
10. 单击**切换检查模式下**按钮以启用*检查模式下*单击导航栏。 右侧的 HTML 代码，在样式窗格中，将看到列表，与应用于所选元素的 CSS 样式。

    > [!NOTE]
    > 因为标头是站点布局的一部分，Page Inspector 还将打开 Site.Master 文件并突出显示的受影响的代码段。

    ![发现样式 WebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "发现样式和所选元素的源文件")

    *发现所选元素的样式和源文件*
11. 启用切换检查指针，将鼠标指针菜单栏下方移动，再单击空白一半的圆圈。

    ![选择元素](using-page-inspector-in-visual-studio-2012/_static/image33.png "选择元素")

    *选择元素*
12. 在样式窗格中，找到**背景图像**项下 **.main 内容**组。 **取消选中** **背景图像**，看看效果。 您将注意到浏览器将立即反映所做的更改，并且该圆形处于隐藏状态。

    > [!NOTE]
    > 在页面检查器样式的选项卡上应用的更改不会影响原始样式表。 您可以取消选中样式或更改无数次，但它们会在还原刷新页面后，其值。

    ![启用和禁用 CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "启用和禁用 CSS 样式")

    *启用和禁用 CSS 样式*
13. 现在，单击**你** **此处显示的徽标**上使用检查模式下的标头的文本。
14. 在中**样式**选项卡上，找到**字体大小**CSS 属性下 **.site 标题**组。 双击该属性一次以编辑其值。 值替换为 2.3em **3em**，然后按 ENTER。 请注意，标题看上去更大。

    ![更改页面 Inspector2 中的 CSS 值](using-page-inspector-in-visual-studio-2012/_static/image35.png "在 Page Inspector 中的更改的 CSS 值")

    *更改在 Page Inspector 中的 CSS 值*
15. 单击**跟踪样式**选项卡上，Page Inspector 的右窗格中。 这是一种方法来查看应用于所选内容，按属性名称排序的所有样式。

    ![所选元素的 CSS 样式跟踪](using-page-inspector-in-visual-studio-2012/_static/image36.png "所选元素的 CSS 样式跟踪")

    *所选元素的 CSS 样式跟踪*
16. Page Inspector 的另一个功能是布局窗格。 使用检查模式下，选择导航栏，然后单击**布局**右窗格上的选项卡。 你将看到所选元素的确切大小以及其偏移量、 边距、 填充和边框的大小。 请注意，您还可以修改此视图中的值。

    ![在 Page Inspector 中的元素布局](using-page-inspector-in-visual-studio-2012/_static/image37.png "在 Page Inspector 中的元素布局")

    *在 Page Inspector 中的元素布局*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>任务 2-查找并修复在照片库中的样式问题

如何诊断与以前版本的 Visual Studio 的网页问题？ 您将可能非常熟悉 web 调试工具，Visual Studio IDE，如 Internet Explorer 开发人员工具或 Firebug 之外运行。 浏览器只了解 HTML、 脚本和样式，而基础框架生成将呈现的 HTML。 为此，通常需要部署的整个站点，以查看网页的显示效果。

当你想要检测和修复问题，在您的网站时，可能必须遵循以下步骤：

1. 从 Visual Studio 中，运行该解决方案或部署 web 服务器上的页。
2. 在浏览器中打开开发人员工具使用，或只需打开源代码和样式并会尝试匹配该问题。 若要查找所涉及的文件，你会使用&quot;搜索&quot;或&quot;文件中的搜索&quot;样式类同名的功能。
3. 一旦检测到错误，停止 Web 浏览器和服务器。
4. 清除浏览器缓存。
5. 返回到 Visual Studio 来应用修补程序。 重复测试的所有步骤。

因为没有不真正所见即所得 ASP.NET WebForms 中，某些样式问题上检测到后面的阶段，在运行或部署后。 现在，使用 Page Inspector，可以预览任何页，而无需运行该解决方案。

在此任务中，将使用 Page inspector，修复照片库应用程序的一些问题。 在以下步骤中，将检测并快速修复标头中的一些简单样式问题。

1. 使用页检查，找到**注册**并**Log In**处的标头的左侧链接。

    请注意，该链接不显示在右侧的预期位置。 现在将对齐到右侧的链接，并可以相应地重新应用。

    ![登录链接位于左侧](using-page-inspector-in-visual-studio-2012/_static/image38.png "定位在左侧的链接以用户身份登录")

    *在左侧定位的登录链接*
2. 使用所选的切换检查模式下，选择日志中的链接以打开其代码。

    请注意，链接源代码位于**Site.Master**文件中，未在 Default.aspx 页，这是一个您可能会看到在第一个位置; 你具有已直接放在正确的源文件。
3. 在中**样式**选项卡上，找到并单击 **&lt;部分&gt;#login**项，它是下面的链接的 HTML 容器。

    请注意， **#login**样式会自动位于**Site.css**单击后。 此外，代码现在已突出显示。

    ![选择的 CSS 样式](using-page-inspector-in-visual-studio-2012/_static/image39.png "选择 CSS 样式")

    *选择的 CSS 样式*
4. 取消注释**文本对齐**属性中突出显示的代码通过删除开始和结束字符并保存**Site.css**文件。

    Page Inspector 可以识别的所有不同文件构成当前页上，以及它可以检测这些文件的任何更改时。 它向你发出警报时在浏览器中的当前页面不是与源文件同步。
5. 在 Page Inspector 浏览器中，单击位于地址栏以保存所做的更改并重新加载页面下方的栏。

    ![重新加载页面](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *重新加载页面*

    链接现在位于右侧，但它们看起来仍像项目符号列表。 现在，将删除的项目符号和水平对齐的链接。

    ![更新后的页面](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *更新后的页面*
6. 使用检查模式下，选择任一 **&lt;li&gt;** 包含的项&quot;注册&quot;并&quot;以用户身份登录&quot;链接。 然后，单击 **&lt;部分&gt;#login**访问项**Styles.css**代码。

    ![查找样式](using-page-inspector-in-visual-studio-2012/_static/image42.png "查找样式")

    *查找样式*
7. 在中**Style.css**，取消注释的代码 **#login li**项。 要添加的样式将隐藏项目符号和水平显示的项目。

    ![重新设计的登录链接](using-page-inspector-in-visual-studio-2012/_static/image43.png "重新设计的登录链接")

    *重新设计的登录链接*
8. 保存**Style.css**文件，并在位于以下地址来重新加载页面的栏上单击一次。 请注意，链接正确显示。

    ![链接到的右侧对齐](using-page-inspector-in-visual-studio-2012/_static/image44.png "链接到的右侧对齐")

    *链接到的右侧对齐*
9. 最后，您将更改标头标题。 而不是搜索**此处显示的徽标**文本中所有文件，使用检查模式下单击文本并生成它的源代码。

    ![查找站点标题](using-page-inspector-in-visual-studio-2012/_static/image45.png "查找网站标题")

    *查找网站标题*
10. 现在你已在**Site.Master**，将为**此处显示的徽标**文本**照片库**。 保存并更新 Page Inspector 浏览器。

    ![照片库页更新](using-page-inspector-in-visual-studio-2012/_static/image46.png "更新照片库页")

    *更新照片库页*
11. 最后，按**F5**运行应用的签出所有所做的更改按预期方式工作。

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>总结

通过完成本动手实验，你已了解到如何使用 Page Inspector 无需重新生成并运行网站的浏览器中预览 Web 应用程序。 此外，你已了解到如何快速查找并修复 bug，通过直接从呈现的输出访问源代码。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附录 a:安装 Visual Studio Express 2012 for Web

你可以安装**Microsoft Visual Studio Express 2012 for Web**或另一个&quot;Express&quot;使用版本 **[Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)** . 以下说明将指导您完成安装所需的步骤*Visual studio Express 2012 for Web*使用*Microsoft Web 平台安装程序*。

1. 转到[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。 或者，如果你已安装 Web 平台安装程序，则可以打开它，并搜索产品&quot; <em>Visual Studio Express 2012 for Web 与 Windows Azure SDK</em>&quot;。
2. 单击**立即安装**。 如果还没有**Web 平台安装程序**将重定向以下载并安装。
3. 一次**Web 平台安装程序**处于打开状态，单击**安装**以启动安装程序。

    ![安装 Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "安装 Visual Studio Express")

    *安装 Visual Studio Express*
4. 阅读所有产品的许可证和条款，然后单击**我接受**以继续。

    ![接受许可条款](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *接受许可条款*
5. 等待，直到下载和安装过程完成。

    ![安装进度](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *安装进度*
6. 安装完成后，单击**完成**。

    ![安装已完成](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *安装已完成*
7. 单击**退出**以关闭 Web 平台安装程序。
8. 若要打开 Visual Studio Express for Web，请转到**启动**屏幕上即可开始编写&quot; **VS Express**&quot;，然后单击**VS Express for Web**磁贴。

    ![VS Express for Web 磁贴](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *VS Express for Web 磁贴*
