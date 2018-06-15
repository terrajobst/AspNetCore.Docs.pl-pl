---
title: Host platformy ASP.NET Core w usłudze systemu Windows
author: guardrex
description: Dowiedz się, jak udostępniać aplikacji platformy ASP.NET Core w usłudze systemu Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 5eba685bbe55d43bb063a01798bc691a1ba0d6fc
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613089"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="8f81b-103">Host platformy ASP.NET Core w usłudze systemu Windows</span><span class="sxs-lookup"><span data-stu-id="8f81b-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="8f81b-104">Przez [Luke Latham](https://github.com/guardrex) i [Dykstra niestandardowy](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8f81b-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="8f81b-105">Aplikacja platformy ASP.NET Core może być obsługiwany w systemie Windows bez użycia usługi IIS jako [usługi systemu Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="8f81b-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="8f81b-106">Gdy hostowany jako usługa systemu Windows, aplikacja może automatycznie uruchamiania po ponownym uruchomieniu i awarii bez udziału człowieka.</span><span class="sxs-lookup"><span data-stu-id="8f81b-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="8f81b-107">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8f81b-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="8f81b-108">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="8f81b-108">Get started</span></span>

<span data-ttu-id="8f81b-109">Następujące minimalne zmiany są wymagane do skonfigurowania istniejącego projektu platformy ASP.NET Core do uruchamiania w usłudze:</span><span class="sxs-lookup"><span data-stu-id="8f81b-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="8f81b-110">W pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="8f81b-110">In the project file:</span></span>

   1. <span data-ttu-id="8f81b-111">Potwierdzić obecność identyfikator środowiska uruchomieniowego lub dodanie go do  **\<PropertyGroup >** zawierający platformy docelowej:</span><span class="sxs-lookup"><span data-stu-id="8f81b-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. <span data-ttu-id="8f81b-112">Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="8f81b-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="8f81b-113">Wprowadź następujące zmiany w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="8f81b-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="8f81b-114">Wywołanie [hosta. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) zamiast `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="8f81b-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="8f81b-115">Jeśli kod wywołuje `UseContentRoot`, użyj ścieżki do aplikacji opublikowane lokalizacji, a nie `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="8f81b-115">If the code calls `UseContentRoot`, use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     <span data-ttu-id="8f81b-116">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]</span><span class="sxs-lookup"><span data-stu-id="8f81b-116">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]</span></span>

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     <span data-ttu-id="8f81b-117">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]</span><span class="sxs-lookup"><span data-stu-id="8f81b-117">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]</span></span>

     ::: moniker-end

1. <span data-ttu-id="8f81b-118">Publikowanie aplikacji w folderze.</span><span class="sxs-lookup"><span data-stu-id="8f81b-118">Publish the app to a folder.</span></span> <span data-ttu-id="8f81b-119">Użyj [publikowania dotnet](/dotnet/articles/core/tools/dotnet-publish) lub [profilu publikowania programu Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) który publikuje do folderu.</span><span class="sxs-lookup"><span data-stu-id="8f81b-119">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

   <span data-ttu-id="8f81b-120">Aby opublikować przykładową aplikację z poziomu wiersza polecenia, uruchom następujące polecenie w oknie konsoli z folderu projektu:</span><span class="sxs-lookup"><span data-stu-id="8f81b-120">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release --output c:\svc
   ```

1. <span data-ttu-id="8f81b-121">Użyj [sc.exe](https://technet.microsoft.com/library/bb490995) narzędzia wiersza polecenia, aby utworzyć usługę (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`).</span><span class="sxs-lookup"><span data-stu-id="8f81b-121">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`).</span></span> <span data-ttu-id="8f81b-122">`binPath` Wartość jest ścieżką do pliku wykonywalnego aplikacji, która zawiera nazwę pliku wykonywalnego.</span><span class="sxs-lookup"><span data-stu-id="8f81b-122">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="8f81b-123">**Odstęp między znak równości i znaku cudzysłowu, która rozpoczyna się ścieżka jest wymagana.**</span><span class="sxs-lookup"><span data-stu-id="8f81b-123">**The space between the equal sign and the quote character that starts the path is required.**</span></span>

   <span data-ttu-id="8f81b-124">Przykładowa aplikacja i polecenia, który jest zgodny usługa jest:</span><span class="sxs-lookup"><span data-stu-id="8f81b-124">For the sample app and command that follows, the service is:</span></span>

   * <span data-ttu-id="8f81b-125">O nazwie **Moja_usługa**.</span><span class="sxs-lookup"><span data-stu-id="8f81b-125">Named **MyService**.</span></span>
   * <span data-ttu-id="8f81b-126">Opublikowany *c:\\svc* folderu.</span><span class="sxs-lookup"><span data-stu-id="8f81b-126">Published to *c:\\svc* folder.</span></span>
   * <span data-ttu-id="8f81b-127">Aplikację pliku wykonywalnego o nazwie *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="8f81b-127">Has an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="8f81b-128">Otwórz powłokę wiersza polecenia z uprawnieniami administracyjnymi i uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="8f81b-128">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   ```

   <span data-ttu-id="8f81b-129">**Upewnij się, że ma miejsce, między `binPath=` argumentu i jego wartość.**</span><span class="sxs-lookup"><span data-stu-id="8f81b-129">**Make sure the space is present between the `binPath=` argument and its value.**</span></span>

1. <span data-ttu-id="8f81b-130">Uruchom usługę z `sc start <SERVICE_NAME>` polecenia.</span><span class="sxs-lookup"><span data-stu-id="8f81b-130">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="8f81b-131">Aby uruchomić usługę aplikacji przykładowej, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="8f81b-131">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="8f81b-132">Polecenie zajmuje kilka sekund, aby uruchomić usługę.</span><span class="sxs-lookup"><span data-stu-id="8f81b-132">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="8f81b-133">`sc query <SERVICE_NAME>` Polecenia może służyć do sprawdzania stanu usługi można określić jego stanu:</span><span class="sxs-lookup"><span data-stu-id="8f81b-133">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="8f81b-134">Aby sprawdzić stan usługi aplikacji przykładowej, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="8f81b-134">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="8f81b-135">Gdy usługa jest w `RUNNING` stanu i usługi w przypadku aplikacji sieci web, przejdź aplikację w jego ścieżki (domyślnie `http://localhost:5000`, która przekierowuje do `https://localhost:5001` przy użyciu [HTTPS przekierowanie w oprogramowaniu pośredniczącym](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="8f81b-135">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="8f81b-136">Usługi aplikacji przykładowej Przeglądaj aplikację w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="8f81b-136">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="8f81b-137">Zatrzymaj usługę z `sc stop <SERVICE_NAME>` polecenia.</span><span class="sxs-lookup"><span data-stu-id="8f81b-137">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="8f81b-138">Polecenie zatrzymuje usługę aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="8f81b-138">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="8f81b-139">Z niewielkim opóźnieniem zatrzymania usługi, odinstaluj usługę z `sc delete <SERVICE_NAME>` polecenia.</span><span class="sxs-lookup"><span data-stu-id="8f81b-139">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="8f81b-140">Sprawdź stan usługi aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="8f81b-140">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="8f81b-141">Gdy przykładowej aplikacji usługi jest w `STOPPED` stanu, użyj następującego polecenia, aby odinstalować usługę aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="8f81b-141">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="8f81b-142">Umożliwiają uruchamianie poza usługą</span><span class="sxs-lookup"><span data-stu-id="8f81b-142">Provide a way to run outside of a service</span></span>

<span data-ttu-id="8f81b-143">Możliwe jest łatwiejsze testowanie i debugowanie podczas uruchamiania poza usługą, dzięki czemu zwyczajowe można dodać kod, który wywołuje `RunAsService` tylko w niektórych warunkach.</span><span class="sxs-lookup"><span data-stu-id="8f81b-143">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="8f81b-144">Na przykład aplikacja może działać jako aplikację konsoli z `--console` argumentu wiersza polecenia lub jeśli jest dołączony debuger:</span><span class="sxs-lookup"><span data-stu-id="8f81b-144">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8f81b-145">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]</span><span class="sxs-lookup"><span data-stu-id="8f81b-145">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]</span></span>

<span data-ttu-id="8f81b-146">Ponieważ konfiguracja platformy ASP.NET Core wymaga pary nazwa wartość dla argumentów wiersza polecenia, `--console` przełącznik jest usunięty przed argumenty są przekazywane do [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="8f81b-146">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8f81b-147">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]</span><span class="sxs-lookup"><span data-stu-id="8f81b-147">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]</span></span>

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="8f81b-148">Obsługa zatrzymywania i uruchamiania zdarzeń</span><span class="sxs-lookup"><span data-stu-id="8f81b-148">Handle stopping and starting events</span></span>

<span data-ttu-id="8f81b-149">Do obsługi [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), i [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) zdarzenia, wprowadź następujące zmiany dodatkowe:</span><span class="sxs-lookup"><span data-stu-id="8f81b-149">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="8f81b-150">Utwórz klasę pochodną [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="8f81b-150">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="8f81b-151">Create — metoda rozszerzenia dla [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) niestandardowego, który przekazuje `WebHostService` do [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="8f81b-151">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="8f81b-152">W `Program.Main`, wywołaj nowej metody rozszerzenia `RunAsCustomService`, zamiast [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="8f81b-152">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   <span data-ttu-id="8f81b-153">[!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]</span><span class="sxs-lookup"><span data-stu-id="8f81b-153">[!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   <span data-ttu-id="8f81b-154">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]</span><span class="sxs-lookup"><span data-stu-id="8f81b-154">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]</span></span>

   ::: moniker-end

<span data-ttu-id="8f81b-155">Jeśli niestandardowa `WebHostService` kod wymaga usługi z iniekcji zależności (np. Rejestrator), Uzyskaj ją z [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) właściwości:</span><span class="sxs-lookup"><span data-stu-id="8f81b-155">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="8f81b-156">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="8f81b-156">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="8f81b-157">Usługi, które interakcję z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub usługę równoważenia obciążenia mogą wymagać dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8f81b-157">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="8f81b-158">Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="8f81b-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="kestrel-endpoint-configuration"></a><span data-ttu-id="8f81b-159">Konfiguracja punktu końcowego kestrel</span><span class="sxs-lookup"><span data-stu-id="8f81b-159">Kestrel endpoint configuration</span></span>

<span data-ttu-id="8f81b-160">Aby uzyskać informacje dotyczące konfiguracji punktu końcowego Kestrel, w tym konfiguracja protokołu HTTPS i SNI pomocy technicznej, zobacz [konfiguracji punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="8f81b-160">For information on Kestrel endpoint configuration, including HTTPS configuration and SNI support, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
