# <a name="gdpr-sample"></a>Przykładowe RODO

* W *appsettings.json*ustaw `CheckNotConsentNeeded` do `false` wymagające zgody; w przeciwnym razie wartość true lub pominąć. Testowanie aplikacji w usłudze `CheckNotConsentNeeded` równa `false` i ustaw `true`.
* Utwórz podstawowe i inne niż niezbędne pliki cookie z każdej wersji `CheckConsentNeeded` i udzielono zgody.
* Zarejestruj użytkownika.
* Usuń pliki cookie.
