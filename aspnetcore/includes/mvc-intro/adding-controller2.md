Zastąp zawartość *Controllers/HelloWorldController.cs* następującym kodem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

Każdy `public` metody w kontrolerze jest wywoływany jako punktu końcowego HTTP. W powyższym przykładzie obu tych metod zwraca ciąg.  Należy pamiętać, komentarze poprzedzających każdej metody.

Punkt końcowy HTTP jest targetable adres URL aplikacji sieci web, takich jak `http://localhost:1234/HelloWorld`i łączy protokół używany: `HTTP`, lokalizacji sieciowej serwera sieci web (w tym z portem TCP): `localhost:1234` i docelowy identyfikator URI `HelloWorld`.

Pierwszy komentarz stany to [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) metodę, która jest wywoływana przez dołączenie "/HelloWorld/" do podstawowego adresu URL. Określa drugi komentarz [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) metody, która jest wywołana, dodając "/ HelloWorld/Witaj /" do adresu URL. Później w tym samouczku użyjesz silnika tworzenia szkieletu do generowania `HTTP POST` metody.

Uruchom aplikację w trybie bez debugowania i Dołącz "nazwę HelloWorld" w ścieżce w pasku adresu. `Index` Metoda zwraca ciąg.

![Okno przeglądarki, wyświetlanie odpowiedzi aplikacji, to jest Moja Akcja domyślna](~/tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC wywołuje klasy kontrolera (i metod akcji w nich), w zależności od przychodzącego adresu URL. Wartość domyślna [logikę routingu adresów URL](xref:mvc/controllers/routing) używany przez MVC używa formatu to w celu określenia, jakie kodu do wywołania:

`/[Controller]/[ActionName]/[Parameters]`

Ustaw format routingu w `Configure` method in Class metoda *Startup.cs* pliku.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Podczas uruchamiania aplikacji, a nie podasz żadnych segmentów adresu URL, jego wartość domyślna to kontroler "Home", a metoda "Index" określona w wiersza szablonu wyróżnione powyżej.

Pierwszy segment adresu URL określa klasę kontrolera do uruchomienia. Dlatego `localhost:xxxx/HelloWorld` mapuje `HelloWorldController` klasy. Druga część segment adresu URL określa metody akcji w klasie. Dlatego `localhost:xxxx/HelloWorld/Index` spowodowałoby `Index` metody `HelloWorldController` klasy do uruchomienia. Należy zauważyć, że masz aby przejść do `localhost:xxxx/HelloWorld` i `Index` wywołano metodę domyślnie. Jest to spowodowane `Index` jest domyślną metodą, która zostanie wywołana na kontrolerze, jeśli nazwa metody nie jest jawnie określona. Trzecia część segment adresu URL ( `id`) dla danych trasy. Zobaczysz dane trasy w późniejszym czasie na w tym samouczku.

Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda uruchamia i zwraca ciąg "To jest metoda powitalnej akcji...". Dla tego adresu URL jest kontroler `HelloWorld` i `Welcome` jest metodą akcji. Pierwszy raz używasz `[Parameters]` wchodzi w skład jeszcze adresu URL.

![Okno przeglądarki, wyświetlanie odpowiedzi aplikacji, to jest metoda akcji-Zapraszamy!](~/tutorials/first-mvc-app/adding-controller/_static/welcome.png)

Zmodyfikuj kod, aby przekazać niektóre informacje o parametrach z adresu URL do kontrolera. Na przykład `/HelloWorld/Welcome?name=Rick&numtimes=4`. Zmiana `Welcome` metodę, aby uwzględnić dwa parametry, jak pokazano w poniższym kodzie. 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

Powyższy kod:

* Używa funkcji opcjonalny parametr języka C# w celu wskazania, że `numTimes` parametru wartość domyślna to 1, jeśli nie przekazano żadnej wartości tego parametru.
* Używa`HtmlEncoder.Default.Encode` chronić aplikację przed złośliwe dane wejściowe (to znaczy JavaScript). 
* Używa [ciągi interpolowane](/dotnet/articles/csharp/language-reference/keywords/interpolated-strings).

Uruchom aplikację, a następnie przejdź do:

   `http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

(Zamiast xxxx numeru portu). Możesz spróbować różne wartości `name` i `numtimes` w adresie URL. MVC [wiązanie modelu](xref:mvc/models/model-binding) system automatycznie mapuje parametry nazwane z ciągu zapytania w pasku adresu do parametrów w metodzie. Zobacz [powiązań modelu](xref:mvc/models/model-binding) Aby uzyskać więcej informacji.

![Okno przeglądarki, przedstawiające odpowiedź aplikacji w Hello Rick jest NumTimes: 4](~/tutorials/first-mvc-app/adding-controller/_static/rick4.png)

Na ilustracji powyżej segment adresu URL (`Parameters`) nie jest używany, `name` i `numTimes` parametry są przekazywane jako [ciągów zapytania](https://wikipedia.org/wiki/Query_string). `?` (Znak zapytania) w powyższym adresie URL jest separatorem, a następnie wykonaj ciągi zapytań. `&` Rozdziela ciągi zapytań.

Zastąp `Welcome` metoda następującym kodem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

Uruchom aplikację, a następnie wprowadź następujący adres URL:  `http://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

![Okno przeglądarki, przedstawiające odpowiedzi aplikacji Hello Rick, identyfikator: 3](~/tutorials/first-mvc-app/adding-controller/_static/rick_routedata.png)

Tym razem trzeci segment adresu URL dopasowane parametru trasy `id`. `Welcome` Metody zawiera parametr `id` pasujących szablon adresu URL w `MapRoute` metody. Końcowe `?` (w `id?`) wskazuje `id` parametr jest opcjonalny.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

W tych przykładach kontrolera została wykonując "VC" część MVC — widok i kontroler pracy. Kontroler zwraca HTML bezpośrednio. Ogólnie nie ma kontrolerów bezpośrednio, zwracając HTML, ponieważ staje się bardzo skomplikowane, kodu i obsługa. Zamiast tego używa się zazwyczaj oddzielny plik szablonu widoku Razor ułatwiający Generowanie odpowiedzi HTML. Można to zrobić w następnym samouczku.
