---
title: "Host usługi systemu Windows"
author: tdykstra
description: "Dowiedz się, jak hostowanie aplikacji platformy ASP.NET Core w usłudze systemu Windows."
keywords: "Hosting platformy ASP.NET Core usługi systemu Windows"
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: a6d1acf5ab8f40b0b4d487a6f34cd83d13907852
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="a234a-104">Host aplikacji platformy ASP.NET Core w usłudze systemu Windows</span><span class="sxs-lookup"><span data-stu-id="a234a-104">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="a234a-105">przez [Dykstra niestandardowy](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a234a-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="a234a-106">Zalecanym sposobem na potrzeby hostowania aplikacji platformy ASP.NET Core w systemie Windows, jeśli nie używasz usług IIS jest uruchamiany w [usługi systemu Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="a234a-106">The recommended way to host an ASP.NET Core app on Windows when you don't use IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="a234a-107">W ten sposób go może automatycznie uruchomić po ponownych rozruchów i awarii (Crash), bez oczekiwania na użytkownika do logowania.</span><span class="sxs-lookup"><span data-stu-id="a234a-107">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="a234a-108">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="a234a-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="a234a-109">Zobacz [następne kroki](#next-steps) sekcji, aby uzyskać instrukcje dotyczące sposobu jego uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="a234a-109">See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a234a-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="a234a-110">Prerequisites</span></span>

* <span data-ttu-id="a234a-111">Aplikacja muszą działać na środowiska uruchomieniowego .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a234a-111">The app must run on the .NET Framework runtime.</span></span>  <span data-ttu-id="a234a-112">W *.csproj* pliku, podaj odpowiednie wartości dla [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) i [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="a234a-112">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="a234a-113">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="a234a-113">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="a234a-114">Podczas tworzenia projektu w programie Visual Studio, użyj **aplikacji platformy ASP.NET Core (.NET Framework)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="a234a-114">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="a234a-115">Jeśli aplikacja otrzyma żądania z Internetu (nie tylko z sieci wewnętrznej), należy użyć [WebListener](xref:fundamentals/servers/weblistener) serwera sieci web zamiast [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="a234a-115">If the app will get requests from the internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="a234a-116">Kestrel musi być używany z programem IIS we wdrożeniach krawędzi.</span><span class="sxs-lookup"><span data-stu-id="a234a-116">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="a234a-117">Aby uzyskać więcej informacji, zobacz [użycie Kestrel z zwrotny serwer proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="a234a-117">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="a234a-118">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="a234a-118">Getting started</span></span>

<span data-ttu-id="a234a-119">W tej sekcji opisano minimalną zmiany wymagane do skonfigurowania istniejącego projektu platformy ASP.NET Core do uruchamiania w usłudze.</span><span class="sxs-lookup"><span data-stu-id="a234a-119">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="a234a-120">Zainstaluj pakiet NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="a234a-120">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="a234a-121">Wprowadź następujące zmiany w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="a234a-121">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="a234a-122">Wywołanie `host.RunAsService` zamiast `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="a234a-122">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="a234a-123">Jeśli kod wywołuje `UseContentRoot`, użyj ścieżki do lokalizacji publikacji zamiast`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="a234a-123">If your code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="a234a-124">Opublikuj aplikację w folderze.</span><span class="sxs-lookup"><span data-stu-id="a234a-124">Publish the application to a folder.</span></span>

  <span data-ttu-id="a234a-125">Użyj [publikowania dotnet](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) lub [profilu publikowania programu Visual Studio](xref:publishing/web-publishing-vs) który publikuje do folderu.</span><span class="sxs-lookup"><span data-stu-id="a234a-125">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:publishing/web-publishing-vs) that publishes to a folder.</span></span>

* <span data-ttu-id="a234a-126">Przetestuj tworzenia i uruchamiania usługi.</span><span class="sxs-lookup"><span data-stu-id="a234a-126">Test by creating and starting the service.</span></span>

  <span data-ttu-id="a234a-127">Otwórz okno wiersza polecenia administratora, aby użyć [sc.exe](https://technet.microsoft.com/library/bb490995) wiersza polecenia narzędzia do tworzenia i uruchamiania usługi.</span><span class="sxs-lookup"><span data-stu-id="a234a-127">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="a234a-128">Jeśli nazwa usługi Moja_usługa, Opublikuj aplikację, aby `c:\svc`, a aplikacja nosi nazwę AspNetCoreService, poleceń będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="a234a-128">If you name your service MyService, you publish your app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  <span data-ttu-id="a234a-129">`binPath` Wartość jest ścieżką do pliku wykonywalnego aplikacji, łącznie z nazwą pliku wykonywalnego, sama.</span><span class="sxs-lookup"><span data-stu-id="a234a-129">The `binPath` value is the path to your app's executable, including the executable filename itself.</span></span>

  ![Okno konsoli tworzenie i uruchamianie przykładu](windows-service/_static/create-start.png)

  <span data-ttu-id="a234a-131">Po zakończeniu tych poleceń, można przejść do tej samej ścieżce jako uruchomienie jako aplikacji konsoli (domyślnie `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="a234a-131">When these commands finish, you can browse to the same path as when you run as a console app (by default, `http://localhost:5000`)</span></span>

  ![Uruchomionych w usłudze](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="a234a-133">Umożliwiają uruchamianie poza usługą</span><span class="sxs-lookup"><span data-stu-id="a234a-133">Provide a way to run outside of a service</span></span>

<span data-ttu-id="a234a-134">Możliwe jest łatwiejsze testowanie i debugowanie podczas pracy poza usługą, dzięki czemu zwyczajowe można dodać kod, który wywołuje `host.RunAsService` tylko w niektórych warunkach.</span><span class="sxs-lookup"><span data-stu-id="a234a-134">It's easier to test and debug when you're running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="a234a-135">Na przykład można uruchomić jako aplikacji konsoli Jeśli otrzymasz `--console` argumentu wiersza polecenia lub Jeśli Debuger jest dołączony.</span><span class="sxs-lookup"><span data-stu-id="a234a-135">For example, you could run as a console app if you get a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="a234a-136">Obsługa zatrzymywania i uruchamiania zdarzeń</span><span class="sxs-lookup"><span data-stu-id="a234a-136">Handle stopping and starting events</span></span>

<span data-ttu-id="a234a-137">Jeśli chcesz obsługiwać `OnStarting`, `OnStarted`, i `OnStopping` zdarzenia, wprowadź następujące zmiany dodatkowe:</span><span class="sxs-lookup"><span data-stu-id="a234a-137">If you want to handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="a234a-138">Utwórz klasę pochodną `WebHostService`.</span><span class="sxs-lookup"><span data-stu-id="a234a-138">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="a234a-139">Create — metoda rozszerzenia dla `IWebHost` , który przekazuje niestandardowe `WebHostService` do `ServiceBase.Run`.</span><span class="sxs-lookup"><span data-stu-id="a234a-139">Create an extension method for `IWebHost` that passes your custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="a234a-140">W `Program.Main` zmiany Wywołanie nowej metody rozszerzenia, zamiast `host.RunAsService`.</span><span class="sxs-lookup"><span data-stu-id="a234a-140">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="a234a-141">Jeśli niestandardowe `WebHostService` kodu musi pobrać usługę z iniekcji zależności (np. Rejestrator), możesz pobrać go z `Services` właściwość `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="a234a-141">If your custom `WebHostService` code needs to get a service from dependency injection (such as a logger), you can get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="a234a-142">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="a234a-142">Next steps</span></span>

<span data-ttu-id="a234a-143">[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) dołączony ten artykuł jest prostej aplikacji sieci web MVC, jak pokazano w poprzednich przykładach został zmodyfikowany.</span><span class="sxs-lookup"><span data-stu-id="a234a-143">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="a234a-144">Aby uruchomić go w usłudze, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="a234a-144">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="a234a-145">Publikowanie *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="a234a-145">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="a234a-146">Otwórz okno administratora.</span><span class="sxs-lookup"><span data-stu-id="a234a-146">Open an administrator window.</span></span>

* <span data-ttu-id="a234a-147">Wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="a234a-147">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="a234a-148">W przeglądarce przejdź do http://localhost: 5000, aby sprawdzić, czy jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="a234a-148">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="a234a-149">Jeśli aplikacja nie uruchamia się zgodnie z oczekiwaniami uruchomionej w usłudze, szybko udostępnić komunikaty o błędach jest dodanie dostawcy logowania, takie jak [Dostawca dziennika zdarzeń systemu Windows](xref:fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="a234a-149">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging/index#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="a234a-150">Potwierdzenia</span><span class="sxs-lookup"><span data-stu-id="a234a-150">Acknowledgments</span></span>

<span data-ttu-id="a234a-151">W tym artykule został napisany za pomocą źródeł, które zostały już opublikowane.</span><span class="sxs-lookup"><span data-stu-id="a234a-151">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="a234a-152">Najwcześniejsza i najbardziej przydatne z nich zostały one:</span><span class="sxs-lookup"><span data-stu-id="a234a-152">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="a234a-153">Hosting platformy ASP.NET Core jako usługa systemu Windows</span><span class="sxs-lookup"><span data-stu-id="a234a-153">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="a234a-154">Jak obsługiwać podstawowych ASP.NET w usłudze systemu Windows</span><span class="sxs-lookup"><span data-stu-id="a234a-154">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
