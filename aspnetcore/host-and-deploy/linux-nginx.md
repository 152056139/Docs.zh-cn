---
title: 使用 Nginx 在 Linux 上托管 ASP.NET Core
author: rick-anderson
description: 描述如何在 Ubuntu 16.04 转发到 ASP.NET 核心 web 应用程序在 Kestrel 上运行的 HTTP 流量的反向代理设置 Nginx。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 64093b9fcfa9047145de8f8b142f72fa1515f248
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="2078e-103">使用 Nginx 在 Linux 上托管 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2078e-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="2078e-104">作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="2078e-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="2078e-105">本指南介绍如何在 Ubuntu 16.04 服务器上设置生产就绪 ASP.NET Core 环境。</span><span class="sxs-lookup"><span data-stu-id="2078e-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

> [!NOTE]
> <span data-ttu-id="2078e-106">对于 Ubuntu 14.04 *supervisord*建议为用于监视 Kestrel 进程的解决方案。</span><span class="sxs-lookup"><span data-stu-id="2078e-106">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="2078e-107">*systemd*在 Ubuntu 14.04 上不可用。</span><span class="sxs-lookup"><span data-stu-id="2078e-107">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="2078e-108">[请参阅本文档的以前版本](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)。</span><span class="sxs-lookup"><span data-stu-id="2078e-108">[See previous version of this document](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="2078e-109">本指南：</span><span class="sxs-lookup"><span data-stu-id="2078e-109">This guide:</span></span>

* <span data-ttu-id="2078e-110">将现有 ASP.NET Core 应用程序反向代理服务器后面。</span><span class="sxs-lookup"><span data-stu-id="2078e-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="2078e-111">将设置反向代理服务器将请求转发到 Kestrel web 服务器。</span><span class="sxs-lookup"><span data-stu-id="2078e-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="2078e-112">可确保在作为后台进程启动时运行的 web 应用。</span><span class="sxs-lookup"><span data-stu-id="2078e-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="2078e-113">配置有助于重新启动 web 应用的进程管理工具。</span><span class="sxs-lookup"><span data-stu-id="2078e-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2078e-114">系统必备</span><span class="sxs-lookup"><span data-stu-id="2078e-114">Prerequisites</span></span>

1. <span data-ttu-id="2078e-115">使用具有 sudo 特权的标准用户帐户访问 Ubuntu 16.04 服务器</span><span class="sxs-lookup"><span data-stu-id="2078e-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="2078e-116">现有的 ASP.NET Core 应用程序</span><span class="sxs-lookup"><span data-stu-id="2078e-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="2078e-117">通过应用程序复制</span><span class="sxs-lookup"><span data-stu-id="2078e-117">Copy over the app</span></span>

<span data-ttu-id="2078e-118">运行[dotnet 发布](/dotnet/core/tools/dotnet-publish)从要打包到一个自包含的目录，可以在服务器上运行的应用程序的开发环境。</span><span class="sxs-lookup"><span data-stu-id="2078e-118">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="2078e-119">将 ASP.NET Core 应用程序复制到使用任何工具集成到组织的工作流 （例如，SCP，FTP） 服务器。</span><span class="sxs-lookup"><span data-stu-id="2078e-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="2078e-120">测试应用，例如：</span><span class="sxs-lookup"><span data-stu-id="2078e-120">Test the app, for example:</span></span>

* <span data-ttu-id="2078e-121">从命令行中，运行`dotnet <app_assembly>.dll`。</span><span class="sxs-lookup"><span data-stu-id="2078e-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="2078e-122">在浏览器中，导航到 `http://<serveraddress>:<port>` 以确认应用在 Linux 上正常运行。</span><span class="sxs-lookup"><span data-stu-id="2078e-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="2078e-123">配置反向代理服务器</span><span class="sxs-lookup"><span data-stu-id="2078e-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="2078e-124">反向代理是为动态 web 应用程序提供服务的常见设置。</span><span class="sxs-lookup"><span data-stu-id="2078e-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="2078e-125">反向代理终止 HTTP 请求，并将其转发到 ASP.NET 核心应用程序。</span><span class="sxs-lookup"><span data-stu-id="2078e-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="2078e-126">为何使用反向代理服务器？</span><span class="sxs-lookup"><span data-stu-id="2078e-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="2078e-127">Kestrel 非常适合从 ASP.NET Core 提供动态内容。</span><span class="sxs-lookup"><span data-stu-id="2078e-127">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="2078e-128">但是，web 服务功能不是为 IIS、 Apache 或 Nginx 例如与服务器的功能丰富。</span><span class="sxs-lookup"><span data-stu-id="2078e-128">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="2078e-129">反向代理服务器可以卸载例如提供静态内容、 缓存请求、 压缩请求和从 HTTP 服务器的 SSL 终止的工作。</span><span class="sxs-lookup"><span data-stu-id="2078e-129">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="2078e-130">反向代理服务器可能驻留在专用计算机上，也可能与 HTTP 服务器一起部署。</span><span class="sxs-lookup"><span data-stu-id="2078e-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="2078e-131">鉴于此指南的目的，使用 Nginx 的单个实例。</span><span class="sxs-lookup"><span data-stu-id="2078e-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="2078e-132">它与 HTTP 服务器一起运行在同一服务器上。</span><span class="sxs-lookup"><span data-stu-id="2078e-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="2078e-133">根据要求，可以选择不同的安装。</span><span class="sxs-lookup"><span data-stu-id="2078e-133">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="2078e-134">因为通过反向代理转发请求，使用从转发标头 Middleware [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/)包。</span><span class="sxs-lookup"><span data-stu-id="2078e-134">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="2078e-135">中间件更新`Request.Scheme`，使用`X-Forwarded-Proto`标头，因此该重定向 Uri 和其他安全策略正常工作。</span><span class="sxs-lookup"><span data-stu-id="2078e-135">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="2078e-136">当使用任何类型的身份验证中间件，转发标头中间件必须运行第一个。</span><span class="sxs-lookup"><span data-stu-id="2078e-136">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="2078e-137">此顺序可确保身份验证中间件可使用的标头值并生成正确的重定向 Uri。</span><span class="sxs-lookup"><span data-stu-id="2078e-137">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2078e-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2078e-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2078e-139">调用[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)中的方法`Startup.Configure`之前调用[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication)或类似的身份验证方案中间件。</span><span class="sxs-lookup"><span data-stu-id="2078e-139">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="2078e-140">配置为转发的中间件`X-Forwarded-For`和`X-Forwarded-Proto`标头：</span><span class="sxs-lookup"><span data-stu-id="2078e-140">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2078e-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2078e-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2078e-142">调用[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)中的方法`Startup.Configure`之前调用[UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity)和[UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication)或类似的身份验证方案中间件。</span><span class="sxs-lookup"><span data-stu-id="2078e-142">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="2078e-143">配置为转发的中间件`X-Forwarded-For`和`X-Forwarded-Proto`标头：</span><span class="sxs-lookup"><span data-stu-id="2078e-143">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="2078e-144">如果没有[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)指定到中间件，要转发的默认标头是`None`。</span><span class="sxs-lookup"><span data-stu-id="2078e-144">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="2078e-145">代理服务器和负载平衡器后面托管的应用程序可能需要其他配置。</span><span class="sxs-lookup"><span data-stu-id="2078e-145">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="2078e-146">有关详细信息，请参阅[配置 ASP.NET 核心以使用代理服务器和负载平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="2078e-146">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="2078e-147">安装 Nginx</span><span class="sxs-lookup"><span data-stu-id="2078e-147">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="2078e-148">如果将安装可选 Nginx 模块，生成从源 Nginx 可能需要。</span><span class="sxs-lookup"><span data-stu-id="2078e-148">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="2078e-149">使用 `apt-get` 安装 Nginx。</span><span class="sxs-lookup"><span data-stu-id="2078e-149">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="2078e-150">安装程序创建一个 System V init 脚本，该脚本运行 Nginx 作为系统启动时的守护程序。</span><span class="sxs-lookup"><span data-stu-id="2078e-150">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="2078e-151">因为是首次安装 Nginx，通过运行以下命令显式启动：</span><span class="sxs-lookup"><span data-stu-id="2078e-151">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="2078e-152">确认浏览器显示 Nginx 的默认登陆页。</span><span class="sxs-lookup"><span data-stu-id="2078e-152">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="2078e-153">配置 Nginx</span><span class="sxs-lookup"><span data-stu-id="2078e-153">Configure Nginx</span></span>

<span data-ttu-id="2078e-154">若要将 Nginx 配置为转发请求向 ASP.NET Core 应用程序的反向代理，修改*/etc/nginx/sites-available/default*。</span><span class="sxs-lookup"><span data-stu-id="2078e-154">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="2078e-155">在文本编辑器中打开它，并将内容替换为以下内容：</span><span class="sxs-lookup"><span data-stu-id="2078e-155">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="2078e-156">如果没有`server_name`Nginx 的匹配项，使用默认服务器。</span><span class="sxs-lookup"><span data-stu-id="2078e-156">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="2078e-157">如果定义了默认的服务器，配置文件中的第一个服务器将是默认服务器。</span><span class="sxs-lookup"><span data-stu-id="2078e-157">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="2078e-158">最佳做法，将添加在配置文件中返回 444 状态代码的特定的默认服务器。</span><span class="sxs-lookup"><span data-stu-id="2078e-158">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="2078e-159">默认服务器配置示例是：</span><span class="sxs-lookup"><span data-stu-id="2078e-159">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="2078e-160">与上述配置文件和默认服务器，Nginx 接受主机标头使用的端口 80 上的公共流量`example.com`或`*.example.com`。</span><span class="sxs-lookup"><span data-stu-id="2078e-160">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="2078e-161">不会获取与这些主机不匹配的请求转发到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="2078e-161">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="2078e-162">Nginx 将匹配的请求转发到在 Kestrel `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="2078e-162">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="2078e-163">请参阅[nginx 如何处理请求](https://nginx.org/docs/http/request_processing.html)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="2078e-163">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="2078e-164">如果未能指定合适[server_name 指令](https://nginx.org/docs/http/server_names.html)公开您的应用程序安全漏洞。</span><span class="sxs-lookup"><span data-stu-id="2078e-164">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="2078e-165">子域通配符绑定 (例如， `*.example.com`) 不会带来安全风险，若要控制整个父域 (相对于`*.com`，这是易受攻击)。</span><span class="sxs-lookup"><span data-stu-id="2078e-165">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="2078e-166">有关详细信息，请参阅 [rfc7230 第 5.4 条](https://tools.ietf.org/html/rfc7230#section-5.4)。</span><span class="sxs-lookup"><span data-stu-id="2078e-166">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="2078e-167">一旦建立 Nginx 配置，运行`sudo nginx -t`若要验证的配置文件的语法。</span><span class="sxs-lookup"><span data-stu-id="2078e-167">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="2078e-168">如果配置文件测试成功，强制 Nginx 以便通过运行选取更改`sudo nginx -s reload`。</span><span class="sxs-lookup"><span data-stu-id="2078e-168">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="2078e-169">监视应用程序</span><span class="sxs-lookup"><span data-stu-id="2078e-169">Monitoring the app</span></span>

<span data-ttu-id="2078e-170">服务器已设置为转发到发出的请求`http://<serveraddress>:80`Kestrel 在上运行 ASP.NET Core 应用到`http://127.0.0.1:5000`。</span><span class="sxs-lookup"><span data-stu-id="2078e-170">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="2078e-171">但是，Nginx 未设置来管理 Kestrel 过程。</span><span class="sxs-lookup"><span data-stu-id="2078e-171">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="2078e-172">*systemd*可以用于创建服务文件以启动和监视基础的 web 应用。</span><span class="sxs-lookup"><span data-stu-id="2078e-172">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="2078e-173">systemd 是一个 init 系统，可以提供用于启动、停止和管理进程的许多强大的功能。</span><span class="sxs-lookup"><span data-stu-id="2078e-173">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="2078e-174">创建服务文件</span><span class="sxs-lookup"><span data-stu-id="2078e-174">Create the service file</span></span>

<span data-ttu-id="2078e-175">创建服务定义文件：</span><span class="sxs-lookup"><span data-stu-id="2078e-175">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="2078e-176">下面是应用程序的示例服务文件：</span><span class="sxs-lookup"><span data-stu-id="2078e-176">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="2078e-177">**注意：**如果用户*www 数据*未使用的配置，此处定义的用户必须创建第一次并且为文件提供的适当的所有权。</span><span class="sxs-lookup"><span data-stu-id="2078e-177">**Note:** If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>
<span data-ttu-id="2078e-178">**注意：** Linux 具有区分大小写的文件系统。</span><span class="sxs-lookup"><span data-stu-id="2078e-178">**Note:** Linux has a case-sensitive file system.</span></span> <span data-ttu-id="2078e-179">搜索配置文件中的"生产"结果设置 ASPNETCORE_ENVIRONMENT *appsettings。Production.json*，而不*appsettings.production.json*。</span><span class="sxs-lookup"><span data-stu-id="2078e-179">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="2078e-180">保存该文件并启用该服务。</span><span class="sxs-lookup"><span data-stu-id="2078e-180">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="2078e-181">启动服务并验证它正在运行。</span><span class="sxs-lookup"><span data-stu-id="2078e-181">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="2078e-182">配置反向代理和管理通过 systemd Kestrel，web 应用完全配置，并且可以从处的本地计算机上的浏览器访问`http://localhost`。</span><span class="sxs-lookup"><span data-stu-id="2078e-182">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="2078e-183">它也是可从远程计算机，禁止任何防火墙可能阻止访问。</span><span class="sxs-lookup"><span data-stu-id="2078e-183">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="2078e-184">检查响应标头，`Server`标头将显示 ASP.NET Core 应用程序提供的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="2078e-184">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="2078e-185">查看日志</span><span class="sxs-lookup"><span data-stu-id="2078e-185">Viewing logs</span></span>

<span data-ttu-id="2078e-186">因为 web 应用使用 Kestrel 管理使用`systemd`，到集中式日志记录所有事件和进程。</span><span class="sxs-lookup"><span data-stu-id="2078e-186">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="2078e-187">但是，此日志包含由 `systemd` 管理的所有服务和进程的全部条目。</span><span class="sxs-lookup"><span data-stu-id="2078e-187">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="2078e-188">若要查看特定于 `kestrel-hellomvc.service` 的项，请使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="2078e-188">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="2078e-189">有关进一步筛选，使用时间选项（如 `--since today`、`--until 1 hour ago`）或这些选项的组合可以减少返回的条目数。</span><span class="sxs-lookup"><span data-stu-id="2078e-189">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="2078e-190">保护应用程序</span><span class="sxs-lookup"><span data-stu-id="2078e-190">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="2078e-191">启用 AppArmor</span><span class="sxs-lookup"><span data-stu-id="2078e-191">Enable AppArmor</span></span>

<span data-ttu-id="2078e-192">Linux 安全模块 (LSM) 是一个框架，是从 Linux 2.6 Linux 内核的一部分。</span><span class="sxs-lookup"><span data-stu-id="2078e-192">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="2078e-193">LSM 支持安全模块的不同实现。</span><span class="sxs-lookup"><span data-stu-id="2078e-193">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="2078e-194">[AppArmor](https://wiki.ubuntu.com/AppArmor) 是实现强制访问控制系统的 LSM，它允许将程序限制在一组有限的资源内。</span><span class="sxs-lookup"><span data-stu-id="2078e-194">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="2078e-195">确保已启用并成功配置 AppArmor。</span><span class="sxs-lookup"><span data-stu-id="2078e-195">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="2078e-196">配置防火墙</span><span class="sxs-lookup"><span data-stu-id="2078e-196">Configuring the firewall</span></span>

<span data-ttu-id="2078e-197">关闭所有未使用的外部端口。</span><span class="sxs-lookup"><span data-stu-id="2078e-197">Close off all external ports that are not in use.</span></span> <span data-ttu-id="2078e-198">通过为配置防火墙提供命令行接口，不复杂的防火墙 (ufw) 为 `iptables` 提供了前端。</span><span class="sxs-lookup"><span data-stu-id="2078e-198">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="2078e-199">验证`ufw`配置为允许所需的任何端口上的流量。</span><span class="sxs-lookup"><span data-stu-id="2078e-199">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="2078e-200">保护 Nginx</span><span class="sxs-lookup"><span data-stu-id="2078e-200">Securing Nginx</span></span>

<span data-ttu-id="2078e-201">Nginx 的默认分配不启用 SSL。</span><span class="sxs-lookup"><span data-stu-id="2078e-201">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="2078e-202">若要启用其他安全功能，请从源生成。</span><span class="sxs-lookup"><span data-stu-id="2078e-202">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="2078e-203">下载源并安装生成依赖项</span><span class="sxs-lookup"><span data-stu-id="2078e-203">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="2078e-204">更改 Nginx 响应名称</span><span class="sxs-lookup"><span data-stu-id="2078e-204">Change the Nginx response name</span></span>

<span data-ttu-id="2078e-205">编辑 src/http/ngx_http_header_filter_module.c：</span><span class="sxs-lookup"><span data-stu-id="2078e-205">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="2078e-206">配置选项和生成</span><span class="sxs-lookup"><span data-stu-id="2078e-206">Configure the options and build</span></span>

<span data-ttu-id="2078e-207">正则表达式需要 PCRE 库。</span><span class="sxs-lookup"><span data-stu-id="2078e-207">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="2078e-208">正则表达式用于 ngx_http_rewrite_module 的位置指令。</span><span class="sxs-lookup"><span data-stu-id="2078e-208">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="2078e-209">http_ssl_module adds HTTPS 协议支持。</span><span class="sxs-lookup"><span data-stu-id="2078e-209">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="2078e-210">请考虑使用类似的 web 应用程序防火墙*ModSecurity*来加强应用程序。</span><span class="sxs-lookup"><span data-stu-id="2078e-210">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="2078e-211">配置 SSL</span><span class="sxs-lookup"><span data-stu-id="2078e-211">Configure SSL</span></span>

* <span data-ttu-id="2078e-212">配置服务器以侦听端口上的 HTTPS 流量`443`通过指定由受信任证书颁发机构 (CA) 颁发的有效证书。</span><span class="sxs-lookup"><span data-stu-id="2078e-212">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="2078e-213">通过采用的一些做法在下面的示例所示加强安全*/etc/nginx/nginx.conf*文件。</span><span class="sxs-lookup"><span data-stu-id="2078e-213">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="2078e-214">示例包括选择更强的密码并将通过 HTTP 的所有流量重定向到 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="2078e-214">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="2078e-215">添加 `HTTP Strict-Transport-Security` (HSTS) 标头可确保由客户端发起的所有后续请求都仅通过 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="2078e-215">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="2078e-216">不添加 Strict 传输安全标头或选择适当`max-age`如果将在将来禁用 SSL。</span><span class="sxs-lookup"><span data-stu-id="2078e-216">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="2078e-217">添加 /etc/nginx/proxy.conf 配置文件：</span><span class="sxs-lookup"><span data-stu-id="2078e-217">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="2078e-218">编辑 /etc/nginx/nginx.conf 配置文件。</span><span class="sxs-lookup"><span data-stu-id="2078e-218">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="2078e-219">示例包含一个配置文件中的 `http` 和 `server` 部分。</span><span class="sxs-lookup"><span data-stu-id="2078e-219">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="2078e-220">保护 Nginx 免受点击劫持的侵害</span><span class="sxs-lookup"><span data-stu-id="2078e-220">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="2078e-221">点击劫持是收集受感染用户的点击数的恶意技术。</span><span class="sxs-lookup"><span data-stu-id="2078e-221">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="2078e-222">点击劫持诱使受害者（访问者）点击受感染的网站。</span><span class="sxs-lookup"><span data-stu-id="2078e-222">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="2078e-223">使用 X-框架的选项以确保网站的安全。</span><span class="sxs-lookup"><span data-stu-id="2078e-223">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="2078e-224">编辑 nginx.conf 文件：</span><span class="sxs-lookup"><span data-stu-id="2078e-224">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="2078e-225">添加行 `add_header X-Frame-Options "SAMEORIGIN";` 并保存文件，然后重新启动 Nginx。</span><span class="sxs-lookup"><span data-stu-id="2078e-225">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="2078e-226">MIME 类型探查</span><span class="sxs-lookup"><span data-stu-id="2078e-226">MIME-type sniffing</span></span>

<span data-ttu-id="2078e-227">此标头可阻止大部分浏览器通过 MIME 方式探查来自已声明内容类型的响应，因为标头会指示浏览器不要替代响应内容类型。</span><span class="sxs-lookup"><span data-stu-id="2078e-227">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="2078e-228">使用 `nosniff` 选项后，如果服务器认为内容是“文本/html”，则浏览器将其显示为“文本/html”。</span><span class="sxs-lookup"><span data-stu-id="2078e-228">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="2078e-229">编辑 nginx.conf 文件：</span><span class="sxs-lookup"><span data-stu-id="2078e-229">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="2078e-230">添加行 `add_header X-Content-Type-Options "nosniff";` 并保存文件，然后重新启动 Nginx。</span><span class="sxs-lookup"><span data-stu-id="2078e-230">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
