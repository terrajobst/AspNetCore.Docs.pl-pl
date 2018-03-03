---
title: "Profilów dla wdrożenia aplikacji platformy ASP.NET Core publikowania programu Visual Studio"
author: rick-anderson
description: "Odnajdywanie sposobu tworzenia profilów dla aplikacji platformy ASP.NET Core publikowania w programie Visual Studio."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: d2c4ec317f235c6d042bd130dbf79f6cb5e2d47d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="1895b-103">Profilów dla wdrożenia aplikacji platformy ASP.NET Core publikowania programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1895b-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="1895b-104">Przez [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1895b-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1895b-105">Ten artykuł skupia się na tworzenie za pomocą programu Visual Studio 2017 profilów publikowania.</span><span class="sxs-lookup"><span data-stu-id="1895b-105">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="1895b-106">Profile utworzone za pomocą programu Visual Studio można uruchomić z MSBuild i Visual Studio 2017 r.</span><span class="sxs-lookup"><span data-stu-id="1895b-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="1895b-107">Artykuł zawiera szczegółowe informacje o procesie publikowania.</span><span class="sxs-lookup"><span data-stu-id="1895b-107">The article provides details of the publishing process.</span></span> <span data-ttu-id="1895b-108">Zobacz [publikowania aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) instrukcje dotyczące publikowania na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="1895b-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="1895b-109">Następujące *.csproj* plik został utworzony przy użyciu polecenia `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="1895b-109">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1895b-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1895b-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1895b-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1895b-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="1895b-112">`Sdk` Atrybutu w `<Project>` elementu powyżej znacznika (w pierwszym wierszu) wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="1895b-112">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="1895b-113">Importuje plik właściwości z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* na początku.</span><span class="sxs-lookup"><span data-stu-id="1895b-113">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="1895b-114">Importuje plik elementów docelowych z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* na końcu.</span><span class="sxs-lookup"><span data-stu-id="1895b-114">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="1895b-115">Domyślna lokalizacja dla `MSBuildSDKsPath` (za pomocą programu Visual Studio Enterprise 2017) jest *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folderu.</span><span class="sxs-lookup"><span data-stu-id="1895b-115">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="1895b-116">`Microsoft.NET.Sdk.Web` zależy od:</span><span class="sxs-lookup"><span data-stu-id="1895b-116">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="1895b-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="1895b-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="1895b-118">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="1895b-118">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="1895b-119">Co spowoduje, że poniższe właściwości i obiekty docelowe do zaimportowania:</span><span class="sxs-lookup"><span data-stu-id="1895b-119">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="1895b-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="1895b-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="1895b-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="1895b-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="1895b-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="1895b-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="1895b-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="1895b-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="1895b-124">Opublikuj prawo zestawu elementów docelowych, na podstawie metody publikowania używane importu obiektów docelowych.</span><span class="sxs-lookup"><span data-stu-id="1895b-124">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="1895b-125">Podczas ładowania projektu programu MSBuild lub Visual Studio są wykonywane następujące akcje wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="1895b-125">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="1895b-126">Tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="1895b-126">Build project</span></span>
* <span data-ttu-id="1895b-127">Obliczenia bazy danych plików do opublikowania</span><span class="sxs-lookup"><span data-stu-id="1895b-127">Compute files to publish</span></span>
* <span data-ttu-id="1895b-128">Publikowanie plików do lokalizacji docelowej</span><span class="sxs-lookup"><span data-stu-id="1895b-128">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="1895b-129">Obliczeniowe elementy projektu</span><span class="sxs-lookup"><span data-stu-id="1895b-129">Compute project items</span></span>

<span data-ttu-id="1895b-130">Po załadowaniu projektu są obliczane elementy projektu (pliki).</span><span class="sxs-lookup"><span data-stu-id="1895b-130">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="1895b-131">`item type` Atrybut określa sposób przetwarzania pliku.</span><span class="sxs-lookup"><span data-stu-id="1895b-131">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="1895b-132">Domyślnie *.cs* pliki znajdują się w `Compile` listy elementów.</span><span class="sxs-lookup"><span data-stu-id="1895b-132">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="1895b-133">Pliki w `Compile` są kompilowane listy elementów.</span><span class="sxs-lookup"><span data-stu-id="1895b-133">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="1895b-134">`Content` Listy elementów zawiera pliki, które są publikowane oprócz plików wyjściowych kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1895b-134">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="1895b-135">Domyślnie pliki zgodne ze wzorcem `wwwroot/**` znajdują się w `Content` elementu.</span><span class="sxs-lookup"><span data-stu-id="1895b-135">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="1895b-136">[Wwwroot /\* \* to wzorzec globbing](https://gruntjs.com/configuring-tasks#globbing-patterns) Określa, że wszystkie pliki w *wwwroot* folderu **i** podfoldery.</span><span class="sxs-lookup"><span data-stu-id="1895b-136">[wwwroot/\*\* is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="1895b-137">Aby jawnie dodać plik do listy publikowania, należy dodać bezpośrednio w pliku *.csproj* plików, jak pokazano w [dołączanie plików](#including-files).</span><span class="sxs-lookup"><span data-stu-id="1895b-137">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="1895b-138">Podczas wybierania **publikowania** przycisk w programie Visual Studio lub podczas publikowania z wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="1895b-138">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="1895b-139">Właściwości/elementy są obliczane (pliki, które są niezbędne do utworzenia).</span><span class="sxs-lookup"><span data-stu-id="1895b-139">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="1895b-140">Tylko dla programu Visual Studio: pakiety NuGet są przywracane.</span><span class="sxs-lookup"><span data-stu-id="1895b-140">Visual Studio only: NuGet packages are restored.</span></span> <span data-ttu-id="1895b-141">(Przywróć musi mieć jawne przez użytkownika za pomocą interfejsu wiersza polecenia).</span><span class="sxs-lookup"><span data-stu-id="1895b-141">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="1895b-142">Tworzy projekt.</span><span class="sxs-lookup"><span data-stu-id="1895b-142">The project builds.</span></span>
* <span data-ttu-id="1895b-143">Publikowanie elementów są obliczane (pliki, które są niezbędne do publikowania).</span><span class="sxs-lookup"><span data-stu-id="1895b-143">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="1895b-144">Projekt nie zostanie opublikowany.</span><span class="sxs-lookup"><span data-stu-id="1895b-144">The project is published.</span></span> <span data-ttu-id="1895b-145">(Obliczona pliki są kopiowane do lokalizacji docelowej publikowania).</span><span class="sxs-lookup"><span data-stu-id="1895b-145">(The computed files are copied to the publish destination.)</span></span>

<span data-ttu-id="1895b-146">Gdy odwołuje się projekt platformy ASP.NET Core `Microsoft.NET.Sdk.Web` w pliku projektu *app_offline.htm* plik znajduje się w głównym katalogu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="1895b-146">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="1895b-147">Gdy plik jest obecny, moduł platformy ASP.NET Core bezpiecznie zamyka aplikację i służy *app_offline.htm* plików podczas wdrażania.</span><span class="sxs-lookup"><span data-stu-id="1895b-147">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="1895b-148">Aby uzyskać więcej informacji, zobacz [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="1895b-148">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="1895b-149">Podstawowe publikowanie wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="1895b-149">Basic command-line publishing</span></span>

<span data-ttu-id="1895b-150">Publikowanie z wiersza polecenia działa na wszystkich platformach .NET Core, obsługiwane i nie wymaga programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1895b-150">Command-line publishing works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="1895b-151">W poniższych przykładach [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie jest uruchamiane od katalogu projektu (która zawiera *.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="1895b-151">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="1895b-152">Jeśli nie jest w folderze projektu jawnie Podaj ścieżkę pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="1895b-152">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="1895b-153">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1895b-153">For example:</span></span>

```console
dotnet publish c:/webs/web1
```

<span data-ttu-id="1895b-154">Uruchom następujące polecenia, aby utworzyć i opublikować aplikację sieci web:</span><span class="sxs-lookup"><span data-stu-id="1895b-154">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1895b-155">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1895b-155">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1895b-156">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1895b-156">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="1895b-157">[Publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie generuje dane wyjściowe podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="1895b-157">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="1895b-158">Wartość domyślna publikowanie folderu jest `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="1895b-158">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="1895b-159">Wartość domyślna dla `$(Configuration)` jest debugowania.</span><span class="sxs-lookup"><span data-stu-id="1895b-159">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="1895b-160">W powyższym przykładowym `<TargetFramework>` jest `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="1895b-160">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="1895b-161">`dotnet publish -h` Wyświetla Pomoc dla publikacji.</span><span class="sxs-lookup"><span data-stu-id="1895b-161">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="1895b-162">Określa polecenie `Release` kompilacji i publikowania katalogu:</span><span class="sxs-lookup"><span data-stu-id="1895b-162">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="1895b-163">[Publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenia wywołuje MSBuild, który wywołuje `Publish` docelowej.</span><span class="sxs-lookup"><span data-stu-id="1895b-163">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild which invokes the `Publish` target.</span></span> <span data-ttu-id="1895b-164">Parametry przekazane do `dotnet publish` są przekazywane do programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="1895b-164">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="1895b-165">`-c` Mapuje parametru `Configuration` właściwości programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="1895b-165">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="1895b-166">`-o` Mapuje parametru `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="1895b-166">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="1895b-167">Właściwości programu MSBuild mogą być przekazywane przy użyciu jednej z następujących formatów:</span><span class="sxs-lookup"><span data-stu-id="1895b-167">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="1895b-168">Polecenie publikuje `Release` kompilacji do udziału sieciowego:</span><span class="sxs-lookup"><span data-stu-id="1895b-168">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="1895b-169">Udział sieciowy zostanie określony z kreskami ukośnymi (*//r8/*) i działa na wszystkich platformach .NET Core, obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="1895b-169">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="1895b-170">Upewnij się, że opublikowana aplikacja dla wdrożenia nie jest uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="1895b-170">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="1895b-171">Pliki w *publikowania* folderu są zablokowane, gdy aplikacja jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="1895b-171">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="1895b-172">Wdrożenie nie może wystąpić, ponieważ zablokowany się, że nie można skopiować plików.</span><span class="sxs-lookup"><span data-stu-id="1895b-172">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="1895b-173">Profilów publikowania</span><span class="sxs-lookup"><span data-stu-id="1895b-173">Publish profiles</span></span>

<span data-ttu-id="1895b-174">Ta sekcja używa programu Visual Studio 2017 i wyższych do tworzenia profilów publikowania.</span><span class="sxs-lookup"><span data-stu-id="1895b-174">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="1895b-175">Po utworzeniu publikacji z wiersza polecenia lub programu Visual Studio jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="1895b-175">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="1895b-176">Publikowanie profili może uprościć proces publikowania.</span><span class="sxs-lookup"><span data-stu-id="1895b-176">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="1895b-177">Wiele profilów publikowania może istnieć.</span><span class="sxs-lookup"><span data-stu-id="1895b-177">Multiple publish profiles can exist.</span></span> <span data-ttu-id="1895b-178">Aby utworzyć profil publikowania w programie Visual Studio, kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="1895b-178">To create a publish profile in Visual Studio, right-click on the project in Solution Explorer and select **Publish**.</span></span> <span data-ttu-id="1895b-179">Można także wybrać **publikowania \<Nazwa projektu >** z menu Kompiluj.</span><span class="sxs-lookup"><span data-stu-id="1895b-179">Alternatively, select **Publish \<project name>** from the build menu.</span></span> <span data-ttu-id="1895b-180">**Publikowania** jest wyświetlany na karcie Strona możliwości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1895b-180">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="1895b-181">Jeśli projekt nie zawiera profil publikowania, zostanie wyświetlona strona następujące:</span><span class="sxs-lookup"><span data-stu-id="1895b-181">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![Wybrany karcie publikowania strony możliwości aplikacji przedstawiający Azure, usługi IIS, FTB, Folder z platformy Azure.](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="1895b-184">Gdy **folderu** jest zaznaczone, **publikowania** przycisk tworzy folder profilu publikowania i publikuje.</span><span class="sxs-lookup"><span data-stu-id="1895b-184">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![** Publikowania ** kartę strony możliwości aplikacji przedstawiający Azure, usługi IIS, FTB, Folder](visual-studio-publish-profiles/_static/pub1.png)

<span data-ttu-id="1895b-186">Po utworzeniu profilu publikowania **publikowania** zmiany, a następnie wybierz **Utwórz nowy profil** do utworzenia nowego profilu.</span><span class="sxs-lookup"><span data-stu-id="1895b-186">Once a publish profile is created, the **Publish** tab changes, and select **Create new profile** to create a new profile.</span></span>

![** Publikowania ** kartę strony możliwości aplikacji przedstawiający FolderProfile — utworzone w ostatnim kroku i Utwórz nowy profil link](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="1895b-188">Kreator publikowania obsługuje następujące elementy docelowe publikowania:</span><span class="sxs-lookup"><span data-stu-id="1895b-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="1895b-189">Usługi aplikacji platformy Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="1895b-189">Microsoft Azure App Service</span></span>
* <span data-ttu-id="1895b-190">Usługi IIS, FTP itp., (na każdym serwerze sieci web)</span><span class="sxs-lookup"><span data-stu-id="1895b-190">IIS, FTP, etc (for any web server)</span></span>
* <span data-ttu-id="1895b-191">Folder</span><span class="sxs-lookup"><span data-stu-id="1895b-191">Folder</span></span>
* <span data-ttu-id="1895b-192">Importowanie profilu (umożliwia importowanie profilów).</span><span class="sxs-lookup"><span data-stu-id="1895b-192">Import profile (allows profile import).</span></span>
* <span data-ttu-id="1895b-193">Maszyny wirtualne Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="1895b-193">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="1895b-194">Zobacz [jakie opcje publikowania jest dla mnie odpowiednia?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="1895b-194">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="1895b-195">Podczas tworzenia profilu publikowania przy użyciu programu Visual Studio, *właściwości/PublishProfiles/\<publikowania name > .pubxml* jest tworzony plik MSBuild.</span><span class="sxs-lookup"><span data-stu-id="1895b-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="1895b-196">To *.pubxml* plik jest plikiem MSBuild i zawiera ustawienia konfiguracji publikowania.</span><span class="sxs-lookup"><span data-stu-id="1895b-196">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="1895b-197">Można zmienić tego pliku dostosowania kompilacji i opublikować procesu.</span><span class="sxs-lookup"><span data-stu-id="1895b-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="1895b-198">Ten plik jest odczytywany przez proces publikowania.</span><span class="sxs-lookup"><span data-stu-id="1895b-198">This file is read by the publishing process.</span></span> <span data-ttu-id="1895b-199">`<LastUsedBuildConfiguration>` jest specjalne, ponieważ jest właściwością globalną i nie powinny należeć do każdego pliku, który jest importowany w kompilacji.</span><span class="sxs-lookup"><span data-stu-id="1895b-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="1895b-200">Zobacz [MSBuild: sposób ustawiania właściwości konfiguracji](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="1895b-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="1895b-201">*.Pubxml* plik nie powinien wyewidencjonowany do kontroli źródła, ponieważ zależy on od *.user* pliku.</span><span class="sxs-lookup"><span data-stu-id="1895b-201">The *.pubxml* file shouldn't be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="1895b-202">*.User* pliku nigdy nie powinna być sprawdzana do kontroli źródła, ponieważ mogą zawierać poufne informacje i jest tylko prawidłowy dla jednego użytkownika i komputera.</span><span class="sxs-lookup"><span data-stu-id="1895b-202">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="1895b-203">Informacje poufne (na przykład hasła publikowania) są szyfrowane na na użytkownika/machine poziomu i przechowywane w *właściwości/PublishProfiles/\<publikowania name >. pubxml.user* pliku.</span><span class="sxs-lookup"><span data-stu-id="1895b-203">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="1895b-204">Ten plik może zawierać informacje poufne, należy **nie** być wyewidencjonowany do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="1895b-204">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="1895b-205">Omówienie sposobu publikowania aplikacji sieci web platformy ASP.NET Core zobacz [hosta i wdrażanie](index.md).</span><span class="sxs-lookup"><span data-stu-id="1895b-205">For an overview of how to publish a web app on ASP.NET Core see [Host and deploy](index.md).</span></span> <span data-ttu-id="1895b-206">[Hostowanie i wdrożyć](index.md) to projekt open source w https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="1895b-206">[Host and deploy](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="1895b-207">`dotnet publish` można użyć folderu, MSDeploy, i [KUDU](https://github.com/projectkudu/kudu/wiki) profilów publikowania:</span><span class="sxs-lookup"><span data-stu-id="1895b-207">`dotnet publish` can use folder, MSDeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="1895b-208">Folder (działa i platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="1895b-208">Folder (works cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="1895b-209">MSDeploy (obecnie ta działa tylko w systemie windows, ponieważ wielu platform nie jest MSDeploy): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="1895b-209">MSDeploy (currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="1895b-210">Pakiet MSDeploy (obecnie ta działa tylko w systemie windows, ponieważ wielu platform nie jest MSDeploy): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="1895b-210">MSDeploy package(currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="1895b-211">W przykładach poprzedzających **nie** przekazać `deployonbuild` do `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="1895b-211">In the preceeding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="1895b-212">Aby uzyskać więcej informacji, zobacz [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="1895b-212">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="1895b-213">`dotnet publish` obsługuje interfejsy API KUDU do publikowania na platformie Azure z dowolną platformą.</span><span class="sxs-lookup"><span data-stu-id="1895b-213">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="1895b-214">Visual Studio publikuje ma obsługę interfejsów API KUDU, ale jest obsługiwany przez websdk dla krzyżowego na wiele publikowanie na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="1895b-214">Visual Studio publish does support the KUDU APIs but it's supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="1895b-215">Dodawanie profilu publikowania do *właściwości/PublishProfiles* folderu o następującej treści:</span><span class="sxs-lookup"><span data-stu-id="1895b-215">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="1895b-216">Uruchom następujące polecenie zips zapasową publikowania i opublikuj go na platformie Azure przy użyciu interfejsów API KUDU:</span><span class="sxs-lookup"><span data-stu-id="1895b-216">Running the following command zips up the publish contents and publish it to Azure using the KUDU APIs:</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="1895b-217">Ustaw następujące właściwości programu MSBuild, korzystając z odpowiednim profilem publikowania:</span><span class="sxs-lookup"><span data-stu-id="1895b-217">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="1895b-218">Podczas publikowania z tym profilem o nazwie *FolderProfile*, aby można było wykonać dowolne z poniższych poleceń:</span><span class="sxs-lookup"><span data-stu-id="1895b-218">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="1895b-219">Podczas wywoływania [kompilacji dotnet](/dotnet/core/tools/dotnet-build), wywołuje `msbuild` do uruchomienia kompilacji, a następnie opublikuj przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="1895b-219">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="1895b-220">Wywoływanie `dotnet build` lub `msbuild` jest równoważne podczas przekazywania w folderze profilu.</span><span class="sxs-lookup"><span data-stu-id="1895b-220">Calling `dotnet build` or `msbuild` is essentially equivalent when passing in a folder profile.</span></span> <span data-ttu-id="1895b-221">Podczas wywoływania MSBuild bezpośrednio w systemie Windows, programu .NET Framework w wersji programu MSBuild jest używany.</span><span class="sxs-lookup"><span data-stu-id="1895b-221">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="1895b-222">MSDeploy jest obecnie ograniczona do komputerów z systemem Windows do publikowania.</span><span class="sxs-lookup"><span data-stu-id="1895b-222">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="1895b-223">Wywoływanie `dotnet build` z systemem innym niż folderu Program MSBuild wywołuje profilu, a MSBuild używa MSDeploy na profilach nie folder.</span><span class="sxs-lookup"><span data-stu-id="1895b-223">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="1895b-224">Wywoływanie `dotnet build` profilu folderu nie wywołuje MSBuild (przy użyciu MSDeploy) i powoduje błąd (nawet wtedy, gdy uruchomiona na platformie systemu Windows).</span><span class="sxs-lookup"><span data-stu-id="1895b-224">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="1895b-225">Aby opublikować za pomocą profilu — do folderu, bezpośrednio wywołać program MSBuild.</span><span class="sxs-lookup"><span data-stu-id="1895b-225">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="1895b-226">Profil publikowania następujący folder został utworzony za pomocą programu Visual Studio i publikuje do udziału sieciowego:</span><span class="sxs-lookup"><span data-stu-id="1895b-226">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="1895b-227">Uwaga `<LastUsedBuildConfiguration>` ma ustawioną wartość `Release`.</span><span class="sxs-lookup"><span data-stu-id="1895b-227">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="1895b-228">Podczas publikowania z programu Visual Studio `<LastUsedBuildConfiguration>` wartości właściwości konfiguracji jest ustawiona przy użyciu wartości, gdy proces publikowania zostanie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="1895b-228">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="1895b-229">`<LastUsedBuildConfiguration>` Właściwości konfiguracji jest szczególna i nie powinna zostać zastąpiona w zaimportowanym pliku programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="1895b-229">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="1895b-230">Ta właściwość może być zastąpiona w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="1895b-230">This property can be overridden from the command line.</span></span> <span data-ttu-id="1895b-231">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1895b-231">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="1895b-232">Przy użyciu programu MSBuild:</span><span class="sxs-lookup"><span data-stu-id="1895b-232">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="1895b-233">Publikowanie do punktu końcowego MSDeploy z wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="1895b-233">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="1895b-234">Jak wcześniej wspomniano, publikowanie można osiągnąć za pomocą `dotnet publish` lub `msbuild` polecenia.</span><span class="sxs-lookup"><span data-stu-id="1895b-234">As previously mentioned, publishing can be accomplished using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="1895b-235">`dotnet publish` jest uruchamiany w kontekście .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1895b-235">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="1895b-236">`msbuild` Polecenia wymaga programu .NET framework i jest ograniczone do środowiska systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="1895b-236">The `msbuild` command requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="1895b-237">Najprostszym sposobem publikowania z MSDeploy jest najpierw utworzyć profil publikowania w programie Visual Studio 2017 i Użyj profilu z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="1895b-237">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="1895b-238">W poniższym przykładzie jest tworzona aplikacja sieci web platformy ASP.NET Core (przy użyciu `dotnet new mvc`), a profil publikowania platformy Azure są dodawane z programem Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1895b-238">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="1895b-239">Uruchom `msbuild` z **wiersz polecenia dla programu VS 2017 deweloperów**.</span><span class="sxs-lookup"><span data-stu-id="1895b-239">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="1895b-240">Wiersz polecenia dewelopera jest prawidłowy *msbuild.exe* w swojej ścieżce niektórych zestaw zmiennych MSBuild.</span><span class="sxs-lookup"><span data-stu-id="1895b-240">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="1895b-241">Program MSBuild używa następującej składni:</span><span class="sxs-lookup"><span data-stu-id="1895b-241">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="1895b-242">Pobierz `Password` z  *\<publikowania name >. PublishSettings* pliku.</span><span class="sxs-lookup"><span data-stu-id="1895b-242">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="1895b-243">Pobierz *. PublishSettings* plik z dowolnej:</span><span class="sxs-lookup"><span data-stu-id="1895b-243">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="1895b-244">Eksplorator rozwiązań: Kliknij prawym przyciskiem myszy w aplikacji sieci Web i wybierz **pobrać profilu publikowania**.</span><span class="sxs-lookup"><span data-stu-id="1895b-244">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="1895b-245">Portalu zarządzania Azure: Wybierz **profilu publikowania Get** w bloku aplikacja sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1895b-245">The Azure Management Portal: Select **Get publish profile** from the Web App blade.</span></span>

<span data-ttu-id="1895b-246">`Username` znajdują się w profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="1895b-246">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="1895b-247">Następujące przykładowe zastosowania "Web11112 — narzędzie Web Deploy" profil publikowania:</span><span class="sxs-lookup"><span data-stu-id="1895b-247">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="1895b-248">Wykluczanie plików</span><span class="sxs-lookup"><span data-stu-id="1895b-248">Excluding files</span></span>

<span data-ttu-id="1895b-249">Podczas publikowania aplikacji sieci web platformy ASP.NET Core, artefaktów kompilacji i zawartość *wwwroot* folderu są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="1895b-249">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="1895b-250">`msbuild` obsługuje [wzorce globbing](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="1895b-250">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="1895b-251">Na przykład następująca `<Content>` znacznika elementu wyklucza cały tekst (*.txt*) plików ze *zawartością/wwwroot* folderze i jego podfolderach.</span><span class="sxs-lookup"><span data-stu-id="1895b-251">For example, the following `<Content>` element markup excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="1895b-252">Kod znaczników powyżej można dodać do profilu publikowania lub *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="1895b-252">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="1895b-253">Po dodaniu do *.csproj* plik, reguła zostanie dodany do wszystkich profilów publikowania w projekcie.</span><span class="sxs-lookup"><span data-stu-id="1895b-253">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="1895b-254">Następujące `<MsDeploySkipRules>` exludes znacznika elementu wszystkie pliki z *zawartością/wwwroot* folderu:</span><span class="sxs-lookup"><span data-stu-id="1895b-254">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="1895b-255">`<MsDeploySkipRules>` nie usuwa *pominąć* elementów docelowych z witryny wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="1895b-255">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="1895b-256">`<Content>` pliki docelowe i foldery są usuwane z lokacji wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="1895b-256">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="1895b-257">Na przykład załóżmy, że wdrożonej aplikacji sieci web ma następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="1895b-257">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="1895b-258">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1895b-258">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="1895b-259">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1895b-259">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="1895b-260">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1895b-260">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="1895b-261">Jeśli następujące `<MsDeploySkipRules>` znaczników dodaniu, nie można usunąć te pliki w witrynie wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="1895b-261">If the following `<MsDeploySkipRules>` markup is added, those files wouldn't be deleted on the deployment site.</span></span>

``` xml
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

<span data-ttu-id="1895b-262">`<MsDeploySkipRules>` Uniemożliwia znaczników pokazanym powyżej *pominięte* plików przed depoyed, ale nie usuwa tych plików, gdy są one wdrażane.</span><span class="sxs-lookup"><span data-stu-id="1895b-262">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed but won't delete those files once they're deployed.</span></span>

<span data-ttu-id="1895b-263">Następujące `<Content>` znaczników usuwa pliki docelowe w witrynie wdrażania:</span><span class="sxs-lookup"><span data-stu-id="1895b-263">The following `<Content>` markup deletes the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="1895b-264">Za pomocą wiersza polecenia z `<Content>` znaczników powyżej powoduje dane wyjściowe podobne do następującego:</span><span class="sxs-lookup"><span data-stu-id="1895b-264">Using command-line deployment with the `<Content>` markup above results in output similar to the following:</span></span>

``` console
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

## <a name="including-files"></a><span data-ttu-id="1895b-265">W tym pliki</span><span class="sxs-lookup"><span data-stu-id="1895b-265">Including files</span></span>

<span data-ttu-id="1895b-266">Obejmuje następujące znaczników *obrazów* folderów znajdujących się poza katalogiem projektu do *wwwroot/obrazów* folderu publikowania witryny:</span><span class="sxs-lookup"><span data-stu-id="1895b-266">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="1895b-267">Znaczników można dodać do *.csproj* pliku lub profil publikowania.</span><span class="sxs-lookup"><span data-stu-id="1895b-267">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="1895b-268">Jeśli jest ona dodawana do *.csproj* pliku umieścił je w każdym profilu publikowania do projektu.</span><span class="sxs-lookup"><span data-stu-id="1895b-268">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="1895b-269">Następujące wyróżnione znaczników pokazuje sposób do:</span><span class="sxs-lookup"><span data-stu-id="1895b-269">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="1895b-270">Skopiuj plik z spoza projektu do *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="1895b-270">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="1895b-271">Wyklucz *wwwroot\Content* folderu.</span><span class="sxs-lookup"><span data-stu-id="1895b-271">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="1895b-272">Wyklucz *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1895b-272">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="1895b-273">Zobacz [WebSDK Readme](https://github.com/aspnet/websdk) dla większej liczby próbek wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="1895b-273">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="1895b-274">Uruchom element docelowy przed lub po opublikowaniu</span><span class="sxs-lookup"><span data-stu-id="1895b-274">Run a target before or after publishing</span></span>

<span data-ttu-id="1895b-275">Wbudowane `BeforePublish` i `AfterPublish` elementy docelowe może służyć do wykonywania elementu docelowego przed lub po lokalizacja docelowa publikowania.</span><span class="sxs-lookup"><span data-stu-id="1895b-275">The built-in `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="1895b-276">Następujący kod, mogą być dodawane do profilu publikowania do logowania wiadomości dane wyjściowe konsoli przed i po opublikowaniu:</span><span class="sxs-lookup"><span data-stu-id="1895b-276">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="1895b-277">Usługa Kudu</span><span class="sxs-lookup"><span data-stu-id="1895b-277">The Kudu service</span></span>

<span data-ttu-id="1895b-278">Aby wyświetlić pliki w wdrożenia aplikacji sieci web usługi aplikacji Azure, użyj [usługi Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="1895b-278">To view the files in the an Azure Apps Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="1895b-279">Dołącz `scm` token na nazwę aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="1895b-279">Append the `scm` token to the name of the web app.</span></span> <span data-ttu-id="1895b-280">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1895b-280">For example:</span></span>

| <span data-ttu-id="1895b-281">Adres URL</span><span class="sxs-lookup"><span data-stu-id="1895b-281">URL</span></span>                                    | <span data-ttu-id="1895b-282">Wynik</span><span class="sxs-lookup"><span data-stu-id="1895b-282">Result</span></span>      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="1895b-283">Aplikacja sieci Web</span><span class="sxs-lookup"><span data-stu-id="1895b-283">Web App</span></span>     |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="1895b-284">Program kudu usługi</span><span class="sxs-lookup"><span data-stu-id="1895b-284">Kudu sevice</span></span> |

<span data-ttu-id="1895b-285">Wybierz [konsoli debugowania](https://github.com/projectkudu/kudu/wiki/Kudu-console) elementu menu Wyświetl/Edytuj/delete/Dodaj pliki.</span><span class="sxs-lookup"><span data-stu-id="1895b-285">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1895b-286">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1895b-286">Additional resources</span></span>

* <span data-ttu-id="1895b-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) upraszcza wdrażanie aplikacji sieci web i witryn sieci Web na serwerach usług IIS.</span><span class="sxs-lookup"><span data-stu-id="1895b-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="1895b-288">[https://github.com/ASPNET/websdk](https://github.com/aspnet/websdk/issues): plik problemy i żądania funkcji dla wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="1895b-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
