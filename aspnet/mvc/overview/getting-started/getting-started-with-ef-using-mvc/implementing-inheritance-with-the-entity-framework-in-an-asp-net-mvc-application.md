---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: 在 ASP.NET MVC 5 应用程序 (12 的 11) 中实现继承，并使用实体框架 6 |Microsoft 文档
author: tdykstra
description: Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 应用程序...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1826659626106993d4796641492c62fcbd22a1b3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2018
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a><span data-ttu-id="d1bac-103">在 ASP.NET MVC 5 应用程序 (11 12 的) 实现与 Entity Framework 6 的继承</span><span class="sxs-lookup"><span data-stu-id="d1bac-103">Implementing Inheritance with the Entity Framework 6 in an ASP.NET MVC 5 Application (11 of 12)</span></span>
====================
<span data-ttu-id="d1bac-104">通过[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d1bac-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d1bac-105">[下载已完成的项目](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下载 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="d1bac-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="d1bac-106">Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 应用程序。</span><span class="sxs-lookup"><span data-stu-id="d1bac-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="d1bac-107">若要了解系列教程，请参阅[本系列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="d1bac-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="d1bac-108">在前面的教程中，你将处理并发异常。</span><span class="sxs-lookup"><span data-stu-id="d1bac-108">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="d1bac-109">本教程将演示如何在数据模型中实现继承。</span><span class="sxs-lookup"><span data-stu-id="d1bac-109">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="d1bac-110">在面向对象编程中，你可以使用[继承](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming))便于[代码重用](http://en.wikipedia.org/wiki/Code_reuse)。</span><span class="sxs-lookup"><span data-stu-id="d1bac-110">In object-oriented programming, you can use [inheritance](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) to facilitate [code reuse](http://en.wikipedia.org/wiki/Code_reuse).</span></span> <span data-ttu-id="d1bac-111">在本教程中，将更改 `Instructor` 和 `Student` 类，以便从 `Person` 基类中派生，该基类包含教师和学生所共有的属性（如 `LastName`）。</span><span class="sxs-lookup"><span data-stu-id="d1bac-111">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="d1bac-112">不会添加或更改任何网页，但会更改部分代码，并将在数据库中自动反映这些更改。</span><span class="sxs-lookup"><span data-stu-id="d1bac-112">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="d1bac-113">将继承映射到数据库表的选项</span><span class="sxs-lookup"><span data-stu-id="d1bac-113">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="d1bac-114">`Instructor`和`Student`中的类`School`数据模型具有相同的多个属性：</span><span class="sxs-lookup"><span data-stu-id="d1bac-114">The `Instructor` and `Student` classes in the `School` data model have several properties that are identical:</span></span>

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="d1bac-116">假设想要消除由 `Instructor` 和`Student` 实体共享的属性的冗余代码。</span><span class="sxs-lookup"><span data-stu-id="d1bac-116">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="d1bac-117">或者想要写入可以格式化姓名的服务，而无需关注该姓名来自教师还是学生。</span><span class="sxs-lookup"><span data-stu-id="d1bac-117">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="d1bac-118">你可以创建`Person`基类其中仅包含那些共享属性，然后进行`Instructor`和`Student`实体继承自该基类，如下面的插图中所示：</span><span class="sxs-lookup"><span data-stu-id="d1bac-118">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="d1bac-120">有多种方法可以在数据库中表示此继承结构。</span><span class="sxs-lookup"><span data-stu-id="d1bac-120">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="d1bac-121">您可以对`Person`包括有关学生和教师单个表中的信息的表。</span><span class="sxs-lookup"><span data-stu-id="d1bac-121">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="d1bac-122">某些列可能仅适用于教师 (`HireDate`)，某些仅向学生 (`EnrollmentDate`)、 某些对这两个 (`LastName`， `FirstName`)。</span><span class="sxs-lookup"><span data-stu-id="d1bac-122">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="d1bac-123">通常，您将需要*鉴别器*指示哪种类型的每一行的列表示。</span><span class="sxs-lookup"><span data-stu-id="d1bac-123">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="d1bac-124">例如，鉴别器列可能包含“Instructor”来指示教师，包含“Student”来指示学生。</span><span class="sxs-lookup"><span data-stu-id="d1bac-124">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Table-per-hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="d1bac-126">从单个数据库表生成的实体继承结构的此模式称为*表每个层次结构*(TPH) 继承。</span><span class="sxs-lookup"><span data-stu-id="d1bac-126">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="d1bac-127">另一种方法是使数据库看起来更像继承结构。</span><span class="sxs-lookup"><span data-stu-id="d1bac-127">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="d1bac-128">例如，您可以对只有名称字段`Person`表，并具有单独`Instructor`和`Student`具有日期字段的表。</span><span class="sxs-lookup"><span data-stu-id="d1bac-128">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="d1bac-130">此模式的数据库表，针对每个实体类称为进行*每种类型的表*(TPT) 继承。</span><span class="sxs-lookup"><span data-stu-id="d1bac-130">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="d1bac-131">另一种方法是将所有非抽象类型映射到单独的表。</span><span class="sxs-lookup"><span data-stu-id="d1bac-131">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="d1bac-132">类的所有属性（包括继承的属性）映射到相应表的列。</span><span class="sxs-lookup"><span data-stu-id="d1bac-132">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="d1bac-133">此模式称为每个具体类一张表 (TPC) 继承。</span><span class="sxs-lookup"><span data-stu-id="d1bac-133">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="d1bac-134">如果实现的 TPC 继承`Person`， `Student`，和`Instructor`类更早版本，所示`Student`和`Instructor`表将与以前实现继承后的外观没有什么不同。</span><span class="sxs-lookup"><span data-stu-id="d1bac-134">If you implemented TPC inheritance for the `Person`, `Student`, and `Instructor` classes as shown earlier, the `Student` and `Instructor` tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="d1bac-135">TPC 和 TPH 继承模式通常提供更好的性能在实体框架中比 TPT 继承模式，因为 TPT 模式可能在复杂的联接查询中。</span><span class="sxs-lookup"><span data-stu-id="d1bac-135">TPC and TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="d1bac-136">本教程将演示如何实现 TPH 继承。</span><span class="sxs-lookup"><span data-stu-id="d1bac-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="d1bac-137">TPH 是实体框架中中的默认继承模式，因此你所要做的就是创建`Person`类中，更改`Instructor`和`Student`类派生自`Person`，添加到新的类`DbContext`，并创建迁移。</span><span class="sxs-lookup"><span data-stu-id="d1bac-137">TPH is the default inheritance pattern in the Entity Framework, so all you have to do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span> <span data-ttu-id="d1bac-138">(有关如何实现其他继承模式的信息，请参阅[映射的每种类型一个表 (TPT) 继承](https://msdn.microsoft.com/data/jj591617#2.5)和[映射表每个具体类 (TPC) 继承](https://msdn.microsoft.com/data/jj591617#2.6)MSDN 中实体框架文档。）</span><span class="sxs-lookup"><span data-stu-id="d1bac-138">(For information about how to implement the other inheritance patterns, see [Mapping the Table-Per-Type (TPT) Inheritance](https://msdn.microsoft.com/data/jj591617#2.5) and [Mapping the Table-Per-Concrete Class (TPC) Inheritance](https://msdn.microsoft.com/data/jj591617#2.6) in the MSDN Entity Framework documentation.)</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="d1bac-139">创建 Person 类</span><span class="sxs-lookup"><span data-stu-id="d1bac-139">Create the Person class</span></span>

<span data-ttu-id="d1bac-140">在*模型*文件夹中，创建*Person.cs*和模板代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="d1bac-140">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="d1bac-141">使 Student 和 Instructor 类从 Person 继承</span><span class="sxs-lookup"><span data-stu-id="d1bac-141">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="d1bac-142">在*Instructor.cs*，派生`Instructor`类`Person`类和删除密钥和名称字段。</span><span class="sxs-lookup"><span data-stu-id="d1bac-142">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="d1bac-143">代码将如下所示：</span><span class="sxs-lookup"><span data-stu-id="d1bac-143">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="d1bac-144">进行类似更改到*Student.cs*。</span><span class="sxs-lookup"><span data-stu-id="d1bac-144">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="d1bac-145">`Student`类将类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="d1bac-145">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a><span data-ttu-id="d1bac-146">将 Person 实体类型添加到模型</span><span class="sxs-lookup"><span data-stu-id="d1bac-146">Add the Person Entity Type to the Model</span></span>

<span data-ttu-id="d1bac-147">在*SchoolContext.cs*，添加`DbSet`属性`Person`实体类型：</span><span class="sxs-lookup"><span data-stu-id="d1bac-147">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="d1bac-148">以上是 Entity Framework 配置每个层次结构一张表继承所需的全部操作。</span><span class="sxs-lookup"><span data-stu-id="d1bac-148">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="d1bac-149">当你将看到更新数据库时，它将具有`Person`表代替了`Student`和`Instructor`表。</span><span class="sxs-lookup"><span data-stu-id="d1bac-149">As you'll see, when the database is updated, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="create-and-update-a-migrations-file"></a><span data-ttu-id="d1bac-150">创建和更新迁移文件</span><span class="sxs-lookup"><span data-stu-id="d1bac-150">Create and Update a Migrations File</span></span>

<span data-ttu-id="d1bac-151">在包管理器控制台 (PMC) 中，输入以下命令：</span><span class="sxs-lookup"><span data-stu-id="d1bac-151">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="d1bac-152">运行`Update-Database`PMC 命令。</span><span class="sxs-lookup"><span data-stu-id="d1bac-152">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="d1bac-153">由于我们有迁移不知道如何处理的现有数据，该命令将在此时失败。</span><span class="sxs-lookup"><span data-stu-id="d1bac-153">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="d1bac-154">您收到以下错误消息：</span><span class="sxs-lookup"><span data-stu-id="d1bac-154">You get an error message like the following one:</span></span>

> <span data-ttu-id="d1bac-155">*无法删除对象 dbo。教师因为 FOREIGN KEY 约束引用它。*</span><span class="sxs-lookup"><span data-stu-id="d1bac-155">*Could not drop object 'dbo.Instructor' because it is referenced by a FOREIGN KEY constraint.*</span></span>


<span data-ttu-id="d1bac-156">打开*迁移\&lt; 时间戳&gt;\_Inheritance.cs*和替换`Up`方法替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="d1bac-156">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="d1bac-157">此代码负责以下数据库更新任务：</span><span class="sxs-lookup"><span data-stu-id="d1bac-157">This code takes care of the following database update tasks:</span></span>

- <span data-ttu-id="d1bac-158">删除指向 Student 表的外键约束和索引。</span><span class="sxs-lookup"><span data-stu-id="d1bac-158">Removes foreign key constraints and indexes that point to the Student table.</span></span>
- <span data-ttu-id="d1bac-159">将 Instructor 表重命名为 Person，根据需要做出更改以存储学生数据：</span><span class="sxs-lookup"><span data-stu-id="d1bac-159">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

    - <span data-ttu-id="d1bac-160">为学生添加可为 NULL 的 EnrollmentDate。</span><span class="sxs-lookup"><span data-stu-id="d1bac-160">Adds nullable EnrollmentDate for students.</span></span>
    - <span data-ttu-id="d1bac-161">添加鉴别器列来指示行代表学生还是教师。</span><span class="sxs-lookup"><span data-stu-id="d1bac-161">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>
    - <span data-ttu-id="d1bac-162">HireDate 可为 NULL，因为学生行不会包含聘用日期。</span><span class="sxs-lookup"><span data-stu-id="d1bac-162">Makes HireDate nullable since student rows won't have hire dates.</span></span>
    - <span data-ttu-id="d1bac-163">添加临时字段，用于更新指向学生的外键。</span><span class="sxs-lookup"><span data-stu-id="d1bac-163">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="d1bac-164">当将学生复制到 Person 表他们将获得新的主键值。</span><span class="sxs-lookup"><span data-stu-id="d1bac-164">When you copy students into the Person table they'll get new primary key values.</span></span>
- <span data-ttu-id="d1bac-165">将数据从 Student 表复制到 Person 表。</span><span class="sxs-lookup"><span data-stu-id="d1bac-165">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="d1bac-166">这将使学生获取分配的新主键值。</span><span class="sxs-lookup"><span data-stu-id="d1bac-166">This causes students to get assigned new primary key values.</span></span>
- <span data-ttu-id="d1bac-167">修复指向学生的外键值。</span><span class="sxs-lookup"><span data-stu-id="d1bac-167">Fixes foreign key values that point to students.</span></span>
- <span data-ttu-id="d1bac-168">重新创建外键约束和索引，现在将它们指向 Person 表。</span><span class="sxs-lookup"><span data-stu-id="d1bac-168">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="d1bac-169">（如果已使用 GUID 而不是整数作为主键类型，那么将不需要更改学生主键值，并且可能已省略其中多个步骤。）</span><span class="sxs-lookup"><span data-stu-id="d1bac-169">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="d1bac-170">运行`update-database`试命令。</span><span class="sxs-lookup"><span data-stu-id="d1bac-170">Run the `update-database` command again.</span></span>

<span data-ttu-id="d1bac-171">（在生产系统中你将进行相应更改 Down 方法以防你曾必须使用，以返回到以前的数据库版本。</span><span class="sxs-lookup"><span data-stu-id="d1bac-171">(In a production system you would make corresponding changes to the Down method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="d1bac-172">本教程中你将不使用 Down 方法。）</span><span class="sxs-lookup"><span data-stu-id="d1bac-172">For this tutorial you won't be using the Down method.)</span></span>

> [!NOTE]
> <span data-ttu-id="d1bac-173">很可能会迁移数据和进行架构更改时获得其他错误。</span><span class="sxs-lookup"><span data-stu-id="d1bac-173">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="d1bac-174">如果您迁移错误无法解决，可以通过更改中的连接字符串来继续本教程*Web.config*文件或通过删除数据库。</span><span class="sxs-lookup"><span data-stu-id="d1bac-174">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or by deleting the database.</span></span> <span data-ttu-id="d1bac-175">最简单方法是在数据库重命名*Web.config*文件。</span><span class="sxs-lookup"><span data-stu-id="d1bac-175">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="d1bac-176">例如，将数据库名称更改为 ContosoUniversity2，如下面的示例中所示：</span><span class="sxs-lookup"><span data-stu-id="d1bac-176">For example, change the database name to ContosoUniversity2 as shown in the following example:</span></span>
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> <span data-ttu-id="d1bac-177">与新数据库，没有数据迁移，和`update-database`命令是更可能完成且未发生错误。</span><span class="sxs-lookup"><span data-stu-id="d1bac-177">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="d1bac-178">有关如何删除数据库的说明，请参阅[如何从 Visual Studio 2012 中删除数据库](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。</span><span class="sxs-lookup"><span data-stu-id="d1bac-178">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="d1bac-179">如果你接受这种方法，以继续本教程，请跳过部署步骤，在本教程末尾，或将部署到新站点和数据库。</span><span class="sxs-lookup"><span data-stu-id="d1bac-179">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial or deploy to a new site and database.</span></span> <span data-ttu-id="d1bac-180">如果将更新部署到您已部署到已在同一站点时，EF 会那里相同的错误时它自动运行迁移。</span><span class="sxs-lookup"><span data-stu-id="d1bac-180">If you deploy an update to the same site you've been deploying to already, EF will get the same error there when it runs migrations automatically.</span></span> <span data-ttu-id="d1bac-181">如果你想要迁移错误的排查，最佳资源是实体框架论坛或 StackOverflow.com 之一。</span><span class="sxs-lookup"><span data-stu-id="d1bac-181">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>


## <a name="testing"></a><span data-ttu-id="d1bac-182">正在测试</span><span class="sxs-lookup"><span data-stu-id="d1bac-182">Testing</span></span>

<span data-ttu-id="d1bac-183">运行站点并尝试各种页面。</span><span class="sxs-lookup"><span data-stu-id="d1bac-183">Run the site and try various pages.</span></span> <span data-ttu-id="d1bac-184">一切都和以前一样。</span><span class="sxs-lookup"><span data-stu-id="d1bac-184">Everything works the same as it did before.</span></span>

<span data-ttu-id="d1bac-185">在**服务器资源管理器，**展开**数据 Connections\SchoolContext**然后**表**，和你看到**学生**和**教师**已替换为表**人员**表。</span><span class="sxs-lookup"><span data-stu-id="d1bac-185">In **Server Explorer,** expand **Data Connections\SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="d1bac-186">展开**人员**表，你会看到它具有所有使用中的列**学生**和**教师**表。</span><span class="sxs-lookup"><span data-stu-id="d1bac-186">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="d1bac-188">右键单击 Person 表，然后单击“显示表数据”以查看鉴别器列。</span><span class="sxs-lookup"><span data-stu-id="d1bac-188">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="d1bac-189">下图说明了新的 School 数据库的结构：</span><span class="sxs-lookup"><span data-stu-id="d1bac-189">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="d1bac-191">将部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="d1bac-191">Deploy to Azure</span></span>

<span data-ttu-id="d1bac-192">此部分要求你已完成可选**将应用部署到 Azure**主题中[第 3 部分，排序、 筛选和分页](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)本系列教程。</span><span class="sxs-lookup"><span data-stu-id="d1bac-192">This section requires you to have completed the optional **Deploying the app to Azure** section in [Part 3, Sorting, Filtering, and Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) of this tutorial series.</span></span> <span data-ttu-id="d1bac-193">如果必须解决通过删除本地项目中的数据库的迁移错误，跳过此步骤;或创建新站点和数据库，并部署到新的环境。</span><span class="sxs-lookup"><span data-stu-id="d1bac-193">If you had migrations errors that you resolved by deleting the database in your local project, skip this step; or create a new site and database, and deploy to the new environment.</span></span>

1. <span data-ttu-id="d1bac-194">在 Visual Studio 中，右键单击中的项目**解决方案资源管理器**和选择**发布**从上下文菜单。</span><span class="sxs-lookup"><span data-stu-id="d1bac-194">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>  
  
    ![在项目上下文菜单中发布](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. <span data-ttu-id="d1bac-196">单击“发布” 。</span><span class="sxs-lookup"><span data-stu-id="d1bac-196">Click **Publish**.</span></span>  
  
    ![publish](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
   <span data-ttu-id="d1bac-198">Web 应用将在默认浏览器中打开。</span><span class="sxs-lookup"><span data-stu-id="d1bac-198">The Web app will open in your default browser.</span></span>
3. <span data-ttu-id="d1bac-199">测试应用程序以验证它是否正常工作。</span><span class="sxs-lookup"><span data-stu-id="d1bac-199">Test the application to verify it's working.</span></span>

    <span data-ttu-id="d1bac-200">第一次运行某个页面访问数据库，实体框架将运行所有迁移`Up`使数据库保持最新的当前数据模型所需的方法。</span><span class="sxs-lookup"><span data-stu-id="d1bac-200">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span>

## <a name="summary"></a><span data-ttu-id="d1bac-201">总结</span><span class="sxs-lookup"><span data-stu-id="d1bac-201">Summary</span></span>

<span data-ttu-id="d1bac-202">你已经为 `Person`、`Student` 和 `Instructor` 类实现了每个层次结构一张表继承。</span><span class="sxs-lookup"><span data-stu-id="d1bac-202">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="d1bac-203">有关此设置和其他的继承结构的详细信息，请参阅[TPT 继承模式](https://msdn.microsoft.com/data/jj618293)和[TPH 继承模式](https://msdn.microsoft.com/data/jj618292)MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="d1bac-203">For more information about this and other inheritance structures, see [TPT Inheritance Pattern](https://msdn.microsoft.com/data/jj618293) and [TPH Inheritance Pattern](https://msdn.microsoft.com/data/jj618292) on MSDN.</span></span> <span data-ttu-id="d1bac-204">下一个教程将介绍如何处理各种相对高级的 Entity Framework 方案。</span><span class="sxs-lookup"><span data-stu-id="d1bac-204">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

<span data-ttu-id="d1bac-205">在找不到其他实体框架资源的链接[ASP.NET 数据访问的推荐资源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="d1bac-205">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d1bac-206">[上一页](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一页](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="d1bac-206">[Previous](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)</span></span>
