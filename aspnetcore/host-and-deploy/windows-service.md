---
title: ASP.NET Core hosta w usłudze systemu Windows
author: guardrex
description: Dowiedz się, jak hostować aplikację ASP.NET Core w usłudze systemu Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/06/2020
uid: host-and-deploy/windows-service
ms.openlocfilehash: 71f7bf3f5dcf8068d0ada03675ef7948267b79f4
ms.sourcegitcommit: bd896935e91236e03241f75e6534ad6debcecbbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/06/2020
ms.locfileid: "77044897"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="fc8a3-103">ASP.NET Core hosta w usłudze systemu Windows</span><span class="sxs-lookup"><span data-stu-id="fc8a3-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="fc8a3-104">Autor [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fc8a3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fc8a3-105">Aplikacja ASP.NET Core może być hostowana w systemie Windows jako [Usługa systemu Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) bez korzystania z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="fc8a3-106">Gdy usługa jest hostowana w systemie Windows, aplikacja jest uruchamiana automatycznie po ponownym uruchomieniu serwera.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="fc8a3-107">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fc8a3-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fc8a3-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="fc8a3-108">Prerequisites</span></span>

* [<span data-ttu-id="fc8a3-109">ASP.NET Core zestaw SDK 2,1 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="fc8a3-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="fc8a3-110">Program PowerShell 6,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="fc8a3-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="fc8a3-111">Szablon usługi procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="fc8a3-111">Worker Service template</span></span>

<span data-ttu-id="fc8a3-112">Szablon usługi ASP.NET Core Worker zapewnia punkt początkowy do pisania długotrwałych aplikacji usługi.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="fc8a3-113">Aby użyć szablonu jako podstawy dla aplikacji usługi systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="fc8a3-114">Utwórz aplikację usługi procesu roboczego na podstawie szablonu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="fc8a3-115">Postępuj zgodnie ze wskazówkami w sekcji [Konfiguracja aplikacji](#app-configuration) , aby zaktualizować aplikację usługi procesu roboczego tak, aby mogła ona działać jako usługa systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

::: moniker-end

## <a name="app-configuration"></a><span data-ttu-id="fc8a3-116">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="fc8a3-116">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fc8a3-117">Aplikacja wymaga odwołania do pakietu dla elementu [Microsoft. Extensions. hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-117">The app requires a package reference for [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span></span>

<span data-ttu-id="fc8a3-118">`IHostBuilder.UseWindowsService` jest wywoływana podczas kompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-118">`IHostBuilder.UseWindowsService` is called when building the host.</span></span> <span data-ttu-id="fc8a3-119">Jeśli aplikacja działa jako usługa systemu Windows, Metoda:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-119">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="fc8a3-120">Ustawia okres istnienia hosta do `WindowsServiceLifetime`.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-120">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="fc8a3-121">Ustawia [katalog główny zawartości](xref:fundamentals/index#content-root) na [AppContext. BaseDirectory](xref:System.AppContext.BaseDirectory).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-121">Sets the [content root](xref:fundamentals/index#content-root) to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span> <span data-ttu-id="fc8a3-122">Aby uzyskać więcej informacji, zapoznaj się z sekcją [Current Directory i root Content](#current-directory-and-content-root) .</span><span class="sxs-lookup"><span data-stu-id="fc8a3-122">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
* <span data-ttu-id="fc8a3-123">Włącza rejestrowanie w dzienniku zdarzeń:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-123">Enables logging to the event log:</span></span>
  * <span data-ttu-id="fc8a3-124">Nazwa aplikacji jest używana jako domyślna nazwa źródła.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-124">The application name is used as the default source name.</span></span>
  * <span data-ttu-id="fc8a3-125">Domyślny poziom rejestrowania jest *ostrzegawczy* lub wyższy dla aplikacji opartej na szablonie ASP.NET Core, który wywołuje `CreateDefaultBuilder` do skompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-125">The default log level is *Warning* or higher for an app based on an ASP.NET Core template that calls `CreateDefaultBuilder` to build the host.</span></span>
  * <span data-ttu-id="fc8a3-126">Zastąp domyślny poziom dziennika kluczem `Logging:EventLog:LogLevel:Default` w pliku *appSettings. json*/*appSettings. { Environment}. JSON* lub inny dostawca konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-126">Override the default log level with the `Logging:EventLog:LogLevel:Default` key in *appsettings.json*/*appsettings.{Environment}.json* or other configuration provider.</span></span>
  * <span data-ttu-id="fc8a3-127">Tylko Administratorzy mogą tworzyć nowe źródła zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-127">Only administrators can create new event sources.</span></span> <span data-ttu-id="fc8a3-128">Gdy nie można utworzyć źródła zdarzeń przy użyciu nazwy aplikacji, w źródle *aplikacji* jest rejestrowane ostrzeżenie, a dzienniki zdarzeń są wyłączone.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-128">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

<span data-ttu-id="fc8a3-129">W `CreateHostBuilder` *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-129">In `CreateHostBuilder` of *Program.cs*:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseWindowsService()
    ...
```

<span data-ttu-id="fc8a3-130">Następujące przykładowe aplikacje zostały dołączone do tego tematu:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-130">The following sample apps accompany this topic:</span></span>

* <span data-ttu-id="fc8a3-131">Przykład usługi procesu roboczego w tle &ndash; przykład aplikacji innej niż sieć Web oparta na [szablonie usługi procesu roboczego](#worker-service-template) , który korzysta z [usług hostowanych](xref:fundamentals/host/hosted-services) na potrzeby zadań w tle.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-131">Background Worker Service Sample &ndash; A non-web app sample based on the [Worker Service template](#worker-service-template) that uses [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>
* <span data-ttu-id="fc8a3-132">Przykład App Service sieci Web &ndash; przykład aplikacji internetowej Razor Pages, która działa jako usługa systemu Windows z [usługami hostowanymi](xref:fundamentals/host/hosted-services) dla zadań w tle.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-132">Web App Service Sample &ndash; A Razor Pages web app sample that runs as a Windows Service with [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>

<span data-ttu-id="fc8a3-133">Wskazówki dotyczące MVC można znaleźć w artykułach w obszarze <xref:mvc/overview> i <xref:migration/22-to-30>.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-133">For MVC guidance, see the articles under <xref:mvc/overview> and <xref:migration/22-to-30>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fc8a3-134">Aplikacja wymaga odwołania do pakietów dla elementu [Microsoft. AspNetCore. hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) i [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-134">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="fc8a3-135">Aby przetestować i debugować w przypadku uruchamiania poza usługą, należy dodać kod, aby określić, czy aplikacja działa jako usługa, czy Aplikacja konsolowa.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-135">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="fc8a3-136">Sprawdź, czy debuger jest podłączony lub czy jest dostępny przełącznik `--console`.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-136">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="fc8a3-137">Jeśli którykolwiek z warunków jest spełniony (aplikacja nie jest uruchomiona jako usługa), wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-137">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="fc8a3-138">Jeśli warunki są fałszywe (aplikacja jest uruchamiana jako usługa):</span><span class="sxs-lookup"><span data-stu-id="fc8a3-138">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="fc8a3-139">Wywołaj <xref:System.IO.Directory.SetCurrentDirectory*> i użyj ścieżki do lokalizacji opublikowanej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-139">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="fc8a3-140">Nie wywołuj <xref:System.IO.Directory.GetCurrentDirectory*>, aby uzyskać ścieżkę, ponieważ aplikacja usługi systemu Windows zwraca folder *C:\\Windows\\system32* w przypadku wywołania <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-140">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="fc8a3-141">Aby uzyskać więcej informacji, zapoznaj się z sekcją [Current Directory i root Content](#current-directory-and-content-root) .</span><span class="sxs-lookup"><span data-stu-id="fc8a3-141">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="fc8a3-142">Ten krok jest wykonywany przed skonfigurowaniem aplikacji w `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-142">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="fc8a3-143">Wywołaj <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>, aby uruchomić aplikację jako usługę.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-143">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="fc8a3-144">Ponieważ [dostawca konfiguracji wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) wymaga par nazwa-wartość dla argumentów wiersza polecenia, przełącznik `--console` jest usuwany z argumentów, zanim <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> odbierze argumenty.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-144">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="fc8a3-145">Aby zapisać w dzienniku zdarzeń systemu Windows, Dodaj dostawcę EventLog do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-145">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="fc8a3-146">Ustaw poziom rejestrowania za pomocą klucza `Logging:LogLevel:Default` w pliku *appSettings. Plik Product. JSON* .</span><span class="sxs-lookup"><span data-stu-id="fc8a3-146">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="fc8a3-147">W poniższym przykładzie z przykładowej aplikacji `RunAsCustomService` jest wywoływana zamiast <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> w celu obsługi zdarzeń okresu istnienia w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-147">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="fc8a3-148">Aby uzyskać więcej informacji, zobacz sekcję [Obsługa zdarzeń dotyczących uruchamiania i zatrzymywania](#handle-starting-and-stopping-events) .</span><span class="sxs-lookup"><span data-stu-id="fc8a3-148">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="fc8a3-149">Typ wdrożenia</span><span class="sxs-lookup"><span data-stu-id="fc8a3-149">Deployment type</span></span>

<span data-ttu-id="fc8a3-150">Aby uzyskać informacje i porady dotyczące scenariuszy wdrażania, zobacz [wdrażanie aplikacji .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-150">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="fc8a3-151">SDK</span><span class="sxs-lookup"><span data-stu-id="fc8a3-151">SDK</span></span>

<span data-ttu-id="fc8a3-152">W przypadku usługi opartej na aplikacji sieci Web używającej platform Razor Pages lub MVC należy określić zestaw SDK sieci Web w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-152">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="fc8a3-153">Jeśli usługa wykonuje tylko zadania w tle (na przykład [usługi hostowane](xref:fundamentals/host/hosted-services)), określ zestaw SDK procesu roboczego w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-153">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="fc8a3-154">Wdrożenie zależne od platformy (FDD)</span><span class="sxs-lookup"><span data-stu-id="fc8a3-154">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="fc8a3-155">Wdrożenie zależne od platformy (FDD) zależy od obecności udostępnionej systemowej wersji platformy .NET Core w systemie docelowym.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-155">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="fc8a3-156">Po przyjęciu scenariusza FDD zgodnie ze wskazówkami zawartymi w tym artykule zestaw SDK tworzy plik wykonywalny ( *. exe*), nazywany *plik wykonywalny zależny od platformy*.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-156">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fc8a3-157">Jeśli używasz [zestawu SDK sieci Web](#sdk), plik *Web. config* , który jest zwykle tworzony podczas publikowania aplikacji ASP.NET Core, jest zbędny dla aplikacji usług systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-157">If using the [Web SDK](#sdk), a *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="fc8a3-158">Aby wyłączyć tworzenie pliku *Web. config* , należy dodać właściwość `<IsTransformWebConfigDisabled>` ustawioną na `true`.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-158">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="fc8a3-159">[Identyfikator środowiska uruchomieniowego systemu Windows (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) zawiera platformę docelową.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-159">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="fc8a3-160">W poniższym przykładzie identyfikator RID jest ustawiony na `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-160">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="fc8a3-161">Właściwość `<SelfContained>` jest ustawiona na `false`.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-161">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="fc8a3-162">Te właściwości instruują zestaw SDK, aby wygenerował plik wykonywalny (*exe*) dla systemu Windows i aplikację, która zależy od współużytkowanej platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-162">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="fc8a3-163">Plik *Web. config* , który jest zwykle tworzony podczas publikowania aplikacji ASP.NET Core, nie jest konieczny dla aplikacji usług systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-163">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="fc8a3-164">Aby wyłączyć tworzenie pliku *Web. config* , należy dodać właściwość `<IsTransformWebConfigDisabled>` ustawioną na `true`.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-164">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="fc8a3-165">[Identyfikator środowiska uruchomieniowego systemu Windows (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) zawiera platformę docelową.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-165">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="fc8a3-166">W poniższym przykładzie identyfikator RID jest ustawiony na `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-166">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="fc8a3-167">Właściwość `<SelfContained>` jest ustawiona na `false`.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-167">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="fc8a3-168">Te właściwości instruują zestaw SDK, aby wygenerował plik wykonywalny (*exe*) dla systemu Windows i aplikację, która zależy od współużytkowanej platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-168">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="fc8a3-169">Właściwość `<UseAppHost>` jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-169">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="fc8a3-170">Ta właściwość zapewnia usługę ze ścieżką aktywacji (plik wykonywalny, *exe*) dla FDD.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-170">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="fc8a3-171">Plik *Web. config* , który jest zwykle tworzony podczas publikowania aplikacji ASP.NET Core, nie jest konieczny dla aplikacji usług systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-171">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="fc8a3-172">Aby wyłączyć tworzenie pliku *Web. config* , należy dodać właściwość `<IsTransformWebConfigDisabled>` ustawioną na `true`.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-172">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="fc8a3-173">Wdrażanie samodzielne (SCD)</span><span class="sxs-lookup"><span data-stu-id="fc8a3-173">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="fc8a3-174">Wdrożenie samodzielne (SCD) nie polega na obecności struktury udostępnionej w systemie hosta.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-174">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="fc8a3-175">Środowisko uruchomieniowe i zależności aplikacji są wdrażane przy użyciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-175">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="fc8a3-176">[Identyfikator środowiska uruchomieniowego systemu Windows (RID)](/dotnet/core/rid-catalog) znajduje się w `<PropertyGroup>`, który zawiera platformę docelową:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-176">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="fc8a3-177">Aby opublikować dla wielu identyfikatorów RID:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-177">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="fc8a3-178">Podaj identyfikatory RID na liście rozdzielanej średnikami.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-178">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="fc8a3-179">Użyj nazwy właściwości [\<RuntimeIdentifiers >](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-179">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="fc8a3-180">Aby uzyskać więcej informacji, zobacz [katalog .NET Core RID Catalog](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-180">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fc8a3-181">Właściwość `<SelfContained>` jest ustawiona na `true`:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-181">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="fc8a3-182">Konto użytkownika usługi</span><span class="sxs-lookup"><span data-stu-id="fc8a3-182">Service user account</span></span>

<span data-ttu-id="fc8a3-183">Aby utworzyć konto użytkownika dla usługi, należy użyć polecenia cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) w powłoce poleceń administracyjnych programu PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-183">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="fc8a3-184">W systemie Windows 10 października 2018 Update (wersja 1809/Kompilacja 10.0.17763) lub nowsza:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-184">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="fc8a3-185">W systemie operacyjnym Windows w wersji starszej niż aktualizacja 2018 systemu Windows 10 października (wersja 1809/Kompilacja 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="fc8a3-185">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="fc8a3-186">Podaj [silne hasło](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) po wyświetleniu monitu.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-186">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="fc8a3-187">Jeśli parametr `-AccountExpires` nie zostanie dostarczony do polecenia cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) z <xref:System.DateTime>em wygaśnięcia, konto nie wygaśnie.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-187">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="fc8a3-188">Aby uzyskać więcej informacji, zobacz [Microsoft. PowerShell. LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) i [konta użytkowników usług](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-188">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="fc8a3-189">Alternatywna metoda zarządzania użytkownikami podczas korzystania z Active Directory polega na użyciu zarządzanych kont usług.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-189">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="fc8a3-190">Aby uzyskać więcej informacji, zobacz [Omówienie kont usług zarządzanych przez grupę](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-190">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="fc8a3-191">Logowanie jako prawa usługi</span><span class="sxs-lookup"><span data-stu-id="fc8a3-191">Log on as a service rights</span></span>

<span data-ttu-id="fc8a3-192">Aby nawiązać *Logowanie jako prawa usługi* dla konta użytkownika usługi:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-192">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="fc8a3-193">Otwórz Edytor lokalnych zasad zabezpieczeń, uruchamiając program *secpol. msc*.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-193">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="fc8a3-194">Rozwiń węzeł **Zasady lokalne** , a następnie wybierz pozycję **Przypisywanie praw użytkownika**.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-194">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="fc8a3-195">Otwórz okno **Logowanie jako usługa** .</span><span class="sxs-lookup"><span data-stu-id="fc8a3-195">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="fc8a3-196">Wybierz pozycję **Dodaj użytkownika lub grupę**.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-196">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="fc8a3-197">Podaj nazwę obiektu (konto użytkownika) przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-197">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="fc8a3-198">Wpisz konto użytkownika (`{DOMAIN OR COMPUTER NAME\USER}`) w polu Nazwa obiektu, a następnie wybierz **przycisk OK** , aby dodać użytkownika do zasad.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-198">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="fc8a3-199">Wybierz pozycję **Zaawansowane**.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-199">Select **Advanced**.</span></span> <span data-ttu-id="fc8a3-200">Wybierz pozycję **Znajdź teraz**.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-200">Select **Find Now**.</span></span> <span data-ttu-id="fc8a3-201">Wybierz z listy konto użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-201">Select the user account from the list.</span></span> <span data-ttu-id="fc8a3-202">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-202">Select **OK**.</span></span> <span data-ttu-id="fc8a3-203">Ponownie wybierz **przycisk OK** , aby dodać użytkownika do zasad.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-203">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="fc8a3-204">Wybierz **przycisk OK** lub **Zastosuj** , aby zaakceptować zmiany.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-204">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="fc8a3-205">Tworzenie usługi systemu Windows i zarządzanie nią</span><span class="sxs-lookup"><span data-stu-id="fc8a3-205">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="fc8a3-206">Tworzenie usługi</span><span class="sxs-lookup"><span data-stu-id="fc8a3-206">Create a service</span></span>

<span data-ttu-id="fc8a3-207">Zarejestrowanie usługi za pomocą poleceń programu PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-207">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="fc8a3-208">Z poziomu powłoki poleceń administracyjnych programu PowerShell 6 wykonaj następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-208">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="fc8a3-209">`{EXE PATH}` &ndash; ścieżkę do folderu aplikacji na hoście (na przykład `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-209">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="fc8a3-210">Nie dołączaj pliku wykonywalnego aplikacji do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-210">Don't include the app's executable in the path.</span></span> <span data-ttu-id="fc8a3-211">Końcowy ukośnik nie jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-211">A trailing slash isn't required.</span></span>
* <span data-ttu-id="fc8a3-212">konto użytkownika usługi &ndash; `{DOMAIN OR COMPUTER NAME\USER}` (na przykład `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-212">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="fc8a3-213">`{SERVICE NAME}` &ndash; nazwę usługi (na przykład `MyService`).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-213">`{SERVICE NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="fc8a3-214">`{EXE FILE PATH}` &ndash; ścieżki pliku wykonywalnego aplikacji (na przykład `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-214">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="fc8a3-215">Uwzględnij nazwę pliku wykonywalnego z rozszerzeniem.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-215">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="fc8a3-216">`{DESCRIPTION}` &ndash; Opis usługi (na przykład `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-216">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="fc8a3-217">Nazwa wyświetlana usługi `{DISPLAY NAME}` &ndash; (na przykład `My Service`).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-217">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="fc8a3-218">Uruchom usługę</span><span class="sxs-lookup"><span data-stu-id="fc8a3-218">Start a service</span></span>

<span data-ttu-id="fc8a3-219">Uruchom usługę za pomocą następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-219">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="fc8a3-220">Uruchomienie usługi może potrwać kilka sekund.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-220">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="fc8a3-221">Określanie stanu usługi</span><span class="sxs-lookup"><span data-stu-id="fc8a3-221">Determine a service's status</span></span>

<span data-ttu-id="fc8a3-222">Aby sprawdzić stan usługi, użyj następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-222">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="fc8a3-223">Stan jest raportowany jako jedna z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-223">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="fc8a3-224">Zatrzymaj usługę</span><span class="sxs-lookup"><span data-stu-id="fc8a3-224">Stop a service</span></span>

<span data-ttu-id="fc8a3-225">Zatrzymaj usługę za pomocą następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-225">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="fc8a3-226">Usuwanie usługi</span><span class="sxs-lookup"><span data-stu-id="fc8a3-226">Remove a service</span></span>

<span data-ttu-id="fc8a3-227">Po krótkim opóźnieniu na zatrzymanie usługi Usuń usługę za pomocą następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-227">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="fc8a3-228">Obsługa zdarzeń uruchamiania i zatrzymywania</span><span class="sxs-lookup"><span data-stu-id="fc8a3-228">Handle starting and stopping events</span></span>

<span data-ttu-id="fc8a3-229">Aby obsłużyć zdarzenia <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>i <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-229">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="fc8a3-230">Utwórz klasę pochodzącą z <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> z metodami `OnStarting`, `OnStarted`i `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-230">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="fc8a3-231">Utwórz metodę rozszerzenia dla <xref:Microsoft.AspNetCore.Hosting.IWebHost>, która przekazuje `CustomWebHostService` do <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-231">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="fc8a3-232">W `Program.Main`Wywołaj metodę rozszerzenia `RunAsCustomService` zamiast <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-232">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="fc8a3-233">Aby zobaczyć lokalizację <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> w `Program.Main`, zapoznaj się z przykładem kodu pokazanym w sekcji [typ wdrożenia](#deployment-type) .</span><span class="sxs-lookup"><span data-stu-id="fc8a3-233">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="fc8a3-234">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="fc8a3-234">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="fc8a3-235">Usługi, które współdziałają z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub modułem równoważenia obciążenia, mogą wymagać dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-235">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="fc8a3-236">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-236">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="fc8a3-237">Konfigurowanie punktów końcowych</span><span class="sxs-lookup"><span data-stu-id="fc8a3-237">Configure endpoints</span></span>

<span data-ttu-id="fc8a3-238">Domyślnie ASP.NET Core wiąże się z `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-238">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="fc8a3-239">Skonfiguruj adres URL i port, ustawiając zmienną środowiskową `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-239">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="fc8a3-240">Aby uzyskać dodatkowe podejścia do konfiguracji adresów URL i portów, zobacz artykuł dotyczący odpowiedniego serwera:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-240">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="fc8a3-241">Powyższe wskazówki obejmują obsługę punktów końcowych HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-241">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="fc8a3-242">Na przykład skonfiguruj aplikację do obsługi protokołu HTTPS, gdy uwierzytelnianie jest używane z usługą systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-242">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="fc8a3-243">Korzystanie z ASP.NET Core certyfikatu deweloperskiego HTTPS w celu zabezpieczenia punktu końcowego usługi nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-243">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="fc8a3-244">Bieżący katalog i główna Zawartość</span><span class="sxs-lookup"><span data-stu-id="fc8a3-244">Current directory and content root</span></span>

<span data-ttu-id="fc8a3-245">Bieżącym katalogiem roboczym zwróconym przez wywoływanie <xref:System.IO.Directory.GetCurrentDirectory*> dla usługi systemu Windows jest folder *C:\\Windows\\system32* .</span><span class="sxs-lookup"><span data-stu-id="fc8a3-245">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="fc8a3-246">Folder *system32* nie jest odpowiednią lokalizacją do przechowywania plików usługi (na przykład plików ustawień).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-246">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="fc8a3-247">Użyj jednego z poniższych metod, aby zachować i uzyskać dostęp do plików ustawień i zasobów usługi.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-247">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="fc8a3-248">Użyj ContentRootPath lub ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="fc8a3-248">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="fc8a3-249">Użyj [IHostEnvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) lub <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider>, aby zlokalizować zasoby aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-249">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

<span data-ttu-id="fc8a3-250">Gdy aplikacja działa jako usługa, <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> ustawia <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> na [AppContext. BaseDirectory](xref:System.AppContext.BaseDirectory).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-250">When the app runs as a service, <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> sets the <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span>

<span data-ttu-id="fc8a3-251">Domyślne pliki ustawień aplikacji, *appSettings. JSON* i *appSettings. { Environment}. JSON*jest ładowany z katalogu głównego zawartości aplikacji przez wywołanie [CreateDefaultBuilder podczas konstruowania hosta](xref:fundamentals/host/generic-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-251">The app's default settings files, *appsettings.json* and *appsettings.{Environment}.json*, are loaded from the app's content root by calling [CreateDefaultBuilder during host construction](xref:fundamentals/host/generic-host#set-up-a-host).</span></span>

<span data-ttu-id="fc8a3-252">W przypadku innych plików ustawień ładowanych przez kod dewelopera w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>nie ma potrzeby wywoływania <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-252">For other settings files loaded by developer code in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, there's no need to call <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="fc8a3-253">W poniższym przykładzie plik *custom_settings. JSON* znajduje się w katalogu głównym zawartości aplikacji i jest ładowany bez jawnego ustawiania ścieżki podstawowej:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-253">In the following example, the *custom_settings.json* file exists in the app's content root and is loaded without explicitly setting a base path:</span></span>

[!code-csharp[](windows-service/samples_snapshot/CustomSettingsExample.cs?highlight=13)]

<span data-ttu-id="fc8a3-254">Nie próbuj używać <xref:System.IO.Directory.GetCurrentDirectory*> do uzyskania ścieżki zasobów, ponieważ aplikacja usługi systemu Windows zwraca folder *C:\\Windows\\system32* jako bieżący katalog.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-254">Don't attempt to use <xref:System.IO.Directory.GetCurrentDirectory*> to obtain a resource path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder as its current directory.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="fc8a3-255">Ustawianie ścieżki katalogu głównego zawartości do folderu aplikacji</span><span class="sxs-lookup"><span data-stu-id="fc8a3-255">Set the content root path to the app's folder</span></span>

<span data-ttu-id="fc8a3-256"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> jest tą samą ścieżką dostarczoną do argumentu `binPath` podczas tworzenia usługi.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-256">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="fc8a3-257">Zamiast wywoływania `GetCurrentDirectory`, aby tworzyć ścieżki do plików ustawień, wywołaj <xref:System.IO.Directory.SetCurrentDirectory*> ze ścieżką do [katalogu głównego zawartości](xref:fundamentals/index#content-root)aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-257">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's [content root](xref:fundamentals/index#content-root).</span></span>

<span data-ttu-id="fc8a3-258">W `Program.Main`określ ścieżkę do folderu wykonywalnego usługi i użyj ścieżki, aby określić katalog główny zawartości aplikacji:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-258">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="fc8a3-259">Przechowywanie plików usługi w odpowiedniej lokalizacji na dysku</span><span class="sxs-lookup"><span data-stu-id="fc8a3-259">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="fc8a3-260">Określ ścieżkę bezwzględną <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> w przypadku używania <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do folderu zawierającego pliki.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-260">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="fc8a3-261">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="fc8a3-261">Troubleshoot</span></span>

<span data-ttu-id="fc8a3-262">Aby rozwiązać problem z aplikacją usługi systemu Windows, zobacz <xref:test/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-262">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="fc8a3-263">Typowe błędy</span><span class="sxs-lookup"><span data-stu-id="fc8a3-263">Common errors</span></span>

* <span data-ttu-id="fc8a3-264">Starsza lub wstępnie wydana wersja programu PowerShell jest używana.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-264">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="fc8a3-265">Zarejestrowana usługa nie używa **opublikowanych** danych wyjściowych aplikacji z [dotnet Publish](/dotnet/core/tools/dotnet-publish) polecenia.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-265">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="fc8a3-266">Dane wyjściowe polecenia [kompilacji dotnet](/dotnet/core/tools/dotnet-build) nie są obsługiwane w przypadku wdrażania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-266">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="fc8a3-267">Opublikowane zasoby znajdują się w dowolnym z następujących folderów w zależności od typu wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-267">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="fc8a3-268">*bin/Release/{Target Framework}/Publish* (FDD)</span><span class="sxs-lookup"><span data-stu-id="fc8a3-268">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="fc8a3-269">*bin/Release/{Target Framework}/{Runtime identyfikator}/Publish* (SCD)</span><span class="sxs-lookup"><span data-stu-id="fc8a3-269">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="fc8a3-270">Usługa nie jest w stanie uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-270">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="fc8a3-271">Ścieżki do zasobów używanych przez aplikację (na przykład certyfikaty) są nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-271">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="fc8a3-272">Ścieżką podstawową usługi systemu Windows jest *c:\\Windows\\system32*.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-272">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="fc8a3-273">Użytkownik nie ma uprawnień do *logowania się jako usługa* .</span><span class="sxs-lookup"><span data-stu-id="fc8a3-273">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="fc8a3-274">Hasło użytkownika wygasło lub zostało nieprawidłowo przesłane podczas wykonywania polecenia `New-Service` PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-274">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="fc8a3-275">Aplikacja wymaga uwierzytelniania ASP.NET Core, ale nie jest skonfigurowana dla połączeń Secure (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-275">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="fc8a3-276">Port adresu URL żądania jest niepoprawny lub niepoprawnie skonfigurowany w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-276">The request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="fc8a3-277">Dzienniki zdarzeń systemu i aplikacji</span><span class="sxs-lookup"><span data-stu-id="fc8a3-277">System and Application Event Logs</span></span>

<span data-ttu-id="fc8a3-278">Uzyskaj dostęp do dzienników zdarzeń systemu i aplikacji:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-278">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="fc8a3-279">Otwórz menu Start, wyszukaj ciąg *Podgląd zdarzeń*i wybierz aplikację **Podgląd zdarzeń** .</span><span class="sxs-lookup"><span data-stu-id="fc8a3-279">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="fc8a3-280">W **Podgląd zdarzeń**Otwórz węzeł **Dzienniki systemu Windows** .</span><span class="sxs-lookup"><span data-stu-id="fc8a3-280">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="fc8a3-281">Wybierz pozycję **system** , aby otworzyć dziennik zdarzeń systemu.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-281">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="fc8a3-282">Wybierz pozycję **aplikacja** , aby otworzyć dziennik zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-282">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="fc8a3-283">Wyszukaj błędy skojarzone z aplikacją się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-283">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="fc8a3-284">Uruchamianie aplikacji w wierszu polecenia</span><span class="sxs-lookup"><span data-stu-id="fc8a3-284">Run the app at a command prompt</span></span>

<span data-ttu-id="fc8a3-285">Wiele błędów uruchamiania nie produkuje użytecznych informacji w dziennikach zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-285">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="fc8a3-286">Przyczyną niektórych błędów można znaleźć, uruchamiając aplikację w wierszu polecenia w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-286">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="fc8a3-287">Aby rejestrować dodatkowe szczegóły aplikacji, Obniż [poziom rejestrowania](xref:fundamentals/logging/index#log-level) lub Uruchom aplikację w [środowisku deweloperskim](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-287">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="fc8a3-288">Wyczyść pamięć podręczną pakietów</span><span class="sxs-lookup"><span data-stu-id="fc8a3-288">Clear package caches</span></span>

<span data-ttu-id="fc8a3-289">Działająca aplikacja może zakończyć się niepowodzeniem natychmiast po uaktualnieniu zestaw .NET Core SDK na komputerze deweloperskim lub zmianie wersji pakietu w ramach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-289">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="fc8a3-290">W niektórych przypadkach niespójne pakietów może spowodować uszkodzenie aplikacji podczas przeprowadzania uaktualnienia głównych.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-290">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="fc8a3-291">Większość z tych problemów można naprawić, wykonując następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-291">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="fc8a3-292">Usuń foldery *bin* i *obj* .</span><span class="sxs-lookup"><span data-stu-id="fc8a3-292">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="fc8a3-293">Wyczyść pamięć podręczną pakietów, wykonując [wszystkie elementy lokalne usługi NuGet programu dotnet--Wyczyść](/dotnet/core/tools/dotnet-nuget-locals) z poziomu powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-293">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="fc8a3-294">Czyszczenie pamięci podręcznych pakietów można również wykonać za pomocą narzędzia [NuGet. exe](https://www.nuget.org/downloads) i wykonując polecenie `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-294">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="fc8a3-295">*NuGet. exe* nie jest pakietem instalowanym z systemem operacyjnym Windows dla komputerów stacjonarnych i musi być uzyskany niezależnie od [witryny sieci Web programu NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-295">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="fc8a3-296">Przywróć i skompiluj ponownie projekt.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-296">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="fc8a3-297">Usuń wszystkie pliki z folderu wdrożenia na serwerze przed ponownym wdrożeniem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-297">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="fc8a3-298">Wolne lub zwisa aplikacji</span><span class="sxs-lookup"><span data-stu-id="fc8a3-298">Slow or hanging app</span></span>

<span data-ttu-id="fc8a3-299">*Zrzut awaryjny* to migawka pamięci systemu, która może pomóc w ustaleniu przyczyny awarii aplikacji, awarii uruchamiania lub powolnej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-299">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="fc8a3-300">Awaria aplikacji lub napotka wyjątek</span><span class="sxs-lookup"><span data-stu-id="fc8a3-300">App crashes or encounters an exception</span></span>

<span data-ttu-id="fc8a3-301">Uzyskaj i Analizuj Zrzut z [raportowanie błędów systemu Windows (raportowanie błędów systemu Windows)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="fc8a3-301">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="fc8a3-302">Utwórz folder do przechowywania plików zrzutu awaryjnego w `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-302">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="fc8a3-303">Uruchom [skrypt programu PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) w programie EnableDumps z nazwą pliku wykonywalnego aplikacji:</span><span class="sxs-lookup"><span data-stu-id="fc8a3-303">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```console
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="fc8a3-304">Uruchom aplikację w warunkach, które powodują awarię.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-304">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="fc8a3-305">Po wystąpieniu awarii Uruchom [skrypt programu DisableDumps PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="fc8a3-305">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span></span>

   ```console
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="fc8a3-306">Po awarii aplikacji i zakończeniu zbierania zrzutów aplikacja może zakończyć normalne działanie.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-306">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="fc8a3-307">Skrypt programu PowerShell konfiguruje raportowanie błędów systemu Windows w celu zebrania do pięciu zrzutów na aplikację.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-307">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="fc8a3-308">Zrzuty awaryjne mogą wymagać dużej ilości miejsca na dysku (do kilku gigabajtów).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-308">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="fc8a3-309">Aplikacja zawiesza się, kończy się niepowodzeniem podczas uruchamiania lub działa normalnie</span><span class="sxs-lookup"><span data-stu-id="fc8a3-309">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="fc8a3-310">Gdy aplikacja *zawiesza* się (bez awarii), kończy się niepowodzeniem podczas uruchamiania lub działa normalnie, zobacz [pliki zrzutu w trybie użytkownika: wybór najlepszego narzędzia](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) do wygenerowania zrzutu.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-310">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="fc8a3-311">Analizowanie zrzutu</span><span class="sxs-lookup"><span data-stu-id="fc8a3-311">Analyze the dump</span></span>

<span data-ttu-id="fc8a3-312">Zrzut można analizować przy użyciu kilku metod.</span><span class="sxs-lookup"><span data-stu-id="fc8a3-312">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="fc8a3-313">Aby uzyskać więcej informacji, zobacz [Analizowanie pliku zrzutu w trybie użytkownika](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="fc8a3-313">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fc8a3-314">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="fc8a3-314">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="fc8a3-315">[Konfiguracja punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym Konfiguracja protokołu HTTPS i obsługa SNI)</span><span class="sxs-lookup"><span data-stu-id="fc8a3-315">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="fc8a3-316">[Konfiguracja punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym Konfiguracja protokołu HTTPS i obsługa SNI)</span><span class="sxs-lookup"><span data-stu-id="fc8a3-316">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
