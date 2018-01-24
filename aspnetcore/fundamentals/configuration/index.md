---
title: "ASP.NET Core 中的配置"
author: rick-anderson
description: "使用配置 API 通过多种方法配置 ASP.NET Core 应用。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: c4f57d1e02ad5f4e235039999af9df9d236756a7
ms.sourcegitcommit: 3d512ea991ac36dfd4c800b7d1f8a27bfc50635e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="1a414-103">配置 ASP.NET Core 应用</span><span class="sxs-lookup"><span data-stu-id="1a414-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="1a414-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Mark Michaelis](http://intellitect.com/author/mark-michaelis/)、[Steve Smith](https://ardalis.com/)、[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1a414-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1a414-105">通过配置 API ，可基于名称/值对列表来配置 ASP.NET Core Web 应用。</span><span class="sxs-lookup"><span data-stu-id="1a414-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="1a414-106">在运行时从多个源读取配置。</span><span class="sxs-lookup"><span data-stu-id="1a414-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="1a414-107">可将这些名称/值对分组到多级层次结构。</span><span class="sxs-lookup"><span data-stu-id="1a414-107">You can group these name-value pairs into a multi-level hierarchy.</span></span>

<span data-ttu-id="1a414-108">配置提供程序适用于：</span><span class="sxs-lookup"><span data-stu-id="1a414-108">There are configuration providers for:</span></span>

* <span data-ttu-id="1a414-109">文件格式（INI、JSON 和 XML）</span><span class="sxs-lookup"><span data-stu-id="1a414-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="1a414-110">命令行参数</span><span class="sxs-lookup"><span data-stu-id="1a414-110">Command-line arguments</span></span>
* <span data-ttu-id="1a414-111">环境变量</span><span class="sxs-lookup"><span data-stu-id="1a414-111">Environment variables</span></span>
* <span data-ttu-id="1a414-112">内存中的 .NET 对象</span><span class="sxs-lookup"><span data-stu-id="1a414-112">In-memory .NET objects</span></span>
* <span data-ttu-id="1a414-113">加密的用户存储</span><span class="sxs-lookup"><span data-stu-id="1a414-113">An encrypted user store</span></span>
* [<span data-ttu-id="1a414-114">Azure 密钥保管库</span><span class="sxs-lookup"><span data-stu-id="1a414-114">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="1a414-115">（已安装或已创建的）自定义提供程序</span><span class="sxs-lookup"><span data-stu-id="1a414-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="1a414-116">每个配置值映射到一个字符串键。</span><span class="sxs-lookup"><span data-stu-id="1a414-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="1a414-117">可借助内置绑定支持，将设置反序列化为自定义 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 对象（一种具有属性的简单 .NET 类）。</span><span class="sxs-lookup"><span data-stu-id="1a414-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="1a414-118">选项模式使用选项类来表示相关设置的组。</span><span class="sxs-lookup"><span data-stu-id="1a414-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="1a414-119">有关使用选项模式的详细信息，请参阅[选项](xref:fundamentals/configuration/options)主题。</span><span class="sxs-lookup"><span data-stu-id="1a414-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="1a414-120">[查看或下载示例代码](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="1a414-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="1a414-121">JSON 配置</span><span class="sxs-lookup"><span data-stu-id="1a414-121">JSON configuration</span></span>

<span data-ttu-id="1a414-122">以下控制台应用使用 JSON 配置提供程序：</span><span class="sxs-lookup"><span data-stu-id="1a414-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="1a414-123">该应用将读取和显示下列配置设置：</span><span class="sxs-lookup"><span data-stu-id="1a414-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="1a414-124">配置就是名称/值对的分层列表，其中节点是由冒号分隔。</span><span class="sxs-lookup"><span data-stu-id="1a414-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="1a414-125">要检索某个值，请使用相应项的键访问 `Configuration` 索引器：</span><span class="sxs-lookup"><span data-stu-id="1a414-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="1a414-126">要在 JSON 格式的配置源中使用数组，请在由冒号分隔的字符串中使用数组索引。</span><span class="sxs-lookup"><span data-stu-id="1a414-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="1a414-127">以下示例获取上述 `wizards` 数组中第一个项的名称：</span><span class="sxs-lookup"><span data-stu-id="1a414-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="1a414-128">写入内置[配置](/dotnet/api/microsoft.extensions.configuration)提供程序的名称/值对不是持久的。</span><span class="sxs-lookup"><span data-stu-id="1a414-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="1a414-129">但是，可以创建一个自定义提供程序来保存值。</span><span class="sxs-lookup"><span data-stu-id="1a414-129">However, you can create a custom provider that saves values.</span></span> <span data-ttu-id="1a414-130">请参阅[自定义配置提供程序](xref:fundamentals/configuration/index#custom-config-providers)。</span><span class="sxs-lookup"><span data-stu-id="1a414-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="1a414-131">前面的示例使用配置索引器来读取值。</span><span class="sxs-lookup"><span data-stu-id="1a414-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="1a414-132">要访问 `Startup` 外部的配置，请使用选项模式。</span><span class="sxs-lookup"><span data-stu-id="1a414-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="1a414-133">有关详细信息，请参阅[选项](xref:fundamentals/configuration/options)主题。</span><span class="sxs-lookup"><span data-stu-id="1a414-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>


## <a name="configuration-by-environment"></a><span data-ttu-id="1a414-134">按环境配置</span><span class="sxs-lookup"><span data-stu-id="1a414-134">Configuration by environment</span></span>

<span data-ttu-id="1a414-135">通常而言，配置设置因环境（如开发、测试和生产等）而异。</span><span class="sxs-lookup"><span data-stu-id="1a414-135">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="1a414-136">ASP.NET Core 2.x 应用中的 `CreateDefaultBuilder` 扩展方法（或直接在 ASP.NET Core 1.x 应用中使用 `AddJsonFile` 和 `AddEnvironmentVariables`）添加了用于读取 JSON 文件和系统配置源的配置提供程序：</span><span class="sxs-lookup"><span data-stu-id="1a414-136">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="1a414-137">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="1a414-137">*appsettings.json*</span></span>
* <span data-ttu-id="1a414-138">appsettings.\<EnvironmentName>.json</span><span class="sxs-lookup"><span data-stu-id="1a414-138">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="1a414-139">环境变量</span><span class="sxs-lookup"><span data-stu-id="1a414-139">Environment variables</span></span>

<span data-ttu-id="1a414-140">ASP.NET Core 1.x 应用需要调用 `AddJsonFile` 和 [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_)。</span><span class="sxs-lookup"><span data-stu-id="1a414-140">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="1a414-141">有关参数的说明，请参阅 [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions)。</span><span class="sxs-lookup"><span data-stu-id="1a414-141">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="1a414-142">仅 ASP.NET Core 1.1 及更高版本支持 `reloadOnChange`。</span><span class="sxs-lookup"><span data-stu-id="1a414-142">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="1a414-143">按指定配置源的顺序读取它们。</span><span class="sxs-lookup"><span data-stu-id="1a414-143">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="1a414-144">在前面的代码中，最后才读取环境变量。</span><span class="sxs-lookup"><span data-stu-id="1a414-144">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="1a414-145">在环境中设置的任意配置值将替换先前两个提供程序中设置的配置值。</span><span class="sxs-lookup"><span data-stu-id="1a414-145">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="1a414-146">请考虑使用以下 appsettings.Staging.json 文件：</span><span class="sxs-lookup"><span data-stu-id="1a414-146">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[Main](index/sample/appsettings.Staging.json)]

<span data-ttu-id="1a414-147">环境设置为 `Staging` 时，以下 `Configure` 方法将读取 `MyConfig` 的值：</span><span class="sxs-lookup"><span data-stu-id="1a414-147">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[Main](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="1a414-148">环境通常设置为 `Development`、`Staging` 或 `Production`。</span><span class="sxs-lookup"><span data-stu-id="1a414-148">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="1a414-149">有关详细信息，请参阅[使用多个环境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="1a414-149">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="1a414-150">配置注意事项：</span><span class="sxs-lookup"><span data-stu-id="1a414-150">Configuration considerations:</span></span>

* <span data-ttu-id="1a414-151">配置数据发生更改时，`IOptionsSnapshot` 可将其重载。</span><span class="sxs-lookup"><span data-stu-id="1a414-151">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="1a414-152">有关详细信息，请参阅 [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot)。</span><span class="sxs-lookup"><span data-stu-id="1a414-152">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="1a414-153">配置密钥不区分大小写。</span><span class="sxs-lookup"><span data-stu-id="1a414-153">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="1a414-154">请勿在配置提供程序代码或纯文本配置文件中存储密码或其他敏感数据。</span><span class="sxs-lookup"><span data-stu-id="1a414-154">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="1a414-155">请勿在开发或测试环境中使用生产机密。</span><span class="sxs-lookup"><span data-stu-id="1a414-155">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="1a414-156">请在项目外部指定机密，避免将其意外提交到存储库。</span><span class="sxs-lookup"><span data-stu-id="1a414-156">Specify secrets outside of the project so that they can't be accidentally committed to your repository.</span></span> <span data-ttu-id="1a414-157">详细了解如何[使用多个环境](xref:fundamentals/environments)和[在开发期间管理应用机密的安全存储](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="1a414-157">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="1a414-158">如果系统不支持在环境变量中使用冒号 (`:`)，请将冒号 (`:`) 替换为双下划线 (`__`)。</span><span class="sxs-lookup"><span data-stu-id="1a414-158">If a colon (`:`) can't be used in environment variables on your system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="1a414-159">内存中提供程序及绑定到 POCO 类</span><span class="sxs-lookup"><span data-stu-id="1a414-159">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="1a414-160">以下示例演示如何使用内存中提供程序及绑定到类：</span><span class="sxs-lookup"><span data-stu-id="1a414-160">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="1a414-161">配置值以字符串的形式返回，但绑定使对象的构造成为可能。</span><span class="sxs-lookup"><span data-stu-id="1a414-161">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="1a414-162">绑定允许你检索 POCO 对象或甚至整个对象图。</span><span class="sxs-lookup"><span data-stu-id="1a414-162">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="1a414-163">GetValue</span><span class="sxs-lookup"><span data-stu-id="1a414-163">GetValue</span></span>

<span data-ttu-id="1a414-164">以下示例演示 [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) 扩展方法：</span><span class="sxs-lookup"><span data-stu-id="1a414-164">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="1a414-165">ConfigurationBinder 的 `GetValue<T>` 方法允许指定默认值（在此示例中为 80）。</span><span class="sxs-lookup"><span data-stu-id="1a414-165">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="1a414-166">`GetValue<T>` 适用于简单方案，并不绑定到整个部分。</span><span class="sxs-lookup"><span data-stu-id="1a414-166">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="1a414-167">`GetValue<T>` 将 `GetSection(key).Value` 中的标量值转换为特定类型。</span><span class="sxs-lookup"><span data-stu-id="1a414-167">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="1a414-168">绑定至对象图</span><span class="sxs-lookup"><span data-stu-id="1a414-168">Bind to an object graph</span></span>

<span data-ttu-id="1a414-169">可以在类中递归绑定每个对象。</span><span class="sxs-lookup"><span data-stu-id="1a414-169">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="1a414-170">请考虑使用以下 `AppSettings` 类：</span><span class="sxs-lookup"><span data-stu-id="1a414-170">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="1a414-171">以下示例绑定到 `AppSettings` 类：</span><span class="sxs-lookup"><span data-stu-id="1a414-171">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="1a414-172">ASP.NET Core 1.1 及更高版本可使用 `Get<T>`，它适用于整个部分。</span><span class="sxs-lookup"><span data-stu-id="1a414-172">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="1a414-173">使用 `Get<T>` 可能比 `Bind` 方便。</span><span class="sxs-lookup"><span data-stu-id="1a414-173">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="1a414-174">以下代码演示了如何通过前述示例使用 `Get<T>`：</span><span class="sxs-lookup"><span data-stu-id="1a414-174">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="1a414-175">使用以下 appsettings.json 文件：</span><span class="sxs-lookup"><span data-stu-id="1a414-175">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="1a414-176">该程序显示 `Height 11`。</span><span class="sxs-lookup"><span data-stu-id="1a414-176">The program displays `Height 11`.</span></span>

<span data-ttu-id="1a414-177">可使用以下代码对配置进行单元测试：</span><span class="sxs-lookup"><span data-stu-id="1a414-177">The following code can be used to unit test the configuration:</span></span>

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="1a414-178">创建 Entity Framework 自定义提供程序</span><span class="sxs-lookup"><span data-stu-id="1a414-178">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="1a414-179">本部分将创建一个使用 EF 从数据库读取名称/值对的基本配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="1a414-179">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="1a414-180">定义 `ConfigurationValue` 实体，用于在数据库中存储配置值：</span><span class="sxs-lookup"><span data-stu-id="1a414-180">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="1a414-181">添加 `ConfigurationContext` 以便存储和访问配置值：</span><span class="sxs-lookup"><span data-stu-id="1a414-181">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="1a414-182">创建实现 [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource) 的类：</span><span class="sxs-lookup"><span data-stu-id="1a414-182">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="1a414-183">通过从 [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider) 继承来创建自定义配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="1a414-183">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="1a414-184">当数据库为空时，配置提供程序将对其进行初始化：</span><span class="sxs-lookup"><span data-stu-id="1a414-184">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="1a414-185">运行示例时将显示数据库中突出显示的值（“value_from_ef_1”和“value_from_ef_2”）。</span><span class="sxs-lookup"><span data-stu-id="1a414-185">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="1a414-186">可以添加 `EFConfigSource` 扩展方法，用于添加配置源：</span><span class="sxs-lookup"><span data-stu-id="1a414-186">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="1a414-187">下面的代码演示如何使用自定义 `EFConfigProvider`：</span><span class="sxs-lookup"><span data-stu-id="1a414-187">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="1a414-188">请注意，示例在 JSON 提供程序之后添加自定义 `EFConfigProvider`，因此数据库中的任何设置都将替代 appsettings.json 文件中的设置。</span><span class="sxs-lookup"><span data-stu-id="1a414-188">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="1a414-189">使用以下 appsettings.json 文件：</span><span class="sxs-lookup"><span data-stu-id="1a414-189">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="1a414-190">显示以下输出：</span><span class="sxs-lookup"><span data-stu-id="1a414-190">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="1a414-191">CommandLine 配置提供程序</span><span class="sxs-lookup"><span data-stu-id="1a414-191">CommandLine configuration provider</span></span>

<span data-ttu-id="1a414-192">[CommandLine 配置提供程序](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider)在运行时接收用于配置的命令行参数键值对。</span><span class="sxs-lookup"><span data-stu-id="1a414-192">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="1a414-193">查看或下载命令行配置示例</span><span class="sxs-lookup"><span data-stu-id="1a414-193">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="1a414-194">设置和使用 CommandLine 配置提供程序</span><span class="sxs-lookup"><span data-stu-id="1a414-194">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="1a414-195">基本配置</span><span class="sxs-lookup"><span data-stu-id="1a414-195">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="1a414-196">要激活命令行配置，请在 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) 的实例上调用 `AddCommandLine` 扩展方法：</span><span class="sxs-lookup"><span data-stu-id="1a414-196">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="1a414-197">运行代码将显示以下输出：</span><span class="sxs-lookup"><span data-stu-id="1a414-197">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="1a414-198">在命令行上传递参数键值对将更改 `Profile:MachineName` 和 `App:MainWindow:Left` 的值：</span><span class="sxs-lookup"><span data-stu-id="1a414-198">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="1a414-199">控制台窗口将显示：</span><span class="sxs-lookup"><span data-stu-id="1a414-199">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="1a414-200">要使用命令行配置替代由其他配置提供程序提供的配置，请在 `ConfigurationBuilder` 上最后调用 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="1a414-200">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1a414-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1a414-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1a414-202">典型的 ASP.NET Core 2.x 应用使用静态简便方法 `CreateDefaultBuilder` 生成主机：</span><span class="sxs-lookup"><span data-stu-id="1a414-202">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="1a414-203">`CreateDefaultBuilder` 从 appsettings.json、appsettings.{Environment}.json、[用户机密](xref:security/app-secrets)（在 `Development` 环境中）、环境变量和命令行参数中加载可选配置。</span><span class="sxs-lookup"><span data-stu-id="1a414-203">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="1a414-204">在最后调用 CommandLine 配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="1a414-204">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="1a414-205">最后调用提供程序，则在运行时传递的命令行参数可以替代之前调用的其他配置提供程序设置的配置。</span><span class="sxs-lookup"><span data-stu-id="1a414-205">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="1a414-206">对于满足以下条件的 appsettings 文件：</span><span class="sxs-lookup"><span data-stu-id="1a414-206">For *appsettings* files where:</span></span>

* <span data-ttu-id="1a414-207">已启用 `reloadOnChange`。</span><span class="sxs-lookup"><span data-stu-id="1a414-207">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="1a414-208">在命令行参数和 appsettings 文件中包含相同的设置。</span><span class="sxs-lookup"><span data-stu-id="1a414-208">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="1a414-209">应用启动后，包含匹配的命令行参数的 appsettings 文件发生更改。</span><span class="sxs-lookup"><span data-stu-id="1a414-209">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="1a414-210">如果上述所有条件均成立，则命令行参数将被覆盖。</span><span class="sxs-lookup"><span data-stu-id="1a414-210">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="1a414-211">ASP.NET Core 2.x 应用可使用 WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)，而不是 \`CreateDefaultBuilder。使用 WebHostBuilder 时，请通过 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) 手动设置配置。</span><span class="sxs-lookup"><span data-stu-id="1a414-211">ASP.NET Core 2.x app can use WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of \`\`CreateDefaultBuilder`. When using `WebHostBuilder\`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="1a414-212">有关详细信息，请参阅 ASP.NET Core 1.x 选项卡。</span><span class="sxs-lookup"><span data-stu-id="1a414-212">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1a414-213">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1a414-213">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1a414-214">创建 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) 并调用 `AddCommandLine` 方法来使用 CommandLine 配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="1a414-214">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="1a414-215">最后调用提供程序，则在运行时传递的命令行参数可以替代之前调用的其他配置提供程序设置的配置。</span><span class="sxs-lookup"><span data-stu-id="1a414-215">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="1a414-216">使用 `UseConfiguration` 方法向 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 应用配置：</span><span class="sxs-lookup"><span data-stu-id="1a414-216">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="1a414-217">自变量</span><span class="sxs-lookup"><span data-stu-id="1a414-217">Arguments</span></span>

<span data-ttu-id="1a414-218">在命令行上传递的参数必须符合下表所示的两种格式之一：</span><span class="sxs-lookup"><span data-stu-id="1a414-218">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="1a414-219">参数格式</span><span class="sxs-lookup"><span data-stu-id="1a414-219">Argument format</span></span>                                                     | <span data-ttu-id="1a414-220">示例</span><span class="sxs-lookup"><span data-stu-id="1a414-220">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="1a414-221">单一参数：由等于号 (`=`) 分隔的键值对</span><span class="sxs-lookup"><span data-stu-id="1a414-221">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="1a414-222">两个参数的序列：由空格分隔的键值对</span><span class="sxs-lookup"><span data-stu-id="1a414-222">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="1a414-223">**单一参数**</span><span class="sxs-lookup"><span data-stu-id="1a414-223">**Single argument**</span></span>

<span data-ttu-id="1a414-224">值必须跟在等于号 (`=`) 之后。</span><span class="sxs-lookup"><span data-stu-id="1a414-224">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="1a414-225">其值可以为 NULL（例如 `mykey=`）。</span><span class="sxs-lookup"><span data-stu-id="1a414-225">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="1a414-226">键可以具有前缀。</span><span class="sxs-lookup"><span data-stu-id="1a414-226">The key may have a prefix.</span></span>

| <span data-ttu-id="1a414-227">键前缀</span><span class="sxs-lookup"><span data-stu-id="1a414-227">Key prefix</span></span>               | <span data-ttu-id="1a414-228">示例</span><span class="sxs-lookup"><span data-stu-id="1a414-228">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="1a414-229">无前缀</span><span class="sxs-lookup"><span data-stu-id="1a414-229">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="1a414-230">单划线 (`-`)†</span><span class="sxs-lookup"><span data-stu-id="1a414-230">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="1a414-231">双划线 (`--`)</span><span class="sxs-lookup"><span data-stu-id="1a414-231">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="1a414-232">正斜杠 (`/`)</span><span class="sxs-lookup"><span data-stu-id="1a414-232">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="1a414-233">†[交换映射](#switch-mappings)中必须提供带有单划线前缀 (`-`) 的键，如下所示。</span><span class="sxs-lookup"><span data-stu-id="1a414-233">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="1a414-234">示例命令：</span><span class="sxs-lookup"><span data-stu-id="1a414-234">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="1a414-235">注意：如果向配置提供程序提供的[交换映射](#switch-mappings)中没有 `-key1`，则将引发 `FormatException`。</span><span class="sxs-lookup"><span data-stu-id="1a414-235">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="1a414-236">**两个参数的序列**</span><span class="sxs-lookup"><span data-stu-id="1a414-236">**Sequence of two arguments**</span></span>

<span data-ttu-id="1a414-237">该值不可为 NULL，且必须跟在由空格分隔的键之后。</span><span class="sxs-lookup"><span data-stu-id="1a414-237">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="1a414-238">键必须具有前缀。</span><span class="sxs-lookup"><span data-stu-id="1a414-238">The key must have a prefix.</span></span>

| <span data-ttu-id="1a414-239">键前缀</span><span class="sxs-lookup"><span data-stu-id="1a414-239">Key prefix</span></span>               | <span data-ttu-id="1a414-240">示例</span><span class="sxs-lookup"><span data-stu-id="1a414-240">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="1a414-241">单划线 (`-`)†</span><span class="sxs-lookup"><span data-stu-id="1a414-241">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="1a414-242">双划线 (`--`)</span><span class="sxs-lookup"><span data-stu-id="1a414-242">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="1a414-243">正斜杠 (`/`)</span><span class="sxs-lookup"><span data-stu-id="1a414-243">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="1a414-244">†[交换映射](#switch-mappings)中必须提供带有单划线前缀 (`-`) 的键，如下所示。</span><span class="sxs-lookup"><span data-stu-id="1a414-244">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="1a414-245">示例命令：</span><span class="sxs-lookup"><span data-stu-id="1a414-245">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="1a414-246">注意：如果向配置提供程序提供的[交换映射](#switch-mappings)中没有 `-key1`，则将引发 `FormatException`。</span><span class="sxs-lookup"><span data-stu-id="1a414-246">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="1a414-247">重复键</span><span class="sxs-lookup"><span data-stu-id="1a414-247">Duplicate keys</span></span>

<span data-ttu-id="1a414-248">如果提供了重复的键，将使用最后一个键值对。</span><span class="sxs-lookup"><span data-stu-id="1a414-248">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="1a414-249">交换映射</span><span class="sxs-lookup"><span data-stu-id="1a414-249">Switch mappings</span></span>

<span data-ttu-id="1a414-250">在使用 `ConfigurationBuilder` 手动生成配置时，可选择向 `AddCommandLine` 方法提供交换映射字典。</span><span class="sxs-lookup"><span data-stu-id="1a414-250">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="1a414-251">可通过交换映射提供键名替换逻辑。</span><span class="sxs-lookup"><span data-stu-id="1a414-251">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="1a414-252">当使用交换映射字典时，会检查字典中是否有与命令行参数提供的键匹配的键。</span><span class="sxs-lookup"><span data-stu-id="1a414-252">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="1a414-253">如果在字典中找到命令行键，则传回字典值（替换键）以设置配置。</span><span class="sxs-lookup"><span data-stu-id="1a414-253">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="1a414-254">对任何具有单划线 (`-`) 前缀的命令行键而言，交换映射都是必需的。</span><span class="sxs-lookup"><span data-stu-id="1a414-254">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="1a414-255">交换映射字典键规则：</span><span class="sxs-lookup"><span data-stu-id="1a414-255">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="1a414-256">交换必须以单划线 (`-`) 或双划线 (`--`) 开头。</span><span class="sxs-lookup"><span data-stu-id="1a414-256">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="1a414-257">交换映射字典不得包含重复键。</span><span class="sxs-lookup"><span data-stu-id="1a414-257">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="1a414-258">在下列示例中，`GetSwitchMappings` 方法允许命令行参数使用单划线 (`-`) 键前缀，并避免使用前导子键前缀。</span><span class="sxs-lookup"><span data-stu-id="1a414-258">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="1a414-259">如果不提供命令行参数，则由提供给 `AddInMemoryCollection` 的字典来设置配置值。</span><span class="sxs-lookup"><span data-stu-id="1a414-259">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="1a414-260">使用以下命令运行应用：</span><span class="sxs-lookup"><span data-stu-id="1a414-260">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="1a414-261">控制台窗口将显示：</span><span class="sxs-lookup"><span data-stu-id="1a414-261">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="1a414-262">使用以下命令传递配置设置：</span><span class="sxs-lookup"><span data-stu-id="1a414-262">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="1a414-263">控制台窗口将显示：</span><span class="sxs-lookup"><span data-stu-id="1a414-263">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="1a414-264">创建交换映射字典后，它将包含下表所示的数据：</span><span class="sxs-lookup"><span data-stu-id="1a414-264">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="1a414-265">键</span><span class="sxs-lookup"><span data-stu-id="1a414-265">Key</span></span>            | <span data-ttu-id="1a414-266">“值”</span><span class="sxs-lookup"><span data-stu-id="1a414-266">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="1a414-267">要使用字典演示键交换，请运行下列命令：</span><span class="sxs-lookup"><span data-stu-id="1a414-267">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="1a414-268">将交换命令行键。</span><span class="sxs-lookup"><span data-stu-id="1a414-268">The command-line keys are swapped.</span></span> <span data-ttu-id="1a414-269">控制台窗口将显示 `Profile:MachineName` 和 `App:MainWindow:Left` 的配置值：</span><span class="sxs-lookup"><span data-stu-id="1a414-269">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="1a414-270">web.config 文件</span><span class="sxs-lookup"><span data-stu-id="1a414-270">The web.config file</span></span>

<span data-ttu-id="1a414-271">在 IIS 或 IIS Express 中托管应用时，需要 web.config 文件。</span><span class="sxs-lookup"><span data-stu-id="1a414-271">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="1a414-272">通过 web.config 中的设置，[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) 可以启动应用并配置其他 IIS 设置和模块。</span><span class="sxs-lookup"><span data-stu-id="1a414-272">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="1a414-273">如果 *web.config* 文件不存在，并且项目文件中包含 `<Project Sdk="Microsoft.NET.Sdk.Web">`，则发布项目时会在发布的输出（“发布”文件夹）中创建一个 *web.config* 文件。</span><span class="sxs-lookup"><span data-stu-id="1a414-273">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="1a414-274">有关详细信息，请参阅 [使用 IIS 在 Windows 上托管 ASP.NET Core](xref:host-and-deploy/iis/index#webconfig)。</span><span class="sxs-lookup"><span data-stu-id="1a414-274">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig).</span></span>

## <a name="additional-notes"></a><span data-ttu-id="1a414-275">附加说明</span><span class="sxs-lookup"><span data-stu-id="1a414-275">Additional notes</span></span>

* <span data-ttu-id="1a414-276">调用 `ConfigureServices` 后才会设置依赖关系注入 (DI)。</span><span class="sxs-lookup"><span data-stu-id="1a414-276">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="1a414-277">配置系统无法感知 DI。</span><span class="sxs-lookup"><span data-stu-id="1a414-277">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="1a414-278">`IConfiguration` 具有两项专用化：</span><span class="sxs-lookup"><span data-stu-id="1a414-278">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="1a414-279">`IConfigurationRoot` 用于根节点。</span><span class="sxs-lookup"><span data-stu-id="1a414-279">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="1a414-280">可以触发重载。</span><span class="sxs-lookup"><span data-stu-id="1a414-280">Can trigger a reload.</span></span>
  * <span data-ttu-id="1a414-281">`IConfigurationSection` 表示配置值的一节。</span><span class="sxs-lookup"><span data-stu-id="1a414-281">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="1a414-282">`GetSection` 和 `GetChildren` 方法返回 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="1a414-282">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="1a414-283">重新加载配置或需要访问每个提供程序时，请使用 [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot)。</span><span class="sxs-lookup"><span data-stu-id="1a414-283">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or need access to each provider.</span></span> <span data-ttu-id="1a414-284">这两种情况都不常见。</span><span class="sxs-lookup"><span data-stu-id="1a414-284">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1a414-285">其他资源</span><span class="sxs-lookup"><span data-stu-id="1a414-285">Additional resources</span></span>

* [<span data-ttu-id="1a414-286">选项</span><span class="sxs-lookup"><span data-stu-id="1a414-286">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="1a414-287">使用多个环境</span><span class="sxs-lookup"><span data-stu-id="1a414-287">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="1a414-288">在开发期间安全存储应用密钥</span><span class="sxs-lookup"><span data-stu-id="1a414-288">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="1a414-289">ASP.NET Core 中的托管</span><span class="sxs-lookup"><span data-stu-id="1a414-289">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="1a414-290">依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="1a414-290">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="1a414-291">Azure Key Vault 配置提供程序</span><span class="sxs-lookup"><span data-stu-id="1a414-291">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
