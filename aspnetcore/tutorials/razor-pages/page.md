---
title: Szkieletu Razor strony platformy ASP.NET Core
author: rick-anderson
description: "W tym artykule wyjaśniono stron Razor generowane przez funkcję szkieletów."
ms.author: riande
manager: wpickett
ms.date: 09/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/page
ms.openlocfilehash: 9d8cdb6ef55d196d01cf6675443807ecfdbde5e0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a>Szkieletu Razor strony platformy ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku sprawdza stron Razor, tworzonych przez szkieletów w poprzednim temacie samouczek [Dodawanie modelu](xref:tutorials/razor-pages/model#scaffold-the-movie-model). 

[Wyświetlanie lub pobieranie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) próbki.

## <a name="the-create-delete-details-and-edit-pages"></a>Utwórz, Usuń, szczegóły i Edycja stron.

Sprawdź *Pages/Movies/Index.cshtml.cs* modelu strony:[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

Pochodne stron razor `PageModel`. Według konwencji `PageModel`-nosi nazwę klasy pochodnej `<PageName>Model`. Używa konstruktora [iniekcji zależności](xref:fundamentals/dependency-injection) można dodać `MovieContext` do strony. Wszystkie strony szkieletu wykonaj tego wzorca. Zobacz [kod asynchroniczny](xref:data/ef-rp/intro#asynchronous-code) uzyskać więcej informacji o asynchronicznych programing Entity Framework.

Po wysłaniu żądania dla tej strony, `OnGetAsync` metoda zwraca listę filmów na stronie aparatu Razor. `OnGetAsync`lub `OnGet` jest wywoływana na stronie aparatu Razor zainicjować stan strony. W takim przypadku `OnGetAsync` pobiera listę filmy i wyświetla je. 

Gdy `OnGet` zwraca `void` lub `OnGetAsync` zwraca`Task`, brak metody zwracany jest używany. Gdy typ zwrotny jest `IActionResult` lub `Task<IActionResult>`, należy podać instrukcji return. Na przykład *Pages/Movies/Create.cshtml.cs* `OnPostAsync` metody:

<!-- TODO - replace with snippet
[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
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

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor można przejście z kodu HTML w C# lub znacznika specyficzne dla elementu Razor. Gdy `@` następuje symbol [Razor zastrzeżone słowa kluczowego](xref:mvc/views/razor#razor-reserved-keywords), jego przejścia do znaczników specyficzne dla elementu Razor, w przeciwnym razie przejścia w języku C#.

`@page` Dyrektywy Razor sprawia, że plik na akcję MVC &mdash; co oznacza, że może obsłużyć żądania. `@page`musi być pierwszym dyrektywy Razor na stronie. `@page`jest przykładem przechodzi do znaczników specyficzne dla elementu Razor. Zobacz [składni Razor](xref:mvc/views/razor#razor-syntax) Aby uzyskać więcej informacji.

Sprawdź wyrażenie lambda, używane w następujących pomocnika kodu HTML:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

`DisplayNameFor` Przeprowadzający pomocnika kodu HTML `Title` właściwości, do którego odwołuje się wyrażenie lambda, aby ustalić nazwę wyświetlaną. Wyrażenie lambda jest sprawdzana zamiast obliczone. Oznacza to, że istnieje żadne naruszenie dostępu podczas `model`, `model.Movie`, lub `model.Movie[0]` są `null` lub jest pusty. Podczas oceny wyrażenia lambda (na przykład z `@Html.DisplayFor(modelItem => item.Title)`), wartości właściwości modelu.

<a name="md"></a>
### <a name="the-model-directive"></a>@model — Dyrektywa

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

`@model` Dyrektywa określa typ modelu przekazywane do strony Razor. W powyższym przykładzie `@model` wiersz sprawia, że `PageModel`-klasy dostępny na stronie aparatu Razor. Model jest używany w `@Html.DisplayNameFor` i `@Html.DisplayName` [pomocników HTML](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) na stronie.

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
###ViewData i układu

Rozważmy następujący kod:

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-)]

Poprzedni wyróżniony kod jest przykładem Razor przechodzi do języka C#. `{` i `}` znaków ujmij bloku kodu C#.

`PageModel` Ma klasy podstawowej `ViewData` właściwości słownik, który może służyć do dodania danych, które mają być przekazywane do widoku. Dodawanie obiektów do `ViewData` słownika przy użyciu wzorca klucza i wartości. W poprzednim przykładzie właściwość "Title" została dodana do `ViewData` słownika. Właściwość "Title" jest używana w *Pages/_Layout.cshtml* pliku. Następujący kod przedstawia pierwsze kilka wierszy *Pages/_Layout.cshtml* pliku.

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]

Wiersz `@*Markup removed for brevity.*@` komentarza Razor. W przeciwieństwie do komentarzy HTML (`<!-- -->`), Razor komentarze nie są wysyłane do klienta.

Uruchom aplikację i przetestować linki w projekcie (**Home**, **o**, **skontaktuj się z**, **Utwórz**, **Edytuj**, i **usunąć**). Każdej strony Ustawia tytuł, który można wyświetlić na karcie przeglądarki. Gdy zakładki na stronie tytuł jest używana do zakładki. *Pages/Index.cshtml* i *Pages/Movies/Index.cshtml* obecnie o takiej samej nazwie, ale można modyfikować mają różne wartości.

`Layout` Właściwość jest ustawiona *Pages/_ViewStart.cshtml* pliku:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

Poprzedni kod znaczników ustawia plik układu *Pages/_Layout.cshtml* wszystkich plików Razor w *stron* folderu. Zobacz [układu](xref:mvc/razor-pages/index#layout) Aby uzyskać więcej informacji.

### <a name="update-the-layout"></a>Aktualizacja układu

Zmień `<title>` element *Pages/_Layout.cshtml* plik krótszego ciągu.

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

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

### <a name="the-create-code-behind-page"></a>Utwórz stronę związane z kodem

Sprawdź *Pages/Movies/Create.cshtml.cs* pliku CodeBehind:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

`OnGet` Metoda inicjuje każdy stan potrzebne dla strony. Utwórz stronę nie ma żadnych stan inicjowania. `Page` Metoda tworzy `PageResult` obiektu, który renderuje *Create.cshtml* strony.

`Movie` Używa właściwości `[BindProperty]` atrybut, aby wyrazić zgodę na [modelu powiązania](xref:mvc/models/model-binding). Gdy formularz Utwórz przesyła wartości formularza, środowisko uruchomieniowe platformy ASP.NET Core wiąże przesłanych wartości do `Movie` modelu.

`OnPostAsync` Metody jest uruchamiany, gdy strona zapisuje dane formularza:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Jeśli wystąpią jakieś błędy modelu, formularza zostanie wyświetlony ponownie, wraz z danymi formularza opublikowane. Większość błędów modelu może być wykrytych po stronie klienta, przed opublikowania formularza. Przykład błąd modelu jest przesyłanie wartość pola daty, której nie można przekonwertować na typ date. Firma Microsoft będzie komunikować więcej informacji na temat weryfikacji po stronie klienta i sprawdzania poprawności modelu później w samouczku.

Jeśli nie ma żadnych błędów w modelu, dane są zapisywane i przeglądarki, zostanie przekierowany do strony indeksu.

### <a name="the-create-razor-page"></a>Utwórz stronę Razor

Sprawdź *Pages/Movies/Create.cshtml* pliku Razor strony:

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

Wyświetla programu Visual Studio `<form method="post">` tag w charakterystyczne czcionkę dla pomocników tagów. `<form method="post">` Jest element [pomocnika Tag formularza](xref:mvc/views/working-with-forms#the-form-tag-helper). Pomocnik Tag formularza automatycznie uwzględnia [antiforgery token](xref:security/anti-request-forgery).

![VS17 widok Create.cshtml strony](page/_static/th.png)

Aparat szkieletów tworzy znaczników Razor dla każdego pola w modelu (z wyjątkiem Identyfikatora) podobny do następującego:

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

[Pomocników tagów weryfikacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` i ` <span asp-validation-for`) wyświetla błędy sprawdzania poprawności. Sprawdzanie poprawności zostało opisane bardziej szczegółowo w dalszej części tej serii.

[Pomocnika tagów etykiety](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generuje podpis etykiety i `for` atrybutu dla `Title` właściwości.

[Pomocnika Tag danych wejściowych](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) używa [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atrybutów i tworzy wymagane dla technologii jQuery weryfikacji po stronie klienta atrybutów HTML.

Następny samouczek wyjaśnia bazy danych LocalDB programu SQL Server i wstępne wypełnianie bazy danych.

>[!div class="step-by-step"]
[Poprzedni: Dodawanie modelu](xref:tutorials/razor-pages/model)
[dalej: bazy danych LocalDB programu SQL Server](xref:tutorials/razor-pages/sql)
