---
title: Host platformy ASP.NET Core w usłudze Windows
author: guardrex
description: Dowiedz się, jak udostępnić aplikację ASP.NET Core w usłudze Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: e9e10b0bc99b2c54bf342121b1a454be5dac66c6
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938200"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="60715-103">Host platformy ASP.NET Core w usłudze Windows</span><span class="sxs-lookup"><span data-stu-id="60715-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="60715-104">Przez [Luke Latham](https://github.com/guardrex) i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="60715-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="60715-105">Aplikacji ASP.NET Core może być hostowana na Windows bez korzystania z usług IIS jako [usługi Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="60715-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="60715-106">Gdy hostowany jako usługa Windows, aplikacja może automatycznie po ponownym uruchomieniu i ulega awarii, nie wymagając interwencji człowieka.</span><span class="sxs-lookup"><span data-stu-id="60715-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="60715-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="60715-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="60715-108">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="60715-108">Get started</span></span>

<span data-ttu-id="60715-109">Następujące minimalne zmiany są wymagane do skonfigurowania istniejący projekt ASP.NET Core, do uruchamiania w usłudze:</span><span class="sxs-lookup"><span data-stu-id="60715-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="60715-110">W pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="60715-110">In the project file:</span></span>

   1. <span data-ttu-id="60715-111">Potwierdzić obecność identyfikator środowiska uruchomieniowego, albo dodaj go do  **\<PropertyGroup >** zawierający platforma docelowa:</span><span class="sxs-lookup"><span data-stu-id="60715-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

   1. <span data-ttu-id="60715-112">Dodaj odwołania do pakietu dla [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="60715-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="60715-113">Wprowadź następujące zmiany w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="60715-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="60715-114">Wywołaj [hosta. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) zamiast `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="60715-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="60715-115">Wywołaj [UseContentRoot](xref:fundamentals/host/web-host#content-root) i ścieżka do aplikacji — opublikowane lokalizacji zamiast używania `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="60715-115">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,12)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="60715-116">Publikowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="60715-116">Publish the app.</span></span> <span data-ttu-id="60715-117">Użyj [publikowania dotnet](/dotnet/articles/core/tools/dotnet-publish) lub [profilu publikowania w programie Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="60715-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span>

   <span data-ttu-id="60715-118">Aby opublikować przykładową aplikację z poziomu wiersza polecenia, uruchom następujące polecenie w oknie konsoli z folderu projektu:</span><span class="sxs-lookup"><span data-stu-id="60715-118">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="60715-119">Użyj [sc.exe](https://technet.microsoft.com/library/bb490995) narzędzie wiersza polecenia, aby utworzyć usługę.</span><span class="sxs-lookup"><span data-stu-id="60715-119">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="60715-120">`binPath` Wartość jest ścieżką do pliku wykonywalnego aplikacji, która zawiera nazwę pliku wykonywalnego.</span><span class="sxs-lookup"><span data-stu-id="60715-120">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="60715-121">**Odstęp między znakiem równości i znaku cudzysłowu na początku ścieżki jest wymagana.**</span><span class="sxs-lookup"><span data-stu-id="60715-121">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="60715-122">Dla usługi, opublikowane w folderze projektu, użyj ścieżki do *publikowania* folder, aby utworzyć usługę.</span><span class="sxs-lookup"><span data-stu-id="60715-122">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="60715-123">W poniższym przykładzie usługa jest:</span><span class="sxs-lookup"><span data-stu-id="60715-123">In the following example, the service is:</span></span>

   * <span data-ttu-id="60715-124">O nazwie **Moja_usługa**.</span><span class="sxs-lookup"><span data-stu-id="60715-124">Named **MyService**.</span></span>
   * <span data-ttu-id="60715-125">Opublikowany *c:\\my_services\\AspNetCoreService\\bin\\wersji\\&lt;TARGET_FRAMEWORK&gt;\\publikowania* folderu.</span><span class="sxs-lookup"><span data-stu-id="60715-125">Published to the *c:\\my_services\\AspNetCoreService\\bin\\Release\\&lt;TARGET_FRAMEWORK&gt;\\publish* folder.</span></span>
   * <span data-ttu-id="60715-126">Reprezentowane przez aplikację o nazwie pliku wykonywalnego *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="60715-126">Represented by an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="60715-127">Otwórz powłokę wiersza polecenia z uprawnieniami administracyjnymi i uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="60715-127">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\<TARGET_FRAMEWORK>\publish\AspNetCoreService.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="60715-128">Upewnij się, ma miejsce, między `binPath=` argumentu i jego wartość.</span><span class="sxs-lookup"><span data-stu-id="60715-128">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="60715-129">Publikowanie i uruchom usługę z innego folderu:</span><span class="sxs-lookup"><span data-stu-id="60715-129">To publish and start the service from a different folder:</span></span>
   
   1. <span data-ttu-id="60715-130">Użyj [--dane wyjściowe &lt;OUTPUT_DIRECTORY&gt; ](/dotnet/core/tools/dotnet-publish#options) opcja `dotnet publish` polecenia.</span><span class="sxs-lookup"><span data-stu-id="60715-130">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span>
   1. <span data-ttu-id="60715-131">Tworzenie usługi przy użyciu `sc.exe` polecenia przy użyciu ścieżki folderu danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="60715-131">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="60715-132">Obejmują nazwę pliku wykonywalnego usługi w ścieżce podanej do `binPath`.</span><span class="sxs-lookup"><span data-stu-id="60715-132">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="60715-133">Uruchom usługę za pomocą `sc start <SERVICE_NAME>` polecenia.</span><span class="sxs-lookup"><span data-stu-id="60715-133">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="60715-134">Aby uruchomić usługę aplikacji przykładowej, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="60715-134">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="60715-135">Polecenie zajmuje kilka sekund, aby uruchomić usługę.</span><span class="sxs-lookup"><span data-stu-id="60715-135">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="60715-136">`sc query <SERVICE_NAME>` Polecenia mogą służyć do sprawdzania stanu usługi, aby określić stan:</span><span class="sxs-lookup"><span data-stu-id="60715-136">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="60715-137">Użyj następującego polecenia, aby sprawdzić stan usługi aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="60715-137">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="60715-138">Kiedy usługa jest w `RUNNING` stanu i usługi w przypadku aplikacji sieci web, przeglądanie aplikacji w ścieżce (domyślnie `http://localhost:5000`, który przekierowuje do `https://localhost:5001` przy użyciu [HTTPS przekierowanie w oprogramowaniu pośredniczącym](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="60715-138">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="60715-139">Usługa app service przykładowego, można przeglądać w tej aplikacji w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="60715-139">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="60715-140">Zatrzymaj usługę za pomocą `sc stop <SERVICE_NAME>` polecenia.</span><span class="sxs-lookup"><span data-stu-id="60715-140">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="60715-141">Następujące polecenie zatrzymuje usługę aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="60715-141">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="60715-142">Po krótkiej chwili zatrzymania usługi, odinstaluj usługę za pomocą `sc delete <SERVICE_NAME>` polecenia.</span><span class="sxs-lookup"><span data-stu-id="60715-142">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="60715-143">Sprawdź stan usługi aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="60715-143">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="60715-144">Gdy usługa app service przykładowego jest w `STOPPED` stanu, użyj następującego polecenia, aby odinstalować usługę aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="60715-144">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="60715-145">Zapewnia możliwość uruchamiania poza usługą</span><span class="sxs-lookup"><span data-stu-id="60715-145">Provide a way to run outside of a service</span></span>

<span data-ttu-id="60715-146">Znacznie łatwiej testować i debugować podczas uruchamiania spoza niej, więc zwyczajowego, aby dodać kod, który wywołuje `RunAsService` tylko pod pewnymi warunkami.</span><span class="sxs-lookup"><span data-stu-id="60715-146">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="60715-147">Na przykład aplikacja może działać jako aplikacji konsoli, za pomocą `--console` argument wiersza polecenia lub jeśli jest dołączony debuger:</span><span class="sxs-lookup"><span data-stu-id="60715-147">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="60715-148">Ponieważ konfiguracja platformy ASP.NET Core wymaga pary nazwa wartość dla argumentów wiersza polecenia `--console` przełącznik został usunięty, zanim argumenty są przekazywane do [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="60715-148">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="60715-149">`isService` nie jest przekazywane z `Main` do `CreateWebHostBuilder` ponieważ podpis `CreateWebHostBuilder` musi być `CreateWebHostBuilder(string[])` aby [Testowanie integracji](xref:test/integration-tests) działało poprawnie.</span><span class="sxs-lookup"><span data-stu-id="60715-149">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="60715-150">Obsługa zatrzymanie i uruchomienie zdarzenia</span><span class="sxs-lookup"><span data-stu-id="60715-150">Handle stopping and starting events</span></span>

<span data-ttu-id="60715-151">Aby obsłużyć [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), i [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) zdarzenia, należy wprowadzić następujące zmiany dodatkowe:</span><span class="sxs-lookup"><span data-stu-id="60715-151">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="60715-152">Utwórz klasę, która pochodzi od klasy [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="60715-152">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="60715-153">Tworzenie metody rozszerzenia dla [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) niestandardowego, który przekazuje `WebHostService` do [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="60715-153">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="60715-154">W `Program.Main`, wywołanie nowej metody rozszerzenia `RunAsCustomService`, zamiast [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="60715-154">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=14)]

   > [!NOTE]
   > <span data-ttu-id="60715-155">`isService` nie jest przekazywane z `Main` do `CreateWebHostBuilder` ponieważ podpis `CreateWebHostBuilder` musi być `CreateWebHostBuilder(string[])` aby [Testowanie integracji](xref:test/integration-tests) działało poprawnie.</span><span class="sxs-lookup"><span data-stu-id="60715-155">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="60715-156">Jeśli niestandardowa `WebHostService` kod wymaga usługi z wstrzykiwanie zależności (np. Rejestrator), Uzyskaj ją z [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) właściwości:</span><span class="sxs-lookup"><span data-stu-id="60715-156">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="60715-157">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="60715-157">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="60715-158">Usługi wchodzić w interakcje z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub moduł równoważenia obciążenia, które mogą wymagać dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="60715-158">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="60715-159">Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="60715-159">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="kestrel-endpoint-configuration"></a><span data-ttu-id="60715-160">Konfiguracja punktu końcowego kestrel</span><span class="sxs-lookup"><span data-stu-id="60715-160">Kestrel endpoint configuration</span></span>

<span data-ttu-id="60715-161">Instrukcje dotyczące konfiguracji punktu końcowego Kestrel, w tym konfiguracja protokołu HTTPS i SNI pomocy technicznej, zobacz [konfiguracji punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="60715-161">For information on Kestrel endpoint configuration, including HTTPS configuration and SNI support, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
