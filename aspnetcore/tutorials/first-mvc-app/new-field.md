---
title: 将新字段添加到 ASP.NET Core 应用
author: rick-anderson
description: 了解如何使用 Entity Framework Code First 迁移将新字段添加到模型，并将此更改迁移到数据库。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: a8299871671979264383fe7997e56c6708b2e741
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729751"
---
# <a name="add-a-new-field-to-an-aspnet-core-app"></a><span data-ttu-id="2c188-103">将新字段添加到 ASP.NET Core 应用</span><span class="sxs-lookup"><span data-stu-id="2c188-103">Add a new field to an ASP.NET Core app</span></span>

<span data-ttu-id="2c188-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2c188-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2c188-105">在本部分中，将使用 [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First 迁移将新字段添加到模型，并将此更改迁移到数据库。</span><span class="sxs-lookup"><span data-stu-id="2c188-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="2c188-106">使用 EF Code First 自动创建数据库时，Code First 会向数据库添加表格，以帮助跟踪数据库的架构是否与从其中生成它的模型类同步。</span><span class="sxs-lookup"><span data-stu-id="2c188-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="2c188-107">如果它们不同步，EF 则会引发异常。</span><span class="sxs-lookup"><span data-stu-id="2c188-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="2c188-108">这使查找不一致的数据库/代码问题变得更加轻松。</span><span class="sxs-lookup"><span data-stu-id="2c188-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="2c188-109">向电影模型添加分级属性</span><span class="sxs-lookup"><span data-stu-id="2c188-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="2c188-110">打开 Models/Movie.cs 文件，并添加 `Rating` 属性：</span><span class="sxs-lookup"><span data-stu-id="2c188-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="2c188-111">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="2c188-111">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="2c188-112">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span><span class="sxs-lookup"><span data-stu-id="2c188-112">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span></span>
::: moniker-end

<span data-ttu-id="2c188-113">生成应用 (Ctrl+Shift+B)。</span><span class="sxs-lookup"><span data-stu-id="2c188-113">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="2c188-114">因为已经添加新字段到 `Movie` 类，所以还需要更新绑定允许名单，将此新属性纳入其中。</span><span class="sxs-lookup"><span data-stu-id="2c188-114">Because you've added a new field to the `Movie` class, you also need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="2c188-115">在 MoviesController.cs 中，更新 `Create` 和 `Edit` 操作方法的 `[Bind]` 属性，以包括 `Rating` 属性：</span><span class="sxs-lookup"><span data-stu-id="2c188-115">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="2c188-116">还需要更新视图模板，以便在浏览器视图中显示、创建和编辑新的 `Rating` 属性。</span><span class="sxs-lookup"><span data-stu-id="2c188-116">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="2c188-117">编辑 /Views/Movies/Index.cshtml 文件并添加 `Rating` 字段：</span><span class="sxs-lookup"><span data-stu-id="2c188-117">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="2c188-118">使用 `Rating` 字段更新 /Views/Movies/Create.cshtml。</span><span class="sxs-lookup"><span data-stu-id="2c188-118">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="2c188-119">可以复制/粘贴之前的“窗体组”，并让 intelliSense 帮助更新字段。</span><span class="sxs-lookup"><span data-stu-id="2c188-119">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="2c188-120">IntelliSense 适用于[标记帮助程序](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="2c188-120">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="2c188-121">请注意：在 Visual Studio 2017 的 RTM 版本中，需要安装 Razor intelliSense 的 [Razor 语言服务](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices)。</span><span class="sxs-lookup"><span data-stu-id="2c188-121">Note: In the RTM verison of Visual Studio 2017 you need to install the [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) for Razor intelliSense.</span></span> <span data-ttu-id="2c188-122">此问题将在下一版本中修复。</span><span class="sxs-lookup"><span data-stu-id="2c188-122">This will be fixed in the next release.</span></span>

![开发人员已在视图的第二个标签元素中键入字母 R 用作 asp-for 的特性值。](new-field/_static/cr.png)

<span data-ttu-id="2c188-126">在将 DB 更新为包括新字段之前，应用不会正常工作。</span><span class="sxs-lookup"><span data-stu-id="2c188-126">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="2c188-127">如果现在运行，会出现以下 `SqlException`：</span><span class="sxs-lookup"><span data-stu-id="2c188-127">If you run it now, you'll get the following `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="2c188-128">看到此错误是因为更新的 Movie 模型类与现有数据库的 Movie 表架构不同。</span><span class="sxs-lookup"><span data-stu-id="2c188-128">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="2c188-129">（数据库表中没有“Rating”列。）</span><span class="sxs-lookup"><span data-stu-id="2c188-129">(There's no Rating column in the database table.)</span></span>

<span data-ttu-id="2c188-130">可通过几种方法解决此错误：</span><span class="sxs-lookup"><span data-stu-id="2c188-130">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="2c188-131">让 Entity Framework 自动丢弃，并基于新的模型类架构重新创建数据库。</span><span class="sxs-lookup"><span data-stu-id="2c188-131">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="2c188-132">在测试数据库上进行开发时，此方法在开发周期早期很方便；通过它可以一起快速改进模型和数据库架构。</span><span class="sxs-lookup"><span data-stu-id="2c188-132">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="2c188-133">但其缺点是会丢失数据库中的现有数据 - 因此请勿对生产数据库使用此方法！</span><span class="sxs-lookup"><span data-stu-id="2c188-133">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="2c188-134">使用初始值设定项，以使用测试数据自动设定数据库种子，这通常是开发应用程序的有效方式。</span><span class="sxs-lookup"><span data-stu-id="2c188-134">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span>

2. <span data-ttu-id="2c188-135">对现有数据库架构进行显式修改，使它与模型类相匹配。</span><span class="sxs-lookup"><span data-stu-id="2c188-135">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="2c188-136">此方法的优点是可以保留数据。</span><span class="sxs-lookup"><span data-stu-id="2c188-136">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="2c188-137">可以手动或通过创建数据库更改脚本进行此更改。</span><span class="sxs-lookup"><span data-stu-id="2c188-137">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="2c188-138">使用 Code First 迁移更新数据库架构。</span><span class="sxs-lookup"><span data-stu-id="2c188-138">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="2c188-139">本教程使用 Code First 迁移。</span><span class="sxs-lookup"><span data-stu-id="2c188-139">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="2c188-140">更新 `SeedData` 类，使它提供新列的值。</span><span class="sxs-lookup"><span data-stu-id="2c188-140">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="2c188-141">示例更改如下所示，但可能需要对每个 `new Movie` 做出此更改。</span><span class="sxs-lookup"><span data-stu-id="2c188-141">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="2c188-142">生成解决方案。</span><span class="sxs-lookup"><span data-stu-id="2c188-142">Build the solution.</span></span>

<span data-ttu-id="2c188-143">从“工具”菜单中，选择“NuGet 包管理器”>“包管理器控制台”。</span><span class="sxs-lookup"><span data-stu-id="2c188-143">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![PMC 菜单](adding-model/_static/pmc.png)

<span data-ttu-id="2c188-145">在 PMC 中，输入以下命令：</span><span class="sxs-lookup"><span data-stu-id="2c188-145">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="2c188-146">`Add-Migration` 命令会通知迁移框架使用当前 `Movie` DB 架构检查当前 `Movie` 模型，并创建必要的代码，将 DB 迁移到新模型。</span><span class="sxs-lookup"><span data-stu-id="2c188-146">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="2c188-147">名称“Rating”是任意的，用于对迁移文件进行命名。</span><span class="sxs-lookup"><span data-stu-id="2c188-147">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="2c188-148">为迁移文件使用有意义的名称是有帮助的。</span><span class="sxs-lookup"><span data-stu-id="2c188-148">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="2c188-149">如果删除 DB 中的所有记录，初始化会设定 DB 种子，并将包括 `Rating` 字段。</span><span class="sxs-lookup"><span data-stu-id="2c188-149">If you delete all the records in the DB, the initialize will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="2c188-150">可以使用浏览器中的删除链接，也可从 SSOX 执行此操作。</span><span class="sxs-lookup"><span data-stu-id="2c188-150">You can do this with the delete links in the browser or from SSOX.</span></span>

<span data-ttu-id="2c188-151">运行应用，并验证是否可以创建/编辑/显示具有 `Rating` 字段的电影。</span><span class="sxs-lookup"><span data-stu-id="2c188-151">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="2c188-152">还应向 `Edit`、`Details` 和 `Delete` 视图模板添加 `Rating` 字段。</span><span class="sxs-lookup"><span data-stu-id="2c188-152">You should also add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2c188-153">[上一页](search.md)
> [下一页](validation.md)</span><span class="sxs-lookup"><span data-stu-id="2c188-153">[Previous](search.md)
[Next](validation.md)</span></span>  
