---
title: "启用使用 Facebook、Google 和其他外部提供程序的身份验证"
author: rick-anderson
description: "本教程演示如何使用外部身份验证提供程序通过 OAuth 2.0 生成 ASP.NET Core 2.x 应用。"
keywords: "ASP.NET Core, 身份验证, 社交, 身份验证提供程序, google, facebook, twitter, microsoft 帐户"
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: eda7ee17-f38c-462e-8d1d-63f459901cf3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/social/index
ms.openlocfilehash: 9cc637f469dcb7097ee1b3996fde8a4ebac8d7ff
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2017
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a><span data-ttu-id="8dc42-104">启用使用 Facebook、Google 和其他外部提供程序的身份验证</span><span class="sxs-lookup"><span data-stu-id="8dc42-104">Enabling authentication using Facebook, Google, and other external providers</span></span>

<a name="security-authentication-social-logins"></a>

<span data-ttu-id="8dc42-105">作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8dc42-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8dc42-106">本教程演示如何生成 ASP.NET Core 2.x 应用，该应用可让用户使用外部身份验证提供程序提供的凭据通过 OAuth 2.0 登录。</span><span class="sxs-lookup"><span data-stu-id="8dc42-106">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="8dc42-107">以下几节中介绍了 [Facebook](facebook-logins.md)、[Twitter](twitter-logins.md)、[Google](google-logins.md) 和 [Microsoft](microsoft-logins.md) 提供程序。</span><span class="sxs-lookup"><span data-stu-id="8dc42-107">[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), and [Microsoft](microsoft-logins.md) providers are covered in the following sections.</span></span> <span data-ttu-id="8dc42-108">第三方程序包中提供了其他提供程序，例如 [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) 和 [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers)。</span><span class="sxs-lookup"><span data-stu-id="8dc42-108">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Facebook、Twitter、Google plus 和 Windows 的社交媒体图标](index/_static/social.png)

<span data-ttu-id="8dc42-110">使用户能够使用其当前凭据登录对用户来说十分便利，并且这样做可以将管理登录进程许多复杂操作转移给第三方。</span><span class="sxs-lookup"><span data-stu-id="8dc42-110">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="8dc42-111">有关社交登录如何驱动流量和客户转换的示例，请参阅 [Facebook](https://www.facebook.com/unsupportedbrowser) 和 [Twitter](https://dev.twitter.com/resources/case-studies) 的案例分析。</span><span class="sxs-lookup"><span data-stu-id="8dc42-111">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="8dc42-112">请注意：此处展示的程序包提取了 OAuth 身份验证流的大量复杂操作，但进行故障排除时，可能需要了解相关详细信息。</span><span class="sxs-lookup"><span data-stu-id="8dc42-112">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="8dc42-113">有许多资源可用，例如，请参阅 [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2)（OAuth 2 简介）或 [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/)（了解 OAuth 2）。</span><span class="sxs-lookup"><span data-stu-id="8dc42-113">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="8dc42-114">某些问题可通过查看 [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src)（提供程序包的 ASP.NET Core 源代码）解决。</span><span class="sxs-lookup"><span data-stu-id="8dc42-114">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="8dc42-115">创建新的 ASP.NET Core 项目</span><span class="sxs-lookup"><span data-stu-id="8dc42-115">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="8dc42-116">在 Visual Studio 2017 中，从“开始”页创建新项目，或通过“文件”>“新建”>“项目”进行创建。</span><span class="sxs-lookup"><span data-stu-id="8dc42-116">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="8dc42-117">选择“Visual C#”>“.NET Core”分类中提供的“ASP.NET Core Web 应用程序”模板：</span><span class="sxs-lookup"><span data-stu-id="8dc42-117">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![“新建项目”对话框](index/_static/new-project.png)

* <span data-ttu-id="8dc42-119">点击“Web 应用程序”，验证“身份验证”是否设置为“单个用户帐户”：</span><span class="sxs-lookup"><span data-stu-id="8dc42-119">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![“新建 Web 应用程序”对话框](index/_static/select-project.png)

<span data-ttu-id="8dc42-121">请注意：本教程适用于可从向导顶部选择的 ASP.NET Core 2.0 SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="8dc42-121">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="8dc42-122">应用迁移</span><span class="sxs-lookup"><span data-stu-id="8dc42-122">Apply migrations</span></span>

* <span data-ttu-id="8dc42-123">运行应用并选择“登录”链接。</span><span class="sxs-lookup"><span data-stu-id="8dc42-123">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="8dc42-124">选择“以新用户身份注册”链接。</span><span class="sxs-lookup"><span data-stu-id="8dc42-124">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="8dc42-125">输入新帐户的电子邮件地址和密码，再选择“注册”。</span><span class="sxs-lookup"><span data-stu-id="8dc42-125">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="8dc42-126">按照说明操作来应用迁移。</span><span class="sxs-lookup"><span data-stu-id="8dc42-126">Follow the instructions to apply migrations.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="8dc42-127">要求 SSL</span><span class="sxs-lookup"><span data-stu-id="8dc42-127">Require SSL</span></span>

<span data-ttu-id="8dc42-128">OAuth 2.0 需要使用 SSL 通过 HTTPS 协议进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="8dc42-128">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="8dc42-129">请注意：如果如上图所示，在项目向导的“更改身份验证”对话框上选择“单个用户帐户”选项，使用 ASP.NET Core 2.x 的“Web 应用程序”或“Web API”项目模板创建的项目自动配置为启用 SSL 并通过 http URL 启动。</span><span class="sxs-lookup"><span data-stu-id="8dc42-129">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="8dc42-130">按照 [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl)（在 ASP.NET Core 应用中强制实施 SSL）主题中的步骤进行操作，要求在站点上使用 SSL。</span><span class="sxs-lookup"><span data-stu-id="8dc42-130">Require SSL on your site by following the steps in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="8dc42-131">使用 SecretManager 存储登录提供程序分配的令牌</span><span class="sxs-lookup"><span data-stu-id="8dc42-131">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="8dc42-132">社交登录提供程序在注册过程中分配“应用程序 ID”和“应用程序机密”（确切命名因提供程序而异）。</span><span class="sxs-lookup"><span data-stu-id="8dc42-132">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process (exact naming varies by provider).</span></span>

<span data-ttu-id="8dc42-133">这些值实际上是应用程序用于访问其 API 的“用户名”和“密码”，它们组成的“机密”可在“机密管理器”的帮助下链接到应用程序配置，而不是直接存储在配置文件中或被硬编码。</span><span class="sxs-lookup"><span data-stu-id="8dc42-133">These values are effectively the *user name* and *password* your application uses to access their API, and constitute the "secrets" that can be linked to your application configuration with the help of **Secret Manager** instead of storing them in configuration files directly or hard-coding them.</span></span>

<span data-ttu-id="8dc42-134">按照 [Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets)（在 ASP.NET Core 中进行开发期间安全存储应用机密）主题中的步骤进行操作，以便可以存储以下每个登录提供程序分配的令牌。</span><span class="sxs-lookup"><span data-stu-id="8dc42-134">Follow the steps in [Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets) topic so that you can store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="8dc42-135">应用程序所需的安装登录提供程序</span><span class="sxs-lookup"><span data-stu-id="8dc42-135">Setup login providers required by your application</span></span>

<span data-ttu-id="8dc42-136">使用以下主题配置应用程序，以使用相应的提供程序：</span><span class="sxs-lookup"><span data-stu-id="8dc42-136">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="8dc42-137">[Facebook](facebook-logins.md) 相关说明</span><span class="sxs-lookup"><span data-stu-id="8dc42-137">[Facebook](facebook-logins.md) instructions</span></span>
* <span data-ttu-id="8dc42-138">[Twitter](twitter-logins.md) 相关说明</span><span class="sxs-lookup"><span data-stu-id="8dc42-138">[Twitter](twitter-logins.md) instructions</span></span>
* <span data-ttu-id="8dc42-139">[Google](google-logins.md) 相关说明</span><span class="sxs-lookup"><span data-stu-id="8dc42-139">[Google](google-logins.md) instructions</span></span>
* <span data-ttu-id="8dc42-140">[Microsoft](microsoft-logins.md) 相关说明</span><span class="sxs-lookup"><span data-stu-id="8dc42-140">[Microsoft](microsoft-logins.md) instructions</span></span>
* <span data-ttu-id="8dc42-141">[其他提供程序](other-logins.md)相关说明</span><span class="sxs-lookup"><span data-stu-id="8dc42-141">[Other provider](other-logins.md) instructions</span></span>

## <a name="optionally-set-password"></a><span data-ttu-id="8dc42-142">选择性地设置密码</span><span class="sxs-lookup"><span data-stu-id="8dc42-142">Optionally set password</span></span>

<span data-ttu-id="8dc42-143">使用外部登录提供程序注册即表明还没有向应用注册密码。</span><span class="sxs-lookup"><span data-stu-id="8dc42-143">When you register with an external login provider, you do not have a password registered with the app.</span></span> <span data-ttu-id="8dc42-144">这可让用户无需创建和记住站点密码，但也会使用户依赖外部登录提供程序。</span><span class="sxs-lookup"><span data-stu-id="8dc42-144">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="8dc42-145">如果外部登录提供程序不可用，则无法登录网站。</span><span class="sxs-lookup"><span data-stu-id="8dc42-145">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="8dc42-146">使用外部提供程序在登录过程中设置的电子邮箱创建密码和登录：</span><span class="sxs-lookup"><span data-stu-id="8dc42-146">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="8dc42-147">点击右上角的“Hello <email alias>”链接，导航到“管理”视图。</span><span class="sxs-lookup"><span data-stu-id="8dc42-147">Tap the **Hello <email alias>** link at the top right corner to navigate to the **Manage** view.</span></span>

![Web 应用程序“管理”视图](index/_static/pass1a.png)

* <span data-ttu-id="8dc42-149">点击“创建”</span><span class="sxs-lookup"><span data-stu-id="8dc42-149">Tap **Create**</span></span>

![“设置密码”页](index/_static/pass2a.png)

* <span data-ttu-id="8dc42-151">设置一个有效密码，可以用此密码和邮箱登录。</span><span class="sxs-lookup"><span data-stu-id="8dc42-151">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8dc42-152">后续步骤</span><span class="sxs-lookup"><span data-stu-id="8dc42-152">Next steps</span></span>

* <span data-ttu-id="8dc42-153">本文介绍了外部身份验证，并说明了向 ASP.NET Core 应用添加外部登录所需的先决条件。</span><span class="sxs-lookup"><span data-stu-id="8dc42-153">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="8dc42-154">引用特定于提供程序的页，为应用所需的提供程序配置登录。</span><span class="sxs-lookup"><span data-stu-id="8dc42-154">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
