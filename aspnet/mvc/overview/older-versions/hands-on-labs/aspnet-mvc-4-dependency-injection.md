---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: "ASP.NET MVC 4 依赖关系注入 |Microsoft 文档"
author: rick-anderson
description: "注意： 此动手实验假定你具有的 ASP.NET MVC 和 ASP.NET MVC 4 的筛选器的基本知识。 如果你没有使用 ASP.NET MVC 4 筛选器之前，我们建议..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: af4967f642ba4615f3392c0c404d2ec62edaaae8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="90dcf-104">ASP.NET MVC 4 依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="90dcf-104">ASP.NET MVC 4 Dependency Injection</span></span>
====================
<span data-ttu-id="90dcf-105">通过[Web 营地团队](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="90dcf-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> [!NOTE]
> <span data-ttu-id="90dcf-106">此动手实验假定你具有的基础知识**ASP.NET MVC**和**ASP.NET MVC 4 筛选**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-106">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="90dcf-107">如果您未使用过**ASP.NET MVC 4 筛选**之前，我们建议你转到**ASP.NET MVC 自定义操作筛选器**动手实验。</span><span class="sxs-lookup"><span data-stu-id="90dcf-107">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>
> 
> <span data-ttu-id="90dcf-108">在 Web 营地培训工具包中，在包括所有的示例代码和代码段[https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843)。</span><span class="sxs-lookup"><span data-stu-id="90dcf-108">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span></span>


<span data-ttu-id="90dcf-109">在**对象面向编程**范例，对象协同工作的协作模型其中有 contributors （参与者） 和使用者。</span><span class="sxs-lookup"><span data-stu-id="90dcf-109">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="90dcf-110">当然，此通信模型生成对象和组件，变得难以管理复杂性增加时之间的依赖关系。</span><span class="sxs-lookup"><span data-stu-id="90dcf-110">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="90dcf-111">![类依赖项和模型复杂性](aspnet-mvc-4-dependency-injection/_static/image1.png "类依赖项和模型复杂性")</span><span class="sxs-lookup"><span data-stu-id="90dcf-111">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="90dcf-112">*类依赖项和模型复杂性*</span><span class="sxs-lookup"><span data-stu-id="90dcf-112">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="90dcf-113">你可能听说**工厂模式**和接口和实现使用服务，往往负责服务位置的客户端对象之间的分离。</span><span class="sxs-lookup"><span data-stu-id="90dcf-113">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="90dcf-114">依赖关系注入模式是控制反向的特定实现。</span><span class="sxs-lookup"><span data-stu-id="90dcf-114">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="90dcf-115">**反向 (IoC) 控件的**意味着对象不会创建其他对象，它们依赖来完成其工作。</span><span class="sxs-lookup"><span data-stu-id="90dcf-115">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="90dcf-116">相反，它们获得所需从外部源 （例如，xml 配置文件） 的对象。</span><span class="sxs-lookup"><span data-stu-id="90dcf-116">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="90dcf-117">**依赖关系注入 (DI)**意味着这是通过无需对象干预，通常将构造函数参数传递一个 framework 组件，设置属性。</span><span class="sxs-lookup"><span data-stu-id="90dcf-117">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="90dcf-118">依赖关系注入 (DI) 设计模式</span><span class="sxs-lookup"><span data-stu-id="90dcf-118">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="90dcf-119">在高级别中，依赖关系注入的目的在于客户端类 (例如*golfer*) 需要满足接口的内容 (例如*IClub*)。</span><span class="sxs-lookup"><span data-stu-id="90dcf-119">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="90dcf-120">它不介意具体类型是什么 (例如*WoodClub，IronClub，WedgeClub*或*PutterClub*)，它想其他人来处理这种情况 (例如良好*托盘*)。</span><span class="sxs-lookup"><span data-stu-id="90dcf-120">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="90dcf-121">ASP.NET mvc 依赖项解析程序可以让你能够注册你的依赖项逻辑的其他位置 (例如容器或*俱乐部包*)。</span><span class="sxs-lookup"><span data-stu-id="90dcf-121">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="90dcf-122">![依赖关系注入图](aspnet-mvc-4-dependency-injection/_static/image2.png "依赖关系注入图")</span><span class="sxs-lookup"><span data-stu-id="90dcf-122">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="90dcf-123">*依赖关系注入-高尔夫类比*</span><span class="sxs-lookup"><span data-stu-id="90dcf-123">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="90dcf-124">使用依赖关系注入模式和控制反向的优点如下所示：</span><span class="sxs-lookup"><span data-stu-id="90dcf-124">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="90dcf-125">减少类耦合</span><span class="sxs-lookup"><span data-stu-id="90dcf-125">Reduces class coupling</span></span>
- <span data-ttu-id="90dcf-126">会增加代码重用</span><span class="sxs-lookup"><span data-stu-id="90dcf-126">Increases code reusing</span></span>
- <span data-ttu-id="90dcf-127">提高代码可维护性</span><span class="sxs-lookup"><span data-stu-id="90dcf-127">Improves code maintainability</span></span>
- <span data-ttu-id="90dcf-128">改进了应用程序测试</span><span class="sxs-lookup"><span data-stu-id="90dcf-128">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="90dcf-129">依赖关系注入有时与抽象工厂设计模式中，但这两种方法之间存在细微的差异。</span><span class="sxs-lookup"><span data-stu-id="90dcf-129">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="90dcf-130">DI 具有后面致力于解决通过调用工厂和注册的服务的依赖关系的框架。</span><span class="sxs-lookup"><span data-stu-id="90dcf-130">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="90dcf-131">现在，你已了解依赖关系注入模式，你将学习在本实验整个如何将其应用于 ASP.NET MVC 4。</span><span class="sxs-lookup"><span data-stu-id="90dcf-131">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="90dcf-132">你将开始使用中的依赖关系注入**控制器**中包含数据库访问服务。</span><span class="sxs-lookup"><span data-stu-id="90dcf-132">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="90dcf-133">接下来，将应用到的依赖关系注入**视图**使用服务并显示信息。</span><span class="sxs-lookup"><span data-stu-id="90dcf-133">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="90dcf-134">最后，你将扩展 DI 到 ASP.NET MVC 4 筛选器，将注入解决方案中的自定义操作筛选器。</span><span class="sxs-lookup"><span data-stu-id="90dcf-134">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="90dcf-135">在本动手实验中，你将了解如何：</span><span class="sxs-lookup"><span data-stu-id="90dcf-135">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="90dcf-136">将 ASP.NET MVC 4 与 Unity 集成为使用 NuGet 包的依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="90dcf-136">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="90dcf-137">使用一个 ASP.NET MVC 控制器内部的依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="90dcf-137">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="90dcf-138">使用 ASP.NET MVC 视图内的依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="90dcf-138">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="90dcf-139">使用 ASP.NET MVC 操作筛选器内的依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="90dcf-139">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="90dcf-140">此实验室使用的依赖项解析为 Unity.Mvc3 NuGet 包，但可以调整任何依赖关系注入框架，用于 ASP.NET MVC 4。</span><span class="sxs-lookup"><span data-stu-id="90dcf-140">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="90dcf-141">先决条件</span><span class="sxs-lookup"><span data-stu-id="90dcf-141">Prerequisites</span></span>

<span data-ttu-id="90dcf-142">你必须具有要完成本实验的以下项：</span><span class="sxs-lookup"><span data-stu-id="90dcf-142">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="90dcf-143">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更高 (读取[附录 A](#AppendixA)有关如何安装它的说明)。</span><span class="sxs-lookup"><span data-stu-id="90dcf-143">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="90dcf-144">安装</span><span class="sxs-lookup"><span data-stu-id="90dcf-144">Setup</span></span>

<span data-ttu-id="90dcf-145">**安装代码片段**</span><span class="sxs-lookup"><span data-stu-id="90dcf-145">**Installing Code Snippets**</span></span>

<span data-ttu-id="90dcf-146">为方便起见，你将沿此实验室管理大部分都是代码的可用作 Visual Studio 代码段。</span><span class="sxs-lookup"><span data-stu-id="90dcf-146">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="90dcf-147">若要安装运行的代码段**.\Source\Setup\CodeSnippets.vsi**文件。</span><span class="sxs-lookup"><span data-stu-id="90dcf-147">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="90dcf-148">如果你不熟悉 Visual Studio 代码段，并想要了解如何使用它们，你可以从该文档引用的附录&quot;[附录 b： 使用代码段](#AppendixB)&quot;。</span><span class="sxs-lookup"><span data-stu-id="90dcf-148">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="90dcf-149">练习</span><span class="sxs-lookup"><span data-stu-id="90dcf-149">Exercises</span></span>

<span data-ttu-id="90dcf-150">此动手实验包含通过在以下练习：</span><span class="sxs-lookup"><span data-stu-id="90dcf-150">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="90dcf-151">练习 1： 将注入控制器</span><span class="sxs-lookup"><span data-stu-id="90dcf-151">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="90dcf-152">练习 2： 将注入视图</span><span class="sxs-lookup"><span data-stu-id="90dcf-152">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="90dcf-153">练习 3： 将注入筛选器</span><span class="sxs-lookup"><span data-stu-id="90dcf-153">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="90dcf-154">每个练习均附带由**结束**包含生成您应该完成练习后获得的解决方案文件夹。</span><span class="sxs-lookup"><span data-stu-id="90dcf-154">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="90dcf-155">如果你需要通过在练习工作的更多帮助，可以使用此解决方案作为指南。</span><span class="sxs-lookup"><span data-stu-id="90dcf-155">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="90dcf-156">估计时间来完成该实验： **30 分钟**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-156">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="90dcf-157">练习 1： 将注入控制器</span><span class="sxs-lookup"><span data-stu-id="90dcf-157">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="90dcf-158">在此练习中，你将了解如何在 ASP.NET MVC 控制器中的依赖关系注入使用通过集成 Unity 使用 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="90dcf-158">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="90dcf-159">为此，将逻辑与数据访问分开你 MvcMusicStore 控制器包括服务。</span><span class="sxs-lookup"><span data-stu-id="90dcf-159">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="90dcf-160">服务将在控制器构造函数，它将使用的帮助的依赖关系注入解决中创建新的依赖关系**Unity**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-160">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="90dcf-161">此方法将显示如何生成不太耦合应用程序、 更灵活且易于维护和测试。</span><span class="sxs-lookup"><span data-stu-id="90dcf-161">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="90dcf-162">你还将了解如何将与 Unity 集成 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="90dcf-162">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="90dcf-163">有关 StoreManager 服务</span><span class="sxs-lookup"><span data-stu-id="90dcf-163">About StoreManager Service</span></span>

<span data-ttu-id="90dcf-164">现在提供开始解决方案中的 MVC 音乐存储包含管理名为的存储控制器数据的服务**StoreService**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-164">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="90dcf-165">你将在下面找到存储服务实现。</span><span class="sxs-lookup"><span data-stu-id="90dcf-165">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="90dcf-166">请注意，所有方法将都返回模型的实体。</span><span class="sxs-lookup"><span data-stu-id="90dcf-166">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="90dcf-167">**StoreController**开始新的解决方案现在将使用来自**StoreService**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-167">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="90dcf-168">从中删除所有数据引用了**StoreController**，现在可以修改当前的数据访问接口，而无需更改任何使用的方法和**StoreService**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-168">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="90dcf-169">你将在下面找到**StoreController**实现与具有依赖关系**StoreService**内类构造函数。</span><span class="sxs-lookup"><span data-stu-id="90dcf-169">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="90dcf-170">在本练习中引入的依赖关系与**控制反向**(IoC)。</span><span class="sxs-lookup"><span data-stu-id="90dcf-170">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="90dcf-171">**StoreController**类构造函数接收**IStoreService**至关重要的是要执行从在类中的服务调用的类型参数。</span><span class="sxs-lookup"><span data-stu-id="90dcf-171">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="90dcf-172">但是， **StoreController**未实现默认构造函数 （不带任何参数） 的任何控制器必须具有要使用 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="90dcf-172">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="90dcf-173">若要解决此依赖关系，控制器已由 （返回指定任何的类型对象的类） 的抽象工厂来创建。</span><span class="sxs-lookup"><span data-stu-id="90dcf-173">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="90dcf-174">当类尝试创建 StoreController 而不发送服务对象，因为没有声明没有无参数构造函数时，会出现错误。</span><span class="sxs-lookup"><span data-stu-id="90dcf-174">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="90dcf-175">任务 1-运行应用程序</span><span class="sxs-lookup"><span data-stu-id="90dcf-175">Task 1 - Running the Application</span></span>

<span data-ttu-id="90dcf-176">在此任务中，你将运行 Begin 应用程序，其中包括插入应用程序逻辑分离开来的数据访问的存储控制器的服务。</span><span class="sxs-lookup"><span data-stu-id="90dcf-176">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="90dcf-177">在运行应用程序，你将收到异常，为控制器服务不默认情况下通过作为参数：</span><span class="sxs-lookup"><span data-stu-id="90dcf-177">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="90dcf-178">打开**开始**解决方案位于**将 Source\Ex01 注入 Controller\Begin**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-178">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

    1. <span data-ttu-id="90dcf-179">你将需要下载一些缺少的 NuGet 程序包才能继续。</span><span class="sxs-lookup"><span data-stu-id="90dcf-179">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="90dcf-180">若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-180">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="90dcf-181">在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。</span><span class="sxs-lookup"><span data-stu-id="90dcf-181">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="90dcf-182">最后，通过单击生成解决方案**生成** | **生成解决方案**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-182">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="90dcf-183">使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。</span><span class="sxs-lookup"><span data-stu-id="90dcf-183">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="90dcf-184">使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。</span><span class="sxs-lookup"><span data-stu-id="90dcf-184">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="90dcf-185">这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。</span><span class="sxs-lookup"><span data-stu-id="90dcf-185">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="90dcf-186">按**Ctrl + F5**运行该应用程序而不进行调试。</span><span class="sxs-lookup"><span data-stu-id="90dcf-186">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="90dcf-187">你将收到错误消息&quot;**没有此对象定义的无参数构造函数**&quot;:</span><span class="sxs-lookup"><span data-stu-id="90dcf-187">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="90dcf-188">![运行 ASP.NET MVC 开始应用程序时出错](aspnet-mvc-4-dependency-injection/_static/image3.png "运行 ASP.NET MVC 开始应用程序时出错")</span><span class="sxs-lookup"><span data-stu-id="90dcf-188">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="90dcf-189">*运行 ASP.NET MVC 开始应用程序时出错*</span><span class="sxs-lookup"><span data-stu-id="90dcf-189">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="90dcf-190">关闭浏览器。</span><span class="sxs-lookup"><span data-stu-id="90dcf-190">Close the browser.</span></span>

<span data-ttu-id="90dcf-191">在以下步骤将处理音乐存储解决方案，将此控制器所需的依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="90dcf-191">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="90dcf-192">任务 2-到 MvcMusicStore 解决方案包括 Unity</span><span class="sxs-lookup"><span data-stu-id="90dcf-192">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="90dcf-193">在此任务中，将包括**Unity.Mvc3**到解决方案的 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="90dcf-193">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="90dcf-194">Unity.Mvc3 包旨在用于 ASP.NET MVC 3，但它是与 ASP.NET MVC 4 完全兼容。</span><span class="sxs-lookup"><span data-stu-id="90dcf-194">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="90dcf-195">Unity 实例是具有的可选支持的轻量、 可扩展的依赖关系注入容器，并键入截获。</span><span class="sxs-lookup"><span data-stu-id="90dcf-195">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="90dcf-196">它是在任何类型的.NET 应用程序中使用的通用容器。</span><span class="sxs-lookup"><span data-stu-id="90dcf-196">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="90dcf-197">它提供了在包括依赖关系注入机制中找到的所有常见功能： 对象创建、 抽象的通过指定运行时和灵活性，依赖项推迟到容器的组件配置的要求。</span><span class="sxs-lookup"><span data-stu-id="90dcf-197">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="90dcf-198">安装**Unity.Mvc3**中 NuGet 包**MvcMusicStore**项目。</span><span class="sxs-lookup"><span data-stu-id="90dcf-198">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="90dcf-199">若要执行此操作，打开**程序包管理器控制台**从**视图** | **其他窗口**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-199">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="90dcf-200">运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="90dcf-200">Run the following command.</span></span>

    <span data-ttu-id="90dcf-201">PMC</span><span class="sxs-lookup"><span data-stu-id="90dcf-201">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="90dcf-202">![安装 Unity.Mvc3 NuGet 包](aspnet-mvc-4-dependency-injection/_static/image4.png "安装 Unity.Mvc3 NuGet 包")</span><span class="sxs-lookup"><span data-stu-id="90dcf-202">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="90dcf-203">*安装 Unity.Mvc3 NuGet 包*</span><span class="sxs-lookup"><span data-stu-id="90dcf-203">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="90dcf-204">一次**Unity.Mvc3**安装包，请浏览的文件和文件夹，它会自动添加为了简化 Unity 配置。</span><span class="sxs-lookup"><span data-stu-id="90dcf-204">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="90dcf-205">![安装的 Unity.Mvc3 程序包](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 包安装")</span><span class="sxs-lookup"><span data-stu-id="90dcf-205">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="90dcf-206">*Unity.Mvc3 包安装*</span><span class="sxs-lookup"><span data-stu-id="90dcf-206">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="90dcf-207">任务 3-在 Global.asax.cs 应用程序中注册 Unity\_启动</span><span class="sxs-lookup"><span data-stu-id="90dcf-207">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="90dcf-208">在此任务中，你将更新**应用程序\_启动**方法位于**Global.asax.cs**调用 Unity 引导程序初始值设定项，然后，更新引导程序文件注册服务和控制器将用于依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="90dcf-208">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="90dcf-209">现在，你将挂钩引导程序，这是初始化 Unity 容器的文件和依赖项解析程序。</span><span class="sxs-lookup"><span data-stu-id="90dcf-209">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="90dcf-210">若要执行此操作，打开**Global.asax.cs**并添加以下突出显示的代码中**应用程序\_启动**方法。</span><span class="sxs-lookup"><span data-stu-id="90dcf-210">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="90dcf-211">(代码段- *ASP.NET 依赖关系注入实验-Ex01-初始化 Unity*)</span><span class="sxs-lookup"><span data-stu-id="90dcf-211">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="90dcf-212">打开**Bootstrapper.cs**文件。</span><span class="sxs-lookup"><span data-stu-id="90dcf-212">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="90dcf-213">包括以下命名空间： **MvcMusicStore.Services**和**MusicStore.Controllers**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-213">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="90dcf-214">(代码段- *ASP.NET 依赖关系注入实验-Ex01-引导程序添加命名空间*)</span><span class="sxs-lookup"><span data-stu-id="90dcf-214">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="90dcf-215">替换**BuildUnityContainer**方法的内容替换为以下代码来注册存储控制器和存储服务。</span><span class="sxs-lookup"><span data-stu-id="90dcf-215">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="90dcf-216">(代码段- *ASP.NET 依赖关系注入实验-Ex01-注册存储控制器和服务*)</span><span class="sxs-lookup"><span data-stu-id="90dcf-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="90dcf-217">任务 4-运行应用程序</span><span class="sxs-lookup"><span data-stu-id="90dcf-217">Task 4 - Running the Application</span></span>

<span data-ttu-id="90dcf-218">在此任务中，你将运行应用程序以验证它现在在加载包括 Unity 之后。</span><span class="sxs-lookup"><span data-stu-id="90dcf-218">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="90dcf-219">按**F5**运行该应用程序，应用程序应现在加载而不显示任何错误消息。</span><span class="sxs-lookup"><span data-stu-id="90dcf-219">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="90dcf-220">![使用依赖关系注入运行应用程序](aspnet-mvc-4-dependency-injection/_static/image6.png "使用依赖关系注入运行应用程序")</span><span class="sxs-lookup"><span data-stu-id="90dcf-220">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="90dcf-221">*使用依赖关系注入运行应用程序*</span><span class="sxs-lookup"><span data-stu-id="90dcf-221">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="90dcf-222">浏览到**应用商店**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-222">Browse to **/Store**.</span></span> <span data-ttu-id="90dcf-223">这将调用**StoreController**，现在为创建使用**Unity**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-223">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="90dcf-224">![MVC 音乐商店](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC 音乐商店")</span><span class="sxs-lookup"><span data-stu-id="90dcf-224">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="90dcf-225">*MVC 音乐商店*</span><span class="sxs-lookup"><span data-stu-id="90dcf-225">*MVC Music Store*</span></span>
3. <span data-ttu-id="90dcf-226">关闭浏览器。</span><span class="sxs-lookup"><span data-stu-id="90dcf-226">Close the browser.</span></span>

<span data-ttu-id="90dcf-227">在以下练习中，您将学习如何扩展使用在 ASP.NET MVC 视图和操作筛选器内的依赖关系注入作用域。</span><span class="sxs-lookup"><span data-stu-id="90dcf-227">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="90dcf-228">练习 2： 将注入视图</span><span class="sxs-lookup"><span data-stu-id="90dcf-228">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="90dcf-229">在此练习中，你将学习如何使用具有 ASP.NET MVC 4 的新功能的视图中的依赖关系注入程序 Unity 集成。</span><span class="sxs-lookup"><span data-stu-id="90dcf-229">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="90dcf-230">若要做到这一点，您将调用自定义服务内存储浏览视图中，其中将显示一条消息和下图。</span><span class="sxs-lookup"><span data-stu-id="90dcf-230">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="90dcf-231">然后，将与 Unity 集成项目，并创建自定义的依赖项解析程序将依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="90dcf-231">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="90dcf-232">任务 1-创建使用服务的视图</span><span class="sxs-lookup"><span data-stu-id="90dcf-232">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="90dcf-233">在此任务中，你将创建执行服务调用，以生成新的依赖关系的视图。</span><span class="sxs-lookup"><span data-stu-id="90dcf-233">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="90dcf-234">服务包含在此解决方案中包含一个简单的消息传递服务。</span><span class="sxs-lookup"><span data-stu-id="90dcf-234">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="90dcf-235">打开**开始**解决方案位于**将 Source\Ex02 注入 View\Begin**文件夹。</span><span class="sxs-lookup"><span data-stu-id="90dcf-235">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="90dcf-236">否则，可能会继续使用**结束**解决方案获取通过完成上一练习。</span><span class="sxs-lookup"><span data-stu-id="90dcf-236">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="90dcf-237">如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。</span><span class="sxs-lookup"><span data-stu-id="90dcf-237">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="90dcf-238">若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-238">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="90dcf-239">在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。</span><span class="sxs-lookup"><span data-stu-id="90dcf-239">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="90dcf-240">最后，通过单击生成解决方案**生成** | **生成解决方案**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-240">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="90dcf-241">使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。</span><span class="sxs-lookup"><span data-stu-id="90dcf-241">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="90dcf-242">使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。</span><span class="sxs-lookup"><span data-stu-id="90dcf-242">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="90dcf-243">这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。</span><span class="sxs-lookup"><span data-stu-id="90dcf-243">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="90dcf-244">有关详细信息，请参阅此文章： [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)。</span><span class="sxs-lookup"><span data-stu-id="90dcf-244">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="90dcf-245">包括**MessageService.cs**和**IMessageService.cs**类位于**源 \Assets**文件夹中的**/**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-245">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="90dcf-246">要执行此操作，请右键单击**服务**文件夹，然后选择**添加现有项**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-246">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="90dcf-247">浏览到文件的位置，并将其包含。</span><span class="sxs-lookup"><span data-stu-id="90dcf-247">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="90dcf-248">![添加消息服务和服务接口](aspnet-mvc-4-dependency-injection/_static/image8.png "添加消息服务和服务接口")</span><span class="sxs-lookup"><span data-stu-id="90dcf-248">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="90dcf-249">*添加消息服务和服务接口*</span><span class="sxs-lookup"><span data-stu-id="90dcf-249">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="90dcf-250">**IMessageService**接口定义两个属性由实现**MessageService**类。</span><span class="sxs-lookup"><span data-stu-id="90dcf-250">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="90dcf-251">这些属性-**消息**和**ImageUrl**-存储要显示的消息和图像的 URL。</span><span class="sxs-lookup"><span data-stu-id="90dcf-251">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="90dcf-252">创建文件夹**/页**在项目的根文件夹，并将现有类**MyBasePage.cs**从**Source\Assets**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-252">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="90dcf-253">将继承从基本页具有以下结构。</span><span class="sxs-lookup"><span data-stu-id="90dcf-253">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="90dcf-254">![Pages 文件夹](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages 文件夹")</span><span class="sxs-lookup"><span data-stu-id="90dcf-254">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="90dcf-255">打开**Browse.cshtml**查看从**/视图/存储**文件夹，并使其从继承**MyBasePage.cs**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-255">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="90dcf-256">在**浏览**视图中，添加对的调用**MessageService**要显示的映像和服务检索一条消息。</span><span class="sxs-lookup"><span data-stu-id="90dcf-256">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
<span data-ttu-id="90dcf-257">(C#)</span><span class="sxs-lookup"><span data-stu-id="90dcf-257">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="90dcf-258">任务 2-包括自定义的依赖项解析程序和自定义视图页激活器</span><span class="sxs-lookup"><span data-stu-id="90dcf-258">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="90dcf-259">在上一任务中，你将插入的视图，用于执行其内部服务调用在新的依赖关系。</span><span class="sxs-lookup"><span data-stu-id="90dcf-259">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="90dcf-260">现在，你将通过实现 ASP.NET MVC 依赖关系注入接口来解决该依赖关系**IViewPageActivator**和**IDependencyResolver**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-260">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="90dcf-261">你将在解决方案中包括的实现**IDependencyResolver** ，将通过使用 Unity 处理服务检索。</span><span class="sxs-lookup"><span data-stu-id="90dcf-261">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="90dcf-262">然后，你将包括的另一个自定义实现**IViewPageActivator**将解决的视图创建的接口。</span><span class="sxs-lookup"><span data-stu-id="90dcf-262">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="90dcf-263">ASP.NET MVC 3 自依赖关系注入的实现已简化这些接口，以便注册服务。</span><span class="sxs-lookup"><span data-stu-id="90dcf-263">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="90dcf-264">**IDependencyResolver**和**IViewPageActivator**属于依赖关系注入的 ASP.NET MVC 3 功能。</span><span class="sxs-lookup"><span data-stu-id="90dcf-264">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="90dcf-265">**-IDependencyResolver**接口替换以前 IMvcServiceLocator。</span><span class="sxs-lookup"><span data-stu-id="90dcf-265">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="90dcf-266">IDependencyResolver 的实施者必须返回的服务或服务集合的实例。</span><span class="sxs-lookup"><span data-stu-id="90dcf-266">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
>
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="90dcf-267">**-IViewPageActivator**接口提供了更好地控制细化查看页通过依赖关系注入实例化的方式。</span><span class="sxs-lookup"><span data-stu-id="90dcf-267">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="90dcf-268">实现的类**IViewPageActivator**接口可以创建使用上下文信息的查看实例。</span><span class="sxs-lookup"><span data-stu-id="90dcf-268">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="90dcf-269">创建 /**工厂**项目的根文件夹中的文件夹。</span><span class="sxs-lookup"><span data-stu-id="90dcf-269">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="90dcf-270">包括**CustomViewPageActivator.cs**到你的解决方案从**/源/资产/**到**工厂**文件夹。</span><span class="sxs-lookup"><span data-stu-id="90dcf-270">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="90dcf-271">为此，请右键单击**/Factories**文件夹，选择**添加 |现有项**，然后选择**CustomViewPageActivator.cs**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-271">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="90dcf-272">此类实现**IViewPageActivator**接口来保存 Unity 容器。</span><span class="sxs-lookup"><span data-stu-id="90dcf-272">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="90dcf-273">**CustomViewPageActivator**负责使用 Unity 容器管理视图的创建。</span><span class="sxs-lookup"><span data-stu-id="90dcf-273">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="90dcf-274">包括**UnityDependencyResolver.cs**文件从**/源/资产**到**/Factories**文件夹。</span><span class="sxs-lookup"><span data-stu-id="90dcf-274">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="90dcf-275">为此，请右键单击**/Factories**文件夹，选择**添加 |现有项**，然后选择**UnityDependencyResolver.cs**文件。</span><span class="sxs-lookup"><span data-stu-id="90dcf-275">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="90dcf-276">**UnityDependencyResolver**类是自定义 DependencyResolver for Unity。</span><span class="sxs-lookup"><span data-stu-id="90dcf-276">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="90dcf-277">当 Unity 容器中找不到服务时，被 invocated 基冲突解决程序。</span><span class="sxs-lookup"><span data-stu-id="90dcf-277">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="90dcf-278">在下面的任务将注册这两个实现，以便知道服务和视图的位置的模型。</span><span class="sxs-lookup"><span data-stu-id="90dcf-278">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="90dcf-279">任务 3-在 Unity 容器内注册依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="90dcf-279">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="90dcf-280">在此任务中，您将放入所有以前的内容在一起以进行工作的依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="90dcf-280">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="90dcf-281">到目前为止你的解决方案具有以下元素：</span><span class="sxs-lookup"><span data-stu-id="90dcf-281">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="90dcf-282">A**浏览**继承自的视图**MyBaseClass**和使用**MessageService**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-282">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="90dcf-283">中间类-**MyBaseClass**-具有依赖关系注入声明服务接口。</span><span class="sxs-lookup"><span data-stu-id="90dcf-283">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="90dcf-284">服务- **MessageService** -及其界面**IMessageService**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-284">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="90dcf-285">Unity 的自定义的依赖项解析程序**UnityDependencyResolver** -服务检索的处理。</span><span class="sxs-lookup"><span data-stu-id="90dcf-285">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="90dcf-286">视图页中激活器- **CustomViewPageActivator** -创建页。</span><span class="sxs-lookup"><span data-stu-id="90dcf-286">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="90dcf-287">若要将注入**浏览**视图中，你将立即注册自定义的依赖项解析程序在 Unity 容器中。</span><span class="sxs-lookup"><span data-stu-id="90dcf-287">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="90dcf-288">打开**Bootstrapper.cs**文件。</span><span class="sxs-lookup"><span data-stu-id="90dcf-288">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="90dcf-289">注册实例**MessageService**到要初始化的服务的 Unity 容器：</span><span class="sxs-lookup"><span data-stu-id="90dcf-289">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="90dcf-290">(代码段- *ASP.NET 依赖关系注入实验-Ex02-注册消息服务*)</span><span class="sxs-lookup"><span data-stu-id="90dcf-290">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="90dcf-291">添加对的引用**MvcMusicStore.Factories**命名空间。</span><span class="sxs-lookup"><span data-stu-id="90dcf-291">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="90dcf-292">(代码段- *ASP.NET 依赖关系注入实验-Ex02-工厂 Namespace*)</span><span class="sxs-lookup"><span data-stu-id="90dcf-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="90dcf-293">注册**CustomViewPageActivator**到 Unity 容器视图页中激活器作为：</span><span class="sxs-lookup"><span data-stu-id="90dcf-293">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="90dcf-294">(代码段- *ASP.NET 依赖关系注入实验-Ex02-注册 CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="90dcf-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="90dcf-295">将替换的实例的 ASP.NET MVC 4 默认依赖项解析程序**UnityDependencyResolver**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-295">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="90dcf-296">若要执行此操作，请替换**初始化**方法内容替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="90dcf-296">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="90dcf-297">(代码段- *ASP.NET 依赖关系注入实验-Ex02-更新依赖项解析程序*)</span><span class="sxs-lookup"><span data-stu-id="90dcf-297">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="90dcf-298">ASP.NET MVC 提供的默认依赖项解析程序类。</span><span class="sxs-lookup"><span data-stu-id="90dcf-298">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="90dcf-299">若要使用与我们已为 unity 创建的自定义的依赖项解析程序，此解析程序具有要替换。</span><span class="sxs-lookup"><span data-stu-id="90dcf-299">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="90dcf-300">任务 4-运行应用程序</span><span class="sxs-lookup"><span data-stu-id="90dcf-300">Task 4 - Running the Application</span></span>

<span data-ttu-id="90dcf-301">在此任务中，你将运行应用程序以验证存储浏览器使用该服务，并说明的映像，以及检索到的消息：</span><span class="sxs-lookup"><span data-stu-id="90dcf-301">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="90dcf-302">按 **F5** 运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="90dcf-302">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="90dcf-303">单击**Rock**风格菜单和，请参阅内如何**MessageService**到视图而插入和加载在欢迎消息和图像。</span><span class="sxs-lookup"><span data-stu-id="90dcf-303">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="90dcf-304">在此示例中，我们正在输入到&quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="90dcf-304">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="90dcf-305">![MVC 音乐商店-视图注入](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC 音乐商店-视图注入")</span><span class="sxs-lookup"><span data-stu-id="90dcf-305">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="90dcf-306">*MVC 音乐商店-视图注入*</span><span class="sxs-lookup"><span data-stu-id="90dcf-306">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="90dcf-307">关闭浏览器。</span><span class="sxs-lookup"><span data-stu-id="90dcf-307">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="90dcf-308">练习 3： 将注入操作筛选器</span><span class="sxs-lookup"><span data-stu-id="90dcf-308">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="90dcf-309">在以前的动手实验**自定义操作筛选器**使用筛选器自定义项和注入过。</span><span class="sxs-lookup"><span data-stu-id="90dcf-309">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="90dcf-310">在此练习中，你将了解如何将筛选器依赖关系注入插入使用 Unity 容器。</span><span class="sxs-lookup"><span data-stu-id="90dcf-310">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="90dcf-311">为此，请将向音乐商店解决方案添加一个将跟踪的站点的活动的自定义操作筛选器。</span><span class="sxs-lookup"><span data-stu-id="90dcf-311">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="90dcf-312">任务 1-包括解决方案中的跟踪筛选器</span><span class="sxs-lookup"><span data-stu-id="90dcf-312">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="90dcf-313">在此任务中，你将包括在音乐存储区中跟踪事件的自定义操作筛选器。</span><span class="sxs-lookup"><span data-stu-id="90dcf-313">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="90dcf-314">为自定义操作筛选器概念已处理在上一实验室&quot;自定义操作筛选器&quot;，你将只包括本实验室中，资产文件夹中的筛选器类，然后创建用于 Unity 的筛选器提供程序：</span><span class="sxs-lookup"><span data-stu-id="90dcf-314">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="90dcf-315">打开**开始**解决方案位于**Source\Ex03-将注入操作 Filter\Begin**文件夹。</span><span class="sxs-lookup"><span data-stu-id="90dcf-315">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="90dcf-316">否则，可能会继续使用**结束**解决方案获取通过完成上一练习。</span><span class="sxs-lookup"><span data-stu-id="90dcf-316">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="90dcf-317">如果你打开提供**开始**解决方案，你将需要下载一些缺少的 NuGet 程序包才能继续。</span><span class="sxs-lookup"><span data-stu-id="90dcf-317">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="90dcf-318">若要执行此操作，请单击**项目**菜单，然后选择**管理 NuGet 包**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-318">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="90dcf-319">在**管理 NuGet 包**对话框中，单击**还原**以便下载缺少的程序包。</span><span class="sxs-lookup"><span data-stu-id="90dcf-319">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="90dcf-320">最后，通过单击生成解决方案**生成** | **生成解决方案**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-320">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="90dcf-321">使用 NuGet 的优点之一是，你无需提供你的项目中的所有库减小项目大小。</span><span class="sxs-lookup"><span data-stu-id="90dcf-321">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="90dcf-322">使用 NuGet 增强工具，请通过指定的包版本在 Packages.config 文件中，你将能够下载首次运行该项目的所有所需的库。</span><span class="sxs-lookup"><span data-stu-id="90dcf-322">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="90dcf-323">这是你将需要从本实验打开现有的解决方案后运行这些步骤的原因。</span><span class="sxs-lookup"><span data-stu-id="90dcf-323">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="90dcf-324">有关详细信息，请参阅此文章： [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)。</span><span class="sxs-lookup"><span data-stu-id="90dcf-324">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="90dcf-325">包括**TraceActionFilter.cs**文件从**/源/资产**到**/筛选**文件夹。</span><span class="sxs-lookup"><span data-stu-id="90dcf-325">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="90dcf-326">此自定义操作筛选器执行 ASP.NET 跟踪。</span><span class="sxs-lookup"><span data-stu-id="90dcf-326">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="90dcf-327">你可以检查&quot;ASP.NET MVC 4 本地和动态操作筛选器&quot;实验室以进行更多引用。</span><span class="sxs-lookup"><span data-stu-id="90dcf-327">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="90dcf-328">添加空类**FilterProvider.cs**到项目的文件夹中  **/筛选器。**</span><span class="sxs-lookup"><span data-stu-id="90dcf-328">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="90dcf-329">添加**System.Web.Mvc**和**Microsoft.Practices.Unity**中的命名空间**FilterProvider.cs**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-329">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="90dcf-330">(代码段- *ASP.NET 依赖关系注入实验室-Ex03-筛选器提供程序添加命名空间*)</span><span class="sxs-lookup"><span data-stu-id="90dcf-330">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="90dcf-331">请从继承的类**IFilterProvider**接口。</span><span class="sxs-lookup"><span data-stu-id="90dcf-331">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="90dcf-332">添加**IUnityContainer**中的属性**FilterProvider**类，，然后创建一个类构造函数来分配容器。</span><span class="sxs-lookup"><span data-stu-id="90dcf-332">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="90dcf-333">(代码段- *ASP.NET 依赖关系注入实验室-Ex03-筛选器提供程序构造函数*)</span><span class="sxs-lookup"><span data-stu-id="90dcf-333">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="90dcf-334">不创建筛选器提供程序类构造函数**新**对象内。</span><span class="sxs-lookup"><span data-stu-id="90dcf-334">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="90dcf-335">容器传递作为参数，并且通过 Unity 解决依赖项。</span><span class="sxs-lookup"><span data-stu-id="90dcf-335">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="90dcf-336">在**FilterProvider**类中，实现方法**GetFilters**从**IFilterProvider**接口。</span><span class="sxs-lookup"><span data-stu-id="90dcf-336">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="90dcf-337">(代码段- *ASP.NET 依赖关系注入实验室-Ex03-筛选器提供程序 GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="90dcf-337">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="90dcf-338">任务 2-注册并启用筛选器</span><span class="sxs-lookup"><span data-stu-id="90dcf-338">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="90dcf-339">在此任务中，你将启用站点跟踪。</span><span class="sxs-lookup"><span data-stu-id="90dcf-339">In this task, you will enable site tracking.</span></span> <span data-ttu-id="90dcf-340">为此，请将注册中的筛选器**Bootstrapper.cs BuildUnityContainer**方法以启动跟踪：</span><span class="sxs-lookup"><span data-stu-id="90dcf-340">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="90dcf-341">打开**Web.config**位于项目根目录和 System.Web 组启用跟踪跟踪。</span><span class="sxs-lookup"><span data-stu-id="90dcf-341">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="90dcf-342">打开**Bootstrapper.cs**项目根目录下。</span><span class="sxs-lookup"><span data-stu-id="90dcf-342">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="90dcf-343">添加对的引用**MvcMusicStore.Filters**命名空间。</span><span class="sxs-lookup"><span data-stu-id="90dcf-343">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="90dcf-344">(代码段- *ASP.NET 依赖关系注入实验室-Ex03-引导程序添加命名空间*)</span><span class="sxs-lookup"><span data-stu-id="90dcf-344">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="90dcf-345">选择**BuildUnityContainer**方法和寄存器 Unity 容器中的筛选器。</span><span class="sxs-lookup"><span data-stu-id="90dcf-345">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="90dcf-346">你将需要注册的筛选器提供程序，以及操作筛选器。</span><span class="sxs-lookup"><span data-stu-id="90dcf-346">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="90dcf-347">(代码段- *ASP.NET 依赖关系注入实验室-Ex03-注册 FilterProvider 和 ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="90dcf-347">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="90dcf-348">任务 3-运行应用程序</span><span class="sxs-lookup"><span data-stu-id="90dcf-348">Task 3 - Running the Application</span></span>

<span data-ttu-id="90dcf-349">在此任务中，你将运行应用程序和测试自定义操作筛选器跟踪活动：</span><span class="sxs-lookup"><span data-stu-id="90dcf-349">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="90dcf-350">按 **F5** 运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="90dcf-350">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="90dcf-351">单击**Rock**风格菜单中。</span><span class="sxs-lookup"><span data-stu-id="90dcf-351">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="90dcf-352">如果你愿意，你可以浏览到更多风格。</span><span class="sxs-lookup"><span data-stu-id="90dcf-352">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="90dcf-353">![音乐商店](aspnet-mvc-4-dependency-injection/_static/image11.png "音乐商店")</span><span class="sxs-lookup"><span data-stu-id="90dcf-353">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="90dcf-354">*音乐商店*</span><span class="sxs-lookup"><span data-stu-id="90dcf-354">*Music Store*</span></span>
3. <span data-ttu-id="90dcf-355">浏览到**/Trace.axd**以查看应用程序跟踪页，，然后单击**查看详细信息**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-355">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="90dcf-356">![应用程序跟踪日志](aspnet-mvc-4-dependency-injection/_static/image12.png "应用程序跟踪日志")</span><span class="sxs-lookup"><span data-stu-id="90dcf-356">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="90dcf-357">*应用程序跟踪日志*</span><span class="sxs-lookup"><span data-stu-id="90dcf-357">*Application Trace Log*</span></span>

    <span data-ttu-id="90dcf-358">![应用程序跟踪的请求详细信息](aspnet-mvc-4-dependency-injection/_static/image13.png "跟踪应用程序的请求的详细信息")</span><span class="sxs-lookup"><span data-stu-id="90dcf-358">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="90dcf-359">*应用程序跟踪的请求详细信息*</span><span class="sxs-lookup"><span data-stu-id="90dcf-359">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="90dcf-360">关闭浏览器。</span><span class="sxs-lookup"><span data-stu-id="90dcf-360">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="90dcf-361">摘要</span><span class="sxs-lookup"><span data-stu-id="90dcf-361">Summary</span></span>

<span data-ttu-id="90dcf-362">通过完成本动手实验中，你已学习如何在 ASP.NET MVC 4 中的依赖关系注入使用通过集成 Unity 使用 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="90dcf-362">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="90dcf-363">若要实现此目的，你使用控制器、 视图和操作筛选器内的依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="90dcf-363">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="90dcf-364">介绍了以下概念：</span><span class="sxs-lookup"><span data-stu-id="90dcf-364">The following concepts were covered:</span></span>

- <span data-ttu-id="90dcf-365">ASP.NET MVC 4 依赖关系注入功能</span><span class="sxs-lookup"><span data-stu-id="90dcf-365">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="90dcf-366">Unity 集成使用 Unity.Mvc3 NuGet 包</span><span class="sxs-lookup"><span data-stu-id="90dcf-366">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="90dcf-367">在控制器中的依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="90dcf-367">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="90dcf-368">在视图中的依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="90dcf-368">Dependency Injection in Views</span></span>
- <span data-ttu-id="90dcf-369">依赖关系注入的操作筛选器</span><span class="sxs-lookup"><span data-stu-id="90dcf-369">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="90dcf-370">附录 a： 安装 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="90dcf-370">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="90dcf-371">你可以安装**Microsoft Visual Studio Express 2012 for Web**或另一个&quot;Express&quot;版本使用 **[Microsoft Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="90dcf-371">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="90dcf-372">以下说明将指导你完成安装所需的步骤*Visual studio Express 2012 for Web*使用*Microsoft Web 平台安装程序*。</span><span class="sxs-lookup"><span data-stu-id="90dcf-372">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="90dcf-373">转到[ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="90dcf-373">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="90dcf-374">或者，如果你已安装 Web 平台安装程序，你可以打开它，并搜索产品&quot; *Visual Studio Express 2012 for Web 的 Windows Azure SDK*&quot;。</span><span class="sxs-lookup"><span data-stu-id="90dcf-374">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="90dcf-375">单击**立即安装**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-375">Click on **Install Now**.</span></span> <span data-ttu-id="90dcf-376">如果你没有**Web 平台安装程序**将重定向以下载并请先安装它。</span><span class="sxs-lookup"><span data-stu-id="90dcf-376">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="90dcf-377">一次**Web 平台安装程序**处于打开状态，单击**安装**以启动安装程序。</span><span class="sxs-lookup"><span data-stu-id="90dcf-377">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="90dcf-378">![安装 Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "安装 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="90dcf-378">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="90dcf-379">*安装 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="90dcf-379">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="90dcf-380">阅读所有产品的许可证和条款，然后单击**我接受**以继续。</span><span class="sxs-lookup"><span data-stu-id="90dcf-380">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受许可条款](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="90dcf-382">*接受许可条款*</span><span class="sxs-lookup"><span data-stu-id="90dcf-382">*Accepting the license terms*</span></span>
5. <span data-ttu-id="90dcf-383">等待，直到下载和安装过程完成。</span><span class="sxs-lookup"><span data-stu-id="90dcf-383">Wait until the downloading and installation process completes.</span></span>

    ![安装进度](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="90dcf-385">*安装进度*</span><span class="sxs-lookup"><span data-stu-id="90dcf-385">*Installation progress*</span></span>
6. <span data-ttu-id="90dcf-386">当安装完成后时，单击**完成**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-386">When the installation completes, click **Finish**.</span></span>

    ![安装已完成](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="90dcf-388">*安装已完成*</span><span class="sxs-lookup"><span data-stu-id="90dcf-388">*Installation completed*</span></span>
7. <span data-ttu-id="90dcf-389">单击**退出**以关闭 Web 平台安装程序。</span><span class="sxs-lookup"><span data-stu-id="90dcf-389">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="90dcf-390">若要打开 Visual Studio Express for Web，请转到**启动**屏幕并开始编写&quot; **VS Express**&quot;，然后单击**VS Express for Web**磁贴。</span><span class="sxs-lookup"><span data-stu-id="90dcf-390">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Web 磁贴的 VS Express](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="90dcf-392">*Web 磁贴的 VS Express*</span><span class="sxs-lookup"><span data-stu-id="90dcf-392">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="90dcf-393">附录 b： 使用代码片段</span><span class="sxs-lookup"><span data-stu-id="90dcf-393">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="90dcf-394">和代码片段，必须将你能够轻松获得所需的所有代码所示。</span><span class="sxs-lookup"><span data-stu-id="90dcf-394">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="90dcf-395">实验室文档将告诉您完全时你可以使用它们，如下图中所示。</span><span class="sxs-lookup"><span data-stu-id="90dcf-395">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="90dcf-396">![使用 Visual Studio 代码段将代码插入到你的项目](aspnet-mvc-4-dependency-injection/_static/image19.png "使用 Visual Studio 代码片段，可将代码插入到你的项目")</span><span class="sxs-lookup"><span data-stu-id="90dcf-396">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="90dcf-397">*使用 Visual Studio 代码段将代码插入到你的项目*</span><span class="sxs-lookup"><span data-stu-id="90dcf-397">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="90dcf-398">***若要添加代码片段使用键盘 (仅限 C#)***</span><span class="sxs-lookup"><span data-stu-id="90dcf-398">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="90dcf-399">将光标置于想要插入代码。</span><span class="sxs-lookup"><span data-stu-id="90dcf-399">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="90dcf-400">开始键入代码段名称 （不带空格或连字符）。</span><span class="sxs-lookup"><span data-stu-id="90dcf-400">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="90dcf-401">观看作为 IntelliSense 显示匹配的代码段的名称。</span><span class="sxs-lookup"><span data-stu-id="90dcf-401">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="90dcf-402">选择正确的代码段 （或继续键入直到选中整个代码段的名称）。</span><span class="sxs-lookup"><span data-stu-id="90dcf-402">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="90dcf-403">按 Tab 键两次以光标位置处插入代码段。</span><span class="sxs-lookup"><span data-stu-id="90dcf-403">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="90dcf-404">![开始键入代码段名称](aspnet-mvc-4-dependency-injection/_static/image20.png "开始键入代码段名称")</span><span class="sxs-lookup"><span data-stu-id="90dcf-404">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="90dcf-405">*开始键入代码段名称*</span><span class="sxs-lookup"><span data-stu-id="90dcf-405">*Start typing the snippet name*</span></span>

<span data-ttu-id="90dcf-406">![按 Tab 以选择突出显示代码段](aspnet-mvc-4-dependency-injection/_static/image21.png "按选项卡以选择突出显示代码段")</span><span class="sxs-lookup"><span data-stu-id="90dcf-406">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="90dcf-407">*按 Tab 以选择突出显示代码段*</span><span class="sxs-lookup"><span data-stu-id="90dcf-407">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="90dcf-408">![再次按 Tab 和代码段将会扩展](aspnet-mvc-4-dependency-injection/_static/image22.png "再次按 Tab 和代码段将会扩展")</span><span class="sxs-lookup"><span data-stu-id="90dcf-408">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="90dcf-409">*再次按 Tab 和代码段将会扩展*</span><span class="sxs-lookup"><span data-stu-id="90dcf-409">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="90dcf-410">***若要添加代码片段使用鼠标 （C#、 Visual Basic 和 XML）*** 1。</span><span class="sxs-lookup"><span data-stu-id="90dcf-410">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="90dcf-411">右键单击你想要插入代码段。</span><span class="sxs-lookup"><span data-stu-id="90dcf-411">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="90dcf-412">选择**插入代码段**跟**我的代码段**。</span><span class="sxs-lookup"><span data-stu-id="90dcf-412">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="90dcf-413">选择相关的代码段从列表中，通过单击它。</span><span class="sxs-lookup"><span data-stu-id="90dcf-413">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="90dcf-414">![右键单击你想要插入代码段，并选择插入代码段](aspnet-mvc-4-dependency-injection/_static/image23.png "右键单击你想要插入代码段，并选择插入代码段")</span><span class="sxs-lookup"><span data-stu-id="90dcf-414">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="90dcf-415">*右键单击你想要插入代码段，并选择插入代码段*</span><span class="sxs-lookup"><span data-stu-id="90dcf-415">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="90dcf-416">![通过单击它选取列表中中的相关代码片段](aspnet-mvc-4-dependency-injection/_static/image24.png "选取相关的代码段从列表中，通过单击它")</span><span class="sxs-lookup"><span data-stu-id="90dcf-416">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="90dcf-417">*通过单击它选取从列表中，相关代码段*</span><span class="sxs-lookup"><span data-stu-id="90dcf-417">*Pick the relevant snippet from the list, by clicking on it*</span></span>
