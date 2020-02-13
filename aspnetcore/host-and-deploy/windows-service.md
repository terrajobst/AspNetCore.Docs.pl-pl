---
title: ASP.NET Core hosta w usłudze systemu Windows
author: guardrex
description: Dowiedz się, jak hostować aplikację ASP.NET Core w usłudze systemu Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: host-and-deploy/windows-service
ms.openlocfilehash: 829c282606e60a80682233555e1268acb706090e
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172330"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="cc9af-103">ASP.NET Core hosta w usłudze systemu Windows</span><span class="sxs-lookup"><span data-stu-id="cc9af-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="cc9af-104">Autor [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="cc9af-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cc9af-105">Aplikacja ASP.NET Core może być hostowana w systemie Windows jako [Usługa systemu Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) bez korzystania z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="cc9af-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="cc9af-106">Gdy usługa jest hostowana w systemie Windows, aplikacja jest uruchamiana automatycznie po ponownym uruchomieniu serwera.</span><span class="sxs-lookup"><span data-stu-id="cc9af-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="cc9af-107">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cc9af-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc9af-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="cc9af-108">Prerequisites</span></span>

* [<span data-ttu-id="cc9af-109">ASP.NET Core zestaw SDK 2,1 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="cc9af-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="cc9af-110">Program PowerShell 6,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="cc9af-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="worker-service-template"></a><span data-ttu-id="cc9af-111">Szablon usługi procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="cc9af-111">Worker Service template</span></span>

<span data-ttu-id="cc9af-112">Szablon usługi ASP.NET Core Worker zapewnia punkt początkowy do pisania długotrwałych aplikacji usługi.</span><span class="sxs-lookup"><span data-stu-id="cc9af-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="cc9af-113">Aby użyć szablonu jako podstawy dla aplikacji usługi systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="cc9af-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="cc9af-114">Utwórz aplikację usługi procesu roboczego na podstawie szablonu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc9af-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="cc9af-115">Postępuj zgodnie ze wskazówkami w sekcji [Konfiguracja aplikacji](#app-configuration) , aby zaktualizować aplikację usługi procesu roboczego tak, aby mogła ona działać jako usługa systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="cc9af-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="app-configuration"></a><span data-ttu-id="cc9af-116">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="cc9af-116">App configuration</span></span>

<span data-ttu-id="cc9af-117">Aplikacja wymaga odwołania do pakietu dla elementu [Microsoft. Extensions. hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="cc9af-117">The app requires a package reference for [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span></span>

<span data-ttu-id="cc9af-118">`IHostBuilder.UseWindowsService` jest wywoływana podczas kompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="cc9af-118">`IHostBuilder.UseWindowsService` is called when building the host.</span></span> <span data-ttu-id="cc9af-119">Jeśli aplikacja działa jako usługa systemu Windows, Metoda:</span><span class="sxs-lookup"><span data-stu-id="cc9af-119">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="cc9af-120">Ustawia okres istnienia hosta do `WindowsServiceLifetime`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-120">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="cc9af-121">Ustawia [katalog główny zawartości](xref:fundamentals/index#content-root) na [AppContext. BaseDirectory](xref:System.AppContext.BaseDirectory).</span><span class="sxs-lookup"><span data-stu-id="cc9af-121">Sets the [content root](xref:fundamentals/index#content-root) to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span> <span data-ttu-id="cc9af-122">Aby uzyskać więcej informacji, zapoznaj się z sekcją [Current Directory i root Content](#current-directory-and-content-root) .</span><span class="sxs-lookup"><span data-stu-id="cc9af-122">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
* <span data-ttu-id="cc9af-123">Włącza rejestrowanie w dzienniku zdarzeń:</span><span class="sxs-lookup"><span data-stu-id="cc9af-123">Enables logging to the event log:</span></span>
  * <span data-ttu-id="cc9af-124">Nazwa aplikacji jest używana jako domyślna nazwa źródła.</span><span class="sxs-lookup"><span data-stu-id="cc9af-124">The application name is used as the default source name.</span></span>
  * <span data-ttu-id="cc9af-125">Domyślny poziom rejestrowania jest *ostrzegawczy* lub wyższy dla aplikacji opartej na szablonie ASP.NET Core, który wywołuje `CreateDefaultBuilder` do skompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="cc9af-125">The default log level is *Warning* or higher for an app based on an ASP.NET Core template that calls `CreateDefaultBuilder` to build the host.</span></span>
  * <span data-ttu-id="cc9af-126">Zastąp domyślny poziom dziennika kluczem `Logging:EventLog:LogLevel:Default` w pliku *appSettings. json*/*appSettings. { Environment}. JSON* lub inny dostawca konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-126">Override the default log level with the `Logging:EventLog:LogLevel:Default` key in *appsettings.json*/*appsettings.{Environment}.json* or other configuration provider.</span></span>
  * <span data-ttu-id="cc9af-127">Tylko Administratorzy mogą tworzyć nowe źródła zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cc9af-127">Only administrators can create new event sources.</span></span> <span data-ttu-id="cc9af-128">Gdy nie można utworzyć źródła zdarzeń przy użyciu nazwy aplikacji, w źródle *aplikacji* jest rejestrowane ostrzeżenie, a dzienniki zdarzeń są wyłączone.</span><span class="sxs-lookup"><span data-stu-id="cc9af-128">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

<span data-ttu-id="cc9af-129">W `CreateHostBuilder` *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="cc9af-129">In `CreateHostBuilder` of *Program.cs*:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseWindowsService()
    ...
```

<span data-ttu-id="cc9af-130">Następujące przykładowe aplikacje zostały dołączone do tego tematu:</span><span class="sxs-lookup"><span data-stu-id="cc9af-130">The following sample apps accompany this topic:</span></span>

* <span data-ttu-id="cc9af-131">Przykład usługi procesu roboczego w tle &ndash; przykład aplikacji innej niż sieć Web oparta na [szablonie usługi procesu roboczego](#worker-service-template) , który korzysta z [usług hostowanych](xref:fundamentals/host/hosted-services) na potrzeby zadań w tle.</span><span class="sxs-lookup"><span data-stu-id="cc9af-131">Background Worker Service Sample &ndash; A non-web app sample based on the [Worker Service template](#worker-service-template) that uses [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>
* <span data-ttu-id="cc9af-132">Przykład App Service sieci Web &ndash; przykład aplikacji internetowej Razor Pages, która działa jako usługa systemu Windows z [usługami hostowanymi](xref:fundamentals/host/hosted-services) dla zadań w tle.</span><span class="sxs-lookup"><span data-stu-id="cc9af-132">Web App Service Sample &ndash; A Razor Pages web app sample that runs as a Windows Service with [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>

<span data-ttu-id="cc9af-133">Wskazówki dotyczące MVC można znaleźć w artykułach w obszarze <xref:mvc/overview> i <xref:migration/22-to-30>.</span><span class="sxs-lookup"><span data-stu-id="cc9af-133">For MVC guidance, see the articles under <xref:mvc/overview> and <xref:migration/22-to-30>.</span></span>

## <a name="deployment-type"></a><span data-ttu-id="cc9af-134">Typ wdrożenia</span><span class="sxs-lookup"><span data-stu-id="cc9af-134">Deployment type</span></span>

<span data-ttu-id="cc9af-135">Aby uzyskać informacje i porady dotyczące scenariuszy wdrażania, zobacz [wdrażanie aplikacji .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="cc9af-135">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="cc9af-136">SDK</span><span class="sxs-lookup"><span data-stu-id="cc9af-136">SDK</span></span>

<span data-ttu-id="cc9af-137">W przypadku usługi opartej na aplikacji sieci Web używającej platform Razor Pages lub MVC należy określić zestaw SDK sieci Web w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="cc9af-137">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="cc9af-138">Jeśli usługa wykonuje tylko zadania w tle (na przykład [usługi hostowane](xref:fundamentals/host/hosted-services)), określ zestaw SDK procesu roboczego w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="cc9af-138">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="cc9af-139">Wdrożenie zależne od platformy (FDD)</span><span class="sxs-lookup"><span data-stu-id="cc9af-139">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="cc9af-140">Wdrożenie zależne od platformy (FDD) zależy od obecności udostępnionej systemowej wersji platformy .NET Core w systemie docelowym.</span><span class="sxs-lookup"><span data-stu-id="cc9af-140">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="cc9af-141">Po przyjęciu scenariusza FDD zgodnie ze wskazówkami zawartymi w tym artykule zestaw SDK tworzy plik wykonywalny ( *. exe*), nazywany *plik wykonywalny zależny od platformy*.</span><span class="sxs-lookup"><span data-stu-id="cc9af-141">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

<span data-ttu-id="cc9af-142">Jeśli używasz [zestawu SDK sieci Web](#sdk), plik *Web. config* , który jest zwykle tworzony podczas publikowania aplikacji ASP.NET Core, jest zbędny dla aplikacji usług systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="cc9af-142">If using the [Web SDK](#sdk), a *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="cc9af-143">Aby wyłączyć tworzenie pliku *Web. config* , należy dodać właściwość `<IsTransformWebConfigDisabled>` ustawioną na `true`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-143">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="cc9af-144">Wdrażanie samodzielne (SCD)</span><span class="sxs-lookup"><span data-stu-id="cc9af-144">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="cc9af-145">Wdrożenie samodzielne (SCD) nie polega na obecności struktury udostępnionej w systemie hosta.</span><span class="sxs-lookup"><span data-stu-id="cc9af-145">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="cc9af-146">Środowisko uruchomieniowe i zależności aplikacji są wdrażane przy użyciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-146">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="cc9af-147">[Identyfikator środowiska uruchomieniowego systemu Windows (RID)](/dotnet/core/rid-catalog) znajduje się w `<PropertyGroup>`, który zawiera platformę docelową:</span><span class="sxs-lookup"><span data-stu-id="cc9af-147">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="cc9af-148">Aby opublikować dla wielu identyfikatorów RID:</span><span class="sxs-lookup"><span data-stu-id="cc9af-148">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="cc9af-149">Podaj identyfikatory RID na liście rozdzielanej średnikami.</span><span class="sxs-lookup"><span data-stu-id="cc9af-149">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="cc9af-150">Użyj nazwy właściwości [\<RuntimeIdentifiers >](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span><span class="sxs-lookup"><span data-stu-id="cc9af-150">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="cc9af-151">Aby uzyskać więcej informacji, zobacz [katalog .NET Core RID Catalog](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="cc9af-151">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

## <a name="service-user-account"></a><span data-ttu-id="cc9af-152">Konto użytkownika usługi</span><span class="sxs-lookup"><span data-stu-id="cc9af-152">Service user account</span></span>

<span data-ttu-id="cc9af-153">Aby utworzyć konto użytkownika dla usługi, należy użyć polecenia cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) w powłoce poleceń administracyjnych programu PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="cc9af-153">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="cc9af-154">W systemie Windows 10 października 2018 Update (wersja 1809/Kompilacja 10.0.17763) lub nowsza:</span><span class="sxs-lookup"><span data-stu-id="cc9af-154">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```powershell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="cc9af-155">W systemie operacyjnym Windows w wersji starszej niż aktualizacja 2018 systemu Windows 10 października (wersja 1809/Kompilacja 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="cc9af-155">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="cc9af-156">Podaj [silne hasło](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) po wyświetleniu monitu.</span><span class="sxs-lookup"><span data-stu-id="cc9af-156">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="cc9af-157">Jeśli parametr `-AccountExpires` nie zostanie dostarczony do polecenia cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) z <xref:System.DateTime>em wygaśnięcia, konto nie wygaśnie.</span><span class="sxs-lookup"><span data-stu-id="cc9af-157">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="cc9af-158">Aby uzyskać więcej informacji, zobacz [Microsoft. PowerShell. LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) i [konta użytkowników usług](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="cc9af-158">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="cc9af-159">Alternatywna metoda zarządzania użytkownikami podczas korzystania z Active Directory polega na użyciu zarządzanych kont usług.</span><span class="sxs-lookup"><span data-stu-id="cc9af-159">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="cc9af-160">Aby uzyskać więcej informacji, zobacz [Omówienie kont usług zarządzanych przez grupę](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="cc9af-160">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="cc9af-161">Logowanie jako prawa usługi</span><span class="sxs-lookup"><span data-stu-id="cc9af-161">Log on as a service rights</span></span>

<span data-ttu-id="cc9af-162">Aby nawiązać *Logowanie jako prawa usługi* dla konta użytkownika usługi:</span><span class="sxs-lookup"><span data-stu-id="cc9af-162">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="cc9af-163">Otwórz Edytor lokalnych zasad zabezpieczeń, uruchamiając program *secpol. msc*.</span><span class="sxs-lookup"><span data-stu-id="cc9af-163">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="cc9af-164">Rozwiń węzeł **Zasady lokalne** , a następnie wybierz pozycję **Przypisywanie praw użytkownika**.</span><span class="sxs-lookup"><span data-stu-id="cc9af-164">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="cc9af-165">Otwórz okno **Logowanie jako usługa** .</span><span class="sxs-lookup"><span data-stu-id="cc9af-165">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="cc9af-166">Wybierz pozycję **Dodaj użytkownika lub grupę**.</span><span class="sxs-lookup"><span data-stu-id="cc9af-166">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="cc9af-167">Podaj nazwę obiektu (konto użytkownika) przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="cc9af-167">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="cc9af-168">Wpisz konto użytkownika (`{DOMAIN OR COMPUTER NAME\USER}`) w polu Nazwa obiektu, a następnie wybierz **przycisk OK** , aby dodać użytkownika do zasad.</span><span class="sxs-lookup"><span data-stu-id="cc9af-168">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="cc9af-169">Wybierz pozycję **Zaawansowane**.</span><span class="sxs-lookup"><span data-stu-id="cc9af-169">Select **Advanced**.</span></span> <span data-ttu-id="cc9af-170">Wybierz pozycję **Znajdź teraz**.</span><span class="sxs-lookup"><span data-stu-id="cc9af-170">Select **Find Now**.</span></span> <span data-ttu-id="cc9af-171">Wybierz z listy konto użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cc9af-171">Select the user account from the list.</span></span> <span data-ttu-id="cc9af-172">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="cc9af-172">Select **OK**.</span></span> <span data-ttu-id="cc9af-173">Ponownie wybierz **przycisk OK** , aby dodać użytkownika do zasad.</span><span class="sxs-lookup"><span data-stu-id="cc9af-173">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="cc9af-174">Wybierz **przycisk OK** lub **Zastosuj** , aby zaakceptować zmiany.</span><span class="sxs-lookup"><span data-stu-id="cc9af-174">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="cc9af-175">Tworzenie usługi systemu Windows i zarządzanie nią</span><span class="sxs-lookup"><span data-stu-id="cc9af-175">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="cc9af-176">Tworzenie usługi</span><span class="sxs-lookup"><span data-stu-id="cc9af-176">Create a service</span></span>

<span data-ttu-id="cc9af-177">Zarejestrowanie usługi za pomocą poleceń programu PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cc9af-177">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="cc9af-178">Z poziomu powłoki poleceń administracyjnych programu PowerShell 6 wykonaj następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="cc9af-178">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="cc9af-179">`{EXE PATH}` &ndash; ścieżkę do folderu aplikacji na hoście (na przykład `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-179">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="cc9af-180">Nie dołączaj pliku wykonywalnego aplikacji do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="cc9af-180">Don't include the app's executable in the path.</span></span> <span data-ttu-id="cc9af-181">Końcowy ukośnik nie jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="cc9af-181">A trailing slash isn't required.</span></span>
* <span data-ttu-id="cc9af-182">konto użytkownika usługi &ndash; `{DOMAIN OR COMPUTER NAME\USER}` (na przykład `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-182">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="cc9af-183">`{SERVICE NAME}` &ndash; nazwę usługi (na przykład `MyService`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-183">`{SERVICE NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="cc9af-184">`{EXE FILE PATH}` &ndash; ścieżki pliku wykonywalnego aplikacji (na przykład `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-184">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="cc9af-185">Uwzględnij nazwę pliku wykonywalnego z rozszerzeniem.</span><span class="sxs-lookup"><span data-stu-id="cc9af-185">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="cc9af-186">`{DESCRIPTION}` &ndash; Opis usługi (na przykład `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-186">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="cc9af-187">Nazwa wyświetlana usługi `{DISPLAY NAME}` &ndash; (na przykład `My Service`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-187">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="cc9af-188">Uruchom usługę</span><span class="sxs-lookup"><span data-stu-id="cc9af-188">Start a service</span></span>

<span data-ttu-id="cc9af-189">Uruchom usługę za pomocą następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="cc9af-189">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="cc9af-190">Uruchomienie usługi może potrwać kilka sekund.</span><span class="sxs-lookup"><span data-stu-id="cc9af-190">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="cc9af-191">Określanie stanu usługi</span><span class="sxs-lookup"><span data-stu-id="cc9af-191">Determine a service's status</span></span>

<span data-ttu-id="cc9af-192">Aby sprawdzić stan usługi, użyj następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="cc9af-192">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="cc9af-193">Stan jest raportowany jako jedna z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="cc9af-193">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="cc9af-194">Zatrzymaj usługę</span><span class="sxs-lookup"><span data-stu-id="cc9af-194">Stop a service</span></span>

<span data-ttu-id="cc9af-195">Zatrzymaj usługę za pomocą następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="cc9af-195">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="cc9af-196">Usuwanie usługi</span><span class="sxs-lookup"><span data-stu-id="cc9af-196">Remove a service</span></span>

<span data-ttu-id="cc9af-197">Po krótkim opóźnieniu na zatrzymanie usługi Usuń usługę za pomocą następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="cc9af-197">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="cc9af-198">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="cc9af-198">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="cc9af-199">Usługi, które współdziałają z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub modułem równoważenia obciążenia, mogą wymagać dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-199">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="cc9af-200">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="cc9af-200">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="cc9af-201">Konfigurowanie punktów końcowych</span><span class="sxs-lookup"><span data-stu-id="cc9af-201">Configure endpoints</span></span>

<span data-ttu-id="cc9af-202">Domyślnie ASP.NET Core wiąże się z `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-202">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="cc9af-203">Skonfiguruj adres URL i port, ustawiając zmienną środowiskową `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-203">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="cc9af-204">Aby uzyskać dodatkowe podejścia do konfiguracji adresów URL i portów, zobacz artykuł dotyczący odpowiedniego serwera:</span><span class="sxs-lookup"><span data-stu-id="cc9af-204">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="cc9af-205">Powyższe wskazówki obejmują obsługę punktów końcowych HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cc9af-205">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="cc9af-206">Na przykład skonfiguruj aplikację do obsługi protokołu HTTPS, gdy uwierzytelnianie jest używane z usługą systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="cc9af-206">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="cc9af-207">Korzystanie z ASP.NET Core certyfikatu deweloperskiego HTTPS w celu zabezpieczenia punktu końcowego usługi nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="cc9af-207">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="cc9af-208">Bieżący katalog i główna Zawartość</span><span class="sxs-lookup"><span data-stu-id="cc9af-208">Current directory and content root</span></span>

<span data-ttu-id="cc9af-209">Bieżącym katalogiem roboczym zwróconym przez wywoływanie <xref:System.IO.Directory.GetCurrentDirectory*> dla usługi systemu Windows jest folder *C:\\Windows\\system32* .</span><span class="sxs-lookup"><span data-stu-id="cc9af-209">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="cc9af-210">Folder *system32* nie jest odpowiednią lokalizacją do przechowywania plików usługi (na przykład plików ustawień).</span><span class="sxs-lookup"><span data-stu-id="cc9af-210">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="cc9af-211">Użyj jednego z poniższych metod, aby zachować i uzyskać dostęp do plików ustawień i zasobów usługi.</span><span class="sxs-lookup"><span data-stu-id="cc9af-211">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="cc9af-212">Użyj ContentRootPath lub ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="cc9af-212">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="cc9af-213">Użyj [IHostEnvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) lub <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider>, aby zlokalizować zasoby aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-213">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

<span data-ttu-id="cc9af-214">Gdy aplikacja działa jako usługa, <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> ustawia <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> na [AppContext. BaseDirectory](xref:System.AppContext.BaseDirectory).</span><span class="sxs-lookup"><span data-stu-id="cc9af-214">When the app runs as a service, <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> sets the <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span>

<span data-ttu-id="cc9af-215">Domyślne pliki ustawień aplikacji, *appSettings. JSON* i *appSettings. { Environment}. JSON*jest ładowany z katalogu głównego zawartości aplikacji przez wywołanie [CreateDefaultBuilder podczas konstruowania hosta](xref:fundamentals/host/generic-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="cc9af-215">The app's default settings files, *appsettings.json* and *appsettings.{Environment}.json*, are loaded from the app's content root by calling [CreateDefaultBuilder during host construction](xref:fundamentals/host/generic-host#set-up-a-host).</span></span>

<span data-ttu-id="cc9af-216">W przypadku innych plików ustawień ładowanych przez kod dewelopera w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>nie ma potrzeby wywoływania <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="cc9af-216">For other settings files loaded by developer code in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, there's no need to call <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="cc9af-217">W poniższym przykładzie plik *custom_settings. JSON* znajduje się w katalogu głównym zawartości aplikacji i jest ładowany bez jawnego ustawiania ścieżki podstawowej:</span><span class="sxs-lookup"><span data-stu-id="cc9af-217">In the following example, the *custom_settings.json* file exists in the app's content root and is loaded without explicitly setting a base path:</span></span>

[!code-csharp[](windows-service/samples_snapshot/CustomSettingsExample.cs?highlight=13)]

<span data-ttu-id="cc9af-218">Nie próbuj używać <xref:System.IO.Directory.GetCurrentDirectory*> do uzyskania ścieżki zasobów, ponieważ aplikacja usługi systemu Windows zwraca folder *C:\\Windows\\system32* jako bieżący katalog.</span><span class="sxs-lookup"><span data-stu-id="cc9af-218">Don't attempt to use <xref:System.IO.Directory.GetCurrentDirectory*> to obtain a resource path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder as its current directory.</span></span>

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="cc9af-219">Przechowywanie plików usługi w odpowiedniej lokalizacji na dysku</span><span class="sxs-lookup"><span data-stu-id="cc9af-219">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="cc9af-220">Określ ścieżkę bezwzględną <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> w przypadku używania <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do folderu zawierającego pliki.</span><span class="sxs-lookup"><span data-stu-id="cc9af-220">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="cc9af-221">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="cc9af-221">Troubleshoot</span></span>

<span data-ttu-id="cc9af-222">Aby rozwiązać problem z aplikacją usługi systemu Windows, zobacz <xref:test/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="cc9af-222">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="cc9af-223">Typowe błędy</span><span class="sxs-lookup"><span data-stu-id="cc9af-223">Common errors</span></span>

* <span data-ttu-id="cc9af-224">Starsza lub wstępnie wydana wersja programu PowerShell jest używana.</span><span class="sxs-lookup"><span data-stu-id="cc9af-224">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="cc9af-225">Zarejestrowana usługa nie używa **opublikowanych** danych wyjściowych aplikacji z [dotnet Publish](/dotnet/core/tools/dotnet-publish) polecenia.</span><span class="sxs-lookup"><span data-stu-id="cc9af-225">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="cc9af-226">Dane wyjściowe polecenia [kompilacji dotnet](/dotnet/core/tools/dotnet-build) nie są obsługiwane w przypadku wdrażania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-226">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="cc9af-227">Opublikowane zasoby znajdują się w dowolnym z następujących folderów w zależności od typu wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="cc9af-227">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="cc9af-228">*bin/Release/{Target Framework}/Publish* (FDD)</span><span class="sxs-lookup"><span data-stu-id="cc9af-228">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="cc9af-229">*bin/Release/{Target Framework}/{Runtime identyfikator}/Publish* (SCD)</span><span class="sxs-lookup"><span data-stu-id="cc9af-229">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="cc9af-230">Usługa nie jest w stanie uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="cc9af-230">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="cc9af-231">Ścieżki do zasobów używanych przez aplikację (na przykład certyfikaty) są nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="cc9af-231">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="cc9af-232">Ścieżką podstawową usługi systemu Windows jest *c:\\Windows\\system32*.</span><span class="sxs-lookup"><span data-stu-id="cc9af-232">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="cc9af-233">Użytkownik nie ma uprawnień do *logowania się jako usługa* .</span><span class="sxs-lookup"><span data-stu-id="cc9af-233">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="cc9af-234">Hasło użytkownika wygasło lub zostało nieprawidłowo przesłane podczas wykonywania polecenia `New-Service` PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cc9af-234">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="cc9af-235">Aplikacja wymaga uwierzytelniania ASP.NET Core, ale nie jest skonfigurowana dla połączeń Secure (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="cc9af-235">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="cc9af-236">Port adresu URL żądania jest niepoprawny lub niepoprawnie skonfigurowany w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-236">The request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="cc9af-237">Dzienniki zdarzeń systemu i aplikacji</span><span class="sxs-lookup"><span data-stu-id="cc9af-237">System and Application Event Logs</span></span>

<span data-ttu-id="cc9af-238">Uzyskaj dostęp do dzienników zdarzeń systemu i aplikacji:</span><span class="sxs-lookup"><span data-stu-id="cc9af-238">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="cc9af-239">Otwórz menu Start, wyszukaj ciąg *Podgląd zdarzeń*i wybierz aplikację **Podgląd zdarzeń** .</span><span class="sxs-lookup"><span data-stu-id="cc9af-239">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="cc9af-240">W **Podgląd zdarzeń**Otwórz węzeł **Dzienniki systemu Windows** .</span><span class="sxs-lookup"><span data-stu-id="cc9af-240">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="cc9af-241">Wybierz pozycję **system** , aby otworzyć dziennik zdarzeń systemu.</span><span class="sxs-lookup"><span data-stu-id="cc9af-241">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="cc9af-242">Wybierz pozycję **aplikacja** , aby otworzyć dziennik zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-242">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="cc9af-243">Wyszukaj błędy skojarzone z aplikacją się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="cc9af-243">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="cc9af-244">Uruchamianie aplikacji w wierszu polecenia</span><span class="sxs-lookup"><span data-stu-id="cc9af-244">Run the app at a command prompt</span></span>

<span data-ttu-id="cc9af-245">Wiele błędów uruchamiania nie produkuje użytecznych informacji w dziennikach zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cc9af-245">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="cc9af-246">Przyczyną niektórych błędów można znaleźć, uruchamiając aplikację w wierszu polecenia w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="cc9af-246">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="cc9af-247">Aby rejestrować dodatkowe szczegóły aplikacji, Obniż [poziom rejestrowania](xref:fundamentals/logging/index#log-level) lub Uruchom aplikację w [środowisku deweloperskim](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="cc9af-247">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="cc9af-248">Wyczyść pamięć podręczną pakietów</span><span class="sxs-lookup"><span data-stu-id="cc9af-248">Clear package caches</span></span>

<span data-ttu-id="cc9af-249">Działająca aplikacja może zakończyć się niepowodzeniem natychmiast po uaktualnieniu zestaw .NET Core SDK na komputerze deweloperskim lub zmianie wersji pakietu w ramach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-249">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="cc9af-250">W niektórych przypadkach niespójne pakietów może spowodować uszkodzenie aplikacji podczas przeprowadzania uaktualnienia głównych.</span><span class="sxs-lookup"><span data-stu-id="cc9af-250">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="cc9af-251">Większość z tych problemów można naprawić, wykonując następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="cc9af-251">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="cc9af-252">Usuń foldery *bin* i *obj* .</span><span class="sxs-lookup"><span data-stu-id="cc9af-252">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="cc9af-253">Wyczyść pamięć podręczną pakietów, wykonując [wszystkie elementy lokalne usługi NuGet programu dotnet--Wyczyść](/dotnet/core/tools/dotnet-nuget-locals) z poziomu powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="cc9af-253">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="cc9af-254">Czyszczenie pamięci podręcznych pakietów można również wykonać za pomocą narzędzia [NuGet. exe](https://www.nuget.org/downloads) i wykonując polecenie `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-254">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="cc9af-255">*NuGet. exe* nie jest pakietem instalowanym z systemem operacyjnym Windows dla komputerów stacjonarnych i musi być uzyskany niezależnie od [witryny sieci Web programu NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="cc9af-255">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="cc9af-256">Przywróć i skompiluj ponownie projekt.</span><span class="sxs-lookup"><span data-stu-id="cc9af-256">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="cc9af-257">Usuń wszystkie pliki z folderu wdrożenia na serwerze przed ponownym wdrożeniem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-257">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="cc9af-258">Wolne lub zwisa aplikacji</span><span class="sxs-lookup"><span data-stu-id="cc9af-258">Slow or hanging app</span></span>

<span data-ttu-id="cc9af-259">*Zrzut awaryjny* to migawka pamięci systemu, która może pomóc w ustaleniu przyczyny awarii aplikacji, awarii uruchamiania lub powolnej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-259">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="cc9af-260">Awaria aplikacji lub napotka wyjątek</span><span class="sxs-lookup"><span data-stu-id="cc9af-260">App crashes or encounters an exception</span></span>

<span data-ttu-id="cc9af-261">Uzyskaj i Analizuj Zrzut z [raportowanie błędów systemu Windows (raportowanie błędów systemu Windows)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="cc9af-261">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="cc9af-262">Utwórz folder do przechowywania plików zrzutu awaryjnego w `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-262">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="cc9af-263">Uruchom [skrypt programu PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) w programie EnableDumps z nazwą pliku wykonywalnego aplikacji:</span><span class="sxs-lookup"><span data-stu-id="cc9af-263">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```console
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="cc9af-264">Uruchom aplikację w warunkach, które powodują awarię.</span><span class="sxs-lookup"><span data-stu-id="cc9af-264">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="cc9af-265">Po wystąpieniu awarii Uruchom [skrypt programu DisableDumps PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="cc9af-265">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span></span>

   ```console
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="cc9af-266">Po awarii aplikacji i zakończeniu zbierania zrzutów aplikacja może zakończyć normalne działanie.</span><span class="sxs-lookup"><span data-stu-id="cc9af-266">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="cc9af-267">Skrypt programu PowerShell konfiguruje raportowanie błędów systemu Windows w celu zebrania do pięciu zrzutów na aplikację.</span><span class="sxs-lookup"><span data-stu-id="cc9af-267">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="cc9af-268">Zrzuty awaryjne mogą wymagać dużej ilości miejsca na dysku (do kilku gigabajtów).</span><span class="sxs-lookup"><span data-stu-id="cc9af-268">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="cc9af-269">Aplikacja zawiesza się, kończy się niepowodzeniem podczas uruchamiania lub działa normalnie</span><span class="sxs-lookup"><span data-stu-id="cc9af-269">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="cc9af-270">Gdy aplikacja *zawiesza* się (bez awarii), kończy się niepowodzeniem podczas uruchamiania lub działa normalnie, zobacz [pliki zrzutu w trybie użytkownika: wybór najlepszego narzędzia](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) do wygenerowania zrzutu.</span><span class="sxs-lookup"><span data-stu-id="cc9af-270">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="cc9af-271">Analizowanie zrzutu</span><span class="sxs-lookup"><span data-stu-id="cc9af-271">Analyze the dump</span></span>

<span data-ttu-id="cc9af-272">Zrzut można analizować przy użyciu kilku metod.</span><span class="sxs-lookup"><span data-stu-id="cc9af-272">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="cc9af-273">Aby uzyskać więcej informacji, zobacz [Analizowanie pliku zrzutu w trybie użytkownika](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="cc9af-273">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cc9af-274">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cc9af-274">Additional resources</span></span>

* <span data-ttu-id="cc9af-275">[Konfiguracja punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym Konfiguracja protokołu HTTPS i obsługa SNI)</span><span class="sxs-lookup"><span data-stu-id="cc9af-275">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="cc9af-276">Aplikacja ASP.NET Core może być hostowana w systemie Windows jako [Usługa systemu Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) bez korzystania z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="cc9af-276">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="cc9af-277">Gdy usługa jest hostowana w systemie Windows, aplikacja jest uruchamiana automatycznie po ponownym uruchomieniu serwera.</span><span class="sxs-lookup"><span data-stu-id="cc9af-277">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="cc9af-278">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cc9af-278">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc9af-279">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="cc9af-279">Prerequisites</span></span>

* [<span data-ttu-id="cc9af-280">ASP.NET Core zestaw SDK 2,1 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="cc9af-280">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="cc9af-281">Program PowerShell 6,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="cc9af-281">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="app-configuration"></a><span data-ttu-id="cc9af-282">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="cc9af-282">App configuration</span></span>

<span data-ttu-id="cc9af-283">Aplikacja wymaga odwołania do pakietów dla elementu [Microsoft. AspNetCore. hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) i [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="cc9af-283">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="cc9af-284">Aby przetestować i debugować w przypadku uruchamiania poza usługą, należy dodać kod, aby określić, czy aplikacja działa jako usługa, czy Aplikacja konsolowa.</span><span class="sxs-lookup"><span data-stu-id="cc9af-284">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="cc9af-285">Sprawdź, czy debuger jest podłączony lub czy jest dostępny przełącznik `--console`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-285">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="cc9af-286">Jeśli którykolwiek z warunków jest spełniony (aplikacja nie jest uruchomiona jako usługa), wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="cc9af-286">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="cc9af-287">Jeśli warunki są fałszywe (aplikacja jest uruchamiana jako usługa):</span><span class="sxs-lookup"><span data-stu-id="cc9af-287">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="cc9af-288">Wywołaj <xref:System.IO.Directory.SetCurrentDirectory*> i użyj ścieżki do lokalizacji opublikowanej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-288">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="cc9af-289">Nie wywołuj <xref:System.IO.Directory.GetCurrentDirectory*>, aby uzyskać ścieżkę, ponieważ aplikacja usługi systemu Windows zwraca folder *C:\\Windows\\system32* w przypadku wywołania <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="cc9af-289">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="cc9af-290">Aby uzyskać więcej informacji, zapoznaj się z sekcją [Current Directory i root Content](#current-directory-and-content-root) .</span><span class="sxs-lookup"><span data-stu-id="cc9af-290">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="cc9af-291">Ten krok jest wykonywany przed skonfigurowaniem aplikacji w `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-291">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="cc9af-292">Wywołaj <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>, aby uruchomić aplikację jako usługę.</span><span class="sxs-lookup"><span data-stu-id="cc9af-292">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="cc9af-293">Ponieważ [dostawca konfiguracji wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) wymaga par nazwa-wartość dla argumentów wiersza polecenia, przełącznik `--console` jest usuwany z argumentów, zanim <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> odbierze argumenty.</span><span class="sxs-lookup"><span data-stu-id="cc9af-293">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="cc9af-294">Aby zapisać w dzienniku zdarzeń systemu Windows, Dodaj dostawcę EventLog do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="cc9af-294">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="cc9af-295">Ustaw poziom rejestrowania za pomocą klucza `Logging:LogLevel:Default` w pliku *appSettings. Plik Product. JSON* .</span><span class="sxs-lookup"><span data-stu-id="cc9af-295">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="cc9af-296">W poniższym przykładzie z przykładowej aplikacji `RunAsCustomService` jest wywoływana zamiast <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> w celu obsługi zdarzeń okresu istnienia w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-296">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="cc9af-297">Aby uzyskać więcej informacji, zobacz sekcję [Obsługa zdarzeń dotyczących uruchamiania i zatrzymywania](#handle-starting-and-stopping-events) .</span><span class="sxs-lookup"><span data-stu-id="cc9af-297">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="deployment-type"></a><span data-ttu-id="cc9af-298">Typ wdrożenia</span><span class="sxs-lookup"><span data-stu-id="cc9af-298">Deployment type</span></span>

<span data-ttu-id="cc9af-299">Aby uzyskać informacje i porady dotyczące scenariuszy wdrażania, zobacz [wdrażanie aplikacji .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="cc9af-299">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="cc9af-300">SDK</span><span class="sxs-lookup"><span data-stu-id="cc9af-300">SDK</span></span>

<span data-ttu-id="cc9af-301">W przypadku usługi opartej na aplikacji sieci Web używającej platform Razor Pages lub MVC należy określić zestaw SDK sieci Web w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="cc9af-301">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="cc9af-302">Jeśli usługa wykonuje tylko zadania w tle (na przykład [usługi hostowane](xref:fundamentals/host/hosted-services)), określ zestaw SDK procesu roboczego w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="cc9af-302">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="cc9af-303">Wdrożenie zależne od platformy (FDD)</span><span class="sxs-lookup"><span data-stu-id="cc9af-303">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="cc9af-304">Wdrożenie zależne od platformy (FDD) zależy od obecności udostępnionej systemowej wersji platformy .NET Core w systemie docelowym.</span><span class="sxs-lookup"><span data-stu-id="cc9af-304">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="cc9af-305">Po przyjęciu scenariusza FDD zgodnie ze wskazówkami zawartymi w tym artykule zestaw SDK tworzy plik wykonywalny ( *. exe*), nazywany *plik wykonywalny zależny od platformy*.</span><span class="sxs-lookup"><span data-stu-id="cc9af-305">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

<span data-ttu-id="cc9af-306">[Identyfikator środowiska uruchomieniowego systemu Windows (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) zawiera platformę docelową.</span><span class="sxs-lookup"><span data-stu-id="cc9af-306">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="cc9af-307">W poniższym przykładzie identyfikator RID jest ustawiony na `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-307">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="cc9af-308">Właściwość `<SelfContained>` jest ustawiona na `false`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-308">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="cc9af-309">Te właściwości instruują zestaw SDK, aby wygenerował plik wykonywalny (*exe*) dla systemu Windows i aplikację, która zależy od współużytkowanej platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc9af-309">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="cc9af-310">Plik *Web. config* , który jest zwykle tworzony podczas publikowania aplikacji ASP.NET Core, nie jest konieczny dla aplikacji usług systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="cc9af-310">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="cc9af-311">Aby wyłączyć tworzenie pliku *Web. config* , należy dodać właściwość `<IsTransformWebConfigDisabled>` ustawioną na `true`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-311">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="cc9af-312">Wdrażanie samodzielne (SCD)</span><span class="sxs-lookup"><span data-stu-id="cc9af-312">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="cc9af-313">Wdrożenie samodzielne (SCD) nie polega na obecności struktury udostępnionej w systemie hosta.</span><span class="sxs-lookup"><span data-stu-id="cc9af-313">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="cc9af-314">Środowisko uruchomieniowe i zależności aplikacji są wdrażane przy użyciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-314">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="cc9af-315">[Identyfikator środowiska uruchomieniowego systemu Windows (RID)](/dotnet/core/rid-catalog) znajduje się w `<PropertyGroup>`, który zawiera platformę docelową:</span><span class="sxs-lookup"><span data-stu-id="cc9af-315">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="cc9af-316">Aby opublikować dla wielu identyfikatorów RID:</span><span class="sxs-lookup"><span data-stu-id="cc9af-316">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="cc9af-317">Podaj identyfikatory RID na liście rozdzielanej średnikami.</span><span class="sxs-lookup"><span data-stu-id="cc9af-317">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="cc9af-318">Użyj nazwy właściwości [\<RuntimeIdentifiers >](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span><span class="sxs-lookup"><span data-stu-id="cc9af-318">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="cc9af-319">Aby uzyskać więcej informacji, zobacz [katalog .NET Core RID Catalog](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="cc9af-319">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="cc9af-320">Właściwość `<SelfContained>` jest ustawiona na `true`:</span><span class="sxs-lookup"><span data-stu-id="cc9af-320">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

## <a name="service-user-account"></a><span data-ttu-id="cc9af-321">Konto użytkownika usługi</span><span class="sxs-lookup"><span data-stu-id="cc9af-321">Service user account</span></span>

<span data-ttu-id="cc9af-322">Aby utworzyć konto użytkownika dla usługi, należy użyć polecenia cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) w powłoce poleceń administracyjnych programu PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="cc9af-322">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="cc9af-323">W systemie Windows 10 października 2018 Update (wersja 1809/Kompilacja 10.0.17763) lub nowsza:</span><span class="sxs-lookup"><span data-stu-id="cc9af-323">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```powershell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="cc9af-324">W systemie operacyjnym Windows w wersji starszej niż aktualizacja 2018 systemu Windows 10 października (wersja 1809/Kompilacja 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="cc9af-324">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="cc9af-325">Podaj [silne hasło](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) po wyświetleniu monitu.</span><span class="sxs-lookup"><span data-stu-id="cc9af-325">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="cc9af-326">Jeśli parametr `-AccountExpires` nie zostanie dostarczony do polecenia cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) z <xref:System.DateTime>em wygaśnięcia, konto nie wygaśnie.</span><span class="sxs-lookup"><span data-stu-id="cc9af-326">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="cc9af-327">Aby uzyskać więcej informacji, zobacz [Microsoft. PowerShell. LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) i [konta użytkowników usług](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="cc9af-327">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="cc9af-328">Alternatywna metoda zarządzania użytkownikami podczas korzystania z Active Directory polega na użyciu zarządzanych kont usług.</span><span class="sxs-lookup"><span data-stu-id="cc9af-328">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="cc9af-329">Aby uzyskać więcej informacji, zobacz [Omówienie kont usług zarządzanych przez grupę](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="cc9af-329">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="cc9af-330">Logowanie jako prawa usługi</span><span class="sxs-lookup"><span data-stu-id="cc9af-330">Log on as a service rights</span></span>

<span data-ttu-id="cc9af-331">Aby nawiązać *Logowanie jako prawa usługi* dla konta użytkownika usługi:</span><span class="sxs-lookup"><span data-stu-id="cc9af-331">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="cc9af-332">Otwórz Edytor lokalnych zasad zabezpieczeń, uruchamiając program *secpol. msc*.</span><span class="sxs-lookup"><span data-stu-id="cc9af-332">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="cc9af-333">Rozwiń węzeł **Zasady lokalne** , a następnie wybierz pozycję **Przypisywanie praw użytkownika**.</span><span class="sxs-lookup"><span data-stu-id="cc9af-333">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="cc9af-334">Otwórz okno **Logowanie jako usługa** .</span><span class="sxs-lookup"><span data-stu-id="cc9af-334">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="cc9af-335">Wybierz pozycję **Dodaj użytkownika lub grupę**.</span><span class="sxs-lookup"><span data-stu-id="cc9af-335">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="cc9af-336">Podaj nazwę obiektu (konto użytkownika) przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="cc9af-336">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="cc9af-337">Wpisz konto użytkownika (`{DOMAIN OR COMPUTER NAME\USER}`) w polu Nazwa obiektu, a następnie wybierz **przycisk OK** , aby dodać użytkownika do zasad.</span><span class="sxs-lookup"><span data-stu-id="cc9af-337">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="cc9af-338">Wybierz pozycję **Zaawansowane**.</span><span class="sxs-lookup"><span data-stu-id="cc9af-338">Select **Advanced**.</span></span> <span data-ttu-id="cc9af-339">Wybierz pozycję **Znajdź teraz**.</span><span class="sxs-lookup"><span data-stu-id="cc9af-339">Select **Find Now**.</span></span> <span data-ttu-id="cc9af-340">Wybierz z listy konto użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cc9af-340">Select the user account from the list.</span></span> <span data-ttu-id="cc9af-341">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="cc9af-341">Select **OK**.</span></span> <span data-ttu-id="cc9af-342">Ponownie wybierz **przycisk OK** , aby dodać użytkownika do zasad.</span><span class="sxs-lookup"><span data-stu-id="cc9af-342">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="cc9af-343">Wybierz **przycisk OK** lub **Zastosuj** , aby zaakceptować zmiany.</span><span class="sxs-lookup"><span data-stu-id="cc9af-343">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="cc9af-344">Tworzenie usługi systemu Windows i zarządzanie nią</span><span class="sxs-lookup"><span data-stu-id="cc9af-344">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="cc9af-345">Tworzenie usługi</span><span class="sxs-lookup"><span data-stu-id="cc9af-345">Create a service</span></span>

<span data-ttu-id="cc9af-346">Zarejestrowanie usługi za pomocą poleceń programu PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cc9af-346">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="cc9af-347">Z poziomu powłoki poleceń administracyjnych programu PowerShell 6 wykonaj następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="cc9af-347">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="cc9af-348">`{EXE PATH}` &ndash; ścieżkę do folderu aplikacji na hoście (na przykład `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-348">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="cc9af-349">Nie dołączaj pliku wykonywalnego aplikacji do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="cc9af-349">Don't include the app's executable in the path.</span></span> <span data-ttu-id="cc9af-350">Końcowy ukośnik nie jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="cc9af-350">A trailing slash isn't required.</span></span>
* <span data-ttu-id="cc9af-351">konto użytkownika usługi &ndash; `{DOMAIN OR COMPUTER NAME\USER}` (na przykład `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-351">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="cc9af-352">`{SERVICE NAME}` &ndash; nazwę usługi (na przykład `MyService`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-352">`{SERVICE NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="cc9af-353">`{EXE FILE PATH}` &ndash; ścieżki pliku wykonywalnego aplikacji (na przykład `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-353">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="cc9af-354">Uwzględnij nazwę pliku wykonywalnego z rozszerzeniem.</span><span class="sxs-lookup"><span data-stu-id="cc9af-354">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="cc9af-355">`{DESCRIPTION}` &ndash; Opis usługi (na przykład `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-355">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="cc9af-356">Nazwa wyświetlana usługi `{DISPLAY NAME}` &ndash; (na przykład `My Service`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-356">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="cc9af-357">Uruchom usługę</span><span class="sxs-lookup"><span data-stu-id="cc9af-357">Start a service</span></span>

<span data-ttu-id="cc9af-358">Uruchom usługę za pomocą następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="cc9af-358">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="cc9af-359">Uruchomienie usługi może potrwać kilka sekund.</span><span class="sxs-lookup"><span data-stu-id="cc9af-359">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="cc9af-360">Określanie stanu usługi</span><span class="sxs-lookup"><span data-stu-id="cc9af-360">Determine a service's status</span></span>

<span data-ttu-id="cc9af-361">Aby sprawdzić stan usługi, użyj następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="cc9af-361">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="cc9af-362">Stan jest raportowany jako jedna z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="cc9af-362">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="cc9af-363">Zatrzymaj usługę</span><span class="sxs-lookup"><span data-stu-id="cc9af-363">Stop a service</span></span>

<span data-ttu-id="cc9af-364">Zatrzymaj usługę za pomocą następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="cc9af-364">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="cc9af-365">Usuwanie usługi</span><span class="sxs-lookup"><span data-stu-id="cc9af-365">Remove a service</span></span>

<span data-ttu-id="cc9af-366">Po krótkim opóźnieniu na zatrzymanie usługi Usuń usługę za pomocą następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="cc9af-366">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="cc9af-367">Obsługa zdarzeń uruchamiania i zatrzymywania</span><span class="sxs-lookup"><span data-stu-id="cc9af-367">Handle starting and stopping events</span></span>

<span data-ttu-id="cc9af-368">Aby obsłużyć zdarzenia <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>i <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>:</span><span class="sxs-lookup"><span data-stu-id="cc9af-368">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="cc9af-369">Utwórz klasę pochodzącą z <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> z metodami `OnStarting`, `OnStarted`i `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="cc9af-369">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="cc9af-370">Utwórz metodę rozszerzenia dla <xref:Microsoft.AspNetCore.Hosting.IWebHost>, która przekazuje `CustomWebHostService` do <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="cc9af-370">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="cc9af-371">W `Program.Main`Wywołaj metodę rozszerzenia `RunAsCustomService` zamiast <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="cc9af-371">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="cc9af-372">Aby zobaczyć lokalizację <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> w `Program.Main`, zapoznaj się z przykładem kodu pokazanym w sekcji [typ wdrożenia](#deployment-type) .</span><span class="sxs-lookup"><span data-stu-id="cc9af-372">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="cc9af-373">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="cc9af-373">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="cc9af-374">Usługi, które współdziałają z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub modułem równoważenia obciążenia, mogą wymagać dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-374">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="cc9af-375">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="cc9af-375">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="cc9af-376">Konfigurowanie punktów końcowych</span><span class="sxs-lookup"><span data-stu-id="cc9af-376">Configure endpoints</span></span>

<span data-ttu-id="cc9af-377">Domyślnie ASP.NET Core wiąże się z `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-377">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="cc9af-378">Skonfiguruj adres URL i port, ustawiając zmienną środowiskową `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-378">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="cc9af-379">Aby uzyskać dodatkowe podejścia do konfiguracji adresów URL i portów, zobacz artykuł dotyczący odpowiedniego serwera:</span><span class="sxs-lookup"><span data-stu-id="cc9af-379">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="cc9af-380">Powyższe wskazówki obejmują obsługę punktów końcowych HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cc9af-380">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="cc9af-381">Na przykład skonfiguruj aplikację do obsługi protokołu HTTPS, gdy uwierzytelnianie jest używane z usługą systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="cc9af-381">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="cc9af-382">Korzystanie z ASP.NET Core certyfikatu deweloperskiego HTTPS w celu zabezpieczenia punktu końcowego usługi nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="cc9af-382">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="cc9af-383">Bieżący katalog i główna Zawartość</span><span class="sxs-lookup"><span data-stu-id="cc9af-383">Current directory and content root</span></span>

<span data-ttu-id="cc9af-384">Bieżącym katalogiem roboczym zwróconym przez wywoływanie <xref:System.IO.Directory.GetCurrentDirectory*> dla usługi systemu Windows jest folder *C:\\Windows\\system32* .</span><span class="sxs-lookup"><span data-stu-id="cc9af-384">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="cc9af-385">Folder *system32* nie jest odpowiednią lokalizacją do przechowywania plików usługi (na przykład plików ustawień).</span><span class="sxs-lookup"><span data-stu-id="cc9af-385">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="cc9af-386">Użyj jednego z poniższych metod, aby zachować i uzyskać dostęp do plików ustawień i zasobów usługi.</span><span class="sxs-lookup"><span data-stu-id="cc9af-386">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="cc9af-387">Ustawianie ścieżki katalogu głównego zawartości do folderu aplikacji</span><span class="sxs-lookup"><span data-stu-id="cc9af-387">Set the content root path to the app's folder</span></span>

<span data-ttu-id="cc9af-388"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> jest tą samą ścieżką dostarczoną do argumentu `binPath` podczas tworzenia usługi.</span><span class="sxs-lookup"><span data-stu-id="cc9af-388">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="cc9af-389">Zamiast wywoływania `GetCurrentDirectory`, aby tworzyć ścieżki do plików ustawień, wywołaj <xref:System.IO.Directory.SetCurrentDirectory*> ze ścieżką do [katalogu głównego zawartości](xref:fundamentals/index#content-root)aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-389">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's [content root](xref:fundamentals/index#content-root).</span></span>

<span data-ttu-id="cc9af-390">W `Program.Main`określ ścieżkę do folderu wykonywalnego usługi i użyj ścieżki, aby określić katalog główny zawartości aplikacji:</span><span class="sxs-lookup"><span data-stu-id="cc9af-390">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="cc9af-391">Przechowywanie plików usługi w odpowiedniej lokalizacji na dysku</span><span class="sxs-lookup"><span data-stu-id="cc9af-391">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="cc9af-392">Określ ścieżkę bezwzględną <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> w przypadku używania <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do folderu zawierającego pliki.</span><span class="sxs-lookup"><span data-stu-id="cc9af-392">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="cc9af-393">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="cc9af-393">Troubleshoot</span></span>

<span data-ttu-id="cc9af-394">Aby rozwiązać problem z aplikacją usługi systemu Windows, zobacz <xref:test/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="cc9af-394">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="cc9af-395">Typowe błędy</span><span class="sxs-lookup"><span data-stu-id="cc9af-395">Common errors</span></span>

* <span data-ttu-id="cc9af-396">Starsza lub wstępnie wydana wersja programu PowerShell jest używana.</span><span class="sxs-lookup"><span data-stu-id="cc9af-396">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="cc9af-397">Zarejestrowana usługa nie używa **opublikowanych** danych wyjściowych aplikacji z [dotnet Publish](/dotnet/core/tools/dotnet-publish) polecenia.</span><span class="sxs-lookup"><span data-stu-id="cc9af-397">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="cc9af-398">Dane wyjściowe polecenia [kompilacji dotnet](/dotnet/core/tools/dotnet-build) nie są obsługiwane w przypadku wdrażania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-398">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="cc9af-399">Opublikowane zasoby znajdują się w dowolnym z następujących folderów w zależności od typu wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="cc9af-399">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="cc9af-400">*bin/Release/{Target Framework}/Publish* (FDD)</span><span class="sxs-lookup"><span data-stu-id="cc9af-400">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="cc9af-401">*bin/Release/{Target Framework}/{Runtime identyfikator}/Publish* (SCD)</span><span class="sxs-lookup"><span data-stu-id="cc9af-401">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="cc9af-402">Usługa nie jest w stanie uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="cc9af-402">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="cc9af-403">Ścieżki do zasobów używanych przez aplikację (na przykład certyfikaty) są nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="cc9af-403">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="cc9af-404">Ścieżką podstawową usługi systemu Windows jest *c:\\Windows\\system32*.</span><span class="sxs-lookup"><span data-stu-id="cc9af-404">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="cc9af-405">Użytkownik nie ma uprawnień do *logowania się jako usługa* .</span><span class="sxs-lookup"><span data-stu-id="cc9af-405">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="cc9af-406">Hasło użytkownika wygasło lub zostało nieprawidłowo przesłane podczas wykonywania polecenia `New-Service` PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cc9af-406">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="cc9af-407">Aplikacja wymaga uwierzytelniania ASP.NET Core, ale nie jest skonfigurowana dla połączeń Secure (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="cc9af-407">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="cc9af-408">Port adresu URL żądania jest niepoprawny lub niepoprawnie skonfigurowany w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-408">The request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="cc9af-409">Dzienniki zdarzeń systemu i aplikacji</span><span class="sxs-lookup"><span data-stu-id="cc9af-409">System and Application Event Logs</span></span>

<span data-ttu-id="cc9af-410">Uzyskaj dostęp do dzienników zdarzeń systemu i aplikacji:</span><span class="sxs-lookup"><span data-stu-id="cc9af-410">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="cc9af-411">Otwórz menu Start, wyszukaj ciąg *Podgląd zdarzeń*i wybierz aplikację **Podgląd zdarzeń** .</span><span class="sxs-lookup"><span data-stu-id="cc9af-411">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="cc9af-412">W **Podgląd zdarzeń**Otwórz węzeł **Dzienniki systemu Windows** .</span><span class="sxs-lookup"><span data-stu-id="cc9af-412">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="cc9af-413">Wybierz pozycję **system** , aby otworzyć dziennik zdarzeń systemu.</span><span class="sxs-lookup"><span data-stu-id="cc9af-413">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="cc9af-414">Wybierz pozycję **aplikacja** , aby otworzyć dziennik zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-414">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="cc9af-415">Wyszukaj błędy skojarzone z aplikacją się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="cc9af-415">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="cc9af-416">Uruchamianie aplikacji w wierszu polecenia</span><span class="sxs-lookup"><span data-stu-id="cc9af-416">Run the app at a command prompt</span></span>

<span data-ttu-id="cc9af-417">Wiele błędów uruchamiania nie produkuje użytecznych informacji w dziennikach zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cc9af-417">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="cc9af-418">Przyczyną niektórych błędów można znaleźć, uruchamiając aplikację w wierszu polecenia w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="cc9af-418">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="cc9af-419">Aby rejestrować dodatkowe szczegóły aplikacji, Obniż [poziom rejestrowania](xref:fundamentals/logging/index#log-level) lub Uruchom aplikację w [środowisku deweloperskim](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="cc9af-419">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="cc9af-420">Wyczyść pamięć podręczną pakietów</span><span class="sxs-lookup"><span data-stu-id="cc9af-420">Clear package caches</span></span>

<span data-ttu-id="cc9af-421">Działająca aplikacja może zakończyć się niepowodzeniem natychmiast po uaktualnieniu zestaw .NET Core SDK na komputerze deweloperskim lub zmianie wersji pakietu w ramach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-421">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="cc9af-422">W niektórych przypadkach niespójne pakietów może spowodować uszkodzenie aplikacji podczas przeprowadzania uaktualnienia głównych.</span><span class="sxs-lookup"><span data-stu-id="cc9af-422">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="cc9af-423">Większość z tych problemów można naprawić, wykonując następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="cc9af-423">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="cc9af-424">Usuń foldery *bin* i *obj* .</span><span class="sxs-lookup"><span data-stu-id="cc9af-424">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="cc9af-425">Wyczyść pamięć podręczną pakietów, wykonując [wszystkie elementy lokalne usługi NuGet programu dotnet--Wyczyść](/dotnet/core/tools/dotnet-nuget-locals) z poziomu powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="cc9af-425">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="cc9af-426">Czyszczenie pamięci podręcznych pakietów można również wykonać za pomocą narzędzia [NuGet. exe](https://www.nuget.org/downloads) i wykonując polecenie `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-426">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="cc9af-427">*NuGet. exe* nie jest pakietem instalowanym z systemem operacyjnym Windows dla komputerów stacjonarnych i musi być uzyskany niezależnie od [witryny sieci Web programu NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="cc9af-427">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="cc9af-428">Przywróć i skompiluj ponownie projekt.</span><span class="sxs-lookup"><span data-stu-id="cc9af-428">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="cc9af-429">Usuń wszystkie pliki z folderu wdrożenia na serwerze przed ponownym wdrożeniem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-429">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="cc9af-430">Wolne lub zwisa aplikacji</span><span class="sxs-lookup"><span data-stu-id="cc9af-430">Slow or hanging app</span></span>

<span data-ttu-id="cc9af-431">*Zrzut awaryjny* to migawka pamięci systemu, która może pomóc w ustaleniu przyczyny awarii aplikacji, awarii uruchamiania lub powolnej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-431">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="cc9af-432">Awaria aplikacji lub napotka wyjątek</span><span class="sxs-lookup"><span data-stu-id="cc9af-432">App crashes or encounters an exception</span></span>

<span data-ttu-id="cc9af-433">Uzyskaj i Analizuj Zrzut z [raportowanie błędów systemu Windows (raportowanie błędów systemu Windows)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="cc9af-433">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="cc9af-434">Utwórz folder do przechowywania plików zrzutu awaryjnego w `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-434">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="cc9af-435">Uruchom [skrypt programu PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) w programie EnableDumps z nazwą pliku wykonywalnego aplikacji:</span><span class="sxs-lookup"><span data-stu-id="cc9af-435">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```console
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="cc9af-436">Uruchom aplikację w warunkach, które powodują awarię.</span><span class="sxs-lookup"><span data-stu-id="cc9af-436">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="cc9af-437">Po wystąpieniu awarii Uruchom [skrypt programu DisableDumps PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="cc9af-437">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span></span>

   ```console
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="cc9af-438">Po awarii aplikacji i zakończeniu zbierania zrzutów aplikacja może zakończyć normalne działanie.</span><span class="sxs-lookup"><span data-stu-id="cc9af-438">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="cc9af-439">Skrypt programu PowerShell konfiguruje raportowanie błędów systemu Windows w celu zebrania do pięciu zrzutów na aplikację.</span><span class="sxs-lookup"><span data-stu-id="cc9af-439">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="cc9af-440">Zrzuty awaryjne mogą wymagać dużej ilości miejsca na dysku (do kilku gigabajtów).</span><span class="sxs-lookup"><span data-stu-id="cc9af-440">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="cc9af-441">Aplikacja zawiesza się, kończy się niepowodzeniem podczas uruchamiania lub działa normalnie</span><span class="sxs-lookup"><span data-stu-id="cc9af-441">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="cc9af-442">Gdy aplikacja *zawiesza* się (bez awarii), kończy się niepowodzeniem podczas uruchamiania lub działa normalnie, zobacz [pliki zrzutu w trybie użytkownika: wybór najlepszego narzędzia](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) do wygenerowania zrzutu.</span><span class="sxs-lookup"><span data-stu-id="cc9af-442">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="cc9af-443">Analizowanie zrzutu</span><span class="sxs-lookup"><span data-stu-id="cc9af-443">Analyze the dump</span></span>

<span data-ttu-id="cc9af-444">Zrzut można analizować przy użyciu kilku metod.</span><span class="sxs-lookup"><span data-stu-id="cc9af-444">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="cc9af-445">Aby uzyskać więcej informacji, zobacz [Analizowanie pliku zrzutu w trybie użytkownika](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="cc9af-445">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cc9af-446">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cc9af-446">Additional resources</span></span>

* <span data-ttu-id="cc9af-447">[Konfiguracja punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym Konfiguracja protokołu HTTPS i obsługa SNI)</span><span class="sxs-lookup"><span data-stu-id="cc9af-447">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="cc9af-448">Aplikacja ASP.NET Core może być hostowana w systemie Windows jako [Usługa systemu Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) bez korzystania z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="cc9af-448">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="cc9af-449">Gdy usługa jest hostowana w systemie Windows, aplikacja jest uruchamiana automatycznie po ponownym uruchomieniu serwera.</span><span class="sxs-lookup"><span data-stu-id="cc9af-449">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="cc9af-450">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cc9af-450">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc9af-451">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="cc9af-451">Prerequisites</span></span>

* [<span data-ttu-id="cc9af-452">ASP.NET Core zestaw SDK 2,1 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="cc9af-452">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="cc9af-453">Program PowerShell 6,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="cc9af-453">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="app-configuration"></a><span data-ttu-id="cc9af-454">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="cc9af-454">App configuration</span></span>

<span data-ttu-id="cc9af-455">Aplikacja wymaga odwołania do pakietów dla elementu [Microsoft. AspNetCore. hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) i [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="cc9af-455">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="cc9af-456">Aby przetestować i debugować w przypadku uruchamiania poza usługą, należy dodać kod, aby określić, czy aplikacja działa jako usługa, czy Aplikacja konsolowa.</span><span class="sxs-lookup"><span data-stu-id="cc9af-456">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="cc9af-457">Sprawdź, czy debuger jest podłączony lub czy jest dostępny przełącznik `--console`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-457">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="cc9af-458">Jeśli którykolwiek z warunków jest spełniony (aplikacja nie jest uruchomiona jako usługa), wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="cc9af-458">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="cc9af-459">Jeśli warunki są fałszywe (aplikacja jest uruchamiana jako usługa):</span><span class="sxs-lookup"><span data-stu-id="cc9af-459">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="cc9af-460">Wywołaj <xref:System.IO.Directory.SetCurrentDirectory*> i użyj ścieżki do lokalizacji opublikowanej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-460">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="cc9af-461">Nie wywołuj <xref:System.IO.Directory.GetCurrentDirectory*>, aby uzyskać ścieżkę, ponieważ aplikacja usługi systemu Windows zwraca folder *C:\\Windows\\system32* w przypadku wywołania <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="cc9af-461">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="cc9af-462">Aby uzyskać więcej informacji, zapoznaj się z sekcją [Current Directory i root Content](#current-directory-and-content-root) .</span><span class="sxs-lookup"><span data-stu-id="cc9af-462">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="cc9af-463">Ten krok jest wykonywany przed skonfigurowaniem aplikacji w `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-463">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="cc9af-464">Wywołaj <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>, aby uruchomić aplikację jako usługę.</span><span class="sxs-lookup"><span data-stu-id="cc9af-464">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="cc9af-465">Ponieważ [dostawca konfiguracji wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) wymaga par nazwa-wartość dla argumentów wiersza polecenia, przełącznik `--console` jest usuwany z argumentów, zanim <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> odbierze argumenty.</span><span class="sxs-lookup"><span data-stu-id="cc9af-465">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="cc9af-466">Aby zapisać w dzienniku zdarzeń systemu Windows, Dodaj dostawcę EventLog do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="cc9af-466">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="cc9af-467">Ustaw poziom rejestrowania za pomocą klucza `Logging:LogLevel:Default` w pliku *appSettings. Plik Product. JSON* .</span><span class="sxs-lookup"><span data-stu-id="cc9af-467">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="cc9af-468">W poniższym przykładzie z przykładowej aplikacji `RunAsCustomService` jest wywoływana zamiast <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> w celu obsługi zdarzeń okresu istnienia w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-468">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="cc9af-469">Aby uzyskać więcej informacji, zobacz sekcję [Obsługa zdarzeń dotyczących uruchamiania i zatrzymywania](#handle-starting-and-stopping-events) .</span><span class="sxs-lookup"><span data-stu-id="cc9af-469">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="deployment-type"></a><span data-ttu-id="cc9af-470">Typ wdrożenia</span><span class="sxs-lookup"><span data-stu-id="cc9af-470">Deployment type</span></span>

<span data-ttu-id="cc9af-471">Aby uzyskać informacje i porady dotyczące scenariuszy wdrażania, zobacz [wdrażanie aplikacji .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="cc9af-471">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="cc9af-472">SDK</span><span class="sxs-lookup"><span data-stu-id="cc9af-472">SDK</span></span>

<span data-ttu-id="cc9af-473">W przypadku usługi opartej na aplikacji sieci Web używającej platform Razor Pages lub MVC należy określić zestaw SDK sieci Web w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="cc9af-473">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="cc9af-474">Jeśli usługa wykonuje tylko zadania w tle (na przykład [usługi hostowane](xref:fundamentals/host/hosted-services)), określ zestaw SDK procesu roboczego w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="cc9af-474">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="cc9af-475">Wdrożenie zależne od platformy (FDD)</span><span class="sxs-lookup"><span data-stu-id="cc9af-475">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="cc9af-476">Wdrożenie zależne od platformy (FDD) zależy od obecności udostępnionej systemowej wersji platformy .NET Core w systemie docelowym.</span><span class="sxs-lookup"><span data-stu-id="cc9af-476">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="cc9af-477">Po przyjęciu scenariusza FDD zgodnie ze wskazówkami zawartymi w tym artykule zestaw SDK tworzy plik wykonywalny ( *. exe*), nazywany *plik wykonywalny zależny od platformy*.</span><span class="sxs-lookup"><span data-stu-id="cc9af-477">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

<span data-ttu-id="cc9af-478">[Identyfikator środowiska uruchomieniowego systemu Windows (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) zawiera platformę docelową.</span><span class="sxs-lookup"><span data-stu-id="cc9af-478">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="cc9af-479">W poniższym przykładzie identyfikator RID jest ustawiony na `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-479">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="cc9af-480">Właściwość `<SelfContained>` jest ustawiona na `false`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-480">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="cc9af-481">Te właściwości instruują zestaw SDK, aby wygenerował plik wykonywalny (*exe*) dla systemu Windows i aplikację, która zależy od współużytkowanej platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc9af-481">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="cc9af-482">Właściwość `<UseAppHost>` jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-482">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="cc9af-483">Ta właściwość zapewnia usługę ze ścieżką aktywacji (plik wykonywalny, *exe*) dla FDD.</span><span class="sxs-lookup"><span data-stu-id="cc9af-483">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="cc9af-484">Plik *Web. config* , który jest zwykle tworzony podczas publikowania aplikacji ASP.NET Core, nie jest konieczny dla aplikacji usług systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="cc9af-484">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="cc9af-485">Aby wyłączyć tworzenie pliku *Web. config* , należy dodać właściwość `<IsTransformWebConfigDisabled>` ustawioną na `true`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-485">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="cc9af-486">Wdrażanie samodzielne (SCD)</span><span class="sxs-lookup"><span data-stu-id="cc9af-486">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="cc9af-487">Wdrożenie samodzielne (SCD) nie polega na obecności struktury udostępnionej w systemie hosta.</span><span class="sxs-lookup"><span data-stu-id="cc9af-487">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="cc9af-488">Środowisko uruchomieniowe i zależności aplikacji są wdrażane przy użyciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-488">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="cc9af-489">[Identyfikator środowiska uruchomieniowego systemu Windows (RID)](/dotnet/core/rid-catalog) znajduje się w `<PropertyGroup>`, który zawiera platformę docelową:</span><span class="sxs-lookup"><span data-stu-id="cc9af-489">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="cc9af-490">Aby opublikować dla wielu identyfikatorów RID:</span><span class="sxs-lookup"><span data-stu-id="cc9af-490">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="cc9af-491">Podaj identyfikatory RID na liście rozdzielanej średnikami.</span><span class="sxs-lookup"><span data-stu-id="cc9af-491">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="cc9af-492">Użyj nazwy właściwości [\<RuntimeIdentifiers >](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span><span class="sxs-lookup"><span data-stu-id="cc9af-492">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="cc9af-493">Aby uzyskać więcej informacji, zobacz [katalog .NET Core RID Catalog](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="cc9af-493">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="cc9af-494">Właściwość `<SelfContained>` jest ustawiona na `true`:</span><span class="sxs-lookup"><span data-stu-id="cc9af-494">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

## <a name="service-user-account"></a><span data-ttu-id="cc9af-495">Konto użytkownika usługi</span><span class="sxs-lookup"><span data-stu-id="cc9af-495">Service user account</span></span>

<span data-ttu-id="cc9af-496">Aby utworzyć konto użytkownika dla usługi, należy użyć polecenia cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) w powłoce poleceń administracyjnych programu PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="cc9af-496">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="cc9af-497">W systemie Windows 10 października 2018 Update (wersja 1809/Kompilacja 10.0.17763) lub nowsza:</span><span class="sxs-lookup"><span data-stu-id="cc9af-497">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```powershell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="cc9af-498">W systemie operacyjnym Windows w wersji starszej niż aktualizacja 2018 systemu Windows 10 października (wersja 1809/Kompilacja 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="cc9af-498">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="cc9af-499">Podaj [silne hasło](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) po wyświetleniu monitu.</span><span class="sxs-lookup"><span data-stu-id="cc9af-499">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="cc9af-500">Jeśli parametr `-AccountExpires` nie zostanie dostarczony do polecenia cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) z <xref:System.DateTime>em wygaśnięcia, konto nie wygaśnie.</span><span class="sxs-lookup"><span data-stu-id="cc9af-500">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="cc9af-501">Aby uzyskać więcej informacji, zobacz [Microsoft. PowerShell. LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) i [konta użytkowników usług](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="cc9af-501">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="cc9af-502">Alternatywna metoda zarządzania użytkownikami podczas korzystania z Active Directory polega na użyciu zarządzanych kont usług.</span><span class="sxs-lookup"><span data-stu-id="cc9af-502">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="cc9af-503">Aby uzyskać więcej informacji, zobacz [Omówienie kont usług zarządzanych przez grupę](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="cc9af-503">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="cc9af-504">Logowanie jako prawa usługi</span><span class="sxs-lookup"><span data-stu-id="cc9af-504">Log on as a service rights</span></span>

<span data-ttu-id="cc9af-505">Aby nawiązać *Logowanie jako prawa usługi* dla konta użytkownika usługi:</span><span class="sxs-lookup"><span data-stu-id="cc9af-505">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="cc9af-506">Otwórz Edytor lokalnych zasad zabezpieczeń, uruchamiając program *secpol. msc*.</span><span class="sxs-lookup"><span data-stu-id="cc9af-506">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="cc9af-507">Rozwiń węzeł **Zasady lokalne** , a następnie wybierz pozycję **Przypisywanie praw użytkownika**.</span><span class="sxs-lookup"><span data-stu-id="cc9af-507">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="cc9af-508">Otwórz okno **Logowanie jako usługa** .</span><span class="sxs-lookup"><span data-stu-id="cc9af-508">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="cc9af-509">Wybierz pozycję **Dodaj użytkownika lub grupę**.</span><span class="sxs-lookup"><span data-stu-id="cc9af-509">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="cc9af-510">Podaj nazwę obiektu (konto użytkownika) przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="cc9af-510">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="cc9af-511">Wpisz konto użytkownika (`{DOMAIN OR COMPUTER NAME\USER}`) w polu Nazwa obiektu, a następnie wybierz **przycisk OK** , aby dodać użytkownika do zasad.</span><span class="sxs-lookup"><span data-stu-id="cc9af-511">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="cc9af-512">Wybierz pozycję **Zaawansowane**.</span><span class="sxs-lookup"><span data-stu-id="cc9af-512">Select **Advanced**.</span></span> <span data-ttu-id="cc9af-513">Wybierz pozycję **Znajdź teraz**.</span><span class="sxs-lookup"><span data-stu-id="cc9af-513">Select **Find Now**.</span></span> <span data-ttu-id="cc9af-514">Wybierz z listy konto użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cc9af-514">Select the user account from the list.</span></span> <span data-ttu-id="cc9af-515">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="cc9af-515">Select **OK**.</span></span> <span data-ttu-id="cc9af-516">Ponownie wybierz **przycisk OK** , aby dodać użytkownika do zasad.</span><span class="sxs-lookup"><span data-stu-id="cc9af-516">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="cc9af-517">Wybierz **przycisk OK** lub **Zastosuj** , aby zaakceptować zmiany.</span><span class="sxs-lookup"><span data-stu-id="cc9af-517">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="cc9af-518">Tworzenie usługi systemu Windows i zarządzanie nią</span><span class="sxs-lookup"><span data-stu-id="cc9af-518">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="cc9af-519">Tworzenie usługi</span><span class="sxs-lookup"><span data-stu-id="cc9af-519">Create a service</span></span>

<span data-ttu-id="cc9af-520">Zarejestrowanie usługi za pomocą poleceń programu PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cc9af-520">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="cc9af-521">Z poziomu powłoki poleceń administracyjnych programu PowerShell 6 wykonaj następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="cc9af-521">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="cc9af-522">`{EXE PATH}` &ndash; ścieżkę do folderu aplikacji na hoście (na przykład `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-522">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="cc9af-523">Nie dołączaj pliku wykonywalnego aplikacji do ścieżki.</span><span class="sxs-lookup"><span data-stu-id="cc9af-523">Don't include the app's executable in the path.</span></span> <span data-ttu-id="cc9af-524">Końcowy ukośnik nie jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="cc9af-524">A trailing slash isn't required.</span></span>
* <span data-ttu-id="cc9af-525">konto użytkownika usługi &ndash; `{DOMAIN OR COMPUTER NAME\USER}` (na przykład `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-525">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="cc9af-526">`{SERVICE NAME}` &ndash; nazwę usługi (na przykład `MyService`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-526">`{SERVICE NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="cc9af-527">`{EXE FILE PATH}` &ndash; ścieżki pliku wykonywalnego aplikacji (na przykład `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-527">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="cc9af-528">Uwzględnij nazwę pliku wykonywalnego z rozszerzeniem.</span><span class="sxs-lookup"><span data-stu-id="cc9af-528">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="cc9af-529">`{DESCRIPTION}` &ndash; Opis usługi (na przykład `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-529">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="cc9af-530">Nazwa wyświetlana usługi `{DISPLAY NAME}` &ndash; (na przykład `My Service`).</span><span class="sxs-lookup"><span data-stu-id="cc9af-530">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="cc9af-531">Uruchom usługę</span><span class="sxs-lookup"><span data-stu-id="cc9af-531">Start a service</span></span>

<span data-ttu-id="cc9af-532">Uruchom usługę za pomocą następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="cc9af-532">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="cc9af-533">Uruchomienie usługi może potrwać kilka sekund.</span><span class="sxs-lookup"><span data-stu-id="cc9af-533">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="cc9af-534">Określanie stanu usługi</span><span class="sxs-lookup"><span data-stu-id="cc9af-534">Determine a service's status</span></span>

<span data-ttu-id="cc9af-535">Aby sprawdzić stan usługi, użyj następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="cc9af-535">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="cc9af-536">Stan jest raportowany jako jedna z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="cc9af-536">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="cc9af-537">Zatrzymaj usługę</span><span class="sxs-lookup"><span data-stu-id="cc9af-537">Stop a service</span></span>

<span data-ttu-id="cc9af-538">Zatrzymaj usługę za pomocą następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="cc9af-538">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="cc9af-539">Usuwanie usługi</span><span class="sxs-lookup"><span data-stu-id="cc9af-539">Remove a service</span></span>

<span data-ttu-id="cc9af-540">Po krótkim opóźnieniu na zatrzymanie usługi Usuń usługę za pomocą następującego polecenia programu PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="cc9af-540">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="cc9af-541">Obsługa zdarzeń uruchamiania i zatrzymywania</span><span class="sxs-lookup"><span data-stu-id="cc9af-541">Handle starting and stopping events</span></span>

<span data-ttu-id="cc9af-542">Aby obsłużyć zdarzenia <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>i <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>:</span><span class="sxs-lookup"><span data-stu-id="cc9af-542">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="cc9af-543">Utwórz klasę pochodzącą z <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> z metodami `OnStarting`, `OnStarted`i `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="cc9af-543">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="cc9af-544">Utwórz metodę rozszerzenia dla <xref:Microsoft.AspNetCore.Hosting.IWebHost>, która przekazuje `CustomWebHostService` do <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="cc9af-544">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="cc9af-545">W `Program.Main`Wywołaj metodę rozszerzenia `RunAsCustomService` zamiast <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="cc9af-545">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="cc9af-546">Aby zobaczyć lokalizację <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> w `Program.Main`, zapoznaj się z przykładem kodu pokazanym w sekcji [typ wdrożenia](#deployment-type) .</span><span class="sxs-lookup"><span data-stu-id="cc9af-546">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="cc9af-547">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="cc9af-547">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="cc9af-548">Usługi, które współdziałają z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub modułem równoważenia obciążenia, mogą wymagać dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-548">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="cc9af-549">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="cc9af-549">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="cc9af-550">Konfigurowanie punktów końcowych</span><span class="sxs-lookup"><span data-stu-id="cc9af-550">Configure endpoints</span></span>

<span data-ttu-id="cc9af-551">Domyślnie ASP.NET Core wiąże się z `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-551">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="cc9af-552">Skonfiguruj adres URL i port, ustawiając zmienną środowiskową `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-552">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="cc9af-553">Aby uzyskać dodatkowe podejścia do konfiguracji adresów URL i portów, zobacz artykuł dotyczący odpowiedniego serwera:</span><span class="sxs-lookup"><span data-stu-id="cc9af-553">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="cc9af-554">Powyższe wskazówki obejmują obsługę punktów końcowych HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cc9af-554">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="cc9af-555">Na przykład skonfiguruj aplikację do obsługi protokołu HTTPS, gdy uwierzytelnianie jest używane z usługą systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="cc9af-555">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="cc9af-556">Korzystanie z ASP.NET Core certyfikatu deweloperskiego HTTPS w celu zabezpieczenia punktu końcowego usługi nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="cc9af-556">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="cc9af-557">Bieżący katalog i główna Zawartość</span><span class="sxs-lookup"><span data-stu-id="cc9af-557">Current directory and content root</span></span>

<span data-ttu-id="cc9af-558">Bieżącym katalogiem roboczym zwróconym przez wywoływanie <xref:System.IO.Directory.GetCurrentDirectory*> dla usługi systemu Windows jest folder *C:\\Windows\\system32* .</span><span class="sxs-lookup"><span data-stu-id="cc9af-558">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="cc9af-559">Folder *system32* nie jest odpowiednią lokalizacją do przechowywania plików usługi (na przykład plików ustawień).</span><span class="sxs-lookup"><span data-stu-id="cc9af-559">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="cc9af-560">Użyj jednego z poniższych metod, aby zachować i uzyskać dostęp do plików ustawień i zasobów usługi.</span><span class="sxs-lookup"><span data-stu-id="cc9af-560">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="cc9af-561">Ustawianie ścieżki katalogu głównego zawartości do folderu aplikacji</span><span class="sxs-lookup"><span data-stu-id="cc9af-561">Set the content root path to the app's folder</span></span>

<span data-ttu-id="cc9af-562"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> jest tą samą ścieżką dostarczoną do argumentu `binPath` podczas tworzenia usługi.</span><span class="sxs-lookup"><span data-stu-id="cc9af-562">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="cc9af-563">Zamiast wywoływania `GetCurrentDirectory`, aby tworzyć ścieżki do plików ustawień, wywołaj <xref:System.IO.Directory.SetCurrentDirectory*> ze ścieżką do [katalogu głównego zawartości](xref:fundamentals/index#content-root)aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-563">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's [content root](xref:fundamentals/index#content-root).</span></span>

<span data-ttu-id="cc9af-564">W `Program.Main`określ ścieżkę do folderu wykonywalnego usługi i użyj ścieżki, aby określić katalog główny zawartości aplikacji:</span><span class="sxs-lookup"><span data-stu-id="cc9af-564">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="cc9af-565">Przechowywanie plików usługi w odpowiedniej lokalizacji na dysku</span><span class="sxs-lookup"><span data-stu-id="cc9af-565">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="cc9af-566">Określ ścieżkę bezwzględną <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> w przypadku używania <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do folderu zawierającego pliki.</span><span class="sxs-lookup"><span data-stu-id="cc9af-566">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="cc9af-567">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="cc9af-567">Troubleshoot</span></span>

<span data-ttu-id="cc9af-568">Aby rozwiązać problem z aplikacją usługi systemu Windows, zobacz <xref:test/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="cc9af-568">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="cc9af-569">Typowe błędy</span><span class="sxs-lookup"><span data-stu-id="cc9af-569">Common errors</span></span>

* <span data-ttu-id="cc9af-570">Starsza lub wstępnie wydana wersja programu PowerShell jest używana.</span><span class="sxs-lookup"><span data-stu-id="cc9af-570">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="cc9af-571">Zarejestrowana usługa nie używa **opublikowanych** danych wyjściowych aplikacji z [dotnet Publish](/dotnet/core/tools/dotnet-publish) polecenia.</span><span class="sxs-lookup"><span data-stu-id="cc9af-571">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="cc9af-572">Dane wyjściowe polecenia [kompilacji dotnet](/dotnet/core/tools/dotnet-build) nie są obsługiwane w przypadku wdrażania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-572">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="cc9af-573">Opublikowane zasoby znajdują się w dowolnym z następujących folderów w zależności od typu wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="cc9af-573">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="cc9af-574">*bin/Release/{Target Framework}/Publish* (FDD)</span><span class="sxs-lookup"><span data-stu-id="cc9af-574">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="cc9af-575">*bin/Release/{Target Framework}/{Runtime identyfikator}/Publish* (SCD)</span><span class="sxs-lookup"><span data-stu-id="cc9af-575">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="cc9af-576">Usługa nie jest w stanie uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="cc9af-576">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="cc9af-577">Ścieżki do zasobów używanych przez aplikację (na przykład certyfikaty) są nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="cc9af-577">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="cc9af-578">Ścieżką podstawową usługi systemu Windows jest *c:\\Windows\\system32*.</span><span class="sxs-lookup"><span data-stu-id="cc9af-578">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="cc9af-579">Użytkownik nie ma uprawnień do *logowania się jako usługa* .</span><span class="sxs-lookup"><span data-stu-id="cc9af-579">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="cc9af-580">Hasło użytkownika wygasło lub zostało nieprawidłowo przesłane podczas wykonywania polecenia `New-Service` PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cc9af-580">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="cc9af-581">Aplikacja wymaga uwierzytelniania ASP.NET Core, ale nie jest skonfigurowana dla połączeń Secure (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="cc9af-581">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="cc9af-582">Port adresu URL żądania jest niepoprawny lub niepoprawnie skonfigurowany w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-582">The request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="cc9af-583">Dzienniki zdarzeń systemu i aplikacji</span><span class="sxs-lookup"><span data-stu-id="cc9af-583">System and Application Event Logs</span></span>

<span data-ttu-id="cc9af-584">Uzyskaj dostęp do dzienników zdarzeń systemu i aplikacji:</span><span class="sxs-lookup"><span data-stu-id="cc9af-584">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="cc9af-585">Otwórz menu Start, wyszukaj ciąg *Podgląd zdarzeń*i wybierz aplikację **Podgląd zdarzeń** .</span><span class="sxs-lookup"><span data-stu-id="cc9af-585">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="cc9af-586">W **Podgląd zdarzeń**Otwórz węzeł **Dzienniki systemu Windows** .</span><span class="sxs-lookup"><span data-stu-id="cc9af-586">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="cc9af-587">Wybierz pozycję **system** , aby otworzyć dziennik zdarzeń systemu.</span><span class="sxs-lookup"><span data-stu-id="cc9af-587">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="cc9af-588">Wybierz pozycję **aplikacja** , aby otworzyć dziennik zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-588">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="cc9af-589">Wyszukaj błędy skojarzone z aplikacją się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="cc9af-589">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="cc9af-590">Uruchamianie aplikacji w wierszu polecenia</span><span class="sxs-lookup"><span data-stu-id="cc9af-590">Run the app at a command prompt</span></span>

<span data-ttu-id="cc9af-591">Wiele błędów uruchamiania nie produkuje użytecznych informacji w dziennikach zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cc9af-591">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="cc9af-592">Przyczyną niektórych błędów można znaleźć, uruchamiając aplikację w wierszu polecenia w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="cc9af-592">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="cc9af-593">Aby rejestrować dodatkowe szczegóły aplikacji, Obniż [poziom rejestrowania](xref:fundamentals/logging/index#log-level) lub Uruchom aplikację w [środowisku deweloperskim](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="cc9af-593">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="cc9af-594">Wyczyść pamięć podręczną pakietów</span><span class="sxs-lookup"><span data-stu-id="cc9af-594">Clear package caches</span></span>

<span data-ttu-id="cc9af-595">Działająca aplikacja może zakończyć się niepowodzeniem natychmiast po uaktualnieniu zestaw .NET Core SDK na komputerze deweloperskim lub zmianie wersji pakietu w ramach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-595">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="cc9af-596">W niektórych przypadkach niespójne pakietów może spowodować uszkodzenie aplikacji podczas przeprowadzania uaktualnienia głównych.</span><span class="sxs-lookup"><span data-stu-id="cc9af-596">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="cc9af-597">Większość z tych problemów można naprawić, wykonując następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="cc9af-597">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="cc9af-598">Usuń foldery *bin* i *obj* .</span><span class="sxs-lookup"><span data-stu-id="cc9af-598">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="cc9af-599">Wyczyść pamięć podręczną pakietów, wykonując [wszystkie elementy lokalne usługi NuGet programu dotnet--Wyczyść](/dotnet/core/tools/dotnet-nuget-locals) z poziomu powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="cc9af-599">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="cc9af-600">Czyszczenie pamięci podręcznych pakietów można również wykonać za pomocą narzędzia [NuGet. exe](https://www.nuget.org/downloads) i wykonując polecenie `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-600">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="cc9af-601">*NuGet. exe* nie jest pakietem instalowanym z systemem operacyjnym Windows dla komputerów stacjonarnych i musi być uzyskany niezależnie od [witryny sieci Web programu NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="cc9af-601">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="cc9af-602">Przywróć i skompiluj ponownie projekt.</span><span class="sxs-lookup"><span data-stu-id="cc9af-602">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="cc9af-603">Usuń wszystkie pliki z folderu wdrożenia na serwerze przed ponownym wdrożeniem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-603">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="cc9af-604">Wolne lub zwisa aplikacji</span><span class="sxs-lookup"><span data-stu-id="cc9af-604">Slow or hanging app</span></span>

<span data-ttu-id="cc9af-605">*Zrzut awaryjny* to migawka pamięci systemu, która może pomóc w ustaleniu przyczyny awarii aplikacji, awarii uruchamiania lub powolnej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc9af-605">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="cc9af-606">Awaria aplikacji lub napotka wyjątek</span><span class="sxs-lookup"><span data-stu-id="cc9af-606">App crashes or encounters an exception</span></span>

<span data-ttu-id="cc9af-607">Uzyskaj i Analizuj Zrzut z [raportowanie błędów systemu Windows (raportowanie błędów systemu Windows)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="cc9af-607">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="cc9af-608">Utwórz folder do przechowywania plików zrzutu awaryjnego w `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="cc9af-608">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="cc9af-609">Uruchom [skrypt programu PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) w programie EnableDumps z nazwą pliku wykonywalnego aplikacji:</span><span class="sxs-lookup"><span data-stu-id="cc9af-609">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```console
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="cc9af-610">Uruchom aplikację w warunkach, które powodują awarię.</span><span class="sxs-lookup"><span data-stu-id="cc9af-610">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="cc9af-611">Po wystąpieniu awarii Uruchom [skrypt programu DisableDumps PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="cc9af-611">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span></span>

   ```console
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="cc9af-612">Po awarii aplikacji i zakończeniu zbierania zrzutów aplikacja może zakończyć normalne działanie.</span><span class="sxs-lookup"><span data-stu-id="cc9af-612">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="cc9af-613">Skrypt programu PowerShell konfiguruje raportowanie błędów systemu Windows w celu zebrania do pięciu zrzutów na aplikację.</span><span class="sxs-lookup"><span data-stu-id="cc9af-613">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="cc9af-614">Zrzuty awaryjne mogą wymagać dużej ilości miejsca na dysku (do kilku gigabajtów).</span><span class="sxs-lookup"><span data-stu-id="cc9af-614">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="cc9af-615">Aplikacja zawiesza się, kończy się niepowodzeniem podczas uruchamiania lub działa normalnie</span><span class="sxs-lookup"><span data-stu-id="cc9af-615">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="cc9af-616">Gdy aplikacja *zawiesza* się (bez awarii), kończy się niepowodzeniem podczas uruchamiania lub działa normalnie, zobacz [pliki zrzutu w trybie użytkownika: wybór najlepszego narzędzia](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) do wygenerowania zrzutu.</span><span class="sxs-lookup"><span data-stu-id="cc9af-616">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="cc9af-617">Analizowanie zrzutu</span><span class="sxs-lookup"><span data-stu-id="cc9af-617">Analyze the dump</span></span>

<span data-ttu-id="cc9af-618">Zrzut można analizować przy użyciu kilku metod.</span><span class="sxs-lookup"><span data-stu-id="cc9af-618">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="cc9af-619">Aby uzyskać więcej informacji, zobacz [Analizowanie pliku zrzutu w trybie użytkownika](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="cc9af-619">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cc9af-620">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cc9af-620">Additional resources</span></span>

* <span data-ttu-id="cc9af-621">[Konfiguracja punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym Konfiguracja protokołu HTTPS i obsługa SNI)</span><span class="sxs-lookup"><span data-stu-id="cc9af-621">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
