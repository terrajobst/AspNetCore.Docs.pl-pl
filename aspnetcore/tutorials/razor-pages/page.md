---
title: Strony razor ze szkieletami w programie ASP.NET Core
author: rick-anderson
description: W tym artykule wyjaśniono stron Razor, generowane przez tworzenie szkieletów.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 12/4/2018
uid: tutorials/razor-pages/page
ms.openlocfilehash: f97930c9e09dbf46acc9e91aff9469db8970fa77
ms.sourcegitcommit: a91e8dd2f4b788114c8bc834507277f4b5e8d6c5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55712292"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a>Strony razor ze szkieletami w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku sprawdza, czy strony Razor, powstałe w wyniku tworzenia szkieletów w poprzednim samouczku.

[Wyświetlanie lub pobieranie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) próbki.

## <a name="the-create-delete-details-and-edit-pages"></a>Tworzenie, usuwanie, szczegóły i edycji stron

Sprawdź *Pages/Movies/Index.cshtml.cs* modelu strony:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

Strony razor są uzyskiwane z `PageModel`. Zgodnie z Konwencją `PageModel`-klasy pochodnej jest wywoływana `<PageName>Model`. Używa konstruktora [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) dodać `RazorPagesMovieContext` ze stroną. Wszystkie strony szkieletu korzystać z tego wzoru. Zobacz [kodu asynchronicznego](xref:data/ef-rp/intro#asynchronous-code) więcej informacji na temat asynchronicznej programistyczne z programem Entity Framework.

Po wysłaniu żądania dla tej strony, `OnGetAsync` metoda zwraca listę filmów do strony Razor. `OnGetAsync` lub `OnGet` jest wywoływana w stronę Razor do zainicjowania stanu dla strony. W tym przypadku `OnGetAsync` pobiera listę filmów, a następnie wyświetli je.

Gdy `OnGet` zwraca `void` lub `OnGetAsync` zwraca`Task`, brak zwracany metody jest używana. Gdy typ zwrotny jest `IActionResult` lub `Task<IActionResult>`, musi być podana instrukcji return. Na przykład *Pages/Movies/Create.cshtml.cs* `OnPostAsync` metody:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> Sprawdź *Pages/Movies/Index.cshtml* strona Razor:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor można przejść z kodu HTML w języku C# lub w znaczników specyficzne dla aparatu Razor. Gdy `@` następuje symbol [Razor zastrzeżone słowa kluczowego](xref:mvc/views/razor#razor-reserved-keywords), przechodzi do znaczników specyficzne dla aparatu Razor, w przeciwnym razie przejścia w języku C#.

`@page` Dyrektywy Razor sprawia, że plik do Akcja kontrolera MVC, co oznacza, że może obsługiwać żądań. `@page` musi być pierwszym dyrektywy Razor na stronie. `@page` jest przykładem przechodzi do znaczników specyficzne dla aparatu Razor. Zobacz [składni Razor](xref:mvc/views/razor#razor-syntax) Aby uzyskać więcej informacji.

Sprawdź wyrażenie lambda, używane w następujących pomocnika kodu HTML:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

`DisplayNameFor` Sprawdza pomocnika kodu HTML `Title` właściwość, do którego odwołuje się wyrażenie lambda, aby ustalić nazwę wyświetlaną. Wyrażenie lambda jest kontrolowane zamiast ocenione. Oznacza, że ma nie naruszenie zasad dostępu podczas `model`, `model.Movie`, lub `model.Movie[0]` są `null` lub jest pusty. Kiedy jest obliczane wyrażenie lambda (na przykład za pomocą `@Html.DisplayFor(modelItem => item.Title)`), są oceniane wartości właściwości modelu.

<a name="md"></a>
### <a name="the-model-directive"></a>@model — Dyrektywa

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

`@model` Dyrektywa określa typ modelu przekazywane do stron Razor. W powyższym przykładzie `@model` wiersz sprawia, że `PageModel`— dostępny na stronie Razor klasy pochodnej. Model jest używany w `@Html.DisplayNameFor` i `@Html.DisplayFor` [pomocników HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) na stronie.

### <a name="the-layout-page"></a>Strona układu

Wybierz linki menu (**RazorPagesMovie**, **Home**, i **zachowania**). Każda strona zawiera ten sam układ menu. Układ menu jest zaimplementowana w *Pages/Shared/_Layout.cshtml* pliku. Otwórz *Pages/Shared/_Layout.cshtml* pliku.

[Układ](xref:mvc/views/layout) Szablony umożliwiają określ układ kontenera HTML Twojej witryny w jednym miejscu, a następnie zastosować je na wielu stronach w witrynie. Znajdź `@RenderBody()` wiersza. `RenderBody` jest symbolem zastępczym, gdzie wszystkie specyficzne dla strony widoki, których tworzenie pokazu, *opakowane* w stronę układu. Na przykład w przypadku wybrania **zachowania** linku **Pages/Privacy.cshtml** renderowania widoku w `RenderBody` metody.

<a name="vd"></a>
### <a name="viewdata-and-layout"></a>ViewData i układu

Rozważmy poniższy kod z *Pages/Movies/Index.cshtml* pliku:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

Poprzedni wyróżniony kod jest przykładem Razor przechodzi do języka C#. `{` i `}` znaków należy umieścić blok kodu C#.

`PageModel` Ma klasę bazową `ViewData` słownika właściwości, który może służyć do dodania danych, które mają być przekazywane do widoku. Dodawanie obiektów do `ViewData` słownika przy użyciu wzorca klucz/wartość. W poprzednim przykładzie, właściwość "Title" zostanie dodany do `ViewData` słownika. 

Właściwość "Title" jest używana w *Pages/Shared/_Layout.cshtml* pliku. Następujący kod przedstawia pierwszych kilka wierszy tego *_Layout.cshtml* pliku.

<!-- we need a snapshot copy of layout because we are
changing in in the next step. 
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

Wiersz `@*Markup removed for brevity.*@` komentarz Razor, które nie znajdują się w pliku układu. W przeciwieństwie do komentarzy HTML (`<!-- -->`), komentarze Razor nie są wysyłane do klienta.

### <a name="update-the-layout"></a>Aktualizowanie układu

Zmiana `<title>` element *Pages/Shared/_Layout.cshtml* pliku wyświetlania **filmu** zamiast **RazorPagesMovie**.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]


Znajdź następujący element zakotwiczenia w *Pages/Shared/_Layout.cshtml* pliku.

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

Zastąp poprzedzający element następującym kodem.

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

Poprzedni element zakotwiczenia jest [Pomocnik tagu](xref:mvc/views/tag-helpers/intro). W tym przypadku ma [Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). `asp-page="/Movies/Index"` Atrybut pomocnika tagów i wartość tworzy łącze do `/Movies/Index` strona Razor. `asp-area` Wartość atrybutu jest puste, dzięki czemu obszar nie jest używane w linku. Zobacz [obszarów](xref:mvc/controllers/areas) Aby uzyskać więcej informacji.

Zapisz zmiany i przetestować aplikację, klikając **RpMovie** łącza. Zobacz [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) plik w usłudze GitHub, jeśli masz jakiekolwiek problemy.

Testowanie inne linki (**Home**, **RpMovie**, **Utwórz**, **Edytuj**, i **Usuń**). Każda strona Ustawia tytuł, który można wyświetlić na karcie przeglądarki. Gdy zakładki na stronie tytuł jest używana do zakładki.

> [!NOTE]
> Nie można wprowadzić dziesiętna przecinkami w `Price` pola. Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") dla punktu dziesiętnego i formaty daty inne niż angielski, należy wykonać kroki, aby sprzedawać aplikację. To [problem w usłudze GitHub 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) instrukcje dotyczące dodawania przecinek dziesiętny.

`Layout` Właściwość jest ustawiona *Pages/_ViewStart.cshtml* pliku:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

Poprzedni kod znaczników ustawia plik układu *Pages/Shared/_Layout.cshtml* dla wszystkich plików Razor, w obszarze *stron* folderu. Zobacz [układ](xref:razor-pages/index#layout) Aby uzyskać więcej informacji.

### <a name="the-create-page-model"></a>Tworzenie modelu strony

Sprawdź *Pages/Movies/Create.cshtml.cs* modelu strony:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

`OnGet` Metoda inicjuje każdy stan potrzebne dla strony. Strona tworzenia nie ma każdy stan, aby zainicjować, więc `Page` jest zwracana. W dalszej części tego samouczka zostanie wyświetlony `OnGet` metoda inicjowania stanu. `Page` Metoda tworzy `PageResult` obiektu, który renderuje *Create.cshtml* strony.

`Movie` Używa właściwości `[BindProperty]` atrybutu, aby wyrazić zgodę na [wiązanie modelu](xref:mvc/models/model-binding). Gdy formularz Utwórz publikuje wartości formularza, środowisko uruchomieniowe programu ASP.NET Core wiąże przesłanych wartości w celu `Movie` modelu.

`OnPostAsync` Metoda jest uruchomiona, gdy Strona publikuje dane formularza:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Jeśli występują błędy modelu, formularza zostanie wyświetlony ponownie, oraz wszelkie dane formularza opublikowane. Większość błędów modelu mogą być przechwytywane po stronie klienta, zanim opublikowania formularza. Przykładem błąd modelu publikuje wartość pola daty, którego nie można przekonwertować na wartość typu date. Weryfikacja po stronie klienta i sprawdzanie poprawności modelu zostały omówione w dalszej części tego samouczka.

Jeśli nie ma żadnych błędów modelu, dane są zapisywane, a przeglądarka jest przekierowywana na strony indeksu.

### <a name="the-create-razor-page"></a>Tworzenie strony Razor

Sprawdź *Pages/Movies/Create.cshtml* wartość pola plik strony Razor:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio Wyświetla `<form method="post">` tag szczególne pogrubioną czcionką używany dla pomocników tagów:

![VS17 widok Create.cshtml strony](page/_static/th.png)
<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Aby uzyskać więcej informacji na temat pomocnicy tagów, takie jak `<form method="post">`, zobacz [pomocników tagów w programie ASP.NET Core](xref:mvc/views/tag-helpers/intro).

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

Program Visual Studio for Mac Wyświetla `<form method="post">` tag szczególne pogrubioną czcionką używany dla pomocników tagów.

---  
<!-- End of VS tabs -->

`<form method="post">` Element jest [Pomocnik tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper). Pomocnik tagu formularza automatycznie uwzględnia [antiforgery token](xref:security/anti-request-forgery).

Aparat tworzenia szkieletów tworzy znaczników Razor dla każdego pola w modelu (z wyjątkiem identyfikator) podobny do następującego:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

[Pomocników tagów weryfikacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` i ` <span asp-validation-for`) wyświetla błędy sprawdzania poprawności. Sprawdzanie poprawności jest omówiona bardziej szczegółowo w dalszej części tej serii.

[Pomocnik tagu etykiet](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generuje podpis etykiety i `for` atrybutu dla `Title` właściwości.

[Pomocnik tagu dane wejściowe](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) używa [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atrybutów, a następnie tworzy atrybutów HTML potrzebne dla technologii jQuery weryfikacji po stronie klienta.

> [!div class="step-by-step"]
> [Poprzednie: Dodawanie modelu](xref:tutorials/razor-pages/model)
> [dalej: Database](xref:tutorials/razor-pages/sql)
