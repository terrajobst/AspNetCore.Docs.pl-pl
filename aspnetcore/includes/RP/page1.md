# <a name="scaffolded-razor-pages-in-aspnet-core"></a>Szkieletu Razor strony platformy ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku sprawdza stron Razor, tworzonych przez szkieletów poprzedniej samouczka. 

[Wyświetlanie lub pobieranie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) próbki.

## <a name="the-create-delete-details-and-edit-pages"></a>Utwórz, Usuń, szczegóły i Edycja stron.

Sprawdź *Pages/Movies/Index.cshtml.cs* modelu strony:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

Pochodne stron razor `PageModel`. Według konwencji `PageModel`-nosi nazwę klasy pochodnej `<PageName>Model`. Używa konstruktora [iniekcji zależności](xref:fundamentals/dependency-injection) można dodać `MovieContext` do strony. Wszystkie strony szkieletu wykonaj tego wzorca. Zobacz [kod asynchroniczny](xref:data/ef-rp/intro#asynchronous-code) uzyskać więcej informacji o asynchronicznych programing Entity Framework.

Po wysłaniu żądania dla tej strony, `OnGetAsync` metoda zwraca listę filmów na stronie aparatu Razor. `OnGetAsync` lub `OnGet` jest wywoływana na stronie aparatu Razor zainicjować stan strony. W takim przypadku `OnGetAsync` pobiera listę filmy i wyświetla je. 

Gdy `OnGet` zwraca `void` lub `OnGetAsync` zwraca`Task`, brak metody zwracany jest używany. Gdy typ zwrotny jest `IActionResult` lub `Task<IActionResult>`, należy podać instrukcji return. Na przykład *Pages/Movies/Create.cshtml.cs* `OnPostAsync` metody:

<!-- TODO - replace with snippet
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
 -->

```csharp
public async Task<IActionResult> OnPostAsync()
{
    if (!ModelState.IsValid)
    {
        return Page();
    }

    _context.Movie.Add(Movie);
    await _context.SaveChangesAsync();

    return RedirectToPage("./Index");
}
```

Sprawdź *Pages/Movies/Index.cshtml* Razor strony:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor można przejście z kodu HTML w C# lub znacznika specyficzne dla elementu Razor. Gdy `@` następuje symbol [Razor zastrzeżone słowa kluczowego](xref:mvc/views/razor#razor-reserved-keywords), jego przejścia do znaczników specyficzne dla elementu Razor, w przeciwnym razie przejścia w języku C#.

`@page` Dyrektywy Razor sprawia, że plik na akcję MVC &mdash; co oznacza, że może obsłużyć żądania. `@page` musi być pierwszym dyrektywy Razor na stronie. `@page` jest przykładem przechodzi do znaczników specyficzne dla elementu Razor. Zobacz [składni Razor](xref:mvc/views/razor#razor-syntax) Aby uzyskać więcej informacji.

Sprawdź wyrażenie lambda, używane w następujących pomocnika kodu HTML:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

`DisplayNameFor` Przeprowadzający pomocnika kodu HTML `Title` właściwości, do którego odwołuje się wyrażenie lambda, aby ustalić nazwę wyświetlaną. Wyrażenie lambda jest sprawdzana zamiast obliczone. Oznacza to, że istnieje żadne naruszenie dostępu podczas `model`, `model.Movie`, lub `model.Movie[0]` są `null` lub jest pusty. Podczas oceny wyrażenia lambda (na przykład z `@Html.DisplayFor(modelItem => item.Title)`), wartości właściwości modelu.

<a name="md"></a>
### <a name="the-model-directive"></a>@model — Dyrektywa

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

`@model` Dyrektywa określa typ modelu przekazywane do strony Razor. W powyższym przykładzie `@model` wiersz sprawia, że `PageModel`-klasy dostępny na stronie aparatu Razor. Model jest używany w `@Html.DisplayNameFor` i `@Html.DisplayName` [pomocników HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) na stronie.

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData i układu

Rozważmy następujący kod:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

Poprzedni wyróżniony kod jest przykładem Razor przechodzi do języka C#. `{` i `}` znaków ujmij bloku kodu C#.

`PageModel` Ma klasy podstawowej `ViewData` właściwości słownik, który może służyć do dodania danych, które mają być przekazywane do widoku. Dodawanie obiektów do `ViewData` słownika przy użyciu wzorca klucza i wartości. W poprzednim przykładzie właściwość "Title" została dodana do `ViewData` słownika. 

::: moniker range="= aspnetcore-2.0"

Właściwość "Title" jest używana w *Pages/_Layout.cshtml* pliku. Następujący kod przedstawia pierwsze kilka wierszy *Pages/_Layout.cshtml* pliku.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Właściwość "Title" jest używana w *Pages/Shared/_Layout.cshtml* pliku. Następujący kod przedstawia pierwsze kilka wierszy *_Layout.cshtml* pliku.

::: moniker-end

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

Wiersz `@*Markup removed for brevity.*@` komentarza Razor. W przeciwieństwie do komentarzy HTML (`<!-- -->`), Razor komentarze nie są wysyłane do klienta.

Uruchom aplikację i przetestować linki w projekcie (**Home**, **o**, **skontaktuj się z**, **Utwórz**, **Edytuj**, i **usunąć**). Każdej strony Ustawia tytuł, który można wyświetlić na karcie przeglądarki. Gdy zakładki na stronie tytuł jest używana do zakładki. *Pages/Index.cshtml* i *Pages/Movies/Index.cshtml* obecnie o takiej samej nazwie, ale można modyfikować mają różne wartości.

> [!NOTE]
> Nie można wprowadzić przecinki dziesiętne w `Price` pola. Do obsługi [weryfikacji jQuery](https://jqueryvalidation.org/) dla innych niż angielski, które użyj przecinka (",") dla punktu dziesiętnego i formaty daty z systemem innym niż angielski, należy wykonać kroki, aby globalize aplikacji. To [GitHub problem 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) instrukcje dotyczące dodawania przecinkiem.

`Layout` Właściwość jest ustawiona *Pages/_ViewStart.cshtml* pliku:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

Poprzedni kod znaczników ustawia plik układu *Pages/_Layout.cshtml* wszystkich plików Razor w *stron* folderu. Zobacz [układu](xref:razor-pages/index#layout) Aby uzyskać więcej informacji.

### <a name="update-the-layout"></a>Aktualizacja układu

Zmień `<title>` element *Pages/_Layout.cshtml* plik krótszego ciągu.

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

Znajdź następujący element zakotwiczenia w *Pages/_Layout.cshtml* pliku.

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
Zastąp następujący kod znaczników poprzedni element.

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

Poprzedni element zakotwiczenia jest [pomocnika tagów](xref:mvc/views/tag-helpers/intro). W takim przypadku ma [pomocnika Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). `asp-page="/Movies/Index"` Atrybut pomocnika tagów i wartość tworzy łącze do `/Movies/Index` Razor strony.

Zapisz zmiany i przetestować aplikację, klikając **RpMovie** łącza. Zobacz [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) pliku w witrynie GitHub.

### <a name="the-create-page-model"></a>Utwórz model strony

Sprawdź *Pages/Movies/Create.cshtml.cs* modelu strony:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]
::: moniker-end


`OnGet` Metoda inicjuje każdy stan potrzebne dla strony. Utwórz stronę nie ma żadnych stan inicjowania to `Page` jest zwracany. Później w samouczku zobacz `OnGet` metody zainicjować stanu. `Page` Metoda tworzy `PageResult` obiektu, który renderuje *Create.cshtml* strony.

`Movie` Używa właściwości `[BindProperty]` atrybut, aby wyrazić zgodę na [modelu powiązania](xref:mvc/models/model-binding). Gdy formularz Utwórz przesyła wartości formularza, środowisko uruchomieniowe platformy ASP.NET Core wiąże przesłanych wartości do `Movie` modelu.

`OnPostAsync` Metody jest uruchamiany, gdy strona zapisuje dane formularza:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Jeśli wystąpią jakieś błędy modelu, formularza zostanie wyświetlony ponownie, wraz z danymi formularza opublikowane. Większość błędów modelu może być wykrytych po stronie klienta, przed opublikowania formularza. Przykład błąd modelu jest przesyłanie wartość pola daty, której nie można przekonwertować na typ date. Firma Microsoft będzie komunikować więcej informacji na temat weryfikacji po stronie klienta i sprawdzania poprawności modelu później w samouczku.

Jeśli nie ma żadnych błędów w modelu, dane są zapisywane i przeglądarki, zostanie przekierowany do strony indeksu.

### <a name="the-create-razor-page"></a>Utwórz stronę Razor

Sprawdź *Pages/Movies/Create.cshtml* pliku Razor strony:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
