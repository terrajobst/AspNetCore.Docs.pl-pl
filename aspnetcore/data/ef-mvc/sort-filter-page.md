---
title: 'Samouczek: Dodaj sortowanie, filtrowanie i stronicowanie — ASP.NET MVC z programem EF Core'
description: W tym samouczku dodasz sortowanie, filtrowanie i stronicowanie funkcji do strony indeksu studentów. Utworzysz też strony, która wykonuje prostą grupowania.
author: rick-anderson
ms.author: tdykstra
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 921e27bf56587813f835357c9090c91a155c087b
ms.sourcegitcommit: b508b115107e0f8d7f62b25cfcc8ad45e1373459
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65212550"
---
# <a name="tutorial-add-sorting-filtering-and-paging---aspnet-mvc-with-ef-core"></a><span data-ttu-id="12a98-104">Samouczek: Dodaj sortowanie, filtrowanie i stronicowanie — ASP.NET MVC z programem EF Core</span><span class="sxs-lookup"><span data-stu-id="12a98-104">Tutorial: Add sorting, filtering, and paging - ASP.NET MVC with EF Core</span></span>

<span data-ttu-id="12a98-105">W poprzednim samouczku wdrożono zestaw stron sieci web w przypadku podstawowych operacji CRUD dla jednostek dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="12a98-105">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="12a98-106">W tym samouczku dodasz sortowanie, filtrowanie i stronicowanie funkcji do strony indeksu studentów.</span><span class="sxs-lookup"><span data-stu-id="12a98-106">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="12a98-107">Utworzysz też strony, która wykonuje prostą grupowania.</span><span class="sxs-lookup"><span data-stu-id="12a98-107">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="12a98-108">Na poniższej ilustracji przedstawiono wygląd strony po zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="12a98-108">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="12a98-109">Nagłówki kolumn odnoszą się łącza, które użytkownik może kliknąć, aby posortować według tej kolumny.</span><span class="sxs-lookup"><span data-stu-id="12a98-109">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="12a98-110">Klikając kolumnę nagłówka wielokrotnie przełącza między malejącą i rosnącą porządek sortowania.</span><span class="sxs-lookup"><span data-stu-id="12a98-110">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Strona indeksu uczniów](sort-filter-page/_static/paging.png)

<span data-ttu-id="12a98-112">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="12a98-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12a98-113">Dodaj kolumnę sortowania łącza</span><span class="sxs-lookup"><span data-stu-id="12a98-113">Add column sort links</span></span>
> * <span data-ttu-id="12a98-114">Dodawanie pola wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="12a98-114">Add a Search box</span></span>
> * <span data-ttu-id="12a98-115">Dodawanie stronicowania do indeksu uczniów</span><span class="sxs-lookup"><span data-stu-id="12a98-115">Add paging to Students Index</span></span>
> * <span data-ttu-id="12a98-116">Dodawanie stronicowania do Index — metoda</span><span class="sxs-lookup"><span data-stu-id="12a98-116">Add paging to Index method</span></span>
> * <span data-ttu-id="12a98-117">Dodawanie łączy stronicowania</span><span class="sxs-lookup"><span data-stu-id="12a98-117">Add paging links</span></span>
> * <span data-ttu-id="12a98-118">Tworzenie strony informacje</span><span class="sxs-lookup"><span data-stu-id="12a98-118">Create an About page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12a98-119">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="12a98-119">Prerequisites</span></span>

* [<span data-ttu-id="12a98-120">Implementowanie funkcji CRUD</span><span class="sxs-lookup"><span data-stu-id="12a98-120">Implement CRUD Functionality</span></span>](crud.md)

## <a name="add-column-sort-links"></a><span data-ttu-id="12a98-121">Dodaj kolumnę sortowania łącza</span><span class="sxs-lookup"><span data-stu-id="12a98-121">Add column sort links</span></span>

<span data-ttu-id="12a98-122">Aby dodać sortowanie do strony indeksu dla uczniów, zmienisz `Index` metody kontrolera studentów i Dodaj kod do widoku indeksu dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="12a98-122">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="12a98-123">Dodaj sortowanie funkcjonalność do Index — metoda</span><span class="sxs-lookup"><span data-stu-id="12a98-123">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="12a98-124">W *StudentsController.cs*, Zastąp `Index` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="12a98-124">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="12a98-125">Ten kod odbiera `sortOrder` parametr ciągu zapytania w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="12a98-125">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="12a98-126">Wartość ciągu zapytania jest dostarczane przez platformę ASP.NET Core MVC jako parametr do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="12a98-126">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="12a98-127">Parametr będzie ciąg, który jest "Name" lub "Date", opcjonalnie, podkreślenia i ciąg "desc", aby określić w kolejności malejącej.</span><span class="sxs-lookup"><span data-stu-id="12a98-127">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="12a98-128">Domyślna kolejność sortowania jest rosnąca.</span><span class="sxs-lookup"><span data-stu-id="12a98-128">The default sort order is ascending.</span></span>

<span data-ttu-id="12a98-129">Przy pierwszym żądaniu strony indeksu nie jest Brak ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="12a98-129">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="12a98-130">Studenci są wyświetlane w kolejności rosnącej według nazwiska, co jest ustawieniem domyślnym, zgodnie z ustaleniami w należą do przypadku `switch` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="12a98-130">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="12a98-131">Kiedy użytkownik kliknie hiperlink nagłówek kolumny, odpowiednie `sortOrder` podana w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="12a98-131">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="12a98-132">Dwa `ViewData` do konfigurowania hiperłącza nagłówek kolumny przy użyciu wartości ciągu zapytania odpowiednie elementy (NameSortParm i DateSortParm) są używane w widoku.</span><span class="sxs-lookup"><span data-stu-id="12a98-132">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="12a98-133">Są to trójargumentowy instrukcji.</span><span class="sxs-lookup"><span data-stu-id="12a98-133">These are ternary statements.</span></span> <span data-ttu-id="12a98-134">Pierwsza z nich Określa, że jeśli `sortOrder` parametr ma wartość null lub pusty, NameSortParm powinna być równa "name_desc"; w przeciwnym razie powinien być ustawiony na pusty ciąg.</span><span class="sxs-lookup"><span data-stu-id="12a98-134">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="12a98-135">Te dwie instrukcje Włącz widok, aby ustawić hiperłącza nagłówek kolumny w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="12a98-135">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="12a98-136">Bieżący kolejność sortowania</span><span class="sxs-lookup"><span data-stu-id="12a98-136">Current sort order</span></span>  | <span data-ttu-id="12a98-137">Ostatnia nazwa hiperłącza</span><span class="sxs-lookup"><span data-stu-id="12a98-137">Last Name Hyperlink</span></span> | <span data-ttu-id="12a98-138">Data hiperłącza</span><span class="sxs-lookup"><span data-stu-id="12a98-138">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="12a98-139">Ostatnie nazwy, rosnąco</span><span class="sxs-lookup"><span data-stu-id="12a98-139">Last Name ascending</span></span>  | <span data-ttu-id="12a98-140">descending</span><span class="sxs-lookup"><span data-stu-id="12a98-140">descending</span></span>          | <span data-ttu-id="12a98-141">ascending</span><span class="sxs-lookup"><span data-stu-id="12a98-141">ascending</span></span>      |
| <span data-ttu-id="12a98-142">Ostatnie nazwy, malejąco</span><span class="sxs-lookup"><span data-stu-id="12a98-142">Last Name descending</span></span> | <span data-ttu-id="12a98-143">ascending</span><span class="sxs-lookup"><span data-stu-id="12a98-143">ascending</span></span>           | <span data-ttu-id="12a98-144">ascending</span><span class="sxs-lookup"><span data-stu-id="12a98-144">ascending</span></span>      |
| <span data-ttu-id="12a98-145">Data, w kolejności rosnącej</span><span class="sxs-lookup"><span data-stu-id="12a98-145">Date ascending</span></span>       | <span data-ttu-id="12a98-146">ascending</span><span class="sxs-lookup"><span data-stu-id="12a98-146">ascending</span></span>           | <span data-ttu-id="12a98-147">descending</span><span class="sxs-lookup"><span data-stu-id="12a98-147">descending</span></span>     |
| <span data-ttu-id="12a98-148">Data, malejąco</span><span class="sxs-lookup"><span data-stu-id="12a98-148">Date descending</span></span>      | <span data-ttu-id="12a98-149">ascending</span><span class="sxs-lookup"><span data-stu-id="12a98-149">ascending</span></span>           | <span data-ttu-id="12a98-150">ascending</span><span class="sxs-lookup"><span data-stu-id="12a98-150">ascending</span></span>      |

<span data-ttu-id="12a98-151">Metoda używa składnik LINQ to Entities, aby określić kolumnę sortowania.</span><span class="sxs-lookup"><span data-stu-id="12a98-151">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="12a98-152">Ten kod tworzy `IQueryable` zmiennej przed instrukcją switch modyfikuje go w instrukcji switch i wywołania `ToListAsync` metody `switch` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="12a98-152">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="12a98-153">Podczas tworzenia i modyfikowania `IQueryable` zmiennych, bez określenia zapytania są wysyłane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="12a98-153">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="12a98-154">Zapytanie nie jest wykonywane, dopóki konwersji `IQueryable` obiektu w kolekcji przez wywołanie metody, takie jak `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="12a98-154">The query isn't executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="12a98-155">W związku z tym, ten kod powoduje pojedyncze zapytanie, które nie jest wykonywane, dopóki `return View` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="12a98-155">Therefore, this code results in a single query that's not executed until the `return View` statement.</span></span>

<span data-ttu-id="12a98-156">Ten kod można pobrać pełny z dużą liczbą kolumn.</span><span class="sxs-lookup"><span data-stu-id="12a98-156">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="12a98-157">[Ostatni samouczek z tej serii](advanced.md#dynamic-linq) pokazuje, jak napisać kod, który pozwala przekazywać nazwę jednostki `OrderBy` kolumny w zmiennej ciągu.</span><span class="sxs-lookup"><span data-stu-id="12a98-157">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="12a98-158">Dodawanie hiperłączy nagłówek kolumny do widoku indeksu dla uczniów</span><span class="sxs-lookup"><span data-stu-id="12a98-158">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="12a98-159">Zastąp kod w *Views/Students/Index.cshtml*, z następującym kodem, aby dodać hiperlinki nagłówek kolumny.</span><span class="sxs-lookup"><span data-stu-id="12a98-159">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="12a98-160">Zmienione wiersze są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="12a98-160">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="12a98-161">Ten kod używa tych informacji w `ViewData` właściwości, aby skonfigurować hiperlinki z zapytaniem odpowiednie wartości ciągu.</span><span class="sxs-lookup"><span data-stu-id="12a98-161">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="12a98-162">Uruchom aplikację, wybierz **studentów** kartę, a następnie kliknij przycisk **nazwisko** i **Data rejestracji** nagłówków kolumn, aby zweryfikować, że sortowanie działa.</span><span class="sxs-lookup"><span data-stu-id="12a98-162">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Strona indeksu studentów według nazwy](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box"></a><span data-ttu-id="12a98-164">Dodawanie pola wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="12a98-164">Add a Search box</span></span>

<span data-ttu-id="12a98-165">Aby dodać filtrowanie do strony indeksu studentów, dodasz pole tekstowe i przycisk Prześlij do widoku i wprowadzić odpowiednie zmiany w `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="12a98-165">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="12a98-166">Pole tekstowe umożliwi wprowadź ciąg do wyszukania w imię pola imienia i nazwiska.</span><span class="sxs-lookup"><span data-stu-id="12a98-166">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="12a98-167">Dodawanie funkcji filtrowania do Index — metoda</span><span class="sxs-lookup"><span data-stu-id="12a98-167">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="12a98-168">W *StudentsController.cs*, Zastąp `Index` metoda następującym kodem (zmiany zostały wyróżnione).</span><span class="sxs-lookup"><span data-stu-id="12a98-168">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="12a98-169">Po dodaniu `searchString` parametr `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="12a98-169">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="12a98-170">Wartość ciągu wyszukiwania są odebrane z pola tekstowego, które należy dodać do widoku indeksu.</span><span class="sxs-lookup"><span data-stu-id="12a98-170">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="12a98-171">Możesz również dodane do instrukcji LINQ where — klauzula, który wybiera tylko uczniowie, w których imię lub nazwisko zawiera ciąg wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="12a98-171">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="12a98-172">Instrukcja, która dodaje where — klauzula jest wykonywany tylko wtedy, gdy wartość do wyszukania.</span><span class="sxs-lookup"><span data-stu-id="12a98-172">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="12a98-173">W tym miejscu wywołujesz `Where` metody `IQueryable` obiektu i filtr będą przetwarzane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="12a98-173">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="12a98-174">W niektórych scenariuszach może być wywołanie `Where` metodę jako metodę rozszerzenia w kolekcji w pamięci.</span><span class="sxs-lookup"><span data-stu-id="12a98-174">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="12a98-175">(Na przykład, załóżmy, że możesz zmienić odwołanie do `_context.Students` tak, to zamiast elementu EF `DbSet` odwołuje się do metody repozytorium, która zwraca `IEnumerable` kolekcji.) Wynik będzie zazwyczaj taki sam, ale w niektórych przypadkach może być inna.</span><span class="sxs-lookup"><span data-stu-id="12a98-175">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="12a98-176">Na przykład implementacji .NET Framework z `Contains` metoda wykonuje porównania uwzględniającego wielkość liter, domyślnie, ale w programie SQL Server jest to określane przez ustawienia sortowania wystąpienia programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="12a98-176">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="12a98-177">To ustawienie domyślne pozycji bez uwzględniania wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="12a98-177">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="12a98-178">Można wywołać `ToUpper` metody testu jawnie bez uwzględniania wielkości liter:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span><span class="sxs-lookup"><span data-stu-id="12a98-178">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="12a98-179">Które zapewniają, że wyniki pozostają takie same, w przypadku zmiany kodu później, aby korzystać z repozytorium, które zwraca `IEnumerable` zbiór zamiast `IQueryable` obiektu.</span><span class="sxs-lookup"><span data-stu-id="12a98-179">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="12a98-180">(Gdy wywołujesz `Contains` metody `IEnumerable` kolekcji, możesz pobrać wdrożenia programu .NET Framework; Jeśli wywołasz ją na `IQueryable` obiektu, możesz uzyskać implementację dostawcy bazy danych.) Jednak jest zmniejszenie wydajności dla tego rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="12a98-180">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there's a performance penalty for this solution.</span></span> <span data-ttu-id="12a98-181">`ToUpper` Kodu umieścić funkcję w klauzuli WHERE w instrukcji TSQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="12a98-181">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="12a98-182">Które uniemożliwiłyby Optymalizator przy użyciu indeksu.</span><span class="sxs-lookup"><span data-stu-id="12a98-182">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="12a98-183">Biorąc pod uwagę, że program SQL przede wszystkim jest zainstalowany jako bez uwzględniania wielkości liter, zaleca się unikać `ToUpper` kodu do momentu migracji do magazynu danych z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="12a98-183">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="12a98-184">Dodaj pole wyszukiwania do widoku indeksu dla uczniów</span><span class="sxs-lookup"><span data-stu-id="12a98-184">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="12a98-185">W *Views/Student/Index.cshtml*, Dodaj wyróżniony kod bezpośrednio przed wykonaniem otwarcia tag tabeli, aby można było utworzyć podpis, pola tekstowego, a **wyszukiwania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="12a98-185">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="12a98-186">Ten kod używa `<form>` [pomocnika tagów](xref:mvc/views/tag-helpers/intro) można dodać pole tekstowe wyszukiwania i przycisku.</span><span class="sxs-lookup"><span data-stu-id="12a98-186">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="12a98-187">Domyślnie `<form>` Pomocnik tagu przesyła dane formularza z WPISEM, co oznacza, że parametry są przekazywane w treści komunikatu HTTP, a nie w adresie URL jako ciągi zapytań.</span><span class="sxs-lookup"><span data-stu-id="12a98-187">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="12a98-188">Po określeniu HTTP GET, dane formularza jest przekazywany w adresie URL jako ciągi zapytań, która umożliwia użytkownikom utworzyć zakładkę ma adres URL.</span><span class="sxs-lookup"><span data-stu-id="12a98-188">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="12a98-189">Zaleca się wytyczne dotyczące W3C, która powinna być używana korzystać z niezrównanej akcja nie powoduje aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="12a98-189">The W3C guidelines recommend that you should use GET when the action doesn't result in an update.</span></span>

<span data-ttu-id="12a98-190">Uruchom aplikację, wybierz **studentów** kartę, wprowadź ciąg wyszukiwania i kliknij przycisk Wyszukaj, aby sprawdzić, czy działa filtrowanie.</span><span class="sxs-lookup"><span data-stu-id="12a98-190">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![Strona indeksu studentów z filtrowaniem](sort-filter-page/_static/filtering.png)

<span data-ttu-id="12a98-192">Należy zauważyć, że adres URL zawiera ciąg wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="12a98-192">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="12a98-193">Jeśli Oznacz tę stronę zakładką uzyskasz filtrowana lista korzystając z zakładki.</span><span class="sxs-lookup"><span data-stu-id="12a98-193">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="12a98-194">Dodawanie `method="get"` do `form` tag jest, co spowodowało ciąg zapytania do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="12a98-194">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="12a98-195">Na tym etapie, po kliknięciu łącza sortowania nagłówka kolumny utracisz wartość filtru, które wprowadziłeś w **wyszukiwania** pole.</span><span class="sxs-lookup"><span data-stu-id="12a98-195">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="12a98-196">Można to naprawić w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="12a98-196">You'll fix that in the next section.</span></span>

## <a name="add-paging-to-students-index"></a><span data-ttu-id="12a98-197">Dodawanie stronicowania do indeksu uczniów</span><span class="sxs-lookup"><span data-stu-id="12a98-197">Add paging to Students Index</span></span>

<span data-ttu-id="12a98-198">Aby dodać stronicowania do strony indeksu uczniów, należy utworzyć `PaginatedList` klasy, która używa `Skip` i `Take` instrukcje, aby filtrować dane na serwerze zamiast zawsze pobierać wszystkie wiersze z tabeli.</span><span class="sxs-lookup"><span data-stu-id="12a98-198">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="12a98-199">Będzie wprowadzić dodatkowe zmiany w `Index` metody i dodać przyciski stronicowania `Index` widoku.</span><span class="sxs-lookup"><span data-stu-id="12a98-199">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="12a98-200">Poniższa ilustracja przedstawia przyciski stronicowania.</span><span class="sxs-lookup"><span data-stu-id="12a98-200">The following illustration shows the paging buttons.</span></span>

![Studenci indeksu stronę linkami stronicowania](sort-filter-page/_static/paging.png)

<span data-ttu-id="12a98-202">W folderze projektu, należy utworzyć `PaginatedList.cs`, a następnie Zastąp kod szablonu poniższym kodem.</span><span class="sxs-lookup"><span data-stu-id="12a98-202">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="12a98-203">`CreateAsync` Metoda ten kod pobiera rozmiar strony i numer strony i stosuje odpowiednie `Skip` i `Take` instrukcje `IQueryable`.</span><span class="sxs-lookup"><span data-stu-id="12a98-203">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="12a98-204">Gdy `ToListAsync` jest wywoływana w `IQueryable`, to zostanie zwrócona lista zawierająca tylko żądanej strony.</span><span class="sxs-lookup"><span data-stu-id="12a98-204">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="12a98-205">Właściwości `HasPreviousPage` i `HasNextPage` pozwala włączyć lub wyłączyć **Wstecz** i **dalej** stronicowania przycisków.</span><span class="sxs-lookup"><span data-stu-id="12a98-205">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="12a98-206">A `CreateAsync` metoda jest używana zamiast konstruktora, aby utworzyć `PaginatedList<T>` obiektu, ponieważ konstruktory nie można uruchomić kod asynchroniczny.</span><span class="sxs-lookup"><span data-stu-id="12a98-206">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-to-index-method"></a><span data-ttu-id="12a98-207">Dodawanie stronicowania do Index — metoda</span><span class="sxs-lookup"><span data-stu-id="12a98-207">Add paging to Index method</span></span>

<span data-ttu-id="12a98-208">W *StudentsController.cs*, Zastąp `Index` metoda następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="12a98-208">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="12a98-209">Ten kod dodaje parametr numer strony, bieżący parametr kolejność sortowania i bieżącego parametru filtru w podpisie metody.</span><span class="sxs-lookup"><span data-stu-id="12a98-209">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? pageNumber)
```

<span data-ttu-id="12a98-210">Po raz pierwszy, ta strona jest wyświetlana, lub jeśli użytkownik nie kliknie, stronicowanie i sortowanie łącza, wszystkich parametrów będzie pusta.</span><span class="sxs-lookup"><span data-stu-id="12a98-210">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="12a98-211">Po kliknięciu łącza stronicowania zmienną strony będzie zawierać numer strony, aby wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="12a98-211">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="12a98-212">`ViewData` Elementu o nazwie CurrentSort zapewnia widok z bieżącej kolejności sortowania, ponieważ ten musi być uwzględniona w łącza stronicowania Aby zachować porządek sortowania, taki sam, podczas stronicowania.</span><span class="sxs-lookup"><span data-stu-id="12a98-212">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="12a98-213">`ViewData` Elementu o nazwie BieżącyFiltr zawiera widok bieżący ciąg filtru.</span><span class="sxs-lookup"><span data-stu-id="12a98-213">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="12a98-214">Ta wartość musi być uwzględniona w łącza stronicowania w celu utrzymania ustawień filtru podczas stronicowania i musi on zostać przywrócony do pola tekstowego po stronie zostanie wyświetlony ponownie.</span><span class="sxs-lookup"><span data-stu-id="12a98-214">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="12a98-215">Jeśli ciąg wyszukiwania została zmieniona podczas stronicowania, strony musi być ustawiony na 1, nowy filtr może powodować różne dane do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="12a98-215">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="12a98-216">Ciąg wyszukiwania jest zmieniany, gdy wartość jest wprowadzana w polu tekstowym i naciśnięciu przycisku Prześlij.</span><span class="sxs-lookup"><span data-stu-id="12a98-216">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="12a98-217">W takim przypadku `searchString` parametru nie ma wartości null.</span><span class="sxs-lookup"><span data-stu-id="12a98-217">In that case, the `searchString` parameter isn't null.</span></span>

```csharp
if (searchString != null)
{
    pageNumber = 1;
}
else
{
    searchString = currentFilter;
}
```

<span data-ttu-id="12a98-218">Na koniec `Index` metody `PaginatedList.CreateAsync` metoda konwertuje zapytań dla uczniów na pojedynczej strony uczniów na typ kolekcji, który obsługuje stronicowanie.</span><span class="sxs-lookup"><span data-stu-id="12a98-218">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="12a98-219">Tego jednostronicowej studentów są następnie przekazywane do widoku.</span><span class="sxs-lookup"><span data-stu-id="12a98-219">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), pageNumber ?? 1, pageSize));
```

<span data-ttu-id="12a98-220">`PaginatedList.CreateAsync` Metoda przyjmuje numeru strony.</span><span class="sxs-lookup"><span data-stu-id="12a98-220">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="12a98-221">Dwa znaki zapytania reprezentują operatora łączenia wartości null.</span><span class="sxs-lookup"><span data-stu-id="12a98-221">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="12a98-222">Operator łączenia wartości null określa wartość domyślną dla typu dopuszczającego wartość null; wyrażenie `(pageNumber ?? 1)` oznacza, że zwracają wartość `pageNumber` jeśli jego wartość, lub zwraca 1, jeśli `pageNumber` ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="12a98-222">The null-coalescing operator defines a default value for a nullable type; the expression `(pageNumber ?? 1)` means return the value of `pageNumber` if it has a value, or return 1 if `pageNumber` is null.</span></span>

## <a name="add-paging-links"></a><span data-ttu-id="12a98-223">Dodawanie łączy stronicowania</span><span class="sxs-lookup"><span data-stu-id="12a98-223">Add paging links</span></span>

<span data-ttu-id="12a98-224">W *Views/Students/Index.cshtml*, Zastąp istniejący kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="12a98-224">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="12a98-225">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="12a98-225">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="12a98-226">`@model` Instrukcji w górnej części strony określa widok teraz pobiera `PaginatedList<T>` zamiast obiektu `List<T>` obiektu.</span><span class="sxs-lookup"><span data-stu-id="12a98-226">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="12a98-227">Linki nagłówek kolumny, użyj ciągu zapytania do przekazania aktualnie wyszukiwanego ciągu do kontrolera, dzięki czemu użytkownik może sortować w ramach wyników filtrowania:</span><span class="sxs-lookup"><span data-stu-id="12a98-227">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="12a98-228">Przyciski stronicowania są wyświetlane przez pomocników tagów:</span><span class="sxs-lookup"><span data-stu-id="12a98-228">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-pageNumber="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="12a98-229">Uruchom aplikację, a następnie przejdź do strony studentów.</span><span class="sxs-lookup"><span data-stu-id="12a98-229">Run the app and go to the Students page.</span></span>

![Studenci indeksu stronę linkami stronicowania](sort-filter-page/_static/paging.png)

<span data-ttu-id="12a98-231">Po kliknięciu łączy stronicowania w różnych sortowania, aby upewnić się, że działa stronicowania.</span><span class="sxs-lookup"><span data-stu-id="12a98-231">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="12a98-232">Następnie wprowadź wyszukiwany ciąg i spróbuj stronicowania ponownie, aby sprawdzić, czy stronicowania również działa poprawnie przy użyciu sortowania i filtrowania.</span><span class="sxs-lookup"><span data-stu-id="12a98-232">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page"></a><span data-ttu-id="12a98-233">Tworzenie strony informacje</span><span class="sxs-lookup"><span data-stu-id="12a98-233">Create an About page</span></span>

<span data-ttu-id="12a98-234">Dla witryny internetowej firmy Contoso University **o** stronie będą wyświetlane, jak wiele studentów zostały zarejestrowane dla każdego dnia rejestracji.</span><span class="sxs-lookup"><span data-stu-id="12a98-234">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="12a98-235">Wymaga to obliczeń grupowania i prostych grup.</span><span class="sxs-lookup"><span data-stu-id="12a98-235">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="12a98-236">Aby to osiągnąć, wykonasz następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="12a98-236">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="12a98-237">Utwórz klasę modelu widoku danych potrzebnych do przekazania do widoku.</span><span class="sxs-lookup"><span data-stu-id="12a98-237">Create a view model class for the data that you need to pass to the view.</span></span>
* <span data-ttu-id="12a98-238">Utwórz metodę informacje w kontrolera głównego.</span><span class="sxs-lookup"><span data-stu-id="12a98-238">Create the About method in the Home controller.</span></span>
* <span data-ttu-id="12a98-239">Utwórz widok informacje.</span><span class="sxs-lookup"><span data-stu-id="12a98-239">Create the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="12a98-240">Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="12a98-240">Create the view model</span></span>

<span data-ttu-id="12a98-241">Tworzenie *SchoolViewModels* folderu w *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="12a98-241">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="12a98-242">W nowym folderze, Dodaj plik klasy *EnrollmentDateGroup.cs* i Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="12a98-242">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="12a98-243">Modyfikowanie kontrolera głównego</span><span class="sxs-lookup"><span data-stu-id="12a98-243">Modify the Home Controller</span></span>

<span data-ttu-id="12a98-244">W *HomeController.cs*, Dodaj następujące instrukcje using na początku pliku:</span><span class="sxs-lookup"><span data-stu-id="12a98-244">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="12a98-245">Dodaj zmienną klasy kontekstu bazy danych bezpośrednio po otwierającym nawiasie klamrowym dla klasy i uzyskiwanie wystąpienia kontekstu platformy ASP.NET Core DI:</span><span class="sxs-lookup"><span data-stu-id="12a98-245">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="12a98-246">Dodaj `About` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="12a98-246">Add an `About` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="12a98-247">Instrukcji LINQ grup jednostek uczniów według daty rejestracji, oblicza liczbę jednostek w każdej grupie i przechowuje wyniki w zbiorze `EnrollmentDateGroup` wyświetlić obiekty w modelu.</span><span class="sxs-lookup"><span data-stu-id="12a98-247">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>

### <a name="create-the-about-view"></a><span data-ttu-id="12a98-248">Utwórz widok — informacje</span><span class="sxs-lookup"><span data-stu-id="12a98-248">Create the About View</span></span>

<span data-ttu-id="12a98-249">Dodaj *Views/Home/About.cshtml* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="12a98-249">Add a *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="12a98-250">Uruchom aplikację, a następnie przejdź do strony informacje.</span><span class="sxs-lookup"><span data-stu-id="12a98-250">Run the app and go to the About page.</span></span> <span data-ttu-id="12a98-251">Liczba studentów każdej daty rejestracji jest wyświetlany w tabeli.</span><span class="sxs-lookup"><span data-stu-id="12a98-251">The count of students for each enrollment date is displayed in a table.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="12a98-252">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="12a98-252">Get the code</span></span>

[<span data-ttu-id="12a98-253">Pobieranie i wyświetlanie ukończonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="12a98-253">Download or view the completed application.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a><span data-ttu-id="12a98-254">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="12a98-254">Next steps</span></span>

<span data-ttu-id="12a98-255">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="12a98-255">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12a98-256">Dodana kolumna sortowania łącza</span><span class="sxs-lookup"><span data-stu-id="12a98-256">Added column sort links</span></span>
> * <span data-ttu-id="12a98-257">Dodano pole wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="12a98-257">Added a Search box</span></span>
> * <span data-ttu-id="12a98-258">Dodano stronicowania do indeksu uczniów</span><span class="sxs-lookup"><span data-stu-id="12a98-258">Added paging to Students Index</span></span>
> * <span data-ttu-id="12a98-259">Dodano stronicowania do Index — metoda</span><span class="sxs-lookup"><span data-stu-id="12a98-259">Added paging to Index method</span></span>
> * <span data-ttu-id="12a98-260">Dodano linki stronicowania</span><span class="sxs-lookup"><span data-stu-id="12a98-260">Added paging links</span></span>
> * <span data-ttu-id="12a98-261">Utworzona na stronie informacje</span><span class="sxs-lookup"><span data-stu-id="12a98-261">Created an About page</span></span>

<span data-ttu-id="12a98-262">Przejdź do następnego samouczka, aby dowiedzieć się, jak obsługiwać zmiany w modelu danych przy użyciu migracji.</span><span class="sxs-lookup"><span data-stu-id="12a98-262">Advance to the next tutorial to learn how to handle data model changes by using migrations.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="12a98-263">Dalej: Obsługa zmiany w modelu danych</span><span class="sxs-lookup"><span data-stu-id="12a98-263">Next: Handle data model changes</span></span>](migrations.md)
