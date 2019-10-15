---
title: Razor Pages szkieletowe w ASP.NET Core
author: rick-anderson
description: Wyjaśnia Razor Pages wygenerowane przez szkielety.
ms.author: riande
ms.date: 08/17/2019
uid: tutorials/razor-pages/page
ms.openlocfilehash: 939ed5c3cdf33d8d99712e3166d8d07d3bac719f
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334080"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a>Razor Pages szkieletowe w ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten samouczek służy do badania Razor Pages utworzonych przez tworzenie szkieletów w [poprzednim samouczku](xref:tutorials/razor-pages/model).

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="the-create-delete-details-and-edit-pages"></a>Strony tworzenie, usuwanie, szczegóły i edycja

Zapoznaj się z modelem stron */filmów/index. cshtml. cs* :

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs)]

Razor Pages pochodzą od `PageModel`. Zgodnie z Konwencją Klasa pochodna @no__t -0 jest nazywana `<PageName>Model`. Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) , aby dodać `RazorPagesMovieContext` do strony. Wszystkie strony szkieletowe są zgodne z tym wzorcem. Zobacz [kod asynchroniczny](xref:data/ef-rp/intro#asynchronous-code) , aby uzyskać więcej informacji na temat programowania asynchronicznego przy użyciu Entity Framework.

Gdy żądanie jest wykonywane dla strony, Metoda `OnGetAsync` zwraca listę filmów do strony Razor. `OnGetAsync` lub `OnGet` jest wywoływana w celu zainicjowania stanu strony. W takim przypadku `OnGetAsync` pobiera listę filmów i wyświetla je.

Gdy `OnGet` zwraca `void` lub `OnGetAsync` zwraca wartość @ no__t-3, nie jest używana instrukcja return. Gdy typem zwracanym jest `IActionResult` lub `Task<IActionResult>`, należy podać instrukcję return. Na przykład: *Pages/Films/Create. cshtml. cs* `OnPostAsync`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a>Przejrzyj stronę */filmy/index. cshtml* Razor:

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml)]

Razor można przenieść z HTML do C# lub do znacznika specyficznego dla Razor. Gdy po symbolu `@` następuje [słowo kluczowe zarezerwowane Razor](xref:mvc/views/razor#razor-reserved-keywords), przechodzi do znacznika specyficznego dla Razor, w przeciwnym razie przechodzi do C#.

### <a name="the-page-directive"></a>Dyrektywa @page

Dyrektywa Razor `@page` powoduje, że plik jest akcją MVC, co oznacza, że może obsługiwać żądania. `@page` musi być pierwszą dyrektywą Razor na stronie. `@page` to przykład przejścia do znacznika specyficznego dla Razor. Aby uzyskać więcej informacji, zobacz [składnia Razor](xref:mvc/views/razor#razor-syntax) .

Bada wyrażenie lambda użyte w następującym Pomocniku HTML:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

Pomocnik HTML `DisplayNameFor` sprawdza Właściwość `Title`, do której odwołuje się wyrażenie lambda w celu określenia nazwy wyświetlanej. Wyrażenie lambda jest sprawdzane, a nie oceniane. Oznacza to, że nie ma żadnych naruszeń dostępu, gdy `model`, `model.Movie` lub `model.Movie[0]` jest `null` lub puste. Gdy wyrażenie lambda jest oceniane (na przykład z `@Html.DisplayFor(modelItem => item.Title)`), wartości właściwości modelu są oceniane.

<a name="md"></a>

### <a name="the-model-directive"></a>Dyrektywa @model

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

Dyrektywa `@model` określa typ modelu przekazaną do strony Razor. W poprzednim przykładzie wiersz `@model` powoduje, że @no__t Klasa pochodna -1 jest dostępna dla strony Razor. Model jest używany w [pomocnikach HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` i `@Html.DisplayFor` na stronie.

### <a name="the-layout-page"></a>Strona układu

Wybierz linki menu (**RazorPagesMovie**, **Home**i **privacy**). Każda Strona wyświetla ten sam układ menu. Układ menu jest implementowany w pliku *Pages/Shared/_Layout. cshtml* . Otwórz plik *Pages/Shared/_Layout. cshtml* .

Szablony [układów](xref:mvc/views/layout) umożliwiają układ kontenera HTML:

* Określone w jednym miejscu.
* Stosowane na wielu stronach w witrynie.

Znajdź wiersz `@RenderBody()`. `RenderBody` jest symbolem zastępczym, w którym wszystkie widoki specyficzne dla strony są wyświetlane, *opakowane* na stronie układ. Na przykład wybierz łącze **prywatność** i widok *strony/prywatność. cshtml* jest renderowany w metodzie `RenderBody`.

<a name="vd"></a>

### <a name="viewdata-and-layout"></a>ViewData i układ

Rozważ następujące oznakowanie w pliku *Pages/Films/index. cshtml* :

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

Poprzedni wyróżniony znacznik jest przykładem przejścia Razor do C#. Znaki `{` i `}` obejmują blok C# kodu.

Klasa bazowa `PageModel` zawiera właściwość słownika `ViewData`, która może służyć do przekazywania danych do widoku. Obiekty są dodawane do słownika `ViewData` przy użyciu wzorca klucz/wartość. W poprzednim przykładzie właściwość `"Title"` jest dodawana do słownika `ViewData`.

Właściwość `"Title"` jest używana w pliku *Pages/Shared/_Layout. cshtml* . Poniższy znacznik pokazuje pierwsze kilka wierszy pliku *_Layout. cshtml* .

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6)]

Wiersz `@*Markup removed for brevity.*@` jest komentarzem Razor. W przeciwieństwie do komentarzy HTML (`<!-- -->`), komentarze Razor nie są wysyłane do klienta.

### <a name="update-the-layout"></a>Aktualizowanie układu

Zmień element `<title>` w pliku *Pages/Shared/_Layout. cshtml* , aby wyświetlał **film** , a nie **RazorPagesMovie**.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

Znajdź następujący element zakotwiczony w pliku *Pages/Shared/_Layout. cshtml* .

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

Zastąp poprzedni element następującym znacznikiem:

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

Poprzedni element zakotwiczenia jest [pomocnikiem tagów](xref:mvc/views/tag-helpers/intro). W tym przypadku jest to [pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). Atrybut i wartość pomocnika tagu `asp-page="/Movies/Index"` tworzy łącze do strony Razor `/Movies/Index`. Wartość atrybutu `asp-area` jest pusta, dlatego obszar nie jest używany w łączu. Aby uzyskać więcej informacji, zobacz [obszary](xref:mvc/controllers/areas) .

Zapisz zmiany i przetestuj aplikację, klikając łącze **RpMovie** . Jeśli występują problemy, zobacz plik [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) w usłudze GitHub.

Przetestuj inne linki (**Narzędzia główne**, **RpMovie**, **Utwórz**, **Edytuj**i **Usuń**). Każda Strona ustawia tytuł, który można zobaczyć na karcie przeglądarki. Po utworzeniu zakładki na stronie tytuł jest używany dla zakładki.

> [!NOTE]
> W polu `Price` nie można wprowadzać przecinków dziesiętnych. Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i formatów dat innych niż angielski, należy wykonać kroki w celu globalizacji aplikacji. Aby uzyskać instrukcje dotyczące dodawania przecinków dziesiętnych, zobacz ten problem w usłudze [GitHub 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) .

Właściwość `Layout` jest ustawiona w pliku *Pages/_ViewStart. cshtml* :

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/_ViewStart.cshtml)]

Powyższe oznakowanie ustawia plik układu na *Pages/Shared/_Layout. cshtml* dla wszystkich plików Razor w folderze *Pages* . Aby uzyskać więcej informacji, zobacz [Układ](xref:razor-pages/index#layout) .

### <a name="the-create-page-model"></a>Model tworzenia strony

Obejrzyj model stron */filmów/Utwórz. cshtml. cs* :

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

Metoda `OnGet` inicjuje wszystkie Stany potrzebne dla strony. Strona tworzenia nie ma żadnego stanu do zainicjowania, więc zwracana jest wartość `Page`. W dalszej części tego samouczka zostanie wyświetlony przykład `OnGet` inicjowanie stanu. Metoda `Page` tworzy obiekt `PageResult`, który renderuje stronę *Create. cshtml* .

Właściwość `Movie` używa atrybutu `[BindProperty]` do przystąpienia do [powiązania modelu](xref:mvc/models/model-binding). Gdy formularz tworzenia zapisuje wartości formularza, środowisko uruchomieniowe ASP.NET Core tworzy powiązanie wartości ogłoszonych z modelem `Movie`.

Metoda `OnPostAsync` jest uruchamiana, gdy strona zawiera dane formularza:

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Jeśli występują jakieś błędy modelu, formularz jest ponownie wyświetlany wraz z wszelkimi opublikowanymi danymi formularza. Większość błędów modelu można przechwycić po stronie klienta przed opublikowaniem formularza. Przykładem błędu modelu jest opublikowanie wartości pola daty, którego nie można przekonwertować na datę. Weryfikowanie po stronie klienta i sprawdzanie poprawności modelu zostały omówione w dalszej części tego samouczka.

Jeśli nie ma żadnych błędów modelu, dane zostaną zapisane i przeglądarka zostanie przekierowana na stronę indeksu.

### <a name="the-create-razor-page"></a>Strona tworzenie Razor

Przejrzyj strony */filmy/Utwórz* plik stronicowania Razor. cshtml:

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Program Visual Studio wyświetla następujące znaczniki w wyróżnionej pogrubionej czcionce używanej przez pomocników tagów:

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

![Widok VS17 na stronie Tworzenie. cshtml](page/_static/th3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Następujące pomocnicy tagów są pokazane w powyższym znaczniku:

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

Program Visual Studio wyświetla następujące znaczniki w wyróżnionej pogrubionej czcionce używanej przez pomocników tagów:

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

---

Element `<form method="post">` jest [pomocnikiem tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper). Pomocnik tagu formularza automatycznie zawiera [token antysfałszowany](xref:security/anti-request-forgery).

Aparat szkieletu tworzy znaczniki Razor dla każdego pola w modelu (z wyjątkiem identyfikatora) podobne do następujących:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml?range=15-20)]

[Pomocnik tagów walidacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` i `<span asp-validation-for`) wyświetla błędy walidacji. Sprawdzanie poprawności jest omówione bardziej szczegółowo w dalszej części tej serii.

[Pomocnik tagu etykiety](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generuje podpis etykiety i atrybut `for` dla właściwości `Title`.

[Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) używa atrybutów [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) i tworzy atrybuty HTML, które są odpowiednie dla walidacji jQuery po stronie klienta.

Aby uzyskać więcej informacji na temat pomocników tagów, takich jak `<form method="post">`, zobacz [pomocników tagów w ASP.NET Core](xref:mvc/views/tag-helpers/intro).

## <a name="additional-resources"></a>Dodatkowe zasoby

> [!div class="step-by-step"]
> [Poprzedni: Dodawanie modelu](xref:tutorials/razor-pages/model)
> [Next: Database](xref:tutorials/razor-pages/sql)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten samouczek służy do badania Razor Pages utworzonych przez tworzenie szkieletów w [poprzednim samouczku](xref:tutorials/razor-pages/model).

[Wyświetl lub Pobierz](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) przykład.

## <a name="the-create-delete-details-and-edit-pages"></a>Strony tworzenie, usuwanie, szczegóły i edycja

Zapoznaj się z modelem stron */filmów/index. cshtml. cs* :

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

Razor Pages pochodzą od `PageModel`. Zgodnie z Konwencją Klasa pochodna @no__t -0 jest nazywana `<PageName>Model`. Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) , aby dodać `RazorPagesMovieContext` do strony. Wszystkie strony szkieletowe są zgodne z tym wzorcem. Zobacz [kod asynchroniczny](xref:data/ef-rp/intro#asynchronous-code) , aby uzyskać więcej informacji na temat programowania asynchronicznego przy użyciu Entity Framework.

Gdy żądanie jest wykonywane dla strony, Metoda `OnGetAsync` zwraca listę filmów do strony Razor. `OnGetAsync` lub `OnGet` jest wywoływana na stronie Razor, aby zainicjować stan dla strony. W takim przypadku `OnGetAsync` pobiera listę filmów i wyświetla je.

Gdy `OnGet` zwraca `void` lub `OnGetAsync` zwraca wartość @ no__t-3, nie jest używana żadna metoda Return. Gdy typem zwracanym jest `IActionResult` lub `Task<IActionResult>`, należy podać instrukcję return. Na przykład: *Pages/Films/Create. cshtml. cs* `OnPostAsync`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a>Przejrzyj stronę */filmy/index. cshtml* Razor:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor można przenieść z HTML do C# lub do znacznika specyficznego dla Razor. Gdy po symbolu `@` następuje [słowo kluczowe zarezerwowane Razor](xref:mvc/views/razor#razor-reserved-keywords), przechodzi do znacznika specyficznego dla Razor, w przeciwnym razie przechodzi do C#.

Dyrektywa Razor `@page` tworzy plik w akcji MVC, co oznacza, że może obsługiwać żądania. `@page` musi być pierwszą dyrektywą Razor na stronie. `@page` to przykład przejścia do znacznika specyficznego dla Razor. Aby uzyskać więcej informacji, zobacz [składnia Razor](xref:mvc/views/razor#razor-syntax) .

Bada wyrażenie lambda użyte w następującym Pomocniku HTML:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

Pomocnik HTML `DisplayNameFor` sprawdza Właściwość `Title`, do której odwołuje się wyrażenie lambda w celu określenia nazwy wyświetlanej. Wyrażenie lambda jest sprawdzane, a nie oceniane. Oznacza to, że nie ma żadnych naruszeń dostępu, gdy `model`, `model.Movie` lub `model.Movie[0]` są `null` lub puste. Gdy wyrażenie lambda jest oceniane (na przykład z `@Html.DisplayFor(modelItem => item.Title)`), wartości właściwości modelu są oceniane.

<a name="md"></a>

### <a name="the-model-directive"></a>Dyrektywa @model

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

Dyrektywa `@model` określa typ modelu przekazaną do strony Razor. W poprzednim przykładzie wiersz `@model` powoduje, że @no__t Klasa pochodna -1 jest dostępna dla strony Razor. Model jest używany w [pomocnikach HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` i `@Html.DisplayFor` na stronie.

### <a name="the-layout-page"></a>Strona układu

Wybierz linki menu (**RazorPagesMovie**, **Home**i **privacy**). Każda Strona wyświetla ten sam układ menu. Układ menu jest implementowany w pliku *Pages/Shared/_Layout. cshtml* . Otwórz plik *Pages/Shared/_Layout. cshtml* .

Szablony [układów](xref:mvc/views/layout) umożliwiają określenie układu kontenera HTML witryny w jednym miejscu, a następnie zastosowanie go na wielu stronach w witrynie. Znajdź wiersz `@RenderBody()`. `RenderBody` jest symbolem zastępczym, w którym wszystkie utworzone widoki związane ze stroną są *wyświetlane na stronie* układ. Na przykład po wybraniu linku **prywatności** widok **strony/prywatność. cshtml** jest renderowany wewnątrz metody `RenderBody`.

<a name="vd"></a>

### <a name="viewdata-and-layout"></a>ViewData i układ

Rozważmy następujący kod z pliku *Pages/Films/index. cshtml* :

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

Poprzedni wyróżniony kod jest przykładem przejścia Razor do C#. Znaki `{` i `}` obejmują blok C# kodu.

Klasa bazowa `PageModel` ma właściwość słownika `ViewData`, która może służyć do dodawania danych, które mają zostać przekazane do widoku. Obiekty są dodawane do słownika `ViewData` przy użyciu wzorca klucz/wartość. W poprzednim przykładzie właściwość "title" została dodana do słownika `ViewData`.

Właściwość "title" jest używana w pliku *Pages/Shared/_Layout. cshtml* . Poniższy znacznik pokazuje pierwsze kilka wierszy pliku *_Layout. cshtml* .

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

Wiersz `@*Markup removed for brevity.*@` jest komentarzem Razor, które nie pojawia się w pliku układu. W przeciwieństwie do komentarzy HTML (`<!-- -->`), komentarze Razor nie są wysyłane do klienta.

### <a name="update-the-layout"></a>Aktualizowanie układu

Zmień element `<title>` w pliku *Pages/Shared/_Layout. cshtml* , aby wyświetlał **film** , a nie **RazorPagesMovie**.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

Znajdź następujący element zakotwiczony w pliku *Pages/Shared/_Layout. cshtml* .

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

Zastąp poprzedni element następującym znacznikiem.

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

Poprzedni element zakotwiczenia jest [pomocnikiem tagów](xref:mvc/views/tag-helpers/intro). W tym przypadku jest to [pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). Atrybut i wartość pomocnika tagu `asp-page="/Movies/Index"` tworzy łącze do strony Razor `/Movies/Index`. Wartość atrybutu `asp-area` jest pusta, dlatego obszar nie jest używany w łączu. Aby uzyskać więcej informacji, zobacz [obszary](xref:mvc/controllers/areas) .

Zapisz zmiany i przetestuj aplikację, klikając łącze **RpMovie** . Jeśli występują problemy, zobacz plik [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) w usłudze GitHub.

Przetestuj inne linki (**Narzędzia główne**, **RpMovie**, **Utwórz**, **Edytuj**i **Usuń**). Każda Strona ustawia tytuł, który można zobaczyć na karcie przeglądarki. Po utworzeniu zakładki na stronie tytuł jest używany dla zakładki.

> [!NOTE]
> W polu `Price` nie można wprowadzać przecinków dziesiętnych. Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i formatów dat innych niż angielski, należy wykonać kroki w celu globalizacji aplikacji. Ten [problem w usłudze GitHub 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) zawiera instrukcje dotyczące dodawania przecinków dziesiętnych.

Właściwość `Layout` jest ustawiona w pliku *Pages/_ViewStart. cshtml* :

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

Powyższe oznakowanie ustawia plik układu na *Pages/Shared/_Layout. cshtml* dla wszystkich plików Razor w folderze *Pages* . Aby uzyskać więcej informacji, zobacz [Układ](xref:razor-pages/index#layout) .

### <a name="the-create-page-model"></a>Model tworzenia strony

Obejrzyj model stron */filmów/Utwórz. cshtml. cs* :

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

Metoda `OnGet` inicjuje wszystkie Stany potrzebne dla strony. Strona tworzenia nie ma żadnego stanu do zainicjowania, więc zwracana jest wartość `Page`. W dalszej części tego samouczka zobaczysz stan zainicjowania metody `OnGet`. Metoda `Page` tworzy obiekt `PageResult`, który renderuje stronę *Create. cshtml* .

Właściwość `Movie` używa atrybutu `[BindProperty]` do przystąpienia do [powiązania modelu](xref:mvc/models/model-binding). Gdy formularz tworzenia zapisuje wartości formularza, środowisko uruchomieniowe ASP.NET Core tworzy powiązanie wartości ogłoszonych z modelem `Movie`.

Metoda `OnPostAsync` jest uruchamiana, gdy strona zawiera dane formularza:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Jeśli występują jakieś błędy modelu, formularz jest ponownie wyświetlany wraz z wszelkimi opublikowanymi danymi formularza. Większość błędów modelu można przechwycić po stronie klienta przed opublikowaniem formularza. Przykładem błędu modelu jest opublikowanie wartości pola daty, którego nie można przekonwertować na datę. Weryfikowanie po stronie klienta i sprawdzanie poprawności modelu zostały omówione w dalszej części tego samouczka.

Jeśli nie ma żadnych błędów modelu, dane zostaną zapisane i przeglądarka zostanie przekierowana na stronę indeksu.

### <a name="the-create-razor-page"></a>Strona tworzenie Razor

Przejrzyj strony */filmy/Utwórz* plik stronicowania Razor. cshtml:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Program Visual Studio Wyświetla tag `<form method="post">` w wyróżnionej pogrubionej czcionce używanej przez pomocników tagów:

![Widok VS17 na stronie Tworzenie. cshtml](page/_static/th.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Aby uzyskać więcej informacji na temat pomocników tagów, takich jak `<form method="post">`, zobacz [pomocników tagów w ASP.NET Core](xref:mvc/views/tag-helpers/intro).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

Visual Studio dla komputerów Mac wyświetla znacznik `<form method="post">` w wyróżnionej pogrubionej czcionce używanej przez pomocników tagów.

---

Element `<form method="post">` jest [pomocnikiem tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper). Pomocnik tagu formularza automatycznie zawiera [token antysfałszowany](xref:security/anti-request-forgery).

Aparat szkieletu tworzy znaczniki Razor dla każdego pola w modelu (z wyjątkiem identyfikatora) podobne do następujących:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

[Pomocnik tagów walidacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` i `<span asp-validation-for`) wyświetla błędy walidacji. Sprawdzanie poprawności jest omówione bardziej szczegółowo w dalszej części tej serii.

[Pomocnik tagu etykiety](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generuje podpis etykiety i atrybut `for` dla właściwości `Title`.

[Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) używa atrybutów [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) i tworzy atrybuty HTML, które są odpowiednie dla walidacji jQuery po stronie klienta.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja tego samouczka usługi YouTube](https://youtu.be/zxgKjPYnOMM)

> [!div class="step-by-step"]
> [Poprzedni: Dodawanie modelu](xref:tutorials/razor-pages/model)
> [Next: Database](xref:tutorials/razor-pages/sql)

::: moniker-end
