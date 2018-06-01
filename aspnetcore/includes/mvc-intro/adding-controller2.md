Zastąp zawartość *Controllers/HelloWorldController.cs* następującym kodem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

Każdy `public` można wywołać jako punkt końcowy HTTP jest metoda w kontrolerze. W powyższym przykładzie obie metody zwracają ciąg.  Należy pamiętać, komentarze przed każdą z tych metod.

Punkt końcowy HTTP jest targetable adres URL w aplikacji sieci web, takich jak `http://localhost:1234/HelloWorld`i protokół używany łączy: `HTTP`, lokalizacji sieciowej serwera sieci web (w tym porcie TCP): `localhost:1234` i docelowy identyfikator URI `HelloWorld`.

Stany pierwszego komentarza to [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) metodę, która jest wywoływana przez dodanie "/HelloWorld/" do podstawowego adresu URL. Określa drugi komentarz [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) metodę, która jest wywoływana przez dołączenie "/ HelloWorld/Zapraszamy /" do adresu URL. Później w samouczku użyjesz aparat szkieletów do generowania `HTTP POST` metody.

Uruchom aplikację w trybie bez debugowania i Dołącz "HelloWorld" do ścieżki w pasku adresu. `Index` Metoda zwraca ciąg.

![Wyświetlanie odpowiedzi aplikacji tego okna przeglądarki jest Mój Akcja domyślna](~/tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC wywołuje klasy kontrolera (i metod akcji w nich), w zależności od przychodzącego adresu URL. Wartość domyślna [logiki routingu adresu URL](xref:mvc/controllers/routing) używany przez MVC używa formatu to w celu określenia, kodu do wywołania:

`/[Controller]/[ActionName]/[Parameters]`

Określanie formatu routingu w `Configure` metody w *Startup.cs* pliku.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Po uruchomieniu aplikacji i nie podawaj wszystkie segmenty adresu URL, domyślnie kontroler "Home" i metoda "Index" określona w wierszu szablonu wyróżnione powyżej.

Pierwszy segment adresu URL określa klasy kontrolera do uruchamiania. Dlatego `localhost:xxxx/HelloWorld` mapuje `HelloWorldController` klasy. Druga część segment adresu URL określa metody akcji w klasie. Dlatego `localhost:xxxx/HelloWorld/Index` spowodowałoby `Index` metody `HelloWorldController` klasy do uruchomienia. Należy zauważyć, że masz aby przejść do `localhost:xxxx/HelloWorld` i `Index` wywołano metodę domyślnie. Jest to spowodowane `Index` jest domyślna metoda, która zostanie wywołana na kontrolerze, jeśli nie jest jawnie określić nazwę metody. Trzeci część segment adresu URL ( `id`) dla danych trasy. Dane trasy zostanie wyświetlone później w tym samouczku.

Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda uruchamia i zwraca ciąg "Jest to metoda akcji Zapraszamy...". Dla tego adresu URL kontrolera jest `HelloWorld` i `Welcome` jest metodą akcji. Nie były używane `[Parameters]` część jeszcze adresu URL.

![Okno przeglądarki, przedstawiające odpowiedzi aplikacji to jest metoda akcji-Zapraszamy!](~/tutorials/first-mvc-app/adding-controller/_static/welcome.png)

Zmodyfikuj kod do przekazywania informacji niektórych parametrów z adresu URL do kontrolera. Na przykład `/HelloWorld/Welcome?name=Rick&numtimes=4`. Zmień `Welcome` metodę w celu uwzględnienia dwóch parametrów, jak pokazano w poniższym kodzie. 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

Poprzedni kod:

* Aby wskazać, że jest używana funkcja opcjonalny parametr C# `numTimes` parametr ma domyślnie wartość 1, jeśli nie przekazano żadnej wartości tego parametru.
* Używa`HtmlEncoder.Default.Encode` do ochrony aplikacji przed złośliwymi danych wejściowych (to znaczy JavaScript). 
* Używa [ciągi interpolowane](/dotnet/articles/csharp/language-reference/keywords/interpolated-strings).

Uruchom aplikację i przejdź do:

   `http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

(Zamiast xxxx numer portu). Możesz spróbować różnych wartości `name` i `numtimes` w adresie URL. MVC [modelu powiązania](xref:mvc/models/model-binding) system automatycznie mapuje parametry nazwane z ciągu zapytania w pasku adresu w parametrach w metodę. Zobacz [powiązanie modelu](xref:mvc/models/model-binding) Aby uzyskać więcej informacji.

![Okno przeglądarki, przedstawiające odpowiedzi aplikacji Hello Rick jest NumTimes: 4](~/tutorials/first-mvc-app/adding-controller/_static/rick4.png)

Na ilustracji powyżej segment adresu URL (`Parameters`) nie jest używany, `name` i `numTimes` parametry są przekazywane jako [ciągów zapytania](https://wikipedia.org/wiki/Query_string). `?` (Znak zapytania) w powyższy adres URL jest separatorem, i wykonaj ciągi zapytań. `&` Rozdziela ciągi zapytań.

Zastąp `Welcome` metodę z następującym kodem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

Uruchom aplikację i wprowadź następujący adres URL:  `http://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

![Okno przeglądarki, przedstawiające odpowiedzi aplikacji Hello Rick, identyfikator: 3](~/tutorials/first-mvc-app/adding-controller/_static/rick_routedata.png)

Teraz trzeci segment adresu URL dopasowane parametru trasy `id`. `Welcome` Metoda zawiera parametr `id` pasujących szablon adresu URL w `MapRoute` metody. Końcowe `?` (w `id?`) wskazuje `id` parametr jest opcjonalny.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

W tych przykładach kontroler został czynności "VC" część MVC — widok i kontroler pracy. Kontroler bezpośrednio zwraca HTML. Zwykle nie mają kontrolerów bezpośrednio, zwracając HTML, ponieważ staje się bardzo trudne kodu i obsługa. Zamiast tego zazwyczaj pozwala oddzielny plik szablonu widoku Razor łatwiej Generowanie odpowiedzi HTML. Można to zrobić w następnym samouczku.
