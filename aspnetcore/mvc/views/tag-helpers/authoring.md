---
title: "在 ASP.NET 核心中的创作标记帮助程序"
author: rick-anderson
description: "了解如何创作 ASP.NET Core 中的标记帮助程序。"
keywords: "ASP.NET 核心，标记帮助程序"
ms.author: riande
manager: wpickett
ms.date: 6/14/2017
ms.topic: article
ms.assetid: 4f16d978-5695-4abf-a785-fdaabf3bbcb9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 97013d06273c0993b74cdacfa16cb0d655c73667
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2017
---
# <a name="authoring-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a><span data-ttu-id="ac1da-104">在 ASP.NET 核心，使用示例演练中的创作标记帮助程序</span><span class="sxs-lookup"><span data-stu-id="ac1da-104">Authoring Tag Helpers in ASP.NET Core, a walkthrough with samples</span></span>

<span data-ttu-id="ac1da-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ac1da-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="ac1da-106">查看或下载示例代码</span><span class="sxs-lookup"><span data-stu-id="ac1da-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample)

## <a name="getting-started-with-tag-helpers"></a><span data-ttu-id="ac1da-107">开始使用标记帮助程序</span><span class="sxs-lookup"><span data-stu-id="ac1da-107">Getting started with Tag Helpers</span></span>

<span data-ttu-id="ac1da-108">本教程介绍编程标记帮助程序。</span><span class="sxs-lookup"><span data-stu-id="ac1da-108">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="ac1da-109">[标记帮助器简介](intro.md)介绍标记帮助程序提供的优势。</span><span class="sxs-lookup"><span data-stu-id="ac1da-109">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="ac1da-110">标记帮助器是实现任何类`ITagHelper`接口。</span><span class="sxs-lookup"><span data-stu-id="ac1da-110">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="ac1da-111">但是，在创作标记帮助器时，你通常派生自`TagHelper`，执行这样的可以访问`Process`方法。</span><span class="sxs-lookup"><span data-stu-id="ac1da-111">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="ac1da-112">创建一个名为的新 ASP.NET Core 项目**AuthoringTagHelpers**。</span><span class="sxs-lookup"><span data-stu-id="ac1da-112">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="ac1da-113">为此项目，你不需要身份验证。</span><span class="sxs-lookup"><span data-stu-id="ac1da-113">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="ac1da-114">创建一个文件夹来保存调用标记帮助程序*TagHelpers*。</span><span class="sxs-lookup"><span data-stu-id="ac1da-114">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="ac1da-115">*TagHelpers*文件夹是*不*必需的但它是合理的约定。</span><span class="sxs-lookup"><span data-stu-id="ac1da-115">The *TagHelpers* folder is *not* required, but it is a reasonable convention.</span></span> <span data-ttu-id="ac1da-116">现在让我们开始吧编写一些简单标记帮助程序。</span><span class="sxs-lookup"><span data-stu-id="ac1da-116">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="ac1da-117">最小的标记帮助器</span><span class="sxs-lookup"><span data-stu-id="ac1da-117">A minimal Tag Helper</span></span>

<span data-ttu-id="ac1da-118">在本部分中，你编写的标记帮助程序更新的电子邮件标记。</span><span class="sxs-lookup"><span data-stu-id="ac1da-118">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="ac1da-119">例如: </span><span class="sxs-lookup"><span data-stu-id="ac1da-119">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="ac1da-120">该服务器将使用我们的电子邮件标记帮助器以将该标记转换为以下：</span><span class="sxs-lookup"><span data-stu-id="ac1da-120">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
   ```

<span data-ttu-id="ac1da-121">即的定位点标记，使此电子邮件链接。</span><span class="sxs-lookup"><span data-stu-id="ac1da-121">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="ac1da-122">你可能想要执行此操作，如果你在编写博客引擎并且需要使用它来发送电子邮件发送的市场营销、 支持和其他联系人，所有内容都在同一个域。</span><span class="sxs-lookup"><span data-stu-id="ac1da-122">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1.  <span data-ttu-id="ac1da-123">添加以下`EmailTagHelper`类到*TagHelpers*文件夹。</span><span class="sxs-lookup"><span data-stu-id="ac1da-123">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="ac1da-124">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-124">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]</span></span>
    
    <span data-ttu-id="ac1da-125">**注意：**</span><span class="sxs-lookup"><span data-stu-id="ac1da-125">**Notes:**</span></span>
    
    * <span data-ttu-id="ac1da-126">标记帮助器使用面向根类名称的元素的命名约定 (减*TagHelper*类名称的一部分)。</span><span class="sxs-lookup"><span data-stu-id="ac1da-126">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="ac1da-127">在此示例中的根名称**电子邮件**TagHelper 是*电子邮件*，因此`<email>`标记将被加载目标。</span><span class="sxs-lookup"><span data-stu-id="ac1da-127">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="ac1da-128">此命名约定也适用于大多数标记帮助程序，稍后我将介绍如何重写它。</span><span class="sxs-lookup"><span data-stu-id="ac1da-128">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
    * <span data-ttu-id="ac1da-129">`EmailTagHelper` 类派生自 `TagHelper`。</span><span class="sxs-lookup"><span data-stu-id="ac1da-129">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="ac1da-130">`TagHelper`类提供用于编写标记帮助器方法和属性。</span><span class="sxs-lookup"><span data-stu-id="ac1da-130">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
    * <span data-ttu-id="ac1da-131">重写`Process`方法控制执行时的标记帮助程序的用途。</span><span class="sxs-lookup"><span data-stu-id="ac1da-131">The  overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="ac1da-132">`TagHelper`类还提供的异步版本 (`ProcessAsync`) 使用相同的参数。</span><span class="sxs-lookup"><span data-stu-id="ac1da-132">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
    * <span data-ttu-id="ac1da-133">上下文参数`Process`(和`ProcessAsync`) 包含与当前的 HTML 标记的执行关联信息。</span><span class="sxs-lookup"><span data-stu-id="ac1da-133">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
    * <span data-ttu-id="ac1da-134">输出参数`Process`(和`ProcessAsync`) 包含用于生成的 HTML 标记和内容的原始源具有代表性的有状态 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="ac1da-134">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
    * <span data-ttu-id="ac1da-135">我们类名称具有是由后缀为**TagHelper**，即*不*必需的但它被视为最佳做法约定。</span><span class="sxs-lookup"><span data-stu-id="ac1da-135">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="ac1da-136">你无法将类声明为：</span><span class="sxs-lookup"><span data-stu-id="ac1da-136">You could declare the class as:</span></span>
    
    ```csharp
    public class Email : TagHelper
    ```

2.  <span data-ttu-id="ac1da-137">若要使`EmailTagHelper`类可用于所有我们 Razor 视图中，添加`addTagHelper`指令至*Views/_ViewImports.cshtml*文件： [!code-html [Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-137">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
    <span data-ttu-id="ac1da-138">上面的代码中使用通配符语法来指定在我们的程序集中的所有标记帮助程序都将提供。</span><span class="sxs-lookup"><span data-stu-id="ac1da-138">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="ac1da-139">之后的第一个字符串`@addTagHelper`指定要加载的标记帮助程序 (使用"*"的所有标记帮助程序)，和第二个字符串"AuthoringTagHelpers"指定标记帮助程序是中的程序集。</span><span class="sxs-lookup"><span data-stu-id="ac1da-139">The first string after `@addTagHelper` specifies the tag helper to load (Use "*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="ac1da-140">另请注意第二行使中使用通配符的语法的 ASP.NET 核心 MVC 标记帮助器 (中讨论了这些帮助器[简介标记帮助程序](intro.md)。)它是`@addTagHelper`Razor 视图提供的标记帮助程序的指令。</span><span class="sxs-lookup"><span data-stu-id="ac1da-140">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="ac1da-141">或者，你可以提供完全限定的名称 (FQN) 的标记帮助程序，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ac1da-141">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
    <span data-ttu-id="ac1da-142">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-142">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]</span></span>
    
    <span data-ttu-id="ac1da-143">若要将标记帮助器添加到使用 FQN 的视图，你首先添加 FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)，，然后是程序集名称 (*AuthoringTagHelpers*)。</span><span class="sxs-lookup"><span data-stu-id="ac1da-143">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="ac1da-144">大多数开发人员会更喜欢使用通配符语法。</span><span class="sxs-lookup"><span data-stu-id="ac1da-144">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="ac1da-145">[标记帮助器简介](intro.md)进入标记帮助器添加、 删除、 层次结构中，和通配符语法中的详细信息。</span><span class="sxs-lookup"><span data-stu-id="ac1da-145">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3.  <span data-ttu-id="ac1da-146">更新中的标记*Views/Home/Contact.cshtml*使用这些更改的文件：</span><span class="sxs-lookup"><span data-stu-id="ac1da-146">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

    <span data-ttu-id="ac1da-147">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-147">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span></span>

4.  <span data-ttu-id="ac1da-148">运行应用并使用你最喜欢的浏览器来查看 HTML 源，以便验证电子邮件标记将替换定位点标记 (例如， `<a>Support</a>`)。</span><span class="sxs-lookup"><span data-stu-id="ac1da-148">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="ac1da-149">*支持*和*市场营销*呈现为链接，但它们不具有`href`属性以使其正常工作。</span><span class="sxs-lookup"><span data-stu-id="ac1da-149">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="ac1da-150">我们将在下一部分中修复此问题。</span><span class="sxs-lookup"><span data-stu-id="ac1da-150">We'll fix that in the next section.</span></span>

<span data-ttu-id="ac1da-151">注意： 如 HTML 标记和属性、 标记、 类名和 Razor，和 C# 中的属性不区分大小写。</span><span class="sxs-lookup"><span data-stu-id="ac1da-151">Note: Like HTML tags and attributes, tags, class names and attributes in Razor, and C# are not case-sensitive.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="ac1da-152">SetAttribute 和 SetContent</span><span class="sxs-lookup"><span data-stu-id="ac1da-152">SetAttribute and SetContent</span></span>

<span data-ttu-id="ac1da-153">在本部分中，我们将更新`EmailTagHelper`，以便它将创建电子邮件的有效定位点标记。</span><span class="sxs-lookup"><span data-stu-id="ac1da-153">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="ac1da-154">我们将更新，使其带有 Razor 视图中的信息 (形式`mail-to`特性) 并使用它在生成定位点。</span><span class="sxs-lookup"><span data-stu-id="ac1da-154">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="ac1da-155">更新`EmailTagHelper`替换为以下类：</span><span class="sxs-lookup"><span data-stu-id="ac1da-155">Update the `EmailTagHelper` class with the following:</span></span>

<span data-ttu-id="ac1da-156">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-156">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]</span></span>

<span data-ttu-id="ac1da-157">**注意：**</span><span class="sxs-lookup"><span data-stu-id="ac1da-157">**Notes:**</span></span>

* <span data-ttu-id="ac1da-158">有关标记帮助程序 Pascal 大小写形式类和属性的名称转换为其[降低 kebab 用例](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)。</span><span class="sxs-lookup"><span data-stu-id="ac1da-158">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="ac1da-159">因此，若要使用`MailTo`属性中，你将使用`<email mail-to="value"/>`等效。</span><span class="sxs-lookup"><span data-stu-id="ac1da-159">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="ac1da-160">最后一行将已完成的内容设置我们按最小方式功能标记帮助器。</span><span class="sxs-lookup"><span data-stu-id="ac1da-160">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="ac1da-161">突出显示的行将显示用于将属性添加的语法：</span><span class="sxs-lookup"><span data-stu-id="ac1da-161">The highlighted line shows the syntax for adding attributes:</span></span>

<span data-ttu-id="ac1da-162">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-162">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]</span></span>

<span data-ttu-id="ac1da-163">只要当前不存在属性集合中，该方法将适用于"href"的属性。</span><span class="sxs-lookup"><span data-stu-id="ac1da-163">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="ac1da-164">你还可以使用`output.Attributes.Add`方法将标记帮助器属性添加到标记特性的集合的末尾。</span><span class="sxs-lookup"><span data-stu-id="ac1da-164">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1.  <span data-ttu-id="ac1da-165">更新中的标记*Views/Home/Contact.cshtml*使用这些更改的文件： [!code-html [Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-165">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2.  <span data-ttu-id="ac1da-166">运行应用程序并验证它会生成正确的链接。</span><span class="sxs-lookup"><span data-stu-id="ac1da-166">Run the app and verify that it generates the correct links.</span></span>
    
    > [!NOTE]
    ><span data-ttu-id="ac1da-167">如果你打算编写电子邮件标记自关闭 (`<email mail-to="Rick" />`)，则最终输出还为自结束。</span><span class="sxs-lookup"><span data-stu-id="ac1da-167">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="ac1da-168">若要启用用于编写将标记与开始标记的功能 (`<email mail-to="Rick">`) 必须修饰替换为以下类：</span><span class="sxs-lookup"><span data-stu-id="ac1da-168">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
    >
    > <span data-ttu-id="ac1da-169">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-169">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]</span></span>
    
    <span data-ttu-id="ac1da-170">具有自结束电子邮件标记帮助程序，则输出为`<a href="mailto:Rick@contoso.com" />`。</span><span class="sxs-lookup"><span data-stu-id="ac1da-170">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="ac1da-171">自结束的定位点标记不是有效的 HTML，因此你不希望创建一个，但你可能想要创建的标记帮助程序为自结束。</span><span class="sxs-lookup"><span data-stu-id="ac1da-171">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that is self-closing.</span></span> <span data-ttu-id="ac1da-172">标记帮助程序设置的一种`TagMode`在读取标记后的属性。</span><span class="sxs-lookup"><span data-stu-id="ac1da-172">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="ac1da-173">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="ac1da-173">ProcessAsync</span></span>

<span data-ttu-id="ac1da-174">在本部分中，我们将编写的异步电子邮件帮助器。</span><span class="sxs-lookup"><span data-stu-id="ac1da-174">In this section, we'll write an asynchronous email helper.</span></span>

1.  <span data-ttu-id="ac1da-175">替换`EmailTagHelper`类替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="ac1da-175">Replace the `EmailTagHelper` class with the following code:</span></span>

    <span data-ttu-id="ac1da-176">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-176">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]</span></span>

    <span data-ttu-id="ac1da-177">**注意：**</span><span class="sxs-lookup"><span data-stu-id="ac1da-177">**Notes:**</span></span>

    * <span data-ttu-id="ac1da-178">此版本使用异步`ProcessAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="ac1da-178">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="ac1da-179">异步`GetChildContentAsync`返回`Task`包含`TagHelperContent`。</span><span class="sxs-lookup"><span data-stu-id="ac1da-179">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

    * <span data-ttu-id="ac1da-180">使用`output`参数，以获取内容的 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="ac1da-180">Use the `output` parameter to get contents of the HTML element.</span></span>

2.  <span data-ttu-id="ac1da-181">进行以下更改到*Views/Home/Contact.cshtml*文件以便标记帮助程序可以获取目标电子邮件。</span><span class="sxs-lookup"><span data-stu-id="ac1da-181">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

    <span data-ttu-id="ac1da-182">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-182">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span></span>

3.  <span data-ttu-id="ac1da-183">运行应用程序并验证它会生成有效的电子邮件链接。</span><span class="sxs-lookup"><span data-stu-id="ac1da-183">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="ac1da-184">RemoveAll、 PreContent.SetHtmlContent 和 PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="ac1da-184">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1.  <span data-ttu-id="ac1da-185">添加以下`BoldTagHelper`类到*TagHelpers*文件夹。</span><span class="sxs-lookup"><span data-stu-id="ac1da-185">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="ac1da-186">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-186">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]</span></span>

    <span data-ttu-id="ac1da-187">**注意：**</span><span class="sxs-lookup"><span data-stu-id="ac1da-187">**Notes:**</span></span>
    
    * <span data-ttu-id="ac1da-188">`[HtmlTargetElement]`属性指定任何包含 HTML 特性的 HTML 元素名为"bold"的属性参数将匹配，传递和`Process`类中的重写方法将运行。</span><span class="sxs-lookup"><span data-stu-id="ac1da-188">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="ac1da-189">在我们示例中，`Process`方法中删除"粗体"的属性，并在包含标记与`<strong></strong>`。</span><span class="sxs-lookup"><span data-stu-id="ac1da-189">In our sample, the `Process`  method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
    * <span data-ttu-id="ac1da-190">由于你不想替换内容的现有标记，你必须编写打开`<strong>`使用标记`PreContent.SetHtmlContent`方法和结束`</strong>`使用标记`PostContent.SetHtmlContent`方法。</span><span class="sxs-lookup"><span data-stu-id="ac1da-190">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2.  <span data-ttu-id="ac1da-191">修改*About.cshtml*视图，以包含`bold`属性值。</span><span class="sxs-lookup"><span data-stu-id="ac1da-191">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="ac1da-192">已完成的代码所示。</span><span class="sxs-lookup"><span data-stu-id="ac1da-192">The completed code is shown below.</span></span>

    <span data-ttu-id="ac1da-193">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-193">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]</span></span>

3.  <span data-ttu-id="ac1da-194">运行该应用。</span><span class="sxs-lookup"><span data-stu-id="ac1da-194">Run the app.</span></span> <span data-ttu-id="ac1da-195">你最喜欢的浏览器可用于检查源和验证标记。</span><span class="sxs-lookup"><span data-stu-id="ac1da-195">You can use your favorite browser to inspect the source and verify the markup.</span></span>

    <span data-ttu-id="ac1da-196">`[HtmlTargetElement]`上面属性只针对提供的"加粗"的属性名称的 HTML 标记。</span><span class="sxs-lookup"><span data-stu-id="ac1da-196">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="ac1da-197">`<bold>`元素均未修改的标记帮助程序。</span><span class="sxs-lookup"><span data-stu-id="ac1da-197">The `<bold>` element was not modified by the tag helper.</span></span>

4. <span data-ttu-id="ac1da-198">注释掉`[HtmlTargetElement]`属性行和它将默认为目标设定`<bold>`标记，也就是说，HTML 标记的窗体`<bold>`。</span><span class="sxs-lookup"><span data-stu-id="ac1da-198">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="ac1da-199">请记住，默认命名约定将匹配的类名称**加粗**到 TagHelper`<bold>`标记。</span><span class="sxs-lookup"><span data-stu-id="ac1da-199">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="ac1da-200">运行应用并确认`<bold>`标记处理的标记帮助程序。</span><span class="sxs-lookup"><span data-stu-id="ac1da-200">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="ac1da-201">修饰的类具有多个`[HtmlTargetElement]`特性导致的目标的逻辑 OR。</span><span class="sxs-lookup"><span data-stu-id="ac1da-201">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="ac1da-202">例如，使用下面的代码，以粗体显示标记或以粗体显示的属性将匹配。</span><span class="sxs-lookup"><span data-stu-id="ac1da-202">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

<span data-ttu-id="ac1da-203">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-203">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]</span></span>

<span data-ttu-id="ac1da-204">当多个属性添加到同一个语句时，运行时将它们视为逻辑 and。</span><span class="sxs-lookup"><span data-stu-id="ac1da-204">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="ac1da-205">例如，在下面的代码中，HTML 元素必须命名为"bold"具有一个名为的属性"bold"(`<bold bold />`) 以匹配。</span><span class="sxs-lookup"><span data-stu-id="ac1da-205">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="ac1da-206">你还可以使用`[HtmlTargetElement]`若要更改目标元素的名称。</span><span class="sxs-lookup"><span data-stu-id="ac1da-206">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="ac1da-207">例如，如果你想`BoldTagHelper`到目标`<MyBold>`标记，则将使用以下属性：</span><span class="sxs-lookup"><span data-stu-id="ac1da-207">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="passing-a-model-to-a-tag-helper"></a><span data-ttu-id="ac1da-208">将模型传递到标记帮助器</span><span class="sxs-lookup"><span data-stu-id="ac1da-208">Passing a model to a Tag Helper</span></span>

1.  <span data-ttu-id="ac1da-209">添加*模型*文件夹。</span><span class="sxs-lookup"><span data-stu-id="ac1da-209">Add a *Models* folder.</span></span>

2.  <span data-ttu-id="ac1da-210">添加以下`WebsiteContext`类到*模型*文件夹：</span><span class="sxs-lookup"><span data-stu-id="ac1da-210">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

    <span data-ttu-id="ac1da-211">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-211">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]</span></span>

3.  <span data-ttu-id="ac1da-212">添加以下`WebsiteInformationTagHelper`类到*TagHelpers*文件夹。</span><span class="sxs-lookup"><span data-stu-id="ac1da-212">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="ac1da-213">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-213">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]</span></span>
    
    <span data-ttu-id="ac1da-214">**注意：**</span><span class="sxs-lookup"><span data-stu-id="ac1da-214">**Notes:**</span></span>
    
    * <span data-ttu-id="ac1da-215">如前所述，标记帮助程序会将转换 Pascal 大小写形式 C# 类名称和属性的标记帮助程序到[降低 kebab 用例](http://wiki.c2.com/?KebabCase)。</span><span class="sxs-lookup"><span data-stu-id="ac1da-215">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="ac1da-216">因此，若要使用`WebsiteInformationTagHelper`在 Razor，你将编写`<website-information />`。</span><span class="sxs-lookup"><span data-stu-id="ac1da-216">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
    * <span data-ttu-id="ac1da-217">显式未识别具有的目标元素`[HtmlTargetElement]`特性，因此默认的`website-information`将加载目标。</span><span class="sxs-lookup"><span data-stu-id="ac1da-217">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="ac1da-218">如果您应用以下属性 （请注意它不 kebab 这种情况，但与类名称匹配）：</span><span class="sxs-lookup"><span data-stu-id="ac1da-218">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    <span data-ttu-id="ac1da-219">较低的 kebab 用例标记`<website-information />`将不匹配。</span><span class="sxs-lookup"><span data-stu-id="ac1da-219">The lower kebab case tag `<website-information />` would not match.</span></span> <span data-ttu-id="ac1da-220">如果你想要`[HtmlTargetElement]`属性中，你将使用 kebab 用例，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ac1da-220">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * <span data-ttu-id="ac1da-221">自结束的元素没有任何内容。</span><span class="sxs-lookup"><span data-stu-id="ac1da-221">Elements that are self-closing have no content.</span></span> <span data-ttu-id="ac1da-222">对于此示例中，Razor 标记将使用自结束标记，但将创建的标记帮助程序[部分](http://www.w3.org/TR/html5/sections.html#the-section-element)元素 (它不是自结束，并且你正在编写中的内容`section`元素)。</span><span class="sxs-lookup"><span data-stu-id="ac1da-222">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which is not self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="ac1da-223">因此，你需要设置`TagMode`到`StartTagAndEndTag`写入输出。</span><span class="sxs-lookup"><span data-stu-id="ac1da-223">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="ac1da-224">或者，你可以注释行设置`TagMode`和写入与结束标记的标记。</span><span class="sxs-lookup"><span data-stu-id="ac1da-224">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="ac1da-225">（示例标记提供更高版本在本教程中）。</span><span class="sxs-lookup"><span data-stu-id="ac1da-225">(Example markup is provided later in this tutorial.)</span></span>
    
    * <span data-ttu-id="ac1da-226">`$` （美元符号） 在下面的一行使用[内插字符串](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span><span class="sxs-lookup"><span data-stu-id="ac1da-226">The `$` (dollar sign) in the following line uses an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  <span data-ttu-id="ac1da-227">添加到以下标记*About.cshtml*视图。</span><span class="sxs-lookup"><span data-stu-id="ac1da-227">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="ac1da-228">突出显示的标记显示 web 站点的信息。</span><span class="sxs-lookup"><span data-stu-id="ac1da-228">The highlighted markup displays the web site information.</span></span>
    
    <span data-ttu-id="ac1da-229">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-229">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]</span></span>
    
    >[!NOTE]
    > <span data-ttu-id="ac1da-230">在 Razor 标记如下所示：</span><span class="sxs-lookup"><span data-stu-id="ac1da-230">In the Razor markup shown below:</span></span>
    >
    ><span data-ttu-id="ac1da-231">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-231">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]</span></span>
    > 
    ><span data-ttu-id="ac1da-232">Razor 知道`info`属性是一个类，不是字符串，并且你想要编写 C# 代码。</span><span class="sxs-lookup"><span data-stu-id="ac1da-232">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="ac1da-233">应而无需编写任何非字符串标记帮助器属性`@`字符。</span><span class="sxs-lookup"><span data-stu-id="ac1da-233">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5.  <span data-ttu-id="ac1da-234">运行应用，并导航到关于视图以查看 web 站点的信息。</span><span class="sxs-lookup"><span data-stu-id="ac1da-234">Run the app, and navigate to the About view to see the web site information.</span></span>

    >[!NOTE]
    ><span data-ttu-id="ac1da-235">你可以使用以下标记与结束标记，并删除在行的`TagMode.StartTagAndEndTag`标记帮助程序中：</span><span class="sxs-lookup"><span data-stu-id="ac1da-235">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
    >
    ><span data-ttu-id="ac1da-236">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-236">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]</span></span>

## <a name="condition-tag-helper"></a><span data-ttu-id="ac1da-237">条件标记帮助器</span><span class="sxs-lookup"><span data-stu-id="ac1da-237">Condition Tag Helper</span></span>

<span data-ttu-id="ac1da-238">条件标记帮助器呈现输出时传递 true 值。</span><span class="sxs-lookup"><span data-stu-id="ac1da-238">The condition tag helper renders output when passed a true value.</span></span>

1.  <span data-ttu-id="ac1da-239">添加以下`ConditionTagHelper`类到*TagHelpers*文件夹。</span><span class="sxs-lookup"><span data-stu-id="ac1da-239">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="ac1da-240">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-240">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]</span></span>

2.  <span data-ttu-id="ac1da-241">内容替换*Views/Home/Index.cshtml*文件替换为以下标记：</span><span class="sxs-lookup"><span data-stu-id="ac1da-241">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

    <!-- literal_block {"xml:space": "preserve", "source": "mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml", "ids": [], "linenos": false, "highlight_args": {"linenostart": 1}} -->
    
    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=Model />
        <div condition="Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  <span data-ttu-id="ac1da-242">替换`Index`中的方法`Home`控制器替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="ac1da-242">Replace the `Index` method in the `Home` controller with the following code:</span></span>

    <span data-ttu-id="ac1da-243">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-243">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]</span></span>

4.  <span data-ttu-id="ac1da-244">运行应用程序，并浏览到主页。</span><span class="sxs-lookup"><span data-stu-id="ac1da-244">Run the app and browse to the home page.</span></span> <span data-ttu-id="ac1da-245">在条件中的标记`div`将不会呈现。</span><span class="sxs-lookup"><span data-stu-id="ac1da-245">The markup in the conditional `div` will not be rendered.</span></span> <span data-ttu-id="ac1da-246">将查询的字符串追加`?approved=true`到的 URL (例如， `http://localhost:1235/Home/Index?approved=true`)。</span><span class="sxs-lookup"><span data-stu-id="ac1da-246">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="ac1da-247">`approved`设置为 true，条件将显示标记。</span><span class="sxs-lookup"><span data-stu-id="ac1da-247">`approved` is set to true and the conditional markup will be displayed.</span></span>

>[!NOTE]
><span data-ttu-id="ac1da-248">使用[nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof)运算符来指定为目标，而不是指定的字符串，就像处理以粗体显示标记帮助器属性：</span><span class="sxs-lookup"><span data-stu-id="ac1da-248">Use the [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
><span data-ttu-id="ac1da-249">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-249">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]</span></span>
>
><span data-ttu-id="ac1da-250">[Nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof)运算符将保护该代码应它曾重构 (我们可能想要将名称更改为`RedCondition`)。</span><span class="sxs-lookup"><span data-stu-id="ac1da-250">The [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoiding-tag-helper-conflicts"></a><span data-ttu-id="ac1da-251">避免标记帮助器冲突</span><span class="sxs-lookup"><span data-stu-id="ac1da-251">Avoiding Tag Helper conflicts</span></span>

<span data-ttu-id="ac1da-252">在本部分中，你编写的成对的自动链接标记帮助程序。</span><span class="sxs-lookup"><span data-stu-id="ac1da-252">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="ac1da-253">第一个将包含到 HTML 定位点标记包含相同的 URL （这将会产生一个链接到的 URL） 从 HTTP URL 的标记。</span><span class="sxs-lookup"><span data-stu-id="ac1da-253">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="ac1da-254">第二个将执行相同操作的 URL 开头 WWW。</span><span class="sxs-lookup"><span data-stu-id="ac1da-254">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="ac1da-255">由于这些两个帮助器密切相关，并且你可能在将来重构，我们将使它们保持在相同的文件。</span><span class="sxs-lookup"><span data-stu-id="ac1da-255">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1.  <span data-ttu-id="ac1da-256">添加以下`AutoLinkerHttpTagHelper`类到*TagHelpers*文件夹。</span><span class="sxs-lookup"><span data-stu-id="ac1da-256">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="ac1da-257">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-257">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]</span></span>

    >[!NOTE]
    ><span data-ttu-id="ac1da-258">`AutoLinkerHttpTagHelper`类目标`p`元素，并使用[正则表达式](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)创建定位点。</span><span class="sxs-lookup"><span data-stu-id="ac1da-258">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

2.  <span data-ttu-id="ac1da-259">将以下标记添加到末尾*Views/Home/Contact.cshtml*文件：</span><span class="sxs-lookup"><span data-stu-id="ac1da-259">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

    <span data-ttu-id="ac1da-260">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-260">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]</span></span>

3.  <span data-ttu-id="ac1da-261">运行应用程序并验证标记帮助程序正常呈现定位点。</span><span class="sxs-lookup"><span data-stu-id="ac1da-261">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4.  <span data-ttu-id="ac1da-262">更新`AutoLinker`类，以包含`AutoLinkerWwwTagHelper`这会将 www 文本转换为还包含原始 www 文本的定位点标记。</span><span class="sxs-lookup"><span data-stu-id="ac1da-262">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="ac1da-263">在下面突出显示更新的代码：</span><span class="sxs-lookup"><span data-stu-id="ac1da-263">The updated code is highlighted below:</span></span>

    <span data-ttu-id="ac1da-264">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-264">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]</span></span>

5.  <span data-ttu-id="ac1da-265">运行该应用。</span><span class="sxs-lookup"><span data-stu-id="ac1da-265">Run the app.</span></span> <span data-ttu-id="ac1da-266">请注意 www 文本呈现为链接，但不是 HTTP 文本。</span><span class="sxs-lookup"><span data-stu-id="ac1da-266">Notice the www text is rendered as a link but the HTTP text is not.</span></span> <span data-ttu-id="ac1da-267">如果将中断点放在这两个类中，你可以看到首先运行 HTTP 标记帮助器类。</span><span class="sxs-lookup"><span data-stu-id="ac1da-267">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="ac1da-268">问题是，标记帮助器输出缓存的并且当 WWW 标记帮助程序运行时，它将覆盖从 HTTP 标记帮助程序缓存的输出。</span><span class="sxs-lookup"><span data-stu-id="ac1da-268">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="ac1da-269">在教程后面部分中，我们将了解如何控制标记帮助程序中运行的顺序。</span><span class="sxs-lookup"><span data-stu-id="ac1da-269">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="ac1da-270">我们将替换为以下修复代码：</span><span class="sxs-lookup"><span data-stu-id="ac1da-270">We'll fix the code with the following:</span></span>

    <span data-ttu-id="ac1da-271">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-271">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]</span></span>

    >[!NOTE]
    ><span data-ttu-id="ac1da-272">中的自动链接标记帮助器第一版，你的内容替换为以下代码目标：</span><span class="sxs-lookup"><span data-stu-id="ac1da-272">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
    >
    ><span data-ttu-id="ac1da-273">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-273">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]</span></span>
    >
    ><span data-ttu-id="ac1da-274">也就是说，调用`GetChildContentAsync`使用`TagHelperOutput`传入`ProcessAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="ac1da-274">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="ac1da-275">如提到的那样以前，因为缓存输出，最后一个标记帮助器以运行 wins。</span><span class="sxs-lookup"><span data-stu-id="ac1da-275">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="ac1da-276">修复该问题替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="ac1da-276">You fixed that problem with the following code:</span></span>
    >
    ><span data-ttu-id="ac1da-277">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-277">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]</span></span>
    >
    ><span data-ttu-id="ac1da-278">上面的代码检查已修改内容，但如果它具有，它获取该内容从输出缓冲区。</span><span class="sxs-lookup"><span data-stu-id="ac1da-278">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6.  <span data-ttu-id="ac1da-279">运行应用程序并验证两个链接按预期方式工作。</span><span class="sxs-lookup"><span data-stu-id="ac1da-279">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="ac1da-280">虽然它可能显示我们自动链接器标记帮助程序是正确和完整，但其细微问题。</span><span class="sxs-lookup"><span data-stu-id="ac1da-280">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="ac1da-281">如果首先运行 WWW 标记帮助器，www 链接不会正确。</span><span class="sxs-lookup"><span data-stu-id="ac1da-281">If the WWW tag helper runs first, the www links will not be correct.</span></span> <span data-ttu-id="ac1da-282">请更新代码，通过添加`Order`重载以控制标记运行中的顺序。</span><span class="sxs-lookup"><span data-stu-id="ac1da-282">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="ac1da-283">`Order`属性确定相对于其他面向同一元素的标记帮助程序的执行顺序。</span><span class="sxs-lookup"><span data-stu-id="ac1da-283">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="ac1da-284">默认顺序值为 0，第一次执行具有较低的值的实例。</span><span class="sxs-lookup"><span data-stu-id="ac1da-284">The default order value is zero and instances with lower values are executed first.</span></span>

    <span data-ttu-id="ac1da-285">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-285">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]</span></span>
    
    <span data-ttu-id="ac1da-286">上面的代码可以保证 HTTP 标记帮助程序运行之前 WWW 标记帮助器。</span><span class="sxs-lookup"><span data-stu-id="ac1da-286">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="ac1da-287">更改`Order`到`MaxValue`并验证是否为 WWW 标记生成的标记不正确。</span><span class="sxs-lookup"><span data-stu-id="ac1da-287">Change `Order` to `MaxValue` and verify that the markup generated for the  WWW tag is incorrect.</span></span>

## <a name="inspecting-and-retrieving-child-content"></a><span data-ttu-id="ac1da-288">检查和检索子内容</span><span class="sxs-lookup"><span data-stu-id="ac1da-288">Inspecting and retrieving child content</span></span>

<span data-ttu-id="ac1da-289">标记帮助程序提供多个属性以检索内容。</span><span class="sxs-lookup"><span data-stu-id="ac1da-289">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="ac1da-290">结果`GetChildContentAsync`可以追加到`output.Content`。</span><span class="sxs-lookup"><span data-stu-id="ac1da-290">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="ac1da-291">你可以检查的结果`GetChildContentAsync`与`GetContent`。</span><span class="sxs-lookup"><span data-stu-id="ac1da-291">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="ac1da-292">如果你修改`output.Content`，不会执行或呈现除非调用 TagHelper 正文`GetChildContentAsync`如我们自动链接器示例所示：</span><span class="sxs-lookup"><span data-stu-id="ac1da-292">If you modify `output.Content`, the TagHelper body will not be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

<span data-ttu-id="ac1da-293">[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]</span><span class="sxs-lookup"><span data-stu-id="ac1da-293">[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]</span></span>

-  <span data-ttu-id="ac1da-294">多次调用`GetChildContentAsync`将返回相同的值而不会重新执行`TagHelper`正文除非你传入一个 false 的参数，指示不使用缓存的结果。</span><span class="sxs-lookup"><span data-stu-id="ac1da-294">Multiple calls to `GetChildContentAsync` will return the same value and will not re-execute the `TagHelper` body unless you pass in a false parameter indicating  not use the cached result.</span></span>
