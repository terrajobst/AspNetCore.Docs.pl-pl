---
title: "Udostępnianie plików cookie między aplikacjami"
author: rick-anderson
description: "Dowiedz się, jak udostępniać pliki cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: f026a287601a51e544482b95c5283e8ee960d656
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="sharing-cookies-among-apps"></a>Udostępnianie plików cookie między aplikacjami

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Luke Latham](https://github.com/guardrex)

Witryny sieci Web często składają się z poszczególnych aplikacji web apps pracujących razem. Aby zapewnić środowisko rejestracji jednokrotnej (SSO), aplikacje sieci web w obrębie lokacji muszą współdzielić plików cookie uwierzytelniania. Z tym scenariuszem stosu ochrony danych umożliwia udostępnianie Katana pliku cookie uwierzytelniania i biletów uwierzytelniania pliku cookie platformy ASP.NET Core.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Przykładową udostępnianie w trzech aplikacji, które korzystają z plików cookie uwierzytelniania pliku cookie:

* Aplikacja ASP.NET Core 2.0 Razor strony bez użycia [ASP.NET Core Identity](xref:security/authentication/identity)
* Platformy ASP.NET Core 2.0 aplikacji MVC za pomocą tożsamości platformy ASP.NET Core
* Struktura ASP.NET 4.6.1 aplikacji MVC za pomocą tożsamości platformy ASP.NET

W przykładach, które należy wykonać:

* Nazwa pliku cookie uwierzytelniania ma ustawioną wartość wspólnej `.AspNet.SharedCookie`.
* `AuthenticationType` Ustawiono `Identity.Application` jawnie lub domyślnie.
* [CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) jest używany jako schemat uwierzytelniania. Stała wartość jest rozpoznawany jako `Cookies`.
* Implementacja używa oprogramowania pośredniczącego uwierzytelniania pliku cookie [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider). `DataProtectionProvider` zapewnia usługi ochrony danych na potrzeby szyfrowania i odszyfrowywania danych ładunku plików cookie uwierzytelniania. `DataProtectionProvider` Wystąpienie jest odizolowana od systemu ochrony danych, które są używane przez inne części aplikacji.
  * Popularne [klucz ochrony danych](xref:security/data-protection/implementation/key-management) lokalizacja magazynu jest używana. Przykładowa aplikacja korzysta z folderem o nazwie *zestaw kluczy* w katalogu głównym rozwiązania do przechowywania kluczy ochrony danych.
  * [DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) akceptuje [DirectoryInfo](/dotnet/api/system.io.directoryinfo) do użytku z plików cookie uwierzytelniania. Przykładowa aplikacja zawiera ścieżkę *zestaw kluczy* folder `DirectoryInfo`.
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) wymaga [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pakietu NuGet. Odwoływać się do uzyskania tego pakietu dla platformy ASP.NET Core 2.0 i nowszych aplikacje, [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage. Jeśli celem .NET Framework, Dodaj odwołanie do pakietu `Microsoft.AspNetCore.DataProtection.Extensions`.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Udostępnianie plików cookie uwierzytelniania między aplikacji platformy ASP.NET Core

Podczas używania ASP.NET Core Identity:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

W `ConfigureServices` metody, użyj [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) — metoda rozszerzenia, aby skonfigurować usługę ochrony danych plików cookie.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

Zobacz *CookieAuthWithIdentity.Core* projektu w [przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

W `Configure` metody, użyj [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) do skonfigurowania:

* Usługa ochrony danych plików cookie.
* `AuthenticationScheme` Odpowiadające ASP.NET 4.x.

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

Podczas używania plików cookie bezpośrednio:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

Zobacz *CookieAuth.Core* projektu w [przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a>Szyfrowanie kluczy ochrony danych magazynowanych

W przypadku wdrożeń produkcyjnych, skonfigurować `DataProtectionProvider` do szyfrowania kluczy magazynowane DPAPI lub X509Certificate. Zobacz [klucza szyfrowania w Rest](xref:security/data-protection/implementation/key-encryption-at-rest) Aby uzyskać więcej informacji.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Udostępnianie plików cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core

Aplikacje ASP.NET 4.x, korzystających z oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana można skonfigurować do generowania plików cookie uwierzytelniania, które są zgodne z oprogramowaniem pośredniczącym uwierzytelniania pliku cookie platformy ASP.NET Core. Dzięki temu uaktualniania aplikacji poszczególnych dużej witryny wyrywkowo zapewniając smooth jednokrotnej całej witryny.

> [!TIP]
> Jeśli aplikacja używa oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana, wywołuje `UseCookieAuthentication` w projekcie *Startup.Auth.cs* pliku. Projekty aplikacji sieci web programu ASP.NET 4.x utworzone za pomocą programu Visual Studio 2013, a następnie użyć oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana domyślnie.

> [!NOTE]
> Aplikacja ASP.NET 4.x musi wskazywać na .NET Framework 4.5.1 lub nowszej. W przeciwnym razie niezbędne pakiety NuGet uniemożliwić instalację.

Aby udostępnić pliki cookie uwierzytelniania między ASP.NET 4.x aplikacje i aplikacje platformy ASP.NET Core, konfigurowania aplikacji platformy ASP.NET Core, jak wspomniano powyżej, a następnie skonfiguruj aplikacje ASP.NET 4.x, wykonując poniższe kroki.

1. Zainstaluj pakiet [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) do każdej ASP.NET 4.x aplikacji.

2. W *Startup.Auth.cs*, zlokalizuj wywołanie `UseCookieAuthentication` i zmodyfikuj go w następujący sposób. Zmień nazwę pliku cookie do dopasowania nazwę używaną przez oprogramowanie pośredniczące uwierzytelniania plików cookie platformy ASP.NET Core. Podaj wystąpienia `DataProtectionProvider` zainicjowany do wspólnej lokalizacji magazynu kluczy ochrony danych.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

Zobacz *CookieAuthWithIdentity.NETFramework* projektu w [przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).

Podczas generowania tożsamości użytkownika, typ uwierzytelniania musi być zgodny z typem zdefiniowanym w `AuthenticationType` ustawiony za pomocą `UseCookieAuthentication`.

*Models/IdentityModels.cs*:

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Ustaw `CookieManager` do interop `ChunkingCookieManager` , format segmentu jest zgodny.

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

## <a name="use-a-common-user-database"></a>Użyj wspólną bazę danych użytkownika

Upewnij się, że w tej samej bazy danych użytkownika jest wskazywana systemu tożsamości dla każdej aplikacji. W przeciwnym razie systemu tożsamości tworzy błędy w czasie wykonywania, podczas próby zgodny z informacjami w pliku cookie uwierzytelniania względem informacji w bazie danych.
