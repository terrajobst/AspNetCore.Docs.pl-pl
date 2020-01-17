---
title: 'Samouczek: wprowadzenie do EF Core w aplikacji sieci Web ASP.NET MVC'
description: Jest to pierwsza z szeregu samouczków, które wyjaśniają, jak skompilować przykładową aplikację firmy Contoso University od podstaw.
author: rick-anderson
ms.author: riande
ms.custom: mvc
ms.date: 02/06/2019
ms.topic: tutorial
uid: data/ef-mvc/intro
ms.openlocfilehash: 42ce63d1a31e033cf97536935b6dc94e015f30fa
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146384"
---
# <a name="tutorial-get-started-with-ef-core-in-an-aspnet-mvc-web-app"></a><span data-ttu-id="ed498-103">Samouczek: wprowadzenie do EF Core w aplikacji sieci Web ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="ed498-103">Tutorial: Get started with EF Core in an ASP.NET MVC web app</span></span>

<span data-ttu-id="ed498-104">Ten samouczek **nie** został zaktualizowany do ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="ed498-104">This tutorial has **not** been updated to ASP.NET Core 3.0.</span></span> <span data-ttu-id="ed498-105">Zaktualizowano [Razor Pages wersję](xref:data/ef-rp/intro) .</span><span class="sxs-lookup"><span data-stu-id="ed498-105">The [Razor Pages version](xref:data/ef-rp/intro) has been updated.</span></span> <span data-ttu-id="ed498-106">Aby uzyskać informacje na temat aktualizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/13920)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="ed498-106">For information on when this might be updated, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/13920).</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc.md)]

<span data-ttu-id="ed498-107">Przykładowa aplikacja internetowa Contoso University demonstruje sposób tworzenia aplikacji sieci Web ASP.NET Core 2,2 MVC przy użyciu Entity Framework (EF) Core 2,2 i Visual Studio 2017 lub 2019.</span><span class="sxs-lookup"><span data-stu-id="ed498-107">The Contoso University sample web application demonstrates how to create ASP.NET Core 2.2 MVC web applications using Entity Framework (EF) Core 2.2 and Visual Studio 2017 or 2019.</span></span>

<span data-ttu-id="ed498-108">Przykładowa aplikacja jest witryną internetową fikcyjnej firmy Contoso University.</span><span class="sxs-lookup"><span data-stu-id="ed498-108">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="ed498-109">Obejmuje funkcje, takie jak czasowej dla uczniów, tworzenia kurs i przypisania instruktora.</span><span class="sxs-lookup"><span data-stu-id="ed498-109">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="ed498-110">Jest to pierwsza z szeregu samouczków, które wyjaśniają, jak skompilować przykładową aplikację firmy Contoso University od podstaw.</span><span class="sxs-lookup"><span data-stu-id="ed498-110">This is the first in a series of tutorials that explain how to build the Contoso University sample application from scratch.</span></span>

<span data-ttu-id="ed498-111">W tym samouczku zostały wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="ed498-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ed498-112">Tworzenie aplikacji sieci Web ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="ed498-112">Create an ASP.NET Core MVC web app</span></span>
> * <span data-ttu-id="ed498-113">Ustawianie stylów lokacji</span><span class="sxs-lookup"><span data-stu-id="ed498-113">Set up the site style</span></span>
> * <span data-ttu-id="ed498-114">Dowiedz się więcej o EF Core pakietach NuGet</span><span class="sxs-lookup"><span data-stu-id="ed498-114">Learn about EF Core NuGet packages</span></span>
> * <span data-ttu-id="ed498-115">Tworzenie modelu danych</span><span class="sxs-lookup"><span data-stu-id="ed498-115">Create the data model</span></span>
> * <span data-ttu-id="ed498-116">Tworzenie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="ed498-116">Create the database context</span></span>
> * <span data-ttu-id="ed498-117">Rejestrowanie kontekstu dla iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="ed498-117">Register the context for dependency injection</span></span>
> * <span data-ttu-id="ed498-118">Zainicjuj bazę danych z danymi testowymi</span><span class="sxs-lookup"><span data-stu-id="ed498-118">Initialize the database with test data</span></span>
> * <span data-ttu-id="ed498-119">Tworzenie kontrolera i widoków</span><span class="sxs-lookup"><span data-stu-id="ed498-119">Create a controller and views</span></span>
> * <span data-ttu-id="ed498-120">Wyświetlanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="ed498-120">View the database</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ed498-121">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ed498-121">Prerequisites</span></span>

* [<span data-ttu-id="ed498-122">.NET Core SDK 2.2</span><span class="sxs-lookup"><span data-stu-id="ed498-122">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download)
* <span data-ttu-id="ed498-123">[Program Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z następującymi obciążeniami:</span><span class="sxs-lookup"><span data-stu-id="ed498-123">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the following workloads:</span></span>
  * <span data-ttu-id="ed498-124">**ASP.NET i programowanie aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="ed498-124">**ASP.NET and web development** workload</span></span>
  * <span data-ttu-id="ed498-125">**Tworzenie aplikacji dla wielu platform w środowisku .NET Core**</span><span class="sxs-lookup"><span data-stu-id="ed498-125">**.NET Core cross-platform development** workload</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ed498-126">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="ed498-126">Troubleshooting</span></span>

<span data-ttu-id="ed498-127">Jeśli napotkasz problem, nie można rozpoznać ogólnie można znaleźć rozwiązania, porównując swój kod, aby [projektu ukończona](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="ed498-127">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span></span> <span data-ttu-id="ed498-128">Aby zapoznać się z listą typowych błędów i sposobu ich rozwiązywania, zobacz [sekcję Rozwiązywanie problemów w ostatnim samouczku w serii](advanced.md#common-errors).</span><span class="sxs-lookup"><span data-stu-id="ed498-128">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="ed498-129">Jeśli nie możesz znaleźć tego, czego potrzebujesz, możesz ogłosić pytanie do StackOverflow.com dla [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="ed498-129">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="ed498-130">Jest to seria 10 samouczków, z których każdy jest oparty na tym, co zostało zrobione we wcześniejszych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="ed498-130">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="ed498-131">Rozważ zapisanie kopii projektu po każdym pomyślnym zakończeniu samouczka.</span><span class="sxs-lookup"><span data-stu-id="ed498-131">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="ed498-132">Jeśli wystąpią problemy, możesz zacząć od poprzedniego samouczka zamiast wrócić do początku całej serii.</span><span class="sxs-lookup"><span data-stu-id="ed498-132">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="contoso-university-web-app"></a><span data-ttu-id="ed498-133">Aplikacja sieci Web firmy Contoso University</span><span class="sxs-lookup"><span data-stu-id="ed498-133">Contoso University web app</span></span>

<span data-ttu-id="ed498-134">Aplikacja, którą tworzysz w tych samouczkach, jest prostą witryną sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ed498-134">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="ed498-135">Użytkownicy mogą przeglądać i aktualizacji dla uczniów, kursu i informacji przez instruktorów.</span><span class="sxs-lookup"><span data-stu-id="ed498-135">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="ed498-136">Poniżej przedstawiono kilka ekranów, które zostaną utworzone.</span><span class="sxs-lookup"><span data-stu-id="ed498-136">Here are a few of the screens you'll create.</span></span>

![Strona indeksu uczniów](intro/_static/students-index.png)

![Strona edytowania uczniów](intro/_static/student-edit.png)

## <a name="create-web-app"></a><span data-ttu-id="ed498-139">Tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="ed498-139">Create web app</span></span>

* <span data-ttu-id="ed498-140">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ed498-140">Open Visual Studio.</span></span>

* <span data-ttu-id="ed498-141">Z menu **plik** wybierz pozycję **Nowy projekt >** .</span><span class="sxs-lookup"><span data-stu-id="ed498-141">From the **File** menu, select **New > Project**.</span></span>

* <span data-ttu-id="ed498-142">W okienku po lewej stronie wybierz pozycję **zainstalowane > C# Visual > Web**.</span><span class="sxs-lookup"><span data-stu-id="ed498-142">From the left pane, select **Installed > Visual C# > Web**.</span></span>

* <span data-ttu-id="ed498-143">Wybierz szablon projektu **aplikacji sieci Web ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="ed498-143">Select the **ASP.NET Core Web Application** project template.</span></span>

* <span data-ttu-id="ed498-144">Wprowadź **ContosoUniversity** jako nazwę, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed498-144">Enter **ContosoUniversity** as the name and click **OK**.</span></span>

  ![Okno dialogowe nowego projektu](intro/_static/new-project2.png)

* <span data-ttu-id="ed498-146">Zaczekaj, aż pojawi się okno dialogowe **Nowa aplikacja sieci Web ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="ed498-146">Wait for the **New ASP.NET Core Web Application** dialog to appear.</span></span>

* <span data-ttu-id="ed498-147">Wybierz pozycję **.NET Core**, **ASP.NET Core 2,2** i szablon **aplikacji sieci Web (Model-View-Controller)** .</span><span class="sxs-lookup"><span data-stu-id="ed498-147">Select **.NET Core**, **ASP.NET Core 2.2** and the **Web Application (Model-View-Controller)** template.</span></span>

* <span data-ttu-id="ed498-148">Upewnij się, że **uwierzytelnianie** jest ustawione na wartość **bez uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="ed498-148">Make sure **Authentication** is set to **No Authentication**.</span></span>

* <span data-ttu-id="ed498-149">Wybierz przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed498-149">Select **OK**</span></span>

  ![Nowe okno dialogowe projektu ASP.NET Core](intro/_static/new-aspnet2.png)

## <a name="set-up-the-site-style"></a><span data-ttu-id="ed498-151">Ustawianie stylów lokacji</span><span class="sxs-lookup"><span data-stu-id="ed498-151">Set up the site style</span></span>

<span data-ttu-id="ed498-152">Kilka prostych zmian spowoduje skonfigurowanie menu witryny, układu i strony głównej.</span><span class="sxs-lookup"><span data-stu-id="ed498-152">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="ed498-153">Otwórz *Widok widoki/Shared/_Layout. cshtml* i wprowadź następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="ed498-153">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="ed498-154">Należy zmienić każde wystąpienie "ContosoUniversity" na "Uniwersytet firmy Contoso".</span><span class="sxs-lookup"><span data-stu-id="ed498-154">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="ed498-155">Istnieją trzy wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ed498-155">There are three occurrences.</span></span>

* <span data-ttu-id="ed498-156">Dodaj pozycje menu dla **informacji o**programie, **studentów**, **kursy**, **Instruktorzy**i **działy**, a następnie usuń wpis menu **prywatność** .</span><span class="sxs-lookup"><span data-stu-id="ed498-156">Add menu entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Privacy** menu entry.</span></span>

<span data-ttu-id="ed498-157">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="ed498-157">The changes are highlighted.</span></span>

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,34-48,63)]

<span data-ttu-id="ed498-158">W obszarze *widoki/Home/index. cshtml*Zastąp zawartość pliku następującym kodem, aby zamienić tekst na ASP.NET i MVC z tekstem dotyczącym tej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="ed498-158">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

<span data-ttu-id="ed498-159">Naciśnij klawisze CTRL + F5, aby uruchomić projekt, lub wybierz polecenie **debuguj > Uruchom bez debugowania** z menu.</span><span class="sxs-lookup"><span data-stu-id="ed498-159">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="ed498-160">Zostanie wyświetlona strona główna z kartami dla stron, które zostaną utworzone w tych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="ed498-160">You see the home page with tabs for the pages you'll create in these tutorials.</span></span>

![Strona główna firmy Contoso University](intro/_static/home-page.png)

## <a name="about-ef-core-nuget-packages"></a><span data-ttu-id="ed498-162">Informacje o pakietach NuGet EF Core</span><span class="sxs-lookup"><span data-stu-id="ed498-162">About EF Core NuGet packages</span></span>

<span data-ttu-id="ed498-163">Aby dodać obsługę EF Core do projektu, zainstaluj dostawcę bazy danych, który ma być celem.</span><span class="sxs-lookup"><span data-stu-id="ed498-163">To add EF Core support to a project, install the database provider that you want to target.</span></span> <span data-ttu-id="ed498-164">Ten samouczek używa SQL Server, a pakiet dostawcy to [Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="ed498-164">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span> <span data-ttu-id="ed498-165">Ten pakiet jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), dlatego nie musisz się odwoływać do pakietu.</span><span class="sxs-lookup"><span data-stu-id="ed498-165">This package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), so you don't need to reference the package.</span></span>

<span data-ttu-id="ed498-166">Pakiet EF SQL Server i jego zależności (`Microsoft.EntityFrameworkCore` i `Microsoft.EntityFrameworkCore.Relational`) zapewniają obsługę środowiska uruchomieniowego dla EF.</span><span class="sxs-lookup"><span data-stu-id="ed498-166">The EF SQL Server package and its dependencies (`Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`) provide runtime support for EF.</span></span> <span data-ttu-id="ed498-167">Pakiet narzędzi zostanie dodany później, w samouczku [migracji](migrations.md) .</span><span class="sxs-lookup"><span data-stu-id="ed498-167">You'll add a tooling package later, in the [Migrations](migrations.md) tutorial.</span></span>

<span data-ttu-id="ed498-168">Aby uzyskać informacje o innych dostawcach baz danych, które są dostępne dla Entity Framework Core, zobacz [dostawcy bazy danych](/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="ed498-168">For information about other database providers that are available for Entity Framework Core, see [Database providers](/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="ed498-169">Tworzenie modelu danych</span><span class="sxs-lookup"><span data-stu-id="ed498-169">Create the data model</span></span>

<span data-ttu-id="ed498-170">Następnie utworzysz klasy jednostek dla aplikacji firmy Contoso University.</span><span class="sxs-lookup"><span data-stu-id="ed498-170">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="ed498-171">Zaczniesz od następujących trzech jednostek.</span><span class="sxs-lookup"><span data-stu-id="ed498-171">You'll start with the following three entities.</span></span>

![Diagram modelu danych kurs — rejestracja-ucznia](intro/_static/data-model-diagram.png)

<span data-ttu-id="ed498-173">Istnieje relacja jeden do wielu między jednostkami `Student` i `Enrollment` i istnieje relacja jeden do wielu między `Course` i `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="ed498-173">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="ed498-174">Innymi słowy, student może być zarejestrowany w dowolnej liczbie kursów, a kurs może mieć dowolną liczbę uczniów zarejestrowanych w nim.</span><span class="sxs-lookup"><span data-stu-id="ed498-174">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="ed498-175">W poniższych sekcjach utworzysz klasę dla każdej z tych jednostek.</span><span class="sxs-lookup"><span data-stu-id="ed498-175">In the following sections you'll create a class for each one of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="ed498-176">Jednostki dla uczniów</span><span class="sxs-lookup"><span data-stu-id="ed498-176">The Student entity</span></span>

![Diagram jednostek dla uczniów](intro/_static/student-entity.png)

<span data-ttu-id="ed498-178">W folderze *modele* Utwórz plik klasy o nazwie *student.cs* i Zastąp kod szablonu poniższym kodem.</span><span class="sxs-lookup"><span data-stu-id="ed498-178">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="ed498-179">Właściwość `ID` stanie się kolumną klucza podstawowego tabeli bazy danych, która odnosi się do tej klasy.</span><span class="sxs-lookup"><span data-stu-id="ed498-179">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="ed498-180">Domyślnie platforma Entity Framework interpretuje właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="ed498-180">By default, the Entity Framework interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="ed498-181">`Enrollments` Właściwość [właściwość nawigacji](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="ed498-181">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="ed498-182">Właściwości nawigacji zawierają inne jednostki, które są powiązane z tą jednostką.</span><span class="sxs-lookup"><span data-stu-id="ed498-182">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="ed498-183">W takim przypadku Właściwość `Enrollments` `Student entity` będzie zawierać wszystkie jednostki `Enrollment`, które są powiązane z tą jednostką `Student`.</span><span class="sxs-lookup"><span data-stu-id="ed498-183">In this case, the `Enrollments` property of a `Student entity` will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="ed498-184">Innymi słowy, jeśli dany wiersz ucznia w bazie danych ma dwa powiązane wiersze rejestracji (wiersze, które zawierają wartość klucza podstawowego tego ucznia w kolumnie klucza obcego StudentID), `Enrollments` właściwość nawigacji tej jednostki `Student` będzie zawierać te dwie jednostki `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="ed498-184">In other words, if a given Student row in the database has two related Enrollment rows (rows that contain that student's primary key value in their StudentID foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="ed498-185">Jeśli właściwość nawigacji może zawierać wiele jednostek (tak jak w przypadku relacji "wiele do wielu" lub "jeden do wielu"), jej typem musi być lista, w której można dodawać, usuwać i aktualizować wpisy, takie jak `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="ed498-185">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection<T>`.</span></span> <span data-ttu-id="ed498-186">Można określić `ICollection<T>` lub typ, taki jak `List<T>` lub `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="ed498-186">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="ed498-187">Jeśli określisz `ICollection<T>`, EF domyślnie tworzy kolekcję `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="ed498-187">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="ed498-188">Jednostki rejestracji</span><span class="sxs-lookup"><span data-stu-id="ed498-188">The Enrollment entity</span></span>

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

<span data-ttu-id="ed498-190">W folderze *modele* Utwórz *Enrollment.cs* i Zastąp istniejący kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ed498-190">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="ed498-191">Właściwość `EnrollmentID` będzie kluczem podstawowym; Ta jednostka używa wzorca `classnameID`, a nie `ID`, jak pokazano w jednostce `Student`.</span><span class="sxs-lookup"><span data-stu-id="ed498-191">The `EnrollmentID` property will be the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="ed498-192">Zwykle należy wybrać jeden wzorzec i używać go w całym modelu danych.</span><span class="sxs-lookup"><span data-stu-id="ed498-192">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="ed498-193">Tutaj, odmiana ilustruje, że można użyć dowolnego wzorca.</span><span class="sxs-lookup"><span data-stu-id="ed498-193">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="ed498-194">W [późniejszym samouczku](inheritance.md)zobaczysz, jak używać identyfikatora bez ClassName, ułatwia implementowanie dziedziczenia w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="ed498-194">In a [later tutorial](inheritance.md), you'll see how using ID without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="ed498-195">`Grade` Właściwość `enum`.</span><span class="sxs-lookup"><span data-stu-id="ed498-195">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="ed498-196">Znak zapytania po `Grade` deklaracji typu wskazuje, że `Grade` właściwość ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="ed498-196">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="ed498-197">Ma wartość null, która różni się od zera klasy korporacyjnej — wartość null oznacza, że wartość nie jest znana lub nie została jeszcze przypisana.</span><span class="sxs-lookup"><span data-stu-id="ed498-197">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="ed498-198">`StudentID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Student`.</span><span class="sxs-lookup"><span data-stu-id="ed498-198">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="ed498-199">Jednostka `Enrollment` jest skojarzona z jedną jednostką `Student`, więc właściwość może zawierać tylko jedną jednostkę `Student` (w przeciwieństwie do `Student.Enrollments`j właściwości nawigacji, która może być przechowywana w wielu jednostkach `Enrollment`).</span><span class="sxs-lookup"><span data-stu-id="ed498-199">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="ed498-200">`CourseID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Course`.</span><span class="sxs-lookup"><span data-stu-id="ed498-200">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="ed498-201">`Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.</span><span class="sxs-lookup"><span data-stu-id="ed498-201">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="ed498-202">Entity Framework interpretuje właściwość jako właściwość klucza obcego, jeśli ma nazwę `<navigation property name><primary key property name>` (na przykład `StudentID` dla właściwości nawigacji `Student`, ponieważ klucz podstawowy jednostki `Student` to `ID`).</span><span class="sxs-lookup"><span data-stu-id="ed498-202">Entity Framework interprets a property as a foreign key property if it's named `<navigation property name><primary key property name>` (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="ed498-203">Właściwości klucza obcego można również nazwać po prostu `<primary key property name>` (na przykład `CourseID`, ponieważ klucz podstawowy jednostki `Course` jest `CourseID`).</span><span class="sxs-lookup"><span data-stu-id="ed498-203">Foreign key properties can also be named simply `<primary key property name>` (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="ed498-204">Jednostki kursu</span><span class="sxs-lookup"><span data-stu-id="ed498-204">The Course entity</span></span>

![Diagram jednostek kursu](intro/_static/course-entity.png)

<span data-ttu-id="ed498-206">W folderze *modele* Utwórz *Course.cs* i Zastąp istniejący kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ed498-206">In the *Models* folder, create *Course.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="ed498-207">`Enrollments` Właściwość jest właściwością nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ed498-207">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="ed498-208">A `Course` jednostki mogą być one związane z dowolną liczbę `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="ed498-208">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="ed498-209">Dowiesz się więcej o atrybucie `DatabaseGenerated` w [późniejszym samouczku](complex-data-model.md) w tej serii.</span><span class="sxs-lookup"><span data-stu-id="ed498-209">We'll say more about the `DatabaseGenerated` attribute in a [later tutorial](complex-data-model.md) in this series.</span></span> <span data-ttu-id="ed498-210">Zasadniczo ten atrybut umożliwia wprowadzenie klucza podstawowego dla kursu, a nie jego wygenerowanie.</span><span class="sxs-lookup"><span data-stu-id="ed498-210">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="ed498-211">Tworzenie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="ed498-211">Create the database context</span></span>

<span data-ttu-id="ed498-212">Klasa główna, która koordynuje funkcje Entity Framework dla danego modelu danych, jest klasą kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed498-212">The main class that coordinates Entity Framework functionality for a given data model is the database context class.</span></span> <span data-ttu-id="ed498-213">Tę klasę można utworzyć, wyprowadzając ją z klasy `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="ed498-213">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="ed498-214">W kodzie możesz określić, które jednostki zostaną uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="ed498-214">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="ed498-215">Można również dostosować pewne zachowanie Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ed498-215">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="ed498-216">W tym projekcie nosi nazwę klasy `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="ed498-216">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="ed498-217">W folderze projektu Utwórz folder o nazwie *dane*.</span><span class="sxs-lookup"><span data-stu-id="ed498-217">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="ed498-218">W folderze *dane* Utwórz nowy plik klasy o nazwie *SchoolContext.cs*i Zastąp kod szablonu następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="ed498-218">In the *Data* folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="ed498-219">Ten kod tworzy właściwość `DbSet` dla każdego zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="ed498-219">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="ed498-220">W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych, a jednostka odpowiada wierszowi w tabeli.</span><span class="sxs-lookup"><span data-stu-id="ed498-220">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<span data-ttu-id="ed498-221">Można pominąć instrukcje `DbSet<Enrollment>` i `DbSet<Course>` i będą one działały tak samo.</span><span class="sxs-lookup"><span data-stu-id="ed498-221">You could've omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="ed498-222">Entity Framework będzie zawierać je niejawnie, ponieważ jednostka `Student` odwołuje się do jednostki `Enrollment`, a jednostka `Enrollment` odwołuje się do jednostki `Course`.</span><span class="sxs-lookup"><span data-stu-id="ed498-222">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="ed498-223">Po utworzeniu bazy danych EF tworzy tabele, które mają nazwy takie same jak nazwy właściwości `DbSet`.</span><span class="sxs-lookup"><span data-stu-id="ed498-223">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="ed498-224">Nazwy właściwości dla kolekcji są zwykle plural (studenci zamiast uczniów), ale deweloperzy zgadzają się na to, czy nazwy tabel powinny być wyrzucane.</span><span class="sxs-lookup"><span data-stu-id="ed498-224">Property names for collections are typically plural (Students rather than Student), but developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="ed498-225">Te samouczki zastąpią domyślne zachowanie, określając pojedyncze nazwy tabel w kontekście DbContext.</span><span class="sxs-lookup"><span data-stu-id="ed498-225">For these tutorials you'll override the default behavior by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="ed498-226">Aby to zrobić, Dodaj następujący wyróżniony kod po ostatniej właściwości Nieogólnymi.</span><span class="sxs-lookup"><span data-stu-id="ed498-226">To do that, add the following highlighted code after the last DbSet property.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-schoolcontext"></a><span data-ttu-id="ed498-227">Zarejestruj SchoolContext</span><span class="sxs-lookup"><span data-stu-id="ed498-227">Register the SchoolContext</span></span>

<span data-ttu-id="ed498-228">ASP.NET Core domyślnie implementuje [iniekcję zależności](../../fundamentals/dependency-injection.md) .</span><span class="sxs-lookup"><span data-stu-id="ed498-228">ASP.NET Core implements [dependency injection](../../fundamentals/dependency-injection.md) by default.</span></span> <span data-ttu-id="ed498-229">Usługi (takie jak kontekst bazy danych EF) są rejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ed498-229">Services (such as the EF database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="ed498-230">Składniki, które wymagają tych usług (takich jak kontrolery MVC), są dostarczane przez parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="ed498-230">Components that require these services (such as MVC controllers) are provided these services via constructor parameters.</span></span> <span data-ttu-id="ed498-231">Zobaczysz kod konstruktora kontrolera, który pobiera wystąpienie kontekstu w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="ed498-231">You'll see the controller constructor code that gets a context instance later in this tutorial.</span></span>

<span data-ttu-id="ed498-232">Aby zarejestrować `SchoolContext` jako usługę, Otwórz *Startup.cs*i Dodaj wyróżnione wiersze do metody `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ed498-232">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=9-10)]

<span data-ttu-id="ed498-233">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ed498-233">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="ed498-234">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="ed498-234">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="ed498-235">Dodaj `using` instrukcje dla przestrzeni nazw `ContosoUniversity.Data` i `Microsoft.EntityFrameworkCore`, a następnie Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="ed498-235">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces, and then build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="ed498-236">Otwórz plik *appSettings. JSON* i Dodaj parametry połączenia, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="ed498-236">Open the *appsettings.json* file and add a connection string as shown in the following example.</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a><span data-ttu-id="ed498-237">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="ed498-237">SQL Server Express LocalDB</span></span>

<span data-ttu-id="ed498-238">Parametry połączenia określają SQL Server bazy danych LocalDB.</span><span class="sxs-lookup"><span data-stu-id="ed498-238">The connection string specifies a SQL Server LocalDB database.</span></span> <span data-ttu-id="ed498-239">LocalDB to uproszczona wersja aparatu bazy danych SQL Server Express i jest przeznaczona do tworzenia aplikacji, a nie do użycia w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="ed498-239">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for application development, not production use.</span></span> <span data-ttu-id="ed498-240">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="ed498-240">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="ed498-241">Domyślnie LocalDB tworzy pliki bazy danych *MDF* w katalogu `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="ed498-241">By default, LocalDB creates *.mdf* database files in the `C:/Users/<user>` directory.</span></span>

## <a name="initialize-db-with-test-data"></a><span data-ttu-id="ed498-242">Zainicjuj bazę danych z danymi testowymi</span><span class="sxs-lookup"><span data-stu-id="ed498-242">Initialize DB with test data</span></span>

<span data-ttu-id="ed498-243">Entity Framework utworzy pustą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="ed498-243">The Entity Framework will create an empty database for you.</span></span> <span data-ttu-id="ed498-244">W tej sekcji napiszesz metodę, która jest wywoływana po utworzeniu bazy danych w celu wypełnienia jej danymi testowymi.</span><span class="sxs-lookup"><span data-stu-id="ed498-244">In this section, you write a method that's called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="ed498-245">W tym miejscu będziesz używać metody `EnsureCreated`, aby automatycznie utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="ed498-245">Here you'll use the `EnsureCreated` method to automatically create the database.</span></span> <span data-ttu-id="ed498-246">W [późniejszym samouczku](migrations.md) zobaczysz, jak obsłużyć zmiany modelu przy użyciu migracje Code First, aby zmienić schemat bazy danych zamiast upuszczania i ponownego tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed498-246">In a [later tutorial](migrations.md) you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="ed498-247">W folderze *dane* Utwórz nowy plik klasy o nazwie *DbInitializer.cs* i Zastąp kod szablonu następującym kodem, co spowoduje utworzenie bazy danych w razie potrzeby i załadowanie danych testowych do nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed498-247">In the *Data* folder, create a new class file named *DbInitializer.cs* and replace the template code with the following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="ed498-248">Kod sprawdza, czy w bazie danych znajdują się uczniowie i czy nie, zakłada, że baza danych jest nowa i należy ją umieścić w danych testowych.</span><span class="sxs-lookup"><span data-stu-id="ed498-248">The code checks if there are any students in the database, and if not, it assumes the database is new and needs to be seeded with test data.</span></span> <span data-ttu-id="ed498-249">Ładuje dane testowe do tablic zamiast `List<T>` kolekcje w celu zoptymalizowania wydajności.</span><span class="sxs-lookup"><span data-stu-id="ed498-249">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="ed498-250">W *program.cs*zmień metodę `Main`, aby wykonać następujące czynności podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="ed498-250">In *Program.cs*, modify the `Main` method to do the following on application startup:</span></span>

* <span data-ttu-id="ed498-251">Pobierz wystąpienie kontekstu bazy danych z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="ed498-251">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="ed498-252">Wywołaj metodę inicjatora, przekazując ją do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="ed498-252">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="ed498-253">Usuń kontekst, gdy metoda inicjatora jest gotowa.</span><span class="sxs-lookup"><span data-stu-id="ed498-253">Dispose the context when the seed method is done.</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

<span data-ttu-id="ed498-254">Dodaj instrukcje `using`:</span><span class="sxs-lookup"><span data-stu-id="ed498-254">Add `using` statements:</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

<span data-ttu-id="ed498-255">W starszych samouczkach można zobaczyć podobny kod w metodzie `Configure` w *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="ed498-255">In older tutorials, you may see similar code in the `Configure` method in *Startup.cs*.</span></span> <span data-ttu-id="ed498-256">Zalecamy użycie metody `Configure` tylko w celu skonfigurowania potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="ed498-256">We recommend that you use the `Configure` method only to set up the request pipeline.</span></span> <span data-ttu-id="ed498-257">Kod uruchamiania aplikacji należy do metody `Main`.</span><span class="sxs-lookup"><span data-stu-id="ed498-257">Application startup code belongs in the `Main` method.</span></span>

<span data-ttu-id="ed498-258">Teraz podczas pierwszego uruchomienia aplikacji baza danych zostanie utworzona i zostanie zainicjowana przy użyciu danych testowych.</span><span class="sxs-lookup"><span data-stu-id="ed498-258">Now the first time you run the application, the database will be created and seeded with test data.</span></span> <span data-ttu-id="ed498-259">Za każdym razem, gdy zmieniasz model danych, możesz usunąć bazę danych, zaktualizować metodę inicjatora i zacząć od nowa baza danych w taki sam sposób.</span><span class="sxs-lookup"><span data-stu-id="ed498-259">Whenever you change your data model, you can delete the database, update your seed method, and start afresh with a new database the same way.</span></span> <span data-ttu-id="ed498-260">W kolejnych samouczkach zobaczysz, jak zmodyfikować bazę danych, gdy zmieni się model danych, bez usuwania i ponownego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="ed498-260">In later tutorials, you'll see how to modify the database when the data model changes, without deleting and re-creating it.</span></span>

## <a name="create-controller-and-views"></a><span data-ttu-id="ed498-261">Tworzenie kontrolera i widoków</span><span class="sxs-lookup"><span data-stu-id="ed498-261">Create controller and views</span></span>

<span data-ttu-id="ed498-262">Następnie użyjesz aparatu tworzenia szkieletów w programie Visual Studio, aby dodać kontroler MVC i widoki, które będą używać EF do wykonywania zapytań i zapisywania danych.</span><span class="sxs-lookup"><span data-stu-id="ed498-262">Next, you'll use the scaffolding engine in Visual Studio to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="ed498-263">Automatyczne tworzenie metod i widoków akcji CRUD jest znane jako rusztowania.</span><span class="sxs-lookup"><span data-stu-id="ed498-263">The automatic creation of CRUD action methods and views is known as scaffolding.</span></span> <span data-ttu-id="ed498-264">Tworzenie szkieletu różni się od generowania kodu, ponieważ kod szkieletowy jest punktem wyjścia, który można zmodyfikować zgodnie z własnymi wymaganiami, ale zazwyczaj nie jest modyfikowany kod wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="ed498-264">Scaffolding differs from code generation in that the scaffolded code is a starting point that you can modify to suit your own requirements, whereas you typically don't modify generated code.</span></span> <span data-ttu-id="ed498-265">Jeśli musisz dostosować wygenerowany kod, użyj klas częściowych lub ponownie Wygeneruj kod, gdy zmienią się.</span><span class="sxs-lookup"><span data-stu-id="ed498-265">When you need to customize generated code, you use partial classes or you regenerate the code when things change.</span></span>

* <span data-ttu-id="ed498-266">Kliknij prawym przyciskiem myszy folder **controllers** w **Eksplorator rozwiązań** a następnie wybierz pozycję **Dodaj > nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="ed498-266">Right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

* <span data-ttu-id="ed498-267">W oknie dialogowym **Dodawanie szkieletu** :</span><span class="sxs-lookup"><span data-stu-id="ed498-267">In the **Add Scaffold** dialog box:</span></span>

  * <span data-ttu-id="ed498-268">Wybierz **kontroler MVC z widokami, używając Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="ed498-268">Select **MVC controller with views, using Entity Framework**.</span></span>

  * <span data-ttu-id="ed498-269">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="ed498-269">Click **Add**.</span></span> <span data-ttu-id="ed498-270">Zostanie wyświetlone okno dialogowe **Dodawanie kontrolera MVC z widokami, przy użyciu Entity Framework** .</span><span class="sxs-lookup"><span data-stu-id="ed498-270">The **Add MVC Controller with views, using Entity Framework** dialog box appears.</span></span>

    ![Student dla szkieletu](intro/_static/scaffold-student2.png)

  * <span data-ttu-id="ed498-272">W obszarze **Klasa modelu** wybierz **ucznia**.</span><span class="sxs-lookup"><span data-stu-id="ed498-272">In **Model class** select **Student**.</span></span>

  * <span data-ttu-id="ed498-273">W obszarze **Klasa kontekstu danych** wybierz pozycję **SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="ed498-273">In **Data context class** select **SchoolContext**.</span></span>

  * <span data-ttu-id="ed498-274">Zaakceptuj domyślną **StudentsController** jako nazwę.</span><span class="sxs-lookup"><span data-stu-id="ed498-274">Accept the default **StudentsController** as the name.</span></span>

  * <span data-ttu-id="ed498-275">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="ed498-275">Click **Add**.</span></span>

  <span data-ttu-id="ed498-276">Po kliknięciu przycisku **Dodaj**aparat szkieletu programu Visual Studio tworzy plik *StudentsController.cs* i zestaw widoków (pliki *. cshtml* ), które współpracują z kontrolerem.</span><span class="sxs-lookup"><span data-stu-id="ed498-276">When you click **Add**, the Visual Studio scaffolding engine creates a *StudentsController.cs* file and a set of views (*.cshtml* files) that work with the controller.</span></span>

<span data-ttu-id="ed498-277">(Aparat tworzenia szkieletów może również utworzyć kontekst bazy danych, jeśli nie utworzysz go ręcznie, jak wcześniej w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="ed498-277">(The scaffolding engine can also create the database context for you if you don't create it manually first as you did earlier for this tutorial.</span></span> <span data-ttu-id="ed498-278">Aby określić nową klasę kontekstu w polu **Dodaj kontroler** , kliknij znak plus z prawej strony **klasy kontekstu danych**.</span><span class="sxs-lookup"><span data-stu-id="ed498-278">You can specify a new context class in the **Add Controller** box by clicking the plus sign to the right of **Data context class**.</span></span>  <span data-ttu-id="ed498-279">Program Visual Studio utworzy następnie klasę `DbContext`, a także kontroler i widoki.</span><span class="sxs-lookup"><span data-stu-id="ed498-279">Visual Studio will then create your `DbContext` class as well as the controller and views.)</span></span>

<span data-ttu-id="ed498-280">Zauważ, że kontroler przyjmuje `SchoolContext` jako parametr konstruktora.</span><span class="sxs-lookup"><span data-stu-id="ed498-280">You'll notice that the controller takes a `SchoolContext` as a constructor parameter.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

<span data-ttu-id="ed498-281">ASP.NET Core iniekcja zależności zajmuje się przekazywaniem wystąpienia `SchoolContext` do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ed498-281">ASP.NET Core dependency injection takes care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="ed498-282">Wcześniej skonfigurowano plik *Startup.cs* .</span><span class="sxs-lookup"><span data-stu-id="ed498-282">You configured that in the *Startup.cs* file earlier.</span></span>

<span data-ttu-id="ed498-283">Kontroler zawiera `Index`ą metodę akcji, która wyświetla wszystkich uczniów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ed498-283">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="ed498-284">Metoda pobiera listę studentów z zestawu jednostek studentów, odczytując Właściwość `Students` wystąpienia kontekstu bazy danych:</span><span class="sxs-lookup"><span data-stu-id="ed498-284">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

<span data-ttu-id="ed498-285">Dowiesz się więcej na temat asynchronicznych elementów programowania w tym kodzie w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="ed498-285">You'll learn about the asynchronous programming elements in this code later in the tutorial.</span></span>

<span data-ttu-id="ed498-286">Widok *widoki/uczniowie/index. cshtml* wyświetla tę listę w tabeli:</span><span class="sxs-lookup"><span data-stu-id="ed498-286">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="ed498-287">Naciśnij klawisze CTRL + F5, aby uruchomić projekt, lub wybierz polecenie **debuguj > Uruchom bez debugowania** z menu.</span><span class="sxs-lookup"><span data-stu-id="ed498-287">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="ed498-288">Kliknij kartę Students (studenci), aby wyświetlić dane testowe, które zostały wstawione przy użyciu metody `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="ed498-288">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="ed498-289">W zależności od tego, jak Zawężasz okno przeglądarki, zobaczysz link `Students` kartę w górnej części strony lub kliknij ikonę nawigacji w prawym górnym rogu, aby zobaczyć link.</span><span class="sxs-lookup"><span data-stu-id="ed498-289">Depending on how narrow your browser window is, you'll see the `Students` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![Strona główna firmy Contoso University Narrow](intro/_static/home-page-narrow.png)

![Strona indeksu uczniów](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="ed498-292">Wyświetlanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="ed498-292">View the database</span></span>

<span data-ttu-id="ed498-293">Po uruchomieniu aplikacji Metoda `DbInitializer.Initialize` wywołuje `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="ed498-293">When you started the application, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="ed498-294">EF wykryto, że nie istniała baza danych i dlatego została utworzona, a następnie pozostała część kodu metody `Initialize` była wypełniana bazą danych.</span><span class="sxs-lookup"><span data-stu-id="ed498-294">EF saw that there was no database and so it created one, then the remainder of the `Initialize` method code populated the database with data.</span></span> <span data-ttu-id="ed498-295">Aby wyświetlić bazę danych w programie Visual Studio, można użyć **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="ed498-295">You can use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span>

<span data-ttu-id="ed498-296">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="ed498-296">Close the browser.</span></span>

<span data-ttu-id="ed498-297">Jeśli okno SSOX nie jest jeszcze otwarte, wybierz je z menu **Widok** w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ed498-297">If the SSOX window isn't already open, select it from the **View** menu in Visual Studio.</span></span>

<span data-ttu-id="ed498-298">W SSOX kliknij pozycję **(LocalDB) \MSSQLLocalDB > bazy danych**, a następnie kliknij wpis dla nazwy bazy danych znajdującej się w parametrach połączenia w pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="ed498-298">In SSOX, click **(localdb)\MSSQLLocalDB > Databases**, and then click the entry for the database name that's in the connection string in your *appsettings.json* file.</span></span>

<span data-ttu-id="ed498-299">Rozwiń węzeł **tabele** , aby wyświetlić tabele w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ed498-299">Expand the **Tables** node to see the tables in your database.</span></span>

![Tabele w SSOX](intro/_static/ssox-tables.png)

<span data-ttu-id="ed498-301">Kliknij prawym przyciskiem myszy tabelę **uczniów** i kliknij polecenie **Wyświetl dane** , aby wyświetlić utworzone kolumny i wiersze, które zostały wstawione do tabeli.</span><span class="sxs-lookup"><span data-stu-id="ed498-301">Right-click the **Student** table and click **View Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

![Tabela uczniów w SSOX](intro/_static/ssox-student-table.png)

<span data-ttu-id="ed498-303">Pliki *. mdf* i *. ldf* znajdują się w folderze *C:\Users\\\<yourUserName >* .</span><span class="sxs-lookup"><span data-stu-id="ed498-303">The *.mdf* and *.ldf* database files are in the *C:\Users\\\<yourusername>* folder.</span></span>

<span data-ttu-id="ed498-304">Ponieważ wywołujesz `EnsureCreated` w metodzie inicjatora uruchamianej podczas uruchamiania aplikacji, możesz teraz wprowadzić zmianę klasy `Student`, usunąć bazę danych, ponownie uruchomić aplikację, a baza danych zostanie automatycznie utworzona ponownie w celu dopasowania do zmiany.</span><span class="sxs-lookup"><span data-stu-id="ed498-304">Because you're calling `EnsureCreated` in the initializer method that runs on app start, you could now make a change to the `Student` class, delete the database, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="ed498-305">Na przykład, jeśli dodasz Właściwość `EmailAddress` do klasy `Student`, zobaczysz nową kolumnę `EmailAddress` w nowo utworzonej tabeli.</span><span class="sxs-lookup"><span data-stu-id="ed498-305">For example, if you add an `EmailAddress` property to the `Student` class, you'll see a new `EmailAddress` column in the re-created table.</span></span>

## <a name="conventions"></a><span data-ttu-id="ed498-306">Konwencje</span><span class="sxs-lookup"><span data-stu-id="ed498-306">Conventions</span></span>

<span data-ttu-id="ed498-307">Ilość kodu, który miał zostać zapisany w celu Entity Framework być w stanie utworzyć kompletną bazę danych, jest minimalny ze względu na stosowanie Konwencji lub zaEntity Framework łożeń.</span><span class="sxs-lookup"><span data-stu-id="ed498-307">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of conventions, or assumptions that the Entity Framework makes.</span></span>

* <span data-ttu-id="ed498-308">Nazwy właściwości `DbSet` są używane jako nazwy tabel.</span><span class="sxs-lookup"><span data-stu-id="ed498-308">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="ed498-309">W przypadku jednostek, do których nie odwołuje się Właściwość `DbSet`, nazwy klas jednostek są używane jako nazwy tabel.</span><span class="sxs-lookup"><span data-stu-id="ed498-309">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="ed498-310">Nazwy właściwości jednostki są używane w nazwach kolumn.</span><span class="sxs-lookup"><span data-stu-id="ed498-310">Entity property names are used for column names.</span></span>

* <span data-ttu-id="ed498-311">Właściwości jednostki o nazwach ID lub classnameID są rozpoznawane jako właściwości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="ed498-311">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="ed498-312">Właściwość jest interpretowana jako właściwość klucza obcego, jeśli jest nazywana *\<nazwy właściwości nawigacji >\<nazwy właściwości klucza podstawowego >* (na przykład `StudentID` właściwości nawigacji `Student`, ponieważ klucz podstawowy jednostki `Student` to `ID`).</span><span class="sxs-lookup"><span data-stu-id="ed498-312">A property is interpreted as a foreign key property if it's named *\<navigation property name>\<primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="ed498-313">Właściwości klucza obcego można również nazwać po prostu *\<nazwy właściwości klucza podstawowego >* (na przykład `EnrollmentID`, ponieważ klucz podstawowy jednostki `Enrollment` to `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="ed498-313">Foreign key properties can also be named simply *\<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="ed498-314">Zachowanie konwencjonalne można zastąpić.</span><span class="sxs-lookup"><span data-stu-id="ed498-314">Conventional behavior can be overridden.</span></span> <span data-ttu-id="ed498-315">Na przykład można jawnie określić nazwy tabel, jak zostało to opisane wcześniej w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="ed498-315">For example, you can explicitly specify table names, as you saw earlier in this tutorial.</span></span> <span data-ttu-id="ed498-316">I można ustawić nazwy kolumn i ustawić dowolną właściwość jako klucz podstawowy lub klucz obcy, jak widać w [późniejszym samouczku](complex-data-model.md) w tej serii.</span><span class="sxs-lookup"><span data-stu-id="ed498-316">And you can set column names and set any property as primary key or foreign key, as you'll see in a [later tutorial](complex-data-model.md) in this series.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="ed498-317">Kod asynchroniczny</span><span class="sxs-lookup"><span data-stu-id="ed498-317">Asynchronous code</span></span>

<span data-ttu-id="ed498-318">Programowanie asynchroniczne jest to domyślny tryb dla platformy ASP.NET Core i programem EF Core.</span><span class="sxs-lookup"><span data-stu-id="ed498-318">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="ed498-319">Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach, w dużym obciążeniem wszystkie dostępne wątki mogło zostać użyte.</span><span class="sxs-lookup"><span data-stu-id="ed498-319">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="ed498-320">Jeśli tak się stanie, serwer nie może przetworzyć nowe żądania aż wątki są zwalniane.</span><span class="sxs-lookup"><span data-stu-id="ed498-320">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="ed498-321">Przy użyciu kodu synchronicznego wiele wątków może powiązane, gdy nie są faktycznie czynności wykonują wszelkie prace ponieważ oczekują one operacji We/Wy zakończyć.</span><span class="sxs-lookup"><span data-stu-id="ed498-321">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="ed498-322">Za pomocą kodu asynchronicznego podczas procesu jest oczekiwania na we/wy ukończyć, jego wątku jest zwalniana dla serwera na potrzeby przetwarzaniem innych żądań.</span><span class="sxs-lookup"><span data-stu-id="ed498-322">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="ed498-323">W rezultacie kod asynchroniczny umożliwia bardziej efektywne wykorzystanie zasobów serwera i serwera jest włączona, aby obsłużyć większy ruch, bez opóźnień.</span><span class="sxs-lookup"><span data-stu-id="ed498-323">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="ed498-324">Kod asynchroniczny wprowadza niewielką ilość narzutu w czasie wykonywania, ale w przypadku niskiego natężenia ruchu, gdy wydajność jest niewielka, w przypadku dużych sytuacji związanych z ruchem jest istotna poprawa wydajności.</span><span class="sxs-lookup"><span data-stu-id="ed498-324">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="ed498-325">W poniższym kodzie słowo kluczowe `async`, `Task<T>` wartość zwracana, słowo kluczowe `await` i Metoda `ToListAsync` sprawiają, że kod jest wykonywany asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="ed498-325">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="ed498-326">Słowo kluczowe `async` informuje kompilator, aby wygenerował wywołania zwrotne dla części treści metody i automatycznie utworzyć obiekt `Task<IActionResult>`, który jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="ed498-326">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that's returned.</span></span>

* <span data-ttu-id="ed498-327">Typ zwracany `Task<IActionResult>` reprezentuje bieżącą współpracę z wynikiem typu `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="ed498-327">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>

* <span data-ttu-id="ed498-328">`await` — Słowo kluczowe powoduje, że kompilator podzielić metodę na dwie części.</span><span class="sxs-lookup"><span data-stu-id="ed498-328">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="ed498-329">Pierwsza część kończy się za pomocą operacji, który jest uruchamiany asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="ed498-329">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="ed498-330">Druga część zostanie przełączone do metody wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.</span><span class="sxs-lookup"><span data-stu-id="ed498-330">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="ed498-331">`ToListAsync` jest to wersja asynchroniczna elementu `ToList` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="ed498-331">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="ed498-332">Niektóre kwestie, o których należy wiedzieć, gdy piszesz kod asynchroniczny, który używa Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="ed498-332">Some things to be aware of when you are writing asynchronous code that uses the Entity Framework:</span></span>

* <span data-ttu-id="ed498-333">Tylko instrukcje, które powodują, że zapytania lub polecenia wysyłane do bazy danych są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="ed498-333">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="ed498-334">Obejmuje to na przykład `ToListAsync`, `SingleOrDefaultAsync`i `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="ed498-334">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="ed498-335">Nie zawiera na przykład instrukcji, które po prostu zmieniają `IQueryable`, takie jak `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="ed498-335">It doesn't include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="ed498-336">Kontekst EF nie jest bezpieczny wątkowo: nie próbuj wykonać równolegle wielu operacji.</span><span class="sxs-lookup"><span data-stu-id="ed498-336">An EF context isn't thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="ed498-337">Gdy wywoływana jest metoda async EF, zawsze używaj słowa kluczowego `await`.</span><span class="sxs-lookup"><span data-stu-id="ed498-337">When you call any async EF method, always use the `await` keyword.</span></span>

* <span data-ttu-id="ed498-338">Jeśli chcesz wykorzystać zalety wydajności w kodzie asynchronicznym, upewnij się, że wszystkie używane pakiety biblioteki (na przykład na potrzeby stronicowania) używają również metody asynchronicznej, jeśli wywołują wszelkie Entity Framework metod, które powodują wysłanie zapytań do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed498-338">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="ed498-339">Aby uzyskać więcej informacji na temat programowania asynchronicznego w programie .NET, zobacz [asynchroniczne Omówienie](/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="ed498-339">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="ed498-340">Uzyskaj kod</span><span class="sxs-lookup"><span data-stu-id="ed498-340">Get the code</span></span>

[<span data-ttu-id="ed498-341">Pobierz lub Wyświetl ukończoną aplikację.</span><span class="sxs-lookup"><span data-stu-id="ed498-341">Download or view the completed application.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a><span data-ttu-id="ed498-342">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="ed498-342">Next steps</span></span>

<span data-ttu-id="ed498-343">W tym samouczku zostały wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="ed498-343">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ed498-344">Utworzono ASP.NET Core aplikacji sieci Web MVC</span><span class="sxs-lookup"><span data-stu-id="ed498-344">Created ASP.NET Core MVC web app</span></span>
> * <span data-ttu-id="ed498-345">Ustawianie stylów lokacji</span><span class="sxs-lookup"><span data-stu-id="ed498-345">Set up the site style</span></span>
> * <span data-ttu-id="ed498-346">Informacje o EF Core pakietach NuGet</span><span class="sxs-lookup"><span data-stu-id="ed498-346">Learned about EF Core NuGet packages</span></span>
> * <span data-ttu-id="ed498-347">Utworzono model danych</span><span class="sxs-lookup"><span data-stu-id="ed498-347">Created the data model</span></span>
> * <span data-ttu-id="ed498-348">Utworzono kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="ed498-348">Created the database context</span></span>
> * <span data-ttu-id="ed498-349">Zarejestrowano SchoolContext</span><span class="sxs-lookup"><span data-stu-id="ed498-349">Registered the SchoolContext</span></span>
> * <span data-ttu-id="ed498-350">Zainicjowano bazę danych z danymi testowymi</span><span class="sxs-lookup"><span data-stu-id="ed498-350">Initialized DB with test data</span></span>
> * <span data-ttu-id="ed498-351">Utworzono kontroler i widoki</span><span class="sxs-lookup"><span data-stu-id="ed498-351">Created controller and views</span></span>
> * <span data-ttu-id="ed498-352">Wyświetlanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="ed498-352">Viewed the database</span></span>

<span data-ttu-id="ed498-353">W poniższym samouczku dowiesz się, jak wykonywać operacje podstawowe CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie).</span><span class="sxs-lookup"><span data-stu-id="ed498-353">In the following tutorial, you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

<span data-ttu-id="ed498-354">Przejdź do następnego samouczka, aby dowiedzieć się, jak wykonywać podstawowe operacje CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie).</span><span class="sxs-lookup"><span data-stu-id="ed498-354">Advance to the next tutorial to learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ed498-355">Zaimplementuj podstawowe funkcje CRUD</span><span class="sxs-lookup"><span data-stu-id="ed498-355">Implement basic CRUD functionality</span></span>](crud.md)

