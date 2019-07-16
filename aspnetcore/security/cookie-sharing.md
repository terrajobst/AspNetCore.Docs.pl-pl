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
# <a name="share-authentication-cookies-among-aspnet-apps"></a>Udostępnianie plików cookie uwierzytelniania między aplikacjami ASP.NET

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Luke Latham](https://github.com/guardrex)

Witryny sieci Web często składają się z poszczególnych aplikacji web apps pracujących razem. Aby zapewnić środowisko logowania jednokrotnego (SSO), aplikacje sieci web w obrębie lokacji muszą współużytkować pliki cookie uwierzytelniania. Aby zapewnić obsługę tego scenariusza, stosu ochrony danych umożliwia udostępnianie Katana uwierzytelniania plików cookie i biletów uwierzytelniania pliku cookie platformy ASP.NET Core.

W przykładach, wykonaj następujące czynności:

* Nazwa pliku cookie uwierzytelniania jest równa wartości typowych `.AspNet.SharedCookie`.
* `AuthenticationType` Ustawiono `Identity.Application` jawnie lub domyślnie.
* Nazwa pospolita aplikacji służy do włączania systemu ochrony danych udostępnić klucze ochrony danych (`SharedCookieApp`).
* `Identity.Application` jest używana jako schematu uwierzytelniania. Niezależnie od schematu jest używany, musi być stosowane konsekwentnie *wewnątrz i pomiędzy* aplikacji udostępnionych plików cookie, jako domyślny schemat lub przez jawne ustawienie go. Schemat jest używany podczas szyfrowania i odszyfrowywania plików cookie, dzięki czemu spójny schemat musi być używana w różnych aplikacjach.
* Często [klucz ochrony danych](xref:security/data-protection/implementation/key-management) lokalizacja magazynu jest używana.
  * W aplikacji platformy ASP.NET Core <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> służy do ustawiania lokalizacji magazynu kluczy.
  * W aplikacjach .NET Framework, oprogramowanie pośredniczące uwierzytelniania plików Cookie korzysta z implementacją <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>. `DataProtectionProvider` udostępnia usługi ochrony danych do szyfrowania i odszyfrowywania danych ładunku plików cookie uwierzytelniania. `DataProtectionProvider` Wystąpienie jest izolowane od system ochrony danych, które są używane przez inne części aplikacji. [DataProtectionProvider.Create (System.IO.DirectoryInfo, Akcja\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) akceptuje <xref:System.IO.DirectoryInfo> Określ lokalizację do przechowywania kluczy ochrony danych.
* `DataProtectionProvider` wymaga [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pakietu NuGet:
  * W aplikacjach ASP.NET Core 2.x odwołania [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).
  * W aplikacjach .NET Framework, należy dodać odwołanie do pakietu [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).
* <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> Ustawia nazwę pospolitą aplikacji.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Udostępnianie plików cookie uwierzytelniania między aplikacjami ASP.NET Core

W przypadku używania tożsamości platformy ASP.NET Core:

* Klucze ochrony danych i nazwę aplikacji musi być współużytkowany przez aplikacje. Wspólnej lokalizacji magazynu kluczy jest dostarczany w celu <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> metody w poniższych przykładach. Użyj <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> skonfigurować nazwę pospolitą udostępnionej aplikacji (`SharedCookieApp` w poniższych przykładach). Aby uzyskać więcej informacji, zobacz <xref:security/data-protection/configuration/overview>.
* Użyj <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> metodę rozszerzenia, aby skonfigurować usługi ochrony danych plików cookie.
* W poniższym przykładzie ustawiono typ uwierzytelniania `Identity.Application` domyślnie.

W `Startup.ConfigureServices`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

Korzystając z plików cookie bezpośrednio, bez tożsamości platformy ASP.NET Core, skonfiguruj ochronę danych i uwierzytelniania w `Startup.ConfigureServices`. W poniższym przykładzie ustawiono typ uwierzytelniania `Identity.Application`:

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

Hostowania aplikacji, które udostępnianie plików cookie między domenami podrzędnymi, należy określić w typowych domeny [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) właściwości. Udostępnianie plików cookie między aplikacjami na `contoso.com`, takich jak `first_subdomain.contoso.com` i `second_subdomain.contoso.com`, określ `Cookie.Domain` jako `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a>Szyfruj klucze ochrony danych magazynowanych

W przypadku wdrożeń produkcyjnych należy skonfigurować `DataProtectionProvider` do szyfrowania kluczy w stanie spoczynku przy użyciu interfejsu DPAPI lub X509Certificate. Aby uzyskać więcej informacji, zobacz <xref:security/data-protection/implementation/key-encryption-at-rest>. W poniższym przykładzie przedstawiono odcisk palca certyfikatu do <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Udostępnianie plików cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core

Aplikacje ASP.NET 4.x, korzystających z oprogramowania pośredniczącego uwierzytelniania pliku Cookie Katana można skonfigurować do generowania plików cookie uwierzytelniania, które są zgodne z oprogramowaniem pośredniczącym uwierzytelniania platformy ASP.NET Core pliku Cookie. Dzięki temu uaktualniania dużej witryny poszczególnych aplikacji w kilku krokach, zapewniając bezproblemowe środowisko logowania jednokrotnego w lokacji.

Jeśli aplikacja używa oprogramowania pośredniczącego uwierzytelniania pliku Cookie Katana, wywołuje `UseCookieAuthentication` w projekcie *Startup.Auth.cs* pliku. Projekty aplikacji sieci web programu ASP.NET 4.x utworzonych za pomocą programu Visual Studio 2013, a później użyć oprogramowanie pośredniczące uwierzytelniania plików Cookie Katana domyślnie. Mimo że `UseCookieAuthentication` jest obsługiwany w przypadku aplikacji platformy ASP.NET Core i wywoływania `UseCookieAuthentication` w programie ASP.NET 4.x aplikacji, która używa oprogramowania pośredniczącego uwierzytelniania pliku Cookie Katana jest prawidłowy.

Aplikacja platformy ASP.NET 4.x musi być przeznaczony dla .NET Framework 4.5.1 lub nowszej. W przeciwnym razie niezbędne pakiety NuGet uniemożliwić instalację.

Aby udostępnić pliki cookie uwierzytelniania między aplikację ASP.NET 4.x i aplikacji ASP.NET Core, konfigurowania aplikacji ASP.NET Core zgodnie z [udostępnianie plików cookie uwierzytelniania między aplikacjami ASP.NET Core](#share-authentication-cookies-among-aspnet-core-apps) sekcji, a następnie skonfigurować aplikację ASP.NET 4.x, jako poniżej.

Upewnij się, że pakiety aplikacji zostały zaktualizowane do najnowszej wersji. Zainstaluj [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) pakietu do każdej aplikacji 4.x ASP.NET.

Zlokalizuj i zmodyfikuj wywołanie `UseCookieAuthentication`:

* Zmień nazwę pliku cookie, aby dopasować nazwę używaną przez oprogramowanie pośredniczące programu ASP.NET Core pliku Cookie uwierzytelniania (`.AspNet.SharedCookie` w przykładzie).
* W poniższym przykładzie ustawiono typ uwierzytelniania `Identity.Application`.
* Podaj wystąpienia `DataProtectionProvider` inicjowany do wspólnej lokalizacji magazynu kluczy ochrony danych.
* Upewnij się, że nazwa aplikacji jest ustawiona na nazwę pospolitą aplikacji używane przez wszystkie aplikacje, których udostępnianie plików cookie uwierzytelniania (`SharedCookieApp` w przykładzie).

Jeśli to ustawienie nie zostanie `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` i `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`ustaw <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> do oświadczeń, która odróżnia unikatowych użytkowników.

*App_Start/Startup.auth.cs*:

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

Podczas generowania tożsamości użytkownika, typ uwierzytelniania (`Identity.Application`) musi być zgodny z typem zdefiniowanym w `AuthenticationType` zestawu przy użyciu `UseCookieAuthentication` w *App_Start/Startup.Auth.cs*.

*Models/IdentityModels.cs*:

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

## <a name="use-a-common-user-database"></a>Użyj wspólnej bazy danych użytkownika

Gdy aplikacje używają tej samej tożsamości schematu (wersja tej samej tożsamości), upewnij się, że system tożsamości dla każdej aplikacji jest wskazywany w tej samej bazy danych użytkownika. W przeciwnym razie systemu tożsamości generuje błędy w czasie wykonywania, gdy podejmuje próbę dopasowania informacje zawarte w pliku cookie uwierzytelniania względem informacji w bazie danych.

Gdy schematu tożsamości różni się między aplikacjami, zwykle ponieważ aplikacje korzystają z różnych wersji tożsamości, udostępnianie wspólną bazę danych na podstawie najnowszej wersji tożsamości nie jest możliwe bez konieczności ponownego mapowania i dodawanie kolumn w schematach tożsamości innej aplikacji. Jest często bardziej efektywne uaktualnienia innych aplikacji, aby korzystać z najnowszej wersji tożsamości, tak aby wspólnej bazy danych mogą być współużytkowane przez aplikacje.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:host-and-deploy/web-farm>
