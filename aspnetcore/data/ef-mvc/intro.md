---
title: 'Samouczek: Rozpoczynanie pracy z programem EF Core w aplikacji sieci web platformy ASP.NET MVC'
description: Jest to pierwszy z serii samouczków, które wyjaśniają, jak tworzyć Contoso University przykładowej aplikacji od podstaw.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/06/2019
ms.topic: tutorial
uid: data/ef-mvc/intro
ms.openlocfilehash: 5a2ec9a8d85925e55840962cc90b3092953e7920
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59012854"
---
# <a name="tutorial-get-started-with-ef-core-in-an-aspnet-mvc-web-app"></a><span data-ttu-id="13d26-103">Samouczek: Rozpoczynanie pracy z programem EF Core w aplikacji sieci web platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="13d26-103">Tutorial: Get started with EF Core in an ASP.NET MVC web app</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc.md)]

<span data-ttu-id="13d26-104">Przykładową aplikację sieci web firmy Contoso University pokazuje, jak tworzyć aplikacje sieci web platformy ASP.NET Core 2.2 MVC przy użyciu podstawowych funkcji programu Entity Framework (EF) 2.2 i Visual Studio 2017 lub 2019 r.</span><span class="sxs-lookup"><span data-stu-id="13d26-104">The Contoso University sample web application demonstrates how to create ASP.NET Core 2.2 MVC web applications using Entity Framework (EF) Core 2.2 and Visual Studio 2017 or 2019.</span></span>

<span data-ttu-id="13d26-105">Przykładowa aplikacja jest witryną sieci web dla uniwersytetu fikcyjnej firmy Contoso.</span><span class="sxs-lookup"><span data-stu-id="13d26-105">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="13d26-106">Obejmuje funkcje, takie jak czasowej dla uczniów, tworzenia kurs i przypisania instruktora.</span><span class="sxs-lookup"><span data-stu-id="13d26-106">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="13d26-107">Jest to pierwszy z serii samouczków, które wyjaśniają, jak tworzyć Contoso University przykładowej aplikacji od podstaw.</span><span class="sxs-lookup"><span data-stu-id="13d26-107">This is the first in a series of tutorials that explain how to build the Contoso University sample application from scratch.</span></span>

<span data-ttu-id="13d26-108">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="13d26-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="13d26-109">Tworzenie aplikacji internetowej ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="13d26-109">Create an ASP.NET Core MVC web app</span></span>
> * <span data-ttu-id="13d26-110">Ustawianie stylów lokacji</span><span class="sxs-lookup"><span data-stu-id="13d26-110">Set up the site style</span></span>
> * <span data-ttu-id="13d26-111">Dowiedz się więcej o pakietach EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="13d26-111">Learn about EF Core NuGet packages</span></span>
> * <span data-ttu-id="13d26-112">Tworzenie modelu danych</span><span class="sxs-lookup"><span data-stu-id="13d26-112">Create the data model</span></span>
> * <span data-ttu-id="13d26-113">Utwórz kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="13d26-113">Create the database context</span></span>
> * <span data-ttu-id="13d26-114">Zarejestruj kontekstu do wstrzykiwania zależności</span><span class="sxs-lookup"><span data-stu-id="13d26-114">Register the context for dependency injection</span></span>
> * <span data-ttu-id="13d26-115">Inicjowanie bazy danych przy użyciu danych testowych</span><span class="sxs-lookup"><span data-stu-id="13d26-115">Initialize the database with test data</span></span>
> * <span data-ttu-id="13d26-116">Tworzenie kontrolera i widoki</span><span class="sxs-lookup"><span data-stu-id="13d26-116">Create a controller and views</span></span>
> * <span data-ttu-id="13d26-117">Widok bazy danych</span><span class="sxs-lookup"><span data-stu-id="13d26-117">View the database</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13d26-118">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="13d26-118">Prerequisites</span></span>

* [<span data-ttu-id="13d26-119">.NET Core SDK 2.2</span><span class="sxs-lookup"><span data-stu-id="13d26-119">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download)
* <span data-ttu-id="13d26-120">[Visual Studio 2017 lub 2019](https://visualstudio.microsoft.com/downloads/) z następującymi pakietami roboczymi:</span><span class="sxs-lookup"><span data-stu-id="13d26-120">[Visual Studio 2017 or 2019](https://visualstudio.microsoft.com/downloads/) with the following workloads:</span></span>
  * <span data-ttu-id="13d26-121">**ASP.NET i tworzenie aplikacji internetowych** obciążenia</span><span class="sxs-lookup"><span data-stu-id="13d26-121">**ASP.NET and web development** workload</span></span>
  * <span data-ttu-id="13d26-122">**Programowanie dla wielu platform .NET core** obciążenia</span><span class="sxs-lookup"><span data-stu-id="13d26-122">**.NET Core cross-platform development** workload</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="13d26-123">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="13d26-123">Troubleshooting</span></span>

<span data-ttu-id="13d26-124">Jeśli napotkasz problem, nie można rozpoznać ogólnie można znaleźć rozwiązania, porównując swój kod, aby [projektu ukończona](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="13d26-124">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span></span> <span data-ttu-id="13d26-125">Aby uzyskać listę typowych błędów i sposobów ich rozwiązania, zobacz [sekcję Rozwiązywanie problemów w ostatni samouczek z tej serii](advanced.md#common-errors).</span><span class="sxs-lookup"><span data-stu-id="13d26-125">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="13d26-126">Jeśli nie możesz znaleźć, potrzebujesz, możesz zadać pytanie na StackOverflow.com dla [platformy ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [programu EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="13d26-126">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="13d26-127">Jest to szereg 10 samouczków, z których każdy jest oparta na czynności wykonane w starszych samouczków.</span><span class="sxs-lookup"><span data-stu-id="13d26-127">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="13d26-128">Warto zapisać kopię projektu po każdym pomyślnym ukończeniu samouczka.</span><span class="sxs-lookup"><span data-stu-id="13d26-128">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="13d26-129">Następnie, jeśli napotkasz problemy, można uruchomić za pośrednictwem z poprzedniego samouczka zamiast po powrocie do początku całej serii.</span><span class="sxs-lookup"><span data-stu-id="13d26-129">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="contoso-university-web-app"></a><span data-ttu-id="13d26-130">Aplikacja sieci web firmy Contoso University</span><span class="sxs-lookup"><span data-stu-id="13d26-130">Contoso University web app</span></span>

<span data-ttu-id="13d26-131">Aplikacja, której można tworzyć w tych samouczkach znajduje university prostą witrynę sieci web.</span><span class="sxs-lookup"><span data-stu-id="13d26-131">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="13d26-132">Użytkownicy mogą przeglądać i aktualizacji dla uczniów, kursu i informacji przez instruktorów.</span><span class="sxs-lookup"><span data-stu-id="13d26-132">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="13d26-133">Oto kilka ekranów, które zostaną utworzone.</span><span class="sxs-lookup"><span data-stu-id="13d26-133">Here are a few of the screens you'll create.</span></span>

![Strona indeksu uczniów](intro/_static/students-index.png)

![Strona edytowania uczniów](intro/_static/student-edit.png)

## <a name="create-web-app"></a><span data-ttu-id="13d26-136">Tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="13d26-136">Create web app</span></span>

* <span data-ttu-id="13d26-137">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13d26-137">Open Visual Studio.</span></span>

* <span data-ttu-id="13d26-138">Z **pliku** menu, wybierz opcję **nowy > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="13d26-138">From the **File** menu, select **New > Project**.</span></span>

* <span data-ttu-id="13d26-139">W okienku po lewej stronie wybierz **zainstalowane > Visual C# > sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="13d26-139">From the left pane, select **Installed > Visual C# > Web**.</span></span>

* <span data-ttu-id="13d26-140">Wybierz **aplikacji sieci Web programu ASP.NET Core** szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="13d26-140">Select the **ASP.NET Core Web Application** project template.</span></span>

* <span data-ttu-id="13d26-141">Wprowadź **ContosoUniversity** jako nazwę i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="13d26-141">Enter **ContosoUniversity** as the name and click **OK**.</span></span>

  ![Okno dialogowe nowego projektu](intro/_static/new-project2.png)

* <span data-ttu-id="13d26-143">Poczekaj, aż **Nowa aplikacja internetowa ASP.NET Core** wyświetlać okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="13d26-143">Wait for the **New ASP.NET Core Web Application** dialog to appear.</span></span>

* <span data-ttu-id="13d26-144">Wybierz **platformy .NET Core**, **platformy ASP.NET Core 2.2** i **aplikacji sieci Web (Model-View-Controller)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="13d26-144">Select **.NET Core**, **ASP.NET Core 2.2** and the **Web Application (Model-View-Controller)** template.</span></span>

* <span data-ttu-id="13d26-145">Upewnij się, że **uwierzytelniania** ustawiono **bez uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="13d26-145">Make sure **Authentication** is set to **No Authentication**.</span></span>

* <span data-ttu-id="13d26-146">Wybierz **OK**</span><span class="sxs-lookup"><span data-stu-id="13d26-146">Select **OK**</span></span>

  ![Okno dialogowe nowego projektu programu ASP.NET Core](intro/_static/new-aspnet2.png)

## <a name="set-up-the-site-style"></a><span data-ttu-id="13d26-148">Ustawianie stylów lokacji</span><span class="sxs-lookup"><span data-stu-id="13d26-148">Set up the site style</span></span>

<span data-ttu-id="13d26-149">Kilka prostych zmian zostanie skonfigurowany w menu witryny, układu i strony głównej.</span><span class="sxs-lookup"><span data-stu-id="13d26-149">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="13d26-150">Otwórz *Views/Shared/_Layout.cshtml* i wprowadź następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="13d26-150">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="13d26-151">Należy zmienić każde wystąpienie "ContosoUniversity" na "Uniwersytet firmy Contoso".</span><span class="sxs-lookup"><span data-stu-id="13d26-151">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="13d26-152">Istnieją trzy wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="13d26-152">There are three occurrences.</span></span>

* <span data-ttu-id="13d26-153">Dodaj elementy menu dla **o**, **studentów**, **kursów**, **Instruktorzy**, i **działów**, i Usuń **zachowania** wpis menu.</span><span class="sxs-lookup"><span data-stu-id="13d26-153">Add menu entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Privacy** menu entry.</span></span>

<span data-ttu-id="13d26-154">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="13d26-154">The changes are highlighted.</span></span>

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,37-48,63)]

<span data-ttu-id="13d26-155">W *Views/Home/Index.cshtml*, Zastąp zawartość pliku następującym kodem, aby zamienić tekst o platformie ASP.NET i MVC z tekstem o tej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="13d26-155">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

<span data-ttu-id="13d26-156">Naciśnij klawisze CTRL + F5, aby uruchomić projekt, lub wybierz **Debuguj > Uruchom bez debugowania** z menu.</span><span class="sxs-lookup"><span data-stu-id="13d26-156">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="13d26-157">Zostanie wyświetlona strona główna z kartami dla stron, które zostaną utworzone w tych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="13d26-157">You see the home page with tabs for the pages you'll create in these tutorials.</span></span>

![Strona główna University firmy Contoso](intro/_static/home-page.png)

## <a name="about-ef-core-nuget-packages"></a><span data-ttu-id="13d26-159">Temat pakietów programu EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="13d26-159">About EF Core NuGet packages</span></span>

<span data-ttu-id="13d26-160">Aby dodać obsługę programu EF Core do projektu, należy zainstalować dostawcy bazy danych, który ma pod kątem.</span><span class="sxs-lookup"><span data-stu-id="13d26-160">To add EF Core support to a project, install the database provider that you want to target.</span></span> <span data-ttu-id="13d26-161">Ten samouczek używa programu SQL Server i pakiet dostawcy jest [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="13d26-161">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span> <span data-ttu-id="13d26-162">Ten pakiet jest objęta [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), więc nie trzeba odwołują się do pakietu.</span><span class="sxs-lookup"><span data-stu-id="13d26-162">This package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), so you don't need to reference the package.</span></span>

<span data-ttu-id="13d26-163">Pakietu EF programu SQL Server i jego zależności (`Microsoft.EntityFrameworkCore` i `Microsoft.EntityFrameworkCore.Relational`) zapewnia obsługę środowiska uruchomieniowego dla platformy EF.</span><span class="sxs-lookup"><span data-stu-id="13d26-163">The EF SQL Server package and its dependencies (`Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`) provide runtime support for EF.</span></span> <span data-ttu-id="13d26-164">Należy dodać pakiet narzędzi w dalszej części [migracje](migrations.md) samouczka.</span><span class="sxs-lookup"><span data-stu-id="13d26-164">You'll add a tooling package later, in the [Migrations](migrations.md) tutorial.</span></span>

<span data-ttu-id="13d26-165">Aby uzyskać informacje o innych dostawców bazy danych, które są dostępne dla platformy Entity Framework Core, zobacz [bazy danych dostawcy](/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="13d26-165">For information about other database providers that are available for Entity Framework Core, see [Database providers](/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="13d26-166">Tworzenie modelu danych</span><span class="sxs-lookup"><span data-stu-id="13d26-166">Create the data model</span></span>

<span data-ttu-id="13d26-167">Następnie utworzysz klas jednostek dla aplikacji Contoso University.</span><span class="sxs-lookup"><span data-stu-id="13d26-167">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="13d26-168">Będzie rozpoczynać następujących trzech jednostek.</span><span class="sxs-lookup"><span data-stu-id="13d26-168">You'll start with the following three entities.</span></span>

![Diagram modelu danych kurs — rejestracja-ucznia](intro/_static/data-model-diagram.png)

<span data-ttu-id="13d26-170">Istnieje relacja jeden do wielu między `Student` i `Enrollment` jednostek i relacji jeden do wielu między `Course` i `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="13d26-170">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="13d26-171">Innymi słowy uczniem/uczennicą mogą być rejestrowane w dowolnej liczbie kursów i kursu mogą mieć dowolną liczbę uczniów zarejestrowane w nim.</span><span class="sxs-lookup"><span data-stu-id="13d26-171">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="13d26-172">W poniższych sekcjach utworzysz klasy dla każdego z tych jednostek.</span><span class="sxs-lookup"><span data-stu-id="13d26-172">In the following sections you'll create a class for each one of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="13d26-173">Jednostki dla uczniów</span><span class="sxs-lookup"><span data-stu-id="13d26-173">The Student entity</span></span>

![Diagram jednostek dla uczniów](intro/_static/student-entity.png)

<span data-ttu-id="13d26-175">W *modeli* folderze utwórz plik klasy o nazwie *Student.cs* i Zastąp kod szablonu poniższym kodem.</span><span class="sxs-lookup"><span data-stu-id="13d26-175">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="13d26-176">`ID` Właściwości staną się kolumna klucza podstawowego w tabeli bazy danych, która odnosi się do tej klasy.</span><span class="sxs-lookup"><span data-stu-id="13d26-176">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="13d26-177">Domyślnie platforma Entity Framework interpretuje właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="13d26-177">By default, the Entity Framework interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="13d26-178">`Enrollments` Właściwość [właściwość nawigacji](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="13d26-178">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="13d26-179">Właściwości nawigacji przechowywania innych jednostek, które są powiązane z tej jednostki.</span><span class="sxs-lookup"><span data-stu-id="13d26-179">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="13d26-180">W tym przypadku `Enrollments` właściwość `Student entity` pomieścić wszystkie `Enrollment` jednostek, które są powiązane z tym, które `Student` jednostki.</span><span class="sxs-lookup"><span data-stu-id="13d26-180">In this case, the `Enrollments` property of a `Student entity` will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="13d26-181">Innymi słowy, jeśli w danym wierszu dla uczniów w bazie danych ma dwa powiązane wiersze rejestracji (wiersze, które zawierają ten uczniów wartość klucza podstawowego w kolumnie klucza obcego ich StudentID) który `Student` jednostki `Enrollments` właściwość nawigacji będzie zawierać tych dwa `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="13d26-181">In other words, if a given Student row in the database has two related Enrollment rows (rows that contain that student's primary key value in their StudentID foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="13d26-182">Jeśli właściwość nawigacji może zawierać wiele jednostek (tak jak w relacji wiele do wielu lub jeden do wielu), jego typ musi być listy, w którym wpisy mogą być dodawane, usuwane lub zaktualizowane, takich jak `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="13d26-182">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection<T>`.</span></span> <span data-ttu-id="13d26-183">Można określić `ICollection<T>` lub typu, takie jak `List<T>` lub `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="13d26-183">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="13d26-184">Jeśli określisz `ICollection<T>`, tworzy EF `HashSet<T>` kolekcji domyślnie.</span><span class="sxs-lookup"><span data-stu-id="13d26-184">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="13d26-185">Jednostki rejestracji</span><span class="sxs-lookup"><span data-stu-id="13d26-185">The Enrollment entity</span></span>

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

<span data-ttu-id="13d26-187">W *modeli* folderze utwórz *Enrollment.cs* i Zastąp istniejący kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="13d26-187">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="13d26-188">`EnrollmentID` Właściwość będzie miała klucz podstawowy; używa tej jednostki `classnameID` wzorca zamiast `ID` sam jak opisany w `Student` jednostki.</span><span class="sxs-lookup"><span data-stu-id="13d26-188">The `EnrollmentID` property will be the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="13d26-189">Zazwyczaj będzie wybrać jeden wzorzec i używać go w całym modelu danych.</span><span class="sxs-lookup"><span data-stu-id="13d26-189">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="13d26-190">W tym miejscu odmiany ilustruje, czy można używać obu wzorca.</span><span class="sxs-lookup"><span data-stu-id="13d26-190">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="13d26-191">W [dalszych samouczków](inheritance.md), zobaczysz, jak przy użyciu Identyfikatora bez classname ułatwia implementują dziedziczenie w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="13d26-191">In a [later tutorial](inheritance.md), you'll see how using ID without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="13d26-192">`Grade` Właściwość `enum`.</span><span class="sxs-lookup"><span data-stu-id="13d26-192">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="13d26-193">Znak zapytania po `Grade` deklaracji typu wskazuje, że `Grade` właściwość ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="13d26-193">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="13d26-194">Ma wartość null, która różni się od zera klasy korporacyjnej — wartość null oznacza, że wartość nie jest znana lub nie została jeszcze przypisana.</span><span class="sxs-lookup"><span data-stu-id="13d26-194">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="13d26-195">`StudentID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Student`.</span><span class="sxs-lookup"><span data-stu-id="13d26-195">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="13d26-196">`Enrollment` Jednostka jest skojarzony z jednym `Student` jednostki, dzięki czemu właściwość może zawierać tylko jeden `Student` jednostki (w przeciwieństwie do `Student.Enrollments` właściwość nawigacji był wyświetlany poprzednio, która zawiera wiele `Enrollment` jednostek).</span><span class="sxs-lookup"><span data-stu-id="13d26-196">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="13d26-197">`CourseID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Course`.</span><span class="sxs-lookup"><span data-stu-id="13d26-197">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="13d26-198">`Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.</span><span class="sxs-lookup"><span data-stu-id="13d26-198">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="13d26-199">Platforma Entity Framework interpretuje właściwość jako właściwość klucza obcego, jeśli jest on nazwany `<navigation property name><primary key property name>` (na przykład `StudentID` dla `Student` właściwość nawigacji od `Student` jest klucz podstawowy jednostki `ID`).</span><span class="sxs-lookup"><span data-stu-id="13d26-199">Entity Framework interprets a property as a foreign key property if it's named `<navigation property name><primary key property name>` (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="13d26-200">Właściwości klucza obcego może też po prostu nazwę `<primary key property name>` (na przykład `CourseID` ponieważ `Course` jest klucz podstawowy jednostki `CourseID`).</span><span class="sxs-lookup"><span data-stu-id="13d26-200">Foreign key properties can also be named simply `<primary key property name>` (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="13d26-201">Jednostki kursu</span><span class="sxs-lookup"><span data-stu-id="13d26-201">The Course entity</span></span>

![Diagram jednostek kursu](intro/_static/course-entity.png)

<span data-ttu-id="13d26-203">W *modeli* folderze utwórz *Course.cs* i Zastąp istniejący kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="13d26-203">In the *Models* folder, create *Course.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="13d26-204">`Enrollments` Właściwość jest właściwością nawigacji.</span><span class="sxs-lookup"><span data-stu-id="13d26-204">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="13d26-205">A `Course` jednostki mogą być one związane z dowolną liczbę `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="13d26-205">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="13d26-206">Przycisk więcej na temat `DatabaseGenerated` atrybutu w [dalszych samouczków](complex-data-model.md) w tej serii.</span><span class="sxs-lookup"><span data-stu-id="13d26-206">We'll say more about the `DatabaseGenerated` attribute in a [later tutorial](complex-data-model.md) in this series.</span></span> <span data-ttu-id="13d26-207">Zasadniczo ten atrybut umożliwia wprowadzenie klucza podstawowego dla kurs zamiast bazy danych, do jego wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="13d26-207">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="13d26-208">Utwórz kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="13d26-208">Create the database context</span></span>

<span data-ttu-id="13d26-209">Główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework jest klasy kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="13d26-209">The main class that coordinates Entity Framework functionality for a given data model is the database context class.</span></span> <span data-ttu-id="13d26-210">Tworzenie tej klasy, wynikające z `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="13d26-210">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="13d26-211">W kodzie należy określić, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="13d26-211">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="13d26-212">Można również dostosować określone zachowanie programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="13d26-212">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="13d26-213">W tym projekcie nosi nazwę klasy `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="13d26-213">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="13d26-214">W folderze projektu, Utwórz folder o nazwie *danych*.</span><span class="sxs-lookup"><span data-stu-id="13d26-214">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="13d26-215">W *danych* folderze utwórz nowy plik klasy o nazwie *SchoolContext.cs*i Zastąp kod szablonu poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="13d26-215">In the *Data* folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="13d26-216">Ten kod tworzy `DbSet` właściwości dla każdego zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="13d26-216">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="13d26-217">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych, a jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="13d26-217">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<span data-ttu-id="13d26-218">Można już pominięto `DbSet<Enrollment>` i `DbSet<Course>` instrukcji i będzie działać tak samo.</span><span class="sxs-lookup"><span data-stu-id="13d26-218">You could've omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="13d26-219">Entity Framework będzie je uwzględnić niejawnie, ponieważ `Student` odwołań do jednostek `Enrollment` jednostki i `Enrollment` odwołań do jednostek `Course` jednostki.</span><span class="sxs-lookup"><span data-stu-id="13d26-219">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="13d26-220">Po utworzeniu bazy danych EF tworzy tabel, które mają takie same jak nazwy `DbSet` nazwy właściwości.</span><span class="sxs-lookup"><span data-stu-id="13d26-220">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="13d26-221">Zazwyczaj są to nazwy właściwości dla kolekcji (uczniów zamiast ucznia) w liczbie mnogiej, ale deweloperzy nie zgadzają się dotyczące tego, czy nazwy tabel należy pluralized czy nie.</span><span class="sxs-lookup"><span data-stu-id="13d26-221">Property names for collections are typically plural (Students rather than Student), but developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="13d26-222">Te samouczki będzie zastąpić domyślne zachowanie przez określenie nazwy w liczbie pojedynczej tabeli w kontekstu DbContext.</span><span class="sxs-lookup"><span data-stu-id="13d26-222">For these tutorials you'll override the default behavior by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="13d26-223">Aby to zrobić, Dodaj następujący wyróżniony kod po ostatniej właściwości DbSet.</span><span class="sxs-lookup"><span data-stu-id="13d26-223">To do that, add the following highlighted code after the last DbSet property.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-schoolcontext"></a><span data-ttu-id="13d26-224">Zarejestruj SchoolContext</span><span class="sxs-lookup"><span data-stu-id="13d26-224">Register the SchoolContext</span></span>

<span data-ttu-id="13d26-225">Implementuje platformy ASP.NET Core [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md) domyślnie.</span><span class="sxs-lookup"><span data-stu-id="13d26-225">ASP.NET Core implements [dependency injection](../../fundamentals/dependency-injection.md) by default.</span></span> <span data-ttu-id="13d26-226">Usługi (takie jak EF kontekst bazy danych) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="13d26-226">Services (such as the EF database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="13d26-227">Składniki, które wymagają tych usług, (na przykład kontrolerów MVC) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="13d26-227">Components that require these services (such as MVC controllers) are provided these services via constructor parameters.</span></span> <span data-ttu-id="13d26-228">Zobaczysz kod konstruktora kontrolera, pobierające wystąpienie kontekstu w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="13d26-228">You'll see the controller constructor code that gets a context instance later in this tutorial.</span></span>

<span data-ttu-id="13d26-229">Aby zarejestrować `SchoolContext` jako usługi, otwórz *Startup.cs*i Dodaj wyróżnione wiersze w celu `ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="13d26-229">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=9-10)]

<span data-ttu-id="13d26-230">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na `DbContextOptionsBuilder` obiektu.</span><span class="sxs-lookup"><span data-stu-id="13d26-230">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="13d26-231">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="13d26-231">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="13d26-232">Dodaj `using` instrukcji dla `ContosoUniversity.Data` i `Microsoft.EntityFrameworkCore` przestrzeni nazw, a następnie Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="13d26-232">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces, and then build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="13d26-233">Otwórz *appsettings.json* pliku i dodaj ciąg połączenia, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="13d26-233">Open the *appsettings.json* file and add a connection string as shown in the following example.</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a><span data-ttu-id="13d26-234">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="13d26-234">SQL Server Express LocalDB</span></span>

<span data-ttu-id="13d26-235">Parametry połączenia określają bazę danych programu SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="13d26-235">The connection string specifies a SQL Server LocalDB database.</span></span> <span data-ttu-id="13d26-236">LocalDB to Uproszczona wersja aparatu programu SQL Server Express bazy danych i jest przeznaczony do tworzenia aplikacji, nie do użytku produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="13d26-236">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for application development, not production use.</span></span> <span data-ttu-id="13d26-237">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="13d26-237">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="13d26-238">Domyślnie tworzy LocalDB *.mdf* bazy danych plików w `C:/Users/<user>` katalogu.</span><span class="sxs-lookup"><span data-stu-id="13d26-238">By default, LocalDB creates *.mdf* database files in the `C:/Users/<user>` directory.</span></span>

## <a name="initialize-db-with-test-data"></a><span data-ttu-id="13d26-239">Zainicjuj kontekst bazy danych przy użyciu danych testowych</span><span class="sxs-lookup"><span data-stu-id="13d26-239">Initialize DB with test data</span></span>

<span data-ttu-id="13d26-240">Entity Framework utworzy pustą bazę danych dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="13d26-240">The Entity Framework will create an empty database for you.</span></span> <span data-ttu-id="13d26-241">W tej sekcji możesz napisanie metody, która jest wywoływana po utworzeniu bazy danych, aby wypełnić je danymi testu.</span><span class="sxs-lookup"><span data-stu-id="13d26-241">In this section, you write a method that's called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="13d26-242">W tym miejscu użyjemy `EnsureCreated` metodę, aby automatycznie utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="13d26-242">Here you'll use the `EnsureCreated` method to automatically create the database.</span></span> <span data-ttu-id="13d26-243">W [dalszych samouczków](migrations.md) zobaczysz jak obsługiwać zmiany modelu, używając migracje Code First do zmiany schematu bazy danych zamiast porzucenie i ponowne utworzenie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="13d26-243">In a [later tutorial](migrations.md) you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="13d26-244">W *danych* folderu, Utwórz nowy plik klasy o nazwie *DbInitializer.cs* i Zastąp kod szablonu poniższym kodem, co powoduje, że bazy danych ma zostać utworzony, w razie i ładowania testów danych do nowego Baza danych.</span><span class="sxs-lookup"><span data-stu-id="13d26-244">In the *Data* folder, create a new class file named *DbInitializer.cs* and replace the template code with the following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="13d26-245">Kod sprawdza, czy istnieją wszystkie studentów w bazie danych, a jeśli nie, zakłada się bazy danych jest nowa i musi zostać rozpoczęta z danymi.</span><span class="sxs-lookup"><span data-stu-id="13d26-245">The code checks if there are any students in the database, and if not, it assumes the database is new and needs to be seeded with test data.</span></span> <span data-ttu-id="13d26-246">Ładuje dane testowe do tablic zamiast `List<T>` kolekcje w celu zoptymalizowania wydajności.</span><span class="sxs-lookup"><span data-stu-id="13d26-246">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="13d26-247">W *Program.cs*, zmodyfikuj `Main` metodę, aby wykonać następujące czynności podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="13d26-247">In *Program.cs*, modify the `Main` method to do the following on application startup:</span></span>

* <span data-ttu-id="13d26-248">Pobierz wystąpienia kontekstu bazy danych z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="13d26-248">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="13d26-249">Wywołaj metodę inicjatora, przekazując mu kontekst.</span><span class="sxs-lookup"><span data-stu-id="13d26-249">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="13d26-250">Po zakończeniu seed — metoda, należy dysponować kontekstu.</span><span class="sxs-lookup"><span data-stu-id="13d26-250">Dispose the context when the seed method is done.</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

<span data-ttu-id="13d26-251">Dodaj `using` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="13d26-251">Add `using` statements:</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

<span data-ttu-id="13d26-252">W samouczkach starsze, może zostać wyświetlony podobny kod w `Configure` method in Class metoda *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="13d26-252">In older tutorials, you may see similar code in the `Configure` method in *Startup.cs*.</span></span> <span data-ttu-id="13d26-253">Firma Microsoft zaleca użycie `Configure` metody tylko po to, aby skonfigurować Potok żądań.</span><span class="sxs-lookup"><span data-stu-id="13d26-253">We recommend that you use the `Configure` method only to set up the request pipeline.</span></span> <span data-ttu-id="13d26-254">Należy do kodu uruchamiania aplikacji `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="13d26-254">Application startup code belongs in the `Main` method.</span></span>

<span data-ttu-id="13d26-255">Teraz podczas pierwszego uruchomienia aplikacji, baza danych zostanie utworzona i zasilany z danymi.</span><span class="sxs-lookup"><span data-stu-id="13d26-255">Now the first time you run the application, the database will be created and seeded with test data.</span></span> <span data-ttu-id="13d26-256">Zawsze wtedy, gdy zmienisz swój model danych, można usunąć bazę danych, zaktualizować seed — metoda i rozpoczynać się od nowa nową bazę danych tak samo.</span><span class="sxs-lookup"><span data-stu-id="13d26-256">Whenever you change your data model, you can delete the database, update your seed method, and start afresh with a new database the same way.</span></span> <span data-ttu-id="13d26-257">W kolejnych samouczkach pokazano, jak zmodyfikować bazy danych, gdy dane modelu zmiany, bez wcześniejszego usunięcia i ponownego utworzenia go.</span><span class="sxs-lookup"><span data-stu-id="13d26-257">In later tutorials, you'll see how to modify the database when the data model changes, without deleting and re-creating it.</span></span>

## <a name="create-controller-and-views"></a><span data-ttu-id="13d26-258">Tworzenie widoków i kontrolerów</span><span class="sxs-lookup"><span data-stu-id="13d26-258">Create controller and views</span></span>

<span data-ttu-id="13d26-259">Następnie użyjemy aparatu tworzenia szkieletów w programie Visual Studio można dodać kontroler MVC i widoki, które użyje EF w celu wykonywania zapytań i zapisywanie danych.</span><span class="sxs-lookup"><span data-stu-id="13d26-259">Next, you'll use the scaffolding engine in Visual Studio to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="13d26-260">Automatyczne tworzenie widoków i CRUD metody akcji jest określana jako tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="13d26-260">The automatic creation of CRUD action methods and views is known as scaffolding.</span></span> <span data-ttu-id="13d26-261">Tworzenie szkieletu różni się od generowania kodu, w tym utworzony szkielet kodu stanowi punkt wyjścia, który można dostosować do własnych własnych wymagań, dlatego należy zwykle nie należy modyfikować wygenerowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="13d26-261">Scaffolding differs from code generation in that the scaffolded code is a starting point that you can modify to suit your own requirements, whereas you typically don't modify generated code.</span></span> <span data-ttu-id="13d26-262">Należy dostosować wygenerowany kod, użyj klasy częściowe lub ponownego generowania kodu w przypadku zmian.</span><span class="sxs-lookup"><span data-stu-id="13d26-262">When you need to customize generated code, you use partial classes or you regenerate the code when things change.</span></span>

* <span data-ttu-id="13d26-263">Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **Dodaj > Nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="13d26-263">Right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

* <span data-ttu-id="13d26-264">W **Dodawanie szkieletu** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="13d26-264">In the **Add Scaffold** dialog box:</span></span>

  * <span data-ttu-id="13d26-265">Wybierz **kontroler MVC z widokami używający narzędzia Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="13d26-265">Select **MVC controller with views, using Entity Framework**.</span></span>

  * <span data-ttu-id="13d26-266">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="13d26-266">Click **Add**.</span></span> <span data-ttu-id="13d26-267">**Dodaj kontroler MVC z widokami używający narzędzia Entity Framework** pojawi się okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="13d26-267">The **Add MVC Controller with views, using Entity Framework** dialog box appears.</span></span>

    ![Tworzenie szkieletu ucznia](intro/_static/scaffold-student2.png)

  * <span data-ttu-id="13d26-269">W **klasa modelu** wybierz **uczniów**.</span><span class="sxs-lookup"><span data-stu-id="13d26-269">In **Model class** select **Student**.</span></span>

  * <span data-ttu-id="13d26-270">W **klasa kontekstu danych** wybierz **SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="13d26-270">In **Data context class** select **SchoolContext**.</span></span>

  * <span data-ttu-id="13d26-271">Zaakceptuj wartość domyślną **StudentsController** jako nazwę.</span><span class="sxs-lookup"><span data-stu-id="13d26-271">Accept the default **StudentsController** as the name.</span></span>

  * <span data-ttu-id="13d26-272">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="13d26-272">Click **Add**.</span></span>

  <span data-ttu-id="13d26-273">Po kliknięciu **Dodaj**, aparat tworzenia szkieletów programu Visual Studio tworzy *StudentsController.cs* plików i zestaw widoków (*.cshtml* plików), pracować z kontrolerem.</span><span class="sxs-lookup"><span data-stu-id="13d26-273">When you click **Add**, the Visual Studio scaffolding engine creates a *StudentsController.cs* file and a set of views (*.cshtml* files) that work with the controller.</span></span>

<span data-ttu-id="13d26-274">(Aparat tworzenia szkieletów można również utworzyć kontekst bazy danych dla Ciebie nie utworzenie go ręcznie najpierw miało to miejsce wcześniej w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="13d26-274">(The scaffolding engine can also create the database context for you if you don't create it manually first as you did earlier for this tutorial.</span></span> <span data-ttu-id="13d26-275">Można określić nową klasę kontekstu w **Dodaj kontroler** pola, klikając znak plus, aby po prawej stronie **klasa kontekstu danych**.</span><span class="sxs-lookup"><span data-stu-id="13d26-275">You can specify a new context class in the **Add Controller** box by clicking the plus sign to the right of **Data context class**.</span></span>  <span data-ttu-id="13d26-276">Visual Studio utworzy swoje `DbContext` klasy, a także kontrolera i widoki.)</span><span class="sxs-lookup"><span data-stu-id="13d26-276">Visual Studio will then create your `DbContext` class as well as the controller and views.)</span></span>

<span data-ttu-id="13d26-277">Można zauważyć, że kontroler trwa `SchoolContext` jako parametr konstruktora.</span><span class="sxs-lookup"><span data-stu-id="13d26-277">You'll notice that the controller takes a `SchoolContext` as a constructor parameter.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

<span data-ttu-id="13d26-278">Wstrzykiwanie zależności platformy ASP.NET Core dba o przekazanie wystąpienia `SchoolContext` do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="13d26-278">ASP.NET Core dependency injection takes care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="13d26-279">Skonfigurowano w *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="13d26-279">You configured that in the *Startup.cs* file earlier.</span></span>

<span data-ttu-id="13d26-280">Zawiera kontroler `Index` metody akcji, który wyświetla wszystkich studentów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="13d26-280">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="13d26-281">Metoda pobiera listę uczniów z zestawu, czytając jednostek studentów `Students` właściwości wystąpienia kontekstu bazy danych:</span><span class="sxs-lookup"><span data-stu-id="13d26-281">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

<span data-ttu-id="13d26-282">W dalszej części tego samouczka, dowiesz się o asynchronicznych elementów programowania, w tym kodzie.</span><span class="sxs-lookup"><span data-stu-id="13d26-282">You'll learn about the asynchronous programming elements in this code later in the tutorial.</span></span>

<span data-ttu-id="13d26-283">*Views/Students/Index.cshtml* widoku tej liście są wyświetlane w tabeli:</span><span class="sxs-lookup"><span data-stu-id="13d26-283">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="13d26-284">Naciśnij klawisze CTRL + F5, aby uruchomić projekt, lub wybierz **Debuguj > Uruchom bez debugowania** z menu.</span><span class="sxs-lookup"><span data-stu-id="13d26-284">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="13d26-285">Kliknij kartę studentów, aby wyświetlić dane z badań, `DbInitializer.Initialize` metoda wstawiony.</span><span class="sxs-lookup"><span data-stu-id="13d26-285">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="13d26-286">W zależności od tego, jak wąskie okno przeglądarki jest, zobaczysz `Students` należy łącze kartę w górnej części strony lub kliknij ikonę nawigacji w prawym górnym rogu, aby zobaczyć łącza.</span><span class="sxs-lookup"><span data-stu-id="13d26-286">Depending on how narrow your browser window is, you'll see the `Students` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![Strona główna firmy Contoso University wąskie](intro/_static/home-page-narrow.png)

![Strona indeksu uczniów](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="13d26-289">Widok bazy danych</span><span class="sxs-lookup"><span data-stu-id="13d26-289">View the database</span></span>

<span data-ttu-id="13d26-290">Po uruchomieniu aplikacji, `DbInitializer.Initialize` wywołania metody `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="13d26-290">When you started the application, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="13d26-291">EF pokazano, że wystąpił brak bazy danych, a więc utworzyć jeden, a następnie w pozostałej części `Initialize` kod metody wypełnione w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="13d26-291">EF saw that there was no database and so it created one, then the remainder of the `Initialize` method code populated the database with data.</span></span> <span data-ttu-id="13d26-292">Możesz użyć **Eksplorator obiektów SQL Server** (SSOX), aby wyświetlić bazy danych w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13d26-292">You can use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span>

<span data-ttu-id="13d26-293">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="13d26-293">Close the browser.</span></span>

<span data-ttu-id="13d26-294">Jeśli okno SSOX nie jest jeszcze otwarty, wybierz go z **widoku** menu w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13d26-294">If the SSOX window isn't already open, select it from the **View** menu in Visual Studio.</span></span>

<span data-ttu-id="13d26-295">SSOX, kliknij **(localdb) \MSSQLLocalDB > bazy danych**, a następnie kliknij wpis dla nazwy bazy danych, która znajduje się w ciągu połączenia w Twojej *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="13d26-295">In SSOX, click **(localdb)\MSSQLLocalDB > Databases**, and then click the entry for the database name that's in the connection string in your *appsettings.json* file.</span></span>

<span data-ttu-id="13d26-296">Rozwiń **tabel** węzeł, aby zobaczyć tabele w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="13d26-296">Expand the **Tables** node to see the tables in your database.</span></span>

![Tabele w SSOX](intro/_static/ssox-tables.png)

<span data-ttu-id="13d26-298">Kliknij prawym przyciskiem myszy **uczniów** tabeli, a następnie kliknij przycisk **dane widoku** kolumn, które zostały utworzone i wierszy, które zostały wstawione do tabeli.</span><span class="sxs-lookup"><span data-stu-id="13d26-298">Right-click the **Student** table and click **View Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

![Tabela student w SSOX](intro/_static/ssox-student-table.png)

<span data-ttu-id="13d26-300">*.Mdf* i *ldf* pliki bazy danych znajdują się w *C:\Users\\\<nazwa_użytkownika >* folderu.</span><span class="sxs-lookup"><span data-stu-id="13d26-300">The *.mdf* and *.ldf* database files are in the *C:\Users\\\<yourusername>* folder.</span></span>

<span data-ttu-id="13d26-301">Ponieważ w przypadku wywoływania `EnsureCreated` w metodzie inicjatora, która działa na uruchomienie aplikacji, można teraz wprowadzić zmianę do `Student` klasy, Usuń bazę danych, uruchom ponownie aplikację i bazy danych będzie automatycznie ponownie tworzone, aby dopasować zmiany.</span><span class="sxs-lookup"><span data-stu-id="13d26-301">Because you're calling `EnsureCreated` in the initializer method that runs on app start, you could now make a change to the `Student` class, delete the database, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="13d26-302">Na przykład jeśli dodasz `EmailAddress` właściwości `Student` klasy, zostanie wyświetlony nowy `EmailAddress` kolumny w tabeli odtworzony.</span><span class="sxs-lookup"><span data-stu-id="13d26-302">For example, if you add an `EmailAddress` property to the `Student` class, you'll see a new `EmailAddress` column in the re-created table.</span></span>

## <a name="conventions"></a><span data-ttu-id="13d26-303">Konwencje</span><span class="sxs-lookup"><span data-stu-id="13d26-303">Conventions</span></span>

<span data-ttu-id="13d26-304">Ilość kodu, który trzeba było pisać w kolejności programu Entity Framework można było utworzyć pełną bazę danych dla Ciebie jest minimalny, ze względu na użycie Konwencji lub założenia, które czynią Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="13d26-304">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of conventions, or assumptions that the Entity Framework makes.</span></span>

* <span data-ttu-id="13d26-305">Nazwy `DbSet` właściwości są używane jako nazwy tabeli.</span><span class="sxs-lookup"><span data-stu-id="13d26-305">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="13d26-306">W przypadku jednostek, które nie odwołują się `DbSet` właściwość, klasa jednostki, które nazwy są używane jako nazwy tabeli.</span><span class="sxs-lookup"><span data-stu-id="13d26-306">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="13d26-307">Nazwy właściwości jednostki są używane dla nazw kolumn.</span><span class="sxs-lookup"><span data-stu-id="13d26-307">Entity property names are used for column names.</span></span>

* <span data-ttu-id="13d26-308">Właściwości jednostki, które noszą nazwy lub classnameID są rozpoznawane jako właściwości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="13d26-308">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="13d26-309">Właściwość jest interpretowany jako właściwość klucza obcego, jeśli jest on nazwany  *\<nazwy właściwości nawigacji >\<nazwa właściwość klucza podstawowego >* (na przykład `StudentID` dla `Student` nawigacji Właściwość od `Student` jest klucz podstawowy jednostki `ID`).</span><span class="sxs-lookup"><span data-stu-id="13d26-309">A property is interpreted as a foreign key property if it's named *\<navigation property name>\<primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="13d26-310">Właściwości klucza obcego może też po prostu nazwę  *\<właściwość klucza podstawowego name >* (na przykład `EnrollmentID` ponieważ `Enrollment` jest klucz podstawowy jednostki `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="13d26-310">Foreign key properties can also be named simply *\<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="13d26-311">Konwencjonalne zachowanie można przesłonić.</span><span class="sxs-lookup"><span data-stu-id="13d26-311">Conventional behavior can be overridden.</span></span> <span data-ttu-id="13d26-312">Na przykład można jawnie określasz nazwy tabel, jak przedstawiono wcześniej w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="13d26-312">For example, you can explicitly specify table names, as you saw earlier in this tutorial.</span></span> <span data-ttu-id="13d26-313">I można ustawić nazwy kolumn i ustaw dowolną właściwość jako klucz podstawowy lub klucz obcy, jak można zauważyć w [dalszych samouczków](complex-data-model.md) w tej serii.</span><span class="sxs-lookup"><span data-stu-id="13d26-313">And you can set column names and set any property as primary key or foreign key, as you'll see in a [later tutorial](complex-data-model.md) in this series.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="13d26-314">Kod asynchroniczny</span><span class="sxs-lookup"><span data-stu-id="13d26-314">Asynchronous code</span></span>

<span data-ttu-id="13d26-315">Programowanie asynchroniczne jest to domyślny tryb dla platformy ASP.NET Core i programem EF Core.</span><span class="sxs-lookup"><span data-stu-id="13d26-315">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="13d26-316">Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach, w dużym obciążeniem wszystkie dostępne wątki mogło zostać użyte.</span><span class="sxs-lookup"><span data-stu-id="13d26-316">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="13d26-317">Jeśli tak się stanie, serwer nie może przetworzyć nowe żądania aż wątki są zwalniane.</span><span class="sxs-lookup"><span data-stu-id="13d26-317">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="13d26-318">Przy użyciu kodu synchronicznego wiele wątków może powiązane, gdy nie są faktycznie czynności wykonują wszelkie prace ponieważ oczekują one operacji We/Wy zakończyć.</span><span class="sxs-lookup"><span data-stu-id="13d26-318">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="13d26-319">Za pomocą kodu asynchronicznego podczas procesu jest oczekiwania na we/wy ukończyć, jego wątku jest zwalniana dla serwera na potrzeby przetwarzaniem innych żądań.</span><span class="sxs-lookup"><span data-stu-id="13d26-319">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="13d26-320">W rezultacie kod asynchroniczny umożliwia bardziej efektywne wykorzystanie zasobów serwera i serwera jest włączona, aby obsłużyć większy ruch, bez opóźnień.</span><span class="sxs-lookup"><span data-stu-id="13d26-320">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="13d26-321">Kod asynchroniczny wprowadzają niewielkiej ilości obciążenia w czasie wykonywania, ale wpływający na wydajność jest nieistotny, podczas w sytuacjach dużego ruchu sytuacjach o niewielkim ruchu, istotne jest potencjalna poprawa wydajności.</span><span class="sxs-lookup"><span data-stu-id="13d26-321">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="13d26-322">W poniższym kodzie `async` — słowo kluczowe, `Task<T>` zwracają wartość, `await` — słowo kluczowe, i `ToListAsync` metoda powoduje, że kod, są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="13d26-322">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="13d26-323">`async` — Słowo kluczowe informuje kompilator, aby wygenerować wywołania zwrotne dla części treści metody, a także automatyczne tworzenie `Task<IActionResult>` obiekt, który jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="13d26-323">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that's returned.</span></span>

* <span data-ttu-id="13d26-324">Zwracany typ `Task<IActionResult>` reprezentuje pracę w toku z wyniku typu `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="13d26-324">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>

* <span data-ttu-id="13d26-325">`await` — Słowo kluczowe powoduje, że kompilator podzielić metodę na dwie części.</span><span class="sxs-lookup"><span data-stu-id="13d26-325">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="13d26-326">Pierwsza część kończy się za pomocą operacji, który jest uruchamiany asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="13d26-326">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="13d26-327">Druga część zostanie przełączone do metody wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.</span><span class="sxs-lookup"><span data-stu-id="13d26-327">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="13d26-328">`ToListAsync` jest to wersja asynchroniczna elementu `ToList` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="13d26-328">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="13d26-329">Niektóre elementy pod uwagę podczas pisania kodu asynchronicznego, który używa programu Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="13d26-329">Some things to be aware of when you are writing asynchronous code that uses the Entity Framework:</span></span>

* <span data-ttu-id="13d26-330">Tylko te instrukcje, które powodują zapytań i poleceń do wysłania do bazy danych są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="13d26-330">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="13d26-331">Zawierającej, na przykład `ToListAsync`, `SingleOrDefaultAsync`, i `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="13d26-331">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="13d26-332">Nie zawiera, na przykład instrukcji, które można zmienić `IQueryable`, takich jak `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="13d26-332">It doesn't include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="13d26-333">Kontekst EF nie jest bezpieczny dla wątków: nie należy próbować wykonać wiele operacji wykonywane równolegle.</span><span class="sxs-lookup"><span data-stu-id="13d26-333">An EF context isn't thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="13d26-334">Po wywołaniu każda metoda asynchroniczna, EF, należy zawsze używać `await` — słowo kluczowe.</span><span class="sxs-lookup"><span data-stu-id="13d26-334">When you call any async EF method, always use the `await` keyword.</span></span>

* <span data-ttu-id="13d26-335">Jeśli chcesz skorzystać z zalet wydajności kod asynchroniczny, upewnij się, wszystkie biblioteki pakietami, które używasz (takie jak w przypadku stronicowania), jeśli wywołują wszystkie metody, że zapytania wysyłane do bazy danych programu Entity Framework użyć async.</span><span class="sxs-lookup"><span data-stu-id="13d26-335">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="13d26-336">Aby uzyskać więcej informacji na temat programowania asynchronicznego w .NET, zobacz [Przegląd Async](/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="13d26-336">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="13d26-337">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="13d26-337">Get the code</span></span>

[<span data-ttu-id="13d26-338">Pobieranie i wyświetlanie ukończonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="13d26-338">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a><span data-ttu-id="13d26-339">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="13d26-339">Next steps</span></span>

<span data-ttu-id="13d26-340">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="13d26-340">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="13d26-341">Utworzoną aplikację sieci web platformy ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="13d26-341">Created ASP.NET Core MVC web app</span></span>
> * <span data-ttu-id="13d26-342">Ustawianie stylów lokacji</span><span class="sxs-lookup"><span data-stu-id="13d26-342">Set up the site style</span></span>
> * <span data-ttu-id="13d26-343">Przedstawia informacje na temat pakietów programu EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="13d26-343">Learned about EF Core NuGet packages</span></span>
> * <span data-ttu-id="13d26-344">Utworzony model danych</span><span class="sxs-lookup"><span data-stu-id="13d26-344">Created the data model</span></span>
> * <span data-ttu-id="13d26-345">Utworzone kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="13d26-345">Created the database context</span></span>
> * <span data-ttu-id="13d26-346">Zarejestrowany SchoolContext</span><span class="sxs-lookup"><span data-stu-id="13d26-346">Registered the SchoolContext</span></span>
> * <span data-ttu-id="13d26-347">Zainicjowana klasa bazy danych przy użyciu danych testowych</span><span class="sxs-lookup"><span data-stu-id="13d26-347">Initialized DB with test data</span></span>
> * <span data-ttu-id="13d26-348">Utworzony kontroler i widoków</span><span class="sxs-lookup"><span data-stu-id="13d26-348">Created controller and views</span></span>
> * <span data-ttu-id="13d26-349">Wyświetlać bazy danych</span><span class="sxs-lookup"><span data-stu-id="13d26-349">Viewed the database</span></span>

<span data-ttu-id="13d26-350">W następującego samouczka, dowiesz się, jak do wykonywania podstawowych operacji CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) operacji.</span><span class="sxs-lookup"><span data-stu-id="13d26-350">In the following tutorial, you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

<span data-ttu-id="13d26-351">Przejdź do następnego samouczka, aby dowiedzieć się, jak i wykonywania podstawowych operacji CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) operacji.</span><span class="sxs-lookup"><span data-stu-id="13d26-351">Advance to the next tutorial to learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="13d26-352">Implementowanie podstawowych funkcji CRUD</span><span class="sxs-lookup"><span data-stu-id="13d26-352">Implement basic CRUD functionality</span></span>](crud.md)
