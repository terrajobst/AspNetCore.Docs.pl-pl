---
title: Skonfiguruj uwierzytelnianie systemu Windows w ASP.NET Core
author: ardalis
description: W tym artykule opisano sposób konfigurowania uwierzytelniania systemu Windows w ASP.NET, za pomocą usług IIS Express, usługi IIS, sterownik HTTP.sys i WebListener Core.
manager: wpickett
ms.author: riande
ms.date: 10/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/windowsauth
ms.openlocfilehash: dbcef095561fe656bdd28c4fa6560c6b269a2db0
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/31/2018
ms.locfileid: "34689012"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="79954-103">Skonfiguruj uwierzytelnianie systemu Windows w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79954-103">Configure Windows authentication in ASP.NET Core</span></span>

<span data-ttu-id="79954-104">Przez [Steve Smith](https://ardalis.com) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="79954-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="79954-105">Uwierzytelnianie systemu Windows dla aplikacji platformy ASP.NET Core hostowanych w przypadku usług IIS można skonfigurować [HTTP.sys](xref:fundamentals/servers/httpsys), lub [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="79954-105">Windows authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="79954-106">Co to jest uwierzytelnianie systemu Windows?</span><span class="sxs-lookup"><span data-stu-id="79954-106">What is Windows authentication?</span></span>

<span data-ttu-id="79954-107">Uwierzytelnianie systemu Windows, zależy od systemu operacyjnego, aby uwierzytelniać użytkowników aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="79954-107">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="79954-108">Można użyć uwierzytelniania systemu Windows, gdy serwer działa w sieci firmowej przy użyciu tożsamości domeny usługi Active Directory lub inne konta systemu Windows do identyfikowania użytkowników.</span><span class="sxs-lookup"><span data-stu-id="79954-108">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="79954-109">Uwierzytelnianie systemu Windows jest najodpowiedniejsze do środowisk intranetowych, w których użytkownicy, aplikacje klienckie i serwery sieci web należą do tej samej domeny systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="79954-109">Windows authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="79954-110">[Więcej informacji na temat uwierzytelniania systemu Windows i instalując je dla usług IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="79954-110">[Learn more about Windows authentication and installing it for IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="79954-111">Włącz uwierzytelnianie systemu Windows w aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79954-111">Enable Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="79954-112">Szablonu aplikacji sieci Web w usłudze Visual Studio może być skonfigurowana do obsługi uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="79954-112">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="79954-113">Szablon aplikacji uwierzytelniania systemu Windows</span><span class="sxs-lookup"><span data-stu-id="79954-113">Use the Windows authentication app template</span></span>

<span data-ttu-id="79954-114">W programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="79954-114">In Visual Studio:</span></span>
1. <span data-ttu-id="79954-115">Utwórz nową aplikację sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="79954-115">Create a new ASP.NET Core Web Application.</span></span> 
1. <span data-ttu-id="79954-116">Wybierz aplikację sieci Web z listy szablonów.</span><span class="sxs-lookup"><span data-stu-id="79954-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="79954-117">Wybierz **Zmień uwierzytelnianie** i wybrać **uwierzytelniania systemu Windows**.</span><span class="sxs-lookup"><span data-stu-id="79954-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="79954-118">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="79954-118">Run the app.</span></span> <span data-ttu-id="79954-119">Nazwa użytkownika jest wyświetlany w górnym rogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="79954-119">The username appears in the top right of the app.</span></span>

![Zrzut ekranu przeglądarki uwierzytelniania systemu Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="79954-121">W przypadku projektach przy użyciu usług IIS Express szablonu zapewnia konfiguracji koniecznych do używania uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="79954-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="79954-122">Poniższej sekcji pokazano, jak ręcznie skonfigurować aplikację ASP.NET Core dla uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="79954-122">The following section shows how to manually configure an ASP.NET Core app for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="79954-123">Ustawienia programu Visual Studio dla systemu Windows i uwierzytelnianie anonimowe</span><span class="sxs-lookup"><span data-stu-id="79954-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="79954-124">Projekt programu Visual Studio **właściwości** strony **debugowania** karta zawiera pola wyboru dla uwierzytelniania anonimowego i uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="79954-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Zrzut ekranu przeglądarki uwierzytelniania systemu Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="79954-126">Alternatywnie można skonfigurować te dwie właściwości w *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="79954-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="79954-127">Włącz uwierzytelnianie systemu Windows z programem IIS</span><span class="sxs-lookup"><span data-stu-id="79954-127">Enable Windows authentication with IIS</span></span>

<span data-ttu-id="79954-128">Usługi IIS używają [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) do aplikacji platformy ASP.NET Core hosta.</span><span class="sxs-lookup"><span data-stu-id="79954-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="79954-129">Moduł umożliwia przepływać do usług IIS domyślnie uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="79954-129">The module allows Windows authentication to flow to IIS by default.</span></span> <span data-ttu-id="79954-130">Uwierzytelnianie systemu Windows jest skonfigurowany w usługach IIS, a nie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="79954-130">Windows authentication is configured in IIS, not the app.</span></span> <span data-ttu-id="79954-131">Poniższe sekcje pokazują, jak skonfigurować aplikację ASP.NET Core uwierzytelniania systemu Windows za pomocą Menedżera usług IIS.</span><span class="sxs-lookup"><span data-stu-id="79954-131">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication.</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="79954-132">Utwórz nową witrynę IIS</span><span class="sxs-lookup"><span data-stu-id="79954-132">Create a new IIS site</span></span>

<span data-ttu-id="79954-133">Określ nazwę i folder i zezwolenie na tworzenie nowej puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="79954-133">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="79954-134">Dostosowywanie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="79954-134">Customize authentication</span></span>

<span data-ttu-id="79954-135">Otwórz menu uwierzytelniania dla tej lokacji.</span><span class="sxs-lookup"><span data-stu-id="79954-135">Open the Authentication menu for the site.</span></span>

![Menu uwierzytelniania usług IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="79954-137">Należy wyłączyć uwierzytelnianie anonimowe, a następnie włączyć uwierzytelnianie systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="79954-137">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Ustawienia uwierzytelniania usług IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="79954-139">Publikowanie projektu do folderu witryny usług IIS</span><span class="sxs-lookup"><span data-stu-id="79954-139">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="79954-140">Przy użyciu programu Visual Studio lub .NET Core interfejsu wiersza polecenia, Opublikuj aplikację do folderu docelowego.</span><span class="sxs-lookup"><span data-stu-id="79954-140">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Okno dialogowe publikowanie programu Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="79954-142">Dowiedz się więcej o [publikowania w usługach IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="79954-142">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="79954-143">Uruchom aplikację w celu zweryfikowania, czy działa uwierzytelnianie systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="79954-143">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a><span data-ttu-id="79954-144">Włącz uwierzytelnianie systemu Windows z pliku HTTP.sys lub WebListener</span><span class="sxs-lookup"><span data-stu-id="79954-144">Enable Windows authentication with HTTP.sys or WebListener</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="79954-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="79954-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="79954-146">Chociaż Kestrel nie obsługuje uwierzytelniania systemu Windows, można użyć [HTTP.sys](xref:fundamentals/servers/httpsys) na potrzeby obsługi scenariuszy własnym hostowanej w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="79954-146">Although Kestrel doesn't support Windows authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="79954-147">Poniższy przykład umożliwia skonfigurowanie aplikacji sieci web hosta do pliku HTTP.sys za pomocą uwierzytelniania systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="79954-147">The following example configures the app's web host to use HTTP.sys with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="79954-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="79954-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="79954-149">Chociaż Kestrel nie obsługuje uwierzytelniania systemu Windows, można użyć [WebListener](xref:fundamentals/servers/weblistener) na potrzeby obsługi scenariuszy własnym hostowanej w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="79954-149">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="79954-150">Poniższy przykład umożliwia skonfigurowanie aplikacji sieci web hosta do WebListener za pomocą uwierzytelniania systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="79954-150">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a><span data-ttu-id="79954-151">Praca z uwierzytelnianiem systemu Windows</span><span class="sxs-lookup"><span data-stu-id="79954-151">Work with Windows authentication</span></span>

<span data-ttu-id="79954-152">Stan konfiguracji dostęp anonimowy Określa sposób, w którym `[Authorize]` i `[AllowAnonymous]` atrybuty są używane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="79954-152">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="79954-153">W poniższych sekcjach dwóch opisano sposób obsługi konfiguracji niedozwolonych i dozwolone stany dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="79954-153">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="79954-154">Nie zezwalaj na dostęp anonimowy</span><span class="sxs-lookup"><span data-stu-id="79954-154">Disallow anonymous access</span></span>

<span data-ttu-id="79954-155">Jeśli jest włączone uwierzytelnianie systemu Windows i dostęp anonimowy jest wyłączony, `[Authorize]` i `[AllowAnonymous]` atrybutów nie obowiązują.</span><span class="sxs-lookup"><span data-stu-id="79954-155">When Windows authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="79954-156">Jeśli witryna usług IIS (lub serwera HTTP.sys lub WebListener) jest skonfigurowany do nie zezwalaj na dostęp anonimowy, żądania nigdy nie osiągnie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="79954-156">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="79954-157">Z tego powodu `[AllowAnonymous]` atrybut nie jest stosowana.</span><span class="sxs-lookup"><span data-stu-id="79954-157">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="79954-158">Zezwalaj na dostęp anonimowy</span><span class="sxs-lookup"><span data-stu-id="79954-158">Allow anonymous access</span></span>

<span data-ttu-id="79954-159">Gdy zarówno uwierzytelnianie systemu Windows i dostęp anonimowy jest włączona, należy użyć `[Authorize]` i `[AllowAnonymous]` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="79954-159">When both Windows authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="79954-160">`[Authorize]` Atrybutu umożliwia zabezpieczenie częściach aplikacji, które są rzeczywiście wymagają uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="79954-160">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows authentication.</span></span> <span data-ttu-id="79954-161">`[AllowAnonymous]` Atrybutu zastąpienia `[Authorize]` atrybutu użycia w aplikacjach, które zezwala na dostęp anonimowy.</span><span class="sxs-lookup"><span data-stu-id="79954-161">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="79954-162">Zobacz [proste autoryzacji](xref:security/authorization/simple) szczegóły użycia atrybutu.</span><span class="sxs-lookup"><span data-stu-id="79954-162">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="79954-163">W przypadku platformy ASP.NET Core 2.x, `[Authorize]` atrybut wymaga dodatkowych czynności konfiguracyjnych *Startup.cs* zażąda anonimowe żądania uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="79954-163">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows authentication.</span></span> <span data-ttu-id="79954-164">Zalecana konfiguracja zależy od nieco używany serwer sieci web.</span><span class="sxs-lookup"><span data-stu-id="79954-164">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="79954-165">Domyślnie użytkownicy, którzy nie mają zezwolenia na dostęp do strony są prezentowane z pustą odpowiedź HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="79954-165">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="79954-166">[Oprogramowanie pośredniczące StatusCodePages](xref:fundamentals/error-handling#configuring-status-code-pages) można skonfigurować, aby zapewnić użytkownikom lepsze środowisko "Odmowa dostępu".</span><span class="sxs-lookup"><span data-stu-id="79954-166">The [StatusCodePages middleware](xref:fundamentals/error-handling#configuring-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="79954-167">IIS</span><span class="sxs-lookup"><span data-stu-id="79954-167">IIS</span></span>

<span data-ttu-id="79954-168">Jeśli za pomocą usług IIS, Dodaj następujący kod do `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="79954-168">If using IIS, add the following to the `ConfigureServices` method:</span></span> 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="79954-169">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="79954-169">HTTP.sys</span></span>

<span data-ttu-id="79954-170">Jeśli z pliku HTTP.sys, Dodaj następujący kod do `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="79954-170">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="79954-171">Personifikacja</span><span class="sxs-lookup"><span data-stu-id="79954-171">Impersonation</span></span>

<span data-ttu-id="79954-172">Nie zawiera implementacji personifikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="79954-172">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="79954-173">Aplikacje są uruchamiane z tożsamości aplikacji dla wszystkich żądań przy użyciu tożsamości puli lub procesu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="79954-173">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="79954-174">Jeśli musisz jawnie wykonania akcji w imieniu użytkownika, użyj `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="79954-174">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="79954-175">Uruchom jednego działania w tym kontekście, a następnie zamknij kontekstu.</span><span class="sxs-lookup"><span data-stu-id="79954-175">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="79954-176">Należy pamiętać, że `RunImpersonated` nie obsługuje operacji asynchronicznych i nie powinny być używane do złożonych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="79954-176">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="79954-177">Na przykład zawijania całego żądania lub łańcuchów oprogramowanie pośredniczące nie jest obsługiwany lub zalecane.</span><span class="sxs-lookup"><span data-stu-id="79954-177">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

---
