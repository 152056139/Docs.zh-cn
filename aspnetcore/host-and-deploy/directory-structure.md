---
title: ASP.NET 核心目录结构
author: guardrex
description: 了解有关已发布 ASP.NET Core 应用的目录结构。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: ac9b777bcc7f4a8634161fc1347a4d0fdc3b4784
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/19/2018
---
# <a name="aspnet-core-directory-structure"></a><span data-ttu-id="8d07b-103">ASP.NET 核心目录结构</span><span class="sxs-lookup"><span data-stu-id="8d07b-103">ASP.NET Core directory structure</span></span>

<span data-ttu-id="8d07b-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8d07b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8d07b-105">在 ASP.NET 核，发布的应用程序目录中，*发布*，组成应用程序文件、 配置文件、 静态资产、 包和运行时 (有关[自包含的部署](/dotnet/core/deploying/#self-contained-deployments-scd))。</span><span class="sxs-lookup"><span data-stu-id="8d07b-105">In ASP.NET Core, the published application directory, *publish*, is comprised of application files, config files, static assets, packages, and the runtime (for [self-contained deployments](/dotnet/core/deploying/#self-contained-deployments-scd)).</span></span>


| <span data-ttu-id="8d07b-106">应用类型</span><span class="sxs-lookup"><span data-stu-id="8d07b-106">App Type</span></span> | <span data-ttu-id="8d07b-107">目录结构</span><span class="sxs-lookup"><span data-stu-id="8d07b-107">Directory Structure</span></span> |
| -------- | ------------------- |
| [<span data-ttu-id="8d07b-108">依赖于框架的部署</span><span class="sxs-lookup"><span data-stu-id="8d07b-108">Framework-dependent Deployment</span></span>](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li><span data-ttu-id="8d07b-109">发布&dagger;</span><span class="sxs-lookup"><span data-stu-id="8d07b-109">publish&dagger;</span></span><ul><li><span data-ttu-id="8d07b-110">日志&dagger;（除非需要接收 stdout 日志可选）</span><span class="sxs-lookup"><span data-stu-id="8d07b-110">logs&dagger; (optional unless required to receive stdout logs)</span></span></li><li><span data-ttu-id="8d07b-111">视图&dagger;（MVC 应用程序; 如果不预编译视图）</span><span class="sxs-lookup"><span data-stu-id="8d07b-111">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="8d07b-112">页&dagger;（MVC 或 Razor 页应用; 如果不预编译页）</span><span class="sxs-lookup"><span data-stu-id="8d07b-112">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="8d07b-113">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="8d07b-113">wwwroot&dagger;</span></span></li><li><span data-ttu-id="8d07b-114">\*\.dll 文件</span><span class="sxs-lookup"><span data-stu-id="8d07b-114">\*\.dll files</span></span></li><li><span data-ttu-id="8d07b-115">\<程序集名称 >。 deps.json</span><span class="sxs-lookup"><span data-stu-id="8d07b-115">\<assembly-name>.deps.json</span></span></li><li><span data-ttu-id="8d07b-116">\<程序集名称 >.dll</span><span class="sxs-lookup"><span data-stu-id="8d07b-116">\<assembly-name>.dll</span></span></li><li><span data-ttu-id="8d07b-117">\<程序集名称 >.pdb</span><span class="sxs-lookup"><span data-stu-id="8d07b-117">\<assembly-name>.pdb</span></span></li><li><span data-ttu-id="8d07b-118">\<程序集名称 >。PrecompiledViews.dll</span><span class="sxs-lookup"><span data-stu-id="8d07b-118">\<assembly-name>.PrecompiledViews.dll</span></span></li><li><span data-ttu-id="8d07b-119">\<程序集名称 >。PrecompiledViews.pdb</span><span class="sxs-lookup"><span data-stu-id="8d07b-119">\<assembly-name>.PrecompiledViews.pdb</span></span></li><li><span data-ttu-id="8d07b-120">\<程序集名称 >。 runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="8d07b-120">\<assembly-name>.runtimeconfig.json</span></span></li><li><span data-ttu-id="8d07b-121">web.config （IIS 部署）</span><span class="sxs-lookup"><span data-stu-id="8d07b-121">web.config (IIS deployments)</span></span></li></ul></li></ul> |
| [<span data-ttu-id="8d07b-122">自包含的部署</span><span class="sxs-lookup"><span data-stu-id="8d07b-122">Self-contained Deployment</span></span>](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li><span data-ttu-id="8d07b-123">发布&dagger;</span><span class="sxs-lookup"><span data-stu-id="8d07b-123">publish&dagger;</span></span><ul><li><span data-ttu-id="8d07b-124">日志&dagger;（除非需要接收 stdout 日志可选）</span><span class="sxs-lookup"><span data-stu-id="8d07b-124">logs&dagger; (optional unless required to receive stdout logs)</span></span></li><li><span data-ttu-id="8d07b-125">refs&dagger;</span><span class="sxs-lookup"><span data-stu-id="8d07b-125">refs&dagger;</span></span></li><li><span data-ttu-id="8d07b-126">视图&dagger;（MVC 应用程序; 如果不预编译视图）</span><span class="sxs-lookup"><span data-stu-id="8d07b-126">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="8d07b-127">页&dagger;（MVC 或 Razor 页应用; 如果不预编译页）</span><span class="sxs-lookup"><span data-stu-id="8d07b-127">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="8d07b-128">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="8d07b-128">wwwroot&dagger;</span></span></li><li><span data-ttu-id="8d07b-129">\*.dll 文件</span><span class="sxs-lookup"><span data-stu-id="8d07b-129">\*.dll files</span></span></li><li><span data-ttu-id="8d07b-130">\<程序集名称 >。 deps.json</span><span class="sxs-lookup"><span data-stu-id="8d07b-130">\<assembly-name>.deps.json</span></span></li><li><span data-ttu-id="8d07b-131">\<程序集名称 >.exe</span><span class="sxs-lookup"><span data-stu-id="8d07b-131">\<assembly-name>.exe</span></span></li><li><span data-ttu-id="8d07b-132">\<程序集名称 >.pdb</span><span class="sxs-lookup"><span data-stu-id="8d07b-132">\<assembly-name>.pdb</span></span></li><li><span data-ttu-id="8d07b-133">\<程序集名称 >。PrecompiledViews.dll</span><span class="sxs-lookup"><span data-stu-id="8d07b-133">\<assembly-name>.PrecompiledViews.dll</span></span></li><li><span data-ttu-id="8d07b-134">\<程序集名称 >。PrecompiledViews.pdb</span><span class="sxs-lookup"><span data-stu-id="8d07b-134">\<assembly-name>.PrecompiledViews.pdb</span></span></li><li><span data-ttu-id="8d07b-135">\<程序集名称 >。 runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="8d07b-135">\<assembly-name>.runtimeconfig.json</span></span></li><li><span data-ttu-id="8d07b-136">web.config （IIS 部署）</span><span class="sxs-lookup"><span data-stu-id="8d07b-136">web.config (IIS deployments)</span></span></li></ul></li></ul> |

<span data-ttu-id="8d07b-137">&dagger;指示目录</span><span class="sxs-lookup"><span data-stu-id="8d07b-137">&dagger;Indicates a directory</span></span>

<span data-ttu-id="8d07b-138">*发布*目录表示*内容的根路径*，也称为*应用程序基路径*，部署。</span><span class="sxs-lookup"><span data-stu-id="8d07b-138">The *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="8d07b-139">任何名称提供给*发布*目录中的服务器上部署的应用程序，其位置用作托管的应用程序服务器的物理路径。</span><span class="sxs-lookup"><span data-stu-id="8d07b-139">Whatever name is given to the *publish* directory of the deployed app on the server, its location serves as the server's physical path to the hosted app.</span></span>

<span data-ttu-id="8d07b-140">*Wwwroot*目录中，如果存在，仅包含静态资产。</span><span class="sxs-lookup"><span data-stu-id="8d07b-140">The *wwwroot* directory, if present, only contains static assets.</span></span>

<span data-ttu-id="8d07b-141">Stdout*日志*可以使用以下两种方法之一部署创建目录：</span><span class="sxs-lookup"><span data-stu-id="8d07b-141">The stdout *logs* directory can be created the deployment using one of the following two approaches:</span></span>

* <span data-ttu-id="8d07b-142">添加以下`<Target>`项目文件的元素：</span><span class="sxs-lookup"><span data-stu-id="8d07b-142">Add the following `<Target>` element to the project file:</span></span>

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="Publish">
     <MakeDir Directories="$(PublishDir)Logs" 
              Condition="!Exists('$(PublishDir)Logs')" />
     <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                       Lines="Generated file" 
                       Overwrite="True" 
                       Condition="!Exists('$(PublishDir)Logs\.log')" />
   </Target>
   ```

   <span data-ttu-id="8d07b-143">`<MakeDir>`元素创建一个空*日志*中已发布的输出文件夹。</span><span class="sxs-lookup"><span data-stu-id="8d07b-143">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="8d07b-144">元素使用`PublishDir`属性来确定用于创建文件夹的目标位置。</span><span class="sxs-lookup"><span data-stu-id="8d07b-144">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="8d07b-145">多种部署方法，例如 Web 部署，在部署过程中跳过空文件夹。</span><span class="sxs-lookup"><span data-stu-id="8d07b-145">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="8d07b-146">`<WriteLinesToFile>`元素生成的文件中*日志*文件夹中，这可确保部署到服务器的文件夹。</span><span class="sxs-lookup"><span data-stu-id="8d07b-146">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="8d07b-147">请注意，如果工作进程不具有写访问权限的目标文件夹的文件夹创建可能仍会失败。</span><span class="sxs-lookup"><span data-stu-id="8d07b-147">Note that folder creation may still fail if the worker process doesn't have write access to the target folder.</span></span>

* <span data-ttu-id="8d07b-148">以物理方式创建*日志*目录部署中的服务器。</span><span class="sxs-lookup"><span data-stu-id="8d07b-148">Physically create the *Logs* directory on the server in the deployment.</span></span>

<span data-ttu-id="8d07b-149">部署目录中需要读取/Execute 权限。</span><span class="sxs-lookup"><span data-stu-id="8d07b-149">The deployment directory requires Read/Execute permissions.</span></span> <span data-ttu-id="8d07b-150">*日志*目录需要读/写权限。</span><span class="sxs-lookup"><span data-stu-id="8d07b-150">The *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="8d07b-151">其中写入文件的其他目录需要读/写权限。</span><span class="sxs-lookup"><span data-stu-id="8d07b-151">Additional directories where files are written require Read/Write permissions.</span></span>
