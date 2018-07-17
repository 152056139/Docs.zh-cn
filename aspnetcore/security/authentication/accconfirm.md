---
title: 帐户确认和 ASP.NET Core 中的密码恢复
author: rick-anderson
description: 了解如何生成使用电子邮件确认及密码重置功能的 ASP.NET Core 应用程序。
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 8e175cd19ca4a9de1e7cf6b330b3d82f309b6501
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063333"
---
::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="b428c-103">请参阅[此 PDF 文件](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf)ASP.NET Core 1.1 和 2.1 版本。</span><span class="sxs-lookup"><span data-stu-id="b428c-103">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="b428c-104">帐户确认和 ASP.NET Core 中的密码恢复</span><span class="sxs-lookup"><span data-stu-id="b428c-104">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="b428c-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="b428c-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="b428c-106">本教程演示如何生成 ASP.NET Core 应用使用电子邮件确认及密码重置。</span><span class="sxs-lookup"><span data-stu-id="b428c-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="b428c-107">本教程是**不**开头主题。</span><span class="sxs-lookup"><span data-stu-id="b428c-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="b428c-108">您应熟悉：</span><span class="sxs-lookup"><span data-stu-id="b428c-108">You should be familiar with:</span></span>

* [<span data-ttu-id="b428c-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b428c-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="b428c-110">身份验证</span><span class="sxs-lookup"><span data-stu-id="b428c-110">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="b428c-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="b428c-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="b428c-112">系统必备</span><span class="sxs-lookup"><span data-stu-id="b428c-112">Prerequisites</span></span>

<span data-ttu-id="b428c-113">[!INCLUDE [](~/includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="b428c-113">[!INCLUDE [](~/includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]</span></span>

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="b428c-114">创建 web 应用并创建标识的基架</span><span class="sxs-lookup"><span data-stu-id="b428c-114">Create a web  app and scaffold Identity</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b428c-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b428c-115">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="b428c-116">在 Visual Studio 中，创建一个新**Web 应用程序**项目。</span><span class="sxs-lookup"><span data-stu-id="b428c-116">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="b428c-117">选择**ASP.NET Core 2.1**。</span><span class="sxs-lookup"><span data-stu-id="b428c-117">Select **ASP.NET Core 2.1**.</span></span>
* <span data-ttu-id="b428c-118">保留默认值**身份验证**设置为**无身份验证**。</span><span class="sxs-lookup"><span data-stu-id="b428c-118">Keep the default **Authentication** set to **No Authentication**.</span></span> <span data-ttu-id="b428c-119">下一步中添加身份验证。</span><span class="sxs-lookup"><span data-stu-id="b428c-119">Authentication is added in the next step.</span></span>

<span data-ttu-id="b428c-120">在下一步：</span><span class="sxs-lookup"><span data-stu-id="b428c-120">In the next step:</span></span>

* <span data-ttu-id="b428c-121">设置布局页为 *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b428c-121">Set the layout page to *~/Pages/Shared/_Layout.cshtml*</span></span>
* <span data-ttu-id="b428c-122">选择*帐户/注册*</span><span class="sxs-lookup"><span data-stu-id="b428c-122">Select *Account/Register*</span></span>
* <span data-ttu-id="b428c-123">创建一个新**数据上下文类**</span><span class="sxs-lookup"><span data-stu-id="b428c-123">Create a new **Data context class**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b428c-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b428c-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet new webapp -o WebPWrecover
cd WebPWrecover
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -fi Account.Register -dc WebPWrecover.Models.WebPWrecoverContext
dotnet ef migrations add CreateIdentitySchema
dotnet ef database drop -f
dotnet ef database update
dotnet build
```

<span data-ttu-id="b428c-125">运行`dotnet aspnet-codegenerator identity --help`以获取有关基架工具的帮助。</span><span class="sxs-lookup"><span data-stu-id="b428c-125">Run `dotnet aspnet-codegenerator identity --help` to get help on the scaffolding tool.</span></span>

------

<span data-ttu-id="b428c-126">按照中的说明[启用身份验证](xref:security/authentication/scaffold-identity#useauthentication):</span><span class="sxs-lookup"><span data-stu-id="b428c-126">Follow the instructions in [Enable authentication](xref:security/authentication/scaffold-identity#useauthentication):</span></span>

* <span data-ttu-id="b428c-127">添加`app.UseAuthentication();`到 `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="b428c-127">Add `app.UseAuthentication();` to `Startup.Configure`</span></span>
* <span data-ttu-id="b428c-128">添加`<partial name="_LoginPartial" />`布局文件。</span><span class="sxs-lookup"><span data-stu-id="b428c-128">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="b428c-129">测试新的用户注册</span><span class="sxs-lookup"><span data-stu-id="b428c-129">Test new user registration</span></span>

<span data-ttu-id="b428c-130">运行该应用程序中，选择**注册**链接，并注册用户。</span><span class="sxs-lookup"><span data-stu-id="b428c-130">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="b428c-131">在此情况下，电子邮件的唯一验证是使用[[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)属性。</span><span class="sxs-lookup"><span data-stu-id="b428c-131">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="b428c-132">提交后注册，登录到应用程序。</span><span class="sxs-lookup"><span data-stu-id="b428c-132">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="b428c-133">更高版本在本教程中，以便验证其电子邮件之前，新用户无法登录。 更新代码。</span><span class="sxs-lookup"><span data-stu-id="b428c-133">Later in the tutorial, the code is updated so new users can't log in until their email is validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="b428c-134">查看标识数据库</span><span class="sxs-lookup"><span data-stu-id="b428c-134">View the Identity database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b428c-135">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b428c-135">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="b428c-136">从**视图**菜单中，选择**SQL Server 对象资源管理器**(SSOX)。</span><span class="sxs-lookup"><span data-stu-id="b428c-136">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="b428c-137">导航到 **(localdb) MSSQLLocalDB (SQL Server 13)**。</span><span class="sxs-lookup"><span data-stu-id="b428c-137">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="b428c-138">右键单击**dbo。AspNetUsers** > **查看数据**:</span><span class="sxs-lookup"><span data-stu-id="b428c-138">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![在 AspNetUsers 表中 SQL Server 对象资源管理器的上下文菜单](accconfirm/_static/ssox.png)

<span data-ttu-id="b428c-140">请注意此表的`EmailConfirmed`字段是`False`。</span><span class="sxs-lookup"><span data-stu-id="b428c-140">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="b428c-141">您可能想要再次在下一步中使用此电子邮件，该应用将发送确认电子邮件时。</span><span class="sxs-lookup"><span data-stu-id="b428c-141">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="b428c-142">右键单击行并选择**删除**。</span><span class="sxs-lookup"><span data-stu-id="b428c-142">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="b428c-143">删除电子邮件别名便于您在以下步骤。</span><span class="sxs-lookup"><span data-stu-id="b428c-143">Deleting the email alias makes it easier in the following steps.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b428c-144">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b428c-144">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b428c-145">请参阅[ASP.NET Core MVC 项目中使用的 SQLite](xref:tutorials/first-mvc-app-xplat/working-with-sql)有关说明如何查看 SQLite 数据库。</span><span class="sxs-lookup"><span data-stu-id="b428c-145">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

------

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="b428c-146">需要电子邮件确认</span><span class="sxs-lookup"><span data-stu-id="b428c-146">Require email confirmation</span></span>

<span data-ttu-id="b428c-147">它是最佳做法以确认新的用户注册的电子邮件地址。</span><span class="sxs-lookup"><span data-stu-id="b428c-147">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="b428c-148">电子邮件确认可帮助以验证它们没有模拟其他人 （即，他们尚未注册与其他人的电子邮件）。</span><span class="sxs-lookup"><span data-stu-id="b428c-148">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="b428c-149">假设有论坛，并且您想阻止"yli@example.com"中注册为"nolivetto@contoso.com"。</span><span class="sxs-lookup"><span data-stu-id="b428c-149">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="b428c-150">而无需电子邮件确认"nolivetto@contoso.com"可能会从您的应用程序收到不需要的电子邮件。</span><span class="sxs-lookup"><span data-stu-id="b428c-150">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="b428c-151">假设用户意外地注册为"ylo@example.com"和"yli"的拼写错误的重视。</span><span class="sxs-lookup"><span data-stu-id="b428c-151">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="b428c-152">它们将无法使用密码恢复，因为此应用没有其正确的电子邮件。</span><span class="sxs-lookup"><span data-stu-id="b428c-152">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="b428c-153">电子邮件确认从智能机器人提供有限的保护。</span><span class="sxs-lookup"><span data-stu-id="b428c-153">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="b428c-154">电子邮件确认不提供保护，免受恶意用户的与许多电子邮件帐户。</span><span class="sxs-lookup"><span data-stu-id="b428c-154">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="b428c-155">你通常想要发布到网站的任何数据，有已确认的电子邮件之前阻止新用户。</span><span class="sxs-lookup"><span data-stu-id="b428c-155">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="b428c-156">更新*Areas/Identity/IdentityHostingStartup.cs*需要确认电子邮件：</span><span class="sxs-lookup"><span data-stu-id="b428c-156">Update *Areas/Identity/IdentityHostingStartup.cs*  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

<span data-ttu-id="b428c-157">`config.SignIn.RequireConfirmedEmail = true;` 可防止已注册的用户登录之前确认其电子邮件。</span><span class="sxs-lookup"><span data-stu-id="b428c-157">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="b428c-158">配置电子邮件提供商</span><span class="sxs-lookup"><span data-stu-id="b428c-158">Configure email provider</span></span>

<span data-ttu-id="b428c-159">在本教程中， [SendGrid](https://sendgrid.com)用于发送电子邮件。</span><span class="sxs-lookup"><span data-stu-id="b428c-159">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="b428c-160">你需要一个 SendGrid 帐户和密钥用于发送电子邮件。</span><span class="sxs-lookup"><span data-stu-id="b428c-160">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="b428c-161">可以使用其他电子邮件提供商。</span><span class="sxs-lookup"><span data-stu-id="b428c-161">You can use other email providers.</span></span> <span data-ttu-id="b428c-162">ASP.NET Core 2.x 包括`System.Net.Mail`，这允许你从你的应用程序发送电子邮件。</span><span class="sxs-lookup"><span data-stu-id="b428c-162">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="b428c-163">我们建议你使用 SendGrid 或另一个电子邮件服务发送电子邮件。</span><span class="sxs-lookup"><span data-stu-id="b428c-163">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="b428c-164">SMTP 很难保护并正确设置。</span><span class="sxs-lookup"><span data-stu-id="b428c-164">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="b428c-165">[选项模式](xref:fundamentals/configuration/options)用于访问用户帐户和密钥设置。</span><span class="sxs-lookup"><span data-stu-id="b428c-165">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="b428c-166">有关详细信息，请参阅[配置](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="b428c-166">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="b428c-167">创建一个类来提取的安全电子邮件键。</span><span class="sxs-lookup"><span data-stu-id="b428c-167">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="b428c-168">对于此示例中，创建*Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="b428c-168">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="b428c-169">配置 SendGrid 用户机密</span><span class="sxs-lookup"><span data-stu-id="b428c-169">Configure SendGrid user secrets</span></span>

<span data-ttu-id="b428c-170">添加一个唯一`<UserSecretsId>`值设为`<PropertyGroup>`项目文件的元素：</span><span class="sxs-lookup"><span data-stu-id="b428c-170">Add a unique `<UserSecretsId>` value to the `<PropertyGroup>` element of the project file:</span></span>

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

<span data-ttu-id="b428c-171">设置`SendGridUser`并`SendGridKey`与[机密管理器工具](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="b428c-171">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="b428c-172">例如：</span><span class="sxs-lookup"><span data-stu-id="b428c-172">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="b428c-173">在 Windows 中，机密管理器存储中的键/值对*secrets.json*文件中`%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`目录。</span><span class="sxs-lookup"><span data-stu-id="b428c-173">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="b428c-174">内容*secrets.json*未加密文件。</span><span class="sxs-lookup"><span data-stu-id="b428c-174">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="b428c-175">*Secrets.json*文件如下所示 (`SendGridKey`删除值。)</span><span class="sxs-lookup"><span data-stu-id="b428c-175">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="install-sendgrid"></a><span data-ttu-id="b428c-176">安装 SendGrid</span><span class="sxs-lookup"><span data-stu-id="b428c-176">Install SendGrid</span></span>

<span data-ttu-id="b428c-177">本教程演示如何添加通过电子邮件通知[SendGrid](https://sendgrid.com/)，但是你可以发送电子邮件使用 SMTP 和其他机制。</span><span class="sxs-lookup"><span data-stu-id="b428c-177">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="b428c-178">安装`SendGrid`NuGet 包：</span><span class="sxs-lookup"><span data-stu-id="b428c-178">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b428c-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b428c-179">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="b428c-180">从包管理器控制台中，输入以下命令：</span><span class="sxs-lookup"><span data-stu-id="b428c-180">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b428c-181">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b428c-181">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b428c-182">从控制台中，输入以下命令：</span><span class="sxs-lookup"><span data-stu-id="b428c-182">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="b428c-183">请参阅[免费开始使用 SendGrid](https://sendgrid.com/free/)注册一个免费的 SendGrid 帐户。</span><span class="sxs-lookup"><span data-stu-id="b428c-183">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>
### <a name="implement-iemailsender"></a><span data-ttu-id="b428c-184">实现 IEmailSender</span><span class="sxs-lookup"><span data-stu-id="b428c-184">Implement IEmailSender</span></span>

<span data-ttu-id="b428c-185">为实现`IEmailSender`，创建*Services/EmailSender.cs* ，类似于以下代码：</span><span class="sxs-lookup"><span data-stu-id="b428c-185">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="b428c-186">将启动以支持电子邮件配置</span><span class="sxs-lookup"><span data-stu-id="b428c-186">Configure startup to support email</span></span>

<span data-ttu-id="b428c-187">将以下代码添加到`ConfigureServices`中的方法*Startup.cs*文件：</span><span class="sxs-lookup"><span data-stu-id="b428c-187">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="b428c-188">添加`EmailSender`作为单一实例服务。</span><span class="sxs-lookup"><span data-stu-id="b428c-188">Add `EmailSender` as a singleton service.</span></span>
* <span data-ttu-id="b428c-189">注册`AuthMessageSenderOptions`配置实例。</span><span class="sxs-lookup"><span data-stu-id="b428c-189">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="b428c-190">启用帐户确认和密码恢复</span><span class="sxs-lookup"><span data-stu-id="b428c-190">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="b428c-191">该模板具有帐户确认和密码恢复的代码。</span><span class="sxs-lookup"><span data-stu-id="b428c-191">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="b428c-192">查找`OnPostAsync`中的方法*Areas/Identity/Pages/Account/Register.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="b428c-192">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="b428c-193">禁止新注册的用户将自动记录通过注释掉以下行：</span><span class="sxs-lookup"><span data-stu-id="b428c-193">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="b428c-194">完整的方法与更改突出显示的行所示：</span><span class="sxs-lookup"><span data-stu-id="b428c-194">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="b428c-195">注册、 确认电子邮件，和重置密码</span><span class="sxs-lookup"><span data-stu-id="b428c-195">Register, confirm email, and reset password</span></span>

<span data-ttu-id="b428c-196">运行 web 应用和测试的帐户确认和密码恢复流。</span><span class="sxs-lookup"><span data-stu-id="b428c-196">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="b428c-197">运行应用并注册一个新用户</span><span class="sxs-lookup"><span data-stu-id="b428c-197">Run the app and register a new user</span></span>

  ![Web 应用程序帐户注册视图](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="b428c-199">检查你的帐户确认链接的电子邮件。</span><span class="sxs-lookup"><span data-stu-id="b428c-199">Check your email for the account confirmation link.</span></span> <span data-ttu-id="b428c-200">请参阅[调试电子邮件](#debug)如果没有收到电子邮件。</span><span class="sxs-lookup"><span data-stu-id="b428c-200">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="b428c-201">单击链接以确认你的电子邮件。</span><span class="sxs-lookup"><span data-stu-id="b428c-201">Click the link to confirm your email.</span></span>
* <span data-ttu-id="b428c-202">电子邮件和密码登录。</span><span class="sxs-lookup"><span data-stu-id="b428c-202">Log in with your email and password.</span></span>
* <span data-ttu-id="b428c-203">注销。</span><span class="sxs-lookup"><span data-stu-id="b428c-203">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="b428c-204">查看管理页</span><span class="sxs-lookup"><span data-stu-id="b428c-204">View the manage page</span></span>

<span data-ttu-id="b428c-205">在浏览器中选择你的用户名：![用户名的浏览器窗口](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="b428c-205">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="b428c-206">您可能需要展开导航栏以查看用户名称。</span><span class="sxs-lookup"><span data-stu-id="b428c-206">You might need to expand the navbar to see user name.</span></span>

![导航栏](accconfirm/_static/x.png)

<span data-ttu-id="b428c-208">管理页显示与**配置文件**选定的选项卡。</span><span class="sxs-lookup"><span data-stu-id="b428c-208">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="b428c-209">**电子邮件**显示已确认一个复选框，表明该电子邮件。</span><span class="sxs-lookup"><span data-stu-id="b428c-209">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="b428c-210">测试密码重置</span><span class="sxs-lookup"><span data-stu-id="b428c-210">Test password reset</span></span>

* <span data-ttu-id="b428c-211">如果在登录，请选择**注销**。</span><span class="sxs-lookup"><span data-stu-id="b428c-211">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="b428c-212">选择**登录**链接，然后选择**忘记了密码？** 链接。</span><span class="sxs-lookup"><span data-stu-id="b428c-212">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="b428c-213">输入用于注册该帐户的电子邮件。</span><span class="sxs-lookup"><span data-stu-id="b428c-213">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="b428c-214">发送一封电子邮件包含用于重置密码的链接。</span><span class="sxs-lookup"><span data-stu-id="b428c-214">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="b428c-215">检查你的电子邮件，并单击链接以重置密码。</span><span class="sxs-lookup"><span data-stu-id="b428c-215">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="b428c-216">已成功重置密码后，可以登录你的电子邮件和新密码。</span><span class="sxs-lookup"><span data-stu-id="b428c-216">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="b428c-217">调试电子邮件</span><span class="sxs-lookup"><span data-stu-id="b428c-217">Debug email</span></span>

<span data-ttu-id="b428c-218">如果无法获取电子邮件工作：</span><span class="sxs-lookup"><span data-stu-id="b428c-218">If you can't get email working:</span></span>

* <span data-ttu-id="b428c-219">中设置断点`EmailSender.Execute`若要验证`SendGridClient.SendEmailAsync`调用。</span><span class="sxs-lookup"><span data-stu-id="b428c-219">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="b428c-220">创建[控制台应用以发送电子邮件](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)使用类似的代码，到`EmailSender.Execute`。</span><span class="sxs-lookup"><span data-stu-id="b428c-220">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="b428c-221">审阅[电子邮件活动](https://sendgrid.com/docs/User_Guide/email_activity.html)页。</span><span class="sxs-lookup"><span data-stu-id="b428c-221">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="b428c-222">请检查垃圾邮件文件夹。</span><span class="sxs-lookup"><span data-stu-id="b428c-222">Check your spam folder.</span></span>
* <span data-ttu-id="b428c-223">请尝试其他电子邮件提供程序 （Microsoft、 Yahoo、 Gmail 等） 上的另一个电子邮件别名</span><span class="sxs-lookup"><span data-stu-id="b428c-223">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="b428c-224">尝试发送到不同的电子邮件帐户。</span><span class="sxs-lookup"><span data-stu-id="b428c-224">Try sending to different email accounts.</span></span>

<span data-ttu-id="b428c-225">**最佳安全方案**设置为**不**中使用生产机密中测试和开发。</span><span class="sxs-lookup"><span data-stu-id="b428c-225">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="b428c-226">如果将应用发布到 Azure，可以为 Azure Web 应用门户中的应用程序设置来设置 SendGrid 机密。</span><span class="sxs-lookup"><span data-stu-id="b428c-226">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="b428c-227">配置系统设置以从环境变量读取密钥。</span><span class="sxs-lookup"><span data-stu-id="b428c-227">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="b428c-228">合并的社交和本地登录帐户</span><span class="sxs-lookup"><span data-stu-id="b428c-228">Combine social and local login accounts</span></span>

<span data-ttu-id="b428c-229">若要完成本部分中，必须先启用外部身份验证提供程序。</span><span class="sxs-lookup"><span data-stu-id="b428c-229">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="b428c-230">请参阅[Facebook、 Google、 和外部提供程序身份验证](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="b428c-230">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="b428c-231">您可以通过单击电子邮件链接组合本地和社交帐户。</span><span class="sxs-lookup"><span data-stu-id="b428c-231">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="b428c-232">按以下顺序"RickAndMSFT@gmail.com"首次创建为本地登录名; 但是，您可以创建帐户为社交登录名，然后添加本地登录名。</span><span class="sxs-lookup"><span data-stu-id="b428c-232">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web 应用程序：RickAndMSFT@gmail.com用户通过身份验证](accconfirm/_static/rick.png)

<span data-ttu-id="b428c-234">单击**管理**链接。</span><span class="sxs-lookup"><span data-stu-id="b428c-234">Click on the **Manage** link.</span></span> <span data-ttu-id="b428c-235">请注意与此帐户关联的 0 外部 （社交登录名）。</span><span class="sxs-lookup"><span data-stu-id="b428c-235">Note the 0 external (social logins) associated with this account.</span></span>

![管理视图](accconfirm/_static/manage.png)

<span data-ttu-id="b428c-237">单击链接到另一个登录名服务，接受应用程序请求。</span><span class="sxs-lookup"><span data-stu-id="b428c-237">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="b428c-238">下图中，在 Facebook 是外部身份验证提供程序：</span><span class="sxs-lookup"><span data-stu-id="b428c-238">In the following image, Facebook is the external authentication provider:</span></span>

![管理你的外部登录名视图，其中列出 Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="b428c-240">已合并两个帐户。</span><span class="sxs-lookup"><span data-stu-id="b428c-240">The two accounts have been combined.</span></span> <span data-ttu-id="b428c-241">你将能够使用任一帐户登录。</span><span class="sxs-lookup"><span data-stu-id="b428c-241">You are able to log on with either account.</span></span> <span data-ttu-id="b428c-242">您可能希望用户其社交登录身份验证服务发生故障，或已为其社交帐户无法访问更有可能的情况下添加本地帐户。</span><span class="sxs-lookup"><span data-stu-id="b428c-242">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="b428c-243">后一个站点中有用户启用帐户确认</span><span class="sxs-lookup"><span data-stu-id="b428c-243">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="b428c-244">与用户启用帐户确认站点上的阻止所有现有用户。</span><span class="sxs-lookup"><span data-stu-id="b428c-244">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="b428c-245">现有用户被锁定，因为不确认其帐户。</span><span class="sxs-lookup"><span data-stu-id="b428c-245">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="b428c-246">若要解决现有用户锁定，请使用以下方法之一：</span><span class="sxs-lookup"><span data-stu-id="b428c-246">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="b428c-247">更新要将标记为正在确认的所有现有用户的数据库。</span><span class="sxs-lookup"><span data-stu-id="b428c-247">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="b428c-248">确认现有用户。</span><span class="sxs-lookup"><span data-stu-id="b428c-248">Confirm exiting users.</span></span> <span data-ttu-id="b428c-249">例如，批的发送确认链接的电子邮件。</span><span class="sxs-lookup"><span data-stu-id="b428c-249">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end