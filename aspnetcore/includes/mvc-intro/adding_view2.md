Zastąp zawartość *Views/HelloWorld/Index.cshtml* plik widoku Razor następującym kodem:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index.cshtml)]

Przejdź do `http://localhost:xxxx/HelloWorld`. `Index` Method in Class metoda `HelloWorldController` nie znacznie; został uruchomiony instrukcji `return View();`, które określone metody należy używać pliku szablonu widoku do renderowania odpowiedzi do przeglądarki. Ponieważ nie zostały jawnie określić nazwę pliku szablonu widoku, MVC używa domyślnie *Index.cshtml* plik widoku w */widoków/HelloWorld* folderu. Na poniższym obrazie przedstawiono ciąg "Hello z naszych Wyświetl szablon"! zakodowane w widoku.

![Okno przeglądarki](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

Jeśli okno przeglądarki jest mały (na przykład na urządzeniu przenośnym), konieczne może być przełącznik (tap) [przycisk nawigacji Bootstrap](http://getbootstrap.com/components/#navbar) w prawym górnym rogu, aby zobaczyć **Home**, **o**, i **skontaktuj się z pomocą** łącza.

![Okno przeglądarki, wyróżnianie przycisk nawigacji Bootstrap](~/tutorials/first-mvc-app/adding-view/_static/1.png)

## <a name="changing-views-and-layout-pages"></a>Zmienianie widoków i układ strony

Naciskając linki menu (**MvcMovie**, **Home**, **o**). Każda strona zawiera ten sam układ menu. Układ menu jest zaimplementowana w *Views/Shared/_Layout.cshtml* pliku. Otwórz *Views/Shared/_Layout.cshtml* pliku.

[Układ](xref:mvc/views/layout) Szablony umożliwiają określ układ kontenera HTML Twojej witryny w jednym miejscu, a następnie zastosować je na wielu stronach w witrynie. Znajdź `@RenderBody()` wiersza. `RenderBody` jest symbolem zastępczym, gdzie wszystkie widoku specyficzne dla strony, możesz utworzyć pokaz, *opakowane* w stronę układu. Na przykład w przypadku wybrania **o** linku **Views/Home/About.cshtml** renderowania widoku w `RenderBody` metody.

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a>Zmienianie tytułu i menu łącza w pliku układu

W elemencie tytuł Zmień `MvcMovie` do `Movie App`. Zmień tekst zakotwiczenia w szablon układu z `MvcMovie` do `Movie App` i kontroler z `Home` do `Movies` jak wyróżniono poniżej:

::: moniker range="<= aspnetcore-2.0"

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout21.cshtml?highlight=6,29)]

::: moniker-end

>[!WARNING]
> Firma Microsoft nie zaimplementował `Movies` jeszcze kontrolera, więc jeśli możesz kliknąć ten link, zostanie wyświetlony błąd 404 (nie znaleziono).

Zapisz zmiany, a następnie naciśnij przycisk **o** łącza. Zwróć uwagę, jak tytuł, na karcie przeglądarki są obecnie wyświetlane **o - aplikacja filmu** zamiast **o - filmu Mvc**: 

![Temat karty](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

Naciśnij pozycję **skontaktuj się z pomocą** link i zwróć uwagę, tekst tytułu i zakotwiczenia także wyświetlać **aplikacja filmu**. Byliśmy w stanie wprowadzić zmianę, jeden raz w szablonie układ, i ma wszystkich stron w witrynie odzwierciedlają nowego tekstu linku i nowy tytuł.

Sprawdź *Views/_ViewStart.cshtml* pliku:


```HTML
@{
    Layout = "_Layout";
}
```

*Views/_ViewStart.cshtml* wiąże plik *Views/Shared/_Layout.cshtml* pliku do każdego widoku. Możesz użyć `Layout` właściwości, aby ustawić inny układ widoku lub ustaw go na `null` więc plik układu nie będą używane.

Zmienianie tytułu `Index` widoku.

Otwórz *Views/HelloWorld/Index.cshtml*. Istnieją dwa miejsca, aby wprowadzić zmiany:

   * Tekst wyświetlany w tytule przeglądarki.
   * Nagłówek dodatkowej (`<h2>` elementu).

Wprowadzisz je nieco, dzięki czemu można zobaczyć, które fragmentem kodu zmienia której części aplikacji.


```HTML
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

`ViewData["Title"] = "Movie List";` w kodzie powyżej zestawy `Title` właściwość `ViewData` słownik "Listę programów filmu". `Title` Właściwość jest używana w `<title>` elementu HTML na stronę układu:


```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

Zapisać zmiany i przejdź do `http://localhost:xxxx/HelloWorld`. Zauważ, że tytuł przeglądarki, podstawowego nagłówka i dodatkowych nagłówków zostały zmienione. (Jeśli nie widzisz zmian w przeglądarce, prawdopodobnie przeglądasz zawartość z pamięci podręcznej. Naciśnij klawisze Ctrl + F5 w przeglądarce, aby wymusić odpowiedzi z serwera do załadowania.) Tytuł przeglądarki jest tworzony z `ViewData["Title"]` jest ustawiany *Index.cshtml* wyświetlić szablonu i dodatkowych "-Movie App" dodane w pliku układu.

Należy również zauważyć, jak zawartości *Index.cshtml* szablon widoku została scalona z *Views/Shared/_Layout.cshtml* Wyświetl szablon i pojedynczą odpowiedź HTML był wysyłany do przeglądarki. Układ Szablony ułatwiają naprawdę wprowadzić zmiany, które są stosowane dla wszystkich stron w aplikacji. Aby dowiedzieć się więcej, zobacz [układ](xref:mvc/views/layout).

![Widok listy filmu](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

Nasze trochę "dane" (w tym przypadku "Hello z naszych Wyświetl szablon!" komunikat) jest ustalony, mimo że. Aplikacja MVC ma "V" (view) i masz "C" (kontroler), ale nie "M" (model) jeszcze.

## <a name="passing-data-from-the-controller-to-the-view"></a>Przekazywanie danych z kontrolera do widoku

Akcji kontrolera są wywoływane w odpowiedzi na przychodzące żądanie adresu URL. Klasa kontrolera jest pisze się kod, który obsługuje przychodzące żądania przeglądarki. Kontroler pobiera dane ze źródła danych i decyduje o rodzaju odpowiedzi do odesłania do przeglądarki. Wyświetlanie szablonów może służyć za pomocą kontrolera do generowania i formatowanie odpowiedzi HTML do przeglądarki.

Kontrolery są odpowiedzialne za świadczenie danych wymagane dla szablonu widoku do renderowania odpowiedzi. Najlepsze rozwiązanie: Wyświetl szablony powinny **nie** wykonania logiki biznesowej lub bezpośredniej interakcji z bazą danych. Przeciwnie Wyświetl szablon powinny działać tylko w przypadku danych, który znajduje się do niego przez kontroler. Obsługa tego "separacji" pomaga kodu czystego, zakresie testować i obsługiwać.

Obecnie `Welcome` method in Class metoda `HelloWorldController` klasy przyjmuje `name` i `ID` parametru, a następnie dane wyjściowe wartości bezpośrednio do przeglądarki. Zamiast kontrolera renderowania tej odpowiedzi jako ciąg, Zmień kontroler zamiast tego użyć szablonu widoku. Wyświetl szablon generuje odpowiedzi dynamicznej, co oznacza, że odpowiednich bitów danych muszą być przekazywane z kontrolera do widoku w celu wygenerowania odpowiedzi. W tym celu o kontrolera, umieść dane dynamiczne (parametry) wymagającym szablon widoku w `ViewData` słownik, który szablon widoku może uzyskać dostęp.

Wróć do *HelloWorldController.cs* pliku, a następnie zmień `Welcome` metody w celu dodania `Message` i `NumTimes` wartość `ViewData` słownika. `ViewData` Słownik jest obiekt dynamiczny, co oznacza, możesz umieścić dowolne; `ViewData` obiekt nie ma zdefiniowanej właściwości do momentu coś znajdującym się w nim umieścić. [System powiązań modelu MVC](xref:mvc/models/model-binding) automatycznie mapuje nazwanych parametrów (`name` i `numTimes`) z ciągu zapytania w pasku adresu do parametrów w metodzie. Pełne *HelloWorldController.cs* pliku wygląda następująco:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

`ViewData` Obiekt słownika zawiera dane, które zostaną przekazane do widoku. 

Tworzenie szablonu-Zapraszamy wyświetlanie o nazwie *Views/HelloWorld/Welcome.cshtml*.

Utworzysz pętlę w *Welcome.cshtml* Wyświetl szablon, który wyświetla "Hello" `NumTimes`. Zastąp zawartość *Views/HelloWorld/Welcome.cshtml* następującym kodem:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

Zapisz zmiany i przejdź do następującego adresu URL:

`http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

Dane są pobierane z adresu URL i przekazywane do kontrolera, za pomocą [integratora modelu MVC](xref:mvc/models/model-binding) . Kontroler pakiety danych do `ViewData` słownik i przebiegi, które obiekt widoku. Widok następnie renderuje dane jako kod HTML do przeglądarki.

![Widok przedstawiający powitalnej etykietę i frazy Hello Rick przedstawiono cztery razy — informacje](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

W powyższym przykładzie użyliśmy `ViewData` słownika do przekazywania danych z kontrolera do widoku. W dalszej części samouczka użyjemy modelu widoku do przekazywania danych za pomocą kontrolera do widoku. Widok modelu sposobem przekazywania danych jest zwykle znacznie preferowany nad `ViewData` podejście słownika. Zobacz [ViewModel vs vs ViewData vs obiekt ViewBag vs TempData sesji w MVC](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc) Aby uzyskać więcej informacji.

Cóż, to był rodzaju "M" dla modelu, ale nie rodzaj bazy danych. Przyjrzyjmy się, co możemy wyjaśniono konto i bazę danych filmów.
