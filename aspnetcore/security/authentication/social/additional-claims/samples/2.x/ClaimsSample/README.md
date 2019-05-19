# <a name="additional-claims-sample-app"></a>Dodatkowe oświadczenia przykładowej aplikacji

Przykładowa aplikacja pokazuje, jak:

* Uzyskaj imię i nazwisko użytkownika z usługi Google i przechowywać oświadczenia nazwy przy użyciu wartości podanych przez firmę Google.
* Store Google token dostępu użytkownika `AuthenticationProperties`.

Aby użyć przykładowej aplikacji:

1. Rejestrowanie aplikacji i uzyskać prawidłowy identyfikator klienta oraz klucz tajny klienta dla uwierzytelniania serwisu Google. Aby uzyskać więcej informacji, zobacz [ustawienia logowania zewnętrznego Google](https://docs.microsoft.com/aspnet/core/security/authentication/social/google-logins).
1. Podanie Identyfikatora klienta oraz klucz tajny klienta aplikacji w [GoogleOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) z `Startup.ConfigureServices`.
1. Uruchom aplikację i żądania na stronie Moje oświadczeń. Gdy użytkownik nie jest zalogowany, aplikacja przekierowuje do firmy Google. Zaloguj się przy użyciu Google. Google przekierowuje użytkownika do aplikacji (`/MyClaims`). Użytkownik jest uwierzytelniony, a na stronie Moje oświadczeń jest ładowany. Imię i nazwisko, oświadczenia są obecne w obszarze **oświadczenia użytkownika** przy użyciu wartości podanych przez firmę Google. Token dostępu zostanie wyświetlony w obszarze **właściwości uwierzytelniania**.
