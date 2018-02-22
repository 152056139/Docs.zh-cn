---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: "电子邮件确认及密码重置 (C#) 使用日志中，创建安全的 ASP.NET MVC 5 web 应用程序 |Microsoft 文档"
author: Rick-Anderson
description: "本教程演示了如何生成与电子邮件确认及密码重置使用 ASP.NET 标识成员资格系统的 ASP.NET MVC 5 web 应用程序。 Ca..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: d55b34135d5bab98ab8de31cc4b12dcc272cbc0a
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2018
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="2ebd1-104">创建安全的 ASP.NET MVC 5 web 应用程序日志中，使用电子邮件确认及密码重置 (C#)</span><span class="sxs-lookup"><span data-stu-id="2ebd1-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>
====================
<span data-ttu-id="2ebd1-105">通过[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="2ebd1-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="2ebd1-106">本教程演示了如何生成与电子邮件确认及密码重置使用 ASP.NET 标识成员资格系统的 ASP.NET MVC 5 web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span> <span data-ttu-id="2ebd1-107">你可以下载已完成的应用程序[此处](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-107">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="2ebd1-108">下载包含能让你无需设置电子邮件或 SMS 提供程序中测试电子邮件确认和短信的调试帮助程序。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-108">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="2ebd1-109">本教程编写的[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-109">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="2ebd1-110">创建 ASP.NET MVC 应用程序</span><span class="sxs-lookup"><span data-stu-id="2ebd1-110">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="2ebd1-111">首先，安装并运行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-111">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="2ebd1-112">安装[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-112">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="2ebd1-113">警告： 你必须安装[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本以完成本教程。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-113">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="2ebd1-114">创建一个新的 ASP.NET Web 项目，然后选择 MVC 模板中。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-114">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="2ebd1-115">Web 窗体还支持 ASP.NET 标识，因此无法执行类似的步骤，在 web 窗体应用程序中。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-115">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="2ebd1-116">保留默认的身份验证作为**单个用户帐户**。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-116">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="2ebd1-117">如果你想要托管该应用程序在 Azure 中的，保留选中复选框。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-117">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="2ebd1-118">稍后在本教程中我们将部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-118">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="2ebd1-119">你可以[免费建立一个 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-119">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="2ebd1-120">设置[项目以使用 SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-120">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="2ebd1-121">运行应用程序，请单击**注册**链接并注册用户。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-121">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="2ebd1-122">此时，电子邮件的唯一验证是使用[[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)属性。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-122">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="2ebd1-123">在服务器资源管理器，导航到**数据 Connections\DefaultConnection\Tables\AspNetUsers**，右键单击并选择**打开表定义**。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-123">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="2ebd1-124">下图显示`AspNetUsers`架构：</span><span class="sxs-lookup"><span data-stu-id="2ebd1-124">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="2ebd1-125">右键单击**AspNetUsers**表，然后选择**显示表数据**。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-125">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="2ebd1-126">此时不已确认电子邮件。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-126">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="2ebd1-127">单击对应的行，并选择删除。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-127">Click on the row and select delete.</span></span> <span data-ttu-id="2ebd1-128">将在下一步的步骤中，再次添加此电子邮件并发送确认电子邮件。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-128">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="2ebd1-129">电子邮件确认</span><span class="sxs-lookup"><span data-stu-id="2ebd1-129">Email confirmation</span></span>

<span data-ttu-id="2ebd1-130">它是一种最佳做法以确认新的用户注册，以验证它们不模拟其他人的电子邮件 （即，它们尚未注册与其他人的电子邮件）。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-130">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="2ebd1-131">假设你有论坛，你想要防止`"bob@example.com"`从将注册为`"joe@contoso.com"`。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-131">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="2ebd1-132">而无需电子邮件确认`"joe@contoso.com"`无法从您的应用程序获取不需要的电子邮件。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-132">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="2ebd1-133">假设 Bob 意外注册为`"bib@example.com"`和未注意到，他将无法使用密码恢复，因为此应用程序没有他正确的电子邮件。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-133">Suppose Bob accidently registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="2ebd1-134">电子邮件确认从机器人提供有限的保护，并从确定垃圾邮件发送者不对提供保护，它们具有许多可用于注册的工作电子邮件别名。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-134">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="2ebd1-135">你通常想要使新用户不能发布到网站的任何数据之前已通过电子邮件，SMS 文本消息或其他机制对它们已确认。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-135">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="2ebd1-136">在下面部分中，我们将启用电子邮件确认并修改代码以防止新注册的用户登录，直到已确认其电子邮件。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-136">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="2ebd1-137">挂钩 SendGrid</span><span class="sxs-lookup"><span data-stu-id="2ebd1-137">Hook up SendGrid</span></span>

<span data-ttu-id="2ebd1-138">虽然本教程只显示如何添加通过电子邮件通知[SendGrid](http://sendgrid.com/)，你可以发送电子邮件使用 SMTP 和其他机制 (请参阅[其他资源](#addRes))。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-138">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="2ebd1-139">在程序包管理器控制台中，输入以下以下命令：</span><span class="sxs-lookup"><span data-stu-id="2ebd1-139">In the Package Manager Console, enter the following the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="2ebd1-140">转到[Azure SendGrid 注册页面](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409)并注册一个免费的 SendGrid 帐户。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-140">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="2ebd1-141">通过添加类似于在以下代码来配置 SendGrid *App_Start/IdentityConfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ebd1-141">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="2ebd1-142">你将需要添加下列内容包括：</span><span class="sxs-lookup"><span data-stu-id="2ebd1-142">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="2ebd1-143">为了简化此示例，我们将存储中的应用设置*web.config*文件：</span><span class="sxs-lookup"><span data-stu-id="2ebd1-143">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="2ebd1-144">安全-永远不会存储在源代码中敏感数据。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-144">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="2ebd1-145">帐户和凭据存储在 appSetting。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-145">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="2ebd1-146">在 Azure 上，你可以安全地存储这些值在**[配置](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**在 Azure 门户中的选项卡。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-146">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="2ebd1-147">请参阅[用于密码和其他敏感数据部署到 ASP.NET 和 Azure 的最佳做法](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-147">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>


### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="2ebd1-148">启用帐户控制器中的电子邮件确认</span><span class="sxs-lookup"><span data-stu-id="2ebd1-148">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="2ebd1-149">验证*Views\Account\ConfirmEmail.cshtml*文件具有正确的 razor 语法。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-149">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="2ebd1-150">(在第一个字符 @ 行可能已丢失。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-150">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="2ebd1-151">)</span><span class="sxs-lookup"><span data-stu-id="2ebd1-151">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="2ebd1-152">运行应用并单击注册链接。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-152">Run the app and click the Register link.</span></span> <span data-ttu-id="2ebd1-153">一旦提交注册表单，你已登录。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-153">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="2ebd1-154">检查你的电子邮件帐户，然后单击链接以确认你的电子邮件。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-154">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="2ebd1-155">需要电子邮件确认之前登录</span><span class="sxs-lookup"><span data-stu-id="2ebd1-155">Require email confirmation before log in</span></span>

<span data-ttu-id="2ebd1-156">当前用户完成后注册表单，则将它们记录在中。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-156">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="2ebd1-157">你通常想要在中记录日志之前，请确认其电子邮件。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-157">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="2ebd1-158">在下面的部分中，我们将修改代码后，需要以具有确认电子邮件，然后将它们记录在 （经过身份验证） 的新用户。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-158">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="2ebd1-159">更新`HttpPost Register`与以下突出显示的更改的方法：</span><span class="sxs-lookup"><span data-stu-id="2ebd1-159">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="2ebd1-160">通过注释掉`SignInAsync`方法，用户不会进行签名的注册。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-160">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="2ebd1-161">`TempData["ViewBagLink"] = callbackUrl;`行可用于[调试应用程序](#dbg)并测试而不发送电子邮件的注册。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-161">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="2ebd1-162">`ViewBag.Message` 用于显示的确认说明进行操作。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-162">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="2ebd1-163">[下载示例](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)包含代码来测试电子邮件确认，无需设置电子邮件，并且也可以使用来调试该应用程序。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-163">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="2ebd1-164">创建`Views\Shared\Info.cshtml`文件并添加以下 razor 标记：</span><span class="sxs-lookup"><span data-stu-id="2ebd1-164">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="2ebd1-165">添加[Authorize 属性](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx)到`Contact`主控制器的操作方法。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-165">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="2ebd1-166">你可以使用单击**联系人**链接以验证匿名用户无权访问和经过身份验证的用户具有访问权限。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-166">You can use click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="2ebd1-167">你还必须更新`HttpPost Login`操作方法：</span><span class="sxs-lookup"><span data-stu-id="2ebd1-167">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="2ebd1-168">更新*Views\Shared\Error.cshtml*视图以显示错误消息：</span><span class="sxs-lookup"><span data-stu-id="2ebd1-168">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="2ebd1-169">删除中的任何帐户**AspNetUsers**表包含你要测试的电子邮件别名。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-169">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="2ebd1-170">运行应用程序并验证直到您已确认你的电子邮件地址无法登录。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-170">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="2ebd1-171">一旦确认你的电子邮件地址，请单击**联系人**链接。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-171">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="2ebd1-172">密码恢复/重置</span><span class="sxs-lookup"><span data-stu-id="2ebd1-172">Password recovery/reset</span></span>

<span data-ttu-id="2ebd1-173">删除注释字符从`HttpPost ForgotPassword`帐户控制器中的操作方法：</span><span class="sxs-lookup"><span data-stu-id="2ebd1-173">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="2ebd1-174">删除注释字符从`ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx)中*Views\Account\Login.cshtml* razor 视图文件：</span><span class="sxs-lookup"><span data-stu-id="2ebd1-174">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="2ebd1-175">登录页现在将显示的链接重置的密码。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-175">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="2ebd1-176">重新发送电子邮件确认链接</span><span class="sxs-lookup"><span data-stu-id="2ebd1-176">Resend email confirmation link</span></span>

<span data-ttu-id="2ebd1-177">一旦用户创建新的本地帐户，它们发送需要它们来使用它们可以登录之前的确认链接。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-177">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="2ebd1-178">如果用户意外删除确认电子邮件，或电子邮件永远不会到达时，他们将需要再次发送的确认链接。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-178">If the user accidently deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="2ebd1-179">以下代码更改显示如何启用此功能。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-179">The following code changes show how to enable this.</span></span>

<span data-ttu-id="2ebd1-180">将下面的 helper 方法添加到底部*Controllers\AccountController.cs*文件：</span><span class="sxs-lookup"><span data-stu-id="2ebd1-180">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="2ebd1-181">更新要使用的新的帮助程序的注册方法：</span><span class="sxs-lookup"><span data-stu-id="2ebd1-181">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="2ebd1-182">更新要重新发送密码的登录方法时如果尚未确认的用户帐户：</span><span class="sxs-lookup"><span data-stu-id="2ebd1-182">Update the Login method to resend the password when if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="2ebd1-183">合并社交和本地登录帐户</span><span class="sxs-lookup"><span data-stu-id="2ebd1-183">Combine social and local login accounts</span></span>

<span data-ttu-id="2ebd1-184">你可以通过单击电子邮件链接组合本地和社交帐户。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-184">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="2ebd1-185">按以下顺序 **RickAndMSFT@gmail.com** 最初创建为本地登录名，但你可以创建帐户作为社交日志中的第一个，然后添加本地登录名。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-185">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="2ebd1-186">单击**管理**链接。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-186">Click on the **Manage** link.</span></span> <span data-ttu-id="2ebd1-187">请注意与此帐户关联 0 外部 （社交登录名）。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-187">Note the 0 external (social logins) associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="2ebd1-188">单击服务中的另一个日志的链接，接受应用程序请求。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-188">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="2ebd1-189">现在合并两个帐户，你将能够使用任一帐户登录。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-189">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="2ebd1-190">你可能希望用户身份验证服务中的其社交日志已关闭，或已对其社交帐户失去访问更有可能的情况下添加本地帐户。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-190">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="2ebd1-191">在下图中，Tom 是一个社交登录 (您可以看到从该**外部登录名： 1**的页上显示)。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-191">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="2ebd1-192">单击**挑选一个密码**允许你上添加本地日志与相同的帐户关联。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-192">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="2ebd1-193">中更深入的电子邮件确认</span><span class="sxs-lookup"><span data-stu-id="2ebd1-193">Email confirmation in more depth</span></span>

<span data-ttu-id="2ebd1-194">我的教程[帐户确认和密码恢复 ASP.NET 标识](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)进入更多详细信息与本主题。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-194">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="2ebd1-195">调试应用程序</span><span class="sxs-lookup"><span data-stu-id="2ebd1-195">Debugging the app</span></span>

<span data-ttu-id="2ebd1-196">如果你不会获得电子邮件包含链接：</span><span class="sxs-lookup"><span data-stu-id="2ebd1-196">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="2ebd1-197">请检查垃圾邮件或垃圾邮件文件夹。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-197">Check your junk or spam folder.</span></span>
- <span data-ttu-id="2ebd1-198">登录到你的 SendGrid 帐户，然后单击[电子邮件活动链接](https://sendgrid.com/logs/index)。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-198">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="2ebd1-199">若要测试验证链接不电子邮件的情况下，下载[已完成的示例](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-199">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="2ebd1-200">将页面上显示的确认链接和确认代码。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-200">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="2ebd1-201">其他资源</span><span class="sxs-lookup"><span data-stu-id="2ebd1-201">Additional Resources</span></span>

- [<span data-ttu-id="2ebd1-202">指向 ASP.NET Identity 的推荐资源</span><span class="sxs-lookup"><span data-stu-id="2ebd1-202">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="2ebd1-203">[帐户确认和密码恢复具有 ASP.NET 标识](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)进入密码恢复和帐户确认的详细信息。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-203">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="2ebd1-204">[使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登录的 MVC 5 应用程序](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)本教程演示如何编写使用 Facebook 和 Google oauth2 授权 ASP.NET MVC 5 应用程序。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-204">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="2ebd1-205">它还演示如何向标识数据库中添加额外的数据。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-205">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="2ebd1-206">[将包含成员资格、 OAuth 和 SQL 数据库的安全 ASP.NET MVC 应用程序部署到 Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-206">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="2ebd1-207">本教程将添加 Azure 部署，如何保护使用角色，对应用程序如何使用成员资格 API 来添加用户和角色，以及其他安全功能。</span><span class="sxs-lookup"><span data-stu-id="2ebd1-207">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="2ebd1-208">创建 oauth2 的 Google 应用和将应用程序连接到项目</span><span class="sxs-lookup"><span data-stu-id="2ebd1-208">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="2ebd1-209">在 Facebook 中创建应用并将应用程序连接到项目</span><span class="sxs-lookup"><span data-stu-id="2ebd1-209">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="2ebd1-210">在项目中的 SSL 设置</span><span class="sxs-lookup"><span data-stu-id="2ebd1-210">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
