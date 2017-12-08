---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: "OWIN 启动类检测 |Microsoft 文档"
author: Praburaj
description: "本教程演示如何配置加载哪些 OWIN startup 类。 有关 OWIN 的详细信息，请参阅项目 Katana 概述。 本教程已..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: a6ac34307b7558ad13684448f339ca74ade9e997
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="80eb9-105">OWIN 启动类检测</span><span class="sxs-lookup"><span data-stu-id="80eb9-105">OWIN Startup Class Detection</span></span>
====================
<span data-ttu-id="80eb9-106">通过[Praburaj Thiagarajan](https://github.com/Praburaj)， [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="80eb9-106">by [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="80eb9-107">本教程演示如何配置加载哪些 OWIN startup 类。</span><span class="sxs-lookup"><span data-stu-id="80eb9-107">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="80eb9-108">有关 OWIN 的详细信息，请参阅[项目概述 Katana](an-overview-of-project-katana.md)。</span><span class="sxs-lookup"><span data-stu-id="80eb9-108">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="80eb9-109">本教程编写由 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )，Praburaj Thiagarajan 和 Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) )。</span><span class="sxs-lookup"><span data-stu-id="80eb9-109">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
> 
> ## <a name="prerequisites"></a><span data-ttu-id="80eb9-110">先决条件</span><span class="sxs-lookup"><span data-stu-id="80eb9-110">Prerequisites</span></span>
> 
> [<span data-ttu-id="80eb9-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="80eb9-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="80eb9-112">OWIN 启动类检测</span><span class="sxs-lookup"><span data-stu-id="80eb9-112">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="80eb9-113">每个 OWIN 应用程序具有一个 startup 类，其中您可以指定应用程序管道的组件。</span><span class="sxs-lookup"><span data-stu-id="80eb9-113">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="80eb9-114">有不同的方式可以与运行时连接 startup 类，具体取决于宿主模型选择 （OwinHost、 IIS 和 IIS Express）。</span><span class="sxs-lookup"><span data-stu-id="80eb9-114">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="80eb9-115">在本教程中所示的启动类可在每个托管的应用程序。</span><span class="sxs-lookup"><span data-stu-id="80eb9-115">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="80eb9-116">与托管运行时使用下列任一方法连接 startup 类：</span><span class="sxs-lookup"><span data-stu-id="80eb9-116">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>  

1. <span data-ttu-id="80eb9-117">**命名约定**: Katana 查找名为的类`Startup`匹配的程序集名称或全局命名空间的命名空间中。</span><span class="sxs-lookup"><span data-stu-id="80eb9-117">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="80eb9-118">**OwinStartup 属性**： 这是大多数开发人员需要指定启动类的方法。</span><span class="sxs-lookup"><span data-stu-id="80eb9-118">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="80eb9-119">以下属性将设置为 startup 类`TestStartup`类`StartupDemo`命名空间。</span><span class="sxs-lookup"><span data-stu-id="80eb9-119">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span> 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

 <span data-ttu-id="80eb9-120">`OwinStartup`特性将重写的命名约定。</span><span class="sxs-lookup"><span data-stu-id="80eb9-120">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="80eb9-121">也可以指定一个友好名称，使用此属性，但是，使用的友好名称要求你还使用`appSetting`配置文件中的元素。</span><span class="sxs-lookup"><span data-stu-id="80eb9-121">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="80eb9-122">**配置文件中的 appSetting 元素**:`appSetting`元素会替代`OwinStartup`属性和命名约定。</span><span class="sxs-lookup"><span data-stu-id="80eb9-122">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="80eb9-123">你可以有多个启动类 (每个使用`OwinStartup`属性) 和配置将使用类似于以下的标记的配置文件中加载哪些 startup 类：</span><span class="sxs-lookup"><span data-stu-id="80eb9-123">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

 <span data-ttu-id="80eb9-124">此外可以使用显式指定的启动类和程序集的以下项：</span><span class="sxs-lookup"><span data-stu-id="80eb9-124">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

 <span data-ttu-id="80eb9-125">下面的 XML 配置文件中指定的友好的启动类名称`ProductionConfiguration`。</span><span class="sxs-lookup"><span data-stu-id="80eb9-125">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

 <span data-ttu-id="80eb9-126">上面的标记都必须使用以下`OwinStartup`属性指定的友好名称，并导致`ProductionStartup2`类运行。</span><span class="sxs-lookup"><span data-stu-id="80eb9-126">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="80eb9-127">若要禁用 OWIN 启动发现，请添加`appSetting owin:AutomaticAppStartup`值为`"false"`web.config 文件中。</span><span class="sxs-lookup"><span data-stu-id="80eb9-127">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="80eb9-128">创建 ASP.NET Web 应用使用 OWIN 启动</span><span class="sxs-lookup"><span data-stu-id="80eb9-128">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="80eb9-129">创建空的 Asp.Net web 应用程序并将其命名**StartupDemo**。</span><span class="sxs-lookup"><span data-stu-id="80eb9-129">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="80eb9-130">-安装`Microsoft.Owin.Host.SystemWeb`使用 NuGet 包管理器。</span><span class="sxs-lookup"><span data-stu-id="80eb9-130">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="80eb9-131">从**工具**菜单上，选择**库程序包管理器**，，然后**程序包管理器控制台**。</span><span class="sxs-lookup"><span data-stu-id="80eb9-131">From the **Tools** menu, select **Library Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="80eb9-132">输入以下命令：</span><span class="sxs-lookup"><span data-stu-id="80eb9-132">Enter the following command:</span></span>  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
- <span data-ttu-id="80eb9-133">添加的 OWIN 启动类。</span><span class="sxs-lookup"><span data-stu-id="80eb9-133">Add an OWIN startup class.</span></span> <span data-ttu-id="80eb9-134">在 Visual Studio 2013 中右键单击项目，然后选择**添加类**。-在**添加新项**对话框框中，输入*OWIN*在搜索字段和更改该名称与 Startup.cs，然后单击**添加**。</span><span class="sxs-lookup"><span data-stu-id="80eb9-134">In Visual Studio 2013 right click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then click **Add**.</span></span>  
  
    ![](owin-startup-class-detection/_static/image1.png)   
  
 <span data-ttu-id="80eb9-135">你想要添加的下一步时间*Owin 启动类*，则将为可从**添加**菜单。</span><span class="sxs-lookup"><span data-stu-id="80eb9-135">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>  
   
    ![](owin-startup-class-detection/_static/image2.png)  
  
 <span data-ttu-id="80eb9-136">或者，你可以右键单击项目并选择**添加**，然后选择**新项**，然后选择**Owin 启动类**。</span><span class="sxs-lookup"><span data-stu-id="80eb9-136">Alternatively, you can right click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>  
  
    ![](owin-startup-class-detection/_static/image3.png)  
  
- <span data-ttu-id="80eb9-137">中的生成的代码替换*Startup.cs*替换为以下文件：</span><span class="sxs-lookup"><span data-stu-id="80eb9-137">Replace the generated code in the *Startup.cs* file with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
 <span data-ttu-id="80eb9-138">`app.Use` Lambda 表达式用于注册指定的中间件组件向 OWIN 管道。</span><span class="sxs-lookup"><span data-stu-id="80eb9-138">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="80eb9-139">在这种情况下，我们要设置的传入请求的响应传入的请求之前的日志记录。</span><span class="sxs-lookup"><span data-stu-id="80eb9-139">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="80eb9-140">`next`参数为的委托 ( [Func](https://msdn.microsoft.com/en-us/library/bb534960(v=vs.100).aspx) &lt; [任务](https://msdn.microsoft.com/en-us/library/dd321424(v=vs.100).aspx) &gt; ) 到管道中的下一个组件。</span><span class="sxs-lookup"><span data-stu-id="80eb9-140">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/en-us/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/en-us/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="80eb9-141">`app.Run` Lambda 表达式挂钩到传入的请求管线，并提供响应机制。</span><span class="sxs-lookup"><span data-stu-id="80eb9-141">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="80eb9-142">在上面的代码情况下，我们已注释掉`OwinStartup`属性，我们要依赖于正在运行名为的类的约定`Startup`。-按***F5***运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="80eb9-142">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="80eb9-143">命中刷新几次。</span><span class="sxs-lookup"><span data-stu-id="80eb9-143">Hit refresh a few times.</span></span>  
  
    ![](owin-startup-class-detection/_static/image4.png)  
<span data-ttu-id="80eb9-144">注意： 在本教程中的映像显示的数字将不匹配看到的选项数目。</span><span class="sxs-lookup"><span data-stu-id="80eb9-144">Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="80eb9-145">毫秒字符串用于刷新页面时显示新的响应。</span><span class="sxs-lookup"><span data-stu-id="80eb9-145">The millisecond string is used to show a new response when you refresh the page.</span></span>  
 <span data-ttu-id="80eb9-146">你可以看到中的跟踪信息**输出**窗口。</span><span class="sxs-lookup"><span data-stu-id="80eb9-146">You can see the trace information in the **Output** window.</span></span>  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="80eb9-147">添加更多的启动类</span><span class="sxs-lookup"><span data-stu-id="80eb9-147">Add More Startup Classes</span></span>

<span data-ttu-id="80eb9-148">在本部分中，我们将添加另一个 Startup 类。</span><span class="sxs-lookup"><span data-stu-id="80eb9-148">In this section we'll add another Startup class.</span></span> <span data-ttu-id="80eb9-149">你可以将多个 OWIN 启动类添加到你的应用程序。</span><span class="sxs-lookup"><span data-stu-id="80eb9-149">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="80eb9-150">例如，你可能想要创建用于开发、 测试和生产的启动类。</span><span class="sxs-lookup"><span data-stu-id="80eb9-150">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="80eb9-151">创建一个新的 OWIN 启动类并将其命名`ProductionStartup`。</span><span class="sxs-lookup"><span data-stu-id="80eb9-151">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="80eb9-152">将生成的代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="80eb9-152">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="80eb9-153">按控件 F5 运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="80eb9-153">Press Control F5 to run the app.</span></span> <span data-ttu-id="80eb9-154">`OwinStartup`属性指定运行生产 startup 类。</span><span class="sxs-lookup"><span data-stu-id="80eb9-154">The `OwinStartup` attribute specifies the production startup class is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="80eb9-155">创建另一个 OWIN 启动类并将其命名`TestStartup`。</span><span class="sxs-lookup"><span data-stu-id="80eb9-155">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="80eb9-156">将生成的代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="80eb9-156">Replace the generated code with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

 <span data-ttu-id="80eb9-157">`OwinStartup`上面的属性重载指定`TestingConfiguration`作为*友好*Startup 类的名称。</span><span class="sxs-lookup"><span data-stu-id="80eb9-157">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="80eb9-158">打开*web.config*文件并添加指定的启动类的友好名称的 OWIN 应用程序启动密钥：</span><span class="sxs-lookup"><span data-stu-id="80eb9-158">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="80eb9-159">按控件 F5 运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="80eb9-159">Press Control F5 to run the app.</span></span> <span data-ttu-id="80eb9-160">应用程序设置元素采用引用单元格以及测试运行配置。</span><span class="sxs-lookup"><span data-stu-id="80eb9-160">The app settings element takes precedent, and the test configuration is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="80eb9-161">删除*友好*名称从`OwinStartup`属性中`TestStartup`类。</span><span class="sxs-lookup"><span data-stu-id="80eb9-161">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="80eb9-162">替换中的 OWIN 应用程序启动密钥*web.config*替换为以下文件：</span><span class="sxs-lookup"><span data-stu-id="80eb9-162">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="80eb9-163">还原`OwinStartup`在由 Visual Studio 生成的默认属性代码的每个类的属性：</span><span class="sxs-lookup"><span data-stu-id="80eb9-163">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

 <span data-ttu-id="80eb9-164">每个以下的 OWIN 应用程序的启动密钥将导致要运行的生产类。</span><span class="sxs-lookup"><span data-stu-id="80eb9-164">Each of the OWIN App startup keys below will cause the production class to run.</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

 <span data-ttu-id="80eb9-165">最后一个的启动密钥指定的启动配置方法。</span><span class="sxs-lookup"><span data-stu-id="80eb9-165">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="80eb9-166">以下的 OWIN 应用程序启动密钥允许您更改配置类的名称`MyConfiguration`。</span><span class="sxs-lookup"><span data-stu-id="80eb9-166">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="80eb9-167">使用 Owinhost.exe</span><span class="sxs-lookup"><span data-stu-id="80eb9-167">Using Owinhost.exe</span></span>

1. <span data-ttu-id="80eb9-168">Web.config 文件替换为以下标记：</span><span class="sxs-lookup"><span data-stu-id="80eb9-168">Replace the Web.config file with the following markup:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

 <span data-ttu-id="80eb9-169">最后一个键 wins，因而在这种情况下`TestStartup`指定。</span><span class="sxs-lookup"><span data-stu-id="80eb9-169">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="80eb9-170">从 PMC 安装 Owinhost:</span><span class="sxs-lookup"><span data-stu-id="80eb9-170">Install Owinhost from the PMC:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="80eb9-171">导航到应用程序文件夹 (此文件夹包含*Web.config*文件) 并在命令提示符并键入：</span><span class="sxs-lookup"><span data-stu-id="80eb9-171">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

 <span data-ttu-id="80eb9-172">命令窗口将显示：</span><span class="sxs-lookup"><span data-stu-id="80eb9-172">The command window will show:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="80eb9-173">启动浏览器的 url `http://localhost:5000/`。</span><span class="sxs-lookup"><span data-stu-id="80eb9-173">Launch a browser with the URL `http://localhost:5000/`.</span></span>  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
 <span data-ttu-id="80eb9-174">OwinHost 遵循上面列出的启动约定。</span><span class="sxs-lookup"><span data-stu-id="80eb9-174">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="80eb9-175">在命令窗口中，按 Enter 以退出 OwinHost。</span><span class="sxs-lookup"><span data-stu-id="80eb9-175">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="80eb9-176">在`ProductionStartup`类中，添加以下 OwinStartup 属性指定的友好名称的*ProductionConfiguration*。</span><span class="sxs-lookup"><span data-stu-id="80eb9-176">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="80eb9-177">在命令提示符并键入：</span><span class="sxs-lookup"><span data-stu-id="80eb9-177">In the command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

 <span data-ttu-id="80eb9-178">加载生产 startup 类。</span><span class="sxs-lookup"><span data-stu-id="80eb9-178">The Production startup class is loaded.</span></span>  
    ![](owin-startup-class-detection/_static/image9.png)  
 <span data-ttu-id="80eb9-179">我们的应用程序具有多个启动类，并在此示例中我们具有延迟到运行时加载哪些 startup 类。</span><span class="sxs-lookup"><span data-stu-id="80eb9-179">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="80eb9-180">测试以下运行时启动选项：</span><span class="sxs-lookup"><span data-stu-id="80eb9-180">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
