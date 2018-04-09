---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Przy użyciu programu Entity Framework 4.0 i kontrolki ObjectDataSource, część 3: filtrowanie i sortowanie | Dokumentacja firmy Microsoft'
author: tdykstra
description: Ten samouczek serii opiera się na aplikację sieci web Contoso University jest tworzony przez wprowadzenie do samouczka serii Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: e412d3ad98a37931e7190a4909cb09fa2abfb3d0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="7a0de-104">Przy użyciu programu Entity Framework 4.0 i kontrolki ObjectDataSource, część 3: filtrowanie i sortowanie</span><span class="sxs-lookup"><span data-stu-id="7a0de-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>
====================
<span data-ttu-id="7a0de-105">Przez [Dykstra niestandardowy](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7a0de-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="7a0de-106">Ten samouczek serii opiera się na aplikację sieci web Contoso University jest tworzony przez [wprowadzenie do korzystania z programu Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) samouczka serii.</span><span class="sxs-lookup"><span data-stu-id="7a0de-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="7a0de-107">Jeśli nie została ukończona wcześniejszych samouczki, jako punkt początkowy dla tego samouczka możesz [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) będzie utworzony.</span><span class="sxs-lookup"><span data-stu-id="7a0de-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="7a0de-108">Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tworzone przez zakończenie samouczka serii.</span><span class="sxs-lookup"><span data-stu-id="7a0de-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="7a0de-109">Jeśli masz pytania dotyczące samouczków, możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).</span><span class="sxs-lookup"><span data-stu-id="7a0de-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>


<span data-ttu-id="7a0de-110">W poprzednich instrukcji zaimplementowana wzorca repozytorium w aplikacji sieci web n warstwowa korzystającej z programu Entity Framework i `ObjectDataSource` formantu.</span><span class="sxs-lookup"><span data-stu-id="7a0de-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="7a0de-111">W tym samouczku pokazano, jak wykonać sortowania i filtrowania i obsługiwać scenariusze główny szczegółowy.</span><span class="sxs-lookup"><span data-stu-id="7a0de-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="7a0de-112">Należy dodać następujące rozszerzenia *Departments.aspx* strony:</span><span class="sxs-lookup"><span data-stu-id="7a0de-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="7a0de-113">Pole tekstowe, aby zezwolić użytkownikom na wybór działów według nazwy.</span><span class="sxs-lookup"><span data-stu-id="7a0de-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="7a0de-114">Lista szkoleń dla każdego działu, który jest widoczny w siatce.</span><span class="sxs-lookup"><span data-stu-id="7a0de-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="7a0de-115">Możliwość sortować, klikając nagłówki kolumn.</span><span class="sxs-lookup"><span data-stu-id="7a0de-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="7a0de-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7a0de-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="7a0de-117">Dodawanie funkcji do sortowania kolumn widoku GridView</span><span class="sxs-lookup"><span data-stu-id="7a0de-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="7a0de-118">Otwórz *Departments.aspx* i dodać `SortParameterName="sortExpression"` atrybutu `ObjectDataSource` formantu o nazwie `DepartmentsObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="7a0de-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="7a0de-119">(Później utworzysz `GetDepartments` metody pobierającej parametr o nazwie `sortExpression`.) Kod znaczników dla otwierający tag formantu teraz podobnego do następującego.</span><span class="sxs-lookup"><span data-stu-id="7a0de-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="7a0de-120">Dodaj `AllowSorting="true"` atrybut do tagu otwierającego `GridView` formantu.</span><span class="sxs-lookup"><span data-stu-id="7a0de-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="7a0de-121">Kod znaczników dla otwierający tag formantu teraz podobnego do następującego.</span><span class="sxs-lookup"><span data-stu-id="7a0de-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="7a0de-122">W *Departments.aspx.cs*, Ustaw domyślną kolejność sortowania, wywołując `GridView` formantu `Sort` metody z `Page_Load` metody:</span><span class="sxs-lookup"><span data-stu-id="7a0de-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="7a0de-123">W klasie logiki biznesowej lub klasę repozytorium można dodać kodu sortuje lub filtrów.</span><span class="sxs-lookup"><span data-stu-id="7a0de-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="7a0de-124">Jeśli chcesz w klasie logiki biznesowej, sortowanie lub filtrowanie zostanie wykonane po dane są pobierane z bazy danych, ponieważ klasa logika biznesowa jest praca z `IEnumerable` obiektu zwróconego przez repozytorium.</span><span class="sxs-lookup"><span data-stu-id="7a0de-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="7a0de-125">Jeśli dodasz sortowania i filtrowania kod w klasie repozytorium i to zrobić przez wyrażenie LINQ lub zapytanie obiektu został przekonwertowany na `IEnumerable` obiekt poleceniach będą przekazywane do bazy danych do przetwarzania, który jest zazwyczaj bardziej wydajne.</span><span class="sxs-lookup"><span data-stu-id="7a0de-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="7a0de-126">W tym samouczku można zaimplementować sortowanie i filtrowanie w taki sposób, który powoduje, że przetwarzanie do wykonania przez bazę danych, to znaczy w repozytorium.</span><span class="sxs-lookup"><span data-stu-id="7a0de-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="7a0de-127">Można dodać możliwości sortowania, możesz dodać nową metodę interfejsu repozytorium i klasy repozytorium, jak również dotyczące klasy logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="7a0de-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="7a0de-128">W *ISchoolRepository.cs* plików, Dodaj nową `GetDepartments` metody pobierającej `sortExpression` parametr, który będzie służyć do sortowania Lista działów, która jest zwracana:</span><span class="sxs-lookup"><span data-stu-id="7a0de-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="7a0de-129">`sortExpression` Parametr będzie określać kolumnę do sortowania i kierunek sortowania.</span><span class="sxs-lookup"><span data-stu-id="7a0de-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="7a0de-130">Dodaj kod dla nowej metody, aby *SchoolRepository.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="7a0de-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="7a0de-131">Zmień istniejące bez parametrów `GetDepartments` metodę do wywołania nowej metody:</span><span class="sxs-lookup"><span data-stu-id="7a0de-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="7a0de-132">W projekcie testowym, dodaj następującą metodę nowe do *MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="7a0de-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="7a0de-133">Jeśli zostały chce utworzyć żadnych testów, które są zależne od ta metoda zwraca posortowaną listę, konieczne będzie listę można sortować przed zwróceniem.</span><span class="sxs-lookup"><span data-stu-id="7a0de-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="7a0de-134">Nie można utworzeniem testy tak jak w tym samouczku, metoda może zwracać tylko nieposortowane listę działów.</span><span class="sxs-lookup"><span data-stu-id="7a0de-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="7a0de-135">W *SchoolBL.cs* plików, dodaj następującą metodę nowej klasy logiki biznesowej:</span><span class="sxs-lookup"><span data-stu-id="7a0de-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="7a0de-136">Ten kod przekazuje sortowania parametru do metody repozytorium.</span><span class="sxs-lookup"><span data-stu-id="7a0de-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="7a0de-137">Uruchom *Departments.aspx* strony.</span><span class="sxs-lookup"><span data-stu-id="7a0de-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="7a0de-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="7a0de-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="7a0de-139">Można teraz kliknij nagłówek dowolnej kolumny, aby sortować według tej kolumny.</span><span class="sxs-lookup"><span data-stu-id="7a0de-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="7a0de-140">Jeśli już jest sortowana kolumna, klikając nagłówek odwraca kierunek sortowania.</span><span class="sxs-lookup"><span data-stu-id="7a0de-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="7a0de-141">Dodawanie pola wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="7a0de-141">Adding a Search Box</span></span>

<span data-ttu-id="7a0de-142">W tej sekcji będzie dodanie pola tekstowego wyszukiwania, połącz go do `ObjectDataSource` kontrolować za pomocą parametru kontroli i Dodawanie metody do klasy logiki biznesowej w celu obsługuje filtrowania.</span><span class="sxs-lookup"><span data-stu-id="7a0de-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="7a0de-143">Otwórz *Departments.aspx* strony i Dodaj następujący kod między nagłówek i pierwszy `ObjectDataSource` sterowania:</span><span class="sxs-lookup"><span data-stu-id="7a0de-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="7a0de-144">W `ObjectDataSource` formantu o nazwie `DepartmentsObjectDataSource`, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="7a0de-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="7a0de-145">Dodaj `SelectParameters` elementu dla parametru o nazwie `nameSearchString` który pobiera wartość wprowadzona w `SearchTextBox` formantu.</span><span class="sxs-lookup"><span data-stu-id="7a0de-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="7a0de-146">Zmień `SelectMethod` wartość do atrybutu `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="7a0de-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="7a0de-147">(Utworzysz tej metody później.)</span><span class="sxs-lookup"><span data-stu-id="7a0de-147">(You'll create this method later.)</span></span>

<span data-ttu-id="7a0de-148">Kod znaczników dla `ObjectDataSource` kontroli teraz podobnego do następującego:</span><span class="sxs-lookup"><span data-stu-id="7a0de-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="7a0de-149">W *ISchoolRepository.cs*, Dodaj `GetDepartmentsByName` metody pobierającej zarówno `sortExpression` i `nameSearchString` parametry:</span><span class="sxs-lookup"><span data-stu-id="7a0de-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="7a0de-150">W *SchoolRepository.cs*, dodaj następującą metodę nowe:</span><span class="sxs-lookup"><span data-stu-id="7a0de-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="7a0de-151">Ten kod zawiera `Where` metody, aby zaznaczyć elementy, które zawierają ciąg wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="7a0de-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="7a0de-152">Jeśli ciąg wyszukiwania jest pusta, zostanie wybrana wszystkie rekordy.</span><span class="sxs-lookup"><span data-stu-id="7a0de-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="7a0de-153">Należy pamiętać, że po określeniu wywołania metod razem w jednej instrukcji następująco (`Include`, następnie `OrderBy`, następnie `Where`), `Where` metody musi zawsze występować na końcu.</span><span class="sxs-lookup"><span data-stu-id="7a0de-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="7a0de-154">Zmień istniejące `GetDepartments` metody pobierającej `sortExpression` parametr, aby wywołać metodę nowe:</span><span class="sxs-lookup"><span data-stu-id="7a0de-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="7a0de-155">W *MockSchoolRepository.cs* projektu testowego, dodaj następującą metodę nowe:</span><span class="sxs-lookup"><span data-stu-id="7a0de-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="7a0de-156">W *SchoolBL.cs*, dodaj następującą metodę nowe:</span><span class="sxs-lookup"><span data-stu-id="7a0de-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="7a0de-157">Uruchom *Departments.aspx* strony, a następnie wprowadź ciąg wyszukiwania, aby upewnić się, że logika wybór działa.</span><span class="sxs-lookup"><span data-stu-id="7a0de-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="7a0de-158">Pozostaw puste pole tekstowe, a następnie spróbuj wyszukiwania, aby upewnić się, że zwracane są wszystkie rekordy.</span><span class="sxs-lookup"><span data-stu-id="7a0de-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="7a0de-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="7a0de-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="7a0de-160">Dodawanie kolumny szczegółów dla każdego wiersza siatki</span><span class="sxs-lookup"><span data-stu-id="7a0de-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="7a0de-161">Następnie chcesz zobaczyć wszystkie szkoleń dla każdego działu wyświetlany w prawej komórki siatki.</span><span class="sxs-lookup"><span data-stu-id="7a0de-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="7a0de-162">Aby to zrobić, użyjesz zagnieżdżoną `GridView` databind i kontroli do danych z `Courses` właściwość nawigacji `Department` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7a0de-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="7a0de-163">Otwórz *Departments.aspx* i w znaczniku dla `GridView` kontrolować, określ obsługi dla `RowDataBound` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="7a0de-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="7a0de-164">Kod znaczników dla otwierający tag formantu teraz podobnego do następującego.</span><span class="sxs-lookup"><span data-stu-id="7a0de-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="7a0de-165">Dodaj nową `TemplateField` elementu po `Administrator` pole szablonu:</span><span class="sxs-lookup"><span data-stu-id="7a0de-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="7a0de-166">Ten kod znaczników tworzy zagnieżdżoną `GridView` formant, który zawiera kursu numer i tytuł listę kursów.</span><span class="sxs-lookup"><span data-stu-id="7a0de-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="7a0de-167">Nie określa źródło danych, ponieważ będzie databind w kodzie w `RowDataBound` programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="7a0de-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="7a0de-168">Otwórz *Departments.aspx.cs* i dodaj następujące obsługę `RowDataBound` zdarzeń:</span><span class="sxs-lookup"><span data-stu-id="7a0de-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="7a0de-169">Ten kod pobiera `Department` konwertuje jednostki w argumentach zdarzenia `Courses` właściwości nawigacji do `List` kolekcji, a zagnieżdżone databinds `GridView` do kolekcji.</span><span class="sxs-lookup"><span data-stu-id="7a0de-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="7a0de-170">Otwórz *SchoolRepository.cs* pliku i określ wczesny ładowania dla `Courses` właściwość nawigacji, wywołując `Include` metody w zapytaniu obiektów, które są tworzone w `GetDepartmentsByName` — metoda.</span><span class="sxs-lookup"><span data-stu-id="7a0de-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="7a0de-171">`return` Instrukcji w `GetDepartmentsByName` metody teraz podobnego do następującego.</span><span class="sxs-lookup"><span data-stu-id="7a0de-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="7a0de-172">Uruchom strony.</span><span class="sxs-lookup"><span data-stu-id="7a0de-172">Run the page.</span></span> <span data-ttu-id="7a0de-173">Oprócz sortowania i filtrowania możliwości dodanego wcześniej kontrolki widoku siatki zawiera szczegóły dotyczące zagnieżdżonych kursu dla każdego działu.</span><span class="sxs-lookup"><span data-stu-id="7a0de-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="7a0de-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="7a0de-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="7a0de-175">Na tym kończy się wprowadzenie do filtrowania, sortowania i wzorzec szczegół scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="7a0de-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="7a0de-176">W następnym samouczku zobaczysz sposób obsługi współbieżności.</span><span class="sxs-lookup"><span data-stu-id="7a0de-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7a0de-177">[Poprzednie](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [dalej](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="7a0de-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
