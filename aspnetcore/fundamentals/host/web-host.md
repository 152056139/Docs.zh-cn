---
title: ASP.NET Core Web 主机
author: guardrex
description: 了解有关 ASP.NET Core 中负责应用启动和生存期管理的 Web 主机。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/web-host
ms.openlocfilehash: ce95599ec8e940635ca63c3bf9a3c28784a3f371
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2018
ms.locfileid: "34687485"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="fd7eb-103">ASP.NET Core Web 主机</span><span class="sxs-lookup"><span data-stu-id="fd7eb-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="fd7eb-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fd7eb-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fd7eb-105">ASP.NET Core 应用配置和启动“主机”。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="fd7eb-106">主机负责应用程序启动和生存期管理。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="fd7eb-107">至少，主机配置服务器和请求处理管道。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="fd7eb-108">本主题介绍 ASP.NET Core Web 主机 ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder))，它可用于托管 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="fd7eb-109">有关 .NET 通用主机 ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)) 的介绍，请参阅[通用主机](xref:fundamentals/host/generic-host)主题。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="fd7eb-110">设置主机</span><span class="sxs-lookup"><span data-stu-id="fd7eb-110">Set up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd7eb-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="fd7eb-112">创建使用 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) 实例的主机。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-112">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="fd7eb-113">通常在应用的入口点来执行 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="fd7eb-114">在项目模板中，`Main` 位于 Program.cs。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="fd7eb-115">典型的 Program.cs 调用 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 以开始设置主机：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

<span data-ttu-id="fd7eb-116">`CreateDefaultBuilder` 执行下列任务：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="fd7eb-117">使用应用的托管配置提供程序将 [Kestrel](xref:fundamentals/servers/kestrel) 配置为 Web 服务器并配置该服务器。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="fd7eb-118">有关 Kestrel 默认选项，请参阅[在 ASP.NET Core 中 Kestrel Web 服务器实现的 Kestrel 选项部分](xref:fundamentals/servers/kestrel#kestrel-options)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-118">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="fd7eb-119">将内容根设置为由 [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) 返回的路径。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="fd7eb-120">从下列选项中加载可选 [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration)：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-120">Loads optional [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) from:</span></span>
  * <span data-ttu-id="fd7eb-121">appsettings.json。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-121">*appsettings.json*.</span></span>
  * <span data-ttu-id="fd7eb-122">appsettings.{Environment}.json。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-122">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="fd7eb-123">应用在使用入口程序集的 `Development` 环境中运行时的[用户机密](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-123">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="fd7eb-124">环境变量。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-124">Environment variables.</span></span>
  * <span data-ttu-id="fd7eb-125">命令行参数。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-125">Command-line arguments.</span></span>
* <span data-ttu-id="fd7eb-126">配置控制台和调试输出的[日志记录](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-126">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="fd7eb-127">日志记录包含 appsettings.json 或 appsettings.{Environment}.json 文件的日志记录配置部分中指定的[日志筛选](xref:fundamentals/logging/index#log-filtering)规则。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-127">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="fd7eb-128">在 IIS 后方运行时，启用 [IIS 集成](xref:host-and-deploy/iis/index)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-128">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="fd7eb-129">当使用 [ASP.NET Core 模块](xref:fundamentals/servers/aspnet-core-module)时配置基路径和被服务器侦听的端口。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-129">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="fd7eb-130">该模块创建 IIS 与 Kestrel 之间的反向代理。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-130">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="fd7eb-131">还配置应用到[捕获启动错误](#capture-startup-errors)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-131">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="fd7eb-132">有关 IIS 默认选项，请参阅[使用 IIS 在 Windows 上托管 ASP.NET Core 的 IIS 选项部分](xref:host-and-deploy/iis/index#iis-options)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-132">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="fd7eb-133">如果应用环境为“开发”，请将 [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) 设为 `true`。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-133">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="fd7eb-134">有关详细信息，请参阅[作用域验证](#scope-validation)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-134">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="fd7eb-135">内容根确定主机搜索内容文件（如 MVC 视图文件）的位置。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-135">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="fd7eb-136">应用从项目的根文件夹启动时，会将项目的根文件夹用作内容根。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-136">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="fd7eb-137">这是 [Visual Studio](https://www.visualstudio.com/) 和 [dotnet new 模板](/dotnet/core/tools/dotnet-new)中使用的默认值。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-137">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="fd7eb-138">有关应用配置的详细信息，请参阅 [ ASP.NET Core 中的配置](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-138">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="fd7eb-139">作为使用静态 `CreateDefaultBuilder` 方法的替代方法，从 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 创建主机是一种受 ASP.NET Core 2.x 支持的方法。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-139">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="fd7eb-140">有关详细信息，请参阅 ASP.NET Core 1.x 选项卡。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-140">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd7eb-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="fd7eb-142">创建使用 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 实例的主机。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-142">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="fd7eb-143">通常执行 `Main` 方法，在应用的入口点来创建主机。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-143">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="fd7eb-144">在项目模板中，`Main` 位于 Program.cs：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-144">In the project templates, `Main` is located in *Program.cs*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .Build();
}
```

<span data-ttu-id="fd7eb-145">`WebHostBuilder` 需要[实现 IServer 的服务器](xref:fundamentals/servers/index)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-145">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="fd7eb-146">内置服务器是 [Kestrel](xref:fundamentals/servers/kestrel) 和 [HTTP.sys](xref:fundamentals/servers/httpsys)（在 ASP.NET Core 2.0 之前的版本中，HTTP.sys 叫做 [WebListener](xref:fundamentals/servers/weblistener)）。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-146">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="fd7eb-147">在此示例中，[UseKestrel 扩展方法](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)指定 Kestrel 服务器。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-147">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="fd7eb-148">内容根确定主机搜索内容文件（如 MVC 视图文件）的位置。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="fd7eb-149">默认内容根通过 [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1) 为 `UseContentRoot` 获得。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-149">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="fd7eb-150">应用从项目的根文件夹启动时，会将项目的根文件夹用作内容根。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="fd7eb-151">这是 [Visual Studio](https://www.visualstudio.com/) 和 [dotnet new 模板](/dotnet/core/tools/dotnet-new)中使用的默认值。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="fd7eb-152">若要使用 IIS 作为反向代理，调用 [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) 作为生成主机的一部分。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-152">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="fd7eb-153">`UseIISIntegration` 不配置服务器，而 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) 要配置。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-153">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="fd7eb-154">当使用 [ASP.NET Core 模块](xref:fundamentals/servers/aspnet-core-module)创建 Kestrel 和 IIS 之间的反向代理时，`UseIISIntegration` 配置基路径和被服务器侦听的端口。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-154">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="fd7eb-155">要在 ASP.NET Core 中使用 IIS，必须指定 `UseKestrel` 和 `UseIISIntegration`。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-155">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="fd7eb-156">`UseIISIntegration` 仅在 IIS 或 IIS Express 后方运行时激活。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-156">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="fd7eb-157">有关详细信息，请参阅 [ASP.NET Core 模块简介](xref:fundamentals/servers/aspnet-core-module)和 [ASP.NET Core 模块配置参考](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-157">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="fd7eb-158">配置主机 （和 ASP.NET Core 应用） 的最小实现包括指定服务器和应用的请求管道的配置项：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-158">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

<span data-ttu-id="fd7eb-159">设置主机时，可以提供[配置](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1)和 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) 方法。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-159">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="fd7eb-160">如果指定 `Startup` 类，必须定义 `Configure` 方法。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-160">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="fd7eb-161">有关详细信息，请参阅 [ASP.NET Core 中的应用程序启动](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-161">For more information, see [Application Startup in ASP.NET Core](xref:fundamentals/startup).</span></span> <span data-ttu-id="fd7eb-162">多次调用 `ConfigureServices` 将追加到另一个。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-162">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="fd7eb-163">多次调用 `WebHostBuilder` 上的 `Configure` 或 `UseStartup` 将替换以前的设置。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-163">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="fd7eb-164">主机配置值</span><span class="sxs-lookup"><span data-stu-id="fd7eb-164">Host configuration values</span></span>

<span data-ttu-id="fd7eb-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 依赖于以下的方法设置主机配置值：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="fd7eb-166">主机生成器配置，其中包括格式 `ASPNETCORE_{configurationKey}` 的环境变量。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-166">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="fd7eb-167">例如 `ASPNETCORE_ENVIRONMENT`。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-167">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="fd7eb-168">显式方法，如 [HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-168">Explicit methods, such as [HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot).</span></span>
* <span data-ttu-id="fd7eb-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) 和关联键。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="fd7eb-170">使用 `UseSetting` 设置值时，该值设置为无论何种类型的字符串。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-170">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="fd7eb-171">主机使用任何一个选项设置上一个值。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-171">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="fd7eb-172">有关详细信息，请参阅下一部分中的[重写配置](#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-172">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="fd7eb-173">捕获启动错误</span><span class="sxs-lookup"><span data-stu-id="fd7eb-173">Capture Startup Errors</span></span>

<span data-ttu-id="fd7eb-174">此设置控制启动错误的捕获。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-174">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="fd7eb-175">**键**：captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="fd7eb-175">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="fd7eb-176">**类型**：布尔型（`true` 或 `1`）</span><span class="sxs-lookup"><span data-stu-id="fd7eb-176">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="fd7eb-177">**默认值**：默认为 `false`，除非应用使用 Kestrel 在 IIS 后方运行，其中默认值是 `true`。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-177">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="fd7eb-178">**设置使用**：`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-178">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="fd7eb-179">**环境变量**：`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-179">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="fd7eb-180">当 `false` 时，启动期间出错导致主机退出。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-180">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="fd7eb-181">当 `true` 时，主机在启动期间捕获异常并尝试启动服务器。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-181">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd7eb-182">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd7eb-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

---

### <a name="content-root"></a><span data-ttu-id="fd7eb-184">内容根</span><span class="sxs-lookup"><span data-stu-id="fd7eb-184">Content Root</span></span>

<span data-ttu-id="fd7eb-185">此设置确定 ASP.NET Core 开始搜索内容文件，如 MVC 视图等。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-185">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="fd7eb-186">**键**：contentRoot</span><span class="sxs-lookup"><span data-stu-id="fd7eb-186">**Key**: contentRoot</span></span>  
<span data-ttu-id="fd7eb-187">**类型**：string</span><span class="sxs-lookup"><span data-stu-id="fd7eb-187">**Type**: *string*</span></span>  
<span data-ttu-id="fd7eb-188">**默认值**：默认为应用程序集所在的文件夹。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-188">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="fd7eb-189">**设置使用**：`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-189">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="fd7eb-190">**环境变量**：`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-190">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="fd7eb-191">内容根也用作 [Web 根设置](#web-root)的基路径。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-191">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="fd7eb-192">如果路径不存在，主机将无法启动。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-192">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd7eb-193">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-193">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd7eb-194">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-194">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

---

### <a name="detailed-errors"></a><span data-ttu-id="fd7eb-195">详细错误</span><span class="sxs-lookup"><span data-stu-id="fd7eb-195">Detailed Errors</span></span>

<span data-ttu-id="fd7eb-196">确定是否应捕获详细错误。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-196">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="fd7eb-197">**键**：detailedErrors</span><span class="sxs-lookup"><span data-stu-id="fd7eb-197">**Key**: detailedErrors</span></span>  
<span data-ttu-id="fd7eb-198">**类型**：布尔型（`true` 或 `1`）</span><span class="sxs-lookup"><span data-stu-id="fd7eb-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="fd7eb-199">**默认值**：false</span><span class="sxs-lookup"><span data-stu-id="fd7eb-199">**Default**: false</span></span>  
<span data-ttu-id="fd7eb-200">**设置使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-200">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="fd7eb-201">**环境变量**：`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-201">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="fd7eb-202">启用（或当<a href="#environment">环境</a>设置为 `Development` ）时，应用捕获详细的异常。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-202">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd7eb-203">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-203">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd7eb-204">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-204">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

---

### <a name="environment"></a><span data-ttu-id="fd7eb-205">环境</span><span class="sxs-lookup"><span data-stu-id="fd7eb-205">Environment</span></span>

<span data-ttu-id="fd7eb-206">设置应用的环境。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-206">Sets the app's environment.</span></span>

<span data-ttu-id="fd7eb-207">**键**：环境</span><span class="sxs-lookup"><span data-stu-id="fd7eb-207">**Key**: environment</span></span>  
<span data-ttu-id="fd7eb-208">**类型**：string</span><span class="sxs-lookup"><span data-stu-id="fd7eb-208">**Type**: *string*</span></span>  
<span data-ttu-id="fd7eb-209">**默认值**：生产</span><span class="sxs-lookup"><span data-stu-id="fd7eb-209">**Default**: Production</span></span>  
<span data-ttu-id="fd7eb-210">**设置使用**：`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-210">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="fd7eb-211">**环境变量**：`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-211">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="fd7eb-212">环境可以设置为任何值。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-212">The environment can be set to any value.</span></span> <span data-ttu-id="fd7eb-213">框架定义的值包括 `Development``Staging` 和 `Production`。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-213">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="fd7eb-214">值不区分大小写。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-214">Values aren't case sensitive.</span></span> <span data-ttu-id="fd7eb-215">默认情况下，从 `ASPNETCORE_ENVIRONMENT` 环境变量读取环境。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-215">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="fd7eb-216">使用 [Visual Studio](https://www.visualstudio.com/) 时，可能会在 launchSettings.json 文件中设置环境变量。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-216">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="fd7eb-217">有关详细信息，请参阅[使用多个环境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-217">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd7eb-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd7eb-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="fd7eb-220">承载启动程序集</span><span class="sxs-lookup"><span data-stu-id="fd7eb-220">Hosting Startup Assemblies</span></span>

<span data-ttu-id="fd7eb-221">设置应用的承载启动程序集。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-221">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="fd7eb-222">**键**：hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="fd7eb-222">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="fd7eb-223">**类型**：string</span><span class="sxs-lookup"><span data-stu-id="fd7eb-223">**Type**: *string*</span></span>  
<span data-ttu-id="fd7eb-224">**默认值**：空字符串</span><span class="sxs-lookup"><span data-stu-id="fd7eb-224">**Default**: Empty string</span></span>  
<span data-ttu-id="fd7eb-225">**设置使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-225">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="fd7eb-226">**环境变量**：`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-226">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="fd7eb-227">承载启动程序集的以分号分隔的字符串在启动时加载。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-227">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="fd7eb-228">此功能是 ASP.NET Core 2.0 中的新增功能。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-228">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="fd7eb-229">虽然配置值默认为空字符串，但是承载启动程序集会始终包含应用的程序集。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-229">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="fd7eb-230">提供承载启动程序集时，当应用在启动过程中生成其公用服务时将它们添加到应用的程序集加载。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-230">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd7eb-231">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-231">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd7eb-232">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-232">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fd7eb-233">此功能在 ASP.NET Core 1.x 中不可用。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-233">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="fd7eb-234">首选承载 URL</span><span class="sxs-lookup"><span data-stu-id="fd7eb-234">Prefer Hosting URLs</span></span>

<span data-ttu-id="fd7eb-235">指示主机是否应该侦听使用 `WebHostBuilder` 配置的 URL，而不是使用 `IServer` 实现配置的 URL。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-235">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="fd7eb-236">**键**：preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="fd7eb-236">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="fd7eb-237">**类型**：布尔型（`true` 或 `1`）</span><span class="sxs-lookup"><span data-stu-id="fd7eb-237">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="fd7eb-238">**默认值**：true</span><span class="sxs-lookup"><span data-stu-id="fd7eb-238">**Default**: true</span></span>  
<span data-ttu-id="fd7eb-239">**设置使用**：`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-239">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="fd7eb-240">**环境变量**：`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-240">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="fd7eb-241">此功能是 ASP.NET Core 2.0 中的新增功能。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-241">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd7eb-242">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-242">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd7eb-243">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-243">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fd7eb-244">此功能在 ASP.NET Core 1.x 中不可用。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-244">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="fd7eb-245">阻止承载启动</span><span class="sxs-lookup"><span data-stu-id="fd7eb-245">Prevent Hosting Startup</span></span>

<span data-ttu-id="fd7eb-246">阻止承载启动程序集自动加载，包括应用的程序集所配置的承载启动程序集。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-246">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="fd7eb-247">有关详细信息，请参阅[使用 IHostingStartup 从外部程序集增强应用](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-247">See [Enhance an app from an external assembly with IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="fd7eb-248">**键**：preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="fd7eb-248">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="fd7eb-249">**类型**：布尔型（`true` 或 `1`）</span><span class="sxs-lookup"><span data-stu-id="fd7eb-249">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="fd7eb-250">**默认值**：false</span><span class="sxs-lookup"><span data-stu-id="fd7eb-250">**Default**: false</span></span>  
<span data-ttu-id="fd7eb-251">**设置使用**：`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-251">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="fd7eb-252">**环境变量**：`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-252">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="fd7eb-253">此功能是 ASP.NET Core 2.0 中的新增功能。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-253">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd7eb-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd7eb-255">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-255">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fd7eb-256">此功能在 ASP.NET Core 1.x 中不可用。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-256">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="fd7eb-257">服务器 URL</span><span class="sxs-lookup"><span data-stu-id="fd7eb-257">Server URLs</span></span>

<span data-ttu-id="fd7eb-258">指示 IP 地址或主机地址，其中包含服务器应针对请求侦听的端口和协议。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-258">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="fd7eb-259">**键**：urls</span><span class="sxs-lookup"><span data-stu-id="fd7eb-259">**Key**: urls</span></span>  
<span data-ttu-id="fd7eb-260">**类型**：string</span><span class="sxs-lookup"><span data-stu-id="fd7eb-260">**Type**: *string*</span></span>  
<span data-ttu-id="fd7eb-261">**默认**：http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="fd7eb-261">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="fd7eb-262">**设置使用**：`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-262">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="fd7eb-263">**环境变量**：`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-263">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="fd7eb-264">设置为服务器应响应的以分号分隔 (;) 的 URL 前缀列表。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-264">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="fd7eb-265">例如 `http://localhost:123`。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-265">For example, `http://localhost:123`.</span></span> <span data-ttu-id="fd7eb-266">使用“\*”指示服务器应针对请求侦听的使用特定端口和协议（例如 `http://*:5000`）的 IP 地址或主机名。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-266">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="fd7eb-267">协议（`http://` 或 `https://`）必须包含每个 URL。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-267">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="fd7eb-268">不同的服务器支持的格式有所不同。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-268">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd7eb-269">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-269">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="fd7eb-270">Kestrel 具有自己的终结点配置 API。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-270">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="fd7eb-271">有关详细信息，请参阅 [ASP.NET Core 中的 Kestrel Web 服务器实现](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-271">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd7eb-272">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-272">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="fd7eb-273">关闭超时</span><span class="sxs-lookup"><span data-stu-id="fd7eb-273">Shutdown Timeout</span></span>

<span data-ttu-id="fd7eb-274">指定等待 Web 主机关闭的时长。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-274">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="fd7eb-275">**键**：shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="fd7eb-275">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="fd7eb-276">**类型**：int</span><span class="sxs-lookup"><span data-stu-id="fd7eb-276">**Type**: *int*</span></span>  
<span data-ttu-id="fd7eb-277">**默认值**：5</span><span class="sxs-lookup"><span data-stu-id="fd7eb-277">**Default**: 5</span></span>  
<span data-ttu-id="fd7eb-278">**设置使用**：`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-278">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="fd7eb-279">**环境变量**：`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-279">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="fd7eb-280">虽然键使用 `UseSetting` 接受 int（例如 `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`），但是 [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) 扩展方法采用 [TimeSpan](/dotnet/api/system.timespan)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-280">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="fd7eb-281">此功能是 ASP.NET Core 2.0 中的新增功能。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-281">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="fd7eb-282">在超时时间段中，托管：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-282">During the timeout period, hosting:</span></span>

* <span data-ttu-id="fd7eb-283">触发器 [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-283">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="fd7eb-284">尝试停止托管服务，对服务停止失败的任何错误进行日志记录。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-284">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="fd7eb-285">如果在所有托管服务停止之前就达到了超时时间，则会在应用关闭时会终止剩余的所有活动的服务。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-285">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="fd7eb-286">即使没有完成处理工作，服务也会停止。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-286">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="fd7eb-287">如果停止服务需要额外的时间，请增加超时时间。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-287">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd7eb-288">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-288">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd7eb-289">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-289">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fd7eb-290">此功能在 ASP.NET Core 1.x 中不可用。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-290">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="fd7eb-291">启动程序集</span><span class="sxs-lookup"><span data-stu-id="fd7eb-291">Startup Assembly</span></span>

<span data-ttu-id="fd7eb-292">确定要在其中搜索 `Startup` 类的程序集。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-292">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="fd7eb-293">**键**：startupAssembly</span><span class="sxs-lookup"><span data-stu-id="fd7eb-293">**Key**: startupAssembly</span></span>  
<span data-ttu-id="fd7eb-294">**类型**：string</span><span class="sxs-lookup"><span data-stu-id="fd7eb-294">**Type**: *string*</span></span>  
<span data-ttu-id="fd7eb-295">**默认值**：应用的程序集</span><span class="sxs-lookup"><span data-stu-id="fd7eb-295">**Default**: The app's assembly</span></span>  
<span data-ttu-id="fd7eb-296">**设置使用**：`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-296">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="fd7eb-297">**环境变量**：`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-297">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="fd7eb-298">按名称（`string`）或类型（`TStartup`）的程序集可以引用。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-298">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="fd7eb-299">如果调用多个 `UseStartup` 方法，优先选择最后一个方法。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-299">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd7eb-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd7eb-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

---

### <a name="web-root"></a><span data-ttu-id="fd7eb-302">Web 根路径</span><span class="sxs-lookup"><span data-stu-id="fd7eb-302">Web Root</span></span>

<span data-ttu-id="fd7eb-303">设置应用的静态资产的相对路径。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-303">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="fd7eb-304">**键**：webroot</span><span class="sxs-lookup"><span data-stu-id="fd7eb-304">**Key**: webroot</span></span>  
<span data-ttu-id="fd7eb-305">**类型**：string</span><span class="sxs-lookup"><span data-stu-id="fd7eb-305">**Type**: *string*</span></span>  
<span data-ttu-id="fd7eb-306">**默认值**：如果未指定，默认值是“(Content Root)/wwwroot”（如果该路径存在）。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-306">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="fd7eb-307">如果该路径不存在，则使用无操作文件提供程序。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-307">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="fd7eb-308">**设置使用**：`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-308">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="fd7eb-309">**环境变量**：`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="fd7eb-309">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd7eb-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd7eb-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

---

## <a name="override-configuration"></a><span data-ttu-id="fd7eb-312">重写配置</span><span class="sxs-lookup"><span data-stu-id="fd7eb-312">Override configuration</span></span>

<span data-ttu-id="fd7eb-313">使用[配置](xref:fundamentals/configuration/index)以配置主机。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-313">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="fd7eb-314">在以下示例中，在 hosting.json 文件中指定主机配置（可选）。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-314">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="fd7eb-315">从 hosting.json 文件加载的任何配置可能会由命令行参数替代。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-315">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="fd7eb-316">生成的配置（在 `config` 中）用于通过 `UseConfiguration` 配置主机。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-316">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd7eb-317">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-317">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fd7eb-318">hosting.json：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-318">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="fd7eb-319">首先用 hosting.json 配置替代 `UseUrls` 提供的配置，然后是命令行参数配置：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-319">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd7eb-320">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-320">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fd7eb-321">hosting.json：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-321">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="fd7eb-322">首先用 hosting.json 配置替代 `UseUrls` 提供的配置，然后是命令行参数配置：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-322">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> <span data-ttu-id="fd7eb-323">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 扩展方法当前不能分析由 `GetSection` 返回的配置部分（例如 `.UseConfiguration(Configuration.GetSection("section"))`。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-323">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="fd7eb-324">`GetSection` 方法将配置键筛选到所请求的部分，但将节名称保留在键上（例如 `section:urls`、`section:environment`）。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-324">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="fd7eb-325">`UseConfiguration` 方法需要键来匹配 `WebHostBuilder` 键（例如 `urls`、`environment`）。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-325">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="fd7eb-326">键上存在的节名称阻止节的值配置主机。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-326">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="fd7eb-327">将在即将发布的版本中解决此问题。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-327">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="fd7eb-328">有关详细信息和解决方法，请参阅[将配置节传入到 WebHostBuilder.UseConfiguration 使用完整的键](https://github.com/aspnet/Hosting/issues/839)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-328">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="fd7eb-329">若要指定在特定的 URL 上运行的主机，所需的值可以在执行 [dotnet 运行](/dotnet/core/tools/dotnet-run)时从命令提示符传入。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-329">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="fd7eb-330">命令行参数替代 hosting.json 文件中的 `urls` 值，且服务器侦听端口 8080：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-330">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="fd7eb-331">管理主机</span><span class="sxs-lookup"><span data-stu-id="fd7eb-331">Manage the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd7eb-332">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-332">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fd7eb-333">**运行**</span><span class="sxs-lookup"><span data-stu-id="fd7eb-333">**Run**</span></span>

<span data-ttu-id="fd7eb-334">`Run` 方法启动 Web 应用并阻止调用线程，直到关闭主机：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-334">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="fd7eb-335">**Start**</span><span class="sxs-lookup"><span data-stu-id="fd7eb-335">**Start**</span></span>

<span data-ttu-id="fd7eb-336">通过调用 `Start` 方法以非阻止方式运行主机：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-336">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="fd7eb-337">如果 URL 列表传递给 `Start` 方法，该列表侦听指定的 URL：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-337">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="fd7eb-338">应用可以使用通过静态便捷方法预配置的 `CreateDefaultBuilder` 默认值初始化并启动新的主机。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-338">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="fd7eb-339">这些方法在没有控制台输出的情况下启动服务器，并使用 [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) 等待中断（Ctrl-C/SIGINT 或 SIGTERM）：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-339">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="fd7eb-340">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="fd7eb-340">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="fd7eb-341">从 `RequestDelegate` 开始：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-341">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="fd7eb-342">在浏览器中向 `http://localhost:5000` 发出请求，接收响应“Hello World!”</span><span class="sxs-lookup"><span data-stu-id="fd7eb-342">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="fd7eb-343">`WaitForShutdown` 受到阻止，直到发出中断（Ctrl-C/SIGINT 或 SIGTERM）。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-343">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="fd7eb-344">应用显示 `Console.WriteLine` 消息并等待 keypress 退出。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-344">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="fd7eb-345">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="fd7eb-345">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="fd7eb-346">从 URL 和 `RequestDelegate` 开始：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-346">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="fd7eb-347">生成与 Start(RequestDelegate app) 相同的结果，除非应用在 `http://localhost:8080` 上响应。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-347">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="fd7eb-348">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="fd7eb-348">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="fd7eb-349">使用 `IRouteBuilder` 的实例 ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) 用于路由中间件：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-349">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="fd7eb-350">该示例中使用以下浏览器请求：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-350">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="fd7eb-351">请求</span><span class="sxs-lookup"><span data-stu-id="fd7eb-351">Request</span></span>                                    | <span data-ttu-id="fd7eb-352">响应</span><span class="sxs-lookup"><span data-stu-id="fd7eb-352">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="fd7eb-353">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="fd7eb-353">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="fd7eb-354">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="fd7eb-354">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="fd7eb-355">使用“ooops!”字符串引发异常</span><span class="sxs-lookup"><span data-stu-id="fd7eb-355">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="fd7eb-356">使用“Uh oh!”字符串引发异常</span><span class="sxs-lookup"><span data-stu-id="fd7eb-356">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="fd7eb-357">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="fd7eb-357">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="fd7eb-358">Hello World!</span><span class="sxs-lookup"><span data-stu-id="fd7eb-358">Hello World!</span></span>                             |

<span data-ttu-id="fd7eb-359">`WaitForShutdown` 受到阻止，直到发出中断（Ctrl-C/SIGINT 或 SIGTERM）。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-359">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="fd7eb-360">应用显示 `Console.WriteLine` 消息并等待 keypress 退出。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-360">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="fd7eb-361">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="fd7eb-361">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="fd7eb-362">使用 URL 和 `IRouteBuilder` 实例：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-362">Use a URL and an instance of `IRouteBuilder`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="fd7eb-363">生成与 Start(Action&lt;IRouteBuilder&gt; routeBuilder) 相同的结果，除非应用在 `http://localhost:8080` 上响应。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-363">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="fd7eb-364">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="fd7eb-364">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="fd7eb-365">提供委托以配置 `IApplicationBuilder`：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-365">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="fd7eb-366">在浏览器中向 `http://localhost:5000` 发出请求，接收响应“Hello World!”</span><span class="sxs-lookup"><span data-stu-id="fd7eb-366">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="fd7eb-367">`WaitForShutdown` 受到阻止，直到发出中断（Ctrl-C/SIGINT 或 SIGTERM）。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-367">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="fd7eb-368">应用显示 `Console.WriteLine` 消息并等待 keypress 退出。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-368">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="fd7eb-369">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="fd7eb-369">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="fd7eb-370">提供 URL 和委托以配置 `IApplicationBuilder`：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-370">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="fd7eb-371">生成与 StartWith(Action&lt;IApplicationBuilder&gt; app) 相同的结果，除非应用在 `http://localhost:8080` 上响应。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-371">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd7eb-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd7eb-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fd7eb-373">**运行**</span><span class="sxs-lookup"><span data-stu-id="fd7eb-373">**Run**</span></span>

<span data-ttu-id="fd7eb-374">`Run` 方法启动 Web 应用并阻止调用线程，直到关闭主机：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-374">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="fd7eb-375">**Start**</span><span class="sxs-lookup"><span data-stu-id="fd7eb-375">**Start**</span></span>

<span data-ttu-id="fd7eb-376">通过调用 `Start` 方法以非阻止方式运行主机：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-376">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="fd7eb-377">如果 URL 列表传递给 `Start` 方法，该列表侦听指定的 URL：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-377">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="fd7eb-378">IHostingEnvironment 接口</span><span class="sxs-lookup"><span data-stu-id="fd7eb-378">IHostingEnvironment interface</span></span>

<span data-ttu-id="fd7eb-379">[IHostingEnvironment 接口](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)提供有关应用的 Web 承载环境的信息。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-379">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="fd7eb-380">使用[构造函数注入](xref:fundamentals/dependency-injection)获取 `IHostingEnvironment` 以使用其属性和扩展方法：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-380">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

<span data-ttu-id="fd7eb-381">[基于约定的方法](xref:fundamentals/environments#startup-conventions)可以用于在启动时基于环境配置应用。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-381">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="fd7eb-382">或者，将 `IHostingEnvironment` 注入到 `Startup` 构造函数用于 `ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-382">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> <span data-ttu-id="fd7eb-383">除了 `IsDevelopment` 扩展方法，`IHostingEnvironment` 提供 `IsStaging`、`IsProduction` 和 `IsEnvironment(string environmentName)` 方法。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-383">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="fd7eb-384">有关详细信息，请参阅[使用多个环境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-384">See [Use multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="fd7eb-385">`IHostingEnvironment` 服务还可以直接注入到 `Configure` 方法以设置处理管道：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-385">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

<span data-ttu-id="fd7eb-386">创建自定义[中间件](xref:fundamentals/middleware/index#writing-middleware)时可以将 `IHostingEnvironment` 注入 `Invoke` 方法：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-386">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="fd7eb-387">IApplicationLifetime 接口</span><span class="sxs-lookup"><span data-stu-id="fd7eb-387">IApplicationLifetime interface</span></span>

<span data-ttu-id="fd7eb-388">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) 允许后启动和关闭活动。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-388">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="fd7eb-389">接口上的三个属性是用于注册 `Action` 方法（用于定义启动和关闭事件）的取消标记。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-389">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="fd7eb-390">取消标记</span><span class="sxs-lookup"><span data-stu-id="fd7eb-390">Cancellation Token</span></span>    | <span data-ttu-id="fd7eb-391">触发条件</span><span class="sxs-lookup"><span data-stu-id="fd7eb-391">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="fd7eb-392">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="fd7eb-392">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="fd7eb-393">主机已完全启动。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-393">The host has fully started.</span></span> |
| [<span data-ttu-id="fd7eb-394">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="fd7eb-394">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="fd7eb-395">主机正在完成正常关闭。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-395">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="fd7eb-396">应处理所有请求。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-396">All requests should be processed.</span></span> <span data-ttu-id="fd7eb-397">关闭受到阻止，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-397">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="fd7eb-398">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="fd7eb-398">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="fd7eb-399">主机正在执行正常关闭。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-399">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="fd7eb-400">仍在处理请求。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-400">Requests may still be processing.</span></span> <span data-ttu-id="fd7eb-401">关闭受到阻止，直到完成此事件。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-401">Shutdown blocks until this event completes.</span></span> |

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime)
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

<span data-ttu-id="fd7eb-402">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) 请求应用终止。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-402">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="fd7eb-403">以下类在调用类的 `Shutdown` 方法时使用 `StopApplication` 正常关闭应用：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-403">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="scope-validation"></a><span data-ttu-id="fd7eb-404">作用域验证</span><span class="sxs-lookup"><span data-stu-id="fd7eb-404">Scope validation</span></span>

<span data-ttu-id="fd7eb-405">在 ASP.NET Core 2.0 或更高版本中，如果应用环境为“开发”，则 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 将 [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) 设为 `true`。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-405">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="fd7eb-406">若将 `ValidateScopes` 设为 `true`，默认服务提供程序会执行检查来验证以下内容：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-406">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="fd7eb-407">没有从根服务提供程序直接或间接解析到有作用域的服务。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-407">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="fd7eb-408">未将有作用域的服务直接或间接注入到单一实例。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-408">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="fd7eb-409">调用 [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) 时，会创建根服务提供程序。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-409">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="fd7eb-410">在启动提供程序和应用时，根服务提供程序的生存期对应于应用/服务的生存期，并在关闭应用时释放。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-410">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="fd7eb-411">有作用域的服务由创建它们的容器释放。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-411">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="fd7eb-412">如果作用域创建于根容器，则该服务的生存会有效地提升至单一实例，因为根容器只会在应用/服务关闭时将其释放。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-412">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="fd7eb-413">验证服务作用域，将在调用 `BuildServiceProvider` 时收集这类情况。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-413">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="fd7eb-414">若要始终验证作用域（包括在生存环境中验证），请使用主机生成器上的 [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) 配置 [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions)：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-414">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="fd7eb-415">疑难解答：System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="fd7eb-415">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="fd7eb-416">**仅适用于 ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="fd7eb-416">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="fd7eb-417">主机可能通过将 `IStartup` 直接注入依赖关系注入容器生成，而不是调用 `UseStartup` 或 `Configure`：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-417">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="fd7eb-418">如果主机以这种方式生成，可能会发生以下错误：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-418">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="fd7eb-419">这是因为 [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey)（当前程序集）需扫描 `HostingStartupAttributes`。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-419">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="fd7eb-420">如果应用手动将 `IStartup` 注入到依赖关系注入容器中，将以下调用添加到具有指定程序集名称的 `WebHostBuilder`：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-420">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="fd7eb-421">或者，将虚拟 `Configure` 添加到自动设置 `applicationName` (`ApplicationKey`) 的 `WebHostBuilder` 中：</span><span class="sxs-lookup"><span data-stu-id="fd7eb-421">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="fd7eb-422">**注意**：仅当使用 ASP.NET Core 2.0 发行版且应用不调用 `UseStartup` 或 `Configure` 时，需要执行此操作。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-422">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="fd7eb-423">有关详细信息，请参阅[公告：已删除 Microsoft.Extensions.PlatformAbstractions（注释）](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938)和 [StartupInjection 示例](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)。</span><span class="sxs-lookup"><span data-stu-id="fd7eb-423">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fd7eb-424">其他资源</span><span class="sxs-lookup"><span data-stu-id="fd7eb-424">Additional resources</span></span>

* [<span data-ttu-id="fd7eb-425">使用 IIS 在 Windows 上进行托管</span><span class="sxs-lookup"><span data-stu-id="fd7eb-425">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="fd7eb-426">在 Linux 上使用 Nginx 进行托管</span><span class="sxs-lookup"><span data-stu-id="fd7eb-426">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="fd7eb-427">在 Linux 上使用 Apache 进行托管</span><span class="sxs-lookup"><span data-stu-id="fd7eb-427">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="fd7eb-428">在 Windows 服务中进行托管</span><span class="sxs-lookup"><span data-stu-id="fd7eb-428">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
