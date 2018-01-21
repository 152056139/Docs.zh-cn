---
title: "使用应用程序模型"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/application-model
ms.openlocfilehash: c69dd1cfae713036ce0ee95f70acc162b1e82cb0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/19/2018
---
# <a name="working-with-the-application-model"></a><span data-ttu-id="1563c-102">使用应用程序模型</span><span class="sxs-lookup"><span data-stu-id="1563c-102">Working with the Application Model</span></span>

<span data-ttu-id="1563c-103">通过[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="1563c-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1563c-104">ASP.NET 核心 MVC 定义*应用程序模型*表示的 MVC 应用程序的组件。</span><span class="sxs-lookup"><span data-stu-id="1563c-104">ASP.NET Core MVC defines an *application model* representing the components of an MVC app.</span></span> <span data-ttu-id="1563c-105">你可以读取和处理此模型来修改 MVC 元素的行为方式。</span><span class="sxs-lookup"><span data-stu-id="1563c-105">You can read and manipulate this model to modify how MVC elements behave.</span></span> <span data-ttu-id="1563c-106">默认情况下，MVC 遵循特定的约定来确定哪些类被视为控制器，这些类的方法是操作，以及参数和路由的行为方式。</span><span class="sxs-lookup"><span data-stu-id="1563c-106">By default, MVC follows certain conventions to determine which classes are considered to be controllers, which methods on those classes are actions, and how parameters and routing behave.</span></span> <span data-ttu-id="1563c-107">你可以自定义此行为，以满足您的应用程序需要通过创建你自己的约定和全局或作为属性应用它们。</span><span class="sxs-lookup"><span data-stu-id="1563c-107">You can customize this behavior to suit your app's needs by creating your own conventions and applying them globally or as attributes.</span></span>

## <a name="models-and-providers"></a><span data-ttu-id="1563c-108">模型和提供程序</span><span class="sxs-lookup"><span data-stu-id="1563c-108">Models and Providers</span></span>

<span data-ttu-id="1563c-109">ASP.NET 核心 MVC 应用程序模型包括抽象的接口和描述的 MVC 应用程序的具体实现类。</span><span class="sxs-lookup"><span data-stu-id="1563c-109">The ASP.NET Core MVC application model include both abstract interfaces and concrete implementation classes that describe an MVC application.</span></span> <span data-ttu-id="1563c-110">此模型是 MVC 发现应用程序的控制器、 操作、 操作参数、 路由以及根据默认的约定的筛选器的结果。</span><span class="sxs-lookup"><span data-stu-id="1563c-110">This model is the result of MVC discovering the app's controllers, actions, action parameters, routes, and filters according to default conventions.</span></span> <span data-ttu-id="1563c-111">通过使用应用程序模型，你可以修改应用以遵循不同于默认 MVC 行为的约定。</span><span class="sxs-lookup"><span data-stu-id="1563c-111">By working with the application model, you can modify your app to follow different conventions from the default MVC behavior.</span></span> <span data-ttu-id="1563c-112">参数、 名称、 路由和筛选器是所有用作配置数据操作和控制器。</span><span class="sxs-lookup"><span data-stu-id="1563c-112">The parameters, names, routes, and filters are all used as configuration data for actions and controllers.</span></span>

<span data-ttu-id="1563c-113">ASP.NET 核心 MVC 应用程序模型具有以下结构：</span><span class="sxs-lookup"><span data-stu-id="1563c-113">The ASP.NET Core MVC Application Model has the following structure:</span></span>

* <span data-ttu-id="1563c-114">ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="1563c-114">ApplicationModel</span></span>
    * <span data-ttu-id="1563c-115">控制器 (ControllerModel)</span><span class="sxs-lookup"><span data-stu-id="1563c-115">Controllers (ControllerModel)</span></span>
        * <span data-ttu-id="1563c-116">操作 (ActionModel)</span><span class="sxs-lookup"><span data-stu-id="1563c-116">Actions (ActionModel)</span></span>
            * <span data-ttu-id="1563c-117">参数 (ParameterModel)</span><span class="sxs-lookup"><span data-stu-id="1563c-117">Parameters (ParameterModel)</span></span>

<span data-ttu-id="1563c-118">每个级别的模型有权访问一组公共`Properties`集合和较低的级别可以访问并覆盖由层次结构中更高级别的设置的属性值。</span><span class="sxs-lookup"><span data-stu-id="1563c-118">Each level of the model has access to a common `Properties` collection, and lower levels can access and overwrite property values set by higher levels in the hierarchy.</span></span> <span data-ttu-id="1563c-119">属性将保存到`ActionDescriptor.Properties`何时创建操作。</span><span class="sxs-lookup"><span data-stu-id="1563c-119">The properties are persisted to the `ActionDescriptor.Properties` when the actions are created.</span></span> <span data-ttu-id="1563c-120">然后时处理请求时，任何属性添加或修改的约定可通过访问`ActionContext.ActionDescriptor.Properties`。</span><span class="sxs-lookup"><span data-stu-id="1563c-120">Then when a request is being handled, any properties a convention added or modified can be accessed through `ActionContext.ActionDescriptor.Properties`.</span></span> <span data-ttu-id="1563c-121">使用属性是基于每个操作配置你的筛选器、 模型联编程序等的好办法。</span><span class="sxs-lookup"><span data-stu-id="1563c-121">Using properties is a great way to configure your filters, model binders, etc. on a per-action basis.</span></span>

> [!NOTE]
> <span data-ttu-id="1563c-122">`ActionDescriptor.Properties`完成后应用程序启动后，集合不是线程安全 （对于写入）。</span><span class="sxs-lookup"><span data-stu-id="1563c-122">The `ActionDescriptor.Properties` collection is not thread safe (for writes) once app startup has finished.</span></span> <span data-ttu-id="1563c-123">约定是以安全地将数据添加到此集合的最佳方式。</span><span class="sxs-lookup"><span data-stu-id="1563c-123">Conventions are the best way to safely add data to this collection.</span></span>

### <a name="iapplicationmodelprovider"></a><span data-ttu-id="1563c-124">IApplicationModelProvider</span><span class="sxs-lookup"><span data-stu-id="1563c-124">IApplicationModelProvider</span></span>

<span data-ttu-id="1563c-125">ASP.NET 核心 MVC 加载应用程序模型时使用的提供程序模式，通过定义[IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider)接口。</span><span class="sxs-lookup"><span data-stu-id="1563c-125">ASP.NET Core MVC loads the application model using a provider pattern, defined by the [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interface.</span></span> <span data-ttu-id="1563c-126">本部分介绍如何的内部实现详细信息的一些此提供程序函数。</span><span class="sxs-lookup"><span data-stu-id="1563c-126">This section covers some of the internal implementation details of how this provider functions.</span></span> <span data-ttu-id="1563c-127">这是一个高级的主题-利用应用程序模型的大多数应用程序应使用约定执行此操作。</span><span class="sxs-lookup"><span data-stu-id="1563c-127">This is an advanced topic - most apps that leverage the application model should do so by working with conventions.</span></span>

<span data-ttu-id="1563c-128">实现`IApplicationModelProvider`接口"包装"另一个，与每个实现调用`OnProvidersExecuting`以升序基于其`Order`属性。</span><span class="sxs-lookup"><span data-stu-id="1563c-128">Implementations of the `IApplicationModelProvider` interface "wrap" one another, with each implementation calling `OnProvidersExecuting` in ascending order based on its `Order` property.</span></span> <span data-ttu-id="1563c-129">`OnProvidersExecuted`然后按相反的顺序调用方法。</span><span class="sxs-lookup"><span data-stu-id="1563c-129">The `OnProvidersExecuted` method is then called in reverse order.</span></span> <span data-ttu-id="1563c-130">框架定义多个提供商：</span><span class="sxs-lookup"><span data-stu-id="1563c-130">The framework defines several providers:</span></span>

<span data-ttu-id="1563c-131">第一个 (`Order=-1000`):</span><span class="sxs-lookup"><span data-stu-id="1563c-131">First (`Order=-1000`):</span></span>

* [`DefaultApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

<span data-ttu-id="1563c-132">然后 (`Order=-990`):</span><span class="sxs-lookup"><span data-stu-id="1563c-132">Then (`Order=-990`):</span></span>

* [`AuthorizationApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> <span data-ttu-id="1563c-133">使用相同的值的两个提供程序中的顺序`Order`称为未定义，，因此应不能依靠它。</span><span class="sxs-lookup"><span data-stu-id="1563c-133">The order in which two providers with the same value for `Order` are called is undefined, and therefore should not be relied upon.</span></span>

> [!NOTE]
> <span data-ttu-id="1563c-134">`IApplicationModelProvider`是一个高级的概念工作 framework 作者为扩展。</span><span class="sxs-lookup"><span data-stu-id="1563c-134">`IApplicationModelProvider` is an advanced concept for framework authors to extend.</span></span> <span data-ttu-id="1563c-135">一般情况下，应用应使用约定，并且框架应使用提供程序。</span><span class="sxs-lookup"><span data-stu-id="1563c-135">In general, apps should use conventions and frameworks should use providers.</span></span> <span data-ttu-id="1563c-136">主要不同之处是提供程序始终约定之前运行。</span><span class="sxs-lookup"><span data-stu-id="1563c-136">The key distinction is that providers always run before conventions.</span></span>

<span data-ttu-id="1563c-137">`DefaultApplicationModelProvider`建立许多由 ASP.NET 核心 MVC 的默认行为。</span><span class="sxs-lookup"><span data-stu-id="1563c-137">The `DefaultApplicationModelProvider` establishes many of the default behaviors used by ASP.NET Core MVC.</span></span> <span data-ttu-id="1563c-138">其职责包括：</span><span class="sxs-lookup"><span data-stu-id="1563c-138">Its responsibilities include:</span></span>

* <span data-ttu-id="1563c-139">将全局筛选器添加到上下文</span><span class="sxs-lookup"><span data-stu-id="1563c-139">Adding global filters to the context</span></span>
* <span data-ttu-id="1563c-140">将控制器添加到上下文</span><span class="sxs-lookup"><span data-stu-id="1563c-140">Adding controllers to the context</span></span>
* <span data-ttu-id="1563c-141">作为操作添加公共控制器方法</span><span class="sxs-lookup"><span data-stu-id="1563c-141">Adding public controller methods as actions</span></span>
* <span data-ttu-id="1563c-142">添加到上下文的操作方法参数</span><span class="sxs-lookup"><span data-stu-id="1563c-142">Adding action method parameters to the context</span></span>
* <span data-ttu-id="1563c-143">正在应用路由和其他特性</span><span class="sxs-lookup"><span data-stu-id="1563c-143">Applying route and other attributes</span></span>

<span data-ttu-id="1563c-144">某些内置行为实现的`DefaultApplicationModelProvider`。</span><span class="sxs-lookup"><span data-stu-id="1563c-144">Some built-in behaviors are implemented by the `DefaultApplicationModelProvider`.</span></span> <span data-ttu-id="1563c-145">此提供程序负责构造[ `ControllerModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel)，它又引用[ `ActionModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel)， [ `PropertyModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel)，和[`ParameterModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel)实例。</span><span class="sxs-lookup"><span data-stu-id="1563c-145">This provider is responsible for constructing the [`ControllerModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), which in turn references [`ActionModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [`PropertyModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), and [`ParameterModel`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) instances.</span></span> <span data-ttu-id="1563c-146">`DefaultApplicationModelProvider`类是内部框架实现详细信息，可以将在以后发生更改。</span><span class="sxs-lookup"><span data-stu-id="1563c-146">The `DefaultApplicationModelProvider` class is an internal framework implementation detail that can and will change in the future.</span></span> 

<span data-ttu-id="1563c-147">`AuthorizationApplicationModelProvider`负责应用与关联的行为`AuthorizeFilter`和`AllowAnonymousFilter`属性。</span><span class="sxs-lookup"><span data-stu-id="1563c-147">The `AuthorizationApplicationModelProvider` is responsible for applying the behavior associated with the `AuthorizeFilter` and `AllowAnonymousFilter` attributes.</span></span> <span data-ttu-id="1563c-148">[了解有关这些属性的详细信息](xref:security/authorization/simple)。</span><span class="sxs-lookup"><span data-stu-id="1563c-148">[Learn more about these attributes](xref:security/authorization/simple).</span></span>

<span data-ttu-id="1563c-149">`CorsApplicationModelProvider`实现与关联的行为`IEnableCorsAttribute`和`IDisableCorsAttribute`，和`DisableCorsAuthorizationFilter`。</span><span class="sxs-lookup"><span data-stu-id="1563c-149">The `CorsApplicationModelProvider` implements behavior associated with the `IEnableCorsAttribute` and `IDisableCorsAttribute`, and the `DisableCorsAuthorizationFilter`.</span></span> <span data-ttu-id="1563c-150">[了解有关 CORS 的详细信息](xref:security/cors)。</span><span class="sxs-lookup"><span data-stu-id="1563c-150">[Learn more about CORS](xref:security/cors).</span></span>

## <a name="conventions"></a><span data-ttu-id="1563c-151">约定</span><span class="sxs-lookup"><span data-stu-id="1563c-151">Conventions</span></span>

<span data-ttu-id="1563c-152">应用程序模型定义约定抽象，提供更简单的方法自定义比重写整个模型或提供程序模型的行为。</span><span class="sxs-lookup"><span data-stu-id="1563c-152">The application model defines convention abstractions that provide a simpler way to customize the behavior of the models than overriding the entire model or provider.</span></span> <span data-ttu-id="1563c-153">这些抽象映射是推荐的方法，以修改您的应用程序的行为。</span><span class="sxs-lookup"><span data-stu-id="1563c-153">These abstractions are the recommended way to modify your app's behavior.</span></span> <span data-ttu-id="1563c-154">约定提供让你可以编写将动态应用自定义项的代码的方法。</span><span class="sxs-lookup"><span data-stu-id="1563c-154">Conventions provide a way for you to write code that will dynamically apply customizations.</span></span> <span data-ttu-id="1563c-155">虽然[筛选器](xref:mvc/controllers/filters)提供一种修改框架的行为，自定义设置让你能够控制整个应用连接在一起。</span><span class="sxs-lookup"><span data-stu-id="1563c-155">While [filters](xref:mvc/controllers/filters) provide a means of modifying the framework's behavior, customizations let you control how the whole app is wired together.</span></span>

<span data-ttu-id="1563c-156">提供了以下约定：</span><span class="sxs-lookup"><span data-stu-id="1563c-156">The following conventions are available:</span></span>

* [`IApplicationModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

<span data-ttu-id="1563c-157">通过将它们添加到 MVC 选项或通过实现应用约定`Attribute`s 并将它们应用到控制器、 操作或操作参数 (类似于[ `Filters` ](xref:mvc/controllers/filters))。</span><span class="sxs-lookup"><span data-stu-id="1563c-157">Conventions are applied by adding them to MVC options or by implementing `Attribute`s and applying them to controllers, actions, or action parameters (similar to [`Filters`](xref:mvc/controllers/filters)).</span></span> <span data-ttu-id="1563c-158">与筛选器，不同应用程序正在启动，不每个请求的一部分时，将仅执行约定。</span><span class="sxs-lookup"><span data-stu-id="1563c-158">Unlike filters, conventions are only executed when the app is starting, not as part of each request.</span></span>

### <a name="sample-modifying-the-applicationmodel"></a><span data-ttu-id="1563c-159">示例： 修改 ApplicationModel</span><span class="sxs-lookup"><span data-stu-id="1563c-159">Sample: Modifying the ApplicationModel</span></span>

<span data-ttu-id="1563c-160">以下约定用于将属性添加到应用程序模型。</span><span class="sxs-lookup"><span data-stu-id="1563c-160">The following convention is used to add a property to the application model.</span></span> 

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

<span data-ttu-id="1563c-161">应用程序模型约定将 MVC 添加中时应用作为选项`ConfigureServices`中`Startup`。</span><span class="sxs-lookup"><span data-stu-id="1563c-161">Application model conventions are applied as options when MVC is added in `ConfigureServices` in `Startup`.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

<span data-ttu-id="1563c-162">属性是可从访问`ActionDescriptor`控制器操作中的属性集合：</span><span class="sxs-lookup"><span data-stu-id="1563c-162">Properties are accessible from the `ActionDescriptor` properties collection within controller actions:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a><span data-ttu-id="1563c-163">示例： 修改 ControllerModel 说明</span><span class="sxs-lookup"><span data-stu-id="1563c-163">Sample: Modifying the ControllerModel Description</span></span>

<span data-ttu-id="1563c-164">与前面的示例中，控制器模型还可以修改为包含自定义属性。</span><span class="sxs-lookup"><span data-stu-id="1563c-164">As in the previous example, the controller model can also be modified to include custom properties.</span></span> <span data-ttu-id="1563c-165">这些将具有相同名称的应用程序模型中指定覆盖现有的属性。</span><span class="sxs-lookup"><span data-stu-id="1563c-165">These will override existing properties with the same name specified in the application model.</span></span> <span data-ttu-id="1563c-166">以下约定属性添加在控制器级别的说明：</span><span class="sxs-lookup"><span data-stu-id="1563c-166">The following convention attribute adds a description at the controller level:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

<span data-ttu-id="1563c-167">此约定将应用作为在控制器上的属性。</span><span class="sxs-lookup"><span data-stu-id="1563c-167">This convention is applied as an attribute on a controller.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

<span data-ttu-id="1563c-168">如前面的示例所示相同的方式访问"description"属性。</span><span class="sxs-lookup"><span data-stu-id="1563c-168">The "description" property is accessed in the same manner as in previous examples.</span></span>

### <a name="sample-modifying-the-actionmodel-description"></a><span data-ttu-id="1563c-169">示例： 修改 ActionModel 说明</span><span class="sxs-lookup"><span data-stu-id="1563c-169">Sample: Modifying the ActionModel Description</span></span>

<span data-ttu-id="1563c-170">单独的属性约定可以应用于各项操作，重写已在应用程序或控制器级别应用的行为。</span><span class="sxs-lookup"><span data-stu-id="1563c-170">A separate attribute convention can be applied to individual actions, overriding behavior already applied at the application or controller level.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

<span data-ttu-id="1563c-171">将此应用于前面的示例控制器中的一个操作演示它将控制器级别约定重写：</span><span class="sxs-lookup"><span data-stu-id="1563c-171">Applying this to an action within the previous example's controller demonstrates how it overrides the controller-level convention:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a><span data-ttu-id="1563c-172">示例： 修改 ParameterModel</span><span class="sxs-lookup"><span data-stu-id="1563c-172">Sample: Modifying the ParameterModel</span></span>

<span data-ttu-id="1563c-173">以下约定可以应用于操作参数来修改其`BindingInfo`。</span><span class="sxs-lookup"><span data-stu-id="1563c-173">The following convention can be applied to action parameters to modify their `BindingInfo`.</span></span> <span data-ttu-id="1563c-174">以下约定要求参数是路由参数;其他潜在的绑定源 （例如查询字符串值） 将被忽略。</span><span class="sxs-lookup"><span data-stu-id="1563c-174">The following convention requires that the parameter be a route parameter; other potential binding sources (such as query string values) are ignored.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

<span data-ttu-id="1563c-175">特性可以应用于任何 action 参数：</span><span class="sxs-lookup"><span data-stu-id="1563c-175">The attribute may be applied to any action parameter:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a><span data-ttu-id="1563c-176">示例： 修改 ActionModel 名称</span><span class="sxs-lookup"><span data-stu-id="1563c-176">Sample: Modifying the ActionModel Name</span></span>

<span data-ttu-id="1563c-177">以下约定修改`ActionModel`更新*名称*应用到的操作。</span><span class="sxs-lookup"><span data-stu-id="1563c-177">The following convention modifies the `ActionModel` to update the *name* of the action to which it is applied.</span></span> <span data-ttu-id="1563c-178">作为参数传递给该属性提供了新的名称。</span><span class="sxs-lookup"><span data-stu-id="1563c-178">The new name is provided as a parameter to the attribute.</span></span> <span data-ttu-id="1563c-179">此新的名称使用路由，因此它将会影响使用来访问此操作方法的路由。</span><span class="sxs-lookup"><span data-stu-id="1563c-179">This new name is used by routing, so it will affect the route used to reach this action method.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

<span data-ttu-id="1563c-180">此特性应用于中的操作方法`HomeController`:</span><span class="sxs-lookup"><span data-stu-id="1563c-180">This attribute is applied to an action method in the `HomeController`:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

<span data-ttu-id="1563c-181">即使方法名称`SomeName`，该属性将覆盖使用的方法名称的 MVC 约定并替换与操作名称`MyCoolAction`。</span><span class="sxs-lookup"><span data-stu-id="1563c-181">Even though the method name is `SomeName`, the attribute overrides the MVC convention of using the method name and replaces the action name with `MyCoolAction`.</span></span> <span data-ttu-id="1563c-182">因此，使用来访问此操作的路由是`/Home/MyCoolAction`。</span><span class="sxs-lookup"><span data-stu-id="1563c-182">Thus, the route used to reach this action is `/Home/MyCoolAction`.</span></span>

> [!NOTE]
> <span data-ttu-id="1563c-183">此示例是实质上是不同于使用内置[ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute)属性。</span><span class="sxs-lookup"><span data-stu-id="1563c-183">This example is essentially the same as using the built-in [ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) attribute.</span></span>

### <a name="sample-custom-routing-convention"></a><span data-ttu-id="1563c-184">示例： 自定义路由约定</span><span class="sxs-lookup"><span data-stu-id="1563c-184">Sample: Custom Routing Convention</span></span>

<span data-ttu-id="1563c-185">你可以使用`IApplicationModelConvention`以自定义路由的工作方式。</span><span class="sxs-lookup"><span data-stu-id="1563c-185">You can use an `IApplicationModelConvention` to customize how routing works.</span></span> <span data-ttu-id="1563c-186">例如，以下约定将将控制器的命名空间合并到其路由，替换`.`命名空间具有`/`路线中：</span><span class="sxs-lookup"><span data-stu-id="1563c-186">For example, the following convention will incorporate Controllers' namespaces into their routes, replacing `.` in the namespace with `/` in the route:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

<span data-ttu-id="1563c-187">约定添加为中启动的一个选项。</span><span class="sxs-lookup"><span data-stu-id="1563c-187">The convention is added as an option in Startup.</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> <span data-ttu-id="1563c-188">你可以添加到约定你[中间件](xref:fundamentals/middleware)通过访问`MvcOptions`使用`services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span><span class="sxs-lookup"><span data-stu-id="1563c-188">You can add conventions to your [middleware](xref:fundamentals/middleware) by accessing `MvcOptions` using `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`</span></span>

<span data-ttu-id="1563c-189">此示例适用于未使用其中控制器的名称中包含"Namespace"的属性路由的路由此约定。</span><span class="sxs-lookup"><span data-stu-id="1563c-189">This sample applies this convention to routes that are not using attribute routing where the controller has  "Namespace" in its name.</span></span> <span data-ttu-id="1563c-190">以下控制器演示了此约定：</span><span class="sxs-lookup"><span data-stu-id="1563c-190">The following controller demonstrates this convention:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a><span data-ttu-id="1563c-191">应用程序模型中 WebApiCompatShim 的使用情况</span><span class="sxs-lookup"><span data-stu-id="1563c-191">Application Model Usage in WebApiCompatShim</span></span>

<span data-ttu-id="1563c-192">ASP.NET 核心 MVC 使用 ASP.NET Web API 2 中不同的约定集。</span><span class="sxs-lookup"><span data-stu-id="1563c-192">ASP.NET Core MVC uses a different set of conventions from ASP.NET Web API 2.</span></span> <span data-ttu-id="1563c-193">使用自定义的约定，可以修改 ASP.NET 核心 MVC 应用程序的行为，使其与 Web API 应用的一致。</span><span class="sxs-lookup"><span data-stu-id="1563c-193">Using custom conventions, you can modify an ASP.NET Core MVC app's behavior to be consistent with that of a Web API app.</span></span> <span data-ttu-id="1563c-194">Microsoft 附带[WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/)专为此目的。</span><span class="sxs-lookup"><span data-stu-id="1563c-194">Microsoft ships the [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) specifically for this purpose.</span></span>

> [!NOTE]
> <span data-ttu-id="1563c-195">详细了解[从 ASP.NET Web API 迁移](xref:migration/webapi)。</span><span class="sxs-lookup"><span data-stu-id="1563c-195">Learn more about [migrating from ASP.NET Web API](xref:migration/webapi).</span></span>

<span data-ttu-id="1563c-196">若要使用 Web API 兼容性填充码，你需要将包添加到你的项目，然后通过调用将约定添加到 MVC`AddWebApiConventions`中`Startup`:</span><span class="sxs-lookup"><span data-stu-id="1563c-196">To use the Web API Compatibility Shim, you need to add the package to your project and then add the conventions to MVC by calling `AddWebApiConventions` in `Startup`:</span></span>

```c#
services.AddMvc().AddWebApiConventions();
```

<span data-ttu-id="1563c-197">提供的填充码的约定仅适用于部分应用程序已应用于它们的某些属性。</span><span class="sxs-lookup"><span data-stu-id="1563c-197">The conventions provided by the shim are only applied to parts of the app that have had certain attributes applied to them.</span></span> <span data-ttu-id="1563c-198">控制哪些控制器应具有由填充码的约定修改其约定使用以下四个属性：</span><span class="sxs-lookup"><span data-stu-id="1563c-198">The following four attributes are used to control which controllers should have their conventions modified by the shim's conventions:</span></span>

* [<span data-ttu-id="1563c-199">UseWebApiActionConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="1563c-199">UseWebApiActionConventionsAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [<span data-ttu-id="1563c-200">UseWebApiOverloadingAttribute</span><span class="sxs-lookup"><span data-stu-id="1563c-200">UseWebApiOverloadingAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [<span data-ttu-id="1563c-201">UseWebApiParameterConventionsAttribute</span><span class="sxs-lookup"><span data-stu-id="1563c-201">UseWebApiParameterConventionsAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [<span data-ttu-id="1563c-202">UseWebApiRoutesAttribute</span><span class="sxs-lookup"><span data-stu-id="1563c-202">UseWebApiRoutesAttribute</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a><span data-ttu-id="1563c-203">操作约定</span><span class="sxs-lookup"><span data-stu-id="1563c-203">Action Conventions</span></span>

<span data-ttu-id="1563c-204">`UseWebApiActionConventionsAttribute`用于将 HTTP 方法映射到操作根据其名称 (例如，`Get`将映射为`HttpGet`)。</span><span class="sxs-lookup"><span data-stu-id="1563c-204">The `UseWebApiActionConventionsAttribute` is used to map the HTTP method to actions based on their name (for instance, `Get` would map to `HttpGet`).</span></span> <span data-ttu-id="1563c-205">它仅适用于不使用的属性路由的操作。</span><span class="sxs-lookup"><span data-stu-id="1563c-205">It only applies to actions that do not use attribute routing.</span></span>

### <a name="overloading"></a><span data-ttu-id="1563c-206">重载</span><span class="sxs-lookup"><span data-stu-id="1563c-206">Overloading</span></span>

<span data-ttu-id="1563c-207">`UseWebApiOverloadingAttribute`用于将应用`WebApiOverloadingApplicationModelConvention`约定。</span><span class="sxs-lookup"><span data-stu-id="1563c-207">The `UseWebApiOverloadingAttribute` is used to apply the `WebApiOverloadingApplicationModelConvention` convention.</span></span> <span data-ttu-id="1563c-208">此约定将添加`OverloadActionConstraint`到操作选择过程中，这就限制了为其请求满足所有的非可选参数与候选操作。</span><span class="sxs-lookup"><span data-stu-id="1563c-208">This convention adds an `OverloadActionConstraint` to the action selection process, which limits candidate actions to those for which the request satisfies all non-optional parameters.</span></span>

### <a name="parameter-conventions"></a><span data-ttu-id="1563c-209">参数约定</span><span class="sxs-lookup"><span data-stu-id="1563c-209">Parameter Conventions</span></span>

<span data-ttu-id="1563c-210">`UseWebApiParameterConventionsAttribute`用于将应用`WebApiParameterConventionsApplicationModelConvention`操作约定。</span><span class="sxs-lookup"><span data-stu-id="1563c-210">The `UseWebApiParameterConventionsAttribute` is used to apply the `WebApiParameterConventionsApplicationModelConvention` action convention.</span></span> <span data-ttu-id="1563c-211">此约定指定，使用作为操作参数的简单类型绑定的 URI 中默认情况下，而复杂类型从请求正文绑定。</span><span class="sxs-lookup"><span data-stu-id="1563c-211">This convention specifies that simple types used as action parameters are bound from the URI by default, while complex types are bound from the request body.</span></span>

### <a name="routes"></a><span data-ttu-id="1563c-212">路由</span><span class="sxs-lookup"><span data-stu-id="1563c-212">Routes</span></span>

<span data-ttu-id="1563c-213">`UseWebApiRoutesAttribute`控件是否`WebApiApplicationModelConvention`应用控制器约定。</span><span class="sxs-lookup"><span data-stu-id="1563c-213">The `UseWebApiRoutesAttribute` controls whether the `WebApiApplicationModelConvention` controller convention is applied.</span></span> <span data-ttu-id="1563c-214">启用时，此约定用于添加对支持[区域](xref:mvc/controllers/areas)为该路由。</span><span class="sxs-lookup"><span data-stu-id="1563c-214">When enabled, this convention is used to add support for [areas](xref:mvc/controllers/areas) to the route.</span></span>

<span data-ttu-id="1563c-215">兼容性包包括一组的约定，除了`System.Web.Http.ApiController`基类来替换提供的 Web API。</span><span class="sxs-lookup"><span data-stu-id="1563c-215">In addition to a set of conventions, the compatibility package includes a `System.Web.Http.ApiController` base class that replaces the one provided by Web API.</span></span> <span data-ttu-id="1563c-216">这允许你为 Web API 和继承编写的控制器从其`ApiController`以按照已设计会在 ASP.NET 核心 MVC 运行时的工作。</span><span class="sxs-lookup"><span data-stu-id="1563c-216">This allows your controllers written for Web API and inheriting from its `ApiController` to work as they were designed, while running on ASP.NET Core MVC.</span></span> <span data-ttu-id="1563c-217">此基本控制器类修饰的所有`UseWebApi*`上面列出的属性。</span><span class="sxs-lookup"><span data-stu-id="1563c-217">This base controller class is decorated with all of the `UseWebApi*` attributes listed above.</span></span> <span data-ttu-id="1563c-218">`ApiController`公开属性、 方法和与 Web API 中兼容的结果类型。</span><span class="sxs-lookup"><span data-stu-id="1563c-218">The `ApiController` exposes properties, methods, and result types that are compatible with those found in Web API.</span></span>

## <a name="using-apiexplorer-to-document-your-app"></a><span data-ttu-id="1563c-219">使用 ApiExplorer 记录您的应用程序</span><span class="sxs-lookup"><span data-stu-id="1563c-219">Using ApiExplorer to Document Your App</span></span>

<span data-ttu-id="1563c-220">应用程序模型公开了[ `ApiExplorer` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel)可用于遍历应用程序的结构每个级别的属性。</span><span class="sxs-lookup"><span data-stu-id="1563c-220">The application model exposes an [`ApiExplorer`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) property at each level that can be used to traverse the app's structure.</span></span> <span data-ttu-id="1563c-221">这可用于[为你的 Web Api 使用 Swagger 之类的工具生成的帮助页](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger)。</span><span class="sxs-lookup"><span data-stu-id="1563c-221">This can be used to [generate help pages for your Web APIs using tools like Swagger](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="1563c-222">`ApiExplorer`属性可公开`IsVisible`属性，可设置为指定应公开您的应用程序模型中的哪些部分。</span><span class="sxs-lookup"><span data-stu-id="1563c-222">The `ApiExplorer` property exposes an `IsVisible` property that can be set to specify which parts of your app's model should be exposed.</span></span> <span data-ttu-id="1563c-223">你可以配置此设置使用约定：</span><span class="sxs-lookup"><span data-stu-id="1563c-223">You can configure this setting using a convention:</span></span>

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

<span data-ttu-id="1563c-224">使用此方法 （和其他的约定，如果需要），可以启用或禁用在您的应用程序中的 API 在任何级别的可见性。</span><span class="sxs-lookup"><span data-stu-id="1563c-224">Using this approach (and additional conventions if required), you can enable or disable API visibility at any level within your app.</span></span> 
