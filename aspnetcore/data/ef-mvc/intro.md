---
title: "ASP.NET 核心 MVC 与实体框架核心-10 的教程 1"
author: tdykstra
description: 
keywords: "ASP.NET 核心，实体框架核心，教程"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: b67c3d4a-f2bf-4132-a48b-4b0d599d7981
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/intro
ms.openlocfilehash: 949733119b4e3a4b8716f2bcc1f631949d5049bc
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-entity-framework-core-using-visual-studio-1-of-10"></a><span data-ttu-id="e6b65-103">ASP.NET 核心 MVC 和使用 Visual Studio (第 1 个 10) 的实体框架核心入门</span><span class="sxs-lookup"><span data-stu-id="e6b65-103">Getting started with ASP.NET Core MVC and Entity Framework Core using Visual Studio (1 of 10)</span></span>

<span data-ttu-id="e6b65-104">通过[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e6b65-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e6b65-105">Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework (EF) 核心 2.0 和 Visual Studio 2017 ASP.NET 核心 2.0 MVC web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="e6b65-105">The Contoso University sample web application demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="e6b65-106">示例应用程序是一个用于 fictional Contoso 大学网站。</span><span class="sxs-lookup"><span data-stu-id="e6b65-106">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="e6b65-107">它包括诸如学生许可、 过程创建和教师分配等功能。</span><span class="sxs-lookup"><span data-stu-id="e6b65-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="e6b65-108">这是一系列教程说明如何生成从零开始的 Contoso 大学示例应用程序中的第一个。</span><span class="sxs-lookup"><span data-stu-id="e6b65-108">This is the first in a series of tutorials that explain how to build the Contoso University sample application from scratch.</span></span>

[<span data-ttu-id="e6b65-109">下载或查看已完成的应用程序。</span><span class="sxs-lookup"><span data-stu-id="e6b65-109">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

<span data-ttu-id="e6b65-110">EF 核心 2.0 是 EF 的最新版本，但还没有的 EF 的所有功能 6.x。</span><span class="sxs-lookup"><span data-stu-id="e6b65-110">EF Core 2.0 is the latest version of EF but does not yet have all the features of EF 6.x.</span></span> <span data-ttu-id="e6b65-111">有关如何 EF 之间进行选择 6.x 和 EF 核心，请参阅[EF 核心 vs。EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/)。</span><span class="sxs-lookup"><span data-stu-id="e6b65-111">For information about how to choose between EF 6.x and EF Core, see [EF Core vs. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/).</span></span> <span data-ttu-id="e6b65-112">如果你选择 EF 6.x 时，请参阅[本系列教程的上一个版本](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application)。</span><span class="sxs-lookup"><span data-stu-id="e6b65-112">If you choose EF 6.x, see [the previous version of this tutorial series](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

> [!NOTE]
> * <span data-ttu-id="e6b65-113">本教程的 ASP.NET 核心 1.1 版本，请参阅[以 PDF 格式在本教程中的 VS 2017 Update 2 版本](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/efmvc/intro/_static/efmvc1.1.pdf)。</span><span class="sxs-lookup"><span data-stu-id="e6b65-113">For the ASP.NET Core 1.1 version of this tutorial, see the [VS 2017 Update 2 version of this tutorial in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/efmvc/intro/_static/efmvc1.1.pdf).</span></span>
> * <span data-ttu-id="e6b65-114">有关本教程的 Visual Studio 2015 版本，请参阅 [PDF 格式的 ASP.NET Core 文档的 VS 2015 版本](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf)。</span><span class="sxs-lookup"><span data-stu-id="e6b65-114">For the Visual Studio 2015 version of this tutorial, see the [VS 2015 version of ASP.NET Core documentation in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6b65-115">先决条件</span><span class="sxs-lookup"><span data-stu-id="e6b65-115">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a><span data-ttu-id="e6b65-116">疑难解答</span><span class="sxs-lookup"><span data-stu-id="e6b65-116">Troubleshooting</span></span>

<span data-ttu-id="e6b65-117">如果你遇到无法解决的问题，通常可以通过比较你的代码查找解决方案[已完成的项目](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)。</span><span class="sxs-lookup"><span data-stu-id="e6b65-117">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span></span> <span data-ttu-id="e6b65-118">有关常见错误以及如何解决这些列表，请参阅[最后一个教程系列中的故障排除部分](advanced.md#common-errors)。</span><span class="sxs-lookup"><span data-stu-id="e6b65-118">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="e6b65-119">如果没有找到你那里需要你可以将问题发布到为 StackOverflow.com [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core)或[EF 核心](https://stackoverflow.com/questions/tagged/entity-framework-core)。</span><span class="sxs-lookup"><span data-stu-id="e6b65-119">If you don't find what you need there, you can post a question to StackOverflow.com for  [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP] 
> <span data-ttu-id="e6b65-120">这是一系列 10 教程，其中每个基于前面教程中完成。</span><span class="sxs-lookup"><span data-stu-id="e6b65-120">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span>  <span data-ttu-id="e6b65-121">请考虑在每个成功的教程完成后保存项目的副本。</span><span class="sxs-lookup"><span data-stu-id="e6b65-121">Consider saving a copy of the project after each successful tutorial completion.</span></span>  <span data-ttu-id="e6b65-122">以后如果遇到问题，你可以开始通过从以前的教程，而不是追溯到整个序列的开头。</span><span class="sxs-lookup"><span data-stu-id="e6b65-122">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="the-contoso-university-web-application"></a><span data-ttu-id="e6b65-123">Contoso 大学 web 应用程序</span><span class="sxs-lookup"><span data-stu-id="e6b65-123">The Contoso University web application</span></span>

<span data-ttu-id="e6b65-124">你将在这些教程中构建的应用程序是简单大学网站。</span><span class="sxs-lookup"><span data-stu-id="e6b65-124">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="e6b65-125">用户可以查看和更新学生、 课程中和教师信息。</span><span class="sxs-lookup"><span data-stu-id="e6b65-125">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="e6b65-126">以下是一些你将创建的屏幕。</span><span class="sxs-lookup"><span data-stu-id="e6b65-126">Here are a few of the screens you'll create.</span></span>

![学生索引页](intro/_static/students-index.png)

![学生编辑页](intro/_static/student-edit.png)

<span data-ttu-id="e6b65-129">此站点的用户界面样式而保留接近什么由运行内置的任何模板，以便在本教程主要关注如何使用实体框架。</span><span class="sxs-lookup"><span data-stu-id="e6b65-129">The UI style of this site has been kept close to what's generated by the built-in templates, so that the tutorial can focus mainly on how to use the Entity Framework.</span></span>

## <a name="create-an-aspnet-core-mvc-web-application"></a><span data-ttu-id="e6b65-130">创建 ASP.NET 核心 MVC web 应用程序</span><span class="sxs-lookup"><span data-stu-id="e6b65-130">Create an ASP.NET Core MVC web application</span></span>

<span data-ttu-id="e6b65-131">打开 Visual Studio 并创建一个新 ASP.NET 核心 C# web 项目名为"ContosoUniversity"。</span><span class="sxs-lookup"><span data-stu-id="e6b65-131">Open Visual Studio and create a new ASP.NET Core C# web project named "ContosoUniversity".</span></span>

* <span data-ttu-id="e6b65-132">从**文件**菜单上，选择**新建 > 项目**。</span><span class="sxs-lookup"><span data-stu-id="e6b65-132">From the **File** menu, select **New > Project**.</span></span>

* <span data-ttu-id="e6b65-133">从左窗格中，选择**已安装 > Visual C# > Web**。</span><span class="sxs-lookup"><span data-stu-id="e6b65-133">From the left pane, select **Installed > Visual C# > Web**.</span></span>

* <span data-ttu-id="e6b65-134">选择**ASP.NET 核心 Web 应用程序**项目模板。</span><span class="sxs-lookup"><span data-stu-id="e6b65-134">Select the **ASP.NET Core Web Application** project template.</span></span>

* <span data-ttu-id="e6b65-135">输入**ContosoUniversity**作为名称，然后单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="e6b65-135">Enter **ContosoUniversity** as the name and click **OK**.</span></span>

  ![“新建项目”对话框](intro/_static/new-project.png)

* <span data-ttu-id="e6b65-137">等待**新 ASP.NET 核心 Web 应用程序 (.NET Core)**显示对话框</span><span class="sxs-lookup"><span data-stu-id="e6b65-137">Wait for the **New ASP.NET Core Web Application (.NET Core)** dialog to appear</span></span>

* <span data-ttu-id="e6b65-138">选择**ASP.NET 核心 2.0**和**Web 应用程序 （模型-视图-控制器）**模板。</span><span class="sxs-lookup"><span data-stu-id="e6b65-138">Select **ASP.NET Core 2.0** and the **Web Application (Model-View-Controller)** template.</span></span>

  <span data-ttu-id="e6b65-139">**注意：**本教程需要安装 ASP.NET 核心 2.0 和 EF 核心 2.0 或更高版本-请确保**ASP.NET 核心 1.1**未选中。</span><span class="sxs-lookup"><span data-stu-id="e6b65-139">**Note:** This tutorial requires ASP.NET Core 2.0 and EF Core 2.0 or later -- make sure that **ASP.NET Core 1.1** is not selected.</span></span>

* <span data-ttu-id="e6b65-140">请确保**身份验证**设置为**无身份验证**。</span><span class="sxs-lookup"><span data-stu-id="e6b65-140">Make sure **Authentication** is set to **No Authentication**.</span></span>

* <span data-ttu-id="e6b65-141">单击“确定” </span><span class="sxs-lookup"><span data-stu-id="e6b65-141">Click **OK**</span></span>

  ![新建 ASP.NET 项目对话框](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a><span data-ttu-id="e6b65-143">设置站点样式</span><span class="sxs-lookup"><span data-stu-id="e6b65-143">Set up the site style</span></span>

<span data-ttu-id="e6b65-144">几个简单的更改将设置站点菜单、 布局和主页。</span><span class="sxs-lookup"><span data-stu-id="e6b65-144">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="e6b65-145">打开*Views/Shared/_Layout.cshtml*并进行以下更改：</span><span class="sxs-lookup"><span data-stu-id="e6b65-145">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="e6b65-146">将每个匹配项的"ContosoUniversity"更改为"Contoso 大学"。</span><span class="sxs-lookup"><span data-stu-id="e6b65-146">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="e6b65-147">有三个匹配项。</span><span class="sxs-lookup"><span data-stu-id="e6b65-147">There are three occurrences.</span></span>

* <span data-ttu-id="e6b65-148">添加菜单项**学生**，**课程**，**教师**，和**部门**，并删除**联系人**菜单项。</span><span class="sxs-lookup"><span data-stu-id="e6b65-148">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="e6b65-149">突出显示所做的更改。</span><span class="sxs-lookup"><span data-stu-id="e6b65-149">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

<span data-ttu-id="e6b65-150">在*Views/Home/Index.cshtml*，将文件的内容替换为以下代码以将有关 ASP.NET 和 MVC 的文本替换为有关此应用程序的文本：</span><span class="sxs-lookup"><span data-stu-id="e6b65-150">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-html[](intro/samples/cu/Views/Home/Index.cshtml)]

<span data-ttu-id="e6b65-151">按 CTRL + F5 来运行该项目或选择**调试 > 启动但不调试**从菜单。</span><span class="sxs-lookup"><span data-stu-id="e6b65-151">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="e6b65-152">你看到其中包含你将创建这些教程中的页的选项卡的主页。</span><span class="sxs-lookup"><span data-stu-id="e6b65-152">You see the home page with tabs for the pages you'll create in these tutorials.</span></span>

![Contoso 大学主页](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a><span data-ttu-id="e6b65-154">实体框架核心 NuGet 包</span><span class="sxs-lookup"><span data-stu-id="e6b65-154">Entity Framework Core NuGet packages</span></span>

<span data-ttu-id="e6b65-155">若要添加到项目的 EF 核心支持，请安装你想要针对的数据库提供程序。</span><span class="sxs-lookup"><span data-stu-id="e6b65-155">To add EF Core support to a project, install the database provider that you want to target.</span></span> <span data-ttu-id="e6b65-156">本教程使用 SQL Server，并且提供程序包[Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)。</span><span class="sxs-lookup"><span data-stu-id="e6b65-156">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span> <span data-ttu-id="e6b65-157">此包包含在[Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage，因此你不必安装它。</span><span class="sxs-lookup"><span data-stu-id="e6b65-157">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="e6b65-158">此包和其依赖项 (`Microsoft.EntityFrameworkCore`和`Microsoft.EntityFrameworkCore.Relational`) 提供 EF 的运行时支持。</span><span class="sxs-lookup"><span data-stu-id="e6b65-158">This package and its dependencies (`Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`) provide runtime support for EF.</span></span> <span data-ttu-id="e6b65-159">你将添加工具包更高版本，在[迁移](migrations.md)教程。</span><span class="sxs-lookup"><span data-stu-id="e6b65-159">You'll add a tooling package later, in the [Migrations](migrations.md) tutorial.</span></span> 

<span data-ttu-id="e6b65-160">有关可用于实体框架核心其他数据库提供程序的信息，请参阅[数据库提供程序](https://docs.microsoft.com/ef/core/providers/)。</span><span class="sxs-lookup"><span data-stu-id="e6b65-160">For information about other database providers that are available for Entity Framework Core, see [Database providers](https://docs.microsoft.com/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="e6b65-161">创建数据模型</span><span class="sxs-lookup"><span data-stu-id="e6b65-161">Create the data model</span></span>

<span data-ttu-id="e6b65-162">接下来你将创建 Contoso 大学应用程序的实体类。</span><span class="sxs-lookup"><span data-stu-id="e6b65-162">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="e6b65-163">你将开始以下三个实体。</span><span class="sxs-lookup"><span data-stu-id="e6b65-163">You'll start with the following three entities.</span></span>

![课程注册学生数据模型关系图](intro/_static/data-model-diagram.png)

<span data-ttu-id="e6b65-165">之间的一对多关系`Student`和`Enrollment`实体，并且没有之间的一个对多关系`Course`和`Enrollment`实体。</span><span class="sxs-lookup"><span data-stu-id="e6b65-165">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="e6b65-166">换而言之，一名学生可以在任意数量的课程中, 注册，并且某一课程可以具有任意数量的学生在其中注册。</span><span class="sxs-lookup"><span data-stu-id="e6b65-166">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="e6b65-167">下列部分中，你将创建每个这些实体的类。</span><span class="sxs-lookup"><span data-stu-id="e6b65-167">In the following sections you'll create a class for each one of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="e6b65-168">学生实体</span><span class="sxs-lookup"><span data-stu-id="e6b65-168">The Student entity</span></span>

![学生实体关系图](intro/_static/student-entity.png)

<span data-ttu-id="e6b65-170">在*模型*文件夹中，创建一个名为的类文件*Student.cs*和模板代码替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="e6b65-170">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code.</span></span>

<span data-ttu-id="e6b65-171">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]</span><span class="sxs-lookup"><span data-stu-id="e6b65-171">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]</span></span>

<span data-ttu-id="e6b65-172">`ID`属性将成为对应于此类数据库表的主键列。</span><span class="sxs-lookup"><span data-stu-id="e6b65-172">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="e6b65-173">默认情况下，实体框架将解释一个属性，名为`ID`或`classnameID`为主键。</span><span class="sxs-lookup"><span data-stu-id="e6b65-173">By default, the Entity Framework interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="e6b65-174">`Enrollments`属性是导航属性。</span><span class="sxs-lookup"><span data-stu-id="e6b65-174">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="e6b65-175">导航属性将分别包含与此实体相关的其他实体。</span><span class="sxs-lookup"><span data-stu-id="e6b65-175">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="e6b65-176">在这种情况下，`Enrollments`属性`Student entity`将保留所有`Enrollment`与该订阅相关的实体`Student`实体。</span><span class="sxs-lookup"><span data-stu-id="e6b65-176">In this case, the `Enrollments` property of a `Student entity` will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="e6b65-177">换而言之，如果在数据库中给定的学生行有两个相关注册行 （包含该学生的主键值其 StudentID 外键列中的行），`Student`实体的`Enrollments`导航属性将包含那些两个`Enrollment`实体。</span><span class="sxs-lookup"><span data-stu-id="e6b65-177">In other words, if a given Student row in the database has two related Enrollment rows (rows that contain that student's primary key value in their StudentID foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="e6b65-178">如果导航属性可以具有多个实体 （如多对多或一对多关系），其类型必须是的列表中的条目可以是添加、 删除和更新，如`ICollection<T>`。</span><span class="sxs-lookup"><span data-stu-id="e6b65-178">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection<T>`.</span></span>  <span data-ttu-id="e6b65-179">你可以指定`ICollection<T>`或类型，如`List<T>`或`HashSet<T>`。</span><span class="sxs-lookup"><span data-stu-id="e6b65-179">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="e6b65-180">如果指定`ICollection<T>`，EF 创建`HashSet<T>`默认情况下的集合。</span><span class="sxs-lookup"><span data-stu-id="e6b65-180">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="e6b65-181">注册实体</span><span class="sxs-lookup"><span data-stu-id="e6b65-181">The Enrollment entity</span></span>

![注册实体关系图](intro/_static/enrollment-entity.png)

<span data-ttu-id="e6b65-183">在*模型*文件夹中，创建*Enrollment.cs*和替换为以下代码替换现有代码：</span><span class="sxs-lookup"><span data-stu-id="e6b65-183">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

<span data-ttu-id="e6b65-184">[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]</span><span class="sxs-lookup"><span data-stu-id="e6b65-184">[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]</span></span>

<span data-ttu-id="e6b65-185">`EnrollmentID`属性将是主键; 此实体使用`classnameID`模式而不是`ID`本身作为你在中看到`Student`实体。</span><span class="sxs-lookup"><span data-stu-id="e6b65-185">The `EnrollmentID` property will be the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="e6b65-186">通常，你将选择一个模式，并使用它在你的数据模型。</span><span class="sxs-lookup"><span data-stu-id="e6b65-186">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="e6b65-187">在这里，变体说明了你可以使用任一模式。</span><span class="sxs-lookup"><span data-stu-id="e6b65-187">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="e6b65-188">在[后面的教程](inheritance.md)，你将看到如何利用 ID 没有类名的情况下可使其更轻松地在数据模型中实现继承。</span><span class="sxs-lookup"><span data-stu-id="e6b65-188">In a [later tutorial](inheritance.md), you'll see how using ID without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="e6b65-189">`Grade`属性是`enum`。</span><span class="sxs-lookup"><span data-stu-id="e6b65-189">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="e6b65-190">后问号`Grade`类型声明指示`Grade`属性可以为 null。</span><span class="sxs-lookup"><span data-stu-id="e6b65-190">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="e6b65-191">为 null 的等级不从零个年级不同--null 意味着一个等级而言未知的或未尚未分配。</span><span class="sxs-lookup"><span data-stu-id="e6b65-191">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="e6b65-192">`StudentID`属性是一个外键，且相应的导航属性为`Student`。</span><span class="sxs-lookup"><span data-stu-id="e6b65-192">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="e6b65-193">`Enrollment`实体是与一个相关联`Student`实体，因此该属性只包含单个`Student`实体 (与不同`Student.Enrollments`导航属性前面所看到的后者可以容纳多个`Enrollment`实体)。</span><span class="sxs-lookup"><span data-stu-id="e6b65-193">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="e6b65-194">`CourseID`属性是一个外键，且相应的导航属性为`Course`。</span><span class="sxs-lookup"><span data-stu-id="e6b65-194">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="e6b65-195">`Enrollment`实体是与一个相关联`Course`实体。</span><span class="sxs-lookup"><span data-stu-id="e6b65-195">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="e6b65-196">实体框架： 将属性解释为外键属性，如果它名为`<navigation property name><primary key property name>`(例如，`StudentID`为`Student`以来的导航属性`Student`实体的主键是`ID`)。</span><span class="sxs-lookup"><span data-stu-id="e6b65-196">Entity Framework interprets a property as a foreign key property if it's named `<navigation property name><primary key property name>` (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="e6b65-197">此外可以只需命名外键属性`<primary key property name>`(例如，`CourseID`由于`Course`实体的主键是`CourseID`)。</span><span class="sxs-lookup"><span data-stu-id="e6b65-197">Foreign key properties can also be named simply `<primary key property name>` (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="e6b65-198">课程实体</span><span class="sxs-lookup"><span data-stu-id="e6b65-198">The Course entity</span></span>

![课程实体关系图](intro/_static/course-entity.png)

<span data-ttu-id="e6b65-200">在*模型*文件夹中，创建*Course.cs*和替换为以下代码替换现有代码：</span><span class="sxs-lookup"><span data-stu-id="e6b65-200">In the *Models* folder, create *Course.cs* and replace the existing code with the following code:</span></span>

<span data-ttu-id="e6b65-201">[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]</span><span class="sxs-lookup"><span data-stu-id="e6b65-201">[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]</span></span>

<span data-ttu-id="e6b65-202">`Enrollments`属性是导航属性。</span><span class="sxs-lookup"><span data-stu-id="e6b65-202">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="e6b65-203">A`Course`可以与任意数量的相关实体`Enrollment`实体。</span><span class="sxs-lookup"><span data-stu-id="e6b65-203">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="e6b65-204">我们举例更多有关`DatabaseGenerated`属性中[后面的教程](complex-data-model.md)本系列。</span><span class="sxs-lookup"><span data-stu-id="e6b65-204">We'll say more about the `DatabaseGenerated` attribute in a [later tutorial](complex-data-model.md) in this series.</span></span> <span data-ttu-id="e6b65-205">基本上，此属性允许您输入的过程，而不是让生成它的数据库的主键。</span><span class="sxs-lookup"><span data-stu-id="e6b65-205">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="e6b65-206">创建的数据库上下文</span><span class="sxs-lookup"><span data-stu-id="e6b65-206">Create the Database Context</span></span>

<span data-ttu-id="e6b65-207">协调为给定的数据模型的实体框架功能的主类是数据库上下文类。</span><span class="sxs-lookup"><span data-stu-id="e6b65-207">The main class that coordinates Entity Framework functionality for a given data model is the database context class.</span></span> <span data-ttu-id="e6b65-208">将通过从 `Microsoft.EntityFrameworkCore.DbContext` 类派生的方式创建此类。</span><span class="sxs-lookup"><span data-stu-id="e6b65-208">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="e6b65-209">在代码中你指定数据模型中包含哪些实体。</span><span class="sxs-lookup"><span data-stu-id="e6b65-209">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="e6b65-210">你还可以自定义某些实体框架行为。</span><span class="sxs-lookup"><span data-stu-id="e6b65-210">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="e6b65-211">在此项目中类命名为`SchoolContext`。</span><span class="sxs-lookup"><span data-stu-id="e6b65-211">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="e6b65-212">在项目文件夹中，创建名为的文件夹*数据*。</span><span class="sxs-lookup"><span data-stu-id="e6b65-212">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="e6b65-213">在*数据*文件夹创建名为的新类文件*SchoolContext.cs*，并将模板代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="e6b65-213">In the *Data* folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

<span data-ttu-id="e6b65-214">[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]</span><span class="sxs-lookup"><span data-stu-id="e6b65-214">[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]</span></span>

<span data-ttu-id="e6b65-215">此代码将创建`DbSet`每个实体集的属性。</span><span class="sxs-lookup"><span data-stu-id="e6b65-215">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="e6b65-216">在实体框架术语中，实体集通常与数据库表相对应，实体与表中的行相对应。</span><span class="sxs-lookup"><span data-stu-id="e6b65-216">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<span data-ttu-id="e6b65-217">可以省略`DbSet<Enrollment>`和`DbSet<Course>`语句和其效果相同。</span><span class="sxs-lookup"><span data-stu-id="e6b65-217">You could have omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="e6b65-218">实体框架会将其包含隐式因为`Student`实体引用`Enrollment`实体和`Enrollment`实体引用`Course`实体。</span><span class="sxs-lookup"><span data-stu-id="e6b65-218">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="e6b65-219">EF 创建数据库后，创建具有名称相同的表`DbSet`属性名称。</span><span class="sxs-lookup"><span data-stu-id="e6b65-219">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="e6b65-220">集合的属性名称通常是有关是否表名称应变为复数形式或不反对复数形式 （学生而不是学生），但开发人员。</span><span class="sxs-lookup"><span data-stu-id="e6b65-220">Property names for collections are typically plural (Students rather than Student), but developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="e6b65-221">对于这些教程将通过在 DbContext 中指定单数表名称来覆盖默认行为。</span><span class="sxs-lookup"><span data-stu-id="e6b65-221">For these tutorials you'll override the default behavior by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="e6b65-222">若要做到这一点，请在最后一个 DbSet 属性之后添加以下突出显示的代码。</span><span class="sxs-lookup"><span data-stu-id="e6b65-222">To do that, add the following highlighted code after the last DbSet property.</span></span>

<span data-ttu-id="e6b65-223">[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]</span><span class="sxs-lookup"><span data-stu-id="e6b65-223">[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]</span></span>

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="e6b65-224">上下文注册依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="e6b65-224">Register the context with dependency injection</span></span>

<span data-ttu-id="e6b65-225">ASP.NET 核心实现[依赖关系注入](../../fundamentals/dependency-injection.md)默认情况下。</span><span class="sxs-lookup"><span data-stu-id="e6b65-225">ASP.NET Core implements [dependency injection](../../fundamentals/dependency-injection.md) by default.</span></span> <span data-ttu-id="e6b65-226">在应用程序启动过程的依赖关系注入注册服务 （例如 EF 数据库上下文中）。</span><span class="sxs-lookup"><span data-stu-id="e6b65-226">Services (such as the EF database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="e6b65-227">需要这些服务 （如 MVC 控制器） 的组件提供了这些服务通过构造函数参数。</span><span class="sxs-lookup"><span data-stu-id="e6b65-227">Components that require these services (such as MVC controllers) are provided these services via constructor parameters.</span></span> <span data-ttu-id="e6b65-228">你将看到控制器构造函数代码，可在本教程后面获取上下文实例。</span><span class="sxs-lookup"><span data-stu-id="e6b65-228">You'll see the controller constructor code that gets a context instance later in this tutorial.</span></span>

<span data-ttu-id="e6b65-229">若要注册`SchoolContext`作为一种服务，打开*Startup.cs*，并添加到突出显示的行`ConfigureServices`方法。</span><span class="sxs-lookup"><span data-stu-id="e6b65-229">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

<span data-ttu-id="e6b65-230">[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]</span><span class="sxs-lookup"><span data-stu-id="e6b65-230">[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]</span></span>

<span data-ttu-id="e6b65-231">连接字符串的名称均在通过传递给上下文上调用的方法`DbContextOptionsBuilder`对象。</span><span class="sxs-lookup"><span data-stu-id="e6b65-231">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="e6b65-232">以进行本地开发， [ASP.NET 核心配置系统](../../fundamentals/configuration.md)读取中的连接字符串*appsettings.json*文件。</span><span class="sxs-lookup"><span data-stu-id="e6b65-232">For local development, the [ASP.NET Core configuration system](../../fundamentals/configuration.md) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="e6b65-233">添加`using`语句`ContosoUniversity.Data`和`Microsoft.EntityFrameworkCore`命名空间，然后生成项目。</span><span class="sxs-lookup"><span data-stu-id="e6b65-233">Add `using` statements for `ContosoUniversity.Data`  and `Microsoft.EntityFrameworkCore` namespaces, and then build the project.</span></span>

<span data-ttu-id="e6b65-234">[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]</span><span class="sxs-lookup"><span data-stu-id="e6b65-234">[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]</span></span>

<span data-ttu-id="e6b65-235">打开*appsettings.json*文件并添加连接字符串，如下面的示例中所示。</span><span class="sxs-lookup"><span data-stu-id="e6b65-235">Open the *appsettings.json* file and add a connection string as shown in the following example.</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a><span data-ttu-id="e6b65-236">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="e6b65-236">SQL Server Express LocalDB</span></span>

<span data-ttu-id="e6b65-237">连接字符串指定 SQL Server LocalDB 数据库。</span><span class="sxs-lookup"><span data-stu-id="e6b65-237">The connection string specifies a SQL Server LocalDB database.</span></span> <span data-ttu-id="e6b65-238">LocalDB 是 SQL Server Express 数据库引擎的轻量级版本，用于应用程序开发，不生产环境中使用。</span><span class="sxs-lookup"><span data-stu-id="e6b65-238">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for application development, not production use.</span></span> <span data-ttu-id="e6b65-239">LocalDB 按需启动和运行在用户模式下，因此没有复杂配置。</span><span class="sxs-lookup"><span data-stu-id="e6b65-239">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="e6b65-240">默认情况下，创建 LocalDB *.mdf*数据库中的文件`C:/Users/<user>`目录。</span><span class="sxs-lookup"><span data-stu-id="e6b65-240">By default, LocalDB creates *.mdf* database files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-database-with-test-data"></a><span data-ttu-id="e6b65-241">添加代码以初始化测试数据的数据库</span><span class="sxs-lookup"><span data-stu-id="e6b65-241">Add code to initialize the database with test data</span></span>

<span data-ttu-id="e6b65-242">实体框架将为你创建空数据库。</span><span class="sxs-lookup"><span data-stu-id="e6b65-242">The Entity Framework will create an empty database for you.</span></span>  <span data-ttu-id="e6b65-243">在本部分中，你编写以便填充测试数据创建数据库后调用的方法。</span><span class="sxs-lookup"><span data-stu-id="e6b65-243">In this section, you write a method that is called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="e6b65-244">此处将使用`EnsureCreated`方法来自动创建数据库。</span><span class="sxs-lookup"><span data-stu-id="e6b65-244">Here you'll use the `EnsureCreated` method to automatically create the database.</span></span> <span data-ttu-id="e6b65-245">在[后面的教程](migrations.md)你将了解如何通过使用 Code First 迁移以更改而不是删除并重新创建数据库的数据库架构处理模型更改。</span><span class="sxs-lookup"><span data-stu-id="e6b65-245">In a [later tutorial](migrations.md) you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="e6b65-246">在*数据*文件夹中，创建名为的新类文件*DbInitializer.cs*和模板代码替换为以下代码，这会导致要在需要时创建的数据库和负载测试到新的数据数据库。</span><span class="sxs-lookup"><span data-stu-id="e6b65-246">In the *Data* folder, create a new class file named *DbInitializer.cs* and replace the template code with the following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

<span data-ttu-id="e6b65-247">[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]</span><span class="sxs-lookup"><span data-stu-id="e6b65-247">[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]</span></span>

<span data-ttu-id="e6b65-248">代码检查是否有任何学生在数据库中，并且如果没有，它假定数据库是新，并且需要使用测试数据进行种子设定。</span><span class="sxs-lookup"><span data-stu-id="e6b65-248">The code checks if there are any students in the database, and if not, it assumes the database is new and needs to be seeded with test data.</span></span>  <span data-ttu-id="e6b65-249">它将测试数据加载到数组而不是`List<T>`集合来优化性能。</span><span class="sxs-lookup"><span data-stu-id="e6b65-249">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="e6b65-250">在*Program.cs*，修改`Main`方法来执行以下操作，在应用程序启动：</span><span class="sxs-lookup"><span data-stu-id="e6b65-250">In *Program.cs*, modify the `Main` method to do the following on application startup:</span></span>

* <span data-ttu-id="e6b65-251">从依赖关系注入容器中获取的数据库上下文实例。</span><span class="sxs-lookup"><span data-stu-id="e6b65-251">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="e6b65-252">调用 seed 方法，将上下文传递到它。</span><span class="sxs-lookup"><span data-stu-id="e6b65-252">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="e6b65-253">Seed 方法完成此操作，请释放上下文。</span><span class="sxs-lookup"><span data-stu-id="e6b65-253">Dispose the context when the seed method is done.</span></span>

<span data-ttu-id="e6b65-254">[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]</span><span class="sxs-lookup"><span data-stu-id="e6b65-254">[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]</span></span>

<span data-ttu-id="e6b65-255">添加`using`语句：</span><span class="sxs-lookup"><span data-stu-id="e6b65-255">Add `using` statements:</span></span>

<span data-ttu-id="e6b65-256">[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Usings)]</span><span class="sxs-lookup"><span data-stu-id="e6b65-256">[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Usings)]</span></span>

<span data-ttu-id="e6b65-257">在较旧教程中，你可能会看到中的类似代码`Configure`中的方法*Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="e6b65-257">In older tutorials, you may see similar code in the `Configure` method in *Startup.cs*.</span></span> <span data-ttu-id="e6b65-258">我们建议你使用`Configure`方法只是为了设置请求管道。</span><span class="sxs-lookup"><span data-stu-id="e6b65-258">We recommend that you use the `Configure` method only to set up the request pipeline.</span></span> <span data-ttu-id="e6b65-259">应用程序启动代码放入`Main`方法。</span><span class="sxs-lookup"><span data-stu-id="e6b65-259">Application startup code belongs in the `Main` method.</span></span>

<span data-ttu-id="e6b65-260">现在首次运行该应用程序，将数据库创建并使用测试数据设定种子。</span><span class="sxs-lookup"><span data-stu-id="e6b65-260">Now the first time you run the application, the database will be created and seeded with test data.</span></span> <span data-ttu-id="e6b65-261">每当你更改你的数据模型，你可以删除数据库、 更新你的 seed 方法，然后重新开始与新数据库相同的方式。</span><span class="sxs-lookup"><span data-stu-id="e6b65-261">Whenever you change your data model, you can delete the database, update your seed method, and start afresh with a new database the same way.</span></span> <span data-ttu-id="e6b65-262">在更高版本的教程中，你将了解如何在数据模型更改，而无需删除并重新创建它时修改数据库。</span><span class="sxs-lookup"><span data-stu-id="e6b65-262">In later tutorials, you'll see how to modify the database when the data model changes, without deleting and re-creating it.</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="e6b65-263">创建控制器和视图</span><span class="sxs-lookup"><span data-stu-id="e6b65-263">Create a controller and views</span></span>

<span data-ttu-id="e6b65-264">接下来，将使用基架引擎在 Visual Studio 中添加一个 MVC 控制器和将使用 EF 来查询和保存数据的视图。</span><span class="sxs-lookup"><span data-stu-id="e6b65-264">Next, you'll use the scaffolding engine in Visual Studio to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="e6b65-265">CRUD 操作方法和视图的自动创建被称为基架。</span><span class="sxs-lookup"><span data-stu-id="e6b65-265">The automatic creation of CRUD action methods and views is known as scaffolding.</span></span> <span data-ttu-id="e6b65-266">基架与不同从代码生成的基架的代码是一个起始点，您可以修改以满足自己需求，而你通常无需修改生成的代码。</span><span class="sxs-lookup"><span data-stu-id="e6b65-266">Scaffolding differs from code generation in that the scaffolded code is a starting point that you can modify to suit your own requirements, whereas you typically don't modify generated code.</span></span> <span data-ttu-id="e6b65-267">当你需要进行自定义生成的代码时，你使用分部类或发生更改时重新生成代码。</span><span class="sxs-lookup"><span data-stu-id="e6b65-267">When you need to customize generated code, you use partial classes or you regenerate the code when things change.</span></span>

* <span data-ttu-id="e6b65-268">右键单击**控制器**文件夹中的**解决方案资源管理器**和选择**添加 > 新建基架项**。</span><span class="sxs-lookup"><span data-stu-id="e6b65-268">Right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

* <span data-ttu-id="e6b65-269">在**添加 MVC 依赖项**对话框中，选择**最小依赖关系**，然后选择**添加**。</span><span class="sxs-lookup"><span data-stu-id="e6b65-269">In the **Add MVC Dependencies** dialog, select **Minimal Dependencies**, and select **Add**.</span></span>

  ![添加依赖项](intro/_static/add-depend.png)

  <span data-ttu-id="e6b65-271">Visual Studio 添加搭建基架控制器所需的依赖关系。</span><span class="sxs-lookup"><span data-stu-id="e6b65-271">Visual Studio adds the dependencies needed to scaffold a controller.</span></span> <span data-ttu-id="e6b65-272">项目文件中的唯一更改是添加`Microsoft.VisualStudio.Web.CodeGeneration.Design`包。</span><span class="sxs-lookup"><span data-stu-id="e6b65-272">The only change in the project file is the addition of the `Microsoft.VisualStudio.Web.CodeGeneration.Design` package.</span></span>

  <span data-ttu-id="e6b65-273">A *ScaffoldingReadMe.txt*这可以删除创建文件。</span><span class="sxs-lookup"><span data-stu-id="e6b65-273">A *ScaffoldingReadMe.txt* file is created which you can delete.</span></span>

* <span data-ttu-id="e6b65-274">同样，右键单击**控制器**文件夹中的**解决方案资源管理器**和选择**添加 > 新建基架项**。</span><span class="sxs-lookup"><span data-stu-id="e6b65-274">Once again, right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

* <span data-ttu-id="e6b65-275">在**添加基架**对话框中：</span><span class="sxs-lookup"><span data-stu-id="e6b65-275">In the **Add Scaffold** dialog box:</span></span>

  * <span data-ttu-id="e6b65-276">选择**使用实体框架的包含视图的 MVC 控制器**。</span><span class="sxs-lookup"><span data-stu-id="e6b65-276">Select **MVC controller with views, using Entity Framework**.</span></span>

  * <span data-ttu-id="e6b65-277">单击 **“添加”**。</span><span class="sxs-lookup"><span data-stu-id="e6b65-277">Click **Add**.</span></span>

* <span data-ttu-id="e6b65-278">在**添加控制器**对话框中：</span><span class="sxs-lookup"><span data-stu-id="e6b65-278">In the **Add Controller** dialog box:</span></span>

  * <span data-ttu-id="e6b65-279">在**模型类**选择**学生**。</span><span class="sxs-lookup"><span data-stu-id="e6b65-279">In **Model class** select **Student**.</span></span>

  * <span data-ttu-id="e6b65-280">在**数据上下文类**选择**SchoolContext**。</span><span class="sxs-lookup"><span data-stu-id="e6b65-280">In **Data context class** select **SchoolContext**.</span></span>

  * <span data-ttu-id="e6b65-281">接受默认值**StudentsController**作为名称。</span><span class="sxs-lookup"><span data-stu-id="e6b65-281">Accept the default **StudentsController** as the name.</span></span>

  * <span data-ttu-id="e6b65-282">单击 **“添加”**。</span><span class="sxs-lookup"><span data-stu-id="e6b65-282">Click **Add**.</span></span>

  ![基架学生](intro/_static/scaffold-student.png)

  <span data-ttu-id="e6b65-284">当你单击**添加**，Visual Studio 基架引擎创建*StudentsController.cs*文件和一组视图 (*.cshtml*文件) 这样与控制器起作用。</span><span class="sxs-lookup"><span data-stu-id="e6b65-284">When you click **Add**, the Visual Studio scaffolding engine creates a *StudentsController.cs* file and a set of views (*.cshtml* files) that work with the controller.</span></span>

<span data-ttu-id="e6b65-285">(基架引擎还可以创建的数据库上下文为你如果你不在手动创建如你此前在本教程第一次。</span><span class="sxs-lookup"><span data-stu-id="e6b65-285">(The scaffolding engine can also create the database context for you if you don't create it manually first as you did earlier for this tutorial.</span></span> <span data-ttu-id="e6b65-286">你可以指定在一个新上下文类**添加控制器**通过单击右侧的加号框**数据上下文类**。</span><span class="sxs-lookup"><span data-stu-id="e6b65-286">You can specify a new context class in the **Add Controller** box by clicking the plus sign to the right of **Data context class**.</span></span>  <span data-ttu-id="e6b65-287">然后，visual Studio 将创建你`DbContext`以及控制器和视图类。)</span><span class="sxs-lookup"><span data-stu-id="e6b65-287">Visual Studio will then create your `DbContext` class as well as the controller and views.)</span></span>

<span data-ttu-id="e6b65-288">你会注意到控制器采用`SchoolContext`作为构造函数参数。</span><span class="sxs-lookup"><span data-stu-id="e6b65-288">You'll notice that the controller takes a `SchoolContext` as a constructor parameter.</span></span>

<span data-ttu-id="e6b65-289">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]</span><span class="sxs-lookup"><span data-stu-id="e6b65-289">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]</span></span>

<span data-ttu-id="e6b65-290">ASP.NET 依赖关系注入将会负责处理传递的一个实例`SchoolContext`插入控制器。</span><span class="sxs-lookup"><span data-stu-id="e6b65-290">ASP.NET dependency injection will take care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="e6b65-291">配置，在*Startup.cs*前面文件。</span><span class="sxs-lookup"><span data-stu-id="e6b65-291">You configured that in the *Startup.cs* file earlier.</span></span>

<span data-ttu-id="e6b65-292">控制器包含`Index`显示数据库中的所有学生的操作方法。</span><span class="sxs-lookup"><span data-stu-id="e6b65-292">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="e6b65-293">该方法获取的学生列表的学生实体集通过读取从`Students`数据库上下文实例属性：</span><span class="sxs-lookup"><span data-stu-id="e6b65-293">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

<span data-ttu-id="e6b65-294">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="e6b65-294">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]</span></span>

<span data-ttu-id="e6b65-295">你将在本教程后面了解此代码中的异步编程元素。</span><span class="sxs-lookup"><span data-stu-id="e6b65-295">You'll learn about the asynchronous programming elements in this code later in the tutorial.</span></span>

<span data-ttu-id="e6b65-296">*Views/Students/Index.cshtml*视图在表中显示此列表：</span><span class="sxs-lookup"><span data-stu-id="e6b65-296">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="e6b65-297">按 CTRL + F5 来运行该项目或选择**调试 > 启动但不调试**从菜单。</span><span class="sxs-lookup"><span data-stu-id="e6b65-297">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="e6b65-298">单击学生选项卡以查看测试的数据，`DbInitializer.Initialize`插入的方法。</span><span class="sxs-lookup"><span data-stu-id="e6b65-298">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="e6b65-299">具体取决于如何窄浏览器窗口，你将看到`Student`选项卡链接在顶部的页或你将需要单击右上角才能看到该链接中的导航图标。</span><span class="sxs-lookup"><span data-stu-id="e6b65-299">Depending on how narrow your browser window is, you'll see the `Student` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![窄的 Contoso 大学主页](intro/_static/home-page-narrow.png)

![学生索引页](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="e6b65-302">查看数据库</span><span class="sxs-lookup"><span data-stu-id="e6b65-302">View the Database</span></span>

<span data-ttu-id="e6b65-303">当你启动了应用程序，`DbInitializer.Initialize`方法调用`EnsureCreated`。</span><span class="sxs-lookup"><span data-stu-id="e6b65-303">When you started the application, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="e6b65-304">EF 看到没有任何数据库，它创建一个，然后的其余部分`Initialize`方法代码填充数据的数据库。</span><span class="sxs-lookup"><span data-stu-id="e6b65-304">EF saw that there was no database and so it created one, then the remainder of the `Initialize` method code populated the database with data.</span></span> <span data-ttu-id="e6b65-305">你可以使用**SQL Server 对象资源管理器**(SSOX) 若要查看 Visual Studio 中的数据库。</span><span class="sxs-lookup"><span data-stu-id="e6b65-305">You can use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span>

<span data-ttu-id="e6b65-306">关闭浏览器。</span><span class="sxs-lookup"><span data-stu-id="e6b65-306">Close the browser.</span></span>

<span data-ttu-id="e6b65-307">如果 SSOX 窗口尚未打开，请选择它从**视图**Visual Studio 中的菜单。</span><span class="sxs-lookup"><span data-stu-id="e6b65-307">If the SSOX window isn't already open, select it from the **View** menu in Visual Studio.</span></span>

<span data-ttu-id="e6b65-308">在 SSOX 中，单击**(localdb) \MSSQLLocalDB > 数据库**，然后单击数据库名称中的连接字符串中的条目你*appsettings.json*文件。</span><span class="sxs-lookup"><span data-stu-id="e6b65-308">In SSOX, click **(localdb)\MSSQLLocalDB > Databases**, and then click the entry for the database name that is in the connection string in your *appsettings.json* file.</span></span>

<span data-ttu-id="e6b65-309">展开**表**节点以查看你的数据库中的表。</span><span class="sxs-lookup"><span data-stu-id="e6b65-309">Expand the **Tables** node to see the tables in your database.</span></span>

![在 SSOX 中的表](intro/_static/ssox-tables.png)

<span data-ttu-id="e6b65-311">右键单击**学生**表，然后单击**查看数据**若要查看已创建的列和已插入到表的行。</span><span class="sxs-lookup"><span data-stu-id="e6b65-311">Right-click the **Student** table and click **View Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

![在 SSOX 中学生表](intro/_static/ssox-student-table.png)

<span data-ttu-id="e6b65-313">*.Mdf*和*.ldf*数据库文件位于*C:\Users\<yourusername >*文件夹。</span><span class="sxs-lookup"><span data-stu-id="e6b65-313">The *.mdf* and *.ldf* database files are in the *C:\Users\<yourusername>* folder.</span></span>

<span data-ttu-id="e6b65-314">因为正在呼叫`EnsureCreated`的初始值设定项方法中，在启动应用程序上运行，你现在可以进行的更改`Student`类、 删除数据库、 运行一次，应用程序和数据库将自动重新创建，以匹配所做的更改。</span><span class="sxs-lookup"><span data-stu-id="e6b65-314">Because you're calling `EnsureCreated` in the initializer method that runs on app start, you could now make a change to the `Student` class, delete the database, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="e6b65-315">例如，如果你添加`EmailAddress`属性`Student`类，你将看到管理新`EmailAddress`重新创建表中的列。</span><span class="sxs-lookup"><span data-stu-id="e6b65-315">For example, if you add an `EmailAddress` property to the `Student` class, you'll see a new `EmailAddress` column in the re-created table.</span></span>

## <a name="conventions"></a><span data-ttu-id="e6b65-316">约定</span><span class="sxs-lookup"><span data-stu-id="e6b65-316">Conventions</span></span>

<span data-ttu-id="e6b65-317">你必须编写实体框架能够为你创建完整的数据库中的代码量很短，因为使用了约定，或者使实体框架的假设。</span><span class="sxs-lookup"><span data-stu-id="e6b65-317">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of conventions, or assumptions that the Entity Framework makes.</span></span>

* <span data-ttu-id="e6b65-318">名称`DbSet`属性用作表名。</span><span class="sxs-lookup"><span data-stu-id="e6b65-318">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="e6b65-319">未被引用的实体`DbSet`属性，实体类名称用作表名称。</span><span class="sxs-lookup"><span data-stu-id="e6b65-319">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="e6b65-320">实体属性名称用于列名称。</span><span class="sxs-lookup"><span data-stu-id="e6b65-320">Entity property names are used for column names.</span></span>

* <span data-ttu-id="e6b65-321">ID 或 classnameID 命名的实体属性被识别为主键属性。</span><span class="sxs-lookup"><span data-stu-id="e6b65-321">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="e6b65-322">如果它名为属性将被解释为外键属性* <navigation property name> <primary key property name> * (例如，`StudentID`为`Student`以来的导航属性`Student`实体的主键是`ID`).</span><span class="sxs-lookup"><span data-stu-id="e6b65-322">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="e6b65-323">此外可以只需命名外键属性* <primary key property name> * (例如，`EnrollmentID`由于`Enrollment`实体的主键是`EnrollmentID`)。</span><span class="sxs-lookup"><span data-stu-id="e6b65-323">Foreign key properties can also be named simply *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="e6b65-324">可以重写常规行为。</span><span class="sxs-lookup"><span data-stu-id="e6b65-324">Conventional behavior can be overridden.</span></span> <span data-ttu-id="e6b65-325">例如，你可以显式指定表名称，正如你此前在本教程中看到。</span><span class="sxs-lookup"><span data-stu-id="e6b65-325">For example, you can explicitly specify table names, as you saw earlier in this tutorial.</span></span> <span data-ttu-id="e6b65-326">你可以设置列名称并将任何属性设置为 primary key 或外键，正如在看到[后面的教程](complex-data-model.md)本系列。</span><span class="sxs-lookup"><span data-stu-id="e6b65-326">And you can set column names and set any property as primary key or foreign key, as you'll see in a [later tutorial](complex-data-model.md) in this series.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="e6b65-327">异步代码</span><span class="sxs-lookup"><span data-stu-id="e6b65-327">Asynchronous code</span></span>

<span data-ttu-id="e6b65-328">异步编程是 ASP.NET Core 和 EF 核心的默认模式。</span><span class="sxs-lookup"><span data-stu-id="e6b65-328">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="e6b65-329">Web 服务器具有有限的数量的线程可用，并且在高负载情况下的所有可用线程可能正在使用。</span><span class="sxs-lookup"><span data-stu-id="e6b65-329">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="e6b65-330">在这种情况，服务器无法处理新请求，直到线程会被释放。</span><span class="sxs-lookup"><span data-stu-id="e6b65-330">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="e6b65-331">与同步代码，多个线程可能会占用时它们实际上不执行任何操作，因为它们正在等待 I/O 完成。</span><span class="sxs-lookup"><span data-stu-id="e6b65-331">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="e6b65-332">与异步代码时进程正在等待 I/O 完成，将其线程被释放服务器用于处理其他请求。</span><span class="sxs-lookup"><span data-stu-id="e6b65-332">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="e6b65-333">因此，异步代码启用更有效地使用服务器资源，并启用该服务器以处理更多流量不会延迟。</span><span class="sxs-lookup"><span data-stu-id="e6b65-333">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="e6b65-334">异步代码在运行时，会引入的少量开销，但潜在的性能改善的性能影响可以忽略不计，时针对的高流量情况低流量情况下，较高。</span><span class="sxs-lookup"><span data-stu-id="e6b65-334">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="e6b65-335">在下面的代码中，`async`关键字，`Task<T>`返回值，`await`关键字，和`ToListAsync`方法使代码异步执行。</span><span class="sxs-lookup"><span data-stu-id="e6b65-335">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

<span data-ttu-id="e6b65-336">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]</span><span class="sxs-lookup"><span data-stu-id="e6b65-336">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]</span></span>

* <span data-ttu-id="e6b65-337">`async`关键字告知编译器生成的方法主体的部件的回调并自动创建`Task<IActionResult>`返回的对象。</span><span class="sxs-lookup"><span data-stu-id="e6b65-337">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that is returned.</span></span>

* <span data-ttu-id="e6b65-338">返回类型`Task<IActionResult>`表示正在进行的工作类型的结果`IActionResult`。</span><span class="sxs-lookup"><span data-stu-id="e6b65-338">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>

* <span data-ttu-id="e6b65-339">`await`关键字会导致编译器拆分为两个部分的方法。</span><span class="sxs-lookup"><span data-stu-id="e6b65-339">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="e6b65-340">第一部分结尾以异步方式启动该操作。</span><span class="sxs-lookup"><span data-stu-id="e6b65-340">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="e6b65-341">第二部分被放入操作完成时调用的回调方法。</span><span class="sxs-lookup"><span data-stu-id="e6b65-341">The second part is put into a callback method that is called when the operation completes.</span></span>

* <span data-ttu-id="e6b65-342">`ToListAsync`是的异步版本`ToList`扩展方法。</span><span class="sxs-lookup"><span data-stu-id="e6b65-342">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="e6b65-343">请注意当你编写异步代码的使用实体框架的一些事项：</span><span class="sxs-lookup"><span data-stu-id="e6b65-343">Some things to be aware of when you are writing asynchronous code that uses the Entity Framework:</span></span>

* <span data-ttu-id="e6b65-344">以异步方式执行导致查询或命令发送到数据库的语句。</span><span class="sxs-lookup"><span data-stu-id="e6b65-344">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="e6b65-345">这包括，例如， `ToListAsync`， `SingleOrDefaultAsync`，和`SaveChangesAsync`。</span><span class="sxs-lookup"><span data-stu-id="e6b65-345">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span>  <span data-ttu-id="e6b65-346">它不包括，例如，只需更改的语句`IQueryable`，如`var students = context.Students.Where(s => s.LastName == "Davolio")`。</span><span class="sxs-lookup"><span data-stu-id="e6b65-346">It does not include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="e6b65-347">EF 上下文不是线程安全： 请勿尝试执行并行的多个操作。</span><span class="sxs-lookup"><span data-stu-id="e6b65-347">An EF context is not thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="e6b65-348">当调用的任何异步 EF 方法时，始终使用`await`关键字。</span><span class="sxs-lookup"><span data-stu-id="e6b65-348">When you call any async EF method, always use the `await` keyword.</span></span>

* <span data-ttu-id="e6b65-349">如果你想要利用异步代码的性能优势，请确保任何库包，你仅使用 （例如，用于分页），还使用异步，如果它们调用任何导致查询发送到数据库的实体框架方法。</span><span class="sxs-lookup"><span data-stu-id="e6b65-349">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="e6b65-350">有关在.NET 中的异步编程的详细信息，请参阅[异步概述](https://docs.microsoft.com/dotnet/articles/standard/async)。</span><span class="sxs-lookup"><span data-stu-id="e6b65-350">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

## <a name="summary"></a><span data-ttu-id="e6b65-351">摘要</span><span class="sxs-lookup"><span data-stu-id="e6b65-351">Summary</span></span>

<span data-ttu-id="e6b65-352">你现在已创建的简单应用程序使用的实体框架核心和 SQL Server Express LocalDB 来存储和显示数据。</span><span class="sxs-lookup"><span data-stu-id="e6b65-352">You've now created a simple application that uses the Entity Framework Core and SQL Server Express LocalDB to store and display data.</span></span> <span data-ttu-id="e6b65-353">在以下教程中，你将学习如何执行基本的 CRUD （创建、 读取、 更新、 删除） 操作。</span><span class="sxs-lookup"><span data-stu-id="e6b65-353">In the following tutorial, you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e6b65-354">下一篇</span><span class="sxs-lookup"><span data-stu-id="e6b65-354">Next</span></span>](crud.md)  
