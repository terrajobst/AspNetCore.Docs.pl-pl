---
title: Razor Pages z Entity Framework Core w ASP.NET Core — samouczek 1 z 8
author: rick-anderson
description: Pokazuje, jak utworzyć aplikację Razor Pages przy użyciu Entity Framework Core
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 09/26/2019
uid: data/ef-rp/intro
ms.openlocfilehash: 01e507326ddd57057aa178ad3909fd4027a013fd
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259373"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="d61c1-103">Razor Pages z Entity Framework Core w ASP.NET Core — samouczek 1 z 8</span><span class="sxs-lookup"><span data-stu-id="d61c1-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

<span data-ttu-id="d61c1-104">Autorzy [Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d61c1-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d61c1-105">Jest to pierwsza z serii samouczków, które pokazują, jak używać rdzenia Entity Framework (EF) w aplikacji [ASP.NET Core Razor Pages](xref:razor-pages/index) .</span><span class="sxs-lookup"><span data-stu-id="d61c1-105">This is the first in a series of tutorials that show how to use Entity Framework (EF) Core in an [ASP.NET Core Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="d61c1-106">Samouczki budują witrynę sieci Web dla fikcyjnej uczelni firmy Contoso.</span><span class="sxs-lookup"><span data-stu-id="d61c1-106">The tutorials build a web site for a fictional Contoso University.</span></span> <span data-ttu-id="d61c1-107">Lokacja obejmuje funkcje, takie jak przyjmowanie uczniów, tworzenie kursu i przydziały instruktora.</span><span class="sxs-lookup"><span data-stu-id="d61c1-107">The site includes functionality such as student admission, course creation, and instructor assignments.</span></span>

[<span data-ttu-id="d61c1-108">Pobierz lub Wyświetl ukończoną aplikację.</span><span class="sxs-lookup"><span data-stu-id="d61c1-108">Download or view the completed app.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="d61c1-109">[Instrukcje pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d61c1-109">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d61c1-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d61c1-110">Prerequisites</span></span>

* <span data-ttu-id="d61c1-111">Jeśli dopiero zaczynasz korzystać z Razor Pages, przejdź do serii samouczek wprowadzenie [do Razor Pages](xref:tutorials/razor-pages/razor-pages-start) , przed rozpoczęciem tej.</span><span class="sxs-lookup"><span data-stu-id="d61c1-111">If you're new to Razor Pages, go through the [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) tutorial series before starting this one.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d61c1-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d61c1-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[VS prereqs](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d61c1-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d61c1-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[VS Code prereqs](~/includes/net-core-prereqs-vsc-3.0.md)]

---

## <a name="database-engines"></a><span data-ttu-id="d61c1-114">Aparaty bazy danych</span><span class="sxs-lookup"><span data-stu-id="d61c1-114">Database engines</span></span>

<span data-ttu-id="d61c1-115">Instrukcje programu Visual Studio używają [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), wersji SQL Server Express, która działa tylko w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="d61c1-115">The Visual Studio instructions use [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), a version of SQL Server Express that runs only on Windows.</span></span>

<span data-ttu-id="d61c1-116">Instrukcje Visual Studio Code korzystają z [oprogramowania SQLite](https://www.sqlite.org/), wieloplatformowego aparatu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-116">The Visual Studio Code instructions use [SQLite](https://www.sqlite.org/), a cross-platform database engine.</span></span>

<span data-ttu-id="d61c1-117">Jeśli zdecydujesz się na korzystanie z oprogramowania SQLite, Pobierz i zainstaluj narzędzie innej firmy służące do zarządzania bazą danych programu SQLite i wyświetlania jej, na przykład w [przeglądarce DB dla oprogramowania SQLite](https://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="d61c1-117">If you choose to use SQLite, download and install a third-party tool for managing and viewing a SQLite database, such as [DB Browser for SQLite](https://sqlitebrowser.org/).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="d61c1-118">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="d61c1-118">Troubleshooting</span></span>

<span data-ttu-id="d61c1-119">Jeśli wystąpi problem, którego nie można rozwiązać, porównaj swój kod z [zakończonym projektem](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="d61c1-119">If you run into a problem you can't resolve, compare your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="d61c1-120">Dobrym sposobem uzyskania pomocy jest opublikowanie pytania do StackOverflow.com przy użyciu [tagu ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [tagu EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="d61c1-120">A good way to get help is by posting a question to StackOverflow.com, using the [ASP.NET Core tag](https://stackoverflow.com/questions/tagged/asp.net-core) or the [EF Core tag](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-sample-app"></a><span data-ttu-id="d61c1-121">Przykładowa aplikacja</span><span class="sxs-lookup"><span data-stu-id="d61c1-121">The sample app</span></span>

<span data-ttu-id="d61c1-122">Aplikacja skompilowana w tych samouczkach jest podstawową szkołą w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d61c1-122">The app built in these tutorials is a basic university web site.</span></span> <span data-ttu-id="d61c1-123">Użytkownicy mogą wyświetlać i aktualizować informacje dotyczące uczniów, kursów i instruktorów.</span><span class="sxs-lookup"><span data-stu-id="d61c1-123">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="d61c1-124">Poniżej przedstawiono kilka ekranów utworzonych w samouczku.</span><span class="sxs-lookup"><span data-stu-id="d61c1-124">Here are a few of the screens created in the tutorial.</span></span>

![Strona indeksu uczniów](intro/_static/students-index30.png)

![Strona edycji uczniów](intro/_static/student-edit30.png)

<span data-ttu-id="d61c1-127">Styl interfejsu użytkownika tej witryny jest oparty na wbudowanych szablonach projektów.</span><span class="sxs-lookup"><span data-stu-id="d61c1-127">The UI style of this site is based on the built-in project templates.</span></span> <span data-ttu-id="d61c1-128">Fokus samouczka dotyczy korzystania z EF Core, a nie dostosowywania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d61c1-128">The tutorial's focus is on how to use EF Core, not how to customize the UI.</span></span>

<span data-ttu-id="d61c1-129">Skorzystaj z linku w górnej części strony, aby uzyskać kod źródłowy dla ukończonego projektu.</span><span class="sxs-lookup"><span data-stu-id="d61c1-129">Follow the link at the top of the page to get the source code for the completed project.</span></span> <span data-ttu-id="d61c1-130">Folder *cu30* ma kod dla ASP.NET Core wersji 3,0 samouczka.</span><span class="sxs-lookup"><span data-stu-id="d61c1-130">The *cu30* folder has the code for the ASP.NET Core 3.0 version of the tutorial.</span></span> <span data-ttu-id="d61c1-131">Pliki odzwierciedlające stan kodu dla samouczków 1-7 można znaleźć w folderze *cu30snapshots* .</span><span class="sxs-lookup"><span data-stu-id="d61c1-131">Files that reflect the state of the code for tutorials 1-7 can be found in the *cu30snapshots* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d61c1-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d61c1-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d61c1-133">Aby uruchomić aplikację po pobraniu ukończonego projektu:</span><span class="sxs-lookup"><span data-stu-id="d61c1-133">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="d61c1-134">Usuń trzy pliki i jeden folder, w którym znajduje się nazwa *oprogramowania SQLite* .</span><span class="sxs-lookup"><span data-stu-id="d61c1-134">Delete three files and one folder that have *SQLite* in the name.</span></span>
* <span data-ttu-id="d61c1-135">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="d61c1-135">Build the project.</span></span>
* <span data-ttu-id="d61c1-136">W konsoli Menedżera pakietów (PMC) Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="d61c1-136">In Package Manager Console (PMC) run the following command:</span></span>

  ```powershell
  Update-Database
  ```

* <span data-ttu-id="d61c1-137">Uruchom projekt, aby wypełniać bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-137">Run the project to seed the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d61c1-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d61c1-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d61c1-139">Aby uruchomić aplikację po pobraniu ukończonego projektu:</span><span class="sxs-lookup"><span data-stu-id="d61c1-139">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="d61c1-140">Usuń *ContosoUniversity. csproj*i Zmień nazwę *ContosoUniversitySQLite. csproj* na *ContosoUniversity. csproj*.</span><span class="sxs-lookup"><span data-stu-id="d61c1-140">Delete *ContosoUniversity.csproj*, and rename *ContosoUniversitySQLite.csproj* to *ContosoUniversity.csproj*.</span></span>
* <span data-ttu-id="d61c1-141">Usuń *Startup.cs*i zmień nazwę *StartupSQLite.cs* na *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d61c1-141">Delete *Startup.cs*, and rename *StartupSQLite.cs* to *Startup.cs*.</span></span>
* <span data-ttu-id="d61c1-142">Usuń plik *appSettings. JSON*i Zmień nazwę *appSettingsSQLite. JSON* na *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="d61c1-142">Delete *appSettings.json*, and rename *appSettingsSQLite.json* to *appSettings.json*.</span></span>
* <span data-ttu-id="d61c1-143">Usuń folder *migracji* , a następnie zmień nazwę *MigrationsSQL* na *migracji*.</span><span class="sxs-lookup"><span data-stu-id="d61c1-143">Delete the *Migrations* folder, and rename *MigrationsSQL* to *Migrations*.</span></span>
* <span data-ttu-id="d61c1-144">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="d61c1-144">Build the project.</span></span>
* <span data-ttu-id="d61c1-145">W wierszu polecenia w folderze projektu uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="d61c1-145">At a command prompt in the project folder, run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet ef database update
  ```

* <span data-ttu-id="d61c1-146">W narzędziu SQLite uruchom następującą instrukcję SQL:</span><span class="sxs-lookup"><span data-stu-id="d61c1-146">In your SQLite tool, run the following SQL statement:</span></span>

  ```sql
  UPDATE Department SET RowVersion = randomblob(8)
  ```

* <span data-ttu-id="d61c1-147">Uruchom projekt, aby wypełniać bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-147">Run the project to seed the database.</span></span>

---

## <a name="create-the-web-app-project"></a><span data-ttu-id="d61c1-148">Tworzenie projektu aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="d61c1-148">Create the web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d61c1-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d61c1-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d61c1-150">Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt**>.</span><span class="sxs-lookup"><span data-stu-id="d61c1-150">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d61c1-151">Wybierz **ASP.NET Core aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="d61c1-151">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="d61c1-152">Nazwij projekt *ContosoUniversity*.</span><span class="sxs-lookup"><span data-stu-id="d61c1-152">Name the project *ContosoUniversity*.</span></span> <span data-ttu-id="d61c1-153">Ważne jest, aby użyć tej dokładnej nazwy, łącznie z wielką literą, więc przestrzenie nazw są zgodne, gdy kod jest kopiowany i wklejany.</span><span class="sxs-lookup"><span data-stu-id="d61c1-153">It's important to use this exact name including capitalization, so the namespaces match when code is copied and pasted.</span></span>
* <span data-ttu-id="d61c1-154">Na liście rozwijanej wybierz pozycję **.NET Core** i **ASP.NET Core 3,0** , a następnie wybierz pozycję **aplikacja sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="d61c1-154">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdowns, and then select **Web Application**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d61c1-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d61c1-155">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d61c1-156">W terminalu przejdź do folderu, w którym ma zostać utworzony folder projektu.</span><span class="sxs-lookup"><span data-stu-id="d61c1-156">In a terminal, navigate to the folder in which the project folder should be created.</span></span>

* <span data-ttu-id="d61c1-157">Uruchom następujące polecenia, aby utworzyć projekt Razor Pages i `cd` do nowego folderu projektu:</span><span class="sxs-lookup"><span data-stu-id="d61c1-157">Run the following commands to create a Razor Pages project and `cd` into the new project folder:</span></span>

  ```dotnetcli
  dotnet new webapp -o ContosoUniversity
  cd ContosoUniversity
  ```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="d61c1-158">Konfigurowanie stylu witryny</span><span class="sxs-lookup"><span data-stu-id="d61c1-158">Set up the site style</span></span>

<span data-ttu-id="d61c1-159">Skonfiguruj nagłówek, stopkę i menu witryny przez aktualizację *stron/Shared/_Layout. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d61c1-159">Set up the site header, footer, and menu by updating *Pages/Shared/_Layout.cshtml*:</span></span>

* <span data-ttu-id="d61c1-160">Zmień każde wystąpienie "ContosoUniversity" na "Uniwersytet firmy Contoso".</span><span class="sxs-lookup"><span data-stu-id="d61c1-160">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="d61c1-161">Istnieją trzy wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="d61c1-161">There are three occurrences.</span></span>

* <span data-ttu-id="d61c1-162">Usuń wpisy menu **Home** i **privacy** oraz Dodaj wpisy dotyczące **osób,** **studentów**, **kursów**, **instruktorów**i **działów**.</span><span class="sxs-lookup"><span data-stu-id="d61c1-162">Delete the **Home** and **Privacy** menu entries, and add entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**.</span></span>

<span data-ttu-id="d61c1-163">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="d61c1-163">The changes are highlighted.</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Shared/_Layout.cshtml?highlight=6,14,21-35,49)]

<span data-ttu-id="d61c1-164">W obszarze *Pages/index. cshtml*Zastąp zawartość pliku następującym kodem, aby zamienić tekst na ASP.NET Core z tekstem dotyczącym tej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d61c1-164">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET Core with text about this app:</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Index.cshtml)]

<span data-ttu-id="d61c1-165">Uruchom aplikację, aby sprawdzić, czy zostanie wyświetlona strona główna.</span><span class="sxs-lookup"><span data-stu-id="d61c1-165">Run the app to verify that the home page appears.</span></span>

## <a name="the-data-model"></a><span data-ttu-id="d61c1-166">Model danych</span><span class="sxs-lookup"><span data-stu-id="d61c1-166">The data model</span></span>

<span data-ttu-id="d61c1-167">W poniższych sekcjach opisano tworzenie modelu danych:</span><span class="sxs-lookup"><span data-stu-id="d61c1-167">The following sections create a data model:</span></span>

![Kurs — Diagram modelu danych ucznia](intro/_static/data-model-diagram.png)

<span data-ttu-id="d61c1-169">Student może zarejestrować się w dowolnej liczbie kursów, a kurs może mieć dowolną liczbę studentów, którzy zarejestrowali się w nim.</span><span class="sxs-lookup"><span data-stu-id="d61c1-169">A student can enroll in any number of courses, and a course can have any number of students enrolled in it.</span></span>

## <a name="the-student-entity"></a><span data-ttu-id="d61c1-170">Jednostka ucznia</span><span class="sxs-lookup"><span data-stu-id="d61c1-170">The Student entity</span></span>

![Diagram jednostek uczniów](intro/_static/student-entity.png)

* <span data-ttu-id="d61c1-172">Utwórz folder *models* w folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="d61c1-172">Create a *Models* folder in the project folder.</span></span> 

* <span data-ttu-id="d61c1-173">Utwórz *modele/uczniów. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="d61c1-173">Create *Models/Student.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Student.cs)]

<span data-ttu-id="d61c1-174">Właściwość `ID` jest kolumną klucza podstawowego tabeli bazy danych, która odnosi się do tej klasy.</span><span class="sxs-lookup"><span data-stu-id="d61c1-174">The `ID` property becomes the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="d61c1-175">Domyślnie EF Core interpretuje właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="d61c1-175">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="d61c1-176">W związku z tym alternatywną automatycznie rozpoznawaną nazwą klucza podstawowego klasy `Student` jest `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-176">So the alternative automatically recognized name for the `Student` class primary key is `StudentID`.</span></span>

<span data-ttu-id="d61c1-177">Właściwość `Enrollments` jest [właściwością nawigacji](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="d61c1-177">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="d61c1-178">Właściwości nawigacji zawierają inne jednostki, które są powiązane z tą jednostką.</span><span class="sxs-lookup"><span data-stu-id="d61c1-178">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="d61c1-179">W takim przypadku Właściwość `Enrollments` jednostki `Student` zawiera wszystkie jednostki `Enrollment`, które są powiązane z tym uczniem.</span><span class="sxs-lookup"><span data-stu-id="d61c1-179">In this case, the `Enrollments` property of a `Student` entity holds all of the `Enrollment` entities that are related to that Student.</span></span> <span data-ttu-id="d61c1-180">Na przykład jeśli wiersz student w bazie danych ma dwa powiązane wiersze rejestracji, właściwość nawigacji `Enrollments` zawiera te dwie jednostki rejestracji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-180">For example, if a Student row in the database has two related Enrollment rows, the `Enrollments` navigation property contains those two Enrollment entities.</span></span> 

<span data-ttu-id="d61c1-181">W bazie danych wiersz rejestracji jest powiązany z wierszem studenta, jeśli jego kolumna StudentID zawiera wartość identyfikatora studenta.</span><span class="sxs-lookup"><span data-stu-id="d61c1-181">In the database, an Enrollment row is related to a Student row if its StudentID column contains the student's ID value.</span></span> <span data-ttu-id="d61c1-182">Załóżmy na przykład, że wiersz student ma identyfikator = 1.</span><span class="sxs-lookup"><span data-stu-id="d61c1-182">For example, suppose a Student row has ID=1.</span></span> <span data-ttu-id="d61c1-183">Powiązane wiersze rejestracji będą mieć StudentID = 1.</span><span class="sxs-lookup"><span data-stu-id="d61c1-183">Related Enrollment rows will have StudentID = 1.</span></span> <span data-ttu-id="d61c1-184">StudentID jest *kluczem obcym* w tabeli rejestracji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-184">StudentID is a *foreign key* in the Enrollment table.</span></span> 

<span data-ttu-id="d61c1-185">Właściwość `Enrollments` jest definiowana jako `ICollection<Enrollment>`, ponieważ może istnieć wiele powiązanych jednostek rejestracji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-185">The `Enrollments` property is defined as `ICollection<Enrollment>` because there may be multiple related Enrollment entities.</span></span> <span data-ttu-id="d61c1-186">Można użyć innych typów kolekcji, takich jak `List<Enrollment>` lub `HashSet<Enrollment>`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-186">You can use other collection types, such as `List<Enrollment>` or `HashSet<Enrollment>`.</span></span> <span data-ttu-id="d61c1-187">Gdy jest używana `ICollection<Enrollment>`, EF Core domyślnie tworzy kolekcję `HashSet<Enrollment>`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-187">When `ICollection<Enrollment>` is used, EF Core creates a `HashSet<Enrollment>` collection by default.</span></span>

## <a name="the-enrollment-entity"></a><span data-ttu-id="d61c1-188">Jednostka rejestracji</span><span class="sxs-lookup"><span data-stu-id="d61c1-188">The Enrollment entity</span></span>

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

<span data-ttu-id="d61c1-190">Utwórz *modele/rejestrację. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="d61c1-190">Create *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

<span data-ttu-id="d61c1-191">Właściwość `EnrollmentID` jest kluczem podstawowym; Ta jednostka używa wzorca `classnameID` zamiast `ID`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-191">The `EnrollmentID` property is the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself.</span></span> <span data-ttu-id="d61c1-192">Dla modelu danych produkcyjnych wybierz jeden wzorzec i użyj go spójnie.</span><span class="sxs-lookup"><span data-stu-id="d61c1-192">For a production data model, choose one pattern and use it consistently.</span></span> <span data-ttu-id="d61c1-193">Ten samouczek używa obu tych metod, aby zilustrować, że obie działają.</span><span class="sxs-lookup"><span data-stu-id="d61c1-193">This tutorial uses both just to illustrate that both work.</span></span> <span data-ttu-id="d61c1-194">Używanie `ID` bez `classname` ułatwia implementowanie niektórych rodzajów zmian modelu danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-194">Using `ID` without `classname` makes it easier to implement some kinds of data model changes.</span></span>

<span data-ttu-id="d61c1-195">Właściwość `Grade` jest `enum`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-195">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="d61c1-196">Znak zapytania po deklaracji typu `Grade` wskazuje, że właściwość `Grade` [dopuszcza wartość null](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/).</span><span class="sxs-lookup"><span data-stu-id="d61c1-196">The question mark after the `Grade` type declaration indicates that the `Grade` property is [nullable](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/).</span></span> <span data-ttu-id="d61c1-197">Klasa o wartości null różni się od zerowej klasy @ no__t-0null oznacza, że Klasa nie jest znana lub nie została jeszcze przypisana.</span><span class="sxs-lookup"><span data-stu-id="d61c1-197">A grade that's null is different from a zero grade&mdash;null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="d61c1-198">Właściwość `StudentID` jest kluczem obcym, a odpowiednia właściwość nawigacji to `Student`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-198">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="d61c1-199">Jednostka `Enrollment` jest skojarzona z jedną jednostką `Student`, więc Właściwość zawiera jedną jednostkę `Student`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-199">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span>

<span data-ttu-id="d61c1-200">Właściwość `CourseID` jest kluczem obcym, a odpowiednia właściwość nawigacji to `Course`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-200">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="d61c1-201">Jednostka `Enrollment` jest skojarzona z jedną jednostką `Course`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-201">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="d61c1-202">EF Core interpretuje właściwość jako klucz obcy, jeśli ma nazwę `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-202">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="d61c1-203">Na przykład `StudentID` jest kluczem obcym dla właściwości nawigacji `Student`, ponieważ klucz podstawowy jednostki `Student` to `ID`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-203">For example,`StudentID` is the foreign key for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="d61c1-204">Właściwości klucza obcego mogą być również nazwane `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-204">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="d61c1-205">Na przykład `CourseID`, ponieważ klucz podstawowy jednostki `Course` jest `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-205">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="d61c1-206">Jednostka kursu</span><span class="sxs-lookup"><span data-stu-id="d61c1-206">The Course entity</span></span>

![Diagram jednostek kursu](intro/_static/course-entity.png)

<span data-ttu-id="d61c1-208">Utwórz *modele/kurs. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="d61c1-208">Create *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

<span data-ttu-id="d61c1-209">Właściwość `Enrollments` jest właściwością nawigacji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-209">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="d61c1-210">Jednostka `Course` może być powiązana z dowolną liczbą jednostek `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-210">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="d61c1-211">Atrybut `DatabaseGenerated` umożliwia aplikacji określenie klucza podstawowego zamiast wygenerowania go przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-211">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the database generate it.</span></span>

<span data-ttu-id="d61c1-212">Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilatora.</span><span class="sxs-lookup"><span data-stu-id="d61c1-212">Build the project to validate that there are no compiler errors.</span></span>

## <a name="scaffold-student-pages"></a><span data-ttu-id="d61c1-213">Strony uczniów tworzenia szkieletu</span><span class="sxs-lookup"><span data-stu-id="d61c1-213">Scaffold Student pages</span></span>

<span data-ttu-id="d61c1-214">W tej sekcji użyjesz narzędzia do tworzenia szkieletów ASP.NET Core do wygenerowania:</span><span class="sxs-lookup"><span data-stu-id="d61c1-214">In this section, you use the ASP.NET Core scaffolding tool to generate:</span></span>

* <span data-ttu-id="d61c1-215">Klasa *kontekstu* EF Core.</span><span class="sxs-lookup"><span data-stu-id="d61c1-215">An EF Core *context* class.</span></span> <span data-ttu-id="d61c1-216">Kontekst jest klasą główną, która koordynuje Entity Framework funkcji dla danego modelu danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-216">The context is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="d61c1-217">Pochodzi ona z klasy `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-217">It derives from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>
* <span data-ttu-id="d61c1-218">Strony Razor obsługujące operacje tworzenia, odczytu, aktualizacji i usuwania (CRUD) dla jednostki `Student`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-218">Razor pages that handle Create, Read, Update, and Delete (CRUD) operations for the `Student` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d61c1-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d61c1-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d61c1-220">Utwórz folder *uczniów* w folderze *strony* .</span><span class="sxs-lookup"><span data-stu-id="d61c1-220">Create a *Students* folder in the *Pages* folder.</span></span>
* <span data-ttu-id="d61c1-221">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *strony/uczniowie* i wybierz polecenie **dodaj** **nowy element szkieletowy**>.</span><span class="sxs-lookup"><span data-stu-id="d61c1-221">In **Solution Explorer**, right-click the *Pages/Students* folder and select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="d61c1-222">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d61c1-222">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>
* <span data-ttu-id="d61c1-223">W oknie dialogowym **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="d61c1-223">In the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
  * <span data-ttu-id="d61c1-224">Z listy rozwijanej **Klasa modelu** wybierz pozycję **student (ContosoUniversity. models)** .</span><span class="sxs-lookup"><span data-stu-id="d61c1-224">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
  * <span data-ttu-id="d61c1-225">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus).</span><span class="sxs-lookup"><span data-stu-id="d61c1-225">In the **Data context class** row, select the **+** (plus) sign.</span></span>
  * <span data-ttu-id="d61c1-226">Zmień nazwę kontekstu danych z *ContosoUniversity. models. ContosoUniversityContext* na *ContosoUniversity. Data. SchoolContext*.</span><span class="sxs-lookup"><span data-stu-id="d61c1-226">Change the data context name from *ContosoUniversity.Models.ContosoUniversityContext* to *ContosoUniversity.Data.SchoolContext*.</span></span>
  * <span data-ttu-id="d61c1-227">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d61c1-227">Select **Add**.</span></span>

<span data-ttu-id="d61c1-228">Następujące pakiety są instalowane automatycznie:</span><span class="sxs-lookup"><span data-stu-id="d61c1-228">The following packages are automatically installed:</span></span>

* `Microsoft.VisualStudio.Web.CodeGeneration.Design`
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.Extensions.Logging.Debug`
* `Microsoft.EntityFrameworkCore.Tools`

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d61c1-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d61c1-229">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d61c1-230">Uruchom następujące polecenia interfejs wiersza polecenia platformy .NET Core, aby zainstalować wymagane pakiety NuGet:</span><span class="sxs-lookup"><span data-stu-id="d61c1-230">Run the following .NET Core CLI commands to install required NuGet packages:</span></span>
<!-- TO DO  After testing, Replace with
[!INCLUDE[](~/includes/includes/add-EF-NuGet-SQLite-CLI.md)]
remove dotnet tool install --global  below
 -->
  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SQLite
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet add package Microsoft.EntityFrameworkCore.Tools
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet add package Microsoft.Extensions.Logging.Debug
  ```

  <span data-ttu-id="d61c1-231">Pakiet Microsoft. VisualStudio. Web. CodeGeneration. Design jest wymagany do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="d61c1-231">The Microsoft.VisualStudio.Web.CodeGeneration.Design package is required for scaffolding.</span></span> <span data-ttu-id="d61c1-232">Mimo że aplikacja nie będzie używać SQL Server, narzędzie do tworzenia szkieletów musi być pakietem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d61c1-232">Although the app won't use SQL Server, the scaffolding tool needs the SQL Server package.</span></span>

* <span data-ttu-id="d61c1-233">Utwórz folder *stron/uczniów* .</span><span class="sxs-lookup"><span data-stu-id="d61c1-233">Create a *Pages/Students* folder.</span></span>

* <span data-ttu-id="d61c1-234">Uruchom następujące polecenie, aby zainstalować narzędzie do tworzenia [szkieletu ASPNET-CodeGenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span><span class="sxs-lookup"><span data-stu-id="d61c1-234">Run the following command to install the [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

* <span data-ttu-id="d61c1-235">Uruchom następujące polecenie, aby uzyskać możliwość tworzenia szkieletu stron z uczniami.</span><span class="sxs-lookup"><span data-stu-id="d61c1-235">Run the following command to scaffold Student pages.</span></span>

  <span data-ttu-id="d61c1-236">**W systemie Windows**</span><span class="sxs-lookup"><span data-stu-id="d61c1-236">**On Windows**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
  ```

  <span data-ttu-id="d61c1-237">**W systemie macOS lub Linux**</span><span class="sxs-lookup"><span data-stu-id="d61c1-237">**On macOS or Linux**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
  ```

---

<span data-ttu-id="d61c1-238">Jeśli wystąpił problem z poprzednim krokiem, Skompiluj projekt i ponów próbę wykonania szkieletu.</span><span class="sxs-lookup"><span data-stu-id="d61c1-238">If you have a problem with the preceding step, build the project and retry the scaffold step.</span></span>

<span data-ttu-id="d61c1-239">Proces tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="d61c1-239">The scaffolding process:</span></span>

* <span data-ttu-id="d61c1-240">Tworzy strony Razor w folderze *stron/uczniów* :</span><span class="sxs-lookup"><span data-stu-id="d61c1-240">Creates Razor pages in the *Pages/Students* folder:</span></span>
  * <span data-ttu-id="d61c1-241">*Create. cshtml* i *Create.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="d61c1-241">*Create.cshtml* and *Create.cshtml.cs*</span></span>
  * <span data-ttu-id="d61c1-242">*DELETE. cshtml* i *DELETE.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="d61c1-242">*Delete.cshtml* and *Delete.cshtml.cs*</span></span>
  * <span data-ttu-id="d61c1-243">*Details. cshtml* i *details.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="d61c1-243">*Details.cshtml* and *Details.cshtml.cs*</span></span>
  * <span data-ttu-id="d61c1-244">*Edit. cshtml* i *Edit.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="d61c1-244">*Edit.cshtml* and *Edit.cshtml.cs*</span></span>
  * <span data-ttu-id="d61c1-245">*Index. cshtml* i *index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="d61c1-245">*Index.cshtml* and *Index.cshtml.cs*</span></span>
* <span data-ttu-id="d61c1-246">Tworzy *dane/SchoolContext. cs*.</span><span class="sxs-lookup"><span data-stu-id="d61c1-246">Creates *Data/SchoolContext.cs*.</span></span>
* <span data-ttu-id="d61c1-247">Dodaje kontekst do iniekcji zależności w *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d61c1-247">Adds the context to dependency injection in *Startup.cs*.</span></span>
* <span data-ttu-id="d61c1-248">Dodaje parametry połączenia z bazą danych do pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="d61c1-248">Adds a database connection string to *appsettings.json*.</span></span>

## <a name="database-connection-string"></a><span data-ttu-id="d61c1-249">Parametry połączenia z bazą danych</span><span class="sxs-lookup"><span data-stu-id="d61c1-249">Database connection string</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d61c1-250">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d61c1-250">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d61c1-251">Parametry połączenia określają [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="d61c1-251">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> 

[!code-json[Main](intro/samples/cu30/appsettings.json?highlight=11)]

<span data-ttu-id="d61c1-252">LocalDB to uproszczona wersja aparatu bazy danych SQL Server Express i jest przeznaczona do tworzenia aplikacji, a nie do użycia w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="d61c1-252">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="d61c1-253">Domyślnie LocalDB tworzy pliki *MDF* w katalogu `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-253">By default, LocalDB creates *.mdf* files in the `C:/Users/<user>` directory.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d61c1-254">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d61c1-254">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d61c1-255">Zmień parametry połączenia w taki sposób, aby wskazywały plik bazy danych programu SQLite o nazwie *cu. DB*:</span><span class="sxs-lookup"><span data-stu-id="d61c1-255">Change the connection string to point to a SQLite database file named *CU.db*:</span></span>

[!code-json[Main](intro/samples/cu30/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a><span data-ttu-id="d61c1-256">Aktualizowanie klasy kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="d61c1-256">Update the database context class</span></span>

<span data-ttu-id="d61c1-257">Klasa główna, która koordynuje funkcje EF Core dla danego modelu danych, jest klasą kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-257">The main class that coordinates EF Core functionality for a given data model is the database context class.</span></span> <span data-ttu-id="d61c1-258">Kontekst pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="d61c1-258">The context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="d61c1-259">Kontekst określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-259">The context specifies which entities are included in the data model.</span></span> <span data-ttu-id="d61c1-260">W tym projekcie Klasa ma nazwę `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-260">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="d61c1-261">Zaktualizuj *SchoolContext.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="d61c1-261">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

<span data-ttu-id="d61c1-262">Wyróżniony kod tworzy właściwość [nieogólnymi @ no__t-> 1TEntity](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla każdego zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="d61c1-262">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="d61c1-263">W EF Core terminologii:</span><span class="sxs-lookup"><span data-stu-id="d61c1-263">In EF Core terminology:</span></span>

* <span data-ttu-id="d61c1-264">Zestaw jednostek zwykle odpowiada tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-264">An entity set typically corresponds to a database table.</span></span>
* <span data-ttu-id="d61c1-265">Jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="d61c1-265">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="d61c1-266">Ponieważ zestaw jednostek zawiera wiele jednostek, właściwości Nieogólnymi powinny mieć nazwy w liczbie mnogiej.</span><span class="sxs-lookup"><span data-stu-id="d61c1-266">Since an entity set contains multiple entities, the DBSet properties should be plural names.</span></span> <span data-ttu-id="d61c1-267">Ponieważ narzędzie tworzenia szkieletów utworzyło obiekt @ no__t-0 Nieogólnymi, ten krok zmienia go na plural `Students`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-267">Since the scaffolding tool created a`Student` DBSet, this step changes it to plural `Students`.</span></span> 

<span data-ttu-id="d61c1-268">Aby kod Razor Pages był zgodny z nową nazwą Nieogólnymi, wprowadź globalną zmianę w całym projekcie `_context.Student`, aby `_context.Students`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-268">To make the Razor Pages code match the new DBSet name, make a global change across the whole project of `_context.Student` to `_context.Students`.</span></span>  <span data-ttu-id="d61c1-269">Istnieją 8 wystąpień.</span><span class="sxs-lookup"><span data-stu-id="d61c1-269">There are 8 occurrences.</span></span>

<span data-ttu-id="d61c1-270">Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilatora.</span><span class="sxs-lookup"><span data-stu-id="d61c1-270">Build the project to verify there are no compiler errors.</span></span>

## <a name="startupcs"></a><span data-ttu-id="d61c1-271">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="d61c1-271">Startup.cs</span></span>

<span data-ttu-id="d61c1-272">ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d61c1-272">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d61c1-273">Usługi (takie jak kontekst bazy danych EF Core) są rejestrowane z iniekcją zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-273">Services (such as the EF Core database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="d61c1-274">Składniki wymagające tych usług (takie jak Razor Pages) są udostępniane przez parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="d61c1-274">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="d61c1-275">Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych jest wyświetlany w dalszej części samouczka.</span><span class="sxs-lookup"><span data-stu-id="d61c1-275">The constructor code that gets a database context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="d61c1-276">Narzędzie do tworzenia szkieletów automatycznie zarejestrowało klasę kontekstu z kontenerem iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="d61c1-276">The scaffolding tool automatically registered the context class with the dependency injection container.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d61c1-277">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d61c1-277">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d61c1-278">W `ConfigureServices` wyróżnione wiersze zostały dodane przez szkieleter:</span><span class="sxs-lookup"><span data-stu-id="d61c1-278">In `ConfigureServices`, the highlighted lines were added by the scaffolder:</span></span>

  [!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d61c1-279">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d61c1-279">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d61c1-280">W `ConfigureServices` upewnij się, że kod dodany przez program tworzący szkielet jest wywoływany `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-280">In `ConfigureServices`, make sure the code added by the scaffolder calls `UseSqlite`.</span></span>

  [!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

---

<span data-ttu-id="d61c1-281">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) .</span><span class="sxs-lookup"><span data-stu-id="d61c1-281">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="d61c1-282">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="d61c1-282">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="d61c1-283">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="d61c1-283">Create the database</span></span>

<span data-ttu-id="d61c1-284">Zaktualizuj *program.cs* , aby utworzyć bazę danych, jeśli nie istnieje:</span><span class="sxs-lookup"><span data-stu-id="d61c1-284">Update *Program.cs* to create the database if it doesn't exist:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

<span data-ttu-id="d61c1-285">Jeśli istnieje baza danych dla kontekstu, Metoda [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) nie przyjmuje żadnej akcji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-285">The [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) method takes no action if a database for the context exists.</span></span> <span data-ttu-id="d61c1-286">Jeśli baza danych nie istnieje, tworzy bazę danych i schemat.</span><span class="sxs-lookup"><span data-stu-id="d61c1-286">If no database exists, it creates the database and schema.</span></span> <span data-ttu-id="d61c1-287">`EnsureCreated` włącza następujący przepływ pracy do obsługi zmian modelu danych:</span><span class="sxs-lookup"><span data-stu-id="d61c1-287">`EnsureCreated` enables the following workflow for handling data model changes:</span></span>

* <span data-ttu-id="d61c1-288">Usuń bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-288">Delete the database.</span></span> <span data-ttu-id="d61c1-289">Wszystkie istniejące dane zostaną utracone.</span><span class="sxs-lookup"><span data-stu-id="d61c1-289">Any existing data is lost.</span></span>
* <span data-ttu-id="d61c1-290">Zmień model danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-290">Change the data model.</span></span> <span data-ttu-id="d61c1-291">Na przykład Dodaj pole `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-291">For example, add an `EmailAddress` field.</span></span>
* <span data-ttu-id="d61c1-292">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="d61c1-292">Run the app.</span></span>
* <span data-ttu-id="d61c1-293">`EnsureCreated` tworzy bazę danych z nowym schematem.</span><span class="sxs-lookup"><span data-stu-id="d61c1-293">`EnsureCreated` creates a database with the new schema.</span></span>

<span data-ttu-id="d61c1-294">Ten przepływ pracy działa dobrze na etapie opracowywania, gdy schemat jest gwałtownie zmieniany, o ile nie trzeba zachować danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-294">This workflow works well early in development when the schema is rapidly evolving, as long as you don't need to preserve data.</span></span> <span data-ttu-id="d61c1-295">Sytuacja jest inna, gdy dane wprowadzone do bazy danych muszą zostać zachowane.</span><span class="sxs-lookup"><span data-stu-id="d61c1-295">The situation is different when data that has been entered into the database needs to be preserved.</span></span> <span data-ttu-id="d61c1-296">W takim przypadku należy użyć migracji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-296">When that is the case, use migrations.</span></span>

<span data-ttu-id="d61c1-297">W dalszej części tego samouczka usuniesz bazę danych, która została utworzona przez `EnsureCreated`, i zamiast tego użyj migracji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-297">Later in the tutorial series, you delete the database that was created by `EnsureCreated` and use migrations instead.</span></span> <span data-ttu-id="d61c1-298">Nie można zaktualizować bazy danych utworzonej przez `EnsureCreated` przy użyciu migracji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-298">A database that is created by `EnsureCreated` can't be updated by using migrations.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="d61c1-299">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d61c1-299">Test the app</span></span>

* <span data-ttu-id="d61c1-300">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="d61c1-300">Run the app.</span></span>
* <span data-ttu-id="d61c1-301">Wybierz link **Students (studenci** ), a następnie **Utwórz nowy**.</span><span class="sxs-lookup"><span data-stu-id="d61c1-301">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="d61c1-302">Przetestuj linki Edytuj, szczegóły i Usuń.</span><span class="sxs-lookup"><span data-stu-id="d61c1-302">Test the Edit, Details, and Delete links.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="d61c1-303">Wypełnianie bazy danych</span><span class="sxs-lookup"><span data-stu-id="d61c1-303">Seed the database</span></span>

<span data-ttu-id="d61c1-304">Metoda `EnsureCreated` tworzy pustą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-304">The `EnsureCreated` method creates an empty database.</span></span> <span data-ttu-id="d61c1-305">Ta sekcja dodaje kod wypełniający bazę danych danymi testowymi.</span><span class="sxs-lookup"><span data-stu-id="d61c1-305">This section adds code that populates the database with test data.</span></span>

<span data-ttu-id="d61c1-306">Utwórz *dane/Dbinitializeer. cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="d61c1-306">Create *Data/DbInitializer.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  <span data-ttu-id="d61c1-307">Kod sprawdza, czy w bazie danych istnieją uczniowie.</span><span class="sxs-lookup"><span data-stu-id="d61c1-307">The code checks if there are any students in the database.</span></span> <span data-ttu-id="d61c1-308">Jeśli nie ma uczniów, dodaje dane testowe do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-308">If there are no students, it adds test data to the database.</span></span> <span data-ttu-id="d61c1-309">Tworzy dane testowe w tablicach, a nie `List<T>` kolekcje w celu zoptymalizowania wydajności.</span><span class="sxs-lookup"><span data-stu-id="d61c1-309">It creates the test data in arrays rather than `List<T>` collections to optimize performance.</span></span>

* <span data-ttu-id="d61c1-310">W *program.cs*Zastąp wywołanie `EnsureCreated` z wywołaniem `DbInitializer.Initialize`:</span><span class="sxs-lookup"><span data-stu-id="d61c1-310">In *Program.cs*, replace the `EnsureCreated` call with a `DbInitializer.Initialize` call:</span></span>

  ```csharp
  // context.Database.EnsureCreated();
  DbInitializer.Initialize(context);
  ````

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d61c1-311">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d61c1-311">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d61c1-312">Zatrzymaj aplikację, jeśli jest uruchomiona, a następnie uruchom następujące polecenie w **konsoli Menedżera pakietów** (PMC):</span><span class="sxs-lookup"><span data-stu-id="d61c1-312">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d61c1-313">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d61c1-313">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d61c1-314">Zatrzymaj aplikację, jeśli jest uruchomiona, a następnie usuń plik *cu. DB* .</span><span class="sxs-lookup"><span data-stu-id="d61c1-314">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

* <span data-ttu-id="d61c1-315">Uruchom ponownie aplikację.</span><span class="sxs-lookup"><span data-stu-id="d61c1-315">Restart the app.</span></span>

* <span data-ttu-id="d61c1-316">Wybierz stronę uczniów, aby zobaczyć dane z rozrzutu.</span><span class="sxs-lookup"><span data-stu-id="d61c1-316">Select the Students page to see the seeded data.</span></span>

## <a name="view-the-database"></a><span data-ttu-id="d61c1-317">Wyświetlanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="d61c1-317">View the database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d61c1-318">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d61c1-318">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d61c1-319">Otwórz **Eksplorator obiektów SQL Server** (SSOX) z menu **Widok** w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d61c1-319">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
* <span data-ttu-id="d61c1-320">W SSOX wybierz pozycję **(LocalDB) \MSSQLLocalDB > bazy danych > SchoolContext-{GUID}** .</span><span class="sxs-lookup"><span data-stu-id="d61c1-320">In SSOX, select **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span> <span data-ttu-id="d61c1-321">Nazwa bazy danych jest generowana na podstawie podanej wcześniej nazwy kontekstu oraz łącznika i identyfikatora GUID.</span><span class="sxs-lookup"><span data-stu-id="d61c1-321">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span>
* <span data-ttu-id="d61c1-322">Rozwiń węzeł **tabele** .</span><span class="sxs-lookup"><span data-stu-id="d61c1-322">Expand the **Tables** node.</span></span>
* <span data-ttu-id="d61c1-323">Kliknij prawym przyciskiem myszy tabelę **uczniów** i kliknij polecenie **Wyświetl dane** , aby wyświetlić utworzone kolumny i wiersze wstawione do tabeli.</span><span class="sxs-lookup"><span data-stu-id="d61c1-323">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>
* <span data-ttu-id="d61c1-324">Kliknij prawym przyciskiem myszy tabelę **uczniów** i kliknij polecenie **Wyświetl kod** , aby zobaczyć, jak model `Student` jest mapowany na schemat tabeli `Student`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-324">Right-click the **Student** table and click **View Code** to see how the `Student` model maps to the `Student` table schema.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d61c1-325">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d61c1-325">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d61c1-326">Użyj narzędzia SQLite, aby wyświetlić schemat bazy danych i dane referencyjne.</span><span class="sxs-lookup"><span data-stu-id="d61c1-326">Use your SQLite tool to view the database schema and seeded data.</span></span> <span data-ttu-id="d61c1-327">Plik bazy danych ma nazwę *cu. DB* i znajduje się w folderze projektu.</span><span class="sxs-lookup"><span data-stu-id="d61c1-327">The database file is named *CU.db* and is located in the project folder.</span></span>

---

## <a name="asynchronous-code"></a><span data-ttu-id="d61c1-328">Kod asynchroniczny</span><span class="sxs-lookup"><span data-stu-id="d61c1-328">Asynchronous code</span></span>

<span data-ttu-id="d61c1-329">Programowanie asynchroniczne jest trybem domyślnym dla ASP.NET Core i EF Core.</span><span class="sxs-lookup"><span data-stu-id="d61c1-329">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="d61c1-330">Serwer sieci Web ma ograniczoną liczbę dostępnych wątków, a w przypadku dużego obciążenia mogą być używane wszystkie dostępne wątki.</span><span class="sxs-lookup"><span data-stu-id="d61c1-330">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="d61c1-331">W takim przypadku serwer nie może przetwarzać nowych żądań, dopóki wątki nie zostaną zwolnione.</span><span class="sxs-lookup"><span data-stu-id="d61c1-331">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="d61c1-332">W przypadku kodu synchronicznego wiele wątków może zostać powiązanych, podczas gdy nie wykonuje żadnej pracy, ponieważ oczekuje na ukończenie operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="d61c1-332">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="d61c1-333">W przypadku kodu asynchronicznego, gdy proces oczekuje na ukończenie operacji we/wy, jego wątek zostanie zwolniony dla serwera do użycia na potrzeby przetwarzania innych żądań.</span><span class="sxs-lookup"><span data-stu-id="d61c1-333">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="d61c1-334">W efekcie kod asynchroniczny umożliwia efektywniejsze wykorzystanie zasobów serwera, a serwer może obsłużyć więcej ruchu bez opóźnień.</span><span class="sxs-lookup"><span data-stu-id="d61c1-334">As a result, asynchronous code enables server resources to be used more efficiently, and the server can handle more traffic without delays.</span></span>

<span data-ttu-id="d61c1-335">Kod asynchroniczny wprowadza niewielką ilość zapasową w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="d61c1-335">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="d61c1-336">W przypadku niskiego ruchu zwiększenie wydajności jest nieznaczne, a jednocześnie w przypadku dużego ruchu, potencjalne udoskonalenia wydajności są istotne.</span><span class="sxs-lookup"><span data-stu-id="d61c1-336">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="d61c1-337">W poniższym kodzie słowo kluczowe [Async](/dotnet/csharp/language-reference/keywords/async) , wartość zwracana `Task<T>`, słowo kluczowe `await` i Metoda `ToListAsync` sprawiają, że kod jest wykonywany asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="d61c1-337">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* <span data-ttu-id="d61c1-338">Słowo kluczowe `async` instruuje kompilator, aby:</span><span class="sxs-lookup"><span data-stu-id="d61c1-338">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="d61c1-339">Generuj wywołania zwrotne dla części treści metody.</span><span class="sxs-lookup"><span data-stu-id="d61c1-339">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="d61c1-340">Utwórz obiekt [zadania](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) , który jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="d61c1-340">Create the [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) object that's returned.</span></span>
* <span data-ttu-id="d61c1-341">Typ zwracany `Task<T>` reprezentuje bieżącą liczbę zadań.</span><span class="sxs-lookup"><span data-stu-id="d61c1-341">The `Task<T>` return type represents ongoing work.</span></span>
* <span data-ttu-id="d61c1-342">Słowo kluczowe `await` powoduje, że kompilator dzieli metodę na dwie części.</span><span class="sxs-lookup"><span data-stu-id="d61c1-342">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="d61c1-343">Pierwsza część jest zakończona operacją uruchomioną asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="d61c1-343">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="d61c1-344">Druga część jest umieszczana w metodzie wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-344">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="d61c1-345">`ToListAsync` jest wersją asynchroniczną metody rozszerzenia `ToList`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-345">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="d61c1-346">Niektóre kwestie, o których należy pamiętać podczas pisania asynchronicznego kodu, który używa EF Core:</span><span class="sxs-lookup"><span data-stu-id="d61c1-346">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="d61c1-347">Tylko instrukcje, które powodują, że zapytania lub polecenia wysyłane do bazy danych są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="d61c1-347">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="d61c1-348">Obejmuje `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` i `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-348">That includes `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="d61c1-349">Nie zawiera instrukcji, które po prostu zmieniają `IQueryable`, takie jak `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-349">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="d61c1-350">Kontekst EF Core nie jest bezpieczny wątkowo: nie próbuj wykonać równolegle wielu operacji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-350">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="d61c1-351">Aby skorzystać z zalet wydajności w kodzie asynchronicznym, należy sprawdzić, czy pakiety biblioteki (na przykład stronicowania) używają wartości asynchronicznej, jeśli są wywoływane EF Core metod wysyłających zapytania do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-351">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the database.</span></span>

<span data-ttu-id="d61c1-352">Aby uzyskać więcej informacji na temat programowania asynchronicznego w programie .NET, zobacz [Omówienie asynchroniczne](/dotnet/standard/async) i [programowanie asynchroniczne z Async i await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="d61c1-352">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d61c1-353">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="d61c1-353">Next steps</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d61c1-354">Następny samouczek</span><span class="sxs-lookup"><span data-stu-id="d61c1-354">Next tutorial</span></span>](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d61c1-355">Przykładowa aplikacja internetowa Contoso University demonstruje sposób tworzenia aplikacji ASP.NET Core Razor Pages przy użyciu narzędzia Entity Framework (EF) Core.</span><span class="sxs-lookup"><span data-stu-id="d61c1-355">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="d61c1-356">Przykładowa aplikacja jest witryną internetową fikcyjnej firmy Contoso University.</span><span class="sxs-lookup"><span data-stu-id="d61c1-356">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="d61c1-357">Obejmuje to funkcje, takie jak przyjmowanie studentów, tworzenie kursu i przydziały instruktora.</span><span class="sxs-lookup"><span data-stu-id="d61c1-357">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="d61c1-358">Ta strona jest pierwszą częścią serii samouczków, które wyjaśniają sposób tworzenia przykładowej aplikacji firmy Contoso University.</span><span class="sxs-lookup"><span data-stu-id="d61c1-358">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="d61c1-359">Pobierz lub Wyświetl ukończoną aplikację.</span><span class="sxs-lookup"><span data-stu-id="d61c1-359">Download or view the completed app.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="d61c1-360">[Instrukcje pobierania](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d61c1-360">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d61c1-361">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d61c1-361">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d61c1-362">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d61c1-362">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d61c1-363">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d61c1-363">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [](~/includes/2.1-SDK.md)]

---

<span data-ttu-id="d61c1-364">Znajomość [Razor Pages](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="d61c1-364">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="d61c1-365">Nowi programiści powinni zakończyć pracę [z Razor Pages](xref:tutorials/razor-pages/razor-pages-start) przed rozpoczęciem tej serii.</span><span class="sxs-lookup"><span data-stu-id="d61c1-365">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="d61c1-366">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="d61c1-366">Troubleshooting</span></span>

<span data-ttu-id="d61c1-367">Jeśli wystąpi problem, którego nie można rozwiązać, można ogólnie znaleźć rozwiązanie, porównując kod z [ukończonym projektem](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="d61c1-367">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="d61c1-368">Dobrym sposobem uzyskania pomocy jest zaksięgowanie pytania [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) na [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="d61c1-368">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="d61c1-369">Aplikacja sieci Web firmy Contoso University</span><span class="sxs-lookup"><span data-stu-id="d61c1-369">The Contoso University web app</span></span>

<span data-ttu-id="d61c1-370">Aplikacja skompilowana w tych samouczkach jest podstawową szkołą w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d61c1-370">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="d61c1-371">Użytkownicy mogą wyświetlać i aktualizować informacje dotyczące uczniów, kursów i instruktorów.</span><span class="sxs-lookup"><span data-stu-id="d61c1-371">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="d61c1-372">Poniżej przedstawiono kilka ekranów utworzonych w samouczku.</span><span class="sxs-lookup"><span data-stu-id="d61c1-372">Here are a few of the screens created in the tutorial.</span></span>

![Strona indeksu uczniów](intro/_static/students-index.png)

![Strona edycji uczniów](intro/_static/student-edit.png)

<span data-ttu-id="d61c1-375">Styl interfejsu użytkownika tej witryny jest zbliżony do zawartości wygenerowanej przez wbudowane szablony.</span><span class="sxs-lookup"><span data-stu-id="d61c1-375">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="d61c1-376">Samouczek samouczka znajduje się na EF Core z Razor Pages, a nie z interfejsem użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d61c1-376">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="d61c1-377">Tworzenie aplikacji sieci Web ContosoUniversity Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d61c1-377">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d61c1-378">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d61c1-378">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d61c1-379">Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt**>.</span><span class="sxs-lookup"><span data-stu-id="d61c1-379">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d61c1-380">Utwórz nową aplikację sieci Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d61c1-380">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="d61c1-381">Nazwij projekt **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="d61c1-381">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="d61c1-382">Ważne jest, aby nazwa projektu *ContosoUniversity* , tak aby przestrzenie nazw były zgodne, gdy kod jest kopiowany/wklejany.</span><span class="sxs-lookup"><span data-stu-id="d61c1-382">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="d61c1-383">Wybierz pozycję **ASP.NET Core 2,1** na liście rozwijanej, a następnie wybierz pozycję **aplikacja sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="d61c1-383">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="d61c1-384">Aby poznać obrazy powyższych kroków, zobacz [Tworzenie aplikacji sieci Web Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span><span class="sxs-lookup"><span data-stu-id="d61c1-384">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span></span>
<span data-ttu-id="d61c1-385">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="d61c1-385">Run the app.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d61c1-386">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d61c1-386">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="d61c1-387">Konfigurowanie stylu witryny</span><span class="sxs-lookup"><span data-stu-id="d61c1-387">Set up the site style</span></span>

<span data-ttu-id="d61c1-388">Kilka zmian powoduje skonfigurowanie menu witryny, układu i strony głównej.</span><span class="sxs-lookup"><span data-stu-id="d61c1-388">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="d61c1-389">Aktualizowanie *stron/Shared/_Layout. cshtml* z następującymi zmianami:</span><span class="sxs-lookup"><span data-stu-id="d61c1-389">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="d61c1-390">Zmień każde wystąpienie "ContosoUniversity" na "Uniwersytet firmy Contoso".</span><span class="sxs-lookup"><span data-stu-id="d61c1-390">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="d61c1-391">Istnieją trzy wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="d61c1-391">There are three occurrences.</span></span>

* <span data-ttu-id="d61c1-392">Dodaj pozycje menu dla **studentów**, **kursów**, **instruktorów**i **działów**, a następnie usuń wpis menu **kontakt** .</span><span class="sxs-lookup"><span data-stu-id="d61c1-392">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="d61c1-393">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="d61c1-393">The changes are highlighted.</span></span> <span data-ttu-id="d61c1-394">(Wszystkie znaczniki *nie* są wyświetlane).</span><span class="sxs-lookup"><span data-stu-id="d61c1-394">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="d61c1-395">W obszarze *Pages/index. cshtml*Zastąp zawartość pliku następującym kodem, aby zamienić tekst na ASP.NET i MVC z tekstem dotyczącym tej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d61c1-395">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="d61c1-396">Tworzenie modelu danych</span><span class="sxs-lookup"><span data-stu-id="d61c1-396">Create the data model</span></span>

<span data-ttu-id="d61c1-397">Utwórz klasy jednostek dla aplikacji firmy Contoso University.</span><span class="sxs-lookup"><span data-stu-id="d61c1-397">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="d61c1-398">Zacznij od następujących trzech jednostek:</span><span class="sxs-lookup"><span data-stu-id="d61c1-398">Start with the following three entities:</span></span>

![Kurs — Diagram modelu danych ucznia](intro/_static/data-model-diagram.png)

<span data-ttu-id="d61c1-400">Istnieje relacja jeden do wielu między jednostkami `Student` i `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-400">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="d61c1-401">Istnieje relacja jeden do wielu między jednostkami `Course` i `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-401">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="d61c1-402">Student może zarejestrować się w dowolnej liczbie kursów.</span><span class="sxs-lookup"><span data-stu-id="d61c1-402">A student can enroll in any number of courses.</span></span> <span data-ttu-id="d61c1-403">Kurs może mieć dowolną liczbę uczniów zarejestrowanych w tym obszarze.</span><span class="sxs-lookup"><span data-stu-id="d61c1-403">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="d61c1-404">W poniższych sekcjach zostanie utworzona Klasa dla każdej z tych jednostek.</span><span class="sxs-lookup"><span data-stu-id="d61c1-404">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="d61c1-405">Jednostka ucznia</span><span class="sxs-lookup"><span data-stu-id="d61c1-405">The Student entity</span></span>

![Diagram jednostek uczniów](intro/_static/student-entity.png)

<span data-ttu-id="d61c1-407">Utwórz folder *models* .</span><span class="sxs-lookup"><span data-stu-id="d61c1-407">Create a *Models* folder.</span></span> <span data-ttu-id="d61c1-408">W folderze *modele* Utwórz plik klasy o nazwie *student.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="d61c1-408">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="d61c1-409">Właściwość `ID` jest kolumną klucza podstawowego tabeli bazy danych (DB), która odnosi się do tej klasy.</span><span class="sxs-lookup"><span data-stu-id="d61c1-409">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="d61c1-410">Domyślnie EF Core interpretuje właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="d61c1-410">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="d61c1-411">W `classnameID`, `classname` jest nazwą klasy.</span><span class="sxs-lookup"><span data-stu-id="d61c1-411">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="d61c1-412">Alternatywny automatycznie rozpoznany klucz podstawowy to `StudentID` w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="d61c1-412">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="d61c1-413">Właściwość `Enrollments` jest [właściwością nawigacji](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="d61c1-413">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="d61c1-414">Właściwości nawigacji łączą się z innymi jednostkami, które są powiązane z tą jednostką.</span><span class="sxs-lookup"><span data-stu-id="d61c1-414">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="d61c1-415">W takim przypadku Właściwość `Enrollments` `Student entity` zawiera wszystkie jednostki `Enrollment`, które są powiązane z tym `Student`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-415">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="d61c1-416">Na przykład jeśli wiersz student w bazie danych ma dwa powiązane wiersze rejestracji, właściwość nawigacji `Enrollments` zawiera te dwa jednostki `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-416">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="d61c1-417">Powiązany `Enrollment` wiersz jest wierszem zawierającym wartość klucza podstawowego studenta w kolumnie `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-417">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="d61c1-418">Załóżmy na przykład, że student o IDENTYFIKATORze 1 ma dwa wiersze w tabeli `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-418">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="d61c1-419">Tabela `Enrollment` ma dwa wiersze z `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="d61c1-419">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="d61c1-420">`StudentID` to klucz obcy w tabeli `Enrollment`, który określa student w tabeli `Student`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-420">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="d61c1-421">Jeśli właściwość nawigacji może zawierać wiele jednostek, właściwość nawigacji musi być typem listy, na przykład `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-421">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="d61c1-422">można określić `ICollection<T>` lub typ taki jak `List<T>` lub `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-422">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="d61c1-423">Gdy jest używana `ICollection<T>`, EF Core domyślnie tworzy kolekcję `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-423">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="d61c1-424">Właściwości nawigacji, które przechowują wiele jednostek, pochodzą z relacji "wiele do wielu" i "jeden do wielu".</span><span class="sxs-lookup"><span data-stu-id="d61c1-424">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="d61c1-425">Jednostka rejestracji</span><span class="sxs-lookup"><span data-stu-id="d61c1-425">The Enrollment entity</span></span>

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

<span data-ttu-id="d61c1-427">W folderze *modele* Utwórz *Enrollment.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="d61c1-427">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="d61c1-428">Właściwość `EnrollmentID` jest kluczem podstawowym.</span><span class="sxs-lookup"><span data-stu-id="d61c1-428">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="d61c1-429">Ta jednostka używa wzorca `classnameID` zamiast `ID`, takiego jak jednostka `Student`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-429">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="d61c1-430">Zazwyczaj deweloperzy wybierają jeden wzorzec i używają go w całym modelu danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-430">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="d61c1-431">W późniejszym samouczku, użycie identyfikatora bez ClassName jest pokazane, aby ułatwić implementację dziedziczenia w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-431">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="d61c1-432">Właściwość `Grade` jest `enum`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-432">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="d61c1-433">Znak zapytania po deklaracji typu `Grade` wskazuje, że właściwość `Grade` dopuszcza wartość null.</span><span class="sxs-lookup"><span data-stu-id="d61c1-433">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="d61c1-434">Klasa o wartości null różni się od klasy zerowej — wartość null oznacza, że Klasa nie jest znana lub nie została jeszcze przypisana.</span><span class="sxs-lookup"><span data-stu-id="d61c1-434">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="d61c1-435">Właściwość `StudentID` jest kluczem obcym, a odpowiednia właściwość nawigacji to `Student`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-435">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="d61c1-436">Jednostka `Enrollment` jest skojarzona z jedną jednostką `Student`, więc Właściwość zawiera jedną jednostkę `Student`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-436">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="d61c1-437">Jednostka `Student` różni się od właściwości nawigacji `Student.Enrollments`, która zawiera wiele jednostek `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-437">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="d61c1-438">Właściwość `CourseID` jest kluczem obcym, a odpowiednia właściwość nawigacji to `Course`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-438">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="d61c1-439">Jednostka `Enrollment` jest skojarzona z jedną jednostką `Course`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-439">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="d61c1-440">EF Core interpretuje właściwość jako klucz obcy, jeśli ma nazwę `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-440">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="d61c1-441">Na przykład `StudentID` dla właściwości nawigacji `Student`, ponieważ klucz podstawowy jednostki `Student` jest `ID`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-441">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="d61c1-442">Właściwości klucza obcego mogą być również nazwane `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-442">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="d61c1-443">Na przykład `CourseID`, ponieważ klucz podstawowy jednostki `Course` jest `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-443">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="d61c1-444">Jednostka kursu</span><span class="sxs-lookup"><span data-stu-id="d61c1-444">The Course entity</span></span>

![Diagram jednostek kursu](intro/_static/course-entity.png)

<span data-ttu-id="d61c1-446">W folderze *modele* Utwórz *Course.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="d61c1-446">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="d61c1-447">Właściwość `Enrollments` jest właściwością nawigacji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-447">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="d61c1-448">Jednostka `Course` może być powiązana z dowolną liczbą jednostek `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-448">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="d61c1-449">Atrybut `DatabaseGenerated` umożliwia aplikacji określenie klucza podstawowego zamiast generowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-449">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="d61c1-450">Tworzenie szkieletu modelu ucznia</span><span class="sxs-lookup"><span data-stu-id="d61c1-450">Scaffold the student model</span></span>

<span data-ttu-id="d61c1-451">W tej sekcji model ucznia jest szkieletem.</span><span class="sxs-lookup"><span data-stu-id="d61c1-451">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="d61c1-452">Oznacza to, że narzędzie tworzenia szkieletów tworzy strony dla operacji Create, Read, Update i Delete (CRUD) dla modelu ucznia.</span><span class="sxs-lookup"><span data-stu-id="d61c1-452">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="d61c1-453">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="d61c1-453">Build the project.</span></span>
* <span data-ttu-id="d61c1-454">Utwórz folder *strony/uczniowie* .</span><span class="sxs-lookup"><span data-stu-id="d61c1-454">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d61c1-455">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d61c1-455">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d61c1-456">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *strony/uczniowie* > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="d61c1-456">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="d61c1-457">W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d61c1-457">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="d61c1-458">Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="d61c1-458">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="d61c1-459">Z listy rozwijanej **Klasa modelu** wybierz pozycję **student (ContosoUniversity. models)** .</span><span class="sxs-lookup"><span data-stu-id="d61c1-459">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="d61c1-460">W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i Zmień wygenerowaną nazwę na **ContosoUniversity. models. SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="d61c1-460">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="d61c1-461">Z listy rozwijanej **Klasa kontekstu danych** wybierz pozycję **ContosoUniversity. models. SchoolContext**</span><span class="sxs-lookup"><span data-stu-id="d61c1-461">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="d61c1-462">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="d61c1-462">Select **Add**.</span></span>

![Okno dialogowe CRUD](intro/_static/s1.png)

<span data-ttu-id="d61c1-464">Zobacz Tworzenie [szkieletu modelu filmu w](xref:tutorials/razor-pages/model#scaffold-the-movie-model) przypadku problemu z poprzednim krokiem.</span><span class="sxs-lookup"><span data-stu-id="d61c1-464">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d61c1-465">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d61c1-465">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d61c1-466">Uruchom następujące polecenia, aby uzyskać szkielet modelu ucznia.</span><span class="sxs-lookup"><span data-stu-id="d61c1-466">Run the following commands to scaffold the student model.</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

---

<span data-ttu-id="d61c1-467">Proces szkieletu utworzył i zmienił następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="d61c1-467">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="d61c1-468">Utworzone pliki</span><span class="sxs-lookup"><span data-stu-id="d61c1-468">Files created</span></span>

* <span data-ttu-id="d61c1-469">*Strony/uczniowie* Tworzenie, usuwanie, szczegóły, edytowanie, indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="d61c1-469">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="d61c1-470">*Data/SchoolContext. cs*</span><span class="sxs-lookup"><span data-stu-id="d61c1-470">*Data/SchoolContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="d61c1-471">Aktualizacje plików</span><span class="sxs-lookup"><span data-stu-id="d61c1-471">File updates</span></span>

* <span data-ttu-id="d61c1-472">*Startup.cs* : szczegółowe zmiany w tym pliku opisano w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-472">*Startup.cs* : Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="d61c1-473">*appSettings. JSON* : dodano parametry połączenia używane do nawiązywania połączenia z lokalną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-473">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="d61c1-474">Sprawdzanie kontekstu zarejestrowanego przy iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="d61c1-474">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="d61c1-475">ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d61c1-475">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d61c1-476">Usługi (takie jak kontekst EF Core DB) są rejestrowane z iniekcją zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-476">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="d61c1-477">Składniki wymagające tych usług (takie jak Razor Pages) są udostępniane przez parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="d61c1-477">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="d61c1-478">Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych, jest wyświetlany w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="d61c1-478">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="d61c1-479">Narzędzie do tworzenia szkieletów automatycznie utworzyło kontekst bazy danych i zarejestrował go przy użyciu kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="d61c1-479">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="d61c1-480">Przeanalizuj metodę `ConfigureServices` w *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d61c1-480">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="d61c1-481">Podświetlony wiersz został dodany przez program do tworzenia szkieletu:</span><span class="sxs-lookup"><span data-stu-id="d61c1-481">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="d61c1-482">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) .</span><span class="sxs-lookup"><span data-stu-id="d61c1-482">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="d61c1-483">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="d61c1-483">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="d61c1-484">Aktualizuj główne</span><span class="sxs-lookup"><span data-stu-id="d61c1-484">Update main</span></span>

<span data-ttu-id="d61c1-485">W *program.cs*zmień metodę `Main`, aby wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="d61c1-485">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="d61c1-486">Pobierz wystąpienie kontekstu bazy danych z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="d61c1-486">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="d61c1-487">Wywołaj [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span><span class="sxs-lookup"><span data-stu-id="d61c1-487">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="d61c1-488">Usuń kontekst, gdy metoda `EnsureCreated` zakończy działanie.</span><span class="sxs-lookup"><span data-stu-id="d61c1-488">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="d61c1-489">Poniższy kod przedstawia zaktualizowany plik *program.cs* .</span><span class="sxs-lookup"><span data-stu-id="d61c1-489">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="d61c1-490">`EnsureCreated` zapewnia, że baza danych dla kontekstu istnieje.</span><span class="sxs-lookup"><span data-stu-id="d61c1-490">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="d61c1-491">Jeśli istnieje, nie zostanie podjęta żadna akcja.</span><span class="sxs-lookup"><span data-stu-id="d61c1-491">If it exists, no action is taken.</span></span> <span data-ttu-id="d61c1-492">Jeśli nie istnieje, zostanie utworzona baza danych i jej wszystkie schematy.</span><span class="sxs-lookup"><span data-stu-id="d61c1-492">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="d61c1-493">`EnsureCreated` nie korzysta z migracji w celu utworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-493">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="d61c1-494">Bazy danych utworzonej przy użyciu `EnsureCreated` nie można później zaktualizować za pomocą migracji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-494">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="d61c1-495">`EnsureCreated` jest wywoływana podczas uruchamiania aplikacji, co pozwala na następujący przepływ pracy:</span><span class="sxs-lookup"><span data-stu-id="d61c1-495">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="d61c1-496">Usuń bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-496">Delete the DB.</span></span>
* <span data-ttu-id="d61c1-497">Zmień schemat bazy danych (na przykład Dodaj pole `EmailAddress`).</span><span class="sxs-lookup"><span data-stu-id="d61c1-497">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="d61c1-498">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="d61c1-498">Run the app.</span></span>
* <span data-ttu-id="d61c1-499">`EnsureCreated` tworzy bazę danych z kolumną @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="d61c1-499">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="d61c1-500">`EnsureCreated` jest wygodnie wczesny podczas opracowywania, gdy schemat szybko się rozwija.</span><span class="sxs-lookup"><span data-stu-id="d61c1-500">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="d61c1-501">W dalszej części tego samouczka baza danych została usunięta, a migracja jest używana.</span><span class="sxs-lookup"><span data-stu-id="d61c1-501">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="d61c1-502">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d61c1-502">Test the app</span></span>

<span data-ttu-id="d61c1-503">Uruchom aplikację i zaakceptuj zasady dotyczące plików cookie.</span><span class="sxs-lookup"><span data-stu-id="d61c1-503">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="d61c1-504">Ta aplikacja nie przechowuje informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-504">This app doesn't keep personal information.</span></span> <span data-ttu-id="d61c1-505">Informacje o zasadach dotyczących plików cookie można znaleźć na stronie [pomocy technicznej ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="d61c1-505">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="d61c1-506">Wybierz link **Students (studenci** ), a następnie **Utwórz nowy**.</span><span class="sxs-lookup"><span data-stu-id="d61c1-506">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="d61c1-507">Przetestuj linki Edytuj, szczegóły i Usuń.</span><span class="sxs-lookup"><span data-stu-id="d61c1-507">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="d61c1-508">Sprawdzanie kontekstu bazy danych SchoolContext DB</span><span class="sxs-lookup"><span data-stu-id="d61c1-508">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="d61c1-509">Klasa główna, która koordynuje funkcje EF Core dla danego modelu danych, jest klasą kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-509">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="d61c1-510">Kontekst danych pochodzi od elementu [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="d61c1-510">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="d61c1-511">Kontekst danych określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-511">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="d61c1-512">W tym projekcie Klasa ma nazwę `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-512">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="d61c1-513">Zaktualizuj *SchoolContext.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="d61c1-513">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="d61c1-514">Wyróżniony kod tworzy właściwość [nieogólnymi @ no__t-> 1TEntity](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla każdego zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="d61c1-514">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="d61c1-515">W EF Core terminologii:</span><span class="sxs-lookup"><span data-stu-id="d61c1-515">In EF Core terminology:</span></span>

* <span data-ttu-id="d61c1-516">Zestaw jednostek zwykle odpowiada tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-516">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="d61c1-517">Jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="d61c1-517">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="d61c1-518">można pominąć `DbSet<Enrollment>` i `DbSet<Course>`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-518">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="d61c1-519">EF Core obejmuje te niejawnie, ponieważ jednostka `Student` odwołuje się do jednostki `Enrollment`, a jednostka `Enrollment` odwołuje się do jednostki `Course`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-519">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="d61c1-520">Na potrzeby tego samouczka Zachowaj `DbSet<Enrollment>` i `DbSet<Course>` w `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-520">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="d61c1-521">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="d61c1-521">SQL Server Express LocalDB</span></span>

<span data-ttu-id="d61c1-522">Parametry połączenia określają [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="d61c1-522">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="d61c1-523">LocalDB to uproszczona wersja aparatu bazy danych SQL Server Express i jest przeznaczona do tworzenia aplikacji, a nie do użycia w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="d61c1-523">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="d61c1-524">LocalDB uruchamia się na żądanie i działa w trybie użytkownika, więc nie istnieje złożona konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="d61c1-524">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="d61c1-525">Domyślnie LocalDB tworzy pliki *MDF* DB w katalogu `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-525">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="d61c1-526">Dodawanie kodu w celu zainicjowania bazy danych z danymi testowymi</span><span class="sxs-lookup"><span data-stu-id="d61c1-526">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="d61c1-527">EF Core tworzy pustą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-527">EF Core creates an empty DB.</span></span> <span data-ttu-id="d61c1-528">W tej sekcji Metoda `Initialize` jest zapisywana w celu wypełnienia jej danymi testowymi.</span><span class="sxs-lookup"><span data-stu-id="d61c1-528">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="d61c1-529">W folderze *dane* Utwórz nowy plik klasy o nazwie *DbInitializer.cs* i Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="d61c1-529">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="d61c1-530">Uwaga: Poprzedni kod używa `Models` dla przestrzeni nazw (`namespace ContosoUniversity.Models`), a nie `Data`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-530">Note: The preceding code uses `Models` for the namespace (`namespace ContosoUniversity.Models`) rather than `Data`.</span></span> <span data-ttu-id="d61c1-531">`Models` jest spójne z kodem generowanym przez program.</span><span class="sxs-lookup"><span data-stu-id="d61c1-531">`Models` is consistent with the scaffolder-generated code.</span></span> <span data-ttu-id="d61c1-532">Aby uzyskać więcej informacji, zobacz [ten problem z szkieletem usługi GitHub](https://github.com/aspnet/Scaffolding/issues/822).</span><span class="sxs-lookup"><span data-stu-id="d61c1-532">For more information, see [this GitHub scaffolding issue](https://github.com/aspnet/Scaffolding/issues/822).</span></span>

<span data-ttu-id="d61c1-533">Kod sprawdza, czy w bazie danych istnieją uczniowie.</span><span class="sxs-lookup"><span data-stu-id="d61c1-533">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="d61c1-534">Jeśli w bazie danych nie ma studentów, baza danych zostanie zainicjowana z danymi testowymi.</span><span class="sxs-lookup"><span data-stu-id="d61c1-534">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="d61c1-535">Ładuje dane testowe do tablic, a nie kolekcji `List<T>` w celu zoptymalizowania wydajności.</span><span class="sxs-lookup"><span data-stu-id="d61c1-535">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="d61c1-536">Metoda `EnsureCreated` automatycznie tworzy bazę danych dla kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-536">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="d61c1-537">Jeśli baza danych istnieje, `EnsureCreated` zwraca bez modyfikowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-537">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="d61c1-538">W *program.cs*zmień metodę `Main`, aby wywołać `Initialize`:</span><span class="sxs-lookup"><span data-stu-id="d61c1-538">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d61c1-539">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d61c1-539">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d61c1-540">Zatrzymaj aplikację, jeśli jest uruchomiona, a następnie uruchom następujące polecenie w **konsoli Menedżera pakietów** (PMC):</span><span class="sxs-lookup"><span data-stu-id="d61c1-540">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d61c1-541">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d61c1-541">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d61c1-542">Zatrzymaj aplikację, jeśli jest uruchomiona, a następnie usuń plik *cu. DB* .</span><span class="sxs-lookup"><span data-stu-id="d61c1-542">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

## <a name="view-the-db"></a><span data-ttu-id="d61c1-543">Wyświetlanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="d61c1-543">View the DB</span></span>

<span data-ttu-id="d61c1-544">Nazwa bazy danych jest generowana na podstawie podanej wcześniej nazwy kontekstu oraz łącznika i identyfikatora GUID.</span><span class="sxs-lookup"><span data-stu-id="d61c1-544">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span> <span data-ttu-id="d61c1-545">W rezultacie nazwa bazy danych będzie "SchoolContext-{GUID}".</span><span class="sxs-lookup"><span data-stu-id="d61c1-545">Thus, the database name will be "SchoolContext-{GUID}".</span></span> <span data-ttu-id="d61c1-546">Identyfikator GUID będzie różny dla każdego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d61c1-546">The GUID will be different for each user.</span></span>
<span data-ttu-id="d61c1-547">Otwórz **Eksplorator obiektów SQL Server** (SSOX) z menu **Widok** w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d61c1-547">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="d61c1-548">W SSOX kliknij pozycję **(LocalDB) \MSSQLLocalDB > bazy danych > SchoolContext-{GUID}** .</span><span class="sxs-lookup"><span data-stu-id="d61c1-548">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span>

<span data-ttu-id="d61c1-549">Rozwiń węzeł **tabele** .</span><span class="sxs-lookup"><span data-stu-id="d61c1-549">Expand the **Tables** node.</span></span>

<span data-ttu-id="d61c1-550">Kliknij prawym przyciskiem myszy tabelę **uczniów** i kliknij polecenie **Wyświetl dane** , aby wyświetlić utworzone kolumny i wiersze wstawione do tabeli.</span><span class="sxs-lookup"><span data-stu-id="d61c1-550">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="d61c1-551">Kod asynchroniczny</span><span class="sxs-lookup"><span data-stu-id="d61c1-551">Asynchronous code</span></span>

<span data-ttu-id="d61c1-552">Programowanie asynchroniczne jest trybem domyślnym dla ASP.NET Core i EF Core.</span><span class="sxs-lookup"><span data-stu-id="d61c1-552">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="d61c1-553">Serwer sieci Web ma ograniczoną liczbę dostępnych wątków, a w przypadku dużego obciążenia mogą być używane wszystkie dostępne wątki.</span><span class="sxs-lookup"><span data-stu-id="d61c1-553">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="d61c1-554">W takim przypadku serwer nie może przetwarzać nowych żądań, dopóki wątki nie zostaną zwolnione.</span><span class="sxs-lookup"><span data-stu-id="d61c1-554">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="d61c1-555">W przypadku kodu synchronicznego wiele wątków może zostać powiązanych, podczas gdy nie wykonuje żadnej pracy, ponieważ oczekuje na ukończenie operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="d61c1-555">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="d61c1-556">W przypadku kodu asynchronicznego, gdy proces oczekuje na ukończenie operacji we/wy, jego wątek zostanie zwolniony dla serwera do użycia na potrzeby przetwarzania innych żądań.</span><span class="sxs-lookup"><span data-stu-id="d61c1-556">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="d61c1-557">W efekcie kod asynchroniczny umożliwia efektywniejsze wykorzystanie zasobów serwera, a serwer jest włączony do obsługi większej ilości ruchu bez opóźnień.</span><span class="sxs-lookup"><span data-stu-id="d61c1-557">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="d61c1-558">Kod asynchroniczny wprowadza niewielką ilość zapasową w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="d61c1-558">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="d61c1-559">W przypadku niskiego ruchu zwiększenie wydajności jest nieznaczne, a jednocześnie w przypadku dużego ruchu, potencjalne udoskonalenia wydajności są istotne.</span><span class="sxs-lookup"><span data-stu-id="d61c1-559">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="d61c1-560">W poniższym kodzie słowo kluczowe [Async](/dotnet/csharp/language-reference/keywords/async) , wartość zwracana `Task<T>`, słowo kluczowe `await` i Metoda `ToListAsync` sprawiają, że kod jest wykonywany asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="d61c1-560">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="d61c1-561">Słowo kluczowe `async` instruuje kompilator, aby:</span><span class="sxs-lookup"><span data-stu-id="d61c1-561">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="d61c1-562">Generuj wywołania zwrotne dla części treści metody.</span><span class="sxs-lookup"><span data-stu-id="d61c1-562">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="d61c1-563">Automatycznie Utwórz obiekt [zadania](/dotnet/api/system.threading.tasks.task) , który jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="d61c1-563">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="d61c1-564">Aby uzyskać więcej informacji, zobacz [Typ zwracany zadania](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="d61c1-564">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="d61c1-565">Niejawny typ zwracany `Task` reprezentuje bieżącą liczbę zadań.</span><span class="sxs-lookup"><span data-stu-id="d61c1-565">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="d61c1-566">Słowo kluczowe `await` powoduje, że kompilator dzieli metodę na dwie części.</span><span class="sxs-lookup"><span data-stu-id="d61c1-566">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="d61c1-567">Pierwsza część jest zakończona operacją uruchomioną asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="d61c1-567">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="d61c1-568">Druga część jest umieszczana w metodzie wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-568">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="d61c1-569">`ToListAsync` jest wersją asynchroniczną metody rozszerzenia `ToList`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-569">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="d61c1-570">Niektóre kwestie, o których należy pamiętać podczas pisania asynchronicznego kodu, który używa EF Core:</span><span class="sxs-lookup"><span data-stu-id="d61c1-570">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="d61c1-571">Tylko instrukcje, które powodują, że zapytania lub polecenia wysyłane do bazy danych są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="d61c1-571">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="d61c1-572">Obejmuje to `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` i `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-572">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="d61c1-573">Nie zawiera instrukcji, które po prostu zmieniają `IQueryable`, takie jak `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="d61c1-573">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="d61c1-574">Kontekst EF Core nie jest bezpieczny wątkowo: nie próbuj wykonać równolegle wielu operacji.</span><span class="sxs-lookup"><span data-stu-id="d61c1-574">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="d61c1-575">Aby skorzystać z zalet wydajności w kodzie asynchronicznym, należy sprawdzić, czy pakiety biblioteki (na przykład stronicowania) używają wartości asynchronicznej, jeśli są wywoływane EF Core metod wysyłających zapytania do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d61c1-575">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="d61c1-576">Aby uzyskać więcej informacji na temat programowania asynchronicznego w programie .NET, zobacz [Omówienie asynchroniczne](/dotnet/standard/async) i [programowanie asynchroniczne z Async i await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="d61c1-576">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="d61c1-577">W następnym samouczku są badane podstawowe operacje CRUD (tworzenie, odczytywanie, aktualizowanie, usuwanie).</span><span class="sxs-lookup"><span data-stu-id="d61c1-577">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>



## <a name="additional-resources"></a><span data-ttu-id="d61c1-578">Zasoby dodatkowe</span><span class="sxs-lookup"><span data-stu-id="d61c1-578">Additional resources</span></span>

* [<span data-ttu-id="d61c1-579">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="d61c1-579">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [<span data-ttu-id="d61c1-580">Dalej</span><span class="sxs-lookup"><span data-stu-id="d61c1-580">Next</span></span>](xref:data/ef-rp/crud)

::: moniker-end
