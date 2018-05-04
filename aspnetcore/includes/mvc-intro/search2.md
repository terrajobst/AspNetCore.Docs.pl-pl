<!--
[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="67f13-101">Poprzedni `Index` metody:</span><span class="sxs-lookup"><span data-stu-id="67f13-101">The previous `Index` method:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

<span data-ttu-id="67f13-102">Zaktualizowany interfejs `Index` metody z `id` parametru:</span><span class="sxs-lookup"><span data-stu-id="67f13-102">The updated `Index` method with `id` parameter:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

<span data-ttu-id="67f13-103">Tytuł wyszukiwania mogą być obecnie przekazywane jako dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="67f13-103">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![Widok indeksu z widma word dodane do adresu Url i filmu zwrócona lista filmów Ghostbusters i Ghostbusters 2](../../tutorials/first-mvc-app/search/_static/g2.png)

<span data-ttu-id="67f13-105">Nie można jednak spodziewać się użytkowników, aby zmodyfikować adres URL, za każdym razem, gdy chcą, aby wyszukać filmu.</span><span class="sxs-lookup"><span data-stu-id="67f13-105">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="67f13-106">Dlatego teraz dodasz elementy interfejsu użytkownika, aby ułatwić im filtrować filmów.</span><span class="sxs-lookup"><span data-stu-id="67f13-106">So now you'll add UI elements to help them filter movies.</span></span> <span data-ttu-id="67f13-107">Zmiana podpisu `Index` metody do testowania sposób przekazywania trasy wiązaniem `ID` parametr, wróć, aby przyspieszyć parametr o nazwie `searchString`:</span><span class="sxs-lookup"><span data-stu-id="67f13-107">If you changed the signature of the `Index` method to test how to pass the route-bound `ID` parameter, change it back so that it takes a parameter named `searchString`:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

<span data-ttu-id="67f13-108">Otwórz *Views/Movies/Index.cshtml* pliku, a następnie dodaj `<form>` znaczników wskazanych poniżej:</span><span class="sxs-lookup"><span data-stu-id="67f13-108">Open the *Views/Movies/Index.cshtml* file, and add the `<form>` markup highlighted below:</span></span>

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

<span data-ttu-id="67f13-109">Kod HTML `<form>` tagów używa [pomocnika Tag formularza](xref:mvc/views/working-with-forms), więc po przesłaniu formularza ciąg filtru jest zamieszczana `Index` akcji kontrolera filmów.</span><span class="sxs-lookup"><span data-stu-id="67f13-109">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms), so when you submit the form, the filter string is posted to the `Index` action of the movies controller.</span></span> <span data-ttu-id="67f13-110">Zapisz zmiany, a następnie sprawdź filtr.</span><span class="sxs-lookup"><span data-stu-id="67f13-110">Save your changes and then test the filter.</span></span>

![Widok indeksu z widma word wpisane w polu tekstowym filtru tytułu](../../tutorials/first-mvc-app/search/_static/filter.png)

<span data-ttu-id="67f13-112">Brak nie `[HttpPost]` przeciążenia z `Index` metodę jako użytkownik może spodziewać się.</span><span class="sxs-lookup"><span data-stu-id="67f13-112">There's no `[HttpPost]` overload of the `Index` method as you might expect.</span></span> <span data-ttu-id="67f13-113">Nie jest konieczne, ponieważ metoda nie jest zmiana stanu aplikacji, po prostu filtrowania danych.</span><span class="sxs-lookup"><span data-stu-id="67f13-113">You don't need it, because the method isn't changing the state of the app, just filtering data.</span></span>

<span data-ttu-id="67f13-114">Można dodać następujące `[HttpPost] Index` metody.</span><span class="sxs-lookup"><span data-stu-id="67f13-114">You could add the following `[HttpPost] Index` method.</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

<span data-ttu-id="67f13-115">`notUsed` Parametr jest używany do tworzenia przeciążenia `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="67f13-115">The `notUsed` parameter is used to create an overload for the `Index` method.</span></span> <span data-ttu-id="67f13-116">Będzie omawianiu który później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="67f13-116">We'll talk about that later in the tutorial.</span></span>

<span data-ttu-id="67f13-117">Jeśli dodasz tej metody, wywołujący akcji spowoduje dopasowanie `[HttpPost] Index` metody i `[HttpPost] Index` metoda może działać, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="67f13-117">If you add this method, the action invoker would match the `[HttpPost] Index` method, and the `[HttpPost] Index` method would run as shown in the image below.</span></span>

![Okno przeglądarki z odpowiedzi aplikacji z indeksu HttpPost: Filtr widma](../../tutorials/first-mvc-app/search/_static/fo.png)

<span data-ttu-id="67f13-119">Jednak nawet jeśli dodasz to `[HttpPost]` wersji `Index` metody, jest to ograniczenie, w jaki sposób to wszystko zostało zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="67f13-119">However, even if you add this `[HttpPost]` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="67f13-120">Załóżmy, że chcesz utworzyć zakładkę określonego wyszukiwania lub chcesz wysłać łącze do znajomych, mogą kliknąć w celu wyświetlenia tego samego filtrowane listy filmów.</span><span class="sxs-lookup"><span data-stu-id="67f13-120">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="67f13-121">Zwróć uwagę, że adres URL żądania HTTP POST jest taki sam jak adres URL dla żądania GET (localhost:xxxxx/filmów/indeksu) — nie ma żadnych informacji wyszukiwania w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="67f13-121">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL.</span></span> <span data-ttu-id="67f13-122">Informacji o ciągu wyszukiwania są wysyłane do serwera jako [tworzą wartość pola](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span><span class="sxs-lookup"><span data-stu-id="67f13-122">The search string information is sent to the server as a [form field value](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span></span> <span data-ttu-id="67f13-123">Możesz sprawdzić, które z narzędzi deweloperskich przeglądarki lub znakomity [narzędzie Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="67f13-123">You can verify that with the browser Developer tools or the excellent [Fiddler tool](http://www.telerik.com/fiddler).</span></span> <span data-ttu-id="67f13-124">Na poniższym obrazie pokazano narzędzi deweloperskich przeglądarki Chrome:</span><span class="sxs-lookup"><span data-stu-id="67f13-124">The image below shows the Chrome browser Developer tools:</span></span>

![Karta Sieć narzędzi dla deweloperów w programie Microsoft Edge przedstawiający treści żądania z wartością parametru Wyszukiwany_ciąg widma](../../tutorials/first-mvc-app/search/_static/f12_rb.png)

<span data-ttu-id="67f13-126">Parametr wyszukiwania jest widoczny i [XSRF](xref:security/anti-request-forgery) tokenu w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="67f13-126">You can see the search parameter and [XSRF](xref:security/anti-request-forgery) token in the request body.</span></span> <span data-ttu-id="67f13-127">Należy zwrócić uwagę, jak wspomniano w poprzedniej samouczka [pomocnika Tag formularza](xref:mvc/views/working-with-forms) generuje [XSRF](xref:security/anti-request-forgery) token zabezpieczający przed sfałszowaniem.</span><span class="sxs-lookup"><span data-stu-id="67f13-127">Note, as mentioned in the previous tutorial, the [Form Tag Helper](xref:mvc/views/working-with-forms) generates an [XSRF](xref:security/anti-request-forgery) anti-forgery token.</span></span> <span data-ttu-id="67f13-128">Firma Microsoft nie modyfikacji danych, więc nie musimy zweryfikować token metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="67f13-128">We're not modifying data, so we don't need to validate the token in the controller method.</span></span>

<span data-ttu-id="67f13-129">Ponieważ parametr wyszukiwania znajduje się w treści żądania, a nie adres URL, nie można przechwytywanie informacji wyszukiwania do zakładki lub udostępniać innym osobom.</span><span class="sxs-lookup"><span data-stu-id="67f13-129">Because the search parameter is in the request body and not the URL, you can't capture that search information to bookmark or share with others.</span></span> <span data-ttu-id="67f13-130">Firma Microsoft będzie rozwiązać ten problem, określając żądania powinien być `HTTP GET`.</span><span class="sxs-lookup"><span data-stu-id="67f13-130">We'll fix this by specifying the request should be `HTTP GET`.</span></span>
