---
title: Dodawanie wyszukiwania do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Pokazuje, jak dodać wyszukiwanie do podstawowej aplikacji ASP.NET Core MVC
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: 41d7494b77edaddbf719cab087142f0132dd3ed6
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208385"
---
# <a name="add-search-to-an-aspnet-core-mvc-app"></a>Dodawanie wyszukiwania do aplikacji ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji możesz dodać możliwości wyszukiwania do `Index` metody akcji, która umożliwia wyszukiwanie filmów przez *gatunku* lub *nazwa*.

Aktualizacja `Index` metoda następującym kodem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

W pierwszym wierszu `Index` tworzy metody akcji [LINQ](/dotnet/standard/using-linq) zapytanie, aby wybrać filmów:

```csharp
var movies = from m in _context.Movie
             select m;
```

Zapytanie jest *tylko* zdefiniowane w tym momencie, ma ona **nie** zostały uruchomione dla bazy danych.

Jeśli `searchString` parametru zawiera ciąg, zapytanie filmy zostanie zmodyfikowany na potrzeby filtrowania na wartość ciągu wyszukiwania:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

`s => s.Title.Contains()` Powyższy kod jest [wyrażenia Lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Wyrażenia lambda są używane w oparte na metodzie [LINQ](/dotnet/standard/using-linq) jako argumenty do standardowych metod operatorów kwerendy takich zapytań jak [gdzie](/dotnet/api/system.linq.enumerable.where) metody lub `Contains` (używane w powyższym kodzie). Zapytania LINQ nie są wykonywane, gdy są one definiowane lub ich modyfikacji przez wywołanie metody, takie jak `Where`, `Contains`, lub `OrderBy`. Przeciwnie wykonanie zapytania jest odroczone.  Oznacza to, że wyniku obliczenia wyrażenia zostanie opóźnione, dopóki wartość zrealizowane faktycznie jest powtarzana lub `ToListAsync` metoda jest wywoływana. Aby uzyskać więcej informacji na temat wykonywania zapytań z opóźnieniem, zobacz [wykonywania zapytania](/dotnet/framework/data/adonet/ef/language-reference/query-execution).

Uwaga: [Zawiera](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metoda jest uruchomiona w bazie danych nie jest w kodzie języka C# pokazano powyżej. Rozróżnianie wielkości liter w zapytaniu, zależy od bazy danych i sortowanie. W programie SQL Server [zawiera](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) mapuje [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), która jest uwzględniana wielkość liter. W SQLite przy użyciu sortowania domyślnego, jest uwzględniana wielkość liter.

Przejdź do adresu `/Movies/Index`. Dołącz ciąg zapytania, takie jak `?searchString=Ghost` do adresu URL. Wyświetlane są filtrowane filmów.

![Widok indeksu](~/tutorials/first-mvc-app/search/_static/ghost.png)

Jeśli zmienisz podpis `Index` metoda będzie miała parametru o nazwie `id`, `id` parametr będzie odpowiadał opcjonalnego `{id}` symbolu zastępczego dla domyślnej kieruje zestawu w *Startup.cs*.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

Zmień parametr `id` i wszystkie wystąpienia `searchString` Zmień `id`.

Poprzedni `Index` metody:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_1stSearch)]

Zaktualizowany interfejs `Index` metody z `id` parametru:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_SearchID)]

Tytuł wyszukiwania można teraz przekazywać jako dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.

![Widok indeksu z ghost word dodawany do adresu Url i zwrócony filmu listę filmów Ghostbusters i Ghostbusters 2](~/tutorials/first-mvc-app/search/_static/g2.png)

Jednak nie można oczekiwać od użytkowników, aby zmodyfikować adres URL, za każdym razem, gdy chcą wyszukiwania filmów. Teraz należy dodać elementy interfejsu użytkownika, aby pomóc im filtrowanie filmów. Jeśli zmienisz podpis `Index` metodę, aby przetestować sposób przekazywania trasy powiązanym `ID` parametr, zmień je z powrotem, aby przyspieszyć parametr o nazwie `searchString`:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_1stSearch)]

Otwórz *Views/Movies/Index.cshtml* pliku i Dodaj `<form>` znaczników, które przedstawiono poniżej:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

Kod HTML `<form>` tagów używa [Pomocnik tagu formularza](xref:mvc/views/working-with-forms), więc gdy prześlesz formularz, aby opublikowaniu ciąg filtru `Index` akcji kontrolera filmów. Zapisz zmiany, a następnie przetestować filtr.

![Widok indeksu z ghost słowa wpisane w polu tekstowym filtru tytułu](~/tutorials/first-mvc-app/search/_static/filter.png)

Istnieje nie `[HttpPost]` przeciążenia `Index` metody można by oczekiwać. Nie są potrzebne, ponieważ metoda nie jest zmiana stanu aplikacji, po prostu filtrowania danych.

Można dodać następujące `[HttpPost] Index` metody.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

`notUsed` Parametr jest używany do tworzenia przeciążenie dla elementu `Index` metody. Omówimy, w dalszej części tego samouczka.

Jeśli dodasz tej metody, wywołujący akcji będzie odpowiadać `[HttpPost] Index` metody i `[HttpPost] Index` metoda może działać, jak pokazano na poniższej ilustracji.

![Okno przeglądarki z odpowiedzi aplikacji z indeksu HttpPost: Filtr ghost](~/tutorials/first-mvc-app/search/_static/fo.png)

Jednak nawet w przypadku dodania to `[HttpPost]` wersję `Index` metody, jest to ograniczenie, w jak to wszystko została zaimplementowana. Wyobraź sobie, że chcesz utworzyć zakładkę określonego wyszukiwania lub chcesz wysłać link do znajomych, mogą kliknąć umożliwiający zobaczenie tej samej listy filtrowane filmów. Należy zauważyć, że adres URL żądania HTTP POST jest taki sam jak adres URL dla żądania GET (localhost:xxxxx/filmy/indeksu) — Brak wyszukiwania informacji w adresie URL. Informacje o parametrach wyszukiwania są wysyłane do serwera jako [stanowią wartość pola](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data). Możesz sprawdzić, czy za pomocą narzędzia deweloperskie przeglądarki lub doskonałą [narzędzie Fiddler](http://www.telerik.com/fiddler). Na poniższym obrazie przedstawiono narzędzia deweloperskie przeglądarki Chrome:

![Karta Sieć narzędzi dla deweloperów w programie Microsoft Edge wyświetlanie treść żądania z wartością Ciągwyszukiwania ghost](~/tutorials/first-mvc-app/search/_static/f12_rb.png)

Możesz zobaczyć parametru wyszukiwania i [XSRF](xref:security/anti-request-forgery) tokenu w treści żądania. Należy pamiętać, zgodnie z opisem w poprzednim samouczku [Pomocnik tagu formularza](xref:mvc/views/working-with-forms) generuje [XSRF](xref:security/anti-request-forgery) token zabezpieczający przed sfałszowaniem. Dane, nie masz zostać zmodyfikowana, więc nie potrzebujemy przeprowadzić walidacji tokenu w metodzie kontrolera.

Ponieważ parametr wyszukiwania znajduje się w treści żądania, a nie jej adres URL, nie można przechwycić informacje wyszukiwania do zakładki lub udostępnienia innym osobom. Poprawka, określając żądania powinna przypominać `HTTP GET`:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGet.cshtml?highlight=12&range=1-23)]

Teraz gdy prześlesz wyszukiwania, adres URL zawiera ciąg zapytania wyszukiwania. Wyszukiwanie będzie także przejść do `HttpGet Index` metody akcji, nawet jeśli masz `HttpPost Index` metody.

![Okno przeglądarki, przedstawiające Ciągwyszukiwania = ghost w adresie Url i filmów, zwrócone, Ghostbusters i Ghostbusters 2, zawierają ghost programu word](~/tutorials/first-mvc-app/search/_static/search_get.png)

Następujący kod przedstawia zmiany `form` tag:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="add-search-by-genre"></a>Dodaj wyszukiwanie według gatunku

Dodaj następujący kod `MovieGenreViewModel` klasy *modeli* folderu:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

Model widoku gatunku filmu będzie zawierać:

* Lista filmów.
* A `SelectList` zawierającego listę gatunki. Dzięki temu użytkownikowi na wybranie określonego rodzaju z listy.
* `MovieGenre`, zawierającą wybrane gatunku.
* `SearchString`, który zawiera tekst, użytkownicy wprowadzają w polu tekstowym wyszukiwania.

Zastąp `Index` method in Class metoda `MoviesController.cs` następującym kodem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

Poniższy kod jest `LINQ` zapytania, który pobiera wszystkie gatunki z bazy danych.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_LINQ)]

`SelectList` Gatunków jest tworzona przy wyświetlaniu distinct gatunki (nie chcemy naszej listy wyboru mają zduplikowane gatunki).

Gdy użytkownik wyszukuje element, do wartości wyszukiwania są przechowywane w polu wyszukiwania.

## <a name="add-search-by-genre-to-the-index-view"></a>Dodaj wyszukiwanie według gatunku do widoku indeksu

Aktualizacja `Index.cshtml` w następujący sposób:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

Sprawdź wyrażenie lambda, używane w następujących pomocnika kodu HTML:

`@Html.DisplayNameFor(model => model.Movies[0].Title)`

W poprzednim kodzie `DisplayNameFor` sprawdza pomocnika kodu HTML `Title` właściwość, do którego odwołuje się wyrażenie lambda, aby ustalić nazwę wyświetlaną. Ponieważ wyrażenie lambda jest kontrolowane zamiast oceniane, nie otrzymasz naruszenie zasad dostępu podczas `model`, `model.Movies`, lub `model.Movies[0]` są `null` lub jest pusty. Kiedy jest obliczane wyrażenie lambda (na przykład `@Html.DisplayFor(modelItem => item.Title)`), są oceniane wartości właściwości modelu.

Testowanie aplikacji przez wyszukiwanie według gatunku, tytuł filmu i przez:

![Wyniki przedstawiający okno przeglądarki https://localhost:5001/Movies?MovieGenre=Comedy&SearchString=2](~/tutorials/first-mvc-app/search/_static/s2.png)

> [!div class="step-by-step"]
> [Poprzednie](controller-methods-views.md)
> [dalej](new-field.md)
