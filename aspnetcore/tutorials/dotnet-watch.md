---
title: Tworzenie aplikacji platformy ASP.NET Core za pomocą monitora plików
author: rick-anderson
description: Ten samouczek pokazuje, jak zainstalować i używać narzędzia obserwatora (dotnet czujki) pliku .NET Core CLI w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: 2a59267b36faf1e00ea2f0cc7e2b9ceb9828f791
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278856"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="43cfe-103">Tworzenie aplikacji platformy ASP.NET Core za pomocą monitora plików</span><span class="sxs-lookup"><span data-stu-id="43cfe-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="43cfe-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Hurdugaci zwycięzcę](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="43cfe-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="43cfe-105">`dotnet watch` to narzędzie, które uruchamia [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools) poleceń podczas zmiany plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="43cfe-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="43cfe-106">Na przykład zmianę pliku może wyzwolić kompilację, wykonywania testów lub wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="43cfe-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="43cfe-107">W tym samouczku korzysta z istniejącej interfejsu API sieci web z dwa punkty końcowe:, który zwraca sumę i zwracające produktu.</span><span class="sxs-lookup"><span data-stu-id="43cfe-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="43cfe-108">Metoda produktu ma usterki, które zawartych w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="43cfe-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="43cfe-109">Pobierz [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="43cfe-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="43cfe-110">Zawiera dwa projekty: *aplikacji sieci Web* (platformy ASP.NET Core interfejsu API sieci web) i *WebAppTests* (testów jednostkowych dla interfejsu API sieci web).</span><span class="sxs-lookup"><span data-stu-id="43cfe-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="43cfe-111">W powłoce poleceń, przejdź do *aplikacji sieci Web* folderu.</span><span class="sxs-lookup"><span data-stu-id="43cfe-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="43cfe-112">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="43cfe-112">Run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="43cfe-113">Dane wyjściowe konsoli przedstawia komunikaty podobne do następujących (co oznacza, że aplikacja jest uruchomiona i oczekujące na żądania):</span><span class="sxs-lookup"><span data-stu-id="43cfe-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="43cfe-114">W przeglądarce sieci web, przejdź do `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="43cfe-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="43cfe-115">Należy sprawdzić działanie `9`.</span><span class="sxs-lookup"><span data-stu-id="43cfe-115">You should see the result of `9`.</span></span>

<span data-ttu-id="43cfe-116">Przejdź do produktu interfejsu API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="43cfe-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="43cfe-117">Zwraca `9`, a nie `20` zgodnie z regułami.</span><span class="sxs-lookup"><span data-stu-id="43cfe-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="43cfe-118">Ten problem został rozwiązany później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="43cfe-118">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="43cfe-119">Dodaj `dotnet watch` do projektu</span><span class="sxs-lookup"><span data-stu-id="43cfe-119">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="43cfe-120">`dotnet watch` Narzędzia obserwatora plików są dołączone do wersji 2.1.300 zestawu SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="43cfe-120">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="43cfe-121">Poniższe kroki są wymagane w przypadku wcześniejszych wersji programu .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="43cfe-121">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="43cfe-122">Dodaj `Microsoft.DotNet.Watcher.Tools` odwołanie do pakietu *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="43cfe-122">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="43cfe-123">Zainstaluj `Microsoft.DotNet.Watcher.Tools` pakietu, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="43cfe-123">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="43cfe-124">Uruchom przy użyciu polecenia interfejsu wiersza polecenia platformy .NET Core `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="43cfe-124">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="43cfe-125">Wszelkie [polecenia interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools#cli-commands) może być uruchamiane przy `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="43cfe-125">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="43cfe-126">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="43cfe-126">For example:</span></span>

| <span data-ttu-id="43cfe-127">Polecenie</span><span class="sxs-lookup"><span data-stu-id="43cfe-127">Command</span></span> | <span data-ttu-id="43cfe-128">Polecenie z czujki</span><span class="sxs-lookup"><span data-stu-id="43cfe-128">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="43cfe-129">Uruchom DotNet</span><span class="sxs-lookup"><span data-stu-id="43cfe-129">dotnet run</span></span> | <span data-ttu-id="43cfe-130">Uruchom czujki DotNet</span><span class="sxs-lookup"><span data-stu-id="43cfe-130">dotnet watch run</span></span> |
| <span data-ttu-id="43cfe-131">Uruchom netcoreapp2.0 -f DotNet</span><span class="sxs-lookup"><span data-stu-id="43cfe-131">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="43cfe-132">Uruchom netcoreapp2.0 -f czujki DotNet</span><span class="sxs-lookup"><span data-stu-id="43cfe-132">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="43cfe-133">Uruchom netcoreapp2.0 -f - DotNet — arg1</span><span class="sxs-lookup"><span data-stu-id="43cfe-133">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="43cfe-134">Obejrzyj DotNet Uruchom netcoreapp2.0 -f ---arg1</span><span class="sxs-lookup"><span data-stu-id="43cfe-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="43cfe-135">DotNet test</span><span class="sxs-lookup"><span data-stu-id="43cfe-135">dotnet test</span></span> | <span data-ttu-id="43cfe-136">test czujki DotNet</span><span class="sxs-lookup"><span data-stu-id="43cfe-136">dotnet watch test</span></span> |

<span data-ttu-id="43cfe-137">Uruchom `dotnet watch run` w *aplikacji sieci Web* folderu.</span><span class="sxs-lookup"><span data-stu-id="43cfe-137">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="43cfe-138">Dane wyjściowe konsoli wskazuje `watch` została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="43cfe-138">The console output indicates `watch` has started.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="43cfe-139">Wprowadź zmiany z `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="43cfe-139">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="43cfe-140">Upewnij się, że `dotnet watch` jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="43cfe-140">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="43cfe-141">Napraw błąd w `Product` metody *MathController.cs* tak aby zwracało produktu, a nie ich sumę:</span><span class="sxs-lookup"><span data-stu-id="43cfe-141">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

<span data-ttu-id="43cfe-142">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="43cfe-142">Save the file.</span></span> <span data-ttu-id="43cfe-143">Dane wyjściowe konsoli wskazuje, że `dotnet watch` Wykryto zmianę pliku i ponownym uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43cfe-143">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="43cfe-144">Sprawdź `http://localhost:<port number>/api/math/product?a=4&b=5` zwraca prawidłowego wyniku.</span><span class="sxs-lookup"><span data-stu-id="43cfe-144">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="43cfe-145">Uruchom testy przy użyciu `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="43cfe-145">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="43cfe-146">Zmień `Product` metody *MathController.cs* do zwracania suma.</span><span class="sxs-lookup"><span data-stu-id="43cfe-146">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="43cfe-147">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="43cfe-147">Save the file.</span></span>
1. <span data-ttu-id="43cfe-148">W powłoce poleceń, przejdź do *WebAppTests* folderu.</span><span class="sxs-lookup"><span data-stu-id="43cfe-148">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="43cfe-149">Uruchom [przywracania dotnet](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="43cfe-149">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="43cfe-150">Run `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="43cfe-150">Run `dotnet watch test`.</span></span> <span data-ttu-id="43cfe-151">Dane wyjściowe wskazuje, że testowanie nie powiodło się i że obserwatora oczekuje na zmiany w pliku:</span><span class="sxs-lookup"><span data-stu-id="43cfe-151">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="43cfe-152">Usuń `Product` metody kod, tak aby zwracało produktu.</span><span class="sxs-lookup"><span data-stu-id="43cfe-152">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="43cfe-153">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="43cfe-153">Save the file.</span></span>

<span data-ttu-id="43cfe-154">`dotnet watch` wykrywa zmiany pliku i zwracające testy.</span><span class="sxs-lookup"><span data-stu-id="43cfe-154">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="43cfe-155">Dane wyjściowe konsoli wskazuje testy zostały zakończone pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="43cfe-155">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="43cfe-156">Dostosowywanie listy plików do monitorowania</span><span class="sxs-lookup"><span data-stu-id="43cfe-156">Customize files list to watch</span></span>

<span data-ttu-id="43cfe-157">Domyślnie `dotnet-watch` śledzi wszystkie pliki następujące wzorce glob dopasowywania:</span><span class="sxs-lookup"><span data-stu-id="43cfe-157">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="43cfe-158">Można dodać więcej elementów do listy czujki, edytując *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="43cfe-158">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="43cfe-159">Można określić elementów pojedynczo lub za pomocą glob wzorce.</span><span class="sxs-lookup"><span data-stu-id="43cfe-159">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="43cfe-160">Wypisz pliki, aby być monitorowane</span><span class="sxs-lookup"><span data-stu-id="43cfe-160">Opt-out of files to be watched</span></span>

<span data-ttu-id="43cfe-161">`dotnet-watch` można skonfigurować tak, aby zignorować jego domyślnych ustawień.</span><span class="sxs-lookup"><span data-stu-id="43cfe-161">`dotnet-watch` can be configured to ignore its default settings.</span></span> <span data-ttu-id="43cfe-162">Ignorowanie określonych plików, dodawanie `Watch="false"` atrybutu do definicji elementu w *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="43cfe-162">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

```xml
<ItemGroup>
    <!-- exclude Generated.cs from dotnet-watch -->
    <Compile Include="Generated.cs" Watch="false" />

    <!-- exclude Strings.resx from dotnet-watch -->
    <EmbeddedResource Include="Strings.resx" Watch="false" />

    <!-- exclude changes in this referenced project -->
    <ProjectReference Include="..\ClassLibrary1\ClassLibrary1.csproj" Watch="false" />
</ItemGroup>
```

## <a name="custom-watch-projects"></a><span data-ttu-id="43cfe-163">Projekty niestandardowych czujki</span><span class="sxs-lookup"><span data-stu-id="43cfe-163">Custom watch projects</span></span>

<span data-ttu-id="43cfe-164">`dotnet-watch` nie jest ograniczony do projektów C#.</span><span class="sxs-lookup"><span data-stu-id="43cfe-164">`dotnet-watch` isn't restricted to C# projects.</span></span> <span data-ttu-id="43cfe-165">Można tworzyć projektów niestandardowych czujki do obsługi różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="43cfe-165">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="43cfe-166">Należy wziąć pod uwagę następujące układ projektu:</span><span class="sxs-lookup"><span data-stu-id="43cfe-166">Consider the following project layout:</span></span>

* <span data-ttu-id="43cfe-167">**Testowanie /**</span><span class="sxs-lookup"><span data-stu-id="43cfe-167">**test/**</span></span>
  * <span data-ttu-id="43cfe-168">*UnitTests/UnitTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="43cfe-168">*UnitTests/UnitTests.csproj*</span></span>
  * <span data-ttu-id="43cfe-169">*IntegrationTests/IntegrationTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="43cfe-169">*IntegrationTests/IntegrationTests.csproj*</span></span>

<span data-ttu-id="43cfe-170">Jeśli celem jest Obejrzyj oba projekty, Utwórz plik projektu niestandardowe skonfigurowane do wykrywania oba projekty:</span><span class="sxs-lookup"><span data-stu-id="43cfe-170">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

```xml
<Project>
    <ItemGroup>
        <TestProjects Include="**\*.csproj" />
        <Watch Include="**\*.cs" />
    </ItemGroup>

    <Target Name="Test">
        <MSBuild Targets="VSTest" Projects="@(TestProjects)" />
    </Target>

    <Import Project="$(MSBuildExtensionsPath)\Microsoft.Common.targets" />
</Project>
```

<span data-ttu-id="43cfe-171">Aby uruchomić plik obserwowanie na obu projektów, zmienić *test* folderu.</span><span class="sxs-lookup"><span data-stu-id="43cfe-171">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="43cfe-172">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="43cfe-172">Execute the following command:</span></span>

```console
dotnet watch msbuild /t:Test
```

<span data-ttu-id="43cfe-173">VSTest wykonuje po zmianie dowolnego pliku w każdym projekcie testowym.</span><span class="sxs-lookup"><span data-stu-id="43cfe-173">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="43cfe-174">`dotnet-watch` w witrynie GitHub</span><span class="sxs-lookup"><span data-stu-id="43cfe-174">`dotnet-watch` in GitHub</span></span>

<span data-ttu-id="43cfe-175">`dotnet-watch` wchodzi w skład GitHub [repozytorium DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span><span class="sxs-lookup"><span data-stu-id="43cfe-175">`dotnet-watch` is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>
