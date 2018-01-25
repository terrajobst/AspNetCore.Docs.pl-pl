---
uid: mvc/overview/getting-started/introduction/adding-search
title: Wyszukiwanie | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 116f681e14af0a09a4eb1502ef9f057c5db2f97d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="search"></a><span data-ttu-id="97531-102">Wyszukaj</span><span class="sxs-lookup"><span data-stu-id="97531-102">Search</span></span>
====================
<span data-ttu-id="97531-103">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="97531-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="97531-104">Dodawanie metody wyszukiwania i widoku wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="97531-104">Adding a Search Method and Search View</span></span>

<span data-ttu-id="97531-105">W tej sekcji dodasz możliwość wyszukiwania `Index` metody akcji, która umożliwia wyszukiwanie filmów genre lub nazwę.</span><span class="sxs-lookup"><span data-stu-id="97531-105">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="updating-the-index-form"></a><span data-ttu-id="97531-106">Formularz indeksu</span><span class="sxs-lookup"><span data-stu-id="97531-106">Updating the Index Form</span></span>

<span data-ttu-id="97531-107">Uruchom aktualizując `Index` metody akcji do istniejącej `MoviesController` klasy.</span><span class="sxs-lookup"><span data-stu-id="97531-107">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="97531-108">Oto kod:</span><span class="sxs-lookup"><span data-stu-id="97531-108">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="97531-109">W pierwszym wierszu `Index` metoda tworzy następujące [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) zapytanie, aby wybrać filmy:</span><span class="sxs-lookup"><span data-stu-id="97531-109">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="97531-110">Zapytanie jest zdefiniowany w tym momencie, ale nie został jeszcze uruchomiony w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="97531-110">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="97531-111">Jeśli `searchString` parametru zawiera ciąg, filmy zapytania są modyfikowane w celu filtrowania wartość ciągu wyszukiwania, używając następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="97531-111">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="97531-112">`s => s.Title` Kod powyżej [wyrażenia Lambda](https://msdn.microsoft.com/library/bb397687.aspx).</span><span class="sxs-lookup"><span data-stu-id="97531-112">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/library/bb397687.aspx).</span></span> <span data-ttu-id="97531-113">Wyrażenia lambda są używane w oparte na metodzie [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) wysyła zapytanie jako argumenty do metod operator standardowej kwerendy, takich jak [gdzie](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) metodę używaną w powyższym kodzie.</span><span class="sxs-lookup"><span data-stu-id="97531-113">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="97531-114">Zapytania LINQ nie są wykonywane, gdy są one zdefiniowane lub modyfikacji przez wywołanie metody, takie jak `Where` lub `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="97531-114">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="97531-115">Zamiast tego wykonywania zapytania jest opóźnione, co oznacza, że obliczania wyrażenia jest opóźnione aż do jej wartość rzeczywista jest rzeczywiście iterowane za pośrednictwem lub [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="97531-115">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="97531-116">W `Search` przykładzie zapytanie jest wykonywane w *Index.cshtml* widoku.</span><span class="sxs-lookup"><span data-stu-id="97531-116">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="97531-117">Aby uzyskać więcej informacji na temat wykonywania zapytań odroczonych, zobacz [wykonywania zapytania](https://msdn.microsoft.com/library/bb738633.aspx).</span><span class="sxs-lookup"><span data-stu-id="97531-117">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="97531-118">[Zawiera](https://msdn.microsoft.com/library/bb155125.aspx) metody jest uruchamiany w bazie danych, a nie kodu c# powyżej.</span><span class="sxs-lookup"><span data-stu-id="97531-118">The [Contains](https://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="97531-119">W bazie danych [zawiera](https://msdn.microsoft.com/library/bb155125.aspx) mapuje [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), która jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="97531-119">On the database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="97531-120">Teraz możesz zaktualizować `Index` widok, który będzie wyświetlany formularz dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="97531-120">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="97531-121">Uruchom aplikację i przejdź do */filmów/indeksu*.</span><span class="sxs-lookup"><span data-stu-id="97531-121">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="97531-122">Dołącz ciąg zapytania, takie jak `?searchString=ghost` do adresu URL.</span><span class="sxs-lookup"><span data-stu-id="97531-122">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="97531-123">Filtrowane filmów są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="97531-123">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="97531-125">Jeśli zmienisz podpis `Index` metoda ma parametr o nazwie `id`, `id` parametr będzie odpowiadać `{id}` symbolu zastępczego dla domyślnej kieruje zestawu w *aplikacji\_Start\ RouteConfig.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="97531-125">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.json)]

<span data-ttu-id="97531-126">Oryginalna `Index` metody wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="97531-126">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="97531-127">Zmodyfikowane `Index` metoda będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="97531-127">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="97531-128">Tytuł wyszukiwania mogą być obecnie przekazywane jako dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="97531-128">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="97531-129">Nie można jednak spodziewać się użytkowników, aby zmodyfikować adres URL, za każdym razem, gdy chcą, aby wyszukać filmu.</span><span class="sxs-lookup"><span data-stu-id="97531-129">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="97531-130">Tak, teraz możesz dodasz interfejsu użytkownika w celu ich filtrowania filmów.</span><span class="sxs-lookup"><span data-stu-id="97531-130">So now you you'll add UI to help them filter movies.</span></span> <span data-ttu-id="97531-131">Zmiana podpisu `Index` metody do testowania sposób przekazywania parametru Identyfikatora trasy wiązaniem, zmień ją tak, że Twoje `Index` metoda przyjmuje parametr ciągu o nazwie `searchString`:</span><span class="sxs-lookup"><span data-stu-id="97531-131">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="97531-132">Otwórz *Views\Movies\Index.cshtml* pliku, a tylko po `@Html.ActionLink("Create New", "Create")`, Dodaj znacznika formularza wskazanych poniżej:</span><span class="sxs-lookup"><span data-stu-id="97531-132">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="97531-133">`Html.BeginForm` Pomocnika tworzy otwierania `<form>` tagu.</span><span class="sxs-lookup"><span data-stu-id="97531-133">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="97531-134">`Html.BeginForm` Pomocnika powoduje, że formularz publikowania do samej siebie, gdy użytkownik przesyła formularz, klikając **filtru** przycisku.</span><span class="sxs-lookup"><span data-stu-id="97531-134">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="97531-135">Visual Studio 2013 ma nieuprzywilejowany poprawy podczas wyświetlania i edytowania plików widoku.</span><span class="sxs-lookup"><span data-stu-id="97531-135">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="97531-136">Po uruchomieniu aplikacji przy użyciu pliku widoku otwarte, Visual Studio 2013 wywołuje metodę akcji kontrolera poprawne, aby wyświetlić widok.</span><span class="sxs-lookup"><span data-stu-id="97531-136">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="97531-137">Z widoku indeksu Otwórz w programie Visual Studio (jak pokazano na ilustracji powyżej), wybierz przycisk F5 ewidencyjne lub F5, aby uruchomić aplikację, a następnie spróbuj wyszukiwanie filmu.</span><span class="sxs-lookup"><span data-stu-id="97531-137">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="97531-138">Brak nie `HttpPost` przeciążenia z `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="97531-138">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="97531-139">Nie jest konieczne, ponieważ metoda nie jest zmiana stanu aplikacji, po prostu filtrowania danych.</span><span class="sxs-lookup"><span data-stu-id="97531-139">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="97531-140">Można dodać następujące `HttpPost Index` metody.</span><span class="sxs-lookup"><span data-stu-id="97531-140">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="97531-141">W takim przypadku element wywołujący akcji spowoduje dopasowanie `HttpPost Index` metody i `HttpPost Index` metoda może działać, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="97531-141">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="97531-143">Jednak nawet jeśli dodasz to `HttpPost` wersji `Index` metody, jest to ograniczenie, w jaki sposób to wszystko zostało zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="97531-143">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="97531-144">Załóżmy, że chcesz utworzyć zakładkę określonego wyszukiwania lub chcesz wysłać łącze do znajomych, mogą kliknąć w celu wyświetlenia tego samego filtrowane listy filmów.</span><span class="sxs-lookup"><span data-stu-id="97531-144">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="97531-145">Zwróć uwagę, że adres URL żądania HTTP POST jest taki sam jak adres URL dla żądania GET (localhost:xxxxx/filmów/indeksu) — nie ma żadnych informacji wyszukiwania w adresie URL, do samej siebie.</span><span class="sxs-lookup"><span data-stu-id="97531-145">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="97531-146">Prawo teraz, wyszukaj ciąg informacje są wysyłane do serwera jako wartość pola formularza.</span><span class="sxs-lookup"><span data-stu-id="97531-146">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="97531-147">Oznacza to, że nie można przechwycić informacje wyszukiwania zakładki lub wysyłać do znajomych w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="97531-147">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="97531-148">Rozwiązaniem jest użycie przeciążenia `BeginForm` , który określa żądanie POST należy dodać informacje dotyczące wyszukiwania na adres URL i że powinny być kierowane do `HttpGet` wersji `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="97531-148">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="97531-149">Zamień istniejące bez parametrów `BeginForm` metody z następujący kod:</span><span class="sxs-lookup"><span data-stu-id="97531-149">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="97531-151">Teraz podczas przesyłania wyszukiwania adres URL zawiera ciąg zapytania wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="97531-151">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="97531-152">Wyszukiwanie będzie także przejść do `HttpGet Index` metody akcji, nawet jeśli masz `HttpPost Index` metody.</span><span class="sxs-lookup"><span data-stu-id="97531-152">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="97531-154">Dodawanie wyszukiwania według rodzaju</span><span class="sxs-lookup"><span data-stu-id="97531-154">Adding Search by Genre</span></span>

<span data-ttu-id="97531-155">Jeśli dodano `HttpPost` wersji `Index` metody, usuń go teraz.</span><span class="sxs-lookup"><span data-stu-id="97531-155">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="97531-156">Następnie dodasz funkcji, aby umożliwić użytkownikom wyszukiwanie filmów według rodzaju.</span><span class="sxs-lookup"><span data-stu-id="97531-156">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="97531-157">Zastąp `Index` metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="97531-157">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="97531-158">Ta wersja `Index` metoda przyjmuje dodatkowych parametrów, a mianowicie `movieGenre`.</span><span class="sxs-lookup"><span data-stu-id="97531-158">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="97531-159">Tworzenie pierwszego kilku wierszy kodu `List` obiektu do przechowywania genres film z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="97531-159">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="97531-160">Następujący kod jest zapytania LINQ, która pobiera wszystkie genres z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="97531-160">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="97531-161">W kodzie użyto `AddRange` metody ogólnej `List` kolekcji można dodać do listy wszystkich unikatowych genres.</span><span class="sxs-lookup"><span data-stu-id="97531-161">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="97531-162">(Bez `Distinct` modyfikator, zostanie dodany genres zduplikowane — na przykład zostanie dodany Komedia dwa razy w naszym przykładzie).</span><span class="sxs-lookup"><span data-stu-id="97531-162">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="97531-163">Kod następnie przechowuje listę gatunkami muzyki w `ViewBag.MovieGenre` obiektu.</span><span class="sxs-lookup"><span data-stu-id="97531-163">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="97531-164">Przechowywanie danych kategorii (takie określonego filmu rodzaju firmy) jako [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) obiektu w `ViewBag`, a następnie uzyskiwanie dostępu do danych kategorii w polu listy rozwijanej jest podejście typowej aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="97531-164">Storing category data (such a movie genre's) as a [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="97531-165">Poniższy kod przedstawia sposób sprawdzania `movieGenre` parametru.</span><span class="sxs-lookup"><span data-stu-id="97531-165">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="97531-166">Jeśli nie jest pusta, kodu ogranicza kwerendę filmy i ograniczyć wybranego filmów do określonego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="97531-166">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="97531-167">Jak wspomniano wcześniej, zapytanie nie jest uruchamiane na bazę danych, aż do listy filmów jest iterowane za pośrednictwem (który odbywa się w widoku, po `Index` metoda akcji zwraca).</span><span class="sxs-lookup"><span data-stu-id="97531-167">As stated previously, the query is not run on the data base until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="97531-168">Dodawanie znaczników do obsługi wyszukiwania według rodzaju widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="97531-168">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="97531-169">Dodaj `Html.DropDownList` Pomocnika *Views\Movies\Index.cshtml* pliku tuż przed `TextBox` pomocnika.</span><span class="sxs-lookup"><span data-stu-id="97531-169">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="97531-170">Poniżej przedstawiono wypełniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="97531-170">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="97531-171">W poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="97531-171">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="97531-172">Parametr "MovieGenre" udostępnia klucz dla `DropDownList` pomocnika można znaleźć `IEnumerable<SelectListItem>` w `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="97531-172">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="97531-173">`ViewBag` Został wypełniony w metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="97531-173">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="97531-174">Parametr "All" zawiera etykiety opcji.</span><span class="sxs-lookup"><span data-stu-id="97531-174">The parameter "All" provides an option label.</span></span> <span data-ttu-id="97531-175">Jeśli wybór ten można sprawdzić w przeglądarce, zobaczysz, że jego atrybut "value" jest pusty.</span><span class="sxs-lookup"><span data-stu-id="97531-175">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="97531-176">Ponieważ kontrolera tylko filtry `if` ciąg nie jest `null` lub jest pusta, przesyłanie pustą wartość dla `movieGenre` pokazuje wszystkie genres.</span><span class="sxs-lookup"><span data-stu-id="97531-176">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="97531-177">Można również ustawić opcję należy wybrać domyślnie.</span><span class="sxs-lookup"><span data-stu-id="97531-177">You can also set an option to be selected by default.</span></span> <span data-ttu-id="97531-178">Jeśli potrzebujesz "Komedia" możliwość domyślna zmieniłby kodu w kontrolerze w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="97531-178">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="97531-179">Uruchom aplikację i przejdź do */filmów/indeksu*.</span><span class="sxs-lookup"><span data-stu-id="97531-179">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="97531-180">Spróbować przeprowadzić wyszukiwanie według rodzaju, nazwa filmu i oba kryteria.</span><span class="sxs-lookup"><span data-stu-id="97531-180">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="97531-181">W tej sekcji utworzono metody akcji wyszukiwania i widok, który zezwala użytkownikom na wyszukiwanie według tytuł filmu i rodzaju.</span><span class="sxs-lookup"><span data-stu-id="97531-181">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="97531-182">W następnej sekcji, możesz wyjaśniono, jak dodać właściwości do `Movie` modelu oraz sposobu dodawania inicjatora automatycznie utworzy testowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="97531-182">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="97531-183">[Poprzednie](examining-the-edit-methods-and-edit-view.md)
[dalej](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="97531-183">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
