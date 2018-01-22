---
title: "ASP.NET 核心 mvc 模型验证"
author: rachelappel
description: "了解有关 ASP.NET 核心 mvc 模型验证。"
ms.author: riande
manager: wpickett
ms.date: 12/18/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/validation
ms.openlocfilehash: 91db17e103723ac411a2ad4f3f9549860f250cce
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a><span data-ttu-id="61658-103">ASP.NET 核心 mvc 模型验证简介</span><span class="sxs-lookup"><span data-stu-id="61658-103">Introduction to model validation in ASP.NET Core MVC</span></span>

<span data-ttu-id="61658-104">通过[Rachel Appel](https://github.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="61658-104">By [Rachel Appel](https://github.com/rachelappel)</span></span>

## <a name="introduction-to-model-validation"></a><span data-ttu-id="61658-105">模型验证简介</span><span class="sxs-lookup"><span data-stu-id="61658-105">Introduction to model validation</span></span>

<span data-ttu-id="61658-106">应用程序在数据库中存储数据之前，应用程序必须验证数据。</span><span class="sxs-lookup"><span data-stu-id="61658-106">Before an app stores data in a database, the app must validate the data.</span></span> <span data-ttu-id="61658-107">必须检查潜在的安全威胁，验证相应地设置格式的类型和大小，并且它必须符合规则的数据。</span><span class="sxs-lookup"><span data-stu-id="61658-107">Data must be checked for potential security threats, verified that it is appropriately formatted by type and size, and it must conform to your rules.</span></span> <span data-ttu-id="61658-108">验证是必需的但也可以是冗余且单调乏味实现。</span><span class="sxs-lookup"><span data-stu-id="61658-108">Validation is necessary although it can be redundant and tedious to implement.</span></span> <span data-ttu-id="61658-109">在 MVC 中，验证在客户端和服务器上发生。</span><span class="sxs-lookup"><span data-stu-id="61658-109">In MVC, validation happens on both the client and server.</span></span>

<span data-ttu-id="61658-110">幸运的是，.NET 已提取到验证特性的验证。</span><span class="sxs-lookup"><span data-stu-id="61658-110">Fortunately, .NET has abstracted validation into validation attributes.</span></span> <span data-ttu-id="61658-111">这些属性包含验证代码，从而减少了必须编写的代码量。</span><span class="sxs-lookup"><span data-stu-id="61658-111">These attributes contain validation code, thereby reducing the amount of code you must write.</span></span>

<span data-ttu-id="61658-112">[查看或从 GitHub 下载示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample)。</span><span class="sxs-lookup"><span data-stu-id="61658-112">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).</span></span>

## <a name="validation-attributes"></a><span data-ttu-id="61658-113">验证特性</span><span class="sxs-lookup"><span data-stu-id="61658-113">Validation Attributes</span></span>

<span data-ttu-id="61658-114">验证特性是一种方法配置模型验证，因此从概念上讲类似验证对数据库表中的字段。</span><span class="sxs-lookup"><span data-stu-id="61658-114">Validation attributes are a way to configure model validation so it's similar conceptually to validation on fields in database tables.</span></span> <span data-ttu-id="61658-115">这包括如将数据类型或必填的字段分配的约束。</span><span class="sxs-lookup"><span data-stu-id="61658-115">This includes constraints such as assigning data types or required fields.</span></span> <span data-ttu-id="61658-116">其他类型的验证包括将模式应用于数据以强制实施业务规则，如信用卡号，电话号码或电子邮件地址。</span><span class="sxs-lookup"><span data-stu-id="61658-116">Other types of validation include applying patterns to data to enforce business rules, such as a credit card, phone number, or email address.</span></span> <span data-ttu-id="61658-117">验证特性使强制实施更简单、 更轻松地使用这些要求。</span><span class="sxs-lookup"><span data-stu-id="61658-117">Validation attributes make enforcing these requirements much simpler and easier to use.</span></span>

<span data-ttu-id="61658-118">下面是带批注`Movie`模型从的应用程序存储有关电影和电视节目的信息。</span><span class="sxs-lookup"><span data-stu-id="61658-118">Below is an annotated `Movie` model from an app that stores information about movies and TV shows.</span></span> <span data-ttu-id="61658-119">大多数属性都需要和多个字符串属性具有长度要求。</span><span class="sxs-lookup"><span data-stu-id="61658-119">Most of the properties are required and several string properties have length requirements.</span></span> <span data-ttu-id="61658-120">此外，没有在此处的数值范围限制`Price`从 0 到 $999.99，以及自定义验证特性的属性。</span><span class="sxs-lookup"><span data-stu-id="61658-120">Additionally, there is a numeric range restriction in place for the `Price` property from 0 to $999.99, along with a custom validation attribute.</span></span>

[!code-csharp[Main](validation/sample/Movie.cs?range=6-29)]

<span data-ttu-id="61658-121">只需读取通过模型将显示有关此应用程序，从而更便于维护代码的数据的规则。</span><span class="sxs-lookup"><span data-stu-id="61658-121">Simply reading through the model reveals the rules about data for this app, making it easier to maintain the code.</span></span> <span data-ttu-id="61658-122">下面是几个常用的内置验证特性：</span><span class="sxs-lookup"><span data-stu-id="61658-122">Below are several popular built-in validation attributes:</span></span>

* <span data-ttu-id="61658-123">`[CreditCard]`： 验证该属性具有信用卡格式。</span><span class="sxs-lookup"><span data-stu-id="61658-123">`[CreditCard]`: Validates the property has a credit card format.</span></span>

* <span data-ttu-id="61658-124">`[Compare]`： 验证在模型匹配的两个属性。</span><span class="sxs-lookup"><span data-stu-id="61658-124">`[Compare]`: Validates two properties in a model match.</span></span>

* <span data-ttu-id="61658-125">`[EmailAddress]`： 验证该属性具有电子邮件格式。</span><span class="sxs-lookup"><span data-stu-id="61658-125">`[EmailAddress]`: Validates the property has an email format.</span></span>

* <span data-ttu-id="61658-126">`[Phone]`： 验证该属性具有电话格式。</span><span class="sxs-lookup"><span data-stu-id="61658-126">`[Phone]`: Validates the property has a telephone format.</span></span>

* <span data-ttu-id="61658-127">`[Range]`： 验证该属性值降到给定范围内。</span><span class="sxs-lookup"><span data-stu-id="61658-127">`[Range]`: Validates the property value falls within the given range.</span></span>

* <span data-ttu-id="61658-128">`[RegularExpression]`： 验证的数据与指定的正则表达式匹配。</span><span class="sxs-lookup"><span data-stu-id="61658-128">`[RegularExpression]`: Validates that the data matches the specified regular expression.</span></span>

* <span data-ttu-id="61658-129">`[Required]`： 使要求的属性。</span><span class="sxs-lookup"><span data-stu-id="61658-129">`[Required]`: Makes a property required.</span></span>

* <span data-ttu-id="61658-130">`[StringLength]`： 验证字符串属性最多包含给定的最大长度。</span><span class="sxs-lookup"><span data-stu-id="61658-130">`[StringLength]`: Validates that a string property has at most the given maximum length.</span></span>

* <span data-ttu-id="61658-131">`[Url]`： 验证该属性具有的 URL 格式。</span><span class="sxs-lookup"><span data-stu-id="61658-131">`[Url]`: Validates the property has a URL format.</span></span>

<span data-ttu-id="61658-132">MVC 支持任何特性的派生自`ValidationAttribute`来进行验证。</span><span class="sxs-lookup"><span data-stu-id="61658-132">MVC supports any attribute that derives from `ValidationAttribute` for validation purposes.</span></span> <span data-ttu-id="61658-133">在找不到许多有用的验证属性[System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations)命名空间。</span><span class="sxs-lookup"><span data-stu-id="61658-133">Many useful validation attributes can be found in the [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations) namespace.</span></span>

<span data-ttu-id="61658-134">可能需要更多的功能比内置属性提供的实例。</span><span class="sxs-lookup"><span data-stu-id="61658-134">There may be instances where you need more features than built-in attributes provide.</span></span> <span data-ttu-id="61658-135">对于这些时间中，你可以创建自定义验证特性的派生自`ValidationAttribute`或更改您的模型来实现`IValidatableObject`。</span><span class="sxs-lookup"><span data-stu-id="61658-135">For those times, you can create custom validation attributes by deriving from `ValidationAttribute` or changing your model to implement `IValidatableObject`.</span></span>

## <a name="notes-on-the-use-of-the-required-attribute"></a><span data-ttu-id="61658-136">必需的特性的使用说明</span><span class="sxs-lookup"><span data-stu-id="61658-136">Notes on the use of the Required attribute</span></span>

<span data-ttu-id="61658-137">从本质上来说，需要不可以为 null 的[值类型](/dotnet/csharp/language-reference/keywords/value-types)（如 `decimal`、`int`、`float` 和 `DateTime`），但不需要 `Required` 特性。</span><span class="sxs-lookup"><span data-stu-id="61658-137">Non-nullable [value types](/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, and `DateTime`) are inherently required and don't need the `Required` attribute.</span></span> <span data-ttu-id="61658-138">该应用程序执行的不可以为 null 的类型的标记的任何服务器端验证检查`Required`。</span><span class="sxs-lookup"><span data-stu-id="61658-138">The app performs no server-side validation checks for non-nullable types that are marked `Required`.</span></span>

<span data-ttu-id="61658-139">MVC 模型绑定，并不关心的验证和验证特性，拒绝包含缺少的值或非 null 的类型的空白窗体字段提交。</span><span class="sxs-lookup"><span data-stu-id="61658-139">MVC model binding, which isn't concerned with validation and validation attributes, rejects a form field submission containing a missing value or whitespace for a non-nullable type.</span></span> <span data-ttu-id="61658-140">在缺少`BindRequired`属性上的目标属性中，模型绑定忽略缺少数据不可为 null 的类型，其中的表单域不存在从传入的窗体数据。</span><span class="sxs-lookup"><span data-stu-id="61658-140">In the absence of a `BindRequired` attribute on the target property, model binding ignores missing data for non-nullable types, where the form field is absent from the incoming form data.</span></span>

<span data-ttu-id="61658-141">[BindRequired 属性](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute)(另请参阅[自定义模型具有属性的绑定行为](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) 有助于确保窗体数据是否完整。</span><span class="sxs-lookup"><span data-stu-id="61658-141">The [BindRequired attribute](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (also see [Customize model binding behavior with attributes](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) is useful to ensure form data is complete.</span></span> <span data-ttu-id="61658-142">当应用于一个属性，以及模型绑定系统将需要该属性的值。</span><span class="sxs-lookup"><span data-stu-id="61658-142">When applied to a property, the model binding system requires a value for that property.</span></span> <span data-ttu-id="61658-143">应用于类型时，模型绑定系统将要求提供的所有该类型的属性的值。</span><span class="sxs-lookup"><span data-stu-id="61658-143">When applied to a type, the model binding system requires values for all of the properties of that type.</span></span>

<span data-ttu-id="61658-144">当你使用[可以为 Null\<T > 类型](/dotnet/csharp/programming-guide/nullable-types/)(例如，`decimal?`或`System.Nullable<decimal>`) 并将其标记`Required`，就像该属性是标准的可以为 null 类型 （对于执行服务器端验证检查示例中， `string`)。</span><span class="sxs-lookup"><span data-stu-id="61658-144">When you use a [Nullable\<T> type](/dotnet/csharp/programming-guide/nullable-types/) (for example, `decimal?` or `System.Nullable<decimal>`) and mark it `Required`, a server-side validation check is performed as if the property were a standard nullable type (for example, a `string`).</span></span>

<span data-ttu-id="61658-145">客户端验证对应于一个模型属性，已标记为窗体字段需要值`Required`和尚未标记为不可为 null 的类型属性`Required`。</span><span class="sxs-lookup"><span data-stu-id="61658-145">Client-side validation requires a value for a form field that corresponds to a model property that you've marked `Required` and for a non-nullable type property that you haven't marked `Required`.</span></span> <span data-ttu-id="61658-146">`Required`可以用于控制客户端验证错误消息。</span><span class="sxs-lookup"><span data-stu-id="61658-146">`Required` can be used to control the client-side validation error message.</span></span>

## <a name="model-state"></a><span data-ttu-id="61658-147">模型状态</span><span class="sxs-lookup"><span data-stu-id="61658-147">Model State</span></span>

<span data-ttu-id="61658-148">模型状态表示提交 HTML 窗体值中的验证错误。</span><span class="sxs-lookup"><span data-stu-id="61658-148">Model state represents validation errors in submitted HTML form values.</span></span>

<span data-ttu-id="61658-149">MVC 将继续验证域，直到达到最大错误 (默认情况下为 200) 数。</span><span class="sxs-lookup"><span data-stu-id="61658-149">MVC will continue validating fields until reaches the maximum number of errors (200 by default).</span></span> <span data-ttu-id="61658-150">你可以配置该数字的方法是插入下面的代码插入`ConfigureServices`中的方法*Startup.cs*文件：</span><span class="sxs-lookup"><span data-stu-id="61658-150">You can configure this number by inserting the following code into the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a><span data-ttu-id="61658-151">处理模型状态错误</span><span class="sxs-lookup"><span data-stu-id="61658-151">Handling Model State Errors</span></span>

<span data-ttu-id="61658-152">在正在调用的每个控制器操作之前，将执行模型验证和操作方法负责检查`ModelState.IsValid`并相应地作出反应。</span><span class="sxs-lookup"><span data-stu-id="61658-152">Model validation occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="61658-153">在许多情况下，正确的反应是返回错误响应，理想情况下详细说明模型验证失败的原因。</span><span class="sxs-lookup"><span data-stu-id="61658-153">In many cases, the appropriate reaction is to return an error response, ideally detailing the reason why model validation failed.</span></span>

<span data-ttu-id="61658-154">某些应用程序会选择遵守标准的约定来处理模型验证错误，在该情况下筛选器可能会为适当的位置，若要实现此类策略。</span><span class="sxs-lookup"><span data-stu-id="61658-154">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a filter may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="61658-155">你应测试你的操作与有效和无效的模型状态的行为方式。</span><span class="sxs-lookup"><span data-stu-id="61658-155">You should test how your actions behave with valid and invalid model states.</span></span>

## <a name="manual-validation"></a><span data-ttu-id="61658-156">手动验证</span><span class="sxs-lookup"><span data-stu-id="61658-156">Manual validation</span></span>

<span data-ttu-id="61658-157">模型绑定和验证完成后，你可能需要重复它的部分。</span><span class="sxs-lookup"><span data-stu-id="61658-157">After model binding and validation are complete, you may want to repeat parts of it.</span></span> <span data-ttu-id="61658-158">例如，用户可能具有文本在字段中输入应为整数，或可能需要计算模型的属性的值。</span><span class="sxs-lookup"><span data-stu-id="61658-158">For example, a user may have entered text in a field expecting an integer, or you may need to compute a value for a model's property.</span></span>

<span data-ttu-id="61658-159">你可能需要手动运行验证。</span><span class="sxs-lookup"><span data-stu-id="61658-159">You may need to run validation manually.</span></span> <span data-ttu-id="61658-160">为此，请调用`TryValidateModel`方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="61658-160">To do so, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a><span data-ttu-id="61658-161">自定义验证</span><span class="sxs-lookup"><span data-stu-id="61658-161">Custom validation</span></span>

<span data-ttu-id="61658-162">验证特性适用于大多数验证需求。</span><span class="sxs-lookup"><span data-stu-id="61658-162">Validation attributes work for most validation needs.</span></span> <span data-ttu-id="61658-163">但是，一些的验证规则是特定于你的业务。</span><span class="sxs-lookup"><span data-stu-id="61658-163">However, some validation rules are specific to your business.</span></span> <span data-ttu-id="61658-164">你的规则可能不是常见数据验证技术，如确保字段是必需的或符合一系列的值。</span><span class="sxs-lookup"><span data-stu-id="61658-164">Your rules might not be common data validation techniques such as ensuring a field is required or that it conforms to a range of values.</span></span> <span data-ttu-id="61658-165">对于这些情况下，自定义验证的特性是不错的解决方案。</span><span class="sxs-lookup"><span data-stu-id="61658-165">For these scenarios, custom validation attributes are a great solution.</span></span> <span data-ttu-id="61658-166">在 MVC 创建你自己的自定义验证特性很容易。</span><span class="sxs-lookup"><span data-stu-id="61658-166">Creating your own custom validation attributes in MVC is easy.</span></span> <span data-ttu-id="61658-167">只需从继承`ValidationAttribute`，并重写`IsValid`方法。</span><span class="sxs-lookup"><span data-stu-id="61658-167">Just inherit from the `ValidationAttribute`, and override the `IsValid` method.</span></span> <span data-ttu-id="61658-168">`IsValid`方法接受两个参数，第一种是已命名的对象*值*第二项是`ValidationContext`对象名为*validationContext*。</span><span class="sxs-lookup"><span data-stu-id="61658-168">The `IsValid` method accepts two parameters, the first is an object named *value* and the second is a `ValidationContext` object named *validationContext*.</span></span> <span data-ttu-id="61658-169">*值*从验证你的自定义验证程序的字段引用的实际值。</span><span class="sxs-lookup"><span data-stu-id="61658-169">*Value* refers to the actual value from the field that your custom validator is validating.</span></span>

<span data-ttu-id="61658-170">在下面的示例中，业务规则规定，用户不可能设置流派为*经典*对于影片之后 1960年发布。</span><span class="sxs-lookup"><span data-stu-id="61658-170">In the following sample, a business rule states that users may not set the genre to *Classic* for a movie released after 1960.</span></span> <span data-ttu-id="61658-171">`[ClassicMovie]`属性会首先，检查流派，如果它是标准，然后它检查发布日期是否晚于 1960年。</span><span class="sxs-lookup"><span data-stu-id="61658-171">The `[ClassicMovie]` attribute checks the genre first, and if it is a classic, then it checks the release date to see that it is later than 1960.</span></span> <span data-ttu-id="61658-172">如果它将被释放后 1960年，验证将失败。</span><span class="sxs-lookup"><span data-stu-id="61658-172">If it is released after 1960, validation fails.</span></span> <span data-ttu-id="61658-173">此属性接受一个表示可用于验证数据的年份的整数参数。</span><span class="sxs-lookup"><span data-stu-id="61658-173">The attribute accepts an integer parameter representing the year that you can use to validate data.</span></span> <span data-ttu-id="61658-174">你可以捕获该特性的构造函数中参数的值，如下所示：</span><span class="sxs-lookup"><span data-stu-id="61658-174">You can capture the value of the parameter in the attribute's constructor, as shown here:</span></span>

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-29)]

<span data-ttu-id="61658-175">`movie`变量表示上面`Movie`包含窗体提交以验证中的数据的对象。</span><span class="sxs-lookup"><span data-stu-id="61658-175">The `movie` variable above represents a `Movie` object that contains the data from the form submission to validate.</span></span> <span data-ttu-id="61658-176">在这种情况下，验证代码检查的日期和中的流派`IsValid`方法`ClassicMovieAttribute`根据规则类。</span><span class="sxs-lookup"><span data-stu-id="61658-176">In this case, the validation code checks the date and genre in the `IsValid` method of the `ClassicMovieAttribute` class as per the rules.</span></span> <span data-ttu-id="61658-177">在成功地验证`IsValid`返回`ValidationResult.Success`代码，并在验证失败时，`ValidationResult`并显示错误消息。</span><span class="sxs-lookup"><span data-stu-id="61658-177">Upon successful validation `IsValid` returns a `ValidationResult.Success` code, and when validation fails, a `ValidationResult` with an error message.</span></span> <span data-ttu-id="61658-178">当用户修改`Genre`字段并提交该表单，`IsValid`方法`ClassicMovieAttribute`将验证电影是否为标准。</span><span class="sxs-lookup"><span data-stu-id="61658-178">When a user modifies the `Genre` field and submits the form, the `IsValid` method of the `ClassicMovieAttribute` will verify whether the movie is a classic.</span></span> <span data-ttu-id="61658-179">与任何内置属性，类似应用`ClassicMovieAttribute`到属性，如`ReleaseDate`以确保验证执行，如前面的代码示例中所示。</span><span class="sxs-lookup"><span data-stu-id="61658-179">Like any built-in attribute, apply the `ClassicMovieAttribute` to a property such as `ReleaseDate` to ensure validation happens, as shown in the previous code sample.</span></span> <span data-ttu-id="61658-180">由于此示例仅处理`Movie`类型，更好的选择是使用`IValidatableObject`下面这一段中所示。</span><span class="sxs-lookup"><span data-stu-id="61658-180">Since the example works only with `Movie` types, a better option is to use `IValidatableObject` as shown in the following paragraph.</span></span>

<span data-ttu-id="61658-181">或者，这一相同代码无法放置在模型中通过实现`Validate`方法`IValidatableObject`接口。</span><span class="sxs-lookup"><span data-stu-id="61658-181">Alternatively, this same code could be placed in the model by implementing the `Validate` method on the `IValidatableObject` interface.</span></span> <span data-ttu-id="61658-182">当自定义验证特性适用于验证单独的属性时，实现`IValidatableObject`可以用来实现类级别验证，如下所示。</span><span class="sxs-lookup"><span data-stu-id="61658-182">While custom validation attributes work well for validating individual properties, implementing `IValidatableObject` can be used to implement class-level validation as seen here.</span></span>

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a><span data-ttu-id="61658-183">客户端验证</span><span class="sxs-lookup"><span data-stu-id="61658-183">Client side validation</span></span>

<span data-ttu-id="61658-184">客户端验证是一个重大便利的用户。</span><span class="sxs-lookup"><span data-stu-id="61658-184">Client side validation is a great convenience for users.</span></span> <span data-ttu-id="61658-185">它将保存它们需要花费时间的时间等待往返行程到服务器。</span><span class="sxs-lookup"><span data-stu-id="61658-185">It saves time they would otherwise spend waiting for a round trip to the server.</span></span> <span data-ttu-id="61658-186">在业务术语中，甚至几秒的乘积数百次每一天的秒的小数部分将添加最多会导致大量时间，费用，并减少失败。</span><span class="sxs-lookup"><span data-stu-id="61658-186">In business terms, even a few fractions of seconds multiplied hundreds of times each day adds up to be a lot of time, expense, and frustration.</span></span> <span data-ttu-id="61658-187">简单和快速验证使用户能够更有效地工作，并生成更好的质量输入和输出。</span><span class="sxs-lookup"><span data-stu-id="61658-187">Straightforward and immediate validation enables users to work more efficiently and produce better quality input and output.</span></span>

<span data-ttu-id="61658-188">你必须具有正确的 JavaScript 脚本引用的视图中的客户端验证工作如下所示准备好。</span><span class="sxs-lookup"><span data-stu-id="61658-188">You must have a view with the proper JavaScript script references in place for client side validation to work as you see here.</span></span>

[!code-cshtml[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

<span data-ttu-id="61658-189">[JQuery 非介入式验证](https://github.com/aspnet/jquery-validation-unobtrusive)脚本是基于流行的自定义 Microsoft 前端库[jQuery 验证](https://jqueryvalidation.org/)插件。</span><span class="sxs-lookup"><span data-stu-id="61658-189">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="61658-190">如果没有 jQuery 非介入式验证，你将必须在两个位置相同的验证逻辑的代码： 一次是在服务器端验证属性对模型属性，然后再次在客户端脚本 (jQuery 验证的示例[ `validate()`](https://jqueryvalidation.org/validate/)方法演示如何复杂可能会很大)。</span><span class="sxs-lookup"><span data-stu-id="61658-190">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server side validation attributes on model properties, and then again in client side scripts (the examples for jQuery Validate's [`validate()`](https://jqueryvalidation.org/validate/) method shows how complex this could become).</span></span> <span data-ttu-id="61658-191">相反，MVC 的[标记帮助程序](xref:mvc/views/tag-helpers/intro)和[HTML 帮助器](xref:mvc/views/overview)能够使用验证属性和类型元数据的模型属性呈现 HTML 5[数据特性](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes)中需要验证窗体元素。</span><span class="sxs-lookup"><span data-stu-id="61658-191">Instead, MVC's [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) are able to use the validation attributes and type metadata from model properties to render HTML 5 [data- attributes](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) in the form elements that need validation.</span></span> <span data-ttu-id="61658-192">MVC 生成`data-`内置和自定义属性的属性。</span><span class="sxs-lookup"><span data-stu-id="61658-192">MVC generates the `data-` attributes for both built-in and custom attributes.</span></span> <span data-ttu-id="61658-193">jQuery 非介入式验证然后分析这些注册表`data-`属性并将逻辑传递给 jQuery 验证，有效地"复制"服务器端验证逻辑到客户端。</span><span class="sxs-lookup"><span data-stu-id="61658-193">jQuery Unobtrusive Validation then parses thes `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server side validation logic to the client.</span></span> <span data-ttu-id="61658-194">你可以使用如下所示的相关标记帮助器客户端上显示验证错误：</span><span class="sxs-lookup"><span data-stu-id="61658-194">You can display validation errors on the client using the relevant tag helpers as shown here:</span></span>

[!code-cshtml[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

<span data-ttu-id="61658-195">上面的标记帮助器呈现的 HTML 下面。</span><span class="sxs-lookup"><span data-stu-id="61658-195">The tag helpers above render the HTML below.</span></span> <span data-ttu-id="61658-196">请注意， `data-` HTML 中的属性输出对应的验证属性`ReleaseDate`属性。</span><span class="sxs-lookup"><span data-stu-id="61658-196">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="61658-197">`data-val-required`下面的属性包含要显示如果用户未填满发行日期字段中的错误消息。</span><span class="sxs-lookup"><span data-stu-id="61658-197">The `data-val-required` attribute below contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="61658-198">jQuery 非介入式验证将此值传递给 jQuery 验证[ `required()` ](https://jqueryvalidation.org/required-method/)方法，然后在随附的说明将显示该消息**\<跨越 >**元素。</span><span class="sxs-lookup"><span data-stu-id="61658-198">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

<span data-ttu-id="61658-199">客户端验证阻止提交，直到该窗体是有效。</span><span class="sxs-lookup"><span data-stu-id="61658-199">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="61658-200">提交按钮运行 JavaScript 提交窗体或显示错误消息。</span><span class="sxs-lookup"><span data-stu-id="61658-200">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="61658-201">MVC 确定基于.NET 数据类型的属性，可能使用重写的类型属性值`[DataType]`属性。</span><span class="sxs-lookup"><span data-stu-id="61658-201">MVC determines type attribute values based on the .NET data type of a property, possibly overridden using `[DataType]` attributes.</span></span> <span data-ttu-id="61658-202">基`[DataType]`属性不会实际的服务器端验证。</span><span class="sxs-lookup"><span data-stu-id="61658-202">The base `[DataType]` attribute does no real server-side validation.</span></span> <span data-ttu-id="61658-203">浏览器选择自己的错误消息，并显示这些错误，但他们希望，但是 jQuery 验证非介入式包可以替代的消息，并与其他一致地显示它们。</span><span class="sxs-lookup"><span data-stu-id="61658-203">Browsers choose their own error messages and display those errors however they wish, however the jQuery Validation Unobtrusive package can override the messages and display them consistently with others.</span></span> <span data-ttu-id="61658-204">发生这种情况最明显时用户应用`[DataType]`如子类`[EmailAddress]`。</span><span class="sxs-lookup"><span data-stu-id="61658-204">This happens most obviously when users apply `[DataType]` subclasses such as `[EmailAddress]`.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="61658-205">将验证添加到动态窗体</span><span class="sxs-lookup"><span data-stu-id="61658-205">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="61658-206">因为第一次加载页面时项目，jQuery 非介入式验证都会将验证逻辑和参数传递到 jQuery 验证中，则动态生成的窗体将不会自动出现验证。</span><span class="sxs-lookup"><span data-stu-id="61658-206">Because jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads, dynamically generated forms will not automatically exhibit validation.</span></span> <span data-ttu-id="61658-207">相反，你必须告知 jQuery 非介入式验证要在创建后立即分析动态窗体。</span><span class="sxs-lookup"><span data-stu-id="61658-207">Instead, you must tell jQuery Unobtrusive Validation to parse the dynamic form immediately after creating it.</span></span> <span data-ttu-id="61658-208">例如，下面的代码演示可能设置通过 AJAX 添加窗体上的客户端验证的方式。</span><span class="sxs-lookup"><span data-stu-id="61658-208">For example, the code below shows how you might set up client side validation on a form added via AJAX.</span></span>

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Could not add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

<span data-ttu-id="61658-209">`$.validator.unobtrusive.parse()`方法接受它的一个参数的 jQuery 选择器。</span><span class="sxs-lookup"><span data-stu-id="61658-209">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="61658-210">此方法指示 jQuery 非介入式验证分析`data-`该选择器中的窗体的属性。</span><span class="sxs-lookup"><span data-stu-id="61658-210">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="61658-211">这些特性的值然后传递给 jQuery 验证插件中，这样窗体表现出所需的客户端验证规则。</span><span class="sxs-lookup"><span data-stu-id="61658-211">The values of those attributes are then passed to the jQuery Validate plugin so that the form exhibits the desired client side validation rules.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="61658-212">将验证添加到动态控件</span><span class="sxs-lookup"><span data-stu-id="61658-212">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="61658-213">你还可以更新窗体上的验证规则，当单个控件，如`<input/>`s 和`<select/>`s，动态生成。</span><span class="sxs-lookup"><span data-stu-id="61658-213">You can also update the validation rules on a form when individual controls, such as `<input/>`s and `<select/>`s, are dynamically generated.</span></span> <span data-ttu-id="61658-214">不能将传递到这些元素选择器`parse()`方法直接因为周围的窗体已分析并不会更新。</span><span class="sxs-lookup"><span data-stu-id="61658-214">You cannot pass selectors for these elements to the `parse()` method directly because the surrounding form has already been parsed and will not update.</span></span> <span data-ttu-id="61658-215">相反，你首先删除现有的验证数据，然后重新分析整个窗体，如下所示：</span><span class="sxs-lookup"><span data-stu-id="61658-215">Instead, you first remove the existing validation data, then reparse the entire form, as shown below:</span></span>

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Could not add form. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        form.removeData("validator")    // Added by the raw jQuery Validate
            .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a><span data-ttu-id="61658-216">IClientModelValidator</span><span class="sxs-lookup"><span data-stu-id="61658-216">IClientModelValidator</span></span>

<span data-ttu-id="61658-217">可能你的自定义特性，创建客户端逻辑和[非介入式验证](http://jqueryvalidation.org/documentation/)将为你自动作为一部分验证客户端上执行。</span><span class="sxs-lookup"><span data-stu-id="61658-217">You may create client side logic for your custom attribute, and [unobtrusive validation](http://jqueryvalidation.org/documentation/) will execute it on the client for you automatically as part of validation.</span></span> <span data-ttu-id="61658-218">第一步是控制哪些数据特性添加通过实现`IClientModelValidator`接口如下所示：</span><span class="sxs-lookup"><span data-stu-id="61658-218">The first step is to control what data- attributes are added by implementing the `IClientModelValidator` interface as shown here:</span></span>

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

<span data-ttu-id="61658-219">实现此接口的属性可以将 HTML 特性添加到生成的字段。</span><span class="sxs-lookup"><span data-stu-id="61658-219">Attributes that implement this interface can add HTML attributes to generated fields.</span></span> <span data-ttu-id="61658-220">检查的输出`ReleaseDate`元素显示非常相似与前面的示例中，但现在没有的 HTML`data-val-classicmovie`属性中定义`AddValidation`方法`IClientModelValidator`。</span><span class="sxs-lookup"><span data-stu-id="61658-220">Examining the output for the `ReleaseDate` element reveals HTML that is similar to the previous example, except now there is a `data-val-classicmovie` attribute that was defined in the `AddValidation` method of `IClientModelValidator`.</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

<span data-ttu-id="61658-221">非介入式验证将使用中的数据`data-`属性来显示错误消息。</span><span class="sxs-lookup"><span data-stu-id="61658-221">Unobtrusive validation uses the data in the `data-` attributes to display error messages.</span></span> <span data-ttu-id="61658-222">但是，jQuery 不知道有关规则或消息之前将它们添加到 jQuery 的`validator`对象。</span><span class="sxs-lookup"><span data-stu-id="61658-222">However, jQuery doesn't know about rules or messages until you add them to jQuery's `validator` object.</span></span> <span data-ttu-id="61658-223">这下面添加一个名为方法的示例中所示`classicmovie`包含自定义客户端验证代码，以便 jQuery`validator`对象。</span><span class="sxs-lookup"><span data-stu-id="61658-223">This is shown in the example below that adds a method named `classicmovie` containing custom client validation code to the jQuery `validator` object.</span></span>

[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

<span data-ttu-id="61658-224">现在 jQuery 具有的信息执行自定义 JavaScript 验证，以及要显示如果该验证代码返回的错误消息。</span><span class="sxs-lookup"><span data-stu-id="61658-224">Now jQuery has the information to execute the custom JavaScript validation as well as the error message to display if that validation code returns false.</span></span>

## <a name="remote-validation"></a><span data-ttu-id="61658-225">远程验证</span><span class="sxs-lookup"><span data-stu-id="61658-225">Remote validation</span></span>

<span data-ttu-id="61658-226">远程验证是很有用的功能，你需要验证服务器上的数据对客户端上的数据时要使用。</span><span class="sxs-lookup"><span data-stu-id="61658-226">Remote validation is a great feature to use when you need to validate data on the client against data on the server.</span></span> <span data-ttu-id="61658-227">例如，你的应用程序可能需要验证是否电子邮件或用户的名称已在使用，并且它必须查询大量的数据进行管理。</span><span class="sxs-lookup"><span data-stu-id="61658-227">For example, your app may need to verify whether an email or user name is already in use, and it must query a large amount of data to do so.</span></span> <span data-ttu-id="61658-228">下载大型数据集的用于验证一个或几个字段会占用过多资源。</span><span class="sxs-lookup"><span data-stu-id="61658-228">Downloading large sets of data for validating one or a few fields consumes too many resources.</span></span> <span data-ttu-id="61658-229">它还可以公开敏感信息。</span><span class="sxs-lookup"><span data-stu-id="61658-229">It may also expose sensitive information.</span></span> <span data-ttu-id="61658-230">一种替代方法是发出往返的请求，以验证字段。</span><span class="sxs-lookup"><span data-stu-id="61658-230">An alternative is to make a round-trip request to validate a field.</span></span>

<span data-ttu-id="61658-231">你可以在一个双步骤过程中实现远程验证。</span><span class="sxs-lookup"><span data-stu-id="61658-231">You can implement remote validation in a two step process.</span></span> <span data-ttu-id="61658-232">首先，必须批注与模型`[Remote]`属性。</span><span class="sxs-lookup"><span data-stu-id="61658-232">First, you must annotate your model with the `[Remote]` attribute.</span></span> <span data-ttu-id="61658-233">`[Remote]`属性接受多个重载可用于将定向到相应的代码以调用的客户端 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="61658-233">The `[Remote]` attribute accepts multiple overloads you can use to direct client side JavaScript to the appropriate code to call.</span></span> <span data-ttu-id="61658-234">下面的示例指向`VerifyEmail`操作方法`Users`控制器。</span><span class="sxs-lookup"><span data-stu-id="61658-234">The example below points to the `VerifyEmail` action method of the `Users` controller.</span></span>

[!code-csharp[Main](validation/sample/User.cs?range=7-8)]

<span data-ttu-id="61658-235">中定义的第二步将验证代码放在相应的操作方法`[Remote]`属性。</span><span class="sxs-lookup"><span data-stu-id="61658-235">The second step is putting the validation code in the corresponding action method as defined in the `[Remote]` attribute.</span></span> <span data-ttu-id="61658-236">根据 jQuery 验证[ `remote()` ](https://jqueryvalidation.org/remote-method/)方法文档：</span><span class="sxs-lookup"><span data-stu-id="61658-236">According to the jQuery Validate [`remote()`](https://jqueryvalidation.org/remote-method/) method documentation:</span></span>

> <span data-ttu-id="61658-237">Serverside 响应必须是必须的 JSON 字符串`"true"`对于有效的元素，并且可以`"false"`， `undefined`，或`null`无效元素，使用默认的错误消息。</span><span class="sxs-lookup"><span data-stu-id="61658-237">The serverside response must be a JSON string that must be `"true"` for valid elements, and can be `"false"`, `undefined`, or `null` for invalid elements, using the default error message.</span></span> <span data-ttu-id="61658-238">如果 serverside 响应是一个字符串，例如。</span><span class="sxs-lookup"><span data-stu-id="61658-238">If the serverside response is a string, eg.</span></span> <span data-ttu-id="61658-239">`"That name is already taken, try peter123 instead"`此字符串将显示为以取代默认的自定义错误消息。</span><span class="sxs-lookup"><span data-stu-id="61658-239">`"That name is already taken, try peter123 instead"`, this string will be displayed as a custom error message in place of the default.</span></span>

<span data-ttu-id="61658-240">定义`VerifyEmail()`方法遵循这些规则，如下所示。</span><span class="sxs-lookup"><span data-stu-id="61658-240">The definition of the `VerifyEmail()` method follows these rules, as shown below.</span></span> <span data-ttu-id="61658-241">它将返回验证错误消息如果采用电子邮件，或`true`如果电子邮件是免费的并将包装中的结果`JsonResult`对象。</span><span class="sxs-lookup"><span data-stu-id="61658-241">It returns a validation error message if the email is taken, or `true` if the email is free, and wraps the result in a `JsonResult` object.</span></span> <span data-ttu-id="61658-242">然后，客户端可以使用返回的值以继续，或如果需要显示错误。</span><span class="sxs-lookup"><span data-stu-id="61658-242">The client side can then use the returned value to proceed or display the error if needed.</span></span>

[!code-csharp[Main](validation/sample/UsersController.cs?range=19-28)]

<span data-ttu-id="61658-243">现在当用户输入的电子邮件，在视图中的 JavaScript 进行远程调用以查看该电子邮件已并且，如果是这样，显示的错误消息。</span><span class="sxs-lookup"><span data-stu-id="61658-243">Now when users enter an email, JavaScript in the view makes a remote call to see if that email has been taken and, if so, displays the error message.</span></span> <span data-ttu-id="61658-244">否则，用户可以像往常一样提交窗体。</span><span class="sxs-lookup"><span data-stu-id="61658-244">Otherwise, the user can submit the form as usual.</span></span>

<span data-ttu-id="61658-245">`AdditionalFields`属性`[Remote]`属性可用于验证对服务器上的数据的字段的组合。</span><span class="sxs-lookup"><span data-stu-id="61658-245">The `AdditionalFields` property of the `[Remote]` attribute is useful for validating combinations of fields against data on the server.</span></span> <span data-ttu-id="61658-246">例如，如果`User`上面提供的模型有两个调用的其他属性`FirstName`和`LastName`，你可能想要验证是否没有现有的用户已有的名称配对。</span><span class="sxs-lookup"><span data-stu-id="61658-246">For example, if the `User` model from above had two additional properties called `FirstName` and `LastName`, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="61658-247">下面的代码中所示，你可以定义新属性：</span><span class="sxs-lookup"><span data-stu-id="61658-247">You define the new properties as shown in the following code:</span></span>

[!code-csharp[Main](validation/sample/User.cs?range=10-13)]

<span data-ttu-id="61658-248">`AdditionalFields`无法显式设置为字符串`"FirstName"`和`"LastName"`，但使用[ `nameof` ](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof)如下运算符简化了更高版本重构。</span><span class="sxs-lookup"><span data-stu-id="61658-248">`AdditionalFields` could have been set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) operator like this simplifies later refactoring.</span></span> <span data-ttu-id="61658-249">要执行验证的操作方法然后必须接受两个自变量，其中一个的值的`FirstName`，另一个的值用于`LastName`。</span><span class="sxs-lookup"><span data-stu-id="61658-249">The action method to perform the validation must then accept two arguments, one for the value of `FirstName` and one for the value of `LastName`.</span></span>


[!code-csharp[Main](validation/sample/UsersController.cs?range=30-39)]

<span data-ttu-id="61658-250">现在当用户输入的第一个和最后一个名称，JavaScript:</span><span class="sxs-lookup"><span data-stu-id="61658-250">Now when users enter a first and last name, JavaScript:</span></span>

* <span data-ttu-id="61658-251">进行远程调用以查看是否已采用了该对的名称。</span><span class="sxs-lookup"><span data-stu-id="61658-251">Makes a remote call to see if that pair of names has been taken.</span></span>
* <span data-ttu-id="61658-252">如果已对，将显示一条错误消息。</span><span class="sxs-lookup"><span data-stu-id="61658-252">If the pair has been taken, an error message is displayed.</span></span> 
* <span data-ttu-id="61658-253">如果不采取，用户可以提交窗体。</span><span class="sxs-lookup"><span data-stu-id="61658-253">If not taken, the user can submit the form.</span></span>

<span data-ttu-id="61658-254">如果你需要验证具有两个或多个其他字段`[Remote]`属性，您为他们提供以逗号分隔的列表。</span><span class="sxs-lookup"><span data-stu-id="61658-254">If you need to validate two or more additional fields with the `[Remote]` attribute, you provide them as a comma-delimited list.</span></span> <span data-ttu-id="61658-255">例如，若要添加`MiddleName`属性设置为模型中，设`[Remote]`特性，如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="61658-255">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following code:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="61658-256">`AdditionalFields`类似于将所有属性变量，必须是常量表达式。</span><span class="sxs-lookup"><span data-stu-id="61658-256">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="61658-257">因此，你必须使用[内插字符串](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interpolated-strings)或调用[ `string.Join()` ](https://msdn.microsoft.com/en-us/library/system.string.join(v=vs.110).aspx)初始化`AdditionalFields`。</span><span class="sxs-lookup"><span data-stu-id="61658-257">Therefore, you must not use an [interpolated string](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interpolated-strings) or call [`string.Join()`](https://msdn.microsoft.com/en-us/library/system.string.join(v=vs.110).aspx) to initialize `AdditionalFields`.</span></span> <span data-ttu-id="61658-258">你将添加到每个其他字段`[Remote]`属性，必须将另一个自变量添加到相应的控制器操作方法。</span><span class="sxs-lookup"><span data-stu-id="61658-258">For every additional field that you add to the `[Remote]` attribute, you must add another argument to the corresponding controller action method.</span></span>
