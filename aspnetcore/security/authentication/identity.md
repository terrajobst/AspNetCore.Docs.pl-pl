---
title: Wprowadzenie do tożsamości na ASP.NET Core
author: rick-anderson
description: Użyj tożsamości z aplikacją ASP.NET Core. Dowiedz się, jak ustawiać wymagania dotyczące haseł (RequireDigit, RequiredLength, RequiredUniqueChars itd.).
ms.author: riande
ms.date: 03/26/2019
uid: security/authentication/identity
ms.openlocfilehash: 6701eb0ac1b1abb8699a5a529bcc29f295e5c7c9
ms.sourcegitcommit: 4115bf0e850c13d4e655beb5ab5e8ff431173cb6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/06/2019
ms.locfileid: "71981909"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Wprowadzenie do tożsamości na ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core Identity to system członkostwa, który dodaje funkcje logowania do ASP.NET Core aplikacji. Użytkownicy mogą utworzyć konto z informacjami logowania przechowywanymi w tożsamości lub korzystać z zewnętrznego dostawcy logowania. Obsługiwane zewnętrzne dostawcy logowania obejmują [serwis Facebook, Google, konto Microsoft i Twitter](xref:security/authentication/social/index).

Tożsamość można skonfigurować przy użyciu bazy danych SQL Server do przechowywania nazw użytkowników, haseł i danych profilu. Alternatywnie można użyć innego magazynu trwałego, na przykład Azure Table Storage.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([jak pobrać)](xref:index#how-to-download-a-sample)).

W tym temacie dowiesz się, jak używać tożsamości do rejestrowania, logowania i wylogowywania użytkownika. Aby uzyskać bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji korzystających z tożsamości, zobacz sekcję następne kroki na końcu tego artykułu.

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a>AddDefaultIdentity i AddIdentity

[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) został wprowadzony w ASP.NET Core 2,1. Wywołanie `AddDefaultIdentity` jest podobne do wywołania następujących:

* [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [AddDefaultUI](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [AddDefaultTokenProviders](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

Aby uzyskać więcej informacji, zobacz [Źródło AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a>Tworzenie aplikacji sieci Web z uwierzytelnianiem

Utwórz projekt aplikacji sieci Web ASP.NET Core przy użyciu poszczególnych kont użytkowników.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Wybierz pozycję **plik** > **Nowy** > **projektu**.
* Wybierz **aplikacji sieci Web platformy ASP.NET Core**. Nazwij projekt **WebApp1** tak, aby miał tę samą przestrzeń nazw co pobieranie projektu. Kliknij przycisk **OK**.
* Wybierz **aplikację sieci Web**ASP.NET Core, a następnie wybierz pozycję **Zmień uwierzytelnianie**.
* Wybierz pozycję **indywidualne konta użytkowników** , a następnie kliknij przycisk **OK**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

Wygenerowany projekt zapewnia [ASP.NET Core tożsamość](xref:security/authentication/identity) jako [bibliotekę klas Razor](xref:razor-pages/ui-class). Biblioteka klas Razor tożsamość ujawnia punkty końcowe z obszarem `Identity`. Na przykład:

* /Identity/Account/Login
* /Identity/Account/Logout
* /Identity/Account/Manage

### <a name="apply-migrations"></a>Zastosuj migracje

Zastosuj migracje, aby zainicjować SFTP bazę danych.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Uruchom następujące polecenie w konsoli Menedżera pakietów (PMC):

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a>Testuj rejestr i logowanie

Uruchom aplikację i zarejestruj użytkownika. W zależności od rozmiaru ekranu może być konieczne wybranie przycisku przełącznika nawigacji, aby wyświetlić linki **rejestru** i **logowania** .

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a>Konfigurowanie usług tożsamości

Usługi są dodawane w `ConfigureServices`. Typowym wzorcem jest Wywołaj wszystkie metody `Add{Service}`, a następnie Wywołaj wszystkie metody `services.Configure{Service}`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

Poprzedni kod konfiguruje tożsamość z domyślnymi wartościami opcji. Usługi są udostępniane aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).

   Tożsamość jest włączona przez wywołanie [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_). `UseAuthentication` dodaje [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) uwierzytelniania do potoku żądania.

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Usługi są udostępniane aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).

   Tożsamość jest włączona dla aplikacji przez wywołanie `UseAuthentication` w metodzie `Configure`. `UseAuthentication` dodaje [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) uwierzytelniania do potoku żądania.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Te usługi są udostępniane aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).

   Tożsamość jest włączona dla aplikacji przez wywołanie `UseIdentity` w metodzie `Configure`. `UseIdentity` dodaje [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) uwierzytelniania opartego na plikach cookie do potoku żądania.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

Aby uzyskać więcej informacji, zobacz [IdentityOptions klasy](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) i [uruchamiania aplikacji](xref:fundamentals/startup).

## <a name="scaffold-register-login-and-logout"></a>Rejestrowanie, logowanie i wylogowywanie szkieletu

Postępuj zgodnie z informacjami o [tożsamości szkieletowej w projekcie Razor z instrukcjami autoryzacji](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) w celu wygenerowania kodu pokazanego w tej sekcji.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Dodaj pliki rejestru, logowania i wylogowywania.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Jeśli utworzono projekt o nazwie **WebApp1**, uruchom następujące polecenia. W przeciwnym razie użyj prawidłowej przestrzeni nazw dla `ApplicationDbContext`:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

Program PowerShell używa średnika jako separatora poleceń. W przypadku korzystania z programu PowerShell wpisz średniki na liście plików lub Umieść listę plików w podwójnym cudzysłowie, jak pokazano w powyższym przykładzie.

---

### <a name="examine-register"></a>Badaj rejestr

::: moniker range=">= aspnetcore-2.1"

   Gdy użytkownik kliknie łącze **zarejestruj** , zostanie wywołana akcja `RegisterModel.OnPostAsync`. Użytkownik jest tworzony przez wartość [IsAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) w obiekcie `_userManager`. `_userManager` jest zapewniona przez iniekcję zależności):

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   Gdy użytkownik kliknie link **register** , Akcja `Register` jest wywoływana w `AccountController`. Akcja `Register` tworzy użytkownika przez wywołanie `CreateAsync` w obiekcie `_userManager` (dostarczone do `AccountController` według iniekcji zależności):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   Jeśli użytkownik został utworzony pomyślnie, użytkownik jest zalogowany przez wywołanie `_signInManager.SignInAsync`.

   **Uwaga:** Zobacz [potwierdzenie konta](xref:security/authentication/accconfirm#prevent-login-at-registration) , aby zapobiec natychmiastowemu logowaniu przy rejestracji.

### <a name="log-in"></a>Zaloguj się

::: moniker range=">= aspnetcore-2.1"

Formularz logowania jest wyświetlany, gdy:

* Wybrano łącze **Zaloguj** .
* Użytkownik próbuje uzyskać dostęp do strony z ograniczeniami, do której nie ma uprawnień dostępu **lub** kiedy nie został uwierzytelniony przez system.

Po przesłaniu formularza na stronie logowania zostaje wywołana akcja `OnPostAsync`. `PasswordSignInAsync` jest wywoływana dla obiektu `_signInManager` (dostarczone przez iniekcję zależności).

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   Klasa Base `Controller` uwidacznia Właściwość `User`, do której można uzyskać dostęp z metod kontrolera. Na przykład można wyliczyć `User.Claims` i podejmować decyzje dotyczące autoryzacji. Aby uzyskać więcej informacji, zobacz <xref:security/authorization/introduction>.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Formularz logowania jest wyświetlany, gdy użytkownik wybierze link **Zaloguj** lub przekierowywać podczas uzyskiwania dostępu do strony wymagającej uwierzytelnienia. Gdy użytkownik prześle formularz na stronie logowania, zostanie wywołana akcja `AccountController` `Login`.

Akcja `Login` wywołuje `PasswordSignInAsync` w obiekcie `_signInManager` (dostarczonym do `AccountController` przez iniekcję zależności).

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

Klasa Base (`Controller` lub `PageModel`) uwidacznia Właściwość `User`. Na przykład `User.Claims` można wyliczyć w celu podejmowania decyzji dotyczących autoryzacji.

::: moniker-end

### <a name="log-out"></a>Wyloguj się

::: moniker range=">= aspnetcore-2.1"

Link **Wyloguj** wywołuje akcję `LogoutModel.OnPost`. 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) czyści oświadczenia użytkownika przechowywane w pliku cookie.

Wpis jest określony na *stronie/Shared/_LoginPartial. cshtml*:

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   Kliknięcie linku **Wyloguj** wywołuje akcję `LogOut`.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   Poprzedni kod wywołuje metodę `_signInManager.SignOutAsync`. Metoda `SignOutAsync` czyści oświadczenia użytkownika przechowywane w pliku cookie.

::: moniker-end

## <a name="test-identity"></a>Testuj tożsamość

Domyślne szablony projektu sieci Web umożliwiają anonimowy dostęp do stron głównych. Aby przetestować tożsamość, Dodaj [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) do strony prywatność.

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

Jeśli użytkownik jest zalogowany, Wyloguj się. Uruchom aplikację i wybierz łącze **prywatność** . Nastąpi przekierowanie do strony logowania.

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a>Eksploruj tożsamość

Aby poznać tożsamość w bardziej szczegółowy sposób:

* [Utwórz źródło interfejsu użytkownika pełnej tożsamości](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* Przejrzyj Źródło poszczególnych stron i przechodzenie przez debuger.

::: moniker-end

## <a name="identity-components"></a>Składniki tożsamości

::: moniker range=">= aspnetcore-2.1"

Wszystkie pakiety NuGet zależne od tożsamości są zawarte w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

::: moniker-end

Podstawowym pakietem tożsamości jest [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/). Ten pakiet zawiera podstawowy zestaw interfejsów dla ASP.NET Core Identity i jest dołączany do `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

## <a name="migrating-to-aspnet-core-identity"></a>Migrowanie do ASP.NET Core Identity

Aby uzyskać więcej informacji i wskazówek dotyczących migrowania istniejącego magazynu tożsamości, zobacz [Migrowanie uwierzytelniania i tożsamości](xref:migration/identity).

## <a name="setting-password-strength"></a>Ustawianie siły hasła

Zobacz [Konfiguracja](#pw) dla przykładu, który ustawia minimalne wymagania dotyczące hasła.

## <a name="next-steps"></a>Następne kroki

* [Konfigurowanie tożsamości](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
