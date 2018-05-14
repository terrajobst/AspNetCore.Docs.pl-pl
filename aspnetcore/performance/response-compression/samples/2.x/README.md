# <a name="response-compression-sample-application-aspnet-core-2x"></a>Aplikacja przykładowa kompresji odpowiedzi (platformy ASP.NET Core 2.x)

Ten przykład przedstawia użycie platformy ASP.NET Core 2.x kompresji odpowiedzi HTTP dla, oprogramowanie pośredniczące kompresji odpowiedzi. Próbka przedstawia Gzip i dostawców niestandardowych kompresji odpowiedzi tekstowych i obrazów i pokazuje, jak dodać typ MIME kompresji. Dla przykładu 1.x platformy ASP.NET Core, zobacz [aplikacji przykładowej kompresji odpowiedzi (platformy ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/1.x).

## <a name="examples-in-this-sample"></a>Przykłady w tym przykładzie
* `GzipCompressionProvider`
  * `text/plain`
    * **/**-Lorem Ipsum tekstu pliku odpowiedzi w 2,044 bajtów, które skompresuje 927 bajtów
    * **/testfile1kb.txt** -tekstu pliku odpowiedzi w 1,033 bajtów, które skompresuje 47 bajtów
    * **/ ścieknie** -wystawione jako pojedynczy znaki w drugim 1 w odstępach czasu odpowiedzi 
  * `image/svg+xml`
    * **/Banner.SVG** -A Scalable wektor grafiki SVG obrazu odpowiedzi na 9,707 bajtów, które skompresuje 4,459 bajtów
* `CustomCompressionProvider`<br>Przedstawia sposób implementowania dostawcy kompresji niestandardowych do użytku z oprogramowaniem pośredniczącym

Jeśli żądanie zawiera `Accept-Encoding` kompresji nagłówka i odpowiedzi zakończy się pomyślnie, oprogramowanie pośredniczące automatycznie dodaje `Vary: Accept-Encoding` nagłówka do odpowiedzi. `Vary` Nagłówka powoduje, że pamięć podręczną do obsługi wielu kopii odpowiedzi na podstawie wartości alternatywnej `Accept-Encoding`, więc zarówno skompresowane (gzip) i wersji nieskompresowanych są przechowywane w pamięci podręcznej dla systemów, które może przyjąć skompresowany lub bez kompresji odpowiedzi.

## <a name="using-the-sample"></a>Przy użyciu próbki
1. Upewnij się, żądania przy użyciu [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), lub [Postman](https://www.getpostman.com/) do aplikacji bez `Accept-Encoding` nagłówka i Uwaga ładunku odpowiedzi, rozmiar odpowiedzi i nagłówki odpowiedzi.
2. Dodaj `Accept-Encoding: gzip` nagłówka i zanotuj rozmiar skompresowanych odpowiedzi i nagłówkami odpowiedzi. Rozmiar odpowiedzi spadnie i `Content-Encoding: gzip` nagłówka odpowiedzi jest dołączony przez oprogramowanie pośredniczące. Podczas przeglądania treść odpowiedzi dla Lorem Ipsum lub **testfile1kb.txt** odpowiedzi, zostanie wyświetlony czy tekst jest skompresowany i nie można go odczytać.
3. Dodaj `Accept-Encoding: mycustomcompression` nagłówka i zanotuj nagłówków odpowiedzi. `CustomCompressionProvider` Jest pusty implementację, która faktycznie nie Kompresuj odpowiedzi, ale można utworzyć strumienia kompresji niestandardowych otokę dla `CreateStream()` metody.
