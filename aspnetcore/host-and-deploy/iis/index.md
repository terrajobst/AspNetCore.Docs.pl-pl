---
title: Host platformy ASP.NET Core w systemie Windows z programem IIS
author: guardrex
description: "Dowiedz się, jak udostępniać aplikacje platformy ASP.NET Core na systemu Windows Server Internet informacji usług (IIS)."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/index
ms.openlocfilehash: 01cedb4e3abb35670d2908fe8cb4367c3fd58b33
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="834d6-103">Host platformy ASP.NET Core w systemie Windows z programem IIS</span><span class="sxs-lookup"><span data-stu-id="834d6-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="834d6-104">Przez [Luke Latham](https://github.com/guardrex) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="834d6-104">By [Luke Latham](https://github.com/guardrex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="834d6-105">Supported operating systems</span><span class="sxs-lookup"><span data-stu-id="834d6-105">Supported operating systems</span></span>

<span data-ttu-id="834d6-106">Obsługiwane są następujące systemy operacyjne:</span><span class="sxs-lookup"><span data-stu-id="834d6-106">The following operating systems are supported:</span></span>

* <span data-ttu-id="834d6-107">Windows 7 i nowsze</span><span class="sxs-lookup"><span data-stu-id="834d6-107">Windows 7 and newer</span></span>
* <span data-ttu-id="834d6-108">Windows Server 2008 R2 lub nowszym &#8224;</span><span class="sxs-lookup"><span data-stu-id="834d6-108">Windows Server 2008 R2 and newer&#8224;</span></span>

<span data-ttu-id="834d6-109">&#8224; Koncepcyjnie, konfiguracji usług IIS, w tym dokumencie opisano odnosi się także do obsługi aplikacji platformy ASP.NET Core na serwerze Nano Server IIS, ale dotyczą [platformy ASP.NET Core z usługami IIS na serwerze Nano](xref:tutorials/nano-server) Aby uzyskać szczegółowe instrukcje.</span><span class="sxs-lookup"><span data-stu-id="834d6-109">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core apps on Nano Server IIS, but refer to [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) for specific instructions.</span></span>

<span data-ttu-id="834d6-110">[Serwer HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej nazywanych [WebListener](xref:fundamentals/servers/weblistener)) nie będzie działać w konfiguracji zwrotny serwer proxy z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="834d6-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) won't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="834d6-111">Użyj [serwera Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="834d6-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="834d6-112">Konfiguracja usług IIS</span><span class="sxs-lookup"><span data-stu-id="834d6-112">IIS configuration</span></span>

<span data-ttu-id="834d6-113">Włącz **serwer sieci Web (IIS)** roli i ustanowienia usług ról.</span><span class="sxs-lookup"><span data-stu-id="834d6-113">Enable the **Web Server (IIS)** role and establish role services.</span></span>

### <a name="windows-desktop-operating-systems"></a><span data-ttu-id="834d6-114">Systemy operacyjne Windows</span><span class="sxs-lookup"><span data-stu-id="834d6-114">Windows desktop operating systems</span></span>

<span data-ttu-id="834d6-115">Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **wyłączyć funkcje systemu Windows na lub wyłącz** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="834d6-115">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="834d6-116">Otwórz grupę, do **Internetowe usługi informacyjne** i **narzędzia zarządzania siecią Web**.</span><span class="sxs-lookup"><span data-stu-id="834d6-116">Open the group for **Internet Information Services** and **Web Management Tools**.</span></span> <span data-ttu-id="834d6-117">Pole wyboru dla **Konsola zarządzania usługami IIS**.</span><span class="sxs-lookup"><span data-stu-id="834d6-117">Check the box for **IIS Management Console**.</span></span> <span data-ttu-id="834d6-118">Pole wyboru dla **usługi sieci World Wide Web**.</span><span class="sxs-lookup"><span data-stu-id="834d6-118">Check the box for **World Wide Web Services**.</span></span> <span data-ttu-id="834d6-119">Zaakceptuj domyślne funkcje dla **usługi sieci World Wide Web** lub dostosować funkcje usług IIS.</span><span class="sxs-lookup"><span data-stu-id="834d6-119">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

![Konsola zarządzania usługami IIS i usługi sieci World Wide Web są zaznaczone w funkcji systemu Windows.](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a><span data-ttu-id="834d6-121">Systemy operacyjne Windows Server</span><span class="sxs-lookup"><span data-stu-id="834d6-121">Windows Server operating systems</span></span>

<span data-ttu-id="834d6-122">Dla systemów operacyjnych serwera, użyj **Dodaj role i funkcje** za pomocą kreatora **Zarządzaj** menu lub link w **Menedżera serwera**.</span><span class="sxs-lookup"><span data-stu-id="834d6-122">For server operating systems, use the **Add Roles and Features** wizard via the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="834d6-123">Na **ról serwera** kroku, pole wyboru dla **serwer sieci Web (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="834d6-123">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

![W kroku role serwera wybierz wybrano roli Serwer sieci Web IIS.](index/_static/server-roles-ws2016.png)

<span data-ttu-id="834d6-125">Na **usług ról** krok, wybierz usługi ról usług IIS konieczne lub zaakceptuj domyślną rolę usług pod warunkiem.</span><span class="sxs-lookup"><span data-stu-id="834d6-125">On the **Role services** step, select the IIS role services desired or accept the default role services provided.</span></span>

![Domyślne usługi ról są zaznaczone w kroku wybierz rolę usług.](index/_static/role-services-ws2016.png)

<span data-ttu-id="834d6-127">Postępuj zgodnie z instrukcjami **potwierdzenie** krok w celu zainstalowania roli serwera sieci web i usług.</span><span class="sxs-lookup"><span data-stu-id="834d6-127">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="834d6-128">Ponowne uruchomienie serwera i IIS nie jest wymagane po zainstalowaniu roli serwera sieci Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="834d6-128">A server/IIS restart isn't required after installing the Web Server (IIS) role.</span></span>

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="834d6-129">Instalacja pakietu Hosting .NET Core systemu Windows Server</span><span class="sxs-lookup"><span data-stu-id="834d6-129">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="834d6-130">Zainstaluj [pakietu .NET Core systemu Windows serwer obsługujący](https://aka.ms/dotnetcore-2-windowshosting) przez system operacyjny.</span><span class="sxs-lookup"><span data-stu-id="834d6-130">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore-2-windowshosting) on the hosting system.</span></span> <span data-ttu-id="834d6-131">Pakiet instaluje wykonawczym .NET Core .NET Core biblioteki, a [platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="834d6-131">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="834d6-132">Moduł tworzy zwrotny serwer proxy między usługami IIS a Kestrel serwera.</span><span class="sxs-lookup"><span data-stu-id="834d6-132">The module creates the reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="834d6-133">Jeśli system nie ma połączenia internetowego, Uzyskaj i zainstaluj [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) przed zainstalowaniem pakietu Hosting .NET Core systemu Windows Server.</span><span class="sxs-lookup"><span data-stu-id="834d6-133">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Windows Server Hosting bundle.</span></span>

2. <span data-ttu-id="834d6-134">Ponowne uruchamianie systemu lub wykonać **net stop została /y** następuje **net start w3svc** z wiersza polecenia, aby pobrać zmiany systemowej PATH.</span><span class="sxs-lookup"><span data-stu-id="834d6-134">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt to pick up a change to the system PATH.</span></span>

> [!NOTE]
> <span data-ttu-id="834d6-135">Aby uzyskać informacje o konfiguracji udostępnionej usług IIS, zobacz [platformy ASP.NET Core modułu z konfiguracji udostępnionej usług IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="834d6-135">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="834d6-136">Zainstaluj pakiet Webdeploy podczas publikowania z programem Visual Studio</span><span class="sxs-lookup"><span data-stu-id="834d6-136">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="834d6-137">W przypadku wdrażania aplikacji na serwerach z [narzędzia Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), zainstaluj najnowszą wersję narzędzia Web Deploy na serwerze.</span><span class="sxs-lookup"><span data-stu-id="834d6-137">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="834d6-138">Aby zainstalować narzędzie Web Deploy, użyj [Instalatora platformy sieci Web (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) lub uzyskać Instalatora bezpośrednio z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="834d6-138">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="834d6-139">Preferowaną metodą jest używać WebPI.</span><span class="sxs-lookup"><span data-stu-id="834d6-139">The preferred method is to use WebPI.</span></span> <span data-ttu-id="834d6-140">WebPI oferuje autonomicznego Instalatora i konfiguracja dla dostawców hostingu.</span><span class="sxs-lookup"><span data-stu-id="834d6-140">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="834d6-141">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="834d6-141">Application configuration</span></span>

### <a name="enabling-the-iisintegration-components"></a><span data-ttu-id="834d6-142">Włączanie składników IISIntegration</span><span class="sxs-lookup"><span data-stu-id="834d6-142">Enabling the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="834d6-143">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="834d6-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="834d6-144">Typowe *Program.cs* wywołania [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) aby rozpocząć konfigurowanie hosta.</span><span class="sxs-lookup"><span data-stu-id="834d6-144">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="834d6-145">`CreateDefaultBuilder`Konfiguruje [Kestrel](xref:fundamentals/servers/kestrel) jako sieci web Integracja serwera i umożliwia usług IIS przez skonfigurowanie ścieżki podstawowej i port [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module):</span><span class="sxs-lookup"><span data-stu-id="834d6-145">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="834d6-146">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="834d6-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="834d6-147">Obejmują zależności na [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) pakietu w zależnościach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="834d6-147">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="834d6-148">Włączenie oprogramowanie pośredniczące integracji usług IIS do aplikacji przez dodanie *UseIISIntegration* metodę rozszerzenie *WebHostBuilder*:</span><span class="sxs-lookup"><span data-stu-id="834d6-148">Incorporate IIS Integration middleware into the app by adding the *UseIISIntegration* extension method to *WebHostBuilder*:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="834d6-149">Zarówno `UseKestrel` i `UseIISIntegration` są wymagane.</span><span class="sxs-lookup"><span data-stu-id="834d6-149">Both `UseKestrel` and `UseIISIntegration` are required.</span></span> <span data-ttu-id="834d6-150">Wywoływanie kodu `UseIISIntegration` nie ma wpływu na przenośność kodu.</span><span class="sxs-lookup"><span data-stu-id="834d6-150">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="834d6-151">Jeśli aplikacja nie jest uruchamiana za usług IIS (na przykład aplikacja jest uruchamiana bezpośrednio na Kestrel), `UseIISIntegration` ops nie.</span><span class="sxs-lookup"><span data-stu-id="834d6-151">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` no-ops.</span></span>

---

<span data-ttu-id="834d6-152">Aby uzyskać więcej informacji dotyczących obsługi, zobacz [Hosting w ASP.NET Core](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="834d6-152">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="834d6-153">Opcje usług IIS</span><span class="sxs-lookup"><span data-stu-id="834d6-153">IIS options</span></span>

<span data-ttu-id="834d6-154">Aby skonfigurować opcje usług IIS, obejmują konfigurację usługi `IISOptions` w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="834d6-154">To configure IIS options, include a service configuration for `IISOptions` in `ConfigureServices`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| <span data-ttu-id="834d6-155">Opcja</span><span class="sxs-lookup"><span data-stu-id="834d6-155">Option</span></span>                         | <span data-ttu-id="834d6-156">Domyślny</span><span class="sxs-lookup"><span data-stu-id="834d6-156">Default</span></span> | <span data-ttu-id="834d6-157">Ustawienie</span><span class="sxs-lookup"><span data-stu-id="834d6-157">Setting</span></span> |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="834d6-158">Jeśli `true`, ustawia oprogramowanie pośredniczące uwierzytelniania `HttpContext.User` i sprostać wymaganiom ogólnego.</span><span class="sxs-lookup"><span data-stu-id="834d6-158">If `true`, the authentication middleware sets the `HttpContext.User` and responds to generic challenges.</span></span> <span data-ttu-id="834d6-159">Jeśli `false`, oprogramowanie pośredniczące uwierzytelniania zapewnia tylko tożsamości (`HttpContext.User`) i sprostać wymaganiom jawnie żądanie `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="834d6-159">If `false`, the authentication middleware only provides an identity (`HttpContext.User`) and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="834d6-160">Należy włączyć uwierzytelnianie systemu Windows w usługach IIS dla `AutomaticAuthentication` funkcji.</span><span class="sxs-lookup"><span data-stu-id="834d6-160">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="834d6-161">Ustawia nazwę wyświetlaną pokazywana użytkownikom na stronach logowania.</span><span class="sxs-lookup"><span data-stu-id="834d6-161">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="834d6-162">Jeśli `true` i `MS-ASPNETCORE-CLIENTCERT` nagłówek żądania jest obecny, `HttpContext.Connection.ClientCertificate` jest wypełnione.</span><span class="sxs-lookup"><span data-stu-id="834d6-162">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig"></a><span data-ttu-id="834d6-163">plik Web.config</span><span class="sxs-lookup"><span data-stu-id="834d6-163">web.config</span></span>

<span data-ttu-id="834d6-164">*Web.config* tego pliku podstawowego służy do konfigurowania [platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="834d6-164">The *web.config* file's primary purpose is to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="834d6-165">Opcjonalnie ona dodatkowych ustawień konfiguracji usług IIS.</span><span class="sxs-lookup"><span data-stu-id="834d6-165">It may optionally provide additional IIS configuration settings.</span></span> <span data-ttu-id="834d6-166">Tworzenie, przekształcanie i publikowanie *web.config* jest obsługiwany przez zestaw SDK programu .NET Core sieci Web (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="834d6-166">Creating, transforming, and publishing *web.config* is handled by the .NET Core Web SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="834d6-167">Zestaw SDK jest ustawiony na początku pliku projektu `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span><span class="sxs-lookup"><span data-stu-id="834d6-167">The SDK is set  at the top of the project file, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span></span> <span data-ttu-id="834d6-168">Aby zapobiec przekształcania zestawu SDK *web.config* plików, dodawanie  **\<IsTransformWebConfigDisabled >** właściwości do pliku projektu z ustawieniem `true`:</span><span class="sxs-lookup"><span data-stu-id="834d6-168">To prevent the SDK from transforming the *web.config* file, add the **\<IsTransformWebConfigDisabled>** property to the project file with a setting of `true`:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="834d6-169">Jeśli *web.config* plik znajduje się w projekcie, jest przekształcana z prawidłowym *processPath* i *argumenty* skonfigurować [platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module) i przenieść do [opublikowane dane wyjściowe](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="834d6-169">If a *web.config* file is in the project, it's transformed with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span> <span data-ttu-id="834d6-170">Transformacja nie zmodyfikować ustawień konfiguracji usług IIS w pliku.</span><span class="sxs-lookup"><span data-stu-id="834d6-170">The transformation doesn't modify IIS configuration settings in the file.</span></span>

### <a name="webconfig-location"></a><span data-ttu-id="834d6-171">Lokalizacja pliku Web.config</span><span class="sxs-lookup"><span data-stu-id="834d6-171">web.config location</span></span>

<span data-ttu-id="834d6-172">Aplikacje .NET core są obsługiwane przez zwrotny serwer proxy między usługami IIS a Kestrel serwera.</span><span class="sxs-lookup"><span data-stu-id="834d6-172">.NET Core apps are hosted via a reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="834d6-173">Aby można było utworzyć zwrotny serwer proxy, *web.config* plik musi znajdować się w ścieżce zawartości katalogu głównego (zazwyczaj ścieżki podstawowej aplikacji) wdrożonej aplikacji, to ścieżka fizyczna witryny sieci Web do usług IIS.</span><span class="sxs-lookup"><span data-stu-id="834d6-173">In order to create the reverse proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app, which is the website physical path provided to IIS.</span></span> <span data-ttu-id="834d6-174">*Web.config* plik jest wymagany w katalogu głównym aplikacji, aby umożliwić publikowanie wielu aplikacji za pomocą narzędzia Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="834d6-174">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="834d6-175">Poufne pliki istnieją na ścieżkę fizyczną aplikacji, w tym podfolderów, takich jak  *\<nazwa_zestawu >. runtimeconfig.json*,  *\<nazwa_zestawu > .xml* (XML Komentarze dokumentacji) i  *\<nazwa_zestawu >. deps.json*.</span><span class="sxs-lookup"><span data-stu-id="834d6-175">Sensitive files exist on the app's physical path, including subfolders, such as *\<assembly_name>.runtimeconfig.json*, *\<assembly_name>.xml* (XML Documentation comments), and *\<assembly_name>.deps.json*.</span></span> <span data-ttu-id="834d6-176">Gdy *web.config* plik istnieje i skonfiguruje lokację, usług IIS zapobiega obsługiwanej przez te poufnych plików.</span><span class="sxs-lookup"><span data-stu-id="834d6-176">When the *web.config* file is present and configures the site, IIS prevents these sensitive files from being served.</span></span> <span data-ttu-id="834d6-177">**W związku z tym ważne jest który *web.config* plik nie jest ani przypadkowo, zmieniono jego nazwę lub usunięte z wdrożenia.**</span><span class="sxs-lookup"><span data-stu-id="834d6-177">**Therefore, it's important that the *web.config* file isn't accidently renamed or removed from the deployment.**</span></span>

## <a name="create-the-iis-website"></a><span data-ttu-id="834d6-178">Utwórz witrynę sieci Web usług IIS</span><span class="sxs-lookup"><span data-stu-id="834d6-178">Create the IIS Website</span></span>

1. <span data-ttu-id="834d6-179">W celu systemu usług IIS, należy utworzyć folder do zawierają aplikacji opublikowanych pliki i foldery, które są opisane w [struktury katalogów](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="834d6-179">On the target IIS system, create a folder to contain the app's published folders and files, which are described in [Directory Structure](xref:host-and-deploy/directory-structure).</span></span>

2. <span data-ttu-id="834d6-180">W folderze, Utwórz *dzienniki* folder do przechowywania dzienników stdout, gdy jest włączone rejestrowanie stdout.</span><span class="sxs-lookup"><span data-stu-id="834d6-180">Within the folder, create a *logs* folder to hold stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="834d6-181">Jeśli aplikacja jest wdrażana z *dzienniki* folderu w ładunku, Pomiń ten krok.</span><span class="sxs-lookup"><span data-stu-id="834d6-181">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="834d6-182">Aby uzyskać instrukcje dotyczące sposobu wprowadzania MSBuild utworzyć *dzienniki* folderu, zobacz [struktury katalogów](xref:host-and-deploy/directory-structure) tematu.</span><span class="sxs-lookup"><span data-stu-id="834d6-182">For instructions on how to make MSBuild create the *logs* folder, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

3. <span data-ttu-id="834d6-183">W **Menedżera usług IIS**, Utwórz nową witrynę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="834d6-183">In **IIS Manager**, create a new website.</span></span> <span data-ttu-id="834d6-184">Podaj **nazwa witryny** i ustaw **ścieżka fizyczna** do folderu wdrożenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="834d6-184">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="834d6-185">Podaj **powiązanie** konfigurację i tworzenie witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="834d6-185">Provide the **Binding** configuration and create the website.</span></span>

4. <span data-ttu-id="834d6-186">Skonfiguruj pulę aplikacji **bez kodu zarządzanego**.</span><span class="sxs-lookup"><span data-stu-id="834d6-186">Set the application pool to **No Managed Code**.</span></span> <span data-ttu-id="834d6-187">Platformy ASP.NET Core działa w oddzielnych procesach i zarządza środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="834d6-187">ASP.NET Core runs in a separate process and manages the runtime.</span></span>

5. <span data-ttu-id="834d6-188">Otwórz **Dodawanie witryny sieci Web** okna.</span><span class="sxs-lookup"><span data-stu-id="834d6-188">Open the **Add Website** window.</span></span>

   ![Wybierz "Dodawanie witryny sieci Web" z menu kontekstowe witryny.](index/_static/add-website-context-menu-ws2016.png)

6. <span data-ttu-id="834d6-190">Konfigurowanie witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="834d6-190">Configure the website.</span></span>

   ![Podaj nazwę lokacji, ścieżka fizyczna i nazwy hosta w kroku Dodawanie witryny sieci Web.](index/_static/add-website-ws2016.png)

7. <span data-ttu-id="834d6-192">W **pul aplikacji** panelu, otwórz **edytowanie puli aplikacji** okno prawym przyciskiem myszy w puli aplikacji witryny sieci Web i wybierając **podstawowych ustawień...**  w menu podręcznym.</span><span class="sxs-lookup"><span data-stu-id="834d6-192">In the **Application Pools** panel, open the **Edit Application Pool** window by right-clicking on the website's app pool and selecting **Basic Settings...** from the popup menu.</span></span>

   ![Wybierz ustawienia podstawowe z menu kontekstowego puli aplikacji.](index/_static/apppools-basic-settings-ws2016.png)

8. <span data-ttu-id="834d6-194">Ustaw **wersja środowiska .NET CLR** do **bez kodu zarządzanego**.</span><span class="sxs-lookup"><span data-stu-id="834d6-194">Set the **.NET CLR version** to **No Managed Code**.</span></span>

   ![Ustaw żadnego kodu zarządzanego dla wersji środowiska CLR programu .NET.](index/_static/edit-apppool-ws2016.png)
     
    <span data-ttu-id="834d6-196">Uwaga: Ustawienie **wersja środowiska .NET CLR** do **bez kodu zarządzanego** jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="834d6-196">Note: Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span> <span data-ttu-id="834d6-197">Platformy ASP.NET Core nie zależą od ładowania CLR pulpitu.</span><span class="sxs-lookup"><span data-stu-id="834d6-197">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span>

9. <span data-ttu-id="834d6-198">Upewnij się, że tożsamość procesu modelu ma odpowiednie uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="834d6-198">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="834d6-199">Jeśli domyślna tożsamość puli aplikacji (**Model procesu** > **tożsamości**) została zmieniona z **puli** tożsamość, sprawdź, czy nową tożsamość ma wymagane uprawnienia dostępu do folderu instalacji aplikacji, bazy danych i innych wymaganych zasobów.</span><span class="sxs-lookup"><span data-stu-id="834d6-199">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span>
   
## <a name="deploy-the-app"></a><span data-ttu-id="834d6-200">Wdrażanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="834d6-200">Deploy the app</span></span>

<span data-ttu-id="834d6-201">Wdróż aplikację na folder utworzony w celu systemu usług IIS.</span><span class="sxs-lookup"><span data-stu-id="834d6-201">Deploy the app to the folder created on the target IIS system.</span></span> <span data-ttu-id="834d6-202">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) to mechanizm zalecane dla wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="834d6-202">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

<span data-ttu-id="834d6-203">Upewnij się, że opublikowana aplikacja dla wdrożenia nie jest uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="834d6-203">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="834d6-204">Pliki w *publikowania* folderu są zablokowane, gdy aplikacja jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="834d6-204">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="834d6-205">Nie można zastąpić zablokowane pliki.</span><span class="sxs-lookup"><span data-stu-id="834d6-205">Locked files can't be overwritten.</span></span> <span data-ttu-id="834d6-206">Aby zwolnić zablokowane pliki we wdrożeniu, zatrzymać puli aplikacji:</span><span class="sxs-lookup"><span data-stu-id="834d6-206">To release locked files in a deployment, stop the app pool:</span></span>

* <span data-ttu-id="834d6-207">Ręcznie w Menedżerze usług IIS na serwerze.</span><span class="sxs-lookup"><span data-stu-id="834d6-207">Manually in the IIS Manager on the server.</span></span>
* <span data-ttu-id="834d6-208">Za pomocą narzędzia Web Deploy i odwołuje się do `Microsoft.NET.Sdk.Web` w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="834d6-208">Using Web Deploy and referencing `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="834d6-209">*App_offline.htm* plik znajduje się w głównym katalogu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="834d6-209">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="834d6-210">Gdy plik jest obecny, moduł platformy ASP.NET Core bezpiecznie zamyka aplikację i służy *app_offline.htm* plików podczas wdrażania.</span><span class="sxs-lookup"><span data-stu-id="834d6-210">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="834d6-211">Aby uzyskać więcej informacji, zobacz [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="834d6-211">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>
* <span data-ttu-id="834d6-212">Zatrzymanie i ponowne uruchomienie puli aplikacji (wymaga programu PowerShell, 5 lub nowszy) przy użyciu programu PowerShell:</span><span class="sxs-lookup"><span data-stu-id="834d6-212">Use PowerShell to stop and restart the app pool (requires PowerShell 5 or later):</span></span>
  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="834d6-213">Narzędzie Web Deploy w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="834d6-213">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="834d6-214">Zobacz [profilów publikowania programu Visual Studio dla wdrożenia aplikacji platformy ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) tematu, aby dowiedzieć się, jak utworzyć profil publikowania do użytku z narzędzia Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="834d6-214">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="834d6-215">Jeśli dostawca hostingu dostarcza profil publikowania lub obsługę utworzenie, Pobierz swój profil i zaimportuj go za pomocą programu Visual Studio **publikowania** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="834d6-215">If the hosting provider supplies a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Okno dialogowe strony publikacji](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="834d6-217">Narzędzie Web Deploy poza programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="834d6-217">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="834d6-218">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) można również poza Visual Studio z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="834d6-218">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="834d6-219">Aby uzyskać więcej informacji, zobacz [narzędzie Web Deployment](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="834d6-219">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="834d6-220">Wdrażanie rozwiązań alternatywnych w sieci Web</span><span class="sxs-lookup"><span data-stu-id="834d6-220">Alternatives to Web Deploy</span></span>

<span data-ttu-id="834d6-221">Przenoszenie aplikacji w systemie hostingu, na przykład Xcopy, Robocopy lub programu PowerShell przy użyciu jednej z kilku metod.</span><span class="sxs-lookup"><span data-stu-id="834d6-221">Use any of several methods to move the app to the hosting system, such as Xcopy, Robocopy, or PowerShell.</span></span> <span data-ttu-id="834d6-222">Visual Studio użytkownicy mogą skorzystać z [przykłady publikowania](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span><span class="sxs-lookup"><span data-stu-id="834d6-222">Visual Studio users may use the [Publish Samples](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="834d6-223">Przeglądaj witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="834d6-223">Browse the website</span></span>

![Przeglądarka Microsoft Edge załadował strony początkowej usług IIS.](index/_static/browsewebsite.png)

## <a name="data-protection"></a><span data-ttu-id="834d6-225">Ochrona danych</span><span class="sxs-lookup"><span data-stu-id="834d6-225">Data protection</span></span>

<span data-ttu-id="834d6-226">Ochrona danych jest używana przez kilka middlewares ASP.NET, w tym używane podczas uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="834d6-226">Data Protection is used by several ASP.NET middlewares, including those used in authentication.</span></span> <span data-ttu-id="834d6-227">Nawet jeśli interfejsy API ochrony danych nie jest wywoływany z kodu użytkownika, ochrony danych należy skonfigurować przy użyciu skryptu wdrożenia lub w kodzie użytkownika, aby utworzyć trwały magazynu kluczy.</span><span class="sxs-lookup"><span data-stu-id="834d6-227">Even if Data Protection APIs aren't called from user's code, Data Protection should be configured with a deployment script or in user code to create a persistent key store.</span></span> <span data-ttu-id="834d6-228">Jeśli nie jest skonfigurowany do ochrony danych, klucze są przechowywane w pamięci i odrzucone po ponownym uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="834d6-228">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="834d6-229">Jeśli zestaw kluczy są przechowywane w pamięci, po ponownym uruchomieniu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="834d6-229">If the keyring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="834d6-230">Wszystkie tokeny uwierzytelniania formularzy zostały unieważnione.</span><span class="sxs-lookup"><span data-stu-id="834d6-230">All forms authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="834d6-231">Użytkownicy muszą ponownie zaloguj się na ich następnego żądania.</span><span class="sxs-lookup"><span data-stu-id="834d6-231">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="834d6-232">Wszystkie dane chronione za pomocą zestaw kluczy nie mogły być odszyfrowane.</span><span class="sxs-lookup"><span data-stu-id="834d6-232">Any data protected with the keyring can no longer be decrypted.</span></span>

<span data-ttu-id="834d6-233">Aby skonfigurować ochronę danych w środowisku usług IIS, należy użyć **jeden** z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="834d6-233">To configure Data Protection under IIS, use **one** of the following approaches:</span></span>

### <a name="create-a-data-protection-registry-hive"></a><span data-ttu-id="834d6-234">Utwórz gałąź rejestru ochrony danych</span><span class="sxs-lookup"><span data-stu-id="834d6-234">Create a Data Protection Registry Hive</span></span>

<span data-ttu-id="834d6-235">Dane ochrony kluczy używanych przez aplikacje ASP.NET są przechowywane w gałęzi rejestru zewnętrzne do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="834d6-235">Data Protection keys used by ASP.NET apps are stored in registry hives external to the apps.</span></span> <span data-ttu-id="834d6-236">Aby zachować kluczy dla danej aplikacji, utwórz gałąź rejestru dla puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="834d6-236">To persist the keys for a given app, create a registry hive for the app pool.</span></span>

<span data-ttu-id="834d6-237">Dla autonomicznej, w przypadku instalacji usług IIS z systemem innym niż farma sieci Web [skrypt programu PowerShell należy AutoGenKeys.ps1 ochrony danych](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) można używać dla każdej puli aplikacji używane w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="834d6-237">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="834d6-238">Ten skrypt tworzy specjalnego klucza rejestru HKLM, które ma ACLed tylko konta procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="834d6-238">This script creates a special registry key in the HKLM registry that is ACLed only to the worker process account.</span></span> <span data-ttu-id="834d6-239">Klucze są szyfrowane, gdy przy użyciu DPAPI za pomocą klucza komputera.</span><span class="sxs-lookup"><span data-stu-id="834d6-239">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

<span data-ttu-id="834d6-240">W scenariuszach kolektywu serwerów sieci web aplikację można skonfigurować do użyć ścieżki UNC do przechowywania jego zestaw kluczy ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="834d6-240">In web farm scenarios, an app can be configured to use a UNC path to store its data protection keyring.</span></span> <span data-ttu-id="834d6-241">Domyślnie klucze ochrony danych nie są szyfrowane.</span><span class="sxs-lookup"><span data-stu-id="834d6-241">By default, the data protection keys are not encrypted.</span></span> <span data-ttu-id="834d6-242">Upewnij się, że uprawnienia udziału takie jest ograniczone do uruchomieniu aplikacji jako konto systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="834d6-242">Ensure that the file permissions for such a share are limited to the Windows account the app runs as.</span></span> <span data-ttu-id="834d6-243">Ponadto X509 certyfikatu może służyć do ochrony kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="834d6-243">In addition, an X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="834d6-244">Należy wziąć pod uwagę mechanizm Zezwalaj użytkownikom na przekazywanie certyfikaty: miejsce certyfikatów do zaufanego certyfikatu użytkownika przechowywania i upewnij się, są one dostępne na wszystkich komputerach, którym jest uruchamiany aplikacji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="834d6-244">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="834d6-245">Zobacz [konfigurowania ochrony danych](xref:security/data-protection/configuration/overview) szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="834d6-245">See [Configuring Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a><span data-ttu-id="834d6-246">Konfigurowanie puli aplikacji usług IIS do załadowania profilu użytkownika</span><span class="sxs-lookup"><span data-stu-id="834d6-246">Configure the IIS Application Pool to load the user profile</span></span>

<span data-ttu-id="834d6-247">To ustawienie jest **Model procesu** w obszarze **Zaawansowane ustawienia** dla puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="834d6-247">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="834d6-248">Ustaw Załaduj profil użytkownika `True`.</span><span class="sxs-lookup"><span data-stu-id="834d6-248">Set Load User Profile to `True`.</span></span> <span data-ttu-id="834d6-249">To są przechowywane klucze w katalogu profilu użytkownika i ich ochrony za pomocą DPAPI kluczem określone konto użytkownika używane w puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="834d6-249">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used for the app pool.</span></span>

### <a name="use-the-file-system-as-a-key-ring-store"></a><span data-ttu-id="834d6-250">System plików jako pierścień klucza magazynu</span><span class="sxs-lookup"><span data-stu-id="834d6-250">Use the file system as a key ring store</span></span>

<span data-ttu-id="834d6-251">Kod aplikacji, aby dopasować [system plików jako magazyn pierścień klucza](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="834d6-251">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="834d6-252">Użyj X509 certyfikatu, aby chronić zestaw kluczy i certyfikat zaufanego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="834d6-252">Use an X509 certificate to protect the keyring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="834d6-253">Jeśli certyfikat z podpisem własnym, należy umieścić certyfikatu w magazynie zaufanych głównych.</span><span class="sxs-lookup"><span data-stu-id="834d6-253">If it's a self signed certificate, place the certificate in the Trusted Root store.</span></span>

<span data-ttu-id="834d6-254">Korzystając z usług IIS w farmie sieci web:</span><span class="sxs-lookup"><span data-stu-id="834d6-254">When using IIS in a web farm:</span></span>

* <span data-ttu-id="834d6-255">Użyj udziału plików, które mogą uzyskiwać dostęp do wszystkich maszyn.</span><span class="sxs-lookup"><span data-stu-id="834d6-255">Use a file share that all machines can access.</span></span>
* <span data-ttu-id="834d6-256">Wdrażanie X509 certyfikatów na każdym komputerze.</span><span class="sxs-lookup"><span data-stu-id="834d6-256">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="834d6-257">Skonfiguruj [ochrony danych w kodzie](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="834d6-257">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

### <a name="set-a-machine-wide-policy-for-data-protection"></a><span data-ttu-id="834d6-258">Ustawienie zasad komputera w przypadku ochrony danych</span><span class="sxs-lookup"><span data-stu-id="834d6-258">Set a machine-wide policy for data protection</span></span>

<span data-ttu-id="834d6-259">System ochrony danych ma ograniczoną obsługę ustawienie domyślne [komputera zasad](xref:security/data-protection/configuration/machine-wide-policy) dla wszystkich aplikacji, które korzystanie z interfejsów API ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="834d6-259">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="834d6-260">Zobacz [ochrony danych](xref:security/data-protection/index) dokumentację, aby uzyskać więcej szczegółowych informacji.</span><span class="sxs-lookup"><span data-stu-id="834d6-260">See the [data protection](xref:security/data-protection/index) documentation for more details.</span></span>

## <a name="configuration-of-sub-applications"></a><span data-ttu-id="834d6-261">Konfiguracja aplikacji podrzędne</span><span class="sxs-lookup"><span data-stu-id="834d6-261">Configuration of sub-applications</span></span>

<span data-ttu-id="834d6-262">Aplikacje podrzędne dodany w obszarze katalogu głównego aplikacji nie powinna zawierać moduł platformy ASP.NET Core jako program obsługi.</span><span class="sxs-lookup"><span data-stu-id="834d6-262">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="834d6-263">Jeśli moduł jest dodawany jako program obsługi w sub-app *web.config* pliku 500.19 (wewnętrzny błąd serwera) odwołuje się do pliku konfiguracji błędny odebraniu podczas próby przeglądania aplikacji sub.</span><span class="sxs-lookup"><span data-stu-id="834d6-263">If the module is added as a handler in a sub-app's *web.config* file, a 500.19 (Internal Server Error) referencing the faulty config file is received when attempting to browse the sub-app.</span></span> <span data-ttu-id="834d6-264">W poniższym przykładzie przedstawiono zawartości opublikowanej *web.config* pliku dla platformy ASP.NET Core sub aplikacji:</span><span class="sxs-lookup"><span data-stu-id="834d6-264">The following example shows the contents of a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="834d6-265">Odnośnie do hostowania - platformy ASP.NET Core sub aplikacji w kontenerze aplikacji platformy ASP.NET Core, jawnie usunąć dziedziczonej obsługi w aplikacji sub *web.config* pliku:</span><span class="sxs-lookup"><span data-stu-id="834d6-265">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="834d6-266">Aby uzyskać więcej informacji na temat konfigurowania moduł platformy ASP.NET Core, zobacz [wprowadzenie do platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module) tematu i [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="834d6-266">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="834d6-267">Konfiguracja usług IIS z pliku web.config</span><span class="sxs-lookup"><span data-stu-id="834d6-267">Configuration of IIS with web.config</span></span>

<span data-ttu-id="834d6-268">Konfiguracja usług IIS ma wpływ  **\<system.webServer >** sekcji *web.config* dla tych funkcji usług IIS, które mają zastosowanie do konfiguracji zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="834d6-268">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="834d6-269">Skonfigurowanie usług IIS na poziomie systemu, aby użyć kompresji dynamicznej to ustawienie zostanie wyłączone dla aplikacji za pomocą  **\<urlCompression >** element w aplikacji *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="834d6-269">If IIS is configured at the system level to use dynamic compression, that setting can be disabled for an app with the **\<urlCompression>** element in the app's *web.config* file.</span></span> <span data-ttu-id="834d6-270">Aby uzyskać więcej informacji, zobacz [konfiguracji odwołania dla \<system.webServer >](https://docs.microsoft.com/iis/configuration/system.webServer/), [odwołania do konfiguracji modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module) i [przy użyciu modułów usług IIS w aplikacji ASP. Podstawowe NET](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="834d6-270">For more information, see the [configuration reference for \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module) and [Using IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="834d6-271">Jeśli trzeba ustawić zmienne środowiskowe dla poszczególnych aplikacji uruchomionych w pulach aplikacji izolowanych (obsługiwane w usługach IIS 10.0 +), zobacz *polecenia AppCmd.exe* sekcji [zmiennych środowiskowych \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) dokumentacji odwołania tematu w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="834d6-271">If there's a need to set environment variables for individual apps running in isolated app pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="834d6-272">Sekcji konfiguracyjnych w pliku Web.config</span><span class="sxs-lookup"><span data-stu-id="834d6-272">Configuration sections of web.config</span></span>

<span data-ttu-id="834d6-273">Sekcje Configruation aplikacji struktury ASP.NET w *web.config* nie są używane przez aplikacje platformy ASP.NET Core dla konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="834d6-273">Configruation sections of ASP.NET Framework apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="834d6-274">**\<System.Web >**</span><span class="sxs-lookup"><span data-stu-id="834d6-274">**\<system.web>**</span></span>
* <span data-ttu-id="834d6-275">**\<appSettings >**</span><span class="sxs-lookup"><span data-stu-id="834d6-275">**\<appSettings>**</span></span>
* <span data-ttu-id="834d6-276">**\<connectionStrings >**</span><span class="sxs-lookup"><span data-stu-id="834d6-276">**\<connectionStrings>**</span></span>
* <span data-ttu-id="834d6-277">**\<Lokalizacja >**</span><span class="sxs-lookup"><span data-stu-id="834d6-277">**\<location>**</span></span>

<span data-ttu-id="834d6-278">Aplikacje platformy ASP.NET Core są skonfigurowane przy użyciu innych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="834d6-278">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="834d6-279">Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="834d6-279">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="834d6-280">Pule aplikacji</span><span class="sxs-lookup"><span data-stu-id="834d6-280">Application Pools</span></span>

<span data-ttu-id="834d6-281">Odnośnie do hostowania wielu witryn sieci Web na jednym komputerze, izolowania aplikacji od siebie, uruchamiając każdej aplikacji w odrębnej puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="834d6-281">When hosting multiple websites on a single system, isolate the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="834d6-282">Usługi IIS **Dodawanie witryny sieci Web** domyślnie okno dialogowe tej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="834d6-282">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="834d6-283">Gdy **nazwa witryny** została podana, tekst jest automatycznie przenoszona do **puli aplikacji** pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="834d6-283">When **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="834d6-284">Jest tworzona nowa pula aplikacji przy użyciu nazwy lokacji po dodaniu witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="834d6-284">A new app pool is created using the site name when the website is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="834d6-285">Tożsamość puli aplikacji</span><span class="sxs-lookup"><span data-stu-id="834d6-285">Application Pool Identity</span></span>

<span data-ttu-id="834d6-286">Konto tożsamości puli aplikacji dzięki czemu aplikacja może działać w ramach unikatowe konto bez konieczności tworzenia i zarządzania nimi domeny lub lokalnego konta.</span><span class="sxs-lookup"><span data-stu-id="834d6-286">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="834d6-287">W programie IIS 8.0 i nowsze proces roboczy administratora usług IIS (WAS) tworzy konto wirtualny o nazwie nowej puli aplikacji i aplikacji w puli procesów roboczych na tym koncie są domyślnie uruchamiane.</span><span class="sxs-lookup"><span data-stu-id="834d6-287">On IIS 8.0+, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="834d6-288">W konsoli zarządzania usług IIS w obszarze **Zaawansowane ustawienia** dla puli aplikacji, upewnij się, że **tożsamości** jest skonfigurowany do używania **puli**:</span><span class="sxs-lookup"><span data-stu-id="834d6-288">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Okno dialogowe Zaawansowane ustawienia puli aplikacji](index/_static/apppool-identity.png)

<span data-ttu-id="834d6-290">Proces zarządzania usług IIS tworzy bezpiecznego identyfikatora o nazwie puli aplikacji w systemie zabezpieczeń systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="834d6-290">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="834d6-291">Zasoby mogą być chronione przy użyciu tej tożsamości; Jednak ta tożsamość nie jest rzeczywistego konta użytkownika i nie będą widoczne w konsoli zarządzania użytkownika systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="834d6-291">Resources can be secured by using this identity; however, this identity isn't a real user account and won't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="834d6-292">Jeśli proces roboczy usług IIS wymaga podwyższonego poziomu dostępu do aplikacji, należy zmodyfikować listy kontroli dostępu (ACL) dla katalogu zawierającej aplikację:</span><span class="sxs-lookup"><span data-stu-id="834d6-292">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="834d6-293">Otwórz Eksploratora Windows i przejdź do katalogu.</span><span class="sxs-lookup"><span data-stu-id="834d6-293">Open Windows Explorer and navigate to the directory.</span></span>

2. <span data-ttu-id="834d6-294">Kliknij prawym przyciskiem myszy na katalog i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="834d6-294">Right-click on the directory and select **Properties**.</span></span>

3. <span data-ttu-id="834d6-295">W obszarze **zabezpieczeń** wybierz opcję **Edytuj** przycisk, a następnie **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="834d6-295">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

4. <span data-ttu-id="834d6-296">Wybierz **lokalizacje** przycisk i upewnij się, że system jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="834d6-296">Select the **Locations** button and make sure the system is selected.</span></span>

5. <span data-ttu-id="834d6-297">Wprowadź **IIS AppPool\DefaultAppPool** w **wprowadź nazwy obiektów do wybrania** pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="834d6-297">Enter **IIS AppPool\DefaultAppPool** in **Enter the object names to select** textbox.</span></span>

   ![Wybierz użytkowników lub grup okno folderu aplikacji](index/_static/select-users-or-groups-1.png)

6. <span data-ttu-id="834d6-299">Wybierz **Sprawdź nazwy** przycisku.</span><span class="sxs-lookup"><span data-stu-id="834d6-299">Select the **Check Names** button.</span></span> <span data-ttu-id="834d6-300">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="834d6-300">Select **OK**.</span></span>

   ![Wybierz użytkowników lub grup okno folderu aplikacji](index/_static/select-users-or-groups-2.png)

<span data-ttu-id="834d6-302">Ponadto można to zrobić za pomocą wiersza polecenia przy użyciu **ICACLS** narzędzie:</span><span class="sxs-lookup"><span data-stu-id="834d6-302">This can also be accomplished via a command prompt using the **ICACLS** tool:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a><span data-ttu-id="834d6-303">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="834d6-303">Additional resources</span></span>

* [<span data-ttu-id="834d6-304">Rozwiązywanie problemów z platformy ASP.NET Core w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="834d6-304">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="834d6-305">Typowe błędy odwołania dla usługi Azure App Service i IIS z platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="834d6-305">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="834d6-306">Wprowadzenie do platformy ASP.NET Core modułu</span><span class="sxs-lookup"><span data-stu-id="834d6-306">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="834d6-307">Konfiguracja modułu Core programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="834d6-307">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="834d6-308">Używanie modułów usług IIS z platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="834d6-308">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="834d6-309">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="834d6-309">Introduction to ASP.NET Core</span></span>](../index.md)
* [<span data-ttu-id="834d6-310">Witryna oficjalnego Microsoft IIS</span><span class="sxs-lookup"><span data-stu-id="834d6-310">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="834d6-311">Bibliotece Microsoft TechNet: Serwer systemu Windows</span><span class="sxs-lookup"><span data-stu-id="834d6-311">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)
