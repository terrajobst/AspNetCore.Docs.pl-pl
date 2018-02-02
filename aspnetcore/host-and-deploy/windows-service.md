---
title: "Host usługi systemu Windows"
author: tdykstra
description: "Dowiedz się, jak udostępniać aplikacji platformy ASP.NET Core w usłudze systemu Windows."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: c14a1f62bce4d06be3b8e6356f45cd5e330a0751
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="be07b-103">Host aplikacji platformy ASP.NET Core w usłudze systemu Windows</span><span class="sxs-lookup"><span data-stu-id="be07b-103">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="be07b-104">przez [Dykstra niestandardowy](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="be07b-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="be07b-105">Zalecanym sposobem hostowania aplikacji platformy ASP.NET Core w systemie Windows bez za pomocą usług IIS jest uruchomienie go w [usługi systemu Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="be07b-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="be07b-106">Gdy hostowany jako usługa systemu Windows, aplikacja może automatycznie uruchamiania po ponownym uruchomieniu i awarii bez udziału człowieka.</span><span class="sxs-lookup"><span data-stu-id="be07b-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="be07b-107">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="be07b-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="be07b-108">Instrukcje na temat uruchamiania przykładowej aplikacji, można znaleźć w próbce *README.md* pliku.</span><span class="sxs-lookup"><span data-stu-id="be07b-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="be07b-109">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="be07b-109">Prerequisites</span></span>

* <span data-ttu-id="be07b-110">Aplikacja muszą działać na środowiska uruchomieniowego .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="be07b-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="be07b-111">W *.csproj* pliku, podaj odpowiednie wartości dla [TargetFramework](/nuget/schema/target-frameworks) i [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="be07b-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="be07b-112">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="be07b-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="be07b-113">Podczas tworzenia projektu w programie Visual Studio, użyj **aplikacji platformy ASP.NET Core (.NET Framework)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="be07b-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="be07b-114">Jeśli aplikacja odbiera żądania z Internetu (nie tylko z sieci wewnętrznej), należy użyć [HTTP.sys](xref:fundamentals/servers/httpsys) serwera sieci web (wcześniej znane jako [WebListener](xref:fundamentals/servers/weblistener) dla platformy ASP.NET Core 1.x aplikacji) zamiast [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="be07b-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="be07b-115">Usługi IIS jest zalecane używanie jako zwrotny serwer proxy serwera z Kestrel wdrożeń krawędzi.</span><span class="sxs-lookup"><span data-stu-id="be07b-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="be07b-116">Aby uzyskać więcej informacji, zobacz [użycie Kestrel z zwrotny serwer proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="be07b-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="be07b-117">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="be07b-117">Getting started</span></span>

<span data-ttu-id="be07b-118">W tej sekcji opisano minimalną zmiany wymagane do skonfigurowania istniejącego projektu platformy ASP.NET Core do uruchamiania w usłudze.</span><span class="sxs-lookup"><span data-stu-id="be07b-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="be07b-119">Zainstaluj pakiet NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="be07b-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="be07b-120">Wprowadź następujące zmiany w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="be07b-120">Make the following changes in `Program.Main`:</span></span>
  
   * <span data-ttu-id="be07b-121">Wywołanie `host.RunAsService` zamiast `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="be07b-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
   * <span data-ttu-id="be07b-122">Jeśli kod wywołuje `UseContentRoot`, użyj ścieżki do lokalizacji publikacji zamiast `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="be07b-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be07b-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be07b-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be07b-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be07b-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

1. <span data-ttu-id="be07b-125">Publikowanie aplikacji w folderze.</span><span class="sxs-lookup"><span data-stu-id="be07b-125">Publish the app to a folder.</span></span> <span data-ttu-id="be07b-126">Użyj [publikowania dotnet](/dotnet/articles/core/tools/dotnet-publish) lub [profilu publikowania programu Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) który publikuje do folderu.</span><span class="sxs-lookup"><span data-stu-id="be07b-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

1. <span data-ttu-id="be07b-127">Przetestuj tworzenia i uruchamiania usługi.</span><span class="sxs-lookup"><span data-stu-id="be07b-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="be07b-128">Otwórz powłokę wiersza polecenia z uprawnieniami administracyjnymi do użycia [sc.exe](https://technet.microsoft.com/library/bb490995) wiersza polecenia narzędzia do tworzenia i uruchamiania usługi.</span><span class="sxs-lookup"><span data-stu-id="be07b-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="be07b-129">Jeśli usługa ma nazwę Moja_usługa, są publikowane w `c:\svc`, a polecenia o nazwie AspNetCoreService, są:</span><span class="sxs-lookup"><span data-stu-id="be07b-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="be07b-130">`binPath` Wartość jest ścieżką do pliku wykonywalnego aplikacji, która zawiera nazwę pliku wykonywalnego.</span><span class="sxs-lookup"><span data-stu-id="be07b-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![Okno konsoli tworzenie i uruchamianie przykładu](windows-service/_static/create-start.png)

   <span data-ttu-id="be07b-132">Po zakończeniu tych poleceń, przejdź do tej samej ścieżki, w przypadku uruchamiania jako aplikacji konsoli (domyślnie `http://localhost:5000`):</span><span class="sxs-lookup"><span data-stu-id="be07b-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![Uruchomionych w usłudze](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="be07b-134">Umożliwiają uruchamianie poza usługą</span><span class="sxs-lookup"><span data-stu-id="be07b-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="be07b-135">Możliwe jest łatwiejsze testowanie i debugowanie podczas uruchamiania poza usługą, dzięki czemu zwyczajowe można dodać kod, który wywołuje `RunAsService` tylko w niektórych warunkach.</span><span class="sxs-lookup"><span data-stu-id="be07b-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="be07b-136">Na przykład aplikacja może działać jako aplikację konsoli z `--console` argumentu wiersza polecenia lub jeśli jest dołączony debuger:</span><span class="sxs-lookup"><span data-stu-id="be07b-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be07b-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be07b-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be07b-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be07b-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="be07b-139">Obsługa zatrzymywania i uruchamiania zdarzeń</span><span class="sxs-lookup"><span data-stu-id="be07b-139">Handle stopping and starting events</span></span>

<span data-ttu-id="be07b-140">Do obsługi `OnStarting`, `OnStarted`, i `OnStopping` zdarzenia, wprowadź następujące zmiany dodatkowe:</span><span class="sxs-lookup"><span data-stu-id="be07b-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="be07b-141">Utwórz klasę pochodną `WebHostService`:</span><span class="sxs-lookup"><span data-stu-id="be07b-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

1. <span data-ttu-id="be07b-142">Create — metoda rozszerzenia dla `IWebHost` niestandardowego, który przekazuje `WebHostService` do `ServiceBase.Run`:</span><span class="sxs-lookup"><span data-stu-id="be07b-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

1. <span data-ttu-id="be07b-143">W `Program.Main`, wywołaj nowej metody rozszerzenia `RunAsCustomService`, zamiast `RunAsService`:</span><span class="sxs-lookup"><span data-stu-id="be07b-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="be07b-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="be07b-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="be07b-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="be07b-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

<span data-ttu-id="be07b-146">Jeśli niestandardowa `WebHostService` kod wymaga usługi z iniekcji zależności (np. Rejestrator), Uzyskaj ją z `Services` właściwości `IWebHost`:</span><span class="sxs-lookup"><span data-stu-id="be07b-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="acknowledgments"></a><span data-ttu-id="be07b-147">Potwierdzenia</span><span class="sxs-lookup"><span data-stu-id="be07b-147">Acknowledgments</span></span>

<span data-ttu-id="be07b-148">W tym artykule został napisany za pomocą opublikowanych źródeł:</span><span class="sxs-lookup"><span data-stu-id="be07b-148">This article was written with the help of published sources:</span></span>

* [<span data-ttu-id="be07b-149">Hosting platformy ASP.NET Core jako usługa systemu Windows</span><span class="sxs-lookup"><span data-stu-id="be07b-149">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="be07b-150">Jak obsługiwać podstawowych ASP.NET w usłudze systemu Windows</span><span class="sxs-lookup"><span data-stu-id="be07b-150">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
