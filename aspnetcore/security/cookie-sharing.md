---
title: Udostępnianie plików cookie między aplikacji ASP.NET i ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak udostępniać pliki cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cookie-sharing
ms.openlocfilehash: c22db501a2689feb8c16649eba4866e1190361a4
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613021"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="c64b7-103">Udostępnianie plików cookie między aplikacji ASP.NET i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c64b7-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="c64b7-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c64b7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c64b7-105">Witryny sieci Web często składają się z poszczególnych aplikacji web apps pracujących razem.</span><span class="sxs-lookup"><span data-stu-id="c64b7-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="c64b7-106">Aby zapewnić środowisko rejestracji jednokrotnej (SSO), aplikacje sieci web w obrębie lokacji muszą współdzielić plików cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="c64b7-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="c64b7-107">Z tym scenariuszem stosu ochrony danych umożliwia udostępnianie Katana pliku cookie uwierzytelniania i biletów uwierzytelniania pliku cookie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c64b7-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="c64b7-108">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c64b7-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c64b7-109">Przykładową udostępnianie w trzech aplikacji, które korzystają z plików cookie uwierzytelniania pliku cookie:</span><span class="sxs-lookup"><span data-stu-id="c64b7-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="c64b7-110">Aplikacja ASP.NET Core 2.0 Razor strony bez użycia [ASP.NET Core Identity](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="c64b7-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="c64b7-111">Platformy ASP.NET Core 2.0 aplikacji MVC za pomocą tożsamości platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c64b7-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="c64b7-112">Struktura ASP.NET 4.6.1 aplikacji MVC za pomocą tożsamości platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c64b7-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="c64b7-113">W przykładach, które należy wykonać:</span><span class="sxs-lookup"><span data-stu-id="c64b7-113">In the examples that follow:</span></span>

* <span data-ttu-id="c64b7-114">Nazwa pliku cookie uwierzytelniania ma ustawioną wartość wspólnej `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="c64b7-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="c64b7-115">`AuthenticationType` Ustawiono `Identity.Application` jawnie lub domyślnie.</span><span class="sxs-lookup"><span data-stu-id="c64b7-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="c64b7-116">Służy nazwą pospolitą aplikacji aby włączyć system ochrony danych udostępnić klucze ochrony danych (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="c64b7-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="c64b7-117">`Identity.Application` jest używany jako schemat uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="c64b7-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="c64b7-118">Niezależnie od schematu jest używana, należy go używać spójnie *w obrębie oraz* aplikacji udostępnionych plików cookie jako domyślny schemat lub przez jawne ustawienie dla niego.</span><span class="sxs-lookup"><span data-stu-id="c64b7-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="c64b7-119">Schemat jest używany podczas szyfrowania i odszyfrowywania plików cookie, więc spójny schemat musi być używany w aplikacjach.</span><span class="sxs-lookup"><span data-stu-id="c64b7-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="c64b7-120">Popularne [klucz ochrony danych](xref:security/data-protection/implementation/key-management) lokalizacja magazynu jest używana.</span><span class="sxs-lookup"><span data-stu-id="c64b7-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="c64b7-121">Przykładowa aplikacja korzysta z folderem o nazwie *zestaw kluczy* w katalogu głównym rozwiązania do przechowywania kluczy ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="c64b7-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="c64b7-122">W aplikacji platformy ASP.NET Core [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) służy do ustawiania lokalizacji magazynu kluczy.</span><span class="sxs-lookup"><span data-stu-id="c64b7-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="c64b7-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) służy do konfigurowania nazwą pospolitą udostępnionej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c64b7-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="c64b7-124">W aplikacji .NET Framework, oprogramowanie pośredniczące uwierzytelniania plików cookie używa implementacja [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="c64b7-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="c64b7-125">`DataProtectionProvider` zapewnia usługi ochrony danych na potrzeby szyfrowania i odszyfrowywania danych ładunku plików cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="c64b7-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="c64b7-126">`DataProtectionProvider` Wystąpienie jest odizolowana od systemu ochrony danych, które są używane przez inne części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c64b7-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="c64b7-127">[DataProtectionProvider.Create (System.IO.DirectoryInfo, Akcja\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) akceptuje [DirectoryInfo](/dotnet/api/system.io.directoryinfo) Określ lokalizację do przechowywania klucza ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="c64b7-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="c64b7-128">Przykładowa aplikacja zawiera ścieżkę *zestaw kluczy* folder `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="c64b7-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="c64b7-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) wspólnej nazwy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c64b7-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="c64b7-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) wymaga [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="c64b7-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="c64b7-131">Odwoływać się do uzyskania tego pakietu dla platformy ASP.NET Core 2.1 i nowsze aplikacje, [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c64b7-131">To obtain this package for ASP.NET Core 2.1 and later apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="c64b7-132">Gdy docelowy program .NET Framework, Dodaj odwołanie do pakietu `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="c64b7-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="c64b7-133">Udostępnianie plików cookie uwierzytelniania między aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c64b7-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="c64b7-134">Podczas używania ASP.NET Core Identity:</span><span class="sxs-lookup"><span data-stu-id="c64b7-134">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c64b7-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c64b7-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="c64b7-136">W `ConfigureServices` metody, użyj [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) — metoda rozszerzenia, aby skonfigurować usługę ochrony danych plików cookie.</span><span class="sxs-lookup"><span data-stu-id="c64b7-136">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="c64b7-137">Klucze ochrony danych i nazwa aplikacji musi być współużytkowane przez aplikacje.</span><span class="sxs-lookup"><span data-stu-id="c64b7-137">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="c64b7-138">W przykładowych aplikacji `GetKeyRingDirInfo` Zwraca lokalizację przechowywania klucza wspólnego [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) metody.</span><span class="sxs-lookup"><span data-stu-id="c64b7-138">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="c64b7-139">Użyj [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) Aby skonfigurować nazwę pospolitą udostępnionej aplikacji (`SharedCookieApp` w próbce).</span><span class="sxs-lookup"><span data-stu-id="c64b7-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="c64b7-140">Aby uzyskać więcej informacji, zobacz [konfigurowania ochrony danych](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="c64b7-140">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="c64b7-141">Zobacz *CookieAuthWithIdentity.Core* projektu w [przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c64b7-141">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c64b7-142">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c64b7-142">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="c64b7-143">W `Configure` metody, użyj [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) do skonfigurowania:</span><span class="sxs-lookup"><span data-stu-id="c64b7-143">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="c64b7-144">Usługa ochrony danych plików cookie.</span><span class="sxs-lookup"><span data-stu-id="c64b7-144">The data protection service for cookies.</span></span>
* <span data-ttu-id="c64b7-145">`AuthenticationScheme` Odpowiadające ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="c64b7-145">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

<span data-ttu-id="c64b7-146">Podczas używania plików cookie bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="c64b7-146">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c64b7-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c64b7-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="c64b7-148">Klucze ochrony danych i nazwa aplikacji musi być współużytkowane przez aplikacje.</span><span class="sxs-lookup"><span data-stu-id="c64b7-148">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="c64b7-149">W przykładowych aplikacji `GetKeyRingDirInfo` Zwraca lokalizację przechowywania klucza wspólnego [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) metody.</span><span class="sxs-lookup"><span data-stu-id="c64b7-149">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="c64b7-150">Użyj [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) Aby skonfigurować nazwę pospolitą udostępnionej aplikacji (`SharedCookieApp` w próbce).</span><span class="sxs-lookup"><span data-stu-id="c64b7-150">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="c64b7-151">Aby uzyskać więcej informacji, zobacz [konfigurowania ochrony danych](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="c64b7-151">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span> 

<span data-ttu-id="c64b7-152">Zobacz *CookieAuth.Core* projektu w [przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c64b7-152">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c64b7-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c64b7-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="c64b7-154">Szyfrowanie kluczy ochrony danych magazynowanych</span><span class="sxs-lookup"><span data-stu-id="c64b7-154">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="c64b7-155">W przypadku wdrożeń produkcyjnych, skonfigurować `DataProtectionProvider` do szyfrowania kluczy magazynowane DPAPI lub X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="c64b7-155">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="c64b7-156">Zobacz [klucza szyfrowania w Rest](xref:security/data-protection/implementation/key-encryption-at-rest) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="c64b7-156">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c64b7-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c64b7-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c64b7-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c64b7-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="c64b7-159">Udostępnianie plików cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c64b7-159">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="c64b7-160">Aplikacje ASP.NET 4.x, korzystających z oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana można skonfigurować do generowania plików cookie uwierzytelniania, które są zgodne z oprogramowaniem pośredniczącym uwierzytelniania pliku cookie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c64b7-160">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="c64b7-161">Dzięki temu uaktualniania aplikacji poszczególnych dużej witryny wyrywkowo zapewniając smooth jednokrotnej całej witryny.</span><span class="sxs-lookup"><span data-stu-id="c64b7-161">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="c64b7-162">Jeśli aplikacja używa oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana, wywołuje `UseCookieAuthentication` w projekcie *Startup.Auth.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="c64b7-162">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="c64b7-163">Projekty aplikacji sieci web programu ASP.NET 4.x utworzone za pomocą programu Visual Studio 2013, a następnie użyć oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana domyślnie.</span><span class="sxs-lookup"><span data-stu-id="c64b7-163">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span> <span data-ttu-id="c64b7-164">Mimo że `UseCookieAuthentication` jest obsługiwany w przypadku aplikacji platformy ASP.NET Core wywoływania `UseCookieAuthentication` w aplikacji ASP.NET 4.x, która używa Katana oprogramowania pośredniczącego uwierzytelniania pliku cookie jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="c64b7-164">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana cookie authentication middleware is valid.</span></span>

<span data-ttu-id="c64b7-165">Aplikacja ASP.NET 4.x musi wskazywać na .NET Framework 4.5.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="c64b7-165">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="c64b7-166">W przeciwnym razie niezbędne pakiety NuGet uniemożliwić instalację.</span><span class="sxs-lookup"><span data-stu-id="c64b7-166">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="c64b7-167">Aby udostępnić pliki cookie uwierzytelniania między aplikację ASP.NET 4.x i aplikacji platformy ASP.NET Core, konfigurowania aplikacji platformy ASP.NET Core, jak wspomniano powyżej, a następnie konfigurowania aplikacji ASP.NET 4.x, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="c64b7-167">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x app by following these steps:</span></span>

1. <span data-ttu-id="c64b7-168">Zainstaluj pakiet [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) do każdej ASP.NET 4.x aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c64b7-168">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="c64b7-169">W *Startup.Auth.cs*, zlokalizuj wywołanie `UseCookieAuthentication` i zmodyfikuj go w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="c64b7-169">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="c64b7-170">Zmień nazwę pliku cookie do dopasowania nazwę używaną przez oprogramowanie pośredniczące uwierzytelniania plików cookie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c64b7-170">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="c64b7-171">Podaj wystąpienia `DataProtectionProvider` zainicjowany do wspólnej lokalizacji magazynu kluczy ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="c64b7-171">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="c64b7-172">Upewnij się, że nazwa aplikacji jest ustawiona na nazwę pospolitą aplikacji używane przez wszystkie aplikacje, które współużytkują plików cookie, `SharedCookieApp` w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c64b7-172">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="c64b7-173">Zobacz *CookieAuthWithIdentity.NETFramework* projektu w [przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c64b7-173">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="c64b7-174">Podczas generowania tożsamości użytkownika, typ uwierzytelniania musi być zgodny z typem zdefiniowanym w `AuthenticationType` ustawiony za pomocą `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="c64b7-174">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="c64b7-175">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="c64b7-175">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a><span data-ttu-id="c64b7-176">Użyj wspólną bazę danych użytkownika</span><span class="sxs-lookup"><span data-stu-id="c64b7-176">Use a common user database</span></span>

<span data-ttu-id="c64b7-177">Upewnij się, że w tej samej bazy danych użytkownika jest wskazywana systemu tożsamości dla każdej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c64b7-177">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="c64b7-178">W przeciwnym razie systemu tożsamości tworzy błędy w czasie wykonywania, podczas próby zgodny z informacjami w pliku cookie uwierzytelniania względem informacji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c64b7-178">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
