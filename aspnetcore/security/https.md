---
title: "设置 ASP.NET Core 中开发的 HTTPS"
author: Rick-Anderson
description: "演示如何为 ASP.NET 核心 2.0 中的开发设置 HTTPS。"
keywords: "ASP.NET 核心，SSL，HTTPS"
ms.author: riande
manager: wpickett
ms.date: 05/10/2017
ms.topic: article
ms.assetid: 94f2f1a4-7d46-45e2-a085-a57916e41724
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/https
ms.openlocfilehash: 7913758ffa045dcf884d73eed9bab223b8d2c3fe
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2017
---
# <a name="setting-up-https-for-development-in-aspnet-core"></a><span data-ttu-id="de601-104">设置 ASP.NET Core 中开发的 HTTPS</span><span class="sxs-lookup"><span data-stu-id="de601-104">Setting up HTTPS for development in ASP.NET Core</span></span>

> [!NOTE] 
> <span data-ttu-id="de601-105">本主题适用于 ASP.NET 核心 2.0 Preview 1</span><span class="sxs-lookup"><span data-stu-id="de601-105">This topic applies to ASP.NET Core 2.0 Preview 1</span></span>

<span data-ttu-id="de601-106">你可以配置你的应用程序在开发过程中使用 HTTPS 来模拟生产环境中的 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="de601-106">You can configure your application to use HTTPS during development to simulate HTTPS in your production environment.</span></span> <span data-ttu-id="de601-107">启用 HTTPS 可能需要启用与各种标识提供程序的集成 (如[Azure AD](https://azure.microsoft.com/services/active-directory)和[Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/))。</span><span class="sxs-lookup"><span data-stu-id="de601-107">Enabling HTTPS may be required to enable integration with various identity providers (like [Azure AD](https://azure.microsoft.com/services/active-directory) and [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/)).</span></span>

<a name="iisxpress"></a>

<span data-ttu-id="de601-108">在 Windows 上如果你已安装 Visual Studio 或 IIS Express，IIS Express 开发证书将在 LocalMachine 证书存储区。</span><span class="sxs-lookup"><span data-stu-id="de601-108">On Windows if you’ve installed Visual Studio or IIS Express, the IIS Express Development Certificate will be in your LocalMachine certificate store.</span></span> <span data-ttu-id="de601-109">你可以更新项目属性中使用此证书时 IIS Express 后面运行 Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="de601-109">You can update your project properties in Visual Studio to use this certificate when running behind IIS Express.</span></span>

   * <span data-ttu-id="de601-110">在解决方案资源管理器，右键单击该项目并选择**属性**。</span><span class="sxs-lookup"><span data-stu-id="de601-110">In Solution Explorer, right-click the project and select **Properties**.</span></span>
   * <span data-ttu-id="de601-111">在左窗格中，选择**调试**。</span><span class="sxs-lookup"><span data-stu-id="de601-111">On the left pane, select **Debug**.</span></span>
   * <span data-ttu-id="de601-112">检查**启用 SSL**</span><span class="sxs-lookup"><span data-stu-id="de601-112">Check **Enable SSL**</span></span>
   * <span data-ttu-id="de601-113">复制 SSL URL 并将其粘贴到**应用 URL**</span><span class="sxs-lookup"><span data-stu-id="de601-113">Copy the SSL URL and paste it into the **App URL**</span></span>

![调试 web 应用程序属性选项卡](enforcing-ssl/_static/ssl.png)

<span data-ttu-id="de601-115">用于开发可以使用 IIS Express 开发证书，如果可用，或创建新证书出于开发目的。</span><span class="sxs-lookup"><span data-stu-id="de601-115">For development you can use the IIS Express Development Certificate if it is available, or create a new certificate for development purposes.</span></span> <span data-ttu-id="de601-116">应在配置的开发证书`appsettings.Development.json`文件，以便在生产环境中不使用它：</span><span class="sxs-lookup"><span data-stu-id="de601-116">The development certificate should be configured in the `appsettings.Development.json` file so that it is not used in production:</span></span>

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "Store",
      "StoreLocation": "LocalMachine",
      "StoreName": "My",
      "Subject": "CN=localhost",
      "AllowInvalid": true
    }
  }
}
```

<span data-ttu-id="de601-117">具有此配置，在生产环境中运行的应用将引发异常指示"没有名为 HTTPS 在当前环境 （生产） 的配置中找到的证书"。</span><span class="sxs-lookup"><span data-stu-id="de601-117">An app with this configuration running in production will throw an exception saying "No certificate named 'HTTPS' found in configuration for the current environment (Production)".</span></span> <span data-ttu-id="de601-118">若要切换[环境](xref:fundamentals/environments)到`Development`，将其设置`ASPNETCORE_ENVIRONMENT`环境变量`Development`。</span><span class="sxs-lookup"><span data-stu-id="de601-118">To switch the [environment](xref:fundamentals/environments) to `Development`, set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development`.</span></span>

<span data-ttu-id="de601-119">如果你没有 IIS Express 开发安装的证书，你可以自行创建开发证书。</span><span class="sxs-lookup"><span data-stu-id="de601-119">If you do not have the IIS Express Development Certificate installed, you can create a development certificate yourself.</span></span> <span data-ttu-id="de601-120">在 Windows 上，你可以创建开发证书并将其添加到当前用户的受信任的根存储中，通过在提升的提示符中运行以下 PowerShell 命令：</span><span class="sxs-lookup"><span data-stu-id="de601-120">On Windows you can create a development certificate and add it to the trusted root store for the current user by running the following PowerShell commands in an elevated prompt:</span></span>

```powershell
$cert = New-SelfSignedCertificate -Subject localhost -DnsName localhost -FriendlyName "ASP.NET Core Development" -KeyUsage DigitalSignature -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") 
Export-Certificate -Cert $cert -FilePath cert.cer
Import-Certificate -FilePath cert.cer -CertStoreLocation cert:/CurrentUser/Root
```

<a name="OpenSSL"></a>

## <a name="kestrel-on--macos-and-linux"></a><span data-ttu-id="de601-121">在 macOS 和 Linux 上的 kestrel</span><span class="sxs-lookup"><span data-stu-id="de601-121">Kestrel on  macOS and Linux</span></span>

<span data-ttu-id="de601-122">你可以配置 Kestrel 以通过配置终结点所需的 IP 地址、 端口和证书通过 HTTPS 侦听。</span><span class="sxs-lookup"><span data-stu-id="de601-122">You can  configure Kestrel to listen over HTTPS by configuring an endpoint with the desired IP address, port, and certificate.</span></span> <span data-ttu-id="de601-123">可以配置的内联，该证书或位于顶级`Certificates`部分，然后按名称引用：</span><span class="sxs-lookup"><span data-stu-id="de601-123">The certificate can be configured inline, or in the top-level `Certificates` section and then referenced by name:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "LocalhostHttps": {
        "Address": "127.0.0.1",
        "Port": "43434",
        "Certificate": "HTTPS"
      }
    }
  }
}

```

<span data-ttu-id="de601-124">在 macOS 和 Linux 上，你可以创建使用 HTTPS 的自签名的证书[OpenSSL](https://www.openssl.org/):</span><span class="sxs-lookup"><span data-stu-id="de601-124">On macOS and Linux you can create a self-signed certificate for HTTPS using [OpenSSL](https://www.openssl.org/):</span></span>

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout localhost.key -out localhost.cer -days 365 -subj /CN=localhost
openssl pkcs12 -export -out certificate.pfx -inkey localhost.key -in localhost.cer
```

<span data-ttu-id="de601-125">一次`certificate.pfx`生成文件，配置中的 HTTPS 证书你`appsettings.Development.json`文件：</span><span class="sxs-lookup"><span data-stu-id="de601-125">Once the `certificate.pfx` file has been generated, configure the HTTPS certificate in your `appsettings.Development.json` file:</span></span>

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "File",
      "Path": "certificate.pfx"
    }
  }
}
```

<span data-ttu-id="de601-126">你还需要通过设置"的证书： HTTPS:Password"配置属性中指定证书的密码。</span><span class="sxs-lookup"><span data-stu-id="de601-126">You will also need to specify the passphrase for the certificate by setting the “Certificates:HTTPS:Password” config property.</span></span> <span data-ttu-id="de601-127">不应以明文形式存储密码。</span><span class="sxs-lookup"><span data-stu-id="de601-127">Passwords should not be stored in plain text.</span></span> <span data-ttu-id="de601-128">请参阅[安全存储的应用程序机密期间开发](app-secrets.md)为合适的处理操作的证书密码。</span><span class="sxs-lookup"><span data-stu-id="de601-128">See [Safe Storage of App Secrets During Development](app-secrets.md) for appropriate handling of the certificate passphrase.</span></span>

<span data-ttu-id="de601-129">在 macOS 上可以[将证书添加到你 keychain](https://support.apple.com/kb/PH20129?locale=en_US)和[更改其信任设置](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US)，以便在开发过程是为支持 HTTPS 的受信任。</span><span class="sxs-lookup"><span data-stu-id="de601-129">On macOS you can [add the certificate to your keychain](https://support.apple.com/kb/PH20129?locale=en_US) and [change its trust settings](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) so that it is trusted for HTTPS during development.</span></span> <span data-ttu-id="de601-130">若要将证书添加到你的密钥链 (等效于`CurrentUser/My`存储在 Windows 上) 运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="de601-130">To add the certificate to your keychain (the equivalent of the `CurrentUser/My` store on Windows) run the following command:</span></span>

```bash
security import certificate.pfx -k ~/Library/Keychains/login.keychain-db
```

<span data-ttu-id="de601-131">然后可以信任的证书：</span><span class="sxs-lookup"><span data-stu-id="de601-131">And then to trust the certificate:</span></span>

```bash
security add-trusted-cert localhost.cer
```

<span data-ttu-id="de601-132">然后，你可以配置你的应用以如下的开发中使用此证书：</span><span class="sxs-lookup"><span data-stu-id="de601-132">You can then configure your app to use this certificate in development like this:</span></span>

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "Store",
      "StoreLocation": "CurrentUser",
      "StoreName": "My",
      "Subject": "CN=localhost",
      "AllowInvalid": true
    }
  }
}
```
