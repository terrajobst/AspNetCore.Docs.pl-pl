---
title: Host platformy ASP.NET Core w usłudze Windows
author: guardrex
description: Dowiedz się, jak udostępnić aplikację ASP.NET Core w usłudze Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/07/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 5393dcec4f5e2eb37ec9cac2435bf15eedb8e361
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159372"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="3017b-103">Host platformy ASP.NET Core w usłudze Windows</span><span class="sxs-lookup"><span data-stu-id="3017b-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="3017b-104">Przez [Luke Latham](https://github.com/guardrex) i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="3017b-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="3017b-105">Aplikacji ASP.NET Core może być hostowana na Windows jako [usługi Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) bez korzystania z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="3017b-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="3017b-106">Po hostowany jako usługa Windows, aplikacja zostanie automatycznie uruchomiona po ponownym uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="3017b-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="3017b-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3017b-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="deployment-type"></a><span data-ttu-id="3017b-108">Typ wdrożenia</span><span class="sxs-lookup"><span data-stu-id="3017b-108">Deployment type</span></span>

<span data-ttu-id="3017b-109">Można utworzyć albo zależny od struktury lub niezależna Windows wdrożenia usługi.</span><span class="sxs-lookup"><span data-stu-id="3017b-109">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="3017b-110">Aby uzyskać informacje i wskazówki dotyczące scenariuszy wdrażania, zobacz [wdrażanie aplikacji .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="3017b-110">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="3017b-111">Wdrożenie zależny od struktury</span><span class="sxs-lookup"><span data-stu-id="3017b-111">Framework-dependent deployment</span></span>

<span data-ttu-id="3017b-112">Zależny od struktury wdrożenia (stacje) zależy od obecności udostępnione całego systemu w wersji programu .NET Core w systemie docelowym.</span><span class="sxs-lookup"><span data-stu-id="3017b-112">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="3017b-113">Stosowania w scenariuszu z stacje za pomocą aplikacji platformy ASP.NET Core Windows Service SDK tworzy plik wykonywalny (*\*.exe*), co jest nazywane *zależny od struktury pliku wykonywalnego*.</span><span class="sxs-lookup"><span data-stu-id="3017b-113">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="3017b-114">Niezależne wdrożenia</span><span class="sxs-lookup"><span data-stu-id="3017b-114">Self-contained deployment</span></span>

<span data-ttu-id="3017b-115">Niezależne wdrożenia (— SCD) nie jest zależny od obecności składniki współużytkowane w systemie docelowym.</span><span class="sxs-lookup"><span data-stu-id="3017b-115">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="3017b-116">Środowisko uruchomieniowe i zależności aplikacji są wdrażane za pomocą aplikacji do systemu macierzystego.</span><span class="sxs-lookup"><span data-stu-id="3017b-116">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="3017b-117">Konwertuj projekt w usłudze Windows</span><span class="sxs-lookup"><span data-stu-id="3017b-117">Convert a project into a Windows Service</span></span>

<span data-ttu-id="3017b-118">Do istniejącego projektu platformy ASP.NET Core do uruchomienia aplikacji jako usługi, należy wprowadzić następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="3017b-118">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="3017b-119">Aktualizacje plików projektu</span><span class="sxs-lookup"><span data-stu-id="3017b-119">Project file updates</span></span>

<span data-ttu-id="3017b-120">Na podstawie wybranego elementu [typu wdrożenia](#deployment-type), należy zaktualizować plik projektu:</span><span class="sxs-lookup"><span data-stu-id="3017b-120">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="3017b-121">Framework-dependent Deployment (FDD)</span><span class="sxs-lookup"><span data-stu-id="3017b-121">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="3017b-122">Dodaj Windows [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) do `<PropertyGroup>` zawierający platformę docelową.</span><span class="sxs-lookup"><span data-stu-id="3017b-122">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="3017b-123">W poniższym przykładzie ustawiono RID `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="3017b-123">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="3017b-124">Dodaj `<SelfContained>` właściwością `false`.</span><span class="sxs-lookup"><span data-stu-id="3017b-124">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="3017b-125">Te właściwości poinstruować zestawu SDK w celu wygenerowania pliku wykonywalnego (*.exe*) pliku Windows.</span><span class="sxs-lookup"><span data-stu-id="3017b-125">These properties instruct the SDK to generate an executable (*.exe*) file for Windows.</span></span>

<span data-ttu-id="3017b-126">A *web.config* pliku, który zazwyczaj jest generowany podczas publikowania aplikacji ASP.NET Core, nie jest konieczne w przypadku aplikacji usługi Windows.</span><span class="sxs-lookup"><span data-stu-id="3017b-126">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="3017b-127">Aby wyłączyć tworzenie *web.config* Dodaj `<IsTransformWebConfigDisabled>` właściwością `true`.</span><span class="sxs-lookup"><span data-stu-id="3017b-127">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="3017b-128">Dodaj `<UseAppHost>` właściwością `true`.</span><span class="sxs-lookup"><span data-stu-id="3017b-128">Add the `<UseAppHost>` property set to `true`.</span></span> <span data-ttu-id="3017b-129">Ta właściwość zapewnia usługi ze ścieżką aktywacji (plik wykonywalny *.exe*) dla Dyskietki.</span><span class="sxs-lookup"><span data-stu-id="3017b-129">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="3017b-130">Niezależne wdrożenia (— SCD)</span><span class="sxs-lookup"><span data-stu-id="3017b-130">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="3017b-131">Potwierdzić obecność Windows [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) lub identyfikatorów RID, aby dodać `<PropertyGroup>` zawierający platformę docelową.</span><span class="sxs-lookup"><span data-stu-id="3017b-131">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="3017b-132">Wyłączyć tworzenie *web.config* pliku, dodając `<IsTransformWebConfigDisabled>` właściwością `true`.</span><span class="sxs-lookup"><span data-stu-id="3017b-132">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="3017b-133">Aby opublikować dla wielu identyfikatorów RID:</span><span class="sxs-lookup"><span data-stu-id="3017b-133">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="3017b-134">Podaj identyfikatorów RID w liście rozdzielanej średnikami.</span><span class="sxs-lookup"><span data-stu-id="3017b-134">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="3017b-135">Użyj nazwy właściwości `<RuntimeIdentifiers>` (w liczbie mnogiej).</span><span class="sxs-lookup"><span data-stu-id="3017b-135">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="3017b-136">Aby uzyskać więcej informacji, zobacz [.NET Core RID katalogu](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="3017b-136">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="3017b-137">Dodaj odwołania do pakietu dla [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="3017b-137">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="3017b-138">Aby włączyć rejestrowanie w dzienniku zdarzeń Windows, należy dodać odwołania do pakietu dla [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="3017b-138">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="3017b-139">Aby uzyskać więcej informacji, zobacz [obsługi, uruchamianie i zatrzymywanie wydarzeń](#handle-starting-and-stopping-events) sekcji.</span><span class="sxs-lookup"><span data-stu-id="3017b-139">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="3017b-140">Aktualizacje Program.Main</span><span class="sxs-lookup"><span data-stu-id="3017b-140">Program.Main updates</span></span>

<span data-ttu-id="3017b-141">Wprowadź następujące zmiany w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="3017b-141">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="3017b-142">Do testowania i debugowania, gdy działają poza usługą, Dodaj kod, aby ustalić, czy aplikacja jest uruchomiona jako usługę lub aplikację konsoli.</span><span class="sxs-lookup"><span data-stu-id="3017b-142">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="3017b-143">Sprawdź, czy jest dołączony debuger a `--console` argument wiersza polecenia jest obecny.</span><span class="sxs-lookup"><span data-stu-id="3017b-143">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="3017b-144">Jeśli tych warunków jest spełniony, (aplikacja nie jest uruchamiana jako usługa), należy wywołać <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> na hoście w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3017b-144">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="3017b-145">Jeśli warunki są fałszywe, (aplikacja jest uruchamiana jako usługa):</span><span class="sxs-lookup"><span data-stu-id="3017b-145">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="3017b-146">Wywołaj <xref:System.IO.Directory.SetCurrentDirectory*> i użyj ścieżki do lokalizacji publikowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3017b-146">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="3017b-147">Nie wywołuj <xref:System.IO.Directory.GetCurrentDirectory*> można uzyskać ścieżki, ponieważ aplikacji usługi Windows, która zwraca *C:\\WINDOWS\\system32* folderu podczas `GetCurrentDirectory` nosi nazwę.</span><span class="sxs-lookup"><span data-stu-id="3017b-147">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when `GetCurrentDirectory` is called.</span></span> <span data-ttu-id="3017b-148">Aby uzyskać więcej informacji, zobacz [bieżący katalog i katalog główny zawartości](#current-directory-and-content-root) sekcji.</span><span class="sxs-lookup"><span data-stu-id="3017b-148">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="3017b-149">Wywołaj <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> do uruchomienia aplikacji jako usługi.</span><span class="sxs-lookup"><span data-stu-id="3017b-149">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="3017b-150">Ponieważ [dostawcę konfiguracji wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) wymaga pary nazwa wartość dla argumentów wiersza poleceń `--console` przełącznik został usunięty z argumentów przed <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> otrzymuje je.</span><span class="sxs-lookup"><span data-stu-id="3017b-150">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="3017b-151">Można zapisać w dzienniku zdarzeń Windows, należy dodać dostawcę dziennika zdarzeń <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="3017b-151">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="3017b-152">Poziom rejestrowania za pomocą `Logging:LogLevel:Default` w *appsettings. Production.JSON* pliku.</span><span class="sxs-lookup"><span data-stu-id="3017b-152">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="3017b-153">Pokaz i testowania pliku ustawień produkcji przykładową aplikację ustawia poziom rejestrowania `Information`.</span><span class="sxs-lookup"><span data-stu-id="3017b-153">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="3017b-154">W środowisku produkcyjnym, wartość jest zazwyczaj równa `Error`.</span><span class="sxs-lookup"><span data-stu-id="3017b-154">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="3017b-155">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="3017b-155">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

### <a name="publish-the-app"></a><span data-ttu-id="3017b-156">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="3017b-156">Publish the app</span></span>

<span data-ttu-id="3017b-157">Publikowanie aplikacji za pomocą [publikowania dotnet](/dotnet/articles/core/tools/dotnet-publish), [profilu publikowania w programie Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles), lub Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3017b-157">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="3017b-158">Jeśli używasz programu Visual Studio, wybierz opcję **FolderProfile** i skonfigurować **lokalizacji docelowej** przed wybraniem **Publikuj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="3017b-158">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="3017b-159">Aby opublikować przykładową aplikację przy użyciu narzędzi interfejsu wiersza polecenia (CLI), uruchom [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie w wierszu polecenia z folderu projektu z konfiguracją wydania, przekazana do [- c |--konfiguracji](/dotnet/core/tools/dotnet-publish#options)opcji.</span><span class="sxs-lookup"><span data-stu-id="3017b-159">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="3017b-160">Użyj [-o |--dane wyjściowe](/dotnet/core/tools/dotnet-publish#options) opcji ze ścieżką do publikowania do folderu poza aplikacją.</span><span class="sxs-lookup"><span data-stu-id="3017b-160">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

#### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="3017b-161">Publikowanie wdrożenia zależny od struktury (stacje)</span><span class="sxs-lookup"><span data-stu-id="3017b-161">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="3017b-162">W poniższym przykładzie aplikacja została opublikowana do *c:\\svc* folderu:</span><span class="sxs-lookup"><span data-stu-id="3017b-162">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

#### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="3017b-163">Publikowanie niezależne wdrożenia (— SCD)</span><span class="sxs-lookup"><span data-stu-id="3017b-163">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="3017b-164">Identyfikator RID musi być określona w `<RuntimeIdenfifier>` (lub `<RuntimeIdentifiers>`) właściwości pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="3017b-164">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="3017b-165">Podaj środowisko uruchomieniowe [- r | — środowisko uruchomieniowe](/dotnet/core/tools/dotnet-publish#options) opcji `dotnet publish` polecenia.</span><span class="sxs-lookup"><span data-stu-id="3017b-165">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="3017b-166">W poniższym przykładzie aplikacja została opublikowana na potrzeby `win7-x64` środowiska uruchomieniowego *c:\\svc* folderu:</span><span class="sxs-lookup"><span data-stu-id="3017b-166">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

### <a name="create-a-user-account"></a><span data-ttu-id="3017b-167">Utwórz konto użytkownika</span><span class="sxs-lookup"><span data-stu-id="3017b-167">Create a user account</span></span>

<span data-ttu-id="3017b-168">Utwórz konto użytkownika dla usługi przy użyciu `net user` polecenia powłoki poleceń administracyjnych:</span><span class="sxs-lookup"><span data-stu-id="3017b-168">Create a user account for the service using the `net user` command from an administrative command shell:</span></span>

```console
net user {USER ACCOUNT} {PASSWORD} /add
```

<span data-ttu-id="3017b-169">Wygaśnięcie hasła domyślny to sześć tygodni.</span><span class="sxs-lookup"><span data-stu-id="3017b-169">The default password expiration is six weeks.</span></span>

<span data-ttu-id="3017b-170">Dla przykładowej aplikacji, należy utworzyć konto użytkownika o nazwie `ServiceUser` i hasła.</span><span class="sxs-lookup"><span data-stu-id="3017b-170">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="3017b-171">W poniższym poleceniu zastąp `{PASSWORD}` z [silne hasło](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="3017b-171">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

```console
net user ServiceUser {PASSWORD} /add
```

<span data-ttu-id="3017b-172">Jeśli potrzebujesz dodać użytkownika do grupy, użyj `net localgroup` polecenie, gdzie `{GROUP}` to nazwa grupy:</span><span class="sxs-lookup"><span data-stu-id="3017b-172">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

```console
net localgroup {GROUP} {USER ACCOUNT} /add
```

<span data-ttu-id="3017b-173">Aby uzyskać więcej informacji, zobacz [kont użytkowników usług](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="3017b-173">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="3017b-174">Innym sposobem zarządzania użytkownikami, podczas korzystania z usługi Active Directory jest użycie kont usług zarządzanych.</span><span class="sxs-lookup"><span data-stu-id="3017b-174">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="3017b-175">Aby uzyskać więcej informacji, zobacz [omówienie kont usług zarządzanych przez grupę](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="3017b-175">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

### <a name="set-permissions"></a><span data-ttu-id="3017b-176">Ustawianie uprawnień</span><span class="sxs-lookup"><span data-stu-id="3017b-176">Set permissions</span></span>

#### <a name="access-to-the-app-folder"></a><span data-ttu-id="3017b-177">Dostęp do folderu aplikacji</span><span class="sxs-lookup"><span data-stu-id="3017b-177">Access to the app folder</span></span>

<span data-ttu-id="3017b-178">Udzielanie zapisu/odczytu/wykonania dostępu do folderu aplikacji przy użyciu [icacls](/windows-server/administration/windows-commands/icacls) polecenia powłoki poleceń administracyjnych:</span><span class="sxs-lookup"><span data-stu-id="3017b-178">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command from an administrative command shell:</span></span>

```console
icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
```

* <span data-ttu-id="3017b-179">`{PATH}` &ndash; Ścieżka do folderu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3017b-179">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="3017b-180">`{USER ACCOUNT}` &ndash; Konto użytkownika (SID).</span><span class="sxs-lookup"><span data-stu-id="3017b-180">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="3017b-181">`(OI)` &ndash; Obiekt dziedziczenia flagi propaguje uprawnienia do podrzędnych plików.</span><span class="sxs-lookup"><span data-stu-id="3017b-181">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="3017b-182">`(CI)` &ndash; Flaga Dziedziczenie kontenera propaguje uprawnienia do folderów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="3017b-182">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="3017b-183">`{PERMISSION FLAGS}` &ndash; Ustawia uprawnienia dostępu do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3017b-183">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="3017b-184">Zapis (`W`)</span><span class="sxs-lookup"><span data-stu-id="3017b-184">Write (`W`)</span></span>
  * <span data-ttu-id="3017b-185">Odczyt (`R`)</span><span class="sxs-lookup"><span data-stu-id="3017b-185">Read (`R`)</span></span>
  * <span data-ttu-id="3017b-186">Wykonaj (`X`)</span><span class="sxs-lookup"><span data-stu-id="3017b-186">Execute (`X`)</span></span>
  * <span data-ttu-id="3017b-187">Pełne (`F`)</span><span class="sxs-lookup"><span data-stu-id="3017b-187">Full (`F`)</span></span>
  * <span data-ttu-id="3017b-188">Modyfikowanie (`M`)</span><span class="sxs-lookup"><span data-stu-id="3017b-188">Modify (`M`)</span></span>
* <span data-ttu-id="3017b-189">`/t` &ndash; Rekursywnie dotyczą plików i folderów podrzędnych istniejących.</span><span class="sxs-lookup"><span data-stu-id="3017b-189">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="3017b-190">Dla przykładowej aplikacji opublikowany *c:\\svc* folder i `ServiceUser` konto z uprawnieniami do zapisu/odczytu/wykonania, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="3017b-190">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

```console
icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
```

<span data-ttu-id="3017b-191">Aby uzyskać więcej informacji, zobacz [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="3017b-191">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

#### <a name="log-on-as-a-service"></a><span data-ttu-id="3017b-192">Zaloguj się jako usługa</span><span class="sxs-lookup"><span data-stu-id="3017b-192">Log on as a service</span></span>

<span data-ttu-id="3017b-193">Aby udzielić [Zaloguj się jako usługa](/windows/security/threat-protection/security-policy-settings/log-on-as-a-service) uprawnień do konta użytkownika:</span><span class="sxs-lookup"><span data-stu-id="3017b-193">To grant the [Log on as a service](/windows/security/threat-protection/security-policy-settings/log-on-as-a-service) privilege to the user account:</span></span>

1. <span data-ttu-id="3017b-194">Znajdź **Przypisywanie praw użytkownika** zasad w konsoli programu zasady zabezpieczeń lokalnych lub konsoli Edytor lokalnych zasad grupy.</span><span class="sxs-lookup"><span data-stu-id="3017b-194">Locate the **User Rights Assignment** policies in either the Local Security Policy console or Local Group Policy Editor console.</span></span> <span data-ttu-id="3017b-195">Instrukcje można znaleźć w tematach: [Konfigurowanie ustawień zasad zabezpieczeń](/windows/security/threat-protection/security-policy-settings/how-to-configure-security-policy-settings).</span><span class="sxs-lookup"><span data-stu-id="3017b-195">For instructions, see: [Configure security policy settings](/windows/security/threat-protection/security-policy-settings/how-to-configure-security-policy-settings).</span></span>
1. <span data-ttu-id="3017b-196">Znajdź `Log on as a service` zasad.</span><span class="sxs-lookup"><span data-stu-id="3017b-196">Locate the `Log on as a service` policy.</span></span> <span data-ttu-id="3017b-197">Kliknij dwukrotnie zasadę, aby go otworzyć.</span><span class="sxs-lookup"><span data-stu-id="3017b-197">Double-click the policy to open it.</span></span>
1. <span data-ttu-id="3017b-198">Wybierz **Dodaj użytkownika lub grupę**.</span><span class="sxs-lookup"><span data-stu-id="3017b-198">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="3017b-199">Wybierz **zaawansowane** i wybierz **Znajdź teraz**.</span><span class="sxs-lookup"><span data-stu-id="3017b-199">Select **Advanced** and select **Find Now**.</span></span>
1. <span data-ttu-id="3017b-200">Wybierz konto użytkownika utworzone w [Utwórz konto użytkownika](#create-a-user-account) wcześniejszej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3017b-200">Select the user account created in the [Create a user account](#create-a-user-account) section earlier.</span></span> <span data-ttu-id="3017b-201">Wybierz **OK** aby zaakceptować wybór.</span><span class="sxs-lookup"><span data-stu-id="3017b-201">Select **OK** to accept the selection.</span></span>
1. <span data-ttu-id="3017b-202">Wybierz **OK** po potwierdzeniu, że nazwa obiektu jest poprawna.</span><span class="sxs-lookup"><span data-stu-id="3017b-202">Select **OK** after confirming that the object name is correct.</span></span>
1. <span data-ttu-id="3017b-203">Wybierz przycisk **Zastosuj**.</span><span class="sxs-lookup"><span data-stu-id="3017b-203">Select **Apply**.</span></span> <span data-ttu-id="3017b-204">Wybierz **OK** aby zamknąć okno zasady.</span><span class="sxs-lookup"><span data-stu-id="3017b-204">Select **OK** to close the policy window.</span></span>

## <a name="manage-the-service"></a><span data-ttu-id="3017b-205">Zarządzanie usługą</span><span class="sxs-lookup"><span data-stu-id="3017b-205">Manage the service</span></span>

### <a name="create-the-service"></a><span data-ttu-id="3017b-206">Tworzenie usługi</span><span class="sxs-lookup"><span data-stu-id="3017b-206">Create the service</span></span>

<span data-ttu-id="3017b-207">Użyj [sc.exe](https://technet.microsoft.com/library/bb490995) narzędzie wiersza polecenia, aby utworzyć usługę z powłoki poleceń administracyjnych.</span><span class="sxs-lookup"><span data-stu-id="3017b-207">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service from an administrative command shell.</span></span> <span data-ttu-id="3017b-208">`binPath` Wartość jest ścieżką do pliku wykonywalnego aplikacji, która zawiera nazwę pliku wykonywalnego.</span><span class="sxs-lookup"><span data-stu-id="3017b-208">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="3017b-209">**Odstęp między równości i znaku cudzysłowu każdego parametru i wartość jest wymagana.**</span><span class="sxs-lookup"><span data-stu-id="3017b-209">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

```console
sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
```

* <span data-ttu-id="3017b-210">`{SERVICE NAME}` &ndash; Nazwa do przypisania do usługi w [Menedżera sterowania usługami](/windows/desktop/services/service-control-manager).</span><span class="sxs-lookup"><span data-stu-id="3017b-210">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
* <span data-ttu-id="3017b-211">`{PATH}` &ndash; Ścieżka do pliku wykonywalnego usługi.</span><span class="sxs-lookup"><span data-stu-id="3017b-211">`{PATH}` &ndash; The path to the service executable.</span></span>
* <span data-ttu-id="3017b-212">`{DOMAIN}` &ndash; Domena komputerze przyłączonym do domeny.</span><span class="sxs-lookup"><span data-stu-id="3017b-212">`{DOMAIN}` &ndash; The domain of a domain-joined machine.</span></span> <span data-ttu-id="3017b-213">Jeśli komputer nie jest, przyłączone do domeny, należy użyć nazwy komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="3017b-213">If the machine isn't domain-joined, use the local machine name.</span></span>
* <span data-ttu-id="3017b-214">`{USER ACCOUNT}` &ndash; Konto użytkownika, pod którym działa usługa.</span><span class="sxs-lookup"><span data-stu-id="3017b-214">`{USER ACCOUNT}` &ndash; The user account under which the service runs.</span></span>
* <span data-ttu-id="3017b-215">`{PASSWORD}` &ndash; Hasło konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3017b-215">`{PASSWORD}` &ndash; The user account password.</span></span>

> [!WARNING]
> <span data-ttu-id="3017b-216">Czy **nie** pominąć `obj` parametru.</span><span class="sxs-lookup"><span data-stu-id="3017b-216">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="3017b-217">Wartością domyślną dla `obj` jest [konta LocalSystem](/windows/desktop/services/localsystem-account) konta.</span><span class="sxs-lookup"><span data-stu-id="3017b-217">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="3017b-218">Uruchamianie usługi w obszarze `LocalSystem` konto stanowi znaczące zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="3017b-218">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="3017b-219">Usługi są zawsze uruchamiane przy użyciu konta użytkownika, które ma ograniczone uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="3017b-219">Always run a service with a user account that has restricted privileges.</span></span>

<span data-ttu-id="3017b-220">W poniższym przykładzie przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3017b-220">In the following example for the sample app:</span></span>

* <span data-ttu-id="3017b-221">Usługa jest o nazwie **Moja_usługa**.</span><span class="sxs-lookup"><span data-stu-id="3017b-221">The service is named **MyService**.</span></span>
* <span data-ttu-id="3017b-222">Opublikowana usługa znajduje się w *c:\\svc* folderu.</span><span class="sxs-lookup"><span data-stu-id="3017b-222">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="3017b-223">Nosi nazwę pliku wykonywalnego aplikacji *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="3017b-223">The app executable is named *SampleApp.exe*.</span></span> <span data-ttu-id="3017b-224">Ujmij `binPath` wartość w znaki cudzysłowu (").</span><span class="sxs-lookup"><span data-stu-id="3017b-224">Enclose the `binPath` value in double quotation marks (").</span></span>
* <span data-ttu-id="3017b-225">Usługa jest uruchamiana w ramach `ServiceUser` konta.</span><span class="sxs-lookup"><span data-stu-id="3017b-225">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="3017b-226">Zastąp `{DOMAIN}` przy użyciu konta użytkownika domeny lub nazwy komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="3017b-226">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="3017b-227">Ujmij `obj` wartość w znaki cudzysłowu (").</span><span class="sxs-lookup"><span data-stu-id="3017b-227">Enclose the `obj` value in double quotation marks (").</span></span> <span data-ttu-id="3017b-228">Przykład: W przypadku hostowania systemu komputera lokalnego, o nazwie `MairaPC`ustaw `obj` do `"MairaPC\ServiceUser"`.</span><span class="sxs-lookup"><span data-stu-id="3017b-228">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
* <span data-ttu-id="3017b-229">Zastąp `{PASSWORD}` przy użyciu hasła konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3017b-229">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="3017b-230">Ujmij `password` wartość w znaki cudzysłowu (").</span><span class="sxs-lookup"><span data-stu-id="3017b-230">Enclose the `password` value in double quotation marks (").</span></span>

```console
sc create MyService binPath= "c:\svc\sampleapp.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
```

> [!IMPORTANT]
> <span data-ttu-id="3017b-231">Upewnij się, że istnieją spacji między znakami równości parametrów i wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="3017b-231">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

### <a name="start-the-service"></a><span data-ttu-id="3017b-232">Uruchom usługę</span><span class="sxs-lookup"><span data-stu-id="3017b-232">Start the service</span></span>

<span data-ttu-id="3017b-233">Uruchom usługę za pomocą `sc start {SERVICE NAME}` polecenia.</span><span class="sxs-lookup"><span data-stu-id="3017b-233">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

<span data-ttu-id="3017b-234">Aby uruchomić usługę aplikacji przykładowej, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="3017b-234">To start the sample app service, use the following command:</span></span>

```console
sc start MyService
```

<span data-ttu-id="3017b-235">Polecenie zajmuje kilka sekund, aby uruchomić usługę.</span><span class="sxs-lookup"><span data-stu-id="3017b-235">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="3017b-236">Sprawdź stan usługi</span><span class="sxs-lookup"><span data-stu-id="3017b-236">Determine the service status</span></span>

<span data-ttu-id="3017b-237">Aby sprawdzić stan usługi, użyj `sc query {SERVICE NAME}` polecenia.</span><span class="sxs-lookup"><span data-stu-id="3017b-237">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="3017b-238">Stan jest zgłaszany jako jeden z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="3017b-238">The status is reported as one of the following values:</span></span>

* `START_PENDING`
* `RUNNING`
* `STOP_PENDING`
* `STOPPED`

<span data-ttu-id="3017b-239">Użyj następującego polecenia, aby sprawdzić stan usługi aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="3017b-239">Use the following command to check the status of the sample app service:</span></span>

```console
sc query MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="3017b-240">Przeglądaj, usługi aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="3017b-240">Browse a web app service</span></span>

<span data-ttu-id="3017b-241">Kiedy usługa jest w `RUNNING` stanu i usługi w przypadku aplikacji sieci web, przeglądanie aplikacji w ścieżce (domyślnie `http://localhost:5000`, który przekierowuje do `https://localhost:5001` przy użyciu [HTTPS przekierowanie w oprogramowaniu pośredniczącym](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="3017b-241">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="3017b-242">Usługa app service przykładowego, można przeglądać w tej aplikacji w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="3017b-242">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="3017b-243">Zatrzymaj usługę</span><span class="sxs-lookup"><span data-stu-id="3017b-243">Stop the service</span></span>

<span data-ttu-id="3017b-244">Zatrzymaj usługę za pomocą `sc stop {SERVICE NAME}` polecenia.</span><span class="sxs-lookup"><span data-stu-id="3017b-244">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

<span data-ttu-id="3017b-245">Następujące polecenie zatrzymuje usługę aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="3017b-245">The following command stops the sample app service:</span></span>

```console
sc stop MyService
```

### <a name="delete-the-service"></a><span data-ttu-id="3017b-246">Usuń usługę</span><span class="sxs-lookup"><span data-stu-id="3017b-246">Delete the service</span></span>

<span data-ttu-id="3017b-247">Po krótkiej chwili zatrzymania usługi, odinstaluj usługę za pomocą `sc delete {SERVICE NAME}` polecenia.</span><span class="sxs-lookup"><span data-stu-id="3017b-247">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

<span data-ttu-id="3017b-248">Sprawdź stan usługi aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="3017b-248">Check the status of the sample app service:</span></span>

```console
sc query MyService
```

<span data-ttu-id="3017b-249">Gdy usługa app service przykładowego jest w `STOPPED` stanu, użyj następującego polecenia, aby odinstalować usługę aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="3017b-249">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

```console
sc delete MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="3017b-250">Obsługa uruchamianie i zatrzymywanie wydarzeń</span><span class="sxs-lookup"><span data-stu-id="3017b-250">Handle starting and stopping events</span></span>

<span data-ttu-id="3017b-251">Aby obsłużyć <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, i <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> zdarzeń, wykonaj następujące dodatkowe zmiany:</span><span class="sxs-lookup"><span data-stu-id="3017b-251">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="3017b-252">Utwórz klasę, która pochodzi od klasy <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> z `OnStarting`, `OnStarted`, i `OnStopping` metody:</span><span class="sxs-lookup"><span data-stu-id="3017b-252">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="3017b-253">Tworzenie metody rozszerzenia dla <xref:Microsoft.AspNetCore.Hosting.IWebHost> , który przekazuje `CustomWebHostService` do <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="3017b-253">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="3017b-254">W `Program.Main`, wywołaj `RunAsCustomService` rozszerzenia zamiast metody <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="3017b-254">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="3017b-255">Aby wyświetlić lokalizację <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> w `Program.Main`, można znaleźć w przykładzie kodu pokazano w [konwertować projekt w usłudze Windows](#convert-a-project-into-a-windows-service) sekcji.</span><span class="sxs-lookup"><span data-stu-id="3017b-255">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="3017b-256">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="3017b-256">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="3017b-257">Usługi wchodzić w interakcje z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub moduł równoważenia obciążenia, które mogą wymagać dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="3017b-257">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="3017b-258">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="3017b-258">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="3017b-259">Konfigurowanie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="3017b-259">Configure HTTPS</span></span>

<span data-ttu-id="3017b-260">Aby skonfigurować usługę z bezpiecznego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="3017b-260">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="3017b-261">Utwórz certyfikat X.509 dla systemu macierzystego przy użyciu pozyskiwania certyfikatu używanej platformy i mechanizmy wdrażania.</span><span class="sxs-lookup"><span data-stu-id="3017b-261">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="3017b-262">Określ [konfiguracji punktu końcowego HTTPS serwera Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) przy użyciu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="3017b-262">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="3017b-263">Użycie certyfikatu deweloperskiego platformy ASP.NET Core HTTPS, aby zabezpieczyć punkt końcowy usługi nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="3017b-263">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="3017b-264">Bieżący katalog i katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="3017b-264">Current directory and content root</span></span>

<span data-ttu-id="3017b-265">Bieżący katalog roboczy zwracany przez wywołanie metody <xref:System.IO.Directory.GetCurrentDirectory*> dla usługi Windows jest *C:\\WINDOWS\\system32* folderu.</span><span class="sxs-lookup"><span data-stu-id="3017b-265">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="3017b-266">*System32* folder nie jest odpowiednią lokalizację do przechowywania plików usługi (na przykład pliki ustawień).</span><span class="sxs-lookup"><span data-stu-id="3017b-266">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="3017b-267">Użyj jednej z następujących metod do utrzymywania i uzyskać dostęp do zasobów i plików ustawień usługi.</span><span class="sxs-lookup"><span data-stu-id="3017b-267">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="3017b-268">Ustaw ścieżkę katalogu głównego zawartości do folderu aplikacji</span><span class="sxs-lookup"><span data-stu-id="3017b-268">Set the content root path to the app's folder</span></span>

<span data-ttu-id="3017b-269"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> Tej samej ścieżki, które są udostępniane `binPath` argumentów podczas tworzenia usługi.</span><span class="sxs-lookup"><span data-stu-id="3017b-269">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="3017b-270">Zamiast wywoływać metodę `GetCurrentDirectory` do tworzenia ścieżek do plików ustawień, należy wywołać <xref:System.IO.Directory.SetCurrentDirectory*> ze ścieżką do katalogu głównego zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3017b-270">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="3017b-271">W `Program.Main`, określić ścieżkę do folderu pliku wykonywalnego usługi i ustanowić zawartości katalogu głównego aplikacji za pomocą ścieżki:</span><span class="sxs-lookup"><span data-stu-id="3017b-271">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="3017b-272">Store usługi plików w odpowiedniej lokalizacji na dysku</span><span class="sxs-lookup"><span data-stu-id="3017b-272">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="3017b-273">Określ ścieżkę bezwzględną z <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> przy użyciu <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do folderu zawierającego pliki.</span><span class="sxs-lookup"><span data-stu-id="3017b-273">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3017b-274">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3017b-274">Additional resources</span></span>

* <span data-ttu-id="3017b-275">[Konfiguracja punktu końcowego kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym konfiguracja protokołu HTTPS i obsługa SNI)</span><span class="sxs-lookup"><span data-stu-id="3017b-275">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
