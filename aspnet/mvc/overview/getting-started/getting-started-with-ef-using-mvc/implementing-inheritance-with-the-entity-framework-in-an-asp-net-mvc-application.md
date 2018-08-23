---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: 在 ASP.NET MVC 5 应用程序 (12 的 11) 中实现继承使用 Entity Framework 6 |Microsoft Docs
author: tdykstra
description: Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 的 ASP.NET MVC 5 应用程序...
ms.author: riande
ms.date: 11/07/2014
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9bfe860761a18cb4a69d33336d64a169e3c834f9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834675"
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>在 ASP.NET MVC 5 应用程序 (12 的 11) 中实现继承，使用实体框架 6
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载已完成的项目](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下载 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 应用程序。 若要了解系列教程，请参阅[本系列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


在上一教程中，你将处理并发异常。 本教程将演示如何在数据模型中实现继承。

在面向对象的编程中，可以使用[继承](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming))便于[代码重用](http://en.wikipedia.org/wiki/Code_reuse)。 在本教程中，将更改 `Instructor` 和 `Student` 类，以便从 `Person` 基类中派生，该基类包含教师和学生所共有的属性（如 `LastName`）。 不会添加或更改任何网页，但会更改部分代码，并将在数据库中自动反映这些更改。

## <a name="options-for-mapping-inheritance-to-database-tables"></a>将继承映射到数据库表的选项

`Instructor`并`Student`中的类`School`数据模型具有完全相同的多个属性：

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

假设想要消除由 `Instructor` 和`Student` 实体共享的属性的冗余代码。 或者想要写入可以格式化姓名的服务，而无需关注该姓名来自教师还是学生。 您可以创建`Person`基类其中只包含这些共享属性，请`Instructor`和`Student`实体继承自该基类，如以下插图所示：

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

有多种方法可以在数据库中表示此继承结构。 您可以`Person`包括有关学生和教师单个表中的信息的表。 某些列可能仅适用于教师 (`HireDate`)，一些仅面向学生 (`EnrollmentDate`)、 一些对这两个 (`LastName`， `FirstName`)。 通常情况下，您必须*鉴别器*指示哪种类型的每一行的列表示。 例如，鉴别器列可能包含“Instructor”来指示教师，包含“Student”来指示学生。

![每个 hierarchy_example 表](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

从单个数据库表生成实体继承结构的此模式称为*每个层次结构一个表*(TPH) 继承。

另一种方法是使数据库看起来更像继承结构。 例如，您可以仅将姓名字段`Person`表，并具有单独`Instructor`和`Student`具有日期字段的表。

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

此模式的数据库表，针对每个实体的类称为进行*每种类型的表*(TPT) 继承。

另一种方法是将所有非抽象类型映射到单独的表。 类的所有属性（包括继承的属性）映射到相应表的列。 此模式称为每个具体类一张表 (TPC) 继承。 如果实现了 TPC 继承`Person`， `Student`，并`Instructor`类如前文所述，`Student`和`Instructor`表看在比以前一样实现继承之后没有什么不同。

TPC 和 TPH 继承模式通常提供更好的性能在实体框架中比 TPT 继承模式，因为 TPT 模式会导致复杂的联接查询。

本教程将演示如何实现 TPH 继承。 TPH 是 Entity Framework 中的默认继承模式，因此您需要做的就是创建`Person`类中，更改`Instructor`并`Student`类继承`Person`，将添加到新的类`DbContext`，并创建迁移。 (有关如何实现其他继承模式的信息，请参阅[映射的每种类型一个表 (TPT) 继承](https://msdn.microsoft.com/data/jj591617#2.5)并[映射表每个具体类 (TPC) 继承](https://msdn.microsoft.com/data/jj591617#2.6)MSDN 中Entity Framework 文档。）

## <a name="create-the-person-class"></a>创建 Person 类

在中*模型*文件夹中，创建*Person.cs*和模板代码替换为以下代码：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>使 Student 和 Instructor 类从 Person 继承

在中*Instructor.cs*，派生`Instructor`类`Person`类，并删除键和姓名字段。 代码将如下所示：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

进行到类似的更改*Student.cs*。 `Student`类看起来如下例所示：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>将 Person 实体类型添加到模型

在中*SchoolContext.cs*，添加`DbSet`属性`Person`实体类型：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

以上是 Entity Framework 配置每个层次结构一张表继承所需的全部操作。 正如您将看到数据库更新时，它将具有`Person`表来代替`Student`和`Instructor`表。

## <a name="create-and-update-a-migrations-file"></a>创建和更新的迁移文件

在包管理器控制台 (PMC) 中，输入以下命令：

`Add-Migration Inheritance`

运行`Update-Database`PMC 命令。 该命令将在这里失败，因为我们已将迁移并不知道如何处理的现有数据。 获取一条错误消息如下：

> *无法删除对象 dbo。讲师因为它引用由 FOREIGN KEY 约束。*


打开*迁移\&l t; 时间戳&gt;\_Inheritance.cs* ，并将为`Up`方法使用以下代码：

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

此代码负责以下数据库更新任务：

- 删除指向 Student 表的外键约束和索引。
- 将 Instructor 表重命名为 Person，根据需要做出更改以存储学生数据：

    - 为学生添加可为 NULL 的 EnrollmentDate。
    - 添加鉴别器列来指示行代表学生还是教师。
    - HireDate 可为 NULL，因为学生行不会包含聘用日期。
    - 添加临时字段，用于更新指向学生的外键。 将学生复制到 Person 表时，他们将获得新的主键值。
- 将数据从 Student 表复制到 Person 表。 这将使学生获取分配的新主键值。
- 修复指向学生的外键值。
- 重新创建外键约束和索引，现在将它们指向 Person 表。

（如果已使用 GUID 而不是整数作为主键类型，那么将不需要更改学生主键值，并且可能已省略其中多个步骤。）

运行`update-database`试命令。

（在生产系统中您将更改相应 Down 方法以防您曾经不得不使用，请返回到以前的数据库版本。 本教程中你不会使用 Down 方法。）

> [!NOTE]
> 就可以将迁移数据，也使架构更改时遇到其他错误。 如果获取的迁移错误则不能解决，可以通过更改中的连接字符串来继续本教程*Web.config*文件或删除数据库。 最简单方法是在数据库重命名*Web.config*文件。 例如，为 ContosoUniversity2 更改数据库名称，如下面的示例中所示：
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> 使用新数据库没有数据迁移，和`update-database`命令是更有望完成且未出错。 有关如何删除数据库的说明，请参阅[如何从 Visual Studio 2012 中删除数据库](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)。 如果你接受这种方法，以继续本教程，请跳过部署步骤，本教程结束时或部署到新的站点和数据库。 如果将更新部署到您已被部署到已在同一站点时，EF 会那里相同的错误时自动运行迁移。 如果你想要解决的迁移错误，最佳资源是 Entity Framework 论坛或 StackOverflow.com 之一。


## <a name="testing"></a>正在测试

运行站点，并尝试各种页面。 一切都和以前一样。

中**服务器资源管理器**展开**数据 Connections\SchoolContext** ，然后**表**，并可以看到**学生**和**讲师**已被取代，表**人员**表。 展开**Person**表，会看到它具有所有使用中的列**学生**并**讲师**表。

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

右键单击 Person 表，然后单击“显示表数据”以查看鉴别器列。

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

下图说明了新的 School 数据库的结构：

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>部署到 Azure

该部分要求你已完成的可选**将其部署到 Azure**主题中[第 3 部分，执行排序、 筛选和分页](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)本系列教程。 如果必须通过删除在本地项目中的数据库解决的迁移错误，请跳过此步骤;或创建新的站点和数据库，并将部署到新环境。

1. 在 Visual Studio 中，右键单击该项目中的**解决方案资源管理器**，然后选择**发布**从上下文菜单。  
  
    ![在项目上下文菜单中发布](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. 单击“发布” 。  
  
    ![publish](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
   Web 应用将在默认浏览器中打开。
3. 测试应用程序以验证它是否正常工作。

    首次运行页面访问数据库，Entity Framework 将运行所有迁移`Up`使数据库保持最新的当前数据模型所必需的方法。

## <a name="summary"></a>总结

你已经为 `Person`、`Student` 和 `Instructor` 类实现了每个层次结构一张表继承。 有关此设置和其他继承结构的详细信息，请参阅[TPT 继承模式](https://msdn.microsoft.com/data/jj618293)并[TPH 继承模式](https://msdn.microsoft.com/data/jj618292)MSDN 上。 下一个教程将介绍如何处理各种相对高级的 Entity Framework 方案。

其他实体框架资源的链接可在[ASP.NET 数据访问-推荐的资源](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [上一页](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一页](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
