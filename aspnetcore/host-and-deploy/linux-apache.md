---
title: "使用 Apache 在 Linux 上托管 ASP.NET Core"
description: "了解如何设置 Apache 为反向代理服务器在 CentOS 中将 HTTP 流量重定向到 ASP.NET 核心 web 应用程序在 Kestrel 上运行。"
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 10/19/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: b11bc811b6aefce22b60a28afd72c2a2d0b26955
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="8854a-103">使用 Apache 在 Linux 上托管 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8854a-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="8854a-104">作者：[Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="8854a-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="8854a-105">使用本指南中，了解如何设置[Apache](https://httpd.apache.org/)为反向代理服务器上[CentOS 7](https://www.centos.org/)将 HTTP 流量重定向到 ASP.NET 核心 web 应用程序上运行[Kestrel](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="8854a-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="8854a-106">[Mod_proxy 扩展](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html)和相关的模块创建服务器的反向代理。</span><span class="sxs-lookup"><span data-stu-id="8854a-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8854a-107">系统必备</span><span class="sxs-lookup"><span data-stu-id="8854a-107">Prerequisites</span></span>

1. <span data-ttu-id="8854a-108">具有 sudo 特权的标准用户帐户运行 CentOS 7 服务器</span><span class="sxs-lookup"><span data-stu-id="8854a-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="8854a-109">ASP.NET Core 应用</span><span class="sxs-lookup"><span data-stu-id="8854a-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="8854a-110">发布应用</span><span class="sxs-lookup"><span data-stu-id="8854a-110">Publish the app</span></span>

<span data-ttu-id="8854a-111">发布应用程序作为[独立的部署](/dotnet/core/deploying/#self-contained-deployments-scd)CentOS 7 运行时的版本配置中 (`centos.7-x64`)。</span><span class="sxs-lookup"><span data-stu-id="8854a-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="8854a-112">内容复制*bin/Release/netcoreapp2.0/centos.7-x64/publish*使用 SCP、 FTP 或其他文件传输方法向服务器的文件夹。</span><span class="sxs-lookup"><span data-stu-id="8854a-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="8854a-113">在生产部署场景，持续集成工作流执行的工作的发布应用程序和复制到服务器的资产。</span><span class="sxs-lookup"><span data-stu-id="8854a-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="8854a-114">配置代理服务器</span><span class="sxs-lookup"><span data-stu-id="8854a-114">Configure a proxy server</span></span>

<span data-ttu-id="8854a-115">反向代理是为动态 web 应用程序提供服务的常见设置。</span><span class="sxs-lookup"><span data-stu-id="8854a-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="8854a-116">反向代理终止 HTTP 请求，并将其转发到 ASP.NET 应用程序。</span><span class="sxs-lookup"><span data-stu-id="8854a-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="8854a-117">代理服务器，则其中一个客户端将请求转发到另一个服务器而不是本身满足请求。</span><span class="sxs-lookup"><span data-stu-id="8854a-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="8854a-118">反向代理转发到固定的目标，通常代表任意客户端。</span><span class="sxs-lookup"><span data-stu-id="8854a-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="8854a-119">在本指南中，Apache 正在配置为在同一服务器上运行 Kestrel 为提供 ASP.NET Core 应用程序服务的反向代理。</span><span class="sxs-lookup"><span data-stu-id="8854a-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="8854a-120">因为通过反向代理转发请求，使用从转发标头 Middleware [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/)包。</span><span class="sxs-lookup"><span data-stu-id="8854a-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="8854a-121">中间件更新`Request.Scheme`，使用`X-Forwarded-Proto`标头，因此该重定向 Uri 和其他安全策略正常工作。</span><span class="sxs-lookup"><span data-stu-id="8854a-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="8854a-122">当使用任何类型的身份验证中间件，转发标头中间件必须运行第一个。</span><span class="sxs-lookup"><span data-stu-id="8854a-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="8854a-123">此顺序可确保身份验证中间件可使用的标头值并生成正确的重定向 Uri。</span><span class="sxs-lookup"><span data-stu-id="8854a-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8854a-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8854a-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8854a-125">调用[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)中的方法`Startup.Configure`之前调用[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication)或类似的身份验证方案中间件：</span><span class="sxs-lookup"><span data-stu-id="8854a-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8854a-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8854a-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8854a-127">调用[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)中的方法`Startup.Configure`之前调用[UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity)和[UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication)或类似的身份验证方案中间件：</span><span class="sxs-lookup"><span data-stu-id="8854a-127">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware:</span></span>

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

<span data-ttu-id="8854a-128">如果没有[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)指定到中间件，要转发的默认标头是`None`。</span><span class="sxs-lookup"><span data-stu-id="8854a-128">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

### <a name="install-apache"></a><span data-ttu-id="8854a-129">安装 Apache</span><span class="sxs-lookup"><span data-stu-id="8854a-129">Install Apache</span></span>

<span data-ttu-id="8854a-130">更新 CentOS 程序包添加到其最新稳定版本：</span><span class="sxs-lookup"><span data-stu-id="8854a-130">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="8854a-131">使用单个在 CentOS 上安装 Apache web 服务器`yum`命令：</span><span class="sxs-lookup"><span data-stu-id="8854a-131">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="8854a-132">运行该命令后输出的示例：</span><span class="sxs-lookup"><span data-stu-id="8854a-132">Sample output after running the command:</span></span>

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> <span data-ttu-id="8854a-133">在此示例中，输出将反映 httpd.86_64，由于 CentOS 7 版本是 64 位。</span><span class="sxs-lookup"><span data-stu-id="8854a-133">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="8854a-134">若要验证 Apache 的安装位置，请从命令提示符运行 `whereis httpd`。</span><span class="sxs-lookup"><span data-stu-id="8854a-134">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="8854a-135">配置 Apache 用于反向代理</span><span class="sxs-lookup"><span data-stu-id="8854a-135">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="8854a-136">Apache 的配置文件位于 `/etc/httpd/conf.d/` 目录内。</span><span class="sxs-lookup"><span data-stu-id="8854a-136">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="8854a-137">具有的所有文件*.conf*扩展处理除了中的模块配置文件按字母顺序`/etc/httpd/conf.modules.d/`，其中包含的任何配置所需加载模块文件。</span><span class="sxs-lookup"><span data-stu-id="8854a-137">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="8854a-138">创建名为的应用配置文件`hellomvc.conf`:</span><span class="sxs-lookup"><span data-stu-id="8854a-138">Create a configuration file for the app named `hellomvc.conf`:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="8854a-139">**VirtualHost**节点可以在服务器上的一个或多个文件中出现多次。</span><span class="sxs-lookup"><span data-stu-id="8854a-139">The **VirtualHost** node can appear multiple times in one or more files on a server.</span></span> <span data-ttu-id="8854a-140">**VirtualHost**设置为使用端口 80 的任何 IP 地址上侦听。</span><span class="sxs-lookup"><span data-stu-id="8854a-140">**VirtualHost** is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="8854a-141">接下来的两行在端口 5000 上设置为根目录下到上 127.0.0.1 的服务器的代理请求。</span><span class="sxs-lookup"><span data-stu-id="8854a-141">The next two lines are set to proxy requests at the root to the server at 127.0.0.1 on port 5000.</span></span> <span data-ttu-id="8854a-142">对于双向通信， *ProxyPass*和*ProxyPassReverse*所需。</span><span class="sxs-lookup"><span data-stu-id="8854a-142">For bi-directional communication, *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="8854a-143">可以每个配置日志记录**VirtualHost**使用**ErrorLog**和**CustomLog**指令。</span><span class="sxs-lookup"><span data-stu-id="8854a-143">Logging can be configured per **VirtualHost** using **ErrorLog** and **CustomLog** directives.</span></span> <span data-ttu-id="8854a-144">**错误日志**是服务器用来记录错误的位置和**CustomLog**设置的文件名和日志文件格式。</span><span class="sxs-lookup"><span data-stu-id="8854a-144">**ErrorLog** is the location where the server logs errors, and **CustomLog** sets the filename and format of log file.</span></span> <span data-ttu-id="8854a-145">在这种情况下，这是记录请求信息的位置。</span><span class="sxs-lookup"><span data-stu-id="8854a-145">In this case, this is where request information is logged.</span></span> <span data-ttu-id="8854a-146">没有为每个请求的一行。</span><span class="sxs-lookup"><span data-stu-id="8854a-146">There's one line for each request.</span></span>

<span data-ttu-id="8854a-147">将文件保存与测试配置。</span><span class="sxs-lookup"><span data-stu-id="8854a-147">Save the file and test the configuration.</span></span> <span data-ttu-id="8854a-148">如果一切正常，响应应为 `Syntax [OK]`。</span><span class="sxs-lookup"><span data-stu-id="8854a-148">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="8854a-149">重新启动 Apache:</span><span class="sxs-lookup"><span data-stu-id="8854a-149">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="8854a-150">监视应用程序</span><span class="sxs-lookup"><span data-stu-id="8854a-150">Monitoring the app</span></span>

<span data-ttu-id="8854a-151">Apache 现在已设置为转发到发出的请求`http://localhost:80`Kestrel 在上运行 ASP.NET Core 应用到`http://127.0.0.1:5000`。</span><span class="sxs-lookup"><span data-stu-id="8854a-151">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="8854a-152">但是，Apache 未设置来管理 Kestrel 过程。</span><span class="sxs-lookup"><span data-stu-id="8854a-152">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="8854a-153">使用*systemd*和创建服务文件以启动和监视基础的 web 应用。</span><span class="sxs-lookup"><span data-stu-id="8854a-153">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="8854a-154">systemd 是一个 init 系统，可以提供用于启动、停止和管理进程的许多强大的功能。</span><span class="sxs-lookup"><span data-stu-id="8854a-154">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="8854a-155">创建服务文件</span><span class="sxs-lookup"><span data-stu-id="8854a-155">Create the service file</span></span>

<span data-ttu-id="8854a-156">创建服务定义文件：</span><span class="sxs-lookup"><span data-stu-id="8854a-156">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="8854a-157">应用程序示例服务文件：</span><span class="sxs-lookup"><span data-stu-id="8854a-157">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if dotnet service crashes
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="8854a-158">**用户**&mdash;如果用户*apache*未使用的配置，用户必须创建第一次并且为文件提供的适当的所有权。</span><span class="sxs-lookup"><span data-stu-id="8854a-158">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="8854a-159">保存该文件并启用该服务：</span><span class="sxs-lookup"><span data-stu-id="8854a-159">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="8854a-160">启动服务，然后验证它正在运行：</span><span class="sxs-lookup"><span data-stu-id="8854a-160">Start the service and verify that it's running:</span></span>

```bash
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="8854a-161">使用反向代理配置和通过管理的 Kestrel *systemd*，web 应用进行了完全配置，并且可以从处的本地计算机上的浏览器访问`http://localhost`。</span><span class="sxs-lookup"><span data-stu-id="8854a-161">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="8854a-162">检查响应标头，**服务器**标头指示 ASP.NET Core 应用由 Kestrel:</span><span class="sxs-lookup"><span data-stu-id="8854a-162">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="8854a-163">查看日志</span><span class="sxs-lookup"><span data-stu-id="8854a-163">Viewing logs</span></span>

<span data-ttu-id="8854a-164">因为 web 应用使用 Kestrel 管理使用*systemd*，到集中式日志记录事件和进程。</span><span class="sxs-lookup"><span data-stu-id="8854a-164">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="8854a-165">但是，此日志包含的服务和由托管的进程的所有条目*systemd*。</span><span class="sxs-lookup"><span data-stu-id="8854a-165">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="8854a-166">若要查看特定于 `kestrel-hellomvc.service` 的项，请使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="8854a-166">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="8854a-167">时间筛选，使用命令中指定时间选项。</span><span class="sxs-lookup"><span data-stu-id="8854a-167">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="8854a-168">例如，使用`--since today`来筛选出存在当天或`--until 1 hour ago`来查看前一小时的条目。</span><span class="sxs-lookup"><span data-stu-id="8854a-168">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="8854a-169">有关详细信息，请参阅[journalctl 手册页](https://www.unix.com/man-page/centos/1/journalctl/)。</span><span class="sxs-lookup"><span data-stu-id="8854a-169">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="8854a-170">保护应用程序</span><span class="sxs-lookup"><span data-stu-id="8854a-170">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="8854a-171">配置防火墙</span><span class="sxs-lookup"><span data-stu-id="8854a-171">Configure firewall</span></span>

<span data-ttu-id="8854a-172">*Firewalld*是动态的守护程序，来管理具有对网络区域支持的防火墙。</span><span class="sxs-lookup"><span data-stu-id="8854a-172">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="8854a-173">端口和数据包筛选仍可通过 iptables 管理。</span><span class="sxs-lookup"><span data-stu-id="8854a-173">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="8854a-174">*Firewalld*默认情况下应安装。</span><span class="sxs-lookup"><span data-stu-id="8854a-174">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="8854a-175">`yum` 可以用于安装包或验证已安装。</span><span class="sxs-lookup"><span data-stu-id="8854a-175">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="8854a-176">使用`firewalld`以打开仅为应用程序所需的端口。</span><span class="sxs-lookup"><span data-stu-id="8854a-176">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="8854a-177">在此示例中，使用的是端口 80 和 443。</span><span class="sxs-lookup"><span data-stu-id="8854a-177">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="8854a-178">以下命令永久设置端口 80 和 443 以打开：</span><span class="sxs-lookup"><span data-stu-id="8854a-178">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="8854a-179">重新加载防火墙设置。</span><span class="sxs-lookup"><span data-stu-id="8854a-179">Reload the firewall settings.</span></span> <span data-ttu-id="8854a-180">检查可用的服务和默认区域中的端口。</span><span class="sxs-lookup"><span data-stu-id="8854a-180">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="8854a-181">选项均可通过检查`firewall-cmd -h`。</span><span class="sxs-lookup"><span data-stu-id="8854a-181">Options are available by inspecting `firewall-cmd -h`.</span></span>

```bash 
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="ssl-configuration"></a><span data-ttu-id="8854a-182">SSL 配置</span><span class="sxs-lookup"><span data-stu-id="8854a-182">SSL configuration</span></span>

<span data-ttu-id="8854a-183">若要配置 ssl，Apache*如果*使用模块。</span><span class="sxs-lookup"><span data-stu-id="8854a-183">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="8854a-184">当*httpd*模块已安装，*如果*同时安装模块。</span><span class="sxs-lookup"><span data-stu-id="8854a-184">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="8854a-185">如果它未安装，请使用`yum`以将其添加到配置。</span><span class="sxs-lookup"><span data-stu-id="8854a-185">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="8854a-186">若要强制实施 SSL，请安装`mod_rewrite`模块以启用 URL 重写：</span><span class="sxs-lookup"><span data-stu-id="8854a-186">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="8854a-187">修改*hellomvc.conf*文件启用 URL 重写且安全的端口 443 上的通信：</span><span class="sxs-lookup"><span data-stu-id="8854a-187">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="8854a-188">此示例中使用的本地生成的证书。</span><span class="sxs-lookup"><span data-stu-id="8854a-188">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="8854a-189">**SSLCertificateFile**应为域名称的主证书文件。</span><span class="sxs-lookup"><span data-stu-id="8854a-189">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="8854a-190">**SSLCertificateKeyFile**密钥文件时应生成创建 CSR。</span><span class="sxs-lookup"><span data-stu-id="8854a-190">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="8854a-191">**SSLCertificateChainFile**应中间证书文件 （如果有），由证书颁发机构提供的。</span><span class="sxs-lookup"><span data-stu-id="8854a-191">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="8854a-192">保存该文件并测试配置：</span><span class="sxs-lookup"><span data-stu-id="8854a-192">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="8854a-193">重新启动 Apache:</span><span class="sxs-lookup"><span data-stu-id="8854a-193">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="8854a-194">其他 Apache 建议</span><span class="sxs-lookup"><span data-stu-id="8854a-194">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="8854a-195">其他标头</span><span class="sxs-lookup"><span data-stu-id="8854a-195">Additional headers</span></span>

<span data-ttu-id="8854a-196">为了保护免受恶意攻击，有几个标头应被修改或添加。</span><span class="sxs-lookup"><span data-stu-id="8854a-196">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="8854a-197">确保`mod_headers`安装模块：</span><span class="sxs-lookup"><span data-stu-id="8854a-197">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="8854a-198">从 clickjacking 攻击的安全 Apache</span><span class="sxs-lookup"><span data-stu-id="8854a-198">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="8854a-199">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)，也称为*UI 改变现有攻击*，是一种恶意攻击其中诱骗网站访问者不是它们当前正在访问单击的链接或另一页上的按钮。</span><span class="sxs-lookup"><span data-stu-id="8854a-199">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="8854a-200">使用`X-FRAME-OPTIONS`以确保网站的安全。</span><span class="sxs-lookup"><span data-stu-id="8854a-200">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="8854a-201">编辑*httpd.conf*文件：</span><span class="sxs-lookup"><span data-stu-id="8854a-201">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="8854a-202">将行添加`Header append X-FRAME-OPTIONS "SAMEORIGIN"`。</span><span class="sxs-lookup"><span data-stu-id="8854a-202">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="8854a-203">保存该文件。</span><span class="sxs-lookup"><span data-stu-id="8854a-203">Save the file.</span></span> <span data-ttu-id="8854a-204">重启 Apache。</span><span class="sxs-lookup"><span data-stu-id="8854a-204">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="8854a-205">MIME 类型探查</span><span class="sxs-lookup"><span data-stu-id="8854a-205">MIME-type sniffing</span></span>

<span data-ttu-id="8854a-206">`X-Content-Type-Options`标头会阻止从 Internet Explorer *MIME 探查*(确定文件的`Content-Type`从该文件的内容)。</span><span class="sxs-lookup"><span data-stu-id="8854a-206">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="8854a-207">如果服务器设置`Content-Type`标头到`text/html`与`nosniff`选项集，Internet Explorer 呈现作为内容`text/html`不考虑文件的内容。</span><span class="sxs-lookup"><span data-stu-id="8854a-207">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="8854a-208">编辑*httpd.conf*文件：</span><span class="sxs-lookup"><span data-stu-id="8854a-208">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="8854a-209">将行添加`Header set X-Content-Type-Options "nosniff"`。</span><span class="sxs-lookup"><span data-stu-id="8854a-209">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="8854a-210">保存该文件。</span><span class="sxs-lookup"><span data-stu-id="8854a-210">Save the file.</span></span> <span data-ttu-id="8854a-211">重启 Apache。</span><span class="sxs-lookup"><span data-stu-id="8854a-211">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="8854a-212">负载平衡</span><span class="sxs-lookup"><span data-stu-id="8854a-212">Load Balancing</span></span> 

<span data-ttu-id="8854a-213">此示例演示如何在同一实例计算机上的 CentOS 7 和 Kestrel 上设置和配置 Apache。</span><span class="sxs-lookup"><span data-stu-id="8854a-213">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="8854a-214">为了不具有单点故障;使用*mod_proxy_balancer*和修改**VirtualHost**将允许用于管理 Apache 代理服务器后面的 web 应用的多个实例。</span><span class="sxs-lookup"><span data-stu-id="8854a-214">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="8854a-215">在配置文件中下面所示的其他实例`hellomvc`应用已设置为在端口 5001 上运行。</span><span class="sxs-lookup"><span data-stu-id="8854a-215">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="8854a-216">*代理*部分设置与平衡器配置具有两个成员进行负载平衡*byrequests*。</span><span class="sxs-lookup"><span data-stu-id="8854a-216">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="8854a-217">速率限制</span><span class="sxs-lookup"><span data-stu-id="8854a-217">Rate Limits</span></span>
<span data-ttu-id="8854a-218">使用*mod_ratelimit*中, 附带*httpd*模块，客户端的带宽可将限制：</span><span class="sxs-lookup"><span data-stu-id="8854a-218">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="8854a-219">示例文件为 600 KB/秒根位置下限制带宽：</span><span class="sxs-lookup"><span data-stu-id="8854a-219">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
