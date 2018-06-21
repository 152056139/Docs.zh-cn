---
title: 托管和部署 ASP.NET Core
author: rick-anderson
description: 了解如何设置托管环境和部署 ASP.NET Core 应用。
ms.author: riande
ms.custom: mvc
ms.date: 08/07/2017
uid: host-and-deploy/index
ms.openlocfilehash: 31444475e39a12d526dd624bb508770429e414ca
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277145"
---
# <a name="host-and-deploy-aspnet-core"></a><span data-ttu-id="35dc6-103">托管和部署 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="35dc6-103">Host and deploy ASP.NET Core</span></span>

<span data-ttu-id="35dc6-104">一般而言，向托管环境部署 ASP.NET Core 应用需执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="35dc6-104">In general, to deploy an ASP.NET Core app to a hosting environment:</span></span>

* <span data-ttu-id="35dc6-105">将应用发布到托管服务器上的文件夹。</span><span class="sxs-lookup"><span data-stu-id="35dc6-105">Publish the app to a folder on the hosting server.</span></span>
* <span data-ttu-id="35dc6-106">设置进程管理器，该管理器在收到请求时启动应用，并在应用发生故障或服务器重启后重新启动应用。</span><span class="sxs-lookup"><span data-stu-id="35dc6-106">Set up a process manager that starts the app when requests arrive and restarts the app after it crashes or the server reboots.</span></span>
* <span data-ttu-id="35dc6-107">如果需要配置反向代理，设置将请求转发到应用的反向代理。</span><span class="sxs-lookup"><span data-stu-id="35dc6-107">If configuration of a reverse proxy is desired, set up a reverse proxy that forwards requests to the app.</span></span>

## <a name="publish-to-a-folder"></a><span data-ttu-id="35dc6-108">发布到文件夹</span><span class="sxs-lookup"><span data-stu-id="35dc6-108">Publish to a folder</span></span>

<span data-ttu-id="35dc6-109">[dotnet 发布](/dotnet/articles/core/tools/dotnet-publish) CLI 命令编译应用代码并复制所需的文件，以便在“发布”文件夹中运行应用。</span><span class="sxs-lookup"><span data-stu-id="35dc6-109">The [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) CLI command compiles app code and copies the files needed to run the app into a *publish* folder.</span></span> <span data-ttu-id="35dc6-110">从 Visual Studio 进行部署时，在文件被复制到部署目标前，系统会自动执行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 步骤。</span><span class="sxs-lookup"><span data-stu-id="35dc6-110">When deploying from Visual Studio, the [dotnet publish](/dotnet/core/tools/dotnet-publish) step happens automatically before the files are copied to the deployment destination.</span></span>

### <a name="folder-contents"></a><span data-ttu-id="35dc6-111">文件夹内容</span><span class="sxs-lookup"><span data-stu-id="35dc6-111">Folder contents</span></span>

<span data-ttu-id="35dc6-112">“发布”文件夹包含应用的“.exe”和“.dll”文件、应用依赖项以及 .NET 运行时（根据需要）。</span><span class="sxs-lookup"><span data-stu-id="35dc6-112">The *publish* folder contains *.exe* and *.dll* files for the app, its dependencies, and optionally the .NET runtime.</span></span>

<span data-ttu-id="35dc6-113">.NET Core 应用可以作为“独立”应用或“依赖于框架”的应用进行发布。</span><span class="sxs-lookup"><span data-stu-id="35dc6-113">A .NET Core app can be published as *self-contained* or *framework-dependent* app.</span></span> <span data-ttu-id="35dc6-114">如果应用是独立应用，则包含 .NET 运行时的“.dll”文件会包括在“发布”文件夹内。</span><span class="sxs-lookup"><span data-stu-id="35dc6-114">If the app is self-contained, the *.dll* files that contain the .NET runtime are included in the *publish* folder.</span></span> <span data-ttu-id="35dc6-115">如果应用依赖于框架，.NET 运行时文件就不包含在内，因为应用包含对服务器上安装的 .NET 版本的引用。</span><span class="sxs-lookup"><span data-stu-id="35dc6-115">If the app is framework-dependent, the .NET runtime files aren't included because the app has a reference to a version of .NET that's installed on the server.</span></span> <span data-ttu-id="35dc6-116">默认部署模型是依赖于框架的模型。</span><span class="sxs-lookup"><span data-stu-id="35dc6-116">The default deployment model is framework-dependent.</span></span> <span data-ttu-id="35dc6-117">有关详细信息，请参阅 [.NET Core 应用程序部署](/dotnet/articles/core/deploying/index)。</span><span class="sxs-lookup"><span data-stu-id="35dc6-117">For more information, see [.NET Core application deployment](/dotnet/articles/core/deploying/index).</span></span>

<span data-ttu-id="35dc6-118">除了“.exe”和“.dll”文件以外，ASP.NET Core 应用的“发布”文件夹通常包含配置文件、静态资产和 MVC 视图。</span><span class="sxs-lookup"><span data-stu-id="35dc6-118">In addition to *.exe* and *.dll* files, the *publish* folder for an ASP.NET Core app typically contains configuration files, static assets, and MVC views.</span></span> <span data-ttu-id="35dc6-119">有关详细信息，请参阅[目录结构](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="35dc6-119">For more information, see [Directory structure](xref:host-and-deploy/directory-structure).</span></span>

## <a name="set-up-a-process-manager"></a><span data-ttu-id="35dc6-120">设置进程管理器</span><span class="sxs-lookup"><span data-stu-id="35dc6-120">Set up a process manager</span></span>

<span data-ttu-id="35dc6-121">ASP.NET Core 应用是一个控制台应用，在服务器启动时必须启动该应用，并且在出现故障后必须重新启动它。</span><span class="sxs-lookup"><span data-stu-id="35dc6-121">An ASP.NET Core app is a console app that must be started when a server boots and restarted if it crashes.</span></span> <span data-ttu-id="35dc6-122">若要自动启动和重新启动，需要使用进程管理器。</span><span class="sxs-lookup"><span data-stu-id="35dc6-122">To automate starts and restarts, a process manager is required.</span></span> <span data-ttu-id="35dc6-123">用于 ASP.NET Core 的最常见进程管理器是：</span><span class="sxs-lookup"><span data-stu-id="35dc6-123">The most common process managers for ASP.NET Core are:</span></span>

* <span data-ttu-id="35dc6-124">Linux</span><span class="sxs-lookup"><span data-stu-id="35dc6-124">Linux</span></span>
  * [<span data-ttu-id="35dc6-125">Nginx</span><span class="sxs-lookup"><span data-stu-id="35dc6-125">Nginx</span></span>](xref:host-and-deploy/linux-nginx)
  * [<span data-ttu-id="35dc6-126">Apache</span><span class="sxs-lookup"><span data-stu-id="35dc6-126">Apache</span></span>](xref:host-and-deploy/linux-apache)
* <span data-ttu-id="35dc6-127">Windows</span><span class="sxs-lookup"><span data-stu-id="35dc6-127">Windows</span></span>
  * [<span data-ttu-id="35dc6-128">IIS</span><span class="sxs-lookup"><span data-stu-id="35dc6-128">IIS</span></span>](xref:host-and-deploy/iis/index)
  * [<span data-ttu-id="35dc6-129">Windows 服务</span><span class="sxs-lookup"><span data-stu-id="35dc6-129">Windows Service</span></span>](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a><span data-ttu-id="35dc6-130">设置反向代理</span><span class="sxs-lookup"><span data-stu-id="35dc6-130">Set up a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="35dc6-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="35dc6-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="35dc6-132">如果该应用使用的是 [Kestrel](xref:fundamentals/servers/kestrel) Web 服务器，则可以将 [Nginx](xref:host-and-deploy/linux-nginx)、[Apache](xref:host-and-deploy/linux-apache) 或 [IIS](xref:host-and-deploy/iis/index) 用作反向代理服务器。</span><span class="sxs-lookup"><span data-stu-id="35dc6-132">If the app uses the [Kestrel](xref:fundamentals/servers/kestrel) web server, [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), or [IIS](xref:host-and-deploy/iis/index) can be used as a reverse proxy server.</span></span> <span data-ttu-id="35dc6-133">反向代理服务器接收到来自 Internet 的 HTTP 请求，并在进行一些初步处理后将这些请求转发到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="35dc6-133">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

<span data-ttu-id="35dc6-134">使用或不使用反向代理服务器进行配置对 ASP.NET Core 2.0 或更高版本的应用来说都是有效且受支持的托管配置。</span><span class="sxs-lookup"><span data-stu-id="35dc6-134">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="35dc6-135">有关详细信息，请参阅[何时结合使用 Kestrel 和反向代理](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="35dc6-135">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="35dc6-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="35dc6-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="35dc6-137">如果该应用使用的是 [Kestrel](xref:fundamentals/servers/kestrel) Web 服务器并将被公开到 Internet，则需将 [Nginx](xref:host-and-deploy/linux-nginx)、[Apache](xref:host-and-deploy/linux-apache) 或 [IIS](xref:host-and-deploy/iis/index) 用作反向代理服务器。</span><span class="sxs-lookup"><span data-stu-id="35dc6-137">If the app uses the [Kestrel](xref:fundamentals/servers/kestrel) web server and will be exposed to the Internet, use [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), or [IIS](xref:host-and-deploy/iis/index) as a reverse proxy server.</span></span> <span data-ttu-id="35dc6-138">反向代理服务器接收到来自 Internet 的 HTTP 请求，并在进行一些初步处理后将这些请求转发到 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="35dc6-138">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span> <span data-ttu-id="35dc6-139">使用反向代理的主要原因是出于安全考虑。</span><span class="sxs-lookup"><span data-stu-id="35dc6-139">The main reason for using a reverse proxy is security.</span></span> <span data-ttu-id="35dc6-140">有关详细信息，请参阅[何时结合使用 Kestrel 和反向代理](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="35dc6-140">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

---

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="35dc6-141">代理服务器和负载均衡器方案</span><span class="sxs-lookup"><span data-stu-id="35dc6-141">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="35dc6-142">对于托管在代理服务器和负载均衡器后方的应用，可能需要附加配置。</span><span class="sxs-lookup"><span data-stu-id="35dc6-142">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="35dc6-143">如不提供附加配置，应用可能无法访问方案 (HTTP/HTTPS) 和发出请求的远程 IP 地址。</span><span class="sxs-lookup"><span data-stu-id="35dc6-143">Without additional configuration, an app might not have access to the scheme (HTTP/HTTPS) and the remote IP address where a request originated.</span></span> <span data-ttu-id="35dc6-144">有关详细信息，请参阅[配置 ASP.NET Core 以使用代理服务器和负载均衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="35dc6-144">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a><span data-ttu-id="35dc6-145">使用 Visual Studio 和 MSBuild 自动执行部署</span><span class="sxs-lookup"><span data-stu-id="35dc6-145">Using Visual Studio and MSBuild to automate deployment</span></span>

<span data-ttu-id="35dc6-146">除将输出从 [dotnet publish](/dotnet/core/tools/dotnet-publish) 复制到服务器外，部署通常还需要其他任务。</span><span class="sxs-lookup"><span data-stu-id="35dc6-146">Deployment often requires additional tasks besides copying the output from [dotnet publish](/dotnet/core/tools/dotnet-publish) to a server.</span></span> <span data-ttu-id="35dc6-147">例如，可能需要从“发布”文件夹获取或排除额外文件。</span><span class="sxs-lookup"><span data-stu-id="35dc6-147">For example, extra files might be required or excluded from the *publish* folder.</span></span> <span data-ttu-id="35dc6-148">Visual Studio 使用 MSBuild 进行 Web 部署，用户可以自定义 MSBuild 以在部署期间执行其他任务。</span><span class="sxs-lookup"><span data-stu-id="35dc6-148">Visual Studio uses MSBuild for web deployment, and MSBuild can be customized to do many other tasks during deployment.</span></span> <span data-ttu-id="35dc6-149">有关详细信息，请参阅 [Visual Studio 中的发布配置文件](xref:host-and-deploy/visual-studio-publish-profiles)和[使用 MSBuild 和 Team Foundation Build](http://msbuildbook.com/) 一书。</span><span class="sxs-lookup"><span data-stu-id="35dc6-149">For more information, see [Publish profiles in Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) and the [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) book.</span></span>

<span data-ttu-id="35dc6-150">通过[发布 Web 功能](xref:tutorials/publish-to-azure-webapp-using-vs)或[内置 Git 支持](xref:host-and-deploy/azure-apps/azure-continuous-deployment)，可以将应用从 Visual Studio 直接部署到 Azure 应用服务。</span><span class="sxs-lookup"><span data-stu-id="35dc6-150">By using [the Publish Web feature](xref:tutorials/publish-to-azure-webapp-using-vs) or [built-in Git support](xref:host-and-deploy/azure-apps/azure-continuous-deployment), apps can be deployed directly from Visual Studio to the Azure App Service.</span></span> <span data-ttu-id="35dc6-151">Visual Studio Team Services 支持[对 Azure App Service 进行持续部署](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts)。</span><span class="sxs-lookup"><span data-stu-id="35dc6-151">Visual Studio Team Services supports [continuous deployment to Azure App Service](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts).</span></span>

## <a name="publishing-to-azure"></a><span data-ttu-id="35dc6-152">发布到 Azure</span><span class="sxs-lookup"><span data-stu-id="35dc6-152">Publishing to Azure</span></span>

<span data-ttu-id="35dc6-153">有关如何使用 Visual Studio 将应用发布到 Azure 的说明，请参阅[使用 Visual Studio 将 ASP.NET Core Web 应用发布到 Azure App Service](xref:tutorials/publish-to-azure-webapp-using-vs)。</span><span class="sxs-lookup"><span data-stu-id="35dc6-153">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish an app to Azure using Visual Studio.</span></span> <span data-ttu-id="35dc6-154">还可以从[命令行](xref:tutorials/publish-to-azure-webapp-using-cli)将该应用发布到 Azure。</span><span class="sxs-lookup"><span data-stu-id="35dc6-154">The app can also be published to Azure from the [command line](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="35dc6-155">其他资源</span><span class="sxs-lookup"><span data-stu-id="35dc6-155">Additional resources</span></span>

<span data-ttu-id="35dc6-156">有关将 Docker 用作托管环境的信息，请参阅[在 Docker 中托管 ASP.NET Core 应用](xref:host-and-deploy/docker/index)。</span><span class="sxs-lookup"><span data-stu-id="35dc6-156">For information on using Docker as a hosting environment, see [Host ASP.NET Core apps in Docker](xref:host-and-deploy/docker/index).</span></span>
