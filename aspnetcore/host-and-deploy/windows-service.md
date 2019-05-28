---
title: Host platformy ASP.NET Core w usłudze Windows
author: guardrex
description: Dowiedz się, jak udostępnić aplikację ASP.NET Core w usłudze Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/21/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: ab36bc1b2827c80bb1e7b9e8cee558b346a991f8
ms.sourcegitcommit: b8ed594ab9f47fa32510574f3e1b210cff000967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66251422"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="40591-103">Host platformy ASP.NET Core w usłudze Windows</span><span class="sxs-lookup"><span data-stu-id="40591-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="40591-104">Przez [Luke Latham](https://github.com/guardrex) i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="40591-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="40591-105">Aplikacji ASP.NET Core może być hostowana na Windows jako [usługi Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) bez korzystania z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="40591-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="40591-106">Gdy hostowany jako usługa Windows, aplikacji uruchamiany automatycznie po ponownym uruchomieniu serwera.</span><span class="sxs-lookup"><span data-stu-id="40591-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="40591-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="40591-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40591-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="40591-108">Prerequisites</span></span>

* [<span data-ttu-id="40591-109">Platforma ASP.NET Core SDK 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="40591-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="40591-110">PowerShell 6.2 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="40591-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="app-configuration"></a><span data-ttu-id="40591-111">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="40591-111">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="40591-112">`IHostBuilder.UseWindowsService`, dostarczone przez [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) pakietu, jest wywoływana podczas tworzenia hosta.</span><span class="sxs-lookup"><span data-stu-id="40591-112">`IHostBuilder.UseWindowsService`, provided by the [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) package, is called when building the host.</span></span> <span data-ttu-id="40591-113">Jeśli aplikacja jest uruchomiona jako usługa Windows metody:</span><span class="sxs-lookup"><span data-stu-id="40591-113">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="40591-114">Ustawia okres istnienia hosta `WindowsServiceLifetime`.</span><span class="sxs-lookup"><span data-stu-id="40591-114">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="40591-115">Ustawia zawartość katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="40591-115">Sets the content root.</span></span>
* <span data-ttu-id="40591-116">Włącza funkcję rejestrowania w dzienniku zdarzeń, z nazwą aplikacji jako domyślną nazwę źródła.</span><span class="sxs-lookup"><span data-stu-id="40591-116">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="40591-117">Poziom dziennika można skonfigurować za pomocą `Logging:LogLevel:Default` w *appsettings. Production.JSON* pliku.</span><span class="sxs-lookup"><span data-stu-id="40591-117">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="40591-118">Tylko administratorzy mogą tworzyć nowe źródła zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="40591-118">Only administrators can create new event sources.</span></span> <span data-ttu-id="40591-119">Gdy nie można utworzyć źródła zdarzeń, przy użyciu nazwy aplikacji, ostrzeżenie jest rejestrowany wpis *aplikacji* źródła i dzienniki zdarzeń są wyłączone.</span><span class="sxs-lookup"><span data-stu-id="40591-119">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="40591-120">Aplikacja wymaga odwołania do pakietu dla [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) i [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="40591-120">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="40591-121">Do testowania i debugowania, gdy działają poza usługą, Dodaj kod, aby ustalić, czy aplikacja jest uruchomiona jako usługę lub aplikację konsoli.</span><span class="sxs-lookup"><span data-stu-id="40591-121">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="40591-122">Sprawdź, czy jest dołączony debuger a `--console` przełącznik jest obecny.</span><span class="sxs-lookup"><span data-stu-id="40591-122">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="40591-123">Jeśli tych warunków jest spełniony, (aplikacja nie jest uruchamiana jako usługa), należy wywołać <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="40591-123">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="40591-124">Jeśli warunki są fałszywe, (aplikacja jest uruchamiana jako usługa):</span><span class="sxs-lookup"><span data-stu-id="40591-124">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="40591-125">Wywołaj <xref:System.IO.Directory.SetCurrentDirectory*> i użyj ścieżki do lokalizacji publikowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40591-125">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="40591-126">Nie wywołuj <xref:System.IO.Directory.GetCurrentDirectory*> można uzyskać ścieżki, ponieważ aplikacji usługi Windows, która zwraca *C:\\WINDOWS\\system32* folderu podczas <xref:System.IO.Directory.GetCurrentDirectory*> nosi nazwę.</span><span class="sxs-lookup"><span data-stu-id="40591-126">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="40591-127">Aby uzyskać więcej informacji, zobacz [bieżący katalog i katalog główny zawartości](#current-directory-and-content-root) sekcji.</span><span class="sxs-lookup"><span data-stu-id="40591-127">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="40591-128">Wykonanie tego kroku, zanim aplikacja jest skonfigurowana w `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="40591-128">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="40591-129">Wywołaj <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> do uruchomienia aplikacji jako usługi.</span><span class="sxs-lookup"><span data-stu-id="40591-129">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="40591-130">Ponieważ [dostawcę konfiguracji wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) wymaga pary nazwa wartość dla argumentów wiersza poleceń `--console` przełącznik został usunięty z argumentów przed <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> odbiera argumenty.</span><span class="sxs-lookup"><span data-stu-id="40591-130">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="40591-131">Można zapisać w dzienniku zdarzeń Windows, należy dodać dostawcę dziennika zdarzeń <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="40591-131">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="40591-132">Poziom rejestrowania za pomocą `Logging:LogLevel:Default` w *appsettings. Production.JSON* pliku.</span><span class="sxs-lookup"><span data-stu-id="40591-132">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="40591-133">W poniższym przykładzie z przykładowej aplikacji `RunAsCustomService` nosi nazwę zamiast <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> w celu obsługi zdarzenia okresu istnienia w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40591-133">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="40591-134">Aby uzyskać więcej informacji, zobacz [obsługi, uruchamianie i zatrzymywanie wydarzeń](#handle-starting-and-stopping-events) sekcji.</span><span class="sxs-lookup"><span data-stu-id="40591-134">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="40591-135">Typ wdrożenia</span><span class="sxs-lookup"><span data-stu-id="40591-135">Deployment type</span></span>

<span data-ttu-id="40591-136">Aby uzyskać informacje i wskazówki dotyczące scenariuszy wdrażania, zobacz [wdrażanie aplikacji .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="40591-136">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="40591-137">Zależny od struktury wdrożenia (stacje)</span><span class="sxs-lookup"><span data-stu-id="40591-137">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="40591-138">Zależny od struktury wdrożenia (stacje) zależy od obecności udostępnione całego systemu w wersji programu .NET Core w systemie docelowym.</span><span class="sxs-lookup"><span data-stu-id="40591-138">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="40591-139">W przypadku scenariusza z stacje zostanie przyjęte, postępując zgodnie ze wskazówkami w tym artykule, zestaw SDK tworzy plik wykonywalny ( *.exe*), co jest nazywane *zależny od struktury pliku wykonywalnego*.</span><span class="sxs-lookup"><span data-stu-id="40591-139">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="40591-140">Dodaj następujące elementy właściwości do pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="40591-140">Add the following property elements to the project file:</span></span>

* <span data-ttu-id="40591-141">`<OutputType>` &ndash; Aplikacja na typ danych wyjściowych (`Exe` dla pliku wykonywalnego).</span><span class="sxs-lookup"><span data-stu-id="40591-141">`<OutputType>` &ndash; The app's output type (`Exe` for executable).</span></span>
* <span data-ttu-id="40591-142">`<LangVersion>` &ndash; C# Wersja językowa (`latest` lub `preview`).</span><span class="sxs-lookup"><span data-stu-id="40591-142">`<LangVersion>` &ndash; The C# language version (`latest` or `preview`).</span></span>

<span data-ttu-id="40591-143">A *web.config* pliku, który zazwyczaj jest generowany podczas publikowania aplikacji ASP.NET Core, nie jest konieczne w przypadku aplikacji usługi Windows.</span><span class="sxs-lookup"><span data-stu-id="40591-143">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="40591-144">Aby wyłączyć tworzenie *web.config* Dodaj `<IsTransformWebConfigDisabled>` właściwością `true`.</span><span class="sxs-lookup"><span data-stu-id="40591-144">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <OutputType>Exe</OutputType>
  <LangVersion>preview</LangVersion>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="40591-145">Windows [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) zawiera platformę docelową.</span><span class="sxs-lookup"><span data-stu-id="40591-145">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="40591-146">W poniższym przykładzie ustawiono RID `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="40591-146">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="40591-147">`<SelfContained>` Właściwość jest ustawiona na `false`.</span><span class="sxs-lookup"><span data-stu-id="40591-147">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="40591-148">Te właściwości poinstruować zestawu SDK w celu wygenerowania pliku wykonywalnego ( *.exe*) pliku Windows i aplikacji, która jest zależna od udostępnionej platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="40591-148">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="40591-149">A *web.config* pliku, który zazwyczaj jest generowany podczas publikowania aplikacji ASP.NET Core, nie jest konieczne w przypadku aplikacji usługi Windows.</span><span class="sxs-lookup"><span data-stu-id="40591-149">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="40591-150">Aby wyłączyć tworzenie *web.config* Dodaj `<IsTransformWebConfigDisabled>` właściwością `true`.</span><span class="sxs-lookup"><span data-stu-id="40591-150">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="40591-151">Windows [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) zawiera platformę docelową.</span><span class="sxs-lookup"><span data-stu-id="40591-151">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="40591-152">W poniższym przykładzie ustawiono RID `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="40591-152">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="40591-153">`<SelfContained>` Właściwość jest ustawiona na `false`.</span><span class="sxs-lookup"><span data-stu-id="40591-153">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="40591-154">Te właściwości poinstruować zestawu SDK w celu wygenerowania pliku wykonywalnego ( *.exe*) pliku Windows i aplikacji, która jest zależna od udostępnionej platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="40591-154">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="40591-155">`<UseAppHost>` Właściwość jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="40591-155">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="40591-156">Ta właściwość zapewnia usługi ze ścieżką aktywacji (plik wykonywalny *.exe*) dla Dyskietki.</span><span class="sxs-lookup"><span data-stu-id="40591-156">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="40591-157">A *web.config* pliku, który zazwyczaj jest generowany podczas publikowania aplikacji ASP.NET Core, nie jest konieczne w przypadku aplikacji usługi Windows.</span><span class="sxs-lookup"><span data-stu-id="40591-157">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="40591-158">Aby wyłączyć tworzenie *web.config* Dodaj `<IsTransformWebConfigDisabled>` właściwością `true`.</span><span class="sxs-lookup"><span data-stu-id="40591-158">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="40591-159">Niezależne wdrożenia (— SCD)</span><span class="sxs-lookup"><span data-stu-id="40591-159">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="40591-160">Niezależne wdrożenia (— SCD) nie jest zależny od obecności udostępnionej platformy w systemie hosta.</span><span class="sxs-lookup"><span data-stu-id="40591-160">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="40591-161">Środowisko uruchomieniowe i zależności aplikacji są wdrażane przy użyciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40591-161">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="40591-162">Windows [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) znajduje się w `<PropertyGroup>` zawierający platforma docelowa:</span><span class="sxs-lookup"><span data-stu-id="40591-162">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="40591-163">Aby opublikować dla wielu identyfikatorów RID:</span><span class="sxs-lookup"><span data-stu-id="40591-163">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="40591-164">Podaj identyfikatorów RID w liście rozdzielanej średnikami.</span><span class="sxs-lookup"><span data-stu-id="40591-164">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="40591-165">Użyj nazwy właściwości [ \<RuntimeIdentifiers >](/dotnet/core/tools/csproj#runtimeidentifiers) (w liczbie mnogiej).</span><span class="sxs-lookup"><span data-stu-id="40591-165">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="40591-166">Aby uzyskać więcej informacji, zobacz [.NET Core RID katalogu](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="40591-166">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="40591-167">A `<SelfContained>` właściwość jest ustawiona na `true`:</span><span class="sxs-lookup"><span data-stu-id="40591-167">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="40591-168">Konto użytkownika usługi</span><span class="sxs-lookup"><span data-stu-id="40591-168">Service user account</span></span>

<span data-ttu-id="40591-169">Aby utworzyć konto użytkownika dla usługi, użyj [New LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) polecenia cmdlet z administracyjne powłoki poleceń programu PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="40591-169">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="40591-170">W systemie Windows 10 października 2018 aktualizacja (wersja 1809/build 10.0.17763) lub nowszej:</span><span class="sxs-lookup"><span data-stu-id="40591-170">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="40591-171">W systemie operacyjnym Windows w starszych niż Windows 10 października 2018 aktualizacja (wersja 1809/build 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="40591-171">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

<span data-ttu-id="40591-172">Podaj [silne hasło](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) po wyświetleniu monitu.</span><span class="sxs-lookup"><span data-stu-id="40591-172">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="40591-173">Chyba że `-AccountExpires` parametru do [New LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) polecenia cmdlet z wygaśnięciem <xref:System.DateTime>, konto nie wygasa.</span><span class="sxs-lookup"><span data-stu-id="40591-173">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="40591-174">Aby uzyskać więcej informacji, zobacz [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) i [kont użytkowników usług](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="40591-174">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="40591-175">Innym sposobem zarządzania użytkownikami, podczas korzystania z usługi Active Directory jest użycie kont usług zarządzanych.</span><span class="sxs-lookup"><span data-stu-id="40591-175">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="40591-176">Aby uzyskać więcej informacji, zobacz [omówienie kont usług zarządzanych przez grupę](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="40591-176">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="40591-177">Zaloguj się jako z uprawnieniami usługi</span><span class="sxs-lookup"><span data-stu-id="40591-177">Log on as a service rights</span></span>

<span data-ttu-id="40591-178">Aby ustanowić *Zaloguj się jako usługa* uprawnienia dla konta użytkownika usługi:</span><span class="sxs-lookup"><span data-stu-id="40591-178">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="40591-179">Otwórz Edytor zasady zabezpieczeń lokalnych, uruchamiając *secpool.msc*.</span><span class="sxs-lookup"><span data-stu-id="40591-179">Open the Local Security Policy editor by running *secpool.msc*.</span></span>
1. <span data-ttu-id="40591-180">Rozwiń **zasady lokalne** a następnie wybierz węzeł **Przypisywanie praw użytkownika**.</span><span class="sxs-lookup"><span data-stu-id="40591-180">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="40591-181">Otwórz **Zaloguj się jako usługa** zasad.</span><span class="sxs-lookup"><span data-stu-id="40591-181">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="40591-182">Wybierz **Dodaj użytkownika lub grupę**.</span><span class="sxs-lookup"><span data-stu-id="40591-182">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="40591-183">Podaj nazwę obiektu (konto użytkownika), przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="40591-183">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="40591-184">Wpisz konto użytkownika (`{DOMAIN OR COMPUTER NAME\USER}`) w obiekcie nazwy pola i wybierz pozycję **OK** dodać użytkownika do zasad.</span><span class="sxs-lookup"><span data-stu-id="40591-184">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="40591-185">Wybierz **zaawansowane**.</span><span class="sxs-lookup"><span data-stu-id="40591-185">Select **Advanced**.</span></span> <span data-ttu-id="40591-186">Wybierz **Znajdź teraz**.</span><span class="sxs-lookup"><span data-stu-id="40591-186">Select **Find Now**.</span></span> <span data-ttu-id="40591-187">Wybierz konto użytkownika z listy.</span><span class="sxs-lookup"><span data-stu-id="40591-187">Select the user account from the list.</span></span> <span data-ttu-id="40591-188">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="40591-188">Select **OK**.</span></span> <span data-ttu-id="40591-189">Wybierz **OK** ponownie, aby dodać użytkownika do zasad.</span><span class="sxs-lookup"><span data-stu-id="40591-189">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="40591-190">Wybierz **OK** lub **Zastosuj** aby zatwierdzić zmiany.</span><span class="sxs-lookup"><span data-stu-id="40591-190">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="40591-191">Tworzenie i zarządzanie usługą Windows</span><span class="sxs-lookup"><span data-stu-id="40591-191">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="40591-192">Tworzenie usługi</span><span class="sxs-lookup"><span data-stu-id="40591-192">Create a service</span></span>

<span data-ttu-id="40591-193">Użyj poleceń programu PowerShell w celu zarejestrowania usługi.</span><span class="sxs-lookup"><span data-stu-id="40591-193">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="40591-194">Z administracyjne powłoki poleceń programu PowerShell 6 wykonaj następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="40591-194">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="40591-195">`{EXE PATH}` &ndash; Ścieżka do folderu aplikacji na hoście (na przykład `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="40591-195">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="40591-196">Nie dołączaj ścieżki pliku wykonywalnego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40591-196">Don't include the app's executable in the path.</span></span> <span data-ttu-id="40591-197">Końcowy ukośnik nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="40591-197">A trailing slash isn't required.</span></span>
* <span data-ttu-id="40591-198">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Konto użytkownika usługi (na przykład `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="40591-198">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="40591-199">`{NAME}` &ndash; Nazwa usługi (na przykład `MyService`).</span><span class="sxs-lookup"><span data-stu-id="40591-199">`{NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="40591-200">`{EXE FILE PATH}` &ndash; Ścieżka do pliku wykonywalnego aplikacji (na przykład `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="40591-200">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="40591-201">Zawierać nazwę pliku wykonywalnego z rozszerzeniem.</span><span class="sxs-lookup"><span data-stu-id="40591-201">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="40591-202">`{DESCRIPTION}` &ndash; Opis usługi (na przykład `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="40591-202">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="40591-203">`{DISPLAY NAME}` &ndash; Wyświetlana nazwa usługi (na przykład `My Service`).</span><span class="sxs-lookup"><span data-stu-id="40591-203">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="40591-204">Uruchom usługę</span><span class="sxs-lookup"><span data-stu-id="40591-204">Start a service</span></span>

<span data-ttu-id="40591-205">Uruchom usługę za pomocą następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="40591-205">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {NAME}
```

<span data-ttu-id="40591-206">Polecenie zajmuje kilka sekund, aby uruchomić usługę.</span><span class="sxs-lookup"><span data-stu-id="40591-206">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="40591-207">Sprawdź stan usługi</span><span class="sxs-lookup"><span data-stu-id="40591-207">Determine a service's status</span></span>

<span data-ttu-id="40591-208">Aby sprawdzić stan usługi, użyj następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="40591-208">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {NAME}
```

<span data-ttu-id="40591-209">Stan jest zgłaszany jako jeden z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="40591-209">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="40591-210">Zatrzymaj usługę</span><span class="sxs-lookup"><span data-stu-id="40591-210">Stop a service</span></span>

<span data-ttu-id="40591-211">Zatrzymaj usługę za pomocą następującego polecenia programu Powershell 6:</span><span class="sxs-lookup"><span data-stu-id="40591-211">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="40591-212">Usuń usługę</span><span class="sxs-lookup"><span data-stu-id="40591-212">Remove a service</span></span>

<span data-ttu-id="40591-213">Po krótkiej chwili zatrzymania usługi należy usunąć usługę za pomocą następującego polecenia programu Powershell 6:</span><span class="sxs-lookup"><span data-stu-id="40591-213">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="40591-214">Obsługa uruchamianie i zatrzymywanie wydarzeń</span><span class="sxs-lookup"><span data-stu-id="40591-214">Handle starting and stopping events</span></span>

<span data-ttu-id="40591-215">Aby obsłużyć <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, i <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> zdarzenia:</span><span class="sxs-lookup"><span data-stu-id="40591-215">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="40591-216">Utwórz klasę, która pochodzi od klasy <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> z `OnStarting`, `OnStarted`, i `OnStopping` metody:</span><span class="sxs-lookup"><span data-stu-id="40591-216">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="40591-217">Tworzenie metody rozszerzenia dla <xref:Microsoft.AspNetCore.Hosting.IWebHost> , który przekazuje `CustomWebHostService` do <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="40591-217">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="40591-218">W `Program.Main`, wywołaj `RunAsCustomService` rozszerzenia zamiast metody <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="40591-218">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="40591-219">Aby wyświetlić lokalizację <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> w `Program.Main`, można znaleźć w przykładzie kodu pokazano w [typu wdrożenia](#deployment-type) sekcji.</span><span class="sxs-lookup"><span data-stu-id="40591-219">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="40591-220">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="40591-220">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="40591-221">Usługi wchodzić w interakcje z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub moduł równoważenia obciążenia, które mogą wymagać dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="40591-221">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="40591-222">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="40591-222">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="40591-223">Konfigurowanie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="40591-223">Configure HTTPS</span></span>

<span data-ttu-id="40591-224">Aby skonfigurować usługę przy użyciu bezpiecznego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="40591-224">To configure a service with a secure endpoint:</span></span>

1. <span data-ttu-id="40591-225">Utwórz certyfikat X.509 dla systemu macierzystego przy użyciu pozyskiwania certyfikatu używanej platformy i mechanizmy wdrażania.</span><span class="sxs-lookup"><span data-stu-id="40591-225">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="40591-226">Określ [konfiguracji punktu końcowego HTTPS serwera Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) przy użyciu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="40591-226">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="40591-227">Użycie certyfikatu deweloperskiego platformy ASP.NET Core HTTPS, aby zabezpieczyć punkt końcowy usługi nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="40591-227">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="40591-228">Bieżący katalog i katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="40591-228">Current directory and content root</span></span>

<span data-ttu-id="40591-229">Bieżący katalog roboczy zwracany przez wywołanie metody <xref:System.IO.Directory.GetCurrentDirectory*> dla usługi Windows jest *C:\\WINDOWS\\system32* folderu.</span><span class="sxs-lookup"><span data-stu-id="40591-229">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="40591-230">*System32* folder nie jest odpowiednią lokalizację do przechowywania plików usługi (na przykład pliki ustawień).</span><span class="sxs-lookup"><span data-stu-id="40591-230">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="40591-231">Użyj jednej z następujących metod do utrzymywania i uzyskać dostęp do zasobów i plików ustawień usługi.</span><span class="sxs-lookup"><span data-stu-id="40591-231">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="40591-232">Użyj ContentRootPath lub ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="40591-232">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="40591-233">Użyj [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) lub <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> lokalizowanie zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40591-233">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="40591-234">Ustaw ścieżkę katalogu głównego zawartości do folderu aplikacji</span><span class="sxs-lookup"><span data-stu-id="40591-234">Set the content root path to the app's folder</span></span>

<span data-ttu-id="40591-235"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> Tej samej ścieżki, które są udostępniane `binPath` argumentów podczas tworzenia usługi.</span><span class="sxs-lookup"><span data-stu-id="40591-235">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="40591-236">Zamiast wywoływać metodę `GetCurrentDirectory` do tworzenia ścieżek do plików ustawień, należy wywołać <xref:System.IO.Directory.SetCurrentDirectory*> ze ścieżką do katalogu głównego zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40591-236">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="40591-237">W `Program.Main`, określić ścieżkę do folderu pliku wykonywalnego usługi i ustanowić zawartości katalogu głównego aplikacji za pomocą ścieżki:</span><span class="sxs-lookup"><span data-stu-id="40591-237">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="40591-238">Store pliki usługi w odpowiedniej lokalizacji na dysku</span><span class="sxs-lookup"><span data-stu-id="40591-238">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="40591-239">Określ ścieżkę bezwzględną z <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> przy użyciu <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do folderu zawierającego pliki.</span><span class="sxs-lookup"><span data-stu-id="40591-239">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="40591-240">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="40591-240">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="40591-241">[Konfiguracja punktu końcowego kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym konfiguracja protokołu HTTPS i obsługa SNI)</span><span class="sxs-lookup"><span data-stu-id="40591-241">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="40591-242">[Konfiguracja punktu końcowego kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym konfiguracja protokołu HTTPS i obsługa SNI)</span><span class="sxs-lookup"><span data-stu-id="40591-242">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
