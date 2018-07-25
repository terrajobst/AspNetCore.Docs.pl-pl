# <a name="cookie-sharing-sample-app"></a>Plik cookie udostępnianie przykładowej aplikacji

Przykład ilustruje udostępnianie między trzy aplikacje, które korzystają z plików cookie uwierzytelniania plików cookie:

| Projekt                             | Opis |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | ASP.NET Core Razor strony aplikacji bez używania tożsamości platformy ASP.NET Core |
| CookieAuthWithIdentity.Core         | Aplikacja MVC platformy ASP.NET Core za pomocą tożsamości platformy ASP.NET Core |
| CookieAuthWithIdentity.NETFramework | Aplikacja MVC platformy ASP.NET Framework w produkcie ASP.NET Identity |

Instrukcje:

1. Uruchom aplikację CookieAuth.Core. Zarejestruj użytkownika. Aplikacja uwierzytelnia użytkownika, gdy użytkownik jest zarejestrowany. Wylogować użytkownika.
1. W tej samej sesji przeglądarki Uruchom aplikację CookieAuthWithIdentity.Core. Zarejestruj tego samego użytkownika jako używane przy użyciu aplikacji podstawowej. Aplikacja uwierzytelnia użytkownika, gdy użytkownik jest zarejestrowany. Wylogować użytkownika.
1. W tej samej sesji przeglądarki Uruchom aplikację CookieAuthWithIdentity.NETFramework. Zarejestruj tego samego użytkownika, jak używane w innych aplikacjach. Aplikacja uwierzytelnia użytkownika, gdy użytkownik jest zarejestrowany. Wylogować użytkownika.
1. Zaloguj się użytkownika do dowolnego z trzech aplikacji. Plik cookie uwierzytelniania jest współużytkowany przez aplikacje. Należy pamiętać, że użytkownik jest automatycznie zalogowany do innych aplikacji.
1. Wyloguj użytkowników pochodzących z dowolnych aplikacji. Należy pamiętać, że użytkownik jest automatycznie wylogowani z innych aplikacji.

W tym przykładzie przedstawiono funkcje opisane w [udostępnianie plików cookie między aplikacjami](https://docs.microsoft.com/aspnet/core/security/cookie-sharing) tematu.
