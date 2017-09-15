---
title: "添加验证"
author: rick-anderson
description: "如何向 Razor 页面添加验证"
keywords: "ASP.NET Core, 验证, DataAnnotations, Razor, Razor 页面"
ms.author: riande
manager: wpickett
ms.date: 8/7/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 734dad7778eba41780f9d3ac0685879687288d47
ms.sourcegitcommit: 8f5277871eff86134ebf68d3737196cfd4a62c2c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2017
---
# <a name="adding-validation-to-a-razor-page"></a><span data-ttu-id="83033-104">向 Razor 页面添加验证</span><span class="sxs-lookup"><span data-stu-id="83033-104">Adding validation to a Razor Page</span></span>

<span data-ttu-id="83033-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="83033-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="83033-106">本部分中向 `Movie` 模型添加了验证逻辑。</span><span class="sxs-lookup"><span data-stu-id="83033-106">In this section validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="83033-107">每当用户创建或编辑电影时，都会强制执行验证规则。</span><span class="sxs-lookup"><span data-stu-id="83033-107">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="83033-108">验证</span><span class="sxs-lookup"><span data-stu-id="83033-108">Validation</span></span>

<span data-ttu-id="83033-109">软件开发的一个关键原则被称为 [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself)（即“不要自我重复”）。</span><span class="sxs-lookup"><span data-stu-id="83033-109">A key tenet of software development is called [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="83033-110">Razor 页面鼓励进行仅指定一次功能的开发，且功能在整个应用中反映。</span><span class="sxs-lookup"><span data-stu-id="83033-110">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="83033-111">DRY 有助于减少应用中的代码量。</span><span class="sxs-lookup"><span data-stu-id="83033-111">DRY can help reduce the amount of code in an app.</span></span> <span data-ttu-id="83033-112">DRY 使代码更加不易出错，且更易于测试和维护。</span><span class="sxs-lookup"><span data-stu-id="83033-112">DRY makes the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="83033-113">Razor 页面和 Entity Framework 提供的验证支持是 DRY 原则的极佳示例。</span><span class="sxs-lookup"><span data-stu-id="83033-113">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="83033-114">验证规则在模型类中的某处以声明方式指定，且在应用的所有位置强制执行。</span><span class="sxs-lookup"><span data-stu-id="83033-114">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

### <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="83033-115">将验证规则添加到电影模型</span><span class="sxs-lookup"><span data-stu-id="83033-115">Adding validation rules to the movie model</span></span>

<span data-ttu-id="83033-116">打开 Movie.cs 文件。</span><span class="sxs-lookup"><span data-stu-id="83033-116">Open the *Movie.cs* file.</span></span> <span data-ttu-id="83033-117">[DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) 提供一组内置验证特性，可通过声明方式应用于类或属性。</span><span class="sxs-lookup"><span data-stu-id="83033-117">[DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) provides a built-in set of validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="83033-118">DataAnnotations 还包含 `DataType` 等格式特性，有助于格式设置但不提供验证。</span><span class="sxs-lookup"><span data-stu-id="83033-118">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide validation.</span></span>

<span data-ttu-id="83033-119">更新 `Movie` 类以使用 `Required`、`StringLength`、`RegularExpression` 和 `Range` 验证特性。</span><span class="sxs-lookup"><span data-stu-id="83033-119">Update the `Movie` class to take advantage of the `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

<span data-ttu-id="83033-120">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="83033-120">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]</span></span>

<span data-ttu-id="83033-121">验证特性用于指定模型属性上强制执行的行为。</span><span class="sxs-lookup"><span data-stu-id="83033-121">Validation attributes specify behavior that is enforced on model properties.</span></span> <span data-ttu-id="83033-122">`Required` 和 `MinimumLength` 特性表示属性必须具有值；但用户可输入空格来满足验证约束。</span><span class="sxs-lookup"><span data-stu-id="83033-122">The `Required` and `MinimumLength` attributes indicates that a property must have a value; but nothing prevents a user from entering white space to satisfy the validation constraint.</span></span> <span data-ttu-id="83033-123">`RegularExpression` 特性用于限制可输入的字符。</span><span class="sxs-lookup"><span data-stu-id="83033-123">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="83033-124">在上述代码中，`Genre` 和 `Rating` 仅可使用字母（禁用空格、数字和特殊字符）。</span><span class="sxs-lookup"><span data-stu-id="83033-124">In the preceding code, `Genre` and `Rating` must use only letters (white space, numbers and special characters are not allowed).</span></span> <span data-ttu-id="83033-125">`Range` 特性将值限制在指定范围内。</span><span class="sxs-lookup"><span data-stu-id="83033-125">The `Range` attribute constrains a value to within a specified range.</span></span> <span data-ttu-id="83033-126">`StringLength` 特性设置字符串的最大长度，且可视情况设置最小长度。</span><span class="sxs-lookup"><span data-stu-id="83033-126">The `StringLength` attribute sets the maximum length of a string, and optionally the minimum length.</span></span> <span data-ttu-id="83033-127">从本质上来说，需要[值类型](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types)（如 `decimal`、`int`、`float`、`DateTime`），但不需要 `[Required]` 特性。</span><span class="sxs-lookup"><span data-stu-id="83033-127">[Value types](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="83033-128">让 ASP.NET Core 强制自动执行验证规则有助于提升应用的可靠性。</span><span class="sxs-lookup"><span data-stu-id="83033-128">Having validation rules automatically enforced by ASP.NET Core helps make an app more robust.</span></span> <span data-ttu-id="83033-129">自动验证模型有助于保护应用，因为添加新代码时无需手动应用它们。</span><span class="sxs-lookup"><span data-stu-id="83033-129">Automatic validation on models helps protect the app because you don't have to remember to apply them when new code is added.</span></span>

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="83033-130">Razor 页面中的“验证错误”UI</span><span class="sxs-lookup"><span data-stu-id="83033-130">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="83033-131">运行应用并导航到“页面/电影”。</span><span class="sxs-lookup"><span data-stu-id="83033-131">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="83033-132">选择“新建”链接。</span><span class="sxs-lookup"><span data-stu-id="83033-132">Select the **Create New** link.</span></span> <span data-ttu-id="83033-133">使用无效值填写表单。</span><span class="sxs-lookup"><span data-stu-id="83033-133">Complete the form with some invalid values.</span></span> <span data-ttu-id="83033-134">当 jQuery 客户端验证检测到错误时，会显示一条错误消息。</span><span class="sxs-lookup"><span data-stu-id="83033-134">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![带有多个 jQuery 客户端验证错误的电影视图表单](validation/_static/val.png)

> [!NOTE]
> <span data-ttu-id="83033-136">可能无法在 `Price` 字段中输入小数点或逗号。</span><span class="sxs-lookup"><span data-stu-id="83033-136">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="83033-137">若要使 [jQuery 验证](http://jqueryvalidation.org/)支持使用逗号（“,”）表示小数点及使用非美国英语日期格式的非英语区域设置，必须执行使应用全球化的步骤。</span><span class="sxs-lookup"><span data-stu-id="83033-137">To support [jQuery validation](http://jqueryvalidation.org/) in non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="83033-138">有关详细信息，请参阅[其他资源](#additional-resources)。</span><span class="sxs-lookup"><span data-stu-id="83033-138">See [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="83033-139">目前只能输入整数，例如 10。</span><span class="sxs-lookup"><span data-stu-id="83033-139">For now, just enter whole numbers like 10.</span></span>

<span data-ttu-id="83033-140">请注意表单如何自动呈现每个包含无效值的字段中的验证错误消息。</span><span class="sxs-lookup"><span data-stu-id="83033-140">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="83033-141">客户端（使用 JavaScript 和 jQuery）和服务器端（若用户禁用 JavaScript）都必定会遇到这些错误。</span><span class="sxs-lookup"><span data-stu-id="83033-141">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="83033-142">一项重要优势是，无需在“创建”或“编辑”页面中更改代码。</span><span class="sxs-lookup"><span data-stu-id="83033-142">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="83033-143">在模型应用 DataAnnotations 后，即已启用验证 UI。</span><span class="sxs-lookup"><span data-stu-id="83033-143">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="83033-144">本教程中自动创建的 Razor 页面自动选取了验证规则（使用 `Movie` 模型类的属性上的验证特性）。</span><span class="sxs-lookup"><span data-stu-id="83033-144">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="83033-145">使用“编辑”页面测试验证后，即已应用相同验证。</span><span class="sxs-lookup"><span data-stu-id="83033-145">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="83033-146">存在客户端验证错误时，不会将表单数据发布到服务器。</span><span class="sxs-lookup"><span data-stu-id="83033-146">The form data is not posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="83033-147">请通过下述一个或多个方法验证是否未发布表单数据：</span><span class="sxs-lookup"><span data-stu-id="83033-147">Verify form data is not posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="83033-148">在 `OnPostAsync` 方法中放置一个断点。</span><span class="sxs-lookup"><span data-stu-id="83033-148">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="83033-149">提交表单（选择“创建”或“保存”）。</span><span class="sxs-lookup"><span data-stu-id="83033-149">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="83033-150">从未命中断点。</span><span class="sxs-lookup"><span data-stu-id="83033-150">The break point is never hit.</span></span>
* <span data-ttu-id="83033-151">使用 [Fiddler 工具](http://www.telerik.com/fiddler)。</span><span class="sxs-lookup"><span data-stu-id="83033-151">Use the [Fiddler tool](http://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="83033-152">使用浏览器开发人员工具监视网络流量。</span><span class="sxs-lookup"><span data-stu-id="83033-152">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="83033-153">服务器端验证</span><span class="sxs-lookup"><span data-stu-id="83033-153">Server-side validation</span></span>

<span data-ttu-id="83033-154">在浏览器中禁用 JavaScript 后，提交出错表单将发布到服务器。</span><span class="sxs-lookup"><span data-stu-id="83033-154">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="83033-155">（可选）测试服务器端验证：</span><span class="sxs-lookup"><span data-stu-id="83033-155">Optional, test server-side validation:</span></span>

* <span data-ttu-id="83033-156">在浏览器中禁用 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="83033-156">Disable JavaScript in the browser.</span></span> <span data-ttu-id="83033-157">如果无法在浏览器中禁用 JavaScript，请尝试其他浏览器。</span><span class="sxs-lookup"><span data-stu-id="83033-157">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="83033-158">在“创建”或“编辑”页面的 `OnPostAsync` 方法中设置断点。</span><span class="sxs-lookup"><span data-stu-id="83033-158">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="83033-159">提交带有验证错误的表单。</span><span class="sxs-lookup"><span data-stu-id="83033-159">Submit a form with validation errors.</span></span>
* <span data-ttu-id="83033-160">验证模型状态是否无效：</span><span class="sxs-lookup"><span data-stu-id="83033-160">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="83033-161">以下代码显示了之前在本教程中设定其基架的“Create.cshtml”的一部分。</span><span class="sxs-lookup"><span data-stu-id="83033-161">The following code shows a portion of the *Create.cshtml* page that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="83033-162">它用于在“创建”和“编辑”页面中显示初始表单并在发生错误后重新显示表单。</span><span class="sxs-lookup"><span data-stu-id="83033-162">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

<span data-ttu-id="83033-163">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]</span><span class="sxs-lookup"><span data-stu-id="83033-163">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]</span></span>

<span data-ttu-id="83033-164">[输入标记帮助程序](xref:mvc/views/working-with-forms)使用 [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) 特性并在客户端生成 jQuery 验证所需的 HTML 特性。</span><span class="sxs-lookup"><span data-stu-id="83033-164">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="83033-165">[验证标记帮助程序](xref:mvc/views/working-with-forms#the-validation-tag-helpers)用于显示验证错误。</span><span class="sxs-lookup"><span data-stu-id="83033-165">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="83033-166">有关详细信息，请参阅[验证](xref:mvc/models/validation)。</span><span class="sxs-lookup"><span data-stu-id="83033-166">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="83033-167">“创建”和“编辑”页面中没有验证规则。</span><span class="sxs-lookup"><span data-stu-id="83033-167">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="83033-168">仅可在 `Movie` 类中指定验证规则和错误字符串。</span><span class="sxs-lookup"><span data-stu-id="83033-168">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="83033-169">这些验证规则将自动应用于编辑 `Movie` 模型的 Razor 页面。</span><span class="sxs-lookup"><span data-stu-id="83033-169">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="83033-170">需要更改验证逻辑时，也只能在该模型中更改。</span><span class="sxs-lookup"><span data-stu-id="83033-170">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="83033-171">将始终在整个应用程序中应用验证（在一处定义验证逻辑）。</span><span class="sxs-lookup"><span data-stu-id="83033-171">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="83033-172">单处验证有助于保持代码干净，且更易于维护和更新。</span><span class="sxs-lookup"><span data-stu-id="83033-172">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="83033-173">使用 DataType 特性</span><span class="sxs-lookup"><span data-stu-id="83033-173">Using DataType Attributes</span></span>

<span data-ttu-id="83033-174">检查 `Movie` 类。</span><span class="sxs-lookup"><span data-stu-id="83033-174">Examine the `Movie` class.</span></span> <span data-ttu-id="83033-175">除了一组内置的验证特性，`System.ComponentModel.DataAnnotations` 命名空间还提供格式特性。</span><span class="sxs-lookup"><span data-stu-id="83033-175">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="83033-176">`DataType` 特性应用于 `ReleaseDate` 和 `Price` 属性。</span><span class="sxs-lookup"><span data-stu-id="83033-176">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

<span data-ttu-id="83033-177">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="83033-177">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]</span></span>

<span data-ttu-id="83033-178">`DataType` 特性仅提供相关提示来帮助视图引擎设置数据格式（并提供特性，例如向 URL 提供 `<a>` 和向电子邮件提供 `<a href="mailto:EmailAddress.com">`）。</span><span class="sxs-lookup"><span data-stu-id="83033-178">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="83033-179">使用 `RegularExpression` 特性验证数据的格式。</span><span class="sxs-lookup"><span data-stu-id="83033-179">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="83033-180">`DataType` 特性用于指定比数据库内部类型更具体的数据类型。</span><span class="sxs-lookup"><span data-stu-id="83033-180">The `DataType` attribute is used to specify a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="83033-181">`DataType` 特性不是验证特性。</span><span class="sxs-lookup"><span data-stu-id="83033-181">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="83033-182">示例应用程序中仅显示日期，不显示时间。</span><span class="sxs-lookup"><span data-stu-id="83033-182">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="83033-183">`DataType` 枚举提供了多种数据类型，例如日期、时间、电话号码、货币、电子邮件地址等。</span><span class="sxs-lookup"><span data-stu-id="83033-183">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="83033-184">应用程序还可通过 `DataType` 特性自动提供类型特定的功能。</span><span class="sxs-lookup"><span data-stu-id="83033-184">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="83033-185">例如，可为 `DataType.EmailAddress` 创建 `mailto:` 链接。</span><span class="sxs-lookup"><span data-stu-id="83033-185">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="83033-186">可在支持 HTML5 的浏览器中为 `DataType.Date` 提供日期选择器。</span><span class="sxs-lookup"><span data-stu-id="83033-186">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="83033-187">`DataType` 特性发出 HTML 5 `data-`（读作 data dash）特性供 HTML 5 浏览器使用。</span><span class="sxs-lookup"><span data-stu-id="83033-187">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="83033-188">`DataType` 特性不提供任何验证。</span><span class="sxs-lookup"><span data-stu-id="83033-188">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="83033-189">`DataType.Date` 不指定显示日期的格式。</span><span class="sxs-lookup"><span data-stu-id="83033-189">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="83033-190">默认情况下，数据字段根据基于服务器的 `CultureInfo` 的默认格式进行显示。</span><span class="sxs-lookup"><span data-stu-id="83033-190">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

<span data-ttu-id="83033-191">`DisplayFormat` 特性用于显式指定日期格式：</span><span class="sxs-lookup"><span data-stu-id="83033-191">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="83033-192">`ApplyFormatInEditMode` 设置用于指定在显示值进行编辑时需应用格式。</span><span class="sxs-lookup"><span data-stu-id="83033-192">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="83033-193">可能不希望某些字段具有此行为。</span><span class="sxs-lookup"><span data-stu-id="83033-193">You might not want that behavior for some fields.</span></span> <span data-ttu-id="83033-194">例如，在货币值中，可能不希望编辑 UI 中存在货币符号。</span><span class="sxs-lookup"><span data-stu-id="83033-194">For example, in currency values, you probably do not want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="83033-195">可单独使用 `DisplayFormat` 特性，但通常建议使用 `DataType` 特性。</span><span class="sxs-lookup"><span data-stu-id="83033-195">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="83033-196">`DataType` 特性传达数据的语义而不是传达如何在屏幕上呈现数据，并提供 DisplayFormat 不具备的以下优势：</span><span class="sxs-lookup"><span data-stu-id="83033-196">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="83033-197">浏览器可启用 HTML5 功能（例如显示日历控件、区域设置适用的货币符号、电子邮件链接等）</span><span class="sxs-lookup"><span data-stu-id="83033-197">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="83033-198">默认情况下，浏览器将根据区域设置采用正确的格式呈现数据。</span><span class="sxs-lookup"><span data-stu-id="83033-198">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="83033-199">借助 `DataType` 特性，ASP.NET Core 框架可选择适当的字段模板来呈现数据。</span><span class="sxs-lookup"><span data-stu-id="83033-199">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="83033-200">单独使用时，`DisplayFormat` 特性将使用字符串模板。</span><span class="sxs-lookup"><span data-stu-id="83033-200">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="83033-201">请注意：jQuery 验证不适用于 `Range` 特性和 `DateTime`。</span><span class="sxs-lookup"><span data-stu-id="83033-201">Note: jQuery validation does not work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="83033-202">例如，以下代码将始终显示客户端验证错误，即便日期在指定的范围内：</span><span class="sxs-lookup"><span data-stu-id="83033-202">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="83033-203">通常，在模型中编译固定日期是不恰当的，因此不推荐使用 `Range` 特性和 `DateTime`。</span><span class="sxs-lookup"><span data-stu-id="83033-203">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="83033-204">以下代码显示组合在一行上的特性：</span><span class="sxs-lookup"><span data-stu-id="83033-204">The following code shows combining attributes on one line:</span></span>

<span data-ttu-id="83033-205">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="83033-205">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]</span></span>

<span data-ttu-id="83033-206">感谢读完这篇 Razor 页面简介。</span><span class="sxs-lookup"><span data-stu-id="83033-206">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="83033-207">我们期待你的意见。</span><span class="sxs-lookup"><span data-stu-id="83033-207">We appreciate any comments you leave.</span></span> <span data-ttu-id="83033-208">完成本教程后，大力推荐了解 [MVC 和 EF Core 入门](xref:data/ef-mvc/intro)。</span><span class="sxs-lookup"><span data-stu-id="83033-208">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="83033-209">其他资源</span><span class="sxs-lookup"><span data-stu-id="83033-209">Additional resources</span></span>

* [<span data-ttu-id="83033-210">使用表单</span><span class="sxs-lookup"><span data-stu-id="83033-210">Working with Forms</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="83033-211">全球化和本地化</span><span class="sxs-lookup"><span data-stu-id="83033-211">Globalization and localization</span></span>](xref:fundamentals/localization)
* [<span data-ttu-id="83033-212">标记帮助程序简介</span><span class="sxs-lookup"><span data-stu-id="83033-212">Introduction to Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="83033-213">创作标记帮助程序</span><span class="sxs-lookup"><span data-stu-id="83033-213">Authoring Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)

>[!div class="step-by-step"]
[<span data-ttu-id="83033-214">上一篇：添加新字段</span><span class="sxs-lookup"><span data-stu-id="83033-214">Previous: Adding a new field</span></span>](xref:tutorials/razor-pages/new-field)