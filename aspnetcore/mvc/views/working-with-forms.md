---
title: "在 ASP.NET 核心中的窗体中的标记帮助程序"
author: rick-anderson
description: "描述的内置标记帮助程序用于窗体。"
keywords: "ASP.NET 核心，标记帮助器，TagHelper，HTML 窗体中，窗体"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 25595059-4fac-4785-8152-f88590e3169b
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd69e008a81abc4f6785d93b89823c03e1a7df83
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="5e915-104">在 ASP.NET Core 中的窗体中使用标记帮助器简介</span><span class="sxs-lookup"><span data-stu-id="5e915-104">Introduction to using tag helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="5e915-105">通过[Rick Anderson](https://twitter.com/RickAndMSFT)， [Dave Paquette](https://twitter.com/Dave_Paquette)，和[Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="5e915-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="5e915-106">本文档演示如何使用窗体和窗体上的常用的 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="5e915-106">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="5e915-107">HTML[窗体](https://www.w3.org/TR/html401/interact/forms.html)元素提供的主要机制 web 应用使用发布到服务器的备份数据。</span><span class="sxs-lookup"><span data-stu-id="5e915-107">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="5e915-108">大部分本文档介绍[标记帮助程序](tag-helpers/intro.md)以及它们可以帮助你高效创建可靠的 HTML 窗体。</span><span class="sxs-lookup"><span data-stu-id="5e915-108">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="5e915-109">我们建议你阅读[简介标记帮助程序](tag-helpers/intro.md)在阅读本文档之前。</span><span class="sxs-lookup"><span data-stu-id="5e915-109">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="5e915-110">在许多情况下，HTML 帮助器向特定的标记帮助器，提供的备用方法，但务必要识别标记帮助程序不替换 HTML 帮助器并不是每个 HTML 帮助器标记帮助器。</span><span class="sxs-lookup"><span data-stu-id="5e915-110">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers do not replace HTML Helpers and there is not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="5e915-111">HTML 帮助器或者当存在时，被提到而变。</span><span class="sxs-lookup"><span data-stu-id="5e915-111">When an HTML Helper alternative exists, it is mentioned.</span></span>

<a name=my-asp-route-param-ref-label></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="5e915-112">窗体标记帮助器</span><span class="sxs-lookup"><span data-stu-id="5e915-112">The Form Tag Helper</span></span>

<span data-ttu-id="5e915-113">[窗体](https://www.w3.org/TR/html401/interact/forms.html)标记帮助器：</span><span class="sxs-lookup"><span data-stu-id="5e915-113">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="5e915-114">生成 HTML [\<窗体 >](https://www.w3.org/TR/html401/interact/forms.html) `action`的 MVC 控制器操作或命名的路由的属性值</span><span class="sxs-lookup"><span data-stu-id="5e915-114">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="5e915-115">生成隐藏[请求验证令牌](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)以防止跨站点请求伪造 (一起使用时`[ValidateAntiForgeryToken]`HTTP Post 操作方法中的属性)</span><span class="sxs-lookup"><span data-stu-id="5e915-115">Generates a hidden [Request Verification Token](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="5e915-116">提供`asp-route-<Parameter Name>`属性，其中`<Parameter Name>`添加到路由值。</span><span class="sxs-lookup"><span data-stu-id="5e915-116">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="5e915-117">`routeValues`参数`Html.BeginForm`和`Html.BeginRouteForm`提供类似的功能。</span><span class="sxs-lookup"><span data-stu-id="5e915-117">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="5e915-118">具有备用的 HTML 帮助器`Html.BeginForm`和`Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="5e915-118">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="5e915-119">示例:</span><span class="sxs-lookup"><span data-stu-id="5e915-119">Sample:</span></span>

<span data-ttu-id="5e915-120">[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="5e915-120">[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]</span></span>

<span data-ttu-id="5e915-121">上述窗体标记帮助程序生成以下 HTML:</span><span class="sxs-lookup"><span data-stu-id="5e915-121">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
   ```

<span data-ttu-id="5e915-122">MVC 运行时生成`action`属性从窗体标记帮助器属性的值`asp-controller`和`asp-action`。</span><span class="sxs-lookup"><span data-stu-id="5e915-122">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="5e915-123">窗体标记帮助器还会生成隐藏[请求验证令牌](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)以防止跨站点请求伪造 (一起使用时`[ValidateAntiForgeryToken]`HTTP Post 操作方法中的属性)。</span><span class="sxs-lookup"><span data-stu-id="5e915-123">The Form Tag Helper also generates a hidden [Request Verification Token](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="5e915-124">一个纯 HTML 窗体防止跨站点请求伪造会很困难，窗体标记帮助器提供此服务为你。</span><span class="sxs-lookup"><span data-stu-id="5e915-124">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="5e915-125">使用命名的路由</span><span class="sxs-lookup"><span data-stu-id="5e915-125">Using a named route</span></span>

<span data-ttu-id="5e915-126">`asp-route`标记帮助器属性还可以为 HTML 生成标记`action`属性。</span><span class="sxs-lookup"><span data-stu-id="5e915-126">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="5e915-127">使用应用程序[路由](../../fundamentals/routing.md)名为`register`可以使用注册页上的以下标记：</span><span class="sxs-lookup"><span data-stu-id="5e915-127">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

<span data-ttu-id="5e915-128">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="5e915-128">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]</span></span>

<span data-ttu-id="5e915-129">中的视图的许多*视图/帐户*文件夹 (在创建与新的 web 应用程序时生成*单个用户帐户*) 包含[asp 路由 returnurl](http://docs.asp.net/en/latest/mvc/views/working-with-forms.html#the-form-tag-helper)属性：</span><span class="sxs-lookup"><span data-stu-id="5e915-129">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](http://docs.asp.net/en/latest/mvc/views/working-with-forms.html#the-form-tag-helper) attribute:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [2]}} -->

```none
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
   ```

>[!NOTE]
><span data-ttu-id="5e915-130">利用内置的模板，`returnUrl`仅将在您尝试访问的授权的资源但不是进行身份验证或授权时自动填充。</span><span class="sxs-lookup"><span data-stu-id="5e915-130">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="5e915-131">当试图未经授权的访问时，安全中间件将你重定向至登录页与`returnUrl`设置。</span><span class="sxs-lookup"><span data-stu-id="5e915-131">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="5e915-132">输入的标记帮助器</span><span class="sxs-lookup"><span data-stu-id="5e915-132">The Input Tag Helper</span></span>

<span data-ttu-id="5e915-133">输入标记帮助器绑定 HTML [\<输入 >](https://www.w3.org/wiki/HTML/Elements/input)为模型表达式在 razor 视图中的元素。</span><span class="sxs-lookup"><span data-stu-id="5e915-133">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="5e915-134">语法：</span><span class="sxs-lookup"><span data-stu-id="5e915-134">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
   ```

<span data-ttu-id="5e915-135">输入的标记帮助器：</span><span class="sxs-lookup"><span data-stu-id="5e915-135">The Input Tag Helper:</span></span>

* <span data-ttu-id="5e915-136">生成`id`和`name`中指定的表达式名称的 HTML 特性`asp-for`属性。</span><span class="sxs-lookup"><span data-stu-id="5e915-136">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="5e915-137">`asp-for="Property1.Property2"` 与 `m => m.Property1.Property2` 相等。</span><span class="sxs-lookup"><span data-stu-id="5e915-137">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="5e915-138">该表达式的名称是用途`asp-for`属性值。</span><span class="sxs-lookup"><span data-stu-id="5e915-138">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="5e915-139">请参阅[表达式名称](#expression-names)部分以了解更多信息。</span><span class="sxs-lookup"><span data-stu-id="5e915-139">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="5e915-140">设置 HTML`type`属性值根据模型类型和[数据注释](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)特性应用于模型属性</span><span class="sxs-lookup"><span data-stu-id="5e915-140">Sets the HTML `type` attribute value based on the model type and  [data annotation](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) attributes applied to the model property</span></span>

* <span data-ttu-id="5e915-141">将不会覆盖 HTML`type`时指定了一个属性值</span><span class="sxs-lookup"><span data-stu-id="5e915-141">Will not overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="5e915-142">生成[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)验证属性从[数据注释](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)特性应用于模型属性</span><span class="sxs-lookup"><span data-stu-id="5e915-142">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) attributes applied to model properties</span></span>

* <span data-ttu-id="5e915-143">具有与重叠的 HTML 帮助器功能`Html.TextBoxFor`和`Html.EditorFor`。</span><span class="sxs-lookup"><span data-stu-id="5e915-143">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="5e915-144">请参阅**输入标记帮助器的 HTML 帮助器替代**部分了解详细信息。</span><span class="sxs-lookup"><span data-stu-id="5e915-144">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="5e915-145">提供强类型化。</span><span class="sxs-lookup"><span data-stu-id="5e915-145">Provides strong typing.</span></span> <span data-ttu-id="5e915-146">如果属性更改和你的名称未更新标记帮助程序将收到错误类似于以下：</span><span class="sxs-lookup"><span data-stu-id="5e915-146">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="5e915-147">`Input`标记帮助器设置的 HTML`type`属性基于.NET 类型。</span><span class="sxs-lookup"><span data-stu-id="5e915-147">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="5e915-148">下表列出了一些常见的.NET 类型和生成的 HTML 类型 （不是每个.NET 类型已经列出）。</span><span class="sxs-lookup"><span data-stu-id="5e915-148">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="5e915-149">.NET 类型</span><span class="sxs-lookup"><span data-stu-id="5e915-149">.NET type</span></span>|<span data-ttu-id="5e915-150">输入的类型</span><span class="sxs-lookup"><span data-stu-id="5e915-150">Input Type</span></span>|
|---|---|
|<span data-ttu-id="5e915-151">Bool</span><span class="sxs-lookup"><span data-stu-id="5e915-151">Bool</span></span>|<span data-ttu-id="5e915-152">类型 ="复选框"</span><span class="sxs-lookup"><span data-stu-id="5e915-152">type=”checkbox”</span></span>|
|<span data-ttu-id="5e915-153">字符串</span><span class="sxs-lookup"><span data-stu-id="5e915-153">String</span></span>|<span data-ttu-id="5e915-154">类型 ="text"</span><span class="sxs-lookup"><span data-stu-id="5e915-154">type=”text”</span></span>|
|<span data-ttu-id="5e915-155">DateTime</span><span class="sxs-lookup"><span data-stu-id="5e915-155">DateTime</span></span>|<span data-ttu-id="5e915-156">类型 ="datetime"</span><span class="sxs-lookup"><span data-stu-id="5e915-156">type=”datetime”</span></span>|
|<span data-ttu-id="5e915-157">Byte</span><span class="sxs-lookup"><span data-stu-id="5e915-157">Byte</span></span>|<span data-ttu-id="5e915-158">类型 ="number"</span><span class="sxs-lookup"><span data-stu-id="5e915-158">type=”number”</span></span>|
|<span data-ttu-id="5e915-159">Int</span><span class="sxs-lookup"><span data-stu-id="5e915-159">Int</span></span>|<span data-ttu-id="5e915-160">类型 ="number"</span><span class="sxs-lookup"><span data-stu-id="5e915-160">type=”number”</span></span>|
|<span data-ttu-id="5e915-161">Single、Double</span><span class="sxs-lookup"><span data-stu-id="5e915-161">Single, Double</span></span>|<span data-ttu-id="5e915-162">类型 ="number"</span><span class="sxs-lookup"><span data-stu-id="5e915-162">type=”number”</span></span>|


<span data-ttu-id="5e915-163">下表显示某些常见[数据注释](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)输入的标记帮助器将映射到特定的输入类型 （列出不是每个验证属性） 的属性：</span><span class="sxs-lookup"><span data-stu-id="5e915-163">The following table shows some common [data annotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="5e915-164">特性</span><span class="sxs-lookup"><span data-stu-id="5e915-164">Attribute</span></span>|<span data-ttu-id="5e915-165">输入的类型</span><span class="sxs-lookup"><span data-stu-id="5e915-165">Input Type</span></span>|
|---|---|
|<span data-ttu-id="5e915-166">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="5e915-166">[EmailAddress]</span></span>|<span data-ttu-id="5e915-167">类型 ="email"</span><span class="sxs-lookup"><span data-stu-id="5e915-167">type=”email”</span></span>|
|<span data-ttu-id="5e915-168">[Url]</span><span class="sxs-lookup"><span data-stu-id="5e915-168">[Url]</span></span>|<span data-ttu-id="5e915-169">类型 ="url"</span><span class="sxs-lookup"><span data-stu-id="5e915-169">type=”url”</span></span>|
|<span data-ttu-id="5e915-170">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="5e915-170">[HiddenInput]</span></span>|<span data-ttu-id="5e915-171">类型 ="隐藏"</span><span class="sxs-lookup"><span data-stu-id="5e915-171">type=”hidden”</span></span>|
|<span data-ttu-id="5e915-172">[Phone]</span><span class="sxs-lookup"><span data-stu-id="5e915-172">[Phone]</span></span>|<span data-ttu-id="5e915-173">类型 ="电话"</span><span class="sxs-lookup"><span data-stu-id="5e915-173">type=”tel”</span></span>|
|<span data-ttu-id="5e915-174">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="5e915-174">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="5e915-175">类型 ="密码"</span><span class="sxs-lookup"><span data-stu-id="5e915-175">type=”password”</span></span>|
|<span data-ttu-id="5e915-176">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="5e915-176">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="5e915-177">类型 ="日期"</span><span class="sxs-lookup"><span data-stu-id="5e915-177">type=”date”</span></span>|
|<span data-ttu-id="5e915-178">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="5e915-178">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="5e915-179">类型 ="时间"</span><span class="sxs-lookup"><span data-stu-id="5e915-179">type=”time”</span></span>|


<span data-ttu-id="5e915-180">示例:</span><span class="sxs-lookup"><span data-stu-id="5e915-180">Sample:</span></span>

<span data-ttu-id="5e915-181">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]</span><span class="sxs-lookup"><span data-stu-id="5e915-181">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]</span></span>

<span data-ttu-id="5e915-182">[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="5e915-182">[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]</span></span>

<span data-ttu-id="5e915-183">上面的代码生成以下 HTML:</span><span class="sxs-lookup"><span data-stu-id="5e915-183">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid e-mail address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
   ```

<span data-ttu-id="5e915-184">应用于数据注释`Email`和`Password`属性生成的模型的元数据。</span><span class="sxs-lookup"><span data-stu-id="5e915-184">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="5e915-185">输入标记帮助器使用的模型元数据并生成[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*`属性 (请参阅[模型验证](../models/validation.md))。</span><span class="sxs-lookup"><span data-stu-id="5e915-185">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="5e915-186">这些属性描述验证程序可以将附加到的输入字段。</span><span class="sxs-lookup"><span data-stu-id="5e915-186">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="5e915-187">这提供了非介入式 HTML5 和[jQuery](https://jquery.com/)验证。</span><span class="sxs-lookup"><span data-stu-id="5e915-187">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="5e915-188">非介入式属性具有格式`data-val-rule="Error Message"`，其中规则是验证规则的名称 (如`data-val-required`， `data-val-email`，`data-val-maxlength`等。)如果此属性中提供了一条错误消息，它显示的值为`data-val-rule`属性。</span><span class="sxs-lookup"><span data-stu-id="5e915-188">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it is displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="5e915-189">也有属性的窗体`data-val-ruleName-argumentName="argumentValue"`，例如，提供有关的规则的其他详细信息`data-val-maxlength-max="1024"`。</span><span class="sxs-lookup"><span data-stu-id="5e915-189">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="5e915-190">输入标记帮助器的 HTML 帮助器的替代方法</span><span class="sxs-lookup"><span data-stu-id="5e915-190">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="5e915-191">`Html.TextBox``Html.TextBoxFor`，`Html.Editor`和`Html.EditorFor`有重叠的功能与输入标记帮助器。</span><span class="sxs-lookup"><span data-stu-id="5e915-191">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="5e915-192">输入标记帮助程序将自动设置`type`属性;`Html.TextBox`和`Html.TextBoxFor`将不会。</span><span class="sxs-lookup"><span data-stu-id="5e915-192">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` will not.</span></span> <span data-ttu-id="5e915-193">`Html.Editor`和`Html.EditorFor`处理集合、 复杂的对象和模板; 输入标记帮助器却没有。</span><span class="sxs-lookup"><span data-stu-id="5e915-193">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper does not.</span></span> <span data-ttu-id="5e915-194">输入标记帮助器，`Html.EditorFor`和`Html.TextBoxFor`强类型 （它们使用 lambda 表达式）;`Html.TextBox`和`Html.Editor`不 （它们使用表达式名称）。</span><span class="sxs-lookup"><span data-stu-id="5e915-194">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="5e915-195">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="5e915-195">HtmlAttributes</span></span>

<span data-ttu-id="5e915-196">`@Html.Editor()`和`@Html.EditorFor()`使用特殊`ViewDataDictionary`条目名为`htmlAttributes`时执行其默认模板。</span><span class="sxs-lookup"><span data-stu-id="5e915-196">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="5e915-197">（可选） 使用扩充此行为`additionalViewData`参数。</span><span class="sxs-lookup"><span data-stu-id="5e915-197">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="5e915-198">键"htmlAttributes"是不区分大小写。</span><span class="sxs-lookup"><span data-stu-id="5e915-198">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="5e915-199">键"htmlAttributes"的方式类似于处理`htmlAttributes`输入类似的帮助程序对象传递给`@Html.TextBox()`。</span><span class="sxs-lookup"><span data-stu-id="5e915-199">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="5e915-200">表达式名称</span><span class="sxs-lookup"><span data-stu-id="5e915-200">Expression names</span></span>

<span data-ttu-id="5e915-201">`asp-for`属性值是`ModelExpression`和 lambda 表达式的右侧。</span><span class="sxs-lookup"><span data-stu-id="5e915-201">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="5e915-202">因此，`asp-for="Property1"`变得`m => m.Property1`在生成的代码，这正是你无需前缀`Model`。</span><span class="sxs-lookup"><span data-stu-id="5e915-202">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="5e915-203">你可以使用"@"字符，以启动内联表达式并移动之前`m.`:</span><span class="sxs-lookup"><span data-stu-id="5e915-203">You can use the "@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="5e915-204">生成以下：</span><span class="sxs-lookup"><span data-stu-id="5e915-204">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="5e915-205">集合属性时，与`asp-for="CollectionProperty[23].Member"`生成相同的名称`asp-for="CollectionProperty[i].Member"`时`i`具有值`23`。</span><span class="sxs-lookup"><span data-stu-id="5e915-205">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="5e915-206">导航子属性</span><span class="sxs-lookup"><span data-stu-id="5e915-206">Navigating child properties</span></span>

<span data-ttu-id="5e915-207">你还可以导航到使用视图模型的属性路径的子属性。</span><span class="sxs-lookup"><span data-stu-id="5e915-207">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="5e915-208">请考虑一个包含子级的更复杂的模型类`Address`属性。</span><span class="sxs-lookup"><span data-stu-id="5e915-208">Consider a more complex model class that contains a child `Address` property.</span></span>

<span data-ttu-id="5e915-209">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]</span><span class="sxs-lookup"><span data-stu-id="5e915-209">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]</span></span>

<span data-ttu-id="5e915-210">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]</span><span class="sxs-lookup"><span data-stu-id="5e915-210">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]</span></span>

<span data-ttu-id="5e915-211">在视图中，我们将绑定到`Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="5e915-211">In the view, we bind to `Address.AddressLine1`:</span></span>

<span data-ttu-id="5e915-212">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]</span><span class="sxs-lookup"><span data-stu-id="5e915-212">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]</span></span>

<span data-ttu-id="5e915-213">为生成的以下 HTML `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="5e915-213">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
   ```

### <a name="expression-names-and-collections"></a><span data-ttu-id="5e915-214">表达式名称和集合</span><span class="sxs-lookup"><span data-stu-id="5e915-214">Expression names and Collections</span></span>

<span data-ttu-id="5e915-215">示例模型包含一个数组`Colors`:</span><span class="sxs-lookup"><span data-stu-id="5e915-215">Sample, a model containing an array of `Colors`:</span></span>

<span data-ttu-id="5e915-216">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]</span><span class="sxs-lookup"><span data-stu-id="5e915-216">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]</span></span>

<span data-ttu-id="5e915-217">操作方法中：</span><span class="sxs-lookup"><span data-stu-id="5e915-217">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
   ```

<span data-ttu-id="5e915-218">以下 Razor 显示如何访问特定`Color`元素：</span><span class="sxs-lookup"><span data-stu-id="5e915-218">The following Razor shows how you access a specific `Color` element:</span></span>

<span data-ttu-id="5e915-219">[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="5e915-219">[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]</span></span>

<span data-ttu-id="5e915-220">*Views/Shared/EditorTemplates/String.cshtml*模板：</span><span class="sxs-lookup"><span data-stu-id="5e915-220">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

<span data-ttu-id="5e915-221">[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="5e915-221">[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]</span></span>

<span data-ttu-id="5e915-222">示例使用`List<T>`:</span><span class="sxs-lookup"><span data-stu-id="5e915-222">Sample using `List<T>`:</span></span>

<span data-ttu-id="5e915-223">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]</span><span class="sxs-lookup"><span data-stu-id="5e915-223">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]</span></span>

<span data-ttu-id="5e915-224">以下 Razor 演示如何循环访问集合：</span><span class="sxs-lookup"><span data-stu-id="5e915-224">The following Razor shows how to iterate over a collection:</span></span>

<span data-ttu-id="5e915-225">[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="5e915-225">[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]</span></span>

<span data-ttu-id="5e915-226">*Views/Shared/EditorTemplates/ToDoItem.cshtml*模板：</span><span class="sxs-lookup"><span data-stu-id="5e915-226">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

<span data-ttu-id="5e915-227">[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="5e915-227">[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]</span></span>


>[!NOTE]
><span data-ttu-id="5e915-228">始终使用`for`(和*不* `foreach`) 以循环访问列表。</span><span class="sxs-lookup"><span data-stu-id="5e915-228">Always use `for` (and *not* `foreach`) to iterate over a list.</span></span> <span data-ttu-id="5e915-229">计算在 LINQ 中的索引器表达式可能很昂贵，并应将最小化。</span><span class="sxs-lookup"><span data-stu-id="5e915-229">Evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="5e915-230">上面的注释的示例代码演示如何将替换放在 lambda 表达式的`@`运算符来访问每个`ToDoItem`列表中。</span><span class="sxs-lookup"><span data-stu-id="5e915-230">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="5e915-231">Textarea 标记帮助器</span><span class="sxs-lookup"><span data-stu-id="5e915-231">The Textarea Tag Helper</span></span>

<span data-ttu-id="5e915-232">`Textarea Tag Helper`标记帮助程序是类似于输入标记帮助器。</span><span class="sxs-lookup"><span data-stu-id="5e915-232">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="5e915-233">生成`id`和`name`特性和的模型中的数据验证属性[ \<textarea >](http://www.w3.org/wiki/HTML/Elements/textarea)元素。</span><span class="sxs-lookup"><span data-stu-id="5e915-233">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](http://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="5e915-234">提供强类型化。</span><span class="sxs-lookup"><span data-stu-id="5e915-234">Provides strong typing.</span></span>

* <span data-ttu-id="5e915-235">HTML 帮助器的替代项：`Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="5e915-235">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="5e915-236">示例:</span><span class="sxs-lookup"><span data-stu-id="5e915-236">Sample:</span></span>

<span data-ttu-id="5e915-237">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]</span><span class="sxs-lookup"><span data-stu-id="5e915-237">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]</span></span>

<span data-ttu-id="5e915-238">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="5e915-238">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]</span></span>

<span data-ttu-id="5e915-239">生成的以下 HTML:</span><span class="sxs-lookup"><span data-stu-id="5e915-239">The following HTML is generated:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [2, 3, 4, 5, 6, 7, 8]}} -->

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a><span data-ttu-id="5e915-240">标签标记帮助器</span><span class="sxs-lookup"><span data-stu-id="5e915-240">The Label Tag Helper</span></span>

* <span data-ttu-id="5e915-241">生成标签标题和`for`属性[ <label> ](https://www.w3.org/wiki/HTML/Elements/label)表达式名称的元素</span><span class="sxs-lookup"><span data-stu-id="5e915-241">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="5e915-242">HTML 帮助器的替代项： `Html.LabelFor`。</span><span class="sxs-lookup"><span data-stu-id="5e915-242">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="5e915-243">`Label Tag Helper`在纯 HTML 标签元素提供了以下好处：</span><span class="sxs-lookup"><span data-stu-id="5e915-243">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="5e915-244">自动获取描述性标签值`Display`属性。</span><span class="sxs-lookup"><span data-stu-id="5e915-244">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="5e915-245">预期的显示名称可能会更改通过时间和的组合`Display`属性和标签标记帮助程序将应用`Display`无处不在使用它。</span><span class="sxs-lookup"><span data-stu-id="5e915-245">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="5e915-246">在源代码中较少标记</span><span class="sxs-lookup"><span data-stu-id="5e915-246">Less markup in source code</span></span>

* <span data-ttu-id="5e915-247">强类型化与模型属性。</span><span class="sxs-lookup"><span data-stu-id="5e915-247">Strong typing with the model property.</span></span>

<span data-ttu-id="5e915-248">示例:</span><span class="sxs-lookup"><span data-stu-id="5e915-248">Sample:</span></span>

<span data-ttu-id="5e915-249">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]</span><span class="sxs-lookup"><span data-stu-id="5e915-249">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]</span></span>

<span data-ttu-id="5e915-250">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="5e915-250">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]</span></span>

<span data-ttu-id="5e915-251">为生成的以下 HTML`<label>`元素：</span><span class="sxs-lookup"><span data-stu-id="5e915-251">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
   ```

<span data-ttu-id="5e915-252">标签标记帮助器生成`for`"Email"，是的 ID 属性值与关联`<input>`元素。</span><span class="sxs-lookup"><span data-stu-id="5e915-252">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="5e915-253">标记帮助程序生成一致`id`和`for`元素以便能够正确关联。</span><span class="sxs-lookup"><span data-stu-id="5e915-253">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="5e915-254">在此示例中的标题来自`Display`属性。</span><span class="sxs-lookup"><span data-stu-id="5e915-254">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="5e915-255">如果该模型未包含`Display`属性标题将该表达式的属性名称。</span><span class="sxs-lookup"><span data-stu-id="5e915-255">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="5e915-256">验证标记帮助程序</span><span class="sxs-lookup"><span data-stu-id="5e915-256">The Validation Tag Helpers</span></span>

<span data-ttu-id="5e915-257">有两个验证标记帮助程序。</span><span class="sxs-lookup"><span data-stu-id="5e915-257">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="5e915-258">`Validation Message Tag Helper` （这会显示一条验证消息对单个属性模型上）、 和`Validation Summary Tag Helper`（它会显示验证错误的摘要）。</span><span class="sxs-lookup"><span data-stu-id="5e915-258">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="5e915-259">`Input Tag Helper`将 HTML5 客户端端验证属性输入元素基于数据模型类上的批注属性添加。</span><span class="sxs-lookup"><span data-stu-id="5e915-259">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="5e915-260">此外在服务器上执行验证。</span><span class="sxs-lookup"><span data-stu-id="5e915-260">Validation is also performed on the server.</span></span> <span data-ttu-id="5e915-261">发生验证错误时，验证标记帮助器将显示这些错误消息。</span><span class="sxs-lookup"><span data-stu-id="5e915-261">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="5e915-262">验证消息标记帮助器</span><span class="sxs-lookup"><span data-stu-id="5e915-262">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="5e915-263">将添加[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"`属性设为[跨越](https://developer.mozilla.org/docs/Web/HTML/Element/span)元素，它将附加在指定的模型属性的输入字段上的验证错误消息。  </span><span class="sxs-lookup"><span data-stu-id="5e915-263">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="5e915-264">客户端验证错误时， [jQuery](https://jquery.com/)显示中的错误消息`<span>`元素。</span><span class="sxs-lookup"><span data-stu-id="5e915-264">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="5e915-265">验证还会在服务器。</span><span class="sxs-lookup"><span data-stu-id="5e915-265">Validation also takes place on the server.</span></span> <span data-ttu-id="5e915-266">客户端可能已禁用 JavaScript，仅可以在服务器端执行一些验证。</span><span class="sxs-lookup"><span data-stu-id="5e915-266">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="5e915-267">HTML 帮助器的替代项：`Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="5e915-267">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="5e915-268">`Validation Message Tag Helper`用于`asp-validation-for`上对 HTML 特性[跨越](https://developer.mozilla.org/docs/Web/HTML/Element/span)元素。</span><span class="sxs-lookup"><span data-stu-id="5e915-268">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
   ```

<span data-ttu-id="5e915-269">验证消息标记帮助器将生成以下 HTML:</span><span class="sxs-lookup"><span data-stu-id="5e915-269">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="5e915-270">通常使用`Validation Message Tag Helper`后`Input`为相同属性的标记帮助器。</span><span class="sxs-lookup"><span data-stu-id="5e915-270">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="5e915-271">这样显示附近的输入将导致错误的任何验证错误消息。</span><span class="sxs-lookup"><span data-stu-id="5e915-271">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="5e915-272">你必须具有正确的 JavaScript 的视图和[jQuery](https://jquery.com/)脚本中的客户端验证准备好的引用。</span><span class="sxs-lookup"><span data-stu-id="5e915-272">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="5e915-273">请参阅[模型验证](../models/validation.md)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="5e915-273">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="5e915-274">（例如当你已自定义服务器端验证或客户端验证已禁用） 服务器端验证错误时，MVC 中放置该错误消息的正文`<span>`元素。</span><span class="sxs-lookup"><span data-stu-id="5e915-274">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="5e915-275">验证摘要标记帮助器</span><span class="sxs-lookup"><span data-stu-id="5e915-275">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="5e915-276">目标`<div>`元素`asp-validation-summary`属性</span><span class="sxs-lookup"><span data-stu-id="5e915-276">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="5e915-277">HTML 帮助器的替代项：`@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="5e915-277">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="5e915-278">`Validation Summary Tag Helper`用于显示的验证消息的摘要。</span><span class="sxs-lookup"><span data-stu-id="5e915-278">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="5e915-279">`asp-validation-summary`属性值可以是任何以下：</span><span class="sxs-lookup"><span data-stu-id="5e915-279">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="5e915-280">asp 验证摘要</span><span class="sxs-lookup"><span data-stu-id="5e915-280">asp-validation-summary</span></span>|<span data-ttu-id="5e915-281">显示的验证消息</span><span class="sxs-lookup"><span data-stu-id="5e915-281">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="5e915-282">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="5e915-282">ValidationSummary.All</span></span>|<span data-ttu-id="5e915-283">属性和模型级别</span><span class="sxs-lookup"><span data-stu-id="5e915-283">Property and model level</span></span>|
|<span data-ttu-id="5e915-284">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="5e915-284">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="5e915-285">模型</span><span class="sxs-lookup"><span data-stu-id="5e915-285">Model</span></span>|
|<span data-ttu-id="5e915-286">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="5e915-286">ValidationSummary.None</span></span>|<span data-ttu-id="5e915-287">无</span><span class="sxs-lookup"><span data-stu-id="5e915-287">None</span></span>|

### <a name="sample"></a><span data-ttu-id="5e915-288">示例</span><span class="sxs-lookup"><span data-stu-id="5e915-288">Sample</span></span>

<span data-ttu-id="5e915-289">在下面的示例中，数据模型用修饰`DataAnnotation`生成验证错误消息的属性`<input>`元素。</span><span class="sxs-lookup"><span data-stu-id="5e915-289">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="5e915-290">出现验证错误时，验证标记帮助器显示的错误消息：</span><span class="sxs-lookup"><span data-stu-id="5e915-290">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

<span data-ttu-id="5e915-291">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]</span><span class="sxs-lookup"><span data-stu-id="5e915-291">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]</span></span>

<span data-ttu-id="5e915-292">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]</span><span class="sxs-lookup"><span data-stu-id="5e915-292">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]</span></span>

<span data-ttu-id="5e915-293">生成的 HTML （如果该模型为有效）：</span><span class="sxs-lookup"><span data-stu-id="5e915-293">The generated HTML (when the model is valid):</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [2, 3, 8, 9, 12, 13]}} -->

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid e-mail address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a><span data-ttu-id="5e915-294">选择标记帮助器</span><span class="sxs-lookup"><span data-stu-id="5e915-294">The Select Tag Helper</span></span>

* <span data-ttu-id="5e915-295">生成[选择](https://www.w3.org/wiki/HTML/Elements/select)和关联[选项](https://www.w3.org/wiki/HTML/Elements/option)模型属性的元素。</span><span class="sxs-lookup"><span data-stu-id="5e915-295">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="5e915-296">具有备用的 HTML 帮助器`Html.DropDownListFor`和`Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="5e915-296">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="5e915-297">`Select Tag Helper` `asp-for`指定的模型属性名称[选择](https://www.w3.org/wiki/HTML/Elements/select)元素和`asp-items`指定[选项](https://www.w3.org/wiki/HTML/Elements/option)元素。</span><span class="sxs-lookup"><span data-stu-id="5e915-297">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="5e915-298">例如: </span><span class="sxs-lookup"><span data-stu-id="5e915-298">For example:</span></span>

<span data-ttu-id="5e915-299">[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]</span><span class="sxs-lookup"><span data-stu-id="5e915-299">[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]</span></span>

<span data-ttu-id="5e915-300">示例:</span><span class="sxs-lookup"><span data-stu-id="5e915-300">Sample:</span></span>

<span data-ttu-id="5e915-301">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]</span><span class="sxs-lookup"><span data-stu-id="5e915-301">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]</span></span>

<span data-ttu-id="5e915-302">`Index`方法初始化`CountryViewModel`、 设置所选国家/地区和将其传递给`Index`视图。</span><span class="sxs-lookup"><span data-stu-id="5e915-302">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

<span data-ttu-id="5e915-303">[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]</span><span class="sxs-lookup"><span data-stu-id="5e915-303">[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]</span></span>

<span data-ttu-id="5e915-304">HTTP POST`Index`方法会显示所选内容：</span><span class="sxs-lookup"><span data-stu-id="5e915-304">The HTTP POST `Index` method displays the selection:</span></span>

<span data-ttu-id="5e915-305">[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]</span><span class="sxs-lookup"><span data-stu-id="5e915-305">[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]</span></span>

<span data-ttu-id="5e915-306">`Index`视图：</span><span class="sxs-lookup"><span data-stu-id="5e915-306">The `Index` view:</span></span>

<span data-ttu-id="5e915-307">[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="5e915-307">[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]</span></span>

<span data-ttu-id="5e915-308">这将生成以下 html 代码虽然 （使用"CA"选择):</span><span class="sxs-lookup"><span data-stu-id="5e915-308">Which generates the following HTML (with "CA" selected):</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [2, 3, 4, 5, 6]}} -->

```HTML
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
   ```

> [!NOTE]
> <span data-ttu-id="5e915-309">我们不建议使用`ViewBag`或`ViewData`与选择标记帮助器。</span><span class="sxs-lookup"><span data-stu-id="5e915-309">We do not recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="5e915-310">视图模型是在提供 MVC 元数据更加可靠和通常更不容易出错。</span><span class="sxs-lookup"><span data-stu-id="5e915-310">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="5e915-311">`asp-for`属性值是一种特殊情况，不要求`Model`前缀，其他标记帮助器属性那样 (如`asp-items`)</span><span class="sxs-lookup"><span data-stu-id="5e915-311">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

<span data-ttu-id="5e915-312">[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]</span><span class="sxs-lookup"><span data-stu-id="5e915-312">[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]</span></span>

### <a name="enum-binding"></a><span data-ttu-id="5e915-313">枚举绑定</span><span class="sxs-lookup"><span data-stu-id="5e915-313">Enum binding</span></span>

<span data-ttu-id="5e915-314">通常可以方便地使用`<select>`与`enum`属性并生成`SelectListItem`元素从`enum`值。</span><span class="sxs-lookup"><span data-stu-id="5e915-314">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="5e915-315">示例:</span><span class="sxs-lookup"><span data-stu-id="5e915-315">Sample:</span></span>

<span data-ttu-id="5e915-316">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]</span><span class="sxs-lookup"><span data-stu-id="5e915-316">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]</span></span>

<span data-ttu-id="5e915-317">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]</span><span class="sxs-lookup"><span data-stu-id="5e915-317">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]</span></span>

<span data-ttu-id="5e915-318">`GetEnumSelectList`方法生成`SelectList`枚举的对象。</span><span class="sxs-lookup"><span data-stu-id="5e915-318">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

<span data-ttu-id="5e915-319">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="5e915-319">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]</span></span>

<span data-ttu-id="5e915-320">你可以按照声明你使用的枚举器列表`Display`属性以获取更丰富的 UI:</span><span class="sxs-lookup"><span data-stu-id="5e915-320">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

<span data-ttu-id="5e915-321">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]</span><span class="sxs-lookup"><span data-stu-id="5e915-321">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]</span></span>

<span data-ttu-id="5e915-322">生成的以下 HTML:</span><span class="sxs-lookup"><span data-stu-id="5e915-322">The following HTML is generated:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [4, 5]}} -->

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
   ```

### <a name="option-group"></a><span data-ttu-id="5e915-323">选项组</span><span class="sxs-lookup"><span data-stu-id="5e915-323">Option Group</span></span>

<span data-ttu-id="5e915-324">HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup)视图模型包含一个或多时，会生成元素`SelectListGroup`对象。</span><span class="sxs-lookup"><span data-stu-id="5e915-324">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="5e915-325">`CountryViewModelGroup`组`SelectListItem`"北美"和"Europe"分组的元素：</span><span class="sxs-lookup"><span data-stu-id="5e915-325">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

<span data-ttu-id="5e915-326">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]</span><span class="sxs-lookup"><span data-stu-id="5e915-326">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]</span></span>

<span data-ttu-id="5e915-327">下面显示了两个组：</span><span class="sxs-lookup"><span data-stu-id="5e915-327">The two groups are shown below:</span></span>

![选项组示例](working-with-forms/_static/grp.png)

<span data-ttu-id="5e915-329">生成的 HTML 中：</span><span class="sxs-lookup"><span data-stu-id="5e915-329">The generated HTML:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [3, 4, 5, 6, 7, 8, 9, 10, 11, 12]}} -->

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a><span data-ttu-id="5e915-330">多个选择</span><span class="sxs-lookup"><span data-stu-id="5e915-330">Multiple select</span></span>

<span data-ttu-id="5e915-331">选择标记帮助程序将自动生成[多个 ="多个"](http://w3c.github.io/html-reference/select.html)属性中指定的属性如果`asp-for`属性是`IEnumerable`。</span><span class="sxs-lookup"><span data-stu-id="5e915-331">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="5e915-332">例如，给定以下模型：</span><span class="sxs-lookup"><span data-stu-id="5e915-332">For example, given the following model:</span></span>

<span data-ttu-id="5e915-333">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]</span><span class="sxs-lookup"><span data-stu-id="5e915-333">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]</span></span>

<span data-ttu-id="5e915-334">使用以下视图：</span><span class="sxs-lookup"><span data-stu-id="5e915-334">With the following view:</span></span>

<span data-ttu-id="5e915-335">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="5e915-335">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]</span></span>

<span data-ttu-id="5e915-336">生成的以下 HTML:</span><span class="sxs-lookup"><span data-stu-id="5e915-336">Generates the following HTML:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [3]}} -->

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a><span data-ttu-id="5e915-337">没有选定内容</span><span class="sxs-lookup"><span data-stu-id="5e915-337">No selection</span></span>

<span data-ttu-id="5e915-338">如果你发现自己使用多个页中的"未指定"选项，你可以创建一个模板以消除重复 HTML:</span><span class="sxs-lookup"><span data-stu-id="5e915-338">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

<span data-ttu-id="5e915-339">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="5e915-339">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]</span></span>

<span data-ttu-id="5e915-340">*Views/Shared/EditorTemplates/CountryViewModel.cshtml*模板：</span><span class="sxs-lookup"><span data-stu-id="5e915-340">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

<span data-ttu-id="5e915-341">[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="5e915-341">[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]</span></span>

<span data-ttu-id="5e915-342">添加 HTML [\<选项 >](https://www.w3.org/wiki/HTML/Elements/option)元素并不局限于*没有选定内容*用例。</span><span class="sxs-lookup"><span data-stu-id="5e915-342">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements is not limited to the *No selection* case.</span></span> <span data-ttu-id="5e915-343">例如，以下视图和操作方法将生成 HTML 类似于上面的代码：</span><span class="sxs-lookup"><span data-stu-id="5e915-343">For example, the following view and action method will generate HTML similar to the code above:</span></span>

<span data-ttu-id="5e915-344">[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]</span><span class="sxs-lookup"><span data-stu-id="5e915-344">[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]</span></span>

<span data-ttu-id="5e915-345">[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="5e915-345">[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]</span></span>

<span data-ttu-id="5e915-346">正确`<option>`将选择的元素 (包含`selected="selected"`属性) 具体取决于当前`Country`值。</span><span class="sxs-lookup"><span data-stu-id="5e915-346">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [5]}} -->

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a><span data-ttu-id="5e915-347">其他资源</span><span class="sxs-lookup"><span data-stu-id="5e915-347">Additional Resources</span></span>

* [<span data-ttu-id="5e915-348">标记帮助程序</span><span class="sxs-lookup"><span data-stu-id="5e915-348">Tag Helpers</span></span>](tag-helpers/intro.md)

* [<span data-ttu-id="5e915-349">HTML 窗体元素</span><span class="sxs-lookup"><span data-stu-id="5e915-349">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)

* [<span data-ttu-id="5e915-350">请求验证令牌</span><span class="sxs-lookup"><span data-stu-id="5e915-350">Request Verification Token</span></span>](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [<span data-ttu-id="5e915-351">模型绑定</span><span class="sxs-lookup"><span data-stu-id="5e915-351">Model Binding</span></span>](../models/model-binding.md)

* [<span data-ttu-id="5e915-352">模型验证</span><span class="sxs-lookup"><span data-stu-id="5e915-352">Model Validation</span></span>](../models/validation.md)

* [<span data-ttu-id="5e915-353">数据注释</span><span class="sxs-lookup"><span data-stu-id="5e915-353">data annotations</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)

* <span data-ttu-id="5e915-354">[代码片段此文档](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample)。</span><span class="sxs-lookup"><span data-stu-id="5e915-354">[Code snippets for this document](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span></span>
