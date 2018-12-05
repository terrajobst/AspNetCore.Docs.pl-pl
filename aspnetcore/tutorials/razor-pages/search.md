---
title: Dodawanie wyszukiwania do stron Razor programu ASP.NET Core
author: rick-anderson
description: Pokazuje, jak dodać wyszukiwanie do stron Razor programu ASP.NET Core
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.date: 12/3/2018
uid: tutorials/razor-pages/search
ms.openlocfilehash: 0afd22be397f3f887fb6ce9718e01dc625548e71
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861956"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a>Dodawanie wyszukiwania do stron Razor programu ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/rp/download.md)]

W poniższych sekcjach wyszukiwanie filmów przez *gatunku* lub *nazwa* zostanie dodany.

Dodaj następujące właściwości wyróżniony, aby *Pages/Movies/Index.cshtml.cs*:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* `SearchString`: zawiera tekst, użytkownicy wprowadzają w polu tekstowym wyszukiwania. `SearchString` zostanie nadany [ `[BindProperty]` ](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) atrybutu. `[BindProperty]` wiąże wartości formularza i ciągi zapytania z taką samą nazwę jak właściwość. `(SupportsGet = true)` jest wymagany dla wiązania w odpowiedzi na żądania GET.
* `Genres`: zawiera listę gatunki. `Genres` Zezwala użytkownikowi na wybranie określonego rodzaju z listy. `SelectList` wymaga `using Microsoft.AspNetCore.Mvc.Rendering;`
* `MovieGenre`: zawiera gatunku określonych przez użytkownika (na przykład "zachodni").

`Genres` i `MovieGenre` właściwości są używane w dalszej części tego samouczka.

Aktualizuj stronę indeksu `OnGetAsync` metoda następującym kodem:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

W pierwszym wierszu `OnGetAsync` metoda tworzy [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) zapytanie, aby wybrać filmów:

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

Zapytanie jest *tylko* zdefiniowane w tym momencie, ma ona **nie** zostały uruchomione dla bazy danych.

Jeśli `SearchString` właściwość nie jest zerowa lub pusta, zapytanie filmy zostanie zmodyfikowany na potrzeby filtrowania ciąg wyszukiwania:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

`s => s.Title.Contains()` Kod jest [wyrażenia Lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Wyrażenia lambda są używane w oparte na metodzie [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) jako argumenty do standardowych metod operatorów kwerendy takich zapytań jak [gdzie](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) metody lub `Contains` (używane w poprzednim kodzie). Zapytania LINQ nie są wykonywane, gdy są one definiowane lub ich modyfikacji przez wywołanie metody (takie jak `Where`, `Contains` lub `OrderBy`). Przeciwnie wykonanie zapytania jest odroczone. Oznacza to, że obliczania wyrażenia została opóźniona do momentu jego rzeczywista wartość jest powtarzana lub `ToListAsync` metoda jest wywoływana. Zobacz [wykonywania zapytania](/dotnet/framework/data/adonet/ef/language-reference/query-execution) Aby uzyskać więcej informacji.

**Uwaga:** [zawiera](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metoda jest uruchomiona w bazie danych, nie w kodzie języka C#. Rozróżnianie wielkości liter w zapytaniu, zależy od bazy danych i sortowanie. W programie SQL Server `Contains` mapuje [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), która jest uwzględniana wielkość liter. W SQLite przy użyciu sortowania domyślnego, jest uwzględniana wielkość liter.

Przejdź do strony filmy i Dołącz ciąg zapytania, takie jak `?searchString=Ghost` do adresu URL (na przykład `https://localhost:5001/Movies?searchString=Ghost`). Wyświetlane są filtrowane filmów.

![Widok indeksu](search/_static/ghost.png)

Jeśli następujący szablon trasy zostanie dodany do strony indeksu, ciąg wyszukiwania mogą być przekazywane jako segment adresu URL (na przykład `https://localhost:5001/Movies/Ghost`).

```cshtml
@page "{searchString?}"
```

Poprzedni ograniczenia trasy umożliwia wyszukiwanie tytuł, np. dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.  `?` w `"{searchString?}"` oznacza, że jest to parametr opcjonalny trasy.

![Widok indeksu z ghost word dodawany do adresu Url i zwrócony filmu listę filmów Ghostbusters i Ghostbusters 2](search/_static/g2.png)

Środowisko uruchomieniowe programu ASP.NET Core używa [wiązanie modelu](xref:mvc/models/model-binding) można ustawić wartość `SearchString` właściwości z ciągu zapytania (`?searchString=Ghost`) lub kierować dane (`https://localhost:5001/Movies/Ghost`). Powiązanie modelu nie jest uwzględniana wielkość liter.

Jednak nie można oczekiwać od użytkowników, aby zmodyfikować adres URL, aby wyszukać film. W tym kroku interfejs użytkownika jest dodawany do filtrowania filmów. Jeśli dodano ograniczenia trasy `"{searchString?}"`, usuń go.

Otwórz *Pages/Movies/Index.cshtml* pliku i Dodaj `<form>` znaczników wyróżnione w poniższym kodzie:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

Kod HTML `<form>` tag korzysta z następujących [pomocników tagów](xref:mvc/views/tag-helpers/intro):

* [Pomocnik tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper). Po przesłaniu formularza ciąg filtru są wysyłane do *stron/filmy/Index* strony za pomocą ciągu zapytania.
* [Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms#the-input-tag-helper)

Zapisz zmiany i przetestować filtr.

![Widok indeksu z ghost słowa wpisane w polu tekstowym filtru tytułu](search/_static/filter.png)

## <a name="search-by-genre"></a>Wyszukaj według gatunku

Aktualizacja `OnGetAsync` metoda następującym kodem:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

Poniższy kod jest zapytanie LINQ, która pobiera wszystkie gatunki z bazy danych.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

`SelectList` Gatunków jest tworzona przy wyświetlaniu gatunki distinct.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a>Dodaj wyszukiwanie według gatunku do strony Razor

Aktualizacja *Index.cshtml* w następujący sposób:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

Testowanie aplikacji przez wyszukiwanie według gatunku, tytuł filmu i obu.

> [!div class="step-by-step"]
> [Poprzedni: Aktualizowanie stron](xref:tutorials/razor-pages/da1)
> [dalej: dodanie nowego pola](xref:tutorials/razor-pages/new-field)
