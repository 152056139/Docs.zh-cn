---
title: "使用 ASP.NET Core 和 Visual Studio for Windows 创建 Web API"
author: rick-anderson
description: "使用 ASP.NET Core MVC 和 Visual Studio for Windows 生成 Web API"
keywords: "ASP.NET Core, WebAPI, Web API, REST, HTTP, 服务, HTTP 服务"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 3ef6fb26eab123c9f6f8275ee1d979b090db0413
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="4998c-104">使用 ASP.NET Core 和 Visual Studio for Windows 创建 Web API</span><span class="sxs-lookup"><span data-stu-id="4998c-104">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="4998c-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="4998c-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="4998c-106">本教程构建了用于管理“待办事项”列表的 Web API。</span><span class="sxs-lookup"><span data-stu-id="4998c-106">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="4998c-107">未创建用户界面 (UI)。</span><span class="sxs-lookup"><span data-stu-id="4998c-107">A user interface (UI) is not created.</span></span>

<span data-ttu-id="4998c-108">本教程提供 3 个版本：</span><span class="sxs-lookup"><span data-stu-id="4998c-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="4998c-109">Windows：使用 Visual Studio for Windows 创建 Web API（本教程）</span><span class="sxs-lookup"><span data-stu-id="4998c-109">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="4998c-110">macOS：[使用 Visual Studio for Mac 创建 Web API](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="4998c-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="4998c-111">macOS、Linux、Windows：[使用 Visual Studio Code 创建 Web API](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="4998c-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="4998c-112">先决条件</span><span class="sxs-lookup"><span data-stu-id="4998c-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="4998c-113">请参阅[此 PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) 了解有关 ASP.NET Core 1.1 版本的信息。</span><span class="sxs-lookup"><span data-stu-id="4998c-113">See [this PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="4998c-114">创建项目</span><span class="sxs-lookup"><span data-stu-id="4998c-114">Create the project</span></span>

<span data-ttu-id="4998c-115">在 Visual Studio 中，选择“文件”菜单 >“新建” > “项目”。</span><span class="sxs-lookup"><span data-stu-id="4998c-115">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="4998c-116">选择“ASP.NET Core Web 应用程序(.NET Core)”项目模板。</span><span class="sxs-lookup"><span data-stu-id="4998c-116">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="4998c-117">将此项目命名为 `TodoApi` ，然后选择“确定”。</span><span class="sxs-lookup"><span data-stu-id="4998c-117">Name the project `TodoApi` and select **OK**.</span></span>

![“新建项目”对话框](first-web-api/_static/new-project.png)

<span data-ttu-id="4998c-119">在“新建 ASP.NET Core Web 应用程序 - TodoApi”对话框中，选择“Web API”模板。</span><span class="sxs-lookup"><span data-stu-id="4998c-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="4998c-120">选择“确定”。</span><span class="sxs-lookup"><span data-stu-id="4998c-120">Select **OK**.</span></span> <span data-ttu-id="4998c-121">请不要选择“启用 Docker 支持”。</span><span class="sxs-lookup"><span data-stu-id="4998c-121">Do **not** select **Enable Docker Support**.</span></span>

![从 ASP.NET Core 模板选择的 Web API 项目模板的“新建 ASP.NET Web 应用程序”对话框](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="4998c-123">启动应用</span><span class="sxs-lookup"><span data-stu-id="4998c-123">Launch the app</span></span>

<span data-ttu-id="4998c-124">在 Visual Studio 中，按 CTRL+F5 启动应用。</span><span class="sxs-lookup"><span data-stu-id="4998c-124">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="4998c-125">Visual Studio 启动浏览器并导航到 `http://localhost:port/api/values`，其中“端口”是随机选择的端口号。</span><span class="sxs-lookup"><span data-stu-id="4998c-125">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="4998c-126">Chrome、Microsoft Edge 和 Firefox 将显示以下输出：</span><span class="sxs-lookup"><span data-stu-id="4998c-126">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="4998c-127">添加模型类</span><span class="sxs-lookup"><span data-stu-id="4998c-127">Add a model class</span></span>

<span data-ttu-id="4998c-128">模型是表示应用程序中的数据的对象。</span><span class="sxs-lookup"><span data-stu-id="4998c-128">A model is an object that represents the data in the app.</span></span> <span data-ttu-id="4998c-129">在此示例中，唯一的模型是待办事项。</span><span class="sxs-lookup"><span data-stu-id="4998c-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="4998c-130">添加名为“Models”的文件夹。</span><span class="sxs-lookup"><span data-stu-id="4998c-130">Add a folder named "Models".</span></span> <span data-ttu-id="4998c-131">在解决方案资源管理器中，右键单击项目。</span><span class="sxs-lookup"><span data-stu-id="4998c-131">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="4998c-132">选择“添加” > “新建文件夹”。</span><span class="sxs-lookup"><span data-stu-id="4998c-132">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="4998c-133">将文件夹命名为“Models”。</span><span class="sxs-lookup"><span data-stu-id="4998c-133">Name the folder *Models*.</span></span>

<span data-ttu-id="4998c-134">注意：模型类可以出现在项目的任意位置。</span><span class="sxs-lookup"><span data-stu-id="4998c-134">Note: The model classes go anywhere in in the project.</span></span> <span data-ttu-id="4998c-135">Models 文件夹按约定用于模型类。</span><span class="sxs-lookup"><span data-stu-id="4998c-135">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="4998c-136">添加 `TodoItem` 类。</span><span class="sxs-lookup"><span data-stu-id="4998c-136">Add a `TodoItem` class.</span></span> <span data-ttu-id="4998c-137">右键单击“Models”文件夹，然后选择“添加” > “类”。</span><span class="sxs-lookup"><span data-stu-id="4998c-137">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="4998c-138">将此类命名为 `TodoItem`，然后选择“添加”。</span><span class="sxs-lookup"><span data-stu-id="4998c-138">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="4998c-139">使用以下代码更新 `TodoItem` 类：</span><span class="sxs-lookup"><span data-stu-id="4998c-139">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="4998c-140">创建 `TodoItem` 时，数据库将生成 `Id`。</span><span class="sxs-lookup"><span data-stu-id="4998c-140">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="4998c-141">创建数据库上下文</span><span class="sxs-lookup"><span data-stu-id="4998c-141">Create the database context</span></span>

<span data-ttu-id="4998c-142">数据库上下文是为给定数据模型协调实体框架功能的主类。</span><span class="sxs-lookup"><span data-stu-id="4998c-142">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="4998c-143">此类由 `Microsoft.EntityFrameworkCore.DbContext` 类派生而来。</span><span class="sxs-lookup"><span data-stu-id="4998c-143">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="4998c-144">添加 `TodoContext` 类。</span><span class="sxs-lookup"><span data-stu-id="4998c-144">Add a `TodoContext` class.</span></span> <span data-ttu-id="4998c-145">右键单击“Models”文件夹，然后选择“添加” > “类”。</span><span class="sxs-lookup"><span data-stu-id="4998c-145">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="4998c-146">将此类命名为 `TodoContext`，然后选择“添加”。</span><span class="sxs-lookup"><span data-stu-id="4998c-146">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="4998c-147">将该类替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="4998c-147">Replace the class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="4998c-148">添加控制器</span><span class="sxs-lookup"><span data-stu-id="4998c-148">Add a controller</span></span>

<span data-ttu-id="4998c-149">在解决方案资源管理器中，右键单击“控制器”文件夹。</span><span class="sxs-lookup"><span data-stu-id="4998c-149">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="4998c-150">选择“添加” > “新建项”。</span><span class="sxs-lookup"><span data-stu-id="4998c-150">Select **Add** > **New Item**.</span></span> <span data-ttu-id="4998c-151">在“添加新项”对话框中，选择“Web API 控制器类”模板。</span><span class="sxs-lookup"><span data-stu-id="4998c-151">In the **Add New Item** dialog, select the **Web API Controller Class** template.</span></span> <span data-ttu-id="4998c-152">将此类命名为 `TodoController`。</span><span class="sxs-lookup"><span data-stu-id="4998c-152">Name the class `TodoController`.</span></span>

![“添加新项”对话框，“控制器”显示在搜索框中，并且“Web API 控制器”已选中](first-web-api/_static/new_controller.png)

<span data-ttu-id="4998c-154">将该类替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="4998c-154">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="4998c-155">启动应用</span><span class="sxs-lookup"><span data-stu-id="4998c-155">Launch the app</span></span>

<span data-ttu-id="4998c-156">在 Visual Studio 中，按 CTRL+F5 启动应用。</span><span class="sxs-lookup"><span data-stu-id="4998c-156">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="4998c-157">Visual Studio 启动浏览器并导航到 `http://localhost:port/api/values`，其中“端口”是随机选择的端口号。</span><span class="sxs-lookup"><span data-stu-id="4998c-157">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="4998c-158">导航到位于 `http://localhost:port/api/todo` 的 `Todo` 控制器。</span><span class="sxs-lookup"><span data-stu-id="4998c-158">Navigate to the `Todo` controller at `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

