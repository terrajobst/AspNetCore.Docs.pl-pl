---
title: ASP.NET Core hosta w usłudze systemu Windows
author: guardrex
description: Dowiedz się, jak hostować aplikację ASP.NET Core w usłudze systemu Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/09/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 995fdd2bbba30ff983bc2055fcb97c14541e2ac6
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081483"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="38bca-103">ASP.NET Core hosta w usłudze systemu Windows</span><span class="sxs-lookup"><span data-stu-id="38bca-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="38bca-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Tomasz Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="38bca-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="38bca-105">Aplikacja ASP.NET Core może być hostowana w systemie Windows jako [Usługa systemu Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) bez korzystania z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="38bca-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="38bca-106">Gdy usługa jest hostowana w systemie Windows, aplikacja jest uruchamiana automatycznie po ponownym uruchomieniu serwera.</span><span class="sxs-lookup"><span data-stu-id="38bca-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="38bca-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="38bca-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="38bca-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="38bca-108">Prerequisites</span></span>

* [<span data-ttu-id="38bca-109">ASP.NET Core zestaw SDK 2,1 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="38bca-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="38bca-110">Program PowerShell 6,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="38bca-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="38bca-111">Szablon usługi procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="38bca-111">Worker Service template</span></span>

<span data-ttu-id="38bca-112">Szablon usługi ASP.NET Core Worker zapewnia punkt początkowy do pisania długotrwałych aplikacji usługi.</span><span class="sxs-lookup"><span data-stu-id="38bca-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="38bca-113">Aby użyć szablonu jako podstawy dla aplikacji usługi systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="38bca-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="38bca-114">Utwórz aplikację usługi procesu roboczego na podstawie szablonu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="38bca-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="38bca-115">Postępuj zgodnie ze wskazówkami w sekcji [Konfiguracja aplikacji](#app-configuration) , aby zaktualizować aplikację usługi procesu roboczego tak, aby mogła ona działać jako usługa systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="38bca-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="38bca-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="38bca-116">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="38bca-117">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="38bca-117">Create a new project.</span></span>
1. <span data-ttu-id="38bca-118">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="38bca-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="38bca-119">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="38bca-119">Select **Next**.</span></span>
1. <span data-ttu-id="38bca-120">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="38bca-120">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="38bca-121">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="38bca-121">Select **Create**.</span></span>
1. <span data-ttu-id="38bca-122">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,0** .</span><span class="sxs-lookup"><span data-stu-id="38bca-122">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="38bca-123">Wybierz szablon **usługi procesu roboczego** .</span><span class="sxs-lookup"><span data-stu-id="38bca-123">Select the **Worker Service** template.</span></span> <span data-ttu-id="38bca-124">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="38bca-124">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="38bca-125">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="38bca-125">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="38bca-126">Użyj szablonu usługi procesu roboczego`worker`() z poleceniem [dotnet New](/dotnet/core/tools/dotnet-new) z powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="38bca-126">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="38bca-127">W poniższym przykładzie jest tworzona aplikacja usługi Worker o nazwie `ContosoWorkerService`.</span><span class="sxs-lookup"><span data-stu-id="38bca-127">In the following example, a Worker Service app is created named `ContosoWorkerService`.</span></span> <span data-ttu-id="38bca-128">Folder dla `ContosoWorkerService` aplikacji jest tworzony automatycznie podczas wykonywania polecenia.</span><span class="sxs-lookup"><span data-stu-id="38bca-128">A folder for the `ContosoWorkerService` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="app-configuration"></a><span data-ttu-id="38bca-129">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="38bca-129">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="38bca-130">`IHostBuilder.UseWindowsService`dostarczone przez pakiet [Microsoft. Extensions. hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) jest wywoływany podczas kompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="38bca-130">`IHostBuilder.UseWindowsService`, provided by the [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) package, is called when building the host.</span></span> <span data-ttu-id="38bca-131">Jeśli aplikacja działa jako usługa systemu Windows, Metoda:</span><span class="sxs-lookup"><span data-stu-id="38bca-131">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="38bca-132">Ustawia okres istnienia hosta `WindowsServiceLifetime`na.</span><span class="sxs-lookup"><span data-stu-id="38bca-132">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="38bca-133">Ustawia katalog główny zawartości.</span><span class="sxs-lookup"><span data-stu-id="38bca-133">Sets the content root.</span></span>
* <span data-ttu-id="38bca-134">Umożliwia rejestrowanie w dzienniku zdarzeń przy użyciu nazwy aplikacji jako domyślnej nazwy źródła.</span><span class="sxs-lookup"><span data-stu-id="38bca-134">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="38bca-135">Poziom dziennika można skonfigurować przy użyciu `Logging:LogLevel:Default` klucza w pliku *appSettings. Plik Product. JSON* .</span><span class="sxs-lookup"><span data-stu-id="38bca-135">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="38bca-136">Tylko Administratorzy mogą tworzyć nowe źródła zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="38bca-136">Only administrators can create new event sources.</span></span> <span data-ttu-id="38bca-137">Gdy nie można utworzyć źródła zdarzeń przy użyciu nazwy aplikacji, w źródle *aplikacji* jest rejestrowane ostrzeżenie, a dzienniki zdarzeń są wyłączone.</span><span class="sxs-lookup"><span data-stu-id="38bca-137">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="38bca-138">Aplikacja wymaga odwołania do pakietów dla elementu [Microsoft. AspNetCore. hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) i [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="38bca-138">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="38bca-139">Aby przetestować i debugować w przypadku uruchamiania poza usługą, należy dodać kod, aby określić, czy aplikacja działa jako usługa, czy Aplikacja konsolowa.</span><span class="sxs-lookup"><span data-stu-id="38bca-139">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="38bca-140">Sprawdź, czy debuger jest dołączony lub czy `--console` jest obecny przełącznik.</span><span class="sxs-lookup"><span data-stu-id="38bca-140">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="38bca-141">Jeśli którykolwiek z warunków jest spełniony (aplikacja nie jest uruchomiona jako usługa), wywołaj metodę <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="38bca-141">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="38bca-142">Jeśli warunki są fałszywe (aplikacja jest uruchamiana jako usługa):</span><span class="sxs-lookup"><span data-stu-id="38bca-142">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="38bca-143">Wywołaj <xref:System.IO.Directory.SetCurrentDirectory*> i użyj ścieżki do opublikowanej lokalizacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="38bca-143">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="38bca-144">Nie wywołuj <xref:System.IO.Directory.GetCurrentDirectory*> , aby uzyskać ścieżkę, ponieważ aplikacja usługi systemu Windows zwraca folder *C\\:\\Windows system32* , <xref:System.IO.Directory.GetCurrentDirectory*> gdy jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="38bca-144">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="38bca-145">Aby uzyskać więcej informacji, zapoznaj się z sekcją [Current Directory i root Content](#current-directory-and-content-root) .</span><span class="sxs-lookup"><span data-stu-id="38bca-145">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="38bca-146">Ten krok jest wykonywany przed skonfigurowaniem aplikacji w programie `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="38bca-146">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="38bca-147">Wywołaj <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> , aby uruchomić aplikację jako usługę.</span><span class="sxs-lookup"><span data-stu-id="38bca-147">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="38bca-148">Ponieważ [dostawca konfiguracji wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) wymaga par nazwa-wartość dla argumentów wiersza polecenia, `--console` przełącznik jest usuwany z argumentów przed <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> odebraniem argumentów.</span><span class="sxs-lookup"><span data-stu-id="38bca-148">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="38bca-149">Aby zapisać w dzienniku zdarzeń systemu Windows, Dodaj dostawcę EventLog do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>programu.</span><span class="sxs-lookup"><span data-stu-id="38bca-149">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="38bca-150">Ustaw poziom rejestrowania przy użyciu `Logging:LogLevel:Default` klucza w pliku *appSettings. Plik Product. JSON* .</span><span class="sxs-lookup"><span data-stu-id="38bca-150">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="38bca-151">W poniższym przykładzie z przykładowej aplikacji `RunAsCustomService` jest wywoływana <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> zamiast w celu obsługi zdarzeń okresu istnienia w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="38bca-151">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="38bca-152">Aby uzyskać więcej informacji, zobacz sekcję [Obsługa zdarzeń dotyczących uruchamiania i zatrzymywania](#handle-starting-and-stopping-events) .</span><span class="sxs-lookup"><span data-stu-id="38bca-152">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="38bca-153">Typ wdrożenia</span><span class="sxs-lookup"><span data-stu-id="38bca-153">Deployment type</span></span>

<span data-ttu-id="38bca-154">Aby uzyskać informacje i porady dotyczące scenariuszy wdrażania, zobacz [wdrażanie aplikacji .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="38bca-154">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="38bca-155">Wdrożenie zależne od platformy (FDD)</span><span class="sxs-lookup"><span data-stu-id="38bca-155">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="38bca-156">Wdrożenie zależne od platformy (FDD) zależy od obecności udostępnionej systemowej wersji platformy .NET Core w systemie docelowym.</span><span class="sxs-lookup"><span data-stu-id="38bca-156">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="38bca-157">Po przyjęciu scenariusza FDD zgodnie ze wskazówkami zawartymi w tym artykule zestaw SDK tworzy plik wykonywalny ( *. exe*), nazywany *plik wykonywalny zależny od platformy*.</span><span class="sxs-lookup"><span data-stu-id="38bca-157">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="38bca-158">Dodaj następujące elementy właściwości do pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="38bca-158">Add the following property elements to the project file:</span></span>

* <span data-ttu-id="38bca-159">`<OutputType>`Typ danych wyjściowych aplikacji (`Exe` dla pliku wykonywalnego). &ndash;</span><span class="sxs-lookup"><span data-stu-id="38bca-159">`<OutputType>` &ndash; The app's output type (`Exe` for executable).</span></span>
* <span data-ttu-id="38bca-160">`<LangVersion>`Wersja językowa (`latest` lub`preview`). &ndash; C#</span><span class="sxs-lookup"><span data-stu-id="38bca-160">`<LangVersion>` &ndash; The C# language version (`latest` or `preview`).</span></span>

<span data-ttu-id="38bca-161">Plik *Web. config* , który jest zwykle tworzony podczas publikowania aplikacji ASP.NET Core, nie jest konieczny dla aplikacji usług systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="38bca-161">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="38bca-162">Aby wyłączyć tworzenie pliku *Web. config* , należy dodać `<IsTransformWebConfigDisabled>` Właściwość ustawioną na `true`.</span><span class="sxs-lookup"><span data-stu-id="38bca-162">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="38bca-163">[Identyfikator środowiska uruchomieniowego systemu Windows (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) zawiera platformę docelową.</span><span class="sxs-lookup"><span data-stu-id="38bca-163">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="38bca-164">W poniższym przykładzie identyfikator RID jest ustawiony na `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="38bca-164">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="38bca-165">Właściwość jest ustawiona na `false`. `<SelfContained>`</span><span class="sxs-lookup"><span data-stu-id="38bca-165">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="38bca-166">Te właściwości instruują zestaw SDK, aby wygenerował plik wykonywalny (*exe*) dla systemu Windows i aplikację, która zależy od współużytkowanej platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="38bca-166">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="38bca-167">Plik *Web. config* , który jest zwykle tworzony podczas publikowania aplikacji ASP.NET Core, nie jest konieczny dla aplikacji usług systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="38bca-167">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="38bca-168">Aby wyłączyć tworzenie pliku *Web. config* , należy dodać `<IsTransformWebConfigDisabled>` Właściwość ustawioną na `true`.</span><span class="sxs-lookup"><span data-stu-id="38bca-168">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="38bca-169">[Identyfikator środowiska uruchomieniowego systemu Windows (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) zawiera platformę docelową.</span><span class="sxs-lookup"><span data-stu-id="38bca-169">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="38bca-170">W poniższym przykładzie identyfikator RID jest ustawiony na `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="38bca-170">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="38bca-171">Właściwość jest ustawiona na `false`. `<SelfContained>`</span><span class="sxs-lookup"><span data-stu-id="38bca-171">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="38bca-172">Te właściwości instruują zestaw SDK, aby wygenerował plik wykonywalny (*exe*) dla systemu Windows i aplikację, która zależy od współużytkowanej platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="38bca-172">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="38bca-173">Właściwość jest ustawiona na `true`. `<UseAppHost>`</span><span class="sxs-lookup"><span data-stu-id="38bca-173">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="38bca-174">Ta właściwość zapewnia usługę ze ścieżką aktywacji (plik wykonywalny, *exe*) dla FDD.</span><span class="sxs-lookup"><span data-stu-id="38bca-174">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="38bca-175">Plik *Web. config* , który jest zwykle tworzony podczas publikowania aplikacji ASP.NET Core, nie jest konieczny dla aplikacji usług systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="38bca-175">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="38bca-176">Aby wyłączyć tworzenie pliku *Web. config* , należy dodać `<IsTransformWebConfigDisabled>` Właściwość ustawioną na `true`.</span><span class="sxs-lookup"><span data-stu-id="38bca-176">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="38bca-177">Wdrażanie samodzielne (SCD)</span><span class="sxs-lookup"><span data-stu-id="38bca-177">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="38bca-178">Wdrożenie samodzielne (SCD) nie polega na obecności struktury udostępnionej w systemie hosta.</span><span class="sxs-lookup"><span data-stu-id="38bca-178">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="38bca-179">Środowisko uruchomieniowe i zależności aplikacji są wdrażane przy użyciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="38bca-179">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="38bca-180">[Identyfikator środowiska uruchomieniowego systemu Windows (RID)](/dotnet/core/rid-catalog) znajduje się `<PropertyGroup>` w, który zawiera platformę docelową:</span><span class="sxs-lookup"><span data-stu-id="38bca-180">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="38bca-181">Aby opublikować dla wielu identyfikatorów RID:</span><span class="sxs-lookup"><span data-stu-id="38bca-181">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="38bca-182">Podaj identyfikatory RID na liście rozdzielanej średnikami.</span><span class="sxs-lookup"><span data-stu-id="38bca-182">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="38bca-183">Użyj nazwy [ \<właściwości RuntimeIdentifiers >](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span><span class="sxs-lookup"><span data-stu-id="38bca-183">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="38bca-184">Aby uzyskać więcej informacji, zobacz [katalog .NET Core RID Catalog](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="38bca-184">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="38bca-185">Właściwość jest ustawiona na `true`: `<SelfContained>`</span><span class="sxs-lookup"><span data-stu-id="38bca-185">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="38bca-186">Konto użytkownika usługi</span><span class="sxs-lookup"><span data-stu-id="38bca-186">Service user account</span></span>

<span data-ttu-id="38bca-187">Aby utworzyć konto użytkownika dla usługi, należy użyć polecenia cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) w powłoce poleceń administracyjnych programu PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="38bca-187">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="38bca-188">W systemie Windows 10 października 2018 Update (wersja 1809/Kompilacja 10.0.17763) lub nowsza:</span><span class="sxs-lookup"><span data-stu-id="38bca-188">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="38bca-189">W systemie operacyjnym Windows w wersji starszej niż aktualizacja 2018 systemu Windows 10 października (wersja 1809/Kompilacja 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="38bca-189">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

<span data-ttu-id="38bca-190">Podaj [silne hasło](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) po wyświetleniu monitu.</span><span class="sxs-lookup"><span data-stu-id="38bca-190">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="38bca-191">Jeśli parametr nie zostanie dostarczony do polecenia cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) z wygaśnięciem <xref:System.DateTime>, konto nie wygaśnie. `-AccountExpires`</span><span class="sxs-lookup"><span data-stu-id="38bca-191">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="38bca-192">Aby uzyskać więcej informacji, zobacz [Microsoft. PowerShell. LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) i [konta użytkowników usług](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="38bca-192">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="38bca-193">Alternatywna metoda zarządzania użytkownikami podczas korzystania z Active Directory polega na użyciu zarządzanych kont usług.</span><span class="sxs-lookup"><span data-stu-id="38bca-193">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="38bca-194">Aby uzyskać więcej informacji, zobacz [Omówienie kont usług zarządzanych przez grupę](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="38bca-194">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="38bca-195">Logowanie jako prawa usługi</span><span class="sxs-lookup"><span data-stu-id="38bca-195">Log on as a service rights</span></span>

<span data-ttu-id="38bca-196">Aby nawiązać *Logowanie jako prawa usługi* dla konta użytkownika usługi:</span><span class="sxs-lookup"><span data-stu-id="38bca-196">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="38bca-197">Otwórz Edytor lokalnych zasad zabezpieczeń, uruchamiając program *secpol. msc*.</span><span class="sxs-lookup"><span data-stu-id="38bca-197">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="38bca-198">Rozwiń węzeł **Zasady lokalne** , a następnie wybierz pozycję **Przypisywanie praw użytkownika**.</span><span class="sxs-lookup"><span data-stu-id="38bca-198">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="38bca-199">Otwórz okno **Logowanie jako usługa** .</span><span class="sxs-lookup"><span data-stu-id="38bca-199">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="38bca-200">Wybierz pozycję **Dodaj użytkownika lub grupę**.</span><span class="sxs-lookup"><span data-stu-id="38bca-200">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="38bca-201">Podaj nazwę obiektu (konto użytkownika) przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="38bca-201">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="38bca-202">Wpisz konto użytkownika (`{DOMAIN OR COMPUTER NAME\USER}`) w polu Nazwa obiektu, a następnie wybierz **przycisk OK** , aby dodać użytkownika do zasad.</span><span class="sxs-lookup"><span data-stu-id="38bca-202">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="38bca-203">Wybierz pozycję **Zaawansowane**.</span><span class="sxs-lookup"><span data-stu-id="38bca-203">Select **Advanced**.</span></span> <span data-ttu-id="38bca-204">Wybierz pozycję **Znajdź teraz**.</span><span class="sxs-lookup"><span data-stu-id="38bca-204">Select **Find Now**.</span></span> <span data-ttu-id="38bca-205">Wybierz z listy konto użytkownika.</span><span class="sxs-lookup"><span data-stu-id="38bca-205">Select the user account from the list.</span></span> <span data-ttu-id="38bca-206">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="38bca-206">Select **OK**.</span></span> <span data-ttu-id="38bca-207">Ponownie wybierz **przycisk OK** , aby dodać użytkownika do zasad.</span><span class="sxs-lookup"><span data-stu-id="38bca-207">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="38bca-208">Wybierz **przycisk OK** lub **Zastosuj** , aby zaakceptować zmiany.</span><span class="sxs-lookup"><span data-stu-id="38bca-208">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="38bca-209">Tworzenie usługi systemu Windows i zarządzanie nią</span><span class="sxs-lookup"><span data-stu-id="38bca-209">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="38bca-210">Tworzenie usługi</span><span class="sxs-lookup"><span data-stu-id="38bca-210">Create a service</span></span>

<span data-ttu-id="38bca-211">Zarejestrowanie usługi za pomocą poleceń programu PowerShell.</span><span class="sxs-lookup"><span data-stu-id="38bca-211">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="38bca-212">Z poziomu powłoki poleceń administracyjnych programu PowerShell 6 wykonaj następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="38bca-212">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="38bca-213">`{EXE PATH}`Ścieżka do folderu aplikacji na hoście (na `d:\myservice`przykład). &ndash;</span><span class="sxs-lookup"><span data-stu-id="38bca-213">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="38bca-214">Nie dołączaj pliku wykonywalnego aplikacji do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="38bca-214">Don't include the app's executable in the path.</span></span> <span data-ttu-id="38bca-215">Końcowy ukośnik nie jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="38bca-215">A trailing slash isn't required.</span></span>
* <span data-ttu-id="38bca-216">`{DOMAIN OR COMPUTER NAME\USER}`Konto użytkownika usługi (na `Contoso\ServiceUser`przykład). &ndash;</span><span class="sxs-lookup"><span data-stu-id="38bca-216">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="38bca-217">`{NAME}`Nazwa usługi (na `MyService`przykład). &ndash;</span><span class="sxs-lookup"><span data-stu-id="38bca-217">`{NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="38bca-218">`{EXE FILE PATH}`Ścieżka pliku wykonywalnego aplikacji (na `d:\myservice\myservice.exe`przykład). &ndash;</span><span class="sxs-lookup"><span data-stu-id="38bca-218">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="38bca-219">Uwzględnij nazwę pliku wykonywalnego z rozszerzeniem.</span><span class="sxs-lookup"><span data-stu-id="38bca-219">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="38bca-220">`{DESCRIPTION}`Opis usługi (na `My sample service`przykład). &ndash;</span><span class="sxs-lookup"><span data-stu-id="38bca-220">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="38bca-221">`{DISPLAY NAME}`Nazwa wyświetlana usługi (na `My Service`przykład). &ndash;</span><span class="sxs-lookup"><span data-stu-id="38bca-221">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="38bca-222">Uruchom usługę</span><span class="sxs-lookup"><span data-stu-id="38bca-222">Start a service</span></span>

<span data-ttu-id="38bca-223">Uruchom usługę za pomocą następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="38bca-223">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {NAME}
```

<span data-ttu-id="38bca-224">Uruchomienie usługi może potrwać kilka sekund.</span><span class="sxs-lookup"><span data-stu-id="38bca-224">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="38bca-225">Określanie stanu usługi</span><span class="sxs-lookup"><span data-stu-id="38bca-225">Determine a service's status</span></span>

<span data-ttu-id="38bca-226">Aby sprawdzić stan usługi, użyj następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="38bca-226">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {NAME}
```

<span data-ttu-id="38bca-227">Stan jest raportowany jako jedna z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="38bca-227">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="38bca-228">Zatrzymaj usługę</span><span class="sxs-lookup"><span data-stu-id="38bca-228">Stop a service</span></span>

<span data-ttu-id="38bca-229">Zatrzymaj usługę za pomocą następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="38bca-229">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="38bca-230">Usuwanie usługi</span><span class="sxs-lookup"><span data-stu-id="38bca-230">Remove a service</span></span>

<span data-ttu-id="38bca-231">Po krótkim opóźnieniu na zatrzymanie usługi Usuń usługę za pomocą następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="38bca-231">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="38bca-232">Obsługa zdarzeń uruchamiania i zatrzymywania</span><span class="sxs-lookup"><span data-stu-id="38bca-232">Handle starting and stopping events</span></span>

<span data-ttu-id="38bca-233">Aby obsłużyć <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>zdarzenia <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> , <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>i:</span><span class="sxs-lookup"><span data-stu-id="38bca-233">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="38bca-234">Utwórz klasę, która dziedziczy z <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> `OnStarting`metody, `OnStarted`, i `OnStopping` :</span><span class="sxs-lookup"><span data-stu-id="38bca-234">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="38bca-235">Utwórz metodę rozszerzenia dla <xref:Microsoft.AspNetCore.Hosting.IWebHost> , która `CustomWebHostService` przekazuje do <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="38bca-235">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="38bca-236">W `Program.Main` <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> ,`RunAsCustomService` Wywołaj metodę rozszerzenia zamiast:</span><span class="sxs-lookup"><span data-stu-id="38bca-236">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="38bca-237">Aby zobaczyć lokalizację <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> programu w programie `Program.Main`, zapoznaj się z przykładem kodu pokazanym w sekcji [typ wdrożenia](#deployment-type) .</span><span class="sxs-lookup"><span data-stu-id="38bca-237">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="38bca-238">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="38bca-238">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="38bca-239">Usługi, które współdziałają z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub modułem równoważenia obciążenia, mogą wymagać dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="38bca-239">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="38bca-240">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="38bca-240">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="38bca-241">Konfigurowanie punktów końcowych</span><span class="sxs-lookup"><span data-stu-id="38bca-241">Configure endpoints</span></span>

<span data-ttu-id="38bca-242">Domyślnie ASP.NET Core wiąże się z `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="38bca-242">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="38bca-243">Skonfiguruj adres URL i port przez ustawienie `ASPNETCORE_URLS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="38bca-243">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="38bca-244">Dodatkowe podejścia do konfiguracji adresów URL i portów, w tym obsługa punktów końcowych HTTPS, można znaleźć w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="38bca-244">For additional URL and port configuration approaches, including support for HTTPS endpoints, see the following topics:</span></span>

* <span data-ttu-id="38bca-245"><xref:fundamentals/servers/kestrel#endpoint-configuration>Kestrel</span><span class="sxs-lookup"><span data-stu-id="38bca-245"><xref:fundamentals/servers/kestrel#endpoint-configuration> (Kestrel)</span></span>
* <span data-ttu-id="38bca-246"><xref:fundamentals/servers/httpsys#configure-windows-server>(HTTP. sys)</span><span class="sxs-lookup"><span data-stu-id="38bca-246"><xref:fundamentals/servers/httpsys#configure-windows-server> (HTTP.sys)</span></span>

> [!NOTE]
> <span data-ttu-id="38bca-247">Korzystanie z ASP.NET Core certyfikatu deweloperskiego HTTPS w celu zabezpieczenia punktu końcowego usługi nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="38bca-247">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="38bca-248">Bieżący katalog i główna Zawartość</span><span class="sxs-lookup"><span data-stu-id="38bca-248">Current directory and content root</span></span>

<span data-ttu-id="38bca-249">Bieżącym katalogiem roboczym zwróconym <xref:System.IO.Directory.GetCurrentDirectory*> przez wywołanie dla usługi systemu Windows jest folder *\\C\\: Windows system32* .</span><span class="sxs-lookup"><span data-stu-id="38bca-249">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="38bca-250">Folder *system32* nie jest odpowiednią lokalizacją do przechowywania plików usługi (na przykład plików ustawień).</span><span class="sxs-lookup"><span data-stu-id="38bca-250">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="38bca-251">Użyj jednego z poniższych metod, aby zachować i uzyskać dostęp do plików ustawień i zasobów usługi.</span><span class="sxs-lookup"><span data-stu-id="38bca-251">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="38bca-252">Użyj ContentRootPath lub ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="38bca-252">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="38bca-253">Użyj [IHostEnvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) lub <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> do lokalizowania zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="38bca-253">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="38bca-254">Ustawianie ścieżki katalogu głównego zawartości do folderu aplikacji</span><span class="sxs-lookup"><span data-stu-id="38bca-254">Set the content root path to the app's folder</span></span>

<span data-ttu-id="38bca-255">Jest to ta sama ścieżka `binPath` do argumentu podczas tworzenia usługi. <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*></span><span class="sxs-lookup"><span data-stu-id="38bca-255">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="38bca-256">Zamiast wywoływania `GetCurrentDirectory` funkcji tworzenia ścieżek do plików ustawień, należy wywołać <xref:System.IO.Directory.SetCurrentDirectory*> ścieżkę do katalogu głównego zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="38bca-256">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="38bca-257">W `Program.Main`programie określ ścieżkę do folderu wykonywalnego usługi i użyj ścieżki, aby określić katalog główny zawartości aplikacji:</span><span class="sxs-lookup"><span data-stu-id="38bca-257">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="38bca-258">Przechowywanie plików usługi w odpowiedniej lokalizacji na dysku</span><span class="sxs-lookup"><span data-stu-id="38bca-258">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="38bca-259">Określ ścieżkę <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> bezwzględną przy <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> użyciu do folderu zawierającego pliki.</span><span class="sxs-lookup"><span data-stu-id="38bca-259">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="38bca-260">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="38bca-260">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="38bca-261">[Konfiguracja punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (obejmuje konfigurację protokołu HTTPS i pomoc techniczną SNI)</span><span class="sxs-lookup"><span data-stu-id="38bca-261">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="38bca-262">[Konfiguracja punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (obejmuje konfigurację protokołu HTTPS i pomoc techniczną SNI)</span><span class="sxs-lookup"><span data-stu-id="38bca-262">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
