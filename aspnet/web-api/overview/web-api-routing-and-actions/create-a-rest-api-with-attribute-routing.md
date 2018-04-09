---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: 使用 ASP.NET Web API 2 中的属性路由创建 REST API |Microsoft 文档
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 1f1e90544c9dd8439a522f2196d81d020ea2f4f2
ms.sourcegitcommit: 7f92990bad6a6cb901265d621dcbc136794f5f3f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="22f08-102">使用 ASP.NET Web API 2 中的路由的属性创建 REST API</span><span class="sxs-lookup"><span data-stu-id="22f08-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="22f08-103">通过[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="22f08-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="22f08-104">Web API 2 支持一种新型的路由，调用*的属性路由*。</span><span class="sxs-lookup"><span data-stu-id="22f08-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="22f08-105">有关的属性路由的一般概述，请参阅[Web API 2 中的属性路由](attribute-routing-in-web-api-2.md)。</span><span class="sxs-lookup"><span data-stu-id="22f08-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="22f08-106">在本教程中，你将使用的属性路由以创建用于丛书的集合的 REST API。</span><span class="sxs-lookup"><span data-stu-id="22f08-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="22f08-107">API 将支持以下操作：</span><span class="sxs-lookup"><span data-stu-id="22f08-107">The API will support the following actions:</span></span>

| <span data-ttu-id="22f08-108">操作</span><span class="sxs-lookup"><span data-stu-id="22f08-108">Action</span></span> | <span data-ttu-id="22f08-109">示例 URI</span><span class="sxs-lookup"><span data-stu-id="22f08-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="22f08-110">获取所有书籍的列表。</span><span class="sxs-lookup"><span data-stu-id="22f08-110">Get a list of all books.</span></span> | <span data-ttu-id="22f08-111">/ api/丛书</span><span class="sxs-lookup"><span data-stu-id="22f08-111">/api/books</span></span> |
| <span data-ttu-id="22f08-112">获取一本书的 id。</span><span class="sxs-lookup"><span data-stu-id="22f08-112">Get a book by ID.</span></span> | <span data-ttu-id="22f08-113">/api/books/1</span><span class="sxs-lookup"><span data-stu-id="22f08-113">/api/books/1</span></span> |
| <span data-ttu-id="22f08-114">获取书的详细信息。</span><span class="sxs-lookup"><span data-stu-id="22f08-114">Get the details of a book.</span></span> | <span data-ttu-id="22f08-115">/api/books/1/details</span><span class="sxs-lookup"><span data-stu-id="22f08-115">/api/books/1/details</span></span> |
| <span data-ttu-id="22f08-116">获取按风格的书籍的列表。</span><span class="sxs-lookup"><span data-stu-id="22f08-116">Get a list of books by genre.</span></span> | <span data-ttu-id="22f08-117">/api/books/fantasy</span><span class="sxs-lookup"><span data-stu-id="22f08-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="22f08-118">按发布日期中获取的书籍的列表。</span><span class="sxs-lookup"><span data-stu-id="22f08-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="22f08-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 （备用窗体）</span><span class="sxs-lookup"><span data-stu-id="22f08-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="22f08-120">获取由特定作者的丛书的列表。</span><span class="sxs-lookup"><span data-stu-id="22f08-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="22f08-121">/api/authors/1/books</span><span class="sxs-lookup"><span data-stu-id="22f08-121">/api/authors/1/books</span></span> |

<span data-ttu-id="22f08-122">所有方法都是只读的 （HTTP GET 请求）。</span><span class="sxs-lookup"><span data-stu-id="22f08-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="22f08-123">对于数据层中，我们将使用实体框架。</span><span class="sxs-lookup"><span data-stu-id="22f08-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="22f08-124">簿记录将具有以下字段：</span><span class="sxs-lookup"><span data-stu-id="22f08-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="22f08-125">Id</span><span class="sxs-lookup"><span data-stu-id="22f08-125">ID</span></span>
- <span data-ttu-id="22f08-126">标题</span><span class="sxs-lookup"><span data-stu-id="22f08-126">Title</span></span>
- <span data-ttu-id="22f08-127">风格</span><span class="sxs-lookup"><span data-stu-id="22f08-127">Genre</span></span>
- <span data-ttu-id="22f08-128">发布日期</span><span class="sxs-lookup"><span data-stu-id="22f08-128">Publication date</span></span>
- <span data-ttu-id="22f08-129">价格</span><span class="sxs-lookup"><span data-stu-id="22f08-129">Price</span></span>
- <span data-ttu-id="22f08-130">描述</span><span class="sxs-lookup"><span data-stu-id="22f08-130">Description</span></span>
- <span data-ttu-id="22f08-131">作者 Id （到作者表的外键）</span><span class="sxs-lookup"><span data-stu-id="22f08-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="22f08-132">但是，对于大多数请求，API 将返回此数据 （标题、 author 和风格） 的子集。</span><span class="sxs-lookup"><span data-stu-id="22f08-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="22f08-133">若要获取完整的记录，客户端请求`/api/books/{id}/details`。</span><span class="sxs-lookup"><span data-stu-id="22f08-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22f08-134">系统必备</span><span class="sxs-lookup"><span data-stu-id="22f08-134">Prerequisites</span></span>

<span data-ttu-id="22f08-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、 Professional 或 Enterprise 版本。</span><span class="sxs-lookup"><span data-stu-id="22f08-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="22f08-136">创建 Visual Studio 项目</span><span class="sxs-lookup"><span data-stu-id="22f08-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="22f08-137">通过运行 Visual Studio 来启动。</span><span class="sxs-lookup"><span data-stu-id="22f08-137">Start by running Visual Studio.</span></span> <span data-ttu-id="22f08-138">从**文件**菜单上，选择**新建**，然后选择**项目**。</span><span class="sxs-lookup"><span data-stu-id="22f08-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="22f08-139">在**模板**窗格中，选择**已安装的模板**展开**Visual C#**节点。</span><span class="sxs-lookup"><span data-stu-id="22f08-139">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="22f08-140">下**Visual C#**，选择**Web**。</span><span class="sxs-lookup"><span data-stu-id="22f08-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="22f08-141">在项目模板列表中，选择**ASP.NET MVC 4 Web 应用程序**。</span><span class="sxs-lookup"><span data-stu-id="22f08-141">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="22f08-142">将项目&quot;BooksAPI&quot;。</span><span class="sxs-lookup"><span data-stu-id="22f08-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="22f08-143">在**新建 ASP.NET 项目**对话框中，选择**空**模板。</span><span class="sxs-lookup"><span data-stu-id="22f08-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="22f08-144">在"添加文件夹和核心引用"，选择**Web API**复选框。</span><span class="sxs-lookup"><span data-stu-id="22f08-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="22f08-145">单击**创建项目**。</span><span class="sxs-lookup"><span data-stu-id="22f08-145">Click **Create Project**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="22f08-146">这将创建为 Web API 功能配置的主干项目。</span><span class="sxs-lookup"><span data-stu-id="22f08-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="22f08-147">域模型</span><span class="sxs-lookup"><span data-stu-id="22f08-147">Domain Models</span></span>

<span data-ttu-id="22f08-148">接下来，添加用于域模型的类。</span><span class="sxs-lookup"><span data-stu-id="22f08-148">Next, add classes for domain models.</span></span> <span data-ttu-id="22f08-149">在解决方案资源管理器，右键单击模型文件夹。</span><span class="sxs-lookup"><span data-stu-id="22f08-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="22f08-150">选择**添加**，然后选择**类**。</span><span class="sxs-lookup"><span data-stu-id="22f08-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="22f08-151">将此类命名为 `Author`。</span><span class="sxs-lookup"><span data-stu-id="22f08-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="22f08-152">将替换为以下 Author.cs 中的代码：</span><span class="sxs-lookup"><span data-stu-id="22f08-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="22f08-153">现在，添加名为的另一个类`Book`。</span><span class="sxs-lookup"><span data-stu-id="22f08-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="22f08-154">添加一个 Web API 控制器</span><span class="sxs-lookup"><span data-stu-id="22f08-154">Add a Web API Controller</span></span>

<span data-ttu-id="22f08-155">在此步骤中，我们将添加作为数据层使用 Entity Framework 的 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="22f08-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="22f08-156">按 Ctrl+Shift+B 生成项目。</span><span class="sxs-lookup"><span data-stu-id="22f08-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="22f08-157">实体框架使用反射来发现的属性的模型，因此它需要编译的程序集，若要创建数据库架构。</span><span class="sxs-lookup"><span data-stu-id="22f08-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="22f08-158">在解决方案资源管理器，右键单击 Controllers 文件夹。</span><span class="sxs-lookup"><span data-stu-id="22f08-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="22f08-159">选择**添加**，然后选择**控制器**。</span><span class="sxs-lookup"><span data-stu-id="22f08-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="22f08-160">在**添加基架**对话框中，选择"Web API 2 控制器具有读/写操作使用 Entity Framework。"</span><span class="sxs-lookup"><span data-stu-id="22f08-160">In the **Add Scaffold** dialog, select "Web API 2 Controller with read/write actions, using Entity Framework."</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="22f08-161">在**添加控制器**对话框中，为**控制器名称**，输入&quot;BooksController&quot;。</span><span class="sxs-lookup"><span data-stu-id="22f08-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="22f08-162">选择&quot;使用异步控制器操作&quot;复选框。</span><span class="sxs-lookup"><span data-stu-id="22f08-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="22f08-163">有关**模型类**，选择&quot;簿&quot;。</span><span class="sxs-lookup"><span data-stu-id="22f08-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="22f08-164">(如果看不到`Book`类列在下拉列表中，则应确保生成项目。)然后单击"+"按钮。</span><span class="sxs-lookup"><span data-stu-id="22f08-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="22f08-165">单击**添加**中**新建数据上下文**对话框。</span><span class="sxs-lookup"><span data-stu-id="22f08-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="22f08-166">单击**添加**中**添加控制器**对话框。</span><span class="sxs-lookup"><span data-stu-id="22f08-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="22f08-167">基架添加一个名为类`BooksController`，它定义的 API 控制器。</span><span class="sxs-lookup"><span data-stu-id="22f08-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="22f08-168">它还添加了一个名为类`BooksAPIContext`Models 文件夹中定义的数据上下文用于实体框架。</span><span class="sxs-lookup"><span data-stu-id="22f08-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="22f08-169">植入到数据库</span><span class="sxs-lookup"><span data-stu-id="22f08-169">Seed the Database</span></span>

<span data-ttu-id="22f08-170">从工具菜单中，选择**库程序包管理器**，然后选择**程序包管理器控制台**。</span><span class="sxs-lookup"><span data-stu-id="22f08-170">From the Tools menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="22f08-171">在 Package Manager Console 窗口中，输入以下命令：</span><span class="sxs-lookup"><span data-stu-id="22f08-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="22f08-172">此命令创建一个 Migrations 文件夹并将添加名为 Configuration.cs 的新代码文件。</span><span class="sxs-lookup"><span data-stu-id="22f08-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="22f08-173">打开此文件并添加以下代码`Configuration.Seed`方法。</span><span class="sxs-lookup"><span data-stu-id="22f08-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="22f08-174">在 Package Manager Console 窗口中，键入以下命令。</span><span class="sxs-lookup"><span data-stu-id="22f08-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="22f08-175">这些命令创建一个本地数据库，并调用 Seed 方法来填充数据库。</span><span class="sxs-lookup"><span data-stu-id="22f08-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="22f08-176">添加 DTO 类</span><span class="sxs-lookup"><span data-stu-id="22f08-176">Add DTO Classes</span></span>

<span data-ttu-id="22f08-177">如果你运行应用程序现在，并将 GET 请求发送到 /api/books/1，响应类似于以下。</span><span class="sxs-lookup"><span data-stu-id="22f08-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="22f08-178">（我添加缩进以提高可读性。）</span><span class="sxs-lookup"><span data-stu-id="22f08-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="22f08-179">相反，我想要此请求返回的字段的子集。</span><span class="sxs-lookup"><span data-stu-id="22f08-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="22f08-180">此外，我想让它返回作者的姓名，而不是作者 id。</span><span class="sxs-lookup"><span data-stu-id="22f08-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="22f08-181">为了实现此目的，我们将修改控制器方法以返回*数据传输对象*(DTO) 而不是 EF 模型。</span><span class="sxs-lookup"><span data-stu-id="22f08-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="22f08-182">DTO 是仅用于携带数据的对象。</span><span class="sxs-lookup"><span data-stu-id="22f08-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="22f08-183">在解决方案资源管理器，右键单击该项目并选择**添加** | **新文件夹**。</span><span class="sxs-lookup"><span data-stu-id="22f08-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="22f08-184">将文件夹命名为&quot;Dto&quot;。</span><span class="sxs-lookup"><span data-stu-id="22f08-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="22f08-185">添加一个名为类`BookDto`到 Dto 文件夹中，替换为以下定义：</span><span class="sxs-lookup"><span data-stu-id="22f08-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="22f08-186">添加名为的另一个类`BookDetailDto`。</span><span class="sxs-lookup"><span data-stu-id="22f08-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="22f08-187">接下来，更新`BooksController`类返回`BookDto`实例。</span><span class="sxs-lookup"><span data-stu-id="22f08-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="22f08-188">我们将使用[Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx)到项目的方法`Book`实例到`BookDto`实例。</span><span class="sxs-lookup"><span data-stu-id="22f08-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="22f08-189">以下是控制器类更新的代码。</span><span class="sxs-lookup"><span data-stu-id="22f08-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="22f08-190">我删除了`PutBook`， `PostBook`，和`DeleteBook`方法，因为本教程不需要它们。</span><span class="sxs-lookup"><span data-stu-id="22f08-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>


<span data-ttu-id="22f08-191">现在如果你运行应用程序，并请求 /api/books/1，响应正文应如下所示：</span><span class="sxs-lookup"><span data-stu-id="22f08-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="22f08-192">添加路由属性</span><span class="sxs-lookup"><span data-stu-id="22f08-192">Add Route Attributes</span></span>

<span data-ttu-id="22f08-193">接下来，我们将转换要使用的属性路由的控制器。</span><span class="sxs-lookup"><span data-stu-id="22f08-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="22f08-194">首先，添加**RoutePrefix**到控制器属性。</span><span class="sxs-lookup"><span data-stu-id="22f08-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="22f08-195">此属性定义在此控制器上的所有方法的初始 URI 段。</span><span class="sxs-lookup"><span data-stu-id="22f08-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="22f08-196">然后添加**[Route]**特性以控制器操作，如下所示：</span><span class="sxs-lookup"><span data-stu-id="22f08-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="22f08-197">每个控制器方法的路由模板是前缀加上在指定的字符串**路由**属性。</span><span class="sxs-lookup"><span data-stu-id="22f08-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="22f08-198">有关`GetBook`方法，路由模板包括参数化的字符串&quot;{id: int}&quot;，它匹配如果 URI 段包含一个整数值。</span><span class="sxs-lookup"><span data-stu-id="22f08-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="22f08-199">方法</span><span class="sxs-lookup"><span data-stu-id="22f08-199">Method</span></span> | <span data-ttu-id="22f08-200">路由模板</span><span class="sxs-lookup"><span data-stu-id="22f08-200">Route Template</span></span> | <span data-ttu-id="22f08-201">示例 URI</span><span class="sxs-lookup"><span data-stu-id="22f08-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="22f08-202">"api/书籍"</span><span class="sxs-lookup"><span data-stu-id="22f08-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="22f08-203">"api/丛书 / {id: int}"</span><span class="sxs-lookup"><span data-stu-id="22f08-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="22f08-204">获取书详细信息</span><span class="sxs-lookup"><span data-stu-id="22f08-204">Get Book Details</span></span>

<span data-ttu-id="22f08-205">若要获取簿详细信息，客户端将发送 GET 请求到`/api/books/{id}/details`，其中*{id}*是书籍的 ID。</span><span class="sxs-lookup"><span data-stu-id="22f08-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="22f08-206">将以下方法添加到 `BooksController` 类。</span><span class="sxs-lookup"><span data-stu-id="22f08-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="22f08-207">如果请求`/api/books/1/details`，响应类似如下所示：</span><span class="sxs-lookup"><span data-stu-id="22f08-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="22f08-208">获取按风格的丛书</span><span class="sxs-lookup"><span data-stu-id="22f08-208">Get Books By Genre</span></span>

<span data-ttu-id="22f08-209">若要获取在特定风格的书籍的列表，客户端将发送 GET 请求到`/api/books/genre`，其中*流派*是流派的名称。</span><span class="sxs-lookup"><span data-stu-id="22f08-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="22f08-210">（例如 `/api/books/fantasy`。）</span><span class="sxs-lookup"><span data-stu-id="22f08-210">(For example, `/api/books/fantasy`.)</span></span>

<span data-ttu-id="22f08-211">添加以下方法`BooksController`。</span><span class="sxs-lookup"><span data-stu-id="22f08-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="22f08-212">此处我们要定义包含 URI 模板中的 {流派} 参数的路由。</span><span class="sxs-lookup"><span data-stu-id="22f08-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="22f08-213">请注意，Web API 是能够区分这些两个 Uri，并将它们路由到不同的方法：</span><span class="sxs-lookup"><span data-stu-id="22f08-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="22f08-214">这是因为`GetBook`方法包含的"id"段必须为整数值的约束：</span><span class="sxs-lookup"><span data-stu-id="22f08-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="22f08-215">如果请求 /api/books/fantasy，响应如下所示：</span><span class="sxs-lookup"><span data-stu-id="22f08-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="22f08-216">获取由作者的丛书</span><span class="sxs-lookup"><span data-stu-id="22f08-216">Get Books By Author</span></span>

<span data-ttu-id="22f08-217">若要获取特定的作者的书籍的列表，客户端将发送 GET 请求到`/api/authors/id/books`，其中*id*是作者的 ID。</span><span class="sxs-lookup"><span data-stu-id="22f08-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="22f08-218">添加以下方法`BooksController`。</span><span class="sxs-lookup"><span data-stu-id="22f08-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="22f08-219">此示例是有趣因为&quot;丛书&quot;是处理的子资源&quot;作者&quot;。</span><span class="sxs-lookup"><span data-stu-id="22f08-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="22f08-220">此模式是 RESTful Api 中十分常见。</span><span class="sxs-lookup"><span data-stu-id="22f08-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="22f08-221">波形符 （~） 路由模板中的重写中的路由前缀**RoutePrefix**属性。</span><span class="sxs-lookup"><span data-stu-id="22f08-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="22f08-222">获取按发布日期的丛书</span><span class="sxs-lookup"><span data-stu-id="22f08-222">Get Books By Publication Date</span></span>

<span data-ttu-id="22f08-223">若要按发布日期中获取的书籍的列表，客户端将发送 GET 请求到`/api/books/date/yyyy-mm-dd`，其中*yyyy-月-日*是日期。</span><span class="sxs-lookup"><span data-stu-id="22f08-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="22f08-224">下面是执行此操作的一种方法：</span><span class="sxs-lookup"><span data-stu-id="22f08-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="22f08-225">`{pubdate:datetime}`参数约束以匹配**DateTime**值。</span><span class="sxs-lookup"><span data-stu-id="22f08-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="22f08-226">此方法有效，但实际更多的权限不是我们想要。</span><span class="sxs-lookup"><span data-stu-id="22f08-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="22f08-227">例如，这些 Uri 还将匹配路由：</span><span class="sxs-lookup"><span data-stu-id="22f08-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="22f08-228">没有任何错误与允许这些 Uri。</span><span class="sxs-lookup"><span data-stu-id="22f08-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="22f08-229">但是，可以通过将正则表达式约束添加到路由模板，路由限制到特定的格式：</span><span class="sxs-lookup"><span data-stu-id="22f08-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="22f08-230">在窗体中的仅限现在日期&quot;yyyy-月-日&quot;将匹配。</span><span class="sxs-lookup"><span data-stu-id="22f08-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="22f08-231">请注意，我们不使用正则表达式来验证我们获得的实际日期。</span><span class="sxs-lookup"><span data-stu-id="22f08-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="22f08-232">当尝试将转换到的 URI 段 Web API 处理**DateTime**实例。</span><span class="sxs-lookup"><span data-stu-id="22f08-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="22f08-233">无效的日期如"2012年-47-99 将无法进行转换，并且客户端将收到 404 错误。</span><span class="sxs-lookup"><span data-stu-id="22f08-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="22f08-234">你还可以支持斜杠分隔符 (`/api/books/date/yyyy/mm/dd`) 通过添加另一个**[Route]**与其他正则表达式的属性。</span><span class="sxs-lookup"><span data-stu-id="22f08-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="22f08-235">没有细微但重要此详述。</span><span class="sxs-lookup"><span data-stu-id="22f08-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="22f08-236">第二个路由模板具有通配符 (\*) 开头的 {pubdate} 参数：</span><span class="sxs-lookup"><span data-stu-id="22f08-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="22f08-237">这将告知路由引擎该 {pubdate} 应匹配的 uri 的其余部分。</span><span class="sxs-lookup"><span data-stu-id="22f08-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="22f08-238">默认情况下，模板参数与单个 URI 段相匹配。</span><span class="sxs-lookup"><span data-stu-id="22f08-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="22f08-239">在这种情况下，我们希望 {pubdate} 跨越多个 URI 段：</span><span class="sxs-lookup"><span data-stu-id="22f08-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="22f08-240">控制器代码</span><span class="sxs-lookup"><span data-stu-id="22f08-240">Controller Code</span></span>

<span data-ttu-id="22f08-241">下面是 BooksController 类的完整代码。</span><span class="sxs-lookup"><span data-stu-id="22f08-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="22f08-242">总结</span><span class="sxs-lookup"><span data-stu-id="22f08-242">Summary</span></span>

<span data-ttu-id="22f08-243">在设计你的 API 的 Uri 的属性路由提供的更多的控制和更大的灵活性。</span><span class="sxs-lookup"><span data-stu-id="22f08-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
