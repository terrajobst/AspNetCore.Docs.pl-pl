# <a name="add-validation-to-an-aspnet-core-mvc-app"></a>Dodawanie walidacji do aplikacji platformy ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji dodasz logikę weryfikacji `Movie` modelu, a będzie wymusić reguł sprawdzania poprawności w dowolnym momencie użytkownik tworzy lub edytuje filmu.

## <a name="keeping-things-dry"></a>Utrzymywanie rzeczy suchej

Jednym z rozwiązań projektu składnika MVC jest [suchego](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("nie powtarzaj samodzielnie"). ASP.NET MVC zachęca do określone funkcje lub działanie tylko raz, a następnie go wszędzie odzwierciedlone w aplikacji. Zmniejsza ilość kodu, które należy napisać i sprawia, że kod napisany mniej błąd podatnych na błędy, łatwiejsze testowanie i łatwiejsze w obsłudze.

Obsługa sprawdzania poprawności, MVC i Entity Framework Core Code First jest dobrym przykładem suchej zasady w akcji. Reguły sprawdzania poprawności można określić deklaratywnie w jednym miejscu (w klasie modelu) i zasady są wymuszane wszędzie w aplikacji.

## <a name="adding-validation-rules-to-the-movie-model"></a>Dodawanie reguł walidacji modelu film

Otwórz *Movie.cs* pliku. DataAnnotations zawiera zestaw wbudowanych atrybutów sprawdzania poprawności, stosowane deklaratywnie do klasy lub właściwości. (Zawiera ona także formatowania atrybutów, takich jak `DataType` czy pomoc w formacie i nie oferują żadnych sprawdzania poprawności.)

Aktualizacja `Movie` klasy, aby móc korzystać z wbudowanych `Required`, `StringLength`, `RegularExpression`, i `Range` atrybutów sprawdzania poprawności.

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie21/Models/MovieDateRatingDA.cs?name=snippet1)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]
::: moniker-end

Atrybuty weryfikacji Określ zachowanie, które mają zostać wymuszone we właściwościach modelu, do którego jest stosowany. `Required` i `MinimumLength` atrybuty wskazuje, że właściwość musi mieć wartość, ale nic nie uniemożliwia wprowadzanie biały znak do zaspokojenia tej weryfikacji przez użytkownika. `RegularExpression` Atrybut służy do ograniczania znaków, które można wprowadzić. W powyższym kodzie `Genre` i `Rating` należy używać tylko liter (pierwsze litery wielkie litery, białe miejsca, cyfry i znaki specjalne są niedozwolone). `Range` Atrybut ogranicza wartość do określonego zakresu. `StringLength` Atrybut pozwala określić maksymalną długość ciągu właściwości oraz opcjonalnie długości minimalnej. Typy wartości (takie jak `decimal`, `int`, `float`, `DateTime`) są z założenia wymagane i nie wymagają `[Required]` atrybutu.

Posiadanie reguły sprawdzania poprawności automatycznie wymuszane przez ASP.NET pomaga upewnij bardziej niezawodnych aplikacji. Gwarantuje również, że nie zapomnisz do sprawdzania poprawności coś i przypadkowo let złe dane do bazy danych.

## <a name="validation-error-ui-in-mvc"></a>Błąd sprawdzania poprawności interfejsu użytkownika na platformie MVC

Uruchom aplikację i przejdź do kontrolera filmów.

Wybierz **Utwórz nowy** łącze, aby dodać nowy filmu. Wypełnij formularz z niektórych z nieprawidłowymi wartościami. Jak weryfikacji po stronie klienta jQuery wykryje błąd, wyświetla komunikat o błędzie.

![Formularz widoku film z wielu błędy weryfikacji po stronie klienta jQuery](~/tutorials/first-mvc-app/validation/_static/val.png)

> [!NOTE]
> Nie można wprowadzić przecinki dziesiętne w `Price` pola. Do obsługi [weryfikacji jQuery](https://jqueryvalidation.org/) dla innych niż angielski, które użyj przecinka (",") dla punktu dziesiętnego i formaty daty z systemem innym niż angielski, należy wykonać kroki, aby globalize aplikacji. To [GitHub problem 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) instrukcje dotyczące dodawania przecinkiem. 

Zwróć uwagę, jak formularz automatycznie renderowany odpowiedni komunikat o błędzie weryfikacji w każdym polu zawierający nieprawidłową wartość. Błędy są wymuszane zarówno po stronie klienta (przy użyciu języka JavaScript i jQuery) i po stronie serwera (w przypadku, gdy użytkownik ma JavaScript wyłączone).

Znaczące korzyści jest, że nie trzeba zmienić pojedynczy wiersz kodu w `MoviesController` klasy lub *Create.cshtml* widoku w celu umożliwienia tej weryfikacji interfejsu użytkownika. Kontroler i widoki utworzone wcześniej w tym samouczku automatycznie pobierane up sprawdzania poprawności reguły, określona za pomocą atrybutów weryfikacji właściwości `Movie` klasa modelu. Test weryfikacji za pomocą `Edit` metody akcji, a tym samym sprawdzania poprawności jest stosowany.

Dane formularza nie jest wysyłane do serwera, dopóki nie ma żadnych błędów weryfikacji po stronie klienta. Można to sprawdzić, ustawiając dla punktu przerwania `HTTP Post` — metoda, za pomocą [narzędzie Fiddler](http://www.telerik.com/fiddler) , lub [F12 Developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).

## <a name="how-validation-works"></a>Działanie sprawdzania poprawności

Może zastanawiasz się, jak weryfikacji interfejsu użytkownika został wygenerowany bez żadnych aktualizacji do kodu w kontrolerze lub widoków. Poniższy kod przedstawia dwa `Create` metody.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]

Pierwszy (HTTP GET) `Create` formularza początkowego Utwórz Wyświetla metody akcji. Druga (`[HttpPost]`) wersja obsługuje post formularza. Drugi `Create` — metoda ( `[HttpPost]` wersji) wywołań `ModelState.IsValid` do sprawdzenia, czy film ma jakieś błędy sprawdzania poprawności. Wywołanie tej metody ocenia wszystkie atrybuty weryfikacji, które zostały zastosowane do tego obiektu. Jeśli obiekt ma błędy sprawdzania poprawności `Create` — metoda zostanie ponownie wyświetlony formularz. Jeśli nie ma żadnych błędów, metoda zapisuje nowe filmu w bazie danych. W naszym przykładzie filmu formularza nie jest zaksięgowany na serwerze, gdy występują błędy sprawdzania poprawności wykryto po stronie klienta; drugi `Create` metoda nigdy nie jest wywoływana, gdy występują błędy sprawdzania poprawności po stronie klienta. Jeśli wyłączysz JavaScript w przeglądarce, sprawdzanie poprawności klienta jest wyłączona i można przetestować HTTP POST `Create` metody `ModelState.IsValid` wykrywanie jakieś błędy sprawdzania poprawności.

Można ustawić punktu przerwania w `[HttpPost] Create` — metoda i sprawdź nigdy nie jest wywoływana metoda, weryfikacji po stronie klienta nie będzie dłużej przesyłać dane formularza, gdy wykryto błędy sprawdzania poprawności. Jeśli musisz wyłączyć JavaScript w przeglądarce, a następnie przesłać formularza z błędami, nastąpi trafienie punktu przerwania. Nadal otrzymywać pełne sprawdzanie poprawności bez JavaScript. 

Na poniższej ilustracji przedstawiono sposób wyłączania JavaScript w przeglądarce FireFox.

![Firefox: Na karcie zawartość okna dialogowego Opcje usunąć zaznaczenie pola wyboru Włącz język Javascript.](~/tutorials/first-mvc-app/validation/_static/ff.png)

Na poniższej ilustracji przedstawiono sposób wyłączania JavaScript w przeglądarce Chrome.

![Google Chrome: Sekcja w Javascript ustawienia zawartości, wybierz nie zezwalają na dowolnej lokacji do uruchomienia kodu JavaScript.](~/tutorials/first-mvc-app/validation/_static/chrome.png)

Po wyłączeniu JavaScript po nieprawidłowe dane i kroku przez debuger.

![Podczas debugowania na post nieprawidłowych danych, Intellisense w ModelState.IsValid pokazano, że wartość ma wartość false.](~/tutorials/first-mvc-app/validation/_static/ms.png)

Poniżej znajduje się część *Create.cshtml* Wyświetl szablon, który szkieletu wcześniej w samouczku. Jest on używany przez metody akcji pokazanym powyżej zarówno do wyświetlania formularza początkowego i wyświetl ją ponownie w przypadku wystąpienia błędu.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Views/Movies/CreateRatingBrevity.cshtml)]

[Pomocnika Tag danych wejściowych](xref:mvc/views/working-with-forms) używa [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atrybutów i tworzy wymagane dla technologii jQuery weryfikacji po stronie klienta atrybutów HTML. [Pomocnika tagów weryfikacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) przedstawia błędy sprawdzania poprawności. Zobacz [weryfikacji](xref:mvc/models/validation) Aby uzyskać więcej informacji.

Co to jest naprawdę nieuprzywilejowany o tej metody oznacza, że żaden kontroler ani `Create` Wyświetl szablon zna niczego dotyczące reguł rzeczywista weryfikacja wymuszany lub określone komunikaty o błędach wyświetlane. Reguły sprawdzania poprawności i ciągi błąd są określane tylko w `Movie` klasy. Te tej samej reguły sprawdzania poprawności są automatycznie stosowane do `Edit` widoku i wszystkich innych widoków szablonów można tworzyć które edytować model.

Jeśli musisz zmienić logikę weryfikacji, możesz to zrobić w dokładnie jednego miejsca przez dodanie atrybutów sprawdzania poprawności modelu (w tym przykładzie `Movie` klasy). Nie musisz martwić się o różnych częściach aplikacji jest niespójna z jak zasady są wymuszane — całą logikę sprawdzania poprawności zostanie zdefiniowana w jednym miejscu i używany wszędzie. Przechowuje kod bardzo czystą i ułatwia utrzymanie i rozwijać. I oznacza, że użytkownik będzie można pełni ramach suchej zasady.

## <a name="using-datatype-attributes"></a>Przy użyciu atrybutów typu danych

Otwórz *Movie.cs* pliku i sprawdź, czy `Movie` klasy. `System.ComponentModel.DataAnnotations` Przestrzeń nazw zawiera atrybuty formatowania oprócz wbudowanych zestaw atrybutów weryfikacji. Zastosowaliśmy już `DataType` wartość wyliczenia Data wydania i pola cen. Poniższy kod przedstawia `ReleaseDate` i `Price` właściwości z odpowiednią `DataType` atrybutu.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

`DataType` Atrybuty zapewniają tylko wskazówki dotyczące aparatu widoku do formatowania danych (i dostarcza elementy/atrybutów, takich jak `<a>` dla adresu URL i `<a href="mailto:EmailAddress.com">` do obsługi poczty e-mail. Można użyć `RegularExpression` atrybut do zweryfikowania formatu danych. `DataType` Atrybut służy do określania typu danych, który jest bardziej szczegółowy niż typ wewnętrznej bazy danych, nie ma atrybutów sprawdzania poprawności. W takim przypadku tylko chcemy śledzić datę, a nie czas. `DataType` Wyliczenie zapewnia dla różnych typów danych, takie jak data, czas, numer telefonu, waluty, EmailAddress i inne. `DataType` Atrybut można również włączyć aplikacji w celu umożliwienia automatycznie funkcji specyficznych dla typu. Na przykład `mailto:` można tworzyć łącza `DataType.EmailAddress`, i może zostać dostarczony selektora daty `DataType.Date` w przeglądarkach obsługujących HTML5. `DataType` Atrybuty emituje HTML 5 `data-` atrybutów (wyraźnym danych dash), które byłyby zrozumiałe dla przeglądarki HTML 5. `DataType` Czy atrybuty **nie** Podaj wszystkich sprawdzania poprawności.

`DataType.Date` nie określono format daty, która jest wyświetlana. Domyślnie pole danych są wyświetlane domyślne formaty oparte na tym serwerze `CultureInfo`.

`DisplayFormat` Atrybut służy do jawnie określić format daty:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` Ustawienie określa, że formatowanie powinny również będą stosowane, gdy wartość jest wyświetlana w polu tekstowym do edycji. (Nie może być który dla niektórych pól — na przykład dla wartości waluty, prawdopodobnie nie ma symbolu waluty w polu tekstowym do edycji.)

Można użyć `DisplayFormat` atrybutu przez sam, ale jest zwykle warto użyć `DataType` atrybutu. `DataType` Atrybut przekazuje semantykę danych zamiast sposób renderowania jej na ekranie i zapewnia następujące korzyści, które nie można uzyskać z DisplayFormat:

* Przeglądarki, można włączyć funkcje HTML5 (na przykład pokazać formant kalendarza, symbol waluty odpowiednie ustawienia regionalne, przesyłanie pocztą e-mail łączy, itp.)

* Domyślnie przeglądarka wyświetli danych przy użyciu właściwego formatu oparte na ustawienia regionalne.

* `DataType` Atrybut można włączyć MVC wybrać szablon pola prawo do renderowania danych ( `DisplayFormat` Jeśli używany przez samego używa szablonu ciągu).

> [!NOTE]
> weryfikacji jQuery nie działa z `Range` atrybutu i `DateTime`. Na przykład następujący kod zawsze wyświetli błąd sprawdzania poprawności po stronie klienta, nawet wtedy, gdy data jest w określonym zakresie:

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

Należy wyłączyć sprawdzanie poprawności daty jQuery do użycia `Range` atrybutem `DateTime`. Zazwyczaj nie jest dobrym rozwiązaniem do skompilowania w modelach przy użyciu twardych daty `Range` atrybutu i `DateTime` jest niezalecane.

Poniższy kod przedstawia łączenie atrybutów w jednym wierszu:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

W następnej części serii, firma Microsoft będzie Przejrzyj aplikacji i poprawiają do automatycznie generowanego `Details` i `Delete` metody.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Praca z formularzami](xref:mvc/views/working-with-forms)
* [Globalizacja i lokalizacja](xref:fundamentals/localization)
* [Wprowadzenie do pomocników tagów](xref:mvc/views/tag-helpers/intro)
* [Autor pomocników tagów](xref:mvc/views/tag-helpers/authoring)
