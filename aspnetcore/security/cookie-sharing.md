---
title: Udostępnianie plików cookie między aplikacjami ASP.NET i ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak udostępnianie plików cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
uid: security/cookie-sharing
ms.openlocfilehash: ed3496db3f7a63a704f0e57faef6b2f6c085a8bd
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228601"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="2a684-103">Udostępnianie plików cookie między aplikacjami ASP.NET i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a684-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="2a684-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2a684-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2a684-105">Witryny sieci Web często składają się z poszczególnych aplikacji web apps pracujących razem.</span><span class="sxs-lookup"><span data-stu-id="2a684-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="2a684-106">Aby zapewnić środowisko logowania jednokrotnego (SSO), aplikacje sieci web w obrębie lokacji muszą współużytkować pliki cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="2a684-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="2a684-107">Aby zapewnić obsługę tego scenariusza, stosu ochrony danych umożliwia udostępnianie Katana uwierzytelniania plików cookie i biletów uwierzytelniania pliku cookie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2a684-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="2a684-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2a684-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2a684-109">Przykład ilustruje udostępnianie między trzy aplikacje, które korzystają z plików cookie uwierzytelniania plików cookie:</span><span class="sxs-lookup"><span data-stu-id="2a684-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="2a684-110">ASP.NET Core 2.0 Razor strony aplikacji bez korzystania z [tożsamości platformy ASP.NET Core](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="2a684-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="2a684-111">Aplikacja MVC platformy ASP.NET Core 2.0 przy użyciu tożsamości platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a684-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="2a684-112">Aplikacja MVC platformy ASP.NET Framework 4.6.1 w produkcie ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="2a684-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="2a684-113">W przykładach, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="2a684-113">In the examples that follow:</span></span>

* <span data-ttu-id="2a684-114">Nazwa pliku cookie uwierzytelniania jest równa wartości typowych `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="2a684-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="2a684-115">`AuthenticationType` Ustawiono `Identity.Application` jawnie lub domyślnie.</span><span class="sxs-lookup"><span data-stu-id="2a684-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="2a684-116">Nazwa pospolita aplikacji służy do włączania systemu ochrony danych udostępnić klucze ochrony danych (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="2a684-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="2a684-117">`Identity.Application` jest używana jako schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="2a684-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="2a684-118">Niezależnie od schematu jest używany, musi być stosowane konsekwentnie *wewnątrz i pomiędzy* aplikacji udostępnionych plików cookie, jako domyślny schemat lub przez jawne ustawienie go.</span><span class="sxs-lookup"><span data-stu-id="2a684-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="2a684-119">Schemat jest używany podczas szyfrowania i odszyfrowywania plików cookie, dzięki czemu spójny schemat musi być używana w różnych aplikacjach.</span><span class="sxs-lookup"><span data-stu-id="2a684-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="2a684-120">Często [klucz ochrony danych](xref:security/data-protection/implementation/key-management) lokalizacja magazynu jest używana.</span><span class="sxs-lookup"><span data-stu-id="2a684-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="2a684-121">Przykładowa aplikacja korzysta z folderem o nazwie *pęku kluczy* w katalogu głównym rozwiązanie do przechowywania kluczy ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="2a684-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="2a684-122">W aplikacji platformy ASP.NET Core [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) służy do ustawiania lokalizacji magazynu kluczy.</span><span class="sxs-lookup"><span data-stu-id="2a684-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="2a684-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) służy do konfigurowania nazwą pospolitą udostępnionej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2a684-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="2a684-124">W aplikacji .NET Framework, oprogramowanie pośredniczące uwierzytelniania plików cookie korzysta z implementacją [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="2a684-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="2a684-125">`DataProtectionProvider` udostępnia usługi ochrony danych do szyfrowania i odszyfrowywania danych ładunku plików cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="2a684-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="2a684-126">`DataProtectionProvider` Wystąpienie jest izolowane od system ochrony danych, które są używane przez inne części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2a684-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="2a684-127">[DataProtectionProvider.Create (System.IO.DirectoryInfo, Akcja\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) akceptuje [DirectoryInfo](/dotnet/api/system.io.directoryinfo) Określ lokalizację do przechowywania kluczy ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="2a684-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="2a684-128">Przykładowa aplikacja udostępnia ścieżkę *pęku kluczy* folder `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="2a684-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="2a684-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) wspólnej nazwy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2a684-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="2a684-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) wymaga [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="2a684-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="2a684-131">Aby uzyskać tego pakietu dla platformy ASP.NET Core 2.1 i nowsze aplikacji, należy odwołać [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2a684-131">To obtain this package for ASP.NET Core 2.1 and later apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="2a684-132">Podczas określania wartości docelowej programu .NET Framework, Dodaj odwołanie do pakietu `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="2a684-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="2a684-133">Udostępnianie plików cookie uwierzytelniania między aplikacjami ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a684-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="2a684-134">W przypadku używania tożsamości platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="2a684-134">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a684-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a684-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="2a684-136">W `ConfigureServices` metody, użyj [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) metodę rozszerzenia, aby skonfigurować usługi ochrony danych plików cookie.</span><span class="sxs-lookup"><span data-stu-id="2a684-136">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="2a684-137">Klucze ochrony danych i nazwę aplikacji musi być współużytkowany przez aplikacje.</span><span class="sxs-lookup"><span data-stu-id="2a684-137">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="2a684-138">W przykładowym aplikacjom oraz `GetKeyRingDirInfo` zwraca wspólnej lokalizacji magazynu kluczy w celu [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) metody.</span><span class="sxs-lookup"><span data-stu-id="2a684-138">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="2a684-139">Użyj [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) skonfigurować nazwę pospolitą udostępnionej aplikacji (`SharedCookieApp` w przykładzie).</span><span class="sxs-lookup"><span data-stu-id="2a684-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="2a684-140">Aby uzyskać więcej informacji, zobacz [konfigurowania ochrony danych](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="2a684-140">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="2a684-141">Hostowania aplikacji, które udostępnianie plików cookie między domenami podrzędnymi, należy określić w typowych domeny [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) właściwości.</span><span class="sxs-lookup"><span data-stu-id="2a684-141">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) property.</span></span> <span data-ttu-id="2a684-142">Udostępnianie plików cookie między aplikacjami na `contoso.com`, takich jak `first_subdomain.contoso.com` i `second_subdomain.contoso.com`, określ `Cookie.Domain` jako `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="2a684-142">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

<span data-ttu-id="2a684-143">Zobacz *CookieAuthWithIdentity.Core* projektu w [przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2a684-143">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a684-144">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a684-144">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="2a684-145">W `Configure` metody, użyj [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) do skonfigurowania:</span><span class="sxs-lookup"><span data-stu-id="2a684-145">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="2a684-146">Usługa ochrony danych plików cookie.</span><span class="sxs-lookup"><span data-stu-id="2a684-146">The data protection service for cookies.</span></span>
* <span data-ttu-id="2a684-147">`AuthenticationScheme` Do dopasowania ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="2a684-147">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

<span data-ttu-id="2a684-148">W przypadku używania plików cookie bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="2a684-148">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a684-149">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a684-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="2a684-150">Klucze ochrony danych i nazwę aplikacji musi być współużytkowany przez aplikacje.</span><span class="sxs-lookup"><span data-stu-id="2a684-150">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="2a684-151">W przykładowym aplikacjom oraz `GetKeyRingDirInfo` zwraca wspólnej lokalizacji magazynu kluczy w celu [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) metody.</span><span class="sxs-lookup"><span data-stu-id="2a684-151">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="2a684-152">Użyj [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) skonfigurować nazwę pospolitą udostępnionej aplikacji (`SharedCookieApp` w przykładzie).</span><span class="sxs-lookup"><span data-stu-id="2a684-152">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="2a684-153">Aby uzyskać więcej informacji, zobacz [konfigurowania ochrony danych](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="2a684-153">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="2a684-154">Hostowania aplikacji, które udostępnianie plików cookie między domenami podrzędnymi, należy określić w typowych domeny [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) właściwości.</span><span class="sxs-lookup"><span data-stu-id="2a684-154">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) property.</span></span> <span data-ttu-id="2a684-155">Udostępnianie plików cookie między aplikacjami na `contoso.com`, takich jak `first_subdomain.contoso.com` i `second_subdomain.contoso.com`, określ `Cookie.Domain` jako `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="2a684-155">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

<span data-ttu-id="2a684-156">Zobacz *CookieAuth.Core* projektu w [przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2a684-156">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a684-157">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a684-157">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="2a684-158">Szyfrowanie kluczy ochrony danych magazynowanych</span><span class="sxs-lookup"><span data-stu-id="2a684-158">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="2a684-159">W przypadku wdrożeń produkcyjnych należy skonfigurować `DataProtectionProvider` do szyfrowania kluczy w stanie spoczynku przy użyciu interfejsu DPAPI lub X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="2a684-159">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="2a684-160">Zobacz [klucza szyfrowania na Rest](xref:security/data-protection/implementation/key-encryption-at-rest) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="2a684-160">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a684-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a684-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a684-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a684-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="2a684-163">Udostępnianie plików cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a684-163">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="2a684-164">Aplikacje platformy ASP.NET 4.x, które używają oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana można skonfigurować do generowania plików cookie uwierzytelniania, które są zgodne z oprogramowaniem pośredniczącym uwierzytelniania pliku cookie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2a684-164">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="2a684-165">Dzięki temu można przyjąć uaktualniania dużej witryny poszczególnych aplikacji, zapewniając bezproblemowe środowisko logowania jednokrotnego w lokacji.</span><span class="sxs-lookup"><span data-stu-id="2a684-165">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="2a684-166">Jeśli aplikacja używa oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana, wywołuje `UseCookieAuthentication` w projekcie *Startup.Auth.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="2a684-166">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="2a684-167">Projekty aplikacji sieci web programu ASP.NET 4.x utworzonych za pomocą programu Visual Studio 2013, a później użyć oprogramowanie pośredniczące uwierzytelniania plików cookie Katana domyślnie.</span><span class="sxs-lookup"><span data-stu-id="2a684-167">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span> <span data-ttu-id="2a684-168">Mimo że `UseCookieAuthentication` jest obsługiwany w przypadku aplikacji platformy ASP.NET Core i wywoływania `UseCookieAuthentication` w aplikacji 4.x ASP.NET, która używa Katana oprogramowania pośredniczącego uwierzytelniania pliku cookie jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="2a684-168">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana cookie authentication middleware is valid.</span></span>

<span data-ttu-id="2a684-169">Aplikacja platformy ASP.NET 4.x musi być przeznaczony dla .NET Framework 4.5.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="2a684-169">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="2a684-170">W przeciwnym razie niezbędne pakiety NuGet uniemożliwić instalację.</span><span class="sxs-lookup"><span data-stu-id="2a684-170">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="2a684-171">Aby udostępnić pliki cookie uwierzytelniania między aplikację ASP.NET 4.x i aplikacji ASP.NET Core, skonfigurować aplikację ASP.NET Core, jak wspomniano powyżej, a następnie konfigurowania aplikacji ASP.NET 4.x, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="2a684-171">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x app by following these steps:</span></span>

1. <span data-ttu-id="2a684-172">Zainstaluj pakiet [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) do każdej aplikacji 4.x ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2a684-172">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="2a684-173">W *Startup.Auth.cs*, Znajdź wywołanie `UseCookieAuthentication` i zmodyfikuj go w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="2a684-173">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="2a684-174">Zmień nazwę pliku cookie, aby dopasować nazwę używaną przez oprogramowanie pośredniczące uwierzytelniania plików cookie programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2a684-174">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="2a684-175">Podaj wystąpienia `DataProtectionProvider` inicjowany do wspólnej lokalizacji magazynu kluczy ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="2a684-175">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="2a684-176">Upewnij się, że nazwa aplikacji jest ustawiona na nazwę pospolitą aplikacji używane przez wszystkie aplikacje, których udostępnianie plików cookie, `SharedCookieApp` w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2a684-176">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="2a684-177">Zobacz *CookieAuthWithIdentity.NETFramework* projektu w [przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2a684-177">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="2a684-178">Podczas generowania tożsamości użytkownika, typ uwierzytelniania musi być zgodny z typem zdefiniowanym w `AuthenticationType` zestawu przy użyciu `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="2a684-178">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="2a684-179">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="2a684-179">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a><span data-ttu-id="2a684-180">Użyj wspólnej bazy danych użytkownika</span><span class="sxs-lookup"><span data-stu-id="2a684-180">Use a common user database</span></span>

<span data-ttu-id="2a684-181">Upewnij się, że system tożsamości dla każdej aplikacji jest wskazywany w tej samej bazy danych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2a684-181">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="2a684-182">W przeciwnym razie systemu tożsamości generuje błędy w czasie wykonywania, gdy podejmuje próbę dopasowania informacje zawarte w pliku cookie uwierzytelniania względem informacji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2a684-182">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2a684-183">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2a684-183">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
