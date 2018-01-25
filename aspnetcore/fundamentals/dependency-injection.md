---
title: "在 ASP.NET 核心中的依赖关系注入"
author: ardalis
description: "了解如何 ASP.NET Core 实现依赖关系注入和如何使用它。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/dependency-injection
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7a5a0991694b2c7caa79dbc09f6471d614f67dac
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-dependency-injection-in-aspnet-core"></a><span data-ttu-id="991e0-103">在 ASP.NET 核心中的依赖关系注入简介</span><span class="sxs-lookup"><span data-stu-id="991e0-103">Introduction to Dependency Injection in ASP.NET Core</span></span>

<a name="fundamentals-dependency-injection"></a>

<span data-ttu-id="991e0-104">通过[Steve Smith](https://ardalis.com/)和[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="991e0-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="991e0-105">ASP.NET 核心旨在从一开始向上支持，并利用依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="991e0-105">ASP.NET Core is designed from the ground up to support and leverage dependency injection.</span></span> <span data-ttu-id="991e0-106">ASP.NET 核心应用程序可以通过让用户启动类中的方法中注入利用内置 framework 服务和应用程序服务可以配置为也注入。</span><span class="sxs-lookup"><span data-stu-id="991e0-106">ASP.NET Core applications can leverage built-in framework services by having them injected into methods in the Startup class, and application services can be configured for injection as well.</span></span> <span data-ttu-id="991e0-107">由 ASP.NET Core 提供的默认服务容器提供最小功能集，并不旨在替换其他容器。</span><span class="sxs-lookup"><span data-stu-id="991e0-107">The default services container provided by ASP.NET Core provides a minimal feature set and isn't intended to replace other containers.</span></span>

<span data-ttu-id="991e0-108">[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="991e0-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="991e0-109">什么是依赖关系注入？</span><span class="sxs-lookup"><span data-stu-id="991e0-109">What is Dependency Injection?</span></span>

<span data-ttu-id="991e0-110">依赖关系注入 (DI) 是一种技术有助于实现对象及其协作者或依赖项之间的松散耦合。</span><span class="sxs-lookup"><span data-stu-id="991e0-110">Dependency injection (DI) is a technique for achieving loose coupling between objects and their collaborators, or dependencies.</span></span> <span data-ttu-id="991e0-111">而不是直接实例化协作者，或使用的静态引用，到以某种方式类提供了一个类执行其操作所需的对象。</span><span class="sxs-lookup"><span data-stu-id="991e0-111">Rather than directly instantiating collaborators, or using static references, the objects a class needs in order to perform its actions are provided to the class in some fashion.</span></span> <span data-ttu-id="991e0-112">大多数情况下，类将声明通过其构造函数，从而使它们可以按照其依赖项[显式依赖关系原则](http://deviq.com/explicit-dependencies-principle/)。</span><span class="sxs-lookup"><span data-stu-id="991e0-112">Most often, classes will declare their dependencies via their constructor, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span> <span data-ttu-id="991e0-113">此方法被称为"构造函数注入"。</span><span class="sxs-lookup"><span data-stu-id="991e0-113">This approach is known as "constructor injection".</span></span>

<span data-ttu-id="991e0-114">当类旨在与 DI 一起记住时，它们是更松散耦合因为它们没有直接的硬编码的依赖项在其协作者。</span><span class="sxs-lookup"><span data-stu-id="991e0-114">When classes are designed with DI in mind, they're more loosely coupled because they don't have direct, hard-coded dependencies on their collaborators.</span></span> <span data-ttu-id="991e0-115">这需要遵循[依赖反向原则](http://deviq.com/dependency-inversion-principle/)，其中指出*"高级别模块不应依赖于低级别模块; 同时应依赖于抽象"。*</span><span class="sxs-lookup"><span data-stu-id="991e0-115">This follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), which states that *"high level modules shouldn't depend on low level modules; both should depend on abstractions."*</span></span> <span data-ttu-id="991e0-116">引用特定的实现，而不是类请求抽象 (通常`interfaces`) 其提供给其构造类时。</span><span class="sxs-lookup"><span data-stu-id="991e0-116">Instead of referencing specific implementations, classes request abstractions (typically `interfaces`) which are provided to them when the class is constructed.</span></span> <span data-ttu-id="991e0-117">提取到接口的依赖关系并提供实现这些接口作为参数也是一种[策略设计模式](http://deviq.com/strategy-design-pattern/)。</span><span class="sxs-lookup"><span data-stu-id="991e0-117">Extracting dependencies into interfaces and providing implementations of these interfaces as parameters is also an example of the [Strategy design pattern](http://deviq.com/strategy-design-pattern/).</span></span>

<span data-ttu-id="991e0-118">当系统设计为使用 DI 时，具有多个请求通过其构造函数 （或属性），其依赖关系的类很有用具有专用于创建这些类具有其关联的依赖关系的类。</span><span class="sxs-lookup"><span data-stu-id="991e0-118">When a system is designed to use DI, with many classes requesting their dependencies via their constructor (or properties), it's helpful to have a class dedicated to creating these classes with their associated dependencies.</span></span> <span data-ttu-id="991e0-119">这些类统称为*容器*，或更具体地说，[控制反向 (IoC)](http://deviq.com/inversion-of-control/)容器或依赖关系注入 (DI) 容器。</span><span class="sxs-lookup"><span data-stu-id="991e0-119">These classes are referred to as *containers*, or more specifically, [Inversion of Control (IoC)](http://deviq.com/inversion-of-control/) containers or Dependency Injection (DI) containers.</span></span> <span data-ttu-id="991e0-120">容器是实质上是一个工厂，它负责提供从它请求的类型的实例。</span><span class="sxs-lookup"><span data-stu-id="991e0-120">A container is essentially a factory that's responsible for providing instances of types that are requested from it.</span></span> <span data-ttu-id="991e0-121">如果给定的类型已声明它具有依赖关系，并配置容器提供依赖关系类型，它将创建创建请求的实例的过程的依赖关系。</span><span class="sxs-lookup"><span data-stu-id="991e0-121">If a given type has declared that it has dependencies, and the container has been configured to provide the dependency types, it will create the dependencies as part of creating the requested instance.</span></span> <span data-ttu-id="991e0-122">这种方式，可以向无需任何硬编码对象构造的类提供复杂的依赖项关系图。</span><span class="sxs-lookup"><span data-stu-id="991e0-122">In this way, complex dependency graphs can be provided to classes without the need for any hard-coded object construction.</span></span> <span data-ttu-id="991e0-123">除了创建它们的依赖项对象，通常管理容器应用程序中的对象生存期。</span><span class="sxs-lookup"><span data-stu-id="991e0-123">In addition to creating objects with their dependencies, containers typically manage object lifetimes within the application.</span></span>

<span data-ttu-id="991e0-124">ASP.NET 核心包括简单的内置容器 (由表示`IServiceProvider`接口)，默认情况下，支持构造函数注入和 ASP.NET 使某些服务可通过 DI。</span><span class="sxs-lookup"><span data-stu-id="991e0-124">ASP.NET Core includes a simple built-in container (represented by the `IServiceProvider` interface) that supports constructor injection by default, and ASP.NET makes certain services available through DI.</span></span> <span data-ttu-id="991e0-125">ASP。NET 的容器引用的类型将作为管理*服务*。</span><span class="sxs-lookup"><span data-stu-id="991e0-125">ASP.NET's container refers to the types it manages as *services*.</span></span> <span data-ttu-id="991e0-126">本文中，其余*服务*将引用由 ASP.NET Core IoC 容器的类型。</span><span class="sxs-lookup"><span data-stu-id="991e0-126">Throughout the rest of this article, *services* will refer to types that are managed by ASP.NET Core's IoC container.</span></span> <span data-ttu-id="991e0-127">配置中的内置容器的服务`ConfigureServices`方法在应用程序的`Startup`类。</span><span class="sxs-lookup"><span data-stu-id="991e0-127">You configure the built-in container's services in the `ConfigureServices` method in your application's `Startup` class.</span></span>

> [!NOTE]
> <span data-ttu-id="991e0-128">Martin Fowler 上编写大量文章[反转的控件容器和依赖关系注入模式](https://www.martinfowler.com/articles/injection.html)。</span><span class="sxs-lookup"><span data-stu-id="991e0-128">Martin Fowler has written an extensive article on [Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html).</span></span> <span data-ttu-id="991e0-129">Microsoft 模式和实践还具有很好的 description[依赖关系注入](https://msdn.microsoft.com/library/hh323705.aspx)。</span><span class="sxs-lookup"><span data-stu-id="991e0-129">Microsoft Patterns and Practices also has a great description of [Dependency Injection](https://msdn.microsoft.com/library/hh323705.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="991e0-130">本文介绍如何依赖关系注入，适用于所有 ASP.NET 应用程序。</span><span class="sxs-lookup"><span data-stu-id="991e0-130">This article covers Dependency Injection as it applies to all ASP.NET applications.</span></span> <span data-ttu-id="991e0-131">中介绍了在 MVC 控制器中的依赖关系注入[依赖关系注入和控制器](../mvc/controllers/dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="991e0-131">Dependency Injection within MVC controllers is covered in [Dependency Injection and Controllers](../mvc/controllers/dependency-injection.md).</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="991e0-132">构造函数注入行为</span><span class="sxs-lookup"><span data-stu-id="991e0-132">Constructor Injection Behavior</span></span>

<span data-ttu-id="991e0-133">构造函数注入需要问题的构造函数将*公共*。</span><span class="sxs-lookup"><span data-stu-id="991e0-133">Constructor injection requires that the constructor in question be *public*.</span></span> <span data-ttu-id="991e0-134">否则，你的应用程序会引发`InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="991e0-134">Otherwise, your app will throw an `InvalidOperationException`:</span></span>

> <span data-ttu-id="991e0-135">找不到类型 YourType 合适的构造函数。</span><span class="sxs-lookup"><span data-stu-id="991e0-135">A suitable constructor for type 'YourType' couldn't be located.</span></span> <span data-ttu-id="991e0-136">请确保该类型是具体的服务注册的所有参数的公共构造函数。</span><span class="sxs-lookup"><span data-stu-id="991e0-136">Ensure the type is concrete and services are registered for all parameters of a public constructor.</span></span>


<span data-ttu-id="991e0-137">构造函数注入要求存在该只有一个适用的构造函数。</span><span class="sxs-lookup"><span data-stu-id="991e0-137">Constructor injection requires that only one applicable constructor exist.</span></span> <span data-ttu-id="991e0-138">构造函数重载都受支持，但只有一个重载可以存在该形参的实参可以所有完成的依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="991e0-138">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="991e0-139">如果存在多个，你的应用程序将引发`InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="991e0-139">If more than one exists, your app will throw an `InvalidOperationException`:</span></span>

> <span data-ttu-id="991e0-140">已在类型 YourType 中找到多个构造函数接受所有给定的自变量类型。</span><span class="sxs-lookup"><span data-stu-id="991e0-140">Multiple constructors accepting all given argument types have been found in type 'YourType'.</span></span> <span data-ttu-id="991e0-141">只应为一个适用的构造函数。</span><span class="sxs-lookup"><span data-stu-id="991e0-141">There should only be one applicable constructor.</span></span>

<span data-ttu-id="991e0-142">构造函数可以接受没有提供的依赖关系注入的参数，但它们必须支持默认值。</span><span class="sxs-lookup"><span data-stu-id="991e0-142">Constructors can accept arguments that are not provided by dependency injection, but these must support default values.</span></span> <span data-ttu-id="991e0-143">例如:</span><span class="sxs-lookup"><span data-stu-id="991e0-143">For example:</span></span>

```csharp
// throws InvalidOperationException: Unable to resolve service for type 'System.String'...
public CharactersController(ICharacterRepository characterRepository, string title)
{
    _characterRepository = characterRepository;
    _title = title;
}

// runs without error
public CharactersController(ICharacterRepository characterRepository, string title = "Characters")
{
    _characterRepository = characterRepository;
    _title = title;
}
```

## <a name="using-framework-provided-services"></a><span data-ttu-id="991e0-144">使用框架提供的服务</span><span class="sxs-lookup"><span data-stu-id="991e0-144">Using Framework-Provided Services</span></span>

<span data-ttu-id="991e0-145">`ConfigureServices`中的方法`Startup`类负责定义应用程序将使用，包括实体框架核心和 ASP.NET 核心 MVC 等平台功能的服务。</span><span class="sxs-lookup"><span data-stu-id="991e0-145">The `ConfigureServices` method in the `Startup` class is responsible for defining the services the application will use, including platform features like Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="991e0-146">最初，`IServiceCollection`提供给`ConfigureServices`已经安装了以下服务定义 (具体取决于[配置主机的方式](xref:fundamentals/hosting)):</span><span class="sxs-lookup"><span data-stu-id="991e0-146">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/hosting)):</span></span>

| <span data-ttu-id="991e0-147">服务类型</span><span class="sxs-lookup"><span data-stu-id="991e0-147">Service Type</span></span> | <span data-ttu-id="991e0-148">生存期</span><span class="sxs-lookup"><span data-stu-id="991e0-148">Lifetime</span></span> |
| ----- | ------- |
| [<span data-ttu-id="991e0-149">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="991e0-149">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="991e0-150">单一实例</span><span class="sxs-lookup"><span data-stu-id="991e0-150">Singleton</span></span> |
| [<span data-ttu-id="991e0-151">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="991e0-151">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="991e0-152">单一实例</span><span class="sxs-lookup"><span data-stu-id="991e0-152">Singleton</span></span> |
| [<span data-ttu-id="991e0-153">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="991e0-153">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="991e0-154">单一实例</span><span class="sxs-lookup"><span data-stu-id="991e0-154">Singleton</span></span> |
| [<span data-ttu-id="991e0-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="991e0-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="991e0-156">暂时性故障</span><span class="sxs-lookup"><span data-stu-id="991e0-156">Transient</span></span> |
| [<span data-ttu-id="991e0-157">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="991e0-157">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="991e0-158">暂时性故障</span><span class="sxs-lookup"><span data-stu-id="991e0-158">Transient</span></span> |
| [<span data-ttu-id="991e0-159">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="991e0-159">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="991e0-160">单一实例</span><span class="sxs-lookup"><span data-stu-id="991e0-160">Singleton</span></span> |
| [<span data-ttu-id="991e0-161">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="991e0-161">System.Diagnostics.DiagnosticSource</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="991e0-162">单一实例</span><span class="sxs-lookup"><span data-stu-id="991e0-162">Singleton</span></span> |
| [<span data-ttu-id="991e0-163">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="991e0-163">System.Diagnostics.DiagnosticListener</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="991e0-164">单一实例</span><span class="sxs-lookup"><span data-stu-id="991e0-164">Singleton</span></span> |
| [<span data-ttu-id="991e0-165">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="991e0-165">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="991e0-166">暂时性故障</span><span class="sxs-lookup"><span data-stu-id="991e0-166">Transient</span></span> |
| [<span data-ttu-id="991e0-167">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="991e0-167">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="991e0-168">单一实例</span><span class="sxs-lookup"><span data-stu-id="991e0-168">Singleton</span></span> |
| [<span data-ttu-id="991e0-169">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="991e0-169">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="991e0-170">暂时性故障</span><span class="sxs-lookup"><span data-stu-id="991e0-170">Transient</span></span> |
| [<span data-ttu-id="991e0-171">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="991e0-171">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="991e0-172">单一实例</span><span class="sxs-lookup"><span data-stu-id="991e0-172">Singleton</span></span> |
| [<span data-ttu-id="991e0-173">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="991e0-173">Microsoft.AspNetCore.Hosting.IStartup</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="991e0-174">单一实例</span><span class="sxs-lookup"><span data-stu-id="991e0-174">Singleton</span></span> |
| [<span data-ttu-id="991e0-175">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="991e0-175">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="991e0-176">单一实例</span><span class="sxs-lookup"><span data-stu-id="991e0-176">Singleton</span></span> |

<span data-ttu-id="991e0-177">下面是如何使用数等扩展方法的容器中添加其他服务的一个示例`AddDbContext`， `AddIdentity`，和`AddMvc`。</span><span class="sxs-lookup"><span data-stu-id="991e0-177">Below is an example of how to add additional services to the container using a number of extension methods like `AddDbContext`, `AddIdentity`, and `AddMvc`.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

<span data-ttu-id="991e0-178">功能和 ASP.NET MVC，如提供的中间件遵循的约定的使用单个添加*ServiceName*扩展方法注册的所有服务该功能所必需的。</span><span class="sxs-lookup"><span data-stu-id="991e0-178">The features and middleware provided by ASP.NET, such as MVC, follow a convention of using a single Add*ServiceName* extension method to register all of the services required by that feature.</span></span>

>[!TIP]
> <span data-ttu-id="991e0-179">你可以请求中某些框架提供的服务`Startup`通过其参数列表的方法请参阅[应用程序启动](startup.md)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="991e0-179">You can request certain framework-provided services within `Startup` methods through their parameter lists - see [Application Startup](startup.md) for more details.</span></span>

## <a name="registering-your-own-services"></a><span data-ttu-id="991e0-180">注册你自己的服务</span><span class="sxs-lookup"><span data-stu-id="991e0-180">Registering Your Own Services</span></span>

<span data-ttu-id="991e0-181">可以按如下方式注册你自己的应用程序服务。</span><span class="sxs-lookup"><span data-stu-id="991e0-181">You can register your own application services as follows.</span></span> <span data-ttu-id="991e0-182">第一种泛型类型表示将从容器请求的类型 （通常接口）。</span><span class="sxs-lookup"><span data-stu-id="991e0-182">The first generic type represents the type (typically an interface) that will be requested from the container.</span></span> <span data-ttu-id="991e0-183">第二个泛型类型表示将由容器实例化和用来满足此类请求的具体类型。</span><span class="sxs-lookup"><span data-stu-id="991e0-183">The second generic type represents the concrete type that will be instantiated by the container and used to fulfill such requests.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> <span data-ttu-id="991e0-184">每个`services.Add<ServiceName>`扩展方法将添加 （和可能配置） 服务。</span><span class="sxs-lookup"><span data-stu-id="991e0-184">Each `services.Add<ServiceName>` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="991e0-185">例如，`services.AddMvc()`添加 MVC 需要的服务。</span><span class="sxs-lookup"><span data-stu-id="991e0-185">For example, `services.AddMvc()` adds the services MVC requires.</span></span> <span data-ttu-id="991e0-186">建议你遵循此约定，放置中的扩展方法`Microsoft.Extensions.DependencyInjection`命名空间，封装的服务注册的组。</span><span class="sxs-lookup"><span data-stu-id="991e0-186">It's recommended that you follow this convention, placing extension methods in the `Microsoft.Extensions.DependencyInjection` namespace, to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="991e0-187">`AddTransient`方法用于将抽象类型映射到为需要它的每个对象单独实例化的具体服务。</span><span class="sxs-lookup"><span data-stu-id="991e0-187">The `AddTransient` method is used to map abstract types to concrete services that are instantiated separately for every object that requires it.</span></span> <span data-ttu-id="991e0-188">这称为服务的*生存期*，和其他生存期选项如下所述。</span><span class="sxs-lookup"><span data-stu-id="991e0-188">This is known as the service's *lifetime*, and additional lifetime options are described below.</span></span> <span data-ttu-id="991e0-189">请务必选择相应的生存期内，每个你注册的服务。</span><span class="sxs-lookup"><span data-stu-id="991e0-189">It's important to choose an appropriate lifetime for each of the services you register.</span></span> <span data-ttu-id="991e0-190">应提供给请求，则每个类的服务的新实例？</span><span class="sxs-lookup"><span data-stu-id="991e0-190">Should a new instance of the service be provided to each class that requests it?</span></span> <span data-ttu-id="991e0-191">应使用整个的给定的 web 请求的一个实例？</span><span class="sxs-lookup"><span data-stu-id="991e0-191">Should one instance be used throughout a given web request?</span></span> <span data-ttu-id="991e0-192">或者，应单个实例使用的应用程序生存期内？</span><span class="sxs-lookup"><span data-stu-id="991e0-192">Or should a single instance be used for the lifetime of the application?</span></span>

<span data-ttu-id="991e0-193">在本文示例中，没有显示字符名称，调用的简单控制器`CharactersController`。</span><span class="sxs-lookup"><span data-stu-id="991e0-193">In the sample for this article, there's a simple controller that displays character names, called `CharactersController`.</span></span> <span data-ttu-id="991e0-194">其`Index`方法会显示的字符的已存储在应用程序中，当前列表并初始化具有少量的字符的集合，如果不存在。</span><span class="sxs-lookup"><span data-stu-id="991e0-194">Its `Index` method displays the current list of characters that have been stored in the application, and initializes the collection with a handful of characters if none exist.</span></span> <span data-ttu-id="991e0-195">请注意，尽管此应用程序使用实体框架核心和`ApplicationDbContext`为其暂留的任何类，非常明显控制器中。</span><span class="sxs-lookup"><span data-stu-id="991e0-195">Note that although this application uses Entity Framework Core and the `ApplicationDbContext` class for its persistence, none of that's apparent in the controller.</span></span> <span data-ttu-id="991e0-196">在接口，后面的特定数据访问机制抽象化已相反， `ICharacterRepository`，它遵循[存储库模式](http://deviq.com/repository-pattern/)。</span><span class="sxs-lookup"><span data-stu-id="991e0-196">Instead, the specific data access mechanism has been abstracted behind an interface, `ICharacterRepository`, which follows the [repository pattern](http://deviq.com/repository-pattern/).</span></span> <span data-ttu-id="991e0-197">实例`ICharacterRepository`，请求通过构造函数和分配给私有字段，然后将其用于将根据需要访问的字符。</span><span class="sxs-lookup"><span data-stu-id="991e0-197">An instance of `ICharacterRepository` is requested via the constructor and assigned to a private field, which is then used to access characters as necessary.</span></span>

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

<span data-ttu-id="991e0-198">`ICharacterRepository`定义两种方法在控制器需要使用`Character`实例。</span><span class="sxs-lookup"><span data-stu-id="991e0-198">The `ICharacterRepository` defines the two methods the controller needs to work with `Character` instances.</span></span>

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

<span data-ttu-id="991e0-199">此接口的具体类型，反过来实现`CharacterRepository`，在运行时使用。</span><span class="sxs-lookup"><span data-stu-id="991e0-199">This interface is in turn implemented by a concrete type, `CharacterRepository`, that's used at runtime.</span></span>

> [!NOTE]
> <span data-ttu-id="991e0-200">与使用的方式 DI`CharacterRepository`类是你可以按照你的应用程序服务，而不仅仅是在"存储库"或数据访问类中的所有常规模型。</span><span class="sxs-lookup"><span data-stu-id="991e0-200">The way DI is used with the `CharacterRepository` class is a general model you can follow for all of your application services, not just in "repositories" or data access classes.</span></span>

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

<span data-ttu-id="991e0-201">请注意，`CharacterRepository`请求`ApplicationDbContext`其构造函数中。</span><span class="sxs-lookup"><span data-stu-id="991e0-201">Note that `CharacterRepository` requests an `ApplicationDbContext` in its constructor.</span></span> <span data-ttu-id="991e0-202">并非要在每个请求的依赖关系，反过来请求其自身的依赖关系如下，以链接形式中使用的依赖关系注入异常。</span><span class="sxs-lookup"><span data-stu-id="991e0-202">It's not unusual for dependency injection to be used in a chained fashion like this, with each requested dependency in turn requesting its own dependencies.</span></span> <span data-ttu-id="991e0-203">容器将负责解决所有依赖项关系图中，并返回完全解析的服务。</span><span class="sxs-lookup"><span data-stu-id="991e0-203">The container is responsible for resolving all of the dependencies in the graph and returning the fully resolved service.</span></span>

> [!NOTE]
> <span data-ttu-id="991e0-204">创建请求的对象，和的所有要求的对象以及所有对象的那些需要，有时称为*对象图*。</span><span class="sxs-lookup"><span data-stu-id="991e0-204">Creating the requested object, and all of the objects it requires, and all of the objects those require, is sometimes referred to as an *object graph*.</span></span> <span data-ttu-id="991e0-205">同样，必须先解决的依赖项集通常称为*依赖关系树*或*依赖项关系图*。</span><span class="sxs-lookup"><span data-stu-id="991e0-205">Likewise, the collective set of dependencies that must be resolved is typically referred to as a *dependency tree* or *dependency graph*.</span></span>

<span data-ttu-id="991e0-206">在这种情况下，同时`ICharacterRepository`和反过来`ApplicationDbContext`必须向服务容器注册`ConfigureServices`中`Startup`。</span><span class="sxs-lookup"><span data-stu-id="991e0-206">In this case, both `ICharacterRepository` and in turn `ApplicationDbContext` must be registered with the services container in `ConfigureServices` in `Startup`.</span></span> <span data-ttu-id="991e0-207">`ApplicationDbContext`配置了对扩展方法的调用`AddDbContext<T>`。</span><span class="sxs-lookup"><span data-stu-id="991e0-207">`ApplicationDbContext` is configured with the call to the extension method `AddDbContext<T>`.</span></span> <span data-ttu-id="991e0-208">下面的代码演示的注册`CharacterRepository`类型。</span><span class="sxs-lookup"><span data-stu-id="991e0-208">The following code shows the registration of the `CharacterRepository` type.</span></span>

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

<span data-ttu-id="991e0-209">实体框架上下文应添加到服务容器使用`Scoped`生存期。</span><span class="sxs-lookup"><span data-stu-id="991e0-209">Entity Framework contexts should be added to the services container using the `Scoped` lifetime.</span></span> <span data-ttu-id="991e0-210">这是处理自动如果你使用的帮助器方法，如上所示。</span><span class="sxs-lookup"><span data-stu-id="991e0-210">This is taken care of automatically if you use the helper methods as shown above.</span></span> <span data-ttu-id="991e0-211">将实体框架使用的存储库应使用相同的生存期。</span><span class="sxs-lookup"><span data-stu-id="991e0-211">Repositories that will make use of Entity Framework should use the same lifetime.</span></span>

>[!WARNING]
> <span data-ttu-id="991e0-212">解决主要的危险，要谨慎`Scoped`服务中，从单一实例。</span><span class="sxs-lookup"><span data-stu-id="991e0-212">The main danger to be wary of is resolving a `Scoped` service from a singleton.</span></span> <span data-ttu-id="991e0-213">很可能在此类情况下，在处理后续请求时，服务将具有不正确的状态。</span><span class="sxs-lookup"><span data-stu-id="991e0-213">It's likely in such a case that the service will have incorrect state when processing subsequent requests.</span></span>

<span data-ttu-id="991e0-214">具有依赖关系服务应在容器中注册它们。</span><span class="sxs-lookup"><span data-stu-id="991e0-214">Services that have dependencies should register them in the container.</span></span> <span data-ttu-id="991e0-215">如果服务的构造函数需要一个基元，例如`string`，这可以通过使用插入[配置](xref:fundamentals/configuration/index)和[选项模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="991e0-215">If a service's constructor requires a primitive, such as a `string`, this can be injected by using [configuration](xref:fundamentals/configuration/index) and the [options pattern](xref:fundamentals/configuration/options).</span></span>

## <a name="service-lifetimes-and-registration-options"></a><span data-ttu-id="991e0-216">服务生命周期和注册选项</span><span class="sxs-lookup"><span data-stu-id="991e0-216">Service Lifetimes and Registration Options</span></span>

<span data-ttu-id="991e0-217">使用以下的生存期，可以配置 ASP.NET 服务：</span><span class="sxs-lookup"><span data-stu-id="991e0-217">ASP.NET services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="991e0-218">**Transient**</span><span class="sxs-lookup"><span data-stu-id="991e0-218">**Transient**</span></span>

<span data-ttu-id="991e0-219">他们正在请求每次都创建暂时性生存期服务。</span><span class="sxs-lookup"><span data-stu-id="991e0-219">Transient lifetime services are created each time they're requested.</span></span> <span data-ttu-id="991e0-220">此生存期适合轻量、 无状态服务。</span><span class="sxs-lookup"><span data-stu-id="991e0-220">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="991e0-221">**作用域**</span><span class="sxs-lookup"><span data-stu-id="991e0-221">**Scoped**</span></span>

<span data-ttu-id="991e0-222">每个请求，指定了作用域的生存期服务创建一次。</span><span class="sxs-lookup"><span data-stu-id="991e0-222">Scoped lifetime services are created once per request.</span></span>

<span data-ttu-id="991e0-223">**Singleton**</span><span class="sxs-lookup"><span data-stu-id="991e0-223">**Singleton**</span></span>

<span data-ttu-id="991e0-224">单一实例生存期服务创建第一次他们正在请求 (时，或者当`ConfigureServices`如果指定实例存在，则运行)，然后每个后续请求将使用同一个实例。</span><span class="sxs-lookup"><span data-stu-id="991e0-224">Singleton lifetime services are created the first time they're requested (or when `ConfigureServices` is run if you specify an instance there) and then every subsequent request will use the same instance.</span></span> <span data-ttu-id="991e0-225">如果你的应用程序需要单一实例行为，而不是实现单独设计模式和自行管理的类中的对象的生存期建议允许服务容器以管理服务的生存期。</span><span class="sxs-lookup"><span data-stu-id="991e0-225">If your application requires singleton behavior, allowing the services container to manage the service's lifetime is recommended instead of implementing the singleton design pattern and managing your object's lifetime in the class yourself.</span></span>

<span data-ttu-id="991e0-226">可以使用多种方式容器注册服务。</span><span class="sxs-lookup"><span data-stu-id="991e0-226">Services can be registered with the container in several ways.</span></span> <span data-ttu-id="991e0-227">我们已经看到了如何通过指定要使用的具体类型使用给定类型注册服务实现。</span><span class="sxs-lookup"><span data-stu-id="991e0-227">We have already seen how to register a service implementation with a given type by specifying the concrete type to use.</span></span> <span data-ttu-id="991e0-228">此外，可以指定工厂，然后将用于按需创建实例。</span><span class="sxs-lookup"><span data-stu-id="991e0-228">In addition, a factory can be specified, which will then be used to create the instance on demand.</span></span> <span data-ttu-id="991e0-229">第三种方法是类型的直接指定要使用的实例，在其中用例容器将永远不会尝试创建实例 （也不会释放的实例）。</span><span class="sxs-lookup"><span data-stu-id="991e0-229">The third approach is to directly specify the instance of the type to use, in which case the container will never attempt to create an instance (nor will it dispose of the instance).</span></span>

<span data-ttu-id="991e0-230">若要显示这些生存期和注册选项之间的差异，请考虑一个简单接口，表示一个或多个任务，如*操作*具有唯一标识符， `OperationId`。</span><span class="sxs-lookup"><span data-stu-id="991e0-230">To demonstrate the difference between these lifetime and registration options, consider a simple interface that represents one or more tasks as an *operation* with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="991e0-231">根据我们配置此服务的生存期的方式，容器将提供服务在对请求的类相同或不同的实例。</span><span class="sxs-lookup"><span data-stu-id="991e0-231">Depending on how we configure the lifetime for this service, the container will provide either the same or different instances of the service to the requesting class.</span></span> <span data-ttu-id="991e0-232">若要清除正在请求的生存期，我们将创建每个生存期选项的一种类型：</span><span class="sxs-lookup"><span data-stu-id="991e0-232">To make it clear which lifetime is being requested, we will create one type per lifetime option:</span></span>

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

<span data-ttu-id="991e0-233">我们实现使用单个类，这些接口`Operation`，它接受`Guid`在其构造函数或使用新`Guid`如果未提供。</span><span class="sxs-lookup"><span data-stu-id="991e0-233">We implement these interfaces using a single class, `Operation`, that accepts a `Guid` in its constructor, or uses a new `Guid` if none is provided.</span></span>

<span data-ttu-id="991e0-234">接下来，在`ConfigureServices`，每个类型添加到根据生存期命名容器：</span><span class="sxs-lookup"><span data-stu-id="991e0-234">Next, in `ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

<span data-ttu-id="991e0-235">请注意，`IOperationSingletonInstance`服务使用的特定实例的已知 ID 以及`Guid.Empty`以便它时，将清除此类型是在使用 （其 Guid 将所有 0）。</span><span class="sxs-lookup"><span data-stu-id="991e0-235">Note that the `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty` so it will be clear when this type is in use (its Guid will be all zeroes).</span></span> <span data-ttu-id="991e0-236">我们已注册`OperationService`这取决于每个其他`Operation`类型，以便它将对请求中清除此服务是否获取与控制器或一个新的活动，为每个操作类型相同的实例。</span><span class="sxs-lookup"><span data-stu-id="991e0-236">We have also registered an `OperationService` that depends on each of the other `Operation` types, so that it will be clear within a request whether this service is getting the same instance as the controller, or a new one, for each operation type.</span></span> <span data-ttu-id="991e0-237">所有此服务的作用是作为属性公开其依赖项，以便它们可以在视图中显示。</span><span class="sxs-lookup"><span data-stu-id="991e0-237">All this service does is expose its dependencies as properties, so they can be displayed in the view.</span></span>

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

<span data-ttu-id="991e0-238">为了演示对象生存期内和应用程序的单独各个请求之间，此示例包括`OperationsController`请求每种`IOperation`类型以及`OperationService`。</span><span class="sxs-lookup"><span data-stu-id="991e0-238">To demonstrate the object lifetimes within and between separate individual requests to the application, the sample includes an `OperationsController` that requests each kind of `IOperation` type as well as an `OperationService`.</span></span> <span data-ttu-id="991e0-239">`Index`随后操作显示的所有控制器的和服务的`OperationId`值。</span><span class="sxs-lookup"><span data-stu-id="991e0-239">The `Index` action then displays all of the controller's and service's `OperationId` values.</span></span>

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

<span data-ttu-id="991e0-240">现在两个单独的请求都对此控制器操作：</span><span class="sxs-lookup"><span data-stu-id="991e0-240">Now two separate requests are made to this controller action:</span></span>

![显示暂时性、 作用域、 转变成 Singleton，和实例控制器和操作的第一个请求上的服务操作的操作 ID 值 (GUID) 的 Microsoft Edge 中运行的依赖关系注入示例 web 应用程序操作视图中。](dependency-injection/_static/lifetimes_request1.png)

![操作查看显示第二个请求的操作 ID 值。](dependency-injection/_static/lifetimes_request2.png)

<span data-ttu-id="991e0-243">观察哪个`OperationId`值会不同请求之间以及在请求中。</span><span class="sxs-lookup"><span data-stu-id="991e0-243">Observe which of the `OperationId` values vary within a request, and between requests.</span></span>

* <span data-ttu-id="991e0-244">*暂时性*对象始终是不同的; 为每个控制器和每个服务提供的新实例。</span><span class="sxs-lookup"><span data-stu-id="991e0-244">*Transient* objects are always different; a new instance is provided to every controller and every service.</span></span>

* <span data-ttu-id="991e0-245">*作用域*对象是否相同请求，但具有不同跨不同请求中</span><span class="sxs-lookup"><span data-stu-id="991e0-245">*Scoped* objects are the same within a request, but different across different requests</span></span>

* <span data-ttu-id="991e0-246">*Singleton*对象都是相同的每个对象和每个请求 (而不考虑是否的实例提供在`ConfigureServices`)</span><span class="sxs-lookup"><span data-stu-id="991e0-246">*Singleton* objects are the same for every object and every request (regardless of whether an instance is provided in `ConfigureServices`)</span></span>

## <a name="request-services"></a><span data-ttu-id="991e0-247">请求服务</span><span class="sxs-lookup"><span data-stu-id="991e0-247">Request Services</span></span>

<span data-ttu-id="991e0-248">在 ASP.NET 内可用的服务请求从`HttpContext`通过公开`RequestServices`集合。</span><span class="sxs-lookup"><span data-stu-id="991e0-248">The services available within an ASP.NET request from `HttpContext` are exposed through the `RequestServices` collection.</span></span>

![HttpContext 请求服务 Intellisense 上下文对话框指出请求服务获取或设置 IServiceProvider 提供请求的服务容器的访问权限。](dependency-injection/_static/request-services.png)

<span data-ttu-id="991e0-250">请求服务表示的服务配置和请求你的应用程序的一部分。</span><span class="sxs-lookup"><span data-stu-id="991e0-250">Request Services represent the services you configure and request as part of your application.</span></span> <span data-ttu-id="991e0-251">当你的对象指定的依赖关系时，这些满足中找到的类型`RequestServices`，而不`ApplicationServices`。</span><span class="sxs-lookup"><span data-stu-id="991e0-251">When your objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="991e0-252">通常情况下，不应使用这些属性直接，那些喜欢改为类似请求您需要通过您的类的构造函数的类的类型和允许将这些依赖关系注入框架。</span><span class="sxs-lookup"><span data-stu-id="991e0-252">Generally, you shouldn't use these properties directly, preferring instead to request the types your classes you require via your class's constructor, and letting the framework inject these dependencies.</span></span> <span data-ttu-id="991e0-253">这将生成更易于测试的类 (请参阅[测试](../testing/index.md)) 和更松散耦合。</span><span class="sxs-lookup"><span data-stu-id="991e0-253">This yields classes that are easier to test (see [Testing](../testing/index.md)) and are more loosely coupled.</span></span>

> [!NOTE]
> <span data-ttu-id="991e0-254">希望访问的构造函数参数作为请求的依赖关系`RequestServices`集合。</span><span class="sxs-lookup"><span data-stu-id="991e0-254">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="designing-your-services-for-dependency-injection"></a><span data-ttu-id="991e0-255">设计你的服务依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="991e0-255">Designing Your Services For Dependency Injection</span></span>

<span data-ttu-id="991e0-256">应设计你的服务以使用依赖关系注入获取其协作者。</span><span class="sxs-lookup"><span data-stu-id="991e0-256">You should design your services to use dependency injection to get their collaborators.</span></span> <span data-ttu-id="991e0-257">这意味着避免有状态的静态方法调用使用 (这会导致称为代码告知[静态粘贴](http://deviq.com/static-cling/)) 和你的服务中的相关类的直接实例化。</span><span class="sxs-lookup"><span data-stu-id="991e0-257">This means avoiding the use of stateful static method calls (which result in a code smell known as [static cling](http://deviq.com/static-cling/)) and the direct instantiation of dependent classes within your services.</span></span> <span data-ttu-id="991e0-258">它可帮助你记住这个短语：[新增是粘附](https://ardalis.com/new-is-glue)，当选择是否若要实例化一个类型或请求通过依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="991e0-258">It may help to remember the phrase, [New is Glue](https://ardalis.com/new-is-glue), when choosing whether to instantiate a type or to request it via dependency injection.</span></span> <span data-ttu-id="991e0-259">按照[纯色原则的对象面向设计](http://deviq.com/solid/)，您的类将自然倾向于小型、 分解，和轻松测试。</span><span class="sxs-lookup"><span data-stu-id="991e0-259">By following the [SOLID Principles of Object Oriented Design](http://deviq.com/solid/), your classes will naturally tend to be small, well-factored, and easily tested.</span></span>

<span data-ttu-id="991e0-260">到你的类往往具有太多的依赖关系注入的方式怎么办？</span><span class="sxs-lookup"><span data-stu-id="991e0-260">What if you find that your classes tend to have way too many dependencies being injected?</span></span> <span data-ttu-id="991e0-261">这通常是登录您的类尝试执行的操作过多，而且可能违反 SRP-[单个责任原则](http://deviq.com/single-responsibility-principle/)。</span><span class="sxs-lookup"><span data-stu-id="991e0-261">This is generally a sign that your class is trying to do too much, and is probably violating SRP - the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/).</span></span> <span data-ttu-id="991e0-262">如果你可以通过将某些其职责移动到新类重构类，请参阅。</span><span class="sxs-lookup"><span data-stu-id="991e0-262">See if you can refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="991e0-263">请记住，你`Controller`类应关注用户界面问题，因此业务规则和数据访问实现详细信息应保存在适用于这些类中[分离关注事项](http://deviq.com/separation-of-concerns/)。</span><span class="sxs-lookup"><span data-stu-id="991e0-263">Keep in mind that your `Controller` classes should be focused on UI concerns, so business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](http://deviq.com/separation-of-concerns/).</span></span>

<span data-ttu-id="991e0-264">数据访问方面具体而言，可以插入`DbContext`到你的控制器 (假设你已向服务容器中添加 EF `ConfigureServices`)。</span><span class="sxs-lookup"><span data-stu-id="991e0-264">With regards to data access specifically, you can inject the `DbContext` into your controllers (assuming you've added EF to the services container in `ConfigureServices`).</span></span> <span data-ttu-id="991e0-265">一些开发人员更愿意使用数据库的存储库接口而不是将注入`DbContext`直接。</span><span class="sxs-lookup"><span data-stu-id="991e0-265">Some developers prefer to use a repository interface to the database rather than injecting the `DbContext` directly.</span></span> <span data-ttu-id="991e0-266">使用接口来封装数据在一个位置访问逻辑可以最大程度减少必须将你的数据库发生更改时更改的多少位。</span><span class="sxs-lookup"><span data-stu-id="991e0-266">Using an interface to encapsulate the data access logic in one place can minimize how many places you will have to change when your database changes.</span></span>

### <a name="disposing-of-services"></a><span data-ttu-id="991e0-267">释放服务</span><span class="sxs-lookup"><span data-stu-id="991e0-267">Disposing of services</span></span>

<span data-ttu-id="991e0-268">容器将调用`Dispose`为`IDisposable`它创建的类型。</span><span class="sxs-lookup"><span data-stu-id="991e0-268">The container will call `Dispose` for `IDisposable` types it creates.</span></span> <span data-ttu-id="991e0-269">但是，如果你将一个实例添加到容器自己，它将不能释放。</span><span class="sxs-lookup"><span data-stu-id="991e0-269">However, if you add an instance to the container yourself, it will not be disposed.</span></span>

<span data-ttu-id="991e0-270">示例:</span><span class="sxs-lookup"><span data-stu-id="991e0-270">Example:</span></span>

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}


public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // container didn't create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> <span data-ttu-id="991e0-271">在 1.0 版中，容器还调用的 dispose*所有*`IDisposable`对象，包括那些未创建。</span><span class="sxs-lookup"><span data-stu-id="991e0-271">In version 1.0, the container called dispose on *all* `IDisposable` objects, including those it didn't create.</span></span>

## <a name="replacing-the-default-services-container"></a><span data-ttu-id="991e0-272">替换默认服务容器</span><span class="sxs-lookup"><span data-stu-id="991e0-272">Replacing the default services container</span></span>

<span data-ttu-id="991e0-273">内置的服务容器旨在提供框架和在其上生成的大多数使用者应用程序的基本需求。</span><span class="sxs-lookup"><span data-stu-id="991e0-273">The built-in services container is meant to serve the basic needs of the framework and most consumer applications built on it.</span></span> <span data-ttu-id="991e0-274">但是，开发人员可以使用其首选容器替换内置的容器。</span><span class="sxs-lookup"><span data-stu-id="991e0-274">However, developers can replace the built-in container with their preferred container.</span></span> <span data-ttu-id="991e0-275">`ConfigureServices`方法通常返回`void`，但是，如果更改其签名，以便返回`IServiceProvider`，可以配置不同的容器，并将其返回。</span><span class="sxs-lookup"><span data-stu-id="991e0-275">The `ConfigureServices` method typically returns `void`, but if its signature is changed to return `IServiceProvider`, a different container can be configured and returned.</span></span> <span data-ttu-id="991e0-276">没有可用于.NET 的多个 IOC 容器。</span><span class="sxs-lookup"><span data-stu-id="991e0-276">There are many IOC containers available for .NET.</span></span> <span data-ttu-id="991e0-277">在此示例中， [Autofac](https://autofac.org/)使用包。</span><span class="sxs-lookup"><span data-stu-id="991e0-277">In this example, the [Autofac](https://autofac.org/) package is used.</span></span>

<span data-ttu-id="991e0-278">首先，安装适当的容器程序包：</span><span class="sxs-lookup"><span data-stu-id="991e0-278">First, install the appropriate container package(s):</span></span>

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

<span data-ttu-id="991e0-279">接下来，配置中的容器`ConfigureServices`并返回`IServiceProvider`:</span><span class="sxs-lookup"><span data-stu-id="991e0-279">Next, configure the container in `ConfigureServices` and return an `IServiceProvider`:</span></span>

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    // Add other framework services

    // Add Autofac
    var containerBuilder = new ContainerBuilder();
    containerBuilder.RegisterModule<DefaultModule>();
    containerBuilder.Populate(services);
    var container = containerBuilder.Build();
    return new AutofacServiceProvider(container);
}
```

> [!NOTE]
> <span data-ttu-id="991e0-280">在使用第三方 DI 容器时，必须更改`ConfigureServices`，以便它返回`IServiceProvider`而不是`void`。</span><span class="sxs-lookup"><span data-stu-id="991e0-280">When using a third-party DI container, you must change `ConfigureServices` so that it returns `IServiceProvider` instead of `void`.</span></span>

<span data-ttu-id="991e0-281">最后，将 Autofac 配置为在正常`DefaultModule`:</span><span class="sxs-lookup"><span data-stu-id="991e0-281">Finally, configure Autofac as normal in `DefaultModule`:</span></span>

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

<span data-ttu-id="991e0-282">在运行时，将使用 Autofac 来解决类型，并将依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="991e0-282">At runtime, Autofac will be used to resolve types and inject dependencies.</span></span> <span data-ttu-id="991e0-283">[了解有关使用 Autofac 和 ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html)。</span><span class="sxs-lookup"><span data-stu-id="991e0-283">[Learn more about using Autofac and ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="991e0-284">线程安全</span><span class="sxs-lookup"><span data-stu-id="991e0-284">Thread safety</span></span>

<span data-ttu-id="991e0-285">需要是线程安全的单一实例服务。</span><span class="sxs-lookup"><span data-stu-id="991e0-285">Singleton services need to be thread safe.</span></span> <span data-ttu-id="991e0-286">如果单一实例服务暂时服务有依赖项，可能还需要暂时服务是线程安全具体取决于使用单一实例的方式。</span><span class="sxs-lookup"><span data-stu-id="991e0-286">If a singleton service has a dependency on a transient service, the transient service may also need to be thread safe depending how it’s used by the singleton.</span></span>

## <a name="recommendations"></a><span data-ttu-id="991e0-287">建议</span><span class="sxs-lookup"><span data-stu-id="991e0-287">Recommendations</span></span>

<span data-ttu-id="991e0-288">在使用依赖关系注入，请记住以下建议：</span><span class="sxs-lookup"><span data-stu-id="991e0-288">When working with dependency injection, keep the following recommendations in mind:</span></span>

* <span data-ttu-id="991e0-289">DI 是的具有复杂的依赖关系的对象。</span><span class="sxs-lookup"><span data-stu-id="991e0-289">DI is for objects that have complex dependencies.</span></span> <span data-ttu-id="991e0-290">控制器、 服务、 适配器和存储库是的可能添加到 DI 的对象的所有示例。</span><span class="sxs-lookup"><span data-stu-id="991e0-290">Controllers, services, adapters, and repositories are all examples of objects that might be added to DI.</span></span>

* <span data-ttu-id="991e0-291">避免在 DI 中直接存储数据和配置。</span><span class="sxs-lookup"><span data-stu-id="991e0-291">Avoid storing data and configuration directly in DI.</span></span> <span data-ttu-id="991e0-292">例如，用户的购物车通常不应添加到服务容器。</span><span class="sxs-lookup"><span data-stu-id="991e0-292">For example, a user's shopping cart shouldn't typically be added to the services container.</span></span> <span data-ttu-id="991e0-293">配置应使用[选项模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="991e0-293">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="991e0-294">同样，避免仅存在以允许访问某些其他对象的"数据持有者"对象。</span><span class="sxs-lookup"><span data-stu-id="991e0-294">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="991e0-295">它是更好的做法请求通过 DI，如有可能需要的实际项目。</span><span class="sxs-lookup"><span data-stu-id="991e0-295">It's better to request the actual item needed via DI, if possible.</span></span>

* <span data-ttu-id="991e0-296">避免静态访问服务。</span><span class="sxs-lookup"><span data-stu-id="991e0-296">Avoid static access to services.</span></span>

* <span data-ttu-id="991e0-297">避免应用程序代码中的服务位置。</span><span class="sxs-lookup"><span data-stu-id="991e0-297">Avoid service location in your application code.</span></span>

* <span data-ttu-id="991e0-298">避免静态访问`HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="991e0-298">Avoid static access to `HttpContext`.</span></span>

> [!NOTE]
> <span data-ttu-id="991e0-299">建议的所有组，如可能会遇到的情况下忽略一个必需。</span><span class="sxs-lookup"><span data-stu-id="991e0-299">Like all sets of recommendations, you may encounter situations where ignoring one is required.</span></span> <span data-ttu-id="991e0-300">我们发现异常很少见-框架本身中的主要非常特殊用例。</span><span class="sxs-lookup"><span data-stu-id="991e0-300">We have found exceptions to be rare -- mostly very special cases within the framework itself.</span></span>

<span data-ttu-id="991e0-301">请记住，依赖关系注入是*备用*到静态/全局对象访问模式。</span><span class="sxs-lookup"><span data-stu-id="991e0-301">Remember, dependency injection is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="991e0-302">你将无法实现 DI 的优点，如果混合使用静态对象访问权限。</span><span class="sxs-lookup"><span data-stu-id="991e0-302">You won't be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="991e0-303">其他资源</span><span class="sxs-lookup"><span data-stu-id="991e0-303">Additional Resources</span></span>

* [<span data-ttu-id="991e0-304">应用程序启动</span><span class="sxs-lookup"><span data-stu-id="991e0-304">Application Startup</span></span>](startup.md)

* [<span data-ttu-id="991e0-305">测试</span><span class="sxs-lookup"><span data-stu-id="991e0-305">Testing</span></span>](../testing/index.md)

* [<span data-ttu-id="991e0-306">在 ASP.NET 内核，它们有依赖关系注入 (MSDN) 中编写清理代码</span><span class="sxs-lookup"><span data-stu-id="991e0-306">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)

* [<span data-ttu-id="991e0-307">容器管理应用程序设计，作： 其中？ 容器属于</span><span class="sxs-lookup"><span data-stu-id="991e0-307">Container-Managed Application Design, Prelude: Where does the Container Belong?</span></span>](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)

* [<span data-ttu-id="991e0-308">显式依赖关系原则</span><span class="sxs-lookup"><span data-stu-id="991e0-308">Explicit Dependencies Principle</span></span>](http://deviq.com/explicit-dependencies-principle/)

* <span data-ttu-id="991e0-309">[反向的控件容器和依赖关系注入模式](https://www.martinfowler.com/articles/injection.html)(Fowler)</span><span class="sxs-lookup"><span data-stu-id="991e0-309">[Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html) (Fowler)</span></span>
