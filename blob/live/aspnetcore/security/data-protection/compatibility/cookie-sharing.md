---
title: "Udostępnianie plików cookie między aplikacjami"
author: rick-anderson
description: "Dowiedz się, jak udostępniać pliki cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: 034c080c3459cd3a9e6b412492d1b19d9e6348a4
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
---
# <a name="sharing-cookies-among-apps"></a><span data-ttu-id="d85d5-103">Udostępnianie plików cookie między aplikacjami</span><span class="sxs-lookup"><span data-stu-id="d85d5-103">Sharing cookies among apps</span></span>

<span data-ttu-id="d85d5-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d85d5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d85d5-105">Witryny sieci Web często składają się z poszczególnych aplikacji web apps pracujących razem.</span><span class="sxs-lookup"><span data-stu-id="d85d5-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="d85d5-106">Aby zapewnić środowisko rejestracji jednokrotnej (SSO), aplikacje sieci web w obrębie lokacji muszą współdzielić plików cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="d85d5-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="d85d5-107">Z tym scenariuszem stosu ochrony danych umożliwia udostępnianie Katana pliku cookie uwierzytelniania i biletów uwierzytelniania pliku cookie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d85d5-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="d85d5-108">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d85d5-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d85d5-109">Przykładową udostępnianie w trzech aplikacji, które korzystają z plików cookie uwierzytelniania pliku cookie:</span><span class="sxs-lookup"><span data-stu-id="d85d5-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="d85d5-110">Aplikacja ASP.NET Core 2.0 Razor strony bez użycia [ASP.NET Core Identity](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="d85d5-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="d85d5-111">Platformy ASP.NET Core 2.0 aplikacji MVC za pomocą tożsamości platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d85d5-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="d85d5-112">Struktura ASP.NET 4.6.1 aplikacji MVC za pomocą tożsamości platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d85d5-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="d85d5-113">W przykładach, które należy wykonać:</span><span class="sxs-lookup"><span data-stu-id="d85d5-113">In the examples that follow:</span></span>

* <span data-ttu-id="d85d5-114">Nazwa pliku cookie uwierzytelniania ma ustawioną wartość wspólnej `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="d85d5-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="d85d5-115">`AuthenticationType` Ustawiono `Identity.Application` jawnie lub domyślnie.</span><span class="sxs-lookup"><span data-stu-id="d85d5-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="d85d5-116">[CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) jest używany jako schemat uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="d85d5-116">[CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) is used as the authentication scheme.</span></span> <span data-ttu-id="d85d5-117">Stała wartość jest rozpoznawany jako `Cookies`.</span><span class="sxs-lookup"><span data-stu-id="d85d5-117">The constant resolves to a value of `Cookies`.</span></span>
* <span data-ttu-id="d85d5-118">Implementacja używa oprogramowania pośredniczącego uwierzytelniania pliku cookie [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="d85d5-118">The cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="d85d5-119">`DataProtectionProvider`zapewnia usługi ochrony danych na potrzeby szyfrowania i odszyfrowywania danych ładunku plików cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="d85d5-119">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="d85d5-120">`DataProtectionProvider` Wystąpienie jest odizolowana od systemu ochrony danych, które są używane przez inne części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d85d5-120">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="d85d5-121">Popularne [klucz ochrony danych](xref:security/data-protection/implementation/key-management) lokalizacja magazynu jest używana.</span><span class="sxs-lookup"><span data-stu-id="d85d5-121">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="d85d5-122">Przykładowa aplikacja korzysta z folderem o nazwie *zestaw kluczy* w katalogu głównym rozwiązania do przechowywania kluczy ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="d85d5-122">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
  * <span data-ttu-id="d85d5-123">[DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) akceptuje [DirectoryInfo](/dotnet/api/system.io.directoryinfo) do użytku z plików cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="d85d5-123">[DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) for use with authentication cookies.</span></span> <span data-ttu-id="d85d5-124">Przykładowa aplikacja zawiera ścieżkę *zestaw kluczy* folder `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="d85d5-124">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span>
  * <span data-ttu-id="d85d5-125">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) wymaga [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="d85d5-125">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="d85d5-126">Odwoływać się do uzyskania tego pakietu dla platformy ASP.NET Core 2.0 i nowszych aplikacje, [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="d85d5-126">To obtain this package for ASP.NET Core 2.0 and later apps, reference the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="d85d5-127">Jeśli celem .NET Framework, Dodaj odwołanie do pakietu `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="d85d5-127">When targeting .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="d85d5-128">Udostępnianie plików cookie uwierzytelniania między aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d85d5-128">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="d85d5-129">Podczas używania ASP.NET Core Identity:</span><span class="sxs-lookup"><span data-stu-id="d85d5-129">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d85d5-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d85d5-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d85d5-131">W `ConfigureServices` metody, użyj [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) — metoda rozszerzenia, aby skonfigurować usługę ochrony danych plików cookie.</span><span class="sxs-lookup"><span data-stu-id="d85d5-131">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="d85d5-132">Zobacz *CookieAuthWithIdentity.Core* projektu w [przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d85d5-132">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d85d5-133">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d85d5-133">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d85d5-134">W `Configure` metody, użyj [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) do skonfigurowania:</span><span class="sxs-lookup"><span data-stu-id="d85d5-134">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="d85d5-135">Usługa ochrony danych plików cookie.</span><span class="sxs-lookup"><span data-stu-id="d85d5-135">The data protection service for cookies.</span></span>
* <span data-ttu-id="d85d5-136">`AuthenticationScheme` Odpowiadające ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="d85d5-136">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = 
        "ApplicationCookie";

    var protectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"));

    options.Cookies.ApplicationCookie.DataProtectionProvider = 
        protectionProvider;

    options.Cookies.ApplicationCookie.TicketDataFormat = 
        new TicketDataFormat(protectionProvider.CreateProtector(
            "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", 
            "Cookies", 
            "v2"));
});
```

---

<span data-ttu-id="d85d5-137">Podczas używania plików cookie bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="d85d5-137">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d85d5-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d85d5-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="d85d5-139">Zobacz *CookieAuth.Core* projektu w [przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d85d5-139">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d85d5-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d85d5-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="d85d5-141">Szyfrowanie kluczy ochrony danych magazynowanych</span><span class="sxs-lookup"><span data-stu-id="d85d5-141">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="d85d5-142">W przypadku wdrożeń produkcyjnych, skonfigurować `DataProtectionProvider` do szyfrowania kluczy magazynowane DPAPI lub X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="d85d5-142">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="d85d5-143">Zobacz [klucza szyfrowania w Rest](xref:security/data-protection/implementation/key-encryption-at-rest) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="d85d5-143">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d85d5-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d85d5-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d85d5-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d85d5-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(
        new DirectoryInfo(@"PATH_TO_KEY_RING"),
        configure =>
        {
            configure.ProtectKeysWithCertificate("thumbprint");
        })
});
```

---

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="d85d5-146">Udostępnianie plików cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d85d5-146">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="d85d5-147">Aplikacje ASP.NET 4.x, korzystających z oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana można skonfigurować do generowania plików cookie uwierzytelniania, które są zgodne z oprogramowaniem pośredniczącym uwierzytelniania pliku cookie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d85d5-147">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="d85d5-148">Dzięki temu uaktualniania aplikacji poszczególnych dużej witryny wyrywkowo zapewniając smooth jednokrotnej całej witryny.</span><span class="sxs-lookup"><span data-stu-id="d85d5-148">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

> [!TIP]
> <span data-ttu-id="d85d5-149">Jeśli aplikacja używa oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana, wywołuje `UseCookieAuthentication` w projekcie *Startup.Auth.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="d85d5-149">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="d85d5-150">Projekty aplikacji sieci web programu ASP.NET 4.x utworzone za pomocą programu Visual Studio 2013, a następnie użyć oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana domyślnie.</span><span class="sxs-lookup"><span data-stu-id="d85d5-150">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="d85d5-151">Aplikacja ASP.NET 4.x musi wskazywać na .NET Framework 4.5.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="d85d5-151">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="d85d5-152">W przeciwnym razie niezbędne pakiety NuGet uniemożliwić instalację.</span><span class="sxs-lookup"><span data-stu-id="d85d5-152">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="d85d5-153">Aby udostępnić pliki cookie uwierzytelniania między ASP.NET 4.x aplikacje i aplikacje platformy ASP.NET Core, konfigurowania aplikacji platformy ASP.NET Core, jak wspomniano powyżej, a następnie skonfiguruj aplikacje ASP.NET 4.x, wykonując poniższe kroki.</span><span class="sxs-lookup"><span data-stu-id="d85d5-153">To share authentication cookies among ASP.NET 4.x apps and ASP.NET Core apps, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x apps by following the steps below.</span></span>

1. <span data-ttu-id="d85d5-154">Zainstaluj pakiet [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) do każdej ASP.NET 4.x aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d85d5-154">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="d85d5-155">W *Startup.Auth.cs*, zlokalizuj wywołanie `UseCookieAuthentication` i zmodyfikuj go w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="d85d5-155">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="d85d5-156">Zmień nazwę pliku cookie do dopasowania nazwę używaną przez oprogramowanie pośredniczące uwierzytelniania plików cookie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d85d5-156">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="d85d5-157">Podaj wystąpienia `DataProtectionProvider` zainicjowany do wspólnej lokalizacji magazynu kluczy ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="d85d5-157">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d85d5-158">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d85d5-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="d85d5-159">Zobacz *CookieAuthWithIdentity.NETFramework* projektu w [przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d85d5-159">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="d85d5-160">Podczas generowania tożsamości użytkownika, typ uwierzytelniania musi być zgodny z typem zdefiniowanym w `AuthenticationType` ustawiony za pomocą `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="d85d5-160">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="d85d5-161">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="d85d5-161">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d85d5-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d85d5-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d85d5-163">Ustaw `CookieManager` do interop `ChunkingCookieManager` , format segmentu jest zgodny.</span><span class="sxs-lookup"><span data-stu-id="d85d5-163">Set the `CookieManager` to interop `ChunkingCookieManager` so the chunking format is compatible.</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
    CookieName = ".AspNetCore.Cookies",
    // CookieName = ".AspNetCore.ApplicationCookie", (if using ASP.NET Identity)
    // CookiePath = "...", (if necessary)
    // ...
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create(
                new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", 
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});
```

---

## <a name="use-a-common-user-database"></a><span data-ttu-id="d85d5-164">Użyj wspólną bazę danych użytkownika</span><span class="sxs-lookup"><span data-stu-id="d85d5-164">Use a common user database</span></span>

<span data-ttu-id="d85d5-165">Upewnij się, że w tej samej bazy danych użytkownika jest wskazywana systemu tożsamości dla każdej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d85d5-165">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="d85d5-166">W przeciwnym razie systemu tożsamości tworzy błędy w czasie wykonywania, podczas próby zgodny z informacjami w pliku cookie uwierzytelniania względem informacji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d85d5-166">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
