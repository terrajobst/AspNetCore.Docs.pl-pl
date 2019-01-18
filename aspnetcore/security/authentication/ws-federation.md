---
title: Uwierzytelnianie użytkowników za pomocą protokołu WS-Federation w programie ASP.NET Core
author: chlowell
description: Ten samouczek pokazuje, jak używać protokołu WS-Federation w aplikacji ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/16/2019
uid: security/authentication/ws-federation
ms.openlocfilehash: 6b568928aaf6c958d66279af9fef80ac0c968c8b
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396158"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>Uwierzytelnianie użytkowników za pomocą protokołu WS-Federation w programie ASP.NET Core

W tym samouczku pokazano, jak umożliwić użytkownikom logowanie za pomocą dostawcy uwierzytelniania protokołu WS-Federation takich jak Active Directory Federation Services (ADFS) lub [usługi Azure Active Directory](/azure/active-directory/) (AAD). Używa ona przykładową aplikację platformy ASP.NET Core 2.0 opisano w [Facebook, Google i uwierzytelniania zewnętrznego dostawcy](xref:security/authentication/social/index).

W przypadku aplikacji ASP.NET Core 2.0, WS-Federation pomoc techniczną świadczy [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Ten składnik jest przenoszone z [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) i udostępnia wiele mechanics tego składnika. Jednak składniki różnią się na kilka sposobów.

Domyślnie nowe oprogramowanie pośredniczące:

* Nie zezwalaj na niechciane logowania. Ta funkcja protokołu WS-Federation jest narażony na ataki XSRF. Jednak można ją włączyć za pomocą `AllowUnsolicitedLogins` opcji.
* Nie sprawdza co post formularza logowania komunikatów. Tylko żądania `CallbackPath` są sprawdzane pod kątem logowania ubezp `CallbackPath` wartość domyślna to `/signin-wsfed` , ale można zmienić za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [ WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) klasy. Tej ścieżki mogą być współużytkowane z innymi dostawcami uwierzytelniania przez włączenie [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) opcji.

## <a name="register-the-app-with-active-directory"></a>Rejestrowanie aplikacji w usłudze Active Directory

### <a name="active-directory-federation-services"></a>Active Directory Federation Services

* Otwórz okno serwera **jednostki uzależnionej strony zaufania Kreatora dodawania** z poziomu konsoli zarządzania usług AD FS:

![Dodaj jednostki uzależnionej zaufania kreatora: Witaj](ws-federation/_static/AdfsAddTrust.png)

* Wybierz ręcznie wprowadź dane jednostki:

![Dodaj jednostki uzależnionej zaufania kreatora: Wybierz źródło danych](ws-federation/_static/AdfsSelectDataSource.png)

* Wprowadź nazwę wyświetlaną dla jednostki uzależnionej. Nazwa nie jest istotne dla aplikacji ASP.NET Core.

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) brakuje obsługi szyfrowania tokenu, dlatego nie należy konfigurować certyfikat szyfrowania tokenu:

![Dodaj jednostki uzależnionej zaufania kreatora: Konfigurowanie certyfikatu](ws-federation/_static/AdfsConfigureCert.png)

* Włącz obsługę protokołu WS-Federation Passive przy użyciu adresu URL aplikacji. Sprawdź, czy port jest poprawna dla aplikacji:

![Dodaj jednostki uzależnionej zaufania kreatora: Skonfiguruj adres URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Musi to być adres URL HTTPS. Usługi IIS Express może zapewnić certyfikat z podpisem własnym, odnośnie do hostowania aplikacji podczas programowania. Kestrel wymaga certyfikatu ręcznej konfiguracji. Zobacz [dokumentacji Kestrel](xref:fundamentals/servers/kestrel) Aby uzyskać więcej informacji.

* Kliknij przycisk **dalej** przez pozostałą część kreatora i **Zamknij** na końcu.

* Tożsamości platformy ASP.NET Core wymaga **identyfikator nazwy** oświadczenia. Dodaj jeden z **Edycja reguł oświadczeń** okno dialogowe:

![Edytuj reguły oświadczeń](ws-federation/_static/EditClaimRules.png)

* W **oświadczenia Kreatorze dodawania reguły przekształcania**, pozostaw wartość domyślną **wysyłać atrybuty LDAP jako oświadczenia** wybrany szablon, a następnie kliknij przycisk **dalej**. Dodaj mapowanie reguły **nazwy konta SAM** atrybutu LDAP, aby **identyfikator nazwy** oświadczenie wychodzące:

![Dodaj kreatora reguły oświadczeń przekształcenia: Konfiguruj regułę dotyczącą oświadczeń](ws-federation/_static/AddTransformClaimRule.png)

* Kliknij przycisk **Zakończ** > **OK** w **Edycja reguł oświadczeń** okna.

### <a name="azure-active-directory"></a>Azure Active Directory

* Przejdź do bloku rejestracje aplikacji dzierżawy usługi AAD. Kliknij przycisk **rejestrowanie nowej aplikacji**:

![Azure Active Directory: Rejestracje aplikacji](ws-federation/_static/AadNewAppRegistration.png)

* Wprowadź nazwę dla rejestracji aplikacji. Nie jest to istotne dla aplikacji ASP.NET Core.
* Wprowadź adres URL aplikacji nasłuchuje jako **adres URL logowania**:

![Azure Active Directory: Tworzenie rejestracji aplikacji](ws-federation/_static/AadCreateAppRegistration.png)

* Kliknij przycisk **punktów końcowych** i zanotuj **dokument metadanych Federacji** adresu URL. To jest oprogramowanie pośredniczące WS-Federation `MetadataAddress`:

![Azure Active Directory: Punkty końcowe](ws-federation/_static/AadFederationMetadataDocument.png)

* Przejdź do nowej rejestracji aplikacji. Kliknij przycisk **ustawienia** > **właściwości** i zanotuj **identyfikator URI Identyfikatora aplikacji**. To jest oprogramowanie pośredniczące WS-Federation `Wtrealm`:

![Azure Active Directory: Właściwości rejestracji aplikacji](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>Dodawanie usługi WS-Federation jako dostawcy logowania zewnętrznego dla tożsamości platformy ASP.NET Core

* Dodać zależność od [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) do projektu.
* Dodaj WS-Federation na `Startup.ConfigureServices`:

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

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>Zaloguj się przy użyciu protokołu WS-Federation

Przejdź do aplikacji i kliknij pozycję **Zaloguj** łącze w nagłówku nawigacji. Istnieje możliwość Zaloguj się przy użyciu WsFederation: ![Zaloguj się na stronie](ws-federation/_static/WsFederationButton.png)

Za pomocą usług AD FS jako dostawcy przycisk wykonuje przekierowanie do strony logowania usług AD FS: ![Strony logowania usług AD FS](ws-federation/_static/AdfsLoginPage.png)

Za pomocą usługi Azure Active Directory jako dostawcy przycisk wykonuje przekierowanie do strony logowania usługi AAD: ![Strona logowania usługi AAD](ws-federation/_static/AadSignIn.png)

Pomyślne logowanie dla nowego użytkownika przekierowuje do strony rejestracji aplikacji: ![Strona rejestracji](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>Użyj protokołu WS-Federation, bez użycia produktu ASP.NET Core Identity

Oprogramowanie pośredniczące WS-Federation, można bez użycia produktu Identity. Na przykład:

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
