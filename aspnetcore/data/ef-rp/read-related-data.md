---
title: "与 EF 核心-razor 页读取相关的数据-8 6"
author: rick-anderson
description: "在本教程中你已阅读并显示相关的数据-即，实体框架将加载到导航属性的数据。"
keywords: "ASP.NET 核心，实体框架核心，相关数据，联接"
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: ba9b17ecdcb605d39117d03230b1db37e8e4d0dd
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2017
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="5d921-104">读取与相关的数据的 EF 内核，它们有 Razor 页 (6 的 8)</span><span class="sxs-lookup"><span data-stu-id="5d921-104">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="5d921-105">通过[Tom Dykstra](https://github.com/tdykstra)， [Jon P Smith](https://twitter.com/thereformedprog)，和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5d921-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="5d921-106">在本教程中，是读取和显示相关的数据。</span><span class="sxs-lookup"><span data-stu-id="5d921-106">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="5d921-107">相关的数据是 EF 核心将加载到导航属性的数据。</span><span class="sxs-lookup"><span data-stu-id="5d921-107">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="5d921-108">如果你遇到无法解决的问题，请下载[对此阶段已完成应用程序](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related)。</span><span class="sxs-lookup"><span data-stu-id="5d921-108">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="5d921-109">下图显示了本教程已完成的页：</span><span class="sxs-lookup"><span data-stu-id="5d921-109">The following illustrations show the completed pages for this tutorial:</span></span>

![课程索引页](read-related-data/_static/courses-index.png)

![教师索引页](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="5d921-112">Eager、 显式，和延迟加载的相关数据</span><span class="sxs-lookup"><span data-stu-id="5d921-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="5d921-113">有多种 EF 核心可以加载到实体的导航属性的相关的数据：</span><span class="sxs-lookup"><span data-stu-id="5d921-113">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="5d921-114">[预先加载](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading)。</span><span class="sxs-lookup"><span data-stu-id="5d921-114">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="5d921-115">预先加载是实体的一种类型的查询还加载相关的实体时。</span><span class="sxs-lookup"><span data-stu-id="5d921-115">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="5d921-116">实体中读取时，会检索其相关的数据。</span><span class="sxs-lookup"><span data-stu-id="5d921-116">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="5d921-117">这通常会导致检索所有具有所需的数据的单一联接查询。</span><span class="sxs-lookup"><span data-stu-id="5d921-117">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="5d921-118">EF 核心将颁发某些类型的预先加载多个的查询。</span><span class="sxs-lookup"><span data-stu-id="5d921-118">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="5d921-119">发出多个查询可以是效率更高，比 ef6 更高版本中的某些查询这种情况其中没有单个查询。</span><span class="sxs-lookup"><span data-stu-id="5d921-119">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="5d921-120">使用指定预先加载`Include`和`ThenInclude`方法。</span><span class="sxs-lookup"><span data-stu-id="5d921-120">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![预先加载示例](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="5d921-122">包含集合 nvavigation 时，预先加载将发送多个查询：</span><span class="sxs-lookup"><span data-stu-id="5d921-122">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="5d921-123">一个查询主查询</span><span class="sxs-lookup"><span data-stu-id="5d921-123">One query for the main query</span></span> 
 * <span data-ttu-id="5d921-124">每个集合"边缘"负载树中的的一个查询。</span><span class="sxs-lookup"><span data-stu-id="5d921-124">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="5d921-125">单独的查询与`Load`： 可以在单独的查询中检索数据并 EF 核心"修复"的导航属性。</span><span class="sxs-lookup"><span data-stu-id="5d921-125">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="5d921-126">"向上修复"意味着 EF 核心自动填充的导航属性。</span><span class="sxs-lookup"><span data-stu-id="5d921-126">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="5d921-127">单独的查询与`Load`很像 explict 加载比预先加载。</span><span class="sxs-lookup"><span data-stu-id="5d921-127">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![单独的查询示例](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="5d921-129">注意： EF 核心与以前已加载到上下文实例的任何其他实体的导航属性将自动修复。</span><span class="sxs-lookup"><span data-stu-id="5d921-129">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="5d921-130">即使一个导航属性的数据已*不*显式包含，可能仍填充属性，如果某些或所有相关实体先前已加载。</span><span class="sxs-lookup"><span data-stu-id="5d921-130">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="5d921-131">[显式加载](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading)。</span><span class="sxs-lookup"><span data-stu-id="5d921-131">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="5d921-132">当第一次读取实体时，不检索相关的数据。</span><span class="sxs-lookup"><span data-stu-id="5d921-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="5d921-133">必须编写代码，当需要时检索相关的数据。</span><span class="sxs-lookup"><span data-stu-id="5d921-133">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="5d921-134">显式加载与单独的查询会导致发送到的数据库的多个查询。</span><span class="sxs-lookup"><span data-stu-id="5d921-134">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="5d921-135">显式加载，该代码指定要加载的导航属性。</span><span class="sxs-lookup"><span data-stu-id="5d921-135">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="5d921-136">使用`Load`方法执行显式加载。</span><span class="sxs-lookup"><span data-stu-id="5d921-136">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="5d921-137">例如: </span><span class="sxs-lookup"><span data-stu-id="5d921-137">For example:</span></span>

 ![显式加载示例](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="5d921-139">[延迟加载](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading)。</span><span class="sxs-lookup"><span data-stu-id="5d921-139">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="5d921-140">[EF 核心当前不支持延迟加载](https://github.com/aspnet/EntityFrameworkCore/issues/3797)。</span><span class="sxs-lookup"><span data-stu-id="5d921-140">[EF Core does not currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="5d921-141">当第一次读取实体时，不检索相关的数据。</span><span class="sxs-lookup"><span data-stu-id="5d921-141">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="5d921-142">第一次访问导航属性时，将自动检索该导航属性所需的数据。</span><span class="sxs-lookup"><span data-stu-id="5d921-142">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="5d921-143">查询发送到每个时间导航属性访问第一次数据库。</span><span class="sxs-lookup"><span data-stu-id="5d921-143">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="5d921-144">`Select`运算符加载仅所需的相关的数据。</span><span class="sxs-lookup"><span data-stu-id="5d921-144">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="5d921-145">创建显示部门名称的课程页</span><span class="sxs-lookup"><span data-stu-id="5d921-145">Create a Courses page that displays department name</span></span>

<span data-ttu-id="5d921-146">课程实体包括一个导航属性，包含`Department`实体。</span><span class="sxs-lookup"><span data-stu-id="5d921-146">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="5d921-147">`Department`实体包含过程分配给的部门。</span><span class="sxs-lookup"><span data-stu-id="5d921-147">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="5d921-148">若要在课程的列表中显示分配的部门的名称：</span><span class="sxs-lookup"><span data-stu-id="5d921-148">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="5d921-149">获取`Name`属性从`Department`实体。</span><span class="sxs-lookup"><span data-stu-id="5d921-149">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="5d921-150">`Department`实体来自于`Course.Department`导航属性。</span><span class="sxs-lookup"><span data-stu-id="5d921-150">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![课程。部门](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="5d921-152">基架过程模型</span><span class="sxs-lookup"><span data-stu-id="5d921-152">Scaffold the Course model</span></span>

* <span data-ttu-id="5d921-153">退出 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="5d921-153">Exit Visual Studio.</span></span>
* <span data-ttu-id="5d921-154">打开项目目录（包含 Program.cs、Startup.cs 和 .csproj 文件的目录）中的命令窗口。</span><span class="sxs-lookup"><span data-stu-id="5d921-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="5d921-155">运行下面的命令：</span><span class="sxs-lookup"><span data-stu-id="5d921-155">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="5d921-156">前面的命令基架`Course`模型。</span><span class="sxs-lookup"><span data-stu-id="5d921-156">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="5d921-157">在 Visual Studio 中打开项目。</span><span class="sxs-lookup"><span data-stu-id="5d921-157">Open the project in Visual Studio.</span></span>

<span data-ttu-id="5d921-158">生成项目。</span><span class="sxs-lookup"><span data-stu-id="5d921-158">Build the project.</span></span> <span data-ttu-id="5d921-159">生成将生成如下所示的错误：</span><span class="sxs-lookup"><span data-stu-id="5d921-159">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="5d921-160">全局更改`_context.Course`到`_context.Courses`(即，将"s"添加到`Course`)。</span><span class="sxs-lookup"><span data-stu-id="5d921-160">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="5d921-161">7 匹配项的发现和更新。</span><span class="sxs-lookup"><span data-stu-id="5d921-161">7 occurrences are found and updated.</span></span>

<span data-ttu-id="5d921-162">打开*Pages/Courses/Index.cshtml.cs*并检查`OnGetAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="5d921-162">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="5d921-163">基架引擎指定为预先加载`Department`导航属性。</span><span class="sxs-lookup"><span data-stu-id="5d921-163">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="5d921-164">`Include`方法指定预先加载。</span><span class="sxs-lookup"><span data-stu-id="5d921-164">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="5d921-165">运行应用并选择**课程**链接。</span><span class="sxs-lookup"><span data-stu-id="5d921-165">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="5d921-166">部门列显示`DepartmentID`，这是毫无意义。</span><span class="sxs-lookup"><span data-stu-id="5d921-166">The department column displays the `DepartmentID`, which is not useful.</span></span>

<span data-ttu-id="5d921-167">使用以下代码更新 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="5d921-167">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="5d921-168">前面的代码将添加`AsNoTracking`。</span><span class="sxs-lookup"><span data-stu-id="5d921-168">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="5d921-169">`AsNoTracking`因为返回的实体不会跟踪，可以提高性能。</span><span class="sxs-lookup"><span data-stu-id="5d921-169">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="5d921-170">因为它们不会更新在当前上下文中，不会跟踪实体。</span><span class="sxs-lookup"><span data-stu-id="5d921-170">The entities are not tracked because they are not updated in the current context.</span></span>

<span data-ttu-id="5d921-171">更新*Views/Courses/Index.cshtml*替换为以下突出显示的标记：</span><span class="sxs-lookup"><span data-stu-id="5d921-171">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="5d921-172">基架的代码进行了以下更改：</span><span class="sxs-lookup"><span data-stu-id="5d921-172">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="5d921-173">从索引的标题更改为课程。</span><span class="sxs-lookup"><span data-stu-id="5d921-173">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="5d921-174">添加**数**显示的列`CourseID`属性值。</span><span class="sxs-lookup"><span data-stu-id="5d921-174">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="5d921-175">默认情况下，主键不基架，因为它们通常是向最终用户无意义。</span><span class="sxs-lookup"><span data-stu-id="5d921-175">By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="5d921-176">但是，在这种情况下的主键是有意义。</span><span class="sxs-lookup"><span data-stu-id="5d921-176">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="5d921-177">更改**部门**列以显示部门名称。</span><span class="sxs-lookup"><span data-stu-id="5d921-177">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="5d921-178">该代码将显示`Name`属性`Department`实体加载到`Department`导航属性：</span><span class="sxs-lookup"><span data-stu-id="5d921-178">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="5d921-179">运行应用并选择**课程**选项卡以查看使用部门名称的列表。</span><span class="sxs-lookup"><span data-stu-id="5d921-179">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![课程索引页](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="5d921-181">正在加载相关与选择的数据</span><span class="sxs-lookup"><span data-stu-id="5d921-181">Loading related data with Select</span></span>

<span data-ttu-id="5d921-182">`OnGetAsync`方法将加载相关的数据与`Include`方法：</span><span class="sxs-lookup"><span data-stu-id="5d921-182">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="5d921-183">`Select`运算符加载仅所需的相关的数据。</span><span class="sxs-lookup"><span data-stu-id="5d921-183">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="5d921-184">为单个项目，如`Department.Name`它使用 SQL INNER JOIN。</span><span class="sxs-lookup"><span data-stu-id="5d921-184">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="5d921-185">对于集合，它使用与另一个数据库访问，但也会更激烈。`Include`</span><span class="sxs-lookup"><span data-stu-id="5d921-185">For collections it uses another database access, but so does the .`Include`</span></span> <span data-ttu-id="5d921-186">集合的运算符。</span><span class="sxs-lookup"><span data-stu-id="5d921-186">operator on collections.</span></span>

<span data-ttu-id="5d921-187">下面的代码加载与相关的数据`Select`方法：</span><span class="sxs-lookup"><span data-stu-id="5d921-187">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="5d921-188">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="5d921-188">The `CourseViewModel`:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="5d921-189">请参阅[IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml)和[IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs)有关完整示例。</span><span class="sxs-lookup"><span data-stu-id="5d921-189">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="5d921-190">创建显示 Courses，并将注册一个教师页面</span><span class="sxs-lookup"><span data-stu-id="5d921-190">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="5d921-191">在此部分中，创建教师页面。</span><span class="sxs-lookup"><span data-stu-id="5d921-191">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="5d921-192">![教师索引页](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="5d921-192">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="5d921-193">此页读取，并通过以下方式显示相关的数据：</span><span class="sxs-lookup"><span data-stu-id="5d921-193">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="5d921-194">教师的列表显示来自的相关的数据`OfficeAssignment`实体 (上图中的 Office)。</span><span class="sxs-lookup"><span data-stu-id="5d921-194">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="5d921-195">`Instructor`和`OfficeAssignment`实体位于对零或一一个关系。</span><span class="sxs-lookup"><span data-stu-id="5d921-195">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="5d921-196">用于预先加载`OfficeAssignment`实体。</span><span class="sxs-lookup"><span data-stu-id="5d921-196">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="5d921-197">预先加载需要显示相关的数据时通常效率更高。</span><span class="sxs-lookup"><span data-stu-id="5d921-197">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="5d921-198">在这种情况下，将显示讲师办公室分配。</span><span class="sxs-lookup"><span data-stu-id="5d921-198">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="5d921-199">当用户选择教师 (上图中的 Harui) 相关`Course`显示实体。</span><span class="sxs-lookup"><span data-stu-id="5d921-199">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="5d921-200">`Instructor`和`Course`实体位于多对多关系。</span><span class="sxs-lookup"><span data-stu-id="5d921-200">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="5d921-201">预先加载的`Course`实体和其相关`Department`使用实体。</span><span class="sxs-lookup"><span data-stu-id="5d921-201">Eager loading for the `Course` entities and their related `Department` entities is used.</span></span> <span data-ttu-id="5d921-202">在这种情况下，因为需要仅为所选教师的课程，单独的查询可能会更有效。</span><span class="sxs-lookup"><span data-stu-id="5d921-202">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="5d921-203">此示例演示如何使用预先加载用于导航属性中的实体中的导航属性。</span><span class="sxs-lookup"><span data-stu-id="5d921-203">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="5d921-204">当用户选择课程 （上图中的化学） 与相关的数据从`Enrollments`显示实体。</span><span class="sxs-lookup"><span data-stu-id="5d921-204">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="5d921-205">在前面的图中，显示学生名称，并将评分。</span><span class="sxs-lookup"><span data-stu-id="5d921-205">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="5d921-206">`Course`和`Enrollment`实体位于一个对多关系。</span><span class="sxs-lookup"><span data-stu-id="5d921-206">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="5d921-207">创建教师索引视图的视图模型</span><span class="sxs-lookup"><span data-stu-id="5d921-207">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="5d921-208">教师页显示三个不同的表中的数据。</span><span class="sxs-lookup"><span data-stu-id="5d921-208">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="5d921-209">创建视图模型，包括表示三个表的三个实体。</span><span class="sxs-lookup"><span data-stu-id="5d921-209">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="5d921-210">在*SchoolViewModels*文件夹中，创建*InstructorIndexData.cs*替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="5d921-210">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="5d921-211">基架教师模型</span><span class="sxs-lookup"><span data-stu-id="5d921-211">Scaffold the Instructor model</span></span>

* <span data-ttu-id="5d921-212">退出 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="5d921-212">Exit Visual Studio.</span></span>
* <span data-ttu-id="5d921-213">打开项目目录（包含 Program.cs、Startup.cs 和 .csproj 文件的目录）中的命令窗口。</span><span class="sxs-lookup"><span data-stu-id="5d921-213">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="5d921-214">运行下面的命令：</span><span class="sxs-lookup"><span data-stu-id="5d921-214">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="5d921-215">前面的命令基架`Instructor`模型。</span><span class="sxs-lookup"><span data-stu-id="5d921-215">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="5d921-216">在 Visual Studio 中打开项目。</span><span class="sxs-lookup"><span data-stu-id="5d921-216">Open the project in Visual Studio.</span></span>

<span data-ttu-id="5d921-217">生成项目。</span><span class="sxs-lookup"><span data-stu-id="5d921-217">Build the project.</span></span> <span data-ttu-id="5d921-218">生成生成错误。</span><span class="sxs-lookup"><span data-stu-id="5d921-218">The build generates errors.</span></span>

<span data-ttu-id="5d921-219">全局更改`_context.Instructor`到`_context.Instructors`(即，将"s"添加到`Instructor`)。</span><span class="sxs-lookup"><span data-stu-id="5d921-219">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="5d921-220">7 匹配项的发现和更新。</span><span class="sxs-lookup"><span data-stu-id="5d921-220">7 occurrences are found and updated.</span></span>

<span data-ttu-id="5d921-221">运行应用并导航到教师页。</span><span class="sxs-lookup"><span data-stu-id="5d921-221">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="5d921-222">替换*Pages/Instructors/Index.cshtml.cs*替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="5d921-222">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

<span data-ttu-id="5d921-223">`OnGetAsync`方法用于所选教师的 ID 将接受可选路线数据。</span><span class="sxs-lookup"><span data-stu-id="5d921-223">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="5d921-224">在检查查询*Pages/Instructors/Index.cshtml*页：</span><span class="sxs-lookup"><span data-stu-id="5d921-224">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="5d921-225">查询具有两个包括：</span><span class="sxs-lookup"><span data-stu-id="5d921-225">The query has two includes:</span></span>

* <span data-ttu-id="5d921-226">`OfficeAssignment`： 显示在[教师视图](#IP)。</span><span class="sxs-lookup"><span data-stu-id="5d921-226">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="5d921-227">`CourseAssignments`： 它可提供讲授的课程中。</span><span class="sxs-lookup"><span data-stu-id="5d921-227">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="5d921-228">更新教师索引页</span><span class="sxs-lookup"><span data-stu-id="5d921-228">Update the instructors Index page</span></span>

<span data-ttu-id="5d921-229">更新*Pages/Instructors/Index.cshtml*替换为以下标记：</span><span class="sxs-lookup"><span data-stu-id="5d921-229">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="5d921-230">前面的标记进行以下更改：</span><span class="sxs-lookup"><span data-stu-id="5d921-230">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="5d921-231">更新`page`指令从`@page`到`@page "{id:int?}"`。</span><span class="sxs-lookup"><span data-stu-id="5d921-231">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="5d921-232">`"{id:int?}"`是一个路由模板。</span><span class="sxs-lookup"><span data-stu-id="5d921-232">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="5d921-233">路由模板更改为路由数据整数在 URL 中的查询字符串。</span><span class="sxs-lookup"><span data-stu-id="5d921-233">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="5d921-234">例如，单击**选择**教师当页面指令产生类似于下面的 URL 的链接：</span><span class="sxs-lookup"><span data-stu-id="5d921-234">For example, clicking on the **Select** link for an instructor when the page directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="5d921-235">Page 指令时`@page "{id:int?}"`，以前的 URL 是：</span><span class="sxs-lookup"><span data-stu-id="5d921-235">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="5d921-236">页标题**教师**。</span><span class="sxs-lookup"><span data-stu-id="5d921-236">Page title is **Instructors**.</span></span>
* <span data-ttu-id="5d921-237">添加**Office**显示的列`item.OfficeAssignment.Location`才`item.OfficeAssignment`不为 null。</span><span class="sxs-lookup"><span data-stu-id="5d921-237">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="5d921-238">这是一个对零或一一个关系，因为不可能的相关的 OfficeAssignment 实体。</span><span class="sxs-lookup"><span data-stu-id="5d921-238">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="5d921-239">添加**课程**通过每个教师讲授显示课程的列。</span><span class="sxs-lookup"><span data-stu-id="5d921-239">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="5d921-240">请参阅[显式行转换与`@:`](xref:mvc/views/razor#explicit-line-transition-with-)有关此 razor 语法的详细信息。</span><span class="sxs-lookup"><span data-stu-id="5d921-240">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="5d921-241">添加动态添加的代码`class="success"`到`tr`的所选教师的元素。</span><span class="sxs-lookup"><span data-stu-id="5d921-241">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="5d921-242">此设置使用 Bootstrap 类所选行的背景色。</span><span class="sxs-lookup"><span data-stu-id="5d921-242">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="5d921-243">添加新的超链接标记为**选择**。</span><span class="sxs-lookup"><span data-stu-id="5d921-243">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="5d921-244">此链接将发送到所选的教师 ID`Index`方法，并设置背景色。</span><span class="sxs-lookup"><span data-stu-id="5d921-244">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="5d921-245">运行应用并选择**教师**选项卡。该页面将显示`Location`(office) 从相关`OfficeAssignment`实体。</span><span class="sxs-lookup"><span data-stu-id="5d921-245">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="5d921-246">如果 OfficeAssignment' 是 null，则将显示空表单元格。</span><span class="sxs-lookup"><span data-stu-id="5d921-246">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![未选择任何项教师索引页](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="5d921-248">单击**选择**链接。</span><span class="sxs-lookup"><span data-stu-id="5d921-248">Click on the **Select** link.</span></span> <span data-ttu-id="5d921-249">行样式更改。</span><span class="sxs-lookup"><span data-stu-id="5d921-249">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="5d921-250">添加由选教师讲授的课程</span><span class="sxs-lookup"><span data-stu-id="5d921-250">Add courses taught by selected instructor</span></span>

<span data-ttu-id="5d921-251">更新`OnGetAsync`中的方法*Pages/Instructors/Index.cshtml.cs*替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="5d921-251">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

<span data-ttu-id="5d921-252">检查更新的查询：</span><span class="sxs-lookup"><span data-stu-id="5d921-252">Examine the updated query:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="5d921-253">前面的查询添加`Department`实体。</span><span class="sxs-lookup"><span data-stu-id="5d921-253">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="5d921-254">下面的代码执行时选择一个教师 (`id != null`)。</span><span class="sxs-lookup"><span data-stu-id="5d921-254">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="5d921-255">所选的教师检索从教师视图模型中的列表中。</span><span class="sxs-lookup"><span data-stu-id="5d921-255">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="5d921-256">视图模型`Courses`属性加载与`Course`从该 instructor 实体`CourseAssignments`导航属性。</span><span class="sxs-lookup"><span data-stu-id="5d921-256">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="5d921-257">`Where`方法返回的集合。</span><span class="sxs-lookup"><span data-stu-id="5d921-257">The `Where` method returns a collection.</span></span> <span data-ttu-id="5d921-258">在前面`Where`方法，只有一个`Instructor`返回实体。</span><span class="sxs-lookup"><span data-stu-id="5d921-258">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="5d921-259">`Single`方法将集合转换为单个`Instructor`实体。</span><span class="sxs-lookup"><span data-stu-id="5d921-259">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="5d921-260">`Instructor`实体提供访问`CourseAssignments`属性。</span><span class="sxs-lookup"><span data-stu-id="5d921-260">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="5d921-261">`CourseAssignments`提供对相关访问`Course`实体。</span><span class="sxs-lookup"><span data-stu-id="5d921-261">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![教师课程多对多](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="5d921-263">`Single`在集合具有只有一个项目时，使用方法的集合。</span><span class="sxs-lookup"><span data-stu-id="5d921-263">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="5d921-264">`Single`方法引发异常，如果该集合为空或没有多个项。</span><span class="sxs-lookup"><span data-stu-id="5d921-264">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="5d921-265">替代方法是`SingleOrDefault`，它返回默认值 (在此情况下 null) 如果该集合为空。</span><span class="sxs-lookup"><span data-stu-id="5d921-265">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="5d921-266">使用`SingleOrDefault`对空集合：</span><span class="sxs-lookup"><span data-stu-id="5d921-266">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="5d921-267">将引发异常 (从尝试查找`Courses`上的 null 引用的属性)。</span><span class="sxs-lookup"><span data-stu-id="5d921-267">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="5d921-268">异常消息将不太清楚地指示问题的原因。</span><span class="sxs-lookup"><span data-stu-id="5d921-268">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="5d921-269">下面的代码填充视图模型`Enrollments`属性选择某一课程时：</span><span class="sxs-lookup"><span data-stu-id="5d921-269">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="5d921-270">将以下标记添加到末尾*Pages/Courses/Index.cshtml* Razor 页：</span><span class="sxs-lookup"><span data-stu-id="5d921-270">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

<span data-ttu-id="5d921-271">前面的标记显示选中一个教师与一个教师相关的课程的列表。</span><span class="sxs-lookup"><span data-stu-id="5d921-271">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="5d921-272">测试应用程序。</span><span class="sxs-lookup"><span data-stu-id="5d921-272">Test the app.</span></span> <span data-ttu-id="5d921-273">单击**选择**教师页上的链接。</span><span class="sxs-lookup"><span data-stu-id="5d921-273">Click on a **Select** link on the instructors page.</span></span>

![选择教师索引页教师](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="5d921-275">显示学生的数据</span><span class="sxs-lookup"><span data-stu-id="5d921-275">Show student data</span></span>

<span data-ttu-id="5d921-276">在此部分中，应用更新以显示所选过程的学生数据。</span><span class="sxs-lookup"><span data-stu-id="5d921-276">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="5d921-277">更新中的查询`OnGetAsync`中的方法*Pages/Instructors/Index.cshtml.cs*替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="5d921-277">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="5d921-278">更新*Pages/Instructors/Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="5d921-278">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="5d921-279">将以下标记添加到文件末尾：</span><span class="sxs-lookup"><span data-stu-id="5d921-279">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="5d921-280">前面的标记显示所选课程中注册的学生的列表。</span><span class="sxs-lookup"><span data-stu-id="5d921-280">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="5d921-281">刷新页面并选择一个教师。</span><span class="sxs-lookup"><span data-stu-id="5d921-281">Refresh the page and select an instructor.</span></span> <span data-ttu-id="5d921-282">选择要查看的已注册的学生和其年级列表课程。</span><span class="sxs-lookup"><span data-stu-id="5d921-282">Select a course to see the list of enrolled students and their grades.</span></span>

![教师索引页教师和所选课程](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="5d921-284">使用单个</span><span class="sxs-lookup"><span data-stu-id="5d921-284">Using Single</span></span>

<span data-ttu-id="5d921-285">`Single`方法可以传入`Where`条件而不是调用`Where`方法单独：</span><span class="sxs-lookup"><span data-stu-id="5d921-285">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="5d921-286">前面`Single`方法通过使用未提供任何好处`Where`。</span><span class="sxs-lookup"><span data-stu-id="5d921-286">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="5d921-287">一些开发人员更喜欢`Single`接近样式。</span><span class="sxs-lookup"><span data-stu-id="5d921-287">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="5d921-288">显式加载</span><span class="sxs-lookup"><span data-stu-id="5d921-288">Explicit loading</span></span>

<span data-ttu-id="5d921-289">当前的代码指定为预先加载`Enrollments`和`Students`:</span><span class="sxs-lookup"><span data-stu-id="5d921-289">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="5d921-290">假设用户很少会想要查看在课程中的注册。</span><span class="sxs-lookup"><span data-stu-id="5d921-290">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="5d921-291">在这种情况下，是一种优化仅加载注册数据，如果它请求。</span><span class="sxs-lookup"><span data-stu-id="5d921-291">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="5d921-292">在本部分中，`OnGetAsync`更新为使用显式加载`Enrollments`和`Students`。</span><span class="sxs-lookup"><span data-stu-id="5d921-292">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="5d921-293">更新`OnGetAsync`替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="5d921-293">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="5d921-294">前面的代码将删除*ThenInclude*方法调用注册和学生数据。</span><span class="sxs-lookup"><span data-stu-id="5d921-294">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="5d921-295">如果选择了某一课程，突出显示的代码检索：</span><span class="sxs-lookup"><span data-stu-id="5d921-295">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="5d921-296">`Enrollment`所选课程的实体。</span><span class="sxs-lookup"><span data-stu-id="5d921-296">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="5d921-297">`Student`每个实体`Enrollment`。</span><span class="sxs-lookup"><span data-stu-id="5d921-297">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="5d921-298">请注意上述代码注释掉`.AsNoTracking()`。</span><span class="sxs-lookup"><span data-stu-id="5d921-298">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="5d921-299">只能为导航属性显式加载的跟踪的实体。</span><span class="sxs-lookup"><span data-stu-id="5d921-299">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="5d921-300">测试应用程序。</span><span class="sxs-lookup"><span data-stu-id="5d921-300">Test the app.</span></span> <span data-ttu-id="5d921-301">从用户角度看，应用程序的行为会与以前的版本相同。</span><span class="sxs-lookup"><span data-stu-id="5d921-301">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="5d921-302">下一教程演示如何更新相关的数据。</span><span class="sxs-lookup"><span data-stu-id="5d921-302">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="5d921-303">[上一页](xref:data/ef-rp/complex-data-model)
>[下一页](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="5d921-303">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>