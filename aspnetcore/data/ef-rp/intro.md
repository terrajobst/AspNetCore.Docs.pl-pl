---
title: Strony razor za pomocą platformy Entity Framework Core w programie ASP.NET Core — samouczek 1 8
author: tdykstra
description: Pokazuje, jak utworzyć aplikację strony Razor za pomocą platformy Entity Framework Core
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 07/22/2019
uid: data/ef-rp/intro
ms.openlocfilehash: 107b348b4484301b86eeb5528833914fe4c1eaf7
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080926"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="7dc73-103">Strony razor za pomocą platformy Entity Framework Core w programie ASP.NET Core — samouczek 1 8</span><span class="sxs-lookup"><span data-stu-id="7dc73-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

<span data-ttu-id="7dc73-104">Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7dc73-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7dc73-105">Jest to pierwsza z serii samouczków, które pokazują, jak używać rdzenia Entity Framework (EF) w aplikacji ASP.NET Core Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7dc73-105">This is the first in a series of tutorials that show how to use Entity Framework (EF) Core in an ASP.NET Core Razor Pages app.</span></span> <span data-ttu-id="7dc73-106">Samouczki budują witrynę sieci Web dla fikcyjnej uczelni firmy Contoso.</span><span class="sxs-lookup"><span data-stu-id="7dc73-106">The tutorials build a web site for a fictional Contoso University.</span></span> <span data-ttu-id="7dc73-107">Lokacja obejmuje funkcje, takie jak przyjmowanie uczniów, tworzenie kursu i przydziały instruktora.</span><span class="sxs-lookup"><span data-stu-id="7dc73-107">The site includes functionality such as student admission, course creation, and instructor assignments.</span></span>

[<span data-ttu-id="7dc73-108">Pobieranie i wyświetlanie ukończonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-108">Download or view the completed app.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="7dc73-109">[Instrukcje pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="7dc73-109">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7dc73-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7dc73-110">Prerequisites</span></span>

* <span data-ttu-id="7dc73-111">Jeśli dopiero zaczynasz korzystać z Razor Pages, przejdź do serii samouczek wprowadzenie [do Razor Pages](xref:tutorials/razor-pages/razor-pages-start) , przed rozpoczęciem tej.</span><span class="sxs-lookup"><span data-stu-id="7dc73-111">If you're new to Razor Pages, go through the [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) tutorial series before starting this one.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7dc73-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dc73-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[VS prereqs](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7dc73-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7dc73-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[VS Code prereqs](~/includes/net-core-prereqs-vsc-3.0.md)]

---

## <a name="database-engines"></a><span data-ttu-id="7dc73-114">Aparaty bazy danych</span><span class="sxs-lookup"><span data-stu-id="7dc73-114">Database engines</span></span>

<span data-ttu-id="7dc73-115">Instrukcje programu Visual Studio używają [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), wersji SQL Server Express, która działa tylko w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="7dc73-115">The Visual Studio instructions use [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), a version of SQL Server Express that runs only on Windows.</span></span>

<span data-ttu-id="7dc73-116">Instrukcje Visual Studio Code korzystają z [oprogramowania SQLite](https://www.sqlite.org/), wieloplatformowego aparatu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-116">The Visual Studio Code instructions use [SQLite](https://www.sqlite.org/), a cross-platform database engine.</span></span>

<span data-ttu-id="7dc73-117">Jeśli zdecydujesz się na korzystanie z oprogramowania SQLite, Pobierz i zainstaluj narzędzie innej firmy służące do zarządzania bazą danych programu SQLite i wyświetlania jej, na przykład w [przeglądarce DB dla oprogramowania SQLite](https://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="7dc73-117">If you choose to use SQLite, download and install a third-party tool for managing and viewing a SQLite database, such as [DB Browser for SQLite](https://sqlitebrowser.org/).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7dc73-118">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="7dc73-118">Troubleshooting</span></span>

<span data-ttu-id="7dc73-119">Jeśli wystąpi problem, którego nie można rozwiązać, porównaj swój kod z [zakończonym projektem](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="7dc73-119">If you run into a problem you can't resolve, compare your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="7dc73-120">Dobrym sposobem uzyskania pomocy jest opublikowanie pytania do StackOverflow.com przy użyciu [tagu ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [tagu EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="7dc73-120">A good way to get help is by posting a question to StackOverflow.com, using the [ASP.NET Core tag](https://stackoverflow.com/questions/tagged/asp.net-core) or the [EF Core tag](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-sample-app"></a><span data-ttu-id="7dc73-121">Przykładowa aplikacja</span><span class="sxs-lookup"><span data-stu-id="7dc73-121">The sample app</span></span>

<span data-ttu-id="7dc73-122">Aplikacja wbudowana w tych samouczkach to podstawowe university witryna sieci web.</span><span class="sxs-lookup"><span data-stu-id="7dc73-122">The app built in these tutorials is a basic university web site.</span></span> <span data-ttu-id="7dc73-123">Użytkownicy mogą przeglądać i aktualizacji dla uczniów, kursu i informacji przez instruktorów.</span><span class="sxs-lookup"><span data-stu-id="7dc73-123">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="7dc73-124">Oto kilka ekranów, utworzone w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="7dc73-124">Here are a few of the screens created in the tutorial.</span></span>

![Strona indeksu uczniów](intro/_static/students-index30.png)

![Strona edytowania uczniów](intro/_static/student-edit30.png)

<span data-ttu-id="7dc73-127">Styl interfejsu użytkownika tej witryny jest oparty na wbudowanych szablonach projektów.</span><span class="sxs-lookup"><span data-stu-id="7dc73-127">The UI style of this site is based on the built-in project templates.</span></span> <span data-ttu-id="7dc73-128">Fokus samouczka dotyczy korzystania z EF Core, a nie dostosowywania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7dc73-128">The tutorial's focus is on how to use EF Core, not how to customize the UI.</span></span>

<span data-ttu-id="7dc73-129">Skorzystaj z linku w górnej części strony, aby uzyskać kod źródłowy dla ukończonego projektu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-129">Follow the link at the top of the page to get the source code for the completed project.</span></span> <span data-ttu-id="7dc73-130">Folder *cu30* ma kod dla ASP.NET Core wersji 3,0 samouczka.</span><span class="sxs-lookup"><span data-stu-id="7dc73-130">The *cu30* folder has the code for the ASP.NET Core 3.0 version of the tutorial.</span></span> <span data-ttu-id="7dc73-131">Pliki odzwierciedlające stan kodu dla samouczków 1-7 można znaleźć w folderze *cu30snapshots* .</span><span class="sxs-lookup"><span data-stu-id="7dc73-131">Files that reflect the state of the code for tutorials 1-7 can be found in the *cu30snapshots* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7dc73-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dc73-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7dc73-133">Aby uruchomić aplikację po pobraniu ukończonego projektu:</span><span class="sxs-lookup"><span data-stu-id="7dc73-133">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="7dc73-134">Usuń trzy pliki i jeden folder, w którym znajduje się nazwa *oprogramowania SQLite* .</span><span class="sxs-lookup"><span data-stu-id="7dc73-134">Delete three files and one folder that have *SQLite* in the name.</span></span>
* <span data-ttu-id="7dc73-135">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="7dc73-135">Build the project.</span></span>
* <span data-ttu-id="7dc73-136">W konsoli Menedżera pakietów (PMC) Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7dc73-136">In Package Manager Console (PMC) run the following command:</span></span>

  ```powershell
  Update-Database
  ```

* <span data-ttu-id="7dc73-137">Uruchom projekt, aby wypełniać bazę danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-137">Run the project to seed the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7dc73-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7dc73-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7dc73-139">Aby uruchomić aplikację po pobraniu ukończonego projektu:</span><span class="sxs-lookup"><span data-stu-id="7dc73-139">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="7dc73-140">Usuń *ContosoUniversity. csproj*i Zmień nazwę *ContosoUniversitySQLite. csproj* na *ContosoUniversity. csproj*.</span><span class="sxs-lookup"><span data-stu-id="7dc73-140">Delete *ContosoUniversity.csproj*, and rename *ContosoUniversitySQLite.csproj* to *ContosoUniversity.csproj*.</span></span>
* <span data-ttu-id="7dc73-141">Usuń *Startup.cs*i zmień nazwę *StartupSQLite.cs* na *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7dc73-141">Delete *Startup.cs*, and rename *StartupSQLite.cs* to *Startup.cs*.</span></span>
* <span data-ttu-id="7dc73-142">Usuń plik *appSettings. JSON*i Zmień nazwę *appSettingsSQLite. JSON* na *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="7dc73-142">Delete *appSettings.json*, and rename *appSettingsSQLite.json* to *appSettings.json*.</span></span>
* <span data-ttu-id="7dc73-143">Usuń folder *migracji* , a następnie zmień nazwę *MigrationsSQL* na *migracji*.</span><span class="sxs-lookup"><span data-stu-id="7dc73-143">Delete the *Migrations* folder, and rename *MigrationsSQL* to *Migrations*.</span></span>
* <span data-ttu-id="7dc73-144">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="7dc73-144">Build the project.</span></span>
* <span data-ttu-id="7dc73-145">W wierszu polecenia w folderze projektu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="7dc73-145">At a command prompt in the project folder, run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef --version 3.0.0-*
  dotnet ef database update
  ```

* <span data-ttu-id="7dc73-146">W narzędziu SQLite uruchom następującą instrukcję SQL:</span><span class="sxs-lookup"><span data-stu-id="7dc73-146">In your SQLite tool, run the following SQL statement:</span></span>

  ```sql
  UPDATE Department SET RowVersion = randomblob(8)
  ```

* <span data-ttu-id="7dc73-147">Uruchom projekt, aby wypełniać bazę danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-147">Run the project to seed the database.</span></span>

---

## <a name="create-the-web-app-project"></a><span data-ttu-id="7dc73-148">Tworzenie projektu aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="7dc73-148">Create the web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7dc73-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dc73-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7dc73-150">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="7dc73-150">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="7dc73-151">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="7dc73-151">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="7dc73-152">Nadaj projektowi nazwę *ContosoUniversity*.</span><span class="sxs-lookup"><span data-stu-id="7dc73-152">Name the project *ContosoUniversity*.</span></span> <span data-ttu-id="7dc73-153">Ważne jest, aby użyć tej dokładnej nazwy, łącznie z wielką literą, więc przestrzenie nazw są zgodne, gdy kod jest kopiowany i wklejany.</span><span class="sxs-lookup"><span data-stu-id="7dc73-153">It's important to use this exact name including capitalization, so the namespaces match when code is copied and pasted.</span></span>
* <span data-ttu-id="7dc73-154">Na liście rozwijanej wybierz pozycję **.NET Core** i **ASP.NET Core 3,0** , a następnie wybierz pozycję **aplikacja sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="7dc73-154">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdowns, and then select **Web Application**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7dc73-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7dc73-155">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7dc73-156">W terminalu przejdź do folderu, w którym ma zostać utworzony folder projektu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-156">In a terminal, navigate to the folder in which the project folder should be created.</span></span>

* <span data-ttu-id="7dc73-157">Uruchom następujące polecenia, aby utworzyć projekt Razor Pages i `cd` w nowym folderze projektu:</span><span class="sxs-lookup"><span data-stu-id="7dc73-157">Run the following commands to create a Razor Pages project and `cd` into the new project folder:</span></span>

  ```dotnetcli
  dotnet new webapp -o ContosoUniversity
  cd ContosoUniversity
  ```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="7dc73-158">Ustawianie stylów lokacji</span><span class="sxs-lookup"><span data-stu-id="7dc73-158">Set up the site style</span></span>

<span data-ttu-id="7dc73-159">Skonfiguruj nagłówek, stopkę i menu witryny przez aktualizację *stron/Shared/_Layout. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7dc73-159">Set up the site header, footer, and menu by updating *Pages/Shared/_Layout.cshtml*:</span></span>

* <span data-ttu-id="7dc73-160">Należy zmienić każde wystąpienie "ContosoUniversity" na "Uniwersytet firmy Contoso".</span><span class="sxs-lookup"><span data-stu-id="7dc73-160">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="7dc73-161">Istnieją trzy wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="7dc73-161">There are three occurrences.</span></span>

* <span data-ttu-id="7dc73-162">Usuń wpisy menu **Home** i **privacy** oraz Dodaj wpisy dotyczące **osób,** **studentów**, **kursów**, **instruktorów**i **działów**.</span><span class="sxs-lookup"><span data-stu-id="7dc73-162">Delete the **Home** and **Privacy** menu entries, and add entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**.</span></span>

<span data-ttu-id="7dc73-163">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="7dc73-163">The changes are highlighted.</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Shared/_Layout.cshtml?highlight=6,14,21-35,49)]

<span data-ttu-id="7dc73-164">W obszarze *Pages/index. cshtml*Zastąp zawartość pliku następującym kodem, aby zamienić tekst na ASP.NET Core z tekstem dotyczącym tej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7dc73-164">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET Core with text about this app:</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Index.cshtml)]

<span data-ttu-id="7dc73-165">Uruchom aplikację, aby sprawdzić, czy zostanie wyświetlona strona główna.</span><span class="sxs-lookup"><span data-stu-id="7dc73-165">Run the app to verify that the home page appears.</span></span>

## <a name="the-data-model"></a><span data-ttu-id="7dc73-166">Model danych</span><span class="sxs-lookup"><span data-stu-id="7dc73-166">The data model</span></span>

<span data-ttu-id="7dc73-167">W poniższych sekcjach opisano tworzenie modelu danych:</span><span class="sxs-lookup"><span data-stu-id="7dc73-167">The following sections create a data model:</span></span>

![Diagram modelu danych kurs — rejestracja-ucznia](intro/_static/data-model-diagram.png)

<span data-ttu-id="7dc73-169">Student może zarejestrować się w dowolnej liczbie kursów, a kurs może mieć dowolną liczbę studentów, którzy zarejestrowali się w nim.</span><span class="sxs-lookup"><span data-stu-id="7dc73-169">A student can enroll in any number of courses, and a course can have any number of students enrolled in it.</span></span>

## <a name="the-student-entity"></a><span data-ttu-id="7dc73-170">Jednostki dla uczniów</span><span class="sxs-lookup"><span data-stu-id="7dc73-170">The Student entity</span></span>

![Diagram jednostek dla uczniów](intro/_static/student-entity.png)

* <span data-ttu-id="7dc73-172">Utwórz folder *models* w folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-172">Create a *Models* folder in the project folder.</span></span> 

* <span data-ttu-id="7dc73-173">Utwórz *modele/uczniów. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="7dc73-173">Create *Models/Student.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Student.cs)]

<span data-ttu-id="7dc73-174">`ID` Właściwość zostanie przestała kolumną klucza podstawowego tabeli bazy danych, która odnosi się do tej klasy.</span><span class="sxs-lookup"><span data-stu-id="7dc73-174">The `ID` property becomes the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="7dc73-175">Domyślnie program EF Core interpretuje właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="7dc73-175">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="7dc73-176">W związku z tym alternatywną automatycznie rozpoznawaną nazwą `Student` klucza podstawowego klasy jest. `StudentID`</span><span class="sxs-lookup"><span data-stu-id="7dc73-176">So the alternative automatically recognized name for the `Student` class primary key is `StudentID`.</span></span>

<span data-ttu-id="7dc73-177">`Enrollments` Właściwość [właściwość nawigacji](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="7dc73-177">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="7dc73-178">Właściwości nawigacji zawierają inne jednostki, które są powiązane z tą jednostką.</span><span class="sxs-lookup"><span data-stu-id="7dc73-178">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="7dc73-179">W takim przypadku `Enrollments` Właściwość `Student` jednostki zawiera wszystkie jednostki, które są powiązane z tym uczniem. `Enrollment`</span><span class="sxs-lookup"><span data-stu-id="7dc73-179">In this case, the `Enrollments` property of a `Student` entity holds all of the `Enrollment` entities that are related to that Student.</span></span> <span data-ttu-id="7dc73-180">Na przykład jeśli wiersz student w bazie danych ma dwa powiązane wiersze rejestracji, `Enrollments` właściwość nawigacji zawiera te dwie jednostki rejestracji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-180">For example, if a Student row in the database has two related Enrollment rows, the `Enrollments` navigation property contains those two Enrollment entities.</span></span> 

<span data-ttu-id="7dc73-181">W bazie danych wiersz rejestracji jest powiązany z wierszem studenta, jeśli jego kolumna StudentID zawiera wartość identyfikatora studenta.</span><span class="sxs-lookup"><span data-stu-id="7dc73-181">In the database, an Enrollment row is related to a Student row if its StudentID column contains the student's ID value.</span></span> <span data-ttu-id="7dc73-182">Załóżmy na przykład, że wiersz student ma identyfikator = 1.</span><span class="sxs-lookup"><span data-stu-id="7dc73-182">For example, suppose a Student row has ID=1.</span></span> <span data-ttu-id="7dc73-183">Powiązane wiersze rejestracji będą mieć StudentID = 1.</span><span class="sxs-lookup"><span data-stu-id="7dc73-183">Related Enrollment rows will have StudentID = 1.</span></span> <span data-ttu-id="7dc73-184">StudentID jest *kluczem obcym* w tabeli rejestracji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-184">StudentID is a *foreign key* in the Enrollment table.</span></span> 

<span data-ttu-id="7dc73-185">Właściwość jest zdefiniowana tak, `ICollection<Enrollment>` ponieważ może istnieć wiele powiązanych jednostek rejestracji. `Enrollments`</span><span class="sxs-lookup"><span data-stu-id="7dc73-185">The `Enrollments` property is defined as `ICollection<Enrollment>` because there may be multiple related Enrollment entities.</span></span> <span data-ttu-id="7dc73-186">Można użyć innych typów kolekcji, takich jak `List<Enrollment>` lub. `HashSet<Enrollment>`</span><span class="sxs-lookup"><span data-stu-id="7dc73-186">You can use other collection types, such as `List<Enrollment>` or `HashSet<Enrollment>`.</span></span> <span data-ttu-id="7dc73-187">Gdy `ICollection<Enrollment>` jest używany, tworzy programu EF Core `HashSet<Enrollment>` kolekcji domyślnie.</span><span class="sxs-lookup"><span data-stu-id="7dc73-187">When `ICollection<Enrollment>` is used, EF Core creates a `HashSet<Enrollment>` collection by default.</span></span>

## <a name="the-enrollment-entity"></a><span data-ttu-id="7dc73-188">Jednostki rejestracji</span><span class="sxs-lookup"><span data-stu-id="7dc73-188">The Enrollment entity</span></span>

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

<span data-ttu-id="7dc73-190">Utwórz *modele/rejestrację. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="7dc73-190">Create *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

<span data-ttu-id="7dc73-191">Właściwość jest kluczem podstawowym; ta jednostka `classnameID` używa wzorca zamiast `ID` samego siebie. `EnrollmentID`</span><span class="sxs-lookup"><span data-stu-id="7dc73-191">The `EnrollmentID` property is the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself.</span></span> <span data-ttu-id="7dc73-192">Dla modelu danych produkcyjnych wybierz jeden wzorzec i użyj go spójnie.</span><span class="sxs-lookup"><span data-stu-id="7dc73-192">For a production data model, choose one pattern and use it consistently.</span></span> <span data-ttu-id="7dc73-193">Ten samouczek używa obu tych metod, aby zilustrować, że obie działają.</span><span class="sxs-lookup"><span data-stu-id="7dc73-193">This tutorial uses both just to illustrate that both work.</span></span> <span data-ttu-id="7dc73-194">Używanie `ID` bez`classname` ułatwi implementowanie niektórych rodzajów zmian modelu danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-194">Using `ID` without `classname` makes it easier to implement some kinds of data model changes.</span></span>

<span data-ttu-id="7dc73-195">`Grade` Właściwość `enum`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-195">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="7dc73-196">Znak zapytania po `Grade` deklaracji typu wskazuje `Grade` , że właściwość [dopuszcza wartość null](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/).</span><span class="sxs-lookup"><span data-stu-id="7dc73-196">The question mark after the `Grade` type declaration indicates that the `Grade` property is [nullable](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/).</span></span> <span data-ttu-id="7dc73-197">Klasa, która ma wartość null, różni się od zerowej klasy&mdash;zerowej oznacza, że Klasa nie jest znana lub nie została jeszcze przypisana.</span><span class="sxs-lookup"><span data-stu-id="7dc73-197">A grade that's null is different from a zero grade&mdash;null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="7dc73-198">`StudentID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Student`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-198">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="7dc73-199">`Enrollment` Jednostka jest skojarzony z jednym `Student` jednostki, więc ta właściwość zawiera pojedynczy `Student` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7dc73-199">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span>

<span data-ttu-id="7dc73-200">`CourseID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Course`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-200">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="7dc73-201">`Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7dc73-201">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="7dc73-202">EF Core interpretuje właściwość jako klucz obcy, jeśli jest on nazwany `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-202">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="7dc73-203">Na przykład`StudentID` jest kluczem obcym `Student` dla właściwości `Student` nawigacji, ponieważ klucz podstawowy jednostki to `ID`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-203">For example,`StudentID` is the foreign key for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="7dc73-204">Może również mieć nazwę właściwości klucza obcego `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-204">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="7dc73-205">Na przykład `CourseID` ponieważ `Course` jest klucz podstawowy jednostki `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-205">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="7dc73-206">Jednostki kursu</span><span class="sxs-lookup"><span data-stu-id="7dc73-206">The Course entity</span></span>

![Diagram jednostek kursu](intro/_static/course-entity.png)

<span data-ttu-id="7dc73-208">Utwórz *modele/kurs. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="7dc73-208">Create *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

<span data-ttu-id="7dc73-209">`Enrollments` Właściwość jest właściwością nawigacji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-209">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="7dc73-210">A `Course` jednostki mogą być one związane z dowolną liczbę `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="7dc73-210">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="7dc73-211">Ten `DatabaseGenerated` atrybut umożliwia aplikacji określenie klucza podstawowego zamiast wygenerowania go przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-211">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the database generate it.</span></span>

<span data-ttu-id="7dc73-212">Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilatora.</span><span class="sxs-lookup"><span data-stu-id="7dc73-212">Build the project to validate that there are no compiler errors.</span></span>

## <a name="scaffold-student-pages"></a><span data-ttu-id="7dc73-213">Strony uczniów tworzenia szkieletu</span><span class="sxs-lookup"><span data-stu-id="7dc73-213">Scaffold Student pages</span></span>

<span data-ttu-id="7dc73-214">W tej sekcji użyjesz narzędzia do tworzenia szkieletów ASP.NET Core do wygenerowania:</span><span class="sxs-lookup"><span data-stu-id="7dc73-214">In this section, you use the ASP.NET Core scaffolding tool to generate:</span></span>

* <span data-ttu-id="7dc73-215">Klasa *kontekstu* EF Core.</span><span class="sxs-lookup"><span data-stu-id="7dc73-215">An EF Core *context* class.</span></span> <span data-ttu-id="7dc73-216">Kontekst jest klasą główną, która koordynuje Entity Framework funkcji dla danego modelu danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-216">The context is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="7dc73-217">Pochodzi od `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="7dc73-217">It derives from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>
* <span data-ttu-id="7dc73-218">Strony Razor obsługujące operacje tworzenia, odczytu, aktualizacji i usuwania (CRUD) dla `Student` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7dc73-218">Razor pages that handle Create, Read, Update, and Delete (CRUD) operations for the `Student` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7dc73-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dc73-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7dc73-220">Utwórz folder *uczniów* w folderze *strony* .</span><span class="sxs-lookup"><span data-stu-id="7dc73-220">Create a *Students* folder in the *Pages* folder.</span></span>
* <span data-ttu-id="7dc73-221">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *strony/uczniowie* i wybierz polecenie **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="7dc73-221">In **Solution Explorer**, right-click the *Pages/Students* folder and select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="7dc73-222">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **strony Razor za pomocą programu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="7dc73-222">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>
* <span data-ttu-id="7dc73-223">W oknie dialogowym **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="7dc73-223">In the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
  * <span data-ttu-id="7dc73-224">W **klasa modelu** listę rozwijaną, wybierz opcję **uczniów (ContosoUniversity.Models)** .</span><span class="sxs-lookup"><span data-stu-id="7dc73-224">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
  * <span data-ttu-id="7dc73-225">W wierszu **klasy kontekstu danych** wybierz **+** znak (plus).</span><span class="sxs-lookup"><span data-stu-id="7dc73-225">In the **Data context class** row, select the **+** (plus) sign.</span></span>
  * <span data-ttu-id="7dc73-226">Zmień nazwę kontekstu danych z *ContosoUniversity. models. ContosoUniversityContext* na *ContosoUniversity. Data. SchoolContext*.</span><span class="sxs-lookup"><span data-stu-id="7dc73-226">Change the data context name from *ContosoUniversity.Models.ContosoUniversityContext* to *ContosoUniversity.Data.SchoolContext*.</span></span>
  * <span data-ttu-id="7dc73-227">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="7dc73-227">Select **Add**.</span></span>

<span data-ttu-id="7dc73-228">Następujące pakiety są instalowane automatycznie:</span><span class="sxs-lookup"><span data-stu-id="7dc73-228">The following packages are automatically installed:</span></span>

* `Microsoft.VisualStudio.Web.CodeGeneration.Design`
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.Extensions.Logging.Debug`
* `Microsoft.EntityFrameworkCore.Tools`

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7dc73-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7dc73-229">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7dc73-230">Uruchom następujące polecenia interfejs wiersza polecenia platformy .NET Core, aby zainstalować wymagane pakiety NuGet:</span><span class="sxs-lookup"><span data-stu-id="7dc73-230">Run the following .NET Core CLI commands to install required NuGet packages:</span></span>

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.Tools --version 3.0.0-*
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
  dotnet add package Microsoft.Extensions.Logging.Debug --version 3.0.0-*
  ```

  <span data-ttu-id="7dc73-231">Pakiet Microsoft. VisualStudio. Web. CodeGeneration. Design jest wymagany do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="7dc73-231">The Microsoft.VisualStudio.Web.CodeGeneration.Design package is required for scaffolding.</span></span> <span data-ttu-id="7dc73-232">Mimo że aplikacja nie będzie używać SQL Server, narzędzie do tworzenia szkieletów musi być pakietem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7dc73-232">Although the app won't use SQL Server, the scaffolding tool needs the SQL Server package.</span></span>

* <span data-ttu-id="7dc73-233">Utwórz folder *stron/uczniów* .</span><span class="sxs-lookup"><span data-stu-id="7dc73-233">Create a *Pages/Students* folder.</span></span>

* <span data-ttu-id="7dc73-234">Uruchom następujące polecenie, aby zainstalować narzędzie do tworzenia [szkieletu ASPNET-CodeGenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span><span class="sxs-lookup"><span data-stu-id="7dc73-234">Run the following command to install the [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
  ```

* <span data-ttu-id="7dc73-235">Uruchom następujące polecenie, aby uzyskać możliwość tworzenia szkieletu stron z uczniami.</span><span class="sxs-lookup"><span data-stu-id="7dc73-235">Run the following command to scaffold Student pages.</span></span>

  <span data-ttu-id="7dc73-236">**W systemie Windows**</span><span class="sxs-lookup"><span data-stu-id="7dc73-236">**On Windows**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
  ```

  <span data-ttu-id="7dc73-237">**W systemie macOS lub Linux**</span><span class="sxs-lookup"><span data-stu-id="7dc73-237">**On macOS or Linux**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
  ```

---

<span data-ttu-id="7dc73-238">Jeśli wystąpił problem z poprzednim krokiem, Skompiluj projekt i ponów próbę wykonania szkieletu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-238">If you have a problem with the preceding step, build the project and retry the scaffold step.</span></span>

<span data-ttu-id="7dc73-239">Proces tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="7dc73-239">The scaffolding process:</span></span>

* <span data-ttu-id="7dc73-240">Tworzy strony Razor w folderze *stron/uczniów* :</span><span class="sxs-lookup"><span data-stu-id="7dc73-240">Creates Razor pages in the *Pages/Students* folder:</span></span>
  * <span data-ttu-id="7dc73-241">*Create. cshtml* i *Create.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="7dc73-241">*Create.cshtml* and *Create.cshtml.cs*</span></span>
  * <span data-ttu-id="7dc73-242">*DELETE. cshtml* i *DELETE.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="7dc73-242">*Delete.cshtml* and *Delete.cshtml.cs*</span></span>
  * <span data-ttu-id="7dc73-243">*Details. cshtml* i *details.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="7dc73-243">*Details.cshtml* and *Details.cshtml.cs*</span></span>
  * <span data-ttu-id="7dc73-244">*Edit. cshtml* i *Edit.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="7dc73-244">*Edit.cshtml* and *Edit.cshtml.cs*</span></span>
  * <span data-ttu-id="7dc73-245">*Index. cshtml* i *index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="7dc73-245">*Index.cshtml* and *Index.cshtml.cs*</span></span>
* <span data-ttu-id="7dc73-246">Tworzy *dane/SchoolContext. cs*.</span><span class="sxs-lookup"><span data-stu-id="7dc73-246">Creates *Data/SchoolContext.cs*.</span></span>
* <span data-ttu-id="7dc73-247">Dodaje kontekst do iniekcji zależności w *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7dc73-247">Adds the context to dependency injection in *Startup.cs*.</span></span>
* <span data-ttu-id="7dc73-248">Dodaje parametry połączenia z bazą danych do pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="7dc73-248">Adds a database connection string to *appsettings.json*.</span></span>

## <a name="database-connection-string"></a><span data-ttu-id="7dc73-249">Parametry połączenia z bazą danych</span><span class="sxs-lookup"><span data-stu-id="7dc73-249">Database connection string</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7dc73-250">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dc73-250">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7dc73-251">Parametry połączenia określają [programu SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="7dc73-251">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> 

[!code-json[Main](intro/samples/cu30/appsettings.json?highlight=11)]

<span data-ttu-id="7dc73-252">LocalDB to Uproszczona wersja aparatu programu SQL Server Express bazy danych i jest przeznaczony do tworzenia aplikacji, a nie do użytku produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="7dc73-252">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="7dc73-253">Domyślnie LocalDB tworzy pliki *MDF* w `C:/Users/<user>` katalogu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-253">By default, LocalDB creates *.mdf* files in the `C:/Users/<user>` directory.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7dc73-254">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7dc73-254">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7dc73-255">Zmień parametry połączenia w taki sposób, aby wskazywały plik bazy danych programu SQLite o nazwie *cu. DB*:</span><span class="sxs-lookup"><span data-stu-id="7dc73-255">Change the connection string to point to a SQLite database file named *CU.db*:</span></span>

[!code-json[Main](intro/samples/cu30/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a><span data-ttu-id="7dc73-256">Aktualizowanie klasy kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="7dc73-256">Update the database context class</span></span>

<span data-ttu-id="7dc73-257">Klasa główna, która koordynuje funkcje EF Core dla danego modelu danych, jest klasą kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-257">The main class that coordinates EF Core functionality for a given data model is the database context class.</span></span> <span data-ttu-id="7dc73-258">Kontekst pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="7dc73-258">The context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="7dc73-259">Kontekst określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-259">The context specifies which entities are included in the data model.</span></span> <span data-ttu-id="7dc73-260">W tym projekcie nosi nazwę klasy `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-260">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="7dc73-261">Aktualizacja *SchoolContext.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7dc73-261">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

<span data-ttu-id="7dc73-262">Wyróżniony kod tworzy [DbSet\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) właściwości dla każdego zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="7dc73-262">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="7dc73-263">W terminologii programu EF Core:</span><span class="sxs-lookup"><span data-stu-id="7dc73-263">In EF Core terminology:</span></span>

* <span data-ttu-id="7dc73-264">Zestaw jednostek zwykle odpowiada tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-264">An entity set typically corresponds to a database table.</span></span>
* <span data-ttu-id="7dc73-265">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="7dc73-265">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="7dc73-266">Ponieważ zestaw jednostek zawiera wiele jednostek, właściwości Nieogólnymi powinny mieć nazwy w liczbie mnogiej.</span><span class="sxs-lookup"><span data-stu-id="7dc73-266">Since an entity set contains multiple entities, the DBSet properties should be plural names.</span></span> <span data-ttu-id="7dc73-267">Ponieważ narzędzie tworzenia szkieletów utworzyło`Student` nieogólnymi, ten krok zmienia go na plural `Students`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-267">Since the scaffolding tool created a`Student` DBSet, this step changes it to plural `Students`.</span></span> 

<span data-ttu-id="7dc73-268">Aby kod Razor Pages był zgodny z nową nazwą nieogólnymi, wprowadź globalną zmianę w całym projekcie `_context.Student` do. `_context.Students`</span><span class="sxs-lookup"><span data-stu-id="7dc73-268">To make the Razor Pages code match the new DBSet name, make a global change across the whole project of `_context.Student` to `_context.Students`.</span></span>  <span data-ttu-id="7dc73-269">Istnieją 8 wystąpień.</span><span class="sxs-lookup"><span data-stu-id="7dc73-269">There are 8 occurrences.</span></span>

<span data-ttu-id="7dc73-270">Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilatora.</span><span class="sxs-lookup"><span data-stu-id="7dc73-270">Build the project to verify there are no compiler errors.</span></span>

## <a name="startupcs"></a><span data-ttu-id="7dc73-271">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="7dc73-271">Startup.cs</span></span>

<span data-ttu-id="7dc73-272">Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7dc73-272">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="7dc73-273">Usługi (takie jak kontekst bazy danych EF Core) są rejestrowane z iniekcją zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-273">Services (such as the EF Core database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="7dc73-274">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="7dc73-274">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="7dc73-275">Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych jest wyświetlany w dalszej części samouczka.</span><span class="sxs-lookup"><span data-stu-id="7dc73-275">The constructor code that gets a database context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="7dc73-276">Narzędzie do tworzenia szkieletów automatycznie zarejestrowało klasę kontekstu z kontenerem iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="7dc73-276">The scaffolding tool automatically registered the context class with the dependency injection container.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7dc73-277">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dc73-277">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7dc73-278">W `ConfigureServices`programie wyróżnione wiersze zostały dodane przez program do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="7dc73-278">In `ConfigureServices`, the highlighted lines were added by the scaffolder:</span></span>

  [!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7dc73-279">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7dc73-279">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7dc73-280">W `ConfigureServices`programie upewnij się, że kod dodany przez program tworzący `UseSqlite`szkielet jest wywoływany.</span><span class="sxs-lookup"><span data-stu-id="7dc73-280">In `ConfigureServices`, make sure the code added by the scaffolder calls `UseSqlite`.</span></span>

  [!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

---

<span data-ttu-id="7dc73-281">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-281">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="7dc73-282">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="7dc73-282">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="7dc73-283">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="7dc73-283">Create the database</span></span>

<span data-ttu-id="7dc73-284">Zaktualizuj *program.cs* , aby utworzyć bazę danych, jeśli nie istnieje:</span><span class="sxs-lookup"><span data-stu-id="7dc73-284">Update *Program.cs* to create the database if it doesn't exist:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

<span data-ttu-id="7dc73-285">Jeśli istnieje baza danych dla kontekstu, Metoda [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) nie przyjmuje żadnej akcji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-285">The [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) method takes no action if a database for the context exists.</span></span> <span data-ttu-id="7dc73-286">Jeśli baza danych nie istnieje, tworzy bazę danych i schemat.</span><span class="sxs-lookup"><span data-stu-id="7dc73-286">If no database exists, it creates the database and schema.</span></span> <span data-ttu-id="7dc73-287">`EnsureCreated`umożliwia korzystanie z następującego przepływu pracy w celu obsługi zmian modelu danych:</span><span class="sxs-lookup"><span data-stu-id="7dc73-287">`EnsureCreated` enables the following workflow for handling data model changes:</span></span>

* <span data-ttu-id="7dc73-288">Usuń bazę danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-288">Delete the database.</span></span> <span data-ttu-id="7dc73-289">Wszystkie istniejące dane zostaną utracone.</span><span class="sxs-lookup"><span data-stu-id="7dc73-289">Any existing data is lost.</span></span>
* <span data-ttu-id="7dc73-290">Zmień model danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-290">Change the data model.</span></span> <span data-ttu-id="7dc73-291">Na przykład Dodaj `EmailAddress` pole.</span><span class="sxs-lookup"><span data-stu-id="7dc73-291">For example, add an `EmailAddress` field.</span></span>
* <span data-ttu-id="7dc73-292">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7dc73-292">Run the app.</span></span>
* <span data-ttu-id="7dc73-293">`EnsureCreated`tworzy bazę danych z nowym schematem.</span><span class="sxs-lookup"><span data-stu-id="7dc73-293">`EnsureCreated` creates a database with the new schema.</span></span>

<span data-ttu-id="7dc73-294">Ten przepływ pracy działa dobrze na etapie opracowywania, gdy schemat jest gwałtownie zmieniany, o ile nie trzeba zachować danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-294">This workflow works well early in development when the schema is rapidly evolving, as long as you don't need to preserve data.</span></span> <span data-ttu-id="7dc73-295">Sytuacja jest inna, gdy dane wprowadzone do bazy danych muszą zostać zachowane.</span><span class="sxs-lookup"><span data-stu-id="7dc73-295">The situation is different when data that has been entered into the database needs to be preserved.</span></span> <span data-ttu-id="7dc73-296">W takim przypadku należy użyć migracji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-296">When that is the case, use migrations.</span></span>

<span data-ttu-id="7dc73-297">W dalszej części tego samouczka zostanie usunięta baza danych, która została `EnsureCreated` utworzona przez program, a zamiast tego zostanie użyta migracja.</span><span class="sxs-lookup"><span data-stu-id="7dc73-297">Later in the tutorial series, you delete the database that was created by `EnsureCreated` and use migrations instead.</span></span> <span data-ttu-id="7dc73-298">Baza danych utworzona przez `EnsureCreated` program nie może zostać zaktualizowana przy użyciu migracji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-298">A database that is created by `EnsureCreated` can't be updated by using migrations.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="7dc73-299">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="7dc73-299">Test the app</span></span>

* <span data-ttu-id="7dc73-300">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7dc73-300">Run the app.</span></span>
* <span data-ttu-id="7dc73-301">Wybierz **studentów** link a następnie **Utwórz nowy**.</span><span class="sxs-lookup"><span data-stu-id="7dc73-301">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="7dc73-302">Przetestuj Edytuj, szczegóły i usuwać łącza.</span><span class="sxs-lookup"><span data-stu-id="7dc73-302">Test the Edit, Details, and Delete links.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="7dc73-303">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="7dc73-303">Seed the database</span></span>

<span data-ttu-id="7dc73-304">`EnsureCreated` Metoda tworzy pustą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-304">The `EnsureCreated` method creates an empty database.</span></span> <span data-ttu-id="7dc73-305">Ta sekcja dodaje kod wypełniający bazę danych danymi testowymi.</span><span class="sxs-lookup"><span data-stu-id="7dc73-305">This section adds code that populates the database with test data.</span></span>

<span data-ttu-id="7dc73-306">Utwórz *dane/Dbinitializeer. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="7dc73-306">Create *Data/DbInitializer.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  <span data-ttu-id="7dc73-307">Kod sprawdza, czy w bazie danych istnieją uczniowie.</span><span class="sxs-lookup"><span data-stu-id="7dc73-307">The code checks if there are any students in the database.</span></span> <span data-ttu-id="7dc73-308">Jeśli nie ma uczniów, dodaje dane testowe do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-308">If there are no students, it adds test data to the database.</span></span> <span data-ttu-id="7dc73-309">Tworzy dane testowe w tablicach, a nie `List<T>` w celu zoptymalizowania wydajności.</span><span class="sxs-lookup"><span data-stu-id="7dc73-309">It creates the test data in arrays rather than `List<T>` collections to optimize performance.</span></span>

* <span data-ttu-id="7dc73-310">W *program.cs*Zastąp `EnsureCreated` wywołanie `DbInitializer.Initialize` wywołaniem:</span><span class="sxs-lookup"><span data-stu-id="7dc73-310">In *Program.cs*, replace the `EnsureCreated` call with a `DbInitializer.Initialize` call:</span></span>

  ```csharp
  // context.Database.EnsureCreated();
  DbInitializer.Initialize(context);
  ````

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7dc73-311">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dc73-311">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7dc73-312">Zatrzymaj aplikację, jeśli jest uruchomiona, a następnie uruchom następujące polecenie w **konsoli Menedżera pakietów** (PMC):</span><span class="sxs-lookup"><span data-stu-id="7dc73-312">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7dc73-313">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7dc73-313">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7dc73-314">Zatrzymaj aplikację, jeśli jest uruchomiona, a następnie usuń plik *cu. DB* .</span><span class="sxs-lookup"><span data-stu-id="7dc73-314">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

* <span data-ttu-id="7dc73-315">Uruchom ponownie aplikację.</span><span class="sxs-lookup"><span data-stu-id="7dc73-315">Restart the app.</span></span>

* <span data-ttu-id="7dc73-316">Wybierz stronę uczniów, aby zobaczyć dane z rozrzutu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-316">Select the Students page to see the seeded data.</span></span>

## <a name="view-the-database"></a><span data-ttu-id="7dc73-317">Wyświetlanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="7dc73-317">View the database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7dc73-318">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dc73-318">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7dc73-319">Otwórz **Eksplorator obiektów SQL Server** (SSOX) z **widoku** menu w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7dc73-319">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
* <span data-ttu-id="7dc73-320">W SSOX wybierz pozycję **(LocalDB) \MSSQLLocalDB > bazy danych > SchoolContext-{GUID}** .</span><span class="sxs-lookup"><span data-stu-id="7dc73-320">In SSOX, select **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span> <span data-ttu-id="7dc73-321">Nazwa bazy danych jest generowana na podstawie podanej wcześniej nazwy kontekstu oraz łącznika i identyfikatora GUID.</span><span class="sxs-lookup"><span data-stu-id="7dc73-321">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span>
* <span data-ttu-id="7dc73-322">Rozwiń **tabel** węzła.</span><span class="sxs-lookup"><span data-stu-id="7dc73-322">Expand the **Tables** node.</span></span>
* <span data-ttu-id="7dc73-323">Kliknij prawym przyciskiem myszy **uczniów** tabeli, a następnie kliknij przycisk **dane widoku** kolumn utworzonych i wiersze wstawione do tabeli.</span><span class="sxs-lookup"><span data-stu-id="7dc73-323">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>
* <span data-ttu-id="7dc73-324">Kliknij prawym przyciskiem myszy tabelę **uczniów** i kliknij polecenie **Wyświetl kod** , aby `Student` zobaczyć, jak model `Student` mapuje się do schematu tabeli.</span><span class="sxs-lookup"><span data-stu-id="7dc73-324">Right-click the **Student** table and click **View Code** to see how the `Student` model maps to the `Student` table schema.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7dc73-325">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7dc73-325">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7dc73-326">Użyj narzędzia SQLite, aby wyświetlić schemat bazy danych i dane referencyjne.</span><span class="sxs-lookup"><span data-stu-id="7dc73-326">Use your SQLite tool to view the database schema and seeded data.</span></span> <span data-ttu-id="7dc73-327">Plik bazy danych ma nazwę *cu. DB* i znajduje się w folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-327">The database file is named *CU.db* and is located in the project folder.</span></span>

---

## <a name="asynchronous-code"></a><span data-ttu-id="7dc73-328">Kod asynchroniczny</span><span class="sxs-lookup"><span data-stu-id="7dc73-328">Asynchronous code</span></span>

<span data-ttu-id="7dc73-329">Programowanie asynchroniczne jest to domyślny tryb dla platformy ASP.NET Core i programem EF Core.</span><span class="sxs-lookup"><span data-stu-id="7dc73-329">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="7dc73-330">Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach, w dużym obciążeniem wszystkie dostępne wątki mogło zostać użyte.</span><span class="sxs-lookup"><span data-stu-id="7dc73-330">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="7dc73-331">Jeśli tak się stanie, serwer nie może przetworzyć nowe żądania aż wątki są zwalniane.</span><span class="sxs-lookup"><span data-stu-id="7dc73-331">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="7dc73-332">Przy użyciu kodu synchronicznego wiele wątków może powiązane, gdy nie są faktycznie czynności wykonują wszelkie prace ponieważ oczekują one operacji We/Wy zakończyć.</span><span class="sxs-lookup"><span data-stu-id="7dc73-332">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="7dc73-333">Za pomocą kodu asynchronicznego podczas procesu jest oczekiwania na we/wy ukończyć, jego wątku jest zwalniana dla serwera na potrzeby przetwarzaniem innych żądań.</span><span class="sxs-lookup"><span data-stu-id="7dc73-333">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="7dc73-334">W efekcie kod asynchroniczny umożliwia efektywniejsze wykorzystanie zasobów serwera, a serwer może obsłużyć więcej ruchu bez opóźnień.</span><span class="sxs-lookup"><span data-stu-id="7dc73-334">As a result, asynchronous code enables server resources to be used more efficiently, and the server can handle more traffic without delays.</span></span>

<span data-ttu-id="7dc73-335">Kod asynchroniczny wprowadzić niewielkiej ilości obciążenia w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="7dc73-335">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="7dc73-336">W sytuacjach o niewielkim ruchu wpływający na wydajność jest nieistotny, podczas w sytuacjach dużego ruchu, potencjalna poprawa wydajności jest znacząca.</span><span class="sxs-lookup"><span data-stu-id="7dc73-336">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="7dc73-337">W poniższym kodzie [async](/dotnet/csharp/language-reference/keywords/async) — słowo kluczowe, `Task<T>` zwracają wartość, `await` — słowo kluczowe, i `ToListAsync` metoda powoduje, że kod, są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="7dc73-337">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* <span data-ttu-id="7dc73-338">`async` — Słowo kluczowe informuje kompilator, aby:</span><span class="sxs-lookup"><span data-stu-id="7dc73-338">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="7dc73-339">Generuj wywołania zwrotne dla części treści metody.</span><span class="sxs-lookup"><span data-stu-id="7dc73-339">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="7dc73-340">Utwórz obiekt [zadania](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) , który jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="7dc73-340">Create the [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) object that's returned.</span></span>
* <span data-ttu-id="7dc73-341">Typ `Task<T>` zwracany reprezentuje bieżącą liczbę zadań.</span><span class="sxs-lookup"><span data-stu-id="7dc73-341">The `Task<T>` return type represents ongoing work.</span></span>
* <span data-ttu-id="7dc73-342">`await` — Słowo kluczowe powoduje, że kompilator podzielić metodę na dwie części.</span><span class="sxs-lookup"><span data-stu-id="7dc73-342">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="7dc73-343">Pierwsza część kończy się za pomocą operacji, który jest uruchamiany asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="7dc73-343">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="7dc73-344">Druga część zostanie przełączone do metody wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-344">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="7dc73-345">`ToListAsync` jest to wersja asynchroniczna elementu `ToList` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="7dc73-345">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="7dc73-346">Kilka rzeczy, w których trzeba pamiętać podczas pisania kodu asynchronicznego, który używa programu EF Core:</span><span class="sxs-lookup"><span data-stu-id="7dc73-346">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="7dc73-347">Tylko instrukcje, które powodują, że zapytania lub polecenia wysyłane do bazy danych są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="7dc73-347">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="7dc73-348">Obejmuje `ToListAsync`, `SingleOrDefaultAsync`, i`FirstOrDefaultAsync`. `SaveChangesAsync`</span><span class="sxs-lookup"><span data-stu-id="7dc73-348">That includes `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="7dc73-349">Nie zawiera instrukcji, które można zmienić `IQueryable`, takich jak `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-349">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="7dc73-350">Z kontekstu programu EF Core nie jest bezpieczny dla wątków: nie należy próbować wykonać wiele operacji wykonywane równolegle.</span><span class="sxs-lookup"><span data-stu-id="7dc73-350">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="7dc73-351">Aby skorzystać z zalet wydajności w kodzie asynchronicznym, należy sprawdzić, czy pakiety biblioteki (na przykład stronicowania) używają wartości asynchronicznej, jeśli są wywoływane EF Core metod wysyłających zapytania do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-351">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the database.</span></span>

<span data-ttu-id="7dc73-352">Aby uzyskać więcej informacji na temat programowania asynchronicznego w .NET, zobacz [Przegląd Async](/dotnet/standard/async) i [programowanie asynchroniczne z async i await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="7dc73-352">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7dc73-353">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="7dc73-353">Next steps</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7dc73-354">Następny samouczek</span><span class="sxs-lookup"><span data-stu-id="7dc73-354">Next tutorial</span></span>](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7dc73-355">Przykładową aplikację sieci web firmy Contoso University pokazuje, jak utworzyć aplikację stron Razor programu ASP.NET Core przy użyciu Core Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="7dc73-355">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="7dc73-356">Przykładowa aplikacja jest witryną sieci web dla uniwersytetu fikcyjnej firmy Contoso.</span><span class="sxs-lookup"><span data-stu-id="7dc73-356">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="7dc73-357">Obejmuje funkcje, takie jak czasowej dla uczniów, tworzenia kurs i przypisania instruktora.</span><span class="sxs-lookup"><span data-stu-id="7dc73-357">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="7dc73-358">Ta strona jest to pierwszy z serii samouczków, które opisują sposób tworzenia przykładowej aplikacji Contoso University.</span><span class="sxs-lookup"><span data-stu-id="7dc73-358">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="7dc73-359">Pobieranie i wyświetlanie ukończonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-359">Download or view the completed app.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="7dc73-360">[Instrukcje pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="7dc73-360">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7dc73-361">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7dc73-361">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7dc73-362">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dc73-362">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7dc73-363">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7dc73-363">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [](~/includes/2.1-SDK.md)]

---

<span data-ttu-id="7dc73-364">Znajomość [stron Razor](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="7dc73-364">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="7dc73-365">Należy wykonać początkujących programistów [Rozpoczynanie pracy ze stronami Razor](xref:tutorials/razor-pages/razor-pages-start) przed rozpoczęciem tej serii.</span><span class="sxs-lookup"><span data-stu-id="7dc73-365">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7dc73-366">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="7dc73-366">Troubleshooting</span></span>

<span data-ttu-id="7dc73-367">Jeśli napotkasz problem, nie można rozpoznać ogólnie można znaleźć rozwiązania, porównując swój kod, aby [projektu ukończona](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="7dc73-367">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="7dc73-368">Dobrym sposobem, aby uzyskać pomoc polega na publikowanie pytań do [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) dla [platformy ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [programu EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="7dc73-368">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="7dc73-369">Aplikacja sieci web firmy Contoso University</span><span class="sxs-lookup"><span data-stu-id="7dc73-369">The Contoso University web app</span></span>

<span data-ttu-id="7dc73-370">Aplikacja wbudowana w tych samouczkach to podstawowe university witryna sieci web.</span><span class="sxs-lookup"><span data-stu-id="7dc73-370">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="7dc73-371">Użytkownicy mogą przeglądać i aktualizacji dla uczniów, kursu i informacji przez instruktorów.</span><span class="sxs-lookup"><span data-stu-id="7dc73-371">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="7dc73-372">Oto kilka ekranów, utworzone w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="7dc73-372">Here are a few of the screens created in the tutorial.</span></span>

![Strona indeksu uczniów](intro/_static/students-index.png)

![Strona edytowania uczniów](intro/_static/student-edit.png)

<span data-ttu-id="7dc73-375">Styl interfejsu użytkownika w tej lokacji znajduje się w pobliżu co to jest generowany przez wbudowane szablony.</span><span class="sxs-lookup"><span data-stu-id="7dc73-375">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="7dc73-376">Samouczek koncentruje się na programu EF Core ze stronami Razor, nie interfejs użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7dc73-376">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="7dc73-377">Tworzenie aplikacji sieci web ContosoUniversity stron Razor</span><span class="sxs-lookup"><span data-stu-id="7dc73-377">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7dc73-378">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dc73-378">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7dc73-379">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="7dc73-379">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="7dc73-380">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7dc73-380">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="7dc73-381">Nadaj projektowi nazwę **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="7dc73-381">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="7dc73-382">Ważne jest, aby nadaj projektowi nazwę *ContosoUniversity* , przestrzenie nazw dopasować sytuacje, kiedy kod jest skopiowane i wklejone.</span><span class="sxs-lookup"><span data-stu-id="7dc73-382">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="7dc73-383">Wybierz **platformy ASP.NET Core 2.1** w listy rozwijanej, a następnie wybierz pozycję **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="7dc73-383">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="7dc73-384">Obrazy te czynności, zobacz [tworzenie aplikacji sieci web Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span><span class="sxs-lookup"><span data-stu-id="7dc73-384">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span></span>
<span data-ttu-id="7dc73-385">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7dc73-385">Run the app.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7dc73-386">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7dc73-386">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="7dc73-387">Ustawianie stylów lokacji</span><span class="sxs-lookup"><span data-stu-id="7dc73-387">Set up the site style</span></span>

<span data-ttu-id="7dc73-388">Kilka zmian, skonfiguruj w menu witryny, układu i strony głównej.</span><span class="sxs-lookup"><span data-stu-id="7dc73-388">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="7dc73-389">Aktualizacja *Pages/Shared/_Layout.cshtml* z następującymi zmianami:</span><span class="sxs-lookup"><span data-stu-id="7dc73-389">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="7dc73-390">Należy zmienić każde wystąpienie "ContosoUniversity" na "Uniwersytet firmy Contoso".</span><span class="sxs-lookup"><span data-stu-id="7dc73-390">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="7dc73-391">Istnieją trzy wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="7dc73-391">There are three occurrences.</span></span>

* <span data-ttu-id="7dc73-392">Dodaj elementy menu dla **studentów**, **kursów**, **Instruktorzy**, i **działów**i Usuń **skontaktuj się z pomocą** wpis menu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-392">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="7dc73-393">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="7dc73-393">The changes are highlighted.</span></span> <span data-ttu-id="7dc73-394">(Wszystkich znaczników jest *nie* wyświetlane.)</span><span class="sxs-lookup"><span data-stu-id="7dc73-394">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="7dc73-395">W *Pages/Index.cshtml*, Zastąp zawartość pliku następującym kodem, aby zamienić tekst o platformie ASP.NET i MVC z tekstem o tej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7dc73-395">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="7dc73-396">Tworzenie modelu danych</span><span class="sxs-lookup"><span data-stu-id="7dc73-396">Create the data model</span></span>

<span data-ttu-id="7dc73-397">Tworzenie klas jednostek dla aplikacji Contoso University.</span><span class="sxs-lookup"><span data-stu-id="7dc73-397">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="7dc73-398">Uruchom przy użyciu następujących trzech jednostek:</span><span class="sxs-lookup"><span data-stu-id="7dc73-398">Start with the following three entities:</span></span>

![Diagram modelu danych kurs — rejestracja-ucznia](intro/_static/data-model-diagram.png)

<span data-ttu-id="7dc73-400">Istnieje relacja jeden do wielu między `Student` i `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="7dc73-400">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="7dc73-401">Istnieje relacja jeden do wielu między `Course` i `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="7dc73-401">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="7dc73-402">Uczniem/uczennicą mogą rejestrować się w dowolnej liczby kursów.</span><span class="sxs-lookup"><span data-stu-id="7dc73-402">A student can enroll in any number of courses.</span></span> <span data-ttu-id="7dc73-403">Kurs może mieć dowolną liczbę uczniów zarejestrowane w nim.</span><span class="sxs-lookup"><span data-stu-id="7dc73-403">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="7dc73-404">W poniższych sekcjach tworzenia klasy dla każdego z tych jednostek.</span><span class="sxs-lookup"><span data-stu-id="7dc73-404">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="7dc73-405">Jednostki dla uczniów</span><span class="sxs-lookup"><span data-stu-id="7dc73-405">The Student entity</span></span>

![Diagram jednostek dla uczniów](intro/_static/student-entity.png)

<span data-ttu-id="7dc73-407">Tworzenie *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-407">Create a *Models* folder.</span></span> <span data-ttu-id="7dc73-408">W *modeli* folderze utwórz plik klasy o nazwie *Student.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7dc73-408">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="7dc73-409">`ID` Właściwość stanie się kolumna klucza podstawowego w tabeli bazy danych (baza danych), która odnosi się do tej klasy.</span><span class="sxs-lookup"><span data-stu-id="7dc73-409">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="7dc73-410">Domyślnie program EF Core interpretuje właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="7dc73-410">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="7dc73-411">W `classnameID`, `classname` jest nazwą klasy.</span><span class="sxs-lookup"><span data-stu-id="7dc73-411">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="7dc73-412">Alternatywnie rozpoznawane automatycznie, klucz podstawowy jest `StudentID` w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="7dc73-412">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="7dc73-413">`Enrollments` Właściwość [właściwość nawigacji](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="7dc73-413">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="7dc73-414">Właściwości nawigacji link do innych jednostek, które są powiązane z tej jednostki.</span><span class="sxs-lookup"><span data-stu-id="7dc73-414">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="7dc73-415">W tym przypadku `Enrollments` właściwość `Student entity` przechowuje wszystkie `Enrollment` jednostek, które są powiązane z tym, które `Student`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-415">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="7dc73-416">Na przykład, jeśli wiersz dla uczniów w bazy danych ma dwa powiązane wiersze rejestracji `Enrollments` właściwość nawigacji zawiera tych dwóch `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="7dc73-416">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="7dc73-417">Powiązane `Enrollment` wiersz jest wierszem, który zawiera ten uczniów wartość klucza podstawowego w `StudentID` kolumny.</span><span class="sxs-lookup"><span data-stu-id="7dc73-417">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="7dc73-418">Na przykład, załóżmy, że dla uczniów o identyfikatorze = 1 ma dwa wiersze `Enrollment` tabeli.</span><span class="sxs-lookup"><span data-stu-id="7dc73-418">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="7dc73-419">`Enrollment` Tabela ma dwa wiersze z `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="7dc73-419">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="7dc73-420">`StudentID` to klucz obcy w `Enrollment` tabela, która określa uczniów w `Student` tabeli.</span><span class="sxs-lookup"><span data-stu-id="7dc73-420">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="7dc73-421">Jeśli właściwość nawigacji może zawierać wiele jednostek, właściwość nawigacji musi być typu listy, takich jak `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-421">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="7dc73-422">`ICollection<T>` można określić, lub typu, takie jak `List<T>` lub `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-422">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="7dc73-423">Gdy `ICollection<T>` jest używany, tworzy programu EF Core `HashSet<T>` kolekcji domyślnie.</span><span class="sxs-lookup"><span data-stu-id="7dc73-423">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="7dc73-424">Właściwości nawigacji, które zawierają wiele jednostek pochodzą z relacji wiele do wielu i jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-424">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="7dc73-425">Jednostki rejestracji</span><span class="sxs-lookup"><span data-stu-id="7dc73-425">The Enrollment entity</span></span>

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

<span data-ttu-id="7dc73-427">W *modeli* folderze utwórz *Enrollment.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7dc73-427">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="7dc73-428">`EnrollmentID` Właściwość jest kluczem podstawowym.</span><span class="sxs-lookup"><span data-stu-id="7dc73-428">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="7dc73-429">Używa tej jednostki `classnameID` wzorca zamiast `ID` takich jak `Student` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7dc73-429">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="7dc73-430">Zwykle programiści wybierz jednym ze wzorców i używać go w całym modelu danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-430">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="7dc73-431">Przy użyciu Identyfikatora bez classname jest wyświetlany później w samouczku, aby ułatwić implementują dziedziczenie w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-431">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="7dc73-432">`Grade` Właściwość `enum`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-432">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="7dc73-433">Znak zapytania po `Grade` deklaracji typu wskazuje, że `Grade` właściwość ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="7dc73-433">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="7dc73-434">Ma wartość null, która różni się od zera klasy korporacyjnej — wartość null oznacza, że wartość nie jest znana lub nie została jeszcze przypisana.</span><span class="sxs-lookup"><span data-stu-id="7dc73-434">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="7dc73-435">`StudentID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Student`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-435">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="7dc73-436">`Enrollment` Jednostka jest skojarzony z jednym `Student` jednostki, więc ta właściwość zawiera pojedynczy `Student` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7dc73-436">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="7dc73-437">`Student` Jednostki różni się od `Student.Enrollments` właściwość nawigacji, która zawiera wiele `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="7dc73-437">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="7dc73-438">`CourseID` Właściwość to klucz obcy i odpowiednią właściwość nawigacji jest `Course`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-438">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="7dc73-439">`Enrollment` Jednostka jest skojarzony z jednym `Course` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7dc73-439">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="7dc73-440">EF Core interpretuje właściwość jako klucz obcy, jeśli jest on nazwany `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-440">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="7dc73-441">Na przykład`StudentID` dla `Student` właściwość nawigacji, ponieważ `Student` jest klucz podstawowy jednostki `ID`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-441">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="7dc73-442">Może również mieć nazwę właściwości klucza obcego `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-442">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="7dc73-443">Na przykład `CourseID` ponieważ `Course` jest klucz podstawowy jednostki `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-443">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="7dc73-444">Jednostki kursu</span><span class="sxs-lookup"><span data-stu-id="7dc73-444">The Course entity</span></span>

![Diagram jednostek kursu](intro/_static/course-entity.png)

<span data-ttu-id="7dc73-446">W *modeli* folderze utwórz *Course.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7dc73-446">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="7dc73-447">`Enrollments` Właściwość jest właściwością nawigacji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-447">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="7dc73-448">A `Course` jednostki mogą być one związane z dowolną liczbę `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="7dc73-448">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="7dc73-449">`DatabaseGenerated` Atrybut umożliwia aplikacji określenie klucza podstawowego, a nie bazy danych po jego wygenerowaniu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-449">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="7dc73-450">Tworzenie szkieletu modelu dla uczniów</span><span class="sxs-lookup"><span data-stu-id="7dc73-450">Scaffold the student model</span></span>

<span data-ttu-id="7dc73-451">W tej sekcji jest szkieletu modelu dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="7dc73-451">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="7dc73-452">Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) dla modelu dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="7dc73-452">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="7dc73-453">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="7dc73-453">Build the project.</span></span>
* <span data-ttu-id="7dc73-454">Tworzenie *stron/uczniów* folderu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-454">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7dc73-455">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dc73-455">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7dc73-456">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *stron/uczniów* folder > **Dodaj** > **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="7dc73-456">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="7dc73-457">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **strony Razor za pomocą programu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="7dc73-457">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="7dc73-458">Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="7dc73-458">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="7dc73-459">W **klasa modelu** listę rozwijaną, wybierz opcję **uczniów (ContosoUniversity.Models)** .</span><span class="sxs-lookup"><span data-stu-id="7dc73-459">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="7dc73-460">W **klasa kontekstu danych** wiersz, wybierz opcję **+** (plus) Zaloguj się i Zmień nazwę wygenerowanego na **ContosoUniversity.Models.SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="7dc73-460">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="7dc73-461">W **klasa kontekstu danych** listę rozwijaną, wybierz opcję **ContosoUniversity.Models.SchoolContext**</span><span class="sxs-lookup"><span data-stu-id="7dc73-461">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="7dc73-462">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="7dc73-462">Select **Add**.</span></span>

![Okno dialogowe CRUD](intro/_static/s1.png)

<span data-ttu-id="7dc73-464">Zobacz [tworzenia szkieletu modelu movie](xref:tutorials/razor-pages/model#scaffold-the-movie-model) Jeśli masz problem z poprzedniego kroku.</span><span class="sxs-lookup"><span data-stu-id="7dc73-464">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7dc73-465">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7dc73-465">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7dc73-466">Uruchom następujące polecenia, aby utworzyć szkielet modelu dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="7dc73-466">Run the following commands to scaffold the student model.</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

---

<span data-ttu-id="7dc73-467">Proces szkieletu tworzonych i zmienianych następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="7dc73-467">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="7dc73-468">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="7dc73-468">Files created</span></span>

* <span data-ttu-id="7dc73-469">*Strony/uczniów* tworzenie, edytowanie usuwania, uzyskać szczegółowe informacje, indeksu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-469">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="7dc73-470">*Data/SchoolContext.cs*</span><span class="sxs-lookup"><span data-stu-id="7dc73-470">*Data/SchoolContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="7dc73-471">Aktualizacje plików</span><span class="sxs-lookup"><span data-stu-id="7dc73-471">File updates</span></span>

* <span data-ttu-id="7dc73-472">*Startup.cs* : Zmiany w tym pliku opisano szczegółowo w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-472">*Startup.cs* : Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="7dc73-473">*appSettings. JSON* : Dodano parametry połączenia używane do nawiązania połączenia z lokalną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-473">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="7dc73-474">Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="7dc73-474">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="7dc73-475">Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7dc73-475">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="7dc73-476">Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-476">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="7dc73-477">Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="7dc73-477">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="7dc73-478">Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="7dc73-478">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="7dc73-479">Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="7dc73-479">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="7dc73-480">Sprawdź `ConfigureServices` method in Class metoda *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7dc73-480">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="7dc73-481">Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="7dc73-481">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="7dc73-482">Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-482">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="7dc73-483">Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="7dc73-483">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="7dc73-484">Zaktualizuj główne</span><span class="sxs-lookup"><span data-stu-id="7dc73-484">Update main</span></span>

<span data-ttu-id="7dc73-485">W *Program.cs*, zmodyfikuj `Main` metodę, aby wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="7dc73-485">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="7dc73-486">Pobierz wystąpienia kontekstu bazy danych z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="7dc73-486">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="7dc73-487">Wywołaj [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span><span class="sxs-lookup"><span data-stu-id="7dc73-487">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="7dc73-488">Usuń kontekst podczas `EnsureCreated` ukończeniu metody.</span><span class="sxs-lookup"><span data-stu-id="7dc73-488">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="7dc73-489">Poniższy kod przedstawia zaktualizowanego *Program.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="7dc73-489">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="7dc73-490">`EnsureCreated` zapewnia, że istnieje baza danych dla kontekstu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-490">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="7dc73-491">Jeśli istnieje, nie podjęto żadnej akcji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-491">If it exists, no action is taken.</span></span> <span data-ttu-id="7dc73-492">Jeśli nie istnieje, bazy danych i jego schematu są tworzone.</span><span class="sxs-lookup"><span data-stu-id="7dc73-492">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="7dc73-493">`EnsureCreated` Używaj migracji do tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-493">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="7dc73-494">Bazy danych, która jest tworzona przy użyciu `EnsureCreated` nie można później zaktualizować przy użyciu migracji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-494">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="7dc73-495">`EnsureCreated` jest wywoływana przy uruchomieniu aplikacji, co pozwala następujący przepływ pracy:</span><span class="sxs-lookup"><span data-stu-id="7dc73-495">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="7dc73-496">Usuwanie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-496">Delete the DB.</span></span>
* <span data-ttu-id="7dc73-497">Zmiany schematu bazy danych (na przykład dodać `EmailAddress` pola).</span><span class="sxs-lookup"><span data-stu-id="7dc73-497">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="7dc73-498">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7dc73-498">Run the app.</span></span>
* <span data-ttu-id="7dc73-499">`EnsureCreated` Tworzy BAZĘ danych za pomocą`EmailAddress` kolumny.</span><span class="sxs-lookup"><span data-stu-id="7dc73-499">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="7dc73-500">`EnsureCreated` jest wygodne na wczesnym etapie projektowania, gdy schemat szybko się zmienia.</span><span class="sxs-lookup"><span data-stu-id="7dc73-500">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="7dc73-501">Później w samouczku baza danych zostanie usunięty i migracje są używane.</span><span class="sxs-lookup"><span data-stu-id="7dc73-501">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="7dc73-502">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="7dc73-502">Test the app</span></span>

<span data-ttu-id="7dc73-503">Uruchom aplikację i zaakceptować zasady plików cookie.</span><span class="sxs-lookup"><span data-stu-id="7dc73-503">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="7dc73-504">Ta aplikacja nie zachowuje informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-504">This app doesn't keep personal information.</span></span> <span data-ttu-id="7dc73-505">Możesz przeczytać temat zasady plików cookie w [Obsługa Unii Europejskiej ogólnego danych (GDPR Protection Regulation)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="7dc73-505">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="7dc73-506">Wybierz **studentów** link a następnie **Utwórz nowy**.</span><span class="sxs-lookup"><span data-stu-id="7dc73-506">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="7dc73-507">Przetestuj Edytuj, szczegóły i usuwać łącza.</span><span class="sxs-lookup"><span data-stu-id="7dc73-507">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="7dc73-508">Badanie kontekstu SchoolContext DB</span><span class="sxs-lookup"><span data-stu-id="7dc73-508">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="7dc73-509">Główna klasa, która służy do koordynowania funkcji EF Core dla modelu danych jest z klasy kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-509">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="7dc73-510">Kontekst danych jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="7dc73-510">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="7dc73-511">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-511">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="7dc73-512">W tym projekcie nosi nazwę klasy `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-512">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="7dc73-513">Aktualizacja *SchoolContext.cs* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7dc73-513">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="7dc73-514">Wyróżniony kod tworzy [DbSet\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) właściwości dla każdego zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="7dc73-514">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="7dc73-515">W terminologii programu EF Core:</span><span class="sxs-lookup"><span data-stu-id="7dc73-515">In EF Core terminology:</span></span>

* <span data-ttu-id="7dc73-516">Zwykle zestaw jednostek odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-516">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="7dc73-517">Jednostki odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="7dc73-517">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="7dc73-518">`DbSet<Enrollment>` i `DbSet<Course>` mógł zostać pominięty.</span><span class="sxs-lookup"><span data-stu-id="7dc73-518">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="7dc73-519">EF Core je uwzględniał niejawnie ponieważ `Student` odwołań do jednostek `Enrollment` jednostki, a `Enrollment` odwołań do jednostek `Course` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7dc73-519">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="7dc73-520">Na potrzeby tego samouczka Zachowaj `DbSet<Enrollment>` i `DbSet<Course>` w `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-520">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="7dc73-521">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="7dc73-521">SQL Server Express LocalDB</span></span>

<span data-ttu-id="7dc73-522">Parametry połączenia określają [programu SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="7dc73-522">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="7dc73-523">LocalDB to Uproszczona wersja aparatu programu SQL Server Express bazy danych i jest przeznaczony do tworzenia aplikacji, a nie do użytku produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="7dc73-523">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="7dc73-524">LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-524">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="7dc73-525">Domyślnie tworzy LocalDB *.mdf* pliki bazy danych `C:/Users/<user>` katalogu.</span><span class="sxs-lookup"><span data-stu-id="7dc73-525">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="7dc73-526">Dodaj kod, aby zainicjować bazy danych przy użyciu danych testowych</span><span class="sxs-lookup"><span data-stu-id="7dc73-526">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="7dc73-527">EF Core tworzy pustą bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-527">EF Core creates an empty DB.</span></span> <span data-ttu-id="7dc73-528">W tej sekcji `Initialize` metody są zapisywane do wypełniania danych testowych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-528">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="7dc73-529">W *danych* folderu, Utwórz nowy plik klasy o nazwie *DbInitializer.cs* i Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="7dc73-529">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="7dc73-530">Uwaga: Poprzedzający kod używa `Models` dla przestrzeni nazw (`namespace ContosoUniversity.Models`), a `Data`nie.</span><span class="sxs-lookup"><span data-stu-id="7dc73-530">Note: The preceding code uses `Models` for the namespace (`namespace ContosoUniversity.Models`) rather than `Data`.</span></span> <span data-ttu-id="7dc73-531">`Models`jest spójny z kodem generowanym przez program rusztowaer.</span><span class="sxs-lookup"><span data-stu-id="7dc73-531">`Models` is consistent with the scaffolder-generated code.</span></span> <span data-ttu-id="7dc73-532">Aby uzyskać więcej informacji, zobacz [ten problem z szkieletem usługi GitHub](https://github.com/aspnet/Scaffolding/issues/822).</span><span class="sxs-lookup"><span data-stu-id="7dc73-532">For more information, see [this GitHub scaffolding issue](https://github.com/aspnet/Scaffolding/issues/822).</span></span>

<span data-ttu-id="7dc73-533">Kod sprawdza, czy wszystkie studentów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-533">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="7dc73-534">W przypadku nie studentów w bazie danych, baza danych jest inicjowany z danych testowych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-534">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="7dc73-535">Ładuje dane testowe do tablic zamiast `List<T>` kolekcje w celu zoptymalizowania wydajności.</span><span class="sxs-lookup"><span data-stu-id="7dc73-535">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="7dc73-536">`EnsureCreated` Metoda automatycznie tworzy bazy danych, aby uzyskać kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-536">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="7dc73-537">Jeśli baza danych istnieje, `EnsureCreated` zwraca bez modyfikowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-537">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="7dc73-538">W *Program.cs*, zmodyfikuj `Main` metodę do wywołania `Initialize`:</span><span class="sxs-lookup"><span data-stu-id="7dc73-538">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7dc73-539">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dc73-539">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7dc73-540">Zatrzymaj aplikację, jeśli jest uruchomiona, a następnie uruchom następujące polecenie w **konsoli Menedżera pakietów** (PMC):</span><span class="sxs-lookup"><span data-stu-id="7dc73-540">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7dc73-541">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7dc73-541">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7dc73-542">Zatrzymaj aplikację, jeśli jest uruchomiona, a następnie usuń plik *cu. DB* .</span><span class="sxs-lookup"><span data-stu-id="7dc73-542">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

## <a name="view-the-db"></a><span data-ttu-id="7dc73-543">Widok bazy danych</span><span class="sxs-lookup"><span data-stu-id="7dc73-543">View the DB</span></span>

<span data-ttu-id="7dc73-544">Nazwa bazy danych jest generowana na podstawie podanej wcześniej nazwy kontekstu oraz łącznika i identyfikatora GUID.</span><span class="sxs-lookup"><span data-stu-id="7dc73-544">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span> <span data-ttu-id="7dc73-545">W rezultacie nazwa bazy danych będzie "SchoolContext-{GUID}".</span><span class="sxs-lookup"><span data-stu-id="7dc73-545">Thus, the database name will be "SchoolContext-{GUID}".</span></span> <span data-ttu-id="7dc73-546">Identyfikator GUID będzie różny dla każdego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7dc73-546">The GUID will be different for each user.</span></span>
<span data-ttu-id="7dc73-547">Otwórz **Eksplorator obiektów SQL Server** (SSOX) z **widoku** menu w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7dc73-547">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="7dc73-548">W SSOX kliknij pozycję **(LocalDB) \MSSQLLocalDB > bazy danych > SchoolContext-{GUID}** .</span><span class="sxs-lookup"><span data-stu-id="7dc73-548">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span>

<span data-ttu-id="7dc73-549">Rozwiń **tabel** węzła.</span><span class="sxs-lookup"><span data-stu-id="7dc73-549">Expand the **Tables** node.</span></span>

<span data-ttu-id="7dc73-550">Kliknij prawym przyciskiem myszy **uczniów** tabeli, a następnie kliknij przycisk **dane widoku** kolumn utworzonych i wiersze wstawione do tabeli.</span><span class="sxs-lookup"><span data-stu-id="7dc73-550">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="7dc73-551">Kod asynchroniczny</span><span class="sxs-lookup"><span data-stu-id="7dc73-551">Asynchronous code</span></span>

<span data-ttu-id="7dc73-552">Programowanie asynchroniczne jest to domyślny tryb dla platformy ASP.NET Core i programem EF Core.</span><span class="sxs-lookup"><span data-stu-id="7dc73-552">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="7dc73-553">Serwer sieci web ma ograniczoną liczbę dostępnych wątków, a w sytuacjach, w dużym obciążeniem wszystkie dostępne wątki mogło zostać użyte.</span><span class="sxs-lookup"><span data-stu-id="7dc73-553">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="7dc73-554">Jeśli tak się stanie, serwer nie może przetworzyć nowe żądania aż wątki są zwalniane.</span><span class="sxs-lookup"><span data-stu-id="7dc73-554">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="7dc73-555">Przy użyciu kodu synchronicznego wiele wątków może powiązane, gdy nie są faktycznie czynności wykonują wszelkie prace ponieważ oczekują one operacji We/Wy zakończyć.</span><span class="sxs-lookup"><span data-stu-id="7dc73-555">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="7dc73-556">Za pomocą kodu asynchronicznego podczas procesu jest oczekiwania na we/wy ukończyć, jego wątku jest zwalniana dla serwera na potrzeby przetwarzaniem innych żądań.</span><span class="sxs-lookup"><span data-stu-id="7dc73-556">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="7dc73-557">W rezultacie kod asynchroniczny umożliwia bardziej efektywne wykorzystanie zasobów serwera i serwera jest włączona, aby obsłużyć większy ruch, bez opóźnień.</span><span class="sxs-lookup"><span data-stu-id="7dc73-557">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="7dc73-558">Kod asynchroniczny wprowadzić niewielkiej ilości obciążenia w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="7dc73-558">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="7dc73-559">W sytuacjach o niewielkim ruchu wpływający na wydajność jest nieistotny, podczas w sytuacjach dużego ruchu, potencjalna poprawa wydajności jest znacząca.</span><span class="sxs-lookup"><span data-stu-id="7dc73-559">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="7dc73-560">W poniższym kodzie [async](/dotnet/csharp/language-reference/keywords/async) — słowo kluczowe, `Task<T>` zwracają wartość, `await` — słowo kluczowe, i `ToListAsync` metoda powoduje, że kod, są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="7dc73-560">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="7dc73-561">`async` — Słowo kluczowe informuje kompilator, aby:</span><span class="sxs-lookup"><span data-stu-id="7dc73-561">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="7dc73-562">Generuj wywołania zwrotne dla części treści metody.</span><span class="sxs-lookup"><span data-stu-id="7dc73-562">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="7dc73-563">Automatycznie twórz [zadań](/dotnet/api/system.threading.tasks.task) obiekt, który jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="7dc73-563">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="7dc73-564">Aby uzyskać więcej informacji, zobacz [typie zwracanym zadań](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="7dc73-564">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="7dc73-565">Niejawny zwracany typ `Task` reprezentuje pracę w toku.</span><span class="sxs-lookup"><span data-stu-id="7dc73-565">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="7dc73-566">`await` — Słowo kluczowe powoduje, że kompilator podzielić metodę na dwie części.</span><span class="sxs-lookup"><span data-stu-id="7dc73-566">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="7dc73-567">Pierwsza część kończy się za pomocą operacji, który jest uruchamiany asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="7dc73-567">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="7dc73-568">Druga część zostanie przełączone do metody wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.</span><span class="sxs-lookup"><span data-stu-id="7dc73-568">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="7dc73-569">`ToListAsync` jest to wersja asynchroniczna elementu `ToList` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="7dc73-569">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="7dc73-570">Kilka rzeczy, w których trzeba pamiętać podczas pisania kodu asynchronicznego, który używa programu EF Core:</span><span class="sxs-lookup"><span data-stu-id="7dc73-570">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="7dc73-571">Tylko te instrukcje, które powodują zapytań i poleceń do wysłania do bazy danych są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="7dc73-571">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="7dc73-572">Zawierającej, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, i `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-572">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="7dc73-573">Nie zawiera instrukcji, które można zmienić `IQueryable`, takich jak `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="7dc73-573">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="7dc73-574">Z kontekstu programu EF Core nie jest bezpieczny dla wątków: nie należy próbować wykonać wiele operacji wykonywane równolegle.</span><span class="sxs-lookup"><span data-stu-id="7dc73-574">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="7dc73-575">Aby móc korzystać z zalet wydajności kod asynchroniczny, sprawdź, czy biblioteka pakietów (np. stronicowania) używają asynchronicznej, jeśli wywołują metod programu EF Core, które wysyłają zapytania do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7dc73-575">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="7dc73-576">Aby uzyskać więcej informacji na temat programowania asynchronicznego w .NET, zobacz [Przegląd Async](/dotnet/standard/async) i [programowanie asynchroniczne z async i await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="7dc73-576">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="7dc73-577">W następnym samouczku basic CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) są badane.</span><span class="sxs-lookup"><span data-stu-id="7dc73-577">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>



## <a name="additional-resources"></a><span data-ttu-id="7dc73-578">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7dc73-578">Additional resources</span></span>

* [<span data-ttu-id="7dc73-579">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="7dc73-579">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [<span data-ttu-id="7dc73-580">Next</span><span class="sxs-lookup"><span data-stu-id="7dc73-580">Next</span></span>](xref:data/ef-rp/crud)

::: moniker-end
