# <a name="aspnet-core-response-caching-sample-aspnet-core-2x"></a>Przykładowe buforowanie odpowiedzi platformy ASP.NET Core (2.x platformy ASP.NET Core)

Ten przykład przedstawia użycie platformy ASP.NET Core [buforowanie odpowiedzi w oprogramowaniu pośredniczącym](xref:performance/caching/middleware) z platformy ASP.NET Core 2.x. Dla przykładu 1.x platformy ASP.NET Core, zobacz [próbki buforowanie odpowiedzi ASP.NET Core (platformy ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples/1.x).

Aplikacja wysyła `Hello World!` komunikat i wraz z bieżącym czasem `Cache-Control` nagłówka, aby skonfigurować zachowanie buforowania. Aplikacja wysyła również `Vary` nagłówka, aby skonfigurować pamięć podręczną do obsługi odpowiedzi tylko wtedy, gdy `Accept-Encoding` nagłówka kolejne żądania jest zgodna z wersją z oryginalnego żądania.

Podczas uruchamiania próbki, odpowiedź jest udostępniany z pamięci podręcznej podczas możliwe i przechowywane do 10 sekund.
