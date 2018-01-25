---
title: "ASP.NET 核心 MVC 中的格式设置响应数据"
author: ardalis
description: "了解如何设置 ASP.NET 核心 mvc 响应数据的格式。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ddda494e0db22031af9d20325e14e8458756cbfd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a><span data-ttu-id="3a7d6-103">ASP.NET 核心 MVC 中的格式设置响应数据简介</span><span class="sxs-lookup"><span data-stu-id="3a7d6-103">Introduction to formatting response data in ASP.NET Core MVC</span></span>

<span data-ttu-id="3a7d6-104">通过[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="3a7d6-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="3a7d6-105">ASP.NET 核心 MVC 具有用于格式设置响应数据，使用固定的格式或以响应客户端规范的内置支持。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="3a7d6-106">[查看或从 GitHub 下载示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample)。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-106">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="3a7d6-107">格式特定的操作结果</span><span class="sxs-lookup"><span data-stu-id="3a7d6-107">Format-Specific Action Results</span></span>

<span data-ttu-id="3a7d6-108">某些操作结果类型是特定于特定的格式，如`JsonResult`和`ContentResult`。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="3a7d6-109">操作可以返回特定结果始终格式化以特定方式。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="3a7d6-110">例如，返回`JsonResult`将返回 JSON 格式的数据，而不考虑客户端首选项。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="3a7d6-111">同样，返回`ContentResult`（如将只返回一个字符串） 将返回纯文本格式的字符串数据。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="3a7d6-112">操作不需要返回任何特定的类型;MVC 支持任何对象的返回值。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="3a7d6-113">如果操作返回`IActionResult`实现和控制器继承自`Controller`，开发人员有许多与许多选项对应的帮助器方法。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="3a7d6-114">从返回对象的操作的结果并非`IActionResult`将使用相应的序列化类型`IOutputFormatter`实现。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="3a7d6-115">若要从控制器继承自特定的格式返回数据`Controller`基类，可以使用内置的帮助器方法`Json`返回 JSON 和`Content`纯文本。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="3a7d6-116">操作方法应返回特定结果类型 (例如， `JsonResult`) 或`IActionResult`。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="3a7d6-117">返回 JSON 格式的数据：</span><span class="sxs-lookup"><span data-stu-id="3a7d6-117">Returning JSON-formatted data:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="3a7d6-118">此操作的示例响应：</span><span class="sxs-lookup"><span data-stu-id="3a7d6-118">Sample response from this action:</span></span>

![网络选项卡中显示的响应的内容类型的 Microsoft Edge 开发人员工具为 application/json](formatting/_static/json-response.png)

<span data-ttu-id="3a7d6-120">请注意，响应的内容类型是`application/json`，同时在列表中的网络请求和响应标头部分中所示。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="3a7d6-121">另请注意 （在此情况下，Microsoft Edge） 中的请求标头部分的 Accept 标头中的浏览器呈现选项的列表。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="3a7d6-122">当前技术正在忽略此标头;下面将讨论 obeying 它。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="3a7d6-123">若要返回格式化的纯文本数据，使用`ContentResult`和`Content`帮助器：</span><span class="sxs-lookup"><span data-stu-id="3a7d6-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="3a7d6-124">来自此操作的响应：</span><span class="sxs-lookup"><span data-stu-id="3a7d6-124">A response from this action:</span></span>

![网络选项卡中显示的内容类型的 Microsoft Edge 开发人员工具是响应的文本/无格式](formatting/_static/text-response.png)

<span data-ttu-id="3a7d6-126">请注意这种情况下`Content-Type`返回`text/plain`。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="3a7d6-127">也可以实现此相同的行为使用只是一种字符串响应类型：</span><span class="sxs-lookup"><span data-stu-id="3a7d6-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="3a7d6-128">对于具有多个重要操作返回类型或选项 （例如，根据执行的操作的结果的不同 HTTP 状态代码），应尽量使用`IActionResult`用作返回类型。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="3a7d6-129">内容协商</span><span class="sxs-lookup"><span data-stu-id="3a7d6-129">Content Negotiation</span></span>

<span data-ttu-id="3a7d6-130">内容协商 (*conneg*简称) 客户端指定时发生[Accept 标头](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="3a7d6-131">使用 ASP.NET 核心 MVC 的默认格式为 JSON。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="3a7d6-132">由实现内容协商`ObjectResult`。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="3a7d6-133">它还将内置在特定的操作结果的帮助器方法从返回的状态代码 (的所有基于`ObjectResult`)。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-133">It's also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="3a7d6-134">你还可以返回的模型类型 （已定义为你的数据传输类型的类） 和框架将自动换行在`ObjectResult`为你。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="3a7d6-135">下面的操作方法使用`Ok`和`NotFound`帮助器方法：</span><span class="sxs-lookup"><span data-stu-id="3a7d6-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="3a7d6-136">将返回 JSON 格式的响应，除非另一种格式请求和服务器可以返回请求的格式。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="3a7d6-137">你可以使用之类的工具[Fiddler](http://www.telerik.com/fiddler)若要创建包含 Accept 标头的请求，并指定另一种格式。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-137">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="3a7d6-138">在此情况下，如果服务器具有*格式化程序*可以生成能产生中请求的格式的响应，将返回的结果中的客户端首选格式。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Fiddler 控制台显示手动创建获取请求的 Accept 标头值为应用程序/xml](formatting/_static/fiddler-composer.png)

<span data-ttu-id="3a7d6-140">在上面的屏幕截图，Fiddler Composer 已用于生成请求，指定`Accept: application/xml`。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="3a7d6-141">默认情况下，ASP.NET 核心 MVC 仅支持 JSON，因此即使指定另一种格式，则返回的结果时，仍 JSON 格式。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="3a7d6-142">你将了解如何在下一部分中添加其他格式化程序。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="3a7d6-143">控制器操作可以返回 POCOs （普通旧 CLR 对象），在这种情况下将自动创建 ASP.NET MVC`ObjectResult`为你包装对象。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET MVC will automatically create an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="3a7d6-144">客户端将获得格式化的序列化的对象 （JSON 格式是默认设置; 你可以配置 XML 或其他格式）。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="3a7d6-145">如果对象被返回为`null`，则框架将返回`204 No Content`响应。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="3a7d6-146">返回的对象类型：</span><span class="sxs-lookup"><span data-stu-id="3a7d6-146">Returning an object type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="3a7d6-147">在示例中，有效作者别名的请求将收到一个 200 OK 响应使用作者的数据。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="3a7d6-148">无效的别名的请求将收到 204 无内容的响应。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="3a7d6-149">显示在 XML 和 JSON 格式的响应的屏幕截图所示。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="3a7d6-150">内容协商过程</span><span class="sxs-lookup"><span data-stu-id="3a7d6-150">Content Negotiation Process</span></span>

<span data-ttu-id="3a7d6-151">内容*协商*才会进行如果`Accept`标头将显示在请求中。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="3a7d6-152">当请求包含 accept 标头时，框架将枚举按优先顺序 accept 标头中的媒体类型，并将尝试查找可以生成指定 accept 标头的格式之一的响应的格式化程序。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="3a7d6-153">以防没有格式化程序找到的可以满足客户端的请求，则框架将尝试找到的第一个格式化程序可以生成响应 (除非开发人员机构上配置了选项`MvcOptions`返回 406 不可接受相反)。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="3a7d6-154">如果此请求指定的 XML，但尚未配置 XML 格式化程序，则将使用 JSON 格式化程序。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="3a7d6-155">一般来说，如果配置未格式化程序，可以提供请求的格式，然后使用的第一个格式化程序可以设置对象的格式。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="3a7d6-156">如果未不提供任何标头，则将使用的第一个格式化程序可以处理要返回的对象要序列化响应。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="3a7d6-157">在这种情况下，没有任何协商发生-服务器确定它将使用何种格式。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="3a7d6-158">如果 Accept 标头包含`*/*`，将忽略标头，除非`RespectBrowserAcceptHeader`设置为 true 上`MvcOptions`。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="3a7d6-159">浏览器和内容协商</span><span class="sxs-lookup"><span data-stu-id="3a7d6-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="3a7d6-160">与典型的 API 客户端不同 web 浏览器倾向于提供`Accept`包括多种不同的格式，其中包括通配符的标头。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="3a7d6-161">默认情况下，当框架检测到该请求是否来自浏览器中，它将忽略`Accept`标头和改为返回应用程序中的内容配置的默认格式 (JSON 除非另外配置)。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="3a7d6-162">使用不同的浏览器要使用 Api 时，这会提供更一致的体验。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="3a7d6-163">如果您希望使用应用程序 honor 浏览器接受标头，则可以配置此 MVC 的配置过程中通过设置`RespectBrowserAcceptHeader`到`true`中`ConfigureServices`中的方法*Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="3a7d6-164">配置格式化程序</span><span class="sxs-lookup"><span data-stu-id="3a7d6-164">Configuring Formatters</span></span>

<span data-ttu-id="3a7d6-165">如果你的应用程序需要支持 JSON 的默认值之外的其他格式，你可以添加 NuGet 包和配置 MVC 可支持它们。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="3a7d6-166">没有为输入和输出的单独格式化程序。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="3a7d6-167">输入的格式化程序使用的[模型绑定](model-binding.md); 输出格式化程序用于格式化响应。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-167">Input formatters are used by [Model Binding](model-binding.md); output formatters are used to format responses.</span></span> <span data-ttu-id="3a7d6-168">你还可以配置[自定义格式化程序](../advanced/custom-formatters.md)。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-168">You can also configure [Custom Formatters](../advanced/custom-formatters.md).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="3a7d6-169">添加 XML 格式的支持</span><span class="sxs-lookup"><span data-stu-id="3a7d6-169">Adding XML Format Support</span></span>

<span data-ttu-id="3a7d6-170">若要添加的 XML 格式的支持，请安装`Microsoft.AspNetCore.Mvc.Formatters.Xml`NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-170">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="3a7d6-171">向 MVC 的配置中添加 XmlSerializerFormatters *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3a7d6-171">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="3a7d6-172">或者，你可以添加只输出格式化程序：</span><span class="sxs-lookup"><span data-stu-id="3a7d6-172">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="3a7d6-173">这两种方法将序列化结果使用`System.Xml.Serialization.XmlSerializer`。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-173">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="3a7d6-174">如果你愿意，你可以使用`System.Runtime.Serialization.DataContractSerializer`加上其关联的格式化程序：</span><span class="sxs-lookup"><span data-stu-id="3a7d6-174">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="3a7d6-175">一旦你已添加对 XML 格式的支持，控制器方法将返回基于请求的相应格式`Accept`标头，作为示例演示了此 Fiddler:</span><span class="sxs-lookup"><span data-stu-id="3a7d6-175">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Fiddler 控制台： 请求的原始选项卡显示 Accept 标头值为 application/xml。](formatting/_static/xml-response.png)

<span data-ttu-id="3a7d6-178">你可以将原始 GET 请求进行的检查器选项卡中看到`Accept: application/xml`标头集。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-178">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="3a7d6-179">响应窗格显示`Content-Type: application/xml`标头，与`Author`对象序列化为 XML。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-179">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="3a7d6-180">使用编辑器选项卡来修改指定的请求`application/json`中`Accept`标头。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-180">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="3a7d6-181">执行请求，并且响应将被格式化为 JSON:</span><span class="sxs-lookup"><span data-stu-id="3a7d6-181">Execute the request, and the response will be formatted as JSON:</span></span>

![Fiddler 控制台： 请求的原始选项卡显示 Accept 标头值为 application/json。](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="3a7d6-184">在此屏幕截图中，你可以看到请求设置标头的`Accept: application/json`并响应指定与相同其`Content-Type`。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-184">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="3a7d6-185">`Author`对象显示 JSON 格式的响应正文中。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-185">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="3a7d6-186">强制某一特定格式</span><span class="sxs-lookup"><span data-stu-id="3a7d6-186">Forcing a Particular Format</span></span>

<span data-ttu-id="3a7d6-187">如果你想要限制特定操作的响应格式可以你可以将应用`[Produces]`筛选器。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-187">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="3a7d6-188">`[Produces]`筛选器指定的特定操作 （或控制器） 的响应格式。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-188">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="3a7d6-189">如大多数[筛选器](../controllers/filters.md)，这可以在操作、 控制器或全局范围内应用。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-189">Like most [Filters](../controllers/filters.md), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="3a7d6-190">`[Produces]`筛选器将强制中的所有操作`AuthorsController`返回 JSON 格式的响应，即使其他格式化程序已配置为应用程序和提供客户端`Accept`标头请求不同，可用格式。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-190">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="3a7d6-191">请参阅[筛选器](../controllers/filters.md)若要了解详细信息，包括如何执行全局应用筛选器。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-191">See [Filters](../controllers/filters.md) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="3a7d6-192">特殊大小写的格式化程序</span><span class="sxs-lookup"><span data-stu-id="3a7d6-192">Special Case Formatters</span></span>

<span data-ttu-id="3a7d6-193">某些特殊的情况下是使用内置的格式化程序实现的。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-193">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="3a7d6-194">默认情况下，`string`返回类型将被格式化为*文本/无格式*(*文本/html*如果请求通过`Accept`标头)。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-194">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="3a7d6-195">此行为可能会通过删除`TextOutputFormatter`。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-195">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="3a7d6-196">删除中的格式化程序`Configure`中的方法*Startup.cs* （如下所示）。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-196">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="3a7d6-197">模型对象的操作返回类型时将返回 204 无内容的响应返回`null`。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-197">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="3a7d6-198">此行为可能会通过删除`HttpNoContentOutputFormatter`。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-198">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="3a7d6-199">下面的代码中删除`TextOutputFormatter`和`HttpNoContentOutputFormatter`。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-199">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="3a7d6-200">而无需`TextOutputFormatter`，`string`返回类型返回 406 不可接受，例如。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-200">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="3a7d6-201">请注意，是否存在 XML 格式化程序，它将格式化`string`返回类型，如果`TextOutputFormatter`中删除。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-201">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="3a7d6-202">而无需`HttpNoContentOutputFormatter`，null 对象的格式使用配置的格式化程序。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-202">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="3a7d6-203">例如，JSON 格式化程序只需将返回的响应的正文`null`，而 XML 格式化程序将返回该属性的空 XML 元素`xsi:nil="true"`设置。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-203">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="3a7d6-204">响应格式 URL 映射</span><span class="sxs-lookup"><span data-stu-id="3a7d6-204">Response Format URL Mappings</span></span>

<span data-ttu-id="3a7d6-205">客户端可以在查询字符串或一部分的路径，或使用特定格式的文件扩展名，如.xml 或.json 中特定格式的 URL，一部分请求如。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-205">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="3a7d6-206">应使用该 API 的路由中指定请求路径中的映射。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-206">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="3a7d6-207">例如:</span><span class="sxs-lookup"><span data-stu-id="3a7d6-207">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="3a7d6-208">此路由将允许请求的格式指定为可选的文件扩展名。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-208">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="3a7d6-209">`[FormatFilter]`属性检查是否存在的中的格式值`RouteData`和创建响应时将映射到适当的格式化程序的响应格式。</span><span class="sxs-lookup"><span data-stu-id="3a7d6-209">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

| <span data-ttu-id="3a7d6-210">路由</span><span class="sxs-lookup"><span data-stu-id="3a7d6-210">Route</span></span>                      | <span data-ttu-id="3a7d6-211">格式化程序</span><span class="sxs-lookup"><span data-stu-id="3a7d6-211">Formatter</span></span>                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | <span data-ttu-id="3a7d6-212">默认的输出格式化程序</span><span class="sxs-lookup"><span data-stu-id="3a7d6-212">The default output formatter</span></span>       |
| `/products/GetById/5.json` | <span data-ttu-id="3a7d6-213">JSON 格式化程序 （如果配置）</span><span class="sxs-lookup"><span data-stu-id="3a7d6-213">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="3a7d6-214">XML 格式化程序 （如果配置）</span><span class="sxs-lookup"><span data-stu-id="3a7d6-214">The XML formatter (if configured)</span></span>  |
