# <a name="aspnet-core-response-caching-sample"></a>Przykładowe buforowanie odpowiedzi platformy ASP.NET Core

Ten przykład przedstawia użycie platformy ASP.NET Core [buforowanie odpowiedzi w oprogramowaniu pośredniczącym](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).

Aplikacja odpowiada jego strony indeksu w tym `Cache-Control` nagłówka, aby skonfigurować zachowanie buforowania. Ustawia również aplikacji `Vary` nagłówka, aby skonfigurować pamięć podręczną do obsługi odpowiedzi tylko wtedy, gdy `Accept-Encoding` nagłówka kolejne żądania jest zgodna z wersją z oryginalnego żądania.

Podczas uruchamiania próbki, strona indeksu jest wyświetlona z pamięci podręcznej podczas przechowywane i pamięci podręcznej do 10 sekund.
