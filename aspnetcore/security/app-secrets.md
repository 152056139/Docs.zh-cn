---
title: 安全存储中 ASP.NET Core 中开发的应用程序机密
author: rick-anderson
description: 演示如何在开发过程中安全地存储机密
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 166111696a9c4244ede44fca8878dd3725bb3099
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="59467-103">安全存储中 ASP.NET Core 中开发的应用程序机密</span><span class="sxs-lookup"><span data-stu-id="59467-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="59467-104">通过[Rick Anderson](https://twitter.com/RickAndMSFT)， [Daniel Roth](https://github.com/danroth27)，和[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="59467-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="59467-105">本文档说明如何在开发中使用密钥管理器工具需要机密放在代码外部。</span><span class="sxs-lookup"><span data-stu-id="59467-105">This document shows how you can use the Secret Manager tool in development to keep secrets out of your code.</span></span> <span data-ttu-id="59467-106">最重要的一点是你应永远不会将密码或其他敏感数据存储在源代码中，且你不应在开发和测试模式下使用生产机密。</span><span class="sxs-lookup"><span data-stu-id="59467-106">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span> <span data-ttu-id="59467-107">你可以改用[配置](xref:fundamentals/configuration/index)系统环境变量读取这些值或从值存储使用密钥管理器工具。</span><span class="sxs-lookup"><span data-stu-id="59467-107">You can instead use the [configuration](xref:fundamentals/configuration/index) system to read these values from environment variables or from values stored using the Secret Manager tool.</span></span> <span data-ttu-id="59467-108">密码管理器工具可帮助避免敏感数据被签入源代码管理。</span><span class="sxs-lookup"><span data-stu-id="59467-108">The Secret Manager tool helps prevent sensitive data from being checked into source control.</span></span> <span data-ttu-id="59467-109">[配置](xref:fundamentals/configuration/index)系统可以读取此文章中所述的密钥管理器工具与存储的机密。</span><span class="sxs-lookup"><span data-stu-id="59467-109">The [configuration](xref:fundamentals/configuration/index) system can read secrets stored with the Secret Manager tool described in this article.</span></span>

<span data-ttu-id="59467-110">密码管理器工具仅用于开发。</span><span class="sxs-lookup"><span data-stu-id="59467-110">The Secret Manager tool is used only in development.</span></span> <span data-ttu-id="59467-111">您可以保护与 Azure 的测试和生产机密[Microsoft Azure 密钥保管库](https://azure.microsoft.com/services/key-vault/)配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="59467-111">You can safeguard Azure test and production secrets with the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider.</span></span> <span data-ttu-id="59467-112">请参阅[Azure 密钥保管库配置提供程序](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="59467-112">See [Azure Key Vault configuration provider](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) for more information.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="59467-113">环境变量</span><span class="sxs-lookup"><span data-stu-id="59467-113">Environment variables</span></span>

<span data-ttu-id="59467-114">若要避免在代码中或在本地配置文件中存储应用程序机密，请在环境变量中存储机密。</span><span class="sxs-lookup"><span data-stu-id="59467-114">To avoid storing app secrets in code or in local configuration files, you store secrets in environment variables.</span></span> <span data-ttu-id="59467-115">你可以设置[配置](xref:fundamentals/configuration/index)框架，以读取从环境变量的值，通过调用`AddEnvironmentVariables`。</span><span class="sxs-lookup"><span data-stu-id="59467-115">You can setup the [configuration](xref:fundamentals/configuration/index) framework to read values from environment variables by calling `AddEnvironmentVariables`.</span></span> <span data-ttu-id="59467-116">然后可以使用环境变量来重写的所有以前指定的配置源的配置值。</span><span class="sxs-lookup"><span data-stu-id="59467-116">You can then use environment variables to override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="59467-117">例如，如果使用单个用户帐户创建新的 ASP.NET 核心 web 应用程序，它将添加到的默认连接字符串*appsettings.json*与键项目文件中的`DefaultConnection`。</span><span class="sxs-lookup"><span data-stu-id="59467-117">For example, if you create a new ASP.NET Core web app with individual user accounts, it will add a default connection string to the *appsettings.json* file in the project with the key `DefaultConnection`.</span></span> <span data-ttu-id="59467-118">默认连接字符串为安装程序以使用 LocalDB，在用户模式下运行，而且不需要密码。</span><span class="sxs-lookup"><span data-stu-id="59467-118">The default connection string is setup to use LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="59467-119">当你部署到测试或生产服务器应用程序时，你可以重写`DefaultConnection`密钥用于测试或生产数据库包含 （可能有敏感凭据） 的连接字符串的环境变量设置的值服务器。</span><span class="sxs-lookup"><span data-stu-id="59467-119">When you deploy your application to a test or production server, you can override the `DefaultConnection` key value with an environment variable setting that contains the connection string (potentially with sensitive credentials) for a test or production database server.</span></span>

>[!WARNING]
> <span data-ttu-id="59467-120">环境变量通常以明文形式存储，并且不加密。</span><span class="sxs-lookup"><span data-stu-id="59467-120">Environment variables are generally stored in plain text and are not encrypted.</span></span> <span data-ttu-id="59467-121">如果计算机或进程受到攻击，则可以通过不受信任方访问环境变量。</span><span class="sxs-lookup"><span data-stu-id="59467-121">If the machine or process is compromised, then environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="59467-122">可能仍需要更多措施，防止用户的机密信息泄露。</span><span class="sxs-lookup"><span data-stu-id="59467-122">Additional measures to prevent disclosure of user secrets may still be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="59467-123">密码管理器</span><span class="sxs-lookup"><span data-stu-id="59467-123">Secret Manager</span></span>

<span data-ttu-id="59467-124">密钥管理器工具存储在项目树之外的开发工作的敏感数据。</span><span class="sxs-lookup"><span data-stu-id="59467-124">The Secret Manager tool stores sensitive data for development work outside of your project tree.</span></span> <span data-ttu-id="59467-125">密钥管理器工具是一个项目的工具，可以用于在开发期间存储机密信息，以便.NET 核心项目。</span><span class="sxs-lookup"><span data-stu-id="59467-125">The Secret Manager tool is a project tool that can be used to store secrets for a .NET Core project during development.</span></span> <span data-ttu-id="59467-126">使用密钥管理器工具中，可以将应用程序机密关联与特定项目，并共享跨多个项目。</span><span class="sxs-lookup"><span data-stu-id="59467-126">With the Secret Manager tool, you can associate app secrets with a specific project and share them across multiple projects.</span></span>

>[!WARNING]
> <span data-ttu-id="59467-127">密码管理器工具不加密存储的机密信息，并不应被视为受信任存储区。</span><span class="sxs-lookup"><span data-stu-id="59467-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="59467-128">它是仅限开发目的。</span><span class="sxs-lookup"><span data-stu-id="59467-128">It's for development purposes only.</span></span> <span data-ttu-id="59467-129">键和值存储在用户配置文件目录中的 JSON 配置文件中。</span><span class="sxs-lookup"><span data-stu-id="59467-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="installing-the-secret-manager-tool"></a><span data-ttu-id="59467-130">安装机密管理器工具</span><span class="sxs-lookup"><span data-stu-id="59467-130">Installing the Secret Manager tool</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="59467-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59467-131">Visual Studio</span></span>](#tab/visual-studio/)
<span data-ttu-id="59467-132">右键单击解决方案资源管理器中的项目并选择**编辑\<文件的内容\>.csproj**从上下文菜单。</span><span class="sxs-lookup"><span data-stu-id="59467-132">Right-click the project in Solution Explorer, and select **Edit \<project_name\>.csproj** from the context menu.</span></span> <span data-ttu-id="59467-133">添加到突出显示的行将*.csproj*文件中，并保存以还原相关联的 NuGet 程序包：</span><span class="sxs-lookup"><span data-stu-id="59467-133">Add the highlighted line to the *.csproj* file, and save to restore the associated NuGet package:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="59467-134">再次右键单击解决方案资源管理器中的项目，然后选择**管理用户的机密信息**从上下文菜单。</span><span class="sxs-lookup"><span data-stu-id="59467-134">Right-click the project in Solution Explorer again, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="59467-135">添加一个新的该笔势`UserSecretsId`中的节点`PropertyGroup`的*.csproj*文件，那么在下面的示例中突出显示：</span><span class="sxs-lookup"><span data-stu-id="59467-135">This gesture adds a new `UserSecretsId` node within a `PropertyGroup` of the *.csproj* file, as highlighted in the following sample:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="59467-136">保存已修改*.csproj*文件还将打开`secrets.json`在文本编辑器中的文件。</span><span class="sxs-lookup"><span data-stu-id="59467-136">Saving the modified *.csproj* file also opens a `secrets.json` file in the text editor.</span></span> <span data-ttu-id="59467-137">内容替换`secrets.json`文件替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="59467-137">Replace the contents of the `secrets.json` file with the following code:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="59467-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="59467-138">Visual Studio Code</span></span>](#tab/visual-studio-code/)
<span data-ttu-id="59467-139">添加`Microsoft.Extensions.SecretManager.Tools`到*.csproj*文件，运行[dotnet 还原](/dotnet/core/tools/dotnet-restore)。</span><span class="sxs-lookup"><span data-stu-id="59467-139">Add `Microsoft.Extensions.SecretManager.Tools` to the *.csproj* file and run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span> <span data-ttu-id="59467-140">可以使用相同的步骤来安装命令行使用该密钥管理器工具。</span><span class="sxs-lookup"><span data-stu-id="59467-140">You can use the same steps to install the Secret Manager Tool using for the command line.</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="59467-141">通过运行以下命令来测试机密管理器工具：</span><span class="sxs-lookup"><span data-stu-id="59467-141">Test the Secret Manager tool by running the following command:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="59467-142">使用情况、 选项和命令的帮助，将显示在密码管理器工具。</span><span class="sxs-lookup"><span data-stu-id="59467-142">The Secret Manager tool will display usage, options and command help.</span></span>

> [!NOTE]
> <span data-ttu-id="59467-143">你必须在与位于同一目录*.csproj*文件以运行中定义的工具*.csproj*文件的`DotNetCliToolReference`节点。</span><span class="sxs-lookup"><span data-stu-id="59467-143">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` nodes.</span></span>

<span data-ttu-id="59467-144">密码管理器工具进行存储用户配置文件中的项目的特定配置设置操作。</span><span class="sxs-lookup"><span data-stu-id="59467-144">The Secret Manager tool operates on project-specific configuration settings that are stored in your user profile.</span></span> <span data-ttu-id="59467-145">若要使用用户的机密信息，项目必须指定`UserSecretsId`值在其*.csproj*文件。</span><span class="sxs-lookup"><span data-stu-id="59467-145">To use user secrets, the project must specify a `UserSecretsId` value in its *.csproj* file.</span></span> <span data-ttu-id="59467-146">值`UserSecretsId`是任意的但通常唯一的项目。</span><span class="sxs-lookup"><span data-stu-id="59467-146">The value of `UserSecretsId` is arbitrary, but is generally unique to the project.</span></span> <span data-ttu-id="59467-147">开发人员通常应生成的 GUID `UserSecretsId`。</span><span class="sxs-lookup"><span data-stu-id="59467-147">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

<span data-ttu-id="59467-148">添加`UserSecretsId`为你的项目中*.csproj*文件：</span><span class="sxs-lookup"><span data-stu-id="59467-148">Add a `UserSecretsId` for your project in the *.csproj* file:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="59467-149">密码管理器工具用于设置机密。</span><span class="sxs-lookup"><span data-stu-id="59467-149">Use the Secret Manager tool to set a secret.</span></span> <span data-ttu-id="59467-150">例如，在命令窗口从项目目录中，输入以下信息：</span><span class="sxs-lookup"><span data-stu-id="59467-150">For example, in a command window from the project directory, enter the following:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

<span data-ttu-id="59467-151">你可以从其他目录，运行密钥管理器工具，但必须使用`--project`选项的路径中传递*.csproj*文件：</span><span class="sxs-lookup"><span data-stu-id="59467-151">You can run the Secret Manager tool from other directories, but you must use the `--project` option to pass in the path to the *.csproj* file:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

<span data-ttu-id="59467-152">你可以使用密钥管理器工具能够列出、 删除和清除应用程序密钥。</span><span class="sxs-lookup"><span data-stu-id="59467-152">You can also use the Secret Manager tool to list, remove and clear app secrets.</span></span>

* * *
## <a name="accessing-user-secrets-via-configuration"></a><span data-ttu-id="59467-153">访问通过配置的用户机密</span><span class="sxs-lookup"><span data-stu-id="59467-153">Accessing user secrets via configuration</span></span>

<span data-ttu-id="59467-154">可以通过配置系统访问机密 Manager 机密。</span><span class="sxs-lookup"><span data-stu-id="59467-154">You access Secret Manager secrets through the configuration system.</span></span> <span data-ttu-id="59467-155">添加`Microsoft.Extensions.Configuration.UserSecrets`打包和运行[dotnet 还原](/dotnet/core/tools/dotnet-restore)。</span><span class="sxs-lookup"><span data-stu-id="59467-155">Add the `Microsoft.Extensions.Configuration.UserSecrets` package and run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>

<span data-ttu-id="59467-156">添加到用户机密配置源`Startup`方法：</span><span class="sxs-lookup"><span data-stu-id="59467-156">Add the user secrets configuration source to the `Startup` method:</span></span>

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

<span data-ttu-id="59467-157">您可以访问用户的机密信息的配置 API 通过：</span><span class="sxs-lookup"><span data-stu-id="59467-157">You can access user secrets via the configuration API:</span></span>

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="59467-158">密码管理器工具的工作原理</span><span class="sxs-lookup"><span data-stu-id="59467-158">How the Secret Manager tool works</span></span>

<span data-ttu-id="59467-159">密码管理器工具避开实现详细信息，例如哪里和如何存储值。</span><span class="sxs-lookup"><span data-stu-id="59467-159">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="59467-160">无需知道这些实现的详细信息，可以使用该工具。</span><span class="sxs-lookup"><span data-stu-id="59467-160">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="59467-161">在当前版本中，这些值存储在[JSON](http://json.org/)用户配置文件目录中的配置文件：</span><span class="sxs-lookup"><span data-stu-id="59467-161">In the current version, the values are stored in a [JSON](http://json.org/) configuration file in the user profile directory:</span></span>

* <span data-ttu-id="59467-162">Windows：`%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="59467-162">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span></span>

* <span data-ttu-id="59467-163">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="59467-163">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

* <span data-ttu-id="59467-164">macOS: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="59467-164">macOS: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

<span data-ttu-id="59467-165">值`userSecretsId`来自中指定的值*.csproj*文件。</span><span class="sxs-lookup"><span data-stu-id="59467-165">The value of `userSecretsId` comes from the value specified in *.csproj* file.</span></span>

<span data-ttu-id="59467-166">你不应编写代码所依赖的位置或使用密码管理器工具中，保存的数据格式，如这些实现的详细信息可能会更改。</span><span class="sxs-lookup"><span data-stu-id="59467-166">You shouldn't write code that depends on the location or format of the data saved with the Secret Manager tool, as these implementation details might change.</span></span> <span data-ttu-id="59467-167">例如，密钥的值是当前*不*今天，加密，但可能有一天。</span><span class="sxs-lookup"><span data-stu-id="59467-167">For example, the secret values are currently *not* encrypted today, but could be someday.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="59467-168">其他资源</span><span class="sxs-lookup"><span data-stu-id="59467-168">Additional resources</span></span>

* [<span data-ttu-id="59467-169">配置</span><span class="sxs-lookup"><span data-stu-id="59467-169">Configuration</span></span>](xref:fundamentals/configuration/index)
