---
title: Platforma ASP.NET Core MVC z programem EF Core — odczytywanie powiązanych danych - 6 10
author: rick-anderson
description: W tym samouczku należy przeczytać i wyświetlanie powiązanych danych — oznacza to, że dane programu Entity Framework wczytywane właściwości nawigacji.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: d5c9b665a80003ef5029754d7ad1780b3254e97e
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38216297"
---
# <a name="aspnet-core-mvc-with-ef-core---read-related-data---6-of-10"></a><span data-ttu-id="612d9-103">Platforma ASP.NET Core MVC z programem EF Core — odczytywanie powiązanych danych - 6 10</span><span class="sxs-lookup"><span data-stu-id="612d9-103">ASP.NET Core MVC with EF Core - Read Related Data - 6 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="612d9-104">Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="612d9-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="612d9-105">Przykładową aplikację sieci web firmy Contoso University pokazuje, jak tworzyć aplikacje sieci web platformy ASP.NET Core MVC za pomocą platformy Entity Framework Core i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="612d9-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="612d9-106">Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](intro.md).</span><span class="sxs-lookup"><span data-stu-id="612d9-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="612d9-107">W poprzednim samouczku można wykonać modelu danych służbowych.</span><span class="sxs-lookup"><span data-stu-id="612d9-107">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="612d9-108">W tym samouczku należy przeczytać i wyświetlanie powiązanych danych — oznacza to, że dane programu Entity Framework wczytywane właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="612d9-108">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="612d9-109">Na poniższych ilustracjach przedstawiono strony, którą będziesz pracować.</span><span class="sxs-lookup"><span data-stu-id="612d9-109">The following illustrations show the pages that you'll work with.</span></span>

![Kursy strony indeksu](read-related-data/_static/courses-index.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="612d9-112">Zapoznamy wyraźne i powolne ładowanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="612d9-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="612d9-113">Istnieje kilka sposobów oprogramowanie obiektowo-relacyjny mapowanie (ORM), takie jak Entity Framework można załadować powiązane dane do właściwości nawigacji jednostki:</span><span class="sxs-lookup"><span data-stu-id="612d9-113">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="612d9-114">Wczesne ładowanie.</span><span class="sxs-lookup"><span data-stu-id="612d9-114">Eager loading.</span></span> <span data-ttu-id="612d9-115">Podczas odczytywania jednostki powiązane dane są pobierane wraz z jej.</span><span class="sxs-lookup"><span data-stu-id="612d9-115">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="612d9-116">Powoduje to zwykle w zapytaniu sprzężenia jednego, który pobiera wszystkie dane potrzebne.</span><span class="sxs-lookup"><span data-stu-id="612d9-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="612d9-117">Możesz określić wczesne ładowanie w Entity Framework Core przy użyciu `Include` i `ThenInclude` metody.</span><span class="sxs-lookup"><span data-stu-id="612d9-117">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![Przykład wczesne ładowanie](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="612d9-119">Możesz pobrać dane w oddzielne zapytania, a EF termin "poprawki" właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="612d9-119">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="612d9-120">Oznacza to EF automatycznie doda jednostki pobrane oddzielnie, których one należą do właściwości nawigacji jednostek wcześniej pobrane.</span><span class="sxs-lookup"><span data-stu-id="612d9-120">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="612d9-121">Dla zapytania, które pobiera dane powiązane, możesz użyć `Load` metody zamiast metody, która zwraca listę lub obiektów, takich jak `ToList` lub `Single`.</span><span class="sxs-lookup"><span data-stu-id="612d9-121">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![Przykład oddzielne zapytania](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="612d9-123">Jawne ładowanie.</span><span class="sxs-lookup"><span data-stu-id="612d9-123">Explicit loading.</span></span> <span data-ttu-id="612d9-124">Podczas odczytywania jednostki powiązane dane nie są pobierane.</span><span class="sxs-lookup"><span data-stu-id="612d9-124">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="612d9-125">Jeśli jest konieczne, piszesz kod, który pobiera powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="612d9-125">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="612d9-126">Tak jak w przypadku eager ładowanie za pomocą oddzielne zapytania jawne ładowanie wyników w wielu zapytań wysyłanych do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="612d9-126">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="612d9-127">Różnica polega na tym, za pomocą jawne ładowanie kod określa właściwości nawigacji, aby go załadować.</span><span class="sxs-lookup"><span data-stu-id="612d9-127">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="612d9-128">Entity Framework Core 1.1 umożliwia `Load` celu jawne ładowanie metodę.</span><span class="sxs-lookup"><span data-stu-id="612d9-128">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="612d9-129">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="612d9-129">For example:</span></span>

  ![Przykład jawne ładowanie](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="612d9-131">Ładowanie z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="612d9-131">Lazy loading.</span></span> <span data-ttu-id="612d9-132">Podczas odczytywania jednostki powiązane dane nie są pobierane.</span><span class="sxs-lookup"><span data-stu-id="612d9-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="612d9-133">Jednak użytkownik podejmie próbę dostępu do właściwości nawigacji po raz pierwszy wymagane dane dla tej właściwości nawigacji jest automatycznie pobierany.</span><span class="sxs-lookup"><span data-stu-id="612d9-133">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="612d9-134">Zapytanie jest wysyłane do bazy danych zawsze próbować pobierać dane z właściwości nawigacji, po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="612d9-134">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="612d9-135">Entity Framework Core 1.0 nie obsługuje ładowania z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="612d9-135">Entity Framework Core 1.0 doesn't support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="612d9-136">Zagadnienia dotyczące wydajności</span><span class="sxs-lookup"><span data-stu-id="612d9-136">Performance considerations</span></span>

<span data-ttu-id="612d9-137">Jeśli znasz powiązane dane potrzebne dla każdej jednostki pobrać eager podczas ładowania często oferuje najlepszą wydajność, ponieważ pojedynczego zapytania wysłanego do bazy danych jest zazwyczaj bardziej efektywne niż oddzielne zapytania, dla każdej jednostki pobierane.</span><span class="sxs-lookup"><span data-stu-id="612d9-137">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="612d9-138">Na przykład załóżmy, że każdy dział ma dziesięć Kursy pokrewne.</span><span class="sxs-lookup"><span data-stu-id="612d9-138">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="612d9-139">Wczesne ładowanie wszystkich powiązanych danych mogłoby spowodować tylko zapytania do jednego (Połącz) i jeden obiegu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="612d9-139">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="612d9-140">Oddzielnego zapytania dla kursy dla każdego działu mogłoby spowodować jedenaście rund do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="612d9-140">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="612d9-141">Dodatkowe rund do bazy danych są szczególnie szkodliwa dla wydajności, gdy opóźnienie jest wysokie.</span><span class="sxs-lookup"><span data-stu-id="612d9-141">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="612d9-142">Z drugiej strony w niektórych scenariuszach oddzielne zapytania jest bardziej wydajne.</span><span class="sxs-lookup"><span data-stu-id="612d9-142">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="612d9-143">Wczesne ładowanie wszystkich powiązanych danych w jednym zapytaniu może spowodować bardzo złożone sprzężenia zostanie wygenerowany, której program SQL Server nie może przetworzyć wydajnie.</span><span class="sxs-lookup"><span data-stu-id="612d9-143">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="612d9-144">Lub jeśli potrzebujesz dostępu do właściwości nawigacji jednostki, tylko dla podzbioru zestaw jednostek, które one przetwarzanie, oddzielne zapytania może działać lepiej ponieważ wczesne ładowanie wszystkich elementów na początku spowoduje pobieranie większej ilości danych niż jest Ci potrzebne.</span><span class="sxs-lookup"><span data-stu-id="612d9-144">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="612d9-145">Jeśli wydajność ma kluczowe znaczenie, najlepiej testowania wydajności w obu kierunkach, aby można było dokonanie najlepszego wyboru.</span><span class="sxs-lookup"><span data-stu-id="612d9-145">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="612d9-146">Tworzenie strony kursów, który wyświetla nazwy działu</span><span class="sxs-lookup"><span data-stu-id="612d9-146">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="612d9-147">Jednostki kurs zawiera właściwość nawigacji, która zawiera jednostkę działu działu, przypisana do kursu.</span><span class="sxs-lookup"><span data-stu-id="612d9-147">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="612d9-148">Aby wyświetlić nazwę działu przypisany na liście kursy, musisz pobrać właściwości nazwy jednostki działu, która znajduje się w `Course.Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="612d9-148">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that's in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="612d9-149">Utworzyć kontroler o nazwie CoursesController dla typu jednostki kurs przy użyciu tych samych opcji dla **kontroler MVC z widokami używający narzędzia Entity Framework** Generator szkieletu wykonanego wcześniej dla kontrolera studentów, jak pokazano w na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="612d9-149">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![Dodawanie kontrolera kursów](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="612d9-151">Otwórz *CoursesController.cs* i zbadaj `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="612d9-151">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="612d9-152">Automatyczne tworzenie szkieletów określono wczesne ładowanie dla `Department` właściwość nawigacji za pomocą `Include` metody.</span><span class="sxs-lookup"><span data-stu-id="612d9-152">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="612d9-153">Zastąp `Index` metoda następującym kodem, który używa bardziej odpowiednie nazwy dla `IQueryable` zwracającego kurs jednostki (`courses` zamiast `schoolContext`):</span><span class="sxs-lookup"><span data-stu-id="612d9-153">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="612d9-154">Otwórz *Views/Courses/Index.cshtml* i Zastąp kod szablonu poniższym kodem.</span><span class="sxs-lookup"><span data-stu-id="612d9-154">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="612d9-155">Zmiany są wyróżnione:</span><span class="sxs-lookup"><span data-stu-id="612d9-155">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="612d9-156">Utworzony szkielet kodu zostały wprowadzone następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="612d9-156">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="612d9-157">Zmienić nagłówek z indeksu do kursów.</span><span class="sxs-lookup"><span data-stu-id="612d9-157">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="612d9-158">Dodano **numer** kolumna, która pokazuje `CourseID` wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="612d9-158">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="612d9-159">Domyślnie klucze podstawowe nie są szkieletu, ponieważ zwykle są bez znaczenia dla użytkowników końcowych.</span><span class="sxs-lookup"><span data-stu-id="612d9-159">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="612d9-160">Jednak w takim przypadku klucz podstawowy ma znaczenie i chcą ją wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="612d9-160">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="612d9-161">Zmienione **działu** kolumny, aby wyświetlić nazwę działu.</span><span class="sxs-lookup"><span data-stu-id="612d9-161">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="612d9-162">Ten kod wyświetla `Name` właściwości jednostki działu, który jest ładowany do `Department` właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="612d9-162">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="612d9-163">Uruchom aplikację i wybierz **kursów** kartę, aby wyświetlić listę nazw działów.</span><span class="sxs-lookup"><span data-stu-id="612d9-163">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kursy strony indeksu](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="612d9-165">Utwórz stronę instruktorów, pokazujący kursów i rejestracji</span><span class="sxs-lookup"><span data-stu-id="612d9-165">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="612d9-166">W tej sekcji utworzysz kontroler i widok dla jednostki przez instruktorów, aby wyświetlić stronę Instruktorzy:</span><span class="sxs-lookup"><span data-stu-id="612d9-166">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

<span data-ttu-id="612d9-168">Ta strona odczytuje i wyświetla powiązanych danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="612d9-168">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="612d9-169">Lista Instruktorzy wyświetli powiązane dane z jednostki OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="612d9-169">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="612d9-170">Jednostki przez instruktorów i OfficeAssignment znajdują się w relacji jeden do zero lub jeden.</span><span class="sxs-lookup"><span data-stu-id="612d9-170">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="612d9-171">Wczesne ładowanie użyjemy dla jednostek OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="612d9-171">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="612d9-172">Jak wyjaśniono wcześniej, wczesne ładowanie jest zazwyczaj bardziej efektywne, gdy będziesz potrzebować powiązanych danych we wszystkich wierszach pobrane w tabeli podstawowej.</span><span class="sxs-lookup"><span data-stu-id="612d9-172">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="612d9-173">W tym przypadku chcesz wyświetlić przypisania pakietu office dla wszystkich wyświetlanych instruktorów.</span><span class="sxs-lookup"><span data-stu-id="612d9-173">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="612d9-174">Gdy użytkownik wybierze pod kierunkiem instruktora, są wyświetlane z powiązanymi obiektami kursu.</span><span class="sxs-lookup"><span data-stu-id="612d9-174">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="612d9-175">Jednostki przez instruktorów i kurs znajdują się w relacji wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="612d9-175">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="612d9-176">Wczesne ładowanie służący do celów kursu jednostek i ich powiązanych jednostek działu.</span><span class="sxs-lookup"><span data-stu-id="612d9-176">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="612d9-177">W tym przypadku oddzielne zapytania może być bardziej efektywne, ponieważ należy kursy tylko dla wybranych przez instruktorów.</span><span class="sxs-lookup"><span data-stu-id="612d9-177">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="612d9-178">Jednak w tym przykładzie pokazano, jak używać wczesne ładowanie dla właściwości nawigacji w ramach jednostek, które znajdują się w oknie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="612d9-178">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="612d9-179">Gdy użytkownik wybierze kurs, dane związane z zestawu jednostek rejestracji jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="612d9-179">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="612d9-180">Jednostki kurs i rejestracji znajdują się w relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="612d9-180">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="612d9-181">Oddzielne zapytania służący do celów rejestracji jednostek i ich powiązanych jednostek dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="612d9-181">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="612d9-182">Tworzenie modelu widoku dla widoku indeksu przez instruktorów</span><span class="sxs-lookup"><span data-stu-id="612d9-182">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="612d9-183">Na stronie Instruktorzy wyświetlane dane z trzech różnych tabelach.</span><span class="sxs-lookup"><span data-stu-id="612d9-183">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="612d9-184">W związku z tym utworzysz zawiera trzy właściwości zawierający dane dla jednej z tabel, model widoku.</span><span class="sxs-lookup"><span data-stu-id="612d9-184">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="612d9-185">W *SchoolViewModels* folderze utwórz *InstructorIndexData.cs* i Zastąp istniejący kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="612d9-185">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="612d9-186">Tworzenie widoków i kontrolerów przez instruktorów</span><span class="sxs-lookup"><span data-stu-id="612d9-186">Create the Instructor controller and views</span></span>

<span data-ttu-id="612d9-187">Utworzyć kontroler Instruktorzy z akcjami odczytu/zapisu EF, jak pokazano na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="612d9-187">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![Dodawanie kontrolera instruktorów](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="612d9-189">Otwórz *InstructorsController.cs* i Dodaj nazwę za pomocą instrukcji dla przestrzeni nazw modele widoków:</span><span class="sxs-lookup"><span data-stu-id="612d9-189">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="612d9-190">Zastąp metodę indeksu następujący kod, aby nie wczesne ładowanie powiązanych danych i umieść ją w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="612d9-190">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="612d9-191">Metoda akceptuje dane opcjonalne trasy (`id`) i parametr ciągu zapytania (`courseID`), podaj wartości identyfikator wybranego przez instruktorów i wybranych kursów.</span><span class="sxs-lookup"><span data-stu-id="612d9-191">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="612d9-192">Parametry są dostarczane przez **wybierz** hiperlinków na stronie.</span><span class="sxs-lookup"><span data-stu-id="612d9-192">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="612d9-193">Kod rozpoczyna się od tworzenia wystąpienia obiektu modelu widoku i umieszczenie w nim listy instruktorów.</span><span class="sxs-lookup"><span data-stu-id="612d9-193">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="612d9-194">Kod ten określa wczesne ładowanie dla `Instructor.OfficeAssignment` i `Instructor.CourseAssignments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="612d9-194">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="612d9-195">W ramach `CourseAssignments` właściwości `Course` właściwość jest załadowany oraz w ramach, `Enrollments` i `Department` właściwości są załadowane, a w ramach każdej `Enrollment` jednostki `Student` właściwość jest ładowany.</span><span class="sxs-lookup"><span data-stu-id="612d9-195">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="612d9-196">Ponieważ widoku zawsze wymaga jednostki OfficeAssignment, jest bardziej wydajne, można pobrać, w tym samym zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="612d9-196">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="612d9-197">Kurs jednostki są wymagane, gdy pod kierunkiem instruktora jest zaznaczona na stronie sieci web, więc pojedynczego zapytania jest lepsze niż wielu zapytań, tylko wtedy, gdy ta strona jest wyświetlana częściej kurs wybrana niż bez.</span><span class="sxs-lookup"><span data-stu-id="612d9-197">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="612d9-198">Kod jest powtarzany `CourseAssignments` i `Course` ponieważ potrzebne dwie właściwości z `Course`.</span><span class="sxs-lookup"><span data-stu-id="612d9-198">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="612d9-199">Pierwszy ciąg `ThenInclude` wywołuje pobiera `CourseAssignment.Course`, `Course.Enrollments`, i `Enrollment.Student`.</span><span class="sxs-lookup"><span data-stu-id="612d9-199">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="612d9-200">W tym momencie w kodzie inny `ThenInclude` byłoby dla właściwości nawigacji `Student`, który nie jest potrzebny.</span><span class="sxs-lookup"><span data-stu-id="612d9-200">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="612d9-201">Ale wywołanie `Include` zaczyna się od ciągu `Instructor` właściwości, dzięki czemu będą musiały przejść przez łańcuch ponownie, określając ten czas `Course.Department` zamiast `Course.Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="612d9-201">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="612d9-202">Poniższy kod wykonuje, gdy wybrano pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="612d9-202">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="612d9-203">Wybrane przez instruktorów są pobierane z listy w modelu widoku instruktorów.</span><span class="sxs-lookup"><span data-stu-id="612d9-203">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="612d9-204">Model widoku `Courses` właściwość jest następnie ładowany z jednostkami kurs z tym instruktora `CourseAssignments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="612d9-204">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="612d9-205">`Where` Metoda zwraca kolekcję, ale w takim przypadku kryteria przekazany do tej metody powodują tylko do jednej przez instruktorów jednostki zwracanego.</span><span class="sxs-lookup"><span data-stu-id="612d9-205">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="612d9-206">`Single` Metoda konwertuje kolekcję na pojedynczej jednostki przez instruktorów, która zapewnia dostęp do tej jednostki `CourseAssignments` właściwości.</span><span class="sxs-lookup"><span data-stu-id="612d9-206">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="612d9-207">`CourseAssignments` Właściwość zawiera `CourseAssignment` jednostek, z których mają tylko powiązane `Course` jednostek.</span><span class="sxs-lookup"><span data-stu-id="612d9-207">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="612d9-208">Możesz użyć `Single` metody w kolekcji, gdy wiadomo, Kolekcja będzie mieć tylko jeden element.</span><span class="sxs-lookup"><span data-stu-id="612d9-208">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="612d9-209">Pojedyncza metoda zgłasza wyjątek, jeśli kolekcja wymagał jest pusta lub ma więcej niż jeden element.</span><span class="sxs-lookup"><span data-stu-id="612d9-209">The Single method throws an exception if the collection passed to it's empty or if there's more than one item.</span></span> <span data-ttu-id="612d9-210">Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (wartość null, w tym przypadku) Jeśli kolekcja jest pusta.</span><span class="sxs-lookup"><span data-stu-id="612d9-210">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="612d9-211">Jednak w takim przypadku, będzie nadal prowadzić do wyjątku (z próby znalezienia `Courses` właściwość odwołanie o wartości null), a komunikat o wyjątku mniej wyraźnie wskazuje przyczynę problemu.</span><span class="sxs-lookup"><span data-stu-id="612d9-211">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="612d9-212">Podczas wywoływania `Single` metody, można również przekazać kiedy warunek, zamiast wywoływać metodę `Where` metoda oddzielnie:</span><span class="sxs-lookup"><span data-stu-id="612d9-212">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="612d9-213">Zamiast:</span><span class="sxs-lookup"><span data-stu-id="612d9-213">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="612d9-214">Następnie jeśli kurs został wybrany, wybranych kursów jest pobierana z Lista kursów model widoku.</span><span class="sxs-lookup"><span data-stu-id="612d9-214">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="612d9-215">Następnie Wyświetl modelu `Enrollments` właściwość jest ładowany z jednostkami rejestracji z danego kursu `Enrollments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="612d9-215">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="612d9-216">Zmodyfikuj widok indeksu przez instruktorów</span><span class="sxs-lookup"><span data-stu-id="612d9-216">Modify the Instructor Index view</span></span>

<span data-ttu-id="612d9-217">W *Views/Instructors/Index.cshtml*, Zastąp kod szablonu poniższym kodem.</span><span class="sxs-lookup"><span data-stu-id="612d9-217">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="612d9-218">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="612d9-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="612d9-219">Następujące zmiany wprowadzone do istniejącego kodu:</span><span class="sxs-lookup"><span data-stu-id="612d9-219">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="612d9-220">Klasa modelu, aby zmienić `InstructorIndexData`.</span><span class="sxs-lookup"><span data-stu-id="612d9-220">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="612d9-221">Zmienić tytuł strony z **indeksu** do **Instruktorzy**.</span><span class="sxs-lookup"><span data-stu-id="612d9-221">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="612d9-222">Dodano **Office** kolumnę wyświetlającą `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie ma wartości null.</span><span class="sxs-lookup"><span data-stu-id="612d9-222">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="612d9-223">(Ponieważ jest to relacja jeden do zero lub jeden, nie może być powiązana jednostka OfficeAssignment.)</span><span class="sxs-lookup"><span data-stu-id="612d9-223">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="612d9-224">Dodano **kursów** kolumnę wyświetlającą kursy prowadzone przez instruktora każdego.</span><span class="sxs-lookup"><span data-stu-id="612d9-224">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="612d9-225">Zobacz [jawne przejście wiersza z `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) więcej informacji na temat tej składni razor.</span><span class="sxs-lookup"><span data-stu-id="612d9-225">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="612d9-226">Dodano kod, w której są dodawane dynamicznie `class="success"` do `tr` elementu wybranego przez instruktorów.</span><span class="sxs-lookup"><span data-stu-id="612d9-226">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="612d9-227">To ustawienie jako kolor tła wybranego wiersza przy użyciu ładowania klasy.</span><span class="sxs-lookup"><span data-stu-id="612d9-227">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="612d9-228">Dodano nowe hiperłącze etykietą **wybierz** bezpośrednio przed innych linków w każdym wierszu, co powoduje, że identyfikator wybranego przez instruktorów do wysłania do `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="612d9-228">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="612d9-229">Uruchom aplikację i wybierz **Instruktorzy** kartę. Strony wyświetla Właściwość Location elementu komórki pustej tabeli i powiązanymi jednostkami OfficeAssignment po nie powiązanej jednostki OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="612d9-229">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Strona indeksu instruktorów, którą nic nie zaznaczone](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="612d9-231">W *Views/Instructors/Index.cshtml* pliku po zamykającym table — element (na końcu pliku), Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="612d9-231">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="612d9-232">Ten kod wyświetla listę kursów związane z kierunkiem instruktora, po wybraniu pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="612d9-232">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="612d9-233">Ten kod odczytuje `Courses` właściwości modelu widoku, aby wyświetlić listę kursów.</span><span class="sxs-lookup"><span data-stu-id="612d9-233">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="612d9-234">Zapewnia także **wybierz** hiperłącze, które wysyła identyfikator wybranego kurs, aby `Index` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="612d9-234">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="612d9-235">Odśwież stronę, a następnie wybierz pozycję pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="612d9-235">Refresh the page and select an instructor.</span></span> <span data-ttu-id="612d9-236">Spowoduje to wyświetlenie siatce, która wyświetla kursy przypisane do wybranego przez instruktorów i każdego kursu możesz zobaczyć nazwę tego działu przypisane.</span><span class="sxs-lookup"><span data-stu-id="612d9-236">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Wybrane przez instruktorów indeks strony instruktorów](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="612d9-238">Po bloku kodu, który właśnie został dodany Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="612d9-238">After the code block you just added, add the following code.</span></span> <span data-ttu-id="612d9-239">Spowoduje to wyświetlenie listy uczniów, którzy są rejestrowane kursu, po wybraniu danego kursu.</span><span class="sxs-lookup"><span data-stu-id="612d9-239">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="612d9-240">Ten kod odczytuje właściwość rejestracji modelu widoku, aby wyświetlić listę uczniów zarejestrowane w ramach tego kursu.</span><span class="sxs-lookup"><span data-stu-id="612d9-240">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="612d9-241">Ponownie Odśwież stronę, a następnie wybierz pozycję pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="612d9-241">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="612d9-242">Następnie wybierz kurs, aby wyświetlić listę zarejestrowanych studentów i ich klas.</span><span class="sxs-lookup"><span data-stu-id="612d9-242">Then select a course to see the list of enrolled students and their grades.</span></span>

![Instruktorzy indeks strony instruktora oraz przypisane kursów wybranych](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="612d9-244">Jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="612d9-244">Explicit loading</span></span>

<span data-ttu-id="612d9-245">Po pobraniu listy Instruktorzy w *InstructorsController.cs*, określony wczesne ładowanie dla `CourseAssignments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="612d9-245">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="612d9-246">Załóżmy, że oczekiwany użytkownikom tylko rzadko mają być wyświetlane rejestracje w wybrany przez instruktorów i kursów.</span><span class="sxs-lookup"><span data-stu-id="612d9-246">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="612d9-247">W takim przypadku można załadować danych rejestracji, tylko wtedy, gdy jest żądany.</span><span class="sxs-lookup"><span data-stu-id="612d9-247">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="612d9-248">Aby zobaczyć przykład sposobu wykonania jawne ładowanie, Zastąp `Index` metoda poniższym kodem, co spowoduje usunięcie wczesne ładowanie na potrzeby rejestracji i ładuje jawnie tej właściwości.</span><span class="sxs-lookup"><span data-stu-id="612d9-248">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="612d9-249">Zmiany kodu są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="612d9-249">The code changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="612d9-250">Nowy kod spada *ThenInclude* metoda wywołuje danych rejestracji z kodu, która pobiera jednostki instruktora.</span><span class="sxs-lookup"><span data-stu-id="612d9-250">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="612d9-251">Jeśli wybrano przez instruktorów i kursów, wyróżniony kod pobiera jednostki rejestracji dla wybranych kursów i uczniów jednostki dla każdej rejestracji.</span><span class="sxs-lookup"><span data-stu-id="612d9-251">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="612d9-252">Uruchom aplikację, przejdź do strony indeksu Instruktorzy teraz i będzie żadnej widocznej różnicy w wyświetlanych na stronie, mimo że zostało zmienione, jak dane są pobierane.</span><span class="sxs-lookup"><span data-stu-id="612d9-252">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="612d9-253">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="612d9-253">Summary</span></span>

<span data-ttu-id="612d9-254">Wczesne ładowanie została obecnie używany za pomocą jednego zapytania i z wieloma zapytaniami, aby odczytać powiązanych danych właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="612d9-254">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="612d9-255">W następnym samouczku dowiesz się, jak zaktualizować powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="612d9-255">In the next tutorial you'll learn how to update related data.</span></span>

::: moniker-end

>[!div class="step-by-step"]
><span data-ttu-id="612d9-256">[Poprzednie](complex-data-model.md)
>[dalej](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="612d9-256">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>
