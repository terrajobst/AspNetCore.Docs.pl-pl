Strona indeks (*wwwroot/index.html*) zawiera skrypt definiujący `AuthenticationService` w języku JavaScript. `AuthenticationService` obsługuje szczegóły niskiego poziomu protokołu OIDC. Aplikacja wewnętrznie wywołuje metody zdefiniowane w skrypcie w celu wykonywania operacji uwierzytelniania.

```html
<script src="_content/Microsoft.Authentication.WebAssembly.Msal/
    AuthenticationService.js"></script>
```
