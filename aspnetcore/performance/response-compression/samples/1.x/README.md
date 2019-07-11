# <a name="response-compression-sample-application-aspnet-core-1x"></a>Aplikacja przykładowa kompresji odpowiedzi (Platforma ASP.NET Core 1.x)

Ten przykład ilustruje sposób używania programu ASP.NET Core 1.x oprogramowanie pośredniczące kompresji odpowiedzi, aby kompresowały odpowiedzi HTTP dla. Próbka pokazuje Gzip i dostawców niestandardowych kompresji odpowiedzi tekstu i obrazów i pokazuje, jak dodać typ MIME dla kompresji. Dla platformy ASP.NET Core 2.x przykładu, zobacz [aplikacji przykładowej kompresji odpowiedzi (Platforma ASP.NET Core 2.x)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x).

## <a name="examples-in-this-sample"></a>Przykłady w tym przykładzie

* `GzipCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum tekst pliku odpowiedzi 2,044 bajtów, które skompresuje 927 bajtów
    * **/testfile1kb.txt** — tekst pliku odpowiedzi 1,033 bajtów, które skompresuje 47 bajtów
    * **/ ścieknie** -wystawione jako pojedyncze znaki w 1, drugi odstępach czasu odpowiedzi
  * `image/svg+xml`
    * **/Banner.SVG** -9,707 bajtów, które skompresuje 4,459 bajtów odpowiedzi obraz grafiki wektorowej skalowalne (SVG)
* `CustomCompressionProvider`<br>Pokazuje, jak do implementowania dostawcy kompresji niestandardowych do użytku z oprogramowaniem pośredniczącym

Jeśli żądanie zawiera `Accept-Encoding` nagłówka Przykładowa aplikacja dodaje `Vary: Accept-Encoding` nagłówka odpowiedzi. `Vary` Pamięci podręcznych do obsługi wielu kopii odpowiedzi na podstawie alternatywne wartości powoduje, że nagłówek `Accept-Encoding`, więc skompresowany (gzip) i wersji nieskompresowanych są przechowywane w pamięci podręczne, w przypadku systemów, które mogą akceptować skompresowany lub bez kompresji odpowiedzi.

## <a name="using-the-sample"></a>Przy użyciu przykładu

1. Upewnij się, żądanie, używając [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), lub [Postman](https://www.getpostman.com/) do aplikacji bez `Accept-Encoding` nagłówka i zanotuj ładunek odpowiedzi rozmiar odpowiedzi i nagłówki odpowiedzi.
1. Dodaj `Accept-Encoding: gzip` nagłówka i zwróć uwagę, Rozmiar skompresowanych odpowiedzi i nagłówkami odpowiedzi. Zostanie wyświetlony rozmiar odpowiedzi, porzucić i `Content-Encoding: gzip` nagłówek odpowiedzi znajduje się za pomocą aplikacji przykładowej. Patrząc na treść odpowiedzi dla Lorem Ipsum lub **testfile1kb.txt** odpowiedzi, możesz zobaczyć, że tekst jest skompresowany i nie można go odczytać.
1. Dodaj `Accept-Encoding: mycustomcompression` nagłówka i zanotuj nagłówki odpowiedzi. `CustomCompressionProvider` Jest pusty implementacji, które faktycznie nie Kompresuj odpowiedzi, ale można utworzyć otokę strumienia niestandardowe kompresji dla `CreateStream()` metody.
