---
title: Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core
author: scottaddie
description: Dowiedz się, jak skonfigurować uwierzytelnianie Windows w programie ASP.NET Core, za pomocą usług IIS Express, usługi IIS, sterownik HTTP.sys i WebListener.
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 87fcab75555c1dae0b2815c30d79fd4615df9660
ms.sourcegitcommit: 85f2939af7a167b9694e1d2093277ffc9a741b23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50968296"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="ee997-103">Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee997-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="ee997-104">Przez [Steve Smith](https://ardalis.com) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="ee997-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="ee997-105">Dla aplikacji platformy ASP.NET Core, obsługiwane w przypadku usług IIS można skonfigurować uwierzytelniania Windows [HTTP.sys](xref:fundamentals/servers/httpsys), lub [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="ee997-105">Windows Authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="windows-authentication"></a><span data-ttu-id="ee997-106">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="ee997-106">Windows Authentication</span></span>

<span data-ttu-id="ee997-107">Uwierzytelnianie Windows opiera się uwierzytelniać użytkowników aplikacji platformy ASP.NET Core w systemie operacyjnym.</span><span class="sxs-lookup"><span data-stu-id="ee997-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="ee997-108">Możesz użyć uwierzytelniania Windows, gdy serwer działa w sieci firmowej przy użyciu tożsamości domeny usługi Active Directory lub innym kontom Windows do identyfikacji użytkowników.</span><span class="sxs-lookup"><span data-stu-id="ee997-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="ee997-109">Uwierzytelnianie Windows najlepiej nadaje się do środowisk intranetowych, w których użytkownicy, aplikacje klienckie i serwery sieci web należą do tej samej domeny Windows.</span><span class="sxs-lookup"><span data-stu-id="ee997-109">Windows Authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="ee997-110">[Dowiedz się więcej o uwierzytelnianiu Windows i instalując je dla usług IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="ee997-110">[Learn more about Windows Authentication and installing it for IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="ee997-111">Włączanie uwierzytelniania Windows w aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee997-111">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="ee997-112">Szablon aplikacji sieci Web w usłudze Visual Studio można skonfigurować tak, aby zapewnić obsługę uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="ee997-112">The Visual Studio Web Application template can be configured to support Windows Authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="ee997-113">Korzystanie z szablonu aplikacji uwierzytelnianie Windows</span><span class="sxs-lookup"><span data-stu-id="ee997-113">Use the Windows Authentication app template</span></span>

<span data-ttu-id="ee997-114">W programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ee997-114">In Visual Studio:</span></span>

1. <span data-ttu-id="ee997-115">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ee997-115">Create a new ASP.NET Core Web Application.</span></span>
1. <span data-ttu-id="ee997-116">Wybierz aplikację sieci Web z listy szablonów.</span><span class="sxs-lookup"><span data-stu-id="ee997-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="ee997-117">Wybierz **Zmień uwierzytelnianie** i wybrać **uwierzytelniania Windows**.</span><span class="sxs-lookup"><span data-stu-id="ee997-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span>

<span data-ttu-id="ee997-118">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="ee997-118">Run the app.</span></span> <span data-ttu-id="ee997-119">Nazwa użytkownika jest wyświetlana w prawym górnym rogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee997-119">The username appears in the top right of the app.</span></span>

![Zrzut ekranu przeglądarki uwierzytelnianie Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="ee997-121">Prace deweloperskie za pomocą usług IIS Express szablon zawiera wszystkie konfiguracje niezbędne do korzystania z uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="ee997-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows Authentication.</span></span> <span data-ttu-id="ee997-122">W poniższej sekcji pokazano, jak ręcznie skonfigurować aplikację ASP.NET Core dla uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="ee997-122">The following section shows how to manually configure an ASP.NET Core app for Windows Authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="ee997-123">Ustawienia programu Visual Studio for Windows i uwierzytelnianie anonimowe</span><span class="sxs-lookup"><span data-stu-id="ee997-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="ee997-124">Projekt programu Visual Studio **właściwości** strony **debugowania** karta zawiera pola wyboru dla uwierzytelniania Windows i uwierzytelnianie anonimowe.</span><span class="sxs-lookup"><span data-stu-id="ee997-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows Authentication and anonymous authentication.</span></span>

![Zrzut ekranu przeglądarki uwierzytelnianie Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="ee997-126">Alternatywnie, można skonfigurować te dwie właściwości w *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="ee997-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="ee997-127">Włączanie uwierzytelniania Windows za pomocą usług IIS</span><span class="sxs-lookup"><span data-stu-id="ee997-127">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="ee997-128">Usługi IIS używają [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) hostująca aplikacje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ee997-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="ee997-129">Uwierzytelnianie Windows jest skonfigurowany w usługach IIS, a nie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee997-129">Windows Authentication is configured in IIS, not the app.</span></span> <span data-ttu-id="ee997-130">Poniższe sekcje pokazują, jak skonfigurować aplikację ASP.NET Core, aby korzystać z uwierzytelniania Windows za pomocą Menedżera usług IIS.</span><span class="sxs-lookup"><span data-stu-id="ee997-130">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows Authentication.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="ee997-131">Konfiguracja programu IIS</span><span class="sxs-lookup"><span data-stu-id="ee997-131">IIS configuration</span></span>

<span data-ttu-id="ee997-132">Włączyć usługi roli usług IIS na potrzeby uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="ee997-132">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="ee997-133">Aby uzyskać więcej informacji, zobacz [Włącz uwierzytelnianie Windows usługami roli usług IIS (zobacz krok 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="ee997-133">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="ee997-134">Oprogramowania pośredniczącego integracji usługi IIS jest domyślnie skonfigurowana do automatycznego uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="ee997-134">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="ee997-135">Aby uzyskać więcej informacji, zobacz [hosta ASP.NET Core na Windows za pomocą programu IIS: IIS opcje (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="ee997-135">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="ee997-136">Modułu ASP.NET Core jest domyślnie skonfigurowana do przekazywania tokenu uwierzytelniania Windows do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee997-136">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="ee997-137">Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji modułu ASP.NET Core: atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="ee997-137">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="ee997-138">Utwórz nową witrynę IIS</span><span class="sxs-lookup"><span data-stu-id="ee997-138">Create a new IIS site</span></span>

<span data-ttu-id="ee997-139">Określ nazwę i folder, a następnie zezwala utworzyć nową pulę aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee997-139">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="ee997-140">Dostosowywanie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="ee997-140">Customize authentication</span></span>

<span data-ttu-id="ee997-141">Otwórz funkcje uwierzytelniania dla tej witryny.</span><span class="sxs-lookup"><span data-stu-id="ee997-141">Open the Authentication features for the site.</span></span>

![Menu uwierzytelniania usług IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="ee997-143">Wyłączyć uwierzytelnianie anonimowe, a następnie włączyć uwierzytelnianie Windows.</span><span class="sxs-lookup"><span data-stu-id="ee997-143">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Ustawienia uwierzytelniania usług IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="ee997-145">Publikowanie projektu do folderu witryny usług IIS</span><span class="sxs-lookup"><span data-stu-id="ee997-145">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="ee997-146">Za pomocą programu Visual Studio lub interfejsu wiersza polecenia platformy .NET Core, Opublikuj aplikację w folderze docelowym.</span><span class="sxs-lookup"><span data-stu-id="ee997-146">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Okno dialogowe publikowanie w programie Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="ee997-148">Dowiedz się więcej o [publikowania w usługach IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="ee997-148">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="ee997-149">Uruchom aplikację, aby sprawdzić, czy działa uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="ee997-149">Launch the app to verify Windows Authentication is working.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="ee997-150">Włącz uwierzytelnianie Windows w pliku HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ee997-150">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="ee997-151">Chociaż Kestrel nie obsługuje uwierzytelniania Windows, możesz użyć [HTTP.sys](xref:fundamentals/servers/httpsys) do obsługi scenariuszy samodzielnie hostowanego na Windows.</span><span class="sxs-lookup"><span data-stu-id="ee997-151">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="ee997-152">Poniższy przykład umożliwia skonfigurowanie aplikacji hosta sieci web HTTP.sys za pomocą uwierzytelniania Windows:</span><span class="sxs-lookup"><span data-stu-id="ee997-152">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="ee997-153">Sterownik HTTP.sys delegatów, aby uwierzytelnianie trybu jądra za pomocą protokołu uwierzytelniania Kerberos.</span><span class="sxs-lookup"><span data-stu-id="ee997-153">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="ee997-154">Uwierzytelnianie w trybie użytkownika nie jest obsługiwana przy użyciu protokołu Kerberos i sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="ee997-154">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="ee997-155">Konto komputera należy używany do odszyfrowywania tokenu/biletu Kerberos uzyskany z usługi Active Directory i przesyłany dalej przez klienta do serwera w celu uwierzytelnienia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ee997-155">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="ee997-156">Rejestrowanie głównej nazwy usługi (SPN) dla hosta, a nie użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee997-156">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="ee997-157">Sterownik HTTP.sys nie jest obsługiwana na serwerze Nano Server w wersji 1709 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="ee997-157">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="ee997-158">Aby użyć uwierzytelniania Windows i sterownik HTTP.sys systemu Nano Server, użyj [Server Core (microsoft/windowsservercore) kontenera](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="ee997-158">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="ee997-159">Aby uzyskać więcej informacji na temat Server Core, zobacz [co to jest opcja instalacji Server Core w systemie Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="ee997-159">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

## <a name="enable-windows-authentication-with-weblistener"></a><span data-ttu-id="ee997-160">Włączanie uwierzytelniania Windows za pomocą WebListener</span><span class="sxs-lookup"><span data-stu-id="ee997-160">Enable Windows Authentication with WebListener</span></span>

<span data-ttu-id="ee997-161">Chociaż Kestrel nie obsługuje uwierzytelniania Windows, możesz użyć [WebListener](xref:fundamentals/servers/weblistener) do obsługi scenariuszy samodzielnie hostowanego na Windows.</span><span class="sxs-lookup"><span data-stu-id="ee997-161">Although Kestrel doesn't support Windows Authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="ee997-162">Poniższy przykład umożliwia skonfigurowanie aplikacji sieci web hosta WebListener za pomocą uwierzytelniania Windows:</span><span class="sxs-lookup"><span data-stu-id="ee997-162">The following example configures the app's web host to use WebListener with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

> [!NOTE]
> <span data-ttu-id="ee997-163">WebListener delegatów, aby uwierzytelnianie trybu jądra za pomocą protokołu uwierzytelniania Kerberos.</span><span class="sxs-lookup"><span data-stu-id="ee997-163">WebListener delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="ee997-164">Uwierzytelnianie w trybie użytkownika nie jest obsługiwana przy użyciu protokołu Kerberos i WebListener.</span><span class="sxs-lookup"><span data-stu-id="ee997-164">User mode authentication isn't supported with Kerberos and WebListener.</span></span> <span data-ttu-id="ee997-165">Konto komputera należy używany do odszyfrowywania tokenu/biletu Kerberos uzyskany z usługi Active Directory i przesyłany dalej przez klienta do serwera w celu uwierzytelnienia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ee997-165">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="ee997-166">Rejestrowanie głównej nazwy usługi (SPN) dla hosta, a nie użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee997-166">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

::: moniker-end

## <a name="work-with-windows-authentication"></a><span data-ttu-id="ee997-167">Praca z uwierzytelnianiem Windows</span><span class="sxs-lookup"><span data-stu-id="ee997-167">Work with Windows Authentication</span></span>

<span data-ttu-id="ee997-168">Stan konfiguracji dostępu anonimowego określa sposób, w którym `[Authorize]` i `[AllowAnonymous]` atrybuty są używane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee997-168">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="ee997-169">W poniższych sekcjach dwóch opisano sposób obsługi konfiguracji niedozwolonych i dozwolone stany dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="ee997-169">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="ee997-170">Nie zezwalaj na dostęp anonimowy</span><span class="sxs-lookup"><span data-stu-id="ee997-170">Disallow anonymous access</span></span>

<span data-ttu-id="ee997-171">Gdy jest włączone uwierzytelnianie Windows i dostęp anonimowy jest wyłączony, `[Authorize]` i `[AllowAnonymous]` atrybuty mają żadnego skutku.</span><span class="sxs-lookup"><span data-stu-id="ee997-171">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="ee997-172">Jeśli witryna usług IIS (lub serwera HTTP.sys lub WebListener) jest skonfigurowany do nie zezwalaj na dostęp anonimowy, żądanie nigdy nie osiągnie Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee997-172">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="ee997-173">Z tego powodu `[AllowAnonymous]` atrybut nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="ee997-173">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="ee997-174">Zezwalaj na dostęp anonimowy</span><span class="sxs-lookup"><span data-stu-id="ee997-174">Allow anonymous access</span></span>

<span data-ttu-id="ee997-175">Po włączeniu uwierzytelniania Windows i dostęp anonimowy używać `[Authorize]` i `[AllowAnonymous]` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="ee997-175">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="ee997-176">`[Authorize]` Atrybut umożliwia zabezpieczenie rodzajów aplikacji, które naprawdę wymagają uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="ee997-176">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="ee997-177">`[AllowAnonymous]` Atrybutu zastąpienia `[Authorize]` atrybutu do użycia w aplikacjach, które zezwala na dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="ee997-177">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="ee997-178">Zobacz [autoryzacja prosta](xref:security/authorization/simple) szczegóły użycia atrybutu.</span><span class="sxs-lookup"><span data-stu-id="ee997-178">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="ee997-179">W programie ASP.NET Core 2.x, `[Authorize]` atrybut wymaga dodatkowej konfiguracji w *Startup.cs* zażąda anonimowe żądania uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="ee997-179">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="ee997-180">Zalecana konfiguracja zależy od nieco używany serwer sieci web.</span><span class="sxs-lookup"><span data-stu-id="ee997-180">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="ee997-181">Domyślnie użytkownicy, którzy nie mają autoryzacji do dostępu do strony są prezentowane z pustą odpowiedź HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="ee997-181">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="ee997-182">[Oprogramowania pośredniczącego StatusCodePages](xref:fundamentals/error-handling#configure-status-code-pages) można skonfigurować, aby zapewnić użytkownikom lepsze środowisko "Odmowa dostępu".</span><span class="sxs-lookup"><span data-stu-id="ee997-182">The [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="ee997-183">IIS</span><span class="sxs-lookup"><span data-stu-id="ee997-183">IIS</span></span>

<span data-ttu-id="ee997-184">Jeśli korzystanie z usług IIS, należy dodać następujące polecenie, aby `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="ee997-184">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="ee997-185">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ee997-185">HTTP.sys</span></span>

<span data-ttu-id="ee997-186">Jeśli używasz HTTP.sys, Dodaj następujące polecenie, aby `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="ee997-186">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="ee997-187">Personifikacja</span><span class="sxs-lookup"><span data-stu-id="ee997-187">Impersonation</span></span>

<span data-ttu-id="ee997-188">Platforma ASP.NET Core nie implementuje personifikacji.</span><span class="sxs-lookup"><span data-stu-id="ee997-188">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="ee997-189">Aplikacje są uruchamiane przy użyciu tożsamości aplikacji dla wszystkich żądań, przy użyciu tożsamości puli lub procesu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee997-189">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="ee997-190">Jeśli musisz jawnie wykonaj akcję w imieniu użytkownika, należy użyć `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="ee997-190">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="ee997-191">Uruchomić jedną akcję w tym kontekście, a następnie zamknij kontekstu.</span><span class="sxs-lookup"><span data-stu-id="ee997-191">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="ee997-192">Należy pamiętać, że `RunImpersonated` nie obsługuje operacji asynchronicznych i nie powinny być używane w przypadku złożonych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="ee997-192">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="ee997-193">Na przykład zawijania całego żądania lub łańcuchów oprogramowanie pośredniczące nie jest obsługiwany lub zalecane.</span><span class="sxs-lookup"><span data-stu-id="ee997-193">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>
