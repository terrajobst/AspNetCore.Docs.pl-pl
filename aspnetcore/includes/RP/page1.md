# <a name="scaffolded-razor-pages-in-aspnet-core"></a>Strony razor ze szkieletami w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku sprawdza, czy strony Razor, powstałe w wyniku tworzenia szkieletów w poprzednim samouczku. 

[Wyświetlanie lub pobieranie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) próbki.

## <a name="the-create-delete-details-and-edit-pages"></a>Tworzenie, usuwanie, szczegóły i edycji stron.

Sprawdź *Pages/Movies/Index.cshtml.cs* modelu strony:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

Strony razor są uzyskiwane z `PageModel`. Zgodnie z Konwencją `PageModel`-klasy pochodnej jest wywoływana `<PageName>Model`. Używa konstruktora [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) dodać `MovieContext` ze stroną. Wszystkie strony szkieletu korzystać z tego wzoru. Zobacz [kodu asynchronicznego](xref:data/ef-rp/intro#asynchronous-code) więcej informacji na temat asynchronicznej programistyczne z programem Entity Framework.

Po wysłaniu żądania dla tej strony, `OnGetAsync` metoda zwraca listę filmów do strony Razor. `OnGetAsync` lub `OnGet` jest wywoływana w stronę Razor do zainicjowania stanu dla strony. W tym przypadku `OnGetAsync` pobiera listę filmów, a następnie wyświetli je.

Gdy `OnGet` zwraca `void` lub `OnGetAsync` zwraca`Task`, brak zwracany metody jest używana. Gdy typ zwrotny jest `IActionResult` lub `Task<IActionResult>`, musi być podana instrukcji return. Na przykład *Pages/Movies/Create.cshtml.cs* `OnPostAsync` metody:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Create.cshtml.cs?name=snippet)]

Sprawdź *Pages/Movies/Index.cshtml* strona Razor:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor można przejść z kodu HTML w języku C# lub w znaczników specyficzne dla aparatu Razor. Gdy `@` następuje symbol [Razor zastrzeżone słowa kluczowego](xref:mvc/views/razor#razor-reserved-keywords), przechodzi do znaczników specyficzne dla aparatu Razor, w przeciwnym razie przejścia w języku C#.

`@page` Dyrektywy Razor sprawia, że plik na akcję MVC &mdash; co oznacza, że może obsługiwać żądań. `@page` musi być pierwszym dyrektywy Razor na stronie. `@page` jest przykładem przechodzi do znaczników specyficzne dla aparatu Razor. Zobacz [składni Razor](xref:mvc/views/razor#razor-syntax) Aby uzyskać więcej informacji.

Sprawdź wyrażenie lambda, używane w następujących pomocnika kodu HTML:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

`DisplayNameFor` Sprawdza pomocnika kodu HTML `Title` właściwość, do którego odwołuje się wyrażenie lambda, aby ustalić nazwę wyświetlaną. Wyrażenie lambda jest kontrolowane zamiast ocenione. Oznacza, że ma nie naruszenie zasad dostępu podczas `model`, `model.Movie`, lub `model.Movie[0]` są `null` lub jest pusty. Kiedy jest obliczane wyrażenie lambda (na przykład za pomocą `@Html.DisplayFor(modelItem => item.Title)`), są oceniane wartości właściwości modelu.

<a name="md"></a>
### <a name="the-model-directive"></a>@model — Dyrektywa

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

`@model` Dyrektywa określa typ modelu przekazywane do stron Razor. W powyższym przykładzie `@model` wiersz sprawia, że `PageModel`— dostępny na stronie Razor klasy pochodnej. Model jest używany w `@Html.DisplayNameFor` i `@Html.DisplayName` [pomocników HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) na stronie.

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData i układu

Rozważmy poniższy kod:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

Poprzedni wyróżniony kod jest przykładem Razor przechodzi do języka C#. `{` i `}` znaków należy umieścić blok kodu C#.

`PageModel` Ma klasę bazową `ViewData` słownika właściwości, który może służyć do dodania danych, które mają być przekazywane do widoku. Dodawanie obiektów do `ViewData` słownika przy użyciu wzorca klucz/wartość. W poprzednim przykładzie, właściwość "Title" zostanie dodany do `ViewData` słownika. 

::: moniker range="= aspnetcore-2.0"

Właściwość "Title" jest używana w *Pages/Shared/_Layout.cshtml* pliku. Następujący kod przedstawia pierwszych kilka wierszy tego *Pages/Shared/_Layout.cshtml* pliku.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Właściwość "Title" jest używana w *Pages/Shared/_Layout.cshtml* pliku. Następujący kod przedstawia pierwszych kilka wierszy tego *_Layout.cshtml* pliku.

::: moniker-end

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

Wiersz `@*Markup removed for brevity.*@` komentarza Razor. W przeciwieństwie do komentarzy HTML (`<!-- -->`), komentarze Razor nie są wysyłane do klienta.

Uruchom aplikację i przetestować linki w projekcie (**Home**, **o**, **skontaktuj się z pomocą**, **Utwórz**, **Edytuj**, i **Usuń**). Każda strona Ustawia tytuł, który można wyświetlić na karcie przeglądarki. Gdy zakładki na stronie tytuł jest używana do zakładki. *Pages/Index.cshtml* i *Pages/Movies/Index.cshtml* obecnie o takiej samej nazwie, ale możesz modyfikować je mogą mieć różne wartości.

> [!NOTE]
> Nie można wprowadzić dziesiętna przecinkami w `Price` pola. Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") dla punktu dziesiętnego i formaty daty inne niż angielski, należy wykonać kroki, aby sprzedawać aplikację. To [problem w usłudze GitHub 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) instrukcje dotyczące dodawania przecinek dziesiętny.

`Layout` Właściwość jest ustawiona *Pages/_ViewStart.cshtml* pliku:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

Poprzedni kod znaczników ustawia plik układu *Pages/Shared/_Layout.cshtml* dla wszystkich plików Razor, w obszarze *stron* folderu. Zobacz [układ](xref:razor-pages/index#layout) Aby uzyskać więcej informacji.

### <a name="update-the-layout"></a>Aktualizowanie układu

Zmiana `<title>` element *Pages/Shared/_Layout.cshtml* pliku, aby użyć krótszego ciągu.

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

Znajdź następujący element zakotwiczenia w *Pages/_Layout.cshtml* pliku.

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
Zastąp poprzedzający element następującym kodem.

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

Poprzedni element zakotwiczenia jest [Pomocnik tagu](xref:mvc/views/tag-helpers/intro). W tym przypadku ma [Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). `asp-page="/Movies/Index"` Atrybut pomocnika tagów i wartość tworzy łącze do `/Movies/Index` strona Razor.

Zapisz zmiany i przetestować aplikację, klikając **RpMovie** łącza. Zobacz [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) pliku w usłudze GitHub.

### <a name="the-create-page-model"></a>Tworzenie modelu strony

Sprawdź *Pages/Movies/Create.cshtml.cs* modelu strony:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]
::: moniker-end


`OnGet` Metoda inicjuje każdy stan potrzebne dla strony. Strona tworzenia nie ma każdy stan, aby zainicjować, więc `Page` jest zwracana. W dalszej części tego samouczka zostanie wyświetlony `OnGet` metoda inicjowania stanu. `Page` Metoda tworzy `PageResult` obiektu, który renderuje *Create.cshtml* strony.

`Movie` Używa właściwości `[BindProperty]` atrybutu, aby wyrazić zgodę na [wiązanie modelu](xref:mvc/models/model-binding). Gdy formularz Utwórz publikuje wartości formularza, środowisko uruchomieniowe programu ASP.NET Core wiąże przesłanych wartości w celu `Movie` modelu.

`OnPostAsync` Metoda jest uruchomiona, gdy Strona publikuje dane formularza:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Jeśli występują błędy modelu, formularza zostanie wyświetlony ponownie, oraz wszelkie dane formularza opublikowane. Większość błędów modelu mogą być przechwytywane po stronie klienta, zanim opublikowania formularza. Przykładem błąd modelu publikuje wartość pola daty, którego nie można przekonwertować na wartość typu date. Omówimy więcej informacji na temat weryfikacji po stronie klienta i weryfikacja modelu w dalszej części tego samouczka.

Jeśli nie ma żadnych błędów modelu, dane są zapisywane, a przeglądarka jest przekierowywana na strony indeksu.

### <a name="the-create-razor-page"></a>Tworzenie strony Razor

Sprawdź *Pages/Movies/Create.cshtml* wartość pola plik strony Razor:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
