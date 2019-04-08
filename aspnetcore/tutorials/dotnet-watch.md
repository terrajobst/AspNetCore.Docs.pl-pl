---
title: Tworzenie aplikacji platformy ASP.NET Core za pomocą obserwator plików
author: rick-anderson
description: W tym samouczku pokazano, jak zainstalować i używać narzędzia obserwatora (dotnet watch) plików .NET Core interfejsu wiersza polecenia w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: 40ecca1c6f9d519b24649d0c28946d95b820c07c
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068199"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="a04a8-103">Tworzenie aplikacji platformy ASP.NET Core za pomocą obserwator plików</span><span class="sxs-lookup"><span data-stu-id="a04a8-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="a04a8-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="a04a8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

`dotnet watch` <span data-ttu-id="a04a8-105">to narzędzie, które uruchamia [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools) polecenia, gdy zmiany plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="a04a8-105">is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="a04a8-106">Na przykład zmianę pliku może wyzwalać kompilacja, wykonywanie testów lub wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="a04a8-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="a04a8-107">W tym samouczku jest używany istniejący interfejs API sieci web z dwoma punktami końcowymi: taki, który zwraca sumę i jedną, która zwraca produktu.</span><span class="sxs-lookup"><span data-stu-id="a04a8-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="a04a8-108">Metoda produktu zawiera usterkę, zostanie rozwiązany w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="a04a8-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="a04a8-109">Pobierz [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="a04a8-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="a04a8-110">Składa się z dwóch projektów: *Aplikacja sieci Web* (Platforma ASP.NET Core internetowego interfejsu API) i *WebAppTests* (testy jednostkowe dla interfejsu API sieci web).</span><span class="sxs-lookup"><span data-stu-id="a04a8-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="a04a8-111">W powłoce poleceń, przejdź do *aplikacji sieci Web* folderu.</span><span class="sxs-lookup"><span data-stu-id="a04a8-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="a04a8-112">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="a04a8-112">Run the following command:</span></span>

```console
dotnet run
```

> [!NOTE]
> <span data-ttu-id="a04a8-113">Możesz użyć `dotnet run --project <PROJECT>` do określenia projektu do uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="a04a8-113">You can use `dotnet run --project <PROJECT>` to specify a project to run.</span></span> <span data-ttu-id="a04a8-114">Aby na przykład uruchomić `dotnet run --project WebApp` z katalogu głównego przykładowego aplikacji spowoduje także wykonanie *aplikacji sieci Web* projektu.</span><span class="sxs-lookup"><span data-stu-id="a04a8-114">For example, running `dotnet run --project WebApp` from the root of the sample app will also run the *WebApp* project.</span></span>

<span data-ttu-id="a04a8-115">Dane wyjściowe konsoli zawierają komunikaty podobne do następujących (co oznacza, że aplikacja działa i oczekujące na żądania):</span><span class="sxs-lookup"><span data-stu-id="a04a8-115">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="a04a8-116">W przeglądarce internetowej przejdź do `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="a04a8-116">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="a04a8-117">Powinien zostać wyświetlony wynik `9`.</span><span class="sxs-lookup"><span data-stu-id="a04a8-117">You should see the result of `9`.</span></span>

<span data-ttu-id="a04a8-118">Przejdź do produktu API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="a04a8-118">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="a04a8-119">Zwraca `9`, a nie `20` zgodnie z regułami.</span><span class="sxs-lookup"><span data-stu-id="a04a8-119">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="a04a8-120">Ten problem został rozwiązany w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="a04a8-120">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="a04a8-121">Dodaj `dotnet watch` do projektu</span><span class="sxs-lookup"><span data-stu-id="a04a8-121">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="a04a8-122">`dotnet watch` Pliku obserwatora narzędzie jest dołączone do wersji 2.1.300 .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="a04a8-122">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="a04a8-123">Poniższe kroki są wymagane, gdy przy użyciu wcześniejszej wersji programu .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="a04a8-123">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="a04a8-124">Dodaj `Microsoft.DotNet.Watcher.Tools` odwołanie do pakietu *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="a04a8-124">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="a04a8-125">Zainstaluj `Microsoft.DotNet.Watcher.Tools` pakietu, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="a04a8-125">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="a04a8-126">Za pomocą poleceń interfejsu wiersza polecenia platformy .NET Core. `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="a04a8-126">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="a04a8-127">Wszelkie [polecenia interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools#cli-commands) mogą być uruchamiane za pomocą `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="a04a8-127">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="a04a8-128">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a04a8-128">For example:</span></span>

| <span data-ttu-id="a04a8-129">Polecenie</span><span class="sxs-lookup"><span data-stu-id="a04a8-129">Command</span></span> | <span data-ttu-id="a04a8-130">Polecenie z zegarkiem</span><span class="sxs-lookup"><span data-stu-id="a04a8-130">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="a04a8-131">Uruchom polecenia DotNet</span><span class="sxs-lookup"><span data-stu-id="a04a8-131">dotnet run</span></span> | <span data-ttu-id="a04a8-132">Obejrzyj DotNet, uruchom</span><span class="sxs-lookup"><span data-stu-id="a04a8-132">dotnet watch run</span></span> |
| <span data-ttu-id="a04a8-133">polecenia DotNet, uruchom netcoreapp2.0 -f</span><span class="sxs-lookup"><span data-stu-id="a04a8-133">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="a04a8-134">Obejrzyj DotNet, uruchom netcoreapp2.0 -f</span><span class="sxs-lookup"><span data-stu-id="a04a8-134">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="a04a8-135">Uruchom netcoreapp2.0 -f - DotNet-arg1</span><span class="sxs-lookup"><span data-stu-id="a04a8-135">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="a04a8-136">Obejrzyj DotNet, uruchom netcoreapp2.0 -f — — arg1</span><span class="sxs-lookup"><span data-stu-id="a04a8-136">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="a04a8-137">polecenia DotNet test</span><span class="sxs-lookup"><span data-stu-id="a04a8-137">dotnet test</span></span> | <span data-ttu-id="a04a8-138">polecenia DotNet test wyrażenie kontrolne</span><span class="sxs-lookup"><span data-stu-id="a04a8-138">dotnet watch test</span></span> |

<span data-ttu-id="a04a8-139">Uruchom `dotnet watch run` w *aplikacji sieci Web* folderu.</span><span class="sxs-lookup"><span data-stu-id="a04a8-139">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="a04a8-140">Dane wyjściowe konsoli wskazuje `watch` została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="a04a8-140">The console output indicates `watch` has started.</span></span>

> [!NOTE]
> <span data-ttu-id="a04a8-141">Możesz użyć `dotnet watch --project <PROJECT>` do określenia projektu, aby obejrzeć.</span><span class="sxs-lookup"><span data-stu-id="a04a8-141">You can use `dotnet watch --project <PROJECT>` to specify a project to watch.</span></span> <span data-ttu-id="a04a8-142">Aby na przykład uruchomić `dotnet watch --project WebApp run` z katalogu głównego przykładowego aplikacji również zostaną uruchomione i obejrzyj *aplikacji sieci Web* projektu.</span><span class="sxs-lookup"><span data-stu-id="a04a8-142">For example, running `dotnet watch --project WebApp run` from the root of the sample app will also run and watch the *WebApp* project.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="a04a8-143">Wprowadzić zmiany za pomocą `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="a04a8-143">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="a04a8-144">Upewnij się, że `dotnet watch` jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="a04a8-144">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="a04a8-145">Naprawienie usterki w poleceniu `Product` metody *MathController.cs* tak aby zwracało poprawnie produktu i nie o sumę wartości:</span><span class="sxs-lookup"><span data-stu-id="a04a8-145">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
    return a * b;
}
```

<span data-ttu-id="a04a8-146">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="a04a8-146">Save the file.</span></span> <span data-ttu-id="a04a8-147">Dane wyjściowe konsoli wskazuje, że `dotnet watch` Wykryto zmianę pliku i ponownym uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a04a8-147">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="a04a8-148">Sprawdź `http://localhost:<port number>/api/math/product?a=4&b=5` zwraca odpowiedni wynik.</span><span class="sxs-lookup"><span data-stu-id="a04a8-148">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="a04a8-149">Uruchom testy przy użyciu `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="a04a8-149">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="a04a8-150">Zmiana `Product` metody *MathController.cs* do zwracania sumy.</span><span class="sxs-lookup"><span data-stu-id="a04a8-150">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="a04a8-151">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="a04a8-151">Save the file.</span></span>
1. <span data-ttu-id="a04a8-152">W powłoce poleceń, przejdź do *WebAppTests* folderu.</span><span class="sxs-lookup"><span data-stu-id="a04a8-152">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="a04a8-153">Uruchom [dotnet restore](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="a04a8-153">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="a04a8-154">Uruchom `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="a04a8-154">Run `dotnet watch test`.</span></span> <span data-ttu-id="a04a8-155">Dane wyjściowe wskazuje, że test nie powiódł się i o obserwatora oczekujących na zmiany w plikach:</span><span class="sxs-lookup"><span data-stu-id="a04a8-155">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="a04a8-156">Napraw `Product` metoda kodu, tak aby zwracało poprawnie produktu.</span><span class="sxs-lookup"><span data-stu-id="a04a8-156">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="a04a8-157">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="a04a8-157">Save the file.</span></span>

`dotnet watch` <span data-ttu-id="a04a8-158">wykrywa zmianę pliku i uruchomienia testów.</span><span class="sxs-lookup"><span data-stu-id="a04a8-158">detects the file change and reruns the tests.</span></span> <span data-ttu-id="a04a8-159">Dane wyjściowe konsoli wskazuje testy zostały zakończone pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="a04a8-159">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="a04a8-160">Dostosuj listę plików, aby obejrzeć</span><span class="sxs-lookup"><span data-stu-id="a04a8-160">Customize files list to watch</span></span>

<span data-ttu-id="a04a8-161">Domyślnie `dotnet-watch` śledzi wszystkich plików zgodnych z następujących wzorców glob:</span><span class="sxs-lookup"><span data-stu-id="a04a8-161">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="a04a8-162">Można dodać więcej elementów do listy obserwowanych przez edycję *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="a04a8-162">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="a04a8-163">Elementy można określić osobno lub za pomocą glob wzorców.</span><span class="sxs-lookup"><span data-stu-id="a04a8-163">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="a04a8-164">Zoptymalizowany pod kątem na poziomie plików, aby być obserwowane</span><span class="sxs-lookup"><span data-stu-id="a04a8-164">Opt-out of files to be watched</span></span>

`dotnet-watch` <span data-ttu-id="a04a8-165">można skonfigurować tak, aby zignorować jego domyślnych ustawień.</span><span class="sxs-lookup"><span data-stu-id="a04a8-165">can be configured to ignore its default settings.</span></span> <span data-ttu-id="a04a8-166">Aby zignorować określonych plików, należy dodać `Watch="false"` atrybutu do definicji przedmiotu w *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="a04a8-166">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

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

## <a name="custom-watch-projects"></a><span data-ttu-id="a04a8-167">Obejrzyj niestandardowych projektów</span><span class="sxs-lookup"><span data-stu-id="a04a8-167">Custom watch projects</span></span>

`dotnet-watch` <span data-ttu-id="a04a8-168">nie jest ograniczony do projektów C#.</span><span class="sxs-lookup"><span data-stu-id="a04a8-168">isn't restricted to C# projects.</span></span> <span data-ttu-id="a04a8-169">Obejrzyj niestandardowe projekty mogą być tworzone do obsługi różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="a04a8-169">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="a04a8-170">Należy wziąć pod uwagę następujące układ projektu:</span><span class="sxs-lookup"><span data-stu-id="a04a8-170">Consider the following project layout:</span></span>

* **<span data-ttu-id="a04a8-171">Test /</span><span class="sxs-lookup"><span data-stu-id="a04a8-171">test/</span></span>**
  * *<span data-ttu-id="a04a8-172">UnitTests/UnitTests.csproj</span><span class="sxs-lookup"><span data-stu-id="a04a8-172">UnitTests/UnitTests.csproj</span></span>*
  * *<span data-ttu-id="a04a8-173">IntegrationTests/IntegrationTests.csproj</span><span class="sxs-lookup"><span data-stu-id="a04a8-173">IntegrationTests/IntegrationTests.csproj</span></span>*

<span data-ttu-id="a04a8-174">Jeśli celem jest, aby obejrzeć oba projekty, należy utworzyć plik projektu niestandardowe skonfigurowane do obu projektów:</span><span class="sxs-lookup"><span data-stu-id="a04a8-174">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

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

<span data-ttu-id="a04a8-175">Aby uruchomić plik oglądania na obu projektów, zmienić *test* folderu.</span><span class="sxs-lookup"><span data-stu-id="a04a8-175">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="a04a8-176">Wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="a04a8-176">Execute the following command:</span></span>

```console
dotnet watch msbuild /t:Test
```

<span data-ttu-id="a04a8-177">VSTest wykonuje, gdy wszelkich zmian w plikach w dowolnym projekcie testowym.</span><span class="sxs-lookup"><span data-stu-id="a04a8-177">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="a04a8-178">`dotnet-watch` w usłudze GitHub</span><span class="sxs-lookup"><span data-stu-id="a04a8-178">`dotnet-watch` in GitHub</span></span>

`dotnet-watch` <span data-ttu-id="a04a8-179">jest częścią usługi GitHub [repozytorium aspnet/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Tools/dotnet-watch).</span><span class="sxs-lookup"><span data-stu-id="a04a8-179">is part of the GitHub [aspnet/AspNetCore repository](https://github.com/aspnet/AspNetCore/tree/master/src/Tools/dotnet-watch).</span></span>
