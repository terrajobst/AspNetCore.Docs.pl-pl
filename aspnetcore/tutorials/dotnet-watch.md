---
title: Tworzenie aplikacji platformy ASP.NET Core za pomocą monitora plików
author: rick-anderson
description: Ten samouczek pokazuje, jak zainstalować i używać narzędzia obserwatora (dotnet czujki) pliku .NET Core CLI w aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 05/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/dotnet-watch
ms.openlocfilehash: 29890640223fe533cca82fb8d39a5ef26e8c6639
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729979"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="eb95c-103">Tworzenie aplikacji platformy ASP.NET Core za pomocą monitora plików</span><span class="sxs-lookup"><span data-stu-id="eb95c-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="eb95c-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Hurdugaci zwycięzcę](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="eb95c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="eb95c-105">`dotnet watch` to narzędzie, które uruchamia [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools) poleceń podczas zmiany plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="eb95c-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="eb95c-106">Na przykład zmianę pliku może wyzwolić kompilację, wykonywania testów lub wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="eb95c-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="eb95c-107">W tym samouczku korzysta z istniejącej interfejsu API sieci web z dwa punkty końcowe:, który zwraca sumę i zwracające produktu.</span><span class="sxs-lookup"><span data-stu-id="eb95c-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="eb95c-108">Metoda produktu ma usterki, które zawartych w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="eb95c-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="eb95c-109">Pobierz [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="eb95c-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="eb95c-110">Zawiera dwa projekty: *aplikacji sieci Web* (platformy ASP.NET Core interfejsu API sieci web) i *WebAppTests* (testów jednostkowych dla interfejsu API sieci web).</span><span class="sxs-lookup"><span data-stu-id="eb95c-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="eb95c-111">W powłoce poleceń, przejdź do *aplikacji sieci Web* folderu.</span><span class="sxs-lookup"><span data-stu-id="eb95c-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="eb95c-112">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="eb95c-112">Run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="eb95c-113">Dane wyjściowe konsoli przedstawia komunikaty podobne do następujących (co oznacza, że aplikacja jest uruchomiona i oczekujące na żądania):</span><span class="sxs-lookup"><span data-stu-id="eb95c-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="eb95c-114">W przeglądarce sieci web, przejdź do `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="eb95c-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="eb95c-115">Należy sprawdzić działanie `9`.</span><span class="sxs-lookup"><span data-stu-id="eb95c-115">You should see the result of `9`.</span></span>

<span data-ttu-id="eb95c-116">Przejdź do produktu interfejsu API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="eb95c-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="eb95c-117">Zwraca `9`, a nie `20` zgodnie z regułami.</span><span class="sxs-lookup"><span data-stu-id="eb95c-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="eb95c-118">Ten problem został rozwiązany później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="eb95c-118">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="eb95c-119">Dodaj `dotnet watch` do projektu</span><span class="sxs-lookup"><span data-stu-id="eb95c-119">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="eb95c-120">`dotnet watch` Narzędzia obserwatora plików są dołączone do wersji 2.1.300 zestawu SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="eb95c-120">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="eb95c-121">Poniższe kroki są wymagane w przypadku wcześniejszych wersji programu .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="eb95c-121">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="eb95c-122">Dodaj `Microsoft.DotNet.Watcher.Tools` odwołanie do pakietu *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="eb95c-122">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="eb95c-123">Zainstaluj `Microsoft.DotNet.Watcher.Tools` pakietu, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="eb95c-123">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="eb95c-124">Uruchom przy użyciu polecenia interfejsu wiersza polecenia platformy .NET Core `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="eb95c-124">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="eb95c-125">Wszelkie [polecenia interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools#cli-commands) może być uruchamiane przy `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="eb95c-125">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="eb95c-126">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="eb95c-126">For example:</span></span>

| <span data-ttu-id="eb95c-127">Polecenie</span><span class="sxs-lookup"><span data-stu-id="eb95c-127">Command</span></span> | <span data-ttu-id="eb95c-128">Polecenie z czujki</span><span class="sxs-lookup"><span data-stu-id="eb95c-128">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="eb95c-129">Uruchom DotNet</span><span class="sxs-lookup"><span data-stu-id="eb95c-129">dotnet run</span></span> | <span data-ttu-id="eb95c-130">Uruchom czujki DotNet</span><span class="sxs-lookup"><span data-stu-id="eb95c-130">dotnet watch run</span></span> |
| <span data-ttu-id="eb95c-131">Uruchom netcoreapp2.0 -f DotNet</span><span class="sxs-lookup"><span data-stu-id="eb95c-131">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="eb95c-132">Uruchom netcoreapp2.0 -f czujki DotNet</span><span class="sxs-lookup"><span data-stu-id="eb95c-132">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="eb95c-133">Uruchom netcoreapp2.0 -f - DotNet — arg1</span><span class="sxs-lookup"><span data-stu-id="eb95c-133">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="eb95c-134">Obejrzyj DotNet Uruchom netcoreapp2.0 -f ---arg1</span><span class="sxs-lookup"><span data-stu-id="eb95c-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="eb95c-135">DotNet test</span><span class="sxs-lookup"><span data-stu-id="eb95c-135">dotnet test</span></span> | <span data-ttu-id="eb95c-136">test czujki DotNet</span><span class="sxs-lookup"><span data-stu-id="eb95c-136">dotnet watch test</span></span> |

<span data-ttu-id="eb95c-137">Uruchom `dotnet watch run` w *aplikacji sieci Web* folderu.</span><span class="sxs-lookup"><span data-stu-id="eb95c-137">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="eb95c-138">Dane wyjściowe konsoli wskazuje `watch` została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="eb95c-138">The console output indicates `watch` has started.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="eb95c-139">Wprowadź zmiany z `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="eb95c-139">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="eb95c-140">Upewnij się, że `dotnet watch` jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="eb95c-140">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="eb95c-141">Napraw błąd w `Product` metody *MathController.cs* tak aby zwracało produktu, a nie ich sumę:</span><span class="sxs-lookup"><span data-stu-id="eb95c-141">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

<span data-ttu-id="eb95c-142">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="eb95c-142">Save the file.</span></span> <span data-ttu-id="eb95c-143">Dane wyjściowe konsoli wskazuje, że `dotnet watch` Wykryto zmianę pliku i ponownym uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="eb95c-143">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="eb95c-144">Sprawdź `http://localhost:<port number>/api/math/product?a=4&b=5` zwraca prawidłowego wyniku.</span><span class="sxs-lookup"><span data-stu-id="eb95c-144">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="eb95c-145">Uruchom testy przy użyciu `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="eb95c-145">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="eb95c-146">Zmień `Product` metody *MathController.cs* do zwracania suma.</span><span class="sxs-lookup"><span data-stu-id="eb95c-146">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="eb95c-147">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="eb95c-147">Save the file.</span></span>
1. <span data-ttu-id="eb95c-148">W powłoce poleceń, przejdź do *WebAppTests* folderu.</span><span class="sxs-lookup"><span data-stu-id="eb95c-148">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="eb95c-149">Uruchom [przywracania dotnet](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="eb95c-149">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="eb95c-150">Run `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="eb95c-150">Run `dotnet watch test`.</span></span> <span data-ttu-id="eb95c-151">Dane wyjściowe wskazuje, że testowanie nie powiodło się i że obserwatora oczekuje na zmiany w pliku:</span><span class="sxs-lookup"><span data-stu-id="eb95c-151">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="eb95c-152">Usuń `Product` metody kod, tak aby zwracało produktu.</span><span class="sxs-lookup"><span data-stu-id="eb95c-152">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="eb95c-153">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="eb95c-153">Save the file.</span></span>

<span data-ttu-id="eb95c-154">`dotnet watch` wykrywa zmiany pliku i zwracające testy.</span><span class="sxs-lookup"><span data-stu-id="eb95c-154">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="eb95c-155">Dane wyjściowe konsoli wskazuje testy zostały zakończone pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="eb95c-155">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="eb95c-156">wyrażenie kontrolne DotNet w serwisie GitHub</span><span class="sxs-lookup"><span data-stu-id="eb95c-156">dotnet-watch in GitHub</span></span>

<span data-ttu-id="eb95c-157">Obejrzyj DotNet jest częścią usługi GitHub [repozytorium DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span><span class="sxs-lookup"><span data-stu-id="eb95c-157">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>

<span data-ttu-id="eb95c-158">[Sekcji MSBuild](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) z [ReadMe czujki dotnet](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) wyjaśniono, jak czujki dotnet można skonfigurować w pliku projektu MSBuild są monitorowane.</span><span class="sxs-lookup"><span data-stu-id="eb95c-158">The [MSBuild section](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="eb95c-159">[ReadMe czujki dotnet](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) zawiera informacje o dotnet czujki nieuwzględnione w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="eb95c-159">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) has information on dotnet-watch not covered in this tutorial.</span></span>
