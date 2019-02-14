---
uid: mvc/overview/getting-started/introduction/adding-search
title: Wyszukiwanie | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/17/2019
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: ada125c917656f3a83524ff39e53b4cfc041a497
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248384"
---
<a name="search"></a><span data-ttu-id="9c446-102">Wyszukaj</span><span class="sxs-lookup"><span data-stu-id="9c446-102">Search</span></span>
====================

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="9c446-103">Dodawanie metody wyszukiwania i widoku wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="9c446-103">Adding a Search Method and Search View</span></span>

<span data-ttu-id="9c446-104">W tej sekcji dodasz możliwości wyszukiwania do `Index` metody akcji, która umożliwia wyszukiwanie filmów według gatunku lub nazwy.</span><span class="sxs-lookup"><span data-stu-id="9c446-104">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c446-105">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9c446-105">Prerequisites</span></span>

<span data-ttu-id="9c446-106">Aby dopasować zrzuty ekranu w tej sekcji, należy do uruchamiania aplikacji (F5) i dodaj następujące filmy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9c446-106">To match this section's screenshots, you need to run the application (F5) and add the following movies to the database.</span></span>

| <span data-ttu-id="9c446-107">Tytuł</span><span class="sxs-lookup"><span data-stu-id="9c446-107">Title</span></span> | <span data-ttu-id="9c446-108">Data wydania</span><span class="sxs-lookup"><span data-stu-id="9c446-108">Release Date</span></span> | <span data-ttu-id="9c446-109">Gatunku</span><span class="sxs-lookup"><span data-stu-id="9c446-109">Genre</span></span> | <span data-ttu-id="9c446-110">Cena</span><span class="sxs-lookup"><span data-stu-id="9c446-110">Price</span></span> |
| ----- | ------------ | ----- | ----- |
| <span data-ttu-id="9c446-111">Ghostbusters</span><span class="sxs-lookup"><span data-stu-id="9c446-111">Ghostbusters</span></span> | <span data-ttu-id="9c446-112">6/8/1984</span><span class="sxs-lookup"><span data-stu-id="9c446-112">6/8/1984</span></span> | <span data-ttu-id="9c446-113">Dokument</span><span class="sxs-lookup"><span data-stu-id="9c446-113">Comedy</span></span> | <span data-ttu-id="9c446-114">6.99</span><span class="sxs-lookup"><span data-stu-id="9c446-114">6.99</span></span> |
| <span data-ttu-id="9c446-115">Ghostbusters II</span><span class="sxs-lookup"><span data-stu-id="9c446-115">Ghostbusters II</span></span> | <span data-ttu-id="9c446-116">6/16/1989</span><span class="sxs-lookup"><span data-stu-id="9c446-116">6/16/1989</span></span> | <span data-ttu-id="9c446-117">Dokument</span><span class="sxs-lookup"><span data-stu-id="9c446-117">Comedy</span></span> | <span data-ttu-id="9c446-118">6.99</span><span class="sxs-lookup"><span data-stu-id="9c446-118">6.99</span></span> |
| <span data-ttu-id="9c446-119">Globalnej małpy</span><span class="sxs-lookup"><span data-stu-id="9c446-119">Planet of the Apes</span></span> | <span data-ttu-id="9c446-120">3/27/1986</span><span class="sxs-lookup"><span data-stu-id="9c446-120">3/27/1986</span></span> | <span data-ttu-id="9c446-121">Akcja</span><span class="sxs-lookup"><span data-stu-id="9c446-121">Action</span></span> | <span data-ttu-id="9c446-122">5.99</span><span class="sxs-lookup"><span data-stu-id="9c446-122">5.99</span></span> |


## <a name="updating-the-index-form"></a><span data-ttu-id="9c446-123">Aktualizowanie indeksu formularza</span><span class="sxs-lookup"><span data-stu-id="9c446-123">Updating the Index Form</span></span>

<span data-ttu-id="9c446-124">Rozpocznij, aktualizując `Index` metody akcji do istniejących `MoviesController` klasy.</span><span class="sxs-lookup"><span data-stu-id="9c446-124">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="9c446-125">Oto kod:</span><span class="sxs-lookup"><span data-stu-id="9c446-125">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="9c446-126">W pierwszym wierszu `Index` metoda tworzy następujące [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) zapytanie, aby wybrać filmów:</span><span class="sxs-lookup"><span data-stu-id="9c446-126">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="9c446-127">Zapytanie jest zdefiniowany w tym momencie, ale nie zostało jeszcze uruchomione w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9c446-127">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="9c446-128">Jeśli `searchString` parametru zawiera ciąg, zapytanie filmy zostanie zmodyfikowany na potrzeby filtrowania na wartość ciągu wyszukiwania, używając następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="9c446-128">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="9c446-129">`s => s.Title` Powyższy kod jest [wyrażenia Lambda](https://msdn.microsoft.com/library/bb397687.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c446-129">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/library/bb397687.aspx).</span></span> <span data-ttu-id="9c446-130">Wyrażenia lambda są używane w oparte na metodzie [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) jako argumenty do standardowych metod operatorów kwerendy takich zapytań jak [gdzie](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) metodę użytą w kodzie powyżej.</span><span class="sxs-lookup"><span data-stu-id="9c446-130">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="9c446-131">Zapytania LINQ nie są wykonywane, gdy są one definiowane lub modyfikacji przez wywołanie metody, takie jak `Where` lub `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="9c446-131">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="9c446-132">Zamiast tego, wykonanie zapytania jest odroczone, co oznacza, że wyniku obliczenia wyrażenia zostanie opóźnione, dopóki wartość zrealizowane faktycznie jest powtarzana lub [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="9c446-132">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="9c446-133">W `Search` przykładzie zapytanie jest wykonywane w *Index.cshtml* widoku.</span><span class="sxs-lookup"><span data-stu-id="9c446-133">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="9c446-134">Aby uzyskać więcej informacji na temat wykonywania zapytań z opóźnieniem, zobacz [wykonywania zapytania](https://msdn.microsoft.com/library/bb738633.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c446-134">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="9c446-135">[Zawiera](https://msdn.microsoft.com/library/bb155125.aspx) metoda jest uruchomiona w bazie danych, a nie kodu c# powyżej.</span><span class="sxs-lookup"><span data-stu-id="9c446-135">The [Contains](https://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="9c446-136">W bazie danych [zawiera](https://msdn.microsoft.com/library/bb155125.aspx) mapuje [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), która jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="9c446-136">On the database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="9c446-137">Teraz możesz zaktualizować `Index` widok, w którym będą wyświetlane na formularzu do użytkownika.</span><span class="sxs-lookup"><span data-stu-id="9c446-137">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="9c446-138">Uruchom aplikację, a następnie przejdź do */filmy/indeks*.</span><span class="sxs-lookup"><span data-stu-id="9c446-138">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="9c446-139">Dołącz ciąg zapytania, takie jak `?searchString=ghost` do adresu URL.</span><span class="sxs-lookup"><span data-stu-id="9c446-139">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="9c446-140">Wyświetlane są filtrowane filmów.</span><span class="sxs-lookup"><span data-stu-id="9c446-140">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="9c446-142">Jeśli zmienisz podpis `Index` metoda będzie miała parametru o nazwie `id`, `id` parametr będzie odpowiadał `{id}` symbolu zastępczego dla domyślnej kieruje zestawu w *aplikacji\_Start\ RouteConfig.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="9c446-142">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.json)]

<span data-ttu-id="9c446-143">Oryginalny `Index` metoda wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="9c446-143">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="9c446-144">Zmodyfikowanego `Index` metoda wyglądałby następująco:</span><span class="sxs-lookup"><span data-stu-id="9c446-144">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="9c446-145">Tytuł wyszukiwania można teraz przekazywać jako dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="9c446-145">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="9c446-146">Jednak nie można oczekiwać od użytkowników, aby zmodyfikować adres URL, za każdym razem, gdy chcą wyszukiwania filmów.</span><span class="sxs-lookup"><span data-stu-id="9c446-146">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="9c446-147">Teraz dodasz interfejs użytkownika, aby pomóc im filtrowanie filmów.</span><span class="sxs-lookup"><span data-stu-id="9c446-147">So now you'll add UI to help them filter movies.</span></span> <span data-ttu-id="9c446-148">Jeśli zmienisz podpis `Index` metodę, aby przetestować sposób przekazywania parametru ID powiązane z tras, zmień ją tak, że Twoje `Index` metoda przyjmuje jako parametr ciągu o nazwie `searchString`:</span><span class="sxs-lookup"><span data-stu-id="9c446-148">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="9c446-149">Otwórz *Views\Movies\Index.cshtml* pliku, a tylko po `@Html.ActionLink("Create New", "Create")`, Dodaj znaczników formularza, które przedstawiono poniżej:</span><span class="sxs-lookup"><span data-stu-id="9c446-149">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="9c446-150">`Html.BeginForm` Pomocnika tworzy otwierający `<form>` tagu.</span><span class="sxs-lookup"><span data-stu-id="9c446-150">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="9c446-151">`Html.BeginForm` Pomocnika powoduje, że formularz do publikowania do samego siebie, gdy użytkownik przesyła formularz, klikając **filtru** przycisku.</span><span class="sxs-lookup"><span data-stu-id="9c446-151">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="9c446-152">Visual Studio 2013 ma nieuprzywilejowany poprawy jakości, podczas wyświetlania i edytowania plików widoku.</span><span class="sxs-lookup"><span data-stu-id="9c446-152">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="9c446-153">Po uruchomieniu aplikacji z plikiem widok Otwórz program Visual Studio 2013 wywołuje metody akcji kontrolera poprawne, aby wyświetlić widok.</span><span class="sxs-lookup"><span data-stu-id="9c446-153">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="9c446-154">Przy użyciu widoku indeksu Otwórz w programie Visual Studio (jak pokazano na ilustracji powyżej), naciśnij przycisk kont F5 lub F5, aby uruchomić aplikację, a następnie spróbuj wyszukać film.</span><span class="sxs-lookup"><span data-stu-id="9c446-154">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="9c446-155">Istnieje nie `HttpPost` przeciążenia `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="9c446-155">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="9c446-156">Nie są potrzebne, ponieważ metoda nie jest zmiana stanu aplikacji, po prostu filtrowania danych.</span><span class="sxs-lookup"><span data-stu-id="9c446-156">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="9c446-157">Można dodać następujące `HttpPost Index` metody.</span><span class="sxs-lookup"><span data-stu-id="9c446-157">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="9c446-158">W takiej sytuacji będzie odpowiadać element wywołujący akcji `HttpPost Index` metody i `HttpPost Index` metoda może działać, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="9c446-158">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="9c446-160">Jednak nawet w przypadku dodania to `HttpPost` wersję `Index` metody, jest to ograniczenie, w jak to wszystko została zaimplementowana.</span><span class="sxs-lookup"><span data-stu-id="9c446-160">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="9c446-161">Wyobraź sobie, że chcesz utworzyć zakładkę określonego wyszukiwania lub chcesz wysłać link do znajomych, mogą kliknąć umożliwiający zobaczenie tej samej listy filtrowane filmów.</span><span class="sxs-lookup"><span data-stu-id="9c446-161">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="9c446-162">Należy zauważyć, że adres URL żądania HTTP POST jest taki sam jak adres URL dla żądania GET (localhost:xxxxx/filmy/indeksu) — nie ma żadnych informacji wyszukiwania w adresie URL, sam.</span><span class="sxs-lookup"><span data-stu-id="9c446-162">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="9c446-163">Po prawej stronie teraz informacje o parametrach wyszukiwania jest wysyłany do serwera jako wartość pola formularza.</span><span class="sxs-lookup"><span data-stu-id="9c446-163">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="9c446-164">Oznacza to, że nie można przechwycić informacje wyszukiwania do zakładki lub wysłać znajomym w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="9c446-164">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="9c446-165">Rozwiązaniem jest używanie przeciążenia `BeginForm` Określa, że żądanie POST, należy dodać informacje dotyczące wyszukiwania do adresu URL i że powinny być kierowane do `HttpGet` wersję `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="9c446-165">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="9c446-166">Zastąp istniejące bez parametrów `BeginForm` metody za pomocą następujących znaczników:</span><span class="sxs-lookup"><span data-stu-id="9c446-166">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="9c446-168">Teraz gdy prześlesz wyszukiwania, adres URL zawiera ciąg zapytania wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="9c446-168">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="9c446-169">Wyszukiwanie będzie także przejść do `HttpGet Index` metody akcji, nawet jeśli masz `HttpPost Index` metody.</span><span class="sxs-lookup"><span data-stu-id="9c446-169">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="9c446-171">Dodawanie wyszukiwania według gatunku</span><span class="sxs-lookup"><span data-stu-id="9c446-171">Adding Search by Genre</span></span>

<span data-ttu-id="9c446-172">Jeśli dodano `HttpPost` wersję `Index` metody, usuń ją teraz.</span><span class="sxs-lookup"><span data-stu-id="9c446-172">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="9c446-173">Następnie dodasz funkcję, aby umożliwić użytkownikom wyszukiwanie filmów według gatunku.</span><span class="sxs-lookup"><span data-stu-id="9c446-173">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="9c446-174">Zastąp `Index` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="9c446-174">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="9c446-175">Ta wersja `Index` metoda przyjmuje dodatkowy parametr, to znaczy `movieGenre`.</span><span class="sxs-lookup"><span data-stu-id="9c446-175">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="9c446-176">Tworzenie pierwszych kilka wierszy kodu `List` obiekt do przechowywania gatunki film z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9c446-176">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="9c446-177">Poniższy kod jest zapytanie LINQ, która pobiera wszystkie gatunki z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9c446-177">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="9c446-178">Kod używa `AddRange` metody ogólnej `List` kolekcję, aby dodać różne gatunki do listy.</span><span class="sxs-lookup"><span data-stu-id="9c446-178">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="9c446-179">(Bez `Distinct` modyfikator, zostaną dodane zduplikowane gatunki — na przykład, zostaną dodane Komedia dwukrotnie w naszym przykładzie).</span><span class="sxs-lookup"><span data-stu-id="9c446-179">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="9c446-180">Kod następnie przechowuje listę gatunki w `ViewBag.MovieGenre` obiektu.</span><span class="sxs-lookup"><span data-stu-id="9c446-180">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="9c446-181">Przechowywanie danych kategorii (takie filmu gatunki) jako [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) obiektu `ViewBag`, uzyskiwanie dostępu do danych kategorii, w polu listy rozwijanej jest typowym podejściem dla aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="9c446-181">Storing category data (such a movie genres) as a [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="9c446-182">Poniższy kod przedstawia sposób sprawdzić `movieGenre` parametru.</span><span class="sxs-lookup"><span data-stu-id="9c446-182">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="9c446-183">Jeśli nie jest pusty, kod dodatkowo ogranicza zapytanie filmy, aby ograniczyć wybranych filmów na określonego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="9c446-183">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="9c446-184">Jak wspomniano wcześniej, zapytania nie jest uruchamiane w bazie danych, aż Lista filmów jest powtarzana (który sytuacja występuje w widoku `Index` metoda akcji zwraca).</span><span class="sxs-lookup"><span data-stu-id="9c446-184">As stated previously, the query is not run on the database until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="9c446-185">Dodawanie znaczników do widoku indeksu, aby obsługiwać wyszukiwanie według gatunku</span><span class="sxs-lookup"><span data-stu-id="9c446-185">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="9c446-186">Dodaj `Html.DropDownList` element pomocniczy służący do *Views\Movies\Index.cshtml* pliku tuż przed `TextBox` pomocnika.</span><span class="sxs-lookup"><span data-stu-id="9c446-186">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="9c446-187">Ukończone znaczników jest pokazany poniżej:</span><span class="sxs-lookup"><span data-stu-id="9c446-187">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="9c446-188">W poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="9c446-188">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="9c446-189">Parametr "MovieGenre" udostępnia klucz dla `DropDownList` pomocnika, aby znaleźć `IEnumerable<SelectListItem>` w `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="9c446-189">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="9c446-190">`ViewBag` Zostało wypełnione w metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="9c446-190">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="9c446-191">Parametr "All" zawiera etykiety opcji.</span><span class="sxs-lookup"><span data-stu-id="9c446-191">The parameter "All" provides an option label.</span></span> <span data-ttu-id="9c446-192">Jeśli wybór sprawdzić się w przeglądarce, zobaczysz, że jego atrybut "value" jest pusty.</span><span class="sxs-lookup"><span data-stu-id="9c446-192">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="9c446-193">Ponieważ kontrolera tylko filtry `if` ciąg nie jest `null` lub jest pusta, pusta wartość dla przesyłania `movieGenre` pokazuje wszystkie gatunki.</span><span class="sxs-lookup"><span data-stu-id="9c446-193">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="9c446-194">Można również ustawić opcję, aby domyślnie zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="9c446-194">You can also set an option to be selected by default.</span></span> <span data-ttu-id="9c446-195">Jeśli chce się "Dokument" jako opcjach domyślny, należy zmienić kod w kontrolerze w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="9c446-195">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="9c446-196">Uruchom aplikację, a następnie przejdź do */filmy/indeks*.</span><span class="sxs-lookup"><span data-stu-id="9c446-196">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="9c446-197">Spróbuj wyszukiwania według gatunku, tytuł filmu i oba kryteria.</span><span class="sxs-lookup"><span data-stu-id="9c446-197">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="9c446-198">W tej sekcji zostały utworzone metody akcji wyszukiwania i widoku, które umożliwiają użytkownikom wyszukiwanie tytuł filmu i gatunku.</span><span class="sxs-lookup"><span data-stu-id="9c446-198">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="9c446-199">W następnej sekcji zostanie przyjrzymy się sposób dodawania właściwości do `Movie` modelu oraz sposób dodawania inicjatora, które automatycznie utworzy test bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9c446-199">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9c446-200">[Poprzednie](examining-the-edit-methods-and-edit-view.md)
> [dalej](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="9c446-200">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
