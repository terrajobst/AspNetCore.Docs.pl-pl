---
title: "Uwierzytelniać użytkowników za pomocą protokołu WS-Federation w ASP.NET Core"
author: chlowell
description: "W tym samouczku przedstawiono sposób użycia protokołu WS-Federation w aplikacji platformy ASP.NET Core."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: 0532f866e9c58b2e45623f522f62438e15017e54
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>Uwierzytelniać użytkowników za pomocą protokołu WS-Federation w ASP.NET Core

Ten samouczek pokazuje, jak umożliwić użytkownikom logowanie za pomocą dostawcy uwierzytelniania protokołu WS-Federation jak Active Directory Federation Services (ADFS) lub [usługi Azure Active Directory](/azure/active-directory/) (AAD). Używa Przykładowa aplikacja platformy ASP.NET Core 2.0 opisanego w [Facebook, Google i zewnętrznego dostawcy uwierzytelniania](xref:security/authentication/social/index).

Dla aplikacji programu ASP.NET 2.0 Core Obsługa protokołu WS-Federation jest zapewniana przez [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Ten składnik jest przenoszone z [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) i udostępnia wiele mechanika tego składnika. Jednak składniki różnią się na kilka sposobów.

Domyślnie nowe oprogramowanie pośredniczące:

* Nie zezwalaj na niechciane logowania. Ta funkcja protokołu WS-Federation jest narażony na ataki XSRF. Jednak można ją włączyć za `AllowUnsolicitedLogins` opcji.
* Nie sprawdza co post formularza dla logowania w wiadomości. Tylko żądania do `CallbackPath` są sprawdzane pod kątem logowania ubezp `CallbackPath` domyślnie `/signin-wsfed` , ale można zmienić. Ta ścieżka może być udostępniane innym innych dostawców uwierzytelniania, włączając `SkipUnrecognizedRequests` opcji.

## <a name="register-the-app-with-active-directory"></a>Rejestrowanie aplikacji w usłudze Active Directory

### <a name="active-directory-federation-services"></a>Usługi federacyjne Active Directory

* Otwórz okno serwera **jednostki uzależnionej strony Kreatora dodawania relacji zaufania** z poziomu konsoli zarządzania usług AD FS:

![Dodaj Kreator zaufania uzależniona:-Zapraszamy!](ws-federation/_static/AdfsAddTrust.png)

* Wybierz ręcznie wprowadź dane:

![Dodaj kreatora zaufania jednostki uzależnionej: Wybierz źródło danych](ws-federation/_static/AdfsSelectDataSource.png)

* Wprowadź nazwę wyświetlaną dla jednostki uzależnionej. Nazwa nie jest istotne dla aplikacji platformy ASP.NET Core.

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) Brak obsługi szyfrowania tokenu, dlatego nie skonfigurować certyfikat szyfrowania tokenu:

![Dodaj kreatora zaufania jednostki uzależnionej: Konfigurowanie certyfikatu](ws-federation/_static/AdfsConfigureCert.png)

* Włącz obsługę protokołu WS-Federation Passive przy użyciu adresu URL aplikacji. Sprawdź, czy port jest poprawna dla aplikacji:

![Dodaj kreatora zaufania jednostki uzależnionej: Skonfiguruj adresy URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Musi to być adres URL HTTPS. Usługi IIS Express zapewniają certyfikatu z podpisem własnym, odnośnie do hostowania aplikacji podczas tworzenia. Kestrel wymaga certyfikatu ręcznej konfiguracji. Zobacz [Kestrel dokumentacji](xref:fundamentals/servers/kestrel) więcej szczegółów.

* Kliknij przycisk **dalej** za pośrednictwem pozostałe kroki kreatora i **Zamknij** na końcu.

* ASP.NET Core Identity wymaga **Identyfikatora nazwy** oświadczeń. Dodaj jeden z **Edycja reguł oświadczeń** okna dialogowego:

![Edycja reguł oświadczeń](ws-federation/_static/EditClaimRules.png)

* W **przekształcenie Kreatorze dodawania reguły oświadczenia**, pozostaw wartość domyślną **wysyłanie atrybutów LDAP jako oświadczeń** wybrany szablon, a następnie kliknij przycisk **dalej**. Dodaj mapowanie reguły **nazwy konta SAM** atrybutu LDAP **Identyfikatora nazwy** oświadczeń wychodzących:

![Dodaj kreatora reguły przekształcania oświadczeń: Konfiguruj regułę dotyczącą oświadczeń](ws-federation/_static/AddTransformClaimRule.png)

* Kliknij przycisk **Zakończ** > **OK** w **Edycja reguł oświadczeń** okna.

### <a name="azure-active-directory"></a>Azure Active Directory

* Przejdź do bloku rejestracji aplikacji dzierżawę usługi AAD. Kliknij przycisk **nowej rejestracji aplikacji**:

![Usługi Azure Active Directory: Rejestracje aplikacji](ws-federation/_static/AadNewAppRegistration.png)

* Wprowadź nazwę dla rejestracji aplikacji. Nie jest to ważne w aplikacji platformy ASP.NET Core.
* Wprowadź adres URL aplikacji nasłuchuje jako **adres URL logowania**:

![Azure Active Directory: Tworzenie rejestracji aplikacji](ws-federation/_static/AadCreateAppRegistration.png)

* Kliknij przycisk **punkty końcowe** i zanotuj **dokument metadanych usług federacyjnych** adresu URL. Jest to oprogramowanie pośredniczące WS-Federation `MetadataAddress`:

![Usługa Azure Active Directory: punkty końcowe](ws-federation/_static/AadFederationMetadataDocument.png)

* Przejdź do nowej rejestracji aplikacji. Kliknij przycisk **ustawienia** > **właściwości** i zanotuj **identyfikator URI aplikacji**. Jest to oprogramowanie pośredniczące WS-Federation `Wtrealm`:

![Usługi Azure Active Directory: Właściwości rejestracji aplikacji](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>Dodaj WS-Federation jako dostawcy logowania zewnętrznego dla ASP.NET Core Identity

* Dodawanie zależności na [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) do projektu.
* Dodaj WS-Federation do `Configure` metody w *Startup.cs*:

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddAuthentication()
        .AddWsFederation(options =>
        {
            // MetadataAddress represents the Active Directory instance used to authenticate users.
            options.MetadataAddress = "https://<ADFS FQDN or AAD tenant>/FederationMetadata/2007-06/FederationMetadata.xml";

            // Wtrealm is the app's identifier in the Active Directory instance.
            // For ADFS, use the relying party's identifier, its WS-Federation Passive protocol URL:
            options.Wtrealm = "https://localhost:44307/";

            // For AAD, use the App ID URI from the app registration's Properties blade:
            options.Wtrealm = "https://wsfedsample.onmicrosoft.com/bf0e7e6d-056e-4e37-b9a6-2c36797b9f01";
        });

    services.AddMvc()
     // ...
    ```

[!INCLUDE[default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>Zaloguj się za pomocą protokołu WS-Federation

Przejdź do aplikacji i kliknij przycisk **Zaloguj** znajdujące się w nagłówku nawigacji. Istnieje możliwość logowania za pomocą WsFederation: ![strony logowania](ws-federation/_static/WsFederationButton.png)

Z usług AD FS jako dostawcę, przycisk przekierowuje do strony logowania usług AD FS: ![strony logowania usług AD FS](ws-federation/_static/AdfsLoginPage.png)

Usłudze Azure Active Directory jako dostawcy, przycisk przekierowuje do strony logowania usługi AAD: ![strony logowania usługi AAD](ws-federation/_static/AadSignIn.png)

Pomyślne logowanie dla nowego użytkownika przekierowuje do strony rejestracji aplikacji: ![strony rejestru](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>Użyj protokołu WS-Federation bez tożsamości platformy ASP.NET Core

Oprogramowania pośredniczącego protokołu WS-Federation może być używany bez tożsamości. Na przykład:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultSignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = WsFederationDefaults.AuthenticationScheme;
    })
    .AddWsFederation(options =>
    {
        options.Wtrealm = Configuration["wsfed:realm"];
        options.MetadataAddress = Configuration["wsfed:metadata"];
    })
    .AddCookie();
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
        // …
}
```
