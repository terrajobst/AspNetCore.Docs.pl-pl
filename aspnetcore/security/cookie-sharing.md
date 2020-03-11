---
title: Udostępnianie plików cookie uwierzytelniania między aplikacjami ASP.NET
author: rick-anderson
description: Dowiedz się, jak udostępniać pliki cookie uwierzytelniania między ASP.NET. x i aplikacjami ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: security/cookie-sharing
ms.openlocfilehash: 7e29be22717f0b97fc115ac036cc54e333bed4e2
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658173"
---
# <a name="share-authentication-cookies-among-aspnet-apps"></a>Udostępnianie plików cookie uwierzytelniania między aplikacjami ASP.NET

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

Witryny sieci Web często składają się ze współpracujących aplikacji sieci Web. Aby zapewnić obsługę logowania jednokrotnego, aplikacje sieci Web w ramach lokacji muszą udostępniać pliki cookie uwierzytelniania. Aby obsłużyć ten scenariusz, stos ochrony danych umożliwia udostępnianie Katana plików cookie oraz ASP.NET Core biletów uwierzytelniania plików cookie.

W następujących przykładach:

* Nazwa pliku cookie uwierzytelniania jest ustawiona na wspólną wartość `.AspNet.SharedCookie`.
* `AuthenticationType` jest ustawiona na `Identity.Application` jawnie lub domyślnie.
* Nazwa wspólnej aplikacji jest używana w celu umożliwienia systemowi ochrony danych udostępniania kluczy ochrony danych (`SharedCookieApp`).
* `Identity.Application` jest używany jako schemat uwierzytelniania. Niezależnie od tego, jaki schemat jest używany, musi być używany spójnie *w ramach i między* udostępnionymi aplikacjami cookie jako schemat domyślny lub przez jawne ustawienie. Schemat jest używany podczas szyfrowania i odszyfrowywania plików cookie, dlatego w aplikacjach musi być używany spójny schemat.
* Używana jest wspólna lokalizacja magazynu [kluczy ochrony danych](xref:security/data-protection/implementation/key-management) .
  * W ASP.NET Core aplikacje <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> służy do ustawiania lokalizacji magazynu kluczy.
  * W .NET Framework aplikacje oprogramowanie pośredniczące uwierzytelniania plików cookie korzysta z implementacji <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>. `DataProtectionProvider` zapewnia usługi ochrony danych do szyfrowania i odszyfrowywania danych ładunku plików cookie uwierzytelniania. Wystąpienie `DataProtectionProvider` jest odizolowane od systemu ochrony danych używanego przez inne części aplikacji. [DataProtectionProvider. Create (System. IO. DirectoryInfo, Action\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) akceptuje <xref:System.IO.DirectoryInfo>, aby określić lokalizację magazynu kluczy ochrony danych.
* `DataProtectionProvider` wymaga pakietu NuGet [Microsoft. AspNetCore. dataprotection. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) :
  * W ASP.NET Core 2. x aplikacje odwołują się do [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).
  * W .NET Framework aplikacje Dodaj odwołanie do pakietu do [Microsoft. AspNetCore. dataprotection. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).
* <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> ustawia wspólną nazwę aplikacji.

## <a name="share-authentication-cookies-with-aspnet-core-identity"></a>Udostępnianie plików cookie uwierzytelniania przy użyciu tożsamości ASP.NET Core

W przypadku korzystania z ASP.NET Core Identity:

* Klucze ochrony danych i nazwa aplikacji muszą być udostępniane między aplikacjami. W poniższych przykładach udostępniono wspólną lokalizację magazynu kluczy w <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> metodzie. Użyj <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*>, aby skonfigurować wspólną nazwę udostępnionej aplikacji (`SharedCookieApp` w poniższych przykładach). Aby uzyskać więcej informacji, zobacz <xref:security/data-protection/configuration/overview>.
* Użyj metody rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*>, aby skonfigurować usługę ochrony danych dla plików cookie.
* Domyślny typ uwierzytelniania to `Identity.Application`.

W pliku `Startup.ConfigureServices`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem("{PATH TO COMMON KEY RING FOLDER}")
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

## <a name="share-authentication-cookies-without-aspnet-core-identity"></a>Udostępnianie plików cookie uwierzytelniania bez tożsamości ASP.NET Core

Korzystając z plików cookie bezpośrednio bez ASP.NET Core Identity, skonfiguruj ochronę danych i uwierzytelnianie w programie `Startup.ConfigureServices`. W poniższym przykładzie typ uwierzytelniania jest ustawiony na `Identity.Application`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem("{PATH TO COMMON KEY RING FOLDER}")
    .SetApplicationName("SharedCookieApp");

services.AddAuthentication("Identity.Application")
    .AddCookie("Identity.Application", options =>
    {
        options.Cookie.Name = ".AspNet.SharedCookie";
    });
```

## <a name="share-cookies-across-different-base-paths"></a>Udostępnianie plików cookie między różnymi ścieżkami podstawowymi

Plik cookie uwierzytelniania używa jako domyślnego [pliku cookie](xref:Microsoft.AspNetCore.Http.CookieBuilder.Path) [. PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) . Path. Jeśli plik cookie aplikacji musi być współużytkowany przez różne ścieżki podstawowe, należy przesłonić `Path`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem("{PATH TO COMMON KEY RING FOLDER}")
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
    options.Cookie.Path = "/";
});
```

## <a name="share-cookies-across-subdomains"></a>Udostępnianie plików cookie w poddomenach

W przypadku hostowania aplikacji, które współużytkują pliki cookie w poddomenach, należy określić wspólną domenę we właściwości [plik cookie. domena](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) . Aby udostępnić pliki cookie między aplikacjami w `contoso.com`, na przykład `first_subdomain.contoso.com` i `second_subdomain.contoso.com`, określ `Cookie.Domain` jako `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a>Szyfruj klucze ochrony danych w spoczynku

W przypadku wdrożeń produkcyjnych Skonfiguruj `DataProtectionProvider`, aby szyfrować klucze przy użyciu funkcji DPAPI lub certyfikatu x509. Aby uzyskać więcej informacji, zobacz <xref:security/data-protection/implementation/key-encryption-at-rest>. W poniższym przykładzie odcisk palca certyfikatu jest dostarczany do <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Udostępnianie plików cookie uwierzytelniania między ASP.NET. x a aplikacjami ASP.NET Core

ASP.NET 4. x aplikacje używające oprogramowania do uwierzytelniania plików cookie Katana mogą być skonfigurowane do generowania plików cookie uwierzytelniania, które są zgodne z ASP.NET Corem oprogramowania pośredniczącego uwierzytelniania plików cookie. Dzięki temu można uaktualnić poszczególne aplikacje dużej witryny w kilku krokach, jednocześnie zapewniając bezproblemowe środowisko logowania jednokrotnego w całej lokacji.

Gdy aplikacja używa oprogramowania pośredniczącego Katana uwierzytelniania plików cookie, wywołuje `UseCookieAuthentication` w pliku *Startup.auth.cs* projektu. Projekty aplikacji sieci Web ASP.NET 4. x utworzone przy użyciu Visual Studio 2013 i później domyślnie korzystają z oprogramowania pośredniczącego uwierzytelniania Katana cookie. Chociaż `UseCookieAuthentication` są przestarzałe i nieobsługiwane w przypadku aplikacji ASP.NET Core, wywoływanie `UseCookieAuthentication` w aplikacji ASP.NET 4. x, która używa oprogramowania pośredniczącego Katana uwierzytelniania plików cookie.

Aplikacja ASP.NET 4. x musi być docelowa .NET Framework 4.5.1 lub nowsza. W przeciwnym razie instalacja niezbędnych pakietów NuGet nie powiodła się.

Aby udostępnić pliki cookie uwierzytelniania między aplikacją ASP.NET 4. x i aplikacją ASP.NET Core, skonfiguruj aplikację ASP.NET Core zgodnie z opisem w sekcji [udostępnianie plików cookie uwierzytelniania w aplikacjach ASP.NET Core Apps](#share-authentication-cookies-with-aspnet-core-identity) , a następnie skonfiguruj aplikację ASP.NET 4. x w następujący sposób.

Upewnij się, że pakiety aplikacji zostały zaktualizowane do najnowszej wersji. Zainstaluj pakiet [Microsoft. Owin. Security. Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) w każdej aplikacji ASP.NET 4. x.

Znajdź i zmodyfikuj wywołanie `UseCookieAuthentication`:

* Zmień nazwę pliku cookie tak, aby odpowiadała nazwie używanej przez oprogramowanie pośredniczące uwierzytelniania plików cookie ASP.NET Core (`.AspNet.SharedCookie` w tym przykładzie).
* W poniższym przykładzie typ uwierzytelniania jest ustawiony na `Identity.Application`.
* Podaj wystąpienie `DataProtectionProvider` zainicjowane do lokalizacji magazynu kluczy Common Data Protection.
* Upewnij się, że nazwa aplikacji jest ustawiona na wspólną nazwę aplikacji używaną przez wszystkie aplikacje, które współużytkują pliki cookie uwierzytelniania (`SharedCookieApp` w tym przykładzie).

Jeśli nie zostanie ustawiona `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` i `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, należy ustawić <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> na zastrzeżenie odróżniające unikatowych użytkowników.

*/Startup.auth.cs App_Start*:

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
            DataProtectionProvider.Create("{PATH TO COMMON KEY RING FOLDER}",
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

Podczas generowania tożsamości użytkownika typ uwierzytelniania (`Identity.Application`) musi być zgodny z typem zdefiniowanym w `AuthenticationType` zestawu z `UseCookieAuthentication` w *App_Start/Startup.auth.cs*.

*Modele/IdentityModels. cs*:

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

## <a name="use-a-common-user-database"></a>Używanie wspólnej bazy danych użytkownika

Gdy aplikacje używają tego samego schematu tożsamości (ta sama wersja tożsamości), upewnij się, że system tożsamości dla każdej aplikacji jest wskazywany w tej samej bazie danych użytkownika. W przeciwnym razie system tożsamości generuje błędy w czasie wykonywania, gdy próbuje dopasować informacje w pliku cookie uwierzytelniania względem informacji w swojej bazie danych.

Gdy schemat tożsamości różni się między aplikacjami, zwykle ponieważ aplikacje korzystają z różnych wersji tożsamości, udostępnianie wspólnej bazy danych opartej na najnowszej wersji tożsamości nie jest możliwe bez ponownego mapowania i dodawania kolumn w schematach tożsamości innych aplikacji. Jest to często bardziej wydajne, aby uaktualnić inne aplikacje w celu korzystania z najnowszej wersji tożsamości, aby Wspólna baza danych mogła być współużytkowana przez aplikacje.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:host-and-deploy/web-farm>
