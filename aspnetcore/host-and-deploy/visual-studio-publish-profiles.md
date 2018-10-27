---
title: Program Visual Studio publikowania profile na potrzeby wdrażania aplikacji platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć profile publikowania w programie Visual Studio i używać ich do zarządzania wdrożeniami aplikacji platformy ASP.NET Core do różnych celów.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3e626f99b06b0343360d6c46447e357890433dda
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148931"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="cbde3-103">Program Visual Studio publikowania profile na potrzeby wdrażania aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cbde3-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="cbde3-104">Przez [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cbde3-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cbde3-105">Ten dokument koncentruje się na temat korzystania z programu Visual Studio 2017 do tworzenia i używania profilów publikowania.</span><span class="sxs-lookup"><span data-stu-id="cbde3-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="cbde3-106">Profile publikowania utworzonych za pomocą programu Visual Studio można uruchomić z programu MSBuild i Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="cbde3-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="cbde3-107">Zobacz [publikowania aplikacji sieci web ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) instrukcje dotyczące publikowania na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="cbde3-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="cbde3-108">Następujący plik projektu został utworzony za pomocą polecenia `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="cbde3-108">The following project file was created with the command `dotnet new mvc`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.9" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.7" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.8" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

::: moniker-end

<span data-ttu-id="cbde3-109">`<Project>` Elementu `Sdk` atrybut wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="cbde3-109">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="cbde3-110">Importuje plik właściwości z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* na początku.</span><span class="sxs-lookup"><span data-stu-id="cbde3-110">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="cbde3-111">Importuje plik elementów docelowych z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* na końcu.</span><span class="sxs-lookup"><span data-stu-id="cbde3-111">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="cbde3-112">Domyślną lokalizacją `MSBuildSDKsPath` (w programie Visual Studio 2017 Enterprise) jest *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folderu.</span><span class="sxs-lookup"><span data-stu-id="cbde3-112">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="cbde3-113">`Microsoft.NET.Sdk.Web` Zależy od zestawu SDK:</span><span class="sxs-lookup"><span data-stu-id="cbde3-113">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="cbde3-114">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="cbde3-114">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="cbde3-115">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="cbde3-115">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="cbde3-116">Co spowoduje, że poniższe właściwości i elementy docelowe do zaimportowania:</span><span class="sxs-lookup"><span data-stu-id="cbde3-116">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="cbde3-117">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="cbde3-117">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="cbde3-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="cbde3-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="cbde3-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="cbde3-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="cbde3-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="cbde3-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="cbde3-121">Opublikuj Importuj obiekty docelowe po prawej stronie Ustaw elementów docelowych na podstawie metody publikowania użytej.</span><span class="sxs-lookup"><span data-stu-id="cbde3-121">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="cbde3-122">Podczas ładowania projektu programu MSBuild lub Visual Studio są wykonywane następujące akcje wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="cbde3-122">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="cbde3-123">Tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="cbde3-123">Build project</span></span>
* <span data-ttu-id="cbde3-124">Obliczenia plików do opublikowania</span><span class="sxs-lookup"><span data-stu-id="cbde3-124">Compute files to publish</span></span>
* <span data-ttu-id="cbde3-125">Publikowanie plików do lokalizacji docelowej</span><span class="sxs-lookup"><span data-stu-id="cbde3-125">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="cbde3-126">Obliczeniowe elementy projektu</span><span class="sxs-lookup"><span data-stu-id="cbde3-126">Compute project items</span></span>

<span data-ttu-id="cbde3-127">Gdy projekt jest ładowany, są obliczane elementy projektu (pliki).</span><span class="sxs-lookup"><span data-stu-id="cbde3-127">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="cbde3-128">`item type` Atrybut określa, jak przebiega przetwarzanie pliku.</span><span class="sxs-lookup"><span data-stu-id="cbde3-128">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="cbde3-129">Domyślnie *.cs* pliki są uwzględnione w `Compile` listy elementów.</span><span class="sxs-lookup"><span data-stu-id="cbde3-129">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="cbde3-130">Pliki `Compile` listy elementów są kompilowane.</span><span class="sxs-lookup"><span data-stu-id="cbde3-130">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="cbde3-131">`Content` Listy elementów zawiera pliki, które są publikowane oprócz dane wyjściowe kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cbde3-131">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="cbde3-132">Domyślnie pliki pasujące do wzorca `wwwroot/**` są objęte `Content` elementu.</span><span class="sxs-lookup"><span data-stu-id="cbde3-132">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="cbde3-133">`wwwroot/\*\*` [Wzoru symboli wieloznacznych](https://gruntjs.com/configuring-tasks#globbing-patterns) uwzględnia wszystkie pliki w *wwwroot* folderu **i** podfoldery.</span><span class="sxs-lookup"><span data-stu-id="cbde3-133">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="cbde3-134">Aby jawnie dodać plik do listy publikowania, należy dodać bezpośrednio w pliku *.csproj* pliku, jak pokazano na [pliki dołączane](#include-files).</span><span class="sxs-lookup"><span data-stu-id="cbde3-134">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="cbde3-135">Podczas wybierania **Publikuj** przycisku w programie Visual Studio lub podczas publikowania z poziomu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="cbde3-135">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="cbde3-136">Elementy/właściwości są obliczane (pliki, które są potrzebne do tworzenia).</span><span class="sxs-lookup"><span data-stu-id="cbde3-136">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="cbde3-137">**Sam program Visual Studio**: pakiety NuGet są przywracane.</span><span class="sxs-lookup"><span data-stu-id="cbde3-137">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="cbde3-138">(Przywróć musi być jawne przez użytkownika na interfejs wiersza polecenia).</span><span class="sxs-lookup"><span data-stu-id="cbde3-138">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="cbde3-139">Projekt zostanie skompilowany.</span><span class="sxs-lookup"><span data-stu-id="cbde3-139">The project builds.</span></span>
* <span data-ttu-id="cbde3-140">Publikuj elementy są obliczane (pliki, które są niezbędne do publikowania).</span><span class="sxs-lookup"><span data-stu-id="cbde3-140">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="cbde3-141">Projekt zostanie opublikowany (obliczanej pliki są kopiowane do lokalizacji docelowej publikowania).</span><span class="sxs-lookup"><span data-stu-id="cbde3-141">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="cbde3-142">Gdy odwołuje się do projektu programu ASP.NET Core `Microsoft.NET.Sdk.Web` w pliku projektu *app_offline.htm* plik zostanie umieszczony w folderze głównym katalogu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="cbde3-142">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="cbde3-143">Gdy plik jest obecny, modułu ASP.NET Core bez problemu zmieniała zamyka aplikację i służy *app_offline.htm* pliku podczas wdrażania.</span><span class="sxs-lookup"><span data-stu-id="cbde3-143">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="cbde3-144">Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="cbde3-144">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="cbde3-145">Podstawowe publikowanie wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="cbde3-145">Basic command-line publishing</span></span>

<span data-ttu-id="cbde3-146">Publikowanie wiersza polecenia działa na wszystkich platformach .NET Core, obsługiwane i nie wymaga programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cbde3-146">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="cbde3-147">W poniższych przykładach [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie jest wykonywane z katalogu projektu (zawierający *.csproj* pliku).</span><span class="sxs-lookup"><span data-stu-id="cbde3-147">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="cbde3-148">W przeciwnym razie w folderze projektu jawne przekazywanie w ścieżce do pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="cbde3-148">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="cbde3-149">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cbde3-149">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="cbde3-150">Uruchom następujące polecenia, aby tworzyć i publikować aplikację sieci web:</span><span class="sxs-lookup"><span data-stu-id="cbde3-150">Run the following commands to create and publish a web app:</span></span>

::: moniker range=">= aspnetcore-2.0"

```console
dotnet new mvc
dotnet publish
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```console
dotnet new mvc
dotnet restore
dotnet publish
```

::: moniker-end

<span data-ttu-id="cbde3-151">[Publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie generuje dane wyjściowe podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="cbde3-151">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="cbde3-152">Wartość domyślna publikowanie folderu jest `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="cbde3-152">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="cbde3-153">Wartość domyślna dla `$(Configuration)` jest *debugowania*.</span><span class="sxs-lookup"><span data-stu-id="cbde3-153">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="cbde3-154">W poprzednim przykładzie `<TargetFramework>` jest `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="cbde3-154">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="cbde3-155">`dotnet publish -h` Wyświetla Pomoc do opublikowania.</span><span class="sxs-lookup"><span data-stu-id="cbde3-155">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="cbde3-156">Poniższe polecenie Określa `Release` kompilowanie i publikowanie katalogu:</span><span class="sxs-lookup"><span data-stu-id="cbde3-156">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="cbde3-157">[Publikowania dotnet](/dotnet/core/tools/dotnet-publish) wywołania programu MSBuild, które wywołuje polecenie `Publish` docelowej.</span><span class="sxs-lookup"><span data-stu-id="cbde3-157">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="cbde3-158">Wszelkie parametry przekazywane do `dotnet publish` są przekazywane do MSBuild.</span><span class="sxs-lookup"><span data-stu-id="cbde3-158">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="cbde3-159">`-c` Mapuje parametr `Configuration` właściwości programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="cbde3-159">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="cbde3-160">`-o` Mapuje parametru `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="cbde3-160">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="cbde3-161">Właściwości programu MSBuild może być przekazywany przy użyciu jednej z następujących formatów:</span><span class="sxs-lookup"><span data-stu-id="cbde3-161">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="cbde3-162">Następujące polecenie publikuje `Release` kompilacja — przejście do udziału sieciowego:</span><span class="sxs-lookup"><span data-stu-id="cbde3-162">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="cbde3-163">Udział sieciowy jest określony za pomocą ukośników (*//r8/*) i działa na wszystkich platformach .NET Core, obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="cbde3-163">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="cbde3-164">Upewnij się, że opublikowanej aplikacji do wdrożenia nie jest uruchomione.</span><span class="sxs-lookup"><span data-stu-id="cbde3-164">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="cbde3-165">Pliki *publikowania* folderu są zablokowane, gdy aplikacja jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="cbde3-165">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="cbde3-166">Wdrożenie nie może wystąpić, ponieważ zablokowany, nie można skopiować plików.</span><span class="sxs-lookup"><span data-stu-id="cbde3-166">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="cbde3-167">Profile publikowania</span><span class="sxs-lookup"><span data-stu-id="cbde3-167">Publish profiles</span></span>

<span data-ttu-id="cbde3-168">Ta sekcja używa programu Visual Studio 2017, aby utworzyć profil publikowania.</span><span class="sxs-lookup"><span data-stu-id="cbde3-168">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="cbde3-169">Po utworzeniu publikowanie z programu Visual Studio lub wiersza polecenia jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="cbde3-169">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="cbde3-170">Publikowanie profilów można uprościć proces publikowania, a może znajdować się dowolna liczba profilów.</span><span class="sxs-lookup"><span data-stu-id="cbde3-170">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="cbde3-171">Tworzenie profilu publikowania w programie Visual Studio, wybierając jedną z następujących ścieżek:</span><span class="sxs-lookup"><span data-stu-id="cbde3-171">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="cbde3-172">Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="cbde3-172">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="cbde3-173">Wybierz **Publikuj &lt;project_name&gt;**  z **kompilacji** menu.</span><span class="sxs-lookup"><span data-stu-id="cbde3-173">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="cbde3-174">**Publikuj** jest wyświetlana na karcie Strona możliwości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cbde3-174">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="cbde3-175">Jeśli projekt nie ma profil publikowania, zostanie wyświetlona następująca strona:</span><span class="sxs-lookup"><span data-stu-id="cbde3-175">If the project lacks a publish profile, the following page is displayed:</span></span>

![Na karcie publikowanie strony pojemności aplikacji](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="cbde3-177">Gdy **folderu** jest zaznaczone, określ ścieżkę folderu do przechowywania zasobów opublikowanych.</span><span class="sxs-lookup"><span data-stu-id="cbde3-177">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="cbde3-178">Domyślnym folderem jest *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="cbde3-178">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="cbde3-179">Kliknij przycisk **Utwórz profil** przycisk, aby zakończyć.</span><span class="sxs-lookup"><span data-stu-id="cbde3-179">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="cbde3-180">Po utworzeniu profilu publikowania **Publikuj** karcie zmiany.</span><span class="sxs-lookup"><span data-stu-id="cbde3-180">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="cbde3-181">Nowo utworzony profil zostanie wyświetlony na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="cbde3-181">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="cbde3-182">Kliknij przycisk **Utwórz nowy profil** do Utwórz inny nowy profil.</span><span class="sxs-lookup"><span data-stu-id="cbde3-182">Click **Create new profile** to create another new profile.</span></span>

![Na karcie publikowanie strony możliwości aplikacji, przedstawiający FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="cbde3-184">Kreator publikowania obsługuje następujące cele publikowania:</span><span class="sxs-lookup"><span data-stu-id="cbde3-184">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="cbde3-185">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="cbde3-185">Azure App Service</span></span>
* <span data-ttu-id="cbde3-186">Usługa Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="cbde3-186">Azure Virtual Machines</span></span>
* <span data-ttu-id="cbde3-187">Usługi IIS, FTP itp., (na dowolnym serwerze sieci web)</span><span class="sxs-lookup"><span data-stu-id="cbde3-187">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="cbde3-188">Folder</span><span class="sxs-lookup"><span data-stu-id="cbde3-188">Folder</span></span>
* <span data-ttu-id="cbde3-189">Importuj profil</span><span class="sxs-lookup"><span data-stu-id="cbde3-189">Import Profile</span></span>

<span data-ttu-id="cbde3-190">Aby uzyskać więcej informacji, zobacz [jakie opcje publikowania są dla mnie odpowiednia](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="cbde3-190">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="cbde3-191">Podczas tworzenia profilu publikowania za pomocą programu Visual Studio *PublishProfiles/właściwości/&lt;nazwa_profilu&gt;.pubxml* zostanie utworzony plik MSBuild.</span><span class="sxs-lookup"><span data-stu-id="cbde3-191">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="cbde3-192">*.Pubxml* plik jest plikiem programu MSBuild i zawiera ustawienia konfiguracji publikowania.</span><span class="sxs-lookup"><span data-stu-id="cbde3-192">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="cbde3-193">Ten plik można zmienić dostosowywania kompilacji i publikowania procesu.</span><span class="sxs-lookup"><span data-stu-id="cbde3-193">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="cbde3-194">Ten plik jest odczytywany przez proces publikowania.</span><span class="sxs-lookup"><span data-stu-id="cbde3-194">This file is read by the publishing process.</span></span> <span data-ttu-id="cbde3-195">`<LastUsedBuildConfiguration>` to specjalne, ponieważ jest właściwością globalną, a nie powinien być w dowolnym pliku, który jest importowany w kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cbde3-195">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="cbde3-196">Zobacz [MSBuild: sposób ustawiania właściwości konfiguracji](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="cbde3-196">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="cbde3-197">Podczas publikowania do obiektu docelowego platformy Azure, *.pubxml* plik zawiera identyfikator swojej subskrypcji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="cbde3-197">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="cbde3-198">Z tym typem docelowym dodając ten plik do kontroli źródła nie jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="cbde3-198">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="cbde3-199">Podczas publikowania w celu spoza platformy Azure, jest bezpieczne do zaewidencjonowania *.pubxml* pliku.</span><span class="sxs-lookup"><span data-stu-id="cbde3-199">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="cbde3-200">Informacje poufne (na przykład hasło publikowania) są szyfrowane na komputerze na poziomie użytkownika/komputera.</span><span class="sxs-lookup"><span data-stu-id="cbde3-200">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="cbde3-201">Jest on przechowywany w *PublishProfiles/właściwości/&lt;nazwa_profilu&gt;. pubxml.user* pliku.</span><span class="sxs-lookup"><span data-stu-id="cbde3-201">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="cbde3-202">Ponieważ ten plik można przechowywać poufne informacje, nie powinny zostać sprawdzone w kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="cbde3-202">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="cbde3-203">Aby uzyskać omówienie sposobu publikowania aplikacji sieci web programu ASP.NET Core, zobacz [hosta i wdrażanie](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="cbde3-203">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="cbde3-204">Zadania programu MSBuild i elementy docelowe niezbędne do opublikowania aplikacji ASP.NET Core są typu open source w https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="cbde3-204">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="cbde3-205">`dotnet publish` można użyć folderu, MSDeploy, i [Kudu](https://github.com/projectkudu/kudu/wiki) profilów publikowania:</span><span class="sxs-lookup"><span data-stu-id="cbde3-205">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="cbde3-206">Folder (działa dla wielu platform):</span><span class="sxs-lookup"><span data-stu-id="cbde3-206">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="cbde3-207">Program MSDeploy (obecnie ta działa tylko w Windows, ponieważ MSDeploy nie jest dla wielu platform):</span><span class="sxs-lookup"><span data-stu-id="cbde3-207">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="cbde3-208">Pakiet narzędzia MSDeploy (obecnie ta działa tylko w Windows, ponieważ MSDeploy nie jest dla wielu platform):</span><span class="sxs-lookup"><span data-stu-id="cbde3-208">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="cbde3-209">W poprzednich przykładach **nie** przekazać `deployonbuild` do `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="cbde3-209">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="cbde3-210">Aby uzyskać więcej informacji, zobacz [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="cbde3-210">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="cbde3-211">`dotnet publish` obsługuje Kudu interfejsy API w celu publikowania na platformie Azure z dowolnej platformy.</span><span class="sxs-lookup"><span data-stu-id="cbde3-211">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="cbde3-212">Program Visual Studio publikuje obsługuje interfejsy API programu Kudu, ale jest obsługiwany przez WebSDK dla wielu platform, opublikuj na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="cbde3-212">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="cbde3-213">Dodaj profil publikowania *właściwości/PublishProfiles* folderu o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="cbde3-213">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="cbde3-214">Uruchom następujące polecenie, aby zip zapasową zawartości publikowania i opublikować go na platformie Azure przy użyciu interfejsów API Kudu:</span><span class="sxs-lookup"><span data-stu-id="cbde3-214">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="cbde3-215">Ustaw następujące właściwości programu MSBuild, korzystając z profilu publikowania:</span><span class="sxs-lookup"><span data-stu-id="cbde3-215">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="cbde3-216">Podczas publikowania za pomocą profilu o nazwie *FolderProfile*, można wykonać jedną z poniższych poleceń:</span><span class="sxs-lookup"><span data-stu-id="cbde3-216">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="cbde3-217">Podczas wywoływania [kompilacji dotnet](/dotnet/core/tools/dotnet-build), wywoływanych przez nią `msbuild` do uruchamiania kompilacji i procesu publikowania.</span><span class="sxs-lookup"><span data-stu-id="cbde3-217">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="cbde3-218">Wywołanie dowolnej `dotnet build` lub `msbuild` odpowiada podczas przekazywania w folderze profilu.</span><span class="sxs-lookup"><span data-stu-id="cbde3-218">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="cbde3-219">Podczas wywoływania MSBuild bezpośrednio na Windows, .NET Framework w wersji programu MSBuild jest używany.</span><span class="sxs-lookup"><span data-stu-id="cbde3-219">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="cbde3-220">Program MSDeploy jest obecnie ograniczona do Windows maszyny na potrzeby publikowania.</span><span class="sxs-lookup"><span data-stu-id="cbde3-220">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="cbde3-221">Wywoływanie `dotnet build` bez folderu profilu wywołuje MSBuild i program MSBuild używa MSDeploy na profile i folderów.</span><span class="sxs-lookup"><span data-stu-id="cbde3-221">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="cbde3-222">Wywoływanie `dotnet build` profil i folderów wywołuje MSBuild (przy użyciu narzędzia MSDeploy) i powoduje błąd (nawet wtedy, gdy uruchomiona na platformie Windows).</span><span class="sxs-lookup"><span data-stu-id="cbde3-222">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="cbde3-223">Aby opublikować za pomocą profilu i folderów, należy wywołać program MSBuild bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="cbde3-223">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="cbde3-224">Profil publikowania następujący folder został utworzony za pomocą programu Visual Studio i publikuje do udziału sieciowego:</span><span class="sxs-lookup"><span data-stu-id="cbde3-224">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="cbde3-225">Uwaga `<LastUsedBuildConfiguration>` ustawiono `Release`.</span><span class="sxs-lookup"><span data-stu-id="cbde3-225">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="cbde3-226">Podczas publikowania z programu Visual Studio `<LastUsedBuildConfiguration>` ustawiono wartość właściwości konfiguracji, przy użyciu wartości, gdy proces publikowania zostanie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="cbde3-226">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="cbde3-227">`<LastUsedBuildConfiguration>` Właściwość konfiguracji jest szczególna i nie powinna zostać zastąpiona w importowanym pliku MSBuild.</span><span class="sxs-lookup"><span data-stu-id="cbde3-227">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="cbde3-228">Ta właściwość może być zastąpiona w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="cbde3-228">This property can be overridden from the command line.</span></span>

<span data-ttu-id="cbde3-229">Korzystanie z platformy .NET Core interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="cbde3-229">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="cbde3-230">Korzystanie z programu MSBuild:</span><span class="sxs-lookup"><span data-stu-id="cbde3-230">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="cbde3-231">Publikowanie do punktu końcowego MSDeploy z wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="cbde3-231">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="cbde3-232">Publikowanie można osiągnąć przy użyciu interfejsu wiersza polecenia platformy .NET Core lub programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="cbde3-232">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="cbde3-233">`dotnet publish` działa w kontekście programu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cbde3-233">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="cbde3-234">`msbuild` Polecenia wymaga programu .NET Framework, która ogranicza ją do środowiska Windows.</span><span class="sxs-lookup"><span data-stu-id="cbde3-234">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="cbde3-235">Najprostszym sposobem publikowania za pomocą narzędzia MSDeploy jest najpierw utwórz profil publikowania w programie Visual Studio 2017 i Użyj profilu z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="cbde3-235">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="cbde3-236">W poniższym przykładzie utworzono aplikację internetową platformy ASP.NET Core (przy użyciu `dotnet new mvc`), a profil publikowania platformy Azure są dodawane przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cbde3-236">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="cbde3-237">Uruchom `msbuild` z **wiersz polecenia programisty dla programu VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="cbde3-237">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="cbde3-238">Wiersz polecenia dla deweloperów jest prawidłowy *msbuild.exe* w swojej ścieżce niektóre zestaw zmiennych MSBuild.</span><span class="sxs-lookup"><span data-stu-id="cbde3-238">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="cbde3-239">Program MSBuild używa następującej składni:</span><span class="sxs-lookup"><span data-stu-id="cbde3-239">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="cbde3-240">Pobierz `Password` z  *\<nazwa Publikuj >. PublishSettings* pliku.</span><span class="sxs-lookup"><span data-stu-id="cbde3-240">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="cbde3-241">Pobierz *. PublishSettings* plików z poziomu:</span><span class="sxs-lookup"><span data-stu-id="cbde3-241">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="cbde3-242">Eksplorator rozwiązań: Kliknij prawym przyciskiem myszy w aplikacji sieci Web i wybierz **Pobierz profil publikowania**.</span><span class="sxs-lookup"><span data-stu-id="cbde3-242">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="cbde3-243">Witryna Azure portal: kliknij **Pobierz profil publikowania** w usłudze Web App **Przegląd** panelu.</span><span class="sxs-lookup"><span data-stu-id="cbde3-243">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="cbde3-244">`Username` można znaleźć w profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="cbde3-244">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="cbde3-245">Następujące przykładowe używa *Web11112 — narzędzie Web Deploy* profil publikowania:</span><span class="sxs-lookup"><span data-stu-id="cbde3-245">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="cbde3-246">Wyklucz pliki</span><span class="sxs-lookup"><span data-stu-id="cbde3-246">Exclude files</span></span>

<span data-ttu-id="cbde3-247">Podczas publikowania aplikacji sieci web platformy ASP.NET Core, artefaktów kompilacji i zawartość *wwwroot* folderu są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="cbde3-247">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="cbde3-248">`msbuild` obsługuje [wzorców obsługi symboli wieloznacznych](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="cbde3-248">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="cbde3-249">Na przykład następująca `<Content>` element nie obejmuje cały tekst (*.txt*) plików ze *wwwroot/zawartości* folderze i jego podfolderach.</span><span class="sxs-lookup"><span data-stu-id="cbde3-249">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="cbde3-250">Poprzedni kod znaczników, które można dodać do profilu publikowania lub *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="cbde3-250">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="cbde3-251">Po dodaniu do *.csproj* plik, reguła zostanie dodany do wszystkich profilów publikowania w projekcie.</span><span class="sxs-lookup"><span data-stu-id="cbde3-251">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="cbde3-252">Następujące `<MsDeploySkipRules>` element wyklucza wszystkie pliki z *wwwroot/zawartości* folderu:</span><span class="sxs-lookup"><span data-stu-id="cbde3-252">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="cbde3-253">`<MsDeploySkipRules>` nie usuwa *pominąć* obiekty docelowe z witryny wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="cbde3-253">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="cbde3-254">`<Content>` docelowe pliki i foldery są usuwane z lokacji wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="cbde3-254">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="cbde3-255">Na przykład załóżmy, że wdrożonej aplikacji sieci web ma następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="cbde3-255">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="cbde3-256">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cbde3-256">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="cbde3-257">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cbde3-257">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="cbde3-258">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cbde3-258">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="cbde3-259">Jeśli następujące `<MsDeploySkipRules>` elementy są dodawane, te pliki nie zostaną usunięte w witrynie wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="cbde3-259">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="cbde3-260">Poprzedni `<MsDeploySkipRules>` elementy zapobiegania *pominięte* pliki z wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="cbde3-260">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="cbde3-261">Nie będzie go usunąć te pliki, gdy są one wdrażane.</span><span class="sxs-lookup"><span data-stu-id="cbde3-261">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="cbde3-262">Następujące `<Content>` elementu usuwa pliki docelowe lokacji wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="cbde3-262">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="cbde3-263">Za pomocą wiersza polecenia z poprzednim okresem `<Content>` element daje następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="cbde3-263">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

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

## <a name="include-files"></a><span data-ttu-id="cbde3-264">Dołącz pliki</span><span class="sxs-lookup"><span data-stu-id="cbde3-264">Include files</span></span>

<span data-ttu-id="cbde3-265">Obejmuje następujące znaczniki *obrazów* folderu poza katalogiem projektu do *wwwroot/obrazów* folderów witrynie publikowania:</span><span class="sxs-lookup"><span data-stu-id="cbde3-265">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="cbde3-266">Znaczniki mogą być dodawane do *.csproj* pliku lub profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="cbde3-266">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="cbde3-267">Jeśli jest dodawany do *.csproj* pliku, jest on zawarty w każdym profilu publikowania w projekcie.</span><span class="sxs-lookup"><span data-stu-id="cbde3-267">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="cbde3-268">Następujące wyróżnione znaczników pokazuje jak do:</span><span class="sxs-lookup"><span data-stu-id="cbde3-268">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="cbde3-269">Skopiuj plik z poza projektem do *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="cbde3-269">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="cbde3-270">Wyklucz *wwwroot\Content* folderu.</span><span class="sxs-lookup"><span data-stu-id="cbde3-270">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="cbde3-271">Wyklucz *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cbde3-271">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="cbde3-272">Zobacz [WebSDK Readme](https://github.com/aspnet/websdk) więcej przykładów wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="cbde3-272">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="cbde3-273">Uruchom element docelowy, przed lub po opublikowaniu</span><span class="sxs-lookup"><span data-stu-id="cbde3-273">Run a target before or after publishing</span></span>

<span data-ttu-id="cbde3-274">Wbudowane `BeforePublish` i `AfterPublish` celów wykonania obiektu docelowego przed lub po docelową lokalizację publikacji.</span><span class="sxs-lookup"><span data-stu-id="cbde3-274">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="cbde3-275">Dodaj następujące elementy do profilu publikowania do rejestrowania komunikatów konsoli przed i po opublikowaniu:</span><span class="sxs-lookup"><span data-stu-id="cbde3-275">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="cbde3-276">Publikowanie na serwerze za pomocą niezaufanego certyfikatu</span><span class="sxs-lookup"><span data-stu-id="cbde3-276">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="cbde3-277">Dodaj `<AllowUntrustedCertificate>` właściwość z wartością `True` profilu publikowania:</span><span class="sxs-lookup"><span data-stu-id="cbde3-277">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="cbde3-278">Usługi Kudu</span><span class="sxs-lookup"><span data-stu-id="cbde3-278">The Kudu service</span></span>

<span data-ttu-id="cbde3-279">Aby wyświetlić pliki w usłudze Azure App Service wdrożenia aplikacji sieci web, należy użyć [usługi Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="cbde3-279">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="cbde3-280">Dołącz `scm` tokenu to nazwa aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="cbde3-280">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="cbde3-281">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cbde3-281">For example:</span></span>

| <span data-ttu-id="cbde3-282">Adres URL</span><span class="sxs-lookup"><span data-stu-id="cbde3-282">URL</span></span>                                    | <span data-ttu-id="cbde3-283">Wynik</span><span class="sxs-lookup"><span data-stu-id="cbde3-283">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="cbde3-284">Aplikacja sieci Web</span><span class="sxs-lookup"><span data-stu-id="cbde3-284">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="cbde3-285">Usługi kudu</span><span class="sxs-lookup"><span data-stu-id="cbde3-285">Kudu service</span></span> |

<span data-ttu-id="cbde3-286">Wybierz [konsoli debugowania](https://github.com/projectkudu/kudu/wiki/Kudu-console) element menu, aby wyświetlać, edytować, usunąć lub dodać pliki.</span><span class="sxs-lookup"><span data-stu-id="cbde3-286">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cbde3-287">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cbde3-287">Additional resources</span></span>

* <span data-ttu-id="cbde3-288">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) upraszcza wdrażanie aplikacji sieci web i witryn sieci Web na serwerach usług IIS.</span><span class="sxs-lookup"><span data-stu-id="cbde3-288">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="cbde3-289">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Plik problemów i funkcje do wdrożenia na żądanie.</span><span class="sxs-lookup"><span data-stu-id="cbde3-289">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="cbde3-290">Publikowanie aplikacji sieci Web platformy ASP.NET na maszynie Wirtualnej platformy Azure z programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cbde3-290">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
