# <a name="aspnet-core-response-caching-sample"></a>Przykład buforowanie odpowiedzi platformy ASP.NET Core

W tym przykładzie pokazano sposób użycia platformy ASP.NET Core [oprogramowanie pośredniczące buforowania odpowiedzi](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).

Aplikacja odpowiada za pomocą jego stronę indeksu w tym `Cache-Control` nagłówek, aby skonfigurować działanie buforowania. Aplikacja ustawia również `Vary` nagłówka, aby skonfigurować pamięć podręczną, aby obsługiwać odpowiedź tylko wtedy, gdy `Accept-Encoding` nagłówka kolejne żądania jest zgodna z wersją z oryginalnego żądania.

Podczas uruchamiania przykładu, strony indeksu są dostarczane z pamięci podręcznej, gdy przechowywanych w pamięci podręcznej i maksymalnie 10 sekund.
