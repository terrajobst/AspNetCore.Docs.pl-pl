---
title: Host platformy ASP.NET Core w usłudze Windows
author: guardrex
description: Dowiedz się, jak udostępnić aplikację ASP.NET Core w usłudze Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/04/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 544eefa87898e82ec2bf8f9f61ce4e26dd554bb7
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068339"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="ee787-103">Host platformy ASP.NET Core w usłudze Windows</span><span class="sxs-lookup"><span data-stu-id="ee787-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="ee787-104">Przez [Luke Latham](https://github.com/guardrex) i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ee787-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ee787-105">Aplikacji ASP.NET Core może być hostowana na Windows jako [usługi Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) bez korzystania z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="ee787-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="ee787-106">Po hostowany jako usługa Windows, aplikacja zostanie automatycznie uruchomiona po ponownym uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="ee787-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="ee787-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ee787-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ee787-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ee787-108">Prerequisites</span></span>

* [<span data-ttu-id="ee787-109">PowerShell 6.2 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="ee787-109">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

> [!NOTE]
> <span data-ttu-id="ee787-110">Dla starszych niż Windows 10 października 2018 r. systemu operacyjnego Windows Update (wersja 1809/build 10.0.17763) [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) muszą zostać zaimportowane modułu [modułu WindowsCompatibility](https://github.com/PowerShell/WindowsCompatibility)do uzyskania dostępu do [New LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) polecenia cmdlet używanych w [Utwórz konto użytkownika](#create-a-user-account) sekcji:</span><span class="sxs-lookup"><span data-stu-id="ee787-110">For Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763), the [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) module must be imported with the [WindowsCompatibility module](https://github.com/PowerShell/WindowsCompatibility) to gain access to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet used in the [Create a user account](#create-a-user-account) section:</span></span>
>
> ```powershell
> Install-Module WindowsCompatibility -Scope CurrentUser
> Import-WinModule Microsoft.PowerShell.LocalAccounts
> ```

## <a name="deployment-type"></a><span data-ttu-id="ee787-111">Typ wdrożenia</span><span class="sxs-lookup"><span data-stu-id="ee787-111">Deployment type</span></span>

<span data-ttu-id="ee787-112">Można utworzyć albo zależny od struktury lub niezależna Windows wdrożenia usługi.</span><span class="sxs-lookup"><span data-stu-id="ee787-112">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="ee787-113">Aby uzyskać informacje i wskazówki dotyczące scenariuszy wdrażania, zobacz [wdrażanie aplikacji .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="ee787-113">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="ee787-114">Wdrożenie zależny od struktury</span><span class="sxs-lookup"><span data-stu-id="ee787-114">Framework-dependent deployment</span></span>

<span data-ttu-id="ee787-115">Zależny od struktury wdrożenia (stacje) zależy od obecności udostępnione całego systemu w wersji programu .NET Core w systemie docelowym.</span><span class="sxs-lookup"><span data-stu-id="ee787-115">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="ee787-116">Stosowania w scenariuszu z stacje za pomocą aplikacji platformy ASP.NET Core Windows Service SDK tworzy plik wykonywalny (*\*.exe*), co jest nazywane *zależny od struktury pliku wykonywalnego*.</span><span class="sxs-lookup"><span data-stu-id="ee787-116">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="ee787-117">Niezależne wdrożenia</span><span class="sxs-lookup"><span data-stu-id="ee787-117">Self-contained deployment</span></span>

<span data-ttu-id="ee787-118">Niezależne wdrożenia (— SCD) nie jest zależny od obecności składniki współużytkowane w systemie docelowym.</span><span class="sxs-lookup"><span data-stu-id="ee787-118">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="ee787-119">Środowisko uruchomieniowe i zależności aplikacji są wdrażane za pomocą aplikacji do systemu macierzystego.</span><span class="sxs-lookup"><span data-stu-id="ee787-119">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="ee787-120">Konwertuj projekt w usłudze Windows</span><span class="sxs-lookup"><span data-stu-id="ee787-120">Convert a project into a Windows Service</span></span>

<span data-ttu-id="ee787-121">Do istniejącego projektu platformy ASP.NET Core do uruchomienia aplikacji jako usługi, należy wprowadzić następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="ee787-121">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="ee787-122">Aktualizacje plików projektu</span><span class="sxs-lookup"><span data-stu-id="ee787-122">Project file updates</span></span>

<span data-ttu-id="ee787-123">Na podstawie wybranego elementu [typu wdrożenia](#deployment-type), należy zaktualizować plik projektu:</span><span class="sxs-lookup"><span data-stu-id="ee787-123">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="ee787-124">Framework-dependent Deployment (FDD)</span><span class="sxs-lookup"><span data-stu-id="ee787-124">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="ee787-125">Dodaj Windows [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) do `<PropertyGroup>` zawierający platformę docelową.</span><span class="sxs-lookup"><span data-stu-id="ee787-125">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="ee787-126">W poniższym przykładzie ustawiono RID `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="ee787-126">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="ee787-127">Dodaj `<SelfContained>` właściwością `false`.</span><span class="sxs-lookup"><span data-stu-id="ee787-127">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="ee787-128">Te właściwości poinstruować zestawu SDK w celu wygenerowania pliku wykonywalnego (*.exe*) pliku Windows.</span><span class="sxs-lookup"><span data-stu-id="ee787-128">These properties instruct the SDK to generate an executable (*.exe*) file for Windows.</span></span>

<span data-ttu-id="ee787-129">A *web.config* pliku, który zazwyczaj jest generowany podczas publikowania aplikacji ASP.NET Core, nie jest konieczne w przypadku aplikacji usługi Windows.</span><span class="sxs-lookup"><span data-stu-id="ee787-129">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="ee787-130">Aby wyłączyć tworzenie *web.config* Dodaj `<IsTransformWebConfigDisabled>` właściwością `true`.</span><span class="sxs-lookup"><span data-stu-id="ee787-130">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

::: moniker range=">= aspnetcore-2.2"

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

<span data-ttu-id="ee787-131">Dodaj `<UseAppHost>` właściwością `true`.</span><span class="sxs-lookup"><span data-stu-id="ee787-131">Add the `<UseAppHost>` property set to `true`.</span></span> <span data-ttu-id="ee787-132">Ta właściwość zapewnia usługi ze ścieżką aktywacji (plik wykonywalny *.exe*) dla Dyskietki.</span><span class="sxs-lookup"><span data-stu-id="ee787-132">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="ee787-133">Niezależne wdrożenia (— SCD)</span><span class="sxs-lookup"><span data-stu-id="ee787-133">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="ee787-134">Potwierdzić obecność Windows [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) lub identyfikatorów RID, aby dodać `<PropertyGroup>` zawierający platformę docelową.</span><span class="sxs-lookup"><span data-stu-id="ee787-134">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="ee787-135">Wyłączyć tworzenie *web.config* pliku, dodając `<IsTransformWebConfigDisabled>` właściwością `true`.</span><span class="sxs-lookup"><span data-stu-id="ee787-135">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="ee787-136">Aby opublikować dla wielu identyfikatorów RID:</span><span class="sxs-lookup"><span data-stu-id="ee787-136">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="ee787-137">Podaj identyfikatorów RID w liście rozdzielanej średnikami.</span><span class="sxs-lookup"><span data-stu-id="ee787-137">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="ee787-138">Użyj nazwy właściwości `<RuntimeIdentifiers>` (w liczbie mnogiej).</span><span class="sxs-lookup"><span data-stu-id="ee787-138">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="ee787-139">Aby uzyskać więcej informacji, zobacz [.NET Core RID katalogu](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="ee787-139">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="ee787-140">Dodaj odwołania do pakietu dla [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="ee787-140">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="ee787-141">Aby włączyć rejestrowanie w dzienniku zdarzeń Windows, należy dodać odwołania do pakietu dla [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="ee787-141">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="ee787-142">Aby uzyskać więcej informacji, zobacz [obsługi, uruchamianie i zatrzymywanie wydarzeń](#handle-starting-and-stopping-events) sekcji.</span><span class="sxs-lookup"><span data-stu-id="ee787-142">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="ee787-143">Aktualizacje Program.Main</span><span class="sxs-lookup"><span data-stu-id="ee787-143">Program.Main updates</span></span>

<span data-ttu-id="ee787-144">Wprowadź następujące zmiany w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="ee787-144">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="ee787-145">Do testowania i debugowania, gdy działają poza usługą, Dodaj kod, aby ustalić, czy aplikacja jest uruchomiona jako usługę lub aplikację konsoli.</span><span class="sxs-lookup"><span data-stu-id="ee787-145">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="ee787-146">Sprawdź, czy jest dołączony debuger a `--console` argument wiersza polecenia jest obecny.</span><span class="sxs-lookup"><span data-stu-id="ee787-146">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="ee787-147">Jeśli tych warunków jest spełniony, (aplikacja nie jest uruchamiana jako usługa), należy wywołać <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> na hoście w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ee787-147">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="ee787-148">Jeśli warunki są fałszywe, (aplikacja jest uruchamiana jako usługa):</span><span class="sxs-lookup"><span data-stu-id="ee787-148">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="ee787-149">Wywołaj <xref:System.IO.Directory.SetCurrentDirectory*> i użyj ścieżki do lokalizacji publikowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee787-149">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="ee787-150">Nie wywołuj <xref:System.IO.Directory.GetCurrentDirectory*> można uzyskać ścieżki, ponieważ aplikacji usługi Windows, która zwraca *C:\\WINDOWS\\system32* folderu podczas <xref:System.IO.Directory.GetCurrentDirectory*> nosi nazwę.</span><span class="sxs-lookup"><span data-stu-id="ee787-150">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="ee787-151">Aby uzyskać więcej informacji, zobacz [bieżący katalog i katalog główny zawartości](#current-directory-and-content-root) sekcji.</span><span class="sxs-lookup"><span data-stu-id="ee787-151">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="ee787-152">Wywołaj <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> do uruchomienia aplikacji jako usługi.</span><span class="sxs-lookup"><span data-stu-id="ee787-152">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="ee787-153">Ponieważ [dostawcę konfiguracji wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) wymaga pary nazwa wartość dla argumentów wiersza poleceń `--console` przełącznik został usunięty z argumentów przed <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> otrzymuje je.</span><span class="sxs-lookup"><span data-stu-id="ee787-153">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="ee787-154">Można zapisać w dzienniku zdarzeń Windows, należy dodać dostawcę dziennika zdarzeń <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="ee787-154">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="ee787-155">Poziom rejestrowania za pomocą `Logging:LogLevel:Default` w *appsettings. Production.JSON* pliku.</span><span class="sxs-lookup"><span data-stu-id="ee787-155">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="ee787-156">Pokaz i testowania pliku ustawień produkcji przykładową aplikację ustawia poziom rejestrowania `Information`.</span><span class="sxs-lookup"><span data-stu-id="ee787-156">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="ee787-157">W środowisku produkcyjnym, wartość jest zazwyczaj równa `Error`.</span><span class="sxs-lookup"><span data-stu-id="ee787-157">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="ee787-158">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="ee787-158">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="publish-the-app"></a><span data-ttu-id="ee787-159">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ee787-159">Publish the app</span></span>

<span data-ttu-id="ee787-160">Publikowanie aplikacji za pomocą [publikowania dotnet](/dotnet/articles/core/tools/dotnet-publish), [profilu publikowania w programie Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles), lub Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ee787-160">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="ee787-161">Jeśli używasz programu Visual Studio, wybierz opcję **FolderProfile** i skonfigurować **lokalizacji docelowej** przed wybraniem **Publikuj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="ee787-161">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="ee787-162">Aby opublikować przykładową aplikację przy użyciu narzędzi interfejsu wiersza polecenia (CLI), uruchom [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie w powłoce poleceń Windows z folderu projektu z konfiguracją wydania, przekazana do [- c | — Konfiguracja ](/dotnet/core/tools/dotnet-publish#options) opcji.</span><span class="sxs-lookup"><span data-stu-id="ee787-162">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command in a Windows command shell from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="ee787-163">Użyj [-o |--dane wyjściowe](/dotnet/core/tools/dotnet-publish#options) opcji ze ścieżką do publikowania do folderu poza aplikacją.</span><span class="sxs-lookup"><span data-stu-id="ee787-163">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="ee787-164">Publikowanie wdrożenia zależny od struktury (stacje)</span><span class="sxs-lookup"><span data-stu-id="ee787-164">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="ee787-165">W poniższym przykładzie aplikacja została opublikowana do *c:\\svc* folderu:</span><span class="sxs-lookup"><span data-stu-id="ee787-165">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="ee787-166">Publikowanie niezależne wdrożenia (— SCD)</span><span class="sxs-lookup"><span data-stu-id="ee787-166">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="ee787-167">Identyfikator RID musi być określona w `<RuntimeIdenfifier>` (lub `<RuntimeIdentifiers>`) właściwości pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="ee787-167">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="ee787-168">Podaj środowisko uruchomieniowe [- r | — środowisko uruchomieniowe](/dotnet/core/tools/dotnet-publish#options) opcji `dotnet publish` polecenia.</span><span class="sxs-lookup"><span data-stu-id="ee787-168">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="ee787-169">W poniższym przykładzie aplikacja została opublikowana na potrzeby `win7-x64` środowiska uruchomieniowego *c:\\svc* folderu:</span><span class="sxs-lookup"><span data-stu-id="ee787-169">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

## <a name="create-a-user-account"></a><span data-ttu-id="ee787-170">Utwórz konto użytkownika</span><span class="sxs-lookup"><span data-stu-id="ee787-170">Create a user account</span></span>

<span data-ttu-id="ee787-171">Utwórz konto użytkownika dla usługi przy użyciu [New LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) polecenia cmdlet z administracyjne powłoki poleceń programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="ee787-171">Create a user account for the service using the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell:</span></span>

```powershell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="ee787-172">Podaj [silne hasło](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) po wyświetleniu monitu.</span><span class="sxs-lookup"><span data-stu-id="ee787-172">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="ee787-173">Dla przykładowej aplikacji, należy utworzyć konto użytkownika o nazwie `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="ee787-173">For the sample app, create a user account with the name `ServiceUser`.</span></span>

```powershell
New-LocalUser -Name ServiceUser
```

<span data-ttu-id="ee787-174">Chyba że `-AccountExpires` parametru do [New LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) polecenia cmdlet z wygaśnięciem <xref:System.DateTime>, konto nie wygasa.</span><span class="sxs-lookup"><span data-stu-id="ee787-174">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="ee787-175">Aby uzyskać więcej informacji, zobacz [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) i [kont użytkowników usług](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="ee787-175">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="ee787-176">Innym sposobem zarządzania użytkownikami, podczas korzystania z usługi Active Directory jest użycie kont usług zarządzanych.</span><span class="sxs-lookup"><span data-stu-id="ee787-176">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="ee787-177">Aby uzyskać więcej informacji, zobacz [omówienie kont usług zarządzanych przez grupę](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="ee787-177">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="set-permission-log-on-as-a-service"></a><span data-ttu-id="ee787-178">Ustaw uprawnienia: Zaloguj się jako usługa</span><span class="sxs-lookup"><span data-stu-id="ee787-178">Set permission: Log on as a service</span></span>

<span data-ttu-id="ee787-179">Udzielanie zapisu/odczytu/wykonania dostępu do folderu aplikacji przy użyciu [icacls](/windows-server/administration/windows-commands/icacls) poleceń administracyjnych powłoki poleceń programu PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="ee787-179">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command an administrative PowerShell 6 command shell.</span></span>

```powershell
icacls "{PATH}" /grant "{USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS}" /t
```

* `{PATH}` <span data-ttu-id="ee787-180">&ndash; Ścieżka do folderu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee787-180">&ndash; Path to the app's folder.</span></span>
* `{USER ACCOUNT}` <span data-ttu-id="ee787-181">&ndash; Konto użytkownika (SID).</span><span class="sxs-lookup"><span data-stu-id="ee787-181">&ndash; The user account (SID).</span></span>
* `(OI)` <span data-ttu-id="ee787-182">&ndash; Obiekt dziedziczenia flagi propaguje uprawnienia do podrzędnych plików.</span><span class="sxs-lookup"><span data-stu-id="ee787-182">&ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* `(CI)` <span data-ttu-id="ee787-183">&ndash; Flaga Dziedziczenie kontenera propaguje uprawnienia do folderów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="ee787-183">&ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* `{PERMISSION FLAGS}` <span data-ttu-id="ee787-184">&ndash; Ustawia uprawnienia dostępu do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee787-184">&ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="ee787-185">Zapis (`W`)</span><span class="sxs-lookup"><span data-stu-id="ee787-185">Write (`W`)</span></span>
  * <span data-ttu-id="ee787-186">Odczyt (`R`)</span><span class="sxs-lookup"><span data-stu-id="ee787-186">Read (`R`)</span></span>
  * <span data-ttu-id="ee787-187">Wykonaj (`X`)</span><span class="sxs-lookup"><span data-stu-id="ee787-187">Execute (`X`)</span></span>
  * <span data-ttu-id="ee787-188">Pełne (`F`)</span><span class="sxs-lookup"><span data-stu-id="ee787-188">Full (`F`)</span></span>
  * <span data-ttu-id="ee787-189">Modyfikowanie (`M`)</span><span class="sxs-lookup"><span data-stu-id="ee787-189">Modify (`M`)</span></span>
* `/t` <span data-ttu-id="ee787-190">&ndash; Rekursywnie dotyczą plików i folderów podrzędnych istniejących.</span><span class="sxs-lookup"><span data-stu-id="ee787-190">&ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="ee787-191">Dla przykładowej aplikacji opublikowany *c:\\svc* folder i `ServiceUser` konta z uprawnieniami do zapisu/odczytu/wykonania, użyj następującego polecenia administracyjne powłoki poleceń programu PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="ee787-191">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command an administrative PowerShell 6 command shell.</span></span>

```powershell
icacls "c:\svc" /grant "ServiceUser:(OI)(CI)WRX" /t
```

<span data-ttu-id="ee787-192">Aby uzyskać więcej informacji, zobacz [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="ee787-192">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

## <a name="create-the-service"></a><span data-ttu-id="ee787-193">Tworzenie usługi</span><span class="sxs-lookup"><span data-stu-id="ee787-193">Create the service</span></span>

<span data-ttu-id="ee787-194">Użyj [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) skrypt programu PowerShell, aby zarejestrować usługę.</span><span class="sxs-lookup"><span data-stu-id="ee787-194">Use the [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) PowerShell script to register the service.</span></span> <span data-ttu-id="ee787-195">Z administracyjne powłoki poleceń programu PowerShell 6 Uruchom skrypt za pomocą następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="ee787-195">From an administrative PowerShell 6 command shell, execute the script with the following command:</span></span>

```powershell
.\RegisterService.ps1 
    -Name {NAME} 
    -DisplayName "{DISPLAY NAME}" 
    -Description "{DESCRIPTION}" 
    -Exe "{PATH TO EXE}\{ASSEMBLY NAME}.exe" 
    -User {DOMAIN\USER}
```

<span data-ttu-id="ee787-196">W poniższym przykładzie przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="ee787-196">In the following example for the sample app:</span></span>

* <span data-ttu-id="ee787-197">Usługa jest o nazwie **Moja_usługa**.</span><span class="sxs-lookup"><span data-stu-id="ee787-197">The service is named **MyService**.</span></span>
* <span data-ttu-id="ee787-198">Opublikowana usługa znajduje się w *c:\\svc* folderu.</span><span class="sxs-lookup"><span data-stu-id="ee787-198">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="ee787-199">Nosi nazwę pliku wykonywalnego aplikacji *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="ee787-199">The app executable is named *SampleApp.exe*.</span></span>
* <span data-ttu-id="ee787-200">Usługa jest uruchamiana w ramach `ServiceUser` konta.</span><span class="sxs-lookup"><span data-stu-id="ee787-200">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="ee787-201">W poniższym przykładzie polecenia nazwy komputera lokalnego jest `Desktop-PC`.</span><span class="sxs-lookup"><span data-stu-id="ee787-201">In the following example command, the local machine name is `Desktop-PC`.</span></span> <span data-ttu-id="ee787-202">Zastąp `Desktop-PC` przy użyciu nazwy komputera lub domeny w systemie.</span><span class="sxs-lookup"><span data-stu-id="ee787-202">Replace `Desktop-PC` with the computer name or domain for your system.</span></span>

```powershell
.\RegisterService.ps1 
    -Name MyService 
    -DisplayName "My Cool Service" 
    -Description "This is the Sample App service." 
    -Exe "c:\svc\SampleApp.exe" 
    -User Desktop-PC\ServiceUser
```

## <a name="manage-the-service"></a><span data-ttu-id="ee787-203">Zarządzanie usługą</span><span class="sxs-lookup"><span data-stu-id="ee787-203">Manage the service</span></span>

### <a name="start-the-service"></a><span data-ttu-id="ee787-204">Uruchom usługę</span><span class="sxs-lookup"><span data-stu-id="ee787-204">Start the service</span></span>

<span data-ttu-id="ee787-205">Uruchom usługę za pomocą `Start-Service -Name {NAME}` polecenia programu PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="ee787-205">Start the service with the `Start-Service -Name {NAME}` PowerShell 6 command.</span></span>

<span data-ttu-id="ee787-206">Aby uruchomić usługę aplikacji przykładowej, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="ee787-206">To start the sample app service, use the following command:</span></span>

```powershell
Start-Service -Name MyService
```

<span data-ttu-id="ee787-207">Polecenie zajmuje kilka sekund, aby uruchomić usługę.</span><span class="sxs-lookup"><span data-stu-id="ee787-207">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="ee787-208">Sprawdź stan usługi</span><span class="sxs-lookup"><span data-stu-id="ee787-208">Determine the service status</span></span>

<span data-ttu-id="ee787-209">Aby sprawdzić stan usługi, użyj `Get-Service -Name {NAME}` polecenia programu PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="ee787-209">To check the status of the service, use the `Get-Service -Name {NAME}` PowerShell 6 command.</span></span> <span data-ttu-id="ee787-210">Stan jest zgłaszany jako jeden z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="ee787-210">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

<span data-ttu-id="ee787-211">Użyj następującego polecenia, aby sprawdzić stan usługi aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="ee787-211">Use the following command to check the status of the sample app service:</span></span>

```powershell
Get-Service -Name MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="ee787-212">Przeglądaj, usługi aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="ee787-212">Browse a web app service</span></span>

<span data-ttu-id="ee787-213">Kiedy usługa jest w `RUNNING` stanu i usługi w przypadku aplikacji sieci web, przeglądanie aplikacji w ścieżce (domyślnie `http://localhost:5000`, który przekierowuje do `https://localhost:5001` przy użyciu [HTTPS przekierowanie w oprogramowaniu pośredniczącym](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="ee787-213">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="ee787-214">Usługa app service przykładowego, można przeglądać w tej aplikacji w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="ee787-214">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="ee787-215">Zatrzymaj usługę</span><span class="sxs-lookup"><span data-stu-id="ee787-215">Stop the service</span></span>

<span data-ttu-id="ee787-216">Zatrzymaj usługę za pomocą `Stop-Service -Name {NAME}` polecenia programu Powershell 6.</span><span class="sxs-lookup"><span data-stu-id="ee787-216">Stop the service with the `Stop-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="ee787-217">Następujące polecenie zatrzymuje usługę aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="ee787-217">The following command stops the sample app service:</span></span>

```powershell
Stop-Service -Name MyService
```

### <a name="remove-the-service"></a><span data-ttu-id="ee787-218">Usuń usługę</span><span class="sxs-lookup"><span data-stu-id="ee787-218">Remove the service</span></span>

<span data-ttu-id="ee787-219">Po krótkiej chwili zatrzymania usługi, należy usunąć usługę z `Remove-Service -Name {NAME}` polecenia programu Powershell 6.</span><span class="sxs-lookup"><span data-stu-id="ee787-219">After a short delay to stop a service, remove the service with the `Remove-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="ee787-220">Sprawdź stan usługi aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="ee787-220">Check the status of the sample app service:</span></span>

```powershell
Remove-Service -Name MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="ee787-221">Obsługa uruchamianie i zatrzymywanie wydarzeń</span><span class="sxs-lookup"><span data-stu-id="ee787-221">Handle starting and stopping events</span></span>

<span data-ttu-id="ee787-222">Aby obsłużyć <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, i <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> zdarzeń, wykonaj następujące dodatkowe zmiany:</span><span class="sxs-lookup"><span data-stu-id="ee787-222">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="ee787-223">Utwórz klasę, która pochodzi od klasy <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> z `OnStarting`, `OnStarted`, i `OnStopping` metody:</span><span class="sxs-lookup"><span data-stu-id="ee787-223">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="ee787-224">Tworzenie metody rozszerzenia dla <xref:Microsoft.AspNetCore.Hosting.IWebHost> , który przekazuje `CustomWebHostService` do <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="ee787-224">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="ee787-225">W `Program.Main`, wywołaj `RunAsCustomService` rozszerzenia zamiast metody <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="ee787-225">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="ee787-226">Aby wyświetlić lokalizację <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> w `Program.Main`, można znaleźć w przykładzie kodu pokazano w [konwertować projekt w usłudze Windows](#convert-a-project-into-a-windows-service) sekcji.</span><span class="sxs-lookup"><span data-stu-id="ee787-226">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ee787-227">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="ee787-227">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ee787-228">Usługi wchodzić w interakcje z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub moduł równoważenia obciążenia, które mogą wymagać dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="ee787-228">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="ee787-229">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="ee787-229">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="ee787-230">Konfigurowanie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="ee787-230">Configure HTTPS</span></span>

<span data-ttu-id="ee787-231">Aby skonfigurować usługę z bezpiecznego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="ee787-231">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="ee787-232">Utwórz certyfikat X.509 dla systemu macierzystego przy użyciu pozyskiwania certyfikatu używanej platformy i mechanizmy wdrażania.</span><span class="sxs-lookup"><span data-stu-id="ee787-232">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="ee787-233">Określ [konfiguracji punktu końcowego HTTPS serwera Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) przy użyciu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="ee787-233">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="ee787-234">Użycie certyfikatu deweloperskiego platformy ASP.NET Core HTTPS, aby zabezpieczyć punkt końcowy usługi nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="ee787-234">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="ee787-235">Bieżący katalog i katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="ee787-235">Current directory and content root</span></span>

<span data-ttu-id="ee787-236">Bieżący katalog roboczy zwracany przez wywołanie metody <xref:System.IO.Directory.GetCurrentDirectory*> dla usługi Windows jest *C:\\WINDOWS\\system32* folderu.</span><span class="sxs-lookup"><span data-stu-id="ee787-236">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="ee787-237">*System32* folder nie jest odpowiednią lokalizację do przechowywania plików usługi (na przykład pliki ustawień).</span><span class="sxs-lookup"><span data-stu-id="ee787-237">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="ee787-238">Użyj jednej z następujących metod do utrzymywania i uzyskać dostęp do zasobów i plików ustawień usługi.</span><span class="sxs-lookup"><span data-stu-id="ee787-238">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="ee787-239">Ustaw ścieżkę katalogu głównego zawartości do folderu aplikacji</span><span class="sxs-lookup"><span data-stu-id="ee787-239">Set the content root path to the app's folder</span></span>

<span data-ttu-id="ee787-240"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> Tej samej ścieżki, które są udostępniane `binPath` argumentów podczas tworzenia usługi.</span><span class="sxs-lookup"><span data-stu-id="ee787-240">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="ee787-241">Zamiast wywoływać metodę `GetCurrentDirectory` do tworzenia ścieżek do plików ustawień, należy wywołać <xref:System.IO.Directory.SetCurrentDirectory*> ze ścieżką do katalogu głównego zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee787-241">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="ee787-242">W `Program.Main`, określić ścieżkę do folderu pliku wykonywalnego usługi i ustanowić zawartości katalogu głównego aplikacji za pomocą ścieżki:</span><span class="sxs-lookup"><span data-stu-id="ee787-242">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="ee787-243">Store usługi plików w odpowiedniej lokalizacji na dysku</span><span class="sxs-lookup"><span data-stu-id="ee787-243">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="ee787-244">Określ ścieżkę bezwzględną z <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> przy użyciu <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do folderu zawierającego pliki.</span><span class="sxs-lookup"><span data-stu-id="ee787-244">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ee787-245">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ee787-245">Additional resources</span></span>

* <span data-ttu-id="ee787-246">[Konfiguracja punktu końcowego kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym konfiguracja protokołu HTTPS i obsługa SNI)</span><span class="sxs-lookup"><span data-stu-id="ee787-246">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
