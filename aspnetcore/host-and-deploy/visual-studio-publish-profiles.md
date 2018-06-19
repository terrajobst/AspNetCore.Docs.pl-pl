---
title: Profilów dla wdrożenia aplikacji platformy ASP.NET Core publikowania programu Visual Studio
author: rick-anderson
description: Informacje o sposobie tworzenia profilów publikowania w programie Visual Studio i używać ich do zarządzania wdrożeniami aplikacji platformy ASP.NET Core do różnych obiektów docelowych.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483373"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="df89b-103">Profilów dla wdrożenia aplikacji platformy ASP.NET Core publikowania programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="df89b-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="df89b-104">Przez [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="df89b-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="df89b-105">Ten dokument koncentruje się na przy użyciu programu Visual Studio 2017 do tworzenia i używania profilów publikowania.</span><span class="sxs-lookup"><span data-stu-id="df89b-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="df89b-106">Profile utworzone za pomocą programu Visual Studio można uruchomić z MSBuild i Visual Studio 2017 r.</span><span class="sxs-lookup"><span data-stu-id="df89b-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="df89b-107">Zobacz [publikowania aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) instrukcje dotyczące publikowania na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="df89b-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="df89b-108">Następujący plik projektu został utworzony przy użyciu polecenia `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="df89b-108">The following project file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df89b-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df89b-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df89b-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df89b-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="df89b-111">`<Project>` Elementu `Sdk` atrybutu wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="df89b-111">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="df89b-112">Importuje plik właściwości z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* na początku.</span><span class="sxs-lookup"><span data-stu-id="df89b-112">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="df89b-113">Importuje plik elementów docelowych z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* na końcu.</span><span class="sxs-lookup"><span data-stu-id="df89b-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="df89b-114">Domyślna lokalizacja dla `MSBuildSDKsPath` (za pomocą programu Visual Studio Enterprise 2017) jest *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folderu.</span><span class="sxs-lookup"><span data-stu-id="df89b-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="df89b-115">`Microsoft.NET.Sdk.Web` Zależy od zestawu SDK:</span><span class="sxs-lookup"><span data-stu-id="df89b-115">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="df89b-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="df89b-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="df89b-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="df89b-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="df89b-118">Co spowoduje, że poniższe właściwości i obiekty docelowe do zaimportowania:</span><span class="sxs-lookup"><span data-stu-id="df89b-118">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="df89b-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="df89b-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="df89b-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="df89b-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="df89b-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="df89b-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="df89b-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="df89b-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="df89b-123">Opublikuj prawo zestawu elementów docelowych, na podstawie metody publikowania używane importu obiektów docelowych.</span><span class="sxs-lookup"><span data-stu-id="df89b-123">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="df89b-124">Podczas ładowania projektu programu MSBuild lub Visual Studio są wykonywane następujące akcje wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="df89b-124">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="df89b-125">Tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="df89b-125">Build project</span></span>
* <span data-ttu-id="df89b-126">Obliczenia bazy danych plików do opublikowania</span><span class="sxs-lookup"><span data-stu-id="df89b-126">Compute files to publish</span></span>
* <span data-ttu-id="df89b-127">Publikowanie plików do lokalizacji docelowej</span><span class="sxs-lookup"><span data-stu-id="df89b-127">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="df89b-128">Obliczeniowe elementy projektu</span><span class="sxs-lookup"><span data-stu-id="df89b-128">Compute project items</span></span>

<span data-ttu-id="df89b-129">Po załadowaniu projektu są obliczane elementy projektu (pliki).</span><span class="sxs-lookup"><span data-stu-id="df89b-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="df89b-130">`item type` Atrybut określa sposób przetwarzania pliku.</span><span class="sxs-lookup"><span data-stu-id="df89b-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="df89b-131">Domyślnie *.cs* pliki znajdują się w `Compile` listy elementów.</span><span class="sxs-lookup"><span data-stu-id="df89b-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="df89b-132">Pliki w `Compile` są kompilowane listy elementów.</span><span class="sxs-lookup"><span data-stu-id="df89b-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="df89b-133">`Content` Listy elementów zawiera pliki, które są publikowane oprócz plików wyjściowych kompilacji.</span><span class="sxs-lookup"><span data-stu-id="df89b-133">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="df89b-134">Domyślnie pliki zgodne ze wzorcem `wwwroot/**` znajdują się w `Content` elementu.</span><span class="sxs-lookup"><span data-stu-id="df89b-134">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="df89b-135">`wwwroot/\*\*` [Wzorzec globbing](https://gruntjs.com/configuring-tasks#globbing-patterns) dopasowanie wszystkich plików w *wwwroot* folderu **i** podfoldery.</span><span class="sxs-lookup"><span data-stu-id="df89b-135">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="df89b-136">Aby jawnie dodać plik do listy publikowania, należy dodać bezpośrednio w pliku *.csproj* plików, jak pokazano w [pliki dołączane](#include-files).</span><span class="sxs-lookup"><span data-stu-id="df89b-136">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="df89b-137">Podczas wybierania **publikowania** przycisk w programie Visual Studio lub podczas publikowania z wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="df89b-137">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="df89b-138">Właściwości/elementy są obliczane (pliki, które są niezbędne do utworzenia).</span><span class="sxs-lookup"><span data-stu-id="df89b-138">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="df89b-139">**Visual Studio tylko**: pakiety NuGet są przywracane.</span><span class="sxs-lookup"><span data-stu-id="df89b-139">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="df89b-140">(Przywróć musi mieć jawne przez użytkownika za pomocą interfejsu wiersza polecenia).</span><span class="sxs-lookup"><span data-stu-id="df89b-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="df89b-141">Tworzy projekt.</span><span class="sxs-lookup"><span data-stu-id="df89b-141">The project builds.</span></span>
* <span data-ttu-id="df89b-142">Publikowanie elementów są obliczane (pliki, które są niezbędne do publikowania).</span><span class="sxs-lookup"><span data-stu-id="df89b-142">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="df89b-143">Projekt nie zostanie opublikowany (obliczona pliki są kopiowane do lokalizacji docelowej publikowania).</span><span class="sxs-lookup"><span data-stu-id="df89b-143">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="df89b-144">Gdy odwołuje się projekt platformy ASP.NET Core `Microsoft.NET.Sdk.Web` w pliku projektu *app_offline.htm* plik znajduje się w głównym katalogu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="df89b-144">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="df89b-145">Gdy plik jest obecny, moduł platformy ASP.NET Core bezpiecznie zamyka aplikację i służy *app_offline.htm* plików podczas wdrażania.</span><span class="sxs-lookup"><span data-stu-id="df89b-145">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="df89b-146">Aby uzyskać więcej informacji, zobacz [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="df89b-146">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="df89b-147">Podstawowe publikowanie wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="df89b-147">Basic command-line publishing</span></span>

<span data-ttu-id="df89b-148">Publikowanie z wiersza polecenia działa na wszystkich platformach obsługiwanych przez oprogramowanie .NET Core i nie wymaga programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="df89b-148">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="df89b-149">W poniższych przykładach [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie jest uruchamiane od katalogu projektu (która zawiera *.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="df89b-149">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="df89b-150">Jeśli nie jest w folderze projektu jawnie Podaj ścieżkę pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="df89b-150">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="df89b-151">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="df89b-151">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="df89b-152">Uruchom następujące polecenia, aby utworzyć i opublikować aplikację sieci web:</span><span class="sxs-lookup"><span data-stu-id="df89b-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df89b-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df89b-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df89b-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df89b-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="df89b-155">[Publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie generuje dane wyjściowe podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="df89b-155">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="df89b-156">Wartość domyślna publikowanie folderu jest `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="df89b-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="df89b-157">Wartość domyślna dla `$(Configuration)` jest *debugowania*.</span><span class="sxs-lookup"><span data-stu-id="df89b-157">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="df89b-158">W poprzednim przykładzie `<TargetFramework>` jest `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="df89b-158">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="df89b-159">`dotnet publish -h` Wyświetla Pomoc dla publikacji.</span><span class="sxs-lookup"><span data-stu-id="df89b-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="df89b-160">Określa polecenie `Release` kompilacji i publikowania katalogu:</span><span class="sxs-lookup"><span data-stu-id="df89b-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="df89b-161">[Publikowania dotnet](/dotnet/core/tools/dotnet-publish) wywołania MSBuild, który wywołuje polecenia `Publish` docelowej.</span><span class="sxs-lookup"><span data-stu-id="df89b-161">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="df89b-162">Parametry przekazane do `dotnet publish` są przekazywane do programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="df89b-162">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="df89b-163">`-c` Mapuje parametru `Configuration` właściwości programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="df89b-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="df89b-164">`-o` Mapuje parametru `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="df89b-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="df89b-165">Właściwości programu MSBuild mogą być przekazywane przy użyciu jednej z następujących formatów:</span><span class="sxs-lookup"><span data-stu-id="df89b-165">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="df89b-166">Polecenie publikuje `Release` kompilacji do udziału sieciowego:</span><span class="sxs-lookup"><span data-stu-id="df89b-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="df89b-167">Udział sieciowy zostanie określony z kreskami ukośnymi (*//r8/*) i działa na wszystkich platformach .NET Core, obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="df89b-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="df89b-168">Upewnij się, że opublikowana aplikacja dla wdrożenia nie jest uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="df89b-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="df89b-169">Pliki w *publikowania* folderu są zablokowane, gdy aplikacja jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="df89b-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="df89b-170">Wdrożenie nie może wystąpić, ponieważ zablokowany się, że nie można skopiować plików.</span><span class="sxs-lookup"><span data-stu-id="df89b-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="df89b-171">Profilów publikowania</span><span class="sxs-lookup"><span data-stu-id="df89b-171">Publish profiles</span></span>

<span data-ttu-id="df89b-172">Ta sekcja używa programu Visual Studio 2017 można utworzyć profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="df89b-172">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="df89b-173">Po utworzeniu publikacji z wiersza polecenia lub programu Visual Studio jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="df89b-173">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="df89b-174">Publikowanie profili może uprościć proces publikowania i dowolną liczbę profilów może istnieć.</span><span class="sxs-lookup"><span data-stu-id="df89b-174">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="df89b-175">Tworzenie profilu publikowania w programie Visual Studio, wybierając jedną z następujących ścieżek:</span><span class="sxs-lookup"><span data-stu-id="df89b-175">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="df89b-176">Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="df89b-176">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="df89b-177">Wybierz **publikowania &lt;project_name&gt;**  z **kompilacji** menu.</span><span class="sxs-lookup"><span data-stu-id="df89b-177">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="df89b-178">**Publikowania** jest wyświetlany na karcie Strona możliwości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="df89b-178">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="df89b-179">Jeśli projekt nie ma profilu publikowania, zostanie wyświetlona strona następujące:</span><span class="sxs-lookup"><span data-stu-id="df89b-179">If the project lacks a publish profile, the following page is displayed:</span></span>

![Karta publikowania strony możliwości aplikacji](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="df89b-181">Gdy **folderu** jest zaznaczone, określ ścieżkę folderu do przechowywania opublikowanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="df89b-181">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="df89b-182">Domyślnym folderem jest *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="df89b-182">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="df89b-183">Kliknij przycisk **Utwórz profil** przycisk, aby zakończyć.</span><span class="sxs-lookup"><span data-stu-id="df89b-183">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="df89b-184">Po utworzeniu profilu publikowania **publikowania** karcie zmiany.</span><span class="sxs-lookup"><span data-stu-id="df89b-184">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="df89b-185">Nowo utworzony profil zostanie wyświetlony na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="df89b-185">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="df89b-186">Kliknij przycisk **Utwórz nowy profil** do utworzenia nowego profilu innego.</span><span class="sxs-lookup"><span data-stu-id="df89b-186">Click **Create new profile** to create another new profile.</span></span>

![Karta publikowania przedstawiający FolderProfile strony możliwości aplikacji](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="df89b-188">Kreator publikowania obsługuje następujące elementy docelowe publikowania:</span><span class="sxs-lookup"><span data-stu-id="df89b-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="df89b-189">Usługa aplikacji Azure</span><span class="sxs-lookup"><span data-stu-id="df89b-189">Azure App Service</span></span>
* <span data-ttu-id="df89b-190">Maszyny wirtualne platformy Azure</span><span class="sxs-lookup"><span data-stu-id="df89b-190">Azure Virtual Machines</span></span>
* <span data-ttu-id="df89b-191">Usługi IIS, FTP, itp. (na każdym serwerze sieci web)</span><span class="sxs-lookup"><span data-stu-id="df89b-191">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="df89b-192">Folder</span><span class="sxs-lookup"><span data-stu-id="df89b-192">Folder</span></span>
* <span data-ttu-id="df89b-193">Importowanie profilu</span><span class="sxs-lookup"><span data-stu-id="df89b-193">Import Profile</span></span>

<span data-ttu-id="df89b-194">Aby uzyskać więcej informacji, zobacz [jakie opcje publikowania jest dla mnie odpowiednia](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="df89b-194">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="df89b-195">Podczas tworzenia profilu publikowania przy użyciu programu Visual Studio, *właściwości/PublishProfiles/&lt;nazwa_profilu&gt;.pubxml* jest tworzony plik MSBuild.</span><span class="sxs-lookup"><span data-stu-id="df89b-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="df89b-196">*.Pubxml* plik jest plikiem MSBuild i zawiera ustawienia konfiguracji publikowania.</span><span class="sxs-lookup"><span data-stu-id="df89b-196">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="df89b-197">Można zmienić tego pliku dostosowania kompilacji i opublikować procesu.</span><span class="sxs-lookup"><span data-stu-id="df89b-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="df89b-198">Ten plik jest odczytywany przez proces publikowania.</span><span class="sxs-lookup"><span data-stu-id="df89b-198">This file is read by the publishing process.</span></span> <span data-ttu-id="df89b-199">`<LastUsedBuildConfiguration>` jest specjalne, ponieważ jest właściwością globalną i nie powinny należeć do każdego pliku, który jest importowany w kompilacji.</span><span class="sxs-lookup"><span data-stu-id="df89b-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="df89b-200">Zobacz [MSBuild: sposób ustawiania właściwości konfiguracji](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="df89b-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="df89b-201">Podczas publikowania do docelowej platformy Azure, *.pubxml* plik zawiera identyfikatora subskrypcji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="df89b-201">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="df89b-202">Z tym typem docelowym dodając ten plik do kontroli źródła nie jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="df89b-202">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="df89b-203">Podczas publikowania z obiektem docelowym z systemem innym niż Azure, jest bezpieczne zaewidencjonować *.pubxml* pliku.</span><span class="sxs-lookup"><span data-stu-id="df89b-203">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="df89b-204">Informacje poufne (na przykład hasła publikowania) są szyfrowane na na poziomie użytkownika/komputera.</span><span class="sxs-lookup"><span data-stu-id="df89b-204">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="df89b-205">Jest on przechowywany w *właściwości/PublishProfiles/&lt;nazwa_profilu&gt;. pubxml.user* pliku.</span><span class="sxs-lookup"><span data-stu-id="df89b-205">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="df89b-206">Ponieważ ten plik można przechowywać poufnych informacji, nie powinien być wyewidencjonowany do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="df89b-206">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="df89b-207">Omówienie sposobu publikowania aplikacji sieci web platformy ASP.NET Core, zobacz [hosta i wdrażanie](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="df89b-207">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="df89b-208">Zadania programu MSBuild i obiekty docelowe niezbędne do publikowania aplikacji platformy ASP.NET Core są open source w https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="df89b-208">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="df89b-209">`dotnet publish` można użyć folderu, MSDeploy, i [Kudu](https://github.com/projectkudu/kudu/wiki) profilów publikowania:</span><span class="sxs-lookup"><span data-stu-id="df89b-209">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="df89b-210">Folder (działa i platform):</span><span class="sxs-lookup"><span data-stu-id="df89b-210">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="df89b-211">MSDeploy (obecnie ta działa tylko w systemie Windows, ponieważ wielu platform nie jest MSDeploy):</span><span class="sxs-lookup"><span data-stu-id="df89b-211">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="df89b-212">Pakiet MSDeploy (obecnie ta działa tylko w systemie Windows, ponieważ wielu platform nie jest MSDeploy):</span><span class="sxs-lookup"><span data-stu-id="df89b-212">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="df89b-213">W poprzednich przykładach **nie** przekazać `deployonbuild` do `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="df89b-213">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="df89b-214">Aby uzyskać więcej informacji, zobacz [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="df89b-214">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="df89b-215">`dotnet publish` obsługuje interfejsy API Kudu do publikowania na platformie Azure z dowolną platformą.</span><span class="sxs-lookup"><span data-stu-id="df89b-215">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="df89b-216">Visual Studio publikuje obsługuje interfejsy API Kudu, ale jest obsługiwany przez WebSDK dla wielu platform publikowanie na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="df89b-216">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="df89b-217">Dodawanie profilu publikowania do *właściwości/PublishProfiles* folderu o następującej treści:</span><span class="sxs-lookup"><span data-stu-id="df89b-217">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="df89b-218">Uruchom następujące polecenie do pliku zip zapasową publikowania i opublikujesz je na platformie Azure przy użyciu interfejsów API Kudu:</span><span class="sxs-lookup"><span data-stu-id="df89b-218">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="df89b-219">Ustaw następujące właściwości programu MSBuild, korzystając z odpowiednim profilem publikowania:</span><span class="sxs-lookup"><span data-stu-id="df89b-219">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="df89b-220">Podczas publikowania z tym profilem o nazwie *FolderProfile*, aby można było wykonać dowolne z poniższych poleceń:</span><span class="sxs-lookup"><span data-stu-id="df89b-220">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="df89b-221">Podczas wywoływania [kompilacji dotnet](/dotnet/core/tools/dotnet-build), wywołuje `msbuild` do uruchomienia kompilacji, a następnie opublikuj przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="df89b-221">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="df89b-222">Wywołanie każdej `dotnet build` lub `msbuild` odpowiada podczas przekazywania w folderze profilu.</span><span class="sxs-lookup"><span data-stu-id="df89b-222">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="df89b-223">Podczas wywoływania MSBuild bezpośrednio w systemie Windows, programu .NET Framework w wersji programu MSBuild jest używany.</span><span class="sxs-lookup"><span data-stu-id="df89b-223">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="df89b-224">MSDeploy jest obecnie ograniczona do komputerów z systemem Windows do publikowania.</span><span class="sxs-lookup"><span data-stu-id="df89b-224">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="df89b-225">Wywoływanie `dotnet build` z systemem innym niż folderu Program MSBuild wywołuje profilu, a MSBuild używa MSDeploy na profilach nie folder.</span><span class="sxs-lookup"><span data-stu-id="df89b-225">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="df89b-226">Wywoływanie `dotnet build` profilu folderu nie wywołuje MSBuild (przy użyciu MSDeploy) i powoduje błąd (nawet wtedy, gdy uruchomiona na platformie systemu Windows).</span><span class="sxs-lookup"><span data-stu-id="df89b-226">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="df89b-227">Aby opublikować za pomocą profilu — do folderu, bezpośrednio wywołać program MSBuild.</span><span class="sxs-lookup"><span data-stu-id="df89b-227">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="df89b-228">Profil publikowania następujący folder został utworzony za pomocą programu Visual Studio i publikuje do udziału sieciowego:</span><span class="sxs-lookup"><span data-stu-id="df89b-228">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="df89b-229">Uwaga `<LastUsedBuildConfiguration>` ma ustawioną wartość `Release`.</span><span class="sxs-lookup"><span data-stu-id="df89b-229">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="df89b-230">Podczas publikowania z programu Visual Studio `<LastUsedBuildConfiguration>` wartości właściwości konfiguracji jest ustawiona przy użyciu wartości, gdy proces publikowania zostanie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="df89b-230">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="df89b-231">`<LastUsedBuildConfiguration>` Właściwości konfiguracji jest szczególna i nie powinna zostać zastąpiona w zaimportowanym pliku programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="df89b-231">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="df89b-232">Ta właściwość może być zastąpiona w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="df89b-232">This property can be overridden from the command line.</span></span>

<span data-ttu-id="df89b-233">Przy użyciu platformy .NET Core interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="df89b-233">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="df89b-234">Przy użyciu programu MSBuild:</span><span class="sxs-lookup"><span data-stu-id="df89b-234">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="df89b-235">Publikowanie do punktu końcowego MSDeploy z wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="df89b-235">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="df89b-236">Publikowanie można wykonywać przy użyciu platformy .NET Core interfejsu wiersza polecenia lub programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="df89b-236">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="df89b-237">`dotnet publish` jest uruchamiany w kontekście .NET Core.</span><span class="sxs-lookup"><span data-stu-id="df89b-237">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="df89b-238">`msbuild` Polecenia wymaga programu .NET Framework, która ogranicza ją do środowiska systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="df89b-238">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="df89b-239">Najprostszym sposobem publikowania z MSDeploy jest najpierw utworzyć profil publikowania w programie Visual Studio 2017 i Użyj profilu z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="df89b-239">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="df89b-240">W poniższym przykładzie jest tworzona aplikacja sieci web platformy ASP.NET Core (przy użyciu `dotnet new mvc`), a profil publikowania platformy Azure są dodawane z programem Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="df89b-240">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="df89b-241">Uruchom `msbuild` z **wiersz polecenia dla programu VS 2017 deweloperów**.</span><span class="sxs-lookup"><span data-stu-id="df89b-241">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="df89b-242">Wiersz polecenia dewelopera jest prawidłowy *msbuild.exe* w swojej ścieżce niektórych zestaw zmiennych MSBuild.</span><span class="sxs-lookup"><span data-stu-id="df89b-242">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="df89b-243">Program MSBuild używa następującej składni:</span><span class="sxs-lookup"><span data-stu-id="df89b-243">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="df89b-244">Pobierz `Password` z  *\<publikowania name >. PublishSettings* pliku.</span><span class="sxs-lookup"><span data-stu-id="df89b-244">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="df89b-245">Pobierz *. PublishSettings* plik z dowolnej:</span><span class="sxs-lookup"><span data-stu-id="df89b-245">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="df89b-246">Eksplorator rozwiązań: Kliknij prawym przyciskiem myszy w aplikacji sieci Web i wybierz **pobrać profilu publikowania**.</span><span class="sxs-lookup"><span data-stu-id="df89b-246">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="df89b-247">Portalu Azure: kliknij **profilu publikowania Get** w aplikacji sieci Web **omówienie** panelu.</span><span class="sxs-lookup"><span data-stu-id="df89b-247">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="df89b-248">`Username` znajdują się w profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="df89b-248">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="df89b-249">Następujące przykładowe używa *Web11112 - Web Deploy* profilu publikowania:</span><span class="sxs-lookup"><span data-stu-id="df89b-249">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="df89b-250">Wyklucz pliki</span><span class="sxs-lookup"><span data-stu-id="df89b-250">Exclude files</span></span>

<span data-ttu-id="df89b-251">Podczas publikowania aplikacji sieci web platformy ASP.NET Core, artefaktów kompilacji i zawartość *wwwroot* folderu są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="df89b-251">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="df89b-252">`msbuild` obsługuje [wzorce globbing](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="df89b-252">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="df89b-253">Na przykład następująca `<Content>` element nie obejmuje cały tekst (*.txt*) plików ze *zawartością/wwwroot* folderze i jego podfolderach.</span><span class="sxs-lookup"><span data-stu-id="df89b-253">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="df89b-254">Poprzedni kod znaczników można dodać do profilu publikowania lub *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="df89b-254">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="df89b-255">Po dodaniu do *.csproj* plik, reguła zostanie dodany do wszystkich profilów publikowania w projekcie.</span><span class="sxs-lookup"><span data-stu-id="df89b-255">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="df89b-256">Następujące `<MsDeploySkipRules>` element wyklucza wszystkie pliki z *zawartością/wwwroot* folderu:</span><span class="sxs-lookup"><span data-stu-id="df89b-256">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="df89b-257">`<MsDeploySkipRules>` nie usuwa *pominąć* elementów docelowych z witryny wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="df89b-257">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="df89b-258">`<Content>` pliki docelowe i foldery są usuwane z lokacji wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="df89b-258">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="df89b-259">Na przykład załóżmy, że wdrożonej aplikacji sieci web ma następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="df89b-259">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="df89b-260">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="df89b-260">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="df89b-261">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="df89b-261">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="df89b-262">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="df89b-262">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="df89b-263">Jeśli następujące `<MsDeploySkipRules>` są dodawane elementy, nie można usunąć te pliki w witrynie wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="df89b-263">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="df89b-264">Poprzedni `<MsDeploySkipRules>` zapobiec elementy *pominięte* pliki z wdrażany.</span><span class="sxs-lookup"><span data-stu-id="df89b-264">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="df89b-265">Nie będzie go usunąć te pliki, gdy są one wdrażane.</span><span class="sxs-lookup"><span data-stu-id="df89b-265">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="df89b-266">Następujące `<Content>` element usuwa pliki docelowe w witrynie wdrażania:</span><span class="sxs-lookup"><span data-stu-id="df89b-266">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="df89b-267">Za pomocą wiersza polecenia z poprzednim `<Content>` element daje następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="df89b-267">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a><span data-ttu-id="df89b-268">Pliki dołączane</span><span class="sxs-lookup"><span data-stu-id="df89b-268">Include files</span></span>

<span data-ttu-id="df89b-269">Obejmuje następujące znaczników *obrazów* folderów znajdujących się poza katalogiem projektu do *wwwroot/obrazów* folderu publikowania witryny:</span><span class="sxs-lookup"><span data-stu-id="df89b-269">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="df89b-270">Znaczników można dodać do *.csproj* pliku lub profil publikowania.</span><span class="sxs-lookup"><span data-stu-id="df89b-270">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="df89b-271">Jeśli jest ona dodawana do *.csproj* pliku umieścił je w każdym profilu publikowania do projektu.</span><span class="sxs-lookup"><span data-stu-id="df89b-271">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="df89b-272">Następujące wyróżnione znaczników pokazuje sposób do:</span><span class="sxs-lookup"><span data-stu-id="df89b-272">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="df89b-273">Skopiuj plik z spoza projektu do *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="df89b-273">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="df89b-274">Wyklucz *wwwroot\Content* folderu.</span><span class="sxs-lookup"><span data-stu-id="df89b-274">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="df89b-275">Wyklucz *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="df89b-275">Exclude *Views\Home\About2.cshtml*.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

<span data-ttu-id="df89b-276">Zobacz [WebSDK Readme](https://github.com/aspnet/websdk) dla większej liczby próbek wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="df89b-276">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="df89b-277">Uruchom element docelowy przed lub po opublikowaniu</span><span class="sxs-lookup"><span data-stu-id="df89b-277">Run a target before or after publishing</span></span>

<span data-ttu-id="df89b-278">Wbudowane `BeforePublish` i `AfterPublish` celów wykonania docelowy przed lub po lokalizacja docelowa publikowania.</span><span class="sxs-lookup"><span data-stu-id="df89b-278">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="df89b-279">Dodaj następujące elementy do profilu publikowania do rejestrowania komunikatów konsoli przed i po opublikowaniu:</span><span class="sxs-lookup"><span data-stu-id="df89b-279">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="df89b-280">Publikowanie na serwerze za pomocą niezaufanego certyfikatu</span><span class="sxs-lookup"><span data-stu-id="df89b-280">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="df89b-281">Dodaj `<AllowUntrustedCertificate>` właściwość z wartością `True` do profilu publikowania:</span><span class="sxs-lookup"><span data-stu-id="df89b-281">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="df89b-282">Usługa Kudu</span><span class="sxs-lookup"><span data-stu-id="df89b-282">The Kudu service</span></span>

<span data-ttu-id="df89b-283">Aby wyświetlić pliki w usłudze Azure App Service wdrożenia aplikacji sieci web, należy użyć [usługi Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="df89b-283">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="df89b-284">Dołącz `scm` token na nazwę aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="df89b-284">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="df89b-285">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="df89b-285">For example:</span></span>

| <span data-ttu-id="df89b-286">Adres URL</span><span class="sxs-lookup"><span data-stu-id="df89b-286">URL</span></span>                                    | <span data-ttu-id="df89b-287">Wynik</span><span class="sxs-lookup"><span data-stu-id="df89b-287">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="df89b-288">Aplikacja sieci Web</span><span class="sxs-lookup"><span data-stu-id="df89b-288">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="df89b-289">Program kudu usługi</span><span class="sxs-lookup"><span data-stu-id="df89b-289">Kudu service</span></span> |

<span data-ttu-id="df89b-290">Wybierz [konsoli debugowania](https://github.com/projectkudu/kudu/wiki/Kudu-console) element menu, aby wyświetlać, edytować, usuwać lub dodawać plików.</span><span class="sxs-lookup"><span data-stu-id="df89b-290">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="df89b-291">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="df89b-291">Additional resources</span></span>

* <span data-ttu-id="df89b-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) upraszcza wdrażanie aplikacji sieci web i witryn sieci Web na serwerach usług IIS.</span><span class="sxs-lookup"><span data-stu-id="df89b-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="df89b-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Plik problemy i żądania funkcji dla wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="df89b-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
