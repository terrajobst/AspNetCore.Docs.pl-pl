# <a name="gdpr-sample"></a>Przykładowe GDPR

* W *appsettings.json*ustaw `CheckNotConsentNeeded` do `false` wymaganie zgody użytkownika; w przeciwnym razie wartość true lub pominąć. Testowanie aplikacji z `CheckNotConsentNeeded` ustawioną `false` i ustawić `true`.
* Tworzenie podstawowych i nieistotne pliki cookie z każdej wersji `CheckConsentNeeded` i udzielić zgody.
* Zarejestruj użytkownika.
* Usuń pliki cookie.
