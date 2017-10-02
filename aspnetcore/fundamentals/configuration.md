---
title: "ASP.NET Core 中的配置"
author: rick-anderson
description: "了解如何使用配置 API 将来自多个源 ASP.NET Core 应用配置。"
keywords: "ASP.NET 核心，配置中，JSON 配置"
ms.author: riande
manager: wpickett
ms.date: 6/24/2017
ms.topic: article
ms.assetid: b3a5984d-e172-42eb-8a48-547e4acb6806
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration
ms.openlocfilehash: ca6b62dd4699536b24c3422a2a51fc3fe1744f0a
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2017
---
# <a name="configuration-in-aspnet-core"></a>ASP.NET Core 中的配置

[Rick Anderson](https://twitter.com/RickAndMSFT)，[标记 Michaelis](http://intellitect.com/author/mark-michaelis/)， [Steve Smith](https://ardalis.com/)， [Daniel Roth](https://github.com/danroth27)，和[Luke Latham](https://github.com/guardrex)

配置 API 提供一种方法配置应用程序中基于名称-值对的列表。 在运行时从多个源读取配置。 名称-值对可以分组到多级的层次结构。 有配置提供程序：

* 文件格式 （INI、 JSON 和 XML）
* 命令行自变量
* 环境变量
* 内存中的.NET 对象
* 一个加密的用户存储区
* [Azure 密钥保管库](xref:security/key-vault-configuration)
* 自定义提供程序，你安装或在创建

每个配置值将映射到字符串键。 若要反序列化到自定义设置的内置绑定支持[POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)对象 （具有属性的简单.NET 类）。

[查看或下载的示例代码](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)([如何下载](xref:tutorials/index#how-to-download-a-sample))

## <a name="simple-configuration"></a>简单的配置

以下控制台应用程序使用 JSON 配置提供程序：

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

应用程序读取，并显示以下配置设置：

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

配置由在其中节点以冒号分隔的名称-值对的分层列表组成。 若要检索一个值，请访问`Configuration`与相应的项的键的索引器：

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

若要使用 JSON 格式配置源中的数组，使用作为一部分的冒号分隔的字符串的数组索引。 下面的示例获取在前面的第一项的名称`wizards`数组：

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

写入到生成的名称-值对中`Configuration`提供程序是**不**保持不变，但是，你可以创建的自定义提供程序将保存值。 请参阅[自定义配置提供程序](xref:fundamentals/configuration#custom-config-providers)。

上面的示例使用配置索引器可以读取值。 外部访问配置`Startup`，使用[选项模式](xref:fundamentals/configuration#options-config-objects)。 *选项模式*本文后面所示。

它是通常具有不同的配置设置的不同的环境，例如，开发、 测试和生产。 `CreateDefaultBuilder` ASP.NET Core 2.x 应用程序中的扩展方法 (或使用`AddJsonFile`和`AddEnvironmentVariables`直接在 ASP.NET Core 1.x 应用程序) 将配置提供程序添加用于读取 JSON 文件和系统配置源：

* *appsettings.json*
* *appsettings。\<EnvironmentName >.json*
* 环境变量

请参阅[AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions)有关参数的说明。 `reloadOnChange`仅支持在 ASP.NET 核心 1.1 和更高版本。 

配置源中指定它们的顺序读取。 在上面的代码中，最后读取环境变量。 任何通过环境设置的配置值会替换在两个以前的提供程序中设置。

环境通常设置为其中一个`Development`， `Staging`，或`Production`。 请参阅[使用多个环境](environments.md)有关详细信息。

配置注意事项：

* `IOptionsSnapshot`当更改时，可以重新加载配置数据。 使用`IOptionsSnapshot`如果需要重新加载配置数据。  请参阅[IOptionsSnapshot](#ioptionssnapshot)有关详细信息。
* 配置密钥是区分大小写。
* 一种最佳做法是上一次，指定环境变量，使本地环境可以重写在已部署的配置文件中设置的任何内容。
* **永远不会**在配置提供程序代码或纯文本配置文件中存储密码或其他敏感数据。 请不要在你的开发中使用生产机密或测试环境。 相反，指定外部项目树中，机密，以便它们不会意外地提交到你的存储库。 详细了解[使用多个环境](environments.md)和管理[安全存储在开发过程中的应用程序机密](../security/app-secrets.md)。
* 如果`:`不能为系统中所用环境变量中，替换`:`与`__`（双下划线）。

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a>使用选项和配置对象

选项模式使用自定义选项类来表示一组相关的设置。 我们建议在你的应用程序中创建分离的类为每个功能。 分离的类遵循：

* [接口分隔原则 (ISP)](http://deviq.com/interface-segregation-principle/) ： 类仅取决于它们使用的配置设置。
* [关注点分离](http://deviq.com/separation-of-concerns/)： 你的应用程序的不同部分的设置不是依赖或与另一个耦合。

选项类必须为非抽象具有一个公共的无参数构造函数。 例如: 

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name=options-example></a>

在下面的代码中，启用 JSON 配置提供程序。 `MyOptions`类是已添加到服务容器并绑定到配置。

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

以下[控制器](../mvc/controllers/index.md)使用[构造函数依赖关系注入](xref:fundamentals/dependency-injection#what-is-dependency-injection)上[ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1)访问设置：

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

替换为以下*appsettings.json*文件：

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

`HomeController.Index`方法返回`option1 = value1_from_json, option2 = 2`。

典型的应用程序不会将整个配置绑定到单个选项文件。 稍后我将介绍如何使用`GetSection`将绑定到一个部分。

在下面的代码中，第二个`IConfigureOptions<TOptions>`服务添加到服务容器。 它使用委托来配置的绑定`MyOptions`。

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

你可以添加多个配置提供程序。 配置提供程序 NuGet 包中。 它们会按它们已注册的顺序应用。

每次调用`Configure<TOptions>`添加`IConfigureOptions<TOptions>`服务到服务容器。 在前面的示例中，值`Option1`和`Option2`同时中指定*appsettings.json* -但的值`Option1`配置委托来重写。 

启用多个配置服务后，最后一个配置源指定"wins"（设置配置值）。 在前面的代码中，`HomeController.Index`方法返回`option1 = value1_from_action, option2 = 2`。

绑定到配置的选项时, 选项类型中的每个属性绑定到窗体的配置键`property[:sub-property:]`。 例如，`MyOptions.Option1`属性绑定到键`Option1`，这从读取`option1`中的属性*appsettings.json*。 在本文后面显示了子属性示例。

在下面的代码中，第三个`IConfigureOptions<TOptions>`服务添加到服务容器。 它将绑定`MySubOptions`的部分`subsection`的*appsettings.json*文件：

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

注意： 此扩展方法要求`Microsoft.Extensions.Options.ConfigurationExtensions`NuGet 包。

使用以下*appsettings.json*文件：

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

`MySubOptions`类：

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

替换为以下`Controller`:

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

`subOption1 = subvalue1_from_json, subOption2 = 200`返回。

也可以提供视图模型中的选项，或将注入`IOptions<TOptions>`直接到视图：

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a>IOptionsSnapshot

*需要 ASP.NET Core 1.1 或更高版本。*

`IOptionsSnapshot`当配置文件发生更改时重新加载配置数据的支持。 它具有最小的开销。 使用`IOptionsSnapshot`与`reloadOnChange: true`，选项绑定到`IConfiguration`并更改时重新加载。

下面的示例演示如何新`IOptionsSnapshot`后创建*config.json*更改。 对服务器请求将返回相同时间*config.json*具有**不**更改。 之后的第一个请求*config.json*做的更改将显示新的时间。

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

下图显示服务器输出：

![浏览器图像显示"上次更新时间： 2016 年 11 月 22 日下午 4:43"](configuration/_static/first.png)

刷新浏览器不会更改消息值或显示的时间 (时*config.json*仍是如此)。

进行更改并保存*config.json*然后刷新浏览器：

![浏览器图像显示"上次更新到 e: 2016 年 11 月 22 日下午 4:53"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>内存中提供程序并绑定到 POCO 类

下面的示例演示如何使用内存中提供程序并将绑定到类：

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

配置值将返回为字符串，但绑定可使对象的构造。 绑定允许您检索 POCO 对象或甚至是整个对象图。 下面的示例演示如何将绑定到`MyWindow`，并且与 ASP.NET 核心 MVC 应用程序使用选项模式：

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

绑定中的自定义类`ConfigureServices`生成主机时：

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

显示从设置`HomeController`:

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a>GetValue

下面的示例演示如何[GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_)扩展方法：

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

ConfigurationBinder`GetValue<T>`方法允许你指定默认值 (在此示例中为 80)。 `GetValue<T>`用于简单的方案，不会绑定到整个部分。 `GetValue<T>`获取标量值从`GetSection(key).Value`转换为特定类型。

## <a name="binding-to-an-object-graph"></a>绑定到对象图

你可以以递归方式绑定到类中每个对象。 请考虑以下`AppOptions`类：

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

下面的示例将绑定到`AppOptions`类：

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET 核心 1.1**和更高版本可以使用`Get<T>`，这适用于整个部分。 `Get<T>`可以是比使用多个 convienent `Bind`。 下面的代码演示如何使用`Get<T>`与上面的示例：

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

使用以下*appsettings.json*文件：

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

该程序显示`Height 11`。

下面的代码可以用于单元测试配置：

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

    var options = new AppOptions();
    config.GetSection("App").Bind(options);

    Assert.Equal("Rick", options.Profile.Machine);
    Assert.Equal(11, options.Window.Height);
    Assert.Equal(11, options.Window.Width);
    Assert.Equal("connectionstring", options.Connection.Value);
}
```

<a name=custom-config-providers></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a>实体框架自定义提供程序的基本示例

在本部分中，创建的基本配置提供程序从使用 EF 数据库中读取名称-值对。 

定义`ConfigurationValue`在数据库中存储配置值的实体：

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

添加`ConfigurationContext`存储和访问配置的值：

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

创建一个类，实现[IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

创建自定义配置提供程序通过继承[ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider)。  为空时，配置提供程序初始化数据库：

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

运行示例时，会显示数据库 （"value_from_ef_1"和"value_from_ef_2"） 中突出显示的值。

你可以添加`EFConfigSource`将该配置源添加的扩展方法：

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

下面的代码演示如何使用自定义`EFConfigProvider`:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

请注意此示例将添加自定义`EFConfigProvider`后 JSON 提供程序，因此数据库中的任何设置将替代设置从*appsettings.json*文件。

使用以下*appsettings.json*文件：

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

将显示以下内容：

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>命令行配置提供程序

[命令行配置提供程序](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider)接收命令行自变量在运行时配置的键 / 值对。

[查看或下载命令行配置示例](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample/CommandLine)

### <a name="setting-up-the-provider"></a>设置提供程序

# <a name="basic-configurationtabbasicconfiguration"></a>[基本配置](#tab/basicconfiguration)

若要激活命令行配置，请调用`AddCommandLine`实例上的扩展方法[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program.cs?highlight=18,21)]

运行代码，将显示以下输出：

```console
MachineName: MairaPC
Left: 1980
```

键 / 值对的自变量传递命令行上更改的值`Profile:MachineName`和`App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

控制台窗口会显示：

```console
MachineName: BartPC
Left: 1979
```

若要重写使用命令行配置提供其他配置提供程序配置，调用`AddCommandLine`中的最后一个`ConfigurationBuilder`:

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

典型的 ASP.NET Core 2.x 应用程序使用的静态简便方法`CreateDefaultBuilder`来生成主机：

[!code-csharp[Main](configuration/sample_snapshot/Program.cs?highlight=12)]

`CreateDefaultBuilder`加载从的可选配置*appsettings.json*， *appsettings。 {环境}.json*，[用户机密](xref:security/app-secrets)(在`Development`环境)，环境变量和命令行参数。 最后调用命令行配置提供程序。 上次调用提供程序允许在运行时以替代配置设置的其他配置提供程序传递的命令行自变量调用更早版本。

请注意，对于*appsettings*文件`reloadOnChange`已启用。 命令行自变量被重写，如果匹配的配置值在*appsettings*文件在应用启动后已更改。

> [!NOTE]
> 作为使用的替代方法`CreateDefaultBuilder`方法，使用创建主机[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)和手动生成配置[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder)中 ASP.NET Core 支持 2.x。 请参阅 ASP.NET Core 1.x 选项卡的详细信息。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

创建[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder)并调用`AddCommandLine`要使用命令行配置提供程序方法。 上次调用提供程序允许在运行时以替代配置设置的其他配置提供程序传递的命令行自变量调用更早版本。 向其应用配置[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)与`UseConfiguration`方法：

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>参数

在命令行上传递自变量必须符合以下表所示的两种格式之一。

| 参数格式                                                     | 示例        |
| ------------------------------------------------------------------- | :------------: |
| 单个参数： 用一个等号分隔的键-值对 (`=`) | `key1=value`   |
| 两个参数的序列： 键 / 值对，并用空格分隔    | `/key1 value1` |

**单个自变量**

值必须跟一个等号 (`=`)。 该值可以为 null (例如， `mykey=`)。

键可能具有的前缀。

| 键前缀               | 示例         |
| ------------------------ | :-------------: |
| 没有任何前缀                | `key1=value1`   |
| 单一短划线 (`-`) &#8224; | `-key2=value2`  |
| 两个短划线 (`--`)        | `--key3=value3` |
| 正斜杠 (`/`)      | `/key4=value4`  |

&#8224;具有单个仪表前缀的键 (`-`) 必须在提供[切换映射](#switch-mappings)、 下面所述。

示例命令：

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

注意： 如果`-key1`不存在于[切换映射](#switch-mappings)赋予配置提供程序，`FormatException`引发。

**两个自变量的序列**

值不能为 null，并且必须符合以空格分隔的密钥。

密钥必须具有的前缀。

| 键前缀               | 示例         |
| ------------------------ | :-------------: |
| 单一短划线 (`-`) &#8224; | `-key1 value1`  |
| 两个短划线 (`--`)        | `--key2 value2` |
| 正斜杠 (`/`)      | `/key3 value3`  |

&#8224;具有单个仪表前缀的键 (`-`) 必须在提供[切换映射](#switch-mappings)、 下面所述。

示例命令：

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

注意： 如果`-key1`不存在于[切换映射](#switch-mappings)赋予配置提供程序，`FormatException`引发。

### <a name="duplicate-keys"></a>重复键

如果提供了重复的键，则使用最后一个键 / 值对。

### <a name="switch-mappings"></a>切换映射

手动生成配置时`ConfigurationBuilder`，你可以根据需要提供交换机映射字典`AddCommandLine`方法。 交换机映射可用于提供密钥名称更换逻辑。

当使用交换机映射字典时，字典将检查与提供的命令行自变量的键匹配的键。 如果在字典中找到命令行的键，字典值 （密钥更换） 传递回来，以便将配置设置。 交换机映射是所必需的任何命令行的键，加上单个短划线 (`-`)。

切换映射字典键规则：

* 交换机必须以短划线开头 (`-`) 或双短划线 (`--`)。
* 交换机映射字典不能包含重复键。

在下面的示例中，`GetSwitchMappings`方法允许你使用单个仪表的命令行自变量 (`-`) 密钥前缀，并避免前导子项的前缀。

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs?highlight=10-19,32)]

未提供命令行参数，字典提供给`AddInMemoryCollection`设置配置值。 使用以下命令运行该应用程序：

```console
dotnet run
```

控制台窗口会显示：

```console
MachineName: RickPC
Left: 1980
```

使用以下方法来配置设置中传递：

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

控制台窗口会显示：

```console
MachineName: DahliaPC
Left: 1984
```

创建交换机映射字典后，它包含下表中显示的数据。

| 键            | 值                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

若要演示密钥切换使用字典，请运行以下命令：

```console
dotnet run -MachineName=ChadPC -Left=1988
```

将交换的命令行的键。 控制台窗口会显示的配置值`Profile:MachineName`和`App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a>Web.config 文件

A *web.config*文件是必需的当你在 IIS 或 IIS Express 应用程序承载。 *web.config*开启 AspNetCoreModule IIS 启动你的应用中。 中的设置*web.config*启用 IIS 来启动你的应用和配置其他 IIS 设置和模块中 AspNetCoreModule。 如果你使用的 Visual Studio 将删除*web.config*，Visual Studio 将创建一个新。

## <a name="additional-notes"></a>附加说明

* 依赖关系注入 (DI) 未设置截止日期之后`ConfigureServices`调用。
* 配置系统不是 DI 感知。
* `IConfiguration`具有两个专用化：
  * `IConfigurationRoot`用于根节点。 可以触发重新加载。
  * `IConfigurationSection`表示一个节的配置值。 `GetSection`和`GetChildren`方法返回`IConfigurationSection`。

## <a name="additional-resources"></a>其他资源

* [使用多个环境](environments.md)
* [在开发期间安全存储应用密钥](../security/app-secrets.md)
* [在 ASP.NET Core 中承载](xref:fundamentals/hosting)
* [依赖关系注入](dependency-injection.md)
* [Azure Key Vault 配置提供程序](xref:security/key-vault-configuration)
