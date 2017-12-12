---
title: "在 ASP.NET Core 中的应用程序启动"
author: ardalis
description: "发现服务和应用程序的请求管道 Startup 类在 ASP.NET 核心中的配置的方式。"
keywords: "ASP.NET 核心，启动时，配置方法，ConfigureServices 方法"
ms.author: tdykstra
manager: wpickett
ms.date: 02/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 83b2647df8beec1feae33400224dacf9823be9b4
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2017
---
# <a name="application-startup-in-aspnet-core"></a><span data-ttu-id="88542-104">在 ASP.NET Core 中的应用程序启动</span><span class="sxs-lookup"><span data-stu-id="88542-104">Application Startup in ASP.NET Core</span></span>

<span data-ttu-id="88542-105">通过[Steve Smith](https://ardalis.com/)和[Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="88542-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="88542-106">`Startup`类配置服务和应用程序的请求管道。</span><span class="sxs-lookup"><span data-stu-id="88542-106">The `Startup` class configures services and the application's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="88542-107">Startup 类</span><span class="sxs-lookup"><span data-stu-id="88542-107">The Startup class</span></span>

<span data-ttu-id="88542-108">ASP.NET Core 应用需要`Startup`类，该类名为`Startup`按照约定。</span><span class="sxs-lookup"><span data-stu-id="88542-108">ASP.NET Core apps require a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="88542-109">指定的启动类名`Main`程序的[WebHostBuilderExtensions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions) [ `UseStartup<TStartup>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)方法。</span><span class="sxs-lookup"><span data-stu-id="88542-109">You specify the startup class name in the `Main` program's [WebHostBuilderExtensions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions) [`UseStartup<TStartup>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) method.</span></span> <span data-ttu-id="88542-110">请参阅[宿主](xref:fundamentals/hosting)以详细了解`WebHostBuilder`，运行之前`Startup`。</span><span class="sxs-lookup"><span data-stu-id="88542-110">See [Hosting](xref:fundamentals/hosting) to learn more about `WebHostBuilder`, which runs before `Startup`.</span></span>

<span data-ttu-id="88542-111">你可以定义单独`Startup`不同环境中，和相应的将会选择一个在运行时类。</span><span class="sxs-lookup"><span data-stu-id="88542-111">You can define separate `Startup` classes for different environments, and the appropriate one will be selected at runtime.</span></span> <span data-ttu-id="88542-112">如果指定`startupAssembly`中[WebHost 配置](https://docs.microsoft.com/aspnet/core/fundamentals/hosting?tabs=aspnetcore2x#configuring-a-host)或选项，承载将加载该程序集的启动和搜索`Startup`或`Startup[Environment]`类型。</span><span class="sxs-lookup"><span data-stu-id="88542-112">If you specify `startupAssembly` in the [WebHost configuration](https://docs.microsoft.com/aspnet/core/fundamentals/hosting?tabs=aspnetcore2x#configuring-a-host) or options, hosting will load that startup assembly and search for a `Startup` or `Startup[Environment]` type.</span></span> <span data-ttu-id="88542-113">类中运行该应用程序的当前环境将按优先级排列其名称后缀匹配项，因此，如果*开发*环境，并同时包含`Startup`和`StartupDevelopment`类，`StartupDevelopment`类将为使用。</span><span class="sxs-lookup"><span data-stu-id="88542-113">The class whose name suffix matches the current environment will be prioritized, so if the app is run in the *Development* environment, and includes both a `Startup` and a `StartupDevelopment` class, the `StartupDevelopment` class will be used.</span></span> <span data-ttu-id="88542-114">请参阅[FindStartupType](https://github.com/aspnet/Hosting/blob/rel/1.1.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs)中`StartupLoader`和[使用多个环境](environments.md#startup-conventions)。</span><span class="sxs-lookup"><span data-stu-id="88542-114">See [FindStartupType](https://github.com/aspnet/Hosting/blob/rel/1.1.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs) in `StartupLoader` and [Working with multiple environments](environments.md#startup-conventions).</span></span>

<span data-ttu-id="88542-115">或者，你可以定义固定`Startup`将通过调用而不考虑环境中使用的类`UseStartup<TStartup>`。</span><span class="sxs-lookup"><span data-stu-id="88542-115">Alternatively, you can define a fixed `Startup` class that will be used regardless of the environment by calling `UseStartup<TStartup>`.</span></span> <span data-ttu-id="88542-116">此为推荐方法。</span><span class="sxs-lookup"><span data-stu-id="88542-116">This is the recommended approach.</span></span>

<span data-ttu-id="88542-117">`Startup`类构造函数可以接受的依赖项，通过提供[依赖关系注入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="88542-117">The `Startup` class constructor can accept dependencies that are provided through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="88542-118">常见方法是使用`IHostingEnvironment`设置[配置](xref:fundamentals/configuration/index)源。</span><span class="sxs-lookup"><span data-stu-id="88542-118">A common approach is to use `IHostingEnvironment` to set up [configuration](xref:fundamentals/configuration/index) sources.</span></span>

<span data-ttu-id="88542-119">`Startup`类必须包含`Configure`方法并且可以根据需要包含`ConfigureServices`方法，这两种应用程序启动时调用。</span><span class="sxs-lookup"><span data-stu-id="88542-119">The `Startup` class must include a `Configure` method and can optionally include a `ConfigureServices` method, both of which are called when the application starts.</span></span> <span data-ttu-id="88542-120">类还可包含[这些方法的特定于环境的版本](xref:fundamentals/environments#startup-conventions)。</span><span class="sxs-lookup"><span data-stu-id="88542-120">The class can also include [environment-specific versions of these methods](xref:fundamentals/environments#startup-conventions).</span></span> <span data-ttu-id="88542-121">`ConfigureServices`如果存在之前, 调用`Configure`。</span><span class="sxs-lookup"><span data-stu-id="88542-121">`ConfigureServices`, if present, is called before `Configure`.</span></span>

<span data-ttu-id="88542-122">了解有关[应用程序启动期间处理的异常](xref:fundamentals/error-handling#startup-exception-handling)。</span><span class="sxs-lookup"><span data-stu-id="88542-122">Learn about [handling exceptions during application startup](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="88542-123">ConfigureServices 方法</span><span class="sxs-lookup"><span data-stu-id="88542-123">The ConfigureServices method</span></span>

<span data-ttu-id="88542-124">[ConfigureServices](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.startupbase#Microsoft_AspNetCore_Hosting_StartupBase_ConfigureServices_Microsoft_Extensions_DependencyInjection_IServiceCollection_)方法是可选的; 但如果使用，则将不调用之前`Configure`web 主机的方法。</span><span class="sxs-lookup"><span data-stu-id="88542-124">The [ConfigureServices](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.startupbase#Microsoft_AspNetCore_Hosting_StartupBase_ConfigureServices_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method is optional; but if used, it's called before the `Configure` method by the web host.</span></span> <span data-ttu-id="88542-125">Web 主机可以配置某些服务之前``Startup``调用方法 (请参阅[承载](xref:fundamentals/hosting))。</span><span class="sxs-lookup"><span data-stu-id="88542-125">The web host may configure some services before ``Startup`` methods are called (see [hosting](xref:fundamentals/hosting)).</span></span> <span data-ttu-id="88542-126">按照约定，[配置选项](xref:fundamentals/configuration/index)在此方法中设置。</span><span class="sxs-lookup"><span data-stu-id="88542-126">By convention, [Configuration options](xref:fundamentals/configuration/index) are set in this method.</span></span>

<span data-ttu-id="88542-127">对于需要大量的安装程序的功能有`Add[Service]`上的扩展方法[IServiceCollection](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.iservicecollection)。</span><span class="sxs-lookup"><span data-stu-id="88542-127">For features that require substantial setup there are `Add[Service]` extension methods on [IServiceCollection](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.iservicecollection).</span></span> <span data-ttu-id="88542-128">此示例摘自默认网站模板配置应用程序，以将服务用于实体框架、 标识和 MVC:</span><span class="sxs-lookup"><span data-stu-id="88542-128">This example from the default web site template configures the app to use services for Entity Framework, Identity, and MVC:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

<span data-ttu-id="88542-129">将服务添加到服务容器使它们可通过应用程序中[依赖关系注入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="88542-129">Adding services to the services container makes them available within your application via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="services-available-in-startup"></a><span data-ttu-id="88542-130">在启动中可用的服务</span><span class="sxs-lookup"><span data-stu-id="88542-130">Services Available in Startup</span></span>

<span data-ttu-id="88542-131">ASP.NET 核心依赖关系注入应用程序的启动期间提供服务。</span><span class="sxs-lookup"><span data-stu-id="88542-131">ASP.NET Core dependency injection provides services during an application's startup.</span></span> <span data-ttu-id="88542-132">你可以通过作为参数包括适当的接口中请求这些服务你`Startup`类的构造函数或其`Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="88542-132">You can request these services by including the appropriate interface as a parameter on your `Startup` class's constructor or its `Configure` method.</span></span> <span data-ttu-id="88542-133">`ConfigureServices`方法仅采用`IServiceCollection`参数 （但此集合中检索，因此其他参数，则不需要任何已注册的服务可以）。</span><span class="sxs-lookup"><span data-stu-id="88542-133">The `ConfigureServices` method only takes an `IServiceCollection` parameter (but any registered service can be retrieved from this collection, so additional parameters are not necessary).</span></span>

<span data-ttu-id="88542-134">以下是一些通常由请求的服务`Startup`方法：</span><span class="sxs-lookup"><span data-stu-id="88542-134">Below are some of the services typically requested by `Startup` methods:</span></span>

* <span data-ttu-id="88542-135">构造函数中： `IHostingEnvironment`，`ILogger<Startup>`</span><span class="sxs-lookup"><span data-stu-id="88542-135">In the constructor:  `IHostingEnvironment`, `ILogger<Startup>`</span></span>
* <span data-ttu-id="88542-136">在`ConfigureServices`:`IServiceCollection`</span><span class="sxs-lookup"><span data-stu-id="88542-136">In `ConfigureServices`:  `IServiceCollection`</span></span>
* <span data-ttu-id="88542-137">在`Configure`: `IApplicationBuilder`， `IHostingEnvironment`，`ILoggerFactory`</span><span class="sxs-lookup"><span data-stu-id="88542-137">In `Configure`:  `IApplicationBuilder`, `IHostingEnvironment`, `ILoggerFactory`</span></span>

<span data-ttu-id="88542-138">通过添加的任何服务``WebHostBuilder````ConfigureServices``方法可请求的``Startup``类构造函数或其``Configure``方法。</span><span class="sxs-lookup"><span data-stu-id="88542-138">Any services added by the ``WebHostBuilder`` ``ConfigureServices`` method may be requested by the ``Startup`` class constructor or its ``Configure`` method.</span></span> <span data-ttu-id="88542-139">使用`WebHostBuilder`若要为任何服务你需要在`Startup`方法。</span><span class="sxs-lookup"><span data-stu-id="88542-139">Use `WebHostBuilder` to provide any services you need during `Startup` methods.</span></span>

## <a name="the-configure-method"></a><span data-ttu-id="88542-140">Configure 方法</span><span class="sxs-lookup"><span data-stu-id="88542-140">The Configure method</span></span>

<span data-ttu-id="88542-141">`Configure`方法用于指定 ASP.NET 应用程序将如何响应 HTTP 请求。</span><span class="sxs-lookup"><span data-stu-id="88542-141">The `Configure` method is used to specify how the ASP.NET application will respond to HTTP requests.</span></span> <span data-ttu-id="88542-142">通过添加配置请求管道[中间件](middleware.md)组件`IApplicationBuilder`提供的依赖关系注入的实例。</span><span class="sxs-lookup"><span data-stu-id="88542-142">The request pipeline is configured by adding [middleware](middleware.md) components to an `IApplicationBuilder` instance that is provided by dependency injection.</span></span>

<span data-ttu-id="88542-143">在以下示例从默认网站模板中，多个扩展方法用于配置支持的管道[浏览器链接](http://vswebessentials.com/features/browserlink)，错误页、 静态文件、 ASP.NET MVC 和标识。</span><span class="sxs-lookup"><span data-stu-id="88542-143">In the following example from the default web site template, several extension methods are used to configure the pipeline with support for [BrowserLink](http://vswebessentials.com/features/browserlink), error pages, static files, ASP.NET MVC, and Identity.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

<span data-ttu-id="88542-144">每个`Use`扩展方法将添加[中间件](xref:fundamentals/middleware)请求管道组件。</span><span class="sxs-lookup"><span data-stu-id="88542-144">Each `Use` extension method adds a [middleware](xref:fundamentals/middleware) component to the request pipeline.</span></span> <span data-ttu-id="88542-145">例如，`UseMvc`扩展方法将添加[路由](routing.md)向请求管道的中间件并配置[MVC](xref:mvc/overview)作为默认处理程序。</span><span class="sxs-lookup"><span data-stu-id="88542-145">For instance, the `UseMvc` extension method adds the [routing](routing.md) middleware to the request pipeline and configures [MVC](xref:mvc/overview) as the default handler.</span></span>

<span data-ttu-id="88542-146">有关如何使用`IApplicationBuilder`，请参阅[中间件](xref:fundamentals/middleware)。</span><span class="sxs-lookup"><span data-stu-id="88542-146">For more information about how to use `IApplicationBuilder`, see [Middleware](xref:fundamentals/middleware).</span></span>

<span data-ttu-id="88542-147">等其他服务，`IHostingEnvironment`和`ILoggerFactory`还可以指定在方法签名中，在这种情况下这些服务将[注入](dependency-injection.md)它们是否可用。</span><span class="sxs-lookup"><span data-stu-id="88542-147">Additional services, like `IHostingEnvironment` and `ILoggerFactory` may also be specified in the method signature, in which case these services will be [injected](dependency-injection.md) if they are available.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="88542-148">其他资源</span><span class="sxs-lookup"><span data-stu-id="88542-148">Additional Resources</span></span>

* [<span data-ttu-id="88542-149">使用多个环境</span><span class="sxs-lookup"><span data-stu-id="88542-149">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="88542-150">中间件</span><span class="sxs-lookup"><span data-stu-id="88542-150">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="88542-151">日志记录</span><span class="sxs-lookup"><span data-stu-id="88542-151">Logging</span></span>](xref:fundamentals/logging/index)
* [<span data-ttu-id="88542-152">配置</span><span class="sxs-lookup"><span data-stu-id="88542-152">Configuration</span></span>](xref:fundamentals/configuration/index)
