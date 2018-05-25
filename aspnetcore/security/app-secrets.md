---
title: 安全存储中 ASP.NET Core 中开发的应用程序机密
author: rick-anderson
description: 了解如何存储和检索在 ASP.NET Core 应用程序开发期间为应用程序机密的敏感信息。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: ece2bf541df2b4acac60a88767cc57ede473bd49
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>安全存储中 ASP.NET Core 中开发的应用程序机密

通过[Rick Anderson](https://twitter.com/RickAndMSFT)， [Daniel Roth](https://github.com/danroth27)，和[Scott Addie](https://github.com/scottaddie)

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

本文档介绍用于存储和检索敏感数据的 ASP.NET Core 应用程序开发过程的技术。 你应永远不会将密码或其他敏感数据存储在源代码中，并且你不应在开发过程中使用生产机密或测试模式。 你可以存储和保护与 Azure 的测试和生产机密[Azure 密钥保管库配置提供程序](xref:security/key-vault-configuration)。

## <a name="environment-variables"></a>环境变量

使用环境变量以避免在代码中或在本地配置文件中的应用程序机密存储。 环境变量重写所有以前指定的配置源的配置的值。

::: moniker range="<= aspnetcore-1.1"
通过调用配置的环境变量值读取[AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables)中`Startup`构造函数：

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

请考虑在其中一个 ASP.NET 核心 web 应用**单个用户帐户**已启用安全性。 默认数据库连接字符串包含在项目的*appsettings.json*文件与键`DefaultConnection`。 默认连接字符串用于 LocalDB，在用户模式下运行，而且不需要密码。 应用程序在部署期间，`DefaultConnection`密钥值可以用环境变量的值重写。 环境变量可以存储敏感的凭据与完整的连接字符串。

> [!WARNING]
> 环境变量通常存储在未加密的纯文本。 如果计算机或进程受到攻击，则可以通过不受信任方访问环境变量。 可能需要更多措施，防止用户的机密信息泄露。

## <a name="secret-manager"></a>密码管理器

密码管理器工具在 ASP.NET Core 项目的开发过程中存储敏感数据。 在此上下文中，一种敏感数据是应用程序密钥。 应用程序机密存储在单独的位置，从项目树中。 应用程序机密是与特定项目关联，或共享跨多个项目。 应用程序机密不签入源控件。

> [!WARNING]
> 密码管理器工具不加密存储的机密信息，并不应被视为受信任存储区。 它是仅限开发目的。 键和值存储在用户配置文件目录中的 JSON 配置文件中。

## <a name="how-the-secret-manager-tool-works"></a>密码管理器工具的工作原理

密码管理器工具避开实现详细信息，例如哪里和如何存储值。 无需知道这些实现的详细信息，可以使用该工具。 值存储在本地计算机上的 JSON 配置文件受系统保护用户配置文件文件夹中：

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

文件系统路径：

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[macOS](#tab/macos)

文件系统路径：

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

文件系统路径：

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

在前面文件路径中，将`<user_secrets_id>`与`UserSecretsId`中指定的值 *.csproj*文件。

不编写代码所依赖的位置或使用密码管理器工具中保存的数据的格式。 这些实现的详细信息如有更改。 例如，密钥值不加密，但是可能在将来。

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a>安装机密管理器工具

密钥管理器工具是与.NET 核心 CLI 截至.NET 核心 SDK 2.1.300 捆绑。 有关.NET 核心 SDK 2.1.300 之前的版本中，工具安装是必需的。

> [!TIP]
> 运行`dotnet --version`从命令行界面中，若要查看已安装的.NET 核心 SDK 版本号。

如果正在使用的.NET 核心 SDK 包括的工具，将显示警告：

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

安装[Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core 项目中的 NuGet 包。 例如：

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

在验证工具的安装命令行界面中执行以下命令：

```console
dotnet user-secrets -h
```

密码管理器工具显示示例用法、 选项和命令帮助：

```console
Usage: dotnet user-secrets [options] [command]

Options:
  -?|-h|--help                        Show help information
  --version                           Show version information
  -v|--verbose                        Show verbose output
  -p|--project <PROJECT>              Path to project. Defaults to searching the current directory.
  -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'.
  --id                                The user secret ID to use.

Commands:
  clear   Deletes all the application secrets
  list    Lists all the application secrets
  remove  Removes the specified user secret
  set     Sets the user secret to the specified value

Use "dotnet user-secrets [command] --help" for more information about a command.
```

> [!NOTE]
> 你必须在与位于同一目录 *.csproj*文件以运行中定义的工具 *.csproj*文件的`DotNetCliToolReference`元素。
::: moniker-end

## <a name="set-a-secret"></a>设置机密

密码管理器工具进行存储用户配置文件中的特定于项目的配置设置操作。 若要使用用户的机密信息，定义`UserSecretsId`中的元素`PropertyGroup`的 *.csproj*文件。 值`UserSecretsId`任意的但是唯一到项目。 开发人员通常应生成的 GUID `UserSecretsId`。

::: moniker range="<= aspnetcore-1.1"
[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end

> [!TIP]
> 在 Visual Studio 中，右键单击解决方案资源管理器中的项目，然后选择**管理用户的机密信息**从上下文菜单。 此手势添加`UserSecretsId`元素，为填充 GUID， *.csproj*文件。 Visual Studio 将打开*secrets.json*在文本编辑器中的文件。 内容替换*secrets.json*要存储的键 / 值对。 例如：[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]

定义一个键和其值组成应用程序密钥。 密钥是与项目的关联`UserSecretsId`值。 例如，从在其中的目录运行以下命令 *.csproj*文件存在：

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

在前面的示例中，冒号表示`Movies`是一个对象文本替换`ServiceApiKey`属性。

密码管理器工具可以过使用从其他目录。 使用`--project`选项提供文件系统路径，在此处 *.csproj*文件存在。 例如：

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a>设置多个机密

可以通过管道传递到的 JSON 设置机密一批`set`命令。 在下面的示例中， *input.json*文件的内容通过管道传递给`set`命令。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

打开命令 shell，并执行以下命令：

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

打开命令 shell，并执行以下命令：

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

打开命令 shell，并执行以下命令：

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>访问机密

::: moniker range="<= aspnetcore-1.1"
[ASP.NET 核心配置 API](xref:fundamentals/configuration/index)提供对机密 Manager 机密的访问。 安装[Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 包。

添加用户机密配置源通过调用[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)中`Startup`构造函数：

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[ASP.NET 核心配置 API](xref:fundamentals/configuration/index)提供对机密 Manager 机密的访问。 如果你的项目面向.NET Framework，安装[Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 包。

在 ASP.NET 核心 2.0 或更高版本，用户机密配置源时，自动添加在开发模式下项目调用[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)初始化带有预配置的默认值的主机的新实例。 `CreateDefaultBuilder` 调用[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)时[EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)是[开发](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

当`CreateDefaultBuilder`不在主机构造过程中调用，将通过调用该用户机密配置源添加[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)中`Startup`构造函数：

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

可以通过检索用户的机密信息`Configuration`API:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a>包含密码的字符串替换

以纯文本格式存储密码是危险的。 例如，数据库连接字符串存储在*appsettings.json*可能包括指定用户的密码：

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

更安全的方法是将密码存储的机密形式。 例如：

```console
dotnet user-secrets set "DbPassword" "pass123"
```

替换中的密码*appsettings.json*其占位符。 在下面的示例中，`{0}`用作到窗体的占位符[复合格式字符串](/dotnet/standard/base-types/composite-formatting#composite-format-string)。

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

机密的值可以将其插入占位符以完成连接字符串：

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]
::: moniker-end

## <a name="list-the-secrets"></a>列出机密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

从在其中的目录运行以下命令 *.csproj*文件存在：

```console
dotnet user-secrets list
```

显示以下输出：

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

在前面的示例中，在项名称中的冒号表示内的对象层次结构*secrets.json*。

## <a name="remove-a-single-secret"></a>删除单个机密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

从在其中的目录运行以下命令 *.csproj*文件存在：

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

应用程序的*secrets.json*已修改文件，以删除与关联的键 / 值对`MoviesConnectionString`密钥：

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

运行`dotnet user-secrets list`显示以下消息：

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>删除所有机密

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

从在其中的目录运行以下命令 *.csproj*文件存在：

```console
dotnet user-secrets clear
```

已从删除应用程序的所有用户机密*secrets.json*文件：

```json
{}
```

运行`dotnet user-secrets list`显示以下消息：

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>其他资源

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
