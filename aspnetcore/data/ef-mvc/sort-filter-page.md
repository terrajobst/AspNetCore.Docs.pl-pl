---
title: Platformy ASP.NET Core MVC EF podstawowych — sortowanie, filtrowanie, stronicowania - 3 10
author: rick-anderson
description: W tym samouczku zostanie dodana sortowanie, filtrowanie i stronicowania funkcji na stronę przy użyciu programu Entity Framework Core i ASP.NET Core.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 34097eacad16c0ffb989efb3b6a8656be4a076cd
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273653"
---
# <a name="aspnet-core-mvc-with-ef-core---sort-filter-paging---3-of-10"></a><span data-ttu-id="609aa-103">Platformy ASP.NET Core MVC EF podstawowych — sortowanie, filtrowanie, stronicowania - 3 10</span><span class="sxs-lookup"><span data-stu-id="609aa-103">ASP.NET Core MVC with EF Core - Sort, Filter, Paging - 3 of 10</span></span>

<span data-ttu-id="609aa-104">Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="609aa-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="609aa-105">Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core MVC przy użyciu programu Entity Framework Core i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="609aa-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="609aa-106">Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](intro.md).</span><span class="sxs-lookup"><span data-stu-id="609aa-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="609aa-107">W samouczku poprzedniej zaimplementowano zestaw stron sieci web dla podstawowych operacji CRUD dla uczniów jednostek.</span><span class="sxs-lookup"><span data-stu-id="609aa-107">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="609aa-108">W tym samouczku zostanie dodany do strony indeksu uczniów lub studentów sortowanie, filtrowanie i funkcje stronicowania.</span><span class="sxs-lookup"><span data-stu-id="609aa-108">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="609aa-109">Utworzysz także strona, która jest prosta grupa.</span><span class="sxs-lookup"><span data-stu-id="609aa-109">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="609aa-110">Na poniższej ilustracji przedstawiono wygląd strony po zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="609aa-110">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="609aa-111">Nagłówki kolumn są łącza, które użytkownik może kliknąć, aby sortować według tej kolumny.</span><span class="sxs-lookup"><span data-stu-id="609aa-111">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="609aa-112">Kliknięcie nagłówka wielokrotnie kolumny przełączanie się między kolumny sortowania.</span><span class="sxs-lookup"><span data-stu-id="609aa-112">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Strona indeksu uczniów lub studentów](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="609aa-114">Dodaj kolumny sortowania łącza do strony indeksu uczniów lub studentów</span><span class="sxs-lookup"><span data-stu-id="609aa-114">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="609aa-115">Aby dodać sortowanie do strony indeksu dla użytkowników domowych, zostanie zmieniona `Index` kontrolera studentów i Dodaj kod do widoku indeksu studenta.</span><span class="sxs-lookup"><span data-stu-id="609aa-115">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="609aa-116">Dodaj sortowania funkcji metody indeksu</span><span class="sxs-lookup"><span data-stu-id="609aa-116">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="609aa-117">W *StudentsController.cs*, Zastąp `Index` metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="609aa-117">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="609aa-118">Ten kod odbiera `sortOrder` parametr ciągu zapytania w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="609aa-118">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="609aa-119">Wartość ciągu kwerendy są udostępniane przez program ASP.NET Core MVC jako parametr do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="609aa-119">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="609aa-120">Parametr będzie ciąg, który jest "Name" lub "Date", opcjonalnie, podkreślenia, a ciąg "desc", aby określić w kolejności malejącej.</span><span class="sxs-lookup"><span data-stu-id="609aa-120">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="609aa-121">Domyślna kolejność sortowania jest rosnąca.</span><span class="sxs-lookup"><span data-stu-id="609aa-121">The default sort order is ascending.</span></span>

<span data-ttu-id="609aa-122">Żądana strona indeksu, po raz pierwszy nie jest Brak ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="609aa-122">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="609aa-123">Studenci są wyświetlane w kolejności rosnącej przez nazwisko, która jest ustawiona domyślnie zgodnie z ustaleniami w przypadku fall-through `switch` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="609aa-123">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="609aa-124">Gdy użytkownik klika hiperłącze nagłówek kolumny, odpowiedni `sortOrder` wartość jest podana w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="609aa-124">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="609aa-125">Dwa `ViewData` elementów (NameSortParm i DateSortParm) są używane przez widok skonfigurowanie hiperłącza nagłówek kolumny z wartości typu QueryString odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="609aa-125">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="609aa-126">Są to trójargumentowy instrukcje.</span><span class="sxs-lookup"><span data-stu-id="609aa-126">These are ternary statements.</span></span> <span data-ttu-id="609aa-127">Pierwsza z nich Określa, że jeśli `sortOrder` parametr ma wartość null lub pusty, NameSortParm powinien być ustawiony na "name_desc"; w przeciwnym razie powinien być ustawiony na pusty ciąg.</span><span class="sxs-lookup"><span data-stu-id="609aa-127">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="609aa-128">Te dwie instrukcje włączyć widok, aby ustawić hiperłącza nagłówek kolumny w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="609aa-128">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="609aa-129">Bieżącej kolejności sortowania</span><span class="sxs-lookup"><span data-stu-id="609aa-129">Current sort order</span></span>  | <span data-ttu-id="609aa-130">Ostatnia nazwa hiperłącza</span><span class="sxs-lookup"><span data-stu-id="609aa-130">Last Name Hyperlink</span></span> | <span data-ttu-id="609aa-131">Data hiperłącza</span><span class="sxs-lookup"><span data-stu-id="609aa-131">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="609aa-132">Ostatni nazwy, rosnąco</span><span class="sxs-lookup"><span data-stu-id="609aa-132">Last Name ascending</span></span>  | <span data-ttu-id="609aa-133">descending</span><span class="sxs-lookup"><span data-stu-id="609aa-133">descending</span></span>          | <span data-ttu-id="609aa-134">ascending</span><span class="sxs-lookup"><span data-stu-id="609aa-134">ascending</span></span>      |
| <span data-ttu-id="609aa-135">Ostatni nazwy, malejąco</span><span class="sxs-lookup"><span data-stu-id="609aa-135">Last Name descending</span></span> | <span data-ttu-id="609aa-136">ascending</span><span class="sxs-lookup"><span data-stu-id="609aa-136">ascending</span></span>           | <span data-ttu-id="609aa-137">ascending</span><span class="sxs-lookup"><span data-stu-id="609aa-137">ascending</span></span>      |
| <span data-ttu-id="609aa-138">Data w kolejności rosnącej</span><span class="sxs-lookup"><span data-stu-id="609aa-138">Date ascending</span></span>       | <span data-ttu-id="609aa-139">ascending</span><span class="sxs-lookup"><span data-stu-id="609aa-139">ascending</span></span>           | <span data-ttu-id="609aa-140">descending</span><span class="sxs-lookup"><span data-stu-id="609aa-140">descending</span></span>     |
| <span data-ttu-id="609aa-141">Data, malejąco</span><span class="sxs-lookup"><span data-stu-id="609aa-141">Date descending</span></span>      | <span data-ttu-id="609aa-142">ascending</span><span class="sxs-lookup"><span data-stu-id="609aa-142">ascending</span></span>           | <span data-ttu-id="609aa-143">ascending</span><span class="sxs-lookup"><span data-stu-id="609aa-143">ascending</span></span>      |

<span data-ttu-id="609aa-144">Metoda używa do składnika LINQ to Entities Określ kolumny, aby posortować według.</span><span class="sxs-lookup"><span data-stu-id="609aa-144">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="609aa-145">Kod tworzy `IQueryable` zmiennej przed instrukcją switch modyfikuje go w instrukcji switch i wywołania `ToListAsync` metody po `switch` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="609aa-145">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="609aa-146">Podczas tworzenia i modyfikowania `IQueryable` zmiennych, nie zapytanie jest wysyłane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="609aa-146">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="609aa-147">Zapytanie nie jest wykonywany do momentu konwersji `IQueryable` obiektu do kolekcji, wywołując metodę, takich jak `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="609aa-147">The query isn't executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="609aa-148">W związku z tym powoduje ten kod w jednym zapytaniu, która nie jest wykonywana do czasu `return View` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="609aa-148">Therefore, this code results in a single query that's not executed until the `return View` statement.</span></span>

<span data-ttu-id="609aa-149">Ten kod może pobrać verbose z dużą liczbą kolumn.</span><span class="sxs-lookup"><span data-stu-id="609aa-149">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="609aa-150">[Ostatni samouczku tej serii](advanced.md#dynamic-linq) pokazano, jak napisać kod, który umożliwia przekazywania nazwy `OrderBy` kolumny w zmiennej ciągu.</span><span class="sxs-lookup"><span data-stu-id="609aa-150">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="609aa-151">Dodawanie hiperłączy nagłówek kolumny do widoku indeksu dla użytkowników domowych</span><span class="sxs-lookup"><span data-stu-id="609aa-151">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="609aa-152">Zastąp kod w *Views/Students/Index.cshtml*, z następujący kod, aby dodać hiperłącza nagłówka kolumny.</span><span class="sxs-lookup"><span data-stu-id="609aa-152">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="609aa-153">Wyróżniono zmienionych wierszy.</span><span class="sxs-lookup"><span data-stu-id="609aa-153">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="609aa-154">Ten kod używa tych informacji w `ViewData` właściwości do skonfigurowania hiperłącza z zapytaniem odpowiednie ciągu wartości.</span><span class="sxs-lookup"><span data-stu-id="609aa-154">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="609aa-155">Uruchom aplikację, wybierz **studentów** , a następnie kliknij pozycję **nazwisko** i **Data rejestracji** działa nagłówki kolumn, aby zweryfikować, że sortowanie.</span><span class="sxs-lookup"><span data-stu-id="609aa-155">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Strona indeksu studentów według nazwy](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="609aa-157">Dodaj pole wyszukiwania do strony indeksu uczniów lub studentów</span><span class="sxs-lookup"><span data-stu-id="609aa-157">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="609aa-158">Aby dodać filtrowanie do strony indeksu studentów, będzie Dodaj pole tekstowe i przycisk Prześlij do widoku i wprowadzić odpowiednie zmiany w `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="609aa-158">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="609aa-159">Pole tekstowe umożliwi wprowadź ciąg do wyszukania w imię pola imienia i nazwiska.</span><span class="sxs-lookup"><span data-stu-id="609aa-159">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="609aa-160">Dodawanie funkcji filtrowania do Index — metoda</span><span class="sxs-lookup"><span data-stu-id="609aa-160">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="609aa-161">W *StudentsController.cs*, Zastąp `Index` metodę z następującym kodem (zmiany zostały wyróżnione).</span><span class="sxs-lookup"><span data-stu-id="609aa-161">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="609aa-162">Dodano `searchString` parametr `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="609aa-162">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="609aa-163">Wartość ciągu wyszukiwania są odebrane z pola tekstowego, która zostanie dodana do widoku indeksu.</span><span class="sxs-lookup"><span data-stu-id="609aa-163">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="609aa-164">Również dodane do instrukcji LINQ where klauzuli, który wybiera tylko studentów, w których imię lub nazwisko zawiera ciąg wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="609aa-164">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="609aa-165">Instrukcja, która dodaje where klauzuli jest wykonywane tylko wtedy, gdy wartość do wyszukania.</span><span class="sxs-lookup"><span data-stu-id="609aa-165">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="609aa-166">W tym miejscu są nawiązywane `Where` metoda `IQueryable` obiektu i filtr będą przetwarzane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="609aa-166">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="609aa-167">W niektórych scenariuszach może być wywołanie `Where` metody jako metodę rozszerzenie w kolekcji w pamięci.</span><span class="sxs-lookup"><span data-stu-id="609aa-167">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="609aa-168">(Na przykład, załóżmy, że możesz zmienić odwołanie do `_context.Students` tak to zamiast elementu EF `DbSet` odwołuje się do metody repozytorium, która zwraca `IEnumerable` kolekcji.) Wynik zazwyczaj będzie taki sam, ale w niektórych przypadkach może się różnić.</span><span class="sxs-lookup"><span data-stu-id="609aa-168">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="609aa-169">Na przykład, .NET Framework wykonania `Contains` metoda przeprowadza porównanie z uwzględnieniem wielkości liter domyślnie, ale w programie SQL Server jest to określane przez ustawienie sortowania wystąpienia programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="609aa-169">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="609aa-170">To ustawienie domyślne pozycji bez uwzględniania wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="609aa-170">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="609aa-171">Można wywołać `ToUpper` metody, aby jawnie bez uwzględniania wielkości liter testu: *gdzie (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*.</span><span class="sxs-lookup"><span data-stu-id="609aa-171">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="609aa-172">Które zapewniają, że wyniki nie zmieniają się po zmianie kod później do korzystania z repozytorium, która zwraca `IEnumerable` kolekcji zamiast `IQueryable` obiektu.</span><span class="sxs-lookup"><span data-stu-id="609aa-172">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="609aa-173">(Podczas wywoływania `Contains` metoda `IEnumerable` kolekcji, możesz pobrać wdrożenia programu .NET Framework; podczas wywoływania go na `IQueryable` obiektu, możesz uzyskać implementacji dostawcy bazy danych.) Istnieje jednak zmniejszenie wydajności dla tego rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="609aa-173">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there's a performance penalty for this solution.</span></span> <span data-ttu-id="609aa-174">`ToUpper` Kod spowodowałaby funkcji w klauzuli WHERE instrukcji TSQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="609aa-174">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="609aa-175">Które uniemożliwiłyby Optymalizator przy użyciu indeksu.</span><span class="sxs-lookup"><span data-stu-id="609aa-175">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="609aa-176">Biorąc pod uwagę, że program SQL jest zainstalowany przede wszystkim jako bez uwzględniania wielkości liter, jest unikanie `ToUpper` kodu do momentu migracji w magazynie danych z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="609aa-176">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="609aa-177">Dodaj pole wyszukiwania do widoku indeksu dla użytkowników domowych</span><span class="sxs-lookup"><span data-stu-id="609aa-177">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="609aa-178">W *Views/Student/Index.cshtml*, Dodaj wyróżniony kod bezpośrednio przed otwarcia tag tabeli, aby można było utworzyć podpis pola tekstowego, a **wyszukiwania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="609aa-178">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="609aa-179">Ten kod zawiera `<form>` [pomocnika tagów](xref:mvc/views/tag-helpers/intro) można dodać pola tekstowego wyszukiwania i przycisku.</span><span class="sxs-lookup"><span data-stu-id="609aa-179">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="609aa-180">Domyślnie `<form>` pomocnika tagów przesyła dane formularza przy użyciu metody POST, co oznacza, że parametry są przekazywane w treści wiadomości HTTP, a nie w adresie URL jako ciągi zapytań.</span><span class="sxs-lookup"><span data-stu-id="609aa-180">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="609aa-181">Po określeniu HTTP GET, formularza dane są przekazywane w adresie URL jako ciągi zapytania, który umożliwia użytkownikom zakładki adres URL.</span><span class="sxs-lookup"><span data-stu-id="609aa-181">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="609aa-182">Akcja nie powoduje aktualizacji UZYSKAĆ zalecane wytyczne W3C, która powinna być używana.</span><span class="sxs-lookup"><span data-stu-id="609aa-182">The W3C guidelines recommend that you should use GET when the action doesn't result in an update.</span></span>

<span data-ttu-id="609aa-183">Uruchom aplikację, wybierz **studentów** karcie, wprowadź ciąg wyszukiwania, a następnie kliknij przycisk Wyszukaj, aby zweryfikować filtrowania.</span><span class="sxs-lookup"><span data-stu-id="609aa-183">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![Strona indeksu studentów z filtrowania](sort-filter-page/_static/filtering.png)

<span data-ttu-id="609aa-185">Zwróć uwagę, że adres URL zawiera ciąg wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="609aa-185">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="609aa-186">Jeśli Oznacz tę stronę zakładką uzyskasz wyfiltrowanej listy używania zakładki.</span><span class="sxs-lookup"><span data-stu-id="609aa-186">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="609aa-187">Dodawanie `method="get"` do `form` tag jest przyczyn ciąg zapytania do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="609aa-187">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="609aa-188">Na tym etapie po kliknięciu łącza sortowania nagłówka kolumny zostanie utracony wartości filtru, wprowadzony w **wyszukiwania** pole.</span><span class="sxs-lookup"><span data-stu-id="609aa-188">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="609aa-189">Użytkownik będzie rozwiązać ten problem w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="609aa-189">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="609aa-190">Dodawanie funkcji stronicowania do strony indeksu uczniów lub studentów</span><span class="sxs-lookup"><span data-stu-id="609aa-190">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="609aa-191">Aby dodać stronicowania do strony indeksu studentów, należy utworzyć `PaginatedList` klasy, która używa `Skip` i `Take` instrukcje, aby filtrować dane na serwerze zamiast zawsze pobierania wszystkie wiersze w tabeli.</span><span class="sxs-lookup"><span data-stu-id="609aa-191">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="609aa-192">Następnie będzie można wprowadzić dodatkowe zmiany w `Index` — metoda i dodać przyciski stronicowania `Index` widoku.</span><span class="sxs-lookup"><span data-stu-id="609aa-192">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="609aa-193">Na poniższej ilustracji przedstawiono przyciski stronicowania.</span><span class="sxs-lookup"><span data-stu-id="609aa-193">The following illustration shows the paging buttons.</span></span>

![Strona indeksu studentów z łączami stronicowania](sort-filter-page/_static/paging.png)

<span data-ttu-id="609aa-195">W folderze projektu Utwórz `PaginatedList.cs`, a następnie Zastąp kod szablonu z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="609aa-195">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="609aa-196">`CreateAsync` Metoda ten kod pobiera rozmiar strony i numer strony i zastosowanie odpowiednich `Skip` i `Take` instrukcje `IQueryable`.</span><span class="sxs-lookup"><span data-stu-id="609aa-196">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="609aa-197">Gdy `ToListAsync` jest wywoływana na `IQueryable`, to zostanie zwrócona lista zawierająca żądanej strony.</span><span class="sxs-lookup"><span data-stu-id="609aa-197">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="609aa-198">Właściwości `HasPreviousPage` i `HasNextPage` pozwala włączyć lub wyłączyć **Wstecz** i **dalej** stronicowania przycisków.</span><span class="sxs-lookup"><span data-stu-id="609aa-198">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="609aa-199">A `CreateAsync` zamiast konstruktora metodę, aby utworzyć `PaginatedList<T>` obiektu, ponieważ konstruktorów nie można uruchomić kod asynchroniczny.</span><span class="sxs-lookup"><span data-stu-id="609aa-199">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="609aa-200">Dodawanie funkcji stronicowania do Index — metoda</span><span class="sxs-lookup"><span data-stu-id="609aa-200">Add paging functionality to the Index method</span></span>

<span data-ttu-id="609aa-201">W *StudentsController.cs*, Zastąp `Index` metodę z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="609aa-201">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="609aa-202">Ten kod dodaje parametr numer strony, bieżącego parametru kolejność sortowania i bieżącego parametru filtru w podpisie metody.</span><span class="sxs-lookup"><span data-stu-id="609aa-202">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="609aa-203">Po raz pierwszy, ta strona jest wyświetlana, lub jeśli użytkownik nie kliknął stronicowania i sortowania łącza, wszystkie parametry będzie miała wartość null.</span><span class="sxs-lookup"><span data-stu-id="609aa-203">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="609aa-204">Po kliknięciu łącza stronicowania zmienną strony będzie zawierać numer strony do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="609aa-204">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="609aa-205">`ViewData` Elementu o nazwie CurrentSort zapewnia widok z bieżącej kolejności sortowania, ponieważ to musi być uwzględniona w łącza stronicowania aby zapewnić porządek sortowania, taka sama podczas stronicowania.</span><span class="sxs-lookup"><span data-stu-id="609aa-205">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="609aa-206">`ViewData` Elementu o nazwie BieżącyFiltr zawiera widok z bieżącym ciąg filtru.</span><span class="sxs-lookup"><span data-stu-id="609aa-206">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="609aa-207">Ta wartość musi być uwzględniona w łącza stronicowania w celu utrzymania ustawień filtru podczas stronicowania i go muszą być przywrócone do pola tekstowego, gdy strona zostanie wyświetlony ponownie.</span><span class="sxs-lookup"><span data-stu-id="609aa-207">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="609aa-208">Jeśli ciąg wyszukiwania została zmieniona podczas stronicowania, strona musi zresetowana do wartości 1, nowy filtr może powodować różnych danych do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="609aa-208">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="609aa-209">Ciąg wyszukiwania jest zmieniany, gdy wartość jest wprowadzana w polu tekstowym i kliknięciu przycisku Prześlij.</span><span class="sxs-lookup"><span data-stu-id="609aa-209">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="609aa-210">W takim przypadku `searchString` parametru nie ma wartości null.</span><span class="sxs-lookup"><span data-stu-id="609aa-210">In that case, the `searchString` parameter isn't null.</span></span>

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

<span data-ttu-id="609aa-211">Na koniec `Index` metody `PaginatedList.CreateAsync` metoda konwertuje zapytania uczniów do pojedynczej strony studentów w typ kolekcji, który obsługuje stronicowanie.</span><span class="sxs-lookup"><span data-stu-id="609aa-211">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="609aa-212">Tej stronie studentów są następnie przekazywane do widoku.</span><span class="sxs-lookup"><span data-stu-id="609aa-212">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="609aa-213">`PaginatedList.CreateAsync` Metoda przyjmuje numer strony.</span><span class="sxs-lookup"><span data-stu-id="609aa-213">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="609aa-214">Dwa znaki zapytania reprezentowania operatora łączenia wartości null.</span><span class="sxs-lookup"><span data-stu-id="609aa-214">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="609aa-215">Wartość domyślna dla typu dopuszczającego wartość null; definiuje operator łączenia wartości null wyrażenie `(page ?? 1)` oznacza zwrócić wartość `page` jeśli jego wartość, lub zwraca 1, jeśli `page` ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="609aa-215">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="609aa-216">Dodawanie łączy stronicowania w widoku indeksu dla użytkowników domowych</span><span class="sxs-lookup"><span data-stu-id="609aa-216">Add paging links to the Student Index view</span></span>

<span data-ttu-id="609aa-217">W *Views/Students/Index.cshtml*, Zastąp istniejący kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="609aa-217">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="609aa-218">Zmiany zostały wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="609aa-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="609aa-219">`@model` Instrukcji w górnej części strony określa, czy widok teraz pobiera `PaginatedList<T>` obiekt zamiast `List<T>` obiektu.</span><span class="sxs-lookup"><span data-stu-id="609aa-219">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="609aa-220">Łącza nagłówka kolumny Użyj ciągu zapytania do przekazania do kontrolera aktualnie wyszukiwanego ciągu, dzięki czemu użytkownik może sortować w wynikach filtrowania:</span><span class="sxs-lookup"><span data-stu-id="609aa-220">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="609aa-221">Przyciski stronicowania są wyświetlane przez pomocników tagów:</span><span class="sxs-lookup"><span data-stu-id="609aa-221">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="609aa-222">Uruchom aplikację i przejdź do strony studenta.</span><span class="sxs-lookup"><span data-stu-id="609aa-222">Run the app and go to the Students page.</span></span>

![Strona indeksu studentów z łączami stronicowania](sort-filter-page/_static/paging.png)

<span data-ttu-id="609aa-224">Kliknij łącza stronicowania w różnych sortowania, aby upewnić się, że działa stronicowania.</span><span class="sxs-lookup"><span data-stu-id="609aa-224">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="609aa-225">Następnie wprowadź ciąg wyszukiwania i spróbuj stronicowania ponownie, aby sprawdzić, czy stronicowania również działa poprawnie z sortowania i filtrowania.</span><span class="sxs-lookup"><span data-stu-id="609aa-225">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="609aa-226">Tworzenie strony informacje, które znajdują się dane statystyczne uczniów</span><span class="sxs-lookup"><span data-stu-id="609aa-226">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="609aa-227">Dla witryny internetowej firmy Contoso University **o** strony, będzie wyświetlane, jak wiele studentów zostały zarejestrowane dla każdego dnia rejestracji.</span><span class="sxs-lookup"><span data-stu-id="609aa-227">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="609aa-228">Wymaga to prosty i grupowania obliczeń grup.</span><span class="sxs-lookup"><span data-stu-id="609aa-228">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="609aa-229">W tym celu będzie wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="609aa-229">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="609aa-230">Utwórz klasę modelu widoku danych, która należy do przekazania do widoku.</span><span class="sxs-lookup"><span data-stu-id="609aa-230">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="609aa-231">Zmodyfikuj metodę informacje w kontrolerze głównej.</span><span class="sxs-lookup"><span data-stu-id="609aa-231">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="609aa-232">Zmodyfikuj widok informacje.</span><span class="sxs-lookup"><span data-stu-id="609aa-232">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="609aa-233">Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="609aa-233">Create the view model</span></span>

<span data-ttu-id="609aa-234">Utwórz *SchoolViewModels* folderu w *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="609aa-234">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="609aa-235">W nowym folderze, Dodaj plik klasy *EnrollmentDateGroup.cs* i Zastąp kod szablonu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="609aa-235">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="609aa-236">Modyfikowanie macierzystego kontrolera</span><span class="sxs-lookup"><span data-stu-id="609aa-236">Modify the Home Controller</span></span>

<span data-ttu-id="609aa-237">W *HomeController.cs*, Dodaj następujące instrukcje using na początku pliku:</span><span class="sxs-lookup"><span data-stu-id="609aa-237">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="609aa-238">Dodaj zmienną klasy kontekstu bazy danych bezpośrednio po otwierającym nawiasie klamrowym klasy i pobrać wystąpienia kontekstu z platformy ASP.NET Core Podpisane.</span><span class="sxs-lookup"><span data-stu-id="609aa-238">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="609aa-239">Zastąp `About` metodę z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="609aa-239">Replace the `About` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="609aa-240">Instrukcja LINQ grupy jednostek uczniowie według daty rejestracji oblicza liczbę jednostek w każdej grupie i przechowuje wyniki w kolekcji z `EnrollmentDateGroup` wyświetlić obiekty modelu.</span><span class="sxs-lookup"><span data-stu-id="609aa-240">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE] 
> <span data-ttu-id="609aa-241">W wersji 1.0 programu Entity Framework Core cały zestaw wyników jest zwracana do klienta, a operacji grupowania na kliencie.</span><span class="sxs-lookup"><span data-stu-id="609aa-241">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="609aa-242">W niektórych scenariuszach to można utworzyć problemy z wydajnością.</span><span class="sxs-lookup"><span data-stu-id="609aa-242">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="609aa-243">Pamiętaj testu wydajności przy użyciu produkcyjnego ilości danych, a w razie potrzeby umożliwia wykonaj grupowanie na serwerze raw SQL.</span><span class="sxs-lookup"><span data-stu-id="609aa-243">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="609aa-244">Informacje o sposobie używania pierwotnych SQL, zobacz [ostatniego samouczku tej serii](advanced.md).</span><span class="sxs-lookup"><span data-stu-id="609aa-244">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="609aa-245">Zmodyfikuj widok — informacje</span><span class="sxs-lookup"><span data-stu-id="609aa-245">Modify the About View</span></span>

<span data-ttu-id="609aa-246">Zastąp kod w *Views/Home/About.cshtml* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="609aa-246">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="609aa-247">Uruchom aplikację i przejdź do strony informacje.</span><span class="sxs-lookup"><span data-stu-id="609aa-247">Run the app and go to the About page.</span></span> <span data-ttu-id="609aa-248">Liczba studentów dla każdego dnia rejestracji jest wyświetlane w formie tabeli.</span><span class="sxs-lookup"><span data-stu-id="609aa-248">The count of students for each enrollment date is displayed in a table.</span></span>

![Strona — informacje](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="609aa-250">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="609aa-250">Summary</span></span>

<span data-ttu-id="609aa-251">W tym samouczku przedstawiono sposób wykonywania sortowanie, filtrowanie, stronicowania i grupowania.</span><span class="sxs-lookup"><span data-stu-id="609aa-251">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="609aa-252">W następnym samouczku nauczysz się, jak obsługiwać zmiany modelu danych przy użyciu migracji.</span><span class="sxs-lookup"><span data-stu-id="609aa-252">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="609aa-253">[Poprzednie](crud.md)
> [dalej](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="609aa-253">[Previous](crud.md)
[Next](migrations.md)</span></span>  
