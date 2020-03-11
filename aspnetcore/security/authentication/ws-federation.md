---
title: Uwierzytelnianie użytkowników za pomocą usługi WS-Federation w ASP.NET Core
author: chlowell
description: W tym samouczku pokazano, jak używać usługi WS-Federation w aplikacji ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/16/2019
uid: security/authentication/ws-federation
ms.openlocfilehash: d82421a14ede6cb6b01ef59f233bb2eba6b56aec
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655429"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>Uwierzytelnianie użytkowników za pomocą usługi WS-Federation w ASP.NET Core

W tym samouczku pokazano, jak umożliwić użytkownikom logowanie się przy użyciu dostawcy uwierzytelniania WS-Federation, takiego jak Active Directory Federation Services (ADFS) lub [Azure Active Directory](/azure/active-directory/) (AAD). Używa przykładowej aplikacji ASP.NET Core 2,0 opisanej w temacie [uwierzytelnianie w serwisach Facebook, Google i dostawcy zewnętrznym](xref:security/authentication/social/index).

W przypadku aplikacji ASP.NET Core 2,0 obsługa protokołu WS-Federation jest zapewniana przez [Microsoft. AspNetCore. Authentication. WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Ten składnik jest przewoźny z [Microsoft. Owin. Security. WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) i udostępnia wiele elementów tego Mechanics. Składniki te są jednak różne na kilka ważnych sposobów.

Domyślnie nowe oprogramowanie pośredniczące:

* Nie zezwala na nieżądane nazwy logowania. Ta funkcja protokołu WS-Federation jest narażona na ataki XSRF. Można go jednak włączyć przy użyciu opcji `AllowUnsolicitedLogins`.
* Nie sprawdza każdego wpisu w formularzu dla wiadomości logowania. Tylko żądania do `CallbackPath` są sprawdzane pod kątem logowań. `CallbackPath` domyślnie `/signin-wsfed`, ale można je zmienić za pomocą dziedziczonej właściwości [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) klasy [WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) . Ta ścieżka może być współużytkowana z innymi dostawcami uwierzytelniania przez włączenie opcji [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) .

## <a name="register-the-app-with-active-directory"></a>Zarejestruj aplikację w Active Directory

### <a name="active-directory-federation-services"></a>Usługi Active Directory Federation Services

* Otwórz **Kreatora dodawania zaufania jednostki uzależnionej** serwera z konsoli zarządzania usług AD FS:

![Kreator dodawania zaufania jednostki uzależnionej: Witamy](ws-federation/_static/AdfsAddTrust.png)

* Wybierz, aby ręcznie wprowadzić dane:

![Kreator dodawania zaufania jednostki uzależnionej: Wybieranie źródła danych](ws-federation/_static/AdfsSelectDataSource.png)

* Wprowadź nazwę wyświetlaną jednostki uzależnionej. Nazwa nie jest ważna dla aplikacji ASP.NET Core.

* [Microsoft. AspNetCore. Authentication. WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) nie obsługuje szyfrowania tokenów, dlatego nie należy konfigurować certyfikatu szyfrowania tokenu:

![Kreator dodawania zaufania jednostki uzależnionej: Konfigurowanie certyfikatu](ws-federation/_static/AdfsConfigureCert.png)

* Włącz obsługę protokołu pasywnego usługi WS-Federation przy użyciu adresu URL aplikacji. Sprawdź, czy port jest prawidłowy dla aplikacji:

![Kreator dodawania zaufania jednostki uzależnionej: Konfigurowanie adresu URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Musi to być adres URL HTTPS. IIS Express może podać certyfikat z podpisem własnym podczas udostępniania aplikacji podczas opracowywania. Kestrel wymaga ręcznej konfiguracji certyfikatu. Aby uzyskać więcej informacji, zobacz [dokumentację Kestrel](xref:fundamentals/servers/kestrel) .

* Kliknij przycisk **dalej** w pozostałej części kreatora i **Zamknij** na końcu.

* Tożsamość ASP.NET Core wymaga podania **identyfikatora nazwy** . Dodaj jedną z okna dialogowego **Edytowanie reguł dotyczących roszczeń** :

![Edytuj reguły dotyczące oświadczeń](ws-federation/_static/EditClaimRules.png)

* W **Kreatorze dodawania reguły przekształcania oświadczeń**pozostaw zaznaczone pole wyboru domyślne **Wysyłaj atrybuty LDAP as** , a następnie kliknij przycisk **dalej**. Dodaj mapowanie reguły atrybut LDAP **nazwa konta sam** do żądania wychodzącego **Identyfikator nazwy** :

![Kreator dodawania reguły przekształcania roszczeń: Konfigurowanie reguły dotyczącej roszczeń](ws-federation/_static/AddTransformClaimRule.png)

* Kliknij przycisk **zakończ** > **OK** w oknie **Edytowanie reguł roszczeń** .

### <a name="azure-active-directory"></a>Azure Active Directory

* Przejdź do bloku rejestracje aplikacji dzierżawy usługi AAD. Kliknij pozycję **rejestracja nowej aplikacji**:

![Azure Active Directory: Rejestracje aplikacji](ws-federation/_static/AadNewAppRegistration.png)

* Wprowadź nazwę rejestracji aplikacji. Nie ma to znaczenia dla aplikacji ASP.NET Core.
* Wprowadź adres URL, na który aplikacja nasłuchuje jako **adres URL logowania**:

![Azure Active Directory: Utwórz rejestrację aplikacji](ws-federation/_static/AadCreateAppRegistration.png)

* Kliknij pozycję **punkty końcowe** i Zanotuj adres URL **dokumentu metadanych Federacji** . Jest to `MetadataAddress`oprogramowania pośredniczącego usługi WS-Federation:

![Azure Active Directory: punkty końcowe](ws-federation/_static/AadFederationMetadataDocument.png)

* Przejdź do rejestracji nowej aplikacji. Kliknij pozycję **ustawienia** > **Właściwości** i zanotuj **Identyfikator URI aplikacji**. Jest to `Wtrealm`oprogramowania pośredniczącego usługi WS-Federation:

![Azure Active Directory: właściwości rejestracji aplikacji](ws-federation/_static/AadAppIdUri.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>Korzystanie z usługi WS-Federation bez tożsamości ASP.NET Core

Oprogramowanie pośredniczące WS-Federation może być używane bez tożsamości. Na przykład:
::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/StartupNon31.cs?name=snippet)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/StartupNon21.cs?name=snippet)]
::: moniker-end

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>Dodawanie protokołu WS-Federation jako dostawcy logowania zewnętrznego dla tożsamości ASP.NET Core

* Dodaj zależność od elementu [Microsoft. AspNetCore. Authentication. WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) do projektu.
* Dodaj usługę WS-Federation do `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/Startup31.cs?name=snippet)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/Startup21.cs?name=snippet)]
::: moniker-end

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>Logowanie za pomocą usługi WS-Federation

Przejdź do aplikacji, a następnie kliknij link **Zaloguj** w nagłówku nawigacji. Istnieje możliwość zalogowania się za pomocą WsFederation: ![log na stronie](ws-federation/_static/WsFederationButton.png)

Za pomocą usług ADFS jako dostawca przycisk przekieruje się do strony logowania usług ADFS: ![strony logowania usług ADFS](ws-federation/_static/AdfsLoginPage.png)

Za pomocą Azure Active Directory jako dostawca przycisk przekierowuje do strony logowania usługi AAD: ![stronie logowania w usłudze AAD](ws-federation/_static/AadSignIn.png)

Pomyślne logowanie do nowego użytkownika przekieruje się do strony rejestracji użytkownika aplikacji: ![rejestracji strony](ws-federation/_static/Register.png)