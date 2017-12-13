---
title: "Tworzenie profilów publikowania dla programu Visual Studio i MSBuild"
author: rick-anderson
description: "W tym artykule wyjaśniono publikowania w sieci web w programie Visual Studio."
keywords: "Platformy ASP.NET Core sieci web publikowanie, publikowania, msbuild, narzędzie web deploy, Opublikuj dotnet, Visual Studio 2017 r."
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: f010f9d90165ce4d6718fe1440e600985f21a01d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a><span data-ttu-id="31326-104">Tworzenie profilów dla programu Visual Studio i MSBuild, jak wdrażać aplikacje platformy ASP.NET Core publikowania</span><span class="sxs-lookup"><span data-stu-id="31326-104">Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps</span></span>

<span data-ttu-id="31326-105">Przez [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="31326-105">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="31326-106">Ten artykuł skupia się na tworzenie za pomocą programu Visual Studio 2017 profilów publikowania.</span><span class="sxs-lookup"><span data-stu-id="31326-106">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="31326-107">Profile utworzone za pomocą programu Visual Studio można uruchomić z MSBuild i Visual Studio 2017 r.</span><span class="sxs-lookup"><span data-stu-id="31326-107">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="31326-108">Artykuł zawiera szczegółowe informacje o procesie publikowania.</span><span class="sxs-lookup"><span data-stu-id="31326-108">The article provides details of the publishing process.</span></span> <span data-ttu-id="31326-109">Zobacz [publikowania aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) instrukcje dotyczące publikowania na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="31326-109">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="31326-110">Następujące *.csproj* plik został utworzony przy użyciu polecenia `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="31326-110">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="31326-111">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="31326-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="31326-112">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="31326-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="31326-113">`Sdk` Atrybutu w `<Project>` elementu powyżej znacznika (w pierwszym wierszu) wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="31326-113">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="31326-114">Importy `props` plik z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* na początku.</span><span class="sxs-lookup"><span data-stu-id="31326-114">Imports the `props` file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="31326-115">Importuje plik elementów docelowych z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* na końcu.</span><span class="sxs-lookup"><span data-stu-id="31326-115">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="31326-116">Domyślna lokalizacja dla `MSBuildSDKsPath` (za pomocą programu Visual Studio Enterprise 2017) jest *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folderu.</span><span class="sxs-lookup"><span data-stu-id="31326-116">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="31326-117">`Microsoft.NET.Sdk.Web`zależy od:</span><span class="sxs-lookup"><span data-stu-id="31326-117">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="31326-118">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="31326-118">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="31326-119">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="31326-119">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="31326-120">Co spowoduje, że następujące `props` i `targets` do zaimportowania:</span><span class="sxs-lookup"><span data-stu-id="31326-120">Which causes the following `props` and `targets` to be imported:</span></span>

* <span data-ttu-id="31326-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="31326-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="31326-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="31326-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="31326-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="31326-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="31326-124">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="31326-124">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="31326-125">Publikowanie elementów docelowych o tej zostanie ponownie import prawidłowego zestawu elementów docelowych, na podstawie publikowania metody używane.</span><span class="sxs-lookup"><span data-stu-id="31326-125">Publish targets will again import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="31326-126">Podczas ładowania projektu programu MSBuild lub Visual Studio są wykonywane następujące akcje wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="31326-126">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="31326-127">Tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="31326-127">Build project</span></span>
* <span data-ttu-id="31326-128">Obliczenia bazy danych plików do opublikowania</span><span class="sxs-lookup"><span data-stu-id="31326-128">Compute files to publish</span></span>
* <span data-ttu-id="31326-129">Publikowanie plików do lokalizacji docelowej</span><span class="sxs-lookup"><span data-stu-id="31326-129">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="31326-130">Obliczeniowe elementy projektu</span><span class="sxs-lookup"><span data-stu-id="31326-130">Compute project items</span></span>

<span data-ttu-id="31326-131">Po załadowaniu projektu są obliczane elementy projektu (pliki).</span><span class="sxs-lookup"><span data-stu-id="31326-131">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="31326-132">`item type` Atrybut określa sposób przetwarzania pliku.</span><span class="sxs-lookup"><span data-stu-id="31326-132">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="31326-133">Domyślnie *.cs* pliki znajdują się w `Compile` listy elementów.</span><span class="sxs-lookup"><span data-stu-id="31326-133">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="31326-134">Pliki w `Compile` są kompilowane listy elementów.</span><span class="sxs-lookup"><span data-stu-id="31326-134">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="31326-135">`Content` Listy elementów zawiera pliki, które zostaną opublikowane oprócz plików wyjściowych kompilacji.</span><span class="sxs-lookup"><span data-stu-id="31326-135">The `Content` item list contains files that will be published in addition to the build outputs.</span></span> <span data-ttu-id="31326-136">Domyślnie pliki zgodne wwwroot wzorzec / ** zostaną uwzględnione w `Content` elementu.</span><span class="sxs-lookup"><span data-stu-id="31326-136">By default, files matching the pattern wwwroot/** will be included in the `Content` item.</span></span> <span data-ttu-id="31326-137">[Wwwroot / ** to wzorzec globbing](https://gruntjs.com/configuring-tasks#globbing-patterns) Określa, że wszystkie pliki w *wwwroot* folderu **i** podfoldery.</span><span class="sxs-lookup"><span data-stu-id="31326-137">[wwwroot/** is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="31326-138">Aby jawnie dodać plik do listy publikowania, należy dodać bezpośrednio w pliku *.csproj* plików, jak pokazano w [dołączanie plików](#including-files).</span><span class="sxs-lookup"><span data-stu-id="31326-138">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="31326-139">Po wybraniu **publikowania** przycisk w Visual Studio lub po opublikowaniu z wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="31326-139">When you select the **Publish** button in Visual Studio or when you publish from command line:</span></span>

- <span data-ttu-id="31326-140">Właściwości/elementy są obliczane (pliki, które są niezbędne do utworzenia).</span><span class="sxs-lookup"><span data-stu-id="31326-140">The properties/items are computed (the files that are needed to build).</span></span>
- <span data-ttu-id="31326-141">Tylko dla programu Visual Studio: pakiety NuGet są przywracane.</span><span class="sxs-lookup"><span data-stu-id="31326-141">Visual Studio only: NuGet packages are restored.</span></span>  <span data-ttu-id="31326-142">(Przywróć musi mieć jawne przez użytkownika za pomocą interfejsu wiersza polecenia).</span><span class="sxs-lookup"><span data-stu-id="31326-142">(Restore needs to be explicit by the user on the CLI.)</span></span>
- <span data-ttu-id="31326-143">Tworzy projekt.</span><span class="sxs-lookup"><span data-stu-id="31326-143">The project builds.</span></span>
- <span data-ttu-id="31326-144">Publikowanie elementów są obliczane (pliki, które są niezbędne do publikowania).</span><span class="sxs-lookup"><span data-stu-id="31326-144">The publish items are computed (the files that are needed to publish).</span></span>
- <span data-ttu-id="31326-145">Projekt nie zostanie opublikowany.</span><span class="sxs-lookup"><span data-stu-id="31326-145">The project is published.</span></span> <span data-ttu-id="31326-146">(Obliczona pliki są kopiowane do lokalizacji docelowej publikowania).</span><span class="sxs-lookup"><span data-stu-id="31326-146">(The computed files are copied to the publish destination.)</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="31326-147">Publikowanie podstawowe wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="31326-147">Basic command line publishing</span></span>

<span data-ttu-id="31326-148">W tej sekcji działa na wszystkich platformach .NET Core, obsługiwane i nie wymaga programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="31326-148">This section works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="31326-149">W poniższych przykładach `dotnet publish` polecenie jest uruchamiane od katalogu projektu (która zawiera *.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="31326-149">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="31326-150">Jeśli nie masz w folderze projektu, jawnie można przekazać w ścieżce pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="31326-150">If you're not in the project folder, you can explicitly pass in the project file path.</span></span> <span data-ttu-id="31326-151">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="31326-151">For example:</span></span>

```console
dotnet publish  c:/webs/web1
```

<span data-ttu-id="31326-152">Uruchom następujące polecenia, aby utworzyć i opublikować aplikację sieci web:</span><span class="sxs-lookup"><span data-stu-id="31326-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="31326-153">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="31326-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="31326-154">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="31326-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

--------------

<span data-ttu-id="31326-155">`dotnet publish` Generuje dane wyjściowe podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="31326-155">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="31326-156">Wartość domyślna publikowanie folderu jest `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="31326-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="31326-157">Wartość domyślna dla `$(Configuration)` jest debugowania.</span><span class="sxs-lookup"><span data-stu-id="31326-157">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="31326-158">W powyższym przykładowym `<TargetFramework>` jest `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="31326-158">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="31326-159">`dotnet publish -h`Wyświetla Pomoc dla publikacji.</span><span class="sxs-lookup"><span data-stu-id="31326-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="31326-160">Określa polecenie `Release` kompilacji i publikowania katalogu:</span><span class="sxs-lookup"><span data-stu-id="31326-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="31326-161">`dotnet publish` Wywołania polecenia `MSBuild` który wywołuje `Publish` docelowej.</span><span class="sxs-lookup"><span data-stu-id="31326-161">The `dotnet publish` command calls `MSBuild` which invokes the `Publish` target.</span></span> <span data-ttu-id="31326-162">Parametry przekazane do `dotnet publish` są przekazywane do `MSBuild`.</span><span class="sxs-lookup"><span data-stu-id="31326-162">Any parameters passed to `dotnet publish` are passed to `MSBuild`.</span></span> <span data-ttu-id="31326-163">`-c` Mapuje parametru `Configuration` właściwości programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="31326-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="31326-164">`-o` Mapuje parametru `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="31326-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="31326-165">Można przekazać właściwości programu MSBuild przy użyciu jednej z następujących formatów:</span><span class="sxs-lookup"><span data-stu-id="31326-165">You can pass MSBuild properties using either of the following formats:</span></span>

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

<span data-ttu-id="31326-166">Polecenie publikuje `Release` kompilacji do udziału sieciowego:</span><span class="sxs-lookup"><span data-stu-id="31326-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="31326-167">Udział sieciowy zostanie określony z kreskami ukośnymi (*//r8/*) i działa na wszystkich platformach .NET Core, obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="31326-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="31326-168">Upewnij się, że opublikowana aplikacja dla wdrożenia nie jest uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="31326-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="31326-169">Pliki w *publikowania* folderu są zablokowane, gdy aplikacja jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="31326-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="31326-170">Wdrożenie nie może wystąpić, ponieważ zablokowany się, że nie można skopiować plików.</span><span class="sxs-lookup"><span data-stu-id="31326-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="31326-171">Profilów publikowania</span><span class="sxs-lookup"><span data-stu-id="31326-171">Publish profiles</span></span>

<span data-ttu-id="31326-172">Ta sekcja używa programu Visual Studio 2017 i wyższych do tworzenia profilów publikowania.</span><span class="sxs-lookup"><span data-stu-id="31326-172">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="31326-173">Po utworzeniu można publikować z wiersza polecenia lub programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="31326-173">Once created, you can publish from Visual Studio or the command line.</span></span>

<span data-ttu-id="31326-174">Publikowanie profili może uprościć proces publikowania.</span><span class="sxs-lookup"><span data-stu-id="31326-174">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="31326-175">Może mieć wiele profilów publikowania.</span><span class="sxs-lookup"><span data-stu-id="31326-175">You can have multiple publish profiles.</span></span> <span data-ttu-id="31326-176">Aby utworzyć profil publikowania w programie Visual Studio, kliknij prawym przyciskiem myszy projekt w rozwiązaniu Eksploruj i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="31326-176">To create a publish profile in Visual Studio, right click on the project in Solution Explore and select **Publish**.</span></span> <span data-ttu-id="31326-177">Alternatywnie można wybrać **publikowania \<Nazwa projektu >** z menu Kompiluj.</span><span class="sxs-lookup"><span data-stu-id="31326-177">Alternatively, you can select **Publish     \<project name>** from the build menu.</span></span> <span data-ttu-id="31326-178">**Publikowania** jest wyświetlany na karcie Strona możliwości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="31326-178">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="31326-179">Jeśli projekt nie zawiera profil publikowania, zostanie wyświetlona strona następujące:</span><span class="sxs-lookup"><span data-stu-id="31326-179">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![** Publikowania ** kartę strony możliwości aplikacji przedstawiający Azure, usługi IIS, FTB, wybrany Folder z platformy Azure.](web-publishing-vs/_static/az.png)

<span data-ttu-id="31326-182">Gdy **folderu** jest zaznaczone, **publikowania** przycisk tworzy folder profilu publikowania i publikuje.</span><span class="sxs-lookup"><span data-stu-id="31326-182">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![** Publikowania ** kartę strony możliwości aplikacji przedstawiający Azure, usługi IIS, FTB, Folder](web-publishing-vs/_static/pub1.png)

<span data-ttu-id="31326-184">Po utworzeniu profilu publikowania **publikowania** kartę zmiany i wybierz **Utwórz nowy profil** do utworzenia nowego profilu.</span><span class="sxs-lookup"><span data-stu-id="31326-184">Once a publish profile is created, the **Publish** tab changes, and you select **Create new profile** to create a new profile.</span></span>

![** Publikowania ** kartę strony możliwości aplikacji przedstawiający FolderProfile — utworzone w ostatnim kroku i Utwórz nowy profil link](web-publishing-vs/_static/create_new.png)

<span data-ttu-id="31326-186">Kreator publikowania obsługuje następujące elementy docelowe publikowania:</span><span class="sxs-lookup"><span data-stu-id="31326-186">The Publish wizard supports the following publish targets:</span></span>

- <span data-ttu-id="31326-187">Usługi aplikacji platformy Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="31326-187">Microsoft Azure App Service</span></span>
- <span data-ttu-id="31326-188">Usługi IIS, FTP itp., (na każdym serwerze sieci web)</span><span class="sxs-lookup"><span data-stu-id="31326-188">IIS, FTP, etc (for any web server)</span></span>
- <span data-ttu-id="31326-189">Folder</span><span class="sxs-lookup"><span data-stu-id="31326-189">Folder</span></span>
- <span data-ttu-id="31326-190">Importowanie profilu (umożliwia importowanie profilu).</span><span class="sxs-lookup"><span data-stu-id="31326-190">Import profile (allows you to import a profile).</span></span>
- <span data-ttu-id="31326-191">Maszyny wirtualne Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="31326-191">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="31326-192">Zobacz [jakie opcje publikowania jest dla mnie odpowiednia?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="31326-192">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="31326-193">Podczas tworzenia profilu publikowania za pomocą programu Visual Studio *właściwości/PublishProfiles/\<publikowania name > .pubxml* jest tworzony plik MSBuild.</span><span class="sxs-lookup"><span data-stu-id="31326-193">When you create a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="31326-194">To *.pubxml* plik jest plikiem MSBuild i zawiera ustawienia konfiguracji publikowania.</span><span class="sxs-lookup"><span data-stu-id="31326-194">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="31326-195">Można zmienić tego pliku dostosowania kompilacji i opublikować procesu.</span><span class="sxs-lookup"><span data-stu-id="31326-195">You can change this file to customize the build and publish process.</span></span> <span data-ttu-id="31326-196">Ten plik jest odczytywany przez proces publikowania.</span><span class="sxs-lookup"><span data-stu-id="31326-196">This file is read by the publishing process.</span></span> <span data-ttu-id="31326-197">`<LastUsedBuildConfiguration>`jest specjalne, ponieważ jest właściwością globalną i nie powinny należeć do każdego pliku, który jest importowany w kompilacji.</span><span class="sxs-lookup"><span data-stu-id="31326-197">`<LastUsedBuildConfiguration>` is special because it’s a global property and shouldn’t be in any file that’s imported in the build.</span></span> <span data-ttu-id="31326-198">Zobacz [MSBuild: sposób ustawiania właściwości konfiguracji](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="31326-198">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="31326-199">*.Pubxml* pliku nie powinna być sprawdzana do kontroli źródła, ponieważ zależy on od *.user* pliku.</span><span class="sxs-lookup"><span data-stu-id="31326-199">The *.pubxml* file should not be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="31326-200">*.User* pliku nigdy nie powinna być sprawdzana do kontroli źródła, ponieważ mogą zawierać poufne informacje i jest tylko prawidłowy dla jednego użytkownika i komputera.</span><span class="sxs-lookup"><span data-stu-id="31326-200">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="31326-201">Informacje poufne (na przykład hasła publikowania) są szyfrowane na na użytkownika/machine poziomu i przechowywane w *właściwości/PublishProfiles/\<publikowania name >. pubxml.user* pliku.</span><span class="sxs-lookup"><span data-stu-id="31326-201">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="31326-202">Ten plik może zawierać informacje poufne, należy **nie** być wyewidencjonowany do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="31326-202">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="31326-203">Omówienie sposobu publikowania aplikacji sieci web platformy ASP.NET Core zobacz [publikowania i wdrażania](index.md).</span><span class="sxs-lookup"><span data-stu-id="31326-203">For an overview of how to publish a web app on ASP.NET Core see [Publishing and Deployment](index.md).</span></span> <span data-ttu-id="31326-204">[Publikowanie i wdrażanie](index.md) to projekt open source w https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="31326-204">[Publishing and Deployment](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

 <span data-ttu-id="31326-205">`dotnet publish`można użyć folderu, Msdeploy, i [KUDU](https://github.com/projectkudu/kudu/wiki) profilów publikowania:</span><span class="sxs-lookup"><span data-stu-id="31326-205">`dotnet publish` can use folder, Msdeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="31326-206">Folder (działa między platformami)`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="31326-206">Folder (works cross-platform) `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="31326-207">MSDeploy (obecnie ta działa tylko w systemie windows, ponieważ nie jest msdeploy i platform):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="31326-207">Msdeploy (currently this only works in windows since msdeploy is not cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="31326-208">Pakiet MSDeploy (obecnie ta działa tylko w systemie windows, ponieważ nie jest msdeploy i platform):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="31326-208">Msdeploy package(currently this only works in windows since msdeploy is not cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="31326-209">W przykładach poprzedzających **nie** przekazać `deployonbuild` do `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="31326-209">In the preceeding samples, **don’t** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="31326-210">Aby uzyskać więcej informacji, zobacz [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)</span><span class="sxs-lookup"><span data-stu-id="31326-210">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)</span></span>

<span data-ttu-id="31326-211">`dotnet publish`obsługuje interfejsy API KUDU do publikowania na platformie Azure z dowolną platformą.</span><span class="sxs-lookup"><span data-stu-id="31326-211">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="31326-212">Visual Studio publikuje ma obsługę interfejsów API KUDU, ale jest obsługiwany przez websdk dla krzyżowego na wiele publikowanie na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="31326-212">Visual Studio publish does support the KUDU APIs but it is supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="31326-213">Dodawanie profilu publikowania do *właściwości/PublishProfiles* folderu o następującej treści:</span><span class="sxs-lookup"><span data-stu-id="31326-213">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="31326-214">Uruchom następujące polecenie zip zapasową publikowania zawartości i opublikować go na platformie Azure przy użyciu interfejsów API KUDU.</span><span class="sxs-lookup"><span data-stu-id="31326-214">Running the following command will zip up the publish contents and publish it to Azure using the KUDU APIs.</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="31326-215">Ustaw następujące właściwości programu MSBuild, korzystając z odpowiednim profilem publikowania:</span><span class="sxs-lookup"><span data-stu-id="31326-215">Set the following MSBuild properties when using a publish profile:</span></span>

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

<span data-ttu-id="31326-216">Na przykład podczas publikowania z tym profilem o nazwie *FolderProfile* możesz wykonać dowolne z poniższych poleceń.</span><span class="sxs-lookup"><span data-stu-id="31326-216">For example, when publishing with a profile named *FolderProfile* you can execute either of the commands below.</span></span>

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="31326-217">Gdy wywołanie `dotnet build` zostanie wywołany `msbuild` do uruchomienia kompilacji, a następnie opublikuj przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="31326-217">When you invoke `dotnet build` it will call `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="31326-218">Wywoływanie `dotnet build` lub `msbuild` jest równoważne podczas przekazywania w folderze profilu.</span><span class="sxs-lookup"><span data-stu-id="31326-218">Calling `dotnet build` or `msbuild` is essentially equivalent when you pass in a folder profile.</span></span> <span data-ttu-id="31326-219">Podczas wywoływania MSBuild bezpośrednio w systemie Windows można pobrać programu .NET Framework w wersji programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="31326-219">When calling MSBuild directly on Windows you get the .NET Framework version of MSBuild.</span></span>  <span data-ttu-id="31326-220">MSDeploy jest obecnie ograniczona do komputerów z systemem Windows do publikowania.</span><span class="sxs-lookup"><span data-stu-id="31326-220">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="31326-221">Wywoływanie `dotnet build` z systemem innym niż folderu Program MSBuild wywołuje profilu, a MSBuild używa MSDeploy na profilach nie folder.</span><span class="sxs-lookup"><span data-stu-id="31326-221">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="31326-222">Wywoływanie `dotnet build` profilu folderu nie wywołuje MSBuild (przy użyciu MSDeploy) i powoduje błąd (nawet wtedy, gdy uruchomiona na platformie systemu Windows).</span><span class="sxs-lookup"><span data-stu-id="31326-222">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="31326-223">Aby opublikować za pomocą profilu — do folderu, bezpośrednio wywołać program MSBuild.</span><span class="sxs-lookup"><span data-stu-id="31326-223">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="31326-224">Profil publikowania następujący folder został utworzony za pomocą programu Visual Studio i publikuje do udziału sieciowego:</span><span class="sxs-lookup"><span data-stu-id="31326-224">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

<span data-ttu-id="31326-225">Uwaga `<LastUsedBuildConfiguration>` ma ustawioną wartość `Release`.</span><span class="sxs-lookup"><span data-stu-id="31326-225">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span>  <span data-ttu-id="31326-226">Podczas publikowania z programu Visual Studio `<LastUsedBuildConfiguration>` wartości właściwości konfiguracji jest ustawiona przy użyciu wartości, gdy proces publikowania zostanie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="31326-226">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="31326-227">`<LastUsedBuildConfiguration>` Właściwości konfiguracji jest szczególna i nie powinna zostać zastąpiona w zaimportowanym pliku programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="31326-227">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn’t be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="31326-228">Można zastąpić tę właściwość z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="31326-228">You can override this property from the command line.</span></span> <span data-ttu-id="31326-229">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="31326-229">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="31326-230">Przy użyciu programu MSBuild:</span><span class="sxs-lookup"><span data-stu-id="31326-230">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="31326-231">Publikowanie do punktu końcowego MSDeploy z wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="31326-231">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="31326-232">Jak wcześniej wspomniano, można opublikować za pomocą `dotnet publish` lub `msbuild` polecenia.</span><span class="sxs-lookup"><span data-stu-id="31326-232">As previously mentioned, you can publish using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="31326-233">`dotnet publish`jest uruchamiany w kontekście .NET Core.</span><span class="sxs-lookup"><span data-stu-id="31326-233">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="31326-234">`msbuild`wymaga programu .NET framework i jest ograniczone do środowiska systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="31326-234">`msbuild` requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="31326-235">Najprostszym sposobem publikowania z MSDeploy jest najpierw utworzyć profil publikowania w programie Visual Studio 2017 i Użyj profilu z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="31326-235">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="31326-236">W poniższym przykładzie jest tworzona aplikacja sieci web platformy ASP.NET Core (przy użyciu `dotnet new mvc`) i dodaje profil publikowania platformy Azure z programem Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="31326-236">In the following sample, an ASP.NET Core web app is created ( using `dotnet new mvc`) and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="31326-237">Uruchomienie `msbuild` z **wiersz polecenia dla programu VS 2017 deweloperów**.</span><span class="sxs-lookup"><span data-stu-id="31326-237">You run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="31326-238">Wiersz polecenia dewelopera ma poprawny *msbuild.exe* w swojej ścieżce i ustaw niektóre zmienne MSBuild.</span><span class="sxs-lookup"><span data-stu-id="31326-238">The Developer Command Prompt will have the correct *msbuild.exe* in its path and set some MSBuild variables.</span></span>

<span data-ttu-id="31326-239">Program MSBuild używa następującej składni:</span><span class="sxs-lookup"><span data-stu-id="31326-239">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="31326-240">Możesz uzyskać `Password` z  *\<publikowania name >. PublishSettings* pliku.</span><span class="sxs-lookup"><span data-stu-id="31326-240">You can get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="31326-241">Możesz pobrać *. PublishSettings* plik:</span><span class="sxs-lookup"><span data-stu-id="31326-241">You can download the *.PublishSettings* file from:</span></span>

- <span data-ttu-id="31326-242">Eksplorator rozwiązań: kliknij prawym przyciskiem myszy aplikację sieci Web i wybierz **pobrać profilu publikowania**.</span><span class="sxs-lookup"><span data-stu-id="31326-242">Solution Explorer: Right click on the Web App and select **Download Publish Profile**.</span></span>
- <span data-ttu-id="31326-243">Portalu zarządzania Azure: Wybierz **profilu publikowania Get** w bloku aplikacja sieci Web.</span><span class="sxs-lookup"><span data-stu-id="31326-243">The Azure Management Portal: Select **Get publish profile** from the  Web App blade.</span></span>

<span data-ttu-id="31326-244">`Username`znajdują się w profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="31326-244">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="31326-245">Następujące przykładowe zastosowania "Web11112 — narzędzie Web Deploy" profil publikowania:</span><span class="sxs-lookup"><span data-stu-id="31326-245">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="31326-246">Wykluczanie plików</span><span class="sxs-lookup"><span data-stu-id="31326-246">Excluding files</span></span>

<span data-ttu-id="31326-247">Podczas publikowania aplikacji sieci web platformy ASP.NET Core, artefaktów kompilacji i zawartość *wwwroot* folderu są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="31326-247">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="31326-248">`msbuild`obsługuje [wzorce globbing](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="31326-248">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="31326-249">Na przykład następująca `<Content>` znacznika elementu spowoduje wykluczenie cały tekst (*.txt*) plików ze *zawartością/wwwroot* folderze i jego podfolderach.</span><span class="sxs-lookup"><span data-stu-id="31326-249">For example, the following `<Content>` element markup will exclude all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="31326-250">Kod znaczników powyżej można dodać do profilu publikowania lub *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="31326-250">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="31326-251">Po dodaniu do *.csproj* plik, reguła zostanie dodany do wszystkich profilów publikowania w projekcie.</span><span class="sxs-lookup"><span data-stu-id="31326-251">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="31326-252">Następujące `<MsDeploySkipRules>` exludes znacznika elementu wszystkie pliki z *zawartością/wwwroot* folderu:</span><span class="sxs-lookup"><span data-stu-id="31326-252">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="31326-253">`<MsDeploySkipRules>`nie spowoduje usunięcia *pominąć* elementów docelowych z witryny wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="31326-253">`<MsDeploySkipRules>`  will not delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="31326-254">`<Content>`docelowe pliki i foldery zostaną usunięte z witryny wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="31326-254">`<Content>` targeted files and folders will be deleted from the deployment site.</span></span> <span data-ttu-id="31326-255">Na przykład załóżmy, że wdrożyli aplikacji sieci web z następujących plików:</span><span class="sxs-lookup"><span data-stu-id="31326-255">For example, suppose you had deployed a web app with the following files:</span></span>

- <span data-ttu-id="31326-256">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="31326-256">*Views/Home/About1.cshtml*</span></span>
- <span data-ttu-id="31326-257">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="31326-257">*Views/Home/About2.cshtml*</span></span>
- <span data-ttu-id="31326-258">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="31326-258">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="31326-259">Jeśli dodano następujące `<MsDeploySkipRules>` znaczników, te pliki nie będzie można usunąć w witrynie wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="31326-259">If you added the following `<MsDeploySkipRules>` markup, those files would not be deleted on the deployment site.</span></span>

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

<span data-ttu-id="31326-260">`<MsDeploySkipRules>` Uniemożliwia znaczników pokazanym powyżej *pominięte* plików przed depoyed, ale zostanie nie Usuń te pliki po ich wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="31326-260">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed, but will not delete those files once they are deployed.</span></span>

<span data-ttu-id="31326-261">Następujące `<Content>` znaczników usunie pliki docelowe w witrynie wdrażania:</span><span class="sxs-lookup"><span data-stu-id="31326-261">The following `<Content>` markup would delete the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="31326-262">Za pomocą wiersza polecenia z `<Content>` znaczników powyżej spowodowałoby dane wyjściowe podobne do następującego:</span><span class="sxs-lookup"><span data-stu-id="31326-262">Using command line deployment with the `<Content>` markup above would result in output similar to the following:</span></span>

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

## <a name="including-files"></a><span data-ttu-id="31326-263">W tym pliki</span><span class="sxs-lookup"><span data-stu-id="31326-263">Including files</span></span>

<span data-ttu-id="31326-264">Następujący kod może służyć do uwzględnienia *obrazów* folderów znajdujących się poza katalogiem projektu do *wwwroot/obrazów* folderu publikowania witryny.</span><span class="sxs-lookup"><span data-stu-id="31326-264">The following markup can be used to include an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site.</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="31326-265">Znaczników można dodać do *.csproj* pliku lub profil publikowania.</span><span class="sxs-lookup"><span data-stu-id="31326-265">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="31326-266">Jeśli jest ona dodawana do *.csproj* pliku, zostanie uwzględniona w każdym profilu publikowania do projektu.</span><span class="sxs-lookup"><span data-stu-id="31326-266">If it's added to the *.csproj* file, it will be included in each publish profile in the project.</span></span>

<span data-ttu-id="31326-267">Następujące wyróżnione znaczników pokazuje sposób do:</span><span class="sxs-lookup"><span data-stu-id="31326-267">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="31326-268">Skopiuj plik z spoza projektu do *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="31326-268">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="31326-269">Wyklucz *wwwroot\Content* folderu.</span><span class="sxs-lookup"><span data-stu-id="31326-269">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="31326-270">Wyklucz *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="31326-270">Exclude *Views\Home\About2.cshtml*.</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

<span data-ttu-id="31326-271">Zobacz [WebSDK Readme](https://github.com/aspnet/websdk) dla większej liczby próbek wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="31326-271">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="31326-272">Uruchom element docelowy przed lub po opublikowaniu</span><span class="sxs-lookup"><span data-stu-id="31326-272">Run a target before or after publishing</span></span>

<span data-ttu-id="31326-273">Wbudowany `BeforePublish` i `AfterPublish` elementy docelowe może służyć do wykonywania elementu docelowego przed lub po lokalizacja docelowa publikowania.</span><span class="sxs-lookup"><span data-stu-id="31326-273">The builtin `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="31326-274">Następujący kod, mogą być dodawane do profilu publikowania do logowania wiadomości dane wyjściowe konsoli przed i po opublikowaniu:</span><span class="sxs-lookup"><span data-stu-id="31326-274">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="31326-275">Usługa Kudu</span><span class="sxs-lookup"><span data-stu-id="31326-275">The Kudu service</span></span>

<span data-ttu-id="31326-276">Aby wyświetlić pliki na aplikację sieci Web platformy Azure, użyj [usługi kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="31326-276">To view the files on your Azure Web App, use the [kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="31326-277">Dołącz `scm` token nazwę lub aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="31326-277">Append the `scm` token to the name or your Web App.</span></span> <span data-ttu-id="31326-278">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="31326-278">For example:</span></span>

| <span data-ttu-id="31326-279">Adres URL</span><span class="sxs-lookup"><span data-stu-id="31326-279">URL</span></span>               | <span data-ttu-id="31326-280">Wynik</span><span class="sxs-lookup"><span data-stu-id="31326-280">Result</span></span>|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | <span data-ttu-id="31326-281">Aplikacja sieci Web</span><span class="sxs-lookup"><span data-stu-id="31326-281">Web App</span></span> |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="31326-282">Program kudu usługi</span><span class="sxs-lookup"><span data-stu-id="31326-282">Kudu sevice</span></span> |

<span data-ttu-id="31326-283">Wybierz [konsoli debugowania](https://github.com/projectkudu/kudu/wiki/Kudu-console) elementu menu Wyświetl/Edytuj/delete/Dodaj pliki.</span><span class="sxs-lookup"><span data-stu-id="31326-283">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="31326-284">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="31326-284">Additional resources</span></span>

- <span data-ttu-id="31326-285">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) upraszcza wdrażanie aplikacji sieci Web i witryn sieci Web na serwerach usług IIS.</span><span class="sxs-lookup"><span data-stu-id="31326-285">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy)  (msdeploy) simplifies deployment of Web applications and Web sites to IIS servers.</span></span>

- <span data-ttu-id="31326-286">[https://github.com/ASPNET/websdk](https://github.com/aspnet/websdk/issues): plik problemy i żądania funkcji dla wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="31326-286">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
