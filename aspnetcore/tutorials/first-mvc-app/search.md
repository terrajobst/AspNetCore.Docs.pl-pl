---
title: Dodawanie wyszukiwania do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Pokazuje, jak dodać wyszukiwanie do podstawowej aplikacji ASP.NET Core MVC
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: 41d7494b77edaddbf719cab087142f0132dd3ed6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087285"
---
# <a name="add-search-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="ffbb3-103">Dodawanie wyszukiwania do aplikacji ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="ffbb3-103">Add search to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="ffbb3-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ffbb3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ffbb3-105">W tej sekcji możesz dodać możliwości wyszukiwania do `Index` metody akcji, która umożliwia wyszukiwanie filmów przez *gatunku* lub *nazwa*.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-105">In this section, you add search capability to the `Index` action method that lets you search movies by *genre* or *name*.</span></span>

<span data-ttu-id="ffbb3-106">Aktualizacja `Index` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ffbb3-106">Update the `Index` method with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

<span data-ttu-id="ffbb3-107">W pierwszym wierszu `Index` tworzy metody akcji [LINQ](/dotnet/standard/using-linq) zapytanie, aby wybrać filmów:</span><span class="sxs-lookup"><span data-stu-id="ffbb3-107">The first line of the `Index` action method creates a [LINQ](/dotnet/standard/using-linq) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="ffbb3-108">Zapytanie jest *tylko* zdefiniowane w tym momencie, ma ona **nie** zostały uruchomione dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-108">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="ffbb3-109">Jeśli `searchString` parametru zawiera ciąg, zapytanie filmy zostanie zmodyfikowany na potrzeby filtrowania na wartość ciągu wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="ffbb3-109">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

<span data-ttu-id="ffbb3-110">`s => s.Title.Contains()` Powyższy kod jest [wyrażenia Lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="ffbb3-110">The `s => s.Title.Contains()` code above is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="ffbb3-111">Wyrażenia lambda są używane w oparte na metodzie [LINQ](/dotnet/standard/using-linq) jako argumenty do standardowych metod operatorów kwerendy takich zapytań jak [gdzie](/dotnet/api/system.linq.enumerable.where) metody lub `Contains` (używane w powyższym kodzie).</span><span class="sxs-lookup"><span data-stu-id="ffbb3-111">Lambdas are used in method-based [LINQ](/dotnet/standard/using-linq) queries as arguments to standard query operator methods such as the [Where](/dotnet/api/system.linq.enumerable.where) method or `Contains` (used in the code above).</span></span> <span data-ttu-id="ffbb3-112">Zapytania LINQ nie są wykonywane, gdy są one definiowane lub ich modyfikacji przez wywołanie metody, takie jak `Where`, `Contains`, lub `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-112">LINQ queries are not executed when they're defined or when they're modified by calling a method such as `Where`, `Contains`, or `OrderBy`.</span></span> <span data-ttu-id="ffbb3-113">Przeciwnie wykonanie zapytania jest odroczone.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-113">Rather, query execution is deferred.</span></span>  <span data-ttu-id="ffbb3-114">Oznacza to, że wyniku obliczenia wyrażenia zostanie opóźnione, dopóki wartość zrealizowane faktycznie jest powtarzana lub `ToListAsync` metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-114">That means that the evaluation of an expression is delayed until its realized value is actually iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="ffbb3-115">Aby uzyskać więcej informacji na temat wykonywania zapytań z opóźnieniem, zobacz [wykonywania zapytania](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="ffbb3-115">For more information about deferred query execution, see [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span></span>

<span data-ttu-id="ffbb3-116">Uwaga: [Zawiera](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metoda jest uruchomiona w bazie danych nie jest w kodzie języka c# pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-116">Note: The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the c# code shown above.</span></span> <span data-ttu-id="ffbb3-117">Rozróżnianie wielkości liter w zapytaniu, zależy od bazy danych i sortowanie.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-117">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="ffbb3-118">W programie SQL Server [zawiera](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) mapuje [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), która jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-118">On SQL Server, [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="ffbb3-119">W SQLite przy użyciu sortowania domyślnego, jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-119">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="ffbb3-120">Przejdź do adresu `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-120">Navigate to `/Movies/Index`.</span></span> <span data-ttu-id="ffbb3-121">Dołącz ciąg zapytania, takie jak `?searchString=Ghost` do adresu URL.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-121">Append a query string such as `?searchString=Ghost` to the URL.</span></span> <span data-ttu-id="ffbb3-122">Wyświetlane są filtrowane filmów.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-122">The filtered movies are displayed.</span></span>

![Widok indeksu](~/tutorials/first-mvc-app/search/_static/ghost.png)

<span data-ttu-id="ffbb3-124">Jeśli zmienisz podpis `Index` metoda będzie miała parametru o nazwie `id`, `id` parametr będzie odpowiadał opcjonalnego `{id}` symbolu zastępczego dla domyślnej kieruje zestawu w *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-124">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the optional `{id}` placeholder for the default routes set in *Startup.cs*.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="ffbb3-125">Zmień parametr `id` i wszystkie wystąpienia `searchString` Zmień `id`.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-125">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

<span data-ttu-id="ffbb3-126">Poprzedni `Index` metody:</span><span class="sxs-lookup"><span data-stu-id="ffbb3-126">The previous `Index` method:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_1stSearch)]

<span data-ttu-id="ffbb3-127">Zaktualizowany interfejs `Index` metody z `id` parametru:</span><span class="sxs-lookup"><span data-stu-id="ffbb3-127">The updated `Index` method with `id` parameter:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_SearchID)]

<span data-ttu-id="ffbb3-128">Tytuł wyszukiwania można teraz przekazywać jako dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-128">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![Widok indeksu z ghost word dodawany do adresu Url i zwrócony filmu listę filmów Ghostbusters i Ghostbusters 2](~/tutorials/first-mvc-app/search/_static/g2.png)

<span data-ttu-id="ffbb3-130">Jednak nie można oczekiwać od użytkowników, aby zmodyfikować adres URL, za każdym razem, gdy chcą wyszukiwania filmów.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-130">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="ffbb3-131">Teraz należy dodać elementy interfejsu użytkownika, aby pomóc im filtrowanie filmów.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-131">So now you'll add UI elements to help them filter movies.</span></span> <span data-ttu-id="ffbb3-132">Jeśli zmienisz podpis `Index` metodę, aby przetestować sposób przekazywania trasy powiązanym `ID` parametr, zmień je z powrotem, aby przyspieszyć parametr o nazwie `searchString`:</span><span class="sxs-lookup"><span data-stu-id="ffbb3-132">If you changed the signature of the `Index` method to test how to pass the route-bound `ID` parameter, change it back so that it takes a parameter named `searchString`:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,6,8&name=snippet_1stSearch)]

<span data-ttu-id="ffbb3-133">Otwórz *Views/Movies/Index.cshtml* pliku i Dodaj `<form>` znaczników, które przedstawiono poniżej:</span><span class="sxs-lookup"><span data-stu-id="ffbb3-133">Open the *Views/Movies/Index.cshtml* file, and add the `<form>` markup highlighted below:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

<span data-ttu-id="ffbb3-134">Kod HTML `<form>` tagów używa [Pomocnik tagu formularza](xref:mvc/views/working-with-forms), więc gdy prześlesz formularz, aby opublikowaniu ciąg filtru `Index` akcji kontrolera filmów.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-134">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms), so when you submit the form, the filter string is posted to the `Index` action of the movies controller.</span></span> <span data-ttu-id="ffbb3-135">Zapisz zmiany, a następnie przetestować filtr.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-135">Save your changes and then test the filter.</span></span>

![Widok indeksu z ghost słowa wpisane w polu tekstowym filtru tytułu](~/tutorials/first-mvc-app/search/_static/filter.png)

<span data-ttu-id="ffbb3-137">Istnieje nie `[HttpPost]` przeciążenia `Index` metody można by oczekiwać.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-137">There's no `[HttpPost]` overload of the `Index` method as you might expect.</span></span> <span data-ttu-id="ffbb3-138">Nie są potrzebne, ponieważ metoda nie jest zmiana stanu aplikacji, po prostu filtrowania danych.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-138">You don't need it, because the method isn't changing the state of the app, just filtering data.</span></span>

<span data-ttu-id="ffbb3-139">Można dodać następujące `[HttpPost] Index` metody.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-139">You could add the following `[HttpPost] Index` method.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

<span data-ttu-id="ffbb3-140">`notUsed` Parametr jest używany do tworzenia przeciążenie dla elementu `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-140">The `notUsed` parameter is used to create an overload for the `Index` method.</span></span> <span data-ttu-id="ffbb3-141">Omówimy, w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-141">We'll talk about that later in the tutorial.</span></span>

<span data-ttu-id="ffbb3-142">Jeśli dodasz tej metody, wywołujący akcji będzie odpowiadać `[HttpPost] Index` metody i `[HttpPost] Index` metoda może działać, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-142">If you add this method, the action invoker would match the `[HttpPost] Index` method, and the `[HttpPost] Index` method would run as shown in the image below.</span></span>

![Okno przeglądarki z odpowiedzi aplikacji z indeksu HttpPost: Filtr ghost](~/tutorials/first-mvc-app/search/_static/fo.png)

<span data-ttu-id="ffbb3-144">Jednak nawet w przypadku dodania to `[HttpPost]` wersję `Index` metody, jest to ograniczenie, w jak to wszystko została zaimplementowana.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-144">However, even if you add this `[HttpPost]` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="ffbb3-145">Wyobraź sobie, że chcesz utworzyć zakładkę określonego wyszukiwania lub chcesz wysłać link do znajomych, mogą kliknąć umożliwiający zobaczenie tej samej listy filtrowane filmów.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-145">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="ffbb3-146">Należy zauważyć, że adres URL żądania HTTP POST jest taki sam jak adres URL dla żądania GET (localhost:xxxxx/filmy/indeksu) — Brak wyszukiwania informacji w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-146">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL.</span></span> <span data-ttu-id="ffbb3-147">Informacje o parametrach wyszukiwania są wysyłane do serwera jako [stanowią wartość pola](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span><span class="sxs-lookup"><span data-stu-id="ffbb3-147">The search string information is sent to the server as a [form field value](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span></span> <span data-ttu-id="ffbb3-148">Możesz sprawdzić, czy za pomocą narzędzia deweloperskie przeglądarki lub doskonałą [narzędzie Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="ffbb3-148">You can verify that with the browser Developer tools or the excellent [Fiddler tool](http://www.telerik.com/fiddler).</span></span> <span data-ttu-id="ffbb3-149">Na poniższym obrazie przedstawiono narzędzia deweloperskie przeglądarki Chrome:</span><span class="sxs-lookup"><span data-stu-id="ffbb3-149">The image below shows the Chrome browser Developer tools:</span></span>

![Karta Sieć narzędzi dla deweloperów w programie Microsoft Edge wyświetlanie treść żądania z wartością Ciągwyszukiwania ghost](~/tutorials/first-mvc-app/search/_static/f12_rb.png)

<span data-ttu-id="ffbb3-151">Możesz zobaczyć parametru wyszukiwania i [XSRF](xref:security/anti-request-forgery) tokenu w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-151">You can see the search parameter and [XSRF](xref:security/anti-request-forgery) token in the request body.</span></span> <span data-ttu-id="ffbb3-152">Należy pamiętać, zgodnie z opisem w poprzednim samouczku [Pomocnik tagu formularza](xref:mvc/views/working-with-forms) generuje [XSRF](xref:security/anti-request-forgery) token zabezpieczający przed sfałszowaniem.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-152">Note, as mentioned in the previous tutorial, the [Form Tag Helper](xref:mvc/views/working-with-forms) generates an [XSRF](xref:security/anti-request-forgery) anti-forgery token.</span></span> <span data-ttu-id="ffbb3-153">Dane, nie masz zostać zmodyfikowana, więc nie potrzebujemy przeprowadzić walidacji tokenu w metodzie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-153">We're not modifying data, so we don't need to validate the token in the controller method.</span></span>

<span data-ttu-id="ffbb3-154">Ponieważ parametr wyszukiwania znajduje się w treści żądania, a nie jej adres URL, nie można przechwycić informacje wyszukiwania do zakładki lub udostępnienia innym osobom.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-154">Because the search parameter is in the request body and not the URL, you can't capture that search information to bookmark or share with others.</span></span> <span data-ttu-id="ffbb3-155">Poprawka, określając żądania powinna przypominać `HTTP GET`:</span><span class="sxs-lookup"><span data-stu-id="ffbb3-155">Fix this by specifying the request should be `HTTP GET`:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGet.cshtml?highlight=12&range=1-23)]

<span data-ttu-id="ffbb3-156">Teraz gdy prześlesz wyszukiwania, adres URL zawiera ciąg zapytania wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-156">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="ffbb3-157">Wyszukiwanie będzie także przejść do `HttpGet Index` metody akcji, nawet jeśli masz `HttpPost Index` metody.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-157">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![Okno przeglądarki, przedstawiające Ciągwyszukiwania = ghost w adresie Url i filmów, zwrócone, Ghostbusters i Ghostbusters 2, zawierają ghost programu word](~/tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="ffbb3-159">Następujący kod przedstawia zmiany `form` tag:</span><span class="sxs-lookup"><span data-stu-id="ffbb3-159">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="add-search-by-genre"></a><span data-ttu-id="ffbb3-160">Dodaj wyszukiwanie według gatunku</span><span class="sxs-lookup"><span data-stu-id="ffbb3-160">Add Search by genre</span></span>

<span data-ttu-id="ffbb3-161">Dodaj następujący kod `MovieGenreViewModel` klasy *modeli* folderu:</span><span class="sxs-lookup"><span data-stu-id="ffbb3-161">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

<span data-ttu-id="ffbb3-162">Model widoku gatunku filmu będzie zawierać:</span><span class="sxs-lookup"><span data-stu-id="ffbb3-162">The movie-genre view model will contain:</span></span>

* <span data-ttu-id="ffbb3-163">Lista filmów.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-163">A list of movies.</span></span>
* <span data-ttu-id="ffbb3-164">A `SelectList` zawierającego listę gatunki.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-164">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="ffbb3-165">Dzięki temu użytkownikowi na wybranie określonego rodzaju z listy.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-165">This allows the user to select a genre from the list.</span></span>
* <span data-ttu-id="ffbb3-166">`MovieGenre`, zawierającą wybrane gatunku.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-166">`MovieGenre`, which contains the selected genre.</span></span>
* <span data-ttu-id="ffbb3-167">`SearchString`, który zawiera tekst, użytkownicy wprowadzają w polu tekstowym wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-167">`SearchString`, which contains the text users enter in the search text box.</span></span>

<span data-ttu-id="ffbb3-168">Zastąp `Index` method in Class metoda `MoviesController.cs` następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ffbb3-168">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

<span data-ttu-id="ffbb3-169">Poniższy kod jest `LINQ` zapytania, który pobiera wszystkie gatunki z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-169">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_LINQ)]

<span data-ttu-id="ffbb3-170">`SelectList` Gatunków jest tworzona przy wyświetlaniu distinct gatunki (nie chcemy naszej listy wyboru mają zduplikowane gatunki).</span><span class="sxs-lookup"><span data-stu-id="ffbb3-170">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

<span data-ttu-id="ffbb3-171">Gdy użytkownik wyszukuje element, do wartości wyszukiwania są przechowywane w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-171">When the user searches for the item, the search value is retained in the search box.</span></span>

## <a name="add-search-by-genre-to-the-index-view"></a><span data-ttu-id="ffbb3-172">Dodaj wyszukiwanie według gatunku do widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="ffbb3-172">Add search by genre to the Index view</span></span>

<span data-ttu-id="ffbb3-173">Aktualizacja `Index.cshtml` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="ffbb3-173">Update `Index.cshtml` as follows:</span></span>

[!code-cshtml[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,19,28,31,34,37,43)]

<span data-ttu-id="ffbb3-174">Sprawdź wyrażenie lambda, używane w następujących pomocnika kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="ffbb3-174">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.Movies[0].Title)`

<span data-ttu-id="ffbb3-175">W poprzednim kodzie `DisplayNameFor` sprawdza pomocnika kodu HTML `Title` właściwość, do którego odwołuje się wyrażenie lambda, aby ustalić nazwę wyświetlaną.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-175">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="ffbb3-176">Ponieważ wyrażenie lambda jest kontrolowane zamiast oceniane, nie otrzymasz naruszenie zasad dostępu podczas `model`, `model.Movies`, lub `model.Movies[0]` są `null` lub jest pusty.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-176">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.Movies`, or `model.Movies[0]` are `null` or empty.</span></span> <span data-ttu-id="ffbb3-177">Kiedy jest obliczane wyrażenie lambda (na przykład `@Html.DisplayFor(modelItem => item.Title)`), są oceniane wartości właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="ffbb3-177">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="ffbb3-178">Testowanie aplikacji przez wyszukiwanie według gatunku, tytuł filmu i przez:</span><span class="sxs-lookup"><span data-stu-id="ffbb3-178">Test the app by searching by genre, by movie title, and by both:</span></span>

![Wyniki przedstawiający okno przeglądarki https://localhost:5001/Movies?MovieGenre=Comedy&SearchString=2](~/tutorials/first-mvc-app/search/_static/s2.png)

> [!div class="step-by-step"]
> <span data-ttu-id="ffbb3-180">[Poprzednie](controller-methods-views.md)
> [dalej](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="ffbb3-180">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>
