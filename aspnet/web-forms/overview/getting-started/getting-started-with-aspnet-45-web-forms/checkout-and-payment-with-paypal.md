---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: "签出和付款与 PayPal |Microsoft 文档"
author: Erikre
description: "本系列教程将教您生成有关我们使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 的 ASP.NET Web 窗体应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: dd975850a3ed3e7b1746d5123572065675a88656
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="checkout-and-payment-with-paypal"></a><span data-ttu-id="4fa08-103">签出和与 PayPal 付款</span><span class="sxs-lookup"><span data-stu-id="4fa08-103">Checkout and Payment with PayPal</span></span>
====================
<span data-ttu-id="4fa08-104">通过[艾力克 Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="4fa08-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="4fa08-105">[下载 Wingtip Toys 示例项目 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下载电子书 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="4fa08-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="4fa08-106">本系列教程将教您构建使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web 窗体应用程序的基础知识。</span><span class="sxs-lookup"><span data-stu-id="4fa08-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="4fa08-107">Visual Studio 2013[包含 C# 源代码项目](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)是可以附带本系列教程。</span><span class="sxs-lookup"><span data-stu-id="4fa08-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="4fa08-108">本教程介绍如何修改 Wingtip Toys 示例应用程序，以包括用户身份验证、 注册和使用 PayPal 的付款。</span><span class="sxs-lookup"><span data-stu-id="4fa08-108">This tutorial describes how to modify the Wingtip Toys sample application to include user authorization, registration, and payment using PayPal.</span></span> <span data-ttu-id="4fa08-109">只有登录的用户将有授权，以购买产品。</span><span class="sxs-lookup"><span data-stu-id="4fa08-109">Only users who are logged in will have authorization to purchase products.</span></span> <span data-ttu-id="4fa08-110">ASP.NET 4.5 Web 窗体项目模板的内置用户注册功能已包含大部分所需内容。</span><span class="sxs-lookup"><span data-stu-id="4fa08-110">The ASP.NET 4.5 Web Forms project template's built-in user registration functionality already includes much of what you need.</span></span> <span data-ttu-id="4fa08-111">你将添加 PayPal Express 签出功能。</span><span class="sxs-lookup"><span data-stu-id="4fa08-111">You will add PayPal Express Checkout functionality.</span></span> <span data-ttu-id="4fa08-112">在本教程，你使用测试环境，以便将不传输任何实际的资金 PayPal 开发人员。</span><span class="sxs-lookup"><span data-stu-id="4fa08-112">In this tutorial you be using the PayPal developer testing environment, so no actual funds will be transferred.</span></span> <span data-ttu-id="4fa08-113">在本教程结束时，将通过选择要添加到购物车，单击签出按钮，然后将数据传输到 PayPal 测试网站产品来测试应用程序。</span><span class="sxs-lookup"><span data-stu-id="4fa08-113">At the end of the tutorial, you will test the application by selecting products to add to the shopping cart, clicking the checkout button, and transferring data to the PayPal testing web site.</span></span> <span data-ttu-id="4fa08-114">在 PayPal 测试网站上，你将确认传送和付款信息，然后返回到本地 Wingtip Toys 示例应用程序以确认和完成购买。</span><span class="sxs-lookup"><span data-stu-id="4fa08-114">On the PayPal testing web site, you will confirm your shipping and payment information and then return to the local Wingtip Toys sample application to confirm and complete the purchase.</span></span>

<span data-ttu-id="4fa08-115">在线购物，该地址可扩展性和安全性中有几个有经验的第三方支付处理器专用化。</span><span class="sxs-lookup"><span data-stu-id="4fa08-115">There are several experienced third-party payment processors that specialize in online shopping that address scalability and security.</span></span> <span data-ttu-id="4fa08-116">ASP.NET 开发人员应考虑利用在实现购物和购买解决方案之前的第三方支付解决方案的优点。</span><span class="sxs-lookup"><span data-stu-id="4fa08-116">ASP.NET developers should consider the advantages of utilizing a third party payment solution before implementing a shopping and purchasing solution.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="4fa08-117">Wingtip Toys 示例应用程序设计为向 ASP.NET web 开发人员显示特定 ASP.NET 概念和功能可用。</span><span class="sxs-lookup"><span data-stu-id="4fa08-117">The Wingtip Toys sample application was designed to shown specific ASP.NET concepts and features available to ASP.NET web developers.</span></span> <span data-ttu-id="4fa08-118">此示例应用程序不进行了优化的可伸缩性和安全性方面的所有可能的情况。</span><span class="sxs-lookup"><span data-stu-id="4fa08-118">This sample application was not optimized for all possible circumstances in regard to scalability and security.</span></span>


## <a name="what-youll-learn"></a><span data-ttu-id="4fa08-119">你将学习：</span><span class="sxs-lookup"><span data-stu-id="4fa08-119">What you'll learn:</span></span>

- <span data-ttu-id="4fa08-120">如何限制对文件夹中的特定页的访问。</span><span class="sxs-lookup"><span data-stu-id="4fa08-120">How to restrict access to specific pages in a folder.</span></span>
- <span data-ttu-id="4fa08-121">如何从匿名的购物车创建已知的购物车。</span><span class="sxs-lookup"><span data-stu-id="4fa08-121">How to create a known shopping cart from an anonymous shopping cart.</span></span>
- <span data-ttu-id="4fa08-122">如何为项目启用 SSL。</span><span class="sxs-lookup"><span data-stu-id="4fa08-122">How to enable SSL for the project.</span></span>
- <span data-ttu-id="4fa08-123">如何向项目添加 OAuth 提供程序。</span><span class="sxs-lookup"><span data-stu-id="4fa08-123">How to add an OAuth provider to the project.</span></span>
- <span data-ttu-id="4fa08-124">如何使用 PayPal 购买产品使用 PayPal 测试环境。</span><span class="sxs-lookup"><span data-stu-id="4fa08-124">How to use PayPal to purchase products using the PayPal testing environment.</span></span>
- <span data-ttu-id="4fa08-125">如何显示从 PayPal 中的详细信息**说明**控件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-125">How to display details from PayPal in a **DetailsView** control.</span></span>
- <span data-ttu-id="4fa08-126">如何使用从 PayPal 获取详细信息更新 Wingtip Toys 应用程序的数据库。</span><span class="sxs-lookup"><span data-stu-id="4fa08-126">How to update the database of the Wingtip Toys application with details obtained from PayPal.</span></span>

## <a name="adding-order-tracking"></a><span data-ttu-id="4fa08-127">添加顺序跟踪</span><span class="sxs-lookup"><span data-stu-id="4fa08-127">Adding Order Tracking</span></span>

<span data-ttu-id="4fa08-128">在本教程中，你将创建两个新类来从用户已创建的顺序跟踪数据。</span><span class="sxs-lookup"><span data-stu-id="4fa08-128">In this tutorial, you'll create two new classes to track data from the order a user has created.</span></span> <span data-ttu-id="4fa08-129">类将跟踪有关传送信息、 购买总计和付款确认数据。</span><span class="sxs-lookup"><span data-stu-id="4fa08-129">The classes will track data regarding shipping information, purchase total, and payment confirmation.</span></span>

### <a name="add-the-order-and-orderdetail-model-classes"></a><span data-ttu-id="4fa08-130">添加的顺序和 OrderDetail 模型类</span><span class="sxs-lookup"><span data-stu-id="4fa08-130">Add the Order and OrderDetail Model Classes</span></span>

<span data-ttu-id="4fa08-131">本系列教程中前面定义的类别，产品，架构以及通过创建项目，购物车`Category`， `Product`，和`CartItem`中的类*模型*文件夹。</span><span class="sxs-lookup"><span data-stu-id="4fa08-131">Earlier in this tutorial series, you defined the schema for categories, products, and shopping cart items by creating the `Category`, `Product`, and `CartItem` classes in the *Models* folder.</span></span> <span data-ttu-id="4fa08-132">现在，你将添加两个新类，以定义为产品订单和订单的详细信息的架构。</span><span class="sxs-lookup"><span data-stu-id="4fa08-132">Now you will add two new classes to define the schema for the product order and the details of the order.</span></span>

1. <span data-ttu-id="4fa08-133">在**模型**文件夹中，添加一个名为的新类*Order.cs*。</span><span class="sxs-lookup"><span data-stu-id="4fa08-133">In the **Models** folder, add a new class named *Order.cs*.</span></span>   
 <span data-ttu-id="4fa08-134">编辑器中将显示新的类文件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-134">The new class file is displayed in the editor.</span></span>
2. <span data-ttu-id="4fa08-135">默认代码替换为以下：</span><span class="sxs-lookup"><span data-stu-id="4fa08-135">Replace the default code with the following:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. <span data-ttu-id="4fa08-136">添加*OrderDetail.cs*类到*模型*文件夹。</span><span class="sxs-lookup"><span data-stu-id="4fa08-136">Add an *OrderDetail.cs* class to the *Models* folder.</span></span>
4. <span data-ttu-id="4fa08-137">默认代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="4fa08-137">Replace the default code with the following code:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

<span data-ttu-id="4fa08-138">`Order`和`OrderDetail`类包含此架构定义用于购买和传送的顺序信息。</span><span class="sxs-lookup"><span data-stu-id="4fa08-138">The `Order` and `OrderDetail` classes contain the schema to define the order information used for purchasing and shipping.</span></span>

<span data-ttu-id="4fa08-139">此外，你将需要更新数据库上下文类，管理的实体类并提供对数据库的数据访问。</span><span class="sxs-lookup"><span data-stu-id="4fa08-139">In addition, you will need to update the database context class that manages the entity classes and that provides data access to the database.</span></span> <span data-ttu-id="4fa08-140">若要执行此操作，你将添加新创建的顺序和`OrderDetail`模型的类来`ProductContext`类。</span><span class="sxs-lookup"><span data-stu-id="4fa08-140">To do this, you will add the newly created Order and `OrderDetail` model classes to `ProductContext` class.</span></span>

1. <span data-ttu-id="4fa08-141">在**解决方案资源管理器**，找到并打开*ProductContext.cs*文件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-141">In **Solution Explorer**, find and open the *ProductContext.cs* file.</span></span>
2. <span data-ttu-id="4fa08-142">添加到突出显示的代码*ProductContext.cs*文件如下所示：</span><span class="sxs-lookup"><span data-stu-id="4fa08-142">Add the highlighted code to the *ProductContext.cs* file as shown below:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

<span data-ttu-id="4fa08-143">如以前在此系列教程的中的代码中所述*ProductContext.cs*文件将添加`System.Data.Entity`命名空间，以便您具有对实体框架的所有核心功能的访问。</span><span class="sxs-lookup"><span data-stu-id="4fa08-143">As mentioned previously in this tutorial series, the code in the *ProductContext.cs* file adds the `System.Data.Entity` namespace so that you have access to all the core functionality of the Entity Framework.</span></span> <span data-ttu-id="4fa08-144">此功能包括查询、 插入、 更新和删除通过使用强类型化对象数据的功能。</span><span class="sxs-lookup"><span data-stu-id="4fa08-144">This functionality includes the capability to query, insert, update, and delete data by working with strongly typed objects.</span></span> <span data-ttu-id="4fa08-145">在上面的代码`ProductContext`类添加到新添加的实体框架访问`Order`和`OrderDetail`类。</span><span class="sxs-lookup"><span data-stu-id="4fa08-145">The above code in the `ProductContext` class adds Entity Framework access to the newly added `Order` and `OrderDetail` classes.</span></span>

## <a name="adding-checkout-access"></a><span data-ttu-id="4fa08-146">添加签出访问</span><span class="sxs-lookup"><span data-stu-id="4fa08-146">Adding Checkout Access</span></span>

<span data-ttu-id="4fa08-147">Wingtip Toys 示例应用程序允许匿名用户，以查看并添加到购物车的产品。</span><span class="sxs-lookup"><span data-stu-id="4fa08-147">The Wingtip Toys sample application allows anonymous users to review and add products to a shopping cart.</span></span> <span data-ttu-id="4fa08-148">但是，当匿名用户选择购买它们添加到购物车的产品，它们就必须登录到站点。</span><span class="sxs-lookup"><span data-stu-id="4fa08-148">However, when anonymous users choose to purchase the products they added to the shopping cart, they must log on to the site.</span></span> <span data-ttu-id="4fa08-149">它们具有登录后，他们可以访问的受限的页面的 Web 应用程序处理签出和购买过程。</span><span class="sxs-lookup"><span data-stu-id="4fa08-149">Once they have logged on, they can access the restricted pages of the Web application that handle the checkout and purchase process.</span></span> <span data-ttu-id="4fa08-150">这些限制的页面包含在*签出*应用程序文件夹。</span><span class="sxs-lookup"><span data-stu-id="4fa08-150">These restricted pages are contained in the *Checkout* folder of the application.</span></span>

### <a name="add-a-checkout-folder-and-pages"></a><span data-ttu-id="4fa08-151">添加签出文件夹和页</span><span class="sxs-lookup"><span data-stu-id="4fa08-151">Add a Checkout Folder and Pages</span></span>

<span data-ttu-id="4fa08-152">您现在将创建*签出*文件夹和它客户将看到在结帐过程中的页。</span><span class="sxs-lookup"><span data-stu-id="4fa08-152">You will now create the *Checkout* folder and the pages in it that the customer will see during the checkout process.</span></span> <span data-ttu-id="4fa08-153">在本教程后面，将更新这些网页。</span><span class="sxs-lookup"><span data-stu-id="4fa08-153">You will update these pages later in this tutorial.</span></span>

1. <span data-ttu-id="4fa08-154">右键单击项目名称 (**Wingtip Toys**) 中**解决方案资源管理器**和选择**添加一个新文件夹**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-154">Right-click the project name (**Wingtip Toys**) in **Solution Explorer** and select **Add a New Folder**.</span></span> 

    ![签出和付款与 PayPal 的新文件夹](checkout-and-payment-with-paypal/_static/image1.png)
2. <span data-ttu-id="4fa08-156">将新文件夹命名*签出*。</span><span class="sxs-lookup"><span data-stu-id="4fa08-156">Name the new folder *Checkout*.</span></span>
3. <span data-ttu-id="4fa08-157">右键单击*签出*文件夹，然后选择**添加**-&gt;**新项**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-157">Right-click the *Checkout* folder and then select **Add**-&gt;**New Item**.</span></span> 

    ![签出和付款与 PayPal-新项](checkout-and-payment-with-paypal/_static/image2.png)
4. <span data-ttu-id="4fa08-159">随即出现“添加新项”对话框。</span><span class="sxs-lookup"><span data-stu-id="4fa08-159">The **Add New Item** dialog box is displayed.</span></span>
5. <span data-ttu-id="4fa08-160">选择**Visual C#**  - &gt; **Web**左侧的模板组。</span><span class="sxs-lookup"><span data-stu-id="4fa08-160">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="4fa08-161">然后，从中间窗格中，选择**包含母版页的 Web 窗体**并将其命名*CheckoutStart.aspx*。</span><span class="sxs-lookup"><span data-stu-id="4fa08-161">Then, from the middle pane, select **Web Form with Master Page**and name it *CheckoutStart.aspx*.</span></span> 

    ![签出和付款 PayPal-使用添加新项对话框](checkout-and-payment-with-paypal/_static/image3.png)
6. <span data-ttu-id="4fa08-163">如前所述，选择*Site.Master*作为主控页文件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-163">As before, select the *Site.Master* file as the master page.</span></span>
7. <span data-ttu-id="4fa08-164">添加到以下的其他页面*签出*文件夹使用上述相同的步骤：</span><span class="sxs-lookup"><span data-stu-id="4fa08-164">Add the following additional pages to the *Checkout* folder using the same steps above:</span></span>   

    - <span data-ttu-id="4fa08-165">CheckoutReview.aspx</span><span class="sxs-lookup"><span data-stu-id="4fa08-165">CheckoutReview.aspx</span></span>
    - <span data-ttu-id="4fa08-166">CheckoutComplete.aspx</span><span class="sxs-lookup"><span data-stu-id="4fa08-166">CheckoutComplete.aspx</span></span>
    - <span data-ttu-id="4fa08-167">CheckoutCancel.aspx</span><span class="sxs-lookup"><span data-stu-id="4fa08-167">CheckoutCancel.aspx</span></span>
    - <span data-ttu-id="4fa08-168">CheckoutError.aspx</span><span class="sxs-lookup"><span data-stu-id="4fa08-168">CheckoutError.aspx</span></span>

### <a name="add-a-webconfig-file"></a><span data-ttu-id="4fa08-169">添加 Web.config 文件</span><span class="sxs-lookup"><span data-stu-id="4fa08-169">Add a Web.config File</span></span>

<span data-ttu-id="4fa08-170">添加一个新的*Web.config*文件为*签出*文件夹中，你将能够限制对文件夹中包含的所有页的访问。</span><span class="sxs-lookup"><span data-stu-id="4fa08-170">By adding a new *Web.config* file to the *Checkout* folder, you will be able to restrict access to all the pages contained in the folder.</span></span>

1. <span data-ttu-id="4fa08-171">右键单击*签出*文件夹，然后选择**添加** - &gt; **新项**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-171">Right-click the *Checkout* folder and select **Add** -&gt; **New Item**.</span></span>  
 <span data-ttu-id="4fa08-172">随即出现“添加新项”对话框。</span><span class="sxs-lookup"><span data-stu-id="4fa08-172">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="4fa08-173">选择**Visual C#**  - &gt; **Web**左侧的模板组。</span><span class="sxs-lookup"><span data-stu-id="4fa08-173">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="4fa08-174">然后，从中间窗格中，选择**Web 配置文件**，接受默认名称的*Web.config*，然后选择**添加**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-174">Then, from the middle pane, select **Web Configuration File**, accept the default name of *Web.config*, and then select **Add**.</span></span>
3. <span data-ttu-id="4fa08-175">替换的现有 XML 内容中*Web.config*替换为以下文件：</span><span class="sxs-lookup"><span data-stu-id="4fa08-175">Replace the existing XML content in the *Web.config* file with the following:</span></span>  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. <span data-ttu-id="4fa08-176">保存*Web.config*文件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-176">Save the *Web.config* file.</span></span>

<span data-ttu-id="4fa08-177">*Web.config*文件指定的 Web 应用程序的所有未知的用户必须被拒绝访问中包含的页*签出*文件夹。</span><span class="sxs-lookup"><span data-stu-id="4fa08-177">The *Web.config* file specifies that all unknown users of the Web application must be denied access to the pages contained in the *Checkout* folder.</span></span> <span data-ttu-id="4fa08-178">但是，如果用户已注册一个帐户，并且用户登录，则它们将是已知的用户，将有权访问中的页*签出*文件夹。</span><span class="sxs-lookup"><span data-stu-id="4fa08-178">However, if the user has registered an account and is logged on, they will be a known user and will have access to the pages in the *Checkout* folder.</span></span>

<span data-ttu-id="4fa08-179">务必要注意，ASP.NET 配置如下所示层次结构，其中每个*Web.config*文件将配置设置应用到中的文件夹和所有它下面的子目录。</span><span class="sxs-lookup"><span data-stu-id="4fa08-179">It's important to note that ASP.NET configuration follows a hierarchy, where each *Web.config* file applies configuration settings to the folder that it is in and to all of the child directories below it.</span></span>

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a><span data-ttu-id="4fa08-180">为项目启用 SSL</span><span class="sxs-lookup"><span data-stu-id="4fa08-180">Enable SSL for the Project</span></span>

 <span data-ttu-id="4fa08-181">安全套接字层 (SSL) 是定义为允许 Web 服务器和 Web 客户端通过使用加密更安全地通信协议。</span><span class="sxs-lookup"><span data-stu-id="4fa08-181">Secure Sockets Layer (SSL) is a protocol defined to allow Web servers and Web clients to communicate more securely through the use of encryption.</span></span> <span data-ttu-id="4fa08-182">不使用 SSL，则客户端和服务器之间发送数据时，具有到网络的物理访问权限的任何人都探查数据包。</span><span class="sxs-lookup"><span data-stu-id="4fa08-182">When SSL is not used, data sent between the client and server is open to packet sniffing by anyone with physical access to the network.</span></span> <span data-ttu-id="4fa08-183">此外，几种常见的身份验证方案是不安全通过一般 HTTP。</span><span class="sxs-lookup"><span data-stu-id="4fa08-183">Additionally, several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="4fa08-184">具体而言，基本身份验证和窗体身份验证发送未加密的凭据。</span><span class="sxs-lookup"><span data-stu-id="4fa08-184">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="4fa08-185">为确保安全，这些身份验证方案必须使用 SSL。</span><span class="sxs-lookup"><span data-stu-id="4fa08-185">To be secure, these authentication schemes must use SSL.</span></span> 

1. <span data-ttu-id="4fa08-186">在**解决方案资源管理器**，单击**WingtipToys**项目，然后按**F4**以显示**属性**窗口。</span><span class="sxs-lookup"><span data-stu-id="4fa08-186">In **Solution Explorer**, click the **WingtipToys** project, then press **F4** to display the **Properties** window.</span></span>
2. <span data-ttu-id="4fa08-187">更改**已启用 SSL**到`true`。</span><span class="sxs-lookup"><span data-stu-id="4fa08-187">Change **SSL Enabled** to `true`.</span></span>
3. <span data-ttu-id="4fa08-188">复制**SSL URL**以便稍后使用。</span><span class="sxs-lookup"><span data-stu-id="4fa08-188">Copy the **SSL URL** so you can use it later.</span></span>   
 <span data-ttu-id="4fa08-189">SSL URL 将为`https://localhost:44300/`除非你之前已创建 SSL 网站 （如下所示）。</span><span class="sxs-lookup"><span data-stu-id="4fa08-189">The SSL URL will be `https://localhost:44300/` unless you've previously created SSL Web Sites (as shown below).</span></span>   
    <span data-ttu-id="4fa08-190">![项目属性](checkout-and-payment-with-paypal/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="4fa08-190">![Project Properties](checkout-and-payment-with-paypal/_static/image4.png)</span></span>
4. <span data-ttu-id="4fa08-191">在**解决方案资源管理器**，右键单击**WingtipToys**项目，然后单击**属性**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-191">In **Solution Explorer**, right click the **WingtipToys** project and click **Properties**.</span></span>
5. <span data-ttu-id="4fa08-192">在左侧选项卡中，单击**Web**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-192">In the left tab, click **Web**.</span></span>
6. <span data-ttu-id="4fa08-193">更改**项目 Url**使用**SSL URL**之前保存。</span><span class="sxs-lookup"><span data-stu-id="4fa08-193">Change the **Project Url** to use the **SSL URL** that you saved earlier.</span></span>   
    <span data-ttu-id="4fa08-194">![项目 Web 属性](checkout-and-payment-with-paypal/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="4fa08-194">![Project Web Properties](checkout-and-payment-with-paypal/_static/image5.png)</span></span>
7. <span data-ttu-id="4fa08-195">通过按保存页面**CTRL + S**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-195">Save the page by pressing **CTRL+S**.</span></span>
8. <span data-ttu-id="4fa08-196">按**Ctrl + F5**运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="4fa08-196">Press **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="4fa08-197">Visual Studio 将显示一个选项可用于避免 SSL 警告。</span><span class="sxs-lookup"><span data-stu-id="4fa08-197">Visual Studio will display an option to allow you to avoid SSL warnings.</span></span>
9. <span data-ttu-id="4fa08-198">单击**是**以信任 IIS Express SSL 证书并继续。</span><span class="sxs-lookup"><span data-stu-id="4fa08-198">Click **Yes** to trust the IIS Express SSL certificate and continue.</span></span>   
    <span data-ttu-id="4fa08-199">![IIS Express SSL 证书详细信息](checkout-and-payment-with-paypal/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="4fa08-199">![IIS Express SSL certificate details](checkout-and-payment-with-paypal/_static/image6.png)</span></span>  
 <span data-ttu-id="4fa08-200">将显示安全警告。</span><span class="sxs-lookup"><span data-stu-id="4fa08-200">A Security Warning is displayed.</span></span>
10. <span data-ttu-id="4fa08-201">单击**是**将证书安装到本地主机。</span><span class="sxs-lookup"><span data-stu-id="4fa08-201">Click **Yes** to install the certificate to your localhost.</span></span>   
    <span data-ttu-id="4fa08-202">![安全警告对话框中](checkout-and-payment-with-paypal/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="4fa08-202">![Security Warning dialog box](checkout-and-payment-with-paypal/_static/image7.png)</span></span>  
 <span data-ttu-id="4fa08-203">将显示浏览器窗口。</span><span class="sxs-lookup"><span data-stu-id="4fa08-203">The browser window will be displayed.</span></span>

<span data-ttu-id="4fa08-204">你现在可以轻松测试 Web 应用程序本地使用 SSL。</span><span class="sxs-lookup"><span data-stu-id="4fa08-204">You can now easily test your Web application locally using SSL.</span></span>

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a><span data-ttu-id="4fa08-205">添加 OAuth 2.0 提供程序</span><span class="sxs-lookup"><span data-stu-id="4fa08-205">Add an OAuth 2.0 Provider</span></span>

<span data-ttu-id="4fa08-206">ASP.NET Web 窗体提供了增强的选项，用于成员身份和身份验证。</span><span class="sxs-lookup"><span data-stu-id="4fa08-206">ASP.NET Web Forms provides enhanced options for membership and authentication.</span></span> <span data-ttu-id="4fa08-207">这些增强功能包括 OAuth。</span><span class="sxs-lookup"><span data-stu-id="4fa08-207">These enhancements include OAuth.</span></span> <span data-ttu-id="4fa08-208">OAuth 是一种开放协议，允许以一种简单而标准的方法，从 web、 移动和桌面应用程序进行安全授权。</span><span class="sxs-lookup"><span data-stu-id="4fa08-208">OAuth is an open protocol that allows secure authorization in a simple and standard method from web, mobile, and desktop applications.</span></span> <span data-ttu-id="4fa08-209">ASP.NET Web 窗体模板使用 OAuth 公开 Facebook、 Twitter、 Google 和 Microsoft 作为身份验证提供程序。</span><span class="sxs-lookup"><span data-stu-id="4fa08-209">The ASP.NET Web Forms template uses OAuth to expose Facebook, Twitter, Google and Microsoft as authentication providers.</span></span> <span data-ttu-id="4fa08-210">虽然本教程仅使用 Google 作为身份验证提供程序，可轻松修改代码以使用任何提供程序。</span><span class="sxs-lookup"><span data-stu-id="4fa08-210">Although this tutorial uses only Google as the authentication provider, you can easily modify the code to use any of the providers.</span></span> <span data-ttu-id="4fa08-211">实施其他提供程序的步骤都非常类似于将在本教程中看到的步骤。</span><span class="sxs-lookup"><span data-stu-id="4fa08-211">The steps to implement other providers are very similar to the steps you will see in this tutorial.</span></span>

<span data-ttu-id="4fa08-212">除了身份验证，本教程还将使用角色实施授权。</span><span class="sxs-lookup"><span data-stu-id="4fa08-212">In addition to authentication, the tutorial will also use roles to implement authorization.</span></span> <span data-ttu-id="4fa08-213">你添加到这些用户`canEdit`角色将能够更改数据 （创建、 编辑或删除联系人）。</span><span class="sxs-lookup"><span data-stu-id="4fa08-213">Only those users you add to the `canEdit` role will be able to change data (create, edit, or delete contacts).</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="4fa08-214">Windows Live 应用程序只接受的实时工作网站，URL，因此不能用于本地网站 URL 测试登录名。</span><span class="sxs-lookup"><span data-stu-id="4fa08-214">Windows Live applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>


<span data-ttu-id="4fa08-215">以下步骤将允许你将添加 Google 身份验证提供程序。</span><span class="sxs-lookup"><span data-stu-id="4fa08-215">The following steps will allow you to add a Google authentication provider.</span></span>

1. <span data-ttu-id="4fa08-216">打开*应用\_Start\Startup.Auth.cs*文件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-216">Open the *App\_Start\Startup.Auth.cs* file.</span></span>
2. <span data-ttu-id="4fa08-217">删除注释字符从`app.UseGoogleAuthentication()`方法以便，该方法显示为如下所示：</span><span class="sxs-lookup"><span data-stu-id="4fa08-217">Remove the comment characters from the `app.UseGoogleAuthentication()` method so that the method appears as follows:</span></span> 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. <span data-ttu-id="4fa08-218">导航到[Google 开发人员控制台](https://console.developers.google.com/)。</span><span class="sxs-lookup"><span data-stu-id="4fa08-218">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span> <span data-ttu-id="4fa08-219">你还需要使用 Google 开发人员电子邮件帐户 (gmail.com) 登录。</span><span class="sxs-lookup"><span data-stu-id="4fa08-219">You will also need to sign-in with your Google developer email account (gmail.com).</span></span> <span data-ttu-id="4fa08-220">如果你没有 Google 帐户，选择**创建帐户**链接。</span><span class="sxs-lookup"><span data-stu-id="4fa08-220">If you do not have a Google account, select the **Create an account** link.</span></span>   
 <span data-ttu-id="4fa08-221">接下来，你将看到**Google 开发人员控制台**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-221">Next, you'll see the **Google Developers Console**.</span></span>   
    <span data-ttu-id="4fa08-222">![Google 开发人员控制台](checkout-and-payment-with-paypal/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="4fa08-222">![Google Developers Console](checkout-and-payment-with-paypal/_static/image8.png)</span></span>
4. <span data-ttu-id="4fa08-223">单击**创建项目**按钮，然后输入项目名称和 ID （可以使用默认值）。</span><span class="sxs-lookup"><span data-stu-id="4fa08-223">Click the **Create Project** button and enter a project name and ID (you can use the default values).</span></span> <span data-ttu-id="4fa08-224">然后，单击**协议复选框**和**创建**按钮。</span><span class="sxs-lookup"><span data-stu-id="4fa08-224">Then, click the **agreement checkbox** and the **Create** button.</span></span>  

    ![Google-新建项目](checkout-and-payment-with-paypal/_static/image9.png)

 <span data-ttu-id="4fa08-226">几秒钟后将创建新项目和你的浏览器将显示新项目页。</span><span class="sxs-lookup"><span data-stu-id="4fa08-226">In a few seconds the new project will be created and your browser will display the new projects page.</span></span>
5. <span data-ttu-id="4fa08-227">在左侧选项卡中，单击**Api&amp;身份验证**，然后单击**凭据**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-227">In the left tab, click **APIs &amp; auth**, and then click **Credentials**.</span></span>
6. <span data-ttu-id="4fa08-228">单击**创建新的客户端 ID**下**OAuth**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-228">Click the **Create New Client ID** under **OAuth**.</span></span>   
 <span data-ttu-id="4fa08-229">**创建客户端 ID**将显示对话框。</span><span class="sxs-lookup"><span data-stu-id="4fa08-229">The **Create Client ID** dialog will be displayed.</span></span>   
    <span data-ttu-id="4fa08-230">![Google-创建客户端 ID](checkout-and-payment-with-paypal/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="4fa08-230">![Google - Create Client ID](checkout-and-payment-with-paypal/_static/image10.png)</span></span>
7. <span data-ttu-id="4fa08-231">在**创建客户端 ID**对话框中，保留默认值**Web 应用程序**的应用程序类型。</span><span class="sxs-lookup"><span data-stu-id="4fa08-231">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
8. <span data-ttu-id="4fa08-232">设置**授权的 JavaScript 来源**为之前在本教程中使用的 SSL URL (`https://localhost:44300/`除非你已创建其他 SSL 项目)。</span><span class="sxs-lookup"><span data-stu-id="4fa08-232">Set the **Authorized JavaScript Origins** to the SSL URL you used earlier in this tutorial (`https://localhost:44300/` unless you've created other SSL projects).</span></span>   
 <span data-ttu-id="4fa08-233">此 URL 是你的应用程序的来源。</span><span class="sxs-lookup"><span data-stu-id="4fa08-233">This URL is the origin for your application.</span></span> <span data-ttu-id="4fa08-234">此示例中，你只需输入 localhost 测试 URL。</span><span class="sxs-lookup"><span data-stu-id="4fa08-234">For this sample, you will only enter the localhost test URL.</span></span> <span data-ttu-id="4fa08-235">但是，您可以输入多个 Url，以针对 localhost 和生产。</span><span class="sxs-lookup"><span data-stu-id="4fa08-235">However, you can enter multiple URLs to account for localhost and production.</span></span>
9. <span data-ttu-id="4fa08-236">设置**授权重定向 URI**所示：</span><span class="sxs-lookup"><span data-stu-id="4fa08-236">Set the **Authorized Redirect URI** to the following:</span></span> 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

 <span data-ttu-id="4fa08-237">此值是 URI ASP.NET OAuth 用户与 google OAuth 服务器通信。</span><span class="sxs-lookup"><span data-stu-id="4fa08-237">This value is the URI that ASP.NET OAuth users to communicate with the google OAuth server.</span></span> <span data-ttu-id="4fa08-238">请记住上面使用的 SSL URL (`https://localhost:44300/`除非你已创建其他 SSL 项目)。</span><span class="sxs-lookup"><span data-stu-id="4fa08-238">Remember the SSL URL you used above (    `https://localhost:44300/` unless you've created other SSL projects).</span></span>
10. <span data-ttu-id="4fa08-239">单击**创建客户端 ID**按钮。</span><span class="sxs-lookup"><span data-stu-id="4fa08-239">Click the **Create Client ID** button.</span></span>
11. <span data-ttu-id="4fa08-240">在 Google 开发人员控制台的左侧菜单上，单击**同意屏幕**菜单项，然后设置你的电子邮件地址和产品名称。</span><span class="sxs-lookup"><span data-stu-id="4fa08-240">On the left menu of the Google Developers Console, click the **Consent screen** menu item, then set your email address and product name.</span></span> <span data-ttu-id="4fa08-241">完成窗体后，单击**保存**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-241">When you have completed the form, click **Save**.</span></span>
12. <span data-ttu-id="4fa08-242">单击**Api**菜单项，向下的滚动，单击**关闭**按钮旁边**Google + API**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-242">Click the **APIs** menu item, scroll down and click the **off** button next to **Google+ API**.</span></span>   
 <span data-ttu-id="4fa08-243">接受此选项将启用 Google + API。</span><span class="sxs-lookup"><span data-stu-id="4fa08-243">Accepting this option will enable the Google+ API.</span></span>
13. <span data-ttu-id="4fa08-244">你还必须更新**Microsoft.Owin** 3.0.0 版的 NuGet 程序包。</span><span class="sxs-lookup"><span data-stu-id="4fa08-244">You must also update the **Microsoft.Owin** NuGet package to version 3.0.0.</span></span>   
 <span data-ttu-id="4fa08-245">从**工具**菜单上，选择**NuGet 包管理器**，然后选择**管理解决方案的 NuGet 包**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-245">From the **Tools** menu, select **NuGet Package Manager** and then select **Manage NuGet Packages for Solution**.</span></span>  
 <span data-ttu-id="4fa08-246">从**管理 NuGet 包**窗口中，找到并更新**Microsoft.Owin** 3.0.0 版的包。</span><span class="sxs-lookup"><span data-stu-id="4fa08-246">From the **Manage NuGet Packages** window, find and update the **Microsoft.Owin** package to version 3.0.0.</span></span>
14. <span data-ttu-id="4fa08-247">在 Visual Studio 中，更新`UseGoogleAuthentication`方法*Startup.Auth.cs*页上通过复制和粘贴**客户端 ID**和**客户端机密**到方法。</span><span class="sxs-lookup"><span data-stu-id="4fa08-247">In Visual Studio, update the `UseGoogleAuthentication` method of the *Startup.Auth.cs* page by copying and pasting the **Client ID** and **Client Secret** into the method.</span></span> <span data-ttu-id="4fa08-248">**客户端 ID**和**客户端机密**如下所示的值是示例，不起作用。</span><span class="sxs-lookup"><span data-stu-id="4fa08-248">The **Client ID** and **Client Secret** values shown below are samples and will not work.</span></span> 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. <span data-ttu-id="4fa08-249">按**CTRL + F5**生成并运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="4fa08-249">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="4fa08-250">单击**登录**链接。</span><span class="sxs-lookup"><span data-stu-id="4fa08-250">Click the **Log in** link.</span></span>
16. <span data-ttu-id="4fa08-251">下**使用另一个服务以要求在登录**，单击**Google**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-251">Under **Use another service to log in**, click **Google**.</span></span>  
    <span data-ttu-id="4fa08-252">![登录](checkout-and-payment-with-paypal/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="4fa08-252">![Log in](checkout-and-payment-with-paypal/_static/image11.png)</span></span>
17. <span data-ttu-id="4fa08-253">如果你需要输入你的凭据，你将重定向到 google 站点将在其中输入你的凭据。</span><span class="sxs-lookup"><span data-stu-id="4fa08-253">If you need to enter your credentials, you will be redirected to the google site where you will enter your credentials.</span></span>  
    ![Google-登录](checkout-and-payment-with-paypal/_static/image12.png)
18. <span data-ttu-id="4fa08-255">输入你的凭据后，系统将提示，为刚创建的 web 应用程序授予权限。</span><span class="sxs-lookup"><span data-stu-id="4fa08-255">After you enter your credentials, you will be prompted to give permissions to the web application you just created.</span></span>  
    ![项目默认服务帐户](checkout-and-payment-with-paypal/_static/image13.png)
19. <span data-ttu-id="4fa08-257">单击**接受**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-257">Click **Accept**.</span></span> <span data-ttu-id="4fa08-258">你现在会定向回**注册**页**WingtipToys**应用程序可以在其中注册你的 Google 帐户。</span><span class="sxs-lookup"><span data-stu-id="4fa08-258">You will now be redirected back to the **Register** page of the **WingtipToys** application where you can register your Google account.</span></span>  
    <span data-ttu-id="4fa08-259">![注册你的 Google 帐户](checkout-and-payment-with-paypal/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="4fa08-259">![Register with your Google Account](checkout-and-payment-with-paypal/_static/image14.png)</span></span>
20. <span data-ttu-id="4fa08-260">你可以选择更改 Gmail 帐户使用的本地电子邮件注册名称，但你通常想要保留的默认电子邮件别名 （即，一个用于身份验证）。</span><span class="sxs-lookup"><span data-stu-id="4fa08-260">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="4fa08-261">单击**登录**如上所示。</span><span class="sxs-lookup"><span data-stu-id="4fa08-261">Click **Log in** as shown above.</span></span>

### <a name="modifying-login-functionality"></a><span data-ttu-id="4fa08-262">修改登录功能</span><span class="sxs-lookup"><span data-stu-id="4fa08-262">Modifying Login Functionality</span></span>

<span data-ttu-id="4fa08-263">如前文所述本系列教程中，大部分用户注册功能已包括在 ASP.NET Web 窗体模板默认情况下。</span><span class="sxs-lookup"><span data-stu-id="4fa08-263">As previously mentioned in this tutorial series, much of the user registration functionality has been included in the ASP.NET Web Forms template by default.</span></span> <span data-ttu-id="4fa08-264">现在将修改默认*Login.aspx*和*Register.aspx*页面调`MigrateCart`方法。</span><span class="sxs-lookup"><span data-stu-id="4fa08-264">Now you will modify the default *Login.aspx* and *Register.aspx* pages to call the `MigrateCart` method.</span></span> <span data-ttu-id="4fa08-265">`MigrateCart`方法将新登录的用户与匿名的购物车相关联。</span><span class="sxs-lookup"><span data-stu-id="4fa08-265">The `MigrateCart` method associates a newly logged in user with an anonymous shopping cart.</span></span> <span data-ttu-id="4fa08-266">通过将关联用户和购物车，Wingtip Toys 示例应用程序将无法保持购物车访问之间的用户。</span><span class="sxs-lookup"><span data-stu-id="4fa08-266">By associating the user and shopping cart, the Wingtip Toys sample application will be able to maintain the shopping cart of the user between visits.</span></span>

1. <span data-ttu-id="4fa08-267">在**解决方案资源管理器**，找到并打开*帐户*文件夹。</span><span class="sxs-lookup"><span data-stu-id="4fa08-267">In **Solution Explorer**, find and open the *Account* folder.</span></span>
2. <span data-ttu-id="4fa08-268">修改名为的代码隐藏页*Login.aspx.cs*包括以黄色，突出显示的代码，以使其显示，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4fa08-268">Modify the code-behind page named *Login.aspx.cs* to include the code highlighted in yellow, so that it appears as follows:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. <span data-ttu-id="4fa08-269">保存*Login.aspx.cs*文件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-269">Save the *Login.aspx.cs* file.</span></span>

<span data-ttu-id="4fa08-270">现在，你可以忽略没有为定义该警告`MigrateCart`方法。</span><span class="sxs-lookup"><span data-stu-id="4fa08-270">For now, you can ignore the warning that there is no definition for the `MigrateCart` method.</span></span> <span data-ttu-id="4fa08-271">你将添加它有点稍后在本教程。</span><span class="sxs-lookup"><span data-stu-id="4fa08-271">You will be adding it a bit later in this tutorial.</span></span>

<span data-ttu-id="4fa08-272">*Login.aspx.cs*代码隐藏文件支持 LogIn 方法。</span><span class="sxs-lookup"><span data-stu-id="4fa08-272">The *Login.aspx.cs* code-behind file supports a LogIn method.</span></span> <span data-ttu-id="4fa08-273">通过检查 Login.aspx 页面，你将看到此页包含"登录"按钮，当单击触发器`LogIn`上隐藏代码处理程序。</span><span class="sxs-lookup"><span data-stu-id="4fa08-273">By inspecting the Login.aspx page, you'll see that this page includes a "Log in" button that when click triggers the `LogIn` handler on the code-behind.</span></span>

<span data-ttu-id="4fa08-274">当`Login`方法*Login.aspx.cs*调用时，购物车名为的新实例`usersShoppingCart`创建。</span><span class="sxs-lookup"><span data-stu-id="4fa08-274">When the `Login` method on the *Login.aspx.cs* is called, a new instance of the shopping cart named `usersShoppingCart` is created.</span></span> <span data-ttu-id="4fa08-275">已检索的购物车 (GUID) 的 ID 并将其设置为`cartId`变量。</span><span class="sxs-lookup"><span data-stu-id="4fa08-275">The ID of the shopping cart (a GUID) is retrieved and set to the `cartId` variable.</span></span> <span data-ttu-id="4fa08-276">然后，`MigrateCart`调用方法时，将同时传递`cartId`和登录的用户对此方法的名称。</span><span class="sxs-lookup"><span data-stu-id="4fa08-276">Then, the `MigrateCart` method is called, passing both the `cartId` and the name of the logged-in user to this method.</span></span> <span data-ttu-id="4fa08-277">在迁移购物车，用于标识匿名购物车的 GUID 将替换的用户名称。</span><span class="sxs-lookup"><span data-stu-id="4fa08-277">When the shopping cart is migrated, the GUID used to identify the anonymous shopping cart is replaced with the user name.</span></span>

<span data-ttu-id="4fa08-278">除了修改*Login.aspx.cs*代码隐藏文件，以将迁移购物车，当用户登录时还必须修改*Register.aspx.cs 代码隐藏文件*迁移购物车当用户创建新帐户并登录。</span><span class="sxs-lookup"><span data-stu-id="4fa08-278">In addition to modifying the *Login.aspx.cs* code-behind file to migrate the shopping cart when the user logs in, you must also modify the *Register.aspx.cs code-behind file* to migrate the shopping cart when the user creates a new account and logs in.</span></span>

1. <span data-ttu-id="4fa08-279">在*帐户*文件夹中，打开代码隐藏文件命名为*Register.aspx.cs*。</span><span class="sxs-lookup"><span data-stu-id="4fa08-279">In the *Account* folder, open the code-behind file named *Register.aspx.cs*.</span></span>
2. <span data-ttu-id="4fa08-280">通过以黄色，包括代码修改的代码隐藏文件，以使其显示，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4fa08-280">Modify the code-behind file by including the code in yellow, so that it appears as follows:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. <span data-ttu-id="4fa08-281">保存*Register.aspx.cs*文件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-281">Save the *Register.aspx.cs* file.</span></span> <span data-ttu-id="4fa08-282">同样，有关忽略该警告`MigrateCart`方法。</span><span class="sxs-lookup"><span data-stu-id="4fa08-282">Once again, ignore the warning about the `MigrateCart` method.</span></span>

<span data-ttu-id="4fa08-283">请注意代码中所使用`CreateUser_Click`事件处理程序是非常类似于你使用的代码`LogIn`方法。</span><span class="sxs-lookup"><span data-stu-id="4fa08-283">Notice that the code you used in the `CreateUser_Click` event handler is very similar to the code you used in the `LogIn` method.</span></span> <span data-ttu-id="4fa08-284">当用户注册或登录到站点，调用`MigrateCart`将成为方法。</span><span class="sxs-lookup"><span data-stu-id="4fa08-284">When the user registers or logs in to the site, a call to the `MigrateCart` method will be made.</span></span>

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="4fa08-285">迁移购物车</span><span class="sxs-lookup"><span data-stu-id="4fa08-285">Migrating the Shopping Cart</span></span>

<span data-ttu-id="4fa08-286">现在，已更新的登录和注册过程，你可以添加代码以迁移购物车使用`MigrateCart`方法。</span><span class="sxs-lookup"><span data-stu-id="4fa08-286">Now that you have the log-in and registration process updated, you can add the code to migrate the shopping cart using the `MigrateCart` method.</span></span>

1. <span data-ttu-id="4fa08-287">在**解决方案资源管理器**，查找*逻辑*文件夹并打开*ShoppingCartActions.cs*类文件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-287">In **Solution Explorer**, find the *Logic* folder and open the *ShoppingCartActions.cs* class file.</span></span>
2. <span data-ttu-id="4fa08-288">添加代码以到中的现有代码的黄色突出显示*ShoppingCartActions.cs*文件，以便中的代码*ShoppingCartActions.cs*文件将出现，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4fa08-288">Add the code highlighted in yellow to the existing code in the *ShoppingCartActions.cs* file, so that the code in the *ShoppingCartActions.cs* file appears as follows:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

<span data-ttu-id="4fa08-289">`MigrateCart`方法使用现有 cartId 查找用户购物车。</span><span class="sxs-lookup"><span data-stu-id="4fa08-289">The `MigrateCart` method uses the existing cartId to find the shopping cart of the user.</span></span> <span data-ttu-id="4fa08-290">接下来，该代码循环访问所有购物车项，并替换`CartId`属性 (所指定的`CartItem`架构) 的登录的用户名。</span><span class="sxs-lookup"><span data-stu-id="4fa08-290">Next, the code loops through all the shopping cart items and replaces the `CartId` property (as specified by the `CartItem` schema) with the logged-in user name.</span></span>

### <a name="updating-the-database-connection"></a><span data-ttu-id="4fa08-291">更新数据库连接</span><span class="sxs-lookup"><span data-stu-id="4fa08-291">Updating the Database Connection</span></span>

<span data-ttu-id="4fa08-292">如果按照此教程使用**预构建**Wingtip Toys 示例应用程序，你必须重新创建默认成员资格数据库。</span><span class="sxs-lookup"><span data-stu-id="4fa08-292">If you are following this tutorial using the **prebuilt** Wingtip Toys sample application, you must recreate the default membership database.</span></span> <span data-ttu-id="4fa08-293">通过修改默认连接字符串，成员资格数据库将创建在下次应用程序运行的时。</span><span class="sxs-lookup"><span data-stu-id="4fa08-293">By modifying the default connection string, the membership database will be created the next time the application runs.</span></span>

1. <span data-ttu-id="4fa08-294">打开*Web.config*在项目的根目录的文件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-294">Open the *Web.config* file at the root of the project.</span></span>
2. <span data-ttu-id="4fa08-295">更新的默认连接字符串，以使其显示，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4fa08-295">Update the default connection string so that it appears as follows:</span></span>   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a><span data-ttu-id="4fa08-296">将 PayPal 集成</span><span class="sxs-lookup"><span data-stu-id="4fa08-296">Integrating PayPal</span></span>

<span data-ttu-id="4fa08-297">PayPal 是接受通过联机商人付款基于 web 的计费平台。</span><span class="sxs-lookup"><span data-stu-id="4fa08-297">PayPal is a web-based billing platform that accepts payments by online merchants.</span></span> <span data-ttu-id="4fa08-298">接下来，本教程介绍如何将 PayPal 的 Express 签出功能集成到你的应用程序。</span><span class="sxs-lookup"><span data-stu-id="4fa08-298">This tutorial next explains how to integrate PayPal's Express Checkout functionality into your application.</span></span> <span data-ttu-id="4fa08-299">快速签出允许您的客户能够使用 PayPal 支付费用他们添加到其购物车的项。</span><span class="sxs-lookup"><span data-stu-id="4fa08-299">Express Checkout allows your customers to use PayPal to pay for the items they have added to their shopping cart.</span></span>

### <a name="create-paylpal-test-accounts"></a><span data-ttu-id="4fa08-300">创建 PaylPal 测试帐户</span><span class="sxs-lookup"><span data-stu-id="4fa08-300">Create PaylPal Test Accounts</span></span>

<span data-ttu-id="4fa08-301">若要使用测试环境 PayPal，必须创建并验证开发人员测试帐户。</span><span class="sxs-lookup"><span data-stu-id="4fa08-301">To use the PayPal testing environment, you must create and verify a developer test account.</span></span> <span data-ttu-id="4fa08-302">开发人员测试帐户将用于创建购买者，测试帐户和 seller 测试帐户。</span><span class="sxs-lookup"><span data-stu-id="4fa08-302">You will use the developer test account to create a buyer test account and a seller test account.</span></span> <span data-ttu-id="4fa08-303">开发人员测试帐户凭据还将允许 Wingtip Toys 示例应用程序访问 PayPal 测试环境。</span><span class="sxs-lookup"><span data-stu-id="4fa08-303">The developer test account credentials also will allow the Wingtip Toys sample application to access the PayPal testing environment.</span></span>

1. <span data-ttu-id="4fa08-304">在浏览器中，导航到测试网站 PayPal 开发人员：</span><span class="sxs-lookup"><span data-stu-id="4fa08-304">In a browser, navigate to the PayPal developer testing site:</span></span>   
    [<span data-ttu-id="4fa08-305">https://developer.paypal.com</span><span class="sxs-lookup"><span data-stu-id="4fa08-305">https://developer.paypal.com</span></span>](https://developer.paypal.com/)
2. <span data-ttu-id="4fa08-306">如果你没有 PayPal 开发人员帐户，通过单击创建一个新帐户**注册**并按照步骤注册。</span><span class="sxs-lookup"><span data-stu-id="4fa08-306">If you don't have a PayPal developer account, create a new account by clicking **Sign Up**and following the sign up steps.</span></span> <span data-ttu-id="4fa08-307">如果你有现有的 PayPal 开发人员帐户，登录通过单击**Log In**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-307">If you have an existing PayPal developer account, sign in by clicking **Log In**.</span></span> <span data-ttu-id="4fa08-308">你将需要 PayPal 开发人员帐户测试本教程中稍后 Wingtip Toys 示例应用程序。</span><span class="sxs-lookup"><span data-stu-id="4fa08-308">You will need your PayPal developer account to test the Wingtip Toys sample application later in this tutorial.</span></span>
3. <span data-ttu-id="4fa08-309">如果你只需注册 PayPal 开发人员帐户，你可能需要验证 PayPal 你 PayPal 开发人员帐户。</span><span class="sxs-lookup"><span data-stu-id="4fa08-309">If you have just signed up for your PayPal developer account, you may need to verify your PayPal developer account with PayPal.</span></span> <span data-ttu-id="4fa08-310">可以按照 PayPal 发送到电子邮件帐户的步骤来验证你的帐户。</span><span class="sxs-lookup"><span data-stu-id="4fa08-310">You can verify your account by following the steps that PayPal sent to your email account.</span></span> <span data-ttu-id="4fa08-311">验证 PayPal 开发人员帐户后，请重新登录到测试网站 PayPal 开发人员。</span><span class="sxs-lookup"><span data-stu-id="4fa08-311">Once you have verified your PayPal developer account, log back into the PayPal developer testing site.</span></span>
4. <span data-ttu-id="4fa08-312">与需要创建一个 PayPal buyer 测试帐户，如果你已不你 PayPal 开发人员帐户登录到 PayPal 开发人员站点后有一个。</span><span class="sxs-lookup"><span data-stu-id="4fa08-312">After you are logged in to the PayPal developer site with your PayPal developer account you need to create a PayPal buyer test account if you don't already have one.</span></span> <span data-ttu-id="4fa08-313">若要创建一个 buyer 测试帐户，在 PayPal 网站，请单击**应用程序**选项卡，然后单击**沙盒帐户**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-313">To create a buyer test account, on the PayPal site click the **Applications** tab and then click **Sandbox accounts**.</span></span>   
 <span data-ttu-id="4fa08-314">**沙盒测试帐户**显示页。</span><span class="sxs-lookup"><span data-stu-id="4fa08-314">The **Sandbox test accounts** page is shown.</span></span>   

    > [!NOTE] 
    > 
    > <span data-ttu-id="4fa08-315">PayPal 开发人员网站已经提供了一个商人测试帐户。</span><span class="sxs-lookup"><span data-stu-id="4fa08-315">The PayPal Developer site already provides a merchant test account.</span></span>

    ![签出和付款与 PayPal-沙盒测试帐户](checkout-and-payment-with-paypal/_static/image15.png)
5. <span data-ttu-id="4fa08-317">在沙盒测试中的帐户页上单击**创建帐户**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-317">On the Sandbox test accounts page, click **Create Account**.</span></span>
6. <span data-ttu-id="4fa08-318">上**创建测试帐户**页上选择购买者，测试帐户电子邮件和你选择的密码。</span><span class="sxs-lookup"><span data-stu-id="4fa08-318">On the **Create test account** page choose a buyer test account email and password of your choice.</span></span>   

    > [!NOTE] 
    > 
    > <span data-ttu-id="4fa08-319">你将需要购买者电子邮件地址和密码以测试本教程末尾的 Wingtip Toys 示例应用程序。</span><span class="sxs-lookup"><span data-stu-id="4fa08-319">You will need the buyer email addresses and password to test the Wingtip Toys sample application at the end of this tutorial.</span></span>

    ![签出和付款与 PayPal-沙盒测试帐户](checkout-and-payment-with-paypal/_static/image16.png)
7. <span data-ttu-id="4fa08-321">通过单击创建 buyer 测试帐户**创建帐户**按钮。</span><span class="sxs-lookup"><span data-stu-id="4fa08-321">Create the buyer test account by clicking the **Create Account** button.</span></span>  
 <span data-ttu-id="4fa08-322">**沙盒测试帐户**显示页。</span><span class="sxs-lookup"><span data-stu-id="4fa08-322">The **Sandbox Test accounts** page is displayed.</span></span> 

    ![签出和与 PayPal-PaylPal 帐户付款](checkout-and-payment-with-paypal/_static/image17.png)
8. <span data-ttu-id="4fa08-324">上**沙盒测试帐户**页上，单击**参与实施者已经**电子邮件帐户。</span><span class="sxs-lookup"><span data-stu-id="4fa08-324">On the **Sandbox test accounts** page, click the **facilitator** email account.</span></span>  
    <span data-ttu-id="4fa08-325">**配置文件**和**通知**显示选项。</span><span class="sxs-lookup"><span data-stu-id="4fa08-325">**Profile** and **Notification** options appear.</span></span>
9. <span data-ttu-id="4fa08-326">选择**配置文件**选项，然后单击**API 凭据**查看商人测试帐户 API 的凭据。</span><span class="sxs-lookup"><span data-stu-id="4fa08-326">Select the **Profile** option, then click **API credentials** to view your API credentials for the merchant test account.</span></span>
10. <span data-ttu-id="4fa08-327">将测试 API 凭据复制到记事本。</span><span class="sxs-lookup"><span data-stu-id="4fa08-327">Copy the TEST API credentials to notepad.</span></span>

<span data-ttu-id="4fa08-328">你将需要测试环境 PayPal 显示经典测试 API 凭据 （用户名、 密码和签名） 可以从 Wingtip Toys 示例应用程序的 API 调用。</span><span class="sxs-lookup"><span data-stu-id="4fa08-328">You will need your displayed Classic TEST API credentials (Username, Password, and Signature) to make API calls from the Wingtip Toys sample application to the PayPal testing environment.</span></span> <span data-ttu-id="4fa08-329">你将在下一步中添加凭据。</span><span class="sxs-lookup"><span data-stu-id="4fa08-329">You will add the credentials in the next step.</span></span>

### <a name="add-paypal-class-and-api-credentials"></a><span data-ttu-id="4fa08-330">添加 PayPal 类和 API 凭据</span><span class="sxs-lookup"><span data-stu-id="4fa08-330">Add PayPal Class and API Credentials</span></span>

<span data-ttu-id="4fa08-331">请将大部分 PayPal 代码成一个类。</span><span class="sxs-lookup"><span data-stu-id="4fa08-331">You will place the majority of the PayPal code into a single class.</span></span> <span data-ttu-id="4fa08-332">此类包含用于与 PayPal 进行通信的方法。</span><span class="sxs-lookup"><span data-stu-id="4fa08-332">This class contains the methods used to communicate with PayPal.</span></span> <span data-ttu-id="4fa08-333">此外，你将与此类添加 PayPal 凭据。</span><span class="sxs-lookup"><span data-stu-id="4fa08-333">Also, you will add your PayPal credentials to this class.</span></span>

1. <span data-ttu-id="4fa08-334">在 Visual Studio 中 Wingtip Toys 示例应用程序中，右键单击**逻辑**文件夹，然后选择**添加** - &gt; **新项**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-334">In the Wingtip Toys sample application within Visual Studio, right-click the **Logic** folder and then select **Add** -&gt; **New Item**.</span></span>   
 <span data-ttu-id="4fa08-335">随即出现“添加新项”对话框。</span><span class="sxs-lookup"><span data-stu-id="4fa08-335">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="4fa08-336">下**Visual C#**从**已安装**左侧窗格中，选择**代码**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-336">Under **Visual C#** from the **Installed** pane on the left, select **Code**.</span></span>
3. <span data-ttu-id="4fa08-337">从中间窗格中，选择**类**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-337">From the middle pane, select **Class**.</span></span> <span data-ttu-id="4fa08-338">将此新类**PayPalFunctions.cs**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-338">Name this new class **PayPalFunctions.cs**.</span></span>
4. <span data-ttu-id="4fa08-339">单击 **“添加”**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-339">Click **Add**.</span></span>  
 <span data-ttu-id="4fa08-340">编辑器中将显示新的类文件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-340">The new class file is displayed in the editor.</span></span>
5. <span data-ttu-id="4fa08-341">默认代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="4fa08-341">Replace the default code with the following code:</span></span>  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. <span data-ttu-id="4fa08-342">添加，以便您可以进行函数调用到 PayPal 测试环境，在本教程中前面显示的 Merchant API 凭据 （用户名、 密码和签名）。</span><span class="sxs-lookup"><span data-stu-id="4fa08-342">Add the Merchant API credentials (Username, Password, and Signature) that you displayed earlier in this tutorial so that you can make function calls to the PayPal testing environment.</span></span>  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="4fa08-343">在此示例应用程序要只需将凭据添加到 C# 文件 (.cs)。</span><span class="sxs-lookup"><span data-stu-id="4fa08-343">In this sample application you are simply adding credentials to a C# file (.cs).</span></span> <span data-ttu-id="4fa08-344">但是，在实现解决方案中，应考虑加密你的凭据配置文件中。</span><span class="sxs-lookup"><span data-stu-id="4fa08-344">However, in a implemented solution, you should consider encrypting your credentials in a configuration file.</span></span>


<span data-ttu-id="4fa08-345">NVPAPICaller 类包含大多数 PayPal 功能。</span><span class="sxs-lookup"><span data-stu-id="4fa08-345">The NVPAPICaller class contains the majority of the PayPal functionality.</span></span> <span data-ttu-id="4fa08-346">在类中的代码提供了进行一次测试 PayPal 测试环境从购买所需的方法。</span><span class="sxs-lookup"><span data-stu-id="4fa08-346">The code in the class provides the methods needed to make a test purchase from the PayPal testing environment.</span></span> <span data-ttu-id="4fa08-347">在以下三个 PayPal 函数用于进行购买时：</span><span class="sxs-lookup"><span data-stu-id="4fa08-347">The following three PayPal functions are used to make purchases:</span></span>

- <span data-ttu-id="4fa08-348">`SetExpressCheckout`函数</span><span class="sxs-lookup"><span data-stu-id="4fa08-348">`SetExpressCheckout` function</span></span>
- <span data-ttu-id="4fa08-349">`GetExpressCheckoutDetails`函数</span><span class="sxs-lookup"><span data-stu-id="4fa08-349">`GetExpressCheckoutDetails` function</span></span>
- <span data-ttu-id="4fa08-350">`DoExpressCheckoutPayment`函数</span><span class="sxs-lookup"><span data-stu-id="4fa08-350">`DoExpressCheckoutPayment` function</span></span>

<span data-ttu-id="4fa08-351">`ShortcutExpressCheckout`方法收集测试采购信息和产品详细信息从购物车和调用`SetExpressCheckout`PayPal 函数。</span><span class="sxs-lookup"><span data-stu-id="4fa08-351">The `ShortcutExpressCheckout` method collects the test purchase information and product details from the shopping cart and calls the `SetExpressCheckout` PayPal function.</span></span> <span data-ttu-id="4fa08-352">`GetCheckoutDetails`方法确认采购详细信息和调用`GetExpressCheckoutDetails`PayPal 函数在进行测试购买之前。</span><span class="sxs-lookup"><span data-stu-id="4fa08-352">The `GetCheckoutDetails` method confirms purchase details and calls the `GetExpressCheckoutDetails` PayPal function before making the test purchase.</span></span> <span data-ttu-id="4fa08-353">`DoCheckoutPayment`方法通过调用来完成测试环境中的测试购买`DoExpressCheckoutPayment`PayPal 函数。</span><span class="sxs-lookup"><span data-stu-id="4fa08-353">The `DoCheckoutPayment` method completes the test purchase from the testing environment by calling the `DoExpressCheckoutPayment` PayPal function.</span></span> <span data-ttu-id="4fa08-354">剩余代码支持 PayPal 方法和过程，例如编码字符串、 解码字符串、 处理数组，以及确定凭据。</span><span class="sxs-lookup"><span data-stu-id="4fa08-354">The remaining code supports the PayPal methods and process, such as encoding strings, decoding strings, processing arrays, and determining credentials.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="4fa08-355">PayPal 允许你将包含可选采购详细信息基于[PayPal 的 API 规范](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout)。</span><span class="sxs-lookup"><span data-stu-id="4fa08-355">PayPal allows you to include optional purchase details based on [PayPal's API specification](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout).</span></span> <span data-ttu-id="4fa08-356">通过扩展 Wingtip Toys 示例应用程序中的代码，可以包括本地化详细信息、 产品说明、 税金、 客户服务编号，以及许多其他可选字段。</span><span class="sxs-lookup"><span data-stu-id="4fa08-356">By extending the code in the Wingtip Toys sample application, you can include localization details, product descriptions, tax, a customer service number, as well as many other optional fields.</span></span>


<span data-ttu-id="4fa08-357">请注意，在指定的返回和取消 Url **ShortcutExpressCheckout**方法使用的端口号。</span><span class="sxs-lookup"><span data-stu-id="4fa08-357">Notice that the return and cancel URLs that are specified in the **ShortcutExpressCheckout** method use a port number.</span></span>

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

<span data-ttu-id="4fa08-358">Visual Web Developer 运行时使用 SSL 的 web 项目，通常使用的端口 44300 是为 web 服务器。</span><span class="sxs-lookup"><span data-stu-id="4fa08-358">When Visual Web Developer runs a web project using SSL, commonly the port 44300 is used for the web server.</span></span> <span data-ttu-id="4fa08-359">如上所示，端口号是 44300。</span><span class="sxs-lookup"><span data-stu-id="4fa08-359">As shown above, the port number is 44300.</span></span> <span data-ttu-id="4fa08-360">运行应用程序时，你可能会看到不同的端口号。</span><span class="sxs-lookup"><span data-stu-id="4fa08-360">When you run the application, you could see a different port number.</span></span> <span data-ttu-id="4fa08-361">你端口号必须是正确的代码中设置，以便你可以成功运行本教程末尾 Wingtip Toys 示例应用程序。</span><span class="sxs-lookup"><span data-stu-id="4fa08-361">Your port number needs to be correctly set in the code so that you can successful run the Wingtip Toys sample application at the end of this tutorial.</span></span> <span data-ttu-id="4fa08-362">本教程的下一节说明如何检索本地主机端口号和更新 PayPal 类。</span><span class="sxs-lookup"><span data-stu-id="4fa08-362">The next section of this tutorial explains how to retrieve the local host port number and update the PayPal class.</span></span>

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a><span data-ttu-id="4fa08-363">更新 PayPal 类中的 LocalHost 端口号</span><span class="sxs-lookup"><span data-stu-id="4fa08-363">Update the LocalHost Port Number in the PayPal Class</span></span>

<span data-ttu-id="4fa08-364">Wingtip Toys 示例应用程序购买产品，通过导航到 PayPal 测试站点，并返回与你的 Wingtip Toys 示例应用程序的本地实例。</span><span class="sxs-lookup"><span data-stu-id="4fa08-364">The Wingtip Toys sample application purchases products by navigating to the PayPal testing site and returning to your local instance of the Wingtip Toys sample application.</span></span> <span data-ttu-id="4fa08-365">为了具有 PayPal 返回到正确的 URL，你需要指定本地运行的端口号示例上面提到的 PayPal 代码中的应用程序。</span><span class="sxs-lookup"><span data-stu-id="4fa08-365">In order to have PayPal return to the correct URL, you need to specify the port number of the locally running sample application in the PayPal code mentioned above.</span></span>

1. <span data-ttu-id="4fa08-366">右键单击项目名称 (**WingtipToys**) 中**解决方案资源管理器**和选择**属性**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-366">Right-click the project name (**WingtipToys**) in **Solution Explorer** and select **Properties**.</span></span>
2. <span data-ttu-id="4fa08-367">在左栏中，选择**Web**选项卡。</span><span class="sxs-lookup"><span data-stu-id="4fa08-367">In the left column, select the **Web** tab.</span></span>
3. <span data-ttu-id="4fa08-368">检索中的端口号**项目 Url**框。</span><span class="sxs-lookup"><span data-stu-id="4fa08-368">Retrieve the port number from the **Project Url** box.</span></span>
4. <span data-ttu-id="4fa08-369">如果需要更新`returnURL`和`cancelURL`PayPal 类中 (`NVPAPICaller`) 中*PayPalFunctions.cs*文件以使用 web 应用程序的端口号：</span><span class="sxs-lookup"><span data-stu-id="4fa08-369">If needed, update the `returnURL` and `cancelURL` in the PayPal class (`NVPAPICaller`) in the *PayPalFunctions.cs* file to use the port number of your web application:</span></span>   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

<span data-ttu-id="4fa08-370">现在你添加的代码将与本地 Web 应用程序的预期的端口匹配。</span><span class="sxs-lookup"><span data-stu-id="4fa08-370">Now the code that you added will match the expected port for your local Web application.</span></span> <span data-ttu-id="4fa08-371">PayPal 将能够在本地计算机上返回到正确的 URL。</span><span class="sxs-lookup"><span data-stu-id="4fa08-371">PayPal will be able to return to the correct URL on your local machine.</span></span>

### <a name="add-the-paypal-checkout-button"></a><span data-ttu-id="4fa08-372">添加 PayPal 签出按钮</span><span class="sxs-lookup"><span data-stu-id="4fa08-372">Add the PayPal Checkout Button</span></span>

<span data-ttu-id="4fa08-373">现在，主要 PayPal 功能已添加到示例应用程序，你可以开始添加标记和调用这些函数所需的代码。</span><span class="sxs-lookup"><span data-stu-id="4fa08-373">Now that the primary PayPal functions have been added to the sample application, you can begin adding the markup and code needed to call these functions.</span></span> <span data-ttu-id="4fa08-374">首先，必须添加的用户将看到在购物车页上的签出按钮。</span><span class="sxs-lookup"><span data-stu-id="4fa08-374">First, you must add the checkout button that the user will see on the shopping cart page.</span></span>

1. <span data-ttu-id="4fa08-375">打开*ShoppingCart.aspx*文件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-375">Open the *ShoppingCart.aspx* file.</span></span>
2. <span data-ttu-id="4fa08-376">滚动到文件的底部并查找`<!--Checkout Placeholder -->`注释。</span><span class="sxs-lookup"><span data-stu-id="4fa08-376">Scroll to the bottom of the file and find the `<!--Checkout Placeholder -->` comment.</span></span>
3. <span data-ttu-id="4fa08-377">将与注释`ImageButton`控制，以便标记替换，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4fa08-377">Replace the comment with an `ImageButton` control so that the mark up is replaced as follows:</span></span>  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. <span data-ttu-id="4fa08-378">在*ShoppingCart.aspx.cs*文件，之后`UpdateBtn_Click`该文件末尾附近的事件处理程序添加`CheckOutBtn_Click`事件处理程序：</span><span class="sxs-lookup"><span data-stu-id="4fa08-378">In the *ShoppingCart.aspx.cs* file, after the `UpdateBtn_Click` event handler near the end of the file, add the `CheckOutBtn_Click` event handler:</span></span>  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. <span data-ttu-id="4fa08-379">另外，请在*ShoppingCart.aspx.cs*文件中，添加对引用`CheckoutBtn`，以便新的图像按钮引用，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4fa08-379">Also in the *ShoppingCart.aspx.cs* file, add a reference to the `CheckoutBtn`, so that the new image button is referenced as follows:</span></span>  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. <span data-ttu-id="4fa08-380">将所做的更改保存到同时*ShoppingCart.aspx*文件和*ShoppingCart.aspx.cs*文件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-380">Save your changes to both the *ShoppingCart.aspx* file and the *ShoppingCart.aspx.cs* file.</span></span>
7. <span data-ttu-id="4fa08-381">从菜单中，选择**调试**-&gt;**生成 WingtipToys**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-381">From the menu, select **Debug**-&gt;**Build WingtipToys**.</span></span>  
 <span data-ttu-id="4fa08-382">将重新生成项目与新添加**ImageButton**控件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-382">The project will be rebuilt with the newly added **ImageButton** control.</span></span>

### <a name="send-purchase-details-to-paypal"></a><span data-ttu-id="4fa08-383">向 PayPal 发送采购详细信息</span><span class="sxs-lookup"><span data-stu-id="4fa08-383">Send Purchase Details to PayPal</span></span>

<span data-ttu-id="4fa08-384">当用户单击**签出**购物车页上的按钮 (*ShoppingCart.aspx*)，它们将首先购买过程。</span><span class="sxs-lookup"><span data-stu-id="4fa08-384">When the user clicks the **Checkout** button on the shopping cart page (*ShoppingCart.aspx*), they'll begin the purchase process.</span></span> <span data-ttu-id="4fa08-385">下面的代码调用购买产品所需的第一个 PayPal 函数。</span><span class="sxs-lookup"><span data-stu-id="4fa08-385">The following code calls the first PayPal function needed to purchase products.</span></span>

1. <span data-ttu-id="4fa08-386">从*签出*文件夹中，打开代码隐藏文件命名为*CheckoutStart.aspx.cs*。</span><span class="sxs-lookup"><span data-stu-id="4fa08-386">From the *Checkout* folder, open the code-behind file named *CheckoutStart.aspx.cs*.</span></span>   
 <span data-ttu-id="4fa08-387">请确保打开代码隐藏文件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-387">Be sure to open the code-behind file.</span></span>
2. <span data-ttu-id="4fa08-388">将现有代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="4fa08-388">Replace the existing code with the following:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

<span data-ttu-id="4fa08-389">当应用程序的用户单击**签出**按钮在购物车页上，浏览器将导航到*CheckoutStart.aspx*页。</span><span class="sxs-lookup"><span data-stu-id="4fa08-389">When the user of the application clicks the **Checkout** button on the shopping cart page, the browser will navigate to the *CheckoutStart.aspx* page.</span></span> <span data-ttu-id="4fa08-390">当*CheckoutStart.aspx*页上加载，`ShortcutExpressCheckout`调用方法。</span><span class="sxs-lookup"><span data-stu-id="4fa08-390">When the *CheckoutStart.aspx* page loads, the `ShortcutExpressCheckout` method is called.</span></span> <span data-ttu-id="4fa08-391">此时，用户会传输到 PayPal 测试网站。</span><span class="sxs-lookup"><span data-stu-id="4fa08-391">At this point, the user is transferred to the PayPal testing web site.</span></span> <span data-ttu-id="4fa08-392">在 PayPal 站点上，用户输入他们 PayPal 凭据、 查看采购详细信息、 接受 PayPal 协议和 Wingtip Toys 示例应用程序返回其中`ShortcutExpressCheckout`方法完成。</span><span class="sxs-lookup"><span data-stu-id="4fa08-392">On the PayPal site, the user enters their PayPal credentials, reviews the purchase details, accepts the PayPal agreement and returns to the Wingtip Toys sample application where the `ShortcutExpressCheckout` method completes.</span></span> <span data-ttu-id="4fa08-393">当`ShortcutExpressCheckout`方法完成，则会重定向到用户*CheckoutReview.aspx*中指定的网页`ShortcutExpressCheckout`方法。</span><span class="sxs-lookup"><span data-stu-id="4fa08-393">When the `ShortcutExpressCheckout` method is complete, it will redirect the user to the *CheckoutReview.aspx* page specified in the `ShortcutExpressCheckout` method.</span></span> <span data-ttu-id="4fa08-394">这允许用户查看订单详细信息从 Wingtip Toys 示例应用程序中。</span><span class="sxs-lookup"><span data-stu-id="4fa08-394">This allows the user to review the order details from within the Wingtip Toys sample application.</span></span>

### <a name="review-order-details"></a><span data-ttu-id="4fa08-395">查看顺序的详细信息</span><span class="sxs-lookup"><span data-stu-id="4fa08-395">Review Order Details</span></span>

<span data-ttu-id="4fa08-396">后从 PayPal，返回*CheckoutReview.aspx* Wingtip Toys 示例应用程序页显示订单详细信息。</span><span class="sxs-lookup"><span data-stu-id="4fa08-396">After returning from PayPal, the *CheckoutReview.aspx* page of the Wingtip Toys sample application displays the order details.</span></span> <span data-ttu-id="4fa08-397">此页面允许用户在购买产品之前查看订单详细信息。</span><span class="sxs-lookup"><span data-stu-id="4fa08-397">This page allows the user to review the order details before purchasing the products.</span></span> <span data-ttu-id="4fa08-398">*CheckoutReview.aspx*必须创建页，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4fa08-398">The *CheckoutReview.aspx* page must be created as follows:</span></span>

1. <span data-ttu-id="4fa08-399">在*签出*文件夹中，打开页上名为*CheckoutReview.aspx*。</span><span class="sxs-lookup"><span data-stu-id="4fa08-399">In the *Checkout* folder, open the page named *CheckoutReview.aspx*.</span></span>
2. <span data-ttu-id="4fa08-400">现有的标记替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="4fa08-400">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. <span data-ttu-id="4fa08-401">打开名为的代码隐藏页*CheckoutReview.aspx.cs*和将现有代码替换为以下：</span><span class="sxs-lookup"><span data-stu-id="4fa08-401">Open the code-behind page named *CheckoutReview.aspx.cs* and replace the existing code with the following:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

<span data-ttu-id="4fa08-402">**说明**控件用于显示从 PayPal 返回了订单详细信息。</span><span class="sxs-lookup"><span data-stu-id="4fa08-402">The **DetailsView** control is used to display the order details that have been returned from PayPal.</span></span> <span data-ttu-id="4fa08-403">此外，上面的代码将订单详细信息保存到 Wingtip Toys 数据库作为`OrderDetail`对象。</span><span class="sxs-lookup"><span data-stu-id="4fa08-403">Also, the above code saves the order details to the Wingtip Toys database as an `OrderDetail` object.</span></span> <span data-ttu-id="4fa08-404">当用户单击**完整 Order**按钮，将他们重定向到*CheckoutComplete.aspx*页。</span><span class="sxs-lookup"><span data-stu-id="4fa08-404">When the user clicks on the **Complete Order** button, they are redirected to the *CheckoutComplete.aspx* page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="4fa08-405">**提示**</span><span class="sxs-lookup"><span data-stu-id="4fa08-405">**Tip**</span></span>
> 
> <span data-ttu-id="4fa08-406">中的标记*CheckoutReview.aspx*页上，请注意，`<ItemStyle>`标记用于更改中的项的样式**说明**页面底部附近的控件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-406">In the markup of the *CheckoutReview.aspx* page, notice that the `<ItemStyle>` tag is used to change the style of the items within the **DetailsView** control near the bottom of the page.</span></span> <span data-ttu-id="4fa08-407">通过查看中的页**设计视图**(通过选择**设计**在 Visual Studio 的左下角)，然后选择**说明**控制，并选择**智能标记**(顶部的箭头图标右侧的控件)，你将能够看到**说明任务**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-407">By viewing the page in **Design View** (by selecting **Design** at the lower left corner of Visual Studio), then selecting the **DetailsView** control, and selecting the **Smart Tag** (the arrow icon at the top right of the control), you will be able to see the **DetailsView Tasks**.</span></span>
> 
> ![签出和付款与 PayPal-编辑字段](checkout-and-payment-with-paypal/_static/image18.png)
> 
> <span data-ttu-id="4fa08-409">通过选择**编辑字段**、**字段**对话框将出现。</span><span class="sxs-lookup"><span data-stu-id="4fa08-409">By selecting **Edit Fields**, the **Fields** dialog box will appear.</span></span> <span data-ttu-id="4fa08-410">在此对话框中您可以轻松地控制可视属性，如**ItemStyle**的**说明**控件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-410">In this dialog box you can easily control the visual properties, such as **ItemStyle**, of the **DetailsView** control.</span></span>
> 
> ![签出和付款与 PayPal-字段对话框](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a><span data-ttu-id="4fa08-412">完成购买</span><span class="sxs-lookup"><span data-stu-id="4fa08-412">Complete Purchase</span></span>

<span data-ttu-id="4fa08-413">*CheckoutComplete.aspx*页从 PayPal 进行购买。</span><span class="sxs-lookup"><span data-stu-id="4fa08-413">*CheckoutComplete.aspx* page makes the purchase from PayPal.</span></span> <span data-ttu-id="4fa08-414">如上所述，用户必须单击**完整 Order**按钮之前应用程序将会定位到*CheckoutComplete.aspx*页。</span><span class="sxs-lookup"><span data-stu-id="4fa08-414">As mentioned above, the user must click on the **Complete Order** button before the application will navigate to the *CheckoutComplete.aspx* page.</span></span>

1. <span data-ttu-id="4fa08-415">在*签出*文件夹中，打开页上名为*CheckoutComplete.aspx*。</span><span class="sxs-lookup"><span data-stu-id="4fa08-415">In the *Checkout* folder, open the page named *CheckoutComplete.aspx*.</span></span>
2. <span data-ttu-id="4fa08-416">现有的标记替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="4fa08-416">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. <span data-ttu-id="4fa08-417">打开名为的代码隐藏页*CheckoutComplete.aspx.cs*和将现有代码替换为以下：</span><span class="sxs-lookup"><span data-stu-id="4fa08-417">Open the code-behind page named *CheckoutComplete.aspx.cs* and replace the existing code with the following:</span></span>   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

<span data-ttu-id="4fa08-418">当*CheckoutComplete.aspx*加载页时，`DoCheckoutPayment`调用方法。</span><span class="sxs-lookup"><span data-stu-id="4fa08-418">When the *CheckoutComplete.aspx* page is loaded, the `DoCheckoutPayment` method is called.</span></span> <span data-ttu-id="4fa08-419">如前所述，`DoCheckoutPayment`方法完成从 PayPal 测试环境购买。</span><span class="sxs-lookup"><span data-stu-id="4fa08-419">As mentioned earlier, the `DoCheckoutPayment` method completes the purchase from the PayPal testing environment.</span></span> <span data-ttu-id="4fa08-420">PayPal 完成后的顺序，购买*CheckoutComplete.aspx*页显示付款事务`ID`到购买者。</span><span class="sxs-lookup"><span data-stu-id="4fa08-420">Once PayPal has completed the purchase of the order, the *CheckoutComplete.aspx* page displays a payment transaction `ID` to the purchaser.</span></span>

### <a name="handle-cancel-purchase"></a><span data-ttu-id="4fa08-421">处理取消购买</span><span class="sxs-lookup"><span data-stu-id="4fa08-421">Handle Cancel Purchase</span></span>

<span data-ttu-id="4fa08-422">如果用户决定取消购买，它们将定向到*CheckoutCancel.aspx*页上，它们将看到已取消其顺序。</span><span class="sxs-lookup"><span data-stu-id="4fa08-422">If the user decides to cancel the purchase, they will be directed to the *CheckoutCancel.aspx* page where they will see that their order has been cancelled.</span></span>

1. <span data-ttu-id="4fa08-423">打开名为的页*CheckoutCancel.aspx*中*签出*文件夹。</span><span class="sxs-lookup"><span data-stu-id="4fa08-423">Open the page named *CheckoutCancel.aspx* in the *Checkout* folder.</span></span>
2. <span data-ttu-id="4fa08-424">现有的标记替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="4fa08-424">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a><span data-ttu-id="4fa08-425">处理购买错误</span><span class="sxs-lookup"><span data-stu-id="4fa08-425">Handle Purchase Errors</span></span>

<span data-ttu-id="4fa08-426">在购买过程中的错误将由*CheckoutError.aspx*页。</span><span class="sxs-lookup"><span data-stu-id="4fa08-426">Errors during the purchase process will be handled by the *CheckoutError.aspx* page.</span></span> <span data-ttu-id="4fa08-427">代码隐藏*CheckoutStart.aspx*页上， *CheckoutReview.aspx*页上，与*CheckoutComplete.aspx*页面将每个重定向到*CheckoutError.aspx*页上，如果发生错误。</span><span class="sxs-lookup"><span data-stu-id="4fa08-427">The code-behind of the *CheckoutStart.aspx* page, the *CheckoutReview.aspx* page, and the *CheckoutComplete.aspx* page will each redirect to the *CheckoutError.aspx* page if an error occurs.</span></span>

1. <span data-ttu-id="4fa08-428">打开名为的页*CheckoutError.aspx*中*签出*文件夹。</span><span class="sxs-lookup"><span data-stu-id="4fa08-428">Open the page named *CheckoutError.aspx* in the *Checkout* folder.</span></span>
2. <span data-ttu-id="4fa08-429">现有的标记替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="4fa08-429">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

<span data-ttu-id="4fa08-430">*CheckoutError.aspx*在结帐过程中发生错误时，页将显示的错误详细信息。</span><span class="sxs-lookup"><span data-stu-id="4fa08-430">The *CheckoutError.aspx* page is displayed with the error details when an error occurs during the checkout process.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="4fa08-431">运行应用程序</span><span class="sxs-lookup"><span data-stu-id="4fa08-431">Running the Application</span></span>

<span data-ttu-id="4fa08-432">运行应用程序以了解如何购买产品。</span><span class="sxs-lookup"><span data-stu-id="4fa08-432">Run the application to see how to purchase products.</span></span> <span data-ttu-id="4fa08-433">请注意，你会在中运行 PayPal 测试环境。</span><span class="sxs-lookup"><span data-stu-id="4fa08-433">Note that you will be running in the PayPal testing environment.</span></span> <span data-ttu-id="4fa08-434">要不交换的任何实际的资金。</span><span class="sxs-lookup"><span data-stu-id="4fa08-434">No actual money is being exchanged.</span></span>

1. <span data-ttu-id="4fa08-435">请确保所有在 Visual Studio 中保存你的文件。</span><span class="sxs-lookup"><span data-stu-id="4fa08-435">Make sure all your files are saved in Visual Studio.</span></span>
2. <span data-ttu-id="4fa08-436">打开 Web 浏览器并导航到[https://developer.paypal.com](https://developer.paypal.com/)。</span><span class="sxs-lookup"><span data-stu-id="4fa08-436">Open a Web browser and navigate to [https://developer.paypal.com](https://developer.paypal.com/).</span></span>
3. <span data-ttu-id="4fa08-437">使用你此前在本教程中创建的 PayPal 开发人员帐户登录。</span><span class="sxs-lookup"><span data-stu-id="4fa08-437">Login with your PayPal developer account that you created earlier in this tutorial.</span></span>  
 <span data-ttu-id="4fa08-438">对于 PayPal 的开发人员沙盒，你需要在登录[https://developer.paypal.com](https://developer.paypal.com/)测试 express 签出。</span><span class="sxs-lookup"><span data-stu-id="4fa08-438">For PayPal's developer sandbox, you need to be logged in at [https://developer.paypal.com](https://developer.paypal.com/) to test express checkout.</span></span> <span data-ttu-id="4fa08-439">这仅适用于测试，不适用于 PayPal 的实时环境 PayPal 的沙盒。</span><span class="sxs-lookup"><span data-stu-id="4fa08-439">This only applies to PayPal's sandbox testing, not to PayPal's live environment.</span></span>
4. <span data-ttu-id="4fa08-440">在 Visual Studio 中，按**F5**运行 Wingtip Toys 示例应用程序。</span><span class="sxs-lookup"><span data-stu-id="4fa08-440">In Visual Studio, press **F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="4fa08-441">浏览器将重新生成数据库后，将打开并显示*Default.aspx*页。</span><span class="sxs-lookup"><span data-stu-id="4fa08-441">After the database rebuilds, the browser will open and show the *Default.aspx* page.</span></span>
5. <span data-ttu-id="4fa08-442">将三个不同的产品添加到购物车中，通过选择产品类别，如"汽车"，然后单击**将添加到购物车**旁边每个产品。</span><span class="sxs-lookup"><span data-stu-id="4fa08-442">Add three different products to the shopping cart by selecting the product category, such as "Cars" and then clicking **Add to Cart** next to each product.</span></span>  
 <span data-ttu-id="4fa08-443">购物车将显示所选的产品。</span><span class="sxs-lookup"><span data-stu-id="4fa08-443">The shopping cart will display the product you have selected.</span></span>
6. <span data-ttu-id="4fa08-444">单击**PayPal**签出的按钮。</span><span class="sxs-lookup"><span data-stu-id="4fa08-444">Click the **PayPal** button to checkout.</span></span> 

    ![签出和付款与 PayPal-购物车](checkout-and-payment-with-paypal/_static/image20.png)

 <span data-ttu-id="4fa08-446">签出将要求你具有用于 Wingtip Toys 示例应用程序的用户帐户。</span><span class="sxs-lookup"><span data-stu-id="4fa08-446">Checking out will require that you have a user account for the Wingtip Toys sample application.</span></span>
7. <span data-ttu-id="4fa08-447">单击**Google**在能够使用现有 gmail.com 电子邮件帐户进行登录页的右侧的链接。</span><span class="sxs-lookup"><span data-stu-id="4fa08-447">Click the **Google** link on the right of the page to log in with an existing gmail.com email account.</span></span>  
 <span data-ttu-id="4fa08-448">如果没有 gmail.com 帐户，则可以创建一个用于测试目的在[www.gmail.com](https://www.gmail.com/)。此外可以通过单击"注册"使用标准的本地帐户。</span><span class="sxs-lookup"><span data-stu-id="4fa08-448">If you do not have a gmail.com account, you can create one for testing purposes at [www.gmail.com](https://www.gmail.com/). You can also use a standard local account by clicking "Register".</span></span> 

    ![签出和付款与 PayPal-登录](checkout-and-payment-with-paypal/_static/image21.png)
8. <span data-ttu-id="4fa08-450">使用你 gmail 帐户和密码登录。</span><span class="sxs-lookup"><span data-stu-id="4fa08-450">Sign in with your gmail account and password.</span></span> 

    ![签出和付款与 PayPal-Gmail 登录](checkout-and-payment-with-paypal/_static/image22.png)
9. <span data-ttu-id="4fa08-452">单击**登录**按钮以将你的 gmail 帐户注册你 Wingtip Toys 示例应用程序的用户名。</span><span class="sxs-lookup"><span data-stu-id="4fa08-452">Click the **Log in** button to register your gmail account with your Wingtip Toys sample application user name.</span></span> 

    ![签出和付款与 PayPal-注册帐户](checkout-and-payment-with-paypal/_static/image23.png)
10. <span data-ttu-id="4fa08-454">在 PayPal 测试站点上，添加你**buyer**电子邮件地址以及你在本教程中，前面创建的密码，然后单击**Log In**按钮。</span><span class="sxs-lookup"><span data-stu-id="4fa08-454">On the PayPal test site, add your **buyer** email address and password that you created earlier in this tutorial, then click the **Log In** button.</span></span> 

    ![签出和付款与 PayPal-PayPal 登录](checkout-and-payment-with-paypal/_static/image24.png)
11. <span data-ttu-id="4fa08-456">同意 PayPal 策略并单击**同意并继续**按钮。</span><span class="sxs-lookup"><span data-stu-id="4fa08-456">Agree to the PayPal policy and click the **Agree and Continue** button.</span></span>  
 <span data-ttu-id="4fa08-457">请注意，此页才显示第一次使用此 PayPal 帐户。</span><span class="sxs-lookup"><span data-stu-id="4fa08-457">Note that this page is only displayed the first time you use this PayPal account.</span></span> <span data-ttu-id="4fa08-458">再次请注意，这是一个测试帐户，没有真正的现金进行交换。</span><span class="sxs-lookup"><span data-stu-id="4fa08-458">Again note that this is a test account, no real money is exchanged.</span></span> 

    ![签出和付款与 PayPal-PayPal 策略](checkout-and-payment-with-paypal/_static/image25.png)
12. <span data-ttu-id="4fa08-460">查看在测试环境审阅页面上，单击 PayPal 订单信息**继续**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-460">Review the order information on the PayPal testing environment review page and click **Continue**.</span></span> 

    ![签出和付款与 PayPal-查看信息](checkout-and-payment-with-paypal/_static/image26.png)
13. <span data-ttu-id="4fa08-462">上*CheckoutReview.aspx*页上，验证订单量并查看生成的邮寄地址。</span><span class="sxs-lookup"><span data-stu-id="4fa08-462">On the *CheckoutReview.aspx* page, verify the order amount and view the generated shipping address.</span></span> <span data-ttu-id="4fa08-463">然后，单击**完整 Order**按钮。</span><span class="sxs-lookup"><span data-stu-id="4fa08-463">Then, click the **Complete Order** button.</span></span> 

    ![签出和付款与 PayPal-订单审核](checkout-and-payment-with-paypal/_static/image27.png)
14. <span data-ttu-id="4fa08-465">**CheckoutComplete.aspx**页显示带有付款事务 id。</span><span class="sxs-lookup"><span data-stu-id="4fa08-465">The **CheckoutComplete.aspx** page is displayed with a payment transaction ID.</span></span> 

    ![签出和与 PayPal-签出完整的付款](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a><span data-ttu-id="4fa08-467">查看数据库</span><span class="sxs-lookup"><span data-stu-id="4fa08-467">Reviewing the Database</span></span>

<span data-ttu-id="4fa08-468">通过运行应用程序后查看 Wingtip Toys 示例应用程序数据库中更新后的数据，你可以看到应用程序成功记录购买产品。</span><span class="sxs-lookup"><span data-stu-id="4fa08-468">By reviewing the updated data in the Wingtip Toys sample application database after running the application, you can see that the application successfully recorded the purchase of the products.</span></span>

<span data-ttu-id="4fa08-469">你可以检查中包含的数据*Wingtiptoys.mdf*数据库文件使用**数据库资源管理器**窗口 (**服务器资源管理器**Visual Studio 窗口中的) 就像之前在本教程系列。</span><span class="sxs-lookup"><span data-stu-id="4fa08-469">You can inspect the data contained in the *Wingtiptoys.mdf* database file by using the **Database Explorer** window (**Server Explorer** window in Visual Studio) as you did earlier in this tutorial series.</span></span>

1. <span data-ttu-id="4fa08-470">如果它仍处于打开状态，请关闭浏览器窗口。</span><span class="sxs-lookup"><span data-stu-id="4fa08-470">Close the browser window if it is still open.</span></span>
2. <span data-ttu-id="4fa08-471">在 Visual Studio 中，选择**显示所有文件**顶部的图标**解决方案资源管理器**以便您可以展开**应用\_数据**文件夹。</span><span class="sxs-lookup"><span data-stu-id="4fa08-471">In Visual Studio, select the **Show All Files** icon at the top of **Solution Explorer** to allow you to expand the **App\_Data** folder.</span></span>
3. <span data-ttu-id="4fa08-472">展开**应用\_数据**文件夹。</span><span class="sxs-lookup"><span data-stu-id="4fa08-472">Expand the **App\_Data** folder.</span></span>  
 <span data-ttu-id="4fa08-473">你可能需要选择**显示所有文件**文件夹图标。</span><span class="sxs-lookup"><span data-stu-id="4fa08-473">You may need to select the **Show All Files** icon for the folder.</span></span>
4. <span data-ttu-id="4fa08-474">右键单击*Wingtiptoys.mdf*数据库文件并选择**打开**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-474">Right-click the *Wingtiptoys.mdf* database file and select **Open**.</span></span>  
    <span data-ttu-id="4fa08-475">**服务器资源管理器**显示。</span><span class="sxs-lookup"><span data-stu-id="4fa08-475">**Server Explorer** is displayed.</span></span>
5. <span data-ttu-id="4fa08-476">展开**表**文件夹。</span><span class="sxs-lookup"><span data-stu-id="4fa08-476">Expand the **Tables** folder.</span></span>
6. <span data-ttu-id="4fa08-477">右键单击**订单**表，然后选择**显示表数据**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-477">Right-click the **Orders**table and select **Show Table Data**.</span></span>  
 <span data-ttu-id="4fa08-478">**订单**显示表。</span><span class="sxs-lookup"><span data-stu-id="4fa08-478">The **Orders** table is displayed.</span></span>
7. <span data-ttu-id="4fa08-479">查看**PaymentTransactionID**列以确认成功的事务。</span><span class="sxs-lookup"><span data-stu-id="4fa08-479">Review the **PaymentTransactionID** column to confirm successful transactions.</span></span> 

    ![签出和 PayPal-查看数据库使用的付款](checkout-and-payment-with-paypal/_static/image29.png)
8. <span data-ttu-id="4fa08-481">关闭**订单**表窗口。</span><span class="sxs-lookup"><span data-stu-id="4fa08-481">Close the **Orders** table window.</span></span>
9. <span data-ttu-id="4fa08-482">在服务器资源管理器，右键单击**OrderDetails**表，然后选择**显示表数据**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-482">In the Server Explorer, right-click the **OrderDetails** table and select **Show Table Data**.</span></span>
10. <span data-ttu-id="4fa08-483">查看`OrderId`和`Username`中值**OrderDetails**表。</span><span class="sxs-lookup"><span data-stu-id="4fa08-483">Review the `OrderId` and `Username` values in the **OrderDetails** table.</span></span> <span data-ttu-id="4fa08-484">请注意，这些值匹配`OrderId`和`Username`中包含的值**订单**表。</span><span class="sxs-lookup"><span data-stu-id="4fa08-484">Note that these values match the `OrderId` and `Username` values included in the **Orders** table.</span></span>
11. <span data-ttu-id="4fa08-485">关闭**OrderDetails**表窗口。</span><span class="sxs-lookup"><span data-stu-id="4fa08-485">Close the **OrderDetails** table window.</span></span>
12. <span data-ttu-id="4fa08-486">右键单击 Wingtip Toys 数据库文件 (*Wingtiptoys.mdf*)，然后选择**关闭连接**。</span><span class="sxs-lookup"><span data-stu-id="4fa08-486">Right-click the Wingtip Toys database file (*Wingtiptoys.mdf*) and select **Close Connection**.</span></span>
13. <span data-ttu-id="4fa08-487">如果看不到**解决方案资源管理器**窗口中，单击**解决方案资源管理器**底部**服务器资源管理器**窗口以显示**解决方案资源管理器**试。</span><span class="sxs-lookup"><span data-stu-id="4fa08-487">If you do not see the **Solution Explorer** window, click **Solution Explorer** at the bottom of the **Server Explorer** window to show the **Solution Explorer** again.</span></span>

## <a name="summary"></a><span data-ttu-id="4fa08-488">摘要</span><span class="sxs-lookup"><span data-stu-id="4fa08-488">Summary</span></span>

<span data-ttu-id="4fa08-489">在本教程中你添加了订单和订单详细信息架构跟踪购买产品。</span><span class="sxs-lookup"><span data-stu-id="4fa08-489">In this tutorial you added order and order detail schemas to track the purchase of products.</span></span> <span data-ttu-id="4fa08-490">你还可以集成到 Wingtip Toys 示例应用程序 PayPal 功能。</span><span class="sxs-lookup"><span data-stu-id="4fa08-490">You also integrated PayPal functionality into the Wingtip Toys sample application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4fa08-491">其他资源</span><span class="sxs-lookup"><span data-stu-id="4fa08-491">Additional Resources</span></span>

<span data-ttu-id="4fa08-492">[ASP.NET 配置概述](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="4fa08-492">[ASP.NET Configuration Overview](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)</span></span>  
[<span data-ttu-id="4fa08-493">将包含成员资格、 OAuth 和 SQL 数据库的安全的 ASP.NET Web 窗体应用程序部署到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4fa08-493">Deploy a Secure ASP.NET Web Forms App with Membership, OAuth, and SQL Database to Azure App Service</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[<span data-ttu-id="4fa08-494">Microsoft Azure 的免费试用版</span><span class="sxs-lookup"><span data-stu-id="4fa08-494">Microsoft Azure - Free Trial</span></span>](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a><span data-ttu-id="4fa08-495">免责声明</span><span class="sxs-lookup"><span data-stu-id="4fa08-495">Disclaimer</span></span>

<span data-ttu-id="4fa08-496">本教程包含示例代码。</span><span class="sxs-lookup"><span data-stu-id="4fa08-496">This tutorial contains sample code.</span></span> <span data-ttu-id="4fa08-497">此类示例代码"按原样"提供，而无需任何形式的保证。</span><span class="sxs-lookup"><span data-stu-id="4fa08-497">Such sample code is provided "as is" without warranty of any kind.</span></span> <span data-ttu-id="4fa08-498">相应地，Microsoft 不保证准确性、 完整性或对示例代码质量。</span><span class="sxs-lookup"><span data-stu-id="4fa08-498">Accordingly, Microsoft does not guarantee the accuracy, integrity, or quality of the sample code.</span></span> <span data-ttu-id="4fa08-499">您同意在你自己的风险时使用的示例代码。</span><span class="sxs-lookup"><span data-stu-id="4fa08-499">You agree to use the sample code at your own risk.</span></span> <span data-ttu-id="4fa08-500">在任何情况下将 Microsoft 的应承担给你以任何方式内容，包括但不是限于任何错误或遗漏的任何示例代码、 内容，或任何丢失或损坏导致的任何示例代码使用产生任何任何的类型示例代码。</span><span class="sxs-lookup"><span data-stu-id="4fa08-500">Under no circumstances will Microsoft be liable to you in any way for any sample code, content, including but not limited to, any errors or omissions in any sample code, content, or any loss or damage of any kind incurred as a result of the use of any sample code.</span></span> <span data-ttu-id="4fa08-501">特此收到通知，特此同意赔偿，保存并保存 Microsoft 无害从和针对所有损失、 丢失、 受伤或的任何类型的包括但不限于，那些由 occasioned 或因你发布的材料的损坏的声明传输、 使用或依赖于包括但不是限于表示其中的视图。</span><span class="sxs-lookup"><span data-stu-id="4fa08-501">You are hereby notified and do hereby agree to indemnify, save and hold Microsoft harmless from and against any and all loss, claims of loss, injury or damage of any kind including, without limitation, those occasioned by or arising from material that you post, transmit, use or rely on including, but not limited to, the views expressed therein.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4fa08-502">[上一页](shopping-cart.md)
[下一页](membership-and-administration.md)</span><span class="sxs-lookup"><span data-stu-id="4fa08-502">[Previous](shopping-cart.md)
[Next](membership-and-administration.md)</span></span>
