---
title: 使用 Nginx 在 Linux 上托管 ASP.NET Core
author: rick-anderson
description: 了解如何在 Ubuntu 16.04 上将 Nginx 设置为反向代理，从而将 HTTP 流量转发到在 Kestrel 上运行的 ASP.NET Core Web 应用。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: edef672ca809c560a3f9faa891586e5e255284b5
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2018
ms.locfileid: "34566810"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="634b3-103">使用 Nginx 在 Linux 上托管 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="634b3-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="634b3-104">作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="634b3-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="634b3-105">本指南介绍如何在 Ubuntu 16.04 服务器上设置生产就绪 ASP.NET Core 环境。</span><span class="sxs-lookup"><span data-stu-id="634b3-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="634b3-106">这些说明可能适用于较新版本的 Ubuntu，但尚未使用较新版本进行测试。</span><span class="sxs-lookup"><span data-stu-id="634b3-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

> [!NOTE]
> <span data-ttu-id="634b3-107">对于 Ubuntu 14.04，建议进行监控，以此作为监视 Kestrel 进程的解决方案。</span><span class="sxs-lookup"><span data-stu-id="634b3-107">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="634b3-108">在 Ubuntu 14.04 上不提供 systemd。</span><span class="sxs-lookup"><span data-stu-id="634b3-108">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="634b3-109">有关 Ubuntu 14.04 的说明，请参阅[本主题的以前版本](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)。</span><span class="sxs-lookup"><span data-stu-id="634b3-109">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="634b3-110">本指南：</span><span class="sxs-lookup"><span data-stu-id="634b3-110">This guide:</span></span>

* <span data-ttu-id="634b3-111">将现有 ASP.NET Core 应用置于反向代理服务器后方。</span><span class="sxs-lookup"><span data-stu-id="634b3-111">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="634b3-112">设置反向代理服务器，以便将请求转发到 Kestrel Web 服务器。</span><span class="sxs-lookup"><span data-stu-id="634b3-112">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="634b3-113">确保 Web 应用在启动时作为守护程序运行。</span><span class="sxs-lookup"><span data-stu-id="634b3-113">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="634b3-114">配置进程管理工具以帮助重新启动 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="634b3-114">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="634b3-115">系统必备</span><span class="sxs-lookup"><span data-stu-id="634b3-115">Prerequisites</span></span>

1. <span data-ttu-id="634b3-116">使用具有 sudo 特权的标准用户帐户访问 Ubuntu 16.04 服务器。</span><span class="sxs-lookup"><span data-stu-id="634b3-116">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="634b3-117">在服务器上安装 .NET Core 运行时。</span><span class="sxs-lookup"><span data-stu-id="634b3-117">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="634b3-118">访问 [.NET Core“所有下载”页](https://www.microsoft.com/net/download/all)。</span><span class="sxs-lookup"><span data-stu-id="634b3-118">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="634b3-119">从“运行时”下的列表中选择最新的非预览运行时。</span><span class="sxs-lookup"><span data-stu-id="634b3-119">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="634b3-120">选择并执行适用于 Ubuntu 且匹配服务器的 Ubuntu 版本的说明。</span><span class="sxs-lookup"><span data-stu-id="634b3-120">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="634b3-121">一个现有 ASP.NET Core 应用。</span><span class="sxs-lookup"><span data-stu-id="634b3-121">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="634b3-122">通过应用发布和复制</span><span class="sxs-lookup"><span data-stu-id="634b3-122">Publish and copy over the app</span></span>

<span data-ttu-id="634b3-123">配置应用以进行[依赖框架的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)。</span><span class="sxs-lookup"><span data-stu-id="634b3-123">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="634b3-124">在开发环境中运行 [dotnet publish](/dotnet/core/tools/dotnet-publish)，将应用打包到可在服务器上运行的目录中（例如 bin/Release/&lt;target_framework_moniker&gt;/publish）：</span><span class="sxs-lookup"><span data-stu-id="634b3-124">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="634b3-125">如果不希望维护服务器上的 .NET Core 运行时，还可将应用发布为[独立部署](/dotnet/core/deploying/#self-contained-deployments-scd)。</span><span class="sxs-lookup"><span data-stu-id="634b3-125">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="634b3-126">使用集成到组织工作流的工具（例如 SCP、SFTP）将 ASP.NET Core 应用复制到服务器。</span><span class="sxs-lookup"><span data-stu-id="634b3-126">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="634b3-127">通常可在 var 目录（例如 var/aspnetcore/hellomvc）下找到 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="634b3-127">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="634b3-128">在生产部署方案中，持续集成工作流会执行发布应用并将资产复制到服务器的工作。</span><span class="sxs-lookup"><span data-stu-id="634b3-128">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="634b3-129">测试应用：</span><span class="sxs-lookup"><span data-stu-id="634b3-129">Test the app:</span></span>

1. <span data-ttu-id="634b3-130">在命令行中运行应用：`dotnet <app_assembly>.dll`。</span><span class="sxs-lookup"><span data-stu-id="634b3-130">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="634b3-131">在浏览器中，导航到 `http://<serveraddress>:<port>` 以确认应用在 Linux 本地正常运行。</span><span class="sxs-lookup"><span data-stu-id="634b3-131">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="634b3-132">配置反向代理服务器</span><span class="sxs-lookup"><span data-stu-id="634b3-132">Configure a reverse proxy server</span></span>

<span data-ttu-id="634b3-133">反向代理是为动态 Web 应用提供服务的常见设置。</span><span class="sxs-lookup"><span data-stu-id="634b3-133">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="634b3-134">反向代理终止 HTTP 请求，并将其转发到 ASP.NET Core 应用。</span><span class="sxs-lookup"><span data-stu-id="634b3-134">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="634b3-135">使用或不使用反向代理服务器进行配置对 ASP.NET Core 2.0 或更高版本的应用来说都是有效且受支持的托管配置。</span><span class="sxs-lookup"><span data-stu-id="634b3-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="634b3-136">有关详细信息，请参阅[何时结合使用 Kestrel 和反向代理](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="634b3-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="634b3-137">使用反向代理服务器</span><span class="sxs-lookup"><span data-stu-id="634b3-137">Use a reverse proxy server</span></span>

<span data-ttu-id="634b3-138">Kestrel 非常适合从 ASP.NET Core 提供动态内容。</span><span class="sxs-lookup"><span data-stu-id="634b3-138">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="634b3-139">但是，Web 服务功能不像服务器（如 IIS、Apache 或 Nginx）那样功能丰富。</span><span class="sxs-lookup"><span data-stu-id="634b3-139">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="634b3-140">反向代理服务器可以从 HTTP 服务器卸载服务静态内容、缓存请求、压缩请求和 SSL 终端等工作。</span><span class="sxs-lookup"><span data-stu-id="634b3-140">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="634b3-141">反向代理服务器可能驻留在专用计算机上，也可能与 HTTP 服务器一起部署。</span><span class="sxs-lookup"><span data-stu-id="634b3-141">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="634b3-142">鉴于此指南的目的，使用 Nginx 的单个实例。</span><span class="sxs-lookup"><span data-stu-id="634b3-142">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="634b3-143">它与 HTTP 服务器一起运行在同一服务器上。</span><span class="sxs-lookup"><span data-stu-id="634b3-143">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="634b3-144">根据要求，可以选择不同的设置。</span><span class="sxs-lookup"><span data-stu-id="634b3-144">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="634b3-145">由于请求是通过反向代理转接的，因此使用 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) 包中的[转接头中间件](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="634b3-145">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="634b3-146">此中间件使用 `X-Forwarded-Proto` 标头来更新 `Request.Scheme`，使重定向 URI 和其他安全策略能够正常工作。</span><span class="sxs-lookup"><span data-stu-id="634b3-146">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="634b3-147">调用转接头中间件后，必须放置依赖于该架构的组件，例如身份验证、链接生成、重定向和地理位置。</span><span class="sxs-lookup"><span data-stu-id="634b3-147">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="634b3-148">作为一般规则，转接头中间件应在诊断和错误处理中间件以外的其他中间件之前运行。</span><span class="sxs-lookup"><span data-stu-id="634b3-148">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="634b3-149">此顺序可确保依赖于转接头信息的中间件可以使用标头值进行处理。</span><span class="sxs-lookup"><span data-stu-id="634b3-149">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="634b3-150">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="634b3-150">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="634b3-151">在调用 [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) 或类似的身份验证方案中间件之前，调用 `Startup.Configure` 中的 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) 方法。</span><span class="sxs-lookup"><span data-stu-id="634b3-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="634b3-152">配置中间件以转接 `X-Forwarded-For` 和 `X-Forwarded-Proto` 标头：</span><span class="sxs-lookup"><span data-stu-id="634b3-152">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="634b3-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="634b3-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="634b3-154">在调用 [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) 和 [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) 或类似的身份验证方案中间件之前，调用 `Startup.Configure` 中的 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) 方法。</span><span class="sxs-lookup"><span data-stu-id="634b3-154">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="634b3-155">配置中间件以转接 `X-Forwarded-For` 和 `X-Forwarded-Proto` 标头：</span><span class="sxs-lookup"><span data-stu-id="634b3-155">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="634b3-156">如果没有为中间件指定 [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)，则要转接的默认标头为 `None`。</span><span class="sxs-lookup"><span data-stu-id="634b3-156">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="634b3-157">对于托管在代理服务器和负载均衡器后方的应用，可能需要附加配置。</span><span class="sxs-lookup"><span data-stu-id="634b3-157">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="634b3-158">有关详细信息，请参阅[配置 ASP.NET Core 以使用代理服务器和负载均衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="634b3-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="634b3-159">安装 Nginx</span><span class="sxs-lookup"><span data-stu-id="634b3-159">Install Nginx</span></span>

<span data-ttu-id="634b3-160">使用 `apt-get` 安装 Nginx。</span><span class="sxs-lookup"><span data-stu-id="634b3-160">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="634b3-161">安装程序将创建一个 systemd init 脚本，该脚本运行 Nginx，作为系统启动时的守护程序。</span><span class="sxs-lookup"><span data-stu-id="634b3-161">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> 

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

<span data-ttu-id="634b3-162">Ubuntu 个人包存档 (PPA) 由志愿者维护，并且不由 [nginx.org](https://nginx.org/) 分发。有关详细信息，请参阅 [Nginx：二进制版本：官方 Debian/Ubuntu 包](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)。</span><span class="sxs-lookup"><span data-stu-id="634b3-162">The Ubuntu Personal Package Archive (PPA) is maintained by volunteers and isn't distributed by [nginx.org](https://nginx.org/). For more information, see [Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="634b3-163">如果需要可选 Nginx 模块，则可能需要从源代码生成 Nginx。</span><span class="sxs-lookup"><span data-stu-id="634b3-163">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="634b3-164">因为是首次安装 Nginx，通过运行以下命令显式启动：</span><span class="sxs-lookup"><span data-stu-id="634b3-164">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="634b3-165">确认浏览器显示 Nginx 的默认登陆页。</span><span class="sxs-lookup"><span data-stu-id="634b3-165">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="634b3-166">可在 `http://<server_IP_address>/index.nginx-debian.html` 访问登陆页面。</span><span class="sxs-lookup"><span data-stu-id="634b3-166">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="634b3-167">配置 Nginx</span><span class="sxs-lookup"><span data-stu-id="634b3-167">Configure Nginx</span></span>

<span data-ttu-id="634b3-168">若要将 Nginx 配置为反向代理以将请求转接到 ASP.NET Core 应用，请修改 /etc/nginx/sites-available/default。</span><span class="sxs-lookup"><span data-stu-id="634b3-168">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="634b3-169">在文本编辑器中打开它，并将内容替换为以下内容：</span><span class="sxs-lookup"><span data-stu-id="634b3-169">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

<span data-ttu-id="634b3-170">当没有匹配的 `server_name` 时，Nginx 使用默认服务器。</span><span class="sxs-lookup"><span data-stu-id="634b3-170">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="634b3-171">如果没有定义默认服务器，则配置文件中的第一台服务器是默认服务器。</span><span class="sxs-lookup"><span data-stu-id="634b3-171">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="634b3-172">作为最佳做法，添加指定默认服务器，它会在配置文件中返回状态代码 444。</span><span class="sxs-lookup"><span data-stu-id="634b3-172">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="634b3-173">默认的服务器配置示例是：</span><span class="sxs-lookup"><span data-stu-id="634b3-173">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="634b3-174">使用上述配置文件和默认服务器，Nginx 接受主机标头 `example.com` 或 `*.example.com` 端口 80 上的公共流量。</span><span class="sxs-lookup"><span data-stu-id="634b3-174">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="634b3-175">与这些主机不匹配的请求不会转接到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="634b3-175">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="634b3-176">Nginx 将匹配的请求转接到 `http://localhost:5000` 中的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="634b3-176">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="634b3-177">有关详细信息，请参阅 [nginx 如何处理请求](https://nginx.org/docs/http/request_processing.html)。</span><span class="sxs-lookup"><span data-stu-id="634b3-177">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="634b3-178">未能指定正确的 [server_name 指令](https://nginx.org/docs/http/server_names.html)会公开应用的安全漏洞。</span><span class="sxs-lookup"><span data-stu-id="634b3-178">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="634b3-179">如果可控制整个父域（区别于易受攻击的 `*.com`），则子域通配符绑定（例如，`*.example.com`）不具有此安全风险。</span><span class="sxs-lookup"><span data-stu-id="634b3-179">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="634b3-180">有关详细信息，请参阅 [rfc7230 第 5.4 条](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="634b3-180">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="634b3-181">完成配置 Nginx 后，运行 `sudo nginx -t` 来验证配置文件的语法。</span><span class="sxs-lookup"><span data-stu-id="634b3-181">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="634b3-182">如果配置文件测试成功，可以通过运行 `sudo nginx -s reload` 强制 Nginx 选取更改。</span><span class="sxs-lookup"><span data-stu-id="634b3-182">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="634b3-183">要直接在服务器上运行应用：</span><span class="sxs-lookup"><span data-stu-id="634b3-183">To directly run the app on the server:</span></span>

1. <span data-ttu-id="634b3-184">请导航到应用目录。</span><span class="sxs-lookup"><span data-stu-id="634b3-184">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="634b3-185">运行应用的可执行文件：`./<app_executable>`。</span><span class="sxs-lookup"><span data-stu-id="634b3-185">Run the app's executable: `./<app_executable>`.</span></span>

<span data-ttu-id="634b3-186">如果出现权限错误，请更改权限：</span><span class="sxs-lookup"><span data-stu-id="634b3-186">If a permissions error occurs, change the permissions:</span></span>

```console
chmod u+x <app_executable>
```

<span data-ttu-id="634b3-187">如果应用在服务器上运行，但无法通过 Internet 响应，请检查服务器的防火墙，并确认端口 80 已打开。</span><span class="sxs-lookup"><span data-stu-id="634b3-187">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="634b3-188">如果使用 Azure Ubuntu VM，请添加启用入站端口 80 流量的网络安全组 (NSG) 规则。</span><span class="sxs-lookup"><span data-stu-id="634b3-188">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="634b3-189">不需要启用出站端口 80 规则，因为启用入站规则后会自动许可出站流量。</span><span class="sxs-lookup"><span data-stu-id="634b3-189">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="634b3-190">测试应用完成后，请在命令提示符处按 `Ctrl+C` 关闭应用。</span><span class="sxs-lookup"><span data-stu-id="634b3-190">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="634b3-191">监视应用</span><span class="sxs-lookup"><span data-stu-id="634b3-191">Monitoring the app</span></span>

<span data-ttu-id="634b3-192">服务器设置为将对 `http://<serveraddress>:80` 发起的请求转接到在 `http://127.0.0.1:5000` 中的 Kestrel 上运行的 ASP.NET Core 应用。</span><span class="sxs-lookup"><span data-stu-id="634b3-192">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="634b3-193">但是，未将 Nginx 设置为管理 Kestrel 进程。</span><span class="sxs-lookup"><span data-stu-id="634b3-193">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="634b3-194">systemd 可用于创建服务文件以启动和监视基础 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="634b3-194">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="634b3-195">systemd 是一个 init 系统，可以提供用于启动、停止和管理进程的许多强大的功能。</span><span class="sxs-lookup"><span data-stu-id="634b3-195">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="634b3-196">创建服务文件</span><span class="sxs-lookup"><span data-stu-id="634b3-196">Create the service file</span></span>

<span data-ttu-id="634b3-197">创建服务定义文件：</span><span class="sxs-lookup"><span data-stu-id="634b3-197">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="634b3-198">以下是应用的一个示例服务文件：</span><span class="sxs-lookup"><span data-stu-id="634b3-198">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="634b3-199">如果配置未使用用户 www-data，则必须先创建此处定义的用户，并为该用户提供适当的文件所有权。</span><span class="sxs-lookup"><span data-stu-id="634b3-199">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="634b3-200">Linux 具有区分大小写的文件系统。</span><span class="sxs-lookup"><span data-stu-id="634b3-200">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="634b3-201">将 ASPNETCORE_ENVIRONMENT 设置为“生产”会导致搜索配置文件 appsettings.Production.json，而不是 appsettings.production.json。</span><span class="sxs-lookup"><span data-stu-id="634b3-201">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="634b3-202">必须转义某些值（例如，SQL 连接字符串）以供配置提供程序读取环境变量。</span><span class="sxs-lookup"><span data-stu-id="634b3-202">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="634b3-203">使用以下命令生成适当的转义值以供在配置文件中使用：</span><span class="sxs-lookup"><span data-stu-id="634b3-203">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="634b3-204">保存该文件并启用该服务。</span><span class="sxs-lookup"><span data-stu-id="634b3-204">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="634b3-205">启用该服务，并确认它正在运行。</span><span class="sxs-lookup"><span data-stu-id="634b3-205">Start the service and verify that it's running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="634b3-206">在配置了反向代理并通过 systemd 管理 Kestrel 后，Web 应用现已完全配置，并能在本地计算机上的浏览器中从 `http://localhost` 进行访问。</span><span class="sxs-lookup"><span data-stu-id="634b3-206">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="634b3-207">也可以从远程计算机进行访问，同时限制可能进行阻止的任何防火墙。</span><span class="sxs-lookup"><span data-stu-id="634b3-207">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="634b3-208">检查响应标头，`Server` 标头显示由 Kestrel 所提供的 ASP.NET Core 应用。</span><span class="sxs-lookup"><span data-stu-id="634b3-208">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="634b3-209">查看日志</span><span class="sxs-lookup"><span data-stu-id="634b3-209">Viewing logs</span></span>

<span data-ttu-id="634b3-210">使用 Kestrel 的 Web 应用是通过 `systemd` 进行管理的，因此所有事件和进程都被记录到集中日志。</span><span class="sxs-lookup"><span data-stu-id="634b3-210">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="634b3-211">但是，此日志包含由 `systemd` 管理的所有服务和进程的全部条目。</span><span class="sxs-lookup"><span data-stu-id="634b3-211">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="634b3-212">若要查看特定于 `kestrel-hellomvc.service` 的项，请使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="634b3-212">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="634b3-213">有关进一步筛选，使用时间选项（如 `--since today`、`--until 1 hour ago`）或这些选项的组合可以减少返回的条目数。</span><span class="sxs-lookup"><span data-stu-id="634b3-213">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="634b3-214">保护应用</span><span class="sxs-lookup"><span data-stu-id="634b3-214">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="634b3-215">启用 AppArmor</span><span class="sxs-lookup"><span data-stu-id="634b3-215">Enable AppArmor</span></span>

<span data-ttu-id="634b3-216">Linux 安全模块 (LSM) 是一个框架，它是自 Linux 2.6 后的 Linux kernel 的一部分。</span><span class="sxs-lookup"><span data-stu-id="634b3-216">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="634b3-217">LSM 支持安全模块的不同实现。</span><span class="sxs-lookup"><span data-stu-id="634b3-217">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="634b3-218">[AppArmor](https://wiki.ubuntu.com/AppArmor) 是实现强制访问控制系统的 LSM，它允许将程序限制在一组有限的资源内。</span><span class="sxs-lookup"><span data-stu-id="634b3-218">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="634b3-219">确保已启用并成功配置 AppArmor。</span><span class="sxs-lookup"><span data-stu-id="634b3-219">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="634b3-220">配置防火墙</span><span class="sxs-lookup"><span data-stu-id="634b3-220">Configuring the firewall</span></span>

<span data-ttu-id="634b3-221">关闭所有未使用的外部端口。</span><span class="sxs-lookup"><span data-stu-id="634b3-221">Close off all external ports that are not in use.</span></span> <span data-ttu-id="634b3-222">通过为配置防火墙提供命令行接口，不复杂的防火墙 (ufw) 为 `iptables` 提供了前端。</span><span class="sxs-lookup"><span data-stu-id="634b3-222">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="634b3-223">确认已配置 `ufw` 以允许所需的任何端口上的流量。</span><span class="sxs-lookup"><span data-stu-id="634b3-223">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="634b3-224">保护 Nginx</span><span class="sxs-lookup"><span data-stu-id="634b3-224">Securing Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="634b3-225">更改 Nginx 响应名称</span><span class="sxs-lookup"><span data-stu-id="634b3-225">Change the Nginx response name</span></span>

<span data-ttu-id="634b3-226">编辑 src/http/ngx_http_header_filter_module.c：</span><span class="sxs-lookup"><span data-stu-id="634b3-226">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="634b3-227">配置选项</span><span class="sxs-lookup"><span data-stu-id="634b3-227">Configure options</span></span>

<span data-ttu-id="634b3-228">用其他必需模块配置服务器。</span><span class="sxs-lookup"><span data-stu-id="634b3-228">Configure the server with additional required modules.</span></span> <span data-ttu-id="634b3-229">请考虑使用 [ModSecurity](https://www.modsecurity.org/) 等 Web 应用防火墙来加强对应用的保护。</span><span class="sxs-lookup"><span data-stu-id="634b3-229">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="configure-ssl"></a><span data-ttu-id="634b3-230">配置 SSL</span><span class="sxs-lookup"><span data-stu-id="634b3-230">Configure SSL</span></span>

* <span data-ttu-id="634b3-231">通过指定由受信任的证书颁发机构 (CA) 颁发的有效证书来配置服务器，以侦听端口 `443` 上的 HTTPS 流量。</span><span class="sxs-lookup"><span data-stu-id="634b3-231">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="634b3-232">通过采用以下“/etc/nginx/nginx.conf”文件中所示的某些做法来增强安全保护。</span><span class="sxs-lookup"><span data-stu-id="634b3-232">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="634b3-233">示例包括选择更强的密码并将通过 HTTP 的所有流量重定向到 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="634b3-233">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="634b3-234">添加 `HTTP Strict-Transport-Security` (HSTS) 标头可确保由客户端发起的所有后续请求都仅通过 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="634b3-234">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="634b3-235">如果以后会禁用 SSL，则不要添加 Strict-Transport-Security 标头或选择相应的 `max-age`。</span><span class="sxs-lookup"><span data-stu-id="634b3-235">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="634b3-236">添加 /etc/nginx/proxy.conf 配置文件：</span><span class="sxs-lookup"><span data-stu-id="634b3-236">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="634b3-237">编辑 /etc/nginx/nginx.conf 配置文件。</span><span class="sxs-lookup"><span data-stu-id="634b3-237">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="634b3-238">示例包含一个配置文件中的 `http` 和 `server` 部分。</span><span class="sxs-lookup"><span data-stu-id="634b3-238">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="634b3-239">保护 Nginx 免受点击劫持的侵害</span><span class="sxs-lookup"><span data-stu-id="634b3-239">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="634b3-240">点击劫持是收集受感染用户的点击数的恶意技术。</span><span class="sxs-lookup"><span data-stu-id="634b3-240">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="634b3-241">点击劫持诱使受害者（访问者）点击受感染的网站。</span><span class="sxs-lookup"><span data-stu-id="634b3-241">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="634b3-242">使用 X-FRAME-OPTIONS 保护站点。</span><span class="sxs-lookup"><span data-stu-id="634b3-242">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="634b3-243">编辑 nginx.conf 文件：</span><span class="sxs-lookup"><span data-stu-id="634b3-243">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="634b3-244">添加行 `add_header X-Frame-Options "SAMEORIGIN";` 并保存文件，然后重新启动 Nginx。</span><span class="sxs-lookup"><span data-stu-id="634b3-244">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="634b3-245">MIME 类型探查</span><span class="sxs-lookup"><span data-stu-id="634b3-245">MIME-type sniffing</span></span>

<span data-ttu-id="634b3-246">此标头可阻止大部分浏览器通过 MIME 方式探查来自已声明内容类型的响应，因为标头会指示浏览器不要替代响应内容类型。</span><span class="sxs-lookup"><span data-stu-id="634b3-246">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="634b3-247">使用 `nosniff` 选项后，如果服务器认为内容是“文本/html”，则浏览器将其显示为“文本/html”。</span><span class="sxs-lookup"><span data-stu-id="634b3-247">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="634b3-248">编辑 nginx.conf 文件：</span><span class="sxs-lookup"><span data-stu-id="634b3-248">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="634b3-249">添加行 `add_header X-Content-Type-Options "nosniff";` 并保存文件，然后重新启动 Nginx。</span><span class="sxs-lookup"><span data-stu-id="634b3-249">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="634b3-250">其他资源</span><span class="sxs-lookup"><span data-stu-id="634b3-250">Additional resources</span></span>

* [<span data-ttu-id="634b3-251">Nginx：二进制版本：官方 Debian/Ubuntu 包</span><span class="sxs-lookup"><span data-stu-id="634b3-251">Nginx: Binary Releases: Official Debian/Ubuntu packages</span></span>](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* [<span data-ttu-id="634b3-252">配置 ASP.NET Core 以使用代理服务器和负载均衡器</span><span class="sxs-lookup"><span data-stu-id="634b3-252">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="634b3-253">NGINX：使用转接头</span><span class="sxs-lookup"><span data-stu-id="634b3-253">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
