---
title: "Razor 页面和 EF Core - CRUD - 第 2 个教程（共 8 个）"
author: rick-anderson
description: "演示如何使用 EF Core 进行创建、读取、更新和删除"
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/crud
ms.openlocfilehash: 757aeb713b645cea0fe633b150784184d2d3571e
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2018
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a><span data-ttu-id="bdf25-103">创建、读取、更新和删除 - EF Core 和 Razor 页面（第 2 个教程，共 8 个）</span><span class="sxs-lookup"><span data-stu-id="bdf25-103">Create, Read, Update, and Delete - EF Core with Razor Pages (2 of 8)</span></span>

<span data-ttu-id="bdf25-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bdf25-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="bdf25-105">本教程将介绍和自定义已搭建基架的 CRUD （创建、读取、更新、删除）代码。</span><span class="sxs-lookup"><span data-stu-id="bdf25-105">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="bdf25-106">请注意：为最大程度降低复杂性并让这些教程集中介绍 EF Core，“Razor 页面”页面模型中将使用 EF Core 代码。</span><span class="sxs-lookup"><span data-stu-id="bdf25-106">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages page models.</span></span> <span data-ttu-id="bdf25-107">某些开发人员使用服务层或存储库模式在 UI（Razor 页面）和数据访问层之间创建抽象层。</span><span class="sxs-lookup"><span data-stu-id="bdf25-107">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="bdf25-108">本教程将修改“学生”文件夹中的“创建”、“编辑”、“删除”和“详细信息”Razor 页面。</span><span class="sxs-lookup"><span data-stu-id="bdf25-108">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="bdf25-109">基架代码将以下模式用于“创建”、“编辑”和“删除”页面：</span><span class="sxs-lookup"><span data-stu-id="bdf25-109">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="bdf25-110">使用 HTTP GET 方法 `OnGetAsync` 获取和显示请求数据。</span><span class="sxs-lookup"><span data-stu-id="bdf25-110">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="bdf25-111">使用 HTTP POST 方法 `OnPostAsync` 将更改保存到数据。</span><span class="sxs-lookup"><span data-stu-id="bdf25-111">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="bdf25-112">“索引”和“详细信息”页面使用 HTTP GET 方法 `OnGetAsync` 获取和显示请求数据</span><span class="sxs-lookup"><span data-stu-id="bdf25-112">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="bdf25-113">将 SingleOrDefaultAsync 替换为 FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="bdf25-113">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="bdf25-114">生成的代码使用 [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) 提取请求的实体。</span><span class="sxs-lookup"><span data-stu-id="bdf25-114">The generated code uses [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="bdf25-115">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) 在提取一个实体时更为高效：</span><span class="sxs-lookup"><span data-stu-id="bdf25-115">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="bdf25-116">代码需要验证查询仅返回一个实体时除外。</span><span class="sxs-lookup"><span data-stu-id="bdf25-116">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="bdf25-117">`SingleOrDefaultAsync` 会提取更多数据并执行不必要的工作。</span><span class="sxs-lookup"><span data-stu-id="bdf25-117">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="bdf25-118">如果有多个实体符合筛选部分，`SingleOrDefaultAsync` 将引发异常。</span><span class="sxs-lookup"><span data-stu-id="bdf25-118">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="bdf25-119">如果有多个实体符合筛选部分，`FirstOrDefaultAsync` 不引发异常。</span><span class="sxs-lookup"><span data-stu-id="bdf25-119">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<span data-ttu-id="bdf25-120">将 `SingleOrDefaultAsync` 全局替换为 `FirstOrDefaultAsync`。</span><span class="sxs-lookup"><span data-stu-id="bdf25-120">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="bdf25-121">`SingleOrDefaultAsync` 用于 5 个位置：</span><span class="sxs-lookup"><span data-stu-id="bdf25-121">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="bdf25-122">“详细信息”页面中的 `OnGetAsync`。</span><span class="sxs-lookup"><span data-stu-id="bdf25-122">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="bdf25-123">“编辑”和“删除”页面中的 `OnGetAsync` 和 `OnPostAsync`。</span><span class="sxs-lookup"><span data-stu-id="bdf25-123">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="bdf25-124">FindAsync</span><span class="sxs-lookup"><span data-stu-id="bdf25-124">FindAsync</span></span>

<span data-ttu-id="bdf25-125">在大部分基架代码中，[FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) 可用于替代 `FirstOrDefaultAsync` 或 `SingleOrDefaultAsync`。</span><span class="sxs-lookup"><span data-stu-id="bdf25-125">In much of the scaffolded code, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="bdf25-126">`FindAsync`：</span><span class="sxs-lookup"><span data-stu-id="bdf25-126">`FindAsync`:</span></span>

* <span data-ttu-id="bdf25-127">查找具有主键 (PK) 的实体。</span><span class="sxs-lookup"><span data-stu-id="bdf25-127">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="bdf25-128">如果具有 PK 的实体正在由上下文跟踪，会返回该实体且不向 DB 发出请求。</span><span class="sxs-lookup"><span data-stu-id="bdf25-128">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="bdf25-129">既简单又简洁。</span><span class="sxs-lookup"><span data-stu-id="bdf25-129">Is simple and concise.</span></span>
* <span data-ttu-id="bdf25-130">经过优化后可查找单个实体。</span><span class="sxs-lookup"><span data-stu-id="bdf25-130">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="bdf25-131">在某些情况下可提供性能优势，但它们很少用于普通的 Web 方案。</span><span class="sxs-lookup"><span data-stu-id="bdf25-131">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="bdf25-132">以隐式方式使用 [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) 而不是 [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)。</span><span class="sxs-lookup"><span data-stu-id="bdf25-132">Implicitly uses [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="bdf25-133">如果想要“包含”其他实体，则“查找”将不再适用。</span><span class="sxs-lookup"><span data-stu-id="bdf25-133">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="bdf25-134">这意味着可能需要放弃“查找”并随着应用运行移动到查询。</span><span class="sxs-lookup"><span data-stu-id="bdf25-134">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="bdf25-135">自定义“详细信息”页</span><span class="sxs-lookup"><span data-stu-id="bdf25-135">Customize the Details page</span></span>

<span data-ttu-id="bdf25-136">浏览到 `Pages/Students` 页面。</span><span class="sxs-lookup"><span data-stu-id="bdf25-136">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="bdf25-137">“编辑”、“详细信息”和“删除”链接是在 Pages/Students/Index.cshtml 文件中由[定位点标记帮助器](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)生成的。</span><span class="sxs-lookup"><span data-stu-id="bdf25-137">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="bdf25-138">选择“详细信息”链接。</span><span class="sxs-lookup"><span data-stu-id="bdf25-138">Select a Details link.</span></span> <span data-ttu-id="bdf25-139">URL 的格式为 `http://localhost:5000/Students/Details?id=2`。</span><span class="sxs-lookup"><span data-stu-id="bdf25-139">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="bdf25-140">“学生 ID”通过查询字符串 (`?id=2`) 进行传递。</span><span class="sxs-lookup"><span data-stu-id="bdf25-140">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="bdf25-141">更新“编辑”、“详细信息”和“删除”Razor 页面以使用 `"{id:int}"` 路由模板。</span><span class="sxs-lookup"><span data-stu-id="bdf25-141">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="bdf25-142">将上述每个页面的页面指令从 `@page` 更改为 `@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="bdf25-142">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="bdf25-143">如果对具有不包含整数路由值的“{id:int}”路由模板的页面发起请求，则该请求将返回 HTTP 404（找不到）错误。</span><span class="sxs-lookup"><span data-stu-id="bdf25-143">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="bdf25-144">例如，`http://localhost:5000/Students/Details` 返回 404 错误。</span><span class="sxs-lookup"><span data-stu-id="bdf25-144">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="bdf25-145">若要使 ID 可选，请将 `?` 追加到路由约束：</span><span class="sxs-lookup"><span data-stu-id="bdf25-145">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="bdf25-146">运行应用，单击“详细信息”链接，并验证确认 URL 正在将 ID 作为路由数据 (`http://localhost:5000/Students/Details/2`) 进行传递。</span><span class="sxs-lookup"><span data-stu-id="bdf25-146">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="bdf25-147">不要将 `@page` 全局更改为 `@page "{id:int}"`，执行此操作会将链接拆分为“主页”和“创建”页。</span><span class="sxs-lookup"><span data-stu-id="bdf25-147">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="bdf25-148">添加相关数据</span><span class="sxs-lookup"><span data-stu-id="bdf25-148">Add related data</span></span>

<span data-ttu-id="bdf25-149">“学生索引”页的基架代码不包括 `Enrollments` 属性。</span><span class="sxs-lookup"><span data-stu-id="bdf25-149">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="bdf25-150">在本部分，`Enrollments` 集合的内容显示在“详细信息”页中。</span><span class="sxs-lookup"><span data-stu-id="bdf25-150">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="bdf25-151">Pages/Students/Details.cshtml.cs 的 `OnGetAsync` 方法使用 `FirstOrDefaultAsync` 方法检索单个 `Student` 实体。</span><span class="sxs-lookup"><span data-stu-id="bdf25-151">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="bdf25-152">添加以下突出显示的代码：</span><span class="sxs-lookup"><span data-stu-id="bdf25-152">Add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="bdf25-153">`Include` 和 `ThenInclude` 方法使上下文加载 `Student.Enrollments` 导航属性，并在每个注册中加载 `Enrollment.Course` 导航属性。</span><span class="sxs-lookup"><span data-stu-id="bdf25-153">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="bdf25-154">这些方法将在与数据读取相关的教程中进行详细介绍。</span><span class="sxs-lookup"><span data-stu-id="bdf25-154">These methods are examinied in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="bdf25-155">对于返回的实体未在当前上下文中更新的情况，`AsNoTracking` 方法将会提升性能。</span><span class="sxs-lookup"><span data-stu-id="bdf25-155">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="bdf25-156">`AsNoTracking` 将在本教程的后续部分中讨论。</span><span class="sxs-lookup"><span data-stu-id="bdf25-156">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="bdf25-157">在“详细信息”页中显示相关注册</span><span class="sxs-lookup"><span data-stu-id="bdf25-157">Display related enrollments on the Details page</span></span>

<span data-ttu-id="bdf25-158">打开 Pages/Students/Details.cshtml。</span><span class="sxs-lookup"><span data-stu-id="bdf25-158">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="bdf25-159">添加以下突出显示的代码以显示注册列表：</span><span class="sxs-lookup"><span data-stu-id="bdf25-159">Add the following highlighted code to display a list of enrollments:</span></span>

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

<span data-ttu-id="bdf25-160">如果代码缩进在粘贴代码后出现错误，请按 CTRL-K-D 进行更正。</span><span class="sxs-lookup"><span data-stu-id="bdf25-160">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="bdf25-161">上面的代码循环通过 `Enrollments` 导航属性中的实体。</span><span class="sxs-lookup"><span data-stu-id="bdf25-161">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="bdf25-162">它将针对每个注册显示课程标题和成绩。</span><span class="sxs-lookup"><span data-stu-id="bdf25-162">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="bdf25-163">课程标题从 Course 实体中检索，该实体存储在 Enrollments 实体的 `Course` 导航属性中。</span><span class="sxs-lookup"><span data-stu-id="bdf25-163">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="bdf25-164">运行应用，选择“学生”选项卡，然后单击学生的“详细信息”链接。</span><span class="sxs-lookup"><span data-stu-id="bdf25-164">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="bdf25-165">随即显示出所选学生的课程和成绩列表。</span><span class="sxs-lookup"><span data-stu-id="bdf25-165">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="bdf25-166">更新“创建”页</span><span class="sxs-lookup"><span data-stu-id="bdf25-166">Update the Create page</span></span>

<span data-ttu-id="bdf25-167">将 Pages/Students/Create.cshtml.cs 中的 `OnPostAsync` 方法更新为以下代码：</span><span class="sxs-lookup"><span data-stu-id="bdf25-167">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="bdf25-168">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="bdf25-168">TryUpdateModelAsync</span></span>

<span data-ttu-id="bdf25-169">检查 [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) 代码：</span><span class="sxs-lookup"><span data-stu-id="bdf25-169">Examine the [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="bdf25-170">在前面的代码中，`TryUpdateModelAsync<Student>` 尝试使用 [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0) 的 [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) 属性中已发布的表单值更新 `emptyStudent` 对象。</span><span class="sxs-lookup"><span data-stu-id="bdf25-170">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="bdf25-171">`TryUpdateModelAsync` 仅更新列出的属性 (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`)。</span><span class="sxs-lookup"><span data-stu-id="bdf25-171">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="bdf25-172">在上述示例中：</span><span class="sxs-lookup"><span data-stu-id="bdf25-172">In the preceding sample:</span></span>

* <span data-ttu-id="bdf25-173">第二个自变量 (` "student", // Prefix`) 是用于查找值的前缀。</span><span class="sxs-lookup"><span data-stu-id="bdf25-173">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="bdf25-174">该自变量不区分大小写。</span><span class="sxs-lookup"><span data-stu-id="bdf25-174">It's not case sensitive.</span></span>
* <span data-ttu-id="bdf25-175">已发布的表单值通过[模型绑定](xref:mvc/models/model-binding#how-model-binding-works)转换为 `Student` 模型中的类型。</span><span class="sxs-lookup"><span data-stu-id="bdf25-175">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="bdf25-176">过多发布</span><span class="sxs-lookup"><span data-stu-id="bdf25-176">Overposting</span></span>

<span data-ttu-id="bdf25-177">使用 `TryUpdateModel` 更新具有已发布值的字段是一种最佳的安全做法，因为这能阻止过多发布。</span><span class="sxs-lookup"><span data-stu-id="bdf25-177">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="bdf25-178">例如，假设 Student 实体包含此网页不应更新或添加的 `Secret` 属性：</span><span class="sxs-lookup"><span data-stu-id="bdf25-178">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="bdf25-179">即使应用的创建/更新 Razor 页面上没有 `Secret` 字段，黑客仍可利用过多发布设置 `Secret` 值。</span><span class="sxs-lookup"><span data-stu-id="bdf25-179">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="bdf25-180">黑客也可使用 Fiddler 等工具或通过编写某个 JavaScript 来发布 `Secret` 表单值。</span><span class="sxs-lookup"><span data-stu-id="bdf25-180">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="bdf25-181">原始代码不会限制模型绑定器在创建“学生”实例时使用的字段。</span><span class="sxs-lookup"><span data-stu-id="bdf25-181">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="bdf25-182">黑客为 `Secret` 表单字段指定的任何值都会在 DB 中更新。</span><span class="sxs-lookup"><span data-stu-id="bdf25-182">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="bdf25-183">下图显示 Fiddler 工具正在将 `Secret` 字段（值为“OverPost”）添加到已发布的表单值。</span><span class="sxs-lookup"><span data-stu-id="bdf25-183">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Fiddler 添加 Secret 字段](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="bdf25-185">值“OverPost”已成功添加到所插入行的 `Secret` 属性中。</span><span class="sxs-lookup"><span data-stu-id="bdf25-185">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="bdf25-186">应用程序设计器绝不会在“创建”页设置 `Secret` 属性。</span><span class="sxs-lookup"><span data-stu-id="bdf25-186">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="bdf25-187">视图模型</span><span class="sxs-lookup"><span data-stu-id="bdf25-187">View model</span></span>

<span data-ttu-id="bdf25-188">视图模型通常包含应用程序所用的模型中包括的属性的子集。</span><span class="sxs-lookup"><span data-stu-id="bdf25-188">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="bdf25-189">应用程序模型通常称为域模型。</span><span class="sxs-lookup"><span data-stu-id="bdf25-189">The application model is often called the domain model.</span></span> <span data-ttu-id="bdf25-190">域模型通常包含 DB 中对应实体所需的全部属性。</span><span class="sxs-lookup"><span data-stu-id="bdf25-190">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="bdf25-191">视图模型仅包含 UI 层（例如“创建”页）所需的属性。</span><span class="sxs-lookup"><span data-stu-id="bdf25-191">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="bdf25-192">除视图模型外，某些应用使用绑定模型或输入模型在“Razor 页面”页面模型类和浏览器之间传递数据。</span><span class="sxs-lookup"><span data-stu-id="bdf25-192">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="bdf25-193">请考虑以下 `Student` 视图模型：</span><span class="sxs-lookup"><span data-stu-id="bdf25-193">Consider the following `Student` view model:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="bdf25-194">视图模型还提供了一种防止过度发布的方法。</span><span class="sxs-lookup"><span data-stu-id="bdf25-194">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="bdf25-195">视图模型仅包含要查看（显示）或更新的属性。</span><span class="sxs-lookup"><span data-stu-id="bdf25-195">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="bdf25-196">以下代码使用 `StudentVM` 视图模型创建新的学生：</span><span class="sxs-lookup"><span data-stu-id="bdf25-196">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="bdf25-197">[SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) 方法通过从另一个 [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) 对象读取值来设置此对象的值。</span><span class="sxs-lookup"><span data-stu-id="bdf25-197">The [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="bdf25-198">`SetValues` 使用属性名称匹配。</span><span class="sxs-lookup"><span data-stu-id="bdf25-198">`SetValues` uses property name matching.</span></span> <span data-ttu-id="bdf25-199">视图模型类型不需要与模型类型相关，它只需要具有匹配的属性。</span><span class="sxs-lookup"><span data-stu-id="bdf25-199">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="bdf25-200">使用 `StudentVM` 时需要更新 [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) 才能使用 `StudentVM` 而非 `Student`。</span><span class="sxs-lookup"><span data-stu-id="bdf25-200">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="bdf25-201">在 Razor 页面，`PageModel` 派生类就是视图模型。</span><span class="sxs-lookup"><span data-stu-id="bdf25-201">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="bdf25-202">更新“编辑”页</span><span class="sxs-lookup"><span data-stu-id="bdf25-202">Update the Edit page</span></span>

<span data-ttu-id="bdf25-203">更新“编辑”页的页面模型：</span><span class="sxs-lookup"><span data-stu-id="bdf25-203">Update the page model for the Edit page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="bdf25-204">代码更改与“创建”页类似，但有少数例外：</span><span class="sxs-lookup"><span data-stu-id="bdf25-204">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="bdf25-205">`OnPostAsync` 具有可选的 `id` 参数。</span><span class="sxs-lookup"><span data-stu-id="bdf25-205">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="bdf25-206">当前学生是从 DB 提取的，而非通过创建空学生获得。</span><span class="sxs-lookup"><span data-stu-id="bdf25-206">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="bdf25-207">已将 `FirstOrDefaultAsync` 替换为 [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="bdf25-207">`FirstOrDefaultAsync` has been replaced with [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="bdf25-208">从主键中选择实体时，使用 `FindAsync` 是一个不错的选择。</span><span class="sxs-lookup"><span data-stu-id="bdf25-208">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="bdf25-209">请参阅 [FindAsync](#FindAsync) 了解详细信息。</span><span class="sxs-lookup"><span data-stu-id="bdf25-209">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="bdf25-210">测试“编辑”和“创建”页</span><span class="sxs-lookup"><span data-stu-id="bdf25-210">Test the Edit and Create pages</span></span>

<span data-ttu-id="bdf25-211">创建和编辑几个学生实体。</span><span class="sxs-lookup"><span data-stu-id="bdf25-211">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="bdf25-212">实体状态</span><span class="sxs-lookup"><span data-stu-id="bdf25-212">Entity States</span></span>

<span data-ttu-id="bdf25-213">DB 上下文会随时跟踪内存中的实体是否已与其在 DB 中的对应行进行同步。</span><span class="sxs-lookup"><span data-stu-id="bdf25-213">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="bdf25-214">DB 上下文同步信息可决定调用 `SaveChanges` 后的行为。</span><span class="sxs-lookup"><span data-stu-id="bdf25-214">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="bdf25-215">例如，将新实体传递到 `Add` 方法时，该实体的状态设置为 `Added`。</span><span class="sxs-lookup"><span data-stu-id="bdf25-215">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="bdf25-216">调用 `SaveChanges` 时，DB 上下文会发出 SQL INSERT 命令。</span><span class="sxs-lookup"><span data-stu-id="bdf25-216">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="bdf25-217">实体可能处于以下状态之一：</span><span class="sxs-lookup"><span data-stu-id="bdf25-217">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="bdf25-218">`Added`：DB 中尚不存在实体。</span><span class="sxs-lookup"><span data-stu-id="bdf25-218">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="bdf25-219">`SaveChanges` 方法发出 INSERT 语句。</span><span class="sxs-lookup"><span data-stu-id="bdf25-219">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="bdf25-220">`Unchanged`：无需保存对该实体所作的任何更改。</span><span class="sxs-lookup"><span data-stu-id="bdf25-220">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="bdf25-221">从 DB 中读取实体时，该实体将具有此状态。</span><span class="sxs-lookup"><span data-stu-id="bdf25-221">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="bdf25-222">`Modified`：已修改实体的部分或全部属性值。</span><span class="sxs-lookup"><span data-stu-id="bdf25-222">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="bdf25-223">`SaveChanges` 方法发出 UPDATE 语句。</span><span class="sxs-lookup"><span data-stu-id="bdf25-223">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="bdf25-224">`Deleted`：已标记该实体进行删除。</span><span class="sxs-lookup"><span data-stu-id="bdf25-224">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="bdf25-225">`SaveChanges` 方法发出 DELETE 语句。</span><span class="sxs-lookup"><span data-stu-id="bdf25-225">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="bdf25-226">`Detached`：DB 上下文未跟踪该实体。</span><span class="sxs-lookup"><span data-stu-id="bdf25-226">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="bdf25-227">在桌面应用中，通常会自动设置状态更改。</span><span class="sxs-lookup"><span data-stu-id="bdf25-227">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="bdf25-228">读取实体并执行更改后，实体状态自动更改为 `Modified`。</span><span class="sxs-lookup"><span data-stu-id="bdf25-228">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="bdf25-229">调用 `SaveChanges` 会生成仅更新已更改属性的 SQL UPDATE 语句。</span><span class="sxs-lookup"><span data-stu-id="bdf25-229">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="bdf25-230">在 Web 应用中，读取实体并显示数据的 `DbContext` 将在页面呈现后进行处理。</span><span class="sxs-lookup"><span data-stu-id="bdf25-230">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="bdf25-231">调用页面 `OnPostAsync` 方法时，将发出具有 `DbContext` 的新实例的 Web 请求。</span><span class="sxs-lookup"><span data-stu-id="bdf25-231">When a pages `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="bdf25-232">如果在这个新的上下文中重新读取实体，则会模拟桌面处理。</span><span class="sxs-lookup"><span data-stu-id="bdf25-232">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="bdf25-233">更新“删除”页</span><span class="sxs-lookup"><span data-stu-id="bdf25-233">Update the Delete page</span></span>

<span data-ttu-id="bdf25-234">在此部分中，当对 `SaveChanges` 的调用失败时，将添加用于实现自定义错误消息的代码。</span><span class="sxs-lookup"><span data-stu-id="bdf25-234">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="bdf25-235">添加字符串，使其包含可能的错误消息：</span><span class="sxs-lookup"><span data-stu-id="bdf25-235">Add a string to contain possible error messages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="bdf25-236">将 `OnGetAsync` 方法替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="bdf25-236">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="bdf25-237">上述代码包含可选参数 `saveChangesError`。</span><span class="sxs-lookup"><span data-stu-id="bdf25-237">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="bdf25-238">`saveChangesError` 指示学生对象删除失败后是否调用该方法。</span><span class="sxs-lookup"><span data-stu-id="bdf25-238">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="bdf25-239">删除操作可能由于暂时性网络问题而失败。</span><span class="sxs-lookup"><span data-stu-id="bdf25-239">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="bdf25-240">云端更可能出现暂时性网络错误。</span><span class="sxs-lookup"><span data-stu-id="bdf25-240">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="bdf25-241">通过 UI 调用“删除”页 `OnGetAsync` 时，`saveChangesError` 为 false。</span><span class="sxs-lookup"><span data-stu-id="bdf25-241">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="bdf25-242">当 `OnPostAsync` 调用 `OnGetAsync`（由于删除操作失败）时，`saveChangesError` 参数为 true。</span><span class="sxs-lookup"><span data-stu-id="bdf25-242">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="bdf25-243">“删除”页 OnPostAsync 方法</span><span class="sxs-lookup"><span data-stu-id="bdf25-243">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="bdf25-244">将 `OnPostAsync` 替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="bdf25-244">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="bdf25-245">上述代码检索所选的实体，然后调用 `Remove` 方法，将实体的状态设置为 `Deleted`。</span><span class="sxs-lookup"><span data-stu-id="bdf25-245">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="bdf25-246">调用 `SaveChanges` 时生成 SQL DELETE 命令。</span><span class="sxs-lookup"><span data-stu-id="bdf25-246">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="bdf25-247">如果 `Remove` 失败：</span><span class="sxs-lookup"><span data-stu-id="bdf25-247">If `Remove` fails:</span></span>

* <span data-ttu-id="bdf25-248">会捕获 DB 异常。</span><span class="sxs-lookup"><span data-stu-id="bdf25-248">The DB exception is caught.</span></span>
* <span data-ttu-id="bdf25-249">通过 `saveChangesError=true` 调用“删除”页 `OnGetAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="bdf25-249">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="bdf25-250">更新“删除”Razor 页面</span><span class="sxs-lookup"><span data-stu-id="bdf25-250">Update the Delete Razor Page</span></span>

<span data-ttu-id="bdf25-251">将以下突出显示的错误消息添加到“删除”Razor 页面。</span><span class="sxs-lookup"><span data-stu-id="bdf25-251">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="bdf25-252">测试“删除”。</span><span class="sxs-lookup"><span data-stu-id="bdf25-252">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="bdf25-253">常见错误</span><span class="sxs-lookup"><span data-stu-id="bdf25-253">Common errors</span></span>

<span data-ttu-id="bdf25-254">“学生/主页”或其他链接不起作用：</span><span class="sxs-lookup"><span data-stu-id="bdf25-254">Student/Home or other links don't work:</span></span>

<span data-ttu-id="bdf25-255">验证确认 Razor 页面包含正确的 `@page` 指令。</span><span class="sxs-lookup"><span data-stu-id="bdf25-255">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="bdf25-256">例如，“学生/主页”Razor 页面不应包含路由模板：</span><span class="sxs-lookup"><span data-stu-id="bdf25-256">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="bdf25-257">每个 Razor 页面均必须包含 `@page` 指令。</span><span class="sxs-lookup"><span data-stu-id="bdf25-257">Each Razor Page must include the `@page` directive.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="bdf25-258">[上一页](xref:data/ef-rp/intro)
[下一页](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="bdf25-258">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
