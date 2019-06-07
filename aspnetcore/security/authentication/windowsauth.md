---
title: Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core
author: scottaddie
description: Dowiedz się, jak skonfigurować uwierzytelnianie Windows w programie ASP.NET Core dla usług IIS i sterownik HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/05/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 900bbf5f14b1876ad537b2b77e4ba07d7aa168f2
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750166"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="6eb00-103">Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6eb00-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="6eb00-104">Przez [Scott Addie](https://twitter.com/Scott_Addie) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6eb00-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6eb00-105">[Uwierzytelnianie Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) można skonfigurować dla aplikacji platformy ASP.NET Core z [IIS](xref:host-and-deploy/iis/index) lub [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="6eb00-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="6eb00-106">Uwierzytelnianie Windows opiera się uwierzytelniać użytkowników aplikacji platformy ASP.NET Core w systemie operacyjnym.</span><span class="sxs-lookup"><span data-stu-id="6eb00-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="6eb00-107">Możesz użyć uwierzytelniania Windows, gdy serwer działa w sieci firmowej przy użyciu tożsamości domeny usługi Active Directory lub konta Windows do identyfikacji użytkowników.</span><span class="sxs-lookup"><span data-stu-id="6eb00-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="6eb00-108">Uwierzytelnianie Windows najlepiej nadaje się do środowisk intranetowych, w którym użytkownicy, aplikacje klienckie i serwery sieci web należą do tej samej domeny Windows.</span><span class="sxs-lookup"><span data-stu-id="6eb00-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="iisiis-express"></a><span data-ttu-id="6eb00-109">Usługi IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="6eb00-109">IIS/IIS Express</span></span>

<span data-ttu-id="6eb00-110">Dodawanie usług uwierzytelniania za pomocą wywołania <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> przestrzeni nazw) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6eb00-110">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a><span data-ttu-id="6eb00-111">Ustawienia (debuger) uruchamiania</span><span class="sxs-lookup"><span data-stu-id="6eb00-111">Launch settings (debugger)</span></span>

<span data-ttu-id="6eb00-112">Konfiguracja ustawień uruchamiania ma wpływ tylko na *Properties/launchSettings.json* plików dla usług IIS Express i nie skonfigurować uwierzytelnianie usług IIS dla Windows.</span><span class="sxs-lookup"><span data-stu-id="6eb00-112">Configuration for launch settings only affects the *Properties/launchSettings.json* file for IIS Express and doesn't configure IIS for Windows Authentication.</span></span> <span data-ttu-id="6eb00-113">Konfiguracja serwera została wyjaśniona w [IIS](#iis) sekcji.</span><span class="sxs-lookup"><span data-stu-id="6eb00-113">Server configuration is explained in the [IIS](#iis) section.</span></span>

<span data-ttu-id="6eb00-114">**Aplikacji sieci Web** szablonu dostępnego za pośrednictwem programu Visual Studio lub interfejsu wiersza polecenia platformy .NET Core, można skonfigurować do obsługi uwierzytelniania Windows, która aktualizuje *Properties/launchSettings.json* pliku automatycznie.</span><span class="sxs-lookup"><span data-stu-id="6eb00-114">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6eb00-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6eb00-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6eb00-116">**Nowy projekt**</span><span class="sxs-lookup"><span data-stu-id="6eb00-116">**New project**</span></span>

1. <span data-ttu-id="6eb00-117">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="6eb00-117">Create a new project.</span></span>
1. <span data-ttu-id="6eb00-118">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6eb00-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="6eb00-119">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="6eb00-119">Select **Next**.</span></span>
1. <span data-ttu-id="6eb00-120">Podaj nazwę w **Nazwa projektu** pola.</span><span class="sxs-lookup"><span data-stu-id="6eb00-120">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="6eb00-121">Upewnij się, **lokalizacji** wpis jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="6eb00-121">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="6eb00-122">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="6eb00-122">Select **Create**.</span></span>
1. <span data-ttu-id="6eb00-123">Wybierz **zmiany** w obszarze **uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="6eb00-123">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="6eb00-124">W **Zmień uwierzytelnianie** wybierz **uwierzytelniania Windows**.</span><span class="sxs-lookup"><span data-stu-id="6eb00-124">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="6eb00-125">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6eb00-125">Select **OK**.</span></span>
1. <span data-ttu-id="6eb00-126">Wybierz **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="6eb00-126">Select **Web Application**.</span></span>
1. <span data-ttu-id="6eb00-127">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="6eb00-127">Select **Create**.</span></span>

<span data-ttu-id="6eb00-128">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="6eb00-128">Run the app.</span></span> <span data-ttu-id="6eb00-129">Nazwa użytkownika pojawia się w interfejsie użytkownika aplikacji renderowany.</span><span class="sxs-lookup"><span data-stu-id="6eb00-129">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="6eb00-130">**Istniejący projekt**</span><span class="sxs-lookup"><span data-stu-id="6eb00-130">**Existing project**</span></span>

<span data-ttu-id="6eb00-131">Właściwości projektu Windows uwierzytelnianie włączyć i wyłączyć uwierzytelnianie anonimowe:</span><span class="sxs-lookup"><span data-stu-id="6eb00-131">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="6eb00-132">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="6eb00-132">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="6eb00-133">Wybierz **debugowania** kartę.</span><span class="sxs-lookup"><span data-stu-id="6eb00-133">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="6eb00-134">Usuń zaznaczenie pola wyboru dla **Włącz uwierzytelnianie anonimowe**.</span><span class="sxs-lookup"><span data-stu-id="6eb00-134">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="6eb00-135">Zaznacz pole wyboru dla **Włącz uwierzytelnianie Windows**.</span><span class="sxs-lookup"><span data-stu-id="6eb00-135">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="6eb00-136">Zapisz i zamknij stronę właściwości.</span><span class="sxs-lookup"><span data-stu-id="6eb00-136">Save and close the property page.</span></span>

<span data-ttu-id="6eb00-137">Alternatywnie, można skonfigurować właściwości w `iisSettings` węźle *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="6eb00-137">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="6eb00-138">Program Visual Studio Code / .NET Core interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="6eb00-138">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="6eb00-139">**Nowy projekt**</span><span class="sxs-lookup"><span data-stu-id="6eb00-139">**New project**</span></span>

<span data-ttu-id="6eb00-140">Wykonaj [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia `webapp` argumentu (aplikację sieci Web platformy ASP.NET Core) i `--auth Windows` przełącznika:</span><span class="sxs-lookup"><span data-stu-id="6eb00-140">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

<span data-ttu-id="6eb00-141">**Istniejący projekt**</span><span class="sxs-lookup"><span data-stu-id="6eb00-141">**Existing project**</span></span>

<span data-ttu-id="6eb00-142">Aktualizacja `iisSettings` węźle *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="6eb00-142">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="6eb00-143">Podczas modyfikowania istniejącego projektu, upewnij się, że plik projektu zawiera odwołania do pakietu dla [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) **lub** [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="6eb00-143">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

### <a name="iis"></a><span data-ttu-id="6eb00-144">IIS</span><span class="sxs-lookup"><span data-stu-id="6eb00-144">IIS</span></span>

<span data-ttu-id="6eb00-145">Usługi IIS używają [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) hostująca aplikacje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6eb00-145">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="6eb00-146">Uwierzytelnianie Windows jest skonfigurowany dla usług IIS za pomocą *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="6eb00-146">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="6eb00-147">Następujące sekcje show jak:</span><span class="sxs-lookup"><span data-stu-id="6eb00-147">The following sections show how to:</span></span>

* <span data-ttu-id="6eb00-148">Zapewnia lokalny *web.config* pliku, który aktywuje uwierzytelniania Windows na serwerze, gdy aplikacja jest wdrożona.</span><span class="sxs-lookup"><span data-stu-id="6eb00-148">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="6eb00-149">Konfigurowanie za pomocą Menedżera usług IIS *web.config* pliku aplikacji platformy ASP.NET Core, która została już wdrożona na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6eb00-149">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="6eb00-150">Jeśli jeszcze tego nie zrobiono, Włącz usługi IIS do hostowania aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6eb00-150">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="6eb00-151">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="6eb00-151">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="6eb00-152">Włączyć usługi roli usług IIS na potrzeby uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="6eb00-152">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="6eb00-153">Aby uzyskać więcej informacji, zobacz [Włącz uwierzytelnianie Windows usługami roli usług IIS (zobacz krok 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="6eb00-153">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="6eb00-154">[Oprogramowania pośredniczącego integracji usługi IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) jest domyślnie skonfigurowana do automatycznego uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="6eb00-154">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="6eb00-155">Aby uzyskać więcej informacji, zobacz [hosta ASP.NET Core na Windows za pomocą programu IIS: Opcje usług IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="6eb00-155">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="6eb00-156">Modułu ASP.NET Core jest domyślnie skonfigurowana do przekazywania tokenu uwierzytelniania Windows do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6eb00-156">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="6eb00-157">Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji modułu ASP.NET Core: Atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="6eb00-157">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="6eb00-158">Użyj **albo** z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="6eb00-158">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="6eb00-159">**Przed opublikowaniem i wdrażania projektu,** Dodaj następujący kod *web.config* plik do katalogu głównego projektu:</span><span class="sxs-lookup"><span data-stu-id="6eb00-159">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="6eb00-160">Gdy projekt zostanie opublikowany przez zestaw SDK platformy .NET Core (bez `<IsTransformWebConfigDisabled>` właściwością `true` w pliku projektu), opublikowanego *web.config* plik zawiera `<location><system.webServer><security><authentication>` sekcji.</span><span class="sxs-lookup"><span data-stu-id="6eb00-160">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="6eb00-161">Aby uzyskać więcej informacji na temat `<IsTransformWebConfigDisabled>` właściwości, zobacz <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="6eb00-161">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="6eb00-162">**Po publikowania i wdrażania projektu,** przeprowadzenia konfiguracji po stronie serwera za pomocą Menedżera usług IIS:</span><span class="sxs-lookup"><span data-stu-id="6eb00-162">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="6eb00-163">W Menedżerze usług IIS wybierz witrynę IIS, w obszarze **witryn** węźle **połączeń** pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="6eb00-163">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="6eb00-164">Kliknij dwukrotnie **uwierzytelniania** w **IIS** obszaru.</span><span class="sxs-lookup"><span data-stu-id="6eb00-164">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="6eb00-165">Wybierz **uwierzytelnianie anonimowe**.</span><span class="sxs-lookup"><span data-stu-id="6eb00-165">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="6eb00-166">Wybierz **wyłączyć** w **akcje** pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="6eb00-166">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="6eb00-167">Wybierz **uwierzytelniania Windows**.</span><span class="sxs-lookup"><span data-stu-id="6eb00-167">Select **Windows Authentication**.</span></span> <span data-ttu-id="6eb00-168">Wybierz **Włącz** w **akcje** pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="6eb00-168">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="6eb00-169">Kiedy te akcje są wykonywane, Menedżera usług IIS modyfikuje aplikacji *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="6eb00-169">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="6eb00-170">A `<system.webServer><security><authentication>` węzeł zostanie dodany ze zaktualizowanymi ustawieniami dla `anonymousAuthentication` i `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="6eb00-170">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="6eb00-171">`<system.webServer>` Dodany do sekcji *web.config* pliku przez Menedżera usług IIS znajduje się poza jej `<location>` sekcji dodawane przez program .NET Core SDK po opublikowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6eb00-171">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="6eb00-172">Ponieważ sekcji zostanie dodany poza `<location>` węzła, ustawienia są dziedziczone przez żaden [aplikacji podrzędnych](xref:host-and-deploy/iis/index#sub-applications) do bieżącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6eb00-172">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="6eb00-173">Aby zapobiec dziedziczenia, Przenieś dodany `<security>` sekcji wewnątrz `<location><system.webServer>` sekcji podanym .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="6eb00-173">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="6eb00-174">W przypadku Menedżera usług IIS można dodać konfiguracji usług IIS dotyczy tylko aplikacji *web.config* pliku na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6eb00-174">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="6eb00-175">Kolejne wdrożenie aplikacji może zastąpić ustawienia na serwerze, jeśli kopię serwera *web.config* zastępuje projektu *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="6eb00-175">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="6eb00-176">Użyj **albo** z następujących metod do zarządzania ustawieniami:</span><span class="sxs-lookup"><span data-stu-id="6eb00-176">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="6eb00-177">Użyj Menedżera usług IIS, aby zresetować ustawienia w *web.config* pliku po plik jest zastępowany we wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="6eb00-177">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="6eb00-178">Dodaj *pliku web.config* aplikacji lokalnie przy użyciu ustawień.</span><span class="sxs-lookup"><span data-stu-id="6eb00-178">Add a *web.config file* to the app locally with the settings.</span></span>

## <a name="httpsys"></a><span data-ttu-id="6eb00-179">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="6eb00-179">HTTP.sys</span></span>

<span data-ttu-id="6eb00-180">W scenariuszach Self-Hosted [Kestrel](xref:fundamentals/servers/kestrel) nie obsługę uwierzytelniania Windows, ale Wy możecie użyć [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="6eb00-180">In self-hosted scenarios, [Kestrel](xref:fundamentals/servers/kestrel) doesn't support Windows Authentication, but you can use [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="6eb00-181">Dodawanie usług uwierzytelniania za pomocą wywołania <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> przestrzeni nazw) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6eb00-181">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="6eb00-182">Konfigurowanie aplikacji sieci web hosta HTTP.sys za pomocą uwierzytelniania Windows (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="6eb00-182">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="6eb00-183"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> Trwa <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="6eb00-183"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="6eb00-184">Sterownik HTTP.sys delegatów, aby uwierzytelnianie trybu jądra za pomocą protokołu uwierzytelniania Kerberos.</span><span class="sxs-lookup"><span data-stu-id="6eb00-184">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="6eb00-185">Uwierzytelnianie w trybie użytkownika nie jest obsługiwana przy użyciu protokołu Kerberos i sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="6eb00-185">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="6eb00-186">Konto komputera należy używany do odszyfrowywania tokenu/biletu Kerberos uzyskany z usługi Active Directory i przesyłany dalej przez klienta do serwera w celu uwierzytelnienia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6eb00-186">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="6eb00-187">Rejestrowanie głównej nazwy usługi (SPN) dla hosta, a nie użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6eb00-187">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="6eb00-188">Sterownik HTTP.sys nie jest obsługiwana na serwerze Nano Server w wersji 1709 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="6eb00-188">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="6eb00-189">Aby użyć uwierzytelniania Windows i sterownik HTTP.sys systemu Nano Server, użyj [Server Core (microsoft/windowsservercore) kontenera](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="6eb00-189">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="6eb00-190">Aby uzyskać więcej informacji na temat Server Core, zobacz [co to jest opcja instalacji Server Core w systemie Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="6eb00-190">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="6eb00-191">Autoryzowanie użytkowników</span><span class="sxs-lookup"><span data-stu-id="6eb00-191">Authorize users</span></span>

<span data-ttu-id="6eb00-192">Stan konfiguracji dostępu anonimowego określa sposób, w którym `[Authorize]` i `[AllowAnonymous]` atrybuty są używane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6eb00-192">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="6eb00-193">W poniższych sekcjach dwóch opisano sposób obsługi konfiguracji niedozwolonych i dozwolone stany dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="6eb00-193">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="6eb00-194">Nie zezwalaj na dostęp anonimowy</span><span class="sxs-lookup"><span data-stu-id="6eb00-194">Disallow anonymous access</span></span>

<span data-ttu-id="6eb00-195">Gdy jest włączone uwierzytelnianie Windows i dostęp anonimowy jest wyłączony, `[Authorize]` i `[AllowAnonymous]` atrybuty mają żadnego skutku.</span><span class="sxs-lookup"><span data-stu-id="6eb00-195">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="6eb00-196">Jeśli witryna usług IIS jest skonfigurowany do nie zezwalaj na dostęp anonimowy, żądanie nigdy nie osiągnie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6eb00-196">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="6eb00-197">Z tego powodu `[AllowAnonymous]` atrybut nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="6eb00-197">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="6eb00-198">Zezwalaj na dostęp anonimowy</span><span class="sxs-lookup"><span data-stu-id="6eb00-198">Allow anonymous access</span></span>

<span data-ttu-id="6eb00-199">Po włączeniu uwierzytelniania Windows i dostęp anonimowy używać `[Authorize]` i `[AllowAnonymous]` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="6eb00-199">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="6eb00-200">`[Authorize]` Atrybut umożliwia bezpieczne punkty końcowe aplikacji, które wymagają uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="6eb00-200">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="6eb00-201">`[AllowAnonymous]` Atrybutu zastąpienia `[Authorize]` atrybutów w aplikacjach, które zezwala na dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="6eb00-201">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="6eb00-202">Atrybut szczegóły użycia można znaleźć <xref:security/authorization/simple>.</span><span class="sxs-lookup"><span data-stu-id="6eb00-202">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="6eb00-203">Domyślnie użytkownicy, którzy nie mają autoryzacji do dostępu do strony są prezentowane z pustą odpowiedź HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="6eb00-203">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="6eb00-204">[Oprogramowania pośredniczącego StatusCodePages](xref:fundamentals/error-handling#usestatuscodepages) można skonfigurować, aby zapewnić użytkownikom lepsze środowisko "Odmowa dostępu".</span><span class="sxs-lookup"><span data-stu-id="6eb00-204">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="6eb00-205">Personifikacja</span><span class="sxs-lookup"><span data-stu-id="6eb00-205">Impersonation</span></span>

<span data-ttu-id="6eb00-206">Platforma ASP.NET Core nie implementuje personifikacji.</span><span class="sxs-lookup"><span data-stu-id="6eb00-206">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="6eb00-207">Aplikacje są uruchamiane przy użyciu tożsamości przez aplikację dla wszystkich żądań, przy użyciu tożsamości puli lub procesu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6eb00-207">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="6eb00-208">Jeśli aplikacja powinna wykonać akcję w imieniu użytkownika, należy użyć [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) w [oprogramowania pośredniczącego terminalu wbudowane](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="6eb00-208">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="6eb00-209">Uruchomić jedną akcję w tym kontekście, a następnie zamknij kontekstu.</span><span class="sxs-lookup"><span data-stu-id="6eb00-209">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="6eb00-210">`RunImpersonated` nie obsługuje operacji asynchronicznych i nie powinny być używane w przypadku złożonych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="6eb00-210">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="6eb00-211">Na przykład zawijania całego żądania lub łańcuchów oprogramowanie pośredniczące nie jest obsługiwany lub zalecane.</span><span class="sxs-lookup"><span data-stu-id="6eb00-211">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

## <a name="claims-transformations"></a><span data-ttu-id="6eb00-212">Przekształcenia oświadczeń</span><span class="sxs-lookup"><span data-stu-id="6eb00-212">Claims transformations</span></span>

<span data-ttu-id="6eb00-213">W przypadku hostowania w trybie usług IIS w trakcie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wewnętrznie wywoływana w celu zainicjowania przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6eb00-213">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="6eb00-214">W związku z tym <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementacji używanego do przekształcania oświadczeń, po każdym uwierzytelniania nie jest aktywowana domyślnie.</span><span class="sxs-lookup"><span data-stu-id="6eb00-214">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="6eb00-215">Aby uzyskać więcej informacji i przykładowy kod, który aktywuje przekształcenia oświadczeń w przypadku hostowania w procesie, zobacz <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="6eb00-215">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6eb00-216">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6eb00-216">Additional resources</span></span>

* [<span data-ttu-id="6eb00-217">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="6eb00-217">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
