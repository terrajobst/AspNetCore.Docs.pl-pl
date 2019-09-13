# <a name="blazor-webassembly-sample-app"></a>Przykładowa aplikacja Blazor webassembly

Ten przykład ilustruje użycie scenariuszy Blazor opisanych w dokumentacji Blazor.

## <a name="call-web-api-example"></a>Przykład wywołania internetowego interfejsu API

Przykład internetowego interfejsu API wymaga działającego internetowego interfejsu API na podstawie przykładowej aplikacji dla <a href="https://docs.microsoft.com/aspnet/core/tutorials/first-web-api">samouczka: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core</a> MVC. Przykładowa aplikacja wysyła żądania do internetowego interfejsu API pod `https://localhost:10000/api/todo`adresem. Jeśli używany jest inny adres internetowego interfejsu API, należy zaktualizować `ServiceEndpoint` wartość stałą w `@functions` bloku składnika Razor.</p>

Przykładowa aplikacja wykonuje żądanie <a href="https://docs.microsoft.com/aspnet/core/security/cors">współużytkowania zasobów między źródłami (CORS)</a> z `http://localhost:5000` lub `https://localhost:5001` do internetowego interfejsu API. Poświadczenia (pliki cookie/nagłówki autoryzacji) są dozwolone. Dodaj poniższą konfigurację oprogramowania CORS do `Startup.Configure` metody internetowego interfejsu API przed wywołaniem: `UseMvc`</p>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization)
    .AllowCredentials());
```

Dostosuj domeny i porty `WithOrigins` dla aplikacji Blazor.

Internetowy interfejs API jest skonfigurowany do obsługi mechanizmu CORS, aby zezwalać na pliki cookie/nagłówki i żądania autoryzacji z kodu klienta, ale internetowy interfejs API utworzony przez samouczek nie zezwala na rzeczywiste żądania. Zapoznaj się z <a href="https://docs.microsoft.com/aspnet/core/security/">artykułami dotyczącymi ASP.NET Core Security i Identity</a> , aby uzyskać wskazówki dotyczące implementacji.
