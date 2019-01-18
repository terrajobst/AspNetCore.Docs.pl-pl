---
title: Platforma ASP.NET Core MVC z programem EF Core — zaawansowane — 10 10
author: rick-anderson
description: W tym samouczku przedstawiono przydatnych tematów dla wykraczających poza podstawowe informacje dotyczące tworzenia aplikacji sieci web platformy ASP.NET Core, korzystających z platformy Entity Framework Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-mvc/advanced
ms.openlocfilehash: ba3834b29e78972bf914a5cba1a2cae3cc19a315
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "50090787"
---
# <a name="aspnet-core-mvc-with-ef-core---advanced---10-of-10"></a><span data-ttu-id="5e26f-103">Platforma ASP.NET Core MVC z programem EF Core — zaawansowane — 10 10</span><span class="sxs-lookup"><span data-stu-id="5e26f-103">ASP.NET Core MVC with EF Core - Advanced - 10 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="5e26f-104">Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5e26f-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5e26f-105">Przykładową aplikację sieci web firmy Contoso University pokazuje, jak tworzyć aplikacje sieci web platformy ASP.NET Core MVC za pomocą platformy Entity Framework Core i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5e26f-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="5e26f-106">Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](intro.md).</span><span class="sxs-lookup"><span data-stu-id="5e26f-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="5e26f-107">W poprzednim samouczku wdrożono Tabela wg hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="5e26f-107">In the previous tutorial, you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="5e26f-108">W tym samouczku przedstawiono kilka tematów, które są przydatne pod uwagę podczas czegoś podstawy tworzenia aplikacji sieci web platformy ASP.NET Core, które używają platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="5e26f-108">This tutorial introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET Core web applications that use Entity Framework Core.</span></span>

## <a name="raw-sql-queries"></a><span data-ttu-id="5e26f-109">Pierwotne zapytania SQL</span><span class="sxs-lookup"><span data-stu-id="5e26f-109">Raw SQL Queries</span></span>

<span data-ttu-id="5e26f-110">Jedną z zalet używający narzędzia Entity Framework jest, że takie rozwiązanie pomaga uniknąć wiązanie kodu zbyt ściśle do określonej metody przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="5e26f-110">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="5e26f-111">Dzieje się tak, generując zapytań SQL i poleceń, która uwalnia użytkownika od konieczności pisania samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="5e26f-111">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="5e26f-112">Ale istnieją scenariusze wyjątkowe, gdy trzeba uruchomić określonego zapytania SQL, które zostały utworzone ręcznie.</span><span class="sxs-lookup"><span data-stu-id="5e26f-112">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created.</span></span> <span data-ttu-id="5e26f-113">Dla tych scenariuszy interfejsu API z pierwszym kodu Entity Framework zawiera metody, które umożliwiają przekazywanie poleceń SQL bezpośrednio w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5e26f-113">For these scenarios, the Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="5e26f-114">W programu EF Core 1.0 są dostępne następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="5e26f-114">You have the following options in EF Core 1.0:</span></span>

* <span data-ttu-id="5e26f-115">Użyj `DbSet.FromSql` metodę dla zapytań zwracających typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="5e26f-115">Use the `DbSet.FromSql` method for queries that return entity types.</span></span> <span data-ttu-id="5e26f-116">Zwracanych obiektów musi być typ oczekiwany przez `DbSet` obiektu, dlatego automatycznie są śledzone przez kontekst bazy danych o ile nie zostanie [wyłączyć śledzenie](crud.md#no-tracking-queries).</span><span class="sxs-lookup"><span data-stu-id="5e26f-116">The returned objects must be of the type expected by the `DbSet` object, and they're automatically tracked by the database context unless you [turn tracking off](crud.md#no-tracking-queries).</span></span>

* <span data-ttu-id="5e26f-117">Użyj `Database.ExecuteSqlCommand` poleceń niebędącą zapytaniem.</span><span class="sxs-lookup"><span data-stu-id="5e26f-117">Use the `Database.ExecuteSqlCommand` for non-query commands.</span></span>

<span data-ttu-id="5e26f-118">Jeśli potrzebujesz uruchomić zapytanie, które zwraca typy, które nie są jednostki, można użyć ADO.NET z dostarczonych przez EF połączenie z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="5e26f-118">If you need to run a query that returns types that aren't entities, you can use ADO.NET with the database connection provided by EF.</span></span> <span data-ttu-id="5e26f-119">Zwrócone dane nie są śledzone przez kontekst bazy danych, nawet w przypadku używania tej metody można pobrać typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="5e26f-119">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>

<span data-ttu-id="5e26f-120">Jak jest zawsze wartość true, gdy wykonywanie poleceń SQL w aplikacji sieci web, należy podjąć środki ostrożności, aby chronić witrynę przed atakami polegającymi na iniekcji SQL.</span><span class="sxs-lookup"><span data-stu-id="5e26f-120">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="5e26f-121">Jednym sposobem wykonania tego zadania jest użyć sparametryzowanych zapytań, aby upewnić się, że ciągi przesłane przez stronę sieci web nie może być interpretowany jako polecenia SQL.</span><span class="sxs-lookup"><span data-stu-id="5e26f-121">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="5e26f-122">W tym samouczku użyjesz zapytaniach parametrycznych podczas integrowania danych wejściowych użytkownika na kwerendę.</span><span class="sxs-lookup"><span data-stu-id="5e26f-122">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

## <a name="call-a-query-that-returns-entities"></a><span data-ttu-id="5e26f-123">Wywołania zapytania, które zwraca jednostek</span><span class="sxs-lookup"><span data-stu-id="5e26f-123">Call a query that returns entities</span></span>

<span data-ttu-id="5e26f-124">`DbSet<TEntity>` Klasa dostarcza metody, która służy do wykonywania zapytania, które zwraca jednostki typu `TEntity`.</span><span class="sxs-lookup"><span data-stu-id="5e26f-124">The `DbSet<TEntity>` class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="5e26f-125">Aby zobaczyć, jak to działa możesz poznasz, jak zmienić kod w `Details` metody kontrolera działu.</span><span class="sxs-lookup"><span data-stu-id="5e26f-125">To see how this works you'll change the code in the `Details` method of the Department controller.</span></span>

<span data-ttu-id="5e26f-126">W *DepartmentsController.cs*w `Details` metody, Zastąp kod, który pobiera działowi `FromSql` wywołania metody, jak pokazano na następujący wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="5e26f-126">In *DepartmentsController.cs*, in the `Details` method, replace the code that retrieves a department with a `FromSql` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

<span data-ttu-id="5e26f-127">Aby sprawdzić, czy nowy kod działa poprawnie, należy wybrać **działów** kartę i następnie **szczegóły** dla jednego z działów.</span><span class="sxs-lookup"><span data-stu-id="5e26f-127">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![Szczegóły działu](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a><span data-ttu-id="5e26f-129">Wywołania zapytania zwracającego innych typów</span><span class="sxs-lookup"><span data-stu-id="5e26f-129">Call a query that returns other types</span></span>

<span data-ttu-id="5e26f-130">Wcześniej utworzono uczniów siatki statystyki dla strony informacje, które wykazało, że liczba studentów każdej daty rejestracji.</span><span class="sxs-lookup"><span data-stu-id="5e26f-130">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="5e26f-131">Masz dane z zestawu jednostek uczniów (`_context.Students`) i używać programu LINQ do projektu wyniki w postaci listy `EnrollmentDateGroup` wyświetlić obiekty w modelu.</span><span class="sxs-lookup"><span data-stu-id="5e26f-131">You got the data from the Students entity set (`_context.Students`) and used LINQ to project the results into a list of `EnrollmentDateGroup` view model objects.</span></span> <span data-ttu-id="5e26f-132">Załóżmy, że chcesz napisać SQL sam, a nie za pomocą LINQ.</span><span class="sxs-lookup"><span data-stu-id="5e26f-132">Suppose you want to write the SQL itself rather than using LINQ.</span></span> <span data-ttu-id="5e26f-133">Aby zrobić, należy uruchomić zapytanie SQL zwracającego coś innego niż obiekty jednostki.</span><span class="sxs-lookup"><span data-stu-id="5e26f-133">To do that you need to run a SQL query that returns something other than entity objects.</span></span> <span data-ttu-id="5e26f-134">W programie EF Core 1.0 jednym ze sposobów, aby to zrobić jest pisanie kodu ADO.NET i uzyskać połączenia z bazą danych z programów EF.</span><span class="sxs-lookup"><span data-stu-id="5e26f-134">In EF Core 1.0, one way to do that is write ADO.NET code and get the database connection from EF.</span></span>

<span data-ttu-id="5e26f-135">W *HomeController.cs*, Zastąp `About` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5e26f-135">In *HomeController.cs*, replace the `About` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

<span data-ttu-id="5e26f-136">Dodaj instrukcję using instrukcji:</span><span class="sxs-lookup"><span data-stu-id="5e26f-136">Add a using statement:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

<span data-ttu-id="5e26f-137">Uruchom aplikację, a następnie przejdź do strony informacje.</span><span class="sxs-lookup"><span data-stu-id="5e26f-137">Run the app and go to the About page.</span></span> <span data-ttu-id="5e26f-138">Wyświetla te same dane, które wcześniej.</span><span class="sxs-lookup"><span data-stu-id="5e26f-138">It displays the same data it did before.</span></span>

![Informacje o stronie](advanced/_static/about.png)

## <a name="call-an-update-query"></a><span data-ttu-id="5e26f-140">Wywołaj zapytanie aktualizujące</span><span class="sxs-lookup"><span data-stu-id="5e26f-140">Call an update query</span></span>

<span data-ttu-id="5e26f-141">Załóżmy, że administratorzy Contoso University chce wykonywać globalnych zmian w bazie danych, takich jak zmienianie liczby środki na korzystanie z każdego kursu.</span><span class="sxs-lookup"><span data-stu-id="5e26f-141">Suppose Contoso University administrators want to perform global changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="5e26f-142">Jeśli uniwersytecie ma dużą liczbę kursów, będzie nieefektywne pobierać je wszystkie jako jednostki, a następnie zmianę ich osobno.</span><span class="sxs-lookup"><span data-stu-id="5e26f-142">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="5e26f-143">W tej sekcji możesz wdrożyć strony sieci web, która umożliwia użytkownikowi określenie współczynnik za pomocą którego można zmienić ilość środków, aby uzyskać wszystkie kursy i wprowadzisz zmiany przez wykonanie instrukcji SQL UPDATE.</span><span class="sxs-lookup"><span data-stu-id="5e26f-143">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL UPDATE statement.</span></span> <span data-ttu-id="5e26f-144">Strony sieci web będzie wyglądał jak na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="5e26f-144">The web page will look like the following illustration:</span></span>

![Strona kurs środki na korzystanie z aktualizacji](advanced/_static/update-credits.png)

<span data-ttu-id="5e26f-146">W *CoursesContoller.cs*, Dodaj metody UpdateCourseCredits HttpGet i HttpPost:</span><span class="sxs-lookup"><span data-stu-id="5e26f-146">In *CoursesContoller.cs*, add UpdateCourseCredits methods for HttpGet and HttpPost:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

<span data-ttu-id="5e26f-147">Gdy kontroler przetworzy żądanie HttpGet, nic nie zostanie zwrócone w `ViewData["RowsAffected"]`, i wyświetla widok puste pole tekstowe i przycisk przesyłania, jak pokazano na poprzedniej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="5e26f-147">When the controller processes an HttpGet request, nothing is returned in `ViewData["RowsAffected"]`, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="5e26f-148">Gdy **aktualizacji** przycisku, wywoływana jest metoda HttpPost i mnożnik został wprowadzony w polu tekstowym.</span><span class="sxs-lookup"><span data-stu-id="5e26f-148">When the **Update** button is clicked, the HttpPost method is called, and multiplier has the value entered in the text box.</span></span> <span data-ttu-id="5e26f-149">Kod wykonuje następnie SQL Server, który aktualizuje kursów i zwraca liczbę wierszy dotyczy do widoku `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="5e26f-149">The code then executes the SQL that updates courses and returns the number of affected rows to the view in `ViewData`.</span></span> <span data-ttu-id="5e26f-150">Jeśli widok pobiera `RowsAffected` wartości, wyświetla liczba zaktualizowanych wierszy.</span><span class="sxs-lookup"><span data-stu-id="5e26f-150">When the view gets a `RowsAffected` value, it displays the number of rows updated.</span></span>

<span data-ttu-id="5e26f-151">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *widoków/kursy* folder, a następnie kliknij **Dodaj > Nowy element**.</span><span class="sxs-lookup"><span data-stu-id="5e26f-151">In **Solution Explorer**, right-click the *Views/Courses* folder, and then click **Add > New Item**.</span></span>

<span data-ttu-id="5e26f-152">W **Dodaj nowy element** okno dialogowe, kliknij przycisk **ASP.NET** w obszarze **zainstalowane** w okienku po lewej stronie kliknij **strona widoku MVC**i Nazwij nowy widok  *UpdateCourseCredits.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5e26f-152">In the **Add New Item** dialog, click **ASP.NET** under **Installed** in the left pane, click **MVC View Page**, and name the new view *UpdateCourseCredits.cshtml*.</span></span>

<span data-ttu-id="5e26f-153">W *Views/Courses/UpdateCourseCredits.cshtml*, Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="5e26f-153">In *Views/Courses/UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

<span data-ttu-id="5e26f-154">Uruchom `UpdateCourseCredits` metody, wybierając **kursów** kartę, następnie dodając "/ UpdateCourseCredits" na końcu adresu URL w pasku adresu przeglądarki (na przykład: `http://localhost:5813/Courses/UpdateCourseCredits`).</span><span class="sxs-lookup"><span data-stu-id="5e26f-154">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:5813/Courses/UpdateCourseCredits`).</span></span> <span data-ttu-id="5e26f-155">Wprowadź liczbę w polu tekstowym:</span><span class="sxs-lookup"><span data-stu-id="5e26f-155">Enter a number in the text box:</span></span>

![Strona kurs środki na korzystanie z aktualizacji](advanced/_static/update-credits.png)

<span data-ttu-id="5e26f-157">Kliknij przycisk **Aktualizuj**.</span><span class="sxs-lookup"><span data-stu-id="5e26f-157">Click **Update**.</span></span> <span data-ttu-id="5e26f-158">Zobaczysz liczbę uwzględnionych wierszy:</span><span class="sxs-lookup"><span data-stu-id="5e26f-158">You see the number of rows affected:</span></span>

![Zaktualizowano wierszy strony kurs środki na korzystanie z aktualizacji](advanced/_static/update-credits-rows-affected.png)

<span data-ttu-id="5e26f-160">Kliknij przycisk **powrót do listy** Aby wyświetlić listę kursy z poprawione ilość środków.</span><span class="sxs-lookup"><span data-stu-id="5e26f-160">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="5e26f-161">Należy pamiętać, że kodu produkcyjnego będą upewnij się, że zawsze aktualizuje wynik na liście prawidłowych danych.</span><span class="sxs-lookup"><span data-stu-id="5e26f-161">Note that production code would ensure that updates always result in valid data.</span></span> <span data-ttu-id="5e26f-162">Uproszczony kod przedstawiony tutaj pomnożyć liczbę środki na korzystanie z wystarczająco spowodować liczby większe niż 5.</span><span class="sxs-lookup"><span data-stu-id="5e26f-162">The simplified code shown here could multiply the number of credits enough to result in numbers greater than 5.</span></span> <span data-ttu-id="5e26f-163">( `Credits` Właściwość ma `[Range(0, 5)]` atrybutu.) Zapytanie update będzie działać, ale nieprawidłowych danych może spowodować nieoczekiwane wyniki w innych części systemu, które zakładają, że ilość środków jest 5 lub mniej.</span><span class="sxs-lookup"><span data-stu-id="5e26f-163">(The `Credits` property has a `[Range(0, 5)]` attribute.) The update query would work but the invalid data could cause unexpected results in other parts of the system that assume the number of credits is 5 or less.</span></span>

<span data-ttu-id="5e26f-164">Aby uzyskać więcej informacji na temat pierwotne zapytania SQL, zobacz [pierwotne zapytania SQL](/ef/core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="5e26f-164">For more information about raw SQL queries, see [Raw SQL Queries](/ef/core/querying/raw-sql).</span></span>

## <a name="examine-sql-sent-to-the-database"></a><span data-ttu-id="5e26f-165">Sprawdź SQL wysyłane do bazy danych</span><span class="sxs-lookup"><span data-stu-id="5e26f-165">Examine SQL sent to the database</span></span>

<span data-ttu-id="5e26f-166">Czasami warto będą mogli zobaczyć rzeczywiste zapytania SQL, które są wysyłane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5e26f-166">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="5e26f-167">Funkcje wbudowane funkcje rejestrowania dla platformy ASP.NET Core jest automatycznie używany przez programu EF Core na zapisywanie dzienników, które zawierają SQL dla zapytań i aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="5e26f-167">Built-in logging functionality for ASP.NET Core is automatically used by EF Core to write logs that contain the SQL for queries and updates.</span></span> <span data-ttu-id="5e26f-168">W tej sekcji zobaczysz niektóre przykłady rejestrowania SQL.</span><span class="sxs-lookup"><span data-stu-id="5e26f-168">In this section you'll see some examples of SQL logging.</span></span>

<span data-ttu-id="5e26f-169">Otwórz *StudentsController.cs* i `Details` Metoda Ustaw punkt przerwania na `if (student == null)` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="5e26f-169">Open *StudentsController.cs* and in the `Details` method set a breakpoint on the `if (student == null)` statement.</span></span>

<span data-ttu-id="5e26f-170">Uruchom aplikację w trybie debugowania, a następnie przejdź do strony szczegółów dla uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="5e26f-170">Run the app in debug mode, and go to the Details page for a student.</span></span>

<span data-ttu-id="5e26f-171">Przejdź do **dane wyjściowe** wyświetlana debugowania w oknie danych wyjściowych, a zobaczysz zapytanie:</span><span class="sxs-lookup"><span data-stu-id="5e26f-171">Go to the **Output** window showing debug output, and you see the query:</span></span>

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

<span data-ttu-id="5e26f-172">W tym miejscu coś, co mogą Cię zaskoczyć, zauważysz: SQL wybiera wiersze do 2 (`TOP(2)`) z tabeli osoby.</span><span class="sxs-lookup"><span data-stu-id="5e26f-172">You'll notice something here that might surprise you: the SQL selects up to 2 rows (`TOP(2)`) from the Person table.</span></span> <span data-ttu-id="5e26f-173">`SingleOrDefaultAsync` Metoda nie jest rozpoznawane jako 1 wiersz na serwerze.</span><span class="sxs-lookup"><span data-stu-id="5e26f-173">The `SingleOrDefaultAsync` method doesn't resolve to 1 row on the server.</span></span> <span data-ttu-id="5e26f-174">Oto Dlaczego:</span><span class="sxs-lookup"><span data-stu-id="5e26f-174">Here's why:</span></span>

* <span data-ttu-id="5e26f-175">Jeśli zapytanie zwróci wiele wierszy, metoda zwraca wartość null.</span><span class="sxs-lookup"><span data-stu-id="5e26f-175">If the query would return multiple rows, the method returns null.</span></span>
* <span data-ttu-id="5e26f-176">Aby ustalić, czy zapytanie zwróci wiele wierszy, EF ma pod, jeśli funkcja zwraca wartość co najmniej 2.</span><span class="sxs-lookup"><span data-stu-id="5e26f-176">To determine whether the query would return multiple rows, EF has to check if it returns at least 2.</span></span>

<span data-ttu-id="5e26f-177">Należy pamiętać, że nie trzeba użyć trybu debugowania i zatrzymywanie w punkcie przerwania, aby uzyskać wyniki rejestracji w **dane wyjściowe** okna.</span><span class="sxs-lookup"><span data-stu-id="5e26f-177">Note that you don't have to use debug mode and stop at a breakpoint to get logging output in the **Output** window.</span></span> <span data-ttu-id="5e26f-178">Jest po prostu wygodny sposób zatrzymywanie rejestrowania w momencie, który chcesz Przyjrzyj się dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="5e26f-178">It's just a convenient way to stop the logging at the point you want to look at the output.</span></span> <span data-ttu-id="5e26f-179">Jeśli nie zrobisz, kontynuuje rejestrowanie i masz wróć do części, które interesują Cię znaleźć.</span><span class="sxs-lookup"><span data-stu-id="5e26f-179">If you don't do that, logging continues and you have to scroll back to find the parts you're interested in.</span></span>

## <a name="repository-and-unit-of-work-patterns"></a><span data-ttu-id="5e26f-180">Repozytorium i jednostki pracy</span><span class="sxs-lookup"><span data-stu-id="5e26f-180">Repository and unit of work patterns</span></span>

<span data-ttu-id="5e26f-181">Wielu programistów napisać kod, aby zaimplementować repozytorium i jednostki pracy jako otoka wokół kodu, który współdziała z platformą Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5e26f-181">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="5e26f-182">Te wzorce są przeznaczone do tworzenia warstwę abstrakcji od warstwy dostępu do danych i warstwy logiki biznesowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e26f-182">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="5e26f-183">Implementacji tych wzorców może pomóc ochronić aplikację przed zmianami w magazynie danych i może ułatwić automatyczne testy jednostkowe i Programowanie oparte na testach (TDD).</span><span class="sxs-lookup"><span data-stu-id="5e26f-183">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="5e26f-184">Jednak tworzenia dodatkowego kodu, aby zaimplementować te wzorce nie zawsze jest najlepszym wyborem dla aplikacji, które używają programu EF, z kilku powodów:</span><span class="sxs-lookup"><span data-stu-id="5e26f-184">However, writing additional code to implement these patterns isn't always the best choice for applications that use EF, for several reasons:</span></span>

* <span data-ttu-id="5e26f-185">Samej klasy kontekstu EF powoduje, że kod w kodzie dotyczące magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="5e26f-185">The EF context class itself insulates your code from data-store-specific code.</span></span>

* <span data-ttu-id="5e26f-186">Klasy kontekstu EF może działać jako klasę jednostki pracy dla bazy danych aktualizacje, czy przy użyciu programu EF.</span><span class="sxs-lookup"><span data-stu-id="5e26f-186">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>

* <span data-ttu-id="5e26f-187">EF zawiera funkcje implementowania TDD bez konieczności pisania kodu w repozytorium.</span><span class="sxs-lookup"><span data-stu-id="5e26f-187">EF includes features for implementing TDD without writing repository code.</span></span>

<span data-ttu-id="5e26f-188">Aby uzyskać informacje o sposobie implementacji repozytorium i jednostki pracy, zobacz [Entity Framework 5 wersję tej serii samouczków](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="5e26f-188">For information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span></span>

<span data-ttu-id="5e26f-189">Entity Framework Core implementuje dostawcy bazy danych w pamięci, który może służyć do testowania.</span><span class="sxs-lookup"><span data-stu-id="5e26f-189">Entity Framework Core implements an in-memory database provider that can be used for testing.</span></span> <span data-ttu-id="5e26f-190">Aby uzyskać więcej informacji, zobacz [testu za pomocą InMemory](/ef/core/miscellaneous/testing/in-memory).</span><span class="sxs-lookup"><span data-stu-id="5e26f-190">For more information, see [Test with InMemory](/ef/core/miscellaneous/testing/in-memory).</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="5e26f-191">Automatyczna zmiana wykrywania</span><span class="sxs-lookup"><span data-stu-id="5e26f-191">Automatic change detection</span></span>

<span data-ttu-id="5e26f-192">Entity Framework Określa, jak jednostki został zmieniony (i w związku z tym aktualizacje, które muszą być wysyłane do bazy danych) przez porównanie bieżącej wartości jednostki przy użyciu oryginalnych wartości.</span><span class="sxs-lookup"><span data-stu-id="5e26f-192">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="5e26f-193">Oryginalne wartości są przechowywane, gdy jednostka jest badane lub dołączone.</span><span class="sxs-lookup"><span data-stu-id="5e26f-193">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="5e26f-194">Niektóre metody, które powodują wykrywania automatycznego zmian są następujące:</span><span class="sxs-lookup"><span data-stu-id="5e26f-194">Some of the methods that cause automatic change detection are the following:</span></span>

* <span data-ttu-id="5e26f-195">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="5e26f-195">DbContext.SaveChanges</span></span>

* <span data-ttu-id="5e26f-196">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="5e26f-196">DbContext.Entry</span></span>

* <span data-ttu-id="5e26f-197">ChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="5e26f-197">ChangeTracker.Entries</span></span>

<span data-ttu-id="5e26f-198">Jeśli prześledzić dużą liczbę jednostek można wywołać jedną z następujących metod wiele razy w pętli, może uzyskać znaczne ulepszenia wydajności, tymczasowo wyłączając przy użyciu wykrywania automatyczna zmiana `ChangeTracker.AutoDetectChangesEnabled` właściwości.</span><span class="sxs-lookup"><span data-stu-id="5e26f-198">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the `ChangeTracker.AutoDetectChangesEnabled` property.</span></span> <span data-ttu-id="5e26f-199">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5e26f-199">For example:</span></span>

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a><span data-ttu-id="5e26f-200">Entity Framework Core źródła kodu i tworzenia planów</span><span class="sxs-lookup"><span data-stu-id="5e26f-200">Entity Framework Core source code and development plans</span></span>

<span data-ttu-id="5e26f-201">Źródło platformy Entity Framework Core jest w [ https://github.com/aspnet/EntityFrameworkCore ](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="5e26f-201">The Entity Framework Core source is at [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span></span> <span data-ttu-id="5e26f-202">Repozytorium programu EF Core zawiera nocne kompilacje, śledzenia problemu, specyfikacji funkcji. zadaniem, notatek ze spotkań, projektowania i [plan do przyszłego rozwoju](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span><span class="sxs-lookup"><span data-stu-id="5e26f-202">The EF Core repository contains nightly builds, issue tracking, feature specs, design meeting notes, and [the roadmap for future development](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span></span> <span data-ttu-id="5e26f-203">Możesz plików lub znajdowania usterek i współtworzyć.</span><span class="sxs-lookup"><span data-stu-id="5e26f-203">You can file or find bugs, and contribute.</span></span>

<span data-ttu-id="5e26f-204">Mimo że kod źródłowy jest otwarty, platformy Entity Framework Core jest w pełni obsługiwany jest produktem firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="5e26f-204">Although the source code is open, Entity Framework Core is fully supported as a Microsoft product.</span></span> <span data-ttu-id="5e26f-205">Zespół Microsoft Entity Framework zachowuje kontroli nad tym, którzy są akceptowane wkładu i testy wszystkich zmian w kodzie w celu zapewnienia jakości każdej wersji.</span><span class="sxs-lookup"><span data-stu-id="5e26f-205">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="reverse-engineer-from-existing-database"></a><span data-ttu-id="5e26f-206">Odtwarzanie z istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="5e26f-206">Reverse engineer from existing database</span></span>

<span data-ttu-id="5e26f-207">Aby odtworzyć modelu danych, w tym klas jednostek z istniejącej bazy danych, należy użyć [dbcontext szkieletu](/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) polecenia.</span><span class="sxs-lookup"><span data-stu-id="5e26f-207">To reverse engineer a data model including entity classes from an existing database, use the [scaffold-dbcontext](/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) command.</span></span> <span data-ttu-id="5e26f-208">Zobacz [samouczek ułatwiający rozpoczęcie](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="5e26f-208">See the [getting-started tutorial](/ef/core/get-started/aspnetcore/existing-db).</span></span>

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a><span data-ttu-id="5e26f-209">Korzystanie z dynamicznej LINQ w celu uproszczenia wybór rozliczeniowy</span><span class="sxs-lookup"><span data-stu-id="5e26f-209">Use dynamic LINQ to simplify sort selection code</span></span>

<span data-ttu-id="5e26f-210">[Trzeci samouczek z tej serii](sort-filter-page.md) pokazuje, jak napisać kod LINQ, kodować nazwy kolumn w `switch` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="5e26f-210">The [third tutorial in this series](sort-filter-page.md) shows how to write LINQ code by hard-coding column names in a `switch` statement.</span></span> <span data-ttu-id="5e26f-211">Dwie kolumny do wyboru to działa prawidłowo, ale jeśli masz wiele kolumn, kod można pobrać pełny.</span><span class="sxs-lookup"><span data-stu-id="5e26f-211">With two columns to choose from, this works fine, but if you have many columns the code could get verbose.</span></span> <span data-ttu-id="5e26f-212">Aby rozwiązać ten problem, można użyć `EF.Property` metodę, aby określić nazwę właściwości jako ciąg.</span><span class="sxs-lookup"><span data-stu-id="5e26f-212">To solve that problem, you can use the `EF.Property` method to specify the name of the property as a string.</span></span> <span data-ttu-id="5e26f-213">Aby wypróbować to podejście, Zastąp `Index` method in Class metoda `StudentsController` następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="5e26f-213">To try out this approach, replace the `Index` method in the `StudentsController` with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a><span data-ttu-id="5e26f-214">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="5e26f-214">Next steps</span></span>

<span data-ttu-id="5e26f-215">Na tym kończy się w tej serii samouczków na temat korzystania z programu Entity Framework Core w aplikacji ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="5e26f-215">This completes this series of tutorials on using the Entity Framework Core in an ASP.NET Core MVC application.</span></span>

<span data-ttu-id="5e26f-216">Aby uzyskać więcej informacji na temat programu EF Core, zobacz [dokumentację programu Entity Framework Core](/ef/core).</span><span class="sxs-lookup"><span data-stu-id="5e26f-216">For more information about EF Core, see the [Entity Framework Core documentation](/ef/core).</span></span> <span data-ttu-id="5e26f-217">Książki jest również dostępna: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span><span class="sxs-lookup"><span data-stu-id="5e26f-217">A book is also available: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span></span>

<span data-ttu-id="5e26f-218">Aby uzyskać informacje na temat wdrażania aplikacji sieci web, zobacz <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="5e26f-218">For information on how to deploy a web app, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="5e26f-219">Aby uzyskać informacje o innych tematów dotyczących platformy ASP.NET Core MVC, takie jak uwierzytelnianie i autoryzacja, zobacz <xref:index>.</span><span class="sxs-lookup"><span data-stu-id="5e26f-219">For information about other topics related to ASP.NET Core MVC, such as authentication and authorization, see <xref:index>.</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="5e26f-220">Potwierdzenia</span><span class="sxs-lookup"><span data-stu-id="5e26f-220">Acknowledgments</span></span>

<span data-ttu-id="5e26f-221">Tom Dykstra i Rick Anderson (twitter @RickAndMSFT) napisany w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="5e26f-221">Tom Dykstra and Rick Anderson (twitter @RickAndMSFT) wrote this tutorial.</span></span> <span data-ttu-id="5e26f-222">Rowan Miller, Diego Vega i inni członkowie zespołu programu Entity Framework korzystającej z przeglądy kodu i pomogła debugowanie problemów, które powstały podczas, gdy firma Microsoft była pisanie kodu dla samouczków.</span><span class="sxs-lookup"><span data-stu-id="5e26f-222">Rowan Miller, Diego Vega, and other members of the Entity Framework team assisted with code reviews and helped debug issues that arose while we were writing code for the tutorials.</span></span>

## <a name="common-errors"></a><span data-ttu-id="5e26f-223">Typowe błędy</span><span class="sxs-lookup"><span data-stu-id="5e26f-223">Common errors</span></span>

### <a name="contosouniversitydll-used-by-another-process"></a><span data-ttu-id="5e26f-224">ContosoUniversity.dll używany przez inny proces</span><span class="sxs-lookup"><span data-stu-id="5e26f-224">ContosoUniversity.dll used by another process</span></span>

<span data-ttu-id="5e26f-225">Komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="5e26f-225">Error message:</span></span>

> <span data-ttu-id="5e26f-226">Nie można otworzyć "... bin\Debug\netcoreapp1.0\ContosoUniversity.dll" do zapisu — "proces nie może uzyskać dostępu do pliku"... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll ", ponieważ jest on używany przez inny proces.</span><span class="sxs-lookup"><span data-stu-id="5e26f-226">Cannot open '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' for writing -- 'The process cannot access the file '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' because it is being used by another process.</span></span>

<span data-ttu-id="5e26f-227">Rozwiązanie:</span><span class="sxs-lookup"><span data-stu-id="5e26f-227">Solution:</span></span>

<span data-ttu-id="5e26f-228">Zatrzymaj witrynę w usługach IIS Express.</span><span class="sxs-lookup"><span data-stu-id="5e26f-228">Stop the site in IIS Express.</span></span> <span data-ttu-id="5e26f-229">Przejdź na pasku zadań systemu Windows, Znajdź usług IIS Express i kliknij prawym przyciskiem myszy jego ikonę, wybierz lokację University firmy Contoso, a następnie kliknij przycisk **zatrzymać witrynę**.</span><span class="sxs-lookup"><span data-stu-id="5e26f-229">Go to the Windows System Tray, find IIS Express and right-click its icon, select the Contoso University site, and then click **Stop Site**.</span></span>

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a><span data-ttu-id="5e26f-230">Działanie bez kodu w górę i w dół metod migracji</span><span class="sxs-lookup"><span data-stu-id="5e26f-230">Migration scaffolded with no code in Up and Down methods</span></span>

<span data-ttu-id="5e26f-231">Możliwa przyczyna:</span><span class="sxs-lookup"><span data-stu-id="5e26f-231">Possible cause:</span></span>

<span data-ttu-id="5e26f-232">Polecenia interfejsu wiersza polecenia platformy EF nie automatycznie Zamknij i Zapisz pliki kodu.</span><span class="sxs-lookup"><span data-stu-id="5e26f-232">The EF CLI commands don't automatically close and save code files.</span></span> <span data-ttu-id="5e26f-233">Jeśli istnieją niezapisane zmiany, po uruchomieniu `migrations add` polecenia EF nie odnajdzie zmiany.</span><span class="sxs-lookup"><span data-stu-id="5e26f-233">If you have unsaved changes when you run the `migrations add` command, EF won't find your changes.</span></span>

<span data-ttu-id="5e26f-234">Rozwiązanie:</span><span class="sxs-lookup"><span data-stu-id="5e26f-234">Solution:</span></span>

<span data-ttu-id="5e26f-235">Uruchom `migrations remove` polecenia, Zapisz zmiany kodu i ponownego przeprowadzania `migrations add` polecenia.</span><span class="sxs-lookup"><span data-stu-id="5e26f-235">Run the `migrations remove` command, save your code changes and rerun the `migrations add` command.</span></span>

### <a name="errors-while-running-database-update"></a><span data-ttu-id="5e26f-236">Błędy podczas aktualizacji bazy danych uruchomione</span><span class="sxs-lookup"><span data-stu-id="5e26f-236">Errors while running database update</span></span>

<span data-ttu-id="5e26f-237">Istnieje możliwość uzyskać inne błędy, podczas wprowadzania zmian schematu w bazie danych, która ma istniejące dane.</span><span class="sxs-lookup"><span data-stu-id="5e26f-237">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="5e26f-238">Jeśli występują błędy migracji, których nie można rozwiązać, możesz zmienić nazwę bazy danych w parametrach połączenia lub usunąć bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5e26f-238">If you get migration errors you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="5e26f-239">Za pomocą nowej bazy danych nie ma żadnych danych do migracji, a polecenie update-database jest znacznie bardziej prawdopodobne zakończyć bez błędów.</span><span class="sxs-lookup"><span data-stu-id="5e26f-239">With a new database, there's no data to migrate, and the update-database command is much more likely to complete without errors.</span></span>

<span data-ttu-id="5e26f-240">Najprostszą metodą jest zmiana nazwy bazy danych w *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5e26f-240">The simplest approach is to rename the database in *appsettings.json*.</span></span> <span data-ttu-id="5e26f-241">Przy następnym uruchomieniu `database update`, będzie można utworzyć nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="5e26f-241">The next time you run `database update`, a new database will be created.</span></span>

<span data-ttu-id="5e26f-242">Aby usunąć bazę danych w SSOX, kliknij prawym przyciskiem myszy bazę danych, kliknij przycisk **Usuń**, a następnie w polu **Usuń bazę danych** wybierz okno dialogowe **Zamknij istniejące połączenia** i kliknij przycisk  **OK**.</span><span class="sxs-lookup"><span data-stu-id="5e26f-242">To delete a database in SSOX, right-click the database, click **Delete**, and then in the **Delete Database** dialog box select **Close existing connections** and click **OK**.</span></span>

<span data-ttu-id="5e26f-243">Aby usunąć bazę danych przy użyciu interfejsu wiersza polecenia, uruchom `database drop` interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="5e26f-243">To delete a database by using the CLI, run the `database drop` CLI command:</span></span>

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="5e26f-244">Błąd podczas lokalizowania wystąpienia programu SQL Server</span><span class="sxs-lookup"><span data-stu-id="5e26f-244">Error locating SQL Server instance</span></span>

<span data-ttu-id="5e26f-245">Komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="5e26f-245">Error Message:</span></span>

> <span data-ttu-id="5e26f-246">Wystąpił błąd związany z siecią lub wystąpieniem podczas nawiązywania połączenia z programem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5e26f-246">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="5e26f-247">Serwer nie został znaleziony lub jest on niedostępny.</span><span class="sxs-lookup"><span data-stu-id="5e26f-247">The server was not found or was not accessible.</span></span> <span data-ttu-id="5e26f-248">Sprawdź, czy nazwa wystąpienia jest prawidłowa i że program SQL Server jest skonfigurowany do zezwalania na połączenia zdalne.</span><span class="sxs-lookup"><span data-stu-id="5e26f-248">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="5e26f-249">(Dostawca: Interfejsy sieciowe programu SQL, błąd: 26 — błąd podczas znajdowania/podanego wystąpienia)</span><span class="sxs-lookup"><span data-stu-id="5e26f-249">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="5e26f-250">Rozwiązanie:</span><span class="sxs-lookup"><span data-stu-id="5e26f-250">Solution:</span></span>

<span data-ttu-id="5e26f-251">Sprawdź parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="5e26f-251">Check the connection string.</span></span> <span data-ttu-id="5e26f-252">Jeśli został ręcznie usunięty plik bazy danych, Zmień nazwę bazy danych w ciągu konstrukcji, aby zacząć od nowa z nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="5e26f-252">If you have manually deleted the database file, change the name of the database in the construction string to start over with a new database.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="5e26f-253">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="5e26f-253">Previous</span></span>](inheritance.md)
