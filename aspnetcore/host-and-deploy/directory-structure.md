---
title: Struktura katalogów platformy ASP.NET Core
author: guardrex
description: Więcej informacji na temat struktury katalogów opublikowane aplikacje platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 06/17/2019
uid: host-and-deploy/directory-structure
ms.openlocfilehash: f1df047decc7a0a6b7dcee57a690c55eea428b05
ms.sourcegitcommit: 28a2874765cefe9eaa068dceb989a978ba2096aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67166979"
---
# <a name="aspnet-core-directory-structure"></a><span data-ttu-id="6cb8a-103">Struktura katalogów platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6cb8a-103">ASP.NET Core directory structure</span></span>

<span data-ttu-id="6cb8a-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6cb8a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6cb8a-105">*Publikowania* katalog zawiera zasoby do wdrożenia aplikacji, utworzona przez testowany [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenia.</span><span class="sxs-lookup"><span data-stu-id="6cb8a-105">The *publish* directory contains the app's deployable assets produced by the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="6cb8a-106">Katalog zawiera:</span><span class="sxs-lookup"><span data-stu-id="6cb8a-106">The directory contains:</span></span>

* <span data-ttu-id="6cb8a-107">Pliki aplikacji</span><span class="sxs-lookup"><span data-stu-id="6cb8a-107">Application files</span></span>
* <span data-ttu-id="6cb8a-108">Pliki konfiguracji</span><span class="sxs-lookup"><span data-stu-id="6cb8a-108">Configuration files</span></span>
* <span data-ttu-id="6cb8a-109">Statyczne elementy zawartości</span><span class="sxs-lookup"><span data-stu-id="6cb8a-109">Static assets</span></span>
* <span data-ttu-id="6cb8a-110">Pakiety</span><span class="sxs-lookup"><span data-stu-id="6cb8a-110">Packages</span></span>
* <span data-ttu-id="6cb8a-111">Środowisko uruchomieniowe ([niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) tylko)</span><span class="sxs-lookup"><span data-stu-id="6cb8a-111">A runtime ([self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) only)</span></span>

| <span data-ttu-id="6cb8a-112">Typ aplikacji</span><span class="sxs-lookup"><span data-stu-id="6cb8a-112">App Type</span></span> | <span data-ttu-id="6cb8a-113">Struktura katalogów</span><span class="sxs-lookup"><span data-stu-id="6cb8a-113">Directory Structure</span></span> |
| -------- | ------------------- |
| [<span data-ttu-id="6cb8a-114">Framework-dependent Deployment</span><span class="sxs-lookup"><span data-stu-id="6cb8a-114">Framework-dependent Deployment</span></span>](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li><span data-ttu-id="6cb8a-115">Publikowanie&dagger;</span><span class="sxs-lookup"><span data-stu-id="6cb8a-115">publish&dagger;</span></span><ul><li><span data-ttu-id="6cb8a-116">Widoki&dagger; (MVC aplikacji; Jeśli widoków nie są wstępnie skompilowane)</span><span class="sxs-lookup"><span data-stu-id="6cb8a-116">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="6cb8a-117">Strony&dagger; (MVC ani stron Razor aplikacji; Jeśli strony nie są wstępnie skompilowane)</span><span class="sxs-lookup"><span data-stu-id="6cb8a-117">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="6cb8a-118">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="6cb8a-118">wwwroot&dagger;</span></span></li><li><span data-ttu-id="6cb8a-119">\*\.pliki dll</span><span class="sxs-lookup"><span data-stu-id="6cb8a-119">\*\.dll files</span></span></li><li><span data-ttu-id="6cb8a-120">{Nazwa zestawu} deps.JSON</span><span class="sxs-lookup"><span data-stu-id="6cb8a-120">{ASSEMBLY NAME}.deps.json</span></span></li><li><span data-ttu-id="6cb8a-121">{Nazwa zestawu} .dll</span><span class="sxs-lookup"><span data-stu-id="6cb8a-121">{ASSEMBLY NAME}.dll</span></span></li><li><span data-ttu-id="6cb8a-122">{Nazwa zestawu} .pdb</span><span class="sxs-lookup"><span data-stu-id="6cb8a-122">{ASSEMBLY NAME}.pdb</span></span></li><li><span data-ttu-id="6cb8a-123">{ASSEMBLY NAME}.Views.dll</span><span class="sxs-lookup"><span data-stu-id="6cb8a-123">{ASSEMBLY NAME}.Views.dll</span></span></li><li><span data-ttu-id="6cb8a-124">{NAZWA ZESTAWU}. Views.pdb</span><span class="sxs-lookup"><span data-stu-id="6cb8a-124">{ASSEMBLY NAME}.Views.pdb</span></span></li><li><span data-ttu-id="6cb8a-125">{Nazwa zestawu}.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="6cb8a-125">{ASSEMBLY NAME}.runtimeconfig.json</span></span></li><li><span data-ttu-id="6cb8a-126">plik Web.config (w przypadku wdrożeń usług IIS)</span><span class="sxs-lookup"><span data-stu-id="6cb8a-126">web.config (IIS deployments)</span></span></li></ul></li></ul> |
| [<span data-ttu-id="6cb8a-127">Niezależne wdrożenia</span><span class="sxs-lookup"><span data-stu-id="6cb8a-127">Self-contained Deployment</span></span>](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li><span data-ttu-id="6cb8a-128">Publikowanie&dagger;</span><span class="sxs-lookup"><span data-stu-id="6cb8a-128">publish&dagger;</span></span><ul><li><span data-ttu-id="6cb8a-129">Widoki&dagger; (MVC aplikacji; Jeśli widoków nie są wstępnie skompilowane)</span><span class="sxs-lookup"><span data-stu-id="6cb8a-129">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="6cb8a-130">Strony&dagger; (MVC ani stron Razor aplikacji; Jeśli strony nie są wstępnie skompilowane)</span><span class="sxs-lookup"><span data-stu-id="6cb8a-130">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="6cb8a-131">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="6cb8a-131">wwwroot&dagger;</span></span></li><li><span data-ttu-id="6cb8a-132">\*pliki z rozszerzeniem dll</span><span class="sxs-lookup"><span data-stu-id="6cb8a-132">\*.dll files</span></span></li><li><span data-ttu-id="6cb8a-133">{Nazwa zestawu} deps.JSON</span><span class="sxs-lookup"><span data-stu-id="6cb8a-133">{ASSEMBLY NAME}.deps.json</span></span></li><li><span data-ttu-id="6cb8a-134">{Nazwa zestawu} .dll</span><span class="sxs-lookup"><span data-stu-id="6cb8a-134">{ASSEMBLY NAME}.dll</span></span></li><li><span data-ttu-id="6cb8a-135">{Nazwa zestawu} .exe</span><span class="sxs-lookup"><span data-stu-id="6cb8a-135">{ASSEMBLY NAME}.exe</span></span></li><li><span data-ttu-id="6cb8a-136">{Nazwa zestawu} .pdb</span><span class="sxs-lookup"><span data-stu-id="6cb8a-136">{ASSEMBLY NAME}.pdb</span></span></li><li><span data-ttu-id="6cb8a-137">{ASSEMBLY NAME}.Views.dll</span><span class="sxs-lookup"><span data-stu-id="6cb8a-137">{ASSEMBLY NAME}.Views.dll</span></span></li><li><span data-ttu-id="6cb8a-138">{NAZWA ZESTAWU}. Views.pdb</span><span class="sxs-lookup"><span data-stu-id="6cb8a-138">{ASSEMBLY NAME}.Views.pdb</span></span></li><li><span data-ttu-id="6cb8a-139">{Nazwa zestawu}.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="6cb8a-139">{ASSEMBLY NAME}.runtimeconfig.json</span></span></li><li><span data-ttu-id="6cb8a-140">plik Web.config (w przypadku wdrożeń usług IIS)</span><span class="sxs-lookup"><span data-stu-id="6cb8a-140">web.config (IIS deployments)</span></span></li></ul></li></ul> |

<span data-ttu-id="6cb8a-141">&dagger;Określa katalog</span><span class="sxs-lookup"><span data-stu-id="6cb8a-141">&dagger;Indicates a directory</span></span>

<span data-ttu-id="6cb8a-142">*Publikowania* reprezentuje katalog *ścieżka katalogu głównego zawartości*, nazywane również *ścieżki podstawowej aplikacji*, wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="6cb8a-142">The *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="6cb8a-143">Jest nadać dowolną nazwę *publikowania* katalogu wdrożonej aplikacji na serwerze, jego lokalizacji służy jako serwer ścieżka fizyczna do hostowanych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6cb8a-143">Whatever name is given to the *publish* directory of the deployed app on the server, its location serves as the server's physical path to the hosted app.</span></span>

<span data-ttu-id="6cb8a-144">*Wwwroot* katalogu, jeśli jest obecny, zawiera tylko statyczne elementy zawartości.</span><span class="sxs-lookup"><span data-stu-id="6cb8a-144">The *wwwroot* directory, if present, only contains static assets.</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6cb8a-145">Tworzenie *dzienniki* folderu jest przydatne w przypadku [modułu ASP.NET Core ulepszone rejestrowanie debugowania](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="6cb8a-145">Creating a *Logs* folder is useful for [ASP.NET Core Module enhanced debug logging](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="6cb8a-146">Foldery w ścieżce podanej do `<handlerSetting>` wartości nie są tworzone automatycznie przez moduł i wstępnie powinno istnieć we wdrożeniu, aby umożliwić modułu do zapisywania dzienników debugowania.</span><span class="sxs-lookup"><span data-stu-id="6cb8a-146">Folders in the path provided to the `<handlerSetting>` value aren't created by the module automatically and should pre-exist in the deployment to allow the module to write the debug log.</span></span>

<span data-ttu-id="6cb8a-147">A *dzienniki* katalogu mogą być tworzone dla wdrożenia przy użyciu jednej z następujących dwóch metod:</span><span class="sxs-lookup"><span data-stu-id="6cb8a-147">A *Logs* directory can be created for the deployment using one of the following two approaches:</span></span>

* <span data-ttu-id="6cb8a-148">Dodaj następujący kod `<Target>` element do pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="6cb8a-148">Add the following `<Target>` element to the project file:</span></span>

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

   <span data-ttu-id="6cb8a-149">`<MakeDir>` Element utworzy pustą *dzienniki* folderu w opublikowanych danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="6cb8a-149">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="6cb8a-150">Element używa `PublishDir` właściwości w celu określenia lokalizacji docelowej dla tworzenia folderu.</span><span class="sxs-lookup"><span data-stu-id="6cb8a-150">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="6cb8a-151">Kilka metod wdrażania, takie jak narzędzie Web Deploy, Pomiń pustych folderów podczas wdrażania.</span><span class="sxs-lookup"><span data-stu-id="6cb8a-151">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="6cb8a-152">`<WriteLinesToFile>` Element generuje plik w *dzienniki* folderu, który gwarantuje wdrożenia folderu na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6cb8a-152">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="6cb8a-153">Tworzenie folderu przy użyciu tej metody nie powiedzie się, jeśli proces roboczy nie ma dostępu do zapisu do folderu docelowego.</span><span class="sxs-lookup"><span data-stu-id="6cb8a-153">Folder creation using this approach fails if the worker process doesn't have write access to the target folder.</span></span>

* <span data-ttu-id="6cb8a-154">Utwórz fizycznie *dzienniki* katalogu na serwerze w trakcie wdrażania.</span><span class="sxs-lookup"><span data-stu-id="6cb8a-154">Physically create the *Logs* directory on the server in the deployment.</span></span>

<span data-ttu-id="6cb8a-155">Katalog wdrażania musi mieć uprawnienia odczytu/Execute.</span><span class="sxs-lookup"><span data-stu-id="6cb8a-155">The deployment directory requires Read/Execute permissions.</span></span> <span data-ttu-id="6cb8a-156">*Dzienniki* katalogu musi mieć uprawnienia odczytu/zapisu.</span><span class="sxs-lookup"><span data-stu-id="6cb8a-156">The *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="6cb8a-157">Dodatkowe katalogi, w którym są zapisywane pliki muszą mieć uprawnienia odczytu/zapisu.</span><span class="sxs-lookup"><span data-stu-id="6cb8a-157">Additional directories where files are written require Read/Write permissions.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6cb8a-158">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6cb8a-158">Additional resources</span></span>

* [<span data-ttu-id="6cb8a-159">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="6cb8a-159">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* [<span data-ttu-id="6cb8a-160">Wdrażanie aplikacji .NET core</span><span class="sxs-lookup"><span data-stu-id="6cb8a-160">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* [<span data-ttu-id="6cb8a-161">Platformy docelowe</span><span class="sxs-lookup"><span data-stu-id="6cb8a-161">Target frameworks</span></span>](/dotnet/standard/frameworks)
* [<span data-ttu-id="6cb8a-162">.NET core RID katalogu</span><span class="sxs-lookup"><span data-stu-id="6cb8a-162">.NET Core RID Catalog</span></span>](/dotnet/core/rid-catalog)
