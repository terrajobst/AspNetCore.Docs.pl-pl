---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: "Uzyskiwanie dostępu do modelu danych z kontrolera | Dokumentacja firmy Microsoft"
author: Rick-Anderson
description: "Uwaga: Zaktualizowaną wersję tego samouczka jest dostępnych tutaj używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, znacznie prostsza do wykonania i demonstracją..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: f323fe37da739d957a609dc7ca4e71a3c3ab475e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="b2d9a-104">Uzyskiwanie dostępu do modelu danych z kontrolera</span><span class="sxs-lookup"><span data-stu-id="b2d9a-104">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="b2d9a-105">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="b2d9a-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="b2d9a-106">Dostępna jest zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="b2d9a-107">Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="b2d9a-108">W tej sekcji utworzysz nową `MoviesController` klasy i pisania kodu, który pobiera dane filmu i wyświetla je w przeglądarce, za pomocą szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-108">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="b2d9a-109">**Tworzenie aplikacji** przed przejściem do następnego kroku.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-109">**Build the application** before going on to the next step.</span></span>

<span data-ttu-id="b2d9a-110">Kliknij prawym przyciskiem myszy *kontrolerów* folderze i utworzyć nową `MoviesController` kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-110">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="b2d9a-111">Poniższe opcje będą widoczne dopiero skompilować aplikację.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-111">The options below will not appear until you build your application.</span></span> <span data-ttu-id="b2d9a-112">Wybierz następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="b2d9a-112">Select the following options:</span></span>

- <span data-ttu-id="b2d9a-113">Nazwa kontrolera: **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-113">Controller name: **MoviesController**.</span></span> <span data-ttu-id="b2d9a-114">(Jest to wartość domyślna.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-114">(This is the default.</span></span> <span data-ttu-id="b2d9a-115">)</span><span class="sxs-lookup"><span data-stu-id="b2d9a-115">)</span></span>
- <span data-ttu-id="b2d9a-116">Szablon: **kontroler MVC z akcjami odczytu/zapisu i widokami używający narzędzia Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-116">Template: **MVC Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="b2d9a-117">Klasa modelu: **Movie (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-117">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="b2d9a-118">Klasa kontekstu danych: **MovieDBContext (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-118">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="b2d9a-119">Widoki: **Razor (CSHTML)**.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-119">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="b2d9a-120">(Ustawienie domyślne).</span><span class="sxs-lookup"><span data-stu-id="b2d9a-120">(The default.)</span></span>

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="b2d9a-122">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-122">Click **Add**.</span></span> <span data-ttu-id="b2d9a-123">Visual Studio Express tworzy następujące pliki i foldery:</span><span class="sxs-lookup"><span data-stu-id="b2d9a-123">Visual Studio Express creates the following files and folders:</span></span>

- <span data-ttu-id="b2d9a-124">*MoviesController.cs* plik do projektu *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-124">*A MoviesController.cs* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="b2d9a-125">A *filmy* folderu do projektu *widoków* folderu.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-125">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="b2d9a-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, i *Index.cshtml* w nowym *Views\Movies* folderu.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="b2d9a-127">ASP.NET MVC 4 tworzone automatycznie CRUD (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoków (automatyczne tworzenie widoków i metod akcji CRUD nosi nazwę szkieletów).</span><span class="sxs-lookup"><span data-stu-id="b2d9a-127">ASP.NET MVC 4 automatically created the CRUD (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="b2d9a-128">Masz teraz aplikacji sieci web w pełni funkcjonalne, która umożliwia tworzenie, listy, edytować i usuwać wpisy filmu.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-128">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="b2d9a-129">Uruchom aplikację i przejdź do `Movies` kontrolera, dodając */Movies* do adresu URL na pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-129">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="b2d9a-130">Ponieważ aplikacja jest polegania na domyślny routing (zdefiniowany w *Global.asax* pliku), żądanie `http://localhost:xxxxx/Movies` jest kierowany do domyślnej `Index` metody akcji `Movies` kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-130">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="b2d9a-131">Innymi słowy żądanie `http://localhost:xxxxx/Movies` jest taka sama jak żądanie `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-131">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="b2d9a-132">Wynik jest wyświetlana pusta lista filmów, ponieważ nie dodano żadnych serwerów jeszcze.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-132">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a><span data-ttu-id="b2d9a-133">Tworzenie filmu</span><span class="sxs-lookup"><span data-stu-id="b2d9a-133">Creating a Movie</span></span>

<span data-ttu-id="b2d9a-134">Wybierz **Utwórz nowy** łącza.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-134">Select the **Create New** link.</span></span> <span data-ttu-id="b2d9a-135">Wprowadź niektóre szczegóły na temat film, a następnie kliknij przycisk **Utwórz** przycisku.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-135">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image3.png)

<span data-ttu-id="b2d9a-136">Kliknięcie przycisku **Utwórz** kliknięcie przycisku powoduje formularza można opublikować na serwerze, gdzie informacje filmu są zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-136">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="b2d9a-137">Użytkownik jest przekierowywane do */Movies* adres URL, w którym można obejrzeć film nowo utworzony na liście.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-137">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="b2d9a-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span><span class="sxs-lookup"><span data-stu-id="b2d9a-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span></span>

<span data-ttu-id="b2d9a-139">Utwórz kilka więcej wpisów filmu.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-139">Create a couple more movie entries.</span></span> <span data-ttu-id="b2d9a-140">Spróbuj **Edytuj**, **szczegóły**, i **usunąć** łącza, które są wszystkie funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-140">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="b2d9a-141">Badanie wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="b2d9a-141">Examining the Generated Code</span></span>

<span data-ttu-id="b2d9a-142">Otwórz *Controllers\MoviesController.cs* pliku i sprawdź, czy wygenerowany `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-142">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="b2d9a-143">Część kontroler film z `Index` metody są wyświetlane poniżej.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-143">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="b2d9a-144">Poniższy wiersz z `MoviesController` klasy tworzy filmu kontekst bazy danych, jak opisano wcześniej.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-144">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="b2d9a-145">Film kontekst bazy danych służy do zapytań, edytowanie i usuwanie filmów.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-145">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

<span data-ttu-id="b2d9a-146">Żądanie `Movies` kontrolera zwraca wszystkie wpisy w `Movies` tabeli filmu bazy danych, a następnie przekazuje wyniki w `Index` widoku.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-146">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="b2d9a-147">Silnie Typizowane modeli i @model — słowo kluczowe</span><span class="sxs-lookup"><span data-stu-id="b2d9a-147">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="b2d9a-148">Wcześniej w tym samouczku widać, jak kontroler może przekazać dane i obiekty do szablonu widoku przy użyciu `ViewBag` obiektu.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-148">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="b2d9a-149">`ViewBag` Jest to obiekt dynamiczny, które oferują wygodny sposób późnym wiązaniem do przekazywania informacji do widoku.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-149">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="b2d9a-150">ASP.NET MVC dostarcza również możliwość przekazywania silnie typizowanej danych lub obiektów do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-150">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="b2d9a-151">To silnie typizowane metody umożliwia lepsze kompilacji Sprawdzanie kodu i bardziej zaawansowane funkcje IntelliSense w edytorze programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-151">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Studio editor.</span></span> <span data-ttu-id="b2d9a-152">Takie podejście z używany mechanizm szkieletów w programie Visual Studio `MoviesController` szablonów klasy i widoku podczas jego tworzenia widoków i metod.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-152">The scaffolding mechanism in Visual Studio used this approach with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="b2d9a-153">W *Controllers\MoviesController.cs* zbadać wygenerowany plik `Details` metody.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-153">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="b2d9a-154">Część kontroler film z `Details` metody są wyświetlane poniżej.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-154">A portion of the movie controller with the `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

<span data-ttu-id="b2d9a-155">Jeśli `Movie` zostanie znaleziony, wystąpienie `Movie` modelu są przekazywane do widoku szczegółów.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-155">If a `Movie` is found, an instance of the `Movie` model is passed to the Details view.</span></span> <span data-ttu-id="b2d9a-156">Sprawdź zawartość *Views\Movies\Details.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-156">Examine the contents of the *Views\Movies\Details.cshtml* file.</span></span>

<span data-ttu-id="b2d9a-157">W tym `@model` instrukcji w górnej części pliku szablonu widoku, można określić typu obiektu, który oczekuje widoku.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-157">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="b2d9a-158">Podczas tworzenia kontrolera film, Visual Studio automatycznie uwzględnione następujące `@model` instrukcji w górnej części *Details.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="b2d9a-158">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

<span data-ttu-id="b2d9a-159">To `@model` dyrektywy umożliwia dostęp do filmów, który kontroler przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-159">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="b2d9a-160">Na przykład w *Details.cshtml* szablonu, kod przekazuje każde pole film, aby `DisplayNameFor` i [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) pomocników HTML z silnie typizowaną `Model` obiektu.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-160">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="b2d9a-161">Tworzenie i edytowanie metody i Wyświetl szablony również przekazać film obiekt modelu.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-161">The Create and Edit methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="b2d9a-162">Sprawdź *Index.cshtml* Wyświetl szablon i `Index` metody w *MoviesController.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-162">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="b2d9a-163">Zwróć uwagę, jak kod tworzy [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) obiektu, gdy wywołuje `View` metody pomocnika w `Index` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-163">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="b2d9a-164">Kod następnie przekazuje to `Movies` listy z kontrolera do widoku:</span><span class="sxs-lookup"><span data-stu-id="b2d9a-164">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

<span data-ttu-id="b2d9a-165">Podczas tworzenia kontrolera film, Visual Studio Express automatycznie uwzględnione następujące `@model` instrukcji w górnej części *Index.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="b2d9a-165">When you created the movie controller, Visual Studio Express automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="b2d9a-166">To `@model` dyrektywy umożliwia dostęp do listy filmów, które kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-166">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="b2d9a-167">Na przykład w *Index.cshtml* szablonu, kod w pętli filmów za pomocą ćwiczeń `foreach` instrukcji na silnie typizowaną `Model` obiektu:</span><span class="sxs-lookup"><span data-stu-id="b2d9a-167">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="b2d9a-168">Ponieważ `Model` obiektu jest silnie typizowane (jako `IEnumerable<Movie>` obiektu), każdy `item` jest typu obiektu w pętli `Movie`.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-168">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="b2d9a-169">Wśród innych korzyści oznacza to, Pobierz sprawdzanie kompilacji kodu, a następnie pełne obsługę funkcji IntelliSense w edytorze kodu:</span><span class="sxs-lookup"><span data-stu-id="b2d9a-169">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="b2d9a-171">Praca z bazy danych LocalDB programu SQL Server</span><span class="sxs-lookup"><span data-stu-id="b2d9a-171">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="b2d9a-172">Entity Framework Code First wykrył, że podany ciąg połączenia bazy danych wskazywanego `Movies` bazy danych, która nie istnieje jeszcze, więc Code First baza danych utworzona automatycznie.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-172">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="b2d9a-173">Możesz sprawdzić, czy jest on utworzony przez wyszukiwanie *aplikacji\_danych* folderu.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-173">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="b2d9a-174">Jeśli nie widzisz *Movies.mdf* plików, kliknij **Pokaż wszystkie pliki** przycisk **Eksploratora rozwiązań** narzędzi, kliknij przycisk **Odśwież** przycisk, a następnie rozwiń *aplikacji\_danych* folderu.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-174">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="b2d9a-175">Kliknij dwukrotnie *Movies.mdf* otworzyć **EKSPLORATORA bazy danych**, następnie rozwiń węzeł **tabel** folder, aby wyświetlić tabelę filmów.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-175">Double-click *Movies.mdf* to open **DATABASE EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span>

<span data-ttu-id="b2d9a-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="b2d9a-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span></span>

> [!NOTE]
> <span data-ttu-id="b2d9a-177">Jeśli nie ma pomocy Eksploratora bazy danych, z **narzędzia** menu, wybierz **Połącz z bazą danych**, następnie Anuluj **wybierz źródło danych** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-177">If the database explorer doesn't appear, from the **TOOLS** menu, select **Connect to Database**, then cancel the **Choose Data Source** dialog.</span></span> <span data-ttu-id="b2d9a-178">Spowoduje to wymuszenie Otwórz Eksploratora bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-178">This will force open the database explorer.</span></span>


> [!NOTE]
> <span data-ttu-id="b2d9a-179">Jeśli używasz VWD lub Visual Studio 2010 i wystąpi błąd podobny do następującego następujących:</span><span class="sxs-lookup"><span data-stu-id="b2d9a-179">If you are using VWD or Visual Studio 2010 and get an error similar to any of the following following:</span></span>
> 
> - <span data-ttu-id="b2d9a-180">Bazy danych "C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF "nie można otworzyć, ponieważ jest wersja 706.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-180">The database 'C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES.MDF' cannot be opened because it is version 706.</span></span> <span data-ttu-id="b2d9a-181">Ten serwer obsługuje wersję 655 i wcześniejszych.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-181">This server supports version 655 and earlier.</span></span> <span data-ttu-id="b2d9a-182">Na starszą nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-182">A downgrade path is not supported.</span></span>
> - <span data-ttu-id="b2d9a-183">&quot;InvalidOperation wyjątek został obsłużony przez kod użytkownika&quot; dostarczony parametr SqlConnection nie określa wykazu początkowego.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-183">&quot;InvalidOperation Exception was unhandled by user code&quot; The supplied SqlConnection does not specify an initial catalog.</span></span>
> 
> <span data-ttu-id="b2d9a-184">Musisz zainstalować [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) i [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span><span class="sxs-lookup"><span data-stu-id="b2d9a-184">You need to install the [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) and [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span></span> <span data-ttu-id="b2d9a-185">Sprawdź `MovieDBContext` parametrów połączenia określonych na poprzedniej stronie.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-185">Verify the `MovieDBContext` connection string specified on the previous page.</span></span>


<span data-ttu-id="b2d9a-186">Kliknij prawym przyciskiem myszy `Movies` tabeli i wybierz **Pokaż dane tabeli** do wyświetlania danych został utworzony.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-186">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image8.png)

<span data-ttu-id="b2d9a-187">Kliknij prawym przyciskiem myszy `Movies` tabeli i wybierz **Otwórz definicję tabeli** do tabeli struktury tego Entity Framework Code First utworzone automatycznie.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-187">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

<span data-ttu-id="b2d9a-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span><span class="sxs-lookup"><span data-stu-id="b2d9a-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span></span>

![](accessing-your-models-data-from-a-controller/_static/image10.png)

<span data-ttu-id="b2d9a-189">Powiadomienie jak schemat `Movies` mapy do tabel `Movie` klasy utworzony wcześniej.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-189">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="b2d9a-190">Entity Framework Code First automatycznie utworzyć ten schemat na podstawie Twojej `Movie` klasy.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-190">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="b2d9a-191">Po zakończeniu zamknij połączenie przez kliknięcie prawym przyciskiem myszy *MovieDBContext* i wybierając **zamknąć połączenia**.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-191">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="b2d9a-192">(Jeśli nie zamkniesz połączenie, może być błąd pojawia się przy następnym uruchomieniu projektu).</span><span class="sxs-lookup"><span data-stu-id="b2d9a-192">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="b2d9a-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="b2d9a-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span></span>

<span data-ttu-id="b2d9a-194">Istnieje już baza danych i proste listy na stronie zawartości z niego.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-194">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="b2d9a-195">W następnym samouczku będzie firma Microsoft bada reszty szkieletu kodu i dodać `SearchIndex` — metoda i `SearchIndex` widoku, który umożliwia wyszukiwanie filmów w tej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b2d9a-195">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b2d9a-196">[Poprzednie](adding-a-model.md)
[dalej](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="b2d9a-196">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
