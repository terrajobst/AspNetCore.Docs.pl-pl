---
title: Zaawansowane platformy ASP.NET Core MVC podstawowych EF - - 10, 10
author: tdykstra
description: "W tym samouczku przedstawiono kilka tematów, które są przydatne pod uwagę przed przejściem do innych niż podstawowe informacje dotyczące projektowania aplikacji sieci web programu ASP.NET korzystających z programu Entity Framework Core."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/advanced
ms.openlocfilehash: ea83e5b17df80e5615dda49335247340d1cfb016
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="advanced-topics---ef-core-with-aspnet-core-mvc-tutorial-10-of-10"></a><span data-ttu-id="5c3e3-103">Tematy zaawansowane - Core EF z samouczek platformy ASP.NET Core MVC (10 10)</span><span class="sxs-lookup"><span data-stu-id="5c3e3-103">Advanced topics - EF Core with ASP.NET Core MVC tutorial (10 of 10)</span></span>

<span data-ttu-id="5c3e3-104">Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5c3e3-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5c3e3-105">Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core MVC przy użyciu programu Entity Framework Core i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="5c3e3-106">Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](intro.md).</span><span class="sxs-lookup"><span data-stu-id="5c3e3-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="5c3e3-107">W samouczku poprzedniej zaimplementowano tabeli na hierarchię dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-107">In the previous tutorial, you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="5c3e3-108">W tym samouczku przedstawiono kilka tematów, które są przydatne pod uwagę przed przejściem do innych niż podstawowe informacje dotyczące projektowania aplikacji sieci web platformy ASP.NET Core, które używają programu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-108">This tutorial introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET Core web applications that use Entity Framework Core.</span></span>

## <a name="raw-sql-queries"></a><span data-ttu-id="5c3e3-109">Zapytania SQL pierwotnych</span><span class="sxs-lookup"><span data-stu-id="5c3e3-109">Raw SQL Queries</span></span>

<span data-ttu-id="5c3e3-110">Jedną z zalet funkcji programu Entity Framework jest, że unika wiązanie kodu zbytnio do określonej metody przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-110">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="5c3e3-111">Robi to przez generowanie zapytań SQL i poleceń, które zwalnia z konieczności pisania samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-111">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="5c3e3-112">Ale w przypadku większości scenariuszy, w należy uruchomić określonego zapytania SQL, które zostały utworzone ręcznie.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-112">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created.</span></span> <span data-ttu-id="5c3e3-113">W tych sytuacjach interfejsu API z pierwszego kodu Entity Framework zawiera metody, które umożliwiają przekazywania poleceń SQL bezpośrednio do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-113">For these scenarios, the Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="5c3e3-114">W wersji 1.0 Core EF są następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-114">You have the following options in EF Core 1.0:</span></span>

* <span data-ttu-id="5c3e3-115">Użyj `DbSet.FromSql` metodę dla zapytań zwracających typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-115">Use the `DbSet.FromSql` method for queries that return entity types.</span></span> <span data-ttu-id="5c3e3-116">Zwracane obiekty muszą mieć typ oczekiwany przez `DbSet` obiektów i ich automatycznie są śledzone przez kontekst bazy danych o ile nie zostanie [wyłączyć śledzenie](crud.md#no-tracking-queries).</span><span class="sxs-lookup"><span data-stu-id="5c3e3-116">The returned objects must be of the type expected by the `DbSet` object, and they are automatically tracked by the database context unless you [turn tracking off](crud.md#no-tracking-queries).</span></span>

* <span data-ttu-id="5c3e3-117">Użyj `Database.ExecuteSqlCommand` -query poleceń.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-117">Use the `Database.ExecuteSqlCommand` for non-query commands.</span></span>

<span data-ttu-id="5c3e3-118">Jeśli musisz uruchomić zapytanie zwracające typy, które nie są jednostek, można użyć ADO.NET z dostarczonych przez EF połączenie z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-118">If you need to run a query that returns types that aren't entities, you can use ADO.NET with the database connection provided by EF.</span></span> <span data-ttu-id="5c3e3-119">Zwrócone dane nie jest śledzony przez kontekst bazy danych, nawet w przypadku użycia tej metody można pobrać typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-119">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>

<span data-ttu-id="5c3e3-120">Podobnie jak zawsze podczas wykonywania polecenia SQL w aplikacji sieci web, należy wykonać środki ostrożności, aby chronić witrynę przed atakami opartymi na wstrzyknięciu kodu SQL.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-120">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="5c3e3-121">Jest jednym ze sposobów to zrobić na potrzeby zapytań sparametryzowanych upewnij się, że ciągi przesłane przez stronę sieci web nie można zinterpretować jako polecenia SQL.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-121">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="5c3e3-122">W tym samouczku użyjesz zapytań sparametryzowanych składników podczas integrowania danych wejściowych użytkownika na kwerendę.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-122">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

## <a name="call-a-query-that-returns-entities"></a><span data-ttu-id="5c3e3-123">Wywołanie kwerendy zwracającej jednostek</span><span class="sxs-lookup"><span data-stu-id="5c3e3-123">Call a query that returns entities</span></span>

<span data-ttu-id="5c3e3-124">`DbSet<TEntity>` Klasa udostępnia metodę, która służy do wykonywania zapytania, które zwraca jednostki typu `TEntity`.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-124">The `DbSet<TEntity>` class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="5c3e3-125">Aby zobaczyć, jak to działa użytkownik będzie Zmień kod w `Details` kontrolera działu.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-125">To see how this works you'll change the code in the `Details` method of the Department controller.</span></span>

<span data-ttu-id="5c3e3-126">W *DepartmentsController.cs*w `Details` metody, Zastąp kod, który pobiera działu z `FromSql` wywołania metody, jak pokazano w poniższym kodzie wyróżnione:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-126">In *DepartmentsController.cs*, in the `Details` method, replace the code that retrieves a department with a `FromSql` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

<span data-ttu-id="5c3e3-127">Aby sprawdzić, czy nowy kod działa poprawnie, wybierz **działów** kartę, a następnie **szczegóły** dla jednego z działów.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-127">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![Szczegóły działu](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a><span data-ttu-id="5c3e3-129">Wywołanie kwerendy zwracającej innych typów</span><span class="sxs-lookup"><span data-stu-id="5c3e3-129">Call a query that returns other types</span></span>

<span data-ttu-id="5c3e3-130">Wcześniej utworzono siatka uczniów statystyki dla strony informacje wskazujące, liczba studentów dla każdego dnia rejestracji.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-130">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="5c3e3-131">Pobrano dane z zestawu jednostek studentów (`_context.Students`) i używać LINQ do projektu wyniki w postaci listy `EnrollmentDateGroup` wyświetlić obiekty modelu.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-131">You got the data from the Students entity set (`_context.Students`) and used LINQ to project the results into a list of `EnrollmentDateGroup` view model objects.</span></span> <span data-ttu-id="5c3e3-132">Załóżmy, że chcesz zapisać SQL samej siebie, a nie za pomocą LINQ.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-132">Suppose you want to write the SQL itself rather than using LINQ.</span></span> <span data-ttu-id="5c3e3-133">Aby zrobić, należy uruchomić kwerendę SQL która zwraca wartość inną niż obiektów jednostek.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-133">To do that you need to run a SQL query that returns something other than entity objects.</span></span> <span data-ttu-id="5c3e3-134">W programie EF Core 1.0 jeden sposób jest napisać kod ADO.NET i uzyskać połączenia z bazą danych z EF.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-134">In EF Core 1.0, one way to do that is write ADO.NET code and get the database connection from EF.</span></span>

<span data-ttu-id="5c3e3-135">W *HomeController.cs*, Zastąp `About` metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-135">In *HomeController.cs*, replace the `About` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

<span data-ttu-id="5c3e3-136">Dodaj using instrukcji:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-136">Add a using statement:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

<span data-ttu-id="5c3e3-137">Uruchom aplikację i przejdź do strony informacje.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-137">Run the app and go to the About page.</span></span> <span data-ttu-id="5c3e3-138">Wyświetla dane, które jak poprzednio.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-138">It displays the same data it did before.</span></span>

![Strona — informacje](advanced/_static/about.png)

## <a name="call-an-update-query"></a><span data-ttu-id="5c3e3-140">Wywołaj zapytanie update</span><span class="sxs-lookup"><span data-stu-id="5c3e3-140">Call an update query</span></span>

<span data-ttu-id="5c3e3-141">Załóżmy, że chcesz wykonać globalnych zmian w bazie danych, takich jak zmiana liczby środków dla porach co pracowników firmy Contoso administracyjnych.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-141">Suppose Contoso University administrators want to perform global changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="5c3e3-142">Jeśli uniwersyteckie ma dużą liczbę kursów, byłoby nieefektywne je odzyskać wszystkie jako jednostek i zmień je pojedynczo.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-142">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="5c3e3-143">W tej sekcji będziesz zaimplementować stronę sieci web, która umożliwia użytkownikowi określenie czynnikiem, o którą należy zmienić numer środków dla wszystkich kursów i wprowadzić zmiany, będzie wykonywania instrukcji SQL UPDATE.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-143">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL UPDATE statement.</span></span> <span data-ttu-id="5c3e3-144">Strony sieci web będzie wyglądać jak na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-144">The web page will look like the following illustration:</span></span>

![Strona środków w trakcie przebiegu aktualizacji](advanced/_static/update-credits.png)

<span data-ttu-id="5c3e3-146">W *CoursesContoller.cs*, Dodaj metody UpdateCourseCredits HttpGet i HttpPost:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-146">In *CoursesContoller.cs*, add UpdateCourseCredits methods for HttpGet and HttpPost:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

<span data-ttu-id="5c3e3-147">Gdy kontroler przetworzy żądanie HttpGet, nic nie zostanie zwrócone w `ViewData["RowsAffected"]`, i wyświetla widok puste pole tekstowe i przycisk Prześlij, jak pokazano na powyższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-147">When the controller processes an HttpGet request, nothing is returned in `ViewData["RowsAffected"]`, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="5c3e3-148">Gdy **aktualizacji** przycisku, wywoływana jest metoda HttpPost i mnożnik ma wartość wprowadzona w polu tekstowym.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-148">When the **Update** button is clicked, the HttpPost method is called, and multiplier has the value entered in the text box.</span></span> <span data-ttu-id="5c3e3-149">Kod wykonywany następnie SQL Server, który aktualizuje kursy i zwraca liczbę wierszy wykorzystywanych do widoku w `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-149">The code then executes the SQL that updates courses and returns the number of affected rows to the view in `ViewData`.</span></span> <span data-ttu-id="5c3e3-150">Jeśli widok pobiera `RowsAffected` wartość wyświetlana liczba zaktualizowanych.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-150">When the view gets a `RowsAffected` value, it displays the number of rows updated.</span></span>

<span data-ttu-id="5c3e3-151">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *widoków/kursów* folder, a następnie kliknij przycisk **Dodaj > Nowy element**.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-151">In **Solution Explorer**, right-click the *Views/Courses* folder, and then click **Add > New Item**.</span></span>

<span data-ttu-id="5c3e3-152">W **Dodaj nowy element** okna dialogowego, kliknij przycisk **ASP.NET** w obszarze **zainstalowana** w okienku po lewej stronie kliknij **strona widoku MVC**oraz nazwę nowego widoku  *UpdateCourseCredits.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-152">In the **Add New Item** dialog, click **ASP.NET** under **Installed** in the left pane, click **MVC View Page**, and name the new view *UpdateCourseCredits.cshtml*.</span></span>

<span data-ttu-id="5c3e3-153">W *Views/Courses/UpdateCourseCredits.cshtml*, Zastąp kod szablonu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-153">In *Views/Courses/UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

<span data-ttu-id="5c3e3-154">Uruchom `UpdateCourseCredits` metody wybierając **kursów** kartę, następnie dodając "/ UpdateCourseCredits" na końcu adresu URL na pasku adresu przeglądarki (na przykład: `http://localhost:5813/Courses/UpdateCourseCredits`).</span><span class="sxs-lookup"><span data-stu-id="5c3e3-154">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:5813/Courses/UpdateCourseCredits`).</span></span> <span data-ttu-id="5c3e3-155">Wprowadź liczbę w polu tekstowym:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-155">Enter a number in the text box:</span></span>

![Strona środków w trakcie przebiegu aktualizacji](advanced/_static/update-credits.png)

<span data-ttu-id="5c3e3-157">Kliknij przycisk **aktualizacji**.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-157">Click **Update**.</span></span> <span data-ttu-id="5c3e3-158">Zostanie wyświetlony liczba zmodyfikowanych wierszy:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-158">You see the number of rows affected:</span></span>

![Uwzględnionych wierszy strony środków w trakcie przebiegu aktualizacji](advanced/_static/update-credits-rows-affected.png)

<span data-ttu-id="5c3e3-160">Kliknij przycisk **powrót do listy** Lista kursów poprawione numer środków.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-160">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="5c3e3-161">Należy pamiętać, że kodzie produkcyjnym czy upewnij się, że zawsze aktualizuje wynikiem są prawidłowe dane.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-161">Note that production code would ensure that updates always result in valid data.</span></span> <span data-ttu-id="5c3e3-162">Kodu uproszczonego pokazane pomnożyć liczbę środków wystarczająco spowodować liczby większe niż 5.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-162">The simplified code shown here could multiply the number of credits enough to result in numbers greater than 5.</span></span> <span data-ttu-id="5c3e3-163">( `Credits` Ma właściwość `[Range(0, 5)]` atrybutu.) Zapytanie update będzie działać, ale nieprawidłowych danych może spowodować nieoczekiwane wyniki w innych częściach systemu, które przyjęto założenie, że liczba kredytów jest 5 lub mniej.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-163">(The `Credits` property has a `[Range(0, 5)]` attribute.) The update query would work but the invalid data could cause unexpected results in other parts of the system that assume the number of credits is 5 or less.</span></span>

<span data-ttu-id="5c3e3-164">Aby uzyskać więcej informacji o raw zapytania SQL, zobacz [Raw zapytania SQL](https://docs.microsoft.com/ef/core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="5c3e3-164">For more information about raw SQL queries, see [Raw SQL Queries](https://docs.microsoft.com/ef/core/querying/raw-sql).</span></span>

## <a name="examine-sql-sent-to-the-database"></a><span data-ttu-id="5c3e3-165">Sprawdź wysłanych do bazy danych SQL</span><span class="sxs-lookup"><span data-stu-id="5c3e3-165">Examine SQL sent to the database</span></span>

<span data-ttu-id="5c3e3-166">Czasami jest przydatne można było wyświetlić rzeczywiste zapytania SQL, które są wysyłane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-166">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="5c3e3-167">Funkcje wbudowane rejestrowanie dla platformy ASP.NET Core jest automatycznie używany przez podstawowe EF zapisywanie dzienników zawierające SQL zapytań i aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-167">Built-in logging functionality for ASP.NET Core is automatically used by EF Core to write logs that contain the SQL for queries and updates.</span></span> <span data-ttu-id="5c3e3-168">W tej sekcji zobaczysz przykłady rejestrowania SQL.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-168">In this section you'll see some examples of SQL logging.</span></span>

<span data-ttu-id="5c3e3-169">Otwórz *StudentsController.cs* i `Details` metody Ustaw punkt przerwania na `if (student == null)` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-169">Open *StudentsController.cs* and in the `Details` method set a breakpoint on the `if (student == null)` statement.</span></span>

<span data-ttu-id="5c3e3-170">Uruchom aplikację w trybie debugowania, a następnie przejdź do strony szczegółów dla studenta.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-170">Run the app in debug mode, and go to the Details page for a student.</span></span>

<span data-ttu-id="5c3e3-171">Przejdź do **dane wyjściowe** wyświetlana debugowania w oknie danych wyjściowych i Zobacz kwerendy:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-171">Go to the **Output** window showing debug output, and you see the query:</span></span>

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

<span data-ttu-id="5c3e3-172">Można zauważyć coś w tym miejscu, które mogą Cię zaskoczył: SQL wybiera wiersze do 2 (`TOP(2)`) z tabeli osoby.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-172">You'll notice something here that might surprise you: the SQL selects up to 2 rows (`TOP(2)`) from the Person table.</span></span> <span data-ttu-id="5c3e3-173">`SingleOrDefaultAsync` — Metoda nie jest rozpoznawane jako 1 wiersz na serwerze.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-173">The `SingleOrDefaultAsync` method doesn't resolve to 1 row on the server.</span></span> <span data-ttu-id="5c3e3-174">Poniżej przedstawiono przyczyny:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-174">Here's why:</span></span>

* <span data-ttu-id="5c3e3-175">Jeśli zapytanie zwróci wiele wierszy, metoda zwraca wartość null.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-175">If the query would return multiple rows, the method returns null.</span></span>
* <span data-ttu-id="5c3e3-176">Aby ustalić, czy zapytanie zwróci wiele wierszy, EF musi sprawdzić, czy zwraca co najmniej 2.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-176">To determine whether the query would return multiple rows, EF has to check if it returns at least 2.</span></span>

<span data-ttu-id="5c3e3-177">Należy pamiętać, że nie trzeba używać trybu debugowania i zatrzyma się w punkt przerwania, aby uzyskać dane wyjściowe rejestrowania w **dane wyjściowe** okna.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-177">Note that you don't have to use debug mode and stop at a breakpoint to get logging output in the **Output** window.</span></span> <span data-ttu-id="5c3e3-178">Jest po prostu wygodny sposób zatrzymania rejestrowania w momencie, które mają zostać znalezione w danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-178">It's just a convenient way to stop the logging at the point you want to look at the output.</span></span> <span data-ttu-id="5c3e3-179">Jeśli użytkownik nie należy kontynuuje rejestrowanie i konieczne przewinięcie do znalezienia interesujących Cię w części.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-179">If you don't do that, logging continues and you have to scroll back to find the parts you're interested in.</span></span>

## <a name="repository-and-unit-of-work-patterns"></a><span data-ttu-id="5c3e3-180">Repozytorium i jednostki pracy</span><span class="sxs-lookup"><span data-stu-id="5c3e3-180">Repository and unit of work patterns</span></span>

<span data-ttu-id="5c3e3-181">Wielu deweloperów napisać kod do implementacji repozytorium i jednostki pracy jako otokę kod, który działa z programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-181">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="5c3e3-182">Te wzorce są przeznaczone do tworzenia warstwy abstrakcji między Warstwa dostępu do danych i warstwy logiki biznesowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-182">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="5c3e3-183">Implementacja tych wzorców może pomóc zabezpieczyć aplikacji od zmian w magazynie danych i może ułatwić automatyczne testy jednostkowe lub projektowanie oparte na (testach TDD).</span><span class="sxs-lookup"><span data-stu-id="5c3e3-183">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="5c3e3-184">Jednak zapisywania dodatkowy kod w celu wdrożenia tych wzorców nie zawsze jest najlepszym wyborem aplikacje używające funkcji EF, z kilku powodów:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-184">However, writing additional code to implement these patterns is not always the best choice for applications that use EF, for several reasons:</span></span>

* <span data-ttu-id="5c3e3-185">Ta sama klasa kontekstu EF powoduje kodu z kodu określonych w przypadku magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-185">The EF context class itself insulates your code from data-store-specific code.</span></span>

* <span data-ttu-id="5c3e3-186">Klasy kontekstu EF może działać jako klasę jednostki pracy dla bazy danych aktualizacje, czy przy użyciu EF.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-186">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>

* <span data-ttu-id="5c3e3-187">EF obejmuje funkcje implementowania TDD bez pisania kodu repozytorium.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-187">EF includes features for implementing TDD without writing repository code.</span></span>

<span data-ttu-id="5c3e3-188">Informacje dotyczące sposobu wdrażania repozytorium i jednostki pracy, zobacz [wersji programu Entity Framework 5 tego samouczka serii](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="5c3e3-188">For information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span></span>

<span data-ttu-id="5c3e3-189">Entity Framework Core implementuje dostawcę bazy danych w pamięci, który może służyć do testowania.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-189">Entity Framework Core implements an in-memory database provider that can be used for testing.</span></span> <span data-ttu-id="5c3e3-190">Aby uzyskać więcej informacji, zobacz [testowanie za pomocą InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).</span><span class="sxs-lookup"><span data-stu-id="5c3e3-190">For more information, see [Testing with InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="5c3e3-191">Zmiana automatycznego wykrywania</span><span class="sxs-lookup"><span data-stu-id="5c3e3-191">Automatic change detection</span></span>

<span data-ttu-id="5c3e3-192">Entity Framework określa zmian jednostki (i w związku z tym aktualizacje, które muszą być wysyłane do bazy danych), porównując bieżące wartości jednostki z oryginalnych wartości.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-192">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="5c3e3-193">Oryginalne wartości są przechowywane po jednostek jest poddawany kwerendzie lub dołączony.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-193">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="5c3e3-194">Niektóre metody, które powodują zmiany automatycznego wykrywania są następujące:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-194">Some of the methods that cause automatic change detection are the following:</span></span>

* <span data-ttu-id="5c3e3-195">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="5c3e3-195">DbContext.SaveChanges</span></span>

* <span data-ttu-id="5c3e3-196">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="5c3e3-196">DbContext.Entry</span></span>

* <span data-ttu-id="5c3e3-197">ChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="5c3e3-197">ChangeTracker.Entries</span></span>

<span data-ttu-id="5c3e3-198">Jeśli podczas śledzenia dużą liczbę jednostek i wywołania jednej z tych metod wiele razy w pętlę, można otrzymać znaczną poprawę wydajności wyłączając tymczasowo zmień automatycznego wykrywania przy użyciu `ChangeTracker.AutoDetectChangesEnabled` właściwości.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-198">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the `ChangeTracker.AutoDetectChangesEnabled` property.</span></span> <span data-ttu-id="5c3e3-199">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-199">For example:</span></span>

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a><span data-ttu-id="5c3e3-200">Entity Framework Core źródła kodu i rozwoju planów</span><span class="sxs-lookup"><span data-stu-id="5c3e3-200">Entity Framework Core source code and development plans</span></span>

<span data-ttu-id="5c3e3-201">Kod źródłowy Entity Framework Core znajduje się w temacie [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="5c3e3-201">The source code for Entity Framework Core is available at [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span></span> <span data-ttu-id="5c3e3-202">Oprócz kodu źródłowego, można pobrać kompilacji co noc, śledzenie problemów, funkcji specyfikacji, uwagi spotkania do projektu [przewodnik przyszłego rozwoju](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap)itd.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-202">Besides source code, you can get nightly builds, issue tracking, feature specs, design meeting notes, [the roadmap for future development](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap), and more.</span></span> <span data-ttu-id="5c3e3-203">Zgłoś błędy, a może przyczynić się własnych rozszerzeń do kodu źródłowego EF.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-203">You can file bugs, and you can contribute your own enhancements to the EF source code.</span></span>

<span data-ttu-id="5c3e3-204">Mimo że kod źródłowy jest otwarty, Entity Framework Core jest w pełni obsługiwany jako produkt firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-204">Although the source code is open, Entity Framework Core is fully supported as a Microsoft product.</span></span> <span data-ttu-id="5c3e3-205">Zespół Microsoft Entity Framework temu formantu, w którym są akceptowane udziały i sprawdza wszystkie zmiany kodu w celu zapewnienia jakości każdej wersji.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-205">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="reverse-engineer-from-existing-database"></a><span data-ttu-id="5c3e3-206">Odtwarzanie z istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="5c3e3-206">Reverse engineer from existing database</span></span>

<span data-ttu-id="5c3e3-207">Aby odtworzyć modelu danych, w tym klas jednostek z istniejącej bazy danych, należy użyć [dbcontext szkieletu](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) polecenia.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-207">To reverse engineer a data model including entity classes from an existing database, use the [scaffold-dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) command.</span></span> <span data-ttu-id="5c3e3-208">Zobacz [samouczka wprowadzającego](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="5c3e3-208">See the [getting-started tutorial](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span></span>

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a><span data-ttu-id="5c3e3-209">Upraszczanie rozliczeniowy wyboru przy użyciu dynamicznych LINQ</span><span class="sxs-lookup"><span data-stu-id="5c3e3-209">Use dynamic LINQ to simplify sort selection code</span></span>

<span data-ttu-id="5c3e3-210">[Trzeci samouczku tej serii](sort-filter-page.md) pokazano, jak napisać kod LINQ przez kodować nazwy kolumn w `switch` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-210">The [third tutorial in this series](sort-filter-page.md) shows how to write LINQ code by hard-coding column names in a `switch` statement.</span></span> <span data-ttu-id="5c3e3-211">Dwie kolumny do wyboru to działa prawidłowo, ale jeśli masz wiele kolumn kodu można pobrać informacji pełnej.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-211">With two columns to choose from, this works fine, but if you have many columns the code could get verbose.</span></span> <span data-ttu-id="5c3e3-212">Aby rozwiązać ten problem, można użyć `EF.Property` metodę, aby określić nazwę właściwości jako ciąg.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-212">To solve that problem, you can use the `EF.Property` method to specify the name of the property as a string.</span></span> <span data-ttu-id="5c3e3-213">Aby wypróbować takie podejście, należy zastąpić `Index` metoda `StudentsController` następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-213">To try out this approach, replace the `Index` method in the `StudentsController` with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a><span data-ttu-id="5c3e3-214">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="5c3e3-214">Next steps</span></span>

<span data-ttu-id="5c3e3-215">Na tym kończy się tej serii samouczków przy użyciu programu Entity Framework Core w aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-215">This completes this series of tutorials on using the Entity Framework Core in an ASP.NET MVC application.</span></span>

<span data-ttu-id="5c3e3-216">Aby uzyskać więcej informacji na temat EF Core, zobacz [dokumentację programu Entity Framework Core](https://docs.microsoft.com/ef/core).</span><span class="sxs-lookup"><span data-stu-id="5c3e3-216">For more information about EF Core, see the [Entity Framework Core documentation](https://docs.microsoft.com/ef/core).</span></span> <span data-ttu-id="5c3e3-217">Jest również dostępna w książce: [Entity Framework Core w akcji](https://www.manning.com/books/entity-framework-core-in-action).</span><span class="sxs-lookup"><span data-stu-id="5c3e3-217">A book is also available: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span></span>

<span data-ttu-id="5c3e3-218">Aby uzyskać informacje na temat wdrażania aplikacji sieci web, zobacz [hosta i wdrażanie](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="5c3e3-218">For information on how to deploy a web app, see [Host and deploy](xref:host-and-deploy/index).</span></span>

<span data-ttu-id="5c3e3-219">Informacje o innych tematów dotyczących podstawowych ASP.NET MVC, takie jak uwierzytelniania i autoryzacji, zobacz [dokumentacji platformy ASP.NET Core](https://docs.microsoft.com/aspnet/core/).</span><span class="sxs-lookup"><span data-stu-id="5c3e3-219">For information about other topics related to ASP.NET Core MVC, such as authentication and authorization, see the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="5c3e3-220">Potwierdzenia</span><span class="sxs-lookup"><span data-stu-id="5c3e3-220">Acknowledgments</span></span>

<span data-ttu-id="5c3e3-221">Tomasz Dykstra i Rick Anderson (twitter @RickAndMSFT) został napisany w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-221">Tom Dykstra and Rick Anderson (twitter @RickAndMSFT) wrote this tutorial.</span></span> <span data-ttu-id="5c3e3-222">Tomaszewski rowan, Diego Vega i innych członków zespołu programu Entity Framework wspierana z przeglądy kodu i pomogła debugowanie problemów, które powstały podczas możemy zostały pisanie kodu dla samouczków.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-222">Rowan Miller, Diego Vega, and other members of the Entity Framework team assisted with code reviews and helped debug issues that arose while we were writing code for the tutorials.</span></span>

## <a name="common-errors"></a><span data-ttu-id="5c3e3-223">Typowe błędy</span><span class="sxs-lookup"><span data-stu-id="5c3e3-223">Common errors</span></span>  

### <a name="contosouniversitydll-used-by-another-process"></a><span data-ttu-id="5c3e3-224">ContosoUniversity.dll używany przez inny proces</span><span class="sxs-lookup"><span data-stu-id="5c3e3-224">ContosoUniversity.dll used by another process</span></span>

<span data-ttu-id="5c3e3-225">Komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-225">Error message:</span></span>

> <span data-ttu-id="5c3e3-226">Nie można otworzyć '... bin\Debug\netcoreapp1.0\ContosoUniversity.dll "do zapisu--" proces nie może uzyskać dostępu do pliku "... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll", ponieważ jest on używany przez inny proces.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-226">Cannot open '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' for writing -- 'The process cannot access the file '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' because it is being used by another process.</span></span>

<span data-ttu-id="5c3e3-227">Rozwiązanie:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-227">Solution:</span></span>

<span data-ttu-id="5c3e3-228">Zatrzymaj witrynę w usługach IIS Express.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-228">Stop the site in IIS Express.</span></span> <span data-ttu-id="5c3e3-229">Przejdź do paska zadań systemu Windows, Znajdź usług IIS Express i kliknij prawym przyciskiem myszy ikonę, wybierz witrynę University firmy Contoso, a następnie kliknij przycisk **Zatrzymaj witrynę**.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-229">Go to the Windows System Tray, find IIS Express and right-click its icon, select the Contoso University site, and then click **Stop Site**.</span></span>

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a><span data-ttu-id="5c3e3-230">Tworzenie szkieletu z żadnego kodu w górę i w dół metody migracji</span><span class="sxs-lookup"><span data-stu-id="5c3e3-230">Migration scaffolded with no code in Up and Down methods</span></span>

<span data-ttu-id="5c3e3-231">Możliwa przyczyna:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-231">Possible cause:</span></span>

<span data-ttu-id="5c3e3-232">Polecenia interfejsu wiersza polecenia EF nie automatycznie Zamknij i Zapisz pliki kodu.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-232">The EF CLI commands don't automatically close and save code files.</span></span> <span data-ttu-id="5c3e3-233">Jeśli masz niezapisane zmiany po uruchomieniu `migrations add` polecenia EF nie odnajdzie zmiany.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-233">If you have unsaved changes when you run the `migrations add` command, EF won't find your changes.</span></span>

<span data-ttu-id="5c3e3-234">Rozwiązanie:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-234">Solution:</span></span>

<span data-ttu-id="5c3e3-235">Uruchom `migrations remove` poleceń, Zapisz zmiany kodu i uruchom ponownie `migrations add` polecenia.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-235">Run the `migrations remove` command, save your code changes and rerun the `migrations add` command.</span></span>

### <a name="errors-while-running-database-update"></a><span data-ttu-id="5c3e3-236">Wystąpiły błędy podczas uruchomionej aktualizacji bazy danych</span><span class="sxs-lookup"><span data-stu-id="5c3e3-236">Errors while running database update</span></span>

<span data-ttu-id="5c3e3-237">Istnieje możliwość pobrania inne błędy podczas wprowadzania zmian schematu w bazie danych istniejących danych.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-237">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="5c3e3-238">Jeśli występują błędy migracji, których nie można rozwiązać, można zmienić nazwę bazy danych w parametrach połączenia lub Usuń bazę danych.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-238">If you get migration errors you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="5c3e3-239">Z nową bazę danych nie ma żadnych danych do migracji, a polecenia update-database jest znacznie bardziej prawdopodobne zakończyć bez błędów.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-239">With a new database, there is no data to migrate, and the update-database command is much more likely to complete without errors.</span></span>

<span data-ttu-id="5c3e3-240">Najprostsza metoda jest można zmienić nazwy bazy danych w *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-240">The simplest approach is to rename the database in *appsettings.json*.</span></span> <span data-ttu-id="5c3e3-241">Przy następnym uruchomieniu `database update`, zostanie utworzona nowa baza danych.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-241">The next time you run `database update`, a new database will be created.</span></span>

<span data-ttu-id="5c3e3-242">Aby usunąć bazę danych w SSOX, kliknij prawym przyciskiem myszy bazę danych, kliknij przycisk **usunąć**, a następnie w **usunąć bazy danych** wybierz okno dialogowe **Zamknij istniejące połączenia** i kliknij przycisk  **OK**.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-242">To delete a database in SSOX, right-click the database, click **Delete**, and then in the **Delete Database** dialog box select **Close existing connections** and click **OK**.</span></span>

<span data-ttu-id="5c3e3-243">Aby usunąć bazę danych za pomocą interfejsu wiersza polecenia, uruchom `database drop` polecenia interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-243">To delete a database by using the CLI, run the `database drop` CLI command:</span></span>

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="5c3e3-244">Błąd podczas lokalizowania wystąpienia programu SQL Server</span><span class="sxs-lookup"><span data-stu-id="5c3e3-244">Error locating SQL Server instance</span></span>

<span data-ttu-id="5c3e3-245">Komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-245">Error Message:</span></span>

> <span data-ttu-id="5c3e3-246">Wystąpił błąd związany z siecią lub wystąpieniem podczas ustanawiania połączenia z programem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-246">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="5c3e3-247">Serwer nie został znaleziony lub był niedostępny.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-247">The server was not found or was not accessible.</span></span> <span data-ttu-id="5c3e3-248">Sprawdź, czy nazwa wystąpienia jest poprawna i czy programu SQL Server jest skonfigurowany do zezwalania na połączenia zdalne.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-248">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="5c3e3-249">(Dostawca: interfejsy sieciowe programu SQL, błąd: 26 - błąd podczas lokalizowania określonego serwera/wystąpienia)</span><span class="sxs-lookup"><span data-stu-id="5c3e3-249">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="5c3e3-250">Rozwiązanie:</span><span class="sxs-lookup"><span data-stu-id="5c3e3-250">Solution:</span></span>

<span data-ttu-id="5c3e3-251">Sprawdź parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-251">Check the connection string.</span></span> <span data-ttu-id="5c3e3-252">Jeśli plik bazy danych został ręcznie usunięty, Zmień nazwę bazy danych w ciągu konstrukcji rozpocząć nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="5c3e3-252">If you have manually deleted the database file, change the name of the database in the construction string to start over with a new database.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="5c3e3-253">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="5c3e3-253">Previous</span></span>](inheritance.md)
