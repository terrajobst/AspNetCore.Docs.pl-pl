# <a name="additional-claims-sample-app"></a>Dodatkowa Przykładowa aplikacja oświadczeń

Przykładowa aplikacja pokazuje, jak:

* Uzyskaj imię i nazwisko użytkownika od firmy Google i Zapisz oświadczenia dotyczące nazw za pomocą wartości dostarczonych przez firmę Google.
* Zapisz token dostępu Google w @no__t użytkownika — 0.

Aby skorzystać z przykładowej aplikacji:

1. Zarejestruj aplikację i uzyskaj prawidłowy identyfikator klienta i klucz tajny klienta na potrzeby uwierzytelniania Google. Aby uzyskać więcej informacji, zobacz [Konfiguracja logowania zewnętrznego Google](https://docs.microsoft.com/aspnet/core/security/authentication/social/google-logins).
1. Podaj identyfikator klienta i klucz tajny klienta do aplikacji w [GoogleOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) `Startup.ConfigureServices`.
1. Uruchom aplikację i zażądaj strony Moje oświadczenia. Gdy użytkownik nie jest zalogowany, aplikacja przekieruje się do firmy Google. Zaloguj się przy użyciu usługi Google. Firma Google przekierowuje użytkownika z powrotem do aplikacji (`/MyClaims`). Użytkownik jest uwierzytelniany, a Strona Moje oświadczenia zostanie załadowana. Podane oświadczenia nazwy i nazwiska są obecne w obszarze **oświadczenia użytkownika** z wartościami podanymi przez firmę Google. Token dostępu jest wyświetlany w obszarze **Właściwości uwierzytelniania**.
