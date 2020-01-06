---
title: Uwierzytelnianie w serwisach Facebook, Google i dostawcy zewnętrznym bez tożsamości ASP.NET Core
author: rick-anderson
description: Wyjaśnienie dotyczące korzystania z usługi Facebook, Google, Twitter itp. i uwierzytelniania użytkownika konta bez tożsamości ASP.NET Core.
ms.author: riande
ms.date: 12/10/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 612964ec9ed4975cdc81780dda3bac6cce96037f
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75359061"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a>Korzystanie z uwierzytelniania przy użyciu dostawcy logowania społecznego bez tożsamości ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

<xref:security/authentication/social/index> opisuje, jak umożliwić użytkownikom logowanie się przy użyciu protokołu OAuth 2,0 z poświadczeniami od zewnętrznych dostawców uwierzytelniania. Podejście opisane w tym temacie obejmuje ASP.NET Core tożsamość jako dostawcę uwierzytelniania.

Ten przykład pokazuje, jak używać zewnętrznego dostawcy uwierzytelniania **bez** tożsamości ASP.NET Core. Jest to przydatne w przypadku aplikacji, które nie wymagają wszystkich funkcji ASP.NET Core tożsamość, ale nadal wymagają integracji z zaufanym dostawcą uwierzytelniania zewnętrznego.

Ten przykład używa [uwierzytelniania Google](xref:security/authentication/google-logins) do uwierzytelniania użytkowników. Korzystanie z usługi Google Authentication przenosi wiele złożoności zarządzania procesem logowania do usługi Google. Aby zintegrować z innym zewnętrznym dostawcą uwierzytelniania, zapoznaj się z następującymi tematami:

* [Uwierzytelnianie przy użyciu usługi Facebook](xref:security/authentication/facebook-logins)
* [Uwierzytelnianie firmy Microsoft](xref:security/authentication/microsoft-logins)
* [Uwierzytelnianie przy użyciu usługi Twitter](xref:security/authentication/twitter-logins)
* [Inni dostawcy](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Konfiguracja

W metodzie `ConfigureServices` Skonfiguruj schematy uwierzytelniania aplikacji przy użyciu metod <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>i <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*>:

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet1)]

Wywołanie <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> ustawia <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>aplikacji. `DefaultScheme` jest domyślnym schematem używanym przez następujące metody rozszerzenia uwierzytelniania `HttpContext`:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Ustawienie `DefaultScheme` aplikacji na [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("pliki cookie") umożliwia skonfigurowanie aplikacji do używania plików cookie jako schematu domyślnego dla tych metod rozszerzających. Ustawienie <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> aplikacji na [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") spowoduje skonfigurowanie aplikacji do używania Google jako domyślnego schematu dla wywołań do `ChallengeAsync`. `DefaultChallengeScheme` zastąpień `DefaultScheme`. Zobacz <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>, aby uzyskać dodatkowe właściwości, które zastępują `DefaultScheme` po ustawieniu.

W `Startup.Configure`Wywołaj `UseAuthentication` i `UseAuthorization` między wywołaniem `UseRouting` i `UseEndpoints`. Ustawia właściwość `HttpContext.User` i uruchamia oprogramowanie pośredniczące autoryzacji dla żądań:

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet2&highlight=3-4)]

Aby dowiedzieć się więcej o schematach uwierzytelniania, zobacz [pojęcia związane z uwierzytelnianiem](xref:security/authentication/index#authentication-concepts). Aby dowiedzieć się więcej o uwierzytelnianiu plików cookie, zobacz <xref:security/authentication/cookie>.

## <a name="apply-authorization"></a>Zastosuj autoryzację

Przetestuj konfigurację uwierzytelniania aplikacji, stosując atrybut `AuthorizeAttribute` do kontrolera, akcji lub strony. Poniższy kod ogranicza dostęp do strony *prywatność* do użytkowników, którzy zostali uwierzytelnieni:

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Wyloguj

Aby wylogować bieżącego użytkownika i usunąć jego plik cookie, wywołaj [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*). Poniższy kod dodaje procedurę obsługi strony `Logout` do strony *indeks* :

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

Zwróć uwagę, że wywołanie `SignOutAsync` nie określa schematu uwierzytelniania. `DefaultScheme` aplikacji `CookieAuthenticationDefaults.AuthenticationScheme` zostanie użyta jako powracanie.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<xref:security/authentication/social/index> opisuje, jak umożliwić użytkownikom logowanie się przy użyciu protokołu OAuth 2,0 z poświadczeniami od zewnętrznych dostawców uwierzytelniania. Podejście opisane w tym temacie obejmuje ASP.NET Core tożsamość jako dostawcę uwierzytelniania.

Ten przykład pokazuje, jak używać zewnętrznego dostawcy uwierzytelniania **bez** tożsamości ASP.NET Core. Jest to przydatne w przypadku aplikacji, które nie wymagają wszystkich funkcji ASP.NET Core tożsamość, ale nadal wymagają integracji z zaufanym dostawcą uwierzytelniania zewnętrznego.

Ten przykład używa [uwierzytelniania Google](xref:security/authentication/google-logins) do uwierzytelniania użytkowników. Korzystanie z usługi Google Authentication przenosi wiele złożoności zarządzania procesem logowania do usługi Google. Aby zintegrować z innym zewnętrznym dostawcą uwierzytelniania, zapoznaj się z następującymi tematami:

* [Uwierzytelnianie przy użyciu usługi Facebook](xref:security/authentication/facebook-logins)
* [Uwierzytelnianie firmy Microsoft](xref:security/authentication/microsoft-logins)
* [Uwierzytelnianie przy użyciu usługi Twitter](xref:security/authentication/twitter-logins)
* [Inni dostawcy](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Konfiguracja

W metodzie `ConfigureServices` Skonfiguruj schematy uwierzytelniania aplikacji przy użyciu metod `AddAuthentication`, `AddCookie`i `AddGoogle`:

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet1)]

Wywołanie metody [addauthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) ustawia [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)aplikacji. `DefaultScheme` jest domyślnym schematem używanym przez następujące metody rozszerzenia uwierzytelniania `HttpContext`:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Ustawienie `DefaultScheme` aplikacji na [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("pliki cookie") umożliwia skonfigurowanie aplikacji do używania plików cookie jako schematu domyślnego dla tych metod rozszerzających. Ustawienie <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> aplikacji na [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") spowoduje skonfigurowanie aplikacji do używania Google jako domyślnego schematu dla wywołań do `ChallengeAsync`. `DefaultChallengeScheme` zastąpień `DefaultScheme`. Zobacz <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>, aby uzyskać dodatkowe właściwości, które zastępują `DefaultScheme` po ustawieniu.

W metodzie `Configure` Wywołaj metodę `UseAuthentication`, aby wywołać oprogramowanie pośredniczące uwierzytelniania, które ustawia właściwość `HttpContext.User`. Wywołaj metodę `UseAuthentication` przed wywołaniem `UseMvcWithDefaultRoute` lub `UseMvc`:

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet2)]

Aby dowiedzieć się więcej o schematach uwierzytelniania, zobacz [pojęcia związane z uwierzytelnianiem](xref:security/authentication/index#authentication-concepts). Aby dowiedzieć się więcej o uwierzytelnianiu plików cookie, zobacz <xref:security/authentication/cookie>.

## <a name="apply-authorization"></a>Zastosuj autoryzację

Przetestuj konfigurację uwierzytelniania aplikacji, stosując atrybut `AuthorizeAttribute` do kontrolera, akcji lub strony. Poniższy kod ogranicza dostęp do strony *prywatność* do użytkowników, którzy zostali uwierzytelnieni:

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Wyloguj

Aby wylogować bieżącego użytkownika i usunąć jego plik cookie, wywołaj [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*). Poniższy kod dodaje procedurę obsługi strony `Logout` do strony *indeks* :

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

Zwróć uwagę, że wywołanie `SignOutAsync` nie określa schematu uwierzytelniania. `DefaultScheme` aplikacji `CookieAuthenticationDefaults.AuthenticationScheme` zostanie użyta jako powracanie.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
