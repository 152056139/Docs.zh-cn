---
title: "使用 ASP.NET Core 的 IIS 模块"
author: guardrex
description: "参考文档，其中描述 ASP.NET Core 应用程序的活动和非活动 IIS 模块。"
keywords: "ASP.NET 核心，iis、 模块、 反向代理"
ms.author: riande
manager: wpickett
ms.date: 03/08/2017
ms.topic: article
ms.assetid: 492b3a7e-04c5-461b-b96a-38ecee5c64bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/iis-modules
ms.openlocfilehash: 4d9d3d863e62373716ecd81b9e1880145a127e9b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/11/2017
---
# <a name="using-iis-modules-with-aspnet-core"></a><span data-ttu-id="ef76b-104">使用 ASP.NET Core 的 IIS 模块</span><span class="sxs-lookup"><span data-stu-id="ef76b-104">Using IIS Modules with ASP.NET Core</span></span>

<span data-ttu-id="ef76b-105">通过[Luke Latham](https://github.com/GuardRex)</span><span class="sxs-lookup"><span data-stu-id="ef76b-105">By [Luke Latham](https://github.com/GuardRex)</span></span>

<span data-ttu-id="ef76b-106">反向代理配置中，由 IIS 承载 ASP.NET Core 应用程序。</span><span class="sxs-lookup"><span data-stu-id="ef76b-106">ASP.NET Core applications are hosted by IIS in a reverse-proxy configuration.</span></span> <span data-ttu-id="ef76b-107">一些本机 IIS 模块和所有托管的 IIS 模块不可用来处理 ASP.NET Core 应用的请求。</span><span class="sxs-lookup"><span data-stu-id="ef76b-107">Some of the native IIS modules and all of the IIS managed modules are not available to process requests for ASP.NET Core apps.</span></span> <span data-ttu-id="ef76b-108">在许多情况下，ASP.NET Core 提供 IIS 本机和托管模块的功能的替代方法。</span><span class="sxs-lookup"><span data-stu-id="ef76b-108">In many cases, ASP.NET Core offers an alternative to the features of IIS native and managed modules.</span></span>

## <a name="native-modules"></a><span data-ttu-id="ef76b-109">本机模块</span><span class="sxs-lookup"><span data-stu-id="ef76b-109">Native Modules</span></span>
<span data-ttu-id="ef76b-110">模块</span><span class="sxs-lookup"><span data-stu-id="ef76b-110">Module</span></span> | <span data-ttu-id="ef76b-111">.NET 核心活动</span><span class="sxs-lookup"><span data-stu-id="ef76b-111">.NET Core Active</span></span> | <span data-ttu-id="ef76b-112">ASP.NET 核心选项</span><span class="sxs-lookup"><span data-stu-id="ef76b-112">ASP.NET Core Option</span></span>
--- | :---: | ---
<span data-ttu-id="ef76b-113">**匿名身份验证**</span><span class="sxs-lookup"><span data-stu-id="ef76b-113">**Anonymous Authentication**</span></span><br>`AnonymousAuthenticationModule` | <span data-ttu-id="ef76b-114">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-114">Yes</span></span> | 
<span data-ttu-id="ef76b-115">**基本身份验证**</span><span class="sxs-lookup"><span data-stu-id="ef76b-115">**Basic Authentication**</span></span><br>`BasicAuthenticationModule` | <span data-ttu-id="ef76b-116">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-116">Yes</span></span> | 
<span data-ttu-id="ef76b-117">**客户端证书映射身份验证**</span><span class="sxs-lookup"><span data-stu-id="ef76b-117">**Client Certification Mapping Authentication**</span></span><br>`CertificateMappingAuthenticationModule` | <span data-ttu-id="ef76b-118">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-118">Yes</span></span> | 
<span data-ttu-id="ef76b-119">**CGI**</span><span class="sxs-lookup"><span data-stu-id="ef76b-119">**CGI**</span></span><br>`CgiModule` | <span data-ttu-id="ef76b-120">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-120">No</span></span> | 
<span data-ttu-id="ef76b-121">**配置验证**</span><span class="sxs-lookup"><span data-stu-id="ef76b-121">**Configuration Validation**</span></span><br>`ConfigurationValidationModule` | <span data-ttu-id="ef76b-122">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-122">Yes</span></span> | 
<span data-ttu-id="ef76b-123">**HTTP 错误**</span><span class="sxs-lookup"><span data-stu-id="ef76b-123">**HTTP Errors**</span></span><br>`CustomErrorModule` | <span data-ttu-id="ef76b-124">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-124">No</span></span> | [<span data-ttu-id="ef76b-125">状态代码页中间件</span><span class="sxs-lookup"><span data-stu-id="ef76b-125">Status Code Pages Middleware</span></span>](xref:fundamentals/error-handling#configuring-status-code-pages)
<span data-ttu-id="ef76b-126">**自定义日志记录**</span><span class="sxs-lookup"><span data-stu-id="ef76b-126">**Custom Logging**</span></span><br>`CustomLoggingModule` | <span data-ttu-id="ef76b-127">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-127">Yes</span></span> | 
<span data-ttu-id="ef76b-128">**默认文档**</span><span class="sxs-lookup"><span data-stu-id="ef76b-128">**Default Document**</span></span><br>`DefaultDocumentModule` | <span data-ttu-id="ef76b-129">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-129">No</span></span> | [<span data-ttu-id="ef76b-130">默认文件中间件</span><span class="sxs-lookup"><span data-stu-id="ef76b-130">Default Files Middleware</span></span>](xref:fundamentals/static-files#serving-a-default-document)
<span data-ttu-id="ef76b-131">**摘要式身份验证**</span><span class="sxs-lookup"><span data-stu-id="ef76b-131">**Digest Authentication**</span></span><br>`DigestAuthenticationModule` | <span data-ttu-id="ef76b-132">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-132">Yes</span></span> | 
<span data-ttu-id="ef76b-133">**目录浏览**</span><span class="sxs-lookup"><span data-stu-id="ef76b-133">**Directory Browsing**</span></span><br>`DirectoryListingModule` | <span data-ttu-id="ef76b-134">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-134">No</span></span> | [<span data-ttu-id="ef76b-135">目录浏览中间件</span><span class="sxs-lookup"><span data-stu-id="ef76b-135">Directory Browsing Middleware</span></span>](xref:fundamentals/static-files#enabling-directory-browsing)
<span data-ttu-id="ef76b-136">**动态压缩**</span><span class="sxs-lookup"><span data-stu-id="ef76b-136">**Dynamic Compression**</span></span><br>`DynamicCompressionModule` | <span data-ttu-id="ef76b-137">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-137">Yes</span></span> | [<span data-ttu-id="ef76b-138">响应压缩中间件</span><span class="sxs-lookup"><span data-stu-id="ef76b-138">Response Compression Middleware</span></span>](xref:performance/response-compression)
<span data-ttu-id="ef76b-139">**跟踪**</span><span class="sxs-lookup"><span data-stu-id="ef76b-139">**Tracing**</span></span><br>`FailedRequestsTracingModule` | <span data-ttu-id="ef76b-140">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-140">Yes</span></span> | [<span data-ttu-id="ef76b-141">ASP.NET 核心日志记录</span><span class="sxs-lookup"><span data-stu-id="ef76b-141">ASP.NET Core Logging</span></span>](xref:fundamentals/logging#the-tracesource-provider)
<span data-ttu-id="ef76b-142">**文件缓存**</span><span class="sxs-lookup"><span data-stu-id="ef76b-142">**File Caching**</span></span><br>`FileCacheModule` | <span data-ttu-id="ef76b-143">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-143">No</span></span> | [<span data-ttu-id="ef76b-144">响应缓存中间件</span><span class="sxs-lookup"><span data-stu-id="ef76b-144">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
<span data-ttu-id="ef76b-145">**HTTP 缓存功能**</span><span class="sxs-lookup"><span data-stu-id="ef76b-145">**HTTP Caching**</span></span><br>`HttpCacheModule` | <span data-ttu-id="ef76b-146">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-146">No</span></span> | [<span data-ttu-id="ef76b-147">响应缓存中间件</span><span class="sxs-lookup"><span data-stu-id="ef76b-147">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
<span data-ttu-id="ef76b-148">**HTTP 日志记录**</span><span class="sxs-lookup"><span data-stu-id="ef76b-148">**HTTP Logging**</span></span><br>`HttpLoggingModule` | <span data-ttu-id="ef76b-149">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-149">Yes</span></span> | [<span data-ttu-id="ef76b-150">ASP.NET 核心日志记录</span><span class="sxs-lookup"><span data-stu-id="ef76b-150">ASP.NET Core Logging</span></span>](xref:fundamentals/logging)<br><span data-ttu-id="ef76b-151">实现： [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging)， [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging)， [NLog](https://github.com/NLog/NLog.Extensions.Logging)， [Serilog](https://github.com/serilog/serilog-framework-logging)</span><span class="sxs-lookup"><span data-stu-id="ef76b-151">Implementations: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-framework-logging)</span></span>
<span data-ttu-id="ef76b-152">**HTTP 重定向**</span><span class="sxs-lookup"><span data-stu-id="ef76b-152">**HTTP Redirection**</span></span><br>`HttpRedirectionModule` | <span data-ttu-id="ef76b-153">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-153">Yes</span></span> | [<span data-ttu-id="ef76b-154">URL 重写中间件</span><span class="sxs-lookup"><span data-stu-id="ef76b-154">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)
<span data-ttu-id="ef76b-155">**IIS 客户端证书映射身份验证**</span><span class="sxs-lookup"><span data-stu-id="ef76b-155">**IIS Client Certificate Mapping Authentication**</span></span><br>`IISCertificateMappingAuthenticationModule` | <span data-ttu-id="ef76b-156">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-156">Yes</span></span> | 
<span data-ttu-id="ef76b-157">**IP 和域限制**</span><span class="sxs-lookup"><span data-stu-id="ef76b-157">**IP and Domain Restrictions**</span></span><br>`IpRestrictionModule` | <span data-ttu-id="ef76b-158">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-158">Yes</span></span> | 
<span data-ttu-id="ef76b-159">**ISAPI 筛选器**</span><span class="sxs-lookup"><span data-stu-id="ef76b-159">**ISAPI Filters**</span></span><br>`IsapiFilterModule` | <span data-ttu-id="ef76b-160">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-160">Yes</span></span> | [<span data-ttu-id="ef76b-161">中间件</span><span class="sxs-lookup"><span data-stu-id="ef76b-161">Middleware</span></span>](xref:fundamentals/middleware)
<span data-ttu-id="ef76b-162">**ISAPI**</span><span class="sxs-lookup"><span data-stu-id="ef76b-162">**ISAPI**</span></span><br>`IsapiModule` | <span data-ttu-id="ef76b-163">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-163">Yes</span></span> | [<span data-ttu-id="ef76b-164">中间件</span><span class="sxs-lookup"><span data-stu-id="ef76b-164">Middleware</span></span>](xref:fundamentals/middleware)
<span data-ttu-id="ef76b-165">**协议支持**</span><span class="sxs-lookup"><span data-stu-id="ef76b-165">**Protocol Support**</span></span><br>`ProtocolSupportModule` | <span data-ttu-id="ef76b-166">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-166">Yes</span></span> | 
<span data-ttu-id="ef76b-167">**请求筛选**</span><span class="sxs-lookup"><span data-stu-id="ef76b-167">**Request Filtering**</span></span><br>`RequestFilteringModule` | <span data-ttu-id="ef76b-168">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-168">Yes</span></span> | [<span data-ttu-id="ef76b-169">URL 重写中间件`IRule`</span><span class="sxs-lookup"><span data-stu-id="ef76b-169">URL Rewriting Middleware `IRule`</span></span>](xref:fundamentals/url-rewriting#irule-based-rule)
<span data-ttu-id="ef76b-170">**请求监视器**</span><span class="sxs-lookup"><span data-stu-id="ef76b-170">**Request Monitor**</span></span><br>`RequestMonitorModule` | <span data-ttu-id="ef76b-171">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-171">Yes</span></span> | 
<span data-ttu-id="ef76b-172">**URL 重写**</span><span class="sxs-lookup"><span data-stu-id="ef76b-172">**URL Rewriting**</span></span><br>`RewriteModule` | <span data-ttu-id="ef76b-173">Yes†</span><span class="sxs-lookup"><span data-stu-id="ef76b-173">Yes†</span></span> | [<span data-ttu-id="ef76b-174">URL 重写中间件</span><span class="sxs-lookup"><span data-stu-id="ef76b-174">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)
<span data-ttu-id="ef76b-175">**服务器端包括**</span><span class="sxs-lookup"><span data-stu-id="ef76b-175">**Server Side Includes**</span></span><br>`ServerSideIncludeModule` | <span data-ttu-id="ef76b-176">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-176">No</span></span> | 
<span data-ttu-id="ef76b-177">**静态压缩**</span><span class="sxs-lookup"><span data-stu-id="ef76b-177">**Static Compression**</span></span><br>`StaticCompressionModule` | <span data-ttu-id="ef76b-178">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-178">No</span></span> | [<span data-ttu-id="ef76b-179">响应压缩中间件</span><span class="sxs-lookup"><span data-stu-id="ef76b-179">Response Compression Middleware</span></span>](xref:performance/response-compression)
<span data-ttu-id="ef76b-180">**静态内容**</span><span class="sxs-lookup"><span data-stu-id="ef76b-180">**Static Content**</span></span><br>`StaticFileModule` | <span data-ttu-id="ef76b-181">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-181">No</span></span> | [<span data-ttu-id="ef76b-182">静态文件中间件</span><span class="sxs-lookup"><span data-stu-id="ef76b-182">Static File Middleware</span></span>](xref:fundamentals/static-files)
<span data-ttu-id="ef76b-183">**令牌缓存**</span><span class="sxs-lookup"><span data-stu-id="ef76b-183">**Token Caching**</span></span><br>`TokenCacheModule` | <span data-ttu-id="ef76b-184">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-184">Yes</span></span> | 
<span data-ttu-id="ef76b-185">**URI 缓存**</span><span class="sxs-lookup"><span data-stu-id="ef76b-185">**URI Caching**</span></span><br>`UriCacheModule` | <span data-ttu-id="ef76b-186">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-186">Yes</span></span> | 
<span data-ttu-id="ef76b-187">**URL 授权**</span><span class="sxs-lookup"><span data-stu-id="ef76b-187">**URL Authorization**</span></span><br>`UrlAuthorizationModule` | <span data-ttu-id="ef76b-188">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-188">Yes</span></span> | [<span data-ttu-id="ef76b-189">ASP.NET 核心标识</span><span class="sxs-lookup"><span data-stu-id="ef76b-189">ASP.NET Core Identity</span></span>](xref:security/authentication/identity)
<span data-ttu-id="ef76b-190">**Windows 身份验证**</span><span class="sxs-lookup"><span data-stu-id="ef76b-190">**Windows Authentication**</span></span><br>`WindowsAuthenticationModule` | <span data-ttu-id="ef76b-191">是</span><span class="sxs-lookup"><span data-stu-id="ef76b-191">Yes</span></span> | 

<span data-ttu-id="ef76b-192">†The URL 重写模块`isFile`和`isDirectory`不能使用 ASP.NET Core 应用程序中的更改由于[目录结构](xref:hosting/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="ef76b-192">†The URL Rewrite Module's `isFile` and `isDirectory` do not work with ASP.NET Core applications due to the changes in [directory structure](xref:hosting/directory-structure).</span></span>

## <a name="managed-modules"></a><span data-ttu-id="ef76b-193">托管的模块</span><span class="sxs-lookup"><span data-stu-id="ef76b-193">Managed Modules</span></span>
<span data-ttu-id="ef76b-194">模块</span><span class="sxs-lookup"><span data-stu-id="ef76b-194">Module</span></span> | <span data-ttu-id="ef76b-195">.NET 核心活动</span><span class="sxs-lookup"><span data-stu-id="ef76b-195">.NET Core Active</span></span> | <span data-ttu-id="ef76b-196">ASP.NET 核心选项</span><span class="sxs-lookup"><span data-stu-id="ef76b-196">ASP.NET Core Option</span></span>
--- | :---: | ---
<span data-ttu-id="ef76b-197">AnonymousIdentification</span><span class="sxs-lookup"><span data-stu-id="ef76b-197">AnonymousIdentification</span></span> | <span data-ttu-id="ef76b-198">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-198">No</span></span> | 
<span data-ttu-id="ef76b-199">DefaultAuthentication</span><span class="sxs-lookup"><span data-stu-id="ef76b-199">DefaultAuthentication</span></span> | <span data-ttu-id="ef76b-200">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-200">No</span></span> | 
<span data-ttu-id="ef76b-201">FileAuthorization</span><span class="sxs-lookup"><span data-stu-id="ef76b-201">FileAuthorization</span></span> | <span data-ttu-id="ef76b-202">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-202">No</span></span> | 
<span data-ttu-id="ef76b-203">FormsAuthentication</span><span class="sxs-lookup"><span data-stu-id="ef76b-203">FormsAuthentication</span></span> | <span data-ttu-id="ef76b-204">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-204">No</span></span> | [<span data-ttu-id="ef76b-205">Cookie 身份验证中间件</span><span class="sxs-lookup"><span data-stu-id="ef76b-205">Cookie Authentication Middleware</span></span>](xref:security/authentication/cookie)
<span data-ttu-id="ef76b-206">OutputCache</span><span class="sxs-lookup"><span data-stu-id="ef76b-206">OutputCache</span></span> | <span data-ttu-id="ef76b-207">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-207">No</span></span> | [<span data-ttu-id="ef76b-208">响应缓存中间件</span><span class="sxs-lookup"><span data-stu-id="ef76b-208">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
<span data-ttu-id="ef76b-209">配置文件</span><span class="sxs-lookup"><span data-stu-id="ef76b-209">Profile</span></span> | <span data-ttu-id="ef76b-210">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-210">No</span></span> | 
<span data-ttu-id="ef76b-211">RoleManager</span><span class="sxs-lookup"><span data-stu-id="ef76b-211">RoleManager</span></span> | <span data-ttu-id="ef76b-212">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-212">No</span></span> | 
<span data-ttu-id="ef76b-213">ScriptModule 4.0</span><span class="sxs-lookup"><span data-stu-id="ef76b-213">ScriptModule-4.0</span></span> | <span data-ttu-id="ef76b-214">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-214">No</span></span> | 
<span data-ttu-id="ef76b-215">会话</span><span class="sxs-lookup"><span data-stu-id="ef76b-215">Session</span></span> | <span data-ttu-id="ef76b-216">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-216">No</span></span> | [<span data-ttu-id="ef76b-217">会话中间件</span><span class="sxs-lookup"><span data-stu-id="ef76b-217">Session Middleware</span></span>](xref:fundamentals/app-state)
<span data-ttu-id="ef76b-218">Url 授权</span><span class="sxs-lookup"><span data-stu-id="ef76b-218">UrlAuthorization</span></span> | <span data-ttu-id="ef76b-219">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-219">No</span></span> | 
<span data-ttu-id="ef76b-220">UrlMappingsModule</span><span class="sxs-lookup"><span data-stu-id="ef76b-220">UrlMappingsModule</span></span> | <span data-ttu-id="ef76b-221">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-221">No</span></span> | [<span data-ttu-id="ef76b-222">URL 重写中间件</span><span class="sxs-lookup"><span data-stu-id="ef76b-222">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)
<span data-ttu-id="ef76b-223">UrlRoutingModule 4.0</span><span class="sxs-lookup"><span data-stu-id="ef76b-223">UrlRoutingModule-4.0</span></span> | <span data-ttu-id="ef76b-224">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-224">No</span></span> | [<span data-ttu-id="ef76b-225">ASP.NET 核心标识</span><span class="sxs-lookup"><span data-stu-id="ef76b-225">ASP.NET Core  Identity</span></span>](xref:security/authentication/identity)
<span data-ttu-id="ef76b-226">WindowsAuthentication</span><span class="sxs-lookup"><span data-stu-id="ef76b-226">WindowsAuthentication</span></span> | <span data-ttu-id="ef76b-227">No</span><span class="sxs-lookup"><span data-stu-id="ef76b-227">No</span></span> | 

## <a name="iis-manager-application-changes"></a><span data-ttu-id="ef76b-228">IIS 管理器应用程序更改</span><span class="sxs-lookup"><span data-stu-id="ef76b-228">IIS Manager application changes</span></span>
<span data-ttu-id="ef76b-229">当你使用 IIS 管理器配置设置时，将直接更改*web.config*应用程序文件。</span><span class="sxs-lookup"><span data-stu-id="ef76b-229">When you use IIS Manager to configure settings, you're directly changing the *web.config* file of the app.</span></span> <span data-ttu-id="ef76b-230">如果你部署您的应用程序，包括*web.config*，使用 IIS 管理器进行的任何更改将被覆盖的部署*web.config 文件*。</span><span class="sxs-lookup"><span data-stu-id="ef76b-230">If you deploy your app and include *web.config*, any changes you made with IIS Manger will be overwritten by the deployed *web.config file*.</span></span> <span data-ttu-id="ef76b-231">因此如果你更改了服务器的*web.config*文件中，复制已更新*web.config*立即将本地项目的文件。</span><span class="sxs-lookup"><span data-stu-id="ef76b-231">Therefore if you make changes to the server's *web.config* file, copy the updated *web.config* file to your local project immediately.</span></span>

## <a name="disabling-iis-modules"></a><span data-ttu-id="ef76b-232">禁用 IIS 模块</span><span class="sxs-lookup"><span data-stu-id="ef76b-232">Disabling IIS modules</span></span>
<span data-ttu-id="ef76b-233">如果你有一个你想要禁用为应用程序的服务器级别配置的 IIS 模块，可以进行此操作而添加到你*web.config*文件。</span><span class="sxs-lookup"><span data-stu-id="ef76b-233">If you have an IIS module configured at the server level that you would like to disable for an application, you can do so with an addition to your *web.config* file.</span></span> <span data-ttu-id="ef76b-234">将模块保留原位和停用它 （如果可用） 使用配置设置，或者从应用程序中删除模块。</span><span class="sxs-lookup"><span data-stu-id="ef76b-234">Either leave the module in place and deactivate it using a configuration setting (if available) or remove the module from the app.</span></span>

### <a name="module-deactivation"></a><span data-ttu-id="ef76b-235">模块停用</span><span class="sxs-lookup"><span data-stu-id="ef76b-235">Module deactivation</span></span>
<span data-ttu-id="ef76b-236">多个模块提供配置设置，您可以禁用它们，而不从应用程序中删除它们。</span><span class="sxs-lookup"><span data-stu-id="ef76b-236">Many modules offer a configuration setting that will allow you to disable them without removing them from the application.</span></span> <span data-ttu-id="ef76b-237">这是最简单且最快速方式停用模块。</span><span class="sxs-lookup"><span data-stu-id="ef76b-237">This is the simplest and quickest way to deactivate a module.</span></span> <span data-ttu-id="ef76b-238">例如，如果你想要禁用 IIS URL 重写模块，请使用`<httpRedirect>`元素如下所示。</span><span class="sxs-lookup"><span data-stu-id="ef76b-238">For example if you wish to disable the IIS URL Rewrite Module, use the `<httpRedirect>` element as shown below.</span></span> <span data-ttu-id="ef76b-239">禁用使用配置设置的模块的详细信息，请按照中的链接*子元素*部分[IIS `<system.webServer>` ](https://www.iis.net/configreference/system.webserver)。</span><span class="sxs-lookup"><span data-stu-id="ef76b-239">For more information on disabling modules with configuration settings, follow the links in the *Child Elements* section of [IIS `<system.webServer>`](https://www.iis.net/configreference/system.webserver).</span></span>

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

### <a name="module-removal"></a><span data-ttu-id="ef76b-240">模块删除</span><span class="sxs-lookup"><span data-stu-id="ef76b-240">Module removal</span></span>
<span data-ttu-id="ef76b-241">如果你选择删除模块中的设置与*web.config*，你必须解锁模块和解锁`<modules>`部分*web.config*第一个。</span><span class="sxs-lookup"><span data-stu-id="ef76b-241">If you opt to remove a module with a setting in *web.config*, you must unlock the module and unlock the `<modules>` section of *web.config* first.</span></span> <span data-ttu-id="ef76b-242">步骤如下所述：</span><span class="sxs-lookup"><span data-stu-id="ef76b-242">The steps are outlined below:</span></span>

1. <span data-ttu-id="ef76b-243">解锁的服务器级别的模块。</span><span class="sxs-lookup"><span data-stu-id="ef76b-243">Unlock the module at the server level.</span></span> <span data-ttu-id="ef76b-244">在 IIS 管理器在 IIS 服务器上单击**连接**侧栏。</span><span class="sxs-lookup"><span data-stu-id="ef76b-244">Click on the IIS server in the IIS Manager **Connections** sidebar.</span></span> <span data-ttu-id="ef76b-245">打开**模块**中**IIS**区域。</span><span class="sxs-lookup"><span data-stu-id="ef76b-245">Open the **Modules** in the **IIS** area.</span></span> <span data-ttu-id="ef76b-246">单击列表中的模块。</span><span class="sxs-lookup"><span data-stu-id="ef76b-246">Click on the module in the list.</span></span> <span data-ttu-id="ef76b-247">在**操作**侧栏右侧，单击**解锁**。</span><span class="sxs-lookup"><span data-stu-id="ef76b-247">In the **Actions** sidebar on the right, click **Unlock**.</span></span> <span data-ttu-id="ef76b-248">解锁任意多个模块，因为你打算删除，但*web.config*更高版本。</span><span class="sxs-lookup"><span data-stu-id="ef76b-248">Unlock as many modules as you plan to remove with *web.config* later.</span></span>

2. <span data-ttu-id="ef76b-249">部署应用程序，而`<modules>`主题中*web.config*。</span><span class="sxs-lookup"><span data-stu-id="ef76b-249">Deploy your application without a `<modules>` section in *web.config*.</span></span> <span data-ttu-id="ef76b-250">如果你部署应用时采用*web.config*包含`<modules>`时你尝试解锁部分，而无需具有解锁部分首先在 IIS 管理器，Configuration Manager 的部分将引发异常。</span><span class="sxs-lookup"><span data-stu-id="ef76b-250">If you deploy an app with a *web.config* containing the `<modules>` section without having unlocked the section first in the IIS Manager, the Configuration Manager will throw an exception when you try to unlock the section.</span></span> <span data-ttu-id="ef76b-251">因此，部署应用程序，而`<modules>`部分。</span><span class="sxs-lookup"><span data-stu-id="ef76b-251">Therefore, deploy your application without a `<modules>` section.</span></span>

3. <span data-ttu-id="ef76b-252">解锁`<modules>`部分*web.config*。</span><span class="sxs-lookup"><span data-stu-id="ef76b-252">Unlock the `<modules>` section of *web.config*.</span></span> <span data-ttu-id="ef76b-253">在**连接**栏中，单击在网站**站点**。</span><span class="sxs-lookup"><span data-stu-id="ef76b-253">In the **Connections** sidebar, click the website in **Sites**.</span></span> <span data-ttu-id="ef76b-254">在**管理**区域中，打开**配置编辑器**。</span><span class="sxs-lookup"><span data-stu-id="ef76b-254">In the **Management** area, open the **Configuration Editor**.</span></span> <span data-ttu-id="ef76b-255">使用导航控件选择`system.webServer/modules`部分。</span><span class="sxs-lookup"><span data-stu-id="ef76b-255">Use the navigation controls to select the `system.webServer/modules` section.</span></span> <span data-ttu-id="ef76b-256">在**操作**侧栏右侧，单击到**解锁**部分。</span><span class="sxs-lookup"><span data-stu-id="ef76b-256">In the **Actions** sidebar on the right, click to **Unlock** the section.</span></span>

4. <span data-ttu-id="ef76b-257">此时，你将能够添加`<modules>`部分为你*web.config*文件`<remove>`元素从应用程序中删除该模块。</span><span class="sxs-lookup"><span data-stu-id="ef76b-257">At this point, you will be able to add a `<modules>` section to your *web.config* file with a `<remove>` element to remove the module from the application.</span></span> <span data-ttu-id="ef76b-258">你可以添加多个`<remove>`要移除多个模块元素。</span><span class="sxs-lookup"><span data-stu-id="ef76b-258">You can add multiple `<remove>` elements to remove multiple modules.</span></span> <span data-ttu-id="ef76b-259">不要忘记，如果你进行*web.config*服务器以便它们可以立即在本地项目上的更改。</span><span class="sxs-lookup"><span data-stu-id="ef76b-259">Don't forget that if you make *web.config* changes on the server to make them immediately in the project locally.</span></span> <span data-ttu-id="ef76b-260">删除这种方式的模块不会影响您的服务器上的其他应用的模块使用。</span><span class="sxs-lookup"><span data-stu-id="ef76b-260">Removing a module this way won't affect your use of the module with other apps on the server.</span></span>

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

<span data-ttu-id="ef76b-261">对于与默认模块安装 IIS 安装，你可以使用以下`<module>`部分，以移除默认模块。</span><span class="sxs-lookup"><span data-stu-id="ef76b-261">For an IIS installation with the default modules installed, you can use the following `<module>` section to remove the default modules.</span></span>

```xml
<modules>
  <remove name="CustomErrorModule" />
  <remove name="DefaultDocumentModule" />
  <remove name="DirectoryListingModule" />
  <remove name="HttpCacheModule" />
  <remove name="HttpLoggingModule" />
  <remove name="ProtocolSupportModule" />
  <remove name="RequestFilteringModule" />
  <remove name="StaticCompressionModule" /> 
  <remove name="StaticFileModule" /> 
</modules>
```

<span data-ttu-id="ef76b-262">您还可以删除某个 IIS 模块具有*Appcmd.exe*。</span><span class="sxs-lookup"><span data-stu-id="ef76b-262">You can also remove an IIS module with *Appcmd.exe*.</span></span> <span data-ttu-id="ef76b-263">提供`MODULE_NAME`和`APPLICATION_NAME`中下面显示的命令：</span><span class="sxs-lookup"><span data-stu-id="ef76b-263">Provide the `MODULE_NAME` and `APPLICATION_NAME` in the command shown below:</span></span>

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

<span data-ttu-id="ef76b-264">下面介绍了如何删除`DynamicCompressionModule`从默认网站：</span><span class="sxs-lookup"><span data-stu-id="ef76b-264">Here's how to remove the `DynamicCompressionModule` from the Default Web Site:</span></span>

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimal-module-configuration"></a><span data-ttu-id="ef76b-265">最少模块配置</span><span class="sxs-lookup"><span data-stu-id="ef76b-265">Minimal module configuration</span></span>
<span data-ttu-id="ef76b-266">若要运行 ASP.NET Core 应用程序所需的唯一模块是匿名身份验证模块和 ASP.NET 核心模块。</span><span class="sxs-lookup"><span data-stu-id="ef76b-266">The only modules required to run an ASP.NET Core application are the Anonymous Authentication Module and the ASP.NET Core Module.</span></span>

![显示最少模块配置到模块打开 IIS 管理器](iis-modules/_static/modules.png)

## <a name="resources"></a><span data-ttu-id="ef76b-268">资源</span><span class="sxs-lookup"><span data-stu-id="ef76b-268">Resources</span></span>
* [<span data-ttu-id="ef76b-269">发布到 IIS</span><span class="sxs-lookup"><span data-stu-id="ef76b-269">Publishing to IIS</span></span>](xref:publishing/iis)
* [<span data-ttu-id="ef76b-270">IIS 模块概述</span><span class="sxs-lookup"><span data-stu-id="ef76b-270">IIS Modules Overview</span></span>](https://www.iis.net/learn/get-started/introduction-to-iis/iis-modules-overview)
* [<span data-ttu-id="ef76b-271">自定义 IIS 7.0 角色和模块</span><span class="sxs-lookup"><span data-stu-id="ef76b-271">Customizing IIS 7.0 Roles and Modules</span></span>](https://technet.microsoft.com/library/cc627313.aspx)
* [<span data-ttu-id="ef76b-272">IIS`<system.webServer>`</span><span class="sxs-lookup"><span data-stu-id="ef76b-272">IIS `<system.webServer>`</span></span>](https://www.iis.net/configreference/system.webserver)
