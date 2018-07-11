---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Uzyskiwanie dostępu do danych modelu za pomocą kontrolera | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: a3f3f4a030650ff65b070528c5efa1605be764a0
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38120156"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="080ae-102">Uzyskiwanie dostępu do danych modelu za pomocą kontrolera</span><span class="sxs-lookup"><span data-stu-id="080ae-102">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="080ae-103">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="080ae-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="080ae-104">W tej sekcji utworzysz nową `MoviesController` klasy, a następnie napisać kod, który pobiera dane filmów i wyświetla go w przeglądarce, za pomocą szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="080ae-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="080ae-105">**Skompiluj aplikację** przed przejściem do następnego kroku.</span><span class="sxs-lookup"><span data-stu-id="080ae-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="080ae-106">Nie można skompilować aplikację, otrzymasz błąd podczas dodawania kontrolera.</span><span class="sxs-lookup"><span data-stu-id="080ae-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="080ae-107">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *kontrolerów* folder, a następnie kliknij przycisk **Dodaj**, następnie **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="080ae-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="080ae-108">W **Dodawanie szkieletu** okno dialogowe, kliknij przycisk **kontroler MVC 5 z widokami używający narzędzia Entity Framework**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="080ae-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="080ae-109">Wybierz **Movie (MvcMovie.Models)** dla klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="080ae-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="080ae-110">Wybierz **MovieDBContext (MvcMovie.Models)** dla klasy kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="080ae-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="080ae-111">Wprowadź nazwę kontrolera **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="080ae-111">For the Controller name enter **MoviesController**.</span></span>

  <span data-ttu-id="080ae-112">Na poniższej ilustracji przedstawiono okno dialogowe zakończone.</span><span class="sxs-lookup"><span data-stu-id="080ae-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="080ae-113">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="080ae-113">Click **Add**.</span></span> <span data-ttu-id="080ae-114">(Jeśli wystąpi błąd, prawdopodobnie nie tworzysz aplikacji przed rozpoczęciem dodawania kontrolera.) Program Visual Studio tworzy następujące pliki i foldery:</span><span class="sxs-lookup"><span data-stu-id="080ae-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="080ae-115">*MoviesController.cs* w pliku *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="080ae-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="080ae-116">A *Views\Movies* folderu.</span><span class="sxs-lookup"><span data-stu-id="080ae-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="080ae-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, i *Index.cshtml* w nowym *Views\Movies* folderu.</span><span class="sxs-lookup"><span data-stu-id="080ae-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="080ae-118">Visual Studio automatycznie utworzony [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoków (automatyczne tworzenie widoków i metod akcji CRUD jest znany jako funkcja szkieletów).</span><span class="sxs-lookup"><span data-stu-id="080ae-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="080ae-119">Masz teraz aplikację internetową w pełni funkcjonalne, która umożliwia tworzenie, listy, edytować i usuwać wpisy filmu.</span><span class="sxs-lookup"><span data-stu-id="080ae-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="080ae-120">Uruchom aplikację, a następnie kliknij pozycję **filmu MVC** łącza (lub przejdź do `Movies` kontrolera, dodając */Movies* do adresu URL w pasku adresu przeglądarki).</span><span class="sxs-lookup"><span data-stu-id="080ae-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="080ae-121">Ponieważ aplikacja powołuje się na domyślny routing (zdefiniowane w *aplikacji\_Start\RouteConfig.cs* pliku), żądanie przeglądarki `http://localhost:xxxxx/Movies` jest kierowany do domyślnego `Index` metody akcji `Movies` kontrolera.</span><span class="sxs-lookup"><span data-stu-id="080ae-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="080ae-122">Innymi słowy żądanie przeglądarki `http://localhost:xxxxx/Movies` skutecznie jest taka sama jak żądanie przeglądarki `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="080ae-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="080ae-123">Wynik jest pusta lista filmy, ponieważ nie dodano żadnego jeszcze.</span><span class="sxs-lookup"><span data-stu-id="080ae-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="080ae-124">Tworzenie filmu</span><span class="sxs-lookup"><span data-stu-id="080ae-124">Creating a Movie</span></span>

<span data-ttu-id="080ae-125">Wybierz **Utwórz nowy** łącza.</span><span class="sxs-lookup"><span data-stu-id="080ae-125">Select the **Create New** link.</span></span> <span data-ttu-id="080ae-126">Wprowadź informacje na temat filmów, a następnie kliknij przycisk **Utwórz** przycisku.</span><span class="sxs-lookup"><span data-stu-id="080ae-126">Enter some details about a movie and then click the **Create** button.</span></span>


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="080ae-127">Nie można wprowadzić w polu Cena kropki i przecinki.</span><span class="sxs-lookup"><span data-stu-id="080ae-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="080ae-128">Do obsługi dotyczącą weryfikacji jQuery dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (&quot;,&quot;) dla punktu dziesiętnego i formaty daty inne niż angielski, należy wprowadzić *globalize.js* i konkretne  *cultures/globalize.cultures.js* pliku (z [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) i języka JavaScript, aby użyć `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="080ae-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="080ae-129">Czy mogę pokazano, jak to zrobić w następnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="080ae-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="080ae-130">Teraz po prostu wprowadź liczbami całkowitymi, takich jak 10.</span><span class="sxs-lookup"><span data-stu-id="080ae-130">For now, just enter whole numbers like 10.</span></span>


<span data-ttu-id="080ae-131">Klikając **Utwórz** przycisku powoduje, że formularz do opublikowania na serwerze, gdzie informacje filmu są zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="080ae-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="080ae-132">Użytkownik jest następnie przekierowywane do */Movies* adresu URL, w którym można zobaczyć nowo utworzoną filmu na liście.</span><span class="sxs-lookup"><span data-stu-id="080ae-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="080ae-133">Utwórz kilka więcej wpisów filmu.</span><span class="sxs-lookup"><span data-stu-id="080ae-133">Create a couple more movie entries.</span></span> <span data-ttu-id="080ae-134">Spróbuj **Edytuj**, **szczegóły**, i **Usuń** łącza, które są wszystkie funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="080ae-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="080ae-135">Badanie wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="080ae-135">Examining the Generated Code</span></span>

<span data-ttu-id="080ae-136">Otwórz *Controllers\MoviesController.cs* plików i zbadaj wygenerowany `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="080ae-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="080ae-137">Część kontroler film z `Index` metoda znajdują się poniżej.</span><span class="sxs-lookup"><span data-stu-id="080ae-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="080ae-138">Żądanie `Movies` kontroler zwraca wszystkie wpisy w `Movies` tabeli, a następnie przekazuje wyniki do `Index` widoku.</span><span class="sxs-lookup"><span data-stu-id="080ae-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="080ae-139">Poniższy wiersz z `MoviesController` klasy tworzy kontekst bazy danych filmów, zgodnie z wcześniejszym opisem.</span><span class="sxs-lookup"><span data-stu-id="080ae-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="080ae-140">Kontekst bazy danych filmów umożliwia zapytania, edytowanie i usuwanie filmów.</span><span class="sxs-lookup"><span data-stu-id="080ae-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="080ae-141">Silnie Typizowane modeli i @model — słowo kluczowe</span><span class="sxs-lookup"><span data-stu-id="080ae-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="080ae-142">Wcześniej w tym samouczku pokazano, jak kontroler można przekazać dane i obiekty za pomocą szablonu widoku `ViewBag` obiektu.</span><span class="sxs-lookup"><span data-stu-id="080ae-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="080ae-143">`ViewBag` To obiekt dynamiczny, która zapewnia wygodny sposób z późnym wiązaniem do przekazywania informacji do widoku.</span><span class="sxs-lookup"><span data-stu-id="080ae-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="080ae-144">MVC udostępnia również możliwość przekazywania *silnie* typizowanych obiektów do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="080ae-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="080ae-145">To silnie typizowany podejście umożliwia lepsze kompilacji sprawdzania kodu i bardziej rozbudowane [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) w edytorze programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="080ae-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="080ae-146">Mechanizm tworzenia szkieletów w programie Visual Studio używane takie podejście (oznacza to, przekazując *silnie* typizowany model) z `MoviesController` klasy i Wyświetl szablony utworzenia metod i widoki.</span><span class="sxs-lookup"><span data-stu-id="080ae-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="080ae-147">W *Controllers\MoviesController.cs* pliku zbadać wygenerowany `Details` metody.</span><span class="sxs-lookup"><span data-stu-id="080ae-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="080ae-148">`Details` Metoda znajdują się poniżej.</span><span class="sxs-lookup"><span data-stu-id="080ae-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="080ae-149">`id` Parametr jest zwykle przekazywany jako dane trasy, na przykład `http://localhost:1234/movies/details/1` ustawi kontrolera do kontrolera filmu, działanie `details` i `id` 1.</span><span class="sxs-lookup"><span data-stu-id="080ae-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="080ae-150">Można również przekazuje się w identyfikatorze o ciągu zapytania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="080ae-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="080ae-151">Jeśli `Movie` zostanie znaleziony, wystąpienie `Movie` modelu jest przekazywany do `Details` widoku:</span><span class="sxs-lookup"><span data-stu-id="080ae-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="080ae-152">Sprawdź zawartość *Views\Movies\Details.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="080ae-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="080ae-153">Jeśli dołączysz `@model` instrukcji w górnej części pliku szablonu widoku, można określić typu obiektu, który oczekuje, że widok.</span><span class="sxs-lookup"><span data-stu-id="080ae-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="080ae-154">Podczas tworzenia kontrolera filmu programu Visual Studio automatycznie uwzględnione następujące `@model` instrukcji na górze *Details.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="080ae-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="080ae-155">To `@model` dyrektywy umożliwia dostęp do filmów, która kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane.</span><span class="sxs-lookup"><span data-stu-id="080ae-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="080ae-156">Na przykład w *Details.cshtml* szablonu, kod przekazuje każdego pola film, aby `DisplayNameFor` i [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) pomocników HTML za pomocą silnie typizowanej `Model` obiektu.</span><span class="sxs-lookup"><span data-stu-id="080ae-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="080ae-157">`Create` i `Edit` metody i Wyświetl szablony też przekazać obiekt modelu filmu.</span><span class="sxs-lookup"><span data-stu-id="080ae-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="080ae-158">Sprawdź *Index.cshtml* Wyświetl szablon i `Index` method in Class metoda *MoviesController.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="080ae-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="080ae-159">Zwróć uwagę, jak kod tworzy [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) obiektu, kiedy wywoływanych przez nią `View` metody pomocnika w `Index` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="080ae-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="080ae-160">Kod następnie przekazuje to `Movies` listy z `Index` metody akcji do widoku:</span><span class="sxs-lookup"><span data-stu-id="080ae-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="080ae-161">Podczas tworzenia kontrolera filmu programu Visual Studio automatycznie uwzględnione następujące `@model` instrukcji na górze *Index.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="080ae-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="080ae-162">To `@model` dyrektywy umożliwia dostęp do listy filmów, które kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane.</span><span class="sxs-lookup"><span data-stu-id="080ae-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="080ae-163">Na przykład w *Index.cshtml* szablonu, kod w pętli filmy wykonując `foreach` instrukcji na silnie typizowaną `Model` obiektu:</span><span class="sxs-lookup"><span data-stu-id="080ae-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="080ae-164">Ponieważ `Model` obiektu zdecydowanie jest wpisane (jako `IEnumerable<Movie>` obiektu), każdy `item` obiektu w pętli jest wpisana jako `Movie`.</span><span class="sxs-lookup"><span data-stu-id="080ae-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="080ae-165">Wśród innych korzyści oznacza to, możesz uzyskać w czasie kompilacji sprawdzania kodu i pełną obsługę technologii IntelliSense w edytorze kodu:</span><span class="sxs-lookup"><span data-stu-id="080ae-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="080ae-167">Praca z bazą danych LocalDB programu SQL Server</span><span class="sxs-lookup"><span data-stu-id="080ae-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="080ae-168">Entity Framework Code First wykrył, że parametry połączenia bazy danych, który został dostarczony wskazywany `Movies` bazy danych, która nie istnieje jeszcze, więc Code First baza danych utworzona automatycznie.</span><span class="sxs-lookup"><span data-stu-id="080ae-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="080ae-169">Możesz sprawdzić, czy został on utworzony przez wyszukiwanie *aplikacji\_danych* folderu.</span><span class="sxs-lookup"><span data-stu-id="080ae-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="080ae-170">Jeśli nie widzisz *Movies.mdf* plików, kliknij **Pokaż wszystkie pliki** znajdujący się w **Eksploratora rozwiązań** narzędzi, kliknij przycisk **Odśwież** przycisk, a następnie rozwiń *aplikacji\_danych* folderu.</span><span class="sxs-lookup"><span data-stu-id="080ae-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="080ae-171">Kliknij dwukrotnie *Movies.mdf* otworzyć **EKSPLORATORA serwera**, następnie rozwiń **tabel** folder, aby wyświetlić tabelę filmów.</span><span class="sxs-lookup"><span data-stu-id="080ae-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="080ae-172">Należy pamiętać, ikonę klucza, obok identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="080ae-172">Note the key icon next to ID.</span></span> <span data-ttu-id="080ae-173">Domyślnie program EF spowoduje, że właściwość o nazwie identyfikator klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="080ae-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="080ae-174">Aby uzyskać więcej informacji na temat struktury jednostek i MVC, zobacz samouczek doskonałą Tom Dykstra na [MVC i programem EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="080ae-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="080ae-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="080ae-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="080ae-176">Kliknij prawym przyciskiem myszy `Movies` tabeli, a następnie wybierz pozycję **Pokaż dane tabeli** do wyświetlenia danych został utworzony.</span><span class="sxs-lookup"><span data-stu-id="080ae-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="080ae-177">Kliknij prawym przyciskiem myszy `Movies` tabeli, a następnie wybierz pozycję **Otwórz definicję tabeli** do znajdują się w tabeli struktury tego Entity Framework Code First utworzone automatycznie.</span><span class="sxs-lookup"><span data-stu-id="080ae-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="080ae-178">Zwróć uwagę jak schemat `Movies` mapy do tabel `Movie` klasa została utworzona wcześniej.</span><span class="sxs-lookup"><span data-stu-id="080ae-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="080ae-179">Entity Framework Code First automatycznie tworzony w tym schemacie na podstawie Twojej `Movie` klasy.</span><span class="sxs-lookup"><span data-stu-id="080ae-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="080ae-180">Gdy skończysz, zamknij połączenie przez kliknięcie prawym przyciskiem myszy *MovieDBContext* i wybierając polecenie **zamknij połączenie**.</span><span class="sxs-lookup"><span data-stu-id="080ae-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="080ae-181">(Jeśli nie zamkniesz połączenie, możesz otrzymać błąd przy następnym uruchomieniu projektu).</span><span class="sxs-lookup"><span data-stu-id="080ae-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="080ae-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="080ae-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span></span>

<span data-ttu-id="080ae-183">Masz teraz bazę danych i strony do wyświetlenia, edytowanie, aktualizowanie i usuwanie danych.</span><span class="sxs-lookup"><span data-stu-id="080ae-183">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="080ae-184">W następnym samouczku utworzymy Sprawdź pozostałą część utworzony szkielet kodu i dodamy `SearchIndex` metody i `SearchIndex` widok, który umożliwia wyszukiwanie filmów w tej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="080ae-184">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="080ae-185">Aby uzyskać więcej informacji na temat korzystania z programu Entity Framework z MVC, zobacz [Tworzenie modelu danych Entity Framework dla aplikacji ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="080ae-185">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="080ae-186">[Poprzednie](creating-a-connection-string.md)
> [dalej](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="080ae-186">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
