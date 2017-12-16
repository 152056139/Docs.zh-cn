---
title: "与 EF 核心-CRUD-2 的 8 razor 页"
author: rick-anderson
description: "演示如何创建、 读取、 更新和删除与 EF 核心"
keywords: "ASP.NET 核心、 实体框架核心、 CRUD，创建、 读取、 更新、 删除"
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/crud
ms.openlocfilehash: 163bc35afed0bf1d9236935d5ce60e6975356594
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2017
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a><span data-ttu-id="286f7-104">创建、 读取、 更新和删除的 EF 内核，它们有 Razor 页 (2 的 8)</span><span class="sxs-lookup"><span data-stu-id="286f7-104">Create, Read, Update, and Delete - EF Core with Razor Pages (2 of 8)</span></span>

<span data-ttu-id="286f7-105">通过[Tom Dykstra](https://github.com/tdykstra)， [Jon P Smith](https://twitter.com/thereformedprog)，和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="286f7-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="286f7-106">在本教程中，基架 CRUD （创建、 读取、 更新、 删除） 查看和自定义代码。</span><span class="sxs-lookup"><span data-stu-id="286f7-106">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="286f7-107">注意： 若要最小化复杂性并保留侧重于 EF 核心这些教程，请 EF 核心使用代码在 Razor 页代码隐藏文件中。</span><span class="sxs-lookup"><span data-stu-id="286f7-107">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages code-behind files.</span></span> <span data-ttu-id="286f7-108">一些开发人员使用一种服务层或存储库模式中的创建 UI （Razor 页） 和数据访问层之间的抽象层。</span><span class="sxs-lookup"><span data-stu-id="286f7-108">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="286f7-109">在本教程、 创建、 编辑、 删除、 和中详细说明 Razor 页*学生*文件夹进行修改。</span><span class="sxs-lookup"><span data-stu-id="286f7-109">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="286f7-110">用于创建、 编辑和删除页面，该基架的代码使用以下模式：</span><span class="sxs-lookup"><span data-stu-id="286f7-110">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="286f7-111">获取并使用 HTTP GET 方法显示所请求的数据`OnGetAsync`。</span><span class="sxs-lookup"><span data-stu-id="286f7-111">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="286f7-112">使用 HTTP POST 方法将更改保存到数据`OnPostAsync`。</span><span class="sxs-lookup"><span data-stu-id="286f7-112">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="286f7-113">索引和详细信息页获取并使用 HTTP GET 方法显示所请求的数据`OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="286f7-113">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="286f7-114">将替换为 FirstOrDefaultAsync SingleOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="286f7-114">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="286f7-115">生成的代码使用[SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)提取请求的实体。</span><span class="sxs-lookup"><span data-stu-id="286f7-115">The generated code uses [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="286f7-116">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_)是，能够有效地提取一个实体：</span><span class="sxs-lookup"><span data-stu-id="286f7-116">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="286f7-117">除非代码需要进行验证，否则是不从查询返回的多个实体。</span><span class="sxs-lookup"><span data-stu-id="286f7-117">Unless the code needs to verify that there is not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="286f7-118">`SingleOrDefaultAsync`提取更多的数据，并执行不必要的工作。</span><span class="sxs-lookup"><span data-stu-id="286f7-118">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="286f7-119">`SingleOrDefaultAsync`如果没有适合的筛选器部分的多个实体，则引发异常。</span><span class="sxs-lookup"><span data-stu-id="286f7-119">`SingleOrDefaultAsync` throws an exception if there is more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="286f7-120">`FirstOrDefaultAsync`不会引发，如果没有适合的筛选器部分的多个实体。</span><span class="sxs-lookup"><span data-stu-id="286f7-120">`FirstOrDefaultAsync` doesn't throw if there is more than one entity that fits the filter part.</span></span>

<span data-ttu-id="286f7-121">全局替换`SingleOrDefaultAsync`与`FirstOrDefaultAsync`。</span><span class="sxs-lookup"><span data-stu-id="286f7-121">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="286f7-122">`SingleOrDefaultAsync`位置也使用 5:</span><span class="sxs-lookup"><span data-stu-id="286f7-122">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="286f7-123">`OnGetAsync`中的详细信息页中。</span><span class="sxs-lookup"><span data-stu-id="286f7-123">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="286f7-124">`OnGetAsync`和`OnPostAsync`在页中编辑和删除。</span><span class="sxs-lookup"><span data-stu-id="286f7-124">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="286f7-125">FindAsync</span><span class="sxs-lookup"><span data-stu-id="286f7-125">FindAsync</span></span>

<span data-ttu-id="286f7-126">在很大程度的基架代码[FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___)可代替了`FirstOrDefaultAsync`或`SingleOrDefaultAsync`。</span><span class="sxs-lookup"><span data-stu-id="286f7-126">In much of the scaffolded code, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="286f7-127">`FindAsync`：</span><span class="sxs-lookup"><span data-stu-id="286f7-127">`FindAsync`:</span></span>

* <span data-ttu-id="286f7-128">使用主键 (PK) 来查找实体。</span><span class="sxs-lookup"><span data-stu-id="286f7-128">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="286f7-129">如果具有在 PK 的实体正在由上下文跟踪时，它将发出的请求返回到的数据库。</span><span class="sxs-lookup"><span data-stu-id="286f7-129">If an entity with the PK is being tracked by the context, it is returned without a request to the DB.</span></span>
* <span data-ttu-id="286f7-130">是简单和简洁。</span><span class="sxs-lookup"><span data-stu-id="286f7-130">Is simple and concise.</span></span>
* <span data-ttu-id="286f7-131">经过优化，可查找单个实体。</span><span class="sxs-lookup"><span data-stu-id="286f7-131">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="286f7-132">可以在某些情况下，有性能的好处，但它们很少派上用场的正常 web 方案。</span><span class="sxs-lookup"><span data-stu-id="286f7-132">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="286f7-133">隐式使用[FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)而不是[SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)。</span><span class="sxs-lookup"><span data-stu-id="286f7-133">Implicitly uses [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="286f7-134">但如果你想要包括其他实体，然后查找不再适当。</span><span class="sxs-lookup"><span data-stu-id="286f7-134">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="286f7-135">这意味着，你可能需要以放弃查找并应用随着你转到查询。</span><span class="sxs-lookup"><span data-stu-id="286f7-135">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="286f7-136">自定义的详细信息页</span><span class="sxs-lookup"><span data-stu-id="286f7-136">Customize the Details page</span></span>

<span data-ttu-id="286f7-137">浏览到`Pages/Students`页。</span><span class="sxs-lookup"><span data-stu-id="286f7-137">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="286f7-138">**编辑**，**详细信息**，和**删除**链接都由[定位点标记帮助器](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)中*页/学生 /Index.cshtml*文件。</span><span class="sxs-lookup"><span data-stu-id="286f7-138">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="286f7-139">选择的详细信息链接。</span><span class="sxs-lookup"><span data-stu-id="286f7-139">Select a Details link.</span></span> <span data-ttu-id="286f7-140">URL 的格式为`http://localhost:5000/Students/Details?id=2`。</span><span class="sxs-lookup"><span data-stu-id="286f7-140">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="286f7-141">使用查询字符串传递学生 ID (`?id=2`)。</span><span class="sxs-lookup"><span data-stu-id="286f7-141">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="286f7-142">更新用于编辑、 详细信息，并删除 Razor 页面`"{id:int}"`路由模板。</span><span class="sxs-lookup"><span data-stu-id="286f7-142">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="286f7-143">将上述每个页面的页面指令从 `@page` 更改为 `@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="286f7-143">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="286f7-144">对"{id: int}"路由模板执行的页的请求**不**包括整数路由值返回 HTTP 404 （找不到） 错误。</span><span class="sxs-lookup"><span data-stu-id="286f7-144">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="286f7-145">例如，`http://localhost:5000/Students/Details`返回 404 错误。</span><span class="sxs-lookup"><span data-stu-id="286f7-145">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="286f7-146">若要使 ID 可选，请将 `?` 追加到路由约束：</span><span class="sxs-lookup"><span data-stu-id="286f7-146">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="286f7-147">运行应用程序，单击详细信息链接，并验证 URL 将 ID 传递作为路由数据 (`http://localhost:5000/Students/Details/2`)。</span><span class="sxs-lookup"><span data-stu-id="286f7-147">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="286f7-148">不要全局更改`@page`到`@page "{id:int}"`、 执行会中断主页的链接和创建页面。</span><span class="sxs-lookup"><span data-stu-id="286f7-148">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="286f7-149">添加相关的数据</span><span class="sxs-lookup"><span data-stu-id="286f7-149">Add related data</span></span>

<span data-ttu-id="286f7-150">学生索引页的基架的代码不包括`Enrollments`属性。</span><span class="sxs-lookup"><span data-stu-id="286f7-150">The scaffolded code for the Students Index page does not include the `Enrollments` property.</span></span> <span data-ttu-id="286f7-151">本节的内容中`Enrollments`集合显示在详细信息页。</span><span class="sxs-lookup"><span data-stu-id="286f7-151">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="286f7-152">`OnGetAsync`方法*Pages/Students/Details.cshtml.cs*使用`FirstOrDefaultAsync`方法来检索单个`Student`实体。</span><span class="sxs-lookup"><span data-stu-id="286f7-152">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="286f7-153">添加以下突出显示的代码：</span><span class="sxs-lookup"><span data-stu-id="286f7-153">Add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="286f7-154">`Include`和`ThenInclude`方法导致要加载的上下文`Student.Enrollments`导航属性，并在每个注册`Enrollment.Course`导航属性。</span><span class="sxs-lookup"><span data-stu-id="286f7-154">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="286f7-155">这些方法在读取相关数据教程中是 examinied 在详细信息。</span><span class="sxs-lookup"><span data-stu-id="286f7-155">These methods are examinied in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="286f7-156">`AsNoTracking`方法可以提高方案中的性能时不会更新在当前上下文中返回的实体。</span><span class="sxs-lookup"><span data-stu-id="286f7-156">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="286f7-157">`AsNoTracking`本教程中稍后将讨论。</span><span class="sxs-lookup"><span data-stu-id="286f7-157">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="286f7-158">详细信息页上显示相关的注册</span><span class="sxs-lookup"><span data-stu-id="286f7-158">Display related enrollments on the Details page</span></span>

<span data-ttu-id="286f7-159">打开*Pages/Students/Details.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="286f7-159">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="286f7-160">添加以下突出显示的代码，以显示注册的列表：</span><span class="sxs-lookup"><span data-stu-id="286f7-160">Add the following highlighted code to display a list of enrollments:</span></span>

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

<span data-ttu-id="286f7-161">如果后粘贴代码，代码缩进有误，，按 CTRL-K-D 更正它。</span><span class="sxs-lookup"><span data-stu-id="286f7-161">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="286f7-162">前面的代码循环访问中的实体`Enrollments`导航属性。</span><span class="sxs-lookup"><span data-stu-id="286f7-162">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="286f7-163">对于每个注册，它显示课程标题和评分。</span><span class="sxs-lookup"><span data-stu-id="286f7-163">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="286f7-164">正在从存储中的课程实体检索课程标题`Course`注册实体导航属性。</span><span class="sxs-lookup"><span data-stu-id="286f7-164">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="286f7-165">运行应用程序中，选择**学生**卡，然后单击**详细信息**一名学生的链接。</span><span class="sxs-lookup"><span data-stu-id="286f7-165">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="286f7-166">将显示 courses，并将所选学生的年级的列表。</span><span class="sxs-lookup"><span data-stu-id="286f7-166">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="286f7-167">更新创建页</span><span class="sxs-lookup"><span data-stu-id="286f7-167">Update the Create page</span></span>

<span data-ttu-id="286f7-168">更新`OnPostAsync`中的方法*Pages/Students/Create.cshtml.cs*替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="286f7-168">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="286f7-169">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="286f7-169">TryUpdateModelAsync</span></span>

<span data-ttu-id="286f7-170">检查[TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_)代码：</span><span class="sxs-lookup"><span data-stu-id="286f7-170">Examine the [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="286f7-171">在前面的代码中，`TryUpdateModelAsync<Student>`尝试更新`emptyStudent`对象使用的已发布的窗体值[PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext)中的属性[PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="286f7-171">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="286f7-172">`TryUpdateModelAsync`仅更新列出的属性 (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`)。</span><span class="sxs-lookup"><span data-stu-id="286f7-172">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="286f7-173">在前面的示例：</span><span class="sxs-lookup"><span data-stu-id="286f7-173">In the preceding sample:</span></span>

* <span data-ttu-id="286f7-174">第二个参数 (` "student", // Prefix`) 是前缀使用来查找值。</span><span class="sxs-lookup"><span data-stu-id="286f7-174">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="286f7-175">不区分大小写。</span><span class="sxs-lookup"><span data-stu-id="286f7-175">It's not case sensitive.</span></span>
* <span data-ttu-id="286f7-176">已发布的窗体值转换为中的类型`Student`模型使用[模型绑定](xref:mvc/models/model-binding#how-model-binding-works)。</span><span class="sxs-lookup"><span data-stu-id="286f7-176">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="286f7-177">过多发布</span><span class="sxs-lookup"><span data-stu-id="286f7-177">Overposting</span></span>

<span data-ttu-id="286f7-178">使用`TryUpdateModel`更新具有发送的值字段先是最佳安全方案，因为它阻止 overposting。</span><span class="sxs-lookup"><span data-stu-id="286f7-178">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="286f7-179">例如，假设学生实体包含`Secret`此 web 页面不应更新或添加的属性：</span><span class="sxs-lookup"><span data-stu-id="286f7-179">For example, suppose the Student entity includes a `Secret` property that this web page should not update or add:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="286f7-180">即使此应用程序没有`Secret`上创建/更新 Razor 页上，黑客字段可以设置`Secret`值过多发布。</span><span class="sxs-lookup"><span data-stu-id="286f7-180">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="286f7-181">黑客无法使用 Fiddler，之类的工具或编写一些 JavaScript，发布`Secret`形成的值。</span><span class="sxs-lookup"><span data-stu-id="286f7-181">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="286f7-182">原始代码不限制时它将创建学生实例模型联编程序使用的字段。</span><span class="sxs-lookup"><span data-stu-id="286f7-182">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="286f7-183">任何值为指定黑客`Secret`在数据库中更新窗体字段。</span><span class="sxs-lookup"><span data-stu-id="286f7-183">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="286f7-184">下图显示 Fiddler 工具添加`Secret`（具有值"OverPost"） 到已发布的窗体值的字段。</span><span class="sxs-lookup"><span data-stu-id="286f7-184">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Fiddler 添加机密字段](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="286f7-186">"OverPost"已成功添加到的值`Secret`所插入行的属性。</span><span class="sxs-lookup"><span data-stu-id="286f7-186">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="286f7-187">预期之外的应用程序设计器`Secret`属性与创建页设置。</span><span class="sxs-lookup"><span data-stu-id="286f7-187">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="286f7-188">视图模型</span><span class="sxs-lookup"><span data-stu-id="286f7-188">View model</span></span>

<span data-ttu-id="286f7-189">视图模型通常包含应用程序使用的模型中包括的属性的子集。</span><span class="sxs-lookup"><span data-stu-id="286f7-189">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="286f7-190">应用程序模型通常称为域模型。</span><span class="sxs-lookup"><span data-stu-id="286f7-190">The application model is often called the domain model.</span></span> <span data-ttu-id="286f7-191">域模型通常包含在数据库中的对应实体所需的所有属性。</span><span class="sxs-lookup"><span data-stu-id="286f7-191">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="286f7-192">视图模型仅包含属性所需的 UI 层 （例如，创建页）。</span><span class="sxs-lookup"><span data-stu-id="286f7-192">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="286f7-193">除视图模型中，某些应用程序，用于绑定模型或输入的模型 Razor 页代码隐藏类和浏览器之间传递数据。</span><span class="sxs-lookup"><span data-stu-id="286f7-193">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages code-behind class and the browser.</span></span> <span data-ttu-id="286f7-194">请考虑以下`Student`视图模型：</span><span class="sxs-lookup"><span data-stu-id="286f7-194">Consider the following `Student` view model:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="286f7-195">查看模型提供一种替代方式，以防止 overposting。</span><span class="sxs-lookup"><span data-stu-id="286f7-195">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="286f7-196">视图模型包含仅查看 （显示），或更新的属性。</span><span class="sxs-lookup"><span data-stu-id="286f7-196">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="286f7-197">下面的代码使用`StudentVM`视图模型来创建新的学生：</span><span class="sxs-lookup"><span data-stu-id="286f7-197">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="286f7-198">[SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_)方法通过从另一个读取值来设置此对象的值的[PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues)对象。</span><span class="sxs-lookup"><span data-stu-id="286f7-198">The [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="286f7-199">`SetValues`使用属性名称匹配。</span><span class="sxs-lookup"><span data-stu-id="286f7-199">`SetValues` uses property name matching.</span></span> <span data-ttu-id="286f7-200">视图模型类型不需要与模型类型相关，它只需要具有匹配的属性。</span><span class="sxs-lookup"><span data-stu-id="286f7-200">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="286f7-201">使用`StudentVM`需要[CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml)将更新为使用`StudentVM`而非`Student`。</span><span class="sxs-lookup"><span data-stu-id="286f7-201">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="286f7-202">在 Razor 页中，`PageModel`派生的类是视图模型。</span><span class="sxs-lookup"><span data-stu-id="286f7-202">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="286f7-203">更新编辑页</span><span class="sxs-lookup"><span data-stu-id="286f7-203">Update the Edit page</span></span>

<span data-ttu-id="286f7-204">更新编辑页代码隐藏文件：</span><span class="sxs-lookup"><span data-stu-id="286f7-204">Update the Edit page code-behind file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="286f7-205">代码更改是类似于创建页面，但有少数例外：</span><span class="sxs-lookup"><span data-stu-id="286f7-205">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="286f7-206">`OnPostAsync`具有可选`id`参数。</span><span class="sxs-lookup"><span data-stu-id="286f7-206">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="286f7-207">当前的学生提取从 DB 中，而不是创建空的学生。</span><span class="sxs-lookup"><span data-stu-id="286f7-207">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="286f7-208">`FirstOrDefaultAsync`已替换为[FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="286f7-208">`FirstOrDefaultAsync` has been replaced with [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="286f7-209">`FindAsync`选择从为主键的实体时，是一个不错的选择。</span><span class="sxs-lookup"><span data-stu-id="286f7-209">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="286f7-210">请参阅[FindAsync](#FindAsync)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="286f7-210">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="286f7-211">测试编辑和创建页</span><span class="sxs-lookup"><span data-stu-id="286f7-211">Test the Edit and Create pages</span></span>

<span data-ttu-id="286f7-212">创建和编辑几个学生实体。</span><span class="sxs-lookup"><span data-stu-id="286f7-212">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="286f7-213">实体状态</span><span class="sxs-lookup"><span data-stu-id="286f7-213">Entity States</span></span>

<span data-ttu-id="286f7-214">在内存中的实体是否与数据库中其相应行同步，将跟踪的数据库上下文。</span><span class="sxs-lookup"><span data-stu-id="286f7-214">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="286f7-215">DB 上下文同步信息确定会发生什么情况时`SaveChanges`调用。</span><span class="sxs-lookup"><span data-stu-id="286f7-215">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="286f7-216">例如，新实体传递到`Add`方法，该实体的状态设置为方法`Added`。</span><span class="sxs-lookup"><span data-stu-id="286f7-216">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="286f7-217">当`SaveChanges`调用时，数据库上下文发出 SQL INSERT 命令。</span><span class="sxs-lookup"><span data-stu-id="286f7-217">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="286f7-218">实体可能处于以下状态之一：</span><span class="sxs-lookup"><span data-stu-id="286f7-218">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="286f7-219">`Added`: 实体的 DB 中尚不存在。</span><span class="sxs-lookup"><span data-stu-id="286f7-219">`Added`: The entity does not yet exist in the DB.</span></span> <span data-ttu-id="286f7-220">`SaveChanges`方法发出 INSERT 语句。</span><span class="sxs-lookup"><span data-stu-id="286f7-220">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="286f7-221">`Unchanged`： 不需要此实体一起保存任何更改。</span><span class="sxs-lookup"><span data-stu-id="286f7-221">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="286f7-222">从数据库中读取时，实体具有此状态。</span><span class="sxs-lookup"><span data-stu-id="286f7-222">An entity has this status when it is read from the DB.</span></span>

* <span data-ttu-id="286f7-223">`Modified`： 某些或所有实体的属性值已都更改。</span><span class="sxs-lookup"><span data-stu-id="286f7-223">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="286f7-224">`SaveChanges`方法发出 UPDATE 语句。</span><span class="sxs-lookup"><span data-stu-id="286f7-224">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="286f7-225">`Deleted`: 实体已标记为删除。</span><span class="sxs-lookup"><span data-stu-id="286f7-225">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="286f7-226">`SaveChanges`方法发出的 DELETE 语句。</span><span class="sxs-lookup"><span data-stu-id="286f7-226">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="286f7-227">`Detached`: 实体不跟踪的数据库上下文。</span><span class="sxs-lookup"><span data-stu-id="286f7-227">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="286f7-228">在桌面应用中，通常会自动设置状态更改。</span><span class="sxs-lookup"><span data-stu-id="286f7-228">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="286f7-229">读取实体，进行更改，和要自动更改为的实体状态`Modified`。</span><span class="sxs-lookup"><span data-stu-id="286f7-229">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="286f7-230">调用`SaveChanges`生成 SQL UPDATE 语句，都会更新仅已更改的属性。</span><span class="sxs-lookup"><span data-stu-id="286f7-230">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="286f7-231">在 web 应用中，`DbContext`读取实体，并显示在呈现页面后释放数据。</span><span class="sxs-lookup"><span data-stu-id="286f7-231">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="286f7-232">当一页面`OnPostAsync`方法被调用时，新的 web 请求进行的新实例的与`DbContext`。</span><span class="sxs-lookup"><span data-stu-id="286f7-232">When a pages `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="286f7-233">重新读取该新的上下文中的实体模拟桌面处理。</span><span class="sxs-lookup"><span data-stu-id="286f7-233">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="286f7-234">更新删除页</span><span class="sxs-lookup"><span data-stu-id="286f7-234">Update the Delete page</span></span>

<span data-ttu-id="286f7-235">在此部分中，不会添加代码以实现自定义错误消息时对调用`SaveChanges`失败。</span><span class="sxs-lookup"><span data-stu-id="286f7-235">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="286f7-236">添加要包含 possile 错误消息的字符串：</span><span class="sxs-lookup"><span data-stu-id="286f7-236">Add a string to contain possile error messages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="286f7-237">将 `OnGetAsync` 方法替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="286f7-237">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="286f7-238">前面的代码中包含的可选参数`saveChangesError`。</span><span class="sxs-lookup"><span data-stu-id="286f7-238">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="286f7-239">`saveChangesError`指示方法是否在无法删除学生对象之后调用。</span><span class="sxs-lookup"><span data-stu-id="286f7-239">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="286f7-240">删除操作由于暂时性网络问题可能会失败。</span><span class="sxs-lookup"><span data-stu-id="286f7-240">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="286f7-241">在云中很有可能暂时性网络错误。</span><span class="sxs-lookup"><span data-stu-id="286f7-241">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="286f7-242">`saveChangesError`为 false 时删除页`OnGetAsync`从 UI 调用。</span><span class="sxs-lookup"><span data-stu-id="286f7-242">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="286f7-243">当`OnGetAsync`由调用`OnPostAsync`（因为删除操作失败），则`saveChangesError`参数为 true。</span><span class="sxs-lookup"><span data-stu-id="286f7-243">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="286f7-244">Delete 页 OnPostAsync 方法</span><span class="sxs-lookup"><span data-stu-id="286f7-244">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="286f7-245">替换`OnPostAsync`替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="286f7-245">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="286f7-246">前面的代码检索所选的实体，然后调用`Remove`方法将实体的状态设置为`Deleted`。</span><span class="sxs-lookup"><span data-stu-id="286f7-246">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="286f7-247">当`SaveChanges`称为 SQL DELETE 命令生成的。</span><span class="sxs-lookup"><span data-stu-id="286f7-247">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="286f7-248">如果`Remove`失败：</span><span class="sxs-lookup"><span data-stu-id="286f7-248">If `Remove` fails:</span></span>

* <span data-ttu-id="286f7-249">DB 异常捕获。</span><span class="sxs-lookup"><span data-stu-id="286f7-249">The DB exception is caught.</span></span>
* <span data-ttu-id="286f7-250">删除页`OnGetAsync`方法调用与`saveChangesError=true`。</span><span class="sxs-lookup"><span data-stu-id="286f7-250">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="286f7-251">更新删除 Razor 页</span><span class="sxs-lookup"><span data-stu-id="286f7-251">Update the Delete Razor Page</span></span>

<span data-ttu-id="286f7-252">将以下突出显示的错误消息添加到删除 Razor 页。</span><span class="sxs-lookup"><span data-stu-id="286f7-252">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="286f7-253">测试删除。</span><span class="sxs-lookup"><span data-stu-id="286f7-253">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="286f7-254">常见错误</span><span class="sxs-lookup"><span data-stu-id="286f7-254">Common errors</span></span>

<span data-ttu-id="286f7-255">学生/主页或其他链接不起作用：</span><span class="sxs-lookup"><span data-stu-id="286f7-255">Student/Home or other links don't work:</span></span>

<span data-ttu-id="286f7-256">验证是否 Razor 页包含正确`@page`指令。</span><span class="sxs-lookup"><span data-stu-id="286f7-256">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="286f7-257">例如，学生/Razor 主页应**不**包含路由模板：</span><span class="sxs-lookup"><span data-stu-id="286f7-257">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="286f7-258">每个 Razor 页必须包括`@page`指令。</span><span class="sxs-lookup"><span data-stu-id="286f7-258">Each Razor Page must include the `@page` directive.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="286f7-259">[上一页](xref:data/ef-rp/intro)
[下一页](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="286f7-259">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
