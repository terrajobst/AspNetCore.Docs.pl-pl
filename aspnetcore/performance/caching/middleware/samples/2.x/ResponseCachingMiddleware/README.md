# <a name="aspnet-core-response-caching-sample"></a>Przykład buforowanie odpowiedzi platformy ASP.NET Core

W tym przykładzie pokazano sposób użycia platformy ASP.NET Core [oprogramowanie pośredniczące buforowania odpowiedzi](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).

Aplikacja odpowiada za pomocą jego stronę indeksu w tym `Cache-Control` nagłówek, aby skonfigurować działanie buforowania. Aplikacja ustawia również `Vary` nagłówka, aby skonfigurować pamięć podręczną, aby obsługiwać odpowiedź tylko wtedy, gdy `Accept-Encoding` nagłówka kolejne żądania jest zgodna z wersją z oryginalnego żądania.

Podczas uruchamiania przykładu, strony indeksu są dostarczane z pamięci podręcznej, gdy przechowywanych w pamięci podręcznej i maksymalnie 10 sekund.

Aby przetestować zachowanie buforowania:

* Nie należy używać przeglądarki do testowania zachowania buforowania. Przeglądarki często Dodaj nagłówek sterowania pamięcią podręczną po ponownym załadowaniu uniemożliwiające oprogramowania pośredniczącego z wyświetleniem strony pamięci podręcznej. Adapterem `Cache-Control` nagłówek, wartością `max-age=0`) mogą być dodawane przez przeglądarkę.
* Użycie narzędzia dla deweloperów, które pozwala na ustawianie nagłówków żądań jawnie, takich jak <a href="https://www.telerik.com/fiddler">Fiddler</a> lub <a href="https://www.getpostman.com/">Postman</a>.
