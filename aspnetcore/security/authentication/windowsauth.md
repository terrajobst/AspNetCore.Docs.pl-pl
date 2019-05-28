---
title: Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core
author: scottaddie
description: Dowiedz się, jak skonfigurować uwierzytelnianie Windows w programie ASP.NET Core, za pomocą usług IIS Express, usługi IIS i sterownik HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 04/03/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 46c9e054b6e9228a709f93c6b73772708f6c6eb0
ms.sourcegitcommit: b8ed594ab9f47fa32510574f3e1b210cff000967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66251227"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="09495-103">Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09495-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="09495-104">Przez [Scott Addie](https://twitter.com/Scott_Addie) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="09495-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="09495-105">[Uwierzytelnianie Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) można skonfigurować dla aplikacji platformy ASP.NET Core z [IIS](xref:host-and-deploy/iis/index) lub [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="09495-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="09495-106">Uwierzytelnianie Windows opiera się uwierzytelniać użytkowników aplikacji platformy ASP.NET Core w systemie operacyjnym.</span><span class="sxs-lookup"><span data-stu-id="09495-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="09495-107">Możesz użyć uwierzytelniania Windows, gdy serwer działa w sieci firmowej przy użyciu tożsamości domeny usługi Active Directory lub konta Windows do identyfikacji użytkowników.</span><span class="sxs-lookup"><span data-stu-id="09495-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="09495-108">Uwierzytelnianie Windows najlepiej nadaje się do środowisk intranetowych, w którym użytkownicy, aplikacje klienckie i serwery sieci web należą do tej samej domeny Windows.</span><span class="sxs-lookup"><span data-stu-id="09495-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="09495-109">Włączanie uwierzytelniania Windows w aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09495-109">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="09495-110">**Aplikacji sieci Web** szablonu dostępnego za pośrednictwem programu Visual Studio lub interfejsu wiersza polecenia platformy .NET Core może być skonfigurowane do obsługi uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="09495-110">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="09495-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09495-111">Visual Studio</span></span>](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a><span data-ttu-id="09495-112">Korzystanie z szablonu aplikacji uwierzytelniania Windows dla nowego projektu</span><span class="sxs-lookup"><span data-stu-id="09495-112">Use the Windows Authentication app template for a new project</span></span>

<span data-ttu-id="09495-113">W programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="09495-113">In Visual Studio:</span></span>

1. <span data-ttu-id="09495-114">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="09495-114">Create a new project.</span></span>
1. <span data-ttu-id="09495-115">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="09495-115">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="09495-116">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="09495-116">Select **Next**.</span></span>
1. <span data-ttu-id="09495-117">Podaj nazwę w **Nazwa projektu** pola.</span><span class="sxs-lookup"><span data-stu-id="09495-117">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="09495-118">Upewnij się, **lokalizacji** wpis jest poprawny lub podaj lokalizację dla projektu.</span><span class="sxs-lookup"><span data-stu-id="09495-118">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="09495-119">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="09495-119">Select **Create**.</span></span>
1. <span data-ttu-id="09495-120">Wybierz **zmiany** w obszarze **uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="09495-120">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="09495-121">W **Zmień uwierzytelnianie** wybierz **uwierzytelniania Windows**.</span><span class="sxs-lookup"><span data-stu-id="09495-121">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="09495-122">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="09495-122">Select **OK**.</span></span>
1. <span data-ttu-id="09495-123">Wybierz **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="09495-123">Select **Web Application**.</span></span>
1. <span data-ttu-id="09495-124">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="09495-124">Select **Create**.</span></span>

<span data-ttu-id="09495-125">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="09495-125">Run the app.</span></span> <span data-ttu-id="09495-126">Nazwa użytkownika pojawia się w interfejsie użytkownika aplikacji renderowany.</span><span class="sxs-lookup"><span data-stu-id="09495-126">The username appears in the rendered app's user interface.</span></span>

### <a name="manual-configuration-for-an-existing-project"></a><span data-ttu-id="09495-127">Ręcznej konfiguracji dla istniejącego projektu</span><span class="sxs-lookup"><span data-stu-id="09495-127">Manual configuration for an existing project</span></span>

<span data-ttu-id="09495-128">Właściwości projektu umożliwiają uwierzytelnianie Windows włączyć i wyłączyć uwierzytelnianie anonimowe:</span><span class="sxs-lookup"><span data-stu-id="09495-128">The project's properties allow you to enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="09495-129">Kliknij prawym przyciskiem myszy projekt w programie Visual Studio **Eksploratora rozwiązań** i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="09495-129">Right-click the project in Visual Studio's **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="09495-130">Wybierz **debugowania** kartę.</span><span class="sxs-lookup"><span data-stu-id="09495-130">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="09495-131">Usuń zaznaczenie pola wyboru dla **Włącz uwierzytelnianie anonimowe**.</span><span class="sxs-lookup"><span data-stu-id="09495-131">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="09495-132">Zaznacz pole wyboru dla **Włącz uwierzytelnianie Windows**.</span><span class="sxs-lookup"><span data-stu-id="09495-132">Select the check box for **Enable Windows Authentication**.</span></span>

<span data-ttu-id="09495-133">Alternatywnie, można skonfigurować właściwości w `iisSettings` węźle *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="09495-133">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="09495-134">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="09495-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="09495-135">Użyj **uwierzytelniania Windows** szablonu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09495-135">Use the **Windows Authentication** app template.</span></span>

<span data-ttu-id="09495-136">Wykonaj [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia `webapp` argumentu (aplikację sieci Web platformy ASP.NET Core) i `--auth Windows` przełącznika:</span><span class="sxs-lookup"><span data-stu-id="09495-136">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

---

<span data-ttu-id="09495-137">Podczas modyfikowania istniejącego projektu, upewnij się, że plik projektu zawiera odwołania do pakietu dla [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) **lub** [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="09495-137">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="09495-138">Włączanie uwierzytelniania Windows za pomocą usług IIS</span><span class="sxs-lookup"><span data-stu-id="09495-138">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="09495-139">Usługi IIS używają [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) hostująca aplikacje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09495-139">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="09495-140">Uwierzytelnianie Windows jest skonfigurowany dla usług IIS za pomocą *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="09495-140">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="09495-141">Następujące sekcje show jak:</span><span class="sxs-lookup"><span data-stu-id="09495-141">The following sections show how to:</span></span>

* <span data-ttu-id="09495-142">Zapewnia lokalny *web.config* pliku, który aktywuje uwierzytelniania Windows na serwerze, gdy aplikacja jest wdrożona.</span><span class="sxs-lookup"><span data-stu-id="09495-142">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="09495-143">Konfigurowanie za pomocą Menedżera usług IIS *web.config* pliku aplikacji platformy ASP.NET Core, która została już wdrożona na serwerze.</span><span class="sxs-lookup"><span data-stu-id="09495-143">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="09495-144">Konfiguracja programu IIS</span><span class="sxs-lookup"><span data-stu-id="09495-144">IIS configuration</span></span>

<span data-ttu-id="09495-145">Jeśli jeszcze tego nie zrobiono, Włącz usługi IIS do hostowania aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09495-145">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="09495-146">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="09495-146">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="09495-147">Włączyć usługi roli usług IIS na potrzeby uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="09495-147">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="09495-148">Aby uzyskać więcej informacji, zobacz [Włącz uwierzytelnianie Windows usługami roli usług IIS (zobacz krok 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="09495-148">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="09495-149">[Oprogramowania pośredniczącego integracji usługi IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) jest domyślnie skonfigurowana do automatycznego uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="09495-149">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="09495-150">Aby uzyskać więcej informacji, zobacz [hosta ASP.NET Core na Windows za pomocą programu IIS: Opcje usług IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="09495-150">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="09495-151">Modułu ASP.NET Core jest domyślnie skonfigurowana do przekazywania tokenu uwierzytelniania Windows do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09495-151">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="09495-152">Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji modułu ASP.NET Core: Atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="09495-152">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="09495-153">Utwórz nową witrynę IIS</span><span class="sxs-lookup"><span data-stu-id="09495-153">Create a new IIS site</span></span>

<span data-ttu-id="09495-154">Określ nazwę i folder, a następnie zezwala utworzyć nową pulę aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09495-154">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="enable-windows-authentication-for-the-app-in-iis"></a><span data-ttu-id="09495-155">Włącz uwierzytelnianie Windows dla aplikacji w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="09495-155">Enable Windows Authentication for the app in IIS</span></span>

<span data-ttu-id="09495-156">Użyj **albo** z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="09495-156">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="09495-157">[Programowanie — konfiguracja po stronie przed opublikowaniem aplikacji](#development-side-configuration-with-a-local-webconfig-file) (*zalecane*)</span><span class="sxs-lookup"><span data-stu-id="09495-157">[Development-side configuration before publishing the app](#development-side-configuration-with-a-local-webconfig-file) (*Recommended*)</span></span>
* [<span data-ttu-id="09495-158">Konfiguracja po stronie serwera po opublikowaniu aplikacji</span><span class="sxs-lookup"><span data-stu-id="09495-158">Server-side configuration after publishing the app</span></span>](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a><span data-ttu-id="09495-159">Programowanie — konfiguracja po stronie przy użyciu pliku lokalnego pliku web.config</span><span class="sxs-lookup"><span data-stu-id="09495-159">Development-side configuration with a local web.config file</span></span>

<span data-ttu-id="09495-160">Wykonaj poniższe kroki **przed** możesz [publikowanie i wdrażanie projektu](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="09495-160">Perform the following steps **before** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

<span data-ttu-id="09495-161">Dodaj następujący kod *web.config* plik do katalogu głównego projektu:</span><span class="sxs-lookup"><span data-stu-id="09495-161">Add the following *web.config* file to the project root:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

<span data-ttu-id="09495-162">Gdy projekt zostanie opublikowany przez zestaw SDK (bez `<IsTransformWebConfigDisabled>` właściwością `true` w pliku projektu), opublikowanego *web.config* plik zawiera `<location><system.webServer><security><authentication>` sekcji.</span><span class="sxs-lookup"><span data-stu-id="09495-162">When the project is published by the SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="09495-163">Aby uzyskać więcej informacji na temat `<IsTransformWebConfigDisabled>` właściwości, zobacz <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="09495-163">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

#### <a name="server-side-configuration-with-the-iis-manager"></a><span data-ttu-id="09495-164">Konfiguracja po stronie serwera za pomocą Menedżera usług IIS</span><span class="sxs-lookup"><span data-stu-id="09495-164">Server-side configuration with the IIS Manager</span></span>

<span data-ttu-id="09495-165">Wykonaj poniższe kroki **po** możesz [publikowanie i wdrażanie projektu](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="09495-165">Perform the following steps **after** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

1. <span data-ttu-id="09495-166">W Menedżerze usług IIS wybierz witrynę IIS, w obszarze **witryn** węźle **połączeń** pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="09495-166">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
1. <span data-ttu-id="09495-167">Kliknij dwukrotnie **uwierzytelniania** w **IIS** obszaru.</span><span class="sxs-lookup"><span data-stu-id="09495-167">Double-click **Authentication** in the **IIS** area.</span></span>
1. <span data-ttu-id="09495-168">Wybierz **uwierzytelnianie anonimowe**.</span><span class="sxs-lookup"><span data-stu-id="09495-168">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="09495-169">Wybierz **wyłączyć** w **akcje** pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="09495-169">Select **Disable** in the **Actions** sidebar.</span></span>
1. <span data-ttu-id="09495-170">Wybierz **uwierzytelniania Windows**.</span><span class="sxs-lookup"><span data-stu-id="09495-170">Select **Windows Authentication**.</span></span> <span data-ttu-id="09495-171">Wybierz **Włącz** w **akcje** pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="09495-171">Select **Enable** in the **Actions** sidebar.</span></span>

<span data-ttu-id="09495-172">Kiedy te akcje są wykonywane, Menedżera usług IIS modyfikuje aplikacji *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="09495-172">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="09495-173">A `<system.webServer><security><authentication>` węzeł zostanie dodany ze zaktualizowanymi ustawieniami dla `anonymousAuthentication` i `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="09495-173">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

<span data-ttu-id="09495-174">`<system.webServer>` Dodany do sekcji *web.config* pliku przez Menedżera usług IIS znajduje się poza jej `<location>` sekcji dodawane przez program .NET Core SDK po opublikowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09495-174">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="09495-175">Ponieważ sekcji zostanie dodany poza `<location>` węzła, ustawienia są dziedziczone przez żaden [aplikacji podrzędnych](xref:host-and-deploy/iis/index#sub-applications) do bieżącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09495-175">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="09495-176">Aby zapobiec dziedziczenia, Przenieś dodany `<security>` sekcji wewnątrz `<location><system.webServer>` sekcji podanym w zestawie SDK.</span><span class="sxs-lookup"><span data-stu-id="09495-176">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the SDK provided.</span></span>

<span data-ttu-id="09495-177">W przypadku Menedżera usług IIS można dodać konfiguracji usług IIS dotyczy tylko aplikacji *web.config* pliku na serwerze.</span><span class="sxs-lookup"><span data-stu-id="09495-177">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="09495-178">Kolejne wdrożenie aplikacji może zastąpić ustawienia na serwerze, jeśli kopię serwera *web.config* zastępuje projektu *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="09495-178">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="09495-179">Użyj **albo** z następujących metod do zarządzania ustawieniami:</span><span class="sxs-lookup"><span data-stu-id="09495-179">Use **either** of the following approaches to manage the settings:</span></span>

* <span data-ttu-id="09495-180">Użyj Menedżera usług IIS, aby zresetować ustawienia w *web.config* pliku po plik jest zastępowany we wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="09495-180">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
* <span data-ttu-id="09495-181">Dodaj *pliku web.config* aplikacji lokalnie przy użyciu ustawień.</span><span class="sxs-lookup"><span data-stu-id="09495-181">Add a *web.config file* to the app locally with the settings.</span></span> <span data-ttu-id="09495-182">Aby uzyskać więcej informacji, zobacz [rozwoju — konfiguracja po stronie](#development-side-configuration-with-a-local-webconfig-file) sekcji.</span><span class="sxs-lookup"><span data-stu-id="09495-182">For more information, see the [Development-side configuration](#development-side-configuration-with-a-local-webconfig-file) section.</span></span>

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a><span data-ttu-id="09495-183">Publikowanie i wdrażanie projektu do folderu witryny usług IIS</span><span class="sxs-lookup"><span data-stu-id="09495-183">Publish and deploy your project to the IIS site folder</span></span>

<span data-ttu-id="09495-184">Za pomocą programu Visual Studio lub interfejsu wiersza polecenia platformy .NET Core, publikowanie i wdrażanie aplikacji do folderu docelowego.</span><span class="sxs-lookup"><span data-stu-id="09495-184">Using Visual Studio or the .NET Core CLI, publish and deploy the app to the destination folder.</span></span>

<span data-ttu-id="09495-185">Aby uzyskać więcej informacji dotyczących obsługi za pomocą programu IIS publikowania i wdrażania, zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="09495-185">For more information on hosting with IIS, publishing, and deployment, see the following topics:</span></span>

* [<span data-ttu-id="09495-186">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="09495-186">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

<span data-ttu-id="09495-187">Uruchom aplikację, aby sprawdzić, czy działa uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="09495-187">Launch the app to verify Windows Authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="09495-188">Włącz uwierzytelnianie Windows w pliku HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="09495-188">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="09495-189">Chociaż Kestrel nie obsługuje uwierzytelniania Windows, możesz użyć [HTTP.sys](xref:fundamentals/servers/httpsys) do obsługi scenariuszy samodzielnie hostowanego na Windows.</span><span class="sxs-lookup"><span data-stu-id="09495-189">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="09495-190">Poniższy przykład umożliwia skonfigurowanie aplikacji hosta sieci web HTTP.sys za pomocą uwierzytelniania Windows:</span><span class="sxs-lookup"><span data-stu-id="09495-190">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="09495-191">Sterownik HTTP.sys delegatów, aby uwierzytelnianie trybu jądra za pomocą protokołu uwierzytelniania Kerberos.</span><span class="sxs-lookup"><span data-stu-id="09495-191">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="09495-192">Uwierzytelnianie w trybie użytkownika nie jest obsługiwana przy użyciu protokołu Kerberos i sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="09495-192">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="09495-193">Konto komputera należy używany do odszyfrowywania tokenu/biletu Kerberos uzyskany z usługi Active Directory i przesyłany dalej przez klienta do serwera w celu uwierzytelnienia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="09495-193">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="09495-194">Rejestrowanie głównej nazwy usługi (SPN) dla hosta, a nie użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09495-194">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="09495-195">Sterownik HTTP.sys nie jest obsługiwana na serwerze Nano Server w wersji 1709 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="09495-195">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="09495-196">Aby użyć uwierzytelniania Windows i sterownik HTTP.sys systemu Nano Server, użyj [Server Core (microsoft/windowsservercore) kontenera](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="09495-196">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="09495-197">Aby uzyskać więcej informacji na temat Server Core, zobacz [co to jest opcja instalacji Server Core w systemie Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="09495-197">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="work-with-windows-authentication"></a><span data-ttu-id="09495-198">Praca z uwierzytelnianiem Windows</span><span class="sxs-lookup"><span data-stu-id="09495-198">Work with Windows Authentication</span></span>

<span data-ttu-id="09495-199">Stan konfiguracji dostępu anonimowego określa sposób, w którym `[Authorize]` i `[AllowAnonymous]` atrybuty są używane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09495-199">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="09495-200">W poniższych sekcjach dwóch opisano sposób obsługi konfiguracji niedozwolonych i dozwolone stany dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="09495-200">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="09495-201">Nie zezwalaj na dostęp anonimowy</span><span class="sxs-lookup"><span data-stu-id="09495-201">Disallow anonymous access</span></span>

<span data-ttu-id="09495-202">Gdy jest włączone uwierzytelnianie Windows i dostęp anonimowy jest wyłączony, `[Authorize]` i `[AllowAnonymous]` atrybuty mają żadnego skutku.</span><span class="sxs-lookup"><span data-stu-id="09495-202">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="09495-203">Jeśli nie zezwala na dostęp anonimowy skonfigurowano witryny usług IIS (lub sterownik HTTP.sys), żądanie nigdy nie osiągnie Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09495-203">If the IIS site (or HTTP.sys) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="09495-204">Z tego powodu `[AllowAnonymous]` atrybut nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="09495-204">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="09495-205">Zezwalaj na dostęp anonimowy</span><span class="sxs-lookup"><span data-stu-id="09495-205">Allow anonymous access</span></span>

<span data-ttu-id="09495-206">Po włączeniu uwierzytelniania Windows i dostęp anonimowy używać `[Authorize]` i `[AllowAnonymous]` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="09495-206">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="09495-207">`[Authorize]` Atrybut umożliwia zabezpieczenie rodzajów aplikacji, które naprawdę wymagają uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="09495-207">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="09495-208">`[AllowAnonymous]` Atrybutu zastąpienia `[Authorize]` atrybutu do użycia w aplikacjach, które zezwala na dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="09495-208">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="09495-209">Zobacz [autoryzacja prosta](xref:security/authorization/simple) szczegóły użycia atrybutu.</span><span class="sxs-lookup"><span data-stu-id="09495-209">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="09495-210">W programie ASP.NET Core 2.x, `[Authorize]` atrybut wymaga dodatkowej konfiguracji w *Startup.cs* zażąda anonimowe żądania uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="09495-210">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="09495-211">Zalecana konfiguracja zależy od nieco używany serwer sieci web.</span><span class="sxs-lookup"><span data-stu-id="09495-211">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="09495-212">Domyślnie użytkownicy, którzy nie mają autoryzacji do dostępu do strony są prezentowane z pustą odpowiedź HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="09495-212">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="09495-213">[Oprogramowania pośredniczącego StatusCodePages](xref:fundamentals/error-handling#usestatuscodepages) można skonfigurować, aby zapewnić użytkownikom lepsze środowisko "Odmowa dostępu".</span><span class="sxs-lookup"><span data-stu-id="09495-213">The [StatusCodePages middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="09495-214">IIS</span><span class="sxs-lookup"><span data-stu-id="09495-214">IIS</span></span>

<span data-ttu-id="09495-215">Jeśli korzystanie z usług IIS, należy dodać następujące polecenie, aby `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="09495-215">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="09495-216">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="09495-216">HTTP.sys</span></span>

<span data-ttu-id="09495-217">Jeśli używasz HTTP.sys, Dodaj następujące polecenie, aby `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="09495-217">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="09495-218">Personifikacja</span><span class="sxs-lookup"><span data-stu-id="09495-218">Impersonation</span></span>

<span data-ttu-id="09495-219">Platforma ASP.NET Core nie implementuje personifikacji.</span><span class="sxs-lookup"><span data-stu-id="09495-219">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="09495-220">Aplikacje są uruchamiane przy użyciu tożsamości przez aplikację dla wszystkich żądań, przy użyciu tożsamości puli lub procesu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09495-220">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="09495-221">Jeśli musisz jawnie wykonaj akcję w imieniu użytkownika, należy użyć [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) w [oprogramowania pośredniczącego terminalu wbudowane](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="09495-221">If you need to explicitly perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="09495-222">Uruchomić jedną akcję w tym kontekście, a następnie zamknij kontekstu.</span><span class="sxs-lookup"><span data-stu-id="09495-222">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="09495-223">`RunImpersonated` nie obsługuje operacji asynchronicznych i nie powinny być używane w przypadku złożonych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="09495-223">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="09495-224">Na przykład zawijania całego żądania lub łańcuchów oprogramowanie pośredniczące nie jest obsługiwany lub zalecane.</span><span class="sxs-lookup"><span data-stu-id="09495-224">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

### <a name="claims-transformations"></a><span data-ttu-id="09495-225">Przekształcenia oświadczeń</span><span class="sxs-lookup"><span data-stu-id="09495-225">Claims transformations</span></span>

<span data-ttu-id="09495-226">W przypadku hostowania w trybie usług IIS w trakcie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wewnętrznie wywoływana w celu zainicjowania przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="09495-226">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="09495-227">W związku z tym <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementacji używanego do przekształcania oświadczeń, po każdym uwierzytelniania nie jest aktywowana domyślnie.</span><span class="sxs-lookup"><span data-stu-id="09495-227">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="09495-228">Aby uzyskać więcej informacji i przykładowy kod, który aktywuje przekształcenia oświadczeń w przypadku hostowania w procesie, zobacz <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="09495-228">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>
