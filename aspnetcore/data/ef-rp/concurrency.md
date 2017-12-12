---
title: "与 EF 核心-并发-8 的 8 razor 页"
author: rick-anderson
description: "本教程演示如何处理冲突，当多个用户在同一时间更新同一实体。"
keywords: "ASP.NET 核心，实体框架核心并发"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/concurrency
ms.openlocfilehash: 0c49376fd1b602fe03ef2a152d19b58513ae2710
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2017
---
<span data-ttu-id="eef36-104">en-我们 /</span><span class="sxs-lookup"><span data-stu-id="eef36-104">en-us/</span></span>

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a><span data-ttu-id="eef36-105">处理并发冲突的 EF 内核，它们有 Razor 页 (8 的第 8)</span><span class="sxs-lookup"><span data-stu-id="eef36-105">Handling concurrency conflicts - EF Core with Razor Pages (8 of 8)</span></span>

<span data-ttu-id="eef36-106">通过[Rick Anderson](https://twitter.com/RickAndMSFT)， [Tom Dykstra](https://github.com/tdykstra)，和[Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="eef36-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and  [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="eef36-107">本教程演示如何在多个用户并发 （在同一时间） 更新实体时处理冲突。</span><span class="sxs-lookup"><span data-stu-id="eef36-107">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="eef36-108">如果你遇到无法解决的问题，请下载[对此阶段已完成应用程序](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8)。</span><span class="sxs-lookup"><span data-stu-id="eef36-108">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="eef36-109">并发冲突</span><span class="sxs-lookup"><span data-stu-id="eef36-109">Concurrency conflicts</span></span>

<span data-ttu-id="eef36-110">发生并发冲突时：</span><span class="sxs-lookup"><span data-stu-id="eef36-110">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="eef36-111">用户导航到实体的编辑页。</span><span class="sxs-lookup"><span data-stu-id="eef36-111">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="eef36-112">在第一个用户的更改写入到数据库之前，另一个用户更新相同的实体。</span><span class="sxs-lookup"><span data-stu-id="eef36-112">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="eef36-113">如果未启用并发检测，当发生并发更新：</span><span class="sxs-lookup"><span data-stu-id="eef36-113">If concurrency detection is not enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="eef36-114">最后一个的更新 wins。</span><span class="sxs-lookup"><span data-stu-id="eef36-114">The last update wins.</span></span> <span data-ttu-id="eef36-115">即，最后一个的更新值将保存到数据库。</span><span class="sxs-lookup"><span data-stu-id="eef36-115">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="eef36-116">当前的更新的第一个都将丢失。</span><span class="sxs-lookup"><span data-stu-id="eef36-116">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="eef36-117">开放式并发</span><span class="sxs-lookup"><span data-stu-id="eef36-117">Optimistic concurrency</span></span>

<span data-ttu-id="eef36-118">开放式并发允许并发冲突发生，，然后反应适当时它们完成。</span><span class="sxs-lookup"><span data-stu-id="eef36-118">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="eef36-119">例如，Jane 访问部门编辑页，并从 $350,000.00 英语部门预算变为 0.00 美元。</span><span class="sxs-lookup"><span data-stu-id="eef36-119">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![将预算更改为 0](concurrency/_static/change-budget.png)

<span data-ttu-id="eef36-121">Jane 单击之前**保存**，John 访问同一页上，并从 2007 年 9 月 1 日的开始日期字段更改为 2013 年 9 月 1 日。</span><span class="sxs-lookup"><span data-stu-id="eef36-121">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![更改为 2013年的开始日期](concurrency/_static/change-date.png)

<span data-ttu-id="eef36-123">Jane 单击**保存**第一个，看到她更改时浏览器将显示索引页面。</span><span class="sxs-lookup"><span data-stu-id="eef36-123">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![更改为零的预算](concurrency/_static/budget-zero.png)

<span data-ttu-id="eef36-125">John 单击**保存**仍会显示 $350,000.00 预算将编辑页上。</span><span class="sxs-lookup"><span data-stu-id="eef36-125">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="eef36-126">接下来如何操作取决于如何处理并发冲突。</span><span class="sxs-lookup"><span data-stu-id="eef36-126">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="eef36-127">开放式并发包括以下选项：</span><span class="sxs-lookup"><span data-stu-id="eef36-127">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="eef36-128">你可以跟踪的用户进行了修改的属性，并更新仅在数据库中的相应列。</span><span class="sxs-lookup"><span data-stu-id="eef36-128">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

 <span data-ttu-id="eef36-129">在方案中，将不会丢失任何数据。</span><span class="sxs-lookup"><span data-stu-id="eef36-129">In the scenario, no data would be lost.</span></span> <span data-ttu-id="eef36-130">不同的属性更新了两个用户。</span><span class="sxs-lookup"><span data-stu-id="eef36-130">Different properties were updated by the two users.</span></span> <span data-ttu-id="eef36-131">下一次有人浏览英语部门，他们将看到 Jane 的和 John 的更改。</span><span class="sxs-lookup"><span data-stu-id="eef36-131">The next time someone browses the English department, they'll see both Jane's and John's changes.</span></span> <span data-ttu-id="eef36-132">这种更新方法可以减少可能导致数据丢失的冲突数。</span><span class="sxs-lookup"><span data-stu-id="eef36-132">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="eef36-133">这种方法: * 无法避免数据丢失，如果同一属性进行竞争的更改。</span><span class="sxs-lookup"><span data-stu-id="eef36-133">This approach: * Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="eef36-134">* 是在 web 应用程序通常不可行。</span><span class="sxs-lookup"><span data-stu-id="eef36-134">* Is generally not practical in a web app.</span></span> <span data-ttu-id="eef36-135">它需要维护以跟踪所有提取的值和新值的有效状态。</span><span class="sxs-lookup"><span data-stu-id="eef36-135">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="eef36-136">保留大量的状态可能会影响应用程序性能。</span><span class="sxs-lookup"><span data-stu-id="eef36-136">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="eef36-137">* 可以增加相比实体上的并发检测的应用程序复杂性。</span><span class="sxs-lookup"><span data-stu-id="eef36-137">* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="eef36-138">你可以让 John 的更改覆盖 Jane 的更改。</span><span class="sxs-lookup"><span data-stu-id="eef36-138">You can let John's change overwrite Jane's change.</span></span>

 <span data-ttu-id="eef36-139">下一次有人浏览英语部门时，他们将看到 2013 年 9 月 1 日和提取 $350,000.00 值。</span><span class="sxs-lookup"><span data-stu-id="eef36-139">The next time someone browses the English department, they'll see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="eef36-140">这种方法称为*客户端优先*或*在 Wins 中最后一个*方案。</span><span class="sxs-lookup"><span data-stu-id="eef36-140">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="eef36-141">（从客户端的所有值都优先于什么是在数据存储。）如果你不执行任何并发处理的编码，客户端优先自动发生。</span><span class="sxs-lookup"><span data-stu-id="eef36-141">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="eef36-142">你可以防止 John 的更改正在更新数据库中。</span><span class="sxs-lookup"><span data-stu-id="eef36-142">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="eef36-143">通常情况下，应用程序将: * 显示一条错误消息。</span><span class="sxs-lookup"><span data-stu-id="eef36-143">Typically, the app would: * Display an error message.</span></span>
        <span data-ttu-id="eef36-144">* 显示数据的当前状态。</span><span class="sxs-lookup"><span data-stu-id="eef36-144">* Show the current state of the data.</span></span>
        <span data-ttu-id="eef36-145">* 允许用户重新应用更改。</span><span class="sxs-lookup"><span data-stu-id="eef36-145">* Allow the user to reapply the changes.</span></span>

 <span data-ttu-id="eef36-146">这称为*存储 Wins*方案。</span><span class="sxs-lookup"><span data-stu-id="eef36-146">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="eef36-147">（数据存储值优先于提交的客户端的值。）在本教程中实现存储 Wins 方案。</span><span class="sxs-lookup"><span data-stu-id="eef36-147">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="eef36-148">此方法可确保任何更改都会收到通知用户的情况下被覆盖。</span><span class="sxs-lookup"><span data-stu-id="eef36-148">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="eef36-149">处理并发</span><span class="sxs-lookup"><span data-stu-id="eef36-149">Handling concurrency</span></span> 

<span data-ttu-id="eef36-150">当属性配置为[并发标记](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="eef36-150">When a property is configured as a [concurrency token](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="eef36-151">EF 核心验证已提取后属性没有已修改过。</span><span class="sxs-lookup"><span data-stu-id="eef36-151">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="eef36-152">就会执行检查时[SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges)或[SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_)调用。</span><span class="sxs-lookup"><span data-stu-id="eef36-152">The check occurs when [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="eef36-153">如果它已提取的后, 更改的属性[DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0)引发。</span><span class="sxs-lookup"><span data-stu-id="eef36-153">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="eef36-154">数据库和数据模型必须配置为支持引发`DbUpdateConcurrencyException`。</span><span class="sxs-lookup"><span data-stu-id="eef36-154">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="eef36-155">检测并发冲突的属性</span><span class="sxs-lookup"><span data-stu-id="eef36-155">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="eef36-156">可以在属性级别检测并发冲突[ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0)属性。</span><span class="sxs-lookup"><span data-stu-id="eef36-156">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="eef36-157">特性可以应用于模型的多个属性。</span><span class="sxs-lookup"><span data-stu-id="eef36-157">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="eef36-158">有关详细信息，请参阅[数据批注 ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations)。</span><span class="sxs-lookup"><span data-stu-id="eef36-158">For more information, see [Data Annotations-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="eef36-159">`[ConcurrencyCheck]`属性不在本教程中使用。</span><span class="sxs-lookup"><span data-stu-id="eef36-159">The `[ConcurrencyCheck]` attribute is not used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="eef36-160">检测并发冲突行上</span><span class="sxs-lookup"><span data-stu-id="eef36-160">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="eef36-161">若要检测并发冲突[rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql)跟踪列添加到模型。</span><span class="sxs-lookup"><span data-stu-id="eef36-161">To detect concurrency conflicts, a [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="eef36-162">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="eef36-162">`rowversion` :</span></span>

* <span data-ttu-id="eef36-163">是特定的 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="eef36-163">Is SQL Server specific.</span></span> <span data-ttu-id="eef36-164">其他数据库可能无法提供相似的功能。</span><span class="sxs-lookup"><span data-stu-id="eef36-164">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="eef36-165">用于确定实体后从 DB 中提取了不被更改。</span><span class="sxs-lookup"><span data-stu-id="eef36-165">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="eef36-166">数据库生成顺序`rowversion`递增每次行的数量会更新。</span><span class="sxs-lookup"><span data-stu-id="eef36-166">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="eef36-167">在`Update`或`Delete`命令，`Where`子句包括将获取的值的`rowversion`。</span><span class="sxs-lookup"><span data-stu-id="eef36-167">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="eef36-168">如果正在更新的行已更改：</span><span class="sxs-lookup"><span data-stu-id="eef36-168">If the row being updated has changed:</span></span>

 * <span data-ttu-id="eef36-169">`rowversion`提取的值不匹配。</span><span class="sxs-lookup"><span data-stu-id="eef36-169">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="eef36-170">`Update`或`Delete`命令未找到行，因为`Where`子句中包括提取`rowversion`。</span><span class="sxs-lookup"><span data-stu-id="eef36-170">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="eef36-171">A`DbUpdateConcurrencyException`引发。</span><span class="sxs-lookup"><span data-stu-id="eef36-171">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="eef36-172">在 EF 核，通过不更新任何行后`Update`或`Delete`命令时，会引发并发异常。</span><span class="sxs-lookup"><span data-stu-id="eef36-172">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="eef36-173">将跟踪属性添加到部门实体</span><span class="sxs-lookup"><span data-stu-id="eef36-173">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="eef36-174">在*Models/Department.cs*，添加名为 RowVersion 的跟踪属性：</span><span class="sxs-lookup"><span data-stu-id="eef36-174">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="eef36-175">[时间戳](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute)属性指定此列包含在`Where`子句`Update`和`Delete`命令。</span><span class="sxs-lookup"><span data-stu-id="eef36-175">The [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="eef36-176">该属性称为`Timestamp`因为以前版本的 SQL Server 使用 SQL`timestamp`之前 SQL 数据类型`rowversion`类型替换它。</span><span class="sxs-lookup"><span data-stu-id="eef36-176">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="eef36-177">Fluent API 还可以指定跟踪属性：</span><span class="sxs-lookup"><span data-stu-id="eef36-177">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="eef36-178">下面的代码显示在更新部门名称时生成的 EF 核心 T-SQL 的一部分：</span><span class="sxs-lookup"><span data-stu-id="eef36-178">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="eef36-179">前面突出显示的代码所示`WHERE`子句包含`RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="eef36-179">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="eef36-180">如果数据库`RowVersion`不等于`RowVersion`参数 (`@p2`)，会更新任何行。</span><span class="sxs-lookup"><span data-stu-id="eef36-180">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="eef36-181">下面突出显示的代码演示 T-SQL 验证恰好一个行已更新：</span><span class="sxs-lookup"><span data-stu-id="eef36-181">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="eef36-182">[@@ROWCOUNT ](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql)返回的最后一个语句所影响的行数。</span><span class="sxs-lookup"><span data-stu-id="eef36-182">[@@ROWCOUNT](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="eef36-183">没有行均已更新，EF 核心引发`DbUpdateConcurrencyException`。</span><span class="sxs-lookup"><span data-stu-id="eef36-183">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="eef36-184">你可以看到在 Visual Studio 的输出窗口中生成的 T-SQL 的 EF 核心。</span><span class="sxs-lookup"><span data-stu-id="eef36-184">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="eef36-185">更新数据库</span><span class="sxs-lookup"><span data-stu-id="eef36-185">Update the DB</span></span>

<span data-ttu-id="eef36-186">添加`RowVersion`属性更改 DB 模型，这需要迁移。</span><span class="sxs-lookup"><span data-stu-id="eef36-186">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="eef36-187">生成项目。</span><span class="sxs-lookup"><span data-stu-id="eef36-187">Build the project.</span></span> <span data-ttu-id="eef36-188">在命令窗口中输入以下命令：</span><span class="sxs-lookup"><span data-stu-id="eef36-188">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="eef36-189">前面的命令：</span><span class="sxs-lookup"><span data-stu-id="eef36-189">The preceding commands:</span></span>

* <span data-ttu-id="eef36-190">将添加*迁移 / {时间 stamp}_RowVersion.cs*迁移文件。</span><span class="sxs-lookup"><span data-stu-id="eef36-190">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="eef36-191">更新*Migrations/SchoolContextModelSnapshot.cs*文件。</span><span class="sxs-lookup"><span data-stu-id="eef36-191">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="eef36-192">该更新添加到以下突出显示的代码`BuildModel`方法：</span><span class="sxs-lookup"><span data-stu-id="eef36-192">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="eef36-193">运行迁移来更新数据库。</span><span class="sxs-lookup"><span data-stu-id="eef36-193">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="eef36-194">基架部门模型</span><span class="sxs-lookup"><span data-stu-id="eef36-194">Scaffold the Departments model</span></span>

* <span data-ttu-id="eef36-195">退出 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="eef36-195">Exit Visual Studio.</span></span>
* <span data-ttu-id="eef36-196">打开项目目录（包含 Program.cs、Startup.cs 和 .csproj 文件的目录）中的命令窗口。</span><span class="sxs-lookup"><span data-stu-id="eef36-196">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="eef36-197">运行下面的命令：</span><span class="sxs-lookup"><span data-stu-id="eef36-197">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

<span data-ttu-id="eef36-198">前面的命令基架`Department`模型。</span><span class="sxs-lookup"><span data-stu-id="eef36-198">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="eef36-199">在 Visual Studio 中打开项目。</span><span class="sxs-lookup"><span data-stu-id="eef36-199">Open the project in Visual Studio.</span></span>

<span data-ttu-id="eef36-200">生成项目。</span><span class="sxs-lookup"><span data-stu-id="eef36-200">Build the project.</span></span> <span data-ttu-id="eef36-201">生成将生成如下所示的错误：</span><span class="sxs-lookup"><span data-stu-id="eef36-201">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="eef36-202">全局更改`_context.Department`到`_context.Departments`(即，将"s"添加到`Department`)。</span><span class="sxs-lookup"><span data-stu-id="eef36-202">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="eef36-203">7 匹配项的发现和更新。</span><span class="sxs-lookup"><span data-stu-id="eef36-203">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="eef36-204">更新部门索引页</span><span class="sxs-lookup"><span data-stu-id="eef36-204">Update the Departments Index page</span></span>

<span data-ttu-id="eef36-205">创建基架引擎`RowVersion`不应显示为索引页面中，但该字段的列。</span><span class="sxs-lookup"><span data-stu-id="eef36-205">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="eef36-206">在本教程中，最后一个字节的`RowVersion`显示以帮助了解并发。</span><span class="sxs-lookup"><span data-stu-id="eef36-206">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="eef36-207">最后一个字节不保证是唯一的。</span><span class="sxs-lookup"><span data-stu-id="eef36-207">The last byte is not guaranteed to be unique.</span></span> <span data-ttu-id="eef36-208">实际的应用程序不会显示`RowVersion`或最后一个字节的`RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="eef36-208">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="eef36-209">更新索引页：</span><span class="sxs-lookup"><span data-stu-id="eef36-209">Update the Index page:</span></span>

* <span data-ttu-id="eef36-210">将替换为部门的索引。</span><span class="sxs-lookup"><span data-stu-id="eef36-210">Replace Index with Departments.</span></span>
* <span data-ttu-id="eef36-211">替换标记包含`RowVersion`与最后一个字节的`RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="eef36-211">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="eef36-212">将替换为 FullName FirstMidName。</span><span class="sxs-lookup"><span data-stu-id="eef36-212">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="eef36-213">以下标记显示更新页上：</span><span class="sxs-lookup"><span data-stu-id="eef36-213">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="eef36-214">更新编辑页模型</span><span class="sxs-lookup"><span data-stu-id="eef36-214">Update the Edit page model</span></span>

<span data-ttu-id="eef36-215">更新*pages\departments\edit.cshtml.cs*替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="eef36-215">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="eef36-216">若要检测的并发问题， [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue)使用更新`rowVersion`实体中的值已提取。</span><span class="sxs-lookup"><span data-stu-id="eef36-216">To detect a concurrency issue, the [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity was it was fetched.</span></span> <span data-ttu-id="eef36-217">EF 核心生成 SQL UPDATE 命令与 WHERE 子句包含原始`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="eef36-217">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="eef36-218">如果更新命令会不影响任何行 (没有行具有原始`RowVersion`值)、 一个`DbUpdateConcurrencyException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="eef36-218">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

<span data-ttu-id="eef36-219">在前面的代码中，`Department.RowVersion`时提取了该实体是值。</span><span class="sxs-lookup"><span data-stu-id="eef36-219">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="eef36-220">`OriginalValue`是数据库中的值时`FirstOrDefaultAsync`调用此方法中。</span><span class="sxs-lookup"><span data-stu-id="eef36-220">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="eef36-221">以下代码将获取客户端值 （发布到此方法的值） 和 DB 值：</span><span class="sxs-lookup"><span data-stu-id="eef36-221">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="eef36-222">以下代码将添加自定义错误消息，为每个列具有 DB 值不同于什么发到`OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="eef36-222">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="eef36-223">以下突出显示代码集`RowVersion`从数据库检索为新值的值。</span><span class="sxs-lookup"><span data-stu-id="eef36-223">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="eef36-224">下次用户单击**保存**，仅将发生这种情况由于将捕获的最后一个显示编辑页的并发错误。</span><span class="sxs-lookup"><span data-stu-id="eef36-224">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="eef36-225">`ModelState.Remove`语句是必需的因为`ModelState`具有旧`RowVersion`值。</span><span class="sxs-lookup"><span data-stu-id="eef36-225">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="eef36-226">在 Razor 页中，`ModelState`字段将优先于模型属性值都存在时的值。</span><span class="sxs-lookup"><span data-stu-id="eef36-226">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="eef36-227">更新编辑页</span><span class="sxs-lookup"><span data-stu-id="eef36-227">Update the Edit page</span></span>

<span data-ttu-id="eef36-228">更新*Pages/Departments/Edit.cshtml*替换为以下标记：</span><span class="sxs-lookup"><span data-stu-id="eef36-228">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="eef36-229">前面的标记：</span><span class="sxs-lookup"><span data-stu-id="eef36-229">The preceding markup:</span></span>

* <span data-ttu-id="eef36-230">更新`page`指令从`@page`到`@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="eef36-230">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="eef36-231">添加一个隐藏的行版本。</span><span class="sxs-lookup"><span data-stu-id="eef36-231">Adds a hidden row version.</span></span> <span data-ttu-id="eef36-232">`RowVersion`必须添加，使回发将的值绑定。</span><span class="sxs-lookup"><span data-stu-id="eef36-232">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="eef36-233">显示的最后一个字节`RowVersion`以便进行调试。</span><span class="sxs-lookup"><span data-stu-id="eef36-233">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="eef36-234">替换`ViewData`使用强类型`InstructorNameSL`。</span><span class="sxs-lookup"><span data-stu-id="eef36-234">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="eef36-235">测试与编辑页的并发冲突</span><span class="sxs-lookup"><span data-stu-id="eef36-235">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="eef36-236">打开两个浏览器上的实例编辑英语部门：</span><span class="sxs-lookup"><span data-stu-id="eef36-236">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="eef36-237">运行应用程序并选择部门。</span><span class="sxs-lookup"><span data-stu-id="eef36-237">Run the app and select Departments.</span></span>
* <span data-ttu-id="eef36-238">右键单击**编辑**英语部门和选择的超链接**新选项卡中打开**。</span><span class="sxs-lookup"><span data-stu-id="eef36-238">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="eef36-239">在第一个选项卡上，单击**编辑**英语部门的超链接。</span><span class="sxs-lookup"><span data-stu-id="eef36-239">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="eef36-240">两个浏览器选项卡显示相同的信息。</span><span class="sxs-lookup"><span data-stu-id="eef36-240">The two browser tabs display the same information.</span></span>

<span data-ttu-id="eef36-241">更改第一个浏览器选项卡中的名称，然后单击**保存**。</span><span class="sxs-lookup"><span data-stu-id="eef36-241">Change the name in the first browser tab and click **Save**.</span></span>

![部门编辑更改后的第 1 页](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="eef36-243">浏览器将显示有更改的值和更新的 rowVersion 指示器的索引页。</span><span class="sxs-lookup"><span data-stu-id="eef36-243">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="eef36-244">请注意更新的 rowVersion 指示器中，它将显示在第二个回发的其他选项卡中。</span><span class="sxs-lookup"><span data-stu-id="eef36-244">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="eef36-245">更改第二个浏览器选项卡中的不同字段。</span><span class="sxs-lookup"><span data-stu-id="eef36-245">Change a different field in the second browser tab.</span></span>

![部门编辑更改后的第 2 页](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="eef36-247">单击“保存” 。</span><span class="sxs-lookup"><span data-stu-id="eef36-247">Click **Save**.</span></span> <span data-ttu-id="eef36-248">你看到不匹配的 DB 值的所有字段的错误的消息：</span><span class="sxs-lookup"><span data-stu-id="eef36-248">You see error messages for all fields that don't match the DB values:</span></span>

![部门编辑页错误消息](concurrency/_static/edit-error.png)

<span data-ttu-id="eef36-250">此浏览器窗口不打算更改名称字段。</span><span class="sxs-lookup"><span data-stu-id="eef36-250">This browser window did not intend to change the Name field.</span></span> <span data-ttu-id="eef36-251">复制并粘贴到名称字段的当前值 （语言）。</span><span class="sxs-lookup"><span data-stu-id="eef36-251">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="eef36-252">选项卡上。客户端验证删除错误消息。</span><span class="sxs-lookup"><span data-stu-id="eef36-252">Tab out. Client-side validation removes the error message.</span></span>

![部门编辑页错误消息](concurrency/_static/cv.png)

<span data-ttu-id="eef36-254">单击**保存**试。</span><span class="sxs-lookup"><span data-stu-id="eef36-254">Click **Save** again.</span></span> <span data-ttu-id="eef36-255">保存在第二个浏览器选项卡中输入的值。</span><span class="sxs-lookup"><span data-stu-id="eef36-255">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="eef36-256">你看到索引页面中的已保存的值。</span><span class="sxs-lookup"><span data-stu-id="eef36-256">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="eef36-257">更新删除页</span><span class="sxs-lookup"><span data-stu-id="eef36-257">Update the Delete page</span></span>

<span data-ttu-id="eef36-258">替换为以下代码更新删除页模型：</span><span class="sxs-lookup"><span data-stu-id="eef36-258">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="eef36-259">在该实体已提取后发生更改时，删除页检测并发冲突。</span><span class="sxs-lookup"><span data-stu-id="eef36-259">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="eef36-260">`Department.RowVersion`提取了实体的行版本时。</span><span class="sxs-lookup"><span data-stu-id="eef36-260">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="eef36-261">当 EF 核心创建 SQL DELETE 命令时，它包括 WHERE 子句和`RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="eef36-261">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="eef36-262">如果受影响的零行中的 SQL DELETE 命令结果：</span><span class="sxs-lookup"><span data-stu-id="eef36-262">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="eef36-263">`RowVersion`中 SQL DELETE 命令不匹配`RowVersion`在数据库中。</span><span class="sxs-lookup"><span data-stu-id="eef36-263">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="eef36-264">会引发一个 DbUpdateConcurrencyException 异常。</span><span class="sxs-lookup"><span data-stu-id="eef36-264">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="eef36-265">`OnGetAsync`使用调用`concurrencyError`。</span><span class="sxs-lookup"><span data-stu-id="eef36-265">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="eef36-266">更新删除页</span><span class="sxs-lookup"><span data-stu-id="eef36-266">Update the Delete page</span></span>

<span data-ttu-id="eef36-267">更新*Pages/Departments/Delete.cshtml*替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="eef36-267">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="eef36-268">前面的标记进行以下更改：</span><span class="sxs-lookup"><span data-stu-id="eef36-268">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="eef36-269">更新`page`指令从`@page`到`@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="eef36-269">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="eef36-270">添加一条错误消息。</span><span class="sxs-lookup"><span data-stu-id="eef36-270">Adds an error message.</span></span>
* <span data-ttu-id="eef36-271">替换 FullName 中 FirstMidName**管理员**字段。</span><span class="sxs-lookup"><span data-stu-id="eef36-271">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="eef36-272">更改`RowVersion`要显示的最后一个字节。</span><span class="sxs-lookup"><span data-stu-id="eef36-272">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="eef36-273">添加一个隐藏的行版本。</span><span class="sxs-lookup"><span data-stu-id="eef36-273">Adds a hidden row version.</span></span> <span data-ttu-id="eef36-274">`RowVersion`必须添加，使回发将的值绑定。</span><span class="sxs-lookup"><span data-stu-id="eef36-274">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="eef36-275">使用删除页上的测试并发冲突</span><span class="sxs-lookup"><span data-stu-id="eef36-275">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="eef36-276">创建测试部门。</span><span class="sxs-lookup"><span data-stu-id="eef36-276">Create a test department.</span></span>

<span data-ttu-id="eef36-277">打开两个浏览器上的实例删除测试部门：</span><span class="sxs-lookup"><span data-stu-id="eef36-277">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="eef36-278">运行应用程序并选择部门。</span><span class="sxs-lookup"><span data-stu-id="eef36-278">Run the app and select Departments.</span></span>
* <span data-ttu-id="eef36-279">右键单击**删除**超链接的测试部门和选择**新选项卡中打开**。</span><span class="sxs-lookup"><span data-stu-id="eef36-279">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="eef36-280">单击**编辑**测试部门的超链接。</span><span class="sxs-lookup"><span data-stu-id="eef36-280">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="eef36-281">两个浏览器选项卡显示相同的信息。</span><span class="sxs-lookup"><span data-stu-id="eef36-281">The two browser tabs display the same information.</span></span>

<span data-ttu-id="eef36-282">更改第一个浏览器选项卡中的预算并单击**保存**。</span><span class="sxs-lookup"><span data-stu-id="eef36-282">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="eef36-283">浏览器将显示有更改的值和更新的 rowVersion 指示器的索引页。</span><span class="sxs-lookup"><span data-stu-id="eef36-283">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="eef36-284">请注意更新的 rowVersion 指示器中，它将显示在第二个回发的其他选项卡中。</span><span class="sxs-lookup"><span data-stu-id="eef36-284">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="eef36-285">从第二个选项卡中删除测试部门。并发错误是显示来自数据库的当前值。</span><span class="sxs-lookup"><span data-stu-id="eef36-285">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="eef36-286">单击**删除**删除的实体，除非`RowVersion`已 updated.department 已被删除。</span><span class="sxs-lookup"><span data-stu-id="eef36-286">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="eef36-287">请参阅[继承](xref:data/ef-mvc/inheritance)如何对数据模型中的继承的指令。</span><span class="sxs-lookup"><span data-stu-id="eef36-287">See [Inheritance](xref:data/ef-mvc/inheritance) for instruction on how to inheritance in the data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="eef36-288">其他资源</span><span class="sxs-lookup"><span data-stu-id="eef36-288">Additional resources</span></span>

* [<span data-ttu-id="eef36-289">在 EF 核心中的并发标记</span><span class="sxs-lookup"><span data-stu-id="eef36-289">Concurrency Tokens in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency)
* [<span data-ttu-id="eef36-290">在 EF 核心中处理并发</span><span class="sxs-lookup"><span data-stu-id="eef36-290">Handling concurrency in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[<span data-ttu-id="eef36-291">上一篇</span><span class="sxs-lookup"><span data-stu-id="eef36-291">Previous</span></span>](xref:data/ef-rp/update-related-data)
