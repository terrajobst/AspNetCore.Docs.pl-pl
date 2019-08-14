# <a name="response-compression-sample-application-aspnet-core-2x"></a>Przykładowa aplikacja do kompresji odpowiedzi (ASP.NET Core 2. x)

Ten przykład ilustruje sposób użycia oprogramowania pośredniczącego kompresji ASP.NET Core 2. x w celu kompresowania odpowiedzi HTTP. W przykładzie przedstawiono dostawców kompresji gzip, Brotli i niestandardowych dla odpowiedzi tekstowych i obrazów oraz pokazano, jak dodać typ MIME dla kompresji. Aby uzyskać przykład ASP.NET Core 1. x, zobacz [Przykładowa aplikacja do kompresji odpowiedzi (ASP.NET Core 1. x)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples/1.x).

## <a name="examples-in-this-sample"></a>Przykłady w tym przykładzie

* `BrotliCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum plik tekstowy odpowiedzi o 2 044 bajtów, który kompresuje do ~ 979 bajtów.
    * **/testfile1kb.txt** — odpowiedź na plik tekstowy o 1 033 bajtów, która kompresuje do ~ 36 bajtów.
    * **/Trickle** — odpowiedź wystawiona jako pojedyncze znaki w 1-sekundowym odstępie czasu.
* `GzipCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum plik tekstowy odpowiedzi o 2 044 bajtów, który kompresuje do ~ 927 bajtów.
    * **/testfile1kb.txt** — odpowiedź na plik tekstowy o 1 033 bajtów, która kompresuje do ~ 47 bajtów.
    * **/Trickle** — odpowiedź wystawiona jako pojedyncze znaki w 1-sekundowym odstępie czasu.
  * `image/svg+xml`
    * **/banner.SVG** — odpowiedź obrazu skalowalnej grafiki wektorowej (SVG) o 9 707 bajtów, która kompresuje do ~ 4 459 bajtów.
* `CustomCompressionProvider`<br>Pokazuje, jak zaimplementować niestandardowego dostawcę kompresji do użytku z programem pośredniczącym.

Gdy żądanie zawiera `Accept-Encoding` pomyślnie kompresję nagłówka i odpowiedzi, oprogramowanie pośredniczące automatycznie `Vary: Accept-Encoding` dodaje nagłówek do odpowiedzi. Nagłówek instruuje pamięć podręczną `Accept-Encoding`, aby zachować wiele kopii odpowiedzi na podstawie alternatywnych wartości, dlatego w pamięci podręcznej są przechowywane skompresowane (gzip lub Brotli) oraz nieskompresowana wersje dla systemów, które mogą zaakceptować `Vary` skompresowana lub nieskompresowana odpowiedź.

## <a name="use-the-sample"></a>Użyj przykładu

1. Wyślij żądanie przy użyciu [programu Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)lub [post](https://www.getpostman.com/) `Accept-Encoding` do aplikacji bez nagłówka i zanotuj obciążenie odpowiedzi, rozmiar odpowiedzi i nagłówki odpowiedzi.
1. Dodaj nagłówek `Accept-Encoding: gzip` lub i zanotuj nagłówki rozmiaru i odpowiedzi skompresowanej odpowiedzi. `Accept-Encoding: br` Rozmiar odpowiedzi spadnie, a `Content-Encoding` nagłówek odpowiedzi jest dołączany przez oprogramowanie pośredniczące wskazujące, że wystąpił błąd kompresji z gzip lub Brotli. Gdy przeszukiwana jest treść odpowiedzi dla odpowiedzi Lorem Ipsum lub **testfile1kb. txt** , zobaczysz, że tekst jest skompresowany i nie można go odczytać.
1. `Accept-Encoding: mycustomcompression` Dodaj nagłówek i zanotuj nagłówki odpowiedzi. Jest to pusta implementacja, która w rzeczywistości nie kompresuje odpowiedzi, ale można utworzyć niestandardową otokę strumienia kompresji `CreateStream()` dla metody. `CustomCompressionProvider`
