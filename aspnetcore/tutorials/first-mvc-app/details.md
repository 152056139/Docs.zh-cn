---
title: 检查 ASP.NET Core 应用的 Details 和 Delete 方法
author: rick-anderson
description: 了解基本 ASP.NET Core MVC 应用中的详细信息控制器方法和视图。
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: f7f9a369e3e612542140fcf1091b21037e530a91
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278643"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a><span data-ttu-id="e32f6-103">检查 ASP.NET Core 应用的 Details 和 Delete 方法</span><span class="sxs-lookup"><span data-stu-id="e32f6-103">Examine the Details and Delete methods of an ASP.NET Core app</span></span>

<span data-ttu-id="e32f6-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e32f6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e32f6-105">打开电影控制器，并检查 `Details` 方法：</span><span class="sxs-lookup"><span data-stu-id="e32f6-105">Open the Movie controller and examine the `Details` method:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="e32f6-106">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]</span><span class="sxs-lookup"><span data-stu-id="e32f6-106">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="e32f6-107">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span><span class="sxs-lookup"><span data-stu-id="e32f6-107">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span></span>

::: moniker-end

<span data-ttu-id="e32f6-108">创建此操作方法的 MVC 基架引擎添加显示调用方法的 HTTP 请求的注释。</span><span class="sxs-lookup"><span data-stu-id="e32f6-108">The MVC scaffolding engine that created this action method adds a comment showing an HTTP request that invokes the method.</span></span> <span data-ttu-id="e32f6-109">在此情况下，它是包含三个 URL 段的 GET 请求，这三个段为 `Movies` 控制器、`Details` 方法和 `id` 值。</span><span class="sxs-lookup"><span data-stu-id="e32f6-109">In this case it's a GET request with three URL segments, the `Movies` controller, the `Details` method and an `id` value.</span></span> <span data-ttu-id="e32f6-110">回顾这些在 Startup.cs 中定义的段。</span><span class="sxs-lookup"><span data-stu-id="e32f6-110">Recall these segments are defined in *Startup.cs*.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="e32f6-111">EF 可以使用 `SingleOrDefaultAsync` 方法轻松搜索数据。</span><span class="sxs-lookup"><span data-stu-id="e32f6-111">EF makes it easy to search for data using the `SingleOrDefaultAsync` method.</span></span> <span data-ttu-id="e32f6-112">该方法中内置的一个重要安全功能是，代码会先验证搜索方法已经找到电影，然后再执行操作。</span><span class="sxs-lookup"><span data-stu-id="e32f6-112">An important security feature built into the method is that the code verifies that the search method has found a movie before it tries to do anything with it.</span></span> <span data-ttu-id="e32f6-113">例如，一个黑客可能通过将链接创建的 URL 从 `http://localhost:xxxx/Movies/Details/1` 更改为类似 `http://localhost:xxxx/Movies/Details/12345` 的值（或者不代表任何实际电影的其他值）将错误引入站点。</span><span class="sxs-lookup"><span data-stu-id="e32f6-113">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like  `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="e32f6-114">如果未检查是否有空电影，则应用可能引发异常。</span><span class="sxs-lookup"><span data-stu-id="e32f6-114">If you didn't check for a null movie, the app would throw an exception.</span></span>

<span data-ttu-id="e32f6-115">检查 `Delete` 和 `DeleteConfirmed` 方法。</span><span class="sxs-lookup"><span data-stu-id="e32f6-115">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="e32f6-116">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]</span><span class="sxs-lookup"><span data-stu-id="e32f6-116">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="e32f6-117">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]</span><span class="sxs-lookup"><span data-stu-id="e32f6-117">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]</span></span>

::: moniker-end

<span data-ttu-id="e32f6-118">请注意，`HTTP GET Delete` 方法不删除指定的电影，而是返回可在其中提交 (HttpPost) 删除的电影视图。</span><span class="sxs-lookup"><span data-stu-id="e32f6-118">Note that the `HTTP GET Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (HttpPost) the deletion.</span></span> <span data-ttu-id="e32f6-119">执行删除操作以响应 GET 请求（或者说，执行编辑操作、创建操作或更改数据的任何其他操作）会打开安全漏洞。</span><span class="sxs-lookup"><span data-stu-id="e32f6-119">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span>

<span data-ttu-id="e32f6-120">删除数据的 `[HttpPost]` 方法命名为 `DeleteConfirmed`，以便为 HTTP POST 方法提供一个唯一的签名或名称。</span><span class="sxs-lookup"><span data-stu-id="e32f6-120">The `[HttpPost]` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="e32f6-121">下面显示了两个方法签名：</span><span class="sxs-lookup"><span data-stu-id="e32f6-121">The two method signatures are shown below:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


<span data-ttu-id="e32f6-122">公共语言运行时 (CLR) 需要重载方法拥有唯一的参数签名（相同的方法名称但不同的参数列表）。</span><span class="sxs-lookup"><span data-stu-id="e32f6-122">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="e32f6-123">但是，这里需要两个 `Delete` 方法 -- 一个用于 GET，另一个用于 POST -- 这两个方法拥有相同的参数签名。</span><span class="sxs-lookup"><span data-stu-id="e32f6-123">However, here you need two `Delete` methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="e32f6-124">（它们都需要接受单个整数作为参数。）</span><span class="sxs-lookup"><span data-stu-id="e32f6-124">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="e32f6-125">可通过两种方法解决此问题，一种是为方法提供不同的名称。</span><span class="sxs-lookup"><span data-stu-id="e32f6-125">There are two approaches to this problem, one is to give the methods different names.</span></span> <span data-ttu-id="e32f6-126">这正是前面的示例中的基架机制进行的操作。</span><span class="sxs-lookup"><span data-stu-id="e32f6-126">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="e32f6-127">但是，这会造成一个小问题：ASP.NET 按名称将 URL 段映射到操作方法，如果重命名方法，则路由通常无法找到该方法。</span><span class="sxs-lookup"><span data-stu-id="e32f6-127">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="e32f6-128">该示例中也提供了解决方案，即向 `DeleteConfirmed` 方法添加 `ActionName("Delete")` 属性。</span><span class="sxs-lookup"><span data-stu-id="e32f6-128">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="e32f6-129">该属性对路由系统执行映射，以便包括 POST 请求的 /Delete/ 的 URL可找到 `DeleteConfirmed` 方法。</span><span class="sxs-lookup"><span data-stu-id="e32f6-129">That attribute performs mapping for the routing system so that a URL that includes /Delete/ for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="e32f6-130">对于名称和签名相同的方法，另一个常用解决方法是手动更改 POST 方法的签名以包括额外（未使用）的参数。</span><span class="sxs-lookup"><span data-stu-id="e32f6-130">Another common work around for methods that have identical names and signatures is to artificially change the signature of the POST method to include an extra (unused) parameter.</span></span> <span data-ttu-id="e32f6-131">这正是前面的文章中添加 `notUsed` 参数时进行的操作。</span><span class="sxs-lookup"><span data-stu-id="e32f6-131">That's what we did in a previous post when we added the `notUsed` parameter.</span></span> <span data-ttu-id="e32f6-132">这里为了 `[HttpPost] Delete` 方法可以执行同样的操作：</span><span class="sxs-lookup"><span data-stu-id="e32f6-132">You could do the same thing here for the `[HttpPost] Delete` method:</span></span>

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a><span data-ttu-id="e32f6-133">发布到 Azure</span><span class="sxs-lookup"><span data-stu-id="e32f6-133">Publish to Azure</span></span>

<span data-ttu-id="e32f6-134">有关如何使用 Visual Studio 将该应用发布到 Azure 的说明，请参阅[使用 Visual Studio 将 ASP.NET Core Web 应用发布到 Azure App Service](xref:tutorials/publish-to-azure-webapp-using-vs)。</span><span class="sxs-lookup"><span data-stu-id="e32f6-134">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure using Visual Studio.</span></span>  <span data-ttu-id="e32f6-135">此外，还可以从[命令行](xref:tutorials/publish-to-azure-webapp-using-cli)发布应用。</span><span class="sxs-lookup"><span data-stu-id="e32f6-135">The app can also be published from the [command line](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e32f6-136">上一篇</span><span class="sxs-lookup"><span data-stu-id="e32f6-136">Previous</span></span>](validation.md)
