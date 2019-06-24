---
title: Program Visual Studio publikowania profile na potrzeby wdrażania aplikacji platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć profile publikowania w programie Visual Studio i używać ich do zarządzania wdrożeniami aplikacji platformy ASP.NET Core do różnych celów.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/21/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 50be5a20f6d927270ef2d9dbc6c1cbf24196978f
ms.sourcegitcommit: 28646e8ca62fb094db1557b5c0c02d5b45531824
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/23/2019
ms.locfileid: "67333423"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="26ebb-103">Program Visual Studio publikowania profile na potrzeby wdrażania aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="26ebb-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="26ebb-104">Przez [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="26ebb-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="26ebb-105">Ten dokument koncentruje się na temat korzystania z programu Visual Studio 2019 lub nowszej, aby tworzenie i używanie profilów publikowania.</span><span class="sxs-lookup"><span data-stu-id="26ebb-105">This document focuses on using Visual Studio 2019 or later to create and use publish profiles.</span></span> <span data-ttu-id="26ebb-106">Profile publikowania utworzonych za pomocą programu Visual Studio może służyć za pomocą programu MSBuild i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26ebb-106">The publish profiles created with Visual Studio can be used with MSBuild and Visual Studio.</span></span> <span data-ttu-id="26ebb-107">Aby uzyskać instrukcje dotyczące publikowania na platformie Azure, zobacz <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="26ebb-107">For instructions on publishing to Azure, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

<span data-ttu-id="26ebb-108">`dotnet new mvc` Polecenie tworzy plik projektu zawierający następujący poziom główny [ \<Projekt > element](/visualstudio/msbuild/project-element-msbuild):</span><span class="sxs-lookup"><span data-stu-id="26ebb-108">The `dotnet new mvc` command produces a project file containing the following root-level [\<Project> element](/visualstudio/msbuild/project-element-msbuild):</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
</Project>
```

<span data-ttu-id="26ebb-109">Poprzedni `<Project>` elementu `Sdk` atrybut importuje MSBuild [właściwości](/visualstudio/msbuild/msbuild-properties) i [cele](/visualstudio/msbuild/msbuild-targets) z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\ SDK.props* i *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="26ebb-109">The preceding `<Project>` element's `Sdk` attribute imports the MSBuild [properties](/visualstudio/msbuild/msbuild-properties) and [targets](/visualstudio/msbuild/msbuild-targets) from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.props* and *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*, respectively.</span></span> <span data-ttu-id="26ebb-110">Domyślną lokalizacją `$(MSBuildSDKsPath)` (w programie Visual Studio 2019 Enterprise) jest *% programfiles (x86) %\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* folderu.</span><span class="sxs-lookup"><span data-stu-id="26ebb-110">The default location for `$(MSBuildSDKsPath)` (with Visual Studio 2019 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="26ebb-111">`Microsoft.NET.Sdk.Web` (Zestaw SDK sieci web) jest zależna od innych zestawów SDK, w tym `Microsoft.NET.Sdk` (.NET Core SDK) i `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)).</span><span class="sxs-lookup"><span data-stu-id="26ebb-111">`Microsoft.NET.Sdk.Web` (Web SDK) depends on other SDKs, including `Microsoft.NET.Sdk` (.NET Core SDK) and `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)).</span></span> <span data-ttu-id="26ebb-112">Właściwości programu MSBuild i elementy docelowe skojarzone z każdym zależnego zestawu SDK są importowane.</span><span class="sxs-lookup"><span data-stu-id="26ebb-112">The MSBuild properties and targets associated with each dependent SDK are imported.</span></span> <span data-ttu-id="26ebb-113">Opublikuj Importuj obiekty docelowe odpowiedni zestaw elementów docelowych na podstawie metody publikowania użytej.</span><span class="sxs-lookup"><span data-stu-id="26ebb-113">Publish targets import the appropriate set of targets based on the publish method used.</span></span>

<span data-ttu-id="26ebb-114">Podczas ładowania projektu programu MSBuild lub Visual Studio są wykonywane następujące akcje wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="26ebb-114">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="26ebb-115">Tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="26ebb-115">Build project</span></span>
* <span data-ttu-id="26ebb-116">Obliczenia plików do opublikowania</span><span class="sxs-lookup"><span data-stu-id="26ebb-116">Compute files to publish</span></span>
* <span data-ttu-id="26ebb-117">Publikowanie plików do lokalizacji docelowej</span><span class="sxs-lookup"><span data-stu-id="26ebb-117">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="26ebb-118">Obliczeniowe elementy projektu</span><span class="sxs-lookup"><span data-stu-id="26ebb-118">Compute project items</span></span>

<span data-ttu-id="26ebb-119">Gdy projekt jest ładowany, [elementów projektu programu MSBuild](/visualstudio/msbuild/common-msbuild-project-items) (pliki) są obliczane.</span><span class="sxs-lookup"><span data-stu-id="26ebb-119">When the project is loaded, the [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) (files) are computed.</span></span> <span data-ttu-id="26ebb-120">Typ elementu określa sposób przetwarzania pliku.</span><span class="sxs-lookup"><span data-stu-id="26ebb-120">The item type determines how the file is processed.</span></span> <span data-ttu-id="26ebb-121">Domyślnie *.cs* pliki są uwzględnione w `Compile` listy elementów.</span><span class="sxs-lookup"><span data-stu-id="26ebb-121">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="26ebb-122">Pliki `Compile` listy elementów są kompilowane.</span><span class="sxs-lookup"><span data-stu-id="26ebb-122">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="26ebb-123">`Content` Listy elementów zawiera pliki, które są publikowane oprócz dane wyjściowe kompilacji.</span><span class="sxs-lookup"><span data-stu-id="26ebb-123">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="26ebb-124">Domyślnie pliki pasujących do wzorców `wwwroot\**`, `**\*.config`, i `**\*.json` znajdują się w `Content` listy elementów.</span><span class="sxs-lookup"><span data-stu-id="26ebb-124">By default, files matching the patterns `wwwroot\**`, `**\*.config`, and `**\*.json` are included in the `Content` item list.</span></span> <span data-ttu-id="26ebb-125">Na przykład `wwwroot\**` [wzoru symboli wieloznacznych](https://gruntjs.com/configuring-tasks#globbing-patterns) uwzględnia wszystkie pliki w *wwwroot* folderze i jego podfolderach.</span><span class="sxs-lookup"><span data-stu-id="26ebb-125">For example, the `wwwroot\**` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder and its subfolders.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="26ebb-126">Zestaw SDK sieci Web importuje [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="26ebb-126">The Web SDK imports the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="26ebb-127">W rezultacie plików pasujących do wzorców `**\*.cshtml` i `**\*.razor` zostaną również uwzględnione w `Content` listy elementów.</span><span class="sxs-lookup"><span data-stu-id="26ebb-127">As a result, files matching the patterns `**\*.cshtml` and `**\*.razor` are also included in the `Content` item list.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="26ebb-128">Zestaw SDK sieci Web importuje [Razor SDK](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="26ebb-128">The Web SDK imports the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="26ebb-129">W rezultacie pliki pasujące `**\*.cshtml` wzorzec znajdują się również w `Content` listy elementów.</span><span class="sxs-lookup"><span data-stu-id="26ebb-129">As a result, files matching the `**\*.cshtml` pattern are also included in the `Content` item list.</span></span>

::: moniker-end

<span data-ttu-id="26ebb-130">Aby jawnie dodać plik do listy publikowania, należy dodać bezpośrednio w pliku *.csproj* pliku, jak pokazano na [pliki dołączane](#include-files) sekcji.</span><span class="sxs-lookup"><span data-stu-id="26ebb-130">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in the [Include Files](#include-files) section.</span></span>

<span data-ttu-id="26ebb-131">Podczas wybierania **Publikuj** przycisku w programie Visual Studio lub podczas publikowania z poziomu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="26ebb-131">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="26ebb-132">Elementy/właściwości są obliczane (pliki, które są potrzebne do tworzenia).</span><span class="sxs-lookup"><span data-stu-id="26ebb-132">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="26ebb-133">**Sam program Visual Studio**: Pakiety NuGet są przywracane.</span><span class="sxs-lookup"><span data-stu-id="26ebb-133">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="26ebb-134">(Przywróć musi być jawne przez użytkownika na interfejs wiersza polecenia).</span><span class="sxs-lookup"><span data-stu-id="26ebb-134">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="26ebb-135">Projekt zostanie skompilowany.</span><span class="sxs-lookup"><span data-stu-id="26ebb-135">The project builds.</span></span>
* <span data-ttu-id="26ebb-136">Publikuj elementy są obliczane (pliki, które są niezbędne do publikowania).</span><span class="sxs-lookup"><span data-stu-id="26ebb-136">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="26ebb-137">Projekt zostanie opublikowany (obliczanej pliki są kopiowane do lokalizacji docelowej publikowania).</span><span class="sxs-lookup"><span data-stu-id="26ebb-137">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="26ebb-138">Gdy odwołuje się do projektu programu ASP.NET Core `Microsoft.NET.Sdk.Web` w pliku projektu *app_offline.htm* plik zostanie umieszczony w folderze głównym katalogu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="26ebb-138">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="26ebb-139">Gdy plik jest obecny, modułu ASP.NET Core bez problemu zmieniała zamyka aplikację i służy *app_offline.htm* pliku podczas wdrażania.</span><span class="sxs-lookup"><span data-stu-id="26ebb-139">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="26ebb-140">Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="26ebb-140">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="26ebb-141">Podstawowe publikowanie wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="26ebb-141">Basic command-line publishing</span></span>

<span data-ttu-id="26ebb-142">Publikowanie wiersza polecenia działa na wszystkich platformach .NET Core, obsługiwane i nie wymaga programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26ebb-142">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="26ebb-143">W poniższych przykładach, interfejsu wiersza polecenia platformy .NET Core w [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie jest wykonywane z katalogu projektu (który zawiera *.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="26ebb-143">In the following examples, the .NET Core CLI's [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="26ebb-144">Jeśli folder projektu nie jest bieżący katalog roboczy, jawne przekazywanie ścieżka pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="26ebb-144">If the project folder isn't the current working directory, explicitly pass in the project file path.</span></span> <span data-ttu-id="26ebb-145">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="26ebb-145">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="26ebb-146">Uruchom następujące polecenia, aby tworzyć i publikować aplikację sieci web:</span><span class="sxs-lookup"><span data-stu-id="26ebb-146">Run the following commands to create and publish a web app:</span></span>

```console
dotnet new mvc
dotnet publish
```

<span data-ttu-id="26ebb-147">`dotnet publish` Polecenie generuje odmianą następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="26ebb-147">The `dotnet publish` command produces a variation of the following output:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version {VERSION} for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 36.81 ms for C:\Webs\Web1\Web1.csproj.
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\Web1.Views.dll
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\publish\
```

<span data-ttu-id="26ebb-148">Folderu publikowania domyślny format jest *bin\Debug\\\publish {MONIKER platformy docelowej}\\* .</span><span class="sxs-lookup"><span data-stu-id="26ebb-148">The default publish folder format is *bin\Debug\\{TARGET FRAMEWORK MONIKER}\publish\\*.</span></span> <span data-ttu-id="26ebb-149">Na przykład *bin\Debug\netcoreapp2.2\publish\\* .</span><span class="sxs-lookup"><span data-stu-id="26ebb-149">For example, *bin\Debug\netcoreapp2.2\publish\\*.</span></span>

<span data-ttu-id="26ebb-150">Poniższe polecenie Określa `Release` kompilowanie i publikowanie katalogu:</span><span class="sxs-lookup"><span data-stu-id="26ebb-150">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="26ebb-151">`dotnet publish` Wywołania programu MSBuild, które wywołuje polecenie `Publish` docelowej.</span><span class="sxs-lookup"><span data-stu-id="26ebb-151">The `dotnet publish` command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="26ebb-152">Wszelkie parametry przekazywane do `dotnet publish` są przekazywane do MSBuild.</span><span class="sxs-lookup"><span data-stu-id="26ebb-152">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="26ebb-153">`-c` i `-o` mapowania parametrów w MSBuild `Configuration` i `OutputPath` właściwości, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="26ebb-153">The `-c` and `-o` parameters map to MSBuild's `Configuration` and `OutputPath` properties, respectively.</span></span>

<span data-ttu-id="26ebb-154">Właściwości programu MSBuild może być przekazywany przy użyciu jednej z następujących formatów:</span><span class="sxs-lookup"><span data-stu-id="26ebb-154">MSBuild properties can be passed using either of the following formats:</span></span>

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="26ebb-155">Na przykład następujące polecenie publikuje `Release` kompilacja — przejście do udziału sieciowego.</span><span class="sxs-lookup"><span data-stu-id="26ebb-155">For example, the following command publishes a `Release` build to a network share.</span></span> <span data-ttu-id="26ebb-156">Udział sieciowy jest określony za pomocą ukośników ( *//r8/* ) i działa na wszystkich platformach .NET Core, obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="26ebb-156">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="26ebb-157">Upewnij się, że opublikowanej aplikacji do wdrożenia nie jest uruchomione.</span><span class="sxs-lookup"><span data-stu-id="26ebb-157">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="26ebb-158">Pliki *publikowania* folderu są zablokowane, gdy aplikacja jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="26ebb-158">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="26ebb-159">Wdrożenie nie może wystąpić, ponieważ zablokowany, nie można skopiować plików.</span><span class="sxs-lookup"><span data-stu-id="26ebb-159">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="26ebb-160">Profile publikowania</span><span class="sxs-lookup"><span data-stu-id="26ebb-160">Publish profiles</span></span>

<span data-ttu-id="26ebb-161">W tej sekcji używa programu Visual Studio 2019 lub nowszej w celu utworzenia profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="26ebb-161">This section uses Visual Studio 2019 or later to create a publishing profile.</span></span> <span data-ttu-id="26ebb-162">Po utworzeniu profilu publikowania z wiersza polecenia lub programu Visual Studio jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="26ebb-162">Once the profile is created, publishing from Visual Studio or the command line is available.</span></span> <span data-ttu-id="26ebb-163">Publikowanie profilów można uprościć proces publikowania, a może znajdować się dowolna liczba profilów.</span><span class="sxs-lookup"><span data-stu-id="26ebb-163">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span>

<span data-ttu-id="26ebb-164">Tworzenie profilu publikowania w programie Visual Studio, wybierając jedną z następujących ścieżek:</span><span class="sxs-lookup"><span data-stu-id="26ebb-164">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="26ebb-165">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="26ebb-165">Right-click the project in **Solution Explorer** and select **Publish**.</span></span>
* <span data-ttu-id="26ebb-166">Wybierz **publikowania {Nazwa projektu}** z **kompilacji** menu.</span><span class="sxs-lookup"><span data-stu-id="26ebb-166">Select **Publish {PROJECT NAME}** from the **Build** menu.</span></span>

<span data-ttu-id="26ebb-167">**Publikuj** jest wyświetlana na karcie Strona możliwości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="26ebb-167">The **Publish** tab of the app capabilities page is displayed.</span></span> <span data-ttu-id="26ebb-168">Jeśli projekt nie ma profilu publikowania, **wybierz lokalizację docelową publikowania** zostanie wyświetlona strona.</span><span class="sxs-lookup"><span data-stu-id="26ebb-168">If the project lacks a publish profile, the **Pick a publish target** page is displayed.</span></span> <span data-ttu-id="26ebb-169">Pojawi się prośba o wybranie jednej z następujących elementów docelowych publikowania:</span><span class="sxs-lookup"><span data-stu-id="26ebb-169">You're asked to select one of the following publish targets:</span></span>

* <span data-ttu-id="26ebb-170">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="26ebb-170">Azure App Service</span></span>
* <span data-ttu-id="26ebb-171">Usługa Azure App Service w systemie Linux</span><span class="sxs-lookup"><span data-stu-id="26ebb-171">Azure App Service on Linux</span></span>
* <span data-ttu-id="26ebb-172">Usługa Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="26ebb-172">Azure Virtual Machines</span></span>
* <span data-ttu-id="26ebb-173">Folder</span><span class="sxs-lookup"><span data-stu-id="26ebb-173">Folder</span></span>
* <span data-ttu-id="26ebb-174">Usługi IIS, FTP, narzędzie Web Deploy (na dowolnym serwerze sieci web)</span><span class="sxs-lookup"><span data-stu-id="26ebb-174">IIS, FTP, Web Deploy (for any web server)</span></span>
* <span data-ttu-id="26ebb-175">Importuj profil</span><span class="sxs-lookup"><span data-stu-id="26ebb-175">Import Profile</span></span>

<span data-ttu-id="26ebb-176">Aby określić najbardziej odpowiednie publikowania docelowych, zobacz [jakie opcje publikowania są dla mnie odpowiednia](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="26ebb-176">To determine the most appropriate publish target, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="26ebb-177">Gdy **folderu** opublikować obiekt docelowy jest wybierany, określ ścieżkę folderu do przechowywania zasobów opublikowanych.</span><span class="sxs-lookup"><span data-stu-id="26ebb-177">When the **Folder** publish target is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="26ebb-178">Domyślna ścieżka folderu jest *bin\\{Konfiguracja projektu}\\\publish {MONIKER platformy docelowej}\\* .</span><span class="sxs-lookup"><span data-stu-id="26ebb-178">The default folder path is *bin\\{PROJECT CONFIGURATION}\\{TARGET FRAMEWORK MONIKER}\publish\\*.</span></span> <span data-ttu-id="26ebb-179">Na przykład *bin\Release\netcoreapp2.2\publish\\* .</span><span class="sxs-lookup"><span data-stu-id="26ebb-179">For example, *bin\Release\netcoreapp2.2\publish\\*.</span></span> <span data-ttu-id="26ebb-180">Wybierz **Utwórz profil** przycisk, aby zakończyć.</span><span class="sxs-lookup"><span data-stu-id="26ebb-180">Select the **Create Profile** button to finish.</span></span>

<span data-ttu-id="26ebb-181">Po utworzeniu profilu publikowania **Publikuj** zmiany zawartości karty.</span><span class="sxs-lookup"><span data-stu-id="26ebb-181">Once a publish profile is created, the **Publish** tab's content changes.</span></span> <span data-ttu-id="26ebb-182">Nowo utworzony profil zostanie wyświetlony na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="26ebb-182">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="26ebb-183">Poniżej listy rozwijanej wybierz **Utwórz nowy profil** do Utwórz inny nowy profil.</span><span class="sxs-lookup"><span data-stu-id="26ebb-183">Below the drop-down list, select **Create new profile** to create another new profile.</span></span>

<span data-ttu-id="26ebb-184">Narzędzia publikowania w programie Visual Studio generuje *właściwości/PublishProfiles / {nazwa profilu} .pubxml* MSBuild pliku opisu profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="26ebb-184">Visual Studio's publish tool produces a *Properties/PublishProfiles/{PROFILE NAME}.pubxml* MSBuild file describing the publish profile.</span></span> <span data-ttu-id="26ebb-185">*.Pubxml* pliku:</span><span class="sxs-lookup"><span data-stu-id="26ebb-185">The *.pubxml* file:</span></span>

* <span data-ttu-id="26ebb-186">Zawiera ustawienia konfiguracji publikowania i jest używany przez proces publikowania.</span><span class="sxs-lookup"><span data-stu-id="26ebb-186">Contains publish configuration settings and is consumed by the publishing process.</span></span>
* <span data-ttu-id="26ebb-187">Można zmodyfikować dostosowywania kompilacji i publikowania procesu.</span><span class="sxs-lookup"><span data-stu-id="26ebb-187">Can be modified to customize the build and publish process.</span></span>

<span data-ttu-id="26ebb-188">Podczas publikowania do obiektu docelowego platformy Azure, *.pubxml* plik zawiera identyfikator swojej subskrypcji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="26ebb-188">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="26ebb-189">Z tym typem docelowym dodając ten plik do kontroli źródła nie jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="26ebb-189">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="26ebb-190">Podczas publikowania w celu spoza platformy Azure, jest bezpieczne do zaewidencjonowania *.pubxml* pliku.</span><span class="sxs-lookup"><span data-stu-id="26ebb-190">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="26ebb-191">Informacje poufne (na przykład hasło publikowania) są szyfrowane na komputerze na poziomie użytkownika/komputera.</span><span class="sxs-lookup"><span data-stu-id="26ebb-191">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="26ebb-192">Jest on przechowywany w *PublishProfiles/właściwości / {Nazwa profilu}.pubxml.user* pliku.</span><span class="sxs-lookup"><span data-stu-id="26ebb-192">It's stored in the *Properties/PublishProfiles/{PROFILE NAME}.pubxml.user* file.</span></span> <span data-ttu-id="26ebb-193">Ponieważ ten plik można przechowywać poufne informacje, nie powinny zostać sprawdzone w kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="26ebb-193">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="26ebb-194">Omówienie sposobu publikowania aplikacji sieci web ASP.NET Core, zobacz <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="26ebb-194">For an overview of how to publish an ASP.NET Core web app, see <xref:host-and-deploy/index>.</span></span> <span data-ttu-id="26ebb-195">Zadania programu MSBuild i elementy docelowe niezbędne do opublikowania aplikacji sieci web ASP.NET Core są typu open source w [repozytorium aspnet/websdk](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="26ebb-195">The MSBuild tasks and targets necessary to publish an ASP.NET Core web app are open-source at the [aspnet/websdk repository](https://github.com/aspnet/websdk).</span></span>

<span data-ttu-id="26ebb-196">`dotnet publish` Polecenia można użyć folderu, MSDeploy, i [Kudu](https://github.com/projectkudu/kudu/wiki) profilów publikowania.</span><span class="sxs-lookup"><span data-stu-id="26ebb-196">The `dotnet publish` command can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles.</span></span> <span data-ttu-id="26ebb-197">Ponieważ MSDeploy nie ma obsługi wielu platform, następujące opcje MSDeploy są obsługiwane tylko w Windows.</span><span class="sxs-lookup"><span data-stu-id="26ebb-197">Because MSDeploy lacks cross-platform support, the following MSDeploy options are supported only on Windows.</span></span>

<span data-ttu-id="26ebb-198">**Folder (działa dla wielu platform):**</span><span class="sxs-lookup"><span data-stu-id="26ebb-198">**Folder (works cross-platform):**</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="26ebb-199">**Program MSDeploy:**</span><span class="sxs-lookup"><span data-stu-id="26ebb-199">**MSDeploy:**</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="26ebb-200">**Pakiet narzędzia MSDeploy:**</span><span class="sxs-lookup"><span data-stu-id="26ebb-200">**MSDeploy package:**</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="26ebb-201">W powyższych przykładach nie przekazuj `deployonbuild` do `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="26ebb-201">In the preceding examples, don't pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="26ebb-202">Aby uzyskać więcej informacji, zobacz [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="26ebb-202">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="26ebb-203">`dotnet publish` obsługuje Kudu interfejsy API w celu publikowania na platformie Azure z dowolnej platformy.</span><span class="sxs-lookup"><span data-stu-id="26ebb-203">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="26ebb-204">Program Visual Studio publikuje obsługuje interfejsy API programu Kudu, ale jest obsługiwany przez WebSDK dla wielu platform, opublikuj na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="26ebb-204">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="26ebb-205">Dodaj profil publikowania do projektu *właściwości/PublishProfiles* folderu o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="26ebb-205">Add a publish profile to the project's *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="26ebb-206">Uruchom następujące polecenie, aby zip zapasową zawartości publikowania i opublikować go na platformie Azure przy użyciu interfejsów API Kudu:</span><span class="sxs-lookup"><span data-stu-id="26ebb-206">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="26ebb-207">Ustaw następujące właściwości programu MSBuild, korzystając z profilu publikowania:</span><span class="sxs-lookup"><span data-stu-id="26ebb-207">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

<span data-ttu-id="26ebb-208">Podczas publikowania za pomocą profilu o nazwie *FolderProfile*, można wykonać jedną z poniższych poleceń:</span><span class="sxs-lookup"><span data-stu-id="26ebb-208">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="26ebb-209">.NET Core interfejsu wiersza polecenia [kompilacji dotnet](/dotnet/core/tools/dotnet-build) polecenia wywołania `msbuild` do uruchamiania kompilacji i procesu publikowania.</span><span class="sxs-lookup"><span data-stu-id="26ebb-209">The .NET Core CLI's [dotnet build](/dotnet/core/tools/dotnet-build) command calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="26ebb-210">`dotnet build` i `msbuild` polecenia są równoważne przy przekazywaniu w folderze profilu.</span><span class="sxs-lookup"><span data-stu-id="26ebb-210">The `dotnet build` and `msbuild` commands are equivalent when passing in a folder profile.</span></span> <span data-ttu-id="26ebb-211">Podczas wywoływania `msbuild` bezpośrednio na Windows, .NET Framework w wersji programu MSBuild jest używany.</span><span class="sxs-lookup"><span data-stu-id="26ebb-211">When calling `msbuild` directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="26ebb-212">Wywoływanie `dotnet build` profil i folderów:</span><span class="sxs-lookup"><span data-stu-id="26ebb-212">Calling `dotnet build` on a non-folder profile:</span></span>

* <span data-ttu-id="26ebb-213">Wywołuje `msbuild`, który używa MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="26ebb-213">Invokes `msbuild`, which uses MSDeploy.</span></span>
* <span data-ttu-id="26ebb-214">Powoduje błąd (nawet wtedy, gdy systemem Windows).</span><span class="sxs-lookup"><span data-stu-id="26ebb-214">Results in a failure (even when running on Windows).</span></span> <span data-ttu-id="26ebb-215">Aby opublikować za pomocą profilu i folderów, należy wywołać `msbuild` bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="26ebb-215">To publish with a non-folder profile, call `msbuild` directly.</span></span>

<span data-ttu-id="26ebb-216">Profil publikowania następujący folder został utworzony za pomocą programu Visual Studio i publikuje do udziału sieciowego:</span><span class="sxs-lookup"><span data-stu-id="26ebb-216">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="26ebb-217">W poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="26ebb-217">In the preceding example:</span></span>

* <span data-ttu-id="26ebb-218">`<ExcludeApp_Data>` Właściwość występuje jedynie w celu spełnienia wymagań schematu XML.</span><span class="sxs-lookup"><span data-stu-id="26ebb-218">The `<ExcludeApp_Data>` property is present merely to satisfy an XML schema requirement.</span></span> <span data-ttu-id="26ebb-219">`<ExcludeApp_Data>` Właściwość nie ma wpływu na proces publikowania, nawet jeśli dostępny jest *App_Data* folderu w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="26ebb-219">The `<ExcludeApp_Data>` property has no effect on the publish process, even if there's an *App_Data* folder in the project root.</span></span> <span data-ttu-id="26ebb-220">*App_Data* folderu nie odbierze specjalnego traktowania, co w projektach programów ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="26ebb-220">The *App_Data* folder doesn't receive special treatment as it does in ASP.NET 4.x projects.</span></span>

* <span data-ttu-id="26ebb-221">`<LastUsedBuildConfiguration>` Właściwość jest ustawiona na `Release`.</span><span class="sxs-lookup"><span data-stu-id="26ebb-221">The `<LastUsedBuildConfiguration>` property is set to `Release`.</span></span> <span data-ttu-id="26ebb-222">Podczas publikowania z programu Visual Studio, wartość `<LastUsedBuildConfiguration>` można ustawić przy użyciu wartości, gdy proces publikowania zostanie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="26ebb-222">When publishing from Visual Studio, the value of `<LastUsedBuildConfiguration>` is set using the value when the publish process is started.</span></span> <span data-ttu-id="26ebb-223">`<LastUsedBuildConfiguration>` jest szczególna i nie powinna zostać zastąpiona w importowanym pliku MSBuild.</span><span class="sxs-lookup"><span data-stu-id="26ebb-223">`<LastUsedBuildConfiguration>` is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="26ebb-224">Tej właściwości można jednak zastąpić z wiersza polecenia przy użyciu jednej z poniższych metod.</span><span class="sxs-lookup"><span data-stu-id="26ebb-224">This property can, however, be overridden from the command line using one of the following approaches.</span></span>
  * <span data-ttu-id="26ebb-225">Korzystanie z platformy .NET Core interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="26ebb-225">Using the .NET Core CLI:</span></span>

    ```console
    dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
    ```

  * <span data-ttu-id="26ebb-226">Korzystanie z programu MSBuild:</span><span class="sxs-lookup"><span data-stu-id="26ebb-226">Using MSBuild:</span></span>

    ```console
    msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
    ```

  <span data-ttu-id="26ebb-227">Aby uzyskać więcej informacji, zobacz [MSBuild: sposób ustawiania właściwości konfiguracji](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).</span><span class="sxs-lookup"><span data-stu-id="26ebb-227">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).</span></span>

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="26ebb-228">Publikowanie do punktu końcowego MSDeploy z wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="26ebb-228">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="26ebb-229">W poniższym przykładzie użyto aplikacji sieci web ASP.NET Core utworzona przez program Visual Studio o nazwie *AzureWebApp*.</span><span class="sxs-lookup"><span data-stu-id="26ebb-229">The following example uses an ASP.NET Core web app created by Visual Studio named *AzureWebApp*.</span></span> <span data-ttu-id="26ebb-230">Profil publikowania aplikacji platformy Azure jest dodawany z programem Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26ebb-230">An Azure Apps publish profile is added with Visual Studio.</span></span> <span data-ttu-id="26ebb-231">Aby uzyskać więcej informacji na temat sposobu tworzenia profilu, zobacz [profilów publikowania](#publish-profiles) sekcji.</span><span class="sxs-lookup"><span data-stu-id="26ebb-231">For more information on how to create a profile, see the [Publish profiles](#publish-profiles) section.</span></span>

<span data-ttu-id="26ebb-232">Aby wdrożyć aplikację za pomocą profilu publikowania, wykonaj `msbuild` polecenia z programu Visual Studio **wiersz polecenia dla deweloperów**.</span><span class="sxs-lookup"><span data-stu-id="26ebb-232">To deploy the app using a publish profile, execute the `msbuild` command from a Visual Studio **Developer Command Prompt**.</span></span> <span data-ttu-id="26ebb-233">Wiersz polecenia jest dostępna w *programu Visual Studio* folderu **Start** menu na pasku zadań Windows.</span><span class="sxs-lookup"><span data-stu-id="26ebb-233">The command prompt is available in the *Visual Studio* folder of the **Start** menu on the Windows taskbar.</span></span> <span data-ttu-id="26ebb-234">W celu ułatwienia dostępu, można dodać wiersza polecenia, aby **narzędzia** menu w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26ebb-234">For easier access, you can add the command prompt to the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="26ebb-235">Aby uzyskać więcej informacji, zobacz [wiersz polecenia programisty dla programu Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="26ebb-235">For more information, see [Developer Command Prompt for Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span></span>

<span data-ttu-id="26ebb-236">Program MSBuild używa następującej składni polecenia:</span><span class="sxs-lookup"><span data-stu-id="26ebb-236">MSBuild uses the following command syntax:</span></span>

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* <span data-ttu-id="26ebb-237">{PATH} &ndash; Ścieżka do pliku projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="26ebb-237">{PATH} &ndash; Path to the app's project file.</span></span>
* <span data-ttu-id="26ebb-238">{PROFIL} &ndash; Nazwa profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="26ebb-238">{PROFILE} &ndash; Name of the publish profile.</span></span>
* <span data-ttu-id="26ebb-239">UŻYTKOWNIK {USERNAME} &ndash; MSDeploy username.</span><span class="sxs-lookup"><span data-stu-id="26ebb-239">{USERNAME} &ndash; MSDeploy username.</span></span> <span data-ttu-id="26ebb-240">{USERNAME} można znaleźć w profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="26ebb-240">The {USERNAME} can be found in the publish profile.</span></span>
* <span data-ttu-id="26ebb-241">{PASSWORD} &ndash; MSDeploy hasła.</span><span class="sxs-lookup"><span data-stu-id="26ebb-241">{PASSWORD} &ndash; MSDeploy password.</span></span> <span data-ttu-id="26ebb-242">Uzyskaj {PASSWORD} z *{profil}. PublishSettings* pliku.</span><span class="sxs-lookup"><span data-stu-id="26ebb-242">Obtain the {PASSWORD} from the *{PROFILE}.PublishSettings* file.</span></span> <span data-ttu-id="26ebb-243">Pobierz *. PublishSettings* plików z poziomu:</span><span class="sxs-lookup"><span data-stu-id="26ebb-243">Download the *.PublishSettings* file from either:</span></span>
  * <span data-ttu-id="26ebb-244">**Eksplorator rozwiązań**: Wybierz **widoku** > **Eksplorator chmury**.</span><span class="sxs-lookup"><span data-stu-id="26ebb-244">**Solution Explorer**: Select **View** > **Cloud Explorer**.</span></span> <span data-ttu-id="26ebb-245">Połącz z subskrypcją platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="26ebb-245">Connect with your Azure subscription.</span></span> <span data-ttu-id="26ebb-246">Otwórz **usług App Services**.</span><span class="sxs-lookup"><span data-stu-id="26ebb-246">Open **App Services**.</span></span> <span data-ttu-id="26ebb-247">Kliknij prawym przyciskiem myszy aplikację.</span><span class="sxs-lookup"><span data-stu-id="26ebb-247">Right-click the app.</span></span> <span data-ttu-id="26ebb-248">Wybierz **Pobierz profil publikowania**.</span><span class="sxs-lookup"><span data-stu-id="26ebb-248">Select **Download Publish Profile**.</span></span>
  * <span data-ttu-id="26ebb-249">Witryna Azure portal: Wybierz **Pobierz profil publikowania** w aplikacji sieci web **Przegląd** panelu.</span><span class="sxs-lookup"><span data-stu-id="26ebb-249">Azure portal: Select **Get publish profile** in the web app's **Overview** panel.</span></span>

<span data-ttu-id="26ebb-250">W poniższym przykładzie użyto profil publikowania o nazwie *AzureWebApp — narzędzie Web Deploy*:</span><span class="sxs-lookup"><span data-stu-id="26ebb-250">The following example uses a publish profile named *AzureWebApp - Web Deploy*:</span></span>

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

<span data-ttu-id="26ebb-251">Profil publikowania można również za pomocą platformy .NET Core interfejsu wiersza polecenia [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) polecenia powłoki poleceń Windows:</span><span class="sxs-lookup"><span data-stu-id="26ebb-251">A publish profile can also be used with the .NET Core CLI's [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command from a Windows command shell:</span></span>

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!IMPORTANT]
> <span data-ttu-id="26ebb-252">`dotnet msbuild` Polecenie to polecenie dla wielu platform i można kompilować aplikacje platformy ASP.NET Core w systemach macOS i Linux.</span><span class="sxs-lookup"><span data-stu-id="26ebb-252">The `dotnet msbuild` command is a cross-platform command and can compile ASP.NET Core apps on macOS and Linux.</span></span> <span data-ttu-id="26ebb-253">Jednak program MSBuild w systemie macOS i Linux nie jest zdolny do wdrażania aplikacji na platformie Azure lub inne punkty końcowe MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="26ebb-253">However, MSBuild on macOS and Linux isn't capable of deploying an app to Azure or other MSDeploy endpoints.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="26ebb-254">Ustaw środowisko</span><span class="sxs-lookup"><span data-stu-id="26ebb-254">Set the environment</span></span>

<span data-ttu-id="26ebb-255">Obejmują `<EnvironmentName>` właściwości w profilu publikowania ( *.pubxml*) lub plik projektu do zestawu aplikacji [środowiska](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="26ebb-255">Include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file to set the app's [environment](xref:fundamentals/environments):</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="26ebb-256">Jeśli potrzebujesz *web.config* przekształcenia (na przykład ustawienie zmiennych środowiskowych na podstawie konfiguracji, profilu lub środowiska), zobacz <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="26ebb-256">If you require *web.config* transformations (for example, setting environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="exclude-files"></a><span data-ttu-id="26ebb-257">Wyklucz pliki</span><span class="sxs-lookup"><span data-stu-id="26ebb-257">Exclude files</span></span>

<span data-ttu-id="26ebb-258">Podczas publikowania aplikacji sieci web platformy ASP.NET Core, uwzględnione są następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="26ebb-258">When publishing ASP.NET Core web apps, the following assets are included:</span></span>

* <span data-ttu-id="26ebb-259">Artefakty kompilacji</span><span class="sxs-lookup"><span data-stu-id="26ebb-259">Build artifacts</span></span>
* <span data-ttu-id="26ebb-260">Foldery i pliki zgodne z następujących wzorców obsługi symboli wieloznacznych:</span><span class="sxs-lookup"><span data-stu-id="26ebb-260">Folders and files matching the following globbing patterns:</span></span>
  * <span data-ttu-id="26ebb-261">`**\*.config` (na przykład *web.config*)</span><span class="sxs-lookup"><span data-stu-id="26ebb-261">`**\*.config` (for example, *web.config*)</span></span>
  * <span data-ttu-id="26ebb-262">`**\*.json` (na przykład *appsettings.json*)</span><span class="sxs-lookup"><span data-stu-id="26ebb-262">`**\*.json` (for example, *appsettings.json*)</span></span>
  * `wwwroot\**`

<span data-ttu-id="26ebb-263">Obsługuje program MSBuild [wzorców obsługi symboli wieloznacznych](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="26ebb-263">MSBuild supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="26ebb-264">Na przykład następująca `<Content>` element pomija kopiowanie tekstu ( *.txt*) pliki *wwwroot\content* folderze i jego podfolderach:</span><span class="sxs-lookup"><span data-stu-id="26ebb-264">For example, the following `<Content>` element suppresses the copying of text (*.txt*) files in the *wwwroot\content* folder and its subfolders:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="26ebb-265">Poprzedni kod znaczników, które można dodać do profilu publikowania lub *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="26ebb-265">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="26ebb-266">Po dodaniu do *.csproj* plik, reguła zostanie dodany do wszystkich profilów publikowania w projekcie.</span><span class="sxs-lookup"><span data-stu-id="26ebb-266">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="26ebb-267">Następujące `<MsDeploySkipRules>` element wyklucza wszystkie pliki z *wwwroot\content* folderu:</span><span class="sxs-lookup"><span data-stu-id="26ebb-267">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot\content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="26ebb-268">`<MsDeploySkipRules>` nie usuwa *pominąć* obiekty docelowe z witryny wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="26ebb-268">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="26ebb-269">`<Content>` docelowe pliki i foldery są usuwane z lokacji wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="26ebb-269">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="26ebb-270">Na przykład załóżmy, że wdrożonej aplikacji sieci web ma następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="26ebb-270">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="26ebb-271">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="26ebb-271">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="26ebb-272">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="26ebb-272">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="26ebb-273">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="26ebb-273">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="26ebb-274">Jeśli następujące `<MsDeploySkipRules>` elementy są dodawane, te pliki nie zostaną usunięte w witrynie wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="26ebb-274">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="26ebb-275">Poprzedni `<MsDeploySkipRules>` elementy zapobiegania *pominięte* pliki z wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="26ebb-275">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="26ebb-276">Nie będzie go usunąć te pliki, gdy są one wdrażane.</span><span class="sxs-lookup"><span data-stu-id="26ebb-276">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="26ebb-277">Następujące `<Content>` elementu usuwa pliki docelowe lokacji wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="26ebb-277">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="26ebb-278">Za pomocą wiersza polecenia z poprzednim okresem `<Content>` element daje odmianą następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="26ebb-278">Using command-line deployment with the preceding `<Content>` element yields a variation of the following output:</span></span>

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\{TARGET FRAMEWORK MONIKER}\PubTmp\Web1.SourceManifest.
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

## <a name="include-files"></a><span data-ttu-id="26ebb-279">Dołącz pliki</span><span class="sxs-lookup"><span data-stu-id="26ebb-279">Include files</span></span>

<span data-ttu-id="26ebb-280">Następujące sekcje konspektu podejścia do dołączania plików w czasie publikacji.</span><span class="sxs-lookup"><span data-stu-id="26ebb-280">The following sections outline different approaches for file inclusion at publish time.</span></span> <span data-ttu-id="26ebb-281">[Dołączania plików ogólnego](#general-file-inclusion) sekcji używa `DotNetPublishFiles` elementu, który jest dostarczany przez plik elementów docelowych publikowania w zestawie SDK sieci Web.</span><span class="sxs-lookup"><span data-stu-id="26ebb-281">The [General file inclusion](#general-file-inclusion) section uses the `DotNetPublishFiles` item, which is provided by a publish targets file in the Web SDK.</span></span> <span data-ttu-id="26ebb-282">[Dołączania plików selektywne](#selective-file-inclusion) sekcji używa `ResolvedFileToPublish` elementu, który jest dostarczany przez plik elementów docelowych publikowania w zestawie SDK programu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="26ebb-282">The [Selective file inclusion](#selective-file-inclusion) section uses the `ResolvedFileToPublish` item, which is provided by a publish targets file in the .NET Core SDK.</span></span> <span data-ttu-id="26ebb-283">Ponieważ zestaw SDK sieci Web jest zależny od zestawu .NET Core SDK, albo element może służyć w projektach programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="26ebb-283">Because the Web SDK depends on the .NET Core SDK, either item can be used in an ASP.NET Core project.</span></span>

### <a name="general-file-inclusion"></a><span data-ttu-id="26ebb-284">Włączenie pliku ogólny</span><span class="sxs-lookup"><span data-stu-id="26ebb-284">General file inclusion</span></span>

<span data-ttu-id="26ebb-285">Poniższy przykład `<ItemGroup>` element pokazuje kopiowania folderu znajduje się poza katalogiem projektu do folderu opublikowanej witryny.</span><span class="sxs-lookup"><span data-stu-id="26ebb-285">The following example's `<ItemGroup>` element demonstrates copying a folder located outside of the project directory to a folder of the published site.</span></span> <span data-ttu-id="26ebb-286">Wszelkie pliki dodane do niej następujące znaczniki `<ItemGroup>` są domyślnie dołączone.</span><span class="sxs-lookup"><span data-stu-id="26ebb-286">Any files added to the following markup's `<ItemGroup>` are included by default.</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotNetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotNetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="26ebb-287">Poprzedni kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="26ebb-287">The preceding markup:</span></span>

* <span data-ttu-id="26ebb-288">Mogą być dodawane do *.csproj* pliku lub profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="26ebb-288">Can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="26ebb-289">Jeśli jest dodawany do *.csproj* pliku, jest on zawarty w każdym profilu publikowania w projekcie.</span><span class="sxs-lookup"><span data-stu-id="26ebb-289">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>
* <span data-ttu-id="26ebb-290">Deklaruje `_CustomFiles` elementu do przechowywania plików pasujących `Include` wzoru symboli wieloznacznych atrybutu.</span><span class="sxs-lookup"><span data-stu-id="26ebb-290">Declares a `_CustomFiles` item to store files matching the `Include` attribute's globbing pattern.</span></span> <span data-ttu-id="26ebb-291">*Obrazów* folder, do którego odwołuje się do wzorca znajduje się poza katalogiem projektu.</span><span class="sxs-lookup"><span data-stu-id="26ebb-291">The *images* folder referenced in the pattern is located outside of the project directory.</span></span> <span data-ttu-id="26ebb-292">A [zastrzeżone właściwość](/visualstudio/msbuild/msbuild-reserved-and-well-known-properties)o nazwie `$(MSBuildProjectDirectory)`, jest rozpoznawana jako ścieżka bezwzględna do pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="26ebb-292">A [reserved property](/visualstudio/msbuild/msbuild-reserved-and-well-known-properties), named `$(MSBuildProjectDirectory)`, resolves to the project file's absolute path.</span></span>
* <span data-ttu-id="26ebb-293">Zawiera listę plików do `DotNetPublishFiles` elementu.</span><span class="sxs-lookup"><span data-stu-id="26ebb-293">Provides a list of files to the `DotNetPublishFiles` item.</span></span> <span data-ttu-id="26ebb-294">Domyślnie element firmy `<DestinationRelativePath>` element jest pusty.</span><span class="sxs-lookup"><span data-stu-id="26ebb-294">By default, the item's `<DestinationRelativePath>` element is empty.</span></span> <span data-ttu-id="26ebb-295">Wartością domyślną jest zastępowany w znaczniku i używa [metadane dobrze znanego elementu](/visualstudio/msbuild/msbuild-well-known-item-metadata) takich jak `%(RecursiveDir)`.</span><span class="sxs-lookup"><span data-stu-id="26ebb-295">The default value is overridden in the markup and uses [well-known item metadata](/visualstudio/msbuild/msbuild-well-known-item-metadata) such as `%(RecursiveDir)`.</span></span> <span data-ttu-id="26ebb-296">Reprezentuje tekst wewnętrzny *wwwroot/obrazów* folderu opublikowanej witryny.</span><span class="sxs-lookup"><span data-stu-id="26ebb-296">The inner text represents the *wwwroot/images* folder of the published site.</span></span>

### <a name="selective-file-inclusion"></a><span data-ttu-id="26ebb-297">Włączenie pliku selektywnego</span><span class="sxs-lookup"><span data-stu-id="26ebb-297">Selective file inclusion</span></span>

<span data-ttu-id="26ebb-298">Przedstawia podświetlony znaczniki w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="26ebb-298">The highlighted markup in the following example demonstrates:</span></span>

* <span data-ttu-id="26ebb-299">Kopiowanie pliku znajdującego się poza projektem w opublikowanej witryny *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="26ebb-299">Copying a file located outside of the project into the published site's *wwwroot* folder.</span></span> <span data-ttu-id="26ebb-300">Nazwa pliku *ReadMe2.md* jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="26ebb-300">The file name of *ReadMe2.md* is maintained.</span></span>
* <span data-ttu-id="26ebb-301">Z wyjątkiem *wwwroot\Content* folderu.</span><span class="sxs-lookup"><span data-stu-id="26ebb-301">Excluding the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="26ebb-302">Z wyjątkiem *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="26ebb-302">Excluding *Views\Home\About2.cshtml*.</span></span>

[!code-xml[](visual-studio-publish-profiles/samples/Web1.pubxml?highlight=18-23)]

<span data-ttu-id="26ebb-303">W poprzednim przykładzie użyto `ResolvedFileToPublish` elementu, którego domyślne zachowanie to zawsze skopiuj pliki udostępniane w `Include` atrybutu do opublikowanej witryny.</span><span class="sxs-lookup"><span data-stu-id="26ebb-303">The preceding example uses the `ResolvedFileToPublish` item, whose default behavior is to always copy the files provided in the `Include` attribute to the published site.</span></span> <span data-ttu-id="26ebb-304">Zastąpić domyślne zachowanie, umieszczając `<CopyToPublishDirectory>` elementu podrzędnego z tekstu wewnętrznego albo `Never` lub `PreserveNewest`.</span><span class="sxs-lookup"><span data-stu-id="26ebb-304">Override the default behavior by including a `<CopyToPublishDirectory>` child element with inner text of either `Never` or `PreserveNewest`.</span></span> <span data-ttu-id="26ebb-305">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="26ebb-305">For example:</span></span>

```xml
<ResolvedFileToPublish Include="..\ReadMe2.md">
  <RelativePath>wwwroot\ReadMe2.md</RelativePath>
  <CopyToPublishDirectory>PreserveNewest</CopyToPublishDirectory>
</ResolvedFileToPublish>
```

<span data-ttu-id="26ebb-306">Aby uzyskać więcej przykładów wdrażania, zobacz [repozytorium zestawu SDK sieci Web Readme](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="26ebb-306">For more deployment samples, see the [Web SDK repository Readme](https://github.com/aspnet/websdk).</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="26ebb-307">Uruchom element docelowy, przed lub po opublikowaniu</span><span class="sxs-lookup"><span data-stu-id="26ebb-307">Run a target before or after publishing</span></span>

<span data-ttu-id="26ebb-308">Wbudowane `BeforePublish` i `AfterPublish` celów wykonania obiektu docelowego przed lub po docelową lokalizację publikacji.</span><span class="sxs-lookup"><span data-stu-id="26ebb-308">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="26ebb-309">Dodaj następujące elementy do profilu publikowania do rejestrowania komunikatów konsoli przed i po opublikowaniu:</span><span class="sxs-lookup"><span data-stu-id="26ebb-309">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="26ebb-310">Publikowanie na serwerze za pomocą niezaufanego certyfikatu</span><span class="sxs-lookup"><span data-stu-id="26ebb-310">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="26ebb-311">Dodaj `<AllowUntrustedCertificate>` właściwość z wartością `True` profilu publikowania:</span><span class="sxs-lookup"><span data-stu-id="26ebb-311">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="26ebb-312">Usługi Kudu</span><span class="sxs-lookup"><span data-stu-id="26ebb-312">The Kudu service</span></span>

<span data-ttu-id="26ebb-313">Aby wyświetlić pliki w usłudze Azure App Service wdrożenia aplikacji sieci web, należy użyć [usługi Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="26ebb-313">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="26ebb-314">Dołącz `scm` tokenu to nazwa aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="26ebb-314">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="26ebb-315">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="26ebb-315">For example:</span></span>

| <span data-ttu-id="26ebb-316">Adres URL</span><span class="sxs-lookup"><span data-stu-id="26ebb-316">URL</span></span>                                    | <span data-ttu-id="26ebb-317">Wynik</span><span class="sxs-lookup"><span data-stu-id="26ebb-317">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="26ebb-318">Aplikacja sieci Web</span><span class="sxs-lookup"><span data-stu-id="26ebb-318">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="26ebb-319">Usługi kudu</span><span class="sxs-lookup"><span data-stu-id="26ebb-319">Kudu service</span></span> |

<span data-ttu-id="26ebb-320">Wybierz [konsoli debugowania](https://github.com/projectkudu/kudu/wiki/Kudu-console) element menu, aby wyświetlać, edytować, usunąć lub dodać pliki.</span><span class="sxs-lookup"><span data-stu-id="26ebb-320">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="26ebb-321">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="26ebb-321">Additional resources</span></span>

* <span data-ttu-id="26ebb-322">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) upraszcza wdrażanie aplikacji sieci web i witryn sieci Web na serwerach usług IIS.</span><span class="sxs-lookup"><span data-stu-id="26ebb-322">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="26ebb-323">[Repozytorium GitHub zestawu SDK sieci Web](https://github.com/aspnet/websdk/issues): Plik problemów i funkcje do wdrożenia na żądanie.</span><span class="sxs-lookup"><span data-stu-id="26ebb-323">[Web SDK GitHub repository](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="26ebb-324">Publikowanie aplikacji sieci Web platformy ASP.NET na maszynie Wirtualnej platformy Azure z programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="26ebb-324">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
