---
title: Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core
author: scottaddie
description: Dowiedz się, jak skonfigurować uwierzytelnianie Windows w programie ASP.NET Core dla usług IIS i sterownik HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/12/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 93f833adff95f25d570947cd1a9035d652f522c2
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034955"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="2f0da-103">Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2f0da-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="2f0da-104">Przez [Scott Addie](https://twitter.com/Scott_Addie) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2f0da-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2f0da-105">Windows Authentication (nazywanego też uwierzytelnianiem Negotiate, Kerberos lub NTLM) można skonfigurować dla aplikacji platformy ASP.NET Core z [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), lub [HTTP.sys](xref:fundamentals/servers/httpsys) .</span><span class="sxs-lookup"><span data-stu-id="2f0da-105">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2f0da-106">Windows Authentication (nazywanego też uwierzytelnianiem Negotiate, Kerberos lub NTLM) można skonfigurować dla aplikacji platformy ASP.NET Core z [IIS](xref:host-and-deploy/iis/index) lub [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="2f0da-106">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

<span data-ttu-id="2f0da-107">Uwierzytelnianie Windows opiera się uwierzytelniać użytkowników aplikacji platformy ASP.NET Core w systemie operacyjnym.</span><span class="sxs-lookup"><span data-stu-id="2f0da-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="2f0da-108">Możesz użyć uwierzytelniania Windows, gdy serwer działa w sieci firmowej przy użyciu tożsamości domeny usługi Active Directory lub konta Windows do identyfikacji użytkowników.</span><span class="sxs-lookup"><span data-stu-id="2f0da-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="2f0da-109">Uwierzytelnianie Windows najlepiej nadaje się do środowisk intranetowych, w którym użytkownicy, aplikacje klienckie i serwery sieci web należą do tej samej domeny Windows.</span><span class="sxs-lookup"><span data-stu-id="2f0da-109">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

> [!NOTE]
> <span data-ttu-id="2f0da-110">Uwierzytelnianie Windows nie jest obsługiwane przy użyciu protokołu HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="2f0da-110">Windows Authentication isn't supported with HTTP/2.</span></span> <span data-ttu-id="2f0da-111">Mogą być wysyłane wezwań do uwierzytelnienia w odpowiedzi HTTP/2, ale klient musi obniżyć wersję usługi do protokołu HTTP/1.1 przed uwierzytelnieniem.</span><span class="sxs-lookup"><span data-stu-id="2f0da-111">Authentication challenges can be sent on HTTP/2 responses, but the client must downgrade to HTTP/1.1 before authenticating.</span></span>

## <a name="iisiis-express"></a><span data-ttu-id="2f0da-112">Usługi IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="2f0da-112">IIS/IIS Express</span></span>

<span data-ttu-id="2f0da-113">Dodawanie usług uwierzytelniania za pomocą wywołania <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> przestrzeni nazw) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2f0da-113">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a><span data-ttu-id="2f0da-114">Ustawienia (debuger) uruchamiania</span><span class="sxs-lookup"><span data-stu-id="2f0da-114">Launch settings (debugger)</span></span>

<span data-ttu-id="2f0da-115">Konfiguracja ustawień uruchamiania ma wpływ tylko na *Properties/launchSettings.json* plików dla usług IIS Express i nie skonfigurować uwierzytelnianie usług IIS dla Windows.</span><span class="sxs-lookup"><span data-stu-id="2f0da-115">Configuration for launch settings only affects the *Properties/launchSettings.json* file for IIS Express and doesn't configure IIS for Windows Authentication.</span></span> <span data-ttu-id="2f0da-116">Konfiguracja serwera została wyjaśniona w [IIS](#iis) sekcji.</span><span class="sxs-lookup"><span data-stu-id="2f0da-116">Server configuration is explained in the [IIS](#iis) section.</span></span>

<span data-ttu-id="2f0da-117">**Aplikacji sieci Web** szablonu dostępnego za pośrednictwem programu Visual Studio lub interfejsu wiersza polecenia platformy .NET Core, można skonfigurować do obsługi uwierzytelniania Windows, która aktualizuje *Properties/launchSettings.json* pliku automatycznie.</span><span class="sxs-lookup"><span data-stu-id="2f0da-117">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2f0da-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f0da-118">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2f0da-119">**Nowy projekt**</span><span class="sxs-lookup"><span data-stu-id="2f0da-119">**New project**</span></span>

1. <span data-ttu-id="2f0da-120">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="2f0da-120">Create a new project.</span></span>
1. <span data-ttu-id="2f0da-121">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2f0da-121">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="2f0da-122">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="2f0da-122">Select **Next**.</span></span>
1. <span data-ttu-id="2f0da-123">Podaj nazwę w **Nazwa projektu** pola.</span><span class="sxs-lookup"><span data-stu-id="2f0da-123">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="2f0da-124">Upewnij się, **lokalizacji** wpis jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="2f0da-124">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="2f0da-125">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="2f0da-125">Select **Create**.</span></span>
1. <span data-ttu-id="2f0da-126">Wybierz **zmiany** w obszarze **uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="2f0da-126">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="2f0da-127">W **Zmień uwierzytelnianie** wybierz **uwierzytelniania Windows**.</span><span class="sxs-lookup"><span data-stu-id="2f0da-127">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="2f0da-128">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f0da-128">Select **OK**.</span></span>
1. <span data-ttu-id="2f0da-129">Wybierz **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="2f0da-129">Select **Web Application**.</span></span>
1. <span data-ttu-id="2f0da-130">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="2f0da-130">Select **Create**.</span></span>

<span data-ttu-id="2f0da-131">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="2f0da-131">Run the app.</span></span> <span data-ttu-id="2f0da-132">Nazwa użytkownika pojawia się w interfejsie użytkownika aplikacji renderowany.</span><span class="sxs-lookup"><span data-stu-id="2f0da-132">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="2f0da-133">**Istniejący projekt**</span><span class="sxs-lookup"><span data-stu-id="2f0da-133">**Existing project**</span></span>

<span data-ttu-id="2f0da-134">Właściwości projektu Windows uwierzytelnianie włączyć i wyłączyć uwierzytelnianie anonimowe:</span><span class="sxs-lookup"><span data-stu-id="2f0da-134">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="2f0da-135">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="2f0da-135">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="2f0da-136">Wybierz **debugowania** kartę.</span><span class="sxs-lookup"><span data-stu-id="2f0da-136">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="2f0da-137">Usuń zaznaczenie pola wyboru dla **Włącz uwierzytelnianie anonimowe**.</span><span class="sxs-lookup"><span data-stu-id="2f0da-137">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="2f0da-138">Zaznacz pole wyboru dla **Włącz uwierzytelnianie Windows**.</span><span class="sxs-lookup"><span data-stu-id="2f0da-138">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="2f0da-139">Zapisz i zamknij stronę właściwości.</span><span class="sxs-lookup"><span data-stu-id="2f0da-139">Save and close the property page.</span></span>

<span data-ttu-id="2f0da-140">Alternatywnie, można skonfigurować właściwości w `iisSettings` węźle *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="2f0da-140">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="2f0da-141">Program Visual Studio Code / .NET Core interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="2f0da-141">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="2f0da-142">**Nowy projekt**</span><span class="sxs-lookup"><span data-stu-id="2f0da-142">**New project**</span></span>

<span data-ttu-id="2f0da-143">Wykonaj [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia `webapp` argumentu (aplikację sieci Web platformy ASP.NET Core) i `--auth Windows` przełącznika:</span><span class="sxs-lookup"><span data-stu-id="2f0da-143">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

<span data-ttu-id="2f0da-144">**Istniejący projekt**</span><span class="sxs-lookup"><span data-stu-id="2f0da-144">**Existing project**</span></span>

<span data-ttu-id="2f0da-145">Aktualizacja `iisSettings` węźle *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="2f0da-145">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="2f0da-146">Podczas modyfikowania istniejącego projektu, upewnij się, że plik projektu zawiera odwołania do pakietu dla [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) **lub** [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="2f0da-146">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

### <a name="iis"></a><span data-ttu-id="2f0da-147">IIS</span><span class="sxs-lookup"><span data-stu-id="2f0da-147">IIS</span></span>

<span data-ttu-id="2f0da-148">Usługi IIS używają [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) hostująca aplikacje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2f0da-148">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="2f0da-149">Uwierzytelnianie Windows jest skonfigurowany dla usług IIS za pomocą *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="2f0da-149">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="2f0da-150">Następujące sekcje show jak:</span><span class="sxs-lookup"><span data-stu-id="2f0da-150">The following sections show how to:</span></span>

* <span data-ttu-id="2f0da-151">Zapewnia lokalny *web.config* pliku, który aktywuje uwierzytelniania Windows na serwerze, gdy aplikacja jest wdrożona.</span><span class="sxs-lookup"><span data-stu-id="2f0da-151">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="2f0da-152">Konfigurowanie za pomocą Menedżera usług IIS *web.config* pliku aplikacji platformy ASP.NET Core, która została już wdrożona na serwerze.</span><span class="sxs-lookup"><span data-stu-id="2f0da-152">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="2f0da-153">Jeśli jeszcze tego nie zrobiono, Włącz usługi IIS do hostowania aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2f0da-153">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="2f0da-154">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="2f0da-154">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="2f0da-155">Włączyć usługi roli usług IIS na potrzeby uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="2f0da-155">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="2f0da-156">Aby uzyskać więcej informacji, zobacz [Włącz uwierzytelnianie Windows usługami roli usług IIS (zobacz krok 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="2f0da-156">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="2f0da-157">[Oprogramowania pośredniczącego integracji usługi IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) jest domyślnie skonfigurowana do automatycznego uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="2f0da-157">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="2f0da-158">Aby uzyskać więcej informacji, zobacz [hosta ASP.NET Core na Windows za pomocą programu IIS: Opcje usług IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="2f0da-158">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="2f0da-159">Modułu ASP.NET Core jest domyślnie skonfigurowana do przekazywania tokenu uwierzytelniania Windows do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2f0da-159">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="2f0da-160">Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji modułu ASP.NET Core: Atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="2f0da-160">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="2f0da-161">Użyj **albo** z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="2f0da-161">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="2f0da-162">**Przed opublikowaniem i wdrażania projektu,** Dodaj następujący kod *web.config* plik do katalogu głównego projektu:</span><span class="sxs-lookup"><span data-stu-id="2f0da-162">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="2f0da-163">Gdy projekt zostanie opublikowany przez zestaw SDK platformy .NET Core (bez `<IsTransformWebConfigDisabled>` właściwością `true` w pliku projektu), opublikowanego *web.config* plik zawiera `<location><system.webServer><security><authentication>` sekcji.</span><span class="sxs-lookup"><span data-stu-id="2f0da-163">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="2f0da-164">Aby uzyskać więcej informacji na temat `<IsTransformWebConfigDisabled>` właściwości, zobacz <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="2f0da-164">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="2f0da-165">**Po publikowania i wdrażania projektu,** przeprowadzenia konfiguracji po stronie serwera za pomocą Menedżera usług IIS:</span><span class="sxs-lookup"><span data-stu-id="2f0da-165">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="2f0da-166">W Menedżerze usług IIS wybierz witrynę IIS, w obszarze **witryn** węźle **połączeń** pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="2f0da-166">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="2f0da-167">Kliknij dwukrotnie **uwierzytelniania** w **IIS** obszaru.</span><span class="sxs-lookup"><span data-stu-id="2f0da-167">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="2f0da-168">Wybierz **uwierzytelnianie anonimowe**.</span><span class="sxs-lookup"><span data-stu-id="2f0da-168">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="2f0da-169">Wybierz **wyłączyć** w **akcje** pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="2f0da-169">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="2f0da-170">Wybierz **uwierzytelniania Windows**.</span><span class="sxs-lookup"><span data-stu-id="2f0da-170">Select **Windows Authentication**.</span></span> <span data-ttu-id="2f0da-171">Wybierz **Włącz** w **akcje** pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="2f0da-171">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="2f0da-172">Kiedy te akcje są wykonywane, Menedżera usług IIS modyfikuje aplikacji *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="2f0da-172">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="2f0da-173">A `<system.webServer><security><authentication>` węzeł zostanie dodany ze zaktualizowanymi ustawieniami dla `anonymousAuthentication` i `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="2f0da-173">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="2f0da-174">`<system.webServer>` Dodany do sekcji *web.config* pliku przez Menedżera usług IIS znajduje się poza jej `<location>` sekcji dodawane przez program .NET Core SDK po opublikowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2f0da-174">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="2f0da-175">Ponieważ sekcji zostanie dodany poza `<location>` węzła, ustawienia są dziedziczone przez żaden [aplikacji podrzędnych](xref:host-and-deploy/iis/index#sub-applications) do bieżącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2f0da-175">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="2f0da-176">Aby zapobiec dziedziczenia, Przenieś dodany `<security>` sekcji wewnątrz `<location><system.webServer>` sekcji podanym .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="2f0da-176">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="2f0da-177">W przypadku Menedżera usług IIS można dodać konfiguracji usług IIS dotyczy tylko aplikacji *web.config* pliku na serwerze.</span><span class="sxs-lookup"><span data-stu-id="2f0da-177">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="2f0da-178">Kolejne wdrożenie aplikacji może zastąpić ustawienia na serwerze, jeśli kopię serwera *web.config* zastępuje projektu *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="2f0da-178">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="2f0da-179">Użyj **albo** z następujących metod do zarządzania ustawieniami:</span><span class="sxs-lookup"><span data-stu-id="2f0da-179">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="2f0da-180">Użyj Menedżera usług IIS, aby zresetować ustawienia w *web.config* pliku po plik jest zastępowany we wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="2f0da-180">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="2f0da-181">Dodaj *pliku web.config* aplikacji lokalnie przy użyciu ustawień.</span><span class="sxs-lookup"><span data-stu-id="2f0da-181">Add a *web.config file* to the app locally with the settings.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="kestrel"></a><span data-ttu-id="2f0da-182">Kestrel</span><span class="sxs-lookup"><span data-stu-id="2f0da-182">Kestrel</span></span>

 <span data-ttu-id="2f0da-183">[Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) pakietu NuGet może być używany z [Kestrel](xref:fundamentals/servers/kestrel) do obsługi uwierzytelniania Windows przy użyciu Negotiate, Kerberos i NTLM w Windows, Linux i macOS.</span><span class="sxs-lookup"><span data-stu-id="2f0da-183">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) NuGet package can be used with [Kestrel](xref:fundamentals/servers/kestrel) to support Windows Authentication using Negotiate, Kerberos, and NTLM on Windows, Linux, and macOS.</span></span>

> [!WARNING]
> <span data-ttu-id="2f0da-184">Poświadczenia mogą zostać utrwalone na żądań połączenia.</span><span class="sxs-lookup"><span data-stu-id="2f0da-184">Credentials can be persisted across requests on a connection.</span></span> <span data-ttu-id="2f0da-185">*Negocjowania uwierzytelniania nie może być używany z serwerami proxy, chyba że serwer proxy przechowuje koligacji połączenia 1:1 (trwałe połączenie), za pomocą Kestrel.*</span><span class="sxs-lookup"><span data-stu-id="2f0da-185">*Negotiate authentication must not be used with proxies unless the proxy maintains a 1:1 connection affinity (a persistent connection) with Kestrel.*</span></span> <span data-ttu-id="2f0da-186">Oznacza to, że uwierzytelniania Negotiate nie mogą być używane z Kestrel za IIS [poza procesem programu ASP.NET Core modułu (ANCM)](xref:host-and-deploy/iis/index#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="2f0da-186">This means that Negotiate authentication must not be used with Kestrel behind the IIS [ASP.NET Core Module (ANCM) out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model).</span></span>

 <span data-ttu-id="2f0da-187">Dodawanie usług uwierzytelniania za pomocą wywołania <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` przestrzeni nazw) i `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` przestrzeni nazw) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2f0da-187">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` namespace) and `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` namespace) in `Startup.ConfigureServices`:</span></span>

 ```csharp
services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
    .AddNegotiate();
```

<span data-ttu-id="2f0da-188">Dodaj oprogramowanie pośredniczące uwierzytelniania przez wywołanie metody <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="2f0da-188">Add Authentication Middleware by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> in `Startup.Configure`:</span></span>

 ```csharp
app.UseAuthentication();

app.UseMvc();
```

<span data-ttu-id="2f0da-189">Aby uzyskać więcej informacji na temat oprogramowania pośredniczącego, zobacz <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="2f0da-189">For more information on middleware, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="2f0da-190">Anonimowe żądania są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="2f0da-190">Anonymous requests are allowed.</span></span> <span data-ttu-id="2f0da-191">Użyj [autoryzacja w programie ASP.NET Core](xref:security/authorization/introduction) zażąda anonimowe żądania uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="2f0da-191">Use [ASP.NET Core Authorization](xref:security/authorization/introduction) to challenge anonymous requests for authentication.</span></span>

### <a name="windows-environment-configuration"></a><span data-ttu-id="2f0da-192">Konfiguracja środowiska Windows</span><span class="sxs-lookup"><span data-stu-id="2f0da-192">Windows environment configuration</span></span>

<span data-ttu-id="2f0da-193">[Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) składnik przeprowadza uwierzytelnianie w trybie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2f0da-193">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) component performs User Mode authentication.</span></span> <span data-ttu-id="2f0da-194">Główne nazwy usług (SPN), należy dodać do konta użytkownika uruchamiającego usługi, a nie na koncie komputera.</span><span class="sxs-lookup"><span data-stu-id="2f0da-194">Service Principal Names (SPNs) must be added to the user account running the service, not the machine account.</span></span> <span data-ttu-id="2f0da-195">Wykonaj `setspn -S HTTP/mysrevername.mydomain.com myuser` w powłoce poleceń administracyjnych.</span><span class="sxs-lookup"><span data-stu-id="2f0da-195">Execute `setspn -S HTTP/mysrevername.mydomain.com myuser` in an administrative command shell.</span></span>

### <a name="linux-and-macos-environment-configuration"></a><span data-ttu-id="2f0da-196">Konfiguracja środowiska systemu Linux i macOS</span><span class="sxs-lookup"><span data-stu-id="2f0da-196">Linux and macOS environment configuration</span></span>

<span data-ttu-id="2f0da-197">Instrukcje dotyczące przyłączania komputera z systemem Linux lub macOS do domeny Windows są dostępne w [Połącz Studio danych platformy Azure z serwerem SQL przy użyciu uwierzytelniania Windows - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) artykułu.</span><span class="sxs-lookup"><span data-stu-id="2f0da-197">Instructions for joining a Linux or macOS machine to a Windows domain are available in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article.</span></span> <span data-ttu-id="2f0da-198">Instrukcje Utwórz konto komputera dla maszyny z systemem Linux w domenie.</span><span class="sxs-lookup"><span data-stu-id="2f0da-198">The instructions create a machine account for the Linux machine on the domain.</span></span> <span data-ttu-id="2f0da-199">Nazwy SPN należy dodać do tego konta maszyny.</span><span class="sxs-lookup"><span data-stu-id="2f0da-199">SPNs must be added to that machine account.</span></span>

> [!NOTE]
> <span data-ttu-id="2f0da-200">Gdy postępując zgodnie ze wskazówkami w [Połącz Studio danych platformy Azure z serwerem SQL przy użyciu uwierzytelniania Windows - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) artykuł, Zastąp `python-software-properties` z `python3-software-properties` w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="2f0da-200">When following the guidance in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article, replace `python-software-properties` with `python3-software-properties` if needed.</span></span>

<span data-ttu-id="2f0da-201">Po w systemie Linux lub macOS komputer jest przyłączony do domeny, są wymagane dodatkowe kroki w celu zapewnienia [pliku keytab](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) za pomocą nazw SPN:</span><span class="sxs-lookup"><span data-stu-id="2f0da-201">Once the Linux or macOS machine is joined to the domain, additional steps are required to provide a [keytab file](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) with the SPNs:</span></span>

* <span data-ttu-id="2f0da-202">Na kontrolerze domeny Dodaj nową usługę sieci web nazwy SPN konta na komputerze:</span><span class="sxs-lookup"><span data-stu-id="2f0da-202">On the domain controller, add new web service SPNs to the machine account:</span></span>
  * `setspn -S HTTP/mywebservice.mydomain.com mymachine`
  * `setspn -S HTTP/mywebservice@MYDOMAIN.COM mymachine`
* <span data-ttu-id="2f0da-203">Użyj [ktpass](/windows-server/administration/windows-commands/ktpass) do generowania pliku keytab:</span><span class="sxs-lookup"><span data-stu-id="2f0da-203">Use [ktpass](/windows-server/administration/windows-commands/ktpass) to generate a keytab file:</span></span>
  * `ktpass -princ HTTP/mywebservice.mydomain.com@MYDOMAIN.COM -pass myKeyTabFilePassword -mapuser MYDOMAIN\mymachine$ -pType KRB5_NT_PRINCIPAL -out c:\temp\mymachine.HTTP.keytab -crypto AES256-SHA1`
  * <span data-ttu-id="2f0da-204">Niektóre pola musi być określona w wielkie wskazane.</span><span class="sxs-lookup"><span data-stu-id="2f0da-204">Some fields must be specified in uppercase as indicated.</span></span>
* <span data-ttu-id="2f0da-205">Skopiuj plik keytab do komputera w systemie Linux lub macOS.</span><span class="sxs-lookup"><span data-stu-id="2f0da-205">Copy the keytab file to the Linux or macOS machine.</span></span>
* <span data-ttu-id="2f0da-206">Wybierz plik keytab za pośrednictwem zmiennej środowiskowej: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span><span class="sxs-lookup"><span data-stu-id="2f0da-206">Select the keytab file via an environment variable: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span></span>
* <span data-ttu-id="2f0da-207">Wywoływanie `klist` aby pokazać nazwy SPN aktualnie dostępne do użycia.</span><span class="sxs-lookup"><span data-stu-id="2f0da-207">Invoke `klist` to show the SPNs currently available for use.</span></span>

> [!NOTE]
> <span data-ttu-id="2f0da-208">Plik keytab zawiera poświadczenia dostępu do domeny i muszą być odpowiednio chronione.</span><span class="sxs-lookup"><span data-stu-id="2f0da-208">A keytab file contains domain access credentials and must be protected accordingly.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="2f0da-209">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="2f0da-209">HTTP.sys</span></span>

<span data-ttu-id="2f0da-210">[Sterownik HTTP.sys](xref:fundamentals/servers/httpsys) obsługuje Windows uwierzytelnianie trybu jądra, przy użyciu Negotiate, NTLM lub uwierzytelniania podstawowego.</span><span class="sxs-lookup"><span data-stu-id="2f0da-210">[HTTP.sys](xref:fundamentals/servers/httpsys) supports Kernel Mode Windows Authentication using Negotiate, NTLM, or Basic authentication.</span></span>

<span data-ttu-id="2f0da-211">Dodawanie usług uwierzytelniania za pomocą wywołania <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> przestrzeni nazw) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2f0da-211">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="2f0da-212">Konfigurowanie aplikacji sieci web hosta HTTP.sys za pomocą uwierzytelniania Windows (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="2f0da-212">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="2f0da-213"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> Trwa <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="2f0da-213"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="2f0da-214">Sterownik HTTP.sys delegatów, aby uwierzytelnianie trybu jądra za pomocą protokołu uwierzytelniania Kerberos.</span><span class="sxs-lookup"><span data-stu-id="2f0da-214">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="2f0da-215">Uwierzytelnianie w trybie użytkownika nie jest obsługiwana przy użyciu protokołu Kerberos i sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="2f0da-215">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="2f0da-216">Konto komputera należy używany do odszyfrowywania tokenu/biletu Kerberos uzyskany z usługi Active Directory i przesyłany dalej przez klienta do serwera w celu uwierzytelnienia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2f0da-216">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="2f0da-217">Rejestrowanie głównej nazwy usługi (SPN) dla hosta, a nie użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2f0da-217">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="2f0da-218">Sterownik HTTP.sys nie jest obsługiwana na serwerze Nano Server w wersji 1709 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="2f0da-218">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="2f0da-219">Aby użyć uwierzytelniania Windows i sterownik HTTP.sys systemu Nano Server, użyj [Server Core (microsoft/windowsservercore) kontenera](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="2f0da-219">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="2f0da-220">Aby uzyskać więcej informacji na temat Server Core, zobacz [co to jest opcja instalacji Server Core w systemie Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="2f0da-220">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="2f0da-221">Autoryzowanie użytkowników</span><span class="sxs-lookup"><span data-stu-id="2f0da-221">Authorize users</span></span>

<span data-ttu-id="2f0da-222">Stan konfiguracji dostępu anonimowego określa sposób, w którym `[Authorize]` i `[AllowAnonymous]` atrybuty są używane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2f0da-222">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="2f0da-223">W poniższych sekcjach dwóch opisano sposób obsługi konfiguracji niedozwolonych i dozwolone stany dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="2f0da-223">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="2f0da-224">Nie zezwalaj na dostęp anonimowy</span><span class="sxs-lookup"><span data-stu-id="2f0da-224">Disallow anonymous access</span></span>

<span data-ttu-id="2f0da-225">Gdy jest włączone uwierzytelnianie Windows i dostęp anonimowy jest wyłączony, `[Authorize]` i `[AllowAnonymous]` atrybuty mają żadnego skutku.</span><span class="sxs-lookup"><span data-stu-id="2f0da-225">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="2f0da-226">Jeśli witryna usług IIS jest skonfigurowany do nie zezwalaj na dostęp anonimowy, żądanie nigdy nie osiągnie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2f0da-226">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="2f0da-227">Z tego powodu `[AllowAnonymous]` atrybut nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="2f0da-227">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="2f0da-228">Zezwalaj na dostęp anonimowy</span><span class="sxs-lookup"><span data-stu-id="2f0da-228">Allow anonymous access</span></span>

<span data-ttu-id="2f0da-229">Po włączeniu uwierzytelniania Windows i dostęp anonimowy używać `[Authorize]` i `[AllowAnonymous]` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="2f0da-229">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="2f0da-230">`[Authorize]` Atrybut umożliwia bezpieczne punkty końcowe aplikacji, które wymagają uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="2f0da-230">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="2f0da-231">`[AllowAnonymous]` Atrybutu zastąpienia `[Authorize]` atrybutów w aplikacjach, które zezwala na dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="2f0da-231">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="2f0da-232">Atrybut szczegóły użycia można znaleźć <xref:security/authorization/simple>.</span><span class="sxs-lookup"><span data-stu-id="2f0da-232">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="2f0da-233">Domyślnie użytkownicy, którzy nie mają autoryzacji do dostępu do strony są prezentowane z pustą odpowiedź HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="2f0da-233">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="2f0da-234">[Oprogramowania pośredniczącego StatusCodePages](xref:fundamentals/error-handling#usestatuscodepages) można skonfigurować, aby zapewnić użytkownikom lepsze środowisko "Odmowa dostępu".</span><span class="sxs-lookup"><span data-stu-id="2f0da-234">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="2f0da-235">Personifikacja</span><span class="sxs-lookup"><span data-stu-id="2f0da-235">Impersonation</span></span>

<span data-ttu-id="2f0da-236">Platforma ASP.NET Core nie implementuje personifikacji.</span><span class="sxs-lookup"><span data-stu-id="2f0da-236">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="2f0da-237">Aplikacje są uruchamiane przy użyciu tożsamości przez aplikację dla wszystkich żądań, przy użyciu tożsamości puli lub procesu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2f0da-237">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="2f0da-238">Jeśli aplikacja powinna wykonać akcję w imieniu użytkownika, należy użyć [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) w [oprogramowania pośredniczącego terminalu wbudowane](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="2f0da-238">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="2f0da-239">Uruchomić jedną akcję w tym kontekście, a następnie zamknij kontekstu.</span><span class="sxs-lookup"><span data-stu-id="2f0da-239">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="2f0da-240">`RunImpersonated` nie obsługuje operacji asynchronicznych i nie powinny być używane w przypadku złożonych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="2f0da-240">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="2f0da-241">Na przykład zawijania całego żądania lub łańcuchów oprogramowanie pośredniczące nie jest obsługiwany lub zalecane.</span><span class="sxs-lookup"><span data-stu-id="2f0da-241">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2f0da-242">Gdy [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) pakiet umożliwia uwierzytelnianie na Windows, Linux i macOS, personifikacja jest obsługiwana tylko na Windows.</span><span class="sxs-lookup"><span data-stu-id="2f0da-242">While the [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package enables authentication on Windows, Linux, and macOS, impersonation is only supported on Windows.</span></span>

::: moniker-end

## <a name="claims-transformations"></a><span data-ttu-id="2f0da-243">Przekształcenia oświadczeń</span><span class="sxs-lookup"><span data-stu-id="2f0da-243">Claims transformations</span></span>

<span data-ttu-id="2f0da-244">W przypadku hostowania w trybie usług IIS w trakcie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wewnętrznie wywoływana w celu zainicjowania przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2f0da-244">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="2f0da-245">W związku z tym <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementacji używanego do przekształcania oświadczeń, po każdym uwierzytelniania nie jest aktywowana domyślnie.</span><span class="sxs-lookup"><span data-stu-id="2f0da-245">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="2f0da-246">Aby uzyskać więcej informacji i przykładowy kod, który aktywuje przekształcenia oświadczeń w przypadku hostowania w procesie, zobacz <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="2f0da-246">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2f0da-247">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2f0da-247">Additional resources</span></span>

* [<span data-ttu-id="2f0da-248">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="2f0da-248">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
