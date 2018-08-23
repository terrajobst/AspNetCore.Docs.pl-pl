---
title: Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core
author: ardalis
description: W tym artykule opisano sposób konfigurowania uwierzytelniania Windows w programie ASP.NET Core, za pomocą usług IIS Express, usługi IIS, sterownik HTTP.sys i WebListener.
ms.author: riande
ms.date: 08/18/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 93b1a1de74ef6554d48709b04870f7e23738846b
ms.sourcegitcommit: 15d7bd0b2c4e6fe9ac335d658bab71a45ca5bc72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/20/2018
ms.locfileid: "41754512"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="1abc2-103">Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1abc2-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="1abc2-104">Przez [Steve Smith](https://ardalis.com) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="1abc2-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="1abc2-105">Dla aplikacji platformy ASP.NET Core, obsługiwane w przypadku usług IIS można skonfigurować uwierzytelniania Windows [HTTP.sys](xref:fundamentals/servers/httpsys), lub [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="1abc2-105">Windows Authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="windows-authentication"></a><span data-ttu-id="1abc2-106">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="1abc2-106">Windows Authentication</span></span>

<span data-ttu-id="1abc2-107">Uwierzytelnianie Windows opiera się uwierzytelniać użytkowników aplikacji platformy ASP.NET Core w systemie operacyjnym.</span><span class="sxs-lookup"><span data-stu-id="1abc2-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="1abc2-108">Możesz użyć uwierzytelniania Windows, gdy serwer działa w sieci firmowej przy użyciu tożsamości domeny usługi Active Directory lub innym kontom Windows do identyfikacji użytkowników.</span><span class="sxs-lookup"><span data-stu-id="1abc2-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="1abc2-109">Uwierzytelnianie Windows najlepiej nadaje się do środowisk intranetowych, w których użytkownicy, aplikacje klienckie i serwery sieci web należą do tej samej domeny Windows.</span><span class="sxs-lookup"><span data-stu-id="1abc2-109">Windows Authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="1abc2-110">[Dowiedz się więcej o uwierzytelnianiu Windows i instalując je dla usług IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="1abc2-110">[Learn more about Windows Authentication and installing it for IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="1abc2-111">Włączanie uwierzytelniania Windows w aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1abc2-111">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="1abc2-112">Szablon aplikacji sieci Web w usłudze Visual Studio można skonfigurować tak, aby zapewnić obsługę uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="1abc2-112">The Visual Studio Web Application template can be configured to support Windows Authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="1abc2-113">Korzystanie z szablonu aplikacji uwierzytelnianie Windows</span><span class="sxs-lookup"><span data-stu-id="1abc2-113">Use the Windows Authentication app template</span></span>

<span data-ttu-id="1abc2-114">W programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="1abc2-114">In Visual Studio:</span></span>

1. <span data-ttu-id="1abc2-115">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1abc2-115">Create a new ASP.NET Core Web Application.</span></span>
1. <span data-ttu-id="1abc2-116">Wybierz aplikację sieci Web z listy szablonów.</span><span class="sxs-lookup"><span data-stu-id="1abc2-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="1abc2-117">Wybierz **Zmień uwierzytelnianie** i wybrać **uwierzytelniania Windows**.</span><span class="sxs-lookup"><span data-stu-id="1abc2-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span>

<span data-ttu-id="1abc2-118">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="1abc2-118">Run the app.</span></span> <span data-ttu-id="1abc2-119">Nazwa użytkownika jest wyświetlana w prawym górnym rogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1abc2-119">The username appears in the top right of the app.</span></span>

![Zrzut ekranu przeglądarki uwierzytelnianie Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="1abc2-121">Prace deweloperskie za pomocą usług IIS Express szablon zawiera wszystkie konfiguracje niezbędne do korzystania z uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="1abc2-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows Authentication.</span></span> <span data-ttu-id="1abc2-122">W poniższej sekcji pokazano, jak ręcznie skonfigurować aplikację ASP.NET Core dla uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="1abc2-122">The following section shows how to manually configure an ASP.NET Core app for Windows Authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="1abc2-123">Ustawienia programu Visual Studio for Windows i uwierzytelnianie anonimowe</span><span class="sxs-lookup"><span data-stu-id="1abc2-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="1abc2-124">Projekt programu Visual Studio **właściwości** strony **debugowania** karta zawiera pola wyboru dla uwierzytelniania Windows i uwierzytelnianie anonimowe.</span><span class="sxs-lookup"><span data-stu-id="1abc2-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows Authentication and anonymous authentication.</span></span>

![Zrzut ekranu przeglądarki uwierzytelnianie Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="1abc2-126">Alternatywnie, można skonfigurować te dwie właściwości w *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="1abc2-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="1abc2-127">Włączanie uwierzytelniania Windows za pomocą usług IIS</span><span class="sxs-lookup"><span data-stu-id="1abc2-127">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="1abc2-128">Usługi IIS używają [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) hostująca aplikacje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1abc2-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="1abc2-129">Uwierzytelnianie Windows jest skonfigurowany w usługach IIS, a nie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1abc2-129">Windows Authentication is configured in IIS, not the app.</span></span> <span data-ttu-id="1abc2-130">Poniższe sekcje pokazują, jak skonfigurować aplikację ASP.NET Core, aby korzystać z uwierzytelniania Windows za pomocą Menedżera usług IIS.</span><span class="sxs-lookup"><span data-stu-id="1abc2-130">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows Authentication.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="1abc2-131">Konfiguracja programu IIS</span><span class="sxs-lookup"><span data-stu-id="1abc2-131">IIS configuration</span></span>

<span data-ttu-id="1abc2-132">Włączyć usługi roli usług IIS na potrzeby uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="1abc2-132">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="1abc2-133">Aby uzyskać więcej informacji, zobacz [Włącz uwierzytelnianie Windows usługami roli usług IIS (zobacz krok 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="1abc2-133">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="1abc2-134">Oprogramowania pośredniczącego integracji usługi IIS jest domyślnie skonfigurowana do automatycznego uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="1abc2-134">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="1abc2-135">Aby uzyskać więcej informacji, zobacz [hosta ASP.NET Core na Windows za pomocą programu IIS: IIS opcje (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="1abc2-135">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="1abc2-136">Modułu ASP.NET Core jest domyślnie skonfigurowana do przekazywania tokenu uwierzytelniania Windows do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1abc2-136">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="1abc2-137">Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji modułu ASP.NET Core: atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="1abc2-137">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="1abc2-138">Utwórz nową witrynę IIS</span><span class="sxs-lookup"><span data-stu-id="1abc2-138">Create a new IIS site</span></span>

<span data-ttu-id="1abc2-139">Określ nazwę i folder, a następnie zezwala utworzyć nową pulę aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1abc2-139">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="1abc2-140">Dostosowywanie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="1abc2-140">Customize authentication</span></span>

<span data-ttu-id="1abc2-141">Otwórz funkcje uwierzytelniania dla tej witryny.</span><span class="sxs-lookup"><span data-stu-id="1abc2-141">Open the Authentication features for the site.</span></span>

![Menu uwierzytelniania usług IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="1abc2-143">Wyłączyć uwierzytelnianie anonimowe, a następnie włączyć uwierzytelnianie Windows.</span><span class="sxs-lookup"><span data-stu-id="1abc2-143">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Ustawienia uwierzytelniania usług IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="1abc2-145">Publikowanie projektu do folderu witryny usług IIS</span><span class="sxs-lookup"><span data-stu-id="1abc2-145">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="1abc2-146">Za pomocą programu Visual Studio lub interfejsu wiersza polecenia platformy .NET Core, Opublikuj aplikację w folderze docelowym.</span><span class="sxs-lookup"><span data-stu-id="1abc2-146">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Okno dialogowe publikowanie w programie Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="1abc2-148">Dowiedz się więcej o [publikowania w usługach IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="1abc2-148">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="1abc2-149">Uruchom aplikację, aby sprawdzić, czy działa uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="1abc2-149">Launch the app to verify Windows Authentication is working.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="1abc2-150">Włącz uwierzytelnianie Windows w pliku HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="1abc2-150">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="1abc2-151">Chociaż Kestrel nie obsługuje uwierzytelniania Windows, możesz użyć [HTTP.sys](xref:fundamentals/servers/httpsys) do obsługi scenariuszy samodzielnie hostowanego na Windows.</span><span class="sxs-lookup"><span data-stu-id="1abc2-151">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="1abc2-152">Poniższy przykład umożliwia skonfigurowanie aplikacji hosta sieci web HTTP.sys za pomocą uwierzytelniania Windows:</span><span class="sxs-lookup"><span data-stu-id="1abc2-152">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="1abc2-153">Sterownik HTTP.sys delegatów, aby uwierzytelnianie trybu jądra za pomocą protokołu uwierzytelniania Kerberos.</span><span class="sxs-lookup"><span data-stu-id="1abc2-153">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="1abc2-154">Uwierzytelnianie w trybie użytkownika nie jest obsługiwana przy użyciu protokołu Kerberos i sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="1abc2-154">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="1abc2-155">Konto komputera należy używany do odszyfrowywania tokenu/biletu Kerberos uzyskany z usługi Active Directory i przesyłany dalej przez klienta do serwera w celu uwierzytelnienia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1abc2-155">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="1abc2-156">Rejestrowanie głównej nazwy usługi (SPN) dla hosta, a nie użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1abc2-156">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

## <a name="enable-windows-authentication-with-weblistener"></a><span data-ttu-id="1abc2-157">Włączanie uwierzytelniania Windows za pomocą WebListener</span><span class="sxs-lookup"><span data-stu-id="1abc2-157">Enable Windows Authentication with WebListener</span></span>

<span data-ttu-id="1abc2-158">Chociaż Kestrel nie obsługuje uwierzytelniania Windows, możesz użyć [WebListener](xref:fundamentals/servers/weblistener) do obsługi scenariuszy samodzielnie hostowanego na Windows.</span><span class="sxs-lookup"><span data-stu-id="1abc2-158">Although Kestrel doesn't support Windows Authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="1abc2-159">Poniższy przykład umożliwia skonfigurowanie aplikacji sieci web hosta WebListener za pomocą uwierzytelniania Windows:</span><span class="sxs-lookup"><span data-stu-id="1abc2-159">The following example configures the app's web host to use WebListener with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

> [!NOTE]
> <span data-ttu-id="1abc2-160">WebListener delegatów, aby uwierzytelnianie trybu jądra za pomocą protokołu uwierzytelniania Kerberos.</span><span class="sxs-lookup"><span data-stu-id="1abc2-160">WebListener delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="1abc2-161">Uwierzytelnianie w trybie użytkownika nie jest obsługiwana przy użyciu protokołu Kerberos i WebListener.</span><span class="sxs-lookup"><span data-stu-id="1abc2-161">User mode authentication isn't supported with Kerberos and WebListener.</span></span> <span data-ttu-id="1abc2-162">Konto komputera należy używany do odszyfrowywania tokenu/biletu Kerberos uzyskany z usługi Active Directory i przesyłany dalej przez klienta do serwera w celu uwierzytelnienia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1abc2-162">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="1abc2-163">Rejestrowanie głównej nazwy usługi (SPN) dla hosta, a nie użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1abc2-163">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

::: moniker-end

## <a name="work-with-windows-authentication"></a><span data-ttu-id="1abc2-164">Praca z uwierzytelnianiem Windows</span><span class="sxs-lookup"><span data-stu-id="1abc2-164">Work with Windows Authentication</span></span>

<span data-ttu-id="1abc2-165">Stan konfiguracji dostępu anonimowego określa sposób, w którym `[Authorize]` i `[AllowAnonymous]` atrybuty są używane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1abc2-165">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="1abc2-166">W poniższych sekcjach dwóch opisano sposób obsługi konfiguracji niedozwolonych i dozwolone stany dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="1abc2-166">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="1abc2-167">Nie zezwalaj na dostęp anonimowy</span><span class="sxs-lookup"><span data-stu-id="1abc2-167">Disallow anonymous access</span></span>

<span data-ttu-id="1abc2-168">Gdy jest włączone uwierzytelnianie Windows i dostęp anonimowy jest wyłączony, `[Authorize]` i `[AllowAnonymous]` atrybuty mają żadnego skutku.</span><span class="sxs-lookup"><span data-stu-id="1abc2-168">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="1abc2-169">Jeśli witryna usług IIS (lub serwera HTTP.sys lub WebListener) jest skonfigurowany do nie zezwalaj na dostęp anonimowy, żądanie nigdy nie osiągnie Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1abc2-169">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="1abc2-170">Z tego powodu `[AllowAnonymous]` atrybut nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="1abc2-170">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="1abc2-171">Zezwalaj na dostęp anonimowy</span><span class="sxs-lookup"><span data-stu-id="1abc2-171">Allow anonymous access</span></span>

<span data-ttu-id="1abc2-172">Po włączeniu uwierzytelniania Windows i dostęp anonimowy używać `[Authorize]` i `[AllowAnonymous]` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="1abc2-172">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="1abc2-173">`[Authorize]` Atrybut umożliwia zabezpieczenie rodzajów aplikacji, które naprawdę wymagają uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="1abc2-173">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="1abc2-174">`[AllowAnonymous]` Atrybutu zastąpienia `[Authorize]` atrybutu do użycia w aplikacjach, które zezwala na dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="1abc2-174">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="1abc2-175">Zobacz [autoryzacja prosta](xref:security/authorization/simple) szczegóły użycia atrybutu.</span><span class="sxs-lookup"><span data-stu-id="1abc2-175">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="1abc2-176">W programie ASP.NET Core 2.x, `[Authorize]` atrybut wymaga dodatkowej konfiguracji w *Startup.cs* zażąda anonimowe żądania uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="1abc2-176">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="1abc2-177">Zalecana konfiguracja zależy od nieco używany serwer sieci web.</span><span class="sxs-lookup"><span data-stu-id="1abc2-177">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="1abc2-178">Domyślnie użytkownicy, którzy nie mają autoryzacji do dostępu do strony są prezentowane z pustą odpowiedź HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="1abc2-178">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="1abc2-179">[Oprogramowania pośredniczącego StatusCodePages](xref:fundamentals/error-handling#configuring-status-code-pages) można skonfigurować, aby zapewnić użytkownikom lepsze środowisko "Odmowa dostępu".</span><span class="sxs-lookup"><span data-stu-id="1abc2-179">The [StatusCodePages middleware](xref:fundamentals/error-handling#configuring-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="1abc2-180">IIS</span><span class="sxs-lookup"><span data-stu-id="1abc2-180">IIS</span></span>

<span data-ttu-id="1abc2-181">Jeśli korzystanie z usług IIS, należy dodać następujące polecenie, aby `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="1abc2-181">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="1abc2-182">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="1abc2-182">HTTP.sys</span></span>

<span data-ttu-id="1abc2-183">Jeśli używasz HTTP.sys, Dodaj następujące polecenie, aby `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="1abc2-183">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="1abc2-184">Personifikacja</span><span class="sxs-lookup"><span data-stu-id="1abc2-184">Impersonation</span></span>

<span data-ttu-id="1abc2-185">Platforma ASP.NET Core nie implementuje personifikacji.</span><span class="sxs-lookup"><span data-stu-id="1abc2-185">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="1abc2-186">Aplikacje są uruchamiane przy użyciu tożsamości aplikacji dla wszystkich żądań, przy użyciu tożsamości puli lub procesu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1abc2-186">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="1abc2-187">Jeśli musisz jawnie wykonaj akcję w imieniu użytkownika, należy użyć `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="1abc2-187">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="1abc2-188">Uruchomić jedną akcję w tym kontekście, a następnie zamknij kontekstu.</span><span class="sxs-lookup"><span data-stu-id="1abc2-188">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="1abc2-189">Należy pamiętać, że `RunImpersonated` nie obsługuje operacji asynchronicznych i nie powinny być używane w przypadku złożonych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="1abc2-189">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="1abc2-190">Na przykład zawijania całego żądania lub łańcuchów oprogramowanie pośredniczące nie jest obsługiwany lub zalecane.</span><span class="sxs-lookup"><span data-stu-id="1abc2-190">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>
