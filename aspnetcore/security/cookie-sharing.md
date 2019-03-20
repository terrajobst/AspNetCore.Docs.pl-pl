---
title: Udostępnianie plików cookie między aplikacjami ASP.NET i ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak udostępnianie plików cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
uid: security/cookie-sharing
ms.openlocfilehash: 504d3369c908aabddeaa2fc3d1ef16a42b5cb771
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208518"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a>Udostępnianie plików cookie między aplikacjami ASP.NET i ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Luke Latham](https://github.com/guardrex)

Witryny sieci Web często składają się z poszczególnych aplikacji web apps pracujących razem. Aby zapewnić środowisko logowania jednokrotnego (SSO), aplikacje sieci web w obrębie lokacji muszą współużytkować pliki cookie uwierzytelniania. Aby zapewnić obsługę tego scenariusza, stosu ochrony danych umożliwia udostępnianie Katana uwierzytelniania plików cookie i biletów uwierzytelniania pliku cookie platformy ASP.NET Core.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Przykład ilustruje udostępnianie między trzy aplikacje, które korzystają z plików cookie uwierzytelniania plików cookie:

* ASP.NET Core 2.0 Razor strony aplikacji bez korzystania z [tożsamości platformy ASP.NET Core](xref:security/authentication/identity)
* Aplikacja MVC platformy ASP.NET Core 2.0 przy użyciu tożsamości platformy ASP.NET Core
* Aplikacja MVC platformy ASP.NET Framework 4.6.1 w produkcie ASP.NET Identity

W przykładach, wykonaj następujące czynności:

* Nazwa pliku cookie uwierzytelniania jest równa wartości typowych `.AspNet.SharedCookie`.
* `AuthenticationType` Ustawiono `Identity.Application` jawnie lub domyślnie.
* Nazwa pospolita aplikacji służy do włączania systemu ochrony danych udostępnić klucze ochrony danych (`SharedCookieApp`).
* `Identity.Application` jest używana jako schematu uwierzytelniania. Niezależnie od schematu jest używany, musi być stosowane konsekwentnie *wewnątrz i pomiędzy* aplikacji udostępnionych plików cookie, jako domyślny schemat lub przez jawne ustawienie go. Schemat jest używany podczas szyfrowania i odszyfrowywania plików cookie, dzięki czemu spójny schemat musi być używana w różnych aplikacjach.
* Często [klucz ochrony danych](xref:security/data-protection/implementation/key-management) lokalizacja magazynu jest używana. Przykładowa aplikacja korzysta z folderem o nazwie *pęku kluczy* w katalogu głównym rozwiązanie do przechowywania kluczy ochrony danych.
* W aplikacji platformy ASP.NET Core [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) służy do ustawiania lokalizacji magazynu kluczy. [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) służy do konfigurowania nazwą pospolitą udostępnionej aplikacji.
* W aplikacji .NET Framework, oprogramowanie pośredniczące uwierzytelniania plików cookie korzysta z implementacją [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider). `DataProtectionProvider` udostępnia usługi ochrony danych do szyfrowania i odszyfrowywania danych ładunku plików cookie uwierzytelniania. `DataProtectionProvider` Wystąpienie jest izolowane od system ochrony danych, które są używane przez inne części aplikacji.
  * [DataProtectionProvider.Create (System.IO.DirectoryInfo, Akcja\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) akceptuje [DirectoryInfo](/dotnet/api/system.io.directoryinfo) Określ lokalizację do przechowywania kluczy ochrony danych. Przykładowa aplikacja udostępnia ścieżkę *pęku kluczy* folder `DirectoryInfo`. [DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) wspólnej nazwy aplikacji.
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) wymaga [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pakietu NuGet. Aby uzyskać tego pakietu dla platformy ASP.NET Core 2.1 i nowsze aplikacji, należy odwołać [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Podczas określania wartości docelowej programu .NET Framework, Dodaj odwołanie do pakietu `Microsoft.AspNetCore.DataProtection.Extensions`.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Udostępnianie plików cookie uwierzytelniania między aplikacjami ASP.NET Core

W przypadku używania tożsamości platformy ASP.NET Core:

::: moniker range=">= aspnetcore-2.0"

W `ConfigureServices` metody, użyj [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) metodę rozszerzenia, aby skonfigurować usługi ochrony danych plików cookie.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

Klucze ochrony danych i nazwę aplikacji musi być współużytkowany przez aplikacje. W przykładowym aplikacjom oraz `GetKeyRingDirInfo` zwraca wspólnej lokalizacji magazynu kluczy w celu [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) metody. Użyj [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) skonfigurować nazwę pospolitą udostępnionej aplikacji (`SharedCookieApp` w przykładzie). Aby uzyskać więcej informacji, zobacz [konfigurowania ochrony danych](xref:security/data-protection/configuration/overview).

Hostowania aplikacji, które udostępnianie plików cookie między domenami podrzędnymi, należy określić w typowych domeny [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) właściwości. Udostępnianie plików cookie między aplikacjami na `contoso.com`, takich jak `first_subdomain.contoso.com` i `second_subdomain.contoso.com`, określ `Cookie.Domain` jako `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

Zobacz *CookieAuthWithIdentity.Core* projektu w [przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([sposobu pobierania](xref:index#how-to-download-a-sample)).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

W `Configure` metody, użyj [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) do skonfigurowania:

* Usługa ochrony danych plików cookie.
* `AuthenticationScheme` Do dopasowania ASP.NET 4.x.

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

::: moniker-end

W przypadku używania plików cookie bezpośrednio:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

Klucze ochrony danych i nazwę aplikacji musi być współużytkowany przez aplikacje. W przykładowym aplikacjom oraz `GetKeyRingDirInfo` zwraca wspólnej lokalizacji magazynu kluczy w celu [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) metody. Użyj [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) skonfigurować nazwę pospolitą udostępnionej aplikacji (`SharedCookieApp` w przykładzie). Aby uzyskać więcej informacji, zobacz [konfigurowania ochrony danych](xref:security/data-protection/configuration/overview).

Hostowania aplikacji, które udostępnianie plików cookie między domenami podrzędnymi, należy określić w typowych domeny [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) właściwości. Udostępnianie plików cookie między aplikacjami na `contoso.com`, takich jak `first_subdomain.contoso.com` i `second_subdomain.contoso.com`, określ `Cookie.Domain` jako `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

Zobacz *CookieAuth.Core* projektu w [przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([sposobu pobierania](xref:index#how-to-download-a-sample)).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

::: moniker-end

## <a name="encrypting-data-protection-keys-at-rest"></a>Szyfrowanie kluczy ochrony danych magazynowanych

W przypadku wdrożeń produkcyjnych należy skonfigurować `DataProtectionProvider` do szyfrowania kluczy w stanie spoczynku przy użyciu interfejsu DPAPI lub X509Certificate. Zobacz [klucza szyfrowania na Rest](xref:security/data-protection/implementation/key-encryption-at-rest) Aby uzyskać więcej informacji.

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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

::: moniker-end

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Udostępnianie plików cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core

Aplikacje platformy ASP.NET 4.x, które używają oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana można skonfigurować do generowania plików cookie uwierzytelniania, które są zgodne z oprogramowaniem pośredniczącym uwierzytelniania pliku cookie platformy ASP.NET Core. Dzięki temu można przyjąć uaktualniania dużej witryny poszczególnych aplikacji, zapewniając bezproblemowe środowisko logowania jednokrotnego w lokacji.

Jeśli aplikacja używa oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana, wywołuje `UseCookieAuthentication` w projekcie *Startup.Auth.cs* pliku. Projekty aplikacji sieci web programu ASP.NET 4.x utworzonych za pomocą programu Visual Studio 2013, a później użyć oprogramowanie pośredniczące uwierzytelniania plików cookie Katana domyślnie. Mimo że `UseCookieAuthentication` jest obsługiwany w przypadku aplikacji platformy ASP.NET Core i wywoływania `UseCookieAuthentication` w aplikacji 4.x ASP.NET, która używa Katana oprogramowania pośredniczącego uwierzytelniania pliku cookie jest prawidłowy.

Aplikacja platformy ASP.NET 4.x musi być przeznaczony dla .NET Framework 4.5.1 lub nowszej. W przeciwnym razie niezbędne pakiety NuGet uniemożliwić instalację.

Aby udostępnić pliki cookie uwierzytelniania między aplikację ASP.NET 4.x i aplikacji ASP.NET Core, skonfigurować aplikację ASP.NET Core, jak wspomniano powyżej, a następnie konfigurowania aplikacji ASP.NET 4.x, wykonaj następujące czynności:

1. Zainstaluj pakiet [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) do każdej aplikacji 4.x ASP.NET.

2. W *Startup.Auth.cs*, Znajdź wywołanie `UseCookieAuthentication` i zmodyfikuj go w następujący sposób. Zmień nazwę pliku cookie, aby dopasować nazwę używaną przez oprogramowanie pośredniczące uwierzytelniania plików cookie programu ASP.NET Core. Podaj wystąpienia `DataProtectionProvider` inicjowany do wspólnej lokalizacji magazynu kluczy ochrony danych. Upewnij się, że nazwa aplikacji jest ustawiona na nazwę pospolitą aplikacji używane przez wszystkie aplikacje, których udostępnianie plików cookie, `SharedCookieApp` w przykładowej aplikacji.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

Zobacz *CookieAuthWithIdentity.NETFramework* projektu w [przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([sposobu pobierania](xref:index#how-to-download-a-sample)).

Podczas generowania tożsamości użytkownika, typ uwierzytelniania musi być zgodny z typem zdefiniowanym w `AuthenticationType` zestawu przy użyciu `UseCookieAuthentication`.

*Models/IdentityModels.cs*:

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a>Użyj wspólnej bazy danych użytkownika

Upewnij się, że system tożsamości dla każdej aplikacji jest wskazywany w tej samej bazy danych użytkownika. W przeciwnym razie systemu tożsamości generuje błędy w czasie wykonywania, gdy podejmuje próbę dopasowania informacje zawarte w pliku cookie uwierzytelniania względem informacji w bazie danych.

## <a name="additional-resources"></a>Dodatkowe zasoby

<xref:host-and-deploy/web-farm>
