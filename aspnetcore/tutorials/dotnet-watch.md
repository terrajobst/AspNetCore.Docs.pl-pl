---
title: Opracowywanie aplikacji ASP.NET Core przy użyciu obserwatora plików
author: rick-anderson
description: W tym samouczku pokazano, jak zainstalować i używać narzędzia obserwatora plików interfejs wiersza polecenia platformy .NET Core (czujka dotnet) w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: a2a0bcdace67052975630f99aab23bbb0fd99bff
ms.sourcegitcommit: 383017d7060a6d58f6a79cf4d7335d5b4b6c5659
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72816137"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="e0566-103">Opracowywanie aplikacji ASP.NET Core przy użyciu obserwatora plików</span><span class="sxs-lookup"><span data-stu-id="e0566-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="e0566-104">Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="e0566-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="e0566-105">[czujka dotnet](https://www.nuget.org/packages/dotnet-watch) jest narzędziem, które uruchamia [interfejs wiersza polecenia platformy .NET Core](/dotnet/core/tools) polecenie, gdy pliki źródłowe zmieniają się.</span><span class="sxs-lookup"><span data-stu-id="e0566-105">[dotnet watch](https://www.nuget.org/packages/dotnet-watch) is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="e0566-106">Na przykład zmiana pliku może wyzwolić kompilację, wykonanie testu lub wdrożenie.</span><span class="sxs-lookup"><span data-stu-id="e0566-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="e0566-107">W tym samouczku jest wykorzystywany istniejący internetowy interfejs API z dwoma punktami końcowymi: jeden, który zwraca sumę i jeden z nich zwraca produkt.</span><span class="sxs-lookup"><span data-stu-id="e0566-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="e0566-108">Metoda produktu zawiera usterkę, która została rozwiązana w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="e0566-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="e0566-109">Pobierz [przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="e0566-109">Download the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="e0566-110">Składa się z dwóch projektów: *webapp* (ASP.NET Core Web API) i *WebAppTests* (testy jednostkowe dla internetowego interfejsu API).</span><span class="sxs-lookup"><span data-stu-id="e0566-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="e0566-111">W powłoce poleceń przejdź do folderu *webapp* .</span><span class="sxs-lookup"><span data-stu-id="e0566-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="e0566-112">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="e0566-112">Run the following command:</span></span>

```dotnetcli
dotnet run
```

> [!NOTE]
> <span data-ttu-id="e0566-113">Możesz użyć `dotnet run --project <PROJECT>`, aby określić projekt do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="e0566-113">You can use `dotnet run --project <PROJECT>` to specify a project to run.</span></span> <span data-ttu-id="e0566-114">Na przykład uruchomienie `dotnet run --project WebApp` z poziomu głównego przykładowej aplikacji spowoduje również uruchomienie projektu *webapp* .</span><span class="sxs-lookup"><span data-stu-id="e0566-114">For example, running `dotnet run --project WebApp` from the root of the sample app will also run the *WebApp* project.</span></span>

<span data-ttu-id="e0566-115">W danych wyjściowych konsoli są wyświetlane komunikaty podobne do następujących (wskazuje, że aplikacja działa i oczekuje na żądania):</span><span class="sxs-lookup"><span data-stu-id="e0566-115">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="e0566-116">W przeglądarce sieci Web przejdź do `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="e0566-116">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="e0566-117">Powinien zostać wyświetlony wynik `9`.</span><span class="sxs-lookup"><span data-stu-id="e0566-117">You should see the result of `9`.</span></span>

<span data-ttu-id="e0566-118">Przejdź do interfejsu API produktu (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="e0566-118">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="e0566-119">Zwraca `9`, nie `20` zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="e0566-119">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="e0566-120">Ten problem został rozwiązany w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="e0566-120">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="e0566-121">Dodawanie `dotnet watch` do projektu</span><span class="sxs-lookup"><span data-stu-id="e0566-121">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="e0566-122">Narzędzie obserwatora plików `dotnet watch` jest dołączone do wersji 2.1.300 zestaw .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="e0566-122">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="e0566-123">Poniższe kroki są wymagane w przypadku korzystania ze starszej wersji zestaw .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="e0566-123">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="e0566-124">Dodaj odwołanie do pakietu `Microsoft.DotNet.Watcher.Tools` do pliku *csproj* :</span><span class="sxs-lookup"><span data-stu-id="e0566-124">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="e0566-125">Zainstaluj pakiet `Microsoft.DotNet.Watcher.Tools`, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="e0566-125">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```dotnetcli
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="e0566-126">Uruchamianie poleceń interfejs wiersza polecenia platformy .NET Core przy użyciu `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="e0566-126">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="e0566-127">Każde [polecenie interfejs wiersza polecenia platformy .NET Core](/dotnet/core/tools#cli-commands) można uruchomić z `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="e0566-127">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="e0566-128">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e0566-128">For example:</span></span>

| <span data-ttu-id="e0566-129">Polecenie</span><span class="sxs-lookup"><span data-stu-id="e0566-129">Command</span></span> | <span data-ttu-id="e0566-130">Polecenie z zegarkiem</span><span class="sxs-lookup"><span data-stu-id="e0566-130">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="e0566-131">dotnet run</span><span class="sxs-lookup"><span data-stu-id="e0566-131">dotnet run</span></span> | <span data-ttu-id="e0566-132">uruchomienie czujka dotnet</span><span class="sxs-lookup"><span data-stu-id="e0566-132">dotnet watch run</span></span> |
| <span data-ttu-id="e0566-133">uruchomienie dotnet-f netcoreapp 2.0</span><span class="sxs-lookup"><span data-stu-id="e0566-133">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="e0566-134">uruchomienie czujka dotnet-f netcoreapp 2.0</span><span class="sxs-lookup"><span data-stu-id="e0566-134">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="e0566-135">uruchomienie dotnet-f netcoreapp 2.0----arg1</span><span class="sxs-lookup"><span data-stu-id="e0566-135">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="e0566-136">uruchomienie czujka dotnet-f netcoreapp 2.0----arg1</span><span class="sxs-lookup"><span data-stu-id="e0566-136">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="e0566-137">dotnet test</span><span class="sxs-lookup"><span data-stu-id="e0566-137">dotnet test</span></span> | <span data-ttu-id="e0566-138">Test czujki dotnet</span><span class="sxs-lookup"><span data-stu-id="e0566-138">dotnet watch test</span></span> |

<span data-ttu-id="e0566-139">Uruchom `dotnet watch run` w folderze *webapp* .</span><span class="sxs-lookup"><span data-stu-id="e0566-139">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="e0566-140">Dane wyjściowe konsoli wskazują, `watch` zostało uruchomione.</span><span class="sxs-lookup"><span data-stu-id="e0566-140">The console output indicates `watch` has started.</span></span>

> [!NOTE]
> <span data-ttu-id="e0566-141">Możesz użyć `dotnet watch --project <PROJECT>`, aby określić projekt do obejrzenia.</span><span class="sxs-lookup"><span data-stu-id="e0566-141">You can use `dotnet watch --project <PROJECT>` to specify a project to watch.</span></span> <span data-ttu-id="e0566-142">Na przykład uruchomienie `dotnet watch --project WebApp run` z poziomu głównego przykładowej aplikacji spowoduje również uruchomienie i obejrzyj projekt *webapp* .</span><span class="sxs-lookup"><span data-stu-id="e0566-142">For example, running `dotnet watch --project WebApp run` from the root of the sample app will also run and watch the *WebApp* project.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="e0566-143">Wprowadzanie zmian przy użyciu `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="e0566-143">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="e0566-144">Upewnij się, że `dotnet watch` jest uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="e0566-144">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="e0566-145">Usuń usterkę w `Product` metodzie *MathController.cs* , aby zwracała produkt, a nie sumę:</span><span class="sxs-lookup"><span data-stu-id="e0566-145">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
    return a * b;
}
```

<span data-ttu-id="e0566-146">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="e0566-146">Save the file.</span></span> <span data-ttu-id="e0566-147">Dane wyjściowe konsoli wskazują, że `dotnet watch` wykryła zmianę pliku i ponownie uruchomiła aplikację.</span><span class="sxs-lookup"><span data-stu-id="e0566-147">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="e0566-148">Sprawdź, czy `http://localhost:<port number>/api/math/product?a=4&b=5` zwraca prawidłowy wynik.</span><span class="sxs-lookup"><span data-stu-id="e0566-148">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="e0566-149">Uruchom testy przy użyciu `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="e0566-149">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="e0566-150">Zmień metodę `Product` *MathController.cs* z powrotem, aby zwracać sumę.</span><span class="sxs-lookup"><span data-stu-id="e0566-150">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="e0566-151">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="e0566-151">Save the file.</span></span>
1. <span data-ttu-id="e0566-152">W powłoce poleceń przejdź do folderu *WebAppTests* .</span><span class="sxs-lookup"><span data-stu-id="e0566-152">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="e0566-153">Uruchom [dotnet Restore](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="e0566-153">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="e0566-154">Uruchom `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="e0566-154">Run `dotnet watch test`.</span></span> <span data-ttu-id="e0566-155">Dane wyjściowe wskazują, że test zakończył się niepowodzeniem i że obserwator oczekuje na zmiany plików:</span><span class="sxs-lookup"><span data-stu-id="e0566-155">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="e0566-156">Popraw kod metody `Product`, aby zwracał produkt.</span><span class="sxs-lookup"><span data-stu-id="e0566-156">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="e0566-157">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="e0566-157">Save the file.</span></span>

<span data-ttu-id="e0566-158">`dotnet watch` wykrywa zmianę pliku i ponownie uruchamia testy.</span><span class="sxs-lookup"><span data-stu-id="e0566-158">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="e0566-159">Dane wyjściowe konsoli wskazują zakończono testy.</span><span class="sxs-lookup"><span data-stu-id="e0566-159">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="e0566-160">Dostosuj listę plików do czujki</span><span class="sxs-lookup"><span data-stu-id="e0566-160">Customize files list to watch</span></span>

<span data-ttu-id="e0566-161">Domyślnie program `dotnet-watch` śledzi wszystkie pliki zgodne z następującymi wzorcami globalizowania:</span><span class="sxs-lookup"><span data-stu-id="e0566-161">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="e0566-162">Więcej elementów można dodać do listy obserwacje, edytując plik *csproj* .</span><span class="sxs-lookup"><span data-stu-id="e0566-162">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="e0566-163">Elementy można określać pojedynczo lub przy użyciu wzorców globalizowania.</span><span class="sxs-lookup"><span data-stu-id="e0566-163">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="e0566-164">Rezygnacja z plików, które mają być obserwowane</span><span class="sxs-lookup"><span data-stu-id="e0566-164">Opt-out of files to be watched</span></span>

<span data-ttu-id="e0566-165">`dotnet-watch` można skonfigurować do ignorowania ustawień domyślnych.</span><span class="sxs-lookup"><span data-stu-id="e0566-165">`dotnet-watch` can be configured to ignore its default settings.</span></span> <span data-ttu-id="e0566-166">Aby zignorować określone pliki, Dodaj atrybut `Watch="false"` do definicji elementu w pliku *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="e0566-166">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

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

## <a name="custom-watch-projects"></a><span data-ttu-id="e0566-167">Niestandardowe projekty czujki</span><span class="sxs-lookup"><span data-stu-id="e0566-167">Custom watch projects</span></span>

<span data-ttu-id="e0566-168">`dotnet-watch` nie jest ograniczony C# do projektów.</span><span class="sxs-lookup"><span data-stu-id="e0566-168">`dotnet-watch` isn't restricted to C# projects.</span></span> <span data-ttu-id="e0566-169">Niestandardowe projekty czujki mogą być tworzone w celu obsługi różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="e0566-169">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="e0566-170">Rozważmy następujący układ projektu:</span><span class="sxs-lookup"><span data-stu-id="e0566-170">Consider the following project layout:</span></span>

* <span data-ttu-id="e0566-171">**badan**</span><span class="sxs-lookup"><span data-stu-id="e0566-171">**test/**</span></span>
  * <span data-ttu-id="e0566-172">*UnitTests/UnitTests. csproj*</span><span class="sxs-lookup"><span data-stu-id="e0566-172">*UnitTests/UnitTests.csproj*</span></span>
  * <span data-ttu-id="e0566-173">*IntegrationTests/IntegrationTests. csproj*</span><span class="sxs-lookup"><span data-stu-id="e0566-173">*IntegrationTests/IntegrationTests.csproj*</span></span>

<span data-ttu-id="e0566-174">Jeśli celem jest oglądanie obu projektów, Utwórz niestandardowy plik projektu, który został skonfigurowany do oglądania obu projektów:</span><span class="sxs-lookup"><span data-stu-id="e0566-174">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

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

<span data-ttu-id="e0566-175">Aby rozpocząć obserwowanie plików w obu projektach, przejdź do folderu *testowego* .</span><span class="sxs-lookup"><span data-stu-id="e0566-175">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="e0566-176">Wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="e0566-176">Execute the following command:</span></span>

```dotnetcli
dotnet watch msbuild /t:Test
```

<span data-ttu-id="e0566-177">VSTest wykonuje się, gdy jakiekolwiek zmiany pliku w każdym projekcie testowym.</span><span class="sxs-lookup"><span data-stu-id="e0566-177">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="e0566-178">`dotnet-watch` w serwisie GitHub</span><span class="sxs-lookup"><span data-stu-id="e0566-178">`dotnet-watch` in GitHub</span></span>

<span data-ttu-id="e0566-179">`dotnet-watch` jest częścią [repozytorium ASPNET/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Tools/dotnet-watch)usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="e0566-179">`dotnet-watch` is part of the GitHub [aspnet/AspNetCore repository](https://github.com/aspnet/AspNetCore/tree/master/src/Tools/dotnet-watch).</span></span>
