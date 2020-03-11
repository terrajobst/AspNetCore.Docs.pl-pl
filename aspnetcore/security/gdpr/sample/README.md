# <a name="gdpr-sample"></a>Przykład Rodo

* W pliku *appSettings. JSON*Ustaw `CheckNotConsentNeeded` na `false`, aby wymagać zgody; w przeciwnym razie ustaw wartość true lub Pomiń. Przetestuj aplikację, korzystając z `CheckNotConsentNeeded` ustawionej na `false` i Ustaw jako `true`.
* Twórz podstawowe i nieistotne pliki cookie z uwzględnieniem każdej zmiany `CheckConsentNeeded` i zgody.
* Zarejestruj użytkownika.
* Usuń pliki cookie.
