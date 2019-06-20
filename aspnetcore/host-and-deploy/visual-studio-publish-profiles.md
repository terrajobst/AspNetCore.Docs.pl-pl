---
title: Program Visual Studio publikowania profile na potrzeby wdrażania aplikacji platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć profile publikowania w programie Visual Studio i używać ich do zarządzania wdrożeniami aplikacji platformy ASP.NET Core do różnych celów.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/20/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: f1711f3ee73b773cee82161668e76bcbcee55507
ms.sourcegitcommit: 3eedd6180fbbdcb81a8e1ebdbeb035bf4f2feb92
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284540"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="11394-103">Program Visual Studio publikowania profile na potrzeby wdrażania aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="11394-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="11394-104">Przez [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="11394-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="11394-105">Ten dokument koncentruje się na temat korzystania z programu Visual Studio 2017 lub nowszej, aby tworzenie i używanie profilów publikowania.</span><span class="sxs-lookup"><span data-stu-id="11394-105">This document focuses on using Visual Studio 2017 or later to create and use publish profiles.</span></span> <span data-ttu-id="11394-106">Profile publikowania utworzonych za pomocą programu Visual Studio można uruchomić z programu MSBuild i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="11394-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio.</span></span> <span data-ttu-id="11394-107">Zobacz [publikowania aplikacji sieci web ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) instrukcje dotyczące publikowania na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="11394-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="11394-108">`dotnet new mvc` Polecenie tworzy plik projektu zawierający następujący poziom główny [ \<Projekt > element](/visualstudio/msbuild/project-element-msbuild):</span><span class="sxs-lookup"><span data-stu-id="11394-108">The `dotnet new mvc` command produces a project file containing the following root-level [\<Project> element](/visualstudio/msbuild/project-element-msbuild):</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
</Project>
```

<span data-ttu-id="11394-109">Poprzedni `<Project>` elementu `Sdk` atrybut importuje MSBuild [właściwości](/visualstudio/msbuild/msbuild-properties) i [cele](/visualstudio/msbuild/msbuild-targets) z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\ SDK.props* i *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="11394-109">The preceding `<Project>` element's `Sdk` attribute imports the MSBuild [properties](/visualstudio/msbuild/msbuild-properties) and [targets](/visualstudio/msbuild/msbuild-targets) from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.props* and *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*, respectively.</span></span> <span data-ttu-id="11394-110">Domyślną lokalizacją `$(MSBuildSDKsPath)` (w programie Visual Studio 2019 Enterprise) jest *% programfiles (x86) %\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* folderu.</span><span class="sxs-lookup"><span data-stu-id="11394-110">The default location for `$(MSBuildSDKsPath)` (with Visual Studio 2019 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="11394-111">`Microsoft.NET.Sdk.Web` (Zestaw SDK sieci web) jest zależna od innych zestawów SDK, w tym `Microsoft.NET.Sdk` (.NET Core SDK) i `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)).</span><span class="sxs-lookup"><span data-stu-id="11394-111">`Microsoft.NET.Sdk.Web` (Web SDK) depends on other SDKs, including `Microsoft.NET.Sdk` (.NET Core SDK) and `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)).</span></span> <span data-ttu-id="11394-112">Właściwości programu MSBuild i elementy docelowe skojarzone z każdym zależnego zestawu SDK są importowane.</span><span class="sxs-lookup"><span data-stu-id="11394-112">The MSBuild properties and targets associated with each dependent SDK are imported.</span></span> <span data-ttu-id="11394-113">Opublikuj Importuj obiekty docelowe odpowiedni zestaw elementów docelowych na podstawie metody publikowania użytej.</span><span class="sxs-lookup"><span data-stu-id="11394-113">Publish targets import the appropriate set of targets based on the publish method used.</span></span>

<span data-ttu-id="11394-114">Podczas ładowania projektu programu MSBuild lub Visual Studio są wykonywane następujące akcje wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="11394-114">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="11394-115">Tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="11394-115">Build project</span></span>
* <span data-ttu-id="11394-116">Obliczenia plików do opublikowania</span><span class="sxs-lookup"><span data-stu-id="11394-116">Compute files to publish</span></span>
* <span data-ttu-id="11394-117">Publikowanie plików do lokalizacji docelowej</span><span class="sxs-lookup"><span data-stu-id="11394-117">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="11394-118">Obliczeniowe elementy projektu</span><span class="sxs-lookup"><span data-stu-id="11394-118">Compute project items</span></span>

<span data-ttu-id="11394-119">Gdy projekt jest ładowany, [elementów projektu programu MSBuild](/visualstudio/msbuild/common-msbuild-project-items) (pliki) są obliczane.</span><span class="sxs-lookup"><span data-stu-id="11394-119">When the project is loaded, the [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) (files) are computed.</span></span> <span data-ttu-id="11394-120">Typ elementu określa sposób przetwarzania pliku.</span><span class="sxs-lookup"><span data-stu-id="11394-120">The item type determines how the file is processed.</span></span> <span data-ttu-id="11394-121">Domyślnie *.cs* pliki są uwzględnione w `Compile` listy elementów.</span><span class="sxs-lookup"><span data-stu-id="11394-121">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="11394-122">Pliki `Compile` listy elementów są kompilowane.</span><span class="sxs-lookup"><span data-stu-id="11394-122">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="11394-123">`Content` Listy elementów zawiera pliki, które są publikowane oprócz dane wyjściowe kompilacji.</span><span class="sxs-lookup"><span data-stu-id="11394-123">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="11394-124">Domyślnie pliki pasujących do wzorców `wwwroot\**`, `**\*.config`, i `**\*.json` znajdują się w `Content` listy elementów.</span><span class="sxs-lookup"><span data-stu-id="11394-124">By default, files matching the patterns `wwwroot\**`, `**\*.config`, and `**\*.json` are included in the `Content` item list.</span></span> <span data-ttu-id="11394-125">Na przykład `wwwroot\**` [wzoru symboli wieloznacznych](https://gruntjs.com/configuring-tasks#globbing-patterns) uwzględnia wszystkie pliki w *wwwroot* folderu **i** jego podfolderów.</span><span class="sxs-lookup"><span data-stu-id="11394-125">For example, the `wwwroot\**` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** its subfolders.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="11394-126">Zestaw SDK sieci Web importuje [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="11394-126">The Web SDK imports the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="11394-127">W rezultacie plików pasujących do wzorców `**\*.cshtml` i `**\*.razor` zostaną również uwzględnione w `Content` listy elementów.</span><span class="sxs-lookup"><span data-stu-id="11394-127">As a result, files matching the patterns `**\*.cshtml` and `**\*.razor` are also included in the `Content` item list.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="11394-128">Zestaw SDK sieci Web importuje [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="11394-128">The Web SDK imports the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="11394-129">W rezultacie pliki pasujące `**\*.cshtml` wzorzec znajdują się również w `Content` listy elementów.</span><span class="sxs-lookup"><span data-stu-id="11394-129">As a result, files matching the `**\*.cshtml` pattern are also included in the `Content` item list.</span></span>

::: moniker-end

<span data-ttu-id="11394-130">Aby jawnie dodać plik do listy publikowania, należy dodać bezpośrednio w pliku *.csproj* pliku, jak pokazano na [pliki dołączane](#include-files) sekcji.</span><span class="sxs-lookup"><span data-stu-id="11394-130">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in the [Include Files](#include-files) section.</span></span>

<span data-ttu-id="11394-131">Podczas wybierania **Publikuj** przycisku w programie Visual Studio lub podczas publikowania z poziomu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="11394-131">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="11394-132">Elementy/właściwości są obliczane (pliki, które są potrzebne do tworzenia).</span><span class="sxs-lookup"><span data-stu-id="11394-132">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="11394-133">**Sam program Visual Studio**: Pakiety NuGet są przywracane.</span><span class="sxs-lookup"><span data-stu-id="11394-133">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="11394-134">(Przywróć musi być jawne przez użytkownika na interfejs wiersza polecenia).</span><span class="sxs-lookup"><span data-stu-id="11394-134">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="11394-135">Projekt zostanie skompilowany.</span><span class="sxs-lookup"><span data-stu-id="11394-135">The project builds.</span></span>
* <span data-ttu-id="11394-136">Publikuj elementy są obliczane (pliki, które są niezbędne do publikowania).</span><span class="sxs-lookup"><span data-stu-id="11394-136">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="11394-137">Projekt zostanie opublikowany (obliczanej pliki są kopiowane do lokalizacji docelowej publikowania).</span><span class="sxs-lookup"><span data-stu-id="11394-137">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="11394-138">Gdy odwołuje się do projektu programu ASP.NET Core `Microsoft.NET.Sdk.Web` w pliku projektu *app_offline.htm* plik zostanie umieszczony w folderze głównym katalogu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="11394-138">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="11394-139">Gdy plik jest obecny, modułu ASP.NET Core bez problemu zmieniała zamyka aplikację i służy *app_offline.htm* pliku podczas wdrażania.</span><span class="sxs-lookup"><span data-stu-id="11394-139">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="11394-140">Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="11394-140">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="11394-141">Podstawowe publikowanie wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="11394-141">Basic command-line publishing</span></span>

<span data-ttu-id="11394-142">Publikowanie wiersza polecenia działa na wszystkich platformach .NET Core, obsługiwane i nie wymaga programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="11394-142">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="11394-143">W następujących przykładach [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie jest wykonywane z katalogu projektu (który zawiera *.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="11394-143">In the following samples, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="11394-144">W przeciwnym razie w folderze projektu jawne przekazywanie w ścieżce do pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="11394-144">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="11394-145">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="11394-145">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="11394-146">Uruchom następujące polecenia, aby tworzyć i publikować aplikację sieci web:</span><span class="sxs-lookup"><span data-stu-id="11394-146">Run the following commands to create and publish a web app:</span></span>

```console
dotnet new mvc
dotnet publish
```

<span data-ttu-id="11394-147">[Publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie generuje odmianą następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="11394-147">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces a variation of the following output:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version {version} for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 36.81 ms for C:\Webs\Web1\Web1.csproj.
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\Web1.Views.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\publish\
```

<span data-ttu-id="11394-148">Wartość domyślna publikowanie folderu jest `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="11394-148">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="11394-149">Wartość domyślna dla `$(Configuration)` jest *debugowania*.</span><span class="sxs-lookup"><span data-stu-id="11394-149">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="11394-150">W poprzednim przykładzie `<TargetFramework>` jest `netcoreapp{X.Y}`.</span><span class="sxs-lookup"><span data-stu-id="11394-150">In the preceding sample, the `<TargetFramework>` is `netcoreapp{X.Y}`.</span></span>

<span data-ttu-id="11394-151">`dotnet publish -h` Wyświetla Pomoc do opublikowania.</span><span class="sxs-lookup"><span data-stu-id="11394-151">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="11394-152">Poniższe polecenie Określa `Release` kompilowanie i publikowanie katalogu:</span><span class="sxs-lookup"><span data-stu-id="11394-152">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="11394-153">[Publikowania dotnet](/dotnet/core/tools/dotnet-publish) wywołania programu MSBuild, które wywołuje polecenie `Publish` docelowej.</span><span class="sxs-lookup"><span data-stu-id="11394-153">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="11394-154">Wszelkie parametry przekazywane do `dotnet publish` są przekazywane do MSBuild.</span><span class="sxs-lookup"><span data-stu-id="11394-154">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="11394-155">`-c` Mapuje parametr `Configuration` właściwości programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="11394-155">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="11394-156">`-o` Mapuje parametru `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="11394-156">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="11394-157">Właściwości programu MSBuild może być przekazywany przy użyciu jednej z następujących formatów:</span><span class="sxs-lookup"><span data-stu-id="11394-157">MSBuild properties can be passed using either of the following formats:</span></span>

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="11394-158">Następujące polecenie publikuje `Release` kompilacja — przejście do udziału sieciowego:</span><span class="sxs-lookup"><span data-stu-id="11394-158">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="11394-159">Udział sieciowy jest określony za pomocą ukośników ( *//r8/* ) i działa na wszystkich platformach .NET Core, obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="11394-159">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="11394-160">Upewnij się, że opublikowanej aplikacji do wdrożenia nie jest uruchomione.</span><span class="sxs-lookup"><span data-stu-id="11394-160">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="11394-161">Pliki *publikowania* folderu są zablokowane, gdy aplikacja jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="11394-161">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="11394-162">Wdrożenie nie może wystąpić, ponieważ zablokowany, nie można skopiować plików.</span><span class="sxs-lookup"><span data-stu-id="11394-162">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="11394-163">Profile publikowania</span><span class="sxs-lookup"><span data-stu-id="11394-163">Publish profiles</span></span>

<span data-ttu-id="11394-164">Ta sekcja używa programu Visual Studio 2017 r. lub nowszej, aby utworzyć profil publikowania.</span><span class="sxs-lookup"><span data-stu-id="11394-164">This section uses Visual Studio 2017 or later to create a publishing profile.</span></span> <span data-ttu-id="11394-165">Po utworzeniu profilu publikowania z wiersza polecenia lub programu Visual Studio jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="11394-165">Once the profile is created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="11394-166">Publikowanie profilów można uprościć proces publikowania, a może znajdować się dowolna liczba profilów.</span><span class="sxs-lookup"><span data-stu-id="11394-166">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="11394-167">Tworzenie profilu publikowania w programie Visual Studio, wybierając jedną z następujących ścieżek:</span><span class="sxs-lookup"><span data-stu-id="11394-167">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="11394-168">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="11394-168">Right-click the project in **Solution Explorer** and select **Publish**.</span></span>
* <span data-ttu-id="11394-169">Wybierz **publikowania {Nazwa projektu}** z **kompilacji** menu.</span><span class="sxs-lookup"><span data-stu-id="11394-169">Select **Publish {PROJECT NAME}** from the **Build** menu.</span></span>

<span data-ttu-id="11394-170">**Publikuj** jest wyświetlana na karcie Strona możliwości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="11394-170">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="11394-171">Jeśli projekt nie ma profil publikowania, zostanie wyświetlona następująca strona:</span><span class="sxs-lookup"><span data-stu-id="11394-171">If the project lacks a publish profile, the following page is displayed:</span></span>

![Na karcie publikowanie strony pojemności aplikacji](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="11394-173">Gdy **folderu** jest zaznaczone, określ ścieżkę folderu do przechowywania zasobów opublikowanych.</span><span class="sxs-lookup"><span data-stu-id="11394-173">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="11394-174">Domyślnym folderem jest *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="11394-174">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="11394-175">Kliknij przycisk **Utwórz profil** przycisk, aby zakończyć.</span><span class="sxs-lookup"><span data-stu-id="11394-175">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="11394-176">Po utworzeniu profilu publikowania **Publikuj** karcie zmiany.</span><span class="sxs-lookup"><span data-stu-id="11394-176">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="11394-177">Nowo utworzony profil zostanie wyświetlony na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="11394-177">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="11394-178">Kliknij przycisk **Utwórz nowy profil** do Utwórz inny nowy profil.</span><span class="sxs-lookup"><span data-stu-id="11394-178">Click **Create new profile** to create another new profile.</span></span>

![Na karcie publikowanie strony możliwości aplikacji, przedstawiający FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="11394-180">Kreator publikowania obsługuje następujące cele publikowania:</span><span class="sxs-lookup"><span data-stu-id="11394-180">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="11394-181">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="11394-181">Azure App Service</span></span>
* <span data-ttu-id="11394-182">Usługa Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="11394-182">Azure Virtual Machines</span></span>
* <span data-ttu-id="11394-183">Usługi IIS, FTP itp., (na dowolnym serwerze sieci web)</span><span class="sxs-lookup"><span data-stu-id="11394-183">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="11394-184">Folder</span><span class="sxs-lookup"><span data-stu-id="11394-184">Folder</span></span>
* <span data-ttu-id="11394-185">Importuj profil</span><span class="sxs-lookup"><span data-stu-id="11394-185">Import Profile</span></span>

<span data-ttu-id="11394-186">Aby uzyskać więcej informacji, zobacz [jakie opcje publikowania są dla mnie odpowiednia](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="11394-186">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="11394-187">Podczas tworzenia profilu publikowania za pomocą programu Visual Studio *PublishProfiles/właściwości / {Nazwa profilu} .pubxml* zostanie utworzony plik MSBuild.</span><span class="sxs-lookup"><span data-stu-id="11394-187">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/{PROFILE NAME}.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="11394-188">*.Pubxml* plik jest plikiem programu MSBuild i zawiera ustawienia konfiguracji publikowania.</span><span class="sxs-lookup"><span data-stu-id="11394-188">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="11394-189">Ten plik można zmienić dostosowywania kompilacji i publikowania procesu.</span><span class="sxs-lookup"><span data-stu-id="11394-189">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="11394-190">Ten plik jest odczytywany przez proces publikowania.</span><span class="sxs-lookup"><span data-stu-id="11394-190">This file is read by the publishing process.</span></span> <span data-ttu-id="11394-191">`<LastUsedBuildConfiguration>` to specjalne, ponieważ jest właściwością globalną, a nie powinien być w dowolnym pliku, który jest importowany w kompilacji.</span><span class="sxs-lookup"><span data-stu-id="11394-191">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="11394-192">Zobacz [MSBuild: sposób ustawiania właściwości konfiguracji](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="11394-192">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="11394-193">Podczas publikowania do obiektu docelowego platformy Azure, *.pubxml* plik zawiera identyfikator swojej subskrypcji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="11394-193">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="11394-194">Z tym typem docelowym dodając ten plik do kontroli źródła nie jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="11394-194">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="11394-195">Podczas publikowania w celu spoza platformy Azure, jest bezpieczne do zaewidencjonowania *.pubxml* pliku.</span><span class="sxs-lookup"><span data-stu-id="11394-195">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="11394-196">Informacje poufne (na przykład hasło publikowania) są szyfrowane na komputerze na poziomie użytkownika/komputera.</span><span class="sxs-lookup"><span data-stu-id="11394-196">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="11394-197">Jest on przechowywany w *PublishProfiles/właściwości / {Nazwa profilu}.pubxml.user* pliku.</span><span class="sxs-lookup"><span data-stu-id="11394-197">It's stored in the *Properties/PublishProfiles/{PROFILE NAME}.pubxml.user* file.</span></span> <span data-ttu-id="11394-198">Ponieważ ten plik można przechowywać poufne informacje, nie powinny zostać sprawdzone w kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="11394-198">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="11394-199">Aby uzyskać omówienie sposobu publikowania aplikacji sieci web programu ASP.NET Core, zobacz [hosta i wdrażanie](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="11394-199">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="11394-200">Zadania programu MSBuild i elementy docelowe niezbędne do opublikowania aplikacji ASP.NET Core są typu open source w [repozytorium aspnet/websdk](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="11394-200">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at the [aspnet/websdk repository](https://github.com/aspnet/websdk).</span></span>

<span data-ttu-id="11394-201">`dotnet publish` można użyć folderu, MSDeploy, i [Kudu](https://github.com/projectkudu/kudu/wiki) profilów publikowania:</span><span class="sxs-lookup"><span data-stu-id="11394-201">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="11394-202">Folder (działa dla wielu platform):</span><span class="sxs-lookup"><span data-stu-id="11394-202">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="11394-203">Program MSDeploy (obecnie ta działa tylko w Windows, ponieważ MSDeploy nie jest dla wielu platform):</span><span class="sxs-lookup"><span data-stu-id="11394-203">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="11394-204">Pakiet narzędzia MSDeploy (obecnie ta działa tylko w Windows, ponieważ MSDeploy nie jest dla wielu platform):</span><span class="sxs-lookup"><span data-stu-id="11394-204">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="11394-205">W powyższych przykładach nie przekazuj `deployonbuild` do `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="11394-205">In the preceding examples, don't pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="11394-206">Aby uzyskać więcej informacji, zobacz [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="11394-206">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="11394-207">`dotnet publish` obsługuje Kudu interfejsy API w celu publikowania na platformie Azure z dowolnej platformy.</span><span class="sxs-lookup"><span data-stu-id="11394-207">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="11394-208">Program Visual Studio publikuje obsługuje interfejsy API programu Kudu, ale jest obsługiwany przez WebSDK dla wielu platform, opublikuj na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="11394-208">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="11394-209">Dodaj profil publikowania *właściwości/PublishProfiles* folderu o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="11394-209">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="11394-210">Uruchom następujące polecenie, aby zip zapasową zawartości publikowania i opublikować go na platformie Azure przy użyciu interfejsów API Kudu:</span><span class="sxs-lookup"><span data-stu-id="11394-210">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="11394-211">Ustaw następujące właściwości programu MSBuild, korzystając z profilu publikowania:</span><span class="sxs-lookup"><span data-stu-id="11394-211">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

<span data-ttu-id="11394-212">Podczas publikowania za pomocą profilu o nazwie *FolderProfile*, można wykonać jedną z poniższych poleceń:</span><span class="sxs-lookup"><span data-stu-id="11394-212">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="11394-213">Podczas wywoływania [kompilacji dotnet](/dotnet/core/tools/dotnet-build), wywoływanych przez nią `msbuild` do uruchamiania kompilacji i procesu publikowania.</span><span class="sxs-lookup"><span data-stu-id="11394-213">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="11394-214">Wywołanie dowolnej `dotnet build` lub `msbuild` odpowiada podczas przekazywania w folderze profilu.</span><span class="sxs-lookup"><span data-stu-id="11394-214">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="11394-215">Podczas wywoływania MSBuild bezpośrednio na Windows, .NET Framework w wersji programu MSBuild jest używany.</span><span class="sxs-lookup"><span data-stu-id="11394-215">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="11394-216">Program MSDeploy jest obecnie ograniczona do Windows maszyny na potrzeby publikowania.</span><span class="sxs-lookup"><span data-stu-id="11394-216">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="11394-217">Wywoływanie `dotnet build` bez folderu profilu wywołuje MSBuild i program MSBuild używa MSDeploy na profile i folderów.</span><span class="sxs-lookup"><span data-stu-id="11394-217">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="11394-218">Wywoływanie `dotnet build` profil i folderów wywołuje MSBuild (przy użyciu narzędzia MSDeploy) i powoduje błąd (nawet wtedy, gdy uruchomiona na platformie Windows).</span><span class="sxs-lookup"><span data-stu-id="11394-218">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="11394-219">Aby opublikować za pomocą profilu i folderów, należy wywołać program MSBuild bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="11394-219">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="11394-220">Profil publikowania następujący folder został utworzony za pomocą programu Visual Studio i publikuje do udziału sieciowego:</span><span class="sxs-lookup"><span data-stu-id="11394-220">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="11394-221">W powyższym przykładzie `<LastUsedBuildConfiguration>` ustawiono `Release`.</span><span class="sxs-lookup"><span data-stu-id="11394-221">In the preceding example, `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="11394-222">Podczas publikowania z programu Visual Studio `<LastUsedBuildConfiguration>` ustawiono wartość właściwości konfiguracji, przy użyciu wartości, gdy proces publikowania zostanie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="11394-222">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="11394-223">`<LastUsedBuildConfiguration>` Właściwość konfiguracji jest szczególna i nie powinna zostać zastąpiona w importowanym pliku MSBuild.</span><span class="sxs-lookup"><span data-stu-id="11394-223">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="11394-224">Ta właściwość może być zastąpiona w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="11394-224">This property can be overridden from the command line.</span></span>

<span data-ttu-id="11394-225">Korzystanie z platformy .NET Core interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="11394-225">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="11394-226">Korzystanie z programu MSBuild:</span><span class="sxs-lookup"><span data-stu-id="11394-226">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="11394-227">Publikowanie do punktu końcowego MSDeploy z wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="11394-227">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="11394-228">W poniższym przykładzie użyto aplikacji sieci web ASP.NET Core utworzona przez program Visual Studio o nazwie *AzureWebApp*.</span><span class="sxs-lookup"><span data-stu-id="11394-228">The following example uses an ASP.NET Core web app created by Visual Studio named *AzureWebApp*.</span></span> <span data-ttu-id="11394-229">Profil publikowania aplikacji platformy Azure jest dodawany z programem Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="11394-229">An Azure Apps publish profile is added with Visual Studio.</span></span> <span data-ttu-id="11394-230">Aby uzyskać więcej informacji na temat sposobu tworzenia profilu, zobacz [profilów publikowania](#publish-profiles) sekcji.</span><span class="sxs-lookup"><span data-stu-id="11394-230">For more information on how to create a profile, see the [Publish profiles](#publish-profiles) section.</span></span>

<span data-ttu-id="11394-231">Aby wdrożyć aplikację za pomocą profilu publikowania, wykonaj `msbuild` polecenia z programu Visual Studio **wiersz polecenia dla deweloperów**.</span><span class="sxs-lookup"><span data-stu-id="11394-231">To deploy the app using a publish profile, execute the `msbuild` command from a Visual Studio **Developer Command Prompt**.</span></span> <span data-ttu-id="11394-232">Wiersz polecenia jest dostępna w *programu Visual Studio* folderu **Start** menu na pasku zadań Windows.</span><span class="sxs-lookup"><span data-stu-id="11394-232">The command prompt is available in the *Visual Studio* folder of the **Start** menu on the Windows taskbar.</span></span> <span data-ttu-id="11394-233">W celu ułatwienia dostępu, można dodać wiersza polecenia, aby **narzędzia** menu w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="11394-233">For easier access, you can add the command prompt to the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="11394-234">Aby uzyskać więcej informacji, zobacz [wiersz polecenia programisty dla programu Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="11394-234">For more information, see [Developer Command Prompt for Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span></span>

<span data-ttu-id="11394-235">Program MSBuild używa następującej składni polecenia:</span><span class="sxs-lookup"><span data-stu-id="11394-235">MSBuild uses the following command syntax:</span></span>

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* <span data-ttu-id="11394-236">{PATH} &ndash; Ścieżka do pliku projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="11394-236">{PATH} &ndash; Path to the app's project file.</span></span>
* <span data-ttu-id="11394-237">{PROFIL} &ndash; Nazwa profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="11394-237">{PROFILE} &ndash; Name of the publish profile.</span></span>
* <span data-ttu-id="11394-238">UŻYTKOWNIK {USERNAME} &ndash; MSDeploy username.</span><span class="sxs-lookup"><span data-stu-id="11394-238">{USERNAME} &ndash; MSDeploy username.</span></span> <span data-ttu-id="11394-239">{USERNAME} można znaleźć w profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="11394-239">The {USERNAME} can be found in the publish profile.</span></span>
* <span data-ttu-id="11394-240">{PASSWORD} &ndash; MSDeploy hasła.</span><span class="sxs-lookup"><span data-stu-id="11394-240">{PASSWORD} &ndash; MSDeploy password.</span></span> <span data-ttu-id="11394-241">Uzyskaj {PASSWORD} z *{profil}. PublishSettings* pliku.</span><span class="sxs-lookup"><span data-stu-id="11394-241">Obtain the {PASSWORD} from the *{PROFILE}.PublishSettings* file.</span></span> <span data-ttu-id="11394-242">Pobierz *. PublishSettings* plików z poziomu:</span><span class="sxs-lookup"><span data-stu-id="11394-242">Download the *.PublishSettings* file from either:</span></span>
  * <span data-ttu-id="11394-243">Eksplorator rozwiązań: Wybierz **widoku** > **Eksplorator chmury**.</span><span class="sxs-lookup"><span data-stu-id="11394-243">Solution Explorer: Select **View** > **Cloud Explorer**.</span></span> <span data-ttu-id="11394-244">Połącz z subskrypcją platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="11394-244">Connect with your Azure subscription.</span></span> <span data-ttu-id="11394-245">Otwórz **usług App Services**.</span><span class="sxs-lookup"><span data-stu-id="11394-245">Open **App Services**.</span></span> <span data-ttu-id="11394-246">Kliknij prawym przyciskiem myszy aplikację.</span><span class="sxs-lookup"><span data-stu-id="11394-246">Right-click the app.</span></span> <span data-ttu-id="11394-247">Wybierz **Pobierz profil publikowania**.</span><span class="sxs-lookup"><span data-stu-id="11394-247">Select **Download Publish Profile**.</span></span>
  * <span data-ttu-id="11394-248">Witryna Azure portal: Wybierz **Pobierz profil publikowania** w aplikacji sieci web **Przegląd** panelu.</span><span class="sxs-lookup"><span data-stu-id="11394-248">Azure portal: Select **Get publish profile** in the web app's **Overview** panel.</span></span>

<span data-ttu-id="11394-249">W poniższym przykładzie użyto profil publikowania o nazwie *AzureWebApp — narzędzie Web Deploy*:</span><span class="sxs-lookup"><span data-stu-id="11394-249">The following example uses a publish profile named *AzureWebApp - Web Deploy*:</span></span>

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

<span data-ttu-id="11394-250">Profil publikowania można również za pomocą interfejsu wiersza polecenia platformy .NET Core [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) polecenie w wierszu polecenia Windows:</span><span class="sxs-lookup"><span data-stu-id="11394-250">A publish profile can also be used with the .NET Core CLI [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command from a Windows command prompt:</span></span>

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!NOTE]
> <span data-ttu-id="11394-251">[Dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) polecenie jest dostępne dla wielu platform i można kompilować aplikacje platformy ASP.NET Core w systemach macOS i Linux.</span><span class="sxs-lookup"><span data-stu-id="11394-251">The [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command is available cross-platform and can compile ASP.NET Core apps on macOS and Linux.</span></span> <span data-ttu-id="11394-252">Jednak program MSBuild w systemie macOS i Linux nie jest zdolny do wdrażania aplikacji na platformie Azure lub innych punktów końcowych MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="11394-252">However, MSBuild on macOS and Linux isn't capable of deploying an app to Azure or other MSDeploy endpoint.</span></span> <span data-ttu-id="11394-253">Program MSDeploy jest dostępna tylko na Windows.</span><span class="sxs-lookup"><span data-stu-id="11394-253">MSDeploy is only available on Windows.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="11394-254">Ustaw środowisko</span><span class="sxs-lookup"><span data-stu-id="11394-254">Set the environment</span></span>

<span data-ttu-id="11394-255">Obejmują `<EnvironmentName>` właściwości w profilu publikowania ( *.pubxml*) lub plik projektu do zestawu aplikacji [środowiska](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="11394-255">Include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file to set the app's [environment](xref:fundamentals/environments):</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="11394-256">Jeśli potrzebujesz *web.config* przekształcenia (na przykład ustawienie zmiennych środowiskowych na podstawie konfiguracji, profilu lub środowiska), zobacz <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="11394-256">If you require *web.config* transformations (for example, setting environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="exclude-files"></a><span data-ttu-id="11394-257">Wyklucz pliki</span><span class="sxs-lookup"><span data-stu-id="11394-257">Exclude files</span></span>

<span data-ttu-id="11394-258">Podczas publikowania aplikacji sieci web platformy ASP.NET Core, uwzględnione są następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="11394-258">When publishing ASP.NET Core web apps, the following assets are included:</span></span>

* <span data-ttu-id="11394-259">Artefakty kompilacji</span><span class="sxs-lookup"><span data-stu-id="11394-259">Build artifacts</span></span>
* <span data-ttu-id="11394-260">Foldery i pliki zgodne z następujących wzorców obsługi symboli wieloznacznych:</span><span class="sxs-lookup"><span data-stu-id="11394-260">Folders and files matching the following globbing patterns:</span></span>
  * <span data-ttu-id="11394-261">`**\*.config` (na przykład *web.config*)</span><span class="sxs-lookup"><span data-stu-id="11394-261">`**\*.config` (for example, *web.config*)</span></span>
  * <span data-ttu-id="11394-262">`**\*.json` (na przykład *appsettings.json*)</span><span class="sxs-lookup"><span data-stu-id="11394-262">`**\*.json` (for example, *appsettings.json*)</span></span>
  * `wwwroot\**`

<span data-ttu-id="11394-263">Obsługuje program MSBuild [wzorców obsługi symboli wieloznacznych](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="11394-263">MSBuild supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="11394-264">Na przykład następująca `<Content>` element pomija kopiowanie tekstu ( *.txt*) pliki *wwwroot\content* folderze i jego podfolderach:</span><span class="sxs-lookup"><span data-stu-id="11394-264">For example, the following `<Content>` element suppresses the copying of text (*.txt*) files in the *wwwroot\content* folder and its subfolders:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="11394-265">Poprzedni kod znaczników, które można dodać do profilu publikowania lub *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="11394-265">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="11394-266">Po dodaniu do *.csproj* plik, reguła zostanie dodany do wszystkich profilów publikowania w projekcie.</span><span class="sxs-lookup"><span data-stu-id="11394-266">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="11394-267">Następujące `<MsDeploySkipRules>` element wyklucza wszystkie pliki z *wwwroot\content* folderu:</span><span class="sxs-lookup"><span data-stu-id="11394-267">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot\content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="11394-268">`<MsDeploySkipRules>` nie usuwa *pominąć* obiekty docelowe z witryny wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="11394-268">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="11394-269">`<Content>` docelowe pliki i foldery są usuwane z lokacji wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="11394-269">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="11394-270">Na przykład załóżmy, że wdrożonej aplikacji sieci web ma następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="11394-270">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="11394-271">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="11394-271">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="11394-272">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="11394-272">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="11394-273">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="11394-273">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="11394-274">Jeśli następujące `<MsDeploySkipRules>` elementy są dodawane, te pliki nie zostaną usunięte w witrynie wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="11394-274">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="11394-275">Poprzedni `<MsDeploySkipRules>` elementy zapobiegania *pominięte* pliki z wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="11394-275">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="11394-276">Nie będzie go usunąć te pliki, gdy są one wdrażane.</span><span class="sxs-lookup"><span data-stu-id="11394-276">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="11394-277">Następujące `<Content>` elementu usuwa pliki docelowe lokacji wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="11394-277">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="11394-278">Za pomocą wiersza polecenia z poprzednim okresem `<Content>` element daje następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="11394-278">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

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

## <a name="include-files"></a><span data-ttu-id="11394-279">Dołącz pliki</span><span class="sxs-lookup"><span data-stu-id="11394-279">Include files</span></span>

<span data-ttu-id="11394-280">Następujące sekcje konspektu podejścia do dołączania plików w czasie publikacji.</span><span class="sxs-lookup"><span data-stu-id="11394-280">The following sections outline different approaches for file inclusion at publish time.</span></span> <span data-ttu-id="11394-281">[Dołączania plików ogólnego](#general-file-inclusion) sekcji używa `DotNetPublishFiles` elementu, który jest dostarczany przez plik elementów docelowych publikowania w zestawie SDK sieci Web.</span><span class="sxs-lookup"><span data-stu-id="11394-281">The [General file inclusion](#general-file-inclusion) section uses the `DotNetPublishFiles` item, which is provided by a publish targets file in the Web SDK.</span></span> <span data-ttu-id="11394-282">[Dołączania plików selektywne](#selective-file-inclusion) sekcji używa `ResolvedFileToPublish` elementu, który jest dostarczany przez plik elementów docelowych publikowania w zestawie SDK programu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="11394-282">The [Selective file inclusion](#selective-file-inclusion) section uses the `ResolvedFileToPublish` item, which is provided by a publish targets file in the .NET Core SDK.</span></span> <span data-ttu-id="11394-283">Ponieważ zestaw SDK sieci Web jest zależny od zestawu .NET Core SDK, albo element może służyć w projektach programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="11394-283">Because the Web SDK depends on the .NET Core SDK, either item can be used in an ASP.NET Core project.</span></span> 

### <a name="general-file-inclusion"></a><span data-ttu-id="11394-284">Włączenie pliku ogólny</span><span class="sxs-lookup"><span data-stu-id="11394-284">General file inclusion</span></span>

<span data-ttu-id="11394-285">Poniższy przykład `<ItemGroup>` element pokazuje kopiowania folderu znajduje się poza katalogiem projektu do folderu opublikowanej witryny.</span><span class="sxs-lookup"><span data-stu-id="11394-285">The following example's `<ItemGroup>` element demonstrates copying a folder located outside of the project directory to a folder of the published site.</span></span> <span data-ttu-id="11394-286">Wszelkie pliki dodane do niej następujące znaczniki `<ItemGroup>` są domyślnie dołączone.</span><span class="sxs-lookup"><span data-stu-id="11394-286">Any files added to the following markup's `<ItemGroup>` are included by default.</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotNetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotNetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="11394-287">Poprzedni kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="11394-287">The preceding markup:</span></span>

* <span data-ttu-id="11394-288">Mogą być dodawane do *.csproj* pliku lub profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="11394-288">Can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="11394-289">Jeśli jest dodawany do *.csproj* pliku, jest on zawarty w każdym profilu publikowania w projekcie.</span><span class="sxs-lookup"><span data-stu-id="11394-289">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>
* <span data-ttu-id="11394-290">Deklaruje `_CustomFiles` elementu do przechowywania plików pasujących `Include` wzoru symboli wieloznacznych atrybutu.</span><span class="sxs-lookup"><span data-stu-id="11394-290">Declares a `_CustomFiles` item to store files matching the `Include` attribute's globbing pattern.</span></span> <span data-ttu-id="11394-291">*Obrazów* folder, do którego odwołuje się do wzorca znajduje się poza katalogiem projektu.</span><span class="sxs-lookup"><span data-stu-id="11394-291">The *images* folder referenced in the pattern is located outside of the project directory.</span></span> <span data-ttu-id="11394-292">A [zastrzeżone właściwość](/visualstudio/msbuild/msbuild-reserved-and-well-known-properties)o nazwie `$(MSBuildProjectDirectory)`, jest rozpoznawana jako ścieżka bezwzględna do pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="11394-292">A [reserved property](/visualstudio/msbuild/msbuild-reserved-and-well-known-properties), named `$(MSBuildProjectDirectory)`, resolves to the project file's absolute path.</span></span>
* <span data-ttu-id="11394-293">Zawiera listę plików do `DotNetPublishFiles` elementu.</span><span class="sxs-lookup"><span data-stu-id="11394-293">Provides a list of files to the `DotNetPublishFiles` item.</span></span> <span data-ttu-id="11394-294">Domyślnie element firmy `<DestinationRelativePath>` element jest pusty.</span><span class="sxs-lookup"><span data-stu-id="11394-294">By default, the item's `<DestinationRelativePath>` element is empty.</span></span> <span data-ttu-id="11394-295">Wartością domyślną jest zastępowany w znaczniku i używa [metadane dobrze znanego elementu](/visualstudio/msbuild/msbuild-well-known-item-metadata) takich jak `%(RecursiveDir)`.</span><span class="sxs-lookup"><span data-stu-id="11394-295">The default value is overridden in the markup and uses [well-known item metadata](/visualstudio/msbuild/msbuild-well-known-item-metadata) such as `%(RecursiveDir)`.</span></span> <span data-ttu-id="11394-296">Reprezentuje tekst wewnętrzny *wwwroot/obrazów* folderu opublikowanej witryny.</span><span class="sxs-lookup"><span data-stu-id="11394-296">The inner text represents the *wwwroot/images* folder of the published site.</span></span>

### <a name="selective-file-inclusion"></a><span data-ttu-id="11394-297">Włączenie pliku selektywnego</span><span class="sxs-lookup"><span data-stu-id="11394-297">Selective file inclusion</span></span>

<span data-ttu-id="11394-298">Przedstawia podświetlony znaczniki w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="11394-298">The highlighted markup in the following example demonstrates:</span></span>

* <span data-ttu-id="11394-299">Kopiowanie pliku znajdującego się poza projektem w opublikowanej witryny *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="11394-299">Copying a file located outside of the project into the published site's *wwwroot* folder.</span></span> <span data-ttu-id="11394-300">Nazwa pliku *ReadMe2.md* jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="11394-300">The file name of *ReadMe2.md* is maintained.</span></span>
* <span data-ttu-id="11394-301">Z wyjątkiem *wwwroot\Content* folderu.</span><span class="sxs-lookup"><span data-stu-id="11394-301">Excluding the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="11394-302">Z wyjątkiem *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="11394-302">Excluding *Views\Home\About2.cshtml*.</span></span>

[!code-xml[](visual-studio-publish-profiles/samples/Web1.pubxml?highlight=18-23)]

<span data-ttu-id="11394-303">W poprzednim przykładzie użyto `ResolvedFileToPublish` elementu, którego domyślne zachowanie to zawsze skopiuj pliki udostępniane w `Include` atrybutu do opublikowanej witryny.</span><span class="sxs-lookup"><span data-stu-id="11394-303">The preceding example uses the `ResolvedFileToPublish` item, whose default behavior is to always copy the files provided in the `Include` attribute to the published site.</span></span> <span data-ttu-id="11394-304">Zastąpić domyślne zachowanie, umieszczając `<CopyToPublishDirectory>` elementu podrzędnego z tekstu wewnętrznego albo `Never` lub `PreserveNewest`.</span><span class="sxs-lookup"><span data-stu-id="11394-304">Override the default behavior by including a `<CopyToPublishDirectory>` child element with inner text of either `Never` or `PreserveNewest`.</span></span> <span data-ttu-id="11394-305">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="11394-305">For example:</span></span>

```xml
<ResolvedFileToPublish Include="..\ReadMe2.md">
  <RelativePath>wwwroot\ReadMe2.md</RelativePath>
  <CopyToPublishDirectory>PreserveNewest</CopyToPublishDirectory>
</ResolvedFileToPublish>
```

<span data-ttu-id="11394-306">Zobacz [repozytorium zestawu SDK sieci Web Readme](https://github.com/aspnet/websdk) więcej przykładów wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="11394-306">See the [Web SDK repository Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="11394-307">Uruchom element docelowy, przed lub po opublikowaniu</span><span class="sxs-lookup"><span data-stu-id="11394-307">Run a target before or after publishing</span></span>

<span data-ttu-id="11394-308">Wbudowane `BeforePublish` i `AfterPublish` celów wykonania obiektu docelowego przed lub po docelową lokalizację publikacji.</span><span class="sxs-lookup"><span data-stu-id="11394-308">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="11394-309">Dodaj następujące elementy do profilu publikowania do rejestrowania komunikatów konsoli przed i po opublikowaniu:</span><span class="sxs-lookup"><span data-stu-id="11394-309">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="11394-310">Publikowanie na serwerze za pomocą niezaufanego certyfikatu</span><span class="sxs-lookup"><span data-stu-id="11394-310">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="11394-311">Dodaj `<AllowUntrustedCertificate>` właściwość z wartością `True` profilu publikowania:</span><span class="sxs-lookup"><span data-stu-id="11394-311">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="11394-312">Usługi Kudu</span><span class="sxs-lookup"><span data-stu-id="11394-312">The Kudu service</span></span>

<span data-ttu-id="11394-313">Aby wyświetlić pliki w usłudze Azure App Service wdrożenia aplikacji sieci web, należy użyć [usługi Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="11394-313">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="11394-314">Dołącz `scm` tokenu to nazwa aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="11394-314">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="11394-315">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="11394-315">For example:</span></span>

| <span data-ttu-id="11394-316">Adres URL</span><span class="sxs-lookup"><span data-stu-id="11394-316">URL</span></span>                                    | <span data-ttu-id="11394-317">Wynik</span><span class="sxs-lookup"><span data-stu-id="11394-317">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="11394-318">Aplikacja sieci Web</span><span class="sxs-lookup"><span data-stu-id="11394-318">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="11394-319">Usługi kudu</span><span class="sxs-lookup"><span data-stu-id="11394-319">Kudu service</span></span> |

<span data-ttu-id="11394-320">Wybierz [konsoli debugowania](https://github.com/projectkudu/kudu/wiki/Kudu-console) element menu, aby wyświetlać, edytować, usunąć lub dodać pliki.</span><span class="sxs-lookup"><span data-stu-id="11394-320">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="11394-321">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="11394-321">Additional resources</span></span>

* <span data-ttu-id="11394-322">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) upraszcza wdrażanie aplikacji sieci web i witryn sieci Web na serwerach usług IIS.</span><span class="sxs-lookup"><span data-stu-id="11394-322">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="11394-323">[Repozytorium GitHub zestawu SDK sieci Web](https://github.com/aspnet/websdk/issues): Plik problemów i funkcje do wdrożenia na żądanie.</span><span class="sxs-lookup"><span data-stu-id="11394-323">[Web SDK GitHub repository](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="11394-324">Publikowanie aplikacji sieci Web platformy ASP.NET na maszynie Wirtualnej platformy Azure z programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="11394-324">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
