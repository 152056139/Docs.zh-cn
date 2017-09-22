---
title: "ASP.NET 核心 MVC 与 EF 核心的排序、 筛选器、 分页-10 3"
author: tdykstra
description: "在本教程中，你将添加排序、 筛选和分页功能页上使用 ASP.NET Core 和实体框架核心。"
keywords: "ASP.NET 核心、 实体框架核心、 排序、 筛选器、 分页，分组"
ms.author: tdykstra
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: e6c1ff3c-5673-43bf-9c2d-077f6ada1f29
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 59fff4dbf4736f0776aac4072f3f4e2d40119842
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2017
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a><span data-ttu-id="9d16b-104">排序、 筛选、 分页和分组-ASP.NET 核心 MVC 教程 (3 的 10) 的 EF 核心</span><span class="sxs-lookup"><span data-stu-id="9d16b-104">Sorting, filtering, paging, and grouping - EF Core with ASP.NET Core MVC tutorial (3 of 10)</span></span>

<span data-ttu-id="9d16b-105">通过[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9d16b-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9d16b-106">Contoso 大学示例 web 应用程序演示如何创建使用实体框架核心和 Visual Studio 的 ASP.NET 核心 MVC web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="9d16b-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="9d16b-107">有关教程系列的信息，请参阅[序列中的第一个教程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="9d16b-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="9d16b-108">在前面的教程，你可以实现的学生实体的基本 CRUD 操作的网页的一组。</span><span class="sxs-lookup"><span data-stu-id="9d16b-108">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="9d16b-109">在本教程将向学生索引页添加排序、 筛选和分页功能。</span><span class="sxs-lookup"><span data-stu-id="9d16b-109">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="9d16b-110">你还将创建简单分组的页。</span><span class="sxs-lookup"><span data-stu-id="9d16b-110">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="9d16b-111">下图显示哪些页面将如下所示完成后。</span><span class="sxs-lookup"><span data-stu-id="9d16b-111">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="9d16b-112">列标题是用户可以单击以按该列排序的链接。</span><span class="sxs-lookup"><span data-stu-id="9d16b-112">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="9d16b-113">单击列标题反复将在升序和降序之间切换。</span><span class="sxs-lookup"><span data-stu-id="9d16b-113">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![学生索引页](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="9d16b-115">将列排序链接添加到学生索引页</span><span class="sxs-lookup"><span data-stu-id="9d16b-115">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="9d16b-116">若要添加排序学生索引页，你将更改`Index`学生控制器方法并将代码添加到学生索引视图。</span><span class="sxs-lookup"><span data-stu-id="9d16b-116">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="9d16b-117">添加排序功能向 Index 方法</span><span class="sxs-lookup"><span data-stu-id="9d16b-117">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="9d16b-118">在*StudentsController.cs*，替换`Index`方法替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="9d16b-118">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="9d16b-119">此代码接收`sortOrder`从 URL 中的查询字符串参数。</span><span class="sxs-lookup"><span data-stu-id="9d16b-119">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="9d16b-120">ASP.NET 核心 MVC 提供查询字符串值作为参数传递给的操作方法。</span><span class="sxs-lookup"><span data-stu-id="9d16b-120">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="9d16b-121">参数将为"Name"日期"，可以选择后跟下划线和字符串"desc"指定降序顺序的字符串。</span><span class="sxs-lookup"><span data-stu-id="9d16b-121">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="9d16b-122">默认排序顺序为升序。</span><span class="sxs-lookup"><span data-stu-id="9d16b-122">The default sort order is ascending.</span></span>

<span data-ttu-id="9d16b-123">第一次请求索引页时，没有任何查询字符串。</span><span class="sxs-lookup"><span data-stu-id="9d16b-123">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="9d16b-124">学生按升序显示按姓氏排序，而是默认值，因为回退通过用例中建立`switch`语句。</span><span class="sxs-lookup"><span data-stu-id="9d16b-124">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="9d16b-125">当用户单击列标题超链接，相应`sortOrder`查询字符串中提供值。</span><span class="sxs-lookup"><span data-stu-id="9d16b-125">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="9d16b-126">这两个`ViewData`元素 （NameSortParm 和 DateSortParm） 供视图，用于将列标题超链接配置使用相应的查询字符串值。</span><span class="sxs-lookup"><span data-stu-id="9d16b-126">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="9d16b-127">这些是三元语句。</span><span class="sxs-lookup"><span data-stu-id="9d16b-127">These are ternary statements.</span></span> <span data-ttu-id="9d16b-128">第一个指定，如果`sortOrder`参数为 null 或为空，NameSortParm 应设置为"name_desc"; 否则，它应设置为一个空字符串。</span><span class="sxs-lookup"><span data-stu-id="9d16b-128">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="9d16b-129">这两个语句启用要设置列标题的超链接，如下所示的视图：</span><span class="sxs-lookup"><span data-stu-id="9d16b-129">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="9d16b-130">当前的排序顺序</span><span class="sxs-lookup"><span data-stu-id="9d16b-130">Current sort order</span></span>  | <span data-ttu-id="9d16b-131">最后一个名称超链接</span><span class="sxs-lookup"><span data-stu-id="9d16b-131">Last Name Hyperlink</span></span> | <span data-ttu-id="9d16b-132">日期超链接</span><span class="sxs-lookup"><span data-stu-id="9d16b-132">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="9d16b-133">上次名称升序排列</span><span class="sxs-lookup"><span data-stu-id="9d16b-133">Last Name ascending</span></span>  | <span data-ttu-id="9d16b-134">descending</span><span class="sxs-lookup"><span data-stu-id="9d16b-134">descending</span></span>          | <span data-ttu-id="9d16b-135">ascending</span><span class="sxs-lookup"><span data-stu-id="9d16b-135">ascending</span></span>      |
| <span data-ttu-id="9d16b-136">上次名称降序</span><span class="sxs-lookup"><span data-stu-id="9d16b-136">Last Name descending</span></span> | <span data-ttu-id="9d16b-137">ascending</span><span class="sxs-lookup"><span data-stu-id="9d16b-137">ascending</span></span>           | <span data-ttu-id="9d16b-138">ascending</span><span class="sxs-lookup"><span data-stu-id="9d16b-138">ascending</span></span>      |
| <span data-ttu-id="9d16b-139">升序的日期</span><span class="sxs-lookup"><span data-stu-id="9d16b-139">Date ascending</span></span>       | <span data-ttu-id="9d16b-140">ascending</span><span class="sxs-lookup"><span data-stu-id="9d16b-140">ascending</span></span>           | <span data-ttu-id="9d16b-141">descending</span><span class="sxs-lookup"><span data-stu-id="9d16b-141">descending</span></span>     |
| <span data-ttu-id="9d16b-142">日期降序</span><span class="sxs-lookup"><span data-stu-id="9d16b-142">Date descending</span></span>      | <span data-ttu-id="9d16b-143">ascending</span><span class="sxs-lookup"><span data-stu-id="9d16b-143">ascending</span></span>           | <span data-ttu-id="9d16b-144">ascending</span><span class="sxs-lookup"><span data-stu-id="9d16b-144">ascending</span></span>      |

<span data-ttu-id="9d16b-145">该方法使用 LINQ to Entities 指定要作为排序依据的列。</span><span class="sxs-lookup"><span data-stu-id="9d16b-145">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="9d16b-146">该代码创建`IQueryable`switch 语句中之前, 的变量对其进行修改在 switch 语句，并调用`ToListAsync`方法之后`switch`语句。</span><span class="sxs-lookup"><span data-stu-id="9d16b-146">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="9d16b-147">当你创建和修改`IQueryable`变量，没有查询发送到数据库。</span><span class="sxs-lookup"><span data-stu-id="9d16b-147">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="9d16b-148">您将转换之前，不执行查询`IQueryable`到通过调用方法，如集合对象`ToListAsync`。</span><span class="sxs-lookup"><span data-stu-id="9d16b-148">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="9d16b-149">因此，此代码将导致直到不执行单个查询`return View`语句。</span><span class="sxs-lookup"><span data-stu-id="9d16b-149">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

<span data-ttu-id="9d16b-150">此代码可以获得详细具有大量列。</span><span class="sxs-lookup"><span data-stu-id="9d16b-150">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="9d16b-151">[本系列最后一个教程](advanced.md#dynamic-linq)演示如何编写代码，你可以将名称传递给`OrderBy`字符串变量中的列。</span><span class="sxs-lookup"><span data-stu-id="9d16b-151">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="9d16b-152">将列标题的超链接添加到学生索引视图</span><span class="sxs-lookup"><span data-stu-id="9d16b-152">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="9d16b-153">中的代码替换*Views/Students/Index.cshtml*，替换为以下代码以添加列标题的超链接。</span><span class="sxs-lookup"><span data-stu-id="9d16b-153">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="9d16b-154">突出显示已更改的行。</span><span class="sxs-lookup"><span data-stu-id="9d16b-154">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="9d16b-155">此代码使用中的信息`ViewData`属性设置超链接与相应的查询字符串值。</span><span class="sxs-lookup"><span data-stu-id="9d16b-155">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="9d16b-156">运行应用程序中，选择**学生**卡，然后单击**姓氏**和**注册日期**列标题，以验证该排序工作原理。</span><span class="sxs-lookup"><span data-stu-id="9d16b-156">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![名称顺序中的学生索引页](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="9d16b-158">将一个搜索框添加到学生索引页</span><span class="sxs-lookup"><span data-stu-id="9d16b-158">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="9d16b-159">若要添加到学生索引页过滤条件，将向视图添加一个文本框和提交按钮，并将中的相应更改`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="9d16b-159">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="9d16b-160">文本框中，你将输入要在名字和姓氏字段中搜索的字符串。</span><span class="sxs-lookup"><span data-stu-id="9d16b-160">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="9d16b-161">将筛选功能添加到索引方法</span><span class="sxs-lookup"><span data-stu-id="9d16b-161">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="9d16b-162">在*StudentsController.cs*，替换`Index`方法替换为以下代码 （突出显示所做的更改）。</span><span class="sxs-lookup"><span data-stu-id="9d16b-162">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="9d16b-163">你已添加`searchString`参数`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="9d16b-163">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="9d16b-164">从将添加到索引视图的文本框中接收到搜索字符串值。</span><span class="sxs-lookup"><span data-stu-id="9d16b-164">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="9d16b-165">你已还添加到 LINQ 语句 where 子句选择仅学生的名字或姓氏包含搜索字符串。</span><span class="sxs-lookup"><span data-stu-id="9d16b-165">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="9d16b-166">添加 where 语句子句执行只有在要搜索的值。</span><span class="sxs-lookup"><span data-stu-id="9d16b-166">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="9d16b-167">此处调用`Where`方法`IQueryable`对象，并且筛选器将在服务器上处理。</span><span class="sxs-lookup"><span data-stu-id="9d16b-167">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="9d16b-168">在某些情况下你可能会调用`Where`作为扩展方法对内存中集合的方法。</span><span class="sxs-lookup"><span data-stu-id="9d16b-168">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="9d16b-169">(例如，假设你更改对引用`_context.Students`，因此的 EF`DbSet`它引用返回的存储库方法`IEnumerable`集合。)结果通常将相同，但在某些情况下可能会不同。</span><span class="sxs-lookup"><span data-stu-id="9d16b-169">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="9d16b-170">例如，.NET Framework 实现的`Contains`方法执行区分大小写的比较，默认情况下，但 SQL Server 中这由 SQL Server 实例的排序规则设置。</span><span class="sxs-lookup"><span data-stu-id="9d16b-170">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="9d16b-171">该设置默认为不区分大小写。</span><span class="sxs-lookup"><span data-stu-id="9d16b-171">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="9d16b-172">您可以调用`ToUpper`来进行测试显式不区分大小写的方法：*其中 (s = > s.LastName.ToUpper()。Contains(searchString.ToUpper())*。</span><span class="sxs-lookup"><span data-stu-id="9d16b-172">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="9d16b-173">这将确保如果更改代码更高版本才能使用它返回的存储库，结果保持相同`IEnumerable`而不是集合`IQueryable`对象。</span><span class="sxs-lookup"><span data-stu-id="9d16b-173">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="9d16b-174">(当你调用`Contains`方法`IEnumerable`集合，你将获取.NET Framework 实现; 当上调用它`IQueryable`对象，则会出现的数据库提供程序实现。)但是，没有为此解决方案对性能产生负面影响。</span><span class="sxs-lookup"><span data-stu-id="9d16b-174">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there is a performance penalty for this solution.</span></span> <span data-ttu-id="9d16b-175">`ToUpper`代码将放入 TSQL SELECT 语句的 WHERE 子句的函数。</span><span class="sxs-lookup"><span data-stu-id="9d16b-175">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="9d16b-176">将阻止优化器使用索引。</span><span class="sxs-lookup"><span data-stu-id="9d16b-176">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="9d16b-177">假设 SQL 主要安装为不区分大小写，最好是避免`ToUpper`代码之前你将迁移到区分大小写的数据存储区。</span><span class="sxs-lookup"><span data-stu-id="9d16b-177">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="9d16b-178">将一个搜索框添加到学生索引视图</span><span class="sxs-lookup"><span data-stu-id="9d16b-178">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="9d16b-179">在*Views/Student/Index.cshtml*，打开以便创建标题，文本框中，表标签之前立即添加突出显示的代码和**搜索**按钮。</span><span class="sxs-lookup"><span data-stu-id="9d16b-179">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="9d16b-180">此代码使用`<form>`[标记帮助器](xref:mvc/views/tag-helpers/intro)添加搜索文本框和按钮。</span><span class="sxs-lookup"><span data-stu-id="9d16b-180">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="9d16b-181">默认情况下，`<form>`标记帮助器提交 post，这意味着，参数作为进行传递 HTTP 消息正文中，不能在 URL 查询字符串的窗体数据。</span><span class="sxs-lookup"><span data-stu-id="9d16b-181">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="9d16b-182">指定 HTTP GET 时，窗体数据是在 URL 中作为查询字符串传递，这使得用户能够创建 URL 的书签。</span><span class="sxs-lookup"><span data-stu-id="9d16b-182">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="9d16b-183">操作未导致更新时，将收到 W3C 准则，则建议你应使用。</span><span class="sxs-lookup"><span data-stu-id="9d16b-183">The W3C guidelines recommend that you should use GET when the action does not result in an update.</span></span>

<span data-ttu-id="9d16b-184">运行应用程序中，选择**学生**选项卡上，输入搜索字符串，然后单击搜索以验证筛选是否正常工作。</span><span class="sxs-lookup"><span data-stu-id="9d16b-184">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![使用筛选的学生索引页](sort-filter-page/_static/filtering.png)

<span data-ttu-id="9d16b-186">请注意该 URL 包含搜索字符串。</span><span class="sxs-lookup"><span data-stu-id="9d16b-186">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="9d16b-187">如果此页加入书签，你将获得的筛选的列表，当你使用书签。</span><span class="sxs-lookup"><span data-stu-id="9d16b-187">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="9d16b-188">添加`method="get"`到`form`标记是什么导致要生成的查询字符串。</span><span class="sxs-lookup"><span data-stu-id="9d16b-188">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="9d16b-189">在此阶段，如果您单击的列标题排序链接你将丢失中输入的筛选器值**搜索**框。</span><span class="sxs-lookup"><span data-stu-id="9d16b-189">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="9d16b-190">你将在下一部分中修复此问题。</span><span class="sxs-lookup"><span data-stu-id="9d16b-190">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="9d16b-191">将分页功能添加到学生索引页</span><span class="sxs-lookup"><span data-stu-id="9d16b-191">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="9d16b-192">将分页添加到学生索引页，你将创建`PaginatedList`类，该类使用`Skip`和`Take`语句而不是始终检索表的所有行的服务器上的数据进行筛选。</span><span class="sxs-lookup"><span data-stu-id="9d16b-192">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="9d16b-193">然后你将使中的其他更改`Index`方法和分页将按钮添加到`Index`视图。</span><span class="sxs-lookup"><span data-stu-id="9d16b-193">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="9d16b-194">下图显示了分页按钮。</span><span class="sxs-lookup"><span data-stu-id="9d16b-194">The following illustration shows the paging buttons.</span></span>

![学生索引页，带有分页链接](sort-filter-page/_static/paging.png)

<span data-ttu-id="9d16b-196">在项目文件夹中，创建`PaginatedList.cs`，然后将模板代码替换下面的代码。</span><span class="sxs-lookup"><span data-stu-id="9d16b-196">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="9d16b-197">`CreateAsync`在此代码中的方法采用页大小和页码，并应用相应`Skip`和`Take`向语句`IQueryable`。</span><span class="sxs-lookup"><span data-stu-id="9d16b-197">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="9d16b-198">当`ToListAsync`上调用`IQueryable`，它将返回包含请求的页的列表。</span><span class="sxs-lookup"><span data-stu-id="9d16b-198">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="9d16b-199">属性`HasPreviousPage`和`HasNextPage`可用来启用或禁用**上一步**和**下一步**分页按钮。</span><span class="sxs-lookup"><span data-stu-id="9d16b-199">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="9d16b-200">A`CreateAsync`方法用于而不是一个构造函数创建`PaginatedList<T>`对象，因为构造函数不能运行异步代码。</span><span class="sxs-lookup"><span data-stu-id="9d16b-200">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="9d16b-201">将分页功能添加到索引方法</span><span class="sxs-lookup"><span data-stu-id="9d16b-201">Add paging functionality to the Index method</span></span>

<span data-ttu-id="9d16b-202">在*StudentsController.cs*，替换`Index`方法替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="9d16b-202">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="9d16b-203">此代码将页数字参数、 当前的排序顺序参数和当前的筛选器参数添加到方法签名中。</span><span class="sxs-lookup"><span data-stu-id="9d16b-203">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="9d16b-204">第一次显示页面，或如果用户未单击分页或排序链接，则所有参数将都为 null。</span><span class="sxs-lookup"><span data-stu-id="9d16b-204">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="9d16b-205">单击分页链接时，如果页变量将包含要显示的页码。</span><span class="sxs-lookup"><span data-stu-id="9d16b-205">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="9d16b-206">`ViewData`名为 CurrentSort 元素提供了与当前的排序顺序中，视图，因为这必须包含在分页链接以保持排序顺序分页时相同。</span><span class="sxs-lookup"><span data-stu-id="9d16b-206">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="9d16b-207">`ViewData`元素名为当前筛选器提供了与当前的筛选器字符串的视图。</span><span class="sxs-lookup"><span data-stu-id="9d16b-207">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="9d16b-208">若要分页，过程维护的筛选器设置情况下，此值必须包括在分页链接，必须还原到文本框中时重新显示页。</span><span class="sxs-lookup"><span data-stu-id="9d16b-208">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="9d16b-209">如果分页期间更改搜索字符串，页将显示被重置为 1，因为新的筛选器可能会导致不同的数据，以显示。</span><span class="sxs-lookup"><span data-stu-id="9d16b-209">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="9d16b-210">在文本框中输入了值，按下提交按钮，则会更改搜索字符串。</span><span class="sxs-lookup"><span data-stu-id="9d16b-210">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="9d16b-211">在这种情况下，`searchString`参数不为 null。</span><span class="sxs-lookup"><span data-stu-id="9d16b-211">In that case, the `searchString` parameter is not null.</span></span>

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

<span data-ttu-id="9d16b-212">在结束`Index`方法，`PaginatedList.CreateAsync`方法将学生查询转换为支持分页的集合类型中的学生的单页。</span><span class="sxs-lookup"><span data-stu-id="9d16b-212">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="9d16b-213">学生的单个页面然后传递给视图。</span><span class="sxs-lookup"><span data-stu-id="9d16b-213">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="9d16b-214">`PaginatedList.CreateAsync`方法采用页号。</span><span class="sxs-lookup"><span data-stu-id="9d16b-214">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="9d16b-215">两个问号表示 null 合并运算符。</span><span class="sxs-lookup"><span data-stu-id="9d16b-215">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="9d16b-216">Null 合并运算符定义可以为 null 的类型; 默认值表达式`(page ?? 1)`意味着返回的值`page`如果它具有一个值，或返回 1，如果`page`为 null。</span><span class="sxs-lookup"><span data-stu-id="9d16b-216">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="9d16b-217">将分页链接添加到学生索引视图</span><span class="sxs-lookup"><span data-stu-id="9d16b-217">Add paging links to the Student Index view</span></span>

<span data-ttu-id="9d16b-218">在*Views/Students/Index.cshtml*，替换为以下代码替换现有代码。</span><span class="sxs-lookup"><span data-stu-id="9d16b-218">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="9d16b-219">突出显示所做的更改。</span><span class="sxs-lookup"><span data-stu-id="9d16b-219">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="9d16b-220">`@model`页顶部的语句指定视图现在获取`PaginatedList<T>`对象而不是`List<T>`对象。</span><span class="sxs-lookup"><span data-stu-id="9d16b-220">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="9d16b-221">列标题链接使用查询字符串将当前的搜索字符串传递到控制器，以便用户可以在筛选器的结果中进行排序：</span><span class="sxs-lookup"><span data-stu-id="9d16b-221">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="9d16b-222">分页按钮显示通过标记帮助程序：</span><span class="sxs-lookup"><span data-stu-id="9d16b-222">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="9d16b-223">运行应用并转到学生页。</span><span class="sxs-lookup"><span data-stu-id="9d16b-223">Run the app and go to the Students page.</span></span>

![学生索引页，带有分页链接](sort-filter-page/_static/paging.png)

<span data-ttu-id="9d16b-225">单击以确保分页工作原理的不同的排序顺序中的分页链接。</span><span class="sxs-lookup"><span data-stu-id="9d16b-225">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="9d16b-226">然后输入搜索字符串，然后重试以验证分页还适用正确使用排序和筛选的分页。</span><span class="sxs-lookup"><span data-stu-id="9d16b-226">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="9d16b-227">创建一个显示学生统计信息的关于页面</span><span class="sxs-lookup"><span data-stu-id="9d16b-227">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="9d16b-228">为 Contoso 大学网站**有关**页上，将显示如何很多学生已注册的每个注册日期。</span><span class="sxs-lookup"><span data-stu-id="9d16b-228">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="9d16b-229">这要求在组上的分组和简单计算。</span><span class="sxs-lookup"><span data-stu-id="9d16b-229">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="9d16b-230">要完成此操作，将执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="9d16b-230">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="9d16b-231">创建一个视图模型类，你需要将传递到该视图的数据。</span><span class="sxs-lookup"><span data-stu-id="9d16b-231">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="9d16b-232">修改对 Home 控制器中的关于方法。</span><span class="sxs-lookup"><span data-stu-id="9d16b-232">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="9d16b-233">修改关于视图。</span><span class="sxs-lookup"><span data-stu-id="9d16b-233">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="9d16b-234">创建视图模型</span><span class="sxs-lookup"><span data-stu-id="9d16b-234">Create the view model</span></span>

<span data-ttu-id="9d16b-235">创建*SchoolViewModels*文件夹中的*模型*文件夹。</span><span class="sxs-lookup"><span data-stu-id="9d16b-235">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="9d16b-236">在新的文件夹中，将类文件添加*EnrollmentDateGroup.cs*和模板代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="9d16b-236">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="9d16b-237">修改主控制器</span><span class="sxs-lookup"><span data-stu-id="9d16b-237">Modify the Home Controller</span></span>

<span data-ttu-id="9d16b-238">在*HomeController.cs*，添加以下 using 语句在该文件的顶部：</span><span class="sxs-lookup"><span data-stu-id="9d16b-238">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="9d16b-239">在类中，左大括号后立即添加的数据库上下文的类变量，并从 ASP.NET 核心 DI 获取上下文的实例：</span><span class="sxs-lookup"><span data-stu-id="9d16b-239">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="9d16b-240">将 `About` 方法替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="9d16b-240">Replace the `About` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="9d16b-241">LINQ 语句学生实体分组按注册日期，计算每个组中的实体数并将结果存储在集合中的`EnrollmentDateGroup`查看模型对象。</span><span class="sxs-lookup"><span data-stu-id="9d16b-241">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE] 
> <span data-ttu-id="9d16b-242">在实体框架核心 1.0 版本中，整个结果集返回到客户端，并在客户端上进行分组。</span><span class="sxs-lookup"><span data-stu-id="9d16b-242">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="9d16b-243">在某些情况下，这会导致性能问题。</span><span class="sxs-lookup"><span data-stu-id="9d16b-243">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="9d16b-244">请务必用生产增长的数据，测试性能，如有必要使用原始 SQL 执行对服务器进行分组。</span><span class="sxs-lookup"><span data-stu-id="9d16b-244">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="9d16b-245">有关如何使用原始的 SQL 的信息，请参阅[本系列最后一个教程](advanced.md)。</span><span class="sxs-lookup"><span data-stu-id="9d16b-245">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="9d16b-246">修改有关视图</span><span class="sxs-lookup"><span data-stu-id="9d16b-246">Modify the About View</span></span>

<span data-ttu-id="9d16b-247">中的代码替换*Views/Home/About.cshtml*文件替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="9d16b-247">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="9d16b-248">运行应用并转到关于页面。</span><span class="sxs-lookup"><span data-stu-id="9d16b-248">Run the app and go to the About page.</span></span> <span data-ttu-id="9d16b-249">每个注册日期的学生计数显示在表格中。</span><span class="sxs-lookup"><span data-stu-id="9d16b-249">The count of students for each enrollment date is displayed in a table.</span></span>

![有关页面](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="9d16b-251">摘要</span><span class="sxs-lookup"><span data-stu-id="9d16b-251">Summary</span></span>

<span data-ttu-id="9d16b-252">在本教程中，你已了解如何执行排序、 筛选、 分页和分组。</span><span class="sxs-lookup"><span data-stu-id="9d16b-252">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="9d16b-253">在下一步的教程中，你将了解如何通过使用迁移来处理数据模型更改。</span><span class="sxs-lookup"><span data-stu-id="9d16b-253">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="9d16b-254">[上一页](crud.md)
[下一页](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="9d16b-254">[Previous](crud.md)
[Next](migrations.md)</span></span>  
