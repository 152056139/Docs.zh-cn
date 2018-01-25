---
title: "在 ASP.NET 核心的测试控制器逻辑"
author: ardalis
description: "了解如何在带有 Moq 和 xUnit 的 ASP.NET 核心中测试控制器逻辑。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/testing
ms.openlocfilehash: f27e7ec43cd17e249dd646a7dfbce5df69d59664
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
# <a name="testing-controller-logic-in-aspnet-core"></a><span data-ttu-id="70c30-103">在 ASP.NET 核心的测试控制器逻辑</span><span class="sxs-lookup"><span data-stu-id="70c30-103">Testing controller logic in ASP.NET Core</span></span>

<span data-ttu-id="70c30-104">通过[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="70c30-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="70c30-105">小型，其注重用户界面顾虑，应为在 ASP.NET MVC 应用程序中的控制器。</span><span class="sxs-lookup"><span data-stu-id="70c30-105">Controllers in ASP.NET MVC apps should be small and focused on user-interface concerns.</span></span> <span data-ttu-id="70c30-106">处理非 UI 问题的大型控制器会测试和维护更加困难。</span><span class="sxs-lookup"><span data-stu-id="70c30-106">Large controllers that deal with non-UI concerns are more difficult to test and maintain.</span></span>

[<span data-ttu-id="70c30-107">查看或从 GitHub 下载示例</span><span class="sxs-lookup"><span data-stu-id="70c30-107">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a><span data-ttu-id="70c30-108">测试控制器</span><span class="sxs-lookup"><span data-stu-id="70c30-108">Testing controllers</span></span>

<span data-ttu-id="70c30-109">控制器是任何 ASP.NET 核心 MVC 应用程序的中央部分。</span><span class="sxs-lookup"><span data-stu-id="70c30-109">Controllers are a central part of any ASP.NET Core MVC application.</span></span> <span data-ttu-id="70c30-110">在这种情况下，你应具有它们的行为符合适用于你的应用程序的置信度。</span><span class="sxs-lookup"><span data-stu-id="70c30-110">As such, you should have confidence they behave as intended for your app.</span></span> <span data-ttu-id="70c30-111">自动的测试可以为您提供这种信心，并可以到达生产之前检测错误。</span><span class="sxs-lookup"><span data-stu-id="70c30-111">Automated tests can provide you with this confidence and can detect errors before they reach production.</span></span> <span data-ttu-id="70c30-112">务必避免将放置在你的控制器中的不必要职责，并确保你测试只关注控制器职责。</span><span class="sxs-lookup"><span data-stu-id="70c30-112">It's important to avoid placing unnecessary responsibilities within your controllers and ensure your tests focus only on controller responsibilities.</span></span>

<span data-ttu-id="70c30-113">控制器逻辑应该很小，并且不侧重于业务逻辑或基础结构问题 （例如，数据访问）。</span><span class="sxs-lookup"><span data-stu-id="70c30-113">Controller logic should be minimal and not be focused on business logic or infrastructure concerns (for example, data access).</span></span> <span data-ttu-id="70c30-114">测试控制器逻辑，不 framework。</span><span class="sxs-lookup"><span data-stu-id="70c30-114">Test controller logic, not the framework.</span></span> <span data-ttu-id="70c30-115">测试如何控制器*行为*基于有效或无效的输入。</span><span class="sxs-lookup"><span data-stu-id="70c30-115">Test how the controller *behaves* based on valid or invalid inputs.</span></span> <span data-ttu-id="70c30-116">测试控制器响应基于业务它所执行的操作的结果。</span><span class="sxs-lookup"><span data-stu-id="70c30-116">Test controller responses based on the result of the business operation it performs.</span></span>

<span data-ttu-id="70c30-117">典型控制器职责：</span><span class="sxs-lookup"><span data-stu-id="70c30-117">Typical controller responsibilities:</span></span>

* <span data-ttu-id="70c30-118">验证`ModelState.IsValid`。</span><span class="sxs-lookup"><span data-stu-id="70c30-118">Verify `ModelState.IsValid`.</span></span>
* <span data-ttu-id="70c30-119">如果返回错误响应`ModelState`无效。</span><span class="sxs-lookup"><span data-stu-id="70c30-119">Return an error response if `ModelState` is invalid.</span></span>
* <span data-ttu-id="70c30-120">从持久性存储中检索业务实体。</span><span class="sxs-lookup"><span data-stu-id="70c30-120">Retrieve a business entity from persistence.</span></span>
* <span data-ttu-id="70c30-121">对业务实体执行操作。</span><span class="sxs-lookup"><span data-stu-id="70c30-121">Perform an action on the business entity.</span></span>
* <span data-ttu-id="70c30-122">将业务实体保存到持久性。</span><span class="sxs-lookup"><span data-stu-id="70c30-122">Save the business entity to persistence.</span></span>
* <span data-ttu-id="70c30-123">返回适当`IActionResult`。</span><span class="sxs-lookup"><span data-stu-id="70c30-123">Return an appropriate `IActionResult`.</span></span>

## <a name="unit-testing"></a><span data-ttu-id="70c30-124">单元测试</span><span class="sxs-lookup"><span data-stu-id="70c30-124">Unit testing</span></span>

<span data-ttu-id="70c30-125">[单元测试](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)涉及在从其基础结构和依赖项隔离中测试应用程序的一部分。</span><span class="sxs-lookup"><span data-stu-id="70c30-125">[Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) involves testing a part of an app in isolation from its infrastructure and dependencies.</span></span> <span data-ttu-id="70c30-126">在测试单元测试控制器逻辑，只有单个操作的内容时，不的行为及其依赖项或框架本身。</span><span class="sxs-lookup"><span data-stu-id="70c30-126">When unit testing controller logic, only the contents of a single action is tested, not the behavior of its dependencies or of the framework itself.</span></span> <span data-ttu-id="70c30-127">作为你的单元测试控制器操作，请确保您仅关注其行为。</span><span class="sxs-lookup"><span data-stu-id="70c30-127">As you unit test your controller actions, make sure you focus only on its behavior.</span></span> <span data-ttu-id="70c30-128">控制器单元测试可避免等[筛选器](filters.md)，[路由](../../fundamentals/routing.md)，或[模型绑定](../models/model-binding.md)。</span><span class="sxs-lookup"><span data-stu-id="70c30-128">A controller unit test avoids things like [filters](filters.md), [routing](../../fundamentals/routing.md), or [model binding](../models/model-binding.md).</span></span> <span data-ttu-id="70c30-129">通过将重点放在测试只是一件事，单元测试通常是编写简单且快速运行。</span><span class="sxs-lookup"><span data-stu-id="70c30-129">By focusing on testing just one thing, unit tests are generally simple to write and quick to run.</span></span> <span data-ttu-id="70c30-130">可以经常运行一组正确编写的单元测试，而无需多开销。</span><span class="sxs-lookup"><span data-stu-id="70c30-130">A well-written set of unit tests can be run frequently without much overhead.</span></span> <span data-ttu-id="70c30-131">但是，单元测试不检测问题在组件之间的交互，该命令的用途是[集成测试](xref:mvc/controllers/testing#integration-testing)。</span><span class="sxs-lookup"><span data-stu-id="70c30-131">However, unit tests don't detect issues in the interaction between components, which is the purpose of [integration testing](xref:mvc/controllers/testing#integration-testing).</span></span>

<span data-ttu-id="70c30-132">如果你在编写自定义筛选器、 路由等，您应该单元测试，但不是作为你的测试上的特定控制器操作的一部分。</span><span class="sxs-lookup"><span data-stu-id="70c30-132">If you're writing custom filters, routes, etc, you should unit test them, but not as part of your tests on a particular controller action.</span></span> <span data-ttu-id="70c30-133">应在隔离中对它们进行了测试。</span><span class="sxs-lookup"><span data-stu-id="70c30-133">They should be tested in isolation.</span></span>

> [!TIP]
> <span data-ttu-id="70c30-134">[创建和使用 Visual Studio 运行单元测试](https://docs.microsoft.com/visualstudio/test/unit-test-your-code)。</span><span class="sxs-lookup"><span data-stu-id="70c30-134">[Create and run unit tests with Visual Studio](https://docs.microsoft.com/visualstudio/test/unit-test-your-code).</span></span>

<span data-ttu-id="70c30-135">若要演示单元测试，请查看以下控制器。</span><span class="sxs-lookup"><span data-stu-id="70c30-135">To demonstrate unit testing, review the following controller.</span></span> <span data-ttu-id="70c30-136">它显示集体讨论会话的列表，并允许新集体讨论会话，以使用 POST 创建：</span><span class="sxs-lookup"><span data-stu-id="70c30-136">It displays a list of brainstorming sessions and allows new brainstorming sessions to be created with a POST:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

<span data-ttu-id="70c30-137">以下控制器[显式依赖关系原则](http://deviq.com/explicit-dependencies-principle/)，应为其提供的实例的依赖关系注入`IBrainstormSessionRepository`。</span><span class="sxs-lookup"><span data-stu-id="70c30-137">The controller is following the [explicit dependencies principle](http://deviq.com/explicit-dependencies-principle/), expecting dependency injection to provide it with an instance of `IBrainstormSessionRepository`.</span></span> <span data-ttu-id="70c30-138">这可以相当轻松地测试使用模拟对象框架，如[Moq](https://www.nuget.org/packages/Moq/)。</span><span class="sxs-lookup"><span data-stu-id="70c30-138">This makes it fairly easy to test using a mock object framework, like [Moq](https://www.nuget.org/packages/Moq/).</span></span> <span data-ttu-id="70c30-139">`HTTP GET Index`方法具有没有循环或分支和只能调用一种方法。</span><span class="sxs-lookup"><span data-stu-id="70c30-139">The `HTTP GET Index` method has no looping or branching and only calls one method.</span></span> <span data-ttu-id="70c30-140">若要测试这`Index`方法，我们需要验证`ViewResult`返回，与`ViewModel`从该存储库的`List`方法。</span><span class="sxs-lookup"><span data-stu-id="70c30-140">To test this `Index` method, we need to verify that a `ViewResult` is returned, with a `ViewModel` from the repository's `List` method.</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

<span data-ttu-id="70c30-141">`HomeController` `HTTP POST Index` （如上所示） 的方法应验证：</span><span class="sxs-lookup"><span data-stu-id="70c30-141">The `HomeController` `HTTP POST Index` method (shown above) should verify:</span></span>

* <span data-ttu-id="70c30-142">操作方法会返回错误的请求`ViewResult`使用适当的数据时`ModelState.IsValid`是`false`</span><span class="sxs-lookup"><span data-stu-id="70c30-142">The action method returns a Bad Request `ViewResult` with the appropriate data when `ModelState.IsValid` is `false`</span></span>

* <span data-ttu-id="70c30-143">`Add`调用在存储库上的方法和一个`RedirectToActionResult`使用正确的自变量返回时`ModelState.IsValid`为 true。</span><span class="sxs-lookup"><span data-stu-id="70c30-143">The `Add` method on the repository is called and a `RedirectToActionResult` is returned with the correct arguments when `ModelState.IsValid` is true.</span></span>

<span data-ttu-id="70c30-144">可以添加使用错误来测试无效模型状态`AddModelError`下面的第一个测试中所示。</span><span class="sxs-lookup"><span data-stu-id="70c30-144">Invalid model state can be tested by adding errors using `AddModelError` as shown in the first test below.</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

<span data-ttu-id="70c30-145">第一个测试确认时`ModelState`无效，相同`ViewResult`同样适用于返回`GET`请求。</span><span class="sxs-lookup"><span data-stu-id="70c30-145">The first test confirms when `ModelState` isn't valid, the same `ViewResult` is returned as for a `GET` request.</span></span> <span data-ttu-id="70c30-146">请注意，测试不会尝试将一个无效的模型。</span><span class="sxs-lookup"><span data-stu-id="70c30-146">Note that the test doesn't attempt to pass in an invalid model.</span></span> <span data-ttu-id="70c30-147">这就仍起作用，因为模型绑定不运行 (尽管[集成测试](xref:mvc/controllers/testing#integration-testing)将练习模型绑定)。</span><span class="sxs-lookup"><span data-stu-id="70c30-147">That wouldn't work anyway since model binding isn't running (though an [integration test](xref:mvc/controllers/testing#integration-testing) would use exercise model binding).</span></span> <span data-ttu-id="70c30-148">在这种情况下，不是所测试的模型绑定。</span><span class="sxs-lookup"><span data-stu-id="70c30-148">In this case, model binding isn't being tested.</span></span> <span data-ttu-id="70c30-149">这些单元测试仅测试中的操作方法的代码的用途。</span><span class="sxs-lookup"><span data-stu-id="70c30-149">These unit tests are only testing what the code in the action method does.</span></span>

<span data-ttu-id="70c30-150">第二个测试验证，当`ModelState`有效，新`BrainstormSession`添加 （通过存储库），并且该方法返回`RedirectToActionResult`具有预期的属性。</span><span class="sxs-lookup"><span data-stu-id="70c30-150">The second test verifies that when `ModelState` is valid, a new `BrainstormSession` is added (via the repository), and the method returns a `RedirectToActionResult` with the expected properties.</span></span> <span data-ttu-id="70c30-151">模拟不调用的调用是正常情况下将其忽略，但调用`Verifiable`末尾的安装程序调用允许它在测试中验证。</span><span class="sxs-lookup"><span data-stu-id="70c30-151">Mocked calls that aren't called are normally ignored, but calling `Verifiable` at the end of the setup call allows it to be verified in the test.</span></span> <span data-ttu-id="70c30-152">这通过调用完成`mockRepo.Verify`，如果预期的方法未调用，这将失败的测试。</span><span class="sxs-lookup"><span data-stu-id="70c30-152">This is done with the call to `mockRepo.Verify`, which will fail the test if the expected method wasn't called.</span></span>

> [!NOTE]
> <span data-ttu-id="70c30-153">此示例中使用的 Moq 库便于在混合使用 （也称为"松散"mock 或存根） 的非可验证 mock 的验证，或"strict"，mock。</span><span class="sxs-lookup"><span data-stu-id="70c30-153">The Moq library used in this sample makes it easy to mix verifiable, or "strict", mocks with non-verifiable mocks (also called "loose" mocks or stubs).</span></span> <span data-ttu-id="70c30-154">详细了解[自定义模型行为与 Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior)。</span><span class="sxs-lookup"><span data-stu-id="70c30-154">Learn more about [customizing Mock behavior with Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).</span></span>

<span data-ttu-id="70c30-155">在应用程序的另一个控制器显示与特定的集体会话相关的信息。</span><span class="sxs-lookup"><span data-stu-id="70c30-155">Another controller in the app displays information related to a particular brainstorming session.</span></span> <span data-ttu-id="70c30-156">它包括一些逻辑以处理无效的 id 值：</span><span class="sxs-lookup"><span data-stu-id="70c30-156">It includes some logic to deal with invalid id values:</span></span>

[!code-csharp[Main](./testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

<span data-ttu-id="70c30-157">控制器操作都有三种情况下，若要测试，每一个`return`语句：</span><span class="sxs-lookup"><span data-stu-id="70c30-157">The controller action has three cases to test, one for each `return` statement:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

<span data-ttu-id="70c30-158">应用程序公开 web API （与集体会话和到会话中添加新的想法的方法相关联的建议列表） 作为功能：</span><span class="sxs-lookup"><span data-stu-id="70c30-158">The app exposes functionality as a web API (a list of ideas associated with a brainstorming session and a method for adding new ideas to a session):</span></span>

<a name="ideas-controller"></a>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

<span data-ttu-id="70c30-159">`ForSession`方法返回的列表`IdeaDTO`类型。</span><span class="sxs-lookup"><span data-stu-id="70c30-159">The `ForSession` method returns a list of `IdeaDTO` types.</span></span> <span data-ttu-id="70c30-160">避免直接通过 API 调用返回您的业务域实体，因为经常包括超过 API 客户端要求，它们与外部公开的 API 不必要地耦合应用程序的内部域模型的数据。</span><span class="sxs-lookup"><span data-stu-id="70c30-160">Avoid returning your business domain entities directly via API calls, since frequently they include more data than the API client requires, and they unnecessarily couple your app's internal domain model with the API you expose externally.</span></span> <span data-ttu-id="70c30-161">可以手动完成域实体与通过网络将返回的类型之间的映射 (使用 LINQ`Select`如此处所示) 或使用类似库[AutoMapper](https://github.com/AutoMapper/AutoMapper)</span><span class="sxs-lookup"><span data-stu-id="70c30-161">Mapping between domain entities and the types you will return over the wire can be done manually (using a LINQ `Select` as shown here) or using a library like [AutoMapper](https://github.com/AutoMapper/AutoMapper)</span></span>

<span data-ttu-id="70c30-162">适用于单元测试`Create`和`ForSession`API 方法：</span><span class="sxs-lookup"><span data-stu-id="70c30-162">The unit tests for the `Create` and `ForSession` API methods:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

<span data-ttu-id="70c30-163">如前所述，若要测试方法的行为时`ModelState`是无效的将模型错误添加到控制器中，为测试的一部分。</span><span class="sxs-lookup"><span data-stu-id="70c30-163">As stated previously, to test the behavior of the method when `ModelState` is invalid, add a model error to the controller as part of the test.</span></span> <span data-ttu-id="70c30-164">请勿尝试在你的单元测试中测试模型验证或模型绑定-只需测试时遇到与特定的操作方法的行为`ModelState`值。</span><span class="sxs-lookup"><span data-stu-id="70c30-164">Don't try to test model validation or model binding in your unit tests - just test your action method's behavior when confronted with a particular `ModelState` value.</span></span>

<span data-ttu-id="70c30-165">第二个测试依赖于存储库返回 null，因此模拟的存储库配置为返回 null。</span><span class="sxs-lookup"><span data-stu-id="70c30-165">The second test depends on the repository returning null, so the mock repository is configured to return null.</span></span> <span data-ttu-id="70c30-166">没有无需创建一个测试数据库 (在内存中或其他) 和构造的查询将返回此结果-它可以在单个语句中所示。</span><span class="sxs-lookup"><span data-stu-id="70c30-166">There's no need to create a test database (in memory or otherwise) and construct a query that will return this result - it can be done in a single statement as shown.</span></span>

<span data-ttu-id="70c30-167">最后一次测试验证该存储库的`Update`调用方法。</span><span class="sxs-lookup"><span data-stu-id="70c30-167">The last test verifies that the repository's `Update` method is called.</span></span> <span data-ttu-id="70c30-168">正如我们做之前，使用调用 mock `Verifiable` ，然后模拟储存库的`Verify`调用方法来确认执行可验证的方法。</span><span class="sxs-lookup"><span data-stu-id="70c30-168">As we did previously, the mock is called with `Verifiable` and then the mocked repository's `Verify` method is called to confirm the verifiable method was executed.</span></span> <span data-ttu-id="70c30-169">它不是单元测试有责任确保`Update`方法保存的数据; 为此，可以与的集成测试。</span><span class="sxs-lookup"><span data-stu-id="70c30-169">It's not a unit test responsibility to ensure that the `Update` method saved the data; that can be done with an integration test.</span></span>

## <a name="integration-testing"></a><span data-ttu-id="70c30-170">集成测试</span><span class="sxs-lookup"><span data-stu-id="70c30-170">Integration testing</span></span>

<span data-ttu-id="70c30-171">[集成测试](../../testing/integration-testing.md)做是为了确保你的应用程序工作中的单独模块正确在一起。</span><span class="sxs-lookup"><span data-stu-id="70c30-171">[Integration testing](../../testing/integration-testing.md) is done to ensure separate modules within your app work correctly together.</span></span> <span data-ttu-id="70c30-172">通常情况下，你可以测试与单元测试的任何内容，你还可以测试与的集成测试，但反之不是如此。</span><span class="sxs-lookup"><span data-stu-id="70c30-172">Generally, anything you can test with a unit test, you can also test with an integration test, but the reverse isn't true.</span></span> <span data-ttu-id="70c30-173">但是，集成测试往往要比单元测试慢得多。</span><span class="sxs-lookup"><span data-stu-id="70c30-173">However, integration tests tend to be much slower than unit tests.</span></span> <span data-ttu-id="70c30-174">因此，最好测试你可以使用单元测试的任何和集成测试用于涉及多个协作者的方案。</span><span class="sxs-lookup"><span data-stu-id="70c30-174">Thus, it's best to test whatever you can with unit tests, and use integration tests for scenarios that involve multiple collaborators.</span></span>

<span data-ttu-id="70c30-175">尽管它们仍可能很有用，但模拟对象很少使用中的整体测试。</span><span class="sxs-lookup"><span data-stu-id="70c30-175">Although they may still be useful, mock objects are rarely used in integration tests.</span></span> <span data-ttu-id="70c30-176">在单元测试，模拟对象是有效的方法来控制在要测试的单元外部协作者出于测试目的的行为方式。</span><span class="sxs-lookup"><span data-stu-id="70c30-176">In unit testing, mock objects are an effective way to control how collaborators outside of the unit being tested should behave for the purposes of the test.</span></span> <span data-ttu-id="70c30-177">在集成测试中，实际的协作者用于确认整个子系统一起工作正常。</span><span class="sxs-lookup"><span data-stu-id="70c30-177">In an integration test, real collaborators are used to confirm the whole subsystem works together correctly.</span></span>

### <a name="application-state"></a><span data-ttu-id="70c30-178">应用程序状态</span><span class="sxs-lookup"><span data-stu-id="70c30-178">Application state</span></span>

<span data-ttu-id="70c30-179">执行集成测试时的一个重要考虑因素是如何设置你的应用程序的状态。</span><span class="sxs-lookup"><span data-stu-id="70c30-179">One important consideration when performing integration testing is how to set your app's state.</span></span> <span data-ttu-id="70c30-180">测试需要运行独立于另一个，并且因此每个测试应该开头的应用程序中已知的状态。</span><span class="sxs-lookup"><span data-stu-id="70c30-180">Tests need to run independent of one another, and so each test should start with the app in a known state.</span></span> <span data-ttu-id="70c30-181">如果你的应用程序不使用数据库或不具有任何持久性，这可能不是问题。</span><span class="sxs-lookup"><span data-stu-id="70c30-181">If your app doesn't use a database or have any persistence, this may not be an issue.</span></span> <span data-ttu-id="70c30-182">但是，大多数实际的应用保留到某种类型的数据存储区，其状态，因此任何所做的一个测试修改会影响另一个测试，除非重置数据存储区。</span><span class="sxs-lookup"><span data-stu-id="70c30-182">However, most real-world apps persist their state to some kind of data store, so any modifications made by one test could impact another test unless the data store is reset.</span></span> <span data-ttu-id="70c30-183">使用内置`TestServer`，到我们的集成测试，在承载 ASP.NET Core 应用程序中非常简单明了，但，不一定授予对要使用的数据的访问。</span><span class="sxs-lookup"><span data-stu-id="70c30-183">Using the built-in `TestServer`, it's very straightforward to host ASP.NET Core apps within our integration tests, but that doesn't necessarily grant access to the data it will use.</span></span> <span data-ttu-id="70c30-184">如果你使用的实际数据库，一种方法是让应用程序连接到测试数据库，该测试可以访问并确保将重置为已知的状态的每个测试执行之前。</span><span class="sxs-lookup"><span data-stu-id="70c30-184">If you're using an actual database, one approach is to have the app connect to a test database, which your tests can access and ensure is reset to a known state before each test executes.</span></span>

<span data-ttu-id="70c30-185">在此示例应用程序中，我使用实体框架核心 InMemoryDatabase 支持，因此只是无法从我的测试项目连接到它。</span><span class="sxs-lookup"><span data-stu-id="70c30-185">In this sample application, I'm using Entity Framework Core's InMemoryDatabase support, so I can't just connect to it from my test project.</span></span> <span data-ttu-id="70c30-186">相反，我公开`InitializeDatabase`从应用程序的方法`Startup`类，该类我调用是否在应用程序启动时`Development`环境。</span><span class="sxs-lookup"><span data-stu-id="70c30-186">Instead, I expose an `InitializeDatabase` method from the app's `Startup` class, which I call when the app starts up if it's in the `Development` environment.</span></span> <span data-ttu-id="70c30-187">我的集成测试自动从中受益，只要它们将环境设置为`Development`。</span><span class="sxs-lookup"><span data-stu-id="70c30-187">My integration tests automatically benefit from this as long as they set the environment to `Development`.</span></span> <span data-ttu-id="70c30-188">我不需要担心重置数据库，因为 InMemoryDatabase 应用程序重新启动每次重置。</span><span class="sxs-lookup"><span data-stu-id="70c30-188">I don't have to worry about resetting the database, since the InMemoryDatabase is reset each time the app restarts.</span></span>

<span data-ttu-id="70c30-189">`Startup`类：</span><span class="sxs-lookup"><span data-stu-id="70c30-189">The `Startup` class:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

<span data-ttu-id="70c30-190">你将看到`GetTestSession`常用在下面的集成测试的方法。</span><span class="sxs-lookup"><span data-stu-id="70c30-190">You'll see the `GetTestSession` method used frequently in the integration tests below.</span></span>

### <a name="accessing-views"></a><span data-ttu-id="70c30-191">访问视图</span><span class="sxs-lookup"><span data-stu-id="70c30-191">Accessing views</span></span>

<span data-ttu-id="70c30-192">每个集成测试类配置`TestServer`会运行 ASP.NET Core 应用。</span><span class="sxs-lookup"><span data-stu-id="70c30-192">Each integration test class configures the `TestServer` that will run the ASP.NET Core app.</span></span> <span data-ttu-id="70c30-193">默认情况下，`TestServer`承载它在何处运行-这种情况下，测试项目文件夹的文件夹中的 web 应用。</span><span class="sxs-lookup"><span data-stu-id="70c30-193">By default, `TestServer` hosts the web app in the folder where it's running - in this case, the test project folder.</span></span> <span data-ttu-id="70c30-194">因此，当你尝试测试返回的控制器操作`ViewResult`，你可能会看到此错误：</span><span class="sxs-lookup"><span data-stu-id="70c30-194">Thus, when you attempt to test controller actions that return `ViewResult`, you may see this error:</span></span>

```
The view 'Index' wasn't found. The following locations were searched:
(list of locations)
```

<span data-ttu-id="70c30-195">若要更正此问题，你需要配置服务器的内容的根，以便它可以找到所测试项目的视图。</span><span class="sxs-lookup"><span data-stu-id="70c30-195">To correct this issue, you need to configure the server's content root, so it can locate the views for the project being tested.</span></span> <span data-ttu-id="70c30-196">这通过调用`UseContentRoot`中`TestFixture`类，如下所示：</span><span class="sxs-lookup"><span data-stu-id="70c30-196">This is done by a call to `UseContentRoot` in the `TestFixture` class, shown below:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

<span data-ttu-id="70c30-197">`TestFixture`类负责配置和创建`TestServer`、 设置`HttpClient`与通信`TestServer`。</span><span class="sxs-lookup"><span data-stu-id="70c30-197">The `TestFixture` class is responsible for configuring and creating the `TestServer`, setting up an `HttpClient` to communicate with the `TestServer`.</span></span> <span data-ttu-id="70c30-198">每个集成测试使用`Client`属性来连接到测试服务器和发出请求。</span><span class="sxs-lookup"><span data-stu-id="70c30-198">Each of the integration tests uses the `Client` property to connect to the test server and make a request.</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

<span data-ttu-id="70c30-199">在上面的第一个测试`responseString`保存从视图，可以进行检查以确定它是否包含预期的结果中的实际呈现 HTML。</span><span class="sxs-lookup"><span data-stu-id="70c30-199">In the first test above, the `responseString` holds the actual rendered HTML from the View, which can be inspected to confirm it contains expected results.</span></span>

<span data-ttu-id="70c30-200">第二个测试构造的窗体 POST 具有唯一的会话名称并发送到应用程序中，然后验证返回预期的重定向。</span><span class="sxs-lookup"><span data-stu-id="70c30-200">The second test constructs a form POST with a unique session name and POSTs it to the app, then verifies that the expected redirect is returned.</span></span>

### <a name="api-methods"></a><span data-ttu-id="70c30-201">API 方法</span><span class="sxs-lookup"><span data-stu-id="70c30-201">API methods</span></span>

<span data-ttu-id="70c30-202">如果你的应用程序公开 web Api，它的自动测试的一个好办法确认它们按预期执行。</span><span class="sxs-lookup"><span data-stu-id="70c30-202">If your app exposes web APIs, it's a good idea to have automated tests confirm they execute as expected.</span></span> <span data-ttu-id="70c30-203">内置`TestServer`可以轻松地测试的 web Api。</span><span class="sxs-lookup"><span data-stu-id="70c30-203">The built-in `TestServer` makes it easy to test web APIs.</span></span> <span data-ttu-id="70c30-204">如果你的 API 方法使用模型绑定，应始终检查`ModelState.IsValid`，而集成测试正确的位置，以确认你的模型验证工作正常。</span><span class="sxs-lookup"><span data-stu-id="70c30-204">If your API methods are using model binding, you should always check `ModelState.IsValid`, and integration tests are the right place to confirm that your model validation is working properly.</span></span>

<span data-ttu-id="70c30-205">下面的一组测试目标`Create`中的方法[IdeasController](xref:mvc/controllers/testing#ideas-controller)上面所示的类：</span><span class="sxs-lookup"><span data-stu-id="70c30-205">The following set of tests target the `Create` method in the [IdeasController](xref:mvc/controllers/testing#ideas-controller) class shown above:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

<span data-ttu-id="70c30-206">与不同的操作返回 HTML 视图的集成测试，返回结果的 web API 方法通常可以反序列化作为强类型化对象，如上述最后一次测试所示。</span><span class="sxs-lookup"><span data-stu-id="70c30-206">Unlike integration tests of actions that returns HTML views, web API methods that return results can usually be deserialized as strongly typed objects, as the last test above shows.</span></span> <span data-ttu-id="70c30-207">在这种情况下，测试反序列化将结果发送到`BrainstormSession`实例，并确认思路已正确添加到其集合的想法。</span><span class="sxs-lookup"><span data-stu-id="70c30-207">In this case, the test deserializes the result to a `BrainstormSession` instance, and confirms that the idea was correctly added to its collection of ideas.</span></span>

<span data-ttu-id="70c30-208">你将这篇文章中找到的集成测试的其他示例[示例项目](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)。</span><span class="sxs-lookup"><span data-stu-id="70c30-208">You'll find additional examples of integration tests in this article's [sample project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample).</span></span>
