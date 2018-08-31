---
title: Strony razor za pomocą platformy Entity Framework Core w programie ASP.NET Core — samouczek 1 8
author: rick-anderson
description: Pokazuje, jak utworzyć aplikację strony Razor za pomocą platformy Entity Framework Core
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/intro
ms.openlocfilehash: d74d0909f03fe4d504f21c3880ec2447cadc8b13
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336097"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="332d9-103">Strony razor za pomocą platformy Entity Framework Core w programie ASP.NET Core — samouczek 1 8</span><span class="sxs-lookup"><span data-stu-id="332d9-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="332d9-104">Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="332d9-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="332d9-105">Przykładową aplikację sieci web firmy Contoso University pokazuje, jak utworzyć aplikację stron Razor programu ASP.NET Core przy użyciu Core Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="332d9-105">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="332d9-106">Przykładowa aplikacja jest witryną sieci web dla uniwersytetu fikcyjnej firmy Contoso.</span><span class="sxs-lookup"><span data-stu-id="332d9-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="332d9-107">Obejmuje funkcje, takie jak czasowej dla uczniów, tworzenia kurs i przypisania instruktora.</span><span class="sxs-lookup"><span data-stu-id="332d9-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="332d9-108">Ta strona jest to pierwszy z serii samouczków, które opisują sposób tworzenia przykładowej aplikacji Contoso University.</span><span class="sxs-lookup"><span data-stu-id="332d9-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="332d9-109">Pobieranie i wyświetlanie ukończonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="332d9-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="332d9-110">[Instrukcje pobierania](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="332d9-110">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="332d9-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="332d9-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="332d9-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="332d9-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="332d9-113">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="332d9-113">.NET Core CLI</span></span>](#tab/netcore-cli)

[!INCLUDE [](~/includes/2.1-SDK.md)]

------

<span data-ttu-id="332d9-114">Znajomość [stron Razor](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="332d9-114">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="332d9-115">Należy wykonać początkujących programistów [Rozpoczynanie pracy ze stronami Razor](xref:tutorials/razor-pages/razor-pages-start) przed rozpoczęciem tej serii.</span><span class="sxs-lookup"><span data-stu-id="332d9-115">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="332d9-116">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="332d9-116">Troubleshooting</span></span>

<span data-ttu-id="332d9-117">Jeśli napotkasz problem, nie można rozpoznać ogólnie można znaleźć rozwiązania, porównując swój kod, aby [projektu ukończona](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="332d9-117">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="332d9-118">Dobrym sposobem, aby uzyskać pomoc polega na publikowanie pytań do [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) dla [platformy ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [programu EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="332d9-118">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="332d9-119">Aplikacja sieci web firmy Contoso University</span><span class="sxs-lookup"><span data-stu-id="332d9-119">The Contoso University web app</span></span>

<span data-ttu-id="332d9-120">Aplikacja wbudowana w tych samouczkach to podstawowe university witryna sieci web.</span><span class="sxs-lookup"><span data-stu-id="332d9-120">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="332d9-121">Użytkownicy mogą przeglądać i aktualizacji dla uczniów, kursu i informacji przez instruktorów.</span><span class="sxs-lookup"><span data-stu-id="332d9-121">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="332d9-122">Oto kilka ekranów, utworzone w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="332d9-122">Here are a few of the screens created in the tutorial.</span></span>

![Strona indeksu uczniów](intro/_static/students-index.png)

![Strona edytowania uczniów](intro/_static/student-edit.png)

<span data-ttu-id="332d9-125">Styl interfejsu użytkownika w tej lokacji znajduje się w pobliżu co to jest generowany przez wbudowane szablony.</span><span class="sxs-lookup"><span data-stu-id="332d9-125">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="332d9-126">Samouczek koncentruje się na programu EF Core ze stronami Razor, nie interfejs użytkownika.</span><span class="sxs-lookup"><span data-stu-id="332d9-126">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="332d9-127">Tworzenie aplikacji sieci web ContosoUniversity stron Razor</span><span class="sxs-lookup"><span data-stu-id="332d9-127">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="332d9-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="332d9-128">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="332d9-129">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="332d9-129">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="332d9-130">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="332d9-130">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="332d9-131">Nadaj projektowi nazwę **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="332d9-131">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="332d9-132">Ważne jest, aby nadaj projektowi nazwę *ContosoUniversity* , przestrzenie nazw dopasować sytuacje, kiedy kod jest skopiowane i wklejone.</span><span class="sxs-lookup"><span data-stu-id="332d9-132">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="332d9-133">Wybierz **platformy ASP.NET Core 2.1** w listy rozwijanej, a następnie wybierz pozycję **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="332d9-133">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="332d9-134">Obrazy te czynności, zobacz [tworzenie aplikacji sieci web Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).</span><span class="sxs-lookup"><span data-stu-id="332d9-134">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).</span></span>
<span data-ttu-id="332d9-135">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="332d9-135">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="332d9-136">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="332d9-136">.NET Core CLI</span></span>](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a><span data-ttu-id="332d9-137">Ustawianie stylów lokacji</span><span class="sxs-lookup"><span data-stu-id="332d9-137">Set up the site style</span></span>

<span data-ttu-id="332d9-138">Kilka zmian, skonfiguruj w menu witryny, układu i strony głównej.</span><span class="sxs-lookup"><span data-stu-id="332d9-138">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="332d9-139">Aktualizacja *Pages/Shared/_Layout.cshtml* z następującymi zmianami:</span><span class="sxs-lookup"><span data-stu-id="332d9-139">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="332d9-140">Należy zmienić każde wystąpienie "ContosoUniversity" na "Uniwersytet firmy Contoso".</span><span class="sxs-lookup"><span data-stu-id="332d9-140">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="332d9-141">Istnieją trzy wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="332d9-141">There are three occurrences.</span></span>

* <span data-ttu-id="332d9-142">Dodaj elementy menu dla **studentów**, **kursów**, **Instruktorzy**, i **działów**i Usuń **skontaktuj się z pomocą** wpis menu.</span><span class="sxs-lookup"><span data-stu-id="332d9-142">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="332d9-143">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="332d9-143">The changes are highlighted.</span></span> <span data-ttu-id="332d9-144">(Wszystkich znaczników jest *nie* wyświetlane.)</span><span class="sxs-lookup"><span data-stu-id="332d9-144">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="332d9-145">W *Pages/Index.cshtml*, Zastąp zawartość pliku następującym kodem, aby zamienić tekst o platformie ASP.NET i MVC z tekstem o tej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="332d9-145">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="332d9-146">Tworzenie modelu danych</span><span class="sxs-lookup"><span data-stu-id="332d9-146">Create the data model</span></span>

<span data-ttu-id="332d9-147">Tworzenie klas jednostek dla aplikacji Contoso University.</span><span class="sxs-lookup"><span data-stu-id="332d9-147">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="332d9-148">Uruchom przy użyciu następujących trzech jednostek:</span><span class="sxs-lookup"><span data-stu-id="332d9-148">Start with the following three entities:</span></span>

![Diagram modelu danych kurs — rejestracja-ucznia](intro/_static/data-model-diagram.png)

<span data-ttu-id="332d9-150">Istnieje relacja jeden do wielu między `Student` i `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="332d9-150">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="332d9-151">Istnieje relacja jeden do wielu między `Course` i `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="332d9-151">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="332d9-152">Uczniem/uczennicą mogą rejestrować się w dowolnej liczby kursów.</span><span class="sxs-lookup"><span data-stu-id="332d9-152">A student can enroll in any number of courses.</span></span> <span data-ttu-id="332d9-153">Kurs może mieć dowolną liczbę uczniów zarejestrowane w nim.</span><span class="sxs-lookup"><span data-stu-id="332d9-153">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="332d9-154">W poniższych sekcjach tworzenia klasy dla każdego z tych jednostek.</span><span class="sxs-lookup"><span data-stu-id="332d9-154">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="332d9-155">Jednostki dla uczniów</span><span class="sxs-lookup"><span data-stu-id="332d9-155">The Student entity</span></span>

![Diagram jednostek dla uczniów](intro/_static/student-entity.png)

<span data-ttu-id="332d9-157">Tworzenie *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="332d9-157">Create a *Models* folder.</span></span> <span data-ttu-id="332d9-158">W *modeli* folderze utwórz plik klasy o nazwie *Student.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="332d9-158">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="332d9-159">`ID` Właściwość stanie się kolumna klucza podstawowego w tabeli bazy danych (baza danych), która odnosi się do tej klasy.</span><span class="sxs-lookup"><span data-stu-id="332d9-159">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="332d9-160">Domyślnie program EF Core interpretuje właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="332d9-160">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="332d9-161">W `classnameID`, `classname` jest nazwą klasy.</span><span class="sxs-lookup"><span data-stu-id="332d9-161">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="332d9-162">Alternatywnie rozpoznawane automatycznie, klucz podstawowy jest `StudentID` w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="332d9-162">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="332d9-163">`Enrollments` Właściwość [właściwość nawigacji](/ef/core/modeling/relationship).</span><span class="sxs-lookup"><span data-stu-id="332d9-163">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationship).</span></span> <span data-ttu-id="332d9-164">Właściwości nawigacji link do innych jednostek, które są powiązane z tej jednostki.</span><span class="sxs-lookup"><span data-stu-id="332d9-164">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="332d9-165">W tym przypadku `Enrollments` właściwość `Student entity` przechowuje wszystkie `Enrollment` jednostek, które są powiązane z tym, które `Student`.</span><span class="sxs-lookup"><span data-stu-id="332d9-165">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="332d9-166">Na przykład, jeśli wiersz dla uczniów w bazy danych ma dwa powiązane wiersze rejestracji `Enrollments` właściwość nawigacji zawiera tych dwóch `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="332d9-166">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="332d9-167">Powiązane `Enrollment` wiersz jest wierszem, który zawiera ten uczniów wartość klucza podstawowego w `StudentID` kolumny.</span><span class="sxs-lookup"><span data-stu-id="332d9-167">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="332d9-168">Na przykład, załóżmy, że dla uczniów o identyfikatorze = 1 ma dwa wiersze `Enrollment` tabeli.</span><span class="sxs-lookup"><span data-stu-id="332d9-168">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="332d9-169">`Enrollment` Tabela ma dwa wiersze z `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="332d9-169">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="332d9-170">`StudentID` to klucz obcy w `Enrollment` tabela, która określa uczniów w `Student` tabeli.</span><span class="sxs-lookup"><span data-stu-id="332d9-170">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="332d9-171">Jeśli właściwość nawigacji może zawierać wiele jednostek, właściwość nawigacji musi być typu listy, takich jak `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="332d9-171">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="332d9-172">`ICollection<T>` można określić, lub typu, takie jak `List<T>` lub `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="332d9-172">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="332d9-173">Gdy `ICollection<T>` jest używany, tworzy programu EF Core `HashSet<T>` kolekcji domyślnie.</span><span class="sxs-lookup"><span data-stu-id="332d9-173">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="332d9-174">Właściwości nawigacji, które zawierają wiele jednostek pochodzą z relacji wiele do wielu i jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="332d9-174">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="332d9-175">Jednostki rejestracji</span><span class="sxs-lookup"><span data-stu-id="332d9-175">The Enrollment entity</span></span>

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

<span data-ttu-id="332d9-177">W *modeli* folderze utwórz *Enrollment.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="332d9-177">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="332d9-178">`EnrollmentID` Właściwość jest kluczem podstawowym.</span><span class="sxs-lookup"><span data-stu-id="332d9-178">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="332d9-179">Używa tej jednostki `classnameID` wzorca zamiast `ID` takich jak `Student` jednostki.</span><span class="sxs-lookup"><span data-stu-id="332d9-179">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="332d9-180">Zwykle programiści wybierz jednym ze wzorców i używać go w całym modelu danych.</span><span class="sxs-lookup"><span data-stu-id="332d9-180">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="332d9-181">Przy użyciu Identyfikatora bez classname jest wyświetlany później w samouczku, aby ułatwić implementują dziedziczenie w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="332d9-181">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="332d9-182">`Grade` Właściwość `enum`.</span><span class="sxs-lookup"><span data-stu-id="332d9-182">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="332d9-183">Znak zapytania po `Grade` deklaracji typu wskazuje, że `Grade` właściwość ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="332d9-183">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="332d9-184">Ma wartość null, która różni się od zera klasy korporacyjnej — wartość null oznacza, że wartość nie jest znana lub nie została jeszcze przypisana.</span><span class="sxs-lookup"><span data-stu-id="332d9-184">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="332d9-185">`StudentID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Student`.</span><span class="sxs-lookup"><span data-stu-id="332d9-185">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="332d9-186">`Enrollment` Jednostka jest skojarzony z jednym `Student` jednostki, więc ta właściwość zawiera pojedynczy `Student` jednostki.</span><span class="sxs-lookup"><span data-stu-id="332d9-186">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="332d9-187">`Student` Jednostki różni się od `Student.Enrollments` właściwość nawigacji, która zawiera wiele `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="332d9-187">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="332d9-188">`CourseID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Course`.</span><span class="sxs-lookup"><span data-stu-id="332d9-188">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="332d9-189">`Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.</span><span class="sxs-lookup"><span data-stu-id="332d9-189">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="332d9-190">EF Core interpretuje właściwość jako klucz obcy, jeśli jest on nazwany `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="332d9-190">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="332d9-191">Na przykład`StudentID` dla `Student` właściwość nawigacji, ponieważ `Student` jest klucz podstawowy jednostki `ID`.</span><span class="sxs-lookup"><span data-stu-id="332d9-191">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="332d9-192">Może również mieć nazwę właściwości klucza obcego `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="332d9-192">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="332d9-193">Na przykład `CourseID` ponieważ `Course` jest klucz podstawowy jednostki `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="332d9-193">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="332d9-194">Jednostki kursu</span><span class="sxs-lookup"><span data-stu-id="332d9-194">The Course entity</span></span>

![Diagram jednostek kursu](intro/_static/course-entity.png)

<span data-ttu-id="332d9-196">W *modeli* folderze utwórz *Course.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="332d9-196">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="332d9-197">`Enrollments` Właściwość jest właściwością nawigacji.</span><span class="sxs-lookup"><span data-stu-id="332d9-197">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="332d9-198">A `Course` jednostki mogą być one związane z dowolną liczbę `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="332d9-198">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="332d9-199">`DatabaseGenerated` Atrybut umożliwia aplikacji określenie klucza podstawowego, a nie bazy danych po jego wygenerowaniu.</span><span class="sxs-lookup"><span data-stu-id="332d9-199">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="332d9-200">Tworzenie szkieletu modelu dla uczniów</span><span class="sxs-lookup"><span data-stu-id="332d9-200">Scaffold the student model</span></span>

<span data-ttu-id="332d9-201">W tej sekcji jest szkieletu modelu dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="332d9-201">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="332d9-202">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) dla modelu dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="332d9-202">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="332d9-203">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="332d9-203">Build the project.</span></span>
* <span data-ttu-id="332d9-204">Tworzenie *stron/uczniów* folderu.</span><span class="sxs-lookup"><span data-stu-id="332d9-204">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="332d9-205">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="332d9-205">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="332d9-206">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *stron/uczniów* folder > **Dodaj** > **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="332d9-206">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="332d9-207">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **strony Razor za pomocą programu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="332d9-207">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="332d9-208">Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="332d9-208">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="332d9-209">W **klasa modelu** listę rozwijaną, wybierz opcję **uczniów (ContosoUniversity.Models)**.</span><span class="sxs-lookup"><span data-stu-id="332d9-209">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="332d9-210">W **klasa kontekstu danych** wiersz, wybierz opcję **+** (plus) Zaloguj się i Zmień nazwę wygenerowanego na **ContosoUniversity.Models.SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="332d9-210">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="332d9-211">W **klasa kontekstu danych** listę rozwijaną, wybierz opcję **ContosoUniversity.Models.SchoolContext**</span><span class="sxs-lookup"><span data-stu-id="332d9-211">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="332d9-212">Wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="332d9-212">Select **Add**.</span></span>

![Okno dialogowe CRUD](intro/_static/s1.png)

<span data-ttu-id="332d9-214">Zobacz [tworzenia szkieletu modelu movie](xref:tutorials/razor-pages/model#scaffold-the-movie-model) Jeśli masz problem z poprzedniego kroku.</span><span class="sxs-lookup"><span data-stu-id="332d9-214">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="332d9-215">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="332d9-215">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="332d9-216">Uruchom następujące polecenia, aby utworzyć szkielet modelu dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="332d9-216">Run the following commands to scaffold the student model.</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
```
------

<span data-ttu-id="332d9-217">Proces szkieletu tworzonych i zmienianych następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="332d9-217">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="332d9-218">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="332d9-218">Files created</span></span>

* <span data-ttu-id="332d9-219">*Strony/uczniów* tworzenie, edytowanie usuwania, uzyskać szczegółowe informacje, indeksu.</span><span class="sxs-lookup"><span data-stu-id="332d9-219">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="332d9-220">*Data/SchoolContext.cs*</span><span class="sxs-lookup"><span data-stu-id="332d9-220">*Data/SchoolContext.cs*</span></span>

### <a name="files-updates"></a><span data-ttu-id="332d9-221">Pliki aktualizacji</span><span class="sxs-lookup"><span data-stu-id="332d9-221">Files updates</span></span>

* <span data-ttu-id="332d9-222">*Startup.cs* : wyszczególniono zmiany do tego pliku w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="332d9-222">*Startup.cs* : Changes to this file in are detailed the next section.</span></span>
* <span data-ttu-id="332d9-223">*appSettings.JSON* : parametry połączenia używane do łączenia z lokalnej bazy danych zostanie dodany.</span><span class="sxs-lookup"><span data-stu-id="332d9-223">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="332d9-224">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="332d9-224">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="332d9-225">Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="332d9-225">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="332d9-226">Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="332d9-226">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="332d9-227">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="332d9-227">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="332d9-228">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="332d9-228">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="332d9-229">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="332d9-229">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="332d9-230">Sprawdź `ConfigureServices` method in Class metoda *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="332d9-230">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="332d9-231">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="332d9-231">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="332d9-232">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="332d9-232">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="332d9-233">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="332d9-233">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="332d9-234">Zaktualizuj główne</span><span class="sxs-lookup"><span data-stu-id="332d9-234">Update main</span></span>

<span data-ttu-id="332d9-235">W *Program.cs*, zmodyfikuj `Main` metodę, aby wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="332d9-235">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="332d9-236">Pobierz wystąpienia kontekstu bazy danych z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="332d9-236">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="332d9-237">Wywołaj [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span><span class="sxs-lookup"><span data-stu-id="332d9-237">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="332d9-238">Usuń kontekst podczas `EnsureCreated` ukończeniu metody.</span><span class="sxs-lookup"><span data-stu-id="332d9-238">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="332d9-239">Poniższy kod przedstawia zaktualizowanego *Program.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="332d9-239">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="332d9-240">`EnsureCreated` zapewnia, że istnieje baza danych dla kontekstu.</span><span class="sxs-lookup"><span data-stu-id="332d9-240">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="332d9-241">Jeśli istnieje, nie podjęto żadnej akcji.</span><span class="sxs-lookup"><span data-stu-id="332d9-241">If it exists, no action is taken.</span></span> <span data-ttu-id="332d9-242">Jeśli nie istnieje, bazy danych i jego schematu są tworzone.</span><span class="sxs-lookup"><span data-stu-id="332d9-242">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="332d9-243">`EnsureCreated` Używaj migracji do tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="332d9-243">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="332d9-244">Bazy danych, która jest tworzona przy użyciu `EnsureCreated` nie można później zaktualizować przy użyciu migracji.</span><span class="sxs-lookup"><span data-stu-id="332d9-244">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="332d9-245">`EnsureCreated` jest wywoływana przy uruchomieniu aplikacji, co pozwala następujący przepływ pracy:</span><span class="sxs-lookup"><span data-stu-id="332d9-245">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="332d9-246">Usuwanie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="332d9-246">Delete the DB.</span></span>
* <span data-ttu-id="332d9-247">Zmiany schematu bazy danych (na przykład dodać `EmailAddress` pola).</span><span class="sxs-lookup"><span data-stu-id="332d9-247">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="332d9-248">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="332d9-248">Run the app.</span></span>
* <span data-ttu-id="332d9-249">`EnsureCreated` Tworzy BAZĘ danych za pomocą`EmailAddress` kolumny.</span><span class="sxs-lookup"><span data-stu-id="332d9-249">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="332d9-250">`EnsureCreated` jest wygodne na wczesnym etapie projektowania, gdy schemat szybko się zmienia.</span><span class="sxs-lookup"><span data-stu-id="332d9-250">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="332d9-251">Później w samouczku baza danych zostanie usunięty i migracje są używane.</span><span class="sxs-lookup"><span data-stu-id="332d9-251">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="332d9-252">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="332d9-252">Test the app</span></span>

<span data-ttu-id="332d9-253">Uruchom aplikację i zaakceptować zasady plików cookie.</span><span class="sxs-lookup"><span data-stu-id="332d9-253">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="332d9-254">Ta aplikacja nie zachowuje informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="332d9-254">This app doesn't keep personal information.</span></span> <span data-ttu-id="332d9-255">Możesz przeczytać temat zasady plików cookie w [Obsługa Unii Europejskiej ogólnego danych (GDPR Protection Regulation)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="332d9-255">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="332d9-256">Wybierz **studentów** link a następnie **Utwórz nowy**.</span><span class="sxs-lookup"><span data-stu-id="332d9-256">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="332d9-257">Przetestuj Edytuj, szczegóły i usuwać łącza.</span><span class="sxs-lookup"><span data-stu-id="332d9-257">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="332d9-258">Badanie kontekstu SchoolContext DB</span><span class="sxs-lookup"><span data-stu-id="332d9-258">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="332d9-259">Główna klasa, która służy do koordynowania funkcji EF Core dla modelu danych jest z klasy kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="332d9-259">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="332d9-260">Kontekst danych jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="332d9-260">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="332d9-261">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="332d9-261">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="332d9-262">W tym projekcie nosi nazwę klasy `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="332d9-262">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="332d9-263">Aktualizacja *SchoolContext.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="332d9-263">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="332d9-264">Wyróżniony kod tworzy [DbSet\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) właściwości dla każdego zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="332d9-264">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="332d9-265">W terminologii programu EF Core:</span><span class="sxs-lookup"><span data-stu-id="332d9-265">In EF Core terminology:</span></span>

* <span data-ttu-id="332d9-266">Zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="332d9-266">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="332d9-267">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="332d9-267">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="332d9-268">`DbSet<Enrollment>` i `DbSet<Course>` mógł zostać pominięty.</span><span class="sxs-lookup"><span data-stu-id="332d9-268">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="332d9-269">EF Core je uwzględniał niejawnie ponieważ `Student` odwołań do jednostek `Enrollment` jednostki, a `Enrollment` odwołań do jednostek `Course` jednostki.</span><span class="sxs-lookup"><span data-stu-id="332d9-269">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="332d9-270">Na potrzeby tego samouczka Zachowaj `DbSet<Enrollment>` i `DbSet<Course>` w `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="332d9-270">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="332d9-271">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="332d9-271">SQL Server Express LocalDB</span></span>

<span data-ttu-id="332d9-272">Parametry połączenia określają [programu SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="332d9-272">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="332d9-273">LocalDB to Uproszczona wersja aparatu programu SQL Server Express bazy danych i jest przeznaczony do tworzenia aplikacji, a nie do użytku produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="332d9-273">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="332d9-274">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="332d9-274">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="332d9-275">Domyślnie tworzy LocalDB *.mdf* pliki bazy danych `C:/Users/<user>` katalogu.</span><span class="sxs-lookup"><span data-stu-id="332d9-275">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="332d9-276">Dodaj kod, aby zainicjować bazy danych przy użyciu danych testowych</span><span class="sxs-lookup"><span data-stu-id="332d9-276">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="332d9-277">EF Core tworzy pustą bazy danych.</span><span class="sxs-lookup"><span data-stu-id="332d9-277">EF Core creates an empty DB.</span></span> <span data-ttu-id="332d9-278">W tej sekcji `Initialize` metody są zapisywane do wypełniania danych testowych.</span><span class="sxs-lookup"><span data-stu-id="332d9-278">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="332d9-279">W *danych* folderu, Utwórz nowy plik klasy o nazwie *DbInitializer.cs* i Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="332d9-279">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="332d9-280">Kod sprawdza, czy wszystkie studentów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="332d9-280">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="332d9-281">W przypadku nie studentów w bazie danych, baza danych jest inicjowany z danych testowych.</span><span class="sxs-lookup"><span data-stu-id="332d9-281">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="332d9-282">Ładuje dane testowe do tablic zamiast `List<T>` kolekcje w celu zoptymalizowania wydajności.</span><span class="sxs-lookup"><span data-stu-id="332d9-282">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="332d9-283">`EnsureCreated` Metoda automatycznie tworzy bazy danych, aby uzyskać kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="332d9-283">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="332d9-284">Jeśli baza danych istnieje, `EnsureCreated` zwraca bez modyfikowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="332d9-284">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="332d9-285">W *Program.cs*, zmodyfikuj `Main` metodę do wywołania `Initialize`:</span><span class="sxs-lookup"><span data-stu-id="332d9-285">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

<span data-ttu-id="332d9-286">Usuń wszystkie rekordy dla uczniów i uruchom ponownie aplikację.</span><span class="sxs-lookup"><span data-stu-id="332d9-286">Delete any student records and restart the app.</span></span> <span data-ttu-id="332d9-287">Jeśli baza danych nie został zainicjowany, należy ustawić punkt przerwania w `Initialize` Aby zdiagnozować problem.</span><span class="sxs-lookup"><span data-stu-id="332d9-287">If the DB is not initialized, set a break point in `Initialize` to diagnose the problem.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="332d9-288">Widok bazy danych</span><span class="sxs-lookup"><span data-stu-id="332d9-288">View the DB</span></span>

<span data-ttu-id="332d9-289">Otwórz **Eksplorator obiektów SQL Server** (SSOX) z **widoku** menu w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="332d9-289">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="332d9-290">SSOX, kliknij **(localdb) \MSSQLLocalDB > bazy danych > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="332d9-290">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="332d9-291">Rozwiń **tabel** węzła.</span><span class="sxs-lookup"><span data-stu-id="332d9-291">Expand the **Tables** node.</span></span>

<span data-ttu-id="332d9-292">Kliknij prawym przyciskiem myszy **uczniów** tabeli, a następnie kliknij przycisk **dane widoku** kolumn utworzonych i wiersze wstawione do tabeli.</span><span class="sxs-lookup"><span data-stu-id="332d9-292">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="332d9-293">Kod asynchroniczny</span><span class="sxs-lookup"><span data-stu-id="332d9-293">Asynchronous code</span></span>

<span data-ttu-id="332d9-294">Programowanie asynchroniczne jest to domyślny tryb dla platformy ASP.NET Core i programem EF Core.</span><span class="sxs-lookup"><span data-stu-id="332d9-294">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="332d9-295">Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach, w dużym obciążeniem wszystkie dostępne wątki mogło zostać użyte.</span><span class="sxs-lookup"><span data-stu-id="332d9-295">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="332d9-296">Jeśli tak się stanie, serwer nie może przetworzyć nowe żądania aż wątki są zwalniane.</span><span class="sxs-lookup"><span data-stu-id="332d9-296">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="332d9-297">Przy użyciu kodu synchronicznego wiele wątków może powiązane, gdy nie są faktycznie czynności wykonują wszelkie prace ponieważ oczekują one operacji We/Wy zakończyć.</span><span class="sxs-lookup"><span data-stu-id="332d9-297">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="332d9-298">Za pomocą kodu asynchronicznego podczas procesu jest oczekiwania na we/wy ukończyć, jego wątku jest zwalniana dla serwera na potrzeby przetwarzaniem innych żądań.</span><span class="sxs-lookup"><span data-stu-id="332d9-298">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="332d9-299">W rezultacie kod asynchroniczny umożliwia bardziej efektywne wykorzystanie zasobów serwera i serwera jest włączona, aby obsłużyć większy ruch, bez opóźnień.</span><span class="sxs-lookup"><span data-stu-id="332d9-299">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="332d9-300">Kod asynchroniczny wprowadzić niewielkiej ilości obciążenia w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="332d9-300">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="332d9-301">W sytuacjach o niewielkim ruchu wpływający na wydajność jest nieistotny, podczas w sytuacjach dużego ruchu, potencjalna poprawa wydajności jest znacząca.</span><span class="sxs-lookup"><span data-stu-id="332d9-301">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="332d9-302">W poniższym kodzie [async](/dotnet/csharp/language-reference/keywords/async) — słowo kluczowe, `Task<T>` zwracają wartość, `await` — słowo kluczowe, i `ToListAsync` metoda powoduje, że kod, są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="332d9-302">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="332d9-303">`async` — Słowo kluczowe informuje kompilator, aby:</span><span class="sxs-lookup"><span data-stu-id="332d9-303">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="332d9-304">Generuj wywołania zwrotne dla części treści metody.</span><span class="sxs-lookup"><span data-stu-id="332d9-304">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="332d9-305">Automatycznie twórz [zadań](/dotnet/api/system.threading.tasks.task) obiekt, który jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="332d9-305">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="332d9-306">Aby uzyskać więcej informacji, zobacz [typie zwracanym zadań](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="332d9-306">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="332d9-307">Niejawny zwracany typ `Task` reprezentuje pracę w toku.</span><span class="sxs-lookup"><span data-stu-id="332d9-307">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="332d9-308">`await` — Słowo kluczowe powoduje, że kompilator podzielić metodę na dwie części.</span><span class="sxs-lookup"><span data-stu-id="332d9-308">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="332d9-309">Pierwsza część kończy się za pomocą operacji, który jest uruchamiany asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="332d9-309">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="332d9-310">Druga część zostanie przełączone do metody wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.</span><span class="sxs-lookup"><span data-stu-id="332d9-310">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="332d9-311">`ToListAsync` jest to wersja asynchroniczna elementu `ToList` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="332d9-311">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="332d9-312">Kilka rzeczy, w których trzeba pamiętać podczas pisania kodu asynchronicznego, który używa programu EF Core:</span><span class="sxs-lookup"><span data-stu-id="332d9-312">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="332d9-313">Tylko te instrukcje, które powodują zapytań i poleceń do wysłania do bazy danych są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="332d9-313">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="332d9-314">Zawierającej, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, i `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="332d9-314">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="332d9-315">Nie zawiera instrukcji, które można zmienić `IQueryable`, takich jak `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="332d9-315">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="332d9-316">Z kontekstu programu EF Core nie jest bezpieczny dla wątków: nie należy próbować wykonać wiele operacji wykonywane równolegle.</span><span class="sxs-lookup"><span data-stu-id="332d9-316">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="332d9-317">Aby móc korzystać z zalet wydajności kod asynchroniczny, sprawdź, czy biblioteka pakietów (np. stronicowania) używają asynchronicznej, jeśli wywołują metod programu EF Core, które wysyłają zapytania do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="332d9-317">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="332d9-318">Aby uzyskać więcej informacji na temat programowania asynchronicznego w .NET, zobacz [Przegląd Async](/dotnet/articles/standard/async) i [programowanie asynchroniczne z async i await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="332d9-318">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="332d9-319">W następnym samouczku basic CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) są badane.</span><span class="sxs-lookup"><span data-stu-id="332d9-319">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>
::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="332d9-320">Next</span><span class="sxs-lookup"><span data-stu-id="332d9-320">Next</span></span>](xref:data/ef-rp/crud)
