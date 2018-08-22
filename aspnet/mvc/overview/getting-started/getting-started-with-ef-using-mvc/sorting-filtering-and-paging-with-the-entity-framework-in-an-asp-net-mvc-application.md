---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 排序、 筛选和分页与实体框架中的 ASP.NET MVC 应用程序 |Microsoft Docs
author: tdykstra
description: Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 应用程序...
ms.author: riande
ms.date: 06/01/2015
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a72d6cbce0e5f20360e1e9bfc20f4a75bfeb3343
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824655"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>排序、 筛选和分页与 ASP.NET MVC 应用程序中的实体框架
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下载 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 应用程序。 若要了解系列教程，请参阅[本系列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


上一教程中实现一组基本的 CRUD 操作执行的 web 页面`Student`实体。 在本教程将添加排序、 筛选和分页功能**学生**索引页。 同时，还将创建一个执行简单分组的页面。

下图展示了完成操作后的页面外观。 列标题是用户可以单击以按该列排序的链接。 重复单击列标题可在升降排序顺序之间切换。

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>向学生索引页添加列排序链接

若要添加到学生索引页进行排序，你将更改`Index`方法`Student`控制器并将代码添加到`Student`索引视图。

### <a name="add-sorting-functionality-to-the-index-method"></a>添加排序功能，到 Index 方法

在中*Controllers\StudentController.cs*，将为`Index`方法使用以下代码：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

此代码接收来自 URL 中的查询字符串的 `sortOrder` 参数。 ASP.NET MVC 提供查询字符串值作为操作方法的参数。 该参数将是一个字符串，可为“Name”或“Date”，可选择后跟下划线和字符串“desc”来指定降序。 默认排序顺序为升序。

首次请求索引页时，没有任何查询字符串。 学生显示按升序排序依据`LastName`，这是默认设置，如在 fall-through 事例所建立`switch`语句。 当用户单击列标题超链接时，查询字符串中会提供相应的 `sortOrder` 值。

这两个`ViewBag`使用变量，以便该视图可以配置与相应的查询字符串值的列标题超链接：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

这些是三元语句。 第一个指定，如果`sortOrder`参数为 null 或为空，`ViewBag.NameSortParm`应设置为"名称\_desc"; 否则为它应设置为空字符串。 通过这两个语句，视图可如下设置列标题超链接：

| 当前排序顺序 | 姓氏超链接 | 日期超链接 |
| --- | --- | --- |
| 姓氏升序 | descending | ascending |
| 姓氏降序 | ascending | ascending |
| 日期升序 | ascending | descending |
| 日期降序 | ascending | ascending |

该方法使用[LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx)指定要作为排序依据的列。 该代码会创建[IQueryable](https://msdn.microsoft.com/library/bb351562.aspx)变量之前`switch`语句，将修改在`switch`语句，并调用`ToList`方法之后`switch`语句。 当创建和修改 `IQueryable` 变量时，不会向数据库发送任何查询。 您将转换之前未执行查询`IQueryable`对象通过调用一个方法，如集合`ToList`。 因此，此代码导致直到才执行单个查询`return View`语句。

编写每个排序顺序不同的 LINQ 语句的替代方法，可以动态创建的 LINQ 语句。 有关动态 LINQ 的信息，请参阅[动态 LINQ](https://go.microsoft.com/fwlink/?LinkID=323957)。

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>添加列标题超链接到学生索引视图

在中*Views\Student\Index.cshtml*，替换`<tr>`和`<th>`的标题行的突出显示的代码元素：

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

此代码使用中的信息`ViewBag`属性具有相应查询的超链接设置字符串值。

运行页面，然后单击**姓氏**并**注册日期**列标题以验证排序是否工作。

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

单击后**姓氏**标题下方，学生显示按降序姓氏的顺序。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>向学生索引页添加搜索框

要向学生索引页添加筛选功能，需将文本框和提交按钮添加到视图，并在 `Index` 方法中做出相应的更改。 在文本框中输入一个字符串以在名字和姓氏字段中进行搜索。

### <a name="add-filtering-functionality-to-the-index-method"></a>向 Index 方法添加筛选功能

在中*Controllers\StudentController.cs*，将为`Index`用下面的代码 （所做的更改会突出显示） 的方法：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

已向 `Index` 方法添加 `searchString` 参数。 从要添加到索引视图的文本框中接收搜索字符串值。 你还向 LINQ 语句添加`where`子句仅选择的学生的名字或姓氏中包含搜索字符串。 添加的语句[其中](https://msdn.microsoft.com/library/bb535040.aspx)子句执行仅当没有要搜索的值。

> [!NOTE]
> 在许多情况下可以对实体框架实体集或作为内存中集合上的扩展方法调用相同的方法。 结果通常都是相同，但在某些情况下可能会不同。
> 
> 例如，.NET Framework 实现`Contains`方法将空字符串传递给它，而 SQL Server Compact 4.0 的实体框架提供程序返回空字符串零行，则返回所有行。 因此该示例中的代码 (将放`Where`内的语句`if`语句) 可确保对于所有版本的 SQL Server 获得相同的结果。 此外，.NET Framework 实现`Contains`方法默认情况下，执行区分大小写比较，但实体框架 SQL Server 提供程序默认情况下执行不区分大小写的比较。 因此，调用`ToUpper`使测试显式不区分大小写的方法可确保结果不更改时更改代码更高版本以使用存储库，它将返回`IEnumerable`而不是集合`IQueryable`对象。 （在 `IEnumerable` 集合上调用 `Contains` 方法时，将获得 .NET Framework 实现；当在 `IQueryable` 对象上调用它时，将获得数据库提供程序实现。）
> 
> Null 处理也可能会因不同的数据库提供程序，或者在使用`IQueryable`使用时，对象相比`IEnumerable`集合。 例如，在某些情况下`Where`如条件`table.Column != 0`可能不会返回具有列`null`作为值。 有关详细信息，请参阅[的 where 子句中的 null 变量不正确处理](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl)。


### <a name="add-a-search-box-to-the-student-index-view"></a>向“学生索引”视图添加搜索框

在中*Views\Student\Index.cshtml*，添加打开之前立即突出显示的代码`table`若要创建标题栏、 文本框中，标记和一个**搜索**按钮。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

运行页上，输入搜索字符串，然后单击**搜索**以验证筛选是否正常工作。

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

请注意，URL 不包含"an"搜索字符串，这意味着，如果此页加入书签，您不会获得筛选的列表，当您使用书签。 这同样适用于列排序链接，因为它们将对整个列表进行排序。 将更改**搜索**按钮，本教程的后面使用筛选器条件的查询字符串。

## <a name="add-paging-to-the-students-index-page"></a>向学生索引页添加分页

若要向学生索引页添加分页，首先你将安装**PagedList.Mvc** NuGet 包。 然后你将进行中的其他更改`Index`方法并添加分页链接到`Index`视图。 **PagedList.Mvc**是许多很好的分页和排序的 ASP.NET MVC 中，包之一，此处使用旨在仅作为示例，不为相对于其他选择为其建议。 下图显示了分页链接。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>安装 PagedList.MVC NuGet 包

NuGet **PagedList.Mvc**会自动安装包**PagedList**包作为依赖项。 **PagedList**打包安装`PagedList`的集合类型和扩展方法`IQueryable`和`IEnumerable`集合。 扩展方法创建单个页面中的数据`PagedList`共集合您`IQueryable`或`IEnumerable`，和`PagedList`集合提供了一些属性和方法的简化分页。 **PagedList.Mvc**包将安装的分页帮助器显示分页按钮。

从**工具**菜单中，选择**库程序包管理器**，然后**程序包管理器控制台**。

在中**程序包管理器控制台**窗口中，请确保**包源**是**nuget.org**并**默认项目**是**ContosoUniversity**，然后输入以下命令：

`Install-Package PagedList.Mvc`

![安装 PagedList.Mvc](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

生成项目。 

### <a name="add-paging-functionality-to-the-index-method"></a>向 Index 方法添加分页功能

在中*Controllers\StudentController.cs*，添加`using`语句`PagedList`命名空间：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

将 `Index` 方法替换为以下代码：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

此代码将添加`page`参数、 当前的排序顺序参数和当前的筛选器参数向方法签名：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

第一次显示页面时，或者如果用户没有单击分页或排序链接，所有参数都将为 NULL。 如果单击分页链接后，`page`变量将包含要显示的页码。

一个`ViewBag`属性提供了与当前的排序顺序中，视图，因为此值必须包含在分页链接为了保持排序顺序都分页时相同：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

另一个属性， `ViewBag.CurrentFilter`，视图提供当前筛选器字符串。 此值必须包含在分页链接中，以便在分页过程中保持筛选器设置，并且在页面重新显示时必须将其还原到文本框中。 如果在分页过程中搜索字符串发生变化，则页面必须重置为 1，因为新的筛选器会导致显示不同的数据。 在文本框中输入值并按下提交按钮，则会更改搜索字符串。 在这种情况下，`searchString`参数不为 null。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

该方法的末尾`ToPagedList`学生的扩展方法`IQueryable`对象将学生查询转换为支持分页的集合类型中的学生的单个页。 该单个学生页面然后传递给视图：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` 方法需要一个页码。 两个问号表示[null 合并运算符](https://msdn.microsoft.com/library/ms173224.aspx)。 NULL 合并运算符为可为 NULL 的类型定义默认值；表达式 `(page ?? 1)` 表示如果 `page` 有值，则返回该值，如果 `page` 为 NULL，则返回 1。

### <a name="add-paging-links-to-the-student-index-view"></a>向学生索引视图添加分页链接

在中*Views\Student\Index.cshtml*，现有代码替换为以下代码。 突出显示所作更改。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

页面顶部的 `@model` 语句指定视图现在获取的是 `PagedList` 对象，而不是 `List` 对象。

`using`语句`PagedList.Mvc`，访问 MVC 帮助程序的分页按钮。

该代码使用的重载[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) ，它指定允许[FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css)。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

默认值[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)提交表单数据对于 POST，这意味着，参数将 HTTP 消息正文中而不是在 URL 作为查询字符串传递。 当指定 HTTP GET 时，表单数据作为查询字符串在 URL 中传递，从而使用户能够将 URL 加入书签。 [使用 HTTP GET 的 W3C 指南](http://www.w3.org/2001/tag/doc/whenToUseGet.html)建议操作不会导致更新时应使用 GET。

单击新页面时可以看到当前的搜索字符串，将使用当前搜索字符串初始化文本框。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

列标题链接使用查询字符串向控制器传递当前搜索字符串，以便用户可以在筛选结果中进行排序：

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

显示当前页和总页数。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

如果没有要显示的页面，会显示"的 0 页 0"。 (在这种情况下页面数大于页计数因为`Model.PageNumber`为 1，和`Model.PageCount`为 0。)

通过显示分页按钮`PagedListPager`帮助程序：

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

`PagedListPager`帮助器提供多种可自定义，包括 Url 和样式设置的选项。 有关详细信息，请参阅[TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) GitHub 站点上。

运行页。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

单击不同排序顺序的分页链接，以确保分页正常工作。 然后输入一个搜索字符串并再次尝试分页，以验证分页也可以正确地进行排序和筛选。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>创建有关页面，其中显示学生统计信息

对于 Contoso 大学网站的一页，将显示每个注册日期注册学生的数量。 这需要对组进行分组和简单计算。 若要完成此操作，需要执行以下操作：

- 为需要传递给视图的数据创建一个视图模型类。
- 修改`About`中的方法`Home`控制器。
- 修改`About`视图。

### <a name="create-the-view-model"></a>创建视图模型

创建*Viewmodel*项目文件夹中的文件夹。 在该文件夹中，将类文件添加*EnrollmentDateGroup.cs*和模板代码替换为以下代码：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>修改主控制器

在中*HomeController.cs*，添加以下`using`语句的文件的顶部：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

类的左大括号后立即添加数据库上下文的类变量：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

将 `About` 方法替换为以下代码：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

LINQ 语句按注册日期对学生实体进行分组，计算每组中实体的数量，并将结果存储在 `EnrollmentDateGroup` 视图模型对象的集合中。

添加`Dispose`方法：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>修改“关于”视图

中的代码替换*Views\Home\About.cshtml*文件使用以下代码：

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

运行应用并单击**有关**链接。 表格中会显示每个注册日期的学生计数。

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>总结

在本教程中已了解如何创建数据模型并实现基本的 CRUD，排序、 筛选、 分页和分组功能。 在下一教程将开始通过展开数据模型来查看更高级的主题。

请在你喜欢本教程的内容，我们可以提高上留下反馈。 此外可以请求新主题[教我代码](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。

其他实体框架资源的链接可在[ASP.NET 数据访问-推荐的资源](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [上一页](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [下一页](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
