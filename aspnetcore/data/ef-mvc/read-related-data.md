---
title: Platformy ASP.NET Core MVC podstawowych EF - odczytanie danych powiązanych — 6 10
author: rick-anderson
description: W tym samouczku będziesz odczytu i wyświetlanie powiązanych danych — to znaczy dane programu Entity Framework wczytywane właściwości nawigacji.
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: db47340aa3dbca486cc30667baf03e49491f1d1a
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-mvc-with-ef-core---read-related-data---6-of-10"></a><span data-ttu-id="06218-103">Platformy ASP.NET Core MVC podstawowych EF - odczytanie danych powiązanych — 6 10</span><span class="sxs-lookup"><span data-stu-id="06218-103">ASP.NET Core MVC with EF Core - Read Related Data - 6 of 10</span></span>

<span data-ttu-id="06218-104">Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="06218-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="06218-105">Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core MVC przy użyciu programu Entity Framework Core i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="06218-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="06218-106">Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](intro.md).</span><span class="sxs-lookup"><span data-stu-id="06218-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="06218-107">W samouczku poprzedniej została ukończona modelu danych służbowych.</span><span class="sxs-lookup"><span data-stu-id="06218-107">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="06218-108">W tym samouczku możesz przeczytać i wyświetlanie powiązanych danych — to znaczy dane programu Entity Framework wczytywane właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="06218-108">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="06218-109">Na poniższych ilustracjach przedstawiono stron, że będzie działać z.</span><span class="sxs-lookup"><span data-stu-id="06218-109">The following illustrations show the pages that you'll work with.</span></span>

![Strona indeksu kursy](read-related-data/_static/courses-index.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="06218-112">Wczesny jawne i opóźnionego ładowania powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="06218-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="06218-113">Istnieje kilka sposobów oprogramowania obiektów relacyjnych mapowania (ORM), takie jak Entity Framework można załadować powiązanych danych do właściwości nawigacji jednostki:</span><span class="sxs-lookup"><span data-stu-id="06218-113">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="06218-114">Ładowanie wczesny.</span><span class="sxs-lookup"><span data-stu-id="06218-114">Eager loading.</span></span> <span data-ttu-id="06218-115">Podczas odczytywania jednostki powiązane dane są pobierane wraz z jej.</span><span class="sxs-lookup"><span data-stu-id="06218-115">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="06218-116">Powoduje to zwykle w zapytaniu sprzężenia jednej, która pobiera wszystkie dane potrzebne.</span><span class="sxs-lookup"><span data-stu-id="06218-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="06218-117">Określ wczesny ładowania w Entity Framework Core za pomocą `Include` i `ThenInclude` metody.</span><span class="sxs-lookup"><span data-stu-id="06218-117">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![Przykład wczesny ładowania](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="06218-119">Możesz pobrać niektóre dane w oddzielne zapytania, a EF "rozwiązuje" właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="06218-119">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="06218-120">Oznacza to EF automatycznie doda oddzielnie pobrane jednostek, w których one należą do właściwości nawigacji jednostek wcześniej pobrane.</span><span class="sxs-lookup"><span data-stu-id="06218-120">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="06218-121">Dla zapytania, które pobiera dane powiązane, można użyć `Load` metody zamiast metody, która zwraca listy lub obiektu, takie jak `ToList` lub `Single`.</span><span class="sxs-lookup"><span data-stu-id="06218-121">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![Przykład oddzielne zapytania](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="06218-123">Jawne ładowania.</span><span class="sxs-lookup"><span data-stu-id="06218-123">Explicit loading.</span></span> <span data-ttu-id="06218-124">Gdy obiekt jest najpierw przeczytać artykuł, pobrać nie jest powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="06218-124">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="06218-125">Możesz pisania kodu, który pobiera dane powiązane, jeśli jest to potrzebne.</span><span class="sxs-lookup"><span data-stu-id="06218-125">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="06218-126">Tak jak w przypadku eager ładowania z oddzielne zapytania jawne, ładowanie wyników w kwerendach wielu wysyłane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="06218-126">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="06218-127">Różnica polega na z jawnego ładowania kodu określa właściwości nawigacji do załadowania.</span><span class="sxs-lookup"><span data-stu-id="06218-127">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="06218-128">W Entity Framework Core 1.1 można użyć `Load` metody w celu jawnego ładowania.</span><span class="sxs-lookup"><span data-stu-id="06218-128">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="06218-129">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="06218-129">For example:</span></span>

  ![Przykład jawnego ładowania](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="06218-131">Powolne ładowanie.</span><span class="sxs-lookup"><span data-stu-id="06218-131">Lazy loading.</span></span> <span data-ttu-id="06218-132">Gdy obiekt jest najpierw przeczytać artykuł, pobrać nie jest powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="06218-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="06218-133">Jednak podczas pierwszej próby dostępu do właściwości nawigacji wymagane dane dla tej właściwości nawigacji jest automatycznie pobierany.</span><span class="sxs-lookup"><span data-stu-id="06218-133">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="06218-134">Zapytanie jest wysyłane do bazy danych zawsze próbować pobierać dane z właściwości nawigacji, po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="06218-134">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="06218-135">Entity Framework Core 1.0 nie obsługuje ładowania opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="06218-135">Entity Framework Core 1.0 doesn't support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="06218-136">Zagadnienia dotyczące wydajności</span><span class="sxs-lookup"><span data-stu-id="06218-136">Performance considerations</span></span>

<span data-ttu-id="06218-137">Jeśli znasz potrzebne dane dotyczące dla każdej jednostki pobrać wczesny ładowania często oferuje najlepszą wydajność, ponieważ pojedynczego zapytania wysyłane do bazy danych jest zazwyczaj bardziej efektywne niż oddzielne zapytania dla każdej jednostki pobrać.</span><span class="sxs-lookup"><span data-stu-id="06218-137">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="06218-138">Na przykład załóżmy, że każdy dział ma dziesięć Kursy pokrewne.</span><span class="sxs-lookup"><span data-stu-id="06218-138">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="06218-139">Ładowanie wczesny wszystkich powiązanych danych spowoduje tylko zapytania jednym (połączenie) i jeden obiegu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="06218-139">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="06218-140">Oddzielne zapytania kursów dla każdego działu spowoduje 11 rund do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="06218-140">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="06218-141">Dodatkowe rund do bazy danych są szczególnie szkodliwe dla wydajności, gdy dużych opóźnieniach.</span><span class="sxs-lookup"><span data-stu-id="06218-141">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="06218-142">Z drugiej strony w niektórych scenariuszach oddzielne zapytania jest bardziej wydajny.</span><span class="sxs-lookup"><span data-stu-id="06218-142">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="06218-143">Ładowanie wczesny wszystkich powiązanych danych w jednym zapytaniu może spowodować sprzężenia bardzo skomplikowane, zostanie wygenerowany, której program SQL Server nie może przetworzyć wydajnie.</span><span class="sxs-lookup"><span data-stu-id="06218-143">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="06218-144">Lub jeśli potrzebujesz dostępu do właściwości nawigacji jednostki tylko przez podzbiór zestawu jednostek, który jest przetwarzanie, oddzielne zapytania mogą działać lepiej ponieważ wczesny ładowania wszystkich elementów na początku czy pobrać więcej danych niż trzeba.</span><span class="sxs-lookup"><span data-stu-id="06218-144">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="06218-145">Jeśli wydajność jest szczególnie ważne, najlepiej testowanie wydajności w obu kierunkach, aby ustawić najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="06218-145">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="06218-146">Utwórz stronę kursy Wyświetla nazwę działu</span><span class="sxs-lookup"><span data-stu-id="06218-146">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="06218-147">Jednostka kursu obejmuje zawierającym jednostkę działu działu kursu przypisane do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="06218-147">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="06218-148">Aby wyświetlić nazwę działu przypisane na liście kursów, należy uzyskać właściwość Name z jednostki działu, który znajduje się w `Course.Department` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="06218-148">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that's in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="06218-149">Tworzenie kontrolera o nazwie CoursesController dla porach typu jednostki, przy użyciu tych samych opcji dla **kontroler MVC z widokami używający narzędzia Entity Framework** tworzenia szkieletu, która została wcześniej kontrolera studentów, jak pokazano w poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="06218-149">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![Dodawanie kontrolera kursy](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="06218-151">Otwórz *CoursesController.cs* i sprawdź, czy `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="06218-151">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="06218-152">Automatyczne szkieletów określono wczesny ładowania dla `Department` właściwość nawigacji za pomocą `Include` metody.</span><span class="sxs-lookup"><span data-stu-id="06218-152">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="06218-153">Zastąp `Index` metodę z następującym kodem, który używa bardziej odpowiednie nazwy dla `IQueryable` zwracającą kursu jednostek (`courses` zamiast `schoolContext`):</span><span class="sxs-lookup"><span data-stu-id="06218-153">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="06218-154">Otwórz *Views/Courses/Index.cshtml* i Zastąp kod szablonu z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="06218-154">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="06218-155">Zmiany są wyróżnione:</span><span class="sxs-lookup"><span data-stu-id="06218-155">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="06218-156">Wprowadzono następujące zmiany do szkieletu kodu:</span><span class="sxs-lookup"><span data-stu-id="06218-156">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="06218-157">Zmieniony nagłówek od indeksu do kursów.</span><span class="sxs-lookup"><span data-stu-id="06218-157">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="06218-158">Dodaje **numer** kolumny, która zawiera `CourseID` wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="06218-158">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="06218-159">Domyślnie nie są szkieletu kluczy podstawowych, ponieważ zwykle są bezużyteczne dla użytkowników końcowych.</span><span class="sxs-lookup"><span data-stu-id="06218-159">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="06218-160">Jednak w takim przypadku klucza podstawowego ma znaczenie i chcesz je wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="06218-160">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="06218-161">Zmienione **działu** kolumny, aby wyświetlić nazwę działu.</span><span class="sxs-lookup"><span data-stu-id="06218-161">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="06218-162">Wyświetla kod `Name` właściwości jednostki działu, który jest ładowany do `Department` właściwość nawigacji:</span><span class="sxs-lookup"><span data-stu-id="06218-162">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="06218-163">Uruchom aplikację i wybierz **kursów** kartę, aby wyświetlić listę z nazwami działu.</span><span class="sxs-lookup"><span data-stu-id="06218-163">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Strona indeksu kursy](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="06218-165">Utwórz stronę instruktorów kursów i rejestracji</span><span class="sxs-lookup"><span data-stu-id="06218-165">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="06218-166">W tej sekcji utworzysz kontrolera i widoku dla obiekt instruktora w celu wyświetlenia strony instruktorów:</span><span class="sxs-lookup"><span data-stu-id="06218-166">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

<span data-ttu-id="06218-168">Ta strona odczytuje i wyświetla powiązanych danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="06218-168">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="06218-169">Na liście instruktorów zostaną wyświetlone dane powiązane z OfficeAssignment jednostki.</span><span class="sxs-lookup"><span data-stu-id="06218-169">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="06218-170">Jednostki instruktora i OfficeAssignment znajdują się w relacji jeden do zero lub jeden.</span><span class="sxs-lookup"><span data-stu-id="06218-170">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="06218-171">Ładowanie wczesny będzie używany dla jednostek OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="06218-171">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="06218-172">Zgodnie z opisem, wczesny ładowania jest zazwyczaj bardziej wydajne, gdy potrzebne są powiązane dane dla wszystkich pobranych wierszy w tabeli podstawowej.</span><span class="sxs-lookup"><span data-stu-id="06218-172">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="06218-173">W takim przypadku mają być wyświetlane przypisań pakietu office dla wszystkich wyświetlanych instruktorów.</span><span class="sxs-lookup"><span data-stu-id="06218-173">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="06218-174">Po wybraniu instruktora powiązanych jednostek kursu są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="06218-174">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="06218-175">Jednostki instruktora i kursu znajdują się w relacji wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="06218-175">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="06218-176">Użyjesz wczesny ładowanie obiektów kursu i ich powiązanych jednostek działu.</span><span class="sxs-lookup"><span data-stu-id="06218-176">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="06218-177">W takim przypadku oddzielne zapytania może być bardziej wydajne, ponieważ należy kursy tylko dla wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="06218-177">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="06218-178">Jednak ten przykład przedstawia sposób użycia wczesny ładowania dla właściwości nawigacji w ramach jednostkami, którymi na właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="06218-178">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="06218-179">Gdy użytkownik wybierze kursu, są wyświetlane powiązane dane z zestawu jednostek rejestracji.</span><span class="sxs-lookup"><span data-stu-id="06218-179">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="06218-180">Jednostki przebiegu i rejestracji są w relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="06218-180">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="06218-181">Użyjesz oddzielne zapytania rejestracji obiektów i ich powiązanych jednostek studenta.</span><span class="sxs-lookup"><span data-stu-id="06218-181">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="06218-182">Utwórz model widoku dla widoku indeksu instruktora</span><span class="sxs-lookup"><span data-stu-id="06218-182">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="06218-183">Na stronie instruktorów znajdują się dane z trzech różnych tabel.</span><span class="sxs-lookup"><span data-stu-id="06218-183">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="06218-184">W związku z tym utworzysz modelu widoku, który zawiera trzy właściwości, zawierający dane z tabel.</span><span class="sxs-lookup"><span data-stu-id="06218-184">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="06218-185">W *SchoolViewModels* folderu, Utwórz *InstructorIndexData.cs* i Zastąp istniejący kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="06218-185">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="06218-186">Tworzenie kontrolera instruktora i widoków</span><span class="sxs-lookup"><span data-stu-id="06218-186">Create the Instructor controller and views</span></span>

<span data-ttu-id="06218-187">Tworzenie kontrolera instruktorów z akcjami odczytu/zapisu EF, jak pokazano na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="06218-187">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![Dodawanie kontrolera instruktorów](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="06218-189">Otwórz *InstructorsController.cs* i dodać za pomocą instrukcji dla przestrzeni nazw ViewModels:</span><span class="sxs-lookup"><span data-stu-id="06218-189">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="06218-190">Zastąp następujący kod do czy ładowanie wczesny powiązanych danych i umieść go w modelu widoku metody indeksu.</span><span class="sxs-lookup"><span data-stu-id="06218-190">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="06218-191">Metoda akceptuje dane trasy opcjonalne (`id`) i parametr ciągu zapytania (`courseID`) umożliwiające wartości Identyfikatora wybranym instruktorze i wybranego kursu.</span><span class="sxs-lookup"><span data-stu-id="06218-191">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="06218-192">Parametry są dostarczane przez **wybierz** hiperłącza, na stronie.</span><span class="sxs-lookup"><span data-stu-id="06218-192">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="06218-193">Kod rozpoczyna się przez utworzenie wystąpienia modelu widoku i umieszczenie w nim listy instruktorów.</span><span class="sxs-lookup"><span data-stu-id="06218-193">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="06218-194">Kod określa wczesny ładowania dla `Instructor.OfficeAssignment` i `Instructor.CourseAssignments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="06218-194">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="06218-195">W ramach `CourseAssignments` właściwość `Course` właściwość jest załadowany oraz w ramach, `Enrollments` i `Department` właściwości są załadowane, a w ramach każdej `Enrollment` jednostki `Student` właściwości jest załadowany.</span><span class="sxs-lookup"><span data-stu-id="06218-195">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="06218-196">Ponieważ widok wymaga zawsze OfficeAssignment jednostki, jest bardziej wydajne, można pobrać który w jednym zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="06218-196">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="06218-197">Jednostek kursu są wymagane w przypadku instruktora jest zaznaczona na stronie sieci web, więc pojedynczego zapytania jest lepszym rozwiązaniem niż wielu zapytań tylko wtedy, gdy strona jest wyświetlana częściej z kursu wybrane niż bez.</span><span class="sxs-lookup"><span data-stu-id="06218-197">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="06218-198">Kod jest powtarzany `CourseAssignments` i `Course` ponieważ potrzebne dwie właściwości z `Course`.</span><span class="sxs-lookup"><span data-stu-id="06218-198">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="06218-199">Pierwszy ciąg `ThenInclude` wywołuje pobiera `CourseAssignment.Course`, `Course.Enrollments`, i `Enrollment.Student`.</span><span class="sxs-lookup"><span data-stu-id="06218-199">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="06218-200">W tym momencie w kodzie innego `ThenInclude` do właściwości nawigacji `Student`, który nie jest potrzebny.</span><span class="sxs-lookup"><span data-stu-id="06218-200">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="06218-201">Ale wywoływania `Include` rozpoczyna się od za pośrednictwem `Instructor` właściwości, więc musisz przejść przez łańcuch ponownie, ten czas, określając `Course.Department` zamiast `Course.Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="06218-201">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="06218-202">Poniższy kod wykonuje się, gdy wybrano instruktora.</span><span class="sxs-lookup"><span data-stu-id="06218-202">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="06218-203">Wybranym instruktorze są pobierane z listy instruktorów w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="06218-203">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="06218-204">Model widoku `Courses` właściwości jest następnie ładowany z kursu jednostek z tego instruktora `CourseAssignments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="06218-204">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="06218-205">`Where` Metoda zwraca kolekcję, ale w takim przypadku kryteria przekazany do tej metody powodują tylko do jednej instruktora jednostki zostały zwrócone.</span><span class="sxs-lookup"><span data-stu-id="06218-205">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="06218-206">`Single` Metoda konwertuje kolekcję do pojedynczej jednostki instruktora, która umożliwia dostęp do tej jednostki `CourseAssignments` właściwości.</span><span class="sxs-lookup"><span data-stu-id="06218-206">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="06218-207">`CourseAssignments` Właściwość zawiera `CourseAssignment` jednostek, z których chcesz tylko powiązane `Course` jednostek.</span><span class="sxs-lookup"><span data-stu-id="06218-207">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="06218-208">Możesz użyć `Single` metoda w kolekcji, gdy wiesz, Kolekcja będzie mieć tylko jeden element.</span><span class="sxs-lookup"><span data-stu-id="06218-208">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="06218-209">Pojedyncza metoda zgłasza wyjątek, jeśli kolekcja przekazywania jest pusta lub jeśli istnieje więcej niż jeden element.</span><span class="sxs-lookup"><span data-stu-id="06218-209">The Single method throws an exception if the collection passed to it's empty or if there's more than one item.</span></span> <span data-ttu-id="06218-210">Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (wartość null, w tym przypadku), jeśli kolekcja jest pusta.</span><span class="sxs-lookup"><span data-stu-id="06218-210">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="06218-211">Jednak w takim przypadku który nadal spowoduje powstanie wyjątku (z próby znalezienia `Courses` właściwości na odwołanie o wartości null), a komunikat o wyjątku mniej wyraźnie wskazuje przyczynę problemu.</span><span class="sxs-lookup"><span data-stu-id="06218-211">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="06218-212">Podczas wywoływania `Single` metody, można również przekazać Where warunku zamiast wywoływać metodę `Where` metody oddzielnie:</span><span class="sxs-lookup"><span data-stu-id="06218-212">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="06218-213">Zamiast:</span><span class="sxs-lookup"><span data-stu-id="06218-213">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="06218-214">Obok Jeśli wybrano kursu, wybranego kursu są pobierane z listy kursy w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="06218-214">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="06218-215">Następnie widok modelu `Enrollments` właściwości jest załadowana rejestracji jednostek z tego kursu `Enrollments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="06218-215">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="06218-216">Zmodyfikuj widok indeksu instruktora</span><span class="sxs-lookup"><span data-stu-id="06218-216">Modify the Instructor Index view</span></span>

<span data-ttu-id="06218-217">W *Views/Instructors/Index.cshtml*, Zastąp kod szablonu z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="06218-217">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="06218-218">Zmiany zostały wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="06218-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="06218-219">Następujące zmiany wprowadzone do istniejącego kodu:</span><span class="sxs-lookup"><span data-stu-id="06218-219">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="06218-220">Klasa modelu, aby zmienić `InstructorIndexData`.</span><span class="sxs-lookup"><span data-stu-id="06218-220">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="06218-221">Zmienić tytuł strony z **indeksu** do **instruktorów**.</span><span class="sxs-lookup"><span data-stu-id="06218-221">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="06218-222">Dodaje **Office** kolumnę wyświetlającą `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie ma wartości null.</span><span class="sxs-lookup"><span data-stu-id="06218-222">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="06218-223">(Ponieważ jest to relacji jeden do zero lub jeden, być może obiekt pokrewny OfficeAssignment.)</span><span class="sxs-lookup"><span data-stu-id="06218-223">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="06218-224">Dodaje **kursów** kolumnę wyświetlającą kursy nauczanych przy każdym instruktora.</span><span class="sxs-lookup"><span data-stu-id="06218-224">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="06218-225">Zobacz [jawne przejścia wiersza z `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) więcej informacji o tej składni razor.</span><span class="sxs-lookup"><span data-stu-id="06218-225">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="06218-226">Dodano kod, który dynamicznie dodaje `class="success"` do `tr` elementu wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="06218-226">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="06218-227">To ustawia kolor tła dla wybranego wiersza za pomocą klasy ładowania początkowego.</span><span class="sxs-lookup"><span data-stu-id="06218-227">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="06218-228">Dodano nowe hiperłącze etykietą **wybierz** bezpośrednio przed innymi łączy w każdym wierszu, co powoduje, że identyfikator wybranego instruktora do wysłania do `Index` metody.</span><span class="sxs-lookup"><span data-stu-id="06218-228">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="06218-229">Uruchom aplikację i wybierz **instruktorów** kartę. Zostaje wyświetlona strona właściwości Location powiązanych jednostek OfficeAssignment i puste komórki po nie powiązanych jednostek OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="06218-229">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Strona indeksu instruktorów nic nie wybrane](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="06218-231">W *Views/Instructors/Index.cshtml* plik, po zamknięcia tabeli elementu (na końcu pliku), Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="06218-231">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="06218-232">Ten kod wyświetla listę kursów związane z instruktora po wybraniu instruktora.</span><span class="sxs-lookup"><span data-stu-id="06218-232">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="06218-233">Ten kod odczytuje `Courses` właściwości modelu widoku, aby wyświetlić listę kursów.</span><span class="sxs-lookup"><span data-stu-id="06218-233">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="06218-234">Zapewnia także **wybierz** hiperłącze, które wysyła identyfikator wybranego kursu do `Index` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="06218-234">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="06218-235">Odśwież stronę i wybierz instruktora.</span><span class="sxs-lookup"><span data-stu-id="06218-235">Refresh the page and select an instructor.</span></span> <span data-ttu-id="06218-236">Spowoduje to wyświetlenie Siatka wyświetla kursy przypisane do wybranych instruktora i dla każdego kursu wyświetlona nazwa przypisanej działu.</span><span class="sxs-lookup"><span data-stu-id="06218-236">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Instruktora strony indeksu instruktorów wybrane](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="06218-238">Po bloku kodu, który właśnie został dodany Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="06218-238">After the code block you just added, add the following code.</span></span> <span data-ttu-id="06218-239">Lista zawiera studentów, którzy są zarejestrowane w toku w przypadku wybrania tego kursu.</span><span class="sxs-lookup"><span data-stu-id="06218-239">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="06218-240">Ten kod odczytuje właściwości rejestracji modelu widoku, aby wyświetlić listę studentów zarejestrowane w trakcie.</span><span class="sxs-lookup"><span data-stu-id="06218-240">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="06218-241">Ponownie Odśwież stronę i wybierz instruktora.</span><span class="sxs-lookup"><span data-stu-id="06218-241">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="06218-242">Następnie wybierz plan, aby wyświetlić listę zarejestrowanych studentów i ich klas.</span><span class="sxs-lookup"><span data-stu-id="06218-242">Then select a course to see the list of enrolled students and their grades.</span></span>

![Instruktora strony indeksu instruktorów i kursów wybranych](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="06218-244">Ładowanie jawne</span><span class="sxs-lookup"><span data-stu-id="06218-244">Explicit loading</span></span>

<span data-ttu-id="06218-245">Po pobraniu listy w instruktorów *InstructorsController.cs*, określony wczesny ładowania dla `CourseAssignments` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="06218-245">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="06218-246">Załóżmy, że oczekiwany użytkownikom rzadko mają być wyświetlane rejestracji w wybranym instruktorze oraz przebiegu.</span><span class="sxs-lookup"><span data-stu-id="06218-246">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="06218-247">W takim przypadku można załadować danych rejestrowania, tylko wtedy, gdy żądanie.</span><span class="sxs-lookup"><span data-stu-id="06218-247">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="06218-248">Aby wyświetlić przykład sposobu wykonania jawnego ładowania, Zastąp `Index` metodę z następującym kodem, który usuwa wczesny ładowania dla rejestracji i ładuje jawnie tej właściwości.</span><span class="sxs-lookup"><span data-stu-id="06218-248">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="06218-249">Zmiany kodu zostały wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="06218-249">The code changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="06218-250">Nowy kod porzuca *ThenInclude* metoda wywołuje danych rejestracji z kod, który pobiera instruktora jednostki.</span><span class="sxs-lookup"><span data-stu-id="06218-250">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="06218-251">Jeśli nie wybrano instruktora oraz przebiegu, wyróżniony kod pobiera jednostek rejestracji dla porach wybranych i uczniów jednostki dla każdej rejestracji.</span><span class="sxs-lookup"><span data-stu-id="06218-251">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="06218-252">Uruchom aplikację, przejdź do strony indeksu instruktorów teraz i będzie żadnej widocznej różnicy w wyświetlanych na stronie, mimo że zostały zmienione, jak dane są pobierane.</span><span class="sxs-lookup"><span data-stu-id="06218-252">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="06218-253">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="06218-253">Summary</span></span>

<span data-ttu-id="06218-254">Teraz wczesny ładowanie z jednego zapytania i używane z wieloma zapytaniami do wczytania powiązanych danych do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="06218-254">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="06218-255">W następnym samouczku nauczysz się, jak zaktualizować powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="06218-255">In the next tutorial you'll learn how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="06218-256">[Poprzednie](complex-data-model.md)
>[dalej](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="06218-256">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>  
