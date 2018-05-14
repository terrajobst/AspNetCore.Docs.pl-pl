Zastąp zawartość *Views/HelloWorld/Index.cshtml* pliku widoku Razor z następujących czynności:

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index.cshtml)]

Przejdź do `http://localhost:xxxx/HelloWorld`. `Index` Metody w `HelloWorldController` nie zrobić wiele; napotkał instrukcji `return View();`, który określony metody należy użyć pliku szablonu widoku do renderowania odpowiedzi do przeglądarki. Ponieważ nie został jawnie określić nazwę pliku szablonu widoku, MVC używa domyślnie *Index.cshtml* pliku widoku w */widoków/HelloWorld* folderu. Na poniższym obrazie pokazano ciąg "Hello z naszych szablonu widoku!" ustalony w widoku.

![Okno przeglądarki](../../tutorials/first-mvc-app/adding-view/_static/hell_template.png)

Jeśli okno przeglądarki jest mały (na przykład na urządzeniu przenośnym), może być konieczne przełączania (tap) [przycisk nawigacji Bootstrap](http://getbootstrap.com/components/#navbar) w prawym górnym rogu, aby wyświetlić **Home**, **o**, i **skontaktuj się z** łącza.

![Wyróżnianie przycisk nawigacji Bootstrap okna przeglądarki](../../tutorials/first-mvc-app/adding-view/_static/1.png)

## <a name="changing-views-and-layout-pages"></a>Zmiana widoków i układ strony

Wybierz z menu łączy (**MvcMovie**, **Home**, **o**). Każda strona zawiera ten sam układ menu. Układ menu jest zaimplementowana w *Views/Shared/_Layout.cshtml* pliku. Otwórz *Views/Shared/_Layout.cshtml* pliku.

[Układ](xref:mvc/views/layout) Szablony umożliwiają określ układ kontenera HTML witryny w jednym miejscu, a następnie zastosować je na wielu stronach w witrynie sieci. Znajdź `@RenderBody()` wiersza. `RenderBody` jest symbol zastępczy, gdzie wszystkie widoku specyficzne dla stron, możesz utworzyć pokaz, *opakowana* na stronie układu. Na przykład w przypadku wybrania **o** łącza, **Views/Home/About.cshtml** renderowania widoku wewnątrz `RenderBody` metody.

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a>Zmień łącza tytuł i menu w pliku układu

W elemencie tytuł zmienić `MvcMovie` do `Movie App`. Zmień tekst zakotwiczenia w szablon układu z `MvcMovie` do `Movie App` i kontroler z `Home` do `Movies` jak wyróżniono poniżej:

Uwaga: Wersja platformy ASP.NET Core 2.0 jest nieco inne. Nie zawiera on `@inject ApplicationInsights` i `@Html.Raw(JavaScriptSnippet.FullScript)`.

[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

>[!WARNING]
> Firma Microsoft nie zaimplementował `Movies` jeszcze kontrolera, dlatego możesz kliknąć łącze, zostanie wyświetlony błąd 404 (nie znaleziono).

Zapisz zmiany, a następnie naciśnij pozycję **o** łącza. Zwróć uwagę, jak tytuł na karcie przeglądarki są obecnie wyświetlane **o - Movie App** zamiast **o - Mvc Movie**: 

![O karcie](../../tutorials/first-mvc-app/adding-view/_static/about2.png)

Wybierz **skontaktuj się z** link i zwróć uwagę tekstu tytułu i zakotwiczenia również wyświetlić **Movie App**. Udało się zmienić raz w szablonie układ i mieć wszystkich stron w witrynie uwzględniać nowy tekst łącza i nowy tytuł.

Sprawdź *Views/_ViewStart.cshtml* pliku:


```HTML
@{
    Layout = "_Layout";
}
```

*Views/_ViewStart.cshtml* zaimportowanie pliku *Views/Shared/_Layout.cshtml* plików do każdego widoku. Można użyć `Layout` właściwość, aby ustawić inny układ widok lub ustaw ją na `null` tak ma układ pliku będzie używany.

Zmień tytuł `Index` widoku.

Otwórz *Views/HelloWorld/Index.cshtml*. Istnieją dwa miejsca do dokonania zmiany:

   * Tekst wyświetlany w tytule przeglądarki.
   * Nagłówek dodatkowej (`<h2>` elementu).

Należy podjąć ich nieco inne pozwala zobaczyć, które fragmentem kodu zmienia której części aplikacji.


```HTML
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

`ViewData["Title"] = "Movie List";` w kodzie powyżej zestawy `Title` właściwość `ViewData` słownik "Listy filmów". `Title` Właściwość jest używana w `<title>` elementu HTML na stronie układu:


```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

Zapisz zmiany i przejdź do `http://localhost:xxxx/HelloWorld`. Należy zauważyć, że tytuł przeglądarki, nagłówek głównej i dodatkowej nagłówki zostały zmienione. (Jeśli nie widzisz zmian w przeglądarce, użytkownik może mieć możliwość wyświetlania zawartości w pamięci podręcznej. Naciśnij klawisz Ctrl + F5 w przeglądarce, aby wymusić odpowiedzi z serwera do załadowania.) Tytuł przeglądarki jest tworzony z `ViewData["Title"]` ustawiliśmy *Index.cshtml* wyświetlić szablonu i dodatkowych "-Movie App" dodane w pliku układu.

Należy również zauważyć, jak zawartości w *Index.cshtml* Wyświetl szablon został scalony z *Views/Shared/_Layout.cshtml* szablon widoku i pojedynczą odpowiedź HTML był wysyłany do przeglądarki. Szablony układu ułatwiają naprawdę wprowadzić zmiany, które są stosowane dla wszystkich stron w aplikacji. Aby dowiedzieć się więcej, zobacz [układu](xref:mvc/views/layout).

![Widok listy filmów](../../tutorials/first-mvc-app/adding-view/_static/hell3.png)

Nasze niewielki "data" (w tym przypadku "Hello z naszych szablonu widoku!" komunikat) jest ustalony, mimo że. Aplikacji MVC ma "V" (Widok) i masz "C" (kontroler), ale nie "M" (model) jeszcze.

## <a name="passing-data-from-the-controller-to-the-view"></a>Przekazywanie danych z kontrolera do widoku

Akcji kontrolera są wywoływane w odpowiedzi na żądania przychodzącego adresu URL. Klasa kontrolera jest, gdzie napisać kod obsługujący żądania przychodzące przeglądarki. Kontroler pobiera dane ze źródła danych i decyduje o rodzaju odpowiedzi do odesłania do przeglądarki. Wyświetlanie szablonów można z kontrolera do generowania i format odpowiedzi HTML do przeglądarki.

Kontrolery są odpowiedzialne za przekazywanie danych wymagane dla szablonu widoku do renderowania odpowiedzi. Najlepsze rozwiązanie: Wyświetl szablony powinien **nie** wykonywania logiki biznesowej lub bezpośredniej interakcji z bazą danych. Zamiast szablonu widoku powinny działać tylko z danymi, które jest podany przez kontroler. Obsługa tego "separacji" zwiększają kodu czystą, zakresie testować i obsługiwać.

Obecnie `Welcome` metody w `HelloWorldController` klasy przyjmuje `name` i `ID` parametr, a następnie dane wyjściowe wartości bezpośrednio do przeglądarki. Zamiast kontrolera renderowania tej odpowiedzi jako ciąg, Zmień kontrolera zamiast tego użyć szablonu widoku. Wyświetl szablon generuje odpowiedzi dynamicznych, co oznacza, że odpowiednich bitów danych muszą być przekazywane z kontrolera do widoku w celu wygenerowania odpowiedzi. W tym celu o kontrolera put danych dynamicznych (parametry), które wymaga Wyświetl szablon o `ViewData` słownik, który szablon widoku można następnie uzyskać dostęp.

Wróć do *HelloWorldController.cs* plików i zmień `Welcome` metody w celu dodania `Message` i `NumTimes` do wartości `ViewData` słownika. `ViewData` Słownik jest obiekt dynamiczny, co oznacza, które można wprowadzić dowolne; `ViewData` obiekt nie ma zdefiniowanej właściwości dopóki coś wewnątrz put. [System powiązanie modelu MVC](xref:mvc/models/model-binding) automatycznie mapuje nazwane parametry (`name` i `numTimes`) z ciągu zapytania w pasku adresu w parametrach w metodę. Pełną *HelloWorldController.cs* pliku wygląda następująco:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

`ViewData` Obiekt słownika zawiera dane, które zostaną przekazane do widoku. 

Tworzenie szablonu powitalnej widok o nazwie *Views/HelloWorld/Welcome.cshtml*.

Utworzysz pętli w *Welcome.cshtml* szablon widoku, który zawiera "tekst Hello" `NumTimes`. Zastąp zawartość *Views/HelloWorld/Welcome.cshtml* następującym kodem:

[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

Zapisz zmiany i przejdź do następującego adresu URL:

`http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

Dane są pobierane z adresu URL i przekazywane do kontrolera przy użyciu [integratora modelu MVC](xref:mvc/models/model-binding) . Kontroler pakiety danych w `ViewData` słownika i przekazuje, które obiekt do widoku. Widok następnie renderuje dane jako HTML do przeglądarki.

![Widok pokazujący powitalnej etykiety i frazy Hello Rick przedstawiono cztery razy — informacje](../../tutorials/first-mvc-app/adding-view/_static/rick2.png)

W powyższym przykładzie użyliśmy `ViewData` słownika do przekazywania danych z kontrolera do widoku. Później w samouczku używamy model widoku do przekazywania danych z kontrolera do widoku. Podejście modelu widoku do przekazywania danych jest zwykle znacznie preferowany nad `ViewData` podejście słownika. Zobacz [ViewModel vs vs ViewData vs obiekt ViewBag TempData vs sesji na platformie MVC](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc) Aby uzyskać więcej informacji.

Dobrze, która jest typu "M" dla modelu, ale nie rodzaj bazy danych. Przyjrzyjmy się, co możemy samouczka jest i utworzyć bazę danych filmów.
