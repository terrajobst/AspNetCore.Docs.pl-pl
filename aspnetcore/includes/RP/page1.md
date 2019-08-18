# <a name="scaffolded-razor-pages-in-aspnet-core"></a>Razor Pages szkieletowe w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten samouczek służy do badania Razor Pages utworzonych przez tworzenie szkieletów w poprzednim samouczku.

[Wyświetlanie lub pobieranie](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21) próbki.

## <a name="the-create-delete-details-and-edit-pages"></a>Strony tworzenie, usuwanie, szczegóły i edycja.

Zapoznaj się z modelem stron */filmów/index. cshtml. cs* :

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

Razor Pages pochodzą od `PageModel`. Zgodnie z `PageModel`Konwencją Klasa pochodna jest wywoływana `<PageName>Model`. Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) , aby dodać `MovieContext` do strony. Wszystkie strony szkieletowe są zgodne z tym wzorcem. Zobacz [kod asynchroniczny](xref:data/ef-rp/intro#asynchronous-code) , aby uzyskać więcej informacji na temat programowania asynchronicznego przy użyciu Entity Framework.

Gdy żądanie jest wykonywane dla strony, `OnGetAsync` Metoda zwraca listę filmów do strony Razor. `OnGetAsync`lub `OnGet` jest wywoływana na stronie Razor, aby zainicjować stan dla strony. W takim przypadku `OnGetAsync` pobiera listę filmów i wyświetla je.

Gdy `OnGet` zwraca `void` lub `OnGetAsync` zwraca,niejestużywanażadnametodaReturn.`Task` Gdy typem zwracanym jest `IActionResult` lub `Task<IActionResult>`, należy podać instrukcję return. Na przykład: *Pages/Films/Create. cshtml. cs.* `OnPostAsync`

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a>Przejrzyj stronę */filmy/index. cshtml* Razor:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor można przenieść z HTML do C# lub do znacznika specyficznego dla Razor. Gdy po C#symbolu następuje [słowo kluczowe zarezerwowane Razor](xref:mvc/views/razor#razor-reserved-keywords), przechodzi do znacznika specyficznego dla Razor, w przeciwnym razie przechodzi do. `@`

Dyrektywa Razor powoduje, że plik jest akcją &mdash; MVC, co oznacza, że może obsługiwać żądania. `@page` `@page`musi być pierwszą dyrektywą Razor na stronie. `@page`jest przykładem przejścia do znacznika specyficznego dla Razor. Aby uzyskać więcej informacji, zobacz [składnia Razor](xref:mvc/views/razor#razor-syntax) .

Bada wyrażenie lambda użyte w następującym Pomocniku HTML:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

Pomocnik html sprawdza `Title` właściwość, do której istnieje odwołanie w wyrażeniu lambda, aby określić nazwę wyświetlaną. `DisplayNameFor` Wyrażenie lambda jest sprawdzane, a nie oceniane. Oznacza to, że nie ma żadnych naruszeń `model`dostępu `model.Movie`, gdy `model.Movie[0]` , `null` , lub są puste. Gdy wyrażenie lambda jest oceniane (na przykład z `@Html.DisplayFor(modelItem => item.Title)`), wartości właściwości modelu są oceniane.

<a name="md"></a>

### <a name="the-model-directive"></a>@model Dyrektywa

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

`@model` Dyrektywa określa typ modelu przekazaną do strony Razor. W poprzednim przykładzie `@model` wiersz `PageModel`powoduje, że Klasa pochodna jest dostępna dla strony Razor. Model jest używany w `@Html.DisplayNameFor` [pomocnikach HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) i `@Html.DisplayFor` na stronie.

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>

### <a name="viewdata-and-layout"></a>ViewData i układ

Rozważmy następujący kod:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

Poprzedni wyróżniony kod jest przykładem przejścia Razor do C#. Znaki `{` i `}` należy ująć w blok C# kodu.

Klasa `PageModel` bazowa `ViewData` ma właściwość dictionary, która może służyć do dodawania danych, które mają zostać przekazane do widoku. Obiekty można dodawać do `ViewData` słownika przy użyciu wzorca klucz/wartość. W poprzednim przykładzie właściwość "title" została dodana do `ViewData` słownika.

::: moniker range="= aspnetcore-2.0"

Właściwość "title" jest używana w pliku *Pages/Shared/_Layout. cshtml* . Poniższe znaczniki pokazują kilka pierwszych wierszy *stron/Shared/_Layout. cshtml* .

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Właściwość "title" jest używana w pliku *Pages/Shared/_Layout. cshtml* . Poniższy znacznik pokazuje pierwsze kilka wierszy pliku *_Layout. cshtml* .

::: moniker-end

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

Wiersz `@*Markup removed for brevity.*@` jest komentarzem Razor. W przeciwieństwie do komentarzy`<!-- -->`HTML (), komentarze Razor nie są wysyłane do klienta.

Uruchom aplikację i przetestuj linki w projekcie (**Strona główna**, **informacje**, **kontakt**, **Utwórz**, **Edytuj**i **Usuń**). Każda Strona ustawia tytuł, który można zobaczyć na karcie przeglądarki. Po utworzeniu zakładki na stronie tytuł jest używany dla zakładki. *Pages/index. cshtml* i *Pages/Films/index. cshtml* mają teraz ten sam tytuł, ale można je zmodyfikować tak, aby zawierały różne wartości.

> [!NOTE]
> Nie można wprowadzić dziesiętna przecinkami w `Price` pola. Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i formatów dat innych niż angielski, należy wykonać kroki w celu globalizacji aplikacji. Ten [problem w usłudze GitHub 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) zawiera instrukcje dotyczące dodawania przecinków dziesiętnych.

Właściwość jest ustawiana w pliku *Pages/_ViewStart. cshtml:* `Layout`

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

Powyższe oznakowanie ustawia plik układu na *Pages/Shared/_Layout. cshtml* dla wszystkich plików Razor w folderze *Pages* . Aby uzyskać więcej informacji, zobacz [Układ](xref:razor-pages/index#layout) .

### <a name="update-the-layout"></a>Aktualizowanie układu

Zmień element w pliku *Pages/Shared/_Layout. cshtml* , aby użyć krótszego ciągu. `<title>`

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

Znajdź następujący element zakotwiczony w pliku *Pages/Shared/_Layout. cshtml* .

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```

Zastąp poprzedni element następującym znacznikiem.

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

Poprzedni element zakotwiczenia jest [pomocnikiem tagów](xref:mvc/views/tag-helpers/intro). W tym przypadku jest to [pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). Atrybut pomocnika `/Movies/Index`tagówi wartość tworzy łącze do strony Razor. `asp-page="/Movies/Index"`

Zapisz zmiany i przetestuj aplikację, klikając łącze **RpMovie** . Zobacz plik [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Shared/_Layout.cshtml) w serwisie GitHub.

### <a name="the-create-page-model"></a>Model tworzenia strony

Obejrzyj model stron */filmów/Utwórz. cshtml. cs* :

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]

::: moniker-end

`OnGet` Metoda inicjuje wszystkie Stany potrzebne dla strony. Strona tworzenia nie ma żadnego stanu do zainicjowania, więc `Page` jest zwracana. W dalszej części tego samouczka zobaczysz `OnGet` stan zainicjowania metody. Metoda tworzy obiekt, który renderuje stronę *Create. cshtml.* `Page` `PageResult`

Właściwość używa atrybutu, `[BindProperty]` aby wybrać [powiązanie modelu.](xref:mvc/models/model-binding) `Movie` Gdy formularz tworzenia zapisuje wartości formularza, środowisko uruchomieniowe ASP.NET Core tworzy powiązanie opublikowanych wartości z `Movie` modelem.

`OnPostAsync` Metoda jest uruchamiana, gdy strona zapisuje dane formularza:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Jeśli występują jakieś błędy modelu, formularz jest ponownie wyświetlany wraz z wszelkimi opublikowanymi danymi formularza. Większość błędów modelu można przechwycić po stronie klienta przed opublikowaniem formularza. Przykładem błędu modelu jest opublikowanie wartości pola daty, którego nie można przekonwertować na datę. Będziemy dowiedzieć się więcej o sprawdzaniu poprawności i weryfikacji modelu po stronie klienta w dalszej części tego samouczka.

Jeśli nie ma żadnych błędów modelu, dane zostaną zapisane i przeglądarka zostanie przekierowana na stronę indeksu.

### <a name="the-create-razor-page"></a>Strona tworzenie Razor

Przejrzyj strony */filmy/Utwórz* plik stronicowania Razor. cshtml:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).

![VS17 view of Create.cshtml page](page/_static/th.png)
-->
