# <a name="aspnet-core-response-caching-sample"></a>Przykład buforowania odpowiedzi ASP.NET Core

Ten przykład ilustruje sposób użycia [oprogramowania pośredniczącego buforowania odpowiedzi](https://docs.microsoft.com/aspnet/core/performance/caching/middleware)ASP.NET Core.

Aplikacja odpowiada za pomocą strony indeksu, w tym nagłówka `Cache-Control`, aby skonfigurować zachowanie buforowania. Aplikacja ustawia również nagłówek `Vary`, aby skonfigurować pamięć podręczną, aby obsługiwała odpowiedź tylko wtedy, gdy nagłówek `Accept-Encoding` kolejnych żądań jest zgodny z oryginalnym żądaniem.

Podczas uruchamiania przykładu strona indeksu jest obsługiwana z pamięci podręcznej, gdy jest przechowywana i buforowana przez maksymalnie 10 sekund.

Aby przetestować zachowanie buforowania:

* Nie używaj przeglądarki do testowania zachowania buforowania. Przeglądarki często dodają nagłówek kontroli pamięci podręcznej po ponownym załadowaniu, który uniemożliwia programowi pośredniczącemu obsługę buforowanej strony. Na przykład nagłówek `Cache-Control` o wartości `max-age=0`) może zostać dodany przez przeglądarkę.
* Użyj narzędzia deweloperskiego, które umożliwia jawne ustawienie nagłówków żądań, takich jak <a href="https://www.telerik.com/fiddler">programu Fiddler</a> lub <a href="https://www.getpostman.com/">Poster</a>.
