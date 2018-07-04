---
title: 在 ASP.NET Core中的常规数据保护法规 (GDPR) 支持
author: rick-anderson
description: 了解如何访问 ASP.NET Core web 应用中的 GDPR 扩展点。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/29/2018
uid: security/gdpr
ms.openlocfilehash: c986eeca572eecb43e76d56dbc5cb872a9dff6b2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277633"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="c6c2d-103">在 ASP.NET Core 欧洲常规数据保护法规 (GDPR) 支持</span><span class="sxs-lookup"><span data-stu-id="c6c2d-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="c6c2d-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c6c2d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c6c2d-105">ASP.NET Core 提供 Api 和模板，以帮助满足一些[欧洲常规数据保护法规 (GDPR)](https://www.eugdpr.org/)要求：</span><span class="sxs-lookup"><span data-stu-id="c6c2d-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="c6c2d-106">项目模板包含扩展点以及可以将替换为您的隐私和 cookie 的使用策略的存根的标记。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-106">The project templates include extension points and stubbed markup you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="c6c2d-107">用于存储的个人信息中，cookie 同意功能允许你以寻求 （和跟踪） 同意的情况下从你的用户。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="c6c2d-108">如果用户不与数据收集同意和应用程序设置[CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded)到`true`，不重要 cookie 将不会发送到浏览器。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-108">If a user has not consented to data collection and the app is set with [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) to `true`, non-essential cookies will not be sent to the browser.</span></span>
* <span data-ttu-id="c6c2d-109">可以将 cookie 标记为重要。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="c6c2d-110">基本 cookie 发送到浏览器中，即使用户不同意，并且已禁用跟踪。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-110">Essential cookies are sent to the browser even when the user has not consented and tracking is disabled.</span></span>
* <span data-ttu-id="c6c2d-111">[TempData 和会话 cookie](#tempdata)禁用跟踪时不起作用。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-111">[TempData and Session cookies](#tempdata) are not functional when tracking is disabled.</span></span>
* <span data-ttu-id="c6c2d-112">[标识管理](#pd)页提供的链接下载和删除用户数据。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="c6c2d-113">[示例应用程序](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample)允许你测试的大多数 GDPR 扩展点和 Api 添加到 ASP.NET Core 2.1 模板。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-113">The [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="c6c2d-114">请参阅[自述文件](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample)测试说明文件。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-114">See the [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="c6c2d-115">[查看或下载示例代码](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="c6c2d-115">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="c6c2d-116">模板中的 ASP.NET Core GDPR 支持生成的代码</span><span class="sxs-lookup"><span data-stu-id="c6c2d-116">ASP.NET Core GDPR support in template generated code</span></span>

<span data-ttu-id="c6c2d-117">Razor 页和 MVC 项目模板创建的项目包括以下 GDPR 支持：</span><span class="sxs-lookup"><span data-stu-id="c6c2d-117">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="c6c2d-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)和[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy)中设置`Startup`。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in `Startup`.</span></span>
* <span data-ttu-id="c6c2d-119">*_CookieConsentPartial.cshtml* [分部视图](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-119">The *_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span>
* <span data-ttu-id="c6c2d-120">*Pages/Privacy.cshtml*或*Home/Privacy.cshtml*视图提供页详细介绍站点的隐私策略。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-120">The *Pages/Privacy.cshtml* or *Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="c6c2d-121">*_CookieConsentPartial.cshtml*文件生成隐私页的链接。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-121">The *_CookieConsentPartial.cshtml* file generates a link to the privacy page.</span></span>
* <span data-ttu-id="c6c2d-122">对于使用单个用户帐户创建的应用程序，管理页提供了用于下载和删除链接[个人用户数据](#pd)。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-122">For applications created with individual user accounts, the manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="c6c2d-123">CookiePolicyOptions 和 UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="c6c2d-123">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="c6c2d-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)中初始化`Startup`类`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="c6c2d-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in the `Startup` class `ConfigureServices` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="c6c2d-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy)中称为`Startup`类`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="c6c2d-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in the `Startup` class `Configure` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="c6c2d-126">_CookieConsentPartial.cshtml 分部视图</span><span class="sxs-lookup"><span data-stu-id="c6c2d-126">_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="c6c2d-127">*_CookieConsentPartial.cshtml*分部视图：</span><span class="sxs-lookup"><span data-stu-id="c6c2d-127">The *_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="c6c2d-128">此部分中：</span><span class="sxs-lookup"><span data-stu-id="c6c2d-128">This partial:</span></span>

* <span data-ttu-id="c6c2d-129">获取跟踪的用户的状态。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-129">Gets the state of tracking for the user.</span></span> <span data-ttu-id="c6c2d-130">如果应用程序配置为需要之前可以跟踪 cookie，用户必须同意的同意。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-130">If the application is configured to require consent the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="c6c2d-131">如果需要同意的情况下，在顶部导航栏中创建固定 cookie 同意 chrome *Pages/Shared/_Layout.cshtml*文件。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-131">If consent is required, the cookie consent chrome is fixed on top of the navigation bar created in the *Pages/Shared/_Layout.cshtml* file.</span></span>
* <span data-ttu-id="c6c2d-132">提供一个 HTML`<p>`元素汇总隐私和 cookie 使用策略。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-132">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="c6c2d-133">提供的链接*Pages/Privacy.cshtml*其中详细介绍站点的隐私策略。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-133">Provides a link to *Pages/Privacy.cshtml* where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="c6c2d-134">基本 cookie</span><span class="sxs-lookup"><span data-stu-id="c6c2d-134">Essential cookies</span></span>

<span data-ttu-id="c6c2d-135">如果不提供同意的情况下，仅标记为重要的 cookie 被发送到浏览器。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-135">If consent has not been given, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="c6c2d-136">下面的代码使 cookie 重要：</span><span class="sxs-lookup"><span data-stu-id="c6c2d-136">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a><span data-ttu-id="c6c2d-137">Tempdata 提供程序和会话状态的 cookie 不重要</span><span class="sxs-lookup"><span data-stu-id="c6c2d-137">Tempdata provider and session state cookies are not essential</span></span>

<span data-ttu-id="c6c2d-138">[Tempdata 提供程序](xref:fundamentals/app-state#tempdata)cookie 不是必需的。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-138">The [Tempdata provider](xref:fundamentals/app-state#tempdata) cookie is not essential.</span></span> <span data-ttu-id="c6c2d-139">如果禁用了跟踪，则 Tempdata 提供程序不起作用。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-139">If tracking is disabled, the Tempdata provider is not functional.</span></span> <span data-ttu-id="c6c2d-140">若要禁用跟踪时，请启用 Tempdata 提供程序，将 TempData cookie 标记为关键`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c6c2d-140">To enable the Tempdata provider when tracking is disabled, mark the TempData cookie as essential in `ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="c6c2d-141">[会话状态](xref:fundamentals/app-state)cookie 不重要。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-141">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="c6c2d-142">禁用跟踪时，会话状态不起作用。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-142">Session state is not functional when tracking is disabled.</span></span>

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="c6c2d-143">个人数据</span><span class="sxs-lookup"><span data-stu-id="c6c2d-143">Personal data</span></span>

<span data-ttu-id="c6c2d-144">ASP.NET Core应用程序使用单个用户帐户创建包含代码，从而下载和删除个人数据。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-144">ASP.NET Core applications created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="c6c2d-145">选择的用户名称，然后选择**个人数据**:</span><span class="sxs-lookup"><span data-stu-id="c6c2d-145">Select the user name and then select **Personal data**:</span></span>

![管理个人数据页](gdpr/_static/pd.png)

<span data-ttu-id="c6c2d-147">注意：</span><span class="sxs-lookup"><span data-stu-id="c6c2d-147">Notes:</span></span>

* <span data-ttu-id="c6c2d-148">若要生成`Account/Manage`代码，请参阅[基架标识](xref:security/authentication/scaffold-identity)。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-148">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="c6c2d-149">删除并下载仅影响的默认标识数据。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-149">Delete and download only impact the default identity data.</span></span> <span data-ttu-id="c6c2d-150">应用程序创建自定义用户数据必须进行扩展，以删除/下载自定义用户数据。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-150">Apps the create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="c6c2d-151">GitHub 问题[如何添加/删除到标识的自定义用户数据](https://github.com/aspnet/Docs/issues/6226)跟踪上创建自定义/删除/下载自定义用户数据的建议的文章。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-151">GitHub issue [How to add/delete custom user data to Identity](https://github.com/aspnet/Docs/issues/6226) tracks a proposed article on creating custom/deleting/downloading custom user data.</span></span> <span data-ttu-id="c6c2d-152">如果你想要查看该主题按优先级排列，将向上反应拇指保留为问题。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-152">If you'd like to see that topic prioritized, leave a thumbs up reaction in the issue.</span></span>
* <span data-ttu-id="c6c2d-153">保存用户标识数据库表中存储的令牌`AspNetUserTokens`时由于的级联删除行为通过删除该用户会被删除[外键](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152)。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-153">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="c6c2d-154">静态加密</span><span class="sxs-lookup"><span data-stu-id="c6c2d-154">Encryption at rest</span></span>

<span data-ttu-id="c6c2d-155">某些数据库和存储机制允许进行静态加密。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-155">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="c6c2d-156">静态加密：</span><span class="sxs-lookup"><span data-stu-id="c6c2d-156">Encryption at rest:</span></span>

* <span data-ttu-id="c6c2d-157">自动加密存储的数据。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-157">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="c6c2d-158">加密而无需配置、 编程中或访问数据的软件的其他工作。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-158">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="c6c2d-159">是最简单且最安全选项。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-159">Is the easiest and safest option.</span></span>
* <span data-ttu-id="c6c2d-160">允许管理密钥和加密的数据库。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-160">Lets the database manage keys and encryption.</span></span>

<span data-ttu-id="c6c2d-161">例如：</span><span class="sxs-lookup"><span data-stu-id="c6c2d-161">For example:</span></span>

* <span data-ttu-id="c6c2d-162">Microsoft SQL 和 Azure SQL 提供[透明数据加密](/sql/relational-databases/security/encryption/transparent-data-encryption)(TDE)。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-162">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="c6c2d-163">SQL Azure 默认情况下加密数据库</span><span class="sxs-lookup"><span data-stu-id="c6c2d-163">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="c6c2d-164">[默认情况下加密 azure Blob、 文件、 表和队列存储](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/)。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-164">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="c6c2d-165">对于不提供内置加密对静止的数据库，你可能能够使用磁盘加密提供相同的保护。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-165">For databases that don't provide built-in encryption at rest you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="c6c2d-166">例如：</span><span class="sxs-lookup"><span data-stu-id="c6c2d-166">For example:</span></span>

* [<span data-ttu-id="c6c2d-167">Windows Server 的 BitLocker</span><span class="sxs-lookup"><span data-stu-id="c6c2d-167">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="c6c2d-168">Linux:</span><span class="sxs-lookup"><span data-stu-id="c6c2d-168">Linux:</span></span>
  * [<span data-ttu-id="c6c2d-169">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="c6c2d-169">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="c6c2d-170">[EncFS](https://github.com/vgough/encfs)。</span><span class="sxs-lookup"><span data-stu-id="c6c2d-170">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c6c2d-171">其他资源</span><span class="sxs-lookup"><span data-stu-id="c6c2d-171">Additional Resources</span></span>

* [<span data-ttu-id="c6c2d-172">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="c6c2d-172">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
