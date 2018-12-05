---
title: Host platformy ASP.NET Core w usłudze Windows
author: guardrex
description: Dowiedz się, jak udostępnić aplikację ASP.NET Core w usłudze Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/01/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: f53c303dc63e092f08e933fea79eb805523cde9b
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861397"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="d50c0-103">Host platformy ASP.NET Core w usłudze Windows</span><span class="sxs-lookup"><span data-stu-id="d50c0-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="d50c0-104">Przez [Luke Latham](https://github.com/guardrex) i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d50c0-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d50c0-105">Aplikacji ASP.NET Core może być hostowana na Windows jako [usługi Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) bez korzystania z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="d50c0-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="d50c0-106">Po hostowany jako usługa Windows, aplikacja zostanie automatycznie uruchomiona po ponownym uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="d50c0-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="d50c0-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d50c0-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="deployment-type"></a><span data-ttu-id="d50c0-108">Typ wdrożenia</span><span class="sxs-lookup"><span data-stu-id="d50c0-108">Deployment type</span></span>

<span data-ttu-id="d50c0-109">Można utworzyć albo zależny od struktury lub niezależna Windows wdrożenia usługi.</span><span class="sxs-lookup"><span data-stu-id="d50c0-109">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="d50c0-110">Aby uzyskać informacje i wskazówki dotyczące scenariuszy wdrażania, zobacz [wdrażanie aplikacji .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="d50c0-110">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="d50c0-111">Wdrożenie zależny od struktury</span><span class="sxs-lookup"><span data-stu-id="d50c0-111">Framework-dependent deployment</span></span>

<span data-ttu-id="d50c0-112">Zależny od struktury wdrożenia (stacje) zależy od obecności udostępnione całego systemu w wersji programu .NET Core w systemie docelowym.</span><span class="sxs-lookup"><span data-stu-id="d50c0-112">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="d50c0-113">Stosowania w scenariuszu z stacje za pomocą aplikacji platformy ASP.NET Core Windows Service SDK tworzy plik wykonywalny (*\*.exe*), co jest nazywane *zależny od struktury pliku wykonywalnego*.</span><span class="sxs-lookup"><span data-stu-id="d50c0-113">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="d50c0-114">Niezależne wdrożenia</span><span class="sxs-lookup"><span data-stu-id="d50c0-114">Self-contained deployment</span></span>

<span data-ttu-id="d50c0-115">Niezależne wdrożenia (— SCD) nie jest zależny od obecności składniki współużytkowane w systemie docelowym.</span><span class="sxs-lookup"><span data-stu-id="d50c0-115">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="d50c0-116">Środowisko uruchomieniowe i zależności aplikacji są wdrażane za pomocą aplikacji do systemu macierzystego.</span><span class="sxs-lookup"><span data-stu-id="d50c0-116">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="d50c0-117">Konwertuj projekt w usłudze Windows</span><span class="sxs-lookup"><span data-stu-id="d50c0-117">Convert a project into a Windows Service</span></span>

<span data-ttu-id="d50c0-118">Do istniejącego projektu platformy ASP.NET Core do uruchomienia aplikacji jako usługi, należy wprowadzić następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="d50c0-118">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="d50c0-119">Aktualizacje plików projektu</span><span class="sxs-lookup"><span data-stu-id="d50c0-119">Project file updates</span></span>

<span data-ttu-id="d50c0-120">Na podstawie wybranego elementu [typu wdrożenia](#deployment-type), należy zaktualizować plik projektu:</span><span class="sxs-lookup"><span data-stu-id="d50c0-120">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="d50c0-121">Zależny od struktury wdrożenia (stacje)</span><span class="sxs-lookup"><span data-stu-id="d50c0-121">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="d50c0-122">Dodaj Windows [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) do `<PropertyGroup>` zawierający platformę docelową.</span><span class="sxs-lookup"><span data-stu-id="d50c0-122">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="d50c0-123">Dodaj `<SelfContained>` właściwością `false`.</span><span class="sxs-lookup"><span data-stu-id="d50c0-123">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="d50c0-124">Wyłączyć tworzenie *web.config* pliku, dodając `<IsTransformWebConfigDisabled>` właściwością `true`.</span><span class="sxs-lookup"><span data-stu-id="d50c0-124">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="d50c0-125">Niezależne wdrożenia (— SCD)</span><span class="sxs-lookup"><span data-stu-id="d50c0-125">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="d50c0-126">Potwierdzić obecność Windows [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) lub identyfikatorów RID, aby dodać `<PropertyGroup>` zawierający platformę docelową.</span><span class="sxs-lookup"><span data-stu-id="d50c0-126">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="d50c0-127">Wyłączyć tworzenie *web.config* pliku, dodając `<IsTransformWebConfigDisabled>` właściwością `true`.</span><span class="sxs-lookup"><span data-stu-id="d50c0-127">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="d50c0-128">Aby opublikować dla wielu identyfikatorów RID:</span><span class="sxs-lookup"><span data-stu-id="d50c0-128">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="d50c0-129">Podaj identyfikatorów RID w liście rozdzielanej średnikami.</span><span class="sxs-lookup"><span data-stu-id="d50c0-129">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="d50c0-130">Użyj nazwy właściwości `<RuntimeIdentifiers>` (w liczbie mnogiej).</span><span class="sxs-lookup"><span data-stu-id="d50c0-130">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="d50c0-131">Aby uzyskać więcej informacji, zobacz [.NET Core RID katalogu](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="d50c0-131">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="d50c0-132">Dodaj odwołania do pakietu dla [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="d50c0-132">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="d50c0-133">Aby włączyć rejestrowanie w dzienniku zdarzeń Windows, należy dodać odwołania do pakietu dla [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="d50c0-133">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="d50c0-134">Aby uzyskać więcej informacji, zobacz [obsługi, uruchamianie i zatrzymywanie wydarzeń](#handle-starting-and-stopping-events) sekcji.</span><span class="sxs-lookup"><span data-stu-id="d50c0-134">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="d50c0-135">Aktualizacje Program.Main</span><span class="sxs-lookup"><span data-stu-id="d50c0-135">Program.Main updates</span></span>

<span data-ttu-id="d50c0-136">Wprowadź następujące zmiany w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="d50c0-136">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="d50c0-137">Do testowania i debugowania, gdy działają poza usługą, Dodaj kod, aby ustalić, czy aplikacja jest uruchomiona jako usługę lub aplikację konsoli.</span><span class="sxs-lookup"><span data-stu-id="d50c0-137">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="d50c0-138">Sprawdź, czy jest dołączony debuger a `--console` argument wiersza polecenia jest obecny.</span><span class="sxs-lookup"><span data-stu-id="d50c0-138">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="d50c0-139">Jeśli tych warunków jest spełniony, (aplikacja nie jest uruchamiana jako usługa), należy wywołać <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> na hoście w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d50c0-139">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="d50c0-140">Jeśli warunki są fałszywe, (aplikacja jest uruchamiana jako usługa):</span><span class="sxs-lookup"><span data-stu-id="d50c0-140">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="d50c0-141">Wywołaj <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> i użyj ścieżki do lokalizacji publikowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d50c0-141">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> and use a path to the app's published location.</span></span> <span data-ttu-id="d50c0-142">Nie wywołuj <xref:System.IO.Directory.GetCurrentDirectory*> można uzyskać ścieżki, ponieważ aplikacji usługi Windows, która zwraca *C:\\WINDOWS\\system32* folderu podczas `GetCurrentDirectory` nosi nazwę.</span><span class="sxs-lookup"><span data-stu-id="d50c0-142">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when `GetCurrentDirectory` is called.</span></span> <span data-ttu-id="d50c0-143">Aby uzyskać więcej informacji, zobacz [bieżący katalog i katalog główny zawartości](#current-directory-and-content-root) sekcji.</span><span class="sxs-lookup"><span data-stu-id="d50c0-143">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="d50c0-144">Wywołaj <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> do uruchomienia aplikacji jako usługi.</span><span class="sxs-lookup"><span data-stu-id="d50c0-144">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="d50c0-145">Ponieważ [dostawcę konfiguracji wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) wymaga pary nazwa wartość dla argumentów wiersza poleceń `--console` przełącznik został usunięty z argumentów przed <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> otrzymuje je.</span><span class="sxs-lookup"><span data-stu-id="d50c0-145">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="d50c0-146">Można zapisać w dzienniku zdarzeń Windows, należy dodać dostawcę dziennika zdarzeń <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="d50c0-146">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="d50c0-147">Poziom rejestrowania za pomocą `Logging:LogLevel:Default` w *appsettings. Production.JSON* pliku.</span><span class="sxs-lookup"><span data-stu-id="d50c0-147">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="d50c0-148">Pokaz i testowania pliku ustawień produkcji przykładową aplikację ustawia poziom rejestrowania `Information`.</span><span class="sxs-lookup"><span data-stu-id="d50c0-148">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="d50c0-149">W środowisku produkcyjnym, wartość jest zazwyczaj równa `Error`.</span><span class="sxs-lookup"><span data-stu-id="d50c0-149">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="d50c0-150">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="d50c0-150">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

### <a name="publish-the-app"></a><span data-ttu-id="d50c0-151">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d50c0-151">Publish the app</span></span>

<span data-ttu-id="d50c0-152">Publikowanie aplikacji za pomocą [publikowania dotnet](/dotnet/articles/core/tools/dotnet-publish), [profilu publikowania w programie Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles), lub Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d50c0-152">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="d50c0-153">Jeśli używasz programu Visual Studio, wybierz opcję **FolderProfile** i skonfigurować **lokalizacji docelowej** przed wybraniem **Publikuj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="d50c0-153">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="d50c0-154">Aby opublikować przykładową aplikację przy użyciu narzędzi interfejsu wiersza polecenia (CLI), uruchom [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie w wierszu polecenia z folderu projektu z konfiguracją wydania, przekazana do [- c |--konfiguracji](/dotnet/core/tools/dotnet-publish#options)opcji.</span><span class="sxs-lookup"><span data-stu-id="d50c0-154">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="d50c0-155">Użyj [-o |--dane wyjściowe](/dotnet/core/tools/dotnet-publish#options) opcji ze ścieżką do publikowania do folderu poza aplikacją.</span><span class="sxs-lookup"><span data-stu-id="d50c0-155">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

#### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="d50c0-156">Publikowanie wdrożenia zależny od struktury (stacje)</span><span class="sxs-lookup"><span data-stu-id="d50c0-156">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="d50c0-157">W poniższym przykładzie aplikacja została opublikowana do *c:\\svc* folderu:</span><span class="sxs-lookup"><span data-stu-id="d50c0-157">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

#### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="d50c0-158">Publikowanie niezależne wdrożenia (— SCD)</span><span class="sxs-lookup"><span data-stu-id="d50c0-158">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="d50c0-159">Identyfikator RID musi być określona w `<RuntimeIdenfifier>` (lub `<RuntimeIdentifiers>`) właściwości pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="d50c0-159">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="d50c0-160">Podaj środowisko uruchomieniowe [- r | — środowisko uruchomieniowe](/dotnet/core/tools/dotnet-publish#options) opcji `dotnet publish` polecenia.</span><span class="sxs-lookup"><span data-stu-id="d50c0-160">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="d50c0-161">W poniższym przykładzie aplikacja została opublikowana na potrzeby `win7-x64` środowiska uruchomieniowego *c:\\svc* folderu:</span><span class="sxs-lookup"><span data-stu-id="d50c0-161">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

### <a name="create-a-user-account"></a><span data-ttu-id="d50c0-162">Utwórz konto użytkownika</span><span class="sxs-lookup"><span data-stu-id="d50c0-162">Create a user account</span></span>

<span data-ttu-id="d50c0-163">Utwórz konto użytkownika dla usługi przy użyciu `net user` polecenia:</span><span class="sxs-lookup"><span data-stu-id="d50c0-163">Create a user account for the service using the `net user` command:</span></span>

```console
net user {USER ACCOUNT} {PASSWORD} /add
```

<span data-ttu-id="d50c0-164">Dla przykładowej aplikacji, należy utworzyć konto użytkownika o nazwie `ServiceUser` i hasła.</span><span class="sxs-lookup"><span data-stu-id="d50c0-164">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="d50c0-165">W poniższym poleceniu zastąp `{PASSWORD}` z [silne hasło](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="d50c0-165">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

```console
net user ServiceUser {PASSWORD} /add
```

<span data-ttu-id="d50c0-166">Jeśli potrzebujesz dodać użytkownika do grupy, użyj `net localgroup` polecenie, gdzie `{GROUP}` to nazwa grupy:</span><span class="sxs-lookup"><span data-stu-id="d50c0-166">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

```console
net localgroup {GROUP} {USER ACCOUNT} /add
```

<span data-ttu-id="d50c0-167">Aby uzyskać więcej informacji, zobacz [kont użytkowników usług](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="d50c0-167">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

### <a name="set-permissions"></a><span data-ttu-id="d50c0-168">Ustawianie uprawnień</span><span class="sxs-lookup"><span data-stu-id="d50c0-168">Set permissions</span></span>

<span data-ttu-id="d50c0-169">Udzielanie zapisu/odczytu/wykonania dostępu do folderu aplikacji przy użyciu [icacls](/windows-server/administration/windows-commands/icacls) polecenia:</span><span class="sxs-lookup"><span data-stu-id="d50c0-169">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

```console
icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
```

* <span data-ttu-id="d50c0-170">`{PATH}` &ndash; Ścieżka do folderu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d50c0-170">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="d50c0-171">`{USER ACCOUNT}` &ndash; Konto użytkownika (SID).</span><span class="sxs-lookup"><span data-stu-id="d50c0-171">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="d50c0-172">`(OI)` &ndash; Obiekt dziedziczenia flagi propaguje uprawnienia do podrzędnych plików.</span><span class="sxs-lookup"><span data-stu-id="d50c0-172">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="d50c0-173">`(CI)` &ndash; Flaga Dziedziczenie kontenera propaguje uprawnienia do folderów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="d50c0-173">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="d50c0-174">`{PERMISSION FLAGS}` &ndash; Ustawia uprawnienia dostępu do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d50c0-174">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="d50c0-175">Zapis (`W`)</span><span class="sxs-lookup"><span data-stu-id="d50c0-175">Write (`W`)</span></span>
  * <span data-ttu-id="d50c0-176">Odczyt (`R`)</span><span class="sxs-lookup"><span data-stu-id="d50c0-176">Read (`R`)</span></span>
  * <span data-ttu-id="d50c0-177">Wykonaj (`X`)</span><span class="sxs-lookup"><span data-stu-id="d50c0-177">Execute (`X`)</span></span>
  * <span data-ttu-id="d50c0-178">Pełne (`F`)</span><span class="sxs-lookup"><span data-stu-id="d50c0-178">Full (`F`)</span></span>
  * <span data-ttu-id="d50c0-179">Modyfikowanie (`M`)</span><span class="sxs-lookup"><span data-stu-id="d50c0-179">Modify (`M`)</span></span>
* <span data-ttu-id="d50c0-180">`/t` &ndash; Rekursywnie dotyczą plików i folderów podrzędnych istniejących.</span><span class="sxs-lookup"><span data-stu-id="d50c0-180">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="d50c0-181">Dla przykładowej aplikacji opublikowany *c:\\svc* folder i `ServiceUser` konto z uprawnieniami do zapisu/odczytu/wykonania, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="d50c0-181">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

```console
icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
```

<span data-ttu-id="d50c0-182">Aby uzyskać więcej informacji, zobacz [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="d50c0-182">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

## <a name="manage-the-service"></a><span data-ttu-id="d50c0-183">Zarządzanie usługą</span><span class="sxs-lookup"><span data-stu-id="d50c0-183">Manage the service</span></span>

### <a name="create-the-service"></a><span data-ttu-id="d50c0-184">Tworzenie usługi</span><span class="sxs-lookup"><span data-stu-id="d50c0-184">Create the service</span></span>

<span data-ttu-id="d50c0-185">Użyj [sc.exe](https://technet.microsoft.com/library/bb490995) narzędzie wiersza polecenia, aby utworzyć usługę.</span><span class="sxs-lookup"><span data-stu-id="d50c0-185">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="d50c0-186">`binPath` Wartość jest ścieżką do pliku wykonywalnego aplikacji, która zawiera nazwę pliku wykonywalnego.</span><span class="sxs-lookup"><span data-stu-id="d50c0-186">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="d50c0-187">**Odstęp między równości i znaku cudzysłowu każdego parametru i wartość jest wymagana.**</span><span class="sxs-lookup"><span data-stu-id="d50c0-187">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

```console
sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
```

* <span data-ttu-id="d50c0-188">`{SERVICE NAME}` &ndash; Nazwa do przypisania do usługi w [Menedżera sterowania usługami](/windows/desktop/services/service-control-manager).</span><span class="sxs-lookup"><span data-stu-id="d50c0-188">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
* <span data-ttu-id="d50c0-189">`{PATH}` &ndash; Ścieżka do pliku wykonywalnego usługi.</span><span class="sxs-lookup"><span data-stu-id="d50c0-189">`{PATH}` &ndash; The path to the service executable.</span></span>
* <span data-ttu-id="d50c0-190">`{DOMAIN}` &ndash; Domena komputerze przyłączonym do domeny.</span><span class="sxs-lookup"><span data-stu-id="d50c0-190">`{DOMAIN}` &ndash; The domain of a domain-joined machine.</span></span> <span data-ttu-id="d50c0-191">Jeśli komputer nie jest przyłączone do domeny, nazwa komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="d50c0-191">If the machine isn't domain-joined, the local machine name.</span></span>
* <span data-ttu-id="d50c0-192">`{USER ACCOUNT}` &ndash; Konto użytkownika, pod którym działa usługa.</span><span class="sxs-lookup"><span data-stu-id="d50c0-192">`{USER ACCOUNT}` &ndash; The user account under which the service runs.</span></span>
* <span data-ttu-id="d50c0-193">`{PASSWORD}` &ndash; Hasło konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d50c0-193">`{PASSWORD}` &ndash; The user account password.</span></span>

> [!WARNING]
> <span data-ttu-id="d50c0-194">Czy **nie** pominąć `obj` parametru.</span><span class="sxs-lookup"><span data-stu-id="d50c0-194">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="d50c0-195">Wartością domyślną dla `obj` jest [konta LocalSystem](/windows/desktop/services/localsystem-account) konta.</span><span class="sxs-lookup"><span data-stu-id="d50c0-195">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="d50c0-196">Uruchamianie usługi w obszarze `LocalSystem` konto stanowi znaczące zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="d50c0-196">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="d50c0-197">Usługi są zawsze uruchamiane przy użyciu konta użytkownika, które ma ograniczone uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="d50c0-197">Always run a service with a user account that has restricted privileges.</span></span>

<span data-ttu-id="d50c0-198">W poniższym przykładzie przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d50c0-198">In the following example for the sample app:</span></span>

* <span data-ttu-id="d50c0-199">Usługa jest o nazwie **Moja_usługa**.</span><span class="sxs-lookup"><span data-stu-id="d50c0-199">The service is named **MyService**.</span></span>
* <span data-ttu-id="d50c0-200">Opublikowana usługa znajduje się w *c:\\svc* folderu.</span><span class="sxs-lookup"><span data-stu-id="d50c0-200">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="d50c0-201">Nosi nazwę pliku wykonywalnego aplikacji *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="d50c0-201">The app executable is named *SampleApp.exe*.</span></span> <span data-ttu-id="d50c0-202">Ujmij `binPath` wartość w znaki cudzysłowu (").</span><span class="sxs-lookup"><span data-stu-id="d50c0-202">Enclose the `binPath` value in double quotation marks (").</span></span>
* <span data-ttu-id="d50c0-203">Usługa jest uruchamiana w ramach `ServiceUser` konta.</span><span class="sxs-lookup"><span data-stu-id="d50c0-203">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="d50c0-204">Zastąp `{DOMAIN}` przy użyciu konta użytkownika domeny lub nazwy komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="d50c0-204">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="d50c0-205">Ujmij `obj` wartość w znaki cudzysłowu (").</span><span class="sxs-lookup"><span data-stu-id="d50c0-205">Enclose the `obj` value in double quotation marks (").</span></span> <span data-ttu-id="d50c0-206">Przykład: W przypadku hostowania systemu komputera lokalnego, o nazwie `MairaPC`ustaw `obj` do `"MairaPC\ServiceUser"`.</span><span class="sxs-lookup"><span data-stu-id="d50c0-206">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
* <span data-ttu-id="d50c0-207">Zastąp `{PASSWORD}` przy użyciu hasła konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d50c0-207">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="d50c0-208">Ujmij `password` wartość w znaki cudzysłowu (").</span><span class="sxs-lookup"><span data-stu-id="d50c0-208">Enclose the `password` value in double quotation marks (").</span></span>

```console
sc create MyService binPath= "c:\svc\sampleapp.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
```

> [!IMPORTANT]
> <span data-ttu-id="d50c0-209">Upewnij się, że istnieją spacji między znakami równości parametrów i wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="d50c0-209">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

### <a name="start-the-service"></a><span data-ttu-id="d50c0-210">Uruchom usługę</span><span class="sxs-lookup"><span data-stu-id="d50c0-210">Start the service</span></span>

<span data-ttu-id="d50c0-211">Uruchom usługę za pomocą `sc start {SERVICE NAME}` polecenia.</span><span class="sxs-lookup"><span data-stu-id="d50c0-211">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

<span data-ttu-id="d50c0-212">Aby uruchomić usługę aplikacji przykładowej, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="d50c0-212">To start the sample app service, use the following command:</span></span>

```console
sc start MyService
```

<span data-ttu-id="d50c0-213">Polecenie zajmuje kilka sekund, aby uruchomić usługę.</span><span class="sxs-lookup"><span data-stu-id="d50c0-213">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="d50c0-214">Sprawdź stan usługi</span><span class="sxs-lookup"><span data-stu-id="d50c0-214">Determine the service status</span></span>

<span data-ttu-id="d50c0-215">Aby sprawdzić stan usługi, użyj `sc query {SERVICE NAME}` polecenia.</span><span class="sxs-lookup"><span data-stu-id="d50c0-215">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="d50c0-216">Stan jest zgłaszany jako jeden z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="d50c0-216">The status is reported as one of the following values:</span></span>

* `START_PENDING`
* `RUNNING`
* `STOP_PENDING`
* `STOPPED`

<span data-ttu-id="d50c0-217">Użyj następującego polecenia, aby sprawdzić stan usługi aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="d50c0-217">Use the following command to check the status of the sample app service:</span></span>

```console
sc query MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="d50c0-218">Przeglądaj, usługi aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="d50c0-218">Browse a web app service</span></span>

<span data-ttu-id="d50c0-219">Kiedy usługa jest w `RUNNING` stanu i usługi w przypadku aplikacji sieci web, przeglądanie aplikacji w ścieżce (domyślnie `http://localhost:5000`, który przekierowuje do `https://localhost:5001` przy użyciu [HTTPS przekierowanie w oprogramowaniu pośredniczącym](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="d50c0-219">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="d50c0-220">Usługa app service przykładowego, można przeglądać w tej aplikacji w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d50c0-220">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="d50c0-221">Zatrzymaj usługę</span><span class="sxs-lookup"><span data-stu-id="d50c0-221">Stop the service</span></span>

<span data-ttu-id="d50c0-222">Zatrzymaj usługę za pomocą `sc stop {SERVICE NAME}` polecenia.</span><span class="sxs-lookup"><span data-stu-id="d50c0-222">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

<span data-ttu-id="d50c0-223">Następujące polecenie zatrzymuje usługę aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="d50c0-223">The following command stops the sample app service:</span></span>

```console
sc stop MyService
```

### <a name="delete-the-service"></a><span data-ttu-id="d50c0-224">Usuń usługę</span><span class="sxs-lookup"><span data-stu-id="d50c0-224">Delete the service</span></span>

<span data-ttu-id="d50c0-225">Po krótkiej chwili zatrzymania usługi, odinstaluj usługę za pomocą `sc delete {SERVICE NAME}` polecenia.</span><span class="sxs-lookup"><span data-stu-id="d50c0-225">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

<span data-ttu-id="d50c0-226">Sprawdź stan usługi aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="d50c0-226">Check the status of the sample app service:</span></span>

```console
sc query MyService
```

<span data-ttu-id="d50c0-227">Gdy usługa app service przykładowego jest w `STOPPED` stanu, użyj następującego polecenia, aby odinstalować usługę aplikacji przykładowej:</span><span class="sxs-lookup"><span data-stu-id="d50c0-227">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

```console
sc delete MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="d50c0-228">Obsługa uruchamianie i zatrzymywanie wydarzeń</span><span class="sxs-lookup"><span data-stu-id="d50c0-228">Handle starting and stopping events</span></span>

<span data-ttu-id="d50c0-229">Aby obsłużyć <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, i <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> zdarzeń, wykonaj następujące dodatkowe zmiany:</span><span class="sxs-lookup"><span data-stu-id="d50c0-229">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="d50c0-230">Utwórz klasę, która pochodzi od klasy <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> z `OnStarting`, `OnStarted`, i `OnStopping` metody:</span><span class="sxs-lookup"><span data-stu-id="d50c0-230">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="d50c0-231">Tworzenie metody rozszerzenia dla <xref:Microsoft.AspNetCore.Hosting.IWebHost> , który przekazuje `CustomWebHostService` do <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="d50c0-231">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="d50c0-232">W `Program.Main`, wywołaj `RunAsCustomService` rozszerzenia zamiast metody <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="d50c0-232">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="d50c0-233">Aby wyświetlić lokalizację <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> w `Program.Main`, można znaleźć w przykładzie kodu pokazano w [konwertować projekt w usłudze Windows](#convert-a-project-into-a-windows-service) sekcji.</span><span class="sxs-lookup"><span data-stu-id="d50c0-233">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="d50c0-234">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="d50c0-234">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="d50c0-235">Usługi wchodzić w interakcje z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub moduł równoważenia obciążenia, które mogą wymagać dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d50c0-235">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="d50c0-236">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="d50c0-236">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="d50c0-237">Konfigurowanie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="d50c0-237">Configure HTTPS</span></span>

<span data-ttu-id="d50c0-238">Aby skonfigurować usługę z bezpiecznego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="d50c0-238">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="d50c0-239">Utwórz certyfikat X.509 dla systemu macierzystego przy użyciu pozyskiwania certyfikatu używanej platformy i mechanizmy wdrażania.</span><span class="sxs-lookup"><span data-stu-id="d50c0-239">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="d50c0-240">Określ [konfiguracji punktu końcowego HTTPS serwera Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) przy użyciu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="d50c0-240">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="d50c0-241">Użycie certyfikatu deweloperskiego platformy ASP.NET Core HTTPS, aby zabezpieczyć punkt końcowy usługi nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="d50c0-241">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="d50c0-242">Bieżący katalog i katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="d50c0-242">Current directory and content root</span></span>

<span data-ttu-id="d50c0-243">Bieżący katalog roboczy zwracany przez wywołanie metody <xref:System.IO.Directory.GetCurrentDirectory*> dla usługi Windows jest *C:\\WINDOWS\\system32* folderu.</span><span class="sxs-lookup"><span data-stu-id="d50c0-243">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="d50c0-244">*System32* folder nie jest odpowiednią lokalizację do przechowywania plików usługi (na przykład pliki ustawień).</span><span class="sxs-lookup"><span data-stu-id="d50c0-244">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="d50c0-245">Użyj jednej z następujących metod do utrzymywania i uzyskać dostęp do zasobów i plików ustawień usługi.</span><span class="sxs-lookup"><span data-stu-id="d50c0-245">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="d50c0-246">Ustaw ścieżkę katalogu głównego zawartości do folderu aplikacji</span><span class="sxs-lookup"><span data-stu-id="d50c0-246">Set the content root path to the app's folder</span></span>

<span data-ttu-id="d50c0-247"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> Tej samej ścieżki, które są udostępniane `binPath` argumentów podczas tworzenia usługi.</span><span class="sxs-lookup"><span data-stu-id="d50c0-247">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="d50c0-248">Zamiast wywoływać metodę `GetCurrentDirectory` do tworzenia ścieżek do plików ustawień, należy wywołać <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> ze ścieżką do katalogu głównego zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d50c0-248">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> with the path to the app's content root.</span></span>

<span data-ttu-id="d50c0-249">W `Program.Main`, określić ścieżkę do folderu pliku wykonywalnego usługi i ustanowić zawartości katalogu głównego aplikacji za pomocą ścieżki:</span><span class="sxs-lookup"><span data-stu-id="d50c0-249">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);

CreateWebHostBuilder(args)
    .UseContentRoot(pathToContentRoot)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="d50c0-250">Store usługi plików w odpowiedniej lokalizacji na dysku</span><span class="sxs-lookup"><span data-stu-id="d50c0-250">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="d50c0-251">Określ ścieżkę bezwzględną z <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> przy użyciu <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do folderu zawierającego pliki.</span><span class="sxs-lookup"><span data-stu-id="d50c0-251">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d50c0-252">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d50c0-252">Additional resources</span></span>

* <span data-ttu-id="d50c0-253">[Konfiguracja punktu końcowego kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym konfiguracja protokołu HTTPS i obsługa SNI)</span><span class="sxs-lookup"><span data-stu-id="d50c0-253">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
