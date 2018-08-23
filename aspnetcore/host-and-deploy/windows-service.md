---
title: Host platformy ASP.NET Core w usłudze Windows
author: guardrex
description: Dowiedz się, jak udostępnić aplikację ASP.NET Core w usłudze Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 68afe77b05a717cffecc32188f18e9fde208b81f
ms.sourcegitcommit: 3ca20ed63bf1469f4365f0c1fbd00c98a3191c84
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/17/2018
ms.locfileid: "41756268"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="3ea86-103">Host platformy ASP.NET Core w usłudze Windows</span><span class="sxs-lookup"><span data-stu-id="3ea86-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="3ea86-104">Przez [Luke Latham](https://github.com/guardrex) i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="3ea86-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="3ea86-105">Aplikacji ASP.NET Core może być hostowana na Windows bez korzystania z usług IIS jako [usługi Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="3ea86-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="3ea86-106">Gdy hostowany jako usługa Windows, aplikacja może automatycznie po ponownym uruchomieniu i ulega awarii, nie wymagając interwencji człowieka.</span><span class="sxs-lookup"><span data-stu-id="3ea86-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="3ea86-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3ea86-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="3ea86-108">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="3ea86-108">Get started</span></span>

<span data-ttu-id="3ea86-109">Następujące minimalne zmiany są wymagane do skonfigurowania istniejący projekt ASP.NET Core, do uruchamiania w usłudze:</span><span class="sxs-lookup"><span data-stu-id="3ea86-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="3ea86-110">W pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="3ea86-110">In the project file:</span></span>

   1. <span data-ttu-id="3ea86-111">Potwierdzić obecność identyfikator środowiska uruchomieniowego, albo dodaj go do  **\<PropertyGroup >** zawierający platforma docelowa:</span><span class="sxs-lookup"><span data-stu-id="3ea86-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>

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

   1. <span data-ttu-id="3ea86-112">Dodaj odwołania do pakietu dla [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="3ea86-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="3ea86-113">Wprowadź następujące zmiany w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="3ea86-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="3ea86-114">Wywołaj [hosta. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) zamiast `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="3ea86-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="3ea86-115">Wywołaj [UseContentRoot](xref:fundamentals/host/web-host#content-root) i ścieżka do aplikacji — opublikowane lokalizacji zamiast używania `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="3ea86-115">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="3ea86-116">Publikowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ea86-116">Publish the app.</span></span> <span data-ttu-id="3ea86-117">Użyj [publikowania dotnet](/dotnet/articles/core/tools/dotnet-publish) lub [profilu publikowania w programie Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="3ea86-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span> <span data-ttu-id="3ea86-118">Jeśli używasz programu Visual Studio, wybierz opcję **FolderProfile**.</span><span class="sxs-lookup"><span data-stu-id="3ea86-118">When using a Visual Studio, select the **FolderProfile**.</span></span>

   <span data-ttu-id="3ea86-119">Aby opublikować przykładową aplikację z poziomu wiersza polecenia, uruchom następujące polecenie w oknie konsoli z folderu projektu:</span><span class="sxs-lookup"><span data-stu-id="3ea86-119">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="3ea86-120">Użyj [sc.exe](https://technet.microsoft.com/library/bb490995) narzędzie wiersza polecenia, aby utworzyć usługę.</span><span class="sxs-lookup"><span data-stu-id="3ea86-120">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="3ea86-121">`binPath` Wartość jest ścieżką do pliku wykonywalnego aplikacji, która zawiera nazwę pliku wykonywalnego.</span><span class="sxs-lookup"><span data-stu-id="3ea86-121">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="3ea86-122">**Odstęp między znakiem równości i znaku cudzysłowu na początku ścieżki jest wymagana.**</span><span class="sxs-lookup"><span data-stu-id="3ea86-122">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="3ea86-123">Dla usługi, opublikowane w folderze projektu, użyj ścieżki do *publikowania* folder, aby utworzyć usługę.</span><span class="sxs-lookup"><span data-stu-id="3ea86-123">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="3ea86-124">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="3ea86-124">In the following example:</span></span>

   * <span data-ttu-id="3ea86-125">Projekt, który znajduje się w `c:\my_services\AspNetCoreService` folderu.</span><span class="sxs-lookup"><span data-stu-id="3ea86-125">The project resides in the `c:\my_services\AspNetCoreService` folder.</span></span>
   * <span data-ttu-id="3ea86-126">Projekt zostanie opublikowany w `Release` konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="3ea86-126">The project is published in `Release` configuration.</span></span>
   * <span data-ttu-id="3ea86-127">Moniker Framework docelowych (TFM) jest `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="3ea86-127">The Target Framework Moniker (TFM) is `netcoreapp2.1`.</span></span>
   * <span data-ttu-id="3ea86-128">Identyfikator środowiska uruchomieniowego (RID) jest `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="3ea86-128">The Runtime Identifer (RID) is `win7-x64`.</span></span>
   * <span data-ttu-id="3ea86-129">Nosi nazwę pliku wykonywalnego aplikacji *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="3ea86-129">The app executable is named *AspNetCoreService.exe*.</span></span>
   * <span data-ttu-id="3ea86-130">Usługa jest o nazwie **Moja_usługa**.</span><span class="sxs-lookup"><span data-stu-id="3ea86-130">The service is named **MyService**.</span></span>

   <span data-ttu-id="3ea86-131">Przykład:</span><span class="sxs-lookup"><span data-stu-id="3ea86-131">Example:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="3ea86-132">Upewnij się, ma miejsce, między `binPath=` argumentu i jego wartość.</span><span class="sxs-lookup"><span data-stu-id="3ea86-132">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="3ea86-133">Publikowanie i uruchom usługę z innego folderu:</span><span class="sxs-lookup"><span data-stu-id="3ea86-133">To publish and start the service from a different folder:</span></span>
   
      1. <span data-ttu-id="3ea86-134">Użyj [--dane wyjściowe &lt;OUTPUT_DIRECTORY&gt; ](/dotnet/core/tools/dotnet-publish#options) opcja `dotnet publish` polecenia.</span><span class="sxs-lookup"><span data-stu-id="3ea86-134">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span> <span data-ttu-id="3ea86-135">Jeśli używasz programu Visual Studio, należy skonfigurować **lokalizacji docelowej** w **FolderProfile** strona właściwości publikowania przed wybraniem **Publikuj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="3ea86-135">If using Visual Studio, configure the **Target Location** in the **FolderProfile** publish property page before selecting the **Publish** button.</span></span>
   1. <span data-ttu-id="3ea86-136">Tworzenie usługi przy użyciu `sc.exe` polecenia przy użyciu ścieżki folderu danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="3ea86-136">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="3ea86-137">Obejmują nazwę pliku wykonywalnego usługi w ścieżce podanej do `binPath`.</span><span class="sxs-lookup"><span data-stu-id="3ea86-137">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="3ea86-138">Uruchom usługę za pomocą `sc start <SERVICE_NAME>` polecenia.</span><span class="sxs-lookup"><span data-stu-id="3ea86-138">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="3ea86-139">Aby uruchomić usługę aplikacji przykładowej, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="3ea86-139">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="3ea86-140">Polecenie zajmuje kilka sekund, aby uruchomić usługę.</span><span class="sxs-lookup"><span data-stu-id="3ea86-140">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="3ea86-141">`sc query <SERVICE_NAME>` Polecenia mogą służyć do sprawdzania stanu usługi, aby określić stan:</span><span class="sxs-lookup"><span data-stu-id="3ea86-141">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="3ea86-142">Użyj następującego polecenia, aby sprawdzić stan usługi aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="3ea86-142">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="3ea86-143">Kiedy usługa jest w `RUNNING` stanu i usługi w przypadku aplikacji sieci web, przeglądanie aplikacji w ścieżce (domyślnie `http://localhost:5000`, który przekierowuje do `https://localhost:5001` przy użyciu [HTTPS przekierowanie w oprogramowaniu pośredniczącym](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="3ea86-143">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="3ea86-144">Usługa app service przykładowego, można przeglądać w tej aplikacji w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="3ea86-144">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="3ea86-145">Zatrzymaj usługę za pomocą `sc stop <SERVICE_NAME>` polecenia.</span><span class="sxs-lookup"><span data-stu-id="3ea86-145">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="3ea86-146">Następujące polecenie zatrzymuje usługę aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="3ea86-146">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="3ea86-147">Po krótkiej chwili zatrzymania usługi, odinstaluj usługę za pomocą `sc delete <SERVICE_NAME>` polecenia.</span><span class="sxs-lookup"><span data-stu-id="3ea86-147">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="3ea86-148">Sprawdź stan usługi aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="3ea86-148">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="3ea86-149">Gdy usługa app service przykładowego jest w `STOPPED` stanu, użyj następującego polecenia, aby odinstalować usługę aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="3ea86-149">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="3ea86-150">Zapewnia możliwość uruchamiania poza usługą</span><span class="sxs-lookup"><span data-stu-id="3ea86-150">Provide a way to run outside of a service</span></span>

<span data-ttu-id="3ea86-151">Znacznie łatwiej testować i debugować podczas uruchamiania spoza niej, więc zwyczajowego, aby dodać kod, który wywołuje `RunAsService` tylko pod pewnymi warunkami.</span><span class="sxs-lookup"><span data-stu-id="3ea86-151">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="3ea86-152">Na przykład aplikacja może działać jako aplikacji konsoli, za pomocą `--console` argument wiersza polecenia lub jeśli jest dołączony debuger:</span><span class="sxs-lookup"><span data-stu-id="3ea86-152">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="3ea86-153">Ponieważ konfiguracja platformy ASP.NET Core wymaga pary nazwa wartość dla argumentów wiersza polecenia `--console` przełącznik został usunięty, zanim argumenty są przekazywane do [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="3ea86-153">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="3ea86-154">`isService` nie jest przekazywane z `Main` do `CreateWebHostBuilder` ponieważ podpis `CreateWebHostBuilder` musi być `CreateWebHostBuilder(string[])` aby [Testowanie integracji](xref:test/integration-tests) działało poprawnie.</span><span class="sxs-lookup"><span data-stu-id="3ea86-154">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="3ea86-155">Obsługa zatrzymanie i uruchomienie zdarzenia</span><span class="sxs-lookup"><span data-stu-id="3ea86-155">Handle stopping and starting events</span></span>

<span data-ttu-id="3ea86-156">Aby obsłużyć [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), i [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) zdarzenia, należy wprowadzić następujące zmiany dodatkowe:</span><span class="sxs-lookup"><span data-stu-id="3ea86-156">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="3ea86-157">Utwórz klasę, która pochodzi od klasy [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="3ea86-157">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="3ea86-158">Tworzenie metody rozszerzenia dla [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) niestandardowego, który przekazuje `WebHostService` do [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="3ea86-158">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="3ea86-159">W `Program.Main`, wywołanie nowej metody rozszerzenia `RunAsCustomService`, zamiast [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="3ea86-159">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="3ea86-160">`isService` nie jest przekazywane z `Main` do `CreateWebHostBuilder` ponieważ podpis `CreateWebHostBuilder` musi być `CreateWebHostBuilder(string[])` aby [Testowanie integracji](xref:test/integration-tests) działało poprawnie.</span><span class="sxs-lookup"><span data-stu-id="3ea86-160">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="3ea86-161">Jeśli niestandardowa `WebHostService` kod wymaga usługi z wstrzykiwanie zależności (np. Rejestrator), Uzyskaj ją z [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) właściwości:</span><span class="sxs-lookup"><span data-stu-id="3ea86-161">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="3ea86-162">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="3ea86-162">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="3ea86-163">Usługi wchodzić w interakcje z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub moduł równoważenia obciążenia, które mogą wymagać dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="3ea86-163">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="3ea86-164">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="3ea86-164">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="3ea86-165">Konfigurowanie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="3ea86-165">Configure HTTPS</span></span>

<span data-ttu-id="3ea86-166">Określ [konfiguracji punktu końcowego HTTPS serwera Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="3ea86-166">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="3ea86-167">Bieżący katalog i katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="3ea86-167">Current directory and content root</span></span>

<span data-ttu-id="3ea86-168">Bieżący katalog roboczy zwracany przez wywołanie metody `Directory.GetCurrentDirectory()` dla usługi Windows jest *C:\WINDOWS\system32* folderu.</span><span class="sxs-lookup"><span data-stu-id="3ea86-168">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\WINDOWS\system32* folder.</span></span> <span data-ttu-id="3ea86-169">*System32* folder nie jest odpowiednią lokalizację do przechowywania plików usługi (na przykład pliki ustawień).</span><span class="sxs-lookup"><span data-stu-id="3ea86-169">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="3ea86-170">Użyj jednej z następujących metod do utrzymywania i uzyskać dostęp do zasobów i plików ustawień za pomocą usługi [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) przy użyciu [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="3ea86-170">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="3ea86-171">Użyj ścieżki katalogu głównego zawartości.</span><span class="sxs-lookup"><span data-stu-id="3ea86-171">Use the content root path.</span></span> <span data-ttu-id="3ea86-172">`IHostingEnvironment.ContentRootPath` Tej samej ścieżki, które są udostępniane `binPath` argumentów podczas tworzenia usługi.</span><span class="sxs-lookup"><span data-stu-id="3ea86-172">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="3ea86-173">Zamiast używania `Directory.GetCurrentDirectory()` Tworzenie ścieżki do plików ustawień, użyj ścieżki katalogu głównego zawartości i przechowuje pliki w katalogu głównym zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ea86-173">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="3ea86-174">Store pliki w odpowiedniej lokalizacji na dysku.</span><span class="sxs-lookup"><span data-stu-id="3ea86-174">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="3ea86-175">Określ ścieżkę bezwzględną z `SetBasePath` do folderu zawierającego pliki.</span><span class="sxs-lookup"><span data-stu-id="3ea86-175">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3ea86-176">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3ea86-176">Additional resources</span></span>

* <span data-ttu-id="3ea86-177">[Konfiguracja punktu końcowego kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym konfiguracja protokołu HTTPS i obsługa SNI)</span><span class="sxs-lookup"><span data-stu-id="3ea86-177">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
