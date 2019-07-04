---
title: Facebook, Google i zewnętrznego dostawcy uwierzytelniania bez tożsamości platformy ASP.NET Core
author: rick-anderson
description: Omówienie usługi Facebook, Google, Twitter, itp. konto użytkownika uwierzytelniania bez tożsamości platformy ASP.NET Core.
ms.author: riande
ms.date: 07/04/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 1e7124e8b07c0faf2d005ec3ef55c0414a697d64
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561565"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a>Użyj uwierzytelniania społecznościowych dostawcy logowania bez tożsamości platformy ASP.NET Core

<xref:security/authentication/social/index> w tym artykule opisano, jak umożliwić użytkownikom na logowanie przy użyciu protokołu OAuth 2.0 przy użyciu poświadczeń z dostawcy uwierzytelniania zewnętrznego. Podejście opisane w tym temacie zawiera tożsamości platformy ASP.NET Core jako dostawcy uwierzytelniania.

W tym przykładzie przedstawiono sposób użycia dostawcy uwierzytelniania zewnętrznych **bez** tożsamości platformy ASP.NET Core. Jest to przydatne w przypadku aplikacji, które nie wymagają wszystkich funkcji tożsamości platformy ASP.NET Core, ale nadal wymagają integracji z zaufanego zewnętrznego dostawcę uwierzytelniania.

W tym przykładzie użyto [uwierzytelniania serwisu Google](xref:security/authentication/google-logins) do uwierzytelniania użytkowników. Za pomocą Google uwierzytelniania przenosi wiele złożoności zarządzania proces logowania do usługi Google. Aby zintegrować przy użyciu innego zewnętrznego dostawcę uwierzytelniania, zobacz następujące tematy:

* [Uwierzytelnianie przy użyciu usługi Facebook](xref:security/authentication/facebook-logins)
* [Uwierzytelnianie firmy Microsoft](xref:security/authentication/microsoft-logins)
* [Uwierzytelnianie przy użyciu usługi Twitter](xref:security/authentication/twitter-logins)
* [Inni dostawcy](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Konfiguracja

W `ConfigureServices` metody, skonfiguruj schematów uwierzytelniania aplikacji za pomocą `AddAuthentication`, `AddCookie` i `AddGoogle` metody:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

Wywołanie [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) ustawia aplikacji [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme). `DefaultScheme` Jest domyślny schemat używany przez następujące `HttpContext` metody rozszerzenia dla uwierzytelniania:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Ustawienie aplikacji `DefaultScheme` do [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("plików cookie" ") umożliwia skonfigurowanie aplikacji na używanie plików cookie jako domyślny schemat dotyczącej tych metod rozszerzenia. Ustawienie aplikacji <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> do [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") umożliwia skonfigurowanie aplikacji do użytku jako domyślny schemat wywołania Google `ChallengeAsync`. `DefaultChallengeScheme` zastępuje `DefaultScheme`. Zobacz <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> dodatkowe właściwości, które zastępują `DefaultScheme` po ustawieniu.

W `Configure` metody, wywołanie `UseAuthentication` metodę do wywołania, oprogramowanie pośredniczące uwierzytelniania, która ustawia `HttpContext.User` właściwości. Wywołaj `UseAuthentication` metoda przed wywołaniem `UseMvcWithDefaultRoute` lub `UseMvc`:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

Aby dowiedzieć się więcej na temat uwierzytelniania plików cookie i schematy uwierzytelniania, zobacz <xref:security/authentication/cookie>.

## <a name="applying-authorization"></a>Stosowanie autoryzacji

Testowanie konfigurację uwierzytelniania aplikacji, stosując `AuthorizeAttribute` atrybutu kontrolerze, akcji lub strony. Poniższy kod ogranicza dostęp do *zachowania* strony do użytkowników uwierzytelnionych:

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Wyloguj

Aby się wylogować bieżącego użytkownika i usuń ich plików cookie, należy wywołać [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0). Poniższy kod dodaje `Logout` obsługi strony ma *indeksu* strony:

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

Należy zauważyć, że wywołanie `SignOutAsync` nie określono schematu uwierzytelniania. Aplikacja `DefaultScheme` z `CookieAuthenticationDefaults.AuthenticationScheme` jest używany jako bazowy.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>
