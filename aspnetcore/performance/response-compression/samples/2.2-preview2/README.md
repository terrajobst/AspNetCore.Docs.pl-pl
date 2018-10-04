# <a name="response-compression-sample-application-aspnet-core-2x"></a>Aplikacja przykładowa kompresji odpowiedzi (Platforma ASP.NET Core 2.x)

> [!IMPORTANT]
> W przykładzie użyto zestawu SDK platformy ASP.NET Core 2.2 w wersji zapoznawczej 2 i środowiska uruchomieniowego. Uzyskiwanie wersji zapoznawczej z [pliki do pobrania platformy .NET Core 2.2](https://www.microsoft.com/net/download/dotnet-core/2.2). Upewnij się, wersja zestawu SDK w *global.json* pliku i [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) wersji są poprawne w pliku projektu dla zestawu SDK i wersję udostępnionych.

Ten przykład ilustruje sposób używania programu ASP.NET Core 2.x, aby kompresowały odpowiedzi HTTP dla, oprogramowanie pośredniczące kompresji odpowiedzi. Próbka pokazuje Gzip, Brotli i dostawców niestandardowych kompresji odpowiedzi tekstu i obrazów i pokazuje, jak dodać typ MIME dla kompresji. Dla platformy ASP.NET Core 1.x przykładu, zobacz [aplikacji przykładowej kompresji odpowiedzi (Platforma ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/1.x).

## <a name="examples-in-this-sample"></a>Przykłady w tym przykładzie

* `BrotliCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum tekst pliku odpowiedzi w bajtach 2,044 kompresuje ~ 979 bajtów.
    * **/testfile1kb.txt** — tekst pliku odpowiedzi w bajtach 1,033 kompresuje ~ 36 bajtów.
    * **/ ścieknie** -wystawione jako pojedyncze znaki w 1, drugi odstępach czasu odpowiedzi.
* `GzipCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum tekst pliku odpowiedzi w bajtach 2,044 kompresuje ~ 927 bajtów.
    * **/testfile1kb.txt** — tekst pliku odpowiedzi w bajtach 1,033 kompresuje ~ 47 bajtów.
    * **/ ścieknie** -wystawione jako pojedyncze znaki w 1, drugi odstępach czasu odpowiedzi.
  * `image/svg+xml`
    * **/Banner.SVG** -odpowiedzi obraz grafiki wektorowej skalowalne (SVG) na 9,707 bajtów, która kompresuje ~ 4,459 bajtów.
* `CustomCompressionProvider`<br>Pokazuje, jak do implementowania dostawcy kompresji niestandardowych do użytku z oprogramowaniem pośredniczącym.

Jeśli żądanie zawiera `Accept-Encoding` kompresja nagłówków i odpowiedzi zakończy się pomyślnie, oprogramowanie pośredniczące automatycznie dodaje `Vary: Accept-Encoding` nagłówka odpowiedzi. `Vary` Pamięci podręcznych do obsługi wielu kopii odpowiedzi na podstawie alternatywne wartości powoduje, że nagłówek `Accept-Encoding`, więc obie skompresowany (Gzip lub Brotli) i wersji nieskompresowanych są przechowywane w pamięci podręcznej dla systemów, które mogą akceptować skompresowany lub bez kompresji odpowiedzi.

## <a name="using-the-sample"></a>Przy użyciu przykładu

1. Upewnij się, żądanie, używając [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), lub [Postman](https://www.getpostman.com/) do aplikacji bez `Accept-Encoding` nagłówka i zanotuj ładunek odpowiedzi rozmiar odpowiedzi i nagłówki odpowiedzi.
1. Dodaj `Accept-Encoding: br` lub `Accept-Encoding: gzip` nagłówka i zwróć uwagę, Rozmiar skompresowanych odpowiedzi i nagłówkami odpowiedzi. Rozmiar odpowiedzi spadnie i `Content-Encoding` nagłówek odpowiedzi znajduje się przez oprogramowanie pośredniczące wskazująca, że kompresja za pomocą dowolnego narzędzia Gzip lub Brotli wystąpił. Patrząc na treść odpowiedzi dla Lorem Ipsum lub **testfile1kb.txt** odpowiedzi, możesz zobaczyć, że tekst jest skompresowany i nie można go odczytać.
1. Dodaj `Accept-Encoding: mycustomcompression` nagłówka i zanotuj nagłówki odpowiedzi. `CustomCompressionProvider` Jest pusty implementacji, które faktycznie nie Kompresuj odpowiedzi, ale można utworzyć otokę strumienia niestandardowe kompresji dla `CreateStream()` metody.
