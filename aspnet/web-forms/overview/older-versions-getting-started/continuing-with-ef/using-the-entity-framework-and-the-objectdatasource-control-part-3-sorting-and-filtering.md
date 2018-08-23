---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Korzystając z programu Entity Framework 4.0 i kontrolka ObjectDataSource, część 3: sortowanie i filtrowanie | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków opiera się na aplikacji sieci web firmy Contoso University, utworzony przez wprowadzenie do serii samouczków Entity Framework 4.0. CZY MOGĘ...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 71e26dc9a63131842daf4aa1549d761690b723ea
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755458"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="a4357-104">Korzystając z programu Entity Framework 4.0 i kontrolka ObjectDataSource, część 3: sortowanie i filtrowanie</span><span class="sxs-lookup"><span data-stu-id="a4357-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>
====================
<span data-ttu-id="a4357-105">przez [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a4357-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="a4357-106">W tej serii samouczków jest oparta na Contoso University aplikacji sieci web, który jest tworzony przez [rozpoczęcie korzystania z programu Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serii samouczków.</span><span class="sxs-lookup"><span data-stu-id="a4357-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="a4357-107">Jeśli nie została ukończona wcześniej samouczki, jako punkt początkowy na potrzeby tego samouczka możesz [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) będzie utworzony.</span><span class="sxs-lookup"><span data-stu-id="a4357-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="a4357-108">Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tworzone przez zakończenie serii samouczków.</span><span class="sxs-lookup"><span data-stu-id="a4357-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="a4357-109">Jeśli masz pytania dotyczące samouczków, możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).</span><span class="sxs-lookup"><span data-stu-id="a4357-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>


<span data-ttu-id="a4357-110">W poprzednim samouczku zaimplementowano wzorzec repozytorium w aplikacji n warstwowej w sieci web, która używa programu Entity Framework i `ObjectDataSource` kontroli.</span><span class="sxs-lookup"><span data-stu-id="a4357-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="a4357-111">W tym samouczku pokazano, jak wykonać sortowanie i filtrowanie i obsługiwać scenariusze wzorzec / szczegół.</span><span class="sxs-lookup"><span data-stu-id="a4357-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="a4357-112">Należy dodać następujące rozszerzenia *Departments.aspx* strony:</span><span class="sxs-lookup"><span data-stu-id="a4357-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="a4357-113">Pole tekstowe, aby umożliwić użytkownikom na wybór działów według nazwy.</span><span class="sxs-lookup"><span data-stu-id="a4357-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="a4357-114">Lista kursów dla każdego działu, która jest wyświetlana w siatce.</span><span class="sxs-lookup"><span data-stu-id="a4357-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="a4357-115">Możliwość sortowania, klikając nagłówki kolumn.</span><span class="sxs-lookup"><span data-stu-id="a4357-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="a4357-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a4357-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="a4357-117">Dodanie możliwości do sortowanie kolumn GridView</span><span class="sxs-lookup"><span data-stu-id="a4357-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="a4357-118">Otwórz *Departments.aspx* strony, a następnie dodaj `SortParameterName="sortExpression"` atrybutu `ObjectDataSource` formantu o nazwie `DepartmentsObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="a4357-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="a4357-119">(Później utworzymy `GetDepartments` metody, która przyjmuje parametr o nazwie `sortExpression`.) Kod znaczników dla znacznika otwierającego formantu teraz przypomina poniższy przykład.</span><span class="sxs-lookup"><span data-stu-id="a4357-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="a4357-120">Dodaj `AllowSorting="true"` atrybutu znaczniku otwierającym elementu `GridView` kontroli.</span><span class="sxs-lookup"><span data-stu-id="a4357-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="a4357-121">Kod znaczników dla znacznika otwierającego formantu teraz przypomina poniższy przykład.</span><span class="sxs-lookup"><span data-stu-id="a4357-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="a4357-122">W *Departments.aspx.cs*, ustaw kolejność sortowania domyślnego, wywołując `GridView` kontrolki `Sort` metody z `Page_Load` metody:</span><span class="sxs-lookup"><span data-stu-id="a4357-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="a4357-123">Możesz dodać kod, która sortuje i filtruje klasy logiki biznesowej lub klasę repozytorium.</span><span class="sxs-lookup"><span data-stu-id="a4357-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="a4357-124">Jeśli możesz zrobić w klasie logiki biznesowej, sortowanie lub filtrowanie pracy zostaną wykonane po pobraniu danych z bazy danych, ponieważ klasa logika biznesowa jest praca z `IEnumerable` obiektu zwróconego przez repozytorium.</span><span class="sxs-lookup"><span data-stu-id="a4357-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="a4357-125">Jeśli dodasz, sortowanie i filtrowanie kod w klasie repozytorium i zrobisz to przed wyrażenie LINQ lub zapytanie obiektów został przekonwertowany na `IEnumerable` obiektu poleceń będą przekazywane do bazy danych do przetwarzania, co jest zazwyczaj bardziej wydajne.</span><span class="sxs-lookup"><span data-stu-id="a4357-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="a4357-126">W tym samouczku możesz wdrożyć sortowanie i filtrowanie w sposób, który powoduje, że przetwarzanie do wykonania przez bazę danych — czyli w repozytorium.</span><span class="sxs-lookup"><span data-stu-id="a4357-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="a4357-127">Aby dodać możliwość sortowania, należy dodać nową metodę do klasy repozytorium, jak również co klasa logika biznesowa i repozytorium interfejsu.</span><span class="sxs-lookup"><span data-stu-id="a4357-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="a4357-128">W *ISchoolRepository.cs* Dodaj nową `GetDepartments` metody, która przyjmuje `sortExpression` parametr, który będzie używany do sortować listę działów, który jest zwracany:</span><span class="sxs-lookup"><span data-stu-id="a4357-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="a4357-129">`sortExpression` Parametr będzie określać kolumnę do sortowania i kierunek sortowania.</span><span class="sxs-lookup"><span data-stu-id="a4357-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="a4357-130">Dodaj kod do nowej metody do *SchoolRepository.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="a4357-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="a4357-131">Zmień istniejące bez parametrów `GetDepartments` metodę, aby wywołać nową metodę:</span><span class="sxs-lookup"><span data-stu-id="a4357-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="a4357-132">W projekcie testowym Dodaj następującą nową metodę do *MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="a4357-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="a4357-133">Jeśli zostały zamierza utworzyć wszelkie testy jednostkowe, które zależą od ta metoda zwraca posortowaną listę, będziesz potrzebować listę można sortować przed zwróceniem.</span><span class="sxs-lookup"><span data-stu-id="a4357-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="a4357-134">Użytkownik nie będzie można tworzenie testów Krewny, który w ramach tego samouczka, więc metoda po prostu może zwrócić listę nieposortowane działów.</span><span class="sxs-lookup"><span data-stu-id="a4357-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="a4357-135">W *SchoolBL.cs* plików, dodaj następującą nową metodę do klasy logiki biznesowej:</span><span class="sxs-lookup"><span data-stu-id="a4357-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="a4357-136">Ten kod przekazuje parametr sortowania do metody repozytorium.</span><span class="sxs-lookup"><span data-stu-id="a4357-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="a4357-137">Uruchom *Departments.aspx* strony.</span><span class="sxs-lookup"><span data-stu-id="a4357-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="a4357-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a4357-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="a4357-139">Teraz możesz kliknąć dowolny nagłówek kolumny, aby posortować według tej kolumny.</span><span class="sxs-lookup"><span data-stu-id="a4357-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="a4357-140">Jeśli kolumna jest już sortowana, klikając nagłówek odwraca kierunek sortowania.</span><span class="sxs-lookup"><span data-stu-id="a4357-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="a4357-141">Dodawanie pola wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="a4357-141">Adding a Search Box</span></span>

<span data-ttu-id="a4357-142">W tej sekcji będzie dodać pole tekstowe wyszukiwania, połączenie jej z `ObjectDataSource` sterować za pomocą parametru formant i dodać metodę do klasy logiki biznesowej, aby obsługiwać filtrowanie.</span><span class="sxs-lookup"><span data-stu-id="a4357-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="a4357-143">Otwórz *Departments.aspx* strony, a następnie dodaj następujący kod między nagłówkiem i pierwszy `ObjectDataSource` sterowania:</span><span class="sxs-lookup"><span data-stu-id="a4357-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="a4357-144">W `ObjectDataSource` formantu o nazwie `DepartmentsObjectDataSource`, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="a4357-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="a4357-145">Dodaj `SelectParameters` elementu dla parametru o nazwie `nameSearchString` , które pobiera wartość wprowadzona w `SearchTextBox` kontroli.</span><span class="sxs-lookup"><span data-stu-id="a4357-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="a4357-146">Zmiana `SelectMethod` wartość do atrybutu `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="a4357-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="a4357-147">(W dalszej części opisano tworzenie tej metody.)</span><span class="sxs-lookup"><span data-stu-id="a4357-147">(You'll create this method later.)</span></span>

<span data-ttu-id="a4357-148">Kod znaczników dla `ObjectDataSource` kontroli teraz podobnego do następującego:</span><span class="sxs-lookup"><span data-stu-id="a4357-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="a4357-149">W *ISchoolRepository.cs*, Dodaj `GetDepartmentsByName` metody, która przyjmuje zarówno `sortExpression` i `nameSearchString` parametry:</span><span class="sxs-lookup"><span data-stu-id="a4357-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="a4357-150">W *SchoolRepository.cs*, dodaj następującą nową metodę:</span><span class="sxs-lookup"><span data-stu-id="a4357-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="a4357-151">Ten kod używa `Where` metodę, aby wybrać elementy zawierające ciąg wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="a4357-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="a4357-152">Jeśli ciąg wyszukiwania jest pusta, zostanie wybrana wszystkie rekordy.</span><span class="sxs-lookup"><span data-stu-id="a4357-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="a4357-153">Należy pamiętać, że po określeniu wywołania metod razem w jednej instrukcji w następujący sposób (`Include`, następnie `OrderBy`, następnie `Where`), `Where` metoda zawsze musi być ostatni.</span><span class="sxs-lookup"><span data-stu-id="a4357-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="a4357-154">Zmień istniejące `GetDepartments` metody, która przyjmuje `sortExpression` parametru, aby wywołać nową metodę:</span><span class="sxs-lookup"><span data-stu-id="a4357-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="a4357-155">W *MockSchoolRepository.cs* w projekcie testowym Dodaj następującą nową metodę:</span><span class="sxs-lookup"><span data-stu-id="a4357-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="a4357-156">W *SchoolBL.cs*, dodaj następującą nową metodę:</span><span class="sxs-lookup"><span data-stu-id="a4357-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="a4357-157">Uruchom *Departments.aspx* stronie, a następnie wprowadź ciąg wyszukiwania, aby upewnić się, że działa logikę wyboru.</span><span class="sxs-lookup"><span data-stu-id="a4357-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="a4357-158">Pozostaw puste pole tekstowe, a następnie spróbuj wyszukiwania, aby upewnić się, że zwracane są wszystkie rekordy.</span><span class="sxs-lookup"><span data-stu-id="a4357-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="a4357-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a4357-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="a4357-160">Dodawanie kolumny szczegółów dla każdego wiersza siatki</span><span class="sxs-lookup"><span data-stu-id="a4357-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="a4357-161">Następnie użytkownik chce zobaczyć wszystkie kursy dla każdego działu wyświetlana w komórce po prawej stronie siatki.</span><span class="sxs-lookup"><span data-stu-id="a4357-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="a4357-162">Aby to zrobić, użyjesz zagnieżdżoną `GridView` kontroli i powiązać z danymi do danych z `Courses` właściwość nawigacji `Department` jednostki.</span><span class="sxs-lookup"><span data-stu-id="a4357-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="a4357-163">Otwórz *Departments.aspx* w znaczniki dla `GridView` kontrolować, określ program obsługi dla `RowDataBound` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="a4357-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="a4357-164">Kod znaczników dla znacznika otwierającego formantu teraz przypomina poniższy przykład.</span><span class="sxs-lookup"><span data-stu-id="a4357-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="a4357-165">Dodaj nową `TemplateField` elementu po `Administrator` pole szablonu:</span><span class="sxs-lookup"><span data-stu-id="a4357-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="a4357-166">Ten kod znaczników tworzy zagnieżdżoną `GridView` kontrolka, która pokazuje liczbę kurs i tytuły listę kursów.</span><span class="sxs-lookup"><span data-stu-id="a4357-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="a4357-167">Nie określa źródło danych, ponieważ należy powiązać z danymi w kodzie w `RowDataBound` programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="a4357-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="a4357-168">Otwórz *Departments.aspx.cs* i Dodaj następujący program obsługi dla `RowDataBound` zdarzeń:</span><span class="sxs-lookup"><span data-stu-id="a4357-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="a4357-169">Ten kod pobiera `Department` jednostki na podstawie argumentów zdarzeń, konwertuje `Courses` właściwość nawigacji do `List` zbierania i databinds zagnieżdżonego `GridView` do kolekcji.</span><span class="sxs-lookup"><span data-stu-id="a4357-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="a4357-170">Otwórz *SchoolRepository.cs* pliku i określić wczesne ładowanie dla `Courses` właściwość nawigacji, wywołując `Include` metody w zapytaniu obiektu, który zostanie utworzony w `GetDepartmentsByName` metody.</span><span class="sxs-lookup"><span data-stu-id="a4357-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="a4357-171">`return` Instrukcji w `GetDepartmentsByName` metoda teraz przypomina poniższy przykład.</span><span class="sxs-lookup"><span data-stu-id="a4357-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="a4357-172">Uruchom stronę.</span><span class="sxs-lookup"><span data-stu-id="a4357-172">Run the page.</span></span> <span data-ttu-id="a4357-173">Oprócz sortowania i filtrowania możliwości, który dodano wcześniej kontrolki GridView zawiera szczegóły dotyczące zagnieżdżonych kursu dla każdego działu.</span><span class="sxs-lookup"><span data-stu-id="a4357-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="a4357-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a4357-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="a4357-175">Na tym kończy się wprowadzenie do filtrowania, sortowania i wzorzec / szczegół scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="a4357-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="a4357-176">W następnym samouczku pojawi się, jak obsługiwać współbieżności.</span><span class="sxs-lookup"><span data-stu-id="a4357-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a4357-177">[Poprzednie](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [dalej](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="a4357-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
