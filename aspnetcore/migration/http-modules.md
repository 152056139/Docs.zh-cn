---
title: "迁移的 HTTP 处理程序和 ASP.NET Core 中间件的模块"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: tdykstra
manager: wpickett
ms.date: 12/07/2016
ms.topic: article
ms.assetid: 9c826a76-fbd2-46b5-978d-6ca6df53531a
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/http-modules
ms.openlocfilehash: f217e5264742826f285444dcbaea4b28b97c4d7e
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2017
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="f9b80-103">迁移的 HTTP 处理程序和 ASP.NET Core 中间件的模块</span><span class="sxs-lookup"><span data-stu-id="f9b80-103">Migrating HTTP handlers and modules to ASP.NET Core middleware</span></span> 

<span data-ttu-id="f9b80-104">通过[Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span><span class="sxs-lookup"><span data-stu-id="f9b80-104">By [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span></span>

<span data-ttu-id="f9b80-105">这篇文章演示如何迁移现有的 ASP.NET [HTTP 模块和处理程序 system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/)到 ASP.NET 核心[中间件](../fundamentals/middleware.md)。</span><span class="sxs-lookup"><span data-stu-id="f9b80-105">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/) to ASP.NET Core [middleware](../fundamentals/middleware.md).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="f9b80-106">模块和处理程序重新访问</span><span class="sxs-lookup"><span data-stu-id="f9b80-106">Modules and handlers revisited</span></span>

<span data-ttu-id="f9b80-107">在继续之前到 ASP.NET 核心中间件，让我们首先会扼要重述 HTTP 模块和处理程序的工作原理：</span><span class="sxs-lookup"><span data-stu-id="f9b80-107">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![模块处理程序](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="f9b80-109">**处理程序：**</span><span class="sxs-lookup"><span data-stu-id="f9b80-109">**Handlers are:**</span></span>

   * <span data-ttu-id="f9b80-110">类实现[IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span><span class="sxs-lookup"><span data-stu-id="f9b80-110">Classes that implement [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span></span>

   * <span data-ttu-id="f9b80-111">用于使用处理请求的给定的文件名或扩展，如*.report*</span><span class="sxs-lookup"><span data-stu-id="f9b80-111">Used to handle requests with a given file name or extension, such as *.report*</span></span>

   * <span data-ttu-id="f9b80-112">[配置](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/)中*Web.config*</span><span class="sxs-lookup"><span data-stu-id="f9b80-112">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="f9b80-113">**模块为：**</span><span class="sxs-lookup"><span data-stu-id="f9b80-113">**Modules are:**</span></span>

   * <span data-ttu-id="f9b80-114">类实现[IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span><span class="sxs-lookup"><span data-stu-id="f9b80-114">Classes that implement [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span></span>

   * <span data-ttu-id="f9b80-115">调用为每个请求</span><span class="sxs-lookup"><span data-stu-id="f9b80-115">Invoked for every request</span></span>

   * <span data-ttu-id="f9b80-116">能够短路 （停止进一步处理请求）</span><span class="sxs-lookup"><span data-stu-id="f9b80-116">Able to short-circuit (stop further processing of a request)</span></span>

   * <span data-ttu-id="f9b80-117">无法添加到 HTTP 响应中，或创建自己</span><span class="sxs-lookup"><span data-stu-id="f9b80-117">Able to add to the HTTP response, or create their own</span></span>

   * <span data-ttu-id="f9b80-118">[配置](https://docs.microsoft.com//iis/configuration/system.webserver/modules/)中*Web.config*</span><span class="sxs-lookup"><span data-stu-id="f9b80-118">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="f9b80-119">**模块顺序处理传入的请求的顺序取决于：**</span><span class="sxs-lookup"><span data-stu-id="f9b80-119">**The order in which modules process incoming requests is determined by:**</span></span>

   1. <span data-ttu-id="f9b80-120">[应用程序生命周期](https://msdn.microsoft.com/library/ms227673.aspx)，这是由 ASP.NET 激发的系列事件： [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest)， [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest)等。每个模块可以创建一个或多个事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="f9b80-120">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

   2. <span data-ttu-id="f9b80-121">对于相同的事件中，在中配置的顺序*Web.config*。</span><span class="sxs-lookup"><span data-stu-id="f9b80-121">For the same event, the order in which they are configured in *Web.config*.</span></span>

<span data-ttu-id="f9b80-122">除模块，还可以添加到生命周期事件的处理程序你*Global.asax.cs*文件。</span><span class="sxs-lookup"><span data-stu-id="f9b80-122">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="f9b80-123">在已配置的模块中的处理程序后运行这些处理程序。</span><span class="sxs-lookup"><span data-stu-id="f9b80-123">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="f9b80-124">从处理程序和到中间件模块</span><span class="sxs-lookup"><span data-stu-id="f9b80-124">From handlers and modules to middleware</span></span>

<span data-ttu-id="f9b80-125">**中间件是 HTTP 模块和处理程序比简单得多：**</span><span class="sxs-lookup"><span data-stu-id="f9b80-125">**Middleware are simpler than HTTP modules and handlers:**</span></span>

   * <span data-ttu-id="f9b80-126">模块、 处理程序， *Global.asax.cs*， *Web.config* （除外 IIS 配置） 和应用程序生命周期已消失</span><span class="sxs-lookup"><span data-stu-id="f9b80-126">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

   * <span data-ttu-id="f9b80-127">模块和处理程序的角色具有接管中间件</span><span class="sxs-lookup"><span data-stu-id="f9b80-127">The roles of both modules and handlers have been taken over by middleware</span></span>

   * <span data-ttu-id="f9b80-128">中间件配置使用代码而不是在*Web.config*</span><span class="sxs-lookup"><span data-stu-id="f9b80-128">Middleware are configured using code rather than in *Web.config*</span></span>

   * <span data-ttu-id="f9b80-129">[管道分支](../fundamentals/middleware.md#middleware-run-map-use)允许将请求发送到特定的中间件，基于不仅也上请求标头、 查询字符串等的 URL。</span><span class="sxs-lookup"><span data-stu-id="f9b80-129">[Pipeline branching](../fundamentals/middleware.md#middleware-run-map-use) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="f9b80-130">**中间件是非常类似于模块：**</span><span class="sxs-lookup"><span data-stu-id="f9b80-130">**Middleware are very similar to modules:**</span></span>

   * <span data-ttu-id="f9b80-131">在每个请求主体中调用</span><span class="sxs-lookup"><span data-stu-id="f9b80-131">Invoked in principle for every request</span></span>

   * <span data-ttu-id="f9b80-132">能够通过短路请求[不将请求传递到下一步的中间件](#http-modules-shortcircuiting-middleware)</span><span class="sxs-lookup"><span data-stu-id="f9b80-132">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

   * <span data-ttu-id="f9b80-133">能够创建他们自己的 HTTP 响应</span><span class="sxs-lookup"><span data-stu-id="f9b80-133">Able to create their own HTTP response</span></span>

<span data-ttu-id="f9b80-134">**中间件和模块按不同顺序处理：**</span><span class="sxs-lookup"><span data-stu-id="f9b80-134">**Middleware and modules are processed in a different order:**</span></span>

   * <span data-ttu-id="f9b80-135">中间件的顺序基于在其中插入到请求管道中，而模块的顺序主要基于的顺序[应用程序生命周期](https://msdn.microsoft.com/library/ms227673.aspx)事件</span><span class="sxs-lookup"><span data-stu-id="f9b80-135">Order of middleware is based on the order in which they are inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

   * <span data-ttu-id="f9b80-136">响应的中间件顺序是对于请求，与反向而模块的顺序是相同的请求和响应</span><span class="sxs-lookup"><span data-stu-id="f9b80-136">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

   * <span data-ttu-id="f9b80-137">请参阅[使用 IApplicationBuilder 创建中间件管道](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="f9b80-137">See [Creating a middleware pipeline with IApplicationBuilder](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![中间件](http-modules/_static/middleware.png)

<span data-ttu-id="f9b80-139">请注意如何在上图中，身份验证中间件 short-circuited 请求。</span><span class="sxs-lookup"><span data-stu-id="f9b80-139">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="f9b80-140">迁移到中间件模块代码</span><span class="sxs-lookup"><span data-stu-id="f9b80-140">Migrating module code to middleware</span></span>

<span data-ttu-id="f9b80-141">现有 HTTP 模块看起来类似于此：</span><span class="sxs-lookup"><span data-stu-id="f9b80-141">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="f9b80-142">中所示[中间件](../fundamentals/middleware.md)页上，ASP.NET Core 中间件是公开的类`Invoke`方法拍摄`HttpContext`并返回`Task`。</span><span class="sxs-lookup"><span data-stu-id="f9b80-142">As shown in the [Middleware](../fundamentals/middleware.md) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="f9b80-143">新中间件将如下所示：</span><span class="sxs-lookup"><span data-stu-id="f9b80-143">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="f9b80-144">上面的中间件模板摘录自部分[编写中间件](../fundamentals/middleware.md#middleware-writing-middleware)。</span><span class="sxs-lookup"><span data-stu-id="f9b80-144">The above middleware template was taken from the section on [writing middleware](../fundamentals/middleware.md#middleware-writing-middleware).</span></span>

<span data-ttu-id="f9b80-145">*MyMiddlewareExtensions*帮助器类，更便于配置中的中间件你`Startup`类。</span><span class="sxs-lookup"><span data-stu-id="f9b80-145">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="f9b80-146">`UseMyMiddleware`方法将您中间件的类添加到请求管道。</span><span class="sxs-lookup"><span data-stu-id="f9b80-146">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="f9b80-147">所需的中间件服务获取注入到中间件的构造函数。</span><span class="sxs-lookup"><span data-stu-id="f9b80-147">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="f9b80-148">你的模块可能终止请求，例如，如果用户未授权：</span><span class="sxs-lookup"><span data-stu-id="f9b80-148">Your module might terminate a request, for example if the user is not authorized:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="f9b80-149">中间件的方式处理这通过不调用`Invoke`在管道中的下一步中间件。</span><span class="sxs-lookup"><span data-stu-id="f9b80-149">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="f9b80-150">请注意这并不完全终止请求，因为响应使成为管道上返回到其方法时，仍可以调用以前的中间件。</span><span class="sxs-lookup"><span data-stu-id="f9b80-150">Keep in mind that this does not fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="f9b80-151">当你迁移到新中间件模块的功能时，你可能会发现你的代码不会编译，因为`HttpContext`类中 ASP.NET Core 已显著更改。</span><span class="sxs-lookup"><span data-stu-id="f9b80-151">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="f9b80-152">[更高版本上](#migrating-to-the-new-httpcontext)，你将了解如何将迁移到新的 ASP.NET 核心 HttpContext。</span><span class="sxs-lookup"><span data-stu-id="f9b80-152">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="f9b80-153">迁移模块插入请求管道</span><span class="sxs-lookup"><span data-stu-id="f9b80-153">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="f9b80-154">HTTP 模块通常会添加到请求管道使用*Web.config*:</span><span class="sxs-lookup"><span data-stu-id="f9b80-154">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="f9b80-155">转换这一点[添加新中间件](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)向请求管道中你`Startup`类：</span><span class="sxs-lookup"><span data-stu-id="f9b80-155">Convert this by [adding your new middleware](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="f9b80-156">你可以将插入新中间件管道中的确切点取决于它作为模块处理的事件 (`BeginRequest`，`EndRequest`等) 和在列表中的模块中的顺序*Web.config*。</span><span class="sxs-lookup"><span data-stu-id="f9b80-156">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="f9b80-157">如前面所述，没有任何应用程序生命周期中 ASP.NET 核心，中间件处理响应的顺序不同于使用模块的顺序。</span><span class="sxs-lookup"><span data-stu-id="f9b80-157">As previously stated, there is no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="f9b80-158">这会使排序决策更具挑战性。</span><span class="sxs-lookup"><span data-stu-id="f9b80-158">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="f9b80-159">如果排序将为问题，无法将你的模块分成多个可在单独对其进行排序的中间件组件。</span><span class="sxs-lookup"><span data-stu-id="f9b80-159">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="f9b80-160">迁移到中间件的处理程序代码</span><span class="sxs-lookup"><span data-stu-id="f9b80-160">Migrating handler code to middleware</span></span>

<span data-ttu-id="f9b80-161">HTTP 处理程序如下所示：</span><span class="sxs-lookup"><span data-stu-id="f9b80-161">An HTTP handler looks something like this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="f9b80-162">在 ASP.NET Core 项目中，你将翻译以下到中间件类似于以下内容：</span><span class="sxs-lookup"><span data-stu-id="f9b80-162">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="f9b80-163">此中间件是非常类似于对应于模块的中间件。</span><span class="sxs-lookup"><span data-stu-id="f9b80-163">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="f9b80-164">唯一的真正的区别是，此处没有不需要调用`_next.Invoke(context)`。</span><span class="sxs-lookup"><span data-stu-id="f9b80-164">The only real difference is that here there is no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="f9b80-165">有意义，因为该处理程序末尾的请求管道，因此将没有下一步的中间件来调用。</span><span class="sxs-lookup"><span data-stu-id="f9b80-165">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="f9b80-166">迁移的处理程序插入到请求管道</span><span class="sxs-lookup"><span data-stu-id="f9b80-166">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="f9b80-167">配置的 HTTP 处理程序中完成*Web.config*和如下所示：</span><span class="sxs-lookup"><span data-stu-id="f9b80-167">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="f9b80-168">你无法将其转换通过将新的处理程序中间件添加到请求管道中你`Startup`类，类似于从模块转换的中间件。</span><span class="sxs-lookup"><span data-stu-id="f9b80-168">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="f9b80-169">这种方法的问题是，它会将所有请求都发送到新的处理程序中间件。</span><span class="sxs-lookup"><span data-stu-id="f9b80-169">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="f9b80-170">但是，你只想具有给定扩展名的请求来访问中间件。</span><span class="sxs-lookup"><span data-stu-id="f9b80-170">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="f9b80-171">这样，你必须与 HTTP 处理程序的相同功能。</span><span class="sxs-lookup"><span data-stu-id="f9b80-171">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="f9b80-172">一种解决方案是分支的扩展名为给定的请求管道使用`MapWhen`扩展方法。</span><span class="sxs-lookup"><span data-stu-id="f9b80-172">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="f9b80-173">执行此操作在同一`Configure`你在其中添加其他中间件的方法：</span><span class="sxs-lookup"><span data-stu-id="f9b80-173">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="f9b80-174">`MapWhen`使用这些参数：</span><span class="sxs-lookup"><span data-stu-id="f9b80-174">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="f9b80-175">采用的 lambda`HttpContext`并返回`true`如果请求应会减少分支。</span><span class="sxs-lookup"><span data-stu-id="f9b80-175">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="f9b80-176">这意味着可以分支请求而不仅仅是基于其扩展，而且取决请求标头、 查询字符串参数，等等。</span><span class="sxs-lookup"><span data-stu-id="f9b80-176">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="f9b80-177">采用的 lambda`IApplicationBuilder`并添加分支的所有中间件。</span><span class="sxs-lookup"><span data-stu-id="f9b80-177">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="f9b80-178">这意味着处理程序中间件的前面添加到分支其他中间件。</span><span class="sxs-lookup"><span data-stu-id="f9b80-178">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="f9b80-179">中间件将添加到管道，然后将所有请求; 调用分支分支将在其上没有任何影响。</span><span class="sxs-lookup"><span data-stu-id="f9b80-179">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="f9b80-180">加载使用选项模式的中间件选项</span><span class="sxs-lookup"><span data-stu-id="f9b80-180">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="f9b80-181">一些模块和处理程序已在存储的配置选项*Web.config*。但是，在 ASP.NET 核心中新的配置模型使用代替了*Web.config*。</span><span class="sxs-lookup"><span data-stu-id="f9b80-181">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="f9b80-182">新[配置系统](xref:fundamentals/configuration/index)为你提供了这些选项，以解决此问题：</span><span class="sxs-lookup"><span data-stu-id="f9b80-182">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="f9b80-183">直接注入到中间件，选项中所示[下一节](#loading-middleware-options-through-direct-injection)。</span><span class="sxs-lookup"><span data-stu-id="f9b80-183">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="f9b80-184">使用[选项模式](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="f9b80-184">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1.  <span data-ttu-id="f9b80-185">创建一个类来保存中间件的选项，例如：</span><span class="sxs-lookup"><span data-stu-id="f9b80-185">Create a class to hold your middleware options, for example:</span></span>

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  <span data-ttu-id="f9b80-186">存储选项的值</span><span class="sxs-lookup"><span data-stu-id="f9b80-186">Store the option values</span></span>

    <span data-ttu-id="f9b80-187">配置系统，可存储选项任意位置所需的值。</span><span class="sxs-lookup"><span data-stu-id="f9b80-187">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="f9b80-188">但是，最站点使用*appsettings.json*，因此我们将采用这种办法：</span><span class="sxs-lookup"><span data-stu-id="f9b80-188">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    <span data-ttu-id="f9b80-189">*MyMiddlewareOptionsSection*下面是部分名称。</span><span class="sxs-lookup"><span data-stu-id="f9b80-189">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="f9b80-190">它不必是你的选项类别的名称相同。</span><span class="sxs-lookup"><span data-stu-id="f9b80-190">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="f9b80-191">将选项值与选项类相关联</span><span class="sxs-lookup"><span data-stu-id="f9b80-191">Associate the option values with the options class</span></span>

    <span data-ttu-id="f9b80-192">选项模式使用 ASP.NET 核心依赖关系注入框架将选项类型相关联 (如`MyMiddlewareOptions`) 与`MyMiddlewareOptions`具有实际选项对象。</span><span class="sxs-lookup"><span data-stu-id="f9b80-192">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="f9b80-193">更新你`Startup`类：</span><span class="sxs-lookup"><span data-stu-id="f9b80-193">Update your `Startup` class:</span></span>

    1.  <span data-ttu-id="f9b80-194">如果你使用*appsettings.json*，将其添加到中的配置生成器`Startup`构造函数：</span><span class="sxs-lookup"><span data-stu-id="f9b80-194">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  <span data-ttu-id="f9b80-195">配置选项服务：</span><span class="sxs-lookup"><span data-stu-id="f9b80-195">Configure the options service:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  <span data-ttu-id="f9b80-196">将你的选项与选项类相关联：</span><span class="sxs-lookup"><span data-stu-id="f9b80-196">Associate your options with your options class:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  <span data-ttu-id="f9b80-197">插入到中间件构造函数的选项。</span><span class="sxs-lookup"><span data-stu-id="f9b80-197">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="f9b80-198">这是类似于将注入到控制器的选项。</span><span class="sxs-lookup"><span data-stu-id="f9b80-198">This is similar to injecting options into a controller.</span></span>

  [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  <span data-ttu-id="f9b80-199">[UseMiddleware](#http-modules-usemiddleware)将添加到中间件的扩展方法`IApplicationBuilder`负责的依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="f9b80-199">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

  <span data-ttu-id="f9b80-200">这并不局限于`IOptions`对象。</span><span class="sxs-lookup"><span data-stu-id="f9b80-200">This is not limited to `IOptions` objects.</span></span> <span data-ttu-id="f9b80-201">这种方式，可插入中间件需要的任何其他对象。</span><span class="sxs-lookup"><span data-stu-id="f9b80-201">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="f9b80-202">加载通过直接注入的中间件选项</span><span class="sxs-lookup"><span data-stu-id="f9b80-202">Loading middleware options through direct injection</span></span>

<span data-ttu-id="f9b80-203">选项模式具有创建松散耦合选项值与其使用者之间的优点。</span><span class="sxs-lookup"><span data-stu-id="f9b80-203">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="f9b80-204">一旦你已为实际的选项值其关联选项类别，任何其他类可获得访问通过依赖关系注入框架的选项。</span><span class="sxs-lookup"><span data-stu-id="f9b80-204">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="f9b80-205">没有无需传递选项值。</span><span class="sxs-lookup"><span data-stu-id="f9b80-205">There is no need to pass around options values.</span></span>

<span data-ttu-id="f9b80-206">这将分解但如果你想要使用相同的中间件两次，使用不同的选项。</span><span class="sxs-lookup"><span data-stu-id="f9b80-206">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="f9b80-207">例如授权中间允许不同的角色的不同分支中使用。</span><span class="sxs-lookup"><span data-stu-id="f9b80-207">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="f9b80-208">不能将两个不同的选项对象与一个选项类相关联。</span><span class="sxs-lookup"><span data-stu-id="f9b80-208">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="f9b80-209">解决方法是获取使用中的实际选项值的选项对象你`Startup`类并将这些直接向中间件的每个实例参数传递。</span><span class="sxs-lookup"><span data-stu-id="f9b80-209">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1.  <span data-ttu-id="f9b80-210">添加到的第二个键*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="f9b80-210">Add a second key to *appsettings.json*</span></span>

    <span data-ttu-id="f9b80-211">若要添加另一组选项*appsettings.json*文件中，使用新密钥来唯一标识它：</span><span class="sxs-lookup"><span data-stu-id="f9b80-211">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  <span data-ttu-id="f9b80-212">检索选项值，并将它们传递给中间件。</span><span class="sxs-lookup"><span data-stu-id="f9b80-212">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="f9b80-213">`Use...`扩展方法 （其中添加到管道的中间件） 是传入选项值的逻辑位置：</span><span class="sxs-lookup"><span data-stu-id="f9b80-213">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  <span data-ttu-id="f9b80-214">启用的中间件来采用选项参数。</span><span class="sxs-lookup"><span data-stu-id="f9b80-214">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="f9b80-215">提供的重载`Use...`扩展方法 (它接收 options 参数并将其传递给`UseMiddleware`)。</span><span class="sxs-lookup"><span data-stu-id="f9b80-215">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="f9b80-216">当`UseMiddleware`调用具有参数，它将参数传递给中间件构造函数时它实例化的中间件对象。</span><span class="sxs-lookup"><span data-stu-id="f9b80-216">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

    [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    <span data-ttu-id="f9b80-217">请注意这如何包装中的选项对象`OptionsWrapper`对象。</span><span class="sxs-lookup"><span data-stu-id="f9b80-217">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="f9b80-218">这将实现`IOptions`，如下所需的中间件构造函数。</span><span class="sxs-lookup"><span data-stu-id="f9b80-218">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="f9b80-219">迁移到新 HttpContext</span><span class="sxs-lookup"><span data-stu-id="f9b80-219">Migrating to the new HttpContext</span></span>

<span data-ttu-id="f9b80-220">你此前看到的`Invoke`中间件中的方法采用一个参数的类型`HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="f9b80-220">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="f9b80-221">`HttpContext`已显著更改 ASP.NET Core 中。</span><span class="sxs-lookup"><span data-stu-id="f9b80-221">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="f9b80-222">本部分说明如何将转换的最常用的属性[System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext)对新`Microsoft.AspNetCore.Http.HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="f9b80-222">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="f9b80-223">HttpContext</span><span class="sxs-lookup"><span data-stu-id="f9b80-223">HttpContext</span></span>

<span data-ttu-id="f9b80-224">**HttpContext.Items**都会转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-224">**HttpContext.Items** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="f9b80-225">**唯一请求 ID （没有 System.Web.HttpContext 对应项）**</span><span class="sxs-lookup"><span data-stu-id="f9b80-225">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="f9b80-226">为你的唯一 id 用于每个请求。</span><span class="sxs-lookup"><span data-stu-id="f9b80-226">Gives you a unique id for each request.</span></span> <span data-ttu-id="f9b80-227">日志中包括非常有用。</span><span class="sxs-lookup"><span data-stu-id="f9b80-227">Very useful to include in your logs.</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="f9b80-228">HttpContext.Request</span><span class="sxs-lookup"><span data-stu-id="f9b80-228">HttpContext.Request</span></span>

<span data-ttu-id="f9b80-229">**HttpContext.Request.HttpMethod**都会转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-229">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="f9b80-230">**HttpContext.Request.QueryString**都会转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-230">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="f9b80-231">**HttpContext.Request.Url**和**HttpContext.Request.RawUrl**将转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-231">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="f9b80-232">**HttpContext.Request.IsSecureConnection**都会转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-232">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="f9b80-233">**HttpContext.Request.UserHostAddress**都会转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-233">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="f9b80-234">**HttpContext.Request.Cookies**都会转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-234">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="f9b80-235">**HttpContext.Request.RequestContext.RouteData**都会转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-235">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="f9b80-236">**HttpContext.Request.Headers**都会转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-236">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="f9b80-237">**HttpContext.Request.UserAgent**都会转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-237">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="f9b80-238">**HttpContext.Request.UrlReferrer**都会转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-238">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="f9b80-239">**HttpContext.Request.ContentType**都会转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-239">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="f9b80-240">**HttpContext.Request.Form**都会转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-240">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="f9b80-241">仅当内容的子类型为读取窗体值*x-响应客户-窗体-urlencoded*或*窗体数据*。</span><span class="sxs-lookup"><span data-stu-id="f9b80-241">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="f9b80-242">**HttpContext.Request.InputStream**都会转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-242">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="f9b80-243">仅在处理程序类型中间件，末尾的管道中使用此代码。</span><span class="sxs-lookup"><span data-stu-id="f9b80-243">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="f9b80-244">每个请求的唯一一次如上所示，你可以阅读原始的正文。</span><span class="sxs-lookup"><span data-stu-id="f9b80-244">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="f9b80-245">尝试后第一次读取读取正文的中间件将读取正文为空。</span><span class="sxs-lookup"><span data-stu-id="f9b80-245">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="f9b80-246">这不适用于读取窗体，如下所示更早版本，因为缓冲区中完成。</span><span class="sxs-lookup"><span data-stu-id="f9b80-246">This does not apply to reading a form as shown earlier, because that is done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="f9b80-247">HttpContext.Response</span><span class="sxs-lookup"><span data-stu-id="f9b80-247">HttpContext.Response</span></span>

<span data-ttu-id="f9b80-248">**HttpContext.Response.Status**和**HttpContext.Response.StatusDescription**将转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-248">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="f9b80-249">**HttpContext.Response.ContentEncoding**和**HttpContext.Response.ContentType**将转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-249">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="f9b80-250">**HttpContext.Response.ContentType**上其自身还转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-250">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="f9b80-251">**HttpContext.Response.Output**都会转换为：</span><span class="sxs-lookup"><span data-stu-id="f9b80-251">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="f9b80-252">**HttpContext.Response.TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="f9b80-252">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="f9b80-253">为文件提供服务讨论[此处](../fundamentals/request-features.md#middleware-and-request-features)。</span><span class="sxs-lookup"><span data-stu-id="f9b80-253">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="f9b80-254">**HttpContext.Response.Headers**</span><span class="sxs-lookup"><span data-stu-id="f9b80-254">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="f9b80-255">发送响应标头非常复杂，这一事实，如果任何内容都已写入响应正文将它们设置，它们将不发送。</span><span class="sxs-lookup"><span data-stu-id="f9b80-255">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="f9b80-256">解决方案是将设置将右之前调用写入响应启动的回调方法。</span><span class="sxs-lookup"><span data-stu-id="f9b80-256">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="f9b80-257">最好的做法是在开始`Invoke`中间件中的方法。</span><span class="sxs-lookup"><span data-stu-id="f9b80-257">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="f9b80-258">这是此回调方法，设置响应标头。</span><span class="sxs-lookup"><span data-stu-id="f9b80-258">It is this callback method that sets your response headers.</span></span>

<span data-ttu-id="f9b80-259">下面的代码设置调用的回调方法`SetHeaders`:</span><span class="sxs-lookup"><span data-stu-id="f9b80-259">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="f9b80-260">`SetHeaders`回调方法将如下所示：</span><span class="sxs-lookup"><span data-stu-id="f9b80-260">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="f9b80-261">**HttpContext.Response.Cookies**</span><span class="sxs-lookup"><span data-stu-id="f9b80-261">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="f9b80-262">Cookie 出差到中的浏览器*Set-cookie*响应标头。</span><span class="sxs-lookup"><span data-stu-id="f9b80-262">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="f9b80-263">因此，发送 cookie 都需要用于发送响应标头为使用相同的回调：</span><span class="sxs-lookup"><span data-stu-id="f9b80-263">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="f9b80-264">`SetCookies`回调方法将如下所示：</span><span class="sxs-lookup"><span data-stu-id="f9b80-264">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="f9b80-265">其他资源</span><span class="sxs-lookup"><span data-stu-id="f9b80-265">Additional Resources</span></span>

* [<span data-ttu-id="f9b80-266">HTTP 处理程序和 HTTP 模块概述</span><span class="sxs-lookup"><span data-stu-id="f9b80-266">HTTP Handlers and HTTP Modules Overview</span></span>](https://docs.microsoft.com/iis/configuration/system.webserver/)

* [<span data-ttu-id="f9b80-267">配置</span><span class="sxs-lookup"><span data-stu-id="f9b80-267">Configuration</span></span>](xref:fundamentals/configuration/index)

* [<span data-ttu-id="f9b80-268">应用程序启动</span><span class="sxs-lookup"><span data-stu-id="f9b80-268">Application Startup</span></span>](../fundamentals/startup.md)

* [<span data-ttu-id="f9b80-269">中间件</span><span class="sxs-lookup"><span data-stu-id="f9b80-269">Middleware</span></span>](../fundamentals/middleware.md)
