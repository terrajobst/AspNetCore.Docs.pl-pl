---
title: Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core
author: scottaddie
description: Dowiedz się, jak skonfigurować uwierzytelnianie Windows w programie ASP.NET Core, za pomocą usług IIS Express, usługi IIS i sterownik HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 01/15/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: c98bdedcf943a9057c96a8e5d62615e400074899
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341657"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="14881-103">Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="14881-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="14881-104">Przez [Scott Addie](https://twitter.com/Scott_Addie) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="14881-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="14881-105">[Uwierzytelnianie Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) można skonfigurować dla aplikacji platformy ASP.NET Core z [IIS](xref:host-and-deploy/iis/index) lub [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="14881-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="14881-106">Uwierzytelnianie Windows opiera się uwierzytelniać użytkowników aplikacji platformy ASP.NET Core w systemie operacyjnym.</span><span class="sxs-lookup"><span data-stu-id="14881-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="14881-107">Możesz użyć uwierzytelniania Windows, gdy serwer działa w sieci firmowej przy użyciu tożsamości domeny usługi Active Directory lub konta Windows do identyfikacji użytkowników.</span><span class="sxs-lookup"><span data-stu-id="14881-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="14881-108">Uwierzytelnianie Windows najlepiej nadaje się do środowisk intranetowych, w którym użytkownicy, aplikacje klienckie i serwery sieci web należą do tej samej domeny Windows.</span><span class="sxs-lookup"><span data-stu-id="14881-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="14881-109">Włączanie uwierzytelniania Windows w aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="14881-109">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="14881-110">**Aplikacji sieci Web** szablonu dostępnego za pośrednictwem programu Visual Studio lub interfejsu wiersza polecenia platformy .NET Core może być skonfigurowane do obsługi uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="14881-110">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="14881-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="14881-111">Visual Studio</span></span>](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a><span data-ttu-id="14881-112">Korzystanie z szablonu aplikacji uwierzytelniania Windows dla nowego projektu</span><span class="sxs-lookup"><span data-stu-id="14881-112">Use the Windows Authentication app template for a new project</span></span>

<span data-ttu-id="14881-113">W programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="14881-113">In Visual Studio:</span></span>

1. <span data-ttu-id="14881-114">Utwórz nową **aplikacji sieci Web programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="14881-114">Create a new **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="14881-115">Wybierz **aplikacji sieci Web** z listy szablonów.</span><span class="sxs-lookup"><span data-stu-id="14881-115">Select **Web Application** from the list of templates.</span></span>
1. <span data-ttu-id="14881-116">Wybierz **Zmień uwierzytelnianie** i wybrać **uwierzytelniania Windows**.</span><span class="sxs-lookup"><span data-stu-id="14881-116">Select the **Change Authentication** button and select **Windows Authentication**.</span></span>

<span data-ttu-id="14881-117">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="14881-117">Run the app.</span></span> <span data-ttu-id="14881-118">Nazwa użytkownika pojawia się w interfejsie użytkownika aplikacji renderowany.</span><span class="sxs-lookup"><span data-stu-id="14881-118">The username appears in the rendered app's user interface.</span></span>

### <a name="manual-configuration-for-an-existing-project"></a><span data-ttu-id="14881-119">Ręcznej konfiguracji dla istniejącego projektu</span><span class="sxs-lookup"><span data-stu-id="14881-119">Manual configuration for an existing project</span></span>

<span data-ttu-id="14881-120">Właściwości projektu umożliwiają uwierzytelnianie Windows włączyć i wyłączyć uwierzytelnianie anonimowe:</span><span class="sxs-lookup"><span data-stu-id="14881-120">The project's properties allow you to enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="14881-121">Kliknij prawym przyciskiem myszy projekt w programie Visual Studio **Eksploratora rozwiązań** i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="14881-121">Right-click the project in Visual Studio's **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="14881-122">Wybierz **debugowania** kartę.</span><span class="sxs-lookup"><span data-stu-id="14881-122">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="14881-123">Usuń zaznaczenie pola wyboru dla **Włącz uwierzytelnianie anonimowe**.</span><span class="sxs-lookup"><span data-stu-id="14881-123">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="14881-124">Zaznacz pole wyboru dla **Włącz uwierzytelnianie Windows**.</span><span class="sxs-lookup"><span data-stu-id="14881-124">Select the check box for **Enable Windows Authentication**.</span></span>

<span data-ttu-id="14881-125">Alternatywnie, można skonfigurować właściwości w `iisSettings` węźle *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="14881-125">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="14881-126">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="14881-126">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="14881-127">Użyj **uwierzytelniania Windows** szablonu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14881-127">Use the **Windows Authentication** app template.</span></span>

<span data-ttu-id="14881-128">Wykonaj [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia `webapp` argumentu (aplikację sieci Web platformy ASP.NET Core) i `--auth Windows` przełącznika:</span><span class="sxs-lookup"><span data-stu-id="14881-128">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

---

<span data-ttu-id="14881-129">Podczas modyfikowania istniejącego projektu, upewnij się, że plik projektu zawiera odwołania do pakietu dla [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) **lub** [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="14881-129">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="14881-130">Włączanie uwierzytelniania Windows za pomocą usług IIS</span><span class="sxs-lookup"><span data-stu-id="14881-130">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="14881-131">Usługi IIS używają [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) hostująca aplikacje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="14881-131">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="14881-132">Uwierzytelnianie Windows jest skonfigurowany dla usług IIS za pomocą *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="14881-132">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="14881-133">Następujące sekcje show jak:</span><span class="sxs-lookup"><span data-stu-id="14881-133">The following sections show how to:</span></span>

* <span data-ttu-id="14881-134">Zapewnia lokalny *web.config* pliku, który aktywuje uwierzytelniania Windows na serwerze, gdy aplikacja jest wdrożona.</span><span class="sxs-lookup"><span data-stu-id="14881-134">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="14881-135">Konfigurowanie za pomocą Menedżera usług IIS *web.config* pliku aplikacji platformy ASP.NET Core, która została już wdrożona na serwerze.</span><span class="sxs-lookup"><span data-stu-id="14881-135">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="14881-136">Konfiguracja programu IIS</span><span class="sxs-lookup"><span data-stu-id="14881-136">IIS configuration</span></span>

<span data-ttu-id="14881-137">Jeśli jeszcze tego nie zrobiono, Włącz usługi IIS do hostowania aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="14881-137">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="14881-138">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="14881-138">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="14881-139">Włączyć usługi roli usług IIS na potrzeby uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="14881-139">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="14881-140">Aby uzyskać więcej informacji, zobacz [Włącz uwierzytelnianie Windows usługami roli usług IIS (zobacz krok 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="14881-140">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="14881-141">Oprogramowania pośredniczącego integracji usługi IIS jest domyślnie skonfigurowana do automatycznego uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="14881-141">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="14881-142">Aby uzyskać więcej informacji, zobacz [hosta ASP.NET Core na Windows za pomocą programu IIS: Opcje usług IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="14881-142">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="14881-143">Modułu ASP.NET Core jest domyślnie skonfigurowana do przekazywania tokenu uwierzytelniania Windows do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14881-143">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="14881-144">Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji modułu ASP.NET Core: Atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="14881-144">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="14881-145">Utwórz nową witrynę IIS</span><span class="sxs-lookup"><span data-stu-id="14881-145">Create a new IIS site</span></span>

<span data-ttu-id="14881-146">Określ nazwę i folder, a następnie zezwala utworzyć nową pulę aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14881-146">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="enable-windows-authentication-for-the-app-in-iis"></a><span data-ttu-id="14881-147">Włącz uwierzytelnianie Windows dla aplikacji w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="14881-147">Enable Windows Authentication for the app in IIS</span></span>

<span data-ttu-id="14881-148">Użyj **albo** z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="14881-148">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="14881-149">[Programowanie — konfiguracja po stronie przed opublikowaniem aplikacji](#development-side-configuration-with-a-local-webconfig-file) (*zalecane*)</span><span class="sxs-lookup"><span data-stu-id="14881-149">[Development-side configuration before publishing the app](#development-side-configuration-with-a-local-webconfig-file) (*Recommended*)</span></span>
* [<span data-ttu-id="14881-150">Konfiguracja po stronie serwera po opublikowaniu aplikacji</span><span class="sxs-lookup"><span data-stu-id="14881-150">Server-side configuration after publishing the app</span></span>](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a><span data-ttu-id="14881-151">Programowanie — konfiguracja po stronie przy użyciu pliku lokalnego pliku web.config</span><span class="sxs-lookup"><span data-stu-id="14881-151">Development-side configuration with a local web.config file</span></span>

<span data-ttu-id="14881-152">Wykonaj poniższe kroki **przed** możesz [publikowanie i wdrażanie projektu](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="14881-152">Perform the following steps **before** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

<span data-ttu-id="14881-153">Dodaj następujący kod *web.config* plik do katalogu głównego projektu:</span><span class="sxs-lookup"><span data-stu-id="14881-153">Add the following *web.config* file to the project root:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

<span data-ttu-id="14881-154">Gdy projekt zostanie opublikowany przez zestaw SDK (bez `<IsTransformWebConfigDisabled>` właściwością `true` w pliku projektu), opublikowanego *web.config* plik zawiera `<location><system.webServer><security><authentication>` sekcji.</span><span class="sxs-lookup"><span data-stu-id="14881-154">When the project is published by the SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="14881-155">Aby uzyskać więcej informacji na temat `<IsTransformWebConfigDisabled>` właściwości, zobacz <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="14881-155">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

#### <a name="server-side-configuration-with-the-iis-manager"></a><span data-ttu-id="14881-156">Konfiguracja po stronie serwera za pomocą Menedżera usług IIS</span><span class="sxs-lookup"><span data-stu-id="14881-156">Server-side configuration with the IIS Manager</span></span>

<span data-ttu-id="14881-157">Wykonaj poniższe kroki **po** możesz [publikowanie i wdrażanie projektu](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="14881-157">Perform the following steps **after** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

1. <span data-ttu-id="14881-158">W Menedżerze usług IIS wybierz witrynę IIS, w obszarze **witryn** węźle **połączeń** pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="14881-158">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
1. <span data-ttu-id="14881-159">Kliknij dwukrotnie **uwierzytelniania** w **IIS** obszaru.</span><span class="sxs-lookup"><span data-stu-id="14881-159">Double-click **Authentication** in the **IIS** area.</span></span>
1. <span data-ttu-id="14881-160">Wybierz **uwierzytelnianie anonimowe**.</span><span class="sxs-lookup"><span data-stu-id="14881-160">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="14881-161">Wybierz **wyłączyć** w **akcje** pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="14881-161">Select **Disable** in the **Actions** sidebar.</span></span>
1. <span data-ttu-id="14881-162">Wybierz **uwierzytelniania Windows**.</span><span class="sxs-lookup"><span data-stu-id="14881-162">Select **Windows Authentication**.</span></span> <span data-ttu-id="14881-163">Wybierz **Włącz** w **akcje** pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="14881-163">Select **Enable** in the **Actions** sidebar.</span></span>

<span data-ttu-id="14881-164">Kiedy te akcje są wykonywane, Menedżera usług IIS modyfikuje aplikacji *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="14881-164">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="14881-165">A `<system.webServer><security><authentication>` węzeł zostanie dodany ze zaktualizowanymi ustawieniami dla `anonymousAuthentication` i `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="14881-165">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

<span data-ttu-id="14881-166">`<system.webServer>` Dodany do sekcji *web.config* pliku przez Menedżera usług IIS znajduje się poza jej `<location>` sekcji dodawane przez program .NET Core SDK po opublikowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14881-166">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="14881-167">Ponieważ sekcji zostanie dodany poza `<location>` węzła, ustawienia są dziedziczone przez żaden [aplikacji podrzędnych](xref:host-and-deploy/iis/index#sub-applications) do bieżącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14881-167">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="14881-168">Aby zapobiec dziedziczenia, Przenieś dodany `<security>` sekcji wewnątrz `<location><system.webServer>` sekcji podanym w zestawie SDK.</span><span class="sxs-lookup"><span data-stu-id="14881-168">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the SDK provided.</span></span>

<span data-ttu-id="14881-169">W przypadku Menedżera usług IIS można dodać konfiguracji usług IIS dotyczy tylko aplikacji *web.config* pliku na serwerze.</span><span class="sxs-lookup"><span data-stu-id="14881-169">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="14881-170">Kolejne wdrożenie aplikacji może zastąpić ustawienia na serwerze, jeśli kopię serwera *web.config* zastępuje projektu *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="14881-170">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="14881-171">Użyj **albo** z następujących metod do zarządzania ustawieniami:</span><span class="sxs-lookup"><span data-stu-id="14881-171">Use **either** of the following approaches to manage the settings:</span></span>

* <span data-ttu-id="14881-172">Użyj Menedżera usług IIS, aby zresetować ustawienia w *web.config* pliku po plik jest zastępowany we wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="14881-172">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
* <span data-ttu-id="14881-173">Dodaj *pliku web.config* aplikacji lokalnie przy użyciu ustawień.</span><span class="sxs-lookup"><span data-stu-id="14881-173">Add a *web.config file* to the app locally with the settings.</span></span> <span data-ttu-id="14881-174">Aby uzyskać więcej informacji, zobacz [rozwoju — konfiguracja po stronie](#development-side-configuration-with-a-local-webconfig-file) sekcji.</span><span class="sxs-lookup"><span data-stu-id="14881-174">For more information, see the [Development-side configuration](#development-side-configuration-with-a-local-webconfig-file) section.</span></span>

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a><span data-ttu-id="14881-175">Publikowanie i wdrażanie projektu do folderu witryny usług IIS</span><span class="sxs-lookup"><span data-stu-id="14881-175">Publish and deploy your project to the IIS site folder</span></span>

<span data-ttu-id="14881-176">Za pomocą programu Visual Studio lub interfejsu wiersza polecenia platformy .NET Core, publikowanie i wdrażanie aplikacji do folderu docelowego.</span><span class="sxs-lookup"><span data-stu-id="14881-176">Using Visual Studio or the .NET Core CLI, publish and deploy the app to the destination folder.</span></span>

<span data-ttu-id="14881-177">Aby uzyskać więcej informacji dotyczących obsługi za pomocą programu IIS publikowania i wdrażania, zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="14881-177">For more information on hosting with IIS, publishing, and deployment, see the following topics:</span></span>

* [<span data-ttu-id="14881-178">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="14881-178">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

<span data-ttu-id="14881-179">Uruchom aplikację, aby sprawdzić, czy działa uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="14881-179">Launch the app to verify Windows Authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="14881-180">Włącz uwierzytelnianie Windows w pliku HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="14881-180">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="14881-181">Chociaż Kestrel nie obsługuje uwierzytelniania Windows, możesz użyć [HTTP.sys](xref:fundamentals/servers/httpsys) do obsługi scenariuszy samodzielnie hostowanego na Windows.</span><span class="sxs-lookup"><span data-stu-id="14881-181">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="14881-182">Poniższy przykład umożliwia skonfigurowanie aplikacji hosta sieci web HTTP.sys za pomocą uwierzytelniania Windows:</span><span class="sxs-lookup"><span data-stu-id="14881-182">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="14881-183">Sterownik HTTP.sys delegatów, aby uwierzytelnianie trybu jądra za pomocą protokołu uwierzytelniania Kerberos.</span><span class="sxs-lookup"><span data-stu-id="14881-183">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="14881-184">Uwierzytelnianie w trybie użytkownika nie jest obsługiwana przy użyciu protokołu Kerberos i sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="14881-184">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="14881-185">Konto komputera należy używany do odszyfrowywania tokenu/biletu Kerberos uzyskany z usługi Active Directory i przesyłany dalej przez klienta do serwera w celu uwierzytelnienia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="14881-185">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="14881-186">Rejestrowanie głównej nazwy usługi (SPN) dla hosta, a nie użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14881-186">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="14881-187">Sterownik HTTP.sys nie jest obsługiwana na serwerze Nano Server w wersji 1709 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="14881-187">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="14881-188">Aby użyć uwierzytelniania Windows i sterownik HTTP.sys systemu Nano Server, użyj [Server Core (microsoft/windowsservercore) kontenera](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="14881-188">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="14881-189">Aby uzyskać więcej informacji na temat Server Core, zobacz [co to jest opcja instalacji Server Core w systemie Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="14881-189">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="work-with-windows-authentication"></a><span data-ttu-id="14881-190">Praca z uwierzytelnianiem Windows</span><span class="sxs-lookup"><span data-stu-id="14881-190">Work with Windows Authentication</span></span>

<span data-ttu-id="14881-191">Stan konfiguracji dostępu anonimowego określa sposób, w którym `[Authorize]` i `[AllowAnonymous]` atrybuty są używane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14881-191">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="14881-192">W poniższych sekcjach dwóch opisano sposób obsługi konfiguracji niedozwolonych i dozwolone stany dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="14881-192">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="14881-193">Nie zezwalaj na dostęp anonimowy</span><span class="sxs-lookup"><span data-stu-id="14881-193">Disallow anonymous access</span></span>

<span data-ttu-id="14881-194">Gdy jest włączone uwierzytelnianie Windows i dostęp anonimowy jest wyłączony, `[Authorize]` i `[AllowAnonymous]` atrybuty mają żadnego skutku.</span><span class="sxs-lookup"><span data-stu-id="14881-194">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="14881-195">Jeśli nie zezwala na dostęp anonimowy skonfigurowano witryny usług IIS (lub sterownik HTTP.sys), żądanie nigdy nie osiągnie Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14881-195">If the IIS site (or HTTP.sys) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="14881-196">Z tego powodu `[AllowAnonymous]` atrybut nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="14881-196">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="14881-197">Zezwalaj na dostęp anonimowy</span><span class="sxs-lookup"><span data-stu-id="14881-197">Allow anonymous access</span></span>

<span data-ttu-id="14881-198">Po włączeniu uwierzytelniania Windows i dostęp anonimowy używać `[Authorize]` i `[AllowAnonymous]` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="14881-198">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="14881-199">`[Authorize]` Atrybut umożliwia zabezpieczenie rodzajów aplikacji, które naprawdę wymagają uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="14881-199">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="14881-200">`[AllowAnonymous]` Atrybutu zastąpienia `[Authorize]` atrybutu do użycia w aplikacjach, które zezwala na dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="14881-200">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="14881-201">Zobacz [autoryzacja prosta](xref:security/authorization/simple) szczegóły użycia atrybutu.</span><span class="sxs-lookup"><span data-stu-id="14881-201">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="14881-202">W programie ASP.NET Core 2.x, `[Authorize]` atrybut wymaga dodatkowej konfiguracji w *Startup.cs* zażąda anonimowe żądania uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="14881-202">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="14881-203">Zalecana konfiguracja zależy od nieco używany serwer sieci web.</span><span class="sxs-lookup"><span data-stu-id="14881-203">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="14881-204">Domyślnie użytkownicy, którzy nie mają autoryzacji do dostępu do strony są prezentowane z pustą odpowiedź HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="14881-204">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="14881-205">[Oprogramowania pośredniczącego StatusCodePages](xref:fundamentals/error-handling#configure-status-code-pages) można skonfigurować, aby zapewnić użytkownikom lepsze środowisko "Odmowa dostępu".</span><span class="sxs-lookup"><span data-stu-id="14881-205">The [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="14881-206">IIS</span><span class="sxs-lookup"><span data-stu-id="14881-206">IIS</span></span>

<span data-ttu-id="14881-207">Jeśli korzystanie z usług IIS, należy dodać następujące polecenie, aby `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="14881-207">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="14881-208">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="14881-208">HTTP.sys</span></span>

<span data-ttu-id="14881-209">Jeśli używasz HTTP.sys, Dodaj następujące polecenie, aby `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="14881-209">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="14881-210">Personifikacja</span><span class="sxs-lookup"><span data-stu-id="14881-210">Impersonation</span></span>

<span data-ttu-id="14881-211">Platforma ASP.NET Core nie implementuje personifikacji.</span><span class="sxs-lookup"><span data-stu-id="14881-211">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="14881-212">Aplikacje są uruchamiane przy użyciu tożsamości przez aplikację dla wszystkich żądań, przy użyciu tożsamości puli lub procesu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14881-212">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="14881-213">Jeśli musisz jawnie wykonaj akcję w imieniu użytkownika, należy użyć [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) w [oprogramowania pośredniczącego terminalu wbudowane](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="14881-213">If you need to explicitly perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="14881-214">Uruchomić jedną akcję w tym kontekście, a następnie zamknij kontekstu.</span><span class="sxs-lookup"><span data-stu-id="14881-214">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="14881-215">`RunImpersonated` nie obsługuje operacji asynchronicznych i nie powinny być używane w przypadku złożonych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="14881-215">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="14881-216">Na przykład zawijania całego żądania lub łańcuchów oprogramowanie pośredniczące nie jest obsługiwany lub zalecane.</span><span class="sxs-lookup"><span data-stu-id="14881-216">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>
