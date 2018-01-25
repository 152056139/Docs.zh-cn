---
title: "使用 ASP.NET Core 中的多个环境"
author: rick-anderson
description: "了解如何 ASP.NET Core 提供支持用于跨多个环境中控制应用行为。"
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 60a1543ce11d08490e6df0eb84f980672ecfe672
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="60e0f-103">使用多个环境</span><span class="sxs-lookup"><span data-stu-id="60e0f-103">Working with multiple environments</span></span>

<span data-ttu-id="60e0f-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="60e0f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="60e0f-105">ASP.NET Core 提供对使用环境变量在运行时设置应用程序行为的支持。</span><span class="sxs-lookup"><span data-stu-id="60e0f-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="60e0f-106">[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="60e0f-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="60e0f-107">环境</span><span class="sxs-lookup"><span data-stu-id="60e0f-107">Environments</span></span>

<span data-ttu-id="60e0f-108">ASP.NET 核心读取环境变量`ASPNETCORE_ENVIRONMENT`在应用程序启动和中的值的存储[IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)。</span><span class="sxs-lookup"><span data-stu-id="60e0f-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="60e0f-109">`ASPNETCORE_ENVIRONMENT`可以将设置为任何值，但[三个值](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0)framework 支持：[开发](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0)，[过渡](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0)，和[生产](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="60e0f-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="60e0f-110">如果`ASPNETCORE_ENVIRONMENT`未设置，它将默认为`Production`。</span><span class="sxs-lookup"><span data-stu-id="60e0f-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="60e0f-111">前面的代码：</span><span class="sxs-lookup"><span data-stu-id="60e0f-111">The preceding code:</span></span>

* <span data-ttu-id="60e0f-112">调用[UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_)和[UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_)时`ASPNETCORE_ENVIRONMENT`设置为`Development`。</span><span class="sxs-lookup"><span data-stu-id="60e0f-112">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="60e0f-113">调用[UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_)时的值`ASPNETCORE_ENVIRONMENT`设置以下项之一：</span><span class="sxs-lookup"><span data-stu-id="60e0f-113">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="60e0f-114">[环境标记帮助器](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)使用的值`IHostingEnvironment.EnvironmentName`用于包含或排除元素中的标记：</span><span class="sxs-lookup"><span data-stu-id="60e0f-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="60e0f-115">注意： 在 Windows 和 macOS 上，环境变量和值不区分大小写。</span><span class="sxs-lookup"><span data-stu-id="60e0f-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="60e0f-116">Linux 环境变量和值是**区分大小写**默认情况下。</span><span class="sxs-lookup"><span data-stu-id="60e0f-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="60e0f-117">开发</span><span class="sxs-lookup"><span data-stu-id="60e0f-117">Development</span></span>

<span data-ttu-id="60e0f-118">开发环境可以启用不应在生产中公开的功能。</span><span class="sxs-lookup"><span data-stu-id="60e0f-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="60e0f-119">例如，ASP.NET Core 模板启用[开发人员异常页](xref:fundamentals/error-handling#the-developer-exception-page)在开发环境中。</span><span class="sxs-lookup"><span data-stu-id="60e0f-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="60e0f-120">可以在设置用于本地计算机开发环境*Properties\launchSettings.json*项目文件。</span><span class="sxs-lookup"><span data-stu-id="60e0f-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="60e0f-121">在中设置环境值*launchSettings.json*重写系统环境中设置值。</span><span class="sxs-lookup"><span data-stu-id="60e0f-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="60e0f-122">下面的 XML 演示三个配置文件从*launchSettings.json*文件：</span><span class="sxs-lookup"><span data-stu-id="60e0f-122">The following XML shows three profiles from a *launchSettings.json* file:</span></span>

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="60e0f-123">当应用程序启动与`dotnet run`，与第一个配置文件`"commandName": "Project"`将使用。</span><span class="sxs-lookup"><span data-stu-id="60e0f-123">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="60e0f-124">值`commandName`指定要启动的 web 服务器。</span><span class="sxs-lookup"><span data-stu-id="60e0f-124">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="60e0f-125">`commandName`可以是之一：</span><span class="sxs-lookup"><span data-stu-id="60e0f-125">`commandName` can be one of :</span></span>

* <span data-ttu-id="60e0f-126">IIS Express</span><span class="sxs-lookup"><span data-stu-id="60e0f-126">IIS Express</span></span>
* <span data-ttu-id="60e0f-127">IIS</span><span class="sxs-lookup"><span data-stu-id="60e0f-127">IIS</span></span>
* <span data-ttu-id="60e0f-128">项目 （该操作将启动 Kestrel）</span><span class="sxs-lookup"><span data-stu-id="60e0f-128">Project (which launches Kestrel)</span></span>

<span data-ttu-id="60e0f-129">当应用程序启动与`dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="60e0f-129">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="60e0f-130">*launchSettings.json*读取如果可用。</span><span class="sxs-lookup"><span data-stu-id="60e0f-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="60e0f-131">`environmentVariables`中的设置*launchSettings.json*重写环境变量。</span><span class="sxs-lookup"><span data-stu-id="60e0f-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="60e0f-132">将显示该宿主环境。</span><span class="sxs-lookup"><span data-stu-id="60e0f-132">The hosting environment is displayed.</span></span>


<span data-ttu-id="60e0f-133">下面的输出显示应用程序入门`dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="60e0f-133">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="60e0f-134">Visual Studio**调试**选项卡提供了一个 GUI 以编辑*launchSettings.json*文件：</span><span class="sxs-lookup"><span data-stu-id="60e0f-134">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![项目属性设置环境变量](environments/_static/project-properties-debug.png)

<span data-ttu-id="60e0f-136">在重新启动 web 服务器之前，对项目配置文件所做更改可能不会生效。</span><span class="sxs-lookup"><span data-stu-id="60e0f-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="60e0f-137">必须重启 kestrel，然后它将检测到其环境所做的更改。</span><span class="sxs-lookup"><span data-stu-id="60e0f-137">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="60e0f-138">*launchSettings.json*不应存储机密。</span><span class="sxs-lookup"><span data-stu-id="60e0f-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="60e0f-139">[机密管理器工具](xref:security/app-secrets)可以用于存储以进行本地开发的机密。</span><span class="sxs-lookup"><span data-stu-id="60e0f-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="60e0f-140">生产</span><span class="sxs-lookup"><span data-stu-id="60e0f-140">Production</span></span>

<span data-ttu-id="60e0f-141">生产环境应配置为最大程度提高安全性、 性能和应用程序的可靠性。</span><span class="sxs-lookup"><span data-stu-id="60e0f-141">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="60e0f-142">将不同于开发生产环境可能具有一些常见设置包括：</span><span class="sxs-lookup"><span data-stu-id="60e0f-142">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="60e0f-143">缓存。</span><span class="sxs-lookup"><span data-stu-id="60e0f-143">Caching.</span></span>
* <span data-ttu-id="60e0f-144">客户端的资源捆绑在一起，缩减的并且可能从 CDN 提供。</span><span class="sxs-lookup"><span data-stu-id="60e0f-144">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="60e0f-145">禁用诊断错误页。</span><span class="sxs-lookup"><span data-stu-id="60e0f-145">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="60e0f-146">启用友好错误页。</span><span class="sxs-lookup"><span data-stu-id="60e0f-146">Friendly error pages enabled.</span></span>
* <span data-ttu-id="60e0f-147">生产日志记录和启用监视。</span><span class="sxs-lookup"><span data-stu-id="60e0f-147">Production logging and monitoring enabled.</span></span> <span data-ttu-id="60e0f-148">例如， [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/)。</span><span class="sxs-lookup"><span data-stu-id="60e0f-148">For example, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="60e0f-149">将环境设置</span><span class="sxs-lookup"><span data-stu-id="60e0f-149">Setting the environment</span></span>

<span data-ttu-id="60e0f-150">通常它可用于设置用于测试的特定环境。</span><span class="sxs-lookup"><span data-stu-id="60e0f-150">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="60e0f-151">如果未设置环境，它将默认为`Production`这将禁用大多数调试功能。</span><span class="sxs-lookup"><span data-stu-id="60e0f-151">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="60e0f-152">将环境设置的方法取决于操作系统。</span><span class="sxs-lookup"><span data-stu-id="60e0f-152">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="60e0f-153">Azure</span><span class="sxs-lookup"><span data-stu-id="60e0f-153">Azure</span></span>

<span data-ttu-id="60e0f-154">对 Azure app service:</span><span class="sxs-lookup"><span data-stu-id="60e0f-154">For Azure app service:</span></span>

* <span data-ttu-id="60e0f-155">选择**应用程序设置**边栏选项卡。</span><span class="sxs-lookup"><span data-stu-id="60e0f-155">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="60e0f-156">将键和值中**应用设置**。</span><span class="sxs-lookup"><span data-stu-id="60e0f-156">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="60e0f-157">Windows</span><span class="sxs-lookup"><span data-stu-id="60e0f-157">Windows</span></span>
<span data-ttu-id="60e0f-158">若要设置`ASPNETCORE_ENVIRONMENT`当前的会话中，如果应用程序将启动使用`dotnet run`，可以使用以下命令</span><span class="sxs-lookup"><span data-stu-id="60e0f-158">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="60e0f-159">**命令行**</span><span class="sxs-lookup"><span data-stu-id="60e0f-159">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="60e0f-160">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="60e0f-160">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="60e0f-161">这些命令将仅为当前窗口的生效。</span><span class="sxs-lookup"><span data-stu-id="60e0f-161">These commands take effect only for the current window.</span></span> <span data-ttu-id="60e0f-162">当窗口已关闭时，ASPNETCORE_ENVIRONMENT 设置将恢复为默认设置或计算机的值。</span><span class="sxs-lookup"><span data-stu-id="60e0f-162">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="60e0f-163">为了上 Windows 打开全局设置的值**控制面板** > **系统** > **高级系统设置**和添加或编辑`ASPNETCORE_ENVIRONMENT`值。</span><span class="sxs-lookup"><span data-stu-id="60e0f-163">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![高级属性的系统](environments/_static/systemsetting_environment.png)

![ASPNET 核心环境变量](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="60e0f-166">**web.config**</span><span class="sxs-lookup"><span data-stu-id="60e0f-166">**web.config**</span></span>

<span data-ttu-id="60e0f-167">请参阅*设置环境变量*部分[ASP.NET 核心模块配置引用](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)主题。</span><span class="sxs-lookup"><span data-stu-id="60e0f-167">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="60e0f-168">**每个 IIS 应用程序池**</span><span class="sxs-lookup"><span data-stu-id="60e0f-168">**Per IIS Application Pool**</span></span>

<span data-ttu-id="60e0f-169">若要设置对于在独立的应用程序池 （IIS 10.0 + 支持） 中运行的单个应用程序的环境变量，请参阅*AppCmd.exe 命令*部分[环境变量\<environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)主题。</span><span class="sxs-lookup"><span data-stu-id="60e0f-169">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="60e0f-170">macOS</span><span class="sxs-lookup"><span data-stu-id="60e0f-170">macOS</span></span>
<span data-ttu-id="60e0f-171">设置 macOS 的当前环境，可以进行串联时运行该应用程序;</span><span class="sxs-lookup"><span data-stu-id="60e0f-171">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="60e0f-172">也可以使用`export`若要在运行应用程序之前设置它。</span><span class="sxs-lookup"><span data-stu-id="60e0f-172">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="60e0f-173">设置计算机级别的环境变量*.bashrc*或*.bash_profile*文件。</span><span class="sxs-lookup"><span data-stu-id="60e0f-173">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="60e0f-174">使用任何文本编辑器编辑该文件并添加以下语句。</span><span class="sxs-lookup"><span data-stu-id="60e0f-174">Edit the file using any text editor and add the following statment.</span></span>

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="60e0f-175">Linux</span><span class="sxs-lookup"><span data-stu-id="60e0f-175">Linux</span></span>
<span data-ttu-id="60e0f-176">对于 Linux 发行版本，使用`export`基于会话的变量设置在命令行命令和*bash_profile*机级别的环境设置的文件。</span><span class="sxs-lookup"><span data-stu-id="60e0f-176">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="60e0f-177">按环境配置</span><span class="sxs-lookup"><span data-stu-id="60e0f-177">Configuration by environment</span></span>

<span data-ttu-id="60e0f-178">请参阅[由环境配置](xref:fundamentals/configuration/index#configuration-by-environment)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="60e0f-178">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="60e0f-179">环境基于 Startup 类和方法</span><span class="sxs-lookup"><span data-stu-id="60e0f-179">Environment based Startup class and methods</span></span>

<span data-ttu-id="60e0f-180">当 ASP.NET Core 应用启动后时， [Startup 类](xref:fundamentals/startup)启动应用程序。</span><span class="sxs-lookup"><span data-stu-id="60e0f-180">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="60e0f-181">如果一个类`Startup{EnvironmentName}`存在，将该调用类`EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="60e0f-181">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="60e0f-182">注意： 调用[WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)重写配置节。</span><span class="sxs-lookup"><span data-stu-id="60e0f-182">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="60e0f-183">[配置](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_)和[ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0)支持环境特定版本的表单`Configure{EnvironmentName}`和`Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="60e0f-183">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="60e0f-184">其他资源</span><span class="sxs-lookup"><span data-stu-id="60e0f-184">Additional Resources</span></span>

* [<span data-ttu-id="60e0f-185">应用程序启动</span><span class="sxs-lookup"><span data-stu-id="60e0f-185">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="60e0f-186">配置</span><span class="sxs-lookup"><span data-stu-id="60e0f-186">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="60e0f-187">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="60e0f-187">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
