---
ms.openlocfilehash: 1c693712cc39c0e01ad19409bd10bc5e24737b3a
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208494"
---
# <a name="add-search-to-an-aspnet-core-mvc-app"></a>Dodawanie wyszukiwania do aplikacji ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji możesz dodać możliwości wyszukiwania do `Index` metody akcji, która umożliwia wyszukiwanie filmów przez *gatunku* lub *nazwa*.

Aktualizacja `Index` metoda następującym kodem:
<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

W pierwszym wierszu `Index` tworzy metody akcji [LINQ](/dotnet/standard/using-linq) zapytanie, aby wybrać filmów:

```csharp
var movies = from m in _context.Movie
             select m;
```

Zapytanie jest *tylko* zdefiniowane w tym momencie, ma ona **nie** zostały uruchomione dla bazy danych.

Jeśli `searchString` parametru zawiera ciąg, zapytanie filmy zostanie zmodyfikowany na potrzeby filtrowania na wartość ciągu wyszukiwania:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

`s => s.Title.Contains()` Powyższy kod jest [wyrażenia Lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Wyrażenia lambda są używane w oparte na metodzie [LINQ](/dotnet/standard/using-linq) jako argumenty do standardowych metod operatorów kwerendy takich zapytań jak [gdzie](/dotnet/api/system.linq.enumerable.where) metody lub `Contains` (używane w powyższym kodzie). Zapytania LINQ nie są wykonywane, gdy są one definiowane lub ich modyfikacji przez wywołanie metody, takie jak `Where`, `Contains` lub `OrderBy`. Przeciwnie wykonanie zapytania jest odroczone.  Oznacza to, że wyniku obliczenia wyrażenia zostanie opóźnione, dopóki wartość zrealizowane faktycznie jest powtarzana lub `ToListAsync` metoda jest wywoływana. Aby uzyskać więcej informacji na temat wykonywania zapytań z opóźnieniem, zobacz [wykonywania zapytania](/dotnet/framework/data/adonet/ef/language-reference/query-execution).

Uwaga: [Zawiera](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metoda jest uruchomiona w bazie danych nie jest w kodzie języka c# pokazano powyżej. Rozróżnianie wielkości liter w zapytaniu, zależy od bazy danych i sortowanie. W programie SQL Server [zawiera](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) mapuje [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), która jest uwzględniana wielkość liter. W SQLite przy użyciu sortowania domyślnego, jest uwzględniana wielkość liter.

Przejdź do adresu `/Movies/Index`. Dołącz ciąg zapytania, takie jak `?searchString=Ghost` do adresu URL. Wyświetlane są filtrowane filmów.

![Widok indeksu](~/tutorials/first-mvc-app/search/_static/ghost.png)

Jeśli zmienisz podpis `Index` metoda będzie miała parametru o nazwie `id`, `id` parametr będzie odpowiadał opcjonalnego `{id}` symbolu zastępczego dla domyślnej kieruje zestawu w *Startup.cs*.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]
