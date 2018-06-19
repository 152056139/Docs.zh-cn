---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: 拖放通过 ReorderList (C#) |Microsoft 文档
author: wenz
description: AJAX 控件工具包中的 ReorderList 控件提供可由用户通过拖放重新排序的列表。 对当前的列表顺序应...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 42464d10f119e0ba51d5eebf2a67e76e3e419bda
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873451"
---
<a name="drag-and-drop-via-reorderlist-c"></a><span data-ttu-id="53e0c-104">拖放通过 ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="53e0c-104">Drag and Drop via ReorderList (C#)</span></span>
====================
<span data-ttu-id="53e0c-105">通过[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="53e0c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="53e0c-106">[下载代码](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip)或[下载 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="53e0c-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span></span>

> <span data-ttu-id="53e0c-107">AJAX 控件工具包中的 ReorderList 控件提供可由用户通过拖放重新排序的列表。</span><span class="sxs-lookup"><span data-stu-id="53e0c-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="53e0c-108">应在服务器保留对当前的列表顺序。</span><span class="sxs-lookup"><span data-stu-id="53e0c-108">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="53e0c-109">概述</span><span class="sxs-lookup"><span data-stu-id="53e0c-109">Overview</span></span>

<span data-ttu-id="53e0c-110">`ReorderList` AJAX 控件工具包中的控件提供可由用户通过拖放重新排序的列表。</span><span class="sxs-lookup"><span data-stu-id="53e0c-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="53e0c-111">应在服务器保留对当前的列表顺序。</span><span class="sxs-lookup"><span data-stu-id="53e0c-111">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="53e0c-112">步骤</span><span class="sxs-lookup"><span data-stu-id="53e0c-112">Steps</span></span>

<span data-ttu-id="53e0c-113">`ReorderList`控件支持将数据从数据库绑定到列表。</span><span class="sxs-lookup"><span data-stu-id="53e0c-113">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="53e0c-114">最重要的是，它还支持将更改写入回数据存储区的列表元素的顺序。</span><span class="sxs-lookup"><span data-stu-id="53e0c-114">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="53e0c-115">此示例使用作为数据存储区的 Microsoft SQL Server 2005 Express Edition。</span><span class="sxs-lookup"><span data-stu-id="53e0c-115">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="53e0c-116">数据库是包括速成版的 Visual Studio 安装的可选 （和可用） 组成部分。</span><span class="sxs-lookup"><span data-stu-id="53e0c-116">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="53e0c-117">此外，还可以作为单独的下载下[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。</span><span class="sxs-lookup"><span data-stu-id="53e0c-117">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="53e0c-118">对于此示例中，我们假定的 SQL Server 2005 Express Edition 实例称为`SQLEXPRESS`和驻留在与 web 服务器; 相同的计算机上也是默认设置。</span><span class="sxs-lookup"><span data-stu-id="53e0c-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="53e0c-119">如果你的设置不同，你必须调整数据库的连接信息。</span><span class="sxs-lookup"><span data-stu-id="53e0c-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="53e0c-120">将数据库设置的最简单方法是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;-4a83-b309-53b7b77edf78&displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) )。</span><span class="sxs-lookup"><span data-stu-id="53e0c-120">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="53e0c-121">连接到服务器，请双击`Databases`并创建一个新的数据库 (右键单击并选择`New Database`) 调用`Tutorials`。</span><span class="sxs-lookup"><span data-stu-id="53e0c-121">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="53e0c-122">在此数据库中，创建名为的新表`AJAX`与以下四列：</span><span class="sxs-lookup"><span data-stu-id="53e0c-122">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="53e0c-123">`id` (主键，整数，标识，不为 NULL)</span><span class="sxs-lookup"><span data-stu-id="53e0c-123">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="53e0c-124">`char` （char （1)，NULL）</span><span class="sxs-lookup"><span data-stu-id="53e0c-124">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="53e0c-125">`description` (varchar(50)，NULL)</span><span class="sxs-lookup"><span data-stu-id="53e0c-125">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="53e0c-126">`position` (int、 NULL)</span><span class="sxs-lookup"><span data-stu-id="53e0c-126">`position` (int, NULL)</span></span>


<span data-ttu-id="53e0c-127">[![AJAX 表的布局](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="53e0c-127">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="53e0c-128">AJAX 表的布局 ([单击以查看实际尺寸的图像](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="53e0c-128">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span></span>


<span data-ttu-id="53e0c-129">接下来，使用几个值填充表。</span><span class="sxs-lookup"><span data-stu-id="53e0c-129">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="53e0c-130">请注意，`position`的列包含的元素的排序顺序。</span><span class="sxs-lookup"><span data-stu-id="53e0c-130">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="53e0c-131">[![AJAX 表中的初始数据](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="53e0c-131">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span></span>

<span data-ttu-id="53e0c-132">AJAX 表中的初始数据 ([单击以查看实际尺寸的图像](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="53e0c-132">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span></span>


<span data-ttu-id="53e0c-133">下一步需要生成`SqlDataSource`控件与新的数据库和它的表进行通信。</span><span class="sxs-lookup"><span data-stu-id="53e0c-133">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="53e0c-134">数据源必须支持`SELECT`和`UPDATE`SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="53e0c-134">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="53e0c-135">当列表元素的顺序更高版本发生更改时，`ReorderList`控件会自动将提交到数据源的两个值`Update`命令： 新的位置和元素的 ID。</span><span class="sxs-lookup"><span data-stu-id="53e0c-135">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="53e0c-136">因此，数据源需要`<UpdateParameters>`这两个值的部分：</span><span class="sxs-lookup"><span data-stu-id="53e0c-136">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="53e0c-137">`ReorderList`控件需要设置以下属性：</span><span class="sxs-lookup"><span data-stu-id="53e0c-137">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="53e0c-138">`AllowReorder`： 是否可以重新排列列表项</span><span class="sxs-lookup"><span data-stu-id="53e0c-138">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="53e0c-139">`DataSourceID`： 数据源的 ID</span><span class="sxs-lookup"><span data-stu-id="53e0c-139">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="53e0c-140">`DataKeyField`： 的名称的数据源中主键列</span><span class="sxs-lookup"><span data-stu-id="53e0c-140">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="53e0c-141">`SortOrderField`： 提供的列表项的排序顺序数据源列</span><span class="sxs-lookup"><span data-stu-id="53e0c-141">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="53e0c-142">在`<DragHandleTemplate>`和`<ItemTemplate>`部分中，列表的布局可以细微调整。</span><span class="sxs-lookup"><span data-stu-id="53e0c-142">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="53e0c-143">此外，数据绑定是可能使用`Eval()`方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="53e0c-143">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="53e0c-144">以下 CSS 样式信息 (中引用`<DragHandleTemplate>`部分`ReorderList`控件) 可确保，鼠标指针将相应更改当悬停拖动句柄：</span><span class="sxs-lookup"><span data-stu-id="53e0c-144">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

<span data-ttu-id="53e0c-145">最后，`ScriptManager`控件初始化 ASP.NET AJAX 页：</span><span class="sxs-lookup"><span data-stu-id="53e0c-145">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="53e0c-146">在浏览器中运行此示例和有点重新排列列表项。</span><span class="sxs-lookup"><span data-stu-id="53e0c-146">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="53e0c-147">然后，重新加载该页面和/或具有在数据库的外观。</span><span class="sxs-lookup"><span data-stu-id="53e0c-147">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="53e0c-148">更改的位置所保留的天数和中的值也会反映`position`列数据库中的所有无需任何代码，只使用标记。</span><span class="sxs-lookup"><span data-stu-id="53e0c-148">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="53e0c-149">[![中的新列表项顺序根据数据库更改的数据](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="53e0c-149">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span></span>

<span data-ttu-id="53e0c-150">根据新列表的数据库更改数据项顺序 ([单击以查看实际尺寸的图像](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="53e0c-150">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="53e0c-151">[上一页](using-postbacks-with-reorderlist-cs.md)
> [下一页](using-postbacks-with-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="53e0c-151">[Previous](using-postbacks-with-reorderlist-cs.md)
[Next](using-postbacks-with-reorderlist-vb.md)</span></span>
