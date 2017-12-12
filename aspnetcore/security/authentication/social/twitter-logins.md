---
title: "Twitter 外部登录设置"
author: rick-anderson
description: "本教程演示的集成到现有的 ASP.NET Core 应用的 Twitter 帐户用户身份验证。"
keywords: "ASP.NET 核心，Twitter 登录名，身份验证"
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: E5931607-31C0-4B20-B416-85E3550F5EA8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/twitter-logins
ms.openlocfilehash: 6751b34b42007cffa9ee92ee49170564b8eac997
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
# <a name="configuring-twitter-authentication"></a><span data-ttu-id="7ad29-104">配置 Twitter 身份验证</span><span class="sxs-lookup"><span data-stu-id="7ad29-104">Configuring Twitter authentication</span></span>

<span data-ttu-id="7ad29-105">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7ad29-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7ad29-106">本教程演示如何启用到你的用户[使用 Twitter 帐户登录](https://dev.twitter.com/web/sign-in/desktop-browser)上使用示例 ASP.NET 核心 2.0 项目创建[上一页](index.md)。</span><span class="sxs-lookup"><span data-stu-id="7ad29-106">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="7ad29-107">在 Twitter 中创建应用程序</span><span class="sxs-lookup"><span data-stu-id="7ad29-107">Create the app in Twitter</span></span>

* <span data-ttu-id="7ad29-108">导航到[https://apps.twitter.com/](https://apps.twitter.com/)并登录。</span><span class="sxs-lookup"><span data-stu-id="7ad29-108">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="7ad29-109">如果你没有 Twitter 帐户，使用**[立即注册](https://twitter.com/signup)**链接创建一个。</span><span class="sxs-lookup"><span data-stu-id="7ad29-109">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="7ad29-110">登录后，**应用程序管理**页所示：</span><span class="sxs-lookup"><span data-stu-id="7ad29-110">After signing in, the **Application Management** page is shown:</span></span>

![在 Microsoft Edge 中打开的 twitter 应用程序管理](index/_static/TwitterAppManage.png)

* <span data-ttu-id="7ad29-112">点击**创建新的应用程序**并填写应用程序**名称**，**说明**和公共**网站**（可以是临时直到你的 URI注册的域名）：</span><span class="sxs-lookup"><span data-stu-id="7ad29-112">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

![创建的应用程序页](index/_static/TwitterCreate.png)

* <span data-ttu-id="7ad29-114">输入你的开发 URI 与*/signin-twitter*追加到**有效的 OAuth 重定向 Uri**字段 (例如： `https://localhost:44320/signin-twitter`)。</span><span class="sxs-lookup"><span data-stu-id="7ad29-114">Enter your development URI with */signin-twitter* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="7ad29-115">本教程中稍后配置的 Twitter 身份验证方案将自动处理请求在*/signin-twitter*要实现的 OAuth 流路由。</span><span class="sxs-lookup"><span data-stu-id="7ad29-115">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at */signin-twitter* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="7ad29-116">填写表单的其余部分并点击**创建 Twitter 应用程序**。</span><span class="sxs-lookup"><span data-stu-id="7ad29-116">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="7ad29-117">显示新的应用程序详细信息：</span><span class="sxs-lookup"><span data-stu-id="7ad29-117">New application details are displayed:</span></span>

![应用程序页上的详细信息选项卡](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="7ad29-119">部署站点时你将需要重新访问**应用程序管理**页上，并注册一个新的公共 URI。</span><span class="sxs-lookup"><span data-stu-id="7ad29-119">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="7ad29-120">存储 Twitter ConsumerKey 和 ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="7ad29-120">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="7ad29-121">链接敏感设置，例如 Twitter`Consumer Key`和`Consumer Secret`到你应用程序配置中使用[机密 Manager](../../app-secrets.md)。</span><span class="sxs-lookup"><span data-stu-id="7ad29-121">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="7ad29-122">对于此教程的目的，命名为令牌`Authentication:Twitter:ConsumerKey`和`Authentication:Twitter:ConsumerSecret`。</span><span class="sxs-lookup"><span data-stu-id="7ad29-122">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="7ad29-123">可以在上找到这些标记**密钥和访问令牌**选项卡后创建新的 Twitter 应用程序：</span><span class="sxs-lookup"><span data-stu-id="7ad29-123">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![密钥和访问令牌的选项卡](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="7ad29-125">配置 Twitter 身份验证</span><span class="sxs-lookup"><span data-stu-id="7ad29-125">Configure Twitter Authentication</span></span>

<span data-ttu-id="7ad29-126">在本教程使用的项目模板可确保[Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter)已安装包。</span><span class="sxs-lookup"><span data-stu-id="7ad29-126">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="7ad29-127">若要使用 Visual Studio 2017 安装此包，请右键单击项目并选择**管理 NuGet 包**。</span><span class="sxs-lookup"><span data-stu-id="7ad29-127">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="7ad29-128">若要使用.NET 核心 CLI 安装，请在项目目录中执行以下命令：</span><span class="sxs-lookup"><span data-stu-id="7ad29-128">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7ad29-129">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7ad29-129">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7ad29-130">添加中的 Twitter 服务`ConfigureServices`中的方法*Startup.cs*文件：</span><span class="sxs-lookup"><span data-stu-id="7ad29-130">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7ad29-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7ad29-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7ad29-132">添加在 Twitter 中间件`Configure`中的方法*Startup.cs*文件：</span><span class="sxs-lookup"><span data-stu-id="7ad29-132">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

<span data-ttu-id="7ad29-133">请参阅[TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) Twitter 身份验证支持的配置选项的详细信息的 API 参考。</span><span class="sxs-lookup"><span data-stu-id="7ad29-133">See the [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="7ad29-134">这可以用于请求有关用户的不同信息。</span><span class="sxs-lookup"><span data-stu-id="7ad29-134">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="7ad29-135">使用 Twitter 登录</span><span class="sxs-lookup"><span data-stu-id="7ad29-135">Sign in with Twitter</span></span>

<span data-ttu-id="7ad29-136">运行你的应用程序，然后单击**登录**。</span><span class="sxs-lookup"><span data-stu-id="7ad29-136">Run your application and click **Log in**.</span></span> <span data-ttu-id="7ad29-137">将显示一个选项以使用 Twitter 登录：</span><span class="sxs-lookup"><span data-stu-id="7ad29-137">An option to sign in with Twitter appears:</span></span>

![Web 应用程序： 用户未经过身份验证](index/_static/DoneTwitter.png)

<span data-ttu-id="7ad29-139">单击**Twitter**将重定向到 Twitter 进行身份验证：</span><span class="sxs-lookup"><span data-stu-id="7ad29-139">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Twitter 身份验证页](index/_static/TwitterLogin.png)

<span data-ttu-id="7ad29-141">在输入你的 Twitter 凭据后，你将重定向回 web 站点，你可以设置你的电子邮件。</span><span class="sxs-lookup"><span data-stu-id="7ad29-141">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="7ad29-142">现在已在使用你的 Twitter 凭据进行登录：</span><span class="sxs-lookup"><span data-stu-id="7ad29-142">You are now logged in using your Twitter credentials:</span></span>

![Web 应用程序： 身份验证的用户](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="7ad29-144">疑难解答</span><span class="sxs-lookup"><span data-stu-id="7ad29-144">Troubleshooting</span></span>

* <span data-ttu-id="7ad29-145">**ASP.NET 核心 2.x 仅：**如果标识未通过调用配置`services.AddIdentity`中`ConfigureServices`，尝试进行身份验证将导致*ArgumentException： 必须提供 SignInScheme 选项*。</span><span class="sxs-lookup"><span data-stu-id="7ad29-145">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="7ad29-146">在本教程使用的项目模板可确保，这完成的。</span><span class="sxs-lookup"><span data-stu-id="7ad29-146">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="7ad29-147">如果尚未通过应用初始迁移创建站点数据库，则会出现*处理请求时，数据库操作失败*错误。</span><span class="sxs-lookup"><span data-stu-id="7ad29-147">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="7ad29-148">点击**应用迁移**创建数据库和刷新可跳过错误。</span><span class="sxs-lookup"><span data-stu-id="7ad29-148">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7ad29-149">后续步骤</span><span class="sxs-lookup"><span data-stu-id="7ad29-149">Next steps</span></span>

* <span data-ttu-id="7ad29-150">本文介绍了你使用 Twitter 可以进行的验证。</span><span class="sxs-lookup"><span data-stu-id="7ad29-150">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="7ad29-151">你可以遵循类似的方法进行身份验证使用其他提供商上列出[上一页](index.md)。</span><span class="sxs-lookup"><span data-stu-id="7ad29-151">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="7ad29-152">一旦您的网站发布到 Azure web 应用时，您应重置`ConsumerSecret`Twitter 开发人员门户中。</span><span class="sxs-lookup"><span data-stu-id="7ad29-152">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="7ad29-153">设置`Authentication:Twitter:ConsumerKey`和`Authentication:Twitter:ConsumerSecret`作为在 Azure 门户中的应用程序设置。</span><span class="sxs-lookup"><span data-stu-id="7ad29-153">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="7ad29-154">配置系统设置以从环境变量中读取项。</span><span class="sxs-lookup"><span data-stu-id="7ad29-154">The configuration system is set up to read keys from environment variables.</span></span>
