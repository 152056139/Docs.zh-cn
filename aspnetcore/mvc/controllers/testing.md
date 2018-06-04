---
title: ASP.NET Core 中的测试控制器逻辑
author: ardalis
description: 了解如何使用 Moq 和 xUnit 测试 ASP.NET Core 中的控制器逻辑。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/testing
ms.openlocfilehash: a0073e4de361c37a6854ceaf54ffd9eaea4837d4
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2018
ms.locfileid: "34567044"
---
# <a name="test-controller-logic-in-aspnet-core"></a><span data-ttu-id="610b8-103">ASP.NET Core 中的测试控制器逻辑</span><span class="sxs-lookup"><span data-stu-id="610b8-103">Test controller logic in ASP.NET Core</span></span>

<span data-ttu-id="610b8-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="610b8-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="610b8-105">ASP.NET MVC 应用的控制器应该是小型的，且重点应对用户界面问题。</span><span class="sxs-lookup"><span data-stu-id="610b8-105">Controllers in ASP.NET MVC apps should be small and focused on user-interface concerns.</span></span> <span data-ttu-id="610b8-106">处理非 UI 问题的大型控制器更难以测试和维护。</span><span class="sxs-lookup"><span data-stu-id="610b8-106">Large controllers that deal with non-UI concerns are more difficult to test and maintain.</span></span>

[<span data-ttu-id="610b8-107">查看或下载 GitHub 中的示例</span><span class="sxs-lookup"><span data-stu-id="610b8-107">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a><span data-ttu-id="610b8-108">测试控制器</span><span class="sxs-lookup"><span data-stu-id="610b8-108">Testing controllers</span></span>

<span data-ttu-id="610b8-109">控制器是任意 ASP.NET Core MVC 应用程序的核心部分。</span><span class="sxs-lookup"><span data-stu-id="610b8-109">Controllers are a central part of any ASP.NET Core MVC application.</span></span> <span data-ttu-id="610b8-110">因此，应该对它们按应用计划表现有信心。</span><span class="sxs-lookup"><span data-stu-id="610b8-110">As such, you should have confidence they behave as intended for your app.</span></span> <span data-ttu-id="610b8-111">自动测试可以给你这种信心，还能在生成前检测错误。</span><span class="sxs-lookup"><span data-stu-id="610b8-111">Automated tests can provide you with this confidence and can detect errors before they reach production.</span></span> <span data-ttu-id="610b8-112">避免在控制器中安置不必要的职责并确保测试仅专注于控制器职责非常重要。</span><span class="sxs-lookup"><span data-stu-id="610b8-112">It's important to avoid placing unnecessary responsibilities within your controllers and ensure your tests focus only on controller responsibilities.</span></span>

<span data-ttu-id="610b8-113">控制器应采用最简单的逻辑，而不应关注业务逻辑或基础结构问题（例如数据访问）。</span><span class="sxs-lookup"><span data-stu-id="610b8-113">Controller logic should be minimal and not be focused on business logic or infrastructure concerns (for example, data access).</span></span> <span data-ttu-id="610b8-114">测试控制器逻辑，而不是框架。</span><span class="sxs-lookup"><span data-stu-id="610b8-114">Test controller logic, not the framework.</span></span> <span data-ttu-id="610b8-115">测试控制器基于有效或无效输入的行为。</span><span class="sxs-lookup"><span data-stu-id="610b8-115">Test how the controller *behaves* based on valid or invalid inputs.</span></span> <span data-ttu-id="610b8-116">测试控制器如何相应其所执行业务操作的结果。</span><span class="sxs-lookup"><span data-stu-id="610b8-116">Test controller responses based on the result of the business operation it performs.</span></span>

<span data-ttu-id="610b8-117">典型控制器职责：</span><span class="sxs-lookup"><span data-stu-id="610b8-117">Typical controller responsibilities:</span></span>

* <span data-ttu-id="610b8-118">验证 `ModelState.IsValid`。</span><span class="sxs-lookup"><span data-stu-id="610b8-118">Verify `ModelState.IsValid`.</span></span>
* <span data-ttu-id="610b8-119">如果 `ModelState` 无效，则返回错误响应。</span><span class="sxs-lookup"><span data-stu-id="610b8-119">Return an error response if `ModelState` is invalid.</span></span>
* <span data-ttu-id="610b8-120">从持久性检索业务实体。</span><span class="sxs-lookup"><span data-stu-id="610b8-120">Retrieve a business entity from persistence.</span></span>
* <span data-ttu-id="610b8-121">对业务实体执行操作。</span><span class="sxs-lookup"><span data-stu-id="610b8-121">Perform an action on the business entity.</span></span>
* <span data-ttu-id="610b8-122">将业务实体保存到持久性。</span><span class="sxs-lookup"><span data-stu-id="610b8-122">Save the business entity to persistence.</span></span>
* <span data-ttu-id="610b8-123">返回相应的 `IActionResult`。</span><span class="sxs-lookup"><span data-stu-id="610b8-123">Return an appropriate `IActionResult`.</span></span>

## <a name="unit-testing"></a><span data-ttu-id="610b8-124">单元测试</span><span class="sxs-lookup"><span data-stu-id="610b8-124">Unit testing</span></span>

<span data-ttu-id="610b8-125">[单元测试](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)涉及在测试应用程序部分时，使之与它的基础结构和依赖关系相隔离。</span><span class="sxs-lookup"><span data-stu-id="610b8-125">[Unit testing](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) involves testing a part of an app in isolation from its infrastructure and dependencies.</span></span> <span data-ttu-id="610b8-126">单元测试控制器逻辑时，仅测试单个操作的内容，不测试其依赖项或框架自身的行为。</span><span class="sxs-lookup"><span data-stu-id="610b8-126">When unit testing controller logic, only the contents of a single action is tested, not the behavior of its dependencies or of the framework itself.</span></span> <span data-ttu-id="610b8-127">单元测试控制器操作时，请确保仅关注其行为。</span><span class="sxs-lookup"><span data-stu-id="610b8-127">As you unit test your controller actions, make sure you focus only on its behavior.</span></span> <span data-ttu-id="610b8-128">控制器单元测试将避开[筛选器](filters.md)、[路由](../../fundamentals/routing.md)或[模型绑定](../models/model-binding.md)等。</span><span class="sxs-lookup"><span data-stu-id="610b8-128">A controller unit test avoids things like [filters](filters.md), [routing](../../fundamentals/routing.md), or [model binding](../models/model-binding.md).</span></span> <span data-ttu-id="610b8-129">仅专注测试一项内容，单位测试通常编写简单、运行迅速。</span><span class="sxs-lookup"><span data-stu-id="610b8-129">By focusing on testing just one thing, unit tests are generally simple to write and quick to run.</span></span> <span data-ttu-id="610b8-130">正确编写的单位测试集无需过多开销即可经常运行。</span><span class="sxs-lookup"><span data-stu-id="610b8-130">A well-written set of unit tests can be run frequently without much overhead.</span></span> <span data-ttu-id="610b8-131">但单元测试不检测组件间交互的问题，[集成测试](xref:mvc/controllers/testing#integration-testing)才会检测。</span><span class="sxs-lookup"><span data-stu-id="610b8-131">However, unit tests don't detect issues in the interaction between components, which is the purpose of [integration tests](xref:mvc/controllers/testing#integration-testing).</span></span>

<span data-ttu-id="610b8-132">如果编写自定义筛选器、路由等，应对其进行单位测试，而不是作为特定控制器操作测试的一部分进行。</span><span class="sxs-lookup"><span data-stu-id="610b8-132">If you're writing custom filters, routes, etc, you should unit test them, but not as part of your tests on a particular controller action.</span></span> <span data-ttu-id="610b8-133">应该对其进行隔离测试。</span><span class="sxs-lookup"><span data-stu-id="610b8-133">They should be tested in isolation.</span></span>

> [!TIP]
> <span data-ttu-id="610b8-134">[使用 Visual Studio 创建和运行单位测试](/visualstudio/test/unit-test-your-code)。</span><span class="sxs-lookup"><span data-stu-id="610b8-134">[Create and run unit tests with Visual Studio](/visualstudio/test/unit-test-your-code).</span></span>

<span data-ttu-id="610b8-135">若要演示单位测试，请查看以下控制器。</span><span class="sxs-lookup"><span data-stu-id="610b8-135">To demonstrate unit testing, review the following controller.</span></span> <span data-ttu-id="610b8-136">它显示集体讨论会话的列表并允许使用 POST 创建新的集体讨论会话：</span><span class="sxs-lookup"><span data-stu-id="610b8-136">It displays a list of brainstorming sessions and allows new brainstorming sessions to be created with a POST:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

<span data-ttu-id="610b8-137">控制器遵循[显式依赖关系原则](http://deviq.com/explicit-dependencies-principle/)，需要依赖关系注入提供 `IBrainstormSessionRepository` 实例。</span><span class="sxs-lookup"><span data-stu-id="610b8-137">The controller is following the [explicit dependencies principle](http://deviq.com/explicit-dependencies-principle/), expecting dependency injection to provide it with an instance of `IBrainstormSessionRepository`.</span></span> <span data-ttu-id="610b8-138">因此使用 mock 对象框架（例如 [Moq](https://www.nuget.org/packages/Moq/)）进行测试很简单。</span><span class="sxs-lookup"><span data-stu-id="610b8-138">This makes it fairly easy to test using a mock object framework, like [Moq](https://www.nuget.org/packages/Moq/).</span></span> <span data-ttu-id="610b8-139">`HTTP GET Index` 方法没有循环或分支，且仅调用一个方法。</span><span class="sxs-lookup"><span data-stu-id="610b8-139">The `HTTP GET Index` method has no looping or branching and only calls one method.</span></span> <span data-ttu-id="610b8-140">若要测试此 `Index` 方法，我们需要验证返回了 `ViewResult`，附带来自存储库 `List` 方法的 `ViewModel`。</span><span class="sxs-lookup"><span data-stu-id="610b8-140">To test this `Index` method, we need to verify that a `ViewResult` is returned, with a `ViewModel` from the repository's `List` method.</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

<span data-ttu-id="610b8-141">`HomeController` `HTTP POST Index` 方法（如上所示）应该验证：</span><span class="sxs-lookup"><span data-stu-id="610b8-141">The `HomeController` `HTTP POST Index` method (shown above) should verify:</span></span>

* <span data-ttu-id="610b8-142">操作方法在 `ModelState.IsValid` 是 `false` 时返回有相应数据的错误请求 `ViewResult`</span><span class="sxs-lookup"><span data-stu-id="610b8-142">The action method returns a Bad Request `ViewResult` with the appropriate data when `ModelState.IsValid` is `false`</span></span>

* <span data-ttu-id="610b8-143">`ModelState.IsValid` 为 true 时，调用存储库上的 `Add` 方法并返回有正确参数的 `RedirectToActionResult`。</span><span class="sxs-lookup"><span data-stu-id="610b8-143">The `Add` method on the repository is called and a `RedirectToActionResult` is returned with the correct arguments when `ModelState.IsValid` is true.</span></span>

<span data-ttu-id="610b8-144">通过使用 `AddModelError` 添加错误，可以测试无效模型状态，如下第一个测试所示。</span><span class="sxs-lookup"><span data-stu-id="610b8-144">Invalid model state can be tested by adding errors using `AddModelError` as shown in the first test below.</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

<span data-ttu-id="610b8-145">第一个测试确认在 `ModelState` 无效时，返回与 `GET` 请求相同的 `ViewResult`。</span><span class="sxs-lookup"><span data-stu-id="610b8-145">The first test confirms when `ModelState` isn't valid, the same `ViewResult` is returned as for a `GET` request.</span></span> <span data-ttu-id="610b8-146">请注意，测试不会尝试传入无效模型。</span><span class="sxs-lookup"><span data-stu-id="610b8-146">Note that the test doesn't attempt to pass in an invalid model.</span></span> <span data-ttu-id="610b8-147">即使传入也无济于事，因为模型绑定不会运行（虽然[集成测试](xref:mvc/controllers/testing#integration-testing)将使用练习模型绑定）。</span><span class="sxs-lookup"><span data-stu-id="610b8-147">That wouldn't work anyway since model binding isn't running (though an [integration test](xref:mvc/controllers/testing#integration-testing) would use exercise model binding).</span></span> <span data-ttu-id="610b8-148">在本例中，不测试模型绑定。</span><span class="sxs-lookup"><span data-stu-id="610b8-148">In this case, model binding isn't being tested.</span></span> <span data-ttu-id="610b8-149">这些单位测试仅测试操作方法中代码的行为。</span><span class="sxs-lookup"><span data-stu-id="610b8-149">These unit tests are only testing what the code in the action method does.</span></span>

<span data-ttu-id="610b8-150">第二个测试验证 `ModelState` 有效时，（通过存储库）添加了新的 `BrainstormSession`，且该方法返回有所需属性的 `RedirectToActionResult`。</span><span class="sxs-lookup"><span data-stu-id="610b8-150">The second test verifies that when `ModelState` is valid, a new `BrainstormSession` is added (via the repository), and the method returns a `RedirectToActionResult` with the expected properties.</span></span> <span data-ttu-id="610b8-151">通常会忽略未调用的模拟调用，但在设置调用末尾调用 `Verifiable` 就可以在测试中对其进行验证。</span><span class="sxs-lookup"><span data-stu-id="610b8-151">Mocked calls that aren't called are normally ignored, but calling `Verifiable` at the end of the setup call allows it to be verified in the test.</span></span> <span data-ttu-id="610b8-152">这通过对 `mockRepo.Verify` 的调用完成，进行这种调用时，如果未调用所需方法，则测试将失败。</span><span class="sxs-lookup"><span data-stu-id="610b8-152">This is done with the call to `mockRepo.Verify`, which will fail the test if the expected method wasn't called.</span></span>

> [!NOTE]
> <span data-ttu-id="610b8-153">通过此示例中使用的 Moq 库，可轻松混合可验证（或称“严格”）mock 和非可验证 mock（也称为“宽松”mock 或存根）。</span><span class="sxs-lookup"><span data-stu-id="610b8-153">The Moq library used in this sample makes it easy to mix verifiable, or "strict", mocks with non-verifiable mocks (also called "loose" mocks or stubs).</span></span> <span data-ttu-id="610b8-154">详细了解[使用 Moq 自定义 Mock 行为](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior)。</span><span class="sxs-lookup"><span data-stu-id="610b8-154">Learn more about [customizing Mock behavior with Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).</span></span>

<span data-ttu-id="610b8-155">应用中的另一个控制器显示与特定集体讨论会话相关的信息。</span><span class="sxs-lookup"><span data-stu-id="610b8-155">Another controller in the app displays information related to a particular brainstorming session.</span></span> <span data-ttu-id="610b8-156">它包括用于处理无效 ID 值的逻辑：</span><span class="sxs-lookup"><span data-stu-id="610b8-156">It includes some logic to deal with invalid id values:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

<span data-ttu-id="610b8-157">控制器操作有三个要测试的事例，每个 `return` 语句对应一个：</span><span class="sxs-lookup"><span data-stu-id="610b8-157">The controller action has three cases to test, one for each `return` statement:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

<span data-ttu-id="610b8-158">应用将功能公开为 Web API（与集体讨论会话相关的想法的列表，以及将新想法添加到会话的方法）：</span><span class="sxs-lookup"><span data-stu-id="610b8-158">The app exposes functionality as a web API (a list of ideas associated with a brainstorming session and a method for adding new ideas to a session):</span></span>

<a name="ideas-controller"></a>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

<span data-ttu-id="610b8-159">`ForSession` 方法返回 `IdeaDTO` 类型的列表。</span><span class="sxs-lookup"><span data-stu-id="610b8-159">The `ForSession` method returns a list of `IdeaDTO` types.</span></span> <span data-ttu-id="610b8-160">避免直接通过 API 调用返回业务域实体，因为它们常包含 API 客户端所需以外的数据，且它们会不必要地将应用的内部域模型与外部公开的 API 配对。</span><span class="sxs-lookup"><span data-stu-id="610b8-160">Avoid returning your business domain entities directly via API calls, since frequently they include more data than the API client requires, and they unnecessarily couple your app's internal domain model with the API you expose externally.</span></span> <span data-ttu-id="610b8-161">域实体和通过网络返回的类型之间的映射可以手动（使用 LINQ `Select`，如此处所示）或使用 [AutoMapper](https://github.com/AutoMapper/AutoMapper) 等库完成</span><span class="sxs-lookup"><span data-stu-id="610b8-161">Mapping between domain entities and the types you will return over the wire can be done manually (using a LINQ `Select` as shown here) or using a library like [AutoMapper](https://github.com/AutoMapper/AutoMapper)</span></span>

<span data-ttu-id="610b8-162">`Create` 和 `ForSession` API 方法的单位测试：</span><span class="sxs-lookup"><span data-stu-id="610b8-162">The unit tests for the `Create` and `ForSession` API methods:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

<span data-ttu-id="610b8-163">如前所述，若要测试方法在 `ModelState` 无效时的行为，请在测试zho 将模型错误添加到控制器。</span><span class="sxs-lookup"><span data-stu-id="610b8-163">As stated previously, to test the behavior of the method when `ModelState` is invalid, add a model error to the controller as part of the test.</span></span> <span data-ttu-id="610b8-164">请勿在单位测试中尝试测试模型有效性或模型绑定 - 仅测试操作方法在遇到特定 `ModelState` 值时的行为。</span><span class="sxs-lookup"><span data-stu-id="610b8-164">Don't try to test model validation or model binding in your unit tests - just test your action method's behavior when confronted with a particular `ModelState` value.</span></span>

<span data-ttu-id="610b8-165">第二个测试依赖存储库返回 null，所以 mock 存储库配置为返回 null。</span><span class="sxs-lookup"><span data-stu-id="610b8-165">The second test depends on the repository returning null, so the mock repository is configured to return null.</span></span> <span data-ttu-id="610b8-166">无需创建测试数据库（在内存中或其他位置）并构建将返回此结果的查询 - 它可以在单个语句中完成，如下所示。</span><span class="sxs-lookup"><span data-stu-id="610b8-166">There's no need to create a test database (in memory or otherwise) and construct a query that will return this result - it can be done in a single statement as shown.</span></span>

<span data-ttu-id="610b8-167">最后一个测试验证调用了存储库的 `Update` 方法。</span><span class="sxs-lookup"><span data-stu-id="610b8-167">The last test verifies that the repository's `Update` method is called.</span></span> <span data-ttu-id="610b8-168">正如我们之前所做，使用 `Verifiable` 调用 mock，然后调用模拟存储库的 `Verify` 方法，以确认执行了可验证方法。</span><span class="sxs-lookup"><span data-stu-id="610b8-168">As we did previously, the mock is called with `Verifiable` and then the mocked repository's `Verify` method is called to confirm the verifiable method was executed.</span></span> <span data-ttu-id="610b8-169">确保 `Update` 方法保存数据不是单位测试的职责，这可以通过集成测试完成。</span><span class="sxs-lookup"><span data-stu-id="610b8-169">It's not a unit test responsibility to ensure that the `Update` method saved the data; that can be done with an integration test.</span></span>

## <a name="integration-testing"></a><span data-ttu-id="610b8-170">集成测试</span><span class="sxs-lookup"><span data-stu-id="610b8-170">Integration testing</span></span>

<span data-ttu-id="610b8-171">完成[集成测试](xref:test/integration-tests)，可确保应用中各独立模块正确协作。</span><span class="sxs-lookup"><span data-stu-id="610b8-171">[Integration tests](xref:test/integration-tests) is done to ensure separate modules within your app work correctly together.</span></span> <span data-ttu-id="610b8-172">通常情况下，可以使用单位测试测试的内容也可以使用集成测试测试，但反之不成立。</span><span class="sxs-lookup"><span data-stu-id="610b8-172">Generally, anything you can test with a unit test, you can also test with an integration test, but the reverse isn't true.</span></span> <span data-ttu-id="610b8-173">但是，集成测试比单位测试慢。</span><span class="sxs-lookup"><span data-stu-id="610b8-173">However, integration tests tend to be much slower than unit tests.</span></span> <span data-ttu-id="610b8-174">因此，最好使用单位测试测试其可测内容，并在设计多个协作者时使用集成测试。</span><span class="sxs-lookup"><span data-stu-id="610b8-174">Thus, it's best to test whatever you can with unit tests, and use integration tests for scenarios that involve multiple collaborators.</span></span>

<span data-ttu-id="610b8-175">虽然它们也可能起作用，但是 mock 对象很少在集成测试中使用。</span><span class="sxs-lookup"><span data-stu-id="610b8-175">Although they may still be useful, mock objects are rarely used in integration tests.</span></span> <span data-ttu-id="610b8-176">在单位测试中，mock 对象是出于测试目的控制测试单位外协作者行为的有效方法。</span><span class="sxs-lookup"><span data-stu-id="610b8-176">In unit testing, mock objects are an effective way to control how collaborators outside of the unit being tested should behave for the purposes of the test.</span></span> <span data-ttu-id="610b8-177">在集成测试中，使用真实协作者确定整个子系统以正确的方式协同工作。</span><span class="sxs-lookup"><span data-stu-id="610b8-177">In an integration test, real collaborators are used to confirm the whole subsystem works together correctly.</span></span>

### <a name="application-state"></a><span data-ttu-id="610b8-178">应用程序状态</span><span class="sxs-lookup"><span data-stu-id="610b8-178">Application state</span></span>

<span data-ttu-id="610b8-179">执行集成测试时一个重要考虑因素是如何设置应用状态。</span><span class="sxs-lookup"><span data-stu-id="610b8-179">One important consideration when performing integration testing is how to set your app's state.</span></span> <span data-ttu-id="610b8-180">测试需要互相独立地运行，因此每个测试都应从处于已知状态的应用开始。</span><span class="sxs-lookup"><span data-stu-id="610b8-180">Tests need to run independent of one another, and so each test should start with the app in a known state.</span></span> <span data-ttu-id="610b8-181">如果应用不使用数据库或有任何持久性，这可能不是问题。</span><span class="sxs-lookup"><span data-stu-id="610b8-181">If your app doesn't use a database or have any persistence, this may not be an issue.</span></span> <span data-ttu-id="610b8-182">但是，大多实际应用将其状态保存到某些类型的数据存储，所以一个测试作出的任意修改都可能影响到另一个测试，除非重置数据存储。</span><span class="sxs-lookup"><span data-stu-id="610b8-182">However, most real-world apps persist their state to some kind of data store, so any modifications made by one test could impact another test unless the data store is reset.</span></span> <span data-ttu-id="610b8-183">使用内置 `TestServer`，在集成测试中承载 ASP.NET Core 应用非常简单，但是这并不一定会授予对其要使用的数据的访问权限。</span><span class="sxs-lookup"><span data-stu-id="610b8-183">Using the built-in `TestServer`, it's very straightforward to host ASP.NET Core apps within our integration tests, but that doesn't necessarily grant access to the data it will use.</span></span> <span data-ttu-id="610b8-184">如果正在使用实际数据库，则一种方法是让应用连接到测试可以访问的测试数据库，并确保其在每个测试执行前重置到已知状态。</span><span class="sxs-lookup"><span data-stu-id="610b8-184">If you're using an actual database, one approach is to have the app connect to a test database, which your tests can access and ensure is reset to a known state before each test executes.</span></span>

<span data-ttu-id="610b8-185">在此示例应用程序中，我是用的是 Entity Framework Core 的 InMemoryDatabase 支持，所以我不能从测试项目直接连接到它。</span><span class="sxs-lookup"><span data-stu-id="610b8-185">In this sample application, I'm using Entity Framework Core's InMemoryDatabase support, so I can't just connect to it from my test project.</span></span> <span data-ttu-id="610b8-186">相反，我公开应用 `Startup` 类的 `InitializeDatabase` 方法，我在应用在 `Development` 环境中启动时调用该类。</span><span class="sxs-lookup"><span data-stu-id="610b8-186">Instead, I expose an `InitializeDatabase` method from the app's `Startup` class, which I call when the app starts up if it's in the `Development` environment.</span></span> <span data-ttu-id="610b8-187">只要集成测试将环境设置为 `Development`，就能自动从中获益。</span><span class="sxs-lookup"><span data-stu-id="610b8-187">My integration tests automatically benefit from this as long as they set the environment to `Development`.</span></span> <span data-ttu-id="610b8-188">我无需担心重置数据库，因为每次应用重启时都会重置 InMemoryDatabase。</span><span class="sxs-lookup"><span data-stu-id="610b8-188">I don't have to worry about resetting the database, since the InMemoryDatabase is reset each time the app restarts.</span></span>

<span data-ttu-id="610b8-189">`Startup` 类：</span><span class="sxs-lookup"><span data-stu-id="610b8-189">The `Startup` class:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

<span data-ttu-id="610b8-190">在以下集成测试中，可以看到频繁使用 `GetTestSession` 方法。</span><span class="sxs-lookup"><span data-stu-id="610b8-190">You'll see the `GetTestSession` method used frequently in the integration tests below.</span></span>

### <a name="accessing-views"></a><span data-ttu-id="610b8-191">访问视图</span><span class="sxs-lookup"><span data-stu-id="610b8-191">Accessing views</span></span>

<span data-ttu-id="610b8-192">每个集成测试类都会配置将运行 ASP.NET Core 应用的 `TestServer`。</span><span class="sxs-lookup"><span data-stu-id="610b8-192">Each integration test class configures the `TestServer` that will run the ASP.NET Core app.</span></span> <span data-ttu-id="610b8-193">默认情况下，`TestServer` 会将 Web 应用存储在运行该应用的文件夹（本例中即测试项目文件夹）。</span><span class="sxs-lookup"><span data-stu-id="610b8-193">By default, `TestServer` hosts the web app in the folder where it's running - in this case, the test project folder.</span></span> <span data-ttu-id="610b8-194">因此，当尝试测试返回 `ViewResult` 的控制器操作时，可能看到此错误：</span><span class="sxs-lookup"><span data-stu-id="610b8-194">Thus, when you attempt to test controller actions that return `ViewResult`, you may see this error:</span></span>

```
The view 'Index' wasn't found. The following locations were searched:
(list of locations)
```

<span data-ttu-id="610b8-195">若要更正此问题，需要配置服务器的内容根，使其可以定位到被测试项目的视图。</span><span class="sxs-lookup"><span data-stu-id="610b8-195">To correct this issue, you need to configure the server's content root, so it can locate the views for the project being tested.</span></span> <span data-ttu-id="610b8-196">这可以通过调用 `TestFixture` 类中的 `UseContentRoot` 完成，如下所示：</span><span class="sxs-lookup"><span data-stu-id="610b8-196">This is done by a call to `UseContentRoot` in the `TestFixture` class, shown below:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

<span data-ttu-id="610b8-197">`TestFixture` 类负责配置和创建 `TestServer`，设置 `HttpClient` 与 `TestServer` 通信。</span><span class="sxs-lookup"><span data-stu-id="610b8-197">The `TestFixture` class is responsible for configuring and creating the `TestServer`, setting up an `HttpClient` to communicate with the `TestServer`.</span></span> <span data-ttu-id="610b8-198">每个集成测试使用 `Client` 属性连接到测试服务器并发出请求。</span><span class="sxs-lookup"><span data-stu-id="610b8-198">Each of the integration tests uses the `Client` property to connect to the test server and make a request.</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

<span data-ttu-id="610b8-199">在上面的第一个测试中，`responseString` 保持“视图”的实际呈现 HTML，可以检查它以确定其包含所需结果。</span><span class="sxs-lookup"><span data-stu-id="610b8-199">In the first test above, the `responseString` holds the actual rendered HTML from the View, which can be inspected to confirm it contains expected results.</span></span>

<span data-ttu-id="610b8-200">第二个测试构造具有唯一会话名称的窗体 POST，并将其发布到应用，然后验证返回了预期重定向。</span><span class="sxs-lookup"><span data-stu-id="610b8-200">The second test constructs a form POST with a unique session name and POSTs it to the app, then verifies that the expected redirect is returned.</span></span>

### <a name="api-methods"></a><span data-ttu-id="610b8-201">API 方法</span><span class="sxs-lookup"><span data-stu-id="610b8-201">API methods</span></span>

<span data-ttu-id="610b8-202">如果应用公开 Web API，最好用自动测试确定它们按照预期执行。</span><span class="sxs-lookup"><span data-stu-id="610b8-202">If your app exposes web APIs, it's a good idea to have automated tests confirm they execute as expected.</span></span> <span data-ttu-id="610b8-203">内置 `TestServer` 使得测试 Web API 更简单。</span><span class="sxs-lookup"><span data-stu-id="610b8-203">The built-in `TestServer` makes it easy to test web APIs.</span></span> <span data-ttu-id="610b8-204">如果 API 方法使用模型绑定，则应始终检查 `ModelState.IsValid`，并用集成测试确定模型验证正确工作。</span><span class="sxs-lookup"><span data-stu-id="610b8-204">If your API methods are using model binding, you should always check `ModelState.IsValid`, and integration tests are the right place to confirm that your model validation is working properly.</span></span>

<span data-ttu-id="610b8-205">以下测试集以 [IdeasController](xref:mvc/controllers/testing#ideas-controller) 类中的 `Create` 方法为目标，如下所示：</span><span class="sxs-lookup"><span data-stu-id="610b8-205">The following set of tests target the `Create` method in the [IdeasController](xref:mvc/controllers/testing#ideas-controller) class shown above:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

<span data-ttu-id="610b8-206">不同于返回 HTML 视图的操作集成测试，返回结果的 Web API 方法常常可以反序列化为强类型对象，如上面的最后一个测试所示。</span><span class="sxs-lookup"><span data-stu-id="610b8-206">Unlike integration tests of actions that returns HTML views, web API methods that return results can usually be deserialized as strongly typed objects, as the last test above shows.</span></span> <span data-ttu-id="610b8-207">在此情况下，测试将结果反序列化到 `BrainstormSession` 实例，并确定想法正确添加到其想法集。</span><span class="sxs-lookup"><span data-stu-id="610b8-207">In this case, the test deserializes the result to a `BrainstormSession` instance, and confirms that the idea was correctly added to its collection of ideas.</span></span>

<span data-ttu-id="610b8-208">可在本文的[示例项目](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)中找到集成测试的其他示例。</span><span class="sxs-lookup"><span data-stu-id="610b8-208">You'll find additional examples of integration tests in this article's [sample project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample).</span></span>
