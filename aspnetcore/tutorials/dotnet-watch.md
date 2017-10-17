---
title: "Tworzenie aplikacji platformy ASP.NET Core za pomocą czujki dotnet"
author: rick-anderson
description: "Ten samouczek pokazuje, jak zainstalować i używać narzędzia obserwatora (dotnet czujki) pliku .NET Core CLI w aplikacji platformy ASP.NET Core."
keywords: "Platformy ASP.NET Core, przy użyciu czujki dotnet"
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 9baf2ce2a1270a728616a8a2ab45deca9a9cde6f
ms.sourcegitcommit: e7f01a649f240b6b57118c53314ab82f7f36f2eb
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/06/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="70965-104">Tworzenie aplikacji platformy ASP.NET Core za pomocą czujki dotnet</span><span class="sxs-lookup"><span data-stu-id="70965-104">Developing ASP.NET Core apps using dotnet watch</span></span>

<span data-ttu-id="70965-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Hurdugaci zwycięzcę](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="70965-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="70965-106">`dotnet watch`to narzędzie, które uruchamia [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools) poleceń podczas zmiany plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="70965-106">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="70965-107">Na przykład zmianę pliku może wyzwolić kompilację, wykonywania testów lub wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="70965-107">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="70965-108">W tym samouczku używamy istniejącej aplikacji interfejsu API sieci Web z dwa punkty końcowe:, który zwraca sumę i zwracające produktu.</span><span class="sxs-lookup"><span data-stu-id="70965-108">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="70965-109">Metoda produktu zawiera usterkę, która będzie możemy naprawić w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="70965-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="70965-110">Pobierz [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="70965-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="70965-111">Zawiera dwa projekty: *aplikacji sieci Web* (platformy ASP.NET Core interfejsu API sieci Web) i *WebAppTests* (testów jednostkowych dla interfejsu API sieci Web).</span><span class="sxs-lookup"><span data-stu-id="70965-111">It contains two projects: *WebApp* (an ASP.NET Core Web API) and *WebAppTests* (unit tests for the Web API).</span></span>

<span data-ttu-id="70965-112">W powłoce poleceń, przejdź do *aplikacji sieci Web* folder i uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="70965-112">In a command shell, navigate to the *WebApp* folder and run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="70965-113">Dane wyjściowe konsoli przedstawia komunikaty podobne do następujących (co oznacza, że aplikacja jest uruchomiona i oczekujące na żądania):</span><span class="sxs-lookup"><span data-stu-id="70965-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="70965-114">W przeglądarce sieci web, przejdź do `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="70965-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="70965-115">Należy sprawdzić działanie `9`.</span><span class="sxs-lookup"><span data-stu-id="70965-115">You should see the result of `9`.</span></span>

<span data-ttu-id="70965-116">Przejdź do produktu interfejsu API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="70965-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="70965-117">Zwraca `9`, a nie `20` zgodnie z regułami.</span><span class="sxs-lookup"><span data-stu-id="70965-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="70965-118">Firma Microsoft będzie rozwiązać ten problem, później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="70965-118">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="70965-119">Dodaj `dotnet watch` do projektu</span><span class="sxs-lookup"><span data-stu-id="70965-119">Add `dotnet watch` to a project</span></span>

1. <span data-ttu-id="70965-120">Dodaj `Microsoft.DotNet.Watcher.Tools` odwołanie do pakietu *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="70965-120">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. <span data-ttu-id="70965-121">Zainstaluj `Microsoft.DotNet.Watcher.Tools` pakietu, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="70965-121">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="70965-122">Za pomocą polecenia interfejsu wiersza polecenia platformy .NET Core.`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="70965-122">Running .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="70965-123">Wszelkie [polecenia interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools#cli-commands) może być uruchamiane przy `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="70965-123">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="70965-124">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="70965-124">For example:</span></span>

| <span data-ttu-id="70965-125">Polecenie</span><span class="sxs-lookup"><span data-stu-id="70965-125">Command</span></span> | <span data-ttu-id="70965-126">Polecenie z czujki</span><span class="sxs-lookup"><span data-stu-id="70965-126">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="70965-127">Uruchom DotNet</span><span class="sxs-lookup"><span data-stu-id="70965-127">dotnet run</span></span> | <span data-ttu-id="70965-128">Uruchom czujki DotNet</span><span class="sxs-lookup"><span data-stu-id="70965-128">dotnet watch run</span></span> |
| <span data-ttu-id="70965-129">Uruchom netcoreapp2.0 -f DotNet</span><span class="sxs-lookup"><span data-stu-id="70965-129">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="70965-130">Uruchom netcoreapp2.0 -f czujki DotNet</span><span class="sxs-lookup"><span data-stu-id="70965-130">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="70965-131">Uruchom netcoreapp2.0 -f - DotNet — arg1</span><span class="sxs-lookup"><span data-stu-id="70965-131">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="70965-132">Obejrzyj DotNet Uruchom netcoreapp2.0 -f ---arg1</span><span class="sxs-lookup"><span data-stu-id="70965-132">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="70965-133">DotNet test</span><span class="sxs-lookup"><span data-stu-id="70965-133">dotnet test</span></span> | <span data-ttu-id="70965-134">test czujki DotNet</span><span class="sxs-lookup"><span data-stu-id="70965-134">dotnet watch test</span></span> |

<span data-ttu-id="70965-135">Uruchom `dotnet watch run` w *aplikacji sieci Web* folderu.</span><span class="sxs-lookup"><span data-stu-id="70965-135">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="70965-136">Dane wyjściowe konsoli wskazuje `watch` została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="70965-136">The console output indicates `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="70965-137">Wprowadzanie zmian z`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="70965-137">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="70965-138">Upewnij się, że `dotnet watch` jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="70965-138">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="70965-139">Napraw błąd w `Product` metody *MathController.cs* tak aby zwracało produktu, a nie ich sumę:</span><span class="sxs-lookup"><span data-stu-id="70965-139">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="70965-140">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="70965-140">Save the file.</span></span> <span data-ttu-id="70965-141">Dane wyjściowe konsoli wskazuje, że `dotnet watch` Wykryto zmianę pliku i ponownym uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="70965-141">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="70965-142">Sprawdź `http://localhost:<port number>/api/math/product?a=4&b=5` zwraca prawidłowego wyniku.</span><span class="sxs-lookup"><span data-stu-id="70965-142">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="70965-143">Uruchamianie testów za pomocą`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="70965-143">Running tests using `dotnet watch`</span></span>

1. <span data-ttu-id="70965-144">Zmień `Product` metody *MathController.cs* z powrotem do zwracania Suma i Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="70965-144">Change the `Product` method of *MathController.cs* back to returning the sum and save the file.</span></span>
1. <span data-ttu-id="70965-145">W powłoce poleceń, przejdź do *WebAppTests* folderu.</span><span class="sxs-lookup"><span data-stu-id="70965-145">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="70965-146">Run `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="70965-146">Run `dotnet restore`.</span></span>
1. <span data-ttu-id="70965-147">Run `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="70965-147">Run `dotnet watch test`.</span></span> <span data-ttu-id="70965-148">Dane wyjściowe wskazuje, że testowanie nie powiodło się i tym obserwatora oczekuje na zmiany w pliku:</span><span class="sxs-lookup"><span data-stu-id="70965-148">Its output indicates that a test failed and that watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="70965-149">Usuń `Product` metody kod, tak aby zwracało produktu.</span><span class="sxs-lookup"><span data-stu-id="70965-149">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="70965-150">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="70965-150">Save the file.</span></span>

<span data-ttu-id="70965-151">`dotnet watch`wykrywa zmiany pliku i zwracające testy.</span><span class="sxs-lookup"><span data-stu-id="70965-151">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="70965-152">Dane wyjściowe konsoli wskazuje testy zostały zakończone pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="70965-152">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="70965-153">wyrażenie kontrolne DotNet w serwisie GitHub</span><span class="sxs-lookup"><span data-stu-id="70965-153">dotnet-watch in GitHub</span></span>

<span data-ttu-id="70965-154">Obejrzyj DotNet jest częścią usługi GitHub [repozytorium DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span><span class="sxs-lookup"><span data-stu-id="70965-154">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span></span>

<span data-ttu-id="70965-155">[Sekcji MSBuild](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) z [ReadMe czujki dotnet](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) wyjaśniono, jak czujki dotnet można skonfigurować w pliku projektu MSBuild są monitorowane.</span><span class="sxs-lookup"><span data-stu-id="70965-155">The [MSBuild section](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="70965-156">[ReadMe czujki dotnet](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) zawiera informacje na temat dotnet czujki nieuwzględnione w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="70965-156">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
