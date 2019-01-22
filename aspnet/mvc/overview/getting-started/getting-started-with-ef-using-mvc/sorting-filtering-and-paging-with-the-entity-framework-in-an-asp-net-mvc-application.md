---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Samouczek: Dodaj sortowanie, filtrowanie i stronicowanie za pomocą programu Entity Framework w aplikacji ASP.NET MVC | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tym samouczku dodasz, sortowanie, filtrowanie i stronicowanie funkcje **studentów** strony indeksu. Możesz również utworzyć strony proste grupowanie.
ms.author: riande
ms.date: 01/14/2019
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 1f18a15d39d58ffb4ac48cfccee6519d33294e85
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444198"
---
# <a name="tutorial-add-sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a><span data-ttu-id="88f81-104">Samouczek: Dodaj sortowanie, filtrowanie i stronicowanie za pomocą programu Entity Framework w aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="88f81-104">Tutorial: Add sorting, filtering, and paging with the Entity Framework in an ASP.NET MVC application</span></span>

<span data-ttu-id="88f81-105">W [poprzedniego samouczka](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), zaimplementowane zestaw stron sieci web w przypadku podstawowych operacji CRUD dla `Student` jednostek.</span><span class="sxs-lookup"><span data-stu-id="88f81-105">In the [previous tutorial](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), you implemented a set of web pages for basic CRUD operations for `Student` entities.</span></span> <span data-ttu-id="88f81-106">W tym samouczku dodasz, sortowanie, filtrowanie i stronicowanie funkcje **studentów** strony indeksu.</span><span class="sxs-lookup"><span data-stu-id="88f81-106">In this tutorial you add sorting, filtering, and paging functionality to the **Students** Index page.</span></span> <span data-ttu-id="88f81-107">Możesz również utworzyć strony proste grupowanie.</span><span class="sxs-lookup"><span data-stu-id="88f81-107">You also create a simple grouping page.</span></span>

<span data-ttu-id="88f81-108">Na poniższej ilustracji przedstawiono, jak będzie wyglądać stronę po zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="88f81-108">The following image shows what the page will look like when you're done.</span></span> <span data-ttu-id="88f81-109">Nagłówki kolumn odnoszą się łącza, które użytkownik może kliknąć, aby posortować według tej kolumny.</span><span class="sxs-lookup"><span data-stu-id="88f81-109">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="88f81-110">Klikając kolumnę nagłówka wielokrotnie przełącza między malejącą i rosnącą porządek sortowania.</span><span class="sxs-lookup"><span data-stu-id="88f81-110">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="88f81-112">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="88f81-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="88f81-113">Dodaj kolumnę sortowania łącza</span><span class="sxs-lookup"><span data-stu-id="88f81-113">Add column sort links</span></span>
> * <span data-ttu-id="88f81-114">Dodawanie pola wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="88f81-114">Add a Search box</span></span>
> * <span data-ttu-id="88f81-115">Dodawanie stronicowania</span><span class="sxs-lookup"><span data-stu-id="88f81-115">Add paging</span></span>
> * <span data-ttu-id="88f81-116">Tworzenie strony informacje</span><span class="sxs-lookup"><span data-stu-id="88f81-116">Create an About page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="88f81-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="88f81-117">Prerequisites</span></span>

* [<span data-ttu-id="88f81-118">Implementowanie podstawowych funkcji CRUD</span><span class="sxs-lookup"><span data-stu-id="88f81-118">Implementing Basic CRUD Functionality</span></span>](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)

## <a name="add-column-sort-links"></a><span data-ttu-id="88f81-119">Dodaj kolumnę sortowania łącza</span><span class="sxs-lookup"><span data-stu-id="88f81-119">Add column sort links</span></span>

<span data-ttu-id="88f81-120">Aby dodać sortowanie do strony indeksu dla uczniów, zmienisz `Index` metody `Student` kontrolera i Dodaj kod do `Student` indeksować widoku.</span><span class="sxs-lookup"><span data-stu-id="88f81-120">To add sorting to the Student Index page, you'll change the `Index` method of the `Student` controller and add code to the `Student` Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="88f81-121">Dodawanie funkcji sortowania do Index — metoda</span><span class="sxs-lookup"><span data-stu-id="88f81-121">Add sorting functionality to the Index method</span></span>

- <span data-ttu-id="88f81-122">W *Controllers\StudentController.cs*, Zastąp `Index` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="88f81-122">In *Controllers\StudentController.cs*, replace the `Index` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="88f81-123">Ten kod odbiera `sortOrder` parametr ciągu zapytania w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="88f81-123">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="88f81-124">Wartość ciągu zapytania jest dostarczane przez platformę ASP.NET MVC jako parametr do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="88f81-124">The query string value is provided by ASP.NET MVC as a parameter to the action method.</span></span> <span data-ttu-id="88f81-125">Parametr jest ciąg, który jest "Name" lub "Date", opcjonalnie, podkreślenia i ciąg "desc", aby określić w kolejności malejącej.</span><span class="sxs-lookup"><span data-stu-id="88f81-125">The parameter is a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="88f81-126">Domyślna kolejność sortowania jest rosnąca.</span><span class="sxs-lookup"><span data-stu-id="88f81-126">The default sort order is ascending.</span></span>

<span data-ttu-id="88f81-127">Przy pierwszym żądaniu strony indeksu nie jest Brak ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="88f81-127">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="88f81-128">Uczniów są wyświetlane w porządku rosnącym według `LastName`, co jest ustawieniem domyślnym, zgodnie z ustaleniami w należą do przypadku `switch` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="88f81-128">The students are displayed in ascending order by `LastName`, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="88f81-129">Kiedy użytkownik kliknie hiperlink nagłówek kolumny, odpowiednie `sortOrder` podana w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="88f81-129">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="88f81-130">Dwa `ViewBag` zmienne są używane, tak aby widok można skonfigurować hiperłącza nagłówków kolumn za pomocą wartości ciągu zapytania odpowiednie:</span><span class="sxs-lookup"><span data-stu-id="88f81-130">The two `ViewBag` variables are used so that the view can configure the column heading hyperlinks with the appropriate query string values:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="88f81-131">Są to trójargumentowy instrukcji.</span><span class="sxs-lookup"><span data-stu-id="88f81-131">These are ternary statements.</span></span> <span data-ttu-id="88f81-132">Pierwsza z nich Określa, że jeśli `sortOrder` parametr ma wartość null lub jest pusta, `ViewBag.NameSortParm` powinna być ustawiona na "nazwa\_desc"; w przeciwnym razie należy ją ustawić na pusty ciąg.</span><span class="sxs-lookup"><span data-stu-id="88f81-132">The first one specifies that if the `sortOrder` parameter is null or empty, `ViewBag.NameSortParm` should be set to "name\_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="88f81-133">Te dwie instrukcje Włącz widok, aby ustawić hiperłącza nagłówek kolumny w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="88f81-133">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

| <span data-ttu-id="88f81-134">Bieżący kolejność sortowania</span><span class="sxs-lookup"><span data-stu-id="88f81-134">Current sort order</span></span> | <span data-ttu-id="88f81-135">Ostatnia nazwa hiperłącza</span><span class="sxs-lookup"><span data-stu-id="88f81-135">Last Name Hyperlink</span></span> | <span data-ttu-id="88f81-136">Data hiperłącza</span><span class="sxs-lookup"><span data-stu-id="88f81-136">Date Hyperlink</span></span> |
| --- | --- | --- |
| <span data-ttu-id="88f81-137">Ostatnie nazwy, rosnąco</span><span class="sxs-lookup"><span data-stu-id="88f81-137">Last Name ascending</span></span> | <span data-ttu-id="88f81-138">descending</span><span class="sxs-lookup"><span data-stu-id="88f81-138">descending</span></span> | <span data-ttu-id="88f81-139">ascending</span><span class="sxs-lookup"><span data-stu-id="88f81-139">ascending</span></span> |
| <span data-ttu-id="88f81-140">Ostatnie nazwy, malejąco</span><span class="sxs-lookup"><span data-stu-id="88f81-140">Last Name descending</span></span> | <span data-ttu-id="88f81-141">ascending</span><span class="sxs-lookup"><span data-stu-id="88f81-141">ascending</span></span> | <span data-ttu-id="88f81-142">ascending</span><span class="sxs-lookup"><span data-stu-id="88f81-142">ascending</span></span> |
| <span data-ttu-id="88f81-143">Data, w kolejności rosnącej</span><span class="sxs-lookup"><span data-stu-id="88f81-143">Date ascending</span></span> | <span data-ttu-id="88f81-144">ascending</span><span class="sxs-lookup"><span data-stu-id="88f81-144">ascending</span></span> | <span data-ttu-id="88f81-145">descending</span><span class="sxs-lookup"><span data-stu-id="88f81-145">descending</span></span> |
| <span data-ttu-id="88f81-146">Data, malejąco</span><span class="sxs-lookup"><span data-stu-id="88f81-146">Date descending</span></span> | <span data-ttu-id="88f81-147">ascending</span><span class="sxs-lookup"><span data-stu-id="88f81-147">ascending</span></span> | <span data-ttu-id="88f81-148">ascending</span><span class="sxs-lookup"><span data-stu-id="88f81-148">ascending</span></span> |

<span data-ttu-id="88f81-149">Metoda używa [składnik LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) określić kolumnę sortowania.</span><span class="sxs-lookup"><span data-stu-id="88f81-149">The method uses [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) to specify the column to sort by.</span></span> <span data-ttu-id="88f81-150">Ten kod tworzy <xref:System.Linq.IQueryable%601> zmiennej przed `switch` instrukcji, modyfikuje je w `switch` instrukcji i wywołania `ToList` metody `switch` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="88f81-150">The code creates an <xref:System.Linq.IQueryable%601> variable before the `switch` statement, modifies it in the `switch` statement, and calls the `ToList` method after the `switch` statement.</span></span> <span data-ttu-id="88f81-151">Podczas tworzenia i modyfikowania `IQueryable` zmiennych, bez określenia zapytania są wysyłane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="88f81-151">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="88f81-152">Zapytanie nie jest wykonywane, dopóki nie można przekonwertować `IQueryable` obiektu w kolekcji przez wywołanie metody, takie jak `ToList`.</span><span class="sxs-lookup"><span data-stu-id="88f81-152">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToList`.</span></span> <span data-ttu-id="88f81-153">W związku z tym, ten kod powoduje pojedyncze zapytanie, które nie jest wykonywane, dopóki `return View` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="88f81-153">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

<span data-ttu-id="88f81-154">Jako alternatywę do pisania różnych instrukcji LINQ dla każdego kolejność sortowania można dynamicznie utworzyć instrukcji LINQ.</span><span class="sxs-lookup"><span data-stu-id="88f81-154">As an alternative to writing different LINQ statements for each sort order, you can dynamically create a LINQ statement.</span></span> <span data-ttu-id="88f81-155">Aby uzyskać informacji na temat dynamicznej LINQ, zobacz [dynamiczne LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).</span><span class="sxs-lookup"><span data-stu-id="88f81-155">For information about dynamic LINQ, see [Dynamic LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="88f81-156">Dodawanie hiperłączy nagłówek kolumny do widoku indeksu dla uczniów</span><span class="sxs-lookup"><span data-stu-id="88f81-156">Add column heading hyperlinks to the Student index view</span></span>

1. <span data-ttu-id="88f81-157">W *Views\Student\Index.cshtml*, Zastąp `<tr>` i `<th>` elementy wiersz nagłówka z wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="88f81-157">In *Views\Student\Index.cshtml*, replace the `<tr>` and `<th>` elements for the heading row with the highlighted code:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   <span data-ttu-id="88f81-158">Ten kod używa tych informacji w `ViewBag` właściwości, aby skonfigurować hiperlinki z zapytaniem odpowiednie wartości ciągu.</span><span class="sxs-lookup"><span data-stu-id="88f81-158">This code uses the information in the `ViewBag` properties to set up hyperlinks with the appropriate query string values.</span></span>

2. <span data-ttu-id="88f81-159">Uruchom stronę, a następnie kliknij przycisk **nazwisko** i **Data rejestracji** nagłówków kolumn, aby zweryfikować, że sortowanie działa.</span><span class="sxs-lookup"><span data-stu-id="88f81-159">Run the page and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

   <span data-ttu-id="88f81-160">Po kliknięciu **nazwisko** nagłówka, studentów są wyświetlane w ostatniej malejącej nazwy.</span><span class="sxs-lookup"><span data-stu-id="88f81-160">After you click the **Last Name** heading, students are displayed in descending last name order.</span></span>

## <a name="add-a-search-box"></a><span data-ttu-id="88f81-161">Dodawanie pola wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="88f81-161">Add a Search box</span></span>

<span data-ttu-id="88f81-162">Aby dodać filtrowanie do strony indeksu studentów, dodasz pole tekstowe i przycisk Prześlij do widoku i wprowadzić odpowiednie zmiany w `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="88f81-162">To add filtering to the Students index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="88f81-163">Pole tekstowe pozwala wprowadzić ciąg do wyszukania w imię pola imienia i nazwiska.</span><span class="sxs-lookup"><span data-stu-id="88f81-163">The text box lets you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="88f81-164">Dodawanie funkcji filtrowania do Index — metoda</span><span class="sxs-lookup"><span data-stu-id="88f81-164">Add filtering functionality to the Index method</span></span>

- <span data-ttu-id="88f81-165">W *Controllers\StudentController.cs*, Zastąp `Index` metoda następującym kodem (zmiany zostały wyróżnione):</span><span class="sxs-lookup"><span data-stu-id="88f81-165">In *Controllers\StudentController.cs*, replace the `Index` method with the following code (the changes are highlighted):</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

<span data-ttu-id="88f81-166">Ten kod dodaje `searchString` parametr `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="88f81-166">The code adds a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="88f81-167">Wartość ciągu wyszukiwania są odebrane z pola tekstowego, które należy dodać do widoku indeksu.</span><span class="sxs-lookup"><span data-stu-id="88f81-167">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="88f81-168">Dodaje także `where` klauzuli do instrukcji LINQ, który wybiera tylko uczniowie, w których imię lub nazwisko zawiera ciąg wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="88f81-168">It also adds a `where` clause to the LINQ statement that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="88f81-169">Instrukcja, która dodaje <xref:System.Linq.Queryable.Where%2A> klauzula jest wykonywany tylko wtedy, gdy wartość do wyszukania.</span><span class="sxs-lookup"><span data-stu-id="88f81-169">The statement that adds the <xref:System.Linq.Queryable.Where%2A> clause executes only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="88f81-170">W wielu przypadkach można wywołać tej samej metody, zestaw jednostek platformy Entity Framework lub jako metodę rozszerzenia w kolekcji w pamięci.</span><span class="sxs-lookup"><span data-stu-id="88f81-170">In many cases you can call the same method either on an Entity Framework entity set or as an extension method on an in-memory collection.</span></span> <span data-ttu-id="88f81-171">Wyniki są zazwyczaj takie same, ale w niektórych przypadkach może być inna.</span><span class="sxs-lookup"><span data-stu-id="88f81-171">The results are normally the same but in some cases may be different.</span></span>
>
> <span data-ttu-id="88f81-172">Na przykład implementacji .NET Framework z `Contains` metoda zwraca wszystkie wiersze, w przypadku przekazania pustego ciągu do niego, ale dostawcy środowiska Entity Framework dla programu SQL Server Compact 4.0 zwraca zero wierszy obecność pustych ciągów.</span><span class="sxs-lookup"><span data-stu-id="88f81-172">For example, the .NET Framework implementation of the `Contains` method returns all rows when you pass an empty string to it, but the Entity Framework provider for SQL Server Compact 4.0 returns zero rows for empty strings.</span></span> <span data-ttu-id="88f81-173">W związku z tym kodem w przykładzie (umieszczanie `Where` instrukcji wewnątrz `if` instrukcji) zapewnia, że, Pobierz te same wyniki dla wszystkich wersji programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="88f81-173">Therefore the code in the example (putting the `Where` statement inside an `if` statement) makes sure that you get the same results for all versions of SQL Server.</span></span> <span data-ttu-id="88f81-174">Ponadto wdrożenia programu .NET Framework z `Contains` metoda wykonuje porównania uwzględniającego wielkość liter, domyślnie, ale dostawcy programu Entity Framework SQL Server wykonania porównania bez uwzględniania wielkości liter, domyślnie.</span><span class="sxs-lookup"><span data-stu-id="88f81-174">Also, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but Entity Framework SQL Server providers perform case-insensitive comparisons by default.</span></span> <span data-ttu-id="88f81-175">Dlatego wywołanie `ToUpper` metody testu jawnie bez uwzględniania wielkości liter zapewnia wyniki nie należy zmieniać po zmianie kodu później, aby korzystać z repozytorium, które zwróci `IEnumerable` zbiór zamiast `IQueryable` obiektu.</span><span class="sxs-lookup"><span data-stu-id="88f81-175">Therefore, calling the `ToUpper` method to make the test explicitly case-insensitive ensures that results do not change when you change the code later to use a repository, which will return an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="88f81-176">(Gdy wywołujesz `Contains` metody `IEnumerable` kolekcji, możesz pobrać wdrożenia programu .NET Framework; Jeśli wywołasz ją na `IQueryable` obiektu, możesz uzyskać implementację dostawcy bazy danych.)</span><span class="sxs-lookup"><span data-stu-id="88f81-176">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.)</span></span>
>
> <span data-ttu-id="88f81-177">Obsługa wartości null, również mogą być różne dla dostawców innej bazy danych lub jeśli używasz `IQueryable` względem obiektu, gdy używasz `IEnumerable` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="88f81-177">Null handling may also be different for different database providers or when you use an `IQueryable` object compared to when you use an `IEnumerable` collection.</span></span> <span data-ttu-id="88f81-178">Na przykład w niektórych scenariuszach `Where` warunków, takich jak `table.Column != 0` mogą nie zwracać kolumn, które mają `null` jako wartość.</span><span class="sxs-lookup"><span data-stu-id="88f81-178">For example, in some scenarios a `Where` condition such as `table.Column != 0` may not return columns that have `null` as the value.</span></span> <span data-ttu-id="88f81-179">Aby uzyskać więcej informacji, zobacz [nieprawidłowa obsługa zmiennych o wartości null w klauzuli "where"](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).</span><span class="sxs-lookup"><span data-stu-id="88f81-179">For more information, see [Incorrect handling of null variables in 'where' clause](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="88f81-180">Dodaj pole wyszukiwania do widoku indeksu dla uczniów</span><span class="sxs-lookup"><span data-stu-id="88f81-180">Add a search box to the Student index view</span></span>

1. <span data-ttu-id="88f81-181">W *Views\Student\Index.cshtml*, Dodaj wyróżniony kod bezpośrednio przed otwierającym `table` tag, aby można było utworzyć podpis, pola tekstowego, a **wyszukiwania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="88f81-181">In *Views\Student\Index.cshtml*, add the highlighted code immediately before the opening `table` tag in order to create a caption, a text box, and a **Search** button.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. <span data-ttu-id="88f81-182">Uruchom strony, wprowadź wyszukiwany ciąg, a następnie kliknij przycisk **wyszukiwania** Aby sprawdzić, czy działa filtrowanie.</span><span class="sxs-lookup"><span data-stu-id="88f81-182">Run the page, enter a search string, and click **Search** to verify that filtering is working.</span></span>

   <span data-ttu-id="88f81-183">Należy zauważyć, że adres URL nie zawiera "" ciąg wyszukiwania, co oznacza, że jeśli Oznacz tę stronę zakładką nie otrzymasz filtrowana lista korzystając z zakładki.</span><span class="sxs-lookup"><span data-stu-id="88f81-183">Notice the URL doesn't contain the "an" search string, which means that if you bookmark this page, you won't get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="88f81-184">Dotyczy to również łączy sortowania kolumn, ponieważ zostaną one posortowane całej listy.</span><span class="sxs-lookup"><span data-stu-id="88f81-184">This applies also to the column sort links, as they will sort the whole list.</span></span> <span data-ttu-id="88f81-185">Poznasz, jak zmienić **wyszukiwania** przycisk, aby użyć ciągów zapytań do kryteriów filtrowania w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="88f81-185">You'll change the **Search** button to use query strings for filter criteria later in the tutorial.</span></span>

## <a name="add-paging"></a><span data-ttu-id="88f81-186">Dodawanie stronicowania</span><span class="sxs-lookup"><span data-stu-id="88f81-186">Add paging</span></span>

<span data-ttu-id="88f81-187">Dodawanie stronicowania do strony indeksu studentów, zostanie najpierw należy zainstalować **PagedList.Mvc** pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="88f81-187">To add paging to the Students index page, you'll start by installing the **PagedList.Mvc** NuGet package.</span></span> <span data-ttu-id="88f81-188">Będzie wprowadzić dodatkowe zmiany w `Index` metody i dodawanie stronicowania łącza do `Index` widoku.</span><span class="sxs-lookup"><span data-stu-id="88f81-188">Then you'll make additional changes in the `Index` method and add paging links to the `Index` view.</span></span> <span data-ttu-id="88f81-189">**PagedList.Mvc** jest jednym z wielu dobre stronicowania i sortowania pakietów dla platformy ASP.NET MVC i jego użycia w tym miejscu jest przeznaczona tylko na potrzeby przykładu nie jako zalecenie dla niego inne opcje.</span><span class="sxs-lookup"><span data-stu-id="88f81-189">**PagedList.Mvc** is one of many good paging and sorting packages for ASP.NET MVC, and its use here is intended only as an example, not as a recommendation for it over other options.</span></span>

### <a name="install-the-pagedlistmvc-nuget-package"></a><span data-ttu-id="88f81-190">Zainstaluj pakiet PagedList.MVC NuGet</span><span class="sxs-lookup"><span data-stu-id="88f81-190">Install the PagedList.MVC NuGet package</span></span>

<span data-ttu-id="88f81-191">NuGet **PagedList.Mvc** pakiet automatycznie instaluje **PagedList** pakietu jako zależność.</span><span class="sxs-lookup"><span data-stu-id="88f81-191">The NuGet **PagedList.Mvc** package automatically installs the **PagedList** package as a dependency.</span></span> <span data-ttu-id="88f81-192">**PagedList** pakiet instaluje `PagedList` kolekcji typu i rozszerzenie metody `IQueryable` i `IEnumerable` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="88f81-192">The **PagedList** package installs a `PagedList` collection type and extension methods for `IQueryable` and `IEnumerable` collections.</span></span> <span data-ttu-id="88f81-193">Metody rozszerzające tworzenia pojedynczej strony danych w `PagedList` kolekcji z Twojej `IQueryable` lub `IEnumerable`i `PagedList` kolekcji zawiera kilka właściwości i metod, które ułatwiają stronicowania.</span><span class="sxs-lookup"><span data-stu-id="88f81-193">The extension methods create a single page of data in a `PagedList` collection out of your `IQueryable` or `IEnumerable`, and the `PagedList` collection provides several properties and methods that facilitate paging.</span></span> <span data-ttu-id="88f81-194">**PagedList.Mvc** pakiet Instaluje pomocnika stronicowania, który wyświetla przyciski stronicowania.</span><span class="sxs-lookup"><span data-stu-id="88f81-194">The **PagedList.Mvc** package installs a paging helper that displays the paging buttons.</span></span>

1. <span data-ttu-id="88f81-195">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** i następnie **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="88f81-195">From the **Tools** menu, select **NuGet Package Manager** and then **Package Manager Console**.</span></span>

2. <span data-ttu-id="88f81-196">W **Konsola Menedżera pakietów** okna, upewnij się, że **źródła pakietu** jest **nuget.org** i **projekt domyślny** jest **ContosoUniversity**, a następnie wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="88f81-196">In the **Package Manager Console** window, make sure the **Package source** is **nuget.org** and the **Default project** is **ContosoUniversity**, and then enter the following command:</span></span>

   ```text
   Install-Package PagedList.Mvc
   ```

3. <span data-ttu-id="88f81-197">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="88f81-197">Build the project.</span></span>

### <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="88f81-198">Dodawanie funkcji stronicowania do Index — metoda</span><span class="sxs-lookup"><span data-stu-id="88f81-198">Add paging functionality to the Index method</span></span>

1. <span data-ttu-id="88f81-199">W *Controllers\StudentController.cs*, Dodaj `using` poufności informacji dotyczące `PagedList` przestrzeni nazw:</span><span class="sxs-lookup"><span data-stu-id="88f81-199">In *Controllers\StudentController.cs*, add a `using` statement for the `PagedList` namespace:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. <span data-ttu-id="88f81-200">Zastąp `Index` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="88f81-200">Replace the `Index` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   <span data-ttu-id="88f81-201">Ten kod dodaje `page` parametr, bieżący parametr kolejność sortowania i bieżący parametr filtru do sygnatury metody:</span><span class="sxs-lookup"><span data-stu-id="88f81-201">This code adds a `page` parameter, a current sort order parameter, and a current filter parameter to the method signature:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   <span data-ttu-id="88f81-202">Po raz pierwszy, ta strona jest wyświetlana, lub jeśli użytkownik nie kliknie, stronicowanie i sortowanie łącza, wszystkie parametry mają wartość null.</span><span class="sxs-lookup"><span data-stu-id="88f81-202">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters are null.</span></span> <span data-ttu-id="88f81-203">Po kliknięciu łącza stronicowania `page` zmienna zawiera numer strony, aby wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="88f81-203">If a paging link is clicked, the `page` variable contains the page number to display.</span></span>

   <span data-ttu-id="88f81-204">A `ViewBag` właściwości zapewnia widok z bieżącej kolejności sortowania, ponieważ ten musi być uwzględniona w łącza stronicowania Aby zachować porządek sortowania, taki sam, podczas stronicowania:</span><span class="sxs-lookup"><span data-stu-id="88f81-204">A `ViewBag` property provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   <span data-ttu-id="88f81-205">Inna właściwość `ViewBag.CurrentFilter`, zawiera widok bieżący ciąg filtru.</span><span class="sxs-lookup"><span data-stu-id="88f81-205">Another property, `ViewBag.CurrentFilter`, provides the view with the current filter string.</span></span> <span data-ttu-id="88f81-206">Ta wartość musi być uwzględniona w łącza stronicowania w celu utrzymania ustawień filtru podczas stronicowania i musi on zostać przywrócony do pola tekstowego po stronie zostanie wyświetlony ponownie.</span><span class="sxs-lookup"><span data-stu-id="88f81-206">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span> <span data-ttu-id="88f81-207">Jeśli ciąg wyszukiwania została zmieniona podczas stronicowania, strony musi być ustawiony na 1, nowy filtr może powodować różne dane do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="88f81-207">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="88f81-208">Ciąg wyszukiwania jest zmieniany, gdy wartość jest wprowadzana w polu tekstowym i naciśnięciu przycisku Prześlij.</span><span class="sxs-lookup"><span data-stu-id="88f81-208">The search string is changed when a value is entered in the text box and the submit button is pressed.</span></span> <span data-ttu-id="88f81-209">W takim przypadku `searchString` parametru nie ma wartości null.</span><span class="sxs-lookup"><span data-stu-id="88f81-209">In that case, the `searchString` parameter is not null.</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   <span data-ttu-id="88f81-210">Na końcu metody `ToPagedList` metody rozszerzenia na uczniów `IQueryable` obiektu konwertuje zapytań dla uczniów na pojedynczej strony uczniów na typ kolekcji, który obsługuje stronicowanie.</span><span class="sxs-lookup"><span data-stu-id="88f81-210">At the end of the method, the `ToPagedList` extension method on the students `IQueryable` object converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="88f81-211">Tego jednostronicowej studentów są następnie przekazywane do widoku:</span><span class="sxs-lookup"><span data-stu-id="88f81-211">That single page of students is then passed to the view:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   <span data-ttu-id="88f81-212">`ToPagedList` Metoda przyjmuje numeru strony.</span><span class="sxs-lookup"><span data-stu-id="88f81-212">The `ToPagedList` method takes a page number.</span></span> <span data-ttu-id="88f81-213">Dwa znaki zapytania reprezentują [operatorem łączenia wartości null](/dotnet/csharp/language-reference/operators/null-coalescing-operator).</span><span class="sxs-lookup"><span data-stu-id="88f81-213">The two question marks represent the [null-coalescing operator](/dotnet/csharp/language-reference/operators/null-coalescing-operator).</span></span> <span data-ttu-id="88f81-214">Operator łączenia wartości null określa wartość domyślną dla typu dopuszczającego wartość null; wyrażenie `(page ?? 1)` oznacza, że zwracają wartość `page` jeśli jego wartość, lub zwraca 1, jeśli `page` ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="88f81-214">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

### <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="88f81-215">Dodawanie stronicowania łączy do widoku indeksu dla uczniów</span><span class="sxs-lookup"><span data-stu-id="88f81-215">Add paging links to the Student index view</span></span>

1. <span data-ttu-id="88f81-216">W *Views\Student\Index.cshtml*, Zastąp istniejący kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="88f81-216">In *Views\Student\Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="88f81-217">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="88f81-217">The changes are highlighted.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   <span data-ttu-id="88f81-218">`@model` Instrukcji w górnej części strony określa widok teraz pobiera `PagedList` zamiast obiektu `List` obiektu.</span><span class="sxs-lookup"><span data-stu-id="88f81-218">The `@model` statement at the top of the page specifies that the view now gets a `PagedList` object instead of a `List` object.</span></span>

   <span data-ttu-id="88f81-219">`using` Poufności informacji dotyczące `PagedList.Mvc` daje dostęp do pomocy MVC przycisków stronicowania.</span><span class="sxs-lookup"><span data-stu-id="88f81-219">The `using` statement for `PagedList.Mvc` gives access to the MVC helper for the paging buttons.</span></span>

   <span data-ttu-id="88f81-220">Kod używa przeciążenia [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) umożliwiająca, aby określić [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100)).</span><span class="sxs-lookup"><span data-stu-id="88f81-220">The code uses an overload of [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) that allows it to specify [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100)).</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   <span data-ttu-id="88f81-221">Wartość domyślna [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) przesyła dane formularza z WPISEM, co oznacza, że parametry są przekazywane w treści komunikatu HTTP, a nie w adresie URL jako ciągi zapytań.</span><span class="sxs-lookup"><span data-stu-id="88f81-221">The default [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="88f81-222">Po określeniu HTTP GET, dane formularza jest przekazywany w adresie URL jako ciągi zapytań, która umożliwia użytkownikom utworzyć zakładkę ma adres URL.</span><span class="sxs-lookup"><span data-stu-id="88f81-222">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="88f81-223">[W3C wytyczne dotyczące użytkowania HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) zaleca się, powinien być używany GET akcja nie powoduje aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="88f81-223">The [W3C guidelines for the use of HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) recommend that you should use GET when the action does not result in an update.</span></span>

   <span data-ttu-id="88f81-224">Pole tekstowe jest inicjowany z aktualnie wyszukiwanego ciągu, dzięki czemu po kliknięciu nowej strony można przeglądać aktualnie wyszukiwanego ciągu.</span><span class="sxs-lookup"><span data-stu-id="88f81-224">The text box is initialized with the current search string so when you click a new page you can see the current search string.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   <span data-ttu-id="88f81-225">Linki nagłówek kolumny, użyj ciągu zapytania do przekazania aktualnie wyszukiwanego ciągu do kontrolera, dzięki czemu użytkownik może sortować w ramach wyników filtrowania:</span><span class="sxs-lookup"><span data-stu-id="88f81-225">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   <span data-ttu-id="88f81-226">Bieżącej strony i łączna liczba stron są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="88f81-226">The current page and total number of pages are displayed.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   <span data-ttu-id="88f81-227">W przypadku stron do wyświetlania jest wyświetlany "Page 0 0".</span><span class="sxs-lookup"><span data-stu-id="88f81-227">If there are no pages to display, "Page 0 of 0" is shown.</span></span> <span data-ttu-id="88f81-228">(W tym przypadku jest większa niż liczba stron na numer strony ponieważ `Model.PageNumber` wynosi 1, a `Model.PageCount` wynosi 0.)</span><span class="sxs-lookup"><span data-stu-id="88f81-228">(In that case the page number is greater than the page count because `Model.PageNumber` is 1, and `Model.PageCount` is 0.)</span></span>

   <span data-ttu-id="88f81-229">Przyciski stronicowania są wyświetlane przez `PagedListPager` pomocy:</span><span class="sxs-lookup"><span data-stu-id="88f81-229">The paging buttons are displayed by the `PagedListPager` helper:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   <span data-ttu-id="88f81-230">`PagedListPager` Pomocnik zapewnia kilka opcji, które można dostosować, w tym adresy URL i ustawianie stylów.</span><span class="sxs-lookup"><span data-stu-id="88f81-230">The `PagedListPager` helper provides a number of options that you can customize, including URLs and styling.</span></span> <span data-ttu-id="88f81-231">Aby uzyskać więcej informacji, zobacz [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="88f81-231">For more information, see [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) on the GitHub site.</span></span>

2. <span data-ttu-id="88f81-232">Uruchom stronę.</span><span class="sxs-lookup"><span data-stu-id="88f81-232">Run the page.</span></span>

   <span data-ttu-id="88f81-233">Po kliknięciu łączy stronicowania w różnych sortowania, aby upewnić się, że działa stronicowania.</span><span class="sxs-lookup"><span data-stu-id="88f81-233">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="88f81-234">Następnie wprowadź wyszukiwany ciąg i spróbuj stronicowania ponownie, aby sprawdzić, czy stronicowania również działa poprawnie przy użyciu sortowania i filtrowania.</span><span class="sxs-lookup"><span data-stu-id="88f81-234">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page"></a><span data-ttu-id="88f81-235">Tworzenie strony informacje</span><span class="sxs-lookup"><span data-stu-id="88f81-235">Create an About page</span></span>

<span data-ttu-id="88f81-236">Dla firmy Contoso University witryny internetowej informacje o stronie będą wyświetlane, jak wiele studentów zostały zarejestrowane dla każdego dnia rejestracji.</span><span class="sxs-lookup"><span data-stu-id="88f81-236">For the Contoso University website's About page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="88f81-237">Wymaga to obliczeń grupowania i prostych grup.</span><span class="sxs-lookup"><span data-stu-id="88f81-237">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="88f81-238">Aby to osiągnąć, wykonasz następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="88f81-238">To accomplish this, you'll do the following:</span></span>

- <span data-ttu-id="88f81-239">Utwórz klasę modelu widoku danych potrzebnych do przekazania do widoku.</span><span class="sxs-lookup"><span data-stu-id="88f81-239">Create a view model class for the data that you need to pass to the view.</span></span>
- <span data-ttu-id="88f81-240">Modyfikowanie `About` method in Class metoda `Home` kontrolera.</span><span class="sxs-lookup"><span data-stu-id="88f81-240">Modify the `About` method in the `Home` controller.</span></span>
- <span data-ttu-id="88f81-241">Modyfikowanie `About` widoku.</span><span class="sxs-lookup"><span data-stu-id="88f81-241">Modify the `About` view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="88f81-242">Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="88f81-242">Create the View Model</span></span>

<span data-ttu-id="88f81-243">Tworzenie *modele widoków* folderu w folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="88f81-243">Create a *ViewModels* folder in the project folder.</span></span> <span data-ttu-id="88f81-244">W tym folderze, Dodaj plik klasy *EnrollmentDateGroup.cs* i Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="88f81-244">In that folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="88f81-245">Modyfikowanie kontrolera głównego</span><span class="sxs-lookup"><span data-stu-id="88f81-245">Modify the Home Controller</span></span>

1. <span data-ttu-id="88f81-246">W *HomeController.cs*, Dodaj następujący kod `using` instrukcji w górnej części pliku:</span><span class="sxs-lookup"><span data-stu-id="88f81-246">In *HomeController.cs*, add the following `using` statements at the top of the file:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. <span data-ttu-id="88f81-247">Dodaj zmienną klasy kontekstu bazy danych bezpośrednio po otwierającym nawiasie klamrowym dla klasy:</span><span class="sxs-lookup"><span data-stu-id="88f81-247">Add a class variable for the database context immediately after the opening curly brace for the class:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. <span data-ttu-id="88f81-248">Zastąp `About` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="88f81-248">Replace the `About` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   <span data-ttu-id="88f81-249">Instrukcji LINQ grup jednostek uczniów według daty rejestracji, oblicza liczbę jednostek w każdej grupie i przechowuje wyniki w zbiorze `EnrollmentDateGroup` wyświetlić obiekty w modelu.</span><span class="sxs-lookup"><span data-stu-id="88f81-249">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>

4. <span data-ttu-id="88f81-250">Dodaj `Dispose` metody:</span><span class="sxs-lookup"><span data-stu-id="88f81-250">Add a `Dispose` method:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a><span data-ttu-id="88f81-251">Zmodyfikuj widok — informacje</span><span class="sxs-lookup"><span data-stu-id="88f81-251">Modify the About View</span></span>

1. <span data-ttu-id="88f81-252">Zastąp kod w *Views\Home\About.cshtml* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="88f81-252">Replace the code in the *Views\Home\About.cshtml* file with the following code:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. <span data-ttu-id="88f81-253">Uruchom aplikację, a następnie kliknij przycisk **o** łącza.</span><span class="sxs-lookup"><span data-stu-id="88f81-253">Run the app and click the **About** link.</span></span>

   <span data-ttu-id="88f81-254">Liczba studentów każdej daty rejestracji wyświetla się w tabeli.</span><span class="sxs-lookup"><span data-stu-id="88f81-254">The count of students for each enrollment date displays in a table.</span></span>

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="get-the-code"></a><span data-ttu-id="88f81-256">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="88f81-256">Get the code</span></span>

[<span data-ttu-id="88f81-257">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="88f81-257">Download the Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a><span data-ttu-id="88f81-258">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="88f81-258">Additional resources</span></span>

<span data-ttu-id="88f81-259">Linki do innych zasobów platformy Entity Framework można znaleźć w [dostęp do danych platformy ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="88f81-259">Links to other Entity Framework resources can be found in [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="88f81-260">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="88f81-260">Next steps</span></span>

<span data-ttu-id="88f81-261">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="88f81-261">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="88f81-262">Dodaj kolumnę sortowania łącza</span><span class="sxs-lookup"><span data-stu-id="88f81-262">Add column sort links</span></span>
> * <span data-ttu-id="88f81-263">Dodawanie pola wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="88f81-263">Add a Search box</span></span>
> * <span data-ttu-id="88f81-264">Dodawanie stronicowania</span><span class="sxs-lookup"><span data-stu-id="88f81-264">Add paging</span></span>
> * <span data-ttu-id="88f81-265">Tworzenie strony informacje</span><span class="sxs-lookup"><span data-stu-id="88f81-265">Create an About page</span></span>

<span data-ttu-id="88f81-266">Przejdź do następnego artykułu, aby dowiedzieć się, jak używać połączenia przejmowanie odporność i polecenia.</span><span class="sxs-lookup"><span data-stu-id="88f81-266">Advance to the next article to learn how to use connection resiliency and command interception.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="88f81-267">Przejmowanie odporność i polecenia połączenia</span><span class="sxs-lookup"><span data-stu-id="88f81-267">Connection resiliency and command interception</span></span>](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)