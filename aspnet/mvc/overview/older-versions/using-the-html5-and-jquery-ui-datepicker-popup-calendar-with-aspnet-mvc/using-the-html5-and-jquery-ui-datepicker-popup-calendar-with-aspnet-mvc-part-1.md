---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: 使用 ASP.NET MVC-第 1 部分中使用 HTML5 和 jQuery UI Datepicker 弹出日历 |Microsoft 文档
author: Rick-Anderson
description: 本教程将教您如何使用编辑器模板、 显示模板和 jQuery UI datepicker 弹出日历，ASP.NET MV 中的基础知识...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: 408b99c9ad4fbc8487e585ebed3183f9aedc9c10
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870682"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a><span data-ttu-id="080b9-103">使用 ASP.NET MVC-第 1 部分中使用 HTML5 和 jQuery UI Datepicker 弹出日历</span><span class="sxs-lookup"><span data-stu-id="080b9-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 1</span></span>
====================
<span data-ttu-id="080b9-104">通过[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="080b9-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="080b9-105">本教程将教您如何使用编辑器模板、 显示模板和 jQuery UI datepicker 弹出日历，ASP.NET MVC Web 应用程序中的基础知识。</span><span class="sxs-lookup"><span data-stu-id="080b9-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


<span data-ttu-id="080b9-106">本教程将教您如何使用编辑器模板、 显示模板和 jQuery 的基础知识[UI datepicker 弹出日历](http://plugins.jquery.com/project/datepicker)ASP.NET MVC Web 应用程序中。</span><span class="sxs-lookup"><span data-stu-id="080b9-106">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery [UI datepicker popup calendar](http://plugins.jquery.com/project/datepicker) in an ASP.NET MVC Web application.</span></span> <span data-ttu-id="080b9-107">对于本教程中，你可以使用 Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;)，这是 Microsoft Visual Studio 的免费版，或者如果你已具有，则可以使用 Visual Studio 2010 SP1。</span><span class="sxs-lookup"><span data-stu-id="080b9-107">For this tutorial, you can use Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;), which is a free version of Microsoft Visual Studio, or you can use Visual Studio 2010 SP1 if you already have that.</span></span>

<span data-ttu-id="080b9-108">在开始之前，请确保已安装下面列出的先决条件。</span><span class="sxs-lookup"><span data-stu-id="080b9-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="080b9-109">你可以通过单击以下链接安装所有这些： [Web 平台安装程序](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="080b9-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="080b9-110">或者，您可以单独安装所需的软件，使用以下链接：</span><span class="sxs-lookup"><span data-stu-id="080b9-110">Alternatively, you can individually install the required software using the following links:</span></span>

- [<span data-ttu-id="080b9-111">Visual Studio Web Developer Express SP1 系统必备</span><span class="sxs-lookup"><span data-stu-id="080b9-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [<span data-ttu-id="080b9-112">ASP.NET MVC 3 Tools 更新</span><span class="sxs-lookup"><span data-stu-id="080b9-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- <span data-ttu-id="080b9-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（运行时 + 工具支持）</span><span class="sxs-lookup"><span data-stu-id="080b9-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>

<span data-ttu-id="080b9-114">如果你使用 Visual Studio 2010 而不 Visual Web Developer 中，通过单击以下链接安装必备组件： [Visual Studio 2010 系统必备组件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="080b9-114">If you're using Visual Studio 2010 instead of Visual Web Developer, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>

<span data-ttu-id="080b9-115">本教程假定你已完成[Getting Started with MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)教程或你熟悉 ASP.NET MVC 开发。</span><span class="sxs-lookup"><span data-stu-id="080b9-115">This tutorial assumes you have completed the [Getting Started with MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) tutorial or that you're familiar with ASP.NET MVC development.</span></span> <span data-ttu-id="080b9-116">本教程开头中完成的项目[Getting Started with MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)教程。</span><span class="sxs-lookup"><span data-stu-id="080b9-116">This tutorial starts with the completed project from the [Getting Started with MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) tutorial.</span></span>

<span data-ttu-id="080b9-117">本教程演示 C# 中的代码。</span><span class="sxs-lookup"><span data-stu-id="080b9-117">This tutorial shows code in C#.</span></span> <span data-ttu-id="080b9-118">但是，[初学者项目](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)和已完成的项目也会出现在 Visual Basic。</span><span class="sxs-lookup"><span data-stu-id="080b9-118">However, the [starter project](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) and completed project are also available in Visual Basic.</span></span>

<span data-ttu-id="080b9-119">与 C# 和 Visual Basic 源代码的 Visual Studio 项目是用本主题可以附带：[下载](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)。</span><span class="sxs-lookup"><span data-stu-id="080b9-119">A Visual Studio project with C# and Visual Basic source code is available to accompany this topic: [Download](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).</span></span>

### <a name="what-youll-build"></a><span data-ttu-id="080b9-120">你将生成</span><span class="sxs-lookup"><span data-stu-id="080b9-120">What You'll Build</span></span>

<span data-ttu-id="080b9-121">你将添加模板 （具体而言，编辑和显示模板） 的简单的影片列表应用程序中创建到[Getting Started with MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)教程。</span><span class="sxs-lookup"><span data-stu-id="080b9-121">You'll add templates (specifically, edit and display templates) to the simple movie-listing application that was created in the [Getting Started with MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) tutorial.</span></span> <span data-ttu-id="080b9-122">你还将添加[jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)弹出日历，以简化输入日期的过程。</span><span class="sxs-lookup"><span data-stu-id="080b9-122">You will also add a [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) popup calendar to simplify the process of entering dates.</span></span> <span data-ttu-id="080b9-123">下面的屏幕截图使用显示 jQuery UI datepicker 弹出日历显示修改后的应用程序。</span><span class="sxs-lookup"><span data-stu-id="080b9-123">The following screenshot shows the modified application with the jQuery UI datepicker popup calendar displayed.</span></span>

![完成的 jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a><span data-ttu-id="080b9-125">你将学习的技能</span><span class="sxs-lookup"><span data-stu-id="080b9-125">Skills You'll Learn</span></span>

<span data-ttu-id="080b9-126">下面是你将要掌握的内容：</span><span class="sxs-lookup"><span data-stu-id="080b9-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="080b9-127">如何使用中的属性[DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空间以显示时会被控制数据的格式和处于编辑模式。</span><span class="sxs-lookup"><span data-stu-id="080b9-127">How to use attributes from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace to control the format of data when it's displayed and when it's in edit mode.</span></span>
- <span data-ttu-id="080b9-128">如何创建模板 （编辑和显示模板） 来控制数据的格式。</span><span class="sxs-lookup"><span data-stu-id="080b9-128">How to create templates (edit and display templates) to control the formatting of data.</span></span>
- <span data-ttu-id="080b9-129">如何添加[jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)作为输入日期字段的方法。</span><span class="sxs-lookup"><span data-stu-id="080b9-129">How to add the [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) as a way to enter date fields.</span></span>

### <a name="getting-started"></a><span data-ttu-id="080b9-130">入门</span><span class="sxs-lookup"><span data-stu-id="080b9-130">Getting Started</span></span>

<span data-ttu-id="080b9-131">如果你还没有从的初学者项目的影片列表应用，将其使用以下链接下载：[下载](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)。</span><span class="sxs-lookup"><span data-stu-id="080b9-131">If you don't already have the movie-listing application from the starter project, download it using the following link: [Download](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).</span></span> <span data-ttu-id="080b9-132">然后在 Windows 资源管理器，右键单击*MvcMovie.zip*文件，然后选择**属性**。</span><span class="sxs-lookup"><span data-stu-id="080b9-132">Then in Windows Explorer, right-click the *MvcMovie.zip* file and select **Properties**.</span></span> <span data-ttu-id="080b9-133">在**MvcMovie.zip 属性**对话框中，选择**解除阻止**。</span><span class="sxs-lookup"><span data-stu-id="080b9-133">In the **MvcMovie.zip Properties** dialog box, select **Unblock**.</span></span> <span data-ttu-id="080b9-134">(取消阻止一条安全警告，当你尝试使用 *.zip*已从 web 下载的文件。)</span><span class="sxs-lookup"><span data-stu-id="080b9-134">(Unblocking prevents a security warning that occurs when you try to use a *.zip* file that you've downloaded from the web.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

<span data-ttu-id="080b9-135">右键单击*MvcMovie.zip*文件，然后选择**提取所有**来解压缩该文件。</span><span class="sxs-lookup"><span data-stu-id="080b9-135">Right-click the *MvcMovie.zip* file and select **Extract All** to unzip the file.</span></span> <span data-ttu-id="080b9-136">在 Visual Web Developer 或 Visual Studio 2010 中，打开*MvcMovieCS\_TU.sln*文件。</span><span class="sxs-lookup"><span data-stu-id="080b9-136">In Visual Web Developer or Visual Studio 2010, open the *MvcMovieCS\_TU.sln* file.</span></span>

<span data-ttu-id="080b9-137">在**解决方案资源管理器**，双击*views/shared\\_Layout.cshtml*以将其打开。</span><span class="sxs-lookup"><span data-stu-id="080b9-137">In **Solution Explorer**, double-click the *Views\Shared\\_Layout.cshtml* to open it.</span></span> <span data-ttu-id="080b9-138">更改`H1`标头**MVC 影片应用程序**到**电影 jQuery**。</span><span class="sxs-lookup"><span data-stu-id="080b9-138">Change the `H1` header from **MVC Movie App** to **Movie jQuery**.</span></span> <span data-ttu-id="080b9-139">按 CTRL + F5 运行应用程序并单击**主页**选项卡上，将跳转到`Index`电影控制器方法。</span><span class="sxs-lookup"><span data-stu-id="080b9-139">Press CTRL+F5 to run the application and click the **Home** tab, which takes you to the `Index` method of the movie controller.</span></span> <span data-ttu-id="080b9-140">若要尝试应用程序，选择**编辑**链接和**详细信息**电影之一的链接。</span><span class="sxs-lookup"><span data-stu-id="080b9-140">To try out the application, select the **Edit** link and the **Details** link for one of the movies.</span></span> <span data-ttu-id="080b9-141">请注意，在中的索引、 编辑和详细信息视图中，发布日期和价格的很好的名称格式：</span><span class="sxs-lookup"><span data-stu-id="080b9-141">Notice that in the Index, Edit, and Details views, the release date and price are nicely formatted:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

<span data-ttu-id="080b9-142">日期和价格的格式是使用的结果[DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)上的属性的属性`Movie`类。</span><span class="sxs-lookup"><span data-stu-id="080b9-142">The formatting for the date and the price is the result of using the [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute on properties of the `Movie` class.</span></span>

<span data-ttu-id="080b9-143">打开*Movie.cs*文件，并注释掉`DisplayFormat`属性`ReleaseDate`和`Price`属性。</span><span class="sxs-lookup"><span data-stu-id="080b9-143">Open the *Movie.cs* file and comment out the `DisplayFormat` attribute on the `ReleaseDate` and `Price` properties.</span></span> <span data-ttu-id="080b9-144">生成`Movie`类类似如下所示：</span><span class="sxs-lookup"><span data-stu-id="080b9-144">The resulting `Movie` class looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

<span data-ttu-id="080b9-145">按 CTRL + F5 以运行应用程序并选择**主页**选项卡以查看影片列表。</span><span class="sxs-lookup"><span data-stu-id="080b9-145">Press CTRL+F5 again to run the application and select the **Home** tab to view the movie list.</span></span> <span data-ttu-id="080b9-146">这一次发布日期显示的日期和时间，并价格字段不再显示的货币符号。</span><span class="sxs-lookup"><span data-stu-id="080b9-146">This time the release date shows the date and time, and the price field no longer shows the currency symbol.</span></span> <span data-ttu-id="080b9-147">在你更改`Movie`类撤消了 nice 格式更早版本，看到但来修复此问题在一段时间。</span><span class="sxs-lookup"><span data-stu-id="080b9-147">Your change in the `Movie` class has undone the nice formatting that you saw earlier, but you'll fix that in a moment.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a><span data-ttu-id="080b9-148">使用 DataAnnotations 数据类型属性以指定数据类型</span><span class="sxs-lookup"><span data-stu-id="080b9-148">Using the DataAnnotations DataType Attribute to Specify the Data Type</span></span>

<span data-ttu-id="080b9-149">将注释掉`DisplayFormat`属性，则为`ReleaseDate`具有属性[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性，使用`Date`枚举。</span><span class="sxs-lookup"><span data-stu-id="080b9-149">Replace the commented-out `DisplayFormat` attribute for the `ReleaseDate` property with the [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute, using the `Date` enumeration.</span></span> <span data-ttu-id="080b9-150">替换`DisplayFormat`属性，则为`Price`具有属性[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)再次，属性这一次使用`Currency`枚举。</span><span class="sxs-lookup"><span data-stu-id="080b9-150">Replace the `DisplayFormat` attribute for the `Price` property with the [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute again, this time using the `Currency` enumeration.</span></span> <span data-ttu-id="080b9-151">这是完整的代码如下所示：</span><span class="sxs-lookup"><span data-stu-id="080b9-151">This is what the completed code looks like:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

<span data-ttu-id="080b9-152">运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="080b9-152">Run the application.</span></span> <span data-ttu-id="080b9-153">现在发布日期和价格属性的格式设置正确 （即使用相应的日期和货币格式）。</span><span class="sxs-lookup"><span data-stu-id="080b9-153">Now the release date and the price properties are formatted correctly (that is, using appropriate date and currency formats).</span></span> <span data-ttu-id="080b9-154">[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性提供类型元数据的内置的 ASP.NET MVC 模板，以便以正确的格式呈现字段。</span><span class="sxs-lookup"><span data-stu-id="080b9-154">The [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute provides type metadata for the built-in ASP.NET MVC templates so that the fields render in the correct format.</span></span> <span data-ttu-id="080b9-155">使用`DataType`属性优于使用`DisplayFormat`最初是在代码中，因为属性`DataType`特性使模型更简洁且更灵活的目的如国际化。</span><span class="sxs-lookup"><span data-stu-id="080b9-155">Using the `DataType` attribute is preferable to using the `DisplayFormat` attribute that was originally in the code, because the `DataType` attribute makes the model cleaner and more flexible for purposes like internationalization.</span></span>

<span data-ttu-id="080b9-156">在下一部分中，你将看到如何使自定义模板，以显示日期字段。</span><span class="sxs-lookup"><span data-stu-id="080b9-156">In the next section you'll see how to make custom templates to display date fields.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="080b9-157">下一篇</span><span class="sxs-lookup"><span data-stu-id="080b9-157">Next</span></span>](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
