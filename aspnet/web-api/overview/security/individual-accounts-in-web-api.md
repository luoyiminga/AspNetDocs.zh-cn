---
uid: web-api/overview/security/individual-accounts-in-web-api
title: 保护 Web API 使用单个帐户和 ASP.NET Web API 2.2 中的本地登录名 |Microsoft Docs
author: MikeWasson
description: 本主题演示如何保护 web API 使用 OAuth2 对成员资格数据库进行身份验证。 在教程的 Visual Studio 201 中使用的软件版本...
ms.author: riande
ms.date: 10/15/2014
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7492c4aa4c2a0a8aeed64c3462bda8fc51f35a6b
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2019
ms.locfileid: "65134303"
---
# <a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>保护 Web API 使用单个帐户和 ASP.NET Web API 2.2 中的本地登录名

通过[Mike Wasson](https://github.com/MikeWasson)

[下载示例应用程序](https://github.com/MikeWasson/LocalAccountsApp)

> 本主题演示如何保护 web API 使用 OAuth2 对成员资格数据库进行身份验证。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Web API 2.2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2.1](../../../identity/index.md)

在 Visual Studio 2013 中，Web API 项目模板也提供三个选项用于身份验证：

- **个人帐户。** 该应用使用成员资格数据库。
- **组织帐户。** 用户使用其 Azure Active Directory、 Office 365 或本地 Active Directory 凭据登录。
- **Windows 身份验证。** 此选项适用于 Intranet 应用程序，并且使用 Windows 身份验证 IIS 模块。

有关这些选项的更多详细信息，请参阅[在 Visual Studio 2013 中创建 ASP.NET Web 项目](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth)。

个人帐户提供的用户记录中的两种方法：

- **本地登录名**。 用户注册的站点上，输入用户名和密码。 应用程序将密码哈希存储在成员资格数据库。 当用户记录时，ASP.NET 标识系统验证的密码。
- **社交登录**。 用户使用外部服务，如 Facebook、 Microsoft 或 Google 登录。 应用程序仍在成员资格数据库中创建用户的条目，但不存储任何凭据。 用户通过登录到外部服务进行身份验证。

本文探讨的本地登录方案。 为本地和社交登录名，Web API 使用 OAuth2 请求进行身份验证。 但是，凭据流是不同的本地和社交登录名。

在本文中，我将演示一个简单的应用，可让用户登录，将发送到 web API 已经过身份验证的 AJAX 调用。 您可以下载示例代码[此处](https://github.com/MikeWasson/LocalAccountsApp)。 自述文件介绍了如何在 Visual Studio 中从头开始创建示例。

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

示例应用使用 Knockout.js 进行数据绑定和 jQuery 用于发送 AJAX 请求。 我将重点放在 AJAX 调用，因此无需知道 Knockout.js 的这篇文章。

此过程中，我将介绍：

- 在客户端上，应用程序执行哪些操作。
- 在服务器上发生了什么。
- 在中间 HTTP 流量。

首先，我们需要定义一些 OAuth2 术语。

- *资源*。 一些可以受保护的数据。
- *资源服务器*。 承载资源的服务器。
- *资源所有者*。 可以授予权限来访问资源的实体。 （通常为用户。）
- *客户端*:想要访问该资源应用。 在本文中，客户端是 web 浏览器。
- *访问令牌*。 授予对资源的访问令牌。
- *持有者令牌*。 特定类型的访问令牌，与任何人都可以使用该令牌的属性。 换而言之，客户端不需要加密密钥或其他机密使用持有者令牌。 为此，持有者令牌应仅用于通过 HTTPS，因此应具有相对较短的到期时间。
- *授权服务器*。 给出的访问令牌的服务器。

应用程序可以充当授权服务器和资源服务器。 Web API 项目模板遵循此模式。

## <a name="local-login-credential-flow"></a>本地登录凭据流

对于从本地登录名使用 Web API[资源所有者密码流](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html)OAuth2 中定义。

1. 用户输入客户端的名称和密码。
2. 客户端将这些凭据发送到授权服务器。
3. 授权服务器进行身份验证凭据，并返回访问令牌。
4. 若要访问受保护的资源，客户端在 HTTP 请求的 Authorization 标头中包括的访问令牌。

![](individual-accounts-in-web-api/_static/image3.png)

当选择**单个帐户**在 Web API 项目模板中，该项目包括验证用户凭据和颁发令牌的授权服务器。 下图显示了在 Web API 组件方面的相同凭据流。

![](individual-accounts-in-web-api/_static/image4.png)

在此方案中，Web API 控制器充当资源服务器。 身份验证筛选器验证访问令牌，并 **[Authorize]** 属性用来保护资源。 当控制器或操作具有 **[Authorize]** 属性，该控制器的所有请求或操作必须进行身份验证。 否则为授权被拒绝，并且 Web API 将返回 401 （未经授权） 错误。

授权服务器和身份验证筛选器都将调用到[OWIN 中间件](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)组件，它处理 OAuth2 的详细信息。 在本教程后面，我将介绍更多详细信息中的设计。

## <a name="sending-an-unauthorized-request"></a>发送未经授权的请求

若要开始，运行应用，并单击**调用 API**按钮。 请求完成后，你应看到中的错误消息**结果**框。 这是因为请求不包含访问令牌，因此该请求是未经授权。

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

**调用 API**按钮时发送 AJAX 请求到 ~/api/值，调用 Web API 控制器操作。 下面是发送 AJAX 请求的 JavaScript 代码的一部分。 在示例应用中，所有 JavaScript 应用程序代码位于 Scripts\app.js 文件中。

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

在用户登录，直到没有任何持有者令牌，并因此没有请求授权标头。 这将导致请求，以便返回 401 错误。

下面是 HTTP 请求。 (我使用了[Fiddler](http://www.telerik.com/fiddler)捕获 HTTP 流量。)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

HTTP 响应：

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

请注意，响应包括 Www-authenticate 标头与设置为持有者质询。 指示服务器所需的持有者令牌。

## <a name="register-a-user"></a>注册用户

在中**注册**部分中的应用程序中，输入一个电子邮件和密码，然后单击**注册**按钮。

不需要有效的电子邮件地址用于此示例中，但真正的应用程序会确认地址。 (请参阅[安全的 ASP.NET MVC 5 web 应用程序创建具有登录、 电子邮件确认及密码重置](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)。)输入密码，使用"Password1 ！"，类似与大写字母、 小写字母、 数字和非字母数字字符。 若要简化应用程序，我省略了客户端验证，所以如果没有密码格式问题，您会收到 400 （错误请求） 错误。

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

**注册**按钮将 POST 请求发送到 ~/api/Account/Register /。 请求正文是一个 JSON 对象，包含名称和密码。 下面是将请求发送的 JavaScript 代码：

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

HTTP 请求：

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

HTTP 响应：

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

此请求由`AccountController`类。 在内部，`AccountController`使用 ASP.NET 标识来管理成员资格数据库。

如果从 Visual Studio 本地运行应用，用户帐户在 LocalDB，存储在 AspNetUsers 表中。 若要在 Visual Studio 中查看的表，请单击**视图**菜单中，选择**服务器资源管理器**，然后展开**数据连接**。

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>获取访问令牌

到目前为止我们所不做任何 OAuth，但现在我们将看到 OAuth 授权服务器，在操作，当我们请求访问令牌。 在中**日志中**区域中的示例应用中，输入电子邮件和密码，然后单击**Log In**。

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

**日志中**按钮向令牌终结点发送请求。 请求的正文包含以下窗体 url 编码数据：

- 授予\_类型:"密码"
- 用户名：&lt;用户的电子邮件&gt;
- 密码：&lt;密码&gt;

下面是发送 AJAX 请求的 JavaScript 代码：

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

如果请求成功，授权服务器响应正文中返回访问令牌。 请注意，我们将该令牌存储在会话存储，以供以后使用时将请求发送到 API。 与某些形式的身份验证 （例如基于 cookie 的身份验证），不同浏览器将不会自动包括访问令牌在后续请求中。 应用程序必须执行显式删除。 这是一件好事，因为它限制[CSRF 漏洞](preventing-cross-site-request-forgery-csrf-attacks.md)。

HTTP 请求：

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

您可以看到该请求包含用户的凭据。 您*必须*使用 HTTPS 来提供传输层安全性。

HTTP 响应：

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

为了提高可读性，我缩进 JSON 和截断的访问令牌，这是一个非常长。

`access_token`， `token_type`，和`expires_in`由 OAuth2 规范定义属性。其他属性 (`userName`， `.issued`，和`.expires`) 是仅供您参考。 您可以找到添加在这些附加属性的代码`TokenEndpoint`/Providers/ApplicationOAuthProvider.cs 文件中的方法。

## <a name="send-an-authenticated-request"></a>发送经过身份验证的请求

现在，我们拥有的持有者令牌，我们可以对 API 进行身份验证的请求。 这可通过在请求中设置授权标头。 单击**调用 API**按钮将再次看到此。

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

HTTP 请求：

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

HTTP 响应：

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>注销

因为浏览器不会缓存凭据或访问令牌，注销是只需"忘记"令牌，通过从会话存储将其删除：

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>了解个人帐户项目模板

当选择**单个帐户**在 ASP.NET Web 应用程序项目模板中，该项目包括：

- OAuth2 授权服务器。
- 用于管理用户帐户的 Web API 终结点
- 用于存储用户帐户的 EF 模型。

下面是实现这些功能的主应用程序类：

- `AccountController`。 提供用于管理用户帐户的 Web API 终结点。 `Register`操作是我们在本教程中使用只有一个。 在类上的其他方法还支持密码重置、 社交登录名和其他功能。
- `ApplicationUser`/Models/IdentityModels.cs 中定义。 此类是 EF 模型的成员资格数据库中的用户帐户。
- `ApplicationUserManager`定义在 /App\_Start/IdentityConfig.cs 派生出此类[UserManager](https://msdn.microsoft.com/library/dn613290.aspx)和用户帐户，如创建新用户，验证密码，等等，对执行操作和自动保存对数据库进行更改。
- `ApplicationOAuthProvider`。 此对象插入到 OWIN 中间件，并处理由中间件引发的事件。 它派生[OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx)。

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>配置授权服务器

在 StartupAuth.cs，以下代码配置 OAuth2 授权服务器。

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

`TokenEndpointPath`属性是向授权服务器终结点的 URL 路径。 是一个 URL，该应用使用来获取持有者令牌。

`Provider`属性指定的提供程序插入到 OWIN 中间件，并处理由中间件引发的事件。

当应用想要获取的令牌时，下面是基本流程：

1. 若要获取访问令牌，应用将请求发送到 ~ / 令牌。
2. OAuth 中间件调用`GrantResourceOwnerCredentials`的提供程序。
3. 在提供程序调用`ApplicationUserManager`验证凭据并创建声明标识。
4. 如果成功，则提供程序创建一个身份验证票证，用于生成令牌。

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

OAuth 中间件完全不了解的用户帐户。 提供程序之间的中间件和 ASP.NET 标识进行通信。 有关实现授权服务器的详细信息，请参阅[OWIN OAuth 2.0 授权服务器](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md)。

### <a name="configuring-web-api-to-use-bearer-tokens"></a>配置要使用持有者令牌的 Web API

在`WebApiConfig.Register`方法中，以下代码中设置了 Web API 管道的身份验证：

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

**HostAuthenticationFilter**类，使用持有者令牌进行身份验证。

**SuppressDefaultHostAuthentication**方法告知 Web API 忽略在请求到达 Web API 管道，IIS 或 OWIN 中间件之前发生的任何身份验证。 这样一来，我们可以限制只使用持有者令牌进行身份验证的 Web API。

> [!NOTE]
> 具体而言，您的应用程序的 MVC 部分可能会使用窗体身份验证，将凭据存储在 cookie 中。 基于 cookie 的身份验证要求使用防伪令牌，以防止 CSRF 攻击。 这是因为没有 web API 的便捷方法以将防伪令牌发送到客户端 web Api 的问题。 (有关此问题的详细背景，请参阅[防止 CSRF 攻击 Web API 中](preventing-cross-site-request-forgery-csrf-attacks.md)。)调用**SuppressDefaultHostAuthentication**可确保 Web API 不会从凭据存储在 cookie 中容易受到 CSRF 攻击。

当客户端请求的受保护的资源时，下面是 Web API 管道中会发生什么情况：

1. **HostAuthentication**筛选器会调用要验证的令牌的 OAuth 中间件。
2. 中间件将声明标识转换为令牌。
3. 此时，该请求是否*进行身份验证*但不是*授权*。
4. 授权筛选器将检查声明标识。 如果声明授权用户对该资源，请对请求进行授权。 默认情况下 **[Authorize]** 属性将授权进行身份验证的任何请求。 但是，可以按角色或其他声明进行授权。 有关详细信息，请参阅[身份验证和 Web API 中的授权](authentication-and-authorization-in-aspnet-web-api.md)。
5. 如果前面的步骤都成功，将控制器返回受保护的资源。 否则，客户端收到 401 （未经授权） 错误。

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>其他资源

- [ASP.NET Identity](../../../identity/index.md)
- [了解 SPA 模板中的安全功能的 VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)。 MSDN 博客文章 Hongye Sun。
- [仔细分析 Web API 个人帐户模板 – 第 2 部分：本地帐户](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/)。 Dominick Baier 的博客文章。
- [托管身份验证和 Web API 与 OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/)。 将详细介绍`SuppressDefaultHostAuthentication`和`HostAuthenticationFilter`由 Brock Allen。
- [自定义 ASP.NET 标识中 VS 2013 模板中的配置文件信息](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)。 Pranav rastogi 撰写的 MSDN 博客文章。
- [每个请求生存期管理 UserManager 类在 ASP.NET 标识中](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx)。 将详细介绍与 MSDN 博客文章由 Suhas Joshi`UserManager`类。
