# <a name="response-compression-sample-application-aspnet-core-2x"></a>Przykładowa aplikacja do kompresji odpowiedzi (ASP.NET Core 2. x)

Ten przykład ilustruje sposób użycia oprogramowania pośredniczącego kompresji ASP.NET Core 2. x w celu kompresowania odpowiedzi HTTP. W przykładzie przedstawiono dostawców kompresji gzip, Brotli i niestandardowych dla odpowiedzi tekstowych i obrazów oraz pokazano, jak dodać typ MIME dla kompresji. Aby uzyskać przykład ASP.NET Core 1. x, zobacz [Przykładowa aplikacja do kompresji odpowiedzi (ASP.NET Core 1. x)](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples/1.x).

## <a name="examples-in-this-sample"></a>Przykłady w tym przykładzie

* `BrotliCompressionProvider`
  * `text/plain`
    * **/** — odpowiedź na plik tekstowy Lorem Ipsum o 2 044 bajtów, która kompresuje do ~ 979 bajtów.
    * **/testfile1kb.txt** — odpowiedź na plik tekstowy o 1 033 bajtów, która kompresuje do ~ 36 bajtów.
    * **/Trickle** — odpowiedź wystawiona jako pojedyncze znaki w 1-sekundowym odstępie czasu.
* `GzipCompressionProvider`
  * `text/plain`
    * **/** — odpowiedź na plik tekstowy Lorem Ipsum o 2 044 bajtów, która kompresuje do ~ 927 bajtów.
    * **/testfile1kb.txt** — odpowiedź na plik tekstowy o 1 033 bajtów, która kompresuje do ~ 47 bajtów.
    * **/Trickle** — odpowiedź wystawiona jako pojedyncze znaki w 1-sekundowym odstępie czasu.
  * `image/svg+xml`
    * **/banner.SVG** — odpowiedź obrazu skalowalnej grafiki wektorowej (SVG) o 9 707 bajtów, która kompresuje do ~ 4 459 bajtów.
* `CustomCompressionProvider`<br>Pokazuje, jak zaimplementować niestandardowego dostawcę kompresji do użytku z programem pośredniczącym.

Gdy żądanie zawiera nagłówek `Accept-Encoding` i kompresja odpowiedzi powiedzie się, oprogramowanie pośredniczące automatycznie dodaje nagłówek `Vary: Accept-Encoding` do odpowiedzi. `Vary` nagłówku instruuje pamięć podręczną, aby zachować wiele kopii odpowiedzi na podstawie alternatywnych wartości `Accept-Encoding`, tak więc zarówno skompresowana, jak i nieskompresowana wersje są przechowywane w pamięci podręcznej dla systemów, które mogą akceptować skompresowane lub nieskompresowane odpowiedzi.

## <a name="use-the-sample"></a>Użyj przykładu

1. Utwórz żądanie przy użyciu [programu Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)lub [Poster](https://www.getpostman.com/) do aplikacji bez nagłówka `Accept-Encoding` i zanotuj ładunek odpowiedzi, rozmiar odpowiedzi i nagłówki odpowiedzi.
1. Dodaj nagłówek `Accept-Encoding: br` lub `Accept-Encoding: gzip` i zanotuj rozmiar skompresowanej odpowiedzi i nagłówki odpowiedzi. Rozmiar odpowiedzi spadnie, a nagłówek odpowiedzi `Content-Encoding` jest dołączany przez oprogramowanie pośredniczące wskazujące, że kompresja z użyciem gzip lub Brotli. Gdy przeszukiwana jest treść odpowiedzi dla odpowiedzi Lorem Ipsum lub **testfile1kb. txt** , zobaczysz, że tekst jest skompresowany i nie można go odczytać.
1. Dodaj nagłówek `Accept-Encoding: mycustomcompression` i zanotuj nagłówki odpowiedzi. `CustomCompressionProvider` jest pustą implementacją, która w rzeczywistości nie kompresuje odpowiedzi, ale można utworzyć niestandardową otokę strumienia kompresji dla metody `CreateStream()`.
