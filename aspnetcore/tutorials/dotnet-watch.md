---
title: Tworzenie aplikacji platformy ASP.NET Core za pomocą obserwator plików
author: rick-anderson
description: W tym samouczku pokazano, jak zainstalować i używać narzędzia obserwatora (dotnet watch) plików .NET Core interfejsu wiersza polecenia w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: f1e0d91b27df4af7cbfb6f2547c94c0370c65d0d
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207505"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="c38b0-103">Tworzenie aplikacji platformy ASP.NET Core za pomocą obserwator plików</span><span class="sxs-lookup"><span data-stu-id="c38b0-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="c38b0-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="c38b0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="c38b0-105">`dotnet watch` to narzędzie, które uruchamia [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools) polecenia, gdy zmiany plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="c38b0-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="c38b0-106">Na przykład zmianę pliku może wyzwalać kompilacja, wykonywanie testów lub wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="c38b0-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="c38b0-107">W tym samouczku jest używany istniejący interfejs API sieci web z dwoma punktami końcowymi: taki, który zwraca sumę i jedną, która zwraca produktu.</span><span class="sxs-lookup"><span data-stu-id="c38b0-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="c38b0-108">Metoda produktu zawiera usterkę, zostanie rozwiązany w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="c38b0-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="c38b0-109">Pobierz [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="c38b0-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="c38b0-110">Składa się z dwóch projektów: *Aplikacja sieci Web* (Platforma ASP.NET Core internetowego interfejsu API) i *WebAppTests* (testy jednostkowe dla interfejsu API sieci web).</span><span class="sxs-lookup"><span data-stu-id="c38b0-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="c38b0-111">W powłoce poleceń, przejdź do *aplikacji sieci Web* folderu.</span><span class="sxs-lookup"><span data-stu-id="c38b0-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="c38b0-112">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="c38b0-112">Run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="c38b0-113">Dane wyjściowe konsoli zawierają komunikaty podobne do następujących (co oznacza, że aplikacja działa i oczekujące na żądania):</span><span class="sxs-lookup"><span data-stu-id="c38b0-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="c38b0-114">W przeglądarce internetowej przejdź do `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="c38b0-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="c38b0-115">Powinien zostać wyświetlony wynik `9`.</span><span class="sxs-lookup"><span data-stu-id="c38b0-115">You should see the result of `9`.</span></span>

<span data-ttu-id="c38b0-116">Przejdź do produktu API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="c38b0-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="c38b0-117">Zwraca `9`, a nie `20` zgodnie z regułami.</span><span class="sxs-lookup"><span data-stu-id="c38b0-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="c38b0-118">Ten problem został rozwiązany w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="c38b0-118">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="c38b0-119">Dodaj `dotnet watch` do projektu</span><span class="sxs-lookup"><span data-stu-id="c38b0-119">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="c38b0-120">`dotnet watch` Pliku obserwatora narzędzie jest dołączone do wersji 2.1.300 .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="c38b0-120">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="c38b0-121">Poniższe kroki są wymagane, gdy przy użyciu wcześniejszej wersji programu .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="c38b0-121">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="c38b0-122">Dodaj `Microsoft.DotNet.Watcher.Tools` odwołanie do pakietu *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="c38b0-122">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="c38b0-123">Zainstaluj `Microsoft.DotNet.Watcher.Tools` pakietu, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="c38b0-123">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="c38b0-124">Za pomocą poleceń interfejsu wiersza polecenia platformy .NET Core. `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="c38b0-124">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="c38b0-125">Wszelkie [polecenia interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools#cli-commands) mogą być uruchamiane za pomocą `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="c38b0-125">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="c38b0-126">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c38b0-126">For example:</span></span>

| <span data-ttu-id="c38b0-127">Polecenie</span><span class="sxs-lookup"><span data-stu-id="c38b0-127">Command</span></span> | <span data-ttu-id="c38b0-128">Polecenie z zegarkiem</span><span class="sxs-lookup"><span data-stu-id="c38b0-128">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="c38b0-129">Uruchom polecenia DotNet</span><span class="sxs-lookup"><span data-stu-id="c38b0-129">dotnet run</span></span> | <span data-ttu-id="c38b0-130">Obejrzyj DotNet, uruchom</span><span class="sxs-lookup"><span data-stu-id="c38b0-130">dotnet watch run</span></span> |
| <span data-ttu-id="c38b0-131">polecenia DotNet, uruchom netcoreapp2.0 -f</span><span class="sxs-lookup"><span data-stu-id="c38b0-131">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="c38b0-132">Obejrzyj DotNet, uruchom netcoreapp2.0 -f</span><span class="sxs-lookup"><span data-stu-id="c38b0-132">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="c38b0-133">Uruchom netcoreapp2.0 -f - DotNet-arg1</span><span class="sxs-lookup"><span data-stu-id="c38b0-133">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="c38b0-134">Obejrzyj DotNet, uruchom netcoreapp2.0 -f — — arg1</span><span class="sxs-lookup"><span data-stu-id="c38b0-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="c38b0-135">polecenia DotNet test</span><span class="sxs-lookup"><span data-stu-id="c38b0-135">dotnet test</span></span> | <span data-ttu-id="c38b0-136">polecenia DotNet test wyrażenie kontrolne</span><span class="sxs-lookup"><span data-stu-id="c38b0-136">dotnet watch test</span></span> |

<span data-ttu-id="c38b0-137">Uruchom `dotnet watch run` w *aplikacji sieci Web* folderu.</span><span class="sxs-lookup"><span data-stu-id="c38b0-137">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="c38b0-138">Dane wyjściowe konsoli wskazuje `watch` została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="c38b0-138">The console output indicates `watch` has started.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="c38b0-139">Wprowadzić zmiany za pomocą `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="c38b0-139">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="c38b0-140">Upewnij się, że `dotnet watch` jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="c38b0-140">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="c38b0-141">Naprawienie usterki w poleceniu `Product` metody *MathController.cs* tak aby zwracało poprawnie produktu i nie o sumę wartości:</span><span class="sxs-lookup"><span data-stu-id="c38b0-141">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

<span data-ttu-id="c38b0-142">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="c38b0-142">Save the file.</span></span> <span data-ttu-id="c38b0-143">Dane wyjściowe konsoli wskazuje, że `dotnet watch` Wykryto zmianę pliku i ponownym uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c38b0-143">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="c38b0-144">Sprawdź `http://localhost:<port number>/api/math/product?a=4&b=5` zwraca odpowiedni wynik.</span><span class="sxs-lookup"><span data-stu-id="c38b0-144">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="c38b0-145">Uruchom testy przy użyciu `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="c38b0-145">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="c38b0-146">Zmiana `Product` metody *MathController.cs* do zwracania sumy.</span><span class="sxs-lookup"><span data-stu-id="c38b0-146">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="c38b0-147">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="c38b0-147">Save the file.</span></span>
1. <span data-ttu-id="c38b0-148">W powłoce poleceń, przejdź do *WebAppTests* folderu.</span><span class="sxs-lookup"><span data-stu-id="c38b0-148">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="c38b0-149">Uruchom [dotnet restore](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="c38b0-149">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="c38b0-150">Uruchom `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="c38b0-150">Run `dotnet watch test`.</span></span> <span data-ttu-id="c38b0-151">Dane wyjściowe wskazuje, że test nie powiódł się i o obserwatora oczekujących na zmiany w plikach:</span><span class="sxs-lookup"><span data-stu-id="c38b0-151">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="c38b0-152">Napraw `Product` metoda kodu, tak aby zwracało poprawnie produktu.</span><span class="sxs-lookup"><span data-stu-id="c38b0-152">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="c38b0-153">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="c38b0-153">Save the file.</span></span>

<span data-ttu-id="c38b0-154">`dotnet watch` wykrywa zmianę pliku i uruchomienia testów.</span><span class="sxs-lookup"><span data-stu-id="c38b0-154">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="c38b0-155">Dane wyjściowe konsoli wskazuje testy zostały zakończone pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="c38b0-155">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="c38b0-156">Dostosuj listę plików, aby obejrzeć</span><span class="sxs-lookup"><span data-stu-id="c38b0-156">Customize files list to watch</span></span>

<span data-ttu-id="c38b0-157">Domyślnie `dotnet-watch` śledzi wszystkich plików zgodnych z następujących wzorców glob:</span><span class="sxs-lookup"><span data-stu-id="c38b0-157">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="c38b0-158">Można dodać więcej elementów do listy obserwowanych przez edycję *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="c38b0-158">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="c38b0-159">Elementy można określić osobno lub za pomocą glob wzorców.</span><span class="sxs-lookup"><span data-stu-id="c38b0-159">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="c38b0-160">Zoptymalizowany pod kątem na poziomie plików, aby być obserwowane</span><span class="sxs-lookup"><span data-stu-id="c38b0-160">Opt-out of files to be watched</span></span>

<span data-ttu-id="c38b0-161">`dotnet-watch` można skonfigurować tak, aby zignorować jego domyślnych ustawień.</span><span class="sxs-lookup"><span data-stu-id="c38b0-161">`dotnet-watch` can be configured to ignore its default settings.</span></span> <span data-ttu-id="c38b0-162">Aby zignorować określonych plików, należy dodać `Watch="false"` atrybutu do definicji przedmiotu w *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="c38b0-162">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

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

## <a name="custom-watch-projects"></a><span data-ttu-id="c38b0-163">Obejrzyj niestandardowych projektów</span><span class="sxs-lookup"><span data-stu-id="c38b0-163">Custom watch projects</span></span>

<span data-ttu-id="c38b0-164">`dotnet-watch` nie jest ograniczony do projektów C#.</span><span class="sxs-lookup"><span data-stu-id="c38b0-164">`dotnet-watch` isn't restricted to C# projects.</span></span> <span data-ttu-id="c38b0-165">Obejrzyj niestandardowe projekty mogą być tworzone do obsługi różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="c38b0-165">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="c38b0-166">Należy wziąć pod uwagę następujące układ projektu:</span><span class="sxs-lookup"><span data-stu-id="c38b0-166">Consider the following project layout:</span></span>

* <span data-ttu-id="c38b0-167">**Test /**</span><span class="sxs-lookup"><span data-stu-id="c38b0-167">**test/**</span></span>
  * <span data-ttu-id="c38b0-168">*UnitTests/UnitTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="c38b0-168">*UnitTests/UnitTests.csproj*</span></span>
  * <span data-ttu-id="c38b0-169">*IntegrationTests/IntegrationTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="c38b0-169">*IntegrationTests/IntegrationTests.csproj*</span></span>

<span data-ttu-id="c38b0-170">Jeśli celem jest, aby obejrzeć oba projekty, należy utworzyć plik projektu niestandardowe skonfigurowane do obu projektów:</span><span class="sxs-lookup"><span data-stu-id="c38b0-170">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

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

<span data-ttu-id="c38b0-171">Aby uruchomić plik oglądania na obu projektów, zmienić *test* folderu.</span><span class="sxs-lookup"><span data-stu-id="c38b0-171">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="c38b0-172">Wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="c38b0-172">Execute the following command:</span></span>

```console
dotnet watch msbuild /t:Test
```

<span data-ttu-id="c38b0-173">VSTest wykonuje, gdy wszelkich zmian w plikach w dowolnym projekcie testowym.</span><span class="sxs-lookup"><span data-stu-id="c38b0-173">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="c38b0-174">`dotnet-watch` w usłudze GitHub</span><span class="sxs-lookup"><span data-stu-id="c38b0-174">`dotnet-watch` in GitHub</span></span>

<span data-ttu-id="c38b0-175">`dotnet-watch` jest częścią usługi GitHub [repozytorium aspnet/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Tools/dotnet-watch).</span><span class="sxs-lookup"><span data-stu-id="c38b0-175">`dotnet-watch` is part of the GitHub [aspnet/AspNetCore repository](https://github.com/aspnet/AspNetCore/tree/master/src/Tools/dotnet-watch).</span></span>
