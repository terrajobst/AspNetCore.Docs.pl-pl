---
title: Struktura katalogów ASP.NET Core
author: guardrex
description: Więcej informacji na temat struktury katalogów opublikowane aplikacje platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 01/28/2020
uid: host-and-deploy/directory-structure
ms.openlocfilehash: ba5cb96dfdcdca10034299e3bbe662ce056af791
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2020
ms.locfileid: "76870269"
---
# <a name="aspnet-core-directory-structure"></a><span data-ttu-id="4eca8-103">Struktura katalogów ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4eca8-103">ASP.NET Core directory structure</span></span>

<span data-ttu-id="4eca8-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4eca8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4eca8-105">Katalog *publikowania* zawiera elementy możliwe do wdrożenia aplikacji wytwarzane przez polecenie [dotnet Publish](/dotnet/core/tools/dotnet-publish) .</span><span class="sxs-lookup"><span data-stu-id="4eca8-105">The *publish* directory contains the app's deployable assets produced by the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="4eca8-106">Katalog zawiera:</span><span class="sxs-lookup"><span data-stu-id="4eca8-106">The directory contains:</span></span>

* <span data-ttu-id="4eca8-107">Pliki aplikacji</span><span class="sxs-lookup"><span data-stu-id="4eca8-107">Application files</span></span>
* <span data-ttu-id="4eca8-108">Pliki konfiguracji</span><span class="sxs-lookup"><span data-stu-id="4eca8-108">Configuration files</span></span>
* <span data-ttu-id="4eca8-109">Statyczne zasoby</span><span class="sxs-lookup"><span data-stu-id="4eca8-109">Static assets</span></span>
* <span data-ttu-id="4eca8-110">Pakiety</span><span class="sxs-lookup"><span data-stu-id="4eca8-110">Packages</span></span>
* <span data-ttu-id="4eca8-111">Środowisko uruchomieniowe (tylko do[wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) )</span><span class="sxs-lookup"><span data-stu-id="4eca8-111">A runtime ([self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) only)</span></span>

| <span data-ttu-id="4eca8-112">Typ aplikacji</span><span class="sxs-lookup"><span data-stu-id="4eca8-112">App Type</span></span> | <span data-ttu-id="4eca8-113">Struktura katalogów</span><span class="sxs-lookup"><span data-stu-id="4eca8-113">Directory Structure</span></span> |
| -------- | ------------------- |
| [<span data-ttu-id="4eca8-114">Plik wykonywalny zależny od platformy (całego)</span><span class="sxs-lookup"><span data-stu-id="4eca8-114">Framework-dependent Executable (FDE)</span></span>](/dotnet/core/deploying/#framework-dependent-executables-fde) | <ul><li><span data-ttu-id="4eca8-115">Publikuj&dagger;</span><span class="sxs-lookup"><span data-stu-id="4eca8-115">publish&dagger;</span></span><ul><li><span data-ttu-id="4eca8-116">Widoki&dagger; aplikacji MVC; Jeśli widoki nie są wstępnie skompilowane</span><span class="sxs-lookup"><span data-stu-id="4eca8-116">Views&dagger; MVC apps; if views aren't precompiled</span></span></li><li><span data-ttu-id="4eca8-117">Strony&dagger; aplikacji MVC lub Razor Pages, jeśli strony nie są wstępnie skompilowane</span><span class="sxs-lookup"><span data-stu-id="4eca8-117">Pages&dagger; MVC or Razor Pages apps, if pages aren't precompiled</span></span></li><li><span data-ttu-id="4eca8-118">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="4eca8-118">wwwroot&dagger;</span></span></li><li><span data-ttu-id="4eca8-119">*pliki. dll</li><li>{nazwa zestawu}. deps. json</li><li>{nazwa zestawu}. dll</li><li>{nazwa zestawu} {. Rozszerzenie} *. exe* w systemie Windows, brak rozszerzenia w systemie MacOS lub Linux</li><li>{nazwa zestawu}. pdb</li><li>{nazwa zestawu}. Widoki. dll</li><li>{nazwa zestawu}. Views. pdb</li><li>{nazwa zestawu}. runtimeconfig. JSON</li><li>plik Web. config (wdrożenia usług IIS)</li><li>zrzutu (narzędzie do zarządzania[zrzutem](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/xplat-minidump-generation.md#configurationpolicy)* systemu Linux)</li><li>.</span><span class="sxs-lookup"><span data-stu-id="4eca8-119">*.dll files</li><li>{ASSEMBLY NAME}.deps.json</li><li>{ASSEMBLY NAME}.dll</li><li>{ASSEMBLY NAME}{.EXTENSION} *.exe* extension on Windows, no extension on macOS or Linux</li><li>{ASSEMBLY NAME}.pdb</li><li>{ASSEMBLY NAME}.Views.dll</li><li>{ASSEMBLY NAME}.Views.pdb</li><li>{ASSEMBLY NAME}.runtimeconfig.json</li><li>web.config (IIS deployments)</li><li>createdump ([Linux createdump utility](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/xplat-minidump-generation.md#configurationpolicy))</li><li>*.so (Linux shared object library)</span></span></li><li><span data-ttu-id="4eca8-120">*. a (archiwum macOS)</li><li>* . DYLIB (macOS Dynamic Library)</span><span class="sxs-lookup"><span data-stu-id="4eca8-120">*.a (macOS archive)</li><li>*.dylib (macOS dynamic library)</span></span></li></ul></li></ul> |
| [<span data-ttu-id="4eca8-121">Wdrażanie samodzielne (SCD)</span><span class="sxs-lookup"><span data-stu-id="4eca8-121">Self-contained Deployment (SCD)</span></span>](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li><span data-ttu-id="4eca8-122">Publikuj&dagger;</span><span class="sxs-lookup"><span data-stu-id="4eca8-122">publish&dagger;</span></span><ul><li><span data-ttu-id="4eca8-123">Widoki&dagger; aplikacji MVC, jeśli widoki nie są wstępnie skompilowane</span><span class="sxs-lookup"><span data-stu-id="4eca8-123">Views&dagger; MVC apps, if views aren't precompiled</span></span></li><li><span data-ttu-id="4eca8-124">Strony&dagger; aplikacji MVC lub Razor Pages, jeśli strony nie są wstępnie skompilowane</span><span class="sxs-lookup"><span data-stu-id="4eca8-124">Pages&dagger; MVC or Razor Pages apps, if pages aren't precompiled</span></span></li><li><span data-ttu-id="4eca8-125">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="4eca8-125">wwwroot&dagger;</span></span></li><li><span data-ttu-id="4eca8-126">\*. dll — pliki</span><span class="sxs-lookup"><span data-stu-id="4eca8-126">\*.dll files</span></span></li><li><span data-ttu-id="4eca8-127">{Nazwa zestawu}. deps. JSON</span><span class="sxs-lookup"><span data-stu-id="4eca8-127">{ASSEMBLY NAME}.deps.json</span></span></li><li><span data-ttu-id="4eca8-128">{Nazwa zestawu}. dll</span><span class="sxs-lookup"><span data-stu-id="4eca8-128">{ASSEMBLY NAME}.dll</span></span></li><li><span data-ttu-id="4eca8-129">{Nazwa zestawu}. exe</span><span class="sxs-lookup"><span data-stu-id="4eca8-129">{ASSEMBLY NAME}.exe</span></span></li><li><span data-ttu-id="4eca8-130">{Nazwa zestawu}. pdb</span><span class="sxs-lookup"><span data-stu-id="4eca8-130">{ASSEMBLY NAME}.pdb</span></span></li><li><span data-ttu-id="4eca8-131">{NAZWA ZESTAWU}. Widoki. dll</span><span class="sxs-lookup"><span data-stu-id="4eca8-131">{ASSEMBLY NAME}.Views.dll</span></span></li><li><span data-ttu-id="4eca8-132">{NAZWA ZESTAWU}. Widoki. pdb</span><span class="sxs-lookup"><span data-stu-id="4eca8-132">{ASSEMBLY NAME}.Views.pdb</span></span></li><li><span data-ttu-id="4eca8-133">{Nazwa zestawu}. runtimeconfig. JSON</span><span class="sxs-lookup"><span data-stu-id="4eca8-133">{ASSEMBLY NAME}.runtimeconfig.json</span></span></li><li><span data-ttu-id="4eca8-134">Web. config (wdrożenia usług IIS)</span><span class="sxs-lookup"><span data-stu-id="4eca8-134">web.config (IIS deployments)</span></span></li></ul></li></ul> |

<span data-ttu-id="4eca8-135">&dagger;wskazuje katalog</span><span class="sxs-lookup"><span data-stu-id="4eca8-135">&dagger;Indicates a directory</span></span>

<span data-ttu-id="4eca8-136">Katalog *publikowania* reprezentuje *ścieżkę katalogu głównego zawartości*, nazywaną również *ścieżką bazową aplikacji*, wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="4eca8-136">The *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="4eca8-137">Dowolnych nazw do katalogu *publikacji* wdrożonej aplikacji na serwerze, jej lokalizacja służy jako ścieżka fizyczna serwera do hostowanej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4eca8-137">Whatever name is given to the *publish* directory of the deployed app on the server, its location serves as the server's physical path to the hosted app.</span></span>

<span data-ttu-id="4eca8-138">Katalog *wwwroot* (jeśli istnieje) zawiera tylko zasoby statyczne.</span><span class="sxs-lookup"><span data-stu-id="4eca8-138">The *wwwroot* directory, if present, only contains static assets.</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4eca8-139">Tworzenie folderu *dzienników* jest przydatne w przypadku [zaawansowanego rejestrowania debugowania modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="4eca8-139">Creating a *Logs* folder is useful for [ASP.NET Core Module enhanced debug logging](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="4eca8-140">Foldery w ścieżce przekazane do wartości `<handlerSetting>` nie są automatycznie tworzone przez moduł i powinny być wcześniej dostępne we wdrożeniu, aby umożliwić modułowi zapisanie dziennika debugowania.</span><span class="sxs-lookup"><span data-stu-id="4eca8-140">Folders in the path provided to the `<handlerSetting>` value aren't created by the module automatically and should pre-exist in the deployment to allow the module to write the debug log.</span></span>

<span data-ttu-id="4eca8-141">Katalog *dzienników* można utworzyć dla wdrożenia przy użyciu jednego z następujących dwóch metod:</span><span class="sxs-lookup"><span data-stu-id="4eca8-141">A *Logs* directory can be created for the deployment using one of the following two approaches:</span></span>

* <span data-ttu-id="4eca8-142">Dodaj następujący `<Target>` elementu do pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="4eca8-142">Add the following `<Target>` element to the project file:</span></span>

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

   <span data-ttu-id="4eca8-143">Element `<MakeDir>` tworzy pusty folder *Logs* w opublikowanym danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="4eca8-143">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="4eca8-144">Element używa właściwości `PublishDir`, aby określić lokalizację docelową tworzenia folderu.</span><span class="sxs-lookup"><span data-stu-id="4eca8-144">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="4eca8-145">Kilka metod wdrażania, takich jak Web Deploy, Pomijaj puste foldery podczas wdrażania.</span><span class="sxs-lookup"><span data-stu-id="4eca8-145">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="4eca8-146">Element `<WriteLinesToFile>` generuje plik w folderze *Logs* , który gwarantuje wdrożenie folderu na serwerze.</span><span class="sxs-lookup"><span data-stu-id="4eca8-146">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="4eca8-147">Tworzenie folderów przy użyciu tego podejścia kończy się niepowodzeniem, jeśli proces roboczy nie ma dostępu do zapisu do folderu docelowego.</span><span class="sxs-lookup"><span data-stu-id="4eca8-147">Folder creation using this approach fails if the worker process doesn't have write access to the target folder.</span></span>

* <span data-ttu-id="4eca8-148">Fizycznie Utwórz katalog *dzienników* na serwerze we wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="4eca8-148">Physically create the *Logs* directory on the server in the deployment.</span></span>

<span data-ttu-id="4eca8-149">Katalog wdrożenia wymaga uprawnień do odczytu/wykonania.</span><span class="sxs-lookup"><span data-stu-id="4eca8-149">The deployment directory requires Read/Execute permissions.</span></span> <span data-ttu-id="4eca8-150">Katalog *dzienników* wymaga uprawnień do odczytu/zapisu.</span><span class="sxs-lookup"><span data-stu-id="4eca8-150">The *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="4eca8-151">Dodatkowe katalogi, w przypadku których zapisano pliki wymagają uprawnień do odczytu/zapisu.</span><span class="sxs-lookup"><span data-stu-id="4eca8-151">Additional directories where files are written require Read/Write permissions.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="4eca8-152">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4eca8-152">Additional resources</span></span>

* [<span data-ttu-id="4eca8-153">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="4eca8-153">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* [<span data-ttu-id="4eca8-154">Wdrażanie aplikacji .NET core</span><span class="sxs-lookup"><span data-stu-id="4eca8-154">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* [<span data-ttu-id="4eca8-155">Platformy docelowe</span><span class="sxs-lookup"><span data-stu-id="4eca8-155">Target frameworks</span></span>](/dotnet/standard/frameworks)
* [<span data-ttu-id="4eca8-156">Katalog programu .NET Core RID</span><span class="sxs-lookup"><span data-stu-id="4eca8-156">.NET Core RID Catalog</span></span>](/dotnet/core/rid-catalog)
