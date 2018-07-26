---
title: ASP.NET Core 中的配置
author: rick-anderson
description: 理解如何使用配置 API 配置 ASP.NET Core 应用。
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 59ab0cd0f6975d15bd01ce7e4128521938182c24
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228619"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="4735c-103">ASP.NET Core 中的配置</span><span class="sxs-lookup"><span data-stu-id="4735c-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="4735c-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Mark Michaelis](http://intellitect.com/author/mark-michaelis/)、[Steve Smith](https://ardalis.com/)、[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4735c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4735c-105">通过配置 API ，可基于名称/值对列表来配置 ASP.NET Core Web 应用。</span><span class="sxs-lookup"><span data-stu-id="4735c-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="4735c-106">在运行时从多个源读取配置。</span><span class="sxs-lookup"><span data-stu-id="4735c-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="4735c-107">可将名称/值对分组到多级层次结构。</span><span class="sxs-lookup"><span data-stu-id="4735c-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="4735c-108">配置提供程序适用于：</span><span class="sxs-lookup"><span data-stu-id="4735c-108">There are configuration providers for:</span></span>

* <span data-ttu-id="4735c-109">文件格式（INI、JSON 和 XML）。</span><span class="sxs-lookup"><span data-stu-id="4735c-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="4735c-110">命令行参数。</span><span class="sxs-lookup"><span data-stu-id="4735c-110">Command-line arguments.</span></span>
* <span data-ttu-id="4735c-111">环境变量。</span><span class="sxs-lookup"><span data-stu-id="4735c-111">Environment variables.</span></span>
* <span data-ttu-id="4735c-112">内存中的 .NET 对象。</span><span class="sxs-lookup"><span data-stu-id="4735c-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="4735c-113">未加密的[机密管理器](xref:security/app-secrets)存储。</span><span class="sxs-lookup"><span data-stu-id="4735c-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="4735c-114">加密的用户存储，如 [Azure Key Vault](xref:security/key-vault-configuration)。</span><span class="sxs-lookup"><span data-stu-id="4735c-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="4735c-115">（已安装或已创建的）自定义提供程序。</span><span class="sxs-lookup"><span data-stu-id="4735c-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="4735c-116">每个配置值映射到一个字符串键。</span><span class="sxs-lookup"><span data-stu-id="4735c-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="4735c-117">可借助内置绑定支持，将设置反序列化为自定义 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 对象（一种具有属性的简单 .NET 类）。</span><span class="sxs-lookup"><span data-stu-id="4735c-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="4735c-118">选项模式使用选项类来表示相关设置的组。</span><span class="sxs-lookup"><span data-stu-id="4735c-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="4735c-119">有关使用选项模式的详细信息，请参阅[选项](xref:fundamentals/configuration/options)主题。</span><span class="sxs-lookup"><span data-stu-id="4735c-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="4735c-120">[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="4735c-120">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4735c-121">本主题中提供的示例依赖于：</span><span class="sxs-lookup"><span data-stu-id="4735c-121">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="4735c-122">使用 [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) 设置应用的基路径。</span><span class="sxs-lookup"><span data-stu-id="4735c-122">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="4735c-123">通过引用 [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 包向应用提供 `SetBasePath`。</span><span class="sxs-lookup"><span data-stu-id="4735c-123">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="4735c-124">使用 [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection) 解析配置文件的节。</span><span class="sxs-lookup"><span data-stu-id="4735c-124">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="4735c-125">通过引用 [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) 包向应用提供 `GetSection`。</span><span class="sxs-lookup"><span data-stu-id="4735c-125">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="4735c-126">用 [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind) 绑定配置。</span><span class="sxs-lookup"><span data-stu-id="4735c-126">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="4735c-127">通过引用 [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) 包向应用提供 `Bind`。</span><span class="sxs-lookup"><span data-stu-id="4735c-127">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

<span data-ttu-id="4735c-128">这些包均包括在 [Microsoft.AspNetCore.App 元包](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="4735c-128">These packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4735c-129">本主题中提供的示例依赖于：</span><span class="sxs-lookup"><span data-stu-id="4735c-129">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="4735c-130">使用 [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) 设置应用的基路径。</span><span class="sxs-lookup"><span data-stu-id="4735c-130">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="4735c-131">通过引用 [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 包向应用提供 `SetBasePath`。</span><span class="sxs-lookup"><span data-stu-id="4735c-131">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="4735c-132">使用 [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection) 解析配置文件的节。</span><span class="sxs-lookup"><span data-stu-id="4735c-132">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="4735c-133">通过引用 [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) 包向应用提供 `GetSection`。</span><span class="sxs-lookup"><span data-stu-id="4735c-133">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="4735c-134">用 [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind) 绑定配置。</span><span class="sxs-lookup"><span data-stu-id="4735c-134">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="4735c-135">通过引用 [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) 包向应用提供 `Bind`。</span><span class="sxs-lookup"><span data-stu-id="4735c-135">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

<span data-ttu-id="4735c-136">这些包均包括在 [Microsoft.AspNetCore.All 元包](xref:fundamentals/metapackage)中。</span><span class="sxs-lookup"><span data-stu-id="4735c-136">These packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="4735c-137">本主题中提供的示例依赖于：</span><span class="sxs-lookup"><span data-stu-id="4735c-137">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="4735c-138">使用 [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) 设置应用的基路径。</span><span class="sxs-lookup"><span data-stu-id="4735c-138">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="4735c-139">通过引用 [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) 包向应用提供 `SetBasePath`。</span><span class="sxs-lookup"><span data-stu-id="4735c-139">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="4735c-140">使用 [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection) 解析配置文件的节。</span><span class="sxs-lookup"><span data-stu-id="4735c-140">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="4735c-141">通过引用 [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) 包向应用提供 `GetSection`。</span><span class="sxs-lookup"><span data-stu-id="4735c-141">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="4735c-142">用 [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind) 绑定配置。</span><span class="sxs-lookup"><span data-stu-id="4735c-142">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="4735c-143">通过引用 [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) 包向应用提供 `Bind`。</span><span class="sxs-lookup"><span data-stu-id="4735c-143">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

::: moniker-end

## <a name="json-configuration"></a><span data-ttu-id="4735c-144">JSON 配置</span><span class="sxs-lookup"><span data-stu-id="4735c-144">JSON configuration</span></span>

<span data-ttu-id="4735c-145">以下控制台应用使用 JSON 配置提供程序：</span><span class="sxs-lookup"><span data-stu-id="4735c-145">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="4735c-146">该应用将读取和显示下列配置设置：</span><span class="sxs-lookup"><span data-stu-id="4735c-146">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="4735c-147">配置包含名称/值对的分层列表，其中节点由冒号 (`:`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="4735c-147">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon (`:`).</span></span> <span data-ttu-id="4735c-148">要检索某个值，请使用相应项的键访问 `Configuration` 索引器：</span><span class="sxs-lookup"><span data-stu-id="4735c-148">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="4735c-149">要在 JSON 格式的配置源中使用数组，请在由冒号分隔的字符串中使用数组索引。</span><span class="sxs-lookup"><span data-stu-id="4735c-149">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="4735c-150">以下示例获取上述 `wizards` 数组中第一个项的名称：</span><span class="sxs-lookup"><span data-stu-id="4735c-150">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="4735c-151">写入内置[配置](/dotnet/api/microsoft.extensions.configuration)提供程序的名称/值对不是持久的。</span><span class="sxs-lookup"><span data-stu-id="4735c-151">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="4735c-152">但是，可以创建一个自定义提供程序来保存值。</span><span class="sxs-lookup"><span data-stu-id="4735c-152">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="4735c-153">请参阅[自定义配置提供程序](xref:fundamentals/configuration/index#custom-config-providers)。</span><span class="sxs-lookup"><span data-stu-id="4735c-153">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="4735c-154">前面的示例使用配置索引器来读取值。</span><span class="sxs-lookup"><span data-stu-id="4735c-154">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="4735c-155">要访问 `Startup` 外部的配置，请使用选项模式。</span><span class="sxs-lookup"><span data-stu-id="4735c-155">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="4735c-156">有关详细信息，请参阅[选项](xref:fundamentals/configuration/options)主题。</span><span class="sxs-lookup"><span data-stu-id="4735c-156">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="4735c-157">XML 配置</span><span class="sxs-lookup"><span data-stu-id="4735c-157">XML configuration</span></span>

<span data-ttu-id="4735c-158">若要在 XML 格式的配置源中使用数组，请向每个元素提供一个 `name` 索引。</span><span class="sxs-lookup"><span data-stu-id="4735c-158">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="4735c-159">使用该索引访问以下值：</span><span class="sxs-lookup"><span data-stu-id="4735c-159">Use the index to access the values:</span></span>

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a><span data-ttu-id="4735c-160">按环境配置</span><span class="sxs-lookup"><span data-stu-id="4735c-160">Configuration by environment</span></span>

<span data-ttu-id="4735c-161">通常而言，配置设置因环境（如开发、测试和生产等）而异。</span><span class="sxs-lookup"><span data-stu-id="4735c-161">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="4735c-162">ASP.NET Core 2.x 应用中的 `CreateDefaultBuilder` 扩展方法（或直接在 ASP.NET Core 1.x 应用中使用 `AddJsonFile` 和 `AddEnvironmentVariables`）添加了用于读取 JSON 文件和系统配置源的配置提供程序：</span><span class="sxs-lookup"><span data-stu-id="4735c-162">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="4735c-163">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="4735c-163">*appsettings.json*</span></span>
* <span data-ttu-id="4735c-164">appsettings.\<EnvironmentName>.json</span><span class="sxs-lookup"><span data-stu-id="4735c-164">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="4735c-165">环境变量</span><span class="sxs-lookup"><span data-stu-id="4735c-165">Environment variables</span></span>

<span data-ttu-id="4735c-166">ASP.NET Core 1.x 应用需要调用 `AddJsonFile` 和 [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_)。</span><span class="sxs-lookup"><span data-stu-id="4735c-166">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="4735c-167">有关参数的说明，请参阅 [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions)。</span><span class="sxs-lookup"><span data-stu-id="4735c-167">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="4735c-168">仅 ASP.NET Core 1.1 及更高版本支持 `reloadOnChange`。</span><span class="sxs-lookup"><span data-stu-id="4735c-168">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="4735c-169">按指定配置源的顺序读取它们。</span><span class="sxs-lookup"><span data-stu-id="4735c-169">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="4735c-170">在前面的代码中，最后才读取环境变量。</span><span class="sxs-lookup"><span data-stu-id="4735c-170">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="4735c-171">在环境中设置的任意配置值将替换先前两个提供程序中设置的配置值。</span><span class="sxs-lookup"><span data-stu-id="4735c-171">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="4735c-172">请考虑使用以下 appsettings.Staging.json 文件：</span><span class="sxs-lookup"><span data-stu-id="4735c-172">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="4735c-173">在下面的代码中，`MyConfig` 会读取 `Configure` 的值：</span><span class="sxs-lookup"><span data-stu-id="4735c-173">In the following code, `Configure` reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

<span data-ttu-id="4735c-174">环境通常设置为 `Development`、`Staging` 或 `Production`。</span><span class="sxs-lookup"><span data-stu-id="4735c-174">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="4735c-175">有关详细信息，请参阅[使用多个环境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="4735c-175">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="4735c-176">配置注意事项：</span><span class="sxs-lookup"><span data-stu-id="4735c-176">Configuration considerations:</span></span>

* <span data-ttu-id="4735c-177">配置数据发生更改时，[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) 可将其重载。</span><span class="sxs-lookup"><span data-stu-id="4735c-177">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) can reload configuration data when it changes.</span></span>
* <span data-ttu-id="4735c-178">配置密钥不区分大小写。</span><span class="sxs-lookup"><span data-stu-id="4735c-178">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="4735c-179">请勿在配置提供程序代码或纯文本配置文件中存储密码或其他敏感数据。</span><span class="sxs-lookup"><span data-stu-id="4735c-179">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="4735c-180">不要在开发或测试环境中使用生产机密。</span><span class="sxs-lookup"><span data-stu-id="4735c-180">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="4735c-181">请在项目外部指定机密，避免将其意外提交到源代码存储库。</span><span class="sxs-lookup"><span data-stu-id="4735c-181">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="4735c-182">详细了解[如何使用多个环境](xref:fundamentals/environments)和[在开发期间管理应用机密的安全存储](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="4735c-182">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets in development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="4735c-183">对于在环境变量中指定的分层配置值，冒号 (`:`) 可能不适用于所有平台。</span><span class="sxs-lookup"><span data-stu-id="4735c-183">For hierarchical config values specified in environment variables, a colon (`:`) may not work on all platforms.</span></span> <span data-ttu-id="4735c-184">而所有平台均支持采用双下划线 (`__`)。</span><span class="sxs-lookup"><span data-stu-id="4735c-184">Double underscore (`__`) is supported by all platforms.</span></span>
* <span data-ttu-id="4735c-185">与配置 API 交互时，冒号 (`:`) 适用于所有平台。</span><span class="sxs-lookup"><span data-stu-id="4735c-185">When interacting with the configuration API, a colon (`:`) works on all platforms.</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="4735c-186">内存中提供程序及绑定到 POCO 类</span><span class="sxs-lookup"><span data-stu-id="4735c-186">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="4735c-187">以下示例演示如何使用内存中提供程序及绑定到类：</span><span class="sxs-lookup"><span data-stu-id="4735c-187">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="4735c-188">配置值以字符串的形式返回，但绑定使对象的构造成为可能。</span><span class="sxs-lookup"><span data-stu-id="4735c-188">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="4735c-189">通过绑定可检索 POCO 对象，甚至可检索整个对象图。</span><span class="sxs-lookup"><span data-stu-id="4735c-189">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="4735c-190">GetValue</span><span class="sxs-lookup"><span data-stu-id="4735c-190">GetValue</span></span>

<span data-ttu-id="4735c-191">以下示例演示 [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) 扩展方法：</span><span class="sxs-lookup"><span data-stu-id="4735c-191">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="4735c-192">ConfigurationBinder 的 `GetValue<T>` 方法允许指定默认值（在此示例中为 80）。</span><span class="sxs-lookup"><span data-stu-id="4735c-192">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="4735c-193">`GetValue<T>` 适用于简单方案，并不绑定到整个部分。</span><span class="sxs-lookup"><span data-stu-id="4735c-193">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="4735c-194">`GetValue<T>` 从转换为特定类型的 `GetSection(key).Value` 中获取标量值。</span><span class="sxs-lookup"><span data-stu-id="4735c-194">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="4735c-195">绑定至对象图</span><span class="sxs-lookup"><span data-stu-id="4735c-195">Bind to an object graph</span></span>

<span data-ttu-id="4735c-196">可递归绑定类中的每个对象。</span><span class="sxs-lookup"><span data-stu-id="4735c-196">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="4735c-197">请考虑使用以下 `AppSettings` 类：</span><span class="sxs-lookup"><span data-stu-id="4735c-197">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="4735c-198">以下示例绑定到 `AppSettings` 类：</span><span class="sxs-lookup"><span data-stu-id="4735c-198">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="4735c-199">ASP.NET Core 1.1 及更高版本可使用 `Get<T>`，它适用于整个部分。</span><span class="sxs-lookup"><span data-stu-id="4735c-199">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="4735c-200">使用 `Get<T>` 可能比 `Bind` 方便。</span><span class="sxs-lookup"><span data-stu-id="4735c-200">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="4735c-201">以下代码演示了如何通过前述示例使用 `Get<T>`：</span><span class="sxs-lookup"><span data-stu-id="4735c-201">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="4735c-202">使用以下 appsettings.json 文件：</span><span class="sxs-lookup"><span data-stu-id="4735c-202">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="4735c-203">该程序显示 `Height 11`。</span><span class="sxs-lookup"><span data-stu-id="4735c-203">The program displays `Height 11`.</span></span>

<span data-ttu-id="4735c-204">可使用以下代码对配置进行单元测试：</span><span class="sxs-lookup"><span data-stu-id="4735c-204">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="4735c-205">创建 Entity Framework 自定义提供程序</span><span class="sxs-lookup"><span data-stu-id="4735c-205">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="4735c-206">本部分将创建一个使用 EF 从数据库读取名称/值对的基本配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="4735c-206">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="4735c-207">定义 `ConfigurationValue` 实体，用于在数据库中存储配置值：</span><span class="sxs-lookup"><span data-stu-id="4735c-207">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="4735c-208">添加 `ConfigurationContext` 以便存储和访问配置值：</span><span class="sxs-lookup"><span data-stu-id="4735c-208">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="4735c-209">创建实现 [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource) 的类：</span><span class="sxs-lookup"><span data-stu-id="4735c-209">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="4735c-210">通过从 [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider) 继承来创建自定义配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="4735c-210">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="4735c-211">当数据库为空时，配置提供程序将对其进行初始化：</span><span class="sxs-lookup"><span data-stu-id="4735c-211">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="4735c-212">运行示例时将显示数据库中突出显示的值（“value_from_ef_1”和“value_from_ef_2”）。</span><span class="sxs-lookup"><span data-stu-id="4735c-212">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="4735c-213">可使用 `EFConfigSource` 扩展方法添加配置源：</span><span class="sxs-lookup"><span data-stu-id="4735c-213">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="4735c-214">下面的代码演示如何使用自定义 `EFConfigProvider`：</span><span class="sxs-lookup"><span data-stu-id="4735c-214">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="4735c-215">请注意，示例在 JSON 提供程序之后添加自定义 `EFConfigProvider`，因此数据库中的任何设置都将替代 appsettings.json 文件中的设置。</span><span class="sxs-lookup"><span data-stu-id="4735c-215">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="4735c-216">使用以下 appsettings.json 文件：</span><span class="sxs-lookup"><span data-stu-id="4735c-216">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="4735c-217">显示以下输出：</span><span class="sxs-lookup"><span data-stu-id="4735c-217">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="4735c-218">CommandLine 配置提供程序</span><span class="sxs-lookup"><span data-stu-id="4735c-218">CommandLine configuration provider</span></span>

<span data-ttu-id="4735c-219">[CommandLine 配置提供程序](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider)在运行时接收用于配置的命令行参数键值对。</span><span class="sxs-lookup"><span data-stu-id="4735c-219">The [CommandLine configuration provider](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="4735c-220">查看或下载命令行配置示例</span><span class="sxs-lookup"><span data-stu-id="4735c-220">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="4735c-221">设置和使用 CommandLine 配置提供程序</span><span class="sxs-lookup"><span data-stu-id="4735c-221">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="4735c-222">基本配置</span><span class="sxs-lookup"><span data-stu-id="4735c-222">Basic Configuration</span></span>](#tab/basicconfiguration/)

<span data-ttu-id="4735c-223">要激活命令行配置，请在 [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) 的实例上调用 `AddCommandLine` 扩展方法：</span><span class="sxs-lookup"><span data-stu-id="4735c-223">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="4735c-224">运行代码将显示以下输出：</span><span class="sxs-lookup"><span data-stu-id="4735c-224">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="4735c-225">在命令行上传递参数键值对将更改 `Profile:MachineName` 和 `App:MainWindow:Left` 的值：</span><span class="sxs-lookup"><span data-stu-id="4735c-225">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="4735c-226">控制台窗口将显示：</span><span class="sxs-lookup"><span data-stu-id="4735c-226">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="4735c-227">要使用命令行配置替代由其他配置提供程序提供的配置，请在 `ConfigurationBuilder` 上最后调用 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="4735c-227">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4735c-228">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4735c-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="4735c-229">典型的 ASP.NET Core 2.x 应用使用静态简便方法 `CreateDefaultBuilder` 生成主机：</span><span class="sxs-lookup"><span data-stu-id="4735c-229">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="4735c-230">`CreateDefaultBuilder` 从 appsettings.json、appsettings.{Environment}.json、[用户机密](xref:security/app-secrets)（在 `Development` 环境中）、环境变量和命令行参数中加载可选配置。</span><span class="sxs-lookup"><span data-stu-id="4735c-230">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="4735c-231">在最后调用 CommandLine 配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="4735c-231">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="4735c-232">最后调用提供程序，则在运行时传递的命令行参数可以替代之前调用的其他配置提供程序设置的配置。</span><span class="sxs-lookup"><span data-stu-id="4735c-232">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="4735c-233">对于满足以下条件的 appsettings 文件：</span><span class="sxs-lookup"><span data-stu-id="4735c-233">For *appsettings* files where:</span></span>

* <span data-ttu-id="4735c-234">已启用 `reloadOnChange`。</span><span class="sxs-lookup"><span data-stu-id="4735c-234">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="4735c-235">在命令行参数和 appsettings 文件中包含相同的设置。</span><span class="sxs-lookup"><span data-stu-id="4735c-235">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="4735c-236">应用启动后，包含匹配的命令行参数的 appsettings 文件发生更改。</span><span class="sxs-lookup"><span data-stu-id="4735c-236">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="4735c-237">如果上述所有条件均成立，则命令行参数将被覆盖。</span><span class="sxs-lookup"><span data-stu-id="4735c-237">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="4735c-238">ASP.NET Core 2.x 应用可使用 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)，而不是 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="4735c-238">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="4735c-239">使用 `WebHostBuilder` 时，请手动通过 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) 设置配置。</span><span class="sxs-lookup"><span data-stu-id="4735c-239">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="4735c-240">有关详细信息，请参阅 ASP.NET Core 1.x 选项卡。</span><span class="sxs-lookup"><span data-stu-id="4735c-240">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4735c-241">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4735c-241">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="4735c-242">创建 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) 并调用 `AddCommandLine` 方法来使用 CommandLine 配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="4735c-242">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="4735c-243">最后调用提供程序，则在运行时传递的命令行参数可以替代之前调用的其他配置提供程序设置的配置。</span><span class="sxs-lookup"><span data-stu-id="4735c-243">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="4735c-244">使用 `UseConfiguration` 方法向 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 应用配置：</span><span class="sxs-lookup"><span data-stu-id="4735c-244">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="4735c-245">自变量</span><span class="sxs-lookup"><span data-stu-id="4735c-245">Arguments</span></span>

<span data-ttu-id="4735c-246">在命令行上传递的参数必须符合下表所示的两种格式之一：</span><span class="sxs-lookup"><span data-stu-id="4735c-246">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="4735c-247">参数格式</span><span class="sxs-lookup"><span data-stu-id="4735c-247">Argument format</span></span>                                                     | <span data-ttu-id="4735c-248">示例</span><span class="sxs-lookup"><span data-stu-id="4735c-248">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="4735c-249">单一参数：由等于号 (`=`) 分隔的键值对</span><span class="sxs-lookup"><span data-stu-id="4735c-249">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="4735c-250">两个参数的序列：由空格分隔的键值对</span><span class="sxs-lookup"><span data-stu-id="4735c-250">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="4735c-251">**单一参数**</span><span class="sxs-lookup"><span data-stu-id="4735c-251">**Single argument**</span></span>

<span data-ttu-id="4735c-252">值必须跟在等于号 (`=`) 之后。</span><span class="sxs-lookup"><span data-stu-id="4735c-252">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="4735c-253">其值可以为 NULL（例如 `mykey=`）。</span><span class="sxs-lookup"><span data-stu-id="4735c-253">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="4735c-254">键可以具有前缀。</span><span class="sxs-lookup"><span data-stu-id="4735c-254">The key may have a prefix.</span></span>

| <span data-ttu-id="4735c-255">键前缀</span><span class="sxs-lookup"><span data-stu-id="4735c-255">Key prefix</span></span>               | <span data-ttu-id="4735c-256">示例</span><span class="sxs-lookup"><span data-stu-id="4735c-256">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="4735c-257">无前缀</span><span class="sxs-lookup"><span data-stu-id="4735c-257">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="4735c-258">单划线 (`-`)†</span><span class="sxs-lookup"><span data-stu-id="4735c-258">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="4735c-259">双划线 (`--`)</span><span class="sxs-lookup"><span data-stu-id="4735c-259">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="4735c-260">正斜杠 (`/`)</span><span class="sxs-lookup"><span data-stu-id="4735c-260">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="4735c-261">†[交换映射](#switch-mappings)中必须提供带有单划线前缀 (`-`) 的键，如下所示。</span><span class="sxs-lookup"><span data-stu-id="4735c-261">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="4735c-262">示例命令：</span><span class="sxs-lookup"><span data-stu-id="4735c-262">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="4735c-263">注意：如果向配置提供程序提供的[交换映射](#switch-mappings)中没有 `-key2`，则将引发 `FormatException`。</span><span class="sxs-lookup"><span data-stu-id="4735c-263">Note: If `-key2` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="4735c-264">**两个参数的序列**</span><span class="sxs-lookup"><span data-stu-id="4735c-264">**Sequence of two arguments**</span></span>

<span data-ttu-id="4735c-265">该值不可为 NULL，且必须跟在由空格分隔的键之后。</span><span class="sxs-lookup"><span data-stu-id="4735c-265">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="4735c-266">键必须具有前缀。</span><span class="sxs-lookup"><span data-stu-id="4735c-266">The key must have a prefix.</span></span>

| <span data-ttu-id="4735c-267">键前缀</span><span class="sxs-lookup"><span data-stu-id="4735c-267">Key prefix</span></span>               | <span data-ttu-id="4735c-268">示例</span><span class="sxs-lookup"><span data-stu-id="4735c-268">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="4735c-269">单划线 (`-`)†</span><span class="sxs-lookup"><span data-stu-id="4735c-269">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="4735c-270">双划线 (`--`)</span><span class="sxs-lookup"><span data-stu-id="4735c-270">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="4735c-271">正斜杠 (`/`)</span><span class="sxs-lookup"><span data-stu-id="4735c-271">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="4735c-272">†[交换映射](#switch-mappings)中必须提供带有单划线前缀 (`-`) 的键，如下所示。</span><span class="sxs-lookup"><span data-stu-id="4735c-272">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="4735c-273">示例命令：</span><span class="sxs-lookup"><span data-stu-id="4735c-273">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="4735c-274">注意：如果向配置提供程序提供的[交换映射](#switch-mappings)中没有 `-key1`，则将引发 `FormatException`。</span><span class="sxs-lookup"><span data-stu-id="4735c-274">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="4735c-275">重复键</span><span class="sxs-lookup"><span data-stu-id="4735c-275">Duplicate keys</span></span>

<span data-ttu-id="4735c-276">如果提供了重复的键，将使用最后一个键值对。</span><span class="sxs-lookup"><span data-stu-id="4735c-276">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="4735c-277">交换映射</span><span class="sxs-lookup"><span data-stu-id="4735c-277">Switch mappings</span></span>

<span data-ttu-id="4735c-278">使用 `ConfigurationBuilder` 手动生成配置时，可将交换映射字典添加到 `AddCommandLine` 方法。</span><span class="sxs-lookup"><span data-stu-id="4735c-278">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="4735c-279">交换映射支持键名替换逻辑。</span><span class="sxs-lookup"><span data-stu-id="4735c-279">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="4735c-280">当使用交换映射字典时，会检查字典中是否有与命令行参数提供的键匹配的键。</span><span class="sxs-lookup"><span data-stu-id="4735c-280">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="4735c-281">如果在字典中找到命令行键，则传回字典值（替换键）以设置配置。</span><span class="sxs-lookup"><span data-stu-id="4735c-281">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="4735c-282">对任何具有单划线 (`-`) 前缀的命令行键而言，交换映射都是必需的。</span><span class="sxs-lookup"><span data-stu-id="4735c-282">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="4735c-283">交换映射字典键规则：</span><span class="sxs-lookup"><span data-stu-id="4735c-283">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="4735c-284">交换必须以单划线 (`-`) 或双划线 (`--`) 开头。</span><span class="sxs-lookup"><span data-stu-id="4735c-284">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="4735c-285">交换映射字典不得包含重复键。</span><span class="sxs-lookup"><span data-stu-id="4735c-285">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="4735c-286">在以下示例中，`GetSwitchMappings` 方法允许命令行参数使用单划线 (`-`) 键前缀，并避免使用前导子键前缀。</span><span class="sxs-lookup"><span data-stu-id="4735c-286">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="4735c-287">如果不提供命令行参数，则由提供给 `AddInMemoryCollection` 的字典来设置配置值。</span><span class="sxs-lookup"><span data-stu-id="4735c-287">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="4735c-288">使用以下命令运行应用：</span><span class="sxs-lookup"><span data-stu-id="4735c-288">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="4735c-289">控制台窗口将显示：</span><span class="sxs-lookup"><span data-stu-id="4735c-289">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="4735c-290">使用以下命令传递配置设置：</span><span class="sxs-lookup"><span data-stu-id="4735c-290">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="4735c-291">控制台窗口将显示：</span><span class="sxs-lookup"><span data-stu-id="4735c-291">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="4735c-292">创建交换映射字典后，它将包含下表所示的数据：</span><span class="sxs-lookup"><span data-stu-id="4735c-292">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="4735c-293">键</span><span class="sxs-lookup"><span data-stu-id="4735c-293">Key</span></span>            | <span data-ttu-id="4735c-294">“值”</span><span class="sxs-lookup"><span data-stu-id="4735c-294">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="4735c-295">要使用字典演示键交换，请运行下列命令：</span><span class="sxs-lookup"><span data-stu-id="4735c-295">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="4735c-296">将交换命令行键。</span><span class="sxs-lookup"><span data-stu-id="4735c-296">The command-line keys are swapped.</span></span> <span data-ttu-id="4735c-297">控制台窗口将显示 `Profile:MachineName` 和 `App:MainWindow:Left` 的配置值：</span><span class="sxs-lookup"><span data-stu-id="4735c-297">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="4735c-298">web.config 文件</span><span class="sxs-lookup"><span data-stu-id="4735c-298">web.config file</span></span>

<span data-ttu-id="4735c-299">在 IIS 或 IIS Express 中托管应用时，需要 web.config 文件。</span><span class="sxs-lookup"><span data-stu-id="4735c-299">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="4735c-300">通过 web.config 中的设置，[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) 可以启动应用并配置其他 IIS 设置和模块。</span><span class="sxs-lookup"><span data-stu-id="4735c-300">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="4735c-301">如果 *web.config* 文件不存在，并且项目文件中包含 `<Project Sdk="Microsoft.NET.Sdk.Web">`，则发布项目时会在发布的输出（“发布”文件夹）中创建一个 *web.config* 文件。</span><span class="sxs-lookup"><span data-stu-id="4735c-301">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="4735c-302">有关详细信息，请参阅 [使用 IIS 在 Windows 上托管 ASP.NET Core](xref:host-and-deploy/iis/index#webconfig-file)。</span><span class="sxs-lookup"><span data-stu-id="4735c-302">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="access-configuration-during-startup"></a><span data-ttu-id="4735c-303">在启动期间访问配置</span><span class="sxs-lookup"><span data-stu-id="4735c-303">Access configuration during startup</span></span>

<span data-ttu-id="4735c-304">若要在启动时访问 `ConfigureServices` 或 `Configure` 中的配置，请参阅[应用程序启动](xref:fundamentals/startup)主题中的示例。</span><span class="sxs-lookup"><span data-stu-id="4735c-304">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="adding-configuration-from-an-external-assembly"></a><span data-ttu-id="4735c-305">从外部程序集添加配置</span><span class="sxs-lookup"><span data-stu-id="4735c-305">Adding configuration from an external assembly</span></span>

<span data-ttu-id="4735c-306">通过 [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) 实现，可在启动时从应用 `Startup` 类之外的外部程序集向应用添加增强功能。</span><span class="sxs-lookup"><span data-stu-id="4735c-306">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="4735c-307">有关详细信息，请参阅[从外部程序集增强应用](xref:fundamentals/configuration/platform-specific-configuration)。</span><span class="sxs-lookup"><span data-stu-id="4735c-307">For more information, see [Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a><span data-ttu-id="4735c-308">在 Razor 页面或 MVC 视图中访问配置</span><span class="sxs-lookup"><span data-stu-id="4735c-308">Access configuration in a Razor Page or MVC view</span></span>

<span data-ttu-id="4735c-309">若要访问 Razor 页面页或 MVC 视图中的配置设置，请为 [Microsoft.Extensions.Configuration 命名空间](/dotnet/api/microsoft.extensions.configuration)添加 [using 指令](xref:mvc/views/razor#using)（[C# 参考：using 指令](/dotnet/csharp/language-reference/keywords/using-directive)）并将 [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) 注入页面或视图。</span><span class="sxs-lookup"><span data-stu-id="4735c-309">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](/dotnet/api/microsoft.extensions.configuration) and inject [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) into the page or view.</span></span>

<span data-ttu-id="4735c-310">在 Razor 页面页中：</span><span class="sxs-lookup"><span data-stu-id="4735c-310">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel

@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="4735c-311">在 MVC 视图中：</span><span class="sxs-lookup"><span data-stu-id="4735c-311">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

## <a name="additional-notes"></a><span data-ttu-id="4735c-312">附加说明</span><span class="sxs-lookup"><span data-stu-id="4735c-312">Additional notes</span></span>

* <span data-ttu-id="4735c-313">调用 `ConfigureServices` 后才会设置依赖项注入 (DI)。</span><span class="sxs-lookup"><span data-stu-id="4735c-313">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="4735c-314">配置系统无法感知 DI。</span><span class="sxs-lookup"><span data-stu-id="4735c-314">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="4735c-315">`IConfiguration` 具有两项专用化：</span><span class="sxs-lookup"><span data-stu-id="4735c-315">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="4735c-316">`IConfigurationRoot` 用于根节点。</span><span class="sxs-lookup"><span data-stu-id="4735c-316">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="4735c-317">可以触发重载。</span><span class="sxs-lookup"><span data-stu-id="4735c-317">Can trigger a reload.</span></span>
  * <span data-ttu-id="4735c-318">`IConfigurationSection` 表示配置值的一节。</span><span class="sxs-lookup"><span data-stu-id="4735c-318">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="4735c-319">`GetSection` 和 `GetChildren` 方法返回 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="4735c-319">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="4735c-320">重新加载配置或要访问每个提供程序时，请使用 [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot)。</span><span class="sxs-lookup"><span data-stu-id="4735c-320">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="4735c-321">这两种情况都不常见。</span><span class="sxs-lookup"><span data-stu-id="4735c-321">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4735c-322">其他资源</span><span class="sxs-lookup"><span data-stu-id="4735c-322">Additional resources</span></span>

* [<span data-ttu-id="4735c-323">选项</span><span class="sxs-lookup"><span data-stu-id="4735c-323">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="4735c-324">使用多个环境</span><span class="sxs-lookup"><span data-stu-id="4735c-324">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="4735c-325">在开发期间安全存储应用机密</span><span class="sxs-lookup"><span data-stu-id="4735c-325">Safe storage of app secrets in development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="4735c-326">在 ASP.NET Core 中托管</span><span class="sxs-lookup"><span data-stu-id="4735c-326">Host in ASP.NET Core</span></span>](xref:fundamentals/host/index)
* [<span data-ttu-id="4735c-327">依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="4735c-327">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="4735c-328">Azure Key Vault 配置提供程序</span><span class="sxs-lookup"><span data-stu-id="4735c-328">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
