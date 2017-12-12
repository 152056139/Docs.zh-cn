---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: "自定义的 ASP.NET 网页 (Razor) 站点的站点范围行为 |Microsoft 文档"
author: tfitzmac
description: "本章介绍如何对整个网站或中的整个文件夹，而不是一页中进行设置。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: b1caa26a23517bd976addfefac89375ae965eb91
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a><span data-ttu-id="1c4f0-103">中的 ASP.NET Web 页 (Razor) 网站的自定义站点范围行为</span><span class="sxs-lookup"><span data-stu-id="1c4f0-103">Customizing Site-Wide Behavior for ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="1c4f0-104">通过[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1c4f0-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1c4f0-105">本文介绍如何在 ASP.NET Web 页 (Razor) 网站中生成的页面的站点端设置。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-105">This article explains how to make site-side settings for pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="1c4f0-106">你将学习：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="1c4f0-107">如何运行代码，可让你设置的站点中的所有页值 （全局值或帮助器设置）。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-107">How to run code that lets you set values (global values or helper settings) for all pages in a site.</span></span>
> - <span data-ttu-id="1c4f0-108">如何运行的代码，可让你设置的文件夹中的所有页的值。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-108">How to run code that lets you set values for all pages in a folder.</span></span>
> - <span data-ttu-id="1c4f0-109">如何运行代码之前和之后页面加载。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-109">How to run code before and after a page loads.</span></span>
> - <span data-ttu-id="1c4f0-110">如何将错误发送到中央错误页。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-110">How to send errors to a central error page.</span></span>
> - <span data-ttu-id="1c4f0-111">如何将身份验证添加到文件夹中的所有页面。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-111">How to add authentication to all pages in a folder.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1c4f0-112">在本教程中使用的软件版本</span><span class="sxs-lookup"><span data-stu-id="1c4f0-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="1c4f0-113">ASP.NET 网页 (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="1c4f0-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="1c4f0-114">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="1c4f0-114">WebMatrix 3</span></span>
> - <span data-ttu-id="1c4f0-115">ASP.NET Web 帮助程序库 （NuGet 包）</span><span class="sxs-lookup"><span data-stu-id="1c4f0-115">ASP.NET Web Helpers Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="1c4f0-116">本教程还适用于 ASP.NET Web Pages 3，并且 Visual Studio 2013 （或 Visual Studio Express 2013 for Web），除非你能使用 ASP.NET Web 帮助程序库。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-116">This tutorial also works with ASP.NET Web Pages 3 and Visual Studio 2013 (or Visual Studio Express 2013 for Web), except you cannot use the ASP.NET Web Helpers Library.</span></span>


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a><span data-ttu-id="1c4f0-117">为 ASP.NET 网页添加网站启动代码</span><span class="sxs-lookup"><span data-stu-id="1c4f0-117">Adding Website Startup Code for ASP.NET Web Pages</span></span>

<span data-ttu-id="1c4f0-118">对于很多编写 ASP.NET 网页中的代码中，单个页面可以包含具有所需的该页面的所有代码。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-118">For much of the code that you write in ASP.NET Web Pages, an individual page can contain all the code that's required for that page.</span></span> <span data-ttu-id="1c4f0-119">例如，如果页发送电子邮件，则可以为该操作的所有代码放都入的一页中。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-119">For example, if a page sends an email message, it's possible to put all the code for that operation in a single page.</span></span> <span data-ttu-id="1c4f0-120">这可能包括代码以初始化用于发送电子邮件的设置 (即，SMTP 服务器) 和用于发送电子邮件消息。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-120">This can include the code to initialize the settings for sending email (that is, for the SMTP server) and for sending the email message.</span></span>

<span data-ttu-id="1c4f0-121">但是，在某些情况下，你可能想要在站点上的任何页面运行之前运行某些代码。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-121">However, in some situations, you might want to run some code before any page on the site runs.</span></span> <span data-ttu-id="1c4f0-122">这可用于设置可以使用在站点中的任意位置的值 (称为*全局值*。)例如，一些帮助器要求你提供如电子邮件设置或帐户密钥的值。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-122">This is useful for setting values that can be used anywhere in the site (referred to as *global values*.) For example, some helpers require you to provide values like email settings or account keys.</span></span> <span data-ttu-id="1c4f0-123">它可方便起见，可以将这些设置保留在全局值。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-123">It can be handy to keep these settings in global values.</span></span>

<span data-ttu-id="1c4f0-124">你可以执行此操作通过创建一个名为页 *\_AppStart.cshtml*站点的根目录中。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-124">You can do this by creating a page named *\_AppStart.cshtml* in the root of the site.</span></span> <span data-ttu-id="1c4f0-125">如果存在此页，它将运行在首次请求站点中的任何页面。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-125">If this page exists, it runs the first time any page in the site is requested.</span></span> <span data-ttu-id="1c4f0-126">因此，它是运行代码以设置全局值的良好开端。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-126">Therefore, it's a good place to run code to set global values.</span></span> <span data-ttu-id="1c4f0-127">(因为 *\_AppStart.cshtml*具有下划线的前缀，即使用户直接请求 ASP.NET 不会将页面发送到浏览器。)</span><span class="sxs-lookup"><span data-stu-id="1c4f0-127">(Because *\_AppStart.cshtml* has an underscore prefix, ASP.NET won't send the page to a browser even if users request it directly.)</span></span>

<span data-ttu-id="1c4f0-128">下图显示了 *\_AppStart.cshtml*页上工作。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-128">The following diagram shows how the *\_AppStart.cshtml* page works.</span></span> <span data-ttu-id="1c4f0-129">当请求传入的页面，并且如果这是任何第一个请求页在站点中，ASP.NET 首先检查是否 *\_AppStart.cshtml*存在页。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-129">When a request comes in for a page, and if this is the first request for any page in the site, ASP.NET first checks whether a *\_AppStart.cshtml* page exists.</span></span> <span data-ttu-id="1c4f0-130">如果是这样中的任何代码 *\_AppStart.cshtml*页上运行，并运行请求的页。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-130">If so, any code in the *\_AppStart.cshtml* page runs, and then the requested page runs.</span></span>

![[image]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a><span data-ttu-id="1c4f0-132">为你的网站设置全局值</span><span class="sxs-lookup"><span data-stu-id="1c4f0-132">Setting Global Values for Your Website</span></span>

1. <span data-ttu-id="1c4f0-133">在 WebMatrix 网站的根文件夹中，创建名为的文件 *\_AppStart.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-133">In the root folder of a WebMatrix website, create a file named *\_AppStart.cshtml*.</span></span> <span data-ttu-id="1c4f0-134">该文件必须在站点的根目录中。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-134">The file must be in the root of the site.</span></span>
2. <span data-ttu-id="1c4f0-135">将现有内容替换为以下：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-135">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    <span data-ttu-id="1c4f0-136">此代码将值存储在`AppState`字典，会自动提供给站点中的所有页面。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-136">This code stores a value in the `AppState` dictionary, which is automatically available to all pages in the site.</span></span> <span data-ttu-id="1c4f0-137">请注意，  *\_AppStart.cshtml*文件在其中没有任何标记。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-137">Notice that the *\_AppStart.cshtml* file does not have any markup in it.</span></span> <span data-ttu-id="1c4f0-138">页将运行代码，然后重定向到最初请求的页面。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-138">The page will run the code and then redirect to the page that was originally requested.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1c4f0-139">将代码放入时要特别小心 *\_AppStart.cshtml*文件。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-139">Be careful when you put code in the *\_AppStart.cshtml* file.</span></span> <span data-ttu-id="1c4f0-140">如果在代码中出现任何错误 *\_AppStart.cshtml*文件，该网站将不会启动。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-140">If any errors occur in code in the *\_AppStart.cshtml* file, the website won't start.</span></span>
3. <span data-ttu-id="1c4f0-141">在根文件夹中，创建一个名为的新页*AppName.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-141">In the root folder, create a new page named *AppName.cshtml*.</span></span>
4. <span data-ttu-id="1c4f0-142">将替换为以下默认标记和代码：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-142">Replace the default markup and code with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    <span data-ttu-id="1c4f0-143">此代码中提取的值从`AppState`中设置的对象 *\_AppStart.cshtml*页。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-143">This code extracts the value from the `AppState` object that you set in the *\_AppStart.cshtml* page.</span></span>
5. <span data-ttu-id="1c4f0-144">运行*AppName.cshtml*在浏览器中的页。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-144">Run the *AppName.cshtml* page in a browser.</span></span> <span data-ttu-id="1c4f0-145">(请确保页中选择**文件**工作区之前运行它。)该页面显示的全局值。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-145">(Make sure the page is selected in the **Files** workspace before you run it.) The page displays the global value.</span></span> 

    ![[image]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a><span data-ttu-id="1c4f0-147">帮助器的设置值</span><span class="sxs-lookup"><span data-stu-id="1c4f0-147">Setting Values for Helpers</span></span>

<span data-ttu-id="1c4f0-148">为很好的用途 *\_AppStart.cshtml*文件是为帮助程序，在你的站点使用时，必须初始化设置值。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-148">A good use for the *\_AppStart.cshtml* file is to set values for helpers that you use in your site and that have to be initialized.</span></span> <span data-ttu-id="1c4f0-149">典型的示例包括的电子邮件设置`WebMail`帮助器和的私有和公共密钥`ReCaptcha`帮助器。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-149">Typical examples are email settings for the `WebMail` helper and the private and public keys for the `ReCaptcha` helper.</span></span> <span data-ttu-id="1c4f0-150">在类似这样的情况下，你可以设置一次中的值 *\_AppStart.cshtml*然后它们正在已经为设置所有页在你的网站。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-150">In cases like these, you can set the values once in the *\_AppStart.cshtml* and then they're already set for all the pages in your site.</span></span>

<span data-ttu-id="1c4f0-151">此过程演示如何设置`WebMail`设置全局。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-151">This procedure shows you how to set `WebMail` settings globally.</span></span> <span data-ttu-id="1c4f0-152">(有关使用`WebMail`帮助器，请参阅[添加到 ASP.NET Web Pages 站点的电子邮件](../getting-started/11-adding-email-to-your-web-site.md)。)</span><span class="sxs-lookup"><span data-stu-id="1c4f0-152">(For more information about using the `WebMail` helper, see [Adding Email to an ASP.NET Web Pages Site](../getting-started/11-adding-email-to-your-web-site.md).)</span></span>

1. <span data-ttu-id="1c4f0-153">将 ASP.NET Web 帮助程序库添加到你的网站中所述[安装 ASP.NET 网页站点中的帮助器](https://go.microsoft.com/fwlink/?LinkId=252372)，如果你尚未添加它。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-153">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="1c4f0-154">如果你还没有 *\_AppStart.cshtml*文件，在网站的根文件夹中创建名为的文件 *\_AppStart.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-154">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
3. <span data-ttu-id="1c4f0-155">添加以下`WebMail`设置应用于 *\_AppStart.cshtml*文件：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-155">Add the following `WebMail` settings to the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    <span data-ttu-id="1c4f0-156">修改以下电子邮件的代码中的相关的设置：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-156">Modify the following email related settings in the code:</span></span>

    - <span data-ttu-id="1c4f0-157">设置`your-SMTP-host`到可以访问 SMTP 服务器的名称。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-157">Set `your-SMTP-host` to the name of the SMTP server that you have access to.</span></span>
    - <span data-ttu-id="1c4f0-158">设置`your-user-name-here`到您的 SMTP 服务器帐户的用户名。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-158">Set `your-user-name-here` to the user name for your SMTP server account.</span></span>
    - <span data-ttu-id="1c4f0-159">设置`your-account-password`到您的 SMTP 服务器帐户的密码。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-159">Set `your-account-password` to the password for your SMTP server account.</span></span>
    - <span data-ttu-id="1c4f0-160">设置`your-email-address-here`为您自己的电子邮件地址。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-160">Set `your-email-address-here` to your own email address.</span></span> <span data-ttu-id="1c4f0-161">这是从发送消息的电子邮件地址。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-161">This is the email address that the message is sent from.</span></span> <span data-ttu-id="1c4f0-162">(某些电子邮件提供商不允许你指定一个不同`From`地址，并将使用你的用户名称作为`From`地址。)</span><span class="sxs-lookup"><span data-stu-id="1c4f0-162">(Some email providers don't let you specify a different `From` address and will use your user name as the `From` address.)</span></span>

    <span data-ttu-id="1c4f0-163">有关 SMTP 设置的详细信息，请参阅[配置电子邮件设置](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings)文章中[从 ASP.NET Web 页 (Razor) 站点发送电子邮件](https://go.microsoft.com/fwlink/?LinkID=202899)和[问题发送电子邮件](https://go.microsoft.com/fwlink/?LinkId=253001#email)中[ASP.NET 网页 (Razor) 故障排除指南 》](https://go.microsoft.com/fwlink/?LinkId=253001)。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-163">For more information about SMTP settings, see [Configuring Email Settings](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) in the article [Sending Email from an ASP.NET Web Pages (Razor) Site](https://go.microsoft.com/fwlink/?LinkID=202899) and [Issues with Sending Email](https://go.microsoft.com/fwlink/?LinkId=253001#email) in the [ASP.NET Web Pages (Razor) Troubleshooting Guide](https://go.microsoft.com/fwlink/?LinkId=253001).</span></span>
- <span data-ttu-id="1c4f0-164">保存 *\_AppStart.cshtml*文件并将其关闭。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-164">Save the *\_AppStart.cshtml* file and close it.</span></span>
- <span data-ttu-id="1c4f0-165">在网站的根文件夹中，创建名为的新页*TestEmail.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-165">In the root folder of a website, create new page named *TestEmail.cshtml*.</span></span>
- <span data-ttu-id="1c4f0-166">将现有内容替换为以下：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-166">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
- <span data-ttu-id="1c4f0-167">运行*TestEmail.cshtml*在浏览器中的页。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-167">Run the *TestEmail.cshtml* page in a browser.</span></span>
- <span data-ttu-id="1c4f0-168">填写字段来向自己发送一封电子邮件，然后单击**发送**。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-168">Fill in the fields to send yourself an email message and then click **Send**.</span></span>
- <span data-ttu-id="1c4f0-169">检查你的电子邮件，以确保你已收到消息。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-169">Check your email to make sure you've gotten the message.</span></span>

<span data-ttu-id="1c4f0-170">此示例的重要部分是，通常不更改的设置-喜欢的 SMTP 服务器和你的电子邮件凭据的名称-在中设置 *\_AppStart.cshtml*文件。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-170">The important part of this example is that the settings that you don't usually change — like the name of your SMTP server and your email credentials — are set in the *\_AppStart.cshtml* file.</span></span> <span data-ttu-id="1c4f0-171">这样，无需它们再次设置每个页面中，发送电子邮件。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-171">That way you don't need to set them again in each page where you send email.</span></span> <span data-ttu-id="1c4f0-172">（尽管如果出于某种原因，你需要更改这些设置，你可以设置它们单独在页中。）在页中，只能设置通常每次发生变化，如收件人和电子邮件消息的正文的值。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-172">(Although if for some reason you need to change those settings, you can set them individually in a page.) In the page, you only set the values that typically change each time, like the recipient and the body of the email message.</span></span>

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a><span data-ttu-id="1c4f0-173">运行代码之前和之后文件夹中的文件</span><span class="sxs-lookup"><span data-stu-id="1c4f0-173">Running Code Before and After Files in a Folder</span></span>

<span data-ttu-id="1c4f0-174">就像可以使用 *\_AppStart.cshtml*站点中的页运行之前，请编写代码，你可以编写代码运行之前 （和之后） 运行的特定文件夹中的任意页。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-174">Just like you can use *\_AppStart.cshtml* to write code before pages in the site run, you can write code that runs before (and after) any page in a particular folder run.</span></span> <span data-ttu-id="1c4f0-175">这可设置的同一布局页面为一个文件夹中的所有页或检查在文件夹中运行一个页面之前登录用户之类的内容。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-175">This is useful for things like setting the same layout page for all the pages in a folder, or for checking that a user is logged in before running a page in the folder.</span></span>

<span data-ttu-id="1c4f0-176">对于页尤其文件夹，你可以创建代码在名为的文件中 *\_PageStart.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-176">For pages in particular folders, you can create code in a file named *\_PageStart.cshtml*.</span></span> <span data-ttu-id="1c4f0-177">下图显示了 *\_PageStart.cshtml*页上工作。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-177">The following diagram shows how the *\_PageStart.cshtml* page works.</span></span> <span data-ttu-id="1c4f0-178">当请求的页面，ASP.NET 首先检查 *\_AppStart.cshtml*页上和运行的。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-178">When a request comes in for a page, ASP.NET first checks for a *\_AppStart.cshtml* page and runs that.</span></span> <span data-ttu-id="1c4f0-179">然后 ASP.NET 将检查是否存在 *\_PageStart.cshtml*页，然后如果是这样，当运行。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-179">Then ASP.NET checks whether there's a *\_PageStart.cshtml* page, and if so, runs that.</span></span> <span data-ttu-id="1c4f0-180">然后，它将运行请求的页。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-180">It then runs the requested page.</span></span>

<span data-ttu-id="1c4f0-181">内部 *\_PageStart.cshtml*页上，您可以在你想请求的页后，可以通过包括运行的处理过程中指定位置`RunPage`方法。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-181">Inside the *\_PageStart.cshtml* page, you can specify where during processing you want the requested page to run by including a `RunPage` method.</span></span> <span data-ttu-id="1c4f0-182">这样就可以在请求的页运行之前运行代码，然后再次在它后面。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-182">This lets you run code before the requested page runs and then again after it.</span></span> <span data-ttu-id="1c4f0-183">如果不包含`RunPage`中的所有代码 *\_PageStart.cshtml*运行，然后请求的页自动运行。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-183">If you don't include `RunPage`, all the code in *\_PageStart.cshtml* runs, and then the requested page runs automatically.</span></span>

![[image]](18-customizing-site-wide-behavior/_static/image3.jpg)

<span data-ttu-id="1c4f0-185">ASP.NET 允许你创建的层次结构 *\_PageStart.cshtml*文件。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-185">ASP.NET lets you create a hierarchy of *\_PageStart.cshtml* files.</span></span> <span data-ttu-id="1c4f0-186">您可以放入 *\_PageStart.cshtml*文件根目录中的站点和任何子文件夹中。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-186">You can put a *\_PageStart.cshtml* file in the root of the site and in any subfolder.</span></span> <span data-ttu-id="1c4f0-187">请求页时，  *\_PageStart.cshtml*文件在最顶层级别 （距离最近的站点根目录中） 运行后, 跟 *\_PageStart.cshtml*下一步中的文件子文件夹，诸如此类之前请求到达包含请求的页的文件夹的子文件夹结构。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-187">When a page is requested, the *\_PageStart.cshtml* file at the top-most level (nearest to the site root) runs, followed by the *\_PageStart.cshtml* file in the next subfolder, and so on down the subfolder structure until the request reaches the folder that contains the requested page.</span></span> <span data-ttu-id="1c4f0-188">后所有适用 *\_PageStart.cshtml*文件已运行，请求的页将运行。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-188">After all the applicable *\_PageStart.cshtml* files have run, the requested page runs.</span></span>

<span data-ttu-id="1c4f0-189">例如，你可能具有的以下组合 *\_PageStart.cshtml*文件和*Default.cshtml*文件：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-189">For example, you might have the following combination of *\_PageStart.cshtml* files and *Default.cshtml* file:</span></span>

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

<span data-ttu-id="1c4f0-190">当你运行*/myfolder/default.cshtml*，你将看到以下：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-190">When you run */myfolder/default.cshtml*, you'll see the following:</span></span>

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a><span data-ttu-id="1c4f0-191">正在运行的初始化代码的文件夹中的所有页面</span><span class="sxs-lookup"><span data-stu-id="1c4f0-191">Running Initialization Code for All Pages in a Folder</span></span>

<span data-ttu-id="1c4f0-192">为很好的用途 *\_PageStart.cshtml*文件是初始化的单个文件夹中的所有文件相同的布局页。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-192">A good use for *\_PageStart.cshtml* files is to initialize the same layout page for all files in a single folder.</span></span>

1. <span data-ttu-id="1c4f0-193">在根文件夹中，创建一个名为的新文件夹*InitPages*。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-193">In the root folder, create a new folder named *InitPages*.</span></span>
2. <span data-ttu-id="1c4f0-194">在*InitPages*文件夹中你的网站，创建名为的文件 *\_PageStart.cshtml*和默认标记和代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-194">In the *InitPages* folder of your website, create a file named *\_PageStart.cshtml* and replace the default markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. <span data-ttu-id="1c4f0-195">在网站的根目录，创建名为的文件夹*共享*。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-195">In the root of the website, create a folder named *Shared*.</span></span>
4. <span data-ttu-id="1c4f0-196">在*共享*文件夹中，创建名为的文件 *\_Layout1.cshtml*和默认标记和代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-196">In the *Shared* folder, create a file named *\_Layout1.cshtml* and replace the default markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. <span data-ttu-id="1c4f0-197">在*InitPages*文件夹中，创建名为的文件*Content1.cshtml*和现有内容替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-197">In the *InitPages* folder, create a file named *Content1.cshtml* and replace the existing content with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. <span data-ttu-id="1c4f0-198">在*InitPages*文件夹中，创建名为的另一个文件*Content2.cshtml*和默认标记替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-198">In the *InitPages* folder, create another file named *Content2.cshtml* and replace the default markup with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. <span data-ttu-id="1c4f0-199">运行*Content1.cshtml*在浏览器。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-199">Run *Content1.cshtml* in a browser.</span></span> 

    ![[image]](18-customizing-site-wide-behavior/_static/image4.jpg)

    <span data-ttu-id="1c4f0-201">当*Content1.cshtml*页上运行，  *\_PageStart.cshtml*文件中设置`Layout`，并设置`PageData["MyBackground"]`为一种颜色。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-201">When the *Content1.cshtml* page runs, the *\_PageStart.cshtml* file sets `Layout` and also sets `PageData["MyBackground"]` to a color.</span></span> <span data-ttu-id="1c4f0-202">在*Content1.cshtml*，应用的布局和颜色。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-202">In *Content1.cshtml*, the layout and color are applied.</span></span>
8. <span data-ttu-id="1c4f0-203">显示*Content2.cshtml*在浏览器。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-203">Display *Content2.cshtml* in a browser.</span></span> 

    <span data-ttu-id="1c4f0-204">布局是相同的因为两个页面使用的相同的布局页和颜色在中初始化 *\_PageStart.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-204">The layout is the same, because both pages use the same layout page and color as initialized in *\_PageStart.cshtml*.</span></span>

## <a name="using-pagestartcshtml-to-handle-errors"></a><span data-ttu-id="1c4f0-205">使用\_PageStart.cshtml 来处理错误</span><span class="sxs-lookup"><span data-stu-id="1c4f0-205">Using \_PageStart.cshtml to Handle Errors</span></span>

<span data-ttu-id="1c4f0-206">另一个良好用于 *\_PageStart.cshtml*文件是创建处理的编程错误 （异常） 可能发生的任何一种方式*.cshtml*文件夹中的页。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-206">Another good use for the *\_PageStart.cshtml* file is to create a way to handle programming errors (exceptions) that might occur in any *.cshtml* page in a folder.</span></span> <span data-ttu-id="1c4f0-207">此示例演示了一种方法执行此操作。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-207">This example shows you one way to do this.</span></span>

1. <span data-ttu-id="1c4f0-208">在根文件夹中，创建名为的文件夹*InitCatch*。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-208">In the root folder, create a folder named *InitCatch*.</span></span>
2. <span data-ttu-id="1c4f0-209">在*InitCatch*文件夹中你的网站，创建名为的文件 *\_PageStart.cshtml*和现有标记和代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-209">In the *InitCatch* folder of your website, create a file named *\_PageStart.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    <span data-ttu-id="1c4f0-210">在此代码中，则尝试通过调用显式运行请求的页`RunPage`中的方法`try`块。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-210">In this code, you try running the requested page explicitly by calling the `RunPage` method inside a `try` block.</span></span> <span data-ttu-id="1c4f0-211">如果在请求中出现的任何编程错误页上内的代码`catch`块将运行。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-211">If any programming errors occur in the requested page, the code inside the `catch` block runs.</span></span> <span data-ttu-id="1c4f0-212">在这种情况下，代码将重定向到一个页面 (*Error.cshtml*)，并将传递作为 URL 的一部分中遇到错误的文件的名称。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-212">In this case, the code redirects to a page (*Error.cshtml*) and passes the name of the file that experienced the error as part of the URL.</span></span> <span data-ttu-id="1c4f0-213">（你将创建页很快。）</span><span class="sxs-lookup"><span data-stu-id="1c4f0-213">(You'll create the page shortly.)</span></span>
3. <span data-ttu-id="1c4f0-214">在*InitCatch*文件夹中你的网站，创建名为的文件*Exception.cshtml*和现有标记和代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-214">In the *InitCatch* folder of your website, create a file named *Exception.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    <span data-ttu-id="1c4f0-215">此示例的目的，你要在此页中执行的操作有意创建错误通过尝试打开不存在的数据库文件。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-215">For purposes of this example, what you're doing in this page is deliberately creating an error by trying to open a database file that doesn't exist.</span></span>
4. <span data-ttu-id="1c4f0-216">在根文件夹中，创建名为的文件*Error.cshtml*和现有标记和代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-216">In the root folder, create a file named *Error.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    <span data-ttu-id="1c4f0-217">在此页中，表达式`@Request["source"]`获取值超出 URL 并将其显示。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-217">In this page, the expression `@Request["source"]` gets the value out of the URL and displays it.</span></span>
5. <span data-ttu-id="1c4f0-218">在工具栏中，单击**保存**。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-218">In the toolbar, click **Save**.</span></span>
6. <span data-ttu-id="1c4f0-219">运行*Exception.cshtml*在浏览器。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-219">Run *Exception.cshtml* in a browser.</span></span> 

    ![[image]](18-customizing-site-wide-behavior/_static/image5.jpg)

    <span data-ttu-id="1c4f0-221">因为在发生错误*Exception.cshtml*、  *\_PageStart.cshtml*页面重定向到*Error.cshtml*文件，其中显示的消息。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-221">Because an error occurs in *Exception.cshtml*, the *\_PageStart.cshtml* page redirects to the *Error.cshtml* file, which displays the message.</span></span>

    <span data-ttu-id="1c4f0-222">有关异常的详细信息，请参阅[ASP.NET 网页编程使用 Razor 语法的简介](https://go.microsoft.com/fwlink/?LinkID=251587)。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-222">For more information about exceptions, see [Introduction to ASP.NET Web Pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkID=251587).</span></span>

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a><span data-ttu-id="1c4f0-223">使用\_PageStart.cshtml 限制文件夹访问</span><span class="sxs-lookup"><span data-stu-id="1c4f0-223">Using \_PageStart.cshtml to Restrict Folder Access</span></span>

<span data-ttu-id="1c4f0-224">你还可以使用 *\_PageStart.cshtml*文件来限制访问的文件夹中的所有文件。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-224">You can also use the *\_PageStart.cshtml* file to restrict access to all the files in a folder.</span></span>

1. <span data-ttu-id="1c4f0-225">在 WebMatrix 中，创建新的网站使用**站点从模板**选项。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-225">In WebMatrix, create a new website using the **Site From Template** option.</span></span>
2. <span data-ttu-id="1c4f0-226">从可用模板中，选择**入门站点**。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-226">From the available templates, select **Starter Site**.</span></span>
3. <span data-ttu-id="1c4f0-227">在根文件夹中，创建名为的文件夹*AuthenticatedContent*。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-227">In the root folder, create a folder named *AuthenticatedContent*.</span></span>
4. <span data-ttu-id="1c4f0-228">在*AuthenticatedContent*文件夹中，创建名为的文件 *\_PageStart.cshtml*和现有标记和代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-228">In the *AuthenticatedContent* folder, create a file named *\_PageStart.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    <span data-ttu-id="1c4f0-229">代码将启动通过阻止从正在缓存的文件夹中的所有文件。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-229">The code starts by preventing all files in the folder from being cached.</span></span> <span data-ttu-id="1c4f0-230">（这是必需的各种方案，例如不希望一个用户的缓存的页面，可用于下一个用户的公用计算机。）接下来的代码将确定是否用户已登录到该站点之前它们可以查看文件夹中的任何页面。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-230">(This is required for scenarios like public computers, where you don't want one user's cached pages to be available to the next user.) Next, the code determines whether the user has signed in to the site before they can view any of the pages in the folder.</span></span> <span data-ttu-id="1c4f0-231">如果用户未登录，代码将重定向到登录页。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-231">If the user is not signed in, the code redirects to the login page.</span></span> <span data-ttu-id="1c4f0-232">登录页可以将用户返回到如果包含一个名为的查询字符串值最初请求的页面`ReturnUrl`。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-232">The login page can return the user to the page that was originally requested if you include a query string value named `ReturnUrl`.</span></span>
5. <span data-ttu-id="1c4f0-233">创建新的页中*AuthenticatedContent*文件夹名为*Page.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-233">Create a new page in the *AuthenticatedContent* folder named *Page.cshtml*.</span></span>
6. <span data-ttu-id="1c4f0-234">默认标记替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="1c4f0-234">Replace the default markup with the following:</span></span>  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. <span data-ttu-id="1c4f0-235">运行*Page.cshtml*在浏览器。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-235">Run *Page.cshtml* in a browser.</span></span> <span data-ttu-id="1c4f0-236">代码将你重定向到登录页。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-236">The code redirects you to a login page.</span></span> <span data-ttu-id="1c4f0-237">你必须注册之前登录。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-237">You must register before logging in.</span></span> <span data-ttu-id="1c4f0-238">已注册和登录后，你可以导航到的页并查看其内容。</span><span class="sxs-lookup"><span data-stu-id="1c4f0-238">After you've registered and logged in, you can navigate to the page and view its contents.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="1c4f0-239">其他资源</span><span class="sxs-lookup"><span data-stu-id="1c4f0-239">Additional Resources</span></span>

[<span data-ttu-id="1c4f0-240">编程使用 Razor 语法的 ASP.NET Web Pages 简介</span><span class="sxs-lookup"><span data-stu-id="1c4f0-240">Introduction to ASP.NET Web Pages Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=251587)
