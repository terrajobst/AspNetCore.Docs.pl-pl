---
title: "Host usługi systemu Windows"
author: tdykstra
description: "Dowiedz się, jak hostowanie aplikacji platformy ASP.NET Core w usłudze systemu Windows."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/30/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: beda34dbd613f6ffe0afa207ab57dd6ebbc489ee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="5b858-103">Host aplikacji platformy ASP.NET Core w usłudze systemu Windows</span><span class="sxs-lookup"><span data-stu-id="5b858-103">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="5b858-104">przez [Dykstra niestandardowy](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5b858-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="5b858-105">Zalecanym sposobem hostowania aplikacji platformy ASP.NET Core w systemie Windows bez za pomocą usług IIS jest uruchomienie go w [usługi systemu Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="5b858-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="5b858-106">W ten sposób go może automatycznie uruchomić po ponownych rozruchów i awarii (Crash), bez oczekiwania na użytkownika do logowania.</span><span class="sxs-lookup"><span data-stu-id="5b858-106">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="5b858-107">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="5b858-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="5b858-108">Zobacz [następne kroki](#next-steps) sekcji, aby uzyskać instrukcje dotyczące sposobu jego uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="5b858-108">See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5b858-109">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5b858-109">Prerequisites</span></span>

* <span data-ttu-id="5b858-110">Aplikacja muszą działać na środowiska uruchomieniowego .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5b858-110">The app must run on the .NET Framework runtime.</span></span>  <span data-ttu-id="5b858-111">W *.csproj* pliku, podaj odpowiednie wartości dla [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) i [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="5b858-111">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="5b858-112">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="5b858-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="5b858-113">Podczas tworzenia projektu w programie Visual Studio, użyj **aplikacji platformy ASP.NET Core (.NET Framework)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="5b858-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="5b858-114">Jeśli aplikacja odbiera żądania z Internetu (nie tylko z sieci wewnętrznej), należy użyć [WebListener](xref:fundamentals/servers/weblistener) serwera sieci web zamiast [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="5b858-114">If the app receives requests from the Internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="5b858-115">Kestrel musi być używany z programem IIS we wdrożeniach krawędzi.</span><span class="sxs-lookup"><span data-stu-id="5b858-115">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="5b858-116">Aby uzyskać więcej informacji, zobacz [użycie Kestrel z zwrotny serwer proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="5b858-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="5b858-117">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="5b858-117">Getting started</span></span>

<span data-ttu-id="5b858-118">W tej sekcji opisano minimalną zmiany wymagane do skonfigurowania istniejącego projektu platformy ASP.NET Core do uruchamiania w usłudze.</span><span class="sxs-lookup"><span data-stu-id="5b858-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="5b858-119">Zainstaluj pakiet NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="5b858-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="5b858-120">Wprowadź następujące zmiany w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="5b858-120">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="5b858-121">Wywołanie `host.RunAsService` zamiast `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="5b858-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="5b858-122">Jeśli kod wywołuje `UseContentRoot`, użyj ścieżki do lokalizacji publikacji zamiast`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="5b858-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="5b858-123">Opublikuj aplikację w folderze.</span><span class="sxs-lookup"><span data-stu-id="5b858-123">Publish the application to a folder.</span></span>

  <span data-ttu-id="5b858-124">Użyj [publikowania dotnet](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) lub [profilu publikowania programu Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) który publikuje do folderu.</span><span class="sxs-lookup"><span data-stu-id="5b858-124">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

* <span data-ttu-id="5b858-125">Przetestuj tworzenia i uruchamiania usługi.</span><span class="sxs-lookup"><span data-stu-id="5b858-125">Test by creating and starting the service.</span></span>

  <span data-ttu-id="5b858-126">Otwórz okno wiersza polecenia administratora, aby użyć [sc.exe](https://technet.microsoft.com/library/bb490995) wiersza polecenia narzędzia do tworzenia i uruchamiania usługi.</span><span class="sxs-lookup"><span data-stu-id="5b858-126">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="5b858-127">Jeśli usługa ma nazwę Moja_usługa, Opublikuj aplikację `c:\svc`, a aplikacja nosi nazwę AspNetCoreService, poleceń będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="5b858-127">If the service is named MyService, publish the app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```

  <span data-ttu-id="5b858-128">`binPath` Wartość jest ścieżką do pliku wykonywalnego aplikacji, łącznie z nazwą pliku wykonywalnego, sama.</span><span class="sxs-lookup"><span data-stu-id="5b858-128">The `binPath` value is the path to the app's executable, including the executable filename itself.</span></span>

  ![Okno konsoli tworzenie i uruchamianie przykładu](windows-service/_static/create-start.png)

  <span data-ttu-id="5b858-130">Po zakończeniu tych poleceń, przejdź do tej samej ścieżki, w przypadku uruchamiania jako aplikacji konsoli (domyślnie `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="5b858-130">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`)</span></span>

  ![Uruchomionych w usłudze](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="5b858-132">Umożliwiają uruchamianie poza usługą</span><span class="sxs-lookup"><span data-stu-id="5b858-132">Provide a way to run outside of a service</span></span>

<span data-ttu-id="5b858-133">Możliwe jest łatwiejsze testowanie i debugowanie podczas uruchamiania poza usługą, dzięki czemu zwyczajowe można dodać kod, który wywołuje `host.RunAsService` tylko w niektórych warunkach.</span><span class="sxs-lookup"><span data-stu-id="5b858-133">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="5b858-134">Na przykład aplikacja może działać jako aplikację konsoli z `--console` argumentu wiersza polecenia lub Jeśli Debuger jest dołączony.</span><span class="sxs-lookup"><span data-stu-id="5b858-134">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="5b858-135">Obsługa zatrzymywania i uruchamiania zdarzeń</span><span class="sxs-lookup"><span data-stu-id="5b858-135">Handle stopping and starting events</span></span>

<span data-ttu-id="5b858-136">Do obsługi `OnStarting`, `OnStarted`, i `OnStopping` zdarzenia, wprowadź następujące zmiany dodatkowe:</span><span class="sxs-lookup"><span data-stu-id="5b858-136">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="5b858-137">Utwórz klasę pochodną `WebHostService`.</span><span class="sxs-lookup"><span data-stu-id="5b858-137">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="5b858-138">Create — metoda rozszerzenia dla `IWebHost` niestandardowego, który przekazuje `WebHostService` do `ServiceBase.Run`.</span><span class="sxs-lookup"><span data-stu-id="5b858-138">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="5b858-139">W `Program.Main` zmiany Wywołanie nowej metody rozszerzenia, zamiast `host.RunAsService`.</span><span class="sxs-lookup"><span data-stu-id="5b858-139">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="5b858-140">Jeśli niestandardowa `WebHostService` kodu musi pobrać usługę z iniekcji zależności (np. Rejestrator), pobierz go z `Services` właściwość `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="5b858-140">If the custom `WebHostService` code needs to get a service from dependency injection (such as a logger), get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="5b858-141">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="5b858-141">Next steps</span></span>

<span data-ttu-id="5b858-142">[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) dołączony ten artykuł jest prostej aplikacji sieci web MVC, jak pokazano w poprzednich przykładach został zmodyfikowany.</span><span class="sxs-lookup"><span data-stu-id="5b858-142">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="5b858-143">Aby uruchomić go w usłudze, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="5b858-143">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="5b858-144">Publikowanie *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="5b858-144">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="5b858-145">Otwórz okno administratora.</span><span class="sxs-lookup"><span data-stu-id="5b858-145">Open an administrator window.</span></span>

* <span data-ttu-id="5b858-146">Wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="5b858-146">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="5b858-147">W przeglądarce przejdź do http://localhost: 5000, aby sprawdzić, czy jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="5b858-147">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="5b858-148">Jeśli aplikacja nie uruchamia się zgodnie z oczekiwaniami uruchomionej w usłudze, szybko udostępnić komunikaty o błędach jest dodanie dostawcy logowania, takie jak [Dostawca dziennika zdarzeń systemu Windows](xref:fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="5b858-148">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging/index#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="5b858-149">Potwierdzenia</span><span class="sxs-lookup"><span data-stu-id="5b858-149">Acknowledgments</span></span>

<span data-ttu-id="5b858-150">W tym artykule został napisany za pomocą źródeł, które zostały już opublikowane.</span><span class="sxs-lookup"><span data-stu-id="5b858-150">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="5b858-151">Najwcześniejsza i najbardziej przydatne z nich zostały one:</span><span class="sxs-lookup"><span data-stu-id="5b858-151">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="5b858-152">Hosting platformy ASP.NET Core jako usługa systemu Windows</span><span class="sxs-lookup"><span data-stu-id="5b858-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="5b858-153">Jak obsługiwać podstawowych ASP.NET w usłudze systemu Windows</span><span class="sxs-lookup"><span data-stu-id="5b858-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
