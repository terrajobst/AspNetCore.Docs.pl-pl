---
title: "Tworzenie aplikacji platformy ASP.NET Core za pomocą czujki dotnet"
author: rick-anderson
description: "Ten samouczek pokazuje, jak zainstalować i używać narzędzia obserwatora (dotnet czujki) pliku .NET Core CLI w aplikacji platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: cadd4a6a78c29e2213c39a02729b5c32a2b93ebd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="f1680-103">Tworzenie aplikacji platformy ASP.NET Core za pomocą czujki dotnet</span><span class="sxs-lookup"><span data-stu-id="f1680-103">Developing ASP.NET Core apps using dotnet watch</span></span>

<span data-ttu-id="f1680-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Hurdugaci zwycięzcę](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="f1680-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="f1680-105">`dotnet watch`to narzędzie, które uruchamia [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools) poleceń podczas zmiany plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="f1680-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="f1680-106">Na przykład zmianę pliku może wyzwolić kompilację, wykonywania testów lub wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="f1680-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="f1680-107">W tym samouczku używamy istniejącej aplikacji interfejsu API sieci Web z dwa punkty końcowe:, który zwraca sumę i zwracające produktu.</span><span class="sxs-lookup"><span data-stu-id="f1680-107">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="f1680-108">Metoda produktu zawiera usterkę, która będzie możemy naprawić w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="f1680-108">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="f1680-109">Pobierz [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="f1680-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="f1680-110">Zawiera dwa projekty: *aplikacji sieci Web* (platformy ASP.NET Core interfejsu API sieci Web) i *WebAppTests* (testów jednostkowych dla interfejsu API sieci Web).</span><span class="sxs-lookup"><span data-stu-id="f1680-110">It contains two projects: *WebApp* (an ASP.NET Core Web API) and *WebAppTests* (unit tests for the Web API).</span></span>

<span data-ttu-id="f1680-111">W powłoce poleceń, przejdź do *aplikacji sieci Web* folder i uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f1680-111">In a command shell, navigate to the *WebApp* folder and run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="f1680-112">Dane wyjściowe konsoli przedstawia komunikaty podobne do następujących (co oznacza, że aplikacja jest uruchomiona i oczekujące na żądania):</span><span class="sxs-lookup"><span data-stu-id="f1680-112">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="f1680-113">W przeglądarce sieci web, przejdź do `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="f1680-113">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="f1680-114">Należy sprawdzić działanie `9`.</span><span class="sxs-lookup"><span data-stu-id="f1680-114">You should see the result of `9`.</span></span>

<span data-ttu-id="f1680-115">Przejdź do produktu interfejsu API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="f1680-115">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="f1680-116">Zwraca `9`, a nie `20` zgodnie z regułami.</span><span class="sxs-lookup"><span data-stu-id="f1680-116">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="f1680-117">Firma Microsoft będzie rozwiązać ten problem, później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="f1680-117">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="f1680-118">Dodaj `dotnet watch` do projektu</span><span class="sxs-lookup"><span data-stu-id="f1680-118">Add `dotnet watch` to a project</span></span>

1. <span data-ttu-id="f1680-119">Dodaj `Microsoft.DotNet.Watcher.Tools` odwołanie do pakietu *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="f1680-119">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. <span data-ttu-id="f1680-120">Zainstaluj `Microsoft.DotNet.Watcher.Tools` pakietu, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f1680-120">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="f1680-121">Za pomocą polecenia interfejsu wiersza polecenia platformy .NET Core.`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="f1680-121">Running .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="f1680-122">Wszelkie [polecenia interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools#cli-commands) może być uruchamiane przy `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="f1680-122">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="f1680-123">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f1680-123">For example:</span></span>

| <span data-ttu-id="f1680-124">Polecenie</span><span class="sxs-lookup"><span data-stu-id="f1680-124">Command</span></span> | <span data-ttu-id="f1680-125">Polecenie z czujki</span><span class="sxs-lookup"><span data-stu-id="f1680-125">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="f1680-126">Uruchom DotNet</span><span class="sxs-lookup"><span data-stu-id="f1680-126">dotnet run</span></span> | <span data-ttu-id="f1680-127">Uruchom czujki DotNet</span><span class="sxs-lookup"><span data-stu-id="f1680-127">dotnet watch run</span></span> |
| <span data-ttu-id="f1680-128">Uruchom netcoreapp2.0 -f DotNet</span><span class="sxs-lookup"><span data-stu-id="f1680-128">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="f1680-129">Uruchom netcoreapp2.0 -f czujki DotNet</span><span class="sxs-lookup"><span data-stu-id="f1680-129">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="f1680-130">Uruchom netcoreapp2.0 -f - DotNet — arg1</span><span class="sxs-lookup"><span data-stu-id="f1680-130">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="f1680-131">Obejrzyj DotNet Uruchom netcoreapp2.0 -f ---arg1</span><span class="sxs-lookup"><span data-stu-id="f1680-131">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="f1680-132">DotNet test</span><span class="sxs-lookup"><span data-stu-id="f1680-132">dotnet test</span></span> | <span data-ttu-id="f1680-133">test czujki DotNet</span><span class="sxs-lookup"><span data-stu-id="f1680-133">dotnet watch test</span></span> |

<span data-ttu-id="f1680-134">Uruchom `dotnet watch run` w *aplikacji sieci Web* folderu.</span><span class="sxs-lookup"><span data-stu-id="f1680-134">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="f1680-135">Dane wyjściowe konsoli wskazuje `watch` została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="f1680-135">The console output indicates `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="f1680-136">Wprowadzanie zmian z`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="f1680-136">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="f1680-137">Upewnij się, że `dotnet watch` jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="f1680-137">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="f1680-138">Napraw błąd w `Product` metody *MathController.cs* tak aby zwracało produktu, a nie ich sumę:</span><span class="sxs-lookup"><span data-stu-id="f1680-138">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="f1680-139">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="f1680-139">Save the file.</span></span> <span data-ttu-id="f1680-140">Dane wyjściowe konsoli wskazuje, że `dotnet watch` Wykryto zmianę pliku i ponownym uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1680-140">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="f1680-141">Sprawdź `http://localhost:<port number>/api/math/product?a=4&b=5` zwraca prawidłowego wyniku.</span><span class="sxs-lookup"><span data-stu-id="f1680-141">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="f1680-142">Uruchamianie testów za pomocą`dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="f1680-142">Running tests using `dotnet watch`</span></span>

1. <span data-ttu-id="f1680-143">Zmień `Product` metody *MathController.cs* z powrotem do zwracania Suma i Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="f1680-143">Change the `Product` method of *MathController.cs* back to returning the sum and save the file.</span></span>
1. <span data-ttu-id="f1680-144">W powłoce poleceń, przejdź do *WebAppTests* folderu.</span><span class="sxs-lookup"><span data-stu-id="f1680-144">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="f1680-145">Run `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="f1680-145">Run `dotnet restore`.</span></span>
1. <span data-ttu-id="f1680-146">Run `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="f1680-146">Run `dotnet watch test`.</span></span> <span data-ttu-id="f1680-147">Dane wyjściowe wskazuje, że testowanie nie powiodło się i tym obserwatora oczekuje na zmiany w pliku:</span><span class="sxs-lookup"><span data-stu-id="f1680-147">Its output indicates that a test failed and that watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="f1680-148">Usuń `Product` metody kod, tak aby zwracało produktu.</span><span class="sxs-lookup"><span data-stu-id="f1680-148">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="f1680-149">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="f1680-149">Save the file.</span></span>

<span data-ttu-id="f1680-150">`dotnet watch`wykrywa zmiany pliku i zwracające testy.</span><span class="sxs-lookup"><span data-stu-id="f1680-150">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="f1680-151">Dane wyjściowe konsoli wskazuje testy zostały zakończone pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="f1680-151">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="f1680-152">dotnet-watch in GitHub</span><span class="sxs-lookup"><span data-stu-id="f1680-152">dotnet-watch in GitHub</span></span>

<span data-ttu-id="f1680-153">Obejrzyj DotNet jest częścią usługi GitHub [repozytorium DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span><span class="sxs-lookup"><span data-stu-id="f1680-153">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>

<span data-ttu-id="f1680-154">[Sekcji MSBuild](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) z [ReadMe czujki dotnet](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) wyjaśniono, jak czujki dotnet można skonfigurować w pliku projektu MSBuild są monitorowane.</span><span class="sxs-lookup"><span data-stu-id="f1680-154">The [MSBuild section](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="f1680-155">[ReadMe czujki dotnet](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) zawiera informacje na temat dotnet czujki nieuwzględnione w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="f1680-155">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
