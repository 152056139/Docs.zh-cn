---
title: "Razor 页单元和集成测试，在 ASP.NET 核心"
author: guardrex
description: "了解如何创建用于 Razor 页的应用程序的单元和集成测试。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: testing/razor-pages-testing
ms.openlocfilehash: 3f53924e0b36b7924d82f97a8702aa461d9ebd78
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="razor-pages-unit-and-integration-testing-in-aspnet-core"></a><span data-ttu-id="e1c00-103">Razor 页单元和集成测试，在 ASP.NET 核心</span><span class="sxs-lookup"><span data-stu-id="e1c00-103">Razor Pages unit and integration testing in ASP.NET Core</span></span>

<span data-ttu-id="e1c00-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e1c00-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e1c00-105">ASP.NET 核心支持单元和集成测试的 Razor 页应用。</span><span class="sxs-lookup"><span data-stu-id="e1c00-105">ASP.NET Core supports unit and integration testing of Razor Pages apps.</span></span> <span data-ttu-id="e1c00-106">测试数据访问层 (DAL)、 页面模型和集成页上组件可帮助确保：</span><span class="sxs-lookup"><span data-stu-id="e1c00-106">Testing the data access layer (DAL), page models, and integrated page components helps ensure:</span></span>

* <span data-ttu-id="e1c00-107">部分 Razor 页面应用程序在应用程序构造期间工作独立以及组合在一起作为一个单元。</span><span class="sxs-lookup"><span data-stu-id="e1c00-107">Parts of a Razor Pages app work independently and together as a unit during app construction.</span></span>
* <span data-ttu-id="e1c00-108">类和方法具有有限的作用域的责任。</span><span class="sxs-lookup"><span data-stu-id="e1c00-108">Classes and methods have limited scopes of responsibility.</span></span>
* <span data-ttu-id="e1c00-109">上应用的行为方式存在其他文档。</span><span class="sxs-lookup"><span data-stu-id="e1c00-109">Additional documentation exists on how the app should behave.</span></span>
* <span data-ttu-id="e1c00-110">回归，不由更新对代码的错误，将自动的生成和部署期间发现。</span><span class="sxs-lookup"><span data-stu-id="e1c00-110">Regressions, which are errors brought about by updates to the code, are found during automated building and deployment.</span></span>

<span data-ttu-id="e1c00-111">本主题假定你已基本了解 Razor 页应用、 单元测试，以及集成测试。</span><span class="sxs-lookup"><span data-stu-id="e1c00-111">This topic assumes that you have a basic understanding of Razor Pages apps, unit testing, and integration testing.</span></span> <span data-ttu-id="e1c00-112">如果你熟悉 Razor 页应用或测试概念，请参阅以下主题：</span><span class="sxs-lookup"><span data-stu-id="e1c00-112">If you're unfamiliar with Razor Pages apps or testing concepts, see the following topics:</span></span>

* [<span data-ttu-id="e1c00-113">Razor 页面介绍</span><span class="sxs-lookup"><span data-stu-id="e1c00-113">Introduction to Razor Pages</span></span>](xref:mvc/razor-pages/index)
* [<span data-ttu-id="e1c00-114">Razor 页面入门</span><span class="sxs-lookup"><span data-stu-id="e1c00-114">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="e1c00-115">单元测试 C# 中使用 dotnet 测试和 xUnit 的.NET 核心</span><span class="sxs-lookup"><span data-stu-id="e1c00-115">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="e1c00-116">集成测试</span><span class="sxs-lookup"><span data-stu-id="e1c00-116">Integration testing</span></span>](xref:testing/integration-testing)

<span data-ttu-id="e1c00-117">[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/razor-pages-testing/sample/)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="e1c00-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/razor-pages-testing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e1c00-118">示例项目组成两个应用程序：</span><span class="sxs-lookup"><span data-stu-id="e1c00-118">The sample project is composed of two apps:</span></span>

| <span data-ttu-id="e1c00-119">应用</span><span class="sxs-lookup"><span data-stu-id="e1c00-119">App</span></span>         | <span data-ttu-id="e1c00-120">项目文件夹</span><span class="sxs-lookup"><span data-stu-id="e1c00-120">Project folder</span></span>                        | <span data-ttu-id="e1c00-121">描述</span><span class="sxs-lookup"><span data-stu-id="e1c00-121">Description</span></span> |
| ----------- | ------------------------------------- | ----------- |
| <span data-ttu-id="e1c00-122">消息应用程序</span><span class="sxs-lookup"><span data-stu-id="e1c00-122">Message app</span></span> | <span data-ttu-id="e1c00-123">*src/RazorPagesTestingSample*</span><span class="sxs-lookup"><span data-stu-id="e1c00-123">*src/RazorPagesTestingSample*</span></span>         | <span data-ttu-id="e1c00-124">允许用户添加、 删除其中一个，删除所有，和分析消息。</span><span class="sxs-lookup"><span data-stu-id="e1c00-124">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="e1c00-125">测试应用程序</span><span class="sxs-lookup"><span data-stu-id="e1c00-125">Test app</span></span>    | <span data-ttu-id="e1c00-126">*tests/RazorPagesTestingSample.Tests*</span><span class="sxs-lookup"><span data-stu-id="e1c00-126">*tests/RazorPagesTestingSample.Tests*</span></span> | <span data-ttu-id="e1c00-127">用于测试消息应用程序。</span><span class="sxs-lookup"><span data-stu-id="e1c00-127">Used to test the message app.</span></span><ul><li><span data-ttu-id="e1c00-128">单元测试： 数据访问层 (DAL)，索引页模型</span><span class="sxs-lookup"><span data-stu-id="e1c00-128">Unit tests: Data access layer (DAL), Index page model</span></span></li><li><span data-ttu-id="e1c00-129">集成测试： 索引页</span><span class="sxs-lookup"><span data-stu-id="e1c00-129">Integration tests: Index page</span></span></li></ul> |

<span data-ttu-id="e1c00-130">可以使用内置的测试功能的一个 IDE，如运行测试[Visual Studio](https://www.visualstudio.com/vs/)。</span><span class="sxs-lookup"><span data-stu-id="e1c00-130">The tests can be run using the built-in testing features of an IDE, such as [Visual Studio](https://www.visualstudio.com/vs/).</span></span> <span data-ttu-id="e1c00-131">如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令行中，执行以下命令在命令提示符中*tests/RazorPagesTestingSample.Tests*文件夹：</span><span class="sxs-lookup"><span data-stu-id="e1c00-131">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesTestingSample.Tests* folder:</span></span>

```console
dotnet test
```

## <a name="message-app-organization"></a><span data-ttu-id="e1c00-132">消息应用组织</span><span class="sxs-lookup"><span data-stu-id="e1c00-132">Message app organization</span></span>

<span data-ttu-id="e1c00-133">消息应用程序是一个简单的 Razor 页消息系统，具有以下特征：</span><span class="sxs-lookup"><span data-stu-id="e1c00-133">The message app is a simple Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="e1c00-134">应用程序的索引页 (*Pages/Index.cshtml*和*Pages/Index.cshtml.cs*) 提供一个用户界面和网页模型方法，用于控制添加、 删除和分析消息 （每个消息的平均词）.</span><span class="sxs-lookup"><span data-stu-id="e1c00-134">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="e1c00-135">一条消息都由描述`Message`类 (*Data/Message.cs*) 具有两个属性： `Id` （密钥） 和`Text`（消息）。</span><span class="sxs-lookup"><span data-stu-id="e1c00-135">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="e1c00-136">`Text`属性是必需的限制为 200 个字符。</span><span class="sxs-lookup"><span data-stu-id="e1c00-136">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="e1c00-137">消息存储使用[实体框架的内存中数据库](/ef/core/providers/in-memory/)&#8224;。</span><span class="sxs-lookup"><span data-stu-id="e1c00-137">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="e1c00-138">此应用程序包含在其数据库上下文类中，数据访问层 (DAL) `AppDbContext` (*Data/AppDbContext.cs*)。</span><span class="sxs-lookup"><span data-stu-id="e1c00-138">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span> <span data-ttu-id="e1c00-139">DAL 方法将被标记`virtual`，这样，模拟在测试中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="e1c00-139">The DAL methods are marked `virtual`, which allows mocking the methods for use in the tests.</span></span>
* <span data-ttu-id="e1c00-140">如果数据库为空应用程序启动时，消息存储区初始化与三条消息。</span><span class="sxs-lookup"><span data-stu-id="e1c00-140">If the database is empty on app startup, the message store is initialized with three messages.</span></span> <span data-ttu-id="e1c00-141">这些*设定种子消息*也用在测试。</span><span class="sxs-lookup"><span data-stu-id="e1c00-141">These *seeded messages* are also used in testing.</span></span>

<span data-ttu-id="e1c00-142">&#8224;EF 主题，[测试以及 InMemory](/ef/core/miscellaneous/testing/in-memory)，说明如何使用内存中数据库来使用 MSTest 测试。</span><span class="sxs-lookup"><span data-stu-id="e1c00-142">&#8224;The EF topic, [Testing with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for testing with MSTest.</span></span> <span data-ttu-id="e1c00-143">本主题使用[xUnit](https://xunit.github.io/)测试框架。</span><span class="sxs-lookup"><span data-stu-id="e1c00-143">This topic uses the [xUnit](https://xunit.github.io/) testing framework.</span></span> <span data-ttu-id="e1c00-144">测试概念和测试跨不同测试框架的实现是类似，但不是完全相同。</span><span class="sxs-lookup"><span data-stu-id="e1c00-144">Testing concepts and test implementations across different testing frameworks are similar but not identical.</span></span>

<span data-ttu-id="e1c00-145">尽管应用程序不使用[存储库模式](http://martinfowler.com/eaaCatalog/repository.html)并不是有效的示例[工作单元 (UoW) 模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)，Razor 页中支持的开发模式。</span><span class="sxs-lookup"><span data-stu-id="e1c00-145">Although the app doesn't use the [repository pattern](http://martinfowler.com/eaaCatalog/repository.html) and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="e1c00-146">有关详细信息，请参阅[设计基础结构持久性层](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)，[在 ASP.NET MVC 应用程序中实现的存储库和单元的工作模式](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)，和[测试控制器逻辑](/aspnet/core/mvc/controllers/testing)（此示例实现的存储库模式）。</span><span class="sxs-lookup"><span data-stu-id="e1c00-146">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [Implementing the Repository and Unit of Work Patterns in an ASP.NET MVC Application](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), and [Testing controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

## <a name="test-app-organization"></a><span data-ttu-id="e1c00-147">测试应用程序的组织</span><span class="sxs-lookup"><span data-stu-id="e1c00-147">Test app organization</span></span>

<span data-ttu-id="e1c00-148">测试应用程序是在一个控制台应用*tests/RazorPagesTestingSample.Tests*文件夹：</span><span class="sxs-lookup"><span data-stu-id="e1c00-148">The test app is a console app inside the *tests/RazorPagesTestingSample.Tests* folder:</span></span>

| <span data-ttu-id="e1c00-149">测试应用程序文件夹</span><span class="sxs-lookup"><span data-stu-id="e1c00-149">Test app folder</span></span>    | <span data-ttu-id="e1c00-150">描述</span><span class="sxs-lookup"><span data-stu-id="e1c00-150">Description</span></span> |
| ------------------ | ----------- |
| <span data-ttu-id="e1c00-151">*IntegrationTests*</span><span class="sxs-lookup"><span data-stu-id="e1c00-151">*IntegrationTests*</span></span> | <ul><li><span data-ttu-id="e1c00-152">*IndexPageTest.cs*包含索引页的集成测试。</span><span class="sxs-lookup"><span data-stu-id="e1c00-152">*IndexPageTest.cs* contains the integration tests for the Index page.</span></span></li><li><span data-ttu-id="e1c00-153">*TestFixture.cs*创建测试主机测试消息应用程序。</span><span class="sxs-lookup"><span data-stu-id="e1c00-153">*TestFixture.cs* creates the test host to test the message app.</span></span></li></ul> |
| <span data-ttu-id="e1c00-154">*UnitTests*</span><span class="sxs-lookup"><span data-stu-id="e1c00-154">*UnitTests*</span></span>        | <ul><li><span data-ttu-id="e1c00-155">*DataAccessLayerTest.cs* DAL 包含单元测试。</span><span class="sxs-lookup"><span data-stu-id="e1c00-155">*DataAccessLayerTest.cs* contains the unit tests for the DAL.</span></span></li><li><span data-ttu-id="e1c00-156">*IndexPageTest.cs*包含针对索引页模型的单元测试。</span><span class="sxs-lookup"><span data-stu-id="e1c00-156">*IndexPageTest.cs* contains the unit tests for the Index page model.</span></span></li></ul> |
| <span data-ttu-id="e1c00-157">*实用程序*</span><span class="sxs-lookup"><span data-stu-id="e1c00-157">*Utilities*</span></span>        | <span data-ttu-id="e1c00-158">*Utilities.cs*包含:</span><span class="sxs-lookup"><span data-stu-id="e1c00-158">*Utilities.cs* contains the:</span></span><ul><li><span data-ttu-id="e1c00-159">`TestingDbContextOptions` 用于创建新的数据库上下文对于每个 DAL 单元测试的选项，以便数据库重置为其基线条件的每个测试方法。</span><span class="sxs-lookup"><span data-stu-id="e1c00-159">`TestingDbContextOptions` method used to create new database context options for each DAL unit test so that the database is reset to its baseline condition for each test.</span></span></li><li><span data-ttu-id="e1c00-160">`GetRequestContentAsync` 方法用于准备`HttpClient`和内容的集成测试在发送到消息应用程序的请求。</span><span class="sxs-lookup"><span data-stu-id="e1c00-160">`GetRequestContentAsync` method used to prepare the `HttpClient` and content for requests that are sent to the message app during integration testing.</span></span></li></ul>

<span data-ttu-id="e1c00-161">测试框架为[xUnit](https://xunit.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="e1c00-161">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="e1c00-162">模拟框架的对象是[Moq](https://github.com/moq/moq4)。</span><span class="sxs-lookup"><span data-stu-id="e1c00-162">The object mocking framework is [Moq](https://github.com/moq/moq4).</span></span> <span data-ttu-id="e1c00-163">集成测试使用执行[ASP.NET 核心测试主机](xref:testing/integration-testing#the-test-host)。</span><span class="sxs-lookup"><span data-stu-id="e1c00-163">Integration tests are conducted using the [ASP.NET Core Test Host](xref:testing/integration-testing#the-test-host).</span></span>

## <a name="unit-testing-the-data-access-layer-dal"></a><span data-ttu-id="e1c00-164">单元测试的数据访问层 (DAL)</span><span class="sxs-lookup"><span data-stu-id="e1c00-164">Unit testing the data access layer (DAL)</span></span>

<span data-ttu-id="e1c00-165">消息应用程序中包含的四个方法与出现 DAL`AppDbContext`类 (*src/RazorPagesTestingSample/Data/AppDbContext.cs*)。</span><span class="sxs-lookup"><span data-stu-id="e1c00-165">The message app has a DAL with four methods contained in the `AppDbContext` class (*src/RazorPagesTestingSample/Data/AppDbContext.cs*).</span></span> <span data-ttu-id="e1c00-166">每个方法中测试应用程序包含一个或两个单元测试。</span><span class="sxs-lookup"><span data-stu-id="e1c00-166">Each method has one or two unit tests in the test app.</span></span>

| <span data-ttu-id="e1c00-167">DAL 方法</span><span class="sxs-lookup"><span data-stu-id="e1c00-167">DAL method</span></span>               | <span data-ttu-id="e1c00-168">函数</span><span class="sxs-lookup"><span data-stu-id="e1c00-168">Function</span></span>                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | <span data-ttu-id="e1c00-169">获取`List<Message>`从数据库按排序`Text`属性。</span><span class="sxs-lookup"><span data-stu-id="e1c00-169">Obtains a `List<Message>` from the database sorted by the `Text` property.</span></span> |
| `AddMessageAsync`        | <span data-ttu-id="e1c00-170">将添加`Message`到数据库。</span><span class="sxs-lookup"><span data-stu-id="e1c00-170">Adds a `Message` to the database.</span></span>                                          |
| `DeleteAllMessagesAsync` | <span data-ttu-id="e1c00-171">删除所有`Message`从数据库的条目。</span><span class="sxs-lookup"><span data-stu-id="e1c00-171">Deletes all `Message` entries from the database.</span></span>                           |
| `DeleteMessageAsync`     | <span data-ttu-id="e1c00-172">将删除单个`Message`从数据库`Id`。</span><span class="sxs-lookup"><span data-stu-id="e1c00-172">Deletes a single `Message` from the database by `Id`.</span></span>                      |

<span data-ttu-id="e1c00-173">DAL 的单元测试需要[DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions)时创建新`AppDbContext`为每个测试。</span><span class="sxs-lookup"><span data-stu-id="e1c00-173">Unit tests of the DAL require [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) when creating a new `AppDbContext` for each test.</span></span> <span data-ttu-id="e1c00-174">创建的一种方法`DbContextOptions`每个测试是使用[DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):</span><span class="sxs-lookup"><span data-stu-id="e1c00-174">One approach to creating the `DbContextOptions` for each test is to use a [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="e1c00-175">使用此方法的问题是，每个测试就会收到数据库中以前的测试中保留它的任何状态。</span><span class="sxs-lookup"><span data-stu-id="e1c00-175">The problem with this approach is that each test receives the database in whatever state the previous test left it.</span></span> <span data-ttu-id="e1c00-176">在尝试写入不会相互影响的原子单元测试时，这可能会产生问题。</span><span class="sxs-lookup"><span data-stu-id="e1c00-176">This can be problematic when trying to write atomic unit tests that don't interfere with each other.</span></span> <span data-ttu-id="e1c00-177">若要强制`AppDbContext`若要对每个测试中使用新的数据库上下文，提供`DbContextOptions`基于新的服务提供程序的实例。</span><span class="sxs-lookup"><span data-stu-id="e1c00-177">To force the `AppDbContext` to use a new database context for each test, supply a `DbContextOptions` instance that's based on a new service provider.</span></span> <span data-ttu-id="e1c00-178">测试应用程序演示如何执行此操作使用其`Utilities`类方法`TestingDbContextOptions`(*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):</span><span class="sxs-lookup"><span data-stu-id="e1c00-178">The test app shows how to do this using its `Utilities` class method `TestingDbContextOptions` (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet1)]

<span data-ttu-id="e1c00-179">使用`DbContextOptions`DAL 单元中测试允许每个测试运行以原子方式与全新的数据库实例：</span><span class="sxs-lookup"><span data-stu-id="e1c00-179">Using the `DbContextOptions` in the DAL unit tests allows each test to run atomically with a fresh database instance:</span></span>

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="e1c00-180">在每个测试方法`DataAccessLayerTest`类 (*UnitTests/DataAccessLayerTest.cs*) 遵循类似的排列 Act 断言模式：</span><span class="sxs-lookup"><span data-stu-id="e1c00-180">Each test method in the `DataAccessLayerTest` class (*UnitTests/DataAccessLayerTest.cs*) follows a similar Arrange-Act-Assert pattern:</span></span>

1. <span data-ttu-id="e1c00-181">排列： 测试配置了数据库和/或定义预期的结果。</span><span class="sxs-lookup"><span data-stu-id="e1c00-181">Arrange: The database is configured for the test and/or the expected outcome is defined.</span></span>
1. <span data-ttu-id="e1c00-182">Act： 执行测试。</span><span class="sxs-lookup"><span data-stu-id="e1c00-182">Act: The test is executed.</span></span>
1. <span data-ttu-id="e1c00-183">断言： 断言可确定测试结果是否成功。</span><span class="sxs-lookup"><span data-stu-id="e1c00-183">Assert: Assertions are made to determine if the test result is a success.</span></span>

<span data-ttu-id="e1c00-184">例如，`DeleteMessageAsync`方法负责删除单个消息由其`Id`(*src/RazorPagesTestingSample/Data/AppDbContext.cs*):</span><span class="sxs-lookup"><span data-stu-id="e1c00-184">For example, the `DeleteMessageAsync` method is responsible for removing a single message identified by its `Id` (*src/RazorPagesTestingSample/Data/AppDbContext.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/AppDbContext.cs?name=snippet4)]

<span data-ttu-id="e1c00-185">没有为此方法的两个测试。</span><span class="sxs-lookup"><span data-stu-id="e1c00-185">There are two tests for this method.</span></span> <span data-ttu-id="e1c00-186">一个测试检查数据库中存在消息时，该方法将删除一条消息。</span><span class="sxs-lookup"><span data-stu-id="e1c00-186">One test checks that the method deletes a message when the message is present in the database.</span></span> <span data-ttu-id="e1c00-187">如果不会更改数据库的其他方法测试消息`Id`删除不存在。</span><span class="sxs-lookup"><span data-stu-id="e1c00-187">The other method tests that the database doesn't change if the message `Id` for deletion doesn't exist.</span></span> <span data-ttu-id="e1c00-188">`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="e1c00-188">The `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` method is shown below:</span></span>

[!code-csharp[](razor-pages-testing/sample_snapshot/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="e1c00-189">首先，该方法执行准备步骤中，执行步骤准备发生。</span><span class="sxs-lookup"><span data-stu-id="e1c00-189">First, the method performs the Arrange step, where preparation for the Act step takes place.</span></span> <span data-ttu-id="e1c00-190">获取并保存在种子设定消息`seedMessages`。</span><span class="sxs-lookup"><span data-stu-id="e1c00-190">The seeding messages are obtained and held in `seedMessages`.</span></span> <span data-ttu-id="e1c00-191">种子设定的消息会保存到数据库中。</span><span class="sxs-lookup"><span data-stu-id="e1c00-191">The seeding messages are saved into the database.</span></span> <span data-ttu-id="e1c00-192">将消息与`Id`的`1`设置为删除。</span><span class="sxs-lookup"><span data-stu-id="e1c00-192">The message with an `Id` of `1` is set for deletion.</span></span> <span data-ttu-id="e1c00-193">当`DeleteMessageAsync`执行方法时，预期的消息应具有所有除外与消息`Id`的`1`。</span><span class="sxs-lookup"><span data-stu-id="e1c00-193">When the `DeleteMessageAsync` method is executed, the expected messages should have all of the messages except for the one with an `Id` of `1`.</span></span> <span data-ttu-id="e1c00-194">`expectedMessages`变量表示此预期的结果。</span><span class="sxs-lookup"><span data-stu-id="e1c00-194">The `expectedMessages` variable represents this expected outcome.</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="e1c00-195">方法是运行：`DeleteMessageAsync`执行方法中传递`recId`的`1`:</span><span class="sxs-lookup"><span data-stu-id="e1c00-195">The method acts: The `DeleteMessageAsync` method is executed passing in the `recId` of `1`:</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

<span data-ttu-id="e1c00-196">最后，此方法获取`Messages`上下文中，并将其到`expectedMessages`两个相等的断言：</span><span class="sxs-lookup"><span data-stu-id="e1c00-196">Finally, the method obtains the `Messages` from the context and compares it to the `expectedMessages` asserting that the two are equal:</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

<span data-ttu-id="e1c00-197">若要比较的两个`List<Message>`相同：</span><span class="sxs-lookup"><span data-stu-id="e1c00-197">In order to compare that the two `List<Message>` are the same:</span></span>

* <span data-ttu-id="e1c00-198">消息有序`Id`。</span><span class="sxs-lookup"><span data-stu-id="e1c00-198">The messages are ordered by `Id`.</span></span>
* <span data-ttu-id="e1c00-199">消息对比较上`Text`属性。</span><span class="sxs-lookup"><span data-stu-id="e1c00-199">Message pairs are compared on the `Text` property.</span></span>

<span data-ttu-id="e1c00-200">类似的测试方法，`DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound`检查正在尝试删除一条消息，不存在的结果。</span><span class="sxs-lookup"><span data-stu-id="e1c00-200">A similar test method, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` checks the result of attempting to delete a message that doesn't exist.</span></span> <span data-ttu-id="e1c00-201">在这种情况下，在数据库中预期的消息应等于后的实际消息`DeleteMessageAsync`执行方法。</span><span class="sxs-lookup"><span data-stu-id="e1c00-201">In this case, the expected messages in the database should be equal to the actual messages after the `DeleteMessageAsync` method is executed.</span></span> <span data-ttu-id="e1c00-202">应没有更改数据库的内容：</span><span class="sxs-lookup"><span data-stu-id="e1c00-202">There should be no change to the database's content:</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-testing-the-page-model-methods"></a><span data-ttu-id="e1c00-203">单元测试页模型方法</span><span class="sxs-lookup"><span data-stu-id="e1c00-203">Unit testing the page model methods</span></span>

<span data-ttu-id="e1c00-204">另一个组的单元测试负责测试页模型方法。</span><span class="sxs-lookup"><span data-stu-id="e1c00-204">Another set of unit tests is responsible for testing page model methods.</span></span> <span data-ttu-id="e1c00-205">在消息应用中，索引页模型中找到`IndexModel`类*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="e1c00-205">In the message app, the Index page models are found in the `IndexModel` class in *src/RazorPagesTestingSample/Pages/Index.cshtml.cs*.</span></span>

| <span data-ttu-id="e1c00-206">页模型方法</span><span class="sxs-lookup"><span data-stu-id="e1c00-206">Page model method</span></span> | <span data-ttu-id="e1c00-207">函数</span><span class="sxs-lookup"><span data-stu-id="e1c00-207">Function</span></span> |
| ----------------- | -------- | 
| `OnGetAsync` | <span data-ttu-id="e1c00-208">从 UI 使用 DAL 获取消息`GetMessagesAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="e1c00-208">Obtains the messages from the DAL for the UI using the `GetMessagesAsync` method.</span></span> |
| `OnPostAddMessageAsync` | <span data-ttu-id="e1c00-209">如果`ModelState`无效，请调用`AddMessageAsync`向数据库添加一条消息。</span><span class="sxs-lookup"><span data-stu-id="e1c00-209">If the `ModelState` is valid, calls `AddMessageAsync` to add a message to the database.</span></span> | 
| `OnPostDeleteAllMessagesAsync` | <span data-ttu-id="e1c00-210">调用`DeleteAllMessagesAsync`删除所有数据库中的消息。</span><span class="sxs-lookup"><span data-stu-id="e1c00-210">Calls `DeleteAllMessagesAsync` to delete all of the messages in the database.</span></span> |
| `OnPostDeleteMessageAsync` | <span data-ttu-id="e1c00-211">执行`DeleteMessageAsync`若要删除的消息`Id`指定。</span><span class="sxs-lookup"><span data-stu-id="e1c00-211">Executes `DeleteMessageAsync` to delete a message with the `Id` specified.</span></span> |
| `OnPostAnalyzeMessagesAsync` | <span data-ttu-id="e1c00-212">如果一个或多个消息是在数据库中，将计算每个消息的单词的平均的数目。</span><span class="sxs-lookup"><span data-stu-id="e1c00-212">If one or more messages are in the database, calculates the average number of words per message.</span></span> |

<span data-ttu-id="e1c00-213">使用七个测试中的测试页模型方法`IndexPageTest`类 (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*)。</span><span class="sxs-lookup"><span data-stu-id="e1c00-213">The page model methods are tested using seven tests in the `IndexPageTest` class (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*).</span></span> <span data-ttu-id="e1c00-214">测试使用熟悉的排列断言 Act 模式。</span><span class="sxs-lookup"><span data-stu-id="e1c00-214">The tests use the familiar Arrange-Assert-Act pattern.</span></span> <span data-ttu-id="e1c00-215">这些测试的重点：</span><span class="sxs-lookup"><span data-stu-id="e1c00-215">These tests focus on:</span></span>

* <span data-ttu-id="e1c00-216">确定方法是否遵循正确的行为时`ModelState`无效。</span><span class="sxs-lookup"><span data-stu-id="e1c00-216">Determining if the methods follow the correct behavior when the `ModelState` is invalid.</span></span>
* <span data-ttu-id="e1c00-217">确认方法生成正确`IActionResult`。</span><span class="sxs-lookup"><span data-stu-id="e1c00-217">Confirming the methods produce the correct `IActionResult`.</span></span>
* <span data-ttu-id="e1c00-218">正在检查属性的值分配都正确。</span><span class="sxs-lookup"><span data-stu-id="e1c00-218">Checking that property value assignments are made correctly.</span></span>

<span data-ttu-id="e1c00-219">测试此组通常模拟 DAL 来生成 Act 步骤执行页模型方法的预期的数据的方法。</span><span class="sxs-lookup"><span data-stu-id="e1c00-219">This group of tests often mock the methods of the DAL to produce expected data for the Act step where a page model method is executed.</span></span> <span data-ttu-id="e1c00-220">例如，`GetMessagesAsync`方法`AppDbContext`模拟以生成输出。</span><span class="sxs-lookup"><span data-stu-id="e1c00-220">For example, the `GetMessagesAsync` method of the `AppDbContext` is mocked to produce output.</span></span> <span data-ttu-id="e1c00-221">当页模型方法执行此方法时，模型将返回的结果。</span><span class="sxs-lookup"><span data-stu-id="e1c00-221">When a page model method executes this method, the mock returns the result.</span></span> <span data-ttu-id="e1c00-222">数据不会从数据库中。</span><span class="sxs-lookup"><span data-stu-id="e1c00-222">The data doesn't come from the database.</span></span> <span data-ttu-id="e1c00-223">这将创建在页模型测试中使用 DAL 的可预测、 可靠的测试条件。</span><span class="sxs-lookup"><span data-stu-id="e1c00-223">This creates predictable, reliable test conditions for using the DAL in the page model tests.</span></span>

<span data-ttu-id="e1c00-224">`OnGetAsync_PopulatesThePageModel_WithAListOfMessages`测试显示如何`GetMessagesAsync`方法模拟的页面模型：</span><span class="sxs-lookup"><span data-stu-id="e1c00-224">The `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test shows how the `GetMessagesAsync` method is mocked for the page model:</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="e1c00-225">当`OnGetAsync`Act 步骤中执行方法时，它调用页模型`GetMessagesAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="e1c00-225">When the `OnGetAsync` method is executed in the Act step, it calls the page model's `GetMessagesAsync` method.</span></span>

<span data-ttu-id="e1c00-226">单元测试 Act 步骤 (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*):</span><span class="sxs-lookup"><span data-stu-id="e1c00-226">Unit test Act step (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet2)]

<span data-ttu-id="e1c00-227">`IndexPage` 页模型`OnGetAsync`方法 (*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="e1c00-227">`IndexPage` page model's `OnGetAsync` method (*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/src/RazorPagesTestingSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

<span data-ttu-id="e1c00-228">`GetMessagesAsync`中 DAL 方法不返回此方法调用的结果。</span><span class="sxs-lookup"><span data-stu-id="e1c00-228">The `GetMessagesAsync` method in the DAL doesn't return the result for this method call.</span></span> <span data-ttu-id="e1c00-229">该方法的模拟的版本返回的结果。</span><span class="sxs-lookup"><span data-stu-id="e1c00-229">The mocked version of the method returns the result.</span></span>

<span data-ttu-id="e1c00-230">在`Assert`步骤，实际的消息 (`actualMessages`) 从分配`Messages`页模型的属性。</span><span class="sxs-lookup"><span data-stu-id="e1c00-230">In the `Assert` step, the actual messages (`actualMessages`) are assigned from the `Messages` property of the page model.</span></span> <span data-ttu-id="e1c00-231">在将消息分配时也执行类型检查。</span><span class="sxs-lookup"><span data-stu-id="e1c00-231">A type check is also performed when the messages are assigned.</span></span> <span data-ttu-id="e1c00-232">通过进行比较的预期和实际的消息其`Text`属性。</span><span class="sxs-lookup"><span data-stu-id="e1c00-232">The expected and actual messages are compared by their `Text` properties.</span></span> <span data-ttu-id="e1c00-233">测试断言，这两个`List<Message>`实例包含对相同消息。</span><span class="sxs-lookup"><span data-stu-id="e1c00-233">The test asserts that the two `List<Message>` instances contain the same messages.</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet3)]

<span data-ttu-id="e1c00-234">此组中的其他测试创建页包括的模型对象`DefaultHttpContext`、 `ModelStateDictionary`、`ActionContext`建立`PageContext`、 `ViewDataDictionary`，和一个`PageContext`。</span><span class="sxs-lookup"><span data-stu-id="e1c00-234">Other tests in this group create page model objects that include the `DefaultHttpContext`, the `ModelStateDictionary`, an `ActionContext` to establish the `PageContext`, a `ViewDataDictionary`, and a `PageContext`.</span></span> <span data-ttu-id="e1c00-235">这些可用于执行测试。</span><span class="sxs-lookup"><span data-stu-id="e1c00-235">These are useful in conducting tests.</span></span> <span data-ttu-id="e1c00-236">例如，消息应用程序建立`ModelState`错误`AddModelError`检查是否是有效`PageResult`时返回`OnPostAddMessageAsync`执行：</span><span class="sxs-lookup"><span data-stu-id="e1c00-236">For example, the message app establishes a `ModelState` error with `AddModelError` to check that a valid `PageResult` is returned when `OnPostAddMessageAsync` is executed:</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="integration-testing-the-app"></a><span data-ttu-id="e1c00-237">测试应用程序的集成</span><span class="sxs-lookup"><span data-stu-id="e1c00-237">Integration testing the app</span></span>

<span data-ttu-id="e1c00-238">集成测试在测试应用程序的组件配合工作的焦点。</span><span class="sxs-lookup"><span data-stu-id="e1c00-238">The integration tests focus on testing that the app's components work together.</span></span> <span data-ttu-id="e1c00-239">集成测试使用执行[ASP.NET 核心测试主机](xref:testing/integration-testing#the-test-host)。</span><span class="sxs-lookup"><span data-stu-id="e1c00-239">Integration tests are conducted using the [ASP.NET Core Test Host](xref:testing/integration-testing#the-test-host).</span></span> <span data-ttu-id="e1c00-240">完整的请求-响应生命周期处理进行测试。</span><span class="sxs-lookup"><span data-stu-id="e1c00-240">Full request-response lifecycle processing is tested.</span></span> <span data-ttu-id="e1c00-241">这些测试断言页生成正确的状态代码和`Location`标头，如果设置。</span><span class="sxs-lookup"><span data-stu-id="e1c00-241">These tests assert that the page produces the correct status code and `Location` header, if set.</span></span>

<span data-ttu-id="e1c00-242">集成测试的示例的示例检查请求的消息应用程序索引页的结果 (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):</span><span class="sxs-lookup"><span data-stu-id="e1c00-242">An integration testing example from the sample checks the result of requesting the Index page of the message app (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet1)]

<span data-ttu-id="e1c00-243">没有排列步。</span><span class="sxs-lookup"><span data-stu-id="e1c00-243">There's no Arrange step.</span></span> <span data-ttu-id="e1c00-244">`GetAsync`方法调用`HttpClient`将 GET 请求发送到终结点。</span><span class="sxs-lookup"><span data-stu-id="e1c00-244">The `GetAsync` method is called on the `HttpClient` to send a GET request to the endpoint.</span></span> <span data-ttu-id="e1c00-245">测试断言，结果是 200 OK 状态代码。</span><span class="sxs-lookup"><span data-stu-id="e1c00-245">The test asserts that the result is a 200-OK status code.</span></span>

<span data-ttu-id="e1c00-246">对消息应用任何 POST 请求必须满足 antiforgery 检查自动是由应用程序的开发[数据保护 antiforgery 系统](xref:security/data-protection/introduction)。</span><span class="sxs-lookup"><span data-stu-id="e1c00-246">Any POST request to the message app must satisfy the antiforgery check that's automatically made by the app's [data protection antiforgery system](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="e1c00-247">若要安排某个测试的 POST 请求，测试应用程序必须：</span><span class="sxs-lookup"><span data-stu-id="e1c00-247">In order to arrange for a test's POST request, the test app must:</span></span>

1. <span data-ttu-id="e1c00-248">请对页的请求。</span><span class="sxs-lookup"><span data-stu-id="e1c00-248">Make a request for the page.</span></span>
1. <span data-ttu-id="e1c00-249">分析 antiforgery cookie 和响应的请求验证令牌。</span><span class="sxs-lookup"><span data-stu-id="e1c00-249">Parse the antiforgery cookie and request validation token from the response.</span></span>
1. <span data-ttu-id="e1c00-250">到位，请使用 antiforgery 的 cookie 与请求验证的 POST 请求令牌。</span><span class="sxs-lookup"><span data-stu-id="e1c00-250">Make the POST request with the antiforgery cookie and request validation token in place.</span></span>

<span data-ttu-id="e1c00-251">`Post_AddMessageHandler_ReturnsRedirectToRoot`测试方法：</span><span class="sxs-lookup"><span data-stu-id="e1c00-251">The `Post_AddMessageHandler_ReturnsRedirectToRoot` test method:</span></span>

* <span data-ttu-id="e1c00-252">准备一条消息和`HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="e1c00-252">Prepares a message and the `HttpClient`.</span></span>
* <span data-ttu-id="e1c00-253">向应用程序发出的 POST 请求。</span><span class="sxs-lookup"><span data-stu-id="e1c00-253">Makes a POST request to the app.</span></span>
* <span data-ttu-id="e1c00-254">检查响应重定向回索引页面。</span><span class="sxs-lookup"><span data-stu-id="e1c00-254">Checks the response is a redirect back to the Index page.</span></span>

<span data-ttu-id="e1c00-255">`Post_AddMessageHandler_ReturnsRedirectToRoot ` 方法 (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):</span><span class="sxs-lookup"><span data-stu-id="e1c00-255">`Post_AddMessageHandler_ReturnsRedirectToRoot ` method (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet2)]

<span data-ttu-id="e1c00-256">`GetRequestContentAsync`实用工具方法管理准备客户端使用 antiforgery cookie 和请求验证令牌。</span><span class="sxs-lookup"><span data-stu-id="e1c00-256">The `GetRequestContentAsync` utility method manages preparing the client with the antiforgery cookie and request verification token.</span></span> <span data-ttu-id="e1c00-257">请注意如何方法接收`IDictionary`允许调用测试方法通过数据用于编码以及请求验证令牌的请求中 (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):</span><span class="sxs-lookup"><span data-stu-id="e1c00-257">Note how the method receives an `IDictionary` that permits the calling test method to pass in data for the request to encode along with the request verification token (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet2&highlight=1-2,8-9,29)]

<span data-ttu-id="e1c00-258">集成测试还可以将错误数据传递给应用程序以测试应用程序的响应行为。</span><span class="sxs-lookup"><span data-stu-id="e1c00-258">Integration tests can also pass bad data to the app to test the app's response behavior.</span></span> <span data-ttu-id="e1c00-259">消息应用程序限制到 200 个字符的消息长度 (*src/RazorPagesTestingSample/Data/Message.cs*):</span><span class="sxs-lookup"><span data-stu-id="e1c00-259">The message app limits message length to 200 characters (*src/RazorPagesTestingSample/Data/Message.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/Message.cs?name=snippet1&highlight=7)]

<span data-ttu-id="e1c00-260">`Post_AddMessageHandler_ReturnsSuccess_WhenMessageTextTooLong`测试`Message`显式传递中带有 201"X"字符的文本。</span><span class="sxs-lookup"><span data-stu-id="e1c00-260">The `Post_AddMessageHandler_ReturnsSuccess_WhenMessageTextTooLong` test `Message` explicitly passes in text with 201 "X" characters.</span></span> <span data-ttu-id="e1c00-261">这会导致`ModelState`错误。</span><span class="sxs-lookup"><span data-stu-id="e1c00-261">This results in a `ModelState` error.</span></span> <span data-ttu-id="e1c00-262">发布不重定向回索引页面。</span><span class="sxs-lookup"><span data-stu-id="e1c00-262">The POST doesn't redirect back to the Index page.</span></span> <span data-ttu-id="e1c00-263">它将返回 200 OK 与`null``Location`标头 (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):</span><span class="sxs-lookup"><span data-stu-id="e1c00-263">It returns a 200-OK with a `null` `Location` header (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet3&highlight=7,16-17)]

## <a name="see-also"></a><span data-ttu-id="e1c00-264">请参阅</span><span class="sxs-lookup"><span data-stu-id="e1c00-264">See also</span></span>

* [<span data-ttu-id="e1c00-265">单元测试 C# 中使用 dotnet 测试和 xUnit 的.NET 核心</span><span class="sxs-lookup"><span data-stu-id="e1c00-265">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="e1c00-266">集成测试</span><span class="sxs-lookup"><span data-stu-id="e1c00-266">Integration testing</span></span>](xref:testing/integration-testing)
* [<span data-ttu-id="e1c00-267">测试控制器</span><span class="sxs-lookup"><span data-stu-id="e1c00-267">Testing controllers</span></span>](xref:mvc/controllers/testing)
* <span data-ttu-id="e1c00-268">[单元测试代码](/visualstudio/test/unit-test-your-code)(Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="e1c00-268">[Unit Test Your Code](/visualstudio/test/unit-test-your-code) (Visual Studio)</span></span>
* [<span data-ttu-id="e1c00-269">xUnit.net</span><span class="sxs-lookup"><span data-stu-id="e1c00-269">xUnit.net</span></span>](https://xunit.github.io/)
* [<span data-ttu-id="e1c00-270">入门 xUnit.net （.NET Core/ASP.NET 核）</span><span class="sxs-lookup"><span data-stu-id="e1c00-270">Getting started with xUnit.net (.NET Core/ASP.NET Core)</span></span>](https://xunit.github.io/docs/getting-started-dotnet-core)
* [<span data-ttu-id="e1c00-271">Moq</span><span class="sxs-lookup"><span data-stu-id="e1c00-271">Moq</span></span>](https://github.com/moq/moq4)
* [<span data-ttu-id="e1c00-272">Moq 快速入门</span><span class="sxs-lookup"><span data-stu-id="e1c00-272">Moq Quickstart</span></span>](https://github.com/Moq/moq4/wiki/Quickstart)
