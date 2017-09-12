---
title: "ASP.NET Core 基础知识"
author: rick-anderson
description: "本文提供了有关构建 ASP.NET Core 应用程序时需要了解的基础概念的高级概述。"
keywords: "ASP.NET Core, 基础知识, 概述"
ms.author: riande
manager: wpickett
ms.date: 08/18/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5d8ca35b0e2e4b6e9b1ec745a3a7cf7c3983c461
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2017
---
# <a name="aspnet-core-fundamentals-overview"></a><span data-ttu-id="d2861-104">ASP.NET Core 基础知识概述</span><span class="sxs-lookup"><span data-stu-id="d2861-104">ASP.NET Core fundamentals overview</span></span>

<span data-ttu-id="d2861-105">ASP.NET Core 应用程序是在其 `Main` 方法中创建 Web 服务器的控制台应用：</span><span class="sxs-lookup"><span data-stu-id="d2861-105">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d2861-106">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d2861-106">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d2861-107">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]</span><span class="sxs-lookup"><span data-stu-id="d2861-107">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]</span></span>

<span data-ttu-id="d2861-108">`Main` 方法调用 `WebHost.CreateDefaultBuilder`，后者按照生成器模式来创建 Web 应用程序主机。</span><span class="sxs-lookup"><span data-stu-id="d2861-108">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="d2861-109">生成器提供定义 Web 服务器（例如，`UseKestrel`）和启动类 (`UseStartup`) 的方法。</span><span class="sxs-lookup"><span data-stu-id="d2861-109">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="d2861-110">在前面的例子中，自动分配了 [Kestrel](xref:fundamentals/servers/kestrel) Web 服务器。</span><span class="sxs-lookup"><span data-stu-id="d2861-110">In the preceding example, a [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="d2861-111">ASP.NET Core 的 Web 主机将尝试在 IIS 上运行（如果可用）。</span><span class="sxs-lookup"><span data-stu-id="d2861-111">ASP.NET Core's web host will attempt to run on IIS, if it is available.</span></span> <span data-ttu-id="d2861-112">对于其他 Web 服务器（如 [HTTP.sys](xref:fundamentals/servers/httpsys)），可通过调用相应的扩展方法来使用。</span><span class="sxs-lookup"><span data-stu-id="d2861-112">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="d2861-113">在下一节对 `UseStartup` 进行了更深入的介绍。</span><span class="sxs-lookup"><span data-stu-id="d2861-113">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="d2861-114">`IWebHostBuilder` 是 `WebHost.CreateDefaultBuilder` 调用的返回类型，它提供了许多可选方法。</span><span class="sxs-lookup"><span data-stu-id="d2861-114">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="d2861-115">其中的一些方法包括用于在 HTTP.sys 中托管应用程序的 `UseHttpSys`，以及用于指定根内容目录的 `UseContentRoot`。</span><span class="sxs-lookup"><span data-stu-id="d2861-115">Some of these methods include `UseHttpSys` for hosting the application in HTTP.sys, and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="d2861-116">`Build` 和 `Run` 方法生成 `IWebHost` 对象，它将托管应用程序并开始侦听 HTTP 请求。</span><span class="sxs-lookup"><span data-stu-id="d2861-116">The `Build` and `Run` methods build the `IWebHost` object that will host the application and begin listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d2861-117">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d2861-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d2861-118">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="d2861-118">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]</span></span>

<span data-ttu-id="d2861-119">`Main` 方法使用 `WebHostBuilder`，后者按照生成器模式来创建 Web 应用程序主机。</span><span class="sxs-lookup"><span data-stu-id="d2861-119">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="d2861-120">生成器提供定义 Web 服务器（例如，`UseKestrel`）和启动类 (`UseStartup`) 的方法。</span><span class="sxs-lookup"><span data-stu-id="d2861-120">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="d2861-121">在前面的示例中，使用了 [Kestrel](xref:fundamentals/servers/kestrel) Web 服务器。</span><span class="sxs-lookup"><span data-stu-id="d2861-121">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="d2861-122">对于其他 Web 服务器（如 [WebListener](xref:fundamentals/servers/weblistener)），可通过调用相应的扩展方法来使用。</span><span class="sxs-lookup"><span data-stu-id="d2861-122">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="d2861-123">在下一节对 `UseStartup` 进行了更深入的介绍。</span><span class="sxs-lookup"><span data-stu-id="d2861-123">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="d2861-124">`WebHostBuilder` 提供了许多可选方法，其中包括用于在 IIS 和 IIS Express 中进行托管的 `UseIISIntegration`，以及用于指定根内容目录的 `UseContentRoot`。</span><span class="sxs-lookup"><span data-stu-id="d2861-124">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express, and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="d2861-125">`Build` 和 `Run` 方法生成 `IWebHost` 对象，它将托管应用程序并开始侦听 HTTP 请求。</span><span class="sxs-lookup"><span data-stu-id="d2861-125">The `Build` and `Run` methods build the `IWebHost` object that will host the application and begin listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="d2861-126">启动</span><span class="sxs-lookup"><span data-stu-id="d2861-126">Startup</span></span>

<span data-ttu-id="d2861-127">`WebHostBuilder` 上的 `UseStartup` 方法为你的应用指定 `Startup` 类：</span><span class="sxs-lookup"><span data-stu-id="d2861-127">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d2861-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d2861-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d2861-129">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="d2861-129">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d2861-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d2861-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d2861-131">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="d2861-131">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]</span></span>

---

<span data-ttu-id="d2861-132">`Startup` 类用于定义请求处理管道和配置应用程序所需的任何服务。</span><span class="sxs-lookup"><span data-stu-id="d2861-132">The `Startup` class is where you define the request handling pipeline and where any services needed by the application are configured.</span></span> <span data-ttu-id="d2861-133">`Startup` 必须是公共类，并包含以下方法：</span><span class="sxs-lookup"><span data-stu-id="d2861-133">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

* <span data-ttu-id="d2861-134">`ConfigureServices` 定义应用程序所使用的[服务](#services)（如 ASP.NET Core MVC、Entity Framework Core、标识等）。</span><span class="sxs-lookup"><span data-stu-id="d2861-134">`ConfigureServices` defines the [Services](#services) used by your application (such as ASP.NET Core MVC, Entity Framework Core, Identity, etc.).</span></span>

* <span data-ttu-id="d2861-135">`Configure` 定义请求管道中的[中间件](xref:fundamentals/middleware)。</span><span class="sxs-lookup"><span data-stu-id="d2861-135">`Configure` defines the [middleware](xref:fundamentals/middleware) in the request pipeline.</span></span>

<span data-ttu-id="d2861-136">有关详细信息，请参阅[应用程序启动](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="d2861-136">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="services"></a><span data-ttu-id="d2861-137">服务</span><span class="sxs-lookup"><span data-stu-id="d2861-137">Services</span></span>

<span data-ttu-id="d2861-138">服务是应用程序中常用的组件。</span><span class="sxs-lookup"><span data-stu-id="d2861-138">A service is a component that is intended for common consumption in an application.</span></span> <span data-ttu-id="d2861-139">可以通过[依存关系注入](xref:fundamentals/dependency-injection) (DI) 来获取服务。</span><span class="sxs-lookup"><span data-stu-id="d2861-139">Services are made available through [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="d2861-140">ASP.NET Core 包括默认支持[构造函数注入](xref:mvc/controllers/dependency-injection#constructor-injection)的本机控制反转 (IoC) 容器。</span><span class="sxs-lookup"><span data-stu-id="d2861-140">ASP.NET Core includes a native inversion of control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="d2861-141">可将本机容器替换成你所选择的容器。</span><span class="sxs-lookup"><span data-stu-id="d2861-141">The native container can be replaced with your container of choice.</span></span> <span data-ttu-id="d2861-142">DI 除了具备松散耦合优势以外，还可以使你在应用程序中使用服务。</span><span class="sxs-lookup"><span data-stu-id="d2861-142">In addition to its loose coupling benefit, DI makes services available throughout your application.</span></span> <span data-ttu-id="d2861-143">例如，可以在应用程序中使用[日志记录](xref:fundamentals/logging)。</span><span class="sxs-lookup"><span data-stu-id="d2861-143">For example, [logging](xref:fundamentals/logging) is available throughout your application.</span></span>

<span data-ttu-id="d2861-144">有关详细信息，请参阅[依存关系注入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="d2861-144">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="d2861-145">中间件</span><span class="sxs-lookup"><span data-stu-id="d2861-145">Middleware</span></span>

<span data-ttu-id="d2861-146">在 ASP.NET Core 中，使用[中间件](xref:fundamentals/middleware)来撰写请求管道。</span><span class="sxs-lookup"><span data-stu-id="d2861-146">In ASP.NET Core, you compose your request pipeline using [Middleware](xref:fundamentals/middleware).</span></span> <span data-ttu-id="d2861-147">ASP.NET Core 中间件在 `HttpContext` 上执行异步逻辑，然后调用序列中的下一个中间件或直接终止请求。</span><span class="sxs-lookup"><span data-stu-id="d2861-147">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="d2861-148">通过在 `Configure` 方法中调用 `UseXYZ` 扩展方法来添加名为“XYZ”的中间件组件。</span><span class="sxs-lookup"><span data-stu-id="d2861-148">A middleware component called "XYZ" is added by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="d2861-149">ASP.NET Core 包含一组丰富的内置中间件：</span><span class="sxs-lookup"><span data-stu-id="d2861-149">ASP.NET Core comes with a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="d2861-150">静态文件</span><span class="sxs-lookup"><span data-stu-id="d2861-150">Static files</span></span>](xref:fundamentals/static-files)

* [<span data-ttu-id="d2861-151">路由</span><span class="sxs-lookup"><span data-stu-id="d2861-151">Routing</span></span>](xref:fundamentals/routing)

* [<span data-ttu-id="d2861-152">身份验证</span><span class="sxs-lookup"><span data-stu-id="d2861-152">Authentication</span></span>](xref:security/authentication/index)

<span data-ttu-id="d2861-153">可以将任何基于 [OWIN](http://owin.org) 的中间件与 ASP.NET Core 结合使用，也可以编写自己的自定义中间件。</span><span class="sxs-lookup"><span data-stu-id="d2861-153">You can use any [OWIN](http://owin.org)-based middleware with ASP.NET Core, and you can write your own custom middleware.</span></span>

<span data-ttu-id="d2861-154">有关详细信息，请参阅[中间件](xref:fundamentals/middleware)和 [.NET 的开放 Web 接口 (OWIN)](xref:fundamentals/owin)。</span><span class="sxs-lookup"><span data-stu-id="d2861-154">For more information, see [Middleware](xref:fundamentals/middleware) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="servers"></a><span data-ttu-id="d2861-155">服务器</span><span class="sxs-lookup"><span data-stu-id="d2861-155">Servers</span></span>

<span data-ttu-id="d2861-156">ASP.NET Core 托管模型并不直接侦听请求，而是依赖于 HTTP 服务器实现来将请求转发到应用程序。</span><span class="sxs-lookup"><span data-stu-id="d2861-156">The ASP.NET Core hosting model does not directly listen for requests; rather, it relies on an HTTP server implementation to forward the request to the application.</span></span> <span data-ttu-id="d2861-157">转发的请求被打包为一组可通过接口进行访问的功能对象。</span><span class="sxs-lookup"><span data-stu-id="d2861-157">The forwarded request is wrapped as a set of feature objects that you can access through interfaces.</span></span> <span data-ttu-id="d2861-158">应用程序将其撰写到 `HttpContext` 中。</span><span class="sxs-lookup"><span data-stu-id="d2861-158">The application composes this set into an `HttpContext`.</span></span> <span data-ttu-id="d2861-159">ASP.NET Core 包含托管的跨平台 Web 服务器，名为 [Kestrel](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="d2861-159">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="d2861-160">Kestrel 通常在生产 Web 服务器（如 [IIS](https://www.iis.net/) 或 [nginx](http://nginx.org)）后台运行。</span><span class="sxs-lookup"><span data-stu-id="d2861-160">Kestrel is typically run behind a production web server like [IIS](https://www.iis.net/) or [nginx](http://nginx.org).</span></span>

<span data-ttu-id="d2861-161">有关详细信息，请参阅[服务器](xref:fundamentals/servers/index)和[托管](xref:fundamentals/hosting)。</span><span class="sxs-lookup"><span data-stu-id="d2861-161">For more information, see [Servers](xref:fundamentals/servers/index) and [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="content-root"></a><span data-ttu-id="d2861-162">内容根</span><span class="sxs-lookup"><span data-stu-id="d2861-162">Content root</span></span>

<span data-ttu-id="d2861-163">内容根是应用所使用的任何内容的基路径，如视图、[Razor 页面](xref:mvc/razor-pages/index) 和静态资产。</span><span class="sxs-lookup"><span data-stu-id="d2861-163">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="d2861-164">默认情况下，内容根与用于托管应用程序的可执行文件的应用程序基路径相同。</span><span class="sxs-lookup"><span data-stu-id="d2861-164">By default, the content root is the same as application base path for the executable hosting the application.</span></span> <span data-ttu-id="d2861-165">内容根的其他位置由 `WebHostBuilder` 指定。</span><span class="sxs-lookup"><span data-stu-id="d2861-165">An alternative location for content root is specified with `WebHostBuilder`.</span></span>

## <a name="web-root"></a><span data-ttu-id="d2861-166">Web 根</span><span class="sxs-lookup"><span data-stu-id="d2861-166">Web root</span></span>

<span data-ttu-id="d2861-167">应用程序的 Web 根是项目中的目录，其中包含公共资源、CSS 等静态资源、JavaScript 和图形文件。</span><span class="sxs-lookup"><span data-stu-id="d2861-167">The web root of an application is the directory in the project containing public, static resources like CSS, JavaScript, and image files.</span></span> <span data-ttu-id="d2861-168">默认情况下，静态文件中间件仅提供 Web 根目录及其子目录中的文件。</span><span class="sxs-lookup"><span data-stu-id="d2861-168">By default, the static files middleware will only serve files from the web root directory and its sub-directories.</span></span> <span data-ttu-id="d2861-169">有关详细信息，请参阅[使用静态文件](xref:fundamentals/static-files)。</span><span class="sxs-lookup"><span data-stu-id="d2861-169">See [working with static files](xref:fundamentals/static-files) for more info.</span></span> <span data-ttu-id="d2861-170">Web 根路径默认为 /wwwroot，但可以使用 `WebHostBuilder` 来指定其他位置。</span><span class="sxs-lookup"><span data-stu-id="d2861-170">The web root path defaults to */wwwroot*, but you can specify a different location using the `WebHostBuilder`.</span></span>

## <a name="configuration"></a><span data-ttu-id="d2861-171">配置</span><span class="sxs-lookup"><span data-stu-id="d2861-171">Configuration</span></span>

<span data-ttu-id="d2861-172">ASP.NET Core 使用新的配置模型来处理简单的名称/值对。</span><span class="sxs-lookup"><span data-stu-id="d2861-172">ASP.NET Core uses a new configuration model for handling simple name-value pairs.</span></span> <span data-ttu-id="d2861-173">新的配置模型不基于 `System.Configuration` 或 web.config，相反，它是从一组有序的配置提供程序中拉取的。</span><span class="sxs-lookup"><span data-stu-id="d2861-173">The new configuration model is not based on `System.Configuration` or *web.config*; rather, it pulls from an ordered set of configuration providers.</span></span> <span data-ttu-id="d2861-174">内置配置提供程序支持各种文件格式（XML、 JSON、INI）和环境变量，从而实现基于环境的配置。</span><span class="sxs-lookup"><span data-stu-id="d2861-174">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="d2861-175">也可以编写你自己的自定义配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="d2861-175">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="d2861-176">有关详细信息，请参阅[配置](xref:fundamentals/configuration)。</span><span class="sxs-lookup"><span data-stu-id="d2861-176">For more information, see [Configuration](xref:fundamentals/configuration).</span></span>

## <a name="environments"></a><span data-ttu-id="d2861-177">环境</span><span class="sxs-lookup"><span data-stu-id="d2861-177">Environments</span></span>

<span data-ttu-id="d2861-178">环境（如“开发”环境和“生产”环境）是 ASP.NET Core 的高级概念，可使用环境变量进行设置。</span><span class="sxs-lookup"><span data-stu-id="d2861-178">Environments, like "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="d2861-179">有关详细信息，请参阅[使用多个环境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="d2861-179">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="d2861-180">.NET Core 与 .NET Framework 运行时</span><span class="sxs-lookup"><span data-stu-id="d2861-180">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="d2861-181">ASP.NET Core 应用程序可以面向 .NET Core 或 .NET Framework 运行时。</span><span class="sxs-lookup"><span data-stu-id="d2861-181">An ASP.NET Core application can target the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="d2861-182">有关详细信息，请参阅[在 .NET Core 和 .NET Framework 之间进行选择](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)。</span><span class="sxs-lookup"><span data-stu-id="d2861-182">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="additional-information"></a><span data-ttu-id="d2861-183">其他信息</span><span class="sxs-lookup"><span data-stu-id="d2861-183">Additional information</span></span>

<span data-ttu-id="d2861-184">另请参阅以下主题：</span><span class="sxs-lookup"><span data-stu-id="d2861-184">See also the following topics:</span></span>

- [<span data-ttu-id="d2861-185">错误处理</span><span class="sxs-lookup"><span data-stu-id="d2861-185">Error Handling</span></span>](xref:fundamentals/error-handling)
- [<span data-ttu-id="d2861-186">文件提供程序</span><span class="sxs-lookup"><span data-stu-id="d2861-186">File Providers</span></span>](xref:fundamentals/file-providers)
- [<span data-ttu-id="d2861-187">全球化和本地化</span><span class="sxs-lookup"><span data-stu-id="d2861-187">Globalization and localization</span></span>](xref:fundamentals/localization)
- [<span data-ttu-id="d2861-188">日志记录</span><span class="sxs-lookup"><span data-stu-id="d2861-188">Logging</span></span>](xref:fundamentals/logging)
- [<span data-ttu-id="d2861-189">管理应用程序状态</span><span class="sxs-lookup"><span data-stu-id="d2861-189">Managing Application State</span></span>](xref:fundamentals/app-state)
