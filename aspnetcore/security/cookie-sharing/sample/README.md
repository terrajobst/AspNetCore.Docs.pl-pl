# <a name="cookie-sharing-sample-app"></a>Plik cookie udostępnianie przykładowej aplikacji

Przykładową udostępnianie w trzech aplikacji, które korzystają z plików cookie uwierzytelniania pliku cookie:

| Projekt                             | Opis |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | Aplikacja ASP.NET Core 2.0 Razor strony bez użycia ASP.NET Core Identity |
| CookieAuthWithIdentity.Core         | Platformy ASP.NET Core 2.0 aplikacji MVC za pomocą tożsamości platformy ASP.NET Core |
| CookieAuthWithIdentity.NETFramework | Struktura ASP.NET 4.6.1 aplikacji MVC za pomocą tożsamości platformy ASP.NET |

Instrukcje:

1. Uruchom aplikację CookieAuth.Core. Zarejestruj użytkownika. Aplikacja uwierzytelnia użytkownika, gdy użytkownik jest zarejestrowany. Wylogowanie użytkownika.
1. W tej samej sesji przeglądarki Uruchom aplikację CookieAuthWithIdentity.Core. Rejestrowanie tego samego użytkownika jako używanych z aplikacją Core. Aplikacja uwierzytelnia użytkownika, gdy użytkownik jest zarejestrowany. Wylogowanie użytkownika.
1. W tej samej sesji przeglądarki Uruchom aplikację CookieAuthWithIdentity.NETFramework. Rejestrowanie tego samego użytkownika jako używane w innych aplikacjach. Aplikacja uwierzytelnia użytkownika, gdy użytkownik jest zarejestrowany. Wylogowanie użytkownika.
1. Zaloguj użytkownika do żadnego z trzech aplikacji. Plik cookie uwierzytelniania jest współdzielona przez aplikacje. Należy pamiętać, że użytkownik jest automatycznie zalogowany do innych aplikacji.
1. Wylogować użytkownika z dowolnej aplikacji. Należy pamiętać, że użytkownik jest automatycznie wylogowani z innych aplikacji.

W tym przykładzie pokazano funkcje opisane w [udostępniania plików cookie między aplikacjami](https://docs.microsoft.com/aspnet/core/security/cookie-sharing) tematu.
