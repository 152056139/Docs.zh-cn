---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: "处理与实体框架中的 ASP.NET MVC 应用程序 (7 个 10) 的并发 |Microsoft 文档"
author: tdykstra
description: "Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b072134043ceda809bfeca98447a132ed407b323
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a><span data-ttu-id="dcb00-103">处理与实体框架中的 ASP.NET MVC 应用程序 (7 个 10) 的并发</span><span class="sxs-lookup"><span data-stu-id="dcb00-103">Handling Concurrency with the Entity Framework in an ASP.NET MVC Application (7 of 10)</span></span>
====================
<span data-ttu-id="dcb00-104">通过[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="dcb00-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="dcb00-105">下载已完成的项目</span><span class="sxs-lookup"><span data-stu-id="dcb00-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="dcb00-106">Contoso 大学示例 web 应用程序演示如何创建使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 应用程序。</span><span class="sxs-lookup"><span data-stu-id="dcb00-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="dcb00-107">有关教程系列的信息，请参阅[序列中的第一个教程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="dcb00-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="dcb00-108">你可以从头开始教程系列或[下载这一章的初学者项目](building-the-ef5-mvc4-chapter-downloads.md)和从这里开始。</span><span class="sxs-lookup"><span data-stu-id="dcb00-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="dcb00-109">如果你遇到无法解决的问题[下载已完成的章](building-the-ef5-mvc4-chapter-downloads.md)并尝试重现你的问题。</span><span class="sxs-lookup"><span data-stu-id="dcb00-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="dcb00-110">你通常可以通过比较你的代码已完成的代码会发现问题的解决方案。</span><span class="sxs-lookup"><span data-stu-id="dcb00-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="dcb00-111">一些常见的错误和如何解决它们，请参阅[错误和解决方法。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="dcb00-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="dcb00-112">在前面的两个教程中，你处理相关的数据。</span><span class="sxs-lookup"><span data-stu-id="dcb00-112">In the previous two tutorials you worked with related data.</span></span> <span data-ttu-id="dcb00-113">本教程演示如何处理并发。</span><span class="sxs-lookup"><span data-stu-id="dcb00-113">This tutorial shows how to handle concurrency.</span></span> <span data-ttu-id="dcb00-114">你将创建使用的 web 页`Department`实体，并编辑和删除的页面`Department`实体将处理并发错误。</span><span class="sxs-lookup"><span data-stu-id="dcb00-114">You'll create web pages that work with the `Department` entity, and the pages that edit and delete `Department` entities will handle concurrency errors.</span></span> <span data-ttu-id="dcb00-115">下图显示的索引和删除页，包括某些并发冲突发生时显示的消息。</span><span class="sxs-lookup"><span data-stu-id="dcb00-115">The following illustrations show the Index and Delete pages, including some messages that are displayed if a concurrency conflict occurs.</span></span>

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a><span data-ttu-id="dcb00-118">并发冲突</span><span class="sxs-lookup"><span data-stu-id="dcb00-118">Concurrency Conflicts</span></span>

<span data-ttu-id="dcb00-119">一个用户才能进行编辑，显示实体的数据，并在第一个用户的更改写入到数据库之前，另一个用户然后更新相同实体的数据时发生并发冲突。</span><span class="sxs-lookup"><span data-stu-id="dcb00-119">A concurrency conflict occurs when one user displays an entity's data in order to edit it, and then another user updates the same entity's data before the first user's change is written to the database.</span></span> <span data-ttu-id="dcb00-120">如果不启用此类冲突检测，任何人都更新数据库上次将覆盖其他用户的更改。</span><span class="sxs-lookup"><span data-stu-id="dcb00-120">If you don't enable the detection of such conflicts, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="dcb00-121">在许多应用程序，这种风险很可接受： 如果有很多用户或几个更新，或如果并不真正重要的一些更改将被覆盖，如果并发编程的开销可能超过带来的好处。</span><span class="sxs-lookup"><span data-stu-id="dcb00-121">In many applications, this risk is acceptable: if there are few users, or few updates, or if isn't really critical if some changes are overwritten, the cost of programming for concurrency might outweigh the benefit.</span></span> <span data-ttu-id="dcb00-122">在这种情况下，你不必配置此应用程序处理并发冲突。</span><span class="sxs-lookup"><span data-stu-id="dcb00-122">In that case, you don't have to configure the application to handle concurrency conflicts.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="dcb00-123">保守式并发 （锁定）</span><span class="sxs-lookup"><span data-stu-id="dcb00-123">Pessimistic Concurrency (Locking)</span></span>

<span data-ttu-id="dcb00-124">如果你的应用程序需要防止并发方案中的意外数据丢失，做到这一点的一种方法是使用数据库锁。</span><span class="sxs-lookup"><span data-stu-id="dcb00-124">If your application does need to prevent accidental data loss in concurrency scenarios, one way to do that is to use database locks.</span></span> <span data-ttu-id="dcb00-125">这称为*保守式并发*。</span><span class="sxs-lookup"><span data-stu-id="dcb00-125">This is called *pessimistic concurrency*.</span></span> <span data-ttu-id="dcb00-126">例如，从数据库读取行之前，你请求的锁只读的或更新访问权限。</span><span class="sxs-lookup"><span data-stu-id="dcb00-126">For example, before you read a row from a database, you request a lock for read-only or for update access.</span></span> <span data-ttu-id="dcb00-127">如果锁定的行更新访问权限时，不允许任何其他用户锁定该行为只读的或更新访问权限，因为它们将获取正在进行更改的数据的副本。</span><span class="sxs-lookup"><span data-stu-id="dcb00-127">If you lock a row for update access, no other users are allowed to lock the row either for read-only or update access, because they would get a copy of data that's in the process of being changed.</span></span> <span data-ttu-id="dcb00-128">如果锁定用于只读访问的行，其他人可以还将其锁定用于只读访问权限但不是用于更新。</span><span class="sxs-lookup"><span data-stu-id="dcb00-128">If you lock a row for read-only access, others can also lock it for read-only access but not for update.</span></span>

<span data-ttu-id="dcb00-129">管理锁也有缺点。</span><span class="sxs-lookup"><span data-stu-id="dcb00-129">Managing locks has disadvantages.</span></span> <span data-ttu-id="dcb00-130">它可以是复杂到程序。</span><span class="sxs-lookup"><span data-stu-id="dcb00-130">It can be complex to program.</span></span> <span data-ttu-id="dcb00-131">它需要大量的数据库管理资源，并且可能导致性能问题的应用程序的用户数会增加 （即，它不能很好）。</span><span class="sxs-lookup"><span data-stu-id="dcb00-131">It requires significant database management resources, and it can cause performance problems as the number of users of an application increases (that is, it doesn't scale well).</span></span> <span data-ttu-id="dcb00-132">出于这些原因，不是所有数据库管理系统都支持保守式并发。</span><span class="sxs-lookup"><span data-stu-id="dcb00-132">For these reasons, not all database management systems support pessimistic concurrency.</span></span> <span data-ttu-id="dcb00-133">本教程不向你展示如何实现它，并且实体框架提供，没有内置支持。</span><span class="sxs-lookup"><span data-stu-id="dcb00-133">The Entity Framework provides no built-in support for it, and this tutorial doesn't show you how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="dcb00-134">开放式并发</span><span class="sxs-lookup"><span data-stu-id="dcb00-134">Optimistic Concurrency</span></span>

<span data-ttu-id="dcb00-135">保守式并发替代方法是*开放式并发*。</span><span class="sxs-lookup"><span data-stu-id="dcb00-135">The alternative to pessimistic concurrency is *optimistic concurrency*.</span></span> <span data-ttu-id="dcb00-136">开放式并发意味着允许并发冲突发生，然后相应地反应，如果他们这样做。</span><span class="sxs-lookup"><span data-stu-id="dcb00-136">Optimistic concurrency means allowing concurrency conflicts to happen, and then reacting appropriately if they do.</span></span> <span data-ttu-id="dcb00-137">例如，John 运行部门编辑页上，更改**预算**量从 $350,000.00 0.00 美元到英语的部门。</span><span class="sxs-lookup"><span data-stu-id="dcb00-137">For example, John runs the Departments Edit page, changes the **Budget** amount for the English department from $350,000.00 to $0.00.</span></span>

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="dcb00-139">John 单击之前**保存**，Jane 运行相同的页和更改**开始日期**字段从 2007 年 9 月 1 日到 2013 年 8 月 8 日。</span><span class="sxs-lookup"><span data-stu-id="dcb00-139">Before John clicks **Save**, Jane runs the same page and changes the **Start Date** field from 9/1/2007 to 8/8/2013.</span></span>

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="dcb00-141">John 单击**保存**第一个并查看他的更改时浏览器返回到索引页面，然后 Jane 单击**保存**。</span><span class="sxs-lookup"><span data-stu-id="dcb00-141">John clicks **Save** first and sees his change when the browser returns to the Index page, then Jane clicks **Save**.</span></span> <span data-ttu-id="dcb00-142">接下来如何操作取决于如何处理并发冲突。</span><span class="sxs-lookup"><span data-stu-id="dcb00-142">What happens next is determined by how you handle concurrency conflicts.</span></span> <span data-ttu-id="dcb00-143">某些选项包括：</span><span class="sxs-lookup"><span data-stu-id="dcb00-143">Some of the options include the following:</span></span>

- <span data-ttu-id="dcb00-144">你可以跟踪的用户进行了修改的属性，并更新仅在数据库中的相应列。</span><span class="sxs-lookup"><span data-stu-id="dcb00-144">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span> <span data-ttu-id="dcb00-145">在示例方案中，任何数据将不会丢失，因为不同的属性已更新由两个用户。</span><span class="sxs-lookup"><span data-stu-id="dcb00-145">In the example scenario, no data would be lost, because different properties were updated by the two users.</span></span> <span data-ttu-id="dcb00-146">下一次有人浏览英语部门时，他们将看到 John 的和 Jane 的更改 — 2013 年 8 月 8 日开始日期和零美元的预算。</span><span class="sxs-lookup"><span data-stu-id="dcb00-146">The next time someone browses the English department, they'll see both John's and Jane's changes — a start date of 8/8/2013 and a budget of Zero dollars.</span></span>

    <span data-ttu-id="dcb00-147">这种更新方法可以减少可能导致数据丢失的冲突数，但它无法避免数据丢失，如果实体的同一属性进行竞争的更改。</span><span class="sxs-lookup"><span data-stu-id="dcb00-147">This method of updating can reduce the number of conflicts that could result in data loss, but it can't avoid data loss if competing changes are made to the same property of an entity.</span></span> <span data-ttu-id="dcb00-148">实体框架是否可以通过这种方式取决于如何实现你更新代码。</span><span class="sxs-lookup"><span data-stu-id="dcb00-148">Whether the Entity Framework works this way depends on how you implement your update code.</span></span> <span data-ttu-id="dcb00-149">它通常是状态的不现实中 web 应用程序，因为它可能需要维护大量，以便跟踪的实体的所有原始属性值以及新值。</span><span class="sxs-lookup"><span data-stu-id="dcb00-149">It's often not practical in a web application, because it can require that you maintain large amounts of state in order to keep track of all original property values for an entity as well as new values.</span></span> <span data-ttu-id="dcb00-150">因为它需要服务器资源或必须包括 （例如，在隐藏的字段） 的 web 页本身中，保留大量的状态可能会影响应用程序性能。</span><span class="sxs-lookup"><span data-stu-id="dcb00-150">Maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself (for example, in hidden fields).</span></span>
- <span data-ttu-id="dcb00-151">你可以让 Jane 的更改覆盖 John 的更改。</span><span class="sxs-lookup"><span data-stu-id="dcb00-151">You can let Jane's change overwrite John's change.</span></span> <span data-ttu-id="dcb00-152">下一次有人浏览英语部门时，他们将看到 2013 年 8 月 8 日和还原的 $350,000.00 值。</span><span class="sxs-lookup"><span data-stu-id="dcb00-152">The next time someone browses the English department, they'll see 8/8/2013 and the restored $350,000.00 value.</span></span> <span data-ttu-id="dcb00-153">这称为*客户端优先*或*在 Wins 中最后一个*方案。</span><span class="sxs-lookup"><span data-stu-id="dcb00-153">This is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="dcb00-154">（客户端的值优先于什么是在数据存储。）中所述的简介本部分中，如果您不执行任何并发处理的编码，这将自动发生。</span><span class="sxs-lookup"><span data-stu-id="dcb00-154">(The client's values take precedence over what's in the data store.) As noted in the introduction to this section, if you don't do any coding for concurrency handling, this will happen automatically.</span></span>
- <span data-ttu-id="dcb00-155">你可以防止在数据库中正在更新 Jane 的更改。</span><span class="sxs-lookup"><span data-stu-id="dcb00-155">You can prevent Jane's change from being updated in the database.</span></span> <span data-ttu-id="dcb00-156">通常情况下，将显示一条错误消息、 她显示的数据的当前状态和让她可以如果她仍然想要对它们进行重新应用其更改。</span><span class="sxs-lookup"><span data-stu-id="dcb00-156">Typically, you would display an error message, show her the current state of the data, and allow her to reapply her changes if she still wants to make them.</span></span> <span data-ttu-id="dcb00-157">这称为*存储 Wins*方案。</span><span class="sxs-lookup"><span data-stu-id="dcb00-157">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="dcb00-158">（数据存储值优先于提交的客户端的值。）在本教程中，你将实现存储 Wins 方案。</span><span class="sxs-lookup"><span data-stu-id="dcb00-158">(The data-store values take precedence over the values submitted by the client.) You'll implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="dcb00-159">此方法可确保到发生了什么情况收到通知用户的情况下，任何更改都会被覆盖。</span><span class="sxs-lookup"><span data-stu-id="dcb00-159">This method ensures that no changes are overwritten without a user being alerted to what's happening.</span></span>

### <a name="detecting-concurrency-conflicts"></a><span data-ttu-id="dcb00-160">检测并发冲突</span><span class="sxs-lookup"><span data-stu-id="dcb00-160">Detecting Concurrency Conflicts</span></span>

<span data-ttu-id="dcb00-161">您可以通过处理中解决冲突[OptimisticConcurrencyException](https://msdn.microsoft.com/en-us/library/system.data.optimisticconcurrencyexception.aspx)实体框架引发的异常。</span><span class="sxs-lookup"><span data-stu-id="dcb00-161">You can resolve conflicts by handling [OptimisticConcurrencyException](https://msdn.microsoft.com/en-us/library/system.data.optimisticconcurrencyexception.aspx) exceptions that the Entity Framework throws.</span></span> <span data-ttu-id="dcb00-162">为了知道何时引发这些异常，实体框架必须能够检测到冲突。</span><span class="sxs-lookup"><span data-stu-id="dcb00-162">In order to know when to throw these exceptions, the Entity Framework must be able to detect conflicts.</span></span> <span data-ttu-id="dcb00-163">因此，你必须从数据库和数据模型正确配置。</span><span class="sxs-lookup"><span data-stu-id="dcb00-163">Therefore, you must configure the database and the data model appropriately.</span></span> <span data-ttu-id="dcb00-164">有关启用冲突检测某些选项包括：</span><span class="sxs-lookup"><span data-stu-id="dcb00-164">Some options for enabling conflict detection include the following:</span></span>

- <span data-ttu-id="dcb00-165">在数据库表中，包括可以用于确定当行已更改的跟踪列。</span><span class="sxs-lookup"><span data-stu-id="dcb00-165">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span> <span data-ttu-id="dcb00-166">然后，你可以配置实体框架可以包括中的列`Where`的 SQL 子句`Update`或`Delete`命令。</span><span class="sxs-lookup"><span data-stu-id="dcb00-166">You can then configure the Entity Framework to include that column in the `Where` clause of SQL `Update` or `Delete` commands.</span></span>

    <span data-ttu-id="dcb00-167">跟踪列的数据类型通常是[rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx)。</span><span class="sxs-lookup"><span data-stu-id="dcb00-167">The data type of the tracking column is typically [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx).</span></span> <span data-ttu-id="dcb00-168">[Rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx)值是每次更新行时递增序列号。</span><span class="sxs-lookup"><span data-stu-id="dcb00-168">The [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="dcb00-169">在`Update`或`Delete`命令，`Where`子句包括跟踪列 （原始行版本） 的原始值。</span><span class="sxs-lookup"><span data-stu-id="dcb00-169">In an `Update` or `Delete` command, the `Where` clause includes the original value of the tracking column (the original row version).</span></span> <span data-ttu-id="dcb00-170">如果正在更新的行已由其他用户时中的值更改`rowversion`列是不同于原始值，因此`Update`或`Delete`语句找不到要由于更新的行`Where`子句。</span><span class="sxs-lookup"><span data-stu-id="dcb00-170">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value, so the `Update` or `Delete` statement can't find the row to update because of the `Where` clause.</span></span> <span data-ttu-id="dcb00-171">在实体框架查找已通过更新了任何行`Update`或`Delete`命令 （即，在受影响的行数为零） 时，它将，解释为并发冲突。</span><span class="sxs-lookup"><span data-stu-id="dcb00-171">When the Entity Framework finds that no rows have been updated by the `Update` or `Delete` command (that is, when the number of affected rows is zero), it interprets that as a concurrency conflict.</span></span>
- <span data-ttu-id="dcb00-172">配置实体框架可以包括在表中的每个列的原始值`Where`子句`Update`和`Delete`命令。</span><span class="sxs-lookup"><span data-stu-id="dcb00-172">Configure the Entity Framework to include the original values of every column in the table in the `Where` clause of `Update` and `Delete` commands.</span></span>

    <span data-ttu-id="dcb00-173">如下所示的第一个选项，如果自第一次读取一行，行中的任何内容已更改`Where`子句不会返回的行，若要更新，该实体框架会将解释为并发冲突。</span><span class="sxs-lookup"><span data-stu-id="dcb00-173">As in the first option, if anything in the row has changed since the row was first read, the `Where` clause won't return a row to update, which the Entity Framework interprets as a concurrency conflict.</span></span> <span data-ttu-id="dcb00-174">对于有多列的数据库表，这种方法可能会导致非常大`Where`子句，并可能需要维护大量的状态。</span><span class="sxs-lookup"><span data-stu-id="dcb00-174">For database tables that have many columns, this approach can result in very large `Where` clauses, and can require that you maintain large amounts of state.</span></span> <span data-ttu-id="dcb00-175">如前文所述，保留大量的状态可能会影响应用程序性能因为它需要服务器资源或必须包含在 web 页本身。</span><span class="sxs-lookup"><span data-stu-id="dcb00-175">As noted earlier, maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself.</span></span> <span data-ttu-id="dcb00-176">因此此方法通常不建议，并且它不在本教程中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="dcb00-176">Therefore this approach generally not recommended, and it isn't the method used in this tutorial.</span></span>

    <span data-ttu-id="dcb00-177">如果您想要实现这种并发的方法，则必须将你想要通过添加跟踪的并发性实体中的所有非主键属性标记[ConcurrencyCheck](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx)属性设为它们。</span><span class="sxs-lookup"><span data-stu-id="dcb00-177">If you do want to implement this approach to concurrency, you have to mark all non-primary-key properties in the entity you want to track concurrency for by adding the [ConcurrencyCheck](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) attribute to them.</span></span> <span data-ttu-id="dcb00-178">更改使实体框架能够在 SQL 中包括所有列`WHERE`子句`UPDATE`语句。</span><span class="sxs-lookup"><span data-stu-id="dcb00-178">That change enables the Entity Framework to include all columns in the SQL `WHERE` clause of `UPDATE` statements.</span></span>

<span data-ttu-id="dcb00-179">本教程的其余部分中将添加[rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx)跟踪属性`Department`实体，创建控制器和视图，并进行测试以验证一切是否正常工作。</span><span class="sxs-lookup"><span data-stu-id="dcb00-179">In the remainder of this tutorial you'll add a [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) tracking property to the `Department` entity, create a controller and views, and test to verify that everything works correctly.</span></span>

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a><span data-ttu-id="dcb00-180">将开放式并发属性添加到部门实体</span><span class="sxs-lookup"><span data-stu-id="dcb00-180">Add an Optimistic Concurrency Property to the Department Entity</span></span>

<span data-ttu-id="dcb00-181">在*Models\Department.cs*，添加名为的跟踪属性`RowVersion`:</span><span class="sxs-lookup"><span data-stu-id="dcb00-181">In *Models\Department.cs*, add a tracking property named `RowVersion`:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

<span data-ttu-id="dcb00-182">[时间戳](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx)属性指定此列将包含在`Where`子句`Update`和`Delete`命令发送到数据库。</span><span class="sxs-lookup"><span data-stu-id="dcb00-182">The [Timestamp](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx) attribute specifies that this column will be included in the `Where` clause of `Update` and `Delete` commands sent to the database.</span></span> <span data-ttu-id="dcb00-183">该属性称为[时间戳](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx)因为以前版本的 SQL Server 使用 SQL[时间戳](https://msdn.microsoft.com/en-us/library/ms182776(v=SQL.90).aspx)之前 SQL 数据类型[rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx)替换它。</span><span class="sxs-lookup"><span data-stu-id="dcb00-183">The attribute is called [Timestamp](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx) because previous versions of SQL Server used a SQL [timestamp](https://msdn.microsoft.com/en-us/library/ms182776(v=SQL.90).aspx) data type before the SQL [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) replaced it.</span></span> <span data-ttu-id="dcb00-184">.Net 类型`rowversion`为字节数组。</span><span class="sxs-lookup"><span data-stu-id="dcb00-184">The .Net type for `rowversion` is a byte array.</span></span> <span data-ttu-id="dcb00-185">如果想要使用 fluent API，则可以使用[IsConcurrencyToken](https://msdn.microsoft.com/en-us/library/gg679501(v=VS.103).aspx)方法，以指定跟踪属性，如下面的示例中所示：</span><span class="sxs-lookup"><span data-stu-id="dcb00-185">If you prefer to use the fluent API, you can use the [IsConcurrencyToken](https://msdn.microsoft.com/en-us/library/gg679501(v=VS.103).aspx) method to specify the tracking property, as shown in the following example:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="dcb00-186">通过添加一个属性，更改数据库模型，因此你需要执行另一个迁移。</span><span class="sxs-lookup"><span data-stu-id="dcb00-186">By adding a property you changed the database model, so you need to do another migration.</span></span> <span data-ttu-id="dcb00-187">在包管理器控制台 (PMC) 中，输入以下命令：</span><span class="sxs-lookup"><span data-stu-id="dcb00-187">In the Package Manager Console (PMC), enter the following commands:</span></span>

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a><span data-ttu-id="dcb00-188">创建部门控制器</span><span class="sxs-lookup"><span data-stu-id="dcb00-188">Create a Department Controller</span></span>

<span data-ttu-id="dcb00-189">创建`Department`控制器和视图相同的方式未其他控制器，使用以下设置：</span><span class="sxs-lookup"><span data-stu-id="dcb00-189">Create a `Department` controller and views the same way you did the other controllers, using the following settings:</span></span>

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

<span data-ttu-id="dcb00-191">在*Controllers\DepartmentController.cs*，添加`using`语句：</span><span class="sxs-lookup"><span data-stu-id="dcb00-191">In *Controllers\DepartmentController.cs*, add a `using` statement:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="dcb00-192">更改"LastName"到"全名"此文件 （四个匹配项） 中的所有位置，以便部门管理员下拉列表将包含教师的完整名称而不是只是最后一个名称。</span><span class="sxs-lookup"><span data-stu-id="dcb00-192">Change "LastName" to "FullName" everywhere in this file (four occurrences) so that the department administrator drop-down lists will contain the full name of the instructor rather than just the last name.</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

<span data-ttu-id="dcb00-193">替换现有代码，以`HttpPost``Edit`方法替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="dcb00-193">Replace the existing code for the `HttpPost` `Edit` method with the following code:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

<span data-ttu-id="dcb00-194">该视图将存储原始`RowVersion`隐藏字段中的值。</span><span class="sxs-lookup"><span data-stu-id="dcb00-194">The view will store the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="dcb00-195">当模型联编程序创建`department`实例，该对象将拥有原始`RowVersion`属性值和其他属性，如由用户在编辑页上输入的新值。</span><span class="sxs-lookup"><span data-stu-id="dcb00-195">When the model binder creates the `department` instance, that object will have the original `RowVersion` property value and the new values for the other properties, as entered by the user on the Edit page.</span></span> <span data-ttu-id="dcb00-196">然后创建时实体框架 SQL`UPDATE`命令时，命令将包括`WHERE`查找具有原始行的子句`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="dcb00-196">Then when the Entity Framework creates a SQL `UPDATE` command, that command will include a `WHERE` clause that looks for a row that has the original `RowVersion` value.</span></span>

<span data-ttu-id="dcb00-197">如果任何行不受`UPDATE`命令 (任何行具有原始`RowVersion`值)，实体框架将引发`DbUpdateConcurrencyException`异常，并且中的代码`catch`块可获取在受影响`Department`的异常中的实体对象。</span><span class="sxs-lookup"><span data-stu-id="dcb00-197">If no rows are affected by the `UPDATE` command (no rows have the original `RowVersion` value), the Entity Framework throws a `DbUpdateConcurrencyException` exception, and the code in the `catch` block gets the affected `Department` entity from the exception object.</span></span> <span data-ttu-id="dcb00-198">此实体具有从数据库读取的值和用户输入的新值：</span><span class="sxs-lookup"><span data-stu-id="dcb00-198">This entity has both the values read from the database and the new values entered by the user:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

<span data-ttu-id="dcb00-199">接下来，代码将添加每个列都具有数据库值不同于在编辑页上输入的用户自定义错误消息：</span><span class="sxs-lookup"><span data-stu-id="dcb00-199">Next, the code adds a custom error message for each column that has database values different from what the user entered on the Edit page:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

<span data-ttu-id="dcb00-200">较长的错误消息说明发生了什么情况以及如何对它执行的操作：</span><span class="sxs-lookup"><span data-stu-id="dcb00-200">A longer error message explains what happened and what to do about it:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="dcb00-201">最后，代码设置`RowVersion`值`Department`从数据库中检索到的新值的对象。</span><span class="sxs-lookup"><span data-stu-id="dcb00-201">Finally, the code sets the `RowVersion` value of the `Department` object to the new value retrieved from the database.</span></span> <span data-ttu-id="dcb00-202">此新`RowVersion`值将存储在隐藏的字段，将重新显示页，编辑和下一次用户单击时**保存**，因为将捕获的编辑页重新显示发生这种情况的并发错误。</span><span class="sxs-lookup"><span data-stu-id="dcb00-202">This new `RowVersion` value will be stored in the hidden field when the Edit page is redisplayed, and the next time the user clicks **Save**, only concurrency errors that happen since the redisplay of the Edit page will be caught.</span></span>

<span data-ttu-id="dcb00-203">在*Views\Department\Edit.cshtml*，添加隐藏的字段以保存`RowVersion`立即之后的隐藏的字段的属性值`DepartmentID`属性：</span><span class="sxs-lookup"><span data-stu-id="dcb00-203">In *Views\Department\Edit.cshtml*, add a hidden field to save the `RowVersion` property value, immediately following the hidden field for the `DepartmentID` property:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

<span data-ttu-id="dcb00-204">在*Views\Department\Index.cshtml*，将现有代码替换为以下代码以向左移动行链接和更改页面标题和列标题以显示`FullName`而不是`LastName`中**管理员**列：</span><span class="sxs-lookup"><span data-stu-id="dcb00-204">In *Views\Department\Index.cshtml*, replace the existing code with the following code to move row links to the left and change the page title and column headings to display `FullName` instead of `LastName` in the **Administrator** column:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a><span data-ttu-id="dcb00-205">测试乐观并发处理</span><span class="sxs-lookup"><span data-stu-id="dcb00-205">Testing Optimistic Concurrency Handling</span></span>

<span data-ttu-id="dcb00-206">运行站点，然后单击**部门**:</span><span class="sxs-lookup"><span data-stu-id="dcb00-206">Run the site and click **Departments**:</span></span>

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="dcb00-208">右键单击**编辑**Kim Abercrombie 并选择超链接**在新的选项卡中打开**然后单击**编辑**为 Kim Abercrombie 的超链接。</span><span class="sxs-lookup"><span data-stu-id="dcb00-208">Right click the **Edit** hyperlink for Kim Abercrombie and select **Open in new tab,** then click the **Edit** hyperlink for Kim Abercrombie.</span></span> <span data-ttu-id="dcb00-209">两个窗口显示相同的信息。</span><span class="sxs-lookup"><span data-stu-id="dcb00-209">The two windows display the same information.</span></span>

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="dcb00-211">更改第一个浏览器窗口中的字段，然后单击**保存**。</span><span class="sxs-lookup"><span data-stu-id="dcb00-211">Change a field in the first browser window and click **Save**.</span></span>

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="dcb00-213">浏览器将显示更改后的值的索引页。</span><span class="sxs-lookup"><span data-stu-id="dcb00-213">The browser shows the Index page with the changed value.</span></span>

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

<span data-ttu-id="dcb00-215">更改第二个浏览器窗口中的任何字段，然后单击**保存**。</span><span class="sxs-lookup"><span data-stu-id="dcb00-215">Change the any field in the second browser window and click **Save**.</span></span>

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

<span data-ttu-id="dcb00-217">单击**保存**在第二个浏览器窗口。</span><span class="sxs-lookup"><span data-stu-id="dcb00-217">Click **Save** in the second browser window.</span></span> <span data-ttu-id="dcb00-218">你看到一条错误消息：</span><span class="sxs-lookup"><span data-stu-id="dcb00-218">You see an error message:</span></span>

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

<span data-ttu-id="dcb00-220">单击**保存**试。</span><span class="sxs-lookup"><span data-stu-id="dcb00-220">Click **Save** again.</span></span> <span data-ttu-id="dcb00-221">在第二个浏览器中输入的值与在第一个浏览器中更改的数据的原始值一起保存。</span><span class="sxs-lookup"><span data-stu-id="dcb00-221">The value you entered in the second browser is saved along with the original value of the data you change in the first browser.</span></span> <span data-ttu-id="dcb00-222">索引页面显示时，将显示保存的值。</span><span class="sxs-lookup"><span data-stu-id="dcb00-222">You see the saved values when the Index page appears.</span></span>

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a><span data-ttu-id="dcb00-224">更新删除页</span><span class="sxs-lookup"><span data-stu-id="dcb00-224">Updating the Delete Page</span></span>

<span data-ttu-id="dcb00-225">对于删除页中，实体框架检测并发冲突某人其他类似的方式编辑部门引起。</span><span class="sxs-lookup"><span data-stu-id="dcb00-225">For the Delete page, the Entity Framework detects concurrency conflicts caused by someone else editing the department in a similar manner.</span></span> <span data-ttu-id="dcb00-226">当`HttpGet``Delete`方法会显示确认视图，则视图包括原始`RowVersion`隐藏字段中的值。</span><span class="sxs-lookup"><span data-stu-id="dcb00-226">When the `HttpGet` `Delete` method displays the confirmation view, the view includes the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="dcb00-227">然后，该值将可供`HttpPost``Delete`用户确认删除时调用的方法。</span><span class="sxs-lookup"><span data-stu-id="dcb00-227">That value is then available to the `HttpPost` `Delete` method that's called when the user confirms the deletion.</span></span> <span data-ttu-id="dcb00-228">当实体框架创建 SQL`DELETE`命令时，它包括`WHERE`子句与原始`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="dcb00-228">When the Entity Framework creates the SQL `DELETE` command, it includes a `WHERE` clause with the original `RowVersion` value.</span></span> <span data-ttu-id="dcb00-229">如果零行中的命令结果的影响 （含义后显示删除确认页，已更改行），则会引发一个并发异常，和`HttpGet Delete`调用方法时，将设置为错误标志`true`以重新显示确认页，并一条错误消息。</span><span class="sxs-lookup"><span data-stu-id="dcb00-229">If the command results in zero rows affected (meaning the row was changed after the Delete confirmation page was displayed), a concurrency exception is thrown, and the `HttpGet Delete` method is called with an error flag set to `true` in order to redisplay the confirmation page with an error message.</span></span> <span data-ttu-id="dcb00-230">还有可能因为行由其他用户时，删除，因此不同的错误消息显示在此情况下影响了零行。</span><span class="sxs-lookup"><span data-stu-id="dcb00-230">It's also possible that zero rows were affected because the row was deleted by another user, so in that case a different error message is displayed.</span></span>

<span data-ttu-id="dcb00-231">在*DepartmentController.cs*，替换`HttpGet``Delete`方法替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="dcb00-231">In *DepartmentController.cs*, replace the `HttpGet` `Delete` method with the following code:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

<span data-ttu-id="dcb00-232">该方法将接受可选参数，该值指示是否页正在后将重新显示并发错误。</span><span class="sxs-lookup"><span data-stu-id="dcb00-232">The method accepts an optional parameter that indicates whether the page is being redisplayed after a concurrency error.</span></span> <span data-ttu-id="dcb00-233">如果此标志为`true`，一条错误消息发送到视图使用`ViewBag`属性。</span><span class="sxs-lookup"><span data-stu-id="dcb00-233">If this flag is `true`, an error message is sent to the view using a `ViewBag` property.</span></span>

<span data-ttu-id="dcb00-234">中的代码替换`HttpPost``Delete`方法 (名为`DeleteConfirmed`) 替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="dcb00-234">Replace the code in the `HttpPost` `Delete` method (named `DeleteConfirmed`) with the following code:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

<span data-ttu-id="dcb00-235">在您刚更换的基架代码，此方法接受仅记录 ID:</span><span class="sxs-lookup"><span data-stu-id="dcb00-235">In the scaffolded code that you just replaced, this method accepted only a record ID:</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

<span data-ttu-id="dcb00-236">你已更改此参数设为`Department`实体实例创建的模型联编程序。</span><span class="sxs-lookup"><span data-stu-id="dcb00-236">You've changed this parameter to a `Department` entity instance created by the model binder.</span></span> <span data-ttu-id="dcb00-237">这使您可以访问`RowVersion`除了的记录密钥的属性值。</span><span class="sxs-lookup"><span data-stu-id="dcb00-237">This gives you access to the `RowVersion` property value in addition to the record key.</span></span>

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

<span data-ttu-id="dcb00-238">您还已更改的操作方法名称`DeleteConfirmed`到`Delete`。</span><span class="sxs-lookup"><span data-stu-id="dcb00-238">You have also changed the action method name from `DeleteConfirmed` to `Delete`.</span></span> <span data-ttu-id="dcb00-239">名为的基架的代码`HttpPost``Delete`方法`DeleteConfirmed`以便`HttpPost`方法一个唯一的签名。</span><span class="sxs-lookup"><span data-stu-id="dcb00-239">The scaffolded code named the `HttpPost` `Delete` method `DeleteConfirmed` to give the `HttpPost` method a unique signature.</span></span> <span data-ttu-id="dcb00-240">（CLR 需要具有不同的方法参数的重载的方法。）签名是唯一的现在可以坚持使用 MVC 约定，使用相同的名称用于`HttpPost`和`HttpGet`删除方法。</span><span class="sxs-lookup"><span data-stu-id="dcb00-240">(The CLR requires overloaded methods to have different method parameters.) Now that the signatures are unique, you can stick with the MVC convention and use the same name for the `HttpPost` and `HttpGet` delete methods.</span></span>

<span data-ttu-id="dcb00-241">如果捕获到并发错误，则该代码将显示删除确认页，并提供一个标志，指示它应显示并发错误消息。</span><span class="sxs-lookup"><span data-stu-id="dcb00-241">If a concurrency error is caught, the code redisplays the Delete confirmation page and provides a flag that indicates it should display a concurrency error message.</span></span>

<span data-ttu-id="dcb00-242">在*Views\Department\Delete.cshtml*，替换替换为以下代码，使一些格式设置该基架的代码更改，并添加一个错误消息域。</span><span class="sxs-lookup"><span data-stu-id="dcb00-242">In *Views\Department\Delete.cshtml*, replace the scaffolded code with the following code that makes some formatting changes and adds an error message field.</span></span> <span data-ttu-id="dcb00-243">突出显示所做的更改。</span><span class="sxs-lookup"><span data-stu-id="dcb00-243">The changes are highlighted.</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

<span data-ttu-id="dcb00-244">此代码将添加一条错误消息之间`h2`和`h3`标题：</span><span class="sxs-lookup"><span data-stu-id="dcb00-244">This code adds an error message between the `h2` and `h3` headings:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

<span data-ttu-id="dcb00-245">它将替换`LastName`与`FullName`中`Administrator`字段：</span><span class="sxs-lookup"><span data-stu-id="dcb00-245">It replaces `LastName` with `FullName` in the `Administrator` field:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

<span data-ttu-id="dcb00-246">最后，它将添加隐藏的字段`DepartmentID`和`RowVersion`后的属性`Html.BeginForm`语句：</span><span class="sxs-lookup"><span data-stu-id="dcb00-246">Finally, it adds hidden fields for the `DepartmentID` and `RowVersion` properties after the `Html.BeginForm` statement:</span></span>

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

<span data-ttu-id="dcb00-247">运行部门索引页。</span><span class="sxs-lookup"><span data-stu-id="dcb00-247">Run the Departments Index page.</span></span> <span data-ttu-id="dcb00-248">右键单击**删除**英语部门和选择的超链接**在新窗口中打开**然后在第一个窗口中单击**编辑**对于英语的超链接部门。</span><span class="sxs-lookup"><span data-stu-id="dcb00-248">Right click the **Delete** hyperlink for the English department and select **Open in new window,** then in the first window click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="dcb00-249">在第一个窗口中，更改一个值，然后单击**保存**:</span><span class="sxs-lookup"><span data-stu-id="dcb00-249">In the first window, change one of the values, and click **Save** :</span></span>

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<span data-ttu-id="dcb00-251">索引页确认更改。</span><span class="sxs-lookup"><span data-stu-id="dcb00-251">The Index page confirms the change.</span></span>

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

<span data-ttu-id="dcb00-253">在第二个窗口中，单击**删除**。</span><span class="sxs-lookup"><span data-stu-id="dcb00-253">In the second window, click **Delete**.</span></span>

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

<span data-ttu-id="dcb00-255">请参阅并发错误消息，并使用什么是当前数据库中刷新的部门值。</span><span class="sxs-lookup"><span data-stu-id="dcb00-255">You see the concurrency error message, and the Department values are refreshed with what's currently in the database.</span></span>

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

<span data-ttu-id="dcb00-257">如果你单击**删除**同样，在重定向到显示部门已被删除的索引页。</span><span class="sxs-lookup"><span data-stu-id="dcb00-257">If you click **Delete** again, you're redirected to the Index page, which shows that the department has been deleted.</span></span>

## <a name="summary"></a><span data-ttu-id="dcb00-258">摘要</span><span class="sxs-lookup"><span data-stu-id="dcb00-258">Summary</span></span>

<span data-ttu-id="dcb00-259">这将完成简介处理并发冲突。</span><span class="sxs-lookup"><span data-stu-id="dcb00-259">This completes the introduction to handling concurrency conflicts.</span></span> <span data-ttu-id="dcb00-260">有关其他方法来处理各种并发方案的信息，请参阅[乐观并发模式](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx)和[使用属性值](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx)实体框架团队博客上。</span><span class="sxs-lookup"><span data-stu-id="dcb00-260">For information about other ways to handle various concurrency scenarios, see [Optimistic Concurrency Patterns](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) and [Working with Property Values](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) on the Entity Framework team blog.</span></span> <span data-ttu-id="dcb00-261">下一教程演示如何实现的每个层次结构一个表继承`Instructor`和`Student`实体。</span><span class="sxs-lookup"><span data-stu-id="dcb00-261">The next tutorial shows how to implement table-per-hierarchy inheritance for the `Instructor` and `Student` entities.</span></span>

<span data-ttu-id="dcb00-262">在找不到其他实体框架资源的链接[ASP.NET 数据访问内容映射](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="dcb00-262">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="dcb00-263">[上一页](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一页](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="dcb00-263">[Previous](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
