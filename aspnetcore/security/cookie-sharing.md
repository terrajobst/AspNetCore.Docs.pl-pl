---
title: Udostępnianie plików cookie uwierzytelniania między aplikacjami ASP.NET
author: rick-anderson
description: Dowiedz się, jak udostępnianie plików cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/15/2019
uid: security/cookie-sharing
ms.openlocfilehash: b2f906ac97fe79b2a66a5ab709bcbcb03ab8cc39
ms.sourcegitcommit: 1bf80f4acd62151ff8cce517f03f6fa891136409
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/15/2019
ms.locfileid: "68223921"
---
# <a name="share-authentication-cookies-among-aspnet-apps"></a><span data-ttu-id="7caa9-103">Udostępnianie plików cookie uwierzytelniania między aplikacjami ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7caa9-103">Share authentication cookies among ASP.NET apps</span></span>

<span data-ttu-id="7caa9-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7caa9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7caa9-105">Witryny sieci Web często składają się z poszczególnych aplikacji web apps pracujących razem.</span><span class="sxs-lookup"><span data-stu-id="7caa9-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="7caa9-106">Aby zapewnić środowisko logowania jednokrotnego (SSO), aplikacje sieci web w obrębie lokacji muszą współużytkować pliki cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7caa9-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="7caa9-107">Aby zapewnić obsługę tego scenariusza, stosu ochrony danych umożliwia udostępnianie Katana uwierzytelniania plików cookie i biletów uwierzytelniania pliku cookie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7caa9-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="7caa9-108">W przykładach, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="7caa9-108">In the examples that follow:</span></span>

* <span data-ttu-id="7caa9-109">Nazwa pliku cookie uwierzytelniania jest równa wartości typowych `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="7caa9-109">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="7caa9-110">`AuthenticationType` Ustawiono `Identity.Application` jawnie lub domyślnie.</span><span class="sxs-lookup"><span data-stu-id="7caa9-110">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="7caa9-111">Nazwa pospolita aplikacji służy do włączania systemu ochrony danych udostępnić klucze ochrony danych (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="7caa9-111">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="7caa9-112">`Identity.Application` jest używana jako schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7caa9-112">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="7caa9-113">Niezależnie od schematu jest używany, musi być stosowane konsekwentnie *wewnątrz i pomiędzy* aplikacji udostępnionych plików cookie, jako domyślny schemat lub przez jawne ustawienie go.</span><span class="sxs-lookup"><span data-stu-id="7caa9-113">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="7caa9-114">Schemat jest używany podczas szyfrowania i odszyfrowywania plików cookie, dzięki czemu spójny schemat musi być używana w różnych aplikacjach.</span><span class="sxs-lookup"><span data-stu-id="7caa9-114">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="7caa9-115">Często [klucz ochrony danych](xref:security/data-protection/implementation/key-management) lokalizacja magazynu jest używana.</span><span class="sxs-lookup"><span data-stu-id="7caa9-115">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span>
  * <span data-ttu-id="7caa9-116">W aplikacji platformy ASP.NET Core <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> służy do ustawiania lokalizacji magazynu kluczy.</span><span class="sxs-lookup"><span data-stu-id="7caa9-116">In ASP.NET Core apps, <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> is used to set the key storage location.</span></span>
  * <span data-ttu-id="7caa9-117">W aplikacjach .NET Framework, oprogramowanie pośredniczące uwierzytelniania plików Cookie korzysta z implementacją <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>.</span><span class="sxs-lookup"><span data-stu-id="7caa9-117">In .NET Framework apps, Cookie Authentication Middleware uses an implementation of <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>.</span></span> <span data-ttu-id="7caa9-118">`DataProtectionProvider` udostępnia usługi ochrony danych do szyfrowania i odszyfrowywania danych ładunku plików cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7caa9-118">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="7caa9-119">`DataProtectionProvider` Wystąpienie jest izolowane od system ochrony danych, które są używane przez inne części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7caa9-119">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span> <span data-ttu-id="7caa9-120">[DataProtectionProvider.Create (System.IO.DirectoryInfo, Akcja\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) akceptuje <xref:System.IO.DirectoryInfo> Określ lokalizację do przechowywania kluczy ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="7caa9-120">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) accepts a <xref:System.IO.DirectoryInfo> to specify the location for data protection key storage.</span></span>
* <span data-ttu-id="7caa9-121">`DataProtectionProvider` wymaga [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pakietu NuGet:</span><span class="sxs-lookup"><span data-stu-id="7caa9-121">`DataProtectionProvider` requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package:</span></span>
  * <span data-ttu-id="7caa9-122">W aplikacjach ASP.NET Core 2.x odwołania [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="7caa9-122">In ASP.NET Core 2.x apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="7caa9-123">W aplikacjach .NET Framework, należy dodać odwołanie do pakietu [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).</span><span class="sxs-lookup"><span data-stu-id="7caa9-123">In .NET Framework apps, add a package reference to [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).</span></span>
* <span data-ttu-id="7caa9-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> Ustawia nazwę pospolitą aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7caa9-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> sets the common app name.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="7caa9-125">Udostępnianie plików cookie uwierzytelniania między aplikacjami ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7caa9-125">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="7caa9-126">W przypadku używania tożsamości platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="7caa9-126">When using ASP.NET Core Identity:</span></span>

* <span data-ttu-id="7caa9-127">Klucze ochrony danych i nazwę aplikacji musi być współużytkowany przez aplikacje.</span><span class="sxs-lookup"><span data-stu-id="7caa9-127">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="7caa9-128">Wspólnej lokalizacji magazynu kluczy jest dostarczany w celu <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> metody w poniższych przykładach.</span><span class="sxs-lookup"><span data-stu-id="7caa9-128">A common key storage location is provided to the <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> method in the following examples.</span></span> <span data-ttu-id="7caa9-129">Użyj <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> skonfigurować nazwę pospolitą udostępnionej aplikacji (`SharedCookieApp` w poniższych przykładach).</span><span class="sxs-lookup"><span data-stu-id="7caa9-129">Use <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> to configure a common shared app name (`SharedCookieApp` in the following examples).</span></span> <span data-ttu-id="7caa9-130">Aby uzyskać więcej informacji, zobacz <xref:security/data-protection/configuration/overview>.</span><span class="sxs-lookup"><span data-stu-id="7caa9-130">For more information, see <xref:security/data-protection/configuration/overview>.</span></span>
* <span data-ttu-id="7caa9-131">Użyj <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> metodę rozszerzenia, aby skonfigurować usługi ochrony danych plików cookie.</span><span class="sxs-lookup"><span data-stu-id="7caa9-131">Use the <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> extension method to set up the data protection service for cookies.</span></span>
* <span data-ttu-id="7caa9-132">W poniższym przykładzie ustawiono typ uwierzytelniania `Identity.Application` domyślnie.</span><span class="sxs-lookup"><span data-stu-id="7caa9-132">In the following example, the authentication type is set to `Identity.Application` by default.</span></span>

<span data-ttu-id="7caa9-133">W `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7caa9-133">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

<span data-ttu-id="7caa9-134">Korzystając z plików cookie bezpośrednio, bez tożsamości platformy ASP.NET Core, skonfiguruj ochronę danych i uwierzytelniania w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7caa9-134">When using cookies directly without ASP.NET Core Identity, configure data protection and authentication in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7caa9-135">W poniższym przykładzie ustawiono typ uwierzytelniania `Identity.Application`:</span><span class="sxs-lookup"><span data-stu-id="7caa9-135">In the following example, the authentication type is set to `Identity.Application`:</span></span>

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.AddAuthentication("Identity.Application")
    .AddCookie("Identity.Application", options =>
    {
        options.Cookie.Name = ".AspNet.SharedCookie";
    });
```

<span data-ttu-id="7caa9-136">Hostowania aplikacji, które udostępnianie plików cookie między domenami podrzędnymi, należy określić w typowych domeny [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) właściwości.</span><span class="sxs-lookup"><span data-stu-id="7caa9-136">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) property.</span></span> <span data-ttu-id="7caa9-137">Udostępnianie plików cookie między aplikacjami na `contoso.com`, takich jak `first_subdomain.contoso.com` i `second_subdomain.contoso.com`, określ `Cookie.Domain` jako `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="7caa9-137">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a><span data-ttu-id="7caa9-138">Szyfruj klucze ochrony danych magazynowanych</span><span class="sxs-lookup"><span data-stu-id="7caa9-138">Encrypt data protection keys at rest</span></span>

<span data-ttu-id="7caa9-139">W przypadku wdrożeń produkcyjnych należy skonfigurować `DataProtectionProvider` do szyfrowania kluczy w stanie spoczynku przy użyciu interfejsu DPAPI lub X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="7caa9-139">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="7caa9-140">Aby uzyskać więcej informacji, zobacz <xref:security/data-protection/implementation/key-encryption-at-rest>.</span><span class="sxs-lookup"><span data-stu-id="7caa9-140">For more information, see <xref:security/data-protection/implementation/key-encryption-at-rest>.</span></span> <span data-ttu-id="7caa9-141">W poniższym przykładzie przedstawiono odcisk palca certyfikatu do <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span><span class="sxs-lookup"><span data-stu-id="7caa9-141">In the following example, a certificate thumbprint is provided to <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span></span>

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="7caa9-142">Udostępnianie plików cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7caa9-142">Share authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="7caa9-143">Aplikacje ASP.NET 4.x, korzystających z oprogramowania pośredniczącego uwierzytelniania pliku Cookie Katana można skonfigurować do generowania plików cookie uwierzytelniania, które są zgodne z oprogramowaniem pośredniczącym uwierzytelniania platformy ASP.NET Core pliku Cookie.</span><span class="sxs-lookup"><span data-stu-id="7caa9-143">ASP.NET 4.x apps that use Katana Cookie Authentication Middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core Cookie Authentication Middleware.</span></span> <span data-ttu-id="7caa9-144">Dzięki temu uaktualniania dużej witryny poszczególnych aplikacji w kilku krokach, zapewniając bezproblemowe środowisko logowania jednokrotnego w lokacji.</span><span class="sxs-lookup"><span data-stu-id="7caa9-144">This allows upgrading a large site's individual apps in several steps while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="7caa9-145">Jeśli aplikacja używa oprogramowania pośredniczącego uwierzytelniania pliku Cookie Katana, wywołuje `UseCookieAuthentication` w projekcie *Startup.Auth.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="7caa9-145">When an app uses Katana Cookie Authentication Middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="7caa9-146">Projekty aplikacji sieci web programu ASP.NET 4.x utworzonych za pomocą programu Visual Studio 2013, a później użyć oprogramowanie pośredniczące uwierzytelniania plików Cookie Katana domyślnie.</span><span class="sxs-lookup"><span data-stu-id="7caa9-146">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana Cookie Authentication Middleware by default.</span></span> <span data-ttu-id="7caa9-147">Mimo że `UseCookieAuthentication` jest obsługiwany w przypadku aplikacji platformy ASP.NET Core i wywoływania `UseCookieAuthentication` w programie ASP.NET 4.x aplikacji, która używa oprogramowania pośredniczącego uwierzytelniania pliku Cookie Katana jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="7caa9-147">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana Cookie Authentication Middleware is valid.</span></span>

<span data-ttu-id="7caa9-148">Aplikacja platformy ASP.NET 4.x musi być przeznaczony dla .NET Framework 4.5.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="7caa9-148">An ASP.NET 4.x app must target .NET Framework 4.5.1 or later.</span></span> <span data-ttu-id="7caa9-149">W przeciwnym razie niezbędne pakiety NuGet uniemożliwić instalację.</span><span class="sxs-lookup"><span data-stu-id="7caa9-149">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="7caa9-150">Aby udostępnić pliki cookie uwierzytelniania między aplikację ASP.NET 4.x i aplikacji ASP.NET Core, konfigurowania aplikacji ASP.NET Core zgodnie z [udostępnianie plików cookie uwierzytelniania między aplikacjami ASP.NET Core](#share-authentication-cookies-among-aspnet-core-apps) sekcji, a następnie skonfigurować aplikację ASP.NET 4.x, jako poniżej.</span><span class="sxs-lookup"><span data-stu-id="7caa9-150">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated in the [Share authentication cookies among ASP.NET Core apps](#share-authentication-cookies-among-aspnet-core-apps) section, then configure the ASP.NET 4.x app as follows.</span></span>

<span data-ttu-id="7caa9-151">Upewnij się, że pakiety aplikacji zostały zaktualizowane do najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="7caa9-151">Confirm that the app's packages are updated to the latest releases.</span></span> <span data-ttu-id="7caa9-152">Zainstaluj [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) pakietu do każdej aplikacji 4.x ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7caa9-152">Install the [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) package into each ASP.NET 4.x app.</span></span>

<span data-ttu-id="7caa9-153">Zlokalizuj i zmodyfikuj wywołanie `UseCookieAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="7caa9-153">Locate and modify the call to `UseCookieAuthentication`:</span></span>

* <span data-ttu-id="7caa9-154">Zmień nazwę pliku cookie, aby dopasować nazwę używaną przez oprogramowanie pośredniczące programu ASP.NET Core pliku Cookie uwierzytelniania (`.AspNet.SharedCookie` w przykładzie).</span><span class="sxs-lookup"><span data-stu-id="7caa9-154">Change the cookie name to match the name used by the ASP.NET Core Cookie Authentication Middleware (`.AspNet.SharedCookie` in the example).</span></span>
* <span data-ttu-id="7caa9-155">W poniższym przykładzie ustawiono typ uwierzytelniania `Identity.Application`.</span><span class="sxs-lookup"><span data-stu-id="7caa9-155">In the following example, the authentication type is set to `Identity.Application`.</span></span>
* <span data-ttu-id="7caa9-156">Podaj wystąpienia `DataProtectionProvider` inicjowany do wspólnej lokalizacji magazynu kluczy ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="7caa9-156">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span>
* <span data-ttu-id="7caa9-157">Upewnij się, że nazwa aplikacji jest ustawiona na nazwę pospolitą aplikacji używane przez wszystkie aplikacje, których udostępnianie plików cookie uwierzytelniania (`SharedCookieApp` w przykładzie).</span><span class="sxs-lookup"><span data-stu-id="7caa9-157">Confirm that the app name is set to the common app name used by all apps that share authentication cookies (`SharedCookieApp` in the example).</span></span>

<span data-ttu-id="7caa9-158">Jeśli to ustawienie nie zostanie `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` i `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`ustaw <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> do oświadczeń, która odróżnia unikatowych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="7caa9-158">If not setting `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` and `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, set <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> to a claim that distinguishes unique users.</span></span>

<span data-ttu-id="7caa9-159">*App_Start/Startup.auth.cs*:</span><span class="sxs-lookup"><span data-stu-id="7caa9-159">*App_Start/Startup.Auth.cs*:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = "Identity.Application",
    CookieName = ".AspNet.SharedCookie",
    LoginPath = new PathString("/Account/Login"),
    Provider = new CookieAuthenticationProvider
    {
        OnValidateIdentity =
            SecurityStampValidator
                .OnValidateIdentity<ApplicationUserManager, ApplicationUser>(
                    validateInterval: TimeSpan.FromMinutes(30),
                    regenerateIdentity: (manager, user) =>
                        user.GenerateUserIdentityAsync(manager))
    },
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create({PATH TO COMMON KEY RING FOLDER},
                (builder) => { builder.SetApplicationName("SharedCookieApp"); })
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies." +
                    "CookieAuthenticationMiddleware",
                "Identity.Application",
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});

System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier =
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name";
```

<span data-ttu-id="7caa9-160">Podczas generowania tożsamości użytkownika, typ uwierzytelniania (`Identity.Application`) musi być zgodny z typem zdefiniowanym w `AuthenticationType` zestawu przy użyciu `UseCookieAuthentication` w *App_Start/Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="7caa9-160">When generating a user identity, the authentication type (`Identity.Application`) must match the type defined in `AuthenticationType` set with `UseCookieAuthentication` in *App_Start/Startup.Auth.cs*.</span></span>

<span data-ttu-id="7caa9-161">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="7caa9-161">*Models/IdentityModels.cs*:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public async Task<ClaimsIdentity> GenerateUserIdentityAsync(
        UserManager<ApplicationUser> manager)
    {
        // The authenticationType must match the one defined in 
        // CookieAuthenticationOptions.AuthenticationType
        var userIdentity = 
            await manager.CreateIdentityAsync(this, "Identity.Application");

        // Add custom user claims here

        return userIdentity;
    }
}
```

## <a name="use-a-common-user-database"></a><span data-ttu-id="7caa9-162">Użyj wspólnej bazy danych użytkownika</span><span class="sxs-lookup"><span data-stu-id="7caa9-162">Use a common user database</span></span>

<span data-ttu-id="7caa9-163">Gdy aplikacje używają tej samej tożsamości schematu (wersja tej samej tożsamości), upewnij się, że system tożsamości dla każdej aplikacji jest wskazywany w tej samej bazy danych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7caa9-163">When apps use the same Identity schema (same version of Identity), confirm that the Identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="7caa9-164">W przeciwnym razie systemu tożsamości generuje błędy w czasie wykonywania, gdy podejmuje próbę dopasowania informacje zawarte w pliku cookie uwierzytelniania względem informacji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7caa9-164">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

<span data-ttu-id="7caa9-165">Gdy schematu tożsamości różni się między aplikacjami, zwykle ponieważ aplikacje korzystają z różnych wersji tożsamości, udostępnianie wspólną bazę danych na podstawie najnowszej wersji tożsamości nie jest możliwe bez konieczności ponownego mapowania i dodawanie kolumn w schematach tożsamości innej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7caa9-165">When the Identity schema is different among apps, usually because apps are using different Identity versions, sharing a common database based on the latest version of Identity isn't possible without remapping and adding columns in other app's Identity schemas.</span></span> <span data-ttu-id="7caa9-166">Jest często bardziej efektywne uaktualnienia innych aplikacji, aby korzystać z najnowszej wersji tożsamości, tak aby wspólnej bazy danych mogą być współużytkowane przez aplikacje.</span><span class="sxs-lookup"><span data-stu-id="7caa9-166">It's often more efficient to upgrade the other apps to use the latest Identity version so that a common database can be shared by the apps.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7caa9-167">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7caa9-167">Additional resources</span></span>

* <xref:host-and-deploy/web-farm>
