---
title: "布局"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: f225e2a93edfc552961f9f16294bc0ace6eb0002
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/19/2018
---
# <a name="layout"></a><span data-ttu-id="4f7c9-102">布局</span><span class="sxs-lookup"><span data-stu-id="4f7c9-102">Layout</span></span>

<span data-ttu-id="4f7c9-103">通过[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="4f7c9-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="4f7c9-104">视图频繁共享 visual 和编程元素。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-104">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="4f7c9-105">在本文中，你将了解如何使用通用的布局、 共享指令，以及在 ASP.NET 应用程序运行之前呈现视图的常见代码。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-105">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="4f7c9-106">什么是一种布局</span><span class="sxs-lookup"><span data-stu-id="4f7c9-106">What is a Layout</span></span>

<span data-ttu-id="4f7c9-107">大多数 web 应用都具有它们导航页之间为用户提供一致的体验的常用布局。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-107">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="4f7c9-108">布局通常包括常见的用户界面元素，如应用程序标题、 导航或菜单元素和页脚。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-108">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![页面布局示例](layout/_static/page-layout.png)

<span data-ttu-id="4f7c9-110">在应用内的很多页也经常使用常见的 HTML 结构，如脚本和样式表。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-110">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="4f7c9-111">在所有这些共享元素可定义*布局*文件，然后在应用程序中使用任何视图引用。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-111">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="4f7c9-112">布局减少重复的代码在视图中，帮助他们按照[不重复自己 （模拟） 原则](http://deviq.com/don-t-repeat-yourself/)。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-112">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="4f7c9-113">按照约定，名为 ASP.NET 应用程序的默认布局`_Layout.cshtml`。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-113">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="4f7c9-114">Visual Studio ASP.NET 核心 MVC 项目模板包含在此布局文件`Views/Shared`文件夹：</span><span class="sxs-lookup"><span data-stu-id="4f7c9-114">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![在解决方案资源管理器的视图文件夹](layout/_static/web-project-views.png)

<span data-ttu-id="4f7c9-116">此布局在应用程序定义视图的顶级模板。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-116">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="4f7c9-117">应用不需要一种布局，并应用可以定义多个布局，与指定不同布局的不同视图。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-117">Apps do not require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="4f7c9-118">示例`_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="4f7c9-118">An example `_Layout.cshtml`:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="4f7c9-119">指定布局</span><span class="sxs-lookup"><span data-stu-id="4f7c9-119">Specifying a Layout</span></span>

<span data-ttu-id="4f7c9-120">具有 razor 视图`Layout`属性。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-120">Razor views have a `Layout` property.</span></span> <span data-ttu-id="4f7c9-121">单个视图，通过设置此属性指定布局：</span><span class="sxs-lookup"><span data-stu-id="4f7c9-121">Individual views specify a layout by setting this property:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="4f7c9-122">指定的布局可以使用完整路径 (示例： `/Views/Shared/_Layout.cshtml`) 或部分名称 (示例： `_Layout`)。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-122">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="4f7c9-123">如果提供部分名称，Razor 视图引擎将搜索使用其标准的发现进程的布局文件。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-123">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="4f7c9-124">控制器关联文件夹搜索第一次后, 跟`Shared`文件夹。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-124">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="4f7c9-125">此发现过程等同于用来发现[分部视图](partial.md)。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-125">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="4f7c9-126">默认情况下，必须调用每个布局`RenderBody`。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-126">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="4f7c9-127">任何位置调用`RenderBody`是放置，就将呈现的视图的内容。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-127">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="4f7c9-128">部分</span><span class="sxs-lookup"><span data-stu-id="4f7c9-128">Sections</span></span>

<span data-ttu-id="4f7c9-129">布局 （可选） 可以引用一个或多个*部分*，通过调用`RenderSection`。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-129">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="4f7c9-130">各节提供了一种方法来组织放置某些页元素。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-130">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="4f7c9-131">每次调用`RenderSection`可以指定该部分为必需或可选。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-131">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="4f7c9-132">如果未找到所需的部分，将引发异常。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-132">If a required section is not found, an exception will be thrown.</span></span> <span data-ttu-id="4f7c9-133">单个视图指定要在中部分使用呈现的内容`@section`Razor 语法。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-133">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="4f7c9-134">如果视图可用于定义一个部分，必须呈现 （或将发生错误）。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-134">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="4f7c9-135">示例`@section`视图中的定义：</span><span class="sxs-lookup"><span data-stu-id="4f7c9-135">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="4f7c9-136">在上面的代码中，验证脚本添加到`scripts`包含一个窗体的视图上的部分。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-136">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="4f7c9-137">同一个应用程序中的其他视图可能不需要任何其他脚本，并因此不需要定义脚本部分。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-137">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="4f7c9-138">在视图中定义的部分是仅在其立即进行布局页中可用。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-138">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="4f7c9-139">不能从它们、 将视图组件或查看系统的其他部分引用。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-139">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="4f7c9-140">忽略部分</span><span class="sxs-lookup"><span data-stu-id="4f7c9-140">Ignoring sections</span></span>

<span data-ttu-id="4f7c9-141">默认情况下，正文和内容页中的所有部分必须所有来呈现布局页。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-141">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="4f7c9-142">Razor 视图引擎将强制实施此通过跟踪是否在呈现的正文和每个部分。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-142">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="4f7c9-143">若要让视图引擎以忽略正文或部分，请调用`IgnoreBody`和`IgnoreSection`方法。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-143">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="4f7c9-144">正文和 Razor 页中的每个部分必须呈现或忽略。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-144">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="4f7c9-145">导入共享的指令</span><span class="sxs-lookup"><span data-stu-id="4f7c9-145">Importing Shared Directives</span></span>

<span data-ttu-id="4f7c9-146">视图可以使用 Razor 指令来执行很多东西，例如导入命名空间或执行[依赖关系注入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-146">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="4f7c9-147">可能在一组公共中指定指令由许多视图共享`_ViewImports.cshtml`文件。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-147">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="4f7c9-148">`_ViewImports`文件支持以下指令：</span><span class="sxs-lookup"><span data-stu-id="4f7c9-148">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="4f7c9-149">该文件不支持其他 Razor 功能，如函数和部分定义。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-149">The file does not support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="4f7c9-150">示例`_ViewImports.cshtml`文件：</span><span class="sxs-lookup"><span data-stu-id="4f7c9-150">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="4f7c9-151">`_ViewImports.cshtml`文件的 ASP.NET 核心 MVC 应用程序通常置于`Views`文件夹。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-151">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="4f7c9-152">A`_ViewImports.cshtml`可以将文件放在任何文件夹中，它将仅应用于的情况下该文件夹及其子文件夹内的视图。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-152">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="4f7c9-153">`_ViewImports`根级别中，开始处理文件，然后对截止到每个文件夹的位置的视图本身，以便设置指定的根级别可能重写在文件夹级别。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-153">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="4f7c9-154">例如，如果根级别`_ViewImports.cshtml`文件指定`@model`和`@addTagHelper`，以及另一个`_ViewImports.cshtml`控制器相关视图的文件夹中的文件指定一个不同`@model`和添加另一个`@addTagHelper`，视图将有权访问这两个标记帮助程序，并将使用后者`@model`。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-154">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="4f7c9-155">如果选择多个`_ViewImports.cshtml`文件是否为视图运行、 组合中包含的指令的行为`ViewImports.cshtml`文件将如下所示：</span><span class="sxs-lookup"><span data-stu-id="4f7c9-155">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="4f7c9-156">`@addTagHelper``@removeTagHelper`： 所有运行，按顺序</span><span class="sxs-lookup"><span data-stu-id="4f7c9-156">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="4f7c9-157">`@tagHelperPrefix`： 到视图最近的一个替代的任何其他</span><span class="sxs-lookup"><span data-stu-id="4f7c9-157">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="4f7c9-158">`@model`： 到视图最近的一个替代的任何其他</span><span class="sxs-lookup"><span data-stu-id="4f7c9-158">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="4f7c9-159">`@inherits`： 到视图最近的一个替代的任何其他</span><span class="sxs-lookup"><span data-stu-id="4f7c9-159">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="4f7c9-160">`@using`： 所有都包含;将忽略重复项</span><span class="sxs-lookup"><span data-stu-id="4f7c9-160">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="4f7c9-161">`@inject`： 对于每个属性，则最近到视图将覆盖具有相同的属性名称的任何其他</span><span class="sxs-lookup"><span data-stu-id="4f7c9-161">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="4f7c9-162">运行代码之前每个视图</span><span class="sxs-lookup"><span data-stu-id="4f7c9-162">Running Code Before Each View</span></span>

<span data-ttu-id="4f7c9-163">如果你的代码需要在每个视图之前运行，这应置于`_ViewStart.cshtml`文件。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-163">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="4f7c9-164">按照约定，`_ViewStart.cshtml`文件位于`Views`文件夹。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-164">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="4f7c9-165">中列出的语句`_ViewStart.cshtml`在每个完整的视图 （不布局和非分部视图） 之前运行。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-165">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="4f7c9-166">如[ViewImports.cshtml](xref:mvc/views/layout#viewimports)，`_ViewStart.cshtml`是分层。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-166">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="4f7c9-167">如果`_ViewStart.cshtml`控制器关联的视图文件夹中定义文件，它将运行后的根目录中定义的一个`Views`文件夹 （如果有）。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-167">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="4f7c9-168">示例`_ViewStart.cshtml`文件：</span><span class="sxs-lookup"><span data-stu-id="4f7c9-168">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="4f7c9-169">上述文件指定所有视图将都使用`_Layout.cshtml`布局。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-169">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="4f7c9-170">既不`_ViewStart.cshtml`也不`_ViewImports.cshtml`通常置于`/Views/Shared`文件夹。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-170">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="4f7c9-171">应直接在放置这些文件的应用程序级版本`/Views`文件夹。</span><span class="sxs-lookup"><span data-stu-id="4f7c9-171">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
