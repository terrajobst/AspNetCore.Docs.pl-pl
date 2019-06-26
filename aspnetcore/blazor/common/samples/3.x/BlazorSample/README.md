# <a name="blazor-client-side-sample-app"></a>Przykładowa aplikacja do Blazor (po stronie klienta)

Ten przykład ilustruje sposób używania Blazor scenariuszy opisanych w dokumentacji Blazor.

## <a name="call-web-api-example"></a>Wywołaj przykład interfejsu API sieci web

Przykład interfejsu API sieci web wymaga uruchomionej interfejsu API sieci web na przykładowej aplikacji na podstawie <a href="https://docs.microsoft.com/aspnet/core/tutorials/first-web-api">samouczka: Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core MVC</a> tematu. Przykładowa aplikacja zgłasza żądania do internetowego interfejsu API w `https://localhost:10000/api/todo`. Jeśli jest używany inny adres interfejsu API sieci web, należy zaktualizować `ServiceEndpoint` wartości stałej w składniku Razor `@functions` bloku.</p>

Przykładowa aplikacja sprawia, że <a href="https://docs.microsoft.com/aspnet/core/security/cors">współużytkowanie zasobów między źródłami (cors)</a> poprosić `http://localhost:5000` lub `https://localhost:5001` do internetowego interfejsu API. Poświadczenia (autoryzacji plików cookie. / nagłówki) są dozwolone. Dodaj następującą konfigurację oprogramowanie pośredniczące CORS do internetowego interfejsu API `Startup.Configure` metoda przed wywołaniem `UseMvc`:</p>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization)
    .AllowCredentials());
```

Dostosuj domen i porty `WithOrigins` stosownie do potrzeb aplikacji Blazor.

Interfejs API sieci web jest skonfigurowany do obsługi mechanizmu CORS zezwolić na żądania i nagłówki plików cookie autoryzacji z poziomu kodu klienta, ale internetowego interfejsu API podczas tworzenia przez samouczek faktycznie nie autoryzacji żądania. Zobacz <a href="https://docs.microsoft.com/aspnet/core/security/">artykuły platformy ASP.NET Core zabezpieczenia i tożsamość</a> uzyskać wytyczne dotyczące implementacji.
