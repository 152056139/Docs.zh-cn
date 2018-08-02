---
title: 在 ASP.NET Core 的 azure 密钥保管库配置提供程序
author: guardrex
description: 了解如何使用 Azure 密钥保管库配置提供程序来配置应用程序使用在运行时加载的名称 / 值对。
ms.author: riande
ms.date: 08/01/2018
uid: security/key-vault-configuration
ms.openlocfilehash: 6474b9f5cb9e441854565a7891c4aac7f781c810
ms.sourcegitcommit: 571d76fbbff05e84406b6d909c8fe9cbea2c8ff1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2018
ms.locfileid: "39410125"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>在 ASP.NET Core 的 azure 密钥保管库配置提供程序

通过[Luke Latham](https://github.com/guardrex)和[Andrew Stanton-nurse](https://github.com/anurse)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

查看或下载 2.x 示例代码：

* [基本示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x)([如何下载](xref:tutorials/index#how-to-download-a-sample))-应用到读取机密值。
* [密钥名称前缀示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x)([如何下载](xref:tutorials/index#how-to-download-a-sample))-读取机密值使用密钥名称前缀表示版本的应用，这允许您加载一组不同的每个应用程序版本的机密值。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

查看或下载 1.x 示例代码：

* [基本示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x)([如何下载](xref:tutorials/index#how-to-download-a-sample))-应用到读取机密值。
* [密钥名称前缀示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x)([如何下载](xref:tutorials/index#how-to-download-a-sample))-读取机密值使用密钥名称前缀表示版本的应用，这允许您加载一组不同的每个应用程序版本的机密值。

---

本文档介绍如何使用[Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/)配置提供程序将从 Azure Key Vault 机密中加载应用配置值。 Azure Key Vault 是基于云的服务，可帮助你保护加密密钥和机密使用的应用和服务。 常见的方案包括控制对敏感的配置数据的访问并符合 fips 140-2 的要求级别 2 硬件安全模块 (HSM) 时验证存储配置数据。 此功能仅适用于面向 ASP.NET Core 1.1 的应用或更高版本。

## <a name="package"></a>Package

若要使用的提供程序，添加到引用[Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)包。

## <a name="app-configuration"></a>应用配置

你可以浏览的提供程序[示例应用](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples)。 建立密钥保管库并在保管库中创建机密后，示例应用安全地将机密值加载到其配置并在网页中显示它们。

提供程序添加到应用程序的配置中使用`AddAzureKeyVault`扩展。 在示例应用中，则该扩展将从加载的三个配置值*appsettings.json*文件。

| 应用设置    | 描述                    | 示例                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Azure 密钥保管库名称           | contosovault                                 |
| `ClientId`     | Azure Active Directory 应用 Id  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Azure Active Directory 应用密钥 | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1)]

## <a name="creating-key-vault-secrets-and-loading-configuration-values-basic-sample"></a>创建密钥保管库机密和加载配置值 （basic 示例）

1. 创建密钥保管库并将设置 Azure Active Directory (Azure AD) 应用程序中的指南[开始使用 Azure 密钥保管库](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)。
   * 将机密添加到密钥保管库使用[AzureRM 密钥保管库 PowerShell 模块](/powershell/module/azurerm.keyvault)从可用[PowerShell 库](https://www.powershellgallery.com/packages/AzureRM.KeyVault)，则[Azure 密钥保管库 REST API](/rest/api/keyvault/)，或者[Azure 门户](https://portal.azure.com/)。 为创建的机密*手动*或*证书*机密。 *证书*机密使用的应用和服务证书，但不是支持配置提供程序。 应使用*手动*选项可创建带有配置提供程序使用的名称-值对机密。
     * 以名称-值对的形式创建简单的机密。 Azure 密钥保管库机密名称被限制为字母数字字符和短划线。
     * 层次结构的值 （配置节） 使用`--`（两个短划线） 作为分隔符，在该示例。 通常用于分隔的子项中的一部分的冒号[ASP.NET Core 配置](xref:fundamentals/configuration/index)，机密名称中不允许出现。 因此，两个短划线的使用和机密加载到应用的配置时交换的冒号。
     * 创建两个*手动*具有以下名称 / 值对的机密。 第一个机密是一个简单的名称和值，并第二个密钥使用部分和子项中的机密名称创建机密值：
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * 与 Azure Active Directory 中注册示例应用程序。
   * 授权应用访问密钥保管库。 当你使用`Set-AzureRmKeyVaultAccessPolicy`PowerShell cmdlet 来授权应用程序访问密钥保管库，提供`List`并`Get`对使用机密的访问`-PermissionsToSecrets list,get`。

2. 更新应用程序的*appsettings.json*包含的值的文件`Vault`， `ClientId`，和`ClientSecret`。
3. 运行示例应用，获取从其配置值`IConfigurationRoot`具有作为机密名称相同的名称。
   * 非层次结构值： 的值`SecretName`一起被获取`config["SecretName"]`。
   * 层次结构的值 （部分）： 使用`:`（冒号） 表示法或`GetSection`扩展方法。 使用任一方法获取的配置值：
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

运行应用时，网页显示加载的机密值：

![通过使用 Azure 密钥保管库配置提供程序加载浏览器窗口，其中显示密钥值](key-vault-configuration/_static/sample1.png)

## <a name="creating-prefixed-key-vault-secrets-and-loading-configuration-values-key-name-prefix-sample"></a>创建带前缀的密钥保管库机密和加载配置值 （密钥的名称的前缀的示例）

`AddAzureKeyVault` 此外提供了接受的实现的重载`IKeyVaultSecretManager`，可用于控制如何密钥保管库机密将转换为配置项。 例如，可以实现的接口来加载基于你在应用启动时提供的前缀值的机密值。 这可以使您例如，若要加载基于应用的版本的密钥。

> [!WARNING]
> 若要将多个应用的机密放入同一个密钥保管库或将环境机密不使用在密钥保管库机密的前缀 (例如，*开发*而不是*生产*机密) 到相同保管库。 我们建议，不同的应用程序和开发/生产环境使用单独的密钥保管库来隔离应用环境的最高级别的安全性。

使用第二个示例应用程序，在 key vault 中创建机密`5000-AppSecret`（密钥保管库机密名称中不允许句点） 表示您的应用程序的版本为 5.0.0.0 应用程序密码。 有关另一个版本，5.1.0.0，创建的机密`5100-AppSecret`。 每个应用程序版本将其自己的机密值加载到作为其配置`AppSecret`、 在加载机密的版本剥离。 该示例的实现如下所示：

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=18)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

`Load`方法调用循环访问保管库机密以找出具有版本前缀的提供程序算法。 当版本前缀找到`Load`，该算法使用`GetKey`方法返回的机密名称的配置名称。 它剥离机密的名称从版本前缀，并返回到应用程序的配置名称-值对的加载的机密名称的其余部分。

当您实现这种方法：

1. 密钥保管库机密进行加载。
2. 字符串密码`5000-AppSecret`匹配。
3. 版本`5000`去除 （替换为短划线），从离开的键名`AppSecret`机密值加载到应用的配置。

> [!NOTE]
> 您还可以提供您自己`KeyVaultClient`实现`AddAzureKeyVault`。 提供自定义客户端可以共享单个实例的配置提供程序和您的应用程序的其他部件之间的客户端。

1. 创建密钥保管库并将设置 Azure Active Directory (Azure AD) 应用程序中的指南[开始使用 Azure 密钥保管库](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)。
   * 将机密添加到密钥保管库使用[AzureRM 密钥保管库 PowerShell 模块](/powershell/module/azurerm.keyvault)从可用[PowerShell 库](https://www.powershellgallery.com/packages/AzureRM.KeyVault)，则[Azure 密钥保管库 REST API](/rest/api/keyvault/)，或者[Azure 门户](https://portal.azure.com/)。 为创建的机密*手动*或*证书*机密。 *证书*机密使用的应用和服务证书，但不是支持配置提供程序。 应使用*手动*选项可创建带有配置提供程序使用的名称-值对机密。
     * 层次结构的值 （配置节） 使用`--`（两个短划线） 作为分隔符。
     * 创建两个*手动*具有以下名称 / 值对的机密：
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * 与 Azure Active Directory 中注册示例应用程序。
   * 授权应用访问密钥保管库。 当你使用`Set-AzureRmKeyVaultAccessPolicy`PowerShell cmdlet 来授权应用程序访问密钥保管库，提供`List`并`Get`对使用机密的访问`-PermissionsToSecrets list,get`。

2. 更新应用程序的*appsettings.json*包含的值的文件`Vault`， `ClientId`，和`ClientSecret`。
3. 运行示例应用，获取从其配置值`IConfigurationRoot`具有作为带前缀的机密名称相同的名称。 在此示例中，前缀是应用的版本，提供给`PrefixKeyVaultSecretManager`添加 Azure Key Vault 配置提供程序时。 值`AppSecret`一起被获取`config["AppSecret"]`。 由应用生成的网页显示加载的值：

   ![浏览器窗口中显示为 5.0.0.0 应用的版本时，通过使用 Azure 密钥保管库配置提供程序加载的机密值](key-vault-configuration/_static/sample2-1.png)

4. 更改中的项目文件中的应用程序集版本`5.0.0.0`到`5.1.0.0`并再次运行应用。 这一次，返回的机密值是`5.1.0.0_secret_value`。 由应用生成的网页显示加载的值：

   ![浏览器窗口，其中显示 5.1.0.0 应用的版本时，通过使用 Azure 密钥保管库配置提供程序加载的机密值](key-vault-configuration/_static/sample2-2.png)

## <a name="controlling-access-to-the-clientsecret"></a>控制对客户端密码的访问

使用[机密管理器工具](xref:security/app-secrets)维护`ClientSecret`外部项目源树。 使用机密管理器中，你将应用程序机密与特定项目相关联并跨多个项目共享它们。

在开发环境支持证书中的.NET Framework 应用时，可以使用 X.509 证书验证到 Azure 密钥保管库。 X.509 证书的私钥由操作系统管理。 有关详细信息，请参阅[使用证书而不是客户端密码进行身份验证](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret)。 使用`AddAzureKeyVault`接受重载`X509Certificate2`(`_env`在下面的示例：

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["Vault"],
    builtConfig["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="reloading-secrets"></a>重新加载机密

机密缓存，直至`IConfigurationRoot.Reload()`调用。 已过期，已禁用，并且已更新密钥保管库中的机密不遵循之前应用此`Reload`执行。

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>已禁用和过期的机密

已禁用并已过期机密引发`KeyVaultClientException`。 若要防止应用引发，将为您的应用程序或更新已禁用或已过期机密。

## <a name="troubleshooting"></a>疑难解答

如果应用程序无法加载使用提供程序的配置，一条错误消息写入到[ASP.NET 日志记录基础结构](xref:fundamentals/logging/index)。 以下条件下将不加载配置：

* 应用未正确配置 Azure Active Directory 中。
* 密钥保管库不存在于 Azure 密钥保管库。
* 应用程序无权访问密钥保管库。
* 访问策略不包括`Get`和`List`权限。
* 在密钥保管库，配置数据 （名称 / 值对） 是错误命名为，缺少，禁用，或已过期。
* 应用了错误的密钥保管库名称 (`Vault`)，Azure AD 应用程序 Id (`ClientId`)，或 Azure AD 密钥 (`ClientSecret`)。
* Azure AD 密钥 (`ClientSecret`) 已过期。
* 配置密钥 （名称） 不正确的应用中的想要加载的值。

## <a name="additional-resources"></a>其他资源

* <xref:fundamentals/configuration/index>
* [Microsoft Azure： 密钥保管库](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure： 密钥保管库文档](/azure/key-vault/)
* [如何生成和传输受 HSM 保护密钥的 Azure 密钥保管库](/azure/key-vault/key-vault-hsm-protected-keys)
* [KeyVaultClient 类](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
