---
title: Dodaj wyszukiwanie do ASP.NET Core Razor Pages
author: rick-anderson
description: Pokazuje, jak dodać wyszukiwanie do ASP.NET Core Razor Pages
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/search
ms.openlocfilehash: 8228207b0f37a6923b29891ac3115dd0be115501
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667707"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a>Dodaj wyszukiwanie do ASP.NET Core Razor Pages

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

W poniższych sekcjach są dodawane przeszukiwania filmów według *gatunku* lub *nazwy* .

Dodaj następujące wyróżnione właściwości do *stron/filmów/index. cshtml. cs*:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* `SearchString`: zawiera tekst wprowadzany przez użytkowników w polu tekstowym Wyszukaj. `SearchString` ma atrybut [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) . `[BindProperty]` wiąże wartości formularzy i ciągi zapytania o tej samej nazwie, co właściwość. `(SupportsGet = true)` jest wymagana do powiązania w żądaniach GET.
* `Genres`: zawiera listę gatunku. `Genres` umożliwia użytkownikowi wybranie gatunku z listy. `SelectList` wymaga `using Microsoft.AspNetCore.Mvc.Rendering;`
* `MovieGenre`: zawiera konkretny gatunek wybierany przez użytkownika (na przykład "zachodni").
* `Genres` i `MovieGenre` są używane w dalszej części tego samouczka.

[!INCLUDE[](~/includes/bind-get.md)]

Zaktualizuj metodę `OnGetAsync` strony indeksu przy użyciu następującego kodu:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

Pierwszy wiersz metody `OnGetAsync` tworzy zapytanie [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) do wybierania filmów:

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

Zapytanie jest zdefiniowane *tylko* w tym momencie, **nie** zostało uruchomione względem bazy danych.

Jeśli właściwość `SearchString` nie ma wartości null lub pustej, kwerenda filmów zostanie zmodyfikowana w celu odfiltrowania ciągu wyszukiwania:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

Kod `s => s.Title.Contains()` jest [wyrażeniem lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Wyrażenia lambda są używane w zapytaniach [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) opartych na metodach jako argumenty dla standardowych metod operatora zapytań, takich jak Metoda [WHERE](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) lub `Contains` (używana w poprzednim kodzie). Zapytania LINQ nie są wykonywane, jeśli są zdefiniowane lub są modyfikowane przez wywołanie metody (takiej jak `Where`, `Contains` lub `OrderBy`). Zamiast tego wykonywanie zapytania jest odroczone. Oznacza to, że Obliczanie wyrażenia jest opóźnione do momentu przekroczenia jego zrealizowanej wartości lub wywołania metody `ToListAsync`. Aby uzyskać więcej informacji, zobacz [wykonywanie zapytań](/dotnet/framework/data/adonet/ef/language-reference/query-execution) .

> [!NOTE]
> Metoda [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) jest uruchamiana w bazie danych, a C# nie w kodzie. Uwzględnianie wielkości liter w zapytaniu zależy od bazy danych i sortowania. W SQL Server, `Contains` mapuje do [bazy danych SQL, np](/sql/t-sql/language-elements/like-transact-sql). bez uwzględniania wielkości liter. W ramach programu SQLite domyślne sortowanie uwzględnia wielkość liter.

Przejdź do strony filmy i dołącz ciąg zapytania, taki jak `?searchString=Ghost` do adresu URL (na przykład `https://localhost:5001/Movies?searchString=Ghost`). Wyświetlane są filtrowane filmy.

![Widok indeksu](search/_static/ghost.png)

Jeśli do strony indeksu zostanie dodany następujący szablon trasy, ciąg wyszukiwania może zostać przekierowany jako segment adresu URL (na przykład `https://localhost:5001/Movies/Ghost`).

```cshtml
@page "{searchString?}"
```

Powyższe ograniczenie trasy umożliwia przeszukiwanie tytułu jako dane trasy (segment adresu URL), a nie jako wartość ciągu zapytania.  `?` w `"{searchString?}"` oznacza, że jest to opcjonalny parametr trasy.

![Widok indeksu z wyrazem Ghost dodany do adresu URL i zwrotną listą filmów dwóch filmów, Ghostbusters i Ghostbusters 2](search/_static/g2.png)

Środowisko uruchomieniowe ASP.NET Core używa [powiązania modelu](xref:mvc/models/model-binding) , aby ustawić wartość właściwości `SearchString` na podstawie ciągu zapytania (`?searchString=Ghost`) lub danych tras (`https://localhost:5001/Movies/Ghost`). W powiązaniu modelu nie jest rozróżniana wielkość liter.

Nie można jednak oczekiwać, że użytkownicy modyfikują adres URL w celu wyszukania filmu. W tym kroku zostanie dodany interfejs użytkownika do filtrowania filmów. Jeśli dodano `"{searchString?}"`ograniczenia trasy, usuń je.

Otwórz plik *Pages/Films/index. cshtml* i Dodaj `<form>` znaczników wyróżnionych w poniższym kodzie:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/Index2.cshtml?highlight=14-19&range=1-22)]

Tag `<form>` HTML używa następujących [pomocników tagów](xref:mvc/views/tag-helpers/intro):

* [Pomocnik tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper). Gdy formularz zostanie przesłany, ciąg filtru jest wysyłany do *stron/filmów/indeksu* za pośrednictwem ciągu zapytania.
* [Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms#the-input-tag-helper)

Zapisz zmiany i przetestuj filtr.

![Widok indeksu z słowem Ghost wpisanych do pola tekstowego filtru tytułu](search/_static/filter.png)

## <a name="search-by-genre"></a>Wyszukaj według gatunku

Zaktualizuj metodę `OnGetAsync` przy użyciu następującego kodu:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

Poniższy kod jest zapytanie LINQ, które pobiera wszystkie gatunki z bazy danych.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

`SelectList` gatuneków jest tworzony przez projekcję odrębnych gatuneków.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a>Dodaj wyszukiwanie według gatunku na stronie Razor

Zaktualizuj *indeks. cshtml* w następujący sposób:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

Przetestuj aplikację, wyszukując według gatunku, tytułu filmu i obu tych elementów.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja tego samouczka usługi YouTube](https://youtu.be/4B6pHtdyo08)

> [!div class="step-by-step"]
> [Poprzedni: aktualizowanie stron](xref:tutorials/razor-pages/da1)
> [Dalej: Dodawanie nowego pola](xref:tutorials/razor-pages/new-field)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

W poniższych sekcjach są dodawane przeszukiwania filmów według *gatunku* lub *nazwy* .

Dodaj następujące wyróżnione właściwości do *stron/filmów/index. cshtml. cs*:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* `SearchString`: zawiera tekst wprowadzany przez użytkowników w polu tekstowym Wyszukaj. `SearchString` ma atrybut [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) . `[BindProperty]` wiąże wartości formularzy i ciągi zapytania o tej samej nazwie, co właściwość. `(SupportsGet = true)` jest wymagana do powiązania w żądaniach GET.
* `Genres`: zawiera listę gatunku. `Genres` umożliwia użytkownikowi wybranie gatunku z listy. `SelectList` wymaga `using Microsoft.AspNetCore.Mvc.Rendering;`
* `MovieGenre`: zawiera konkretny gatunek wybierany przez użytkownika (na przykład "zachodni").
* `Genres` i `MovieGenre` są używane w dalszej części tego samouczka.

[!INCLUDE[](~/includes/bind-get.md)]

Zaktualizuj metodę `OnGetAsync` strony indeksu przy użyciu następującego kodu:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

Pierwszy wiersz metody `OnGetAsync` tworzy zapytanie [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) do wybierania filmów:

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

Zapytanie jest zdefiniowane *tylko* w tym momencie, **nie** zostało uruchomione względem bazy danych.

Jeśli właściwość `SearchString` nie ma wartości null lub pustej, kwerenda filmów zostanie zmodyfikowana w celu odfiltrowania ciągu wyszukiwania:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

Kod `s => s.Title.Contains()` jest [wyrażeniem lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Wyrażenia lambda są używane w zapytaniach [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) opartych na metodach jako argumenty dla standardowych metod operatora zapytań, takich jak Metoda [WHERE](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) lub `Contains` (używana w poprzednim kodzie). Zapytania LINQ nie są wykonywane, jeśli są zdefiniowane lub są modyfikowane przez wywołanie metody (takiej jak `Where`, `Contains` lub `OrderBy`). Zamiast tego wykonywanie zapytania jest odroczone. Oznacza to, że Obliczanie wyrażenia jest opóźnione do momentu przekroczenia jego zrealizowanej wartości lub wywołania metody `ToListAsync`. Aby uzyskać więcej informacji, zobacz [wykonywanie zapytań](/dotnet/framework/data/adonet/ef/language-reference/query-execution) .

**Uwaga:** Metoda [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) jest uruchamiana w bazie danych, a C# nie w kodzie. Uwzględnianie wielkości liter w zapytaniu zależy od bazy danych i sortowania. W SQL Server, `Contains` mapuje do [bazy danych SQL, np](/sql/t-sql/language-elements/like-transact-sql). bez uwzględniania wielkości liter. W ramach programu SQLite domyślne sortowanie uwzględnia wielkość liter.

Przejdź do strony filmy i dołącz ciąg zapytania, taki jak `?searchString=Ghost` do adresu URL (na przykład `https://localhost:5001/Movies?searchString=Ghost`). Wyświetlane są filtrowane filmy.

![Widok indeksu](search/_static/ghost.png)

Jeśli do strony indeksu zostanie dodany następujący szablon trasy, ciąg wyszukiwania może zostać przekierowany jako segment adresu URL (na przykład `https://localhost:5001/Movies/Ghost`).

```cshtml
@page "{searchString?}"
```

Powyższe ograniczenie trasy umożliwia przeszukiwanie tytułu jako dane trasy (segment adresu URL), a nie jako wartość ciągu zapytania.  `?` w `"{searchString?}"` oznacza, że jest to opcjonalny parametr trasy.

![Widok indeksu z wyrazem Ghost dodany do adresu URL i zwrotną listą filmów dwóch filmów, Ghostbusters i Ghostbusters 2](search/_static/g2.png)

Środowisko uruchomieniowe ASP.NET Core używa [powiązania modelu](xref:mvc/models/model-binding) , aby ustawić wartość właściwości `SearchString` na podstawie ciągu zapytania (`?searchString=Ghost`) lub danych tras (`https://localhost:5001/Movies/Ghost`). W powiązaniu modelu nie jest rozróżniana wielkość liter.

Nie można jednak oczekiwać, że użytkownicy modyfikują adres URL w celu wyszukania filmu. W tym kroku zostanie dodany interfejs użytkownika do filtrowania filmów. Jeśli dodano `"{searchString?}"`ograniczenia trasy, usuń je.

Otwórz plik *Pages/Films/index. cshtml* i Dodaj `<form>` znaczników wyróżnionych w poniższym kodzie:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

Tag `<form>` HTML używa następujących [pomocników tagów](xref:mvc/views/tag-helpers/intro):

* [Pomocnik tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper). Gdy formularz zostanie przesłany, ciąg filtru jest wysyłany do *stron/filmów/indeksu* za pośrednictwem ciągu zapytania.
* [Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms#the-input-tag-helper)

Zapisz zmiany i przetestuj filtr.

![Widok indeksu z słowem Ghost wpisanych do pola tekstowego filtru tytułu](search/_static/filter.png)

## <a name="search-by-genre"></a>Wyszukaj według gatunku

Zaktualizuj metodę `OnGetAsync` przy użyciu następującego kodu:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

Poniższy kod jest zapytanie LINQ, które pobiera wszystkie gatunki z bazy danych.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

`SelectList` gatuneków jest tworzony przez projekcję odrębnych gatuneków.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a>Dodaj wyszukiwanie według gatunku na stronie Razor

Zaktualizuj *indeks. cshtml* w następujący sposób:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

Przetestuj aplikację, wyszukując według gatunku, tytułu filmu i obu tych elementów.
Powyższy kod używa pomocnika [SELECT tag](xref:mvc/views/working-with-forms#the-select-tag-helper) i znacznika opcji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja tego samouczka usługi YouTube](https://youtu.be/4B6pHtdyo08)

> [!div class="step-by-step"]
> [Poprzedni: aktualizowanie stron](xref:tutorials/razor-pages/da1)
> [Dalej: Dodawanie nowego pola](xref:tutorials/razor-pages/new-field)

::: moniker-end
