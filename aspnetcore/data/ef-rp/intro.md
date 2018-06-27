---
title: Stron razor z Entity Framework Core w platformy ASP.NET Core - 1 samouczka 8
author: rick-anderson
description: Pokazuje, jak utworzyć aplikację stron Razor przy użyciu programu Entity Framework Core
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/intro
ms.openlocfilehash: 97ae8a0f268d3ca6fb2deee72128714ab6e89266
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961363"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="603b6-103">Stron razor z Entity Framework Core w platformy ASP.NET Core - 1 samouczka 8</span><span class="sxs-lookup"><span data-stu-id="603b6-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="603b6-104">Wersja platformy ASP.NET Core 2.0 tego samouczka można znaleźć w [plik PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/PDF-6-18-18.pdf).</span><span class="sxs-lookup"><span data-stu-id="603b6-104">The ASP.NET Core 2.0 version of this tutorial can be found in [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/PDF-6-18-18.pdf).</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="603b6-105">Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="603b6-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="603b6-106">Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji platformy ASP.NET Core Razor strony za pomocą Core Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="603b6-106">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="603b6-107">Przykładowa aplikacja jest witryną sieci web dla fikcyjnej uniwersytetu Contoso.</span><span class="sxs-lookup"><span data-stu-id="603b6-107">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="603b6-108">Obejmuje funkcje, takie jak wprowadzenia studentów, tworzenie kursu i instruktora przypisania.</span><span class="sxs-lookup"><span data-stu-id="603b6-108">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="603b6-109">Ta strona jest to pierwszy z serii samouczków, które opisują sposób kompilowania aplikacji przykładowej Contoso University.</span><span class="sxs-lookup"><span data-stu-id="603b6-109">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="603b6-110">Pobrania lub wyświetlenia ukończonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="603b6-110">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="603b6-111">[Instrukcje pobierania](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="603b6-111">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="603b6-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="603b6-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="603b6-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="603b6-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="603b6-114">[! OBEJMUJĄ [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="603b6-114">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="603b6-115">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="603b6-115">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="603b6-116">[! [] INCLUDE (~ / includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="603b6-116">[!INCLUDE [](~/includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]</span></span>

------

<span data-ttu-id="603b6-117">Znajomość [stron Razor](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="603b6-117">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="603b6-118">Nowe programistów powinno zakończyć się [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start) przed uruchomieniem tej serii.</span><span class="sxs-lookup"><span data-stu-id="603b6-118">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="603b6-119">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="603b6-119">Troubleshooting</span></span>

<span data-ttu-id="603b6-120">Jeśli napotkasz problem, nie można rozpoznać zwykle można znaleźć rozwiązania na podstawie porównania ilości kodu do [projektu zakończone](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="603b6-120">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="603b6-121">Dobrym sposobem uzyskania pomocy jest publikując pytanie, aby [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) dla [platformy ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="603b6-121">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="603b6-122">Aplikacja sieci web firmy Contoso University</span><span class="sxs-lookup"><span data-stu-id="603b6-122">The Contoso University web app</span></span>

<span data-ttu-id="603b6-123">Aplikacji utworzony w tych samouczkach jest witryną sieci web university podstawowe.</span><span class="sxs-lookup"><span data-stu-id="603b6-123">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="603b6-124">Użytkownicy mogą wyświetlać i uczniów, kursu i informacje o wykładowcach aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="603b6-124">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="603b6-125">Poniżej przedstawiono kilka ekranów utworzone w samouczku.</span><span class="sxs-lookup"><span data-stu-id="603b6-125">Here are a few of the screens created in the tutorial.</span></span>

![Strona indeksu uczniów lub studentów](intro/_static/students-index.png)

![Strona Edytuj uczniów lub studentów](intro/_static/student-edit.png)

<span data-ttu-id="603b6-128">Styl interfejsu użytkownika w tej lokacji jest bliski co to jest generowany przez wbudowane szablony.</span><span class="sxs-lookup"><span data-stu-id="603b6-128">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="603b6-129">Samouczek koncentruje się na podstawowych EF Razor strony, nie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="603b6-129">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="603b6-130">Tworzenie aplikacji sieci web ContosoUniversity stron Razor</span><span class="sxs-lookup"><span data-stu-id="603b6-130">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="603b6-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="603b6-131">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="603b6-132">W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="603b6-132">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="603b6-133">Utwórz nową aplikację sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="603b6-133">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="603b6-134">Nazwij projekt **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="603b6-134">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="603b6-135">Ważne jest, aby nazwa projektu *ContosoUniversity* tak dopasuj przestrzenie nazw, jeśli kod jest skopiowane i wklejone.</span><span class="sxs-lookup"><span data-stu-id="603b6-135">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="603b6-136">Wybierz **platformy ASP.NET Core 2.1** w listy rozwijanej, a następnie wybierz **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="603b6-136">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="603b6-137">W przypadku obrazów powyższych kroków, zobacz [utworzenia aplikacji sieci web Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).</span><span class="sxs-lookup"><span data-stu-id="603b6-137">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).</span></span>
<span data-ttu-id="603b6-138">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="603b6-138">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="603b6-139">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="603b6-139">.NET Core CLI</span></span>](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a><span data-ttu-id="603b6-140">Ustawianie stylów lokacji</span><span class="sxs-lookup"><span data-stu-id="603b6-140">Set up the site style</span></span>

<span data-ttu-id="603b6-141">Kilka zmian ustawienia lokacji menu, układu i strony głównej.</span><span class="sxs-lookup"><span data-stu-id="603b6-141">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="603b6-142">Aktualizacja *Pages/Shared/_Layout.cshtml* z następującymi zmianami:</span><span class="sxs-lookup"><span data-stu-id="603b6-142">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="603b6-143">Zmienić każde wystąpienie "ContosoUniversity" na "Contoso University".</span><span class="sxs-lookup"><span data-stu-id="603b6-143">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="603b6-144">Istnieją trzy wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="603b6-144">There are three occurrences.</span></span>

* <span data-ttu-id="603b6-145">Dodaj elementy menu dla **studentów**, **kursów**, **instruktorów**, i **działów**i Usuń **skontaktuj się z** wpisu menu.</span><span class="sxs-lookup"><span data-stu-id="603b6-145">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="603b6-146">Zmiany zostały wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="603b6-146">The changes are highlighted.</span></span> <span data-ttu-id="603b6-147">(Kod znaczników jest *nie* wyświetlane.)</span><span class="sxs-lookup"><span data-stu-id="603b6-147">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="603b6-148">W *Pages/Index.cshtml*, Zastąp zawartość pliku następującym kodem, aby zastąpić tekst o ASP.NET i MVC tekst o tej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="603b6-148">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="603b6-149">Tworzenie modelu danych</span><span class="sxs-lookup"><span data-stu-id="603b6-149">Create the data model</span></span>

<span data-ttu-id="603b6-150">Tworzenie klas jednostek dla aplikacji Contoso University.</span><span class="sxs-lookup"><span data-stu-id="603b6-150">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="603b6-151">Rozpocząć następujące trzy jednostki:</span><span class="sxs-lookup"><span data-stu-id="603b6-151">Start with the following three entities:</span></span>

![Diagram modelu danych kursu-rejestracji — dla użytkowników domowych](intro/_static/data-model-diagram.png)

<span data-ttu-id="603b6-153">Brak relacji jeden do wielu między `Student` i `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="603b6-153">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="603b6-154">Brak relacji jeden do wielu między `Course` i `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="603b6-154">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="603b6-155">Gdy uczniowie mogą rejestrować się w dowolnej liczby kursów.</span><span class="sxs-lookup"><span data-stu-id="603b6-155">A student can enroll in any number of courses.</span></span> <span data-ttu-id="603b6-156">Kursu może mieć dowolną liczbę studentów zarejestrowane w nim.</span><span class="sxs-lookup"><span data-stu-id="603b6-156">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="603b6-157">W poniższych sekcjach utworzeniu klasy dla każdego z tych obiektów.</span><span class="sxs-lookup"><span data-stu-id="603b6-157">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="603b6-158">Jednostki dla użytkowników domowych</span><span class="sxs-lookup"><span data-stu-id="603b6-158">The Student entity</span></span>

![Diagram jednostek dla użytkowników domowych](intro/_static/student-entity.png)

<span data-ttu-id="603b6-160">Utwórz *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="603b6-160">Create a *Models* folder.</span></span> <span data-ttu-id="603b6-161">W *modele* folderu, Utwórz plik klasy o nazwie *Student.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="603b6-161">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="603b6-162">`ID` Właściwości staje się kolumna klucza podstawowego tabeli bazy danych (bazy danych), która odnosi się do tej klasy.</span><span class="sxs-lookup"><span data-stu-id="603b6-162">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="603b6-163">Domyślnie EF rdzenie będą interpretowane przez właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="603b6-163">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="603b6-164">W `classnameID`, `classname` jest nazwą klasy.</span><span class="sxs-lookup"><span data-stu-id="603b6-164">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="603b6-165">Alternatywne rozpoznawane automatycznie, klucz podstawowy jest `StudentID` w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="603b6-165">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="603b6-166">`Enrollments` Właściwość jest właściwością nawigacji.</span><span class="sxs-lookup"><span data-stu-id="603b6-166">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="603b6-167">Właściwości nawigacji, połącz się z innymi obiektami, które są powiązane z tą jednostką.</span><span class="sxs-lookup"><span data-stu-id="603b6-167">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="603b6-168">W takim przypadku `Enrollments` właściwość `Student entity` przechowuje wszystkie `Enrollment` jednostek, które są powiązane z których `Student`.</span><span class="sxs-lookup"><span data-stu-id="603b6-168">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="603b6-169">Na przykład, jeśli wiersz uczniów w bazie danych ma dwa powiązane wiersze rejestracji `Enrollments` właściwość nawigacji zawiera tych dwóch `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="603b6-169">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="603b6-170">Powiązane `Enrollment` wiersz jest wierszem zawiera Studenta dla tej wartości klucza podstawowego w `StudentID` kolumny.</span><span class="sxs-lookup"><span data-stu-id="603b6-170">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="603b6-171">Na przykład, załóżmy, że uczniów o identyfikatorze = 1 ma dwa wiersze `Enrollment` tabeli.</span><span class="sxs-lookup"><span data-stu-id="603b6-171">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="603b6-172">`Enrollment` Tabela zawiera dwa wiersze z `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="603b6-172">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="603b6-173">`StudentID` jest to kolumna klucza obcego w `Enrollment` tabeli określający student w `Student` tabeli.</span><span class="sxs-lookup"><span data-stu-id="603b6-173">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="603b6-174">Jeśli właściwość nawigacji może zawierać wiele jednostek, właściwość nawigacji musi być typem listy, takich jak `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="603b6-174">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="603b6-175">`ICollection<T>` można określić, takie jak typ lub `List<T>` lub `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="603b6-175">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="603b6-176">Gdy `ICollection<T>` jest używana, tworzy EF Core `HashSet<T>` kolekcji domyślnie.</span><span class="sxs-lookup"><span data-stu-id="603b6-176">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="603b6-177">Właściwości nawigacji, które zawierają wiele jednostek pochodzą z relacji wiele do wielu oraz jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="603b6-177">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="603b6-178">Jednostka rejestracji</span><span class="sxs-lookup"><span data-stu-id="603b6-178">The Enrollment entity</span></span>

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

<span data-ttu-id="603b6-180">W *modele* folderu, Utwórz *Enrollment.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="603b6-180">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="603b6-181">`EnrollmentID` Właściwość jest kluczem podstawowym.</span><span class="sxs-lookup"><span data-stu-id="603b6-181">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="603b6-182">Używa tej jednostki `classnameID` wzorca zamiast `ID` jak `Student` jednostki.</span><span class="sxs-lookup"><span data-stu-id="603b6-182">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="603b6-183">Zwykle programiści wybierz jeden wzorzec i używać go w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="603b6-183">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="603b6-184">W samouczku nowsze za pomocą Identyfikatora bez classname przedstawiono ułatwiające wdrażanie dziedziczenia w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="603b6-184">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="603b6-185">`Grade` Jest właściwość `enum`.</span><span class="sxs-lookup"><span data-stu-id="603b6-185">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="603b6-186">Znak zapytania po `Grade` deklaracji typu wskazuje, że `Grade` właściwość dopuszcza wartość null.</span><span class="sxs-lookup"><span data-stu-id="603b6-186">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="603b6-187">Ma wartość null, która różni się od zera klasy — wartość null oznacza, że klasa nie jest znana lub nie została jeszcze przypisana.</span><span class="sxs-lookup"><span data-stu-id="603b6-187">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="603b6-188">`StudentID` Właściwość jest kluczem obcym i odpowiednią właściwość nawigacji jest `Student`.</span><span class="sxs-lookup"><span data-stu-id="603b6-188">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="603b6-189">`Enrollment` Jednostka jest skojarzony z jednym `Student` jednostki, więc ta właściwość zawiera jeden `Student` jednostki.</span><span class="sxs-lookup"><span data-stu-id="603b6-189">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="603b6-190">`Student` Jednostki różni się od `Student.Enrollments` właściwość nawigacji, która zawiera wiele `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="603b6-190">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="603b6-191">`CourseID` Właściwość jest kluczem obcym i odpowiednią właściwość nawigacji jest `Course`.</span><span class="sxs-lookup"><span data-stu-id="603b6-191">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="603b6-192">`Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.</span><span class="sxs-lookup"><span data-stu-id="603b6-192">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="603b6-193">EF Core interpretuje właściwość jako klucz obcy, jeśli jest o nazwie `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="603b6-193">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="603b6-194">Na przykład`StudentID` dla `Student` właściwość nawigacji, ponieważ `Student` klucza podstawowego jednostki jest `ID`.</span><span class="sxs-lookup"><span data-stu-id="603b6-194">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="603b6-195">Właściwości klucza obcego może również być nazwany `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="603b6-195">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="603b6-196">Na przykład `CourseID` ponieważ `Course` klucza podstawowego jednostki jest `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="603b6-196">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="603b6-197">Jednostki ciągu</span><span class="sxs-lookup"><span data-stu-id="603b6-197">The Course entity</span></span>

![Diagram jednostek kursu](intro/_static/course-entity.png)

<span data-ttu-id="603b6-199">W *modele* folderu, Utwórz *Course.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="603b6-199">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="603b6-200">`Enrollments` Właściwość jest właściwością nawigacji.</span><span class="sxs-lookup"><span data-stu-id="603b6-200">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="603b6-201">A `Course` jednostka może być powiązane z dowolną liczbę `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="603b6-201">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="603b6-202">`DatabaseGenerated` Atrybutu umożliwia aplikacji określenie klucza podstawowego, a nie bazy danych o jego wygenerowaniu.</span><span class="sxs-lookup"><span data-stu-id="603b6-202">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="603b6-203">Tworzenie szkieletu modelu dla użytkowników domowych</span><span class="sxs-lookup"><span data-stu-id="603b6-203">Scaffold the student model</span></span>

<span data-ttu-id="603b6-204">W tej sekcji jest szkieletu modelu studenta.</span><span class="sxs-lookup"><span data-stu-id="603b6-204">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="603b6-205">Oznacza to, że narzędzie szkieletów zawiera strony dla operacji tworzenia, odczytu, aktualizacji i usuwania (CRUD) dla uczniów modelu.</span><span class="sxs-lookup"><span data-stu-id="603b6-205">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="603b6-206">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="603b6-206">Build the project.</span></span>
* <span data-ttu-id="603b6-207">Utwórz *stron/uczniów lub studentów* folderu.</span><span class="sxs-lookup"><span data-stu-id="603b6-207">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="603b6-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="603b6-208">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="603b6-209">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *stron/uczniów lub studentów* folder > **Dodaj** > **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="603b6-209">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="603b6-210">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **stron Razor przy użyciu programu Entity Framework (CRUD)** > **dodać**.</span><span class="sxs-lookup"><span data-stu-id="603b6-210">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="603b6-211">Zakończenie **Dodaj Razor strony przy użyciu programu Entity Framework (CRUD)** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="603b6-211">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="603b6-212">W **klasa modelu** listy rozwijanej, wybierz pozycję **uczniów (ContosoUniversity.Models)**.</span><span class="sxs-lookup"><span data-stu-id="603b6-212">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="603b6-213">W **klasa kontekstu danych** wierszu, wybierz opcję **+** (oraz) Zaloguj się i Zmień nazwę wygenerowanego **ContosoUniversity.Models.SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="603b6-213">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="603b6-214">W **klasa kontekstu danych** listy rozwijanej, wybierz pozycję **ContosoUniversity.Models.SchoolContext**</span><span class="sxs-lookup"><span data-stu-id="603b6-214">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="603b6-215">Wybierz **dodać**.</span><span class="sxs-lookup"><span data-stu-id="603b6-215">Select **Add**.</span></span>

![Okno dialogowe CRUD](intro/_static/s1.png)

<span data-ttu-id="603b6-217">Zobacz [szkieletu modelu filmu](xref:tutorials/razor-pages/model#scaffold-the-movie-model) Jeśli masz problem z poprzedniego kroku.</span><span class="sxs-lookup"><span data-stu-id="603b6-217">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="603b6-218">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="603b6-218">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="603b6-219">Uruchom następujące polecenia, aby utworzyć szkielet uczniów modelu.</span><span class="sxs-lookup"><span data-stu-id="603b6-219">Run the following commands to scaffold the student model.</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
```
------

<span data-ttu-id="603b6-220">Proces szkieletu tworzonych i zmienianych następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="603b6-220">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="603b6-221">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="603b6-221">Files created</span></span>

* <span data-ttu-id="603b6-222">*Strony/uczniów lub studentów* tworzenie, usuwanie i uzyskać szczegółowe informacje, edytowanie indeksu.</span><span class="sxs-lookup"><span data-stu-id="603b6-222">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="603b6-223">*Data/ContosoUniversityContext.cs*</span><span class="sxs-lookup"><span data-stu-id="603b6-223">*Data/ContosoUniversityContext.cs*</span></span>

### <a name="files-updates"></a><span data-ttu-id="603b6-224">Pliki aktualizacji</span><span class="sxs-lookup"><span data-stu-id="603b6-224">Files updates</span></span>

* <span data-ttu-id="603b6-225">*Startup.cs* : wyszczególnione modyfikacje tego pliku w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="603b6-225">*Startup.cs* : Changes to this file in are detailed the next section.</span></span>
* <span data-ttu-id="603b6-226">*appSettings.JSON* : parametry połączenia używane do nawiązania połączenia z lokalną bazą danych został dodany.</span><span class="sxs-lookup"><span data-stu-id="603b6-226">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="603b6-227">Sprawdź kontekst zarejestrowany iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="603b6-227">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="603b6-228">Zbudowany platformy ASP.NET Core za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="603b6-228">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="603b6-229">Usługi (takie jak kontekst danych podstawowych EF) są zarejestrowane w usłudze iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="603b6-229">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="603b6-230">Składniki, które wymagają tych usług (takich jak strony Razor) są udostępniane tych usług za pomocą parametrów konstruktora.</span><span class="sxs-lookup"><span data-stu-id="603b6-230">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="603b6-231">Kod konstruktora, który pobiera wystąpienia kontekstu db przedstawiono w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="603b6-231">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="603b6-232">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i zarejestrowana kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="603b6-232">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="603b6-233">Sprawdź `ConfigureServices` metody w *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="603b6-233">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="603b6-234">Wybrany element został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="603b6-234">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="603b6-235">Nazwa ciągu połączenia jest przekazywany do kontekstu, wywołując metodę na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="603b6-235">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="603b6-236">Dla wdrożenia lokalnego [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="603b6-236">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="603b6-237">Aktualizowanie głównego</span><span class="sxs-lookup"><span data-stu-id="603b6-237">Update main</span></span>

<span data-ttu-id="603b6-238">W *Program.cs*, zmodyfikuj `Main` metody wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="603b6-238">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="603b6-239">Pobierz wystąpienia kontekstu DB z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="603b6-239">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="603b6-240">Wywołanie [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span><span class="sxs-lookup"><span data-stu-id="603b6-240">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="603b6-241">Usuwanie kontekście podczas `EnsureCreated` ukończeniu metody.</span><span class="sxs-lookup"><span data-stu-id="603b6-241">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="603b6-242">Poniższy kod przedstawia zaktualizowanego *Program.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="603b6-242">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="603b6-243">`EnsureCreated` zapewnia, że istnieje bazy danych dla kontekstu.</span><span class="sxs-lookup"><span data-stu-id="603b6-243">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="603b6-244">Jeśli istnieje, nie podjęto żadnej akcji.</span><span class="sxs-lookup"><span data-stu-id="603b6-244">If it exists, no action is taken.</span></span> <span data-ttu-id="603b6-245">Jeśli nie istnieje, bazę danych i jego schematu są tworzone.</span><span class="sxs-lookup"><span data-stu-id="603b6-245">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="603b6-246">`EnsureCreated` nie używa migracji można utworzyć bazy danych.</span><span class="sxs-lookup"><span data-stu-id="603b6-246">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="603b6-247">Bazy danych, który jest tworzony z `EnsureCreated` nie można później zaktualizować przy użyciu migracji.</span><span class="sxs-lookup"><span data-stu-id="603b6-247">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="603b6-248">`EnsureCreated` jest wywoływana po uruchomieniu aplikacji, dzięki czemu następującego przepływu pracy:</span><span class="sxs-lookup"><span data-stu-id="603b6-248">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="603b6-249">Usuń bazę danych.</span><span class="sxs-lookup"><span data-stu-id="603b6-249">Delete the DB.</span></span>
* <span data-ttu-id="603b6-250">Zmień schemat bazy danych (na przykład dodać `EmailAddress` pól).</span><span class="sxs-lookup"><span data-stu-id="603b6-250">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="603b6-251">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="603b6-251">Run the app.</span></span>
* <span data-ttu-id="603b6-252">`EnsureCreated` Tworzy baza danych o`EmailAddress` kolumny.</span><span class="sxs-lookup"><span data-stu-id="603b6-252">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="603b6-253">`EnsureCreated` szybko ewoluuje schematu, jest wygodne wczesnym etapie programowania.</span><span class="sxs-lookup"><span data-stu-id="603b6-253">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="603b6-254">Później w samouczku bazy danych są usuwane i migracji są używane.</span><span class="sxs-lookup"><span data-stu-id="603b6-254">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="603b6-255">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="603b6-255">Test the app</span></span>

<span data-ttu-id="603b6-256">Uruchom aplikację i zaakceptuj zasady pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="603b6-256">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="603b6-257">Ta aplikacja nie zachowanie poufności informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="603b6-257">This app doesn't keep personal information.</span></span> <span data-ttu-id="603b6-258">Informacje o zasad pliku cookie na [obsługę interfejsów UE ogólne dane ochrony rozporządzenia (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="603b6-258">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="603b6-259">Wybierz **studentów** łącza, a następnie **Utwórz nowy**.</span><span class="sxs-lookup"><span data-stu-id="603b6-259">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="603b6-260">Testowanie edycji, uzyskać szczegółowe informacje i Usuń linki.</span><span class="sxs-lookup"><span data-stu-id="603b6-260">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="603b6-261">Sprawdź kontekst bazy danych SchoolContext</span><span class="sxs-lookup"><span data-stu-id="603b6-261">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="603b6-262">Klasy głównym, która koordynuje EF podstawową funkcjonalność o dany model danych jest klasa kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="603b6-262">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="603b6-263">Kontekst danych jest określana na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="603b6-263">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="603b6-264">Kontekst danych określa jednostki, które znajdują się w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="603b6-264">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="603b6-265">W tym projekcie klasy o nazwie `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="603b6-265">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="603b6-266">Aktualizacja *SchoolContext.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="603b6-266">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="603b6-267">Tworzy wyróżniony kod [DbSet\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) właściwości dla każdego zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="603b6-267">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="603b6-268">W terminologii EF rdzeni:</span><span class="sxs-lookup"><span data-stu-id="603b6-268">In EF Core terminology:</span></span>

* <span data-ttu-id="603b6-269">Zwykle zestawu jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="603b6-269">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="603b6-270">Jednostka odpowiada wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="603b6-270">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="603b6-271">`DbSet<Enrollment>` i `DbSet<Course>` może być pominięte.</span><span class="sxs-lookup"><span data-stu-id="603b6-271">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="603b6-272">Podstawowe EF są uwzględniane niejawnie ponieważ `Student` odwołań do jednostek `Enrollment` jednostki i `Enrollment` odwołań do jednostek `Course` jednostki.</span><span class="sxs-lookup"><span data-stu-id="603b6-272">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="603b6-273">W tym samouczku, Zachowaj `DbSet<Enrollment>` i `DbSet<Course>` w `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="603b6-273">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="603b6-274">SQL Server Express LocalDB.</span><span class="sxs-lookup"><span data-stu-id="603b6-274">SQL Server Express LocalDB</span></span>

<span data-ttu-id="603b6-275">Określa parametry połączenia [bazy danych LocalDB programu SQL Server](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="603b6-275">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="603b6-276">LocalDB jest wersja programu SQL Server Express aparatu bazy danych i jest przeznaczony do tworzenia aplikacji, a nie do użytku produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="603b6-276">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="603b6-277">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="603b6-277">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="603b6-278">Domyślnie tworzy LocalDB *.mdf* pliki bazy danych w `C:/Users/<user>` katalogu.</span><span class="sxs-lookup"><span data-stu-id="603b6-278">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="603b6-279">Dodaj kod, aby zainicjować bazy danych z danych testowych</span><span class="sxs-lookup"><span data-stu-id="603b6-279">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="603b6-280">Podstawowe EF tworzy puste bazy danych.</span><span class="sxs-lookup"><span data-stu-id="603b6-280">EF Core creates an empty DB.</span></span> <span data-ttu-id="603b6-281">W tej sekcji `Initialize` zapisywana jest metoda wypełnić je danymi testu.</span><span class="sxs-lookup"><span data-stu-id="603b6-281">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="603b6-282">W *danych* folderu, Utwórz nowy plik klasy o nazwie *DbInitializer.cs* i Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="603b6-282">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="603b6-283">Kod sprawdza, czy wszystkie studentów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="603b6-283">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="603b6-284">Jeśli w bazie danych nie nie studentów, bazy danych jest inicjowany z danych testowych.</span><span class="sxs-lookup"><span data-stu-id="603b6-284">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="603b6-285">Ładuje dane testowe do tablic zamiast `List<T>` kolekcje w celu optymalizacji wydajności.</span><span class="sxs-lookup"><span data-stu-id="603b6-285">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="603b6-286">`EnsureCreated` Metoda automatycznie tworzy bazy danych dla kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="603b6-286">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="603b6-287">Jeśli baza danych istnieje, `EnsureCreated` zwraca bez modyfikowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="603b6-287">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="603b6-288">W *Program.cs*, zmodyfikuj `Main` metodę do wywołania `Initialize`:</span><span class="sxs-lookup"><span data-stu-id="603b6-288">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

<span data-ttu-id="603b6-289">Usuń żadnych rekordów dla użytkowników domowych i uruchom ponownie aplikację.</span><span class="sxs-lookup"><span data-stu-id="603b6-289">Delete any student records and restart the app.</span></span> <span data-ttu-id="603b6-290">Jeśli bazy danych nie został zainicjowany, ustaw punkt przerwania `Initialize` do zdiagnozowania problemu.</span><span class="sxs-lookup"><span data-stu-id="603b6-290">If the DB is not initialized, set a break point in `Initialize` to diagnose the problem.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="603b6-291">Widok bazy danych</span><span class="sxs-lookup"><span data-stu-id="603b6-291">View the DB</span></span>

<span data-ttu-id="603b6-292">Otwórz **Eksplorator obiektów SQL Server** (SSOX) z **widoku** menu w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="603b6-292">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="603b6-293">W SSOX, kliknij przycisk **(localdb) \MSSQLLocalDB > baz danych > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="603b6-293">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="603b6-294">Rozwiń węzeł **tabel** węzła.</span><span class="sxs-lookup"><span data-stu-id="603b6-294">Expand the **Tables** node.</span></span>

<span data-ttu-id="603b6-295">Kliknij prawym przyciskiem myszy **uczniowie** tabeli, a następnie kliknij przycisk **danych widoku** kolumn utworzona i wiersze wstawione do tabeli.</span><span class="sxs-lookup"><span data-stu-id="603b6-295">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="603b6-296">Kod asynchroniczny</span><span class="sxs-lookup"><span data-stu-id="603b6-296">Asynchronous code</span></span>

<span data-ttu-id="603b6-297">Programowanie asynchroniczne jest to domyślny tryb dla platformy ASP.NET Core i EF Core.</span><span class="sxs-lookup"><span data-stu-id="603b6-297">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="603b6-298">Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach wysokie obciążenie wszystkich dostępnych wątków może być używana.</span><span class="sxs-lookup"><span data-stu-id="603b6-298">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="603b6-299">W takim przypadku serwer nie może przetworzyć nowe żądania aż wątki są zwalniane.</span><span class="sxs-lookup"><span data-stu-id="603b6-299">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="603b6-300">Z kodem synchroniczne wiele wątków może powiązany, gdy nie są faktycznie wykonują pracę, ponieważ ich oczekiwania operacji We/Wy zakończyć.</span><span class="sxs-lookup"><span data-stu-id="603b6-300">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="603b6-301">Asynchroniczne kodu podczas procesu jest oczekiwania operacji We/Wy zakończyć, jego wątku zostanie zwolniona dla serwera na potrzeby przetwarzania innych żądań.</span><span class="sxs-lookup"><span data-stu-id="603b6-301">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="603b6-302">W związku z tym kod asynchroniczny umożliwia bardziej efektywne wykorzystanie zasobów serwera, a serwer jest skonfigurowany do obsługi ruchu więcej bez opóźnień.</span><span class="sxs-lookup"><span data-stu-id="603b6-302">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="603b6-303">Asynchroniczne kodu wprowadzenie niewielkiej liczby dodatkowych czynności w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="603b6-303">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="603b6-304">W sytuacjach niski ruchu trafień wydajności jest niewielka, podczas w sytuacjach dużego natężenia ruchu sieciowego, potencjalne zwiększenie wydajności jest znacząca.</span><span class="sxs-lookup"><span data-stu-id="603b6-304">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="603b6-305">W poniższym kodzie [async](/dotnet/csharp/language-reference/keywords/async) — słowo kluczowe, `Task<T>` zwrócić wartość, `await` — słowo kluczowe, i `ToListAsync` metoda powoduje, że kod asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="603b6-305">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="603b6-306">`async` — Słowo kluczowe informuje kompilator, aby:</span><span class="sxs-lookup"><span data-stu-id="603b6-306">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="603b6-307">Generuj wywołania zwrotne dla części treści metody.</span><span class="sxs-lookup"><span data-stu-id="603b6-307">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="603b6-308">Automatyczne tworzenie [zadań](/dotnet/api/system.threading.tasks.task) obiekt, który jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="603b6-308">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="603b6-309">Aby uzyskać więcej informacji, zobacz [typu zwracanego zadań](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="603b6-309">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="603b6-310">Typ zwracany niejawne `Task` reprezentuje pracy w toku.</span><span class="sxs-lookup"><span data-stu-id="603b6-310">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="603b6-311">`await` — Słowo kluczowe powoduje, że kompilator podzielić na dwie części metodę.</span><span class="sxs-lookup"><span data-stu-id="603b6-311">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="603b6-312">Pierwsza część kończy operację, który jest uruchamiany asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="603b6-312">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="603b6-313">Druga część są umieszczane w metodę wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.</span><span class="sxs-lookup"><span data-stu-id="603b6-313">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="603b6-314">`ToListAsync` jest to wersja asynchroniczna elementu `ToList` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="603b6-314">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="603b6-315">Należy pamiętać o podczas pisania kodu asynchroniczne EF Core używa w kilku kwestiach:</span><span class="sxs-lookup"><span data-stu-id="603b6-315">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="603b6-316">Tylko te instrukcje, które powodują zapytań i poleceń do wysłania do bazy danych są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="603b6-316">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="603b6-317">Zawierającej, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, i `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="603b6-317">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="603b6-318">Nie zawiera instrukcji, które można zmienić `IQueryable`, takich jak `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="603b6-318">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="603b6-319">Kontekst EF Core nie jest bezpieczne dla wątków: nie należy próbować wykonać kilka operacji wykonywane równolegle.</span><span class="sxs-lookup"><span data-stu-id="603b6-319">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="603b6-320">Można wykorzystać zalety wydajności async kodu, sprawdź, czy pakiety bibliotekę (takie jak w przypadku stronicowania) używają async Jeśli wywołują metody EF podstawowych, które wysyłanie zapytań do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="603b6-320">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="603b6-321">Aby uzyskać więcej informacji na temat programowania asynchronicznego w programie .NET, zobacz [omówienie Async](/dotnet/articles/standard/async) i [programowanie asynchroniczne z async i await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="603b6-321">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="603b6-322">W następnym samouczku basic CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) operacje są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="603b6-322">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>
::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="603b6-323">Next</span><span class="sxs-lookup"><span data-stu-id="603b6-323">Next</span></span>](xref:data/ef-rp/crud)