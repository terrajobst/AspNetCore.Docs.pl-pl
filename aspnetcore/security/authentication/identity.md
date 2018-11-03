---
title: Wprowadzenie do tożsamości programu ASP.NET Core
author: rick-anderson
description: Tożsamość za pomocą aplikacji ASP.NET Core. Dowiedz się, jak ustawić wymagania dotyczące hasła (RequireDigit, RequiredLength, RequiredUniqueChars i inne).
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: 099ebd398238173079e5e659171f31ee5b1f7452
ms.sourcegitcommit: 85f2939af7a167b9694e1d2093277ffc9a741b23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50968335"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Wprowadzenie do tożsamości programu ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Tożsamość platformy ASP.NET Core jest systemu członkostwa, który dodaje funkcje logowania do aplikacji platformy ASP.NET Core. Użytkownicy mogą tworzyć konta informacje logowania, przechowywane w tożsamość lub używają dostawcy logowania zewnętrznego. Dostawcy logowania zewnętrznego obsługiwanych obejmują [Facebook, Google, Account Microsoft i Twitter](xref:security/authentication/social/index).

Tożsamości można skonfigurować przy użyciu bazy danych programu SQL Server do przechowywania nazwy użytkownika, hasła i dane profilu. Alternatywnie innego magazynu trwałego można, na przykład usługa Azure Table Storage.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([sposobu pobierania)](xref:index#how-to-download-a-sample)).

W tym temacie możesz dowiedzieć się, jak zarejestrować, zaloguj się za pomocą tożsamości i wylogowania użytkownika. Aby uzyskać bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji korzystających z tożsamości zobacz sekcję następne kroki na końcu tego artykułu.

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>
## <a name="adddefaultidentity-and-addidentity"></a>AddDefaultIdentity i AddIdentity

[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) została wprowadzona w ASP. Core 2.1. Wywoływanie `AddDefaultIdentity` jest podobny do wywoływania następujące czynności:

* [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [AddDefaultUI](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [AddDefaultTokenProviders](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

Zobacz [źródła AddDefaultIdentity](https://github.com/aspnet/Identity/blob/2634637fd535b229762b5e4a49cdd128f4d8f12e/src/UI/IdentityServiceCollectionUIExtensions.cs#L47-L64) Aby uzyskać więcej informacji.

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a>Tworzenie aplikacji sieci Web z uwierzytelnianiem

Tworzenie projektu aplikacji sieci Web programu ASP.NET Core z indywidualnymi kontami użytkowników.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Wybierz **pliku** > **nowe** > **projektu**.
* Wybierz **aplikacji sieci Web platformy ASP.NET Core**. Nadaj projektowi nazwę **WebApp1** mieć tej samej przestrzeni nazw jako do pobrania projektu. Kliknij przycisk **OK**.
* Wybierz platformy ASP.NET Core **aplikacji sieci Web** platformy ASP.NET Core 2.1, następnie wybierz **Zmień uwierzytelnianie**.
* Wybierz **indywidualne konta użytkowników** i kliknij przycisk **OK**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

Wygenerowany projekt zawiera [tożsamości platformy ASP.NET Core](xref:security/authentication/identity) jako [biblioteki klas Razor](xref:razor-pages/ui-class).

### <a name="test-register-and-login"></a>Rejestrowanie testu i zaloguj się

Uruchom aplikację i zarejestrować użytkownik. Zależności od wielkości ekranu, może być konieczne wybrać przycisk przełączania nawigacji, aby wyświetlić **zarejestrować** i **logowania** łącza.

![Przełącz pasek nawigacyjny przycisku](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a>Konfigurowanie usługi zarządzania tożsamościami

Usługi są dodawane w `ConfigureServices`. Typowy wzorzec polega na wywołaniu wszystkich `Add{Service}` metody, a następnie wywołania wszystkich `services.Configure{Service}` metody. Poniższy kod nie zawiera wygenerowanego szablonu `CookiePolicyOptions`:

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

Powyższy kod konfiguruje tożsamości przy użyciu wartości opcji domyślnych. Usługi są dostępne dla aplikacji za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).

   Tożsamość jest włączona, wywołując [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_). `UseAuthentication` dodaje uwierzytelniania [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) do potoku żądania.

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Usługi są dostępne dla aplikacji za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).

   Tożsamość jest włączone dla aplikacji, wywołując `UseAuthentication` w `Configure` metody. `UseAuthentication` dodaje uwierzytelniania [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) do potoku żądania.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Te usługi są dostępne dla aplikacji za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).

   Tożsamość jest włączone dla aplikacji, wywołując `UseIdentity` w `Configure` metody. `UseIdentity` dodaje na podstawie plików cookie uwierzytelniania [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) do potoku żądania.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

Aby uzyskać więcej informacji, zobacz [klasy IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) i [uruchamiania aplikacji](xref:fundamentals/startup).

## <a name="scaffold-register-login-and-logout"></a>Zarejestruj szkieletu, logowania i wylogowania

Postępuj zgodnie z [tworzenia szkieletu tożsamości do projektu Razor z autoryzacją](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instrukcjami, aby wygenerować kod przedstawiony w tej sekcji.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Dodaj pliki rejestru, logowania i wylogowania.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Jeśli utworzono projekt o nazwie **WebApp1**, uruchom następujące polecenia. W przeciwnym razie użyj poprawną przestrzeń nazw dla `ApplicationDbContext`:

```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

PowerShell używa średnika jako separatora polecenia. Przy użyciu programu PowerShell, ucieczki średnikami, na liście plików lub umieścić na liście plików w podwójnym cudzysłowie, jak w poprzednim przykładzie.

---

### <a name="examine-register"></a>Sprawdź Rejestr

::: moniker range=">= aspnetcore-2.1"

   Kiedy użytkownik kliknie **zarejestrować** linku `RegisterModel.OnPostAsync` akcja jest wywoływana. Użytkownik jest tworzony przez [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) na `_userManager` obiektu. `_userManager` znajduje się przez wstrzykiwanie zależności):

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   Kiedy użytkownik kliknie **zarejestrować** linku `Register` jest wywoływana Akcja `AccountController`. `Register` Akcja powoduje utworzenie użytkownika, wywołując `CreateAsync` na `_userManager` obiektu (udostępniane `AccountController` przez wstrzykiwanie zależności):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   Jeśli użytkownik został pomyślnie utworzony, użytkownik jest zalogowany przez wywołanie `_signInManager.SignInAsync`.

   **Uwaga:** zobacz [konta potwierdzenia](xref:security/authentication/accconfirm#prevent-login-at-registration) kroki uniknąć natychmiastowego logowania podczas rejestracji.

### <a name="log-in"></a>Zaloguj się

::: moniker range=">= aspnetcore-2.1"

Zostanie wyświetlony formularz logowania po:

* **Zaloguj** łącze jest zaznaczone.
* Użytkownik próbuje uzyskać dostęp ograniczony strona, która nie wolno im dostępu **lub** gdy jeszcze nie został uwierzytelniony przez system.

Po przesłaniu formularza na stronie logowania `OnPostAsync` nosi nazwę akcji. `PasswordSignInAsync` jest wywoływana w `_signInManager` obiektu (udostępnione przez wstrzykiwanie zależności).

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   Podstawa `Controller` klasy ujawnia `User` właściwość, której będziesz mieć dostęp z metody kontrolera. Na przykład, można wyliczyć `User.Claims` i podejmowania decyzji dotyczących autoryzacji. Aby uzyskać więcej informacji, zobacz <xref:security/authorization/introduction>.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Formularz logowania jest wyświetlane, gdy użytkownicy wybiorą **Zaloguj** połączenia lub nastąpi przekierowanie podczas uzyskiwania dostępu do strony, która wymaga uwierzytelnienia. Gdy użytkownik przesyła formularz na stronie logowania `AccountController` `Login` nosi nazwę akcji.

`Login` Wywołania akcji `PasswordSignInAsync` na `_signInManager` obiektu (udostępniane `AccountController` przez wstrzykiwanie zależności).

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

Podstawa (`Controller` lub `PageModel`) klasy ujawnia `User` właściwości. Na przykład `User.Claims` mogą być wyliczane w celu podejmowania decyzji dotyczących autoryzacji.

::: moniker-end

### <a name="log-out"></a>Wyloguj

::: moniker range=">= aspnetcore-2.1"

**Wyloguj** wywołana `LogoutModel.OnPost` akcji. 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) czyści oświadczenia użytkownika przechowywane w pliku cookie. Nie przekierowania po wywołaniu `SignOutAsync` lub użytkownik będzie **nie** wylogowanie.

Wpis jest określona w *Pages/Shared/_LoginPartial.cshtml*:

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   Klikając **Wyloguj** link wywołania `LogOut` akcji.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   Poprzedni kod wywołuje `_signInManager.SignOutAsync` metody. `SignOutAsync` Metoda czyści oświadczenia użytkownika przechowywane w pliku cookie.

::: moniker-end

## <a name="test-identity"></a>Testowanie tożsamości

Domyślne szablony projektów sieci web zezwolić na dostęp anonimowy do strony głównej. Aby sprawdzić tożsamość, należy dodać [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) do strony informacje.

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

Jeśli użytkownik jest zalogowany, wyloguj się. Uruchom aplikację i wybierz **o** łącza. Nastąpi przekierowanie do strony logowania.

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a>Zapoznaj się z tożsamości

Aby bardziej szczegółowo zapoznać się z tożsamości:

* [Tworzenie pełnej tożsamości interfejsu użytkownika źródła](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* Sprawdź źródła poszczególnych stron i krok po kroku debugera.

::: moniker-end

## <a name="identity-components"></a>Składniki tożsamości

::: moniker range=">= aspnetcore-2.1"

Wszystkie tożsamości zależne pakiety NuGet są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end

Pakiet podstawowego dla tożsamości jest [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/). Ten pakiet zawiera podstawowy zestaw interfejsów dla produktu ASP.NET Core Identity i jest dołączony przez `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

## <a name="migrating-to-aspnet-core-identity"></a>Migrowanie tożsamości platformy ASP.NET Core

Aby uzyskać więcej informacji oraz wskazówki dotyczące migrowania istniejących sklepu tożsamości, zobacz [migracji uwierzytelnianie i tożsamość](xref:migration/identity).

## <a name="setting-password-strength"></a>Ustawianie siły hasła

Zobacz [konfiguracji](#pw) przykład określająca wymagania dotyczące minimalnych hasła.

## <a name="next-steps"></a>Następne kroki

* [Konfigurowanie tożsamości](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
